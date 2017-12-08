---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: "Praktische Übungseinheit: Visual Studio 2013-Webtools | Microsoft Docs"
author: rick-anderson
description: "Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte. Er enthält einen leistungsstarke Text-Editor, der auf einfache Weise verwendet werden kann..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Praktische Übungseinheit: Visual Studio 2013-Webtools
====================
durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte. Sie enthält einen leistungsstarke Text-Editor, der einfach verwendet werden kann, um eigenständige Dateien ohne ein Projekt zu bearbeiten.
> 
> Visual Studio verwaltet eine voll ausgestattete Analysestruktur, wie Sie jede Datei bearbeiten. Dadurch können Visual Studio unvergleichliche automatische Vervollständigung und Dokument basierende Aktionen beim Erstellen der entwicklungserfahrung wesentlich schneller und bequemen bereitstellen. Diese Funktionen sind besonders leistungsstark in HTML und CSS-Dokumenten.
> 
> Alle von dieser Power steht auch für Erweiterungen, einfach die Editoren leistungsstarke neue Features nach Ihren Bedürfnissen zu erweitern. Web Essentials ist eine Auflistung von (größtenteils) Web-bezogene Erweiterungen für Visual Studio. Er umfasst viele neue IntelliSense Beendigungen (insbesondere für CSS), neue Browserlink-Funktionen, automatische JSHint für JavaScript-Dateien, die neue Warnungen für HTML, CSS und viele weitere Funktionen, die für moderne Webentwicklung unverzichtbar sind.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden Sie neue HTML-Features im Editor im Web Essentials enthalten sind, z. B. umfangreiche HTML5-Codeausschnitte und Zen-Codierung
- Verwenden der neue CSS-Editor-Funktionen in Web Essentials enthalten sind, z. B. die Farbauswahl und ein Browser Matrix QuickInfo
- Verwenden Sie die neue JavaScript-Editor-Funktionen in Web Essentials z. B. "in der Datei und IntelliSense extrahieren" für alle HTML-Elemente enthalten
- Exchange-Daten zwischen dem Browser und Visual Studio mithilfe der Browserlink

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) oder höher
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie in des Labors **Quelle** Ordner.
2. Mit der rechten Maustaste **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** um den Setupvorgang zu starten, die Ihrer Umgebung konfigurieren und installieren die Visual Studio-Codeausschnitte für diese Übungseinheit.
3. Wenn das Dialogfeld Benutzerkontensteuerung angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese Umgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden die Codeausschnitte

