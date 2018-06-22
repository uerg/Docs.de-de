---
title: ASP.NET Core SignalR .NET Client
author: rachelappel
description: Informationen zu ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273294"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET Client

Von [Rachel Appel](http://twitter.com/rachelappel)

Der ASP.NET Core SignalR .NET Client kann von Xamarin, WPF, Windows Forms, Konsole und .NET Core-apps verwendet werden. Wie die [JavaScript-Client](xref:signalr/javascript-client), der .NET Client können Sie empfangen und senden und Empfangen von Nachrichten mit einem Hub in Echtzeit.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.

## <a name="install-the-signalr-net-client-package"></a>Installieren Sie das Clientpaket SignalR .NET

Die `Microsoft.AspNetCore.SignalR.Client` Paket ist für .NET-Clients zur Verbindung mit SignalR-Hubs erforderlich. Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **Package Manager Console** Fenster:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Herstellen einer Verbindung mit einem hub

Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`. Der Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden. Konfigurieren Sie alle erforderlichen Optionen durch Einfügen die `HubConnectionBuilder` Methoden in `Build`. Starten Sie die Verbindung mit `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>Aufruf von hubmethoden vom client

`InvokeAsync` Ruft Methoden für den Hub. Übergeben Sie den Hubnamen-Methode und Argumente definiert, in der hubmethode, um `InvokeAsync`. SignalR asynchron ist, verwenden Sie daher `async` und `await` beim Aufrufen.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>Aufrufen von Methoden vom Hub für client

Definieren von Methoden, die der Hub Aufrufe mit `connection.On` erst nach der Erstellung, jedoch vor dem Starten der Verbindung.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

Der vorangehende Code in `connection.On` ausgeführt wird, wenn serverseitige Code aufgerufen wird, mithilfe der `SendAsync` Methode.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und-Protokollierung

Behandeln von Fehlern mit einer Try-Catch-Anweisung. Überprüfen Sie die `Exception` Objekts bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hubs](xref:signalr/hubs)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)