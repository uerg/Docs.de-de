---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines OData-Diensts aus einem .NET-Client (c#) | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Tutorial zeigt, wie auf einen OData-Dienst über eine C#-Client-Anwendung aufgerufen wird. Softwareversionen, die verwendet werden, in dem Tutorial Visual Studio 2013 (kann mit Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 75f8e3eab7bd5667bbdcccbb5ae8a8e5b1f5fdba
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912051"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Aufrufen eines OData-Diensts aus einem .NET-Client (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Dieses Tutorial zeigt, wie auf einen OData-Dienst über eine C#-Client-Anwendung aufgerufen wird.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013) (funktioniert mit Visual Studio 2012)
> - [WCF Data Services-Clientbibliothek](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web-API 2. (Im Beispiel OData-Dienst wird mithilfe von Web-API 2 erstellt, die Client-Anwendung ist jedoch nicht erforderlich für Web-API.)


In diesem Tutorial erläutere ich, wie Sie erstellen eine Clientanwendung, die einen OData-Dienst aufruft. Der OData-Dienst stellt die folgenden Entitäten:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

In den folgenden Artikeln wird beschrieben, wie den OData-Dienst in Web-API zu implementieren. (Sie müssen nicht gelesen werden, damit dieses Lernprogramm jedoch klar ist.)

- [Erstellen eines OData-Endpunkts in Web-API 2](creating-an-odata-endpoint.md)
- [OData-Entität-Beziehungen in der Web-API 2](working-with-entity-relations.md)
- [OData-Aktionen in der Web-API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generieren des Webdienstproxys

Der erste Schritt ist einen neuer Proxy zu generieren. Der-Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Zunächst öffnen das OData-Dienst-Projekt in Visual Studio. Drücken Sie STRG + F5, um den Dienst lokal in IIS Express ausführen. Beachten Sie die lokale Adresse, einschließlich der Portnummer, die von Visual Studio zugewiesen. Sie benötigen diese Adresse, wenn Sie den Proxy erstellen.

Als Nächstes öffnen Sie eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolenanwendungsprojekt. Die Konsolenanwendung werden die OData-Client-Anwendung. (Sie können auch das Projekt der gleichen Projektmappe wie der Dienst hinzufügen.)

> [!NOTE]
> Die verbleibenden Schritte finden Sie das Konsolen-Projekt.


Klicken Sie im Projektmappen-Explorer mit der Maustaste **Verweise** , und wählen Sie **Hinzufügen eines Dienstverweises**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

In der **Hinzufügen eines Dienstverweises** Dialogfeld Geben Sie die Adresse des OData-Diensts:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

wo *Port* gibt die Portnummer an.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Für **Namespace**, geben Sie "ProductService". Diese Option definiert den Namespace der Proxyklasse.

Klicken Sie auf **Go**. Visual Studio liest Metadatendokument des OData, um die Entitäten in den Dienst zu ermitteln.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Klicken Sie auf **OK** die Proxyklasse zu Ihrem Projekt hinzuzufügen.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Erstellen Sie eine Instanz der Proxyklasse Service

Innerhalb Ihrer `Main` -Methode, erstellen Sie eine neue Instanz der Proxyklasse, wie folgt:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

In diesem Fall verwenden Sie die tatsächliche Portnummer an, in dem der Dienst ausgeführt wird. Wenn Sie Ihren Dienst bereitstellen, verwenden Sie den URI des live-Diensts. Sie müssen nicht den Proxy zu aktualisieren.

Der folgende Code Fügt einen Ereignishandler, der die Anforderungs-URIs in das Konsolenfenster ausgibt. Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage finden Sie unter.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Abfragen des Diensts

Der folgende Code Ruft die Liste der Produkte ab, aus dem OData-Dienst.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Beachten Sie, dass Sie nicht zum Schreiben von Code zum Senden der HTTP-Anforderung oder Antwort analysiert werden müssen. Die Proxyklasse wird automatisch für die Sie auflisten, wenn die `Container.Products` Sammlung in der **Foreach** Schleife.

Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Verwenden Sie zum Abrufen einer Entität nach der ID einer `where` Klausel.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Für den Rest dieses Themas, ich nicht die gesamte angezeigt `Main` Funktion, den Code zum Aufrufen des Diensts erforderlich sind.

## <a name="apply-query-options"></a>Anwenden von Abfrageoptionen

OData definiert [Abfrageoptionen](../supporting-odata-query-options.md) , die zum Filtern, sortieren, Seitendaten und So weiter verwendet werden kann. In der Proxyklasse können Sie diese Optionen anwenden, mit verschiedenen LINQ-Ausdrücke.

In diesem Abschnitt zeige ich, kurze Beispiele für. Weitere Informationen finden Sie im Thema [Überlegungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.

### <a name="filtering-filter"></a>Filtern ($filter)

Verwenden Sie zum Filtern einer `where` Klausel. Das folgende Beispiel filtert nach Produktkategorie.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Dieser Code entspricht der folgenden OData-Abfrage.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Beachten Sie, die der Proxy konvertiert die `where` Klausel für einen OData- `$filter` Ausdruck.

### <a name="sorting-orderby"></a>Sortierung ($orderby)

Verwenden Sie zum Sortieren einer `orderby` Klausel. Im folgenden Beispiel wird sortiert nach Preis, vom höchsten zum niedrigsten.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Hier ist die entsprechenden OData-Anforderung.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Clientseitige Auslagerung ("$skip" und "$top")

Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu beschränken. Beispielsweise kann ein Client 10 Einträge zu einem Zeitpunkt anzeigen. Dies wird als bezeichnet *clientseitigen Auslagerung*. (Es gibt auch [serverseitiges Paging](../supporting-odata-query-options.md#server-paging), in denen der Server die Anzahl der Ergebnisse beschränkt.) Verwenden Sie zum Ausführen der clientseitigen Auslagerung LINQ **überspringen** und **dauern** Methoden. Im folgenden Beispiel überspringt die ersten 40 Ergebnisse und die nächsten 10 verwendet.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

So sieht die entsprechenden OData-Anforderung aus:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Option ($select), und erweitern ($expand)

Um verknüpfte Entitäten einzuschließen, verwenden die `DataServiceQuery<t>.Expand` Methode. Beispielsweise enthalten die `Supplier` für jede `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

So sieht die entsprechenden OData-Anforderung aus:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Um die Form der Antwort zu ändern, verwenden Sie LINQ **wählen** Klausel. Im folgenden Beispiel wird nur der Name jedes Produkts, ohne andere Eigenschaften.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

So sieht die entsprechenden OData-Anforderung aus:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Eine select-Klausel kann es sich um verknüpfte Entitäten enthalten. Rufen Sie in diesem Fall nicht **erweitern**; der Proxy schließt automatisch die Erweiterung in diesem Fall. Im folgende Beispiel ruft den Namen und den Lieferanten für jedes Produkt ab.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Hier ist die entsprechenden OData-Anforderung. Beachten Sie, die es enthält die **$expand-** Option.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Weitere Informationen zu $select und $expand erweitern, finden Sie unter [verwenden $select, $expand, und $value in Web-API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Eine neue Entität hinzufügen

Um eine neue Entität für eine entitätsmenge hinzuzufügen, rufen `AddToEntitySet`, wobei *EntitySet* ist der Name der Entitätenmenge. Z. B. `AddToProducts` Fügt ein neues `Product` auf die `Products` Entitätenmenge. Wenn Sie den Proxy zu generieren, WCF Data Services erstellt automatisch diese stark typisierte **AddTo** Methoden.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Verwenden Sie zum Hinzufügen einer Verknüpfung zwischen zwei Entitäten die **AddLink** und **SetLink** Methoden. Der folgende Code Fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen diesen beiden.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Verwendung **AddLink** bei die Navigationseigenschaft eine Auflistung ist. In diesem Beispiel werden wir ein Produkt hinzufügen der `Products` Auflistung auf den Lieferanten.

Verwendung **SetLink** bei die Navigationseigenschaft eine einzelne Entität ist. In diesem Beispiel legen wir die `Supplier` Eigenschaft für das Produkt.

## <a name="update--patch"></a>Aktualisieren und Patchen

Rufen Sie zum Aktualisieren einer Entitätstyps die **UpdateObject** Methode.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Die Aktualisierung wird ausgeführt, wenn Sie aufrufen **"SaveChanges"**. Standardmäßig sendet WCF eine HTTP MERGE-Anforderung. Die **PatchOnUpdate** -Option weist WCF an, die eine HTTP-PATCH stattdessen senden.

> [!NOTE]
> Warum im Vergleich zu MERGE PATCH? Die ursprüngliche HTTP 1.1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) keiner HTTP-Methode, mit der Semantik von "partielle Aktualisierung" definieren. Die OData-Spezifikation definiert die MERGE-Methode, um teilupdates zu unterstützen. In 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definiert die PATCH-Methode für die partielle Aktualisierungen. Finden Sie einige der in diesem Verlauf [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) auf die WCF Data Services-Blog. PATCH wird heute über MERGE bevorzugt. Die von der Web-API-Gerüstbau erstellten OData-Controller unterstützt beide Methoden.


Wenn Sie die gesamte Entität (PUT-Semantik) ersetzen möchten, geben Sie die **ReplaceOnUpdate** Option. Dies bewirkt, dass WCF eine HTTP PUT-Anforderung senden.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Löschen einer Entität

Um eine Entität zu löschen, rufen **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Rufen Sie eine OData-Aktion

In OData [Aktionen](odata-actions.md) sind eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.

Obwohl Metadatendokument des OData, die Aktionen beschrieben, erstellt die Proxyklasse keine stark typisierte Methoden keine für sie. Sie können immer noch eine OData-Aktion aufrufen, über die generische **Execute** Methode. Allerdings müssen Sie wissen, die Datentypen der Parameter und der zurückgegebene Wert.

Z. B. die `RateProduct` Parameter mit dem Namen "Rating" vom Typ "Aktion" `Int32` und gibt eine `double`. Der folgende Code zeigt, wie Sie diese Aktion aufrufen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Weitere Informationen finden Sie unter[Aufrufen von Dienstvorgängen und-Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).

Eine Möglichkeit ist, erweitern Sie die **Container** Klasse, um eine stark typisierte Methode bereitzustellen, die die Aktion aufruft:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
