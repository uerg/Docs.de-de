---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
title: 'Teil 5: Bearbeiten von Formularen von Formaten und Vorlagen | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 5 wird von Formaten und Vorlagen bearbeiten Forms behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 6b09413a-6d6a-425a-87c9-629f91b91b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-5
msc.type: authoredcontent
ms.openlocfilehash: d584e614b5a4124044cd9decd2272192ca164643
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874911"
---
<a name="part-5-edit-forms-and-templating"></a>Teil 5: Bearbeiten von Formularen von Formaten und Vorlagen
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.
> 
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 5 wird von Formaten und Vorlagen bearbeiten Forms behandelt.


In den letzten Kapitel wurden wir Laden von Daten aus der Datenbank und anzeigen. In diesem Kapitel werden wir auch aktivieren, die Daten bearbeiten.

## <a name="creating-the-storemanagercontroller"></a>Erstellen die StoreManagerController

Wir beginnen, indem das Erstellen eines neuen Domänencontrollers aufgerufen **StoreManagerController**. Für diesen Controller dauern wir die Gerüstbau-Funktionen in ASP.NET MVC 3 Tools Update verfügbar. Legen Sie die Optionen für das Dialogfeld "Controller hinzufügen" aus, wie unten dargestellt.

![](mvc-music-store-part-5/_static/image1.png)

Wenn Sie die Schaltfläche "hinzufügen" klicken, sehen Sie sich, dass der ASP.NET MVC 3-Gerüstbau großzügig bemessenen Arbeit für Sie erledigt:

- Erstellt neue StoreManagerController mit einer lokalen Variablen für die Entity Framework
- Die Ansichtenordner des Projekts hinzugefügt einen Ordner StoreManager
- Es fügt Create.cshtml, Delete.cshtml Details.cshtml, Edit.cshtml und Index.cshtml Ansicht stark typisiert auf die Album-Klasse

![](mvc-music-store-part-5/_static/image2.png)

Die neue StoreManager Controllerklasse enthält CRUD (erstellen, lesen, aktualisieren und löschen) Controlleraktionen zum Arbeiten mit dem Album kennen modellschemas und verwenden Sie unsere Entity Framework-Datenkontext für den Datenbankzugriff.

## <a name="modifying-a-scaffolded-view"></a>Das Ändern einer Sicht Scaffolded

Es ist wichtig zu beachten, dass dieser Code für uns erstellt wurde, ist ASP.NET MVC-Standardcode, so wie wir in diesem Lernprogramm geschrieben haben. Es wurde so sparen Sie Zeit beim würde auf Textbaustein controllercode schreiben, und erstellen manuell die stark typisierten Ansichten vorgesehen, dies ist jedoch die Art des generierten Codes Sie haben gelernt, die vorangestellt schlimm Warnungen in den Kommentaren gewarnt wird, wie Sie ändern darf nicht den Code. Dies ist der Code, und es wird vorausgesetzt, um ihn zu ändern.

Daher beginnen wir mit einer schnellen bearbeiten, um die Indexansicht StoreManager (/ Views/StoreManager/Index.cshtml). Diese Ansicht zeigt eine Tabelle die Alben in unserem Store mit bearbeiten listet / Details / Löschen von Verknüpfungen und schließt das Album öffentlichen Eigenschaften. Das Feld AlbumArtUrl müssen entfernt werden, da es nicht in dieser Anzeige sehr nützlich ist. In &lt;Tabelle&gt; Abschnitt des Codes Ansicht Entfernen der &lt;ten&gt; und &lt;td&gt; Elemente umgebenden AlbumArtUrl verweisen, wie durch die hervorgehobenen Zeilen unten angegeben:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample1.cshtml)]

Der Code geänderte anzeigen wird wie folgt aussehen:

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample2.cshtml)]

## <a name="a-first-look-at-the-store-manager"></a>Ein erster Blick auf die Speicher-Manager

Jetzt wird führen Sie die Anwendung aus, und navigieren Sie zu/StoreManager /. Dadurch wird der Index Manager speichern soeben zeigt eine Liste von Alben im Speicher mit Links geänderten zu bearbeiten, Details, und löschen, angezeigt.

