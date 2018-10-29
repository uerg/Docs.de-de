---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: guardrex
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/25/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 7f19db0a1d12b904daff989bc969daf8d2302bfa
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325782"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hosten von ASP.NET Core in einem Windows-Dienst

Von [Luke Latham](https://github.com/guardrex) und [Tom Dykstra](https://github.com/tdykstra)

Eine ASP.NET Core-App kann unter Windows gehostet werden, ohne IIS als [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) zu verwenden. Wenn die App als Windows-Dienst gehostet wird, erfolgen Neustarts automatisch.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="convert-a-project-into-a-windows-service"></a>Konvertieren eines Projekts in einen Windows-Dienst

Die folgenden minimalen Änderungen sind für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung als Dienst erforderlich:

1. In der Projektdatei:

   * Bestätigen Sie das Vorhandensein eines Windows [Runtime-Bezeichners (RID)](/dotnet/core/rid-catalog), oder fügen Sie diesen der `<PropertyGroup>` hinzu, die das Zielframework enthält:

      ::: moniker range=">= aspnetcore-2.1"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="= aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.0</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      ::: moniker range="< aspnetcore-2.0"

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp1.1</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      ::: moniker-end

      So führen Sie die Veröffentlichung für mehrere RIDs aus:

      * Geben Sie die RIDs in einer durch Semikolons getrennten Liste an.
      * Verwenden Sie den Eigenschaftennamen `<RuntimeIdentifiers>` (Plural).

      Weitere Informationen finden Sie im [.NET Core RID-Katalog](/dotnet/core/rid-catalog).

   * Fügen Sie einen Paketverweis auf [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices) hinzu.

1. Nehmen Sie in `Program.Main` die folgenden Änderungen vor:

   * Rufen Sie [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) anstelle von `host.Run` auf.

   * Rufen Sie [UseContentRoot](xref:fundamentals/host/web-host#content-root) auf, und verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum veröffentlichten Speicherort der App.

     ::: moniker range=">= aspnetcore-2.0"

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

     ::: moniker-end

     ::: moniker range="< aspnetcore-2.0"

     [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=3-4,8,13)]

     ::: moniker-end

1. Veröffentlichen Sie die App. Verwenden Sie [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles). Wählen Sie **FolderProfile** aus, wenn Sie Visual Studio verwenden.

   Um die Beispiel-App mit CLI-Tools (Befehlszeilenschnittstelle) zu veröffentlichen, führen Sie den Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish) an einer Eingabeaufforderung aus dem Projektordner aus. Der RID muss in der Eigenschaft `<RuntimeIdenfifier>` (oder `<RuntimeIdentifiers>`) der Projektdatei angegeben werden. Im folgenden Beispiel wird die App in Releasekonfiguration für die `win7-x64`-Runtime veröffentlicht:

   ```console
   dotnet publish --configuration Release --runtime win7-x64
   ```

