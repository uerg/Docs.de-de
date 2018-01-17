---
title: ASP.NET Core-Web-API-Hilfeseiten mit Swagger
author: spboyer
description: "Dieses Tutorial enthält eine exemplarische Vorgehensweise für das Hinzufügen von Swagger, um Dokumentationen und Hilfeseiten für eine Web-API-Anwendung zu generieren."
keywords: ASP.NET Core, Swagger, Swashbuckle, Hilfeseiten, Web-API
ms.author: spboyer
manager: wpickett
ms.date: 09/01/2017
ms.topic: article
ms.assetid: 54bb961d-29d9-4dee-8e2c-a93fc33c16f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 630378510f4182034735cb4c306dfc5a761543ab
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/19/2017
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="1a4d2-104">ASP.NET Core-Web-API-Hilfeseiten mit Swagger</span><span class="sxs-lookup"><span data-stu-id="1a4d2-104">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="1a4d2-105">Von [Shayne Boyer](https://twitter.com/spboyer) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1a4d2-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1a4d2-106">Das Verstehen der verschiedenen Methoden einer API kann beim Erstellen einer verarbeitenden Anwendung eine Herausforderung für einen Entwickler sein.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-106">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="1a4d2-107">Das Generieren von guten Dokumentationen und Hilfeseiten für Ihre Web-API ist mithilfe von [Swagger](https://swagger.io/) mit der .NET Core-Implementierung [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) genauso leicht wie das Hinzufügen von NuGet-Paketen und das Ändern von *Startup.cs*.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-107">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="1a4d2-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ist ein Open Source-Projekt zum Generieren von Swagger-Dokumenten für Web-APIs von ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-108">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="1a4d2-109">[Swagger](https://swagger.io/) ist eine maschinenlesbare Darstellung einer RESTful-API, die Unterstützung für die interaktive Dokumentation, das Generieren von Client SDK und die Ermittelbarkeit ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-109">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="1a4d2-110">In diesem Tutorial wird das Beispiel von [Building Your First Web API with ASP.NET Core MVC and Visual Studio (Erstellen Ihrer ersten Web-API mit ASP.NET Core MVC und Visual Studio)](xref:tutorials/first-web-api) erstellt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-110">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="1a4d2-111">Für weitere Informationen laden Sie das Beispiel unter [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) herunter.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-111">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="1a4d2-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="1a4d2-112">Getting Started</span></span>

<span data-ttu-id="1a4d2-113">Es gibt drei Hauptkomponenten von Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-113">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="1a4d2-114">`Swashbuckle.AspNetCore.Swagger`: Ein Swagger-Objektmodell bzw. eine Swagger-Middleware, um `SwaggerDocument`-Objekte als JSON-Endpunkte verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-114">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="1a4d2-115">`Swashbuckle.AspNetCore.SwaggerGen`: Ein Swagger-Generator, der `SwaggerDocument`-Objekte direkt aus Ihren Routen, Controllern und Modellen erstellt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-115">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="1a4d2-116">Dieser wird üblicherweise mit der Middleware für den Swagger-Endpunkt kombiniert, um Swagger-JSONs automatisch verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-116">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="1a4d2-117">`Swashbuckle.AspNetCore.SwaggerUI`: Eine eingebettete Version des Swagger-UI-Tools, das Swagger-JSONs interpretiert, um die Web-API-Funktionen umfassend und anpassbar zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-117">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="1a4d2-118">Es enthält integrierte Testumgebungen für die öffentlichen Methoden.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-118">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="1a4d2-119">NuGet-Pakete</span><span class="sxs-lookup"><span data-stu-id="1a4d2-119">NuGet Packages</span></span>

<span data-ttu-id="1a4d2-120">Swashbuckle kann mit folgenden Vorgehensweisen hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-120">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a4d2-121">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a4d2-121">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a4d2-122">Aus dem Fenster **Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-122">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="1a4d2-123">Aus dem Dialogfeld **NuGet-Pakete verwalten**:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-123">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="1a4d2-124">Klicken Sie mit der rechten Maustaste unter **Projektmappen-Explorer** > **NuGet-Pakete verwalten** auf Ihr Projekt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-124">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="1a4d2-125">Legen Sie die **Paketquelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-125">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="1a4d2-126">Geben Sie „Swashbuckle.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-126">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="1a4d2-127">Wählen Sie das Paket „Swashbuckle.AspNetCore“ auf der Registerkarte **Durchsuchen** aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-127">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a4d2-128">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1a4d2-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1a4d2-129">Klicken Sie mit der rechten Maustaste auf den Ordner *Pakete* unter **Lösungspad** > **Pakete hinzufügen...**.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-129">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="1a4d2-130">Legen Sie im Fenster **Pakete hinzufügen** das Dropdownmenü **Quelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-130">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="1a4d2-131">Geben Sie „Swashbuckle.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-131">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="1a4d2-132">Wählen Sie das Paket „Swashbuckle.AspNetCore“ aus dem Ergebnisbereich aus, und klicken Sie auf **Paket hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-132">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a4d2-133">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a4d2-133">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1a4d2-134">Führen Sie folgenden Befehl aus dem **integrierten Terminal** aus:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-134">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a4d2-135">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="1a4d2-135">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a4d2-136">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-136">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="1a4d2-137">Hinzufügen und Konfigurieren von Swagger für die Middleware</span><span class="sxs-lookup"><span data-stu-id="1a4d2-137">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="1a4d2-138">Fügen Sie den Swagger-Generator zu der Services-Sammlung in der `ConfigureServices`-Methode von *Startup.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-138">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="1a4d2-139">Fügen Sie die folgende Using-Anweisung für die `Info`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-139">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="1a4d2-140">Aktivieren Sie die Middleware in der `Configure`-Methode von *Startup.cs*, um das generierte JSON-Dokument und die Swagger-Benutzeroberfläche zu verarbeiten:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-140">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="1a4d2-141">Starten Sie die App, und navigieren Sie zu `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-141">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="1a4d2-142">Das generierte Dokument, das den Endpunkt beschreibt, wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-142">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="1a4d2-143">**Hinweis:** Microsoft Edge, Google Chrome und Firefox zeigen JSON-Dokumente nativ an.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-143">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="1a4d2-144">Es gibt Erweiterungen für Chrome, die das Dokument für eine verbesserte Lesbarkeit formatieren.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-144">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="1a4d2-145">*Das folgende Beispiel wird aus Gründen der Übersichtlichkeit reduziert.*</span><span class="sxs-lookup"><span data-stu-id="1a4d2-145">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
           "type": "object",
            "properties": {
                "id": {
                    "format": "int64",
                    "type": "integer"
                },
                "name": {
                    "type": "string"
                },
                "isComplete": {
                    "default": false,
                    "type": "boolean"
                }
            }
       }
   },
   "securityDefinitions": {}
}
```

<span data-ttu-id="1a4d2-146">Dieses Dokument steuert die Swagger-Benutzeroberfläche, die durch Navigieren zu `http://localhost:<random_port>/swagger` angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-146">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Swagger-Benutzeroberfläche](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="1a4d2-148">Jede öffentliche Aktionsmethode in `TodoController` kann über die Benutzeroberfläche getestet werden.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-148">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="1a4d2-149">Klicken Sie auf einen Methodennamen, um den Abschnitt zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-149">Click a method name to expand the section.</span></span> <span data-ttu-id="1a4d2-150">Fügen Sie die erforderlichen Parameter hinzu, und klicken Sie auf „Probieren Sie es aus!“.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-150">Add any necessary parameters, and click "Try it out!".</span></span>

![Beispiel für einen Swagger-GET-Test](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="1a4d2-152">Anpassung und Erweiterbarkeit</span><span class="sxs-lookup"><span data-stu-id="1a4d2-152">Customization & Extensibility</span></span>

<span data-ttu-id="1a4d2-153">Swagger stellt Optionen für das Dokumentieren des Objektmodells und das Anpassen der Benutzeroberfläche bereit, damit diese mit Ihrem Design übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-153">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="1a4d2-154">API-Informationen und -Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1a4d2-154">API Info and Description</span></span>

<span data-ttu-id="1a4d2-155">Die Konfigurationsaktion, die an die `AddSwaggerGen`-Methode übergeben wurde, kann zum Hinzufügen von Informationen, z.B. Autor, Lizenz und Beschreibung, verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-155">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="1a4d2-156">Die folgende Abbildung zeigt die Swagger-Benutzeroberfläche, die die Versionsinformationen anzeigt:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-156">The following image depicts the Swagger UI displaying the version information:</span></span>

![Swagger-Benutzeroberfläche mit Versionsinformationen: Beschreibung, Autor, Link „Mehr anzeigen“](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="1a4d2-158">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="1a4d2-158">XML Comments</span></span>

<span data-ttu-id="1a4d2-159">XML-Kommentare können mithilfe der folgenden Ansätze aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-159">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1a4d2-160">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1a4d2-160">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1a4d2-161">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-161">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="1a4d2-162">Überprüfen Sie das Feld **XML-Dokumentationsdatei** unter dem Abschnitt **Ausgabe** der Registerkarte **Erstellen**:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-162">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Projekteigenschaften auf der Registerkarte „Erstellen“](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1a4d2-164">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1a4d2-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1a4d2-165">Öffnen Sie das Dialogfeld **Projektoptionen** > **Erstellen** > **Compiler**.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-165">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="1a4d2-166">Überprüfen Sie das Feld **XML-Dokumentation generieren** im Abschnitt **Allgemeine Optionen**:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-166">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Abschnitt „Allgemeine Optionen“ der Projektoptionen](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1a4d2-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1a4d2-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1a4d2-169">Fügen Sie den folgenden Codeausschnitt manuell zu der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-169">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1a4d2-170">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="1a4d2-170">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1a4d2-171">Zeigen Sie Visual Studio Code an.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-171">See Visual Studio Code.</span></span>

---

<span data-ttu-id="1a4d2-172">Konfigurieren Sie Swagger, um die generierte XML-Datei verwenden.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-172">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="1a4d2-173">Bei Linux oder anderen Betriebssystemen als Windows kann bei Dateinamen und -pfaden die Groß- und Kleinschreibung berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-173">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="1a4d2-174">Die Datei *ToDoApi.XML* könnte beispielsweise unter Windows gefunden werden, nicht aber unter CentOS.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-174">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="1a4d2-175">Im vorangehenden Code ruft `ApplicationBasePath` den Basispfad der App ab.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-175">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="1a4d2-176">Der Basispfad wird verwendet, um die XML-Kommentardatei zu suchen.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-176">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="1a4d2-177">*TodoApi.xml* funktioniert nur für dieses Beispiel, da der Name für die generierte XML-Kommentardatei auf dem Anwendungsnamen basiert.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-177">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="1a4d2-178">Das Hinzufügen von Kommentaren mit drei Schrägstrichen zur Methode verbessert die Swagger-Benutzeroberfläche, indem die Beschreibung zum Header des Abschnitts hinzugefügt wird:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-178">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Swagger-Benutzeroberfläche mit dem XML-Kommentar „Löscht ein bestimmtes TodoItem.“](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="1a4d2-181">Die Benutzeroberfläche wird von der generierten JSON-Datei gesteuert, die auch diese Kommentare enthält:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-181">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="1a4d2-182">Fügen Sie einen [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks)-Tag zu der Dokumentation der `Create`-Aktionsmethode hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-182">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="1a4d2-183">Dieser ergänzt die Informationen, die im `<summary>`-Tag angegeben wurden und bietet eine robustere Swagger-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-183">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="1a4d2-184">Der Inhalt des `<remarks>`-Tags kann aus Text, JSON oder XML bestehen.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-184">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="1a4d2-185">Beachten Sie die Verbesserungen in der Benutzeroberfläche durch diese zusätzlichen Kommentare.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-185">Notice the UI enhancements with these additional comments.</span></span>

![Swagger-Benutzeroberfläche mit zusätzlichen Kommentaren](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="1a4d2-187">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="1a4d2-187">Data Annotations</span></span>

<span data-ttu-id="1a4d2-188">Ergänzen Sie das Modell mit Attributen, die in `System.ComponentModel.DataAnnotations` gefunden werden können, um das Steuern der Swagger-Benutzeroberflächenkomponenten zu vereinfachen.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-188">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="1a4d2-189">Fügen Sie das `[Required]`-Attribut der `Name`-Eigenschaft der `TodoItem`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-189">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="1a4d2-190">Das Vorhandensein dieses Attributs ändert das Verhalten der Benutzeroberfläche und des zugrunde liegenden JSON-Schemas:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-190">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="1a4d2-191">Fügen Sie das `[Produces("application/json")]`-Attribut zum API-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-191">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="1a4d2-192">Dadurch kann deklariert werden, dass die Aktionen des Controllers die Rückgabe eines Inhaltstypen von *application/json* unterstützen:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-192">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="1a4d2-193">Im Dropdownmenü des **Anforderungsinhaltstyps** ist dieser Inhaltstyp als Standard für die GET-Aktionen des Controllers ausgewählt:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-193">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger UI mit Standardinhaltstyp für die Antwort](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="1a4d2-195">Mit zunehmender Verwendung von Datenanmerkungen in der Web-API werden die UI- und API-Hilfeseiten beschreibender und nützlicher.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-195">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="1a4d2-196">Beschreiben von Antworttypen</span><span class="sxs-lookup"><span data-stu-id="1a4d2-196">Describing Response Types</span></span>

<span data-ttu-id="1a4d2-197">Für verarbeitende Entwickler ist es am wichtigsten, was zurückgegeben wird &mdash; besonders Antworttypen und Fehlercodes (wenn diese nicht dem Standard entsprechen).</span><span class="sxs-lookup"><span data-stu-id="1a4d2-197">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="1a4d2-198">Diese werden in XML-Kommentaren und Datenanmerkungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-198">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="1a4d2-199">Die `Create`-Aktion gibt bei Erfolg `201 Created` zurück oder `400 Bad Request`, wenn der bereitgestellte Anforderungstext NULL ist.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-199">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="1a4d2-200">Ohne richtige Dokumentation in der Swagger-Benutzeroberfläche fehlt dem Consumer das Wissen über diese erwarteten Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-200">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="1a4d2-201">Das Problem kann behoben werden, indem die hervorgehobenen Zeilen im folgenden Beispiel hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-201">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="1a4d2-202">Die Swagger-Benutzeroberfläche dokumentiert nun deutlich die erwarteten HTTP-Antwortcodes:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-202">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger-Benutzeroberfläche, die die Klassenbeschreibung „Gibt das neu erstellte Todo-Element zurück“ nach der Antwort anzeigt sowie „400“ für den Statuscode und „Wenn das Element NULL ist“ als Grund unter den Antwortnachrichten](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="1a4d2-204">Anpassen der Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="1a4d2-204">Customizing the UI</span></span>

<span data-ttu-id="1a4d2-205">Die Stock UI ist funktionsfähig und vorzeigbar, wenn Sie jedoch Dokumentationsseiten für Ihre API erstellen, möchten Sie eventuell, dass diese Ihre Marke oder Ihr Design darstellt.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-205">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="1a4d2-206">Das Ausführen dieser Aufgabe mit Swashbuckle-Komponenten erfordert das Hinzufügen der Ressourcen, um statische Dateien zu verarbeiten und dann die Ordnerstruktur zu erstellen, um diese Dateien zu hosten.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-206">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="1a4d2-207">Wenn Sie Anwendungen für .NET Framework entwickeln, fügen Sie das NuGet-Paket `Microsoft.AspNetCore.StaticFiles` zum Projekt hinzu:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-207">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="1a4d2-208">Aktivieren Sie die Middleware für statische Dateien:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-208">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="1a4d2-209">Rufen Sie die Inhalte des Ordners *dist* aus dem [GitHub-Repository für die Swagger-Benutzeroberfläche](https://github.com/swagger-api/swagger-ui/tree/2.x/dist) ab.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-209">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="1a4d2-210">Dieser Ordner enthält die erforderlichen Ressourcen für die Seite der Swagger-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-210">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="1a4d2-211">Erstellen Sie einen *wwwroot/swagger/ui*-Ordner, und kopieren Sie die Inhalte des Ordners *dist* in ihn.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-211">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="1a4d2-212">Erstellen Sie die Datei *wwwroot/swagger/ui/css/custom.css* mit dem folgenden CSS, um den Seitenheader anzupassen:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-212">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="1a4d2-213">Verweisen Sie in der Datei *index.html* auf *custom.css*:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-213">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="1a4d2-214">Navigieren Sie zur Seite *index.html* auf `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-214">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="1a4d2-215">Geben Sie `http://localhost:<random_port>/swagger/v1/swagger.json` in das Textfeld des Headers ein, und klicken Sie dann auf die Schaltfläche **Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-215">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="1a4d2-216">Die Ergebnisseite sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="1a4d2-216">The resulting page looks as follows:</span></span>

![Swagger-Benutzeroberfläche mit benutzerdefiniertem Headertitel](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="1a4d2-218">Es gibt noch viele weitere Verwendungsmöglichkeiten für diese Seite.</span><span class="sxs-lookup"><span data-stu-id="1a4d2-218">There is much more you can do with the page.</span></span> <span data-ttu-id="1a4d2-219">Weitere Informationen zum vollen Funktionsumgang der UI-Ressourcen finden Sie im [GitHub-Repository für die Swagger-Benutzeroberfläche](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="1a4d2-219">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
