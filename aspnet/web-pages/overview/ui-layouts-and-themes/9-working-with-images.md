---
uid: web-pages/overview/ui-layouts-and-themes/9-working-with-images
title: Arbeiten mit Bildern an einem Standort der ASP.NET Web Pages (Razor) | Microsoft Docs
author: tfitzmac
description: "Das Kapitel zeigt, wie Sie hinzufügen, anzeigen und Bearbeiten von Bildern (Größe ändern, spiegeln und Hinzufügen von Wasserzeichen) in Ihrer Website."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 778c4e58-4372-4d25-bab9-aec4a8d8e38d
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/9-working-with-images
msc.type: authoredcontent
ms.openlocfilehash: 74838dd364a43f3f4c966c1417d0f0b2cc242f28
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="working-with-images-in-an-aspnet-web-pages-razor-site"></a>Arbeiten mit Bildern an einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diesem Artikel erfahren Sie, wie Sie hinzufügen, anzeigen und Bearbeiten von Bildern (Größe ändern, spiegeln und Hinzufügen von Wasserzeichen) in einer ASP.NET Web Pages (Razor)-Website.
> 
> Lernen Sie:
> 
> - Wie Sie ein Bild dynamisch zu einer Seite hinzufügen.
> - Wie können Benutzer ein Image hochladen.
> - So ändern Sie die Größe ein Bild.
> - Vorgehensweise spiegeln oder drehen ein Bild.
> - Wie Sie ein Wasserzeichen auf ein Bild hinzufügen.
> - Wie Sie ein Bild als Wasserzeichen verwenden.
> 
> Hierbei handelt es sich um den ASP.NET Programmieren von Funktionen, die im Artikel:
> 
> - Die `WebImage` Helper.
> - Die `Path` -Objekt, das Methoden bereitstellt, mit denen Sie den Pfad und den Dateinamen zu ändern.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 2
>   
> 
> Dieses Lernprogramm funktioniert auch mit WebMatrix-3.


<a id="Adding_an_Image"></a>
## <a name="adding-an-image-to-a-web-page-dynamically"></a>Hinzufügen eines Bilds zu einer Webseite dynamisch

Sie können Bilder zu Ihrer Website und für einzelne Seiten hinzufügen, während der Entwicklung der Website. Ferner können Benutzer, die Bilder, hochgeladen, die für Aufgaben wie z. B. ein Profilfoto hinzufügen birgt dies nützlich sein können.

Wenn ein Image bereits auf Ihrer Website verfügbar ist, und Sie nur auf einer Seite anzeigen möchten, verwenden Sie ein HTML `<img>` Element wie folgt:

[!code-html[Main](9-working-with-images/samples/sample1.html)]

In einigen Fällen müssen Sie jedoch in der Lage, Anzeige von Bildern dynamisch sein &#8212; d. h. wissen nicht Sie, welches Bild angezeigt, bis die Seite ausgeführt wird.

Die Verfahren in diesem Abschnitt wird gezeigt, wie ein Bild im Handumdrehen angezeigt, in dem Benutzer den Bilddateinamen aus einer Liste der imagenamen angeben, wird. Benutzer den Namen des Bilds aus einer Dropdownliste auswählen, und wenn sie die Seite senden, wird das Bild, das sie ausgewählt angezeigt.

![[Image] ] (9-working-with-images/_static/image1.jpg "ch9images 1.jpg")

