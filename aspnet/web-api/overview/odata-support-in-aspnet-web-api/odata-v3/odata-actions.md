---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "Unterstützen von OData-Aktionen in ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: "In OData sind Aktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen aufgeführt: implementieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>Unterstützen von OData-Aktionen in ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> In OData *Aktionen* bieten eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Einige Verwendungsmöglichkeiten für Aktionen aufgeführt:
> 
> - Implementieren von komplexen Transaktionen.
> - Bearbeiten gleichzeitig mehrere Entitäten.
> - Zulassen, dass Updates nur für bestimmte Eigenschaften einer Entität.
> - Senden von Informationen an den Server, die nicht in einer Entität definiert ist.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 2
> - OData-Version 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Beispiel: Bewertung eines Produkts

In diesem Beispiel möchten wir, damit Benutzer Produkte zu bewerten, und machen Sie dann die durchschnittsbewertungen für jedes Produkt. In der Datenbank speichern wir eine Liste von Bewertungen, die als Schlüssel für Produkte.

Hier wird das Modell, die zur Darstellung der Bewertungen in Entity Framework verwendet werden kann:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Aber wir nicht möchten, dass Clients auf POST ein `ProductRating` Objekt, das eine Auflistung von "Bewertung". Intuitiv die Bewertung der Sammlung von Produkten zugeordnet ist, und der Client müssen nur den Bewertungswert zu senden.

Aus diesem Grund definieren anstelle der normalen CRUD-Vorgänge wir eine Aktion, die ein Client aufrufen kann an einem Produkt. In der Terminologie von OData-Aktion ist *gebunden* Product-Entitäten.

>Aktionen verursachen Nebeneffekte auf dem Server. Aus diesem Grund werden sie aufgerufen, indem HTTP POST-Anforderungen. Aktionen möglich, Parameter und Rückgabetypen, die in den Dienstmetadaten beschrieben werden. Der Client sendet die Parameter im Hauptteil Anforderung, und der Server sendet den Rückgabewert im Antworttext. Um die Aktion "Rate Produkt" aufzurufen, sendet der Client einer POST-Anforderung an einen URI wie folgt:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

Die Daten in die POST-Anforderung ist einfach die Bewertung des Produkts:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Deklarieren Sie die Aktion in das Entity Data Model

Fügen Sie die Aktion in Ihrer Web-API-Konfiguration mit den Entity Data Model (EDM):

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Dieser Code definiert "RateProduct" als eine Aktion, die für die Product-Entitäten durchgeführt werden kann. Deklariert außerdem, dass die Aktion wird ein **Int** Parameter mit dem Namen "Bewertung", und gibt eine **Int** Wert.

## <a name="add-the-action-to-the-controller"></a>Die Aktion an den Controller hinzufügen

Die Aktion "RateProduct" ist an der Product-Entitäten gebunden. Um die Aktion zu implementieren, fügen Sie eine Methode namens `RateProduct` an den Controller Produkte:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Beachten Sie, dass der Name der Methode den Namen der Aktion im EDM übereinstimmt. Die Methode verfügt über zwei Parameter:

- *Schlüssel*: der Schlüssel für das Produkt zu Rate.
- *Parameter*: ein Wörterbuch von Parameterwerten für die Aktion.

Wenn Sie standardroutingkonventionen verwenden, muss der Key-Parameter "Key" benannt werden. Es ist auch wichtig, enthalten die **[FromOdataUri]** Attribut, wie dargestellt. Dieses Attribut weist Web API OData-Syntaxregeln verwenden, wenn es sich um den Schlüssel vom Anforderungs-URI analysiert werden.

Verwenden der *Parameter* Wörterbuch zum Abrufen der Action-Parameter:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

Wenn der Client sendet, der Action-Parameter in der richtigen zu formatieren, den Wert der **ModelState.IsValid** ist "true". In diesem Fall können Sie die **ODataActionParameters** Wörterbuch, das die Werte der Parameter erhalten. In diesem Beispiel wird die `RateProduct` Aktion nimmt einen Parameter mit dem Namen "Bewertung".

## <a name="action-metadata"></a>Aktion-Metadaten

Um die Metadaten des Diensts anzuzeigen, senden Sie eine GET-Anforderung an /odata/$ Metadaten ein. Hier ist der Teil der Metadaten, die deklariert die `RateProduct` Aktion:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

Die **FunctionImport** Element deklariert, die Aktion. Die meisten Felder sind selbsterklärend, doch es sind zwei Folgendes zu beachten:

