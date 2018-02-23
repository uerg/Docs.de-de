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
ms.openlocfilehash: 5acae4de56e3f714f0cda2c00d5446d2dcddaf36
ms.sourcegitcommit: 9f758b1550fcae88ab1eb284798a89e6320548a5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/19/2018
---
# <a name="configure-an-aspnet-core-app"></a>Konfigurieren einer ASP.NET Core-App

Von [Rick Anderson](https://twitter.com/RickAndMSFT), [Mark Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27) und [Luke Latham](https://github.com/guardrex)

Mithilfe der Konfigurations-API kann eine ASP.NET Core-Web-App basierend auf einer Liste von Name/Wert-Paaren konfiguriert werden. Die Konfiguration wird zur Runtime aus verschiedenen Quellen gelesen. Name-Wert-Paare können in eine Hierarchie mit mehreren Ebenen gruppiert werden.

Für Folgendes stehen Konfigurationsanbieter zur Verfügung:

* Dateiformate (INI, JSON und XML)
* Befehlszeilenargumente
* Umgebungsvariablen
* Speicherinterne .NET Objekte
* Ein verschlüsselter Benutzerspeicher
* [Azure Key Vault](xref:security/key-vault-configuration)
* Benutzerdefinierte Anbieter (installiert oder erstellt)

Jeder Konfigurationswert ist einem Zeichenfolgenschlüssel zugeordnet. Für die Deserialisierung von Einstellungen in ein benutzerdefiniertes [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object)-Objekt (eine einfache .NET-Klasse mit Eigenschaften) kann auf eine integrierte Bindungsunterstützung zurückgegriffen werden.

Das Optionsmuster stellt anhand von Optionsklassen Gruppen von zusammengehörigen Einstellungen dar. Weitere Informationen zur Verwendung des Optionsmusters finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/index/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="json-configuration"></a>JSON-Konfiguration

Die folgende Konsolen-App verwendet den JSON-Konfigurationsanbieter:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs)]

Die App liest die folgenden Konfigurationseinstellungen und zeigt diese an:

[!code-json[Main](index/sample/ConfigJson/appsettings.json)]

Eine Konfiguration besteht aus einer hierarchischen Liste von Name/Wert-Paaren, in denen die Knoten durch einen Doppelpunkt getrennt sind. Um einen Wert abzurufen, rufen Sie mit dem entsprechenden Schlüssel des Elements den `Configuration`-Indexer auf:

[!code-csharp[Main](index/sample/ConfigJson/Program.cs?range=21-22)]

