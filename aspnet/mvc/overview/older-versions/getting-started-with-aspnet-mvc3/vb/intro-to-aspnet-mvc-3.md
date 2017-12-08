---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: "Einführung in ASP.NET MVC 3 (VB) | Microsoft Docs"
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: f0717e8d635818322be8b3242efe756a20351340
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="intro-to-aspnet-mvc-3-vb"></a>Einführung in ASP.NET MVC 3 (VB)
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.


In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:

- [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)

Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Ein Visual Web Developer-Projekt mit Quellcode VB ist zu diesem Thema steht zur Verfügung. [Laden Sie die VB-Version hier](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Falls CSharp gewünscht, wechseln Sie zu der [CSharp Version](../cs/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.

## <a name="what-youll-build"></a>Was müssen Sie erstellen

Implementieren Sie eine einfache Anwendung von Film-Auflistung, die unterstützt werden, erstellen, bearbeiten und Auflisten von Filmen aus einer Datenbank. Im folgenden sind zwei Screenshots der Anwendung, die Sie erstellen müssen. Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

Die Anwendung lässt auch hinzufügen, bearbeiten und Löschen von Filmen sowie ausführliche Informationen finden Sie Details zu einzelnen diejenigen. Alle Dateneingabe Szenarien umfassen die Überprüfung aus, um sicherzustellen, dass die in der Datenbank gespeicherten Daten richtig ist.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Fähigkeiten, die Sie erfahren

Hier ist Sie lernen:

- Gewusst wie: Erstellen eines neuen ASP.NET MVC-Projekts
- Gewusst wie: Erstellen einer neuen Datenbank mithilfe von Entity Framework Code first
- Vorgehensweise: Erstellen von ASP.NET MVC-Controller und Ansichten
- Das Abrufen und Anzeigen von Daten
- Zum Bearbeiten von Daten und aktivieren Sie die datenüberprüfung

## <a name="getting-started"></a>Erste Schritte

Starten Sie durch Ausführen von Visual Web Developer 2010 Express (kurz "VWD"), und wählen Sie **neues Projekt** aus der **starten** Seite.

Visual Web Developer ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Web Developer ist eine Symbolleiste am oberen Rand mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. statt der Auswahl **neues Projekt** aus der **starten** Seite können Sie mithilfe des Menüs und auswählen **Datei** &gt; **neues Projekt**.)

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Erstellen einer ersten Anwendung

Sie können Anwendungen mit Ihrer Wahl von Visual Basic oder Visual c# als Programmiersprache erstellen. Für dieses Lernprogramm wählen Sie auf der linken Seite Visual Basic, und wählen Sie dann **ASP.NET MVC 3-Webanwendung**. Namen für das Projekt "MvcMovie", und klicken Sie dann auf **OK**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Lassen Sie **Razor** als Standardansichtsmodul.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Klicken Sie auf **OK**. Visual Web Developer verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche! Dies ist eine einfache "Hello World!" der Projekt, und es werden eine gute zum Starten der Anwendung.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggings F5.

F5 bewirkt, dass Visual Web Developer zum Starten von eines Development Webservers, und führen Sie die Webanwendung. VWD anschließend startet dieser einen Browser und Startseite der Anwendung geöffnet. Beachten Sie, die besagt, die Adressleiste des Browsers dass `localhost` und nicht etwas wie `example.com`. Grund hierfür ist, `localhost` verweist immer auf Ihrem lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie soeben erstellt haben. Wenn VWD ein Webprojekt ausgeführt wird, wird für das Projekt ein zufälliger Port verwendet. In der folgenden Abbildung ist die Anzahl der zufälligen Port 43246. Wahrscheinlich verwendet das Projekt eine andere Portnummer an.

![](intro-to-aspnet-mvc-3/_static/image12.png)

Direktes bietet diese Standardvorlage Sie zwei Seiten besuchen und eine grundlegende Anmeldeseite. Wir ändern, wie diese Anwendung funktioniert, und erfahren Sie etwas ASP.NET-MVC im Prozess. Schließen Sie Ihren Browser, und ändern Sie Code.

>[!div class="step-by-step"]
[Nächste](adding-a-controller.md)
