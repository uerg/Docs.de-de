---
title: Wiederverwendbare Razor-Benutzeroberfläche in Klassenbibliotheken mit ASP.NET Core
author: Rick-Anderson
description: Informationen zum Erstellen einer wiederverwendbaren Razor-Benutzeroberfläche in einer Klassenbibliothek
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: advanced
uid: mvc/razor-pages/ui-class
ms.openlocfilehash: 44091ab8ab5d69a5975e191fd00fca1d1d957238
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a><span data-ttu-id="56314-103">Erstellen einer wiederverwendbaren Benutzeroberfläche mithilfe eines RCL-Projekts in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="56314-103">Create reusable UI using the Razor Class Library project in ASP.NET Core.</span></span>

<span data-ttu-id="56314-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="56314-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="56314-105">Ansichten, Seiten, Controller, Seitenmodelle und Datenmodelle im Zusammenhang mit Razor können in einer Razor-Klassenbibliothek (Razor Class Library, RCL) zusammengefasst und erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="56314-105">Razor views, pages, controllers, page models, and data models can be built into a Razor Class Library (RCL).</span></span> <span data-ttu-id="56314-106">Die RCL kann verpackt und wiederverwendet werden.</span><span class="sxs-lookup"><span data-stu-id="56314-106">The RCL can be packaged and reused.</span></span> <span data-ttu-id="56314-107">Sie kann in Anwendungen eingebunden werden, wodurch sich die in der RCL enthaltenen Ansichten und Seiten überschreiben lassen.</span><span class="sxs-lookup"><span data-stu-id="56314-107">Applications can include the RCL and override the views and pages it contains.</span></span> <span data-ttu-id="56314-108">Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang.</span><span class="sxs-lookup"><span data-stu-id="56314-108">When a view, partial view, or Razor Page is found in both the web app and the RCL, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span>

<span data-ttu-id="56314-109">Für dieses Feature ist [!INCLUDE[](~/includes/2.1-SDK.md)] erforderlich.</span><span class="sxs-lookup"><span data-stu-id="56314-109">This feature requires [!INCLUDE[](~/includes/2.1-SDK.md)]</span></span>

<span data-ttu-id="56314-110">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="56314-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="create-a-class-library-containing-razor-ui"></a><span data-ttu-id="56314-111">Erstellen einer Klassenbibliothek, die eine Razor-Benutzeroberfläche enthält</span><span class="sxs-lookup"><span data-stu-id="56314-111">Create a class library containing Razor UI</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56314-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56314-112">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="56314-113">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="56314-113">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="56314-114">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="56314-114">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="56314-115">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="56314-115">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="56314-116">Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="56314-116">Select **Razor Class Library** > **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56314-117">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="56314-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="56314-118">Führen Sie `dotnet new razorclasslib` über die Befehlszeile aus.</span><span class="sxs-lookup"><span data-stu-id="56314-118">From the commandline, run `dotnet new razorclasslib`.</span></span> <span data-ttu-id="56314-119">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="56314-119">For example:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