Verwenden Sie für die Arbeit mit Arrays in JSON-formatierten Konfigurationsquellen einen Arrayindex als Teil der durch Doppelpunkte getrennten Zeichenfolge. Im folgenden Beispiel wird der Name des ersten Elements im vorherigen `wizards`-Array abgerufen:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}");
// Output: Gandalf
```

Name/Wert-Paare, die in die integrierten [Konfigurationsanbieter](/dotnet/api/microsoft.extensions.configuration) geschrieben werden, werden **nicht** beibehalten. Allerdings kann ein benutzerdefinierter Anbieter erstellt werden, der Werte speichert. Weitere Informationen finden Sie im Artikel zum [benutzerdefinierten Konfigurationsanbieter](xref:fundamentals/configuration/index#custom-config-providers).

Im vorhergehenden Beispiel wird der Konfigurationsindexer zum Lesen von Werten verwendet. Um außerhalb von `Startup` auf die Konfiguration zuzugreifen, verwenden Sie das *Optionsmuster*. Weitere Informationen finden Sie im Thema [Optionen](xref:fundamentals/configuration/options).


## <a name="configuration-by-environment"></a>Konfiguration nach Umgebung

Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen (z.B. für Entwicklung, Tests und Produktion) zu verwalten. Durch die Erweiterungsmethode `CreateDefaultBuilder` in einer ASP.NET Core 2.x-App (oder durch die direkte Verwendung von `AddJsonFile` und `AddEnvironmentVariables` in einer ASP.NET Core 1.x-App) werden Konfigurationsanbieter zum Lesen von JSON-Dateien und Systemkonfigurationsquellen hinzugefügt:

* *appsettings.json*
* *appsettings.\<Umgebungsname>.json*
* Umgebungsvariablen

ASP.NET Core 1.x-Apps müssen `AddJsonFile` und [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables#Microsoft_Extensions_Configuration_EnvironmentVariablesExtensions_AddEnvironmentVariables_Microsoft_Extensions_Configuration_IConfigurationBuilder_System_String_) aufrufen.

Eine Erläuterung der Parameter finden Sie unter [AddJsonFile](/dotnet/api/microsoft.extensions.configuration.jsonconfigurationextensions). `reloadOnChange` wird nur in ASP.NET Core 1.1 und höher unterstützt.

Konfigurationsquellen werden in der Reihenfolge, in der sie angegeben sind, gelesen. Im vorangehenden Code werden die Umgebungsvariablen zuletzt gelesen. Alle über die Umgebung festgelegten Konfigurationswerte ersetzen diejenigen in den beiden vorherigen Anbietern.

Beachten Sie die folgende *appsettings.Staging.json*-Datei:

[!code-json[Main](index/sample/appsettings.Staging.json)]

Wenn die Umgebung auf `Staging` festgelegt ist, liest die folgende `Configure`-Methode den Wert von `MyConfig`:

[!code-csharp[Main](index/sample/StartupConfig.cs?name=snippet&highlight=3,4)]


Die Umgebung ist in der Regel auf `Development`, `Staging` oder `Production` festgelegt. Weitere Informationen finden Sie unter [Working with multiple environments (Verwenden von mehreren Umgebungen)](xref:fundamentals/environments).

Überlegungen zur Konfiguration:

* `IOptionsSnapshot` kann Konfigurationsdaten erneut laden, wenn sich diese ändern. Weitere Informationen finden Sie unter [IOptionsSnapshot](xref:fundamentals/configuration/options#reload-configuration-data-with-ioptionssnapshot).
* Bei Konfigurationsschlüsseln wird die Groß-/Kleinschreibung **nicht** beachtet.
* Speichern Sie **niemals** Kennwörter oder andere sensible Daten im Konfigurationsanbietercode oder in Nur-Text-Konfigurationsdateien. Verwenden Sie keine Produktionsgeheimnisse in Entwicklungs- oder Testumgebungen. Geben Sie Geheimnisse außerhalb des Projekts an, damit sie nicht versehentlich in ein Quellcoderepository übernommen werden können. Erfahren Sie mehr zum [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments) und einer [sicheren Speicherung von App-Geheimnissen während der Entwicklung](xref:security/app-secrets).
* Wenn in Umgebungsvariablen in einem System kein Doppelpunkt (`:`) verwendet werden kann, ersetzen Sie den Doppelpunkt (`:`) durch zwei Unterstriche (`__`).

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>In-Memory-Anbieter und Bindung an eine POCO-Klasse

Im folgenden Beispiel wird gezeigt, wie der In-Memory-Anbieter verwendet und an eine Klasse gebunden wird:

[!code-csharp[Main](index/sample/InMemory/Program.cs)]

Konfigurationswerte werden als Zeichenfolgen zurückgegeben, durch eine Bindung wird jedoch die Erstellung von Objekten ermöglicht. Durch eine Bindung können POCO-Objekte oder sogar ganze Objektdiagramme abgerufen werden.

### <a name="getvalue"></a>GetValue

Im folgenden Beispiel wird die Erweiterungsmethode [GetValue&lt;T&gt;](/dotnet/api/microsoft.extensions.configuration.configurationbinder.get?view=aspnetcore-2.0#Microsoft_Extensions_Configuration_ConfigurationBinder_Get__1_Microsoft_Extensions_Configuration_IConfiguration_) gezeigt:

[!code-csharp[Main](index/sample/InMemoryGetValue/Program.cs?highlight=31)]

Durch die `GetValue<T>`-Methode von ConfigurationBinder kann ein Standardwert (hier 80) angegeben werden. `GetValue<T>` ist für einfache Szenarien vorgesehen und kann nicht an gesamte Abschnitte gebunden werden. `GetValue<T>` ruft skalare Werte von `GetSection(key).Value` ab, die in einen bestimmten Typ konvertiert wurden.

## <a name="bind-to-an-object-graph"></a>Binden an ein Objektdiagramm

Jedes Objekt in einer Klasse kann rekursiv gebunden werden. Betrachten wir einmal die folgende `AppSettings`-Klasse:

[!code-csharp[Main](index/sample/ObjectGraph/AppSettings.cs)]

Im folgenden Beispiel erfolgt eine Bindung an die `AppSettings`-Klasse:

[!code-csharp[Main](index/sample/ObjectGraph/Program.cs?highlight=15-16)]

Bei **ASP.NET Core 1.1** und höher kann `Get<T>` für gesamte Abschnitte verwendet werden. `Get<T>` ist eventuell praktischer als `Bind`. Der folgende Code zeigt, wie `Get<T>` in Bezug auf das vorherige Beispiel verwendet wird:

```csharp
var appConfig = config.GetSection("App").Get<AppSettings>();
```

Verwenden Sie die folgende Datei *appsettings.json*:

[!code-json[Main](index/sample/ObjectGraph/appsettings.json)]

Das Programm zeigt `Height 11` an.

Mit dem folgenden Code kann ein Komponententest für die Konfiguration durchgeführt werden:

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

## <a name="create-an-entity-framework-custom-provider"></a>Erstellen eines benutzerdefinierten Entity Framework-Anbieters

In diesem Abschnitt wird ein Standardkonfigurationsanbieter erstellt, der mithilfe des EF Name/Wert-Paare aus einer Datenbank liest.

Definieren Sie eine `ConfigurationValue`-Entität zum Speichern von Konfigurationswerten in der Datenbank:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Fügen Sie eine `ConfigurationContext`-Instanz hinzu, um die konfigurierten Werte zu speichern und auf diese zugreifen:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Erstellen Sie eine Klasse, die [IConfigurationSource](/dotnet/api/Microsoft.Extensions.Configuration.IConfigurationSource) implementiert:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Erstellen Sie den benutzerdefinierten Konfigurationsanbieter durch Vererbung von [ConfigurationProvider](/dotnet/api/Microsoft.Extensions.Configuration.ConfigurationProvider). Der Konfigurationsanbieter initialisiert die Datenbank, wenn diese leer ist:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Bei Ausführung des Beispiels werden die hervorgehobenen Werte von der Datenbank („value_from_ef_1“ und „value_from_ef_2“) angezeigt.

Eine `EFConfigSource`-Erweiterungsmethode zum Hinzufügen der Konfigurationsquelle kann verwendet werden:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Im folgenden Code wird die Verwendung des benutzerdefinierten Anbieters `EFConfigProvider` veranschaulicht:

[!code-csharp[Main](index/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Beachten Sie, dass der benutzerdefinierte Anbieter `EFConfigProvider` im Beispiel nach dem JSON-Anbieter hinzugefügt wird, sodass Datenbankeinstellungen die Einstellungen aus der Datei *appsettings.json* überschreiben.

Verwenden Sie die folgende Datei *appsettings.json*:

[!code-json[Main](index/sample/CustomConfigurationProvider/appsettings.json)]

Die folgende Ausgabe wird angezeigt:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>CommandLine-Konfigurationsanbieter

Der [CommandLine-Konfigurationsanbieter](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) erhält Schlüssel/Wert-Paare der Befehlszeilenargumente für die Konfiguration zur Runtime.

[Beispiel für den CommandLine-Konfigurationsanbieter anzeigen oder herunterladen](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/index/sample/CommandLine)

### <a name="setup-and-use-the-commandline-configuration-provider"></a>Einrichten und Verwenden des CommandLine-Konfigurationsanbieters

# <a name="basic-configurationtabbasicconfiguration"></a>[Standardkonfiguration](#tab/basicconfiguration)

Um die Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine`-Erweiterungsmethode für eine [ConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.configurationbuilder)-Instanz ab:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program.cs?highlight=18,21)]

