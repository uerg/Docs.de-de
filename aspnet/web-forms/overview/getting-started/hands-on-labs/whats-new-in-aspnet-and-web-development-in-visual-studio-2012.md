---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Was ist neu in ASP.NET und Webentwicklung in Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: Die neue Version von Visual Studio führt eine Reihe von Verbesserungen, die die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien mit dem Schwerpunkt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: d037ccb62693a07ab8e2640b2d930ec7093b35da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375884"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Neuerungen in ASP.NET und Webentwicklung in Visual Studio 2012
====================
durch [Web Camps Team](https://twitter.com/webcamps)

> Die neue Version von Visual Studio führt eine Reihe von Verbesserungen, die auf die Leistung und die Leistung verbessern, bei der Arbeit mit Web-Technologien konzentrieren. Visual Studio-Editoren für CSS, JavaScript und HTML-haben vollständig überarbeitet, um viele der Code die am häufigsten geforderte Hilfsmittel, wie IntelliSense und automatischer Einzug enthalten. In Bezug auf die Leistung zu erzielen sind Bündelung und Minimierung jetzt integriert, wie der Ladevorgang der integrierter Funktionen, um ganz einfach die Seite zu reduzieren.
> 
> Visual Studio ermöglicht Ihnen die Arbeit mit den neuesten Technologien für die Website. Browserübergreifende CSS3-Ausschnitte können Sie sicherstellen, dass Ihre Website unabhängig von der Clientplattform funktioniert, und auch die Nutzung der neuen HTML5-Elemente und Funktionen.
> 
> Schreiben und die profilerstellung für JavaScript-Code sollte mit dieser Version von Visual Studio einfacher sein. IntelliSense-Listen, die integrierte XML-Dokumentation und Navigation-Funktionen sind jetzt für JavaScript-Code verfügbar. Sie haben nun den JavaScript-Katalog zu evaluieren. Darüber hinaus können Sie überprüfen ECMAScript5-Konformität mit Ihren Skripts und Syntaxfehler in einem frühen Stadium erkennen.
> 
> Guter Letzt, implementiert diese Version von Visual Studio integrierte Bündelung und Minimierung. Ihr Skriptdateien und Stylesheets gepackt und komprimiert, sodass die Website schneller durchführt.
> 
> Dieses Lab führt Sie durch die Verbesserungen und neue Funktionen, die zuvor beschriebenen durch Anwenden von geringen Änderungen mit einer Beispiel-Webanwendung, die im Quellordner bereitgestellt.
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In diesem praktische Übungen, erfahren Sie, wie Sie:

- Verwenden Sie die neuen Features und Verbesserungen in der CSS-editor
- Verwenden Sie die neuen Features und Verbesserungen in der HTML-editor
- Verwenden Sie die neuen Features und Verbesserungen in der JavaScript-editor
- Konfigurieren und Verwenden von Bündelung und Minimierung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (für den Setupskripts: installiert unter Windows 8 und Windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - oder ein kompatibler HTML5-Browser

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische testumgebung umfasst die folgenden Übungen:

1. [Übung 1: Was ist neu in der CSS-Editor](#Exercise1)
2. [Übung 2: Was ist neu in der HTML-Editor](#Exercise2)
3. [Übung 3: Was ist neu in der JavaScript-Editor](#Exercise3)
4. [Übung 4: Bündelung und Minimierung](#Exercise4)

Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Übung 1: Was ist neu in der CSS-Editor

Webentwickler sollten viele der Probleme im Zusammenhang mit der Bearbeitung von CSS vertraut sein. Einer der größten Probleme von CSS-Stilen ist browserübergreifende Kompatibilität. Es kommt häufig vor, nach dem Anwenden von Stilen auf Ihrer Website, Sie feststellen, dass es anders aussieht, wenn Sie es in einem anderen Browser oder Gerät öffnen. Aus diesem Grund können Sie eine sehr viel Zeit verbringen, an deren Behebung dieser Probleme bei visual um bewusst ist, kann sie Wenn Sie schließlich in einem Browser funktionieren vereinfachen, in den anderen unterteilt ist.

Visual Studio enthält nun die Features, mit deren Hilfe Entwickler den Zugriff, arbeiten und effektiv Organisieren von CSS-Stylesheets. In dieser Übung werden Sie die neuen Features für eine effektive Organisation und Edition sowie die CSS3-Codeausschnitte für browserübergreifende Kompatibilität erfüllen.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Aufgabe 1: neue Features im Editor

In dieser Aufgabe werden Sie die neuen Funktionen von CSS-Editor feststellen. Diesen neuen Editor können Sie Ihre Produktivität zu steigern, durch die Nutzung von den neuen intelligenten Einzug, verbesserte Codekommentare und die verbesserte IntelliSense-Liste.

1. Starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Lösung befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen Sie im Projektmappen-Explorer die **"Site.CSS"** Datei befindet sich unter dem **Stile** Ordner. Stellen Sie sicher, dass die **Text-Editor** Tools sind auf der Symbolleiste angezeigt. Wählen Sie zu diesem Zweck die **Ansicht** | **Symbolleisten** Menüoption, und aktivieren die **Text-Editor** Optionen. Sie sehen, die seit dieser neuen Version der **Kommentar** Schaltfläche (![Kommentar-Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) und die **Auswahlkommentar löschen** Schaltfläche (![auskommentierung Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) sind ebenfalls für CSS-Editor aktiviert.

    ![Aktivieren der Editor und CSS-Tools](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "aktivieren-Editor und CSS-Tools")

    *Aktivieren der Editor und CSS-Tools*
3. Scrollen Sie im Code, und wählen Sie eine beliebige CSS-Klassendefinition. Klicken Sie auf die **Kommentar** (![Kommentar-Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) Schaltfläche, um die ausgewählte Zeilen kommentieren. Klicken Sie auf die **Auswahlkommentar löschen** (![auskommentierung Schaltfläche](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) Schaltfläche, um die Änderungen rückgängig zu machen.
4. Klicken Sie auf die **reduzieren** (![reduzieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) und **erweitern** (![erweitern](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) Schaltflächen befinden sich am linken Rand des Texts. Beachten Sie, dass Sie jetzt die Stile, die Sie nicht verwenden ausblenden können, um eine übersichtlichere Ansicht zu erhalten.

    ![Reduzieren von CSS-Klassen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Reduzieren von CSS-Klassen")

    *Reduzieren von CSS-Klassen*
5. Stellen Sie sicher, dass die Funktion des intelligenten Einzugs aktiviert ist. Wählen Sie die **Tools** | **Optionen** Menüoption, und wählen Sie dann die **Text-Editor** | **CSS**  |  **Formatierung** Seite im linken Bereich des Bildschirms. Überprüfen Sie die **Hierarchischer Einzug** Option.

    ![Hierarchischen Einzug aktivieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "aktivieren hierarchischen Einzug")

    *Hierarchischen Einzug aktivieren*
6. Suchen Sie die Hauptklasse-Definition (.main) aus, und fügen Sie eine Formatvorlage auf die Div-Elemente. Beachten Sie der Code automatisch Benutzern um die übergeordneten Klassen finden Sie auf einen Blick ausgerichtet.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Hierarchische Ausrichtung in CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "hierarchische Ausrichtung in CSS")

    *Hierarchische Ausrichtung in CSS*
7. In **.main Div** Klasse, und suchen Sie den Cursor am Ende der **Rahmen: 0px;** , und drücken Sie **EINGABETASTE** zum Anzeigen der IntelliSense-Liste. Mit der Eingabe beginnen **oben** , und beachten Sie, wie die Liste gefiltert wird, während der Eingabe. Die Liste zeigt die Elemente, enthalten **oben** an einen beliebigen Teil des Worts (In früheren Versionen von Visual Studio, die Liste wird von den Elementen gefiltert, *beginnen* mit dem Begriff).

    ![IntelliSense-Verbesserungen in CSS-](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "IntelliSense-Verbesserungen in CSS")

    *IntelliSense-Verbesserungen in CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Aufgabe 2: die Farbauswahl

In dieser Aufgabe werden Sie feststellen, dass der neue CSS-Farbauswahl in Visual Studio IntelliSense integriert.

1. In **"Site.CSS",** suchen Sie die Header-Klassendefinition (.header), und platzieren Sie den Cursor neben **Hintergrundfarbe** Attribut, zwischen den &quot;:&quot; und &quot; # &quot; Zeichen in dieser Codezeile **.**

    ![Suchen den Cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "suchen den Cursor")

    *Suchen den cursor*
2. Löschen der **Doppelpunkt** (:) und Schreiben Sie ihn erneut aus, um die Farbauswahl angezeigt. Beachten Sie, dass die erste Farben, die angezeigt werden, die am häufigsten Farben Ihrer Website. Wenn Sie die Farbe weiße klicken, wird der zugehörige HTML-Farbcode (#fff) den aktuellen Farbcode im Stylesheet ersetzen.

    ![Farbauswahl](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Farbauswahl")

    *Farbauswahl*
3. Drücken Sie die **erweitern** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) Schaltfläche auf dem Farbwähler auf-Anfangsfarbe des Farbverlaufs anzuzeigen, und ziehen dann den Farbverlauf Cursor um eine andere Farbe auswählen. Klicken Sie anschließend auf die **Formatpipette** Schaltfläche, und wählen Sie eine beliebige Farbe auf dem Bildschirm. Beachten Sie, dass hintergrundfarbwerts dynamisch ändert, während Sie den Cursor bewegen.

    ![Auswahl der Farbverlauf](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Farbverlauf-Auswahl")

    *Farbverlauf-Auswahl*
4. In der **Deckkraft** Schieberegler, verschieben Sie die Auswahl in der Mitte des den Balken, um die Deckkraft reduziert. Beachten Sie, dass die Hintergrundfarbe Wert jetzt die Bereitstellung in RGBA geändert wird.

    ![Farbauswahl Deckkraft](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Farbauswahl Deckkraft")

    *Farbauswahl Deckkraft*

    > [!NOTE]
    > Der RGBA (Rot, Grün, Blau, Alpha) Farbdefinition CSS3 können Sie die Deckkraft Farbwert für ein einzelnes Element zu definieren. Im Gegensatz zu **Deckkraft -** ein ähnliches Attribut der CSS- **-** RGBA-Farben sind auch mit den neuesten Browsern kompatibel.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Aufgabe 3: CSS-kompatiblen Codeausschnitte

In dieser Aufgabe lernen Sie, wie Sie die browserübergreifende kompatibel CSS3-Codeausschnitte verwenden, um einige Funktionen in Ihrer Website implementieren.

1. In der **"Site.CSS"** Datei, suchen Sie nach der **Header** CSS Klassendefinition (.header), und platzieren Sie den Cursor unterhalb der **/ \*Border-Radius\* /** Platzhalter, um einen neuen Ausschnitt hinzufügen. Drücken Sie **EINGABETASTE** zum Anzeigen der IntelliSense-Liste und Typ **Radius** zum Filtern der Liste. Wählen Sie die **Rahmen-Radius** option aus der Liste mit einem Doppelklick, und drücken Sie dann die **Registerkarte** -Taste, um den Ausschnitt einzufügen. Geben Sie dann eine Radiusgröße in Pixel, und drücken Sie **EINGABETASTE**. Geben Sie z. B. **15px**.

    Die CSS3-Attribute hinzugefügt werden, von dem Ausschnitt rendert Abgerundete Rahmen in den meisten HTML5 Compliance Browsern, einschließlich Mozilla und WebKit-basierten Browsern.

    ![Verwenden einen Border-Radius-Ausschnitt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "mit einem Rahmen-Radius-Codeausschnitt")

    *Verwenden einen Border-Radius-Ausschnitt*
2. Gelten die gleichen **Rahmen** Codeausschnitte in die Formatvorlage (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Drücken Sie **F5** um die Projektmappe auszuführen. Beachten Sie, dass jede Seite jetzt Rahmen gerundet wurde.

    ![Abgerundete Ecken](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "abgerundete Ecken")

    *Abgerundete Ecken*
4. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.
5. Öffnen der **Custom.css** Datei befindet sich unter der **Stile** Ordner, und platzieren Sie den Cursor im **div.images Ul li Img** Definition der Klasse.
6. Drücken Sie die EINGABETASTE zum Anzeigen der IntelliSense-Liste Typ **Feld-Schattierung** , und drücken Sie die **Registerkarte** Taste zweimal, um die standardmäßige Shadow-Codeausschnitt innerhalb der Klassendefinition eingefügt werden soll. Legen Sie die Schattenwerte auf **10px 10px 5px #888**. Geben Sie dann **Rahmen-Radius** und den Codeausschnitt einfügen. Typ **15px** festzulegende RADIUS-Größe, und drücken Sie **EINGABETASTE**.

    ![Abgerundete Ecken mit Schatten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "abgerundete Ecken mit Schatten")

    *Abgerundete Ecken mit Schatten*

    > [!NOTE]
    > Zu diesem Zeitpunkt wird der Volumeschattenkopie-Attribut mit dem entsprechenden Präfix (Moz, Webkit, o), zur Unterstützung von Mozilla und (Chrome, Safari, Konkeror)-Webkit-Browsers eingefügt.
7. Erstellen Sie eine neue Klasse **div.images Ul li Img:hover** unterhalb der **div.images Ul li Img** Definition der Klasse, und platzieren Sie den Cursor innerhalb der Klammern **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Typ **transformieren** , und drücken Sie die **Registerkarte** Schlüssel, zweimal, um den Transform-Ausschnitt einzufügen. Geben Sie dann die **rotate(-15deg)** so ändern Sie den Wert des 3D-Drehungswinkels des Wenn Images gezeigt werden.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Drücken Sie **F5** , führen Sie die Projektmappe, und navigieren Sie zu der CSS3-Seite. Beachten Sie, dass die Bilder Ecken abgerundete haben und Schatten Feld. Bewegen Sie den Mauszeiger über die Bilder aus, und beobachten sie drehen.

    ![Drehen eines Bilds Codeausschnitt transformieren](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformation Codeausschnitt Drehen eines Bilds")

    *Drehen eines Bilds Codeausschnitt transformieren*

    > [!NOTE]
    > Wenn Sie Internet Explorer 10 verwenden und nicht der Schatten angezeigt, stellen Sie sicher, dass der Dokumentmodus IE10-Standards festgelegt ist. Drücken Sie **F12** , öffnen Sie Internet Explorer-Entwicklertools, und klicken Sie auf **Dokumentmodus** IE10-Standards zu ändern.

    ![Informationen zu-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Übung 2: Was ist neu in der HTML-Editor

Visual Studio bietet einen verbesserte HTML-Editor. Zu den Verbesserungen in dieser Version enthalten sind Intelligenter Einzug in HTML-Dokumente, HTML5-Ausschnitte, HTML-Start und Ende-übereinstimmende Tags und HTML-Validierung. In dieser Übung sehen Sie, wie diese Änderungen Ihre zu verbessern, bei der Arbeit in das Markup für die Website.

Wie die CSS-Editor wurde der HTML-Editor auch verbessert. Die meisten dieser Verbesserungen sind kleinere Tabellen, die die Web-Entwicklern das Leben erleichtern. Dinge wie weitere Codeausschnitte für HTML5, intelligenten Einzug übereinstimmende Start- und Endtags, wenn Bearbeitung und Überprüfung für das HTML-Dokument DOCTYPE einige dieser Verbesserungen sind.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Aufgabe 1: verbesserte DOCTYPE-Überprüfung

Der HTML-Editor hat jetzt die Möglichkeit, den DOKUMENTTYP der Seite zu überprüfen, obwohl die Definition der Masterseite ist. Je nach den DOKUMENTTYP der Seite der HTML-Editor den richtigen Satz von Regeln überprüft und filtert die IntelliSense-Liste, die in Betracht ziehen die DOCTYPE-Elemente.

In dieser Aufgabe ändern Sie den DOKUMENTTYP einer Seite, um festzustellen, wie die HTML-Editor-Verhalten entsprechend ändert.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Lösung befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen der **Site.Master** Seite.
3. Beachten Sie, dass das Zielschema für Validierung-Symbolleiste. Die Möglichkeit, die HTML-Editor-Verhalten (Validierung, IntelliSense, usw.) ändert sich ordnungsgemäß entsprechend den Dokumenttyp ausgewählt.

    ![Verwenden Sie in der Symbolleiste HTML-Quellcodebearbeitung Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "verwenden \"Doctype\" in der Symbolleiste HTML-Quellcodebearbeitung")

    *Verwenden Sie Doctype HTML-Quellcodebearbeitung Symbolleiste.*
4. Ändern Sie das Zielschema in HTML 4.01 an.

    ![Ändern in der Symbolleiste HTML-Quellcodebearbeitung Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Doctype ändern, in der Symbolleiste HTML-Quellcodebearbeitung")

    *Ändern in der Symbolleiste HTML-Quellcodebearbeitung Doctype*
5. Platzieren Sie den Cursor unter den **Text** Element, und starten Sie den Namen eines HTML5-Elements eingeben (z. B. **video**). Beachten Sie, dass das Element nicht in der IntelliSense-Liste verfügbar ist.

    ![HTML5-Elemente, die nicht aufgelisteten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "HTML5-Elemente, die nicht aufgeführt.")

    *HTML5-Elemente, die nicht aufgeführt.*
6. Verwerfen Sie die Änderungen in das Zielschema für Validierung Symbolleiste DOCTYPE auswählen: XHTML5 aus der Dropdownliste aus.

    ![Verwenden Sie in der Symbolleiste HTML-Quellcodebearbeitung Doctype](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "verwenden \"Doctype\" in der Symbolleiste HTML-Quellcodebearbeitung")

    *Zurücksetzen der "Doctype" in der Symbolleiste HTML-Quellcodebearbeitung*
7. Platzieren Sie den Cursor unter den **Text** -Element, und beginnen Sie erneut mit einem HTML5-Element (z. B. wie **video**). Beachten Sie, dass die HTML5-Elemente jetzt in der IntelliSense-Liste verfügbar sind.

    ![HTML5-Elemente, die verhindern](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "HTML5-Elemente, die verhindern")

    *HTML5-Elemente, die verhindern*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Aufgabe 2: Start/End-Tags, automatische Updates

Visual Studio aktualisiert nun den HTML-Code beim Öffnen oder schließen die Tags des Elements, das Sie bearbeiten, um als übereinstimmend. Dieses neue Feature steigert Ihre Produktivität, beim Bearbeiten von HTML-Tags.

1. Auf der **"default.aspx"** Seite, fügen Sie eine **H3** Element mit einem Titel (z. B. Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Ändern der **H3** Tag, und geben **H2** oder **H1.**

    Beachten Sie, dass das Endtag wird automatisch aktualisiert. Sie können auch ändern, dass das Endtag, um festzustellen, ob das Starttag zu entsprechend aktualisiert.

    ![Automatische Aktualisierung der endkennung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Automatisches Update der endkennung")

    *Automatische Aktualisierung der endkennung*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Aufgabe 3: neue HTML5-Codeausschnitte

Visual Studio enthält jetzt einige HTML5-Codeausschnitte. In dieser Aufgabe verwenden Sie einige der Codeausschnitte.

1. Fügen Sie einen neuen Ordner namens **audio** in das Stammverzeichnis des Ordners Website. Öffnen Sie Windows Explorer, und kopieren Sie alle audio-Datei in die **audio** Ordner die **WhatsNewASPNET.sln** Lösung.
2. In der **"default.aspx"** Seite, und suchen Sie den Cursor unter der Web 11 Rocks! Header. Typ **audio** , und drücken Sie die TAB-Taste.

    Der neue HTML-Editor enthält Codeausschnitte für HTML5-Inhalt. Denken Sie daran, um die richtige DOCTYPE-Definition verwenden, um die HTML5-Ausschnitte zu aktivieren.

    ![Einfügen von Codeausschnitten HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Einfügen von HTML5-Codeausschnitten")

    *Einfügen von HTML5-Codeausschnitten*
3. Aktualisieren Sie die Audioquelle, um auf eine vorhandene Audiodatei zu verweisen.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Sie müssen die Audiodatei zur Projektmappe hinzuzufügen.
4. Drücken Sie **F5** führen Sie die Website und Wiedergeben der Audiodaten.

    ![Audio-Steuerelement ausführen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "mit audio-Steuerelement")

    *Ausführen von audio-Steuerelement*

    > [!NOTE]
    > Sie können auch weitere Codeausschnitte in Visual Studio enthalten, z. B. Videos, Abbildung usw. versuchen.
5. Nun versuchen Sie, ein Steuerelement in einem Teil der Seite ein. Beispielsweise versucht, fügen Sie eine **GridView** -Steuerelement, aber anstatt  **&lt;Raster an,** mit der Eingabe beginnen  **&lt;GV**. Beachten Sie, die die IntelliSense-Liste enthält die **Asp: GridView** Steuerelement.

    IntelliSense im HTML-Editor bietet jetzt titelschreibweise Suche als auch teilweise übereinstimmenden (Abrufen aller Elemente, die den Begriff enthält).

    ![Einfügen einer GridView-Ansicht mit IntelliSense-Listen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Einfügen einer GridView-Ansicht mit IntelliSense-Listen")

    *Einfügen einer GridView-Ansicht mit IntelliSense-Listen*

    Wenn Sie eingeben  **&lt;Raster** erhalten Sie alle Elemente, die den Begriff überein, aber Visual Studio schlägt die **Gridview** Steuerelement:

    ![Einfügen einer GridView-Ansicht mit IntelliSense-Listen und teilübereinstimmungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "einer GridView-Ansicht mit IntelliSense-Listen und teilübereinstimmungen einfügen")

    *Einfügen einer GridView-Ansicht mit IntelliSense-Listen und teilübereinstimmungen*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Aufgabe 4: HTML-Editor-Smart Tags

Eine weitere Verbesserung im HTML-Editor ist das Feature Smarttags. Smarttags erleichtern die Entwicklung für allgemeine oder sich wiederholende Aufgaben pro pro-Steuerelement. Dieses Feature war bereits verfügbar im HTML-Designer, jedoch nicht in der HTML-Editor.

1. Open **Site.Master** und suchen Sie nach der **Asp-Menü:** Element. Setzen Sie den Cursor auf dem Starttag und beachten Sie, dass das kleine Symbol angezeigt, die am unteren Rand der Element - es um das Menü "Smarttasks" zu öffnen. Klicken Sie auf. Beachten Sie, dass Sie schnellen Zugriff auf bestimmte Aufgaben im Zusammenhang mit den Menu-Steuerelement.

    ![Aufgaben für das Menüsteuerelement intelligente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Smart-Aufgaben für den Menu-Steuerelement")

    *Smarttasks für den Menu-Steuerelement*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Aufgabe 5: Intelligenter Einzug

Eine bewährte Methoden in HTML-Code wird die geschachtelten Elemente um den Code lesbarer zu halten Einzug. In Visual Studio 2012 werden Sie feststellen, dass der Editor die Elemente automatisch zieht, während Sie den Code schreiben.

> [!NOTE]
> In früheren Version von Visual Studio, intelligenten Einzug gab es in der XML-Editor aber nicht in der HTML-Editor.


1. Stellen Sie sicher, dass die Indenting-Konfiguration auf der HTML-Editor intelligenten Einzug festgelegt ist. Wählen Sie zu diesem Zweck die **Tools | Optionen** Menüoption, und wählen Sie dann die **Text-Editor | HTML | Registerkarten** Seite im linken Bereich des Bildschirms. Wählen Sie die intelligenten Einzugs-Option.

    ![HTML-Editor-Einstellungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "HTML-Editor-Einstellungen")

    *HTML-Editor-Einstellungen*
2. Auf der **"default.aspx"** Seite verwenden, entfernen Sie alle Inhalte unter dem audio-Element.
3. Platzieren Sie den Cursor am Ende das öffnendes **audio** -Element, und drücken **EINGABETASTE**.

    Beachten Sie, dass die neue Position des Cursors eine zusätzliche Einzugsebene.

    ![Den intelligenten Einzug in der HTML-Editor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "den intelligenten Einzug in der HTML-Editor")

    *Intelligenter Einzug in der HTML-Editor*
4. Wiederherstellen mit dem Inhalt, die Sie entfernt haben, oder schließen Sie das audio-Tag **"default.aspx"** ohne die Änderungen zu speichern.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Aufgabe 6: Extrahieren an Steuerelemente

Die Refactoring-Tools in Visual Studio enthalten, z. B. einen Teil des Codes in eine Funktion extrahieren sind hervorragende Funktionen, die die Verbesserung und das refactoring des vorhandenen Codes zu vereinfachen. Die Entsprechung für ASP.NET-Seiten wäre das Extrahieren von HTML-Code zu einem Benutzersteuerelement. Dies manuell würden, wie ein neues Benutzersteuerelement erstellen, verschieben Codeabschnitt auf das Benutzersteuerelement, registrieren ein Tagpräfix für das Benutzersteuerelement und, instanziieren zum Schluss das Benutzersteuerelement auf den Seiten, mehrere Schritte umfassen. Jetzt die neue *in Benutzersteuerelement extrahieren* Tool führt automatisch alle diese Schritte für Sie.

In dieser Aufgabe verwenden Sie die neue extrahieren Benutzersteuerelement kontextbezogenen Vorgang um ein neues Benutzersteuerelement aus den ausgewählten Code zu generieren.

1. Auf der **"default.aspx"** Seite die **H2** und **audio** Elemente.
2. Klicken Sie mit der rechten Maustaste auf, und wählen Sie **in Benutzersteuerelement extrahieren**.

    ![Extrahieren Sie in der Menüoption Benutzersteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Menüoption Benutzersteuerelement extrahieren")

    *Menüoption Benutzersteuerelement extrahieren*
3. Geben Sie einen Namen für das neue Benutzersteuerelement ein. Z. B. **Jukebox.ascx**, und klicken Sie dann auf **OK**.

    ![Speichern das extrahierte Benutzersteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "das extrahierte Benutzersteuerelement speichern")

    *Speichern das extrahierte Benutzersteuerelement*
4. Beachten Sie, dass der ausgewählte Code zu einem Benutzersteuerelement extrahiert wurden und am ursprüngliche Speicherort des ausgewählten Codes mit einer Instanz von das neue Benutzersteuerelement ersetzt wurde.

    ![Seite automatisch aktualisiert wird, und verwenden Sie das neue Benutzersteuerelement](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Seite automatisch aktualisiert, um das neue Benutzersteuerelement zu verwenden.")

    *Seite aktualisiert automatisch, um das neue Benutzersteuerelement zu verwenden.*
5. Drücken Sie **F5** führen Sie die Seite, und stellen Sie sicher, dass das Steuerelement funktioniert.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Übung 3: Was ist neu in der JavaScript-Editor

Schreiben oder Bearbeiten von JavaScript-Code ist keine einfache Aufgabe, insbesondere, wenn die Anwendung gestartet wird, desto größer und Sie sich bei langen Dateien und Hunderte von Funktionen. Skriptentwickler müssen sich in der Regel einige zusätzlicher Schritte, um die Lesbarkeit von Code zu verwalten, und navigieren Sie in Dateien zu erreichen. Durch die Aufnahme von JavaScript-Bibliotheken wie jQuery ist Skript Navigation aufgrund der Codelänge selbst schwierig geworden.

Visual Studio hat den JavaScript-Editor mit der Versprechen, dass den Code-Modus zugegriffen werden kann und organisierten erneuert. Viele Visual Studio-Funktionen, die bereits in c# oder VB-Editoren vorhanden waren, werden jetzt in der JavaScript-Editor implementiert: Gehe zu Definition, automatischer Einzug, Dokumentation und Validierung, wenn Sie schreiben. Mit der erneuerte IntelliSense-Liste müssen Sie den JavaScript-Funktion-Katalog zu evaluieren.

In dieser Übung lernen Sie einige der neuen Features und Verbesserungen von JavaScript-Editor. Sie Beispieldateien durchsuchen und ermitteln jede der neuen Eigenschaften, die die JavaScript-Programmierung in Visual Studio 2012 effizienter machen.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Aufgabe 1: neue Features für JavaScript-Editor

Diese Aufgabe wird Sie auf einige der neuen JavaScript-Editor-Funktionen bereitzustellen, die konzentrieren sich auf Ihren Code organisieren und eine bessere Erfahrung zu bringen.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Lösung befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Drücken Sie **F5** um die Anwendung auszuführen, klicken Sie dann auf den Link "JavaScript" in der Navigationsleiste auf. Aktualisieren Sie die Seite mehrere Male und Überprüfung wie Erhöhung des Leistungsindikators.

    ![Seitenzähler](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Seitenzähler")

    *Seitenzähler*
3. Schließen Sie den Browser, und wechseln Sie zurück zu Visual Studio.
4. Öffnen der **JavaScript.aspx** Seite, und suchen Sie die **&lt;Skript&gt;** blockieren (siehe unten).

    Der folgende Code verwendet die lokalen HTML5-Speicher zum Speichern einer *PageLoadCount* Variable, die die Anzahl der speichert die Seite der aktuelle Benutzer besucht wurde wurde. Lokaler Speicher ist eine clientseitige Schlüssel-Wert-Datenbank, die mit dem HTML5-Standard eingeführt. Die Daten werden auf dem lokalen Computer, in den Browser des Benutzers gespeichert.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Stellen Sie sicher, dass die DOCTYPE, XHTML5 festgelegt ist, bevor Sie mit den nächsten Schritten fortfahren.
5. Bearbeiten Sie den Code, und beachten Sie, dass IntelliSense für JavaScript HTML5-Features wie lokalen Speicher und ihre internen Methoden enthält.

    ![HTML5-JavaScript-Funktionen in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "HTML5, JavaScript-Funktionen in JavaScript")

    *HTML5-JavaScript-Funktionen in JavaScript*
6. Klicken Sie auf öffnende eckige Klammer (**{**) aus, das die Skriptoptionen code, und beachten Sie, dass die Klammern hervorgehoben werden.

    ![Eckige Klammern hervorgehoben werden](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "werden Klammern hervorgehoben.")

    *Es werden Klammern hervorgehoben.*
7. Heben Sie die auskommentierung der Funktion **testAutoAlign()** (Wählen Sie die drei Zeilen und Sie können **STRG** + **K**; **STRG** + **U**), und suchen Sie den Cursor innerhalb des Funktionscodes. Drücken Sie die EINGABETASTE, um eine zweite Zeile anfügen. Beachten Sie, dass der Code jetzt **ausgerichtet** und **automatisch eingerückt**.

    ![JavaScript-Code ist automatisch ausgerichtet](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "JavaScript-Code ist automatisch ausgerichtet")

    *JavaScript-Code ist automatisch ausgerichtet*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Aufgabe 2: Überprüfen von JavaScript

In dieser Aufgabe werden Sie die neue JavaScript-Validierung für den Standard ECMAScript5 feststellen. Mit diesem Feature können Sie kompatible JavaScript-Code beim verhindern, dass skripterstellungsprobleme vor der Bereitstellung des Standorts zu schreiben.

> [!NOTE]
> Visual Studio 2010 implementiert ECMAStript3-Kompatibilität, während Visual Studio 2012 ECMAScript5 Konformität bereitgestellt.


1. Open **ECMA5script5.js** befindet sich unter dem **Scripts\custom** Projektordner. Testen Sie jetzt die Überprüfung von ECMAScript5-standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Sehen Sie sich die &quot; **verwenden strenge** &quot; Richtung, in der ersten Zeile der Datei, wodurch ECMAScript5 **strict-Modus**. In diesem Modus besteht aus einer Teilmenge der Programmiersprache, die Mehrdeutigkeiten aus der letzten Ausgabe verdeutlicht, und fügt einige neue Features, z. B. Getter und Setter und bibliotheksunterstützung für JSON und umfassendere Reflektion auf Objekteigenschaften.
2. Öffnen der **Fehlerliste** Wenn nicht bereits geöffnet (**Ansicht** Menü | **Fehlerliste**). Beachten Sie, dass die **Funktion** Deklaration unterstrichen ist. Dies liegt in ECMA5 Standardfunktionen nicht innerhalb von Sprachstrukturen geschachtelt werden können. Der Fehler wird die Liste unten die Warnungsdetails angezeigt.

    ![JavaScript-Überprüfungsfehlermeldung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Überprüfungsfehlermeldung für JavaScript")

    *Fehlermeldung für JavaScript-Validierung*
3. Kommentieren Sie die **&quot;verwenden strenge&quot;** Richtung und beachten Sie, die Fehler behoben werden, aber die Warnungen zu bleiben.
4. Schreiben Sie in der letzten Zeile der Datei, die eine beliebige Zeichenfolge wie **&quot;testen&quot;** (einschließlich der Anführungszeichen, um anzugeben, dass es als Zeichenfolge). Schreiben Sie einen Punkt neben die Zeichenfolge, die Anzeigen der IntelliSense-Liste, und wählen die **trim** Option.

    Im ECMAScript5-Standard-Werte und Variablen verfügen außerdem über Zeichenfolgenmethoden definiert, wie kürzen, Großbuchstaben, suchen und ersetzen.

    ![IntelliSense-Liste in JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "in JavaScript IntelliSense-Liste")

    *IntelliSense-Liste in JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Aufgabe 3: XML-Dokumentation für JavaScript

In dieser Aufgabe untersuchen Sie Visual Studio-Funktionen für die XML-Dokumentation in JavaScript. Sie sehen, dass die XML-Dokumentation für jede Funktion in die JavaScript-IntelliSense-Liste jetzt angezeigt wird. Darüber hinaus werden Sie die Navigation-Funktion in JavaScript feststellen.

1. Open **XMLDoc.js** Datei im **Skripts/benutzerdefinierte** Projektordner. Diese Datei enthält die XML-Dokumentation auf jedem von der JavaScript-Funktionen.

    ![JavaScript XML-Dokumentation integriert, um IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "in IntelliSense integriert und JavaScript XML-Dokumentation")

    *In IntelliSense integriert und JavaScript XML-Dokumentation*
2. Unten **hinzufügen** -Funktion in **XMLDoc.js** Datei, erstellen Sie eine neue Funktion, die mit dem Namen **testen**.
3. In der **testen** funktioniert, rufen Sie die **Multiplizieren** -Funktion, die zwei Parameter empfängt. Beachten Sie, dass das QuickInfo-Feld angezeigt wird, die **Multiplizieren** Dokumentation funktionieren.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![XML-Dokumentation für JavaScript-Funktionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "XML-Dokumentation für JavaScript-Funktionen")

    *XML-Dokumentation für JavaScript-Funktionen*
4. Führen Sie die Funktion Call-Anweisung und den Typ einer *Punkt* zum Öffnen der IntelliSense-Liste auf den zurückgegebenen Wert. Beachten Sie, die Visual Studio erkennt die **zurückgeben** Wert in der Dokumentation behandelt den Wert als Zahl.

    ![XML-Dokumentation für die Rückgabetypen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "XML-Dokumentation für die Rückgabetypen")

    *XML-Dokumentation für die Rückgabetypen*
5. Fügen Sie nun einen Aufruf von Funktion hinzufügen. Beachten Sie, dass der JavaScript-Editor jetzt funktionsüberladungen unterstützt. Wenn Sie den Namen einer Funktion schreiben, werden Sie die verfügbaren Optionen, die in der Dokumentation angegebenen wählen können.

    ![XML-Dokumentation für Überladungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "XML-Dokumentation für Überladungen")

    *XML-Dokumentation für Überladungen*
6. Open **GotoDefinition.js** Datei, und suchen Sie die **$().html()** Funktionsaufruf. Suchen Sie den Cursor auf **html**.
7. Drücken Sie **F12** und navigieren Sie zu der Definition. Beachten Sie, dass Sie können jetzt Zugriff auf und Suchen nach JavaScript-Code ohne Verwendung der **finden** Tool.
8. Suchen Sie den Cursor auf die jQuery-Anweisung, bevor Sie den Signaturblock am unteren Rand der Codedatei ein. Drücken Sie **F12**. Sie gelangen auf die jQuery-Bibliothek-Datei. Beachten Sie, Sie können auch auf die jQuery-Dateien, die mithilfe von navigieren **F12**.

    ![Navigieren zu jQuery-Definitionen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navigieren zu jQuery-Definitionen")

    *Navigieren zu jQuery-Definitionen*

> [!NOTE]
> Stellen Sie sicher, dass GotoDefinition.js keine Syntaxfehler aufweist, bevor Sie die Datei speichern.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Übung 4: Bündelung und Minimierung

Wie viele Male beinhalten Ihrer Websites mehr als ein JavaScript- oder CSS-Datei? Dies ist ein sehr häufiges Szenario, in denen Bündelung und Minimierung können zum Reduzieren der Dateigröße und die Website schneller ausgeführt. Das neue Bündelung-Feature in ASP.NET 4.5 packs einen Satz von js- oder CSS-Dateien in einem einzelnen Element, und reduziert die Größe durch den Inhalt (z. B. nicht erforderliche Leerzeichen entfernen, Entfernen von Kommentaren, Bezeichnern zu reduzieren) minimieren.

Bündelung und Minimierung in ASP.NET 4.5 erfolgt zur Laufzeit, damit der Prozess identifizieren den Benutzer-Agent (z. B. Internet Explorer, Mozilla, usw.), und kann daher die Komprimierung verbessern, indem Sie auf den Browser des Benutzers (z. B. entfernen Elemente, die bestimmte Mozilla ist Wenn die Anforderung von IE stammt).

In dieser Übung lernen Sie, wie Sie aktivieren und verwenden die verschiedenen Typen von Bündelung und Minimierung in ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Aufgabe 1: Installieren der Bündelung und Minimierung-Paket von NuGet

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Lösung befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. Öffnen Sie die NuGet-Paket-Manager-Konsole. Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Other Windows** | **-Paket-Manager-Konsole**.

    ![Öffnen die Paket-Manager-file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "die Paket-Manager-Konsole öffnen")

    *Öffnen die Paket-Manager-Konsole*
3. In der **-Paket-Manager-Konsole** Typ **Install-Package Microsoft.Web.Optimization** , und drücken Sie **EINGABETASTE**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Aufgabe 2: Standardbundles

Die einfachste Möglichkeit, Bündelung und Minimierung zu verwenden ist, um den standardbündeln zu aktivieren. Diese Methode verwendet die Konventionen der gebündelte und minimierte Version für die js- und CSS-Dateien in einem Ordner verweisen zu können.

In dieser Aufgabe erfahren Sie, wie aktivieren, oder auf die gebündelten und minimierten js- und CSS-Dateien verweisen, und zeigen Sie die resultierende Ausgabe.

1. Wenn nicht bereits geöffnet ist, starten Sie **Visual Studio** , und öffnen Sie die **WhatsNewASPNET.sln** Lösung befindet sich in der **Source\WhatsNewASPNET** Ordner dieser Anleitung.
2. In der **Projektmappen-Explorer**, erweitern Sie die **Stile**, **Scripts\custom** und **Scripts\bundle** Ordner.

    Beachten Sie, dass mehrere CSS- und JS-Datei der Anwendung verwendet wird.

    ![Mehrere Stylesheets und JavaScript-Dateien in der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "mehrere Stylesheets und JavaScript-Dateien in der Anwendung")

    *Mehrere Stylesheets und JavaScript-Dateien in der Anwendung*
3. Öffnen der **"Global.asax.cs"** Datei.

    Beachten Sie, dass die neue **Microsoft.Web.Optimization** Namespace am Anfang der Datei auskommentiert ist. Auskommentierung aufheben, die mit Direktive, um die Bündelung und Minimierung Funktionen umfassen.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Suchen Sie die **Anwendung\_starten** Methode.

    Bei dieser Methode heben Sie die auskommentierung des Aufrufs EnableDefaultBundles, wie im folgenden Codeausschnitt gezeigt. Können wir eine gebündelte Sammlung von CSS-Dateien in einem Ordner zu verweisen, indem Sie den Pfad für diesen Ordner sowie die &quot;CSS&quot; oder &quot;JS&quot; Suffix.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Öffnen der **Optimization.aspx** Datei, und suchen Sie das Steuerelement für **HeadContent**.

    Beachten Sie, dass der CSS-Dateien und die JS-Dateien ein einzelnes Tag für die auf die verwiesen wird.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Dieser Code ist für Demozwecke zu nutzen. Im Idealfall werden Sie auf die Pakete in die Datei Site.Master verweisen. In diesem Beispielcode werden Sie feststellen, dass einige der gebündelten Dateien auch durch die Datei Site.Master referenziert werden, die redundante dieser letzten verweist.
6. Beachten Sie, dass die Links in die Bündelung Konventionen verwenden die **Href** Attribut, das CSS oder JS-Dateien aus der Formatvorlagen und Scripts\custom ab Ordner bzw.

    Können Sie den Pfad **Skripts/benutzerdefinierte/JS** wie unten dargestellt zu bündeln und minimieren die JS-Dateien in einem **Skripts/benutzerdefinierte** Ordner. Dies ist das Standardverhalten, mit der Standard-Paketen.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Öffnen der **Styles\Site.css** Datei.

    Beachten Sie, dass die ursprüngliche CSS-Datei enthält, eingerückt Code, Leerzeichen und Kommentare, die die Datei zu vergrößern. (Enthält auch die JavaScript-Datei Leerzeichen und Kommentare).

    ![Eine der ursprünglichen CSS-Dateien im Ordner "Scripts"](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "eine des ursprünglichen CSS-Dateien im Ordner \"Scripts\"")

    *Eine der ursprünglichen CSS-Dateien im Ordner "Scripts"*
8. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren zu der **Optimierung** Seite.
9. Klicken Sie auf die **CSS-Bundle** Link zum Herunterladen und öffnen Sie die Datei.

    Sehen Sie sich die minimierte gebündelte Datei. Sie werden feststellen, dass alle Leerzeichen, Kommentare und Einzug Zeichen entfernt wurden, generieren eine kleinere Datei.

    ![CSS-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "gebündelten CSS-Dateien")

    *Gebündelten CSS-Dateien*
10. Klicken Sie nun die **JS-Bundle** Link, um die gebündelten JavaScript-Datei zu öffnen. Sie können den Explorer Warnung ignorieren. Beachten Sie, dass die JavaScript-Dateien unter dem **benutzerdefinierte** Ordner werden auch gebündelten und minimierten.

    ![JavaScript-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "gebündelt JavaScript-Dateien")

    *Gebündelte JavaScript-Dateien*

    Aktivieren der Komprimierung für CSS- oder JS-Dateien wurde in früheren Version von ASP.NET sehr viel komplizierter. Nun, wie Sie gesehen haben, müssen Sie nur eine Zeile im Hinzufügen der *"Global.asax"* -Datei, um die Aktivierung der Bündelung und verweisen dann von Ihrer Website gebündelten Dateien.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Aufgabe 3: statische Pakete

Der statische Bundle-Ansatz können Sie zum Anpassen des Satz von Dateien zum Paket, den Verweis und der Minimierung-Methode, die verwendet werden.

In dieser Aufgabe Konfigurieren Sie eine statische-Bundle, um einen bestimmten Satz von Dateien, die Bündelung und Minimierung definieren.

1. Schließen Sie den Browser.
2. Öffnen der **"Global.asax.cs"** Datei, und suchen Sie die **Anwendung\_starten** Methode.
3. Entfernen Sie die statische Bundle-Code wie im folgenden Code gezeigt.

    Definieren Sie eine statische Paket, das mit verwiesen wird die &quot; **~/StaticBundle** &quot; virtuellen Pfad und der Verwendung **JsMinify** für die Minimierung von alle angegebenen Dateien mit der **AddFile** Methode. Schließlich das statische Paket zum Hinzufügen der **BundleTable** und aktivieren.

    Beachten Sie, dass die Dateien nicht am selben Ort befinden. Dies ist ein weiterer Vorteil über die standardmäßigen bündeln.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Öffnen der **Optimization.aspx** Datei.

    Beachten Sie, dass der Link zum **statische JS-Bundle** wird anhand des Pfads, der bei der Konfiguration des statischen Pakets in der Datei "Global.asax.cs" deklariert haben: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.
6. Klicken Sie auf die **statische JS-Bundle** Link zum Öffnen der Datei.

    Beachten Sie, dass der verkleinerte JavaScript-Datei gebündelt wird die Ausgabe für alle JavaScript-Dateien, die in der statischen Bundle-Datei unter dem Pfad konfiguriert &quot;/StaticBundle&quot;.

    ![Statische JavaScript-Dateien Bundle](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "statische JavaScript-Dateien-Paket")

    *Statische JavaScript-Dateien gebündelt werden.*
7. Schließen Sie den Browser, und kehren Sie zu Visual Studio zurück.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Aufgabe 4: dynamische Ordner Pakete

In dieser Aufgabe lernen Sie die Ordner "dynamic" Pakete konfigurieren. Die Leistungsfähigkeit von dynamischen Bündelung ist, dass Sie statische JavaScript sowie andere Dateien enthalten, Sprachen, die in JavaScript kompiliert wird und benötigen daher einige verarbeiten, bevor der Bündelung ausgeführt wird.

In diesem Beispiel erfahren Sie, wie mit der **DynamicFolderBundle** Klasse, um ein dynamisches Paket für Dateien, die in CofeeScript geschrieben zu erstellen. CofeeScript ist eine Programmiersprache, die in JavaScript kompiliert und eine einfachere Syntax für das Schreiben von JavaScript-Code, Verbessern der Übersichtlichkeit und Lesbarkeit von JavaScript.

1. Öffnen der **"Global.asax.cs"** Datei, und suchen Sie die **Anwendung\_starten** Methode.
2. Entfernen Sie die dynamische Bundle-Code wie im folgenden Code gezeigt.

    Definieren Sie eine dynamische ordnerpaket, die verwendet werden die **CoffeeMinify** benutzerdefinierte Minimierung-Prozessor, die nur für Dateien mit den gelten die &quot; **. Coffee** &quot; Erweiterung ( CoffeeScript-Dateien). Beachten Sie, dass Sie ein Suchmuster, zum Auswählen der Dateien verwenden können gebündelt werden in einem Ordner, z. B. "\*. Coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Öffnen Sie die NuGet-Paket-Manager-Konsole. Zu diesem Zweck verwenden Sie das Menü **Ansicht** | **Other Windows** | **-Paket-Manager-Konsole**.
4. In der **-Paket-Manager-Konsole** Typ **Install-Package CoffeeSharp** , und drücken Sie **EINGABETASTE**.
5. Klicken Sie auf die **alle Dateien anzeigen** Schaltfläche der **Projektmappen-Explorer** Fenster

    ![Anzeigen aller Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Anzeigen aller Dateien")

    *Anzeigen aller Dateien*
6. Klicken Sie mit der rechten Maustaste auf die **CoffeeMinify.cs** Datei die **Projektmappen-Explorer** , und wählen Sie **in Projekt einschließen**

    ![Schließen Sie die CoffeeMinify.cs-Datei im Projekt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "die CoffeeMinify.cs-Datei in das Projekt einfügen.")

    *Die CoffeeMinify.cs-Datei in das Projekt einfügen.*
7. Öffnen der **CoffeeMinify.cs** Datei.

    Diese Klasse erbt von JsMinify, die JavaScript-Ausgabe der CoffeeScript-Code-Kompilierung zu minimieren. Ruft den CoffeeScript-Compiler zu generieren Sie zuerst den JavaScript-Code, und er sendet diese dann an die JsMinify.Process-Methode, die den resultierenden Code zu minimieren.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Öffnen der **Script1.coffee** und **Script2.coffee** Dateien aus dem **Skripts/Bundle** Ordner.

    Diese Dateien enthalten den CoffeScript Code kompiliert werden, beim Ausführen der Bündelung mit der CoffeeMinify-Klasse.

    Aus Gründen der Einfachheit halber einbeziehen die bereitgestellten CoffeeScript-Dateien nur CoffeeScript-Beispielcode. Die Kommentare werden durch den Prozess JsMinify ausgeschlossen.

    ![CoffeeScript-Dateien](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript-Dateien")

    *CoffeeScript-Dateien*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) bietet eine einfachere Syntax für das Schreiben von JavaScript-Code, Verbessern der Übersichtlichkeit und Lesbarkeit von JavaScript, sowie andere Funktionen wie das Array Verständnis und zum Musterabgleich hinzufügen.
9. Öffnen der **Optimization.aspx** Datei, und suchen Sie die Paket-Links.

    Beachten Sie, dass der Link zum **dynamische JS-Bundle** verweist auf die **Skripts/Paket** Ordner mit der **/Kaffee** Suffix, die Sie für das dynamische ordnerpaket konfiguriert.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Drücken Sie **F5** , führen Sie die Anwendung, und navigieren Sie zu der **Optimierung** Seite.
11. Klicken Sie auf die **dynamische JS-Bundle** Link, um die generierte Datei zu öffnen.

    Beachten Sie, dass mit der Inhalt, die in diesem Paket enthalten war nur **. Coffee** Dateien. Sie sehen auch, dass der CoffeeScript-Code in JavaScript kompiliert wurde und die auskommentierten Zeilen entfernt wurde.

    ![Dynamische JS-Dateien gebündelt](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "dynamische JS-Dateien-Paket")

    *Dynamische JS-Dateien gebündelt werden.*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Diese Übungseinheit hilft Ihnen, zu verstehen, was neu in ASP.NET und Webentwicklung in Visual Studio 2012 ist und wie die Vielzahl der Verbesserungen in Visual Studio 2012 nutzen.

Von dieser praktischen Übungseinheit haben Sie Gewusst wie: Verwenden Sie die neuen Features und Verbesserungen in Visual Studio 2012-Editoren für CSS, JavaScript und HTML-Daten. Darüber hinaus haben Sie gelernt, wie Visual Studio 2012 integrierte Bündelung und Minimierung implementiert.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *Visual Studio Express für Web-Kachel*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang B: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder. Geben Sie einen Namen für die Regel, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Bestätigen von Änderungen*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

    ![Anwendung auf Windows Azure veröffentlicht](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Anwendung auf Windows Azure veröffentlicht")

    *Anwendung in Windows Azure veröffentlicht*
