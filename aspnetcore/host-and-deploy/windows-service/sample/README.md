# <a name="custom-webhost-service-sample"></a>Beispiel: Benutzerdefinierter WebHost-Dienst

In diesem Beispiel wird die Vorgehensweise zum Hosten einer ASP.NET Core-App als Windows-Dienst ohne Verwendung von IIS dargestellt. In diesem Beispiel wird das unter [Hosten einer ASP.NET Core-App in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service) beschriebene Szenario veranschaulicht.

## <a name="instructions"></a>Anweisungen

Bei der Beispiel-App handelt es sich gemäß den Anweisungen unter [Hosten einer ASP.NET Core-App in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service) um eine geänderte Razor Pages-Web-App.

Führen Sie die folgenden Schritte aus, um die App in einem Dienst auszuführen:

1. Erstellen Sie unter *c:\svc* einen Ordner.

1. Veröffentlichen Sie die App in dem Ordner mit `dotnet publish --configuration Release --output c:\\svc`. Die Objekte der App werden durch den Befehl in den Ordner *svc* verschoben, einschließlich der erforderlichen `appsettings.json`-Datei und des `wwwroot`-Ordners.

1. Öffnen Sie eine **Administrator**-Eingabeaufforderung.

1. Führen Sie den folgenden Befehl aus:

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  *Der Abstand zwischen dem Gleichheitszeichen und dem Beginn der Pfadzeichenfolge ist erforderlich.*

1. Navigieren Sie in einem Browser zu `http://localhost:5000`, und überprüfen Sie, ob der Dienst ausgeführt wird. Die App wird zum sicheren Endpunkt `https://localhost:5001` umgeleitet.

1. Verwenden Sie zum Beenden des Diensts den folgenden Befehl:

   ```console
   sc stop MyService
   ```

Wenn die App nicht wie erwartet gestartet wird, können Fehlermeldungen auf schnelle Weise zugänglich gemacht werden, indem ein Protokollierungsanbieter, z.B. der [Windows EventLog-Anbieter](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog), hinzugefügt wird. Eine weitere Möglichkeit besteht darin, das Anwendungsereignisprotokoll mit der Ereignisanzeige im System zu überprüfen. Im Folgenden wird beispielsweise eine unbehandelte Ausnahme für einen Fehler vom Typ „FileNotFound“ im Anwendungsereignisprotokoll aufgeführt:

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