![](mvc-music-store-part-5/_static/image3.png)

Durch Klicken auf den Link bearbeiten zeigt ein Bearbeitungsformular mit Feldern für das Album, einschließlich der Dropdownlisten für "Genre" und Interpreten.

![](mvc-music-store-part-5/_static/image4.png)

Klicken Sie auf den Link "Zurück zur Liste" im unteren Bereich, und klicken Sie auf den Link Details für ein Album. Dies zeigt detaillierte Informationen für eine einzelne Album.

![](mvc-music-store-part-5/_static/image5.png)

Klicken Sie erneut, klicken Sie auf der Rückseite Liste verknüpfen, dann klicken Sie auf einen Link löschen. Dies zeigt ein Dialogfeld zur Bestätigung Anzeigen der Albumdetails und gefragt werden, wenn es wirklich gelöscht werden soll.

![](mvc-music-store-part-5/_static/image6.png)

Klicken auf die Schaltfläche "löschen" im unteren Bereich wird die Album löschen und zurückgegeben, um die Indexseite, die das Album gelöscht dargestellt wird.

Mit der Speicher-Manager sind nicht fertig, jedoch können wir arbeiten, Controller und Ansichtscode für die CRUD-Vorgänge aus starten.

## <a name="looking-at-the-store-manager-controller-code"></a>Betrachtung des Codes von Store-Manager-Controller

Der Speicher-Manager-Controller enthält großzügig bemessenen Code. Gehen Sie wir noch über diesen von oben nach unten. Der Controller enthält einige standard-Namespaces für ein MVC-Controller als auch einen Verweis auf unseren Modelle-Namespace. Der Controller hat eine private Instanz von MusicStoreEntities, von der Controlleraktionen jeweils für den Datenzugriff verwendet.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample3.cs)]

### <a name="store-manager-index-and-details-actions"></a>Speichern Sie die Aktionen Manager Index "und" Details

Die Indexansicht Ruft eine Liste von Alben, einschließlich jedes Album verwiesen wird "Genre" und Interpreten, wie wir bereits gesehen haben, bei der Arbeit für die Methode Store durchsuchen. Die Indexansicht liegt die Verweise auf die verknüpften Objekte im Anschluss an, damit dieses jedes Album "Genre" Namen und den Namen des Künstlers anzeigen kann, damit der Controller effizienter und Abfragen für diese Informationen in der ursprünglichen Anforderung wird.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample4.cs)]

Details-Controlleraktion der StoreManager Controller funktioniert genauso wie die Store Controller Details-Aktion, die wir Ihnen mitgeteilt zuvor haben - abgefragt Album nach ID, die mit der Methode wird, dann gibt sie zurück zur Ansicht.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample5.cs)]

### <a name="the-create-action-methods"></a>Das Erstellen von Aktionsmethoden

Die Aktionsmethoden erstellen sind etwas anders aus Einsen, die bisher gesehen haben, da sie Formulareingaben behandeln. Wenn ein Benutzer zuerst /StoreManager/erstellen/besucht werden sie ein leeres Formular angezeigt. Die HTML-Seite enthält eine &lt;Formular&gt; -Element, Dropdownlisten und Textfeld enthält, Eingabeelementen in dem sie das Album-Details eingeben können.

Wenn der Benutzer in die Formularwerte Album voll ist, können sie die Schaltfläche "Speichern" zu senden, dass diese Änderungen werden wieder an die Anwendung innerhalb der Datenbank gespeichert. Wenn der Benutzer die Schaltfläche "Speichern" drückt die &lt;Formular&gt; HTTP-POST an die /StoreManager/erstellen/URL durchgeführt und übermitteln der &lt;Formular&gt; Werte als Teil der HTTP-POST-Anforderung.

ASP.NET MVC erlaubt es uns, der Logik dieser beiden Szenarien der URL-Aufruf problemlos aufteilen, indem Sie uns, implementieren Sie zwei separate "Erstellen" Aktionsmethoden in unsere StoreManagerController-Klasse – eine zum Behandeln der anfänglichen HTTP-GET-durchsuchen, um die /StoreManager/Create aktivieren / URL und die andere, die HTTP-POST der übermittelten Änderungen zu behandeln.

