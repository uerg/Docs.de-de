---
title: Hinzufügen eines Modells zu einer ASP.NET Core MVC-App
author: rick-anderson
description: Fügen Sie ein Modell zu einer einfachen ASP.NET Core-App hinzu.
manager: wpickett
ms.author: riande
ms.date: 09/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app-xplat/adding-model
ms.openlocfilehash: 77750ba0df7775d6a0e4744811848bfe9782d995
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2018
---
# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Hinzufügen eines Modells zu einer ASP.NET Core MVC-App

[!INCLUDE [adding-model1](../../includes/mvc-intro/adding-model1.md)]

* Fügen Sie zum Ordner *Modelle* eine Klasse mit dem Namen *Movie.cs* hinzu.
* Fügen Sie der Datei *Models/Movie.cs* den folgenden Code hinzu:

[!code-csharp[](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

Die Datenbank benötigt das Feld `ID` für den primären Schlüssel. 

Erstellen Sie die App, um sicherzustellen, dass keine Fehler auftreten und Sie ein **M**odell zu Ihrer **M**VC-App hinzugefügt haben.

## <a name="prepare-the-project-for-scaffolding"></a>Vorbereiten des Projekts für den Gerüstbau

- Fügen Sie die folgenden hervorgehobenen NuGet-Pakete in die Datei *MvcMovie.csproj* ein:
             
   [!code-csharp[](start-mvc/sample/MvcMovie/MvcMovie.csproj?highlight=7,10)]

- Speichern Sie die Datei, und klicken Sie bei der **Info-Meldung** „There are unresolved dependencies“ (Es bestehen ungelöste Abhängigkeiten) auf **Wiederherstellen**.
- Erstellen sie eine *Models/MvcMovieContext.cs*-Datei, und fügen Sie die folgende Klasse des Typs `MvcMovieContext` hinzu:

   [!code-csharp[](start-mvc/sample/MvcMovie/Models/MvcMovieContext.cs)]
   
- Öffnen Sie die Datei *Startup.cs*, und fügen Sie zwei Usings hinzu:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet1&highlight=1,2)]

- Fügen Sie den Datenbankkontext zur Datei *Startup.cs* hinzu:

   [!code-csharp[](start-mvc/sample/MvcMovie/Startup.cs?name=snippet2&highlight=6-7)]

  Dadurch weiß das Entity Framework, welche Modellklassen im Datenmodell enthalten sind. Sie definieren eine *Entitätenmenge* von Filmobjekten, die in der Datenbank als Tabelle namens „Film“ dargestellt wird.

- Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

## <a name="scaffold-the-moviecontroller"></a>Erstellen des Gerüsts für „MovieController“

Öffnen Sie im Projektordner ein Terminalfenster, und führen Sie die folgenden Befehle aus:

```
dotnet restore
dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries 
```
Die Gerüstbau-Engine erstellt Folgendes:

* Einen Filmcontroller (*Controllers/MoviesController.cs*)
* Razor-Ansichtsdateien für Seiten der Typen „Erstellen“, „Löschen“, „Details“ und „Index“ (*Views/Movies/\*.cshtml*)

Die automatische Erstellung von [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete)-Aktionsmethoden und Ansichten (create, read update and delete – Erstellen, Lesen, Aktualisieren und Löschen) wird als *Gerüstbau* bezeichnet. Bald haben Sie eine voll funktionsfähige Webanwendung, mit der Sie eine Filmdatenbank verwalten können.

[!INCLUDE [adding-model 2x](../../includes/mvc-intro/adding-model2xp.md)]

[!INCLUDE [adding-model](../../includes/mvc-intro/adding-model3.md)]

Sie verfügen jetzt über eine Datenbank und Seiten zum Anzeigen, Bearbeiten, Aktualisieren und Löschen von Dateien. Im nächsten Tutorial werden wir mit dieser Datenbank arbeiten.

### <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Taghilfsprogramme](xref:mvc/views/tag-helpers/intro)
* [Globalization and localization (Globalisierung und Lokalisierung)](xref:fundamentals/localization)

> [!div class="step-by-step"]
> [Vorheriges Tutorial: Adding a View (Hinzufügen einer Ansicht)](adding-view.md)
> [Nächstes Tutorial: Working with SQLite (Arbeiten mit SQLite)](working-with-sql.md)
