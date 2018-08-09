---
title: Dateianbieter in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie ASP.NET Core Dateisystemzugriff durch die Verwendung von Dateianbietern abstrahiert.
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 512229cfe7d7efdcd9050fa13dbdbf793be29a0b
ms.sourcegitcommit: 571d76fbbff05e84406b6d909c8fe9cbea2c8ff1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/01/2018
ms.locfileid: "39410155"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="705ed-103">Dateianbieter in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="705ed-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="705ed-104">Von [Steve Smith](https://ardalis.com/) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="705ed-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="705ed-105">ASP.NET Core abstrahiert Dateisystemzugriff durch die Verwendung von Dateianbietern.</span><span class="sxs-lookup"><span data-stu-id="705ed-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="705ed-106">Dateianbieter werden im gesamten ASP.NET Core-Framework verwendet:</span><span class="sxs-lookup"><span data-stu-id="705ed-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="705ed-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) stellt das Inhaltsstammverzeichnis und das Webstammverzeichnis der Anwendung als `IFileProvider`-Typen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="705ed-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="705ed-108">Die [Middleware der statischen Dateien](xref:fundamentals/static-files) verwendet Dateianbieter, um statische Dateien zu suchen.</span><span class="sxs-lookup"><span data-stu-id="705ed-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="705ed-109">[Razor](xref:mvc/views/razor) verwendet Dateianbieter, um Seiten und Ansichten zu suchen.</span><span class="sxs-lookup"><span data-stu-id="705ed-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="705ed-110">.NET Core-Tools verwenden Dateianbieter und Globmuster, um anzugeben, welche Dateien veröffentlicht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="705ed-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="705ed-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="705ed-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="705ed-112">Dateianbieterschnittstellen</span><span class="sxs-lookup"><span data-stu-id="705ed-112">File Provider interfaces</span></span>

