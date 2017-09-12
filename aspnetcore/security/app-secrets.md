---
title: "Sichere Speicherung von app-Kennwörter während der Entwicklung in ASP.NET Core"
author: rick-anderson
description: "Zeigt, wie sichere Speichern von geheimen Schlüsseln während der Entwicklung"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/app-secrets
ms.openlocfilehash: 56214c2fbdca84591c5c1a6b7f2451f33ee64ef0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a><span data-ttu-id="ff784-104">Sichere Speicherung von app-Kennwörter während der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ff784-104">Safe storage of app secrets during development in ASP.NET Core</span></span>

<a name=security-app-secrets></a>

<span data-ttu-id="ff784-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="ff784-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="ff784-106">Dieses Dokument wird erläutert, wie das Schlüssel-Manager-Tool in der Entwicklung können aus dem Code geheime Schlüssel beibehalten.</span><span class="sxs-lookup"><span data-stu-id="ff784-106">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="ff784-107">Der wichtigste Punkt ist, sollten Sie niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern und Produktion geheime Schlüssel verwenden, im Modus für Entwicklung und Tests keine.</span><span class="sxs-lookup"><span data-stu-id="ff784-107">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="ff784-108">Stattdessen können Sie die [Konfiguration](../fundamentals/configuration.md) System lesen Sie diese Werte von Umgebungsvariablen oder tool aus Werten, die mit dem geheimen Schlüssel-Manager gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ff784-108">You can instead use the [configuration](../fundamentals/configuration.md) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="ff784-109">Das Schlüssel-Manager-Tool verhindert, dass sensible Daten in die quellcodeverwaltung überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="ff784-109">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="ff784-110">Die [Konfiguration](../fundamentals/configuration.md) System kann mit dem geheimen Schlüssel-Manager-Tool, das in diesem Artikel beschriebenen gespeicherten geheimen Schlüssel gelesen.</span><span class="sxs-lookup"><span data-stu-id="ff784-110">The [configuration](../fundamentals/configuration.md) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="ff784-111">Der geheime Schlüssel-Manager-Tool wird nur in der Entwicklung verwendet.</span><span class="sxs-lookup"><span data-stu-id="ff784-111">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="ff784-112">Sie können Azure geheime Schlüssel Test- und produktionsumgebungen mit Schützen der [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Konfigurationsanbieter.</span><span class="sxs-lookup"><span data-stu-id="ff784-112">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="ff784-113">Finden Sie unter [Azure Key Vault-Konfigurationsanbieter](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="ff784-113">See [Azure Key Vault configuration provider](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="ff784-114">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="ff784-114">Environment variables</span></span>

<span data-ttu-id="ff784-115">Um zu vermeiden, geheime app-Schlüssel im Code oder in lokalen Konfigurationsdateien gespeichert, geheime Schlüssel in Umgebungsvariablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ff784-115">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="ff784-116">Sie können das setup die [Konfiguration](../fundamentals/configuration.md) Framework Lesen von Werten durch Aufrufen von Umgebungsvariablen `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="ff784-116">You can setup the [configuration](../fundamentals/configuration.md) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="ff784-117">Umgebungsvariablen können dann die Konfigurationswerte für alle zuvor angegebenen Konfigurationsquellen außer Kraft setzen.</span><span class="sxs-lookup"><span data-stu-id="ff784-117">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="ff784-118">Z. B. Wenn Sie eine neue ASP.NET Core-Web-app mit einzelner Benutzerkonten erstellen, dabei werden hinzugefügt. eine Standard-Verbindungszeichenfolge für die *appsettings.json* Datei im Projekt mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="ff784-118">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="ff784-119">Die Standardverbindungszeichenfolge bezieht Setup LocalDB, verwendet wird, die im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="ff784-119">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="ff784-120">Wenn Sie Ihre Anwendung auf einem Test- oder Produktionscomputer Server bereitstellen, können Sie Sie überschreiben die `DefaultConnection` -Schlüsselwert mit einer Einstellung der Umgebungsvariablen, die die Verbindungszeichenfolge (u. u. mit sensible Anmeldeinformationen) für eine Test- oder Produktionscomputer Datenbank enthält Server.</span><span class="sxs-lookup"><span data-stu-id="ff784-120">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="ff784-121">Umgebungsvariablen werden im Allgemeinen in Klartext gespeichert und werden nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="ff784-121">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="ff784-122">Wenn der Computer oder einen Prozess beschädigt ist, können von nicht vertrauenswürdigen Parteien Umgebungsvariablen zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="ff784-122">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="ff784-123">Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise weiterhin erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ff784-123">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="ff784-124">Geheime Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="ff784-124">Secret Manager</span></span>

