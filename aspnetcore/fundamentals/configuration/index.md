---
title: Konfiguration in ASP.NET Core
author: rick-anderson
description: "Mithilfe der Konfigurations-API können Sie eine ASP.NET Core-App anhand verschiedener Methoden konfigurieren."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/index
ms.openlocfilehash: 7c41621db835b452c9aad9463a9ffccdf0c06484
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="configure-an-aspnet-core-app"></a><span data-ttu-id="374cf-103">Konfigurieren einer ASP.NET Core-App</span><span class="sxs-lookup"><span data-stu-id="374cf-103">Configure an ASP.NET Core App</span></span>

<span data-ttu-id="374cf-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="374cf-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="374cf-105">Mithilfe der Konfigurations-API kann eine ASP.NET Core-Web-App basierend auf einer Liste von Name/Wert-Paaren konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-105">The Configuration API provides a way to configure an ASP.NET Core web app based on a list of name-value pairs.</span></span> <span data-ttu-id="374cf-106">Die Konfiguration wird zur Runtime aus verschiedenen Quellen gelesen.</span><span class="sxs-lookup"><span data-stu-id="374cf-106">Configuration is read at runtime from multiple sources.</span></span> <span data-ttu-id="374cf-107">Name-Wert-Paare können in eine Hierarchie mit mehreren Ebenen gruppiert werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-107">Name-value pairs can be grouped into a multi-level hierarchy.</span></span>

<span data-ttu-id="374cf-108">Für Folgendes stehen Konfigurationsanbieter zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="374cf-108">There are configuration providers for:</span></span>

