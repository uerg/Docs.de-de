---
title: Erste Schritte mit Swashbuckle und ASP.NET Core
author: zuckerthoben
description: Erfahren Sie, wie Sie Ihren ASP.NET Core-Web-API-Projekten Swashbuckle hinzufügen, um die Swagger-Benutzeroberfläche zu integrieren.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/05/2018
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 945a2ebe138ba6a1f6029f9e867887b1ce8d628f
ms.sourcegitcommit: 09affee3d234cb27ea6fe33bc113b79e68900d22
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2018
ms.locfileid: "51191281"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="1059a-103">Erste Schritte mit Swashbuckle und ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1059a-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="1059a-104">Von [Shayne Boyer](https://twitter.com/spboyer) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1059a-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1059a-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1059a-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1059a-106">Es gibt drei Hauptkomponenten von Swashbuckle:</span><span class="sxs-lookup"><span data-stu-id="1059a-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="1059a-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): ein Swagger-Objektmodell und eine Swagger-Middleware, um `SwaggerDocument`-Objekte als JSON-Endpunkte verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="1059a-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="1059a-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): ein Swagger-Generator, der `SwaggerDocument`-Objekte direkt aus Routen, Controllern und Modellen erstellt.</span><span class="sxs-lookup"><span data-stu-id="1059a-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="1059a-109">Dieser wird üblicherweise mit der Middleware für den Swagger-Endpunkt kombiniert, um Swagger-JSONs automatisch verfügbar zu machen.</span><span class="sxs-lookup"><span data-stu-id="1059a-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="1059a-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): eine eingebettete Version des Swagger UI-Tools.</span><span class="sxs-lookup"><span data-stu-id="1059a-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="1059a-111">Diese interpretiert JSON-Daten von Swagger zur Erstellung einer umfassenden und anpassbaren Benutzeroberfläche, auf der Web-API-Funktionen beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="1059a-111">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="1059a-112">Es enthält integrierte Testumgebungen für die öffentlichen Methoden.</span><span class="sxs-lookup"><span data-stu-id="1059a-112">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="1059a-113">Paketinstallation</span><span class="sxs-lookup"><span data-stu-id="1059a-113">Package installation</span></span>

<span data-ttu-id="1059a-114">Swashbuckle kann mit folgenden Vorgehensweisen hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="1059a-114">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1059a-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1059a-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="1059a-116">Aus dem Fenster **Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="1059a-116">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="1059a-117">Navigieren Sie zu **Ansicht** > **Weitere Fenster** > **Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="1059a-117">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="1059a-118">Navigieren Sie zu dem Verzeichnis, in dem die *TodoApi.csproj*-Datei gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="1059a-118">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="1059a-119">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1059a-119">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="1059a-120">Aus dem Dialogfeld **NuGet-Pakete verwalten**:</span><span class="sxs-lookup"><span data-stu-id="1059a-120">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="1059a-121">Klicken Sie mit der rechten Maustaste unter **Projektmappen-Explorer** > **NuGet-Pakete verwalten** auf Ihr Projekt.</span><span class="sxs-lookup"><span data-stu-id="1059a-121">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="1059a-122">Legen Sie die **Paketquelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="1059a-122">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="1059a-123">Geben Sie „Swashbuckle.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="1059a-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="1059a-124">Wählen Sie das Paket „Swashbuckle.AspNetCore“ auf der Registerkarte **Durchsuchen** aus, und klicken Sie auf **Installieren**.</span><span class="sxs-lookup"><span data-stu-id="1059a-124">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1059a-125">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1059a-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="1059a-126">Klicken Sie mit der rechten Maustaste auf den Ordner *Pakete* unter **Lösungspad** > **Pakete hinzufügen...**.</span><span class="sxs-lookup"><span data-stu-id="1059a-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="1059a-127">Legen Sie im Fenster **Pakete hinzufügen** das Dropdownmenü **Quelle** auf „nuget.org“ fest.</span><span class="sxs-lookup"><span data-stu-id="1059a-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="1059a-128">Geben Sie „Swashbuckle.AspNetCore“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="1059a-128">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="1059a-129">Wählen Sie das Paket „Swashbuckle.AspNetCore“ aus dem Ergebnisbereich aus, und klicken Sie auf **Paket hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="1059a-129">Select the "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1059a-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1059a-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1059a-131">Führen Sie folgenden Befehl aus dem **integrierten Terminal** aus:</span><span class="sxs-lookup"><span data-stu-id="1059a-131">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1059a-132">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="1059a-132">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1059a-133">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1059a-133">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="1059a-134">Hinzufügen und Konfigurieren von Swagger-Middleware</span><span class="sxs-lookup"><span data-stu-id="1059a-134">Add and configure Swagger middleware</span></span>

<span data-ttu-id="1059a-135">Fügen Sie den Swagger-Generator zu der services-Sammlung in der `Startup.ConfigureServices`-Methode hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-135">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9-12)]

::: moniker-end

<span data-ttu-id="1059a-136">Importieren Sie den folgenden Namespace zur Verwendung der `Info`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="1059a-136">Import the following namespace to use the `Info` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="1059a-137">Aktivieren Sie die Middleware in der `Startup.Configure`-Methode, um das generierte JSON-Dokument und die Swagger-Benutzeroberfläche bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="1059a-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

<span data-ttu-id="1059a-138">Der obige `UseSwaggerUI`-Methodenaufruf aktiviert die [Middleware für statische Dateien](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="1059a-138">The preceding `UseSwaggerUI` method call enables the [Static Files Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="1059a-139">Wenn .NET Framework oder .NET Core 1.x die Zielkomponente ist, müssen Sie Ihrem Projekt das NuGet-Paket [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1059a-139">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="1059a-140">Starten Sie die App, und navigieren Sie zu `http://localhost:<port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="1059a-140">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="1059a-141">Das generierte Dokument mit der Beschreibung der Endpunkte wird entsprechend der [Swagger-Spezifikation (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1059a-141">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="1059a-142">Die Swagger-Benutzeroberfläche ist unter `http://localhost:<port>/swagger` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="1059a-142">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="1059a-143">Mit der Swagger-Benutzeroberfläche können Sie die API kennenlernen und sie in andere Programme integrieren.</span><span class="sxs-lookup"><span data-stu-id="1059a-143">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="1059a-144">Legen Sie für die Eigenschaft `RoutePrefix` eine leere Zeichenfolge fest, um die Swagger-Benutzeroberfläche im App-Stamm (`http://localhost:<port>/`) bereitzustellen:</span><span class="sxs-lookup"><span data-stu-id="1059a-144">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize-and-extend"></a><span data-ttu-id="1059a-145">Anpassen und Erweitern</span><span class="sxs-lookup"><span data-stu-id="1059a-145">Customize and extend</span></span>

<span data-ttu-id="1059a-146">Swagger stellt Optionen für das Dokumentieren des Objektmodells und das Anpassen der Benutzeroberfläche bereit, damit diese mit Ihrem Design übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="1059a-146">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="1059a-147">API-Informationen und -Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1059a-147">API info and description</span></span>

<span data-ttu-id="1059a-148">Durch die Konfigurationsaktion, die an die `AddSwaggerGen`-Methode übergeben wird, werden Informationen wie Autor, Lizenz und Beschreibung hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="1059a-148">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="1059a-149">Auf der Swagger-Benutzeroberfläche werden die Versionsinformationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1059a-149">The Swagger UI displays the version's information:</span></span>

![Swagger-Benutzeroberfläche mit Versionsinformationen: Beschreibung, Autor, Link „Mehr anzeigen“](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="1059a-151">XML-Kommentare</span><span class="sxs-lookup"><span data-stu-id="1059a-151">XML comments</span></span>

<span data-ttu-id="1059a-152">XML-Kommentare können mithilfe der folgenden Ansätze aktiviert werden:</span><span class="sxs-lookup"><span data-stu-id="1059a-152">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1059a-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1059a-153">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="1059a-154">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **<project_name>.csproj bearbeiten** aus.</span><span class="sxs-lookup"><span data-stu-id="1059a-154">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="1059a-155">Fügen Sie die hervorgehobenen Zeilen manuell der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-155">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="1059a-156">Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.</span><span class="sxs-lookup"><span data-stu-id="1059a-156">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="1059a-157">Überprüfen Sie das Feld **XML-Dokumentationsdatei** im Abschnitt **Ausgabe** der Registerkarte **Erstellen**.</span><span class="sxs-lookup"><span data-stu-id="1059a-157">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="1059a-158">Visual Studio für Mac</span><span class="sxs-lookup"><span data-stu-id="1059a-158">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="1059a-159">Drücken und halten Sie im *Lösungspad* die **CONTROL**-Taste, und klicken Sie auf den Projektnamen.</span><span class="sxs-lookup"><span data-stu-id="1059a-159">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="1059a-160">Navigieren Sie zu **Extras** > **Datei bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="1059a-160">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="1059a-161">Fügen Sie die hervorgehobenen Zeilen manuell der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-161">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="1059a-162">Öffnen Sie das Dialogfeld **Projektoptionen** > **Erstellen** > **Compiler**.</span><span class="sxs-lookup"><span data-stu-id="1059a-162">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="1059a-163">Überprüfen Sie das Feld **XML-Dokumentation generieren** im Abschnitt **Allgemeine Optionen**.</span><span class="sxs-lookup"><span data-stu-id="1059a-163">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="1059a-164">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="1059a-164">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="1059a-165">Fügen Sie die hervorgehobenen Zeilen manuell der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-165">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1059a-166">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="1059a-166">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="1059a-167">Fügen Sie die hervorgehobenen Zeilen manuell der *CSPROJ*-Datei hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-167">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="1059a-168">Das Aktivieren von XML-Kommentaren stellt Debuginformationen zu nicht-dokumentierten öffentlichen Typen und Members bereit.</span><span class="sxs-lookup"><span data-stu-id="1059a-168">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="1059a-169">Auf nicht dokumentierte Typen und Member wird durch die Warnmeldung verwiesen.</span><span class="sxs-lookup"><span data-stu-id="1059a-169">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="1059a-170">Die folgende Meldung macht z.B. durch den Warnungscode 1591 auf einen Verstoß aufmerksam:</span><span class="sxs-lookup"><span data-stu-id="1059a-170">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="1059a-171">Um Warnungen projektübergreifend zu unterdrücken, definieren Sie eine Liste der zu ignorierenden Warnungscodes (mit Semikolon als Trennzeichen).</span><span class="sxs-lookup"><span data-stu-id="1059a-171">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="1059a-172">Das Anfügen von Warnungscodes an `$(NoWarn);` gilt auch für die C#-Standardwerte.</span><span class="sxs-lookup"><span data-stu-id="1059a-172">Appending the warning codes to `$(NoWarn);` applies the C# default values too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="1059a-173">Um Warnungen nur für bestimmte Member zu unterdrücken, schließen Sie den Code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning)-Präprozessordirektiven ein.</span><span class="sxs-lookup"><span data-stu-id="1059a-173">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="1059a-174">Dieser Ansatz eignet sich für Code, der nicht über die API-Dokumentation verfügbar gemacht werden soll. Im folgenden Beispiel wird der Warnungscode CS1591 für die gesamte Klasse `Program` ignoriert.</span><span class="sxs-lookup"><span data-stu-id="1059a-174">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="1059a-175">Das Erzwingen des Warnungscodes wird am Ende der Klassendefinition wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="1059a-175">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="1059a-176">Geben Sie mehrere Warnungscodes in einer kommagetrennten Liste an.</span><span class="sxs-lookup"><span data-stu-id="1059a-176">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="1059a-177">Konfigurieren Sie Swagger, um die generierte XML-Datei verwenden.</span><span class="sxs-lookup"><span data-stu-id="1059a-177">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="1059a-178">Bei Linux oder anderen Betriebssystemen als Windows können bei Dateinamen und -pfaden Groß-/Kleinbuchstaben berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="1059a-178">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="1059a-179">Die Datei *TodoApi.XML* ist beispielsweise unter Windows, nicht aber unter CentOS gültig.</span><span class="sxs-lookup"><span data-stu-id="1059a-179">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="1059a-180">Im vorangehenden Codeausschnitt wurde durch [Reflektion](/dotnet/csharp/programming-guide/concepts/reflection) ein XML-Dateiname erstellt, der dem Namen des Web-API-Projekts entspricht.</span><span class="sxs-lookup"><span data-stu-id="1059a-180">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="1059a-181">Die [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory)-Eigenschaft wird verwendet, um einen Pfad zu der XML-Datei zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1059a-181">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="1059a-182">Das Hinzufügen von Kommentaren mit drei Schrägstrichen zu einer Aktion erweitert die Swagger-Benutzeroberfläche, indem die Beschreibung zum Header des Abschnitts hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="1059a-182">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="1059a-183">Fügen Sie nun über der `Delete`-Aktion ein [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary)-Element hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-183">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="1059a-184">Auf der Swagger-Benutzeroberfläche wird der innere Text des `<summary>`-Elements angezeigt, das im letzten Codeausschnitt angegeben wurde:</span><span class="sxs-lookup"><span data-stu-id="1059a-184">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Swagger-Benutzeroberfläche mit dem XML-Kommentar „Löscht ein bestimmtes TodoItem.“](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="1059a-187">Die Benutzeroberfläche wird durch das generierte JSON-Schema gesteuert:</span><span class="sxs-lookup"><span data-stu-id="1059a-187">The UI is driven by the generated JSON schema:</span></span>

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

<span data-ttu-id="1059a-188">Fügen Sie der Dokumentation der `Create`-Aktionsmethode ein [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks)-Element hinzu.</span><span class="sxs-lookup"><span data-stu-id="1059a-188">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="1059a-189">Dieses ergänzt die Informationen, die im `<summary>`-Element angegeben wurden, und sorgt für eine robustere Swagger-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="1059a-189">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="1059a-190">Das `<remarks>`-Element kann aus Text, JSON-Code oder XML-Code bestehen.</span><span class="sxs-lookup"><span data-stu-id="1059a-190">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="1059a-191">Durch die zusätzlichen Kommentare wird die Benutzeroberfläche wie unten gezeigt erweitert:</span><span class="sxs-lookup"><span data-stu-id="1059a-191">Notice the UI enhancements with these additional comments:</span></span>

![Swagger-Benutzeroberfläche mit zusätzlichen Kommentaren](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="1059a-193">Datenanmerkungen</span><span class="sxs-lookup"><span data-stu-id="1059a-193">Data annotations</span></span>

<span data-ttu-id="1059a-194">Sie können die Komponenten der Swagger-Benutzeroberfläche steuern, indem Sie dem Modell Attribute aus dem Namespace [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1059a-194">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="1059a-195">Fügen Sie das `[Required]`-Attribut der `Name`-Eigenschaft der `TodoItem`-Klasse hinzu:</span><span class="sxs-lookup"><span data-stu-id="1059a-195">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="1059a-196">Das Vorhandensein dieses Attributs ändert das Verhalten der Benutzeroberfläche und des zugrunde liegenden JSON-Schemas:</span><span class="sxs-lookup"><span data-stu-id="1059a-196">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="1059a-197">Fügen Sie das `[Produces("application/json")]`-Attribut zum API-Controller hinzu.</span><span class="sxs-lookup"><span data-stu-id="1059a-197">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="1059a-198">Dadurch kann deklariert werden, dass die Aktionen des Controllers den Inhaltstyp *application/json* für Antworten unterstützen:</span><span class="sxs-lookup"><span data-stu-id="1059a-198">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="1059a-199">Im Dropdownmenü des **Anforderungsinhaltstyps** ist dieser Inhaltstyp als Standard für die GET-Aktionen des Controllers ausgewählt:</span><span class="sxs-lookup"><span data-stu-id="1059a-199">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger UI mit Standardinhaltstyp für die Antwort](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="1059a-201">Mit zunehmender Verwendung von Datenanmerkungen in der Web-API werden die UI- und API-Hilfeseiten beschreibender und nützlicher.</span><span class="sxs-lookup"><span data-stu-id="1059a-201">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="1059a-202">Beschreiben von Antworttypen</span><span class="sxs-lookup"><span data-stu-id="1059a-202">Describe response types</span></span>

<span data-ttu-id="1059a-203">Entwickler, die gleichzeitig API-Nutzer sind, interessieren sich vor allem dafür, welche Antworttypen und Fehlercodes zurückgegeben werden (wenn diese nicht dem Standard entsprechen).</span><span class="sxs-lookup"><span data-stu-id="1059a-203">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="1059a-204">Die Antworttypen und Fehlercodes sind in den XML-Kommentaren und Datenanmerkungen gekennzeichnet.</span><span class="sxs-lookup"><span data-stu-id="1059a-204">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="1059a-205">Die Aktion `Create` gibt bei einer erfolgreichen Anforderung den Statuscode „HTTP 201“ zurück.</span><span class="sxs-lookup"><span data-stu-id="1059a-205">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="1059a-206">Der Statuscode „HTTP 400“ wird zurückgegeben, wenn der gesendete Anforderungstext NULL ist.</span><span class="sxs-lookup"><span data-stu-id="1059a-206">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="1059a-207">Ohne richtige Dokumentation in der Swagger-Benutzeroberfläche fehlt dem Consumer das Wissen über diese erwarteten Ergebnisse.</span><span class="sxs-lookup"><span data-stu-id="1059a-207">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="1059a-208">Sie können dieses Problem beheben, indem Sie die hervorgehobenen Zeilen im folgenden Beispiel hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1059a-208">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="1059a-209">Die Swagger-Benutzeroberfläche dokumentiert nun deutlich die erwarteten HTTP-Antwortcodes:</span><span class="sxs-lookup"><span data-stu-id="1059a-209">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger-Benutzeroberfläche, die die Klassenbeschreibung „Gibt das neu erstellte Todo-Element zurück“ nach der Antwort anzeigt sowie „400“ für den Statuscode und „Wenn das Element NULL ist“ als Grund unter den Antwortnachrichten](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="1059a-211">Anpassen der Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="1059a-211">Customize the UI</span></span>

<span data-ttu-id="1059a-212">Die Standardbenutzeroberfläche ist zwar funktionsfähig und visuell ansprechend,</span><span class="sxs-lookup"><span data-stu-id="1059a-212">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="1059a-213">auf API-Dokumentationsseiten sollte jedoch Ihre Marke oder Ihr Design zu sehen sein.</span><span class="sxs-lookup"><span data-stu-id="1059a-213">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="1059a-214">Für das Branding von Swashbuckle-Komponenten sind zusätzliche Ressourcen erforderlich, damit statische Dateien bereitgestellt werden können und sich die Ordnerstruktur zum Hosten dieser Dateien erstellt lässt.</span><span class="sxs-lookup"><span data-stu-id="1059a-214">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="1059a-215">Wenn .NET Framework oder .NET Core 1.x die Zielkomponente ist, müssen Sie Ihrem Projekt das NuGet-Paket [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1059a-215">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="1059a-216">Dieses Paket ist bereits installiert, wenn Sie als Zielkomponente .NET Core 2.x und das [Metapaket](xref:fundamentals/metapackage) verwenden.</span><span class="sxs-lookup"><span data-stu-id="1059a-216">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="1059a-217">Aktivieren Sie die Middleware für statische Dateien:</span><span class="sxs-lookup"><span data-stu-id="1059a-217">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="1059a-218">Rufen Sie die Inhalte des Ordners *dist* aus dem [GitHub-Repository für die Swagger-Benutzeroberfläche](https://github.com/swagger-api/swagger-ui/tree/master/dist) ab.</span><span class="sxs-lookup"><span data-stu-id="1059a-218">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="1059a-219">Dieser Ordner enthält die erforderlichen Ressourcen für die Seite der Swagger-Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="1059a-219">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="1059a-220">Erstellen Sie einen *wwwroot/swagger/ui*-Ordner, und kopieren Sie die Inhalte des Ordners *dist* in ihn.</span><span class="sxs-lookup"><span data-stu-id="1059a-220">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="1059a-221">Erstellen Sie unter *wwwroot/swagger/ui* die Datei *custom.css* mit dem folgenden CSS-Code, um den Seitenheader anzupassen:</span><span class="sxs-lookup"><span data-stu-id="1059a-221">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="1059a-222">Verweisen Sie in der Datei *index.html* unterhalb der anderen CSS-Dateien auf *custom.css*:</span><span class="sxs-lookup"><span data-stu-id="1059a-222">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="1059a-223">Navigieren Sie zur Seite *index.html* auf `http://localhost:<port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="1059a-223">Browse to the *index.html* page at `http://localhost:<port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="1059a-224">Geben Sie `http://localhost:<port>/swagger/v1/swagger.json` in das Textfeld des Headers ein, und klicken Sie dann auf die Schaltfläche **Durchsuchen**.</span><span class="sxs-lookup"><span data-stu-id="1059a-224">Enter `http://localhost:<port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="1059a-225">Die Ergebnisseite sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="1059a-225">The resulting page looks as follows:</span></span>

![Swagger-Benutzeroberfläche mit benutzerdefiniertem Headertitel](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="1059a-227">Es gibt noch viele weitere Verwendungsmöglichkeiten für diese Seite.</span><span class="sxs-lookup"><span data-stu-id="1059a-227">There's much more you can do with the page.</span></span> <span data-ttu-id="1059a-228">Weitere Informationen zum vollen Funktionsumgang der UI-Ressourcen finden Sie im [GitHub-Repository für die Swagger-Benutzeroberfläche](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="1059a-228">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
