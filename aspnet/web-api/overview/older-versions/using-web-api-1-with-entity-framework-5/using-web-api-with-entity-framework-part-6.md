---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
title: 'Teil 6: Erstellen von Produkt- und Reihenfolge Controller | Microsoft Docs'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 91ee29ee-0689-40ee-914a-e7dd733b6622
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-6
msc.type: authoredcontent
ms.openlocfilehash: ef7674476e0db334642daa29e352f615135b07ab
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-6-creating-product-and-order-controllers"></a>Teil 6: Erstellen von Produkt- und Reihenfolge Controller
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-a-products-controller"></a>Hinzufügen einer Produkts-Controllers

Der Administrator-Controller ist für Benutzer mit Administratorrechten aus. Kunden, andererseits, können Produkte anzeigen, jedoch kann nicht erstellen, aktualisieren oder löschen Sie sie.

Wir können problemlos den Zugriff auf die Methoden Post, Put und Delete einschränken, ohne die Get-Methoden. Aber sehen Sie sich die Daten, die für ein Produkt zurückgegeben werden:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample1.json?highlight=1)]

Die `ActualCost` Eigenschaft darf nicht für Kunden sichtbar sein! Die Lösung besteht darin, definieren Sie eine *Datenübertragungsobjekt* (DTO) enthält, die eine Teilmenge der Eigenschaften, die Kunden angezeigt werden sollen. Wir verwenden LINQ zum Projekt `Product` -Instanzen `ProductDTO` Instanzen.

Fügen Sie eine Klasse mit dem Namen `ProductDTO` Ordner Models.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample2.cs)]

Nun fügen Sie den Controller hinzu. Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**. In der **Controller hinzufügen** Dialogfeld Namen des Controllers &quot;ProductsController&quot;. Klicken Sie unter **Vorlage**Option **leere API-Controller**.

![](using-web-api-with-entity-framework-part-6/_static/image1.png)

Ersetzen Sie alles in der Quelldatei an, durch den folgenden Code:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample3.cs)]

Der Controller verwendet weiterhin den `OrdersContext` zum Abfragen der Datenbank. Aber anstatt `Product` Instanzen direkt, wir rufen `MapProducts` , diese auf projizieren `ProductDTO` Instanzen:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample4.cs?highlight=1)]

Die `MapProducts` Methode gibt ein **IQueryable**, sodass wir das Ergebnis mit anderen Abfrageparametern verfassen können. Sehen Sie hierzu finden Sie unter der `GetProduct` -Methode, die Fügt eine **, in dem** -Klausel, um die Abfrage:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample5.cs?highlight=2)]

## <a name="add-an-orders-controller"></a>Fügen Sie einen Orders-Controller

Als Nächstes fügen Sie einen Controller, mit dem Benutzer erstellen und Anzeigen von Aufträgen an.

Wir beginnen mit einem anderen DTO. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in den Ordner Models, und fügen Sie eine Klasse mit dem Namen `OrderDTO` verwenden Sie die folgende Implementierung:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample6.cs)]

Nun fügen Sie den Controller hinzu. Im Projektmappen-Explorer mit der rechten Maustaste des Ordners Controllers. Wählen Sie **hinzufügen**, und wählen Sie dann **Controller**. In der **Controller hinzufügen** im Dialogfeld die folgenden Optionen festlegen:

- Klicken Sie unter **Controllernamen**, geben Sie "OrdersController".
- Klicken Sie unter **Vorlage**Option "API-Controller mit Lese-/Schreibzugriff Aktionen unter Verwendung von Entity Framework".
- Klicken Sie unter **Modellschemas**Option &quot;Reihenfolge (ProductStore.Models)&quot;.
- Klicken Sie unter **Datenkontextklasse**Option &quot;OrdersContext (ProductStore.Models)&quot;.

![](using-web-api-with-entity-framework-part-6/_static/image2.png)

Klicken Sie auf **Hinzufügen**. Dadurch wird eine Datei namens OrdersController.cs hinzugefügt. Als Nächstes müssen wir die Standardimplementierung des Controllers ändern.

Löschen Sie zuerst die `PutOrder` und `DeleteOrder` Methoden. Für dieses Beispiel nicht Kunden ändern oder löschen vorhandene Aufträge. In einer realen Anwendung benötigen Sie viele der Back-End-Logik, um diese Fälle zu behandeln. Geliefert (z. B. wurde die Bestellung bereits?)

Ändern der `GetOrders` Methode, um nur die Aufträge zurückzugeben, die dem Benutzer gehören:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample7.cs)]

Ändern der `GetOrder` Methode wie folgt:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample8.cs)]

Hier werden die Änderungen, die wir an die Methode vorgenommen:

- Der Rückgabewert ist eine `OrderDTO` Instanz anstelle von einer `Order`.
- Wenn wir die Datenbank für die Bestellung abzufragen, verwenden wir die [DbQuery.Include](https://msdn.microsoft.com/en-us/library/gg696395) Methode, um den zugehörigen fetch `OrderDetail` und `Product` Entitäten.
- Wir haben das Ergebnis mithilfe einer Projektion vereinfachen.

Die HTTP-Antwort enthält ein Array von Produkten mit Mengen:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample9.json)]

Dieses Format ist einfacher, Clients als die ursprüngliche Objektdiagramm nutzen die verschachtelte Entitäten (Reihenfolge, Details und Produkte) enthält.

Die letzte Methode, die beachtet werden `PostOrder`. Jetzt, diese Methode nimmt ein `Order` Instanz. Aber in Betracht ziehen, was geschieht, wenn ein Client, einen Anforderungstext wie folgt sendet:

[!code-json[Main](using-web-api-with-entity-framework-part-6/samples/sample10.json)]

Dies ist eine gut strukturierte Reihenfolge und Entity Framework wird es problemlos in die Datenbank einfügen. Aber es enthält eine Produktentität, die nicht bereits vorhanden ist. Der Client ein neues Produkt in unserer Datenbank erstellte! Dies wird eine plötzliche an die Reihenfolge Fullfilment Abteilung sein, wenn sie eine Bestellung für Koala wegen finden Sie unter. Die Moral ist, achten Sie tatsächlich zu Daten, die Sie in einer POST- oder PUT-Anforderung akzeptieren.

Um dieses Problem zu vermeiden, ändern Sie die `PostOrder` Methode, um eine `OrderDTO` Instanz. Verwenden der `OrderDTO` zum Erstellen der `Order`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample11.cs)]

Beachten Sie, die wir verwenden die `ProductID` und `Quantity` Eigenschaften, und es ignoriert alle Werte, die der Client für Produktnamen oder Preis gesendet. Wenn die Produkt-ID ungültig ist, es wird die foreign Key-Einschränkung in der Datenbank verletzen, und die Einfügung schlägt fehlt, wie er es sollte.

Hier wird die vollständige `PostOrder` Methode:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample12.cs)]

Fügen Sie schließlich die **autorisieren** -Attribut auf den Controller:

[!code-csharp[Main](using-web-api-with-entity-framework-part-6/samples/sample13.cs)]

Jetzt können nur registrierte Benutzer erstellen oder Aufträge anzeigen.

>[!div class="step-by-step"]
[Zurück](using-web-api-with-entity-framework-part-5.md)
[Weiter](using-web-api-with-entity-framework-part-7.md)
