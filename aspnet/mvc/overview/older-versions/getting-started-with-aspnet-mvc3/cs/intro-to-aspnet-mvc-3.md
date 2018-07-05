---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
title: Einführung in ASP.NET MVC 3 (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 86a80b35-88bd-4b7c-bd58-f6e7997197d4
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: b0ff8da1911b36e6e74e5c7057f27d891ad57f61
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832288"
---
<a name="intro-to-aspnet-mvc-3-c"></a>Einführung in ASP.NET MVC 3 (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.
> 
> 
> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


## <a name="what-youll-build"></a>Sie lernen Folgendes

Implementieren Sie eine einfache Auflistung von Movie-Anwendung, die unterstützt werden, erstellen, bearbeiten und Auflisten von Filme aus einer Datenbank. Im folgenden sind die beiden Screenshots der Anwendung, die Sie erstellen. Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:

![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image1.png)

Die Anwendung kann auch hinzufügen, bearbeiten und Löschen von Filmen sowie finden Sie Details für einzelne Elemente. Alle Dateneingabe-Szenarien umfassen das Überprüfung sicher, dass die in der Datenbank gespeicherten Daten richtig ist.

![](intro-to-aspnet-mvc-3/_static/image2.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

Hier ist Sie lernen Folgendes:

- Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt.
- Vorgehensweise: Erstellen von ASP.NET MVC-Controller und Ansichten.
- Vorgehensweise: erstellen eine neue Datenbank mit Entity Framework Code First-Paradigmas.
- Informationen zum Abrufen und Anzeigen von Daten.
- Informationen zum Bearbeiten von Daten, und aktivieren Sie die datenüberprüfung.

## <a name="getting-started"></a>Erste Schritte

Starten Sie durch Ausführen von Visual Web Developer 2010 Express ("Visual Web Developer" kurz), und wählen Sie **neues Projekt** aus der **starten** Seite.

Visual Web Developer ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. In Visual Web Developer ist eine Symbolleiste entlang des oberen mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. anstelle **neues Projekt** aus der **starten** Seite können Sie das Menü und auswählen **Datei** &gt; **neues Projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können Anwendungen mithilfe von Visual Basic oder Visual c# als Programmiersprache erstellen. Wählen Sie Visual c# auf der linken Seite, und wählen Sie dann **ASP.NET MVC 3-Webanwendung**. Benennen Sie das Projekt "MvcMovie", und klicken Sie dann auf **OK**. (Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.)

![](intro-to-aspnet-mvc-3/_static/image5.png)

In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Überprüfen Sie **mit HTML5-Markup** und lassen Sie **Razor** als die standardansichts-Engine.

![](intro-to-aspnet-mvc-3/_static/image6.png)

Klicken Sie auf **OK**. Visual Web Developer verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache "Hello World!" Projekt, und es ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

[![](intro-to-aspnet-mvc-3/_static/image8.png)](intro-to-aspnet-mvc-3/_static/image7.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-3/_static/image9.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggings mit F5 ist.

F5 bewirkt, dass Visual Web Developer zum Starten von eines Development Webservers, und führen Sie Ihre Webanwendung. Visual Web Developer startet einen Browser dann und Startseite der Anwendung wird geöffnet. Beachten Sie, dass mit dem Text die Adressleiste des Browsers `localhost` und nicht etwas wie `example.com`. Der Grund dafür ist `localhost` verweist immer auf Ihre eigenen lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie gerade erstellt haben. Wenn Visual Web Developer ein Webprojekt ausgeführt wird, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Anzahl der zufälligen Port 43246. Wenn Sie die Anwendung ausführen, sehen Sie wahrscheinlich eine andere Portnummer an.

![](intro-to-aspnet-mvc-3/_static/image10.png)

Anwendungstelemetriedaten ermöglicht dieser Standardvorlage finden Sie auf zwei Seiten und eine einfache Anmeldeseite. Der nächste Schritt besteht, zu ändern, wie diese Anwendung funktioniert, und erfahren etwas, Informationen zu ASP.NET MVC im Prozess. Schließen Sie Ihren Browser, und Ändern von Code.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