### <a name="passing-information-to-a-view-using-viewbag"></a>Übergeben von Informationen zu einer Sicht mithilfe des ViewBag

Wir haben das ViewBag-Objekt oben in diesem Lernprogramm verwendet, aber noch nicht viel über gesprochen. Das ViewBag ermöglicht, Informationen zur Ansicht zu übergeben, ohne Verwendung eines stark typisierten Model-Objekts. In diesem Fall unsere bearbeiten HTTP-GET-Controlleraktion sowohl eine Liste von Genres und Künstler zum Formular, um den Dropdownlisten Auffüllen übergeben muss, und die einfachste Möglichkeit hierzu ist, um als ViewBag Elemente zurückzugeben.

Das ViewBag-Objekt ist ein dynamisches Objekt ist, was bedeutet, dass Sie ViewBag.Foo oder ViewBag.YourNameHere eingeben können, ohne das Schreiben von Code, um diese Eigenschaften zu definieren. In diesem Fall wird der controllercode, damit die Dropdown-Werte, die mit dem Formular übermittelt werden, GenreId und ArtistId, die die Album-Eigenschaften, die sie festlegen, sind ViewBag.GenreId und ViewBag.ArtistId verwendet.

Diese Dropdownliste Werte werden zurückgegeben, in das Formular unter Verwendung der SelectList-Objekts, die nur für diesen Zweck erstellt wird. Dies erfolgt mithilfe von Code wie folgt:

[!code-csharp[Main](mvc-music-store-part-5/samples/sample6.cs)]

Wie Sie aus der Methodencode Aktion sehen können, sind drei Parameter verwendet wird, auf dieses Objekt zu erstellen:

- Die Liste der Elemente aus die Dropdownliste wird angezeigt werden. Beachten Sie, dass dieses Element nicht bloß eine Zeichenfolge – wir haben eine Liste von Genres übergeben.
- Der nächste Parameter, die die SelectList übergeben werden, ist der Wert der ausgewählten. Diese wie der SelectList weiß, wie ein Element in der Liste vorab ausgewählt werden kann. Dies ist einfacher zu verstehen, wenn das Formular bearbeiten, betrachten wir also ziemlich ähnliche wird.
- Der letzte Parameter ist die Eigenschaft angezeigt werden. In diesem Fall ist dies an, dass die Genre.Name-Eigenschaft ist, was dem Benutzer angezeigt wird.

Denken Sie daran klicken Sie dann das Erstellen von HTTP-GET-Aktion ist ganz einfach – zwei SelectLists ViewBag hinzugefügt werden und keine Modellobjekt an das Formular übergeben wird, (da er noch nicht erstellt wurde).

[!code-csharp[Main](mvc-music-store-part-5/samples/sample7.cs)]

### <a name="html-helpers-to-display-the-drop-downs-in-the-create-view"></a>HTML-Hilfsmethoden, um das Ausschalten des Drop in die Create View anzuzeigen

Da wir nicht die Rede haben wie das Dropdown-Werte in die Ansicht übergeben werden, werfen wir einen kurzen Blick auf die Ansicht, um festzustellen, wie diese Werte angezeigt werden. In den Code anzeigen (/ Views/StoreManager/Create.cshtml), sehen Sie zum Anzeigen der Dropdownliste "Genre" ist der folgende Aufruf erfolgt nach unten.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample8.cshtml)]

Dies wird als eine HTML-Hilfsobjekt - eine Hilfsprogrammmethode bezeichnet, die eine allgemeine Ansicht Aufgabe ausführt. HTML-Hilfsmethoden sind sehr nützlich, in unserem Code anzeigen lassen, präziser und besser lesbar. Das Hilfsobjekt Html.DropDownList wird von ASP.NET MVC bereitgestellt, aber später eingehendem ist es möglich, unser eigenes Hilfen für Code anzeigen zu erstellen, die wir in unserer Anwendung wiederverwendet werden.

