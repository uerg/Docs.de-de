---
title: Teile der Anwendung in ASP.NET Core
author: ardalis
description: "Informationen Sie zum Verwenden von Anwendungsparts, die Abstrations über die Ressourcen einer App, zum Konfigurieren Ihrer Anwendung zur Ermittlung oder das Laden von Funktionen aus einer Assembly verhindern."
keywords: ASP.NET Core, Anwendungsteil, die Teil der app
ms.author: riande
manager: wpickett
ms.date: 01/04/2017
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899963613a98
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/extensibility/app-parts
ms.openlocfilehash: a260675e7461105d4f6a0c61fd13971663c268f2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="application-parts-in-aspnet-core"></a><span data-ttu-id="b2c94-104">Teile der Anwendung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b2c94-104">Application Parts in ASP.NET Core</span></span>

<span data-ttu-id="b2c94-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2c94-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b2c94-106">Ein *Anwendungspart* ist eine Abstraktion für die Ressourcen einer Anwendung, von dem MVC von wie Domänencontroller, Komponenten anzeigen Features, oder die Tag-Hilfsprogrammen können ermittelt werden.</span><span class="sxs-lookup"><span data-stu-id="b2c94-106">An *Application Part* is an abstraction over the resources of an application, from which MVC features like controllers, view components, or tag helpers may be discovered.</span></span> <span data-ttu-id="b2c94-107">Ein Beispiel für einen Anwendungsteil ist ein AssemblyPart, der einen Assemblyverweis und stellt Typen und Kompilierung Verweise kapselt.</span><span class="sxs-lookup"><span data-stu-id="b2c94-107">One example of an application part is an AssemblyPart, which encapsulates an assembly reference and exposes types and compilation references.</span></span> <span data-ttu-id="b2c94-108">*Feature Anbieter* funktionieren mit Anwendungsparts, um die Funktionen von einer ASP.NET-MVC-Anwendung Core aufzufüllen.</span><span class="sxs-lookup"><span data-stu-id="b2c94-108">*Feature providers* work with application parts to populate the features of an ASP.NET Core MVC app.</span></span> <span data-ttu-id="b2c94-109">Hauptverwendungszweck für Teile der Anwendung zutrifft, ermöglichen Ihnen das Konfigurieren Ihrer Anwendung zur Ermittlung (oder vermeiden laden) MVC-Funktionen aus einer Assembly.</span><span class="sxs-lookup"><span data-stu-id="b2c94-109">The main use case for application parts is to allow you to configure your app to discover (or avoid loading) MVC features from an assembly.</span></span>

## <a name="introducing-application-parts"></a><span data-ttu-id="b2c94-110">Einführung in die Teile der Anwendung</span><span class="sxs-lookup"><span data-stu-id="b2c94-110">Introducing Application Parts</span></span>

