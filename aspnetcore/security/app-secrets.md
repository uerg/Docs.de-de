---
title: Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Speichern und Abrufen von vertraulichen Informationen als app geheime Schlüssel während der Entwicklung einer App ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 88b4ee9a963543f8cc97cb66271628a14fe657de
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="4a9c6-103">Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4a9c6-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="4a9c6-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="4a9c6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a9c6-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="4a9c6-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4a9c6-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="4a9c6-107">Dieses Dokument erläutert Techniken zum Speichern und Abrufen von sensiblen Daten während der Entwicklung einer App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="4a9c6-108">Sie sollten niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern, und Sie darf keine Produktion geheime Schlüssel in der Entwicklung oder Modus zu testen.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="4a9c6-109">Sie können speichern und Schützen von Azure geheime Schlüssel Test- und produktionsumgebungen mit der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="4a9c6-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="4a9c6-110">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="4a9c6-110">Environment variables</span></span>

<span data-ttu-id="4a9c6-111">Umgebungsvariablen werden zum Speichern von app-Kennwörter im Code oder in den lokalen Konfigurationsdateien zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="4a9c6-112">Umgebungsvariablen außer Kraft setzen von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen aus.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-113">Konfigurieren Sie das Lesen der Werte für Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="4a9c6-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="4a9c6-115">Betrachten Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="4a9c6-116">Eine Standard-Datenbank-Verbindungszeichenfolge wird in das Projekt aufgenommenes *appsettings.json* Datei mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="4a9c6-117">Die Standard-Verbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="4a9c6-118">Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="4a9c6-119">Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="4a9c6-120">Umgebungsvariablen werden im Allgemeinen in einfachen, unverschlüsselte Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="4a9c6-121">Wenn der Computer oder einen Prozess beschädigt ist, können der Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="4a9c6-122">Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise erforderlich.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="4a9c6-123">Geheime Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="4a9c6-123">Secret Manager</span></span>

