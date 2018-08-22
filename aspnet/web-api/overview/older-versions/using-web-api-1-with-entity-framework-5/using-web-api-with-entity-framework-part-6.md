---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt- und Order-Controllern | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: 642ff4554ed3664af0b5cc8e49d6b236c568131b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827333"
---
<a name="part-6-creating-product-and-order-controllers"></a>Teil 6: Erstellen von Produkt- und in der Reihenfolge Controller
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Hinzufügen eines Controllers Produkte

Der Administrator-Controller ist für Benutzer, die über Administratorrechte verfügen. Kunden, auf der anderen Seite können Produkte anzeigen, jedoch kann nicht erstellen, aktualisieren oder löschen.

Wir können problemlos den Zugriff auf die POST-, PUT- und Delete-Methoden, einschränken, bei die Get-Methoden geöffnet bleibt. Aber sehen Sie sich die Daten, die für ein Produkt zurückgegeben werden:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Die `ActualCost` Eigenschaft sollte nicht für Kunden sichtbar sein! Die Lösung besteht darin, definieren Sie eine *Datenübertragungsobjekt* (DTO), umfasst eine Teilmenge der Eigenschaften, die für Kunden sichtbar sein sollen. Wir verwenden LINQ zum Projekt `Product` -Instanzen `ProductDTO` Instanzen.

Fügen Sie eine Klasse, die mit dem Namen `ProductDTO` zum Ordner "Models".

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Nun fügen Sie den Controller hinzu. Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**. In der **Controller hinzufügen** Dialogfeld benennen Sie den Controller &quot;ProductsController&quot;. Klicken Sie unter **Vorlage**Option **leeren API-Controller**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Ersetzen Sie alles, was in der Quelldatei an, mit dem folgenden Code:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Der Controller verwendet weiterhin die `OrdersContext` zum Abfragen der Datenbank. Aber anstatt `Product` Instanzen direkt, wir rufen `MapProducts` , diese auf projizieren `ProductDTO` Instanzen:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Die `MapProducts` Methode gibt ein **"IQueryable"**, sodass wir das Ergebnis mit anderen Abfrageparametern verfassen können. Sehen Sie in der `GetProduct` -Methode, die Fügt eine **, in denen** -Klausel, um die Abfrage:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Fügen Sie eine Orders-Controller hinzu.

Als Nächstes fügen Sie einen Controller, mit dem Benutzer erstellen und Anzeigen von Bestellungen hinzu.

Wir beginnen mit einem anderen DTO. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner "Models", und fügen Sie eine Klasse, die mit dem Namen `OrderDTO` verwenden Sie die folgende Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Nun fügen Sie den Controller hinzu. Klicken Sie im Projektmappen-Explorer den Ordner "Controllers". Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**. In der **Controller hinzufügen** im Dialogfeld die folgenden Optionen festlegen:

- Klicken Sie unter **Controllername**, geben Sie "OrdersController".
- Klicken Sie unter **Vorlage**Option "API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entitätsframework".
- Klicken Sie unter **Modellklasse**Option &quot;Reihenfolge (ProductStore.Models)&quot;.
- Klicken Sie unter **Datenkontextklasse**Option &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Dadurch wird eine Datei namens OrdersController.cs hinzugefügt. Als Nächstes müssen wir die Standardimplementierung des Controllers zu ändern.

Löschen Sie zuerst die `PutOrder` und `DeleteOrder` Methoden. In diesem Beispiel nicht Kunden ändern oder löschen vorhandene Aufträge. In einer realen Anwendung benötigen Sie viele Back-End-Logik zum Behandeln dieser Fälle. Geliefert (z. B. wurde die Bestellung bereits?)

Ändern der `GetOrders` Methode, um nur die Aufträge zurückzugeben, die dem Benutzer gehören:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Ändern der `GetOrder` -Methode wie folgt:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Hier sind die Änderungen, die wir der Methode vorgenommen:

- Der Rückgabewert ist ein `OrderDTO` Instanz statt einer `Order`.
- Wenn wir die Datenbank für die Bestellung abgefragt wird, verwenden wir die [DbQuery.Include](https://msdn.microsoft.com/library/gg696395) Methode, um die zugehörigen abrufen `OrderDetail` und `Product` Entitäten.
- Wir vereinfachen das Ergebnis mithilfe einer Projektion.

Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Dieses Format ist einfacher, Clients als im ursprünglichen Objektdiagramm nutzen, die geschachtelte Entitäten (Order, Details und Produkte) enthält.

Die letzte Methode, die beachtet werden `PostOrder`. Derzeit, diese Methode akzeptiert eine `Order` Instanz. Aber bedenken, was geschieht, wenn ein Client, Anforderungstext sollte wie folgt sendet:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Dies ist eine gut strukturierte Reihenfolge aus, und Entity Framework wird zum Glück fügen sie in der Datenbank. Aber es enthält eine Product-Entität, die nicht bereits vorhanden ist. Der Client erstellt einfach ein neues Produkt in der Datenbank. Dies wird an die Reihenfolge Ausführung-Abteilung, überrascht sein, wenn sie einen Auftrag für Koala Bears finden Sie unter. Die Moral, seien Sie wirklich die Daten, die Sie in einer POST- oder PUT-Anforderung annehmen.

Um dieses Problem zu vermeiden, ändern Sie die `PostOrder` Methode, um eine `OrderDTO` Instanz. Verwenden der `OrderDTO` zum Erstellen der `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Beachten Sie, die wir verwenden die `ProductID` und `Quantity` Eigenschaften und ignorieren Sie alle Werte, die vom Client für Produktnamen oder Preis gesendet wurde. Wenn die Produkt-ID ungültig ist, wird es verletzen, foreign Key-Einschränkung in der Datenbank und der Einfügevorgang fehl, wie er es sollte.

Hier ist die vollständige `PostOrder` Methode:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Fügen Sie abschließend die **autorisieren** Attribut mit dem Controller:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Jetzt können nur registrierte Benutzer erstellen oder Anzeigen von Bestellungen.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-5.md)
> [Weiter](using-web-api-with-entity-framework-part-7.md)
