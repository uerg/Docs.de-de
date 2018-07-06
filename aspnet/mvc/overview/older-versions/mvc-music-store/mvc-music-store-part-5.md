---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Teil 5: Von Bearbeitungsformularen und Vorlagen | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 5 behandelt das von Bearbeitungsformularen und Vorlagen.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: acb4a4c427870e375ff19823f0bdfa208438e899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836000"
---
<a name="part-5-edit-forms-and-templating"></a>Teil 5: Von Bearbeitungsformularen und Vorlagen
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 5 behandelt das von Bearbeitungsformularen und Vorlagen.


Im letzten Kapitel waren wir Laden von Daten aus der Datenbank und anzuzeigen. In diesem Kapitel werden wir auch aktivieren, Bearbeiten der Daten.

## <a name="creating-the-storemanagercontroller"></a>Erstellen die StoreManagerController

Wir erstellen Sie zunächst einen neuen Controller namens **StoreManagerController**. Für diesen Controller werden wir die Gerüstbau-Funktionen, die von der ASP.NET MVC 3 Tools Update nutzen. Legen Sie die Optionen für das Dialogfeld "Controller hinzufügen" aus, wie unten dargestellt.

![](mvc-music-store-part-5/_static/image1.png)

Wenn Sie die Schaltfläche "hinzufügen" klicken, sehen Sie sich, dass der ASP.NET MVC 3-Gerüstbau eine gute Menge an Arbeit für Sie übernimmt:

- Erstellen der neuen StoreManagerController mit einer lokalen Variablen von Entity Framework
- Ordner "Views" des Projekts hinzugefügt einen Ordner StoreManager
- Es fügt Create.cshtml, Delete.cshtml, Details.cshtml, Edit.cshtml und "Index.cshtml" Ansicht stark typisiert auf die Album-Klasse

![](mvc-music-store-part-5/_static/image2.png)

Die neue Controller-Klasse für den StoreManager enthält CRUD (erstellen, lesen, aktualisieren und löschen) Controlleraktionen, die weiß, wie das Album zusammenarbeiten Modellklasse und unsere Entity Framework-Kontext für den Datenbankzugriff verwendet.

## <a name="modifying-a-scaffolded-view"></a>Das Ändern einer erstellten Sicht

Es ist wichtig zu beachten, dass während dieser Code für Sie generiert wurde, ist es ASP.NET MVC-Standardcode, wie wir in diesem Tutorial geschrieben haben. Es sollen Sie sparen Zeit damit, würde Controller Codebausteine schreiben und erstellen manuell die stark typisierten Ansichten, dies ist jedoch die Art des generierten Codes Sie sicherlich bemerkt haben, die Voranstellung ernsthafte Warnungen in Kommentaren, wie Sie ändern darf nicht das Code. Dies ist Ihr Code ein, und Ihnen wird erwartet, um ihn zu ändern.

Also, wir beginnen mit der eine schnelle Änderung an den StoreManager Index (/ Views/StoreManager/Index.cshtml). Diese Ansicht zeigt eine Tabelle die Alben in unseres neuen Speichers mit bearbeiten listet / Details / Löschen von Verknüpfungen und öffentlichen Eigenschaften des Albums enthält. Wir entfernen das Feld AlbumArtUrl, da er nicht in dieser Anzeige sehr nützlich ist. In &lt;Tabelle&gt; Abschnitt des Codes Ansicht Entfernen der &lt;th&gt; und &lt;td&gt; AlbumArtUrl Verweise, um, wie durch die hervorgehobenen Zeilen, die folgenden Elemente:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Der Code für die veränderten wird wie folgt aussehen:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Ein erster Blick auf die Store Manager

Nun führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager /. Der Store Manager-Index nur mit einer Liste der Alben im Store mit Links zu bearbeiten, Details, und Löschen von geändert, wird angezeigt.

![](mvc-music-store-part-5/_static/image3.png)

Klicken Sie auf den Link "Bearbeiten" zeigt ein Formular zum Bearbeiten mit Feldern für das Album, einschließlich der Dropdownmenüs für "Genre" und Künstler diese geändert hat.

![](mvc-music-store-part-5/_static/image4.png)

Klicken Sie auf den Link "Zurück zur Liste" am unteren Rand, und klicken Sie dann auf den Link "Details" für ein Album. Dies zeigt detaillierte Informationen für ein einzelner Album.

![](mvc-music-store-part-5/_static/image5.png)

In diesem Fall klicken Sie auf der Rückseite Liste-Link, und klicken Sie dann auf einen Link "löschen". Dies zeigt ein Bestätigungsdialogfeld mit genaueren Informationen Album und gefragt, ob wir sind Sie sicher, dass wir sie löschen möchten.