<span data-ttu-id="4a9c6-124">Das Schlüssel-Manager-Tool speichert vertrauliche Daten während der Entwicklung eines Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="4a9c6-125">In diesem Kontext ist ein Teil der sensiblen Daten ein app-Geheimnis.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="4a9c6-126">App-Kennwörter werden in einem anderen Speicherort aus der Projektstruktur gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="4a9c6-127">Die app-Kennwörter werden mit einem bestimmten Projekt verknüpft ist oder mehreren Projekten gemeinsam.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="4a9c6-128">Die app-Kennwörter werden nicht in das Quellsteuerelement eingecheckt.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="4a9c6-129">Der geheime Schlüssel-Manager-Tool nicht die gespeicherten geheimen Schlüssel zum Verschlüsseln und sollte nicht als vertrauenswürdigen Speicher behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="4a9c6-130">Es ist nur für Entwicklungszwecke.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-130">It's for development purposes only.</span></span> <span data-ttu-id="4a9c6-131">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="4a9c6-132">Wie funktioniert das Schlüssel-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="4a9c6-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="4a9c6-133">Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="4a9c6-134">Sie können das Tool verwenden, ohne diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="4a9c6-135">Die Werte werden gespeichert, einem [JSON](https://json.org/) Konfigurationsdatei in einem System geschütztem Benutzerprofilordner auf dem lokalen Computer:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4a9c6-136">Windows</span><span class="sxs-lookup"><span data-stu-id="4a9c6-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="4a9c6-137">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="4a9c6-138">macOS</span><span class="sxs-lookup"><span data-stu-id="4a9c6-138">macOS</span></span>](#tab/macos)

<span data-ttu-id="4a9c6-139">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="4a9c6-140">Linux</span><span class="sxs-lookup"><span data-stu-id="4a9c6-140">Linux</span></span>](#tab/linux)

<span data-ttu-id="4a9c6-141">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-141">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="4a9c6-142">Klicken Sie in den vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-142">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="4a9c6-143">Schreiben Sie Code, der abhängig von der Speicherort oder das Format der Daten, die mit dem geheimen Schlüssel-Manager-Tool gespeichert nicht.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-143">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="4a9c6-144">Diese Implementierungsdetails können geändert werden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-144">These implementation details may change.</span></span> <span data-ttu-id="4a9c6-145">Z. B. die Werte für den geheimen werden nicht verschlüsselt, aber in der Zukunft werden konnte.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-145">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="4a9c6-146">Installieren Sie den geheimen Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="4a9c6-146">Install the Secret Manager tool</span></span>

<span data-ttu-id="4a9c6-147">Das Schlüssel-Manager-Tool wird mit der .NET Core-CLI in .NET Core SDK 2.1 gebündelt.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-147">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="4a9c6-148">Für .NET Core SDK 2.0 und früheren Versionen ist das Tool-Installationsordner erforderlich.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-148">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="4a9c6-149">Installieren der [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket im Projekt ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="4a9c6-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-150">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="4a9c6-151">Führen Sie den folgenden Befehl in eine Befehlsshell beim Überprüfen der Installation der Tools:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="4a9c6-152">Das Schlüssel-Manager-Tool zeigt die Verwendung des Beispiels, der Optionen und der Befehl Hilfe:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="4a9c6-153">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Elemente.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="4a9c6-154">Legen Sie einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-154">Set a secret</span></span>

<span data-ttu-id="4a9c6-155">Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="4a9c6-156">Definieren, um Benutzer geheime Schlüssel zu verwenden, eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="4a9c6-157">Der Wert des `UserSecretsId` ist willkürlich, jedoch ist charakteristisch für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="4a9c6-158">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-159">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="4a9c6-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-160">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="4a9c6-161">Klicken Sie in Visual Studio mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-161">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="4a9c6-162">Fügt dieser Bewegung eine `UserSecretsId` -Element, mit einer GUID, die aufgefüllt sind, zu der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-162">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="4a9c6-163">Visual Studio öffnet eine *secrets.json* Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-163">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="4a9c6-164">Ersetzen Sie den Inhalt des *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-164">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="4a9c6-165">Beispiel: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-165">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="4a9c6-166">Definieren Sie ein app-Geheimnis bestehend aus einem Schlüssel und seinen Wert an.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="4a9c6-167">Der geheime Schlüssel ist mit des Projekts zugeordneten `UserSecretsId` Wert.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="4a9c6-168">Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="4a9c6-169">Im vorherigen Beispiel der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt mit Literalen eine `ServiceApiKey` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="4a9c6-170">Das Schlüssel-Manager-Tool kann von anderen Verzeichnissen zu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="4a9c6-171">Verwenden der `--project` Option aus, um den Dateisystempfad zu dem Angeben der *csproj* Datei vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="4a9c6-172">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-172">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="4a9c6-173">Legen Sie mehrere geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-173">Set multiple secrets</span></span>

<span data-ttu-id="4a9c6-174">Ein Batch von geheimen Schlüsseln kann festgelegt werden, indem piping JSON in der `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-174">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="4a9c6-175">Im folgenden Beispiel die *input.json* Dateiinhalt sind über die Pipeline an das `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-175">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="4a9c6-176">Windows</span><span class="sxs-lookup"><span data-stu-id="4a9c6-176">Windows</span></span>](#tab/windows)

<span data-ttu-id="4a9c6-177">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-177">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="4a9c6-178">macOS</span><span class="sxs-lookup"><span data-stu-id="4a9c6-178">macOS</span></span>](#tab/macos)

<span data-ttu-id="4a9c6-179">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-179">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="4a9c6-180">Linux</span><span class="sxs-lookup"><span data-stu-id="4a9c6-180">Linux</span></span>](#tab/linux)

<span data-ttu-id="4a9c6-181">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="4a9c6-182">Zugriff auf einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-182">Access a secret</span></span>

<span data-ttu-id="4a9c6-183">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-183">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="4a9c6-184">Wenn .NET Core als Ziel dient 1.x oder .NET Framework, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-184">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-185">Hinzufügen der Benutzer geheime Schlüssel Konfigurationsquelle angibt, die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-185">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="4a9c6-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-186">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="4a9c6-187">Vertrauliche Benutzerdaten abgerufen werden können, über die `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-187">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-188">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="4a9c6-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-189">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="4a9c6-190">Zeichenfolgenersetzungen mit geheimen Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="4a9c6-190">String replacement with secrets</span></span>

<span data-ttu-id="4a9c6-191">Es ist riskant, Speichern von Kennwörtern als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-191">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="4a9c6-192">Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *appsettings.json* eventuell ein Kennwort für den angegebenen Benutzer:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-192">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="4a9c6-193">Ein sicherer Ansatz besteht darin, das Kennwort als ein geheimer Schlüssel zu speichern.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-193">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="4a9c6-194">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-194">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="4a9c6-195">Ersetzen Sie das Kennwort in *appsettings.json* durch einen Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-195">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="4a9c6-196">Im folgenden Beispiel `{0}` dient als Platzhalter in Form einer [zusammengesetzte Formatzeichenfolge](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="4a9c6-196">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="4a9c6-197">Der Schlüssel-Wert kann in den Platzhalter zum Abschließen der Verbindungszeichenfolge eingegeben werden:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-197">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="4a9c6-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-198">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="4a9c6-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="4a9c6-199">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="4a9c6-200">Liste der geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-200">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4a9c6-201">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-201">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="4a9c6-202">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-202">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="4a9c6-203">Im vorherigen Beispiel kennzeichnet ein Doppelpunkt in den Schlüsselnamen in die Objekthierarchie *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="4a9c6-203">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="4a9c6-204">Entfernen Sie einen einzigen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-204">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4a9c6-205">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-205">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="4a9c6-206">Der app *secrets.json* Datei wurde geändert, um das Schlüssel-Wert-Paar zugeordnet Entfernen der `MoviesConnectionString` Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-206">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="4a9c6-207">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-207">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="4a9c6-208">Entfernen Sie aller geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="4a9c6-208">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="4a9c6-209">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-209">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="4a9c6-210">Alle Benutzer geheime Schlüssel für die app aus gelöscht wurden die *secrets.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-210">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="4a9c6-211">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="4a9c6-211">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="4a9c6-212">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="4a9c6-212">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>