---
title: Hosten von ASP.NET Core in einem Windows-Dienst
author: rick-anderson
description: Erfahren Sie, wie eine ASP.NET Core-App in einem Windows-Dienst gehostet wird.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 01/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: 29f83ee585c73aeb57a09f70ea8e28650c05ce69
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
ms.locfileid: "34153528"
---
# <a name="host-aspnet-core-in-a-windows-service"></a>Hosten von ASP.NET Core in einem Windows-Dienst

Von [Tom Dykstra](https://github.com/tdykstra)

Wenn Sie eine ASP.NET Core-App unter Windows ohne Verwendung von IIS hosten möchten, wird empfohlen, diese in einem [Windows-Dienst](/dotnet/framework/windows-services/introduction-to-windows-service-applications) auszuführen. Wird die App als Windows-Dienst gehostet, kann sie nach Neustarts und Abstürzen automatisch gestartet werden, ohne dass manuelles Eingreifen erforderlich wird.

[Zeigen Sie Beispielcode an, oder laden Sie diesen herunter](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)). Anweisungen zum Ausführen der Beispiel-App finden Sie in der Datei *README.md* des Beispiels.

## <a name="prerequisites"></a>Erforderliche Komponenten

* Die App muss in der .NET Framework-Runtime ausgeführt werden. Geben Sie in der *CSPROJ*-Datei entsprechende Werte für [TargetFramework](/nuget/schema/target-frameworks) und [RuntimeIdentifier](/dotnet/articles/core/rid-catalog) an. Im Folgenden ein Beispiel:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Verwenden Sie bei der Erstellung eines Projekts in Visual Studio die Vorlage **ASP.NET Core-Anwendung (.NET Framework)**.

* Wenn die App Anforderungen über das Internet empfängt (nicht nur über ein internes Netzwerk), muss Sie den [HTTP.sys](xref:fundamentals/servers/httpsys)-Webserver (früher bekannt als [WebListener](xref:fundamentals/servers/weblistener) für ASP.NET Core 1.x-Apps) anstelle von [Kestrel](xref:fundamentals/servers/kestrel) verwenden. Für die Verwendung als Reverseproxyserver mit Kestrel für Edge-Bereitstellungen wird IIS empfohlen. Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="get-started"></a>Erste Schritte

In diesem Abschnitt wird erläutert, welche minimalen Änderungen für die Einrichtung eines vorhandenen ASP.NET Core-Projekts zur Ausführung in einem Dienst erforderlich sind.

1. Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

2. Nehmen Sie in `Program.Main` die folgenden Änderungen vor:

   * Rufen Sie `host.RunAsService` statt `host.Run` auf.

   * Wenn der Code `UseContentRoot` aufruft, verwenden Sie anstelle von `Directory.GetCurrentDirectory()` einen Pfad zum Ort der Veröffentlichung.

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,7,12)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

   ---

3. Veröffentlichen Sie die App in einem Ordner. Verwenden Sie [dotnet publish](/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio-Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) für die Veröffentlichung in einem Ordner.

4. Führen Sie einen Test durch, indem Sie den Dienst erstellen und starten.

   Öffnen Sie eine Befehlsshell mit Administratorrechten, um das Befehlszeilentool [sc.exe](https://technet.microsoft.com/library/bb490995) zum Erstellen und Starten eines Diensts zu verwenden. Wenn der Dienst den Namen „MyService“ trägt, in `c:\svc` veröffentlicht wird und den Namen „AspNetCoreService“ erhält, lauten die Befehle wie folgt:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

   Der Wert `binPath` ist der Pfad zu der ausführbaren Datei der App, der den Namen der ausführbaren Datei enthält.

   ![Beispiel zum Erstellen und Starten im Konsolenfenster](windows-service/_static/create-start.png)

   Navigieren Sie nach Abschluss dieser Befehle zu dem gleichen Pfad wie bei der Ausführung als Konsolen-App (standardmäßig `http://localhost:5000`):

   ![Ausführung in einem Dienst](windows-service/_static/running-in-service.png)

## <a name="provide-a-way-to-run-outside-of-a-service"></a>Bieten einer Möglichkeit zur Ausführung außerhalb eines Diensts

Das Testen und Debuggen ist bei der Ausführung außerhalb eines Diensts einfacher. Daher ist es üblich, dass Code hinzugefügt wird, der `RunAsService` nur unter bestimmten Bedingungen aufruft. Beispielsweise kann die App als Konsolen-App mit dem Befehlszeilenargument `--console` ausgeführt werden oder wenn der Debugger angefügt wird:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](windows-service/sample_snapshot/Program.cs?name=ServiceOrConsole)]

---

## <a name="handle-stopping-and-starting-events"></a>Behandeln des Stoppens und Startens von Ereignissen

Nehmen Sie die folgenden zusätzlichen Änderungen vor, um Ereignisse vom Typ `OnStarting`, `OnStarted` und `OnStopping` zu verarbeiten:

1. Erstellen Sie eine von `WebHostService` abgeleitete Klasse:

   [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

2. Erstellen Sie eine Erweiterungsmethode für `IWebHost`, die den benutzerdefinierten `WebHostService` an `ServiceBase.Run` übergibt:

   [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. Rufen Sie in `Program.Main` anstelle von `RunAsService` die neue Erweiterungsmethode `RunAsCustomService` auf:

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

   [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=24)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

   [!code-csharp[](windows-service/sample_snapshot/Program.cs?name=HandleStopStart&highlight=26)]

   ---

Wenn für den benutzerdefinierten `WebHostService`-Code ein Dienst aus der Abhängigkeitsinjektion erforderlich ist (z.B. eine Protokollierung), rufen Sie diese über die `Services`-Eigenschaft von `IWebHost` ab:

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Dienste, die mit Anforderungen aus dem Internet oder einem Unternehmensnetzwerk interagieren und hinter einem Proxy oder Lastenausgleich ausgeführt werden, erfordern möglicherweise zusätzliche Konfigurationen. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

## <a name="acknowledgments"></a>Danksagungen

Dieser Artikel wurde mithilfe von veröffentlichten Quellen geschrieben:

* [Hosten von ASP.NET Core als Windows-Dienst](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Hosten Ihres ASP.NET Core in einem Windows-Dienst](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
