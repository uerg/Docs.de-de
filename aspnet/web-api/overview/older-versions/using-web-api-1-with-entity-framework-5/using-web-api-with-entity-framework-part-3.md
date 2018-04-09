---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Controllers Admin | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: 588d9d1b5d27759692cd840faabf2c3549c309d6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-3-creating-an-admin-controller"></a>Teil 3: Erstellen eines Admin-Controllers
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Fügen Sie einen Controller-Administrator hinzu

In diesem Abschnitt fügen wir einen Web-API-Controller, der CRUD unterstützt (erstellen, lesen, aktualisieren und löschen) Vorgänge für Produkte. Der Controller wird Entity Framework verwendet, für die Kommunikation mit der Datenbankebene. Nur Administratoren werden können diesen Controller verwendet. Kunden werden die Produkte über einen anderen Controller zugreifen.

Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen** und dann **Controller**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

In der **Controller hinzufügen** Dialogfeld Namen des Controllers `AdminController`. Klicken Sie unter **Vorlage**Option &quot;API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entity Framework&quot;. Klicken Sie unter **Modellschemas**, wählen Sie "Product (ProductStore.Models)". Klicken Sie unter **Datenkontext**Option "&lt;neuen Datenkontext&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Wenn die **Modellschemas** Dropdownliste zeigt keine Modellklassen stellen Sie sicher, dass Sie das Projekt kompiliert. Entity Framework verwendet Reflektion, damit sie die kompilierte Assembly benötigt.


Auswählen von "&lt;neuen Datenkontext&gt;" wird geöffnet, die **neuen Datenkontext** Dialogfeld. Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Klicken Sie auf **OK** beim Schließen der **neuen Datenkontext** Dialogfeld. In der **Controller hinzufügen** Dialogfeld klicken Sie auf **hinzufügen**.

Hier ist, was dem Projekt hinzugefügt haben:

- Eine Klasse namens `OrdersContext` abgeleitet, die **DbContext**. Diese Klasse stellt die Verbindung zwischen den POCO-Modellen und die Datenbank bereit.
- Eine Web-API-Controller mit dem Namen `AdminController`. Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen. Er verwendet die `OrdersContext` Klasse für die Kommunikation mit Entity Framework.
- Eine neue Datenbank-Verbindungszeichenfolge in der Datei "Web.config".

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Öffnen Sie die OrdersContext.cs-Datei. Beachten Sie, dass der Konstruktor gibt den Namen der Datenbank-Verbindungszeichenfolge an. Dieser Name bezieht sich auf die Verbindungszeichenfolge, die Datei "Web.config" hinzugefügt wurde.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Ein **DbSet** stellt eine Reihe von Entitäten, die abgefragt werden können. Hier wird die vollständige Auflistung für die `OrdersContext` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Die `AdminController` Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionalität zu implementieren. Jede Methode entspricht ein URI, der der Client aufrufen kann:

| Controllermethode | Beschreibung | URI | HTTP-Methode |
| --- | --- | --- | --- |
| GetProducts | Ruft alle Produkte ab. | API-Produkte | GET |
| GetProduct | Sucht nach einem Produkt nach ID auf. | api/products/*id* | GET |
| PutProduct | Aktualisiert ein Produkt. | api/products/*id* | PUT |
| PostProduct | Erstellt ein neues Produkt. | API-Produkte | BEREITSTELLEN |
| DeleteProduct | Löscht ein Produkt. | api/products/*id* | DELETE |

Jede Methode ruft `OrdersContext` zum Abfragen der Datenbank. Aufrufen von Methoden, die die Auflistung (PUT, POST und DELETE) ändern `db.SaveChanges` damit die Änderungen in der Datenbank beibehalten. Controller sind pro HTTP-Anforderung erstellt, und dann verworfen, daher ist es erforderlich, um Änderungen beizubehalten, bevor eine Methode zurückgegeben.

## <a name="add-a-database-initializer"></a>Hinzufügen eines Datenbankinitialisierers

Entity Framework ist ein nützliches Feature, mit dem Sie die füllen Sie die Datenbank beim Start und die Datenbank automatisch neu, wenn die Modelle ändern. Diese Funktion ist nützlich während der Entwicklung, da Sie immer einige Testdaten verfügen, auch wenn Sie die Modelle ändern.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner Models, und erstellen Sie eine neue Klasse mit dem Namen `OrdersContextInitializer`. Fügen Sie in der folgenden Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Durch Erben von der **DropCreateDatabaseIfModelChanges** -Klasse, sagen wir Entity Framework, die Datenbank abzulegen, wenn wir die Modellklassen ändern. Wenn Entity Framework erstellt (oder neu erstellt) die Datenbank, ruft er die **Ausgangswert** Methode, um die Tabellen aufgefüllt. Wir verwenden die **Ausgangswert** Methode, um einige Beispiel-Produkte sowie eine Beispiel-Bestellung hinzuzufügen.

Diese Funktion eignet sich hervorragend für das Testen, aber verwenden Sie nicht die **DropCreateDatabaseIfModelChanges** Klasse in der Produktion, da Ihre Daten beeinträchtigt werden könnte, wenn jemand eine Modellklasse ändert.

"Global.asax" öffnen, und fügen Sie folgenden Code, der **Anwendung\_starten** Methode:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Senden Sie eine Anforderung an den Controller

An diesem Punkt wir noch keinen Clientcode geschrieben, aber Sie können die Web-API mit einem Webbrowser oder ein HTTP-debugging-wie z. B. Tools Aufrufen [Fiddler](http://www.fiddler2.com/fiddler2/). Drücken Sie F5, um mit dem Debuggen beginnen, in Visual Studio. Ihr Browser wird geöffnet, um `http://localhost:*portnum*/`, wobei *Portnum* ist eine Portnummer.

Senden Sie eine HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`. Die erste Anforderung möglicherweise nur langsam abgeschlossen ist, möglicherweise Entify Framework zum Erstellen und den Ausgangswert für der Datenbank muss. Die Antwort sollte etwa ähnlich der folgenden:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)