<span data-ttu-id="705ed-113">Die primäre Schnittstelle ist [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span><span class="sxs-lookup"><span data-stu-id="705ed-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="705ed-114">`IFileProvider` stellt Methoden zur Verfügung, zum:</span><span class="sxs-lookup"><span data-stu-id="705ed-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="705ed-115">Abrufen von Dateiinformationen ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span><span class="sxs-lookup"><span data-stu-id="705ed-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="705ed-116">Abrufen von Verzeichnisinformationen ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span><span class="sxs-lookup"><span data-stu-id="705ed-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="705ed-117">Einrichten von Änderungsbenachrichtigungen (mithilfe eines [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span><span class="sxs-lookup"><span data-stu-id="705ed-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="705ed-118">`IFileInfo` stellt Methoden und Eigenschaften für die Arbeit mit Dateien bereit:</span><span class="sxs-lookup"><span data-stu-id="705ed-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="705ed-119">Exists</span><span class="sxs-lookup"><span data-stu-id="705ed-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="705ed-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="705ed-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="705ed-121">Name</span><span class="sxs-lookup"><span data-stu-id="705ed-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="705ed-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in Bytes)</span><span class="sxs-lookup"><span data-stu-id="705ed-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="705ed-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified)-Datum</span><span class="sxs-lookup"><span data-stu-id="705ed-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="705ed-124">Mithilfe der [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream)-Methode können Sie die Datei auslesen.</span><span class="sxs-lookup"><span data-stu-id="705ed-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="705ed-125">Die Beispiel-App demonstriert die Konfiguration eines Dateianbieters in `Startup.ConfigureServices` für die Verwendung in der gesamten App über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="705ed-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="705ed-126">Dateianbieterimplementierungen</span><span class="sxs-lookup"><span data-stu-id="705ed-126">File Provider implementations</span></span>

<span data-ttu-id="705ed-127">Es sind drei Implementierungen von `IFileProvider` verfügbar.</span><span class="sxs-lookup"><span data-stu-id="705ed-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="705ed-128">Implementierung</span><span class="sxs-lookup"><span data-stu-id="705ed-128">Implementation</span></span> | <span data-ttu-id="705ed-129">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="705ed-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="705ed-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="705ed-131">Der physische Anbieter wird verwendet, um auf die physischen Systemdateien zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="705ed-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="705ed-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="705ed-133">Der eingebettete Manifesanbieter wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="705ed-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="705ed-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="705ed-135">Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnisse von einem oder mehreren anderen Anbietern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="705ed-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="705ed-136">Implementierung</span><span class="sxs-lookup"><span data-stu-id="705ed-136">Implementation</span></span> | <span data-ttu-id="705ed-137">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="705ed-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="705ed-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="705ed-139">Der physische Anbieter wird verwendet, um auf die physischen Systemdateien zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="705ed-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="705ed-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="705ed-141">Der eingebettete Anbieter wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="705ed-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="705ed-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="705ed-143">Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnisse von einem oder mehreren anderen Anbietern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="705ed-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="705ed-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-144">PhysicalFileProvider</span></span>

<span data-ttu-id="705ed-145">Der [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ermöglicht den Zugriff auf das physische Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="705ed-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="705ed-146">`PhysicalFileProvider` verwendet den [System.IO.File](/dotnet/api/system.io.file)-Typ (für den physischen Anbieter). Alle Pfade werden einem Verzeichnis und seinen untergeordneten Elementen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="705ed-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="705ed-147">Diese Zuordnung verhindert den Zugriff auf das Dateisystem außerhalb des angegebenen Verzeichnisses und den untergeordneten Elementen.</span><span class="sxs-lookup"><span data-stu-id="705ed-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="705ed-148">Beim Instanziieren dieses Anbieters ist ein Verzeichnispfad erforderlich, der als Basispfad für alle Anforderungen über diesen Anbieter dient.</span><span class="sxs-lookup"><span data-stu-id="705ed-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="705ed-149">Sie können einen `PhysicalFileProvider`-Anbieter direkt instanziieren oder einen `IFileProvider` in einem Konstruktor über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) anfordern.</span><span class="sxs-lookup"><span data-stu-id="705ed-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="705ed-150">**Statische Typen**</span><span class="sxs-lookup"><span data-stu-id="705ed-150">**Static types**</span></span>

<span data-ttu-id="705ed-151">Der folgende Code zeigt die Erstellung eines `PhysicalFileProvider` und dessen Verwendung zum Abrufen von Verzeichnisinhalten und Dateiinformationen:</span><span class="sxs-lookup"><span data-stu-id="705ed-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="705ed-152">Typen im vorherigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="705ed-152">Types in the preceding example:</span></span>

* <span data-ttu-id="705ed-153">`provider` ist ein `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="705ed-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="705ed-154">`contents` ist ein `IDirectoryContents`.</span><span class="sxs-lookup"><span data-stu-id="705ed-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="705ed-155">`fileInfo` ist ein `IFileInfo`.</span><span class="sxs-lookup"><span data-stu-id="705ed-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="705ed-156">Der Dateianbieter kann verwendet werden, um das durch `applicationRoot` angegebene Verzeichnis zu durchlaufen oder `GetFileInfo` aufzurufen, um die Informationen einer Datei anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="705ed-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="705ed-157">Der Dateianbieter hat keinen Zugriff außerhalb des `applicationRoot`-Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="705ed-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="705ed-158">Die Beispiel-App erstellt einen Anbieter in `Startup.ConfigureServices`-Klasse der App mit [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span><span class="sxs-lookup"><span data-stu-id="705ed-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="705ed-159">**Abrufen der Dateianbietertypen mit Abhängigkeitsinjektion**</span><span class="sxs-lookup"><span data-stu-id="705ed-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="705ed-160">Fügen Sie den Anbieter in jeden Klassenkonstruktor ein, und weisen Sie ihn einem lokalen Feld zu.</span><span class="sxs-lookup"><span data-stu-id="705ed-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="705ed-161">Verwenden Sie das Feld in der Methoden der Klasse für den Dateizugriff.</span><span class="sxs-lookup"><span data-stu-id="705ed-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="705ed-162">In der Beispiel-App erhält die `IndexModel`-Klasse eine `IFileProvider`-Instanz, um Inhalt des Verzeichnisses für die den Basispfad der App zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="705ed-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="705ed-163">*Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="705ed-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="705ed-164">Die `IDirectoryContents` werden auf der Seite durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="705ed-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="705ed-165">*Pages/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="705ed-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="705ed-166">In der Beispiel-App erhält die `HomeController`-Klasse eine `IFileProvider`-Instanz, um Inhalt des Verzeichnisses für die den Basispfad der App zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="705ed-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="705ed-167">*Controllers/HomeController.cs*:</span><span class="sxs-lookup"><span data-stu-id="705ed-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="705ed-168">Die `IDirectoryContents` werden in der Ansicht durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="705ed-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="705ed-169">*Views/Home/Index.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="705ed-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="705ed-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="705ed-171">Der [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="705ed-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="705ed-172">Der `ManifestEmbeddedFileProvider` verwendet ein in die Assembly kompiliertes Manifest, um die ursprünglichen Pfade der eingebetteten Dateien zu rekonstruieren.</span><span class="sxs-lookup"><span data-stu-id="705ed-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="705ed-173">Der `ManifestEmbeddedFileProvider` ist in ASP.NET Core 2.1 und höher verfügbar.</span><span class="sxs-lookup"><span data-stu-id="705ed-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="705ed-174">Informationen zum Zugriff auf Dateien, die in Assemblys in ASP.NET Core 2.0 oder früher eingebettet sind, finden Sie in der Version [ASP.NET Core 1.x dieses Themas](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="705ed-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](xref:fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="705ed-175">Um ein Manifest der eingebetteten Dateien zu generieren, legen die `<GenerateEmbeddedFilesManifest>`-Eigenschaft auf `true` fest.</span><span class="sxs-lookup"><span data-stu-id="705ed-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="705ed-176">Geben Sie die einzubettenden Dateien mit [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) an:</span><span class="sxs-lookup"><span data-stu-id="705ed-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="705ed-177">Verwenden Sie [Globmuster](#glob-patterns), um eine oder mehr Dateien anzugeben, die in die Assembly eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="705ed-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="705ed-178">Die Beispiel-App erstellt einen `ManifestEmbeddedFileProvider` und übergibt die aktuell ausgeführte Assembly an den Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="705ed-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="705ed-179">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="705ed-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="705ed-180">Mit zusätzlichen Überladungen können Sie:</span><span class="sxs-lookup"><span data-stu-id="705ed-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="705ed-181">Einen relativen Dateipfad angeben.</span><span class="sxs-lookup"><span data-stu-id="705ed-181">Specify a relative file path.</span></span>
* <span data-ttu-id="705ed-182">Dateien einem zuletzt geänderten Datum zuordnen.</span><span class="sxs-lookup"><span data-stu-id="705ed-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="705ed-183">Die eingebettete Ressource benennen, die das eingebettete Dateimanifest enthält.</span><span class="sxs-lookup"><span data-stu-id="705ed-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="705ed-184">Überladung</span><span class="sxs-lookup"><span data-stu-id="705ed-184">Overload</span></span> | <span data-ttu-id="705ed-185">Beschreibung </span><span class="sxs-lookup"><span data-stu-id="705ed-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="705ed-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="705ed-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="705ed-187">Akzeptiert einen optionalen relativen `root`-Pfadparameter.</span><span class="sxs-lookup"><span data-stu-id="705ed-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="705ed-188">Geben Sie `root` an, um Aufrufe an [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) den Ressourcen im bereitgestellten Pfad zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="705ed-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="705ed-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="705ed-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="705ed-190">Akzeptiert einen optionalen relativen `root`-Pfadparameter und einen `lastModified` ([DateTimeOffset](/dotnet/api/system.datetimeoffset))-Datumsparameter.</span><span class="sxs-lookup"><span data-stu-id="705ed-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="705ed-191">Das `lastModified`-Datum ordnet das letzte Änderungsdatum für die [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)-Instanzen zu, die vom [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) zurückgegeben wurden.</span><span class="sxs-lookup"><span data-stu-id="705ed-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="705ed-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="705ed-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="705ed-193">Akzeptiert einen optionalen, relativen `root`-Pfad, ein `lastModified`-Datum und `manifestName`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="705ed-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="705ed-194">`manifestName` stellt den Namen der eingebetteten Ressource dar, die das Manifest enthält.</span><span class="sxs-lookup"><span data-stu-id="705ed-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="705ed-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="705ed-196">Der [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="705ed-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="705ed-197">Geben Sie die Dateien an, die mit der [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)-Eigenschaft in der Projektdatei eingebettet werden sollen:</span><span class="sxs-lookup"><span data-stu-id="705ed-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="705ed-198">Verwenden Sie [Globmuster](#glob-patterns), um eine oder mehr Dateien anzugeben, die in die Assembly eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="705ed-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="705ed-199">Die Beispiel-App erstellt einen `EmbeddedFileProvider` und übergibt die aktuell ausgeführte Assembly an den Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="705ed-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="705ed-200">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="705ed-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="705ed-201">Eingebettete Ressourcen machen keine Verzeichnisse verfügbar.</span><span class="sxs-lookup"><span data-stu-id="705ed-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="705ed-202">Stattdessen wird der Ressourcenpfad (über dessen Namespace) mithilfe von `.`-Trennzeichen in ihren Dateinamen eingebettet.</span><span class="sxs-lookup"><span data-stu-id="705ed-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="705ed-203">In der Beispiel-App ist `baseNamespace` `FileProviderSample.`.</span><span class="sxs-lookup"><span data-stu-id="705ed-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="705ed-204">Der [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_)-Konstruktor akzeptiert einen optionalen `baseNamespace`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="705ed-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="705ed-205">Geben Sie den Basisnamespace an, um Aufrufe an [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) den Ressourcen im bereitgestellten Namespace zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="705ed-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="705ed-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="705ed-206">CompositeFileProvider</span></span>

<span data-ttu-id="705ed-207">Der [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) kombiniert `IFileProvider`-Instanzen, die eine einzelne Schnittstelle zum Arbeiten mit Dateien von mehreren Anbietern verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="705ed-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="705ed-208">Beim Erstellen des `CompositeFileProvider` übergeben Sie eine oder mehrere `IFileProvider`-Instanzen an seinen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="705ed-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="705ed-209">In der Beispiel-App stellt ein `PhysicalFileProvider` und ein `ManifestEmbeddedFileProvider` Dateien an ein `CompositeFileProvider`, der im Dienstcontainer der App registriert ist:</span><span class="sxs-lookup"><span data-stu-id="705ed-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="705ed-210">In der Beispiel-App stellt ein `PhysicalFileProvider` und ein `EmbeddedFileProvider` Dateien an ein `CompositeFileProvider`, der im Dienstcontainer der App registriert ist:</span><span class="sxs-lookup"><span data-stu-id="705ed-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="705ed-211">Überwachen auf Änderungen</span><span class="sxs-lookup"><span data-stu-id="705ed-211">Watch for changes</span></span>

<span data-ttu-id="705ed-212">Die [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch)-Methode bietet ein Szenario, eine oder mehrere Dateien oder Verzeichnisse auf Änderungen zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="705ed-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="705ed-213">`Watch`akzeptiert eine Pfadzeichenfolge, die [Globmuster](#glob-patterns) verwenden kann, um mehrere Dateien anzugeben.</span><span class="sxs-lookup"><span data-stu-id="705ed-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="705ed-214">`Watch` gibt ein [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) zurück.</span><span class="sxs-lookup"><span data-stu-id="705ed-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="705ed-215">Das Änderungstoken macht folgendes verfügbar:</span><span class="sxs-lookup"><span data-stu-id="705ed-215">The change token exposes:</span></span>

* <span data-ttu-id="705ed-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): eine Eigenschaft, die überprüft werden kann, um festzustellen, ob eine Änderung vorgenommen wurde.</span><span class="sxs-lookup"><span data-stu-id="705ed-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="705ed-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): wird aufgerufen, wenn Änderungen an der angegebenen Pfadzeichenfolge erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="705ed-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="705ed-218">Jedes Änderungstoken ruft nur seine zugeordneten Rückrufe als Antwort auf eine einzelne Änderung auf.</span><span class="sxs-lookup"><span data-stu-id="705ed-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="705ed-219">Zur Aktivierung der konstanten Überwachung können Sie wie unten dargestellt eine [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) verwenden oder `IChangeToken`-Instanzen als Reaktion auf Änderungen neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="705ed-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="705ed-220">In der Beispiel-App wird die *WatchConsole*-Konsolen-App konfiguriert, damit sie bei einer Änderung einer Textdatei eine Meldung anzeigt:</span><span class="sxs-lookup"><span data-stu-id="705ed-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="705ed-221">Einige Dateisysteme, z.B. Docker-Container und Netzwerkfreigaben, senden Änderungsmeldungen möglicherweise nicht zuverlässig .</span><span class="sxs-lookup"><span data-stu-id="705ed-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="705ed-222">Legen Sie die `DOTNET_USE_POLLING_FILE_WATCHER`-Umgebungsvariable auf `1` oder `true` fest, um das Dateisystem alle 4 Sekunden nach Änderungen zu untersuchen (nicht konfigurierbar).</span><span class="sxs-lookup"><span data-stu-id="705ed-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="705ed-223">Globmuster</span><span class="sxs-lookup"><span data-stu-id="705ed-223">Glob patterns</span></span>

<span data-ttu-id="705ed-224">Dateisystempfade verwenden Platzhaltermuster namens *Globmuster*.</span><span class="sxs-lookup"><span data-stu-id="705ed-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="705ed-225">Geben Sie Gruppen von Dateien mit diesen Mustern an.</span><span class="sxs-lookup"><span data-stu-id="705ed-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="705ed-226">Die zwei Platzhalterzeichen sind `*` und `**`:</span><span class="sxs-lookup"><span data-stu-id="705ed-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="705ed-227">Überprüft alles auf der aktuellen Ordnerebene, jeden Dateinamen oder jede Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="705ed-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="705ed-228">Übereinstimmungen werden durch `/`- und `.`-Zeichen im Dateipfad beendet.</span><span class="sxs-lookup"><span data-stu-id="705ed-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="705ed-229">Überprüft alles auf mehrere Verzeichnisebenen.</span><span class="sxs-lookup"><span data-stu-id="705ed-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="705ed-230">Kann verwendet werden, um rekursiv eine Übereinstimmung für viele Dateien innerhalb einer Verzeichnishierarchie zu finden.</span><span class="sxs-lookup"><span data-stu-id="705ed-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="705ed-231">**Beispiele für Globmuster**</span><span class="sxs-lookup"><span data-stu-id="705ed-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="705ed-232">Entspricht einer bestimmten Datei in einem bestimmten Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="705ed-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="705ed-233">Entspricht allen Dateien mit *.txt*-Erweiterung in einem bestimmten Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="705ed-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="705ed-234">Entspricht allen `appsettings.json`-Dateien in Verzeichnissen auf der nächsttieferen Ebene des *directory*-Ordners.</span><span class="sxs-lookup"><span data-stu-id="705ed-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="705ed-235">Entspricht allen Dateien mit *.txt*-Erweiterung, die an einer beliebigen Stelle unter dem *directory*-Ordner gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="705ed-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
