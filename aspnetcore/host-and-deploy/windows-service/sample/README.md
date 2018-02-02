# <a name="custom-webhost-service-sample"></a><span data-ttu-id="1bb9a-101">Benutzerdefinierte WebHost-Beispiel</span><span class="sxs-lookup"><span data-stu-id="1bb9a-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="1bb9a-102">Dieses Beispiel zeigt die empfohlene Methode, um eine ASP.NET Core-app unter Windows zu hosten, ohne mit IIS als Windows-Dienst.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="1bb9a-103">In diesem Beispiel wird veranschaulicht, die in beschriebenen Funktionen [hosten Sie eine ASP.NET Core-app in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="1bb9a-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="1bb9a-104">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="1bb9a-104">Instructions</span></span>

<span data-ttu-id="1bb9a-105">Die Beispiel-app ist eine einfache MVC-Web-app geändert wird, gemäß der Anleitung in [hosten Sie eine ASP.NET Core-app in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="1bb9a-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="1bb9a-106">Um die app in einem Dienst auszuführen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="1bb9a-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="1bb9a-107">Erstellen Sie einen Ordner am *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="1bb9a-108">Veröffentlichen Sie die app zum Ordner mit der `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="1bb9a-109">Der Befehl wird der App wechseln Sie zum Ordner, einschließlich der erforderlichen `appsettings.json` Datei und die `wwwroot` Ordner samt Inhalt.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="1bb9a-110">Öffnen einer **Administrator** -Befehlsshell.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="1bb9a-111">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1bb9a-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="1bb9a-112">Navigieren Sie in einem Browser auf `http://localhost:5000` um sicherzustellen, dass der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="1bb9a-113">Um den Dienst zu beenden, verwenden Sie den Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="1bb9a-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="1bb9a-114">Wenn die app nicht wie erwartet, bei der Ausführung in einem Dienst gestartet, ist eine schnelle Möglichkeit, Fehlermeldungen zugänglich zu machen, wie z. B. hinzufügen ein Anbieters für die Protokollierung der [Anbieter Windows-Ereignisprotokoll](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="1bb9a-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="1bb9a-115">Eine weitere Option besteht, überprüfen Sie das Anwendungsereignisprotokoll, die mithilfe der Ereignisanzeige auf dem System.</span><span class="sxs-lookup"><span data-stu-id="1bb9a-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="1bb9a-116">Hier ist z. B. eine nicht behandelte Ausnahme auf einen FileNotFound Fehler im Anwendungsereignisprotokoll:</span><span class="sxs-lookup"><span data-stu-id="1bb9a-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
