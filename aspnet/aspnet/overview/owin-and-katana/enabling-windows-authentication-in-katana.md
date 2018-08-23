---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Aktivieren der Windows-Authentifizierung in Katana | Microsoft-Dokumentation
author: MikeWasson
description: 'In diesem Artikel wird das Aktivieren der Windows-Authentifizierung in Katana veranschaulicht. Es werden zwei Szenarien behandelt: von IIS für das Katana-Host, und "HttpListener" verwendet, um Kat selbst hosten...'
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 0aa578020a1f02fa68c74e758014c642219b4265
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831923"
---
<a name="enabling-windows-authentication-in-katana"></a><span data-ttu-id="c3b95-104">Aktivieren der Windows-Authentifizierung in Katana</span><span class="sxs-lookup"><span data-stu-id="c3b95-104">Enabling Windows Authentication in Katana</span></span>
====================
<span data-ttu-id="c3b95-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c3b95-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c3b95-106">In diesem Artikel wird das Aktivieren der Windows-Authentifizierung in Katana veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="c3b95-106">This article shows how to enable Windows Authentication in Katana.</span></span> <span data-ttu-id="c3b95-107">Es werden zwei Szenarien behandelt: von IIS für das Katana-Host, und "HttpListener" verwendet, um Katana selbst in einem benutzerdefinierten Prozess hosten.</span><span class="sxs-lookup"><span data-stu-id="c3b95-107">It covers two scenarios: Using IIS to host Katana, and using HttpListener to self-host Katana in a custom process.</span></span> <span data-ttu-id="c3b95-108">Nochmals vielen Dank an David Matson, Barry Dorrans und Chris Ross für die Durchsicht dieses Artikels.</span><span class="sxs-lookup"><span data-stu-id="c3b95-108">Thanks to Barry Dorrans, David Matson, and Chris Ross for reviewing this article.</span></span>


