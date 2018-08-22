---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Verwenden von ViewData und Implementieren von ViewModel-Klassen | Microsoft-Dokumentation
author: microsoft
description: Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Szenarien, bei der formularbearbeitung, und erörtert die zwei Ansätze, die verwendet werden können, um Daten von Controllern an Ansichten übergeben:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826155"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Verwenden von ViewData und Implementieren von ViewModel-Klassen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 6 der ein kostenloses ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Szenarien, bei der formularbearbeitung, und erörtert die zwei Ansätze, die verwendet werden können, um Daten von Controllern an Ansichten übergeben: "ViewData" und "ViewModel".
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner, Schritt 6: "ViewData" und "ViewModel"

Wir haben eine Reihe von Szenarien für Formular-Post behandelt und erläutert, wie implementieren erstellen, aktualisieren und löschen (CRUD) unterstützt. Wir jetzt unsere Implementierung "dinnerscontroller" weitergehen und Unterstützung für umfangreichere Szenarien bei der formularbearbeitung aktivieren. Dabei beschäftigen wir uns mit zwei Ansätze, die verwendet werden können, um Daten von Controllern an Ansichten übergeben: "ViewData" und "ViewModel".

### <a name="passing-data-from-controllers-to-view-templates"></a>Übergeben von Daten von Controllern an Vorlagen anzeigen

Eine definierende Merkmale des MVC-Musters ist, dass die strikte "Trennung von Belangen" erleichtert, zwischen den verschiedenen Komponenten einer Anwendung zu erzwingen. Modellen, Controllern und Ansichten jedes verfügen über klar definierte Rollen und Zuständigkeiten, und sie kommunizieren untereinander auf gut definierten Weise. Dadurch wird die testbarkeit heraufstufen und die Wiederverwendung von code.

Wenn eine Controllerklasse entscheidet sich, eine HTML-Antwort zurück an einen Client zu rendern, dient er explizit übergeben an die Vorlage anzeigen, die alle Daten benötigt, um die Antwort zu rendern. Anzeigen von Vorlagen sollten niemals Daten abrufen oder eine Anwendung Logik – ausführen und sollten stattdessen beschränken sich selbst, dass nur Code zum Rendern, die das Modell/Daten vom Controller übergebenen gesteuert wird.

Jetzt die Modelldaten durch unsere "dinnerscontroller" übergebenen Klasse, um unsere Ansichtsvorlagen ist einfach und selbsterklärend – eine Liste der Dinner-Objekte im Fall von Index() und einem einzelnen Dinner das Objekt im Fall von Details(), Edit(), Create() und Delete(). Wie wir unsere Anwendung weitere Benutzeroberflächenfunktionen hinzufügen, werden oft wir mehr als nur diese Daten zum Rendern von HTML-Antworten in unsere Ansichtsvorlagen übergeben müssen. Wir möchten beispielsweise ändern Sie das Feld "Land" in unserem bearbeiten und Erstellen von Sichten wird ein HTML-Textfeld zu einem DropDownList-Steuerelement. Anstatt hartcodieren der Dropdown-Liste der Ländernamen in der Vorlage anzeigen möchten wir sie aus einer Liste der unterstützten Länder zu generieren, die wir dynamisch aufzufüllen. Wir benötigen eine Möglichkeit, sowohl die Dinner-Objekt übergeben *und* der Liste der unterstützten Länder, die aus den Controller zu unseren Vorlagen anzeigen.

Wir sehen uns zwei Möglichkeiten, die wir dies erreichen können.

### <a name="using-the-viewdata-dictionary"></a>Verwenden das ViewData-Wörterbuch

Die Basisklasse für Controller verfügbar macht, eine "ViewData"-Wörterbuch-Eigenschaft, die verwendet werden kann, um zusätzliche Datenelemente von Controllern an Ansichten übergeben wird.

