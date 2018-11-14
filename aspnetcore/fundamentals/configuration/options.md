---
title: Optionsmuster in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie Optionsmuster verwenden, um Gruppen von zusammengehörigen Einstellungen in ASP.NET Core-Anwendungen darzustellen.
ms.author: riande
ms.custom: mvc
ms.date: 11/09/2018
uid: fundamentals/configuration/options
ms.openlocfilehash: 99aa5028a8704c7e9e3010415137e2560213a2ad
ms.sourcegitcommit: edb9d2d78c9a4d68b397e74ae2aff088b325a143
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "51505792"
---
# <a name="options-pattern-in-aspnet-core"></a>Optionsmuster in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Das Optionsmuster verwendet Klassen, um Gruppen von zusammengehörigen Einstellungen darzustellen. Wenn [Konfigurationseinstellungen](xref:fundamentals/configuration/index) nach Szenario in separate Klassen isoliert werden, entspricht die Anwendung zwei wichtigen Prinzipien der Softwareentwicklung:

* Das [Schnittstellentrennungsprinzip (ISP)](http://deviq.com/interface-segregation-principle/): Szenarios (Klassen), die von Konfigurationseinstellungen abhängen, sind nur von den Konfigurationseinstellungen abhängig, die sie verwenden.
* [Trennung von Bereichen](http://deviq.com/separation-of-concerns/): Einstellungen für die verschiedenen Teile der Anwendung hängen nicht voneinander ab und sind nicht gekoppelt.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample)) Dieser Artikel kann mit der Beispielanwendung einfacher gelesen werden.

## <a name="prerequisites"></a>Erforderliche Komponenten

::: moniker range=">= aspnetcore-2.1"

Verweisen Sie auf das [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app), oder fügen Sie einen Paketverweis auf das Paket [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) hinzu.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Verweisen Sie auf das [Microsoft.AspNetCore.All-Metapaket](xref:fundamentals/metapackage), oder fügen Sie einen Paketverweis auf das Paket [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) hinzu.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Fügen Sie einen Paketverweis auf das Paket [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/) hinzu.

::: moniker-end

## <a name="basic-options-configuration"></a>Grundlegende Optionskonfiguration

Grundlegende Optionskonfiguration wird als Beispiel &num;1 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

Eine Optionsklasse muss nicht abstrakt sein und über einen öffentlichen parameterlosen Konstruktor verfügen. Die folgende Klasse `MyOptions` verfügt über die zwei Eigenschaften: `Option1` und `Option2`. Das Festlegen von Standardwerten ist optional, aber der Klassenkonstruktor im folgenden Beispiel legt den Standardwert von `Option1` fest. `Option2` hat den Standardwert festgelegt, indem die Eigenschaft direkt initialisiert wurde (*Models/MyOptions.cs*):

[!code-csharp[](options/sample/Models/MyOptions.cs?name=snippet1)]

Die `MyOptions`-Klasse wird zum Dienstcontainer mit [Configure&lt;TOptions&gt;(IServiceCollection, IConfiguration)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsconfigurationservicecollectionextensions.configure#Microsoft_Extensions_DependencyInjection_OptionsConfigurationServiceCollectionExtensions_Configure__1_Microsoft_Extensions_DependencyInjection_IServiceCollection_Microsoft_Extensions_Configuration_IConfiguration_) hinzugefügt und an die Konfiguration gebunden:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example1)]

Das folgende Seitenmodel verwendet [konstruktorbasierte Dependency Injection](xref:mvc/controllers/dependency-injection) mit [IOptions&lt;TOptions&gt;](/dotnet/api/Microsoft.Extensions.Options.IOptions-1), um auf die Einstellungen zugreifen zu können (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example1)]

Die *appsettings.json*-Datei des Beispiels gibt Werte für `option1` und `option2` an:

[!code-json[](options/sample/appsettings.json?highlight=2-3)]

Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode des Seitenmodells eine Zeichenfolge zurück, die die Werte der Optionsklasse anzeigt:

```html
option1 = value1_from_json, option2 = -1
```

> [!NOTE]
> Wenn Sie eine benutzerdefinierte [ConfigurationBuilder](/dotnet/api/system.configuration.configurationbuilder)-Klasse verwenden, um Konfigurationsoptionen aus einer Einstellungsdatei zu laden, bestätigen Sie, dass der Basispfad ordnungsgemäß festgelegt ist:
>
> ```csharp
> var configBuilder = new ConfigurationBuilder()
>    .SetBasePath(Directory.GetCurrentDirectory())
>    .AddJsonFile("appsettings.json", optional: true);
> var config = configBuilder.Build();
>
> services.Configure<MyOptions>(config);
> ```
>
> Sie müssen den Basispfad nicht explizit festlegen, wenn Sie Konfigurationsoptionen über [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) aus der Einstellungsdatei laden.

## <a name="configure-simple-options-with-a-delegate"></a>Konfigurieren von einfachen Optionen mit einem Delegaten

Das Konfigurieren von einfachen Optionen mit einem Delegaten wird als Beispiel &num;2 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

Verwenden Sie einen Delegaten zum Festlegen von Optionswerten. Die Beispielanwendung verwendet die `MyOptionsWithDelegateConfig`-Klasse (*Models/MyOptionsWithDelegateConfig.cs*):

[!code-csharp[](options/sample/Models/MyOptionsWithDelegateConfig.cs?name=snippet1)]

Im folgenden Code wird ein zweiter `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzugefügt. Er verwendet einen Delegaten zum Konfigurieren der Bindung mit `MyOptionsWithDelegateConfig`:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example2)]

*Index.cshtml.cs*:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=3,9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example2)]

Sie können mehrere Konfigurationsanbieter hinzufügen. Konfigurationsanbieter stehen in NuGet-Paketen zur Verfügung. Sie werden in der Reihenfolge ihrer Registrierung angewendet.

Jeder Aufruf von [Configure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1.configure) fügt einen `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzu. Im vorherigen Beispiel wurden die Werte von `Option1` und `Option2` beide in *appsettings.json* angegeben, aber die Werte von `Option1` und `Option2` werden vom konfigurierten Delegaten überschrieben.

Wenn mehr als ein Konfigurationsdienst aktiviert ist, *gewinnt* die letzte angegebene Konfigurationsquelle und legt den Konfigurationswert fest. Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode des Seitenmodells eine Zeichenfolge zurück, die die Werte der Optionsklasse anzeigt:

```html
delegate_option1 = value1_configured_by_delgate, delegate_option2 = 500
```

## <a name="suboptions-configuration"></a>Konfigurieren von Unteroptionen

Das Konfigurieren von Unteroptionen wird als Beispiel &num;3 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

Anwendungen sollten Optionsklassen erstellen, die für bestimmte Szenariogruppen (Klassen) in der Anwendung gelten. Die Komponenten der Anwendung, die Konfigurationswerte erfordern, sollten nur über Zugriff auf die Konfigurationswerte verfügen, die sie verwenden.

Beim Binden von Optionen zur Konfiguration ist jede Eigenschaft im Optionstyp an einen Konfigurationsschlüssel des `property[:sub-property:]`-Formulars gebunden. Die `MyOptions.Option1`-Eigenschaft ist beispielsweise an den Schlüssel `Option1` gebunden, der von der `option1`-Eigenschaft in *appsettings.json* gelesen wird.

Im folgenden Code wird ein dritter `IConfigureOptions<TOptions>`-Dienst zum Dienstcontainer hinzugefügt. Er verbindet `MySubOptions` mit dem `subsection`-Abschnitt der *appSettings.json*-Datei:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example3)]

