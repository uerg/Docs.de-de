---
title: Dateianbieter in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core Dateisystemzugriff durch die Verwendung von Dateianbietern abstrahiert.
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276238"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="d5fec-103">Dateianbieter in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5fec-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="d5fec-104">Von [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d5fec-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d5fec-105">ASP.NET Core abstrahiert Dateisystemzugriff durch die Verwendung von Dateianbietern.</span><span class="sxs-lookup"><span data-stu-id="d5fec-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="d5fec-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d5fec-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="d5fec-107">Dateianbieterabstraktionen</span><span class="sxs-lookup"><span data-stu-id="d5fec-107">File Provider abstractions</span></span>

<span data-ttu-id="d5fec-108">Dateianbieter sind eine Abstraktion über Dateisysteme.</span><span class="sxs-lookup"><span data-stu-id="d5fec-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="d5fec-109">Die Hauptschnittstelle ist `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="d5fec-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="d5fec-110">`IFileProvider` macht Methoden zum Abrufen von Informationen (`IFileInfo`), Verzeichnisinformationen (`IDirectoryContents`) und zum Einrichten von Benachrichtigungen (mithilfe eines `IChangeToken`) verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d5fec-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="d5fec-111">`IFileInfo` stellt Methoden und Eigenschaften einzelner Dateien oder Verzeichnisse zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d5fec-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="d5fec-112">Sie verfügt über zwei boolesche Eigenschaften, `Exists` und `IsDirectory` sowie über Eigenschaften, die `Name`, `Length` (in Bytes) und das `LastModified`-Datum der Datei beschreiben.</span><span class="sxs-lookup"><span data-stu-id="d5fec-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="d5fec-113">Sie können die Datei mithilfe der `CreateReadStream`-Methode lesen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="d5fec-114">Dateianbieterimplementierungen</span><span class="sxs-lookup"><span data-stu-id="d5fec-114">File Provider implementations</span></span>

<span data-ttu-id="d5fec-115">Es sind drei Implementierungen von `IFileProvider` verfügbar: physische, eingebettete und zusammengesetzte.</span><span class="sxs-lookup"><span data-stu-id="d5fec-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="d5fec-116">Der physische Anbieter wird verwendet, um auf die tatsächlichen Systemdateien zuzugreifen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="d5fec-117">Der eingebettete Anbieter wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="d5fec-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="d5fec-118">Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnisse von einem oder mehreren anderen Anbietern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="d5fec-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="d5fec-119">PhysicalFileProvider</span></span>

<span data-ttu-id="d5fec-120">Der `PhysicalFileProvider` ermöglicht den Zugriff auf das physische Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="d5fec-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="d5fec-121">Er dient als Wrapper für den `System.IO.File`-Typ (für den physischen Anbieter). Alle Pfade werden einem Verzeichnis und seinen untergeordneten Elementen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="d5fec-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="d5fec-122">Diese Bereichsdefinition schränkt den Zugriff auf ein bestimmtes Verzeichnis und die untergeordneten Elemente ein, wodurch der Zugriff auf Dateisysteme außerhalb dieses Bereichs verhindert wird.</span><span class="sxs-lookup"><span data-stu-id="d5fec-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="d5fec-123">Beim Instanziieren dieses Anbieters müssen Sie einen Verzeichnispfad zur Verfügung stellen, der als Basispfad für alle Anforderungen an diesen Anbieter dient (und den Zugriff außerhalb dieses Pfads beschränkt).</span><span class="sxs-lookup"><span data-stu-id="d5fec-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="d5fec-124">In einer ASP.NET Core-Anwendung können Sie einen `PhysicalFileProvider`-Anbieter direkt instanziieren oder einen `IFileProvider` in einem Controller oder Dienstkonstruktor über [Dependeny Injection](dependency-injection.md) anfordern.</span><span class="sxs-lookup"><span data-stu-id="d5fec-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="d5fec-125">Der letzte Ansatz ergibt in der Regel eine flexiblere und testbare Lösung.</span><span class="sxs-lookup"><span data-stu-id="d5fec-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="d5fec-126">Das folgende Beispiel zeigt, wie Sie einen `PhysicalFileProvider` erstellen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="d5fec-127">Sie können die Verzeichnisinhalte durchlaufen oder die Informationen für eine bestimmte Datei durch die Bereitstellung eines Unterpfads abrufen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="d5fec-128">Zur Anbieteranforderung von einem Controller müssen Sie ihn im Konstruktor des Controllers angeben und einem lokalen Feld zuweisen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="d5fec-129">Verwenden Sie die lokale Instanz aus Ihren Aktionsmethoden:</span><span class="sxs-lookup"><span data-stu-id="d5fec-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="d5fec-130">Erstellen Sie dann den Anbieter in der `Startup`-Anwendungsklasse:</span><span class="sxs-lookup"><span data-stu-id="d5fec-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="d5fec-131">Durchlaufen Sie in der *Index.cshtml*-Ansicht die bereitgestellten `IDirectoryContents`:</span><span class="sxs-lookup"><span data-stu-id="d5fec-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="d5fec-132">Das Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="d5fec-132">The result:</span></span>

