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
ms.openlocfilehash: a14bc7fbcdac9acddfdab4fcd6e40385ca48bcc4
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
<a name=fundamentals-configuration></a>

  # <a name="configuration-in-aspnet-core"></a>Konfiguration in ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [Markierung Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), und [Daniel Roth](https://github.com/danroth27)

Der Konfigurations-API bietet eine Möglichkeit zum Konfigurieren von einer app basierend auf eine Liste von Name / Wert-Paaren. Konfiguration wird zur Laufzeit aus mehreren Quellen gelesen werden. Die Name-Wert-Paare können in einer Hierarchie mit mehreren Ebenen gruppiert werden. Es gibt Konfigurationsanbieter für ein:

* Dateiformate (INI, JSON und XML)
* Befehlszeilenargumente
* Umgebungsvariablen
* Die .NET Objekte im Arbeitsspeicher
* Eine verschlüsselte Benutzerspeicher
* [Azure Key Vault.](xref:security/key-vault-configuration)
* Benutzerdefinierte Anbieter, die Sie installieren, oder erstellen

Jede Konfigurationswert ordnet einen Zeichenfolgenschlüssel. Integrierte Bindung unterstützt beim Deserialisieren von Einstellungen in einer benutzerdefinierten [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) Objekt (eine einfache .NET Klasse mit Eigenschaften).

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

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
* * "appSettings". \<EnvironmentName > JSON
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

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>Mithilfe der Optionen und Konfigurationsobjekten

Das Muster Optionen verwendet benutzerdefinierte Optionsklassen, um eine Gruppe von verwandten Einstellungen darstellen. Es wird empfohlen, entkoppelte Klassen für jede Funktion in Ihrer app zu erstellen. Entkoppelte Klassen entsprechen:

* Die [Schnittstelle Trennung Prinzip (ISP)](http://deviq.com/interface-segregation-principle/) : Klassen richten sich nur auf den Konfigurationseinstellungen, die sie verwenden.
* [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) : Einstellungen für die verschiedenen Teile Ihrer App sind nicht abhängige oder gekoppelten miteinander.

Die Optionsklasse muss nicht abstrakten mit einem öffentlichen parameterlosen Konstruktor. Zum Beispiel:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

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

<a name=in-memory-provider></a>

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

<a name=custom-config-providers></a>

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

Im folgenden wird angezeigt:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>CommandLine Konfigurationsanbieter

Im folgende Beispiel ermöglicht den Konfigurationsanbieter CommandLine letzten:

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]

Verwenden Sie die folgenden Konfigurationseinstellungen übergeben:

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

Das wird angezeigt:

```console
Hello Bob
Left 1234
```

Die `GetSwitchMappings` Methode können Sie mit `-` statt `/` und entfernt die führenden Unterschlüsseln Präfixe. Zum Beispiel:

```console
dotnet run -MachineName=Bob -Left=7734
```

Zeigt die:

```console
Hello Bob
Left 7734
```

Befehlszeilenargumente müssen einen Wert enthalten (es kann null sein). Zum Beispiel:

```console
dotnet run /Profile:MachineName=
```

Ist in Ordnung, aber

```console
dotnet run /Profile:MachineName
```

/ / ergibt eine Ausnahme. Wenn Sie ein Präfix Befehlszeilenoption von - oder--für das es keine entsprechende Switch Zuordnung angeben, wird eine Ausnahme ausgelöst werden.

## <a name="the-webconfig-file"></a>Die Datei "Web.config"

Ein *"Web.config"* Datei ist erforderlich, wenn Sie die app in IIS oder IIS Express hosten. *"Web.config"* Schaltet die AspNetCoreModule in IIS, um Ihre app zu starten. Einstellungen im *"Web.config"* die AspNetCoreModule in IIS für Ihre app starten und konfigurieren, Module und andere IIS-Einstellungen zu aktivieren. Wenn Sie Visual Studio verwenden, und löschen *"Web.config"*, Visual Studio erstellt ein neues Konto.

### <a name="additional-notes"></a>Zusätzliche Hinweise

* (Dependency Injection, DI) wird nicht bis zum nach gesetzt `ConfigureServices` aufgerufen wird.
* Das Konfigurationssystem ist nicht DI beachten.
* `IConfiguration`verfügt über zwei spezialisierungen:
  * `IConfigurationRoot`Für den Stammknoten verwendet. Können erneut auslösen.
  * `IConfigurationSection`Stellt einen Abschnitt der Konfigurationswerte. Die `GetSection` und `GetChildren` -Methoden zurückgeben einer `IConfigurationSection`.

### <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Arbeiten mit mehreren Umgebungen](environments.md)
* [Sicheres Speichern geheimer App-Schlüssel während der Entwicklung](../security/app-secrets.md)
* [Abhängigkeitsinjektion](dependency-injection.md)
* [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration)
