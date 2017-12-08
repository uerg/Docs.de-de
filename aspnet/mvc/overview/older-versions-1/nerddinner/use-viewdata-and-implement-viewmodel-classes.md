---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Verwendung ViewData und ViewModel-Klassen implementieren | Microsoft Docs
author: microsoft
description: "Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Formular Szenarien, bearbeiten und erläutert außerdem zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben:..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 36b9e87cc24f74f7f2cc592afb5102709b598f74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>ViewData verwenden und implementieren ViewModel-Klassen
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 6 mit einer kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 6 zeigt wie Aktivieren der Unterstützung für umfangreichere Formular Szenarien, bearbeiten und erläutert außerdem zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben: ViewData und ViewModel.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner Schritt 6: ViewData und ViewModel

Wir haben eine Reihe von Formular-Post-Szenarien abgedeckt und Implementierung erläutert erstellen, aktualisieren und löschen (CRUD) unterstützt. Wir nun unsere DinnersController Implementierung weitere ergreifen und Aktivieren der Unterstützung für umfangreichere Formular Szenarien bearbeiten. Während der Durchführung dieser besprechen wir zwei Ansätze, die verwendet werden können, um Daten aus der Controller mit Ansichten zu übergeben: ViewData und ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Übergeben von Daten von Domänencontrollern an Vorlagen anzeigen

Keines der definierenden Eigenschaften das MVC-Schema handelt es sich um die strikte "Trennung von Anliegen" sie hilft bei der zwischen den verschiedenen Komponenten einer Anwendung zu erzwingen. Modelle, Controller und Ansichten jedes gut definiert und Rollen und Zuständigkeiten und Kommunikation untereinander in gut definierte Weise. Dadurch können höher stufen Prüfbarkeit und Wiederverwendung von code.

Wenn eine Controllerklasse entscheidet, eine HTML-Antwort an den Client zu rendern, wird die verantwortlich für die Übergabe explizit an die Vorlage Anzeigen aller Daten benötigt, um die Antwort zu rendern. Ansichtsvorlagen keine Daten abrufen oder eine Anwendung die Logik – sollten nie ausführen und sollten stattdessen einschränken selbst, um nur Renderingcodes verfügen, die aus den Modelldaten/vom Controller übergebenen gesteuert wird.

Jetzt die Modelldaten durch unsere DinnersController übergebenen Klasse, um unsere Ansichtsvorlagen ist einfach und selbsterklärend – im Falle von Details(), Edit(), Create()- und Delete() eine Liste von Dinner Objekten im Fall von Index() und eine einzelne Dinner Objekt. Wenn wir unsere Anwendung weitere Benutzeroberflächenfunktionen hinzufügen, werden häufig wir müssen mehr als nur diese Daten für das Rendern von HTML-Antworten in unserer Ansichtsvorlagen übergeben. Wir möchten beispielsweise ändern Sie das Feld "Country" in unseren bearbeiten und Erstellen von Sichten wird ein HTML-Textfeld zu einer Dropdownlist. Anstatt hartcodieren der Dropdown-Liste Ländernamen in der Vorlage anzeigen sollten wir es aus einer Liste mit unterstützten Ländern zu generieren, die wir dynamisch aufgefüllt werden. Wir benötigen eine Möglichkeit, sowohl die Dinner-Objekt übergeben *und* die Liste der unterstützten Ländern aus unserem Controller zu unserem Vorlagen anzeigen.

Betrachten wir zwei Möglichkeiten, die wir dies zu erreichen können.

### <a name="using-the-viewdata-dictionary"></a>Verwenden das ViewData-Wörterbuch

Die Basisklasse für Controller stellt die Wörterbucheigenschaft "ViewData", die verwendet werden kann, um zusätzliche Datenelemente aus Controller mit Ansichten zu übergeben.

