<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="7517d-101">Erstellen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="7517d-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="7517d-102">Öffnen Sie ein Befehlsfenster im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs*, und *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="7517d-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7517d-103">Führen Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="7517d-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```