---
title: Konfiguration in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Konfigurations-API verwenden, um eine ASP.NET Core-app aus mehreren Quellen zu konfigurieren.
keywords: ASP.NET Core, Konfiguration, JSON, Konfiguration
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a><span data-ttu-id="e38a4-104">Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e38a4-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="e38a4-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Markierung Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e38a4-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e38a4-106">Der Konfigurations-API bietet eine Möglichkeit zum Konfigurieren von einer app basierend auf eine Liste von Name / Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="e38a4-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="e38a4-107">Konfiguration wird zur Laufzeit aus mehreren Quellen gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="e38a4-108">Die Name-Wert-Paare können in einer Hierarchie mit mehreren Ebenen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="e38a4-109">Es gibt Konfigurationsanbieter für ein:</span><span class="sxs-lookup"><span data-stu-id="e38a4-109">There are configuration providers for:</span></span>

* <span data-ttu-id="e38a4-110">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="e38a4-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="e38a4-111">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="e38a4-111">Command-line arguments</span></span>
* <span data-ttu-id="e38a4-112">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="e38a4-112">Environment variables</span></span>
* <span data-ttu-id="e38a4-113">Die .NET Objekte im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="e38a4-113">In-memory .NET objects</span></span>
* <span data-ttu-id="e38a4-114">Eine verschlüsselte Benutzerspeicher</span><span class="sxs-lookup"><span data-stu-id="e38a4-114">An encrypted user store</span></span>
* [<span data-ttu-id="e38a4-115">Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="e38a4-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="e38a4-116">Benutzerdefinierte Anbieter, die Sie installieren, oder erstellen</span><span class="sxs-lookup"><span data-stu-id="e38a4-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="e38a4-117">Jede Konfigurationswert ordnet einen Zeichenfolgenschlüssel.</span><span class="sxs-lookup"><span data-stu-id="e38a4-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="e38a4-118">Integrierte Bindung unterstützt beim Deserialisieren von Einstellungen in einer benutzerdefinierten [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) Objekt (eine einfache .NET Klasse mit Eigenschaften).</span><span class="sxs-lookup"><span data-stu-id="e38a4-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="e38a4-119">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e38a4-119">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="simple-configuration"></a><span data-ttu-id="e38a4-120">Einfache Konfiguration</span><span class="sxs-lookup"><span data-stu-id="e38a4-120">Simple configuration</span></span>

<span data-ttu-id="e38a4-121">Die folgenden Konsolen-app verwendet die JSON-Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="e38a4-121">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

<span data-ttu-id="e38a4-122">Die Anwendung liest und zeigt die folgenden Konfigurationseinstellungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-122">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="e38a4-123">Konfiguration besteht eine hierarchische Liste von Name / Wert-Paaren, die in denen die Knoten durch einen Doppelpunkt getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="e38a4-123">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="e38a4-124">Um einen Wert abzurufen, Zugriff auf die `Configuration` einen Indexer mit dem entsprechenden Schlüssel des Elements:</span><span class="sxs-lookup"><span data-stu-id="e38a4-124">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="e38a4-125">Verwenden Sie zum Arbeiten mit Arrays in JSON-formatierte Konfigurationsquellen einem Arrayindex als Teil der Doppelpunkt getrennte Zeichenfolge ein.</span><span class="sxs-lookup"><span data-stu-id="e38a4-125">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="e38a4-126">Im folgenden Beispiel wird der Name des ersten Elements in den vorherigen `wizards` Array:</span><span class="sxs-lookup"><span data-stu-id="e38a4-126">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="e38a4-127">Name-Wert-Paare, die in der integrierten geschrieben in `Configuration` Anbieter sind **nicht** beibehalten, Sie können jedoch erstellen ein benutzerdefiniertes Anbieters, der Werte speichert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-127">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="e38a4-128">Finden Sie unter [standardkonfigurationsanbieter](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="e38a4-128">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="e38a4-129">Das vorhergehende Beispiel verwendet den Indexer für die Konfiguration, um Werte zu lesen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-129">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="e38a4-130">Auf die Konfiguration außerhalb von `Startup`, verwenden Sie die [Optionen Muster](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="e38a4-130">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="e38a4-131">Die *Optionen Muster* wird weiter unten in diesem Artikel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-131">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="e38a4-132">Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen, z. B. Entwicklungs-, Test- und produktionsumgebung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-132">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="e38a4-133">Die `CreateDefaultBuilder` Erweiterungsmethode in einer ASP.NET Core 2.x-app (oder mit `AddJsonFile` und `AddEnvironmentVariables` direkt in einer ASP.NET Core 1.x-app) Konfigurationsanbieter für das Lesen von JSON-Dateien und das System Konfigurationsquellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-133">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="e38a4-134">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="e38a4-134">*appsettings.json*</span></span>
* <span data-ttu-id="e38a4-135">*"appSettings". \<EnvironmentName > JSON*</span><span class="sxs-lookup"><span data-stu-id="e38a4-135">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="e38a4-136">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="e38a4-136">environment variables</span></span>

<span data-ttu-id="e38a4-137">Finden Sie unter [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) eine Erläuterung der Parameter.</span><span class="sxs-lookup"><span data-stu-id="e38a4-137">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="e38a4-138">`reloadOnChange`wird nur in ASP.NET Core 1.1 und höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-138">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="e38a4-139">Konfigurationsquellen werden in der Reihenfolge gelesen, die sie angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="e38a4-139">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="e38a4-140">Im obigen Code werden die Umgebungsvariablen für die zuletzt gelesen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-140">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="e38a4-141">Alle Konfigurationswerte, die durch die Umgebung eingerichtet würde er in den beiden vorherigen Anbietern ersetzen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-141">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="e38a4-142">Die Umgebung wird auf einen der in der Regel festgelegt `Development`, `Staging`, oder `Production`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-142">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="e38a4-143">Finden Sie unter [arbeiten mit mehreren Umgebungen](environments.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-143">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="e38a4-144">Überlegungen zur Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e38a4-144">Configuration considerations:</span></span>

* <span data-ttu-id="e38a4-145">`IOptionsSnapshot`Konfigurationsdaten können erneut laden, wenn sich ändert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-145">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="e38a4-146">Verwendung `IOptionsSnapshot` Konfigurationsdaten werden erneut geladen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-146">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="e38a4-147">Finden Sie unter [IOptionsSnapshot](#ioptionssnapshot) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-147">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="e38a4-148">-Konfigurationsschlüssel werden Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="e38a4-148">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="e38a4-149">Eine bewährte Methode ist Umgebungsvariablen letzten, an, sodass die lokale Umgebung, alles in bereitgestellten Konfigurationsdateien festlegen überschreiben kann.</span><span class="sxs-lookup"><span data-stu-id="e38a4-149">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="e38a4-150">**Nie** Kennwörter oder andere vertraulichen Daten im Anbietercode Konfiguration oder in Konfigurationsdateien als nur-Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-150">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="e38a4-151">Keine Produktion geheime Schlüssel in der Entwicklung verwenden, oder Umgebungen zu testen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-151">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="e38a4-152">Geben Sie stattdessen geheime Schlüssel außerhalb der Projektstruktur auf, damit sie in das Repository nicht versehentlich eingetragen werden können.</span><span class="sxs-lookup"><span data-stu-id="e38a4-152">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="e38a4-153">Erfahren Sie mehr über [arbeiten mit mehreren Umgebungen](environments.md) und Verwalten von [sichere Speicherung von app-Kennwörter während der Entwicklung](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="e38a4-153">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="e38a4-154">Wenn `:` nicht ersetzen in Umgebungsvariablen verwendet, in Ihrem System, `:` mit `__` (doppelter Unterstrich).</span><span class="sxs-lookup"><span data-stu-id="e38a4-154">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="e38a4-155">Mithilfe der Optionen und Konfigurationsobjekten</span><span class="sxs-lookup"><span data-stu-id="e38a4-155">Using Options and configuration objects</span></span>

<span data-ttu-id="e38a4-156">Das Muster Optionen verwendet benutzerdefinierte Optionsklassen, um eine Gruppe von verwandten Einstellungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-156">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="e38a4-157">Es wird empfohlen, entkoppelte Klassen für jede Funktion in Ihrer app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-157">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="e38a4-158">Entkoppelte Klassen entsprechen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-158">Decoupled classes follow:</span></span>

* <span data-ttu-id="e38a4-159">Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/) : Klassen richten sich nur auf den Konfigurationseinstellungen, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-159">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="e38a4-160">[Trennung von Anliegen](http://deviq.com/separation-of-concerns/) : Einstellungen für die verschiedenen Teile Ihrer App sind nicht abhängige oder gekoppelten miteinander.</span><span class="sxs-lookup"><span data-stu-id="e38a4-160">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="e38a4-161">Die Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="e38a4-161">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="e38a4-162">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e38a4-162">For example:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

<span data-ttu-id="e38a4-163">Im folgenden Code wird die JSON-Konfigurationsanbieter aktiviert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-163">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="e38a4-164">Die `MyOptions` Klasse dem Dienstcontainer hinzugefügt und an Configuration gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="e38a4-164">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

<span data-ttu-id="e38a4-165">Die folgenden [Controller](../mvc/controllers/index.md) verwendet [Konstruktor Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#what-is-dependency-injection) auf [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) auf Einstellungen zugreifen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-165">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="e38a4-166">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38a4-166">With the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

<span data-ttu-id="e38a4-167">Die `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-167">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="e38a4-168">Standard-apps wird nicht die gesamte Konfiguration in eine Optionsdatei für die einzelnen gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-168">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="e38a4-169">Später zeige ich wie `GetSection` zu einem Abschnitt binden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-169">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="e38a4-170">Im folgenden Code wird eine zweite `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-170">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e38a4-171">Er verwendet ein Delegat zum Konfigurieren der Bindung mit `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-171">It uses a delegate to configure the binding with `MyOptions`.</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

<span data-ttu-id="e38a4-172">Sie können mehrere Konfigurationsanbieter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-172">You can add multiple configuration providers.</span></span> <span data-ttu-id="e38a4-173">Konfigurationsanbieter stehen im NuGet-Pakete zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="e38a4-173">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="e38a4-174">Sie werden in Reihenfolge angewendet, die sie registriert sind.</span><span class="sxs-lookup"><span data-stu-id="e38a4-174">They are applied in order they are registered.</span></span>

<span data-ttu-id="e38a4-175">Jeder Aufruf von `Configure<TOptions>` Fügt eine `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="e38a4-175">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="e38a4-176">Im vorherigen Beispiel, die Werte der `Option1` und `Option2` werden in angegebenen *appsettings.json* –, aber der Wert des `Option1` durch den konfigurierten Delegaten überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-176">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="e38a4-177">Wenn mehr als ein Dienst aktiviert ist, die letzte Konfigurationsquelle angegeben "gewinnt" (setzt den Konfigurationswert).</span><span class="sxs-lookup"><span data-stu-id="e38a4-177">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="e38a4-178">Im vorangehenden Code der `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-178">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="e38a4-179">Wenn Sie Optionen zur Konfiguration binden, jede Eigenschaft im Optionstyp Ihres einem Konfigurationsschlüssel des Formulars gebunden ist `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-179">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="e38a4-180">Z. B. die `MyOptions.Option1` Eigenschaft gebunden ist, auf den Schlüssel `Option1`, das Auslesen der `option1` Eigenschaft im *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e38a4-180">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="e38a4-181">Ein Beispiel für die untergeordnete Eigenschaft ist später in diesem Artikel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-181">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="e38a4-182">Im folgenden Code ein drittes `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-182">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="e38a4-183">Sie bindet `MySubOptions` Abschnitt `subsection` von der *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38a4-183">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

<span data-ttu-id="e38a4-184">Hinweis: Diese Erweiterungsmethode erfordert die `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="e38a4-184">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="e38a4-185">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38a4-185">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

<span data-ttu-id="e38a4-186">Die `MySubOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e38a4-186">The `MySubOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="e38a4-187">Mit den folgenden `Controller`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-187">With the following `Controller`:</span></span>

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

<span data-ttu-id="e38a4-188">`subOption1 = subvalue1_from_json, subOption2 = 200`wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="e38a4-188">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="e38a4-189">Sie können auch angeben von Optionen in einem Ansichtsmodell oder einfügen `IOptions<TOptions>` direkt in eine Sicht:</span><span class="sxs-lookup"><span data-stu-id="e38a4-189">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="e38a4-190">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="e38a4-190">IOptionsSnapshot</span></span>

<span data-ttu-id="e38a4-191">*Erfordert ASP.NET Core 1.1 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="e38a4-191">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="e38a4-192">`IOptionsSnapshot`unterstützt die Konfigurationsdaten werden erneut geladen, wenn sich die Konfigurationsdatei geändert hat.</span><span class="sxs-lookup"><span data-stu-id="e38a4-192">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="e38a4-193">Er verfügt über einen minimalen Aufwand.</span><span class="sxs-lookup"><span data-stu-id="e38a4-193">It has minimal overhead.</span></span> <span data-ttu-id="e38a4-194">Mit `IOptionsSnapshot` mit `reloadOnChange: true`, sind die Optionen zum gebunden `IConfiguration` und erneut geladen, wenn sich geändert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-194">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="e38a4-195">Im folgende Beispiel wird veranschaulicht, wie ein neuer `IOptionsSnapshot` wird erstellt, nachdem *config.json* ändert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-195">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="e38a4-196">Anforderungen an den Server werden zurückgegeben, die gleiche Uhrzeit *config.json* hat **nicht** geändert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-196">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="e38a4-197">Die erste Anforderung nach *config.json* Änderungen werden die Zeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-197">The first request after *config.json* changes will show a new time.</span></span>

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

<span data-ttu-id="e38a4-198">Die folgende Abbildung zeigt die Serverausgabe:</span><span class="sxs-lookup"><span data-stu-id="e38a4-198">The following image shows the server output:</span></span>

![Browser Bild anzeigen "Letzte Aktualisierung: 11/22/2016 16:43 Uhr"](configuration/_static/first.png)

<span data-ttu-id="e38a4-200">Aktualisieren des Browsers ändert sich der Nachrichtenwert oder angezeigte Uhrzeit (Wenn *config.json* wurde nicht geändert).</span><span class="sxs-lookup"><span data-stu-id="e38a4-200">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="e38a4-201">Ändern und speichern Sie die *config.json* und den Browser zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="e38a4-201">Change and save the  *config.json* and then refresh the browser:</span></span>

![Browser Bild anzeigen "," e: "Letzte Aktualisierung 11/22/2016 16:53:00 Uhr"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="e38a4-203">In-Memory-Anbieter und die Bindung an einer POCO-Klasse</span><span class="sxs-lookup"><span data-stu-id="e38a4-203">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="e38a4-204">Das folgende Beispiel zeigt, wie den in-Memory-Anbieter verwenden und auf eine Klasse gebunden:</span><span class="sxs-lookup"><span data-stu-id="e38a4-204">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

<span data-ttu-id="e38a4-205">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, aber Binden ermöglicht die Erstellung von Objekten.</span><span class="sxs-lookup"><span data-stu-id="e38a4-205">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="e38a4-206">Bindung ermöglicht es Ihnen POCO-Objekte oder Objektdiagramme ganzer abgerufen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-206">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="e38a4-207">Im folgende Beispiel wird gezeigt, wie zum Binden an `MyWindow` und das Muster Optionen mit einer ASP.NET-MVC-Anwendung Core verwenden:</span><span class="sxs-lookup"><span data-stu-id="e38a4-207">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

<span data-ttu-id="e38a4-208">Binden Sie die benutzerdefinierte Klasse in `ConfigureServices` beim Host zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-208">Bind the custom class in `ConfigureServices` when building the host:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

<span data-ttu-id="e38a4-209">Zeigen Sie die Einstellungen aus der `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-209">Display the settings from the `HomeController`:</span></span>

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a><span data-ttu-id="e38a4-210">GetValue</span><span class="sxs-lookup"><span data-stu-id="e38a4-210">GetValue</span></span>

<span data-ttu-id="e38a4-211">Das folgende Beispiel zeigt die [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="e38a4-211">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

<span data-ttu-id="e38a4-212">Die ConfigurationBinder `GetValue<T>` Methode können Sie einen Standardwert (80 in der Stichprobe) angeben.</span><span class="sxs-lookup"><span data-stu-id="e38a4-212">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="e38a4-213">`GetValue<T>`ist für einfache Szenarien, und Bindung erfolgt nicht auf die gesamte Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="e38a4-213">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="e38a4-214">`GetValue<T>`Ruft die Skalarwerte aus `GetSection(key).Value` für einen bestimmten Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-214">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="e38a4-215">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="e38a4-215">Binding to an object graph</span></span>

<span data-ttu-id="e38a4-216">Sie können rekursiv Bindung auf jedes Objekt in einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="e38a4-216">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="e38a4-217">Beachten Sie Folgendes `AppOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e38a4-217">Consider the following `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

<span data-ttu-id="e38a4-218">Im folgende Beispiel bindet an die `AppOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="e38a4-218">The following sample binds to the `AppOptions` class:</span></span>

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="e38a4-219">**ASP.NET Core 1.1** und höher können `Get<T>`, die gesamte Abschnitte funktioniert.</span><span class="sxs-lookup"><span data-stu-id="e38a4-219">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="e38a4-220">`Get<T>`kann weitere Convienent als die Verwendung von `Bind`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-220">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="e38a4-221">Der folgende Code zeigt, wie Sie `Get<T>` mit dem obigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="e38a4-221">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="e38a4-222">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38a4-222">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="e38a4-223">Das Programm zeigt `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-223">The program displays `Height 11`.</span></span>

<span data-ttu-id="e38a4-224">Der folgende Code verwendet werden kann, Einheit Testen der Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="e38a4-224">The following code can be used to unit test the configuration:</span></span>

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="e38a4-225">Basic-Beispiel des benutzerdefinierten Anbieter für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e38a4-225">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="e38a4-226">In diesem Abschnitt wird ein grundlegende Konfigurationsanbieter, das Name-Wert-Paare aus einer Datenbank mithilfe von EF erstellt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-226">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="e38a4-227">Definieren einer `ConfigurationValue` Entität für die Konfigurationswerte in der Datenbank gespeichert:</span><span class="sxs-lookup"><span data-stu-id="e38a4-227">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="e38a4-228">Hinzufügen einer `ConfigurationContext` zum Speichern und Zugreifen auf die konfigurierten Werte:</span><span class="sxs-lookup"><span data-stu-id="e38a4-228">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="e38a4-229">Erstellen Sie eine Klasse, die implementiert [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="e38a4-229">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="e38a4-230">Erstellen Sie den standardkonfigurationsanbieter durch Vererben von [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="e38a4-230">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="e38a4-231">Der Konfigurationsanbieter initialisiert die Datenbank aus, wenn sie leer ist:</span><span class="sxs-lookup"><span data-stu-id="e38a4-231">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="e38a4-232">Die hervorgehobenen Werte aus der Datenbank ("value_from_ef_1" und "value_from_ef_2") werden angezeigt, wenn das Beispiel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-232">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="e38a4-233">Sie können Hinzufügen einer `EFConfigSource` Erweiterungsmethode für die Konfigurationsquelle hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-233">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="e38a4-234">Der folgende Code zeigt, wie Sie die Verwendung der benutzerdefinierten `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-234">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="e38a4-235">Beachten Sie das Beispiel fügt die benutzerdefinierte `EFConfigProvider` nach der JSON-Anbieter, also alle Einstellungen aus der Datenbank überschrieben aus der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="e38a4-235">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="e38a4-236">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="e38a4-236">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="e38a4-237">Folgendes wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-237">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="e38a4-238">CommandLine Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="e38a4-238">CommandLine configuration provider</span></span>

<span data-ttu-id="e38a4-239">Die [CommandLine Konfigurationsanbieter](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) Befehlszeilenargument Schlüssel-Wert-Paare für die Konfiguration zur Laufzeit erhält.</span><span class="sxs-lookup"><span data-stu-id="e38a4-239">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="e38a4-240">Zeigen Sie an oder Herunterladen Sie der CommandLine Konfigurationsbeispiel</span><span class="sxs-lookup"><span data-stu-id="e38a4-240">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a><span data-ttu-id="e38a4-241">Der Anbieter einrichten</span><span class="sxs-lookup"><span data-stu-id="e38a4-241">Setting up the provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="e38a4-242">Basiskonfiguration</span><span class="sxs-lookup"><span data-stu-id="e38a4-242">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="e38a4-243">Um Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine` Erweiterungsmethode in einer Instanz von [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="e38a4-243">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="e38a4-244">Der Code ausführen, wird die folgende Ausgabe angezeigt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-244">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="e38a4-245">Argumentübergabe Schlüssel-Wert-Paare in der Befehlszeile ändert die Werte der `Profile:MachineName` und `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-245">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="e38a4-246">Das Konsolenfenster zeigt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-246">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="e38a4-247">Zum Lieferumfang von anderen Anbietern Konfiguration Befehlszeilenkonfiguration außer Kraft setzen, rufen `AddCommandLine` auf letzte `ConfigurationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-247">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e38a4-248">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e38a4-248">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e38a4-249">Typische ASP.NET Core 2.x-apps verwenden die statische Hilfsmethode `CreateDefaultBuilder` auf den Host zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-249">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

<span data-ttu-id="e38a4-250">`CreateDefaultBuilder`optionale Konfiguration von lädt *appsettings.json*, *"appSettings". {} Umgebung} JSON*, [vertrauliche Benutzerdaten](xref:security/app-secrets) (in der `Development` Umgebung), Umgebungsvariablen und Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="e38a4-250">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="e38a4-251">Der Konfigurationsanbieter CommandLine zuletzt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-251">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="e38a4-252">Den Anbieter zuletzt aufrufen kann die Befehlszeilenargumente übergeben, die zur Laufzeit Konfigurationssatz durch die anderen Konfigurationsanbieter überschreiben zuvor aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-252">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="e38a4-253">Beachten Sie, dass für *"appSettings"* -Dateien mit `reloadOnChange` aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e38a4-253">Note that for *appsettings* files that `reloadOnChange` is enabled.</span></span> <span data-ttu-id="e38a4-254">Befehlszeilenargumente werden überschrieben, wenn ein übereinstimmendes Konfigurationswert in ein *"appSettings"* Datei geändert wird, nachdem die app gestartet wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-254">Command-line arguments are overridden if a matching configuration value in an *appsettings* file is changed after the app starts.</span></span>

> [!NOTE]
> <span data-ttu-id="e38a4-255">Als Alternative zur Verwendung der `CreateDefaultBuilder` -Methode, erstellen einen Host mit [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) und erstellen manuell mit der [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) wird in ASP.NET Core unterstützt 2.x.</span><span class="sxs-lookup"><span data-stu-id="e38a4-255">As an alternative to using the `CreateDefaultBuilder` method, creating a host using [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) and manually building configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) is supported in ASP.NET Core 2.x.</span></span> <span data-ttu-id="e38a4-256">Finden Sie unter der Registerkarte "1.x ASP.NET Core" für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-256">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e38a4-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e38a4-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e38a4-258">Erstellen einer [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) , und rufen Sie die `AddCommandLine` Methode, um den Konfigurationsanbieter CommandLine verwenden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-258">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="e38a4-259">Den Anbieter zuletzt aufrufen kann die Befehlszeilenargumente übergeben, die zur Laufzeit Konfigurationssatz durch die anderen Konfigurationsanbieter überschreiben zuvor aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-259">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="e38a4-260">Wendet die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) mit der `UseConfiguration` Methode:</span><span class="sxs-lookup"><span data-stu-id="e38a4-260">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="e38a4-261">Argumente</span><span class="sxs-lookup"><span data-stu-id="e38a4-261">Arguments</span></span>

<span data-ttu-id="e38a4-262">In der Befehlszeile übergebenen Argumente müssen in einem der zwei Formate, die in der folgenden Tabelle aufgeführten entsprechen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-262">Arguments passed on the command line must conform to one of two formats shown in the following table.</span></span>

| <span data-ttu-id="e38a4-263">Arguments-format</span><span class="sxs-lookup"><span data-stu-id="e38a4-263">Argument format</span></span>                                                     | <span data-ttu-id="e38a4-264">Beispiel</span><span class="sxs-lookup"><span data-stu-id="e38a4-264">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="e38a4-265">Einzelne Argument: ein Schlüssel-Wert-Paar, das durch ein Gleichheitszeichen getrennt (`=`)</span><span class="sxs-lookup"><span data-stu-id="e38a4-265">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="e38a4-266">Sequenz der zwei Argumente: ein Schlüssel-Wert-Paar, getrennt durch ein Leerzeichen</span><span class="sxs-lookup"><span data-stu-id="e38a4-266">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="e38a4-267">**Einzelnes argument**</span><span class="sxs-lookup"><span data-stu-id="e38a4-267">**Single argument**</span></span>

<span data-ttu-id="e38a4-268">Der Wert muss ein Gleichheitszeichen folgen (`=`).</span><span class="sxs-lookup"><span data-stu-id="e38a4-268">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="e38a4-269">Der Wert kann null sein (z. B. `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="e38a4-269">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="e38a4-270">Der Schlüssel möglicherweise ein Präfix aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-270">The key may have a prefix.</span></span>

| <span data-ttu-id="e38a4-271">Präfix</span><span class="sxs-lookup"><span data-stu-id="e38a4-271">Key prefix</span></span>               | <span data-ttu-id="e38a4-272">Beispiel</span><span class="sxs-lookup"><span data-stu-id="e38a4-272">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e38a4-273">Kein Präfix</span><span class="sxs-lookup"><span data-stu-id="e38a4-273">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="e38a4-274">Einzelne Dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e38a4-274">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="e38a4-275">Zwei Bindestriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="e38a4-275">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="e38a4-276">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="e38a4-276">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="e38a4-277">&#8224; Ein Schlüssel mit einem Bindestrich-Präfix (`-`) muss angegeben werden, [wechseln Zuordnungen](#switch-mappings), unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e38a4-277">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e38a4-278">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="e38a4-278">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="e38a4-279">Hinweis: Wenn `-key1` ist nicht vorhanden, in der [wechseln Zuordnungen](#switch-mappings) übergeben, um den Konfigurationsanbieter eine `FormatException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-279">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="e38a4-280">**Sequenz aus zwei Argumenten**</span><span class="sxs-lookup"><span data-stu-id="e38a4-280">**Sequence of two arguments**</span></span>

<span data-ttu-id="e38a4-281">Der Wert muss darf nicht null sein und den Schlüssel durch ein Leerzeichen getrennt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-281">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="e38a4-282">Der Schlüssel muss ein Präfix aufweisen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-282">The key must have a prefix.</span></span>

| <span data-ttu-id="e38a4-283">Präfix</span><span class="sxs-lookup"><span data-stu-id="e38a4-283">Key prefix</span></span>               | <span data-ttu-id="e38a4-284">Beispiel</span><span class="sxs-lookup"><span data-stu-id="e38a4-284">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="e38a4-285">Einzelne Dash (`-`) &#8224;</span><span class="sxs-lookup"><span data-stu-id="e38a4-285">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="e38a4-286">Zwei Bindestriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="e38a4-286">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="e38a4-287">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="e38a4-287">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="e38a4-288">&#8224; Ein Schlüssel mit einem Bindestrich-Präfix (`-`) muss angegeben werden, [wechseln Zuordnungen](#switch-mappings), unten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="e38a4-288">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="e38a4-289">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="e38a4-289">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="e38a4-290">Hinweis: Wenn `-key1` ist nicht vorhanden, in der [wechseln Zuordnungen](#switch-mappings) übergeben, um den Konfigurationsanbieter eine `FormatException` ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-290">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="e38a4-291">Doppelte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="e38a4-291">Duplicate keys</span></span>

<span data-ttu-id="e38a4-292">Wenn doppelte Schlüssel bereitgestellt werden, wird das letzte Schlüssel-Wert-Paar verwendet.</span><span class="sxs-lookup"><span data-stu-id="e38a4-292">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="e38a4-293">Switch-Zuordnungen</span><span class="sxs-lookup"><span data-stu-id="e38a4-293">Switch mappings</span></span>

<span data-ttu-id="e38a4-294">Wenn mit der manuellen Erstellung `ConfigurationBuilder`, Sie können optional einen Switch Zuordnungen Wörterbuch, das Bereitstellen der `AddCommandLine` Methode.</span><span class="sxs-lookup"><span data-stu-id="e38a4-294">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="e38a4-295">Switch-Zuordnungen können Sie Logik für den Austausch Schlüsselnamen zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="e38a4-295">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="e38a4-296">Wenn das Wörterbuch der Switch-Zuordnungen verwendet wird, wird das Wörterbuch für einen Schlüssel überprüft, die den vom ein Befehlszeilenargument angegebenen Schlüssel entspricht.</span><span class="sxs-lookup"><span data-stu-id="e38a4-296">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="e38a4-297">Wenn das Befehlszeile-Schlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (schlüsselersetzung) zurück, um die Konfiguration übergeben.</span><span class="sxs-lookup"><span data-stu-id="e38a4-297">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="e38a4-298">Eine Switch-Zuordnung für eine beliebige Taste Befehlszeilen, die einen einzelnen Bindestrich vorangestellt erforderlich ist (`-`).</span><span class="sxs-lookup"><span data-stu-id="e38a4-298">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="e38a4-299">Wechseln Sie wichtige Wörterbuchregeln Zuordnungen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-299">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="e38a4-300">Switches müssen mit einem Bindestrich beginnen (`-`) oder Double-Dash (`--`).</span><span class="sxs-lookup"><span data-stu-id="e38a4-300">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="e38a4-301">Das Wörterbuch der Switch-Zuordnungen dürfen keine doppelte Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="e38a4-301">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="e38a4-302">Im folgenden Beispiel die `GetSwitchMappings` Methode ermöglicht die Befehlszeilenargumente, die einen Bindestrich verwenden (`-`) Schlüssel Präfix, und vermeiden Sie führende Unterschlüsseln Präfixe.</span><span class="sxs-lookup"><span data-stu-id="e38a4-302">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="e38a4-303">Ohne Befehlszeilenargumente Wörterbuch bereitgestellt, um `AddInMemoryCollection` legt die Konfigurationswerte.</span><span class="sxs-lookup"><span data-stu-id="e38a4-303">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="e38a4-304">Führen Sie die app mit dem folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="e38a4-304">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="e38a4-305">Das Konsolenfenster zeigt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-305">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="e38a4-306">Verwenden Sie die folgenden Konfigurationseinstellungen übergeben:</span><span class="sxs-lookup"><span data-stu-id="e38a4-306">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="e38a4-307">Das Konsolenfenster zeigt:</span><span class="sxs-lookup"><span data-stu-id="e38a4-307">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="e38a4-308">Nachdem das Wörterbuch der Switch-Zuordnungen erstellt wurde, enthält es die Daten, die in der folgenden Tabelle gezeigt.</span><span class="sxs-lookup"><span data-stu-id="e38a4-308">After the switch mappings dictionary is created, it contains the data shown in the following table.</span></span>

| <span data-ttu-id="e38a4-309">Key</span><span class="sxs-lookup"><span data-stu-id="e38a4-309">Key</span></span>            | <span data-ttu-id="e38a4-310">Wert</span><span class="sxs-lookup"><span data-stu-id="e38a4-310">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="e38a4-311">Führen Sie zur Veranschaulichung Key wechseln, Wörterbuch mit den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="e38a4-311">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="e38a4-312">Die Befehlszeile Schlüssel vertauscht werden.</span><span class="sxs-lookup"><span data-stu-id="e38a4-312">The command-line keys are swapped.</span></span> <span data-ttu-id="e38a4-313">Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left`:</span><span class="sxs-lookup"><span data-stu-id="e38a4-313">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="e38a4-314">Die Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="e38a4-314">The web.config file</span></span>

<span data-ttu-id="e38a4-315">Ein *"Web.config"* Datei ist erforderlich, wenn Sie die app in IIS oder IIS Express hosten.</span><span class="sxs-lookup"><span data-stu-id="e38a4-315">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="e38a4-316">*"Web.config"* Schaltet die AspNetCoreModule in IIS, um Ihre app zu starten.</span><span class="sxs-lookup"><span data-stu-id="e38a4-316">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="e38a4-317">Einstellungen im *"Web.config"* die AspNetCoreModule in IIS für Ihre app starten und konfigurieren, Module und andere IIS-Einstellungen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e38a4-317">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="e38a4-318">Wenn Sie Visual Studio verwenden, und löschen *"Web.config"*, Visual Studio erstellt ein neues Konto.</span><span class="sxs-lookup"><span data-stu-id="e38a4-318">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="e38a4-319">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="e38a4-319">Additional notes</span></span>

* <span data-ttu-id="e38a4-320">(Dependency Injection, DI) wird nicht bis zum nach gesetzt `ConfigureServices` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="e38a4-320">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="e38a4-321">Das Konfigurationssystem ist nicht DI beachten.</span><span class="sxs-lookup"><span data-stu-id="e38a4-321">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="e38a4-322">`IConfiguration`verfügt über zwei spezialisierungen:</span><span class="sxs-lookup"><span data-stu-id="e38a4-322">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="e38a4-323">`IConfigurationRoot`Für den Stammknoten verwendet.</span><span class="sxs-lookup"><span data-stu-id="e38a4-323">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="e38a4-324">Können erneut auslösen.</span><span class="sxs-lookup"><span data-stu-id="e38a4-324">Can trigger a reload.</span></span>
  * <span data-ttu-id="e38a4-325">`IConfigurationSection`Stellt einen Abschnitt der Konfigurationswerte.</span><span class="sxs-lookup"><span data-stu-id="e38a4-325">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="e38a4-326">Die `GetSection` und `GetChildren` -Methoden zurückgeben einer `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="e38a4-326">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e38a4-327">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="e38a4-327">Additional resources</span></span>

* [<span data-ttu-id="e38a4-328">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="e38a4-328">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="e38a4-329">Sicheres Speichern geheimer App-Schlüssel während der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="e38a4-329">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="e38a4-330">Hosten in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e38a4-330">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="e38a4-331">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="e38a4-331">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="e38a4-332">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="e38a4-332">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