Beispielsweise können zur Unterstützung des Szenarios, in dem wir das Textfeld "Country" in unseren Bearbeitungsansicht ändern, wird ein HTML-Textfeld zu einer Dropdownlist möchten, wir aktualisieren unsere Edit()-Aktionsmethode, um ein SelectList-Objekt (zusätzlich zum ein Dinner-Objekt) zu übergeben, die als das m verwendet werden können Odel der Dropdownlist Ländern.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Der Konstruktor für die oben genannten SelectList nimmt eine Liste der Landkreise zum Auffüllen der Drop-Downlist mit als auch den derzeit ausgewählten Wert.

Wir können dann aktualisieren, dass unsere Vorlage Edit.aspx anzeigen, um die Hilfsmethode Html.DropDownList() statt die Hilfsmethode Html.TextBox() zu verwenden, die wir zuvor verwendet:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Die oben genannten Html.DropDownList() Hilfsmethode akzeptiert zwei Parameter. Die erste ist der Name des HTML-Formularelements ausgegeben. Das zweite ist das "SelectList"-Modell, das es über das ViewData-Wörterbuch übergeben. Wird der C#-"as"-Schlüsselwort verwendet, der im Wörterbuch als eine SelectList umgewandelt.

Und nun wir Ausführung von unserem Anwendung und den Zugriff der */Dinners/Edit/1* URL innerhalb dieser Browser wir sehen, dass unsere Bearbeitungsoberfläche aktualisiert wurde, und eine Dropdownlist Länder oder Regionen, statt ein Textfeld angezeigt:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Da wir auch zu Rendern der Vorlage bearbeiten anzeigen, von der HTTP-POST bearbeiten-Methode (in Fällen, bei Auftreten eines Fehlers), benötigen wir sicherstellen, dass wir auch diese Methode, um das Hinzufügen der SelectList ViewData beim Rendern der Vorlage anzeigen in Fehlerszenarien aktualisieren:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Und nun unterstützt unsere DinnersController bearbeiten Szenario eine DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Verwenden ein ViewModel-Muster

Der ViewData Wörterbuch Ansatz hat den Vorteil, relativ schnell und einfach zu implementieren. Einige Entwickler nicht gefällt zeichenfolgenbasierte Wörterbücher verwenden, da Tippfehler zu Fehlern führen können, die zum Zeitpunkt der Kompilierung nicht abgefangen wird. Nicht typisierte ViewData-Wörterbuch ist auch mithilfe des Operators "als" oder bei Verwendung in einer Vorlage für die Sicht eine stark typisierte Sprache wie C#-Umwandlung erforderlich.

Eine alternative Methode, die wir verwenden könnten ist häufig als "ViewModel" Muster bezeichnet. Bei Verwendung dieses Muster erstellen wir stark typisierter Klassen, die für unsere bestimmte Ansicht Szenarien optimiert sind und die Eigenschaften für die dynamischen Werte/Content durch unsere Ansichtsvorlagen erforderlich machen. Unsere Controllerklassen können Auffüllen und diese Ansicht optimiert Klassen an unsere anzeigen, die zu verwendende Vorlage übergeben. Dies ermöglicht typsicherheit, kompilierzeitüberprüfung und -Editor Intellisense in Vorlagen anzeigen.

