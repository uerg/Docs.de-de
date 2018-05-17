---
title: Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core
author: rick-anderson
description: Zeigt, wie sichere Speichern von geheimen Schlüsseln während der Entwicklung
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: a268fd76a303dc1185b451e4f678fc2fe761e80a
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/12/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="35c2b-103">Sichere Speicherung von geheime app-Schlüssel in der Entwicklung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="35c2b-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="35c2b-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), und [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="35c2b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://scottaddie.com)</span></span> 

<span data-ttu-id="35c2b-105">Dieses Dokument wird erläutert, wie das Schlüssel-Manager-Tool in der Entwicklung können aus dem Code geheime Schlüssel beibehalten.</span><span class="sxs-lookup"><span data-stu-id="35c2b-105">This document shows how you can use the Secret Manager tool in development to keep secrets out of your code.</span></span> <span data-ttu-id="35c2b-106">Der wichtigste Punkt ist, sollten Sie niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern und Produktion geheime Schlüssel verwenden, im Modus für Entwicklung und Tests keine.</span><span class="sxs-lookup"><span data-stu-id="35c2b-106">The most important point is you should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development and test mode.</span></span> <span data-ttu-id="35c2b-107">Stattdessen können Sie die [Konfiguration](xref:fundamentals/configuration/index) System lesen Sie diese Werte von Umgebungsvariablen oder tool aus Werten, die mit dem geheimen Schlüssel-Manager gespeichert.</span><span class="sxs-lookup"><span data-stu-id="35c2b-107">You can instead use the [configuration](xref:fundamentals/configuration/index) system to read these values from environment variables or from values stored using the Secret Manager tool.</span></span> <span data-ttu-id="35c2b-108">Das Schlüssel-Manager-Tool verhindert, dass sensible Daten in die quellcodeverwaltung überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="35c2b-108">The Secret Manager tool helps prevent sensitive data from being checked into source control.</span></span> <span data-ttu-id="35c2b-109">Die [Konfiguration](xref:fundamentals/configuration/index) System kann mit dem geheimen Schlüssel-Manager-Tool, das in diesem Artikel beschriebenen gespeicherten geheimen Schlüssel gelesen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-109">The [configuration](xref:fundamentals/configuration/index) system can read secrets stored with the Secret Manager tool described in this article.</span></span>

<span data-ttu-id="35c2b-110">Der geheime Schlüssel-Manager-Tool wird nur in der Entwicklung verwendet.</span><span class="sxs-lookup"><span data-stu-id="35c2b-110">The Secret Manager tool is used only in development.</span></span> <span data-ttu-id="35c2b-111">Sie können Azure geheime Schlüssel Test- und produktionsumgebungen mit Schützen der [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Konfigurationsanbieter.</span><span class="sxs-lookup"><span data-stu-id="35c2b-111">You can safeguard Azure test and production secrets with the [Microsoft Azure Key Vault](https://azure.microsoft.com/services/key-vault/) configuration provider.</span></span> <span data-ttu-id="35c2b-112">Finden Sie unter [Azure Key Vault-Konfigurationsanbieter](xref:security/key-vault-configuration) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-112">See [Azure Key Vault configuration provider](xref:security/key-vault-configuration) for more information.</span></span>

## <a name="environment-variables"></a><span data-ttu-id="35c2b-113">Umgebungsvariablen</span><span class="sxs-lookup"><span data-stu-id="35c2b-113">Environment variables</span></span>

<span data-ttu-id="35c2b-114">Um zu vermeiden, geheime app-Schlüssel im Code oder in lokalen Konfigurationsdateien gespeichert, geheime Schlüssel in Umgebungsvariablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="35c2b-114">To avoid storing app secrets in code or in local configuration files, you store secrets in environment variables.</span></span> <span data-ttu-id="35c2b-115">Sie können das setup die [Konfiguration](xref:fundamentals/configuration/index) Framework Lesen von Werten durch Aufrufen von Umgebungsvariablen `AddEnvironmentVariables`.</span><span class="sxs-lookup"><span data-stu-id="35c2b-115">You can setup the [configuration](xref:fundamentals/configuration/index) framework to read values from environment variables by calling `AddEnvironmentVariables`.</span></span> <span data-ttu-id="35c2b-116">Umgebungsvariablen können dann die Konfigurationswerte für alle zuvor angegebenen Konfigurationsquellen außer Kraft setzen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-116">You can then use environment variables to override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="35c2b-117">Z. B. Wenn Sie eine neue ASP.NET Core-Web-app mit einzelner Benutzerkonten erstellen, dabei werden hinzugefügt. eine Standard-Verbindungszeichenfolge für die *appsettings.json* Datei im Projekt mit dem Schlüssel `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="35c2b-117">For example, if you create a new ASP.NET Core web app with individual user accounts, it will add a default connection string to the *appsettings.json* file in the project with the key `DefaultConnection`.</span></span> <span data-ttu-id="35c2b-118">Die Standardverbindungszeichenfolge bezieht Setup LocalDB, verwendet wird, die im Benutzermodus ausgeführt wird und ein Kennwort erfordert.</span><span class="sxs-lookup"><span data-stu-id="35c2b-118">The default connection string is setup to use LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="35c2b-119">Wenn Sie Ihre Anwendung auf einem Test- oder Produktionscomputer Server bereitstellen, können Sie Sie überschreiben die `DefaultConnection` -Schlüsselwert mit einer Einstellung der Umgebungsvariablen, die die Verbindungszeichenfolge (u. u. mit sensible Anmeldeinformationen) für eine Test- oder Produktionscomputer Datenbank enthält Server.</span><span class="sxs-lookup"><span data-stu-id="35c2b-119">When you deploy your application to a test or production server, you can override the `DefaultConnection` key value with an environment variable setting that contains the connection string (potentially with sensitive credentials) for a test or production database server.</span></span>

>[!WARNING]
> <span data-ttu-id="35c2b-120">Umgebungsvariablen werden im Allgemeinen in Klartext gespeichert und werden nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="35c2b-120">Environment variables are generally stored in plain text and are not encrypted.</span></span> <span data-ttu-id="35c2b-121">Wenn der Computer oder einen Prozess beschädigt ist, können von nicht vertrauenswürdigen Parteien Umgebungsvariablen zugegriffen werden.</span><span class="sxs-lookup"><span data-stu-id="35c2b-121">If the machine or process is compromised, then environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="35c2b-122">Zusätzliche Maßnahmen, um die Offenlegung von vertrauliche Benutzerdaten zu verhindern, dass möglicherweise weiterhin erforderlich.</span><span class="sxs-lookup"><span data-stu-id="35c2b-122">Additional measures to prevent disclosure of user secrets may still be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="35c2b-123">Geheime Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="35c2b-123">Secret Manager</span></span>

<span data-ttu-id="35c2b-124">Das Schlüssel-Manager-Tool speichert die sensible Daten für andere Entwicklungen außerhalb der Projektstruktur.</span><span class="sxs-lookup"><span data-stu-id="35c2b-124">The Secret Manager tool stores sensitive data for development work outside of your project tree.</span></span> <span data-ttu-id="35c2b-125">Das Schlüssel-Manager-Tool ist ein Verwaltungstool, das zum Speichern von geheimen Schlüssel für ein Projekt .NET Core während der Entwicklung verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="35c2b-125">The Secret Manager tool is a project tool that can be used to store secrets for a .NET Core project during development.</span></span> <span data-ttu-id="35c2b-126">Mit dem geheimen Schlüssel-Manager-Tool können Sie ein bestimmtes Projekt geheime app-Schlüssel zuordnen und diese projektübergreifend freigeben.</span><span class="sxs-lookup"><span data-stu-id="35c2b-126">With the Secret Manager tool, you can associate app secrets with a specific project and share them across multiple projects.</span></span>

>[!WARNING]
> <span data-ttu-id="35c2b-127">Der geheime Schlüssel-Manager-Tool nicht die gespeicherten geheimen Schlüssel zum Verschlüsseln und sollte nicht als vertrauenswürdigen Speicher behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="35c2b-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="35c2b-128">Es ist nur für Entwicklungszwecke.</span><span class="sxs-lookup"><span data-stu-id="35c2b-128">It's for development purposes only.</span></span> <span data-ttu-id="35c2b-129">Die Schlüssel und Werte werden in einer JSON-Konfigurationsdatei im Verzeichnis Benutzers-Profil gespeichert.</span><span class="sxs-lookup"><span data-stu-id="35c2b-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="installing-the-secret-manager-tool"></a><span data-ttu-id="35c2b-130">Installieren den geheimen Schlüssel-Manager</span><span class="sxs-lookup"><span data-stu-id="35c2b-130">Installing the Secret Manager tool</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="35c2b-131">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="35c2b-131">Visual Studio</span></span>](#tab/visual-studio/)

<span data-ttu-id="35c2b-132">Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **bearbeiten \<Project_name\>csproj** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="35c2b-132">Right-click the project in Solution Explorer, and select **Edit \<project_name\>.csproj** from the context menu.</span></span> <span data-ttu-id="35c2b-133">Fügen Sie die hervorgehobene Zeile auf die *csproj* Datei, und speichern, um das zugehörige NuGet-Paket wiederherstellen:</span><span class="sxs-lookup"><span data-stu-id="35c2b-133">Add the highlighted line to the *.csproj* file, and save to restore the associated NuGet package:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="35c2b-134">Mit der rechten Maustaste erneut auf des Projekts im Projektmappen-Explorer, und wählen Sie **verwalten Benutzer geheime Schlüssel** aus dem Kontextmenü.</span><span class="sxs-lookup"><span data-stu-id="35c2b-134">Right-click the project in Solution Explorer again, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="35c2b-135">Diese Bewegung wird ein neues `UserSecretsId` Knoten innerhalb einer `PropertyGroup` von der *csproj* Datei, wie im folgenden Beispiel verdeutlicht:</span><span class="sxs-lookup"><span data-stu-id="35c2b-135">This gesture adds a new `UserSecretsId` node within a `PropertyGroup` of the *.csproj* file, as highlighted in the following sample:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="35c2b-136">Speichern der geänderten *csproj* Datei auch öffnet eine `secrets.json` Datei im Text-Editor.</span><span class="sxs-lookup"><span data-stu-id="35c2b-136">Saving the modified *.csproj* file also opens a `secrets.json` file in the text editor.</span></span> <span data-ttu-id="35c2b-137">Ersetzen Sie den Inhalt von der `secrets.json` Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="35c2b-137">Replace the contents of the `secrets.json` file with the following code:</span></span>

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="35c2b-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="35c2b-138">Visual Studio Code</span></span>](#tab/visual-studio-code/)

<span data-ttu-id="35c2b-139">Hinzufügen `Microsoft.Extensions.SecretManager.Tools` auf die *csproj* , und führen Sie [Dotnet Wiederherstellung](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="35c2b-139">Add `Microsoft.Extensions.SecretManager.Tools` to the *.csproj* file and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span> <span data-ttu-id="35c2b-140">Die gleichen Schritte können Sie um das für die Befehlszeile mit Schlüssel-Manager-Tool zu installieren.</span><span class="sxs-lookup"><span data-stu-id="35c2b-140">You can use the same steps to install the Secret Manager Tool using for the command line.</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

<span data-ttu-id="35c2b-141">Testen Sie das Schlüssel-Manager-Tool, indem Sie den folgenden Befehl ausführen:</span><span class="sxs-lookup"><span data-stu-id="35c2b-141">Test the Secret Manager tool by running the following command:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="35c2b-142">Das Schlüssel-Manager-Tool werden Verwendung, Optionen und Befehl Hilfe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="35c2b-142">The Secret Manager tool will display usage, options and command help.</span></span>

> [!NOTE]
> <span data-ttu-id="35c2b-143">Müssen Sie sich im selben Verzeichnis wie die *csproj* Datei im definierten Tools ausführen, die *csproj* Datei `DotNetCliToolReference` Knoten.</span><span class="sxs-lookup"><span data-stu-id="35c2b-143">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` nodes.</span></span>

<span data-ttu-id="35c2b-144">Das Schlüssel-Manager-Tool verarbeitet projektspezifische Konfigurationseinstellungen, die in Ihrem Profil gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="35c2b-144">The Secret Manager tool operates on project-specific configuration settings that are stored in your user profile.</span></span> <span data-ttu-id="35c2b-145">Um Benutzer geheime Schlüssel zu verwenden, das Projekt angegeben, muss ein `UserSecretsId` Wert in seine *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="35c2b-145">To use user secrets, the project must specify a `UserSecretsId` value in its *.csproj* file.</span></span> <span data-ttu-id="35c2b-146">Der Wert des `UserSecretsId` ist willkürlich, jedoch ist für das Projekt in der Regel eindeutig.</span><span class="sxs-lookup"><span data-stu-id="35c2b-146">The value of `UserSecretsId` is arbitrary, but is generally unique to the project.</span></span> <span data-ttu-id="35c2b-147">Entwickler in der Regel generiert eine GUID für die `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="35c2b-147">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

<span data-ttu-id="35c2b-148">Hinzufügen einer `UserSecretsId` für das Projekt in der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="35c2b-148">Add a `UserSecretsId` for your project in the *.csproj* file:</span></span>

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

<span data-ttu-id="35c2b-149">Verwenden Sie den geheimen Schlüssel-Manager, um einen geheimen Schlüssel festzulegen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-149">Use the Secret Manager tool to set a secret.</span></span> <span data-ttu-id="35c2b-150">Geben Sie in einem Befehlsfenster aus dem Projektverzeichnis beispielsweise Folgendes ein:</span><span class="sxs-lookup"><span data-stu-id="35c2b-150">For example, in a command window from the project directory, enter the following:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

<span data-ttu-id="35c2b-151">Sie können das Schlüssel-Manager-Tool ausführen, aus anderen Verzeichnissen, aber Sie müssen die `--project` Option aus, um den Pfad zum Übergeben der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="35c2b-151">You can run the Secret Manager tool from other directories, but you must use the `--project` option to pass in the path to the *.csproj* file:</span></span>

```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

<span data-ttu-id="35c2b-152">Sie können auch das Tool Secret-Manager angezeigt wird, entfernen und app-Kennwörter zu löschen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-152">You can also use the Secret Manager tool to list, remove and clear app secrets.</span></span>

---

## <a name="accessing-user-secrets-via-configuration"></a><span data-ttu-id="35c2b-153">Zugreifen auf vertrauliche Benutzerdaten über die Konfiguration</span><span class="sxs-lookup"><span data-stu-id="35c2b-153">Accessing user secrets via configuration</span></span>

<span data-ttu-id="35c2b-154">Sie können die geheimen Schlüssel-Manager über das Konfigurationssystem zugreifen.</span><span class="sxs-lookup"><span data-stu-id="35c2b-154">You access Secret Manager secrets through the configuration system.</span></span> <span data-ttu-id="35c2b-155">Hinzufügen der `Microsoft.Extensions.Configuration.UserSecrets` Verpacken und ausführen [Dotnet Wiederherstellung](/dotnet/core/tools/dotnet-restore).</span><span class="sxs-lookup"><span data-stu-id="35c2b-155">Add the `Microsoft.Extensions.Configuration.UserSecrets` package and run [dotnet restore](/dotnet/core/tools/dotnet-restore).</span></span>

<span data-ttu-id="35c2b-156">Hinzufügen der Benutzer geheime Schlüssel Konfigurationsquelle angibt, die `Startup` Methode:</span><span class="sxs-lookup"><span data-stu-id="35c2b-156">Add the user secrets configuration source to the `Startup` method:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

<span data-ttu-id="35c2b-157">Sie können Benutzer geheime Schlüssel über den Konfigurations-API zugreifen:</span><span class="sxs-lookup"><span data-stu-id="35c2b-157">You can access user secrets via the configuration API:</span></span>

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="35c2b-158">Wie funktioniert das Schlüssel-Manager-tool</span><span class="sxs-lookup"><span data-stu-id="35c2b-158">How the Secret Manager tool works</span></span>

<span data-ttu-id="35c2b-159">Das Schlüssel-Manager-Tool abstrahiert die Implementierungsdetails, z. B., wo und wie die Werte gespeichert werden.</span><span class="sxs-lookup"><span data-stu-id="35c2b-159">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="35c2b-160">Sie können das Tool verwenden, ohne diese Implementierungsdetails.</span><span class="sxs-lookup"><span data-stu-id="35c2b-160">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="35c2b-161">In der aktuellen Version werden die Werte gespeichert, einem [JSON](http://json.org/) Konfigurationsdatei im Verzeichnis Benutzers-Profil:</span><span class="sxs-lookup"><span data-stu-id="35c2b-161">In the current version, the values are stored in a [JSON](http://json.org/) configuration file in the user profile directory:</span></span>

* <span data-ttu-id="35c2b-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="35c2b-162">Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`</span></span>

* <span data-ttu-id="35c2b-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="35c2b-163">Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

* <span data-ttu-id="35c2b-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="35c2b-164">macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`</span></span>

<span data-ttu-id="35c2b-165">Der Wert der `userSecretsId` stammt aus dem Wert im angegebenen *csproj* Datei.</span><span class="sxs-lookup"><span data-stu-id="35c2b-165">The value of `userSecretsId` comes from the value specified in *.csproj* file.</span></span>

<span data-ttu-id="35c2b-166">Sie sollten nicht Code schreiben, der auf dem Speicherort oder das Format der Daten gespeichert werden, mit dem geheimen Schlüssel-Manager-Tool abhängig ist, da diese Implementierungsdetails ändern können.</span><span class="sxs-lookup"><span data-stu-id="35c2b-166">You shouldn't write code that depends on the location or format of the data saved with the Secret Manager tool, as these implementation details might change.</span></span> <span data-ttu-id="35c2b-167">Beispielsweise sind die Werte für den geheimen derzeit *nicht* heute verschlüsselt, aber irgendwann werden konnte.</span><span class="sxs-lookup"><span data-stu-id="35c2b-167">For example, the secret values are currently *not* encrypted today, but could be someday.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="35c2b-168">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="35c2b-168">Additional resources</span></span>

* [<span data-ttu-id="35c2b-169">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="35c2b-169">Configuration</span></span>](xref:fundamentals/configuration/index)
