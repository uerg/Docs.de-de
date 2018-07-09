---
title: Migrieren von ASP.NET Web-API in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie Sie eine Web-API-Implementierung von ASP.NET Web-API zu ASP.NET Core MVC zu migrieren.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894191"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>Migrieren von ASP.NET Web-API in ASP.NET Core

Von [Steve Smith](https://ardalis.com/) und [Scott Addie](https://scottaddie.com)

Web-APIs sind HTTP-Dienste, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten zu erreichen. ASP.NET Core MVC bietet Unterstützung für die Erstellung von Web-APIs, die eine einzelne, konsistente Möglichkeit zum Erstellen von Webanwendungen bereitgestellt wird. In diesem Artikel zeigen wir Ihnen die erforderlichen Schritte zum Migrieren von einer Web-API-Implementierung von ASP.NET Web-API ASP.NET Core Mvc.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Überprüfen Sie ASP.NET Web-API-Projekt

In diesem Artikel wird das Beispielprojekt *ProductsApp*, der in diesem Artikel [erste Schritte mit ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) als Ausgangspunkt erforderlich. In diesem Projekt ist ein einfaches ASP.NET Web-API-Projekt wie folgt konfiguriert werden.

In *"Global.asax.cs"*, erfolgt ein Aufruf zum `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` wird in definiert *App_Start*, und weist nur eine statische `Register` Methode:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Diese Klasse konfiguriert [attributrouting](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), obwohl sie nicht tatsächlich im Projekt verwendet wird. Außerdem konfiguriert die Routingtabelle, die von ASP.NET Web-API verwendet wird. In diesem Fall ASP.NET Web-API erwarten, dass Sie URLs für das Format entsprechen */api/ {Controller} / {Id}*, mit *{Id}* wird optional.

Die *ProductsApp* -Projekt enthält nur eine einfache Controller, der erbt `ApiController` und macht zwei Methoden verfügbar:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Abschließend das Modell *Produkt*verwendet, durch die *ProductsApp*, ist eine einfache Klasse:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Nun, wir ein einfaches Projekt, von dem aus gestartet haben, können wir diese Web-API-Projekts zu ASP.NET Core MVC migrieren veranschaulicht.

## <a name="create-the-destination-project"></a>Erstellen Sie das Zielprojekt

Erstellen Sie eine neue, leere Projektmappe mit Visual Studio, und nennen Sie es *WebAPIMigration*. Fügen Sie der vorhandenen *ProductsApp* Projekt darauf, und klicken Sie dann, fügen Sie der Projektmappe ein neues ASP.NET Core Web-Anwendungsprojekt. Nennen Sie das neue Projekt *ProductsCore*.

![Dialogfeld "Neues Projekt" öffnen, um Web-Vorlagen](webapi/_static/add-web-project.png)

Wählen Sie anschließend die Web-API-Projektvorlage aus. Wir migrieren die *ProductsApp* Inhalt in dieses neue Projekt.

![Dialogfeld "neue Webanwendung"-Web-API-Projektvorlage, die in der Liste der ASP.NET Core-Vorlagen ausgewählt](webapi/_static/aspnet-5-webapi.png)

Löschen der `Project_Readme.html` Datei aus dem neuen Projekt. Ihre Lösung sollte nun wie folgt aussehen:

![Anwendung geöffneten Projektmappe im Projektmappen-Explorer mit Dateien und Ordner der Projekte ProductsApp und ProductsCore](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Migrieren der Konfiguration

Nicht mehr mithilfe von ASP.NET Core *"Global.asax"*, *"Web.config"*, oder *App_Start* Ordner. Stattdessen werden alle starttasks ausgeführt, *"Startup.cs"* im Stammverzeichnis des Projekts (finden Sie unter [Anwendungsstart](../fundamentals/startup.md)). In ASP.NET Core MVC, attributbasierten routing ist jetzt standardmäßig enthalten, bei der `UseMvc()` aufgerufen wird; dies ist die empfohlene Vorgehensweise zum Konfigurieren von Web-API-Routen (und ist wie das Startprojekt für die Web-API behandelt routing).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Angenommen, Sie verwenden das attributrouting in Ihrem Projekt in Zukunft möchten, ist keine zusätzliche Konfiguration erforderlich. Wenden Sie einfach die Attribute, die nach Bedarf für Ihre Controller und Aktionen, wie im Beispiel der Fall ist `ValuesController` -Klasse, die im Starter-Web-API-Projekt enthalten ist:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Beachten Sie das Vorhandensein von *[Controller]* in Zeile 8. Attributbasierte jetzt routing unterstützt bestimmte Token, z. B. *[Controller]* und *[Action]*. Diese Token werden zur Laufzeit mit dem Namen des Controllers oder der Aktion bzw. ersetzt, der das Attribut angewendet wurde. Dies dient zum Reduzieren der Anzahl der Magic-Zeichenfolgen im Projekt, und es wird sichergestellt, dass es sich bei die Routen beibehalten dann mit ihren entsprechenden Controller und Aktionen automatisch umbenennen Refactorings angewendet werden.

Um Produkte API-Controller zu migrieren, müssen wir zuerst kopieren *ProductsController* in das neue Projekt. Klicken Sie dann enthalten Sie das routenattribut, auf dem Controller einfach:

```csharp
[Route("api/[controller]")]
```

Sie müssen auch hinzufügen der `[HttpGet]` -Attribut auf die beiden Methoden, da beide über HTTP-Get aufgerufen werden soll. Schließen Sie erwarten, dass ein Parameter "Id" in das Attribut für `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

An diesem Punkt ist das routing richtig konfiguriert. Wir können keine jedoch noch sie testen. Zusätzliche Änderungen müssen erfolgen, bevor *ProductsController* kompiliert.

## <a name="migrate-models-and-controllers"></a>Migrieren von Modellen und Controllern

Der letzte Schritt des Migrationsvorgangs für dieses einfache Web-API-Projekt ist, kopieren Sie die Controller und alle Modelle, die sie verwenden. Kopieren Sie in diesem Fall einfach *Controllers/ProductsController.cs* aus dem ursprünglichen Projekt in das neue Projekt. Kopieren Sie dann den gesamten Ordner "Models" aus dem ursprünglichen Projekt in das neue Projekt. Passen Sie die Namespaces entsprechend den neuen Projektnamen (*ProductsCore*).  An diesem Punkt können Sie die Anwendung erstellen, und sehen Sie eine Anzahl von Kompilierungsfehlern. Diese sollten in der Regel in der folgenden Kategorien fallen:

* *"ApiController"* ist nicht vorhanden

* *"System.Web.http"* Namespace ist nicht vorhanden.

* *IHttpActionResult* ist nicht vorhanden

Glücklicherweise sind dies alles sehr einfach zu beheben:

* Änderung *ApiController* zu *Controller* (müssen Sie möglicherweise hinzufügen *mit Microsoft.AspNetCore.Mvc*)

* Löschen Sie alle using-Anweisung, die auf *"System.Web.http"*

* Ändern Sie jede Methode zurückgeben *IHttpActionResult* zurückzugebenden eine *IActionResult*

Nachdem diese Änderungen wurden vorgenommen und nicht verwendete using-Anweisungen entfernt, wird das migrierte *ProductsController* Klasse sieht wie folgt aus:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Sie sollten jetzt in der Lage, führen Sie die migrierte Projekt, und navigieren Sie zu */api/Produkte*; und, sollte die vollständige Liste der 3 Produkte. Navigieren Sie zu */api/products/1* sollte das erste Produkt angezeigt.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>Kompatibilitäts-Shims für ASP.NET 4.x-Web-API 2

Ein nützliches Tool beim Migrieren von ASP.NET Web-API in ASP.NET Core-Projekte ist die [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) Bibliothek. Der Kompatibilitäts-Shims erweitert die ASP.NET Core, damit eine Reihe von anderen Web-API 2-Konventionen verwendet werden kann. Das Beispiel, das zuvor in diesem Dokument portiert ist einfach genug, dass der Kompatibilitäts-Shims nicht erforderlich war. Für größere Projekte kann mithilfe der Kompatibilitäts-Shims für vorübergehend die API-Lücke, zwischen ASP.NET Core und ASP.NET Web API 2 nützlich.

Die Web-API-kompatibilitätsshim dient als vorübergehende Maßnahme verwendet werden, um die Migration von Web-API-großprojekte zu ASP.NET Core zu vereinfachen. Im Laufe der Zeit sollten Projekte aktualisiert werden, um ASP.NET Core-Muster zu verwenden, statt die standardremotedatenbank von der Kompatibilitäts-Shims.

Kompatibilitätsfeatures in enthaltenen `Microsoft.AspNetCore.Mvc.WebApiCompatShim` enthalten:

* Fügt eine `ApiController` geben, damit Controllern Basistypen nicht aktualisiert werden müssen.
* Ermöglicht die modellbindung für die Web-API-Format. ASP.NET Core MVC-Modell Bindung funktioniert ähnlich wie MVC 5, in der Standardeinstellung. Der Shim-kompatibilitätsänderungen modellbindung ähnelt eher der Web-API 2-Bindung modellkonventionen sein. Komplexe Typen werden beispielsweise automatisch aus dem Anforderungstext gebunden.
* Modellbindung erweitert, sodass Controlleraktionen Parameter des Typs nutzen können `HttpRequestMessage`.
* Fügt der Nachrichtenformatierungsprogramme zulassen von Aktionen zum Zurückgeben von Ergebnissen vom Typ `HttpResponseMessage`.
* Fügt zusätzliche Response-Methoden, die Web-API 2-Aktionen verwendet haben können, um Antworten zu verarbeiten:
  * HttpResponseMessage-Generatoren:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Ergebnis-Aktionsmethoden:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Fügt eine Instanz der `IContentNegotiator` in der app DI-Container und macht content Negotiation-bezogenen Typen von [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) verfügbar. Dies schließt Typen ein, z. B. `DefaultContentNegotiator`, `MediaTypeFormatter`usw..

Um die Kompatibilitäts-Shims verwenden zu können, müssen Sie:

* Installieren Sie die [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet-Paket.
* Registrieren der Kompatibilitäts-Shims für die Dienste mit der app-DI-Container durch Aufrufen von `services.AddMvc().AddWebApiConventions()` der App `Startup.ConfigureServices` Methode.
* Definieren Sie die Web-API-spezifische Routen mit `MapWebApiRoute` auf die `IRouteBuilder` der App `IApplicationBuilder.UseMvc` aufrufen.

## <a name="summary"></a>Zusammenfassung

Migrieren eines einfachen ASP.NET Web-API-Projekts zu ASP.NET Core MVC ist recht einfach, Dank der integrierten Unterstützung für Web-APIs in ASP.NET Core MVC. Die Hauptkomponenten, die jedes ASP.NET Web-API-Projekt für die Migration benötigen werden Routen, Controllern und Modellen sowie Aktualisierungen für die Typen von Controllern und Aktionen verwendet.
