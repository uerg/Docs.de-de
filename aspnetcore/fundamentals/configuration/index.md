---
title: Konfiguration in ASP.NET Core
author: rick-anderson
description: In diesem Artikel erfahren Sie, wie Sie mit der Konfigurations-API eine ASP.NET Core-App konfigurieren.
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
uid: fundamentals/configuration/index
ms.openlocfilehash: 59ab0cd0f6975d15bd01ce7e4128521938182c24
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228623"
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="a386f-103">Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a386f-103">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="a386f-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a386f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a386f-105">Mithilfe der Konfigurations-API kann eine ASP.NET Core-Web-App basierend auf einer Liste von Name/Wert-Paaren konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="a386f-106">Die Konfiguration wird zur Runtime aus verschiedenen Quellen gelesen.</span><span class="sxs-lookup"><span data-stu-id="a386f-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="a386f-107">Name-Wert-Paare können in eine Hierarchie mit mehreren Ebenen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="a386f-108">Für Folgendes stehen Konfigurationsanbieter zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="a386f-108">There are configuration providers for:</span></span>

* <span data-ttu-id="a386f-109">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="a386f-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="a386f-110">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="a386f-110">Command-line arguments.</span></span>
* <span data-ttu-id="a386f-111">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="a386f-111">Environment variables.</span></span>
* <span data-ttu-id="a386f-112">Speicherinterne .NET-Objekte</span><span class="sxs-lookup"><span data-stu-id="a386f-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="a386f-113">Der unverschlüsselte Speicher von [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a386f-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="a386f-114">Ein unverschlüsselter Benutzerspeicher, z.B. [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="a386f-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="a386f-115">Benutzerdefinierte Anbieter (installiert oder erstellt).</span><span class="sxs-lookup"><span data-stu-id="a386f-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="a386f-116">Jeder Konfigurationswert ist einem Zeichenfolgenschlüssel zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="a386f-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="a386f-117">Für die Deserialisierung von Einstellungen in ein benutzerdefiniertes [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekt (eine einfache .NET-Klasse mit Eigenschaften) kann auf eine integrierte Bindungsunterstützung zurückgegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="a386f-118">Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar.</span><span class="sxs-lookup"><span data-stu-id="a386f-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="a386f-119">Weitere Informationen zur Verwendung des Optionsmusters finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a386f-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="a386f-120">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a386f-120">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="a386f-121">Für die in diesem Artikel genannten Beispiele müssen folgende Bedingungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="a386f-121">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="a386f-122">Der Basispfad der App muss mit [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) festgelegt sein.</span><span class="sxs-lookup"><span data-stu-id="a386f-122">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="a386f-123">Die App kann auf `SetBasePath` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-123">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="a386f-124">Abschnitte von Konfigurationsdateien müssen mit [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-124">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="a386f-125">Die App kann auf `GetSection` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-125">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="a386f-126">Es muss eine Bindungskonfiguration mit [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-126">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="a386f-127">Die App kann auf `Bind` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-127">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="a386f-128">Diese Pakete sind im Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) enthalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-128">These packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="a386f-129">Für die in diesem Artikel genannten Beispiele müssen folgende Bedingungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="a386f-129">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="a386f-130">Der Basispfad der App muss mit [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) festgelegt sein.</span><span class="sxs-lookup"><span data-stu-id="a386f-130">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="a386f-131">Die App kann auf `SetBasePath` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-131">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="a386f-132">Abschnitte von Konfigurationsdateien müssen mit [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-132">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="a386f-133">Die App kann auf `GetSection` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-133">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="a386f-134">Es muss eine Bindungskonfiguration mit [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-134">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="a386f-135">Die App kann auf `Bind` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-135">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

<span data-ttu-id="a386f-136">Diese Pakete sind im Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) enthalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-136">These packages are included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="a386f-137">Für die in diesem Artikel genannten Beispiele müssen folgende Bedingungen erfüllt sein:</span><span class="sxs-lookup"><span data-stu-id="a386f-137">Examples provided in this topic rely upon:</span></span>

