---
title: "Erkennen von Änderungen mit Change-Token in ASP.NET Core"
author: guardrex
description: "Informationen Sie zum Ändern von Token verwenden zum Nachverfolgen von Änderungen."
manager: wpickett
ms.author: riande
ms.date: 11/10/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/primitives/change-tokens
ms.openlocfilehash: 94bf356fcbfab3930804485c1b65e4a0f4c52b8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="detect-changes-with-change-tokens-in-aspnet-core"></a>Erkennen von Änderungen mit Change-Token in ASP.NET Core

Von [Luke Latham](https://github.com/guardrex)

Ein *ändern Token* ist ein allgemeines, Low-Level-Baustein zum Nachverfolgen von Änderungen verwendet.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/primitives/change-tokens/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="ichangetoken-interface"></a>IChangeToken-Schnittstelle

[IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken) weitergibt Benachrichtigungen, dass eine Änderung aufgetreten ist. `IChangeToken`befindet sich in der [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) Namespace. Für apps, die nicht die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Metapackage Verweis der [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet-Paket in der Projektdatei.

`IChangeToken`verfügt über zwei Eigenschaften:

* [ActiveChangedCallbacks](/dotnet/api/microsoft.extensions.primitives.ichangetoken.activechangecallbacks) anzugeben, ob das Token proaktiv Rückrufe löst. Wenn `ActiveChangedCallbacks` festgelegt ist, um `false`, ein Rückruf wird nie aufgerufen, und die app abrufen muss `HasChanged` für Änderungen. Es ist auch möglich, dass ein Token, das nie abgebrochen werden, wenn keine Änderungen auftreten, oder die zugrunde liegenden Änderungslistener gelöscht oder deaktiviert ist.
* [HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged) Ruft einen Wert, der angibt, ob eine Änderung aufgetreten ist.

Die Schnittstelle verfügt über eine Methode, [RegisterChangeCallback (Aktion&lt;Objekt&gt;, Objekt)](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback), die registriert eines Rückrufs, der aufgerufen wird, wenn das Token geändert hat. `HasChanged`muss festgelegt werden, bevor der Rückruf aufgerufen wird.

## <a name="changetoken-class"></a>' ChangeToken '-Klasse

`ChangeToken`eine statische Klasse wird zum Benachrichtigungen verteilt wird, dass eine Änderung aufgetreten ist. `ChangeToken`befindet sich in der [Microsoft.Extensions.Primitives](/dotnet/api/microsoft.extensions.primitives) Namespace. Für apps, die nicht die [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) Metapackage Verweis der [Microsoft.Extensions.Primitives](https://www.nuget.org/packages/Microsoft.Extensions.Primitives/) NuGet-Paket in der Projektdatei.

Die `ChangeToken` [OnChange (Func&lt;IChangeToken&gt;, Aktion)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action_) Methode registriert eine `Action` aufrufen, wenn das Token geändert:
* `Func<IChangeToken>`erzeugt das Token an.
* `Action`wird aufgerufen, wenn das Token ändert.

`ChangeToken`verfügt über eine [OnChange&lt;TState&gt;(Func&lt;IChangeToken&gt;, Aktion&lt;TState&gt;, TState)](/dotnet/api/microsoft.extensions.primitives.changetoken.onchange?view=aspnetcore-2.0#Microsoft_Extensions_Primitives_ChangeToken_OnChange__1_System_Func_Microsoft_Extensions_Primitives_IChangeToken__System_Action___0____0_) Überladung mit einem zusätzlichen `TState`Parameter, der in der token Consumer übergeben wird `Action`.

`OnChange`Gibt eine [IDisposable](/dotnet/api/system.idisposable). Aufrufen von [Dispose](/dotnet/api/system.idisposable.dispose) beendet das Token von der Überwachung für weitere Änderungen und gibt das Token Ressourcen frei.

## <a name="example-uses-of-change-tokens-in-aspnet-core"></a>Wird mit der Änderung von Token in ASP.NET Core

Ändern von Token werden in gut sichtbaren Bereiche von ASP.NET Core Überwachen von Änderungen an Objekten verwendet:

* Für die Überwachung von Änderungen an den Dateien, [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)des [Überwachen](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) Methode erstellt eine `IChangeToken` für die angegebenen Dateien oder Ordner überwachen.
* `IChangeToken`Einträge im Cache cacheentfernungen bei Änderung auslösen können Token hinzugefügt werden.
* Für `TOptions` ändert den Standard [OptionsMonitor](/dotnet/api/microsoft.extensions.options.optionsmonitor-1) Implementierung von [IOptionsMonitor](/dotnet/api/microsoft.extensions.options.ioptionsmonitor-1) weist eine Überladung, die eine oder mehrere akzeptiert [IOptionsChangeTokenSource](/dotnet/api/microsoft.extensions.options.ioptionschangetokensource-1)Instanzen. Gibt jede Instanz einer `IChangeToken` ein Rückrufs ändern Tracking Änderungen Optionen zu registrieren.

## <a name="monitoring-for-configuration-changes"></a>Überwachung für Änderungen an der Konfiguration

Standardmäßig verwenden Sie Vorlagen für ASP.NET Core [JSON-Konfigurationsdateien](xref:fundamentals/configuration/index#json-configuration) (*appsettings.json*, *"appSettings". Development.JSON*, und *"appSettings". Production.JSON*) beim Laden der app-Konfigurationseinstellungen.

Diese Dateien werden mithilfe von konfiguriert die [AddJsonFile ("IConfigurationBuilder", "String", "Boolean", "Boolean")](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions.addjsonfile?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_JsonConfigurationExtensions_AddJsonFile_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_System_Boolean_System_Boolean_) Erweiterungsmethode auf [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder) , akzeptiert eine `reloadOnChange` Parameter (ASP.NET Core 1.1 und höher). `reloadOnChange`Gibt an, ob Konfiguration auf Änderungen der Datenbankdatei neu geladen werden soll. Finden Sie diese Einstellung in der [WebHost](/dotnet/api/microsoft.aspnetcore.webhost) Hilfsmethode [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) ([Verweisquelle](https://github.com/aspnet/MetaPackages/blob/rel/2.0.3/src/Microsoft.AspNetCore/WebHost.cs#L152-L193)):

```csharp
config.AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
      .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true, reloadOnChange: true);
```

Dateibasierte Konfiguration dargestellte [FileConfigurationSource](/dotnet/api/microsoft.extensions.configuration.fileconfigurationsource). `FileConfigurationSource`verwendet [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) ([Verweisquelle](https://github.com/aspnet/FileSystem/blob/patch/2.0.1/src/Microsoft.Extensions.FileProviders.Abstractions/IFileProvider.cs)) zum Überwachen von Dateien.

Wird standardmäßig die `IFileMonitor` wird bereitgestellt, indem eine [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) ([Verweisquelle](https://github.com/aspnet/Configuration/blob/patch/2.0.1/src/Microsoft.Extensions.Configuration.FileExtensions/FileConfigurationSource.cs#L82)), verwendet [FileSystemWatcher](/dotnet/api/system.io.filesystemwatcher) zum Überwachen von Konfigurationsdatei ändert.

Die Beispiel-app veranschaulicht zwei Implementierungen für die Überwachung von Änderungen an der Konfiguration. Wenn entweder die *appsettings.json* Datei oder die Umgebung Version der Datei geändert wird, jede Implementierung führt benutzerdefinierten Code. Die Beispiel-app schreibt eine Meldung an die Konsole.

Einer Konfigurationsdatei `FileSystemWatcher` kann mehrere token Rückrufe für eine einzelne Datei konfigurationsänderung ausgelöst werden. Das Beispiel für die Implementierung schützt gegen dieses Problem durch Überprüfen der Dateihashes auf die Konfigurationsdateien. Dateihashes Überprüfung wird sichergestellt, dass mindestens eine der Konfigurationsdateien geändert hat, bevor Sie den benutzerdefinierten Code ausführen. Das Beispiel verwendet SHA1 Datei hashing (*Utilities/Utilities.cs*):

   [!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet1)]

   Eine Wiederholung wird mit einem exponentiellen Backoff-implementiert. Erneut versuchen ist vorhanden, da Sperren von Dateien auftreten können, die vorübergehend das Berechnen eines neuen Hashs auf eine der Dateien.

### <a name="simple-startup-change-token"></a>Simple-starttasks Change-token

Registrieren Sie einen token Consumer `Action` Rückruf für änderungsbenachrichtigungen auf das Token der Konfiguration laden (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet2)]

`config.GetReloadToken()`Gibt das Token an. Der Rückruf der `InvokeChanged` Methode:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet3)]

Die `state` des Rückrufs wird zum Übergeben der `IHostingEnvironment`. Dies ist hilfreich, um zu bestimmen, den richtigen *"appSettings"* JSON-Konfigurationsdatei zu überwachenden *"appSettings".&lt; Umgebung&gt;.json*. Dateihashes werden verwendet, um zu verhindern, dass die `WriteConsole` Anweisung ausgeführt wird, mehrere Male aufgrund mehrerer token Rückrufe, wenn sich die Konfigurationsdatei nur einmal geändert hat.

Dieses System wird so lange wie die app wird ausgeführt und kann nicht deaktiviert werden, indem der Benutzer ausgeführt.

### <a name="monitoring-configuration-changes-as-a-service"></a>Überwachen Änderungen an der Konfiguration als Dienst

Das Beispiel implementiert:

* Grundlegende Start token überwachen.
* Als ein Dienst überwacht.
* Ein Mechanismus zum Aktivieren und Deaktivieren der Überwachung.

Im Beispiel wird ein `IConfigurationMonitor` Schnittstelle (*Extensions/ConfigurationMonitor.cs*):

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet1)]

Der Konstruktor der implementierten Klasse `ConfigurationMonitor`, registriert einen Rückruf für änderungsbenachrichtigungen:

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet2)]

`config.GetReloadToken()`Gibt das Token an. `InvokeChanged`ist die Rückrufmethode. Die `state` in dieser Instanz ist eine Zeichenfolge, die Überwachungsstatus beschreibt. Es werden zwei Eigenschaften verwendet:

* `MonitoringEnabled`Gibt an, ob der Rückruf des benutzerdefinierten Codes ausgeführt werden soll.
* `CurrentState`Beschreibt den aktuellen Überwachungsstatus für die Verwendung in der Benutzeroberfläche.

Die `InvokeChanged` Methode ist vergleichbar mit der früheren Ansatz, außer dass sie:

* Den Code nicht ausgeführt werden, es sei denn, `MonitoringEnabled` ist `true`.
* Legt die `CurrentState` -Eigenschaftszeichenfolge zu einer beschreibenden Meldung an, in dem die Zeit aufgezeichnet, die der Code ausgeführt wurden.
* Anmerkungen zur aktuellen `state` in seiner `WriteConsole` Ausgabe.

[!code-csharp[Main](change-tokens/sample/Extensions/ConfigurationMonitor.cs?name=snippet3)]

Eine Instanz `ConfigurationMonitor` registriert ist, als Dienst in `ConfigureServices` von *Startup.cs*:

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet1)]

Die Indexseite bietet das Benutzersteuerelement über die Konfiguration überwachen. Die Instanz von `IConfigurationMonitor` eingefügt wird, in der `IndexModel`:

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet1)]