<span data-ttu-id="b2c94-111">MVC-apps laden, deren Funktionen aus [Anwendungsparts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span><span class="sxs-lookup"><span data-stu-id="b2c94-111">MVC apps load their features from [application parts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart).</span></span> <span data-ttu-id="b2c94-112">Insbesondere die [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) Klasse stellt einen Anwendungsteil, der von einer Assembly unterstützt wird.</span><span class="sxs-lookup"><span data-stu-id="b2c94-112">In particular, the [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) class represents an application part that is backed by an assembly.</span></span> <span data-ttu-id="b2c94-113">Sie können diese Klassen verwenden, ermitteln und Laden von MVC-Funktionen, z. B. Controller, ansichtskomponenten Tag Hilfsprogramme und Razor Kompilierung Quellen.</span><span class="sxs-lookup"><span data-stu-id="b2c94-113">You can use these classes to discover and load MVC features, such as controllers, view components, tag helpers, and razor compilation sources.</span></span> <span data-ttu-id="b2c94-114">Die [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) ist verantwortlich für das Nachverfolgen von Anwendungsparts und verfügbaren Funktionsanbieter für die MVC-app.</span><span class="sxs-lookup"><span data-stu-id="b2c94-114">The [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) is responsible for tracking the application parts and feature providers available to the MVC app.</span></span> <span data-ttu-id="b2c94-115">Sie können die Interaktion mit der `ApplicationPartManager` in `Startup` beim Konfigurieren von MVC:</span><span class="sxs-lookup"><span data-stu-id="b2c94-115">You can interact with the `ApplicationPartManager` in `Startup` when you configure MVC:</span></span>

```csharp
// create an assembly part from a class's assembly
var assembly = typeof(Startup).GetTypeInfo().Assembly;
services.AddMvc()
    .AddApplicationPart(assembly);

// OR
var assembly = typeof(Startup).GetTypeInfo().Assembly;
var part = new AssemblyPart(assembly);
services.AddMvc()
    .ConfigureApplicationPartManager(apm => p.ApplicationParts.Add(part));
```

<span data-ttu-id="b2c94-116">Standardmäßig wird MVC die Abhängigkeitsstruktur suchen, und Suchen von Domänencontrollern (auch in anderen Assemblys).</span><span class="sxs-lookup"><span data-stu-id="b2c94-116">By default MVC will search the dependency tree and find controllers (even in other assemblies).</span></span> <span data-ttu-id="b2c94-117">Um eine beliebige Assembly (z. B. über ein Plug-in, das zum Zeitpunkt der Kompilierung verwiesen wird, ist nicht) zu laden, können Sie einen Anwendungsteil verwenden.</span><span class="sxs-lookup"><span data-stu-id="b2c94-117">To load an arbitrary assembly (for instance, from a plugin that isn't referenced at compile time), you can use an application part.</span></span>

<span data-ttu-id="b2c94-118">Können Sie Teile der Anwendung zu *vermeiden* Domänencontroller in einer bestimmten Assembly oder Speicherort gesucht.</span><span class="sxs-lookup"><span data-stu-id="b2c94-118">You can use application parts to *avoid* looking for controllers in a particular assembly or location.</span></span> <span data-ttu-id="b2c94-119">Sie können steuern, welche Teile (oder Assemblys) für die app verfügbar, durch ändern sind der `ApplicationParts` Auflistung von der `ApplicationPartManager`.</span><span class="sxs-lookup"><span data-stu-id="b2c94-119">You can control which parts (or assemblies) are available to the app by modifying the `ApplicationParts` collection of the `ApplicationPartManager`.</span></span> <span data-ttu-id="b2c94-120">Die Reihenfolge der Einträge in der `ApplicationParts` Auflistung ist nicht wichtig.</span><span class="sxs-lookup"><span data-stu-id="b2c94-120">The order of the entries in the `ApplicationParts` collection is not important.</span></span> <span data-ttu-id="b2c94-121">Es ist wichtig, die vollständige Konfiguration der `ApplicationPartManager` vor dem verwenden, um die Dienste im Container zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b2c94-121">It is important to fully configure the `ApplicationPartManager` before using it to configure services in the container.</span></span> <span data-ttu-id="b2c94-122">Beispielsweise sollten Sie vollständig Konfigurieren der `ApplicationPartManager` vor dem Aufrufen `AddControllersAsServices`.</span><span class="sxs-lookup"><span data-stu-id="b2c94-122">For example, you should fully configure the `ApplicationPartManager` before invoking `AddControllersAsServices`.</span></span> <span data-ttu-id="b2c94-123">Zu diesem Zweck Fehlschlagen bedeutet, dass der Controller in Teilen der Anwendung nach hinzugefügt, dass der Methodenaufruf wird nicht beeinträchtigt (wird nicht erhalten als Dienste registriert) was falsche Bevavior der Anwendung führen kann.</span><span class="sxs-lookup"><span data-stu-id="b2c94-123">Failing to do so, will mean that controllers in application parts added after that method call will not be affected (will not get registered as services) which might result in incorrect bevavior of your application.</span></span>

<span data-ttu-id="b2c94-124">Wenn Sie eine Assembly, die Domänencontroller enthält verfügen, Sie möchten nicht verwendet werden, entfernen Sie es aus der `ApplicationPartManager`:</span><span class="sxs-lookup"><span data-stu-id="b2c94-124">If you have an assembly that contains controllers you do not want to be used, remove it from the `ApplicationPartManager`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p =>
    {
        var dependentLibrary = p.ApplicationParts
            .FirstOrDefault(part => part.Name == "DependentLibrary");

        if (dependentLibrary != null)
        {
           p.ApplicationParts.Remove(dependentLibrary);
        }
    })
```

<span data-ttu-id="b2c94-125">Zusätzlich zu der Assembly des Projekts und dessen abhängige Assemblys die `ApplicationPartManager` enthält Teile für `Microsoft.AspNetCore.Mvc.TagHelpers` und `Microsoft.AspNetCore.Mvc.Razor` standardmäßig.</span><span class="sxs-lookup"><span data-stu-id="b2c94-125">In addition to your project's assembly and its dependent assemblies, the `ApplicationPartManager` will include parts for `Microsoft.AspNetCore.Mvc.TagHelpers` and `Microsoft.AspNetCore.Mvc.Razor` by default.</span></span>

## <a name="application-feature-providers"></a><span data-ttu-id="b2c94-126">Anwendung Featureanbieter</span><span class="sxs-lookup"><span data-stu-id="b2c94-126">Application Feature Providers</span></span>

<span data-ttu-id="b2c94-127">Anwendung Anwendungsparts untersuchen und bieten Funktionen für die Teile.</span><span class="sxs-lookup"><span data-stu-id="b2c94-127">Application Feature Providers examine application parts and provide features for those parts.</span></span> <span data-ttu-id="b2c94-128">Es sind Anbieter für die integrierte Funktion für die MVC-Funktionen:</span><span class="sxs-lookup"><span data-stu-id="b2c94-128">There are built-in feature providers for the following MVC features:</span></span>

* [<span data-ttu-id="b2c94-129">Controller</span><span class="sxs-lookup"><span data-stu-id="b2c94-129">Controllers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [<span data-ttu-id="b2c94-130">Metadatenverweis</span><span class="sxs-lookup"><span data-stu-id="b2c94-130">Metadata Reference</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [<span data-ttu-id="b2c94-131">Taghilfsprogramme</span><span class="sxs-lookup"><span data-stu-id="b2c94-131">Tag Helpers</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [<span data-ttu-id="b2c94-132">Anzeigen von Komponenten</span><span class="sxs-lookup"><span data-stu-id="b2c94-132">View Components</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

<span data-ttu-id="b2c94-133">Funktionsanbieter Vererben `IApplicationFeatureProvider<T>`, wobei `T` ist der Typ der Funktion.</span><span class="sxs-lookup"><span data-stu-id="b2c94-133">Feature providers inherit from `IApplicationFeatureProvider<T>`, where `T` is the type of the feature.</span></span> <span data-ttu-id="b2c94-134">Sie können eine eigene Funktion implementieren-Anbieter für keines MVCs-Funktionstypen oben aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="b2c94-134">You can implement your own feature providers for any of MVC's feature types listed above.</span></span> <span data-ttu-id="b2c94-135">Die Reihenfolge der Featureanbieter in der `ApplicationPartManager.FeatureProviders` Auflistung wichtig sein, da Aktionen, die von vorherigen Anbieter höher Anbieter reagieren können.</span><span class="sxs-lookup"><span data-stu-id="b2c94-135">The order of feature providers in the `ApplicationPartManager.FeatureProviders` collection can be important, since later providers can react to actions taken by previous providers.</span></span>

### <a name="sample-generic-controller-feature"></a><span data-ttu-id="b2c94-136">Beispiel: Generische Controller-Funktion</span><span class="sxs-lookup"><span data-stu-id="b2c94-136">Sample: Generic controller feature</span></span>

<span data-ttu-id="b2c94-137">Standardmäßig ignoriert Core ASP.NET-MVC generische Controller (z. B. `SomeController<T>`).</span><span class="sxs-lookup"><span data-stu-id="b2c94-137">By default, ASP.NET Core MVC ignores generic controllers (for example, `SomeController<T>`).</span></span> <span data-ttu-id="b2c94-138">Dieses Beispiel verwendet einen Controller Feature-Anbieter, der ausgeführt wird, nachdem der Standardanbieter und fügt Controllerinstanzen der generischen für eine angegebene Liste von Typen (definiert `EntityTypes.Types`):</span><span class="sxs-lookup"><span data-stu-id="b2c94-138">This sample uses a controller feature provider that runs after the default provider and adds generic controller instances for a specified list of types (defined in `EntityTypes.Types`):</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

<span data-ttu-id="b2c94-139">Die Entitätstypen:</span><span class="sxs-lookup"><span data-stu-id="b2c94-139">The entity types:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

<span data-ttu-id="b2c94-140">Der Funktionsanbieter wird hinzugefügt, `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b2c94-140">The feature provider is added in `Startup`:</span></span>

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

<span data-ttu-id="b2c94-141">Standardmäßig die Namen der generischen Domänencontroller für das routing verwendet werden würde, des Formulars *GenericController'1 [Widget]* anstelle von *Widget*.</span><span class="sxs-lookup"><span data-stu-id="b2c94-141">By default, the generic controller names used for routing would be of the form *GenericController\`1[Widget]* instead of *Widget*.</span></span> <span data-ttu-id="b2c94-142">So ändern Sie den Namen für den generischen Typ verwendet, die vom Controller entsprechen, wird das folgende Attribut verwendet:</span><span class="sxs-lookup"><span data-stu-id="b2c94-142">The following attribute is used to modify the name to correspond to the generic type used by the controller:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

<span data-ttu-id="b2c94-143">Die `GenericController` Klasse:</span><span class="sxs-lookup"><span data-stu-id="b2c94-143">The `GenericController` class:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

<span data-ttu-id="b2c94-144">Das Ergebnis, wenn eine übereinstimmende Route angefordert wird:</span><span class="sxs-lookup"><span data-stu-id="b2c94-144">The result, when a matching route is requested:</span></span>

![Beispielausgabe aus der Beispiel-app liest, "Hello von einem generischen Sproket Controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a><span data-ttu-id="b2c94-146">Beispiel: Anzeige verfügbaren features</span><span class="sxs-lookup"><span data-stu-id="b2c94-146">Sample: Display available features</span></span>

<span data-ttu-id="b2c94-147">Sie durchlaufen können ausgefüllten verfügbaren Funktionen an Ihre app durch die Anforderung ein `ApplicationPartManager` über [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) und dazu verwenden, die zum Auffüllen der Instanzen der entsprechenden Funktionen:</span><span class="sxs-lookup"><span data-stu-id="b2c94-147">You can iterate through the populated features available to your app by requesting an `ApplicationPartManager` through [dependency injection](../../fundamentals/dependency-injection.md) and using it to populate instances of the appropriate features:</span></span>

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

<span data-ttu-id="b2c94-148">Beispielausgabe:</span><span class="sxs-lookup"><span data-stu-id="b2c94-148">Example output:</span></span>

![Beispielausgabe aus der Beispiel-app](app-parts/_static/available-features.png)
