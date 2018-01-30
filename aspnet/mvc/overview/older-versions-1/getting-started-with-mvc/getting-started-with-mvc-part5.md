---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
title: Zugriff auf das Modell Daten aus einem Controller | Microsoft Docs
author: shanselman
description: "Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 004703cd-e0e9-4ba7-9974-1b0475c71222
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part5
msc.type: authoredcontent
ms.openlocfilehash: cf81d351aef45af3640f5d113eb3619911e03606
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="accessing-your-models-data-from-a-controller"></a>Zugriff auf das Modell Daten aus einem Controller
====================
durch [Scott Hanselman](https://github.com/shanselman)

> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


In diesem Abschnitt werden wir eine neue MoviesController-Klasse erstellen und Code schreiben, der unsere Filmdaten abgerufen und wieder an den Browser, die mithilfe einer Ansichtenvorlage angezeigt.

Klicken Sie mit der rechten Maustaste auf den Ordner Controller, und stellen Sie eine neue MoviesController.

[![Hinzufügen eines Controllers](getting-started-with-mvc-part5/_static/image2.png)](getting-started-with-mvc-part5/_static/image1.png)

Dadurch wird eine neue "MoviesController.cs" Datei unterhalb unsere \Controllers Ordner im Projekt erstellt. Wir aktualisieren die MovieController, um die Liste von Filmen aus unserer neu aufgefüllt Datenbank abzurufen.

[!code-csharp[Main](getting-started-with-mvc-part5/samples/sample1.cs)]

Wir Ausführen eine LINQ-Abfrage, damit wir nur freigegeben, nachdem die Sommer 1984 Filme abgerufen werden. Benötigen wir eine Vorlage anzeigen, diese Liste von Filmen wieder zu rendern, also mit der rechten Maustaste in der Methode und Auswählen von "Ansicht hinzufügen", um ihn zu erstellen.

In das Dialogfeld "Ansicht hinzufügen" fügen wir anzugeben, dass wir eine Liste übergeben&lt;Movies.Models.Movie&gt; zu unserem Vorlage anzeigen. Im Gegensatz zum älteren Zeiten, die wir das Dialogfeld "Ansicht hinzufügen" verwendet und eine "Empty" Vorlage erstellen möchten, möchten wir anzugeben, dass fügen diesmal wir Visual Studio automatisch "eine Ansichtenvorlage für uns mit einigen Standardinhalt Gerüst". Wir müssen dazu "Listenelement" in "Content-Dropdownliste im Menü Ansicht.

Beachten Sie, dass, wenn Sie ein erstelltes haben eine neue Klasse, Sie für die Anwendungskompilierung dafür in der Ansicht-Dialogfeld angezeigt müssen.

![Fügen Sie die Ansicht](getting-started-with-mvc-part5/_static/image3.png)

Klicken Sie auf Hinzufügen, und das System generiert automatisch den Code für eine Ansicht für uns, die die Liste von Filmen anzeigt. Dies ist ein guter Zeitpunkt zum Ändern der &lt;h2&gt; Überschrift z. B. "Meine Film-Liste" wie zuvor mit der "Hello World" angezeigt.

[![Filme – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part5/_static/image5.png)](getting-started-with-mvc-part5/_static/image4.png)

Führen Sie die Anwendung, und besuchen Sie /Movies in der Adressleiste. Jetzt haben wir werden Daten aus der Datenbank mithilfe einer einfachen Abfrage innerhalb der Controller abgefragt und die Daten an eine Ansicht, die über Filme weiß zurückgegeben. Diese Sicht wird dann durch die Liste von Filmen stößt und eine Tabelle mit Daten für uns erstellt.

[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image7.png)](getting-started-with-mvc-part5/_static/image6.png)

Es wird nicht implementieren bearbeiten "," Details "und" Delete-Funktionalität mit dieser Anwendung - daher die Standardlinks brauchen wir, die die Vorlage Gerüst für uns erstellt. Öffnen Sie die /Movies/Index.aspx-Datei, und entfernen Sie sie.

Hier ist der Quellcode für wie unsere aktualisierte Ansichtenvorlage aussehen soll, sobald wir diese Änderungen vorzunehmen:

[!code-aspx[Main](getting-started-with-mvc-part5/samples/sample2.aspx)]

Es wird Links, die wir nicht brauchen, erstellen, sodass wir in diesem Beispiel gelöscht werden. Wir behält unsere Link neu erstellen, weiter darin! Hier sieht unsere app mit dieser Spalte entfernt.

[![Movie List - Windows Internet Explorer](getting-started-with-mvc-part5/_static/image9.png)](getting-started-with-mvc-part5/_static/image8.png)

Wir haben jetzt eine einfache Liste von unseren Filmdaten. Jedoch wenn wir den Link "Neu erstellen" klicken, wird einen Fehler erhalten, da er nicht verknüpft ist! Wir implementieren Sie eine Create Action-Methode, und ermöglichen einem Benutzer, geben neue Kinofilmen in die Datenbank.

>[!div class="step-by-step"]
[Zurück](getting-started-with-mvc-part4.md)
[Weiter](getting-started-with-mvc-part6.md)
