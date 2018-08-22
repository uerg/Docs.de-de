---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Einführung in ASP.NET MVC 4 | Microsoft-Dokumentation
author: Rick-Anderson
description: Eine aktualisierte Version, wenn in diesem Tutorial verwendet Visual Studio 2013 verfügbar ist. Das neue Tutorial verwendet ASP.NET MVC 5, bietet viele Verbesserungen für t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 6442477c22c124f782642b770f9394873a988e85
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827309"
---
<a name="intro-to-aspnet-mvc-4"></a>Einführung in ASP.NET MVC 4
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Eine aktualisierte Version ist in diesem Tutorial verfügbaren [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Tutorial verwendet ASP.NET MVC 5, die viele Verbesserungen für dieses Tutorial bietet.
> 
> Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET MVC 4-Web-Anwendung mithilfe von Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 wird empfohlen, Sie nicht für dieses Tutorial installieren müssen. Wenn Sie Visual Studio 2010 verwenden, müssen Sie die unten aufgelisteten Komponenten installieren. Sie können alle installieren, indem Sie auf den folgenden Links:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [WPI Installer für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die [WPI Installer für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) und: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> In diesem Tutorial führen Sie die Anwendung in Visual Studio. Sie können auch die Anwendung verfügbar über das Internet, indem sie in einem Hostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem [kostenlose Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Informationen dazu, wie Sie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen und Bereitstellen einer ASP.NET-Website und SQL-Datenbank mit Visual Studio](https://docs.microsoft.com/dotnet/azure/). In diesem Tutorial wird gezeigt, wie Entity Framework Code First-Migrationen, die zum Bereitstellen von SQL Server-Datenbank auf Windows Azure SQL-Datenbank (früher SQL Azure) verwenden.
> 
> In diesem Tutorial wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Sie lernen Folgendes

> [!NOTE]
> Eine aktualisierte Version ist in diesem Tutorial verfügbaren [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Tutorial verwendet ASP.NET MVC 5, die viele Verbesserungen für dieses Tutorial bietet.


Implementieren Sie eine einfache Auflistung von Movie-Anwendung, die unterstützt werden, erstellen, bearbeiten, suchen und Auflisten von Filme aus einer Datenbank. Im folgenden sind die beiden Screenshots der Anwendung, die Sie erstellen. Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Die Anwendung kann auch hinzufügen, bearbeiten und Löschen von Filmen sowie finden Sie Details für einzelne Elemente. Alle Dateneingabe-Szenarien umfassen das Überprüfung sicher, dass die in der Datenbank gespeicherten Daten richtig ist.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Erste Schritte

Visual Studio Express 2012 oder Visual Web Developer 2010 Express ausführen, um zu beginnen. Die meisten die Screenshots in dieser Serie Visual Studio Express 2012, aber Sie können dieses Tutorial mit Visual Studio 2010 SP1, Visual Studio 2012, Visual Studio Express 2012 oder Visual Web Developer 2010 Express ausführen. Wählen Sie **neues Projekt** aus der **starten** Seite.

Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. In Visual Studio wird eine Symbolleiste entlang des oberen mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. anstelle **neues Projekt** aus der **starten** Seite können Sie das Menü und auswählen **Datei** &gt; **neues Projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Sie können Anwendungen mithilfe von Visual Basic oder Visual c# als Programmiersprache erstellen. Wählen Sie Visual c# auf der linken Seite, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**. Benennen Sie Ihr Projekt &quot;MvcMovie&quot; , und klicken Sie dann auf **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Lassen Sie **Razor** als die standardansichts-Engine.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Klicken Sie auf **OK**. Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache &quot;Hello World!&quot; -Projekt, und die ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggings mit F5 ist.

F5 bewirkt, dass Visual Studio zum Starten von IIS Express und die Webanwendung ausgeführt wird. Visual Studio klicken Sie dann einen Browser und Startseite der Anwendung wird geöffnet. Beachten Sie, dass mit dem Text die Adressleiste des Browsers `localhost` und nicht etwas wie `example.com`. Der Grund dafür ist `localhost` verweist immer auf Ihre eigenen lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie gerade erstellt haben. Wenn Visual Studio ein Webprojekt ausgeführt wird, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Nummer des Ports 41788. Wenn Sie die Anwendung ausführen, sehen Sie wahrscheinlich eine andere Portnummer an.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Anwendungstelemetriedaten bietet dieser Standardvorlage Seiten "Home", "Wenden Sie sich an" und "über. Außerdem unterstützt die Registrierung und Anmeldung, und enthält links zu Facebook und Twitter. Der nächste Schritt besteht, zu ändern, wie diese Anwendung funktioniert, und erfahren etwas zu ASP.NET MVC. Schließen Sie Ihren Browser, und Ändern von Code.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