![](mvc-music-store-part-5/_static/image6.png)

Klicken Sie auf die Schaltfläche "löschen" im unteren Bereich wird das Album zu löschen und gelangen Sie wieder in die Indexseite, die das Album gelöscht wird.

Fertig nicht mit dem Store-Manager, aber wir über eine funktionierende Controller und Ansichtscode für die CRUD-Vorgänge zum Starten von verfügen.

## <a name="looking-at-the-store-manager-controller-code"></a>Der Store Manager-controllercode ansehen

Der Store Manager-Controller enthält eine gute Menge an Code. Schauen Sie wir dies von oben nach unten. Der Controller enthält einige standard-Namespaces für einen MVC-Controller als auch einen Verweis auf unsere Modelle-Namespace. Der Controller hat eine private Instanz des MusicStoreEntities, für den Datenzugriff von jedem der Controlleraktionen verwendet.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Store Manager-Index und Details-Aktionen

Ansicht "Index" Ruft eine Liste der Alben, einschließlich des Albums auf die verwiesen wird "Genre" und Interpreten, wie wir bereits gesehen haben, bei der Arbeit an den Store durchsuchen-Methode ab. Ansicht "Index" folgt, damit sie jedes Album der Name des Genres und Namen der Interpreten, anzeigen kann, damit der Controller effizienter und Abfragen dieser Informationen in der ursprünglichen Anforderung wird die Verweise auf die verknüpften Objekte.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Details-Controlleraktion des Controllers StoreManager funktioniert genauso wie die Details der Store-Controller-Aktion, die wir geschrieben bislang haben - Abfragen für das Album durch die ID, die mit der Find()-Methode, sendet Sie dann zurück zur Ansicht.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Das Erstellen von Aktionsmethoden

Die erstellen-Aktionsmethoden sind etwas anders aus, die wir bisher gesehen haben, da sie die Formulareingabe behandeln. Wenn ein Benutzer zuerst /StoreManager/erstellen/besucht werden sie ein leeres Formular angezeigt. Diese HTML-Seite enthält eine &lt;Formular&gt; -Element, das Dropdownfeld und das Textfeld enthält Eingabeelement in dem sie das Album Details eingeben können.

Nachdem der Benutzer die Formularwerte Album ausfüllt, können sie die Schaltfläche "Speichern" zu senden, dass diese Änderungen für unsere Anwendung, die in der Datenbank speichern zurück drücken. Wenn der Benutzer drückt die Schaltfläche "Speichern" die &lt;Formular&gt; führt eine HTTP-POST an die /StoreManager/erstellen/URL aus und übermitteln der &lt;Formular&gt; Werte als Teil der HTTP-POST-Anforderung.

ASP.NET MVC ermöglicht uns, die die Logik dieser beiden Szenarien der URL-Aufruf problemlos aufteilen, indem ermöglicht uns, implementieren Sie zwei separate "Erstellen" Aktionsmethoden in unserem StoreManagerController-Klasse: eine zum Verarbeiten der anfänglichen HTTP-GET navigieren Sie zu der /StoreManager/Create / URL, und die andere zum Verarbeiten von HTTP-POST für die übermittelten Änderungen.

### <a name="passing-information-to-a-view-using-viewbag"></a>Übergeben von Informationen zu einer Ansicht mithilfe von "ViewBag"

Wir haben die ViewBag-Klasse weiter oben in diesem Tutorial verwendet, aber noch nicht viel darüber gesprochen. Die ViewBag-Klasse ermöglicht uns, die Informationen an die Ansicht zu übergeben, ohne Verwendung eines stark typisierten Model-Objekts. In diesem Fall unsere Bearbeiten von HTTP-GET-Controlleraktion sowohl eine Liste der Genres und Interpreten auf das Formular aus, um die aufzufüllen übergeben muss, und die einfachste Möglichkeit hierzu ist, um als "ViewBag" Elemente zurückzugeben.

Die ViewBag-Klasse ist ein dynamisches Objekt, was bedeutet, dass Sie ViewBag.Foo oder ViewBag.YourNameHere eingeben können, ohne das Schreiben von Code, um diese Eigenschaften zu definieren. In diesem Fall verwendet der controllercode ViewBag.GenreId und ViewBag.ArtistId, damit die Dropdown-Werte, die mit dem Formular übermittelt werden, GenreId und ArtistId, sind die Album-Eigenschaften, die sie festlegen.

