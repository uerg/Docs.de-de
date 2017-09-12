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
ms.openlocfilehash: a14bc7fbcdac9acddfdab4fcd6e40385ca48bcc4
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a><span data-ttu-id="42785-104">Konfiguration in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="42785-104">Configuration in ASP.NET Core</span></span>

<span data-ttu-id="42785-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Markierung Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), und [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="42785-105">[Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), and [Daniel Roth](https://github.com/danroth27)</span></span>

<span data-ttu-id="42785-106">Der Konfigurations-API bietet eine Möglichkeit zum Konfigurieren von einer app basierend auf eine Liste von Name / Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="42785-106">The Configuration API provides a way of configuring an app based on a list of name-value pairs.</span></span> <span data-ttu-id="42785-107">Konfiguration wird zur Laufzeit aus mehreren Quellen gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="42785-107">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="42785-108">Die Name-Wert-Paare können in einer Hierarchie mit mehreren Ebenen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="42785-108">The name-value pairs can be grouped into a multi-level hierarchy.</span></span> <span data-ttu-id="42785-109">Es gibt Konfigurationsanbieter für ein:</span><span class="sxs-lookup"><span data-stu-id="42785-109">There are configuration providers for:</span></span>

* <span data-ttu-id="42785-110">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="42785-110">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="42785-111">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="42785-111">Command-line arguments</span></span>
* <span data-ttu-id="42785-112">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="42785-112">Environment variables</span></span>
* <span data-ttu-id="42785-113">Die .NET Objekte im Arbeitsspeicher</span><span class="sxs-lookup"><span data-stu-id="42785-113">In-memory .NET objects</span></span>
* <span data-ttu-id="42785-114">Eine verschlüsselte Benutzerspeicher</span><span class="sxs-lookup"><span data-stu-id="42785-114">An encrypted user store</span></span>
* [<span data-ttu-id="42785-115">Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="42785-115">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="42785-116">Benutzerdefinierte Anbieter, die Sie installieren, oder erstellen</span><span class="sxs-lookup"><span data-stu-id="42785-116">Custom providers, which you install or create</span></span>

<span data-ttu-id="42785-117">Jede Konfigurationswert ordnet einen Zeichenfolgenschlüssel.</span><span class="sxs-lookup"><span data-stu-id="42785-117">Each configuration value maps to a string key.</span></span> <span data-ttu-id="42785-118">Integrierte Bindung unterstützt beim Deserialisieren von Einstellungen in einer benutzerdefinierten [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) Objekt (eine einfache .NET Klasse mit Eigenschaften).</span><span class="sxs-lookup"><span data-stu-id="42785-118">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

[<span data-ttu-id="42785-119">Anzeigen oder Herunterladen von Beispielcode</span><span class="sxs-lookup"><span data-stu-id="42785-119">View or download sample code</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a><span data-ttu-id="42785-120">Einfache Konfiguration</span><span class="sxs-lookup"><span data-stu-id="42785-120">Simple configuration</span></span>

<span data-ttu-id="42785-121">Die folgenden Konsolen-app verwendet die JSON-Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="42785-121">The following console app uses the JSON configuration provider:</span></span>

<span data-ttu-id="42785-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-122">[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]</span></span>

<span data-ttu-id="42785-123">Die Anwendung liest und zeigt die folgenden Konfigurationseinstellungen vornehmen:</span><span class="sxs-lookup"><span data-stu-id="42785-123">The app reads and displays the following configuration settings:</span></span>

<span data-ttu-id="42785-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-124">[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]</span></span>

<span data-ttu-id="42785-125">Konfiguration besteht eine hierarchische Liste von Name / Wert-Paaren, die in denen die Knoten durch einen Doppelpunkt getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="42785-125">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="42785-126">Um einen Wert abzurufen, Zugriff auf die `Configuration` einen Indexer mit dem entsprechenden Schlüssel des Elements:</span><span class="sxs-lookup"><span data-stu-id="42785-126">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

<span data-ttu-id="42785-127">Verwenden Sie zum Arbeiten mit Arrays in JSON-formatierte Konfigurationsquellen einem Arrayindex als Teil der Doppelpunkt getrennte Zeichenfolge ein.</span><span class="sxs-lookup"><span data-stu-id="42785-127">To work with arrays in JSON-formatted configuration sources, use a array index as part of the colon-separated string.</span></span> <span data-ttu-id="42785-128">Im folgenden Beispiel wird der Name des ersten Elements in den vorherigen `wizards` Array:</span><span class="sxs-lookup"><span data-stu-id="42785-128">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

<span data-ttu-id="42785-129">Name-Wert-Paare, die in der integrierten geschrieben in `Configuration` Anbieter sind **nicht** beibehalten, Sie können jedoch erstellen ein benutzerdefiniertes Anbieters, der Werte speichert.</span><span class="sxs-lookup"><span data-stu-id="42785-129">Name-value pairs written to the built in `Configuration` providers are **not** persisted, however, you can create a custom provider that saves values.</span></span> <span data-ttu-id="42785-130">Finden Sie unter [standardkonfigurationsanbieter](xref:fundamentals/configuration#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="42785-130">See [custom configuration provider](xref:fundamentals/configuration#custom-config-providers).</span></span>

<span data-ttu-id="42785-131">Das vorhergehende Beispiel verwendet den Indexer für die Konfiguration, um Werte zu lesen.</span><span class="sxs-lookup"><span data-stu-id="42785-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="42785-132">Auf die Konfiguration außerhalb von `Startup`, verwenden Sie die [Optionen Muster](xref:fundamentals/configuration#options-config-objects).</span><span class="sxs-lookup"><span data-stu-id="42785-132">To access configuration outside of `Startup`, use the [options pattern](xref:fundamentals/configuration#options-config-objects).</span></span> <span data-ttu-id="42785-133">Die *Optionen Muster* wird weiter unten in diesem Artikel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="42785-133">The *options pattern* is shown later in this article.</span></span>

<span data-ttu-id="42785-134">Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen, z. B. Entwicklungs-, Test- und produktionsumgebung aufweisen.</span><span class="sxs-lookup"><span data-stu-id="42785-134">It's typical to have different configuration settings for different environments, for example, development, test, and production.</span></span> <span data-ttu-id="42785-135">Die `CreateDefaultBuilder` Erweiterungsmethode in einer ASP.NET Core 2.x-app (oder mit `AddJsonFile` und `AddEnvironmentVariables` direkt in einer ASP.NET Core 1.x-app) Konfigurationsanbieter für das Lesen von JSON-Dateien und das System Konfigurationsquellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="42785-135">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="42785-136">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="42785-136">*appsettings.json*</span></span>
* <span data-ttu-id="42785-137">* "appSettings". \<EnvironmentName > JSON</span><span class="sxs-lookup"><span data-stu-id="42785-137">*appsettings.\<EnvironmentName>.json</span></span>
* <span data-ttu-id="42785-138">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="42785-138">environment variables</span></span>

<span data-ttu-id="42785-139">Finden Sie unter [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) eine Erläuterung der Parameter.</span><span class="sxs-lookup"><span data-stu-id="42785-139">See [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="42785-140">`reloadOnChange`wird nur in ASP.NET Core 1.1 und höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="42785-140">`reloadOnChange` is only supported in ASP.NET Core 1.1 and higher.</span></span> 

<span data-ttu-id="42785-141">Konfigurationsquellen werden in der Reihenfolge gelesen, die sie angegeben sind.</span><span class="sxs-lookup"><span data-stu-id="42785-141">Configuration sources are read in the order they are specified.</span></span> <span data-ttu-id="42785-142">Im obigen Code werden die Umgebungsvariablen für die zuletzt gelesen.</span><span class="sxs-lookup"><span data-stu-id="42785-142">In the code above, the environment variables are read last.</span></span> <span data-ttu-id="42785-143">Alle Konfigurationswerte, die durch die Umgebung eingerichtet würde er in den beiden vorherigen Anbietern ersetzen.</span><span class="sxs-lookup"><span data-stu-id="42785-143">Any configuration values set through the environment would replace those set in the two previous providers.</span></span>

<span data-ttu-id="42785-144">Die Umgebung wird auf einen der in der Regel festgelegt `Development`, `Staging`, oder `Production`.</span><span class="sxs-lookup"><span data-stu-id="42785-144">The environment is typically set to one of `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="42785-145">Finden Sie unter [arbeiten mit mehreren Umgebungen](environments.md) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="42785-145">See [Working with Multiple Environments](environments.md) for more information.</span></span>

<span data-ttu-id="42785-146">Überlegungen zur Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="42785-146">Configuration considerations:</span></span>

* <span data-ttu-id="42785-147">`IOptionsSnapshot`Konfigurationsdaten können erneut laden, wenn sich ändert.</span><span class="sxs-lookup"><span data-stu-id="42785-147">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="42785-148">Verwendung `IOptionsSnapshot` Konfigurationsdaten werden erneut geladen werden sollen.</span><span class="sxs-lookup"><span data-stu-id="42785-148">Use `IOptionsSnapshot` if you need to reload configuration data.</span></span>  <span data-ttu-id="42785-149">Finden Sie unter [IOptionsSnapshot](#ioptionssnapshot) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="42785-149">See [IOptionsSnapshot](#ioptionssnapshot) for more information.</span></span>
* <span data-ttu-id="42785-150">-Konfigurationsschlüssel werden Groß-/Kleinschreibung.</span><span class="sxs-lookup"><span data-stu-id="42785-150">Configuration keys are case insensitive.</span></span>
* <span data-ttu-id="42785-151">Eine bewährte Methode ist Umgebungsvariablen letzten, an, sodass die lokale Umgebung, alles in bereitgestellten Konfigurationsdateien festlegen überschreiben kann.</span><span class="sxs-lookup"><span data-stu-id="42785-151">A best practice is to specify environment variables last, so that the local environment can override anything set in deployed configuration files.</span></span>
* <span data-ttu-id="42785-152">**Nie** Kennwörter oder andere vertraulichen Daten im Anbietercode Konfiguration oder in Konfigurationsdateien als nur-Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="42785-152">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="42785-153">Keine Produktion geheime Schlüssel in der Entwicklung verwenden, oder Umgebungen zu testen.</span><span class="sxs-lookup"><span data-stu-id="42785-153">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="42785-154">Geben Sie stattdessen geheime Schlüssel außerhalb der Projektstruktur auf, damit sie in das Repository nicht versehentlich eingetragen werden können.</span><span class="sxs-lookup"><span data-stu-id="42785-154">Instead, specify secrets outside the project tree, so they cannot be accidentally committed into your repository.</span></span> <span data-ttu-id="42785-155">Erfahren Sie mehr über [arbeiten mit mehreren Umgebungen](environments.md) und Verwalten von [sichere Speicherung von app-Kennwörter während der Entwicklung](../security/app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="42785-155">Learn more about [Working with Multiple Environments](environments.md) and managing [safe storage of app secrets during development](../security/app-secrets.md).</span></span>
* <span data-ttu-id="42785-156">Wenn `:` nicht ersetzen in Umgebungsvariablen verwendet, in Ihrem System, `:` mit `__` (doppelter Unterstrich).</span><span class="sxs-lookup"><span data-stu-id="42785-156">If `:` cannot be used in environment variables in your system,  replace `:`  with `__` (double underscore).</span></span>

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a><span data-ttu-id="42785-157">Mithilfe der Optionen und Konfigurationsobjekten</span><span class="sxs-lookup"><span data-stu-id="42785-157">Using Options and configuration objects</span></span>

<span data-ttu-id="42785-158">Das Muster Optionen verwendet benutzerdefinierte Optionsklassen, um eine Gruppe von verwandten Einstellungen darstellen.</span><span class="sxs-lookup"><span data-stu-id="42785-158">The options pattern uses custom options classes to represent a group of related settings.</span></span> <span data-ttu-id="42785-159">Es wird empfohlen, entkoppelte Klassen für jede Funktion in Ihrer app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="42785-159">We recommended that you create decoupled classes for each feature within your app.</span></span> <span data-ttu-id="42785-160">Entkoppelte Klassen entsprechen:</span><span class="sxs-lookup"><span data-stu-id="42785-160">Decoupled classes follow:</span></span>

* <span data-ttu-id="42785-161">Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/) : Klassen richten sich nur auf den Konfigurationseinstellungen, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="42785-161">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/) : Classes depend only on the configuration settings they use.</span></span>
* <span data-ttu-id="42785-162">[Trennung von Anliegen](http://deviq.com/separation-of-concerns/) : Einstellungen für die verschiedenen Teile Ihrer App sind nicht abhängige oder gekoppelten miteinander.</span><span class="sxs-lookup"><span data-stu-id="42785-162">[Separation of Concerns](http://deviq.com/separation-of-concerns/) : Settings for different parts of your app are not dependent or coupled with one another.</span></span>

<span data-ttu-id="42785-163">Die Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="42785-163">The options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="42785-164">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="42785-164">For example:</span></span>

<span data-ttu-id="42785-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-165">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]</span></span>

<a name=options-example></a>

<span data-ttu-id="42785-166">Im folgenden Code wird die JSON-Konfigurationsanbieter aktiviert.</span><span class="sxs-lookup"><span data-stu-id="42785-166">In the following code, the JSON configuration provider is enabled.</span></span> <span data-ttu-id="42785-167">Die `MyOptions` Klasse dem Dienstcontainer hinzugefügt und an Configuration gebunden ist.</span><span class="sxs-lookup"><span data-stu-id="42785-167">The `MyOptions` class is added to the service container and bound to configuration.</span></span>

<span data-ttu-id="42785-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span><span class="sxs-lookup"><span data-stu-id="42785-168">[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]</span></span>

<span data-ttu-id="42785-169">Die folgenden [Controller](../mvc/controllers/index.md) verwendet [Konstruktor Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#what-is-dependency-injection) auf [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) auf Einstellungen zugreifen:</span><span class="sxs-lookup"><span data-stu-id="42785-169">The following [controller](../mvc/controllers/index.md)  uses [constructor Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) on [`IOptions<TOptions>`](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) to access settings:</span></span>

<span data-ttu-id="42785-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="42785-170">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]</span></span>

<span data-ttu-id="42785-171">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="42785-171">With the following *appsettings.json* file:</span></span>

<span data-ttu-id="42785-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-172">[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]</span></span>

<span data-ttu-id="42785-173">Die `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_json, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="42785-173">The `HomeController.Index` method returns `option1 = value1_from_json, option2 = 2`.</span></span>

<span data-ttu-id="42785-174">Standard-apps wird nicht die gesamte Konfiguration in eine Optionsdatei für die einzelnen gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="42785-174">Typical apps won't bind the entire configuration to a single options file.</span></span> <span data-ttu-id="42785-175">Später zeige ich wie `GetSection` zu einem Abschnitt binden.</span><span class="sxs-lookup"><span data-stu-id="42785-175">Later on I'll show how to use `GetSection` to bind to a section.</span></span>

<span data-ttu-id="42785-176">Im folgenden Code wird eine zweite `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="42785-176">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="42785-177">Er verwendet ein Delegat zum Konfigurieren der Bindung mit `MyOptions`.</span><span class="sxs-lookup"><span data-stu-id="42785-177">It uses a delegate to configure the binding with `MyOptions`.</span></span>

<span data-ttu-id="42785-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span><span class="sxs-lookup"><span data-stu-id="42785-178">[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]</span></span>

<span data-ttu-id="42785-179">Sie können mehrere Konfigurationsanbieter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="42785-179">You can add multiple configuration providers.</span></span> <span data-ttu-id="42785-180">Konfigurationsanbieter stehen im NuGet-Pakete zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="42785-180">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="42785-181">Sie werden in Reihenfolge angewendet, die sie registriert sind.</span><span class="sxs-lookup"><span data-stu-id="42785-181">They are applied in order they are registered.</span></span>

<span data-ttu-id="42785-182">Jeder Aufruf von `Configure<TOptions>` Fügt eine `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="42785-182">Each call to `Configure<TOptions>` adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="42785-183">Im vorherigen Beispiel, die Werte der `Option1` und `Option2` werden in angegebenen *appsettings.json* –, aber der Wert des `Option1` durch den konfigurierten Delegaten überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="42785-183">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json* -- but the value of `Option1` is overridden by the configured delegate.</span></span> 

<span data-ttu-id="42785-184">Wenn mehr als ein Dienst aktiviert ist, die letzte Konfigurationsquelle angegeben "gewinnt" (setzt den Konfigurationswert).</span><span class="sxs-lookup"><span data-stu-id="42785-184">When more than one configuration service is enabled, the last configuration source specified "wins" (sets the configuration value).</span></span> <span data-ttu-id="42785-185">Im vorangehenden Code der `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_action, option2 = 2`.</span><span class="sxs-lookup"><span data-stu-id="42785-185">In the preceding code, the `HomeController.Index` method returns `option1 = value1_from_action, option2 = 2`.</span></span>

<span data-ttu-id="42785-186">Wenn Sie Optionen zur Konfiguration binden, jede Eigenschaft im Optionstyp Ihres einem Konfigurationsschlüssel des Formulars gebunden ist `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="42785-186">When you bind options to configuration, each property in your options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="42785-187">Z. B. die `MyOptions.Option1` Eigenschaft gebunden ist, auf den Schlüssel `Option1`, das Auslesen der `option1` Eigenschaft im *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="42785-187">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span> <span data-ttu-id="42785-188">Ein Beispiel für die untergeordnete Eigenschaft ist später in diesem Artikel dargestellt.</span><span class="sxs-lookup"><span data-stu-id="42785-188">A sub-property sample is shown later in this article.</span></span>

<span data-ttu-id="42785-189">Im folgenden Code ein drittes `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="42785-189">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="42785-190">Sie bindet `MySubOptions` Abschnitt `subsection` von der *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="42785-190">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

<span data-ttu-id="42785-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span><span class="sxs-lookup"><span data-stu-id="42785-191">[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]</span></span>

<span data-ttu-id="42785-192">Hinweis: Diese Erweiterungsmethode erfordert die `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="42785-192">Note: This extension method requires the `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet package.</span></span>

<span data-ttu-id="42785-193">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="42785-193">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="42785-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-194">[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]</span></span>

<span data-ttu-id="42785-195">Die `MySubOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="42785-195">The `MySubOptions` class:</span></span>

<span data-ttu-id="42785-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="42785-196">[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="42785-197">Mit den folgenden `Controller`:</span><span class="sxs-lookup"><span data-stu-id="42785-197">With the following `Controller`:</span></span>

<span data-ttu-id="42785-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="42785-198">[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]</span></span>

<span data-ttu-id="42785-199">`subOption1 = subvalue1_from_json, subOption2 = 200`wird zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="42785-199">`subOption1 = subvalue1_from_json, subOption2 = 200` is returned.</span></span>

<span data-ttu-id="42785-200">Sie können auch angeben von Optionen in einem Ansichtsmodell oder einfügen `IOptions<TOptions>` direkt in eine Sicht:</span><span class="sxs-lookup"><span data-stu-id="42785-200">You can also supply options in a view model or inject `IOptions<TOptions>` directly into a view:</span></span>

<span data-ttu-id="42785-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span><span class="sxs-lookup"><span data-stu-id="42785-201">[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]</span></span>

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a><span data-ttu-id="42785-202">IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="42785-202">IOptionsSnapshot</span></span>

<span data-ttu-id="42785-203">*Erfordert ASP.NET Core 1.1 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="42785-203">*Requires ASP.NET Core 1.1 or higher.*</span></span>

<span data-ttu-id="42785-204">`IOptionsSnapshot`unterstützt die Konfigurationsdaten werden erneut geladen, wenn sich die Konfigurationsdatei geändert hat.</span><span class="sxs-lookup"><span data-stu-id="42785-204">`IOptionsSnapshot` supports reloading configuration data when the configuration file has changed.</span></span> <span data-ttu-id="42785-205">Er verfügt über einen minimalen Aufwand.</span><span class="sxs-lookup"><span data-stu-id="42785-205">It has minimal overhead.</span></span> <span data-ttu-id="42785-206">Mit `IOptionsSnapshot` mit `reloadOnChange: true`, sind die Optionen zum gebunden `IConfiguration` und erneut geladen, wenn sich geändert.</span><span class="sxs-lookup"><span data-stu-id="42785-206">Using `IOptionsSnapshot` with `reloadOnChange: true`, the options are bound to `IConfiguration` and reloaded when changed.</span></span>

<span data-ttu-id="42785-207">Im folgende Beispiel wird veranschaulicht, wie ein neuer `IOptionsSnapshot` wird erstellt, nachdem *config.json* ändert.</span><span class="sxs-lookup"><span data-stu-id="42785-207">The following sample demonstrates how a new `IOptionsSnapshot` is created after *config.json* changes.</span></span> <span data-ttu-id="42785-208">Anforderungen an den Server werden zurückgegeben, die gleiche Uhrzeit *config.json* hat **nicht** geändert.</span><span class="sxs-lookup"><span data-stu-id="42785-208">Requests to the server will return the same time when *config.json* has **not** changed.</span></span> <span data-ttu-id="42785-209">Die erste Anforderung nach *config.json* Änderungen werden die Zeit angezeigt.</span><span class="sxs-lookup"><span data-stu-id="42785-209">The first request after *config.json* changes will show a new time.</span></span>

<span data-ttu-id="42785-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span><span class="sxs-lookup"><span data-stu-id="42785-210">[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]</span></span>

<span data-ttu-id="42785-211">Die folgende Abbildung zeigt die Serverausgabe:</span><span class="sxs-lookup"><span data-stu-id="42785-211">The following image shows the server output:</span></span>

![Browser Bild anzeigen "Letzte Aktualisierung: 11/22/2016 16:43 Uhr"](configuration/_static/first.png)

<span data-ttu-id="42785-213">Aktualisieren des Browsers ändert sich der Nachrichtenwert oder angezeigte Uhrzeit (Wenn *config.json* wurde nicht geändert).</span><span class="sxs-lookup"><span data-stu-id="42785-213">Refreshing the browser doesn't change the message value or time displayed (when *config.json* has not changed).</span></span>

<span data-ttu-id="42785-214">Ändern und speichern Sie die *config.json* und den Browser zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="42785-214">Change and save the  *config.json* and then refresh the browser:</span></span>

![Browser Bild anzeigen "," e: "Letzte Aktualisierung 11/22/2016 16:53:00 Uhr"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="42785-216">In-Memory-Anbieter und die Bindung an einer POCO-Klasse</span><span class="sxs-lookup"><span data-stu-id="42785-216">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="42785-217">Das folgende Beispiel zeigt, wie den in-Memory-Anbieter verwenden und auf eine Klasse gebunden:</span><span class="sxs-lookup"><span data-stu-id="42785-217">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

<span data-ttu-id="42785-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-218">[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]</span></span>

<span data-ttu-id="42785-219">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, aber Binden ermöglicht die Erstellung von Objekten.</span><span class="sxs-lookup"><span data-stu-id="42785-219">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="42785-220">Bindung ermöglicht es Ihnen POCO-Objekte oder Objektdiagramme ganzer abgerufen.</span><span class="sxs-lookup"><span data-stu-id="42785-220">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span> <span data-ttu-id="42785-221">Im folgende Beispiel wird gezeigt, wie zum Binden an `MyWindow` und das Muster Optionen mit einer ASP.NET-MVC-Anwendung Core verwenden:</span><span class="sxs-lookup"><span data-stu-id="42785-221">The following sample shows how to bind to `MyWindow` and use the options pattern with a ASP.NET Core MVC app:</span></span>

<span data-ttu-id="42785-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-222">[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]</span></span>

<span data-ttu-id="42785-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-223">[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]</span></span>

<span data-ttu-id="42785-224">Binden Sie die benutzerdefinierte Klasse in `ConfigureServices` beim Host zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="42785-224">Bind the custom class in `ConfigureServices` when building the host:</span></span>

<span data-ttu-id="42785-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span><span class="sxs-lookup"><span data-stu-id="42785-225">[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]</span></span>

<span data-ttu-id="42785-226">Zeigen Sie die Einstellungen aus der `HomeController`:</span><span class="sxs-lookup"><span data-stu-id="42785-226">Display the settings from the `HomeController`:</span></span>

<span data-ttu-id="42785-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-227">[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]</span></span>

### <a name="getvalue"></a><span data-ttu-id="42785-228">GetValue</span><span class="sxs-lookup"><span data-stu-id="42785-228">GetValue</span></span>

<span data-ttu-id="42785-229">Das folgende Beispiel zeigt die [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) Erweiterungsmethode:</span><span class="sxs-lookup"><span data-stu-id="42785-229">The following sample demonstrates the [GetValue<T>](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

<span data-ttu-id="42785-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span><span class="sxs-lookup"><span data-stu-id="42785-230">[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]</span></span>

<span data-ttu-id="42785-231">Die ConfigurationBinder `GetValue<T>` Methode können Sie einen Standardwert (80 in der Stichprobe) angeben.</span><span class="sxs-lookup"><span data-stu-id="42785-231">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="42785-232">`GetValue<T>`ist für einfache Szenarien, und Bindung erfolgt nicht auf die gesamte Abschnitte.</span><span class="sxs-lookup"><span data-stu-id="42785-232">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="42785-233">`GetValue<T>`Ruft die Skalarwerte aus `GetSection(key).Value` für einen bestimmten Typ konvertiert.</span><span class="sxs-lookup"><span data-stu-id="42785-233">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="binding-to-an-object-graph"></a><span data-ttu-id="42785-234">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="42785-234">Binding to an object graph</span></span>

<span data-ttu-id="42785-235">Sie können rekursiv Bindung auf jedes Objekt in einer Klasse.</span><span class="sxs-lookup"><span data-stu-id="42785-235">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="42785-236">Beachten Sie Folgendes `AppOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="42785-236">Consider the following `AppOptions` class:</span></span>

<span data-ttu-id="42785-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-237">[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]</span></span>

<span data-ttu-id="42785-238">Im folgende Beispiel bindet an die `AppOptions` Klasse:</span><span class="sxs-lookup"><span data-stu-id="42785-238">The following sample binds to the `AppOptions` class:</span></span>

<span data-ttu-id="42785-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span><span class="sxs-lookup"><span data-stu-id="42785-239">[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]</span></span>

<span data-ttu-id="42785-240">**ASP.NET Core 1.1** und höher können `Get<T>`, die gesamte Abschnitte funktioniert.</span><span class="sxs-lookup"><span data-stu-id="42785-240">**ASP.NET Core 1.1** and higher can use  `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="42785-241">`Get<T>`kann weitere Convienent als die Verwendung von `Bind`.</span><span class="sxs-lookup"><span data-stu-id="42785-241">`Get<T>` can be more convienent than using `Bind`.</span></span> <span data-ttu-id="42785-242">Der folgende Code zeigt, wie Sie `Get<T>` mit dem obigen Beispiel:</span><span class="sxs-lookup"><span data-stu-id="42785-242">The following code shows how to use `Get<T>` with the sample above:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

<span data-ttu-id="42785-243">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="42785-243">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="42785-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-244">[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]</span></span>

<span data-ttu-id="42785-245">Das Programm zeigt `Height 11`.</span><span class="sxs-lookup"><span data-stu-id="42785-245">The program displays `Height 11`.</span></span>

<span data-ttu-id="42785-246">Der folgende Code verwendet werden kann, Einheit Testen der Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="42785-246">The following code can be used to unit test the configuration:</span></span>

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

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a><span data-ttu-id="42785-247">Basic-Beispiel des benutzerdefinierten Anbieter für Entity Framework</span><span class="sxs-lookup"><span data-stu-id="42785-247">Basic sample of Entity Framework custom provider</span></span>

<span data-ttu-id="42785-248">In diesem Abschnitt wird ein grundlegende Konfigurationsanbieter, das Name-Wert-Paare aus einer Datenbank mithilfe von EF erstellt.</span><span class="sxs-lookup"><span data-stu-id="42785-248">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span> 

<span data-ttu-id="42785-249">Definieren einer `ConfigurationValue` Entität für die Konfigurationswerte in der Datenbank gespeichert:</span><span class="sxs-lookup"><span data-stu-id="42785-249">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

<span data-ttu-id="42785-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-250">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]</span></span>

<span data-ttu-id="42785-251">Hinzufügen einer `ConfigurationContext` zum Speichern und Zugreifen auf die konfigurierten Werte:</span><span class="sxs-lookup"><span data-stu-id="42785-251">Add a `ConfigurationContext` to store and access the configured values:</span></span>

<span data-ttu-id="42785-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="42785-252">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]</span></span>

<span data-ttu-id="42785-253">Erstellen Sie eine Klasse, die implementiert [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span><span class="sxs-lookup"><span data-stu-id="42785-253">Create an class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

<span data-ttu-id="42785-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="42785-254">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]</span></span>

<span data-ttu-id="42785-255">Erstellen Sie den standardkonfigurationsanbieter durch Vererben von [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="42785-255">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span>  <span data-ttu-id="42785-256">Der Konfigurationsanbieter initialisiert die Datenbank aus, wenn sie leer ist:</span><span class="sxs-lookup"><span data-stu-id="42785-256">The configuration provider initializes the database when it's empty:</span></span>

<span data-ttu-id="42785-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span><span class="sxs-lookup"><span data-stu-id="42785-257">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]</span></span>

<span data-ttu-id="42785-258">Die hervorgehobenen Werte aus der Datenbank ("value_from_ef_1" und "value_from_ef_2") werden angezeigt, wenn das Beispiel ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="42785-258">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="42785-259">Sie können Hinzufügen einer `EFConfigSource` Erweiterungsmethode für die Konfigurationsquelle hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="42785-259">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

<span data-ttu-id="42785-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span><span class="sxs-lookup"><span data-stu-id="42785-260">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]</span></span>

<span data-ttu-id="42785-261">Der folgende Code zeigt, wie Sie die Verwendung der benutzerdefinierten `EFConfigProvider`:</span><span class="sxs-lookup"><span data-stu-id="42785-261">The following code shows how to use the custom `EFConfigProvider`:</span></span>

<span data-ttu-id="42785-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span><span class="sxs-lookup"><span data-stu-id="42785-262">[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]</span></span>

<span data-ttu-id="42785-263">Beachten Sie das Beispiel fügt die benutzerdefinierte `EFConfigProvider` nach der JSON-Anbieter, also alle Einstellungen aus der Datenbank überschrieben aus der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="42785-263">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="42785-264">Mit den folgenden *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="42785-264">Using the following *appsettings.json* file:</span></span>

<span data-ttu-id="42785-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span><span class="sxs-lookup"><span data-stu-id="42785-265">[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]</span></span>

<span data-ttu-id="42785-266">Im folgenden wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42785-266">The following is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="42785-267">CommandLine Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="42785-267">CommandLine configuration provider</span></span>

<span data-ttu-id="42785-268">Im folgende Beispiel ermöglicht den Konfigurationsanbieter CommandLine letzten:</span><span class="sxs-lookup"><span data-stu-id="42785-268">The following sample enables the CommandLine configuration provider last:</span></span>

<span data-ttu-id="42785-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span><span class="sxs-lookup"><span data-stu-id="42785-269">[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]</span></span>

<span data-ttu-id="42785-270">Verwenden Sie die folgenden Konfigurationseinstellungen übergeben:</span><span class="sxs-lookup"><span data-stu-id="42785-270">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

<span data-ttu-id="42785-271">Das wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="42785-271">Which displays:</span></span>

```console
Hello Bob
Left 1234
```

<span data-ttu-id="42785-272">Die `GetSwitchMappings` Methode können Sie mit `-` statt `/` und entfernt die führenden Unterschlüsseln Präfixe.</span><span class="sxs-lookup"><span data-stu-id="42785-272">The `GetSwitchMappings` method allows you to use `-` rather than `/` and it strips the leading subkey prefixes.</span></span> <span data-ttu-id="42785-273">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="42785-273">For example:</span></span>

```console
dotnet run -MachineName=Bob -Left=7734
```

<span data-ttu-id="42785-274">Zeigt die:</span><span class="sxs-lookup"><span data-stu-id="42785-274">Displays:</span></span>

```console
Hello Bob
Left 7734
```

<span data-ttu-id="42785-275">Befehlszeilenargumente müssen einen Wert enthalten (es kann null sein).</span><span class="sxs-lookup"><span data-stu-id="42785-275">Command-line arguments must include a value (it can be null).</span></span> <span data-ttu-id="42785-276">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="42785-276">For example:</span></span>

```console
dotnet run /Profile:MachineName=
```

<span data-ttu-id="42785-277">Ist in Ordnung, aber</span><span class="sxs-lookup"><span data-stu-id="42785-277">Is OK, but</span></span>

```console
dotnet run /Profile:MachineName
```

<span data-ttu-id="42785-278">/ / ergibt eine Ausnahme.</span><span class="sxs-lookup"><span data-stu-id="42785-278">results in an exception.</span></span> <span data-ttu-id="42785-279">Wenn Sie ein Präfix Befehlszeilenoption von - oder--für das es keine entsprechende Switch Zuordnung angeben, wird eine Ausnahme ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="42785-279">An exception will be thrown if you specify a command-line switch prefix of - or -- for which there's no corresponding switch mapping.</span></span>

## <a name="the-webconfig-file"></a><span data-ttu-id="42785-280">Die Datei "Web.config"</span><span class="sxs-lookup"><span data-stu-id="42785-280">The web.config file</span></span>

<span data-ttu-id="42785-281">Ein *"Web.config"* Datei ist erforderlich, wenn Sie die app in IIS oder IIS Express hosten.</span><span class="sxs-lookup"><span data-stu-id="42785-281">A *web.config* file is required when you host the app in IIS or IIS-Express.</span></span> <span data-ttu-id="42785-282">*"Web.config"* Schaltet die AspNetCoreModule in IIS, um Ihre app zu starten.</span><span class="sxs-lookup"><span data-stu-id="42785-282">*web.config* turns on the AspNetCoreModule in IIS to launch your app.</span></span> <span data-ttu-id="42785-283">Einstellungen im *"Web.config"* die AspNetCoreModule in IIS für Ihre app starten und konfigurieren, Module und andere IIS-Einstellungen zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="42785-283">Settings in *web.config* enable the AspNetCoreModule in IIS to launch your app and configure other IIS settings and modules.</span></span> <span data-ttu-id="42785-284">Wenn Sie Visual Studio verwenden, und löschen *"Web.config"*, Visual Studio erstellt ein neues Konto.</span><span class="sxs-lookup"><span data-stu-id="42785-284">If you are using Visual Studio and delete *web.config*, Visual Studio will create a new one.</span></span>

### <a name="additional-notes"></a><span data-ttu-id="42785-285">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="42785-285">Additional notes</span></span>

* <span data-ttu-id="42785-286">(Dependency Injection, DI) wird nicht bis zum nach gesetzt `ConfigureServices` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="42785-286">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="42785-287">Das Konfigurationssystem ist nicht DI beachten.</span><span class="sxs-lookup"><span data-stu-id="42785-287">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="42785-288">`IConfiguration`verfügt über zwei spezialisierungen:</span><span class="sxs-lookup"><span data-stu-id="42785-288">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="42785-289">`IConfigurationRoot`Für den Stammknoten verwendet.</span><span class="sxs-lookup"><span data-stu-id="42785-289">`IConfigurationRoot`  Used for the root node.</span></span> <span data-ttu-id="42785-290">Können erneut auslösen.</span><span class="sxs-lookup"><span data-stu-id="42785-290">Can trigger a reload.</span></span>
  * <span data-ttu-id="42785-291">`IConfigurationSection`Stellt einen Abschnitt der Konfigurationswerte.</span><span class="sxs-lookup"><span data-stu-id="42785-291">`IConfigurationSection`  Represents a section of configuration values.</span></span> <span data-ttu-id="42785-292">Die `GetSection` und `GetChildren` -Methoden zurückgeben einer `IConfigurationSection`.</span><span class="sxs-lookup"><span data-stu-id="42785-292">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="42785-293">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="42785-293">Additional resources</span></span>

* [<span data-ttu-id="42785-294">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="42785-294">Working with Multiple Environments</span></span>](environments.md)
* [<span data-ttu-id="42785-295">Sicheres Speichern geheimer App-Schlüssel während der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="42785-295">Safe storage of app secrets during development</span></span>](../security/app-secrets.md)
* [<span data-ttu-id="42785-296">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="42785-296">Dependency Injection</span></span>](dependency-injection.md)
* [<span data-ttu-id="42785-297">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="42785-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
