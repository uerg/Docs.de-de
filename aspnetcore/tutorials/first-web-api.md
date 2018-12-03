---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio
author: rick-anderson
description: Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: tutorials/first-web-api
ms.openlocfilehash: eb23d5376742e04712f714263815582fddc5d18e
ms.sourcegitcommit: e9b99854b0a8021dafabee0db5e1338067f250a9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2018
ms.locfileid: "52450696"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio"></a>Erstellen einer Web-API mit ASP.NET Core und Visual Studio

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)

In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt. Es wird keine Benutzeroberfläche (UI) erstellt.

Es gibt drei Versionen dieses Tutorials:

* Windows: Web-API mit Visual Studio für Windows (dieses Tutorial, siehe [Videoversion](https://www.youtube.com/watch?v=TTkhEyGBfAk))
* macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

> [!NOTE]
> Wir testen gerade eine vorgeschlagene neue Struktur für das ASP.NET Core-Inhaltsverzeichnis.  Falls Sie einige Minuten Zeit haben, um einen Test durchzuführen, in dem Sie sieben unterschiedliche Artikel im aktuellen und vorgeschlagene Inhaltsverzeichnis finden sollen, [klicken Sie hier, um daran teilzunehmen](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Erstellen eines Projekts

Führen Sie in Visual Studio die folgenden Schritte aus:

* Wählen Sie im Menü **Datei** den Befehl **Neu** > **Projekt** aus.
* Wählen Sie die Vorlage **ASP.NET Core-Webanwendung** aus. Geben Sie dem Projekt den Namen *TodoApi*, und klicken Sie auf **OK**.
* Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die ASP.NET Core-Version aus. Wählen Sie die Vorlage **API** aus, und klicken Sie auf **OK**. Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.

### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio STRG+F5 zum Starten der App. Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt. Chrome, Microsoft Edge und Firefox zeigen die folgende Ausgabe an:

```json
["value1","value2"]
```

Wenn Sie Internet Explorer verwenden, werden Sie dazu aufgefordert, eine *values.json*-Datei zu speichern.

### <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein Modell ist ein Objekt, das die Daten in der App darstellt. In diesem Fall ist das einzige Modell ein To-do-Element.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen. Klicken Sie auf **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

> [!NOTE]
> Die Modellklassen können überall im Projekt gespeichert werden. Der Ordner *Modelle* wird gemäß Konvention für Modellklassen verwendet.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse *TodoItem*, und klicken Sie auf **Hinzufügen**.

Aktualisieren Sie die `TodoItem`-Klasse mit dem folgenden Code:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.

### <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert. Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie anschließend **Hinzufügen** > **Klasse** aus. Nennen Sie die Klasse *TodoContext*, und klicken Sie auf **Hinzufügen**.

Ersetzen Sie die Klasse durch den folgenden Code:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Hinzufügen eines Controllers

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*. Klicken Sie auf **Hinzufügen** > **Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **API-Controllerklasse** aus. Nennen Sie die Klasse *TodoController*, und klicken Sie auf **Hinzufügen**.

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

Ersetzen Sie die Klasse durch den folgenden Code:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio STRG+F5 zum Starten der App. Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>/api/values`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt. Navigieren Sie zum `Todo`-Controller auf `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
