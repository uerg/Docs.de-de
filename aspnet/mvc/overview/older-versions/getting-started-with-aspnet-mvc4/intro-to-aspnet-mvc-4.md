---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Einführung in ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Eine aktualisierte Version dieses Lernprogramm hier mit Visual Studio 2013 verfügbar ist. Das neue Lernprogramm verwendet ASP.NET MVC 5, zahlreiche Verbesserungen über t bietet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 519bac22ba2607931c5f3123b9b567859a2b3d1c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="intro-to-aspnet-mvc-4"></a>Einführung in ASP.NET MVC 4
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Eine aktualisierte Version ist in diesem Lernprogramm verfügbar [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Lernprogramm verwendet ASP.NET MVC 5, viele Verbesserungen über dieses Lernprogramms bereitstellt.
> 
> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC 4-Web-Anwendung mithilfe von Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) oder Visual Web Developer 2010 Express Service Pack 1. Visual Studio 2012 wird empfohlen, Sie wird nicht nichts unternommen, um das Lernprogramm abgeschlossen installieren müssen. Wenn Sie Visual Studio 2010 verwenden, müssen Sie die folgenden Komponenten installieren. Alle von ihnen installieren, indem Sie den folgenden Links:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [WPI Installer für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, installieren Sie die [WPI Installer für ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) und den: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Die C#-Version herunterladen](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
> 
> In diesem Lernprogramm führen Sie die Anwendung in Visual Studio. Sie können auch die Anwendung verfügbar über das Internet, indem sie in einem Hostinganbieter bereitstellen. Microsoft bietet kostenlose Webhosting für bis zu 10 Websites in einer [frei von Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Weitere Informationen dazu, wie ein Visual Studio-Webprojekt auf einer Windows Azure-Website bereitstellen, finden Sie unter [erstellen und Bereitstellen einer ASP.NET-Website und SQL-Datenbank mit Visual Studio](https://docs.microsoft.com/dotnet/azure/). Dieses Lernprogramm wird gezeigt, wie Entity Framework Code First-Migrationen zu verwenden, um die SQL Server-Datenbank auf Windows Azure SQL-Datenbank (früher SQL Azure) bereitstellen.
> 
> Dieses Lernprogramm wurde von Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="what-youll-build"></a>Was müssen Sie erstellen

> [!NOTE]
> Eine aktualisierte Version ist in diesem Lernprogramm verfügbar [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Lernprogramm verwendet ASP.NET MVC 5, viele Verbesserungen über dieses Lernprogramms bereitstellt.


Implementieren Sie eine einfache Anwendung von Film-Auflistung, die unterstützt werden, erstellen, bearbeiten, suchen und Auflisten von Filmen aus einer Datenbank. Im folgenden sind zwei Screenshots der Anwendung, die Sie erstellen müssen. Es enthält eine Seite, die eine Liste von Filmen aus einer Datenbank anzeigt:

![](intro-to-aspnet-mvc-4/_static/image1.png)

Die Anwendung lässt auch hinzufügen, bearbeiten und Löschen von Filmen sowie ausführliche Informationen finden Sie Details zu einzelnen diejenigen. Alle Dateneingabe Szenarien umfassen die Überprüfung aus, um sicherzustellen, dass die in der Datenbank gespeicherten Daten richtig ist.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Erste Schritte

Starten Sie Visual Studio Express 2012 oder Visual Web Developer 2010 Express ausführen. Die meisten die Screenshots in dieser Serie mit Visual Studio Express 2012, aber Sie können dieses Lernprogramm mit Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 oder Visual Web Developer 2010 Express abzuschließen. Wählen Sie **neues Projekt** aus der **starten** Seite.

Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Studio ist eine Symbolleiste am oberen Rand mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. statt der Auswahl **neues Projekt** aus der **starten** Seite können Sie mithilfe des Menüs und auswählen **Datei** &gt; **neues Projekt**.)

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Erstellen einer ersten Anwendung

Sie können Anwendungen mit Visual Basic oder Visual c# als Programmiersprache erstellen. Visual c# auf der linken Seite und wählen Sie dann **ASP.NET MVC 4-Webanwendung**. Namen für das Projekt &quot;MvcMovie&quot; , und klicken Sie dann auf **OK**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Lassen Sie **Razor** als Standardansichtsmodul.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Klicken Sie auf **OK**. Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche! Dies ist eine einfache &quot;Hello World!&quot; -Projekt, und die eine gute zum Starten der Anwendung.

![](intro-to-aspnet-mvc-4/_static/image6.png)

Wählen Sie im Menü **Debuggen** die Option **Debugging starten**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Beachten Sie, dass die Tastenkombination zum Starten des Debuggings F5.

F5 bewirkt, dass Visual Studio starten Sie IIS Express, und führen Sie die Webanwendung. Visual Studio dann öffnet einen Browser und Startseite der Anwendung geöffnet. Beachten Sie, die besagt, die Adressleiste des Browsers dass `localhost` und nicht etwas wie `example.com`. Grund hierfür ist, `localhost` verweist immer auf Ihrem lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie soeben erstellt haben. Wenn Visual Studio ein Webprojekt ausgeführt wird, wird für den Webserver ein zufälliger Port verwendet. In der folgenden Abbildung ist die Nummer des Ports 41788. Wenn Sie die Anwendung ausführen, sehen Sie möglicherweise eine andere Portnummer an.

![](intro-to-aspnet-mvc-4/_static/image8.png)

Direkt einsatzbereiten bietet diese Standardvorlage Seiten "Home", "Wenden Sie sich an" und "zu. Außerdem bietet Unterstützung zum Registrieren, und melden Sie sich, und enthält links zu Facebook und Twitter. Der nächste Schritt besteht ändern, wie diese Anwendung funktioniert, und erfahren etwas über ASP.NET MVC. Schließen Sie Ihren Browser, und ändern Sie Code.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
