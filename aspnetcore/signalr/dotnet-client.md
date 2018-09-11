---
title: ASP.NET Core SignalR .NET Client
author: tdykstra
description: Informationen zum ASP.NET Core SignalR .NET Client
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373318"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET Client

Von [Rachel Appel](http://twitter.com/rachelappel)

Der ASP.NET Core SignalR .NET Client Library können Sie die Kommunikation mit SignalR-Hubs in .NET apps.

> [!NOTE]
> Xamarin stellt besondere Anforderungen an die Visual Studio-Version. Weitere Informationen finden Sie unter [SignalR-Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

Das Codebeispiel in diesem Artikel ist eine WPF-app, die ASP.NET Core SignalR .NET Client verwendet.

## <a name="install-the-signalr-net-client-package"></a>Installieren Sie das SignalR .NET Client-Paket

Die `Microsoft.AspNetCore.SignalR.Client` für .NET-Clients zur Verbindung mit SignalR-Hubs ist ein Paket erforderlich. Um die Clientbibliothek zu installieren, führen Sie den folgenden Befehl der **-Paket-Manager-Konsole** Fenster:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Verbinden mit einem hub

Um eine Verbindung herzustellen, erstellen Sie eine `HubConnectionBuilder` , und rufen Sie `Build`. Die Hub-URL, Protokoll, Transporttyp, Protokollebene, Header und anderen Optionen können beim Erstellen einer Verbindungs konfiguriert werden. Konfigurieren Sie alle erforderlichen Optionen durch Einfügen eines der `HubConnectionBuilder` Methoden `Build`. Starten Sie die Verbindung mit `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>Aufrufen von Hub-Methoden von client

`InvokeAsync` Ruft Methoden für den Hub. Übergeben Sie den Namen des Hubs-Methode und alle Argumente, die in die hubmethode, die definiert `InvokeAsync`. SignalR ist asynchron, verwenden Sie daher `async` und `await` bei den aufrufen.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>Rufen Sie Client-Methoden von Hub-Instanz

Definieren Sie Methoden, die der Hub Aufrufe mithilfe von `connection.On` nach der Erstellung, jedoch vor dem Starten der Verbindung.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Der vorangehende Code in `connection.On` ausgeführt wird, wenn Sie serverseitigen Code aufruft, mit der `SendAsync` Methode.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Fehlerbehandlung und Protokollierung

Behandeln von Fehlern mit einem Try / Catch-Anweisung. Überprüfen Sie die `Exception` Objekt, um zu bestimmen, die entsprechende Aktion aus, um aufzuzeichnen, wenn ein Fehler auftritt.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Hubs](xref:signalr/hubs)
* [JavaScript-Client](xref:signalr/javascript-client)
* [Veröffentlichen in Azure](xref:signalr/publish-to-azure-web-app)
