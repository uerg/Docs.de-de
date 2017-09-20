---
title: "Hinzufügen eines Modells zu einer ASP.NET Core MVC-App"
author: rick-anderson
description: "Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

Hinweis: Die ASP.NET Core 2.0-Vorlagen enthalten den Ordner *Modelle*.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **MvcMovie** > **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle* > **Hinzufügen** > **Klasse**. Nennen Sie die Klasse **Movie**, und fügen Sie Ihr die folgenden Eigenschaften hinzu:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Die Datenbank benötigt das Feld `ID` für den primären Schlüssel. 

Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen. Sie verfügen jetzt über ein **Modell** in Ihrer **MVC**-App.

## <a name="scaffolding-a-controller"></a>Gerüstbau für einen Controller

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* > **Hinzufügen > Controller**.

![Screenshot für oben genannten Schritt](adding-model/_static/add_controller.png)

Wählen Sie im Dialogfeld **MVC-Abhängigkeiten hinzufügen** die Option **Mindestens erforderliche Abhängigkeiten** und dann **Hinzufügen** aus.

![Screenshot für oben genannten Schritt](adding-model/_static/add_depend.png)

Visual Studio fügt die für das Gerüst eines Controllers erforderlichen Abhängigkeiten hinzu, aber der Controller selbst wird nicht erstellt. Der nächste Aufruf von **> Hinzufügen > Controller** erstellt den Controller. 

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Ordner *Controller* > **Hinzufügen > Controller**.

![Screenshot für oben genannten Schritt](adding-model/_static/add_controller.png)

Tippen Sie im Dialogfeld **Gerüst hinzufügen** auf **MVC-Controller mit Ansichten unter Verwendung von Entity Framework > Hinzufügen**.

![Dialogfeld „Gerüst hinzufügen“](adding-model/_static/add_scaffold2.png)

Bearbeiten Sie das Dialogfeld **Controller hinzufügen**:

* **Modellklasse:** *Movie (MvcMovie.Models)*
* **Datenkontextklasse:** Wählen Sie das Symbol **+** aus, und fügen Sie den Standardtyp **MvcMovie.Models.MvcMovieContext** hinzu

![Datenkontext hinzufügen](adding-model/_static/dc.png)

* **Ansichten:** Lassen Sie den Standardwert der einzelnen Optionen aktiviert
* **Controllername:** Behalten Sie den Standardwert *MoviesController* bei
* Tippen Sie auf **Hinzufügen**.

![Dialogfeld „Controller hinzufügen“](adding-model/_static/add_controller2.png)

Visual Studio erstellt Folgendes:

* Eine Entity Framework Core-[Datenbankkontext-Klasse](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)
* Einen Movies-Controller (*Controllers/MoviesController.cs*)
* Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ „Bearbeiten“ und „Index“ (*Views/Movies/&ast;.cshtml*)

Die automatische Erstellung des Datenbankkontexts und der [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden (create, read, update and delete – Erstellen, Lesen, Aktualisieren und Löschen) und Ansichten wird als *Gerüstbau* bezeichnet. Bald verfügen Sie über eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.

Wenn Sie die App auszuführen und auf den Link **Mvc Movie** klicken, erhalten Sie eine Fehlermeldung wie die folgende:

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

Sie müssen die Datenbank erstellen und verwenden dazu das EF Core-Feature [Migrationen](xref:data/ef-mvc/migrations). Mit „Migrationen“ können Sie eine Datenbank erstellen, die mit Ihrem Datenmodell übereinstimmt, und das Datenbankschema aktualisieren, wenn sich Ihr Datenmodell ändert.

## <a name="add-ef-tooling-and-perform-initial-migration"></a>Hinzufügen von EF-Tools und Ausführen der anfänglichen Migration

In diesem Abschnitt führen Sie folgende Aktionen mit der Paket-Manager-Konsole (Package Manager Console, PMC) aus:

* Fügen Sie das Entity Framework Core-Toolpaket hinzu. Dieses Paket ist erforderlich, um Migrationen hinzuzufügen und die Datenbank zu aktualisieren.
* Fügen Sie eine anfängliche Migration hinzu.
* Aktualisieren Sie die Datenbank mit der anfänglichen Migration.

Öffnen Sie das Menü **Extras**. Wählen Sie dort **NuGet-Paket-Manager > Paket-Manager-Konsole** aus.

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![PMC-Menü](adding-model/_static/pmc.png)

Geben Sie in der PMC die folgenden Befehle ein:

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

**Hinweis:** Wenn ein Fehler mit dem Befehl `Install-Package` ausgegeben wird, öffnen Sie den NuGet-Paket-Manager, und suchen Sie nach dem `Microsoft.EntityFrameworkCore.Tools`-Paket. Dadurch können Sie das Paket installieren oder überprüfen, ob es bereits installiert ist. Nutzen Sie bei Problemen mit PMC alternativ den [CLI-Ansatz](#cli).

Mit dem Befehl `Add-Migration` wird Code erstellt, um das anfängliche Datenbankschema zu erstellen. Das Schema basiert auf dem Modell, das in der Datei *Data/MvcMovieContext.cs* für `DbContext` angegeben ist. Das Argument `Initial` wird verwendet, um die Migrationen zu benennen. Sie können einen beliebigen Namen auswählen, sollten aber der Konvention zufolge einen Namen verwenden, der die Migration beschreibt. Weitere Informationen finden Sie unter [Introduction to migrations (Einführung in Migrationen)](xref:data/ef-mvc/migrations#introduction-to-migrations).

Mit dem Befehl `Update-Database` führen Sie in der Datei *Migrations/\<time-stamp>_InitialCreate.cs* die Methode `Up` aus, mit der die Datenbank erstellt wird.

<a name="cli"></a> Sie können die vorherigen Schritte über die Befehlszeilenschnittstelle (CLI) statt mit PMC ausführen:

* Fügen Sie [EF Core-Tools](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) zur *CSPROJ*-Datei hinzu.
* Führen Sie die folgenden Befehle über die Konsole aus (im Projektverzeichnis):

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![IntelliSense-Kontextmenü für ein Modellelement mit den verfügbaren Eigenschaften für ID, Preis, Veröffentlichungsdatum und Titel](adding-model/_static/ints.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro)
* [Globalisierung und Lokalisierung](xref:fundamentals/localization)

>[!div class="step-by-step"]
[Vorheriges Tutorial: Hinzufügen einer Ansicht](adding-view.md)
[Nächstes Tutorial: Arbeiten mit SQL](working-with-sql.md)  
