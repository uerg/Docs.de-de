---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Arbeiten mit Bildern in einer ASP.NET Web Pages (Razor)-Website | Microsoft-Dokumentation
author: tfitzmac
description: Das Kapitel zeigt, wie Sie hinzufügen, anzeigen und Bearbeiten von Bildern (Größe ändern, spiegeln und Hinzufügen von Wasserzeichen) auf Ihrer Website.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 32cfb9cc3c3eabcd6d724e05b85c5bf1160a39f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362926"
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Bildern in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel erfahren Sie, wie Sie hinzufügen, anzeigen und Bearbeiten von Bildern (Größe ändern, spiegeln und Hinzufügen von Wasserzeichen) auf einer Website für ASP.NET Web Pages (Razor).
> 
> Sie lernen Folgendes:
> 
> - Informationen zum Hinzufügen eines Bilds zu einer Seite dynamisch.
> - Informationen zum Benutzer ein Bild hochladen können.
> - Informationen zum Ändern der Größe ein Bild.
> - So kippen oder Drehen eines Bilds.
> - Informationen zum Hinzufügen eines Wasserzeichens zu einem Bild.
> - So verwenden Sie ein Bild als Wasserzeichen.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Funktionen dar:
> 
> - Die `WebImage` Helper.
> - Die `Path` -Objekt, das Methoden bereitstellt, mit denen Sie die Pfad-und Dateinamen zu ändern.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> In diesem Tutorial funktioniert auch mit WebMatrix 3.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Hinzufügen eines Bilds zu einer Webseite dynamisch

Sie können Images auf Ihrer Website und für einzelne Seiten hinzufügen, während der Entwicklung der Websites. Sie können auch Benutzer Hochladen von Bildern, die möglicherweise nützlich für Aufgaben wie das, und sie ein Profilfoto hinzufügen lassen.

Wenn ein Image bereits auf Ihrer Website verfügbar ist, und Sie nur auf einer Seite anzeigen möchten, verwenden Sie eine HTML `<img>` Element wie folgt:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

In einigen Fällen müssen Sie jedoch in der Lage, zum Anzeigen von Bildern dynamisch sein &#8212; d. h., Sie wissen nicht, was Bild, um anzuzeigen, bis die Seite ausgeführt wird.

Das Verfahren in diesem Abschnitt veranschaulicht, wie ein Bild im laufenden Betrieb anzuzeigen, in dem Benutzer den Namen der Abbilddatei aus einer Liste mit imagenamen angeben. Sie wählen Sie den Namen des Bilds aus einer Dropdown-Liste, und das Bild, das sie ausgewählt wird angezeigt, wenn sie die Seite senden.

