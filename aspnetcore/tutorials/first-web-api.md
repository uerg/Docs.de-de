---
title: "Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows"
author: rick-anderson
description: "Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Windows"
keywords: ASP.NET Core, WebAPI, Web-API, REST, HTTP, Service, HTTP-Dienst
ms.author: riande
manager: wpickett
ms.date: 8/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-web-api
ms.openlocfilehash: 4aab61c7ee4498b33a4ea8bbec6033ce9828e2af
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
#<a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Windows

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)

In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen. Sie werden keine Benutzeroberfläche erstellen.

Es gibt drei Versionen dieses Tutorials:

* Windows: Web-API mit Visual Studio für Windows (dieses Tutorial)
* macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[install 2.0](../includes/install2.0.md)]

In [dieser PDF](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/first-web-api/_static/_webAPI.pdf) finden Sie die Version ASP.NET Core 1.1.

## <a name="create-the-project"></a>Erstellen eines Projekts

Klicken Sie in Visual Studio auf das Menü **Datei** > **Neu** > **Projekt**.

Wählen Sie die Projektvorlage **ASP.NET Core Web Application (.NET Core)** aus. Geben Sie dem Projekt den Namen `TodoApi`, und klicken Sie auf **OK**.

![Dialogfeld "Neues Projekt"](first-web-api/_static/new-project.png)

Wählen Sie im Dialogfeld **ASP.NET Core-Webanwendung – TodoAPI** die Vorlage **Web-API** aus. Klicken Sie auf **OK**. Wählen Sie **nicht** **Enable Docker Support** (Docker-Unterstützung aktivieren) aus.

![Dialogfeld Neue ASP.NET-Webanwendung mit ausgewählter Web-API-Projektvorlage aus ASP.NET Core-Vorlagen](first-web-api/_static/web-api-project.png)

### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio STRG+F5 zum Starten der App. Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei es sich bei *port* um eine zufällig ausgewählte Portnummer handelt. Chrome, Edge und Firefox zeigen Folgendes an:

```
["value1","value2"]
``` 

### <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. In diesem Fall ist das einzige Modell ein To-Do-Element.

Fügen Sie einen Ordner namens „Modelle“ hinzu. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen. Klicken Sie auf **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

Hinweis: Die Modellklassen können überall in Ihrem Projekt platziert werden, aber der Ordner *Modelle* wird gemäß der Konvention verwendet.

Fügen Sie eine `TodoItem`-Klasse hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus. Benennen Sie die Klasse `TodoItem`, und klicken Sie auf **Hinzufügen**.

Ersetzen Sie den generierten Code durch den folgenden:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.

### <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert. Diese Klasse wird durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellt.

Fügen Sie eine `TodoContext`-Klasse hinzu. Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und wählen Sie **Hinzufügen** > **Klasse** aus. Benennen Sie die Klasse `TodoContext`, und klicken Sie auf **Hinzufügen**.

Ersetzen Sie den generierten Code durch den folgenden:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Hinzufügen eines Controllers

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Ordner *Controller*. Klicken Sie auf **Hinzufügen** > **Neues Element**. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die Vorlage **Web API Controller Class** aus. Nennen Sie die Klasse `TodoController`.

![Dialogfeld „Neues Element“ mit Controller im Suchfeld und ausgewähltem Web-API-Controller](first-web-api/_static/new_controller.png)

Ersetzen Sie den generierten Code durch den folgenden:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]
  
### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio STRG+F5 zum Starten der App. Visual Studio startet einen Browser und navigiert zu `http://localhost:port/api/values`, wobei *port* eine zufällig ausgewählte Portnummer ist. Wenn Sie Chrome, Edge oder Firefox verwenden, werden die Daten angezeigt. Wenn Sie Internet Explorer verwenden, werden Sie aufgefordert, die Datei *values.json* zu öffnen oder zu speichern. Navigieren Sie zum `Todo`-Controller, den wir eben erstellt haben `http://localhost:port/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]