Nach der Ausführung des Codes wird die folgende Ausgabe angezeigt:

```console
MachineName: MairaPC
Left: 1980
```

Durch die Übergabe von Schlüssel/Wert-Paaren der Argumente in der Befehlszeile werden die Werte von `Profile:MachineName` und `App:MainWindow:Left` geändert:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Das Konsolenfenster wird angezeigt:

```console
MachineName: BartPC
Left: 1979
```

Um die von anderen Konfigurationsanbietern bereitgestellte Konfiguration mit der Befehlszeilenkonfiguration zu überschreiben, rufen Sie die letzte Funktion `AddCommandLine` von `ConfigurationBuilder` auf:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Typische ASP.NET Core 2.x-Apps verwenden für die Erstellung des Hosts die statische Komfortmethode `CreateDefaultBuilder`:

[!code-csharp[Main](index/sample_snapshot//Program.cs?highlight=12)]

`CreateDefaultBuilder` lädt die optionale Konfiguration von *appsettings.json*, *appsettings.{Umgebung}.json*, [Benutzergeheimnisse](xref:security/app-secrets) (in der `Development`-Umgebung), Umgebungsvariablen und Befehlszeilenargumente. Der CommandLine-Konfigurationsanbieter wird zuletzt aufgerufen. Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird.

Für *appsettings*-Dateien, bei denen:

* `reloadOnChange` aktiviert ist.
* die gleiche Einstellung in den Befehlszeilenargumenten sowie eine *appsettings*-Datei enthalten ist.
* die *appsettings*-Datei, die das entsprechende Befehlszeilenargument enthält, nach dem Start der App geändert wird.

Wenn alle oben genannten Bedingungen erfüllt sind, werden die Befehlszeilenargumente überschrieben.

Die ASP.NET Core 2.x-App kann [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) statt `CreateDefaultBuilder` verwenden. Wenn Sie `WebHostBuilder` verwenden, legen Sie die Konfiguration manuell mit [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) fest. Weitere Informationen finden Sie auf der Registerkarte zu ASP.NET Core 1.x.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen Sie einen [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder)-Anbieter, und rufen Sie die `AddCommandLine`-Methode auf, um den CommandLine-Konfigurationsanbieter zu verwenden. Indem der Anbieter zuletzt aufgerufen wird, können die Befehlszeilenargumente zur Runtime übergeben werden, sodass der von den anderen zuvor aufgerufenen Konfigurationsanbietern festgelegte Konfigurationssatz überschrieben wird. Wenden Sie mit der `UseConfiguration`-Methode die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) an:

[!code-csharp[Main](index/sample_snapshot//CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumente

Über die Befehlszeile übergebene Argumente müssen eines der zwei Formate, die in der folgenden Tabelle aufgeführt sind, aufweisen:

| Argumentformat                                                     | Beispiel        |
| ------------------------------------------------------------------- | :------------: |
| Einzelnes Argument: ein durch ein Gleichheitszeichen (`=`) getrenntes Schlüssel/Wert-Paar | `key1=value`   |
| Sequenz der zwei Argumente: ein durch einen Leerraum getrenntes Schlüssel/Wert-Paar    | `/key1 value1` |

**Einzelnes Argument**

Der Wert muss einem Gleichheitszeichen (`=`) nachgestellt sein. Der Wert kann NULL sein (z.B. `mykey=`).

Der Schlüssel darf einen Präfix enthalten.

| Schlüsselpräfix               | Beispiel         |
| ------------------------ | :-------------: |
| Ohne Präfix                | `key1=value1`   |
| Ein Gedankenstrich (`-`)&#8224; | `-key2=value2`  |
| Zwei Gedankenstriche (`--`)        | `--key3=value3` |
| Schrägstrich (`/`)      | `/key4=value4`  |

&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.

Beispielbefehl:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.

**Sequenz aus zwei Argumenten**

Der Wert darf nicht null und muss dem Schlüssel getrennt durch einen Leerraum nachgestellt sein.

Der Schlüssel muss ein Präfix enthalten.

| Schlüsselpräfix               | Beispiel         |
| ------------------------ | :-------------: |
| Ein Gedankenstrich (`-`)&#8224; | `-key1 value1`  |
| Zwei Gedankenstriche (`--`)        | `--key2 value2` |
| Schrägstrich (`/`)      | `/key3 value3`  |

&#8224;Wie nachfolgend beschrieben wird, muss ein Schlüssel mit einem Gedankenstrich (`-`) als Präfix in [Switchmappings](#switch-mappings) angegeben werden.

Beispielbefehl:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Hinweis: Wenn `-key1` nicht in den für einen Konfigurationsanbieter bereitgestellten [Switchmappings](#switch-mappings) vorhanden ist, wird `FormatException` ausgelöst.

### <a name="duplicate-keys"></a>Doppelte Schlüssel

Wenn Schlüssel doppelt angegeben werden, wird das letzte Schlüssel/Wert-Paar verwendet.

### <a name="switch-mappings"></a>Switchmappings

Bei der manuellen Erstellung von Konfigurationen mit `ConfigurationBuilder` kann ein Switchmappingswörterbuch der `AddCommandLine`-Methode hinzugefügt werden. Switchmappings erlauben das Angeben einer Logik zum Ersetzen von Schlüsselnamen.

Wenn das Switchmappingwörterbuch verwendet wird, wird das Wörterbuch für Schlüssel, die dem von einem Befehlszeilenargument angegebenen Schlüssel entsprechen, überprüft. Wenn der Befehlszeilenschlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (der Schlüsselersatz) zur Festlegung der Konfiguration zurückgegeben. Ein Switchmapping ist für jeden Befehlszeilenschlüssel erforderlich, dem ein einzelner Gedankenstrich (`-`) vorangestellt ist.

Regeln für Schlüssel von Switchmappingwörterbüchern:

* Switchmappings müssen mit einem Gedankenstrich (`-`) oder zwei Gedankenstrichen (`--`) beginnen.
* Das Switchmappingwörterbuch darf keine doppelten Schlüssel enthalten.

Im folgenden Beispiel können durch die `GetSwitchMappings`-Methode einzelne Gedankenstriche (`-`) als Schlüsselpräfix in Befehlszeilenargumenten verwendet und führende Unterschlüsselpräfixe vermieden werden.

[!code-csharp[Main](index/sample/CommandLine/Program.cs?highlight=10-19,32)]

Ohne die Angabe von Befehlszeilenargumenten legt das auf `AddInMemoryCollection` festgelegte Wörterbuch die Konfigurationswerte fest. Führen Sie die App mit dem folgenden Befehl aus:

```console
dotnet run
```

Das Konsolenfenster wird angezeigt:

```console
MachineName: RickPC
Left: 1980
```

Verwenden Sie den folgenden Code, um die Konfigurationseinstellungen zu übergeben:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Das Konsolenfenster wird angezeigt:

```console
MachineName: DahliaPC
Left: 1984
```

Das erstellte Switchmappingwörterbuch enthält die in der folgenden Tabelle gezeigten Daten:

| Key            | Wert                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Führen Sie zur Veranschaulichung des Schlüsselwechsels mit dem Wörterbuch den folgenden Befehl aus:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Die Befehlszeilenschlüssel sind ausgetauscht. Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left` an:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="webconfig-file"></a>Datei „web.config“

Eine Datei namens *web.config* ist erforderlich, wenn Sie die App in IIS oder IIS Express hosten. Die Einstellungen in der Datei *web.config* aktivieren das [ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module), um die App zu starten und andere IIS-Einstellungen und -Module zu konfigurieren. Wenn die Datei *web.config* nicht vorhanden ist und die Projektdatei `<Project Sdk="Microsoft.NET.Sdk.Web">` enthält, wird bei der Veröffentlichung des Projekts eine Datei namens *web.config* in der veröffentlichten Ausgabe (dem Ordner *publish*) erstellt. Weitere Informationen finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index#webconfig-file).

## <a name="accessing-configuration-during-startup"></a>Zugriff auf die Konfiguration während des Starts

Beispiele für den Zugriff auf die Konfigurationen in `ConfigureServices` oder `Configure` während des Starts finden Sie im Artikel zum [Start der Anwendung](xref:fundamentals/startup).

## <a name="additional-notes"></a>Zusätzliche Hinweise

* Die Abhängigkeitsinjektion (Dependency Injection, DI) wird erst nach Aufrufen von `ConfigureServices` eingerichtet.
* Das Konfigurationssystem ist nicht DI-fähig.
* `IConfiguration` weist zwei Spezialisierungen auf:
  * `IConfigurationRoot` Wird für den Stammknoten verwendet. Kann einen erneuten Ladevorgang auslösen.
  * `IConfigurationSection` Stellt einen Abschnitt der Konfigurationswerte dar. Die Methoden `GetSection` und `GetChildren` geben `IConfigurationSection` zurück.
  * Verwenden Sie [IConfigurationRoot](/dotnet/api/microsoft.extensions.configuration.iconfigurationroot), wenn Sie Konfigurationen neu laden oder auf jeden Anbieter zugreifen möchten. Keine dieser Situationen tritt häufig auf.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Optionen](xref:fundamentals/configuration/options)
* [Arbeiten mit mehreren Umgebungen](xref:fundamentals/environments)
* [Sicheres Speichern geheimer App-Schlüssel während der Entwicklung](xref:security/app-secrets)
* [Einrichten eines Hosts](xref:fundamentals/hosting)
* [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection)
* [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration)
