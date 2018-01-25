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
# <a name="options-pattern-in-aspnet-core"></a>Optionen-Musters in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar. Wenn Konfigurationseinstellungen von der Funktion in getrennte Optionsklassen isoliert werden, unterliegen die app zwei wichtige Softwareentwicklung Prinzipien:

* Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/): Funktionen (Klassen), die auf Konfigurationseinstellungen beruhen abhängig sind, nur auf den Konfigurationseinstellungen, die sie verwenden.
* [Trennung von Anliegen](http://deviq.com/separation-of-concerns/): Einstellungen für die verschiedenen Teile der app werden nicht abhängige oder gekoppelten miteinander.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)) in diesem Artikel ist einfacher, fügen Sie die Beispiel-app.

## <a name="basic-options-configuration"></a>Grundlegende Optionen-Konfiguration

Grundlegende Optionskonfiguration wird als Beispiel veranschaulicht &num;1 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Eine Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor. Die folgende Klasse `MyOptions`, verfügt über zwei Eigenschaften `Option1` und `Option2`. Festlegen von Standardwerten ist optional, aber den Konstruktor der Klasse im folgenden Beispiel wird der Standardwert von `Option1`. `Option2`hat den Standardwert festgelegt, indem Sie die Eigenschaft direkt zu initialisieren (*Models/MyOptions.cs*):

[!code-csharp[Main](options/sample/Models/MyOptions.cs?name=snippet1)]

Die `MyOptions` Klasse hinzugefügt, mit dem Dienstcontainer [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) und Konfiguration gebunden:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example1)]

