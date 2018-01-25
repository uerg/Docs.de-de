---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Erstellen Sie eine Client-App OData v4 (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 51a3c7b9c5b6525d6d82b9a45910f58b71268b7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="9f8e6-102">Erstellen Sie eine Client-App OData v4 (c#)</span><span class="sxs-lookup"><span data-stu-id="9f8e6-102">Create an OData v4 Client App (C#)</span></span>
====================
<span data-ttu-id="9f8e6-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9f8e6-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9f8e6-104">Im vorherigen Lernprogramm erstellt Sie einen grundlegenden OData-Dienst, der CRUD-Vorgänge unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="9f8e6-105">Jetzt erstellen wir einen Client für den Dienst.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="9f8e6-106">Starten Sie eine neue Instanz von Visual Studio und erstellen Sie ein neues Konsolenanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="9f8e6-107">In der **neues Projekt** wählen Sie im Dialogfeld **installiert** &gt; **Vorlagen** &gt; **Visual C#-** &gt; **Windows Desktop**, und wählen Sie die **Konsolenanwendung** Vorlage.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="9f8e6-108">Nennen Sie das Projekt &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="9f8e6-109">Sie können auch die Konsolen-app auf der gleichen Visual Studio-Projektmappe hinzufügen, die den OData-Dienst enthält.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>


## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="9f8e6-110">Installieren Sie den OData-Client-Code-Generator</span><span class="sxs-lookup"><span data-stu-id="9f8e6-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="9f8e6-111">Aus der **Tools** klicken Sie im Menü **Erweiterungen und Updates**.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="9f8e6-112">Wählen Sie **Online** &gt; **Visual Studio Gallery**.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="9f8e6-113">Suchen Sie in das Suchfeld &quot;OData-Client-Codegenerator&quot;.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="9f8e6-114">Klicken Sie auf **herunterladen** VSIX-Pakete installieren.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="9f8e6-115">Sie möglicherweise aufgefordert, die Visual Studio neu starten.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="9f8e6-116">Die OData-Dienst lokal ausführen</span><span class="sxs-lookup"><span data-stu-id="9f8e6-116">Run the OData Service Locally</span></span>

<span data-ttu-id="9f8e6-117">Führen Sie das ProductService-Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="9f8e6-118">Standardmäßig wird in Visual Studio das Stammverzeichnis der Anwendung in einen Browser gestartet.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="9f8e6-119">Beachten Sie den URI an. Dies ist im nächsten Schritt benötigen.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="9f8e6-120">Unterbrechen Sie die Ausführung der Anwendung nicht.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="9f8e6-121">Wenn Sie beide Projekte in der gleichen Projektmappe festlegen, stellen Sie sicher, um das ProductService Projekt ohne Debuggen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="9f8e6-122">Im nächsten Schritt müssen Sie zu der Dienst ausgeführt wird, während Sie das Konsolenanwendungsprojekt ändern.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>


## <a name="generate-the-service-proxy"></a><span data-ttu-id="9f8e6-123">Generieren des Webdienstproxys</span><span class="sxs-lookup"><span data-stu-id="9f8e6-123">Generate the Service Proxy</span></span>

<span data-ttu-id="9f8e6-124">Der Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="9f8e6-125">Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="9f8e6-126">Erstellen Sie die Proxyklasse durch Ausführen einer [T4-Vorlage](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="9f8e6-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="9f8e6-127">Mit der rechten Maustaste in des Projekts.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-127">Right-click the project.</span></span> <span data-ttu-id="9f8e6-128">Wählen Sie **hinzufügen** &gt; **neues Element**.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="9f8e6-129">In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C#-Elemente** &gt; **Code** &gt; **OData-Client**.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="9f8e6-130">Nennen Sie die Vorlage &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="9f8e6-131">Klicken Sie auf **hinzufügen** und über die sicherheitswarnung auf.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="9f8e6-132">An diesem Punkt erhalten Sie einen Fehler, die Sie ignorieren können.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="9f8e6-133">Visual Studio automatisch die Vorlage ausgeführt, aber die Vorlage benötigt einige Konfigurationseinstellungen erste.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="9f8e6-134">Öffnen Sie die Datei ProductClient.odata.config. In der `Parameter` Element einfügen, die im URI aus dem Projekt ProductService (vorigen Schritt).</span><span class="sxs-lookup"><span data-stu-id="9f8e6-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="9f8e6-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="9f8e6-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="9f8e6-136">Führen Sie die Vorlage erneut aus.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-136">Run the template again.</span></span> <span data-ttu-id="9f8e6-137">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf die ProductClient.tt-Datei, und wählen **benutzerdefiniertes Tool ausführen**.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="9f8e6-138">Die Projektvorlage erstellt eine Codedatei mit dem Namen ProductClient.cs, die den Proxy definiert.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="9f8e6-139">Wie Sie Ihre app entwickeln, wenn Sie den OData-Endpunkt ändern, führen Sie die Vorlage erneut aus, um den Proxy zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="9f8e6-140">Verwenden Sie den Dienstproxy zum Aufrufen des OData-Diensts</span><span class="sxs-lookup"><span data-stu-id="9f8e6-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="9f8e6-141">Öffnen Sie die Datei "Program.cs", und Ersetzen Sie den Standardcode durch Folgendes.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="9f8e6-142">Ersetzen Sie den Wert der *ServiceUri* mit dem Dienst-URI aus zuvor.</span><span class="sxs-lookup"><span data-stu-id="9f8e6-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="9f8e6-143">Beim Ausführen der app sollte die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="9f8e6-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
