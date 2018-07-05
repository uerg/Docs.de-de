---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial wird gezeigt, wie CRUD-Vorgänge in einen HTTP-Dienst über ASP.NET Web-API unterstützt. Die Softwareversionen, die in den Tutorials Visual Studio 2012 Web Zugriffspunkt verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402927"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> In diesem Tutorial wird gezeigt, wie CRUD-Vorgänge in einen HTTP-Dienst über ASP.NET Web-API unterstützt.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Visual Studio 2012
> - Web-API 1 (Außerdem funktioniert mit Web-API 2)


CRUD steht für &quot;erstellen, lesen, aktualisieren und löschen,&quot; die sind die vier grundlegenden Datenbankvorgänge. Viele HTTP-Dienste das Modell auch CRUD-Vorgänge über REST oder die REST-ähnliche APIs.

In diesem Tutorial erstellen Sie ein sehr einfaches Web-API, um eine Liste von Produkten zu verwalten. Jedes Produkt, Preis, und Auswählen einer Kategorie enthält (z. B. &quot;Toys&quot; oder &quot;Hardware&quot;), sowie eine Produkt-ID.

Die API-Produkte werden folgende Methoden verfügbar machen.

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | /api/products |
| Abrufen eines Produkts nach ID | GET | /api/products/*id* |
| Abrufen eines Produkts nach Kategorie | GET | /api/products?category=*category* |
| Erstellen eines neuen Produkts | POST | /api/products |
| Aktualisieren eines Produkts | PUT | /api/products/*id* |
| Löschen eines Produkts | DELETE | /api/products/*id* |

Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten. Um das Produkt zu erhalten, deren ID ist die 28, z. B. sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.

### <a name="resources"></a>Ressourcen

Die API-Produkte werden URIs für zwei Ressourcentypen definiert:

| Ressource | URI |
| --- | --- |
| Die Liste aller Produkte. | /api/products |
| Ein einzelnes Produkt. | /api/products/*id* |

### <a name="methods"></a>Methoden

Die vier wichtigsten HTTP-Methoden (GET, PUT, POST und DELETE) können die CRUD-Vorgängen wie folgt zugeordnet:

- GET Ruft die Darstellung der Ressource am angegebenen URI ab. GET, sollte auf dem Server keine Nebeneffekte haben.
- PUT aktualisiert eine Ressource auf einem angegebenen URI. PUT kann auch verwendet werden, um eine neue Ressource am angegebenen URI, zu erstellen, wenn der Server, Clients an neue URIs zulässt. In diesem Tutorial wird in die API-Erstellung über PUT nicht unterstützt.
- POST wird eine neue Ressource erstellt. Der Server weist den URI für das neue Objekt und gibt diesen URI als Teil der Antwortnachricht zurück.
- DELETE Löscht eine Ressource auf einem angegebenen URI.

Hinweis: Die PUT-Methode ersetzt die gesamte Product-Entität. Der Client soll, also eine vollständige Darstellung des aktualisierten Produkts zu senden. Wenn Sie möchten die teilupdates zu unterstützen, wird die PATCH-Methode bevorzugt. In diesem Tutorial implementiert PATCH nicht.

## <a name="create-a-new-web-api-project"></a>Erstellen eines neuen Web-API-Projekts

Starten, indem Sie Ausführung von Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Alternativ wählen Sie in der **Datei** , wählen Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-** Option **Web**. Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt &quot;ProductStore&quot; , und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Web-API-** , und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Hinzufügen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. In ASP.NET Web-API können Sie stark typisierte CLR-Objekte als Modelle verwenden, und sie werden automatisch in XML oder JSON serialisiert werden, für den Client.

Für die API ProductStore, besteht unsere Daten von Produkten, daher wir eine neue Klasse namens erstellen `Product`.

Wenn der Projektmappen-Explorer nicht angezeigt wird, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**. Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner. Wählen Sie die Kontext-Meny **hinzufügen**, und wählen Sie dann **Klasse**. Nennen Sie die Klasse &quot;Produkt&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Hinzufügen eines Repositorys

Wir müssen eine Sammlung von Produkten zu speichern. Es ist eine gute Idee, trennen Sie die Sammlung von unsere dienstimplementierung. Auf diese Weise können wir Sicherungsspeicher ändern, ohne die Dienstklasse umzuschreiben. Diese Art von Design wird aufgerufen, die *Repository* Muster. Starten Sie durch die Definition einer generischen Schnittstelle für das Repository.

Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Modelle** Ordner. Wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie den C#-Knoten. Wählen Sie unter c#, **Code**. Wählen Sie in der Liste der Codevorlagen, **Schnittstelle**. Benennen Sie die Schnittstelle &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Fügen Sie die folgende Implementierung:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Fügen Sie eine andere Klasse jetzt im Ordner "Models", mit dem Namen &quot;ProductRepository.&quot; Diese Klasse implementiert die `IProductRespository`-Schnittstelle. Fügen Sie die folgende Implementierung:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Das Repository speichert die Liste im lokalen Speicher. Dies ist in Ordnung ein Tutorial, aber in einer echten Anwendung würden Sie die Daten extern, entweder eine Datenbank speichern oder in einem cloudspeicher. Das Repository-Muster wird so ändern Sie die Implementierung zu einem späteren Zeitpunkt erleichtern.

## <a name="adding-a-web-api-controller"></a>Hinzufügen eines Web-API-Controllers

Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits vertraut mit Controllern. In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen vom Client verarbeitet. Der Assistent für neues Projekt für Sie zwei Controller erstellt werden, wenn es sich bei der Erstellung des Projekts. Um anzuzeigen, erweitern Sie im Projektmappen-Explorer den Ordner "Controllers" aus.

- HomeController ist eine herkömmliche ASP.NET MVC-Controller. Es wird dient zum Bereitstellen von HTML-Seiten für den Standort, und nicht direkt an unsere Web-API.
- ValuesController ist eine Beispiel-Web-API-Controller.

Löschen Sie ValuesController, indem Sie mit der rechten Maustaste in der Datei im Projektmappen-Explorer und auswählen **löschen.** Fügen Sie einen neuen Controller jetzt wie folgt:

In **Projektmappen-Explorer**, mit der rechten Maustaste in den Ordner "Controllers". Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

In der **Controller hinzufügen** Assistenten benennen Sie den Controller &quot;ProductsController&quot;. In der **Vorlage** Dropdown-Liste **leerer API-Controller**. Klicken Sie anschließend auf **Hinzufügen**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Es ist nicht notwendig, einen Ordner Controller Namens Ihrer Serviceprojekt abgelegt. Der Name des Ordners ist nicht wichtig; Es ist lediglich eine bequeme Möglichkeit, Ihre Quelldateien zu organisieren.


Die **Controller hinzufügen** Assistent erstellt eine Datei namens ProductsController.cs im Ordner "Controllers". Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen. Fügen Sie die folgenden **mit** Anweisung:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Hinzufügen eines Felds, das enthält ein **IProductRepository** Instanz.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Aufrufen von `new ProductRepository()` im Controller ist nicht der optimale Entwurf, da es den Controller eine bestimmte Implementierung bindet `IProductRepository`. Ein besserer Ansatz ist, finden Sie unter [mithilfe der Web-API-Abhängigkeitskonfliktlöser](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Abrufen einer Ressourcensatzes

Die ProductStore-API macht mehrere &quot;lesen&quot; Aktionen als HTTP GET-Methoden. Jede Aktion entspricht einer Methode in der `ProductsController` Klasse.

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | /api/products |
| Abrufen eines Produkts nach ID | GET | /api/products/*id* |
| Abrufen eines Produkts nach Kategorie | GET | /api/products?category=*category* |

Um die Liste aller Produkte zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Der Name der Methode beginnt mit &quot;erhalten&quot;, so, dass gemäß der Konvention sie GET-Anforderungen zugeordnet. Darüber hinaus, da die Methode keine Parameter enthält, entspricht dem ein URI, der keine enthält ein *&quot;Id&quot;* Segment im Pfad.

Um ein Produkt nach ID zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Dieser Methodenname beginnt auch mit &quot;erhalten&quot;, aber die Methode weist einen Parameter namens *Id*. Dieser Parameter zugeordnet ist die &quot;Id&quot; Segment des URI-Pfads. Das ASP.NET Web-API-Framework konvertiert automatisch die ID in den richtigen Datentyp (**Int**) für den Parameter.

Die GetProduct-Methode löst eine Ausnahme vom Typ **HttpResponseException** Wenn *Id* ist ungültig. Diese Ausnahme wird vom Framework in den Fehler 404 (nicht gefunden) übersetzt.

Abschließend fügen Sie eine Methode, um die Produkte nach Kategorie zu suchen:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Wenn der Anforderungs-URI eine Abfragezeichenfolge enthält, versucht Web-API ab, die Abfrageparameter an einen Parameter in der Controllermethode übereinstimmen. Aus diesem Grund einen URI im Format "api/Produkte? Kategorie =*Kategorie*" an diese Methode zugeordnet wird.

## <a name="creating-a-resource"></a>Erstellen einer Ressource

Als Nächstes fügen wir eine Methode, um die `ProductsController` Klasse zum Erstellen eines neuen Produkts. Hier ist eine einfache Implementierung der Methode ein:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Beachten Sie diese Methode zwei Dinge:

- Der Name der Methode beginnt mit &quot;Post... &quot;. Um ein neues Produkt zu erstellen, sendet der Client eine HTTP POST-Anforderung.
- Die Methode nimmt einen Parameter vom Typ Produkt. In der Web-API werden die Parameter mit komplexen Typen aus dem Anforderungstext deserialisiert. Daher erwarten wir den Client um eine serialisierte Darstellung des ein Product-Objekt, in XML oder JSON-Format zu senden.

Diese Implementierung funktioniert, aber es ist leider unvollständig. Wir möchten im Idealfall HTTP-Antwort auf die Folgendes umfassen:

- **Antwortcode:** Standardmäßig legt der Web-API-Framework Statuscode der Antwort 200 (OK). Doch gemäß der HTTP/1.1-Protokoll, wenn eine POST-Anforderung bei der Erstellung einer Ressource, führt die sollte Serverantwort mit dem Status 201 (erstellt).
- **Speicherort:** , wenn der Server eine Ressource erstellt, sollte es den URI der neuen Ressource im Location-Header der Antwort enthalten.

ASP.NET Web-API erleichtert die Bearbeitung von HTTP-Antwortnachricht. Dies ist die verbesserte Implementierung ein:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Beachten Sie, dass der Rückgabetyp der Methode jetzt **HttpResponseMessage**. Durch Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts, wir können die Details der HTTP-Antwortnachricht, einschließlich der Statuscode und den Location-Header steuern.

Die **CreateResponse** -Methode erstellt eine **HttpResponseMessage** und schreibt Sie automatisch eine serialisierte Darstellung der Product-Objekt in Text fo die Response-Nachricht.

> [!NOTE]
> In diesem Beispiel überprüft nicht die `Product`. Weitere Informationen zur modellvalidierung finden Sie unter [Model Validation in ASP.NET Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualisieren einer Ressource

Aktualisieren eines Produkts bei Verwendung von PUT ist einfach:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Der Name der Methode beginnt mit &quot;einfügen... &quot;, sodass es Web-API-PUT-Anforderungen entspricht. Die Methode nimmt zwei Parameter, die Produkt-ID und des aktualisierten Produkts. Die *Id* Parameter stammt aus dem URI-Pfad, und die *Produkt* Parameter aus dem Anforderungstext deserialisiert wird. Standardmäßig führt das ASP.NET Web-API-Framework einfache Parametertypen aus der Route und komplexe Typen aus dem Anforderungstext.

## <a name="deleting-a-resource"></a>Löschen einer Ressource

Definieren Sie eine "..." löschen"-Methode, um eine Ressourcen zu löschen.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Wenn eine DELETE-Anforderung erfolgreich ist, kann es Status 200 (OK) mit einem Entity-Body zurückgegeben wird, die den Status beschreibt. Status 202 (akzeptiert), wenn der Löschvorgang noch ausstehende; oder der Status 204 (kein Inhalt) ohne Text für die Entität. In diesem Fall die `DeleteProduct` Methode verfügt über eine `void` Typ zurückgeben, sodass ASP.NET Web-API diese automatisch in den Status der übersetzt code 204 (No Content).