<span data-ttu-id="c3b95-109">Katana ist Microsofts Umsetzung dieser [OWIN](http://owin.org/), Open Web Interface for .NET.</span><span class="sxs-lookup"><span data-stu-id="c3b95-109">Katana is Microsoft's implementation of [OWIN](http://owin.org/), the Open Web Interface for .NET.</span></span> <span data-ttu-id="c3b95-110">Sie erhalten eine Einführung in die OWIN und Katana [hier](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="c3b95-110">You can read an introduction to OWIN and Katana [here](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="c3b95-111">Die OWIN-Architektur besteht aus mehreren Ebenen:</span><span class="sxs-lookup"><span data-stu-id="c3b95-111">The OWIN architecture has several layers:</span></span>

- <span data-ttu-id="c3b95-112">Host: Verwaltet den Prozess, in dem die OWIN-Pipeline ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c3b95-112">Host: Manages the process in which the OWIN pipeline runs.</span></span>
- <span data-ttu-id="c3b95-113">Server: Ein Netzwerksocket geöffnet und Lauscht auf Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="c3b95-113">Server: Opens a network socket and listens for requests.</span></span>
- <span data-ttu-id="c3b95-114">Middleware: Verarbeitet HTTP-Anforderung und Antwort.</span><span class="sxs-lookup"><span data-stu-id="c3b95-114">Middleware: Processes the HTTP request and response.</span></span>

<span data-ttu-id="c3b95-115">Katana bietet derzeit zwei Server, die beide über integrierte Windows-Authentifizierung unterstützt auf:</span><span class="sxs-lookup"><span data-stu-id="c3b95-115">Katana currently provides two servers, both of which support Windows Integrated Authentication:</span></span>

- <span data-ttu-id="c3b95-116">**Microsoft.Owin.Host.SystemWeb**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-116">**Microsoft.Owin.Host.SystemWeb**.</span></span> <span data-ttu-id="c3b95-117">IIS wird mit der ASP.NET-Pipeline verwendet.</span><span class="sxs-lookup"><span data-stu-id="c3b95-117">Uses IIS with the ASP.NET pipeline.</span></span>
- <span data-ttu-id="c3b95-118">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-118">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="c3b95-119">Verwendet [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3b95-119">Uses [System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx).</span></span> <span data-ttu-id="c3b95-120">Dieser Server ist zurzeit die Standardoption, wenn Sie Selbsthosting Katana.</span><span class="sxs-lookup"><span data-stu-id="c3b95-120">This server is currently the default option when self-hosting Katana.</span></span>

> [!NOTE]
> <span data-ttu-id="c3b95-121">Katana bietet derzeit OWIN-Middleware für die Windows-Authentifizierung keine, da diese Funktion bereits in der Server verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="c3b95-121">Katana does not currently provide OWIN middleware for Windows Authentication, because this functionality is already available in the servers.</span></span>


## <a name="windows-authentication-in-iis"></a><span data-ttu-id="c3b95-122">Windows-Authentifizierung in IIS</span><span class="sxs-lookup"><span data-stu-id="c3b95-122">Windows Authentication in IIS</span></span>

<span data-ttu-id="c3b95-123">Verwenden "Microsoft.owin.Host.systemweb", können Sie einfach Windows-Authentifizierung in IIS aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c3b95-123">Using Microsoft.Owin.Host.SystemWeb, you can simply enable Windows Authentication in IIS.</span></span>

<span data-ttu-id="c3b95-124">Wir erstellen zunächst eine neue ASP.NET-Anwendung mit mithilfe der Projektvorlage "Leere ASP.NET-Webanwendung".</span><span class="sxs-lookup"><span data-stu-id="c3b95-124">Let's start by creating a new ASP.NET application, using the "ASP.NET Empty Web Application" project template.</span></span>

![](enabling-windows-authentication-in-katana/_static/image1.png)

<span data-ttu-id="c3b95-125">Als Nächstes fügen Sie die NuGet-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3b95-125">Next, add NuGet packages.</span></span> <span data-ttu-id="c3b95-126">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-126">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3b95-127">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c3b95-127">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

<span data-ttu-id="c3b95-128">Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c3b95-128">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

<span data-ttu-id="c3b95-129">Das ist alles, was, die Sie benötigen zum Erstellen einer "Hello World"-Anwendung für OWIN, die unter IIS.</span><span class="sxs-lookup"><span data-stu-id="c3b95-129">That's all you need to create a "Hello world" application for OWIN, running on IIS.</span></span> <span data-ttu-id="c3b95-130">Drücken Sie F5, um die Anwendung zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="c3b95-130">Press F5 to debug the application.</span></span> <span data-ttu-id="c3b95-131">"Hello World!" sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="c3b95-131">You should see "Hello World!"</span></span> <span data-ttu-id="c3b95-132">im Browserfenster.</span><span class="sxs-lookup"><span data-stu-id="c3b95-132">in the browser window.</span></span>

![](enabling-windows-authentication-in-katana/_static/image2.png)

<span data-ttu-id="c3b95-133">Aktivieren Sie als Nächstes Windows-Authentifizierung in IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c3b95-133">Next, we'll enable Windows Authentication in IIS Express.</span></span> <span data-ttu-id="c3b95-134">Von der **Ansicht** , wählen Sie im Menü **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-134">From the **View** menu, select **Properties**.</span></span> <span data-ttu-id="c3b95-135">Klicken Sie auf den Projektnamen im Projektmappen-Explorer auf die Projekteigenschaften anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c3b95-135">Click on the project name in Solution Explorer to view the project properties.</span></span>

<span data-ttu-id="c3b95-136">In der **Eigenschaften** legen **anonyme Authentifizierung** zu **deaktiviert** und **Windows-Authentifizierung** zu  **Aktiviert**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-136">In the **Properties** window, set **Anonymous Authentication** to **Disabled** and set **Windows Authentication** to **Enabled**.</span></span>

![](enabling-windows-authentication-in-katana/_static/image3.png)

<span data-ttu-id="c3b95-137">Wenn Sie die Anwendung aus Visual Studio ausführen, müssen die IIS Express, dass Windows-Anmeldeinformationen des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="c3b95-137">When you run the application from Visual Studio, IIS Express will require the user's Windows credentials.</span></span> <span data-ttu-id="c3b95-138">Dies sehen Sie mithilfe von [Fiddler](http://fiddler2.com/home) oder einem anderen HTTP-debugging-Tools.</span><span class="sxs-lookup"><span data-stu-id="c3b95-138">You can see this by using [Fiddler](http://fiddler2.com/home) or another HTTP debugging tool.</span></span> <span data-ttu-id="c3b95-139">Hier ist ein Beispiel für HTTP-Antwort:</span><span class="sxs-lookup"><span data-stu-id="c3b95-139">Here is an example HTTP response:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

<span data-ttu-id="c3b95-140">Der WWW-Authenticate-Header in dieser Antwort anzugeben, dass der Server unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Protokoll, das entweder Kerberos oder NTLM verwendet.</span><span class="sxs-lookup"><span data-stu-id="c3b95-140">The WWW-Authenticate headers in this response indicate that the server supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) protocol, which uses either Kerberos or NTLM.</span></span>

