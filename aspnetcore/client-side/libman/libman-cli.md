---
title: Verwenden Sie die LibMan-Befehlszeilenschnittstelle (CLI) mit ASP.NET Core
author: scottaddie
description: Erfahren Sie, wie die LibMan-Befehlszeilenschnittstelle (CLI) in einem ASP.NET Core-Projekt verwenden.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/30/2018
uid: client-side/libman/libman-cli
ms.openlocfilehash: ad81af2e789a31382f50ed37754bfc94469eb197
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336036"
---
# <a name="use-the-libman-command-line-interface-cli-with-aspnet-core"></a><span data-ttu-id="aff36-103">Verwenden Sie die LibMan-Befehlszeilenschnittstelle (CLI) mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aff36-103">Use the LibMan command-line interface (CLI) with ASP.NET Core</span></span>

<span data-ttu-id="aff36-104">Von [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="aff36-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="aff36-105">Die [LibMan](xref:client-side/libman/index) -Befehlszeilenschnittstelle ist ein plattformübergreifendes Tool, die überall unterstützt .NET Core wird unterstützt.</span><span class="sxs-lookup"><span data-stu-id="aff36-105">The [LibMan](xref:client-side/libman/index) CLI is a cross-platform tool that's supported everywhere .NET Core is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aff36-106">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="aff36-106">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](../../includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="aff36-107">Installation</span><span class="sxs-lookup"><span data-stu-id="aff36-107">Installation</span></span>

<span data-ttu-id="aff36-108">So installieren Sie die LibMan CLI:</span><span class="sxs-lookup"><span data-stu-id="aff36-108">To install the LibMan CLI:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli
```

<span data-ttu-id="aff36-109">Ein [Global .NET Core-Tool](/dotnet/core/tools/global-tools#install-a-global-tool) installiert ist, aus der [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="aff36-109">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.Web.LibraryManager.Cli](https://www.nuget.org/packages/Microsoft.Web.LibraryManager.Cli/) NuGet package.</span></span>

<span data-ttu-id="aff36-110">So installieren Sie die LibMan CLI über eine bestimmte NuGet-Paketquelle</span><span class="sxs-lookup"><span data-stu-id="aff36-110">To install the LibMan CLI from a specific NuGet package source:</span></span>

```console
dotnet tool install -g Microsoft.Web.LibraryManager.Cli --version 1.0.94-g606058a278 --add-source C:\Temp\
```

<span data-ttu-id="aff36-111">Im vorherigen Beispiel ist eine .NET Core-Global-Tool aus dem lokalen Windows-Computer installiert *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* Datei.</span><span class="sxs-lookup"><span data-stu-id="aff36-111">In the preceding example, a .NET Core Global Tool is installed from the local Windows machine's *C:\Temp\Microsoft.Web.LibraryManager.Cli.1.0.94-g606058a278.nupkg* file.</span></span>

## <a name="usage"></a><span data-ttu-id="aff36-112">Verwendung</span><span class="sxs-lookup"><span data-stu-id="aff36-112">Usage</span></span>

<span data-ttu-id="aff36-113">Der folgende Befehl kann verwendet werden, nach der erfolgreichen Installation der CLI:</span><span class="sxs-lookup"><span data-stu-id="aff36-113">After successful installation of the CLI, the following command can be used:</span></span>

```console
libman
```

<span data-ttu-id="aff36-114">So zeigen Sie die installierten CLI-Version an:</span><span class="sxs-lookup"><span data-stu-id="aff36-114">To view the installed CLI version:</span></span>

```console
libman --version
```

<span data-ttu-id="aff36-115">So zeigen Sie die verfügbaren CLI-Befehlen an:</span><span class="sxs-lookup"><span data-stu-id="aff36-115">To view the available CLI commands:</span></span>

```console
libman --help
```

<span data-ttu-id="aff36-116">Der vorhergehende Befehl zeigt eine Ausgabe ähnlich der folgenden:</span><span class="sxs-lookup"><span data-stu-id="aff36-116">The preceding command displays output similar to the following:</span></span>

```console
 1.0.163+g45474d37ed

Usage: libman [options] [command]

Options:
  --help|-h  Show help information
  --version  Show version information

Commands:
  cache      List or clean libman cache contents
  clean      Deletes all library files defined in libman.json from the project
  init       Create a new libman.json
  install    Add a library definition to the libman.json file, and download the 
             library to the specified location
  restore    Downloads all files from provider and saves them to specified 
             destination
  uninstall  Deletes all files for the specified library from their specified 
             destination, then removes the specified library definition from 
             libman.json
  update     Updates the specified library

Use "libman [command] --help" for more information about a command.
```

<span data-ttu-id="aff36-117">Den folgenden Abschnitten werden die verfügbaren CLI-Befehlen.</span><span class="sxs-lookup"><span data-stu-id="aff36-117">The following sections outline the available CLI commands.</span></span>

## <a name="initialize-libman-in-the-project"></a><span data-ttu-id="aff36-118">LibMan in das Projekt initialisieren</span><span class="sxs-lookup"><span data-stu-id="aff36-118">Initialize LibMan in the project</span></span>

<span data-ttu-id="aff36-119">Die `libman init` Befehl erstellt eine *libman.json* Datei, wenn es nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="aff36-119">The `libman init` command creates a *libman.json* file if one doesn't exist.</span></span> <span data-ttu-id="aff36-120">Die Datei wird mit den standardmäßigen Element Vorlage-Inhalt erstellt.</span><span class="sxs-lookup"><span data-stu-id="aff36-120">The file is created with the default item template content.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-121">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-121">Synopsis</span></span>

```console
libman init [-d|--default-destination] [-p|--default-provider] [--verbosity]
libman init [-h|--help]
```

### <a name="options"></a><span data-ttu-id="aff36-122">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-122">Options</span></span>

<span data-ttu-id="aff36-123">Die folgenden Optionen stehen für die `libman init` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-123">The following options are available for the `libman init` command:</span></span>

* `-d|--default-destination <PATH>`

  <span data-ttu-id="aff36-124">Ein Pfad relativ zum aktuellen Ordner.</span><span class="sxs-lookup"><span data-stu-id="aff36-124">A path relative to the current folder.</span></span> <span data-ttu-id="aff36-125">Bibliotheksdateien werden an diesem Speicherort installiert, wenn keine `destination` Eigenschaft wird definiert, für eine Bibliothek in *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-125">Library files are installed in this location if no `destination` property is defined for a library in *libman.json*.</span></span> <span data-ttu-id="aff36-126">Die `<PATH>` Wert wird geschrieben, um die `defaultDestination` Eigenschaft *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-126">The `<PATH>` value is written to the `defaultDestination` property of *libman.json*.</span></span>

* `-p|--default-provider <PROVIDER>`

  <span data-ttu-id="aff36-127">Der Anbieter verwenden, wenn kein Anbieter für eine bestimmte Bibliothek definiert ist.</span><span class="sxs-lookup"><span data-stu-id="aff36-127">The provider to use if no provider is defined for a given library.</span></span> <span data-ttu-id="aff36-128">Die `<PROVIDER>` Wert wird geschrieben, um die `defaultProvider` Eigenschaft *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-128">The `<PROVIDER>` value is written to the `defaultProvider` property of *libman.json*.</span></span> <span data-ttu-id="aff36-129">Ersetzen Sie dies `<PROVIDER>` mit einem der folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="aff36-129">Replace `<PROVIDER>` with one of the following values:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-130">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-130">Examples</span></span>

<span data-ttu-id="aff36-131">Zum Erstellen einer *libman.json* -Datei in einem ASP.NET Core-Projekt:</span><span class="sxs-lookup"><span data-stu-id="aff36-131">To create a *libman.json* file in an ASP.NET Core project:</span></span>

* <span data-ttu-id="aff36-132">Navigieren Sie zu das Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="aff36-132">Navigate to the project root.</span></span>
* <span data-ttu-id="aff36-133">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="aff36-133">Run the following command:</span></span>

  ```console
  libman init
  ```

* <span data-ttu-id="aff36-134">Geben Sie den Namen des Standardanbieters oder drücken Sie `Enter` den CDNJS-Standardanbieter verwendet.</span><span class="sxs-lookup"><span data-stu-id="aff36-134">Type the name of the default provider, or press `Enter` to use the default CDNJS provider.</span></span> <span data-ttu-id="aff36-135">Gültige Werte sind:</span><span class="sxs-lookup"><span data-stu-id="aff36-135">Valid values include:</span></span>

  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  ![Libman Init-Befehl – Standard-Anbieter](_static/libman-init-provider.png)

<span data-ttu-id="aff36-137">Ein *libman.json* Datei wird hinzugefügt, um das Stammverzeichnis des Projekts mit folgendem Inhalt:</span><span class="sxs-lookup"><span data-stu-id="aff36-137">A *libman.json* file is added to the project root with the following content:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

## <a name="add-library-files"></a><span data-ttu-id="aff36-138">Fügen Sie Bibliotheksdateien</span><span class="sxs-lookup"><span data-stu-id="aff36-138">Add library files</span></span>

<span data-ttu-id="aff36-139">Die `libman install` Befehl heruntergeladen und installiert Sie Bibliotheksdateien in das Projekt.</span><span class="sxs-lookup"><span data-stu-id="aff36-139">The `libman install` command downloads and installs library files into the project.</span></span> <span data-ttu-id="aff36-140">Ein *libman.json* Datei wird hinzugefügt, wenn es nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="aff36-140">A *libman.json* file is added if one doesn't exist.</span></span> <span data-ttu-id="aff36-141">Die *libman.json* Datei wird geändert, um die Konfigurationsdetails für die Bibliotheksdateien speichern.</span><span class="sxs-lookup"><span data-stu-id="aff36-141">The *libman.json* file is modified to store configuration details for the library files.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-142">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-142">Synopsis</span></span>

```console
libman install <LIBRARY> [-d|--destination] [--files] [-p|--provider] [--verbosity]
libman install [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="aff36-143">Argumente</span><span class="sxs-lookup"><span data-stu-id="aff36-143">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="aff36-144">Der Name der Bibliothek zu installieren.</span><span class="sxs-lookup"><span data-stu-id="aff36-144">The name of the library to install.</span></span> <span data-ttu-id="aff36-145">Dieser Name kann die Anzahl-versionsnotation enthalten (z. B. `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="aff36-145">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="aff36-146">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-146">Options</span></span>

<span data-ttu-id="aff36-147">Die folgenden Optionen stehen für die `libman install` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-147">The following options are available for the `libman install` command:</span></span>

* `-d|--destination <PATH>`

  <span data-ttu-id="aff36-148">Der Speicherort, der die Bibliothek zu installieren.</span><span class="sxs-lookup"><span data-stu-id="aff36-148">The location to install the library.</span></span> <span data-ttu-id="aff36-149">Wenn nicht angegeben, wird der Standardspeicherort verwendet.</span><span class="sxs-lookup"><span data-stu-id="aff36-149">If not specified, the default location is used.</span></span> <span data-ttu-id="aff36-150">Wenn kein `defaultDestination` -Eigenschaft angegeben wird, im *libman.json*, diese Option ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="aff36-150">If no `defaultDestination` property is specified in *libman.json*, this option is required.</span></span>

* `--files <FILE>`

  <span data-ttu-id="aff36-151">Geben Sie den Namen der Datei, die aus der Bibliothek zu installieren.</span><span class="sxs-lookup"><span data-stu-id="aff36-151">Specify the name of the file to install from the library.</span></span> <span data-ttu-id="aff36-152">Wenn nicht angegeben, werden alle Dateien aus der Bibliothek installiert.</span><span class="sxs-lookup"><span data-stu-id="aff36-152">If not specified, all files from the library are installed.</span></span> <span data-ttu-id="aff36-153">Geben Sie einen `--files` Option pro Datei installiert werden.</span><span class="sxs-lookup"><span data-stu-id="aff36-153">Provide one `--files` option per file to be installed.</span></span> <span data-ttu-id="aff36-154">Relative Pfade werden ebenfalls unterstützt.</span><span class="sxs-lookup"><span data-stu-id="aff36-154">Relative paths are supported too.</span></span> <span data-ttu-id="aff36-155">Beispiel: `--files dist/browser/signalr.js`.</span><span class="sxs-lookup"><span data-stu-id="aff36-155">For example: `--files dist/browser/signalr.js`.</span></span>

* `-p|--provider <PROVIDER>`

  <span data-ttu-id="aff36-156">Der Name des Anbieters, um für die Übernahme der Bibliothek verwenden.</span><span class="sxs-lookup"><span data-stu-id="aff36-156">The name of the provider to use for the library acquisition.</span></span> <span data-ttu-id="aff36-157">Ersetzen Sie dies `<PROVIDER>` mit einem der folgenden Werte:</span><span class="sxs-lookup"><span data-stu-id="aff36-157">Replace `<PROVIDER>` with one of the following values:</span></span>
  
  [!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

  <span data-ttu-id="aff36-158">Wenn nicht angegeben, die `defaultProvider` -Eigenschaft in *libman.json* verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="aff36-158">If not specified, the `defaultProvider` property in *libman.json* is used.</span></span> <span data-ttu-id="aff36-159">Wenn kein `defaultProvider` -Eigenschaft angegeben wird, im *libman.json*, diese Option ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="aff36-159">If no `defaultProvider` property is specified in *libman.json*, this option is required.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-160">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-160">Examples</span></span>

<span data-ttu-id="aff36-161">Beachten Sie Folgendes *libman.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="aff36-161">Consider the following *libman.json* file:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": []
}
```

<span data-ttu-id="aff36-162">So installieren Sie die jQuery-Version 3.2.1 *"jQuery.Min.js"* -Datei in die *Wwwroot\scripts\jquery* Ordner mit dem Anbieter CDNJS:</span><span class="sxs-lookup"><span data-stu-id="aff36-162">To install the jQuery version 3.2.1 *jquery.min.js* file to the *wwwroot\scripts\jquery* folder using the CDNJS provider:</span></span>

```console
libman install jquery@3.2.1 --provider cdnjs --destination wwwroot\scripts\jquery --files jquery.min.js
```

<span data-ttu-id="aff36-163">Die *libman.json* Datei ähnelt der folgenden:</span><span class="sxs-lookup"><span data-stu-id="aff36-163">The *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    }
  ]
}
```

<span data-ttu-id="aff36-164">So installieren Sie die *calendar.js* und *calendar.css* Dateien von *C:\\Temp\\ContosoCalendar\\*  mit dem Dateisystem Anbieter:</span><span class="sxs-lookup"><span data-stu-id="aff36-164">To install the *calendar.js* and *calendar.css* files from *C:\\temp\\contosoCalendar\\* using the file system provider:</span></span>

  ```console
  libman install C:\temp\contosoCalendar\ --provider filesystem --files calendar.js --files calendar.css
  ```

<span data-ttu-id="aff36-165">Die folgende Aufforderung angezeigt wird, aus zwei Gründen:</span><span class="sxs-lookup"><span data-stu-id="aff36-165">The following prompt appears for two reasons:</span></span>

* <span data-ttu-id="aff36-166">Die *libman.json* Datei keine `defaultDestination` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="aff36-166">The *libman.json* file doesn't contain a `defaultDestination` property.</span></span>
* <span data-ttu-id="aff36-167">Die `libman install` Befehl enthält nicht die `-d|--destination` Option.</span><span class="sxs-lookup"><span data-stu-id="aff36-167">The `libman install` command doesn't contain the `-d|--destination` option.</span></span>

![Libman Installationsbefehl - Ziel](_static/libman-install-destination.png)

<span data-ttu-id="aff36-169">Nach dem Akzeptieren des Standardziel, dem *libman.json* Datei ähnelt der folgenden:</span><span class="sxs-lookup"><span data-stu-id="aff36-169">After accepting the default destination, the *libman.json* file resembles the following:</span></span>

```json
{
  "version": "1.0",
  "defaultProvider": "cdnjs",
  "libraries": [
    {
      "library": "jquery@3.2.1",
      "destination": "wwwroot\\scripts\\jquery",
      "files": [
        "jquery.min.js"
      ]
    },
    {
      "library": "C:\\temp\\contosoCalendar\\",
      "provider": "filesystem",
      "destination": "wwwroot\\lib\\contosoCalendar",
      "files": [
        "calendar.js",
        "calendar.css"
      ]
    }
  ]
}
```

## <a name="restore-library-files"></a><span data-ttu-id="aff36-170">Wiederherstellen von Bibliotheksdateien</span><span class="sxs-lookup"><span data-stu-id="aff36-170">Restore library files</span></span>

<span data-ttu-id="aff36-171">Die `libman restore` Befehl installiert die Bibliotheksdateien, die in definierten *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-171">The `libman restore` command installs library files defined in *libman.json*.</span></span> <span data-ttu-id="aff36-172">Dabei gelten folgende Regeln:</span><span class="sxs-lookup"><span data-stu-id="aff36-172">The following rules apply:</span></span>

* <span data-ttu-id="aff36-173">Wenn kein *libman.json* Datei in das Stammverzeichnis des Projekts vorhanden ist, wird ein Fehler zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="aff36-173">If no *libman.json* file exists in the project root, an error is returned.</span></span>
* <span data-ttu-id="aff36-174">Wenn eine Bibliothek gibt an, einen Anbieter, der `defaultProvider` -Eigenschaft in *libman.json* wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="aff36-174">If a library specifies a provider, the `defaultProvider` property in *libman.json* is ignored.</span></span>
* <span data-ttu-id="aff36-175">Wenn eine Bibliothek ein Ziel, gibt die `defaultDestination` -Eigenschaft in *libman.json* wird ignoriert.</span><span class="sxs-lookup"><span data-stu-id="aff36-175">If a library specifies a destination, the `defaultDestination` property in *libman.json* is ignored.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-176">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-176">Synopsis</span></span>

```console
libman restore [--verbosity]
libman restore [-h|--help]
```

### <a name="options"></a><span data-ttu-id="aff36-177">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-177">Options</span></span>

<span data-ttu-id="aff36-178">Die folgenden Optionen stehen für die `libman restore` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-178">The following options are available for the `libman restore` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-179">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-179">Examples</span></span>

<span data-ttu-id="aff36-180">Zum Wiederherstellen der Library-Dateien, die in definierten *libman.json*:</span><span class="sxs-lookup"><span data-stu-id="aff36-180">To restore the library files defined in *libman.json*:</span></span>

```console
libman restore
```

## <a name="delete-library-files"></a><span data-ttu-id="aff36-181">Löschen Sie Bibliotheksdateien</span><span class="sxs-lookup"><span data-stu-id="aff36-181">Delete library files</span></span>

<span data-ttu-id="aff36-182">Die `libman clean` Befehl löscht die Bibliotheksdateien, die zuvor über LibMan wiederhergestellt.</span><span class="sxs-lookup"><span data-stu-id="aff36-182">The `libman clean` command deletes library files previously restored via LibMan.</span></span> <span data-ttu-id="aff36-183">Ordner, die nach diesem Vorgang leer sind, werden gelöscht.</span><span class="sxs-lookup"><span data-stu-id="aff36-183">Folders that become empty after this operation are deleted.</span></span> <span data-ttu-id="aff36-184">Der Bibliotheksdateien verknüpft ist, Konfigurationen in der `libraries` Eigenschaft *libman.json* werden nicht entfernt.</span><span class="sxs-lookup"><span data-stu-id="aff36-184">The library files' associated configurations in the `libraries` property of *libman.json* aren't removed.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-185">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-185">Synopsis</span></span>

```console
libman clean [--verbosity]
libman clean [-h|--help]
```

### <a name="options"></a><span data-ttu-id="aff36-186">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-186">Options</span></span>

<span data-ttu-id="aff36-187">Die folgenden Optionen stehen für die `libman clean` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-187">The following options are available for the `libman clean` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-188">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-188">Examples</span></span>

<span data-ttu-id="aff36-189">So löschen Sie Bibliotheksdateien, die über LibMan installiert:</span><span class="sxs-lookup"><span data-stu-id="aff36-189">To delete library files installed via LibMan:</span></span>

```console
libman clean
```

## <a name="uninstall-library-files"></a><span data-ttu-id="aff36-190">Deinstallieren Sie Bibliotheksdateien</span><span class="sxs-lookup"><span data-stu-id="aff36-190">Uninstall library files</span></span>

<span data-ttu-id="aff36-191">Die `libman uninstall` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-191">The `libman uninstall` command:</span></span>

* <span data-ttu-id="aff36-192">Löscht alle Dateien im Zusammenhang mit der angegebenen Bibliothek aus dem Ziel in *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-192">Deletes all files associated with the specified library from the destination in *libman.json*.</span></span>
* <span data-ttu-id="aff36-193">Entfernt den zugeordneten Netzwerkbibliotheks-Konfiguration von *libman.json*.</span><span class="sxs-lookup"><span data-stu-id="aff36-193">Removes the associated library configuration from *libman.json*.</span></span>

<span data-ttu-id="aff36-194">Ein Fehler tritt auf, wenn:</span><span class="sxs-lookup"><span data-stu-id="aff36-194">An error occurs when:</span></span>

* <span data-ttu-id="aff36-195">Keine *libman.json* Datei vorhanden ist, in das Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="aff36-195">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="aff36-196">Die angegebene Bibliothek ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="aff36-196">The specified library doesn't exist.</span></span>

<span data-ttu-id="aff36-197">Wenn mehr als eine Bibliothek mit demselben Namen installiert ist, werden Sie aufgefordert, eine auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="aff36-197">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-198">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-198">Synopsis</span></span>

```console
libman uninstall <LIBRARY> [--verbosity]
libman uninstall [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="aff36-199">Argumente</span><span class="sxs-lookup"><span data-stu-id="aff36-199">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="aff36-200">Der Name der Bibliothek zu deinstallieren.</span><span class="sxs-lookup"><span data-stu-id="aff36-200">The name of the library to uninstall.</span></span> <span data-ttu-id="aff36-201">Dieser Name kann die Anzahl-versionsnotation enthalten (z. B. `@1.2.0`).</span><span class="sxs-lookup"><span data-stu-id="aff36-201">This name may include version number notation (for example, `@1.2.0`).</span></span>

### <a name="options"></a><span data-ttu-id="aff36-202">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-202">Options</span></span>

<span data-ttu-id="aff36-203">Die folgenden Optionen stehen für die `libman uninstall` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-203">The following options are available for the `libman uninstall` command:</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-204">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-204">Examples</span></span>

<span data-ttu-id="aff36-205">Beachten Sie Folgendes *libman.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="aff36-205">Consider the following *libman.json* file:</span></span>

[!code-json[](samples/LibManSample/libman.json)]

* <span data-ttu-id="aff36-206">Erfolgreich ausgeführt um jQuery zu deinstallieren, einen der folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="aff36-206">To uninstall jQuery, either of the following commands succeed:</span></span>

  ```console
  libman uninstall jquery
  ```

  ```console
  libman uninstall jquery@3.3.1
  ```

* <span data-ttu-id="aff36-207">So deinstallieren Sie die Lodash-Dateien installiert, über die `filesystem` Anbieter:</span><span class="sxs-lookup"><span data-stu-id="aff36-207">To uninstall the Lodash files installed via the `filesystem` provider:</span></span>

  ```console
  libman uninstall C:\temp\lodash\
  ```

## <a name="update-library-version"></a><span data-ttu-id="aff36-208">Update-Bibliotheksversion</span><span class="sxs-lookup"><span data-stu-id="aff36-208">Update library version</span></span>

<span data-ttu-id="aff36-209">Die `libman update` Befehl wird eine Bibliothek, die über LibMan installiert wird, der angegebenen Version aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="aff36-209">The `libman update` command updates a library installed via LibMan to the specified version.</span></span>

<span data-ttu-id="aff36-210">Ein Fehler tritt auf, wenn:</span><span class="sxs-lookup"><span data-stu-id="aff36-210">An error occurs when:</span></span>

* <span data-ttu-id="aff36-211">Keine *libman.json* Datei vorhanden ist, in das Stammverzeichnis des Projekts.</span><span class="sxs-lookup"><span data-stu-id="aff36-211">No *libman.json* file exists in the project root.</span></span>
* <span data-ttu-id="aff36-212">Die angegebene Bibliothek ist nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="aff36-212">The specified library doesn't exist.</span></span>

<span data-ttu-id="aff36-213">Wenn mehr als eine Bibliothek mit demselben Namen installiert ist, werden Sie aufgefordert, eine auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="aff36-213">If more than one library with the same name is installed, you're prompted to choose one.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-214">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-214">Synopsis</span></span>

```console
libman update <LIBRARY> [-pre] [--to] [--verbosity]
libman update [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="aff36-215">Argumente</span><span class="sxs-lookup"><span data-stu-id="aff36-215">Arguments</span></span>

`LIBRARY`

<span data-ttu-id="aff36-216">Der Name der Bibliothek aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="aff36-216">The name of the library to update.</span></span>

### <a name="options"></a><span data-ttu-id="aff36-217">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-217">Options</span></span>

<span data-ttu-id="aff36-218">Die folgenden Optionen stehen für die `libman update` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-218">The following options are available for the `libman update` command:</span></span>

* `-pre`

  <span data-ttu-id="aff36-219">Rufen Sie die aktuelle Vorabversion der Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="aff36-219">Obtain the latest prerelease version of the library.</span></span>

* `--to <VERSION>`

  <span data-ttu-id="aff36-220">Rufen Sie eine bestimmte Version der Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="aff36-220">Obtain a specific version of the library.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-221">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-221">Examples</span></span>

* <span data-ttu-id="aff36-222">JQuery auf die neueste Version zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="aff36-222">To update jQuery to the latest version:</span></span>

  ```console
  libman update jquery
  ```

* <span data-ttu-id="aff36-223">So aktualisieren Sie jQuery Version 3.3.1:</span><span class="sxs-lookup"><span data-stu-id="aff36-223">To update jQuery to version 3.3.1:</span></span>

  ```console
  libman update jquery --to 3.3.1
  ```

* <span data-ttu-id="aff36-224">JQuery auf die aktuelle Vorabversion zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="aff36-224">To update jQuery to the latest prerelease version:</span></span>

  ```console
  libman update jquery -pre
  ```

## <a name="manage-library-cache"></a><span data-ttu-id="aff36-225">Verwalten von Bibliothek-cache</span><span class="sxs-lookup"><span data-stu-id="aff36-225">Manage library cache</span></span>

<span data-ttu-id="aff36-226">Die `libman cache` Befehl den LibMan-Bibliothek-Cache verwaltet.</span><span class="sxs-lookup"><span data-stu-id="aff36-226">The `libman cache` command manages the LibMan library cache.</span></span> <span data-ttu-id="aff36-227">Die `filesystem` -Anbieter nicht den Cache für die Bibliothek verwenden.</span><span class="sxs-lookup"><span data-stu-id="aff36-227">The `filesystem` provider doesn't use the library cache.</span></span>

### <a name="synopsis"></a><span data-ttu-id="aff36-228">Übersicht</span><span class="sxs-lookup"><span data-stu-id="aff36-228">Synopsis</span></span>

```console
libman cache clean [<PROVIDER>] [--verbosity]
libman cache list [--files] [--libraries] [--verbosity]
libman cache [-h|--help]
```

### <a name="arguments"></a><span data-ttu-id="aff36-229">Argumente</span><span class="sxs-lookup"><span data-stu-id="aff36-229">Arguments</span></span>

`PROVIDER`

<span data-ttu-id="aff36-230">Nur der Verwendung der `clean` Befehl.</span><span class="sxs-lookup"><span data-stu-id="aff36-230">Only used with the `clean` command.</span></span> <span data-ttu-id="aff36-231">Gibt an, die der Cache bereinigt.</span><span class="sxs-lookup"><span data-stu-id="aff36-231">Specifies the provider cache to clean.</span></span> <span data-ttu-id="aff36-232">Gültige Werte sind:</span><span class="sxs-lookup"><span data-stu-id="aff36-232">Valid values include:</span></span>

[!INCLUDE [LibMan provider names](../../includes/libman-cli/provider-names.md)]

### <a name="options"></a><span data-ttu-id="aff36-233">Optionen</span><span class="sxs-lookup"><span data-stu-id="aff36-233">Options</span></span>

<span data-ttu-id="aff36-234">Die folgenden Optionen stehen für die `libman cache` Befehl:</span><span class="sxs-lookup"><span data-stu-id="aff36-234">The following options are available for the `libman cache` command:</span></span>

* `--files`

  <span data-ttu-id="aff36-235">Listet die Namen der Dateien, die zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="aff36-235">List the names of files that are cached.</span></span>

* `--libraries`

  <span data-ttu-id="aff36-236">Listet die Namen von Bibliotheken, die zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="aff36-236">List the names of libraries that are cached.</span></span>

[!INCLUDE [standard-cli-options](../../includes/libman-cli/standard-cli-options.md)]

### <a name="examples"></a><span data-ttu-id="aff36-237">Beispiele</span><span class="sxs-lookup"><span data-stu-id="aff36-237">Examples</span></span>

* <span data-ttu-id="aff36-238">Verwenden Sie zum Anzeigen der Namen von zwischengespeicherten Bibliotheken pro Anbieter, eine der folgenden Befehle aus:</span><span class="sxs-lookup"><span data-stu-id="aff36-238">To view the names of cached libraries per provider, use one of the following commands:</span></span>

  ```console
  libman cache list
  ```

  ```console
  libman cache list --libraries
  ```

  <span data-ttu-id="aff36-239">Dadurch werden Informationen angezeigt, die mit denen der folgenden Ausgabe vergleichbar sind:</span><span class="sxs-lookup"><span data-stu-id="aff36-239">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      font-awesome
      jquery
      knockout
      lodash.js
      react
  ```

* <span data-ttu-id="aff36-240">So zeigen Sie die Namen der zwischengespeicherten Bibliotheksdateien pro Anbieter an:</span><span class="sxs-lookup"><span data-stu-id="aff36-240">To view the names of cached library files per provider:</span></span>

  ```console
  libman cache list --files
  ```

  <span data-ttu-id="aff36-241">Dadurch werden Informationen angezeigt, die mit denen der folgenden Ausgabe vergleichbar sind:</span><span class="sxs-lookup"><span data-stu-id="aff36-241">Output similar to the following is displayed:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout:
          <list omitted for brevity>
      react:
          <list omitted for brevity>
      vue:
          <list omitted for brevity>
  cdnjs:
      font-awesome
          metadata.json
      jquery
          metadata.json
          3.2.1\core.js
          3.2.1\jquery.js
          3.2.1\jquery.min.js
          3.2.1\jquery.min.map
          3.2.1\jquery.slim.js
          3.2.1\jquery.slim.min.js
          3.2.1\jquery.slim.min.map
          3.3.1\core.js
          3.3.1\jquery.js
          3.3.1\jquery.min.js
          3.3.1\jquery.min.map
          3.3.1\jquery.slim.js
          3.3.1\jquery.slim.min.js
          3.3.1\jquery.slim.min.map
      knockout
          metadata.json
          3.4.2\knockout-debug.js
          3.4.2\knockout-min.js
      lodash.js
          metadata.json
          4.17.10\lodash.js
          4.17.10\lodash.min.js
      react
          metadata.json
  ```

  <span data-ttu-id="aff36-242">Beachten Sie, dass der obigen Ausgabe zeigt, dass jQuery, Version 3.2.1 und 3.3.1 unter dem Anbieter CDNJS zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="aff36-242">Notice the preceding output shows that jQuery versions 3.2.1 and 3.3.1 are cached under the CDNJS provider.</span></span>

* <span data-ttu-id="aff36-243">Um den Cache für die Bibliothek für den Anbieter CDNJS zu leeren:</span><span class="sxs-lookup"><span data-stu-id="aff36-243">To empty the library cache for the CDNJS provider:</span></span>

  ```console
  libman cache clean cdnjs
  ```

  <span data-ttu-id="aff36-244">Nach dem leeren des Caches CDNJS-Anbieter, der `libman cache list` Befehl werden folgende Informationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="aff36-244">After emptying the CDNJS provider cache, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      knockout
      react
      vue
  cdnjs:
      (empty)
  ```

* <span data-ttu-id="aff36-245">Zum Leeren des Caches für alle unterstützten Anbieter:</span><span class="sxs-lookup"><span data-stu-id="aff36-245">To empty the cache for all supported providers:</span></span>

  ```console
  libman cache clean
  ```

  <span data-ttu-id="aff36-246">Nach dem leeren des Caches für alle Anbieter, der `libman cache list` Befehl werden folgende Informationen angezeigt:</span><span class="sxs-lookup"><span data-stu-id="aff36-246">After emptying all provider caches, the `libman cache list` command displays the following:</span></span>

  ```console
  Cache contents:
  ---------------
  unpkg:
      (empty)
  cdnjs:
      (empty)
  ```

## <a name="additional-resources"></a><span data-ttu-id="aff36-247">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="aff36-247">Additional resources</span></span>

* [<span data-ttu-id="aff36-248">Installieren Sie ein globales Tool</span><span class="sxs-lookup"><span data-stu-id="aff36-248">Install a Global Tool</span></span>](/dotnet/core/tools/global-tools#install-a-global-tool)
* <xref:client-side/libman/libman-vs>
* [<span data-ttu-id="aff36-249">GitHub-Repository für LibMan</span><span class="sxs-lookup"><span data-stu-id="aff36-249">LibMan GitHub repository</span></span>](https://github.com/aspnet/LibraryManager)
