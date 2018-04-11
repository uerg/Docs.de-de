---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Rufen Sie eine Web-API aus einem .NET-Client (c#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: a243eeb982ba581e237263c4e31e130d634aff0e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a>Rufen Sie eine Web-API aus einem .NET-Client (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson) und [Rick Anderson](https://twitter.com/RickAndMSFT)

[Herunterladen des abgeschlossenen Projekts](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Dieses Lernprogramm zeigt, wie eine Web-API in einer .NET-Anwendung mit [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

In diesem Lernprogramm wird eine Client-app, die die folgenden Web-API nutzt geschrieben:

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen eines Produkts nach ID | GET | /api/products/*id* |
| Erstellen eines neuen Produkts | POST | /api/products |
| Aktualisieren eines Produkts | PUT | /api/products/*id* |
| Löschen eines Produkts | DELETE | /api/products/*id* |

Zum Implementieren dieser API mit ASP.NET Web-API finden Sie unter [erstellen eine Web-API, CRUD-Vorgänge unterstützt](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Der Einfachheit halber wird die Clientanwendung in diesem Lernprogramm eine Windows-Konsolenanwendung. **"HttpClient"** wird auch für Windows Phone und Windows Store-apps unterstützt. Weitere Informationen finden Sie unter [Schreiben von Web-API-Clientcode für mehrere Plattformen mithilfe von portablen Bibliotheken](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Erstellen der Konsolenanwendung

In Visual Studio, erstellen Sie eine neue Windows-Konsolen-app mit dem Namen **HttpClientSample** und fügen Sie in den folgenden Code:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Der vorhergehende Code ist die vollständige Client-app.

`RunAsync` ausgeführt wird, und blockiert, bis er abgeschlossen wurde. Die meisten **"HttpClient"** Methoden sind asynchrone, daran, dass sie die Netzwerk-e/a ausführen. Alle asynchronen Vorgänge erfolgen in `RunAsync`. Normalerweise keine app den Haupt-Thread blockieren, aber diese app lässt nicht die ein Eingreifen.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installieren Sie die Web-API-Client-Bibliotheken

Verwenden Sie NuGet Package Manager zum Installieren des Pakets Web API Client Libraries.

Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus. Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl aus:

`Install-Package Microsoft.AspNet.WebApi.Client`

Dieser Befehl wird dem Projekt die folgenden NuGet-Pakete hinzugefügt:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET ist eine beliebte hohe Leistung JSON-Framework für .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Fügen Sie eine Modellklasse hinzu

Überprüfen Sie die `Product` Klasse:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Diese Klasse entspricht das Datenmodell, das von der Web-API verwendet. Können Sie eine app **"HttpClient"** zum Lesen einer `Product` -Instanz anhand einer HTTP-Antwort. Die app keine Deserialisierungscode schreiben.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Erstellen und initialisieren Sie "HttpClient"

Überprüfen Sie die statische **"HttpClient"** Eigenschaft:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**"HttpClient"** einmal instanziiert werden soll und die Lebensdauer einer Anwendung wiederverwendet wird. Die folgenden Bedingungen können dazu führen, **SocketException** Fehler:

* Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung.
* Der Server stark ausgelastet.

Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung kann die verfügbaren Sockets ausgeschöpft.

Der folgende code initialisiert das **"HttpClient"** Instanz:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Der vorangehende Code:

* Definiert die Basis-URI für HTTP-Anforderungen. Ändern Sie die Portnummer an den Port in der Server-app verwendet. Die app funktioniert nicht, es sei denn, der Port für die Server-app verwendet wird.
* Legt den Accept-Header auf "Application/Json" fest. Dieser Header festlegen, weist den Server zum Senden von Daten im JSON-Format.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Senden einer GET-Anforderung zum Abrufen einer Ressource

Der folgende Code sendet eine GET-Anforderung für ein Produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Die **GetAsync** Methode sendet die HTTP-GET-Anforderung. Wenn die Methode abgeschlossen wird, gibt es eine **HttpResponseMessage** , enthält die HTTP-Antwort. Wenn der Statuscode in der Antwort einen Erfolgscode ist, enthält der Antworttext die JSON-Darstellung eines Produkts. Rufen Sie **ReadAsAsync** zu deserialisierende JSON-Nutzlast an einen `Product` Instanz. Die **ReadAsAsync** Methode ist asynchron, da der Antworttext beliebig groß sein kann.

**"HttpClient"** löst keine Ausnahme aus, wenn die HTTP-Antwort einen Fehlercode enthält. Stattdessen die **IsSuccessStatusCode** Eigenschaft **"false"** lautet der Status ein Fehlercode. Wenn Sie es vorziehen, die HTTP-Fehlercodes als Ausnahmen behandelt werden sollen, rufen Sie [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) auf dem Antwortobjekt. `EnsureSuccessStatusCode` löst eine Ausnahme aus, wenn der Statuscode außerhalb des Bereichs 200 liegt&ndash;299. Beachten Sie, dass **"HttpClient"** kann Ausnahmen auslösen, aus anderen Gründen &mdash; z. B., wenn die Anforderung ein Timeout eintritt.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Medientypformatierer deserialisiert

Wenn **ReadAsAsync** wird aufgerufen, ohne Parameter verwendet eine Reihe von *medienformatierer* zum Lesen des Antworttexts. Die Standard-Formatierer unterstützt JSON und XML-Formular-Url-codierte Daten.

Anstatt die Standard-Formatierer verwenden, können Sie eine Liste der Formatierer zum Bereitstellen der **ReadAsAsync** Methode.  Verwenden eine Liste der Formatierer ist hilfreich, wenn Sie eine benutzerdefinierte medientypformatierer haben:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Weitere Informationen finden Sie unter [Medienformatierer in ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Sendet eine POST-Anforderung zum Erstellen einer Ressource

Der folgende Code sendet eine POST-Anforderung, die enthält eine `Product` Instanz im JSON-Format:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Die **PostAsJsonAsync** Methode:

* Serialisiert ein Objekt in JSON.
* Sendet die JSON-Nutzlast in einer POST-Anforderung.

Wenn die Anforderung erfolgreich ist:

* Es sollte eine 201 (erstellt) Antwort zurückgeben.
* Die Antwort sollte die URL der erstellten Ressourcen im Location-Header enthalten.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Sendet eine PUT-Anforderung zum Aktualisieren einer Ressourcenpools

Der folgende Code sendet eine PUT-Anforderung, um ein Produkt zu aktualisieren:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Die **PutAsJsonAsync** Methode funktioniert wie **PostAsJsonAsync**, außer dass eine PUT-Anforderung anstelle von POST sendet.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Senden eine DELETE-Anforderung zum Löschen eines Ressourcenpools

Der folgende Code sendet eine DELETE-Anforderung zum Löschen eines Produkts:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Wie "GET" muss eine DELETE-Anforderung keinen Anforderungstext. Sie müssen nicht JSON oder XML-Format mit DELETE angeben.

## <a name="test-the-sample"></a>Testen des Beispiels

So testen Sie die Client-app:

1. [Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) und führen Sie die Server-app. [Anweisungen zum Download.](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample) Stellen Sie sicher, dass die Server-app funktioniert. Für Exaxmple `http://localhost:64195/api/products` sollte eine Liste der Produkte zurück.
2. Legen Sie die Basis-URI für HTTP-Anforderungen. Ändern Sie die Portnummer an den Port in der Server-app verwendet.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Führen Sie die Client-app. Es wird die folgende Ausgabe generiert:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