Der Aufruf Html.DropDownList muss nur zwei Dinge - mitgeteilt werden, wobei zum Abrufen der Liste aus, um anzuzeigen, und welche Wert (sofern vorhanden) vorab ausgewählt werden soll. Der erste Parameter, GenreId, weist der DropDownList-Wert namens GenreId in das Modell oder die ViewBag gesucht werden soll. Der zweite Parameter wird verwendet, an, dass der Wert, um anzuzeigen, wie zunächst in der Dropdownliste ausgewählt. Da dieses Formular ein Erstellungsformular ist, wird kein Wert vorab ausgewählten sein, und "String.Empty" übergeben.

### <a name="handling-the-posted-form-values"></a>Behandlung von die gebucht Formularwerte

Wie bereits erwähnt, sind zwei Aktionsmethoden jedes Formular zugeordnet. Die erste verarbeitet die HTTP-GET-Anforderung und zeigt das Formular. Die zweite verarbeitet die HTTP-POST-Anforderung, die die übermittelte Formularwerte enthält. Beachten Sie, dass Controlleraktion ein [HttpPost]-Attribut verfügt, die ASP.NET MVC mitteilt, dass er nur auf HTTP-POST-Anforderungen reagieren soll.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample9.cs)]

Diese Aktion hat vier Aufgaben:

- 1. Lesen Sie die Formularwerte
- 2. Überprüfen Sie, ob die Formularwerte alle Validierungsregeln übergeben
- 3. Wenn es sich bei der Übermittlung des Formulars gültig ist, speichern Sie die Daten und die aktualisierte Liste angezeigt.
- 4. Wenn es sich bei der Übermittlung des Formulars nicht gültig ist, erneut anzuzeigen Sie das Formular mit Überprüfungsfehlern.

#### <a name="reading-form-values-with-model-binding"></a>Lesen von Formularwerte mit Modellbindung

Controlleraktion wird von einer Formularübergabe verarbeitet, die Werte für GenreId und ArtistId (aus der Dropdown-Liste) und Textfeldwerte für Titel, Preis und AlbumArtUrl enthält. Es ist, zwar möglich, direkt auf Formularwerte zuzugreifen ist ein besserer Ansatz zum Verwenden der Modell Bindung-Funktionen in ASP.NET MVC erstellt. Wenn eine Controlleraktion einen Modelltyp als Parameter akzeptiert, versucht ASP.NET MVC, ein Objekt dieses Typs mit Formulareingaben (ebenso wie Route und Abfragezeichenfolgen-Werte) auffüllen. Dies geschieht, indem die Suche nach Werten, deren Namen entsprechen die Eigenschaften des Modellobjekts, z. B. Wenn Sie das neue Album Objekt GenreId Wert festlegen, sucht es nach der Eingabe mit dem Namen GenreId. Beim Erstellen von Ansichten, verwenden die Standardmethoden in ASP.NET MVC, werden die Formulare immer mithilfe von Eigenschaftsnamen als Eingabe Feldnamen, damit diese Feldnamen nur übereinstimmen gerendert werden.

#### <a name="validating-the-model"></a>Beim Validieren des Modells

Das Modell wird durch einen einfachen Aufruf ModelState.IsValid überprüft. Dies nicht getan haben wir unsere Album-Klasse alle Validierungsregeln hinzugefügt noch - wir erledigen, die in einem Bit – also jetzt diese Prüfung viel zu tun hat. Wichtig ist, dass diese Überprüfung ModelStat.IsValid den Validierungsregeln angepasst wird, die wir uns auf unserem Modell konzentrieren, damit zukünftige Änderungen Validierungsregeln alle Updates für den Controller Aktionscode erforderlich ist.

#### <a name="saving-the-submitted-values"></a>Speichern der gesendeten Werte

Wenn es sich bei der Übermittlung des Formulars die Clusterüberprüfung erfolgreich war, ist es Zeit für die Werte in der Datenbank zu speichern. Mit Entity Framework erfordert, dass nur das Modell auf die Alben Auflistung hinzufügen und SaveChanges aufrufen.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample10.cs)]

