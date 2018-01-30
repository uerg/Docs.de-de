---
title: Host in einem Windowsdienst
author: tdykstra
description: Erfahren Sie, wie eine ASP.NET Core-Anwendung in einem Windows-Dienst zu hosten.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/30/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/windows-service
ms.openlocfilehash: beda34dbd613f6ffe0afa207ab57dd6ebbc489ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Hosten Sie eine ASP.NET Core-app in einem Windowsdienst

Durch [Tom Dykstra](https://github.com/tdykstra)

Die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS ist die Ausführung in einem [Windowsdienst](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Auf diese Weise können sie automatisch nach Neustarts und Abstürzen, starten, ohne sich im Wartezustand.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)). Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt Anleitungen für die Ausführung.

## <a name="prerequisites"></a>Erforderliche Komponenten

* In der .NET Framework-Laufzeit muss die app auszuführen.  In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) und [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Im Folgenden ein Beispiel:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.

* Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) Anforderungen empfängt, verwenden sie die [WebListener](xref:fundamentals/servers/weblistener) Webserver statt [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel muss mit IIS für Edge-Bereitstellungen verwendet werden.  Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Erste Schritte

In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.

* Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Nehmen Sie die folgenden Änderungen in `Program.Main`:
  
  * Rufen Sie `host.RunAsService` anstelle von `host.Run`.
  
  * Wenn der Code ruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Veröffentlichen Sie die Anwendung in einen Ordner.

  Verwendung [Dotnet veröffentlichen](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:host-and-deploy/visual-studio-publish-profiles) , die in einen Ordner veröffentlicht.

* Testen von erstellen und den Dienst zu starten.

  Öffnen Sie eine Administrator-Eingabeaufforderung mit dem [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.  
  
  Wenn der Dienst "MyService" benannt wird, veröffentlichen Sie die app `c:\svc`, und die app selbst heißt AspNetCoreService, würde die Befehle wie folgt aussehen:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```

  Die `binPath` Wert ist der Pfad zur ausführbaren Datei der der app, einschließlich der ausführbaren Datei selbst.

  ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

  Wenn diese Befehle abgeschlossen haben, navigieren Sie zu dem gleichen Pfad wie bei Ausführung als eine Konsolen-app (standardmäßig `http://localhost:5000`)

  ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen

Es ist einfacher, testen und Debuggen außerhalb von einem Dienst ausgeführt wird, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `host.RunAsService` nur unter bestimmten Bedingungen.  Beispielsweise kann die app auszuführen, als eine Konsolen-app mit einer `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Behandeln von Ereignissen starten und beenden

Behandeln `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:

* Erstellen Sie eine von der `WebHostService`-Klasse abgeleitete Klasse.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die die benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* In `Program.Main` Änderung rufen Sie die neue Erweiterungsmethode anstelle von `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Wenn die benutzerdefinierte `WebHostService` auszugehen zum Abrufen eines Diensts vom abhängigkeiteneinschleusung (z. B. eine Protokollierung), können Sie es aus der `Services` Eigenschaft `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Nächste Schritte

Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/sample) , die begleitet dieser Artikel ist eine einfache MVC-Web-app, die wie gezeigt in vorherigen Codebeispiele geändert wurde.  Um es in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:

* Veröffentlichen in *c:\svc*.

* Öffnen Sie eine Administratorfenster.

* Geben Sie die folgenden Befehle aus:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * Wechseln Sie in einem Browser zu http://localhost: 5000, um sicherzustellen, dass er ausgeführt wird.

Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich sind zum Hinzufügen eines Anbieters für die Protokollierung wie dem [Anbieter Windows-Ereignisprotokoll](xref:fundamentals/logging/index#eventlog).

## <a name="acknowledgments"></a>Bestätigungen

In diesem Artikel wurde mit Hilfe von Quellen geschrieben, die bereits veröffentlicht wurden. Das früheste und besonders hilfreich, diese wurden diese:

* [Hosten von ASP.NET Core als Windows-Dienst](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
