---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1 | Microsoft Docs"
author: MikeWasson
description: "Dieses Lernprogramm zeigt, wie zur Unterstützung von CRUD-Vorgänge in einen HTTP-Dienst mithilfe der ASP.NET Web API. Softwareversionen, in dem Lernprogramm Visual Studio 2012 Web Zugriffspunkt verwendet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Aktivieren von CRUD-Vorgänge in ASP.NET Web-API 1
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Dieses Lernprogramm zeigt, wie zur Unterstützung von CRUD-Vorgänge in einen HTTP-Dienst mithilfe der ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Visual Studio 2012
> - Web-API 1 (auch funktioniert mit Web-API 2)


CRUD steht für &quot;Create, Read, Update und "Delete"&quot; sind die vier grundlegenden Datenbankvorgängen. Viele HTTP-Diensten Modell auch CRUD-Vorgänge über REST oder REST-ähnliche-APIs.

In diesem Lernprogramm erstellen Sie eine sehr einfache Web-API zum Verwalten einer Liste von Produkten. Jedes Produkt enthält ein Name, Preis und die Kategorie (z. B. &quot;"Toy"&quot; oder &quot;Hardware&quot;), sowie eine Produkt-ID.

Die API-Produkte werden folgende Methoden verfügbar machen.

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | / api /-Produkte |
| Abrufen eines Produkts nach ID | GET | /API/Produkte/*Id* |
| Abrufen eines Produkts nach Kategorie | GET | Produkte/api /? Kategorie =*Kategorie* |
| Erstellen eines neuen Produkts | BEREITSTELLEN | / api /-Produkte |
| Aktualisieren eines Produkts | PUT | /API/Produkte/*Id* |
| Löschen eines Produkts | DELETE | /API/Produkte/*Id* |

Beachten Sie, dass einige der URIs die Produkt-ID im Pfad enthalten. Um das Produkt zu erhalten, deren ID 28 ist, beispielsweise sendet der Client eine GET-Anforderung für `http://hostname/api/products/28`.

### <a name="resources"></a>Ressourcen

Die Produkte API definiert URIs für zwei Ressourcentypen zur Verfügung:

| Ressource | URI |
| --- | --- |
| Die Liste aller Produkte. | / api /-Produkte |
| Ein einzelnes Produkt. | /API/Produkte/*Id* |

### <a name="methods"></a>Methoden

Die vier wichtigsten HTTP-Methoden (GET, PUT, POST und DELETE) können für CRUD-Vorgänge wie folgt zugeordnet:

- GET Ruft die Darstellung der Ressource an einem angegebenen URI ab. GET sollte auf dem Server keine Nebeneffekte haben.
- PUT aktualisiert eine Ressource an einem angegebenen URI. PUT kann auch zum Erstellen einer neuen Ressource an einem angegebenen URI verwendet werden, wenn der Server Clients an neue URIs zulässt. In diesem Lernprogramm wird die API Erstellung über PUT nicht unterstützt.
- POST erstellt eine neue Ressource. Der Server weist den URI für das neue Objekt und gibt diesen URI im Rahmen der Antwortnachricht zurück.
- DELETE Löscht eine Ressource an einem angegebenen URI.

Hinweis: Die PUT-Methode ersetzt die gesamte Product-Entität. Der Client muss, also eine vollständige Darstellung des aktualisierten Produkts zu senden. Wenn Sie teilweise Updates unterstützen möchten, ist die PATCH-Methode bevorzugt. Dieses Lernprogramm implementiert den PATCH nicht.

## <a name="create-a-new-web-api-project"></a>Erstellen Sie eine neue Web-API-Projekt

Starten Sie durch Ausführen von Visual Studio, und wählen Sie **neues Projekt** aus der **starten** Seite. Oder von der **Datei** klicken Sie im Menü **neu** und dann **Projekt**.

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten. Klicken Sie unter **Visual C#-**Option **Web**. Wählen Sie in der Liste der Projektvorlagen **ASP.NET MVC 4-Webanwendung**. Nennen Sie das Projekt &quot;ProductStore&quot; , und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Web-API** , und klicken Sie auf **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Hinzufügen eines Modells

Ein *Modell* ist ein Objekt, das die Daten in Ihrer Anwendung darstellt. In ASP.NET Web-API können Sie als Modelle stark typisierter CLR-Objekte, und sie werden automatisch in XML oder JSON serialisiert werden, für den Client.

Die ProductStore-API, besteht unsere Daten von Produkten, damit wir eine neue Klasse namens erstellen `Product`.

Wenn der Projektmappen-Explorer nicht sichtbar ist, klicken Sie auf die **Ansicht** Menü **Projektmappen-Explorer**. Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Modelle** Ordner. Wählen Sie aus dem Kontext Meny **hinzufügen**, und wählen Sie dann **Klasse**. Benennen Sie die Klasse &quot;Produkt&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Fügen Sie die folgenden Eigenschaften auf der `Product` Klasse.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Hinzufügen eines Repositorys

Speichern einer Sammlung von Produkten ist erforderlich. Es ist eine gute Idee, trennen Sie die Auflistung von unseren dienstimplementierung. Auf diese Weise können wir den Sicherungsspeicher ändern, ohne die Dienstklasse umzuschreiben. Diese Art von Design heißt die *Repository* Muster. Starten Sie durch die Definition einer generischen Schnittstelle für das Repository.

Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Modelle** Ordner. Wählen Sie **hinzufügen**, und wählen Sie dann **neues Element**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

In der **Vorlagen** klicken Sie im Bereich **installierte Vorlagen** und erweitern Sie den C#-Knoten. Wählen Sie unter C#-, **Code**. Wählen Sie in der Liste der Vorlagen für Code, **Schnittstelle**. Benennen Sie die Schnittstelle &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Fügen Sie die folgende Implementierung hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Fügen Sie einer anderen Klasse jetzt Ordner Models, mit dem Namen &quot;ProductRepository.&quot; Diese Klasse implementiert die `IProductRespository`-Schnittstelle. Fügen Sie die folgende Implementierung hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Das Repository behält die Liste im lokalen Speicher. Dies ist in Ordnung ein Lernprogramm, aber in einer echten Anwendung würden Sie extern können die Daten entweder einer Datenbank speichern oder im cloudspeicher. Das Repositorymuster wird so ändern Sie die Implementierung zu einem späteren Zeitpunkt erleichtern.

## <a name="adding-a-web-api-controller"></a>Hinzufügen eines Web-API-Controllers

Wenn Sie mit ASP.NET MVC gearbeitet haben, sind Sie bereits vertraut mit Domänencontrollern. In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen vom Client verarbeitet. Der neue Assistent für zwei Controller für Sie erstellt, bei der Erstellung des Projekts. Um diese anzuzeigen, erweitern Sie den Ordner "Controller" im Projektmappen-Explorer aus.

- HomeController ist eine herkömmliche ASP.NET MVC-Controllers. Er ist verantwortlich für das Bereitstellen von HTML-Seiten für die Website, und nicht direkt mit unserer Web-API verknüpft ist.
- ValuesController ist eine Beispiel-WebAPI-Controller.

Fahren Sie fort, und Löschen von ValuesController, indem Sie die Datei im Projektmappen-Explorer mit der rechten Maustaste und auswählen **löschen.** Nun fügen Sie einen neuen Domänencontroller wie folgt:

In **Projektmappen-Explorer**, mit der rechten Maustaste den Ordner. Wählen Sie **hinzufügen** und wählen Sie dann **Controller**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

In der **Controller hinzufügen** Assistenten, der Name des Controllers &quot;ProductsController&quot;. In der **Vorlage** Dropdown-Liste **leerer API-Controller**. Klicken Sie anschließend auf **Hinzufügen**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Es ist nicht notwendig, einen Ordner namens Controller Ihrer Controllern abgelegt. Der Name des Ordners spielt keine Rolle; Es ist einfach eine einfache Möglichkeit, Ihre Quelldateien zu organisieren.


Die **Controller hinzufügen** Assistenten wird eine Datei namens ProductsController.cs im Ordner "Controller" erstellt. Wenn diese Datei noch nicht geöffnet ist, doppelklicken Sie auf die Datei, um ihn zu öffnen. Fügen Sie die folgenden **mit** Anweisung:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Hinzufügen eines Felds, der enthält einem **IProductRepository** Instanz.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Aufrufen von `new ProductRepository()` im Controller ist nicht zum optimale Entwurf, da es den Controller aus, um eine bestimmte Implementierung des bindet `IProductRepository`. Ein besserer Ansatz finden Sie unter [mithilfe der Web-API-Abhängigkeitskonfliktlöser](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Abrufen einer Ressourcenpools

Die ProductStore-API macht mehrere &quot;lesen&quot; Aktionen als HTTP GET-Methoden. Jede Aktion wird eine Methode in entsprechen den `ProductsController` Klasse.

| Aktion | HTTP-Methode | Relativer URI |
| --- | --- | --- |
| Abrufen einer Liste aller Produkte | GET | / api /-Produkte |
| Abrufen eines Produkts nach ID | GET | /API/Produkte/*Id* |
| Abrufen eines Produkts nach Kategorie | GET | Produkte/api /? Kategorie =*Kategorie* |

Um die Liste aller Produkte abzurufen, fügen Sie diese Methode, um die `ProductsController` Klasse:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Der Name der Methode beginnt mit &quot;abrufen&quot;, so, dass gemäß der Konvention sie GET-Anforderungen zugeordnet. Ordnet auch dann, da die Methode keine Parameter hat, es ein URI, der keine enthält ein  *&quot;Id&quot;*  Segment im Pfad.

Um ein Produkt nach ID zu erhalten, fügen Sie diese Methode, um die `ProductsController` Klasse:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Dieser Methodenname auch beginnt mit &quot;abrufen&quot;, aber die Methode verfügt über einen Parameter namens *Id*. Dieser Parameter zugeordnet ist die &quot;Id&quot; Segment des URI-Pfads. Der ASP.NET Web API-Framework konvertiert automatisch die ID in den richtigen Datentyp (**Int**) für den Parameter.

Die GetProduct Methode löst eine Ausnahme vom Typ **HttpResponseException** Wenn *Id* ist ungültig. Diese Ausnahme wird vom Framework in Fehler 404 (Nichtgefunden) übersetzt werden.

Abschließend fügen Sie eine Methode zum Suchen nach Produkten nach Kategorie hinzu:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Wenn im Anforderungs-URI eine Abfragezeichenfolge enthält, versucht Web-API ab, die Abfrageparameter an einen Parameter in der Controllermethode übereinstimmen. Deshalb einen URI im Format "api-Produkte? Kategorie =*Kategorie*" werden an diese Methode zugeordnet.

## <a name="creating-a-resource"></a>Erstellen einer Ressource

Als Nächstes fügen wir eine Methode, um die `ProductsController` Klasse, um ein neues Produkt zu erstellen. Hier ist eine einfache Implementierung der Methode:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Beachten Sie zwei Dinge zu dieser Methode ein:

- Der Name der Methode beginnt mit &quot;Post... &quot;. So erstellen ein neues Produkt, sendet der Client eine HTTP POST-Anforderung.
- Die Methode nimmt einen Parameter vom Typ Produkt. In der Web-API werden die Parameter mit komplexen Typen aus dem Anforderungstext deserialisiert. Daher erwarten wir den Client eine serialisierte Darstellung eines Objekts Produkt in XML oder JSON-Format senden.

Diese Implementierung funktioniert, aber es ist nicht ganz abgeschlossen. Im Idealfall möchten wir die HTTP-Antwort, die Folgendes umfassen:

- **Antwortcode:** Standardmäßig legt der Web-API-Framework Statuscode der Antwort 200 (OK). Jedoch gemäß der HTTP/1.1-Protokoll, wenn bei der Erstellung einer Ressource, eine POST-Anforderung führt der Server sollten Antworten mit dem Status 201 (erstellt).
- **Ort:** , wenn der Server eine Ressource erstellt, sollte er den URI der neuen Ressource im Location-Header der Antwort enthalten.

ASP.NET Web API erleichtert die Bearbeitung der HTTP-Antwort-Nachricht. So sieht die verbesserte Implementierung aus:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Beachten Sie, dass der Rückgabetyp der Methode jetzt **HttpResponseMessage**. Durch das Zurückgeben einer **HttpResponseMessage** anstelle eines Produkts können zu steuern, die Details der HTTP-Antwortnachricht, einschließlich der Statuscode und den Location-Header.

Die **CreateResponse** Methode erstellt ein **HttpResponseMessage** und schreibt Sie eine serialisierte Darstellung des Objekts Produkt automatisch in den Textkörper fo der Antwortnachricht.

> [!NOTE]
> In diesem Beispiel führt keine Überprüfung der `Product`. Informationen zur modellvalidierung finden Sie unter [Modellvalidierung in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualisieren einer Ressource

Aktualisieren eines Produkts bei PUT ist einfach:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Der Name der Methode beginnt mit &quot;einfügen... &quot;, sodass es Web-API für PUT-Anforderungen entspricht. Die Methode akzeptiert zwei Parameter, die Produkt-ID und des aktualisierten Produkts. Die *Id* Parameter stammt aus dem URI-Pfad und der *Produkt* Parameter aus dem Anforderungstext deserialisiert wird. Standardmäßig führt der ASP.NET Web API-Framework einfache Parametertypen aus der Route und komplexe Typen aus dem Anforderungstext.

## <a name="deleting-a-resource"></a>Beim Löschen einer Ressource

Definieren Sie eine "Löschen..."-Methode, um eine Ressourcen zu löschen.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Wenn eine DELETE-Anforderung erfolgreich ist, können sie Statusinformationen 200 (OK) mit einem Entitätstext zurückgeben, die den Status beschreibt; Status 202 (akzeptiert), wenn der Löschvorgang noch ausstehende; oder der Status 204 (kein Inhalt) ohne Text Entität. In diesem Fall die `DeleteProduct` Methode verfügt über eine `void` Typ zurückgeben, damit ASP.NET Web API dies automatisch in den Status der übersetzt code 204 (kein Inhalt).
