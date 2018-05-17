---
title: Erste Schritte mit NSwag und ASP.NET Core
author: zuckerthoben
description: Erfahren Sie, wie Sie NSwag zum Generieren von Dokumentationen und Hilfeseiten für eine ASP.NET Core-Web-API-App verwenden können.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-nswag
ms.openlocfilehash: 80e6a9e1702d8f68d139d2ff9c3a01a27c40cecb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-nswag-and-aspnet-core"></a>Erste Schritte mit NSwag und ASP.NET Core

Von [Christoph Nienaber](https://twitter.com/zuckerthoben) und [Rico Suter](https://rsuter.com)

Die Verwendung von [NSwag](https://github.com/RSuter/NSwag) mit ASP.NET Core-Middleware erfordert das NuGet-Paket [NSwag.AspNetCore](https://www.nuget.org/packages/NSwag.AspNetCore/). Das Paket besteht aus einem Swagger-Generator, der Swagger-Benutzeroberfläche (Version 2 und 3) und der [ReDoc-Benutzeroberfläche](https://github.com/Rebilly/ReDoc).

Es wird empfohlen, die Funktionen zur Codegenerierung von NSwag zu verwenden. Wählen Sie eine der folgenden Optionen für die Codegenerierung aus:

* Verwenden Sie [NSwagStudio](https://github.com/NSwag/NSwag/wiki/NSwagStudio). Dies ist eine Windows-Desktop-App für das Generieren von Clientcode in C# und TypeScript für Ihre API.
* Verwenden Sie das NuGet-Paket [NSwag.CodeGeneration.CSharp](https://www.nuget.org/packages/NSwag.CodeGeneration.CSharp/) oder [NSwag.CodeGeneration.TypeScript](https://www.nuget.org/packages/NSwag.CodeGeneration.TypeScript/), um Code innerhalb des Projekts zu generieren.
* Verwenden Sie NSwag über die [Befehlszeile](https://github.com/NSwag/NSwag/wiki/CommandLine).
* Verwenden Sie das NuGet-Paket [NSwag.MSBuild](https://github.com/NSwag/NSwag/wiki/MSBuild).

## <a name="features"></a>Features

Der Hauptgrund für die Verwendung von NSwag liegt darin, dass nicht nur die Swagger-Benutzeroberfläche und der Swagger-Generator, sondern auch die flexiblen Funktionen für die Codegenerierung verwendet werden können. Sie müssen keine API besitzen, sondern können APIs von Drittanbietern verwenden, die Swagger integrieren, und NSwag eine Clientimplementierung generieren lassen. In jedem Fall wird der Entwicklungszyklus beschleunigt und Sie können sich schnell an API-Änderungen anpassen.

## <a name="package-installation"></a>Paketinstallation

Das NuGet-Paket „NSwag“ kann folgendermaßen hinzugefügt werden:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Aus dem Fenster **Paket-Manager-Konsole**:

    ```powershell
    Install-Package NSwag.AspNetCore
    ```

* Aus dem Dialogfeld **NuGet-Pakete verwalten**:
  * Klicken Sie mit der rechten Maustaste unter **Projektmappen-Explorer** > **NuGet-Pakete verwalten** auf Ihr Projekt.
  * Legen Sie die **Paketquelle** auf „nuget.org“ fest.
  * Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.
  * Wählen Sie das Paket „NSwag.AspNetCore“ auf der Registerkarte **Durchsuchen** aus, und klicken Sie auf **Installieren**.

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio für Mac](#tab/visual-studio-mac)

* Klicken Sie mit der rechten Maustaste auf den Ordner *Pakete* unter **Lösungspad** > **Pakete hinzufügen...**.
* Legen Sie im Fenster **Pakete hinzufügen** das Dropdownmenü **Quelle** auf „nuget.org“ fest.
* Geben Sie „NSwag.AspNetCore“ in das Suchfeld ein.
* Wählen Sie das Paket „NSwag.AspNetCore“ aus dem Ergebnisbereich aus, und klicken Sie auf **Paket hinzufügen**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Führen Sie folgenden Befehl aus dem **integrierten Terminal** aus:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli)

Führen Sie den folgenden Befehl aus:

```console
dotnet add TodoApi.NSwag.csproj package NSwag.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a>Hinzufügen und Konfigurieren von Swagger-Middleware

Importieren Sie folgende Namespaces in die `Info`-Klasse:

```csharp
using NSwag.AspNetCore;
using System.Reflection;
using NJsonSchema;
```

Aktivieren Sie die Middleware in der `Startup.Configure`-Methode, um die generierte Swagger-Spezifikation und die Swagger-Benutzeroberfläche zu verarbeiten:

[!code-cs[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/Startup.cs?name=snippet_Configure&highlight=4,7-10)]

Starten Sie die App. Navigieren Sie zu `/swagger`, um die Swagger-Benutzeroberfläche anzuzeigen. Navigieren Sie zu `/swagger/v1/swagger.json`, um die Swagger-Spezifikation anzuzeigen.

## <a name="code-generation"></a>Codeerzeugung

### <a name="via-nswagstudio"></a>Über NSwagStudio

* Installieren Sie `NSwagStudio` über das offizielle [GitHub-Repository](https://github.com/RSuter/NSwag/wiki/NSwagStudio).

* Starten Sie NSwagStudio. Geben Sie den Speicherort von *swagger.json* ein, oder kopieren Sie diesen direkt:

![NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio.png)

* Geben Sie den gewünschten Ausgabetyp für den Client an. Zu den Optionen zählen **TypeScript-Client**, **CSharp-Client**, und **CSharp-Web-API-Controller**. Bei der Verwendung eines Web-API-Controllers handelt es sich im Grunde um eine umgekehrte Generierung. Dieser verwendet die Spezifikation eines Diensts, um den Dienst erneut zu erstellen.

* Klicken Sie auf **Generate Outputs** (Ausgaben generieren).

* Hier finden Sie eine vollständige Clientimplementierung des *TodoApi.NSwag*-Beispiels in C#:

![Ausgabe von NSwagStudio](web-api-help-pages-using-swagger/_static/NSwagStudio-Output.png)

* Fügen Sie die Datei zu einem Clientprojekt (z.B. zu einer [Xamarin.Forms](/xamarin/xamarin-forms/)-App) hinzu. Verwendung der API:

```csharp
var todoClient = new TodoClient();

// Gets all Todos from the Api
var allTodos = await todoClient.GetAllAsync();

// Create a new TodoItem and save it in the Api
var createdTodo = await todoClient.CreateAsync(new TodoItem);

// Get a single Todo by Id
var foundTodo = await todoClient.GetByIdAsync(1);
```

> [!NOTE]
> Sie können eine Basis-URL und/oder einen HTTP-Client in den API-Client einfügen. Eine bewährte Methode besteht darin, [HttpClient](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/) immer wieder zu verwenden.

Sie können Ihre API nun einfach in Clientprojekte implementieren.

### <a name="other-ways-to-generate-client-code"></a>Weitere Möglichkeiten zum Generieren von Clientcode

Sie können den Code auf andere Arten generieren, die besser zu Ihrem Workflow passen:

* [MSBuild](https://www.nuget.org/packages/NSwag.MSBuild/)

* [Im Code](https://github.com/NSwag/NSwag/wiki/SwaggerToCSharpClientGenerator)

* [T4-Vorlagen](https://github.com/NSwag/NSwag/wiki/T4)

## <a name="customization"></a>Anpassung

### <a name="xml-comments"></a>XML-Kommentare

XML-Kommentare werden mithilfe von folgenden Ansätzen aktiviert:

#### <a name="visual-studiotabvisual-studio-xml"></a>[Visual Studio](#tab/visual-studio-xml/)
* Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus.
* Überprüfen Sie das Feld **XML-Dokumentationsdatei** unter dem Abschnitt **Ausgabe** der Registerkarte **Erstellen**:

![Projekteigenschaften auf der Registerkarte „Erstellen“](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[Visual Studio für Mac](#tab/visual-studio-mac-xml/)
* Öffnen Sie das Dialogfeld **Projektoptionen** > **Erstellen** > **Compiler**.
* Überprüfen Sie das Feld **XML-Dokumentation generieren** im Abschnitt **Allgemeine Optionen**:

![Abschnitt „Allgemeine Optionen“ der Projektoptionen](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[Visual Studio Code](#tab/visual-studio-code-xml/)
Fügen Sie den folgenden Codeausschnitt manuell zu der *CSPROJ*-Datei hinzu:

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.NSwag/TodoApiNSwag.csproj?range=7-9)]

* * *
## <a name="data-annotations"></a>Datenanmerkungen

NSwag verwendet die [Reflektion](/dotnet/csharp/programming-guide/concepts/reflection), und eine bewährte Methode für Web-API-Aktionen besteht in der Rückgabe von [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult). Folglich kann NSwag nicht ableiten, was die Aktion ausführt und was sie zurückgibt. Betrachten Sie das folgende Beispiel:

```csharp
public IActionResult Create([FromBody] TodoItem item)
{
    if (item == null)
    {
        return BadRequest();
    }

    _context.TodoItems.Add(item);
    _context.SaveChanges();

     return CreatedAtRoute("GetTodo", new { id = item.Id }, item);
}
```

Die vorherige Aktion gibt `IActionResult` zurück. Innerhalb der Aktion gibt sie jedoch [CreatedAtRoute](/dotnet/api/system.web.http.apicontroller.createdatroute) oder [BadRequest](/dotnet/api/system.web.http.apicontroller.badrequest) zurück. Datenanmerkungen werden verwendet, um Clients mitzuteilen, welche HTTP-Antwort die Aktion zurückgibt. Ergänzen Sie die Aktion mit folgenden Attributen:

```csharp
[HttpPost]
[ProducesResponseType(typeof(TodoItem), 201)] // Created
[ProducesResponseType(typeof(TodoItem), 400)] // BadRequest
public IActionResult Create([FromBody] TodoItem item)
```

Der Swagger-Generator kann diese Aktion nun genau beschreiben, und generierte Clients wissen, was sie beim Aufrufen des Endpunkts empfangen. Es wird empfohlen, alle Aktionen mit diesen Attributen zu ergänzen. Weitere Informationen zu den Richtlinien dazu, welche HTTP-Antworten Ihre API-Aktionen zurückgeben sollten, finden Sie in der [RFC 7231 specification (Spezifikation von RFC 7231)](https://tools.ietf.org/html/rfc7231#section-4.3).
