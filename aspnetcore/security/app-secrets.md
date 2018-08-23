---
title: Sichere Speicherung von app-Geheimnissen bei der Entwicklung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Speichern und Abrufen von vertraulichen Informationen wie app-Geheimnissen während der Entwicklung von ASP.NET Core-Apps.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2018
ms.locfileid: "41836258"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="1997e-103">Sichere Speicherung von app-Geheimnissen bei der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1997e-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="1997e-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="1997e-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="1997e-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1997e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1997e-106">Dieses Dokument erläutert die Verfahren zum Speichern und Abrufen von sensiblen Daten während der Entwicklung von ASP.NET Core-Apps.</span><span class="sxs-lookup"><span data-stu-id="1997e-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="1997e-107">Sie sollten niemals Kennwörter oder andere sensiblen Daten im Quellcode speichern, und Sie sollte nicht verwenden produktionsgeheimnisse in Entwicklungs- oder test-Modus.</span><span class="sxs-lookup"><span data-stu-id="1997e-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="1997e-108">Sie können Azure-Test- und -Produktionsgeheimnisse mit dem [Konfigurationsanbieter Azure Key Vault](xref:security/key-vault-configuration) speichern und schützen.</span><span class="sxs-lookup"><span data-stu-id="1997e-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="1997e-109">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="1997e-109">Environment variables</span></span>

<span data-ttu-id="1997e-110">Umgebungsvariablen werden zum Speichern von app-Geheimnisse in Code oder in lokalen Konfigurationsdateien zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="1997e-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="1997e-111">Umgebungsvariablen Überschreiben von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen.</span><span class="sxs-lookup"><span data-stu-id="1997e-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1997e-112">Konfigurieren Sie das Lesen der Werte von Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="1997e-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="1997e-113">Erwägen Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit ist aktiviert.</span><span class="sxs-lookup"><span data-stu-id="1997e-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="1997e-114">Ist eine standardmäßige Datenbank-Verbindungszeichenfolge des Projekts enthalten *"appSettings.JSON"* Datei mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="1997e-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="1997e-115">Die Standardverbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="1997e-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="1997e-116">Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert für eine Umgebungsvariable.</span><span class="sxs-lookup"><span data-stu-id="1997e-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="1997e-117">Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="1997e-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="1997e-118">Umgebungsvariablen werden in der Regel in einfachen, unverschlüsselten Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1997e-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="1997e-119">Wenn der Computer oder Prozess gefährdet ist, können die Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="1997e-120">Zusätzliche Maßnahmen zum Verhindern der Offenlegung von geheimen Benutzerschlüssel können erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="1997e-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="1997e-121">Geheimnis-Manager</span><span class="sxs-lookup"><span data-stu-id="1997e-121">Secret Manager</span></span>

