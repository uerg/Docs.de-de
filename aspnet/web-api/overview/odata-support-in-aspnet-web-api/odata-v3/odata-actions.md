---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Unterstützen von OData-Aktionen in der ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In OData sind Aktionen eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen umfassen: implementieren...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: b7a968082587120c2a19be86524f9b2eba80856e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370454"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Unterstützung von OData-Aktionen in der ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In OData *Aktionen* sind eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen umfassen:
> 
> - Implementieren von komplexen Transaktionen.
> - Bearbeiten gleichzeitig mehrere Entitäten.
> - Erlauben nur bestimmte Eigenschaften einer Entität aktualisiert.
> - Senden von Informationen an den Server, die nicht in einer Entität definiert wird.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API 2
> - OData v3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Beispiel: Bewertung eines Produkts

In diesem Beispiel möchten wir, damit Benutzer, die Produkte zu bewerten, und machen Sie dann die durchschnittliche Bewertung für jedes Produkt. In der Datenbank werden wir eine Liste von Bewertungen auf Produkte verschlüsselt gespeichert.

Hier ist das Modell, das wir verwenden können, um die Bewertungen im Entity Framework darzustellen:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Wir möchten jedoch nicht POST-Clients eine `ProductRating` Objekt, das eine Auflistung von "Ratings". Intuitiv die Bewertung der Sammlung von Produkten zugeordnet ist, und der Client müssen nur den Bewertungswert bereitstellen.

Aus diesem Grund definieren anstatt die normalen CRUD-Vorgänge zu verwenden, wir eine Aktion, die ein Client aufrufen kann, auf ein Produkt. In der OData-Terminologie, die Aktion ist *gebunden* zu Product-Entitäten.

>Aktionen verursachen Nebeneffekte auf dem Server. Aus diesem Grund werden sie die mit HTTP POST-Anforderungen aufgerufen. Aktionen möglich, Parameter und Rückgabetypen auf, die in den Metadaten beschrieben werden. Der Client sendet die Parameter im Hauptteil Anforderung, und der Server sendet den Rückgabewert in den Antworttext. Zum Aufrufen der Aktion "Rate Produkt" sendet der Client einen Beitrag zu einem URI wie folgt:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Die Daten in der POST-Anforderung werden einfach die Produkt-Bewertung:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarieren Sie die Aktion in das Entity Data Model

Fügen Sie in Ihrer Web-API-Konfiguration die Aktion mit den Entity Data Model (EDM) hinzu:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Dieser Code definiert die "RateProduct" als eine Aktion, die für die Product-Entitäten ausgeführt werden kann. Deklariert außerdem, dass die Aktion eine **Int** Parameter mit dem Namen "Rating", und gibt eine **Int** Wert.

## <a name="add-the-action-to-the-controller"></a>Hinzufügen der Aktions zum Controller

Die Aktion "RateProduct" ist an Product-Entitäten gebunden. Um die Aktion zu implementieren, fügen Sie eine Methode, die mit dem Namen `RateProduct` an den Controller Produkte:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Beachten Sie, dass der Name der Methode mit dem Namen der Aktion im EDM übereinstimmt. Die Methode verfügt über zwei Parameter:

- *Schlüssel*: der Schlüssel für das Produkt zu Rate.
- *Parameter*: ein Wörterbuch mit Werten für Aktion-Parameter.

Wenn Sie die Standard-routingkonventionen verwenden, muss der Key-Parameter "Key" benannt werden. Es ist auch wichtig, Sie enthalten die **[FromOdataUri]** Attribut, wie gezeigt. Dieses Attribut teilt die Web-API-OData-Syntaxregeln beim Analysieren des Schlüssels aus der Anforderungs-URI zu verwenden.

Verwenden der *Parameter* Wörterbuch, das die Aktionsparameter zu erhalten:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Wenn der Client der Action-Parameter in der richtigen sendet zu formatieren, den Wert der **ModelState.IsValid** ist "true". In diesem Fall können Sie die **ODataActionParameters** Wörterbuch, das die Parameterwerte abzurufen. In diesem Beispiel die `RateProduct` Aktion nimmt einen einzelnen Parameter, die mit dem Namen "Rating".

## <a name="action-metadata"></a>Aktion-Metadaten

Um die Metadaten des Diensts anzuzeigen, senden Sie eine GET-Anforderung an /odata/$ Metadaten ein. Dies ist der Teil der Metadaten, die deklariert die `RateProduct` Aktion:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Die **FunctionImport** Element deklariert, die Aktion. Die meisten Felder sind selbsterklärend, doch die beiden sind zu beachten:

