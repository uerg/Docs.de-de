---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines OData-Diensts aus einem .NET-Client (c#) | Microsoft Docs
author: MikeWasson
description: Dieses Lernprogramm zeigt, wie auf einen OData-Dienst aus einer C#-Client-Anwendung aufzurufen. Die Versionen der Software verwendet werden, in dem Lernprogramm Visual Studio 2013 (kann mit Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a>Aufrufen eines OData-Diensts aus einem .NET-Client (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Dieses Lernprogramm zeigt, wie auf einen OData-Dienst aus einer C#-Client-Anwendung aufzurufen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert mit Visual Studio 2012)
> - [WCF Data Services-Clientbibliothek](https://msdn.microsoft.com/library/cc668772.aspx)
> - Web API 2. (Im Beispiel OData-Dienst wird mithilfe von Web-API 2 erstellt, aber die Clientanwendung hängt nicht von Web-API.)


In diesem Lernprogramm durchgehen ich Erstellen einer Clientanwendung, die einen OData-Dienst aufruft. Die OData-Dienst macht die folgenden Elemente:

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

In den folgenden Artikeln wird beschrieben, wie die OData-Dienst im Web-API implementiert. (Sie müssen nicht gelesen werden, um dieses Lernprogramm jedoch zu verstehen.)

- [Erstellen einen OData-Endpunkt in der Web-API 2](creating-an-odata-endpoint.md)
- [OData-Entitätsbeziehungen in Web-API 2](working-with-entity-relations.md)
- [OData-Aktionen in der Web-API 2](odata-actions.md)

## <a name="generate-the-service-proxy"></a>Generieren des Webdienstproxys

Der erste Schritt besteht darin einen neuer Proxy zu generieren. Der Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

Öffnen das OData-Dienst-Projekt in Visual Studio starten. Drücken Sie STRG + F5, um den Dienst in IIS Express lokal auszuführen. Beachten Sie die lokale Adresse, einschließlich der Portnummer, die Visual Studio zuweist. Sie benötigen diese Adresse, wenn Sie den Proxy erstellt.

Als Nächstes öffnen Sie eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolenanwendungsprojekt. Die Konsolenanwendung werden die OData-Clientanwendung. (Sie können auch das Projekt der gleichen Lösung wie der Dienst hinzufügen.)

> [!NOTE]
> Die restlichen Schritte finden Sie unter der Konsolenprojekt.


Klicken Sie im Projektmappen-Explorer mit der Maustaste **Verweise** , und wählen Sie **Hinzufügen eines Dienstverweises**.

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

In der **Hinzufügen eines Dienstverweises** Dialogfeld, geben Sie die Adresse des OData-Diensts:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

wobei *Port* ist die Portnummer.

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

Für **Namespace**, geben Sie "ProductService". Diese Option wird den Namespace der Proxyklasse definiert.

Klicken Sie auf **Go**. Visual Studio liest die OData-Metadatendokument um die Entitäten im Dienst zu ermitteln.

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

Klicken Sie auf **OK** die Proxyklasse zu Ihrem Projekt hinzufügen.

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a>Erstellen Sie eine Instanz der Proxyklasse Service

Innerhalb der `Main` -Methode, erstellen Sie eine neue Instanz der Proxyklasse, wie folgt:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

Erneut, verwenden Sie die Anzahl der tatsächlich verwendeten Port, auf dem der Dienst ausgeführt wird. Wenn Sie den Dienst bereitstellen, verwenden Sie den URI des live-Diensts. Sie müssen nicht den Proxy zu aktualisieren.

Der folgende Code Fügt einen Ereignishandler, der die Anforderungs-URIs an das Konsolenfenster ausgibt. Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage finden Sie unter.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a>Abfragen des Diensts

Der folgende Code Ruft die Liste der Produkte aus der OData-Dienst ab.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

Beachten Sie, dass Sie nicht zum Senden von HTTP-Anforderung oder Antwort analysieren Code schreiben müssen. Die Proxy-Klasse wird automatisch für die Sie auflisten, wenn die `Container.Products` Sammlung in der **Foreach** Schleife.

Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

Verwenden Sie zum Abrufen einer Entität nach der ID einer `where` Klausel.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

Für den Rest dieses Themas, ich zeigen nicht an die gesamte `Main` -Funktion, den Code zum Aufrufen des Diensts erforderlich sind.

## <a name="apply-query-options"></a>Anwenden von Abfrageoptionen

OData definiert [Abfrageoptionen](../supporting-odata-query-options.md) , der zum Filtern, sortieren, Seitendaten usw. verwendet werden können. In den Dienstproxy können Sie diese Optionen anwenden, mit verschiedenen LINQ-Ausdrücke.

In diesem Abschnitt zeige ich kurze Beispiele. Weitere Informationen finden Sie im Thema [Überlegungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.

### <a name="filtering-filter"></a>Filtern ($filter)

Verwenden Sie zum Filtern einer `where` Klausel. Im folgenden Beispiel wird gefiltert nach Produktkategorie.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

Dieser Code entspricht den folgenden OData-Abfrage.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

Beachten Sie, die der Proxy konvertiert die `where` Klausel in einer OData `$filter` Ausdruck.

### <a name="sorting-orderby"></a>Sortierung ($orderby)

Verwenden Sie zum Sortieren einer `orderby` Klausel. Im folgende Beispiel wird nach dem Preis, vom höchsten zum niedrigsten sortiert.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

Im folgenden wird die entsprechende OData-Anforderung.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a>Clientseitige Auslagerung ("$skip" und "$top")

Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu begrenzen. Beispielsweise kann ein Client 10 Einträge zu einem Zeitpunkt angezeigt. Hierbei spricht *clientseitigen Auslagerung*. (Es gibt auch [serverseitiges Paging](../supporting-odata-query-options.md#server-paging), in denen der Server die Anzahl der Ergebnisse beschränkt.) Verwenden Sie zum Ausführen einer clientseitigen Auslagerung LINQ **Skip** und **dauern** Methoden. Im folgenden Beispiel überspringt die ersten 40 Ergebnisse und die nächsten 10 akzeptiert.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

Hier wird die entsprechende OData-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a>Auswählen ($select), und erweitern Sie diese Option (expand, $)

Um verknüpfte Entitäten einzuschließen, verwenden die `DataServiceQuery<t>.Expand` Methode. Beispielsweise enthalten die `Supplier` für jede `Product`:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

Hier wird die entsprechende OData-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

Um die Form der Antwort zu ändern, verwenden Sie die LINQ **wählen** Klausel. Im folgenden Beispiel wird nur der Name jedes Produkts, ohne andere Eigenschaften.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

Hier wird die entsprechende OData-Anforderung:

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

Eine select-Klausel kann es sich um verknüpfte Entitäten enthalten. Rufen Sie in diesem Fall nicht **erweitern**; der Proxy schließt automatisch die Erweiterung in diesem Fall. Das folgende Beispiel ruft den Namen und den Hersteller der einzelnen Produkte.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

Im folgenden wird die entsprechende OData-Anforderung. Beachten Sie, die dieses enthält die **$expand-** Option.

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

Weitere Informationen zu $select und $erweitern, finden Sie unter [mit $select, $expand, und $value in Web-API 2](../using-select-expand-and-value.md).

## <a name="add-a-new-entity"></a>Fügen Sie eine neue Entität hinzu.

Um eine neue Entität für eine entitätsmenge hinzuzufügen, rufen Sie `AddToEntitySet`, wobei *EntitySet* ist der Name der Entitätenmenge. Beispielsweise `AddToProducts` wird ein neues `Product` auf die `Products` Entitätenmenge. Wenn Sie den Proxy zu generieren, WCF Data Services erstellt automatisch diese stark typisierte **AddTo** Methoden.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

Verwenden Sie zum Hinzufügen einer Verknüpfung zwischen zwei Entitäten die **AddLink** und **SetLink** Methoden. Der folgende Code Fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen diesen.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

Verwendung **AddLink** Wenn die Navigationseigenschaft eine Auflistung ist. In diesem Beispiel werden wir ein Produkt hinzufügen der `Products` Auflistung für den Lieferanten.

Verwendung **SetLink** die Navigationseigenschaft ist bei einer einzelnen Entität. In diesem Beispiel werden wir das Festlegen der `Supplier` auf der Produkt-Eigenschaft.

## <a name="update--patch"></a>Update / Patch

Um eine Entität zu aktualisieren, rufen die **UpdateObject** Methode.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

Das Update wird ausgeführt, wenn Sie rufen **SaveChanges**. Standardmäßig sendet WCF eine HTTP MERGE-Anforderung. Die **PatchOnUpdate** Option weist die WCF zum Senden einer HTTP-PATCH.

> [!NOTE]
> Warum PATCH im Vergleich zu verbinden? Die ursprüngliche HTTP 1.1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) alle HTTP-Methode mit der Semantik von "teilweises Update" wurde nicht definiert. Unterstützung von teilupdates, definiert die OData-Spezifikation die MERGE-Methode. In 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definiert die PATCH-Methode für teilweise Updates. Erfahren Sie einige der Verlauf in diesem [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) auf die WCF Data Services-Blog. Heute sind PATCH über MERGE bevorzugt. Der OData-Controller erstellt, indem das Gerüst für die Web-API unterstützt beide Methoden.


Wenn Sie die gesamte Entität (PUT-Semantik) ersetzen möchten, geben Sie die **ReplaceOnUpdate** Option. Dies bewirkt, dass WCF zum Senden einer HTTP PUT-Anforderung.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a>Löschen einer Entität

Um eine Entität zu löschen, rufen **DeleteObject**.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a>Rufen Sie eine OData-Aktion

In OData [Aktionen](odata-actions.md) bieten eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.

Obwohl das Metadatendokument des OData-Aktionen beschrieben, erstellt die Proxyklasse keine stark typisierte Methoden nicht für sie. Sie können immer noch eine OData-Aktion aufrufen, indem die generischen **Execute** Methode. Allerdings müssen Sie wissen, die Datentypen der Parameter und der Rückgabewert.

Z. B. die `RateProduct` Aktion akzeptiert Parameter, die mit dem Namen "Bewertung" vom Typ `Int32` und gibt eine `double`. Der folgende Code zeigt, wie diese Aktion aufgerufen wird.

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

Weitere Informationen finden Sie unter[Dienstvorgänge aufrufen und Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).

Eine Möglichkeit besteht darin, Erweitern der **Container** Klasse eine stark typisierte Methode bereitstellen, die die Aktion aufruft:

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