1. Verwenden Sie das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995), um den Dienst zu erstellen. Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält. **Das Leerzeichen zwischen dem Gleichheitszeichen und dem Anführungszeichen am Anfang des Pfads ist erforderlich.**

   ```console
   sc create <SERVICE_NAME> binPath= "<PATH_TO_SERVICE_EXECUTABLE>"
   ```

   Verwenden Sie zum Erstellen eines Diensts, der im Projektordner veröffentlicht wird, den Pfad zum *Veröffentlichungsordner*. Im folgenden Beispiel:

   * Das Projekt befindet sich im Ordner *„C:\\meine_dienste\\AspNetCoreService*“.
   * Das Projekt wird in der `Release`-Konfiguration veröffentlicht.
   * Der Zielframeworkmoniker (Target Framework Moniker, TFM) ist `netcoreapp2.1`.
   * Der Runtimebezeichner (RID) ist `win7-x64`.
   * Der Name der ausführbaren App-Datei lautet *AspNetCoreService.exe*.
   * Der Dienst heißt **MyService**.

   Beispiel:

   ```console
   sc create MyService binPath= "c:\my_services\AspNetCoreService\bin\Release\netcoreapp2.1\win7-x64\publish\AspNetCoreService.exe"
   ```

   > [!IMPORTANT]
   > Achten Sie auf das Leerzeichen zwischen dem `binPath=`-Argument und seinem Wert.

   So veröffentlichen und starten Sie den Dienst aus einem anderen Ordner:

      * Verwenden Sie die Option [--output &lt;OUTPUT_DIRECTORY&gt;](/dotnet/core/tools/dotnet-publish#options) im Befehl `dotnet publish`. Wenn Sie Visual Studio verwenden, konfigurieren Sie den **Zielspeicherort** auf der Seite **FolderProfile** der publish-Eigenschaft, bevor Sie auf die Schaltfläche **Veröffentlichen** klicken.
      * Erstellen Sie den Dienst mit dem Befehl `sc.exe`, indem Sie den Pfad des Ausgabeordners verwenden. Fügen Sie den Namen der ausführbaren Datei des Diensts in dem Pfad hinzu, der für `binPath` bereitgestellt wird.

1. Starten Sie den Dienst mithilfe des Befehls `sc start <SERVICE_NAME>`.

   Verwenden Sie zum Starten des Diensts der Beispiel-App den folgenden Befehl:

   ```console
   sc start MyService
   ```

   Das Starten des Diensts dauert ein paar Sekunden.

1. Um den Status des Diensts zu überprüfen, verwenden Sie den `sc query <SERVICE_NAME>`-Befehl. Der Status wird als einer der folgenden Werte gemeldet:

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

1. Verwenden Sie zum Beenden des Diensts den Befehl `sc stop <SERVICE_NAME>`.

   Verwenden Sie zum Beenden des Diensts der Beispiel-App den folgenden Befehl:

   ```console
   sc stop MyService
   ```

1. Deinstallieren Sie den Dienst nach einer kurzen Verzögerung zum Beenden des Diensts mit dem Befehl `sc delete <SERVICE_NAME>`.

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

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

Da für die Konfiguration von ASP.NET Core Name/Wert-Paare für Befehlszeilenargumente erforderlich sind, wird der `--console`-Schalter entfernt, bevor die Argumente an [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) übergeben werden.

> [!NOTE]
> `isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

::: moniker-end

## <a name="handle-stopping-and-starting-events"></a>Behandeln des Stoppens und Startens von Ereignissen

Nehmen Sie zum Behandeln von [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting)-, [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted)- und [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping)-Ereignissen die folgenden zusätzlichen Änderungen vor:

1. Erstellen Sie eine von [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice) abgeleitete Klasse:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. Erstellen Sie eine Erweiterungsmethode für [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost), die den benutzerdefinierten `WebHostService` an [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run) übergibt:

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Rufen Sie in `Program.Main` anstelle von [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) die neue Erweiterungsmethode `RunAsCustomService` auf:

   ::: moniker range=">= aspnetcore-2.0"

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > `isService` wird nicht von `Main` an `CreateWebHostBuilder` übergeben. Die Signatur von `CreateWebHostBuilder` muss `CreateWebHostBuilder(string[])` sein, damit die [Integrationstests](xref:test/integration-tests) korrekt ablaufen.

   ::: moniker-end

   ::: moniker range="< aspnetcore-2.0"

   [!code-csharp[](windows-service/samples_snapshot/1.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=27)]

   ::: moniker-end

Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services)-Eigenschaft ab:

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen. Weitere Informationen finden Sie unter <xref:host-and-deploy/proxy-load-balancer>.

## <a name="configure-https"></a>HTTPS konfigurieren

Geben Sie einen [Kestrel-Server-HTTPS-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) ein.

## <a name="current-directory-and-content-root"></a>Aktuelles Verzeichnis und Inhaltsstammverzeichnis

Das aktuelle Arbeitsverzeichnis, das durch Aufrufen von `Directory.GetCurrentDirectory()` für einen Windows-Dienst zurückgegeben wird, ist der Ordner *C:\\WINDOWS\\system32*. Der Ordner *system32* ist kein geeigneter Speicherort für die Dateien eines Diensts (z.B. Einstellungsdateien). Verwenden Sie einen der folgenden Ansätze, um die Ressourcen und Einstellungsdateien eines Diensts mit [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) zu verwalten und auf diese zuzugreifen, wenn Sie [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) verwenden:

* Verwenden Sie den Inhaltsstammpfad. `IHostingEnvironment.ContentRootPath` entspricht dem gleichen Pfad, der für das Argument `binPath` bereitgestellt wird, wenn der Dienst erstellt wird. Anstatt `Directory.GetCurrentDirectory()` zum Erstellen von Pfaden zu Einstellungsdateien zu verwenden, verwenden Sie den Inhaltsstammpfad, und verwalten Sie die Dateien im Inhaltsstammverzeichnis der App.
* Speichern Sie die Dateien an einem geeigneten Speicherort auf dem Datenträger. Geben Sie mit `SetBasePath` einen absoluten Pfad zu dem Ordner an, der die Dateien enthält.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Kestrel-Endpunktkonfiguration](xref:fundamentals/servers/kestrel#endpoint-configuration) (einschließlich HTTPS-Konfiguration und Unterstützung für SNI)
* <xref:fundamentals/host/web-host>
