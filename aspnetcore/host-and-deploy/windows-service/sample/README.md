# <a name="custom-webhost-service-sample"></a>Benutzerdefinierte WebHost-Beispiel

Dieses Beispiel zeigt die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS als Windows-Dienst. In diesem Beispiel wird veranschaulicht, die in beschriebenen Funktionen [hosten Sie eine ASP.NET Core-app in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Anweisungen

Die Beispiel-app ist eine einfache MVC-Web-app geändert wird, gemäß der Anleitung in [hosten Sie eine ASP.NET Core-app in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Um die app in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:

1. Erstellen Sie einen Ordner am *c:\svc*.

1. Veröffentlichen Sie die app zum Ordner mit der `dotnet publish --configuration Release --output c:\\svc`. Der Befehl wird der App wechseln Sie zum Ordner, einschließlich der erforderlichen `appsettings.json` Datei und die `wwwroot` Ordner samt Inhalt.

1. Öffnen einer **Administrator** -Befehlsshell.

1. Führen Sie den folgenden Befehl aus:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. Navigieren Sie in einem Browser auf `http://localhost:5000` um sicherzustellen, dass der Dienst ausgeführt wird.

1. Um den Dienst zu beenden, verwenden Sie den Befehl aus:

   ```console
   sc stop MyService
   ```

Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich zu machen, wie z. B. hinzufügen ein Anbieters für die Protokollierung der [Anbieter Windows-Ereignisprotokoll](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Eine weitere Option besteht, überprüfen Sie das Anwendungsereignisprotokoll, die mithilfe der Ereignisanzeige auf dem System. Hier ist z. B. eine nicht behandelte Ausnahme auf einen FileNotFound Fehler im Anwendungsereignisprotokoll:

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
