---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen Sie die Sicht (UI) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: eeaa36ede45b82afd112a270acf68105d1ae6dbc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820985"
---
<a name="create-the-view-ui"></a>Erstellen Sie die Sicht (UI)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

In diesem Abschnitt Starten Sie den HTML-Code für die app zu definieren und die Datenbindung zwischen der HTML-Code und das Ansichtsmodell hinzufügen.

Öffnen Sie die Datei Views/Home/Index.cshtml. Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Die meisten der `div` Elemente sind für [Bootstrap](http://getbootstrap.com/) formatieren. Die wichtigen Elemente sind mit `data-bind` Attribute. Dieses Attribut verknüpft den HTML-Code an das Ansichtsmodell.

Zum Beispiel:

[!code-html[Main](part-7/samples/sample2.html)]

In diesem Beispiel die &quot; `text` &quot; Bindung bewirkt, dass die `<p>` Element den Wert der anzuzeigenden der `error` Eigenschaft aus dem Anzeigemodell. Zur Erinnerung: `error` deklariert wurde, als eine `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Jedes Mal, wenn ein neuer Wert zugewiesen wird, um `error`, aktualisiert Knockout des Texts in der `<p>` Element.

Die `foreach` Bindung weist Knockout so durchlaufen Sie den Inhalt der `books` Array. Knockout erstellt für jedes Element im Array, ein neues &lt;li&gt; Element. Bindungen, die innerhalb des Kontexts der `foreach` verweisen auf Eigenschaften für das Arrayelement. Zum Beispiel:

[!code-html[Main](part-7/samples/sample4.html)]

Hier die `text` Bindung liest die Author-Eigenschaft für jedes Buch.

Wenn Sie die Anwendung jetzt ausführen, sollte es wie folgt aussehen:

![](part-7/_static/image1.png)

Die Liste der Bücher lädt asynchron, nachdem die Seite geladen werden. Momentan die &quot;Details&quot; Links sind nicht funktionsfähig. Wir fügen diese Funktionalität im nächsten Abschnitt.

> [!div class="step-by-step"]
> [Zurück](part-6.md)
> [Weiter](part-8.md)
