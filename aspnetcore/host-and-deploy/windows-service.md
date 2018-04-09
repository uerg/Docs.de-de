---
title: Hosten von ASP.NET Core in einem Windowsdienst
author: tdykstra
description: Erfahren Sie, wie eine ASP.NET Core-app in einem Windows-Dienst zu hosten.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: b0b27f274de1ca88b20bf582127132527b553ce0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hosten von ASP.NET Core in einem Windowsdienst

Von [Tom Dykstra](https://github.com/tdykstra)

Die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS ist die Ausführung in einem [Windowsdienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications). Wenn als Windows-Dienst gehostet wird, kann die app automatisch nach dem Start neu gestartet und ohne Benutzereingriff stürzt ab.

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)). Anweisungen zum Ausführen der Beispiel-app finden Sie im Beispiels *README.md* Datei.

## <a name="prerequisites"></a>Erforderliche Komponenten

* In der .NET Framework-Laufzeit muss die app auszuführen. In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](/nuget/schema/target-frameworks) und [RuntimeIdentifier](/dotnet/articles/core/rid-catalog). Im Folgenden ein Beispiel:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.

* Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) Anforderungen empfängt, verwenden sie die [HTTP.sys](xref:fundamentals/servers/httpsys) Webserver (früher bekannt als [WebListener](xref:fundamentals/servers/weblistener) für ASP.NET Core 1.x-apps) anstatt [Kestrel](xref:fundamentals/servers/kestrel). IIS ist für die Verwendung als reverse-Proxy-Server mit Kestrel für Edge-Bereitstellungen empfohlen. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Erste Schritte

In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.

1. Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Nehmen Sie die folgenden Änderungen in `Program.Main`:

   * Rufen Sie `host.RunAsService` anstelle von `host.Run`.

   * Wenn der Code ruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von `Directory.GetCurrentDirectory()`.

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   * * *

3. Veröffentlichen Sie die app in einem Ordner. Verwendung [Dotnet veröffentlichen](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) , die in einen Ordner veröffentlicht.

4. Testen von erstellen und den Dienst zu starten.

   Öffnen Sie eine Befehlsshell mit Administratorrechten verwendet die [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts. Wenn der Dienst "MyService" benannt ist, veröffentlicht `c:\svc`, und mit dem Namen AspNetCoreService, die Befehle sind:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   Die `binPath` Wert ist der Pfad zur ausführbaren Datei die app, die den Namen der ausführbaren Datei enthält.

   ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

   Wenn diese Befehle abgeschlossen haben, navigieren Sie zu dem gleichen Pfad wie bei Ausführung als eine Konsolen-app (standardmäßig `http://localhost:5000`):

   ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen

Es ist einfacher, testen und Debuggen außerhalb von einem Dienst ausgeführt wird, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `RunAsService` nur unter bestimmten Bedingungen. Beispielsweise kann die app auszuführen, als eine Konsolen-app mit einer `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist:

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

* * *
## <a name="handle-stopping-and-starting-events"></a>Behandeln von Ereignissen starten und beenden

Behandeln `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:

1. Erstellen Sie eine Klasse, die abgeleitet `WebHostService`:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die die benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. In `Program.Main`, rufen Sie die neue Erweiterungsmethode `RunAsCustomService`, anstelle von `RunAsService`:

   #### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   #### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   * * *
Wenn die benutzerdefinierte `WebHostService` Code erfordert einen Dienst aus abhängigkeiteneinschleusung (z. B. eine Protokollierung), erhalten sie über die `Services` Eigenschaft `IWebHost`:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Load Balancer-Szenarien

Dienste, die Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und werden hinter einem Proxy oder den load Balancer möglicherweise zusätzliche Konfiguration erforderlich ist. Weitere Informationen finden Sie unter [konfigurieren ASP.NET Core zum Arbeiten mit Proxyservern und load balancer](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Danksagungen

In diesem Artikel wurde mithilfe von veröffentlichten Datenquellen geschrieben:

* [Hosten von ASP.NET Core als Windows-Dienst](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