Eine Schaltfläche aktiviert und deaktiviert die Überwachung:

[!code-cshtml[Main](change-tokens/sample/Pages/Index.cshtml?range=35)]

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet2)]

Wenn `OnPostStartMonitoring` wird ausgelöst, die Überwachung aktiviert und der aktuelle Status ist deaktiviert. Wenn `OnPostStopMonitoring` wird ausgelöst, die Überwachung ist deaktiviert, und der Zustand ist festgelegt, um widerzuspiegeln, die Überwachung wird nicht ausgeführt.

## <a name="monitoring-cached-file-changes"></a>Wenn es zwischengespeicherte Dateien überwachen

Inhalt der Datei kann mithilfe von zwischengespeicherten in-Memory- [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache). Im Arbeitsspeicher Zwischenspeichern wird beschrieben, der [im Arbeitsspeicher zwischenspeichern](xref:performance/caching/memory) Thema. Ohne zusätzliche Schritte ausführen, z. B. die unten beschriebenen Implementierung *veralteter* (veraltete) Daten aus einem Cache zurückgegeben werden, wenn die Quelldaten ändern.

Ohne Berücksichtigung des Status einer zwischengespeicherten Quelldatei beim Erneuern einer [gleitende Ablaufzeit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) Zeitraum führt zu einer veraltete Daten zwischenzuspeichern. Jede Anforderung für die Daten erneuert das gleitende Ablaufdatum, aber die Datei nie in den Cache geladen wird. Alle app-Funktionen, mit denen der Inhalt der Datei zwischengespeicherte unterliegen möglicherweise veraltete Inhalt empfangen.

