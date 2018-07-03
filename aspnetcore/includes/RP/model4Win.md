<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="8cb7b-101">Aufbauen des Filmmodells</span><span class="sxs-lookup"><span data-stu-id="8cb7b-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="8cb7b-102">Führen Sie über die Befehlszeile im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien) folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="8cb7b-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

<span data-ttu-id="8cb7b-103">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="8cb7b-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="8cb7b-104">Der zuvor beschriebene Fehler tritt auf, wenn Sie sich im falschen Verzeichnis befinden.</span><span class="sxs-lookup"><span data-stu-id="8cb7b-104">The preceeding error happens when you are in the wrong directory.</span></span> <span data-ttu-id="8cb7b-105">Öffnen Sie eine Befehlsshell im Projektverzeichnis (das Verzeichnis mit den Dateien *Program.cs*, *Startup.cs* und *CSPROJ*-Dateien), und führen Sie den zuvor aufgeführten Befehl aus.</span><span class="sxs-lookup"><span data-stu-id="8cb7b-105">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files), and then run the preceeding command.</span></span>

<span data-ttu-id="8cb7b-106">Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:</span><span class="sxs-lookup"><span data-stu-id="8cb7b-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="8cb7b-107">Beenden Sie Visual Studio, und führen Sie den Befehl erneut aus.</span><span class="sxs-lookup"><span data-stu-id="8cb7b-107">Exit Visual Studio and run the command again.</span></span>
