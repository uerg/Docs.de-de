---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie ASP.NET Web-API in einer Konsolenanwendung, die mithilfe von OWIN zum selfhosten der Web-API-Framework gehostet wird. Öffnen Sie die Web Interface for .NET (OWIN) d...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389559"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API 2
====================
durch [Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> In diesem Tutorial wird gezeigt, wie ASP.NET Web-API in einer Konsolenanwendung, die mithilfe von OWIN zum selfhosten der Web-API-Framework gehostet wird.
> 
> [Öffnen von Weboberfläche für .NET](http://owin.org) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen. OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für das Selbsthosting einer Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert auch mit Visual Studio 2012)
> - Web-API 2


> [!NOTE]
> Sie finden den vollständigen Quellcode für dieses Tutorial auf [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).


## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**. Von **installierte Vorlagen**, klicken Sie unter Visual c# **Windows** , und klicken Sie dann auf **Konsolenanwendung**. Nennen Sie das Projekt "OwinSelfhostSample", und klicken Sie auf **OK**.

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Fügen Sie die Web-API und OWIN-Pakete

Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Hiermit wird dem WebAPI-OWIN-Selfhost-Paket und alle erforderlichen Pakete der OWIN-installiert.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Konfigurieren von Web-API für selbst gehostete Dienste

Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen. Nennen Sie die Klasse `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Fügen Sie eine Web-API-Controller hinzu.

Als Nächstes fügen Sie eine Web-API-Controller-Klasse hinzu. Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen. Nennen Sie die Klasse `ValuesController`.

Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>Starten Sie die OWIN-Host, und stellen Sie eine Anforderung mithilfe von "HttpClient"

Ersetzen Sie alle die Codebausteine in der Datei "Program.cs" durch Folgendes:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>Ausführen der Anwendung

Um die Anwendung auszuführen, drücken Sie F5 in Visual Studio aus. Die Ausgabe sollte wie folgt aussehen:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Übersicht über das Katana-Projekt](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Hosten von ASP.NET Web-API in einer Azure-Workerrolle](host-aspnet-web-api-in-an-azure-worker-role.md)
