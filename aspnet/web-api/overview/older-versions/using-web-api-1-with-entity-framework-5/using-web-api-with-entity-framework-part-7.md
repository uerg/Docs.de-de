---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: 'Teil 7: Erstellen den Hauptknoten Seite | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 2c378e68a1e6600daf655c19afbfe355e89496d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="part-7-creating-the-main-page"></a>Teil 7: Erstellen den Hauptknoten Seite
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Erstellen den Hauptknoten Seite

In diesem Abschnitt erstellen Sie die Seite "hauptanwendung". Auf dieser Seite wird komplexer als die Seite "Administrator" sein, damit wir es in mehreren Schritten Ansatz wird. Nebenbei sehen Sie, einige erweiterten Knockout.js Techniken. Hier wird das grundlegende Layout der Seite:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Products" enthält ein Array von Produkten.
- "Einkaufswagen" enthält ein Array von Produkten mit Mengen. Klicken auf "Add to Cart" wird der Einkaufswagen aktualisiert.
- "Orders" enthält ein Array von bestellungs-IDs.
- "Details" enthält ein Order Details anzeigen, die ein Array von Elementen (Produkte mit Mengen) ist

Wir beginnen mit einige grundlegende Layout in HTML, ohne die Datenbindung oder das Skript definieren. Öffnen Sie die Datei Views/Home/Index.cshtml, und Ersetzen Sie den Inhalt durch Folgendes:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Als Nächstes fügen Sie einen Abschnitt zu Skripts hinzu, und erstellen Sie ein leeres Modell anzeigen:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Unsere Ansichtsmodell benötigt basierend auf den Entwurf, die zuvor skizziert, eine Wahrnehmbare Elemente für Produkte, Einkaufswagen, Aufträge und Details. Fügen Sie die folgenden Variablen, die `AppViewModel` Objekt:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Benutzer können Elemente aus der Produktliste in den Einkaufswagen hinzufügen und Entfernen von Elementen aus dem Einkaufswagen. Um diese Funktionen zu kapseln, erstellen wir eine andere ViewModel Klasse, die ein Produkt darstellt. Fügen Sie den folgenden Code zu `AppViewModel` hinzu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

Die `ProductViewModel` Klasse enthält zwei Funktionen, die verwendet werden, um das Produkt zu und aus dem Einkaufswagen zu verschieben: `addItemToCart` Fügt eine Einheit des Produkts zum Einkaufswagen und `removeAllFromCart` entfernt alle Mengen des Produkts.

Benutzer können wählen Sie einen vorhandenen Auftrag und erhalten die Auftragsdetails enthält. Wir werden diese Funktionen in einer anderen Ansichtsmodell kapseln:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

Die `OrderDetailsViewModel` wird initialisiert, indem eine Bestellung und den Bestellungsdetails durch Senden einer AJAX-Anforderung an den Server abgerufen.

Beachten Sie auch, dass die `total` Eigenschaft auf die `OrderDetailsViewModel`. Diese Eigenschaft ist eine spezielle Art von Observable-Objekt aufgerufen, eine [Observable-Objekt berechnet](http://knockoutjs.com/documentation/computedObservables.html). Wie der Name schon sagt, eine berechnete Observable-Objekt können Sie Daten binden an einen berechneten Wert&#8212;in diesem Fall die Gesamtkosten der Bestellung.

Als Nächstes fügen Sie diese Funktionen `AppViewModel`:

- `resetCart` Entfernt alle Elemente aus dem Einkaufswagen.
- `getDetails` Ruft die Details einer Bestellung (durch ein neues Pusing `OrderDetailsViewModel` auf die `details` Liste).
- `createOrder` erstellt eine neue Bestellung und leert den Einkaufswagen.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Initialisieren Sie schließlich das Ansichtsmodell durch AJAX-Anforderungen für die Produkte und Aufträge:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK, also viel Code, aber wir erstellt er Sie schrittweise, wir hoffen Entwurf ist klar. Jetzt können wir einige Knockout.js Bindungen auf den HTML-Code hinzufügen.

**Produkte**

Hier werden die Bindungen für die Produktliste aus:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Diese Produkte Array durchläuft und zeigt den Namen und den Preis. Die Schaltfläche "Hinzufügen, Order" ist sichtbar, nur, wenn der Benutzer angemeldet ist.

Schaltfläche "Hinzufügen, Order" ruft `addItemToCart` auf die `ProductViewModel` Instanz für das Produkt. Dies beweist, dass ein nützliches Feature der Knockout.js: bei einem Ansichtsmodell andere Modelle anzeigen enthält, können Sie die Bindungen auf das interne Modell anwenden. In diesem Beispiel die Bindungen innerhalb der `foreach` gelten für jede der `ProductViewModel` Instanzen. Dieser Ansatz ist wesentlich übersichtlicher als einfügen alle Funktionen in einem einzelnen Modell anzeigen.

**Einkaufswagen**

Hier werden die Bindungen für den Einkaufswagen:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Dies führt eine Iteration durch die Warenkorb-Array und zeigt den Namen, den Preis und die Menge. Beachten Sie, dass der Link "Entfernen" und die Schaltfläche "Create Order" für das ViewModel Funktionen gebunden sind.

**Aufträge**

Hier werden die Bindungen für die Orders-Liste aus:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

Dies führt eine Iteration durch die Bestellungen und zeigt die Bestell-ID Das Click-Ereignis auf den Link gebunden ist, um die `getDetails` Funktion.

**Auftragsdetails**

Hier werden die Bindungen für die Auftragsdetails enthält:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Dies führt eine Iteration durch die Elemente in der Reihenfolge und zeigt an, das Produkt, Preis und eines Orts. Umgebenden DIV-Elements ist nur sichtbar, wenn das Details-Array ein oder mehrere Elemente enthält.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm erstellt Sie eine Anwendung, die Entity Framework verwendet, um die Kommunikation mit dem Datenbank- und ASP.NET Web-API, um eine öffentliche Schnittstelle auf die Datenschicht bereitzustellen. Wir verwenden die ASP.NET MVC 4 zum Rendern der HTML-Seiten und den Knockout.js sowie die jQuery um dynamische Aktivitäten ohne Seite Neuladen bereitzustellen.

Zusätzliche Ressourcen:

- [ASP.NET Data Access-Inhaltszuordnung](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Entity Framework-Developer Center](https://msdn.microsoft.com/data/ef)

> [!div class="step-by-step"]
> [Vorherige](using-web-api-with-entity-framework-part-6.md)