<span data-ttu-id="1997e-122">Secret Manager-Tool werden sensible Daten gespeichert, während der Entwicklung von einem ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="1997e-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="1997e-123">In diesem Kontext ist ein Stück von sensiblen Daten ein app-Geheimnis an.</span><span class="sxs-lookup"><span data-stu-id="1997e-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="1997e-124">App-Geheimnisse werden in einem gesonderten Speicherort aus der Projektstruktur gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1997e-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="1997e-125">Die app-Geheimnisse sind einem bestimmten Projekt zugeordnet oder für mehrere Projekte freigegeben.</span><span class="sxs-lookup"><span data-stu-id="1997e-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="1997e-126">Die app-Geheimnisse sind nicht in quellcodeverwaltung eingecheckt.</span><span class="sxs-lookup"><span data-stu-id="1997e-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="1997e-127">Secret Manager-Tool nicht zum Verschlüsseln der vertraulichen Daten und sollte nicht als vertrauenswürdiger Speicher behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="1997e-128">Es ist nur zu Entwicklungszwecken.</span><span class="sxs-lookup"><span data-stu-id="1997e-128">It's for development purposes only.</span></span> <span data-ttu-id="1997e-129">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Benutzerprofilverzeichnis gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1997e-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="1997e-130">Wie funktioniert das Secret Manager-tool</span><span class="sxs-lookup"><span data-stu-id="1997e-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="1997e-131">Secret Manager-Tool abstrahiert die Implementierungsdetails wie, wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="1997e-132">Sie können das Tool verwenden, ohne zu wissen, diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="1997e-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="1997e-133">Die Werte werden in einer JSON-Konfigurationsdatei in eine System-geschützten Benutzerprofilordner auf dem lokalen Computer gespeichert:</span><span class="sxs-lookup"><span data-stu-id="1997e-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1997e-134">Windows</span><span class="sxs-lookup"><span data-stu-id="1997e-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="1997e-135">System-Dateipfad:</span><span class="sxs-lookup"><span data-stu-id="1997e-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="1997e-136">macOS</span><span class="sxs-lookup"><span data-stu-id="1997e-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="1997e-137">System-Dateipfad:</span><span class="sxs-lookup"><span data-stu-id="1997e-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="1997e-138">Linux</span><span class="sxs-lookup"><span data-stu-id="1997e-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="1997e-139">System-Dateipfad:</span><span class="sxs-lookup"><span data-stu-id="1997e-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="1997e-140">Klicken Sie in der vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="1997e-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="1997e-141">Schreiben Sie Code, von denen abhängig von der Speicherort oder das Format der Daten gespeichert werden, mit dem Geheimnis-Manager-Tool nicht.</span><span class="sxs-lookup"><span data-stu-id="1997e-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="1997e-142">Details dieser Implementierung können sich ändern.</span><span class="sxs-lookup"><span data-stu-id="1997e-142">These implementation details may change.</span></span> <span data-ttu-id="1997e-143">Z. B. die geheimen Werte werden nicht verschlüsselt, aber konnte nicht in der Zukunft.</span><span class="sxs-lookup"><span data-stu-id="1997e-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="1997e-144">Installieren Sie das Geheimnis-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="1997e-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="1997e-145">Secret Manager-Tool ist im Paket mit .NET Core-CLI in .NET Core SDK 2.1.300 oder höher.</span><span class="sxs-lookup"><span data-stu-id="1997e-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="1997e-146">Für .NET Core SDK-Versionen vor 2.1.300 ist ein Tool-Installationsordner erforderlich.</span><span class="sxs-lookup"><span data-stu-id="1997e-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="1997e-147">Führen Sie `dotnet --version` über eine Befehlsshell, die Anzahl der installierten .NET Core SDK-Version finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="1997e-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="1997e-148">Wenn das .NET Core SDK verwendet das Tool enthält, wird eine Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1997e-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="1997e-149">Installieren Sie die [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket in Ihre ASP.NET Core-Projekt.</span><span class="sxs-lookup"><span data-stu-id="1997e-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="1997e-150">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1997e-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="1997e-151">Führen Sie den folgenden Befehl in einer Befehlsshell zum Überprüfen der Tool-Installationsordner:</span><span class="sxs-lookup"><span data-stu-id="1997e-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="1997e-152">Secret Manager-Tool zeigt das Beispiel für die Verwendung, Optionen und Hilfe zu dem Befehl:</span><span class="sxs-lookup"><span data-stu-id="1997e-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="1997e-153">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei ist zum Ausführen des Tools, die definiert, der *csproj* Datei `DotNetCliToolReference` Elemente.</span><span class="sxs-lookup"><span data-stu-id="1997e-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="1997e-154">Legen Sie einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1997e-154">Set a secret</span></span>

