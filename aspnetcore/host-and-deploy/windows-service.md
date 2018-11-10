---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: guardrex
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758192"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="fae50-103">Hosten von ASP.NET Core in einem Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="fae50-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="fae50-104">Von [Luke Latham](https://github.com/guardrex) und [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fae50-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="fae50-105">Eine ASP.NET Core-App kann unter Windows gehostet werden, ohne IIS als [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fae50-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="fae50-106">Wenn die App als Windows-Dienst gehostet wird, erfolgen Neustarts automatisch.</span><span class="sxs-lookup"><span data-stu-id="fae50-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="fae50-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fae50-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="fae50-108">Konvertieren eines Projekts in einen Windows-Dienst</span><span class="sxs-lookup"><span data-stu-id="fae50-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="fae50-109">Die folgenden minimalen Änderungen sind für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung als Dienst erforderlich:</span><span class="sxs-lookup"><span data-stu-id="fae50-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="fae50-110">In der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="fae50-110">In the project file:</span></span>

   * <span data-ttu-id="fae50-111">Bestätigen Sie das Vorhandensein eines Windows [Runtime-Bezeichners (RID)](/dotnet/core/rid-catalog), oder fügen Sie diesen der `<PropertyGroup>` hinzu, die das Zielframework enthält:</span><span class="sxs-lookup"><span data-stu-id="fae50-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="fae50-112">So führen Sie die Veröffentlichung für mehrere RIDs aus:</span><span class="sxs-lookup"><span data-stu-id="fae50-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="fae50-113">Geben Sie die RIDs in einer durch Semikolons getrennten Liste an.</span><span class="sxs-lookup"><span data-stu-id="fae50-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="fae50-114">Verwenden Sie den Eigenschaftennamen `<RuntimeIdentifiers>` (Plural).</span><span class="sxs-lookup"><span data-stu-id="fae50-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="fae50-115">Weitere Informationen finden Sie im [.NET Core RID-Katalog](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="fae50-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="fae50-116">Fügen Sie einen Paketverweis auf [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) hinzu.</span><span class="sxs-lookup"><span data-stu-id="fae50-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="fae50-117">Nehmen Sie in `Program.Main` die folgenden Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="fae50-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="fae50-118">Rufen Sie [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) anstelle von `host.Run` auf.</span><span class="sxs-lookup"><span data-stu-id="fae50-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="fae50-119">Rufen Sie [UseContentRoot](xref:fundamentals/host/web-host#content-root) auf, und verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum veröffentlichten Speicherort der App.</span><span class="sxs-lookup"><span data-stu-id="fae50-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="fae50-120">Veröffentlichen Sie die App mit [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), einem [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) oder Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="fae50-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="fae50-121">Wählen Sie bei Verwendung von Visual Studio das **FolderProfile** aus, und konfigurieren Sie den **Zielspeicherort**, bevor Sie auf die Schaltfläche **Veröffentlichen** klicken.</span><span class="sxs-lookup"><span data-stu-id="fae50-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="fae50-122">Um die Beispiel-App mit CLI-Tools (Befehlszeilenschnittstelle) zu veröffentlichen, führen Sie den Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) an einer Eingabeaufforderung aus dem Projektordner aus.</span><span class="sxs-lookup"><span data-stu-id="fae50-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="fae50-123">Der RID muss in der Eigenschaft `<RuntimeIdenfifier>` (oder `<RuntimeIdentifiers>`) der Projektdatei angegeben werden.</span><span class="sxs-lookup"><span data-stu-id="fae50-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="fae50-124">Im folgenden Beispiel wird die App in der Releasekonfiguration für die `win7-x64`-Runtime in einem unter *c:\\svc* erstellten Ordner veröffentlicht:</span><span class="sxs-lookup"><span data-stu-id="fae50-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="fae50-125">Erstellen Sie mit dem `net user`-Befehl ein Benutzerkonto für den Dienst:</span><span class="sxs-lookup"><span data-stu-id="fae50-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="fae50-126">Erstellen Sie für die Beispiel-App ein Benutzerkonto mit dem Namen `ServiceUser` und einem Kennwort.</span><span class="sxs-lookup"><span data-stu-id="fae50-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="fae50-127">Ersetzen Sie im folgenden Befehl `{PASSWORD}` durch ein [sicheres Kennwort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="fae50-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="fae50-128">Wenn Sie den Benutzer einer Gruppe hinzufügen müssen, verwenden Sie den Befehl `net localgroup`. Hierbei steht `{GROUP}` für den Namen der Gruppe:</span><span class="sxs-lookup"><span data-stu-id="fae50-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="fae50-129">Weitere Informationen finden Sie unter [Dienstbenutzerkonten](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="fae50-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="fae50-130">Gewähren Sie mit dem Befehl [icacls](/windows-server/administration/windows-commands/icacls) Schreib-/Lese-/Ausführungszugriff für den App-Ordner:</span><span class="sxs-lookup"><span data-stu-id="fae50-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="fae50-131">`{PATH}` &ndash; Pfad zum App-Ordner.</span><span class="sxs-lookup"><span data-stu-id="fae50-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="fae50-132">`{USER ACCOUNT}` &ndash; Das Benutzerkonto (SID).</span><span class="sxs-lookup"><span data-stu-id="fae50-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="fae50-133">`(OI)`&ndash; Mit dem Flag für die Objektvererbung werden Berechtigungen an untergeordnete Dateien weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="fae50-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="fae50-134">`(CI)`&ndash; Mit dem Flag für die Containervererbung werden Berechtigungen an untergeordnete Ordner weitergegeben.</span><span class="sxs-lookup"><span data-stu-id="fae50-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="fae50-135">`{PERMISSION FLAGS}` &ndash; Legt die Zugriffsberechtigungen für die App fest.</span><span class="sxs-lookup"><span data-stu-id="fae50-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="fae50-136">Schreiben (`W`)</span><span class="sxs-lookup"><span data-stu-id="fae50-136">Write (`W`)</span></span>
     * <span data-ttu-id="fae50-137">Lesen (`R`)</span><span class="sxs-lookup"><span data-stu-id="fae50-137">Read (`R`)</span></span>
     * <span data-ttu-id="fae50-138">Ausführen (`X`)</span><span class="sxs-lookup"><span data-stu-id="fae50-138">Execute (`X`)</span></span>
     * <span data-ttu-id="fae50-139">Vollzugriff (`F`)</span><span class="sxs-lookup"><span data-stu-id="fae50-139">Full (`F`)</span></span>
     * <span data-ttu-id="fae50-140">Ändern (`M`)</span><span class="sxs-lookup"><span data-stu-id="fae50-140">Modify (`M`)</span></span>
   * <span data-ttu-id="fae50-141">`/t` &ndash; Rekursive Anwendung auf vorhandene untergeordnete Ordner und Dateien.</span><span class="sxs-lookup"><span data-stu-id="fae50-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="fae50-142">Verwenden Sie für die im Ordner *c:\\svc* veröffentlichte Beispiel-App und das Konto `ServiceUser` mit Schreib-/Lese-/Ausführungsberechtigungen den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="fae50-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="fae50-143">Weitere Informationen finden Sie unter [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="fae50-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="fae50-144">Verwenden Sie das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995), um den Dienst zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fae50-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="fae50-145">Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält.</span><span class="sxs-lookup"><span data-stu-id="fae50-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="fae50-146">**Das Leerzeichen zwischen dem Gleichheitszeichen und dem Anführungszeichen für jeden Parameter und Wert ist erforderlich.**</span><span class="sxs-lookup"><span data-stu-id="fae50-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="fae50-147">`{SERVICE NAME}` &ndash; Der Name, der dem Dienst im [Dienststeuerungs-Manager](/windows/desktop/services/service-control-manager) zugewiesen wird.</span><span class="sxs-lookup"><span data-stu-id="fae50-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="fae50-148">`{PATH}`&ndash; Der Pfad zur ausführbaren Datei des Diensts.</span><span class="sxs-lookup"><span data-stu-id="fae50-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="fae50-149">`{DOMAIN}` (oder der lokale Computername, wenn der Computer nicht in die Domäne eingebunden ist) und `{USER ACCOUNT}` &ndash; Die Domäne (oder der lokale Computername) und das Benutzerkonto, mit dem der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="fae50-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="fae50-150">Lassen Sie den Parameter`obj` **nicht** aus.</span><span class="sxs-lookup"><span data-stu-id="fae50-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="fae50-151">Der Standardwert für `obj` ist das [LocalSystem-Konto](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="fae50-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="fae50-152">Die Ausführung eines Diensts mit dem `LocalSystem`-Konto stellt ein erhebliches Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="fae50-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="fae50-153">Führen Sie einen Dienst immer mit einem Benutzerkonto mit eingeschränkten Berechtigungen auf dem Server aus.</span><span class="sxs-lookup"><span data-stu-id="fae50-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="fae50-154">`{PASSWORD}` &ndash; Das Kennwort für das Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="fae50-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="fae50-155">Im folgenden Beispiel:</span><span class="sxs-lookup"><span data-stu-id="fae50-155">In the following example:</span></span>

   * <span data-ttu-id="fae50-156">Der Dienst heißt **MyService**.</span><span class="sxs-lookup"><span data-stu-id="fae50-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="fae50-157">Der veröffentlichte Dienst ist im Ordner *c:\\svc* vorhanden.</span><span class="sxs-lookup"><span data-stu-id="fae50-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="fae50-158">Der Name der ausführbaren App-Datei lautet *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="fae50-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="fae50-159">Der `binPath`-Wert wird in doppelte gerade Anführungszeichen (") eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="fae50-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="fae50-160">Der Dienst wird mit dem Konto `ServiceUser` ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="fae50-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="fae50-161">Ersetzen Sie `{DOMAIN}` durch den Domänennamen oder den lokalen Computernamen für das Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="fae50-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="fae50-162">Schließen Sie den Wert `obj` in doppelte gerade Anführungszeichen (") ein.</span><span class="sxs-lookup"><span data-stu-id="fae50-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="fae50-163">Beispiel: Wenn das Hostsystem ein lokaler Computer mit Namen `MairaPC` ist, legen Sie `obj` auf `"MairaPC\ServiceUser"` fest.</span><span class="sxs-lookup"><span data-stu-id="fae50-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="fae50-164">Ersetzen Sie `{PASSWORD}` durch das Kennwort für das Benutzerkonto.</span><span class="sxs-lookup"><span data-stu-id="fae50-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="fae50-165">Der `password`-Wert wird in doppelte gerade Anführungszeichen (") eingeschlossen.</span><span class="sxs-lookup"><span data-stu-id="fae50-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="fae50-166">Stellen Sie sicher, dass Leerzeichen zwischen den Gleichheitszeichen des Parameters und den Parameterwerten vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="fae50-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="fae50-167">Starten Sie den Dienst mithilfe des Befehls `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="fae50-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="fae50-168">Verwenden Sie zum Starten des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="fae50-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="fae50-169">Das Starten des Diensts dauert ein paar Sekunden.</span><span class="sxs-lookup"><span data-stu-id="fae50-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="fae50-170">Um den Status des Diensts zu überprüfen, verwenden Sie den `sc query {SERVICE NAME}`-Befehl.</span><span class="sxs-lookup"><span data-stu-id="fae50-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="fae50-171">Der Status wird als einer der folgenden Werte gemeldet:</span><span class="sxs-lookup"><span data-stu-id="fae50-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="fae50-172">Verwenden Sie den folgenden Befehl, um den Status des Diensts der Beispiel-App zu überprüfen:</span><span class="sxs-lookup"><span data-stu-id="fae50-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="fae50-173">Befindet sich der Dienst im Status `RUNNING`, und ist der Dienst gleichzeitig eine Web-App, rufen Sie die App über ihren Pfad auf (Standardpfad ist `http://localhost:5000`, der umgeleitet wird zu `https://localhost:5001`, wenn [Middleware für HTTPS-Umleitung](xref:security/enforcing-ssl) verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="fae50-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="fae50-174">Rufen Sie die App für den Dienst der Beispiel-App über `http://localhost:5000` auf.</span><span class="sxs-lookup"><span data-stu-id="fae50-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="fae50-175">Verwenden Sie zum Beenden des Diensts den Befehl `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="fae50-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="fae50-176">Verwenden Sie zum Beenden des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="fae50-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="fae50-177">Deinstallieren Sie den Dienst nach einer kurzen Verzögerung zum Beenden des Diensts mit dem Befehl `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="fae50-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="fae50-178">Überprüfen Sie den Status des Diensts der Beispiel-App:</span><span class="sxs-lookup"><span data-stu-id="fae50-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="fae50-179">Befindet sich der Dienst der Beispiel-App im Status `STOPPED`, verwenden Sie zum Deinstallieren des Diensts der Beispiel-App den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="fae50-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="fae50-180">Ausführen der App außerhalb eines Diensts</span><span class="sxs-lookup"><span data-stu-id="fae50-180">Run the app outside of a service</span></span>

<span data-ttu-id="fae50-181">Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft.</span><span class="sxs-lookup"><span data-stu-id="fae50-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="fae50-182">Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:</span><span class="sxs-lookup"><span data-stu-id="fae50-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="fae50-183">Da für die Konfiguration von ASP.NET Core Name/Wert-Paare für Befehlszeilenargumente erforderlich sind, wird der `--console`-Schalter entfernt, bevor die Argumente an [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="fae50-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="fae50-184">`isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.</span><span class="sxs-lookup"><span data-stu-id="fae50-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="fae50-185">Behandeln des Stoppens und Startens von Ereignissen</span><span class="sxs-lookup"><span data-stu-id="fae50-185">Handle stopping and starting events</span></span>

<span data-ttu-id="fae50-186">Nehmen Sie zum Behandeln von [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)-, [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)- und [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping)-Ereignissen die folgenden zusätzlichen Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="fae50-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="fae50-187">Erstellen Sie eine von [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) abgeleitete Klasse:</span><span class="sxs-lookup"><span data-stu-id="fae50-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="fae50-188">Erstellen Sie eine Erweiterungsmethode für [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), die den benutzerdefinierten `WebHostService` an [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) übergibt:</span><span class="sxs-lookup"><span data-stu-id="fae50-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="fae50-189">Rufen Sie in `Program.Main` anstelle von [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) die neue Erweiterungsmethode `RunAsCustomService` auf:</span><span class="sxs-lookup"><span data-stu-id="fae50-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="fae50-190">`isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.</span><span class="sxs-lookup"><span data-stu-id="fae50-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="fae50-191">Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services)-Eigenschaft ab:</span><span class="sxs-lookup"><span data-stu-id="fae50-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="fae50-192">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="fae50-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="fae50-193">Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fae50-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="fae50-194">Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="fae50-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="fae50-195">HTTPS konfigurieren</span><span class="sxs-lookup"><span data-stu-id="fae50-195">Configure HTTPS</span></span>

<span data-ttu-id="fae50-196">So konfigurieren Sie den Dienst mit einem sicheren Endpunkt:</span><span class="sxs-lookup"><span data-stu-id="fae50-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="fae50-197">Erstellen Sie über die Mechanismen zum Abrufen und Bereitstellen eines Zertifikats für Ihre Plattform ein X.509-Zertifikat für das Hostsystem.</span><span class="sxs-lookup"><span data-stu-id="fae50-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="fae50-198">Geben Sie eine [Kestrel-Server-HTTPS-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) an, um das Zertifikat zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="fae50-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="fae50-199">Die Verwendung des ASP.NET Core-HTTPS-Entwicklerzertifikats zum Schützen eines Dienstendpunkts wird nicht unterstützt.</span><span class="sxs-lookup"><span data-stu-id="fae50-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="fae50-200">Aktuelles Verzeichnis und Inhaltsstammverzeichnis</span><span class="sxs-lookup"><span data-stu-id="fae50-200">Current directory and content root</span></span>

<span data-ttu-id="fae50-201">Das aktuelle Arbeitsverzeichnis, das durch Aufrufen von `Directory.GetCurrentDirectory()` für einen Windows-Dienst zurückgegeben wird, ist der Ordner *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="fae50-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="fae50-202">Der Ordner *system32* ist kein geeigneter Speicherort für die Dateien eines Diensts (z.B. Einstellungsdateien).</span><span class="sxs-lookup"><span data-stu-id="fae50-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="fae50-203">Verwenden Sie einen der folgenden Ansätze, um die Ressourcen und Einstellungsdateien eines Diensts mit [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) zu verwalten und auf diese zuzugreifen, wenn Sie [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) verwenden:</span><span class="sxs-lookup"><span data-stu-id="fae50-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="fae50-204">Verwenden Sie den Inhaltsstammpfad.</span><span class="sxs-lookup"><span data-stu-id="fae50-204">Use the content root path.</span></span> <span data-ttu-id="fae50-205">`IHostingEnvironment.ContentRootPath` entspricht dem gleichen Pfad, der für das Argument `binPath` bereitgestellt wird, wenn der Dienst erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="fae50-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="fae50-206">Anstatt `Directory.GetCurrentDirectory()` zum Erstellen von Pfaden zu Einstellungsdateien zu verwenden, verwenden Sie den Inhaltsstammpfad, und verwalten Sie die Dateien im Inhaltsstammverzeichnis der App.</span><span class="sxs-lookup"><span data-stu-id="fae50-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="fae50-207">Speichern Sie die Dateien an einem geeigneten Speicherort auf dem Datenträger.</span><span class="sxs-lookup"><span data-stu-id="fae50-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="fae50-208">Geben Sie mit `SetBasePath` einen absoluten Pfad zu dem Ordner an, der die Dateien enthält.</span><span class="sxs-lookup"><span data-stu-id="fae50-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fae50-209">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="fae50-209">Additional resources</span></span>

* <span data-ttu-id="fae50-210">[Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) (einschließlich HTTPS-Konfiguration und Unterstützung für SNI)</span><span class="sxs-lookup"><span data-stu-id="fae50-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
