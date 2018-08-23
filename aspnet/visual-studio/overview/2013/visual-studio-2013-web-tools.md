---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Praxisnahe Übung: Visual Studio 2013-Webtools | Microsoft-Dokumentation'
author: rick-anderson
description: Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte. Es enthält einen leistungsfähige Text-Editor ein, der auf einfache Weise verwendet werden kann...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 82248efd767c1110b9a4067b7d0c0e2ecafcbef9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827437"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Praxisnahe Übung: Visual Studio 2013-Webtools
====================
durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](http://aka.ms/webcamps-training-kit)

> Visual Studio ist eine ausgezeichnete Entwicklungsumgebung für. NET-basierten Windows und Webprojekte. Sie enthält einen leistungsfähige Text-Editor ein, der leicht verwendet werden kann, um eigenständige Dateien ohne ein Projekt bearbeiten.
> 
> Visual Studio verwaltet eine voll funktionsfähige Analysestruktur an, wie Sie jede Datei bearbeiten. Dadurch können Visual Studio, um eine beispiellose automatische Vervollständigung und dokumentbasierte Aktionen bieten und gleichzeitig die Entwicklung schneller und problemloser. Diese Funktionen sind besonders interessant, in HTML und CSS-Dokumenten.
> 
> Alle von dieser Leistung steht auch für Erweiterungen, die Editoren mit leistungsfähigen neuen Features entsprechend Ihren Anforderungen erweitern erleichtert. Web Essentials ist eine Auflistung von (hauptsächlich) webbezogene Verbesserungen in Visual Studio. Er enthält viele neue IntelliSense-vervollständigungen (insbesondere bei CSS), neuen Features von Browser Link, automatische JSHint für JavaScript-Dateien, neue Warnungen für HTML, CSS und viele weitere Features, die die moderne Webentwicklung unverzichtbar sind.
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Übersicht

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Verwenden Sie die neuen HTML-Editor-Features in Web Essentials z. B. rich-HTML5-Codeausschnitte und Zen-Codierung
- Verwenden Sie die neue CSS-Editor-Funktionen, die im Web Essentials enthalten sind, z. B. die Farbauswahl angezeigt und die QuickInfo für Browser-matrix
- Verwenden Sie die neue JavaScript-Editor-Funktionen, die im Web Essentials, z. B. extrahieren in die Datei und IntelliSense für alle HTML-Elemente enthalten
- Austauschen von Daten zwischen Ihrem Browser, und Visual Studio verwenden einer Browserverknüpfung

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Folgendes ist erforderlich, um diese praktische Übungseinheit auszuführen:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) oder höher
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Setup

Um die Übungen in dieser praktischen Übungseinheit auszuführen, müssen Sie zunächst die Umgebung einrichten.

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zu des Labs die **Quelle** Ordner.
2. Mit der rechten Maustaste **"Setup.cmd"** , und wählen Sie **als Administrator ausführen** zum Starten des Setup-Prozesses, die die Umgebung konfigurieren, und installieren die Visual Studio-Codeausschnitte für diese Übung.
3. Wenn das Dialogfeld "Benutzerkontensteuerung" angezeigt wird, bestätigen Sie die Aktion aus, um den Vorgang fortzusetzen.

> [!NOTE]
> Stellen Sie sicher, dass Sie alle Abhängigkeiten für diese laborumgebung aktiviert haben, bevor Sie das Setup ausführen.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Verwenden von Codeausschnitten

In diesem Dokument Lab werden Sie aufgefordert, zum Einfügen von Codeblöcken. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitten, die Sie in Visual Studio 2013, um zu vermeiden, müssen sie manuell hinzufügen aus zugreifen können.

