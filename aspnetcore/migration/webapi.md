---
title: Migrieren von ASP.NET Web-API zu ASP.NET Core
author: ardalis
description: Erfahren Sie, wie eine Web-API-Implementierung von ASP.NET Web-API zu ASP.NET Core MVC zu migrieren.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 2f1d0b43f565dbf6189406bfd65158f809e1f18f
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrieren von ASP.NET Web-API zu ASP.NET Core

Von [Steve Smith](https://ardalis.com/) und [Scott Addie](https://scottaddie.com)

Web-APIs sind HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen. ASP.NET Core MVC umfasst Unterstützung für das Erstellen von Web-APIs, die einen einzelnen, konsistenten lässt sich das Erstellen von Webanwendungen. In diesem Artikel veranschaulichen wir die erforderlichen Schritte zum Migrieren von einer Web-API-Implementierung von ASP.NET Web-API zu ASP.NET Core MVC.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Überprüfen Sie ASP.NET Web-API-Projekt

In diesem Artikel verwendet das Beispielprojekt *ProductsApp*, in dem Artikel erstellte [erste Schritte mit ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) als Ausgangspunkt erforderlich. In diesem Projekt wird ein einfache ASP.NET Web-API-Projekt wie folgt konfiguriert werden.

In *Global.asax.cs*, erfolgt ein Aufruf zum `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` wird definiert, *App_Start*, und weist nur eine statische `Register` Methode:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Diese Klasse konfiguriert [routing-Attribut](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), obwohl sie nicht tatsächlich im Projekt verwendet wird. Außerdem konfiguriert die Routingtabelle der von ASP.NET Web-API verwendet wird. In diesem Fall wird ASP.NET Web API URLs entsprechend das Format erwarten */api/ {Controller} / {Id}*, mit *{Id}* wird optional.

Die *ProductsApp* -Projekt enthält nur eine einfache Controller, der vom erbt `ApiController` und macht zwei Methoden verfügbar:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Zum Schluss das Modell *Produkt*, verwendet der *ProductsApp*, ist eine einfache Klasse:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Nun, da wir ein einfaches Projekt aus dem gestartet haben, können wir diese Web-API-Projekts zu ASP.NET Core MVC migrieren veranschaulicht.

## <a name="create-the-destination-project"></a>Erstellen Sie das Zielprojekt

Erstellen Sie eine neue, leere Projektmappe mithilfe von Visual Studio, und nennen Sie sie *WebAPIMigration*. Fügen Sie der vorhandenen *ProductsApp* Projekt, und dann eine neue ASP.NET Core Webanwendungsprojekt zur Projektmappe hinzufügen. Nennen Sie das neue Projekt *ProductsCore*.

![Dialogfeld "Neues Projekt" öffnen, um Webvorlagen](webapi/_static/add-web-project.png)

Wählen Sie anschließend die Web-API-Projektvorlage aus. Wir migrieren der *ProductsApp* Inhalt in dieses neue Projekt.

![Dialogfeld "neues Web Application" mit Web-API-Projektvorlage, die in der Liste der Vorlagen ASP.NET Core ausgewählt](webapi/_static/aspnet-5-webapi.png)

Löschen der `Project_Readme.html` Datei aus dem neuen Projekt. Die Projektmappe sollte jetzt wie folgt aussehen:

![Öffnen Sie im Projektmappen-Explorer mit Dateien und Ordner der Projekte ProductsApp und ProductsCore webanwendungslösung](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrieren der Konfiguration

Nicht mehr mithilfe von ASP.NET Core *"Global.asax"*, *"Web.config"*, oder *App_Start* Ordner. Alle starttasks erfolgen dagegen *Startup.cs* im Stammverzeichnis des Projekts (finden Sie unter [Anwendungsstart](../fundamentals/startup.md)). In ASP.NET-MVC Core attributbasierte routing ist jetzt standardmäßig enthalten, wenn `UseMvc()` aufgerufen wird; dies ist die empfohlene Vorgehensweise zum Konfigurieren von Routen für Web-API (und ist wie das Startprojekt für die Web-API behandelt routing).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

Angenommen, Sie in Ihrem Projekt vorwärts routing-Attribut verwenden möchten, ist keine zusätzliche Konfiguration erforderlich. Wenden Sie die Attribute nach Bedarf auf Ihre Controller und Aktionen, wie im Beispiel erfolgt einfach `ValuesController` -Klasse, die in das Startprojekt für die Web-API enthalten ist:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Beachten Sie das Vorhandensein von *[Controller]* in Zeile 8. Attribut-basierter jetzt routing unterstützt bestimmte-Token, wie z. B. *[Controller]* und *[Aktion]*. Diese Token werden zur Laufzeit mit dem Namen des Controllers oder Aktion, bzw. ersetzt, der das Attribut angewendet wurde. Dies dient zum Reduzieren der Anzahl der Magic-Zeichenfolgen im Projekt, und es wird sichergestellt, dass die Routen verbleiben synchronisiert mit ihren entsprechenden Controller und Aktionen wenn automatische Umbenennung Refactorings angewendet werden.

Um die Produkte-API-Controller zu migrieren, müssen wir zuerst kopieren *ProductsController* in das neue Projekt. Klicken Sie dann einfach enthalten Sie das routenattribut auf dem Controller:

```csharp
[Route("api/[controller]")]
```

Sie müssen auch hinzufügen der `[HttpGet]` -Attribut auf die beiden Methoden, da beide über HTTP Get aufgerufen werden soll. Der Erwartung, dass ein Parameter "Id" im Attribut enthalten `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

An diesem Punkt ist das routing richtig konfiguriert. jedoch können wir noch es nicht testen. Müssen weitere Änderungen vorgenommen werden, bevor *ProductsController* kompiliert.

## <a name="migrate-models-and-controllers"></a>Migrieren von Modellen und Controllern

Der letzte Schritt des Migrationsvorgangs für dieses einfache Web-API-Projekt ist, kopieren Sie die Controller und alle Modelle, die sie verwenden. In diesem Fall einfach kopieren *Controllers/ProductsController.cs* aus dem ursprünglichen Projekt in das neue Projekt. Kopieren Sie Sie dann den gesamten Ordner Models aus dem ursprünglichen Projekt in das neue Projekt. Passen Sie die Namespaces, um den neuen Projektnamen angepasst (*ProductsCore*).  An diesem Punkt können Sie die Anwendung erstellen, und finden Sie eine Anzahl von Kompilierungsfehlern. Diese sollte im Allgemeinen in den folgenden Kategorien fallen:

* *ApiController* ist nicht vorhanden

* *System.Web.Http* Namespace ist nicht vorhanden.

* *IHttpActionResult* ist nicht vorhanden

Glücklicherweise sind dies so beheben alle sehr einfach:

* Änderung *ApiController* auf *Controller* (müssen Sie möglicherweise hinzufügen *mit Microsoft.AspNetCore.Mvc*)

* Löschen Sie alle mit Verweisen auf Anweisung *System.Web.Http*

* Ändern Sie eine beliebige Methode zurückgeben *IHttpActionResult* zurückzugebenden eine *IActionResult*

Nachdem diese Änderungen wurden vorgenommen und nicht verwendete using-Anweisungen entfernt, wird das migrierte *ProductsController* Klasse sieht wie folgt aus:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Sie sollten jetzt möglich, führen Sie die migrierte Projekt, und navigieren Sie zu */api/Products*; und die vollständige Liste der 3 Produkte angezeigt. Navigieren Sie zu */api/products/1* und das erste Produkt sollte angezeigt werden.

## <a name="summary"></a>Zusammenfassung

Migrieren eines einfachen ASP.NET Web-API-Projekts zu ASP.NET Core MVC ist recht einfach, Dank an die integrierte Unterstützung für Web-APIs in ASP.NET Core MVC. Die Hauptteilen jedes ASP.NET Web-API-Projekt migrieren müssen werden Routen, Controllern und Modelle, zusammen mit Updates für die Typen von Controllern und Aktionen verwendet.
