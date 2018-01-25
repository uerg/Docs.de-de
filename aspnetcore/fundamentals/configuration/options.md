---
title: Optionen-Musters in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie das Optionen-Muster verwendet wird, um Gruppen von Verwandte Einstellungen in ASP.NET Core apps darzustellen.
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 11/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration/options
ms.openlocfilehash: aab96b5313a8632950e51f5586612c1d0d3d176e
ms.sourcegitcommit: 83b5a4715fd25e4eb6f7c8427c0ef03850a7fa07
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/25/2018
---
# <a name="options-pattern-in-aspnet-core"></a><span data-ttu-id="fca84-103">Optionen-Musters in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fca84-103">Options pattern in ASP.NET Core</span></span>

<span data-ttu-id="fca84-104">Von [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fca84-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fca84-105">Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar.</span><span class="sxs-lookup"><span data-stu-id="fca84-105">The options pattern uses options classes to represent groups of related settings.</span></span> <span data-ttu-id="fca84-106">Wenn Konfigurationseinstellungen von der Funktion in getrennte Optionsklassen isoliert werden, unterliegen die app zwei wichtige Softwareentwicklung Prinzipien:</span><span class="sxs-lookup"><span data-stu-id="fca84-106">When configuration settings are isolated by feature into separate options classes, the app adheres to two important software engineering principles:</span></span>

* <span data-ttu-id="fca84-107">Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/): Funktionen (Klassen), die auf Konfigurationseinstellungen beruhen abhängig sind, nur auf den Konfigurationseinstellungen, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="fca84-107">The [Interface Segregation Principle (ISP)](http://deviq.com/interface-segregation-principle/): Features (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
* <span data-ttu-id="fca84-108">[Trennung von Anliegen](http://deviq.com/separation-of-concerns/): Einstellungen für die verschiedenen Teile der app werden nicht abhängige oder gekoppelten miteinander.</span><span class="sxs-lookup"><span data-stu-id="fca84-108">[Separation of Concerns](http://deviq.com/separation-of-concerns/): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="fca84-109">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)) in diesem Artikel ist einfacher, fügen Sie die Beispiel-app.</span><span class="sxs-lookup"><span data-stu-id="fca84-109">[View or download sample code](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)) This article is easier to follow with the sample app.</span></span>

## <a name="basic-options-configuration"></a><span data-ttu-id="fca84-110">Grundlegende Optionen-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="fca84-110">Basic options configuration</span></span>

<span data-ttu-id="fca84-111">Grundlegende Optionskonfiguration wird als Beispiel veranschaulicht &num;1 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-111">Basic options configuration is demonstrated as Example &num;1 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-112">Eine Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="fca84-112">An options class must be non-abstract with a public parameterless constructor.</span></span> <span data-ttu-id="fca84-113">Die folgende Klasse `MyOptions`, verfügt über zwei Eigenschaften `Option1` und `Option2`.</span><span class="sxs-lookup"><span data-stu-id="fca84-113">The following class, `MyOptions`, has two properties, `Option1` and `Option2`.</span></span> <span data-ttu-id="fca84-114">Festlegen von Standardwerten ist optional, aber den Konstruktor der Klasse im folgenden Beispiel wird der Standardwert von `Option1`.</span><span class="sxs-lookup"><span data-stu-id="fca84-114">Setting default values is optional, but the class constructor in the following example sets the default value of `Option1`.</span></span> <span data-ttu-id="fca84-115">`Option2`hat den Standardwert festgelegt, indem Sie die Eigenschaft direkt zu initialisieren (*Models/MyOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-115">`Option2` has a default value set by initializing the property directly (*Models/MyOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

<span data-ttu-id="fca84-116">Die `MyOptions` Klasse hinzugefügt, mit dem Dienstcontainer [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) und Konfiguration gebunden:</span><span class="sxs-lookup"><span data-stu-id="fca84-116">The `MyOptions` class is added to the service container with [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) and bound to configuration:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

<span data-ttu-id="fca84-117">Die folgende Seite verwendet [Konstruktor Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#what-is-dependency-injection) mit [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) auf die Einstellungen zugreifen (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-117">The following page model uses [constructor dependency injection](xref:fundamentals/dependency-injection#what-is-dependency-injection) with [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) to access the settings (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

<span data-ttu-id="fca84-118">Im Beispiels *appsettings.json* Datei gibt Werte für `option1` und `option2`:</span><span class="sxs-lookup"><span data-stu-id="fca84-118">The sample's *appsettings.json* file specifies values for `option1` and `option2`:</span></span>

[!code-json[Main](options/sample/appsettings.json)]

<span data-ttu-id="fca84-119">Wenn die app ausgeführt wird, wird die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge, die die Optionswerte für die Klasse angezeigt:</span><span class="sxs-lookup"><span data-stu-id="fca84-119">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a><span data-ttu-id="fca84-120">Konfigurieren Sie einfache Optionen mit einem Delegaten</span><span class="sxs-lookup"><span data-stu-id="fca84-120">Configure simple options with a delegate</span></span>

<span data-ttu-id="fca84-121">Konfigurieren der einfachen Optionen mit einem Delegaten wird veranschaulicht, wie Beispiel &num;2 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-121">Configuring simple options with a delegate is demonstrated as Example &num;2 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-122">Verwenden von Delegaten Optionen Werte festlegen.</span><span class="sxs-lookup"><span data-stu-id="fca84-122">Use a delegate to set options values.</span></span> <span data-ttu-id="fca84-123">Das Beispiel-app verwendet die `MyOptionsWithDelegateConfig` Klasse (*Models/MyOptionsWithDelegateConfig.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-123">The sample app uses the `MyOptionsWithDelegateConfig` class (*Models/MyOptionsWithDelegateConfig.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

<span data-ttu-id="fca84-124">Im folgenden Code wird eine zweite `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="fca84-124">In the following code, a second `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="fca84-125">Er verwendet ein Delegat zum Konfigurieren der Bindung mit `MyOptionsWithDelegateConfig`:</span><span class="sxs-lookup"><span data-stu-id="fca84-125">It uses a delegate to configure the binding with `MyOptionsWithDelegateConfig`:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

<span data-ttu-id="fca84-126">*Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="fca84-126">*Index.cshtml.cs*:</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

<span data-ttu-id="fca84-127">Sie können mehrere Konfigurationsanbieter hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="fca84-127">You can add multiple configuration providers.</span></span> <span data-ttu-id="fca84-128">Konfigurationsanbieter stehen im NuGet-Pakete zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="fca84-128">Configuration providers are available in NuGet packages.</span></span> <span data-ttu-id="fca84-129">Sie sind angewendet, sodass sie registriert sind.</span><span class="sxs-lookup"><span data-stu-id="fca84-129">They're applied in order that they're registered.</span></span>

<span data-ttu-id="fca84-130">Jeder Aufruf von [konfigurieren&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) Fügt eine `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer.</span><span class="sxs-lookup"><span data-stu-id="fca84-130">Each call to [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) adds an `IConfigureOptions<TOptions>` service to the service container.</span></span> <span data-ttu-id="fca84-131">Im vorherigen Beispiel, die Werte der `Option1` und `Option2` werden in angegebenen *appsettings.json*, aber die Werte der `Option1` und `Option2` werden von der konfigurierten Delegat überschrieben.</span><span class="sxs-lookup"><span data-stu-id="fca84-131">In the preceding example, the values of `Option1` and `Option2` are both specified in *appsettings.json*, but the values of `Option1` and `Option2` are overridden by the configured delegate.</span></span>

<span data-ttu-id="fca84-132">Wenn mehr als ein Dienst aktiviert ist, die letzte Konfigurationsquelle angegeben *Wins* und legt den Konfigurationswert fest.</span><span class="sxs-lookup"><span data-stu-id="fca84-132">When more than one configuration service is enabled, the last configuration source specified *wins* and sets the configuration value.</span></span> <span data-ttu-id="fca84-133">Wenn die app ausgeführt wird, wird die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge, die die Optionswerte für die Klasse angezeigt:</span><span class="sxs-lookup"><span data-stu-id="fca84-133">When the app is run, the page model's `OnGet` method returns a string showing the option class values:</span></span>

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a><span data-ttu-id="fca84-134">Unteroptionen-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="fca84-134">Suboptions configuration</span></span>

<span data-ttu-id="fca84-135">Unteroptionen Konfiguration wird als Beispiel veranschaulicht &num;3 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-135">Suboptions configuration is demonstrated as Example &num;3 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-136">Apps sollten Optionsklassen erstellen, die bestimmte Funktion Gruppen (Klassen) in der app betreffen.</span><span class="sxs-lookup"><span data-stu-id="fca84-136">Apps should create options classes that pertain to specific feature groups (classes) in the app.</span></span> <span data-ttu-id="fca84-137">Teile der app, die Konfigurationswerte erfordern sollte nur den Zugriff auf die Konfigurationswerte aufweisen, die sie verwenden.</span><span class="sxs-lookup"><span data-stu-id="fca84-137">Parts of the app that require configuration values should only have access to the configuration values that they use.</span></span>

<span data-ttu-id="fca84-138">Beim Binden von Optionen zur Konfiguration ist jede Eigenschaft im Optionstyp einem Konfigurationsschlüssel des Formulars gebunden `property[:sub-property:]`.</span><span class="sxs-lookup"><span data-stu-id="fca84-138">When binding options to configuration, each property in the options type is bound to a configuration key of the form `property[:sub-property:]`.</span></span> <span data-ttu-id="fca84-139">Z. B. die `MyOptions.Option1` Eigenschaft gebunden ist, auf den Schlüssel `Option1`, das Auslesen der `option1` Eigenschaft im *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="fca84-139">For example, the `MyOptions.Option1` property is bound to the key `Option1`, which is read from the `option1` property in *appsettings.json*.</span></span>

<span data-ttu-id="fca84-140">Im folgenden Code ein drittes `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="fca84-140">In the following code, a third `IConfigureOptions<TOptions>` service is added to the service container.</span></span> <span data-ttu-id="fca84-141">Sie bindet `MySubOptions` Abschnitt `subsection` von der *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="fca84-141">It binds `MySubOptions` to the section `subsection` of the *appsettings.json* file:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

<span data-ttu-id="fca84-142">Die `GetSection` Erweiterungsmethode erfordert die [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="fca84-142">The `GetSection` extension method requires the [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet package.</span></span> <span data-ttu-id="fca84-143">Wenn die app verwendet die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Metapackage, das Paket wird automatisch mit eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="fca84-143">If the app uses the [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) metapackage, the package is automatically included.</span></span>

<span data-ttu-id="fca84-144">Im Beispiels *appsettings.json* -Datei definiert eine `subsection` Element mit dem Schlüssel für `suboption1` und `suboption2`:</span><span class="sxs-lookup"><span data-stu-id="fca84-144">The sample's *appsettings.json* file defines a `subsection` member with keys for `suboption1` and `suboption2`:</span></span>

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

<span data-ttu-id="fca84-145">Die `MySubOptions` Klasse definiert die Eigenschaften, `SubOption1` und `SubOption2`zur Aufnahme der untergeordneten Optionswerte (*Models/MySubOptions.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-145">The `MySubOptions` class defines properties, `SubOption1` and `SubOption2`, to hold the sub-option values (*Models/MySubOptions.cs*):</span></span>

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

<span data-ttu-id="fca84-146">Die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge mit der untergeordneten Optionswerte (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-146">The page model's `OnGet` method returns a string with the sub-option values (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

<span data-ttu-id="fca84-147">Wenn die app ausgeführt wird, die `OnGet` Methode gibt eine Zeichenfolge, die mit der untergeordneten Option Klasse-Werten zurück:</span><span class="sxs-lookup"><span data-stu-id="fca84-147">When the app is run, the `OnGet` method returns a string showing the sub-option class values:</span></span>

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a><span data-ttu-id="fca84-148">Optionen, die durch ein Modell anzeigen oder mit direkten injection</span><span class="sxs-lookup"><span data-stu-id="fca84-148">Options provided by a view model or with direct view injection</span></span>

<span data-ttu-id="fca84-149">Optionen, die von einem Ansichtsmodell oder mit direkten Injection bereitgestellten wird veranschaulicht, wie Beispiel &num;4 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-149">Options provided by a view model or with direct view injection is demonstrated as Example &num;4 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-150">Optionen können angegeben werden, in einem Ansichtsmodell oder Räumen `IOptions<TOptions>` direkt in eine Sicht (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-150">Options can be supplied in a view model or by injecting `IOptions<TOptions>` directly into a view (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

<span data-ttu-id="fca84-151">Für die direkte Injection einfügen `IOptions<MyOptions>` mit einem `@inject` Richtlinie:</span><span class="sxs-lookup"><span data-stu-id="fca84-151">For direct injection, inject `IOptions<MyOptions>` with an `@inject` directive:</span></span>

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

<span data-ttu-id="fca84-152">Wenn die Anwendung ausgeführt wird, werden die Optionswerte in der gerenderten Seite angezeigt:</span><span class="sxs-lookup"><span data-stu-id="fca84-152">When the app is run, the option values are shown in the rendered page:</span></span>

![Optionen für Werte Option1: value1_from_json und Option2:-1 werden von Injection in die Sicht aus dem Modell geladen.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a><span data-ttu-id="fca84-154">Laden Sie Konfigurationsdaten mit IOptionsSnapshot</span><span class="sxs-lookup"><span data-stu-id="fca84-154">Reload configuration data with IOptionsSnapshot</span></span>

<span data-ttu-id="fca84-155">Erneutes Laden der Konfigurationsdaten mit `IOptionsSnapshot` wird im Beispiel veranschaulicht &num;5 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-155">Reloading configuration data with `IOptionsSnapshot` is demonstrated in Example &num;5 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-156">*Erfordert ASP.NET Core 1.1 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="fca84-156">*Requires ASP.NET Core 1.1 or later.*</span></span>

<span data-ttu-id="fca84-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) Optionen mit minimalen Verarbeitungsaufwand Neuladen unterstützt.</span><span class="sxs-lookup"><span data-stu-id="fca84-157">[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) supports reloading options with minimal processing overhead.</span></span> <span data-ttu-id="fca84-158">In ASP.NET Core 1.1 `IOptionsSnapshot` ist eine Momentaufnahme des [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) und Updates automatisch bei jedem der Monitor ausgelöst werden. Änderungen auf Grundlage der Daten-Quelle geändert.</span><span class="sxs-lookup"><span data-stu-id="fca84-158">In ASP.NET Core 1.1, `IOptionsSnapshot` is a snapshot of [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) and updates automatically whenever the monitor triggers changes based on the data source changing.</span></span> <span data-ttu-id="fca84-159">In ASP.NET Core 2.0 und höher werden Optionen einmal pro Anforderung beim Zugriff auf und für die Lebensdauer der Anforderung zwischengespeichert berechnet.</span><span class="sxs-lookup"><span data-stu-id="fca84-159">In ASP.NET Core 2.0 and later, options are computed once per request when accessed and cached for the lifetime of the request.</span></span>

<span data-ttu-id="fca84-160">Im folgende Beispiel wird veranschaulicht, wie ein neuer `IOptionsSnapshot` wird erstellt, nachdem *appsettings.json* Änderungen (*Pages/Index.cshtml.cs*).</span><span class="sxs-lookup"><span data-stu-id="fca84-160">The following example demonstrates how a new `IOptionsSnapshot` is created after *appsettings.json* changes (*Pages/Index.cshtml.cs*).</span></span> <span data-ttu-id="fca84-161">Mehrere Anforderungen an den Server zurückgegeben, Konstante Werte, die bereitgestellt werden, indem Sie die *appsettings.json* Datei, bis die Datei geändert wird und die Konfiguration lädt.</span><span class="sxs-lookup"><span data-stu-id="fca84-161">Multiple requests to the server return constant values provided by the *appsettings.json* file until the file is changed and configuration reloads.</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

<span data-ttu-id="fca84-162">Die folgende Abbildung zeigt die anfängliche `option1` und `option2` Werte geladen wird, aus der *appsettings.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="fca84-162">The following image shows the initial `option1` and `option2` values loaded from the *appsettings.json* file:</span></span>

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

<span data-ttu-id="fca84-163">Ändern Sie die Werte in der *appsettings.json* Datei `value1_from_json UPDATED` und `200`.</span><span class="sxs-lookup"><span data-stu-id="fca84-163">Change the values in the *appsettings.json* file to `value1_from_json UPDATED` and `200`.</span></span> <span data-ttu-id="fca84-164">Speichern Sie die *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="fca84-164">Save the *appsettings.json* file.</span></span> <span data-ttu-id="fca84-165">Aktualisieren Sie den Browser, um festzustellen, ob die Optionen Werte aktualisiert werden:</span><span class="sxs-lookup"><span data-stu-id="fca84-165">Refresh the browser to see that the options values are updated:</span></span>

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a><span data-ttu-id="fca84-166">Optionen-Unterstützung mit IConfigureNamedOptions namens</span><span class="sxs-lookup"><span data-stu-id="fca84-166">Named options support with IConfigureNamedOptions</span></span>

<span data-ttu-id="fca84-167">Mit dem Namen Optionen-Unterstützung mit [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) wird als Beispiel veranschaulicht &num;6 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span><span class="sxs-lookup"><span data-stu-id="fca84-167">Named options support with [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) is demonstrated as Example &num;6 in the [sample app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).</span></span>

<span data-ttu-id="fca84-168">*Erfordert ASP.NET Core 2.0 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="fca84-168">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="fca84-169">*Mit dem Namen Optionen* Unterstützung ermöglicht der app, die zwischen den Konfigurationen benannte Optionen unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="fca84-169">*Named options* support allows the app to distinguish between named options configurations.</span></span> <span data-ttu-id="fca84-170">In der Beispiel-app benannte Optionen deklariert werden, mit der [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurieren Sie](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) Methode:</span><span class="sxs-lookup"><span data-stu-id="fca84-170">In the sample app, named options are declared with the [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) method:</span></span>

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

<span data-ttu-id="fca84-171">Die Beispiel-app greift auf die benannte Optionen mit [IOptionsSnapshot&lt;TOptions&gt;. Abrufen](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="fca84-171">The sample app accesses the named options with [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):</span></span>

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

<span data-ttu-id="fca84-172">Ausführen der Beispielapp, werden die benannten Optionen zurückgegeben:</span><span class="sxs-lookup"><span data-stu-id="fca84-172">Running the sample app, the named options are returned:</span></span>

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

<span data-ttu-id="fca84-173">`named_options_1`Werte werden bereitgestellt, aus der Konfiguration, die aus geladen wird: der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="fca84-173">`named_options_1` values are provided from configuration, which are loaded from the *appsettings.json* file.</span></span> <span data-ttu-id="fca84-174">`named_options_2`Werte werden vom bereitgestellt:</span><span class="sxs-lookup"><span data-stu-id="fca84-174">`named_options_2` values are provided by:</span></span>

* <span data-ttu-id="fca84-175">Die `named_options_2` in Delegieren `ConfigureServices` für `Option1`.</span><span class="sxs-lookup"><span data-stu-id="fca84-175">The `named_options_2` delegate in `ConfigureServices` for `Option1`.</span></span>
* <span data-ttu-id="fca84-176">Der Standardwert für `Option2` gebotenen der `MyOptions` Klasse.</span><span class="sxs-lookup"><span data-stu-id="fca84-176">The default value for `Option2` provided by the `MyOptions` class.</span></span>

<span data-ttu-id="fca84-177">Konfigurieren Sie alle Optionen benannte Instanzen mit dem [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) Methode.</span><span class="sxs-lookup"><span data-stu-id="fca84-177">Configure all named options instances with the [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) method.</span></span> <span data-ttu-id="fca84-178">Der folgende Code konfiguriert `Option1` mit dem Namen der für alle Instanzen der Konfiguration mit einem gemeinsamen Wert.</span><span class="sxs-lookup"><span data-stu-id="fca84-178">The following code configures `Option1` for all named configuration instances with a common value.</span></span> <span data-ttu-id="fca84-179">Fügen Sie den folgenden Code manuell die `Configure` Methode:</span><span class="sxs-lookup"><span data-stu-id="fca84-179">Add the following code manually to the `Configure` method:</span></span>

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

<span data-ttu-id="fca84-180">Ausführen der Beispiel-app nach dem Hinzufügen des Codes führt zu folgendem Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="fca84-180">Running the sample app after adding the code produces the following result:</span></span>

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> <span data-ttu-id="fca84-181">In ASP.NET Core 2.0 und höher sind alle Optionen benannte Instanzen.</span><span class="sxs-lookup"><span data-stu-id="fca84-181">In ASP.NET Core 2.0 and later, all options are named instances.</span></span> <span data-ttu-id="fca84-182">Vorhandene `IConfigureOption` Instanzen werden behandelt, als Ziel der `Options.DefaultName` -Instanz, die ist `string.Empty`.</span><span class="sxs-lookup"><span data-stu-id="fca84-182">Existing `IConfigureOption` instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="fca84-183">`IConfigureNamedOptions`implementiert außerdem `IConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="fca84-183">`IConfigureNamedOptions` also implements `IConfigureOptions`.</span></span> <span data-ttu-id="fca84-184">Die standardmäßige Implementierung des der [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([Verweisquelle](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) verfügt über eine Logik ordnungsgemäß verwendet.</span><span class="sxs-lookup"><span data-stu-id="fca84-184">The default implementation of the [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([reference source](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) has logic to use each appropriately.</span></span> <span data-ttu-id="fca84-185">Die `null` benannte Option wird verwendet, um alle benannten Instanzen anstelle einer bestimmten benannten Instanz als Ziel ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) und [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) dieser Konvention verwenden).</span><span class="sxs-lookup"><span data-stu-id="fca84-185">The `null` named option is used to target all of the named instances instead of a specific named instance ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) and [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) use this convention).</span></span>

## <a name="ipostconfigureoptions"></a><span data-ttu-id="fca84-186">IPostConfigureOptions</span><span class="sxs-lookup"><span data-stu-id="fca84-186">IPostConfigureOptions</span></span>

<span data-ttu-id="fca84-187">*Erfordert ASP.NET Core 2.0 oder höher.*</span><span class="sxs-lookup"><span data-stu-id="fca84-187">*Requires ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="fca84-188">Legen Sie mit postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span><span class="sxs-lookup"><span data-stu-id="fca84-188">Set postconfiguration with [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1).</span></span> <span data-ttu-id="fca84-189">PostConfiguration ausführt, nachdem alle [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) Konfiguration auftritt:</span><span class="sxs-lookup"><span data-stu-id="fca84-189">Postconfiguration runs after all [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) configuration occurs:</span></span>

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="fca84-190">[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) ist nach dem Konfigurieren der benannten Optionen verfügbar:</span><span class="sxs-lookup"><span data-stu-id="fca84-190">[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="fca84-191">Verwendung [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) alle nach dem konfigurieren, mit dem Namen Konfiguration Instanzen:</span><span class="sxs-lookup"><span data-stu-id="fca84-191">Use [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) to post-configure all named configuration instances:</span></span>

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a><span data-ttu-id="fca84-192">Optionen-Factory, Überwachung und cache</span><span class="sxs-lookup"><span data-stu-id="fca84-192">Options factory, monitoring, and cache</span></span>

<span data-ttu-id="fca84-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) wird für Benachrichtigungen verwendet, wenn `TOptions` Instanzen ändern.</span><span class="sxs-lookup"><span data-stu-id="fca84-193">[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) is used for notifications when `TOptions` instances change.</span></span> <span data-ttu-id="fca84-194">`IOptionsMonitor`reloadable Optionen unterstützt, änderungsbenachrichtigungen und `IPostConfigureOptions`.</span><span class="sxs-lookup"><span data-stu-id="fca84-194">`IOptionsMonitor` supports reloadable options, change notifications, and `IPostConfigureOptions`.</span></span>

<span data-ttu-id="fca84-195">[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 oder höher) ist für das Erstellen neuer Instanzen Optionen verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="fca84-195">[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 or later) is responsible for creating new options instances.</span></span> <span data-ttu-id="fca84-196">Er verfügt über ein einzelnes [erstellen](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) Methode.</span><span class="sxs-lookup"><span data-stu-id="fca84-196">It has a single [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) method.</span></span> <span data-ttu-id="fca84-197">Die standardmäßige Implementierung akzeptiert alle registrierten `IConfigureOptions` und `IPostConfigureOptions` und führt alle die zuerst konfiguriert, gefolgt von den nachträglich konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="fca84-197">The default implementation takes all registered `IConfigureOptions` and `IPostConfigureOptions` and runs all the configures first, followed by the post-configures.</span></span> <span data-ttu-id="fca84-198">Es unterscheidet zwischen `IConfigureNamedOptions` und `IConfigureOptions` und ruft nur die entsprechende Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="fca84-198">It distinguishes between `IConfigureNamedOptions` and `IConfigureOptions` and only calls the appropriate interface.</span></span>

<span data-ttu-id="fca84-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span><span class="sxs-lookup"><span data-stu-id="fca84-199">[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances.</span></span> <span data-ttu-id="fca84-200">Die `IOptionsMonitorCache` Optionen Instanzen im Systemmonitor erklärt, sodass der Wert neu berechnet wird ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span><span class="sxs-lookup"><span data-stu-id="fca84-200">The `IOptionsMonitorCache` invalidates options instances in the monitor so that the value is recomputed ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)).</span></span> <span data-ttu-id="fca84-201">Werte können manuell eingeleitet werden ebenfalls mit [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span><span class="sxs-lookup"><span data-stu-id="fca84-201">Values can be manually introduced as well with [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd).</span></span> <span data-ttu-id="fca84-202">Die [deaktivieren](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) Methode wird verwendet, wenn alle benannten Instanzen bei Bedarf neu erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="fca84-202">The [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) method is used when all named instances should be recreated on demand.</span></span>

## <a name="accessing-options-during-startup"></a><span data-ttu-id="fca84-203">Beim Zugriff auf Optionen während des Starts</span><span class="sxs-lookup"><span data-stu-id="fca84-203">Accessing options during startup</span></span>

<span data-ttu-id="fca84-204">`IOptions`kann verwendet werden, `Configure`, da Dienste, bevor erstellt werden die `Configure` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fca84-204">`IOptions` can be used in `Configure`, since services are built before the `Configure` method executes.</span></span> <span data-ttu-id="fca84-205">Wenn ein Dienstanbieter in integrierten `ConfigureServices` um Optionen zuzugreifen, er wäre nicht enthalten sein Konfigurationen nach der Erstellung des Dienstanbieters bereitgestellten Optionen.</span><span class="sxs-lookup"><span data-stu-id="fca84-205">If a service provider is built in `ConfigureServices` to access options, it wouldn't contain any options configurations provided after the service provider is built.</span></span> <span data-ttu-id="fca84-206">Aus diesem Grund kann Zustand inkonsistente Optionen vorhanden, aufgrund der Reihenfolge von Dienst-Registrierungen.</span><span class="sxs-lookup"><span data-stu-id="fca84-206">Therefore, an inconsistent options state may exist due to the ordering of service registrations.</span></span>

<span data-ttu-id="fca84-207">Da Optionen in der Regel aus der Konfiguration geladen werden, kann Konfiguration verwendet werden, in den starttasks in beiden `Configure` und `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="fca84-207">Since options are typically loaded from configuration, configuration can be used in startup in both `Configure` and `ConfigureServices`.</span></span> <span data-ttu-id="fca84-208">Beispiele für die Verwendung der Konfiguration während des Starts, finden Sie unter der [Anwendungsstart](xref:fundamentals/startup) Thema.</span><span class="sxs-lookup"><span data-stu-id="fca84-208">For examples of using configuration during startup, see the [Application startup](xref:fundamentals/startup) topic.</span></span>

## <a name="see-also"></a><span data-ttu-id="fca84-209">Siehe auch</span><span class="sxs-lookup"><span data-stu-id="fca84-209">See also</span></span>

* [<span data-ttu-id="fca84-210">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="fca84-210">Configuration</span></span>](xref:fundamentals/configuration/index)
