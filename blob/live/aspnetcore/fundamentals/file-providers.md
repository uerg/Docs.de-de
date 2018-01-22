---
title: Datei-Anbieter in ASP.NET Core
author: ardalis
description: Erfahren Sie, wie ASP.NET Core Dateisystemzugriff durch die Verwendung von Datei Anbieter abstrahiert.
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: db207f19b7ddc24dea36009138840be6efebdb84
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="4a7c3-103">Datei-Anbieter in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a7c3-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="4a7c3-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="4a7c3-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="4a7c3-105">ASP.NET Core abstrahiert Dateisystemzugriff durch die Verwendung von Datei-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="4a7c3-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a7c3-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="4a7c3-107">Datei Anbieter Abstraktionen</span><span class="sxs-lookup"><span data-stu-id="4a7c3-107">File Provider abstractions</span></span>

<span data-ttu-id="4a7c3-108">Anbieter der Datei sind eine Abstraktion über Dateisysteme.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="4a7c3-109">Ist die Hauptschnittstelle `IFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="4a7c3-110">`IFileProvider`Macht Methoden zum Abrufen von Informationen (`IFileInfo`), Verzeichnisinformationen (`IDirectoryContents`), und zum Einrichten von Benachrichtigungen (mithilfe einer `IChangeToken`).</span><span class="sxs-lookup"><span data-stu-id="4a7c3-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="4a7c3-111">`IFileInfo`Stellt Methoden und Eigenschaften für einzelne Dateien oder Verzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="4a7c3-112">Sie verfügt über zwei boolesche Eigenschaften, `Exists` und `IsDirectory`, sowie Eigenschaften, die der Datei beschreibt `Name`, `Length` (in Bytes), und `LastModified` Datum.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="4a7c3-113">Erfahren Sie aus der Datei mithilfe der `CreateReadStream` Methode.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="4a7c3-114">Datei-anbieterimplementierungen</span><span class="sxs-lookup"><span data-stu-id="4a7c3-114">File Provider implementations</span></span>

<span data-ttu-id="4a7c3-115">Drei Implementierungen des `IFileProvider` sind verfügbar: physische, eingebettet und zusammengesetzte.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="4a7c3-116">Der physische Anbieter wird verwendet, auf die tatsächlichen Systemdateien zugreifen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="4a7c3-117">Der eingebettete Anbieter wird verwendet, um den Zugriff auf Dateien, die in Assemblys eingebettet.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="4a7c3-118">Der zusammengesetzte Anbieter wird verwendet, um kombinierten Zugriff auf Dateien und Verzeichnissen von anderen Anbietern bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="4a7c3-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="4a7c3-119">PhysicalFileProvider</span></span>

<span data-ttu-id="4a7c3-120">Die `PhysicalFileProvider` ermöglicht den Zugriff auf dem physischen Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="4a7c3-121">Es dient als Wrapper für die `System.IO.File` Typ (für den physischen Anbieter), alle Pfade zu einem Verzeichnis und seinen untergeordneten Elementen Bereichsauswahl.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="4a7c3-122">Diese Bereichsdefinition schränkt den Zugriff auf einem bestimmten Verzeichnis und seinen untergeordneten Elementen, die im Dateisystem außerhalb dieser Grenze den Zugriff zu verhindern.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="4a7c3-123">Dieser Anbieter zu instanziieren, müssen Sie es mit einem Verzeichnispfad bereit dient als der Basispfad für alle Anforderungen an diesen Anbieter vorgenommen wurden (und die schränkt den Zugriff außerhalb dieser Pfad).</span><span class="sxs-lookup"><span data-stu-id="4a7c3-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="4a7c3-124">In einer app ASP.NET Core instanziieren Sie eine `PhysicalFileProvider` Anbieter direkt, oder Sie können anfordern, ein `IFileProvider` in einem Controller oder des Diensts-Konstruktor über [Abhängigkeitsinjektion](dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="4a7c3-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="4a7c3-125">Der zweite Ansatz ergibt in der Regel eine Lösung flexible und getestet werden können.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="4a7c3-126">Das folgende Beispiel zeigt, wie Sie erstellen eine `PhysicalFileProvider`.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="4a7c3-127">Sie können der Verzeichnisinhalt durchlaufen oder Abrufen von Informationen für eine bestimmte Datei durch die Bereitstellung ein Unterpfad.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="4a7c3-128">Um einen Anbieter von einem Controller anzufordern, geben Sie es im Konstruktor des Controllers aus, und weisen sie Sie ein lokales Feld.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="4a7c3-129">Verwenden Sie die lokale Instanz von Ihrem Aktionsmethoden:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="4a7c3-130">Erstellen Sie dann den Anbieter in der app `Startup` Klasse:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="4a7c3-131">In der *Index.cshtml* anzeigen, durchlaufen die `IDirectoryContents` bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="4a7c3-132">Das Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-132">The result:</span></span>

![Datei Anbieter Beispiel Anwendung Angebot physischen Dateien und Ordner](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="4a7c3-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="4a7c3-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="4a7c3-135">Die `EmbeddedFileProvider` wird verwendet, um den Zugriff auf Dateien, die in Assemblys eingebettet.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="4a7c3-136">In .NET Core, betten Sie Dateien in eine Assembly mit dem `<EmbeddedResource>` Element in der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="4a7c3-137">Sie können [Globmodus Muster](#globbing-patterns) beim Festlegen der Dateien in der Assembly eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="4a7c3-138">Diese Muster können verwendet werden, eine oder mehrere Dateien übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="4a7c3-139">Es ist unwahrscheinlich, dass Sie jemals tatsächlich jede JS-Datei in Ihr Projekt in seiner Assembly einbetten möchten. Im obigen Beispiel ist nur zu Demonstrationszwecken.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="4a7c3-140">Beim Erstellen einer `EmbeddedFileProvider`, übergeben Sie die Assembly, die sie an ihren Konstruktor gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="4a7c3-141">Der obige Codeausschnitt veranschaulicht, wie ein `EmbeddedFileProvider` mit Zugriff auf den gerade ausgeführten Assembly.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="4a7c3-142">Aktualisieren die Beispielapp für die Verwendung einer `EmbeddedFileProvider` ergibt sich die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![Datei-Anbieter-beispielanwendung, die eingebettete Dateien auflisten](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="4a7c3-144">Eingebettete Ressourcen bereit keine Verzeichnisse.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-144">Embedded resources do not expose directories.</span></span> <span data-ttu-id="4a7c3-145">Stattdessen wird der Pfad auf die Ressource (über dessen Namespace) eingebettet, in einer Filename mithilfe `.` Trennzeichen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="4a7c3-146">Die `EmbeddedFileProvider` Konstruktor akzeptiert einen optionalen `baseNamespace` Parameter.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="4a7c3-147">Durch diese Angabe wird die Aufrufe ausgeweitet `GetDirectoryContents` auf diese Ressourcen unter dem angegebenen Namespace.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="4a7c3-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="4a7c3-148">CompositeFileProvider</span></span>

<span data-ttu-id="4a7c3-149">Die `CompositeFileProvider` kombiniert `IFileProvider` Instanzen, die eine einzelne Schnittstelle zum Arbeiten mit Dateien, die von mehreren Anbietern verfügbar macht.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="4a7c3-150">Beim Erstellen der `CompositeFileProvider`, Sie übergeben eine oder mehrere `IFileProvider` Instanzen an ihren Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="4a7c3-151">Aktualisieren die Beispielapp für die Verwendung einer `CompositeFileProvider` , dass schließt sowohl die physischen und eingebettete Datenanbieter zuvor konfigurierten, ergibt sich die folgende Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![Datei-Anbieter-beispielanwendung, die sowohl für physische Dateien und eingebettete Dateien und Ordner auflisten](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="4a7c3-153">Überwachen von Änderungen</span><span class="sxs-lookup"><span data-stu-id="4a7c3-153">Watching for changes</span></span>

<span data-ttu-id="4a7c3-154">Die `IFileProvider` `Watch` Methode bietet eine Möglichkeit, eine oder mehrere Dateien oder Verzeichnisse auf Änderungen überwacht.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="4a7c3-155">Diese Methode akzeptiert eine Pfadzeichenfolge, dem können [Globmodus Muster](#globbing-patterns) an mehrere Dateien und gibt ein `IChangeToken`.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="4a7c3-156">Dieses Token stellt eine `HasChanged` -Eigenschaft, die überprüft werden kann, und eine `RegisterChangeCallback` Methode, die aufgerufen wird, wenn Änderungen an der angegebenen Pfadzeichenfolge erkannt werden.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that is called when changes are detected to the specified path string.</span></span> <span data-ttu-id="4a7c3-157">Beachten Sie, dass jede Änderungstoken zugeordneten Rückrufs nur als Antwort auf eine einzelne Änderung aufruft.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="4a7c3-158">Um die Konstante Überwachung zu aktivieren, können Sie eine `TaskCompletionSource` wie unten dargestellt oder Neuerstellen `IChangeToken` Instanzen als Reaktion auf Änderungen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="4a7c3-159">In diesem Artikel Beispiel konfiguriert eine Konsolenanwendung wird eine Meldung angezeigt, bei einer Änderung eine Textdatei:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="4a7c3-160">Das Ergebnis nach dem Speichern der Datei mehrmals:</span><span class="sxs-lookup"><span data-stu-id="4a7c3-160">The result, after saving the file several times:</span></span>

![Befehlsfenster nach Dotnet ausführen Ausführen, zeigt die Anwendung, die Überwachung der quotes.txt-Datei für die Änderungen, die die Datei verfügt über fünf Mal geändert.](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="4a7c3-162">Einige Dateisysteme, z. B. Docker-Containern und Netzwerkfreigaben, möglicherweise nicht zuverlässig Benachrichtigungen gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="4a7c3-163">Legen Sie die `DOTNET_USE_POLLINGFILEWATCHER` Umgebungsvariablen `1` oder `true` im Dateisystem nach Änderungen alle 4 Sekunden abruft.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="4a7c3-164">Globmodus-Muster</span><span class="sxs-lookup"><span data-stu-id="4a7c3-164">Globbing patterns</span></span>

<span data-ttu-id="4a7c3-165">Pfade des Dateisystems verwenden Platzhaltermustern aufgerufen *Globmodus Muster*.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="4a7c3-166">Diese einfache Muster können verwendet werden, um Gruppen von Dateien anzugeben.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="4a7c3-167">Die zwei Platzhalterzeichen sind `*` und `**`.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="4a7c3-168">Entspricht allem am aktuellen Ordnerebene oder einen Dateinamen oder eine Dateierweiterung.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="4a7c3-169">Übereinstimmungen werden durch beendet `/` und `.` Zeichen im Dateipfad.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="4a7c3-170">Entspricht allem über mehrere Verzeichnisebenen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="4a7c3-171">Kann verwendet werden, um rekursiv viele Dateien innerhalb einer Verzeichnishierarchie entsprechen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="4a7c3-172">Beispiele für Globmodus-Muster</span><span class="sxs-lookup"><span data-stu-id="4a7c3-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="4a7c3-173">Entspricht eine bestimmte Datei in einem bestimmten Verzeichnis an.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="4a7c3-174">Entspricht allen Dateien mit `.txt` Erweiterung in einem bestimmten Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="4a7c3-175">Entspricht allen `bower.json` Dateien in Verzeichnissen genau nächsttieferen Ebene aus der `directory` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="4a7c3-176">Entspricht allen Dateien mit `.txt` Erweiterung finden Sie eine beliebige Stelle unter der `directory` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="4a7c3-177">Anbieter Dateiverwendung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a7c3-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="4a7c3-178">Einige Teile der ASP.NET Core nutzen Datei Anbieter.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="4a7c3-179">`IHostingEnvironment`macht die app Inhalts-Stamm und Web-Stammverzeichnis als `IFileProvider` Typen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="4a7c3-180">Die Middleware für statische Dateien verwendet Datei Anbieter, um statische Dateien zu suchen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="4a7c3-181">Razor nutzt intensiv `IFileProvider` in Suchen von Ansichten.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="4a7c3-182">Der Dotnet veröffentlichen Funktionalität verwendet Datei Anbieter und Globmodus-Muster, um anzugeben, welche Dateien veröffentlicht werden sollen.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="4a7c3-183">Empfehlungen für apps</span><span class="sxs-lookup"><span data-stu-id="4a7c3-183">Recommendations for use in apps</span></span>

<span data-ttu-id="4a7c3-184">Wenn Ihre app ASP.NET Core Dateisystemzugriff erforderlich ist, können Sie anfordern, dass eine Instanz von `IFileProvider` über Abhängigkeitsinjektion, und verwenden Sie die Methoden für den Zugriff, wie in diesem Beispiel gezeigt.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="4a7c3-185">Dadurch können Sie zum Konfigurieren des Anbieters einmal, wenn die Anwendung wird gestartet, und die Anzahl der von Implementierungstypen reduziert, die Ihre app instanziiert wird.</span><span class="sxs-lookup"><span data-stu-id="4a7c3-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