1. Erstellen Sie eine neue Website in WebMatrix.
2. Fügen Sie eine neue Seite mit dem Namen *DynamicImage.cshtml*.
3. Klicken Sie im Stammordner der Website, fügen Sie einen neuen Ordner hinzu, und nennen Sie sie *Bilder*.
4. Hinzufügen von vier Bilder für die *Bilder* soeben erstellten Ordner. (Keine Bilder haben Sie praktisch führen wird, aber sie sollten auf eine Seite passen.) Benennen Sie die Bilder *Photo1.jpg*, *Photo2.jpg*, *Photo3.jpg*, und *Photo4.jpg*. (Sie wird nicht verwenden, *Photo4.jpg* in dieser Prozedur, aber Sie verwenden es später in diesem Artikel.)
5. Stellen Sie sicher, dass die vier Bilder nicht als schreibgeschützt markiert sind.
6. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden:

    [!code-cshtml[Main](9-working-with-images/samples/sample2.cshtml)]

    Der Text der Seite enthält eine Dropdown-Liste (eine `<select>` Element) mit der Bezeichnung `photoChoice`. Die Liste enthält drei Optionen und die `value` Attribut der einzelnen Optionen für die Liste hat den Namen eines der Bilder aus, das Sie in einfügen die *Bilder* Ordner. Im Wesentlichen kann die Liste den Benutzer, wählen Sie einen Anzeigenamen wie &quot;Foto 1&quot;, und übergibt dann die *jpg* Dateiname, wenn die Seite übermittelt wird.

    Im Code können Sie die Auswahl des Benutzers (also den Bilddateinamen) aus der Liste abrufen, indem Sie lesen `Request["photoChoice"]`. Zuerst angezeigt, wenn eine Auswahl überhaupt vorhanden ist. Wenn Sie vorhanden ist, erstellen Sie einen Pfad für das Bild, das der den Namen des Ordners für die Bilder und Bilddateinamen des Benutzers besteht. (Wenn Sie versucht haben, erstellen Sie einen Pfad, aber es nichts in wurde `Request["photoChoice"]`, erhalten Sie eine Fehlermeldung.) Daraus ergibt sich ein relativer Pfad wie folgt:

    *Bilder/Photo1.jpg*

    Der Pfad befindet sich in der Variablen, die mit dem Namen `imagePath` , die Sie weiter unten auf der Seite benötigen.

    Im Hauptteil, es gibt auch eine `<img>` -Element, das verwendet wird, um das Bild angezeigt wird, die der Benutzer ausgewählt. Die `src` Attribut ist nicht auf einen Dateinamen oder eine URL festgelegt, wie Sie vornehmen möchten, um ein statisches Element anzuzeigen. Stattdessen wird er auf festgelegt `@imagePath`, was bedeutet, dass er seinen Wert aus dem Pfad ruft im Code festlegen.

    Beim ersten, der die Seite ausgeführt wird, jedoch besteht kein Bild angezeigt, da der Benutzer nichts ausgewählt wurde nicht. Dies würde normalerweise bedeuten, dass die `src` wäre Attribut leer und das Bild als ein rotes angezeigt würde &quot;x&quot; (oder nach Belieben der Browser rendert, wenn ein Image gefunden werden kann). Vermeiden möchten, setzen Sie die `<img>` Element in eine `if` Block, der testet, ob die `imagePath` Variable hat nichts darin. Wenn der Benutzer eine Auswahl getroffen `imagePath` enthält den Pfad. Wenn der Benutzer hat nicht ein Bild auswählen, oder wenn das erste Mal wird die Seite angezeigt, die `<img>` Element befindet sich nicht selbst gerendert.
7. Speichern Sie die Datei, und führen Sie die Seite in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.)
8. Wählen Sie ein Image aus der Dropdown-Liste, und klicken Sie dann auf **Beispielbild**. Stellen Sie sicher, dass Sie verschiedene Bilder für die verschiedenen Auswahlmöglichkeiten finden Sie unter.

<a id="Uploading_an_Image"></a>
## <a name="uploading-an-image"></a>Ein Image hochladen

Im vorherige Beispiel wurde gezeigt, wie ein Bild angezeigt wird, dynamisch, aber es funktioniert nur mit Bildern, die bereits auf Ihrer Website waren. Dieses Verfahren veranschaulicht, wie Benutzer ein Image hochladen, die dann auf der Seite angezeigt werden können. In ASP.NET können Sie Bearbeiten von Bildern auf einfache Weise mithilfe der `WebImage` Helper, verfügt über Methoden, mit denen Sie erstellen, bearbeiten und Speichern von Bildern. Die `WebImage` Helper unterstützt alle die allgemeine Web Bilddateitypen, einschließlich *jpg*, *PNG*, und *.bmp*. Verwenden Sie in diesem Artikel *jpg* Bilder, aber Sie können den Image-Typen verwenden.

![[Image] ] (9-working-with-images/_static/image2.jpg "ch9images 2.jpg")