<span data-ttu-id="ff784-125">Das Schlüssel-Manager-Tool speichert die sensible Daten für andere Entwicklungen außerhalb der Projektstruktur.</span><span class="sxs-lookup"><span data-stu-id="ff784-125">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="ff784-126">Das Schlüssel-Manager-Tool ist ein Verwaltungstool, das verwendet werden kann, Speichern von geheimen Schlüsseln für eine [.NET Core](https://www.microsoft.com/net/core) Projekt während der Entwicklung.</span><span class="sxs-lookup"><span data-stu-id="ff784-126">The Secret Manager tool is a project tool that can be used to store secrets for a [.NET Core](https://www.microsoft.com/net/core) project during development.</span></span> <span data-ttu-id="ff784-127">Mit dem geheimen Schlüssel-Manager-Tool können Sie ein bestimmtes Projekt geheime app-Schlüssel zuordnen und diese projektübergreifend freigeben.</span><span class="sxs-lookup"><span data-stu-id="ff784-127">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="ff784-128">Der geheime Schlüssel-Manager-Tool die gespeicherten geheimen Schlüssel nicht verschlüsselt und nicht als vertrauenswürdigen Speicher behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="ff784-128">The Secret Manager tool does not encrypt the stored secrets and should not be treated as a trusted store.</span></span> <span data-ttu-id="ff784-129">Es ist nur für Entwicklungszwecke.</span><span class="sxs-lookup"><span data-stu-id="ff784-129">It is for development purposes only.</span></span> <span data-ttu-id="ff784-130">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ff784-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

### <a name="visual-studio-2017-installing-the-secret-manager-tool"></a><span data-ttu-id="ff784-131">Visual Studio-2017: Installieren des Schlüssel-Manager-Tools</span><span class="sxs-lookup"><span data-stu-id="ff784-131">Visual Studio 2017: Installing the Secret Manager tool</span></span>

<span data-ttu-id="ff784-132">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **bearbeiten \<Project_name\>csproj** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="ff784-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="ff784-133">Fügen Sie die hervorgehobene Zeile auf die *csproj* Datei, und speichern, um das zugehörige NuGet-Paket wiederherstellen:</span><span class="sxs-lookup"><span data-stu-id="ff784-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

<span data-ttu-id="ff784-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="ff784-134">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="ff784-135">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="ff784-135">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="ff784-136">Diese Bewegung wird ein neues `UserSecretsId` Knoten innerhalb einer `PropertyGroup` von der *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="ff784-136">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="ff784-137">Öffnen Sie es auch eine `secrets.json` -Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="ff784-137">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="ff784-138">Fügen Sie `secrets.json` Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="ff784-138">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-2015-installing-the-secret-manager-tool"></a><span data-ttu-id="ff784-139">Visual Studio 2015: Installieren des Schlüssel-Manager-Tools</span><span class="sxs-lookup"><span data-stu-id="ff784-139">Visual Studio 2015: Installing the Secret Manager tool</span></span>

<span data-ttu-id="ff784-140">Öffnen Sie das Projekt `project.json` Datei.</span><span class="sxs-lookup"><span data-stu-id="ff784-140">Open the project's `project.json` file.</span></span> <span data-ttu-id="ff784-141">Hinzufügen eines Verweises auf `Microsoft.Extensions.SecretManager.Tools` innerhalb der `tools` -Eigenschaft, und speichern, um das zugehörige NuGet-Paket wiederherstellen:</span><span class="sxs-lookup"><span data-stu-id="ff784-141">Add a reference to `Microsoft.Extensions.SecretManager.Tools` within the `tools` property, and save to restore the associated NuGet package:</span></span>

```json
"tools": {
    "Microsoft.Extensions.SecretManager.Tools": "1.0.0-preview2-final",
    "Microsoft.AspNetCore.Server.IISIntegration.Tools": "1.0.0-preview2-final"
},
```

<span data-ttu-id="ff784-142">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="ff784-142">Right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="ff784-143">Diese Bewegung wird ein neues `userSecretsId` Eigenschaft `project.json`.</span><span class="sxs-lookup"><span data-stu-id="ff784-143">This gesture adds a new `userSecretsId` property to `project.json`.</span></span> <span data-ttu-id="ff784-144">Öffnen Sie es auch eine `secrets.json` -Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="ff784-144">It also opens a `secrets.json` file in the text editor.</span></span>

<span data-ttu-id="ff784-145">Fügen Sie `secrets.json` Folgendes hinzu:</span><span class="sxs-lookup"><span data-stu-id="ff784-145">Add the following to `secrets.json`:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

### <a name="visual-studio-code-or-command-line-installing-the-secret-manager-tool"></a><span data-ttu-id="ff784-146">Visual Studio-Code oder über die Befehlszeile: Installieren den geheimen Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="ff784-146">Visual Studio Code or Command Line: Installing the Secret Manager tool</span></span>

<span data-ttu-id="ff784-147">Hinzufügen `Microsoft.Extensions.SecretManager.Tools` auf die *csproj* , und führen Sie `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="ff784-147">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run `dotnet restore`.</span></span>

<span data-ttu-id="ff784-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span><span class="sxs-lookup"><span data-stu-id="ff784-148">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?highlight=21)]</span></span>

<span data-ttu-id="ff784-149">Testen Sie das Schlüssel-Manager-Tool, indem Sie den folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="ff784-149">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="ff784-150">Das Schlüssel-Manager-Tool werden Verwendung, Optionen und Befehl Hilfe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ff784-150">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="ff784-151">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Knoten.</span><span class="sxs-lookup"><span data-stu-id="ff784-151">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="ff784-152">Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="ff784-152">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="ff784-153">Um Benutzer geheime Schlüssel zu verwenden, das Projekt angegeben, muss ein `UserSecretsId` Wert in seine *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="ff784-153">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="ff784-154">Der Wert des `UserSecretsId` ist willkürlich, jedoch ist für das Projekt in der Regel eindeutig.</span><span class="sxs-lookup"><span data-stu-id="ff784-154">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="ff784-155">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="ff784-155">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="ff784-156">Hinzufügen einer `UserSecretsId` für das Projekt in der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="ff784-156">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

<span data-ttu-id="ff784-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="ff784-157">[!code-xml[Main](app-secrets/sample/UserSecrets/UserSecrets.csproj?range=7-9&highlight=2)]</span></span>

<span data-ttu-id="ff784-158">Verwenden Sie den geheimen Schlüssel-Manager, um einen geheimen Schlüssel festzulegen.</span><span class="sxs-lookup"><span data-stu-id="ff784-158">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="ff784-159">Geben Sie in einem Befehlsfenster aus dem Projektverzeichnis beispielsweise Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="ff784-159">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="ff784-160">Sie können das Schlüssel-Manager-Tool ausführen, aus anderen Verzeichnissen, aber Sie müssen die `--project` Option aus, um den Pfad zum Übergeben der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="ff784-160">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="ff784-161">Sie können auch das Tool Secret-Manager angezeigt wird, entfernen und app-Kennwörter zu löschen.</span><span class="sxs-lookup"><span data-stu-id="ff784-161">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="ff784-162">Zugreifen auf vertrauliche Benutzerdaten über die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ff784-162">Accessing user secrets via configuration</span></span>

<span data-ttu-id="ff784-163">Sie können die geheimen Schlüssel-Manager über das Konfigurationssystem zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ff784-163">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="ff784-164">Hinzufügen der `Microsoft.Extensions.Configuration.UserSecrets` Verpacken und ausführen `dotnet restore`.</span><span class="sxs-lookup"><span data-stu-id="ff784-164">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run `dotnet restore`.</span></span>

<span data-ttu-id="ff784-165">Hinzufügen der Benutzer geheime Schlüssel Konfigurationsquelle angibt, die `Startup` Methode:</span><span class="sxs-lookup"><span data-stu-id="ff784-165">Add the user secrets configuration source to the `Startup` method:</span></span>

<span data-ttu-id="ff784-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span><span class="sxs-lookup"><span data-stu-id="ff784-166">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]</span></span>

<span data-ttu-id="ff784-167">Sie können Benutzer geheime Schlüssel über den Konfigurations-API zugreifen:</span><span class="sxs-lookup"><span data-stu-id="ff784-167">You can access user secrets via the configuration API:</span></span>

<span data-ttu-id="ff784-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span><span class="sxs-lookup"><span data-stu-id="ff784-168">[!code-csharp[Main](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="ff784-169">Wie funktioniert das Schlüssel-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="ff784-169">How the Secret Manager tool works</span></span>

<span data-ttu-id="ff784-170">Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="ff784-170">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="ff784-171">Sie können das Tool verwenden, ohne diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="ff784-171">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="ff784-172">In der aktuellen Version werden die Werte gespeichert, einem [JSON](http://json.org/) Konfigurationsdatei im Verzeichnis Benutzers-Profil:</span><span class="sxs-lookup"><span data-stu-id="ff784-172">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="ff784-173">Windows:`%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="ff784-173">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="ff784-174">Linux:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="ff784-174">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="ff784-175">Mac:`~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="ff784-175">Mac: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="ff784-176">Der Wert der `userSecretsId` stammt aus dem Wert im angegebenen *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="ff784-176">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="ff784-177">Sie sollten nicht Code schreiben, der auf dem Speicherort oder das Format der Daten gespeichert werden, mit dem geheimen Schlüssel-Manager-Tool abhängig ist, da diese Implementierungsdetails ändern können.</span><span class="sxs-lookup"><span data-stu-id="ff784-177">You should not write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="ff784-178">Beispielsweise sind die Werte für den geheimen derzeit *nicht* heute verschlüsselt, aber irgendwann werden konnte.</span><span class="sxs-lookup"><span data-stu-id="ff784-178">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ff784-179">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ff784-179">Additional Resources</span></span>

* [<span data-ttu-id="ff784-180">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="ff784-180">Configuration</span></span>](../fundamentals/configuration.md)
