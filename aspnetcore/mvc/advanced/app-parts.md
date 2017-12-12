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
# <a name="application-parts-in-aspnet-core"></a>Teile der Anwendung in ASP.NET Core

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/app-parts/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Ein *Anwendungspart* ist eine Abstraktion für die Ressourcen einer Anwendung, von dem MVC von wie Domänencontroller, Komponenten anzeigen Features, oder die Tag-Hilfsprogrammen können ermittelt werden. Ein Beispiel für einen Anwendungsteil ist ein AssemblyPart, der einen Assemblyverweis und stellt Typen und Kompilierung Verweise kapselt. *Feature Anbieter* funktionieren mit Anwendungsparts, um die Funktionen von einer ASP.NET-MVC-Anwendung Core aufzufüllen. Hauptverwendungszweck für Teile der Anwendung zutrifft, ermöglichen Ihnen das Konfigurieren Ihrer Anwendung zur Ermittlung (oder vermeiden laden) MVC-Funktionen aus einer Assembly.

## <a name="introducing-application-parts"></a>Einführung in die Teile der Anwendung

MVC-apps laden, deren Funktionen aus [Anwendungsparts](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpart). Insbesondere die [AssemblyPart](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.assemblypart#Microsoft_AspNetCore_Mvc_ApplicationParts_AssemblyPart) Klasse stellt einen Anwendungsteil, der von einer Assembly unterstützt wird. Sie können diese Klassen verwenden, ermitteln und Laden von MVC-Funktionen, z. B. Controller, ansichtskomponenten Tag Hilfsprogramme und Razor Kompilierung Quellen. Die [ApplicationPartManager](/aspnet/core/api/microsoft.aspnetcore.mvc.applicationparts.applicationpartmanager) ist verantwortlich für das Nachverfolgen von Anwendungsparts und verfügbaren Funktionsanbieter für die MVC-app. Sie können die Interaktion mit der `ApplicationPartManager` in `Startup` beim Konfigurieren von MVC:

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

Standardmäßig wird MVC die Abhängigkeitsstruktur suchen, und Suchen von Domänencontrollern (auch in anderen Assemblys). Um eine beliebige Assembly (z. B. über ein Plug-in, das zum Zeitpunkt der Kompilierung verwiesen wird, ist nicht) zu laden, können Sie einen Anwendungsteil verwenden.

Können Sie Teile der Anwendung zu *vermeiden* Domänencontroller in einer bestimmten Assembly oder Speicherort gesucht. Sie können steuern, welche Teile (oder Assemblys) für die app verfügbar, durch ändern sind der `ApplicationParts` Auflistung von der `ApplicationPartManager`. Die Reihenfolge der Einträge in der `ApplicationParts` Auflistung ist nicht wichtig. Es ist wichtig, die vollständige Konfiguration der `ApplicationPartManager` vor dem verwenden, um die Dienste im Container zu konfigurieren. Beispielsweise sollten Sie vollständig Konfigurieren der `ApplicationPartManager` vor dem Aufrufen `AddControllersAsServices`. Zu diesem Zweck Fehlschlagen bedeutet, dass der Controller in Teilen der Anwendung nach hinzugefügt, dass der Methodenaufruf wird nicht beeinträchtigt (wird nicht erhalten als Dienste registriert) was falsche Bevavior der Anwendung führen kann.

Wenn Sie eine Assembly, die Domänencontroller enthält verfügen, Sie möchten nicht verwendet werden, entfernen Sie es aus der `ApplicationPartManager`:

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

Zusätzlich zu der Assembly des Projekts und dessen abhängige Assemblys die `ApplicationPartManager` enthält Teile für `Microsoft.AspNetCore.Mvc.TagHelpers` und `Microsoft.AspNetCore.Mvc.Razor` standardmäßig.

## <a name="application-feature-providers"></a>Anwendung Featureanbieter

Anwendung Anwendungsparts untersuchen und bieten Funktionen für die Teile. Es sind Anbieter für die integrierte Funktion für die MVC-Funktionen:

* [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controllers.controllerfeatureprovider)
* [Metadatenverweis](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.compilation.metadatareferencefeatureprovider)
* [Taghilfsprogramme](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.razor.taghelpers.taghelperfeatureprovider)
* [Anzeigen von Komponenten](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.viewcomponents.viewcomponentfeatureprovider)

Funktionsanbieter Vererben `IApplicationFeatureProvider<T>`, wobei `T` ist der Typ der Funktion. Sie können eine eigene Funktion implementieren-Anbieter für keines MVCs-Funktionstypen oben aufgelistet. Die Reihenfolge der Featureanbieter in der `ApplicationPartManager.FeatureProviders` Auflistung wichtig sein, da Aktionen, die von vorherigen Anbieter höher Anbieter reagieren können.

### <a name="sample-generic-controller-feature"></a>Beispiel: Generische Controller-Funktion

Standardmäßig ignoriert Core ASP.NET-MVC generische Controller (z. B. `SomeController<T>`). Dieses Beispiel verwendet einen Controller Feature-Anbieter, der ausgeführt wird, nachdem der Standardanbieter und fügt Controllerinstanzen der generischen für eine angegebene Liste von Typen (definiert `EntityTypes.Types`):

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerFeatureProvider.cs?highlight=13&range=18-36)]

Die Entitätstypen:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Model/EntityTypes.cs?range=6-16)]

Der Funktionsanbieter wird hinzugefügt, `Startup`:

```csharp
services.AddMvc()
    .ConfigureApplicationPartManager(p => 
        p.FeatureProviders.Add(new GenericControllerFeatureProvider()));
```

Standardmäßig die Namen der generischen Domänencontroller für das routing verwendet werden würde, des Formulars *GenericController'1 [Widget]* anstelle von *Widget*. So ändern Sie den Namen für den generischen Typ verwendet, die vom Controller entsprechen, wird das folgende Attribut verwendet:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericControllerNameConvention.cs)]

Die `GenericController` Klasse:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/GenericController.cs?highlight=5-6)]

Das Ergebnis, wenn eine übereinstimmende Route angefordert wird:

![Beispielausgabe aus der Beispiel-app liest, "Hello von einem generischen Sproket Controller".](app-parts/_static/generic-controller.png)

### <a name="sample-display-available-features"></a>Beispiel: Anzeige verfügbaren features

Sie durchlaufen können ausgefüllten verfügbaren Funktionen an Ihre app durch die Anforderung ein `ApplicationPartManager` über [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) und dazu verwenden, die zum Auffüllen der Instanzen der entsprechenden Funktionen:

[!code-csharp[Main](./app-parts/sample/AppPartsSample/Controllers/FeaturesController.cs?highlight=16,25-27)]

Beispielausgabe:

![Beispielausgabe aus der Beispiel-app](app-parts/_static/available-features.png)
