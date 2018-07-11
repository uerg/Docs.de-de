---
title: Sichere Speicherung von app-Geheimnissen bei der Entwicklung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Speichern und Abrufen von vertraulichen Informationen wie app-Geheimnissen während der Entwicklung von ASP.NET Core-Apps.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38126910"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Sichere Speicherung von app-Geheimnissen bei der Entwicklung in ASP.NET Core

Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Dieses Dokument erläutert die Verfahren zum Speichern und Abrufen von sensiblen Daten während der Entwicklung von ASP.NET Core-Apps. Sie sollten niemals Kennwörter oder andere sensiblen Daten im Quellcode speichern, und Sie sollte nicht verwenden produktionsgeheimnisse in Entwicklungs- oder test-Modus. Sie können Azure-Test- und -Produktionsgeheimnisse mit dem [Konfigurationsanbieter Azure Key Vault](xref:security/key-vault-configuration) speichern und schützen.

## <a name="environment-variables"></a>Umgebungsvariablen

Umgebungsvariablen werden zum Speichern von app-Geheimnisse in Code oder in lokalen Konfigurationsdateien zu vermeiden. Umgebungsvariablen Überschreiben von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen.

::: moniker range="<= aspnetcore-1.1"
Konfigurieren Sie das Lesen der Werte von Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Erwägen Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit ist aktiviert. Ist eine standardmäßige Datenbank-Verbindungszeichenfolge des Projekts enthalten *"appSettings.JSON"* Datei mit dem Schlüssel `DefaultConnection`. Die Standardverbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert. Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert für eine Umgebungsvariable. Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.

> [!WARNING]
> Umgebungsvariablen werden in der Regel in einfachen, unverschlüsselten Text gespeichert. Wenn der Computer oder Prozess gefährdet ist, können die Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden. Zusätzliche Maßnahmen zum Verhindern der Offenlegung von geheimen Benutzerschlüssel können erforderlich sein.

## <a name="secret-manager"></a>Geheimnis-Manager

Secret Manager-Tool werden sensible Daten gespeichert, während der Entwicklung von einem ASP.NET Core-Projekt. In diesem Kontext ist ein Stück von sensiblen Daten ein app-Geheimnis an. App-Geheimnisse werden in einem gesonderten Speicherort aus der Projektstruktur gespeichert. Die app-Geheimnisse sind einem bestimmten Projekt zugeordnet oder für mehrere Projekte freigegeben. Die app-Geheimnisse sind nicht in quellcodeverwaltung eingecheckt.

> [!WARNING]
> Secret Manager-Tool nicht zum Verschlüsseln der vertraulichen Daten und sollte nicht als vertrauenswürdiger Speicher behandelt werden. Es ist nur zu Entwicklungszwecken. Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Benutzerprofilverzeichnis gespeichert.

## <a name="how-the-secret-manager-tool-works"></a>Wie funktioniert das Secret Manager-tool

Secret Manager-Tool abstrahiert die Implementierungsdetails wie, wo und wie die Werte gespeichert werden. Sie können das Tool verwenden, ohne zu wissen, diese Implementierungsdetails. Die Werte werden in einer JSON-Konfigurationsdatei in eine System-geschützten Benutzerprofilordner auf dem lokalen Computer gespeichert:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

System-Dateipfad:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

System-Dateipfad:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

System-Dateipfad:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Klicken Sie in der vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.

Schreiben Sie Code, von denen abhängig von der Speicherort oder das Format der Daten gespeichert werden, mit dem Geheimnis-Manager-Tool nicht. Details dieser Implementierung können sich ändern. Z. B. die geheimen Werte werden nicht verschlüsselt, aber konnte nicht in der Zukunft.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Installieren Sie das Geheimnis-Manager-tool

Secret Manager-Tool wird mit der .NET Core-CLI ab .NET Core SDK 2.1.300 gebündelt. Für .NET Core SDK-Versionen vor 2.1.300 ist ein Tool-Installationsordner erforderlich.

> [!TIP]
> Führen Sie `dotnet --version` über eine Befehlsshell, die Anzahl der installierten .NET Core SDK-Version finden Sie unter.

