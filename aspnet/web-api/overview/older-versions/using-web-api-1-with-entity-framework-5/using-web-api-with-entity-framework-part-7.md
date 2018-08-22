---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Teil 7: Erstellen der Hauptseite Seite | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb4704e7f4f13fab04acdbdd642174884517e18a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826759"
---
<a name="part-7-creating-the-main-page"></a>Teil 7: Erstellen der Hauptseite Seite
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Erstellen der Hauptseite Seite

In diesem Abschnitt erstellen Sie die Seite für die Hauptassembly der Anwendung. Auf dieser Seite wird komplexer als die Seite "Administrator" sein, damit wir es in mehreren Schritten angehen müssen. Dabei werden einige erweiterten Verfahren von "Knockout.js" angezeigt. Hier ist das grundlegende Layout der Seite:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkte" enthält ein Array von Produkten.
- "Warenkorb" enthält ein Array von Produkten mit Mengen. Klicken Sie auf "Add to Cart" aktualisiert des Warenkorbs.
- "Orders" enthält ein Array von bestellungs-IDs.
- "Details" enthält ein Order-Details, die ein Array von Elementen (Produkte mit Mengen) ist

Wir beginnen mit dem einige grundlegende Layout in HTML-Code, ohne Datenbindung oder ein Skript zu definieren. Öffnen Sie die Datei Views/Home/Index.cshtml, und Ersetzen Sie den Inhalt durch Folgendes:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Als Nächstes fügen Sie einen Abschnitt des Skripts ein, und erstellen Sie ein leeres Modell anzeigen:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Basierend auf den Entwurf, die zuvor skizziert, benötigt unsere Ansichtsmodell beobachtbare Objekte, für Produkte, Einkaufswagen, Bestellungen und Details. Fügen Sie die folgenden Variablen auf die `AppViewModel` Objekt:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Benutzer können Elemente aus der Liste der Produkte in den Einkaufswagen hinzufügen und Entfernen von Elementen aus dem Warenkorb. Um diese Funktionen zu kapseln, erstellen wir eine andere Ansichtsmodell Klasse, die ein Produkt darstellt. Fügen Sie den folgenden Code zu `AppViewModel` hinzu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Die `ProductViewModel` -Klasse enthält zwei Funktionen, die verwendet werden, um das Produkt in und aus dem Warenkorb zu verschieben: `addItemToCart` eine Einheit des Produkts zum Einkaufswagen hinzugefügt und `removeAllFromCart` entfernt alle Mengen des Produkts.

Benutzer können wählen Sie einen vorhandenen Auftrag und den Details der Bestellung zu erhalten. Wir werden diese Funktionalität in einer anderen Ansicht-Modell kapseln:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Die `OrderDetailsViewModel` wird initialisiert, indem eine Bestellung, und es Ruft die Details der durch eine AJAX-Anforderung an den Server gesendet.

Beachten Sie auch die `total` Eigenschaft für die `OrderDetailsViewModel`. Diese Eigenschaft ist eine besondere Art von Observable-Objekt namens eine [berechnet Observable](http://knockoutjs.com/documentation/computedObservables.html). Wie der Name schon sagt, berechnete ein beobachtbares Objekt können Sie die Datenbindung an einen berechneten Wert&#8212;in diesem Fall die Gesamtkosten der Bestellung.

Fügen Sie diese Funktionen `AppViewModel`:

- `resetCart` Entfernt alle Elemente aus dem Warenkorb.
- `getDetails` Ruft die Details einer Bestellung (von Pusing ein neues `OrderDetailsViewModel` auf die `details` Liste).
- `createOrder` erstellt eine neue Bestellung und leert den Einkaufswagen.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Zum Schluss initialisieren Sie das Ansichtsmodell, die AJAX-Anforderungen für die Produkte und Bestellungen:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, das ist viel Code, aber wir integrierter Sie Schritt für Schritt, hoffen wir der Entwurf ist klar. Jetzt können wir den HTML-Code einige Bindungen "Knockout.js" hinzufügen.

**Produkte**

Hier sind die Bindungen für die Produktliste:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Dies führt eine Iteration durch die Products-Array und zeigt den Namen und den Preis. Die Schaltfläche "Hinzufügen, Order" ist sichtbar, nur, wenn der Benutzer angemeldet ist.

Schaltfläche "Hinzufügen, Order" ruft `addItemToCart` auf die `ProductViewModel` Instanz für das Produkt. Dies veranschaulicht ein nützliches Feature von "Knockout.js": Wenn ein Ansichtsmodell andere Ansichtsmodelle enthält, können Sie die Bindungen auf das interne Modell anwenden. In diesem Beispiel die Bindungen in der `foreach` gelten für jede der `ProductViewModel` Instanzen. Dieser Ansatz ist wesentlich übersichtlicher als sämtlicher Funktionen in ein einzelnes Ansichtsmodell.

**Warenkorb**

Hier sind die Bindungen für den Warenkorb:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Dies das Cart-Array durchläuft und zeigt den Namen, den Preis und die Menge. Beachten Sie, dass der Link "Entfernen" und die Schaltfläche "Create Order" Anzeigemodell Funktionen gebunden sind.

**Aufträge**

Hier sind die Bindungen für die Liste der Bestellungen:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Dies führt eine Iteration durch die Bestellungen und zeigt die Auftrags-ID. Das Click-Ereignis, auf den Link gebunden ist, um die `getDetails` Funktion.

**Auftragsdetails**

Hier sind die Bindungen für die Bestelldetails anzeigen:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Dies führt eine Iteration durch die Elemente in der Reihenfolge und zeigt an, das Produkt, Preis und eines Orts. Das umgebende DIV-Element ist nur sichtbar, wenn das Details-Array ein oder mehrere Elemente enthält.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben Sie eine Anwendung, die Entity Framework verwendet, um die Kommunikation mit der Datenbank und ASP.NET Web-API zu einer öffentlich zugänglichen Schnittstelle auf der Datenschicht erstellt. Wir verwenden die ASP.NET MVC 4 zum Rendern der HTML-Seiten, und den "Knockout.js" sowie die jQuery zum dynamischen Interaktionen ohne Neuladen der Seite bereitstellen.

Zusätzliche Ressourcen:

- [ASP.NET Data Access-Inhaltszuordnung](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework-Entwicklercenter](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Vorherige](using-web-api-with-entity-framework-part-6.md)