- **IsBindable** bedeutet, dass die Aktion kann aufgerufen werden, für die Zielentität, mindestens ein Teil der Zeit.
- **IsAlwaysBindable** bedeutet, dass die Aktion kann für die Zielentität immer aufgerufen werden.

Der Unterschied besteht darin, dass einige Aktionen immer für Clients verfügbar sind, aber andere Aktionen vom Zustand der Entität abhängig können. Nehmen wir beispielsweise an, dass Sie eine Aktion "Kaufen" definieren. Sie können nur ein Element erwerben, die auf Lager ist. Wenn der Artikel nicht vorrätig ist, kann kein Client mit dieser Aktion aufrufen.

Wenn Sie das EDM definieren die **Aktion** Methode erstellt eine Aktion immer gebunden werden kann:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Ich werde nicht immer – bindbare Aktionen (so genannte *vorübergehende* Aktionen) weiter unten in diesem Thema.

## <a name="invoking-the-action"></a>Aufrufen der Aktion

Jetzt sehen wir uns an, wie ein Client dadurch aufgerufen würde. Angenommen, das der Client möchte gerne eine Bewertung von 2 für das Produkt mit der ID = 4. Hier ist eine Beispiel-Anforderungsnachricht, die mithilfe von JSON-Format für den Anforderungstext ein:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

So sieht die Response-Nachricht aus:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Binden eine Aktion an einer Entitätenmenge

Im vorherigen Beispiel ist die Aktion an eine einzelne Entität gebunden ist: der Client bewertet, ein einzelnes Produkt. Sie können auch eine Aktion an eine Auflistung von Entitäten binden. Stellen Sie einfach die folgenden Änderungen:

Im EDM hinzufügen der Aktion auf der Entität **Auflistung** Eigenschaft.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Lassen Sie in der Controllermethode, die *Schlüssel* Parameter.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Nun ruft der Client die Aktion auf die Entitätenmenge Produkte:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Aktionen mit Parametern für die Datensammlung

Aktionen können Parameter aufweisen, die eine Auflistung von Werten zu akzeptieren. Verwenden Sie im EDM **CollectionParameter&lt;T&gt;**  um den Parameter zu deklarieren.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Damit wird deklariert einen Parameter namens "Ratings", die eine Auflistung von akzeptiert **Int** Werte. In der Controllermethode, erhalten Sie immer noch den Wert des Parameters aus der **ODataActionParameters** -Objekt, aber jetzt der Wert ist eine **ICollection&lt;Int&gt;**  Wert:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Vorübergehende Aktionen

Im Beispiel "RateProduct" können Benutzer ein Produkt, immer bewerten, damit die Aktion immer verfügbar ist. Aber einige Aktionen abhängig vom Zustand der Entität. Beispielsweise ist in einem Dienst videoverleih die Aktion "Kasse" nicht immer verfügbar. (Diese hängt davon ab, ob eine Kopie des betreffenden Videos verfügbar ist.) Diese Art von Aktion wird aufgerufen, eine *vorübergehende* Aktion.

In den Dienstmetadaten eine vorübergehende Aktion verfügt **IsAlwaysBindable** gleich "false". Das ist eigentlich den Standardwert, damit die Metadaten wie folgt aussehen wird:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Der folgende warum dies wichtig ist: Wenn eine Aktion vorübergehend auftritt, ist der Server muss dem Client weiß, dass die Aktion verfügbar ist. Hierzu werden ein Link zu der Aktion in der Entität eingeschlossen. Hier ist ein Beispiel für eine Movie-Entität:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Die Eigenschaft "#CheckOut" enthält einen Link zu der CheckOut-Aktion. Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.

Um eine vorübergehende Aktion im EDM zu deklarieren, rufen Sie die **TransientAction** Methode:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Darüber hinaus müssen Sie eine Funktion bereitstellen, die einen Aktionslink für eine bestimmte Entität zurückgibt. Legen Sie diese Funktion durch den Aufruf **HasActionLink**. Sie können die Funktion als Lambda-Ausdruck schreiben:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link auf die Aktion zurück. Die OData-Serialisierer enthält diesen Link an, beim Serialisieren der Entitäts. Wenn die Aktion nicht verfügbar ist, wird die Funktion gibt `null`.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel für OData-Aktionen](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
