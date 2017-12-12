---
title: Arbeiten mit dem Anwendungsmodell
author: ardalis
description: 
keywords: Core,ASP.NET Core ASP.NET-MVC Anwendungsmodell
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4eb7e52f-5665-41a4-a3e3-e348d07337f2
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/application-model
ms.openlocfilehash: 3c35184921dbe26cde100fd3d5124e38ea0d06cf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="working-with-the-application-model"></a>Arbeiten mit dem Anwendungsmodell

Durch [Steve Smith](https://ardalis.com/)

Core ASP.NET-MVC definiert ein *Anwendungsmodell* , die die Bestandteile einer MVC-app darstellt. Sie können lesen und Bearbeiten dieses Modell, um das Verhalten des MVC-Elemente zu ändern. MVC befolgt standardmäßig bestimmte Konventionen, um zu bestimmen, welche Klassen als Controller werden, welche Methoden für diese Klassen Aktionen ausgeführt werden und wie Parameter und routing Verhalten. Sie können dieses Verhalten, um die Bedürfnisse Ihrer app von Ihren eigenen Konventionen erstellen und sie global oder als Attribute angewendet anpassen.

## <a name="models-and-providers"></a>Modelle und Anbietern

Das Anwendungsmodell Core ASP.NET-MVC enthalten Schnittstellen abstrakte und konkrete Implementierung-Klassen, die eine MVC-Anwendung beschreiben. Dieses Modell ist das Ergebnis von MVC, Ermitteln der app Controller, Aktionen, Aktionsparameter, Routen und Filter gemäß Standardkonventionen. Mit dem Anwendungsmodell arbeiten, können Sie Ihre app aus, um das Standardverhalten für MVC unterschiedliche Konventionen ändern. Die Parameter, die Namen, die Routen und die Filter werden alle als Konfigurationsdaten für Aktionen und Controller verwendet.

Das Anwendungsmodell für ASP.NET Core MVC weist die folgende Struktur:

* ApplicationModel
    * Domänencontroller (ControllerModel)
        * Aktionen (ActionModel)
            * Parameter (ParameterModel)

Jede Ebene des Modells hat Zugriff auf eine gemeinsame `Properties` Auflistung und die niedrigeren Ebenen zugreifen und überschreiben Eigenschaftswerte, die von höheren Ebenen in der Hierarchie festlegen können. Die Eigenschaften werden beibehalten, um die `ActionDescriptor.Properties` wenn Aktionen erstellt werden. Klicken Sie dann, wenn eine Anforderung behandelt wird, alle Eigenschaften eine Konvention hinzugefügt oder geändert durch zugegriffen werden kann `ActionContext.ActionDescriptor.Properties`. Verwenden von Eigenschaften ist eine hervorragende Möglichkeit, Filter, Modellbinder usw. für pro-Aktion zu konfigurieren.

> [!NOTE]
> Die `ActionDescriptor.Properties` Auflistung ist nicht threadsicher (für Schreibvorgänge), sobald die app-Starts abgeschlossen ist. Konventionen sind die beste Möglichkeit, Daten, die dieser Sammlung hinzufügen.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC lädt das Anwendungsmodell unter Verwendung eines Anbieters-Musters, definiert durch die [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) Schnittstelle. Dieser Abschnitt enthält einige Details dazu, wie interne Implementierung dieser Anbieterfunktionen. Dies ist ein-Thema für fortgeschrittene – die meisten apps, die das Anwendungsmodell nutzen sollten mithilfe von Konventionen dies tun.

Implementierungen von der `IApplicationModelProvider` "umschließen Schnittstelle" miteinander, mit jeder Implementierung aufrufen `OnProvidersExecuting` in aufsteigender Reihenfolge auf Grundlage seiner `Order` Eigenschaft. Die `OnProvidersExecuted` Methode wird anschließend in umgekehrter Reihenfolge aufgerufen. Das Framework definiert die verschiedenen Anbietern:

Erste (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Klicken Sie dann (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Die Reihenfolge, in welche zwei Anbieter mit dem gleichen Wert für `Order` heißen ist nicht definiert, und daher nicht werden Sicherheitsmerkmal.

> [!NOTE]
> `IApplicationModelProvider`ist ein erweiterter Ansatz für Frameworkautoren von erweitern. Im Allgemeinen apps sollten Konventionen verwenden und Frameworks sollten Anbieter verwenden. Der Hauptunterschied besteht darin, dass der Anbieter immer vor dem Konventionen ausgeführt.

Die `DefaultApplicationModelProvider` viele das Standardverhalten von Core ASP.NET-MVC verwendet wird. Die folgende Verantwortungsbereiche:

* Hinzufügen von globalen Filter an den Kontext
* Hinzufügen von Domänencontrollern an den Kontext
* Hinzufügen von öffentlichen Controllermethoden als Aktionen
* Hinzufügen von Aktionsmethodenparameter an den Kontext
* Anwenden von Route und andere Attribute

Einige integrierte Verhaltensweisen, die von implementiert die `DefaultApplicationModelProvider`. Dieser Anbieter ist verantwortlich für die Erstellung der [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), das wiederum auf verweist [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), und [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) Instanzen. Die `DefaultApplicationModelProvider` Klasse wird vom internen Framework Implementierungsdetail beibehalten, die und wird in der Zukunft ändern können. 

Die `AuthorizationApplicationModelProvider` ist verantwortlich für das Verhalten beim Anwenden der `AuthorizeFilter` und `AllowAnonymousFilter` Attribute. [Erfahren Sie mehr über diese Attribute](xref:security/authorization/simple).

Die `CorsApplicationModelProvider` zugeordnete Verhalten implementiert die `IEnableCorsAttribute` und `IDisableCorsAttribute`, und die `DisableCorsAuthorizationFilter`. [Weitere Informationen zu CORS](xref:security/cors).

## <a name="conventions"></a>Konventionen

Das Anwendungsmodell definiert Konvention Abstraktionen, die eine einfachere Möglichkeit zum Anpassen des Verhaltens der Modelle als überschreiben das gesamte Modell oder Anbieter bereitstellen. Diese Abstraktionen sind die empfohlene Methode, um das Verhalten Ihrer app zu ändern. Konventionen bieten eine Möglichkeit, Code zu schreiben, die dynamisch Anpassungen angewendet wird. Während [Filter](xref:mvc/controllers/filters) bieten eine Möglichkeit zum Ändern der Framework-Verhalten, Anpassungen können Sie steuern, wie die gesamte app miteinander verkabelt ist.

Die folgenden Konventionen sind verfügbar:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

Konventionen werden angewendet, indem sie "Optionen" MVC hinzugefügt werden oder durch Implementierung `Attribute`s und Anwendung auf Domänencontrollern, Aktionen oder Aktionsparameter (ähnlich wie [ `Filters` ](xref:mvc/controllers/filters)). Im Unterschied zu Filtern werden die Konventionen nur ausgeführt, wenn die app, nicht als Teil jeder Anforderung gestartet wird.

### <a name="sample-modifying-the-applicationmodel"></a>Beispiel: Ändern der ApplicationModel

Die folgende Konvention wird verwendet, um eine Eigenschaft des Anwendungsmodells hinzufügen. 

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Anwendung Modell Konventionen gelten als Optionen, wenn es sich bei MVC hinzugefügt wird `ConfigureServices` in `Startup`.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Eigenschaften sind über die `ActionDescriptor` Properties-Auflistung in Controlleraktionen:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Beispiel: Ändern Sie die ControllerModel-Beschreibung

Wie im vorherigen Beispiel kann die Controllermodell auch geändert werden, um benutzerdefinierte Eigenschaften enthalten. Diese werden mit dem gleichen Namen in das Anwendungsmodell angegebene vorhandene Eigenschaften überschrieben. Die folgende Konvention Attribut fügt eine Beschreibung auf Controllerebene hinzu:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Diese Konvention wird als Attribut für einen Controller angewendet.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Die Eigenschaft "Beschreibung" wird auf die gleiche Weise wie in vorherigen Beispielen zugegriffen.

### <a name="sample-modifying-the-actionmodel-description"></a>Beispiel: Ändern Sie die ActionModel-Beschreibung

Eine separates Attribut Konvention kann auf einzelne Aktionen, Überschreiben von Verhalten, die bereits angewendet, auf die Anwendung oder Controllerebene angewendet werden.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Dies auf eine Aktion innerhalb der im vorherigen Beispiel Controller angewendet wird veranschaulicht, wie sie die Konvention Controllerebene überschreibt:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Beispiel: Ändern der ParameterModel

Die folgende Konvention auf, die zu ändernden Aktionsparameter angewendet werden kann ihre `BindingInfo`. Die folgende Konvention erfordert, dass der Parameter einen Routenparameter sein. andere mögliche Bindungsquellen (z. B. Abfragezeichenfolgen-Werte) werden ignoriert.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Das Attribut kann auf alle Aktionsparameter angewendet werden:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Beispiel: Ändern der ActionModel-Name

Die folgende Konvention ändert der `ActionModel` zum Aktualisieren der *Name* der Aktion, die auf das es angewendet wird. Der neue Name wird als Parameter für das Attribut bereitgestellt. Dieser neue Name wird vom routing verwendet, damit dies beeinflusst die Route, die zum Erreichen dieser Aktionsmethode verwendet.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Dieses Attribut wird angegeben, zu einer Aktionsmethode in der `HomeController`:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Obwohl der Methodenname ist `SomeName`, das Attribut überschreibt die MVC-Konvention für die Verwendung der Methodenname und ersetzt den Aktionsnamen mit `MyCoolAction`. Daher die Route verwendet, um diese Aktion zu erreichen ist `/Home/MyCoolAction`.

> [!NOTE]
> In diesem Beispiel entspricht im wesentlichen unter Verwendung der integrierten [ActionName](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) Attribut.

### <a name="sample-custom-routing-convention"></a>Beispiel: Benutzerdefinierte Routing-Konvention

Sie können eine `IApplicationModelConvention` Funktionsweise des Routings anpassen. Z. B. die folgende Konvention wird Controller-Namespaces in ihre Routen, und Ersetzen Sie dabei integrieren `.` im Namespace mit `/` in der Route:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Die Konvention wird als Option in den Starttasks hinzugefügt.

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Sie können die Konventionen zum Hinzufügen Ihrer [Middleware](xref:fundamentals/middleware) durch den Zugriff auf `MvcOptions` verwenden`services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

In diesem Beispiel gilt diese Konvention für Routen, die nicht routing-Attribut verwenden, auf dem der Controller "Namespace" im Namen aufweist. Die folgenden Controller veranschaulicht diese Konvention:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Modell der Anwendungsnutzung in WebApiCompatShim

ASP.NET Core MVC verwendet einen anderen Satz von Konventionen von ASP.NET Web API 2 an. Mithilfe von benutzerdefinierten Konventionen, können Sie eine ASP.NET Core MVC-app-Verhalten konsistent mit der eine Web-API-app ändern. Microsoft umfasst die [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) speziell für diesen Zweck.

> [!NOTE]
> Erfahren Sie mehr über [Migrieren von ASP.NET Web API](xref:migration/webapi).

Um der Web-API-Kompatibilität Shim verwenden zu können, müssen Sie das Projekt das Paket hinzu, und fügen Sie dann die Konventionen für MVC durch Aufrufen `AddWebApiConventions` in `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Die Konventionen der Shim gebotenen werden nur auf Teile der app angewendet, die hatten, bestimmte Attribute angewendet werden. Die folgenden vier Attribute werden zum Steuerelement verwendet, die Controller ihre Konventionen, die durch den Shim-Konventionen geändert haben, sollten:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Aktion-Konventionen

Die `UseWebApiActionConventionsAttribute` wird verwendet, um die HTTP-Methode Aktionen basierend auf deren Namen zugeordnet (z. B. `Get` zuordnen würden `HttpGet`). Dies gilt nur für Aktionen, die keine routing-Attribut verwenden.

### <a name="overloading"></a>Überladen

Die `UseWebApiOverloadingAttribute` dient zum Anwenden der `WebApiOverloadingApplicationModelConvention` Konvention. Diese Konvention Fügt eine `OverloadActionConstraint` auf die Aktion Auswahlprozesses verwendet werden, beschränkt die möglichen Aktionen, die für die die Anforderung alle nicht optionalen Parameter erfüllt.

### <a name="parameter-conventions"></a>Parameterkonventionen

Die `UseWebApiParameterConventionsAttribute` dient zum Anwenden der `WebApiParameterConventionsApplicationModelConvention` Aktion Konvention. Diese Konvention gibt an, dass einfache Typen, die als Aktionsparameter verwendet gebunden sind aus dem URI standardmäßig während der komplexe Typen aus dem Anforderungstext gebunden sind.

### <a name="routes"></a>Routen

Die `UseWebApiRoutesAttribute` steuert, ob die `WebApiApplicationModelConvention` Controller Konvention wird angewendet. Wenn aktiviert, wird dieser Konvention verwendet, zum Hinzufügen der Unterstützung für [Bereiche](xref:mvc/controllers/areas) der Route.

Zusätzlich zu einem Satz von Konventionen, die Kompatibilität-Paket enthält eine `System.Web.Http.ApiController` Basisklasse, die dem Bedingungsoperator für die Web-API ersetzt. Dadurch können Ihre Controller für Web-API und erben geschrieben aus seiner `ApiController` , wie sie beim Ausführen von auf ASP.NET Core MVC entwickelt wurden, funktionieren. Diese Klasse Basiscontroller ebenfalls mit allen ergänzt wird die `UseWebApi*` oben aufgeführten Attribute. Die `ApiController` verfügbar macht, Eigenschaften, Methoden und Typen, die mit denen im Web-API kompatibel sind.

## <a name="using-apiexplorer-to-document-your-app"></a>Dokumentieren Sie Ihre App mithilfe von ApiExplorer

Das Anwendungsmodell macht eine [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) Eigenschaft auf jeder Ebene, der zum Durchlaufen der app-Struktur verwendet werden kann. Dies kann zur [Hilfeseiten für Ihre Web-APIs, die mithilfe von Tools wie Swagger generieren](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). Die `ApiExplorer` Eigenschaft macht eine `IsVisible` -Eigenschaft, die festgelegt werden kann, um anzugeben, welche Teile des Modells für Ihre app verfügbar gemacht werden sollen. Sie können diese Einstellung, die mithilfe einer Konvention konfigurieren:

[!code-csharp[Main](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Mit diesem Ansatz (und zusätzliche Konventionen, falls erforderlich), können Sie aktivieren oder Deaktivieren von API-Sichtbarkeit auf jeder Ebene innerhalb Ihrer app. 
