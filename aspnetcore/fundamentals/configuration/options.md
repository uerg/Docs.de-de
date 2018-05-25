---
title: Optionsmuster in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Optionsmuster verwenden, um Gruppen von zusammengehörigen Einstellungen in ASP.NET Core-Anwendungen darzustellen.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/28/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/options
ms.openlocfilehash: 800ff2039e7cc1fa37315ed55a77711dc9f47504
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="a2ec5-103">Optionsmuster in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2ec5-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="a2ec5-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="a2ec5-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a2ec5-105">Das Optionsmuster verwendet Klassen, um Gruppen von zusammengehörigen Einstellungen darzustellen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-105">The options pattern uses classes to represent groups of related settings.</span></span> <span data-ttu-id="a2ec5-106">Wenn Konfigurationseinstellungen nach Feature in separate Klassen isoliert werden, entspricht die Anwendung zwei wichtigen Prinzipien der Softwareentwicklung:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-106">When configuration settings are isolated by feature into separate classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="a2ec5-107">Das [Schnittstellentrennungsprinzip (ISP)](http://deviq.com/interface-segregation-principle/): Features (Klassen), die von Konfigurationseinstellungen abhängen, sind nur von den Konfigurationseinstellungen abhängig, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="a2ec5-108">[Trennung von Bereichen](http://deviq.com/separation-of-concerns/): Einstellungen für die verschiedenen Teile der Anwendung hängen nicht voneinander ab und sind nicht gekoppelt.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="a2ec5-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)) Dieser Artikel kann mit der Beispielanwendung einfacher gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="a2ec5-110">Grundlegende Optionskonfiguration</span><span class="sxs-lookup"><span data-stu-id="a2ec5-110">Basic options configuration</span></span>

<span data-ttu-id="a2ec5-111">Grundlegende Optionskonfiguration wird als Beispiel &num;1 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-112">Eine Optionsklasse muss nicht abstrakt sein und über einen öffentlichen parameterlosen Konstruktor verfügen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="a2ec5-113">Die folgende Klasse `MyOptions` verfügt über die zwei Eigenschaften: `Option1` und `Option2`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="a2ec5-114">Das Festlegen von Standardwerten ist optional, aber der Klassenkonstruktor im folgenden Beispiel legt den Standardwert von `Option1` fest.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="a2ec5-115">`Option2` hat den Standardwert festgelegt, indem die Eigenschaft direkt initialisiert wurde (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2ec5-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="a2ec5-116">Die `MyOptions`-Klasse wird zum Dienstcontainer mit [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) hinzugefügt und an die Konfiguration gebunden:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-116">The `MyOptions` class is added to the service container with [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) and bound to configuration:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="a2ec5-117">Das folgende Seitenmodel verwendet [konstruktorbasierte Dependency Injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) mit [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1), um auf die Einstellungen zugreifen zu können (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2ec5-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="a2ec5-118">Die *appsettings.json*-Datei des Beispiels gibt Werte für `option1` und `option2` an:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[](options/sample/appsettings.json)]

