---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: "Hinzufügen einer neuen Kategorie unter Verwendung von jQuery UI DropDownList | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2012
ms.topic: article
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 0cc51fbe84124a62f0c1254faab796cbcdc7efd6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Hinzufügen einer neuen Kategorie unter Verwendung von jQuery UI DropDownList
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

Der HTML-Code `Select` Tag eignet sich ideal für eine geordnete Liste der festen Kategoriedaten jedoch oft müssen Sie eine neue Kategorie hinzufügen. Nehmen wir an, dass wir den "Genre" "Opera" zu den Kategorien in der Datenbank hinzufügen möchten? In diesem Abschnitt werden wir jQuery UI verwenden, um ein Dialogfeld hinzufügen, wird eine neue Kategorie hinzufügen können. Die folgende Abbildung zeigt, wie die Benutzeroberfläche im Browser darstellen.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Wenn ein Benutzer wählt die **Hinzufügen eines neuen "Genre"** Link, ein Popup-Dialogfeld fordert den Benutzer für einen neuen Namen für die "Genre" (und optional eine Beschreibung). Das Bild unten zeigen die **hinzufügen "Genre"** Popup-Dialogfeld.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Wenn ein neuen Namen für die "Genre" eingegeben wird und die **speichern** Schaltfläche abgelegt wird, geschieht Folgendes:

