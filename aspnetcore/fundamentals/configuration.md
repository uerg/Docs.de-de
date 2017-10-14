---
title: Konfiguration in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Konfigurations-API verwenden, um eine ASP.NET Core-app aus mehreren Quellen zu konfigurieren.
keywords: ASP.NET Core, Konfiguration, JSON, Konfiguration
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: d626768fe1a485705e104a5c758cbdb0b46685a3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="configuration-in-aspnet-core"></a>Konfiguration in ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [Markierung Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), [Daniel Roth](https://github.com/danroth27), und [Luke Latham](https://github.com/guardrex)

Der Konfigurations-API bietet eine Möglichkeit zum Konfigurieren von einer app basierend auf eine Liste von Name / Wert-Paaren. Konfiguration wird zur Laufzeit aus mehreren Quellen gelesen werden. Die Name-Wert-Paare können in einer Hierarchie mit mehreren Ebenen gruppiert werden. Es gibt Konfigurationsanbieter für ein:

* Dateiformate (INI, JSON und XML)
* Befehlszeilenargumente
* Umgebungsvariablen
* Die .NET Objekte im Arbeitsspeicher
* Eine verschlüsselte Benutzerspeicher
* [Azure Key Vault.](xref:security/key-vault-configuration)
* Benutzerdefinierte Anbieter, die Sie installieren, oder erstellen

Jede Konfigurationswert ordnet einen Zeichenfolgenschlüssel. Integrierte Bindung unterstützt beim Deserialisieren von Einstellungen in einer benutzerdefinierten [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) Objekt (eine einfache .NET Klasse mit Eigenschaften).

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="simple-configuration"></a>Einfache Konfiguration

Die folgenden Konsolen-app verwendet die JSON-Konfigurationsanbieter:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

Die Anwendung liest und zeigt die folgenden Konfigurationseinstellungen vornehmen:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Konfiguration besteht eine hierarchische Liste von Name / Wert-Paaren, die in denen die Knoten durch einen Doppelpunkt getrennt sind. Um einen Wert abzurufen, Zugriff auf die `Configuration` einen Indexer mit dem entsprechenden Schlüssel des Elements:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Verwenden Sie zum Arbeiten mit Arrays in JSON-formatierte Konfigurationsquellen einem Arrayindex als Teil der Doppelpunkt getrennte Zeichenfolge ein. Im folgenden Beispiel wird der Name des ersten Elements in den vorherigen `wizards` Array:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Name-Wert-Paare, die in der integrierten geschrieben in `Configuration` Anbieter sind **nicht** beibehalten, Sie können jedoch erstellen ein benutzerdefiniertes Anbieters, der Werte speichert. Finden Sie unter [standardkonfigurationsanbieter](xref:fundamentals/configuration#custom-config-providers).

Das vorhergehende Beispiel verwendet den Indexer für die Konfiguration, um Werte zu lesen. Auf die Konfiguration außerhalb von `Startup`, verwenden Sie die [Optionen Muster](xref:fundamentals/configuration#options-config-objects). Die *Optionen Muster* wird weiter unten in diesem Artikel dargestellt.

Es ist üblich, verschiedene Konfigurationseinstellungen für unterschiedliche Umgebungen, z. B. Entwicklungs-, Test- und produktionsumgebung aufweisen. Die `CreateDefaultBuilder` Erweiterungsmethode in einer ASP.NET Core 2.x-app (oder mit `AddJsonFile` und `AddEnvironmentVariables` direkt in einer ASP.NET Core 1.x-app) Konfigurationsanbieter für das Lesen von JSON-Dateien und das System Konfigurationsquellen hinzugefügt:

* *appsettings.json*
* *"appSettings". \<EnvironmentName > JSON*
* Umgebungsvariablen

Finden Sie unter [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) eine Erläuterung der Parameter. `reloadOnChange`wird nur in ASP.NET Core 1.1 und höher unterstützt. 

Konfigurationsquellen werden in der Reihenfolge gelesen, die sie angegeben sind. Im obigen Code werden die Umgebungsvariablen für die zuletzt gelesen. Alle Konfigurationswerte, die durch die Umgebung eingerichtet würde er in den beiden vorherigen Anbietern ersetzen.

Die Umgebung wird auf einen der in der Regel festgelegt `Development`, `Staging`, oder `Production`. Finden Sie unter [arbeiten mit mehreren Umgebungen](environments.md) für Weitere Informationen.

Überlegungen zur Konfiguration:

* `IOptionsSnapshot`Konfigurationsdaten können erneut laden, wenn sich ändert. Verwendung `IOptionsSnapshot` Konfigurationsdaten werden erneut geladen werden sollen.  Finden Sie unter [IOptionsSnapshot](#ioptionssnapshot) für Weitere Informationen.
* -Konfigurationsschlüssel werden Groß-/Kleinschreibung.
* Eine bewährte Methode ist Umgebungsvariablen letzten, an, sodass die lokale Umgebung, alles in bereitgestellten Konfigurationsdateien festlegen überschreiben kann.
* **Nie** Kennwörter oder andere vertraulichen Daten im Anbietercode Konfiguration oder in Konfigurationsdateien als nur-Text gespeichert. Keine Produktion geheime Schlüssel in der Entwicklung verwenden, oder Umgebungen zu testen. Geben Sie stattdessen geheime Schlüssel außerhalb der Projektstruktur auf, damit sie in das Repository nicht versehentlich eingetragen werden können. Erfahren Sie mehr über [arbeiten mit mehreren Umgebungen](environments.md) und Verwalten von [sichere Speicherung von app-Kennwörter während der Entwicklung](../security/app-secrets.md).
* Wenn `:` nicht ersetzen in Umgebungsvariablen verwendet, in Ihrem System, `:` mit `__` (doppelter Unterstrich).

<a name="options-config-objects"></a>

## <a name="using-options-and-configuration-objects"></a>Mithilfe der Optionen und Konfigurationsobjekten

Das Muster Optionen verwendet benutzerdefinierte Optionsklassen, um eine Gruppe von verwandten Einstellungen darstellen. Es wird empfohlen, entkoppelte Klassen für jede Funktion in Ihrer app zu erstellen. Entkoppelte Klassen entsprechen:

* Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/) : Klassen richten sich nur auf den Konfigurationseinstellungen, die sie verwenden.
* [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) : Einstellungen für die verschiedenen Teile Ihrer App sind nicht abhängige oder gekoppelten miteinander.

Die Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor. Zum Beispiel:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name="options-example"></a>

Im folgenden Code wird die JSON-Konfigurationsanbieter aktiviert. Die `MyOptions` Klasse dem Dienstcontainer hinzugefügt und an Configuration gebunden ist.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

Die folgenden [Controller](../mvc/controllers/index.md) verwendet [Konstruktor Abhängigkeitsinjektion](xref:fundamentals/dependency-injection#what-is-dependency-injection) auf [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) auf Einstellungen zugreifen:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Mit den folgenden *appsettings.json* Datei:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

Die `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_json, option2 = 2`.

Standard-apps wird nicht die gesamte Konfiguration in eine Optionsdatei für die einzelnen gebunden werden. Später zeige ich wie `GetSection` zu einem Abschnitt binden.

Im folgenden Code wird eine zweite `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird. Er verwendet ein Delegat zum Konfigurieren der Bindung mit `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

Sie können mehrere Konfigurationsanbieter hinzufügen. Konfigurationsanbieter stehen im NuGet-Pakete zur Verfügung. Sie werden in Reihenfolge angewendet, die sie registriert sind.

Jeder Aufruf von `Configure<TOptions>` Fügt eine `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer. Im vorherigen Beispiel, die Werte der `Option1` und `Option2` werden in angegebenen *appsettings.json* –, aber der Wert des `Option1` durch den konfigurierten Delegaten überschrieben wird. 

Wenn mehr als ein Dienst aktiviert ist, die letzte Konfigurationsquelle angegeben "gewinnt" (setzt den Konfigurationswert). Im vorangehenden Code der `HomeController.Index` -Methode zurückkehrt `option1 = value1_from_action, option2 = 2`.

Wenn Sie Optionen zur Konfiguration binden, jede Eigenschaft im Optionstyp Ihres einem Konfigurationsschlüssel des Formulars gebunden ist `property[:sub-property:]`. Z. B. die `MyOptions.Option1` Eigenschaft gebunden ist, auf den Schlüssel `Option1`, das Auslesen der `option1` Eigenschaft im *appsettings.json*. Ein Beispiel für die untergeordnete Eigenschaft ist später in diesem Artikel dargestellt.

Im folgenden Code ein drittes `IConfigureOptions<TOptions>` Dienst dem Dienstcontainer hinzugefügt wird. Sie bindet `MySubOptions` Abschnitt `subsection` von der *appsettings.json* Datei:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Hinweis: Diese Erweiterungsmethode erfordert die `Microsoft.Extensions.Options.ConfigurationExtensions` NuGet-Paket.

Mit den folgenden *appsettings.json* Datei:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

Die `MySubOptions` Klasse:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Mit den folgenden `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`wird zurückgegeben.

Sie können auch angeben von Optionen in einem Ansichtsmodell oder einfügen `IOptions<TOptions>` direkt in eine Sicht:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name="in-memory-provider"></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Erfordert ASP.NET Core 1.1 oder höher.*

`IOptionsSnapshot`unterstützt die Konfigurationsdaten werden erneut geladen, wenn sich die Konfigurationsdatei geändert hat. Er verfügt über einen minimalen Aufwand. Mit `IOptionsSnapshot` mit `reloadOnChange: true`, sind die Optionen zum gebunden `IConfiguration` und erneut geladen, wenn sich geändert.

Im folgende Beispiel wird veranschaulicht, wie ein neuer `IOptionsSnapshot` wird erstellt, nachdem *config.json* ändert. Anforderungen an den Server werden zurückgegeben, die gleiche Uhrzeit *config.json* hat **nicht** geändert. Die erste Anforderung nach *config.json* Änderungen werden die Zeit angezeigt.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

Die folgende Abbildung zeigt die Serverausgabe:

![Browser Bild anzeigen "Letzte Aktualisierung: 11/22/2016 16:43 Uhr"](configuration/_static/first.png)

Aktualisieren des Browsers ändert sich der Nachrichtenwert oder angezeigte Uhrzeit (Wenn *config.json* wurde nicht geändert).

Ändern und speichern Sie die *config.json* und den Browser zu aktualisieren:

![Browser Bild anzeigen "," e: "Letzte Aktualisierung 11/22/2016 16:53:00 Uhr"](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>In-Memory-Anbieter und die Bindung an einer POCO-Klasse

Das folgende Beispiel zeigt, wie den in-Memory-Anbieter verwenden und auf eine Klasse gebunden:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

Konfigurationswerte werden als Zeichenfolgen zurückgegeben, aber Binden ermöglicht die Erstellung von Objekten. Bindung ermöglicht es Ihnen POCO-Objekte oder Objektdiagramme ganzer abgerufen. Im folgende Beispiel wird gezeigt, wie zum Binden an `MyWindow` und das Muster Optionen mit einer ASP.NET-MVC-Anwendung Core verwenden:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Binden Sie die benutzerdefinierte Klasse in `ConfigureServices` beim Host zu erstellen:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Zeigen Sie die Einstellungen aus der `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

Das folgende Beispiel zeigt die [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) Erweiterungsmethode:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

Die ConfigurationBinder `GetValue<T>` Methode können Sie einen Standardwert (80 in der Stichprobe) angeben. `GetValue<T>`ist für einfache Szenarien, und Bindung erfolgt nicht auf die gesamte Abschnitte. `GetValue<T>`Ruft die Skalarwerte aus `GetSection(key).Value` für einen bestimmten Typ konvertiert.

## <a name="binding-to-an-object-graph"></a>Binden an ein Objektdiagramm

Sie können rekursiv Bindung auf jedes Objekt in einer Klasse. Beachten Sie Folgendes `AppOptions` Klasse:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

Im folgende Beispiel bindet an die `AppOptions` Klasse:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**ASP.NET Core 1.1** und höher können `Get<T>`, die gesamte Abschnitte funktioniert. `Get<T>`kann weitere Convienent als die Verwendung von `Bind`. Der folgende Code zeigt, wie Sie `Get<T>` mit dem obigen Beispiel:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

Mit den folgenden *appsettings.json* Datei:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

Das Programm zeigt `Height 11`.

Der folgende Code verwendet werden kann, Einheit Testen der Konfiguration:

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

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name="custom-config-providers"></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Basic-Beispiel des benutzerdefinierten Anbieter für Entity Framework

In diesem Abschnitt wird ein grundlegende Konfigurationsanbieter, das Name-Wert-Paare aus einer Datenbank mithilfe von EF erstellt. 

Definieren einer `ConfigurationValue` Entität für die Konfigurationswerte in der Datenbank gespeichert:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Hinzufügen einer `ConfigurationContext` zum Speichern und Zugreifen auf die konfigurierten Werte:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Erstellen Sie eine Klasse, die implementiert [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Erstellen Sie den standardkonfigurationsanbieter durch Vererben von [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  Der Konfigurationsanbieter initialisiert die Datenbank aus, wenn sie leer ist:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Die hervorgehobenen Werte aus der Datenbank ("value_from_ef_1" und "value_from_ef_2") werden angezeigt, wenn das Beispiel ausgeführt wird.

Sie können Hinzufügen einer `EFConfigSource` Erweiterungsmethode für die Konfigurationsquelle hinzufügen:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

Der folgende Code zeigt, wie Sie die Verwendung der benutzerdefinierten `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Beachten Sie das Beispiel fügt die benutzerdefinierte `EFConfigProvider` nach der JSON-Anbieter, also alle Einstellungen aus der Datenbank überschrieben aus der *appsettings.json* Datei.

Mit den folgenden *appsettings.json* Datei:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

Folgendes wird angezeigt:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>CommandLine Konfigurationsanbieter

Die [CommandLine Konfigurationsanbieter](/aspnet/core/api/microsoft.extensions.configuration.commandline.commandlineconfigurationprovider) Befehlszeilenargument Schlüssel-Wert-Paare für die Konfiguration zur Laufzeit erhält.

[Zeigen Sie an oder Herunterladen Sie der CommandLine Konfigurationsbeispiel](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample/CommandLine)

### <a name="setting-up-the-provider"></a>Der Anbieter einrichten

# <a name="basic-configurationtabbasicconfiguration"></a>[Basiskonfiguration](#tab/basicconfiguration)

Um Befehlszeilenkonfiguration zu aktivieren, rufen Sie die `AddCommandLine` Erweiterungsmethode in einer Instanz von [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder):

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program.cs?highlight=18,21)]

Der Code ausführen, wird die folgende Ausgabe angezeigt:

```console
MachineName: MairaPC
Left: 1980
```

Argumentübergabe Schlüssel-Wert-Paare in der Befehlszeile ändert die Werte der `Profile:MachineName` und `App:MainWindow:Left`:

```console
dotnet run Profile:MachineName=BartPC App:MainWindow:Left=1979
```

Das Konsolenfenster zeigt:

```console
MachineName: BartPC
Left: 1979
```

Zum Lieferumfang von anderen Anbietern Konfiguration Befehlszeilenkonfiguration außer Kraft setzen, rufen `AddCommandLine` auf letzte `ConfigurationBuilder`:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?range=11-16&highlight=1,5)]

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Typische ASP.NET Core 2.x-apps verwenden die statische Hilfsmethode `CreateDefaultBuilder` auf den Host zu erstellen:

[!code-csharp[Main](configuration/sample_snapshot/Program.cs?highlight=12)]

`CreateDefaultBuilder`optionale Konfiguration von lädt *appsettings.json*, *"appSettings". {} Umgebung} JSON*, [vertrauliche Benutzerdaten](xref:security/app-secrets) (in der `Development` Umgebung), Umgebungsvariablen und Befehlszeilenargumente. Der Konfigurationsanbieter CommandLine zuletzt aufgerufen. Den Anbieter zuletzt aufrufen kann die Befehlszeilenargumente übergeben, die zur Laufzeit Konfigurationssatz durch die anderen Konfigurationsanbieter überschreiben zuvor aufgerufen.

Beachten Sie, dass für *"appSettings"* -Dateien mit `reloadOnChange` aktiviert ist. Befehlszeilenargumente werden überschrieben, wenn ein übereinstimmendes Konfigurationswert in ein *"appSettings"* Datei geändert wird, nachdem die app gestartet wird.

> [!NOTE]
> Als Alternative zur Verwendung der `CreateDefaultBuilder` -Methode, erstellen einen Host mit [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) und erstellen manuell mit der [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) wird in ASP.NET Core unterstützt 2.x. Finden Sie unter der Registerkarte "1.x ASP.NET Core" für Weitere Informationen.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Erstellen einer [ConfigurationBuilder](/api/microsoft.extensions.configuration.configurationbuilder) , und rufen Sie die `AddCommandLine` Methode, um den Konfigurationsanbieter CommandLine verwenden. Den Anbieter zuletzt aufrufen kann die Befehlszeilenargumente übergeben, die zur Laufzeit Konfigurationssatz durch die anderen Konfigurationsanbieter überschreiben zuvor aufgerufen. Wendet die Konfiguration auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) mit der `UseConfiguration` Methode:

[!code-csharp[Main](configuration/sample_snapshot/CommandLine/Program2.cs?highlight=11,15,19)]

---

### <a name="arguments"></a>Argumente

In der Befehlszeile übergebenen Argumente müssen in einem der zwei Formate, die in der folgenden Tabelle aufgeführten entsprechen.

| Arguments-format                                                     | Beispiel        |
| ------------------------------------------------------------------- | :------------: |
| Einzelne Argument: ein Schlüssel-Wert-Paar, das durch ein Gleichheitszeichen getrennt (`=`) | `key1=value`   |
| Sequenz der zwei Argumente: ein Schlüssel-Wert-Paar, getrennt durch ein Leerzeichen    | `/key1 value1` |

**Einzelnes argument**

Der Wert muss ein Gleichheitszeichen folgen (`=`). Der Wert kann null sein (z. B. `mykey=`).

Der Schlüssel möglicherweise ein Präfix aufweisen.

| Präfix               | Beispiel         |
| ------------------------ | :-------------: |
| Kein Präfix                | `key1=value1`   |
| Einzelne Dash (`-`) &#8224; | `-key2=value2`  |
| Zwei Bindestriche (`--`)        | `--key3=value3` |
| Schrägstrich (`/`)      | `/key4=value4`  |

&#8224; Ein Schlüssel mit einem Bindestrich-Präfix (`-`) muss angegeben werden, [wechseln Zuordnungen](#switch-mappings), unten beschrieben.

Beispielbefehl:

```console
dotnet run key1=value1 -key2=value2 --key3=value3 /key4=value4
```

Hinweis: Wenn `-key1` ist nicht vorhanden, in der [wechseln Zuordnungen](#switch-mappings) übergeben, um den Konfigurationsanbieter eine `FormatException` ausgelöst wird.

**Sequenz aus zwei Argumenten**

Der Wert muss darf nicht null sein und den Schlüssel durch ein Leerzeichen getrennt.

Der Schlüssel muss ein Präfix aufweisen.

| Präfix               | Beispiel         |
| ------------------------ | :-------------: |
| Einzelne Dash (`-`) &#8224; | `-key1 value1`  |
| Zwei Bindestriche (`--`)        | `--key2 value2` |
| Schrägstrich (`/`)      | `/key3 value3`  |

&#8224; Ein Schlüssel mit einem Bindestrich-Präfix (`-`) muss angegeben werden, [wechseln Zuordnungen](#switch-mappings), unten beschrieben.

Beispielbefehl:

```console
dotnet run -key1 value1 --key2 value2 /key3 value3
```

Hinweis: Wenn `-key1` ist nicht vorhanden, in der [wechseln Zuordnungen](#switch-mappings) übergeben, um den Konfigurationsanbieter eine `FormatException` ausgelöst wird.

### <a name="duplicate-keys"></a>Doppelte Schlüssel

Wenn doppelte Schlüssel bereitgestellt werden, wird das letzte Schlüssel-Wert-Paar verwendet.

### <a name="switch-mappings"></a>Switch-Zuordnungen

Wenn mit der manuellen Erstellung `ConfigurationBuilder`, Sie können optional einen Switch Zuordnungen Wörterbuch, das Bereitstellen der `AddCommandLine` Methode. Switch-Zuordnungen können Sie Logik für den Austausch Schlüsselnamen zu übermitteln.

Wenn das Wörterbuch der Switch-Zuordnungen verwendet wird, wird das Wörterbuch für einen Schlüssel überprüft, die den vom ein Befehlszeilenargument angegebenen Schlüssel entspricht. Wenn das Befehlszeile-Schlüssel im Wörterbuch gefunden wird, wird der Wörterbuchwert (schlüsselersetzung) zurück, um die Konfiguration übergeben. Eine Switch-Zuordnung für eine beliebige Taste Befehlszeilen, die einen einzelnen Bindestrich vorangestellt erforderlich ist (`-`).

Wechseln Sie wichtige Wörterbuchregeln Zuordnungen:

* Switches müssen mit einem Bindestrich beginnen (`-`) oder Double-Dash (`--`).
* Das Wörterbuch der Switch-Zuordnungen dürfen keine doppelte Schlüssel enthalten.

Im folgenden Beispiel die `GetSwitchMappings` Methode ermöglicht die Befehlszeilenargumente, die einen Bindestrich verwenden (`-`) Schlüssel Präfix, und vermeiden Sie führende Unterschlüsseln Präfixe.

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs?highlight=10-19,32)]

Ohne Befehlszeilenargumente Wörterbuch bereitgestellt, um `AddInMemoryCollection` legt die Konfigurationswerte. Führen Sie die app mit dem folgenden Befehl ein:

```console
dotnet run
```

Das Konsolenfenster zeigt:

```console
MachineName: RickPC
Left: 1980
```

Verwenden Sie die folgenden Konfigurationseinstellungen übergeben:

```console
dotnet run /Profile:MachineName=DahliaPC /App:MainWindow:Left=1984
```

Das Konsolenfenster zeigt:

```console
MachineName: DahliaPC
Left: 1984
```

Nachdem das Wörterbuch der Switch-Zuordnungen erstellt wurde, enthält es die Daten, die in der folgenden Tabelle gezeigt.

| Key            | Wert                 |
| -------------- | --------------------- |
| `-MachineName` | `Profile:MachineName` |
| `-Left`        | `App:MainWindow:Left` |

Führen Sie zur Veranschaulichung Key wechseln, Wörterbuch mit den folgenden Befehl ein:

```console
dotnet run -MachineName=ChadPC -Left=1988
```

Die Befehlszeile Schlüssel vertauscht werden. Das Konsolenfenster zeigt die Konfigurationswerte für `Profile:MachineName` und `App:MainWindow:Left`:

```console
MachineName: ChadPC
Left: 1988
```

## <a name="the-webconfig-file"></a>Die Datei "Web.config"

Ein *"Web.config"* Datei ist erforderlich, wenn Sie die app in IIS oder IIS Express hosten. *"Web.config"* Schaltet die AspNetCoreModule in IIS, um Ihre app zu starten. Einstellungen im *"Web.config"* die AspNetCoreModule in IIS für Ihre app starten und konfigurieren, Module und andere IIS-Einstellungen zu aktivieren. Wenn Sie Visual Studio verwenden, und löschen *"Web.config"*, Visual Studio erstellt ein neues Konto.

## <a name="additional-notes"></a>Zusätzliche Hinweise

* (Dependency Injection, DI) wird nicht bis zum nach gesetzt `ConfigureServices` aufgerufen wird.
* Das Konfigurationssystem ist nicht DI beachten.
* `IConfiguration`verfügt über zwei spezialisierungen:
  * `IConfigurationRoot`Für den Stammknoten verwendet. Können erneut auslösen.
  * `IConfigurationSection`Stellt einen Abschnitt der Konfigurationswerte. Die `GetSection` und `GetChildren` -Methoden zurückgeben einer `IConfigurationSection`.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Arbeiten mit mehreren Umgebungen](environments.md)
* [Sicheres Speichern geheimer App-Schlüssel während der Entwicklung](../security/app-secrets.md)
* [Hosten in ASP.NET Core](xref:fundamentals/hosting)
* [Abhängigkeitsinjektion](dependency-injection.md)
* [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration)
