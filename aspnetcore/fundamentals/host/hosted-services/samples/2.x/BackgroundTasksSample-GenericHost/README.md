# <a name="aspnet-core-background-tasks-sample-generic-host"></a>Beispiel für ASP.NET Core-Hintergrundtasks (Generischer Host)

Dieses Beispiel veranschaulicht die Verwendung von [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice). Dieses Beispiel veranschaulicht die Features, die im Artikel [Background tasks with hosted services in ASP.NET Core (Hintergrundtasks bei gehosteten Diensten in ASP.NET Core)](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) beschrieben werden.

Beim Ausführen des Beispiels in [Visual Studio Code](https://code.visualstudio.com/) legen Sie den Wert **console** der Konsolenkonfiguration in *.vscode/launch.json* entweder auf `externalTerminal` oder auf `integratedTerminal` fest. Die Verwendung der `internalConsole` ist nicht kompatibel mit der Konsolentastatureingabe, die von der App für das Einreihen von Hintergrundarbeitselementen in die Warteschlange verwendet wird.
