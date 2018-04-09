---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-7
title: Erstellen Sie die Sicht (UI) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: b2445062-a1fe-4133-8994-f510280f6d9a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-7
msc.type: authoredcontent
ms.openlocfilehash: 5052d7cca4a5c12a9ea56eb929d4794b19e82603
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-the-view-ui"></a>Erstellen Sie die Sicht (UI)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

In diesem Abschnitt beginnen Sie den HTML-Code für die app definieren und die Datenbindung zwischen den HTML-Code und das Ansichtsmodell hinzufügen.

Öffnen Sie die Datei Views/Home/Index.cshtml. Ersetzen Sie den gesamten Inhalt dieser Datei durch Folgendes.

[!code-cshtml[Main](part-7/samples/sample1.cshtml)]

Die meisten der `div` Elemente müssen für die [Bootstrap](http://getbootstrap.com/) formatieren. Die wichtigen Elemente sind die Personen mit `data-bind` Attribute. Dieses Attribut verknüpft den HTML-Code für das Modell anzeigen.

Zum Beispiel:

[!code-html[Main](part-7/samples/sample2.html)]

In diesem Beispiel wird die &quot; `text` &quot; binden Ursachen der `<p>` Element den Wert der anzuzeigenden der `error` Eigenschaft aus dem Modell anzeigen. Bedenken Sie, dass `error` wurde als deklariert eine `ko.observable`:

[!code-javascript[Main](part-7/samples/sample3.js)]

Wenn ein neuer Wert zugewiesen wird, um `error`, Knockout aktualisiert den Text in die `<p>` Element.

Die `foreach` Bindung weist Knockout so durchlaufen Sie den Inhalt der `books` Array. Für jedes Element im Array Knockout erstellt ein neues &lt;li&gt; Element. Bindungen, die innerhalb des Kontexts der `foreach` verweisen auf Eigenschaften auf das Arrayelement. Zum Beispiel:

[!code-html[Main](part-7/samples/sample4.html)]

Hier die `text` Bindung liest die Author-Eigenschaft, jedes Buch.

Wenn Sie die Anwendung jetzt ausführen, sollten sie wie folgt aussehen:

![](part-7/_static/image1.png)

Die Liste der Bücher lädt asynchron ausgeführt wird, nachdem die Seite geladen. Zurzeit die &quot;Details&quot; Links sind nicht funktionsfähig. Wir werden diese Funktionalität im nächsten Abschnitt hinzufügen.

> [!div class="step-by-step"]
> [Zurück](part-6.md)
> [Weiter](part-8.md)
