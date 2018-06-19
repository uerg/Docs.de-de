---
title: Erste Schritte mit NSwag und ASP.NET Core
author: zuckerthoben
description: Erfahren Sie, wie Sie NSwag zum Generieren von Dokumentationen und Hilfeseiten für eine ASP.NET Core-Web-API-App verwenden können.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 0f0aa0eeaa174ef40f03aab2500a8b3ce37e9448
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
ms.locfileid: "34094891"
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="f3de9-103">Erste Schritte mit NSwag und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3de9-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="f3de9-104">Von [Christoph Nienaber](https://twitter.com/zuckerthoben) und [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="f3de9-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f3de9-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f3de9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f3de9-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f3de9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="f3de9-107">Die Verwendung von [NSwag](https://github.com/RSuter/NSwag) mit ASP.NET Core-Middleware erfordert das NuGet-Paket [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="f3de9-107">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="f3de9-108">Das Paket besteht aus einem Swagger-Generator, der Swagger-Benutzeroberfläche (Version 2 und 3) und der [ReDoc-Benutzeroberfläche](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="f3de9-108">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="f3de9-109">Es wird empfohlen, die Funktionen zur Codegenerierung von NSwag zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f3de9-109">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="f3de9-110">Wählen Sie eine der folgenden Optionen für die Codegenerierung aus:</span><span class="sxs-lookup"><span data-stu-id="f3de9-110">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="f3de9-111">Verwenden Sie [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio). Dies ist eine Windows-Desktop-App zum Generieren von Clientcode in C# und TypeScript für Ihre API.</span><span class="sxs-lookup"><span data-stu-id="f3de9-111">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API.</span></span>
* <span data-ttu-id="f3de9-112">Verwenden Sie die NuGet-Pakete [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) oder [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), um Code innerhalb des Projekts zu generieren.</span><span class="sxs-lookup"><span data-stu-id="f3de9-112">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project.</span></span>
* <span data-ttu-id="f3de9-113">Verwenden Sie NSwag über die [Befehlszeile](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="f3de9-113">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine).</span></span>
* <span data-ttu-id="f3de9-114">Verwenden Sie das NuGet-Paket [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="f3de9-114">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package.</span></span>

## <a name="features"></a><span data-ttu-id="f3de9-115">Features</span><span class="sxs-lookup"><span data-stu-id="f3de9-115">Features</span></span>

<span data-ttu-id="f3de9-116">Der Hauptgrund für die Verwendung von NSwag liegt darin, dass nicht nur die Swagger-Benutzeroberfläche und der Swagger-Generator, sondern auch die flexiblen Funktionen für die Codegenerierung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="f3de9-116">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="f3de9-117">Sie müssen keine API besitzen, sondern können APIs von Drittanbietern verwenden, die Swagger integrieren, und NSwag eine Clientimplementierung generieren lassen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-117">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="f3de9-118">In jedem Fall wird der Entwicklungszyklus beschleunigt und Sie können sich schnell an API-Änderungen anpassen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-118">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="f3de9-119">Paketinstallation</span><span class="sxs-lookup"><span data-stu-id="f3de9-119">Package installation</span></span>

<span data-ttu-id="f3de9-120">Das NuGet-Paket „NSwag“ kann folgendermaßen hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="f3de9-120">The NSwag NuGet package can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f3de9-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3de9-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f3de9-122">Aus dem Fenster **Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="f3de9-122">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="f3de9-123">Navigieren Sie zu **Ansicht** > **Weitere Fenster** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-123">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="f3de9-124">Navigieren Sie zu dem Verzeichnis, in dem die *TodoApi.csproj*-Datei gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="f3de9-124">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="f3de9-125">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f3de9-125">Execute the following command:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="f3de9-126">Aus dem Dialogfeld **NuGet-Pakete verwalten**:</span><span class="sxs-lookup"><span data-stu-id="f3de9-126">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="f3de9-127">Klicken Sie mit der rechten Maustaste unter **Projektmappen-Explorer** > **NuGet-Pakete verwalten** auf Ihr Projekt.</span><span class="sxs-lookup"><span data-stu-id="f3de9-127">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="f3de9-128">Legen Sie die **Paketquelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="f3de9-128">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="f3de9-129">Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="f3de9-129">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="f3de9-130">Wählen Sie das Paket „NSwag.AspNetCore“ auf der Registerkarte **Durchsuchen** aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-130">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f3de9-131">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="f3de9-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f3de9-132">Klicken Sie mit der rechten Maustaste auf den Ordner *Pakete* unter **Lösungspad** > **Pakete hinzufügen...**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-132">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="f3de9-133">Legen Sie im Fenster **Pakete hinzufügen** das Dropdownmenü **Quelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="f3de9-133">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="f3de9-134">Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="f3de9-134">Enter "NSwag.AspNetCore" in the search box</span></span>
* <span data-ttu-id="f3de9-135">Wählen Sie das Paket „NSwag.AspNetCore“ aus dem Ergebnisbereich aus, und klicken Sie auf **Paket hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-135">Select the "NSwag.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f3de9-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3de9-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f3de9-137">Führen Sie folgenden Befehl aus dem **integrierten Terminal** aus:</span><span class="sxs-lookup"><span data-stu-id="f3de9-137">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="f3de9-138">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="f3de9-138">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="f3de9-139">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="f3de9-139">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="f3de9-140">Hinzufügen und Konfigurieren von Swagger-Middleware</span><span class="sxs-lookup"><span data-stu-id="f3de9-140">Add and configure Swagger middleware</span></span>

<span data-ttu-id="f3de9-141">Importieren Sie folgende Namespaces in die `Startup`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="f3de9-141">Import the following namespaces in the `Startup` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_StartupConfigureImports)]

