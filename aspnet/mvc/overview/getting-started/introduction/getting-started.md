---
uid: mvc/overview/getting-started/introduction/getting-started
title: Erste Schritte mit ASP.NET MVC 5 | Microsoft-Dokumentation
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: 2cc9364b815cae0207fc59784303c6a0906f1b94
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578444"
---
<a name="getting-started-with-aspnet-mvc-5"></a>Erste Schritte mit ASP.NET MVC 5
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Dieses Lernprogramm zeigt Ihnen die Grundlagen zum Erstellen einer ASP.NET MVC 5 Web-app mit [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Der endgültige Quellcode für das Tutorial befindet sich auf [GitHub](https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

In diesem Tutorial wurde von geschrieben [Scott Guthrie](https://weblogs.asp.net/scottgu/) (twitter-[ @scottgu ](https://twitter.com/scottgu) ), [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](https://twitter.com/shanselman) ) , und [Rick Anderson](https://twitter.com/RickAndMSFT) ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )

Sie benötigen ein Azure-Konto auf diese app in Azure bereit:

- Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

## <a name="get-started"></a>Erste Schritte

Zunächst [Installation von Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Öffnen Sie dann Visual Studio.

Visual Studio ist eine IDE oder der integrierten Entwicklungsumgebung. Wie Sie Microsoft Word zum Schreiben von Dokumenten verwenden, verwenden Sie eine IDE, um Anwendungen zu erstellen. In Visual Studio ist eine Liste entlang des unteren mit verschiedenen verfügbaren Optionen für Sie vorhanden. Es gibt auch ein Menü, das eine andere Möglichkeit, Aufgaben in der IDE bereitstellt. Beispielsweise anstelle von **neues Projekt** auf die **Startseite**, können Sie verwenden die Menüleiste und auswählen **Datei** > **neues Projekt**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Erstellen der ersten App

Auf der **Startseite**Option **neues Projekt**. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C#-** Kategorie auf der linken Seite, klicken Sie dann **Web**, und wählen Sie dann die **ASP.NET-Webanwendung ((.NET Framework)**  Projektvorlage. Benennen Sie das Projekt "MvcMovie", und wählen Sie dann **OK**.

![](getting-started/_static/image2.png)

In der **neue ASP.NET-Webanwendung** Dialogfeld Wählen Sie **MVC** und wählen Sie dann **OK**.

![](getting-started/_static/image3.png)

Visual Studio verwendet eine Standardvorlage für das ASP.NET MVC-Projekt, das Sie gerade erstellt haben, müssen Sie eine funktionierende Anwendung jetzt ohne Benutzereingriff. Dies ist eine einfache "Hello World!" Projekt, und es ist ein guter Ausgangspunkt, um Ihre Anwendung zu starten.

![](getting-started/_static/image4.png)

Drücken Sie die Taste **F5** , um mit dem Debuggen zu beginnen. Wenn Sie drücken **F5**, Visual Studio startet [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) und führt Ihre Web-app. Visual Studio klicken Sie dann einen Browser und Startseite der Anwendung wird geöffnet. Beachten Sie, dass mit dem Text die Adressleiste des Browsers `localhost:port#` und nicht etwas wie `example.com`. Der Grund dafür ist `localhost` verweist immer auf Ihre eigenen lokalen Computer, die in diesem Fall die Anwendung ausgeführt wird Sie gerade erstellt haben. Wenn Visual Studio ein Webprojekt ausgeführt wird, wird ein zufälliger Port für den Webserver verwendet. In der folgenden Abbildung ist die Nummer des Ports 1234. Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.

![](getting-started/_static/image5.png)

Anwendungstelemetriedaten dieser Standardvorlage bietet Ihnen `Home`, `Contact`, und `About` Seiten. In der Abbildung oben zeigt nicht an die **Startseite**, **zu**, und **wenden Sie sich an** Links. Je nach Größe Ihres Browserfensters müssen Sie auf das Symbol "berichtsnavigation", um die folgenden Links finden Sie unter.

![](getting-started/_static/image6.png)

Die Anwendung liefert auch Support, um die Registrierung und Anmeldung. Der nächste Schritt besteht, zu ändern, wie diese Anwendung funktioniert, und erfahren etwas zu ASP.NET MVC. Schließen Sie die ASP.NET MVC-Anwendung, und Ändern von Code.

Eine Liste der aktuellen Lernprogramme, finden Sie unter [MVC Artikel empfohlen](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser app in Azure ausführen

Möchten Sie die fertige Website als live-Web-app ausgeführt wird, finden Sie unter? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach die folgende Schaltfläche.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/aspnet/Docs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie noch nicht über ein Konto verfügen, verwenden Sie eine der folgenden Optionen erstellen:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- [Visual Studio-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) -Ihr Visual Studio-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

> [!div class="step-by-step"]
> [Nächste](adding-a-controller.md)