Wenn das .NET Core SDK verwendet das Tool enthält, wird eine Warnung angezeigt:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Installieren Sie die [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket in Ihre ASP.NET Core-Projekt. Zum Beispiel:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Führen Sie den folgenden Befehl in einer Befehlsshell zum Überprüfen der Tool-Installationsordner:

```console
dotnet user-secrets -h
```

Secret Manager-Tool zeigt das Beispiel für die Verwendung, Optionen und Hilfe zu dem Befehl:

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei ist zum Ausführen des Tools, die definiert, der *csproj* Datei `DotNetCliToolReference` Elemente.
::: moniker-end

## <a name="set-a-secret"></a>Legen Sie einen geheimen Schlüssel

Secret Manager-Tool wirkt sich auf projektspezifische Konfigurationseinstellungen, die in Ihrem Benutzerprofil gespeichert. Um vertrauliche Informationen eines Benutzers zu verwenden, definieren eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei. Der Wert des `UserSecretsId` ist beliebig, aber in dem Projekt eindeutig ist. Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> Klicken Sie in Visual Studio mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen **geheime Benutzerschlüssel verwalten** aus dem Kontextmenü. Diese stiftbewegung Fügt eine `UserSecretsId` Elements aufgefüllt durch eine GUID, zu der *csproj* Datei. Visual Studio öffnet eine *secrets.json* Datei im Text-Editor. Ersetzen Sie den Inhalt der *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden. Beispiel: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Definieren Sie ein app-Geheimnis, bestehend aus einem Schlüssel und seinen Wert an. Der geheime Schlüssel des Projekts zugeordnet ist `UserSecretsId` Wert. Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Im vorherigen Beispiel ist der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt als literal mit einem `ServiceApiKey` Eigenschaft.

Secret Manager-Tool kann auch aus anderen Verzeichnissen verwendet werden. Verwenden der `--project` Option aus, um den Dateisystempfad an dem Angeben der *csproj* Datei vorhanden ist. Zum Beispiel:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Legen Sie mehrere geheime Schlüssel

Ein Batch von geheimen Schlüsseln kann festgelegt werden, durch das Weiterleiten von JSON-Code für die `set` Befehl. Im folgenden Beispiel die *"Input.JSON"* Dateiinhalt sind über die Pipeline an das `set` Befehl.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Zugriff auf einen geheimen Schlüssel

::: moniker range="<= aspnetcore-1.1"
Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf Secret Manager Geheimnisse. Installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.

Hinzufügen die Konfigurationsquelle für Benutzer geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf Secret Manager Geheimnisse. Wenn das Projekt .NET Framework verwendet, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.

In ASP.NET Core 2.0 oder höher, die Benutzer Geheimnisse Konfigurationsquelle wird automatisch hinzugefügt im Entwicklungsmodus Wenn, das Projekt aufruft ["createdefaultbuilder"](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) eine neue Instanz des Hosts mit vorkonfigurierten Standardwerten initialisiert werden. `CreateDefaultBuilder` Aufrufe [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) bei der [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) ist [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Wenn `CreateDefaultBuilder` wird nicht aufgerufen wird, während der Erstellung des Hosts, hinzufügen die Konfigurationsquelle für Benutzer geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Vertrauliche Informationen eines Benutzers abgerufen werden können, über die `Configuration` API:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Zeichenfolgenersetzungen mit geheimen Schlüsseln

Speichern von Kennwörtern als nur-Text ist unsicher. Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *"appSettings.JSON"* kann ein Kennwort für den angegebenen Benutzer enthalten:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Ein sicherer Ansatz ist das Kennwort als Geheimnis gespeichert. Zum Beispiel:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Entfernen Sie die `Password` Schlüssel / Wert-Paar aus der Verbindungszeichenfolge in *"appSettings.JSON"*. Zum Beispiel:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Der geheime Schlüssel Wert kann festgelegt werden, auf eine [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) des Objekts [Kennwort](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) Eigenschaft, um die Verbindungszeichenfolge abgeschlossen:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a>Auflisten der Geheimnisse.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets list
```

Die folgende Ausgabe wird angezeigt:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

Im vorherigen Beispiel, bezeichnet ein Doppelpunkt in den Schlüsselnamen die Objekthierarchie in *secrets.json*.

## <a name="remove-a-single-secret"></a>Entfernen Sie einen einzelnen geheimen Schlüssel

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Der app *secrets.json* Datei wurde geändert, um das zugeordnete Schlüssel-Wert-Paar Entfernen der `MoviesConnectionString` Schlüssel:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Ausführung `dotnet user-secrets list` wird die folgende Meldung angezeigt:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Entfernen Sie alle Geheimnisse

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:

```console
dotnet user-secrets clear
```

Alle benutzergeheimnisse für die app aus gelöscht wurden die *secrets.json* Datei:

```json
{}
```

Ausführung `dotnet user-secrets list` wird die folgende Meldung angezeigt:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
