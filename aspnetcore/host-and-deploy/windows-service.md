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
# <a name="host-aspnet-core-in-a-windows-service"></a>Hosten von ASP.NET Core in einem Windows-Dienst

Von [Luke Latham](https://github.com/guardrex) und [Tom Dykstra](https://github.com/tdykstra)

Eine ASP.NET Core-App kann unter Windows gehostet werden, ohne IIS als [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) zu verwenden. Wenn die App als Windows-Dienst gehostet wird, erfolgen Neustarts automatisch.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Konvertieren eines Projekts in einen Windows-Dienst

Die folgenden minimalen Änderungen sind für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung als Dienst erforderlich:

1. In der Projektdatei:

   * Bestätigen Sie das Vorhandensein eines Windows [Runtime-Bezeichners (RID)](/dotnet/core/rid-catalog), oder fügen Sie diesen der `<PropertyGroup>` hinzu, die das Zielframework enthält:

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      So führen Sie die Veröffentlichung für mehrere RIDs aus:

      * Geben Sie die RIDs in einer durch Semikolons getrennten Liste an.
      * Verwenden Sie den Eigenschaftennamen `<RuntimeIdentifiers>` (Plural).

      Weitere Informationen finden Sie im [.NET Core RID-Katalog](/dotnet/core/rid-catalog).

   * Fügen Sie einen Paketverweis auf [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) hinzu.

1. Nehmen Sie in `Program.Main` die folgenden Änderungen vor:

   * Rufen Sie [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) anstelle von `host.Run` auf.

   * Rufen Sie [UseContentRoot](xref:fundamentals/host/web-host#content-root) auf, und verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum veröffentlichten Speicherort der App.

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. Veröffentlichen Sie die App mit [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), einem [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) oder Visual Studio Code. Wählen Sie bei Verwendung von Visual Studio das **FolderProfile** aus, und konfigurieren Sie den **Zielspeicherort**, bevor Sie auf die Schaltfläche **Veröffentlichen** klicken.

   Um die Beispiel-App mit CLI-Tools (Befehlszeilenschnittstelle) zu veröffentlichen, führen Sie den Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) an einer Eingabeaufforderung aus dem Projektordner aus. Der RID muss in der Eigenschaft `<RuntimeIdenfifier>` (oder `<RuntimeIdentifiers>`) der Projektdatei angegeben werden. Im folgenden Beispiel wird die App in der Releasekonfiguration für die `win7-x64`-Runtime in einem unter *c:\\svc* erstellten Ordner veröffentlicht:

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. Erstellen Sie mit dem `net user`-Befehl ein Benutzerkonto für den Dienst:

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   Erstellen Sie für die Beispiel-App ein Benutzerkonto mit dem Namen `ServiceUser` und einem Kennwort. Ersetzen Sie im folgenden Befehl `{PASSWORD}` durch ein [sicheres Kennwort](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   Wenn Sie den Benutzer einer Gruppe hinzufügen müssen, verwenden Sie den Befehl `net localgroup`. Hierbei steht `{GROUP}` für den Namen der Gruppe:

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   Weitere Informationen finden Sie unter [Dienstbenutzerkonten](/windows/desktop/services/service-user-accounts).

1. Gewähren Sie mit dem Befehl [icacls](/windows-server/administration/windows-commands/icacls) Schreib-/Lese-/Ausführungszugriff für den App-Ordner:

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * `{PATH}` &ndash; Pfad zum App-Ordner.
   * `{USER ACCOUNT}` &ndash; Das Benutzerkonto (SID).
   * `(OI)`&ndash; Mit dem Flag für die Objektvererbung werden Berechtigungen an untergeordnete Dateien weitergegeben.
   * `(CI)`&ndash; Mit dem Flag für die Containervererbung werden Berechtigungen an untergeordnete Ordner weitergegeben.
   * `{PERMISSION FLAGS}` &ndash; Legt die Zugriffsberechtigungen für die App fest.
     * Schreiben (`W`)
     * Lesen (`R`)
     * Ausführen (`X`)
     * Vollzugriff (`F`)
     * Ändern (`M`)
   * `/t` &ndash; Rekursive Anwendung auf vorhandene untergeordnete Ordner und Dateien.

   Verwenden Sie für die im Ordner *c:\\svc* veröffentlichte Beispiel-App und das Konto `ServiceUser` mit Schreib-/Lese-/Ausführungsberechtigungen den folgenden Befehl:

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   Weitere Informationen finden Sie unter [icacls](/windows-server/administration/windows-commands/icacls).

1. Verwenden Sie das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995), um den Dienst zu erstellen. Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält. **Das Leerzeichen zwischen dem Gleichheitszeichen und dem Anführungszeichen für jeden Parameter und Wert ist erforderlich.**

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * `{SERVICE NAME}` &ndash; Der Name, der dem Dienst im [Dienststeuerungs-Manager](/windows/desktop/services/service-control-manager) zugewiesen wird.
   * `{PATH}`&ndash; Der Pfad zur ausführbaren Datei des Diensts.
   * `{DOMAIN}` (oder der lokale Computername, wenn der Computer nicht in die Domäne eingebunden ist) und `{USER ACCOUNT}` &ndash; Die Domäne (oder der lokale Computername) und das Benutzerkonto, mit dem der Dienst ausgeführt wird. Lassen Sie den Parameter`obj` **nicht** aus. Der Standardwert für `obj` ist das [LocalSystem-Konto](/windows/desktop/services/localsystem-account). Die Ausführung eines Diensts mit dem `LocalSystem`-Konto stellt ein erhebliches Sicherheitsrisiko dar. Führen Sie einen Dienst immer mit einem Benutzerkonto mit eingeschränkten Berechtigungen auf dem Server aus.
   * `{PASSWORD}` &ndash; Das Kennwort für das Benutzerkonto.

   Im folgenden Beispiel:

   * Der Dienst heißt **MyService**.
   * Der veröffentlichte Dienst ist im Ordner *c:\\svc* vorhanden. Der Name der ausführbaren App-Datei lautet *AspNetCoreService.exe*. Der `binPath`-Wert wird in doppelte gerade Anführungszeichen (") eingeschlossen.
   * Der Dienst wird mit dem Konto `ServiceUser` ausgeführt. Ersetzen Sie `{DOMAIN}` durch den Domänennamen oder den lokalen Computernamen für das Benutzerkonto. Schließen Sie den Wert `obj` in doppelte gerade Anführungszeichen (") ein. Beispiel: Wenn das Hostsystem ein lokaler Computer mit Namen `MairaPC` ist, legen Sie `obj` auf `"MairaPC\ServiceUser"` fest.
   * Ersetzen Sie `{PASSWORD}` durch das Kennwort für das Benutzerkonto. Der `password`-Wert wird in doppelte gerade Anführungszeichen (") eingeschlossen.

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > Stellen Sie sicher, dass Leerzeichen zwischen den Gleichheitszeichen des Parameters und den Parameterwerten vorhanden sind.

1. Starten Sie den Dienst mithilfe des Befehls `sc start {SERVICE NAME}`.

   Verwenden Sie zum Starten des Diensts der Beispiel-App den folgenden Befehl:

   ```console
   sc start MyService
   ```

   Das Starten des Diensts dauert ein paar Sekunden.

1. Um den Status des Diensts zu überprüfen, verwenden Sie den `sc query {SERVICE NAME}`-Befehl. Der Status wird als einer der folgenden Werte gemeldet:

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   Verwenden Sie den folgenden Befehl, um den Status des Diensts der Beispiel-App zu überprüfen:

   ```console
   sc query MyService
   ```

1. Befindet sich der Dienst im Status `RUNNING`, und ist der Dienst gleichzeitig eine Web-App, rufen Sie die App über ihren Pfad auf (Standardpfad ist `http://localhost:5000`, der umgeleitet wird zu `https://localhost:5001`, wenn [Middleware für HTTPS-Umleitung](xref:security/enforcing-ssl) verwendet wird).

   Rufen Sie die App für den Dienst der Beispiel-App über `http://localhost:5000` auf.

1. Verwenden Sie zum Beenden des Diensts den Befehl `sc stop {SERVICE NAME}`.

   Verwenden Sie zum Beenden des Diensts der Beispiel-App den folgenden Befehl:

   ```console
   sc stop MyService
   ```

1. Deinstallieren Sie den Dienst nach einer kurzen Verzögerung zum Beenden des Diensts mit dem Befehl `sc delete {SERVICE NAME}`.

   Überprüfen Sie den Status des Diensts der Beispiel-App:

   ```console
   sc query MyService
   ```

   Befindet sich der Dienst der Beispiel-App im Status `STOPPED`, verwenden Sie zum Deinstallieren des Diensts der Beispiel-App den folgenden Befehl:

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a>Ausführen der App außerhalb eines Diensts

Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft. Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Da für die Konfiguration von ASP.NET Core Name/Wert-Paare für Befehlszeilenargumente erforderlich sind, wird der `--console`-Schalter entfernt, bevor die Argumente an [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) übergeben werden.

> [!NOTE]
> `isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.

## <a name="handle-stopping-and-starting-events"></a>Behandeln des Stoppens und Startens von Ereignissen

Nehmen Sie zum Behandeln von [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)-, [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)- und [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping)-Ereignissen die folgenden zusätzlichen Änderungen vor:

1. Erstellen Sie eine von [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) abgeleitete Klasse:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Erstellen Sie eine Erweiterungsmethode für [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), die den benutzerdefinierten `WebHostService` an [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) übergibt:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Rufen Sie in `Program.Main` anstelle von [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) die neue Erweiterungsmethode `RunAsCustomService` auf:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.

Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services)-Eigenschaft ab:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen. Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>HTTPS konfigurieren

So konfigurieren Sie den Dienst mit einem sicheren Endpunkt:

1. Erstellen Sie über die Mechanismen zum Abrufen und Bereitstellen eines Zertifikats für Ihre Plattform ein X.509-Zertifikat für das Hostsystem.
1. Geben Sie eine [Kestrel-Server-HTTPS-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) an, um das Zertifikat zu verwenden.

Die Verwendung des ASP.NET Core-HTTPS-Entwicklerzertifikats zum Schützen eines Dienstendpunkts wird nicht unterstützt.

## <a name="current-directory-and-content-root"></a>Aktuelles Verzeichnis und Inhaltsstammverzeichnis

Das aktuelle Arbeitsverzeichnis, das durch Aufrufen von `Directory.GetCurrentDirectory()` für einen Windows-Dienst zurückgegeben wird, ist der Ordner *C:\\WINDOWS\\system32*. Der Ordner *system32* ist kein geeigneter Speicherort für die Dateien eines Diensts (z.B. Einstellungsdateien). Verwenden Sie einen der folgenden Ansätze, um die Ressourcen und Einstellungsdateien eines Diensts mit [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) zu verwalten und auf diese zuzugreifen, wenn Sie [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) verwenden:

* Verwenden Sie den Inhaltsstammpfad. `IHostingEnvironment.ContentRootPath` entspricht dem gleichen Pfad, der für das Argument `binPath` bereitgestellt wird, wenn der Dienst erstellt wird. Anstatt `Directory.GetCurrentDirectory()` zum Erstellen von Pfaden zu Einstellungsdateien zu verwenden, verwenden Sie den Inhaltsstammpfad, und verwalten Sie die Dateien im Inhaltsstammverzeichnis der App.
* Speichern Sie die Dateien an einem geeigneten Speicherort auf dem Datenträger. Geben Sie mit `SetBasePath` einen absoluten Pfad zu dem Ordner an, der die Dateien enthält.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) (einschließlich HTTPS-Konfiguration und Unterstützung für SNI)
* <xref:fundamentals/host/web-host>