Die `GetSection`-Erweiterungsmethode erfordert das [Microsoft.Extensions.Options.ConfigurationExtensions](https://www.nuget.org/packages/Microsoft.Extensions.Options.ConfigurationExtensions/)-NuGet-Paket. Wenn die Anwendung das [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app)-Metapaket (ASP.NET Core 2.1 oder höher) verwendet, ist das Paket automatisch eingeschlossen.

Die *appsettings.json*-Datei des Beispiels definiert ein `subsection`-Element mit Schlüsseln für `suboption1` und `suboption2`:

[!code-json[](options/sample/appsettings.json?highlight=4-7)]

Die `MySubOptions`-Klasse definiert die Eigenschaften `SubOption1` und `SubOption2` zur Aufnahme der Optionswerte (*Models/MySubOptions.cs*):

[!code-csharp[](options/sample/Models/MySubOptions.cs?name=snippet1)]

Die `OnGet`-Methode des Seitenmodells gibt eine Zeichenfolge mit den Optionswerten (*Pages/Index.cshtml.cs*) zurück:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=4,10)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example3)]

Wenn die Anwendung ausgeführt wird, gibt die `OnGet`-Methode eine Zeichenfolge zurück, die die Werte der untergeordneten Optionsklasse anzeigt:

```html
subOption1 = subvalue1_from_json, subOption2 = 200
```