<span data-ttu-id="56314-120">Weitere Informationen finden Sie unter [dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="56314-120">For more information, see [dotnet new](/dotnet/core/tools/dotnet-new).</span></span>

------
<span data-ttu-id="56314-121">Fügen Sie der RCL Razor-Dateien hinzu.</span><span class="sxs-lookup"><span data-stu-id="56314-121">Add Razor files to the RCL.</span></span>

<span data-ttu-id="56314-122">Es wird empfohlen, RCL-Inhalte im Ordner *Areas* (Bereiche) abzulegen.</span><span class="sxs-lookup"><span data-stu-id="56314-122">We recommend RCL content go in the *Areas* folder.</span></span> 


## <a name="referencing-razor-class-library-content"></a><span data-ttu-id="56314-123">Verweisen auf RCL-Inhalte</span><span class="sxs-lookup"><span data-stu-id="56314-123">Referencing Razor Class Library content</span></span>

<span data-ttu-id="56314-124">Folgende Komponenten können auf die RCL verweisen:</span><span class="sxs-lookup"><span data-stu-id="56314-124">The RCL can be referenced by:</span></span>

* <span data-ttu-id="56314-125">NuGet-Pakete.</span><span class="sxs-lookup"><span data-stu-id="56314-125">NuGet package.</span></span> <span data-ttu-id="56314-126">Weitere Informationen finden Sie unter [Creating NuGet packages](/nuget/create-packages/creating-a-package) (Erstellen von NuGet-Paketen), [dotnet add package](/dotnet/core/tools/dotnet-add-package) und [Erstellen und Veröffentlichen eines NuGet-Pakets](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="56314-126">See [Creating NuGet packages](/nuget/create-packages/creating-a-package) and [dotnet add package](/dotnet/core/tools/dotnet-add-package) and [Create and publish a NuGet package](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).</span></span>
* <span data-ttu-id="56314-127">*{ProjectName}.csproj*-Dateien.</span><span class="sxs-lookup"><span data-stu-id="56314-127">*{ProjectName}.csproj*.</span></span> <span data-ttu-id="56314-128">Weitere Informationen finden Sie unter [dotnet add reference](/dotnet/core/tools/dotnet-add-reference).</span><span class="sxs-lookup"><span data-stu-id="56314-128">See [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).</span></span>

### <a name="partial-files-access-in-the-rcl"></a><span data-ttu-id="56314-129">Zugriff auf Teildateien in der RCL</span><span class="sxs-lookup"><span data-stu-id="56314-129">Partial files access in the RCL</span></span>

<span data-ttu-id="56314-130">Die ASP.NET Core-Laufzeit sucht im Fall von Inhalten, die sich außerhalb der RCL befinden, nicht nach Teildateien in der RCL.</span><span class="sxs-lookup"><span data-stu-id="56314-130">For content outside the RCL, the ASP.NET Core runtime does not search for partial files in the RCL.</span></span>

<span data-ttu-id="56314-131">Im Beispieldownload kann innerhalb von *WebApp1\Pages\About.cshtml* beispielsweise **nicht** auf die Teilansicht *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* verwiesen werden.</span><span class="sxs-lookup"><span data-stu-id="56314-131">For example, in the sample download, the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view can **not** be referenced in *WebApp1\Pages\About.cshtml*.</span></span> <span data-ttu-id="56314-132">Seiten in der RCL (*RazorUIClassLib /*) **können hingegen durchaus** auf *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* zugreifen.</span><span class="sxs-lookup"><span data-stu-id="56314-132">However, pages in the RCL ( *RazorUIClassLib/* **can** access *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a><span data-ttu-id="56314-133">Exemplarische Vorgehensweise: Erstellen eines RCL-Projekts und Verwenden des Projekts über ein Razor-Seiten-Projekt</span><span class="sxs-lookup"><span data-stu-id="56314-133">Walkthrough: Create a Razor Class Library project and use from a Razor Pages project</span></span>

<span data-ttu-id="56314-134">Sie können das [vollständige Projekt](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) herunterladen und testen, anstatt es zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="56314-134">You can download the [complete project](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/ui-class/samples) and test it rather than creating it.</span></span> <span data-ttu-id="56314-135">Der Beispieldownload enthält zusätzlichen Code sowie Links, die das Testen des Projekts erleichtern.</span><span class="sxs-lookup"><span data-stu-id="56314-135">The sample download contains additional code and links that make the project easy to test.</span></span> <span data-ttu-id="56314-136">[In diesem GitHub-Issue](https://github.com/aspnet/Docs/issues/6098) können Sie in Form von Kommentaren Feedback zu Unterschieden zwischen Beispieldownloads und ausführlichen Anleitungen geben.</span><span class="sxs-lookup"><span data-stu-id="56314-136">You can leave feedback in [this GitHub issue](https://github.com/aspnet/Docs/issues/6098) with your comments on download samples versus step-by-step instructions.</span></span>

### <a name="test-the-download-app"></a><span data-ttu-id="56314-137">Testen der heruntergeladenen App</span><span class="sxs-lookup"><span data-stu-id="56314-137">Test the download app</span></span>

<span data-ttu-id="56314-138">Wenn Sie die vollständige App nicht heruntergeladen haben und das Beispielprojekt selbst erstellen möchten, können Sie mit dem [nächsten Abschnitt](#create-a-razor-class-library) fortfahren.</span><span class="sxs-lookup"><span data-stu-id="56314-138">If you haven't downloaded the completed app and would rather create the walkthrough project, skip to the [next section](#create-a-razor-class-library).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56314-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56314-139">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="56314-140">Öffnen Sie die *SLN*-Datei in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="56314-140">Open the *.sln* file in Visual Studio.</span></span> <span data-ttu-id="56314-141">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="56314-141">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56314-142">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="56314-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="56314-143">Erstellen Sie über die Eingabeaufforderung im *cli*-Verzeichnis die RCL und die Web-App.</span><span class="sxs-lookup"><span data-stu-id="56314-143">From a command prompt in the *cli* directory, build the RCL and web app.</span></span>

``` CLI
dotnet build
```

<span data-ttu-id="56314-144">Wechseln Sie ins Verzeichnis *WebApp1*, und führen Sie die App aus:</span><span class="sxs-lookup"><span data-stu-id="56314-144">Move to the *WebApp1* directory and run the app:</span></span>

``` CLI
dotnet run
```
------

<span data-ttu-id="56314-145">Folgen Sie den Anweisungen in [Testen des WebApp1-Projekts](#test).</span><span class="sxs-lookup"><span data-stu-id="56314-145">Follow the instructions in [Test WebApp1](#test)</span></span>

## <a name="create-a-razor-class-library"></a><span data-ttu-id="56314-146">Erstellen einer RCL</span><span class="sxs-lookup"><span data-stu-id="56314-146">Create a Razor Class Library</span></span>

<span data-ttu-id="56314-147">In diesem Abschnitt wird das Erstellen einer RCL beschrieben.</span><span class="sxs-lookup"><span data-stu-id="56314-147">In this section, a Razor Class Library (RCL) is created.</span></span> <span data-ttu-id="56314-148">Dabei werden der RCL Razor-Dateien hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="56314-148">Razor files are added to the RCL.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56314-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56314-149">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="56314-150">Erstellen Sie das RCL-Projekt:</span><span class="sxs-lookup"><span data-stu-id="56314-150">Create the RCL project:</span></span>

* <span data-ttu-id="56314-151">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="56314-151">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="56314-152">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="56314-152">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="56314-153">Legen Sie als Namen für die App **RazorUIClassLib** fest.</span><span class="sxs-lookup"><span data-stu-id="56314-153">Name the app **RazorUIClassLib**.</span></span>
* <span data-ttu-id="56314-154">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="56314-154">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="56314-155">Klicken Sie auf **Razor Class Library** (Razor-Klassenbibliothek) > **OK**.</span><span class="sxs-lookup"><span data-stu-id="56314-155">Select **Razor Class Library** > **OK**.</span></span>

<span data-ttu-id="56314-156">Erstellen Sie die Web-App mit Razor-Seiten:</span><span class="sxs-lookup"><span data-stu-id="56314-156">Create the Razor Pages web app:</span></span>

* <span data-ttu-id="56314-157">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf die Projektmappe und anschließend auf **Hinzufügen** >  **Neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="56314-157">From **Solution Explorer**, right-click the solution > **Add** >  **New Project**.</span></span>
* <span data-ttu-id="56314-158">Wählen Sie **ASP.NET Core-Webanwendung** aus.</span><span class="sxs-lookup"><span data-stu-id="56314-158">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="56314-159">Legen Sie als Namen für die App **WebApp1** fest.</span><span class="sxs-lookup"><span data-stu-id="56314-159">Name the app **WebApp1**.</span></span>
* <span data-ttu-id="56314-160">Überprüfen Sie, ob **ASP.NET Core 2.1** oder höher ausgewählt ist.</span><span class="sxs-lookup"><span data-stu-id="56314-160">Verify **ASP.NET Core 2.1** or later is selected.</span></span>
* <span data-ttu-id="56314-161">Klicken Sie auf **Webanwendung** > **OK**.</span><span class="sxs-lookup"><span data-stu-id="56314-161">Select **Web Application** > **OK**.</span></span>

### <a name="add-razor-files-and-folders-to-the-project"></a><span data-ttu-id="56314-162">Fügen Sie dem Projekt Razor-Dateien und -Ordner hinzu:</span><span class="sxs-lookup"><span data-stu-id="56314-162">Add Razor files and folders to the project.</span></span>

* <span data-ttu-id="56314-163">Fügen Sie eine Razor-Datei für die Teilansicht mit dem Namen „_Message.cshtml“ unter *RazorUIClassLib/Areas/MyFeature/Pages/Shared* hinzu.</span><span class="sxs-lookup"><span data-stu-id="56314-163">Add a Razor partial view file named *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span>
* <span data-ttu-id="56314-164">Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="56314-164">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="56314-165">Kopieren Sie die Datei *_ViewStart.cshtml* aus dem WebApp1-Projekt nach *RazorUIClassLib/Areas/MyFeature/Pages*.</span><span class="sxs-lookup"><span data-stu-id="56314-165">Copy the *_ViewStart.cshtml* file from the WebApp1 project to  *RazorUIClassLib/Areas/MyFeature/Pages/_ViewStart.cshtml*.</span></span>

  <span data-ttu-id="56314-166">Die Datei [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) ist für die Nutzung des Layouts des Razor-Seiten-Projekts erforderlich.</span><span class="sxs-lookup"><span data-stu-id="56314-166">The [viewstart](xref:mvc/views/layout#running-code-before-each-view) file is required to use the layout of the Razor Pages project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56314-167">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="56314-167">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="56314-168">Führen Sie die folgenden Befehle über die Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="56314-168">From the command line, run the following:</span></span>

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="56314-169">Die obenstehenden Befehle haben folgende Konsequenzen:</span><span class="sxs-lookup"><span data-stu-id="56314-169">The preceding commands:</span></span>

* <span data-ttu-id="56314-170">Die `RazorUIClassLib`-RCL wird erstellt.</span><span class="sxs-lookup"><span data-stu-id="56314-170">Creates the `RazorUIClassLib` Razor Class Library (RCL).</span></span>
* <span data-ttu-id="56314-171">Die Seite „Razor_Message“ wird erstellt und der RCL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="56314-171">Creates a Razor _Message page, and adds it to the RCL.</span></span> <span data-ttu-id="56314-172">Durch den `-np`-Parameter wird die Seite ohne `PageModel` erstellt.</span><span class="sxs-lookup"><span data-stu-id="56314-172">The `-np` parameter creates the page without a `PageModel`.</span></span>
* <span data-ttu-id="56314-173">Die Datei [_ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) wird erstellt und der RCL hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="56314-173">Creates a [viewstart](xref:mvc/views/layout#running-code-before-each-view) file and adds it to the RCL.</span></span>

<span data-ttu-id="56314-174">Die Datei „_ViewStart.cshtml“ ist erforderlich, um das Layout des Razor-Seiten-Projekts (das im nächsten Abschnitt hinzugefügt wird) verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="56314-174">The viewstart file is required to use the layout of the Razor Pages project (which is added in the next section).</span></span>

<span data-ttu-id="56314-175">Aktualisieren Sie die Razor-Seiten wie folgt:</span><span class="sxs-lookup"><span data-stu-id="56314-175">Update the Razor Pages:</span></span>

* <span data-ttu-id="56314-176">Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="56314-176">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* <span data-ttu-id="56314-177">Ersetzen Sie das Markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="56314-177">Replace the markup in *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* with the following code:</span></span>

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

<span data-ttu-id="56314-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` ist erforderlich, um die Teilansicht `<partial name="_Message" />` verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="56314-178">`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` is required to use the partial view (`<partial name="_Message" />`).</span></span> <span data-ttu-id="56314-179">Wenn Sie nicht die `@addTagHelper`-Anweisung einbinden möchten, können Sie die Datei *_ViewImports.cshtml* hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="56314-179">Rather than including the `@addTagHelper` directive, you can add a *_ViewImports.cshtml* file.</span></span> <span data-ttu-id="56314-180">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="56314-180">For example:</span></span>

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

<span data-ttu-id="56314-181">Weitere Informationen zu „_ViewImports.cshtml“ finden Sie unter [Importieren gemeinsam verwendeter Anweisungen](xref:mvc/views/layout#importing-shared-directives).</span><span class="sxs-lookup"><span data-stu-id="56314-181">For more information on viewimports, see [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives)</span></span>

* <span data-ttu-id="56314-182">Erstellen Sie die Klassenbibliothek, um sicherzustellen, dass keine Compilerfehler vorliegen:</span><span class="sxs-lookup"><span data-stu-id="56314-182">Build the class library to verify there are no compiler errors:</span></span>

``` CLI
dotnet build RazorUIClassLib
```

<span data-ttu-id="56314-183">Die Buildausgabe enthält *RazorUIClassLib.dll* und *RazorUIClassLib.Views.dll*.</span><span class="sxs-lookup"><span data-stu-id="56314-183">The build output contains *RazorUIClassLib.dll* and *RazorUIClassLib.Views.dll*.</span></span> <span data-ttu-id="56314-184">*RazorUIClassLib.Views.dll* enthält den kompilierten Razor-Inhalt.</span><span class="sxs-lookup"><span data-stu-id="56314-184">*RazorUIClassLib.Views.dll* contains the compiled Razor content.</span></span>

------

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a><span data-ttu-id="56314-185">Verwenden der Razor-Benutzeroberflächenbibliothek über ein Razor-Seiten-Projekt</span><span class="sxs-lookup"><span data-stu-id="56314-185">Use the Razor UI library from a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="56314-186">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="56314-186">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="56314-187">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Als Startprojekt festlegen**.</span><span class="sxs-lookup"><span data-stu-id="56314-187">From **Solution Explorer**, right-click on **WebApp1** and select **Set as StartUp Project**.</span></span>
* <span data-ttu-id="56314-188">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Buildabhängigkeiten** > **Projektabhängigkeiten**.</span><span class="sxs-lookup"><span data-stu-id="56314-188">From **Solution Explorer**, right-click on **WebApp1** and select **Build Dependencies** > **Project Dependencies**.</span></span>
* <span data-ttu-id="56314-189">Aktivieren Sie für **WebApp1** die Abhängigkeit **RazorUIClassLib**.</span><span class="sxs-lookup"><span data-stu-id="56314-189">Check **RazorUIClassLib** as a dependency of **WebApp1**.</span></span>
* <span data-ttu-id="56314-190">Klicken Sie im **Projektmappen-Explorer** zuerst mit der rechten Maustaste auf **WebApp1** und anschließend auf **Hinzufügen** > **Verweis**.</span><span class="sxs-lookup"><span data-stu-id="56314-190">From **Solution Explorer**, right-click on **WebApp1** and select **Add** > **Reference**.</span></span>
* <span data-ttu-id="56314-191">Aktivieren Sie im Dialogfeld **Verweis-Manager** das Kontrollkästchen neben **RazorUIClassLib**, und klicken Sie anschließend auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="56314-191">In the **Reference Manager** dialog, check **RazorUIClassLib** > **OK**.</span></span>

<span data-ttu-id="56314-192">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="56314-192">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="56314-193">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="56314-193">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="56314-194">Erstellen Sie eine Web-App mit Razor-Seiten und eine Projektmappendatei, die die Razor-Seiten-App und die RCL enthält:</span><span class="sxs-lookup"><span data-stu-id="56314-194">Create a Razor Pages web app and a solution file containing the Razor Pages app and the Razor Class Library:</span></span>

``` CLI
dotnet new razor -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

<span data-ttu-id="56314-195">Erstellen Sie die Web-App, und führen Sie sie aus:</span><span class="sxs-lookup"><span data-stu-id="56314-195">Build and run the web app:</span></span>

``` CLI
cd WebApp1
dotnet run
```

------

<a name="test"></a>

### <a name="test-webapp1"></a><span data-ttu-id="56314-196">Testen des WebApp1-Projekts</span><span class="sxs-lookup"><span data-stu-id="56314-196">Test WebApp1</span></span>

<span data-ttu-id="56314-197">Überprüfen Sie, ob die Razor-Benutzeroberflächenbibliothek verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="56314-197">Verify the Razor UI class library is being used.</span></span>

* <span data-ttu-id="56314-198">Wechseln Sie zu `/MyFeature/Page1`.</span><span class="sxs-lookup"><span data-stu-id="56314-198">Browse to `/MyFeature/Page1`.</span></span>

## <a name="override-views-partial-views-and-pages"></a><span data-ttu-id="56314-199">Überschreiben von Ansichten, Teilansichten und Seiten</span><span class="sxs-lookup"><span data-stu-id="56314-199">Override views, partial views, and pages</span></span>

<span data-ttu-id="56314-200">Wenn eine Ansicht, Teilansicht oder Razor-Seite sowohl in der Web-App als auch in der RCL enthalten ist, hat das Razor-Markup (die *CSHTML*-Datei) in der Web-App Vorrang.</span><span class="sxs-lookup"><span data-stu-id="56314-200">When a view, partial view, or Razor Page is found in both the web app and the Razor Class Library, the Razor markup (*.cshtml* file) in the web app takes precedence.</span></span> <span data-ttu-id="56314-201">Wenn Sie z.B. *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* zu WebApp1 hinzufügen, hat Page1 in WebApp1 Vorrang vor Page1 in der RCL.</span><span class="sxs-lookup"><span data-stu-id="56314-201">For example, add *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* to WebApp1, and Page1 in the WebApp1 will take precedence over Page1in the Razor Class Library.</span></span>

<span data-ttu-id="56314-202">Ob Page1 in WebApp1 tatsächlich die Priorität zugeordnet wird, können Sie testen, indem Sie im Beispieldownload *WebApp1/Areas/MyFeature2* in *WebApp1/Areas/MyFeature* umbenennen.</span><span class="sxs-lookup"><span data-stu-id="56314-202">In the sample download, rename *WebApp1/Areas/MyFeature2* to *WebApp1/Areas/MyFeature* to test precedence.</span></span>

<span data-ttu-id="56314-203">Kopieren Sie die Teilansicht *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* nach *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="56314-203">Copy the *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* partial view to *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*.</span></span> <span data-ttu-id="56314-204">Aktualisieren Sie das Markup, um den neuen Speicherort anzugeben.</span><span class="sxs-lookup"><span data-stu-id="56314-204">Update the markup to indicate the new location.</span></span> <span data-ttu-id="56314-205">Erstellen Sie die App, und führen Sie sie aus, um sicherzustellen, dass die Version mit der Teilansicht der App verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="56314-205">Build and run the app to verify the app's version of the partial is being used.</span></span>