* <span data-ttu-id="a386f-138">Der Basispfad der App muss mit [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) festgelegt sein.</span><span class="sxs-lookup"><span data-stu-id="a386f-138">Setting the base path of the app with [SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath).</span></span> <span data-ttu-id="a386f-139">Die App kann auf `SetBasePath` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-139">`SetBasePath` is made available to the app by referencing the [Microsoft.Extensions.Configuration.FileExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.FileExtensions/) package.</span></span>
* <span data-ttu-id="a386f-140">Abschnitte von Konfigurationsdateien müssen mit [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection) aufgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-140">Resolving sections of configuration files with [GetSection](/dotnet/api/microsoft.extensions.configuration.configurationsection.getsection).</span></span> <span data-ttu-id="a386f-141">Die App kann auf `GetSection` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-141">`GetSection` is made available to the app by referencing the [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration/) package.</span></span>
* <span data-ttu-id="a386f-142">Es muss eine Bindungskonfiguration mit [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind) hergestellt werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-142">Binding configuration with [Bind](/dotnet/api/microsoft.extensions.configuration.configurationbinder.bind).</span></span> <span data-ttu-id="a386f-143">Die App kann auf `Bind` zugreifen, wenn ein Verweis auf das Paket [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) hergestellt wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-143">`Bind` is made available to the app by referencing the [Microsoft.Extensions.Configuration.Binder](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Binder/) package.</span></span>

::: moniker-end

## <a name="json-configuration"></a><span data-ttu-id="a386f-144">JSON-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a386f-144">JSON configuration</span></span>

<span data-ttu-id="a386f-145">Die folgende Konsolen-App verwendet den JSON-Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="a386f-145">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="a386f-146">Die App liest die folgenden Konfigurationseinstellungen und zeigt diese an:</span><span class="sxs-lookup"><span data-stu-id="a386f-146">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="a386f-147">Eine Konfiguration besteht aus einer hierarchischen Liste von Name/Wert-Paaren, in denen die Knoten durch einen Doppelpunkt (`:`) getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="a386f-147">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon (`:`).</span></span> <span data-ttu-id="a386f-148">Um einen Wert abzurufen, rufen Sie mit dem entsprechenden Schlüssel des Elements den `Configuration`-Indexer auf:</span><span class="sxs-lookup"><span data-stu-id="a386f-148">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="a386f-149">Verwenden Sie für die Arbeit mit Arrays in JSON-formatierten Konfigurationsquellen einen Arrayindex als Teil der durch Doppelpunkte getrennten Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="a386f-149">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="a386f-150">Im folgenden Beispiel wird der Name des ersten Elements im vorherigen `wizards`-Array abgerufen:</span><span class="sxs-lookup"><span data-stu-id="a386f-150">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="a386f-151">Name/Wert-Paare, die in die integrierten [Konfigurationsanbieter](/dotnet/api/microsoft.extensions.configuration) geschrieben werden, werden **nicht** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-151">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="a386f-152">Allerdings kann ein benutzerdefinierter Anbieter erstellt werden, der Werte speichert.</span><span class="sxs-lookup"><span data-stu-id="a386f-152">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="a386f-153">Weitere Informationen finden Sie im Artikel zum [benutzerdefinierten Konfigurationsanbieter](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="a386f-153">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="a386f-154">Im vorhergehenden Beispiel wird der Konfigurationsindexer zum Lesen von Werten verwendet.</span><span class="sxs-lookup"><span data-stu-id="a386f-154">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="a386f-155">Um außerhalb von `Startup` auf die Konfiguration zuzugreifen, verwenden Sie das *Optionsmuster*.</span><span class="sxs-lookup"><span data-stu-id="a386f-155">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="a386f-156">Weitere Informationen finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="a386f-156">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="a386f-157">XML-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a386f-157">XML configuration</span></span>

<span data-ttu-id="a386f-158">Stellen Sie einen `name`-Index für jedes Element bereit, um mit Arrays in Konfigurationsquellen im XML-Format zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="a386f-158">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="a386f-159">Verwenden Sie den Index, um auf die Werte zuzugreifen:</span><span class="sxs-lookup"><span data-stu-id="a386f-159">Use the index to access the values:</span></span>

```xml
<wizards>
  <wizard name="Gandalf">
    <age>1000</age>
  </wizard>
  <wizard name="Harry">
    <age>17</age>
  </wizard>
</wizards>
```

```csharp
Console.Write($"{Configuration["wizard:Harry:age"]}");
// Output: 17
```

## <a name="configuration-by-environment"></a><span data-ttu-id="a386f-160">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="a386f-160">Configuration by environment</span></span>

