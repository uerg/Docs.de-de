---
title: Azure Key Vault-Konfigurationsanbieter in ASP.NET Core
author: guardrex
description: Erfahren Sie, wie Sie mit der Azure Key Vault-Konfigurationsanbieter, so konfigurieren Sie eine app mithilfe von Name / Wert-Paaren, die zur Laufzeit geladen.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/17/2018
uid: security/key-vault-configuration
ms.openlocfilehash: 474824cccdc63bb3dc3978ed68cf4c89cec12ad5
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391141"
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Azure Key Vault-Konfigurationsanbieter in ASP.NET Core

Durch [Luke Latham](https://github.com/guardrex) und [Andrew Stanton-Nurse](https://github.com/anurse)

In diesem Dokument wird erläutert, wie Sie mit der [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Konfigurationsanbieter zum Laden von app-Konfigurationswerte aus Azure Key Vault-Geheimnissen. Azure Key Vault ist ein cloudbasierter Dienst, der Sie schützen Sie Kryptografieschlüssel und Geheimnisse mithilfe von apps und Diensten unterstützt. Häufige Szenarien sind die Steuerung des Zugriffs auf sensible Konfigurationsdaten und erfüllt die Anforderung für FIPS 140-2 Level 2-überprüften Hardwaresicherheitsmodulen (HSM) beim Speichern der Konfigurationsdaten. Dieses Feature ist verfügbar für apps mit ASP.NET Core 1.1 oder höher.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="package"></a>Package

Um den Anbieter zu verwenden, fügen Sie einen Verweis auf die [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) Paket.

## <a name="app-configuration"></a>App-Konfiguration

Sie können untersuchen, den Anbieter mit der [Beispiel-apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Nachdem Sie einen schlüsseltresor einrichten und geheime Schlüssel im Tresor zu erstellen, laden geheime Werte in ihre Konfigurationen die Beispiel-apps sicher und in Webseiten anzeigen.

Der Anbieter wird hinzugefügt, in der app-Konfiguration mit der `AddAzureKeyVault` Erweiterung. In der Beispiel-apps, die Erweiterung verwendet drei Konfigurationswerte aus geladen der *"appSettings.JSON"* Datei.

| App-Einstellung    | Beschreibung                    | Beispiel                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure Key Vault-Namen           | contosovault                                 |
| `ClientId`     | Azure Active Directory-App-Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory-App-Schlüssel | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1)]

## <a name="create-key-vault-secrets-and-load-configuration-values-basic-sample"></a>Erstellen von Key Vault-Geheimnisse und Laden Sie die Konfigurationswerte (Basic-Beispiel)

1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die app, die gemäß der Anleitung im [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Geheimnisse mit Key Vault-Instanz hinzufügen der [AzureRM Key Vault-PowerShell-Modul](/powershell/module/azurerm.keyvault) erhältliche Komponente der [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.KeyVault), wird die [Azure Key Vault-REST-API](/rest/api/keyvault/), oder die [Azure-Portal](https://portal.azure.com/). Geheime Schlüssel werden erstellt, entweder als *manuelle* oder *Zertifikat* Geheimnisse. *Zertifikat* Geheimnisse sind Zertifikate für die Verwendung durch apps und Dienste, jedoch werden von der Konfigurationsanbieter nicht unterstützt. Verwenden Sie die *manuelle* Option zum Erstellen von Name / Wert-Paar-Geheimnisse für die Verwendung mit den Konfigurationsanbieter.
     * Einfache geheime Schlüssel werden als Name / Wert-Paaren erstellt. Geheime Azure Key Vault-Namen sind auf alphanumerische Zeichen und Bindestriche beschränkt.
     * Verwenden von hierarchische Werten (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen im Beispiel. Doppelpunkte, die normalerweise verwendet werden, um einen Abschnitt über einen Unterschlüssel in zu begrenzen [ASP.NET Core-Konfiguration](xref:fundamentals/configuration/index), sind in den geheimen Namen nicht zulässig. Daher sind die beiden Striche verwendet und für einen Doppelpunkt ausgetauscht werden, wenn die geheimen Schlüssel in der app Konfiguration geladen werden.
     * Erstellen Sie zwei *manuelle* geheime Schlüssel mit den folgenden Name / Wert-Paaren. Der erste geheime Schlüssel ist, einen einfachen Namen und einen Wert aus, und das zweite Geheimnis erstellt einen geheimen Wert mit einem Abschnitt und die Unterschlüssel im Namen geheimen Schlüssels:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registrieren der Beispiel-app in Azure Active Directory.
   * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` Geben Sie die PowerShell-Cmdlet, um die app für den Zugriff auf den schlüsseltresor autorisieren `List` und `Get` den Zugriff auf Geheimnisse mit `-PermissionsToSecrets list,get`.

2. Aktualisieren der app *"appSettings.JSON"* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, die erhält die Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den Namen des geheimen Schlüssels.
   * Nicht-hierarchischen Werten: der Wert für `SecretName` wird abgerufen, mit `config["SecretName"]`.
   * Hierarchischen Werten (Abschnitte): verwenden `:` (Doppelpunkt)-Notation oder der `GetSection` -Erweiterungsmethode. Verwenden Sie einen dieser Ansätze, um den Konfigurationswert zu erhalten:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Wenn Sie die app ausführen, zeigt eine Webseite geladen geheime Werte an:

![Browserfenster mit geheime Werte, die über den Azure Key Vault-Konfigurationsanbieter geladen](key-vault-configuration/_static/sample1.png)

## <a name="bind-an-array-to-a-class"></a>Binden eines Arrays an eine Klasse

Der Anbieter ist der Konfigurationswerte in ein Array für die Bindung an ein POCO-Array lesen kann.

Wenn aus einer Konfigurationsquelle zu lesen, die Schlüssel für den Doppelpunkt enthalten kann (`:`) Trennzeichen, die einen numerischen Schlüsselsegment wird verwendet, um die Schlüssel zu unterscheiden, aus denen ein Array (`:0:`, `:1:`,... `:{n}:`) angezeigt wird. Weitere Informationen finden Sie unter [Konfiguration: ein Array an eine Klasse gebunden](xref:fundamentals/configuration/index#bind-an-array-to-a-class).

Azure Key Vault-Schlüssel können keinen Doppelpunkt als Trennzeichen verwendet. In diesem Thema beschriebene Ansatz verwendet die doppelten Bindestriche (`--`) als Trennzeichen bei hierarchischen Werten (Abschnitte). Array-Schlüssel werden in Azure Key Vault gespeichert, mit doppelten Bindestrichen und numerische wichtigsten Segmente (`--0--`, `--1--`,... `--{n}--`) angezeigt wird.

Überprüfen Sie die folgenden [Serilog](https://serilog.net/) Protokollierung Anbieterkonfiguration, die von einer JSON-Datei bereitgestellt. Es gibt zwei Literale in definierte Objekt der `WriteTo` Array, das zwei Serilog entsprechend *senken*, welche Ziele für die Ausgabe der konsolenprotokollierung beschreiben:

```json
"Serilog": {
  "WriteTo": [
    {
      "Name": "AzureTableStorage",
      "Args": {
        "storageTableName": "logs",
        "connectionString": "DefaultEnd...ountKey=Eby8...GMGw=="
      }
    },
    {
      "Name": "AzureDocumentDB",
      "Args": {
        "endpointUrl": "https://contoso.documents.azure.com:443",
        "authorizationKey": "Eby8...GMGw=="
      }
    }
  ]
}
```

Die Konfiguration dargestellt, in der obigen JSON-Datei befindet sich in Azure Key Vault mit doppelter Bindestrich (`--`)-Notation und numerische Segmente:

| Key | Wert |
| --- | ----- |
| `Serilog--WriteTo--0--Name` | `AzureTableStorage` |
| `Serilog--WriteTo--0--Args--storageTableName` | `logs` |
| `Serilog--WriteTo--0--Args--connectionString` | `DefaultEnd...ountKey=Eby8...GMGw==` |
| `Serilog--WriteTo--1--Name` | `AzureDocumentDB` |
| `Serilog--WriteTo--1--Args--endpointUrl` | `https://contoso.documents.azure.com:443` |
| `Serilog--WriteTo--1--Args--authorizationKey` | `Eby8...GMGw==` |

## <a name="create-prefixed-key-vault-secrets-and-load-configuration-values-key-name-prefix-sample"></a>Erstellen Sie mit Präfix Key Vault-Geheimnisse und Laden Sie die Konfigurationswerte (Key-Name-Präfix-Beispiel)

`AddAzureKeyVault` bietet auch eine Überladung, die eine Implementierung von akzeptiert `IKeyVaultSecretManager`, wodurch Sie steuern, wie Key Vault-Geheimnisse in Konfigurationsschlüssel konvertiert werden. Beispielsweise können Sie implementieren die Schnittstelle zum Laden der geheime Werte basierend auf einem Präfixwert, die, den Sie beim Starten der app bereitstellen. Dadurch können Sie z. B., um Geheimnisse, die basierend auf der Version der app geladen werden.

> [!WARNING]
> Verwenden Sie keine Präfixe auf Key Vault-Geheimnisse, geheime Schlüssel für mehrere apps in dem gleichen schlüsseltresor zu platzieren oder environmental Geheimnisse zu setzen (z. B. *Entwicklung* im Vergleich zu *Produktion* Geheimnisse) in der gleichen Tresor. Es wird empfohlen, dass verschiedene apps und Entwicklungs-und produktionsumgebungen verwenden Sie separate Key Vault-Instanzen, um app-Umgebungen für das Höchstmaß an Sicherheit zu isolieren.

Verwenden die zweite Beispiel-app, Sie in den schlüsseltresor für einen geheimen Schlüssel erstellen `5000-AppSecret` (Punkte sind nicht zulässig, in Key Vault-Instanz den geheimen Namen), die ein app-Geheimnis für Ihre app-Version 5.0.0.0 darstellt. Für eine andere Version, 5.1.0.0, erstellen Sie einen geheimen Schlüssel für `5100-AppSecret`. Jede Version der app lädt einen eigenen geheimen Wert in der Konfiguration als `AppSecret`, Ausblasegerät aus der Version, die den geheimen Schlüssel geladen. Die Implementierung des Beispiels wird unten gezeigt:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=18)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

Die `Load` Methode wird aufgerufen, indem ein anbieteralgorithmus, der durchläuft die Vault-Geheimnissen, um diejenigen zu finden, die das Version-Präfix besitzen. Wenn ein Präfix Version wurden gefunden, die `Load`, verwendet der Algorithmus die `GetKey` Methode, um den Konfigurationsnamen des Namen des geheimen Schlüssels zurückzugeben. Es entfernt das Version-Präfix aus der Name des Geheimnisses und gibt den Rest der Namen des geheimen Schlüssels für das Laden von in der app-Konfiguration-Name-Wert-Paaren.

Wenn Sie diesen Ansatz zu implementieren:

1. Die Key Vault-Geheimnisse werden geladen.
2. Der geheime Zeichenfolge für `5000-AppSecret` abgeglichen wird.
3. Die Version `5000` wird entfernt (mit den Bindestrich), auf den Namen des Schlüssels verlässt `AppSecret` , mit dem geheimen Wert in der app Konfiguration zu laden.

> [!NOTE]
> Sie bieten auch eigene `KeyVaultClient` Implementierung `AddAzureKeyVault`. Bereitstellen eines benutzerdefinierten Clients können Sie eine einzelne Instanz des Clients zwischen den Konfigurationsanbieter und andere Teile Ihrer app nutzen.

1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die app, die gemäß der Anleitung im [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Geheimnisse mit Key Vault-Instanz hinzufügen der [AzureRM Key Vault-PowerShell-Modul](/powershell/module/azurerm.keyvault) erhältliche Komponente der [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.KeyVault), wird die [Azure Key Vault-REST-API](/rest/api/keyvault/), oder die [Azure-Portal](https://portal.azure.com/). Geheime Schlüssel werden erstellt, entweder als *manuelle* oder *Zertifikat* Geheimnisse. *Zertifikat* Geheimnisse sind Zertifikate für die Verwendung durch apps und Dienste, jedoch werden von der Konfigurationsanbieter nicht unterstützt. Verwenden Sie die *manuelle* Option zum Erstellen von Name / Wert-Paar-Geheimnisse für die Verwendung mit den Konfigurationsanbieter.
     * Verwenden von hierarchische Werten (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen.
     * Erstellen Sie zwei *manuelle* geheime Schlüssel mit den folgenden Name / Wert-Paaren:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrieren der Beispiel-app in Azure Active Directory.
   * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` Geben Sie die PowerShell-Cmdlet, um die app für den Zugriff auf den schlüsseltresor autorisieren `List` und `Get` den Zugriff auf Geheimnisse mit `-PermissionsToSecrets list,get`.

2. Aktualisieren der app *"appSettings.JSON"* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, die erhält die Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den geheimen Namen des mit Präfix. In diesem Beispiel wird das Präfix der app-Version, die Sie bereitgestellt ist die `PrefixKeyVaultSecretManager` beim Hinzufügen des Azure Key Vault-Konfigurationsanbieters. Der Wert für `AppSecret` wird abgerufen, mit `config["AppSecret"]`. Die Webseite, die von der app generierten zeigt den geladenen Wert:

   ![Browserfenster mit der ein geheimer Wert, der über die Azure Key Vault-Konfigurationsanbieter geladen wird, wenn die Version des 5.0.0.0 ist](key-vault-configuration/_static/sample2-1.png)

4. Ändern Sie die Version der app-Assembly in der Projektdatei aus `5.0.0.0` zu `5.1.0.0` und führen Sie die app erneut aus. In diesem Fall der geheimen zurückgegebene Wert ist `5.1.0.0_secret_value`. Die Webseite, die von der app generierten zeigt den geladenen Wert:

   ![Browserfenster mit der ein geheimer Wert, der über die Azure Key Vault-Konfigurationsanbieter geladen wird, wenn die Version des 5.1.0.0 ist](key-vault-configuration/_static/sample2-2.png)

## <a name="control-access-to-the-clientsecret"></a>Steuern des Zugriffs auf die ClientSecret

Verwenden der [Secret Manager-Tool](xref:security/app-secrets) verwalten die `ClientSecret` außerhalb Ihrer Projektstruktur für die Quelle. Mit dem Geheimnis-Manager, die Sie app-Geheimnissen mit einem bestimmten Projekt verknüpfen und für mehrere Projekte freigeben.

Wenn Sie eine .NET Framework-app in einer Umgebung entwickeln, die Zertifikate unterstützt, können Sie mit einem x. 509-Zertifikat in Azure Key Vault authentifizieren. Privaten Schlüssel des x. 509-Zertifikats wird vom Betriebssystem verwaltet. Weitere Informationen finden Sie unter [authentifizieren mit einem Zertifikat anstelle eines geheimen Clientschlüssels](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Verwenden der `AddAzureKeyVault` Überladung verwenden, akzeptiert eine `X509Certificate2` (`_env` im folgenden Beispiel:

```csharp
var builtConfig = config.Build();

var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates
    .Find(X509FindType.FindByThumbprint, 
        config["CertificateThumbprint"], false);

config.AddAzureKeyVault(
    builtConfig["Vault"],
    builtConfig["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(context.HostingEnvironment.ApplicationName));

store.Close();
```

## <a name="reload-secrets"></a>Laden Sie die Geheimnisse

Geheimnisse werden zwischengespeichert, bis `IConfigurationRoot.Reload()` aufgerufen wird. Abgelaufen ist, deaktiviert, und die aktualisierte Geheimnisse im schlüsseltresor werden nicht berücksichtigt, von der app bis `Reload` ausgeführt wird.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Deaktiviert und abgelaufene Geheimnisse

Deaktiviert und abgelaufene Geheimnisse Auslösen einer `KeyVaultClientException`. Um zu verhindern, dass Ihre app auslösen, ersetzen Sie Ihre app, oder aktualisieren Sie das Geheimnis deaktiviert/ist abgelaufen.

## <a name="troubleshoot"></a>Problembehandlung

Bei die app ein Fehler beim Laden der Konfiguration, die mithilfe des Anbieters auftritt, wird eine Fehlermeldung geschrieben, auf die [Infrastruktur von ASP.NET Core-Protokollierung](xref:fundamentals/logging/index). Die folgenden Bedingungen werden verhindert, dass die Konfiguration geladen:

* Die app ist nicht ordnungsgemäß in Azure Active Directory konfiguriert.
* Die Key Vault-Instanz ist in Azure Key Vault nicht vorhanden.
* Die app ist nicht autorisiert, auf den schlüsseltresor zugreifen.
* Die Zugriffsrichtlinie beinhaltet keine `Get` und `List` Berechtigungen.
* In den schlüsseltresor ist die Konfigurationsdaten (Name / Wert-Paar) falsch benannt, fehlen, deaktiviert oder abgelaufen.
* Die app hat den falschen schlüsseltresornamen (`Vault`), Azure AD-App-Id (`ClientId`), oder Azure AD Key (`ClientSecret`).
* Die Azure AD Key (`ClientSecret`) ist abgelaufen.
* Der Konfigurationsschlüssel (Name) ist falsch, in der app für den Wert an, die, den Sie laden möchten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/configuration/index>
* [Microsoft Azure: Key Vault-Instanz](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Dokumentation zu Key Vault](/azure/key-vault/)
* [Das Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault](/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient-Klasse](/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