<span data-ttu-id="f3de9-142">Aktivieren Sie die Middleware in der `Startup.Configure`-Methode, um die generierte Swagger-Spezifikation und die Swagger-Benutzeroberfläche zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="f3de9-142">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=6-10)]

<span data-ttu-id="f3de9-143">Starten Sie die App.</span><span class="sxs-lookup"><span data-stu-id="f3de9-143">Launch the app.</span></span> <span data-ttu-id="f3de9-144">Navigieren Sie zu `http://localhost:<port>/swagger`, um die Swagger-Benutzeroberfläche anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-144">Navigate to `http://localhost:<port>/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="f3de9-145">Navigieren Sie zu `http://localhost:<port>/swagger/v1/swagger.json`, um die Swagger-Spezifikation anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-145">Navigate to `http://localhost:<port>/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="f3de9-146">Codeerzeugung</span><span class="sxs-lookup"><span data-stu-id="f3de9-146">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="f3de9-147">Über NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="f3de9-147">Via NSwagStudio</span></span>

* <span data-ttu-id="f3de9-148">Installieren Sie NSwagStudio über das offizielle [GitHub-Repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="f3de9-148">Install NSwagStudio from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>
* <span data-ttu-id="f3de9-149">Starten Sie NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="f3de9-149">Launch NSwagStudio.</span></span> <span data-ttu-id="f3de9-150">Geben Sie die Datei-URL *swagger.json* in das Textfeld **Swagger Specification URL** (URL zur Spezifizierung des Swaggers) ein, und klicken Sie auf die Schaltfläche **Create local Copy** (Lokale Kopie erstellen).</span><span class="sxs-lookup"><span data-stu-id="f3de9-150">Enter the *swagger.json* file URL in the **Swagger Specification URL** textbox, and click the **Create local Copy** button.</span></span>
* <span data-ttu-id="f3de9-151">Klicken Sie auf den Clientausgabetyp **CSharp Client**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-151">Select the **CSharp Client** client output type.</span></span> <span data-ttu-id="f3de9-152">Zu den Optionen zählen außerdem der **TypeScript-Client** und der **CSharp-Web-API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-152">Other options include **TypeScript Client** and **CSharp Web API Controller**.</span></span> <span data-ttu-id="f3de9-153">Bei der Verwendung eines Web-API-Controllers handelt es sich im Grunde um eine umgekehrte Generierung.</span><span class="sxs-lookup"><span data-stu-id="f3de9-153">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="f3de9-154">Dieser verwendet die Spezifikation eines Diensts, um den Dienst erneut zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-154">It uses a specification of a service to rebuild the service.</span></span>
* <span data-ttu-id="f3de9-155">Klicken Sie auf die Schaltfläche **Ausgaben generieren**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-155">Click the **Generate Outputs** button.</span></span> <span data-ttu-id="f3de9-156">Dann wird eine vollständige Clientimplementierung des *TodoApi.NSwag*-Projekts in C# durchgeführt.</span><span class="sxs-lookup"><span data-stu-id="f3de9-156">A complete C# client implementation of the *TodoApi.NSwag* project is produced.</span></span> <span data-ttu-id="f3de9-157">Klicken Sie auf die Registerkarte **CSharp-Client** im Abschnitt **Ausgaben**, damit der generierte Clientcode angezeigt wird:</span><span class="sxs-lookup"><span data-stu-id="f3de9-157">Click the **CSharp Client** tab of the **Outputs** section to see the generated client code:</span></span>

```csharp
//----------------------
// <auto-generated>
//     Generated using the NSwag toolchain v11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0)) (http://NSwag.org)
// </auto-generated>
//----------------------