Beispielsweise können zur Unterstützung der Szenarios, in dem wir das Textfeld "Land" in unserer Ansicht bearbeiten ändern, wird ein HTML-Textfeld zu einem DropDownList-Steuerelement möchten, wir aktualisieren unsere Edit() Action-Methode, um ein SelectList-Objekt (zusätzlich zu einem Dinner-Objekt) zu übergeben, die als das m verwendet werden kann odell von einer Dropdownlist Ländern.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Der Konstruktor, der die oben genannten SelectList nimmt eine Liste von Bezirken, die zum Auffüllen der Drop-Downlist mit als auch den derzeit ausgewählten Wert.

Wir können dann aktualisieren, dass unsere Vorlage Edit.aspx anzeigen, um die Hilfsmethode Html.DropDownList() anstelle der Html.TextBox()-Hilfsmethode verwenden, die wir zuvor verwendet:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Die oben genannten Html.DropDownList()-Hilfsmethode verwendet zwei Parameter an. Die erste ist der Name des HTML-Formularelements ausgegeben. Die zweite ist das "SelectList"-Modell, das wir über das ViewData-Wörterbuch zu übergeben. Wir verwenden die c# "as"-Schlüsselwort, um der im Wörterbuch als eine SelectList umgewandelt.

Und jetzt ausführen unserer Anwendung und den Zugriff der */Dinners/Edit/1* URL in unserem Browser sehen, dass unsere Bearbeitungsoberfläche zum Anzeigen von einem DropDownList-Steuerelement von Ländern, anstatt ein TextBox-Element aktualisiert wurde:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Da wir die ansichtsvorlage "Bearbeiten" aus der Bearbeiten Sie die HTTP-POST-Methode (in Szenarien, die beim Auftreten von Fehlern) auch rendern, müssen wir möchten sicherstellen, dass wir auch aktualisieren, diese Methode, um "ViewData" der SelectList hinzugefügt wird, wenn die ansichtsvorlage in Fehlerszenarien gerendert wird:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Und unsere "dinnerscontroller" Bearbeiten-Szenario unterstützt jetzt einem DropDownList-Steuerelement.

### <a name="using-a-viewmodel-pattern"></a>Verwenden eine ViewModel-Muster

Der ViewData-Wörterbuch-Ansatz hat den Vorteil, ziemlich schnell und einfach zu implementieren. Einige Entwickler mag nicht mithilfe von Wörterbüchern zeichenfolgenbasierten jedoch, da Tippfehler zu Fehlern führen können, die zum Zeitpunkt der Kompilierung nicht erfasst wird. Das nicht typisierte ViewData-Wörterbuch erfordert auch mithilfe des Operators "als" bzw. eine Umwandlung bei Verwendung einer stark typisierten Sprache wie c#, die in einer Vorlage anzeigen.

Ein alternativer Ansatz, den wir verwenden können, ist einer die oft als das "ViewModel"-Muster bezeichnet. Verwendung dieses Musters erstellen wir die stark typisierte Klassen, die für unseren bestimmten Ansicht-Szenarien optimiert sind, und das Verfügbarmachen der Eigenschaften für das dynamische Werte bzw. den Inhalt von unserem Ansichtsvorlagen benötigt werden. Die Controllerklassen können klicken Sie dann Auffüllen und übergeben diese Klassen Ansicht optimiert, unsere ansichtsvorlage verwenden. Dadurch wird typsicherheit kompilierzeitüberprüfung und -Editor Intellisense in Vorlagen anzeigen.