## <a name="options-provided-by-a-view-model-or-with-direct-view-injection"></a>Optionen, die durch ein Ansichtsmodell oder mit direkter View Injection bereitgestellt werden

Optionen, die durch ein Ansichtsmodell oder mit direkter View Injection bereitgestellt werden, werden im Beispiel &num;4 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

Optionen können in einem Ansichtsmodell oder durch direkte Injection von `IOptions<TOptions>` in eine Ansicht (*Pages/Index.cshtml.cs*) angegeben werden:

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=9)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=2,8)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example4)]

Fügen Sie für die direkte Injection `IOptions<MyOptions>` mit einer `@inject`-Anweisung ein:

[!code-cshtml[](options/sample/Pages/Index.cshtml?range=1-10&highlight=5)]

Wenn die App ausgeführt wird, werden die Optionswerte in der gerenderten Seite angezeigt:

![Optionswerte Option1: value1_from_json und Option2: -1 werden aus dem Modell geladen und in die Ansicht eingefügt.](options/_static/view.png)

::: moniker range=">= aspnetcore-1.1"

## <a name="reload-configuration-data-with-ioptionssnapshot"></a>Neuladen der Konfigurationsdaten mit IOptionsSnapshot

Das Neuladen der Konfigurationsdaten mit `IOptionsSnapshot` wird im Beispiel &num;5 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

[IOptionsSnapshot](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1) unterstützt das Neuladen von Optionen mit minimalem Verarbeitungsaufwand.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Optionen werden einmal pro Anforderung berechnet. Dies geschieht, wenn auf sie zugegriffen wird und sie für die Dauer der Anforderung zwischengespeichert werden.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

`IOptionsSnapshot` ist eine Momentaufnahme von [IOptionsMonitor&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) und wird automatisch aktualisiert, wenn der Monitor auf der Grundlage sich ändernder Datenquellen Änderungen auslöst.

::: moniker-end

::: moniker range=">= aspnetcore-1.1"

Im folgenden Beispiel wird veranschaulicht, wie eine neue `IOptionsSnapshot` nach den *appsettings.json*-Veränderungen erstellt wird (*Pages/Index.cshtml.cs*). Mehrere Anforderungen an den Server geben konstante Werte zurück, die von der *appsettings.json*-Datei bereitgestellt werden, bis die Datei geändert wird und die Konfiguration neu lädt.

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=12)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=5,11)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example5)]

Die folgende Abbildung zeigt die anfänglichen `option1`- und `option2`-Werte, die aus der *appsettings.json*-Datei geladen werden:

```html
snapshot option1 = value1_from_json, snapshot option2 = -1
```

Ändern Sie die Werte in der *appsettings.json*-Datei auf `value1_from_json UPDATED` und `200`. Speichern Sie die *appsettings.json*-Datei. Aktualisieren Sie den Browser, um festzustellen, ob die Optionswerte aktualisiert wurden:

```html
snapshot option1 = value1_from_json UPDATED, snapshot option2 = 200
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="named-options-support-with-iconfigurenamedoptions"></a>Unterstützung für benannte Optionen mit IConfigureNamedOptions

Unterstützung für benannte Optionen mit [IConfigureNamedOptions](/dotnet/api/microsoft.extensions.options.iconfigurenamedoptions-1) wird als Beispiel &num;6 in der [Beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/options/sample) veranschaulicht.

Die Unterstützung für *benannte Optionen* ermöglicht es der Anwendung, zwischen den Konfigurationen benannter Optionen zu unterscheiden. In der Beispiel-App werden benannte Optionen mit [OptionsServiceCollectionExtensions.Configure&lt;TOptions&gt;(IServiceCollection, String, Action&lt;TOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configure) deklariert. Dadurch wird wiederum die Erweiterungsmethode [ConfigureNamedOptions&lt;TOptions&gt;.Configure](/dotnet/api/microsoft.extensions.options.configurenamedoptions-1.configure) aufgerufen:

[!code-csharp[](options/sample/Startup.cs?name=snippet_Example6)]

Die Beispielanwendung greift mit [IOptionsSnapshot&lt;TOptions&gt;.Get](/dotnet/api/microsoft.extensions.options.ioptionssnapshot-1.get) auf die benannten Optionen zu (*Pages/Index.cshtml.cs*):

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?range=13-14)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet2&highlight=6,12-13)]

[!code-csharp[](options/sample/Pages/Index.cshtml.cs?name=snippet_Example6)]

Bei der Ausführung der Beispielanwendung werden die benannten Optionen zurückgegeben:

```html
named_options_1: option1 = value1_from_json, option2 = -1
named_options_2: option1 = named_options_2_value1_from_action, option2 = 5
```

`named_options_1`-Werte werden von der Konfiguration bereitgestellt. Sie werden aus der *appsettings.json*-Datei geladen. `named_options_2`-Werte werden bereitgestellt vom:

* `named_options_2`-Delegat in `ConfigureServices` für `Option1`.
* Der Standardwert für `Option2` wird von der `MyOptions`-Klasse bereitgestellt.

## <a name="configure-all-options-with-the-configureall-method"></a>Konfigurieren aller Optionen mit der ConfigureAll-Methode

Konfigurieren Sie alle Optionsinstanzen mit der [OptionsServiceCollectionExtensions.ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall)-Methode. Der folgende Code konfiguriert `Option1` für alle Konfigurationsinstanzen mit einem gemeinsamen Wert. Fügen Sie der `ConfigureServices`-Methode manuell den folgenden Code hinzu:

```csharp
services.ConfigureAll<MyOptions>(myOptions => 
{
    myOptions.Option1 = "ConfigureAll replacement value";
});
```

Das Ausführen der Beispielanwendung nach dem Hinzufügen des Codes führt zu folgendem Ergebnis:

```html
named_options_1: option1 = ConfigureAll replacement value, option2 = -1
named_options_2: option1 = ConfigureAll replacement value, option2 = 5
```

> [!NOTE]
> Alle Optionen sind benannte Instanzen. Vorhandene `IConfigureOption`-Instanzen werden behandelt, als ob sie auf die `Options.DefaultName`-Instanz abzielen. Diese ist `string.Empty`. `IConfigureNamedOptions` implementiert auch `IConfigureOptions`. Die Standardimplementierung von [IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ([Verweisquelle](https://github.com/aspnet/Options/blob/release/2.0/src/Microsoft.Extensions.Options/IOptionsFactory.cs)) verfügt über eine Logik zur ordnungsgemäßen Anwendung. Die benannte Option `null` wird verwendet, um auf alle benannten Instanzen anstelle einer bestimmten benannten Instanz abzuzielen ([ConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.configureall) und [PostConfigureAll](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) verwenden diese Konvention).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="optionsbuilder-api"></a>OptionsBuilder-API

<xref:Microsoft.Extensions.Options.OptionsBuilder`1> dient zum Konfigurieren von `TOptions`-Instanzen. `OptionsBuilder` optimiert die Erstellung von benannten Optionen, da es sich nur um einen einzelnen Parameter für den ersten `AddOptions<TOptions>(string optionsName)`-Aufruf handelt statt um alle nachfolgenden Aufrufe. Die Optionsvalidierung und die `ConfigureOptions`-Überladungen, die Dienstabhängigkeiten akzeptieren, sind nur über `OptionsBuilder` verfügbar.

```csharp
// Options.DefaultName = "" is used.
services.AddOptions<MyOptions>().Configure(o => o.Property = "default");
    
services.AddOptions<MyOptions>("optionalName")
    .Configure(o => o.Property = "named");
```