namespace MyNamespace
{
    #pragma warning disable // Disable all warnings

    [System.CodeDom.Compiler.GeneratedCode("NSwag",
        "11.17.3.0 (NJsonSchema v9.10.46.0 (Newtonsoft.Json v9.0.0.0))")]
    public partial class TodoClient
    {
        private string _baseUrl = "http://localhost:50499";
        private System.Lazy<Newtonsoft.Json.JsonSerializerSettings> _settings;

        public TodoClient()
        {
            _settings = new System.Lazy
                <Newtonsoft.Json.JsonSerializerSettings>(() =>
            {
                var settings = new Newtonsoft.Json.JsonSerializerSettings();
                UpdateJsonSerializerSettings(settings);
                return settings;
            });
        }

        public string BaseUrl
        {
            get { return _baseUrl; }
            set { _baseUrl = value; }
        }

        // code omitted for brevity
```

> [!TIP]
> <span data-ttu-id="f3de9-158">Der C#-Clientcode wird basierend auf den Einstellungen generiert, die über die Registerkarte **Einstellungen** der Registerkarte **CSharp-Client** definiert werden. Ändern Sie die Einstellungen, um Aufgaben wie das Umbenennen von Standardnamespaces und die Generierung von synchronen Methoden auszuführen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-158">The C# client code is generated based on settings defined in the **Settings** tab of the **CSharp Client** tab. Modify the settings to perform tasks such as default namespace renaming and synchronous method generation.</span></span>

* <span data-ttu-id="f3de9-159">Kopieren Sie den generierten C#_code in eine Datei in einem Clientprojekt (z.B. zu einer [Xamarin.Forms](/xamarin/xamarin-forms/)-App).</span><span class="sxs-lookup"><span data-stu-id="f3de9-159">Copy the generated C# code into a file in a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span>
* <span data-ttu-id="f3de9-160">Verwendung der Web-API:</span><span class="sxs-lookup"><span data-stu-id="f3de9-160">Start consuming the web API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all to-dos from the API
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem, and save it in the API
var createdTodo = await todoClient.CreateAsync(new TodoItem());

// Get a single to-do by ID
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="f3de9-161">Sie können eine Basis-URL und/oder einen HTTP-Client in den API-Client einfügen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-161">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="f3de9-162">Eine bewährte Methode besteht darin, [HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/) immer wieder zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="f3de9-162">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="f3de9-163">Weitere Möglichkeiten zum Generieren von Clientcode</span><span class="sxs-lookup"><span data-stu-id="f3de9-163">Other ways to generate client code</span></span>

<span data-ttu-id="f3de9-164">Sie können den Clientcode auf andere Arten generieren, die besser zu Ihrem Workflow passen:</span><span class="sxs-lookup"><span data-stu-id="f3de9-164">You can generate the client code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="f3de9-165">MSBuild</span><span class="sxs-lookup"><span data-stu-id="f3de9-165">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="f3de9-166">Im Code</span><span class="sxs-lookup"><span data-stu-id="f3de9-166">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="f3de9-167">T4-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="f3de9-167">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customize"></a><span data-ttu-id="f3de9-168">Anpassen</span><span class="sxs-lookup"><span data-stu-id="f3de9-168">Customize</span></span>

<span data-ttu-id="f3de9-169">Swagger umfasst Optionen zum Dokumentieren des Objektmodells, um die Verwendung der Web-API zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-169">Swagger provides options for documenting the object model to ease consumption of the web API.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="f3de9-170">API-Informationen und -Beschreibung</span><span class="sxs-lookup"><span data-stu-id="f3de9-170">API info and description</span></span>

<span data-ttu-id="f3de9-171">In der `Startup.Configure`-Methode werden Informationen wie Autor, Lizenz und Beschreibung durch die Konfigurationsaktion hinzugefügt, die an die `UseSwagger`-Methode übergeben wird:</span><span class="sxs-lookup"><span data-stu-id="f3de9-171">In the `Startup.Configure` method, a configuration action passed to the `UseSwagger` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Startup2.cs?name=snippet_UseSwagger)]

<span data-ttu-id="f3de9-172">Auf der Swagger-Benutzeroberfläche werden die Versionsinformationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="f3de9-172">The Swagger UI displays the version's information:</span></span>

![Swagger-Benutzeroberfläche mit Versionsinformationen](web-api-help-pages-using-swagger/_static/custom-info-nswag.png)

### <a name="xml-comments"></a><span data-ttu-id="f3de9-174">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="f3de9-174">XML comments</span></span>

<span data-ttu-id="f3de9-175">XML-Kommentare werden mithilfe von folgenden Ansätzen aktiviert:</span><span class="sxs-lookup"><span data-stu-id="f3de9-175">XML comments are enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="f3de9-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f3de9-176">Visual Studio</span></span>](#tab/visual-studio-xml/)

* <span data-ttu-id="f3de9-177">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="f3de9-177">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="f3de9-178">Überprüfen Sie das Feld **XML-Dokumentationsdatei** im Abschnitt **Ausgabe** der Registerkarte **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-178">Check the **XML documentation file** box under the **Output** section of the **Build** tab</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="f3de9-179">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="f3de9-179">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)

* <span data-ttu-id="f3de9-180">Öffnen Sie das Dialogfeld **Projektoptionen** > **Erstellen** > **Compiler**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-180">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="f3de9-181">Überprüfen Sie das Feld **XML-Dokumentation generieren** im Abschnitt **Allgemeine Optionen**.</span><span class="sxs-lookup"><span data-stu-id="f3de9-181">Check the **Generate xml documentation** box under the **General Options** section</span></span>

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="f3de9-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f3de9-182">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)

<span data-ttu-id="f3de9-183">Fügen Sie den folgenden Codeausschnitt manuell zu der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="f3de9-183">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/TodoApi.csproj?name=snippet_DocumentationFileElement)]

---

### <a name="data-annotations"></a><span data-ttu-id="f3de9-184">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="f3de9-184">Data annotations</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="f3de9-185">NSwag verwendet die [Reflektion](/dotnet/csharp/programming-guide/concepts/reflection), und der für Web-API-Aktionen empfohlene Rückgabetyp lautet [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="f3de9-185">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="f3de9-186">Folglich kann NSwag nicht ableiten, was die Aktion ausführt und was sie zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f3de9-186">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="f3de9-187">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f3de9-187">Consider the following example:</span></span>

<span data-ttu-id="f3de9-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="f3de9-188">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="f3de9-189">Die vorherige Aktion gibt `IActionResult` zurück. Innerhalb der Aktion gibt sie jedoch [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) oder [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) zurück.</span><span class="sxs-lookup"><span data-stu-id="f3de9-189">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="f3de9-190">Datenanmerkungen werden verwendet, um Clients mitzuteilen, welche HTTP-Statuscodes diese Aktion in der Regel zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f3de9-190">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="f3de9-191">Ergänzen Sie die Aktion mit folgenden Attributen:</span><span class="sxs-lookup"><span data-stu-id="f3de9-191">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="f3de9-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="f3de9-192">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="f3de9-193">NSwag verwendet [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), und der empfohlene Rückgabetyp für Web-API-Aktionen ist [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span><span class="sxs-lookup"><span data-stu-id="f3de9-193">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the recommended return type for web API actions is [ActionResult\<T>](/dotnet/api/microsoft.aspnetcore.mvc.actionresult-1).</span></span> <span data-ttu-id="f3de9-194">D.h., NSwag kann nur den von `T` definierten Rückgabetypen ableiten.</span><span class="sxs-lookup"><span data-stu-id="f3de9-194">Consequently, NSwag can only infer the return type defined by `T`.</span></span> <span data-ttu-id="f3de9-195">Es können keine weiteren Rückgabetypen in dieser Aktion abgeleitet werden.</span><span class="sxs-lookup"><span data-stu-id="f3de9-195">Other possible return types in the action cannot be inferred.</span></span> <span data-ttu-id="f3de9-196">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f3de9-196">Consider the following example:</span></span>

<span data-ttu-id="f3de9-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span><span class="sxs-lookup"><span data-stu-id="f3de9-197">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateAction)]</span></span>

<span data-ttu-id="f3de9-198">Die vorherige Aktion gibt `ActionResult<T>` zurück. Innerhalb der Aktion gibt sie jedoch [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) zurück.</span><span class="sxs-lookup"><span data-stu-id="f3de9-198">The preceding action returns `ActionResult<T>`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute).</span></span> <span data-ttu-id="f3de9-199">Da der Controller mit dem [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute)-Attribut ausgestattet ist, ist auch eine [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest)-Antwort möglich.</span><span class="sxs-lookup"><span data-stu-id="f3de9-199">Since the controller is decorated with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute, a [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) response is possible too.</span></span> <span data-ttu-id="f3de9-200">Weitere Informationen finden Sie unter [Automatic HTTP 400 responses (Automatische HTTP 400-Antworten)](xref:web-api/index#automatic-http-400-responses).</span><span class="sxs-lookup"><span data-stu-id="f3de9-200">See [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses) for more info.</span></span> <span data-ttu-id="f3de9-201">Datenanmerkungen werden verwendet, um Clients mitzuteilen, welche HTTP-Statuscodes diese Aktion in der Regel zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f3de9-201">Data annotations are used to tell clients which HTTP status codes this action is known to return.</span></span> <span data-ttu-id="f3de9-202">Ergänzen Sie die Aktion mit folgenden Attributen:</span><span class="sxs-lookup"><span data-stu-id="f3de9-202">Decorate the action with the following attributes:</span></span>

<span data-ttu-id="f3de9-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span><span class="sxs-lookup"><span data-stu-id="f3de9-203">[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.NSwag/Controllers/TodoController.cs?name=snippet_CreateActionAttributes)]</span></span>
::: moniker-end

<span data-ttu-id="f3de9-204">Der Swagger-Generator kann diese Aktion nun genau beschreiben, und generierte Clients wissen, was sie beim Aufrufen des Endpunkts empfangen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-204">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="f3de9-205">Es wird empfohlen, alle Aktionen mit diesen Attributen zu ergänzen.</span><span class="sxs-lookup"><span data-stu-id="f3de9-205">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="f3de9-206">Weitere Informationen zu den Richtlinien dazu, welche HTTP-Antworten Ihre API-Aktionen zurückgeben sollten, finden Sie in der [RFC 7231 specification (Spezifikation von RFC 7231)](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="f3de9-206">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