1. Fügen Sie eine neue Seite hinzu, und nennen Sie sie *UploadImage.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](9-working-with-images/samples/sample3.cshtml)]

    Der Textkörper des Texts wurde ein `<input type="file">` -Element, das Benutzer eine Datei zum Hochladen auswählen kann. Wenn die klicken Sie auf **Absenden**, die Datei, die sie ausgewählt wird zusammen mit der Form übermittelt.

    Um ein hochgeladenes Image zu erhalten, verwenden Sie die `WebImage` Hilfsfunktion, die alle möglichen nützlichen Methoden zum Arbeiten mit Bildern hat. Verwenden Sie insbesondere `WebImage.GetImageFromRequest` erhalten ein hochgeladenes Image (sofern vorhanden), und speichern es in einer Variablen namens `photo`.

    Ein Großteil der Arbeit in diesem Beispiel beinhaltet abrufen und Festlegen von Datei-und Pfadnamen auf. Das Problem ist, dass Sie verwenden möchten, rufen Sie den Namen (und nur der Name) des Bilds, die der Benutzer hochgeladen haben, und erstellen Sie einen neuen Pfad für Sie zum Speichern des Abbilds wo an. Da Benutzer möglicherweise mehrere Abbilder, die den gleichen Namen haben hochladen können, verwenden Sie viel zusätzlichen Code zum Erstellen von eindeutiger Namen und stellen Sie sicher, dass Benutzer nicht vorhandener Bilder überschrieben.

    Wenn tatsächlich ein Bild hochgeladen wurde (der Test `if (photo != null)`), Sie erhalten den Namen des Abbilds aus des Images `FileName` Eigenschaft. Wenn der Benutzer das Bild hochlädt `FileName` enthält den Namen des Benutzers ursprünglichen, der den Pfad von dem Computer des Benutzers enthält. Es kann wie folgt aussehen:

    *C:\Users\Joe\Pictures\SamplePhoto1.jpg*

    Sie möchten schließlich nicht alle dieser Pfadinformationen, obwohl &#8212; Sie möchten nur den tatsächlichen Dateinamen (*SamplePhoto1.jpg*). Sie müssen nur die Datei aus einem Pfad entfernen können, mithilfe der `Path.GetFileName` -Methode, wie folgt:

    [!code-csharp[Main](9-working-with-images/samples/sample4.cs)]

    Anschließend erstellen Sie einen neuen eindeutigen Dateinamen an, indem Sie eine GUID an den ursprünglichen Namen. (Weitere Informationen über GUIDs finden Sie unter [zu GUIDs](#SB_AboutGUIDs) weiter unten in diesem Artikel.) Anschließend erstellen Sie einen vollständigen Pfad, den Sie verwenden können, um das Bild zu speichern. Der Speichervorgang Pfad setzt sich aus den neuen Dateinamen an, den Ordner (Images) und den aktuellen Speicherort der Website.

    > [!NOTE]
    > In der Reihenfolge für den Code zum Speichern von Dateien in den *Bilder* Ordner der Anwendung muss Lese-/ Schreibzugriff Berechtigungen für diesen Ordner. Auf Ihrem Computer ist dies nicht in der Regel ein Problem. Wenn Sie Ihre Website mit einem Hostinganbieter Webserver veröffentlichen, müssen Sie diese Berechtigungen explizit festzulegen. Wenn Sie diesen Code auf einem Hostinganbieter Server ausführen und Fehler gemeldet werden, überprüfen Sie mit dem Hostinganbieter, herauszufinden, wie diese Berechtigungen festlegen.

    Übergeben Sie Sie abschließend den Speichervorgang Pfad zu der `Save` Methode der `WebImage` Helper. Dadurch werden die hochgeladene Image unter dem neuen Namen gespeichert. Der Speichervorgang Methode sieht wie folgt: `photo.Save(@"~\" + imagePath)`. Der vollständige Pfad angefügt `@"~\"`, also in den aktuellen Speicherort der Website. (Informationen zu den `~` -Operator, finden Sie unter [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#ID_WorkingWithFileAndFolderPaths).)

    Wie im vorherigen Beispiel enthält der Hauptteil der Seite ein `<img>` Element zur Anzeige des Bilds. Wenn `imagePath` festgelegt wurde, wird die `<img>` Element gerendert wird und die zugehörige `src` -Attributsatz zur der `imagePath` Wert.
3. Führen Sie die Seite in einem Browser aus.
4. Laden Sie ein Bild hoch, und stellen Sie sicher, dass er auf der Seite angezeigt wird.
5. Öffnen Sie an Ihrem Standort die *Bilder* Ordner. Sie sehen, dass eine neue Datei hinzugefügt wurde, deren Dateiname etwa wie folgt aussieht: 

    *45ea4527-7ddd-4965-b9ca-c6444982b342\_MyPhoto.png*

    Hierbei handelt es sich um das Image, das Sie mit einer GUID, die den Namen vorangestellt hochgeladen. (Eine eigene Datei müssen Sie einen anderen GUID und wahrscheinlich heißt etwas anderes als *MyPhoto.png*.)

> [!TIP] 
> 
> <a id="SB_AboutGUIDs"></a>
> ### <a name="about-guids"></a>Informationen zu GUIDs
> 
> Eine GUID (global eindeutige ID) ist ein Bezeichner, der in der Regel in einem Format wie folgt gerendert wird: `936DA01F-9ABD-4d9d-80C7-02AF85C822A8`. Die Zahlen und Buchstaben (A-F) unterscheiden sich die für jede GUID, folgen das Muster der Verwendung von Gruppen von 8-4-4-4-12 Zeichen jedoch. (Eine GUID ist technisch gesehen eine 16-Byte/128-Bit-Zahl.) Wenn Sie eine GUID benötigen, können Sie spezielle Code aufrufen, die eine GUID für Sie generiert. Die Idee GUIDs wird zwischen der beachtliche Größe der Zahl (3.4 x 10<sup>38</sup>) und der Algorithmus zum Generieren von es, die daraus resultierende Zahl ist eine Art kann praktisch garantiert. GUIDs sind daher eine gute Möglichkeit zum Generieren von Namen für Elemente, wenn Sie sicherstellen müssen, dass Sie zwei Mal den gleichen Namen verwenden wird nicht. Der Nachteil ist natürlich, dass GUIDs nicht besonders benutzerfreundlich, damit sie tendenziell eher verwendet werden, wenn der Name nur im Code verwendet wird.


<a id="Resizing_an_Image"></a>
## <a name="resizing-an-image"></a>Größenänderungen bei Bildern

Wenn Ihre Website Abbilder von einem Benutzer annimmt, empfiehlt es sich, die Bilder Größe ändern, bevor Sie anzeigen oder speichern. Vorgang können Sie die `WebImage` für dieses Hilfsprogramm.

Dieses Verfahren veranschaulicht das Ändern der Größe eines hochgeladenen Images zum Erstellen einer Miniaturansicht und speichern Sie dann die Miniaturansicht und das ursprüngliche Bild auf der Website. Sie die Miniaturansicht auf der Seite anzeigen und verwenden einen Link, um Benutzer umzuleiten, auf das Bild in voller Größe.

![[Image] ] (9-working-with-images/_static/image3.jpg "ch9images 3.jpg")

1. Fügen Sie eine neue Seite mit dem Namen *Thumbnail.cshtml*.
2. In der *Bilder* Ordner einen Unterordner mit dem Namen *Daumen*.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](9-working-with-images/samples/sample5.cshtml)]

    Dieser Code stellt ähnlich dem Code aus dem vorherigen Beispiel. Der Unterschied besteht darin, dass dieser Code das Bild zweimal, einmal normalerweise speichert einmal nach dem Erstellen einer Miniaturansichten Kopie des Abbilds. Zuerst ein hochgeladenes Image abrufen und speichern Sie sie in der *Bilder* Ordner. Sie erstellen dann einen neuen Pfad für die Miniaturansicht. Um die Miniaturansicht tatsächlich zu erstellen, rufen Sie die `WebImage` der Hilfsfunktion `Resize` Methode zum Erstellen eines Abbilds 60 x 60 Pixel. Im Beispiel wird gezeigt, wie Sie das Seitenverhältnis beibehalten und wie Sie verhindern, können das Bild wird vergrößert (für den Fall, dass die neue Größe tatsächlich das Bild größer machen würden). Das vergrößerte Bild wird dann gespeichert, der *Daumen* Unterordner.

    Am Ende des Markups, verwenden Sie die gleiche `<img>` Element mit dynamischen `src` -Attribut, das Sie in den vorherigen Beispielen gesehen haben, um das Bild bedingt anzuzeigen. In diesem Fall die Miniaturansicht angezeigt. Sie verwenden, auch ein `<a>` Element, um einen Link, um die big-Version des Images zu erstellen. Wie bei der `src` Attribut des der `<img>` Element, legen Sie die `href` Attribut des der `<a>` Element dynamisch an den Inhalt `imagePath`. Um sicherzustellen, dass der Pfad als URL arbeiten kann, übergeben Sie `imagePath` auf die `Html.AttributeEncode` -Methode, die reservierte Zeichen in den Pfad in Zeichen konvertiert wird, die in einer URL Ordnung sein.
4. Führen Sie die Seite in einem Browser aus.
5. Ein Foto hochladen, und stellen Sie sicher, dass die Miniaturansicht angezeigt wird.
6. Klicken Sie auf die Miniaturansicht, um das Bild in voller Größe anzuzeigen.
7. In der *Bilder* und *Bilder/Daumen*, beachten Sie, dass neue Dateien hinzugefügt wurden.

<a id="Rotating_and_Flipping"></a>
## <a name="rotating-and-flipping-an-image"></a>Drehen und Kippen eines Bildes

Die `WebImage` Hilfsprogramm können Sie auch kippen und Drehen von Bildern. Dieses Verfahren wird gezeigt, wie ein Bild vom Server abrufen, spiegeln das Bild stehend (vertikal), speichern Sie sie und klicken Sie dann das gespiegelte Bild auf der Seite angezeigt wird. In diesem Beispiel verwenden Sie nur eine Datei, die Sie bereits, auf dem Server verfügen (*Photo2.jpg*). In einer echten Anwendung würden Sie wahrscheinlich ein Bild Blättern, deren Namen Sie dynamisch, erhalten wie Sie in den vorherigen Beispielen ausgeführt haben.

![[Image] ] (9-working-with-images/_static/image4.jpg "ch9images 4.jpg")

1. Fügen Sie eine neue Seite mit dem Namen *FlipImage.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](9-working-with-images/samples/sample6.cshtml)]

    Der Code verwendet die `WebImage` Helper ein Bild vom Server abgerufen. Sie erstellen den Pfad zu dem Bild, das über das gleiche Verfahren, die Sie für das Speichern von Bildern in vorherigen Beispielen verwendete und, dass der Pfad zu übergeben, wenn Sie ein Bild mit erstellen `WebImage`:

    [!code-javascript[Main](9-working-with-images/samples/sample7.js)]

    Wenn ein Bild gefunden wird, erstellen Sie einen neuen Pfad und Dateinamen ein, wie Sie in vorherigen Beispielen ausgeführt haben. Um das Bild zu blättern, rufen Sie die `FlipVertical` speichern Sie das Bild-Methode, und klicken Sie dann erneut.

    Das Bild wird wieder auf der Seite angezeigt, mit der `<img>` Element mit dem `src` -Attributsatz zur `imagePath`.
