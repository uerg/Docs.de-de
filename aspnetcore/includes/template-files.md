* Startup.cs: [Startklasse](../fundamentals/startup.md) -Klasse, konfiguriert die Anforderungspipeline, der alle Anforderungen an die Anwendung behandelt.
* Datei "Program.cs": [Programmklasse](../fundamentals/index.md) , enthält die Main-Einstiegspunkt der Anwendung.
* firstapp.csproj: [Projektdatei](https://docs.microsoft.com/dotnet/articles/core/preview3/tools/csproj) MSBuild-Projektdateiformat für ASP.NET Core-Anwendungen. Enthält Verweise zwischen Projekten, NuGet-Verweise und das andere Projekt verknüpfte Elemente.
* AppSettings.JSON / "appSettings". Development.JSON: Umgebung Basis app-Einstellungen der Konfigurationsdatei. [Finden Sie unter Konfiguration](xref:fundamentals/configuration).
* "bower.JSON": Bower paketabhängigkeiten für das Projekt.
* ".bowerrc": Bower-Konfigurationsdatei, die definiert, wo Sie die Komponenten zu installieren, wenn Bower die Ressourcen heruntergeladen.
* bundleconfig.JSON: Konfigurationsdateien für bundling und Minimierung mit Front-End-JavaScript und CSS-Objekte.
* Ansichten: Enthält die Razor-Ansichten. Ansichten sind die Komponenten, die die app-Benutzeroberfläche (UI) anzeigen. Im Allgemeinen wird dieser Benutzeroberfläche die Modelldaten angezeigt.
* Domänencontroller: Anfangs enthält die MVC-Controller *HomeController.cs*. Controller sind Klassen, die Browseranforderungen zu verarbeiten.
* "Wwwroot": Stammordner der Web-Anwendung.

Weitere Informationen finden Sie unter [der MVC-Musters](xref:mvc/overview).
