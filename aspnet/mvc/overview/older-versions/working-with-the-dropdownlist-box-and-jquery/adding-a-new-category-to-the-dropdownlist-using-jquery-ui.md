---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Hinzufügen einer neuen Kategorie zu DropDownList mithilfe der jQuery-Benutzeroberfläche | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802525"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Hinzufügen einer neuen Kategorie zu DropDownList mithilfe der jQuery-Benutzeroberfläche
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

Der HTML-Code `Select` Tag eignet sich ideal für eine geordnete Liste der festen Kategoriedaten, aber häufig müssen Sie eine neue Kategorie hinzufügen. Nehmen wir an, dass das Genre "Opera", die Kategorien in der Datenbank hinzugefügt werden soll? In diesem Abschnitt verwenden wir die jQuery-Benutzeroberfläche auf ein Dialogfeld hinzufügen, mit denen wir eine neue Kategorie hinzuzufügen. Die folgende Abbildung zeigt, wie die Benutzeroberfläche im Browser darstellen wird.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Wenn ein Benutzer wählt die **Hinzufügen eines neuen "Genre"** Link, ein Popup-Dialogfeld fordert den Benutzer für einen neuen Namen für "Genre" (und optional eine Beschreibung). Das Bild unten zeigen die **hinzufügen "Genre"** Popup-Dialogfeld.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Wenn ein neuer Name für "Genre" eingegeben wird und die **speichern** Schaltfläche gedrückt wird, geschieht Folgendes:

1. Ein AJAX-Aufruf sendet die Daten in der Create-Methode des Controllers "Genre", die neue "Genre" in der Datenbank gespeichert und die neue "Genre"-Informationen (Name des Genres und ID) im JSON-Format zurückgegeben.
2. JavaScript fügt die neuen Daten für "Genre" in der select-Liste hinzu.
3. JavaScript macht dem neuen Genre des ausgewählten Elements an.

   In der folgenden Abbildung **Opera** wurde der Datenbank hinzugefügt und ausgewählt, die der **"Genre"** Dropdown-Liste. 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Öffnen der *Views\StoreManager\Create.cshtml* -Datei und Ersetzen Sie das Markup für "Genre" durch den folgenden den folgenden Code:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Die `_ChooseGenre` partielle Ansicht enthält die gesamte Logik, um die JavaScript- und jQuery verwendet, um das Hinzufügen neuer "Genre" Feature implementieren einbinden. Nachdem wir den Code haben, wird es sehr einfach, mit der Interpret-Benutzeroberfläche identisch sein.

Im Projektmappen-Explorer mit der rechten Maustaste die *Views\StoreManager* Ordner, und wählen **hinzufügen**, klicken Sie dann **Ansicht**. In der **Sichtname** eingeben, geben Sie `_ChooseGenre` wählen Sie dann **hinzufügen**. Ersetzen Sie das Markup in der *Views\StoreManager\\_ChooseGenre.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

Die erste Zeile gibt an, dass wir übergeben eine `Album` als unser Modell genauso model-Anweisung finden Sie in der Ansicht "erstellen". Die nächsten Zeilen werden die **Bezeichnung** Helper-Markup. Die nächste Zeile ist die **DropDownList** Helper-aufrufen, wie in der ursprünglichen Create-Ansicht. Die nächste Zeile Fügt einen Link mit dem Namen `Add New Genre`, und es wie eine Schaltfläche zu formatieren. Die Zeile mit `ValidationMessageFor` direkt aus der Ansicht "erstellen" kopiert. Die folgenden Zeilen:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

erstellt ein ausgeblendetes DIV-Element, mit der ID `genreDialog`. Verwenden wir jQuery um zu verknüpfen, unsere **hinzufügen "Genre"** Dialogfeld mit der ID `genreDialog` in dieses DIV-Element Die letzten beiden Skripttags enthalten Links zu den JavaScript-Dateien, die wir verwenden werden, um das Hinzufügen neuer "Genre"-Feature zu implementieren. Die */Scripts/chooseGenre.js* Datei dient für Sie in das Projekt, ihn später in diesem Tutorial untersuchen wir wird.

Führen Sie die Anwendung, und klicken Sie auf die **Hinzufügen eines neuen "Genre"** Schaltfläche. In der **hinzufügen "Genre"** Dialogfeld Geben Sie **Opera** in die **Namen** Eingabefeld.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Klicken Sie auf die Schaltfläche **Speichern**. Ein AJAX-Aufruf erstellt die Kategorie "Opera", und klicken Sie dann füllt die Dropdownliste mit Opera, und legt Opera als das ausgewählte Genre.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Geben Sie eine Interpret, Titel und Preis, und wählen Sie dann die **erstellen** Schaltfläche. Wenn Sie einen Preis ein, der kleiner als $8,99 eingeben, wird das neue Album am oberen Rand der Ansicht "Index" angezeigt. Stellen Sie sicher, dass der neue Album-Eintrag in der Datenbank gespeichert wurde.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Versuchen Sie, eine neue "Genre" mit nur einem Buchstaben zu erstellen. Im folgenden code in die *Models\Genre.cs* Datei legt die minimale und maximale Länge des Namens "Genre" fest.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Clientseitige Validierung meldet, dass Sie eine Zeichenfolge zwischen 2 und 20 Zeichen eingeben müssen.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Untersuchen der wie ein neues "Genre" wird die Datenbank und der Select-Liste hinzugefügt.

