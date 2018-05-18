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
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="3b3fa-103">Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3b3fa-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="3b3fa-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="3b3fa-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b3fa-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="3b3fa-106">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b3fa-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="3b3fa-107">Dieses Dokument erläutert Techniken zum Speichern und Abrufen von sensiblen Daten während der Entwicklung einer App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="3b3fa-108">Sie sollten niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern, und Sie darf keine Produktion geheime Schlüssel in der Entwicklung oder Modus zu testen.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="3b3fa-109">Sie können speichern und Schützen von Azure geheime Schlüssel Test- und produktionsumgebungen mit der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="3b3fa-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="3b3fa-110">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="3b3fa-110">Environment variables</span></span>

<span data-ttu-id="3b3fa-111">Umgebungsvariablen werden zum Speichern von app-Kennwörter im Code oder in den lokalen Konfigurationsdateien zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="3b3fa-112">Umgebungsvariablen außer Kraft setzen von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen aus.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-113">Konfigurieren Sie das Lesen der Werte für Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="3b3fa-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="3b3fa-115">Betrachten Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="3b3fa-116">Eine Standard-Datenbank-Verbindungszeichenfolge wird in das Projekt aufgenommenes *appsettings.json* Datei mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="3b3fa-117">Die Standard-Verbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="3b3fa-118">Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="3b3fa-119">Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="3b3fa-120">Umgebungsvariablen werden im Allgemeinen in einfachen, unverschlüsselte Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="3b3fa-121">Wenn der Computer oder einen Prozess beschädigt ist, können der Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="3b3fa-122">Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="3b3fa-123">Geheime Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="3b3fa-123">Secret Manager</span></span>

