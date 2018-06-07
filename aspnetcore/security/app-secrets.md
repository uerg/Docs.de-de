---
title: Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Speichern und Abrufen von vertraulichen Informationen als app geheime Schlüssel während der Entwicklung einer App ASP.NET Core.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: fd5cf5cdffd7281d7f4e0d96e8230b60be64a7c3
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/06/2018
ms.locfileid: "34819135"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="c1074-103">Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c1074-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="c1074-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="c1074-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="c1074-105">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c1074-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="c1074-106">Dieses Dokument erläutert Techniken zum Speichern und Abrufen von sensiblen Daten während der Entwicklung einer App ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1074-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="c1074-107">Sie sollten niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern, und Sie darf keine Produktion geheime Schlüssel in der Entwicklung oder Modus zu testen.</span><span class="sxs-lookup"><span data-stu-id="c1074-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="c1074-108">Sie können speichern und Schützen von Azure geheime Schlüssel Test- und produktionsumgebungen mit der [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="c1074-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="c1074-109">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="c1074-109">Environment variables</span></span>

<span data-ttu-id="c1074-110">Umgebungsvariablen werden zum Speichern von app-Kennwörter im Code oder in den lokalen Konfigurationsdateien zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="c1074-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="c1074-111">Umgebungsvariablen außer Kraft setzen von Konfigurationswerten für alle zuvor angegebenen Konfigurationsquellen aus.</span><span class="sxs-lookup"><span data-stu-id="c1074-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="c1074-112">Konfigurieren Sie das Lesen der Werte für Umgebungsvariablen, durch den Aufruf [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c1074-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="c1074-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="c1074-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="c1074-114">Betrachten Sie eine ASP.NET Core-Web-app in der **einzelne Benutzerkonten** Sicherheit aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="c1074-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="c1074-115">Eine Standard-Datenbank-Verbindungszeichenfolge wird in das Projekt aufgenommenes *appsettings.json* Datei mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="c1074-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="c1074-116">Die Standard-Verbindungszeichenfolge ist für LocalDB, im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="c1074-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="c1074-117">Während der Bereitstellung von Apps die `DefaultConnection` Schlüssel-Wert kann überschrieben werden, mit dem Wert einer Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="c1074-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="c1074-118">Die Umgebungsvariable kann die vollständige Verbindungszeichenfolge mit vertraulichen Anmeldeinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="c1074-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="c1074-119">Umgebungsvariablen werden im Allgemeinen in einfachen, unverschlüsselte Text gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1074-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="c1074-120">Wenn der Computer oder einen Prozess beschädigt ist, können der Umgebungsvariablen von nicht vertrauenswürdigen Parteien zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="c1074-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="c1074-121">Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c1074-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="c1074-122">Geheime Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="c1074-122">Secret Manager</span></span>

<span data-ttu-id="c1074-123">Das Schlüssel-Manager-Tool speichert vertrauliche Daten während der Entwicklung eines Projekts ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1074-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="c1074-124">In diesem Kontext ist ein Teil der sensiblen Daten ein app-Geheimnis.</span><span class="sxs-lookup"><span data-stu-id="c1074-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="c1074-125">App-Kennwörter werden in einem anderen Speicherort aus der Projektstruktur gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1074-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="c1074-126">Die app-Kennwörter werden mit einem bestimmten Projekt verknüpft ist oder mehreren Projekten gemeinsam.</span><span class="sxs-lookup"><span data-stu-id="c1074-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="c1074-127">Die app-Kennwörter werden nicht in das Quellsteuerelement eingecheckt.</span><span class="sxs-lookup"><span data-stu-id="c1074-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="c1074-128">Der geheime Schlüssel-Manager-Tool nicht die gespeicherten geheimen Schlüssel zum Verschlüsseln und sollte nicht als vertrauenswürdigen Speicher behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="c1074-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="c1074-129">Es ist nur für Entwicklungszwecke.</span><span class="sxs-lookup"><span data-stu-id="c1074-129">It's for development purposes only.</span></span> <span data-ttu-id="c1074-130">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1074-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="c1074-131">Wie funktioniert das Schlüssel-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="c1074-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="c1074-132">Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="c1074-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="c1074-133">Sie können das Tool verwenden, ohne diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="c1074-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="c1074-134">Die Werte werden in einer JSON-Konfigurationsdatei in einem System geschütztem Benutzerprofilordner auf dem lokalen Computer gespeichert:</span><span class="sxs-lookup"><span data-stu-id="c1074-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c1074-135">Windows</span><span class="sxs-lookup"><span data-stu-id="c1074-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="c1074-136">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="c1074-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="c1074-137">macOS</span><span class="sxs-lookup"><span data-stu-id="c1074-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="c1074-138">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="c1074-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="c1074-139">Linux</span><span class="sxs-lookup"><span data-stu-id="c1074-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="c1074-140">-Pfad:</span><span class="sxs-lookup"><span data-stu-id="c1074-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="c1074-141">Klicken Sie in den vorherigen Dateipfade, ersetzen Sie `<user_secrets_id>` mit der `UserSecretsId` Wert in der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c1074-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="c1074-142">Schreiben Sie Code, der abhängig von der Speicherort oder das Format der Daten, die mit dem geheimen Schlüssel-Manager-Tool gespeichert nicht.</span><span class="sxs-lookup"><span data-stu-id="c1074-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="c1074-143">Diese Implementierungsdetails können geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c1074-143">These implementation details may change.</span></span> <span data-ttu-id="c1074-144">Z. B. die Werte für den geheimen werden nicht verschlüsselt, aber in der Zukunft werden konnte.</span><span class="sxs-lookup"><span data-stu-id="c1074-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="c1074-145">Installieren Sie den geheimen Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="c1074-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="c1074-146">Das Schlüssel-Manager-Tool wird mit der .NET Core CLI ab .NET Core SDK 2.1.300 gebündelt.</span><span class="sxs-lookup"><span data-stu-id="c1074-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="c1074-147">Für .NET Core SDK-Versionen vor 2.1.300 ist ein Tool-Installationsordner erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c1074-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="c1074-148">Führen Sie `dotnet --version` aus eine Befehlsshell, die Anzahl der installierten .NET Core SDK-Version finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="c1074-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="c1074-149">Wenn die .NET Core SDK verwendet das Tool enthält, wird eine Warnung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c1074-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="c1074-150">Installieren der [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet-Paket im Projekt ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c1074-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="c1074-151">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c1074-151">For example:</span></span>

<span data-ttu-id="c1074-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="c1074-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="c1074-153">Führen Sie den folgenden Befehl in eine Befehlsshell beim Überprüfen der Installation der Tools:</span><span class="sxs-lookup"><span data-stu-id="c1074-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="c1074-154">Das Schlüssel-Manager-Tool zeigt die Verwendung des Beispiels, der Optionen und der Befehl Hilfe:</span><span class="sxs-lookup"><span data-stu-id="c1074-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="c1074-155">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Elemente.</span><span class="sxs-lookup"><span data-stu-id="c1074-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="c1074-156">Legen Sie einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-156">Set a secret</span></span>

<span data-ttu-id="c1074-157">Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="c1074-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="c1074-158">Definieren, um Benutzer geheime Schlüssel zu verwenden, eine `UserSecretsId` Element innerhalb einer `PropertyGroup` von der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c1074-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="c1074-159">Der Wert des `UserSecretsId` ist willkürlich, jedoch ist charakteristisch für das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c1074-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="c1074-160">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="c1074-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="c1074-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="c1074-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="c1074-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="c1074-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="c1074-163">Klicken Sie in Visual Studio mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="c1074-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="c1074-164">Fügt dieser Bewegung eine `UserSecretsId` -Element, mit einer GUID, die aufgefüllt sind, zu der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="c1074-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="c1074-165">Visual Studio öffnet eine *secrets.json* Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="c1074-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="c1074-166">Ersetzen Sie den Inhalt des *secrets.json* mit Schlüssel-Wert-Paare gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="c1074-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="c1074-167">Beispiel: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="c1074-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="c1074-168">Definieren Sie ein app-Geheimnis bestehend aus einem Schlüssel und seinen Wert an.</span><span class="sxs-lookup"><span data-stu-id="c1074-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="c1074-169">Der geheime Schlüssel ist mit des Projekts zugeordneten `UserSecretsId` Wert.</span><span class="sxs-lookup"><span data-stu-id="c1074-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="c1074-170">Führen Sie beispielsweise den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="c1074-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="c1074-171">Im vorherigen Beispiel der Doppelpunkt bedeutet, dass `Movies` ist ein Objekt mit Literalen eine `ServiceApiKey` Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="c1074-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="c1074-172">Das Schlüssel-Manager-Tool kann von anderen Verzeichnissen zu verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="c1074-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="c1074-173">Verwenden der `--project` Option aus, um den Dateisystempfad zu dem Angeben der *csproj* Datei vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="c1074-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="c1074-174">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c1074-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="c1074-175">Legen Sie mehrere geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-175">Set multiple secrets</span></span>

<span data-ttu-id="c1074-176">Ein Batch von geheimen Schlüsseln kann festgelegt werden, indem piping JSON in der `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="c1074-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="c1074-177">Im folgenden Beispiel die *input.json* Dateiinhalt sind über die Pipeline an das `set` Befehl.</span><span class="sxs-lookup"><span data-stu-id="c1074-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="c1074-178">Windows</span><span class="sxs-lookup"><span data-stu-id="c1074-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="c1074-179">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c1074-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="c1074-180">macOS</span><span class="sxs-lookup"><span data-stu-id="c1074-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="c1074-181">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c1074-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="c1074-182">Linux</span><span class="sxs-lookup"><span data-stu-id="c1074-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="c1074-183">Öffnen Sie eine Befehlsshell, und führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c1074-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="c1074-184">Zugriff auf einen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="c1074-185">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager.</span><span class="sxs-lookup"><span data-stu-id="c1074-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="c1074-186">Installieren der [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="c1074-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="c1074-187">Fügen Sie die Benutzer Konfigurationsquelle für geheime Schlüssel mit einem Aufruf von [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c1074-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="c1074-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="c1074-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="c1074-189">Die [ASP.NET Core-Konfigurations-API](xref:fundamentals/configuration/index) ermöglicht den Zugriff auf den geheimen Schlüssel-Manager.</span><span class="sxs-lookup"><span data-stu-id="c1074-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="c1074-190">Wenn Ihr Projekt auf .NET Framework abzielt, installieren Sie die [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="c1074-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="c1074-191">In ASP.NET Core 2.0 oder höher, die Benutzer-Konfigurationsquelle geheime Schlüssel automatisch für den Entwicklungsmodus hinzugefügt, wenn das Projekt aufgerufen [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zum Initialisieren einer neuen Instanz des Hosts mit Standardwerten vorkonfiguriert.</span><span class="sxs-lookup"><span data-stu-id="c1074-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="c1074-192">`CreateDefaultBuilder` Aufrufe [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) bei der [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) ist [Entwicklung](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="c1074-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="c1074-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="c1074-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="c1074-194">Wenn `CreateDefaultBuilder` wird nicht aufgerufen wird, während der Erstellung des Hosts, die Benutzer Konfigurationsquelle für geheime Schlüssel mit einem Aufruf von hinzufügen [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in die `Startup` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="c1074-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="c1074-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="c1074-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="c1074-196">Vertrauliche Benutzerdaten abgerufen werden können, über die `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="c1074-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="c1074-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="c1074-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="c1074-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="c1074-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="c1074-199">Zeichenfolgenersetzungen mit geheimen Schlüsseln</span><span class="sxs-lookup"><span data-stu-id="c1074-199">String replacement with secrets</span></span>

<span data-ttu-id="c1074-200">Speichern von Kennwörtern als nur-Text ist unsicher.</span><span class="sxs-lookup"><span data-stu-id="c1074-200">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="c1074-201">Z. B. eine Datenbankverbindungszeichenfolge, die in gespeicherten *appsettings.json* eventuell ein Kennwort für den angegebenen Benutzer:</span><span class="sxs-lookup"><span data-stu-id="c1074-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="c1074-202">Ein sicherer Ansatz besteht darin, das Kennwort als ein geheimer Schlüssel zu speichern.</span><span class="sxs-lookup"><span data-stu-id="c1074-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="c1074-203">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c1074-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="c1074-204">Entfernen Sie die `Password` Schlüssel-Wert-Paar aus der Verbindungszeichenfolge in *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="c1074-204">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="c1074-205">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c1074-205">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="c1074-206">Der Schlüssel-Wert kann festgelegt werden, auf eine ["SqlConnectionStringBuilder"](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) des Objekts [Kennwort](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) Eigenschaft die Verbindungszeichenfolge abgeschlossen:</span><span class="sxs-lookup"><span data-stu-id="c1074-206">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="c1074-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="c1074-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="c1074-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span><span class="sxs-lookup"><span data-stu-id="c1074-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="c1074-209">Liste der geheime Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="c1074-210">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="c1074-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="c1074-211">Die folgende Ausgabe wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c1074-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="c1074-212">Im vorherigen Beispiel kennzeichnet ein Doppelpunkt in den Schlüsselnamen in die Objekthierarchie *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="c1074-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="c1074-213">Entfernen Sie einen einzigen geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="c1074-214">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="c1074-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="c1074-215">Der app *secrets.json* Datei wurde geändert, um das Schlüssel-Wert-Paar zugeordnet Entfernen der `MoviesConnectionString` Schlüssel:</span><span class="sxs-lookup"><span data-stu-id="c1074-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="c1074-216">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c1074-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="c1074-217">Entfernen Sie aller geheimen Schlüssel</span><span class="sxs-lookup"><span data-stu-id="c1074-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="c1074-218">Führen den folgenden Befehl aus dem Verzeichnis, in dem die *csproj* Datei vorhanden ist:</span><span class="sxs-lookup"><span data-stu-id="c1074-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="c1074-219">Alle Benutzer geheime Schlüssel für die app aus gelöscht wurden die *secrets.json* Datei:</span><span class="sxs-lookup"><span data-stu-id="c1074-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="c1074-220">Ausführen `dotnet user-secrets list` wird die folgende Meldung angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c1074-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="c1074-221">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="c1074-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