<span data-ttu-id="1997e-155">Secret Manager-Tool wirkt sich auf projektspezifische Konfigurationseinstellungen, die in Ihrem Benutzerprofil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1997e-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="1997e-156">Um vertrauliche Informationen eines Benutzers zu verwenden, definieren eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="1997e-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="1997e-157">Der Wert des `UserSecretsId` ist beliebig, aber in dem Projekt eindeutig ist.</span><span class="sxs-lookup"><span data-stu-id="1997e-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="1997e-158">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="1997e-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="1997e-159">Klicken Sie in Visual Studio mit der rechten Maustaste in des Projekts im Projektmappen-Explorer, und wählen **geheime Benutzerschlüssel verwalten** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="1997e-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="1997e-160">Diese stiftbewegung Fügt eine `UserSecretsId` Elements aufgefüllt durch eine GUID, zu der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="1997e-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="1997e-161">Visual Studio öffnet eine *secrets.json* Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="1997e-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="1997e-162">Ersetzen Sie den Inhalt der *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="1997e-163">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1997e-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="1997e-164">Die JSON-Struktur wird nach Änderungen über vereinfacht `dotnet user-secrets remove` oder `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="1997e-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="1997e-165">Z. B. Ausführung `dotnet user-secrets remove "Movies:ConnectionString"` reduziert die `Movies` Objektliteral.</span><span class="sxs-lookup"><span data-stu-id="1997e-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="1997e-166">Die geänderte Datei ähnelt der folgenden:</span><span class="sxs-lookup"><span data-stu-id="1997e-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="1997e-167">Definieren Sie ein app-Geheimnis, bestehend aus einem Schlüssel und seinen Wert an.</span><span class="sxs-lookup"><span data-stu-id="1997e-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="1997e-168">Der geheime Schlüssel des Projekts zugeordnet ist `UserSecretsId` Wert.</span><span class="sxs-lookup"><span data-stu-id="1997e-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="1997e-169">Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="1997e-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="1997e-170">Im vorherigen Beispiel ist der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt als literal mit einem `ServiceApiKey` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="1997e-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="1997e-171">Secret Manager-Tool kann auch aus anderen Verzeichnissen verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="1997e-172">Verwenden der `--project` Option aus, um den Dateisystempfad an dem Angeben der *csproj* Datei vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="1997e-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="1997e-173">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1997e-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="1997e-174">Legen Sie mehrere geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1997e-174">Set multiple secrets</span></span>

<span data-ttu-id="1997e-175">Ein Batch von geheimen Schlüsseln kann festgelegt werden, durch das Weiterleiten von JSON-Code für die `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="1997e-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="1997e-176">Im folgenden Beispiel die *"Input.JSON"* Dateiinhalt sind über die Pipeline an das `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="1997e-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1997e-177">Windows</span><span class="sxs-lookup"><span data-stu-id="1997e-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="1997e-178">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="1997e-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="1997e-179">macOS</span><span class="sxs-lookup"><span data-stu-id="1997e-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="1997e-180">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="1997e-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="1997e-181">Linux</span><span class="sxs-lookup"><span data-stu-id="1997e-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="1997e-182">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="1997e-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="1997e-183">Zugriff auf einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1997e-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="1997e-184">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf Secret Manager Geheimnisse.</span><span class="sxs-lookup"><span data-stu-id="1997e-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="1997e-185">Wenn das Projekt .NET Framework verwendet, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="1997e-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="1997e-186">In ASP.NET Core 2.0 oder höher, die Benutzer Geheimnisse Konfigurationsquelle wird automatisch hinzugefügt im Entwicklungsmodus Wenn, das Projekt aufruft ["createdefaultbuilder"](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) eine neue Instanz des Hosts mit vorkonfigurierten Standardwerten initialisiert werden.</span><span class="sxs-lookup"><span data-stu-id="1997e-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="1997e-187">`CreateDefaultBuilder` Aufrufe [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) bei der [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) ist [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="1997e-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="1997e-188">Wenn `CreateDefaultBuilder` wird nicht aufgerufen wird, während der Erstellung des Hosts, hinzufügen die Konfigurationsquelle für Benutzer geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="1997e-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="1997e-189">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf Secret Manager Geheimnisse.</span><span class="sxs-lookup"><span data-stu-id="1997e-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="1997e-190">Installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="1997e-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="1997e-191">Hinzufügen die Konfigurationsquelle für Benutzer geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="1997e-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="1997e-192">Vertrauliche Informationen eines Benutzers abgerufen werden können, über die `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="1997e-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="1997e-193">Map-Geheimtipps für die ein POCO-Objekt</span><span class="sxs-lookup"><span data-stu-id="1997e-193">Map secrets to a POCO</span></span>

<span data-ttu-id="1997e-194">Zuordnen eines kompletten Objekts literal, ein POCO-Objekt (eine einfache .NET Klasse mit Eigenschaften) eignet sich für das Aggregieren von verwandten Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="1997e-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1997e-195">Verwenden, um die vorherigen geheimen Schlüssel ein POCO-Objekt zugeordnet sind, die `Configuration` APIs [Objekt Graph Bindung](xref:fundamentals/configuration/index#bind-to-an-object-graph) Feature.</span><span class="sxs-lookup"><span data-stu-id="1997e-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="1997e-196">Der folgende Code bindet an einen benutzerdefinierten `MovieSettings` POCO und greift auf die `ServiceApiKey` Eigenschaftswert:</span><span class="sxs-lookup"><span data-stu-id="1997e-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="1997e-197">Die `Movies:ConnectionString` und `Movies:ServiceApiKey` geheime Schlüssel werden in den entsprechenden Eigenschaften im zugeordnet `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="1997e-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="1997e-198">Zeichenfolgenersetzungen mit geheimen Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="1997e-198">String replacement with secrets</span></span>

<span data-ttu-id="1997e-199">Speichern von Kennwörtern als nur-Text ist unsicher.</span><span class="sxs-lookup"><span data-stu-id="1997e-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="1997e-200">Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *"appSettings.JSON"* kann ein Kennwort für den angegebenen Benutzer enthalten:</span><span class="sxs-lookup"><span data-stu-id="1997e-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="1997e-201">Ein sicherer Ansatz ist das Kennwort als Geheimnis gespeichert.</span><span class="sxs-lookup"><span data-stu-id="1997e-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="1997e-202">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1997e-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="1997e-203">Entfernen Sie die `Password` Schlüssel / Wert-Paar aus der Verbindungszeichenfolge in *"appSettings.JSON"*.</span><span class="sxs-lookup"><span data-stu-id="1997e-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="1997e-204">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1997e-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="1997e-205">Der geheime Schlüssel Wert kann festgelegt werden, auf eine [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) des Objekts [Kennwort](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) Eigenschaft, um die Verbindungszeichenfolge abgeschlossen:</span><span class="sxs-lookup"><span data-stu-id="1997e-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="1997e-206">Auflisten der Geheimnisse.</span><span class="sxs-lookup"><span data-stu-id="1997e-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1997e-207">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="1997e-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="1997e-208">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1997e-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="1997e-209">Im vorherigen Beispiel, bezeichnet ein Doppelpunkt in den Schlüsselnamen die Objekthierarchie in *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="1997e-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="1997e-210">Entfernen Sie einen einzelnen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="1997e-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1997e-211">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="1997e-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="1997e-212">Der app *secrets.json* Datei wurde geändert, um das zugeordnete Schlüssel-Wert-Paar Entfernen der `MoviesConnectionString` Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="1997e-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="1997e-213">Ausführung `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1997e-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="1997e-214">Entfernen Sie alle Geheimnisse</span><span class="sxs-lookup"><span data-stu-id="1997e-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="1997e-215">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="1997e-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="1997e-216">Alle benutzergeheimnisse für die app aus gelöscht wurden die *secrets.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="1997e-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="1997e-217">Ausführung `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="1997e-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="1997e-218">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="1997e-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