Diese Dropdownliste Werte werden zurückgegeben, auf das Formular unter Verwendung der SelectList-Objekts, die nur für diesen Zweck erstellt wird. Dies erfolgt mithilfe von Code wie folgt:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Wie Sie aus Code der Aktion sehen können, sind drei Parameter verwendet wird, dieses Objekt erstellt:

- Die Liste der Elemente aus die Dropdownliste anzeigen lassen. Beachten Sie, dass dies nicht bloß eine Zeichenfolge ist: eine Liste der Genres übergeben.
- Der nächste Parameter, die an die SelectList übergeben werden, ist der Wert der ausgewählten. Dieser wie der SelectList weiß, wie ein Element in der Liste vorab ausgewählt werden kann. Dies ist einfacher zu verstehen, wenn wir das Bearbeitungsformular handelt es sich recht ähnlich betrachten wird.
- Der letzte Parameter ist die Eigenschaft, die angezeigt werden. In diesem Fall ist dies an, dass die Genre.Name-Eigenschaft ist, was dem Benutzer angezeigt wird.

Mit Denken Sie daran, klicken Sie dann die Aktion zum Erstellen von HTTP-GET ist ziemlich einfach: zwei SelectLists die ViewBag-Klasse hinzugefügt werden, und kein Modellobjekt ist (da sie noch nicht erstellt wurde) an das Formular übergeben.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML-Hilfsprogramme, die Drop-Elemente in der Create View angezeigt

Da wir eingegangen sind, zu wie das Dropdown-Werte an die Ansicht übergeben werden, werfen wir einen kurzen Blick auf die Ansicht, um herauszufinden, wie diese Werte angezeigt werden. In den Anzeigecode (/ Views/StoreManager/Create.cshtml), sehen Sie den folgende Aufruf erfolgt, zum Anzeigen der Dropdownliste "Genre" nach unten.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Dies wird als eine HTML-Hilfsobjekt - eine Utility-Methode bezeichnet, die eine häufige Aufgabe für die Sicht ausführt. HTML-Hilfsmethoden sind hilfreich beim Schutz von unserem Anzeigecode, präziser und besser lesbar. Das Hilfsprogramm Html.DropDownList wird von ASP.NET MVC bereitgestellt, wie wir später sehen werden es ist jedoch möglich, eigene Hilfsprogramme für Ansichtscode zu erstellen, die wir in unserer Anwendung wiederverwendet werden.

Der Aufruf Html.DropDownList muss lediglich zwei Dinge: mitgeteilt werden, wenn zum Abrufen der Liste aus, um anzuzeigen, und welchen Nutzen (sofern vorhanden) vorab ausgewählt werden soll. Der erste Parameter, GenreId, teilt der Dropdownliste einen Wert, der mit dem Namen GenreId im Modell oder "ViewBag" gesucht. Der zweite Parameter wird verwendet, an den Wert aus, um anzuzeigen, wie es ursprünglich in der Dropdownliste ausgewählt. Da diese Form einen erstellen-Form ist, wird kein Wert vorab ausgewählten sein, und es wird String.Empty übergeben.

### <a name="handling-the-posted-form-values"></a>Umgang mit den Werten Formular gesendet

Wie bereits erwähnt, gibt es zwei Aktionsmethoden, die jedes Formular zugeordnet sind. Der erste verarbeitet die HTTP-GET-Anforderung und zeigt das Formular. Die zweite behandelt, die HTTP-POST-Anforderung, die die übermittelten Formularwerte enthält. Beachten Sie, dass die Controlleraktion verfügt über ein [HttpPost]-Attribut, das ASP.NET MVC teilt mit, dass es nur auf HTTP-POST-Anforderungen reagieren soll.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Diese Aktion hat vier Aufgaben:

- 1. Lesen Sie die Formularwerte
- 2. Überprüfen Sie, ob die Formularwerte alle Überprüfungsregeln zu erfüllen
- 3. Wenn die Übermittlung des Formulars gültig ist, speichern Sie die Daten, und zeigen Sie die aktualisierte Liste
- 4. Wenn die Übermittlung des Formulars nicht gültig ist, erneut anzuzeigen Sie das Formular mit Validierungsfehlern.

#### <a name="reading-form-values-with-model-binding"></a>Lesen der Formularwerte mit Modellbindung