<span data-ttu-id="a386f-161">Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen (z.B. für Entwicklung, Tests und Produktion) zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-161">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="a386f-162">Durch die Erweiterungsmethode `CreateDefaultBuilder` in einer ASP.NET Core 2.x-App (oder durch die direkte Verwendung von `AddJsonFile` und `AddEnvironmentVariables` in einer ASP.NET Core 1.x-App) werden Konfigurationsanbieter zum Lesen von JSON-Dateien und Systemkonfigurationsquellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="a386f-162">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="a386f-163">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="a386f-163">*appsettings.json*</span></span>
* <span data-ttu-id="a386f-164">*appsettings.\<Umgebungsname>.json*</span><span class="sxs-lookup"><span data-stu-id="a386f-164">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="a386f-165">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="a386f-165">Environment variables</span></span>

<span data-ttu-id="a386f-166">ASP.NET Core 1.x-Apps müssen `AddJsonFile` und [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a386f-166">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="a386f-167">Eine Erläuterung der Parameter finden Sie unter [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="a386f-167">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="a386f-168">`reloadOnChange` wird nur in ASP.NET Core 1.1 und höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a386f-168">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="a386f-169">Konfigurationsquellen werden in der Reihenfolge, in der sie angegeben sind, gelesen.</span><span class="sxs-lookup"><span data-stu-id="a386f-169">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="a386f-170">Im vorangehenden Code werden die Umgebungsvariablen zuletzt gelesen.</span><span class="sxs-lookup"><span data-stu-id="a386f-170">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="a386f-171">Alle über die Umgebung festgelegten Konfigurationswerte ersetzen diejenigen in den beiden vorherigen Anbietern.</span><span class="sxs-lookup"><span data-stu-id="a386f-171">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="a386f-172">Beachten Sie die folgende *appsettings.Staging.json*-Datei:</span><span class="sxs-lookup"><span data-stu-id="a386f-172">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="a386f-173">Im folgenden Code wird der Wert von `Configure` auf `MyConfig` festgelegt:</span><span class="sxs-lookup"><span data-stu-id="a386f-173">In the following code, `Configure` reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]

