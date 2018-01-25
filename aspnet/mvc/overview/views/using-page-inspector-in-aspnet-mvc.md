---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: "Verwenden der Seitenprüfung in ASP.NET MVC | Microsoft Docs"
author: rick-anderson
description: "Page Inspector in Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Wählen Sie jedes Element im integrierten Browser und Page Inspector i..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5b443963a089f96a9dab11b7db4a25451075d6be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="using-page-inspector-in-aspnet-mvc"></a>Verwenden der Seitenprüfung in ASP.NET MVC
====================
von Tim Ammann

> Page Inspector in Visual Studio 2012 ist ein Webentwicklungstool mit einem integrierten Browser. Wählen Sie jedes Element im integrierten Browser und Page Inspector sofort hervorgehoben, des Elements Quell- und CSS. Sie können alle MVC-Ansicht wechseln, schnelles Auffinden Quellen gerenderten Markups, und Browsertools direkt in Visual Studio-Umgebung verwenden.
> 
> [Zeigt das Video](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> Dieses Lernprogramm veranschaulicht die Überprüfungsmodus, aktivieren Sie dann schnell suchen und Bearbeiten von Markup und CSS in das Webprojekt. Das Lernprogramm verwendet ein MVC-Projekt, aber Sie können auch die Seitenprüfung für [Web Forms](https://go.microsoft.com/?linkid=9802001) und andere ASP.NET-Anwendungen.
> 
> Das Lernprogramm enthält die folgenden Abschnitte:
> 
> - [Erforderliche Komponenten](#_1_prerequisites)
> - [Erstellen Sie eine Webanwendung](#_2_creating_a)
> - [Verwenden Sie die Seitenprüfung auf Durchsuchen, um eine Sicht](#_3_using_page)
> - [Überprüfungsmodus aktivieren](#_4_inspection_mode)
> - [Mit der Seitenprüfung Markup ändern](#_5_using_page)
> - [Überprüfungsmodus und das Fenster "HTML"](#_6_inspection_mode)
> - [CSS-Vorschau der Änderungen im Fenster Stile](#_7_previewing_css)
> - [Automatische CSS-Synchronisierung](#css_auto_sync)
> - [Mithilfe der CSS-Farbauswahl](#css_color_picker)
> - [Zuordnen von dynamische Seitenelemente für JavaScript](#map_dynamic_elements)


<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Erforderliche Komponenten

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) oder [Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Verwenden Sie zum Abrufen der neuesten Version des Page Inspector [Webplattform-Installer](https://go.microsoft.com/fwlink/?LinkId=255386) Windows Azure SDK für .NET 2.0 installieren.


Seitenprüfung ist mit Microsoft Web Developer Tools enthalten. Die neueste Version ist 1.3. Überprüfen Sie die Version haben Sie, Visual Studio ausführen, und wählen Sie **zu Microsoft Visual Studio** aus der **Hilfe** Menü.

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Erstellen Sie eine Webanwendung

Erstellen Sie zunächst eine Webanwendung, der Seitenprüfung mit verwendet werden. Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**. Erweitern Sie auf der linken Seite **Visual C#-**Option **Web**, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**.

![Neues ASP.NET MVC-Anwendung](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Klicken Sie auf **OK**.

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung**. Lassen Sie **Razor** als Standardansichtsmodul.

![Neues ASP.NET MVC-Projekt - Internetanwendung](using-page-inspector-in-aspnet-mvc/_static/image4.png)

Öffnet die Anwendung in **Quelle** anzeigen.

![Neues ASP.NET MVC-Anwendung in der Quellansicht](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Nun, dass Sie eine Anwendung mit verwendet haben, können Sie Page Inspector überprüfen und ändern.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Verwenden Sie die Seitenprüfung auf Durchsuchen, um eine Sicht

In Visual Studio 2012, Sie können mit der rechten Maustaste einer beliebigen Ansicht in Ihrem Projekt, wählen **in Seitenprüfung anzeigen**, und Page Inspector herausfinden, die Route und die Seite angezeigt wird.

In **Projektmappen-Explorer**, erweitern Sie die **Ansichten** Ordner und dann die **Home** Ordner. Klicken Sie mit der mit der rechten Maustaste auf die Index.cshtml-Datei, und wählen Sie **in Seitenprüfung anzeigen**.

![Index.cshtml in Seitenprüfung anzeigen](using-page-inspector-in-aspnet-mvc/_static/image8.png)

Standardmäßig ist die Seitenprüfung als Fenster auf der linken Seite der Visual Studio-Umgebung angedockt. Falls gewünscht, können Sie an anderer Stelle andocken, oder die Verankerung aufheben, des Fensters. Finden Sie unter [Vorgehensweise: anordnen und Andocken von Fenstern](https://msdn.microsoft.com/library/z4y0hsax.aspx).

Im obere Bereich des Fensters Page Inspector zeigt die aktuelle Seite in einem Browserfenster angezeigt. Unteren Bereich zeigt die Seite im HTML-Markup, zusammen mit einigen Registerkarten, mit denen Sie verschiedene Aspekte der Seite zu überprüfen. Der untere Bereich ist ähnlich wie die [F12 Entwicklertools](https://msdn.microsoft.com/ie/aa740478) in Internet Explorer.

![ASP.NET MVC-Anwendung in der Seitenprüfung](using-page-inspector-in-aspnet-mvc/_static/image10.png)

In diesem Lernprogramm verwenden Sie die **HTML** und **Stile** Registerkarten, um schnell navigieren und Änderungen an der Anwendung vornehmen.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>EnableInspection-Modus

Um die Seitenprüfung in den Überprüfungsmodus zu speichern, klicken Sie auf die **Inspect** Schaltfläche. Im Überprüfungsmodus Wenn Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite halten wird das entsprechende Quellmarkup oder der Code hervorgehoben.

![Überprüfungsmodus zum ein-/ausschalten](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Jetzt können bewegen Sie die Maus über verschiedene Teile der Seite innerhalb Page Inspector. Wie in diesem Fall der Mauszeiger verwandelt sich in einem großen Pluszeichen (+), und das Element unterhalb hervorgehoben wird:

![Mit dem Mauszeiger auf div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

Während Sie den Mauszeiger bewegen, werden in Visual Studio die entsprechenden Razor-Syntax in der Quelldatei hervorgehoben. Wenn das HTML-Element aus einer anderen Quelldatei stammt, wird die Datei in Visual Studio automatisch geöffnet.

In der Seitenprüfung die **HTML** Registerkarte zeigt den HTML-Code, der vom Razor-Syntax generiert wurde. Während Sie den Mauszeiger bewegen, werden die HTML-Elemente hervorgehoben. Die **Stile** Registerkarte zeigt die CSS-Regeln für das Element.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Mit der Seitenprüfung Markup ändern

Page Inspector können Sie Markup suchen, deren Speicherort möglicherweise nicht offensichtlich. Anschließend können Sie das Markup ändern und die resultierenden Änderungen anzuzeigen.

Um dies zu sehen, klicken Sie auf **Inspect** und führen Sie einen Bildlauf zum unteren Rand der Seite in der Seite Inspektor-Fenster.

Wenn Sie den Mauszeiger in der Fußzeile bewegen, wird die Seitenprüfung öffnet die \_Layout.cshtml-Datei und markiert den Abschnitt der Layoutseite, die Sie ausgewählt haben. Wie Sie sehen können, die Fußzeile werden in der Layoutdatei und nicht die Sicht selbst definiert wird.

![Fußzeile](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Bewegen Sie den Mauszeiger jetzt über die Zeile mit der Copyright <a id="a"> </a>beachten. In der \_Layout.cshtml-Seite, die entsprechende Zeile wird hervorgehoben.

![Fußzeile copyright Zeile hervorgehoben](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Hinzufügen von Text an das Ende der Zeile in der \_Layout.cshtml-Datei.

&lt;p&gt;&amp;kopieren. @DateTime.Now.Year -Meine ASP.NET MVC-Anwendung einzigartig!  &lt; /p&gt;

Jetzt, drücken Sie Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste, um die Ergebnisse im Browserfenster Page Inspector anzuzeigen.

![Meine ASP.NET Anwendung Rocks!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Möglicherweise haben man, dass die Fußzeile, die in Index.cshtml definiert, aber es war in der \_Layout.cshtml und Page Inspector gefundenen für Sie.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Überprüfungsmodus und das Fenster "HTML"

Als Nächstes müssen Sie einen kurzen Blick auf das HTML-Fenster, und wie sie Elemente für Sie zugeordnet.

Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.

Klicken Sie auf den oberen Teil der Seite mit dem Hinweis "Die Logohere" angezeigt. Sie sind ein bestimmtes Element im Detail, damit die Anzeige im Browserfenster nicht mehr geändert werden, wie Sie den Mauszeiger untersucht.

Verschieben Sie nun den Mauszeiger an die **HTML** Fenster. Während Sie den Mauszeiger bewegen, werden Page Inspector das Element innerhalb der **HTML** Fenster und das entsprechende Element im Browserfenster hervorgehoben.

![HTML-Fenster](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Wie bereits zuvor können die Seitenprüfung öffnet die \_Layout.cshtml-Datei für Sie in einer temporären Registerkarte. Klicken Sie auf die \_Layout.cshtml temporäre Registerkarte und das entsprechende Markup wird hervorgehoben werden die &lt;Header&gt; Abschnitt für Sie:

![Hervorgehobene markup](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>CSS-Vorschau der Änderungen im Fenster Stile

Als Nächstes verwenden Sie die Seitenprüfung **Stile** Fenster aus, um Änderungen an CSS in der Vorschau anzeigen.

Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.

Im Browserfenster Page Inspector verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird.

![Mit dem Mauszeiger auf div.content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Klicken Sie in dem Abschnitt div.content Wrapper einmal, und verschieben Sie den Mauszeiger an die **Stile** Fenster. Die **Stile** Fenster enthält alle CSS-Regeln für dieses Element. Blättern Sie zum Suchen der .featured .content-Wrapper Klassenauswahl. Deaktivieren Sie nun das Kontrollkästchen für die Hintergrundfarbe Eigenschaft.

![Clear-Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Beachten Sie, wie die Änderung sofort im Browserfenster Page Inspector in der Vorschau anzeigt.

Aktivieren Sie das Kontrollkästchen erneut, doppelklicken Sie auf den Wert der Eigenschaft, und ändern Sie ihn in Rot. Die Änderung wird sofort gezeigt:

![Rote Hintergrundfarbe](using-page-inspector-in-aspnet-mvc/_static/image30.png)

Die **Stile** Fenster macht es einfach, testen und eine Vorschau CSS ändert, bevor die Änderungen zu, den Stil übernehmen Stylesheet selbst.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Automatische CSS-Synchronisierung

> [!NOTE]
> Dieses Feature erfordert Version 1.3 der Seitenprüfung.


Das Feature für die automatische CSS-Synchronisierung können Sie eine CSS-Datei direkt bearbeiten und die Änderungen werden sofort im Browser Seitenprüfung.

Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen.

Im Browser Seitenprüfung verschieben Sie den Mauszeiger über den Abschnitt "Startseite", bis die **div.content Wrapper** Bezeichnung angezeigt wird. Klicken Sie auf einmal, um dieses Element auszuwählen.

Die **Stile** Fenster enthält alle CSS-Regeln für dieses Element. Blättern Sie zum Suchen der .featured .content-Wrapper Klassenauswahl. Klicken Sie auf ".featured .content-Wrapper". Die Seitenprüfung öffnet die CSS-Datei, die definiert dieses Format ("Site.CSS" ändern) und hebt die entsprechenden CSS-Stil.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Jetzt ändern Sie den Wert für `background-color` auf "Rot". Die Änderung wird sofort im Browser Seitenprüfung angezeigt.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Mithilfe der CSS-Farbauswahl

CSS-Editor in Visual Studio 2012 verfügt über ein Farbwähler, der es einfach, wählen aus, und legen Sie Farben. Die Farbauswahl enthält eine Standardpalette mit Farben, unterstützt standardfarbnamen, Hashcodes, RGB-, RGBA-HSL- und HSLA Farben und verwaltet eine Liste der Farben, die Sie im Dokument zuletzt verwendet haben.

Im vorherigen Abschnitt den Wert geändert die `background-color` Eigenschaft. Um die Farbauswahl aufzurufen, platzieren Sie die Einfügemarke nach dem Eigenschaftsnamen und geben  **#**  oder **Rgb (**.

![Die CSS-Auswahl Farbleiste](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Klicken Sie auf eine Farbe aus, wählen Sie ihn aus, oder drücken die nach-unten-Taste, und klicken Sie dann verwenden Sie die linke und Rechte Pfeiltasten, um die Farben zu durchlaufen. Wenn Sie eine Farbe besuchen, wird der entsprechende Hexadezimalwert in der Vorschau angezeigt:

![Background-Color-Wert der Eigenschaft, die in der Vorschau angezeigt](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Wenn der Farbleiste nicht den gewünschten exakte der Farbe haben, können Sie der Farbe Datumsauswahl Pop aus. Um sie zu öffnen, klicken Sie auf das doppelte Chevron am rechten Ende der Farbleiste, oder drücken Sie den Pfeil nach unten ein-oder zweimal auf der Tastatur.

![CSS-Farbe Datumsauswahl Pop aus](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Klicken Sie auf eine Farbe aus der senkrechte Strich auf der rechten Seite. Dies zeigt einen Verlauf für diese Farbe im Hauptfenster. Wählen Sie eine Farbe direkt aus der senkrechte Strich durch Drücken der EINGABETASTE, oder klicken Sie auf einem beliebigen Punkt im Hauptfenster mit größerer Genauigkeit auswählen.

Wenn eine Farbe vorliegt, auf dem Bildschirm, die Sie verwenden möchten (keinen innerhalb der Visual Studio-Benutzeroberfläche), können Sie den Wert mithilfe der Pipette in der unteren rechten Ecke erfassen.

Sie können auch die Deckkraft einer Farbe ändern, mithilfe des Schiebereglers am unteren Rand der Farbauswahl. Auf diese Weise so Änderungen RGBA-Werten, Farbe, da das RGBA-Format Deckkraft darstellen kann.

Nachdem Sie eine Farbe ausgewählt haben, drücken Sie die EINGABETASTE, und geben Sie ein Semikolon zum Abschließen des Hintergrundfarbe Eintrags in der *"Site.CSS" ändern* Datei.

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Die Seite Inspektor Update-Leiste

Page Inspector sofort erkennt die Änderung an der *"Site.CSS" ändern* Datei und eine Warnung in eine Update-Leiste angezeigt.

![Update-Leiste](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Speichern Sie alle Dateien und Browser der Seitenprüfung zu aktualisieren, drücken Strg + Alt + Eingabe, oder klicken Sie auf die Update-Leiste. Die Änderung in der Hervorhebungsfarbe wird im Browser angezeigt.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Zuordnen von dynamische Seitenelemente für JavaScript

In modernen Webanwendungen werden die Elemente auf der Seite häufig dynamisch mit JavaScript generiert. Das bedeutet, dass es keine statische Markup (HTML- oder Razor), das diese Seitenelemente entspricht.

Mit Version 1.3 können Page Inspector Elemente zuordnen, die auf der Seite wieder auf den entsprechenden JavaScript-Code dynamisch hinzugefügt wurden. Um diese Funktion zu veranschaulichen, verwenden wir die [einzelnen Seite Anwendung (SPA) Vorlage](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> Die Vorlage SPA erfordert die [ASP.NET und Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650) aktualisieren.


Wählen Sie in Visual Studio **Datei** &gt; **neues Projekt**. Erweitern Sie auf der linken Seite **Visual C#-**Option **Web**, und wählen Sie dann **ASP.NET MVC 4-Webanwendung**. Klicken Sie auf **OK**.

In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **einseitigen Anwendung**.

Erweitern Sie im Projektmappen-Explorer die **Ansichten** Ordner und dann die **Home** Ordner. Klicken Sie mit der mit der rechten Maustaste auf die Index.cshtml-Datei, und wählen Sie **in Seitenprüfung anzeigen**.

Erstes, d. h. angezeigten im Browser Seitenprüfung ist eine Anmeldeseite. Klicken Sie auf "Registrieren", und erstellen Sie einen Benutzernamen und Kennwort. Sobald Sie sich anmelden, wird die Anwendung Sie anmeldet und erstellt eine to-Do-Liste mit einige Beispiel-Elemente.

Klicken Sie auf **Inspect** Page Inspector im Überprüfungsmodus ablegen. Klicken Sie im Browser Seitenprüfung auf eines der to-do-Elemente. Beachten Sie, dass anstelle von blau hervorgehoben wird, das Element in Orange ändert, die mit "JS" neben dem Elementnamen hervorgehoben ist. Dies gibt an, dass das Element dynamisch über ein Skript erstellt wurde.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Darüber hinaus eine orangefarbene Unterstreichung wird angezeigt, auf die **Aufrufliste** Registerkarte. Dies bedeutet, dass die **Aufrufliste** Bereich verfügt über weitere Informationen über das Element.

Klicken Sie auf die **Aufrufliste** Registerkarte. Die **Aufrufliste** Bereich zeigt die Aufrufliste für den JavaScript-Aufruf, der das Element erstellt. Aufrufe von externen Bibliotheken wie z. B. jQuery werden reduziert, so, dass Sie die Aufrufe an das Anwendungsskript leicht erkennen können.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Die Knoten mit der Bezeichnung "Bibliotheken" können erweitern, um die vollständige Aufrufliste, darunter auch Aufrufe an externe Bibliotheken finden Sie unter:

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Wenn Sie ein Element in der Aufrufliste klicken, wird von Visual Studio öffnet die Codedatei und das entsprechende Skript hervorgehoben.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Siehe auch

[Einführung in ASP.NET MVC 4 mit Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (ASP.NET-Website)

[Einführung in Seitenprüfung](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (Channel 9-Video)

[Fehlermeldungen der Seitenprüfung](https://go.microsoft.com/?linkid=9813062) (MSDN)
