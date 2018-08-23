---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Selbst gehostete ASP.NET Web-API 1 (c#) | Microsoft-Dokumentation
author: MikeWasson
description: ASP.NET Web-API ist IIS nicht erforderlich. Sie können eine Web-API selbst in Ihrem eigenen Hostprozess hosten. In diesem Tutorial wird gezeigt, wie eine Web-API in einer Anwendung auf dem Konsole gehostet wird...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: cac0d5aeaf49f45051d062935e0e9207ce59c7eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832824"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="46918-105">Selbst gehostete ASP.NET Web-API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="46918-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="46918-106">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="46918-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="46918-107">ASP.NET Web-API ist IIS nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="46918-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="46918-108">Sie können eine Web-API selbst in Ihrem eigenen Hostprozess hosten.</span><span class="sxs-lookup"><span data-stu-id="46918-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="46918-109">In diesem Tutorial wird gezeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="46918-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="46918-110">**Neue Anwendungen sollten OWIN zum selfhosten der Web-API verwenden.**</span><span class="sxs-lookup"><span data-stu-id="46918-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="46918-111">Finden Sie unter [verwenden Sie OWIN zum Selfhosten von ASP.NET-Web-API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="46918-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="46918-112">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="46918-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="46918-113">Web-API 1</span><span class="sxs-lookup"><span data-stu-id="46918-113">Web API 1</span></span>
> - <span data-ttu-id="46918-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="46918-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="46918-115">Erstellen Sie das Projekt der Konsolenanwendung</span><span class="sxs-lookup"><span data-stu-id="46918-115">Create the Console Application Project</span></span>

<span data-ttu-id="46918-116">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="46918-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="46918-117">Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="46918-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="46918-118">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="46918-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="46918-119">Klicken Sie unter **Visual C#-** Option **Windows**.</span><span class="sxs-lookup"><span data-stu-id="46918-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="46918-120">Wählen Sie in der Liste der Projektvorlagen das Projekt **Konsolenanwendung**.</span><span class="sxs-lookup"><span data-stu-id="46918-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="46918-121">Nennen Sie das Projekt &quot;SelfHost&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="46918-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="46918-122">Legen Sie das Zielframework (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="46918-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="46918-123">Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Zielframework auf .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="46918-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="46918-124">(Standardmäßig wird die Projektvorlage abzielt der [.Net Framework-Clientprofil](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="46918-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="46918-125">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="46918-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="46918-126">In der **Zielframework** Dropdownliste, ändern Sie das Zielframework auf .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="46918-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="46918-127">Wenn Sie dazu aufgefordert werden, um die Änderung zu übernehmen, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="46918-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="46918-128">Installieren von NuGet-Paket-Manager</span><span class="sxs-lookup"><span data-stu-id="46918-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="46918-129">Die NuGet-Paket-Manager ist die einfachste Möglichkeit, um die Web-API-Assemblys zu einem ASP.NET-Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="46918-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="46918-130">Um festzustellen, ob NuGet-Paket-Manager installiert ist, klicken Sie auf die **Tools** Menü in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46918-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="46918-131">Wenn Sie ein Menüelement mit dem Namen finden Sie unter **Bibliothekspaket-Manager**, müssen Sie die NuGet-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="46918-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="46918-132">So installieren Sie die NuGet-Paket-Manager:</span><span class="sxs-lookup"><span data-stu-id="46918-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="46918-133">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="46918-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="46918-134">Von der **Tools** , wählen Sie im Menü **Erweiterungen und Updates**.</span><span class="sxs-lookup"><span data-stu-id="46918-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="46918-135">In der **Erweiterungen und Updates** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="46918-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="46918-136">Wenn Sie "NuGet-Paket-Manager" nicht angezeigt wird, geben Sie "Nuget-Paket-Manager" in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="46918-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="46918-137">Wählen Sie den NuGet-Paket-Manager aus, und klicken Sie auf **herunterladen**.</span><span class="sxs-lookup"><span data-stu-id="46918-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="46918-138">Nachdem der Download abgeschlossen ist, werden Sie aufgefordert werden, installieren.</span><span class="sxs-lookup"><span data-stu-id="46918-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="46918-139">Nach Abschluss der Installation werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.</span><span class="sxs-lookup"><span data-stu-id="46918-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="46918-140">Hinzufügen der Web-API-NuGet-Pakets</span><span class="sxs-lookup"><span data-stu-id="46918-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="46918-141">Nach der Installation von NuGet-Paket-Manager fügen Sie das selfhosten der Web-API-Paket dem Projekt hinzu.</span><span class="sxs-lookup"><span data-stu-id="46918-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="46918-142">Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="46918-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="46918-143">*Beachten Sie*: Wenn Sie nicht dieses Menü sehen-Element, stellen Sie sicher, dass dieses NuGet-Paket-Manager ordnungsgemäß installiert.</span><span class="sxs-lookup"><span data-stu-id="46918-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="46918-144">Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="46918-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="46918-145">In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="46918-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="46918-146">Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="46918-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="46918-147">Wählen Sie das ASP.NET Web API-Self-Host-Paket, und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="46918-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="46918-148">Nachdem das Paket installiert wurde, klicken Sie auf **schließen** um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="46918-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="46918-149">Stellen Sie sicher, dass das Paket mit dem Namen Microsoft.AspNet.WebApi.SelfHost, nicht AspNetWebApi.SelfHost zu installieren.</span><span class="sxs-lookup"><span data-stu-id="46918-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="46918-150">Erstellen Sie das Modell und Controller</span><span class="sxs-lookup"><span data-stu-id="46918-150">Create the Model and Controller</span></span>

<span data-ttu-id="46918-151">In diesem Tutorial verwendet die gleichen Modell und Controller-Klassen als die [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="46918-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="46918-152">Fügen Sie eine öffentliche Klasse, die mit dem Namen `Product`.</span><span class="sxs-lookup"><span data-stu-id="46918-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="46918-153">Fügen Sie eine öffentliche Klasse, die mit dem Namen `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="46918-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="46918-154">Leiten Sie diese Klasse von **"System.Web.http.ApiController"**.</span><span class="sxs-lookup"><span data-stu-id="46918-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="46918-155">Weitere Informationen zu den Code in diesem Controller, finden Sie unter den [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="46918-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="46918-156">Dieser Controller definiert drei GET-Aktionen:</span><span class="sxs-lookup"><span data-stu-id="46918-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="46918-157">URI</span><span class="sxs-lookup"><span data-stu-id="46918-157">URI</span></span> | <span data-ttu-id="46918-158">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="46918-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="46918-159">/api/products</span><span class="sxs-lookup"><span data-stu-id="46918-159">/api/products</span></span> | <span data-ttu-id="46918-160">Ruft eine Liste aller Produkte.</span><span class="sxs-lookup"><span data-stu-id="46918-160">Get a list of all products.</span></span> |
| <span data-ttu-id="46918-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="46918-161">/api/products/*id*</span></span> | <span data-ttu-id="46918-162">Abrufen eines Produkts nach ID auf.</span><span class="sxs-lookup"><span data-stu-id="46918-162">Get a product by ID.</span></span> |
| <span data-ttu-id="46918-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="46918-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="46918-164">Erhalten Sie eine Liste der Produkte nach Kategorie.</span><span class="sxs-lookup"><span data-stu-id="46918-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="46918-165">Hosten der Web-API</span><span class="sxs-lookup"><span data-stu-id="46918-165">Host the Web API</span></span>

<span data-ttu-id="46918-166">Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="46918-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="46918-167">Fügen Sie den folgenden Code der **Programm** Klasse.</span><span class="sxs-lookup"><span data-stu-id="46918-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="46918-168">(Optional) Fügen Sie eine HTTP-URL-Namespace-Reservierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="46918-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="46918-169">Diese Anwendung Lauscht auf `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="46918-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="46918-170">Standardmäßig sind Administratorrechte erforderlich, wenn Sie in einer bestimmten HTTP-Adresse lauschen.</span><span class="sxs-lookup"><span data-stu-id="46918-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="46918-171">Wenn Sie das Tutorial ausführen, daher kann diese Fehlermeldung erhalten: "HTTP konnte URL nicht registrieren http://+:8080/" Es gibt zwei Möglichkeiten, um diesen Fehler zu vermeiden:</span><span class="sxs-lookup"><span data-stu-id="46918-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="46918-172">Führen Sie Visual Studio mit Administratorberechtigungen, oder</span><span class="sxs-lookup"><span data-stu-id="46918-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="46918-173">Nutzen Sie Netsh.exe, um die Berechtigung, reservieren die URL für Ihr Konto zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="46918-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="46918-174">Um Netsh.exe verwenden zu können, öffnen Sie eine Eingabeaufforderung mit Administratorrechten aus, und geben Sie den folgenden Befehl: folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="46918-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="46918-175">wo *"Computer\Benutzername"* ist Ihr Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="46918-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="46918-176">Wenn Sie Selbsthosting abgeschlossen sind, achten Sie darauf, dass Sie die Reservierung zu löschen:</span><span class="sxs-lookup"><span data-stu-id="46918-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="46918-177">Rufen Sie die Web-API von einer Clientanwendung (c#)</span><span class="sxs-lookup"><span data-stu-id="46918-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="46918-178">Schreiben wir nun eine einfache Konsolenanwendung, die die Web-API aufruft.</span><span class="sxs-lookup"><span data-stu-id="46918-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="46918-179">Fügen Sie der Projektmappe ein neues Konsolenanwendungsprojekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="46918-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="46918-180">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und wählen **neues Projekt hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="46918-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="46918-181">Erstellen Sie eine neue Konsolenanwendung, die mit dem Namen &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="46918-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="46918-182">Verwenden Sie NuGet Package Manager, das Paket für die ASP.NET Web-API-Core-Bibliotheken hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="46918-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="46918-183">Wählen Sie im Menü Extras **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="46918-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="46918-184">Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="46918-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="46918-185">In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="46918-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="46918-186">Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="46918-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="46918-187">Wählen Sie das Paket Microsoft ASP.NET Web-API-Client-Bibliotheken, und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="46918-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="46918-188">Fügen Sie einen Verweis auf das Projekt SelfHost in ClientApp:</span><span class="sxs-lookup"><span data-stu-id="46918-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="46918-189">Klicken Sie im Projektmappen-Explorer das Projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="46918-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="46918-190">Klicken Sie auf **Verweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="46918-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="46918-191">In der **Verweis-Manager** Dialogfeld unter **Lösung**Option **Projekte**.</span><span class="sxs-lookup"><span data-stu-id="46918-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="46918-192">Wählen Sie das SelfHost-Projekt.</span><span class="sxs-lookup"><span data-stu-id="46918-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="46918-193">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="46918-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="46918-194">Öffnen Sie die Client/Program.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="46918-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="46918-195">Fügen Sie die folgenden **mit** Anweisung:</span><span class="sxs-lookup"><span data-stu-id="46918-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="46918-196">Fügen Sie einen statischen **"HttpClient"** Instanz:</span><span class="sxs-lookup"><span data-stu-id="46918-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="46918-197">Fügen Sie die folgenden Methoden zum Auflisten aller Produkte, Liste eines Produkts nach ID und Liste Produkte nach Kategorie hinzu.</span><span class="sxs-lookup"><span data-stu-id="46918-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="46918-198">Jede dieser Methoden folgt demselben Muster:</span><span class="sxs-lookup"><span data-stu-id="46918-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="46918-199">Rufen Sie **HttpClient.GetAsync** eine GET-Anforderung an den entsprechenden URI senden.</span><span class="sxs-lookup"><span data-stu-id="46918-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="46918-200">Rufen Sie **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="46918-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="46918-201">Diese Methode löst eine Ausnahme aus, wenn der HTTP-Antwortstatus ein Fehlercode ist.</span><span class="sxs-lookup"><span data-stu-id="46918-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="46918-202">Rufen Sie **ReadAsAsync&lt;T&gt;**  um einen CLR-Typ der HTTP-Antwort zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="46918-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="46918-203">Diese Methode ist eine Erweiterungsmethode, die in definierten **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="46918-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="46918-204">Die **"getasync"** und **ReadAsAsync** Methoden sind zwar beide asynchron.</span><span class="sxs-lookup"><span data-stu-id="46918-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="46918-205">Geben sie zurück **Aufgabe** Objekte, die den asynchronen Vorgang darstellen.</span><span class="sxs-lookup"><span data-stu-id="46918-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="46918-206">Abrufen der **Ergebnis** -Eigenschaft sperrt den Thread, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="46918-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="46918-207">Weitere Informationen zur Verwendung von "HttpClient", wie Sie diesen nicht blockierende Aufrufe finden Sie unter [Aufrufen einer Web-API aus einer .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="46918-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="46918-208">Legen Sie vor dem Aufrufen dieser Methoden, auf die HttpClient-Instanz, um die BaseAddress-Eigenschaft "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="46918-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="46918-209">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="46918-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="46918-210">Diese sollte folgende Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="46918-210">This should output the following.</span></span> <span data-ttu-id="46918-211">(Denken Sie daran, die ersten Ausführen der Anwendung SelfHost).</span><span class="sxs-lookup"><span data-stu-id="46918-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