Um beispielsweise Dinner Formular Bearbeitung Szenarien erstellen wir eine "DinnerFormViewModel" wie im folgenden-Klasse zu ermöglichen, die zwei stark typisierte Eigenschaften verfügbar macht: ein Dinner-Objekt, und der SelectList-Modell erforderlich, um die Länder Dropdownlist aufzufüllen:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Wir können dann aktualisieren unsere Edit() Aktionsmethode zum Erstellen der DinnerFormViewModel unter Verwendung des Dinner-Objekts, die wir aus unserer Repository abrufen, und übergeben Sie ihn an unsere Vorlage anzeigen:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Wir senden dann aktualisieren, die unsere Vorlage anzeigen, damit die It "DinnerFormViewModel" anstelle einer "Dinner" erwartet-Objekt nach dem Ändern des Attributs "erbt" am oberen Rand der Seite "edit.aspx" wie folgt:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Sobald wir dies tun, werden Intellisense der Eigenschaft "Model" in unseren Vorlage anzeigen aktualisiert werden, um das Objektmodell des Typs DinnerFormViewModel wider, die wir ihn übergeben werden:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Wir können unsere Code anzeigen aus sie arbeiten dann aktualisieren. Beachten Sie, dass unten wie wir nicht ändern werden die Namen der Eingabeelemente erstellt wird (der Formularelemente noch den Namen "Title", "Land") – aber aktualisieren wir die HTML-Hilfsmethoden zum Abrufen der Werte, die mit der DinnerFormViewModel-Klasse:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Wir werden unsere bearbeiten Post-Methode zur Verwendung der DinnerFormViewModel-Klasse, beim Rendern der Fehler auch aktualisieren:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Wir können auch aktualisieren unsere Create()-Aktionsmethoden, die genaue erneut zu verwenden dieselbe *DinnerFormViewModel* Klasse Länder und Regionen DropDownList innerhalb dieser ebenfalls zu aktivieren. Im folgenden sehen Sie die HTTP-GET-Implementierung:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Im folgenden sehen Sie die Implementierung der Methode HTTP-POST zu erstellen:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Und jetzt die Bildschirmen bearbeiten und erstellen Drop Downlists für das Land Entnahme unterstützen.

### <a name="custom-shaped-viewmodel-classes"></a>Benutzerdefinierte geformten ViewModel-Klassen

Im obigen Szenario macht das Modellobjekt Dinner als Eigenschaft zusammen mit einer unterstützenden SelectList Modelleigenschaft direkt unsere DinnerFormViewModel-Klasse verfügbar. Dieser Ansatz funktioniert gut für Szenarien, in dem die HTML-UI, die wir in unserer Ansichtenvorlage erstellen möchten relativ am ehesten unsere domänenmodellobjekten entspricht.

Ist für Szenarien, in denen dies nicht, zutrifft, wird eine Option, die Sie verwenden können zum Erstellen einer benutzerdefinierten geformten ViewModel-Klasse, deren Objektmodell ist mehr für die Nutzung durch die Sicht – optimiert und kann die grundlegend von der zugrunde liegenden Modellobjekts Domäne aussehen. Beispielsweise konnte es anderen Eigenschaftennamen und/oder Aggregieren Eigenschaften, die aus mehreren Modellobjekte gesammelt ausgesetzt.

Benutzerdefinierte geformten ViewModel-Klassen kann sowohl zum Übergeben von Daten von Domänencontrollern mit Ansichten zu rendern sowie Formulardaten, die an eine Controlleraktionsmethode zurückgesendet Bewältigung beitragen verwendet. Weiter unten in diesem Szenario müssen Sie möglicherweise die Aktionsmethode ein ViewModel-Objekt mit den Daten senden Formulars aktualisieren, und verwenden Sie das ViewModel-Instanz zuordnen oder eine tatsächliche Modell Domänenobjekt abrufen.

Benutzerdefinierte geformten ViewModel-Klassen bieten ein hohes Maß an Flexibilität und etwas zu einem beliebigen Zeitpunkt zu forschen, die Sie Renderingcodes innerhalb Ihrer Vorlagen anzeigen oder den Formular-Buchung Code innerhalb der Aktionsmethoden zu kompliziert abzurufenden ab finden sind. Dies ist häufig ein Anzeichen, dass Ihre Domänenmodelle ordnungsgemäß an die Benutzeroberfläche nicht übereinstimmen, die Sie generieren und eine benutzerdefinierte geformten ViewModel Zwischenklasse helfen kann.

### <a name="next-step"></a>Nächster Schritt

Jetzt betrachten wir Verwendung Replikatsgruppenelemente und Master-Seiten erneut verwenden und die Benutzeroberfläche von der Anwendung gemeinsam genutzt.

>[!div class="step-by-step"]
[Zurück](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Weiter](re-use-ui-using-master-pages-and-partials.md)
