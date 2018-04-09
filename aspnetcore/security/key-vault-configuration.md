---
title: Azure Key Vault Konfigurationsanbieter in ASP.NET Kern
author: guardrex
description: Erfahren Sie, wie der Azure Key Vault-Konfigurationsanbieter verwenden Sie zum Konfigurieren einer Anwendung mithilfe von Name / Wert-Paaren, die zur Laufzeit geladen.
manager: wpickett
ms.author: riande
ms.date: 08/09/2017
ms.prod: asp.net-core
ms.topic: article
uid: security/key-vault-configuration
ms.openlocfilehash: 09f28ec3792cf137fbcfdecc593e27ce6b2e7e09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="azure-key-vault-configuration-provider-in-aspnet-core"></a>Azure Key Vault Konfigurationsanbieter in ASP.NET Kern

Von [Lukas Latham](https://github.com/guardrex) und [Andrew Stanton Krankenschwester](https://github.com/anurse)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Anzeigen oder Herunterladen von Beispielcode für 2.x:

* [Einfache Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)) – liest geheimen Werte in einer Anwendung.
* [Name des Präfix Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/2.x) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)) - Lesevorgänge geheimen Werte mit dem Präfix Schlüsselnamen, der verschiedene geheime Werte für jede Version der Anwendung laden können die Version einer Anwendung darstellt.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Anzeigen oder Herunterladen von Beispielcode für 1.x:

