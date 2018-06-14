Führen Sie die Identität Scaffolder:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Von **Projektmappen-Explorer**, mit der rechten Maustaste auf das Projekt > **hinzufügen** > **neues Gerüstelement**.
* Im linken Bereich des der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Identität** > **hinzufügen**.
* In der **ADD Identität** Dialogfeld Wählen Sie die gewünschten Optionen.
  * Wählen Ihre vorhandene Seitenlayout, oder durch falsche Markup Layout-Datei überschrieben werden. Wenn eine vorhandene Datei "_Layout.cshtml" ausgewählt ist, ist es **nicht** überschrieben.

 Z. B. `~/Pages/Shared/_Layout.cshtml` für Razor-Seiten `~/Views/Shared/_Layout.cshtml` für MVC-Projekte
* Um Ihre vorhandenen Datenkontext zu verwenden, wählen Sie mindestens eine Datei zu überschreiben. Sie müssen mindestens eine Datei auf Ihrem Datenkontext hinzufügen auswählen.
  * Wählen Sie Ihre Daten Context-Klasse.
  * Wählen Sie **hinzufügen**.
* Erstellt einen neuen Benutzerkontext aus, und erstellen eine benutzerdefinierte Klasse möglicherweise für Identität:
  * Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Datenkontextklasse**.
  * Wählen Sie **hinzufügen**.

Hinweis: Wenn Sie einen neuen Benutzerkontext erstellen, müssen Sie eine Datei außer Kraft setzen möchten.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Wenn Sie die ASP.NET Scaffolder noch nicht installiert haben, installieren Sie es jetzt ein:

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

Hinzufügen eines Verweises Paket auf [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) zum Projekt (\*csproj) Datei. Führen Sie den folgenden Befehl in das Projektverzeichnis:

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

Führen Sie den folgenden Befehl zum Auflisten von Optionen Scaffolder Identität:

```cli
dotnet aspnet-codegenerator identity -h
```

Führen Sie die Identität Scaffolder in den Projektordner mit den gewünschten Optionen. Angenommen, um die Identität mit der Standardbenutzeroberfläche und die minimale Anzahl von Dateien einrichten möchten, führen Sie den folgenden Befehl an. Verwenden Sie den richtigen vollqualifizierten Namen für den DB-Kontext:

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
