---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie ASP.NET Web-API in einer Konsolenanwendung, die mithilfe von OWIN zum selfhosten der Web-API-Framework gehostet wird. Öffnen Sie die Web Interface for .NET (OWIN) d...
ms.author: aspnetcontent
ms.date: 07/09/2013
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9fba2774e3873d32115a14fa0c84b99466eda04f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830910"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a><span data-ttu-id="08104-104">Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API 2</span><span class="sxs-lookup"><span data-stu-id="08104-104">Use OWIN to Self-Host ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="08104-105">durch [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span><span class="sxs-lookup"><span data-stu-id="08104-105">by [Kanchan Mehrotra](https://twitter.com/kanchanmeh)</span></span>

> <span data-ttu-id="08104-106">In diesem Tutorial wird gezeigt, wie ASP.NET Web-API in einer Konsolenanwendung, die mithilfe von OWIN zum selfhosten der Web-API-Framework gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="08104-106">This tutorial shows how to host ASP.NET Web API in a console application, using OWIN to self-host the Web API framework.</span></span>
> 
> <span data-ttu-id="08104-107">[Öffnen von Weboberfläche für .NET](http://owin.org) (OWIN) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="08104-107">[Open Web Interface for .NET](http://owin.org) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="08104-108">OWIN entkoppelt die Webanwendung aus dem Server, wodurch OWIN ideal für das Selbsthosting einer Webanwendung in Ihrem eigenen Prozess, außerhalb von IIS.</span><span class="sxs-lookup"><span data-stu-id="08104-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="08104-109">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="08104-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="08104-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert auch mit Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="08104-110">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (also works with Visual Studio 2012)</span></span>
> - <span data-ttu-id="08104-111">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="08104-111">Web API 2</span></span>


> [!NOTE]
> <span data-ttu-id="08104-112">Sie finden den vollständigen Quellcode für dieses Tutorial auf [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span><span class="sxs-lookup"><span data-stu-id="08104-112">You can find the complete source code for this tutorial at [aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt).</span></span>


## <a name="create-a-console-application"></a><span data-ttu-id="08104-113">Erstellen einer Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="08104-113">Create a Console Application</span></span>

<span data-ttu-id="08104-114">Auf der **Datei** Menü klicken Sie auf **neu**, klicken Sie dann auf **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="08104-114">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="08104-115">Von **installierte Vorlagen**, klicken Sie unter Visual c# **Windows** , und klicken Sie dann auf **Konsolenanwendung**.</span><span class="sxs-lookup"><span data-stu-id="08104-115">From **Installed Templates**, under Visual C#, click **Windows** and then click **Console Application**.</span></span> <span data-ttu-id="08104-116">Nennen Sie das Projekt "OwinSelfhostSample", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="08104-116">Name the project "OwinSelfhostSample" and click **OK**.</span></span>

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a><span data-ttu-id="08104-117">Fügen Sie die Web-API und OWIN-Pakete</span><span class="sxs-lookup"><span data-stu-id="08104-117">Add the Web API and OWIN Packages</span></span>

<span data-ttu-id="08104-118">Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager**, klicken Sie dann auf **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="08104-118">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span> <span data-ttu-id="08104-119">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="08104-119">In the Package Manager Console window, enter the following command:</span></span>

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

<span data-ttu-id="08104-120">Hiermit wird dem WebAPI-OWIN-Selfhost-Paket und alle erforderlichen Pakete der OWIN-installiert.</span><span class="sxs-lookup"><span data-stu-id="08104-120">This will install the WebAPI OWIN selfhost package and all the required OWIN packages.</span></span>

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a><span data-ttu-id="08104-121">Konfigurieren von Web-API für selbst gehostete Dienste</span><span class="sxs-lookup"><span data-stu-id="08104-121">Configure Web API for Self-Host</span></span>

<span data-ttu-id="08104-122">Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="08104-122">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="08104-123">Nennen Sie die Klasse `Startup`.</span><span class="sxs-lookup"><span data-stu-id="08104-123">Name the class `Startup`.</span></span>

![](use-owin-to-self-host-web-api/_static/image5.png)

<span data-ttu-id="08104-124">Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="08104-124">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a><span data-ttu-id="08104-125">Fügen Sie eine Web-API-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="08104-125">Add a Web API Controller</span></span>

<span data-ttu-id="08104-126">Als Nächstes fügen Sie eine Web-API-Controller-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="08104-126">Next, add a Web API controller class.</span></span> <span data-ttu-id="08104-127">Klicken Sie im Projektmappen-Explorer klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** / **Klasse** eine neue Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="08104-127">In Solution Explorer, right click the project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="08104-128">Nennen Sie die Klasse `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="08104-128">Name the class `ValuesController`.</span></span>

<span data-ttu-id="08104-129">Ersetzen Sie alle die Codebausteine in dieser Datei durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="08104-129">Replace all of the boilerplate code in this file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a><span data-ttu-id="08104-130">Starten Sie die OWIN-Host, und stellen Sie eine Anforderung mithilfe von "HttpClient"</span><span class="sxs-lookup"><span data-stu-id="08104-130">Start the OWIN Host and Make a Request Using HttpClient</span></span>

<span data-ttu-id="08104-131">Ersetzen Sie alle die Codebausteine in der Datei "Program.cs" durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="08104-131">Replace all of the boilerplate code in the Program.cs file with the following:</span></span>

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a><span data-ttu-id="08104-132">Ausführen der Anwendung</span><span class="sxs-lookup"><span data-stu-id="08104-132">Running the Application</span></span>

<span data-ttu-id="08104-133">Um die Anwendung auszuführen, drücken Sie F5 in Visual Studio aus.</span><span class="sxs-lookup"><span data-stu-id="08104-133">To run the application, press F5 in Visual Studio.</span></span> <span data-ttu-id="08104-134">Die Ausgabe sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="08104-134">The output should look like the following:</span></span>

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a><span data-ttu-id="08104-135">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="08104-135">Additional Resources</span></span>

[<span data-ttu-id="08104-136">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="08104-136">An Overview of Project Katana</span></span>](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[<span data-ttu-id="08104-137">Hosten von ASP.NET Web-API in einer Azure-Workerrolle</span><span class="sxs-lookup"><span data-stu-id="08104-137">Host ASP.NET Web API in an Azure Worker Role</span></span>](host-aspnet-web-api-in-an-azure-worker-role.md)