1. Ein AJAX-Aufruf sendet die Daten in der Create-Methode des Controllers "Genre", die die neue "Genre" in der Datenbank gespeichert und gibt den neuen "Genre"-Informationen ("Genre" Name "und" ID ") als JSON.
2. JavaScript hinzugefügt der select-Liste die neue Daten für "Genre".
3. JavaScript wird das neue "Genre" des ausgewählten Elements an.

 In der folgenden Abbildung **Opera** wurde der Datenbank hinzugefügt und ausgewählt, die der **"Genre"** Dropdown-Liste. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Öffnen der *Views\StoreManager\Create.cshtml* -Datei und Ersetzen Sie durch Folgendes Markup "Genre" den folgenden Code:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Die `_ChooseGenre` partielle Ansicht enthält die gesamte Logik, um die JavaScript und jQuery verwendet, um das Hinzufügen neuer "Genre" Feature implementieren verknüpft. Nachdem wir den Code, der er sehr einfach, identisch mit der Interpret-UI kann abgeschlossen wurden.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die *Views\StoreManager* Ordner, und wählen **hinzufügen**, klicken Sie dann **Ansicht**. In der **Sichtname** Eingabe, geben Sie `_ChooseGenre` wählen Sie dann **hinzufügen**. Ersetzen Sie das Markup in der *Views\StoreManager\\_ChooseGenre.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Die erste Zeile deklariert, dass wir, in übergeben werden eine `Album` als unserem Modell genau in der Create View-Anweisung zu modellieren. Die nächsten Zeilen sind die **Bezeichnung** Helper Markup. Die nächste Zeile ist die **DropDownList** Helper aufrufen, genauso wie die ursprüngliche Ansicht erstellen. Die nächste Zeile Fügt eine Verknüpfung mit dem Namen `Add New Genre`, und es sich wie eine Schaltfläche Stile. Die Zeile mit `ValidationMessageFor` wird direkt über die Erstellungsansicht kopiert. Die folgenden Zeilen:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

erstellt ein ausgeblendetes Div mit der ID `genreDialog`. Verwenden wir jQuery anbinden unsere **hinzufügen "Genre"** Dialogfeld mit der ID `genreDialog` in dieser div. Die letzten beiden Skripttags enthalten Links zu den JavaScript-Dateien, die verwendet wird um das Hinzufügen neuer "Genre"-Funktion zu implementieren. Die */Scripts/chooseGenre.js* Datei wird für Sie in das Projekt, ihn später in diesem Lernprogramm untersucht wird.

Führen Sie die Anwendung, und klicken Sie auf die **Hinzufügen eines neuen "Genre"** Schaltfläche. In der **hinzufügen "Genre"** Dialogfeld Geben Sie **Opera** in der **Namen** Eingabefeld.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Klicken Sie auf die **speichern** Schaltfläche. Ein AJAX-Aufruf den Opera-Kategorie erstellt und anschließend füllt die Dropdownliste mit Opera und legt Opera als der ausgewählte "Genre".

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Geben Sie eine Künstler, Titel und Preis, und wählen Sie dann die **erstellen** Schaltfläche. Wenn Sie einen Preis ein, der kleiner als $8,99 eingeben, wird das neue Album am oberen Rand die Indexansicht angezeigt. Stellen Sie sicher, dass der neue Album Eintrag in der Datenbank gespeichert wurde.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Versuchen Sie, eine neue "Genre" mit nur einem Buchstaben zu erstellen. Der folgende code in der *Models\Genre.cs* Datei legt die minimale und maximale Länge des Namens "Genre".

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Clientseitige Validierung meldet, dass Sie eine Zeichenfolge zwischen 2 und 20 Zeichen eingeben müssen.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Untersuchen wie ein neues "Genre" wird die Datenbank und der Select-Liste hinzugefügt.

Öffnen der *Scripts\chooseGenre.js* Datei, und überprüfen Sie den Code.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Die zweite Zeile verwendet die ID `genreDialog` So erstellen ein Dialogfeld auf das Div-Tag in der *Views\StoreManager\\_ChooseGenre.cshtml* Datei. Die meisten benannte Parameter sind erklärende. Die `autoOpen` Parameter auf "false" festgelegt ist Auswählen der **erstellen "Genre"** Schaltfläche Öffnen den Dialog explizit (hierzu finden Sie auf letztere). Der Dialog besitzt zwei Schaltflächen **speichern** und **"Abbrechen"**. Die **"Abbrechen"** Schaltfläche wird das Dialogfeld geschlossen. Der folgende code zeigt die **speichern** Schaltfläche Funktion.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Die `var createGenreForm` ausgewählt ist, aus der `createGenreForm` -ID. Die `createGenreForm` -ID wurde festgelegt, in den folgenden Code, der der *Views\Genre\\_CreateGenre.cshtml* Datei.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Die [Html.BeginForm](https://msdn.microsoft.com/en-us/library/dd492714.aspx) Helper-Überladung, die verwendet wird, der *Views\Genre\\_CreateGenre.cshtml* Datei HTML mit ein, der die URL zum Absenden des Formulars Aktionsattribut generiert. Sie können dies überprüfen, von der Seite "Album erstellen" in einem Browser anzeigen und Auswählen der anzeigen-Quelle im Browser. Das folgende Markup zeigt die generierten HTML-Codes, die das Formulartag enthält.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Die jQuery `$.post` Zeile führt einen AJAX-Aufruf an das Action-Attribut (`/StoreManager/Create`) und übergibt die Daten aus der **erstellen "Genre"** (Dialogfeld). Die Daten bestehen aus den Namen für die neue "Genre" und eine optionale Beschreibung. Wenn die AJAX-Aufruf erfolgreich ist, neue "Genre" Name und Wert sind Select Markup hinzugefügt, und die neue "Genre" auf den ausgewählten Wert festgelegt ist. Da es sich dynamisch generierten Markup handelt, nicht die neue select-Option durch Anzeigen der Quelle im Browser angezeigt werden. Sie können den neuen HTML-Code mit den Internet Explorer 9 F12 Entwicklertools sehen. Um die neue select-Option in Internet Explorer 9 anzuzeigen, drücken Sie F12-Taste, um die F12 Entwicklertools zu starten. Navigieren Sie zu der Seite "erstellen", und fügen Sie eine neue "Genre" hinzu, damit die neue "Genre" in der Auswahlliste "Genre" ausgewählt ist. In den F12-Entwicklungstools:

1. Wählen Sie die Registerkarte "HTML".
2. Drücken Sie das Symbol "Aktualisierung".  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Geben Sie in das Suchfeld GenreID aus.
4. Verwenden das Symbol "Weiter",   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
 Navigieren Sie zu der folgenden select-Tags:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Erweitern Sie den letzten Optionswert.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Den folgenden code in die *Scripts\chooseGenre.js* Datei veranschaulicht, wie die **Hinzufügen eines neuen "Genre"** Schaltfläche ruft mit dem Klickereignis verbunden und wie sich das **Hinzufügen eines neuen "Genre"** wird das Dialogfeld erstellt.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Die erste Zeile erstellt eine klicken-Funktion, die angefügt werden, um die **Hinzufügen eines neuen "Genre"** Schaltfläche. Das folgende Markup aus der Views\StoreManager\\_ChooseGenre.cshtml-Datei zeigt wie die **Hinzufügen eines neuen "Genre"** Schaltfläche wird erstellt:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Die Load-Methode erstellt und öffnet das Dialogfeld "hinzufügen" Genre "" und dem jQuery ruft `parse` Methode, damit die Clientvalidierung für Daten, die im Dialogfeld eingegebene stattfindet.

In diesem Abschnitt haben Sie gelernt, wie Sie ein Dialogfeld erstellen, die zum Hinzufügen von neuen Kategoriedaten zu einer select-Liste verwendet werden kann. Sie können das gleiche Verfahren zum Erstellen von Benutzeroberfläche zum Hinzufügen eines neuen Interpreten der Interpret-Auswahlliste folgen. In diesem Lernprogramm gegeben hat einen Überblick über das Arbeiten mit der ASP.NET MVC-HTML-Hilfsobjekt **DropDownList**. Weitere Informationen zum Arbeiten mit der **DropDownList**, finden Sie im Abschnitt Verweise hinzufügen unten. Bitte lassen Sie uns wissen Sie, ob dieses Lernprogramms hilfreich war.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Zusätzliche Referenzen

- [ASP.NET MVC – kaskadierende Dropdownlisten Lernprogramm](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) von [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Ausgewählte](http://harvesthq.github.com/chosen/) eine JavaScript-Plug-Ins, die mehrere Elemente auswählen und Filtern zu unterstützen.

### <a name="contributors"></a>Mitwirkende

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Bearbeiter

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike PPPoE
- Tom Dykstra

>[!div class="step-by-step"]
[Zurück](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
