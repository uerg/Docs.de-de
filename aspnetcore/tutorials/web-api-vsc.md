---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
author: rick-anderson
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/web-api-vsc
ms.openlocfilehash: 4c41c949a9b5ca8db8928a0a53aff928fd7c8a4e
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38216236"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-code"></a>Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)

In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt. Es wird keine Benutzeroberfläche erstellt.

Es gibt drei Versionen dieses Tutorials:

* macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)
* macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)
* Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[prerequisites](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-the-project"></a>Erstellen eines Projekts

Führen Sie in einer Konsole die folgenden Befehle aus:

```console
dotnet new webapi -o TodoApi
code TodoApi
```

Der Ordner *TodoApi* wird in Visual Studio Code (VS Code) geöffnet. Wählen Sie die Datei *Startup.cs* aus.

* Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird.
* Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird. Nicht erneut nachfragen, Nicht jetzt, Ja](web-api-vsc/_static/vsc_restore.png)

Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen. Navigieren Sie in einem Browser zu http://localhost:5000/api/values. Die folgende Ausgabe wird angezeigt:

```json
["value1","value2"]
```

In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.

## <a name="add-support-for-entity-framework-core"></a>Hinzufügen der Unterstützung für Entity Framework Core

:::moniker range="<= aspnetcore-2.0"
Durch das Erstellen eines neuen Projekts in ASP.NET Core 2.0 wird der Paketverweis [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) zur Datei *TodoApi.csproj* hinzugefügt:

[!code-xml[](first-web-api/samples/2.0/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end
:::moniker range=">= aspnetcore-2.1"
Durch das Erstellen eines neuen Projekts in ASP.NET Core 2.1 oder höher wird der Paketverweis [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) der Datei *TodoApi.csproj* hinzugefügt:

[!code-xml[](first-web-api/samples/2.1/TodoApi/TodoApi.csproj?name=snippet_Metapackage&highlight=2)]
:::moniker-end

Der Datenbankanbieter [Entity Framework Core InMemory](/ef/core/providers/in-memory/) muss nicht separat installiert werden. Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.

## <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein Modell ist ein Objekt, das die Daten in Ihrer App darstellt. In diesem Fall ist das einzige Modell ein To-do-Element.

Fügen Sie einen Ordner namens *Models* hinzu. Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.

Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert. Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.

Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Hinzufügen eines Controllers

Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`. Ersetzen Sie den Inhalt durch folgenden Code:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio Code F5, um die App zu starten. Navigieren Sie zu http://localhost:5000/api/todo (der `Todo`-Controller, den wir eben erstellt haben).

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Hilfe zu Visual Studio Code

* [Erste Schritte](https://code.visualstudio.com/docs)
* [Debuggen](https://code.visualstudio.com/docs/editor/debugging)
* [Integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
* [Tastenkombinationen](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  * [macOS-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  * [Linux-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  * [Windows-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]
