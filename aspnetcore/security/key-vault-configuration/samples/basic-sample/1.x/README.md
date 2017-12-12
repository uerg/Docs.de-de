# <a name="key-vault-configuration-provider-sample-application-aspnet-core-1x"></a>Key Vault-Konfigurationsanbieter-beispielanwendung (ASP.NET Core 1.x)

Dieses Beispiel veranschaulicht die Verwendung von Azure Key Vault-Konfigurationsanbieter für ASP.NET Core 1.x. Das ASP.NET Core 2.x-Beispiel finden Sie unter [Konfigurationsanbieter für Key Vault-beispielanwendung (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/basic-sample/2.x).

> [!NOTE]
> Der Konfigurationsanbieter ist nicht verfügbar für ASP.NET Core 1.0. Wenn Sie den Konfigurationsanbieter implementieren möchten, und die app eine ASP.NET Core 1.0-app ist, aktualisieren Sie die app auf 1.1 oder höher zuerst.

Weitere Informationen zur Funktionsweise des Beispiels finden Sie unter der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) Thema.

## <a name="using-the-sample"></a>Verwenden des Beispiels
1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die Anwendung gemäß der Anleitung in [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
  * Fügen Sie geheime Schlüssel mit dem schlüsseltresor die [AzureRM Key Vault-PowerShell-Modul](/powershell/module/azurerm.keyvault) verfügbar der [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM.KeyVault), die [REST-API von Azure Key Vault](/rest/api/keyvault/), oder die [Azure-Portal](https://portal.azure.com/). Geheime Schlüssel werden erstellt, entweder als *manuell* oder *Zertifikat* geheime Schlüssel. *Zertifikat* geheime Schlüssel sind die Zertifikate für die Verwendung durch apps und Dienste, aber werden nicht unterstützt, indem Sie den Konfigurationsanbieter. Verwenden Sie die *manuell* Option zum Erstellen von Name / Wert-Paar geheime Schlüssel für die Verwendung mit den Konfigurationsanbieter.
    * Einfache Kennwörter werden als Name / Wert-Paare erstellt. Azure Key Vault geheime Namen sind nur alphanumerische Zeichen und Bindestriche enthalten.
    * Verwenden Sie hierarchische Werte (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen in der Stichprobe. Doppelpunkte, die normalerweise, zur Begrenzung von eines Abschnitts über einen Unterschlüssel in verwendet werden [ASP.NET kernkonfiguration](xref:fundamentals/configuration/index), für den geheimen Namen sind nicht zulässig. Aus diesem Grund sind zwei Bindestriche verwendet und für einen Doppelpunkt ausgetauscht werden, wenn der geheime Schlüssel in der app-Konfiguration geladen werden.
    * Erstellen Sie zwei *manuell* geheime Schlüssel mit den folgenden Name / Wert-Paaren. Der erste geheime Schlüssel ist ein einfacher Name und Wert und der zweiten geheimen Schlüssel erstellt einen geheimen Wert mit einem Abschnitt und der Unterschlüssel im Namen geheimen Schlüssels:
      * `SecretName`: `secret_value_1`
      * `Section--SecretName`: `secret_value_2`
  * Registrieren der Beispiel-app bei Azure Active Directory.
  * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` PowerShell-Cmdlet zum Autorisieren der app, Zugriff auf den schlüsseltresor, bieten `List` und `Get` Zugriff auf geheime Schlüssel mit `-PermissionsToKeys list,get`.
2. Aktualisieren Sie der app *appsettings.json* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, das erhält seine Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den Namen des geheimen Schlüssels.
  * Nicht-hierarchischen Werten: der Wert für `SecretName` abgerufen wird, mit `config["SecretName"]`.
  * Hierarchische Werte (Abschnitte): Verwendung `:` (Doppelpunkt)-Notation oder `GetSection` Erweiterungsmethode. Verwenden Sie einen dieser Ansätze, um den Konfigurationswert zu erhalten:
    * `config["Section:SecretName"]`
    * `config.GetSection("Section")["SecretName"]`
