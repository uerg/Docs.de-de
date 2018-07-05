---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Einführung in ASP.NET MVC 3 (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 1a870f10d39980335c7acb4a543907fecd408eb2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372352"
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Einführung in ASP.NET MVC 3 (VB)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:

- [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Ein Visual Web Developer-Projekt mit VB-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die VB-Version hier](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Falls CSharp gewünscht, wechseln Sie zu der [CSharp Version](../cs/intro-to-aspnet-mvc-3.md) in diesem Tutorial.

## <a name="what-youll-build"></a>Sie lernen Folgendes

Implementieren Sie eine einfache Auflistung von Movie-Anwendung, die unterstützt werden, erstellen, bearbeiten und Auflisten von Filme aus einer Datenbank. Im folgenden sind die beiden Screenshots der Anwendung, die Sie erstellen. Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Die Anwendung kann auch hinzufügen, bearbeiten und Löschen von Filmen sowie finden Sie Details für einzelne Elemente. Alle Dateneingabe-Szenarien umfassen das Überprüfung sicher, dass die in der Datenbank gespeicherten Daten richtig ist.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, mit denen, die Sie lernen Folgendes

Hier ist Sie lernen Folgendes:

- Gewusst wie: Erstellen eines neuen ASP.NET MVC-Projekts
- Vorgehensweise: erstellen eine neue Datenbank mit Entity Framework Code first
- Vorgehensweise: Erstellen von ASP.NET MVC-Controllern und-Ansichten
- Das Abrufen und Anzeigen von Daten
- Bearbeiten von Daten, und aktivieren die datenüberprüfung

## <a name="getting-started"></a>Erste Schritte

Starten Sie durch Ausführen von Visual Web Developer 2010 Express (kurz "VWD"), und wählen Sie **neues Projekt** aus der **starten** Seite.

Visual Web Developer ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. In Visual Web Developer ist eine Symbolleiste entlang des oberen mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. anstelle **neues Projekt** aus der **starten** Seite können Sie das Menü und auswählen **Datei** &gt; **neues Projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können mit der Programmiersprache Ihrer Wahl von Visual Basic oder Visual C#-Anwendungen erstellen. Für dieses Lernprogramm wählen Sie die Visual Basic auf der linken Seite, und wählen Sie dann **ASP.NET MVC 3-Webanwendung**. Benennen Sie das Projekt "MvcMovie", und klicken Sie dann auf **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Lassen Sie **Razor** als die standardansichts-Engine.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Klicken Sie auf **OK**. Visual Web Developer verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache "Hello World!" Projekt, und es ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggings mit F5 ist.

F5 bewirkt, dass Visual Web Developer zum Starten von eines Development Webservers, und führen Sie Ihre Webanwendung. VWD dann startet einen Browser und Startseite der Anwendung wird geöffnet. Beachten Sie, dass mit dem Text die Adressleiste des Browsers `localhost` und nicht etwas wie `example.com`. Der Grund dafür ist `localhost` verweist immer auf Ihre eigenen lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie gerade erstellt haben. Wenn VWD ein Webprojekt ausgeführt wird, wird ein zufälliger Port für das Projekt verwendet. In der folgenden Abbildung ist die Anzahl der zufälligen Port 43246. Das Projekt wird wahrscheinlich eine andere Portnummer verwenden.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Standardmäßig gibt dieser Standardvorlage Sie zwei Seiten besuchen und eine einfache Anmeldeseite. Wir ändern, wie diese Anwendung funktioniert, und erfahren Sie etwas, Informationen zu ASP.NET MVC im Prozess. Schließen Sie Ihren Browser, und Ändern von Code.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
