# <a name="prefix-key-vault-configuration-provider-sample-application-aspnet-core-2x"></a>Präfix der Konfigurationsanbieter für Key Vault-beispielanwendung (ASP.NET Core 2.x)

Dieses Beispiel veranschaulicht die Verwendung von Azure Key Vault-Konfigurationsanbieter für ASP.NET Core 2.x Schlüsselname Präfixe verwenden. Das ASP.NET Core 1.x-Beispiel finden Sie unter [Key Vault-Konfigurationsanbieter Präfix-beispielanwendung (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/key-vault-configuration/samples/key-name-prefix-sample/1.x).

Weitere Informationen zur Funktionsweise des Beispiels finden Sie unter der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) Thema.

## <a name="using-the-sample"></a>Verwenden des Beispiels
1. Erstellen eines schlüsseltresors und Einrichten von Azure Active Directory (Azure AD) für die Anwendung gemäß der Anleitung in [erste Schritte mit Azure Key Vault](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).
   * Fügen Sie geheime Schlüssel für den schlüsseltresor verwenden das Azure-PowerShell-Modul, das Azure-Dienstverwaltungs-API oder der Azure-Verwaltungsportal hinzu. Geheime Schlüssel werden erstellt, entweder als *manuell* oder *Zertifikat* geheime Schlüssel. *Zertifikat* Geheimnisse sind Zertifikate für apps und Dienste jedoch nicht von den Konfigurationsanbieter unterstützt. Verwenden Sie die *manuell* Option zum Erstellen von Name / Wert-Paar geheime Schlüssel für die Verwendung mit den Konfigurationsanbieter.
     * Verwenden Sie hierarchische Werte (Konfigurationsabschnitte) `--` (zwei Bindestriche) als Trennzeichen.
     * Für die Beispiel-app erstellen Sie zwei *manuell* geheime Schlüssel mit den folgenden Name / Wert-Paaren:
       * `5000-AppSecret`: `5.0.0.0_secret_value`
       * `5100-AppSecret`: `5.1.0.0_secret_value`
   * Registrieren der Beispiel-app bei Azure Active Directory.
   * Autorisieren Sie die app auf den schlüsseltresor zugreifen. Bei Verwendung der `Set-AzureRmKeyVaultAccessPolicy` PowerShell-Cmdlet zum Autorisieren der app, Zugriff auf den schlüsseltresor, bieten `List` und `Get` Zugriff auf geheime Schlüssel mit `-PermissionsToSecrets list,get`.

2. Aktualisieren Sie der app *appsettings.json* Datei mit den Werten der `Vault`, `ClientId`, und `ClientSecret`.
3. Führen Sie die Beispielapp, das erhält seine Konfigurationswerte aus `IConfigurationRoot` mit dem gleichen Namen wie den Namen des mit Präfix geheimen Schlüssels. In diesem Beispiel wird das Präfix der app-Version, die Sie zur Verfügung gestellt wird die `PrefixKeyVaultSecretManager` beim Hinzufügen von Azure Key Vault-Konfigurationsanbieter. Der Wert für `AppSecret` abgerufen wird, mit `config["AppSecret"]`.
4. Ändern Sie die Version der app-Assembly in der Projektdatei von `5.0.0.0` auf `5.1.0.0` und führen Sie die app erneut aus. Dieses Mal die geheime zurückgegebene Wert ist `5.1.0.0_secret_value`.
