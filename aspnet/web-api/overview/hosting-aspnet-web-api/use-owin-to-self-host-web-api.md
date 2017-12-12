---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Mit der ASP.NET Web API 2 Selbsthosting OWIN | Microsoft Docs
author: rick-anderson
description: "Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Konsolenanwendung, die mithilfe der Web-API-Framework selbst gehostet wird OWIN. Öffnen Sie die Weboberfläche für .NET (OWIN) d..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Mit der ASP.NET Web API 2 Selbsthosting OWIN
====================
durch [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> Dieses Lernprogramm zeigt, wie zum Hosten von ASP.NET Web-API in einer Konsolenanwendung, die mithilfe der Web-API-Framework selbst gehostet wird OWIN.
> 
> [Öffnen Sie die Weboberfläche für .NET](http://owin.org) (OWIN) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für Selbsthosting einer Webanwendung in einen eigenen Prozess, außerhalb von IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert auch mit Visual Studio 2012)
> - Web-API 2


> [!NOTE]
> Sie finden den vollständigen Quellcode für dieses Lernprogramm zur [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual C#- **Windows** , und klicken Sie dann auf **Konsolenanwendung**. Nennen Sie das Projekt "OwinSelfhostSample", und klicken Sie auf **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Hinzufügen der Web-API und die OWIN-Pakete

Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Dadurch wird das WebAPI OWIN Selfhost-Paket und die erforderlichen OWIN-Pakete installiert.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurieren von Web-API für Selbsthosting

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu. Nennen Sie die Klasse `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu

Als Nächstes fügen Sie eine Web-API-Controllerklasse hinzu. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** So fügen Sie eine neue Klasse hinzu. Nennen Sie die Klasse `ValuesController`.

Ersetzen Sie alle in dieser Datei den Standardcode durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Starten des OWIN-Hosts, und führen Sie eine Anforderung mithilfe von HttpClient

Ersetzen Sie alle in der Datei "Program.cs" den Standardcode durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Um die Anwendung auszuführen, drücken Sie F5 in Visual Studio. Die Ausgabe sollte wie folgt aussehen:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Eine Übersicht über Project Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hosten von ASP.NET Web-API in einer Azure-Workerrolle](host-aspnet-web-api-in-an-azure-worker-role.md)