![[Image]](9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Erstellen Sie eine neue Website in WebMatrix.
2. Hinzufügen einer neuen Seite, die mit dem Namen *DynamicImage.cshtml*.
3. Klicken Sie im Stammordner der Website, fügen Sie einen neuen Ordner hinzu, und nennen Sie sie *Images*.
4. Fügen Sie vier Images, die *Images* Ordner, die Sie gerade erstellt haben. (Keine Bilder haben Sie praktisch ist, werden, aber sie sollte auf eine Seite passen.) Benennen Sie die Grafiken *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, und *Photo4.jpg*. (Sie nicht verwendet, *Photo4.jpg* in dieser Prozedur, aber Sie verwenden ihn später in diesem Artikel.)
5. Stellen Sie sicher, dass vier Images nicht als schreibgeschützt gekennzeichnet sind.
6. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Der Hauptteil der Seite hat eine Dropdown-Liste (ein `<select>` Element) mit dem Namen `photoChoice`. Die Liste enthält drei Optionen, und die `value` Attribut der einzelnen Optionen für die Liste hat den Namen eines der Images, die Sie in aufnehmen die *Images* Ordner. Im Grundsatz gestattet die Liste den Benutzer, wählen Sie einen Anzeigenamen wie &quot;Foto 1&quot;, und klicken Sie dann die *jpg* Dateiname, wenn die Seite gesendet wird.

    Im Code können Sie die Auswahl des Benutzers (also des Bilddateinamens) aus der Liste abrufen, indem Sie beim Lesen der `Request["photoChoice"]`. Zuerst angezeigt, wenn überhaupt eine Auswahl vorhanden ist. Wenn vorhanden ist, erstellen Sie einen Pfad für das Bild, das der den Namen des Ordners für die Bilder und den Namen des Benutzers Image-Datei besteht. (Wenn Sie versucht haben, erstellen Sie einen Pfad ein, aber es nichts in gäbe `Request["photoChoice"]`, erhalten Sie einen Fehler.) Dadurch wird ein relativer Pfad wie folgt:

    *Bilder/Photo1.jpg*

    Der Pfad befindet sich in der Variablen, die mit dem Namen `imagePath` , die Sie weiter unten auf der Seite benötigen.

    Im Text, es gibt auch eine `<img>` -Element, das verwendet wird, um das Bild anzuzeigen, die der Benutzer ausgewählt. Die `src` -Attributsatz ist nicht auf einen Dateinamen oder die URL ein, wie Sie vornehmen möchten, um ein statisches Element anzuzeigen. Stattdessen wird er zum festgelegt `@imagePath`, was bedeutet, dass er seinen Wert aus dem Pfad erhält Sie im Code festlegen.

    Beim ersten, die die Seite ausgeführt wird, ist allerdings kein Bild angezeigt werden, da der Benutzer etwas ausgewählt noch nicht. Dies würde normalerweise bedeuten, dass die `src` Attribut wären leer und das Bild als ein rotes angezeigt würde &quot;x&quot; (oder was auch immer der Browser rendert, wenn sie ein Image nicht gefunden). Um dies zu verhindern, fügen Sie der `<img>` Element im eine `if` blockieren, die überprüft, um anzuzeigen, ob die `imagePath` Variable enthält alles. Der Benutzer eine Auswahl getroffen `imagePath` enthält den Pfad. Wenn der Benutzer kein Bild auswählen oder wenn das erste Mal die Seite angezeigt wird, die `<img>` Element nicht selbst gerendert.
7. Speichern Sie die Datei, und führen Sie die Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
8. Wählen Sie ein Image aus der Dropdown-Liste, und klicken Sie dann auf **Beispielbild**. Stellen Sie sicher, dass verschiedene Bilder für andere Optionen angezeigt.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Hochladen eines Image

Im vorherige Beispiel wurde gezeigt, wie ein Bild dynamisch angezeigt, aber es hat funktioniert nur mit Images, die bereits auf Ihrer Website waren. Dieses Verfahren veranschaulicht, damit Benutzer ein Bild hochladen, die dann auf der Seite angezeigt werden. In ASP.NET können Sie Bilder im Handumdrehen mit Ändern der `WebImage` Hilfsmethode, die über Methoden, mit denen verfügt, Sie erstellen, bearbeiten und Speichern von Bildern. Die `WebImage` Hilfsprogramm unterstützt alle allgemeinen Web Image Dateitypen, einschließlich *jpg*, *PNG*, und *BMP*. In diesem Artikel verwenden Sie *jpg* Images, aber Sie können jede der Bildtypen verwenden.

![[Image]](9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Fügen Sie eine neue Seite hinzu, und nennen Sie sie *UploadImage.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Der Textkörper des Texts verfügt über eine `<input type="file">` -Element, das Benutzer eine Datei zum Hochladen auswählen kann. Beim Klicken auf **senden**, die Datei, die sie ausgewählt, und das Formular übermittelt wird.

    Rufen Sie das hochgeladene Bild, das Sie verwenden die `WebImage` Hilfsmethode, die alle möglichen nützliche Methoden zum Arbeiten mit Bildern verfügt. Verwenden Sie insbesondere `WebImage.GetImageFromRequest` erhalten das hochgeladene Bild (sofern vorhanden), und speichern es in einer Variablen namens `photo`.

    Ein Großteil der Aufgaben in diesem Beispiel umfasst die abrufen und Festlegen von Datei-und Pfadnamen. Das Problem ist, dass Sie erhalten den Namen (und nur der Name) des Bilds, das der Benutzer hochgeladen, und erstellen Sie einen neuen Pfad für die, in dem man das Abbild speichern möchten. Da Benutzer möglicherweise mehrere Images, die den gleichen Namen haben hochladen können, verwenden Sie ein wenig zusätzlichem Code, um eindeutige Namen zu erstellen, und stellen Sie sicher, dass Benutzer nicht vorhandener Bilder überschrieben.

    Wenn ein Bild, tatsächlich hochgeladen wurde (der Test `if (photo != null)`), erhalten Sie den Namen des Images aus des Images des `FileName` Eigenschaft. Wenn der Benutzer das Bild hochlädt `FileName` enthält die ursprünglichen des Benutzers, der den Pfad aus dem Computer des Benutzers enthält. Es könnte folgendermaßen aussehen:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Alle diese Pfadinformationen, nicht jedoch soll &#8212; sollen einfach nur den tatsächlichen Dateinamen (*SamplePhoto1.jpg*). Sie können sich nur auf die Datei aus einem Pfad entfernen, mit der `Path.GetFileName` -Methode wie folgt:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Anschließend erstellen Sie einen neuen eindeutigen Dateinamen durch eine GUID in den ursprünglichen Namen hinzufügen. (Weitere Informationen über GUIDs finden Sie unter [zu GUIDs](#SB_AboutGUIDs) weiter unten in diesem Artikel.) Klicken Sie dann erstellen Sie einen vollständigen Pfad an, dem Sie verwenden können, um das Image zu speichern. Der Speichervorgang Pfad besteht aus den neuen Dateinamen an, den Ordner (Images) und den aktuellen Speicherort der Website.

    > [!NOTE]
    > In der Reihenfolge für Ihren Code zum Speichern von Dateien in die *Images* Ordner der Anwendung benötigt Lese-/ Schreibzugriff Berechtigungen für diesen Ordner. Auf dem Entwicklungscomputer ist dies nicht in der Regel ein Problem. Wenn Sie Ihre Website eines Hostinganbieters Webserver veröffentlichen, müssen Sie jedoch die Berechtigungen explizit festzulegen. Wenn Sie diesen Code auf einem Hostinganbieter Server ausführen und Fehler auftreten, überprüfen Sie mit der hosting-Anbieter finden Sie heraus, wie Sie diese Berechtigungen festlegen.

    Schließlich übergeben Sie den Speichervorgang Pfad zu der `Save` Methode der `WebImage` Helper. Dadurch wird das hochgeladene Bild unter dem neuen Namen gespeichert. Der Speichervorgang Methode sieht wie folgt aus: `photo.Save(@"~\" + imagePath)`. Wird der vollständige Pfad angefügt `@"~\"`, gibt den aktuellen Speicherort der Website. (Informationen zu den `~` -Operator, finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Wie im vorherigen Beispiel enthält der Hauptteil der Seite ein `<img>` Element zur Anzeige des Bilds. Wenn `imagePath` festgelegt wurde, wird die `<img>` Element gerendert wird und die zugehörige `src` -Attributsatz auf die `imagePath` Wert.
3. Führen Sie die Seite in einem Browser.
4. Hochladen von Images, und stellen Sie sicher, dass er auf der Seite angezeigt wird.
5. Öffnen Sie auf der Website der *Images* Ordner. Sie sehen, dass eine neue Datei hinzugefügt wurde, dessen Name wie folgt aussehen: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Dies ist das Bild, das Sie mit einer GUID, die auf den Namen das Präfix hochgeladen haben. (Eine eigene Datei haben Sie eine andere GUID und wahrscheinlich heißt etwas anderes als *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Informationen zu GUIDs
> 
> Eine GUID (globally unique ID) ist ein Bezeichner, der in der Regel in einem Format wie folgt gerendert wird: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Die Zahlen und Buchstaben (von A bis F) unterscheiden sich für jede GUID, aber sie folgen diesem Muster mithilfe von Gruppen von 8-4-4-4-12 Zeichen ein. (Eine GUID ist technisch gesehen eine 16-Byte/128-Bit-Zahl.) Wenn Sie eine GUID benötigen, können Sie die speziellen Code aufrufen, die eine GUID generiert. Die Idee hinter GUIDs ist, dass zwischen der beachtliche Größe der Zahl (3,4 x 10<sup>38</sup>) und den Algorithmus zum Generieren von es, die sich ergebende Anzahl wird praktisch garantiert eine von der Art. GUIDs sind daher eine gute Möglichkeit, den Namen für die Dinge zu generieren, wenn Sie sicherstellen müssen, dass Sie den gleichen Namen nicht zweimal verwenden. Der Nachteil ist natürlich, dass GUIDs nicht besonders benutzerfreundlich, damit sie tendenziell eher verwendet werden, wenn der Name nur in Code verwendet wird.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Größenänderungen bei Bildern

Wenn Ihre Website Images von einem Benutzer akzeptiert werden, empfiehlt es sich, die Bilder Größe zu ändern, bevor Sie anzeigen oder speichern Sie sie. Sie können wieder verwenden die `WebImage` -Hilfsprogramm für diese.

Diese Prozedur zeigt, wie die Größe ein hochgeladenen Bilds zum Erstellen einer Miniaturansicht, und speichern Sie die Miniaturansicht und das Originalbild in der Website ändern. Sie die Miniaturansicht anzuzeigen, auf der Seite und verwenden einen Link, um Benutzer umzuleiten, auf das Bild in voller Größe.

![[Image]](9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Hinzufügen einer neuen Seite, die mit dem Namen *Thumbnail.cshtml*.
2. In der *Images* Ordner, erstellen Sie einen Unterordner mit dem Namen *Thumbs*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Dieser Code ist ähnlich wie der Code aus dem vorherigen Beispiel. Der Unterschied besteht darin, dass dieser Code das Bild, zweimal, einmal normalerweise speichert einmal nach der Erstellung einer Miniaturansichten Kopie des Abbilds. Zuerst Sie das hochgeladene Bild abrufen und speichern Sie ihn in das *Images* Ordner. Sie erstellen dann einen neuen Pfad für die Miniaturansicht. Um die Miniaturansicht tatsächlich zu erstellen, rufen Sie die `WebImage` des Hilfsprogramms `Resize` Methode zum Erstellen eines Images 60 x 60 Pixel. Das Beispiel zeigt, wie Sie das Seitenverhältnis beibehalten, und wie Sie verhindern, können das Abbild (für den Fall, dass die neue Größe tatsächlich das Bild vergrößern würde) vergrößert wird. Das angepasste Bild wird dann gespeichert, der *Thumbs* Unterordner.

    Am Ende des Markups, verwenden Sie dieselben `<img>` Element mit dem dynamischen `src` -Attribut, das Sie in den vorherigen Beispielen gesehen haben, bedingt auf das Bild angezeigt. In diesem Fall werden die Miniaturansicht angezeigt. Verwenden Sie Sie auch eine `<a>` Element um einen Link zu der großen Version des Images zu erstellen. Wie bei der `src` Attribut der `<img>` -Element legen Sie Sie der `href` Attribut der `<a>` Element dynamisch an den Inhalt `imagePath`. Um sicherzustellen, dass der Pfad als URL arbeiten kann, übergeben Sie `imagePath` auf die `Html.AttributeEncode` -Methode, die reservierte Zeichen in den Pfad in Zeichen konvertiert wird, die in einer URL ok sind.
4. Führen Sie die Seite in einem Browser.
5. Foto hochladen, und stellen Sie sicher, dass die Miniaturansicht angezeigt wird.
6. Klicken Sie auf die Miniaturansicht, um das Bild in voller Größe anzuzeigen.
7. In der *Images* und *Bilder/Daumen*, beachten Sie, dass neue Dateien hinzugefügt wurden.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Drehen und Kippen eines Bilds

Die `WebImage` Hilfsprogramm können Sie auch kippen und Drehen von Bildern. Diese Prozedur zeigt, wie Sie ein Image vom Server abrufen, spiegeln das Bild auf dem Kopf stehend (vertikal), speichern Sie sie und klicken Sie dann das gespiegelte Bild auf der Seite angezeigt wird. In diesem Beispiel verwenden Sie nur eine Datei, die Sie bereits, auf dem Server verfügen (*Photo2.jpg*). In einer echten Anwendung würden Sie wahrscheinlich kippen Sie ein Bild, dessen Name Sie dynamisch abrufen, wie Sie in den vorherigen Beispielen.

![[Image]](9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Hinzufügen einer neuen Seite, die mit dem Namen *FlipImage.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Der Code verwendet die `WebImage` Helper ein Bild vom Server abgerufen. Sie erstellen Sie den Pfad zu dem Bild mit derselben Technik, die Sie für das Speichern von Bildern in vorherigen Beispielen verwendete auf und übergeben diesen Pfad bei der Erstellung ein Bild mit `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Wenn ein Bild gefunden wird, erstellen Sie einen neuen Pfad und Dateinamen ein, wie in vorherigen Beispielen. Um das Bild zu kippen, rufen Sie die `FlipVertical` Methode, und klicken Sie dann Sie speichern das Abbild erneut.

    Das Bild wird wieder auf der Seite angezeigt, mit der `<img>` -Element mit dem `src` -Attributsatz auf `imagePath`.
3. Führen Sie die Seite in einem Browser. Das Bild für *Photo2.jpg* stehend angezeigt wird.
4. Aktualisieren Sie die Seite, oder fordern Sie die Seite, um festzustellen, dass das Bild rechts gekippt wieder einsatzbereit ist.

Um ein Bild zu drehen, verwenden Sie den gleichen Code, außer dass statt der `FlipVertical` oder `FlipHorizontal`, rufen Sie `RotateLeft` oder `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Hinzufügen eines Wasserzeichens zu einem Bild

Beim Hinzufügen von Bildern zu Ihrer Website, empfiehlt es sich um ein Wasserzeichen auf das Bild hinzufügen, bevor Sie speichern oder auf einer Seite angezeigt. Benutzer verwenden häufig Wasserzeichen, copyright-Informationen zu einem Bild hinzufügen oder deren Namen Business angekündigt werden soll.

![[Image]](9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Hinzufügen einer neuen Seite, die mit dem Namen *Watermark.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Dieser Code ist wie der Code in die *FlipImage.cshtml* Seite von oben (obwohl dies Zeit verwendet die *Photo3.jpg* Datei). Um das Wasserzeichen hinzuzufügen, rufen Sie die `WebImage` des Hilfsprogramms `AddTextWatermark` -Methode auf, bevor Sie das Abbild zu speichern. Im Aufruf von `AddTextWatermark`, übergeben Sie den Text &quot;Meine Wasserzeichen&quot;, legen Sie die Schriftfarbe auf Gelb, und die Schriftartfamilie auf Arial festgelegt. (Obwohl es hier nicht dargestellt, ist die `WebImage` Hilfsprogramm können Sie auch Deckkraft, Schriftfamilie und Schriftgrad und die Position des Wasserzeichentexts angeben.) Wenn Sie das Image speichern darf es nicht schreibgeschützt sein.

    Wie Sie vor dem gesehen haben, das Bild wird angezeigt auf der Seite mit den `<img>` Element mit dem das Src-Attribut festgelegt werden, um `@imagePath`.
3. Führen Sie die Seite in einem Browser. Beachten Sie den Text "My Wasserzeichens" unter der unteren rechten Ecke des Bilds ein.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Verwenden eines Bilds als ein Wasserzeichen

Anstatt Sie Text für ein Wasserzeichen verwenden, können Sie ein anderes Image verwenden. Benutzer hat die Images wie z.B. ein Unternehmenslogo manchmal als Wasserzeichen verwenden oder ein Wasserzeichenbild anstelle von Text für die copyright-Informationen verwendet.

![[Image] ](9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Hinzufügen einer neuen Seite, die mit dem Namen *ImageWatermark.cshtml*.
2. Fügen Sie ein Bild, um die *Images* Ordner an, dass Sie als ein Logo verwenden können, und nennen Sie das Image *MyCompanyLogo.jpg*. Dieses Image muss ein Bild, das Sie klar ersichtlich, wenn es auf 80 Pixel breit und 20 Pixel hoch festgelegt ist.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite durch Folgendes: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Dies ist eine andere Variante auf den Code aus den vorherigen Beispielen. In diesem Fall rufen Sie `AddImageWatermark` das Zielabbild Wasserzeichenbild hinzu (*Photo3.jpg*), bevor Sie das Abbild zu speichern. Beim Aufruf `AddImageWatermark`, legen Sie seine Breite auf 80 Pixel und die Höhe und 20 Pixel. Die *MyCompanyLogo.jpg* Bild wird horizontal zentriert ausgerichtet und vertikal am unteren Rand der zielimage ausgerichtet. Die Durchlässigkeit auf 100 % festgelegt ist, und die Auffüllung wird auf 10 Pixel festgelegt. Wenn das Wasserzeichenbild größer als das Zielbild ist, geschieht nichts. Wenn das Wasserzeichenbild größer als das Zielbild ist und den Abstand für das Image Wasserzeichen auf Null festlegen, wird das Wasserzeichen ignoriert.

    Wenn Sie das Image mit zuvor Anzeigen der `<img>` -Element und ein dynamisches `src` Attribut.
4. Führen Sie die Seite in einem Browser. Beachten Sie, dass das Wasserzeichenbild am unteren Rand des Hauptbilds angezeigt wird.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Arbeiten mit Dateien in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202896)

[Einführung zu ASP.NET Web Pages-Programmierung mit Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
