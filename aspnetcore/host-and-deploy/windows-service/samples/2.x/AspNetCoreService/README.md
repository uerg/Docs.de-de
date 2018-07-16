# <a name="custom-webhost-service-sample"></a><span data-ttu-id="c9969-101">Beispiel: Benutzerdefinierter WebHost-Dienst</span><span class="sxs-lookup"><span data-stu-id="c9969-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="c9969-102">In diesem Beispiel wird die Vorgehensweise zum Hosten einer ASP.NET Core-App als Windows-Dienst ohne Verwendung von IIS dargestellt.</span><span class="sxs-lookup"><span data-stu-id="c9969-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="c9969-103">In diesem Beispiel wird das unter [Hosten einer ASP.NET Core-App in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service) beschriebene Szenario veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="c9969-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="c9969-104">Anweisungen</span><span class="sxs-lookup"><span data-stu-id="c9969-104">Instructions</span></span>

<span data-ttu-id="c9969-105">Bei der Beispiel-App handelt es sich gemäß den Anweisungen unter [Hosten einer ASP.NET Core-App in einem Windows-Dienst](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service) um eine geänderte Razor Pages-Web-App.</span><span class="sxs-lookup"><span data-stu-id="c9969-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="c9969-106">Führen Sie die folgenden Schritte aus, um die App in einem Dienst auszuführen:</span><span class="sxs-lookup"><span data-stu-id="c9969-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="c9969-107">Erstellen Sie unter *c:\svc* einen Ordner.</span><span class="sxs-lookup"><span data-stu-id="c9969-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="c9969-108">Veröffentlichen Sie die App in dem Ordner mit `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="c9969-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="c9969-109">Die Objekte der App werden durch den Befehl in den Ordner *svc* verschoben, einschließlich der erforderlichen `appsettings.json`-Datei und des `wwwroot`-Ordners.</span><span class="sxs-lookup"><span data-stu-id="c9969-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="c9969-110">Öffnen Sie eine **Administrator**-Eingabeaufforderung.</span><span class="sxs-lookup"><span data-stu-id="c9969-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="c9969-111">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="c9969-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="c9969-112">*Der Abstand zwischen dem Gleichheitszeichen und dem Beginn der Pfadzeichenfolge ist erforderlich.*</span><span class="sxs-lookup"><span data-stu-id="c9969-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="c9969-113">Navigieren Sie in einem Browser zu `http://localhost:5000`, und überprüfen Sie, ob der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c9969-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="c9969-114">Die App wird zum sicheren Endpunkt `https://localhost:5001` umgeleitet.</span><span class="sxs-lookup"><span data-stu-id="c9969-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="c9969-115">Verwenden Sie zum Beenden des Diensts den folgenden Befehl:</span><span class="sxs-lookup"><span data-stu-id="c9969-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="c9969-116">Wenn die App nicht wie erwartet gestartet wird, können Fehlermeldungen auf schnelle Weise zugänglich gemacht werden, indem ein Protokollierungsanbieter, z.B. der [Windows EventLog-Anbieter](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog), hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="c9969-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="c9969-117">Eine weitere Möglichkeit besteht darin, das Anwendungsereignisprotokoll mit der Ereignisanzeige im System zu überprüfen.</span><span class="sxs-lookup"><span data-stu-id="c9969-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="c9969-118">Im Folgenden wird beispielsweise eine unbehandelte Ausnahme für einen Fehler vom Typ „FileNotFound“ im Anwendungsereignisprotokoll aufgeführt:</span><span class="sxs-lookup"><span data-stu-id="c9969-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