> [!NOTE]
> Jede Übung umfasst eine ab Lösung befindet sich in der **beginnen** Ordner der Übung, mit dem Sie jede Übung unabhängig von den anderen verfolgen kann. Bedenken Sie bitte, dass die Codeausschnitte, die während der Übung hinzugefügt werden fehlen aus diesen Lösungen ab und funktioniert möglicherweise nicht, bis Sie in dieser Übung abgeschlossen haben. In den Quellcode für eine Übung, finden Sie auch eine **End** Ordner, der Visual Studio-Projektmappe mit dem Code, die aus der Schritte in der entsprechenden Übung enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Verwenden von Browserlink und die Web Essentials](#Exercise1)
2. [Nutzung von Codeausschnitten und IntelliSense](#Exercise2)

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal starten, müssen Sie eine der vordefinierten Einstellungen Sammlungen auswählen. Jede vordefinierte Sammlung dient einem bestimmten Entwicklungsstil und bestimmt, Fensterlayouts, Editor-Verhalten, IntelliSense-Codeausschnitte und Dialogfeld "Optionen". In dieser Übung wird beschrieben, die erforderlichen Aktionen zum Ausführen der jeweiligen Aufgabe in Visual Studio bei Verwendung der **allgemeine Entwicklungseinstellungen** Auflistung. Wenn Sie eine Sammlung mit anderen Einstellungen für Ihre Entwicklungsumgebung auswählen, unter Umständen gibt es bestehen Unterschiede in den Schritten, die Sie berücksichtigen sollten.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Übung 1: Verwendung von Browserlink und die Web Essentials

**Web Essentials** ist eine Visual Studio-Erweiterung, die eine Vielzahl von nützlichen Features für die moderne Webentwicklung, vor allem darauf konzentriert, die die webentwicklungserfahrung viel schneller und problemloser hinzufügt. Sie können die Web Essentials aus dem Katalog der Erweiterung in Visual Studio installieren.

**Browserverknüpfung** ist ein neues Feature in Visual Studio 2013, die einen Kanal zwischen Visual Studio-IDE und jedem geöffneten Browser zum Austauschen von Daten zwischen der Webanwendung und die Visual Studio bietet enthalten. Web Essentials erweitert Browserlink mit Tools zum Bearbeiten des DOM-Objekt und die CSS-Formate von Webseiten direkt über den Browser.

In dieser Übung werden Sie lernen Sie einige der vom unterstützten Funktionen **Web Essentials** und **Browserlink** um eine einfache Quiz-Seite zu verbessern.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Aufgabe 1: Ausführen des Projekts in mehreren Browsern

In dieser Aufgabe Konfigurieren Sie Ihre Webanwendung in mehreren Browsern gleichzeitig ausführen Dies ist hilfreich für browserübergreifende Tests.

1. Open **Microsoft Visual Studio**.
2. In der **Datei** , wählen Sie im Menü **Open | Projekt/Projektmappe...**  und navigieren Sie zu **Ex1-WorkingwithBrowserLinkandWebEssentials\Begin** in die **Quelle** Ordner der Umgebung (C:\WebCampsTK\HOL\VSWebTooling\Source). Wählen Sie **Begin.sln** , und klicken Sie auf **öffnen**.
3. Erweitern Sie in der Symbolleiste von Visual Studio das Menü "Browser"- **Browserauswahl...** .

    ![Navigieren Sie mit der Menüoption](visual-studio-2013-web-tools/_static/image1.png "durchsuchen mit... im \"Objektkatalog\"")

    *Navigieren Sie mit der Menüoption*
4. In der **Browserauswahl** (Dialogfeld), wählen Sie **Google Chrome** und **Internet Explorer** halten die **STRG** gedrückt, und klicken Sie auf  **Als Standard festlegen**.

    ![Navigieren Sie im Dialogfeld](visual-studio-2013-web-tools/_static/image2.png "navigieren Sie im Dialogfeld")

    *Mehrere Standardbrowser auswählen*
5. Sowohl für Google Chrome als auch für Internet Explorer sollte nun wie der Standardbrowser angezeigt werden. Klicken Sie auf **Abbrechen** um das Dialogfeld zu schließen.

    ![Google Chrome und Internet Explorer als Standardbrowser](visual-studio-2013-web-tools/_static/image3.png "Google Chrome und Internet Explorer Standardbrowser")

    *Google Chrome und Internet Explorer als Standardbrowser*

    > [!NOTE]
    > Nach dem Konfigurieren der Standardbrowser der **mehreren Browsern** Option im Menü "Browser" ausgewählt ist.
    > 
    > ![Mehrere Browser](visual-studio-2013-web-tools/_static/image4.png "mehrere Browser")
6. Drücken Sie **STRG** + **F5** um die Anwendung ohne Debuggen auszuführen.
7. Wenn beide Browserfenster öffnen, müssen platzieren Sie eine von ihnen über die anderen, um die Updates gleichzeitig auf beide Browser anzuzeigen. Der Browser sollte eine Webseite mit einem hellblaue Rechteck angezeigt werden.

    ![Platzieren einen Browser über der anderen](visual-studio-2013-web-tools/_static/image5.png "Platzieren von einem Browser über der anderen")

    *Platzieren einen Browser über der anderen*
8. Schließen Sie den Browser nicht. Sie verwenden diese in der nächsten Aufgabe.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Aufgabe 2: Verwendung Zen-Codierung zum Erstellen von HTML-Elementen

**Codieren von Zen** ist Sie ein Editor-Plug-In für Hochgeschwindigkeits-HTML, XML, XSL-(oder jedes andere strukturiertem Code-Format) zu programmieren und zu bearbeiten. Der Kern der dieses Plug-in ist eine leistungsstarke Abkürzung-Engine – ähnlich wie CSS-Selektoren - Ausdrücke, in HTML-Code erweitern können. Zen-Codierung ist eine schnelle Möglichkeit zum Schreiben, dass HTML mithilfe einer CSS-Selektor-Syntax formatieren.

In dieser Übung verwenden Sie die Codierung Zen-Funktion von Web Essentials bereitgestellt, die HTML-Schaltflächen generieren, die die Optionen der Frage darstellt.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der **"Index.cshtml"** Datei befindet sich in der **Ansichten** | **Startseite** Ordner.
3. Ersetzen der **&lt;!--TODO: Fügen Sie die Optionen zur Verfügung –&gt;** Kommentar mit dem folgenden Code, und drücken Sie **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. Der Code sollte in HTML erweitert werden.

    ![Erweitert HTML](visual-studio-2013-web-tools/_static/image6.png "erweitert HTML")

    *Erweiterte HTML*

    > [!NOTE]
    > Weitere Informationen zur Codierung von Zen-Syntax finden Sie unter den folgenden [Artikel](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Klicken Sie auf die **verknüpfte Browser aktualisieren** Schaltfläche, um beide Browser zu aktualisieren.

    ![Verknüpfte Browser aktualisieren](visual-studio-2013-web-tools/_static/image7.png "verknüpfte Browser aktualisieren")

    *Verknüpfte Browser aktualisieren*

    ![InternetExplorer - Seite aktualisiert, mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image8.png "InternetExplorer - Seite aktualisiert, mit vier Schaltflächen")

    *InternetExplorer - Seite mit vier Schaltflächen aktualisiert*

    ![Google Chrome – Seite aktualisiert, mit vier Schaltflächen](visual-studio-2013-web-tools/_static/image9.png "Google Chrome – Seite aktualisiert, mit vier Schaltflächen")

    *Google Chrome – aktualisiert die Seite mit vier Schaltflächen*
6. Wechseln Sie zurück zu Visual Studio.
7. Sie haben die Schaltflächen auf der Seite hinzugefügt, jedoch müssen Sie immer noch eine Frage der Vorlage hinzufügen. Zu diesem Zweck verwenden Sie ein neues Feature in Web Essentials namens **Lorem Ipsum Generator**. Suchen Sie die **Div** -Element mit dem **Klasse** Attribut **Front**.
8. Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div**, und drücken Sie die **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. Der Code sollte in HTML erweitert werden.

    ![Lorem Ipsum automatisch generierte](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum automatisch generiert")

    *Lorem Ipsum automatisch generiert*

    > [!NOTE]
    > Im Rahmen der Zen-Codierung können Sie jetzt Lorem Ipsum-Code direkt im HTML-Editor generieren. Geben Sie einfach **Lorem** und klicken Sie auf **Registerkarte** und eine 30 Lorem Ipsum Text eingefügt werden soll. Beispiel: *lorem10* fügt 10 Lorem Ipsum-Wörter.
10. Fügen Sie ein Logo am oberen Rand der Frage ein weiteres neues Feature in Web Essentials aufgerufen mit **Lorem Pixel Generator**. Fügen Sie den folgenden Code als das erste untergeordnete Element von der **Div** -Element mit **Container** als **Klasse** Wert ein, und drücken Sie **Registerkarte**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. Der Code sollte in HTML erweitern.

    ![Lorem Pixel automatisch generierte](visual-studio-2013-web-tools/_static/image11.png "Lorem Pixel automatisch generiert")

    *Lorem Pixel automatisch generiert*

    > [!NOTE]
    > Im Rahmen der Zen-Codierung können Sie auch Lorem-Pixel-Code direkt im HTML-Editor generieren. Geben Sie einfach **Pix-200 x 200-Tiere** und klicken Sie auf **Registerkarte** und **Img** Tag mit einem 200 x 200-Image von einem Tier eingefügt werden soll. Weitere Informationen finden Sie unter [Lorem Pixel](http://www.lorempixel.com).
12. Klicken Sie auf die **verknüpfte Browser aktualisieren** Schaltfläche, um beide Browser zu aktualisieren.

    ![InternetExplorer – automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer – automatisch generierte Bild und Text")

    *InternetExplorer – automatisch generierte Bild und text*

    ![Google Chrome – automatisch generierte Bild und Text](visual-studio-2013-web-tools/_static/image13.png "Google Chrome – automatisch generierte Bild und Text")

    *Google Chrome – automatisch generierte Bild und text*

    > [!NOTE]
    > Da das Bild nach dem Zufallsprinzip ausgewählt ist, wenn Sie den Codeausschnitt hinzufügen, das Bild in den Browsern unterscheiden sich möglicherweise.
13. Schließen Sie den Browser nicht. Sie verwenden diese in der nächsten Aufgabe.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Aufgabe 3: Aktualisieren einer Style-Eigenschaft

In dieser Aufgabe verwenden Sie den Browserlink **Modus überprüfen** Feature zum Erkennen des exakten Speicherort, an dem das bestimmte DOM-Element generiert wird, und aktualisieren Sie dann die Color-Eigenschaft dieses Elements, das über eine Farbauswahl angezeigt, die vom Webdienst bereitgestellt Essentials.

1. Drücken Sie in der Internet Explorer-Browser **STRG** + **ALT** + **ich** Aktivieren des Modus zu überprüfen.
2. Zeigen Sie auf den hellen blauen Rahmen, und klicken Sie auf.

    ![Bewegen des Mauszeigers über den hellen blauen Rahmen](visual-studio-2013-web-tools/_static/image14.png "Bewegen des Mauszeigers über den hellen blauen Rahmen")

    *Bewegen des Mauszeigers über den hellen blauen Rahmen*
3. Wechseln Sie zurück zu Visual Studio. Beachten Sie, wie das HTML-Element, das Sie im Browser ausgewählt ebenfalls in der Visual Studio-HTML-Editor aktiviert ist.

    ![HTML-Element in der Visual Studio-HTML-Editor ausgewählten](visual-studio-2013-web-tools/_static/image15.png "HTML-Element in der Visual Studio-HTML-Editor ausgewählt")

    *HTML-Element in der Visual Studio-HTML-Editor ausgewählt*
4. Aktualisieren Sie jetzt die **Front** CSS-Klasse, um den Stil des ausgewählten Elements zu ändern. Drücken Sie die zu diesem Zweck **STRG** + **,** zum Öffnen der **Navigieren zu** Suchfeld. Typ **"Site.CSS"** , und drücken Sie **EINGABETASTE** zum Öffnen der Datei.

    ![Öffnen die Datei "Site.CSS"](visual-studio-2013-web-tools/_static/image16.png "Öffnen der Datei \"Site.CSS\"")

    *Öffnen der Datei "Site.CSS"*
5. Drücken Sie **STRG** + **F** und **.flip-Containter .front** die CSS-Auswahl gefunden.
6. Klicken Sie auf das Licht blaue Quadrat aus, in die Border-Eigenschaft der Klasse, um den Farbwähler zu öffnen.

    ![Öffnen die Farbauswahl](visual-studio-2013-web-tools/_static/image17.png "öffnen die Farbauswahl")

    *Öffnen die Farbauswahl*
7. Erweitern Sie die Farbauswahl, indem Sie auf die Chevron-Schaltfläche, und wählen Sie eine neue Farbe.

    ![Erweitern die Farbauswahl](visual-studio-2013-web-tools/_static/image18.png "die Farbauswahl erweitern")

    *Die Farbauswahl erweitern*
8. Drücken Sie **STRG** + **ALT** + **EINGABETASTE** verknüpften Browser zu aktualisieren.
9. Wechseln Sie zu Internet Explorer, und beachten Sie, wie sich die Farbe des Rahmens geändert hat.

    ![InternetExplorer – die Farbe des Rahmens aktualisiert](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer – die Farbe des Rahmens aktualisiert")

    *InternetExplorer – die Farbe des Rahmens aktualisiert*
10. Wechseln Sie zur Google Chrome, und beachten Sie, wie sich die Farbe des Rahmens geändert hat.

    ![Google Chrome – die Farbe des Rahmens aktualisiert](visual-studio-2013-web-tools/_static/image20.png "Google Chrome – die Farbe des Rahmens aktualisiert")

    *Google Chrome – die Farbe des Rahmens aktualisiert*
11. Wechseln Sie zurück zu Visual Studio.
12. Wechseln Sie an das Ende der **"Site.CSS"** -Datei, und drücken Sie **STRG** + **F** finden die **.btn** Selektor.
13. Beachten Sie, dass die **- Webkit-Rahmen-Radius** Eigenschaft Grün unterstrichen ist.

    ![-Webkit-Rahmen-Radius-Eigenschaft des Selektors Btn](visual-studio-2013-web-tools/_static/image21.png "- Webkit-Rahmen-Radius-Eigenschaft des Selektors Btn")

    *-Webkit-Rahmen-Radius-Eigenschaft des Selektors btn*
14. Platzieren Sie den Textcursor in den **- Webkit-Rahmen-Radius** Eigenschaft. Eine blaue Linie sollte unter dem ersten Buchstaben des ersten Worts der Eigenschaft angezeigt werden. Dies ist die **Smarttag**.
15. Drücken Sie **STRG** + **.** Öffnen Sie das Menü Vorschläge, und klicken Sie auf **hinzufügen fehlender Standardeigenschaft (Border-Radius)**.

    ![Hinzufügen fehlender Standardeigenschaft Vorschlag](visual-studio-2013-web-tools/_static/image22.png "hinzufügen fehlender Standardeigenschaft Vorschlag")

    *Fügen Sie fehlender Standardeigenschaft Vorschlag hinzu*
16. Die **Rahmen-Radius** Eigenschaft wird die CSS-Regel automatisch hinzugefügt.

    ![Fehlende Standardeigenschaft hinzugefügt](visual-studio-2013-web-tools/_static/image23.png "fehlt die standard-Eigenschaft hinzugefügt")

    *Fehlende Standardeigenschaft hinzugefügt*
17. Zeigen Sie auf die **Rahmen-Radius** Eigenschaft zum Anzeigen der **Browser Matrix QuickInfo**. Die **Browser Matrix QuickInfo** zeigt die Verfügbarkeit der Eigenschaft in jedem Browser.

    ![Browser Matrix QuickInfo](visual-studio-2013-web-tools/_static/image24.png "Browser Matrix QuickInfo")

    *Browser-Matrix-QuickInfo*
18. Beachten Sie, dass der Wert des der **Rahmen-Radius** -Eigenschaft ist immer noch unterstrichen. Zeigen Sie auf den Wert in der Warnmeldung angezeigt.

    ![Border-Radius-Eigenschaft Warnung](visual-studio-2013-web-tools/_static/image25.png "Rahmen-Radius-Eigenschaft-Wert-Warnung")

    *Warnung Rahmen-Radius-Eigenschaft*
19. Entfernen Sie die Einheit für die **Rahmen-Radius** Eigenschaftswert gemäß dem Vorschlag von der QuickInfo.
20. Als **Rahmen-Radius** ist die standard-Eigenschaft für die Definition wie abgerundeten Rahmen, die Ecken sind, können Sie entfernen die **- Webkit-Rahmen-Radius** -Eigenschaft und Wert in der CSS-Regel.
21. Platzieren Sie den Textcursor in den **Zeilenumbruch** -Eigenschaft und das Smarttag auch unten angezeigt.
22. Öffnen Sie das Menü, und klicken Sie auf **hinzufügen fehlender Besonderheiten der jeweiligen Hersteller**.

    ![Hinzufügen fehlender Hersteller Besonderheiten Vorschlag](visual-studio-2013-web-tools/_static/image26.png "fehlenden Anbieter Besonderheiten der jeweiligen Empfehlung hinzufügen")

    *Fügen Sie fehlender Hersteller Besonderheiten Vorschlag hinzu*
23. Die **-ms-Zeilenumbruch** Eigenschaft wird die CSS-Regel automatisch hinzugefügt.

    ![Hersteller (Eigenschaft) bestimmte hinzugefügt](visual-studio-2013-web-tools/_static/image27.png "bestimmten Hersteller (Eigenschaft) hinzugefügt")

    *Bestimmten Hersteller (Eigenschaft) hinzugefügt*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Aufgabe 4: Aktualisieren der HTML-Code aus dem Browser

In dieser Aufgabe verwenden Sie den Browserlink **Entwurfsmodus** Funktion bearbeiten das DOM-Objekt, aus dem Browser, und übertragen die Änderungen an der HTML-Quelldatei in Visual Studio.

1. Drücken Sie in Google Chrome, **STRG** + **ALT** + **D** Entwurfsmodus aktivieren.
2. Zeigen Sie auf die **Lorem Ipsum Dolor Sit Amet** bezeichnen, und klicken Sie auf.

    ![Bearbeiten die Frage](visual-studio-2013-web-tools/_static/image28.png "die Frage bearbeiten")

    *Die Frage bearbeiten*
3. Ein Cursor sollte angezeigt werden. Ersetzen Sie den ursprünglichen Text mit *Was sieht, wenn eine Frage mehr geschrieben?*, und drücken Sie dann **ESC** zum Entwurfsmodus zu beenden.

    ![Frage bearbeitet](visual-studio-2013-web-tools/_static/image29.png "Frage bearbeitet")

    *Frage bearbeitet*
4. Wechseln Sie zurück zu Visual Studio und öffnen **"Index.cshtml"**, sofern nicht bereits geöffnet. Beachten Sie, dass der innere Text des der **&lt;p&gt;** Element aktualisiert wurde.

    ![Aktualisierte Frage im HTML-Seite](visual-studio-2013-web-tools/_static/image30.png "aktualisierte Frage im HTML-Seite")

    *Aktualisierte Frage im HTML-Seite*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Aufgabe 5: Überprüfung SEO-bezogene Warnungen

**Suchmaschinenoptimierung** (SEO) ist der Prozess der höheren Rang Website auf eine Such-Engine-Liste mit Ergebnissen vornehmen. Je höher die Rangfolge bestimmt der Website und je konsistenter wird aufgeführt, die mehr Besucher die Website erhalten aus dieser Such-Engine. Web Essentials enthält, in der HTML untersucht Instrument, Berichte, die Probleme gefunden, und bietet Unterstützung zu erhalten, um sie zu beheben.

1. Wechseln Sie zu der **Ansicht** Menü, und klicken Sie auf **Fehlerliste** zum Öffnen der **Fehlerliste** Fenster.

    ![In der Ansicht Fehlerliste Menü](visual-studio-2013-web-tools/_static/image31.png "Fehlerliste im Menü \"Ansicht\"")

    *Fehler in der Ansicht im Menü aufgeführt.*
2. Beachten Sie, dass es eine SEO-Warnung benachrichtigt, die eine **&lt;Meta&gt;** tag für die Beschreibung für die Seite fehlt. Doppelklicken Sie auf den SEO-Warnung-Eintrag, um das Problem zu beheben.

    ![Fehlerliste (Fenster)](visual-studio-2013-web-tools/_static/image32.png "Fenster \"Fehlerliste\"")

    *Fehlerliste (Fenster)*
3. In der **Web Essentials** Dialogfeld klicken Sie auf **Ja** , fügen Sie eine Beschreibung &lt;Meta&gt; Tag.

    ![Web Essentials-Dialogfeld](visual-studio-2013-web-tools/_static/image33.png "Web Essentials-Dialogfeld")

    *Web Essentials-Dialogfeld*
4. Der Editor für  **\_Layout.cshtml** wird geöffnet und die **&lt;Meta&gt;** Tag wird automatisch hinzugefügt, die **Head** Teil der HTML-Datei.

    ![Meta-Tag, die automatisch auf _Layout hinzugefügt](visual-studio-2013-web-tools/_static/image34.png "Meta-Tag _Layout Seite automatisch hinzugefügt.")

    *Meta-Tag automatisch hinzugefügt, \_Layoutseite*
5. Ändern Sie den Wert, der die **Inhalt** Attribut *GeekQuiz* und speichern Sie die Datei.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Übung 2: Nutzen von Codeausschnitten und IntelliSense

Mit Web Essentials wurde der HTML-Editor mit zusätzlicher Funktionalität erweitert. In dieser Übung sehen Sie einige neue Features, die Sie bei der Entwicklung von Webanwendungen.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Aufgabe 1: Verwenden von IntelliSense in HTML-Dokumente

Das erste neue Feature wird Ihnen in dieser Aufgabe wird aufgerufen, **dynamische IntelliSense**. Dynamische IntelliSense liest andere Tags und Attribute, die möglichen Ids abzuleiten, die Sie verwenden möchten.

In dieser Aufgabe erstellen Sie ein neues HTML-Formular-Element enthält eine Bezeichnung und einem Eingabefeld ein. Fügen Sie hinzu eine **für** -Attribut auf die Bezeichnung, die die Bindung an die Eingabe, und Sie sehen die IntelliSense-Vorschläge basierend auf die Ids der Eingaben im Bereich.

1. Open **Visual Studio Express 2013 für Web** und **Begin.sln** Lösung befindet sich in der **Quelle/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Anfang** Ordner. Alternativ können Sie die Lösung weiter nutzen, den Sie in der vorherigen Übung haben.
2. In **Projektmappen-Explorer**öffnen die **"Index.cshtml"** Datei befindet sich in der **Ansichten** | **Startseite** Ordner.
3. Hinzufügen der folgenden Form innerhalb der **&lt;Abschnitt&gt;** Element.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *Formular*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. Das input-Tag sollte eine Bezeichnung mit dem eine Beschreibung des Felds vorangestellt werden. Fügen Sie die folgende Bezeichnung vor den input-Tag hinzu.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. Die **für** Attribut eine **&lt;Bezeichnung&gt;** gibt an, an welches Formularelement eine Beschriftung gebunden ist. Der Wert des Attributs muss gleich der Id des verknüpften Elements. Hinzufügen der **für** -Attribut auf die **&lt;Bezeichnung&gt;** Element. Wie in der folgenden Abbildung dargestellt die &quot;Namen&quot; Wert eingeblendet wird, klicken Sie im IntelliSense basierend auf der Id der Elemente innerhalb des gleichen Bereichs (die einschließende  **&lt;Formular&gt;**).

    ![Die Id in IntelliSense angezeigt](visual-studio-2013-web-tools/_static/image35.png "die Id wird in IntelliSense angezeigt.")

    *Die Id wird in IntelliSense angezeigt.*
6. Löschen Sie die zuletzt hinzugefügte **&lt;Formular&gt;** Element und dessen Inhalt.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Aufgabe 2: mithilfe von HTML-Codeausschnitten

HTML5 wurden mehr als 25 neue semantische Tags eingeführt. Visual Studio war bereits ein IntelliSense-Unterstützung für diese Tags, aber Visual Studio 2013 können sie schneller und einfacher, Markup durch das Hinzufügen neuer Codeausschnitte zu schreiben. Obwohl diese Tags nicht komplizierter sind, sind sie auch mit ein paar feinheiten auf, z. B. das Hinzufügen der korrekten Codec-Fallbacks für die *audio* Tag. In dieser Aufgabe wird die HTML-Codeausschnitte für das audio Tag angezeigt.

1. In der **"Index.cshtml"** Geben Sie eine Datei  **&lt;Aud** innerhalb der **&lt;Abschnitt&gt;** -Element wie in der folgenden Abbildung gezeigt.

    ![Ein audio-Element einfügen](visual-studio-2013-web-tools/_static/image36.png "ein audio-Element einfügen")

    *Ein audio-Element einfügen*
2. Drücken Sie **Registerkarte** zweimal und beachten Sie, wie im folgende Code auf der Seite hinzugefügt wird, und der Cursor befindet sich auf die **Src** Attribut der ersten Quelle.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Durch Drücken der **Registerkarte** Schlüssel und der Codeausschnitt eingefügt wird. Der audio-Codeausschnitt zeigt die Standardverwendung von der *audio* -Tag mit zwei Quelldateien für die verbesserte Unterstützung.
3. Die zweite Zeile löschen und aktualisieren Sie die Datenquelle der ersten Zeile mit den folgenden Link in der WebCampsTV Katana-Show: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). Der resultierende Code ist unten dargestellt.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > Die Quelldatei *KatanaProject.mp3* als Beispiel verwendet wird. Falls gewünscht, können Sie eine andere Quelle.
4. Drücken Sie **STRG** + **S** zum Speichern der Datei.
5. Drücken Sie **STRG** + **F5** zum Starten der Anwendung.
6. Beachten Sie, dass die Anwendung ein Audioplayer hinzugefügt wurde.

    ![Audio-Player in Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Audio-Player in Internet Explorer")

    *Audio-Player in Internet Explorer*

    ![Audioplayer in Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Audio-Player im Google Chrome")

    *Audioplayer in Google Chrome*
7. Schließen Sie den Browser nicht. Sie verwenden diese in der nächsten Aufgabe.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Aufgabe 3: Verwenden von IntelliSense in JavaScript-Dokumenten

Web Essentials 2013 wird eine Liste von IDs und Klassennamen Stylesheets und HTML-Seiten erstellen. In dieser Aufgabe lernen Sie, wie diese Listen mit JavaScript-IntelliSense-Unterstützung in Web Essentials 2013 zu verbessern.

1. In der **"Index.cshtml"** Datei, fügen Sie folgenden Code zum Definieren einer **Skript** Tag für JavaScript-Code.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Fügen Sie den folgenden Code innerhalb der **Skript** Tag, um den bereit-Abruffunktion definieren.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Platzieren Sie den Textcursor in den **Skript** Tag, und drücken Sie **STRG** + **.** um das Menü "Vorschlag" zu öffnen.
4. Klicken Sie auf **extrahieren Sie in der Datei**.

    ![Extrahieren Sie JavaScript in Datei-Vorschlag](visual-studio-2013-web-tools/_static/image39.png "JavaScript zu extrahieren, in der Datei Vorschlag")

    *Extrahieren Sie JavaScript in Datei-Vorschlag*
5. In der **speichern** wählen Sie im Fenster der **Skripts** Ordner, benennen Sie die Datei **init.js** , und klicken Sie auf **speichern**.

    ![Fenster "Speichern unter"](visual-studio-2013-web-tools/_static/image40.png "Fenster Speichern unter")

    *Fenster "Speichern unter"*

    > [!NOTE]
    > Die **init.js** Datei wird erstellt und der Inhalt des Skripts wird in der Datei verschoben.
    > 
    > ![Init.js-Datei mit den entsprechenden Inhalt erstellt](visual-studio-2013-web-tools/_static/image41.png "Init.js-Datei, die mit den entsprechenden Inhalt erstellt wurde")
    > 
    > *Init.js-Datei, die mit den entsprechenden Inhalt erstellt wurde*
6. Öffnen der **"Index.cshtml"** Datei, und überprüfen Sie, dass das Skripttag ersetzt wurde, mit einem Verweis auf die **init.js** Datei.

    ![Die HTML-Verweis init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js HTML-Referenz")

    *Init.js HTML-Referenz*
7. Wechseln Sie zu der **Projektmappen-Explorer** und beachten Sie, dass die **init.js** Datei automatisch in der Projektmappe enthalten war.

    ![Init.js-Datei, die in der Lösung enthaltenen](visual-studio-2013-web-tools/_static/image43.png "Init.js-Datei, die in Projektmappen enthalten")

    *Init.js-Datei, die in Projektmappen enthalten*
8. Wechseln Sie zurück zu der **init.js** Datei zum Aktualisieren der **bereit** Callback-Funktion.
9. In der Definition der Funktion-Rückruf, der an übergebene *bereit*, fügen Sie den folgenden Code rufen Sie alle Elemente durch eine bestimmte Klasse-Attribut.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Drücken Sie **STRG** + **Speicherplatz** zwischen den Anführungszeichen in der **GetElementsByClassName** Funktionsaufruf.

    ![Anzeigen der IntelliSense für die Funktion GetElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "zeigt IntelliSense für die GetElementsByClassName-Funktion")

    *Anzeigen der IntelliSense für die GetElementsByClassName-Funktion*

    > [!NOTE]
    > Beachten Sie, IntelliSense die Klassen, die in der Projekt-Stylesheets definiert zeigt.
11. Ersetzen Sie die Zeile, die Sie mit der folgende Code erstellt haben.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Positionieren Sie den Cursor nach **au** in Anführungszeichen in der **GetElementsByTagName** -Funktion, und drücken Sie **STRG** + **Speicherplatz**.

    ![Anzeigen der IntelliSense für die Methode GetElementByTagName](visual-studio-2013-web-tools/_static/image45.png "zeigt IntelliSense für die GetElementByTagName-Methode")

    *Anzeigen der IntelliSense für die GetElementsByTagName-Methode*
13. Wählen Sie **&quot;audio&quot;** aus der Liste und drücken Sie **EINGABETASTE**. Das Ergebnis ist in der folgenden Abbildung dargestellt.

    ![Abrufen von Audio Elementen](visual-studio-2013-web-tools/_static/image46.png "Audio Elemente abrufen")

    *Abrufen von Audio-Elementen*
14. In **Projektmappen-Explorer**, mit der rechten Maustaste die **init.js** Datei die **Skripts** Ordner, und wählen **minimieren JavaScript-Dateien** aus der **Web Essentials** Menü.

    ![JavaScript-Dateien zu minimieren](visual-studio-2013-web-tools/_static/image47.png "minimieren JavaScript-Dateien")

    *Minimieren Sie die JavaScript-Dateien*
15. Wenn Sie aufgefordert werden, um automatische Minimierung zu aktivieren, wenn der Quelldatei geändert, klicken Sie auf **Ja**.

    ![Aktivieren automatische Minimierung Warnung](visual-studio-2013-web-tools/_static/image48.png "automatische Minimierung Warnung aktivieren")

    *Aktivieren automatische Minimierung-Warnung*

    > [!NOTE]
    > Die **init.min.js** wird erstellt und wurde als Abhängigkeit von der **init.js** Datei.
    > 
    > ![Erstellte init.Min.js Datei](visual-studio-2013-web-tools/_static/image49.png "Init.min.js-Datei erstellt wurde")
    > 
    > *Init.Min.js-Datei erstellt wurde*
16. Öffnen der **init.min.js** Datei, und beachten Sie, dass die Datei verkleinert wird.

    ![Dateiinhalt init.Min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js Dateiinhalt")

    *Dateiinhalt init.Min.js*
17. In der **init.js** Datei, fügen Sie den folgenden Code unter der **GetElementsByTagName** Funktionsaufruf alle Elemente der audio wiedergegeben.

    (Codeausschnitt - *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Klicken Sie auf **STRG** + **S** zum Speichern der Datei. Da die minimierte Datei bereits geöffnet ist, sehen Sie einem Dialogfeld darauf hingewiesen, dass die Datei außerhalb des Quellcode-Editors geändert wurde. Klicken Sie auf **Ja**.

    ![Microsoft Visual Studio-Warnung](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio-Warnung")

    *Microsoft Visual Studio-Warnung*
19. Wechseln Sie zurück zu den **init.min.js** Datei, um sicherzustellen, dass die Datei mit den neuen Code aktualisiert wurde.

    ![Init.Min.js-Datei aktualisiert](visual-studio-2013-web-tools/_static/image52.png "Init.min.js-Datei aktualisiert")

    *Init.Min.js-Datei aktualisiert*
20. Klicken Sie auf die **Browser Link aktualisieren** Schaltfläche.
21. Sobald Sie beide Browser aktualisiert werden, werden automatisch Wiedergabe der Audioplayer, die Sie in der vorherigen Aufgabe haben gesehen, gestartet.

    ![In der Ansicht enthaltenen Audioplayer](visual-studio-2013-web-tools/_static/image53.png "Audio-Player, die in der Ansicht enthalten")

    *In der Ansicht enthaltenen Audioplayer*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Nach Abschluss dieser praktischen Übungseinheit Sie haben gelernt, wie Sie:

- Verwenden Sie die neuen HTML-Editor-Features in Web Essentials z. B. rich-HTML5-Codeausschnitte und Zen-Codierung
- Verwenden Sie die neue CSS-Editor-Funktionen, die im Web Essentials enthalten sind, z. B. die Farbauswahl angezeigt und die QuickInfo für Browser-matrix
- Verwenden Sie die neue JavaScript-Editor-Funktionen, die im Web Essentials, z. B. extrahieren in die Datei und IntelliSense für alle HTML-Elemente enthalten
- Austauschen von Daten zwischen Ihrem Browser, und Visual Studio verwenden einer Browserverknüpfung