<span data-ttu-id="c3b95-141">Später, wenn Sie die Anwendung auf einem Server bereitstellen, führen Sie [folgendermaßen](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) um Windows-Authentifizierung in IIS auf diesem Server zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="c3b95-141">Later, when you deploy the application to a server, follow [these steps](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) to enable Windows Authentication in IIS on that server.</span></span>

## <a name="windows-authentication-in-httplistener"></a><span data-ttu-id="c3b95-142">Windows-Authentifizierung in HttpListener</span><span class="sxs-lookup"><span data-stu-id="c3b95-142">Windows Authentication in HttpListener</span></span>

<span data-ttu-id="c3b95-143">Wenn Sie zum selfhosten von Katana Microsoft.Owin.Host.HttpListener verwenden, können Sie die Windows-Authentifizierung aktivieren, direkt auf die **HttpListener** Instanz.</span><span class="sxs-lookup"><span data-stu-id="c3b95-143">If you are using Microsoft.Owin.Host.HttpListener to self-host Katana, you can enable Windows Authentication directly on the **HttpListener** instance.</span></span>

<span data-ttu-id="c3b95-144">Erstellen Sie zunächst eine neue Konsolenanwendung.</span><span class="sxs-lookup"><span data-stu-id="c3b95-144">First, create a new console application.</span></span> <span data-ttu-id="c3b95-145">Als Nächstes fügen Sie die NuGet-Pakete hinzu.</span><span class="sxs-lookup"><span data-stu-id="c3b95-145">Next, add NuGet packages.</span></span> <span data-ttu-id="c3b95-146">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="c3b95-146">From the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3b95-147">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c3b95-147">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

<span data-ttu-id="c3b95-148">Nun fügen Sie eine Klasse, die mit dem Namen `Startup` durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="c3b95-148">Now add a class named `Startup` with the following code:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

<span data-ttu-id="c3b95-149">Diese Klasse implementiert die gleichen "Hello World"-Beispiel vorher, aber sie setzt auch die Windows-Authentifizierung, als Authentifizierungsschema.</span><span class="sxs-lookup"><span data-stu-id="c3b95-149">This class implements the same "Hello world" example from before, but it also sets Windows Authentication as the authentication scheme.</span></span>

<span data-ttu-id="c3b95-150">In der `Main` funktioniert, starten Sie die OWIN-Pipeline:</span><span class="sxs-lookup"><span data-stu-id="c3b95-150">Inside the `Main` function, start the OWIN pipeline:</span></span>

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

<span data-ttu-id="c3b95-151">Sie können eine Anforderung senden, in Fiddler ausführen, um sicherzustellen, dass die Anwendung Windows-Authentifizierung verwendet:</span><span class="sxs-lookup"><span data-stu-id="c3b95-151">You can send a request in Fiddler to confirm that the application is using Windows Authentication:</span></span>

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a><span data-ttu-id="c3b95-152">Verwandte Themen</span><span class="sxs-lookup"><span data-stu-id="c3b95-152">Related Topics</span></span>

[<span data-ttu-id="c3b95-153">Übersicht über das Katana-Projekt</span><span class="sxs-lookup"><span data-stu-id="c3b95-153">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)

[<span data-ttu-id="c3b95-154">System.Net.HttpListener</span><span class="sxs-lookup"><span data-stu-id="c3b95-154">System.Net.HttpListener</span></span>](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[<span data-ttu-id="c3b95-155">Grundlegendes zur OWIN-Formularauthentifizierung in MVC 5</span><span class="sxs-lookup"><span data-stu-id="c3b95-155">Understanding OWIN Forms Authentication in MVC 5</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
