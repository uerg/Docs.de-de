---
title: Erste Schritte mit NSwag und ASP.NET Core
author: zuckerthoben
description: Erfahren Sie, wie Sie NSwag zum Generieren von Dokumentationen und Hilfeseiten für eine ASP.NET Core-Web-API-App verwenden können.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a><span data-ttu-id="ef07b-103">Erste Schritte mit NSwag und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef07b-103">Get started with NSwag and ASP.NET Core</span></span>

<span data-ttu-id="ef07b-104">Von [Christoph Nienaber](https://twitter.com/zuckerthoben) und [Rico Suter](https://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="ef07b-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://rsuter.com)</span></span>

<span data-ttu-id="ef07b-105">Die Verwendung von [NSwag](https://github.com/RSuter/NSwag) mit ASP.NET Core-Middleware erfordert das NuGet-Paket [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/).</span><span class="sxs-lookup"><span data-stu-id="ef07b-105">Using [NSwag](https://github.com/RSuter/NSwag) with ASP.NET Core middleware requires the [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/) NuGet package.</span></span> <span data-ttu-id="ef07b-106">Das Paket besteht aus einem Swagger-Generator, der Swagger-Benutzeroberfläche (Version 2 und 3) und der [ReDoc-Benutzeroberfläche](https://github.com/Rebilly/ReDoc).</span><span class="sxs-lookup"><span data-stu-id="ef07b-106">The package consists of a Swagger generator, Swagger UI (v2 and v3), and [ReDoc UI](https://github.com/Rebilly/ReDoc).</span></span>

<span data-ttu-id="ef07b-107">Es wird empfohlen, die Funktionen zur Codegenerierung von NSwag zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ef07b-107">It's highly recommended to make use of NSwag's code generation capabilities.</span></span> <span data-ttu-id="ef07b-108">Wählen Sie eine der folgenden Optionen für die Codegenerierung aus:</span><span class="sxs-lookup"><span data-stu-id="ef07b-108">Choose one of the following options for code generation:</span></span>

* <span data-ttu-id="ef07b-109">Verwenden Sie [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio). Dies ist eine Windows-Desktop-App für das Generieren von Clientcode in C# und TypeScript für Ihre API.</span><span class="sxs-lookup"><span data-stu-id="ef07b-109">Use [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio), a Windows desktop app for generating client code in C# and TypeScript for your API</span></span>
* <span data-ttu-id="ef07b-110">Verwenden Sie das NuGet-Paket [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) oder [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), um Code innerhalb des Projekts zu generieren.</span><span class="sxs-lookup"><span data-stu-id="ef07b-110">Use the [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) or [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/) NuGet packages to do code generation inside your project</span></span>
* <span data-ttu-id="ef07b-111">Verwenden Sie NSwag über die [Befehlszeile](https://github.com/NSwag/NSwag/wiki/CommandLine).</span><span class="sxs-lookup"><span data-stu-id="ef07b-111">Use NSwag from the [command line](https://github.com/NSwag/NSwag/wiki/CommandLine)</span></span>
* <span data-ttu-id="ef07b-112">Verwenden Sie das NuGet-Paket [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).</span><span class="sxs-lookup"><span data-stu-id="ef07b-112">Use the [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild) NuGet package</span></span>

## <a name="features"></a><span data-ttu-id="ef07b-113">Features</span><span class="sxs-lookup"><span data-stu-id="ef07b-113">Features</span></span>

<span data-ttu-id="ef07b-114">Der Hauptgrund für die Verwendung von NSwag liegt darin, dass nicht nur die Swagger-Benutzeroberfläche und der Swagger-Generator, sondern auch die flexiblen Funktionen für die Codegenerierung verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="ef07b-114">The main reason to use NSwag is the ability to not only introduce the Swagger UI and Swagger generator, but to make use of the flexible code generation capabilities.</span></span> <span data-ttu-id="ef07b-115">Sie müssen keine API besitzen, sondern können APIs von Drittanbietern verwenden, die Swagger integrieren, und NSwag eine Clientimplementierung generieren lassen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-115">You don't need an existing API&mdash;you can use third-party APIs that incorporate Swagger and let NSwag generate a client implementation.</span></span> <span data-ttu-id="ef07b-116">In jedem Fall wird der Entwicklungszyklus beschleunigt und Sie können sich schnell an API-Änderungen anpassen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-116">Either way, the development cycle is expedited and you can more easily adapt to API changes.</span></span>

## <a name="package-installation"></a><span data-ttu-id="ef07b-117">Paketinstallation</span><span class="sxs-lookup"><span data-stu-id="ef07b-117">Package installation</span></span>

<span data-ttu-id="ef07b-118">Das NuGet-Paket „NSwag“ kann folgendermaßen hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="ef07b-118">The NSwag NuGet package can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ef07b-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef07b-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ef07b-120">Aus dem Fenster **Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="ef07b-120">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* <span data-ttu-id="ef07b-121">Aus dem Dialogfeld **NuGet-Pakete verwalten**:</span><span class="sxs-lookup"><span data-stu-id="ef07b-121">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="ef07b-122">Klicken Sie mit der rechten Maustaste unter **Projektmappen-Explorer** > **NuGet-Pakete verwalten** auf Ihr Projekt.</span><span class="sxs-lookup"><span data-stu-id="ef07b-122">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="ef07b-123">Legen Sie die **Paketquelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="ef07b-123">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="ef07b-124">Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="ef07b-124">Enter "NSwag.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="ef07b-125">Wählen Sie das Paket „NSwag.AspNetCore“ auf der Registerkarte **Durchsuchen** aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="ef07b-125">Select the "NSwag.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ef07b-126">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="ef07b-126">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ef07b-127">Klicken Sie mit der rechten Maustaste auf den Ordner *Pakete* unter **Lösungspad** > **Pakete hinzufügen...**.</span><span class="sxs-lookup"><span data-stu-id="ef07b-127">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="ef07b-128">Legen Sie im Fenster **Pakete hinzufügen** das Dropdownmenü **Quelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="ef07b-128">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="ef07b-129">Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="ef07b-129">Enter NSwag.AspNetCore in the search box</span></span>
* <span data-ttu-id="ef07b-130">Wählen Sie das Paket „NSwag.AspNetCore“ aus dem Ergebnisbereich aus, und klicken Sie auf **Paket hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="ef07b-130">Select the NSwag.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ef07b-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef07b-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ef07b-132">Führen Sie folgenden Befehl aus dem **integrierten Terminal** aus:</span><span class="sxs-lookup"><span data-stu-id="ef07b-132">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ef07b-133">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="ef07b-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ef07b-134">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="ef07b-134">Run the following command:</span></span>

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="ef07b-135">Hinzufügen und Konfigurieren von Swagger-Middleware</span><span class="sxs-lookup"><span data-stu-id="ef07b-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="ef07b-136">Importieren Sie folgende Namespaces in die `Info`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="ef07b-136">Import the following namespaces in the `Info` class:</span></span>

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

<span data-ttu-id="ef07b-137">Aktivieren Sie die Middleware in der `Startup.Configure`-Methode, um die generierte Swagger-Spezifikation und die Swagger-Benutzeroberfläche zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="ef07b-137">In the `Startup.Configure` method, enable the middleware for serving the generated Swagger specification and the Swagger UI:</span></span>

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="ef07b-138">Starten Sie die App.</span><span class="sxs-lookup"><span data-stu-id="ef07b-138">Launch the app.</span></span> <span data-ttu-id="ef07b-139">Navigieren Sie zu `/swagger`, um die Swagger-Benutzeroberfläche anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-139">Navigate to `/swagger` to view the Swagger UI.</span></span> <span data-ttu-id="ef07b-140">Navigieren Sie zu `/swagger/v1/swagger.json`, um die Swagger-Spezifikation anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-140">Navigate to `/swagger/v1/swagger.json` to view the Swagger specification.</span></span>

## <a name="code-generation"></a><span data-ttu-id="ef07b-141">Codeerzeugung</span><span class="sxs-lookup"><span data-stu-id="ef07b-141">Code generation</span></span>

### <a name="via-nswagstudio"></a><span data-ttu-id="ef07b-142">Über NSwagStudio</span><span class="sxs-lookup"><span data-stu-id="ef07b-142">Via NSwagStudio</span></span>

* <span data-ttu-id="ef07b-143">Installieren Sie `NSwagStudio` über das offizielle [GitHub-Repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span><span class="sxs-lookup"><span data-stu-id="ef07b-143">Install `NSwagStudio` from the official [GitHub repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).</span></span>

* <span data-ttu-id="ef07b-144">Starten Sie NSwagStudio.</span><span class="sxs-lookup"><span data-stu-id="ef07b-144">Launch NSwagStudio.</span></span> <span data-ttu-id="ef07b-145">Geben Sie den Speicherort von *swagger.json* ein, oder kopieren Sie diesen direkt:</span><span class="sxs-lookup"><span data-stu-id="ef07b-145">Enter the location of your *swagger.json* or copy it directly:</span></span>

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* <span data-ttu-id="ef07b-147">Geben Sie den gewünschten Ausgabetyp für den Client an.</span><span class="sxs-lookup"><span data-stu-id="ef07b-147">Indicate the desired client output type.</span></span> <span data-ttu-id="ef07b-148">Zu den Optionen zählen **TypeScript-Client**, **CSharp-Client**, und **CSharp-Web-API-Controller**.</span><span class="sxs-lookup"><span data-stu-id="ef07b-148">Options include **TypeScript Client**, **CSharp Client**, or **CSharp Web API Controller**.</span></span> <span data-ttu-id="ef07b-149">Bei der Verwendung eines Web-API-Controllers handelt es sich im Grunde um eine umgekehrte Generierung.</span><span class="sxs-lookup"><span data-stu-id="ef07b-149">Using a Web API Controller is basically a reverse generation.</span></span> <span data-ttu-id="ef07b-150">Dieser verwendet die Spezifikation eines Diensts, um den Dienst erneut zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-150">It uses a specification of a service to rebuild the service.</span></span>

* <span data-ttu-id="ef07b-151">Klicken Sie auf **Generate Outputs** (Ausgaben generieren).</span><span class="sxs-lookup"><span data-stu-id="ef07b-151">Click **Generate Outputs**.</span></span>

* <span data-ttu-id="ef07b-152">Hier finden Sie eine vollständige Clientimplementierung des *TodoApi.NSwag*-Beispiels in C#:</span><span class="sxs-lookup"><span data-stu-id="ef07b-152">Here you see a complete client implementation of the *TodoApi.NSwag* sample in C#:</span></span>

![Ausgabe von NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* <span data-ttu-id="ef07b-154">Fügen Sie die Datei zu einem Clientprojekt (z.B. zu einer [Xamarin.Forms](/xamarin/xamarin-forms/)-App) hinzu.</span><span class="sxs-lookup"><span data-stu-id="ef07b-154">Put the file into a client project (for example, a [Xamarin.Forms](/xamarin/xamarin-forms/) app).</span></span> <span data-ttu-id="ef07b-155">Verwendung der API:</span><span class="sxs-lookup"><span data-stu-id="ef07b-155">Start consuming the API:</span></span>

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> <span data-ttu-id="ef07b-156">Sie können eine Basis-URL und/oder einen HTTP-Client in den API-Client einfügen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-156">You can inject a base URL and/or a HTTP client into the API client.</span></span> <span data-ttu-id="ef07b-157">Eine bewährte Methode besteht darin, [HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/) immer wieder zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ef07b-157">The best practice is to always [reuse the HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/).</span></span>

<span data-ttu-id="ef07b-158">Sie können Ihre API nun einfach in Clientprojekte implementieren.</span><span class="sxs-lookup"><span data-stu-id="ef07b-158">You can now start implementing your API into client projects easily.</span></span>

### <a name="other-ways-to-generate-client-code"></a><span data-ttu-id="ef07b-159">Weitere Möglichkeiten zum Generieren von Clientcode</span><span class="sxs-lookup"><span data-stu-id="ef07b-159">Other ways to generate client code</span></span>

<span data-ttu-id="ef07b-160">Sie können den Code auf andere Arten generieren, die besser zu Ihrem Workflow passen:</span><span class="sxs-lookup"><span data-stu-id="ef07b-160">You can generate the code in other ways, more suited to your workflow:</span></span>

* [<span data-ttu-id="ef07b-161">MSBuild</span><span class="sxs-lookup"><span data-stu-id="ef07b-161">MSBuild</span></span>](https://www.nuget.org/packages/NSwag.MSBuild/)

* [<span data-ttu-id="ef07b-162">Im Code</span><span class="sxs-lookup"><span data-stu-id="ef07b-162">In code</span></span>](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [<span data-ttu-id="ef07b-163">T4-Vorlagen</span><span class="sxs-lookup"><span data-stu-id="ef07b-163">T4 templates</span></span>](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a><span data-ttu-id="ef07b-164">Anpassung</span><span class="sxs-lookup"><span data-stu-id="ef07b-164">Customization</span></span>

### <a name="xml-comments"></a><span data-ttu-id="ef07b-165">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="ef07b-165">XML comments</span></span>

<span data-ttu-id="ef07b-166">XML-Kommentare werden mithilfe von folgenden Ansätzen aktiviert:</span><span class="sxs-lookup"><span data-stu-id="ef07b-166">XML comments are enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="ef07b-167">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef07b-167">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="ef07b-168">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="ef07b-168">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="ef07b-169">Überprüfen Sie das Feld **XML-Dokumentationsdatei** unter dem Abschnitt **Ausgabe** der Registerkarte **Erstellen**:</span><span class="sxs-lookup"><span data-stu-id="ef07b-169">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Projekteigenschaften auf der Registerkarte „Erstellen“](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="ef07b-171">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="ef07b-171">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="ef07b-172">Öffnen Sie das Dialogfeld **Projektoptionen** > **Erstellen** > **Compiler**.</span><span class="sxs-lookup"><span data-stu-id="ef07b-172">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="ef07b-173">Überprüfen Sie das Feld **XML-Dokumentation generieren** im Abschnitt **Allgemeine Optionen**:</span><span class="sxs-lookup"><span data-stu-id="ef07b-173">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Abschnitt „Allgemeine Optionen“ der Projektoptionen](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="ef07b-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ef07b-175">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="ef07b-176">Fügen Sie den folgenden Codeausschnitt manuell zu der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="ef07b-176">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a><span data-ttu-id="ef07b-177">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="ef07b-177">Data annotations</span></span>

<span data-ttu-id="ef07b-178">NSwag verwendet die [Reflektion](/dotnet/csharp/programming-guide/concepts/reflection), und eine bewährte Methode für Web-API-Aktionen besteht in der Rückgabe von [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span><span class="sxs-lookup"><span data-stu-id="ef07b-178">NSwag uses [Reflection](/dotnet/csharp/programming-guide/concepts/reflection), and the best practice for Web API actions is to return [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult).</span></span> <span data-ttu-id="ef07b-179">Folglich kann NSwag nicht ableiten, was die Aktion ausführt und was sie zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ef07b-179">Consequently, NSwag can't infer what your action is doing and what it returns.</span></span> <span data-ttu-id="ef07b-180">Betrachten Sie das folgende Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ef07b-180">Consider the following example:</span></span>

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

<span data-ttu-id="ef07b-181">Die vorherige Aktion gibt `IActionResult` zurück. Innerhalb der Aktion gibt sie jedoch [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) oder [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) zurück.</span><span class="sxs-lookup"><span data-stu-id="ef07b-181">The preceding action returns `IActionResult`, but inside the action it's returning either [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) or [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest).</span></span> <span data-ttu-id="ef07b-182">Datenanmerkungen werden verwendet, um Clients mitzuteilen, welche HTTP-Antwort die Aktion zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="ef07b-182">Data annotations are used to tell clients which HTTP response this action is returning.</span></span> <span data-ttu-id="ef07b-183">Ergänzen Sie die Aktion mit folgenden Attributen:</span><span class="sxs-lookup"><span data-stu-id="ef07b-183">Decorate the action with the following attributes:</span></span>

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

<span data-ttu-id="ef07b-184">Der Swagger-Generator kann diese Aktion nun genau beschreiben, und generierte Clients wissen, was sie beim Aufrufen des Endpunkts empfangen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-184">The Swagger generator can now accurately describe this action, and generated clients know what they receive when calling the endpoint.</span></span> <span data-ttu-id="ef07b-185">Es wird empfohlen, alle Aktionen mit diesen Attributen zu ergänzen.</span><span class="sxs-lookup"><span data-stu-id="ef07b-185">Decorating all actions with these attributes is highly recommended.</span></span> <span data-ttu-id="ef07b-186">Weitere Informationen zu den Richtlinien dazu, welche HTTP-Antworten Ihre API-Aktionen zurückgeben sollten, finden Sie in der [RFC 7231 specification (Spezifikation von RFC 7231)](https://tools.ietf.org/html/rfc7231#section-4.3).</span><span class="sxs-lookup"><span data-stu-id="ef07b-186">For guidelines on what HTTP responses your API actions should return, see the [RFC 7231 specification](https://tools.ietf.org/html/rfc7231#section-4.3).</span></span>
