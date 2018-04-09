---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
title: 'Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 9d9cb3b0-f4a7-434e-a508-9fc0ad0eb813
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-5
msc.type: authoredcontent
ms.openlocfilehash: b63446d076fbb1143641dead788042967b996bf8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-5-creating-a-dynamic-ui-with-knockoutjs"></a>Teil 5: Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-a-dynamic-ui-with-knockoutjs"></a>Erstellen einer dynamischen Benutzeroberfläche mit Knockout.js

In diesem Abschnitt werden wir Knockout.js verwenden, um die Funktionalität der Admin-Ansicht hinzufügen.

[Knockout.js](http://knockoutjs.com/) ist eine Javascript-Bibliothek, die vereinfacht, HTML-Steuerelementen an Daten gebunden werden. Knockout.js verwendet das Model-View-ViewModel (MVVM)-Muster.

- Die *Modell* ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Products und Orders).
- Die *Ansicht* ist die Darstellungsschicht (HTML).
- Die *ViewModel* ist ein Javascript-Objekt, das die Modelldaten enthält. Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche. Sie hat keine Kenntnis der HTML-Darstellung. Stattdessen repräsentiert die abstrakte Funktionen von der Sicht, z. B. "eine Liste von Elementen".

Die Ansicht für das Ansichtsmodell datengebunden ist. Updates für das ViewModel werden automatisch in der Sicht widergespiegelt. Das Ansichtsmodell auch ruft Ereignisse aus der Sicht, z. B. auf eine Schaltfläche, ab und führt Vorgänge für das Modell, z. B. das Erstellen einer Bestellung.

![](using-web-api-with-entity-framework-part-5/_static/image1.png)

Definieren zunächst das Ansichtsmodell. Danach werden wir das HTML-Markup für das Ansichtsmodell binden.

Fügen Sie den folgenden Razor-Abschnitt, um Admin.cshtml:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample1.cshtml)]

Sie können eine beliebige Stelle in diesem Abschnitt in der Datei hinzufügen. Wenn die Ansicht der Abschnitt wird am unteren Rand der HTML-Seite angezeigt gerendert wird, mit der rechten Maustaste vor dem abschließenden &lt;/body&gt; Tag.

Alle des Skripts für diese Seite wird innerhalb des Kommentars erkennbar Skripttags gesendet:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample2.html)]

Zuerst definieren Sie eine Ansichtsmodell Klasse:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample3.js)]

**ko.observableArray** ist eine besondere Art von Objekt in Knockout, bezeichnet ein *Observable*. Aus der [Knockout.js Dokumentation](http://knockoutjs.com/documentation/observables.html): Observable-Objekt ist ein "JavaScript-Objekt, mit denen Abonnenten über Änderungen benachrichtigt werden kann." Wenn der Inhalt der Observable-Objekt ändern, wird die Ansicht automatisch entsprechend aktualisiert.

Zum Auffüllen der `products` Arrays, nehmen Sie eine AJAX-Anforderung an die Web-API. Beachten Sie, dass wir den Basis-URI für die API in den ansichtsbehälter gespeichert (siehe [Teil 4](using-web-api-with-entity-framework-part-4.md) des Lernprogramms).

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample4.js?highlight=5)]

Fügen Sie anschließend die Funktionen zum Erstellen, aktualisieren und Löschen von Produkten Modell anzeigen. Diese Funktionen AJAX-Aufrufe an die Web-API senden, und verwenden die Ergebnisse der anzeigen-Modell zu aktualisieren.

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample5.js?highlight=7)]

Jetzt die wichtigste Schritt: Wenn das DOM ist fulled geladen und Aufruf der **ko.applyBindings** Funktion, und übergeben Sie eine neue Instanz der der `ProductsViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-5/samples/sample6.js)]

Die **ko.applyBindings** Methode Knockout aktiviert und auf die Ansicht das Ansichtsmodell verbindet.

Nun mit dem Modell anzeigen, können wir die Bindungen zu erstellen. In Knockout.js, Sie hierzu fügen `data-bind` -Attribute verwenden, um HTML-Elemente. Beispielsweise verwenden, um eine HTML-Liste in ein Array zu binden, die `foreach` Bindung:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample7.html?highlight=1)]

Die `foreach` Bindung durchläuft das Array und erstellt Sie untergeordnete Elemente für jedes Objekt im Array. Bindungen für die untergeordneten Elemente verweisen auf Eigenschaften auf das Array von Objekten.

Fügen Sie der Liste "Updateprodukten" die folgenden Bindungen hinzu:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample8.html)]

Die `<li>` Element befindet sich innerhalb des Bereichs der **Foreach** Bindung. Dass bedeutet, dass Knockout wird das Element einmal für jedes Produkt in gerendert werden die `products` Array. Alle Bindungen innerhalb der `<li>` Element verweisen auf diese produktinstanz. Beispielsweise `$data.Name` bezieht sich auf die `Name` auf der Produkt-Eigenschaft.

Verwenden Sie zum Festlegen der Werte von der Texteingaben der `value` Bindung. Die Schaltflächen an Funktionen gebunden sind, auf die Modellansicht mithilfe der `click` Bindung. Der Product-Instanz wird für jede Funktion als Parameter übergeben. Weitere Informationen die [Knockout.js Dokumentation](http://knockoutjs.com/documentation/observables.html) gute Beschreibungen der verschiedenen Bindungen hat.

Fügen Sie eine Bindung für die **übermitteln** Ereignis auf dem Formular Produkt hinzufügen:

[!code-html[Main](using-web-api-with-entity-framework-part-5/samples/sample9.html)]

Diese Bindung ruft die `create` Funktion auf das Ansichtsmodell, erstellen Sie ein neues Produkt.

Hier wird der vollständige Code für die Admin-Ansicht:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-5/samples/sample10.cshtml)]

Führen Sie die Anwendung, melden Sie sich mit dem Administratorkonto an, und klicken Sie auf den Link "Admin". Sie sollten finden Sie in der Liste der Produkte und in der Lage, erstellen, aktualisieren oder Löschen von Produkten.

> [!div class="step-by-step"]
> [Zurück](using-web-api-with-entity-framework-part-4.md)
> [Weiter](using-web-api-with-entity-framework-part-6.md)
