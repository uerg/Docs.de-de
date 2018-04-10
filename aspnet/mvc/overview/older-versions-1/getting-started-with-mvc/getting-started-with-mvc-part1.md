---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Einführung in ASP.NET MVC | Microsoft Docs
author: shanselman
description: Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 476d832e389b9b5a26fe2d552ca648c79b100056
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="intro-to-aspnet-mvc"></a>Einführung in ASP.NET MVC
====================
durch [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version ist in diesem Lernprogramm verfügbar [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Lernprogramm verwendet ASP.NET MVC 5, viele Verbesserungen über dieses Lernprogramms bereitstellt.
> 
> 
> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


Wir machen unsere ersten ASP.NET-MVC-Webanwendung mit [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Treffen wir eine kleine Film-List-Anwendung, die wir erstellen und Filme auflisten.

## <a name="what-youll-build"></a>Was müssen Sie erstellen

Hier sind zwei Screenshots der Anwendung, die Sie erstellen müssen. Sie müssen eine einfache Tabelle von Filmen mit verschiedenen Spalten.

[![Film List – Windows Internet Explorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Und Sie müssen ein Formular erstellen, damit Filme zur Liste hinzugefügt werden können.

[![Create a Movie - Windows Internet Explorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Web-Anwendung mit Visual Studio. Erfahren Sie:

- Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt
- Gewusst wie: Erstellen einer neuen Datenbank mit SQL Server
- Vorgehensweise: Erstellen von ASP.NET MVC-Controller und Ansichten
- Das Abrufen und Anzeigen von Daten
- Zum Bearbeiten von Daten und aktivieren Sie die datenüberprüfung
- Vorgehensweise beim Aktualisieren des Datenbankschemas

## <a name="get-started"></a>Erste Schritte

Starten Sie durch Ausführen von Visual Web Developer 2010 Express (Ich werde sie von nun an "VWD" aufrufen), und wählen Sie Neues Projekt aus den Startbildschirm.

Visual Web Developer ist eine IDE oder Integrated Developer Environment an. Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen. Es wird eine Symbolleiste am oberen Rand mit verschiedenen verfügbaren Optionen, die Sie als auch im Menü, das Sie hätte auch auf die Datei auswählen | Neues Projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen einer ersten Anwendung

Sie können Anwendungen mit Visual Basic oder Visual c# erstellen. Jetzt, wählen Sie Visual c# auf der linken Seite Wählen Sie "ASP.NET MVC 2-Webanwendung". Namen für das Projekt "Videos", und klicken Sie auf OK.

[![Neues Projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Auf die rechte Seite ist im Projektmappen-Explorer die Dateien und Ordner in der Anwendung angezeigt. Das große Fenster in der Mitte ist, in dem Code bearbeiten und die meiste Ihrer Zeit in Anspruch. Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche! Dies ist eine einfache "Hello World! Projekt, und es ist eine gute, für die Anwendung zu starten.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Wählen Sie die Schaltfläche "Wiedergabe" auf der Symbolleiste.

![Debugging starten](getting-started-with-mvc-part1/_static/image11.png)

Es ist ein grüner Pfeil nach rechts, die das Programm zu kompilieren und starten Sie die Anwendung in einem Webbrowser.

*Hinweis: Sie können stattdessen drücken Sie F5, oder wählen Sie Debug -&gt;Starten des Debuggens aus dem Menü "Debuggen".*

Dadurch wird Visual Web Developer zum Starten eines entwicklungswebservers, und führen unsere Webanwendung (es gibt keine Konfiguration oder manuelle Schritte erforderlich, um dies zu ermöglichen). Anschließend wird einen Browser starten und konfigurieren, dass die Anwendung-Startseite zu navigieren. Unten können sehen Sie, dass die Adressleiste des Browsers "Localhost" und nicht etwa ' example.com ' lautet. Das liegt daran "localhost" stets auf Ihren eigenen lokalen Computer - Zeigt die in diesem Fall die Anwendung ausgeführt wird, die wir gerade erstellt.

[![Startseite](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Direktes bietet diese Standardvorlage Sie zwei Seiten besuchen und eine grundlegende Anmeldeseite. Wir ändern, wie diese Anwendung funktioniert, und erfahren Sie etwas ASP.NET-MVC im Prozess. Schließen Sie Ihren Browser, und ermöglicht es, Code zu ändern.

> [!div class="step-by-step"]
> [Nächste](getting-started-with-mvc-part2.md)
