---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: "Mit Page Inspector für Visual Studio 2012 in der ASP.NET Web Forms | Microsoft Docs"
author: rick-anderson
description: "Page Inspector für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Wählen Sie jedes Element im integrierten Browser und Page Inspector..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: a2ac8334e62e6ab7af7042572cfd5950c687001b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Mithilfe von Page Inspector für Visual Studio 2012 in ASP.NET Web Forms
====================
von Tim Ammann

> Page Inspector für Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Wählen Sie jedes Element im integrierten Browser und Page Inspector sofort hervorgehoben, des Elements Quell- und CSS. Sie können eine beliebige Seite in der Anwendung suchen, schnelles Auffinden Quellen gerenderten Markups, und Browsertools direkt in Visual Studio-Umgebung verwenden.
> 
> Dieses Lernprogramm Shwos wie Überprüfungsmodus aktivieren und dann schnell suchen und durch Bearbeiten der CSS-Regeln und Text innerhalb des Webprojekts. Das Lernprogramm verwendet ein Web Forms-Anwendungsprojekt, aber Sie können auch Page Inspector für Websiteprojekte und [MVC](https://go.microsoft.com/?linkid=9802002) Anwendungen.
> 
> Das Lernprogramm enthält die folgenden Abschnitte:
> 
> [Erforderliche Komponenten](#_1_prerequisites)
> 
> [Erstellen Sie eine Webanwendung](#_2_creating_a)
> 
> [Verwenden Sie zum Anzeigen der Anwendung Seitenprüfung](#_3_using_page)
> 
> [Überprüfungsmodus aktivieren](#_4_inspection_mode)
> 
> [Mit der Seitenprüfung Markup ändern](#_5_using_page)
> 
> [Überprüfungsmodus und das Fenster "HTML"](#_6_inspection_mode)
> 
> [CSS-Vorschau der Änderungen im Fenster Stile](#_7_previewing_css)
> 
> [Automatische CSS-Synchronisierung](#css_auto_sync)
> 
> [Mithilfe der CSS-Farbauswahl](#css_color_picker)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11/en-us) oder [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11/en-us/downloads#express-web).

> [!NOTE]
> Verwenden Sie zum Abrufen der neuesten Version des Page Inspector [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) auf das Azure SDK für .NET 2.0 installieren.


Seitenprüfung ist mit Microsoft Web Developer Tools enthalten. Die neueste Version ist 1.3. Überprüfen Sie die Version haben Sie, Visual Studio ausführen, und wählen Sie **zu Microsoft Visual Studio** aus der **Hilfe** Menü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Erstellen Sie eine Webanwendung

Zunächst erstellen Sie eine Webanwendung, der Seitenprüfung mit verwendet werden. Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**. Erweitern Sie auf der linken Seite **Visual C#-**Option **Web**, und wählen Sie dann **ASP.NET Web Forms-Anwendung**.

![Neue Web Forms-Anwendung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Klicken Sie auf **OK**.

Öffnet die Anwendung in **Quelle** anzeigen.

![Neue Web Forms-Anwendung in der Quellansicht](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Nun, dass Sie eine Anwendung mit verwendet haben, können Sie Page Inspector überprüfen und ändern.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Verwenden Sie zum Anzeigen der Anwendung Seitenprüfung

Als Nächstes zeigen Sie die Anwendung mit Page Inspector an. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie dann **in Seitenprüfung anzeigen**.

![In Seitenprüfung anzeigen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

Standardmäßig Wenn Page Inspector zum ersten Mal gestartet wird ist es als schmale Fenster auf der linken Seite der Visual Studio-Umgebung angedockt. Lassen Sie das Feld auf der linken Seite angedockt, und legen Sie es auf eine Breite, die für Sie vertraut ist, oder in einem der Bereiche Tool auf nach oben, unten oder rechts Andocken:

![Page Inspector andockbaren Positionen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Wenn Sie das Fenster Page Inspector abdocken, können Sie fügen Sie ihn außerhalb von Visual Studio oder sogar auf einen zweiten Bildschirm sofern vorhanden. In der Reihenfolge nach ALT + TAB zwischen Page Inspector und Visual Studio, wenn die Page Inspector-Fenster nicht angedockter ist, führen jedoch zu **Tools** &gt; **Optionen** &gt;  **Umgebung** &gt; **Registerkarten und Fenster**, und wählen Sie unter **Registerkarte auch**, deaktivieren Sie das Kontrollkästchen namens **Floating-Toolfenster immer auf der Basis von der im Hauptfenster**:

![Deaktivieren Sie das unverankerte Tool Windows-Kontrollkästchen ALT + TAB zwischen Visual Studio "und" nicht angedockter Page Inspector "](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

Im obere Bereich des Fensters Page Inspector zeigt die aktuelle Seite in einem Browserfenster angezeigt. Unteren Bereich zeigt die Seite im HTML-Markup auf der linken Seite, und überprüfen Sie einige Registerkarten auf der rechten Seite, mit denen Sie verschiedene Aspekte der Seite. Der untere Bereich ist ähnlich wie die [F12 Entwicklertools](https://msdn.microsoft.com/en-us/ie/aa740478) in Internet Explorer. (Allerdings können im Gegensatz zu den Entwicklertools Sie Page Inspector direkt in Visual Studio verwenden.)

![Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

In diesem Lernprogramm verwenden Sie im Bereich für die Seitenprüfung Browser und dem **HTML** und **Stile** Registerkarten können Sie schnell zu navigieren und Änderungen an der Anwendung vornehmen.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Überprüfungsmodus aktivieren

Als Nächstes sehen Sie, wie der Seitenprüfung Überprüfungsmodus funktioniert. Klicken Sie im Fenster Seitenprüfung auf die **Inspect** Schaltfläche.

![Element untersuchen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Um Überprüfungsmodus in Aktion anzuzeigen, bewegen Sie die Maus über verschiedene Teile der Seite im Browserfenster Page Inspector fest. Wie in diesem Fall der Mauszeiger verwandelt sich in einem großen Pluszeichen (+), und das Element unterhalb hervorgehoben wird:

![Mit dem Mauszeiger auf div.content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Wenn Sie den Mauszeiger verschieben, beachten Sie, dass

- Der Inhalt in **Quelle** anzeigen geändert, um das Markup für das ausgewählte Element auf der Seite anzuzeigen. Das relevante Markup wird hervorgehoben. Wenn die Quelle in einer anderen Datei ist, wird diese Datei durch das relevante Markup hervorgehoben in der Quellansicht geöffnet.

- Das Markup angezeigt, der **HTML** Registerkarte in der Seitenprüfung ändert sich auch um das ausgewählte Element auf der Seite zu entsprechen. In der **HTML** Registerkarte wird das relevante Markup erläutert.

- Die **Stile** Registerkarte zeigt die CSS-Regeln für die aktuelle Auswahl relevant.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Mit der Seitenprüfung Markup ändern

Jetzt sehen Sie, wie Sie die Seitenprüfung verwenden können, suchen, und nehmen Sie Änderungen an Markup oder Text, dessen Speicherort möglicherweise nicht sofort bemerkbar.

Put Page Inspector im Überprüfungsmodus aus, und führen Sie einen Bildlauf zum unteren Rand der Seite "Home".

Als Eingabe der Fußzeilenbereich wird die Seitenprüfung öffnet die *Site.Master* Layoutdatei in **Quelle** Sicht in einer temporären Registerkarte rechts neben den anderen Registerkarten und markiert den Abschnitt des Masters Seite, die Sie ausgewählt haben. Auf diese Weise wird Page Inspector können wie Suchen und Anzeigen von Inhalt auf einer Seite, die tatsächlich von demjenigen, der eine andere Datei stammen kann, die Sie ursprünglich geöffnet.

![Fußzeile Markierungen im Überprüfungsmodus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

Im Browserfenster Page Inspector bewegen Sie den Mauszeiger über der Zeile mit den Copyrighthinweis <a id="a"> </a>beachten.

In der *Site.Master* Seite der entsprechenden Zeile wird hervorgehoben.

![Fußzeile copyright Zeile hervorgehoben](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Hinzufügen von Text an das Ende der Zeile in der *Site.Master* Datei.

&lt;p&gt;&amp;kopieren. &lt;%: DateTime.Now.Year %&gt; -meine ASP.NET Anwendung Rocks!&lt; / p&gt;

Jetzt, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste, um die Ergebnisse im Browserfenster Page Inspector anzuzeigen.

![Meine ASP.NET Anwendung Rocks!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Sie möglicherweise, dass die Fußzeile auf wurde betrachtet haben die *"default.aspx"* Seite, aber es war in der master-Layoutseite und Page Inspector es für Sie gefunden.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Überprüfungsmodus und das Fenster "HTML"

Als Nächstes müssen Sie einen kurzen Blick auf das HTML-Fenster, und wie sie Elemente für Sie zugeordnet.

Fügen Sie die Seitenprüfung im Überprüfungsmodus aus.

![Element untersuchen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Klicken Sie auf den oberen Teil der Seite, die besagt, dass "Ihr Logo hier einfügen". Sie sind ein bestimmtes Element im Detail, damit die Anzeige im Browserfenster nicht mehr geändert werden, wie Sie den Mauszeiger untersucht.

Verschieben Sie nun den Mauszeiger an die **HTML** Fenster. Während Sie den Mauszeiger bewegen, werden Page Inspector das Element innerhalb der **HTML** Fenster und das entsprechende Element im Browserfenster hervorgehoben.

![HTML-Fenster](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Wie bereits zuvor können die Seitenprüfung öffnet die *Site.Master* -Datei für Sie in einer temporären Registerkarte. Klicken Sie auf der Registerkarte "Site.Master", und das entsprechende Markup wird hervorgehoben, der &lt;Header&gt; Abschnitt:

![Hervorgehobene markup](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>CSS-Vorschau der Änderungen im Fenster Stile

Danach sehen Sie Verwendung der Seitenprüfung **Stile** Fenster aus, um Änderungen an CSS in der Vorschau anzeigen.

Klicken Sie auf die **Inspect** Schaltfläche Page Inspector im Überprüfungsmodus ablegen.

Im Browserfenster Page Inspector verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird.

![Mit dem Mauszeiger auf Elemente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Klicken Sie in dem Abschnitt div.content Wrapper einmal, und verschieben Sie den Mauszeiger an die **Stile** Fenster. Deaktivieren Sie unter .featured .content-Wrapper Klassenauswahl und aktivieren Sie das Kontrollkästchen für die Hintergrundfarbe Eigenschaft.

![Clear-Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Beachten Sie, wie die Änderung sofort im Browserfenster Page Inspector in der Vorschau anzeigt.

Wählen Sie das Kontrollkästchen erneut aus, doppelklicken Sie auf den Wert der Eigenschaft, und ändern Sie ihn in `red`. Die Änderung wird sofort gezeigt:

![Rote Hintergrundfarbe](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

Die **Stile** Fenster macht es einfach, testen und eine Vorschau CSS ändert, bevor die Änderungen zu, den Stil übernehmen Stylesheet selbst.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatische CSS-Synchronisierung

> [!NOTE]
> Dieses Feature erfordert Version 1.3 der Seitenprüfung.


Das Feature für die automatische CSS-Synchronisierung können Sie eine CSS-Datei direkt bearbeiten und die Änderungen werden sofort im Browser Seitenprüfung.

Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.

Im Browser Seitenprüfung verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird. Klicken Sie auf einmal, um dieses Element auszuwählen.

Die **Stile** Fenster enthält alle CSS-Regeln für dieses Element. Blättern Sie zum Suchen der .featured .content-Wrapper Klassenauswahl. Klicken Sie auf ".featured .content-Wrapper". Die Seitenprüfung öffnet die CSS-Datei, die definiert dieses Format ("Site.CSS" ändern) und hebt die entsprechenden CSS-Stil.

![CSS-Datei](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Jetzt ändern Sie den Wert für `background-color` auf "Rot". Die Änderung wird sofort im Browser Seitenprüfung angezeigt.

![Browser der Seitenprüfung](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Mithilfe der CSS-Farbauswahl

Als Nächstes erfahren Sie, wie mit Page Inspector Schnelles Auffinden und den CSS-Code für den markierten Text in der standardanwendung zu ändern. In diesem Beispiel haben Sie sich entschieden, dass Sie nicht wie die blaue Hervorhebung und es in eine andere Farbe ändern möchten.

Klicken Sie auf die **Inspect** Schaltfläche.

![Element untersuchen](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

Im Browserfenster Page Inspector bewegen Sie den Mauszeiger über den hervorgehobenen "Videos, Lernprogramme und Beispiele" Text, damit das CSS Bezeichnung "kennzeichnen" angezeigt wird.

![Mit dem Mauszeiger auf das Element markieren](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Klicken Sie auf den Text, um ihn auszuwählen. Die entsprechenden CSS-Auswahl Markierung wird am unteren Rand der **Stile** Fenster.

![Markieren Sie die Auswahl in das Fenster Stile](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Klicken Sie auf die Auswahl der Markierung. Daraufhin wird die *"Site.CSS" ändern* Datei für die Webanwendung. Klicken Sie auf der Registerkarte "Site.CSS" ändern ", und die entsprechenden CSS-Selektor wird hervorgehoben:

![Markieren Sie die Auswahl im Stylesheet](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Wählen Sie aus, und entfernen Sie die Zeile mit der Background-Color-Eigenschaft.

Verwenden Sie nun die neue Visual Studio 2012 CSS-Farbauswahl auf eine neue Farbe für die **markieren** Background-Color-Eigenschaft.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Mithilfe der Visual Studio 2012 CSS-Farbauswahl

CSS-Editor in Visual Studio 2012 verfügt über ein Farbwähler, der es einfach, wählen aus, und legen Sie Farben. Er verfügt über eine einfache Farbleiste und eine Auswahl von "Pop-Down", die eine genauere Steuerung des bietet.

Die Farbauswahl enthält eine Standardpalette mit Farben, unterstützt standardfarbnamen, Hashcodes, RGB-, RGBA-HSL- und HSLA Farben und verwaltet eine Liste der Farben, die Sie im Dokument zuletzt verwendet haben.

Klicken Sie in der Zeile, in dem die Background-Color-Eigenschaft wurde, geben Sie "bc", und drücken Sie den Pfeil nach unten einmal.

Wenn Sie das erste Zeichen jedes Worts in einen Bindestrich getrennte Eigenschaft wie "Background-Color" eingeben, wird die Liste nur die Eigenschaften angezeigt werden, passen Sie von IntelliSense gefiltert:

![IntelliSense gefiltert Werte](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Geben Sie nun einen Doppelpunkt. In diesem Fall wird der vollständige Hintergrundfarbe Eigenschaftsname eingefügt. Typ  **#**  oder **Rgb (**, und die Auswahl einer Farbleiste angezeigt wird:

![Die CSS-Auswahl Farbleiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Um zu sehen, wie die Auswahl einer Farbleiste funktioniert, klicken Sie auf die Farben der Mauszeiger die Form, oder drücken Sie die nach-unten-Taste, und klicken Sie dann verwenden Sie die linke und Rechte Pfeiltasten, um die Farben zu durchlaufen. Wenn Sie eine Farbe besuchen, wird der entsprechende Wert für die Background-Color-Eigenschaft in der Vorschau angezeigt:

![Background-Color-Wert der Eigenschaft, die in der Vorschau angezeigt](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

An diesem Punkt können Sie Drücken der EINGABETASTE können Sie den Wert, und klicken Sie dann ein Semikolon (;) zum Abschließen des CSS-Eintrags auswählen. Jetzt fahren Sie mit dem nächsten Abschnitt, damit Sie sehen können, die Funktionsweise der Farbe Datumsauswahl Pop aus.

#### <a name="using-the-color-picker-pop-down"></a>Mithilfe der Farbe Datumsauswahl Pop aus

Wenn bei der Farbleiste genau die Farbe, die Sie suchen, können Sie die Farbauswahl Pop aus.

Um sie zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie den Pfeil nach unten ein-oder zweimal auf der Tastatur.

![CSS-Farbe Datumsauswahl Pop aus](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Klicken Sie auf eine Farbe aus der senkrechte Strich auf der rechten Seite. Dies zeigt einen Verlauf für diese Farbe im Hauptfenster. Wählen Sie eine Farbe direkt aus der senkrechte Strich durch Drücken der EINGABETASTE, oder klicken Sie auf einem beliebigen Punkt im Hauptfenster mit größerer Genauigkeit auswählen.

Wenn eine Farbe vorliegt, auf dem Bildschirm, die Sie verwenden möchten (keinen innerhalb der Visual Studio-Benutzeroberfläche), können Sie den Wert mithilfe der Pipette in der unteren rechten Ecke erfassen.

Sie können auch die Deckkraft einer Farbe ändern, mithilfe des Schiebereglers am unteren Rand der Farbauswahl. Da Sie dadurch Änderungen RGBA Werten Farbe, da das RGBA-Format Deckkraft darstellen kann.

Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie ein Semikolon zum Abschließen des Hintergrundfarbe Eintrags in der *"Site.CSS" ändern* Datei.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Die Seite Inspektor Update-Leiste

Page Inspector sofort erkennt die Änderung an der *"Site.CSS" ändern* Datei (oder alle Dateien in der Anwendung) und eine Warnung in einer Update-Leiste angezeigt.

![Update-Leiste](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Speichern Sie alle Dateien und Browser der Seitenprüfung zu aktualisieren, drücken Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste. Die Änderung in der Hervorhebungsfarbe wird im Browser angezeigt:

![Hervorhebungsfarbe geändert](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Beachten Sie, dass Sie den Browser der Seitenprüfung direkt in der Visual Studio-Umgebung problemlos aktualisiert. Mit Page Inspector anstelle von einem externen Browser können Sie im Editor zu bleiben, wenn Sie Ihre Internetanwendungen zu entwickeln.

## <a name="see-also"></a>Siehe auch

[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (Channel 9-Video)

[Fehlermeldungen der Seitenprüfung](https://go.microsoft.com/?linkid=9813062) (MSDN)
