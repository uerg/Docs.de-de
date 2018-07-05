---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Teil 3: Erstellen eines Administratorcontrollers | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: caf7649f2e420b4063966fd1a371c3d9a5eb4d04
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815456"
---
<a name="part-3-creating-an-admin-controller"></a>Teil 3: Erstellen eines Administratorcontrollers
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Hinzufügen eines Administratorcontrollers

In diesem Abschnitt fügen wir einen Web-API-Controller, der unterstützt CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge für Produkte. Der Controller wird Entity Framework verwenden, um mit der Datenbankebene kommunizieren zu können. Nur Administratoren werden dieser Controller verwenden können. Kunden werden die Produkte über einen anderen Controller zugreifen.

Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen** und dann **Controller**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller `AdminController`. Klicken Sie unter **Vorlage**Option &quot;API-Controller mit Lese-/schreibaktionen, mithilfe von Entity Framework&quot;. Klicken Sie unter **Modellklasse**, wählen Sie "Product (ProductStore.Models)". Klicken Sie unter **Datenkontext**wählen "&lt;neuer Datenkontext&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Wenn die **Modellklasse** Dropdownliste zeigt keine Modellklassen und stellen Sie sicher, dass Sie das Projekt kompiliert. Entitätsframework verwendet Reflektion, daher muss die kompilierte Assembly festgelegt.


Auswählen von "&lt;neuer Datenkontext&gt;" wird geöffnet, die **neuer Datenkontext** Dialogfeld. Benennen Sie den Datenkontext `ProductStore.Models.OrdersContext`.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Klicken Sie auf **OK** zum Verwerfen der **neuer Datenkontext** Dialogfeld. In der **Controller hinzufügen** Dialogfeld klicken Sie auf **hinzufügen**.

Hier ist, was dem Projekt hinzugefügt haben:

- Eine Klasse namens `OrdersContext` abgeleitet, die **"DbContext"**. Diese Klasse stellt die Verbindung zwischen der POCO-Modelle und die Datenbank.
- Eine Web-API-Controller mit dem Namen `AdminController`. Dieser Controller unterstützt CRUD-Vorgänge auf `Product` Instanzen. Er verwendet den `OrdersContext` Klasse für die Kommunikation mit Entity Framework.
- Eine neue Datenbank-Verbindungszeichenfolge in der Datei "Web.config".

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Öffnen Sie die OrdersContext.cs-Datei. Beachten Sie, dass der Konstruktor den Namen der Verbindungszeichenfolge der Datenbank angibt. Dieser Name bezieht sich auf die Verbindungszeichenfolge, die Datei "Web.config" hinzugefügt wurde.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Fügen Sie der Klasse `OrdersContext` die folgenden Eigenschaften hinzu:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Ein **"DbSet"** stellt einen Satz von Entitäten, die abgefragt werden können. Hier ist die vollständige Auflistung für die `OrdersContext` Klasse:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

Die `AdminController` -Klasse definiert fünf Methoden, die grundlegende CRUD-Funktionen zu implementieren. Jede Methode entspricht ein URI, der der Client aufrufen kann:

| Controller-Methode | Beschreibung | URI | HTTP-Methode |
| --- | --- | --- | --- |
| GetProducts | Ruft alle Produkte ab. | API/Produkte | GET |
| GetProduct | Sucht nach einem Produkt nach ID auf. | API/Produkte/*Id* | GET |
| PutProduct | Ein Produkt wird aktualisiert. | API/Produkte/*Id* | PUT |
| PostProduct | Erstellt ein neues Produkt an. | API/Produkte | POST |
| DeleteProduct | Löscht ein Produkt. | API/Produkte/*Id* | DELETE |

Jede Methode ruft `OrdersContext` zum Abfragen der Datenbank. Rufen Sie die Methoden, die die Auflistung aus (PUT, POST und DELETE) geändert werden `db.SaveChanges` , die Änderungen in der Datenbank beizubehalten. Controller pro HTTP-Anforderung erstellt und dann verworfen, daher ist es erforderlich, um Änderungen permanent zu speichern, bevor eine Methode einen Wert zurückgibt.

## <a name="add-a-database-initializer"></a>Fügen Sie einen Datenbankinitialisierer hinzu.

Entitätsframework hat ein nützliches Feature, mit dem Sie das Auffüllen der Datenbank beim Start und die Datenbank automatisch neu, wenn die Modelle ändern. Dieses Feature ist während der Entwicklung nützlich, da Sie einige Testdaten immer haben, auch wenn Sie die Modelle ändern.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Models", und erstellen Sie eine neue Klasse namens `OrdersContextInitializer`. Fügen Sie in der folgenden Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Durch Erben von der **DropCreateDatabaseIfModelChanges** -Klasse, weisen wir Entity Framework für die Datenbank zu löschen, wenn wir die Modellklassen ändern. Wenn Entity Framework erstellt (oder neu erstellt) die Datenbank, ruft er die **Ausgangswert** Methode, um Tabellen aufzufüllen. Wir verwenden die **Ausgangswert** Methode, um einige Beispiel-Produkte sowie eine Beispiel-Reihenfolge hinzuzufügen.

Diese Funktion ist gut für Tests geeignet, aber verwenden Sie nicht die **DropCreateDatabaseIfModelChanges** Klasse in der Produktion, da Sie die Daten verlieren können, wenn jemand eine Modellklasse ändert.

Öffnen Sie "Global.asax", und fügen Sie den folgenden Code der **Anwendung\_starten** Methode:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Senden Sie eine Anforderung an den Controller

An diesem Punkt nicht getan haben wir keinen Clientcode geschrieben, aber Sie können die Web-API mit einem Webbrowser oder eine HTTP-debugging-wie Tool aufrufen [Fiddler](http://www.fiddler2.com/fiddler2/). Drücken Sie in Visual Studio F5, um mit dem Debuggen beginnen. Ihr Webbrowser wird geöffnet, um `http://localhost:*portnum*/`, wobei *Portnum* ist eine Portnummer.

Senden von HTTP-Anforderung an "`http://localhost:*portnum*/api/admin`. Die erste Anforderung nur langsam abgeschlossen ist, möglicherweise Entify-Framework zum Erstellen und das Seeding für die Datenbank benötigt. Die Antwort sollte etwa wie Folgendes:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-2.md)
> [Weiter](using-web-api-with-entity-framework-part-4.md)
