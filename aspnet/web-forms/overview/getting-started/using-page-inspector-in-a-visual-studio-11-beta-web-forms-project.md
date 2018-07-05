---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Verwenden der Seitenprüfung für Visual Studio 2012 in der ASP.NET Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Auswählen eines Elements in der integrierten Browser, und klicken Sie auf der Seitenprüfung...
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 8e914b87305fa729659822ec1166e9d1947e59cb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806273"
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Verwenden der Seitenprüfung für Visual Studio 2012 in ASP.NET Web Forms
====================
von Tim Ammann

> Seitenprüfung für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Auswählen eines Elements in der integrierten Browser und der Seitenprüfung sofort hervorgehoben, Quell- und CSS-des-Elements. Sie können einer beliebigen Seite in Ihrer Anwendung navigieren, finden die Quellen der gerenderten Markup, und schnell Browsertools direkt in Visual Studio-Umgebung verwenden.
> 
> Dieses Tutorial Shwos wie Überprüfungsmodus zu aktivieren und schnell suchen und Bearbeiten von CSS-Regeln und Text in Ihrer Web-Projekt. Das Tutorial verwendet ein Web Forms-Anwendungsprojekt, aber Sie können auch die Seitenprüfung für Websiteprojekte und [MVC](https://go.microsoft.com/?linkid=9802002) Anwendungen.
> 
> Das Tutorial enthält die folgenden Abschnitte:
> 
> [Erforderliche Komponenten](#_1_prerequisites)
> 
> [Erstellen einer Webanwendung](#_2_creating_a)
> 
> [Verwenden Sie die Seitenprüfung, um die Anwendung anzuzeigen](#_3_using_page)
> 
> [Überprüfungsmodus zu aktivieren](#_4_inspection_mode)
> 
> [Verwenden der Seitenprüfung Markup ändern](#_5_using_page)
> 
> [Überprüfungsmodus und HTML-Fenster](#_6_inspection_mode)
> 
> [Vorschau der CSS-Änderungen im Fenster Stile](#_7_previewing_css)
> 
> [Automatische CSS-Synchronisierung](#css_auto_sync)
> 
> [Mithilfe der CSS-Farbauswahl](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Rufen Sie die neueste Version der Seitenprüfung mit [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) auf das Azure SDK für .NET 2.0 installiert.


Die Seitenprüfung ist Microsoft Web Developer Tools enthalten. Die neueste Version ist 1.3. Um die Version haben Sie, Visual Studio ausführen, und wählen Sie **Info zu Microsoft Visual Studio** aus der **Hilfe** Menü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Erstellen einer Webanwendung

Zunächst erstellen Sie eine Webanwendung, der Seitenprüfung mit verwendet werden. Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**. Erweitern Sie auf der linken Seite **Visual C#-** Option **Web**, und wählen Sie dann **ASP.NET Web Forms-Anwendung**.

![Neue Web Forms-Anwendung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Klicken Sie auf **OK**.

Öffnet die Anwendung in **Quelle** anzeigen.

![Neue Web Forms-Anwendung in der Quellansicht](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Nun, da Sie eine Anwendung gearbeitet haben, können Sie der Seitenprüfung verwenden, überprüfen und ändern Sie sie.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Verwenden Sie die Seitenprüfung, um die Anwendung anzuzeigen

Als Nächstes zeigen Sie die Anwendung mit der Seitenprüfung an. In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **in Seitenprüfung anzeigen**.

![In Seitenprüfung anzeigen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

In der Standardeinstellung ist Page Inspector zum ersten Mal gestartet wird. er als schmalen Fenster auf der linken Seite der Visual Studio-Umgebung angedockt. Lassen Sie auf der linken Seite angedockt, und legen Sie ihn auf eine Breite, die Ihnen vertraut ist, oder es in einer der Bereiche Tool für die oben, unten oder rechts Andocken:

![Die Seitenprüfung docking-Positionen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Wenn Sie das Fenster der Seitenprüfung abdocken, kann man diese außerhalb von Visual Studio oder sogar über einen zweiten Bildschirm falls vorhanden. Allerdings in der Reihenfolge nach ALT + TAB zwischen der Seitenprüfung und Visual Studio, wenn das Page Inspector-Fenster abgedockt ist, wechseln Sie zu **Tools** &gt; **Optionen** &gt;  **Umgebung** &gt; **Registerkarten und Windows**, und wählen Sie unter **Registerkarte auch**, deaktivieren Sie das Kontrollkästchen namens **Unverankert Toolfenster bleiben immer auf die Klicken Sie im Hauptfenster**:

![Deaktivieren Sie das unverankerte Tool Windows-Kontrollkästchen, ALT + TAB zwischen Visual Studio und das nicht angedockte Fenster für die Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Im obere Bereich des Fensters die Seitenprüfung zeigt die aktuelle Seite in einem Browserfenster angezeigt. Im unteren Bereich zeigt die Seite im HTML-Markup auf der linken Seite, und einige Registerkarten auf der rechten Seite, mit denen Sie verschiedene Aspekte der Seite zu überprüfen. Der untere Bereich ist ähnlich wie die [F12-Entwicklertools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer. (Allerdings können im Gegensatz zu den Entwicklertools, Sie der Seitenprüfung direkt in Visual Studio verwenden.)

![Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In diesem Tutorial verwenden Sie im Browser der Seitenprüfung Bereich und die **HTML** und **Stile** Registerkarten können Sie schnell zu navigieren, und nehmen Sie Änderungen an der Anwendung.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Überprüfungsmodus zu aktivieren

Als Nächstes sehen Sie, wie der Seitenprüfung Überprüfungsmodus funktioniert. Klicken Sie im Fenster der Seitenprüfung auf die **prüfen** Schaltfläche.

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Um Überprüfungsmodus in Aktion zu sehen, bewegen Sie den Mauszeiger über verschiedene Teile der Seite im Fenster Browser der Seitenprüfung. Wie Sie dies tun, wird der Mauszeiger ändert sich zu einem großen Pluszeichen, und untergeordnete Element wird hervorgehoben:

![Mit dem Mauszeiger auf div.content-Wrappers](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Wenn Sie den Mauszeiger bewegen, beachten Sie, dass

- Der Inhalt **Quelle** Anzeigen von Änderungen an das Markup für das ausgewählte Element auf der Seite angezeigt. Das relevante Markup wird hervorgehoben. Wenn die Quelle in einer anderen Datei handelt, wird diese Datei mit den relevanten Markup hervorgehoben in der Quellansicht geöffnet.

- Das Markup angezeigt, der **HTML** Registerkarte in der Seitenprüfung ändert sich auch um das ausgewählte Element auf der Seite zu entsprechen. In der **HTML** Registerkarte wird das relevante Markup beschrieben.

- Die **Stile** Registerkarte zeigt die CSS-Regeln für die aktuelle Auswahl relevant.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Verwenden der Seitenprüfung Markup ändern

Jetzt sehen Sie, wie Sie die Seitenprüfung verwenden können, suchen, und nehmen Sie Änderungen an Markup oder Text, dessen Speicherort möglicherweise nicht sofort ersichtlich.

Fügen Sie der Seitenprüfung im Überprüfungsmodus aus, und führen Sie einen Bildlauf zum unteren Rand der Startseite.

Werden, sobald Sie den Footerbereich eingeben, wird die Seitenprüfung öffnet die *Site.Master* Layoutdatei im **Quelle** Ansicht in eine temporäre Registerkarte rechts neben den anderen Registerkarten, und werden im Abschnitt des Masters Seite, die Sie ausgewählt haben. So erfahren Sie, wie der Seitenprüfung suchen und Anzeigen von Inhalt auf einer Seite, die tatsächlich von einer anderen Datei als die stammen kann, die Sie ursprünglich geöffnet haben können.

![Highlights der Fußzeile im Überprüfungsmodus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Verschieben Sie den Mauszeiger im Fenster Browser der Seitenprüfung auf die Zeile mit dem Copyright <a id="a"> </a>beachten.

In der *Site.Master* Seite, die entsprechende Zeile wird hervorgehoben.

![Hervorgehobene Zeile der Fußzeile-copyright](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Hinzufügen von Text an das Ende der Zeile in der *Site.Master* Datei.

&lt;p&gt;&amp;kopieren; &lt;%: DateTime.Now.Year %&gt; -meine ASP.NET Application Rocks!&lt; / p&gt;

Jetzt drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste zum Anzeigen der Ergebnisse im Fenster Browser der Seitenprüfung.

![Meine ASP.NET Application Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Sie können daran gedacht, dass die Fußzeile auf war die *"default.aspx"* Seite, aber es stellte sich heraus auf der Seite Masterlayout sein, und der Seitenprüfung fand es für Sie.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Überprüfungsmodus und HTML-Fenster

Als Nächstes müssen Sie einen kurzen Blick auf die HTML-Fenster, und wie sie Elemente für Sie zugeordnet.

Fügen Sie der Seitenprüfung im Überprüfungsmodus aus.

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Klicken Sie auf den oberen Teil der Seite die Fehlermeldung "Ihr Logo hier einfügen". Untersuchen Sie ein bestimmtes Element im Detail, damit die Anzeige im Browserfenster nicht mehr geändert werden, wie Sie den Mauszeiger bewegen.

Nun verschieben Sie den Mauszeiger an die **HTML** Fenster. Während Sie den Mauszeiger bewegen, wird der Seitenprüfung beschrieben, das Element innerhalb der **HTML** Fenster und das entsprechende Element im Browserfenster hervorgehoben.

![HTML-Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Wie zuvor die Seitenprüfung öffnet die *Site.Master* -Datei für Sie in eine temporäre Registerkarte. Klicken Sie auf der Registerkarte "Site.Master", und das entsprechende Markup markiert die &lt;Header&gt; Abschnitt:

![Hervorgehobene markup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vorschau der CSS-Änderungen im Fenster Stile

Als Nächstes werden Sie sehen, wie Sie die Seitenprüfung verwenden können **Stile** Fenster Vorschau der Änderungen an CSS.

Klicken Sie auf die **prüfen** Schaltfläche, um die Seitenprüfung im Überprüfungsmodus zu platzieren.

Verschieben Sie im Fenster Browser der Seitenprüfung den Mauszeiger über den Abschnitt "Startseite", bis die **div.content-Wrappers** Bezeichnung angezeigt wird.

![Mit dem Mauszeiger auf Elemente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Klicken Sie im Abschnitt div.content-Wrappers einmal, und verschieben Sie den Mauszeiger an die **Stile** Fenster. Deaktivieren Sie unter dem Selektor .featured .content-Wrapper-Klasse einen Wert ein, und aktivieren Sie das Kontrollkästchen für die Background-Color-Eigenschaft.

![Clear-Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Beachten Sie, wie die Änderung sofort im Browser der Seitenprüfung-Fenster zeigt in der Vorschau.

Wählen Sie das Kontrollkästchen erneut aus, doppelklicken Sie auf den Wert der Eigenschaft, und ändern Sie ihn in `red`. Die Änderung wird sofort aus:

![Rote Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Die **Stile** Fenster macht es einfach, test und Vorschau CSS Änderungen erforderlich, bevor die Änderungen zu, um den Stil übernehmen Stylesheet selbst.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatische CSS-Synchronisierung

> [!NOTE]
> Dieses Feature erfordert Version 1.3 der Seitenprüfung.


Das Feature für die automatische CSS-Synchronisierung können Sie eine CSS-Datei direkt bearbeiten und die Änderungen sofort im Browser Seitenprüfung.

Klicken Sie auf **prüfen** der Seitenprüfung im Überprüfungsmodus zu platzieren.

Im Browser der Seitenprüfung verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content-Wrappers** Bezeichnung angezeigt wird. Klicken Sie auf einmal, um dieses Element auszuwählen.

Die **Stile** Fenster zeigt alle CSS-Regeln für dieses Element. Scrollen Sie zur Klassenselektor Suchen der .featured .content-Wrapper. Klicken Sie auf ".featured .content-Wrapper". Seitenprüfung öffnet die CSS-Datei, die dieses Format ("Site.CSS") definiert und hebt den entsprechenden CSS-Stil.

![CSS-Datei](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Ändern Sie nun den Wert für `background-color` auf "Rot". Die Änderung wird sofort im Browser Seitenprüfung angezeigt.

![Browser der Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Mithilfe der CSS-Farbauswahl

Als Nächstes erfahren Sie, wie Sie mit der Seitenprüfung schnell finden und das CSS für den markierten Text in der standardanwendung zu ändern. In diesem Beispiel haben Sie sich entschieden, dass Sie nicht wie die blaue Hervorhebung und es zu einer anderen Farbe ändern möchten.

Klicken Sie auf die **prüfen** Schaltfläche.

![Element prüfen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Im Fenster Browser der Seitenprüfung bewegen Sie den Mauszeiger über die hervorgehobene "Videos, Lernprogramme und Beispiele" Text, damit das CSS Bezeichnung "kennzeichnen" angezeigt wird.

![Mit dem Mauszeiger auf das Element markieren](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Klicken Sie auf den Text, um es auszuwählen. Die entsprechenden CSS-Auswahl Mark wird am unteren Rand der **Stile** Fenster.

![Markieren Sie die Auswahl im Fenster Stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Klicken Sie auf die Auswahl der Markierung. Daraufhin wird die *"Site.CSS"* -Datei für die Webanwendung. Klicken Sie auf der Registerkarte "Site.CSS", und die entsprechenden CSS-Selektor markiert ist:

![Markieren Sie die Auswahl im Stylesheet](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Wählen Sie aus, und entfernen Sie die Zeile mit der Background-Color-Eigenschaft.

Sie verwenden nun die neue Visual Studio 2012-CSS-Farbauswahl, wählen Sie eine neue Farbe für die **markieren** Background-Color-Eigenschaft.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Mithilfe der Visual Studio 2012 CSS-Farbauswahl

CSS-Editor in Visual Studio 2012 verfügt über eine Farbauswahl angezeigt, die ganz einfach auswählen, und fügen Sie Farben. Sie verfügt über eine einfache Farbleiste und eine Auswahl von "Pop-Down", die eine präzisere Kontrolle bietet.

Die Farbauswahl enthält eine Standardpalette mit Farben, unterstützt standardfarbnamen, Hashcodes, RGB-, RGBA-, HSL- und HSLA-Farben und verwaltet eine Liste der Farben, die Sie zuletzt in das Dokument verwendet haben.

Klicken Sie in der Zeile, in denen die Background-Color-Eigenschaft wurde, geben Sie "bc", und drücken Sie den Pfeil nach unten einmal.

Wenn Sie das erste Zeichen jedes Worts in einer Eigenschaft Bindestrich getrennte, z. B. "Background-Color" eingeben, filtert IntelliSense die Liste für Sie nur die Eigenschaften anzeigen, die mit übereinstimmen:

![IntelliSense gefiltert Werte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Geben Sie nun einen Doppelpunkt. Wenn Sie dies tun, wird der vollständige Hintergrundfarbe Eigenschaftenname eingefügt. Typ **#** oder **Rgb (**, und die Auswahl Farbleiste angezeigt wird:

![Die CSS-Auswahl Farbleiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Um die Funktionsweise die Farbleiste für die Auswahl, klicken Sie auf die Farben mit der Maus oder drücken Sie die nach-unten-Taste, und klicken Sie dann verwenden Sie die linke und Rechte Pfeiltasten, um die Farben zu durchlaufen. Wenn Sie eine Farbe besuchen, wird der entsprechende Wert für die Background-Color-Eigenschaft in der Vorschau angezeigt:

![Background-Color-Wert der Eigenschaft, die in der Vorschau angezeigt](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

An diesem Punkt können Sie den Wert, und klicken Sie dann ein Semikolon (;) zum Abschließen des CSS-Eintrags auswählen EINGABETASTE. Jetzt fahren Sie mit dem nächsten Abschnitt, damit Sie sehen können, die Funktionsweise der Farbe Auswahl Pop aus.

#### <a name="using-the-color-picker-pop-down"></a>Mithilfe der Farbe Auswahl Pop aus

Wenn bei der Farbleiste die genaue Farbe, die Sie benötigen, können Sie die Pop Dropdown-Farbauswahl.

Um es zu öffnen, klicken Sie auf die Chevron-Doppelpfeil am rechten Ende der Farbleiste, oder ein-oder zweimal drücken Sie den Pfeil nach unten auf der Tastatur.

![CSS-Farbe Auswahl Pop-ab](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Klicken Sie auf eine Farbe aus der senkrechte Strich auf der rechten Seite. Dies zeigt einen Farbverlauf für diese Farbe im Hauptfenster. Wählen Sie eine Farbe direkt aus der senkrechte Strich durch Drücken der EINGABETASTE, oder klicken Sie auf einem beliebigen Zeitpunkt im Hauptfenster mit größerer Genauigkeit zu wählen.

Wenn eine Farbe vorliegt, auf dem Bildschirm, die Sie verwenden möchten (es muss keine werden innerhalb der Visual Studio-Benutzeroberfläche), Sie können den Wert erfassen, indem Sie mit dem Tool Pipette in der unteren rechten Ecke.

Sie können auch die Deckkraft einer Farbe ändern, indem Sie den Schieberegler am unteren Rand der Farbauswahl. Auf diese Weise Änderungen Werte RGBA-Werte Farbe, da das RGBA-Format Deckkraft darstellen kann.

Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie ein Semikolon zum Abschließen des Background-Color-Eintrags in der *"Site.CSS"* Datei.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Die Seite Inspector-Update-Leiste

Die Seitenprüfung sofort erkennt die Änderung an der *"Site.CSS"* Datei (oder einer beliebigen Datei innerhalb der Anwendung) und eine Warnung in eine Update-Leiste angezeigt.

![Update-Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Speichern Sie alle Dateien und den Browser der Seitenprüfung zu aktualisieren, drücken Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste. Die Änderung in die Hervorhebungsfarbe wird im Browser angezeigt:

![Hervorhebungsfarbe geändert](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Beachten Sie, dass Sie bequem Browser der Seitenprüfung direkt aus Visual Studio-Umgebung aktualisiert. Verwenden anstelle von einem externen Browser der Seitenprüfung können Sie im Editor zu bleiben, wenn Sie Ihre Webanwendungen entwickeln.

## <a name="see-also"></a>Siehe auch

[Einführung in der Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9-Video)

[Fehlermeldungen der Seitenprüfung](https://go.microsoft.com/?linkid=9813062) (MSDN)
