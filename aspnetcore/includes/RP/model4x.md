<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="5b6fa-101">Erstellen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="5b6fa-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="5b6fa-102">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="5b6fa-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="5b6fa-103">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5b6fa-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="5b6fa-104">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="5b6fa-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="5b6fa-105">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="5b6fa-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>