- **IsBindable** bedeutet, dass die Aktion kann aufgerufen werden, in der Zielentität mindestens ein Teil der Zeit.
- **IsAlwaysBindable** bedeutet, dass die Aktion kann für die Zielentität immer aufgerufen werden.

Der Unterschied ist, dass einige Aktionen immer für Clients verfügbar sind, aber andere Aktionen können von den Zustand der Entität abhängen. Nehmen wir beispielsweise an, dass Sie eine Aktion "Einkauf" definieren. Sie können nur ein Element erwerben, die vorrätig ist. Wenn das Element keine Lagerbestände mehr verfügbar ist, kann kein Client mit dieser Aktion aufrufen.

Wenn Sie das EDM definieren die **Aktion** Methode erstellt eine Aktion immer gebunden werden kann:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Ich werde zu Aktionen Not stets-gebunden werden kann (so genannte *vorübergehenden* Aktionen) weiter unten in diesem Thema.

## <a name="invoking-the-action"></a>Aufrufen der Aktion

Jetzt sehen wir, wie ein Client dieser Aktion aufgerufen werden würde. Angenommen, das der Client möchte Geben Sie eine Bewertung von 2 für das Produkt mit der ID = 4. Hier ist eine Beispiel-Anforderungsnachricht, für Anforderungstext JSON-Format verwenden:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

So sieht die Antwortnachricht aus:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Binden eine Aktion an einer Entitätenmenge

Im vorherigen Beispiel ist die Aktion für eine einzelne Entität gebunden: der Client bewertet ein einzelnes Produkts. Sie können auch eine Aktion an eine Auflistung von Entitäten binden. Nur die folgenden Änderungen vornehmen:

Fügen Sie die Aktion im EDM, auf der Entität **Auflistung** Eigenschaft.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Lassen Sie die Controllermethode, die *Schlüssel* Parameter.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Nachdem der Client die Aktion auf die Entitätenmenge Produkte aufruft:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Aktionen mit Parametern für die Datensammlung

Aktionen können Parameter verfügen, die eine Auflistung von Werten annehmen. Verwenden Sie im EDM **CollectionParameter&lt;T&gt;**  um den Parameter zu deklarieren.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Damit wird deklariert einen Parameter namens "Bewertung", die eine Auflistung von akzeptiert **Int** Werte. In der Controllermethode erhalten Sie weiterhin den Wert des Parameters aus der **ODataActionParameters** Objekt aber jetzt der Wert ist ein **ICollection&lt;Int&gt;**  Wert:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Vorübergehender Aktionen

Im Beispiel "RateProduct" können Benutzer immer ein Produkt bewerten, sodass die Aktion immer verfügbar ist. Aber einige Aktionen hängen von den Zustand der Entität. Beispielsweise ist in einem Dienst videoverleih die Aktion "Kasse" nicht immer verfügbar. (Es abhängt, ob eine Kopie dieses Video verfügbar ist.) Dieser Typ der Aktion wird aufgerufen, eine *vorübergehenden* Aktion.

In den Dienstmetadaten wurde eine vorübergehende Aktion **IsAlwaysBindable** gleich "false". Die tatsächlich Wert ist der Standardwert, damit die Metadaten wie folgt aussieht:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Hier warum dies wichtig ist: Wenn eine Aktion vorübergehend ist, der Server muss dem Client feststellen, wann die Aktion verfügbar ist. Es wird ein Link zu der Aktion in der Entität eingeschlossen. Hier ist ein Beispiel für eine Entität Film:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

Die Eigenschaft "#CheckOut" enthält einen Link zu der Auscheckaktion. Wenn die Aktion nicht verfügbar ist, lässt der Server den Link aus.

Um einen vorübergehenden Aktion im EDM zu deklarieren, rufen Sie die **TransientAction** Methode:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Darüber hinaus müssen Sie eine Funktion bereitstellen, die einen Aktionslink für eine bestimmte Entität zurückgibt. Legen Sie diese Funktion durch Aufrufen von **HasActionLink**. Sie können die Funktion als Lambda-Ausdruck schreiben:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Wenn die Aktion verfügbar ist, gibt der Lambda-Ausdruck einen Link mit der Aktion zurück. Die OData-Serialisierer enthält diesen Link, wenn sie die Entität serialisiert. Wenn die Aktion nicht verfügbar ist, gibt die Funktion `null`.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel für OData-Aktionen](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
