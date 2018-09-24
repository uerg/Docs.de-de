---
title: Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Sie Klassen für das Verwalten von Filmen mithilfe von Entity Framework Core (EF Core) zu einer Datenbank hinzufügen.
ms.author: riande
ms.date: 05/30/2018
uid: tutorials/razor-pages/model
ms.openlocfilehash: de82738509bb009f030a02e28904e3155088fa6a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011357"
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a>Hinzufügen eines Modells zu einer App mit Razor-Seiten in ASP.NET Core

::: moniker range=">= aspnetcore-2.1"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Hinzufügen eines Datenmodells

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*. Wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:

Ersetzen Sie den Inhalt der Klasse `Movie` durch folgenden Code:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie21/Models/Movie1.cs?name=snippet)]

## <a name="scaffold-the-movie-model"></a>Erstellen des Gerüsts für das Filmmodell

In diesem Abschnitt wird das Gerüst für das Filmmodell erstellt. Mit dem Tool für den Gerüstbau werden Seiten für die Vorgänge „Create“ (Erstellen), „Read“ (Lesen), „Update“ (Aktualisieren) und „Delete“ (Löschen), kurz CRUD-Vorgänge, für das Filmmodell erstellt.

Erstellen Sie den Ordner *Pages/Movies*:

* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Pages* > **Hinzufügen** > **Neuer Ordner**.
* Geben Sie dem Ordner den Namen *Movies*

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Pages/Movies* > **Hinzufügen** > **Neues Gerüstelement**.

![Abbildung der vorherigen Anweisungen.](model/_static/sca.png)

Wählen Sie im Dialogfeld **Gerüst hinzufügen** den Eintrag **Razor Pages mit Entity Framework (CRUD)** > **HINZUFÜGEN** aus.

![Abbildung der vorherigen Anweisungen.](model/_static/add_scaffold.png)

Vervollständigen Sie das Dialogfeld **Razor Pages mit Entity Framework (CRUD) hinzufügen**:

* Wählen Sie in der Dropdownliste **Modellklasse** den Eintrag **Film (RazorPagesMovie.Models)** aus.
* Wählen Sie in der Zeile **Datenkontextklasse** das Zeichen **+** (Plus) aus, und akzeptieren Sie den generierten Namen **RazorPagesMovie.Models.RazorPagesMovieContext**.
* Wählen Sie in der Dropdownliste **Datenkontextklasse** den Eintrag **RazorPagesMovie.Models.RazorPagesMovieContext** aus.
* Wählen Sie **Hinzufügen** aus.

![Abbildung der vorherigen Anweisungen.](model/_static/arp.png)

Der Gerüstprozess hat folgende Dateien erstellt und geändert:

### <a name="files-created"></a>Erstellte Dateien

* *Pages/Movies/Create.cshtml.cs* ( bzw. /Delete, /Details, /Edit, /Index). Diese Seiten werden im nächsten Tutorial ausführlich erläutert.
* *Data/RazorPagesMovieContext.cs*

### <a name="files-updates"></a>Aktualisierte Dateien

* *Startup.cs:* Die Änderungen an dieser Datei werden im nächsten Abschnitt ausführlich erläutert.
* *appsettings.json*: Die Verbindungszeichenfolge, die zum Herstellen einer Verbindung mit einer lokalen Datenbank verwendet wird, wurde hinzugefügt.

## <a name="examine-the-context-registered-with-dependency-injection"></a>Überprüfen des mit Dependency Injection registrierten Kontexts

ASP.NET Core wird mit [Dependency Injection](xref:fundamentals/dependency-injection) erstellt. Dienste (z.B. der Datenbankkontext Entity Framework Core) werden über Dependency Injection beim Anwendungsstart registriert. Komponenten, die diese Dienste erfordern (z.B. Razor Pages), werden von diesen Diensten über Konstruktorparameter bereitgestellt. Der Konstruktorcode, der eine Datenbankkontext-Instanz abruft, wird später in diesem Tutorial erläutert.

Das Gerüstbautool hat automatisch einen Datenbankkontext erstellt und diesen mit dem Dependency Injection-Container registriert.

Untersuchen Sie die Methode `Startup.ConfigureServices`. Die hervorgehobene Zeile wurde vom Gerüst hinzugefügt:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Startup.cs?name=snippet_ConfigureServices&highlight=12-13)]

Bei der Datenbankkontextklasse handelt es sich um die Hauptklasse, die die Entity Framework Core-Funktionen für ein angegebenes Datenmodell koordiniert. Der Datenkontext wird von [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) abgeleitet. Der Datenkontext gibt an, welche Entitäten im Datenmodell enthalten sind. In diesem Projekt heißt die Klasse `RazorPagesMovieContext`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie21/Data/RazorPagesMovieContext.cs)]

Der vorangehende Code erstellt eine [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1)-Eigenschaft für die Entitätenmenge. In der Terminologie von Entity Framework entspricht eine Entitätenmenge in der Regel einer Datenbanktabelle. Entitäten entsprechen Zeilen in Tabellen.

