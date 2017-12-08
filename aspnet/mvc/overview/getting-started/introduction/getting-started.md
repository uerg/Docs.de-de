---
uid: mvc/overview/getting-started/introduction/getting-started
title: Erste Schritte mit ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier mit Visual Studio 2015 verfügbar. Das neue Lernprogramm verwendet ASP.NET Core MVC 6, die viele Improvem bietet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8f2cb9b3f14ad95acd9e4c7a0ed26228d3c480e0
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/23/2017
---
<a name="getting-started-with-aspnet-mvc-5"></a>Erste Schritte mit ASP.NET MVC 5
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[consider RP](../../../../includes/razor.md)]

 
 In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC 5 Webanwendung mit [Visual Studio 2017](https://www.visualstudio.com/). Endgültige Quelle Lernprogramm befindet sich auf [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)
 
 
 Dieses Lernprogramm wurde von geschrieben [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , und [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )
 
 Sie benötigen ein Azure-Konto um diese app in Azure bereitzustellen:
 
 - Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) -erhalten Sie Gutschriften können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
 - Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.


## <a name="getting-started"></a>Erste Schritte

Starten, indem Sie installieren und Ausführen von [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word verwenden, um Dokumente zu schreiben, verwenden Sie eine IDE zum Erstellen von Anwendungen. In Visual Studio wird eine Liste entlang des unteren mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. statt der Auswahl **neues Projekt** aus der **starten** Seite können Sie mithilfe des Menüs und auswählen **Datei** &gt; **neues Projekt**.)

   
![](getting-started/_static/image1.png)  
 

## <a name="creating-your-first-application"></a>Erstellen einer ersten Anwendung

Klicken Sie auf **neues Projekt**, wählen Sie dann Visual c# auf der linken Seite dann **Web** und wählen Sie dann **ASP.NET-Webanwendung ((.NET Framework)**. Namen für das Projekt "MvcMovie", und klicken Sie dann auf **OK**.

![](getting-started/_static/image2.png)

In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC** , und klicken Sie dann auf **OK**.

![](getting-started/_static/image3.png)

Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, das müssen Sie eine funktionierende Anwendung jetzt ohne jegliche! Dies ist eine einfache "Hello World!" der Projekt, und es werden eine gute zum Starten der Anwendung.

![](getting-started/_static/image4.png)

Klicken Sie auf F5, um mit dem Debuggen beginnen. F5 bewirkt, dass Visual Studio starten [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) , und führen Sie Ihre Web-app. Visual Studio dann öffnet einen Browser und Startseite der Anwendung geöffnet. Beachten Sie, die besagt, die Adressleiste des Browsers dass `localhost:port#` und nicht etwas wie `example.com`. Grund hierfür ist, `localhost` verweist immer auf Ihrem lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie soeben erstellt haben. Wenn Visual Studio ein Webprojekt ausgeführt wird, wird für den Webserver ein zufälliger Port verwendet. In der folgenden Abbildung ist die Portnummer 1234. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

![](getting-started/_static/image5.png)

Direkt einsatzbereiten bietet diese Standardvorlage Seiten "Home", "Wenden Sie sich an" und "zu. Der Abbildung oben zeigen nicht die **Home**, **zu** und **wenden Sie sich an** Links. Je nach Größe Ihres Browserfensters müssen Sie auf das Symbol "Navigation", um die folgenden Links finden Sie unter.

![](getting-started/_static/image6.png)  

Die Anwendung bietet auch Unterstützung zum Registrieren, und melden Sie sich. Der nächste Schritt besteht ändern, wie diese Anwendung funktioniert, und erfahren etwas über ASP.NET MVC. Schließen Sie die ASP.NET MVC-Anwendung, und wir ändern Sie Teil des Codes zu.

Eine Liste der aktuellen Lernprogramme, finden Sie unter [MVC empfohlen Artikel](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App auf Azure ausgeführt wird

Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.

>[!div class="step-by-step"]
[Nächste](adding-a-controller.md)
