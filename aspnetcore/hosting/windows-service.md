---
title: Host in einem Windowsdienst
author: tdykstra
description: Erfahren Sie, wie eine ASP.NET Core-Anwendung in einem Windows-Dienst zu hosten.
keywords: Hosten von ASP.NET Core, Windows-Dienst
ms.author: tdykstra
manager: wpickett
ms.date: 03/30/2017
ms.topic: article
ms.assetid: d9a65066-d7cb-47df-b046-64629c4d2c6f
ms.technology: aspnet
ms.prod: aspnet-core
uid: hosting/windows-service
ms.openlocfilehash: ca3b98f0b0405fcd5751cb7d9bc7a40257739084
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2017
---
# <a name="host-an-aspnet-core-app-in-a-windows-service"></a>Hosten Sie eine ASP.NET Core-app in einem Windowsdienst

Durch [Tom Dykstra](https://github.com/tdykstra)

Die empfohlene Methode zum Hosten einer ASP.NET Core-app unter Windows bei Verwendung von IIS nicht ist, führen Sie es in einem [Windowsdienst](https://docs.microsoft.com/dotnet/framework/windows-services/introduction-to-windows-service-applications). Auf diese Weise können sie automatisch nach Neustarts und Abstürzen, starten, ohne sich im Wartezustand.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) ([zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample)). Finden Sie unter der [Arbeitsschritte](#next-steps) Abschnitt Anleitungen für die Ausführung.

## <a name="prerequisites"></a>Erforderliche Komponenten

* In der .NET Framework-Laufzeit muss die app auszuführen.  In der *csproj* Datei, geben Sie entsprechende Werte für [TargetFramework](https://docs.microsoft.com/nuget/schema/target-frameworks) und [RuntimeIdentifier](https://docs.microsoft.com/dotnet/articles/core/rid-catalog). Im Folgenden ein Beispiel:

  [!code-xml[](windows-service/sample/AspNetCoreService.csproj?range=3-6)]

  Verwenden Sie beim Erstellen eines Projekts in Visual Studio die **Core ASP.NET-Anwendung ((.NET Framework)** Vorlage.

* Wenn die app aus dem Internet (nicht nur über ein internes Netzwerk) die Anforderungen erhalten werden, verwenden sie die [WebListener](xref:fundamentals/servers/weblistener) Webserver statt [Kestrel](xref:fundamentals/servers/kestrel).  Kestrel muss mit IIS für Edge-Bereitstellungen verwendet werden.  Weitere Informationen finden Sie unter [When to use Kestrel with a reverse proxy (Verwenden von Kestrel mit einem Reverseproxy)](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).

## <a name="getting-started"></a>Erste Schritte

In diesem Abschnitt wird erläutert, die minimale Änderungen erforderlich, um ein vorhandenes Projekt für ASP.NET Core einrichten, um in einem Dienst ausgeführt wird.

* Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices/).

* Nehmen Sie die folgenden Änderungen in `Program.Main`:
  
  * Rufen Sie `host.RunAsService` anstelle von `host.Run`.
  
  * Wenn Sie Ihren Code aufruft `UseContentRoot`, verwenden Sie einen Pfad an den Veröffentlichungsort anstelle von`Directory.GetCurrentDirectory()` 
  
  [!code-csharp[](windows-service/sample/Program.cs?name=ServiceOnly&highlight=3-4,8,14)]

* Veröffentlichen Sie die Anwendung in einen Ordner.

  Verwendung [Dotnet veröffentlichen](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) oder ein [Visual Studio das Veröffentlichungsprofil](xref:publishing/web-publishing-vs) , die in einen Ordner veröffentlicht.

* Testen von erstellen und den Dienst zu starten.

  Öffnen Sie eine Administrator-Eingabeaufforderung mit dem [sc.exe](https://technet.microsoft.com/library/bb490995) Befehlszeilentool zum Erstellen und Starten eines Diensts.  
  
  Wenn Sie den Dienst MyService benennen, Veröffentlichen Ihrer app zu `c:\svc`, und die app selbst heißt AspNetCoreService, würde die Befehle wie folgt aussehen:

  ```console
  sc create MyService binPath="C:\Svc\AspNetCoreService.exe"
  sc start MyService
  ```
  Die `binPath` Wert ist der Pfad zu Ihrer app ausführbare Datei, einschließlich der ausführbaren Datei selbst.

  ![Konsolenfenster erstellen und starten Beispiel](windows-service/_static/create-start.png)

  Wenn diese Befehle abgeschlossen haben, können Sie navigieren Sie zu dem gleichen Pfad wie beim als eine Konsolen-app ausführen (standardmäßig `http://localhost:5000`)

  ![In einem Dienst ausgeführt wird](windows-service/_static/running-in-service.png)


## <a name="provide-a-way-to-run-outside-of-a-service"></a>Bieten Sie eine Möglichkeit, die außerhalb eines Diensts ausführen

Es ist einfacher, testen und Debuggen, wenn Sie außerhalb von einem Dienst ausführen, daher ist es üblich, So fügen Sie Code hinzu, die aufruft `host.RunAsService` nur unter bestimmten Bedingungen.  Beispielsweise konnte Ausführen als eine Konsolen-app, wenn Sie erhalten eine `--console` Befehlszeilenargument oder wenn der Debugger angefügt ist.

[!code-csharp[](windows-service/sample/Program.cs?name=ServiceOrConsole)]

## <a name="handle-stopping-and-starting-events"></a>Behandeln von Ereignissen starten und beenden

Wenn Sie behandeln möchten `OnStarting`, `OnStarted`, und `OnStopping` Ereignisse, die folgenden zusätzliche Änderungen vornehmen:

* Erstellen Sie eine von der `WebHostService`-Klasse abgeleitete Klasse.

  [!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=NoLogging)]

* Erstellen Sie eine Erweiterungsmethode für `IWebHost` , die Ihre benutzerdefinierte übergibt `WebHostService` auf `ServiceBase.Run`.

  [!code-csharp[](windows-service/sample/WebHostServiceExtensions.cs?name=ExtensionsClass)]

* In `Program.Main` Änderung rufen Sie die neue Erweiterungsmethode anstelle von `host.RunAsService`.

  [!code-csharp[](windows-service/sample/Program.cs?name=HandleStopStart&highlight=26)]

Wenn Ihre benutzerdefinierte `WebHostService` Code muss einen Dienst von abhängigkeiteneinschleusung (z. B. eine Protokollierung) abrufen, können Sie es aus der `Services` Eigenschaft `IWebHost`.

[!code-csharp[](windows-service/sample/CustomWebHostService.cs?name=Logging&highlight=7)]

## <a name="next-steps"></a>Nächste Schritte

Die [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/hosting/windows-service/sample) , die begleitet dieser Artikel ist eine einfache MVC-Web-app, die wie gezeigt in vorherigen Codebeispiele geändert wurde.  Um es in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:

* Veröffentlichen in *c:\svc*.

* Öffnen Sie eine Administratorfenster.

* Geben Sie die folgenden Befehle aus:

  ```console
  sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
  sc start MyService
  ```

  * Wechseln Sie in einem Browser zu http://localhost: 5000, um sicherzustellen, dass er ausgeführt wird.

Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich sind zum Hinzufügen eines Anbieters für die Protokollierung wie dem [Anbieter Windows-Ereignisprotokoll](xref:fundamentals/logging#eventlog).

## <a name="acknowledgments"></a>Bestätigungen

In diesem Artikel wurde mit Hilfe von Quellen geschrieben, die bereits veröffentlicht wurden. Das früheste und besonders hilfreich, diese wurden diese:

* [Hosten von ASP.NET Core als Windows-Dienst](https://stackoverflow.com/questions/37346383/hosting-asp-net-core-as-windows-service/37464074)
* [Wie Ihre ASP.NET Core in einem Windows-Dienst gehostet wird.](https://dotnetthoughts.net/how-to-host-your-aspnet-core-in-a-windows-service/)