Entity Framework generiert die entsprechenden SQL-Befehle auf den Wert beizubehalten. Nach dem Speichern der Daten, leiten wir zurück zur Liste der Alben, damit wir unsere Update sehen können. Dies erfolgt durch RedirectToAction zurückgeben, mit dem Namen der Controlleraktion, die wir anzeigen möchten. In diesem Fall ist, die die Index-Methode.

#### <a name="displaying-invalid-form-submissions-with-validation-errors"></a>Ungültige Formularübermittlung mit Überprüfungsfehlern anzeigen

Im Fall von ungültigen Eingaben die Dropdownliste Werte hinzugefügt, das ViewBag-Objekt (wie im Fall des HTTP-GET) und gebundene Modellwerte werden zurück an die Ansicht übergeben, für die Anzeige. Validierungsfehler werden automatisch angezeigt, mit der @Html.ValidationMessageFor HTML-Hilfsobjekt.

#### <a name="testing-the-create-form"></a>Testen das Formular erstellen

Um dies, führen Sie die Anwendung und navigieren Sie zu /StoreManager/erstellen / - testen erfahren dies Sie, leere Formular, die von der StoreController erstellen HTTP-GET-Methode zurückgegeben wurde.

Einige Werte füllen Sie, und klicken Sie auf die Schaltfläche "erstellen", um die Übermittlung des Formulars.

![](mvc-music-store-part-5/_static/image7.png)

![](mvc-music-store-part-5/_static/image8.png)

### <a name="handling-edits"></a>Behandlung von Änderungen

Das Bearbeiten-Aktion-Paar (HTTP-GET und HTTP-POST) sind sehr ähnlich Aktionsmethoden erstellen, denen wir gerade angesehen. Da das Szenario Bearbeiten arbeiten mit einem vorhandenen Album, das Bearbeiten HTTP-GET umfasst Methode basierte auf dem Parameter "Id" Album lädt übergebenen über die Route. Dieser Code zum Abrufen von ein Album AlbumId entspricht dem wir zuvor in den Details-Controlleraktion besprochen haben. Wie bei der Create / HTTP-GET-Methode das Dropdown-Werte werden über das ViewBag-Objekt zurückgegeben. Dies ermöglicht es, ein Album als unsere Modellobjekt an die Sicht zurückzugeben (die auf die Klasse Album stark typisiert ist) beim Übergeben von zusätzlicher Daten (z. B. eine Liste von Genres) über das ViewBag-Objekt.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample11.cs)]

Die HTTP-POST bearbeiten-Aktion ist vergleichbar mit dem die Aktion des HTTP-POST erstellen. Der einzige Unterschied ist, die anstelle der Datenbank ein neues Album hinzugefügt. Alben-Auflistung, haben wir die aktuelle Instanz des Albums Suchen von mit Db. Entry(Album) und seinen Status auf "geändert" festlegen. Das weist Entity Framework, dass es einem vorhandenen Album im Gegensatz zur Erstellung einer neuen gewählt haben.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample12.cs)]

Wir können dies zu testen, durch Ausführen der Anwendung zu/StoreManger/durchsuchen, und klicken Sie auf den Bearbeitungslink für ein Album.

![](mvc-music-store-part-5/_static/image9.png)

Dies zeigt den bearbeiten-Formular angezeigt, indem Sie das Bearbeiten von HTTP-GET-Methode. Einige Werte füllen Sie, und klicken Sie auf die Schaltfläche "Speichern".

![](mvc-music-store-part-5/_static/image10.png)

Das Formular sendet, speichert die Werte, es gibt uns Album-Liste, die anzeigt, dass die Werte aktualisiert wurden.

![](mvc-music-store-part-5/_static/image11.png)

### <a name="handling-deletion"></a>Behandlung von löschen

Löschvorgang folgt demselben Muster wie bearbeiten und erstellen, verwenden einen Controlleraktion aus, um das Bestätigungsformular anzuzeigen und eine andere Controlleraktion der Übermittlung des Formulars zu behandeln.

Löschen von HTTP-GET-Controlleraktion entspricht genau unserer vorherigen Details zum Speicher-Manager-Controlleraktion.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample13.cs)]

