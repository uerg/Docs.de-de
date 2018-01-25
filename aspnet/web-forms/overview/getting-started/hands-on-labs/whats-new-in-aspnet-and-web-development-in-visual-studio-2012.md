---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Neues in ASP.NET und Webentwicklung in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Die neue Version von Visual Studio stellt eine Reihe von Verbesserungen konzentriert sich auf die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f0818cce2a82ede80556b3471cec9d965c3e987f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Was ist neu in ASP.NET und Webentwicklung in Visual Studio 2012
====================
durch [Web Lager Team](https://twitter.com/webcamps)

> Die neue Version von Visual Studio enthält eine Reihe von Verbesserungen konzentriert sich auf die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien. Visual Studio-Editoren für CSS, JavaScript und HTML haben vollständig überarbeitet, um viele der am häufigsten Bedarf Code Aids, z. B. IntelliSense und automatische Einzug einzuschließen. Bezüglich der Leistung sind Bündelung und Minimierung jetzt integriert, wie integrierte Funktionen zum Seite problemlos reduzieren Mal geladen.
> 
> Visual Studio ermöglicht Ihnen, mit der neuesten Technologien für die Website zu arbeiten. Browserübergreifende CSS3 Ausschnitte können Sie sicherstellen, dass Ihre Website unabhängig von der Clientplattform funktioniert, bei der auch die Vorteile der neuen HTML5-Elemente und Funktionen.
> 
> Schreiben und die profilerstellung für JavaScript-Code sollten nicht mit dieser Version von Visual Studio einfacher sein. IntelliSense-Listen, die integrierten Funktionen des XML-Dokumentation und Navigation sind jetzt für JavaScript-Code verfügbar. Sie haben jetzt den JavaScript-Katalog jederzeit griffbereit. Darüber hinaus können Sie überprüfen ECMAScript5-Kompatibilität mit Skripts und Syntaxfehler in einem frühen Stadium zu erkennen.
> 
> Zuletzt jedoch nicht mindestens implementiert diese Version von Visual Studio, integrierte Bündelung und Minimierung. Ihre Skriptdateien und Stylesheets werden verpackt und komprimiert, sodass die Website schneller ausgeführt.
> 
> Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktische Übungseinheiten, erfahren Sie, wie Sie:

- Verwenden Sie die neuen Features und Verbesserungen in der CSS-editor
- Verwenden Sie die neuen Features und Verbesserungen in der HTML-editor
- Verwenden Sie die neuen Features und Verbesserungen in der JavaScript-editor
- Konfigurieren und Verwenden von Bündelung und Minimierung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (für Setupskripts - bereits installiert unter Windows 8 und Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - oder einem kompatiblen HTML5-Browser

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheiten umfasst die folgenden Übungen durcharbeiten:

1. [Übung 1: Was ist neu in der CSS-Editor](#Exercise1)
2. [Übung 2: Was ist neu im HTML-Editor](#Exercise2)
3. [Übung 3: Was ist neu in der JavaScript-Editor](#Exercise3)
4. [Übung 4: Bündelung und Minimierung](#Exercise4)

Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Übung 1: Was ist neu in der CSS-Editor

Web-Entwickler sollten mit viele der Probleme im Zusammenhang mit der Bearbeitung von CSS-vertraut sein. Eines der größten Probleme von CSS-Stilen ist browserübergreifende Kompatibilität. Es kommt häufig vor, dass nach dem Anwenden von Stilen auf Ihrer Website, Sie bemerken, dass er unterscheidet, wenn sie in einem anderen Browser oder das Gerät zu öffnen. Aus diesem Grund können Sie eine sehr viel Zeit verbringen, beachten Sie, dass Sie schließlich in einem Browser funktioniert erleichtern, es, in der anderen aufgegliedert wird diese visual Probleme behoben.

Visual Studio enthält nun Funktionen, mit deren Hilfe Entwickler zugreifen, arbeiten und CSS-Stylesheets effektiv zu organisieren. Während dieser Übung werden Sie die neuen Funktionen für eine effektive Organisation und die Edition sowie die CSS3-Codeausschnitte für browserübergreifende Kompatibilität erfüllen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Aufgabe 1 – neue Features im Editor

In dieser Aufgabe werden die neuen Funktionen der CSS-Editor ermittelt werden. Diese neuen Editor helfen Ihnen die Ihre Produktivität zu steigern, durch den neuen intelligenten Einzug, der verbesserte Codekommentare und verbesserte IntelliSense-Liste nutzen.

1. Starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen Sie im Projektmappen-Explorer die **"Site.CSS" ändern** -Datei unter der **Stile** Ordner. Stellen Sie sicher, dass die **Texteditor** Tools sind auf der Symbolleiste angezeigt. Wählen Sie hierzu die **Ansicht** | **Symbolleisten** Menüoption, und aktivieren Sie die **Text-Editor** Optionen. Sie bemerken, dass, seit diese neue Version der **Kommentar** Schaltfläche (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) und die **entfernen Sie den Kommentar** Schaltfläche (![kommentieren-Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) für den CSS-Editor auch aktiviert sind.

    ![Aktivieren Editor und CSS-Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "-Editor und CSS-Tools aktivieren")

    *Aktivieren Editor und CSS-Tools*
3. Führen Sie einen Bildlauf im Code, und wählen Sie eine beliebige CSS-Klassendefinition. Klicken Sie auf die **Kommentar** (![Kommentar Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) Schaltfläche, um die ausgewählten Zeilen zu kommentieren. Klicken Sie auf die **entfernen Sie den Kommentar** (![kommentieren Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) Schaltfläche, um die Änderungen rückgängig zu machen.
4. Klicken Sie auf die **reduzieren** (![reduzieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) und **erweitern** (![erweitern](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) Schaltflächen befinden sich am linken Rand des Texts. Beachten Sie, dass Sie jetzt die Stile, die Sie verwenden ausblenden können, um eine übersichtlichere Ansicht haben.

    ![Reduzieren die CSS-Klassen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Reduzieren von CSS-Klassen")

    *Reduzieren die CSS-Klassen*
5. Stellen Sie sicher, dass die intelligenten Einzug-Funktion aktiviert ist. Wählen Sie die **Tools** | **Optionen** Menüoption, und wählen Sie dann die **Texteditor** | **CSS**  |  **Formatierung** Seite im linken Bereich des Bildschirms. Überprüfen Sie die **Hierarchischer Einzug** Option.

    ![Aktivieren der hierarchischen Einzug](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "aktivieren hierarchischen Einzug")

    *Aktivieren der hierarchischen Einzug*
6. Suchen der wichtigsten Klassendefinition (.main), und fügen Sie eine Formatvorlage auf das Div-Elemente. Beachten Sie, dass der Code automatisch, Unterstützung der Benutzer der übergeordneten Klassen auf einen Blick finden ausgerichtet.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchische Ausrichtung in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchische Ausrichtung in CSS")

    *Hierarchische Ausrichtung in CSS*
7. In **.main Div** Klasse, suchen Sie den Cursor am Ende der **Rahmen: 0px;** , und drücken Sie **EINGABETASTE** die IntelliSense-Liste angezeigt. Mit der Eingabe beginnen **oben** , und beachten Sie, wie die Liste gefiltert wird, während der Eingabe. Die Liste zeigt die Elemente, die enthalten **oben** an einen beliebigen Teil des Worts (In früheren Versionen von Visual Studio wird die Liste gefiltert, die von Elementen, die *beginnen* zusammen mit dem Begriff).

    ![IntelliSense-Erweiterungen in CSS-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense-Erweiterungen in CSS")

    *IntelliSense-Erweiterungen in CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Aufgabe 2: die Farbauswahl

In dieser Aufgabe werden Sie feststellen, dass der neuen CSS-Farbauswahl in Visual Studio IntelliSense integriert.

1. In **"Site.CSS" ändern,** suchen Sie die Header-Klassendefinition (.header), und platzieren Sie den Cursor neben **Hintergrundfarbe** Attribut, zwischen den &quot;:&quot; und &quot; # &quot; Zeichen in dieser Zeile des Codes **.**

    ![Suchen den Cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "suchen den Cursor")

    *Suchen den cursor*
2. Löschen der **Doppelpunkt** (:) und Schreiben Sie ihn erneut aus, um die Farbauswahl angezeigt. Beachten Sie, dass die erste Farben, die Sie sehen die am häufigsten auftretenden Farben Ihres Standorts sind. Wenn Sie die Farbe weiße klicken, wird die HTML-Farbcode (#fff) den aktuellen-Farbcode im Stylesheet ersetzen.

    ![Farbauswahl](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Farbauswahl")

    *Farbauswahl*
3. Drücken Sie die **erweitern** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) auf die Farbauswahl, um den Farbverlauf anzuzeigen, und ziehen dann den Farbverlauf Cursor, um eine andere Farbe auszuwählen. Klicken Sie danach auf die **Formatpipette** Schaltfläche und wählen Sie eine Farbe aus dem Bildschirm. Beachten Sie, dass der Wert für Hintergrundfarbe dynamisch ändert, während Sie den Cursor bewegen.

    ![Auswahl einer Farbverlauf](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Datumsauswahl Farbverlauf")

    *Auswahl einer Farbverlauf*
4. In der **Deckkraft** Schieberegler, verschieben Sie die Auswahl in die Mitte des Balkens die Deckkraft zu reduzieren. Beachten Sie, dass die Hintergrundfarbe der Wert jetzt ihrer Skala RGBA ändert.

    ![Farbauswahl Deckkraft](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Farbauswahl Deckkraft")

    *Farbauswahl Deckkraft*

    > [!NOTE]
    > Die Definition des RGBA (Rot, Grün, Blau, Alpha)-Farbe in CSS3 können Sie die Deckkraft Farbwert für ein einzelnes Element definieren. Im Gegensatz zu **Deckkraft -** ein ähnliches Attribut der CSS-  **-**  RGBA Farben werden auch mit neueren Browsern kompatibel.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Aufgabe 3 - kompatible CSS-Codeausschnitte

In dieser Aufgabe lernen Sie das browserübergreifende kompatibel CSS3 Ausschnitte zu verwenden, um einige Features in Ihrer Website zu implementieren.

1. In der **"Site.CSS" ändern** Datei, suchen Sie nach der **Header** CSS Klassendefinition (.header), und platzieren Sie den Cursor unterhalb der  **/ \*Border-Radius\* /**  Platzhalter sind, einen neuen Ausschnitt hinzufügen. Drücken Sie **EINGABETASTE** zum Anzeigen der IntelliSense-Liste und Typ **Radius** zum Filtern der Liste. Wählen Sie die **Border-Radius** aus der Liste mit einem doppelten Mausklick aus, und drücken Sie dann die **Registerkarte** Schlüssel zum Einfügen des Codeausschnitts. Geben Sie eine Radiusgröße in Pixel, und drücken Sie **EINGABETASTE**. Geben Sie z. B. **15px**.

    Die CSS3-Attribute hinzugefügt werden, indem Sie den Ausschnitt rendert abgerundeten Ränder in den meisten HTML5 Kompatibilität Browsern, einschließlich Mozilla und Browser WebKit-basiert.

    ![Mit einem Border-Radius-Ausschnitt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "mit einem Border-Radius-Ausschnitt")

    *Verwenden einen Border-Radius-Ausschnitt*
2. Übernehmen Sie die gleiche **Rahmen** Codeausschnitte in der Seite "-Stil (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Drücken Sie **F5** um die Projektmappe auszuführen. Beachten Sie, dass jeder Seite jetzt Rahmen gerundet wurde.

    ![Abgerundete Ecken](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "abgerundete Ecken")

    *Abgerundeten Ecken*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
5. Öffnen der **Custom.css** -Datei unter der **Stile** Ordner, und platzieren Sie den Cursor innerhalb **div.images Ul li Img** Klassendefinition.
6. EINGABETASTE drücken, um die IntelliSense-Liste anzuzeigen Typ **' Box-Shadow** , und drücken Sie die **Registerkarte** Taste zweimal, um den Standard-Volumeschattenkopie-Codeausschnitt innerhalb der Klassendefinition einfügen. Legen Sie die Schattenwerte auf **10px 10px 5px #888**. Geben Sie dann **Border-Radius** , und fügen Sie den Codeausschnitt. Typ **15px** festzulegende Radiusgröße, und drücken Sie **EINGABETASTE**.

    ![Abgerundete Ecken mit Schatten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "abgerundete Ecken mit Schatten")

    *Abgerundeten Ecken mit Schatten*

    > [!NOTE]
    > Zu diesem Zeitpunkt wird der Volumeschattenkopie-Attribut mit dem entsprechenden Präfix (Moz, Webkit, o) zur Unterstützung von Mozilla und Browser Webkit (Chrom, Safari, Konkeror) eingefügt.
7. Erstellen Sie eine neue Klasse **div.images Ul li Img:hover** unterhalb der **div.images Ul li Img** Klassendefinition, und platzieren Sie den Cursor innerhalb der Klammern **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **transformieren** , und drücken Sie die **Registerkarte** Taste zweimal, um die Transformation-Ausschnitt eingefügt wird. Geben Sie dann die **rotate(-15deg)** den Drehwinkel ändern, wenn Bilder gezeigt werden.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Drücken Sie **F5** führen Sie die Projektmappe, und navigieren Sie zu der Seite "CSS3". Beachten Sie, dass die Bilder abgerundete Ecken haben und Schatten Feld. Zeigen Sie mit der Maus über die Bilder, und beobachten sie drehen.

    ![Drehen eines Bilds Ausschnitt transformieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformation Ausschnitt Drehen eines Bildes")

    *Transformieren der Ausschnitt Drehen eines Bildes*

    > [!NOTE]
    > Wenn Sie Internet Explorer 10 verwenden und nicht die Schatten sehen, stellen Sie sicher, dass der Dokumentmodus IE10-Standards festgelegt ist. Drücken Sie **F12** , öffnen Sie Internet Explorer-Entwicklertools, und klicken Sie auf **Dokumentmodus** IE10-Standards zu ändern.

    ![Informationen zu – de](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Übung 2: Was ist neu im HTML-Editor

Visual Studio verfügt über eine verbesserte HTML-Editor. Zu den Verbesserungen in dieser Version enthalten sind intelligenten Einzug in HTML-Dokumente, HTML5-Ausschnitte, HTML-Start und Ende Tag übereinstimmen und HTML-Validierung. Während dieser Übung sehen Sie, wie diese Änderungen bei der Arbeit in das Markup für die Website, die flüssige verbessern.

Der HTML-Editor wurde auch verbessert, wie CSS-Editor. Die meisten dieser Verbesserungen sind kleine Server, auf denen die Web Developer Leben erleichtern. Elemente wie weitere Codeausschnitte für HTML5, intelligenten Einzug übereinstimmende Start- und Endtags, beim Bearbeiten und Validierung, HTML-Dokument DOCTYPE Zielgruppenadressierung einige diese Verbesserungen sind.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Aufgabe 1: verbesserte DOCTYPE-Überprüfung

Der HTML-Editor hat jetzt die Möglichkeit zum Überprüfen der Rand der Seite "DOCTYPE", obwohl die Definition der Masterseite sein kann. Abhängig von der "DOCTYPE" Rand der Seite HTML-Editor mit der richtige Satz von Regeln überprüft und filtert die IntelliSense-Liste, die Berücksichtigung der DOCTYPE-Elemente.

In dieser Aufgabe ändern Sie die DOCTYPE einer Seite, um festzustellen, wie sich das Verhalten des HTML-Editor entsprechend ändert.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen der **Site.Master** Seite.
3. Beachten Sie das Zielschema für Validierung Symbolleiste aus. Die Möglichkeit, die HTML-Editor verhält sich (Validierung, IntelliSense, usw.) ändert sich ordnungsgemäß entsprechend die "Doctype" ausgewählt.

    ![Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "verwenden "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste")

    *Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden*
4. Ändern Sie das Zielschema in HTML 4.01.

    ![Ändern der Symbolleiste des HTML-Quellcodebearbeitung Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "ändern Doctype HTML-Quellcodebearbeitung-Symbolleiste")

    *Doctype HTML-Quellcodebearbeitung Symbolleiste ändern*
5. Platzieren Sie den Cursor unter den **Text** -Element, und beginnen Sie mit den Namen eines HTML5-Elements (z. B. **video**). Beachten Sie, dass das Element nicht in der IntelliSense-Liste verfügbar ist.

    ![HTML5-Elemente, die nicht aufgelisteten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5-Elemente, die nicht aufgeführt.")

    *HTML5-Elemente, die nicht aufgeführt.*
6. Die Änderungen in das Zielschema für Validierung Symbolleiste DOCTYPE Entnahme rückgängig machen: XHTML5 aus der Dropdownliste aus.

    ![Doctype HTML-Quellcodebearbeitung Symbolleiste verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "verwenden "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste")

    *Zurücksetzen der "Doctype" in HTML-Quellcodebearbeitung-Symbolleiste*
7. Platzieren Sie den Cursor unter den **Text** Element- und anfangen zu tippen erneut eine HTML5-Element (z. B. wie **video**). Beachten Sie, dass jetzt die HTML5-Elemente in der IntelliSense-Liste verfügbar sind.

    ![HTML5-Elemente aufgelistet wird](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5-Elemente, die aufgelistet werden")

    *HTML5-Elemente, die aufgelistet werden*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Aufgabe 2: Start/End-Tags, automatische Updates

Visual Studio aktualisiert den HTML-Code beim Öffnen oder Endtags des Elements, das Sie bearbeiten, um als übereinstimmend. Dieses neue Feature verbessert die Produktivität, beim Bearbeiten von HTML-Tags.

1. Auf der **"default.aspx"** Seite, fügen Sie ein **H3** Element mit einem Titel (z. B. Visual Studio 2012 Rocks!).


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Ändern der **H3** Tag, und geben **H2** oder **H1.**

    Beachten Sie, dass das Endtag wird automatisch aktualisiert. Sie können auch ändern, dass das Endtag, um festzustellen, ob das Starttag zu entsprechend aktualisiert.

    ![Automatische Aktualisierung des Endtags](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "automatische Aktualisierung des Endtags")

    *Automatische Aktualisierung des Endtags*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Aufgabe 3 – neue HTML5-Codeausschnitte

Visual Studio enthält nun einige HTML5-Codeausschnitte. In dieser Aufgabe verwenden Sie einige der diese Codeausschnitte.

1. Fügen Sie einen neuen Ordner namens **audio** auf das Stammverzeichnis der Website-Ordner. Öffnen Sie Windows Explorer, und kopieren Sie alle Audiodatei in der **audio** Ordner, der die **WhatsNewASPNET.sln** Lösung.
2. In der **"default.aspx"** Seite, und suchen Sie den Cursor unter der Web 11 Rocks!! Header. Typ **audio** , und drücken Sie die TAB-Taste.

    Neue HTML-Editor enthält Codeausschnitte für HTML5-Inhalt. Denken Sie daran, um die richtige DOCTYPE-Definition verwenden, um die HTML5-Ausschnitte zu aktivieren.

    ![Einfügen von HTML5-Codeausschnitte](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Einfügen von HTML5-Codeausschnitte")

    *Einfügen von HTML5-Codeausschnitte*
3. Aktualisieren Sie die audio Quelle auf eine vorhandene Audiodatei verweisen.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Sie müssen die Audiodatei zur Projektmappe hinzuzufügen.
4. Drücken Sie **F5** zum Ausführen von der Website und das Audio wiedergegeben.

    ![Ausführen der Audiosteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "mit audio-Steuerelement")

    *Ausführen von audio-Steuerelement*

    > [!NOTE]
    > Sie können auch versuchen, weitere Ausschnitte, die in Visual Studio, z. B. Video, Abbildung usw. enthalten.
5. Nun versuchen Sie, ein Steuerelement in einem Teil der Seite eingefügt. Z. B. versuchen, fügen Sie eine **GridView** -Steuerelement, aber statt einzugeben  **&lt;Gri,** mit der Eingabe beginnen  **&lt;/GV**. Beachten Sie, die die IntelliSense-Liste zeigt die **Asp: GridView** Steuerelement.

    IntelliSense im HTML-Editor bietet jetzt titelschreibweise Suche als auch teilweise übereinstimmenden (Abrufen aller Elemente, die den Begriff enthält).

    ![Einfügen einer GridView mit IntelliSense-Listen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Einfügen einer GridView mit IntelliSense-Listen")

    *Einfügen einer GridView mit IntelliSense-Listen*

    Wenn Sie eingeben  **&lt;Raster** erhalten Sie alle Elemente, die den Begriff überein, aber Visual Studio schlägt die **Gridview** Steuerelement:

    ![Einfügen einer GridView mit IntelliSense-Listen und teilweise übereinstimmenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "eine GridView mit IntelliSense-Listen und teilweise übereinstimmenden einfügen")

    *Einfügen einer GridView mit partiellen Übereinstimmung und IntelliSense-Listen*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Aufgabe 4: HTML-Editor Smart Tags

Eine weitere Verbesserung im HTML-Editor ist die Smarttags-Funktion. Smarttags erleichtern die allgemeine oder sich wiederholende Entwicklungsaufgaben regelmäßig pro Steuerelement ausführen. Diese Funktion wurde bereits verfügbar im HTML-Designer, jedoch nicht in der HTML-Editor.

1. Open **Site.Master** und suchen Sie die **Asp-Menü:** Element. Platzieren Sie den Cursor auf dem Starttag und beachten Sie, dass das kleine Symbol angezeigt, die am unteren Rand der Element - intelligenten Aufgabenmenü öffnen sie das klicken. Beachten Sie, dass Sie schnellen Zugriff auf bestimmte Aufgaben im Zusammenhang mit der das Menüsteuerelement verfügen.

    ![Aufgaben für den Menu-Steuerelement für intelligente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Aufgaben für den Menu-Steuerelement für intelligente")

    *Intelligente Aufgaben für den Menu-Steuerelement*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Aufgabe 5: intelligenten Einzug

Eine der empfohlenen Vorgehensweisen im HTML ist die geschachtelten Elemente, um die Lesbarkeit des Codes den Einzug. In Visual Studio 2012 werden Sie feststellen, dass der Editor automatisch die Elemente zieht, während Sie den Code schreiben.

> [!NOTE]
> In der vorherigen Version von Visual Studio intelligenten Einzug verfügbar wurde in der XML-Editor jedoch nicht in der HTML-Editor.


1. Stellen Sie sicher, dass die Konfiguration der Einzug der HTML-Editor intelligenten Einzug festgelegt ist. Wählen Sie hierzu die **Tools | Optionen** Menüoption, und wählen Sie dann die **Text-Editor | HTML | Registerkarten** Seite im linken Bereich des Bildschirms. Wählen Sie die Option intelligenten Einzug.

    ![HTML-Editor-Einstellungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML-Editor-Einstellungen")

    *HTML-Editor-Einstellungen*
2. Auf der **"default.aspx"** Seite, entfernen Sie alle Inhalte unter den audio-Element.
3. Platzieren Sie den Cursor am Ende das öffnende **audio** -Element, und drücken **EINGABETASTE**.

    Beachten Sie, dass die neue Position des Cursors eine zusätzliche Einzugsebene verfügt.

    ![Einzug in der HTML-Editor für intelligente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "intelligente Einzug in der HTML-Editor")

    *Intelligenten Einzug in der HTML-Editor*
4. Wiederherstellen mit dem Inhalt, die Sie entfernt haben, oder schließen Sie das audio Tag **"default.aspx"** ohne die Änderungen zu speichern.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Aufgabe 6: "in Benutzersteuerelement extrahieren"

Die Umgestaltung-Tools in Visual Studio, z. B. Extrahieren eines Teils des Codes einer Funktion enthalten sind zahlreichen Funktionen, die die Verbesserung und Umgestaltung den vorhandenen Code zu vereinfachen. Die Entsprechung für die ASP.NET-Seiten wäre das Extrahieren von HTML-Code in einem Benutzersteuerelement. Dies manuell würden, z. B. Erstellen eines neuen Benutzersteuerelements Codeabschnitt verschieben, auf das Benutzersteuerelement, registrieren ein Tagpräfix für das Benutzersteuerelement und, instanziieren schließlich das Benutzersteuerelement auf den Seiten, mehrere Schritte umfassen. Jetzt das neue *in Benutzersteuerelement extrahieren* Tool alle diese Schritte automatisch für Sie ausführt.

In dieser Aufgabe verwenden Sie neue in kontextabhängige Vorgang Benutzersteuerelement extrahieren, um ein neues benutzerdefiniertes Steuerelement aus den ausgewählten Code zu generieren.

1. Auf der **"default.aspx"** Seite der **H2** und **audio** Elemente.
2. Klicken Sie mit der rechten Maustaste und wählen **in Benutzersteuerelement extrahieren**.

    ![Menüoption Benutzersteuerelement extrahieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Menüoption Benutzersteuerelement extrahieren")

    *Menüoption Benutzersteuerelement extrahieren*
3. Geben Sie einen Namen für das neue Benutzersteuerelement. Z. B. **Jukebox.ascx**, und klicken Sie dann auf **OK**.

    ![Speichern die extrahierten Benutzersteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "das extrahierte Benutzersteuerelement speichern")

    *Speichern die extrahierten Benutzersteuerelement*
4. Beachten Sie, dass der ausgewählte Code in einem Benutzersteuerelement extrahiert wurden und am ursprüngliche Speicherort des ausgewählten Codes mit einer Instanz des neuen Benutzersteuerelements ersetzt wurde.

    ![Seite automatisch aktualisiert, um das neue Benutzersteuerelement verwenden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Seite automatisch aktualisiert, um das neue Benutzersteuerelement verwenden")

    *Seite aktualisiert automatisch, um das neue Benutzersteuerelement verwenden*
5. Drücken Sie **F5** führen Sie die Seite, und stellen Sie sicher, dass das Steuerelement funktioniert.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Übung 3: Was ist neu in der JavaScript-Editor

Schreiben oder Bearbeiten von JavaScript-Code ist eine einfache Aufgabe nicht, insbesondere, wenn die Anwendung wird gestartet, an Größe zunehmen, und Sie Umgang mit langer Dateien und Hunderte von Funktionen. Skriptentwickler müssen in der Regel keine werden zusätzliche Aufgaben ausführen, um die Lesbarkeit von Code und in Dateien zu navigieren. Unter Einbeziehung von JavaScript-Bibliotheken wie jQuery geworden Skript Navigation ist eine Herausforderung aufgrund der Codelänge.

Visual Studio hat den JavaScript-Editor mit die Zusage, die den Modus Code organisiert und zugänglichen Datenbanken stellen erneuert. Viele Visual Studio-Funktionen, die bereits in c# oder VB-Editoren vorhanden waren, werden jetzt in der JavaScript-Editor implementiert: Gehe zu Definition, automatischer Einzug, Dokumentation und Validierung, wenn Sie schreiben. Mit der erneuerte IntelliSense-Liste müssen Sie den JavaScript-Funktion Katalog jederzeit griffbereit.

In dieser Übung erfahren Sie einige der neuen Features und Verbesserungen der JavaScript-Editor. Sie Beispieldateien durchsuchen und ermitteln jeweils die neuen Eigenschaften, die JavaScript-Programmierung in Visual Studio 2012 effizienter machen.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Aufgabe 1: neue Funktionen für JavaScript-Editor

Diese Aufgabe werden einige der neuen JavaScript-Editor-Funktionen vorgestellt dem darauf konzentrieren, Organisieren den Code und eine bessere benutzererfahrung zu schalten.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Drücken Sie **F5** um die Anwendung auszuführen, klicken Sie dann auf den Link "JavaScript" in der Navigationsleiste. Aktualisieren Sie die Seite mehrere Male und Überprüfung wie der Indikator erhöht.

    ![Seitenzähler](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Seitenzähler")

    *Seitenzähler*
3. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
4. Öffnen der **JavaScript.aspx** Seite, und suchen Sie die  **&lt;Skript&gt;**  Block (siehe unten).

    Der folgende Code verwendet HTML5 lokalen Speicher zum Speichern einer *PageLoadCount* Variable an, wie oft speichert die Seite vom aktuellen Benutzer besucht wurde wurde. Lokaler Speicher ist eine clientseitige Schlüssel-Wert-Datenbank, die mit dem HTML5-Standard eingeführt. Die Daten werden auf dem lokalen Computer, in den Browser des Benutzers gespeichert.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Stellen Sie sicher, dass die "DOCTYPE" auf XHTML5 festgelegt ist, bevor Sie mit den nächsten Schritten fortfahren.
5. Bearbeiten Sie den Code, und beachten Sie, dass IntelliSense für JavaScript HTML5-Funktionen wie die lokalen Speicher und ihre internen Methoden enthält.

    ![HTML5-JavaScript-Funktionen in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5-JavaScript-Funktionen in JavaScript")

    *HTML5-JavaScript-Funktionen in JavaScript*
6. Klicken Sie auf eine öffnende Klammer (**{**) aus, das die Skriptoptionen code, und beachten Sie, dass die Klammern hervorgehoben werden.

    ![Klammern hervorgehoben werden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Klammern hervorgehoben werden")

    *Es werden Klammern hervorgehoben.*
7. Kommentieren Sie die Funktion **testAutoAlign()** (Wählen Sie die drei Zeilen, und Sie können **STRG** + **K**; **STRG** + **U**), und suchen Sie den Cursor innerhalb der Funktionscode. Drücken Sie EINGABETASTE, um eine zweite Zeile angefügt werden soll. Beachten Sie, dass der Code jetzt **ausgerichtet** und **automatisch eingezogen**.

    ![JavaScript-Code wird automatisch ausgerichtet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "ist "Auto ausgerichtet" JavaScript-Code")

    *JavaScript-Code wird automatisch ausgerichtet*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Aufgabe 2: Überprüfen von JavaScript

In dieser Aufgabe werden neue JavaScript-Validierung für den Standard ECMAScript5 ermittelt werden. Diese Funktion hilft Ihnen, kompatibel JavaScript-Code, bei Problemen mit verhindern, dass Skripts vor der Bereitstellung der Website zu schreiben.

> [!NOTE]
> Visual Studio 2010 implementiert ECMAStript3-Kompatibilität, während Visual Studio 2012 ECMAScript5-Kompatibilität bereitstellt.


1. Open **ECMA5script5.js** unterhalb der **Scripts\custom** Projektordner. Testen Sie jetzt Validierung für ECMAScript5-standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Sehen Sie sich die &quot; **verwenden strict** &quot; Richtung in der ersten Zeile der Datei, wodurch ECMAScript5 **strict-Modus**. Dieser Modus besteht aus einer Teilmenge der Programmiersprache, die Mehrdeutigkeiten aus der früheren Edition verdeutlicht und fügt einige neuen Funktionen, z. B. Getter und Setter, Library-Unterstützung für JSON und vollständigere Reflektion auf die Objekteigenschaften aus.
2. Öffnen der **Fehlerliste** Wenn nicht bereits geöffnet (**Ansicht** Menü | **Fehlerliste**). Beachten Sie, dass die **Funktion** Deklaration unterstrichen ist. Dies liegt daran in ECMA5 Standardfunktionen in Language-Strukturen geschachtelt werden nicht möglich. Der Fehler wird die Liste unterhalb der Warndetails angezeigt.

    ![JavaScript-Überprüfungsfehlermeldung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Überprüfungsfehlermeldung JavaScript")

    *JavaScript-Überprüfungsfehlermeldung*
3. Kommentieren Sie Sie aus der  **&quot;verwenden strenge&quot;**  Richtung und beachten Sie, die Fehler nicht mehr angezeigt, aber die Warnungen bleiben.
4. Schreiben Sie eine beliebige Zeichenfolge wie in der letzten Zeile der Datei  **&quot;testen&quot;**  (einschließlich der Anführungszeichen kennzeichnen, ist als Zeichenfolge). Schreiben Sie einen Punkt neben die Zeichenfolge, die IntelliSense-Liste angezeigt, und wählen Sie die **trim** Option.

    ECMAScript5-Standard verfügen Zeichenfolgenwerte und Variablen, String-Methoden, die definiert, wie das trim, Großbuchstaben, suchen und ersetzen.

    ![IntelliSense-Liste in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "in JavaScript IntelliSense-Liste")

    *IntelliSense-Liste in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Aufgabe 3 - XML-Dokumentation für JavaScript

In dieser Aufgabe untersuchen Sie die Visual Studio-Funktionen für die XML-Dokumentation in JavaScript. Sie sehen, dass die JavaScript-IntelliSense-Liste zeigt nun für jede Funktion die XML-Dokumentation. Darüber hinaus werden Sie das Navigationsfeature "in JavaScript feststellen.

1. Open **XMLDoc.js** -Datei im **Skripts/Custom** Projektordner. Diese Datei enthält XML-Dokumentation auf jedem der JavaScript-Funktionen.

    ![JavaScript XML-Dokumentation integriert, um IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "JavaScript XML-Dokumentation integriert, um IntelliSense")

    *JavaScript XML-Dokumentation, die für IntelliSense integriert*
2. Im folgenden **hinzufügen** -Funktion in **XMLDoc.js** Datei, erstellen Sie eine neue Funktion mit dem Namen **testen**.
3. In der **testen** funktionieren, rufen Sie die **Multiplizieren** -Funktion, die zwei Parameter empfängt. Beachten Sie das Kontrollkästchen für die QuickInfo wird angezeigt. die **Multiplizieren** Dokumentation-Funktion.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![XML-Dokumentation für JavaScript-Funktionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-Dokumentation für JavaScript-Funktionen")

    *XML-Dokumentation für JavaScript-Funktionen*
4. Führen Sie die Funktion Call-Anweisung und den Typ einer *Punkt* zum Öffnen der IntelliSense-Liste auf den zurückgegebenen Wert. Beachten Sie die Visual Studio erkennt die **zurückgeben** Wert in der Dokumentation, die den Wert als Zahl behandelt.

    ![XML-Dokumentation für die Rückgabetypen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-Dokumentation für die Rückgabetypen")

    *XML-Dokumentation für die Rückgabetypen*
5. Fügen Sie nun, einen Aufruf von Funktion hinzuzufügen. Beachten Sie, dass der JavaScript-Editor jetzt funktionsüberladungen unterstützt. Wenn Sie einen Funktionsnamen schreiben, werden Sie keines der verfügbaren Überladungen, die in der Dokumentation angegebenen auswählen können.

    ![XML-Dokumentation für Überladungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-Dokumentation für Überladungen")

    *XML-Dokumentation für Überladungen*
6. Open **GotoDefinition.js** Datei, und suchen Sie die **$().html()** Funktionsaufruf. Suchen Sie den Cursor auf **html**.
7. Drücken Sie **F12** , und navigieren Sie zur Definition. Beachten Sie, Sie können jetzt zugreifen und Durchsuchen von JavaScript-Code ohne Verwendung der **suchen** Tool.
8. Suchen Sie den Cursor auf die jQuery-Anweisung vor den Signaturblock am unteren Rand der Codedatei ein. Drücken Sie **F12**. Sie gelangen auf die jQuery-Bibliotheksdatei. Beachten Sie, Sie können auch über die jQuery-Dateien, die über navigieren **F12**.

    ![Navigieren zu jQuery-Definitionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "navigieren jQuery-Definitionen")

    *Navigieren zu jQuery-Definitionen*

> [!NOTE]
> Stellen Sie sicher, dass GotoDefinition.js keine Syntaxfehler aufweist, bevor Sie die Datei speichern.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Übung 4: Bündelung und Minimierung

Wie oft schließen Ihre Websites mehr als ein JavaScript- oder CSS-Datei? Dies ist ein sehr häufiges Szenario, bei denen Bündelung und Minimierung beitragen können, reduzieren Sie die Dateigröße, und stellen die Website, die schneller ausgeführt. Der neuen bundling Funktion in ASP.NET 4.5 packs einen Satz von JS oder CSS-Dateien in einem einzelnen Element, und reduziert die Größe von weboptimierungsframework den Inhalt (d. h. Entfernen von Leerzeichen nicht erforderlich, Entfernen von Kommentaren, Bezeichnern verringert).

Bundling und Minimierung in ASP.NET 4.5 erfolgt zur Laufzeit, damit der Prozess identifizieren den Benutzer-Agent (z. B. Internet Explorer, Mozilla usw.), und kann daher die Komprimierung verbessern, indem Sie den Benutzer Browser (z. B. entfernt haben Zugriff auf Inhalte, die bestimmte Mozilla ist Wenn die Anforderung IE stammen).

In dieser Übung erfahren Sie, wie zum Aktivieren und verwenden die verschiedenen Typen von Bündelung und Minimierung in ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Aufgabe 1: Installieren der Bündelung und Minimierung-Pakets von NuGet

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen Sie das NuGet-Paket-Manager-Konsole. Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Weitere Fenster** | **Package Manager Console**.

    ![Öffnen die Paket-Manager-file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "die Paket-Manager-Konsole öffnen")

    *Öffnen die Paket-Manager-Konsole*
3. In der **Paket-Manager-Konsole** Typ **Install-Package Microsoft.Web.Optimization** , und drücken Sie **EINGABETASTE**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Aufgabe 2: Standard-Pakete

Die einfachste Methode zum Verwenden von Bündelung und Minimierung ist die Standard-Pakete zu aktivieren. Diese Methode verwendet Konventionen können Sie die gebündelte und verkleinerte Version für die JS und CSS-Dateien in einem Ordner zu verweisen.

In dieser Aufgabe lernen Sie, das Aktivieren und die gebündelten und verkleinerte JS und CSS-Dateien zu verweisen und die resultierende Ausgabe anzeigen.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Projektmappe befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. In der **Projektmappen-Explorer**, erweitern Sie die **Stile**, **Scripts\custom** und **Scripts\bundle** Ordner.

    Beachten Sie, dass die Anwendung mehrere CSS- und JS-Datei verwendet wird.

    ![Mehrere Stylesheets und JavaScript-Dateien in der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "mehrere Stylesheets und JavaScript-Dateien in der Anwendung")

    *Mehrere Stylesheets und JavaScript-Dateien in der Anwendung*
3. Öffnen der **Global.asax.cs** Datei.

    Beachten Sie, dass die neue **Microsoft.Web.Optimization** Namespace am Anfang der Datei auskommentiert ist. Kommentieren Sie die mit Direktive, um die Bündelung und Minimierung Funktionen umfassen.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Suchen Sie die **Anwendung\_starten** Methode.

    Kommentieren Sie in dieser Methode den EnableDefaultBundles-Aufruf wie im folgenden Codeausschnitt gezeigt. Dies ermöglicht es, zu der eine gebündelte Auflistung von CSS-Dateien in einem Ordner zu verweisen, indem Sie den Pfad für diesen Ordner sowie die &quot;CSS&quot; oder &quot;JS&quot; Suffix.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Öffnen der **Optimization.aspx** Datei, und suchen Sie das Inhaltssteuerelement für **HeadContent**.

    Beachten Sie die CSS-Dateien und die JS-Dateien, die ein einzelnes referenziertes Tag aufweisen.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Dieser Code wird zu Demonstrationszwecken. Im Idealfall werden Sie die Pakete in die Datei Site.Master verweisen. In diesem Beispielcode werden Sie feststellen, dass gebündelten Dateien auch durch die Datei Site.Master verwiesen werden Bezugnahme dieser letzten redundant.
6. Beachten Sie, dass die Links in die bundling Konventionen verwenden die **Href** Attribut, um alle CSS oder JS-Dateien entnommen werden die Formate und Scripts\custom Ordner bzw.

    Können Sie den Pfad **Skripts/Custom/JS** wie unten dargestellt, bündeln und verkleinernde die JS-Dateien in einem **Skripts/benutzerdefinierte** Ordner. Dies ist das Standardverhalten, mit der Standard-Pakete.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Öffnen der **Styles\Site.css** Datei.

    Beachten Sie, dass die ursprüngliche CSS-Datei enthält Code eingezogen, Leerzeichen und Kommentare, die die Datei zu vergrößern. (Enthält auch die JavaScript-Datei Leerzeichen und Kommentare).

    ![Einer der ursprünglichen CSS-Dateien im Ordner "Skripts"](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "eine der ursprünglichen CSS-Dateien im Ordner "Skripts"")

    *Einer der ursprünglichen CSS-Dateien im Ordner "Skripts"*
8. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren zu der **Optimierung** Seite.
9. Klicken Sie auf die **CSS-Bundle** Link zum Herunterladen, und öffnen Sie die Datei.

    Die verkleinerte gebündelte Datei auschecken. Beachten Sie, dass alle Leerräume, Kommentare und Einzug Zeichen entfernt wurden, generieren eine kleinere Datei.

    ![CSS-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "gebündelt CSS-Dateien")

    *Gebündelte CSS-Dateien*
10. Klicken Sie nun auf die **JS-Bundle** Link zum Öffnen der Datei JavaScript gebündelt. Sie können den Explorer Warnung ignorieren. Beachten Sie die JavaScript-Dateien unter den **benutzerdefinierte** Ordner auch gebündelt und verkleinert.

    ![JavaScript-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "gebündelt JavaScript-Dateien")

    *Gebündelte JavaScript-Dateien*

    Die Aktivierung der Komprimierung für CSS oder JS-Dateien wurde in früheren ASP.NET-Version wesentlich komplexer. Nun, wie Sie gesehen haben, nur müssen Sie eine Zeile im Hinzufügen der *"Global.asax"* Datei ein, aktivieren die Bündelung und verweisen dann gebündelten Dateien von Ihrem Standort.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Aufgabe 3: statische Pakete

Der statische Bundle Ansatz ermöglicht Ihnen die Anpassung des Satz von Dateien zum Paket, den Verweis und die Minimierung-Methode, die verwendet werden.

In dieser Aufgabe Konfigurieren Sie eine statische Paket um einen bestimmten Satz von Dateien zu bündeln und verkleinernde zu definieren.

1. Schließen Sie den Browser.
2. Öffnen der **Global.asax.cs** Datei, und suchen Sie die **Anwendung\_starten** Methode.
3. Kommentieren Sie den statischen Bundle-Code wie im folgenden Code gezeigt.

    Definieren Sie eine statische Paket, das mit verwiesen wird die &quot; **~/StaticBundle** &quot; virtuellen Pfad und Verwendung **JsMinify** für Minimierung von alle angegebenen Dateien mit der **AddFile** Methode. Schließlich Sie statische Paket zum Hinzufügen der **BundleTable** und aktivieren es.

    Beachten Sie, dass die Dateien nicht am gleichen Ort befinden. Dies ist ein weiterer Vorteil über die standardmäßige bündeln.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Öffnen der **Optimization.aspx** Datei.

    Beachten Sie, dass der Link zum **statische JS-Bundle** wird unter Verwendung des Pfads, die bei der Konfiguration der statischen Pakets in der Datei Global.asax.cs deklariert haben: **/StaticBundle**.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Drücken Sie **F5** führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.
6. Klicken Sie auf die **statische JS-Bundle** Link zum Öffnen der Datei.

    Beachten Sie, dass der verkleinerte JavaScript-Datei gebündelt ist die Ausgabe für alle JavaScript-Dateien, die in der statischen Bundle-Datei unter dem Pfad konfiguriert &quot;/StaticBundle&quot;.

    ![Statische JavaScript-Dateien Bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statische JavaScript-Dateien-Paket")

    *Bündeln von statische JavaScript-Dateien*
7. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Aufgabe 4: dynamische Ordner Pakete

In dieser Aufgabe lernen Sie das dynamische Ordner Pakete zu konfigurieren. Die Leistungsfähigkeit von dynamischen bundling besteht, können Sie statische JavaScript als auch andere Dateien enthalten, in Sprachen, die in JavaScript kompiliert wird und daher, erfordern einige verarbeiten, bevor die Bündelung ausgeführt wird.

In diesem Beispiel erfahren Sie, wie Sie die **DynamicFolderBundle** Klasse, um ein dynamisches Paket für CofeeScript-Dateien zu erstellen. CofeeScript ist eine Programmiersprache, die in JavaScript kompiliert und eine einfachere Syntax für das Schreiben von JavaScript-Code, JavaScript Übersichtlichkeit und Lesbarkeit verbessern.

1. Öffnen der **Global.asax.cs** Datei, und suchen Sie die **Anwendung\_starten** Methode.
2. Kommentieren Sie den dynamischen Bundle-Code wie im folgenden Code gezeigt.

    Definieren Sie eine dynamische ordnerpaket, die verwendet werden die **CoffeeMinify** benutzerdefinierte Minimierung-Prozessor, die nur für Dateien mit den gelten die &quot; **.coffee** &quot; (Erweiterung CoffeeScript-Dateien). Beachten Sie, dass Sie die einem Suchmuster, zum Auswählen der Dateien verwenden können, wie z. B. innerhalb eines Ordners zu bündeln "\*.coffee".


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Öffnen Sie das NuGet-Paket-Manager-Konsole. Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Weitere Fenster** | **Package Manager Console**.
4. In der **Paket-Manager-Konsole** Typ **Install-Package CoffeeSharp** , und drücken Sie **EINGABETASTE**.
5. Klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** Fenster

    ![Anzeigen aller Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Anzeigen aller Dateien")

    *Anzeigen aller Dateien*
6. Klicken Sie mit der rechten Maustaste auf die **CoffeeMinify.cs** in der Datei die **Projektmappen-Explorer** , und wählen Sie **zu Projekt hinzufügen**

    ![Schließen Sie die CoffeeMinify.cs-Datei im Projekt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "CoffeeMinify.cs-Datei in das Projekt einfügen.")

    *Die CoffeeMinify.cs-Datei in das Projekt einfügen.*
7. Öffnen der **CoffeeMinify.cs** Datei.

    Diese Klasse erbt von JsMinify zu verkleinernde der JavaScript-Ausgabe von CoffeeScript-Code-Kompilierung. Ruft den CoffeeScript-Compiler um zunächst den JavaScript-Code zu generieren, und er sendet diese dann an die JsMinify.Process-Methode, die den resultierenden Code verkleinernde.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Öffnen der **Script1.coffee** und **Script2.coffee** Dateien aus dem **Skripts-Bundle** Ordner.

    Diese Dateien werden CoffeScript Code kompiliert werden, beim Ausführen der im Lieferumfang der CoffeeMinify-Klasse enthalten.

    Aus Gründen der Einfachheit halber werden die bereitgestellten CoffeeScript-Dateien nur CoffeeScript-Beispielcode einschließlich. Die Kommentare werden durch den Prozess JsMinify ausgeschlossen.

    ![CoffeeScript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript-Dateien")

    *CoffeeScript-Dateien*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) bietet eine einfachere Syntax zum Schreiben von JavaScript-Code, JavaScript Übersichtlichkeit und Lesbarkeit verbessern, sowie andere Funktionen wie Mustervergleiche und Array Kennzeichnung hinzufügen.
9. Öffnen der **Optimization.aspx** Datei, und suchen Sie die Paket-Links.

    Beachten Sie, dass der Link zum **dynamische JS-Bundle** verweist auf die **Skripts-Bundle** Ordner mithilfe der **/Kaffee** Suffix, die Sie für das dynamische ordnerpaket konfiguriert.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Drücken Sie **F5** führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.
11. Klicken Sie auf die **dynamische JS-Bundle** klicken, um die generierte Datei zu öffnen.

    Beachten Sie, die der Inhalt, der in diesem Paket enthalten war, nur enthält **.coffee** Dateien. Sie können auch sehen, dass der CoffeeScript-Code in JavaScript kompiliert wurde und die auskommentierten Zeilen entfernt wurde.

    ![Dynamische JS-Dateien zu bündeln](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamische JS-Dateien-Paket")

    *Bündeln von dynamische JS-Dateien*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Diese Übungseinheit hilft Ihnen zu verstehen, was neu in ASP.NET und Webentwicklung in Visual Studio 2012 ist und wie die Vielzahl von Erweiterungen in Visual Studio 2012 nutzen.

Durch diese praktische Übungseinheit haben wie die neuen Features und Verbesserungen in Visual Studio 2012-Editoren für CSS, JavaScript und HTML verwenden gelernt werden. Darüber hinaus haben Sie gelernt, wie Visual Studio 2012 integrierte Bündelung und Minimierung implementiert.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Visual Studio Express für Web-Kachel*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder. Geben Sie einen Namen für die Regel, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Bestätigen von Änderungen*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

    - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
    - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
    - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
    - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

    ![Konfigurieren von Zielverbindungszeichenfolge](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Zielverbindungszeichenfolge konfigurieren")

    *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

    ![Anwendung in Windows Azure veröffentlicht](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Veröffentlichung der Anwendung in Windows Azure")

    *Anwendung, die auf Windows Azure veröffentlicht werden soll*