* [Einfache Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/1.x) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)) – liest geheimen Werte in einer Anwendung.
* [Name des Präfix Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x) ([herunterladen](xref:tutorials/index#how-to-download-a-sample)) - Lesevorgänge geheimen Werte mit dem Präfix Schlüsselnamen, der verschiedene geheime Werte für jede Version der Anwendung laden können die Version einer Anwendung darstellt. 

---

Dieses Dokument erläutert die Verwendung der [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Konfigurationsanbieter Konfigurationswerte für die Anwendung aus Azure Key Vault geheime Schlüssel geladen. Azure Key Vault ist ein cloudbasierter Dienst, mit dem Sie kryptografischer und geheimer Schlüssel apps und Dienste zu schützen. Häufige Szenarien sind das Steuern des Zugriffs auf sensible Konfigurationsdaten und validiert Hardwaresicherheitsmodulen (HSM) Erfüllung der Anforderung für FIPS 140-2 Level 2 werden, wenn Konfigurationsdaten zu speichern. Diese Funktion ist verfügbar für Anwendungen, die als ASP.NET Core 1.1 Ziel oder höher.

## <a name="package"></a>Package
Um den Anbieter zu verwenden, fügen Sie einen Verweis auf die [Microsoft.Extensions.Configuration.AzureKeyVault](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.AzureKeyVault/) Paket.

## <a name="application-configuration"></a>Anwendungskonfiguration
Untersuchen Sie den Anbieter mit der [Beispiel-apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples). Nachdem Sie einen schlüsseltresor herstellen und geheimen Schlüssel im Tresor erstellen, laden die Werte für den geheimen in ihre Konfigurationen die beispielapps sicher und in Webseiten anzuzeigen.

Der Anbieter wurde die `ConfigurationBuilder` mit der `AddAzureKeyVault` Erweiterung. In der Beispiel-apps, die Erweiterung verwendet drei Konfigurationswerte aus geladen der *appsettings.json* Datei.

| App-Einstellung    | Beschreibung                    | Beispiel                                      |
| -------------- | ------------------------------ | -------------------------------------------- |
| `Vault`        | Azure Key Vault-name           | contosovault                                 |
| `ClientId`     | Azure Active Directory-App-Id  | 627e911e-43cc-61d4-992e-12db9c81b413         |
| `ClientSecret` | Azure Active Directory-Anwendungsschlüssel | g58K3dtg59o1Pa+e59v2Tx829w6VxTB2yv9sv/101di= |

[!code-csharp[Program](key-vault-configuration/samples/basic-sample/2.x/Program.cs?name=snippet1&highlight=2,7-10)]

## <a name="creating-key-vault-secrets-and-loading-configuration-values-basic-sample"></a>Schlüsseltresor geheime Schlüssel erstellen und das Laden von Konfigurationswerten (Basic-Beispiel)
1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die Anwendung gemäß der Anleitung in [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Fügen Sie geheime Schlüssel mit dem schlüsseltresor die [AzureRM Key Vault-PowerShell-Modul](/powershell/module/azurerm.keyvault) verfügbar der [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.KeyVault), die [REST-API von Azure Key Vault](/rest/api/keyvault/), oder die [Azure-Portal](https://portal.azure.com/). Geheime Schlüssel werden erstellt, entweder als *manuell* oder *Zertifikat* geheime Schlüssel. *Zertifikat* Geheimnisse sind Zertifikate für apps und Dienste jedoch nicht von den Konfigurationsanbieter unterstützt. Verwenden Sie die *manuell* Option zum Erstellen von Name / Wert-Paar geheime Schlüssel für die Verwendung mit den Konfigurationsanbieter.
     * Einfache Kennwörter werden als Name / Wert-Paare erstellt. Azure Key Vault geheime Namen sind nur alphanumerische Zeichen und Bindestriche enthalten.
     * Verwenden Sie hierarchische Werte (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen in der Stichprobe. Doppelpunkte, die normalerweise, zur Begrenzung von eines Abschnitts über einen Unterschlüssel in verwendet werden [ASP.NET kernkonfiguration](xref:fundamentals/configuration/index), für den geheimen Namen sind nicht zulässig. Aus diesem Grund sind zwei Bindestriche verwendet und für einen Doppelpunkt ausgetauscht werden, wenn der geheime Schlüssel in der app-Konfiguration geladen werden.
     * Erstellen Sie zwei *manuell* geheime Schlüssel mit den folgenden Name / Wert-Paaren. Der erste geheime Schlüssel ist ein einfacher Name und Wert und der zweiten geheimen Schlüssel erstellt einen geheimen Wert mit einem Abschnitt und der Unterschlüssel im Namen geheimen Schlüssels:
       * `SecretName`: `secret_value_1`
       * `Section--SecretName`: `secret_value_2`
   * Registrieren der Beispiel-app bei Azure Active Directory.
   * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` PowerShell-Cmdlet zum Autorisieren der app, Zugriff auf den schlüsseltresor, bieten `List` und `Get` Zugriff auf geheime Schlüssel mit `-PermissionsToSecrets list,get`.

2. Aktualisieren Sie der app *appsettings.json* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, das erhält seine Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den Namen des geheimen Schlüssels.
   * Nicht-hierarchischen Werten: der Wert für `SecretName` abgerufen wird, mit `config["SecretName"]`.
   * Hierarchische Werte (Abschnitte): Verwendung `:` (Doppelpunkt)-Notation oder `GetSection` Erweiterungsmethode. Verwenden Sie einen dieser Ansätze, um den Konfigurationswert zu erhalten:
     * `config["Section:SecretName"]`
     * `config.GetSection("Section")["SecretName"]`

Beim Ausführen der app zeigt eine Webseite für den geheimen geladenen Werte:

![Browserfenster, die mit geheimen Werte, die über den Azure Key Vault Konfigurationsanbieter geladen](key-vault-configuration/_static/sample1.png)

## <a name="creating-prefixed-key-vault-secrets-and-loading-configuration-values-key-name-prefix-sample"></a>Mit Präfix schlüsseltresor. geheime Schlüssel erstellen und das Laden von Konfigurationswerten (Key-Name-Präfix-Beispiel)
`AddAzureKeyVault` bietet außerdem eine Überladung, eine Implementierung von akzeptiert `IKeyVaultSecretManager`, wodurch Sie steuern, wie Vault Geheimnisse Konfigurationsschlüssel umgewandelt werden. Beispielsweise können Sie die Schnittstelle, um geheime Werte basierend auf einem Präfixwert, die, den Sie beim Starten der app bereitstellen, laden implementieren. Dadurch können Sie z. B. um Geheimnisse anhand der Version der app geladen werden.

> [!WARNING]
> Verwenden Sie keine Präfixe auf schlüsseltresor geheime Schlüssel zum Ablegen geheime Schlüssel für mehrere apps im selben schlüsseltresor wird oder dass environmental geheime Schlüssel (z. B. *Entwicklung* im Vergleich zu *Produktion* geheime Schlüssel) in der gleichen Tresor. Es wird empfohlen, dass anderer apps und Entwicklung/produktionsumgebungen verwenden Sie separate Schlüssel Tresore zum Isolieren von app-Umgebungen für das höchste Maß an Sicherheit.

Verwenden das zweite Beispiel-app, einen geheimen Schlüssel im schlüsseltresor für erstellen `5000-AppSecret` (Punkte sind nicht zulässig, in der für den geheimen schlüsseltresornamen), ein app-Geheimnis für app-Version 5.0.0.0 darstellt. Für eine andere Version, 5.1.0.0, erstellen Sie einen geheimen Schlüssel für `5100-AppSecret`. Jede Version der app lädt einen eigenen geheimen Wert in seine Konfiguration als `AppSecret`, Ausblasegerät die Version, die den geheimen Schlüssel geladen. Das Beispiel für die Implementierung wird unten gezeigt:

[!code-csharp[Configuration builder](key-vault-configuration/samples/key-name-prefix-sample/2.x/Program.cs?name=snippet1&highlight=12)]

[!code-csharp[PrefixKeyVaultSecretManager](key-vault-configuration/samples/key-name-prefix-sample/2.x/Startup.cs?name=snippet1)]

Die `Load` Methode wird aufgerufen, von einem anbieteralgorithmus, der die geheimen Tresor auf diejenigen zu suchen, die das Präfix Version haben durchläuft. Wenn ein Präfix Version gefunden wird, mit `Load`, verwendet der Algorithmus die `GetKey` Methode, um den Konfigurationsnamen der Namen des geheimen Schlüssels zurückzugeben. Das Präfix "Version" aus den geheimen Namen entfernt, und gibt den Rest der Namen des geheimen Schlüssels für das Laden in das app-Konfiguration Name / Wert-Paaren.

Wenn Sie diesen Ansatz zu implementieren:

1. Die geheimen schlüsseltresor werden geladen.
2. Die Zeichenfolge geheimen Schlüssel für `5000-AppSecret` abgeglichen wird.
3. Die Version `5000` (mit Bindestrich), aus dem Schlüsselnamen verlassen entfernt `AppSecret` , mit dem Wert für den geheimen in der app-Konfiguration zu laden.

> [!NOTE]
> Eigene bieten `KeyVaultClient` Implementierung `AddAzureKeyVault`. Angeben eines benutzerdefinierten Clients, ermöglicht Ihnen, eine einzelne Instanz des Clients zwischen den Konfigurationsanbieter und anderer Teile Ihrer app zu verwenden.

1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die Anwendung gemäß der Anleitung in [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Fügen Sie geheime Schlüssel mit dem schlüsseltresor die [AzureRM Key Vault-PowerShell-Modul](/powershell/module/azurerm.keyvault) verfügbar der [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.KeyVault), die [REST-API von Azure Key Vault](/rest/api/keyvault/), oder die [Azure-Portal](https://portal.azure.com/). Geheime Schlüssel werden erstellt, entweder als *manuell* oder *Zertifikat* geheime Schlüssel. *Zertifikat* Geheimnisse sind Zertifikate für apps und Dienste jedoch nicht von den Konfigurationsanbieter unterstützt. Verwenden Sie die *manuell* Option zum Erstellen von Name / Wert-Paar geheime Schlüssel für die Verwendung mit den Konfigurationsanbieter.
     * Verwenden Sie hierarchische Werte (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen.
     * Erstellen Sie zwei *manuell* geheime Schlüssel mit den folgenden Name / Wert-Paaren:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrieren der Beispiel-app bei Azure Active Directory.
   * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` PowerShell-Cmdlet zum Autorisieren der app, Zugriff auf den schlüsseltresor, bieten `List` und `Get` Zugriff auf geheime Schlüssel mit `-PermissionsToSecrets list,get`.

2. Aktualisieren Sie der app *appsettings.json* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, das erhält seine Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den Namen des mit Präfix geheimen Schlüssels. In diesem Beispiel wird das Präfix der app-Version, die Sie zur Verfügung gestellt wird die `PrefixKeyVaultSecretManager` beim Hinzufügen von Azure Key Vault-Konfigurationsanbieter. Der Wert für `AppSecret` abgerufen wird, mit `config["AppSecret"]`. Die Webseite, die von der app generiert wurde, zeigt den geladenen Wert:

   ![Für den geheimen Werte über Azure Key Vault-Konfigurationsanbieter geladen werden, wenn die app-Version 5.0.0.0 ist Browserfenster](key-vault-configuration/_static/sample2-1.png)

4. Ändern Sie die Version der app-Assembly in der Projektdatei von `5.0.0.0` auf `5.1.0.0` und führen Sie die app erneut aus. Dieses Mal die geheime zurückgegebene Wert ist `5.1.0.0_secret_value`. Die Webseite, die von der app generiert wurde, zeigt den geladenen Wert:

   ![Browser-Fenster für den geheimen Werte über Azure Key Vault-Konfigurationsanbieter geladen werden, wenn die app-Version 5.1.0.0 ist](key-vault-configuration/_static/sample2-2.png)

## <a name="controlling-access-to-the-clientsecret"></a>Steuern des Zugriffs auf die ClientSecret
Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Verwalten der `ClientSecret` außerhalb der Quellstruktur Projekt. Mit dem geheimen Schlüssel-Manager, die Sie ein bestimmtes Projekt geheime app-Schlüssel zuordnen und diese projektübergreifend freigeben.

Beim Entwickeln von einer .NET Framework-Apps in einer Umgebung, die Zertifikate unterstützt, können Sie mit einem x. 509-Zertifikat in den Azure Key Vault authentifizieren. Private Schlüssel des x. 509-Zertifikats wird vom Betriebssystem verwaltet. Weitere Informationen finden Sie unter [authentifizieren mit einem Zertifikat anstatt eines geheimen Clientschlüssels](https://docs.microsoft.com/azure/key-vault/key-vault-use-from-web-application#authenticate-with-a-certificate-instead-of-a-client-secret). Verwenden der `AddAzureKeyVault` Überladung, akzeptiert eine `X509Certificate2`.

```csharp
var store = new X509Store(StoreLocation.CurrentUser);
store.Open(OpenFlags.ReadOnly);
var cert = store.Certificates.Find(X509FindType.FindByThumbprint, config["CertificateThumbprint"], false);

builder.AddAzureKeyVault(
    config["Vault"],
    config["ClientId"],
    cert.OfType<X509Certificate2>().Single(),
    new EnvironmentSecretManager(env.ApplicationName));
store.Close();

Configuration = builder.Build();
```

## <a name="reloading-secrets"></a>Erneutes Laden der geheime Schlüssel
Geheime Schlüssel werden zwischengespeichert, bis `IConfigurationRoot.Reload()` aufgerufen wird. Abgelaufen ist, wird deaktiviert, und die aktualisierte geheime Schlüssel im schlüsseltresor werden nicht eingehalten wird, von der Anwendung bis `Reload` ausgeführt wird.

```csharp
Configuration.Reload();
```

## <a name="disabled-and-expired-secrets"></a>Deaktiviert und abgelaufene Kennwörter
Deaktiviert und abgelaufene Kennwörter Auslösen einer `KeyVaultClientException`. Um zu verhindern, dass Ihre app auslösen, ersetzen Sie Ihre app oder aktualisieren Sie den geheimen deaktiviert/Ablauffrist.

## <a name="troubleshooting"></a>Problembehandlung
Wenn die Anwendung ein Fehler beim Laden der Konfiguration mithilfe des Anbieters auftritt, wird eine Fehlermeldung geschrieben, auf die [ASP.NET Protokollierung Infrastruktur](xref:fundamentals/logging/index). Die folgenden Bedingungen verhindert die Konfiguration geladen:
* Die app nicht korrekt in Azure Active Directory konfiguriert.
* Der schlüsseltresor existiert nicht in Azure Key Vault.
* Die app ist nicht autorisiert, auf den schlüsseltresor zugreifen.
* Die Zugriffsrichtlinie ist nicht enthalten `Get` und `List` Berechtigungen.
* Im schlüsseltresor ist die Konfigurationsdaten (Name / Wert-Paar) falsch benannt fehlt, deaktiviert oder abgelaufen.
* Die app hat den falschen schlüsseltresornamen (`Vault`), Azure AD-App-Id (`ClientId`), oder Azure AD-Schlüssel (`ClientSecret`).
* Der Azure AD-Schlüssel (`ClientSecret`) ist abgelaufen.
* Der Konfigurationsschlüssel (Name) ist falsch, in der app für den Wert, den Sie laden möchten.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Konfiguration](xref:fundamentals/configuration/index)
* [Microsoft Azure: Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Microsoft Azure: Key Vault-Dokumentation](https://docs.microsoft.com/azure/key-vault/)
* [Zum Generieren und Übertragen von HSM-geschützten Schlüsseln für Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys)
* [KeyVaultClient-Klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault.keyvaultclient)
