---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio Code
author: rick-anderson
description: Erstellen einer Web-API unter macOS, Linux oder Windows mit ASP.NET Core MVC und Visual Studio Code
keywords: ASP.NET Core, WebAPI, Web API, REST, Mac, Linux,HTTP, Dienst, HTTP-Dienst, Visual Studio Code
ms.author: riande
manager: wpickett
ms.date: 5/24/2017
ms.topic: get-started-article
ms.assetid: 830b4bf5-dd14-423e-9f59-764a6f13a8f6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/web-api-vsc
ms.openlocfilehash: 17687e38aae066bdab4663268a2af54f20a6ad75
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-code-on-linux-macos-and-windows"></a>Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio Code unter macOS Linux und Windows

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)

In diesem Tutorial erstellen Sie eine Web-API zum Verwalten einer Liste von „To-Do“-Elementen. Sie werden keine Benutzeroberfläche erstellen.

Es gibt drei Versionen dieses Tutorials:

* macOS, Linux, Windows: Web-API mit Visual Studio Code (dieses Tutorial)
* macOS: [Web-API mit Visual Studio für Mac](xref:tutorials/first-web-api-mac)
* Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

## <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Sie müssen Folgendes herunterladen und installieren:
- [.NET Core](https://www.microsoft.com/net/core)
- [Visual Studio Code](https://code.visualstudio.com)
- Visual Studio Code [C#-Erweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

## <a name="create-the-project"></a>Erstellen eines Projekts

Führen Sie in einer Konsole die folgenden Befehle aus:

```console
mkdir TodoApi
cd TodoApi
dotnet new webapi
```

Öffnen Sie den Ordner *TodoApi* in Visual Studio Code, und wählen Sie die Datei *Startup.cs* aus.

- Klicken Sie auf **Ja**, wenn die **Warnung** „Required assets to build and debug are missing from 'TodoApi'. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird.
- Klicken Sie bei der **Infomeldung** „There are unresolved depedencies“ („Es bestehen ungelöste Abhängigkeiten“) auf **Wiederherstellen**.

<!-- uid: tutorials/first-mvc-app-xplat/start-mvc uses the pic below. If you change it, make sure it's consistent -->

![VS Code mit der Warnung „Required assets to build and debug are missing from „MvcMovie“. Add them?“ („Die erforderlichen Objekte für die Erstellung und das Debugging sind in 'TodoApi' nicht vorhanden. Sollen Sie hinzugefügt werden?“) angezeigt wird. „Nicht mehr fragen“, „Nicht jetzt“, „Ja“ und auch die Information über nicht aufgelöste Abhängigkeiten – Wiederherstellen – Schließen](web-api-vsc/_static/vsc_restore.png)

Drücken Sie **Debuggen** (F5), um das Programm zu erstellen und auszuführen. Navigieren Sie in einem Browser zu „http://localhost:5000/api/values“. Folgendes wird angezeigt:

`["value1","value2"]`

In der [Visual Studio Code-Hilfe](#visual-studio-code-help) finden Sie Tipps zum Arbeiten mit VS Code.

## <a name="add-support-for-entity-framework-core"></a>Hinzufügen der Unterstützung für Entity Framework Core

Bearbeiten Sie die Datei *TodoApi.csproj* so, dass der Datenbankanbieter [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) installiert wird. Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.

[!code-xml[Main](web-api-vsc/sample/TodoApi/TodoApi.csproj?highlight=12)]

Führen Sie `dotnet restore` zum Herunterladen und Installieren des Datenbankanbieters „EF Core InMemory“ aus. Sie können `dotnet restore` im Terminal ausführen oder `⌘⇧P` (macOS) oder `Ctrl+Shift+P` (Linux) in VS Code und dann **.NET** eingeben. Wählen Sie **.NET: Pakete wiederherstellen** aus.

## <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein Modell ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. In diesem Fall ist das einzige Modell ein To-Do-Element.

Fügen Sie einen Ordner namens *Models* hinzu. Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.

Fügen Sie eine `TodoItem`-Klasse mit dem folgenden Code hinzu:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert. Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.

Fügen Sie eine `TodoContext`-Klasse zum Ordner *Models* hinzu:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Hinzufügen eines Controllers

Erstellen Sie im Ordner *Controllers* eine Klasse namens `TodoController`. Fügen Sie den folgenden Code hinzu:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Starten der App

Drücken Sie in Visual Studio Code F5, um die App zu starten. Navigieren Sie zu „http://localhost:5000/api/todo“ (zum zuvor erstellten Controller `Todo`).

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

## <a name="visual-studio-code-help"></a>Hilfe zu Visual Studio Code

- [Erste Schritte](https://code.visualstudio.com/docs)
- [Debuggen](https://code.visualstudio.com/docs/editor/debugging)
- [Integriertes Terminal](https://code.visualstudio.com/docs/editor/integrated-terminal)
- [Tastenkombinationen](https://code.visualstudio.com/docs/getstarted/keybindings#_keyboard-shortcuts-reference)

  - [Mac-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
  - [Linux-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)
  - [Windows-Tastenkombinationen](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)

[!INCLUDE[next steps](../includes/webApi/next.md)]


