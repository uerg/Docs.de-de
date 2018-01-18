---
title: Konfiguration in ASP.NET Core
author: rick-anderson
description: "Mithilfe der Konfigurations-API können Sie eine ASP.NET Core-App anhand verschiedener Methoden konfigurieren."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 1/11/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/index
ms.openlocfilehash: 0f8618898089418f709506aee5eb013f983dc294
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/17/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="9698b-103">Konfigurieren einer ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="9698b-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="9698b-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9698b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="9698b-105">Mithilfe der Konfigurations-API kann eine ASP.NET Core-Web-App basierend auf einer Liste von Name/Wert-Paaren konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="9698b-106">Die Konfiguration wird zur Runtime aus verschiedenen Quellen gelesen.</span><span class="sxs-lookup"><span data-stu-id="9698b-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="9698b-107">Sie können diese Name/Wert-Paare in einer Hierarchie mit mehreren Ebenen gruppieren.</span><span class="sxs-lookup"><span data-stu-id="9698b-107">You can group these name-value pairs into a multi-level hierarchy.</span></span>

<span data-ttu-id="9698b-108">Für Folgendes stehen Konfigurationsanbieter zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="9698b-108">There are configuration providers for:</span></span>

* <span data-ttu-id="9698b-109">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="9698b-109">File formats (INI, JSON, and XML)</span></span>
* <span data-ttu-id="9698b-110">Befehlszeilenargumente</span><span class="sxs-lookup"><span data-stu-id="9698b-110">Command-line arguments</span></span>
* <span data-ttu-id="9698b-111">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="9698b-111">Environment variables</span></span>
* <span data-ttu-id="9698b-112">Speicherinterne .NET Objekte</span><span class="sxs-lookup"><span data-stu-id="9698b-112">In-memory .NET objects</span></span>
* <span data-ttu-id="9698b-113">Ein verschlüsselter Benutzerspeicher</span><span class="sxs-lookup"><span data-stu-id="9698b-113">An encrypted user store</span></span>
* [<span data-ttu-id="9698b-114">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="9698b-114">Azure Key Vault</span></span>](xref:security/key-vault-configuration)
* <span data-ttu-id="9698b-115">Benutzerdefinierte Anbieter (installiert oder erstellt)</span><span class="sxs-lookup"><span data-stu-id="9698b-115">Custom providers (installed or created)</span></span>

<span data-ttu-id="9698b-116">Jeder Konfigurationswert ist einem Zeichenfolgenschlüssel zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="9698b-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="9698b-117">Für die Deserialisierung von Einstellungen in ein benutzerdefiniertes [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekt (eine einfache .NET-Klasse mit Eigenschaften) kann auf eine integrierte Bindungsunterstützung zurückgegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="9698b-118">Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar.</span><span class="sxs-lookup"><span data-stu-id="9698b-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="9698b-119">Weitere Informationen zur Verwendung des Optionsmusters finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="9698b-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="9698b-120">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9698b-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="9698b-121">JSON-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="9698b-121">JSON configuration</span></span>

<span data-ttu-id="9698b-122">Die folgende Konsolen-App verwendet den JSON-Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="9698b-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="9698b-123">Die App liest die folgenden Konfigurationseinstellungen und zeigt diese an:</span><span class="sxs-lookup"><span data-stu-id="9698b-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="9698b-124">Eine Konfiguration besteht aus einer hierarchischen Liste von Name/Wert-Paaren, in denen die Knoten durch einen Doppelpunkt getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="9698b-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="9698b-125">Um einen Wert abzurufen, rufen Sie mit dem entsprechenden Schlüssel des Elements den `Configuration`-Indexer auf:</span><span class="sxs-lookup"><span data-stu-id="9698b-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=24-24)]