Die Controlleraktion wird eine formularübertragung verarbeitet, die Werte für GenreId und ArtistId (aus der Dropdown-Liste) und Textbox-Werte für Titel, Preis und AlbumArtUrl enthält. Es ist, zwar möglich, direkt auf Formularwerte zugreifen ist ein besserer Ansatz zum Verwenden der Modellbindung-Funktionen in ASP.NET MVC integriert. Wenn eine Controlleraktion einen Modelltyp als Parameter akzeptiert, versucht ASP.NET MVC, ein Objekt dieses Typs mit formeingaben (sowie Routen- und Querystring-Werte) zu füllen. Dies geschieht durch die Suche nach Werten, deren Namen entsprechen die Eigenschaften des Model-Objekts, z. B. Wenn Sie den neuen Albums Wert des Objekts GenreId festlegen zu können, sieht Sie für eine Eingabe mit dem Namen GenreId. Beim Erstellen von Ansichten, die mit den standard-Methoden in ASP.NET MVC, werden die Formulare immer mithilfe von Eigenschaftsnamen als Eingabe Feldnamen, daher dies die Feldnamen nur, Sie überein werden gerendert werden.

#### <a name="validating-the-model"></a>Beim Validieren des Modells

Das Modell wird durch einen einfachen Aufruf ModelState.IsValid überprüft. Wir noch keine Validierungsregeln auf unsere Klasse Album hinzugefügt noch - machen wir, die in ein wenig – also jetzt diese Überprüfung viel zu tun haben nicht. Wichtig ist, dass diese Überprüfung ModelStat.IsValid den Validierungsregeln anpassen, den wir in unserem Modell ein, damit zukünftige Änderungen an der Validierungsregeln keine Updates für den Controller Aktionscode erforderlich ist.

#### <a name="saving-the-submitted-values"></a>Speichern die übermittelten Werten

Wenn die Übermittlung des Formulars die Validierung erfolgreich war, ist es Zeit, die Werte in der Datenbank zu speichern. Mit Entity Framework erfordert, dass nur das Modell der Alben-Auflistung hinzugefügt und "SaveChanges" aufgerufen.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entitätsframework generiert die entsprechenden SQL-Befehle aus, den Wert persistent speichern. Nach dem Speichern der das, leiten wir zurück zur Liste der Alben, damit wir unsere Update sehen können. Dies erfolgt durch RedirectToAction zurückgeben, mit dem Namen der Controlleraktion, die wir angezeigt werden sollen. In diesem Fall ist, die Index-Methode.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Ungültige Formularübermittlung mit Validierungsfehlern anzeigen

Im Fall von ungültigen Eingaben die Dropdownliste Werte hinzugefügt, die ViewBag-Klasse (wie im Fall des HTTP-GET), und die gebundene Modell-Werte werden zurück an die Ansicht übergeben, für die Anzeige. Validierungsfehler werden automatisch angezeigt, mit der @Html.ValidationMessageFor HTML-Hilfsobjekt.

#### <a name="testing-the-create-form"></a>Testen das Formular erstellen

Dies out, führen Sie testen die Anwendung und navigieren Sie zu /StoreManager/erstellen / - zeigt leere Formular an, der von der StoreController erstellen HTTP-GET-Methode zurückgegeben wurde.

Geben Sie einige Werte aus, und klicken Sie auf die Schaltfläche "erstellen", um das Formular übermitteln.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Behandeln von Änderungen

Das Bearbeiten-Aktion-Paar (HTTP-GET und HTTP-POST) sind sehr ähnlich ist, auf die Aktionsmethoden erstellen, die, denen wir uns gerade angesehen haben. Da das Szenario bearbeiten, arbeiten mit einem vorhandenen Album, den bearbeiten HTTP-GET umfasst Methode das Album auf Grundlage des Parameters "Id" lädt übergebenen über die Route. Dieser Code zum Abrufen eines Albums von AlbumId entspricht dem wir zuvor in die Details-Controlleraktion erläutert habe. Wie bei der Create / HTTP-GET-Methode, die Dropdown-Werte werden über die ViewBag-Klasse zurückgegeben. Dadurch können wir ein Album als unsere Model-Objekts an die Ansicht zurückgegeben (die auf dem Album-Klasse stark typisiert ist) beim Übergeben von zusätzlicher Daten (z. B. eine Liste der Genres) über die ViewBag-Klasse.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Die Bearbeiten von HTTP-POST-Aktion ist sehr ähnlich ist, auf die Aktion des HTTP-POST erstellen. Der einzige Unterschied ist, die anstelle der Datenbank ein neues Album hinzugefügt. Sammlung von Alben, wir die aktuelle Instanz des Albums schwierigkeiten mit Db. Entry(Album) und seinen Zustand auf "geändert". Dies weist Entity Framework, dass wir zum Ändern eines vorhandenen Albums statt einen neuen zu erstellen.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Wir können dies sich durch Ausführen der Anwendung zu/StoreManger/navigieren, und klicken Sie auf den Link "Bearbeiten" für ein Album testen.