Verwenden von Change-Token in einer Datei Zwischenspeichern Szenario wird verhindert, dass veraltete Dateiinhalt im Cache. Die Beispiel-app zeigt eine Implementierung des Ansatzes.

Das Beispiel verwendet `GetFileContent` an:

* Dateiinhalt zurückgibt.
* Implementieren Sie eine Wiederholungsalgorithmus mit exponentiellen Backoff Abdeckung Fälle, in denen eine Dateisperre vorübergehend eine Datei verhindert, dass gelesen wird.

*Utilities/Utilities.cs*:

[!code-csharp[Main](change-tokens/sample/Utilities/Utilities.cs?name=snippet2)]

Ein `FileService` wird erstellt, um die zwischengespeicherte Datei Suchvorgänge zu behandeln. Die `GetFileContent` Methodenaufruf des Diensts versucht, Inhalt von in-Memory-Caches abgerufen und an den Aufrufer zurückgeben (*Services/FileService.cs*).

Wenn mithilfe des Cacheschlüssels zwischengespeicherte Inhalte nicht gefunden wird, werden folgende Aktionen durchgeführt:

1. Der Inhalt der Datei mit abgerufen wird `GetFileContent`.
1. Ein Änderungstoken des Datei-Anbieters mit abgerufen wird [IFileProviders.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch). Das Token Rückruf wird ausgelöst, wenn die Datei geändert wird.
1. Der Inhalt der Datei mit zwischengespeichert eine [gleitende Ablaufzeit](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryoptions.slidingexpiration) Zeitraum. Das Änderungstoken ist mit angefügt [MemoryCacheEntryExtensions.AddExpirationToken](/dotnet/api/microsoft.extensions.caching.memory.memorycacheentryextensions.addexpirationtoken) auf den Cacheeintrag entfernen, wenn die Datei ändert, während diese zwischengespeichert wird.

