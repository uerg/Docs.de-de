---
uid: config-builder
title: Konfigurations-Generatoren für ASP.NET
author: rick-anderson
description: Erfahren Sie, wie Sie Konfigurationsdaten aus anderen Quellen als Datei "Web.config"-Werte aus externen Quellen abrufen.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148836"
---
# <a name="configuration-builders-for-aspnet"></a>Konfigurations-Generatoren für ASP.NET

Durch [Stephen Molloy](https://github.com/StephenMolloy) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Konfigurations-Generatoren bieten einen Mechanismus für moderne und flexible für ASP.NET-Apps Konfigurationswerte aus externen Quellen abgerufen.

Konfigurationsgeneratoren:

* Sind in .NET Framework 4.7.1 und höher verfügbar.
* Geben Sie einen flexiblen Mechanismus zum Lesen von Werten.
* Beheben Sie einige grundlegende Anforderungen von apps in einem Container zu verschieben und fokussierte Umgebung der cloud.
* Dienen zur Verbesserung des Schutzes von Konfigurationsdaten durch Zeichnen von Quellen, die zuvor nicht verfügbar (z. B. Azure Key Vault und Umgebungsvariablen) im Konfigurationssystem .NET.

## <a name="keyvalue-configuration-builders"></a>Schlüssel/Wert-Konfigurations-Generatoren

Ein häufiges Szenario, das vom Konfigurations-Generatoren verarbeitet werden kann, ist, einen einfachen Schlüssel-Wert-Ersatz-Mechanismus für Konfigurationsabschnitte bereitzustellen, die ein Schlüssel/Wert-Muster folgen. Das .NET Framework-Konzept der ConfigurationBuilders ist nicht beschränkt auf bestimmte Konfigurationsabschnitte oder Muster. Allerdings viele der in der Konfigurations-Generatoren `Microsoft.Configuration.ConfigurationBuilders` ([Github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) innerhalb des Schlüssel-Wert-Musters arbeiten.

## <a name="keyvalue-configuration-builders-settings"></a>Schlüssel/Wert-Generatoren Konfigurationseinstellungen

Die folgenden Einstellungen gelten für alle Schlüssel/Wert-konfigurationsbuilder in `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modus

Der Konfigurations-Generatoren verwenden eine externe Quelle von Schlüssel-/Wertinformationen um ausgewählten Schlüssel/Wert-Elementen des Konfigurationssystems zu füllen. Insbesondere die `<appSettings/>` und `<connectionStrings/>` Abschnitten erhalten Sie spezielle Behandlung von Konfigurations-Generatoren. Die Generatoren arbeiten in drei Modi:

* `Strict` -Der Standardmodus. In diesem Modus kann der Konfigurations-Generator nur auf bekannte Konfigurationsabschnitte für Schlüssel/Wert-orientierte eingesetzt. `Strict` Jeder Schlüssel im Abschnitt listet die Modus. Wenn ein übereinstimmenden Schlüssel in der externen Quelle gefunden wird:

   * Der Konfigurations-Generatoren ersetzen Sie den Wert im Abschnitt mit resultierenden mit dem Wert aus der externen Quelle.
* `Greedy` : Dieser Modus ist eng verwandt mit `Strict` Modus. Anstatt die Begrenzung auf Schlüssel, die bereits in der ursprünglichen Konfiguration vorhanden sein:

  * Der Konfigurations-Generatoren fügt alle Schlüssel/Wert-Paare aus der externen Quelle in der resultierenden Konfigurationsabschnitt hinzu.

* `Expand` -Arbeitet mit den unformatierten XML-Daten, bevor er in ein Konfigurationsobjekt für den Abschnitt aufgelöst wird. Sie kann als Erweiterung von Token in einer Zeichenfolge betrachtet werden. Einen beliebigen Teil der unformatierten XML-Zeichenfolge mit dem Muster übereinstimmt `${token}` ist ein Kandidat für tokenerweiterung. Wenn keine entsprechenden Wert in der externen Quelle gefunden wird, wird das Token nicht geändert werden. Generatoren, die in diesem Modus sind nicht darauf beschränkt die `<appSettings/>` und `<connectionStrings/>` Abschnitte.

Das folgende Markup aus *"Web.config"* ermöglicht die [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` Modus:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` angezeigt, in der vorherigen *"Web.config"* Datei:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:

* Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.
* Die Werte der Umgebung Variable If festgelegt.

Z. B. `ServiceID` enthält:

* "ServiceID Wert aus" Web.config "", wenn die Umgebungsvariable `ServiceID` ist nicht festgelegt.
* Der Wert des der `ServiceID` Umgebungsvariable, falls festgelegt.

Die folgende Abbildung zeigt die `<appSettings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:

![Umgebung-editor](config-builder/static/env.png)

Hinweis: Sie sollten zu beenden und starten Sie Visual Studio, um Änderungen in Umgebungsvariablen anzuzeigen.

### <a name="prefix-handling"></a>Behandlung von Präfix

Wichtige Präfixe können Einstellungsschlüssel vereinfachen, da:

* Die .NET Framework-Konfiguration ist komplex und geschachtelte.
* Externen Schlüssel/Wert-Quellen sind häufig basic "und" naturgemäß Flatfile. Beispielsweise werden Umgebungsvariablen nicht geschachtelt werden.

Eine der folgenden Methoden zum Einfügen von sowohl verwenden `<appSettings/>` und `<connectionStrings/>` in die Konfiguration über Umgebungsvariablen:

* Mit der `EnvironmentConfigBuilder` in der standardmäßigen `Strict` Modus und den entsprechenden Schlüsselnamen in der Konfigurationsdatei. Der vorherige Code und Markup verwendet diesen Ansatz. Mit diesem Ansatz Sie können **nicht** haben identisch benannten Schlüssel in beiden `<appSettings/>` und `<connectionStrings/>`.
* Verwenden Sie zwei `EnvironmentConfigBuilder`s in `Greedy` Modus mit unterschiedlichen Präfixen und `stripPrefix`. Bei diesem Ansatz kann die app lesen `<appSettings/>` und `<connectionStrings/>` ohne die Konfigurationsdatei aktualisieren. Im nächsten Abschnitt [StripPrefix](#stripprefix), veranschaulicht dies.
* Verwenden Sie zwei `EnvironmentConfigBuilder`s in `Greedy` Modus mit unterschiedlichen Präfixen. Bei diesem Ansatz können nicht Sie doppelte Schlüssel aufweisen wie Schlüsselnamen nach Präfix unterscheiden müssen.  Zum Beispiel:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Mit der im obigen Markup kann die gleiche einfache Schlüssel/Wert-Quelle zum Auffüllen der Konfiguration für zwei verschiedene Abschnitte verwendet werden.

Die folgende Abbildung zeigt die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:

![Umgebung-editor](config-builder/static/prefix.png)

Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte enthalten, die in der vorherigen *"Web.config"* Datei:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:

* Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.
* Die Werte der Umgebung Variable If festgelegt.

Verwenden Sie beispielsweise die vorherige *"Web.config"* -Datei, die die Schlüssel/Werte in der vorherigen Abbildung Umgebung-Editor und dem vorherigen Code, die folgenden Werte festgelegt sind:

|  Key              | Wert |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID von Env-Variablen|
|    AppSetting_default            | AppSetting_default Wert env |
|       ConnStr_default         | ConnStr_default Val von env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: Boolesch, standardmäßig `false`. 

Das vorhergehende XML-Markup trennt von app-Einstellungen von Verbindungszeichenfolgen, jedoch müssen Sie alle Schlüssel in der *"Web.config"* Datei, die das angegebene Präfix verwendet. Z. B. das Präfix `AppSetting` muss hinzugefügt werden, um die `ServiceID` Schlüssel ("AppSetting_ServiceID"). Mit `stripPrefix`, das Präfix wird nicht verwendet, der *"Web.config"* Datei. Das Präfix muss in der Konfigurations-Generator-Quelle (z. B. in der Umgebung.) Wir erwarten die meisten Entwickler verwenden `stripPrefix`.

Anwendungen werden in der Regel aus dem Präfix entfernen. Die folgenden *"Web.config"* entfernt das Präfix:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

In der vorherigen *"Web.config"* -Datei, die `default` Schlüssel wird sowohl die `<appSettings/>` und `<connectionStrings/>`.

Die folgende Abbildung zeigt die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte aus dem vorherigen *"Web.config"* Dateisatz in der Umgebung-Editor:

![Umgebung-editor](config-builder/static/prefix.png)

Der folgende code liest die `<appSettings/>` und `<connectionStrings/>` Schlüssel/Werte enthalten, die in der vorherigen *"Web.config"* Datei:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

Der vorangehende Code wird die Eigenschaftswerte festgelegt, um:

* Die Werte in der *"Web.config"* Datei, wenn die Schlüssel in Umgebungsvariablen nicht festgelegt werden.
* Die Werte der Umgebung Variable If festgelegt.

Verwenden Sie beispielsweise die vorherige *"Web.config"* -Datei, die die Schlüssel/Werte in der vorherigen Abbildung Umgebung-Editor und dem vorherigen Code, die folgenden Werte festgelegt sind:

|  Key              | Wert |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID von Env-Variablen|
|    default            | AppSetting_default Wert env |
|    default         | ConnStr_default Val von env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: String, Standardwert `@"\$\{(\w+)\}"`

Die `Expand` Verhalten der Generatoren sucht das unformatierte XML für Token, die wie folgt aussehen `${token}`. Suche erfolgt mit dem regulären Standardausdruck `@"\$\{(\w+)\}"`. Der Satz von Zeichen, die entspricht `\w` ist strenger als XML und viele Konfigurationsquellen zulassen. Verwendung `tokenPattern` Wenn mehr Zeichen als `@"\$\{(\w+)\}"` in den token-Namen erforderlich sind.

`tokenPattern`: Zeichenfolge:

* Ermöglicht Entwicklern, die dem regulären Ausdruck zu ändern, für den Abgleich von token verwendet wird.
* Keine Validierung wird vorgenommen, um sicherzustellen, dass es sich um ein Regex wohlgeformt und nicht riskant ist.
* Es muss eine Erfassungsgruppe enthalten. Das gesamte Token muss mit dem gesamten regulären Ausdruck übereinstimmen. Die erste Erfassung muss den token-Namen, die in der Konfigurationsquelle gesucht werden.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Konfigurations-Generatoren in Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

Die [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Ist die einfachste der konfigurationsgeneratoren.
* Liest die Werte aus der Umgebung.
* Es muss keine zusätzlichen Konfigurationsoptionen.
* Die `name` Attributwert ist willkürlich.

**Hinweis:** In einer Umgebung mit Windows-Container werden nur zur Laufzeit festgelegten Variablen in der Umgebung der EntryPoint-Prozess eingefügt. Apps, die als Dienst ausführen, oder ein Prozess nicht EntryPoint wählen Sie diese Variablen, wenn sie andernfalls über einen Mechanismus in den Container eingefügt werden. Für [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basierte Container, die aktuelle Version des [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) verarbeitet dies in der *DefaultAppPool* nur. Andere Windows-basierten Container Varianten müssen möglicherweise ihre eigenen Injection-Methode für nicht-EntryPoint-Prozesse zu entwickeln.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nie Speichern von Kennwörtern, vertrauliche Verbindungszeichenfolgen und andere sensiblen Daten im Quellcode. Produktionsgeheimnisse sollte nicht verwendet werden zu Entwicklungs- oder Testzwecken.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Diese Konfigurations-Generator bietet eine Funktion, die ähnlich wie [Geheimnis-Manager für ASP.NET Core](/aspnet/core/security/app-secrets).

Die [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) in .NET Framework-Projekten verwendet werden kann, aber eine Datei mit Geheimnissen muss angegeben werden. Alternativ können Sie definieren die `UserSecretsId` Eigenschaft im Projekt Datei und erstellt die unformatierten geheimnisdatei am richtigen Speicherort für das Lesen. Um externe Abhängigkeiten aus Ihrem Projekt zu halten, ist die geheimnisdatei XML-Format. Die XML-Formatierung ist ein Implementierungsdetail, und das Format sollte nicht als zuverlässig betrachtet werden. Wenn Sie zum Freigeben benötigen eine *secrets.json* mit .NET Core-Projekte, erwägen Sie die Verwendung der [SimpleJsonConfigBuilder](#simplejsonconfig). Die `SimpleJsonConfigBuilder` format für .NET Core ein Implementierungsdetail unterliegen ebenfalls berücksichtigt werden sollten.

Konfiguration für Attribute `UserSecretsConfigBuilder`:

* `userSecretsId` -Dies ist die bevorzugte Methode zum Identifizieren einer XML-Datei mit Geheimnissen aus. Es funktioniert ähnlich wie .NET Core, verwendet eine `UserSecretsId` Projekteigenschaft zum Speichern von diesen Bezeichner. Die Zeichenfolge muss eindeutig sein, sie muss keine GUID sein. Mit diesem Attribut den `UserSecretsConfigBuilder` Suchen in einem bekannten Speicherort (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) für eine Datei mit Geheimnissen, die zu dieser Bezeichner gehören.
* `userSecretsFile` – Ein optionales Attribut, das die Datei mit die geheimen Schlüssel angeben. Die `~` Zeichen am Anfang auf den Stammordner der Anwendung verwendet werden kann. Dieses Attribut oder das `userSecretsId` Attribut ist erforderlich. Wenn beide angegeben werden, `userSecretsFile` hat Vorrang vor.
* `optional`: Boolesch, Standardwert `true` -wird keine Ausnahme aus, wenn die Datei mit Geheimnissen nicht gefunden werden kann. 
* Die `name` Attributwert ist willkürlich.

Die Datei mit Geheimnissen weist das folgende Format:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

Die [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) gespeicherten Werte liest die [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` ist erforderlich, entweder (der Name des Tresors) oder ein URI in den Tresor. Die anderen Attribute ermöglichen die Steuerung zu welchem Tresor für die Verbindung, jedoch nur dann erforderlich sind, wenn die Anwendung nicht in einer Umgebung ausgeführt wird, die mit `Microsoft.Azure.Services.AppAuthentication`. Die Authentifizierungsbibliothek von Azure-Dienste werden automatisch um Verbindungsinformationen aus die ausführungsumgebung möglichst zu übernehmen. Sie können automatisch überschreiben durch eine Verbindungszeichenfolge Bereitstellen von Verbindungsinformationen übernehmen.

* `vaultName` -Erforderlich, wenn `uri` in nicht angegeben. Gibt den Namen des Tresors in Ihrem Azure-Abonnement aus dem Schlüssel/Wert-Paare gelesen.
* `connectionString` -Eine Verbindungszeichenfolge, die verwendet werden, indem [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Eine Verbindung mit anderen Key Vault-Anbieter mit dem angegebenen `uri` Wert. Wenn nicht angegeben ist, Azure (`vaultName`) ist die schlüsseltresor-Anbieters.
* `version` – Azure Key Vault bietet eine versionsverwaltung für geheime Schlüssel. Wenn `version` angegeben ist, wird der Generator ruft nur Geheimnisse, die diese Version entspricht.
* `preloadSecretNames` -Wird standardmäßig dieser Generator Abfragen **alle** Schlüssel Namen in den schlüsseltresor aus, wenn es initialisiert wird. Legen Sie dieses Attribut auf, um zu verhindern, lesen alle Schlüsselwerte, `false`. Wenn dies auf `false` geheime Schlüssel 1 zu einem Zeitpunkt liest. Lesen von Geheimnissen, die jeweils einzeln kann sinnvoll sein, wenn im Tresor ermöglicht den Zugriff "Abrufen", aber nicht "List" zugreifen. **Hinweis:** Verwendung `Greedy` Modus `preloadSecretNames` muss `true` (die Standardeinstellung.)

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) ein Basiskonfiguration-Generator, der einem Verzeichnis enthaltenen Dateien als Quelle für Werte verwendet wird. Der Name einer Datei ist der Schlüssel, und der Inhalt ist der Wert. Diese Konfigurations-Generator kann nützlich sein, wenn in einem orchestrierten containerumgebung ausgeführt. Systeme, z. B. das Bereitstellen von Docker Swarm und Kubernetes `secrets` für ihre orchestrierten Windows-Container auf diese Weise Schlüssel pro Datei.

Attributdetails:

* `directoryPath` – Erforderlich. Gibt einen Pfad nach Werten gesucht werden soll. Docker für Windows, die vertraulichen Informationen sich in befinden der *C:\ProgramData\Docker\secrets* Verzeichnis standardmäßig.
* `ignorePrefix` -Dateien, die mit diesem Präfix beginnen, werden ausgeschlossen. Der Standardwert ist "ignorieren".
* `keyDelimiter` – Der Standardwert ist `null`. Wenn angegeben, durchsucht der Konfigurations-Generator mehrere Ebenen des Verzeichnisses, Schlüsselnamen mit diesem Trennzeichen erstellen. Wenn dieser Wert ist `null`, sieht der Konfigurations-Generator nur auf der obersten Ebene des Verzeichnisses.
* `optional` – Der Standardwert ist `false`. Gibt an, ob der Konfigurations-Generator zu Fehlern führen sollten, wenn das Quellverzeichnis nicht vorhanden.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nie Speichern von Kennwörtern, vertrauliche Verbindungszeichenfolgen und andere sensiblen Daten im Quellcode. Produktionsgeheimnisse sollte nicht verwendet werden zu Entwicklungs- oder Testzwecken.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

.NET Core-Projekte verwenden häufig die JSON-Dateien für die Konfiguration. Die [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) -Generator können Sie .NET Core-JSON-Dateien, die in .NET Framework verwendet werden. Diese Konfigurations-Generator ist, stellt eine einfache Zuordnung zwischen einer einfache Schlüssel/Wert-Quelle in bestimmten Schlüssel/Wert-Bereiche von .NET Framework-Konfiguration. Diese Konfigurations-Generator ist **nicht** für hierarchische Konfigurationen bereitstellen. Die zugrunde liegende JSON-Datei ähnelt einem Wörterbuch vorhanden ist, keine komplexe hierarchische-Objekt. Eine hierarchische-Datei mit mehreren Ebene kann verwendet werden. Dieser Anbieter `flatten`s die Tiefe durch den Namen der Eigenschaft auf jeder Ebene mit anhängen `:` als Trennzeichen.

Attributdetails:

* `jsonFile` – Erforderlich. Gibt an, die JSON-Datei gelesen werden. Die `~` Zeichen kann zu Beginn verwendet werden, auf das um Stammverzeichnis der app zu verweisen.
* `optional` – Boolesch, Standardwert ist `true`. Verhindert, dass Ausnahmen auslösen, wenn die JSON-Datei nicht gefunden werden kann.
* `jsonMode` - `[Flat|Sectional]`. Standardmäßig ist `Flat` festgelegt. Wenn `jsonMode` ist `Flat`, die JSON-Datei ist eine einzelne einfache Schlüssel/Wert-Quelle. Die `EnvironmentConfigBuilder` und `AzureKeyVaultConfigBuilder` sind auch einzelne einfache Schlüssel/Wert-Quellen. Wenn die `SimpleJsonConfigBuilder` erfolgt `Sectional` Modus:

  * Die JSON-Datei ist im Prinzip nur auf der obersten Ebene in mehreren Wörterbüchern unterteilt.
  * Jede der Wörterbücher wird nur auf den Konfigurationsabschnitt angewendet, die die Eigenschaft der obersten Ebene Namens auf diese entspricht. Zum Beispiel:

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementieren einen benutzerdefinierte Schlüssel-Wert-Konfigurations-Generator

Wenn der Konfigurations-Generatoren nicht Ihren Anforderungen entsprechen, können Sie eine benutzerdefinierte Aktivität schreiben. Die `KeyValueConfigBuilder` Basisklasse behandelt das Ersatz-Modi und die meisten Präfix Bedenken. Es müssen nur ein Projekt implementieren:

* Von der Basisklasse erben, und implementieren Sie eine grundlegende Quelle von Schlüssel/Wert-Paaren, durch die `GetValue` und `GetAllValues`:
* Hinzufügen der [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) zum Projekt.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

Die `KeyValueConfigBuilder` Basisklasse bietet im Wesentlichen das Geschäfts-, Schul- oder konsistent Verhalten für Schlüssel/Wert-Konfigurations-Generatoren.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Konfigurations-Generatoren-GitHub-repository](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Dienst-zu-Dienst-Authentifizierung in Azure Key Vault mithilfe von .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