## <a name="configurelttoptions-tdep1--tdep4gt-method"></a>Konfigurieren der Methode &lt;TOptions, TDep1, ... TDep4&gt;

Die Konfiguration von Optionen mit Diensten von DI durch die Implementierung von `IConfigure[Named]Options` als Baustein ist ausführlich. Überladungen für `ConfigureOptions` in `OptionsBuilder<TOptions>` ermöglichen es Ihnen, Optionen mit bis zu fünf Diensten zu konfigurieren:

```csharp
services.AddOptions<MyOptions>("optionalName")
    .Configure<Service1, Service2, Service3, Service4, Service5>(
        (o, s, s2, s3, s4, s5) => 
            o.Property = DoSomethingWith(s, s2, s3, s4, s5));
```

Die Überladung registriert eine vorübergehende generische <xref:Microsoft.Extensions.Options.IConfigureNamedOptions`1>, die über einen Konstruktor verfügt, der die angegebenen generischen Diensttypen akzeptiert. 

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="options-validation"></a>Überprüfung von Optionen

Sie können Optionen überprüfen, wenn diese konfiguriert werden. Rufen Sie `Validate` mit einer Überprüfungsmethode auf, die `true` zurückgibt, wenn die Optionen gültig sind, und `false` zurückgibt, wenn sie es nicht sind:

```csharp
// Registration
services.AddOptions<MyOptions>("optionalOptionsName")
    .Configure(o => { }) // Configure the options
    .Validate(o => YourValidationShouldReturnTrueIfValid(o), 
        "custom error");
        
// Consumption
var monitor = services.BuildServiceProvider()
    .GetService<IOptionsMonitor<MyOptions>>();
  
try
{
    var options = monitor.Get("optionalOptionsName");
} 
catch (OptionsValidationException e) 
{
   // e.OptionsName returns "optionalOptionsName"
   // e.OptionsType returns typeof(MyOptions)
   // e.Failures returns a list of errors, which would contain 
   //     "custom error"
}
```

Im vorherigen Beispiel wurde die benannte Optionsinstanz auf `optionalOptionsName` festgelegt. Die Standardoptionsinstanz ist `Options.DefaultName`.

Überprüfungen werden ausgeführt, wenn die Optionsinstanz erstellt wird. Ihre Optionsinstanz besteht die Überprüfung beim ersten Zugriff garantiert.

> [!IMPORTANT]
> Die Überprüfung von Optionen schützt nicht davor, dass Optionen nach der ersten Konfiguration und Überprüfung geändert werden.

Die `Validate`-Methode akzeptiert ein `Func<TOptions, bool>`-Element. Um die Überprüfung vollständig anzupassen, implementieren Sie `IValidateOptions<TOptions>`. Dies ermöglicht Folgendes:

* Eine Überprüfung von mehreren Optionstypen: `class ValidateTwo : IValidateOptions<Option1>, IValidationOptions<Option2>`
* Eine Überprüfung, die von einem anderen Optionstyp abhängig ist: `public DependsOnAnotherOptionValidator(IOptions<AnotherOption> options)`

`IValidateOptions` validiert Folgendes:

* Eine bestimmte benannte Optionsinstanz.
* Alle Optionen, wenn `name` den Wert `null` aufweist.

Geben Sie ein `ValidateOptionsResult` aus Ihrer Implementierung der Schnittstelle zurück:

```csharp
public interface IValidateOptions<TOptions> where TOptions : class
{
    ValidateOptionsResult Validate(string name, TOptions options);
}
```

Die Überprüfung auf Basis von Datenanmerkungen ist über das Paket [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) durch Aufrufen der `ValidateDataAnnotations`-Methode in `OptionsBuilder<TOptions>` verfügbar:

```csharp
private class AnnotatedOptions
{
    [Required]
    public string Required { get; set; }

    [StringLength(5, ErrorMessage = "Too long.")]
    public string StringLength { get; set; }

    [Range(-5, 5, ErrorMessage = "Out of range.")]
    public int IntRange { get; set; }
}
    