Beispielsweise, um die Dinner-Formular bearbeitende Szenarien erstellen wir eine "DinnerFormViewModel" wie unten-Klasse zu ermöglichen, die zwei stark typisierte Eigenschaften verfügbar macht: ein Dinner-Objekt, und der SelectList-Modell erforderlich, um die Länder Dropdownlist aufzufüllen:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Wir können dann aktualisieren unserer Edit() Action-Methode zum Erstellen der DinnerFormViewModel unter Verwendung des Dinner-Objekts, die wir in unserem Repository abrufen und übergibt diese dann an unsere Vorlage anzeigen:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Wir senden, und klicken Sie dann Update unsere Vorlage anzeigen, damit die It erwartet, dass eine "DinnerFormViewModel" anstelle von einem "Dinner" ändern Sie das Attribut "erbt" am oberen Rand der Seite "edit.aspx"-Objekt wie folgt:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Sobald wir dies tun, werden Intellisense der "Model"-Eigenschaft in unsere ansichtsvorlage aktualisiert werden, um das Objektmodell des Typs DinnerFormViewModel widerzuspiegeln, die wir es übergeben werden:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Wir können unseren Code anzeigen, funktionieren ab dann aktualisieren. Beachten Sie, dass unten wie wir nicht verändert werden die Namen der Eingabeelemente erstellt wird (das Form-Elemente werden immer noch den Namen "Title", "Land") – aber aktualisieren wir die HTML-Hilfsmethoden zum Abrufen der Werte, die mithilfe der DinnerFormViewModel-Klasse:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Wir aktualisieren zudem unsere bearbeiten-Post-Methode, um die DinnerFormViewModel-Klasse verwenden, beim Rendern von Fehlern:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Wir können auch aktualisieren, unsere Create() Aktionsmethoden Wiederverwendungsrate für genau gleich *DinnerFormViewModel* Klasse, um die Länder DropDownList innerhalb dieser ebenfalls zu aktivieren. Im folgenden sehen Sie die HTTP-GET-Implementierung:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Im folgenden finden Sie die Implementierung das Erstellen von HTTP-POST-Methode:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Und jetzt mit die bearbeiten und Erstellen von Bildschirmen Drop-Downlists unterstützen, für die Entscheidung für des Lands.

### <a name="custom-shaped-viewmodel-classes"></a>Benutzerdefinierte gestalteten ViewModel-Klassen

Im Szenario oben macht unsere DinnerFormViewModel-Klasse direkt Dinner Modellobjekts, das als Eigenschaft zusammen mit einer unterstützenden SelectList Modelleigenschaft verfügbar. Dieser Ansatz funktioniert gut für Szenarien, in dem die HTML-UI, die wir in unserer ansichtsvorlage erstellen möchten relativ am ehesten unsere domänenmodellobjekten entspricht.

Für Szenarien, in denen dies nicht, gilt, eine Option, mit denen Sie eine benutzerdefinierte gestalteten ViewModel-Klasse, deren Objektmodell wird eher für die Nutzung durch die Sicht – optimiert, und sieht die grundlegend von der zugrunde liegenden Modellobjekts Domäne, erstellen. Beispielsweise können sie potenziell unterschiedlichen Eigenschaftsnamen und/oder aggregierte Eigenschaften, die von mehreren Modellobjekte gesammelten verfügbar machen.

Benutzerdefinierte gestalteten ViewModel-Klassen möglich sowohl verwendet, um Daten von Controllern an Ansichten rendern als auch können Sie die Form-Daten zurück an die Aktionsmethode des Controllers gesendet zu übergeben. Weiter unten in diesem Szenario müssen Sie möglicherweise die Aktionsmethode ein "ViewModel"-Objekt mit den Formular-gesendet, Daten aktualisieren und verwenden Sie dann die ViewModel-Instanz zuordnen oder Abrufen eines tatsächlichen Domäne Model-Objekts.

Benutzerdefinierte gestalteten ViewModel-Klassen bieten ein hohes Maß an Flexibilität und gilt es, jederzeit zu forschen, den Renderingcode in Ihrer Vorlagen anzeigen oder den Formular-Posting-Code in Ihre Aktionsmethoden zu kompliziert wird gestartet finden Sie. Dies ist häufig ein Zeichen, dass Ihre Domänenmodelle ordnungsgemäß an die Benutzeroberfläche entsprechen nicht, die Sie generieren und benutzerdefinierte gestalteten "ViewModel"-Klasse helfen kann.

### <a name="next-step"></a>Nächster Schritt

Jetzt betrachten wir Verwendung von Teilansichten und Masterseiten erneut verwenden und die Benutzeroberfläche für die Anwendung freigeben.

> [!div class="step-by-step"]
> [Zurück](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Weiter](re-use-ui-using-master-pages-and-partials.md)