3. Führen Sie die Seite in einem Browser aus. Das Bild für *Photo2.jpg* stehend angezeigt wird.
4. Aktualisieren Sie die Seite, oder bitten Sie die Seite erneut aus, um feststellen, dass das Bild rechts gekippte wieder einsatzbereit ist.

Um ein Bild zu wechseln, verwenden Sie den gleichen Code, außer dass statt der `FlipVertical` oder `FlipHorizontal`, rufen Sie `RotateLeft` oder `RotateRight`.

<a id="Adding_a_Watermark"></a>
## <a name="adding-a-watermark-to-an-image"></a>Hinzufügen von Wasserzeichen zu einem Bild

Wenn Sie Ihre Website Bilder hinzufügen, empfiehlt es sich, ein Wasserzeichen auf das Abbild hinzufügen, bevor Sie speichern oder auf einer Seite anzeigen. Benutzer verwenden häufig Wasserzeichen copyright-Informationen zu einem Bild hinzuzufügen oder um ihre Geschäftsnamen ankündigen.

![[Image] ] (9-working-with-images/_static/image5.jpg "ch9images 5.jpg")

1. Fügen Sie eine neue Seite mit dem Namen *Watermark.cshtml*.
2. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](9-working-with-images/samples/sample8.cshtml)]

    Dieser Code ist wie der Code in der *FlipImage.cshtml* Seite von oben (Obwohl dieses Mal es verwendet die *Photo3.jpg* Datei). Um das Wasserzeichen hinzuzufügen, rufen Sie die `WebImage` der Hilfsfunktion `AddTextWatermark` -Methode auf, bevor Sie das Abbild zu speichern. Im Aufruf von `AddTextWatermark`, übergeben Sie den Text &quot;Meine Wasserzeichen&quot;, legen Sie die Schriftart Farbe in Gelb und Schriftfamilie auf Arial festgelegt. (Obwohl hier nicht angezeigt wird die `WebImage` Hilfsprogramm ermöglicht auch die Angabe Deckkraft, Schriftart und Schriftgrad und die Position des Wasserzeichentexts.) Wenn Sie das Abbild zu speichern darf es nicht schreibgeschützt sein.

    Wie Sie vor dem gesehen haben, wird das Bild auf der Seite mit angezeigten der `<img>` mit dem Src-Attribut-Elementgruppe `@imagePath`.