[Fact]
public void CanValidateDataAnnotations()
{
    var services = new ServiceCollection();
    services.AddOptions<AnnotatedOptions>()
        .Configure(o =>
        {
            o.StringLength = "111111";
            o.IntRange = 10;
            o.Custom = "nowhere";
        })
        .ValidateDataAnnotations();

    var sp = services.BuildServiceProvider();

    var error = Assert.Throws<OptionsValidationException>(() => 
        sp.GetRequiredService<IOptions<AnnotatedOptions>>().Value);
    ValidateFailure<AnnotatedOptions>(error, Options.DefaultName, 1,
        "DataAnnotation validation failed for members Required " +
            "with the error 'The Required field is required.'.",
        "DataAnnotation validation failed for members StringLength " +
            "with the error 'Too long.'.",
        "DataAnnotation validation failed for members IntRange " +
            "with the error 'Out of range.'.");
}    
```

Die vorzeitige Überprüfung (Fail-fast beim Start) wird für ein späteres Release in Erwägung.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

## <a name="ipostconfigureoptions"></a>IPostConfigureOptions

Legen Sie die Postkonfiguration mit [IPostConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1) fest. Die Postkonfiguration wird nach der [IConfigureOptions&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1)-Konfiguration durchgeführt:

```csharp
services.PostConfigure<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

[PostConfigure&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ipostconfigureoptions-1.postconfigure) ist nach dem Konfigurieren der benannten Optionen verfügbar:

```csharp
services.PostConfigure<MyOptions>("named_options_1", myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

Verwenden Sie [PostConfigureAll&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.dependencyinjection.optionsservicecollectionextensions.postconfigureall) zur Postkonfiguration aller Konfigurationsinstanzen:

```csharp
services.PostConfigureAll<MyOptions>(myOptions =>
{
    myOptions.Option1 = "post_configured_option1_value";
});
```

::: moniker-end

## <a name="options-factory-monitoring-and-cache"></a>Optionsfactory, Überwachung und Cache

[IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) wird für Benachrichtigungen verwendet, wenn sich die `TOptions`-Instanzen verändern. `IOptionsMonitor` unterstützt neu ladbare Optionen, Änderungsbenachrichtigungen und `IPostConfigureOptions`.

::: moniker range=">= aspnetcore-2.0"

[IOptionsFactory&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1) ist für das Erstellen neuer Optionsinstanzen zuständig. Es verfügt über eine einzelne [Create](/dotnet/api/microsoft.extensions.options.ioptionsfactory-1.create)-Methode. Die Standardimplementierung akzeptiert alle registrierten `IConfigureOptions` und `IPostConfigureOptions` und führt alle Konfigurationen zuerst und die Postkonfigurationen danach aus. Es wird zwischen `IConfigureNamedOptions` und `IConfigureOptions` unterschieden, und es werden nur die entsprechenden Schnittstellen aufgerufen.

[IOptionsMonitorCache&lt;TOptions&gt;](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1) wird von `IOptionsMonitor` zum Zwischenspeichern von `TOptions`-Instanzen verwendet. `IOptionsMonitorCache` erklärt Optionsinstanzen im Monitor ungültig, sodass der Wert neu berechnet wird ([TryRemove](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryremove)). Werte können manuell oder mit [TryAdd](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.tryadd) eingeführt werden. Die [Clear](/dotnet/api/microsoft.extensions.options.ioptionsmonitorcache-1.clear)-Methode wird verwendet, wenn alle benannten Instanzen bei Bedarf neu erstellt werden sollen.

::: moniker-end

## <a name="accessing-options-during-startup"></a>Zugreifen auf Optionen während des Starts

`IOptions` kann in `Startup.Configure` verwendet werden, da Dienste erstellt werden, bevor die `Configure`-Methode ausgeführt wird.

```csharp
public void Configure(IApplicationBuilder app, IOptions<MyOptions> optionsAccessor)
{
    var option1 = optionsAccessor.Value.Option1;
}
```

`IOptions` sollte nicht in `Startup.ConfigureServices` verwendet werden. Es kann einen inkonsistenten Optionszustand geben. Dies liegt an der Reihenfolge der Dienstregistrierungen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/configuration/index>
