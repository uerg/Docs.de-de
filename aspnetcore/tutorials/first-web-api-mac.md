---
title: Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac
author: rick-anderson
description: Erstellen einer Web-API mit ASP.NET Core MVC und Visual Studio für Mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 699fbbf54abf1dc5c4156c559761110cdb375558
ms.sourcegitcommit: c867d7427bd4a88a78b2322e156367733b532730
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
ms.locfileid: "33923203"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Erstellen einer Web-API mit ASP.NET Core und Visual Studio für Mac

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Mike Wasson](https://github.com/mikewasson)

In diesem Tutorial wird eine Web-API zum Verwalten einer Liste von Aufgabenelementen erstellt. Die Benutzeroberfläche wird nicht erstellt.

Es gibt drei Versionen dieses Tutorials:

* macOS: Web-API mit Visual Studio für Mac (dieses Tutorial)
* Windows: [Web-API mit Visual Studio für Windows](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [Web-API mit Visual Studio Code](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Ein Beispiel für die Verwendung einer beständigen Datenbank finden Sie unter [Einführung in ASP.NET Core MVC unter macOS oder Linux](xref:tutorials/first-mvc-app-xplat/index).

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Erstellen eines Projekts

Klicken Sie in Visual Studio auf **Datei** > **Neue Projektmappe**.

![Neue Projektmappe in macOS](first-web-api-mac/_static/sln.png)

Klicken Sie auf **.NET Core-App** > **ASP.NET Core-Web-API** > **Weiter**.

![Dialogfeld „Neue Projektmappe“ in macOS](first-web-api-mac/_static/1.png)

Geben Sie *TodoApi* als **Projektnamen** ein, und klicken Sie dann auf **Erstellen**.

![Dialogfeld „Konfiguration“](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen** > **Mit Debugging starten**, um die App zu starten. Visual Studio startet dann einen Browser und navigiert zu `http://localhost:5000`. Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt. Ändern Sie die URL zu `http://localhost:<port>/api/values`. Die Daten von `ValuesController` werden angezeigt:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Hinzufügen der Unterstützung für Entity Framework Core

Installieren Sie den Datenbankanbieter [Entity Framework Core InMemory](/ef/core/providers/in-memory/). Dieser Datenbankanbieter ermöglicht, dass Entity Framework Core mit einer In-Memory Database verwendet wird.

* Klicken Sie im Menü **Projekt** auf **NuGet-Pakete hinzufügen**.

  * Sie können alternativ mit der rechten Maustaste auf **Abhängigkeiten** und dann auf **Pakete hinzufügen** klicken.

* Geben Sie `EntityFrameworkCore.InMemory` in das Suchfeld ein.
* Wählen Sie `Microsoft.EntityFrameworkCore.InMemory` aus, und klicken Sie dann auf **Pakete hinzufügen**.

### <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Ein Modell ist ein Objekt, das die Daten in Ihrer App darstellt. In diesem Fall ist das einzige Modell ein To-do-Element.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Projektnamen. Klicken Sie auf **Hinzufügen** > **Neuer Ordner**. Geben Sie dem Ordner den Namen *Modelle*.

![Neuer Ordner](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Hinweis: Sie können Modellklassen überall in Ihrem Projekt unterbringen, aber gemäß Konvention wird der Ordner *Models* verwendet.

Klicken Sie mit der rechten Maustaste auf den Ordner *Modelle*, und klicken Sie auf **Hinzufügen** > **Neue Datei** > **Allgemein** > **Leere Klasse**. Nennen Sie die Klasse *TodoItem*, und klicken Sie dann auf **Neu**.

Ersetzen Sie den generierten Code mit Folgendem:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Die Datenbank generiert die `Id`, wenn ein `TodoItem` erstellt wird.

### <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Der *Datenbankkontext* ist die Hauptklasse, die die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert. Sie können diese Klasse durch Ableiten von der `Microsoft.EntityFrameworkCore.DbContext`-Klasse erstellen.

Fügen Sie eine `TodoContext`-Klasse zum Ordner *Modelle* hinzu.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Hinzufügen eines Controllers

Fügen Sie im Ordner *Controller* im Projektmappen-Explorer die Klasse `TodoController` hinzu.

Ersetzen Sie den generierten Code durch den folgenden:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Starten der App

Klicken Sie in Visual Studio auf **Ausführen** > **Mit Debugging starten**, um die App zu starten. Visual Studio startet einen Browser und navigiert zu `http://localhost:<port>`, wobei es sich bei `<port>` um eine zufällig ausgewählte Portnummer handelt. Ihnen wird der HTTP-Fehler „404 (Not Found)“ angezeigt. Ändern Sie die URL zu `http://localhost:<port>/api/values`. Die Daten von `ValuesController` werden angezeigt:

```json
["value1","value2"]
```

Navigieren Sie zum `Todo`-Controller auf `http://localhost:<port>/api/todo`. Die folgende JSON-Datei wird zurückgegeben:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Implementieren der anderen CRUD-Vorgänge

Fügen Sie Methoden `Create`, `Update` und `Delete` zum Controller hinzu. Bei diesen Methoden handelt sich um Variationen eines Designs, darum wird der Code mit den hervorgehobenen Hauptunterschieden dargestellt. Erstellen Sie das Projekt, nachdem Sie Code hinzugefügt oder geändert haben.

### <a name="create"></a>Erstellen

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Die zuvor aufgeführte Methode reagiert auf eine HTTP POST-Anforderung, die vom Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird. Das [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute)-Attribut weist MVC an, den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung abzurufen.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Die zuvor aufgeführte Methode reagiert auf eine HTTP POST-Anforderung, die vom Attribut [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) angegeben wird. MVC ruft den Wert des To-Do-Elements aus dem Text der HTTP-Anforderung ab.
::: moniker-end

Die `CreatedAtRoute`-Methode gibt eine 201-Antwort zurück. Dies ist die Standardantwort für eine HTTP POST-Methode, die eine neue Ressource auf dem Server erstellt. `CreatedAtRoute` fügt der Antwort außerdem einen Adressheader hinzu. Der Adressheader gibt den URI des neu erstellten To-Do-Elements zurück. Siehe [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Verwenden von Postman zum Senden einer Erstellungsanforderung

* Starten Sie die App (**Ausführen** > **Mit Debugging starten**).
* Öffnen Sie Postman.

![Postman-Konsole](first-web-api/_static/pmc.png)

* Aktualisieren Sie die Portnummer in der localhost-URL.
* Legen Sie die HTTP-Methode auf *POST* fest.
* Klicken Sie auf die Registerkarte **Body** (Text).
* Wählen Sie das Optionsfeld **raw** (Unformatiert) aus.
* Legen Sie den Typ auf *JSON (application/json)* fest.
* Geben Sie einen Anforderungstext mit einem To-Do-Element ein, der folgendem JSON-Code ähnelt:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Klicken Sie auf die Schaltfläche **Senden**.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Wenn nach dem Klicken auf **Senden** keine Antwort angezeigt wird, deaktivieren Sie die Option **SSL certification verification** (Überprüfung der SSL-Zertifizierung). Diese finden Sie unter **Datei** > **Einstellungen**. Klicken Sie erneut auf die Schaltfläche **Senden**, nachdem Sie diese Einstellung deaktiviert haben.
::: moniker-end

Klicken Sie auf die Registerkarte **Header** im Bereich **Antwort**, und kopieren Sie den Headerwert von **Location** (Speicherort):

![Registerkarte „Header“ in der Postman-Konsole](first-web-api/_static/pmc2.png)

Sie können den Adressheader-URI verwenden, um auf die Ressource zuzugreifen, die Sie erstellt haben. Die `Create`-Methode gibt [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_) zurück. Der erste Parameter, der an `CreatedAtRoute` übergeben wird, stellt die benannte Route dar, die für das Generieren der URL verwendet werden soll. Bedenken Sie, dass die `GetById`-Methode die benannte Route `"GetTodo"` erstellt hat:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Update

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` ähnelt `Create`, verwendet aber HTTP PUT. Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). Gemäß der HTTP-Spezifikation erfordert eine PUT-Anforderung, dass der Client die gesamte aktualisierte Entität und nicht nur die Deltas sendet. Verwenden Sie HTTP PATCH, um Teilupdates zu unterstützen.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Löschen

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Die Antwort ist [204 (Kein Inhalt)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![Postman-Konsole mit der Antwort „204 (Kein Inhalt)“](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