<span data-ttu-id="a2ec5-119">Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode des Seitenmodells eine Zeichenfolge zurück, die die Werte der Optionsklasse anzeigt:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="a2ec5-120">Konfigurieren von einfachen Optionen mit einem Delegaten</span><span class="sxs-lookup"><span data-stu-id="a2ec5-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="a2ec5-121">Das Konfigurieren von einfachen Optionen mit einem Delegaten wird als Beispiel &num;2 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-122">Verwenden Sie einen Delegaten zum Festlegen von Optionswerten.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-122">Use a delegate to set options values.</span></span> <span data-ttu-id="a2ec5-123">Die Beispielanwendung verwendet die `MyOptionsWithDelegateConfig`-Klasse (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2ec5-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="a2ec5-124">Im folgenden Code wird ein zweiter `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="a2ec5-125">Er verwendet einen Delegaten zum Konfigurieren der Bindung mit `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="a2ec5-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="a2ec5-127">Sie können mehrere Konfigurationsanbieter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="a2ec5-128">Konfigurationsanbieter stehen in NuGet-Paketen zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="a2ec5-129">Sie werden in der Reihenfolge ihrer Registrierung angewendet.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="a2ec5-130">Jeder Aufruf von [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) fügt einen `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzu.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="a2ec5-131">Im vorherigen Beispiel wurden die Werte von `Option1` und `Option2` beide in *appsettings.json* angegeben, aber die Werte von `Option1` und `Option2` werden vom konfigurierten Delegaten überschrieben.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="a2ec5-132">Wenn mehr als ein Konfigurationsdienst aktiviert ist, *gewinnt* die letzte angegebene Konfigurationsquelle und legt den Konfigurationswert fest.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="a2ec5-133">Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode des Seitenmodells eine Zeichenfolge zurück, die die Werte der Optionsklasse anzeigt:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="a2ec5-134">Konfigurieren von Unteroptionen</span><span class="sxs-lookup"><span data-stu-id="a2ec5-134">Suboptions configuration</span></span>

<span data-ttu-id="a2ec5-135">Das Konfigurieren von Unteroptionen wird als Beispiel &num;3 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-136">Anwendungen sollten Optionsklassen erstellen, die für bestimmte Featuregruppen (Klassen) in der Anwendung gelten.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="a2ec5-137">Die Komponenten der Anwendung, die Konfigurationswerte erfordern, sollten nur über Zugriff auf die Konfigurationswerte verfügen, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="a2ec5-138">Beim Binden von Optionen zur Konfiguration ist jede Eigenschaft im Optionstyp an einen Konfigurationsschlüssel des `property[:sub-property:]`-Formulars gebunden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="a2ec5-139">Die `MyOptions.Option1`-Eigenschaft ist beispielsweise an den Schlüssel `Option1` gebunden, der von der `option1`-Eigenschaft in *appsettings.json* gelesen wird.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="a2ec5-140">Im folgenden Code wird ein dritter `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="a2ec5-141">Er verbindet `MySubOptions` mit dem `subsection`-Abschnitt der *appSettings.json*-Datei:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="a2ec5-142">Die `GetSection`-Erweiterungsmethode erfordert das [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/)-NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="a2ec5-143">Wenn die Anwendung das [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)-Metapaket verwendet, ist das Paket automatisch eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="a2ec5-144">Die *appsettings.json*-Datei des Beispiels definiert ein `subsection`-Element mit Schlüsseln für `suboption1` und `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="a2ec5-145">Die `MySubOptions`-Klasse definiert die Eigenschaften `SubOption1` und `SubOption2` zur Aufnahme der untergeordneten Optionswerte (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2ec5-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="a2ec5-146">Die `OnGet`-Methode des Seitenmodells gibt eine Zeichenfolge mit den untergeordneten Optionswerten (*Pages/Index.cshtml.cs*) zurück:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="a2ec5-147">Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode eine Zeichenfolge zurück, die die Werte der untergeordneten Optionsklasse anzeigt:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="a2ec5-148">Optionen, die durch ein Ansichtsmodell oder mit direkter View Injection bereitgestellt werden</span><span class="sxs-lookup"><span data-stu-id="a2ec5-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="a2ec5-149">Optionen, die durch ein Ansichtsmodell oder mit direkter View Injection bereitgestellt werden, werden im Beispiel &num;4 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-150">Optionen können in einem Ansichtsmodell oder durch direkte Injection von `IOptions<TOptions>` in eine Ansicht (*Pages/Index.cshtml.cs*) angegeben werden:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="a2ec5-151">Fügen Sie für die direkte Injection `IOptions<MyOptions>` mit einer `@inject`-Anweisung ein:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="a2ec5-152">Wenn die Anwendung ausgeführt wird, werden die Optionswerte in der gerenderten Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Optionswerte Option1: value1_from_json und Option2: -1 werden aus dem Modell geladen und in die Ansicht eingefügt.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="a2ec5-154">Neuladen der Konfigurationsdaten mit IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="a2ec5-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="a2ec5-155">Das Neuladen der Konfigurationsdaten mit `IOptionsSnapshot` wird im Beispiel &num;5 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-156">*Erfordert ASP.NET Core 1.1 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="a2ec5-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="a2ec5-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) unterstützt das Neuladen von Optionen mit minimalem Verarbeitungsaufwand.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="a2ec5-158">In ASP.NET Core 1.1 ist `IOptionsSnapshot` eine Momentaufnahme von [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) und wird automatisch aktualisiert, wenn der Monitor auf der Grundlage sich ändernder Datenquellen Änderungen auslöst.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="a2ec5-159">In ASP.NET Core 2.0 und höher werden Optionen einmal pro Anforderung berechnet, wenn auf sie zugegriffen wird und sie für die Lebensdauer der Anforderung zwischengespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="a2ec5-160">Im folgenden Beispiel wird veranschaulicht, wie eine neue `IOptionsSnapshot` nach den *appsettings.json*-Veränderungen erstellt wird (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="a2ec5-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="a2ec5-161">Mehrere Anforderungen an den Server geben konstante Werte zurück, die von der *appsettings.json*-Datei bereitgestellt werden, bis die Datei geändert wird und die Konfiguration neu lädt.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="a2ec5-162">Die folgende Abbildung zeigt die anfänglichen `option1`- und `option2`-Werte, die aus der *appsettings.json*-Datei geladen werden:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="a2ec5-163">Ändern Sie die Werte in der *appsettings.json*-Datei auf `value1_from_json UPDATED` und `200`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="a2ec5-164">Speichern Sie die *appsettings.json*-Datei.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="a2ec5-165">Aktualisieren Sie den Browser, um festzustellen, ob die Optionswerte aktualisiert wurden:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="a2ec5-166">Unterstützung für benannte Optionen mit IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="a2ec5-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="a2ec5-167">Unterstützung für benannte Optionen mit [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) wird als Beispiel &num;6 in der [Beispielanwendung](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="a2ec5-168">*Erfordert ASP.NET Core 2.0 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="a2ec5-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="a2ec5-169">Die Unterstützung für *benannte Optionen* ermöglicht es der Anwendung, zwischen den Konfigurationen benannter Optionen zu unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="a2ec5-170">In der Beispiel-App werden benannte Optionen mit [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) deklariert. Dadurch wird wiederum die Erweiterungsmethode [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-170">In the sample app, named options are declared with the [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) which in turn calls the extension method [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="a2ec5-171">Die Beispielanwendung greift mit [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) auf die benannten Optionen zu (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="a2ec5-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="a2ec5-172">Bei der Ausführung der Beispielanwendung werden die benannten Optionen zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="a2ec5-173">`named_options_1`-Werte werden von der Konfiguration bereitgestellt. Sie werden aus der *appsettings.json*-Datei geladen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="a2ec5-174">`named_options_2`-Werte werden bereitgestellt vom:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="a2ec5-175">`named_options_2`-Delegat in `ConfigureServices` für `Option1`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="a2ec5-176">Der Standardwert für `Option2` wird von der `MyOptions`-Klasse bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="a2ec5-177">Konfigurieren Sie alle benannten Optionsinstanzen mit der [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)-Methode.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="a2ec5-178">Der folgende Code konfiguriert `Option1` für alle benannten Konfigurationsinstanzen mit einem gemeinsamen Wert.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="a2ec5-179">Fügen Sie der `Configure`-Methode manuell den folgenden Code hinzu:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="a2ec5-180">Das Ausführen der Beispielanwendung nach dem Hinzufügen des Codes führt zu folgendem Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="a2ec5-181">In ASP.NET Core 2.0 und höher sind alle Optionen benannte Instanzen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="a2ec5-182">Vorhandene `IConfigureOption`-Instanzen werden behandelt, als ob sie auf die `Options.DefaultName`-Instanz abzielen. Diese ist `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="a2ec5-183">`IConfigureNamedOptions` implementiert auch `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="a2ec5-184">Die Standardimplementierung von [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([Verweisquelle](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) verfügt über eine Logik zur ordnungsgemäßen Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs) has logic to use each appropriately.</span></span> <span data-ttu-id="a2ec5-185">Die benannte Option `null` wird verwendet, um auf alle benannten Instanzen anstelle einer bestimmten benannten Instanz abzuzielen ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) und [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) verwenden diese Konvention).</span><span class="sxs-lookup"><span data-stu-id="a2ec5-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="a2ec5-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="a2ec5-186">IPostConfigureOptions</span></span>

<span data-ttu-id="a2ec5-187">*Erfordert ASP.NET Core 2.0 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="a2ec5-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="a2ec5-188">Legen Sie die Postkonfiguration mit [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) fest.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="a2ec5-189">Die Postkonfiguration wird nach der [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)-Konfiguration durchgeführt:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a2ec5-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) ist nach dem Konfigurieren der benannten Optionen verfügbar:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="a2ec5-191">Verwenden Sie [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) zur Postkonfiguration aller benannten Konfigurationsinstanzen:</span><span class="sxs-lookup"><span data-stu-id="a2ec5-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="a2ec5-192">Optionsfactory, Überwachung und Cache</span><span class="sxs-lookup"><span data-stu-id="a2ec5-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="a2ec5-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) wird für Benachrichtigungen verwendet, wenn sich die `TOptions`-Instanzen verändern.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="a2ec5-194">`IOptionsMonitor` unterstützt neu ladbare Optionen, Änderungsbenachrichtigungen und `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="a2ec5-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 oder höher) ist für das Erstellen neuer Optionsinstanzen verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="a2ec5-196">Es verfügt über eine einzelne [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)-Methode.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="a2ec5-197">Die Standardimplementierung akzeptiert alle registrierten `IConfigureOptions` und `IPostConfigureOptions` und führt alle Konfigurationen zuerst und die Postkonfigurationen danach aus.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="a2ec5-198">Es wird zwischen `IConfigureNamedOptions` und `IConfigureOptions` unterschieden, und es werden nur die entsprechenden Schnittstellen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="a2ec5-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 oder höher) wird von `IOptionsMonitor` zum Zwischenspeichern von `TOptions`-Instanzen verwendet.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="a2ec5-200">`IOptionsMonitorCache` erklärt Optionsinstanzen im Monitor ungültig, sodass der Wert neu berechnet wird ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="a2ec5-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="a2ec5-201">Werte können manuell oder mit [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd) eingeführt werden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="a2ec5-202">Die [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)-Methode wird verwendet, wenn alle benannten Instanzen bei Bedarf neu erstellt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="a2ec5-203">Zugreifen auf Optionen während des Starts</span><span class="sxs-lookup"><span data-stu-id="a2ec5-203">Accessing options during startup</span></span>

<span data-ttu-id="a2ec5-204">`IOptions` kann in `Configure` verwendet werden, da Dienste erstellt werden, bevor die `Configure`-Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="a2ec5-205">Wenn ein Dienstanbieter für den Zugriff auf Optionen in `ConfigureServices` integriert ist, würde er keine Optionskonfigurationen enthalten, die nach der Erstellung des Dienstanbieters bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="a2ec5-206">Aus diesem Grund kann es einen inkonsistenten Optionszustand geben. Dies liegt an der Reihenfolge der Dienstregistrierungen.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="a2ec5-207">Da Optionen in der Regel aus der Konfiguration geladen werden, kann die Konfiguration beim Start in `Configure` und `ConfigureServices` verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="a2ec5-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="a2ec5-208">Beispiele für die Verwendung der Konfiguration während des Starts finden Sie im Artikel [Starten der Anwendung](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="a2ec5-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="a2ec5-209">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="a2ec5-209">See also</span></span>

* [<span data-ttu-id="a2ec5-210">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="a2ec5-210">Configuration</span></span>](xref:fundamentals/configuration/index)