![Die Beispielanwendung des Dateianbieters mit physischen Dateien und Ordnern](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="d5fec-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="d5fec-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="d5fec-135">Der `EmbeddedFileProvider` wird verwendet, um auf Dateien zuzugreifen, die in Assemblys eingebettet sind.</span><span class="sxs-lookup"><span data-stu-id="d5fec-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="d5fec-136">Betten Sie Ihre Dateien in .NET Core mit dem `<EmbeddedResource>`-Element in der *CSPROJ*-Datei in eine Assembly ein:</span><span class="sxs-lookup"><span data-stu-id="d5fec-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="d5fec-137">Sie können [Globmuster](#globbing-patterns) verwenden, wenn Sie Dateien für die Einbettung in der Assembly angeben.</span><span class="sxs-lookup"><span data-stu-id="d5fec-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="d5fec-138">Diese Muster können verwendet werden, um eine oder mehrere Dateien zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="d5fec-139">Es ist unwahrscheinlich, dass Sie jemals tatsächlich jede JS-Datei in Ihrem Projekt in die Assembly einbetten möchten. Das obige Beispiel dient nur zu Demonstrationszwecken.</span><span class="sxs-lookup"><span data-stu-id="d5fec-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="d5fec-140">Beim Erstellen eines `EmbeddedFileProvider` übergeben Sie die Assembly, die für ihren Konstruktor gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="d5fec-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="d5fec-141">Der obige Codeausschnitt veranschaulicht die Erstellung eines `EmbeddedFileProvider` mit Zugriff auf die gerade ausgeführte Assembly.</span><span class="sxs-lookup"><span data-stu-id="d5fec-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="d5fec-142">Das Aktualisieren der Beispielanwendung für die Verwendung eines `EmbeddedFileProvider` ergibt die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="d5fec-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Die Beispielanwendung des Dateianbieters mit eingebetteten Dateien](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="d5fec-144">Eingebettete Ressourcen machen keine Verzeichnisse verfügbar.</span><span class="sxs-lookup"><span data-stu-id="d5fec-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="d5fec-145">Stattdessen wird der Ressourcenpfad (über dessen Namespace) mithilfe von `.`-Trennzeichen in ihren Dateinamen eingebettet.</span><span class="sxs-lookup"><span data-stu-id="d5fec-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="d5fec-146">Der `EmbeddedFileProvider`-Konstruktor akzeptiert einen optionalen `baseNamespace`-Parameter.</span><span class="sxs-lookup"><span data-stu-id="d5fec-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="d5fec-147">Durch diese Angabe werden Aufrufe von `GetDirectoryContents` auf diese Ressourcen unter dem angegebenen Namespace festgelegt.</span><span class="sxs-lookup"><span data-stu-id="d5fec-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="d5fec-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="d5fec-148">CompositeFileProvider</span></span>

<span data-ttu-id="d5fec-149">Der `CompositeFileProvider` kombiniert `IFileProvider`-Instanzen, die eine einzelne Schnittstelle zum Arbeiten mit Dateien von mehreren Anbietern verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="d5fec-150">Beim Erstellen des `CompositeFileProvider` übergeben Sie eine oder mehrere `IFileProvider`-Instanzen an seinen Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="d5fec-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="d5fec-151">Aktualisieren die Beispielanwendung für die Verwendung eines `CompositeFileProvider`. Hier sind zuvor konfigurierte physische und eingebettete Anbieter enthalten. Daraus ergibt sich die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="d5fec-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Die Beispielanwendung des Dateianbieters mit physischen Dateien und Ordnern sowie eingebetteten Dateien](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="d5fec-153">Erkennen von Änderungen</span><span class="sxs-lookup"><span data-stu-id="d5fec-153">Watching for changes</span></span>

<span data-ttu-id="d5fec-154">Die `IFileProvider` `Watch`-Methode bietet eine Möglichkeit, eine oder mehrere Dateien oder Verzeichnisse auf Änderungen zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="d5fec-155">Diese Methode akzeptiert eine Pfadzeichenfolge, die [Globmuster](#globbing-patterns) verwenden kann, um mehrere Dateien anzugeben und ein `IChangeToken` zurückzugeben.</span><span class="sxs-lookup"><span data-stu-id="d5fec-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="d5fec-156">Dieses Token stellt eine `HasChanged`-Eigenschaft zur Verfügung, die überprüft werden kann, und eine `RegisterChangeCallback`-Methode, die aufgerufen wird, wenn Änderungen der angegebenen Pfadzeichenfolge erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="d5fec-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="d5fec-157">Beachten Sie, dass jedes Änderungstoken nur seine zugeordneten Rückrufe als Antwort auf eine einzelne Änderung aufruft.</span><span class="sxs-lookup"><span data-stu-id="d5fec-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="d5fec-158">Zur Aktivierung der konstanten Überwachung können Sie wie unten dargestellt eine `TaskCompletionSource` verwenden oder `IChangeToken`-Instanzen als Reaktion auf Änderungen neu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="d5fec-159">Im Beispiel dieses Artikels wird eine Konsolenanwendung konfiguriert, damit sie bei einer Änderung einer Textdatei eine Meldung anzeigt:</span><span class="sxs-lookup"><span data-stu-id="d5fec-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="d5fec-160">Das Ergebnis nach mehrmaligem Speichern der Datei:</span><span class="sxs-lookup"><span data-stu-id="d5fec-160">The result, after saving the file several times:</span></span>

![Befehlsfenster nach der Ausführung von .NET zeigt die Anwendung, die nach Änderungen in der quotes.txt-Datei Ausschau hält, und dass die Datei fünf Mal geändert wurde.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="d5fec-162">Einige Dateisysteme, z.B. Docker-Container und Netzwerkfreigaben, senden Änderungsmeldungen möglicherweise nicht zuverlässig .</span><span class="sxs-lookup"><span data-stu-id="d5fec-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="d5fec-163">Legen Sie die `DOTNET_USE_POLLINGFILEWATCHER`-Umgebungsvariable auf `1` oder `true` fest, um das Dateisystem alle 4 Sekunden nach Änderungen zu untersuchen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="d5fec-164">Globmuster</span><span class="sxs-lookup"><span data-stu-id="d5fec-164">Globbing patterns</span></span>

<span data-ttu-id="d5fec-165">Dateisystempfade verwenden Platzhaltermuster namens *Globmuster*.</span><span class="sxs-lookup"><span data-stu-id="d5fec-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="d5fec-166">Diese einfachen Muster können verwendet werden, um Dateigruppen anzugeben.</span><span class="sxs-lookup"><span data-stu-id="d5fec-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="d5fec-167">Die zwei Platzhalterzeichen sind `*` und `**`.</span><span class="sxs-lookup"><span data-stu-id="d5fec-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="d5fec-168">Überprüft alles auf der aktuellen Ordnerebene, jeden Dateinamen und jede Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="d5fec-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="d5fec-169">Übereinstimmungen werden durch `/`- und `.`-Zeichen im Dateipfad beendet.</span><span class="sxs-lookup"><span data-stu-id="d5fec-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="d5fec-170">Überprüft alles auf mehrere Verzeichnisebenen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="d5fec-171">Kann verwendet werden, um rekursiv eine Übereinstimmung für viele Dateien innerhalb einer Verzeichnishierarchie zu finden.</span><span class="sxs-lookup"><span data-stu-id="d5fec-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="d5fec-172">Beispiele für Globmuster</span><span class="sxs-lookup"><span data-stu-id="d5fec-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="d5fec-173">Entspricht einer bestimmten Datei in einem bestimmten Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d5fec-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="d5fec-174">Entspricht allen Dateien mit `.txt`-Erweiterung in einem bestimmten Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="d5fec-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="d5fec-175">Entspricht allen `bower.json`-Dateien in Verzeichnissen auf der nächsttieferen Ebene des `directory`-Verzeichnisses.</span><span class="sxs-lookup"><span data-stu-id="d5fec-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="d5fec-176">Entspricht allen Dateien mit `.txt`-Erweiterung, die an einer beliebigen Stelle unter dem `directory`-Verzeichnis gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="d5fec-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="d5fec-177">Verwenden des Dateianbieters in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5fec-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="d5fec-178">Einige Komponenten von ASP.NET Core nutzen Dateianbieter.</span><span class="sxs-lookup"><span data-stu-id="d5fec-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="d5fec-179">`IHostingEnvironment` stellt das Inhaltsstammverzeichnis und das Webstammverzeichnis der Anwendung als `IFileProvider`-Typen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="d5fec-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="d5fec-180">Die Middleware der statischen Dateien verwendet Dateianbieter, um statische Dateien zu suchen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="d5fec-181">Razor nutzt `IFileProvider` intensiv für die Ansichtssuche.</span><span class="sxs-lookup"><span data-stu-id="d5fec-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="d5fec-182">Die Veröffentlichungsfunktion von .NET verwendet Dateianbieter und Globmuster, um anzugeben, welche Dateien veröffentlicht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="d5fec-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="d5fec-183">Verwendungsempfehlungen in Anwendungen</span><span class="sxs-lookup"><span data-stu-id="d5fec-183">Recommendations for use in apps</span></span>

<span data-ttu-id="d5fec-184">Wenn Ihre ASP.NET Core-Anwendung Dateisystemzugriff erfordert, können Sie über Dependeny Injection eine `IFileProvider`-Instanz anfordern und anschließend die Methoden für den Zugriff verwenden, wie in diesem Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="d5fec-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="d5fec-185">Somit können Sie den Anbieter beim Start der Anwendung einmal konfigurieren und die Anzahl der Implementierungstypen, die von Ihrer Anwendung instanziiert werden, reduzieren.</span><span class="sxs-lookup"><span data-stu-id="d5fec-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