Öffnen der *Scripts\chooseGenre.js* Datei, und überprüfen Sie den Code.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

Die zweite Zeile verwendet die ID `genreDialog` So erstellen ein Dialogfeld für das Div-Tag in die *Views\StoreManager\\_ChooseGenre.cshtml* Datei. Die meisten benannte Parameter sind Erläuterung erforderlich. Die `autoOpen` Parameter auf "False" festgelegt ist Auswählen der **erstellen "Genre"** Schaltfläche wird das Dialogfeld geöffnet, explizit (hierzu finden Sie auf letztere). Der Dialog besitzt zwei Schaltflächen, **speichern** und **Abbrechen**. Die **Abbrechen** Schaltfläche wird das Dialogfeld geschlossen. Der folgende code zeigt die **speichern** Schaltfläche Funktion.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Die `var createGenreForm` ausgewählt ist, aus der `createGenreForm` -ID. Die `createGenreForm` -ID wurde festgelegt, in den folgenden Code finden Sie in der *Views\Genre\\_CreateGenre.cshtml* Datei.

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

Die [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) Helper-Überladung, die in verwendet die *Views\Genre\\_CreateGenre.cshtml* Datei generiert HTML-Code mit einer Action-Attribut, das mit der URL zum Absenden des Formulars. Sie können dies überprüfen, die Seite "Album erstellen" in einem Browser anzeigen und Auswählen der anzeigen-Quelle im Browser. Das folgende Markup zeigt die generierten HTML-Code, der das Form-Tag enthält.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

Die jQuery `$.post` Zeile wird einen AJAX-Aufruf an das Action-Attribut (`/StoreManager/Create`) auf und übergibt die Daten aus der **erstellen "Genre"** Dialogfeld. Die Daten bestehen aus den Namen für das neue Genre und eine optionale Beschreibung. Wenn der AJAX-Aufruf erfolgreich ist, den neuen Namen für "Genre" und den Wert der Option Markup hinzugefügt werden, und die neue "Genre" in den ausgewählten Wert festgelegt ist. Es sich dynamisch generiertes Markup handelt, nicht die neue Option für die Option durch Anzeigen der Quelle im Browser angezeigt. Sie sehen den neuen HTML-Code mit den Internet Explorer 9 F12-Entwicklertools. Um die neue Option wählen, in Internet Explorer 9 anzuzeigen, drücken Sie die F12-Taste, um die F12-Entwicklertools zu starten. Navigieren Sie zu der Seite "erstellen", und fügen Sie eine neue "Genre" hinzu, damit die neue "Genre" in der Auswahlliste "Genre" ausgewählt ist. In den Entwicklertools (F12):

1. Wählen Sie die Registerkarte "HTML".
2. Drücken Sie das Aktualisierungssymbol klicken.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Geben Sie in das Suchfeld GenreID aus.
4. Verwenden den Symbol "Weiter",   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   Navigieren Sie zu der folgenden select-Kennung:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Erweitern Sie den letzten Optionswert.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Im folgenden code in der *Scripts\chooseGenre.js* Datei zeigt, wie der **Hinzufügen eines neuen "Genre"** Schaltfläche ruft verbunden, um das Click-Ereignis, und wie die **Hinzufügen eines neuen "Genre"** Dialogfeld können Sie erstellt.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

Die erste Zeile erstellt eine auf-Funktion, die angefügt werden, um die **Hinzufügen eines neuen "Genre"** Schaltfläche. Das folgende Markup aus der Views\StoreManager\\_ChooseGenre.cshtml-Datei zeigt wie die **Hinzufügen eines neuen "Genre"** Schaltfläche erstellt wird:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Die Load-Methode erstellt und öffnet das Dialogfeld "hinzufügen" Genre "" und ruft die jQuery `parse` Methode, sodass der Client von Überprüfungen auf Daten in das Dialogfeld eingegeben haben.

In diesem Abschnitt haben Sie gelernt, wie ein Dialogfeld erstellen, die verwendet werden kann, um die neue Kategoriedaten in einer select-Liste hinzufügen können. Sie können das gleiche Verfahren zum Erstellen der Benutzeroberfläche zum Hinzufügen eines neuen Interpreten der Auswahlliste der Künstler diese geändert hat folgen. In diesem Tutorial erhält einen Überblick über das Arbeiten mit der ASP.NET MVC-HTML-Hilfsobjekt **DropDownList**. Weitere Informationen zum Arbeiten mit der **DropDownList**, finden Sie unter folgenden Referenzabschnitt hinzufügen. Informieren Sie uns, wenn Sie fanden dieses Lernprogramm hilfreich.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Zusätzliche Referenzen

- [ASP.NET MVC – kaskadierenden Dropdownlisten Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) von [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Ausgewählte](http://harvesthq.github.com/chosen/) eine JavaScript-Plug-Ins, die mehrere Elemente auswählen und Filtern zu unterstützen.

### <a name="contributors"></a>Contributors

- [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Bearbeiter

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Priester
- Tom Dykstra

> [!div class="step-by-step"]
> [Vorherige](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