<span data-ttu-id="a386f-174">Die Umgebung ist in der Regel auf `Development`, `Staging` oder `Production` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="a386f-174">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="a386f-175">Weitere Informationen finden Sie unter [Verwenden mehrerer Umgebungen](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="a386f-175">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="a386f-176">Überlegungen zur Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="a386f-176">Configuration considerations:</span></span>

* <span data-ttu-id="a386f-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) kann Konfigurationsdaten erneut laden, wenn sich diese ändern.</span><span class="sxs-lookup"><span data-stu-id="a386f-177">[IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot) can reload configuration data when it changes.</span></span>
* <span data-ttu-id="a386f-178">Bei Konfigurationsschlüsseln wird die Groß-/Kleinschreibung **nicht** beachtet.</span><span class="sxs-lookup"><span data-stu-id="a386f-178">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="a386f-179">Speichern Sie **niemals** Kennwörter oder andere sensible Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="a386f-179">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="a386f-180">Verwenden Sie keine Produktionsgeheimnisse in Entwicklungs- oder Testumgebungen.</span><span class="sxs-lookup"><span data-stu-id="a386f-180">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="a386f-181">Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in ein Quellcoderepository übernommen werden können.</span><span class="sxs-lookup"><span data-stu-id="a386f-181">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="a386f-182">Erfahren Sie mehr zum [Verwenden mehrerer Umgebungen](xref:fundamentals/environments) und zu einer [sicheren Speicherung von App-Geheimnissen bei der Entwicklung](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a386f-182">Learn more about [how to use multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets in development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="a386f-183">Für hierarchische Konfigurationswerte, die in Umgebungsvariablen angegeben sind, funktioniert ein Doppelpunkt (`:`) möglicherweise nicht auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="a386f-183">For hierarchical config values specified in environment variables, a colon (`:`) may not work on all platforms.</span></span> <span data-ttu-id="a386f-184">Ein doppelter Unterstrich (`__`) wird auf allen Plattformen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="a386f-184">Double underscore (`__`) is supported by all platforms.</span></span>
* <span data-ttu-id="a386f-185">Wird mit der Konfigurations-API interagiert, funktioniert ein Doppelpunkt (`:`) auf allen Plattformen.</span><span class="sxs-lookup"><span data-stu-id="a386f-185">When interacting with the configuration API, a colon (`:`) works on all platforms.</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="a386f-186">In-Memory-Anbieter und Bindung an eine POCO-Klasse</span><span class="sxs-lookup"><span data-stu-id="a386f-186">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="a386f-187">Im folgenden Beispiel wird gezeigt, wie der In-Memory-Anbieter verwendet und an eine Klasse gebunden wird:</span><span class="sxs-lookup"><span data-stu-id="a386f-187">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="a386f-188">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, durch eine Bindung wird jedoch die Erstellung von Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="a386f-188">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="a386f-189">Durch eine Bindung können POCO-Objekte oder sogar ganze Objektdiagramme abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-189">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="a386f-190">GetValue</span><span class="sxs-lookup"><span data-stu-id="a386f-190">GetValue</span></span>

<span data-ttu-id="a386f-191">Im folgenden Beispiel wird die Erweiterungsmethode [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-191">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="a386f-192">Durch die `GetValue<T>`-Methode von ConfigurationBinder kann ein Standardwert (hier 80) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-192">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="a386f-193">`GetValue<T>` ist für einfache Szenarien vorgesehen und kann nicht an gesamte Abschnitte gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-193">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="a386f-194">`GetValue<T>` ruft skalare Werte von `GetSection(key).Value` ab, die in einen bestimmten Typ konvertiert wurden.</span><span class="sxs-lookup"><span data-stu-id="a386f-194">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="a386f-195">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="a386f-195">Bind to an object graph</span></span>

<span data-ttu-id="a386f-196">Jedes Objekt in einer Klasse kann rekursiv gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-196">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="a386f-197">Betrachten wir einmal die folgende `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="a386f-197">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="a386f-198">Im folgenden Beispiel erfolgt eine Bindung an die `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="a386f-198">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="a386f-199">Bei **ASP.NET Core 1.1** und höher kann `Get<T>` für gesamte Abschnitte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-199">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="a386f-200">`Get<T>` ist eventuell praktischer als `Bind`.</span><span class="sxs-lookup"><span data-stu-id="a386f-200">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="a386f-201">Der folgende Code zeigt, wie `Get<T>` in Bezug auf das vorherige Beispiel verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="a386f-201">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="a386f-202">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a386f-202">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="a386f-203">Das Programm zeigt `Height 11` an.</span><span class="sxs-lookup"><span data-stu-id="a386f-203">The program displays `Height 11`.</span></span>

<span data-ttu-id="a386f-204">Mit dem folgenden Code kann ein Komponententest für die Konfiguration durchgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="a386f-204">The following code can be used to unit test the configuration:</span></span>

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var settings = new AppSettings();
    config.GetSection("App").Bind(settings);

    Assert.Equal("Rick", settings.Profile.Machine);
    Assert.Equal(11, settings.Window.Height);
    Assert.Equal(11, settings.Window.Width);
    Assert.Equal("connectionstring", settings.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="a386f-205">Erstellen eines benutzerdefinierten Entity Framework-Anbieters</span><span class="sxs-lookup"><span data-stu-id="a386f-205">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="a386f-206">In diesem Abschnitt wird ein Standardkonfigurationsanbieter erstellt, der mithilfe des EF Name/Wert-Paare aus einer Datenbank liest.</span><span class="sxs-lookup"><span data-stu-id="a386f-206">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="a386f-207">Definieren Sie eine `ConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="a386f-207">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="a386f-208">Fügen Sie eine `ConfigurationContext`-Instanz hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen:</span><span class="sxs-lookup"><span data-stu-id="a386f-208">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="a386f-209">Erstellen Sie eine Klasse, die [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) implementiert:</span><span class="sxs-lookup"><span data-stu-id="a386f-209">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="a386f-210">Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="a386f-210">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="a386f-211">Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist:</span><span class="sxs-lookup"><span data-stu-id="a386f-211">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="a386f-212">Bei Ausführung des Beispiels werden die hervorgehobenen Werte von der Datenbank („value_from_ef_1“ und „value_from_ef_2“) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a386f-212">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="a386f-213">Eine `EFConfigSource`-Erweiterungsmethode zum Hinzufügen der Konfigurationsquelle kann verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="a386f-213">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="a386f-214">Im folgenden Code wird die Verwendung des benutzerdefinierten Anbieters `EFConfigProvider` veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="a386f-214">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="a386f-215">Beachten Sie, dass der benutzerdefinierte Anbieter `EFConfigProvider` im Beispiel nach dem JSON-Anbieter hinzugefügt wird, sodass Datenbankeinstellungen die Einstellungen aus der Datei *appsettings.json* überschreiben.</span><span class="sxs-lookup"><span data-stu-id="a386f-215">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="a386f-216">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="a386f-216">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="a386f-217">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-217">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="a386f-218">CommandLine-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="a386f-218">CommandLine configuration provider</span></span>

<span data-ttu-id="a386f-219">Der [CommandLine-Konfigurationsanbieter](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) erhält Schlüssel/Wert-Paare der Befehlszeilenargumente für die Konfiguration zur Runtime.</span><span class="sxs-lookup"><span data-stu-id="a386f-219">The [CommandLine configuration provider](/dotnet/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="a386f-220">Beispiel für den CommandLine-Konfigurationsanbieter anzeigen oder herunterladen</span><span class="sxs-lookup"><span data-stu-id="a386f-220">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="a386f-221">Einrichten und Verwenden des CommandLine-Konfigurationsanbieters</span><span class="sxs-lookup"><span data-stu-id="a386f-221">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="a386f-222">Standardkonfiguration</span><span class="sxs-lookup"><span data-stu-id="a386f-222">Basic Configuration</span></span>](#tab/basicconfiguration/)

<span data-ttu-id="a386f-223">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine`-Erweiterungsmethode für eine [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)-Instanz ab:</span><span class="sxs-lookup"><span data-stu-id="a386f-223">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="a386f-224">Nach der Ausführung des Codes wird die folgende Ausgabe angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-224">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="a386f-225">Durch die Übergabe von Schlüssel/Wert-Paaren der Argumente in der Befehlszeile werden die Werte von `Profile:MachineName` und `App:MainWindow:Left` geändert:</span><span class="sxs-lookup"><span data-stu-id="a386f-225">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="a386f-226">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-226">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="a386f-227">Um die von anderen Konfigurationsanbietern bereitgestellte Konfiguration mit der Befehlszeilenkonfiguration zu überschreiben, rufen Sie die letzte Funktion `AddCommandLine` von `ConfigurationBuilder` auf:</span><span class="sxs-lookup"><span data-stu-id="a386f-227">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a386f-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a386f-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="a386f-229">Typische ASP.NET Core 2.x-Apps verwenden für die Erstellung des Hosts die statische Komfortmethode `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="a386f-229">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="a386f-230">`CreateDefaultBuilder` lädt die optionale Konfiguration von *appsettings.json*, *appsettings.{Umgebung}.json*, [Benutzergeheimnisse](xref:security/app-secrets) (in der `Development`-Umgebung), Umgebungsvariablen und Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="a386f-230">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="a386f-231">Der CommandLine-Konfigurationsanbieter wird zuletzt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a386f-231">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="a386f-232">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-232">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="a386f-233">Für *appsettings*-Dateien, bei denen:</span><span class="sxs-lookup"><span data-stu-id="a386f-233">For *appsettings* files where:</span></span>

* <span data-ttu-id="a386f-234">`reloadOnChange` aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="a386f-234">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="a386f-235">die gleiche Einstellung in den Befehlszeilenargumenten sowie eine *appsettings*-Datei enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="a386f-235">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="a386f-236">die *appsettings*-Datei, die das entsprechende Befehlszeilenargument enthält, nach dem Start der App geändert wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-236">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="a386f-237">Wenn alle oben genannten Bedingungen erfüllt sind, werden die Befehlszeilenargumente überschrieben.</span><span class="sxs-lookup"><span data-stu-id="a386f-237">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="a386f-238">Die ASP.NET Core 2.x-App kann [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) statt `CreateDefaultBuilder` verwenden.</span><span class="sxs-lookup"><span data-stu-id="a386f-238">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="a386f-239">Wenn Sie `WebHostBuilder` verwenden, legen Sie die Konfiguration manuell mit [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) fest.</span><span class="sxs-lookup"><span data-stu-id="a386f-239">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="a386f-240">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="a386f-240">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a386f-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a386f-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="a386f-242">Erstellen Sie einen [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)-Anbieter, und rufen Sie die `AddCommandLine`-Methode auf, um den CommandLine-Konfigurationsanbieter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="a386f-242">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="a386f-243">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="a386f-243">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="a386f-244">Wenden Sie mit der `UseConfiguration`-Methode die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) an:</span><span class="sxs-lookup"><span data-stu-id="a386f-244">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="a386f-245">Argumente</span><span class="sxs-lookup"><span data-stu-id="a386f-245">Arguments</span></span>

<span data-ttu-id="a386f-246">Über die Befehlszeile übergebene Argumente müssen eines der zwei Formate, die in der folgenden Tabelle aufgeführt sind, aufweisen:</span><span class="sxs-lookup"><span data-stu-id="a386f-246">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="a386f-247">Argumentformat</span><span class="sxs-lookup"><span data-stu-id="a386f-247">Argument format</span></span>                                                     | <span data-ttu-id="a386f-248">Beispiel</span><span class="sxs-lookup"><span data-stu-id="a386f-248">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="a386f-249">Einzelnes Argument: ein durch ein Gleichheitszeichen (`=`) getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="a386f-249">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="a386f-250">Sequenz der zwei Argumente: ein durch einen Leerraum getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="a386f-250">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="a386f-251">**Einzelnes Argument**</span><span class="sxs-lookup"><span data-stu-id="a386f-251">**Single argument**</span></span>

<span data-ttu-id="a386f-252">Der Wert muss einem Gleichheitszeichen (`=`) nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="a386f-252">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="a386f-253">Der Wert kann NULL sein (z.B. `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="a386f-253">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="a386f-254">Der Schlüssel darf einen Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-254">The key may have a prefix.</span></span>

| <span data-ttu-id="a386f-255">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="a386f-255">Key prefix</span></span>               | <span data-ttu-id="a386f-256">Beispiel</span><span class="sxs-lookup"><span data-stu-id="a386f-256">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a386f-257">Ohne Präfix</span><span class="sxs-lookup"><span data-stu-id="a386f-257">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="a386f-258">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="a386f-258">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="a386f-259">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="a386f-259">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="a386f-260">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="a386f-260">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="a386f-261">& #8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-261">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a386f-262">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="a386f-262">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="a386f-263">Hinweis: Wenn `-key2` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a386f-263">Note: If `-key2` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="a386f-264">**Sequenz aus zwei Argumenten**</span><span class="sxs-lookup"><span data-stu-id="a386f-264">**Sequence of two arguments**</span></span>

<span data-ttu-id="a386f-265">Der Wert darf nicht null und muss dem Schlüssel getrennt durch einen Leerraum nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="a386f-265">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="a386f-266">Der Schlüssel muss ein Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-266">The key must have a prefix.</span></span>

| <span data-ttu-id="a386f-267">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="a386f-267">Key prefix</span></span>               | <span data-ttu-id="a386f-268">Beispiel</span><span class="sxs-lookup"><span data-stu-id="a386f-268">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="a386f-269">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="a386f-269">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="a386f-270">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="a386f-270">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="a386f-271">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="a386f-271">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="a386f-272">& #8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-272">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="a386f-273">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="a386f-273">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="a386f-274">Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="a386f-274">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="a386f-275">Doppelte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="a386f-275">Duplicate keys</span></span>

<span data-ttu-id="a386f-276">Wenn Schlüssel doppelt angegeben werden, wird das letzte Schlüssel/Wert-Paar verwendet.</span><span class="sxs-lookup"><span data-stu-id="a386f-276">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="a386f-277">Switchmappings</span><span class="sxs-lookup"><span data-stu-id="a386f-277">Switch mappings</span></span>

<span data-ttu-id="a386f-278">Bei der manuellen Erstellung von Konfigurationen mit `ConfigurationBuilder` kann ein Switchmappingswörterbuch der `AddCommandLine`-Methode hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-278">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="a386f-279">Switchmappings erlauben das Angeben einer Logik zum Ersetzen von Schlüsselnamen.</span><span class="sxs-lookup"><span data-stu-id="a386f-279">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="a386f-280">Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft.</span><span class="sxs-lookup"><span data-stu-id="a386f-280">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="a386f-281">Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zur Festlegung der Konfiguration zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="a386f-281">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="a386f-282">Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="a386f-282">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="a386f-283">Regeln für Schlüssel von Switchmappingwörterbüchern:</span><span class="sxs-lookup"><span data-stu-id="a386f-283">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="a386f-284">Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.</span><span class="sxs-lookup"><span data-stu-id="a386f-284">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="a386f-285">Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="a386f-285">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="a386f-286">Im folgenden Beispiel können durch die `GetSwitchMappings`-Methode einzelne Gedankenstriche (`-`) als Schlüsselpräfix in Befehlszeilenargumenten verwendet und führende Unterschlüsselpräfixe vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="a386f-286">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="a386f-287">Ohne die Angabe von Befehlszeilenargumenten legt das auf `AddInMemoryCollection` festgelegte Wörterbuch die Konfigurationswerte fest.</span><span class="sxs-lookup"><span data-stu-id="a386f-287">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="a386f-288">Führen Sie die App mit dem folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a386f-288">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="a386f-289">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-289">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="a386f-290">Verwenden Sie den folgenden Code, um die Konfigurationseinstellungen zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="a386f-290">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="a386f-291">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a386f-291">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="a386f-292">Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten:</span><span class="sxs-lookup"><span data-stu-id="a386f-292">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="a386f-293">Taste</span><span class="sxs-lookup"><span data-stu-id="a386f-293">Key</span></span>            | <span data-ttu-id="a386f-294">Wert</span><span class="sxs-lookup"><span data-stu-id="a386f-294">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="a386f-295">Führen Sie zur Veranschaulichung des Schlüsselwechsels mit dem Wörterbuch den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="a386f-295">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="a386f-296">Die Befehlszeilenschlüssel sind ausgetauscht.</span><span class="sxs-lookup"><span data-stu-id="a386f-296">The command-line keys are swapped.</span></span> <span data-ttu-id="a386f-297">Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left` an:</span><span class="sxs-lookup"><span data-stu-id="a386f-297">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="a386f-298">Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="a386f-298">web.config file</span></span>

<span data-ttu-id="a386f-299">Eine Datei namens *Web.config* ist erforderlich, wenn Sie die App in IIS oder IIS Express hosten.</span><span class="sxs-lookup"><span data-stu-id="a386f-299">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="a386f-300">Die Einstellungen in der Datei *Web.config* aktivieren das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), um die App zu starten und andere IIS-Einstellungen und -Module zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="a386f-300">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="a386f-301">Wenn die Datei *Web.config* nicht vorhanden ist und die Projektdatei `<Project Sdk="Microsoft.NET.Sdk.Web">` enthält, wird bei der Veröffentlichung des Projekts eine Datei namens *Web.config* in der veröffentlichten Ausgabe (dem Ordner *publish*) erstellt.</span><span class="sxs-lookup"><span data-stu-id="a386f-301">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="a386f-302">Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="a386f-302">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="access-configuration-during-startup"></a><span data-ttu-id="a386f-303">Zugreifen auf die Konfiguration während des Starts</span><span class="sxs-lookup"><span data-stu-id="a386f-303">Access configuration during startup</span></span>

<span data-ttu-id="a386f-304">Beispiele für den Zugriff auf die Konfigurationen in `ConfigureServices` oder `Configure` während des Starts finden Sie im Artikel zum [Start der Anwendung](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a386f-304">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="adding-configuration-from-an-external-assembly"></a><span data-ttu-id="a386f-305">Hinzufügen von Konfigurationen aus einer externen Assembly</span><span class="sxs-lookup"><span data-stu-id="a386f-305">Adding configuration from an external assembly</span></span>

<span data-ttu-id="a386f-306">Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung können einer App beim Start von einer externen Assemblys aus, die sich außerhalb der `Startup`-Klasse der App befindet, Erweiterungen hinzugefügt werden</span><span class="sxs-lookup"><span data-stu-id="a386f-306">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="a386f-307">Weitere Informationen finden Sie im Artikel zum [Erweitern einer App von einer externen Assembly aus](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="a386f-307">For more information, see [Enhance an app from an external assembly](xref:fundamentals/configuration/platform-specific-configuration).</span></span>

## <a name="access-configuration-in-a-razor-page-or-mvc-view"></a><span data-ttu-id="a386f-308">Zugreifen auf die Konfiguration auf einer Razor-Seite oder in einer MVC-Ansicht</span><span class="sxs-lookup"><span data-stu-id="a386f-308">Access configuration in a Razor Page or MVC view</span></span>

<span data-ttu-id="a386f-309">Um auf die Konfigurationseinstellungen auf einer Razor Pages-Seite oder in einer MVC-Ansicht zuzugreifen, fügen Sie eine [using-Direktive](xref:mvc/views/razor#using) ([C#-Referenz: using-Direktive](/dotnet/csharp/language-reference/keywords/using-directive)) für den [Microsoft.Extensions.Configuration-Namespace ](/dotnet/api/microsoft.extensions.configuration) hinzu, und fügen Sie [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) auf der Seite oder in der Ansicht ein.</span><span class="sxs-lookup"><span data-stu-id="a386f-309">To access configuration settings in a Razor Pages page or an MVC view, add a [using directive](xref:mvc/views/razor#using) ([C# reference: using directive](/dotnet/csharp/language-reference/keywords/using-directive)) for the [Microsoft.Extensions.Configuration namespace](/dotnet/api/microsoft.extensions.configuration) and inject [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) into the page or view.</span></span>

<span data-ttu-id="a386f-310">Auf einer Razor Pages-Seite:</span><span class="sxs-lookup"><span data-stu-id="a386f-310">In a Razor Pages page:</span></span>

```cshtml
@page
@model IndexModel

@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index Page</title>
</head>
<body>
    <h1>Access configuration in a Razor Pages page</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

<span data-ttu-id="a386f-311">In einer MVC-Ansicht:</span><span class="sxs-lookup"><span data-stu-id="a386f-311">In an MVC view:</span></span>

```cshtml
@using Microsoft.Extensions.Configuration
@inject IConfiguration Configuration

<!DOCTYPE html>
<html lang="en">
<head>
    <title>Index View</title>
</head>
<body>
    <h1>Access configuration in an MVC view</h1>
    <p>Configuration[&quot;key&quot;]: @Configuration["key"]</p>
</body>
</html>
```

## <a name="additional-notes"></a><span data-ttu-id="a386f-312">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="a386f-312">Additional notes</span></span>

* <span data-ttu-id="a386f-313">Die Abhängigkeitsinjektion (Dependency Injection, DI) wird erst nach Aufrufen von `ConfigureServices` eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="a386f-313">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="a386f-314">Das Konfigurationssystem ist nicht DI-fähig.</span><span class="sxs-lookup"><span data-stu-id="a386f-314">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="a386f-315">`IConfiguration` weist zwei Spezialisierungen auf:</span><span class="sxs-lookup"><span data-stu-id="a386f-315">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="a386f-316">`IConfigurationRoot` Wird für den Stammknoten verwendet.</span><span class="sxs-lookup"><span data-stu-id="a386f-316">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="a386f-317">Kann einen erneuten Ladevorgang auslösen.</span><span class="sxs-lookup"><span data-stu-id="a386f-317">Can trigger a reload.</span></span>
  * <span data-ttu-id="a386f-318">`IConfigurationSection` Stellt einen Abschnitt der Konfigurationswerte dar.</span><span class="sxs-lookup"><span data-stu-id="a386f-318">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="a386f-319">Die Methoden `GetSection` und `GetChildren` geben `IConfigurationSection` zurück.</span><span class="sxs-lookup"><span data-stu-id="a386f-319">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="a386f-320">Verwenden Sie [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot), wenn Sie Konfigurationen neu laden oder auf jeden Anbieter zugreifen möchten.</span><span class="sxs-lookup"><span data-stu-id="a386f-320">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="a386f-321">Keine dieser Situationen tritt häufig auf.</span><span class="sxs-lookup"><span data-stu-id="a386f-321">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a386f-322">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a386f-322">Additional resources</span></span>

* [<span data-ttu-id="a386f-323">Optionen</span><span class="sxs-lookup"><span data-stu-id="a386f-323">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="a386f-324">Verwenden mehrerer Umgebungen</span><span class="sxs-lookup"><span data-stu-id="a386f-324">Use multiple environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="a386f-325">Sicheres Speichern geheimer App-Schlüssel in der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="a386f-325">Safe storage of app secrets in development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="a386f-326">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a386f-326">Host in ASP.NET Core</span></span>](xref:fundamentals/host/index)
* [<span data-ttu-id="a386f-327">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="a386f-327">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="a386f-328">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="a386f-328">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
