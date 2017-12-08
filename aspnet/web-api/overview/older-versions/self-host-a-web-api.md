---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Selbsthosting ASP.NET Web-API 1 (c#) | Microsoft Docs
author: MikeWasson
description: "ASP.NET Web-API erfordert keine IIS. Eine Web-API können in Ihren eigenen Hostprozess selbst gehostet werden. In diesem Lernprogramm wird gezeigt, wie eine Web-API in einer Konsole applic hosten wird..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="1b8b6-105">Selbsthosting ASP.NET Web-API 1 (c#)</span><span class="sxs-lookup"><span data-stu-id="1b8b6-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="1b8b6-106">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1b8b6-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="1b8b6-107">ASP.NET Web-API erfordert keine IIS.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="1b8b6-108">Eine Web-API können in Ihren eigenen Hostprozess selbst gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="1b8b6-109">In diesem Lernprogramm wird gezeigt, wie eine Web-API in einer Konsolenanwendung gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="1b8b6-110">**Neue Anwendungen sollten mithilfe von OWIN Web API Self host.**</span><span class="sxs-lookup"><span data-stu-id="1b8b6-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="1b8b6-111">Finden Sie unter [mit der ASP.NET Web API 2 Selbsthosting OWIN](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1b8b6-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="1b8b6-112">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="1b8b6-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="1b8b6-113">Web-API 1</span><span class="sxs-lookup"><span data-stu-id="1b8b6-113">Web API 1</span></span>
> - <span data-ttu-id="1b8b6-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="1b8b6-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="1b8b6-115">Erstellen Sie das Konsolenanwendungsprojekt</span><span class="sxs-lookup"><span data-stu-id="1b8b6-115">Create the Console Application Project</span></span>

<span data-ttu-id="1b8b6-116">Starten Sie Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="1b8b6-117">Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="1b8b6-118">In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="1b8b6-119">Klicken Sie unter **Visual C#-**Option **Windows**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="1b8b6-120">Wählen Sie in der Liste der Projektvorlagen **Konsolenanwendung**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="1b8b6-121">Nennen Sie das Projekt &quot;SelfHost&quot; , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="1b8b6-122">Legen Sie das Zielframework (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="1b8b6-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="1b8b6-123">Wenn Sie Visual Studio 2010 verwenden, ändern Sie das Zielframework in .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="1b8b6-124">(Standardmäßig wird die Projektvorlage abzielt die [.Net Framework Client Profile](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="1b8b6-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="1b8b6-125">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="1b8b6-126">In der **Zielframework** Dropdownliste ändern das Zielframework auf .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="1b8b6-127">Wenn Sie dazu aufgefordert werden, um die Änderung zu übernehmen, klicken Sie auf **Ja**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="1b8b6-128">Installieren von NuGet-Paket-Manager</span><span class="sxs-lookup"><span data-stu-id="1b8b6-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="1b8b6-129">Die NuGet-Paket-Manager ist die einfachste Möglichkeit, die Web-API-Assemblys zu einem ASP.NET-Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="1b8b6-130">Um festzustellen, ob NuGet Package Manager installiert ist, klicken Sie auf die **Tools** Menü in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="1b8b6-131">Wenn Sie ein Datenelement namens Menü finden Sie unter **Bibliothekspaket-Manager**, dann verfügen Sie über NuGet-Paket-Manager.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="1b8b6-132">So installieren Sie das NuGet-Paket-Manager:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="1b8b6-133">Starten Sie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="1b8b6-134">Aus der **Tools** klicken Sie im Menü **Erweiterungen und Updates**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="1b8b6-135">In der **Erweiterungen und Updates** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="1b8b6-136">Wenn Sie "NuGet-Paket-Manager" nicht angezeigt wird, geben Sie "NuGet-Paket-Manager" in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="1b8b6-137">Wählen Sie den NuGet-Paket-Manager, und klicken Sie auf **herunterladen**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="1b8b6-138">Nachdem der Download abgeschlossen ist, werden Sie aufgefordert, diese zu installieren.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="1b8b6-139">Nach Abschluss der Installation werden Sie möglicherweise aufgefordert, Visual Studio neu zu starten.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="1b8b6-140">Fügen Sie das Web-API-NuGet-Paket hinzu</span><span class="sxs-lookup"><span data-stu-id="1b8b6-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="1b8b6-141">Nach der Installation von NuGet-Paket-Manager fügen Sie hinzu, zum Projekt Web-API Self-Host.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="1b8b6-142">Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="1b8b6-143">*Hinweis*: Wenn Sie nicht dieses Menü Element sehen, stellen Sie sicher, dass dieses NuGet-Paket-Manager ordnungsgemäß installiert.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="1b8b6-144">Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="1b8b6-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="1b8b6-145">In der **NugGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="1b8b6-146">Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="1b8b6-147">Wählen Sie die ASP.NET Web API Self Host-Paket, und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="1b8b6-148">Nachdem das Paket installiert wurde, klicken Sie auf **schließen** um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="1b8b6-149">Stellen Sie sicher, dass das Paket mit dem Namen Microsoft.AspNet.WebApi.SelfHost nicht AspNetWebApi.SelfHost installieren.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="1b8b6-150">Erstellen Sie das Modell und der Controller</span><span class="sxs-lookup"><span data-stu-id="1b8b6-150">Create the Model and Controller</span></span>

<span data-ttu-id="1b8b6-151">Dieses Lernprogramm verwendet die gleichen Modell und Controller-Klassen wie der [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="1b8b6-152">Fügen Sie eine öffentliche Klasse, die mit dem Namen `Product`.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="1b8b6-153">Fügen Sie eine öffentliche Klasse, die mit dem Namen `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="1b8b6-154">Leiten Sie diese Klasse von **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="1b8b6-155">Weitere Informationen zu den Code in diesem Controller, finden Sie unter der [Einstieg](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="1b8b6-156">Dieser Controller definiert drei GET-Aktionen:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="1b8b6-157">URI</span><span class="sxs-lookup"><span data-stu-id="1b8b6-157">URI</span></span> | <span data-ttu-id="1b8b6-158">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1b8b6-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="1b8b6-159">/ api /-Produkte</span><span class="sxs-lookup"><span data-stu-id="1b8b6-159">/api/products</span></span> | <span data-ttu-id="1b8b6-160">Ruft eine Liste aller Produkte.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-160">Get a list of all products.</span></span> |
| <span data-ttu-id="1b8b6-161">/API/Produkte/*Id*</span><span class="sxs-lookup"><span data-stu-id="1b8b6-161">/api/products/*id*</span></span> | <span data-ttu-id="1b8b6-162">Abrufen eines Produkts nach ID auf.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-162">Get a product by ID.</span></span> |
| <span data-ttu-id="1b8b6-163">/ API/Produkte /? Kategorie =*Kategorie*</span><span class="sxs-lookup"><span data-stu-id="1b8b6-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="1b8b6-164">Abrufen einer Liste der Produkte nach Kategorie.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="1b8b6-165">Hosten Sie die Web-API</span><span class="sxs-lookup"><span data-stu-id="1b8b6-165">Host the Web API</span></span>

<span data-ttu-id="1b8b6-166">Öffnen Sie die Datei "Program.cs", und fügen Sie die folgenden using-Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="1b8b6-167">Fügen Sie folgenden Code, der **Programm** Klasse.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="1b8b6-168">(Optional) Fügen Sie eine HTTP-URL-Namespace-Reservierung hinzu.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="1b8b6-169">Diese Anwendung überwacht `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="1b8b6-170">Standardmäßig sind Administratorrechte erforderlich, wenn Sie an einer bestimmten HTTP-Adresse lauschen.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="1b8b6-171">Wenn Sie das Lernprogramm ausführen, daher erhalten Sie möglicherweise diesen Fehler: "HTTP konnte URL http://+:8080/ nicht registriert" Es gibt zwei Möglichkeiten, um diesen Fehler zu vermeiden:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="1b8b6-172">Ausführen von Visual Studio mit erweiterten Administratorberechtigungen, oder</span><span class="sxs-lookup"><span data-stu-id="1b8b6-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="1b8b6-173">Verwenden Sie Netsh.exe, um den Kontoberechtigungen reserviert die URL zu gewähren.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="1b8b6-174">Um Netsh.exe verwenden möchten, öffnen Sie eine Eingabeaufforderung mit Administratorrechten, und geben Sie den folgenden Befehl: folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="1b8b6-175">wobei *Computer\Benutzername* Ihr Benutzerkonto ist.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="1b8b6-176">Wenn Sie Selbsthosting abgeschlossen sind, achten Sie darauf, dass Sie die Reservierung zu löschen:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="1b8b6-177">Rufen Sie die Web-API aus einer Clientanwendung (c#)</span><span class="sxs-lookup"><span data-stu-id="1b8b6-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="1b8b6-178">Schreiben Sie eine einfache Konsolenanwendung, die die Web-API aufruft.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="1b8b6-179">Fügen Sie der Projektmappe ein neues Konsolenanwendungsprojekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="1b8b6-180">Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in der Projektmappe, und wählen **neues Projekt hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="1b8b6-181">Erstellen Sie eine neue Konsolenanwendung mit dem Namen &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="1b8b6-182">Verwenden Sie NuGet Package Manager, die ASP.NET Web API Core Libraries Paket hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="1b8b6-183">Wählen Sie im Menü Extras **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="1b8b6-184">Wählen Sie **NuGet-Pakete für Projektmappe verwalten...**</span><span class="sxs-lookup"><span data-stu-id="1b8b6-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="1b8b6-185">In der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **Online**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="1b8b6-186">Geben Sie in das Suchfeld &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="1b8b6-187">Wählen Sie das Paket für Microsoft ASP.NET Web API Client Libraries, und klicken Sie auf **installieren**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="1b8b6-188">Fügen Sie einen Verweis auf das Projekt SelfHost in ClientApp:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="1b8b6-189">Im Projektmappen-Explorer mit der rechten Maustaste ClientApp-Projekt.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="1b8b6-190">Wählen Sie **Verweis hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="1b8b6-191">In der **Verweis-Manager** Dialogfeld unter **Lösung**Option **Projekte**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="1b8b6-192">Wählen Sie das Projekt SelfHost.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="1b8b6-193">Klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="1b8b6-194">Öffnen Sie die Client/Program.cs-Datei.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="1b8b6-195">Fügen Sie die folgenden **mit** Anweisung:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="1b8b6-196">Fügen Sie einen statischen **"HttpClient"** Instanz:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="1b8b6-197">Fügen Sie die folgenden Methoden zum Auflisten aller Produkte, ein Produkt nach ID-Liste und Liste Produkte nach Kategorie.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="1b8b6-198">Jede dieser Methoden erfolgt nach dem gleichen Muster:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="1b8b6-199">Rufen Sie **HttpClient.GetAsync** zum Senden einer GET-Anforderung zum entsprechenden URI.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="1b8b6-200">Rufen Sie **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="1b8b6-201">Diese Methode löst eine Ausnahme aus, der HTTP-Antwort-Status ist ein Fehlercode.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="1b8b6-202">Rufen Sie **ReadAsAsync&lt;T&gt;**  um einen CLR-Typ der HTTP-Antwort zu deserialisieren.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="1b8b6-203">Diese Methode ist eine Erweiterungsmethode, die in definierten **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="1b8b6-204">Die **GetAsync** und **ReadAsAsync** Methoden sind sowohl asynchrone.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="1b8b6-205">Diese zurückgeben **Aufgabe** Objekte, die den asynchronen Vorgang darstellen.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="1b8b6-206">Abrufen der **Ergebnis** -Eigenschaft sperrt den Thread, bis der Vorgang abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="1b8b6-207">Weitere Informationen zum Verwenden von "HttpClient", einschließlich der Erstellung nicht blockierende Aufrufe, finden Sie unter [ein Web-API aus einer .NET Client aufrufen](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="1b8b6-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="1b8b6-208">Vor dem Aufrufen dieser Methoden, legen Sie die BaseAddress-Eigenschaft auf die Instanz "HttpClient", "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="1b8b6-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="1b8b6-209">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1b8b6-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="1b8b6-210">Diese sollte folgende Ausgabe.</span><span class="sxs-lookup"><span data-stu-id="1b8b6-210">This should output the following.</span></span> <span data-ttu-id="1b8b6-211">(Denken Sie daran, führen Sie die Anwendung SelfHost zuerst).</span><span class="sxs-lookup"><span data-stu-id="1b8b6-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
