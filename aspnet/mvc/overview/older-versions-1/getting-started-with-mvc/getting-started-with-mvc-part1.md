---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
title: Einführung in ASP.NET MVC | Microsoft-Dokumentation
author: shanselman
description: Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Erstellen Sie eine einfache Webanwendung, die aus einer Datenbank liest und schreibt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: bf4a1c19-0a94-4208-b268-a96ddcf26946
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part1
msc.type: authoredcontent
ms.openlocfilehash: 408256d116a7e73e01c34b0a11881e14c5b8401d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385731"
---
<a name="intro-to-aspnet-mvc"></a>Einführung in ASP.NET MVC
====================
durch [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version ist in diesem Tutorial verfügbaren [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Tutorial verwendet ASP.NET MVC 5, die viele Verbesserungen für dieses Tutorial bietet.
> 
> 
> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


Lassen Sie uns unsere erste ASP.NET MVC-Webanwendung mit [Visual Web Developer 2010 Express](https://www.microsoft.com/express/Web/). Wir erstellen eine kleine Anwendung "Movie List", die wir erstellen und Liste von Filmen.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Hier sind die beiden Screenshots der Anwendung, die Sie erstellen. Sie haben eine einfache Tabelle mit den Filmen mit verschiedenen Spalten.

[![Filmliste – Windows InternetExplorer (12)](getting-started-with-mvc-part1/_static/image2.png)](getting-started-with-mvc-part1/_static/image1.png)

Und Sie müssen ein Formular erstellen, damit Filme der Liste hinzugefügt werden können.

[![Erstellen Sie einen Film - Windows InternetExplorer (2)](getting-started-with-mvc-part1/_static/image4.png)](getting-started-with-mvc-part1/_static/image3.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Webanwendung mithilfe von Visual Studio. Sie lernen Folgendes:

- Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt
- Gewusst wie: Erstellen einer neuen Datenbank mit SQL Server
- Gewusst wie: Erstellen von ASP.NET MVC-Controller und Ansichten
- Das Abrufen und Anzeigen von Daten
- Bearbeiten von Daten, und aktivieren die datenüberprüfung
- Wie Sie das Datenbankschema aktualisieren

## <a name="get-started"></a>Erste Schritte

Starten Sie durch Ausführen von Visual Web Developer 2010 Express (Ich werde sie von nun an "VWD" bezeichnet), und wählen Sie Neues Projekt aus der Startbildschirm ausgeblendet wird.

Visual Web Developer ist eine IDE oder Integrated Developer Environment. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. Es wird eine Symbolleiste am oberen mit verschiedenen Optionen zur Verfügung, die Sie als auch im Menü kann außerdem verwendet haben, um die Datei auswählen | Neues Projekt.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image6.png)](getting-started-with-mvc-part1/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können mit Visual Basic oder Visual C#-Anwendungen erstellen. Jetzt wählen Sie Visual C# -Code auf der linken Seite Wählen Sie dann "ASP.NET MVC 2-Webanwendung". Benennen Sie das Projekt "Movies", und klicken Sie auf OK.

[![Neues Projekt](getting-started-with-mvc-part1/_static/image8.png)](getting-started-with-mvc-part1/_static/image7.png)

Auf die rechte Seite ist im Projektmappen-Explorer mit der alle Dateien und Ordner in Ihrer Anwendung. Fenster in der Mitte große ist, in dem Sie Ihren Code bearbeiten und verbringen die meiste Zeit. Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache "Hello World! Projekt, und es ist ein guter Ausgangspunkt für die Anwendung zu starten.

[![Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part1/_static/image10.png)](getting-started-with-mvc-part1/_static/image9.png)

Wählen Sie die Schaltfläche "Wiedergabe" auf der Symbolleiste aus.

![Debugging starten](getting-started-with-mvc-part1/_static/image11.png)

Es ist ein grüner Pfeil auf der rechten Seite, die das Programm zu kompilieren und starten Sie die Anwendung in einem Webbrowser.

*Hinweis: Sie können stattdessen drücken Sie F5 auf der Tastatur, oder wählen Sie Debug -&gt;Starten des Debuggens aus dem Menü "Debuggen".*

Dadurch wird Visual Web Developer zum Starten von eines Development Web-Servers und Ausführen von unserer Webanwendung (es gibt keine Konfiguration oder manuelle Schritte erforderlich, um dies zu ermöglichen). Sie klicken Sie dann einen Browser starten und konfigurieren, dass die Startseite der Anwendung navigieren. Unten können sehen Sie, dass die Adressleiste des Browsers "Localhost", und nicht etwa "example.com" lautet. Das ist, da "localhost" auf Ihrem lokalen Computer - immer verweist, in dem in diesem Fall die Anwendung ausgeführt wird, die wir soeben erstellt haben.

[![Auf der Startseite](getting-started-with-mvc-part1/_static/image13.png)](getting-started-with-mvc-part1/_static/image12.png)

Standardmäßig gibt dieser Standardvorlage Sie zwei Seiten besuchen und eine einfache Anmeldeseite. Wir ändern, wie diese Anwendung funktioniert, und erfahren Sie etwas, Informationen zu ASP.NET MVC im Prozess. Schließen Sie Ihren Browser, und ermöglicht, Code zu ändern.

> [!div class="step-by-step"]
> [Nächste](getting-started-with-mvc-part2.md)