[!code-csharp[Main](change-tokens/sample/Services/FileService.cs?name=snippet1)]

Die `FileService` ist im Dienstcontainer zusammen mit der Speicher-caching Service registriert (*Startup.cs*):

[!code-csharp[Main](change-tokens/sample/Startup.cs?name=snippet4)]

Die Seitenmodell lädt Inhalt der Datei, die mithilfe des Diensts (*Pages/Index.cshtml.cs*):

[!code-csharp[Main](change-tokens/sample/Pages/Index.cshtml.cs?name=snippet3)]

## <a name="compositechangetoken-class"></a>CompositeChangeToken-Klasse

Für die Darstellung von mindestens `IChangeToken` Instanzen in ein einzelnes Objekt, verwenden die [CompositeChangeToken](/dotnet/api/microsoft.extensions.primitives.compositechangetoken) Klasse ([Verweisquelle](https://github.com/aspnet/Common/blob/patch/2.0.1/src/Microsoft.Extensions.Primitives/CompositeChangeToken.cs)).

```csharp
var firstCancellationTokenSource = new CancellationTokenSource();
var secondCancellationTokenSource = new CancellationTokenSource();

var firstCancellationToken = firstCancellationTokenSource.Token;
var secondCancellationToken = secondCancellationTokenSource.Token;

var firstCancellationChangeToken = new CancellationChangeToken(firstCancellationToken);
var secondCancellationChangeToken = new CancellationChangeToken(secondCancellationToken);

var compositeChangeToken = 
    new CompositeChangeToken(
        new List<IChangeToken> 
        { 
            firstCancellationChangeToken, 
            secondCancellationChangeToken
        });
```

`HasChanged`für die kombinierte token Berichte `true` , wenn alle Token dargestellten `HasChanged` ist `true`. `ActiveChangeCallbacks`für die kombinierte token Berichte `true` , wenn alle Token dargestellten `ActiveChangeCallbacks` ist `true`. Wenn mehrere gleichzeitige Änderungsereignisse auftreten, wird der Rückruf für die kombinierte Änderung genau einmal aufgerufen.

## <a name="see-also"></a>Siehe auch

* [Zwischenspeicherung im Speicher](xref:performance/caching/memory)
* [Arbeiten mit einem verteilten Cache](xref:performance/caching/distributed)
* [Erkennen von Änderungen mit Änderungstoken](xref:fundamentals/primitives/change-tokens)
* [Zwischenspeichern von Antworten](xref:performance/caching/response)
* [Antworten zwischenspeichernde Middleware](xref:performance/caching/middleware)
* [Cache-Taghilfsprogramm](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [Taghilfsprogramm für verteilten Cache](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