<span data-ttu-id="9698b-126">Verwenden Sie für die Arbeit mit Arrays in JSON-formatierten Konfigurationsquellen einen Arrayindex als Teil der durch Doppelpunkte getrennten Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="9698b-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="9698b-127">Im folgenden Beispiel wird der Name des ersten Elements im vorherigen `wizards`-Array abgerufen:</span><span class="sxs-lookup"><span data-stu-id="9698b-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="9698b-128">Name/Wert-Paare, die in die integrierten [Konfigurationsanbieter](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration) geschrieben werden, werden **nicht** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="9698b-128">Name-value pairs written to the built-in [Configuration](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="9698b-129">Allerdings können Sie einen benutzerdefinierten Anbieter erstellen, der Werte speichert.</span><span class="sxs-lookup"><span data-stu-id="9698b-129">However, you can create a custom provider that saves values.</span></span> <span data-ttu-id="9698b-130">Weitere Informationen finden Sie im Artikel zum [benutzerdefinierten Konfigurationsanbieter](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="9698b-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="9698b-131">Im vorhergehenden Beispiel wird der Konfigurationsindexer zum Lesen von Werten verwendet.</span><span class="sxs-lookup"><span data-stu-id="9698b-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="9698b-132">Um außerhalb von `Startup` auf die Konfiguration zuzugreifen, verwenden Sie das *Optionsmuster*.</span><span class="sxs-lookup"><span data-stu-id="9698b-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="9698b-133">Weitere Informationen finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="9698b-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>


## <a name="configuration-by-environment"></a><span data-ttu-id="9698b-134">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="9698b-134">Configuration by environment</span></span>

<span data-ttu-id="9698b-135">Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen (z.B. für Entwicklung, Tests und Produktion) zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="9698b-135">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="9698b-136">Durch die Erweiterungsmethode `CreateDefaultBuilder` in einer ASP.NET Core 2.x-App (oder durch die direkte Verwendung von `AddJsonFile` und `AddEnvironmentVariables` in einer ASP.NET Core 1.x-App) werden Konfigurationsanbieter zum Lesen von JSON-Dateien und Systemkonfigurationsquellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="9698b-136">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="9698b-137">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="9698b-137">*appsettings.json*</span></span>
* <span data-ttu-id="9698b-138">*appsettings.\<Umgebungsname>.json*</span><span class="sxs-lookup"><span data-stu-id="9698b-138">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="9698b-139">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="9698b-139">Environment variables</span></span>

<span data-ttu-id="9698b-140">ASP.NET Core 1.x-Apps müssen `AddJsonFile` und [AddEnvironmentVariables](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables #Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="9698b-140">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables #Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="9698b-141">Eine Erläuterung der Parameter finden Sie unter [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="9698b-141">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="9698b-142">`reloadOnChange` wird nur in ASP.NET Core 1.1 und höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="9698b-142">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="9698b-143">Konfigurationsquellen werden in der Reihenfolge, in der sie angegeben sind, gelesen.</span><span class="sxs-lookup"><span data-stu-id="9698b-143">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="9698b-144">Im vorangehenden Code werden die Umgebungsvariablen zuletzt gelesen.</span><span class="sxs-lookup"><span data-stu-id="9698b-144">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="9698b-145">Alle über die Umgebung festgelegten Konfigurationswerte ersetzen diejenigen in den beiden vorherigen Anbietern.</span><span class="sxs-lookup"><span data-stu-id="9698b-145">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="9698b-146">Beachten Sie die folgende *appsettings.Staging.json*-Datei:</span><span class="sxs-lookup"><span data-stu-id="9698b-146">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[Main](index/sample/appsettings.Staging.json)]

<span data-ttu-id="9698b-147">Wenn die Umgebung auf `Staging` festgelegt ist, liest die folgende `Configure`-Methode den Wert von `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="9698b-147">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="9698b-148">Die Umgebung ist in der Regel auf `Development`, `Staging` oder `Production` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="9698b-148">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="9698b-149">Weitere Informationen finden Sie unter [Working with multiple environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="9698b-149">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="9698b-150">Überlegungen zur Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="9698b-150">Configuration considerations:</span></span>

* <span data-ttu-id="9698b-151">`IOptionsSnapshot` kann Konfigurationsdaten erneut laden, wenn sich diese ändern.</span><span class="sxs-lookup"><span data-stu-id="9698b-151">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="9698b-152">Weitere Informationen finden Sie unter [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="9698b-152">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="9698b-153">Bei Konfigurationsschlüsseln wird die Groß-/Kleinschreibung **nicht** beachtet.</span><span class="sxs-lookup"><span data-stu-id="9698b-153">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="9698b-154">Speichern Sie **niemals** Kennwörter oder andere sensible Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="9698b-154">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="9698b-155">Verwenden Sie keine Produktionsgeheimnisse in Ihren Entwicklungs- oder Testumgebungen.</span><span class="sxs-lookup"><span data-stu-id="9698b-155">Don't use production secrets in your development or test environments.</span></span> <span data-ttu-id="9698b-156">Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in Ihr Repository übernommen werden können.</span><span class="sxs-lookup"><span data-stu-id="9698b-156">Specify secrets outside of the project so that they can't be accidentally committed to your repository.</span></span> <span data-ttu-id="9698b-157">Erfahren Sie mehr zum [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) und einer [sicheren Speicherung von App-Geheimnissen während der Entwicklung](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="9698b-157">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="9698b-158">Wenn in Umgebungsvariablen in Ihrem System kein Doppelpunkt (`:`) verwendet werden kann, ersetzen Sie den Doppelpunkt (`:`) durch zwei Unterstriche (`__`).</span><span class="sxs-lookup"><span data-stu-id="9698b-158">If a colon (`:`) can't be used in environment variables on your system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="9698b-159">In-Memory-Anbieter und Bindung an eine POCO-Klasse</span><span class="sxs-lookup"><span data-stu-id="9698b-159">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="9698b-160">Im folgenden Beispiel wird gezeigt, wie der In-Memory-Anbieter verwendet und an eine Klasse gebunden wird:</span><span class="sxs-lookup"><span data-stu-id="9698b-160">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

<span data-ttu-id="9698b-161">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, durch eine Bindung wird jedoch die Erstellung von Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="9698b-161">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="9698b-162">Durch eine Bindung können Sie POCO-Objekte oder sogar ganze Objektdiagramme abrufen.</span><span class="sxs-lookup"><span data-stu-id="9698b-162">Binding allows you to retrieve POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="9698b-163">GetValue</span><span class="sxs-lookup"><span data-stu-id="9698b-163">GetValue</span></span>

<span data-ttu-id="9698b-164">Im folgenden Beispiel wird die Erweiterungsmethode [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) gezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-164">The following sample demonstrates the [GetValue&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) extension method:</span></span>

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="9698b-165">Durch die `GetValue<T>`-Methode von ConfigurationBinder können Sie einen Standardwert (hier 80) angeben.</span><span class="sxs-lookup"><span data-stu-id="9698b-165">The ConfigurationBinder's `GetValue<T>` method allows you to specify a default value (80 in the sample).</span></span> <span data-ttu-id="9698b-166">`GetValue<T>` ist für einfache Szenarien vorgesehen und kann nicht an gesamte Abschnitte gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-166">`GetValue<T>` is for simple scenarios and does not bind to entire sections.</span></span> <span data-ttu-id="9698b-167">`GetValue<T>` ruft skalare Werte von `GetSection(key).Value` – konvertiert in einen bestimmten Typ – ab.</span><span class="sxs-lookup"><span data-stu-id="9698b-167">`GetValue<T>` gets scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="9698b-168">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="9698b-168">Bind to an object graph</span></span>

<span data-ttu-id="9698b-169">Sie können für jedes Objekt in einer Klasse eine rekursive Bindung vornehmen.</span><span class="sxs-lookup"><span data-stu-id="9698b-169">You can recursively bind to each object in a class.</span></span> <span data-ttu-id="9698b-170">Betrachten wir einmal die folgende `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="9698b-170">Consider the following `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="9698b-171">Im folgenden Beispiel erfolgt eine Bindung an die `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="9698b-171">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="9698b-172">Bei **ASP.NET Core 1.1** und höher kann `Get<T>` für gesamte Abschnitte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-172">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="9698b-173">`Get<T>` ist eventuell praktischer als `Bind`.</span><span class="sxs-lookup"><span data-stu-id="9698b-173">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="9698b-174">Der folgende Code zeigt, wie `Get<T>` in Bezug auf das vorherige Beispiel verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="9698b-174">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="9698b-175">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9698b-175">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="9698b-176">Das Programm zeigt `Height 11` an.</span><span class="sxs-lookup"><span data-stu-id="9698b-176">The program displays `Height 11`.</span></span>

<span data-ttu-id="9698b-177">Mit dem folgenden Code kann ein Komponententest für die Konfiguration durchgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="9698b-177">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="9698b-178">Erstellen eines benutzerdefinierten Entity Framework-Anbieters</span><span class="sxs-lookup"><span data-stu-id="9698b-178">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="9698b-179">In diesem Abschnitt wird ein Standardkonfigurationsanbieter erstellt, der mithilfe des EF Name/Wert-Paare aus einer Datenbank liest.</span><span class="sxs-lookup"><span data-stu-id="9698b-179">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="9698b-180">Definieren Sie eine `ConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="9698b-180">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="9698b-181">Fügen Sie eine `ConfigurationContext`-Instanz hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen:</span><span class="sxs-lookup"><span data-stu-id="9698b-181">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="9698b-182">Erstellen Sie eine Klasse, die [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource) implementiert:</span><span class="sxs-lookup"><span data-stu-id="9698b-182">Create a class that implements [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="9698b-183">Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span><span class="sxs-lookup"><span data-stu-id="9698b-183">Create the custom configuration provider by inheriting from [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).</span></span> <span data-ttu-id="9698b-184">Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist:</span><span class="sxs-lookup"><span data-stu-id="9698b-184">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="9698b-185">Bei Ausführung des Beispiels werden die hervorgehobenen Werte von der Datenbank („value_from_ef_1“ und „value_from_ef_2“) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="9698b-185">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="9698b-186">Sie können zum Hinzufügen der Konfigurationsquelle eine `EFConfigSource`-Erweiterungsmethode hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="9698b-186">You can add an `EFConfigSource` extension method for adding the configuration source:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="9698b-187">Im folgenden Code wird die Verwendung des benutzerdefinierten Anbieters `EFConfigProvider` veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="9698b-187">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="9698b-188">Beachten Sie, dass der benutzerdefinierte Anbieter `EFConfigProvider` im Beispiel nach dem JSON-Anbieter hinzugefügt wird, sodass Datenbankeinstellungen die Einstellungen aus der Datei *appsettings.json* überschreiben.</span><span class="sxs-lookup"><span data-stu-id="9698b-188">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="9698b-189">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9698b-189">Using the following *appsettings.json* file:</span></span>

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="9698b-190">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-190">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="9698b-191">CommandLine-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="9698b-191">CommandLine configuration provider</span></span>

<span data-ttu-id="9698b-192">Der [CommandLine-Konfigurationsanbieter](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) erhält Schlüssel/Wert-Paare der Befehlszeilenargumente für die Konfiguration zur Runtime.</span><span class="sxs-lookup"><span data-stu-id="9698b-192">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="9698b-193">Beispiel für den CommandLine-Konfigurationsanbieter anzeigen oder herunterladen</span><span class="sxs-lookup"><span data-stu-id="9698b-193">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="9698b-194">Einrichten und Verwenden des CommandLine-Konfigurationsanbieters</span><span class="sxs-lookup"><span data-stu-id="9698b-194">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="9698b-195">Standardkonfiguration</span><span class="sxs-lookup"><span data-stu-id="9698b-195">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="9698b-196">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine`-Erweiterungsmethode für eine [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)-Instanz ab:</span><span class="sxs-lookup"><span data-stu-id="9698b-196">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="9698b-197">Nach der Ausführung des Codes wird die folgende Ausgabe angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-197">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="9698b-198">Durch die Übergabe von Schlüssel/Wert-Paaren der Argumente in der Befehlszeile werden die Werte von `Profile:MachineName` und `App:MainWindow:Left` geändert:</span><span class="sxs-lookup"><span data-stu-id="9698b-198">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="9698b-199">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-199">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="9698b-200">Um die von anderen Konfigurationsanbietern bereitgestellte Konfiguration mit der Befehlszeilenkonfiguration zu überschreiben, rufen Sie die letzte Funktion `AddCommandLine` von `ConfigurationBuilder` auf:</span><span class="sxs-lookup"><span data-stu-id="9698b-200">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9698b-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9698b-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9698b-202">Typische ASP.NET Core 2.x-Apps verwenden für die Erstellung des Hosts die statische Komfortmethode `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="9698b-202">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="9698b-203">`CreateDefaultBuilder` lädt die optionale Konfiguration von *appsettings.json*, *appsettings.{Umgebung}.json*, [Benutzergeheimnisse](xref:security/app-secrets) (in der `Development`-Umgebung), Umgebungsvariablen und Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="9698b-203">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="9698b-204">Der CommandLine-Konfigurationsanbieter wird zuletzt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="9698b-204">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="9698b-205">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="9698b-205">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="9698b-206">Für *appsettings*-Dateien, bei denen:</span><span class="sxs-lookup"><span data-stu-id="9698b-206">For *appsettings* files where:</span></span>

* <span data-ttu-id="9698b-207">`reloadOnChange` aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="9698b-207">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="9698b-208">die gleiche Einstellung in den Befehlszeilenargumenten sowie eine *appsettings*-Datei enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="9698b-208">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="9698b-209">die *appsettings*-Datei, die das entsprechende Befehlszeilenargument enthält, nach dem Start der App geändert wird.</span><span class="sxs-lookup"><span data-stu-id="9698b-209">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="9698b-210">Wenn alle oben genannten Bedingungen erfüllt sind, werden die Befehlszeilenargumente überschrieben.</span><span class="sxs-lookup"><span data-stu-id="9698b-210">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="9698b-211">Die ASP.NET Core 2.x-App kann „WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)“ anstatt „CreateDefaultBuilder`. When using `WebHostBuilder“ verwenden und die Konfiguration manuell mit [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) festlegen.</span><span class="sxs-lookup"><span data-stu-id="9698b-211">ASP.NET Core 2.x app can use WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of \`\`CreateDefaultBuilder`. When using `WebHostBuilder\`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="9698b-212">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9698b-212">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9698b-213">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9698b-213">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9698b-214">Erstellen Sie einen [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)-Anbieter, und rufen Sie die `AddCommandLine`-Methode auf, um den CommandLine-Konfigurationsanbieter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="9698b-214">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="9698b-215">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="9698b-215">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="9698b-216">Wenden Sie mit der `UseConfiguration`-Methode die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) an:</span><span class="sxs-lookup"><span data-stu-id="9698b-216">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="9698b-217">Argumente</span><span class="sxs-lookup"><span data-stu-id="9698b-217">Arguments</span></span>

<span data-ttu-id="9698b-218">Über die Befehlszeile übergebene Argumente müssen eines der zwei Formate, die in der folgenden Tabelle aufgeführt sind, aufweisen:</span><span class="sxs-lookup"><span data-stu-id="9698b-218">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="9698b-219">Argumentformat</span><span class="sxs-lookup"><span data-stu-id="9698b-219">Argument format</span></span>                                                     | <span data-ttu-id="9698b-220">Beispiel</span><span class="sxs-lookup"><span data-stu-id="9698b-220">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="9698b-221">Einzelnes Argument: ein durch ein Gleichheitszeichen (`=`) getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="9698b-221">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="9698b-222">Sequenz der zwei Argumente: ein durch einen Leerraum getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="9698b-222">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="9698b-223">**Einzelnes Argument**</span><span class="sxs-lookup"><span data-stu-id="9698b-223">**Single argument**</span></span>

<span data-ttu-id="9698b-224">Der Wert muss einem Gleichheitszeichen (`=`) nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="9698b-224">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="9698b-225">Der Wert kann NULL sein (z.B. `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="9698b-225">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="9698b-226">Der Schlüssel darf einen Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="9698b-226">The key may have a prefix.</span></span>

| <span data-ttu-id="9698b-227">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="9698b-227">Key prefix</span></span>               | <span data-ttu-id="9698b-228">Beispiel</span><span class="sxs-lookup"><span data-stu-id="9698b-228">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="9698b-229">Ohne Präfix</span><span class="sxs-lookup"><span data-stu-id="9698b-229">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="9698b-230">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="9698b-230">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="9698b-231">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="9698b-231">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="9698b-232">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="9698b-232">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="9698b-233">&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-233">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="9698b-234">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="9698b-234">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="9698b-235">Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="9698b-235">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="9698b-236">**Sequenz aus zwei Argumenten**</span><span class="sxs-lookup"><span data-stu-id="9698b-236">**Sequence of two arguments**</span></span>

<span data-ttu-id="9698b-237">Der Wert darf nicht null und muss dem Schlüssel getrennt durch einen Leerraum nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="9698b-237">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="9698b-238">Der Schlüssel muss ein Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="9698b-238">The key must have a prefix.</span></span>

| <span data-ttu-id="9698b-239">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="9698b-239">Key prefix</span></span>               | <span data-ttu-id="9698b-240">Beispiel</span><span class="sxs-lookup"><span data-stu-id="9698b-240">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="9698b-241">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="9698b-241">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="9698b-242">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="9698b-242">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="9698b-243">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="9698b-243">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="9698b-244">&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-244">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="9698b-245">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="9698b-245">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="9698b-246">Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="9698b-246">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="9698b-247">Doppelte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="9698b-247">Duplicate keys</span></span>

<span data-ttu-id="9698b-248">Wenn Schlüssel doppelt angegeben werden, wird das letzte Schlüssel/Wert-Paar verwendet.</span><span class="sxs-lookup"><span data-stu-id="9698b-248">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="9698b-249">Switchmappings</span><span class="sxs-lookup"><span data-stu-id="9698b-249">Switch mappings</span></span>

<span data-ttu-id="9698b-250">Bei der manuellen Erstellung von Konfigurationen mit `ConfigurationBuilder` können Sie optional ein Switchmappingwörterbuch für die `AddCommandLine`-Methode angeben.</span><span class="sxs-lookup"><span data-stu-id="9698b-250">When manually building configuration with `ConfigurationBuilder`, you can optionally provide a switch mappings dictionary to the `AddCommandLine` method.</span></span> <span data-ttu-id="9698b-251">Mit Switchmappings können Sie eine Logik zum Ersetzen von Schlüsselnamen angeben.</span><span class="sxs-lookup"><span data-stu-id="9698b-251">Switch mappings allow you to provide key name replacement logic.</span></span>

<span data-ttu-id="9698b-252">Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft.</span><span class="sxs-lookup"><span data-stu-id="9698b-252">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="9698b-253">Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zur Festlegung der Konfiguration zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="9698b-253">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="9698b-254">Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="9698b-254">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="9698b-255">Regeln für Schlüssel von Switchmappingwörterbüchern:</span><span class="sxs-lookup"><span data-stu-id="9698b-255">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="9698b-256">Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.</span><span class="sxs-lookup"><span data-stu-id="9698b-256">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="9698b-257">Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="9698b-257">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="9698b-258">Im folgenden Beispiel können durch die `GetSwitchMappings`-Methode einzelne Gedankenstriche (`-`) als Schlüsselpräfix in Befehlszeilenargumenten verwendet und führende Unterschlüsselpräfixe vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="9698b-258">In the following example, the `GetSwitchMappings` method allows your command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="9698b-259">Ohne die Angabe von Befehlszeilenargumenten legt das auf `AddInMemoryCollection` festgelegte Wörterbuch die Konfigurationswerte fest.</span><span class="sxs-lookup"><span data-stu-id="9698b-259">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="9698b-260">Führen Sie die App mit dem folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="9698b-260">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="9698b-261">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-261">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="9698b-262">Verwenden Sie den folgenden Code, um die Konfigurationseinstellungen zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="9698b-262">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="9698b-263">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="9698b-263">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="9698b-264">Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten:</span><span class="sxs-lookup"><span data-stu-id="9698b-264">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="9698b-265">Key</span><span class="sxs-lookup"><span data-stu-id="9698b-265">Key</span></span>            | <span data-ttu-id="9698b-266">Wert</span><span class="sxs-lookup"><span data-stu-id="9698b-266">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="9698b-267">Führen Sie zur Veranschaulichung des Schlüsselwechsels mit dem Wörterbuch den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="9698b-267">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="9698b-268">Die Befehlszeilenschlüssel sind ausgetauscht.</span><span class="sxs-lookup"><span data-stu-id="9698b-268">The command-line keys are swapped.</span></span> <span data-ttu-id="9698b-269">Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left` an:</span><span class="sxs-lookup"><span data-stu-id="9698b-269">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a><span data-ttu-id="9698b-270">Die Datei „Web.config“</span><span class="sxs-lookup"><span data-stu-id="9698b-270">The web.config file</span></span>

<span data-ttu-id="9698b-271">Eine Datei namens *Web.config* ist erforderlich, wenn Sie die App in IIS oder IIS Express hosten.</span><span class="sxs-lookup"><span data-stu-id="9698b-271">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="9698b-272">Die Einstellungen in der Datei *Web.config* aktivieren das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), um die App zu starten und andere IIS-Einstellungen und -Module zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="9698b-272">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="9698b-273">Wenn die Datei *Web.config* nicht vorhanden ist und die Projektdatei `<Project Sdk="Microsoft.NET.Sdk.Web">` enthält, wird bei der Veröffentlichung des Projekts eine Datei namens *Web.config* in der veröffentlichten Ausgabe (dem Ordner *publish*) erstellt.</span><span class="sxs-lookup"><span data-stu-id="9698b-273">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="9698b-274">Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index#webconfig).</span><span class="sxs-lookup"><span data-stu-id="9698b-274">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig).</span></span>

## <a name="additional-notes"></a><span data-ttu-id="9698b-275">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="9698b-275">Additional notes</span></span>

* <span data-ttu-id="9698b-276">Die Abhängigkeitsinjektion (Dependency Injection, DI) wird erst nach `ConfigureServices` eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="9698b-276">Dependency Injection (DI) is not set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="9698b-277">Das Konfigurationssystem ist nicht DI-fähig.</span><span class="sxs-lookup"><span data-stu-id="9698b-277">The configuration system is not DI aware.</span></span>
* <span data-ttu-id="9698b-278">`IConfiguration` weist zwei Spezialisierungen auf:</span><span class="sxs-lookup"><span data-stu-id="9698b-278">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="9698b-279">`IConfigurationRoot` Wird für den Stammknoten verwendet.</span><span class="sxs-lookup"><span data-stu-id="9698b-279">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="9698b-280">Kann einen erneuten Ladevorgang auslösen.</span><span class="sxs-lookup"><span data-stu-id="9698b-280">Can trigger a reload.</span></span>
  * <span data-ttu-id="9698b-281">`IConfigurationSection` Stellt einen Abschnitt der Konfigurationswerte dar.</span><span class="sxs-lookup"><span data-stu-id="9698b-281">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="9698b-282">Die Methoden `GetSection` und `GetChildren` geben `IConfigurationSection` zurück.</span><span class="sxs-lookup"><span data-stu-id="9698b-282">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="9698b-283">Verwenden Sie [IConfigurationRoot](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.iconfigurationroot), wenn Sie Konfigurationen neu laden müssen oder Zugriff auf jeden Provider benötigen.</span><span class="sxs-lookup"><span data-stu-id="9698b-283">Use [IConfigurationRoot](https://docs.microsoft.com/ dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or need access to each provider.</span></span> <span data-ttu-id="9698b-284">Keine dieser Situationen tritt häufig auf.</span><span class="sxs-lookup"><span data-stu-id="9698b-284">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9698b-285">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="9698b-285">Additional resources</span></span>

* [<span data-ttu-id="9698b-286">Optionen</span><span class="sxs-lookup"><span data-stu-id="9698b-286">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="9698b-287">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="9698b-287">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="9698b-288">Sicheres Speichern geheimer App-Schlüssel während der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="9698b-288">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="9698b-289">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="9698b-289">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="9698b-290">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="9698b-290">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="9698b-291">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="9698b-291">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
