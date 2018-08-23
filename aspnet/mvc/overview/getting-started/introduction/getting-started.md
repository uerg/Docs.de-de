---
uid: mvc/overview/getting-started/introduction/getting-started
title: Erste Schritte mit ASP.NET MVC 5 | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier mithilfe von Visual Studio 2015 verfügbar. Das neue Tutorial verwendet die ASP.NET Core MVC 6, die viele Improvem bereitstellt...'
ms.author: riande
ms.date: 05/28/2015
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 8417be945e16ed99e655f628134c9190429f1d67
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838025"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Erste Schritte mit ASP.NET MVC 5
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

 Dieses Tutorial vermittelt Ihnen die Grundlagen zum Erstellen einer ASP.NET MVC 5 Web-app mit [Visual Studio 2017](https://www.visualstudio.com/). Letzte Quelle Tutorial befindet sich auf [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie)


 In diesem Tutorial wurde von geschrieben [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter-[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , und [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

 Sie benötigen ein Azure-Konto auf diese app in Azure bereit:

 - Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
 - Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.


## <a name="getting-started"></a>Erste Schritte

Zunächst installieren und Ausführen von [Visual Studio 2017](https://www.visualstudio.com/).

Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. In Visual Studio ein, es gibt eine Liste entlang des unteren mit verschiedenen verfügbaren Optionen für Sie. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. (Z. B. anstelle **neues Projekt** aus der **starten** Seite können Sie das Menü und auswählen **Datei** &gt; **neues Projekt**.)


![](getting-started/_static/image1.png)  


## <a name="creating-your-first-application"></a>Erstellen Ihrer ersten Anwendung

Klicken Sie auf **neues Projekt**, wählen Sie dann Visual C# -Code auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung ((.NET Framework)**. Benennen Sie das Projekt "MvcMovie", und klicken Sie dann auf **OK**.

![](getting-started/_static/image2.png)

In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC** , und klicken Sie dann auf **OK**.

![](getting-started/_static/image3.png)

Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache "Hello World!" Projekt, und es ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

![](getting-started/_static/image4.png)

Klicken Sie auf F5, um mit dem Debuggen beginnen. F5 wird in Visual Studio zu [IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) , und führen Sie Ihre Web-app. Visual Studio klicken Sie dann einen Browser und Startseite der Anwendung wird geöffnet. Beachten Sie, dass mit dem Text die Adressleiste des Browsers `localhost:port#` und nicht etwas wie `example.com`. Der Grund dafür ist `localhost` verweist immer auf Ihre eigenen lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie gerade erstellt haben. Wenn Visual Studio ein Webprojekt ausgeführt wird, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Nummer des Ports 1234. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

![](getting-started/_static/image5.png)

Anwendungstelemetriedaten bietet dieser Standardvorlage Seiten "Home", "Wenden Sie sich an" und "über. In der Abbildung oben zeigt nicht an die **Startseite**, **zu** und **wenden Sie sich an** Links. Je nach Größe Ihres Browserfensters müssen Sie auf das Symbol "berichtsnavigation", um die folgenden Links finden Sie unter.

![](getting-started/_static/image6.png)  

Die Anwendung liefert auch Support, um die Registrierung und Anmeldung. Der nächste Schritt besteht, zu ändern, wie diese Anwendung funktioniert, und erfahren etwas zu ASP.NET MVC. Schließen Sie die ASP.NET MVC-Anwendung, und Ändern von Code.

Eine Liste der aktuellen Lernprogramme, finden Sie unter [MVC Artikel empfohlen](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App in Azure ausführen

Möchten Sie die fertige Website als live-Web-app ausgeführt wird, finden Sie unter? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach die folgende Schaltfläche.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