* <span data-ttu-id="374cf-109">Dateiformate (INI, JSON und XML)</span><span class="sxs-lookup"><span data-stu-id="374cf-109">File formats (INI, JSON, and XML).</span></span>
* <span data-ttu-id="374cf-110">Befehlszeilenargumenten</span><span class="sxs-lookup"><span data-stu-id="374cf-110">Command-line arguments.</span></span>
* <span data-ttu-id="374cf-111">Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="374cf-111">Environment variables.</span></span>
* <span data-ttu-id="374cf-112">Speicherinterne .NET-Objekte</span><span class="sxs-lookup"><span data-stu-id="374cf-112">In-memory .NET objects.</span></span>
* <span data-ttu-id="374cf-113">Der unverschlüsselte Speicher von [Secret Manager](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="374cf-113">The unencrypted [Secret Manager](xref:security/app-secrets) storage.</span></span>
* <span data-ttu-id="374cf-114">Ein unverschlüsselter Benutzerspeicher, z.B. [Azure Key Vault](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="374cf-114">An encrypted user store, such as [Azure Key Vault](xref:security/key-vault-configuration).</span></span>
* <span data-ttu-id="374cf-115">Benutzerdefinierte Anbieter (installiert oder erstellt).</span><span class="sxs-lookup"><span data-stu-id="374cf-115">Custom providers (installed or created).</span></span>

<span data-ttu-id="374cf-116">Jeder Konfigurationswert ist einem Zeichenfolgenschlüssel zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="374cf-116">Each configuration value maps to a string key.</span></span> <span data-ttu-id="374cf-117">Für die Deserialisierung von Einstellungen in ein benutzerdefiniertes [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekt (eine einfache .NET-Klasse mit Eigenschaften) kann auf eine integrierte Bindungsunterstützung zurückgegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-117">There's built-in binding support to deserialize settings into a custom [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) object (a simple .NET class with properties).</span></span>

<span data-ttu-id="374cf-118">Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar.</span><span class="sxs-lookup"><span data-stu-id="374cf-118">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="374cf-119">Weitere Informationen zur Verwendung des Optionsmusters finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="374cf-119">For more information on using the options pattern, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

<span data-ttu-id="374cf-120">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="374cf-120">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="json-configuration"></a><span data-ttu-id="374cf-121">JSON-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="374cf-121">JSON configuration</span></span>

<span data-ttu-id="374cf-122">Die folgende Konsolen-App verwendet den JSON-Konfigurationsanbieter:</span><span class="sxs-lookup"><span data-stu-id="374cf-122">The following console app uses the JSON configuration provider:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs)]

<span data-ttu-id="374cf-123">Die App liest die folgenden Konfigurationseinstellungen und zeigt diese an:</span><span class="sxs-lookup"><span data-stu-id="374cf-123">The app reads and displays the following configuration settings:</span></span>

[!code-json[](index/sample/ConfigJson/appsettings.json)]

<span data-ttu-id="374cf-124">Eine Konfiguration besteht aus einer hierarchischen Liste von Name/Wert-Paaren, in denen die Knoten durch einen Doppelpunkt getrennt sind.</span><span class="sxs-lookup"><span data-stu-id="374cf-124">Configuration consists of a hierarchical list of name-value pairs in which the nodes are separated by a colon.</span></span> <span data-ttu-id="374cf-125">Um einen Wert abzurufen, rufen Sie mit dem entsprechenden Schlüssel des Elements den `Configuration`-Indexer auf:</span><span class="sxs-lookup"><span data-stu-id="374cf-125">To retrieve a value, access the `Configuration` indexer with the corresponding item's key:</span></span>

[!code-csharp[](index/sample/ConfigJson/Program.cs?range=21-22)]

<span data-ttu-id="374cf-126">Verwenden Sie für die Arbeit mit Arrays in JSON-formatierten Konfigurationsquellen einen Arrayindex als Teil der durch Doppelpunkte getrennten Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="374cf-126">To work with arrays in JSON-formatted configuration sources, use an array index as part of the colon-separated string.</span></span> <span data-ttu-id="374cf-127">Im folgenden Beispiel wird der Name des ersten Elements im vorherigen `wizards`-Array abgerufen:</span><span class="sxs-lookup"><span data-stu-id="374cf-127">The following example gets the name of the first item in the preceding `wizards` array:</span></span>

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

<span data-ttu-id="374cf-128">Name/Wert-Paare, die in die integrierten [Konfigurationsanbieter](/dotnet/api/microsoft.extensions.configuration) geschrieben werden, werden **nicht** beibehalten.</span><span class="sxs-lookup"><span data-stu-id="374cf-128">Name-value pairs written to the built-in [Configuration](/dotnet/api/microsoft.extensions.configuration) providers are **not** persisted.</span></span> <span data-ttu-id="374cf-129">Allerdings kann ein benutzerdefinierter Anbieter erstellt werden, der Werte speichert.</span><span class="sxs-lookup"><span data-stu-id="374cf-129">However, a custom provider that saves values can be created.</span></span> <span data-ttu-id="374cf-130">Weitere Informationen finden Sie im Artikel zum [benutzerdefinierten Konfigurationsanbieter](xref:fundamentals/configuration/index#custom-config-providers).</span><span class="sxs-lookup"><span data-stu-id="374cf-130">See [custom configuration provider](xref:fundamentals/configuration/index#custom-config-providers).</span></span>

<span data-ttu-id="374cf-131">Im vorhergehenden Beispiel wird der Konfigurationsindexer zum Lesen von Werten verwendet.</span><span class="sxs-lookup"><span data-stu-id="374cf-131">The preceding sample uses the configuration indexer to read values.</span></span> <span data-ttu-id="374cf-132">Um außerhalb von `Startup` auf die Konfiguration zuzugreifen, verwenden Sie das *Optionsmuster*.</span><span class="sxs-lookup"><span data-stu-id="374cf-132">To access configuration outside of `Startup`, use the *options pattern*.</span></span> <span data-ttu-id="374cf-133">Weitere Informationen finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).</span><span class="sxs-lookup"><span data-stu-id="374cf-133">For more information, see the [Options](xref:fundamentals/configuration/options) topic.</span></span>

## <a name="xml-configuration"></a><span data-ttu-id="374cf-134">XML-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="374cf-134">XML configuration</span></span>

<span data-ttu-id="374cf-135">Stellen Sie einen `name`-Index für jedes Element bereit, um mit Arrays in Konfigurationsquellen im XML-Format zu arbeiten.</span><span class="sxs-lookup"><span data-stu-id="374cf-135">To work with arrays in XML-formatted configuration sources, provide a `name` index to each element.</span></span> <span data-ttu-id="374cf-136">Verwenden Sie den Index, um auf die Werte zuzugreifen:</span><span class="sxs-lookup"><span data-stu-id="374cf-136">Use the index to access the values:</span></span>

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

## <a name="configuration-by-environment"></a><span data-ttu-id="374cf-137">Konfiguration nach Umgebung</span><span class="sxs-lookup"><span data-stu-id="374cf-137">Configuration by environment</span></span>

<span data-ttu-id="374cf-138">Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen (z.B. für Entwicklung, Tests und Produktion) zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="374cf-138">It's typical to have different configuration settings for different environments, for example, development, testing, and production.</span></span> <span data-ttu-id="374cf-139">Durch die Erweiterungsmethode `CreateDefaultBuilder` in einer ASP.NET Core 2.x-App (oder durch die direkte Verwendung von `AddJsonFile` und `AddEnvironmentVariables` in einer ASP.NET Core 1.x-App) werden Konfigurationsanbieter zum Lesen von JSON-Dateien und Systemkonfigurationsquellen hinzugefügt:</span><span class="sxs-lookup"><span data-stu-id="374cf-139">The `CreateDefaultBuilder` extension method in an ASP.NET Core 2.x app (or using `AddJsonFile` and `AddEnvironmentVariables` directly in an ASP.NET Core 1.x app) adds configuration providers for reading JSON files and system configuration sources:</span></span>

* <span data-ttu-id="374cf-140">*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="374cf-140">*appsettings.json*</span></span>
* <span data-ttu-id="374cf-141">*appsettings.\<Umgebungsname>.json*</span><span class="sxs-lookup"><span data-stu-id="374cf-141">*appsettings.\<EnvironmentName>.json*</span></span>
* <span data-ttu-id="374cf-142">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="374cf-142">Environment variables</span></span>

<span data-ttu-id="374cf-143">ASP.NET Core 1.x-Apps müssen `AddJsonFile` und [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) aufrufen.</span><span class="sxs-lookup"><span data-stu-id="374cf-143">ASP.NET Core 1.x apps need to call `AddJsonFile` and [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_).</span></span>

<span data-ttu-id="374cf-144">Eine Erläuterung der Parameter finden Sie unter [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions).</span><span class="sxs-lookup"><span data-stu-id="374cf-144">See [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions) for an explanation of the parameters.</span></span> <span data-ttu-id="374cf-145">`reloadOnChange` wird nur in ASP.NET Core 1.1 und höher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="374cf-145">`reloadOnChange` is only supported in ASP.NET Core 1.1 and later.</span></span>

<span data-ttu-id="374cf-146">Konfigurationsquellen werden in der Reihenfolge, in der sie angegeben sind, gelesen.</span><span class="sxs-lookup"><span data-stu-id="374cf-146">Configuration sources are read in the order that they're specified.</span></span> <span data-ttu-id="374cf-147">Im vorangehenden Code werden die Umgebungsvariablen zuletzt gelesen.</span><span class="sxs-lookup"><span data-stu-id="374cf-147">In the preceding code, the environment variables are read last.</span></span> <span data-ttu-id="374cf-148">Alle über die Umgebung festgelegten Konfigurationswerte ersetzen diejenigen in den beiden vorherigen Anbietern.</span><span class="sxs-lookup"><span data-stu-id="374cf-148">Any configuration values set through the environment replace those set in the two previous providers.</span></span>

<span data-ttu-id="374cf-149">Beachten Sie die folgende *appsettings.Staging.json*-Datei:</span><span class="sxs-lookup"><span data-stu-id="374cf-149">Consider the following *appsettings.Staging.json* file:</span></span>

[!code-json[](index/sample/appsettings.Staging.json)]

<span data-ttu-id="374cf-150">Wenn die Umgebung auf `Staging` festgelegt ist, liest die folgende `Configure`-Methode den Wert von `MyConfig`:</span><span class="sxs-lookup"><span data-stu-id="374cf-150">When the environment is set to `Staging`, the following `Configure` method reads the value of `MyConfig`:</span></span>

[!code-csharp[](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


<span data-ttu-id="374cf-151">Die Umgebung ist in der Regel auf `Development`, `Staging` oder `Production` festgelegt.</span><span class="sxs-lookup"><span data-stu-id="374cf-151">The environment is typically set to `Development`, `Staging`, or `Production`.</span></span> <span data-ttu-id="374cf-152">Weitere Informationen finden Sie unter [Working with multiple environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="374cf-152">For more information, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="374cf-153">Überlegungen zur Konfiguration:</span><span class="sxs-lookup"><span data-stu-id="374cf-153">Configuration considerations:</span></span>

* <span data-ttu-id="374cf-154">`IOptionsSnapshot` kann Konfigurationsdaten erneut laden, wenn sich diese ändern.</span><span class="sxs-lookup"><span data-stu-id="374cf-154">`IOptionsSnapshot` can reload configuration data when it changes.</span></span> <span data-ttu-id="374cf-155">Weitere Informationen finden Sie unter [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).</span><span class="sxs-lookup"><span data-stu-id="374cf-155">For more information, see [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).,</span></span>
* <span data-ttu-id="374cf-156">Bei Konfigurationsschlüsseln wird die Groß-/Kleinschreibung **nicht** beachtet.</span><span class="sxs-lookup"><span data-stu-id="374cf-156">Configuration keys are **not** case-sensitive.</span></span>
* <span data-ttu-id="374cf-157">Speichern Sie **niemals** Kennwörter oder andere sensible Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien.</span><span class="sxs-lookup"><span data-stu-id="374cf-157">**Never** store passwords or other sensitive data in configuration provider code or in plain text configuration files.</span></span> <span data-ttu-id="374cf-158">Verwenden Sie keine Produktionsgeheimnisse in Entwicklungs- oder Testumgebungen.</span><span class="sxs-lookup"><span data-stu-id="374cf-158">Don't use production secrets in development or test environments.</span></span> <span data-ttu-id="374cf-159">Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in ein Quellcoderepository übernommen werden können.</span><span class="sxs-lookup"><span data-stu-id="374cf-159">Specify secrets outside of the project so that they can't be accidentally committed to a source code repository.</span></span> <span data-ttu-id="374cf-160">Erfahren Sie mehr zum [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) und einer [sicheren Speicherung von App-Geheimnissen während der Entwicklung](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="374cf-160">Learn more about [working with multiple environments](xref:fundamentals/environments) and managing [safe storage of app secrets during development](xref:security/app-secrets).</span></span>
* <span data-ttu-id="374cf-161">Wenn in Umgebungsvariablen in einem System kein Doppelpunkt (`:`) verwendet werden kann, ersetzen Sie den Doppelpunkt (`:`) durch zwei Unterstriche (`__`).</span><span class="sxs-lookup"><span data-stu-id="374cf-161">If a colon (`:`) can't be used in environment variables on a system, replace the colon (`:`) with a double-underscore (`__`).</span></span>

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a><span data-ttu-id="374cf-162">In-Memory-Anbieter und Bindung an eine POCO-Klasse</span><span class="sxs-lookup"><span data-stu-id="374cf-162">In-memory provider and binding to a POCO class</span></span>

<span data-ttu-id="374cf-163">Im folgenden Beispiel wird gezeigt, wie der In-Memory-Anbieter verwendet und an eine Klasse gebunden wird:</span><span class="sxs-lookup"><span data-stu-id="374cf-163">The following sample shows how to use the in-memory provider and bind to a class:</span></span>

[!code-csharp[](index/sample/InMemory/Program.cs)]

<span data-ttu-id="374cf-164">Konfigurationswerte werden als Zeichenfolgen zurückgegeben, durch eine Bindung wird jedoch die Erstellung von Objekten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="374cf-164">Configuration values are returned as strings, but binding enables the construction of objects.</span></span> <span data-ttu-id="374cf-165">Durch eine Bindung können POCO-Objekte oder sogar ganze Objektdiagramme abgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-165">Binding allows the retrieval of POCO objects or even entire object graphs.</span></span>

### <a name="getvalue"></a><span data-ttu-id="374cf-166">GetValue</span><span class="sxs-lookup"><span data-stu-id="374cf-166">GetValue</span></span>

<span data-ttu-id="374cf-167">Im folgenden Beispiel wird die Erweiterungsmethode [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) gezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-167">The following sample demonstrates the [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) extension method:</span></span>

[!code-csharp[](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

<span data-ttu-id="374cf-168">Durch die `GetValue<T>`-Methode von ConfigurationBinder kann ein Standardwert (hier 80) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-168">The ConfigurationBinder's `GetValue<T>` method allows the specification of a default value (80 in the sample).</span></span> <span data-ttu-id="374cf-169">`GetValue<T>` ist für einfache Szenarien vorgesehen und kann nicht an gesamte Abschnitte gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-169">`GetValue<T>` is for simple scenarios and doesn't bind to entire sections.</span></span> <span data-ttu-id="374cf-170">`GetValue<T>` ruft skalare Werte von `GetSection(key).Value` ab, die in einen bestimmten Typ konvertiert wurden.</span><span class="sxs-lookup"><span data-stu-id="374cf-170">`GetValue<T>` obtains scalar values from `GetSection(key).Value` converted to a specific type.</span></span>

## <a name="bind-to-an-object-graph"></a><span data-ttu-id="374cf-171">Binden an ein Objektdiagramm</span><span class="sxs-lookup"><span data-stu-id="374cf-171">Bind to an object graph</span></span>

<span data-ttu-id="374cf-172">Jedes Objekt in einer Klasse kann rekursiv gebunden werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-172">Each object in a class can be recursively bound.</span></span> <span data-ttu-id="374cf-173">Betrachten wir einmal die folgende `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="374cf-173">Consider the following `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/AppSettings.cs)]

<span data-ttu-id="374cf-174">Im folgenden Beispiel erfolgt eine Bindung an die `AppSettings`-Klasse:</span><span class="sxs-lookup"><span data-stu-id="374cf-174">The following sample binds to the `AppSettings` class:</span></span>

[!code-csharp[](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

<span data-ttu-id="374cf-175">Bei **ASP.NET Core 1.1** und höher kann `Get<T>` für gesamte Abschnitte verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-175">**ASP.NET Core 1.1** and higher can use `Get<T>`, which works with entire sections.</span></span> <span data-ttu-id="374cf-176">`Get<T>` ist eventuell praktischer als `Bind`.</span><span class="sxs-lookup"><span data-stu-id="374cf-176">`Get<T>` can be more convenient than using `Bind`.</span></span> <span data-ttu-id="374cf-177">Der folgende Code zeigt, wie `Get<T>` in Bezug auf das vorherige Beispiel verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="374cf-177">The following code shows how to use `Get<T>` with the preceding sample:</span></span>

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

<span data-ttu-id="374cf-178">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="374cf-178">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/ObjectGraph/appsettings.json)]

<span data-ttu-id="374cf-179">Das Programm zeigt `Height 11` an.</span><span class="sxs-lookup"><span data-stu-id="374cf-179">The program displays `Height 11`.</span></span>

<span data-ttu-id="374cf-180">Mit dem folgenden Code kann ein Komponententest für die Konfiguration durchgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="374cf-180">The following code can be used to unit test the configuration:</span></span>

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

## <a name="create-an-entity-framework-custom-provider"></a><span data-ttu-id="374cf-181">Erstellen eines benutzerdefinierten Entity Framework-Anbieters</span><span class="sxs-lookup"><span data-stu-id="374cf-181">Create an Entity Framework custom provider</span></span>

<span data-ttu-id="374cf-182">In diesem Abschnitt wird ein Standardkonfigurationsanbieter erstellt, der mithilfe des EF Name/Wert-Paare aus einer Datenbank liest.</span><span class="sxs-lookup"><span data-stu-id="374cf-182">In this section, a basic configuration provider that reads name-value pairs from a database using EF is created.</span></span>

<span data-ttu-id="374cf-183">Definieren Sie eine `ConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="374cf-183">Define a `ConfigurationValue` entity for storing configuration values in the database:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

<span data-ttu-id="374cf-184">Fügen Sie eine `ConfigurationContext`-Instanz hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen:</span><span class="sxs-lookup"><span data-stu-id="374cf-184">Add a `ConfigurationContext` to store and access the configured values:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

<span data-ttu-id="374cf-185">Erstellen Sie eine Klasse, die [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) implementiert:</span><span class="sxs-lookup"><span data-stu-id="374cf-185">Create a class that implements [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource):</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

<span data-ttu-id="374cf-186">Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span><span class="sxs-lookup"><span data-stu-id="374cf-186">Create the custom configuration provider by inheriting from [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider).</span></span> <span data-ttu-id="374cf-187">Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist:</span><span class="sxs-lookup"><span data-stu-id="374cf-187">The configuration provider initializes the database when it's empty:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

<span data-ttu-id="374cf-188">Bei Ausführung des Beispiels werden die hervorgehobenen Werte von der Datenbank („value_from_ef_1“ und „value_from_ef_2“) angezeigt.</span><span class="sxs-lookup"><span data-stu-id="374cf-188">The highlighted values from the database ("value_from_ef_1" and "value_from_ef_2") are displayed when the sample is run.</span></span>

<span data-ttu-id="374cf-189">Eine `EFConfigSource`-Erweiterungsmethode zum Hinzufügen der Konfigurationsquelle kann verwendet werden:</span><span class="sxs-lookup"><span data-stu-id="374cf-189">An `EFConfigSource` extension method for adding the configuration source can be used:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

<span data-ttu-id="374cf-190">Im folgenden Code wird die Verwendung des benutzerdefinierten Anbieters `EFConfigProvider` veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="374cf-190">The following code shows how to use the custom `EFConfigProvider`:</span></span>

[!code-csharp[](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

<span data-ttu-id="374cf-191">Beachten Sie, dass der benutzerdefinierte Anbieter `EFConfigProvider` im Beispiel nach dem JSON-Anbieter hinzugefügt wird, sodass Datenbankeinstellungen die Einstellungen aus der Datei *appsettings.json* überschreiben.</span><span class="sxs-lookup"><span data-stu-id="374cf-191">Note the sample adds the custom `EFConfigProvider` after the JSON provider, so any settings from the database will override settings from the *appsettings.json* file.</span></span>

<span data-ttu-id="374cf-192">Verwenden Sie die folgende Datei *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="374cf-192">Using the following *appsettings.json* file:</span></span>

[!code-json[](index/sample/CustomConfigurationProvider/appsettings.json)]

<span data-ttu-id="374cf-193">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-193">The following output is displayed:</span></span>

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a><span data-ttu-id="374cf-194">CommandLine-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="374cf-194">CommandLine configuration provider</span></span>

<span data-ttu-id="374cf-195">Der [CommandLine-Konfigurationsanbieter](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) erhält Schlüssel/Wert-Paare der Befehlszeilenargumente für die Konfiguration zur Runtime.</span><span class="sxs-lookup"><span data-stu-id="374cf-195">The [CommandLine configuration provider](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) receives command-line argument key-value pairs for configuration at runtime.</span></span>

[<span data-ttu-id="374cf-196">Beispiel für den CommandLine-Konfigurationsanbieter anzeigen oder herunterladen</span><span class="sxs-lookup"><span data-stu-id="374cf-196">View or download the CommandLine configuration sample</span></span>](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a><span data-ttu-id="374cf-197">Einrichten und Verwenden des CommandLine-Konfigurationsanbieters</span><span class="sxs-lookup"><span data-stu-id="374cf-197">Setup and use the CommandLine configuration provider</span></span>

# <a name="basic-configurationtabbasicconfiguration"></a>[<span data-ttu-id="374cf-198">Standardkonfiguration</span><span class="sxs-lookup"><span data-stu-id="374cf-198">Basic Configuration</span></span>](#tab/basicconfiguration)

<span data-ttu-id="374cf-199">Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine`-Erweiterungsmethode für eine [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)-Instanz ab:</span><span class="sxs-lookup"><span data-stu-id="374cf-199">To activate command-line configuration, call the `AddCommandLine` extension method on an instance of [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder):</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

<span data-ttu-id="374cf-200">Nach der Ausführung des Codes wird die folgende Ausgabe angezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-200">Running the code, the following output is displayed:</span></span>

```console
MachineName: MairaPC
Left: 1980
```

<span data-ttu-id="374cf-201">Durch die Übergabe von Schlüssel/Wert-Paaren der Argumente in der Befehlszeile werden die Werte von `Profile:MachineName` und `App:MainWindow:Left` geändert:</span><span class="sxs-lookup"><span data-stu-id="374cf-201">Passing argument key-value pairs on the command line changes the values of `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

<span data-ttu-id="374cf-202">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-202">The console window displays:</span></span>

```console
MachineName: BartPC
Left: 1979
```

<span data-ttu-id="374cf-203">Um die von anderen Konfigurationsanbietern bereitgestellte Konfiguration mit der Befehlszeilenkonfiguration zu überschreiben, rufen Sie die letzte Funktion `AddCommandLine` von `ConfigurationBuilder` auf:</span><span class="sxs-lookup"><span data-stu-id="374cf-203">To override configuration provided by other configuration providers with command-line configuration, call `AddCommandLine` last on `ConfigurationBuilder`:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="374cf-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="374cf-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="374cf-205">Typische ASP.NET Core 2.x-Apps verwenden für die Erstellung des Hosts die statische Komfortmethode `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="374cf-205">Typical ASP.NET Core 2.x apps use the static convenience method `CreateDefaultBuilder` to build the host:</span></span>

[!code-csharp[](index/sample_snapshot//Program.cs?highlight=12)]

<span data-ttu-id="374cf-206">`CreateDefaultBuilder` lädt die optionale Konfiguration von *appsettings.json*, *appsettings.{Umgebung}.json*, [Benutzergeheimnisse](xref:security/app-secrets) (in der `Development`-Umgebung), Umgebungsvariablen und Befehlszeilenargumente.</span><span class="sxs-lookup"><span data-stu-id="374cf-206">`CreateDefaultBuilder` loads optional configuration from *appsettings.json*, *appsettings.{Environment}.json*, [user secrets](xref:security/app-secrets) (in the `Development` environment), environment variables, and command-line arguments.</span></span> <span data-ttu-id="374cf-207">Der CommandLine-Konfigurationsanbieter wird zuletzt aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="374cf-207">The CommandLine configuration provider is called last.</span></span> <span data-ttu-id="374cf-208">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="374cf-208">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span>

<span data-ttu-id="374cf-209">Für *appsettings*-Dateien, bei denen:</span><span class="sxs-lookup"><span data-stu-id="374cf-209">For *appsettings* files where:</span></span>

* <span data-ttu-id="374cf-210">`reloadOnChange` aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="374cf-210">`reloadOnChange` is enabled.</span></span>
* <span data-ttu-id="374cf-211">die gleiche Einstellung in den Befehlszeilenargumenten sowie eine *appsettings*-Datei enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="374cf-211">Contain the same setting in the command-line arguments and an *appsettings* file.</span></span>
* <span data-ttu-id="374cf-212">die *appsettings*-Datei, die das entsprechende Befehlszeilenargument enthält, nach dem Start der App geändert wird.</span><span class="sxs-lookup"><span data-stu-id="374cf-212">The *appsettings* file containing the matching command-line argument is changed after the app starts.</span></span>

<span data-ttu-id="374cf-213">Wenn alle oben genannten Bedingungen erfüllt sind, werden die Befehlszeilenargumente überschrieben.</span><span class="sxs-lookup"><span data-stu-id="374cf-213">If all the preceding conditions are true, the command-line arguments are overridden.</span></span>

<span data-ttu-id="374cf-214">Die ASP.NET Core 2.x-App kann [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) statt `CreateDefaultBuilder` verwenden.</span><span class="sxs-lookup"><span data-stu-id="374cf-214">ASP.NET Core 2.x app can use [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) instead of `CreateDefaultBuilder`.</span></span> <span data-ttu-id="374cf-215">Wenn Sie `WebHostBuilder` verwenden, legen Sie die Konfiguration manuell mit [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) fest.</span><span class="sxs-lookup"><span data-stu-id="374cf-215">When using `WebHostBuilder`, manually set configuration with [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder).</span></span> <span data-ttu-id="374cf-216">Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="374cf-216">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="374cf-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="374cf-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="374cf-218">Erstellen Sie einen [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)-Anbieter, und rufen Sie die `AddCommandLine`-Methode auf, um den CommandLine-Konfigurationsanbieter zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="374cf-218">Create a [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) and call the `AddCommandLine` method to use the CommandLine configuration provider.</span></span> <span data-ttu-id="374cf-219">Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.</span><span class="sxs-lookup"><span data-stu-id="374cf-219">Calling the provider last allows the command-line arguments passed at runtime to override configuration set by the other configuration providers called earlier.</span></span> <span data-ttu-id="374cf-220">Wenden Sie mit der `UseConfiguration`-Methode die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) an:</span><span class="sxs-lookup"><span data-stu-id="374cf-220">Apply the configuration to [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) with the `UseConfiguration` method:</span></span>

[!code-csharp[](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a><span data-ttu-id="374cf-221">Argumente</span><span class="sxs-lookup"><span data-stu-id="374cf-221">Arguments</span></span>

<span data-ttu-id="374cf-222">Über die Befehlszeile übergebene Argumente müssen eines der zwei Formate, die in der folgenden Tabelle aufgeführt sind, aufweisen:</span><span class="sxs-lookup"><span data-stu-id="374cf-222">Arguments passed on the command line must conform to one of two formats shown in the following table:</span></span>

| <span data-ttu-id="374cf-223">Argumentformat</span><span class="sxs-lookup"><span data-stu-id="374cf-223">Argument format</span></span>                                                     | <span data-ttu-id="374cf-224">Beispiel</span><span class="sxs-lookup"><span data-stu-id="374cf-224">Example</span></span>        |
| ------------------------------------------------------------------- | :------------: |
| <span data-ttu-id="374cf-225">Einzelnes Argument: ein durch ein Gleichheitszeichen (`=`) getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="374cf-225">Single argument: a key-value pair separated by an equals sign (`=`)</span></span> | `key1=value`   |
| <span data-ttu-id="374cf-226">Sequenz der zwei Argumente: ein durch einen Leerraum getrenntes Schlüssel/Wert-Paar</span><span class="sxs-lookup"><span data-stu-id="374cf-226">Sequence of two arguments: a key-value pair separated by a space</span></span>    | `/key1 value1` |

<span data-ttu-id="374cf-227">**Einzelnes Argument**</span><span class="sxs-lookup"><span data-stu-id="374cf-227">**Single argument**</span></span>

<span data-ttu-id="374cf-228">Der Wert muss einem Gleichheitszeichen (`=`) nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="374cf-228">The value must follow an equals sign (`=`).</span></span> <span data-ttu-id="374cf-229">Der Wert kann NULL sein (z.B. `mykey=`).</span><span class="sxs-lookup"><span data-stu-id="374cf-229">The value can be null (for example, `mykey=`).</span></span>

<span data-ttu-id="374cf-230">Der Schlüssel darf einen Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="374cf-230">The key may have a prefix.</span></span>

| <span data-ttu-id="374cf-231">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="374cf-231">Key prefix</span></span>               | <span data-ttu-id="374cf-232">Beispiel</span><span class="sxs-lookup"><span data-stu-id="374cf-232">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="374cf-233">Ohne Präfix</span><span class="sxs-lookup"><span data-stu-id="374cf-233">No prefix</span></span>                | `key1=value1`   |
| <span data-ttu-id="374cf-234">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="374cf-234">Single dash (`-`)&#8224;</span></span> | `-key2=value2`  |
| <span data-ttu-id="374cf-235">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="374cf-235">Two dashes (`--`)</span></span>        | `--key3=value3` |
| <span data-ttu-id="374cf-236">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="374cf-236">Forward slash (`/`)</span></span>      | `/key4=value4`  |

<span data-ttu-id="374cf-237">&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-237">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="374cf-238">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="374cf-238">Example command:</span></span>

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

<span data-ttu-id="374cf-239">Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="374cf-239">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

<span data-ttu-id="374cf-240">**Sequenz aus zwei Argumenten**</span><span class="sxs-lookup"><span data-stu-id="374cf-240">**Sequence of two arguments**</span></span>

<span data-ttu-id="374cf-241">Der Wert darf nicht null und muss dem Schlüssel getrennt durch einen Leerraum nachgestellt sein.</span><span class="sxs-lookup"><span data-stu-id="374cf-241">The value can't be null and must follow the key separated by a space.</span></span>

<span data-ttu-id="374cf-242">Der Schlüssel muss ein Präfix enthalten.</span><span class="sxs-lookup"><span data-stu-id="374cf-242">The key must have a prefix.</span></span>

| <span data-ttu-id="374cf-243">Schlüsselpräfix</span><span class="sxs-lookup"><span data-stu-id="374cf-243">Key prefix</span></span>               | <span data-ttu-id="374cf-244">Beispiel</span><span class="sxs-lookup"><span data-stu-id="374cf-244">Example</span></span>         |
| ------------------------ | :-------------: |
| <span data-ttu-id="374cf-245">Ein Gedankenstrich (`-`)&#8224;</span><span class="sxs-lookup"><span data-stu-id="374cf-245">Single dash (`-`)&#8224;</span></span> | `-key1 value1`  |
| <span data-ttu-id="374cf-246">Zwei Gedankenstriche (`--`)</span><span class="sxs-lookup"><span data-stu-id="374cf-246">Two dashes (`--`)</span></span>        | `--key2 value2` |
| <span data-ttu-id="374cf-247">Schrägstrich (`/`)</span><span class="sxs-lookup"><span data-stu-id="374cf-247">Forward slash (`/`)</span></span>      | `/key3 value3`  |

<span data-ttu-id="374cf-248">&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-248">&#8224;A key with a single dash prefix (`-`) must be provided in [switch mappings](#switch-mappings), described below.</span></span>

<span data-ttu-id="374cf-249">Beispielbefehl:</span><span class="sxs-lookup"><span data-stu-id="374cf-249">Example command:</span></span>

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

<span data-ttu-id="374cf-250">Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="374cf-250">Note: If `-key1` isn't present in the [switch mappings](#switch-mappings) given to the configuration provider, a `FormatException` is thrown.</span></span>

### <a name="duplicate-keys"></a><span data-ttu-id="374cf-251">Doppelte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="374cf-251">Duplicate keys</span></span>

<span data-ttu-id="374cf-252">Wenn Schlüssel doppelt angegeben werden, wird das letzte Schlüssel/Wert-Paar verwendet.</span><span class="sxs-lookup"><span data-stu-id="374cf-252">If duplicate keys are provided, the last key-value pair is used.</span></span>

### <a name="switch-mappings"></a><span data-ttu-id="374cf-253">Switchmappings</span><span class="sxs-lookup"><span data-stu-id="374cf-253">Switch mappings</span></span>

<span data-ttu-id="374cf-254">Bei der manuellen Erstellung von Konfigurationen mit `ConfigurationBuilder` kann ein Switchmappingswörterbuch der `AddCommandLine`-Methode hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-254">When manually building configuration with `ConfigurationBuilder`, a switch mappings dictionary can be added to the `AddCommandLine` method.</span></span> <span data-ttu-id="374cf-255">Switchmappings erlauben das Angeben einer Logik zum Ersetzen von Schlüsselnamen.</span><span class="sxs-lookup"><span data-stu-id="374cf-255">Switch mappings allow key name replacement logic.</span></span>

<span data-ttu-id="374cf-256">Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft.</span><span class="sxs-lookup"><span data-stu-id="374cf-256">When the switch mappings dictionary is used, the dictionary is checked for a key that matches the key provided by a command-line argument.</span></span> <span data-ttu-id="374cf-257">Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zur Festlegung der Konfiguration zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="374cf-257">If the command-line key is found in the dictionary, the dictionary value (the key replacement) is passed back to set the configuration.</span></span> <span data-ttu-id="374cf-258">Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.</span><span class="sxs-lookup"><span data-stu-id="374cf-258">A switch mapping is required for any command-line key prefixed with a single dash (`-`).</span></span>

<span data-ttu-id="374cf-259">Regeln für Schlüssel von Switchmappingwörterbüchern:</span><span class="sxs-lookup"><span data-stu-id="374cf-259">Switch mappings dictionary key rules:</span></span>

* <span data-ttu-id="374cf-260">Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.</span><span class="sxs-lookup"><span data-stu-id="374cf-260">Switches must start with a dash (`-`) or double-dash (`--`).</span></span>
* <span data-ttu-id="374cf-261">Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="374cf-261">The switch mappings dictionary must not contain duplicate keys.</span></span>

<span data-ttu-id="374cf-262">Im folgenden Beispiel können durch die `GetSwitchMappings`-Methode einzelne Gedankenstriche (`-`) als Schlüsselpräfix in Befehlszeilenargumenten verwendet und führende Unterschlüsselpräfixe vermieden werden.</span><span class="sxs-lookup"><span data-stu-id="374cf-262">In the following example, the `GetSwitchMappings` method allows command-line arguments to use a single dash (`-`) key prefix and avoid leading subkey prefixes.</span></span>

[!code-csharp[](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

<span data-ttu-id="374cf-263">Ohne die Angabe von Befehlszeilenargumenten legt das auf `AddInMemoryCollection` festgelegte Wörterbuch die Konfigurationswerte fest.</span><span class="sxs-lookup"><span data-stu-id="374cf-263">Without providing command-line arguments, the dictionary provided to `AddInMemoryCollection` sets the configuration values.</span></span> <span data-ttu-id="374cf-264">Führen Sie die App mit dem folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="374cf-264">Run the app with the following command:</span></span>

```console
dotnet run
```

<span data-ttu-id="374cf-265">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-265">The console window displays:</span></span>

```console
MachineName: RickPC
Left: 1980
```

<span data-ttu-id="374cf-266">Verwenden Sie den folgenden Code, um die Konfigurationseinstellungen zu übergeben:</span><span class="sxs-lookup"><span data-stu-id="374cf-266">Use the following to pass in configuration settings:</span></span>

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

<span data-ttu-id="374cf-267">Das Konsolenfenster wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="374cf-267">The console window displays:</span></span>

```console
MachineName: DahliaPC
Left: 1984
```

<span data-ttu-id="374cf-268">Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten:</span><span class="sxs-lookup"><span data-stu-id="374cf-268">After the switch mappings dictionary is created, it contains the data shown in the following table:</span></span>

| <span data-ttu-id="374cf-269">Key</span><span class="sxs-lookup"><span data-stu-id="374cf-269">Key</span></span>            | <span data-ttu-id="374cf-270">Wert</span><span class="sxs-lookup"><span data-stu-id="374cf-270">Value</span></span>                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

<span data-ttu-id="374cf-271">Führen Sie zur Veranschaulichung des Schlüsselwechsels mit dem Wörterbuch den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="374cf-271">To demonstrate key switching using the dictionary, run the following command:</span></span>

```console
dotnet run -MachineName=ChadPC -Left=1988
```

<span data-ttu-id="374cf-272">Die Befehlszeilenschlüssel sind ausgetauscht.</span><span class="sxs-lookup"><span data-stu-id="374cf-272">The command-line keys are swapped.</span></span> <span data-ttu-id="374cf-273">Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left` an:</span><span class="sxs-lookup"><span data-stu-id="374cf-273">The console window displays the configuration values for `Profile:MachineName` and `App:MainWindow:Left`:</span></span>

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a><span data-ttu-id="374cf-274">Datei „web.config“</span><span class="sxs-lookup"><span data-stu-id="374cf-274">web.config file</span></span>

<span data-ttu-id="374cf-275">Eine Datei namens *Web.config* ist erforderlich, wenn Sie die App in IIS oder IIS Express hosten.</span><span class="sxs-lookup"><span data-stu-id="374cf-275">A *web.config* file is required when hosting the app in IIS or IIS Express.</span></span> <span data-ttu-id="374cf-276">Die Einstellungen in der Datei *Web.config* aktivieren das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), um die App zu starten und andere IIS-Einstellungen und -Module zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="374cf-276">Settings in *web.config* enable the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to launch the app and configure other IIS settings and modules.</span></span> <span data-ttu-id="374cf-277">Wenn die Datei *Web.config* nicht vorhanden ist und die Projektdatei `<Project Sdk="Microsoft.NET.Sdk.Web">` enthält, wird bei der Veröffentlichung des Projekts eine Datei namens *Web.config* in der veröffentlichten Ausgabe (dem Ordner *publish*) erstellt.</span><span class="sxs-lookup"><span data-stu-id="374cf-277">If the *web.config* file isn't present and the project file includes `<Project Sdk="Microsoft.NET.Sdk.Web">`, publishing the project creates a *web.config* file in the published output (the *publish* folder).</span></span> <span data-ttu-id="374cf-278">Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index#webconfig-file).</span><span class="sxs-lookup"><span data-stu-id="374cf-278">For more information, see [Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#webconfig-file).</span></span>

## <a name="accessing-configuration-during-startup"></a><span data-ttu-id="374cf-279">Zugriff auf die Konfiguration während des Starts</span><span class="sxs-lookup"><span data-stu-id="374cf-279">Accessing configuration during startup</span></span>

<span data-ttu-id="374cf-280">Beispiele für den Zugriff auf die Konfigurationen in `ConfigureServices` oder `Configure` während des Starts finden Sie im Artikel zum [Start der Anwendung](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="374cf-280">To access configuration within `ConfigureServices` or `Configure` during startup, see the examples in the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="374cf-281">Zusätzliche Hinweise</span><span class="sxs-lookup"><span data-stu-id="374cf-281">Additional notes</span></span>

* <span data-ttu-id="374cf-282">Die Abhängigkeitsinjektion (Dependency Injection, DI) wird erst nach Aufrufen von `ConfigureServices` eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="374cf-282">Dependency Injection (DI) isn't set up until after `ConfigureServices` is invoked.</span></span>
* <span data-ttu-id="374cf-283">Das Konfigurationssystem ist nicht DI-fähig.</span><span class="sxs-lookup"><span data-stu-id="374cf-283">The configuration system isn't DI aware.</span></span>
* <span data-ttu-id="374cf-284">`IConfiguration` weist zwei Spezialisierungen auf:</span><span class="sxs-lookup"><span data-stu-id="374cf-284">`IConfiguration` has two specializations:</span></span>
  * <span data-ttu-id="374cf-285">`IConfigurationRoot` Wird für den Stammknoten verwendet.</span><span class="sxs-lookup"><span data-stu-id="374cf-285">`IConfigurationRoot` Used for the root node.</span></span> <span data-ttu-id="374cf-286">Kann einen erneuten Ladevorgang auslösen.</span><span class="sxs-lookup"><span data-stu-id="374cf-286">Can trigger a reload.</span></span>
  * <span data-ttu-id="374cf-287">`IConfigurationSection` Stellt einen Abschnitt der Konfigurationswerte dar.</span><span class="sxs-lookup"><span data-stu-id="374cf-287">`IConfigurationSection` Represents a section of configuration values.</span></span> <span data-ttu-id="374cf-288">Die Methoden `GetSection` und `GetChildren` geben `IConfigurationSection` zurück.</span><span class="sxs-lookup"><span data-stu-id="374cf-288">The `GetSection` and `GetChildren` methods return an `IConfigurationSection`.</span></span>
  * <span data-ttu-id="374cf-289">Verwenden Sie [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot), wenn Sie Konfigurationen neu laden oder auf jeden Anbieter zugreifen möchten.</span><span class="sxs-lookup"><span data-stu-id="374cf-289">Use [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot) when reloading configuration or for access to each provider.</span></span> <span data-ttu-id="374cf-290">Keine dieser Situationen tritt häufig auf.</span><span class="sxs-lookup"><span data-stu-id="374cf-290">Neither of these situations are common.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="374cf-291">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="374cf-291">Additional resources</span></span>

* [<span data-ttu-id="374cf-292">Optionen</span><span class="sxs-lookup"><span data-stu-id="374cf-292">Options</span></span>](xref:fundamentals/configuration/options)
* [<span data-ttu-id="374cf-293">Arbeiten mit mehreren Umgebungen</span><span class="sxs-lookup"><span data-stu-id="374cf-293">Working with Multiple Environments</span></span>](xref:fundamentals/environments)
* [<span data-ttu-id="374cf-294">Sicheres Speichern geheimer App-Schlüssel während der Entwicklung</span><span class="sxs-lookup"><span data-stu-id="374cf-294">Safe storage of app secrets during development</span></span>](xref:security/app-secrets)
* [<span data-ttu-id="374cf-295">Einrichten eines Hosts</span><span class="sxs-lookup"><span data-stu-id="374cf-295">Hosting in ASP.NET Core</span></span>](xref:fundamentals/hosting)
* [<span data-ttu-id="374cf-296">Abhängigkeitsinjektion</span><span class="sxs-lookup"><span data-stu-id="374cf-296">Dependency Injection</span></span>](xref:fundamentals/dependency-injection)
* [<span data-ttu-id="374cf-297">Azure Key Vault-Konfigurationsanbieter</span><span class="sxs-lookup"><span data-stu-id="374cf-297">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