In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie in Visual Studio 2013 zu vermeiden, dass sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung wird eine beginnend Projektmappe befindet sich im beiliegen der **beginnen** Ordner der Übung, die Ihnen ermöglicht, jede Übung unabhängig von den anderen folgen. Denken Sie daran, dass die Codeausschnitte, die während einer Übung hinzugefügt werden in diese Lösungen starten fehlen, werden und funktionieren möglicherweise nicht, bis die Übung abgeschlossen ist. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Arbeiten mit Browserlink und Web Essentials](#Exercise1)
2. [Vorteile von IntelliSense und Codeausschnitten](#Exercise2)

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient zur einer bestimmten Entwicklungsstil und Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Optionen des Dialogfelds bestimmt. In dieser Übung wird beschrieben, die Aktionen erforderlich, um eine bestimmte Aufgabe in Visual Studio auszuführen, bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wählen Sie eine Auflistung von unterschiedlichen Einstellungen für Ihre Entwicklungsumgebung möglicherweise Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Übung 1: Arbeiten mit Browserlink und Web Essentials

**Web Essentials** ist eine Visual Studio-Erweiterung, die eine Vielzahl von nützlichen Funktionen für moderne Webentwicklung meist darauf konzentriert, die Entwicklungsaufgaben Web wesentlich schneller und bequemen hinzufügt. Sie können Web Essentials aus dem Katalog Erweiterung in Visual Studio installieren.

**Browserlink** ist ein neues Feature in Visual Studio 2013, die einen Kanal zwischen Visual Studio-IDE und jeden beliebigen Browser öffnen, für den Austausch von Daten zwischen der Webanwendung und Visual Studio enthalten. Web Essentials erweitert Browserlink mit Tools zum Bearbeiten des DOM-Objekt und der CSS-Formatvorlagen der Webseiten direkt über den Browser.

In dieser Übung untersuchen Sie einige der unterstützten Funktionen **Web Essentials** und **Browserlink** um eine einfache Quiz-Seite zu verbessern.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Aufgabe 1: Ausführen des Projekts in mehreren Browsern

In dieser Aufgabe Konfigurieren Sie Ihre Webanwendung gleichzeitig in mehreren Browsern ausgeführt was für browserübergreifende Tests nützlich ist.

1. Open **Microsoft Visual Studio**.
2. In der **Datei** klicken Sie im Menü **öffnen | Projekt/Projektmappe...**  und navigieren Sie zu **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** in der **Quelle** Ordner die Umgebung fertiggestellt (C:\WebCampsTK\HOL\VSWebTooling\Source). Wählen Sie **Begin.sln** , und klicken Sie auf **öffnen**.
3. Klicken Sie in der Symbolleiste von Visual Studio erweitern Sie im Browser-Menü, und wählen **durchsuchen mit...** .

    ![Navigieren Sie mit der Menüoption](visual-studio-2013-web-tools/_static/image1.png "durchsuchen mit... Klicken Sie im Browser")

    *Navigieren Sie mit der Menüoption*
4. In der **Browserauswahl** (Dialogfeld), wählen Sie **Google Chrome** und **Internet Explorer** halten die **STRG** Taste, und klicken Sie auf  **Als Standard festlegen**.

    ![Navigieren Sie im Dialogfeld](visual-studio-2013-web-tools/_static/image2.png "Browserauswahl (Dialogfeld)")

    *Auswählen mehrerer Standardbrowser*
5. Google Chrome und Internet Explorer sollte nun als der Standardbrowser angezeigt werden. Klicken Sie auf **"Abbrechen"** um das Dialogfeld zu schließen.

    ![Google Chrome und Internet Explorer als Standardbrowser](visual-studio-2013-web-tools/_static/image3.png "standardmäßig den Browsern Google Chrome und Internet Explorer")

    *Google Chrome und Internet Explorer als Standardbrowser*

    > [!NOTE]
    > Nach dem Konfigurieren der Standardbrowser der **mehreren Browsern** Option ausgewählt ist, klicken Sie im Browser.
    > 
    > ![Mehreren Browsern](visual-studio-2013-web-tools/_static/image4.png "mehreren Browsern")
6. Drücken Sie **STRG** + **F5** um die Anwendung ohne Debuggen auszuführen.
7. Wenn beide Browserfenster öffnen, platzieren Sie eine von ihnen über die anderen um die Updates auf beiden Browsern gleichzeitig angezeigt. Der Browser sollte eine Webseite mit einem hellblaue Rechteck angezeigt werden.

    ![Platzieren einen Browser übereinander](visual-studio-2013-web-tools/_static/image5.png "ein Browser übereinander platzieren")

    *Platzieren einen Browser übereinander*
8. Schließen Sie den Browser nicht. Sie verwenden sie in der nächsten Aufgabe.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Aufgabe 2: Verwendung Zen Programmierung zum Gewährleisten der HTML-Elemente erstellen

**Codierung Zen** ist ein Editor-Plug-In für Hochgeschwindigkeits-HTML, XML, XSL-(oder jedes andere strukturiertem Code-Format) zu programmieren und zu bearbeiten. Der Kern von dieses Plug-in ist eine leistungsstarke Abkürzung-Engine - ähnlich wie CSS-Selektoren - Ausdrücke in HTML-Code erweitern können. Codierung Zen bietet eine schnelle Möglichkeit für das Formatieren von HTML mithilfe einer CSS-Selektor-Syntax schreiben.

In dieser Übung verwenden Sie die Codierung Zen-Funktion von Web Essentials bereitgestellt, die HTML-Schaltflächen generieren, die die Optionen der Frage darstellen.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der **Index.cshtml** -Datei die **Ansichten** | **Home** Ordner.
3. Ersetzen Sie die  **&lt;!--TODO: Hier--Optionen hinzufügen&gt;**  Kommentar mit dem folgenden Code, und drücken Sie **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Der Code sollte HTML erweitert werden.

    ![Erweitert HTML](visual-studio-2013-web-tools/_static/image6.png "erweitert HTML")

    *Erweiterte HTML*

    > [!NOTE]
    > Weitere Informationen zur Codierung Zen Syntax finden Sie unter den folgenden [Artikel](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Klicken Sie auf die **aktualisieren verknüpfte Browsern** Schaltfläche, um beide Browser zu aktualisieren.

    ![Aktualisieren Sie verknüpfte Browsern](visual-studio-2013-web-tools/_static/image7.png "verknüpften Browser aktualisieren")

    *Aktualisieren Sie verknüpfte Browsern*

    ![Internet Explorer - Seite mit vier Schaltflächen aktualisiert](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - Seite aktualisiert, mit vier Schaltflächen")

    *Internet Explorer - Seite mit vier Schaltflächen aktualisiert*

    ![Google Chrome - Seite mit vier Schaltflächen aktualisiert](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - Seite aktualisiert, mit vier Schaltflächen")

    *Google Chrome - Seite mit vier Schaltflächen aktualisiert*
6. Wechseln Sie zurück zu Visual Studio.
7. Sie haben die Schaltflächen auf der Seite hinzugefügt, jedoch müssen Sie immer noch eine Vorlage Frage hinzufügen. Zu diesem Zweck verwenden Sie ein neues Feature in Web Essentials aufgerufen **Lorem Ipsum Generator**. Suchen Sie die **Div** Element mit der **Klasse** Attribut **Vorderseite**.
8. Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div**, und drücken Sie die **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Der Code sollte HTML erweitert werden.

    ![Lorem Ipsum automatisch generierten](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum automatisch generierten")

    *Lorem Ipsum automatisch generierten*

    > [!NOTE]
    > Im Rahmen des Zen codieren können Sie jetzt Lorem Ipsum Code direkt im HTML-Editor generieren. Geben Sie einfach **Lorem** und klicken Sie **Registerkarte** und eine 30 word Lorem Ipsum Text eingefügt werden. Z. B. *lorem10* fügt 10 Lorem Ipsum Wörter.
10. Fügen Sie ein Logo am oberen Rand die Frage mit der eine weitere neue Funktion in Web Essentials aufgerufen **Lorem Pixel Generator**. Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div** Element mit **Container** als **Klasse** Wert, und drücken Sie **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Der Code sollte in HTML erweitern.

    ![Lorem Pixel automatisch generierten](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel automatisch generierten")

    *Lorem Pixel automatisch generierten*

    > [!NOTE]
    > Im Rahmen des Zen codieren können Sie auch Lorem Pixel Code direkt im HTML-Editor generieren. Geben Sie einfach **Pix-200 x 200-Tieren** und klicken Sie **Registerkarte** und ein **Img** Tag mit einem 200 x 200-Image von einem Tier eingefügt werden. Weitere Informationen finden Sie unter [Lorem Pixel](http://www.lorempixel.com).
12. Klicken Sie auf die **aktualisieren verknüpfte Browsern** Schaltfläche, um beide Browser zu aktualisieren.

    ![Internet Explorer - automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - automatisch generierte Bild und Text")

    *Internet Explorer - automatisch generierte Bild und text*

    ![Google Chrome - automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - automatisch generierte Bild und Text")

    *Google Chrome - automatisch generierte Bild und text*

    > [!NOTE]
    > Da das Bild nach dem Zufallsprinzip ausgewählt ist, wenn Sie den Codeausschnitt hinzufügen abweichen das Bild im Browser angezeigt.
13. Schließen Sie den Browser nicht. Sie verwenden sie in der nächsten Aufgabe.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Aufgabe 3: Aktualisieren einer Style-Eigenschaft

In dieser Aufgabe verwenden Sie den Browserlink **überprüfen Modus** Feature zum Erkennen des exakten Speicherort, an dem das bestimmte DOM-Element generiert wird, und aktualisieren Sie dann die Color-Eigenschaft des jeweiligen Elements, das mit einer vom Webdienst bereitgestellten Farbwähler Essentials.

1. Drücken Sie in Internet Explorer-Browser **STRG** + **ALT** + **ich** überprüfen Modus zu aktivieren.
2. Bewegen Sie den Zeiger über den hellen blauen Rahmen, und klicken Sie auf.

    ![Verschieben den Zeiger über den hellen blauen Rahmen](visual-studio-2013-web-tools/_static/image14.png "verschieben den Zeiger über den hellen blauen Rahmen")

    *Verschieben Sie den Zeiger über den hellen blauen Rahmen*
3. Wechseln Sie zurück zu Visual Studio. Beachten Sie, wie das HTML-Element, das Sie im Browser ausgewählt auch in der Visual Studio-HTML-Editor ausgewählt ist.

    ![HTML-Element in der Visual Studio-HTML-Editor ausgewählten](visual-studio-2013-web-tools/_static/image15.png "HTML-Element in der Visual Studio-HTML-Editor ausgewählt")

    *HTML-Element in der Visual Studio-HTML-Editor ausgewählt*
4. Aktualisieren Sie jetzt die **Vorderseite** CSS-Klasse, um das Format des ausgewählten Elements zu ändern. Drücken Sie die zu diesem Zweck **STRG** + **,** So öffnen die **Navigieren zu** Suchfeld. Typ **"Site.CSS" ändern** , und drücken Sie **EINGABETASTE** zum Öffnen der Datei.

    ![Öffnen die Datei "Site.CSS" ändern](visual-studio-2013-web-tools/_static/image16.png "Öffnen der Datei "Site.CSS" ändern")

    *Öffnen der Datei "Site.CSS" ändern*
5. Drücken Sie **STRG** + **F** und Typ **.flip Containter .front** die CSS-Auswahl gefunden.
6. Klicken Sie auf das Licht Blau Quadrat in die Border-Eigenschaft der Klasse, um die Farbauswahl zu öffnen.

    ![Öffnen die Farbauswahl](visual-studio-2013-web-tools/_static/image17.png "öffnen die Farbauswahl")

    *Öffnen die Farbauswahl*
7. Erweitern Sie die Farbauswahl, indem Sie auf die Chevron-Schaltfläche, und wählen Sie eine neue Farbe.

    ![Erweitern die Farbauswahl](visual-studio-2013-web-tools/_static/image18.png "erweitern die Farbauswahl")

    *Erweitern die Farbauswahl*
8. Drücken Sie **STRG** + **ALT** + **EINGABETASTE** verknüpften Browser zu aktualisieren.
9. Wechseln Sie zu Internet Explorer, und beachten Sie, wie die Farbe des Rahmens geändert hat.

    ![Internet Explorer - Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - Rahmenfarbe aktualisiert")

    *Internet Explorer - Rahmenfarbe aktualisiert*
10. Wechseln Sie zu Google Chrome, und beachten Sie, wie die Farbe des Rahmens geändert hat.

    ![Google Chrome - Rahmenfarbe aktualisiert](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - Rahmenfarbe aktualisiert")

    *Google Chrome - Rahmenfarbe aktualisiert*
11. Wechseln Sie zurück zu Visual Studio.
12. Wechseln Sie an das Ende der **"Site.CSS" ändern** -Datei, und drücken Sie **STRG** + **F** zum Suchen der **.btn** Selektor.
13. Beachten Sie, dass die **- Webkit-Border-Radius** Eigenschaft Grün unterstrichen ist.

    ![-Webkit-Border-Radius-Eigenschaft des Btn auswahlzeigers](visual-studio-2013-web-tools/_static/image21.png "- Webkit-Border-Radius-Eigenschaft des Btn auswahlzeigers")

    *-Webkit-Border-Radius-Eigenschaft des Btn auswahlzeigers*
14. Einfügen des Caretzeichens in das **- Webkit-Border-Radius** Eigenschaft. Unter dem ersten Buchstaben des ersten Wortes der Eigenschaft sollte eine blaue Linie angezeigt. Dies ist die **Smarttag**.
15. Drücken Sie **STRG** + **.** Öffnen Sie das Menü Vorschläge, und klicken Sie auf **hinzufügen, die Standardeigenschaft (Border-Radius) fehlt**.

    ![Hinzufügen fehlender Standardeigenschaft Vorschlag](visual-studio-2013-web-tools/_static/image22.png "fehlt der Standardeigenschaft Vorschlag hinzufügen")

    *Fügen Sie fehlender Standardeigenschaft Vorschlag hinzu*
16. Die **Border-Radius** Eigenschaft der CSS-Regel automatisch hinzugefügt.

    ![Fehlende Standardeigenschaft hinzugefügt](visual-studio-2013-web-tools/_static/image23.png "fehlt der Standardeigenschaft hinzugefügt")

    *Fehlende Standardeigenschaft hinzugefügt*
17. Bewegen Sie den Mauszeiger über die **Border-Radius** Eigenschaft zum Anzeigen der **Browser Matrix QuickInfo**. Die **Browser Matrix QuickInfo** zeigt die Verfügbarkeit der Eigenschaft in jedem Browser.

    ![Browser Matrix QuickInfo](visual-studio-2013-web-tools/_static/image24.png "Browser Matrix QuickInfo")

    *Browser Matrix QuickInfo*
18. Beachten Sie, dass der Wert von der **Border-Radius** Eigenschaft noch unterstrichen ist. Bewegen Sie den Zeiger über den Wert, der die Warnmeldung angezeigt.

    ![Border-Radius-Eigenschaft Wert Warnung](visual-studio-2013-web-tools/_static/image25.png "Border-Radius-Eigenschaft-Wert-Warnung")

    *Border-Radius-Eigenschaft-Wert-Warnung*
19. Entfernen Sie die Einheit für die **Border-Radius** Eigenschaftswert gemäß der von der QuickInfo.
20. Als **Border-Radius** ist die Standardeigenschaft für definieren wie abgerundeten Ecken sind, können Sie entfernen Rahmen der **- Webkit-Border-Radius** Eigenschaft und Wert in der CSS-Regel.
21. Einfügen des Caretzeichens in das **Zeilenumbruch** -Eigenschaft, und beachten Sie die unten auch das Smarttag angezeigt wird.
22. Öffnen Sie das Menü, und klicken Sie auf **hinzufügen fehlender Hersteller Besonderheiten**.

    ![Hinzufügen fehlender Hersteller Besonderheiten Vorschlag](visual-studio-2013-web-tools/_static/image26.png "fehlende Hersteller Besonderheiten Vorschlag hinzufügen")

    *Fehlende Hersteller Besonderheiten Vorschlag hinzufügen*
23. Die **-ms-Zeilenumbruch** Eigenschaft der CSS-Regel automatisch hinzugefügt.

    ![Bestimmte Hersteller (Eigenschaft) hinzugefügt](visual-studio-2013-web-tools/_static/image27.png "bestimmten Hersteller (Eigenschaft) hinzugefügt")

    *Bestimmte Hersteller (Eigenschaft) hinzugefügt*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Aufgabe 4: Aktualisieren der HTML-Code im Browser

In dieser Aufgabe verwenden Sie den Browserlink **Entwurfsmodus** Feature bearbeiten das DOM-Objekt über den Browser, und übertragen die Änderungen an der HTML-Quelldatei in Visual Studio.

1. Drücken Sie im Browser Google Chrome **STRG** + **ALT** + **D** So aktivieren Sie den Entwurfsmodus zu wechseln.
2. Bewegen Sie den Mauszeiger über die **Lorem Ipsum Dolor Sit Amet** Bezeichnung, und klicken Sie auf.

    ![Bearbeiten die Frage](visual-studio-2013-web-tools/_static/image28.png "die Frage bearbeiten")

    *Die Frage bearbeiten*
3. Ein Cursor sollte angezeigt werden. Ersetzen Sie den ursprünglichen Text mit *aussehen wie beim Schreiben von längeren Frage?*, und drücken Sie dann die **ESC** zum Entwurfsmodus zu beenden.

    ![Frage bearbeitet](visual-studio-2013-web-tools/_static/image29.png "Frage bearbeitet")

    *Frage bearbeitet*
4. Wechseln Sie zurück zu Visual Studio und öffnen **Index.cshtml**, sofern nicht bereits geöffnet. Beachten Sie, dass der innere Text des der  **&lt;p&gt;**  Element aktualisiert wurde.

    ![Aktualisierte Frage in das HTML-Seite](visual-studio-2013-web-tools/_static/image30.png "aktualisierte Frage in das HTML-Seite")

    *Aktualisierte Frage in das HTML-Seite*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Aufgabe 5: Überprüfen SEO-bezogene Warnungen

**Suchen Engine Optimization** (SEO) versteht man höheren Rang Website auf einer Suchmaschine die Liste der Ergebnisse vornehmen. Ein höherer Rangfolge bestimmt der Standort und desto konsistent aufgeführt, mehr Besucher die Website von diesem Suchmaschine erhalten. Web Essentials umfasst eine analytische Tool, mit dem HTML untersucht, Berichte, die Probleme gefunden und bietet Unterstützung nach ", um das Problem beheben.

1. Wechseln Sie zu der **Ansicht** Menü, und klicken Sie auf **Fehlerliste** So öffnen die **Fehlerliste** Fenster.

    ![In der Sicht Fehlerliste Menü](visual-studio-2013-web-tools/_static/image31.png "Fehlerliste im Menü "Ansicht"")

    *Fehler in der Sicht im Menü angezeigt.*
2. Beachten Sie, dass ein SEO-Warnung benachrichtigt, die eine  **&lt;Meta&gt;**  tag für die Beschreibung für die Seite fehlt. Doppelklicken Sie auf den Eintrag SEO Warnung, um dies zu beheben.

    ![Fehlerliste (Fenster)](visual-studio-2013-web-tools/_static/image32.png "Fenster "Fehlerliste"")

    *Fehlerliste (Fenster)*
3. In der **Web Essentials** (Dialogfeld), klicken Sie auf **Ja** zum Einfügen einer Beschreibung &lt;Meta&gt; Tag.

    ![Dialogfeld für Web Essentials](visual-studio-2013-web-tools/_static/image33.png "Web Essentials (Dialogfeld)")

    *Essentials (Dialogfeld)*
4. Der Editor für  **\_Layout.cshtml** wird geöffnet und die  **&lt;Meta&gt;**  Tag wird automatisch hinzugefügt, die **Head** Teil der HTML-Datei.

    ![Meta-Tag _Layout auf der Seite automatisch hinzugefügt](visual-studio-2013-web-tools/_static/image34.png "Meta-Tag _Layout auf der Seite automatisch hinzugefügt.")

    *Meta-Tag automatisch hinzugefügt, \_-Layoutseite*
5. Ändern Sie den Wert, der die **Inhalt** -Attribut *GeekQuiz* und speichern Sie die Datei.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Übung 2: Nutzen der Vorteile von IntelliSense und Codeausschnitten

Mit Web Essentials wurde der HTML-Editor mit zusätzlicher Funktionalität erweitert. In dieser Übung sehen Sie einige neuen Funktionen, die beim Entwickeln von Webanwendungen hilfreich sind.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Aufgabe 1: Verwenden von IntelliSense in HTML-Dokumente

Die erste neue Funktion Sie in dieser Aufgabe sehen wird aufgerufen, **dynamische IntelliSense**. Dynamische IntelliSense liest andere Tags und Attribute, die mögliche Ids abzuleiten, die Sie verwenden.

In dieser Aufgabe erstellen Sie ein neues HTML-Formular-Element enthält eine Bezeichnung sowie einem Eingabefeld. Sie hinzufügen, werden eine **für** -Attribut auf die Bezeichnung, die die Bindung an die Eingabe, und sehen Sie IntelliSense Vorschläge basierend auf den Ids der Eingaben im Gültigkeitsbereich.

1. Open **Visual Studio Express 2013 für Web** und die **Begin.sln** Projektmappe befindet sich in der **Quell-/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Anfang** Ordner. Alternativ können Sie die Projektmappe fortsetzen, dass Sie im vorherigen Schritt abgerufen haben.
2. In **Projektmappen-Explorer**öffnen die **Index.cshtml** -Datei die **Ansichten** | **Home** Ordner.
3. Hinzufügen der folgenden Form innerhalb der  **&lt;Abschnitt&gt;**  Element.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *Formular*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Eine Bezeichnung mit der Beschreibung des Felds sollte die Eingabetag vorangestellt werden. Fügen Sie der folgenden Beschriftung vor der Eingabetag hinzu.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Die **für** Attribut von einem  **&lt;Bezeichnung&gt;**  gibt an, welche Form-Elements eine Beschriftung an gebunden ist. Der Wert des Attributs sollte die Id des verknüpften Elements gleich sein. Hinzufügen der **für** -Attribut auf die  **&lt;Bezeichnung&gt;**  Element. Wie in der folgenden Abbildung gezeigt den &quot;Namen&quot; Wert eingeblendet, in das Feld IntelliSense anhand der Id der Elemente innerhalb desselben Gültigkeitsbereichs (einschließenden  **&lt;Formular&gt;**).

    ![Die Id in IntelliSense angezeigt](visual-studio-2013-web-tools/_static/image35.png "mit der Id in IntelliSense")

    *Die Id wird in IntelliSense angezeigt.*
6. Löschen Sie die zuletzt hinzugefügte  **&lt;Formular&gt;**  Element und dessen Inhalt.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Aufgabe 2 – mit HTML-Codeausschnitte

HTML5 wurden mehr als 25 neuen semantischen Tags eingeführt. Visual Studio hat bereits die IntelliSense-Unterstützung für diese Tags, aber Visual Studio 2013 ist es schneller und einfacher Markup zu schreiben, indem Sie neue Codeausschnitte hinzufügen. Obwohl diese Tags nicht kompliziert sind, kommen sie mit wenigen kleine Besonderheiten, z. B. das Hinzufügen der richtigen Codec Zugriffe für die *audio* Tag. In dieser Aufgabe wird die HTML-Codeausschnitte für das audio Tag angezeigt.

1. In der **Index.cshtml** Datei, geben Sie  **&lt;Aud** innerhalb der  **&lt;Abschnitt&gt;**  Element wie in der folgenden Abbildung dargestellt.

    ![Einfügen von audio-Element](visual-studio-2013-web-tools/_static/image36.png "Einfügen von audio-Element")

    *Einfügen von audio-element*
2. Drücken Sie **Registerkarte** zweimal und beachten Sie, wie im folgende Code auf der Seite hinzugefügt wurde und der Cursor befindet sich auf die **Src** Attribut der ersten Quelle.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Durch Drücken der **Registerkarte** Schlüssel und der Codeausschnitt eingefügt wird. Der audio-Codeausschnitt zeigt das Standardverfahren für die *audio* -Tag entspricht, durch zwei Quelldateien für verbesserte Unterstützung.
3. Die zweite Zeile löschen und aktualisieren Sie die Quelle der ersten Zeile mit dem folgenden Link anzuzeigende WebCampsTV Katana: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Der resultierende Code wird unten gezeigt.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Die Quelldatei *KatanaProject.mp3* dient als Beispiel. Falls gewünscht, können Sie eine andere Quelle.
4. Drücken Sie **STRG** + **S** zum Speichern der Datei.
5. Drücken Sie **STRG** + **F5** zum Starten der Anwendung.
6. Beachten Sie, dass die Anwendung ein Audioplayer hinzugefügt wurde.

    ![Audioplayer in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio-Player im Internet Explorer")

    *Audioplayer in Internet Explorer*

    ![Audio-Player im Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio-Player im Google Chrome")

    *Audio-Player im Google Chrome*
7. Schließen Sie den Browser nicht. Sie verwenden sie in der nächsten Aufgabe.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Aufgabe 3: Verwenden von IntelliSense in JavaScript-Dokumenten

Mit Web Essentials 2013 die HTML-Seiten und Stylesheets, erzeugen die einer Liste von IDs und Klassennamen. In dieser Aufgabe erfahren Sie, wie aufstellungen JavaScript-IntelliSense-Unterstützung im Web Essentials 2013 zu verbessern.

1. In der **Index.cshtml** Datei, fügen Sie folgenden Code zum Definieren einer **Skript** Tag für JavaScript-Code.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Fügen Sie den folgenden Code innerhalb der **Skript** Tag zum Definieren der Rückruffunktion bereit.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Einfügen des Caretzeichens in das **Skript** Tag, und drücken Sie **STRG** + **.** um den Vorschlag-Menü zu öffnen.
4. Klicken Sie auf **Datei extrahieren**.

    ![JavaScript extrahieren Datei Vorschlag](visual-studio-2013-web-tools/_static/image39.png "JavaScript in Datei Vorschlag extrahieren")

    *JavaScript in Datei Vorschlag extrahieren*
5. In der **speichern unter** wählen die **Skripts** Ordner, benennen Sie die Datei **init.js** , und klicken Sie auf **speichern**.

    ![Speichern als Fenster](visual-studio-2013-web-tools/_static/image40.png "Fenster Speichern unter")

    *Fenster Speichern unter*

    > [!NOTE]
    > Die **init.js** Datei wird erstellt und der Inhalt des Skripts wird in der Datei verschoben.
    > 
    > ![Init.js-Datei erstellt, mit dem Inhalt enthalten](visual-studio-2013-web-tools/_static/image41.png "Init.js-Datei erstellt, mit dem Inhalt enthalten")
    > 
    > *Erstellt mit dem Inhalt enthalten init.js-Datei*
6. Öffnen der **Index.cshtml** Datei, und überprüfen Sie, dass das Skript-Tag ersetzt wurde, mit einem Verweis auf die **init.js** Datei.

    ![Html-Verweis init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js HTML-Referenz")

    *Init.js HTML-Referenz*
7. Wechseln Sie zu der **Projektmappen-Explorer** und beachten Sie, dass die **init.js** Datei wurde in der Projektmappe automatisch eingeschlossen.

    ![Init.js-Datei, die in Projektmappen enthalten](visual-studio-2013-web-tools/_static/image43.png "Init.js-Datei, die in Projektmappen enthalten")

    *Init.js-Datei, die in Projektmappen enthalten*
8. Wechseln Sie zurück zu den **init.js** Datei beim Aktualisieren der **bereit** Callback-Funktion.
9. In der Funktionsdefinition für den Rückruf, der zum übergeben wird *bereit*, fügen Sie folgenden Code aus, um alle Elemente von einer bestimmten Klasse-Attribut zu erhalten.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Drücken Sie **STRG** + **Speicherplatz** zwischen den Anführungszeichen innerhalb der **GetElementsByClassName** Funktionsaufruf.

    ![Anzeigen von IntelliSense für die Funktion GetElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zeigt IntelliSense für die GetElementsByClassName-Funktion")

    *Anzeigen von IntelliSense für die GetElementsByClassName-Funktion*

    > [!NOTE]
    > Beachten Sie, dass IntelliSense die Klassen, die in die Projekt-Stylesheets definiert zeigt.
11. Ersetzen Sie die Zeile, die Sie mit der folgende Code erstellt haben.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Positionieren Sie den Cursor nach **Australien** innerhalb der Anführungszeichen in der **GetElementsByTagName** -Funktion, und drücken Sie **STRG** + **Speicherplatz**.

    ![Anzeigen von IntelliSense für die Methode GetElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zeigt IntelliSense für die GetElementByTagName-Methode")

    *Anzeigen von IntelliSense für die GetElementsByTagName-Methode*
13. Wählen Sie  **&quot;audio&quot;**  aus der Liste aus und drücken Sie **EINGABETASTE**. Das Ergebnis ist in der folgenden Abbildung dargestellt.

    ![Abrufen von Audio Elementen](visual-studio-2013-web-tools/_static/image46.png "Audio Elemente abrufen")

    *Abrufen von Audio-Elementen*
14. In **Projektmappen-Explorer**, mit der rechten Maustaste die **init.js** in der Datei die **Skripts** Ordner, und wählen **verkleinernde JavaScript-Dateien** aus der **Web Essentials** Menü.

    ![JavaScript-Dateien zu verkleinernde](visual-studio-2013-web-tools/_static/image47.png "verkleinernde JavaScript-Dateien")

    *Verkleinernde JavaScript-Dateien*
15. Bei der Aufforderung zum automatischen Minimierung zu aktivieren, wenn die Quelldatei geändert klicken Sie auf **Ja**.

    ![Aktivieren der automatischen Minimierung Warnung](visual-studio-2013-web-tools/_static/image48.png "automatische Minimierung Warnung aktivieren")

    *Aktivieren der automatischen Minimierung Warnung*

    > [!NOTE]
    > Die **init.min.js** wird erstellt und wird als eine Abhängigkeit hinzugefügt, die **init.js** Datei.
    > 
    > ![Erstellte init.Min.js Datei](visual-studio-2013-web-tools/_static/image49.png "Init.min.js-Datei erstellt wurde")
    > 
    > *Init.Min.js-Datei erstellt wurde*
16. Öffnen der **init.min.js** Datei, und beachten Sie, dass die Datei verkleinert wird.

    ![Dateiinhalt init.Min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js Dateiinhalt")

    *Dateiinhalt init.Min.js*
17. In der **init.js** Datei, fügen Sie den folgenden Code unter der **GetElementsByTagName** Funktionsaufruf die audio Elemente wiedergegeben.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Klicken Sie auf **STRG** + **S** zum Speichern der Datei. Da die verkleinerte Datei bereits geöffnet ist, sehen Sie einem Dialogfeld darauf hingewiesen, dass die Datei außerhalb der Quellen-Editor geändert wurde. Klicken Sie auf **Ja**.

    ![Microsoft Visual Studio-Warnung](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio-Warnung")

    *Microsoft Visual Studio-Warnung*
19. Wechseln Sie zurück zu den **init.min.js** Datei, um sicherzustellen, dass die Datei mit den neuen Code aktualisiert wurde.

    ![Init.Min.js-Datei aktualisiert](visual-studio-2013-web-tools/_static/image52.png "Init.min.js-Datei aktualisiert wird")

    *Init.Min.js-Datei aktualisiert wird*
20. Klicken Sie auf die **Browser Link aktualisieren** Schaltfläche.
21. Sobald beide Browser aktualisiert werden werden die audio-Playern, die Sie in der vorherigen Aufgabe gesehen haben mit unterschiedlichen Rollen automatisch gestartet.

    ![In der Sicht enthaltenen Audioplayer](visual-studio-2013-web-tools/_static/image53.png "Audio-Player, die in der Sicht enthalten")

    *In der Sicht enthaltenen Audioplayer*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Dieser praktischen Übungseinheit abschließen, indem Sie haben gelernt, wie Sie:

- Verwenden Sie neue HTML-Features im Editor im Web Essentials enthalten sind, z. B. umfangreiche HTML5-Codeausschnitte und Zen-Codierung
- Verwenden der neue CSS-Editor-Funktionen in Web Essentials enthalten sind, z. B. die Farbauswahl und ein Browser Matrix QuickInfo
- Verwenden Sie die neue JavaScript-Editor-Funktionen in Web Essentials z. B. "in der Datei und IntelliSense extrahieren" für alle HTML-Elemente enthalten
- Exchange-Daten zwischen dem Browser und Visual Studio mithilfe der Browserlink