Die folgende Seite verwendet [Konstruktor Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#what-is-dependency-injection) mit [IOptions&lt;TOptions&gt; ](/dotnet/api/Microsoft.Extensions.Options.IOptions-1) auf die Einstellungen zugreifen (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Im Beispiels *appsettings.json* Datei gibt Werte für `option1` und `option2`:

[!code-json[Main](options/sample/appsettings.json)]

Wenn die app ausgeführt wird, wird die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge, die die Optionswerte für die Klasse angezeigt:

```html
option1 = value1_from_json, option2 = -1
```

## <a name="configure-simple-options-with-a-delegate"></a>Konfigurieren Sie einfache Optionen mit einem Delegaten

Konfigurieren der einfachen Optionen mit einem Delegaten wird veranschaulicht, wie Beispiel &num;2 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Verwenden von Delegaten Optionen Werte festlegen. Das Beispiel-app verwendet die `MyOptionsWithDelegateConfig` Klasse (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[Main](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Im folgenden Code wird eine zweite `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird. Er verwendet ein Delegat zum Konfigurieren der Bindung mit `MyOptionsWithDelegateConfig`:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Sie können mehrere Konfigurationsanbieter hinzufügen. Konfigurationsanbieter stehen im NuGet-Pakete zur Verfügung. Sie sind angewendet, sodass sie registriert sind.

Jeder Aufruf von [konfigurieren&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) Fügt eine `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer. Im vorherigen Beispiel, die Werte der `Option1` und `Option2` werden in angegebenen *appsettings.json*, aber die Werte der `Option1` und `Option2` werden von der konfigurierten Delegat überschrieben.

Wenn mehr als ein Dienst aktiviert ist, die letzte Konfigurationsquelle angegeben *Wins* und legt den Konfigurationswert fest. Wenn die app ausgeführt wird, wird die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge, die die Optionswerte für die Klasse angezeigt:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Unteroptionen-Konfiguration

Unteroptionen Konfiguration wird als Beispiel veranschaulicht &num;3 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Apps sollten Optionsklassen erstellen, die bestimmte Funktion Gruppen (Klassen) in der app betreffen. Teile der app, die Konfigurationswerte erfordern sollte nur den Zugriff auf die Konfigurationswerte aufweisen, die sie verwenden.

Beim Binden von Optionen zur Konfiguration ist jede Eigenschaft im Optionstyp einem Konfigurationsschlüssel des Formulars gebunden `property[:sub-property:]`. Z. B. die `MyOptions.Option1` Eigenschaft gebunden ist, auf den Schlüssel `Option1`, das Auslesen der `option1` Eigenschaft im *appsettings.json*.

Im folgenden Code ein drittes `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird. Sie bindet `MySubOptions` Abschnitt `subsection` von der *appsettings.json* Datei:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example3)]

Die `GetSection` Erweiterungsmethode erfordert die [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) NuGet-Paket. Wenn die app verwendet die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Metapackage, das Paket wird automatisch mit eingeschlossen.

Im Beispiels *appsettings.json* -Datei definiert eine `subsection` Element mit dem Schlüssel für `suboption1` und `suboption2`:

[!code-json[Main](options/sample/appsettings.json?highlight=4-7)]

Die `MySubOptions` Klasse definiert die Eigenschaften, `SubOption1` und `SubOption2`zur Aufnahme der untergeordneten Optionswerte (*Models/MySubOptions.cs*):

[!code-csharp[Main](options/sample/Models/MySubOptions.cs?name=snippet1)]

Die Seitenmodell `OnGet` Methode gibt eine Zeichenfolge mit der untergeordneten Optionswerte (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Wenn die app ausgeführt wird, die `OnGet` Methode gibt eine Zeichenfolge, die mit der untergeordneten Option Klasse-Werten zurück:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Optionen, die durch ein Modell anzeigen oder mit direkten injection

Optionen, die von einem Ansichtsmodell oder mit direkten Injection bereitgestellten wird veranschaulicht, wie Beispiel &num;4 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

Optionen können angegeben werden, in einem Ansichtsmodell oder Räumen `IOptions<TOptions>` direkt in eine Sicht (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Für die direkte Injection einfügen `IOptions<MyOptions>` mit einem `@inject` Richtlinie:

[!code-cshtml[Main](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Wenn die Anwendung ausgeführt wird, werden die Optionswerte in der gerenderten Seite angezeigt:

![Optionen für Werte Option1: value1_from_json und Option2:-1 werden von Injection in die Sicht aus dem Modell geladen.](options/_static/view.png)

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Laden Sie Konfigurationsdaten mit IOptionsSnapshot

Erneutes Laden der Konfigurationsdaten mit `IOptionsSnapshot` wird im Beispiel veranschaulicht &num;5 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Erfordert ASP.NET Core 1.1 oder höher.*

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) Optionen mit minimalen Verarbeitungsaufwand Neuladen unterstützt. In ASP.NET Core 1.1 `IOptionsSnapshot` ist eine Momentaufnahme des [IOptionsMonitor&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) und Updates automatisch bei jedem der Monitor ausgelöst werden. Änderungen auf Grundlage der Daten-Quelle geändert. In ASP.NET Core 2.0 und höher werden Optionen einmal pro Anforderung beim Zugriff auf und für die Lebensdauer der Anforderung zwischengespeichert berechnet.

Im folgende Beispiel wird veranschaulicht, wie ein neuer `IOptionsSnapshot` wird erstellt, nachdem *appsettings.json* Änderungen (*Pages/Index.cshtml.cs*). Mehrere Anforderungen an den Server zurückgegeben, Konstante Werte, die bereitgestellt werden, indem Sie die *appsettings.json* Datei, bis die Datei geändert wird und die Konfiguration lädt.

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Die folgende Abbildung zeigt die anfängliche `option1` und `option2` Werte geladen wird, aus der *appsettings.json* Datei:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Ändern Sie die Werte in der *appsettings.json* Datei `value1_from_json UPDATED` und `200`. Speichern Sie die *appsettings.json* Datei. Aktualisieren Sie den Browser, um festzustellen, ob die Optionen Werte aktualisiert werden:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Optionen-Unterstützung mit IConfigureNamedOptions namens

Mit dem Namen Optionen-Unterstützung mit [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) wird als Beispiel veranschaulicht &num;6 in der [Beispiel-app](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/options/sample).

*Erfordert ASP.NET Core 2.0 oder höher.*

*Mit dem Namen Optionen* Unterstützung ermöglicht der app, die zwischen den Konfigurationen benannte Optionen unterscheiden. In der Beispiel-app benannte Optionen deklariert werden, mit der [ConfigureNamedOptions&lt;TOptions&gt;. Konfigurieren Sie](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) Methode:

[!code-csharp[Main](options/sample/Startup.cs?name=snippet_Example6)]

Die Beispiel-app greift auf die benannte Optionen mit [IOptionsSnapshot&lt;TOptions&gt;. Abrufen](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[Main](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Ausführen der Beispielapp, werden die benannten Optionen zurückgegeben:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`Werte werden bereitgestellt, aus der Konfiguration, die aus geladen wird: der *appsettings.json* Datei. `named_options_2`Werte werden vom bereitgestellt:

* Die `named_options_2` in Delegieren `ConfigureServices` für `Option1`.
* Der Standardwert für `Option2` gebotenen der `MyOptions` Klasse.

Konfigurieren Sie alle Optionen benannte Instanzen mit dem [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) Methode. Der folgende Code konfiguriert `Option1` mit dem Namen der für alle Instanzen der Konfiguration mit einem gemeinsamen Wert. Fügen Sie den folgenden Code manuell die `Configure` Methode:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Ausführen der Beispiel-app nach dem Hinzufügen des Codes führt zu folgendem Ergebnis:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> In ASP.NET Core 2.0 und höher sind alle Optionen benannte Instanzen. Vorhandene `IConfigureOption` Instanzen werden behandelt, als Ziel der `Options.DefaultName` -Instanz, die ist `string.Empty`. `IConfigureNamedOptions`implementiert außerdem `IConfigureOptions`. Die standardmäßige Implementierung des der [IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([Verweisquelle](https://github.com/aspnet/Options/blob/release/2.0.0/src/Microsoft.Extensions.Options/OptionsFactory.cs)) verfügt über eine Logik ordnungsgemäß verwendet. Die `null` benannte Option wird verwendet, um alle benannten Instanzen anstelle einer bestimmten benannten Instanz als Ziel ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) und [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) dieser Konvention verwenden).

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

*Erfordert ASP.NET Core 2.0 oder höher.*

Legen Sie mit postconfiguration [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1). PostConfiguration ausführt, nachdem alle [IConfigureOptions&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) Konfiguration auftritt:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) ist nach dem Konfigurieren der benannten Optionen verfügbar:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Verwendung [PostConfigureAll&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) alle nach dem konfigurieren, mit dem Namen Konfiguration Instanzen:

```csharp
services.PostConfigureAll<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="options-factory-monitoring-and-cache"></a>Optionen-Factory, Überwachung und cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) wird für Benachrichtigungen verwendet, wenn `TOptions` Instanzen ändern. `IOptionsMonitor`reloadable Optionen unterstützt, änderungsbenachrichtigungen und `IPostConfigureOptions`.

[IOptionsFactory&lt;TOptions&gt; ](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) (ASP.NET Core 2.0 oder höher) ist für das Erstellen neuer Instanzen Optionen verantwortlich. Er verfügt über ein einzelnes [erstellen](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create) Methode. Die standardmäßige Implementierung akzeptiert alle registrierten `IConfigureOptions` und `IPostConfigureOptions` und führt alle die zuerst konfiguriert, gefolgt von den nachträglich konfiguriert. Es unterscheidet zwischen `IConfigureNamedOptions` und `IConfigureOptions` und ruft nur die entsprechende Schnittstelle.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) (ASP.NET Core 2.0 or later) is used by `IOptionsMonitor` to cache `TOptions` instances. Die `IOptionsMonitorCache` Optionen Instanzen im Systemmonitor erklärt, sodass der Wert neu berechnet wird ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Werte können manuell eingeleitet werden ebenfalls mit [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd). Die [deaktivieren](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear) Methode wird verwendet, wenn alle benannten Instanzen bei Bedarf neu erstellt werden soll.

## <a name="accessing-options-during-startup"></a>Beim Zugriff auf Optionen während des Starts

`IOptions`kann verwendet werden, `Configure`, da Dienste, bevor erstellt werden die `Configure` Methode ausgeführt wird. Wenn ein Dienstanbieter in integrierten `ConfigureServices` um Optionen zuzugreifen, er wäre nicht enthalten sein Konfigurationen nach der Erstellung des Dienstanbieters bereitgestellten Optionen. Aus diesem Grund kann Zustand inkonsistente Optionen vorhanden, aufgrund der Reihenfolge von Dienst-Registrierungen.

Da Optionen in der Regel aus der Konfiguration geladen werden, kann Konfiguration verwendet werden, in den starttasks in beiden `Configure` und `ConfigureServices`. Beispiele für die Verwendung der Konfiguration während des Starts, finden Sie unter der [Anwendungsstart](xref:fundamentals/startup) Thema.

## <a name="see-also"></a>Siehe auch

* [Konfiguration](xref:fundamentals/configuration/index)