Wir haben ein Formular, das stark typisiert ist auf einen Album-Typ, mit der Delete-Ansicht-Inhaltsvorlage anzeigen.

![](mvc-music-store-part-5/_static/image12.png)

Die Vorlage "löschen" zeigt alle Felder für das Modell, aber wir können diese aus erheblich vereinfachen. Ändern Sie den Code anzeigen in /Views/StoreManager/Delete.cshtml wie.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample14.cshtml)]

Dadurch wird eine vereinfachte Löschen Bestätigung angezeigt.

![](mvc-music-store-part-5/_static/image13.png)

Klicken auf die Schaltfläche "löschen" bewirkt, dass das Formular an den Server bereitgestellt werden, die DeleteConfirmed Aktion ausführt.

[!code-csharp[Main](mvc-music-store-part-5/samples/sample15.cs)]

Unsere HTTP-POST löschen Controlleraktion werden folgende Aktionen ausgeführt:

- 1. Lädt das Album nach ID
- 2. Das Album gelöscht und die Änderungen zu speichern
- 3. Leitet auf den Index, der anzeigt, dass das Album aus der Liste entfernt wurde

Um dies zu testen, führen Sie die Anwendung aus, und navigieren Sie zu /StoreManager. Wählen Sie ein Album aus der Liste aus, und klicken Sie auf den Link löschen.

![](mvc-music-store-part-5/_static/image14.png)

Dadurch werden unsere Delete Bestätigungsbildschirm angezeigt.

![](mvc-music-store-part-5/_static/image15.png)

Klicken auf die Schaltfläche "löschen" Album entfernt und gibt uns, die Speicher-Manager-Indexseite, der anzeigt, dass das Album gelöscht wurde.

![](mvc-music-store-part-5/_static/image16.png)

### <a name="using-a-custom-html-helper-to-truncate-text"></a>Mithilfe einer benutzerdefinierten HTML-Hilfsmethoden zum Abschneiden von text

Wir haben ein mögliches Problem mit unserer Indexseite von Speicher-Manager. Unsere Albumtitel und Namen Interpreten Eigenschaften können sowohl lang genug sein, dass sie deaktiviert unsere Tabellenformat auslösen könnte. Wir erstellen eine benutzerdefinierte HTML-Hilfsobjekt um zulässt, diese und andere Eigenschaften in unserer Ansichten einfach abgeschnitten.

![](mvc-music-store-part-5/_static/image17.png)

Der Razor- @helper Syntax machte es leicht zu eigener Hilfsfunktionen für die Verwendung in Ihren Ansichten zu erstellen. Öffnen Sie die /Views/StoreManager/Index.cshtml anzuzeigen, und fügen Sie folgenden Code direkt nach der @model Zeile.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample16.cshtml)]

Diese Hilfsmethode verwendet eine Zeichenfolge und einer maximalen Länge aus, um zu ermöglichen. Wenn der angegebene Text kürzer als die Länge angegeben ist, werden sie als das Hilfsobjekt ausgegeben-ist. Wenn es länger ist, klicken Sie dann kürzt den Text und rendert "..." für den Rest.

Jetzt können wir unsere Truncate-Hilfsprogramm verwenden, um sicherzustellen, dass sowohl die Interpret den Namen der Albumtitel weniger als 25 Zeichen sind. Der vollständige Ansichtscode mit unserem neuen Truncate-Hilfsprogramm wird unten angezeigt.

[!code-cshtml[Main](mvc-music-store-part-5/samples/sample17.cshtml)]

Jetzt Wenn wir die /StoreManager/ URL navigieren, werden die Alben und Titel unten unsere maximale Länge gespeichert.

![](mvc-music-store-part-5/_static/image18.png)

Hinweis: Dies zeigt die einfachen Fall erstellen und verwenden eine Hilfsmethode in einer Ansicht. Weitere Informationen finden Sie Informationen zum Erstellen von Hilfsprogrammen, die Sie in Ihrer Website verwenden können, finden Sie in meinem Blogbeitrag: [http://bit.ly/mvc3-helper-options](http://bit.ly/mvc3-helper-options)


> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-4.md)
> [Weiter](mvc-music-store-part-6.md)