Der Name der Verbindungszeichenfolge wird an den Kontext übergeben, indem Sie eine Methode auf einem [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions)-Objekt aufrufen. Für die lokale Entwicklung liest das [ASP.NET Core-Konfigurationssystem](xref:fundamentals/configuration/index) die Verbindungszeichenfolge aus der *appsettings.json*-Datei.

<a name="pmc"></a>
## <a name="perform-initial-migration"></a>Ausführen der anfänglichen Migration

In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:

* Fügen Sie eine anfängliche Migration hinzu.
* Aktualisieren Sie die Datenbank mit der anfänglichen Migration.

Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

Geben Sie in der PMC die folgenden Befehle ein:

```PMC
Add-Migration Initial
Update-Database
```

Alternativ können die folgenden .NET Core-CLI-Befehle vom Projektordner aus verwendet werden:

```console
dotnet ef migrations add Initial
dotnet ef database update
```

Die folgende Warnmeldung können Sie ignorieren, Sie werden dies in einem späteren Tutorial beheben:

`Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'.*

Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen. Das Schema basiert auf dem Modell, das in der Datei *Data/RazorPagesMovieContext.cs* für `RazorPagesMovieContext` angegeben ist. Das Argument `Initial` wird verwendet, um die Migrationen zu benennen. Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt. Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).

Mit dem Befehl `Update-Database` führen Sie die Methode `Up` in der Datei *Migrations/{time-stamp}_InitialCreate.cs* aus, mit der die Datenbank erstellt wird.

Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:

SqlException: Die bei der Anmeldung angeforderte Datenbank „RazorPagesMovieContext-GUID“ kann nicht geöffnet werden. Die Anmeldung ist fehlgeschlagen.
Fehler bei der Anmeldung des Benutzers „Benutzername“.

Sie haben den [Migrationsschritt](#pmc) verpasst.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!INCLUDE [model1](~/includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Hinzufügen eines Datenmodells

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **RazorPagesMovie** > **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*. Wählen Sie **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:

[!INCLUDE [model 2](~/includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a>Hinzufügen einer Datenbank-Verbindungszeichenfolge

Fügen Sie der Datei *appsettings.json* eine Verbindungszeichenfolge hinzu.

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a>Registrieren des Datenbankkontexts

Registrieren Sie den Datenbankkontext mit dem [Abhängigkeitsinjektionscontainer](xref:fundamentals/dependency-injection) in der [ConfigureServices-Methode der Startup-Klasse](xref:fundamentals/startup#the-startup-class) (*Startup.cs*):

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a>Hinzufügen von Tools für den Gerüstbau und Ausführen der anfänglichen Migration

In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:

* Fügen Sie das Visual Studio-Paket für die Webcodegenerierung hinzu. Dieses Paket ist für die Ausführung der Gerüstbau-Engine erforderlich.
* Fügen Sie eine anfängliche Migration hinzu.
* Aktualisieren Sie die Datenbank mit der anfänglichen Migration.

Wählen Sie im Menü **Tools** die Option **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus.

  ![PMC-Menü](../first-mvc-app/adding-model/_static/pmc.png)

Geben Sie in der PMC die folgenden Befehle ein:

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.3
Add-Migration Initial
Update-Database
```

Alternativ können folgende Befehle der .NET Core-CLI verwendet werden:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

Die folgende Meldung können Sie ignorieren:

    `Microsoft.EntityFrameworkCore.Model.Validation[30000]`

      *No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'ForHasColumnType()'*

Dieser Fehler wird im nächsten Tutorial behoben.

Mit dem Befehl `Install-Package` werden die für die Gerüstbau-Engine benötigten Tools installiert.

Mit dem Befehl `Add-Migration` wird Code generiert, um das anfängliche Datenbankschema zu erstellen. Das Schema basiert auf dem Modell, das in der Datei *Modelle/MovieContext.cs* für `DbContext` angegeben ist. Das Argument `Initial` wird verwendet, um die Migrationen zu benennen. Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt. Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).

Mit dem Befehl `Update-Database` führen Sie die Methode `Up` in der Datei *Migrations/{time-stamp}_InitialCreate.cs* aus, mit der die Datenbank erstellt wird.

[!INCLUDE [model 4windows](~/includes/RP/model4Win.md)]

[!INCLUDE [model 4](~/includes/RP/model4tbl.md)]

::: moniker-end

<a name="test"></a>

### <a name="test-the-app"></a>Testen der App

* Führen Sie die App aus, und fügen Sie `/Movies` an die URL im Browser an (`http://localhost:port/movies`).
* Testen Sie den Link **Create** (Erstellen).

  ![Seite „Create“](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* Testen Sie die Links **Edit** (Bearbeiten), **Details** und **Delete** (Löschen).

Überprüfen Sie bei der Auslösung einer SQL-Ausnahme, ob Migrationen ausgeführt wurden und die Datenbank aktualisiert wurde.

Im nächsten Tutorial finden Sie Erläuterungen zu den Dateien, die durch den Gerüstbau erstellt wurden.

> [!div class="step-by-step"]
> [Zurück: Erste Schritte](xref:tutorials/razor-pages/razor-pages-start)
> [Weiter: Gerüstbau mit Razor-Seiten](xref:tutorials/razor-pages/page)