<span data-ttu-id="3b3fa-124">Das Schlüssel-Manager-Tool speichert vertrauliche Daten während der Entwicklung eines Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="3b3fa-125">In diesem Kontext ist ein Teil der sensiblen Daten ein app-Geheimnis.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="3b3fa-126">App-Kennwörter werden in einem anderen Speicherort aus der Projektstruktur gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="3b3fa-127">Die app-Kennwörter werden mit einem bestimmten Projekt verknüpft ist oder mehreren Projekten gemeinsam.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="3b3fa-128">Die app-Kennwörter werden nicht in das Quellsteuerelement eingecheckt.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="3b3fa-129">Der geheime Schlüssel-Manager-Tool nicht die gespeicherten geheimen Schlüssel zum Verschlüsseln und sollte nicht als vertrauenswürdigen Speicher behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="3b3fa-130">Es ist nur für Entwicklungszwecke.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-130">It's for development purposes only.</span></span> <span data-ttu-id="3b3fa-131">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="3b3fa-132">Wie funktioniert das Schlüssel-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="3b3fa-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="3b3fa-133">Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="3b3fa-134">Sie können das Tool verwenden, ohne diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="3b3fa-135">Die Werte werden gespeichert, einem [JSON](https://json.org/) Konfigurationsdatei in einem System geschütztem Benutzerprofilordner auf dem lokalen Computer:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="3b3fa-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="3b3fa-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="3b3fa-137">Linux & MacOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="3b3fa-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="3b3fa-138">Klicken Sie in den vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="3b3fa-139">Schreiben Sie Code, der abhängig von der Speicherort oder das Format der Daten, die mit dem geheimen Schlüssel-Manager-Tool gespeichert nicht.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="3b3fa-140">Diese Implementierungsdetails können geändert werden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-140">These implementation details may change.</span></span> <span data-ttu-id="3b3fa-141">Z. B. die Werte für den geheimen werden nicht verschlüsselt, aber in der Zukunft werden konnte.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="3b3fa-142">Installieren Sie den geheimen Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="3b3fa-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="3b3fa-143">Das Schlüssel-Manager-Tool wird mit der .NET Core-CLI in .NET Core SDK 2.1 gebündelt.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="3b3fa-144">Für .NET Core SDK 2.0 und früheren Versionen ist das Tool-Installationsordner erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="3b3fa-145">Installieren der [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket im Projekt ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="3b3fa-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="3b3fa-147">Führen Sie den folgenden Befehl in eine Befehlsshell beim Überprüfen der Installation der Tools:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="3b3fa-148">Das Schlüssel-Manager-Tool zeigt die Verwendung des Beispiels, der Optionen und der Befehl Hilfe:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="3b3fa-149">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Elemente.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="3b3fa-150">Legen Sie einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-150">Set a secret</span></span>

<span data-ttu-id="3b3fa-151">Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="3b3fa-152">Definieren, um Benutzer geheime Schlüssel zu verwenden, eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="3b3fa-153">Der Wert des `UserSecretsId` ist willkürlich, jedoch ist charakteristisch für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="3b3fa-154">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="3b3fa-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="3b3fa-157">Klicken Sie in Visual Studio mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="3b3fa-158">Fügt dieser Bewegung eine `UserSecretsId` -Element, mit einer GUID, die aufgefüllt sind, zu der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="3b3fa-159">Visual Studio öffnet eine *secrets.json* Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="3b3fa-160">Ersetzen Sie den Inhalt des *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="3b3fa-161">Beispiel: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="3b3fa-162">Definieren Sie ein app-Geheimnis bestehend aus einem Schlüssel und seinen Wert an.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="3b3fa-163">Der geheime Schlüssel ist mit des Projekts zugeordneten `UserSecretsId` Wert.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="3b3fa-164">Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="3b3fa-165">Im vorherigen Beispiel der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt mit Literalen eine `ServiceApiKey` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="3b3fa-166">Das Schlüssel-Manager-Tool kann von anderen Verzeichnissen zu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="3b3fa-167">Verwenden der `--project` Option aus, um den Dateisystempfad zu dem Angeben der *csproj* Datei vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="3b3fa-168">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="3b3fa-169">Legen Sie mehrere geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-169">Set multiple secrets</span></span>

<span data-ttu-id="3b3fa-170">Ein Batch von geheimen Schlüsseln kann festgelegt werden, indem piping JSON in der `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="3b3fa-171">Im folgenden Beispiel die *input.json* Dateiinhalt sind über die Pipeline an das `set` Befehl unter Windows:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="3b3fa-172">Verwenden Sie den folgenden Befehl auf MacOS und Linux:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="3b3fa-173">Zugriff auf einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-173">Access a secret</span></span>

<span data-ttu-id="3b3fa-174">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="3b3fa-175">Wenn .NET Core als Ziel dient 1.x oder .NET Framework, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-176">Hinzufügen der Benutzer geheime Schlüssel Konfigurationsquelle angibt, die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="3b3fa-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="3b3fa-178">Vertrauliche Benutzerdaten abgerufen werden können, über die `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="3b3fa-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="3b3fa-181">Zeichenfolgenersetzungen mit geheimen Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="3b3fa-181">String replacement with secrets</span></span>

<span data-ttu-id="3b3fa-182">Es ist riskant, Speichern von Kennwörtern als nur-Text.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="3b3fa-183">Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *appsettings.json* eventuell ein Kennwort für den angegebenen Benutzer:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="3b3fa-184">Ein sicherer Ansatz besteht darin, das Kennwort als ein geheimer Schlüssel zu speichern.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="3b3fa-185">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="3b3fa-186">Ersetzen Sie das Kennwort in *appsettings.json* durch einen Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="3b3fa-187">Im folgenden Beispiel `{0}` dient als Platzhalter in Form einer [zusammengesetzte Formatzeichenfolge](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="3b3fa-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="3b3fa-188">Der Schlüssel-Wert kann in den Platzhalter zum Abschließen der Verbindungszeichenfolge eingegeben werden:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="3b3fa-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="3b3fa-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="3b3fa-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="3b3fa-191">Liste der geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="3b3fa-192">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="3b3fa-193">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="3b3fa-194">Im vorherigen Beispiel kennzeichnet ein Doppelpunkt in den Schlüsselnamen in die Objekthierarchie *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="3b3fa-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="3b3fa-195">Entfernen Sie einen einzigen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="3b3fa-196">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="3b3fa-197">Der app *secrets.json* Datei wurde geändert, um das Schlüssel-Wert-Paar zugeordnet Entfernen der `MoviesConnectionString` Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="3b3fa-198">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="3b3fa-199">Entfernen Sie aller geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="3b3fa-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="3b3fa-200">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="3b3fa-201">Alle Benutzer geheime Schlüssel für die app aus gelöscht wurden die *secrets.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="3b3fa-202">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="3b3fa-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="3b3fa-203">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3b3fa-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>