![](mvc-music-store-part-5/_static/image9.png)

Dadurch werden das Bearbeitungsformular angezeigt, indem Sie das Bearbeiten von HTTP-GET-Methode angezeigt. Geben Sie einige Werte aus, und klicken Sie auf die Schaltfläche "Speichern".

![](mvc-music-store-part-5/_static/image10.png)

Dies das Formular sendet, speichert die Werte und gibt uns zur Album-Liste angezeigt, dass die Werte aktualisiert wurden.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Löschung verarbeiten

Löschen von folgt demselben Muster wie bearbeiten und erstellen, verwenden eine Controlleraktion auf das Bestätigungsformular zur anzuzeigen, und einer anderen Controlleraktion, die Übermittlung des Formulars zu behandeln.

Die Delete HTTP-GET-Controlleraktion entspricht genau unseren vorherigen zu der Store Manager-Controlleraktion.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Ein Formular, das stark typisiert ist auf einen Album-Typ, der mit den Inhalt der Vorlage löschen Ansicht angezeigt.

![](mvc-music-store-part-5/_static/image12.png)

Die Delete-Vorlage zeigt alle Felder für das Modell, aber wir können diese nach unten etwas vereinfachen. Ändern Sie den Anzeigen von Code in /Views/StoreManager/Delete.cshtml folgt.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Eine vereinfachte Delete-Bestätigung wird angezeigt.

![](mvc-music-store-part-5/_static/image13.png)

Klicken Sie auf die Schaltfläche "löschen" bewirkt, dass das Formular wieder an den Server bereitgestellt werden, führt die Aktion "deleteconfirmed".

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Unsere HTTP-POST löschen Controlleraktion führt die folgenden Aktionen:

- 1. Lädt das Album nach ID
- 2. Das Album gelöscht, und Speichern von Änderungen
- 3. Umleitungen auf den Index, der anzeigt, dass das Album aus der Liste entfernt wurde

Um dies zu testen, führen Sie die Anwendung aus, und navigieren Sie zu /StoreManager. Wählen Sie ein Album aus der Liste aus, und klicken Sie auf den Link "löschen".

![](mvc-music-store-part-5/_static/image14.png)

Dadurch werden unsere löschen Bestätigungsbildschirm angezeigt.

![](mvc-music-store-part-5/_static/image15.png)

Klicken Sie auf die Schaltfläche "löschen" das Album entfernt und zu dem die Store Manager-Indexseite, der anzeigt, dass das Album gelöscht wurde.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Verwenden einer benutzerdefinierten HTML-Hilfsmethoden zum Abschneiden von text

Wir haben ein mögliches Problem mit unserem Store Manager-Indexseite. Unsere Albumtitel und Namen der Interpreten Eigenschaften können sowohl lang genug sein, dass sie aus unseren tabellenformatierung auslösen könnte. Wir erstellen eine benutzerdefinierte HTML-Hilfsfunktion, um uns diese und andere Eigenschaften in unserem Ansichten problemlos Abschneiden zu ermöglichen.

![](mvc-music-store-part-5/_static/image17.png)

Razor @helper Syntax hat es recht einfach, Ihre eigenen Hilfsfunktionen für die Verwendung in Ihren Ansichten zu erstellen. Öffnen Sie die Ansicht /Views/StoreManager/Index.cshtml, und fügen Sie den folgenden Code direkt nach der @model Zeile.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Diese Hilfsmethode akzeptiert eine Zeichenfolge und einer maximalen Länge, um zu ermöglichen. Wenn der angegebene Text kürzer als die Länge angegeben ist, das Hilfsprogramm gibt es als-ist. Wenn es länger ist, klicken Sie dann kürzt den Text und rendert "...", für den Rest.

Jetzt können wir unsere Truncate-Hilfsprogramm verwenden, um sicherzustellen, dass die Albumtitel und den Namen der Interpreten Eigenschaften weniger als 25 Zeichen sind. Die vollständige Ansichtscode mit unserem neuen Truncate-Hilfe wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Jetzt Wenn wir die /StoreManager/ URL navigieren, werden die Alben und Titel unten unsere maximale Länge gespeichert.

![](mvc-music-store-part-5/_static/image18.png)

Hinweis: Dieses Feld zeigt erstellen und Verwenden eines Hilfsprogramms in einer Ansicht ein einfachen Vorgang. Weitere Informationen finden Sie Informationen zum Erstellen von Hilfsprogrammen, die Sie innerhalb der gesamten Website verwenden können, finden Sie in meinem Blogbeitrag: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-4.md)
> [Weiter](mvc-music-store-part-6.md)
