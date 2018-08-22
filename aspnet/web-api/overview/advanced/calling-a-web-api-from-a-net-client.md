---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Aufrufen eine Web-API aus einem .NET-Client (c#) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831903"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Aufrufen einer Webs-API aus einem .NET-Client (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson) und [Rick Anderson](https://twitter.com/RickAndMSFT)

[Herunterladen des abgeschlossenen Projekts](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Anweisungen zum Download.](/aspnet/core/tutorials/#how-to-download-a-sample) 

In diesem Tutorial wird gezeigt, wie eine Web-API aufrufen, aus einer .NET-Anwendung mit [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

In diesem Tutorial wird eine Client-app geschrieben, die die folgende Web-API nutzt:

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen eines Produkts nach ID | GET | /api/products/*id* |
| Erstellen eines neuen Produkts | POST | /api/products |
| Aktualisieren eines Produkts | PUT | /api/products/*id* |
| Löschen eines Produkts | DELETE | /api/products/*id* |

Gewusst wie: Implementieren Sie diese API mit ASP.NET Web-API finden Sie unter [erstellen eine Web-API, CRUD-Vorgänge unterstützt](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Der Einfachheit halber ist die Client-Anwendung in diesem Tutorial eine Windows-Konsolenanwendung. **"HttpClient"** wird auch für Windows Phone und Windows Store-apps unterstützt. Weitere Informationen finden Sie unter [Schreiben von Web-API-Client-Code für mehrere Plattformen mithilfe von portablen Bibliotheken](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Erstellen der Konsolenanwendung

Erstellen Sie in Visual Studio eine neue Windows-Konsolen-app mit dem Namen **HttpClientSample** und fügen Sie in den folgenden Code:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Der vorangehende Code ist die vollständige Client-app.

`RunAsync` Ausführungen und blockiert, bis der Vorgang abgeschlossen ist. Die meisten **"HttpClient"** Methoden sind asynchrone, da sie die Netzwerk-e/a ausführen. Alle asynchronen Vorgänge erfolgen in `RunAsync`. Normalerweise eine app den Hauptthread blockiert nicht, aber diese app nicht eingreifen kann.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Installieren Sie die Web-API-Client-Bibliotheken

Verwenden Sie NuGet Package Manager zum Installieren des Pakets für die Web-API-Client-Bibliotheken.

Öffnen Sie das Menü **Extras**, und wählen Sie **NuGet-Paket-Manager** > **Paket-Manager-Konsole** aus. Geben Sie in der Paket-Manager-Konsole (PMC), den folgenden Befehl aus:

`Install-Package Microsoft.AspNet.WebApi.Client`

Der vorherige Befehl fügt die folgenden NuGet-Pakete dem Projekt hinzu:

* Microsoft.AspNet.WebApi.Client
* "Newtonsoft.JSON"

Json.NET ist ein gängiges Hochleistungs-JSON-Framework für .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Hinzufügen einer Modellklasse

Überprüfen Sie die `Product` Klasse:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Diese Klasse entspricht, das Datenmodell, die von der Web-API verwendet wird. Eine app kann verwenden **"HttpClient"** zum Lesen einer `Product` -Instanz anhand einer HTTP-Antwort. Die app keine Deserialisierungscode zu schreiben.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Erstellen und Initialisieren von "HttpClient"

Überprüfen Sie die statische **"HttpClient"** Eigenschaft:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**"HttpClient"** einmal instanziiert werden soll und während der Lebensdauer einer Anwendung wiederverwendet wird. Die folgenden Bedingungen können dazu führen, **SocketException** Fehler:

* Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung.
* Der Server stark ausgelastet ist.

Erstellen eines neuen **"HttpClient"** Instanz pro Anforderung kann die verfügbaren Sockets ausgeschöpft.

Der folgende code initialisiert die **"HttpClient"** Instanz:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Der vorangehende Code:

* Definiert die Basis-URI für HTTP-Anforderungen. Ändern Sie die Nummer des Ports, auf den Port, der in der Server-app verwendet. Die app funktioniert nicht, es sei denn, der Port für den Server-app verwendet wird.
* Legt den Accept-Header auf "Application/Json" fest. Dieser Header festlegen, weist den Server zum Senden von Daten im JSON-Format.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Senden Sie eine GET-Anforderung zum Abrufen einer Ressource

Der folgende Code sendet eine GET-Anforderung für ein Produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

Die **"getasync"** Methode sendet die HTTP-GET-Anforderung. Wenn die Methode abgeschlossen ist, wird ein **HttpResponseMessage** , die die HTTP-Antwort enthält. Wenn der Statuscode der Antwort einen Erfolgscode ist, enthält der Antworttext die JSON-Darstellung eines Produkts. Rufen Sie **ReadAsAsync** zum Deserialisieren der JSON-Nutzlast, die eine `Product` Instanz. Die **ReadAsAsync** -Methode asynchron ist, da der Antworttext beliebig groß sein kann.

**"HttpClient"** löst keine Ausnahme aus, wenn die HTTP-Antwort einen Fehlercode enthält. Stattdessen die **IsSuccessStatusCode** Eigenschaft **"false"** lautet der Status ein Fehlercode. Falls gewünscht, HTTP-Fehlercodes als Ausnahmen zu behandeln, rufen [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) auf das Response-Objekt. `EnsureSuccessStatusCode` löst eine Ausnahme aus, wenn der Statuscode außerhalb des Bereichs 200 liegt&ndash;299. Beachten Sie, dass **"HttpClient"** kann Ausnahmen auslösen, aus anderen Gründen &mdash; z. B., wenn die Anforderung ein Timeout auftritt.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Der Medientypformatierer, zu deserialisieren

Wenn **ReadAsAsync** wird aufgerufen, ohne Parameter verwendet eine Reihe von *medienformatierer* zum Lesen des Antworttexts. Die Standard-Formatierer unterstützt JSON, XML und Formular-Url-codierte Daten.

Anstatt die Standard-Formatierer verwenden, Sie können eine Liste der Formatierer zum Bereitstellen der **ReadAsAsync** Methode.  Verwenden eine Liste der Formatierer ist nützlich, wenn Sie eine benutzerdefinierte medientypformatierer haben:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Weitere Informationen finden Sie unter [Medienformatierer in der ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Senden eine POST-Anforderung zum Erstellen einer Ressource

Der folgende Code sendet eine POST-Anforderung, die enthält eine `Product` Instanz im JSON-Format:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

Die **PostAsJsonAsync** Methode:

* Serialisiert ein Objekt in JSON.
* Die JSON-Nutzlast sendet eine POST-Anforderung.

Wenn die Anforderung erfolgreich ist:

* Es sollte eine 201 (Created)-Antwort zurückgeben.
* Die Antwort sollte die URL der erstellten Ressourcen im Location-Header enthalten.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Senden von PUT-Anforderung zum Aktualisieren einer Ressource

Der folgende Code sendet eine PUT-Anforderung zum Aktualisieren eines Produkts:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

Die **PutAsJsonAsync** -Methode funktioniert wie **PostAsJsonAsync**, außer dass eine PUT-Anforderung anstelle von POST gesendet.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Senden einer DELETE-Anforderung zum Löschen einer Ressource

Der folgende Code sendet eine DELETE-Anforderung, um ein Produkt zu löschen:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Wie GET muss eine DELETE-Anforderung nicht über einen Anforderungstext. Sie müssen nicht JSON oder XML-Format mit DELETE angeben.

## <a name="test-the-sample"></a>Testen Sie das Beispiel

So testen Sie die Client-app:

1. [Herunterladen](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) , und führen Sie die Server-app. [Anweisungen zum Download.](/aspnet/core/tutorials/#how-to-download-a-sample) Stellen Sie sicher, dass die Server-app ausgeführt wird. Für Exaxmple `http://localhost:64195/api/products` sollte eine Liste von Produkten zurückgegeben.
2. Legen Sie die Basis-URI für HTTP-Anforderungen. Ändern Sie die Nummer des Ports, auf den Port, der in der Server-app verwendet.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Führen Sie die Client-app. Es wird die folgende Ausgabe generiert:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