3. Führen Sie die Seite in einem Browser aus. Beachten Sie den Text "Meine Watermark" in der unteren rechten Ecke des Bilds ein.

<a id="Using_an_Image_as_a_Watermark"></a>
## <a name="using-an-image-as-a-watermark"></a>Verwenden eines Bilds als Wasserzeichen

Anstelle von Text für ein Wasserzeichen verwenden, können Sie ein anderes Bild. Personen Bilder, z. B. ein Unternehmenslogo manchmal als Wasserzeichen verwenden, oder sie verwenden ein Wasserzeichenbild anstelle von Text aus, für die copyright-Informationen.

![[Image] ] (9-working-with-images/_static/image6.jpg "ch9images 6.jpg")

1. Fügen Sie eine neue Seite mit dem Namen *ImageWatermark.cshtml*.
2. Hinzufügen eines Bilds auf der *Bilder* Ordner, in dem Sie können als ein Logo aus, und benennen Sie das Bild *MyCompanyLogo.jpg*. Dieses Bild muss ein Bild, das Sie deutlich erkennen können, wenn er auf 80 Pixel breit und 20 Pixel hoch festgelegt ist.
3. Ersetzen Sie den vorhandenen Inhalt auf der Seite mit den folgenden: 

    [!code-cshtml[Main](9-working-with-images/samples/sample9.cshtml)]

    Dies ist eine Variante für den Code aus den vorherigen Beispielen. In diesem Fall rufen Sie `AddImageWatermark` so die Zielbild Wasserzeichenbild hinzu (*Photo3.jpg*), bevor Sie das Abbild zu speichern. Beim Aufruf `AddImageWatermark`, legen Sie seine Breite auf 80 Pixel und die Höhe auf 20 Pixel. Die *MyCompanyLogo.jpg* Bild horizontal ausgerichtet wird, in der Mitte und am unteren Rand der Zielbild vertikal ausgerichtet ist. Die Deckkraft auf 100 % und die Auffüllung wird auf 10 Pixel festgelegt. Wenn das Wasserzeichenbild größer als das Ziel-Image ist, geschieht nichts. Das Wasserzeichenbild ist größer als das Ziel-Image, und legen Sie die Auffüllung für das Image Wasserzeichen auf 0 (null), wird das Wasserzeichen ignoriert.

    Wie bereits zuvor können Sie das Bild mithilfe anzeigen, die `<img>` Element und ein dynamisches `src` Attribut.
4. Führen Sie die Seite in einem Browser aus. Beachten Sie, dass das Wasserzeichenbild am unteren Rand der wichtigsten Bild angezeigt wird.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Arbeiten mit Dateien in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202896)

[Einführung in ASP.NET Web Pages Programmierung mithilfe von Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=251587)
