---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: d019941700992e173a5812b11b558b6726dfd809
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831290"
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js

In diesem Abschnitt verwenden wir "Knockout.js" der administratoransicht Funktionen hinzu.

["Knockout.js"](http://knockoutjs.com/) ist eine Javascript-Bibliothek, die einfach HTML-Steuerelemente an Daten gebunden werden können. "Knockout.js" wird das Model-View-ViewModel (MVVM)-Muster verwendet.

- Die *Modell* ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Produkte und Bestellungen).
- Die *Ansicht* spielt die Darstellungsschicht (HTML).
- Die *Anzeigemodell* ist ein Javascript-Objekt, das Modelldaten enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Es wurde keine Kenntnisse über die HTML-Darstellung. Stattdessen stellt es sich um abstrakte Funktionen in der Ansicht, z. B. "eine Liste von Elementen".

Die Ansicht an das Ansichtsmodell datengebunden ist. Updates an das Ansichtsmodell werden in der Ansicht automatisch wiedergegeben. Das Ansichtsmodell ist außerdem ruft Ereignisse aus der Sicht, z. B. Klicks ab und führt Vorgänge für das Modell, z. B. einen Auftrag zu erstellen.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Zuerst definieren wir das Ansichtsmodell. Danach werden wir das HTML-Markup an das Ansichtsmodell binden.

Fügen Sie den folgenden Razor-Abschnitt, um Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Sie können eine beliebige Stelle in diesem Abschnitt in der Datei hinzufügen. Wenn die Ansicht der Bereich wird am unteren Rand der HTML-Seite gerendert wird, nach rechts vor dem schließenden &lt;/body&gt; Tag.

All das Skript für diese Seite gelangen in die Skript-Tag, die durch den Kommentar angegeben:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Definieren Sie zuerst eine Ansichtsmodell Klasse:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** ist eine besondere Art von Objekt in Knockout bezeichnet ein *Observable*. Von der ["Knockout.js" Dokumentation](http://knockoutjs.com/documentation/observables.html): ein beobachtbares Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann." Wenn der Inhalt der Observable-Objekt ändern, wird die Ansicht automatisch entsprechend aktualisiert.

Zum Auffüllen der `products` Arrays, eine AJAX-Anforderung an die Web-API vornehmen. Denken Sie daran, dass wir den Basis-URI für die API in der ansichtsbehälter gespeichert (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Lernprogramms).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Als Nächstes fügen Sie Funktionen hinzu, an das Ansichtsmodell erstellen, aktualisieren und Löschen von Produkten. Diese Funktionen AJAX-Aufrufe an die Web-API zu übermitteln, und verwenden Sie die Ergebnisse des anzeigemodells zu aktualisieren.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Jetzt der wichtigste Teil: Wenn das DOM ist fulled geladen und Aufruf der **ko.applyBindings** Funktion, und übergeben Sie eine neue Instanz der der `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Die **ko.applyBindings** Methode Knockout aktiviert und für die Ansicht das Ansichtsmodell verbindet.

Nun, wir ein Ansichtsmodell haben, können wir die Bindungen erstellen. In "Knockout.js", erreichen Sie dies durch Hinzufügen von `data-bind` Attribute auf HTML-Elemente. Um eine HTML-Liste in ein Array zu binden, verwenden Sie z. B. die `foreach` Bindung:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Die `foreach` Bindung durchläuft das Array und erstellt Sie untergeordnete Elemente für jedes Objekt im Array. Bindungen an die untergeordneten Elemente können auf Eigenschaften für die Arrayobjekte verweisen.

Fügen Sie der Liste "Update-Produkte" die folgenden Bindungen hinzu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Die `<li>` Element innerhalb des Bereichs befindet, die **Foreach** Bindung. Bedeutet, dass Knockout wird das Element einmal für jedes Produkt im Rendern der `products` Array. Alle Bindungen innerhalb der `<li>` -Element auf die produktinstanz verweisen. Z. B. `$data.Name` bezieht sich auf die `Name` Eigenschaft für das Produkt.

Verwenden Sie zum Festlegen der Werte, der die Texteingaben der `value` Bindung. Die Schaltflächen an Funktionen gebunden sind, in der Modell-Ansicht mit den `click` Bindung. Der Product-Instanz wird für jede Funktion als Parameter übergeben. Weitere Informationen die ["Knockout.js" Dokumentation](http://knockoutjs.com/documentation/observables.html) gute Beschreibungen der verschiedenen Bindungen hat.

Fügen Sie eine Bindung für die **übermitteln** Ereignis auf dem Formular Produkt hinzufügen:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Diese Bindung ruft die `create` Funktion im Ansichtsmodell, beim Erstellen eines neuen Produkts.

Hier ist der vollständige Code für das anzeigen (Administrator):

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Führen Sie die Anwendung aus, melden Sie sich mit dem Administratorkonto an, und klicken Sie auf den Link "Admin". Sie sollten finden Sie unter der Liste der Produkte und in der Lage zu erstellen, aktualisieren oder Löschen von Produkten.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)
