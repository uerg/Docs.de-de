---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Neues in ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistung von ASP.NET und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 485d2ba7a1274bbb36cfbcbca9322cecc8c8d77c
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/18/2018
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Was ist neu in ASP.NET MVC 4

Durch [Web Lager Team](https://twitter.com/webcamps)

[Herunterladen von Web-Lager Training Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistung von ASP.NET und .NET Framework. Diese neue, vierte Version des Frameworks konzentriert sich auf die mobile Anwendung Webentwicklung einfacher zu machen.

Zunächst bei der Erstellung eines neuen ASP.NET MVC 4-Projekts besteht jetzt eine mobile Anwendung-Projektvorlage, die Sie verwenden können, um eine eigenständige app speziell für mobile Geräte zu erstellen. Darüber hinaus wird die ASP.NET MVC 4 mit jQuery Mobile über jQuery.Mobile.MVC NuGet-Paket integriert. jQuery Mobile ist ein HTML5-basierte Framework für die Entwicklung von webapps, die mit allen gängigen mobilen Gerät-Plattformen, einschließlich Windows Phone, iPhone, Android usw. kompatibel sind. Wenn die gewünschte Spezialisierung ermöglicht Sie verschiedene Ansichten für verschiedene Geräte dienen und gerätespezifische Optimierungen allerdings auch ASP.NET MVC 4.

In dieser praktischen Übungseinheit, beginnen Sie mit der ASP.NET MVC 4 &quot;Internetanwendung&quot; Projektvorlage für eine Fotogalerie-Anwendung zu erstellen. Sie werden schrittweise erhöhen, die app mithilfe von jQuery Mobile und ASP.NET MVC 4 neuen Funktionen, die um sie mit anderen mobilen Geräten und desktop Webbrowsern kompatibel zu machen. Außerdem lernen Sie neuen Code Know-how für die codegenerierung und wie ASP.NET MVC 4 asynchrone Aktionsmethoden schreiben, durch die Unterstützung einer Aufgabe erleichtert&lt;ActionResult&gt; Rückgabetypen.

> [!NOTE]
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [Versionen von Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für diese Übung finden Sie unter [Neuigkeiten in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Nutzen Sie die Erweiterungen der für die ASP.NET MVC-Projekt Vorlagen, einschließlich der neue Projektvorlage für die mobile Anwendung
- Verwenden Sie das HTML5 Viewport-Attribut und der CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern
- Verwenden Sie für progressiven Verbesserungen und zum Erstellen von Web-Touch-optimierte Benutzeroberfläche jQuery Mobile
- Mobile-spezifischen Ansichten erstellen
- Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente
- Erstellen von asynchronen Controller Dank Unterstützung der Aufgabe

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).
- [ASP.NET MVC 4](../../../mvc4.md) (enthalten in der Microsoft Visual Studio 2012-Installation)
- Windows Phone-Emulator (enthalten der [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Optional – [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) mit **Electric Plum iPhone-Simulator** -Erweiterung (nur für Übung 3 verwendet, um die Webanwendung mit einem iPhone-Simulator durchsuchen)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

In der Lab-Dokument werden Sie aufgefordert, Codeblöcke einfügen. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitte, die Sie von innerhalb von Visual Studio verwenden können, um zu vermeiden, müssen sie manuell hinzufügen.

So installieren Sie die Codeausschnitte

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie in des Labors **Source\Setup** Ordner.
2. Doppelklicken Sie auf die **"Setup.cmd"** Datei in diesem Ordner zum Installieren von Visual Studio-Codeausschnitte.

Wenn Sie nicht mit den Visual Studio-Codeausschnitte und zu erfahren, wie deren Verwendung vertraut sind, Sie können finden Sie im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Neue ASP.NET MVC 4-Projektvorlagen](#Exercise1)
2. [Erstellen der Fotogalerie Web-Anwendung](#Exercise2)
3. [Hinzufügen von Unterstützung für Mobile Geräte](#Exercise3)
4. [Verwenden asynchrone Controller](#Exercise4)

> [!NOTE]
> Jede Übung beiliegen ein **End** Ordner, der sich so ergebende Lösung, die Sie nach Abschluss der Übung abrufen soll. Sie können diese Lösung als Leitfaden verwenden, ggf. zusätzliche Hilfe bei der Übungen durcharbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Übung 1: Neues ASP.NET MVC 4-Projektvorlagen

In dieser Übung untersuchen Sie die Erweiterungen in ASP.NET MVC 4-Projektvorlagen. Zusätzlich zu den Internet-Anwendungsvorlage enthält in MVC 3 bereits vorhanden., diese Version nun eine separate Vorlage für Mobile Anwendungen. Zuerst sehen Sie sich einige relevanten Funktionen der einzelnen Vorlagen. Klicken Sie dann, arbeiten Sie auf das Rendern der Seite ordnungsgemäß auf die verschiedenen Plattformen mithilfe des richtigen Ansatzes.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Aufgabe 1: Untersuchen der Internet Application-Vorlage

1. Open **Visual Studio**.
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **PhotoGallery**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**.

    > [!NOTE]
    > Später passen Sie PhotoGallery ASP.NET MVC 4-Lösung, die Sie erstellen.

    ![Erstellen eines neuen Projekts](whats-new-in-aspnet-mvc-4/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.

    ![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](whats-new-in-aspnet-mvc-4/_static/image2.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt. Das Ziel besteht darin, die Anzahl von Zeichen und Tastatureingaben in einer Datei erforderlich, eine schnelle und Fluid Codierung Workflow aktivieren zu minimieren. Razor nutzt vorhandenen c# / VB (oder anderen) Sprachkenntnisse und übermittelt Sie eine Vorlage Markup-Syntax, die einen awesome HTML-Konstruktion-Workflow ermöglicht.
4. Drücken Sie **F5** um die erneuerten Vorlagen anzuzeigen und die Projektmappe auszuführen. Sie können die folgenden Funktionen Auschecken:

    **Modernes Vorlagen**

    Die Vorlagen haben erneuert wurde, mehr Modern aussehende Arten bereitstellen.

    ![Vorlagen für ASP.NET MVC 4 restyled](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled Vorlagen")

    *ASP.NET MVC 4 restyled Vorlagen*

    ![Seite "Neues wenden Sie sich an"](whats-new-in-aspnet-mvc-4/_static/image4.png "Seite Neuer Kontakt")

    *Neuer Kontakt (Seite)*

    **Adaptives Rendering**

    Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout dynamisch an die Größe des neuen Fensters anpasst. Diese Vorlagen mithilfe der adaptiven Renderingtechnik um ordnungsgemäß auf Desktop- und mobile Plattformen ohne Anpassung zu rendern.

    ![ASP.NET MVC 4-Projektvorlage in anderen Browser Größen](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4-Projektvorlage in anderen Browser Größen")

    *ASP.NET MVC 4-Projektvorlage in anderen Browser Größen*

    **Umfangreichere-Benutzeroberfläche in JavaScript**

    Eine andere Erweiterung aus Standardprojektvorlagen ist die Verwendung von JavaScript, überwiegend interaktive JavaScript bereitzustellen. Die Links anmelden und registrieren, die in der Vorlage benutzten vereinigen, wie mithilfe der jQuery-Überprüfungen Eingabefelder aus einer clientseitigen überprüfen.

    ![jQuery-Validierung](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery-Validierung*

    > [!NOTE]
    > Beachten Sie, dass die beiden Abschnitte, die im ersten Abschnitt Sie melden Sie sich in kann melden Sie sich mit einem Konto registriert, von der Website und im zweiten Abschnitt, den Sie Altenativelly melden Sie sich mit einem anderen Authentifizierungsdienst wie Google (standardmäßig deaktiviert können).
5. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
6. Öffnen Sie die Datei **AuthConfig.cs** unterhalb der **App\_starten** Ordner.
7. Entfernen Sie die Kommentarzeichen aus der letzten Zeile zum Registrieren von Google-Client für *OAuth* Authentifizierung.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Beachten Sie, dass Sie problemlos alle OpenID oder OAuth-Dienst wie Facebook, Twitter, Microsoft-Authentifizierung aktivieren.
8. Drücken Sie **F5** führen Sie die Projektmappe, und navigieren zur Anmeldeseite.
9. Wählen Sie **Google** Dienst zum Anmelden.

    ![Das Protokoll auswählen im Dienst](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Das Protokoll auswählen im Dienst*
10. Melden Sie sich mit Ihrem Google-Konto.
11. Zulassen Sie den Standort ("localhost") zum Abrufen von Informationen von Google-Konto
12. Schließlich müssen Sie am Standort der Google-Konto zuordnen zu registrieren.

   ![Ordnen Sie Ihre Google-Konto](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Zuordnen von Google-Konto*
13. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
14. Untersuchen Sie nun die Projektmappe Auschecken einige andere neuen Funktionen, die in der Projektvorlage von ASP.NET MVC 4 eingeführt.

   ![Die Projektvorlage für ASP.NET MVC 4 Internet Anwendung](whats-new-in-aspnet-mvc-4/_static/image9.png "der Projektvorlage Internet ASP.NET MVC 4")

   *Die ASP.NET MVC 4 Internet Application-Projektvorlage*

   - **HTML 5 Markup**

       Durchsuchen Sie die Ansichten der Vorlagen so ermitteln Sie das neue Design Markup.

       ![Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml.")

       *Neue Vorlage für die Verwendung von Razor und HTML5-Markups (About.cshtml).*
   - **Aktualisierte JavaScript-Bibliotheken**

       Die ASP.NET MVC 4-Standardvorlage enthält nun KnockoutJS, ein MVVM JavaScript-Framework, das rich erstellen kann und extrem reaktionsschnellen Webanwendungen, die mit JavaScript und HTML. Wie werden in MVC3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.

     > [!NOTE]
     > Sie erhalten weitere Informationen zur KnockOutJS Library unter diesem Link: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Darüber hinaus erfahren Sie über jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Aufgabe 2: untersuchen die Vorlage für die Mobile Anwendung

ASP.NET MVC 4 erleichtert die Entwicklung von Websites für mobile Geräte und Tablet-Browser. Diese Vorlage hat die gleiche Anwendungsstruktur wie die Internet Application-Vorlage (Beachten Sie, dass der controllercode praktisch identisch ist), aber der Stil wurde geändert, um auf mobilen Geräten Touch-basierte ordnungsgemäß gerendert.

1. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **PhotoGallery.Mobile**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), wählen Sie &quot;zu Projektmappe hinzufügen&quot; , und klicken Sie auf **OK**.
2. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Mobile-Anwendung** -Projektvorlage, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.

    ![Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung](whats-new-in-aspnet-mvc-4/_static/image11.png "Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung*
3. Jetzt sind Sie in der Lage, untersuchen die Projektmappe, und sehen Sie sich einige der neuen Funktionen von der ASP.NET MVC 4-Projektmappenvorlage für mobile Geräte:

    - **jQuery Mobile-Bibliothek**

        Die Mobile Anwendung-Projektvorlage enthält die mobilen jQuery-Bibliothek, also eine open Source-Bibliothek für mobile Browserkompatibilität. jQuery Mobile gilt schrittweise zunehmenden Verbesserungen für mobile Browser, CSS und JavaScript zu unterstützen. Schrittweise zunehmenden Verbesserungen ermöglicht allen Browsern, die grundlegenden Inhalte einer Webseite anzuzeigen, während er nur die leistungsstärksten Browser zum Anzeigen von umfangreichen Inhalten ermöglicht. Die JavaScript und CSS-Dateien, enthalten, in dem jQuery Mobile Stil Hilfe mobile Browsern an den Inhalt in den Bildschirm passen, ohne jede Änderung im Markup Seite.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *in der Vorlage enthaltenen jQuery mobile-Bibliothek*
    - **HTML5-basierte markup**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Mobile Anwendung-Vorlage, die mit HTML5 Markup (Login.cshtml und index.cshtml)*
4. Drücken Sie **F5** um die Projektmappe auszuführen.
5. Öffnen der **Windows Phone 7 Emulator**.
6. Öffnen Sie Internet Explorer, in der Phone-Startbildschirm. Sehen Sie sich die URL, wobei der desktop-Anwendung gestartet, und navigieren Sie zu der URL, die vom Telefon (z. B. `http://localhost:[PortNumber]/`).
7. Jetzt können Sie geben die Seite "Login", oder sehen Sie sich die zu den Seiten. Beachten Sie, dass das Format der Website auf die neue Windows Store-app für mobile Geräte. Die ASP.NET MVC 4-Projektvorlage wird richtig angezeigt, auf mobilen Geräten, und stellen Sie sicher, dass alle Elemente der Seite sichtbar und aktiviert sind. Beachten Sie, dass die Links in der Kopfzeile groß genug, um geklickt oder abgerufen werden.

    ![Projekt-Seiten in einem mobilen Gerät](whats-new-in-aspnet-mvc-4/_static/image14.png "Projekt Vorlagenseiten für ein mobiles Gerät")

    *Vorlagenseiten für das Projekt in einem mobilen Gerät*
8. Die neue Vorlage verwendet auch die **Viewport Meta-Tag**. Die meisten mobile Browsern definieren eine Breite für ein virtuelles Browserfenster oder &quot;Viewport&quot;, ist größer als die tatsächliche Breite des mobilen Geräts. Dadurch können mobile Browser, um die gesamte Webseite innerhalb der virtuellen Anzeige anzuzeigen. Die **Viewport Meta-Tag** können Webentwickler, legen Sie die Breite, Höhe und Skalierung Browserbereich auf mobilen Geräten **.** Die ASP.NET MVC 4-Vorlage für Mobile Anwendungen wird des Viewports auf die Breite des Geräts (&quot;Breite = Gerät Breite&quot;) in die Layoutvorlage (*Views\Shared\_Layout.cshtml*), sodass alle der Seiten werden ihre Viewport auf das Gerät Bildschirmbreite festgelegt haben. Beachten Sie, dass das Viewport-Meta-Tag die Standardansicht für den Browser nicht geändert werden.
9. Open  **\_Layout.cshtml**befindet sich im die **Ansichten | Freigegebene** Ordner und einen Kommentar das Viewport-Meta-Tag. Führen Sie die Anwendung, wenn nicht bereits geöffnet, und sehen Sie sich die Unterschiede.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Der Standort nach dem kommentieren den Viewport-Meta-Tag](whats-new-in-aspnet-mvc-4/_static/image15.png "der Standort nach dem kommentieren den Viewport-Meta-Tag")

*Der Standort nach dem kommentieren den Viewport-Meta-tag*
10. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
11. Kommentieren Sie den Viewport-Meta-Tag.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Aufgabe 3 - mit adaptiver Rendering

In dieser Aufgabe erfahren Sie, eine andere Methode zum Rendern einer Webseite ordnungsgemäß auf mobilen Geräten und Webbrowsern zur gleichen Zeit ohne jede Anpassung. Sie haben bereits Viewport Meta-Tag mit einem ähnlichen Zweck verwendet. Nachdem Sie eine weitere leistungsstarke Methode erfüllt: *adaptive Rendering*.

Adaptives Rendering ist eine Technik, die verwendet **CSS3-Medienabfragen** angewendet auf eine Seite anpassen. Medienabfragen definieren die Bedingungen in einem Stylesheet, gruppieren CSS-Formatvorlagen unter einer bestimmten Bedingung. Nur, wenn die Bedingung "true" ist, wird das Format für die deklarierte Objekte angewendet.

Die adaptive Renderingtechnik Flexibilität ermöglicht eine Anpassung für den Standort auf verschiedenen Geräten anzeigen. Sie können beliebig viele Formatvorlagen auf ein einzelnes Stylesheet ohne das Schreiben von Code für Geschäftslogik, sollen den Stil wählen definieren. Daher ist es sehr praktisch Möglichkeit zur Anpassung der Seite Formatvorlagen, wie er verringert jedoch die Menge der doppelten Code und die Logik auch für das Rendern von Zwecken. Andererseits, würde Auslastung der Netzwerkbandbreite erhöhen, wie die Größe der CSS-Dateien sind unwesentlich zunehmen kann.

Mithilfe der adaptiven Renderingtechnik Ihrer Website werden **ordnungsgemäß, unabhängig vom Browser angezeigt.** Sie sollten jedoch berücksichtigen, wenn die zusätzliche Bandbreite Laden ist relevant.

> [!NOTE]
> Das Grundformat einer Medienabfrage ist: @media \[Bereich: alle | handheld | drucken | Projektion | Bildschirm\] ([Eigenschaft: Wert] und... [Eigenschaft: Wert])


Beispiele für Medienabfragen: &gt;  <strong>@media alle und (max. Breite von NULL: 1000px) und (min Breite: 700px) {}:</strong> für alle Lösungen zwischen 700px und 1000px.

> <strong>@media Bildschirm und (min Breite: 400 px) und (max. Breite von NULL: 700px) {...}:</strong> nur für Bildschirme. Die Lösung muss zwischen 400 und 700px sein.
> 
> <strong>@media Handheld und (min Breite: 20em), Bildschirm und (min Breite: 20em) {...}:</strong> für Handheld-Geräte (Mobile und Geräte) und Bildschirme. Die minimale Breite muss größer als 20em sein.
> 
> Weitere Informationen hierzu finden Sie auf der [W3C-Website](http://www.w3.org/TR/css3-mediaqueries/).


Sie werden jetzt untersuchen, wie die adaptive Wiedergabe funktioniert, verbessern die Lesbarkeit des ASP.NET-MVC 4-Standard-Website-Vorlage.

1. Öffnen der **PhotoGallery.sln** Lösung, die Sie in Aufgabe 1 erstellt haben, und wählen Sie die **PhotoGallery** Projekt. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Ändern Sie die Größe des Browsers Breite, die Windows festlegen, um die Hälfte oder weniger als ein Viertel ihrer ursprünglichen Größe beträgt. Beachten Sie, was geschieht, mit den Elementen in der Kopfzeile: Einige Elemente werden nicht in den sichtbaren Bereich des Headers angezeigt.
3. Open <strong>"Site.CSS" ändern</strong> -Datei aus Visual Studio-Projektmappen-Explorer befindet sich im <strong>Content</strong> Projektordner. Drücken Sie <strong>STRG + F</strong> integrierte Suche in Visual Studio öffnen und das Schreiben <strong>@media</strong> zum Suchen der <strong>CSS Medienabfrage</strong>.

    Die Media-Abfrage-Bedingung in dieser Vorlage definierten funktioniert auf diese Weise: Wenn der Browser Fenstergröße unterschreitet **850 px**, die CSS-Regeln angewendet sind die Personen, die innerhalb dieses Blocks Medien definiert.

    ![Suchen die Medienabfrage](whats-new-in-aspnet-mvc-4/_static/image16.png "die Medienabfrage suchen")

    *Die Medienabfrage suchen*
4. Ersetzen Sie den Max-Width-Attribut-Wert, der in 850 festgelegt px mit **10px**, um das adaptive Rendering zu deaktivieren, und drücken Sie **STRG + S** um die Änderungen zu speichern. Zurück zu den Browser, und drücken Sie **STRG + F5** zur Aktualisierung von der Seite mit den Änderungen vorgenommen wurden. Beachten Sie die Unterschiede in den beiden Seiten, wenn die Breite des Fensters anpassen.

    ![In der linken Seite anwendet der @media Format, in der rechten Seite den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image17.png "In der linken Seite wendet die @media Format, in der rechten Seite den Stil ausgelassen wird")

    <em>In der linken Seite wendet die @media Formatvorlage in das Recht, das Format wird weggelassen.</em>

    Jetzt prüfen wir heraus, was auf mobilen Geräten:

    ![In der linken Seite anwendet der @media Format, in der rechten Seite den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image18.png "In der linken Seite wendet die @media Format, in der rechten Seite den Stil ausgelassen wird")

    <em>In der linken Seite wendet die @media Formatvorlage in das Recht, das Format wird weggelassen.</em>

    Obwohl Sie bemerken, dass die Änderungen beim Rendern der Seite in einem Webbrowser nicht sehr wichtig ist, sind, wenn ein mobiles Gerät verwenden werden die Unterschiede leichter ersichtlich. Auf der linken Seite des Bilds sehen wir, dass das benutzerdefinierte Format die Lesbarkeit verbessert.

    Adaptive rendern kann in viele weitere Szenarien, erleichtert es, gelten bedingte Formatvorlagen auf eine Website und Lösung von allgemeinen formatvorlagenprobleme praktisch Ansatz verwendet werden.

    Die Viewport-Meta-Tag und der CSS-Medienabfragen sind nicht spezifisch für ASP.NET MVC 4, damit Sie diese Funktionen in einer beliebigen Webanwendung nutzen können.
5. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Übung 2: Erstellen der Fotogalerie Web-Anwendung

In dieser Übung arbeiten Sie für eine Fotogalerie-Anwendung auf die Fotos anzeigen. Beginnen Sie mit der ASP.NET MVC 4-Projektvorlage, und klicken Sie dann fügen Sie eine Funktion zum Abrufen von Fotos von einem Dienst, und sie auf der Startseite anzeigen.

In der folgenden Übung aktualisieren Sie diese Lösung, um die Möglichkeit zu verbessern, die er auf mobilen Geräten angezeigt wird.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Aufgabe 1: Erstellen eines simulierten Foto-Diensts

In dieser Aufgabe erstellen Sie ein Mock des Foto-Diensts zum Abrufen des Inhalts, der im Katalog angezeigt werden soll. Zu diesem Zweck fügen Sie einen neuen Domänencontroller, der einfach eine JSON-Datei mit den Daten von jedem Foto zurückgibt.

1. Open **Visual Studio** Wenn nicht bereits geöffnet.
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual C# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **PhotoGallery**, wählen Sie einen Speicherort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**. Alternativ können Sie Arbeit fortsetzen, aus der vorhandenen ASP.NET MVC 4 **Internetanwendung** -Lösung von **Übung 1** und den nächsten Schritt überspringen.
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** -Projektvorlage, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.
4. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **App\_Daten** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**. Navigieren Sie zu der **Source\Assets\App\_Daten** Ordner dieser Anleitung und Hinzufügen der **Photos.json** Datei.
5. Erstellen Sie einen neuen Domänencontroller mit dem Namen **PhotoController**. Zu diesem Zweck mit der Maustaste auf die **Controller** wechseln Sie zum Ordner **hinzufügen** , und wählen Sie **Controller.** Führen Sie den Namen des Controllers, lassen Sie die **leerer MVC-Controller** Vorlage, und klicken Sie auf **hinzufügen**.

    ![Hinzufügen der PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "der PhotoController hinzufügen")

    *Hinzufügen der PhotoController*
6. Ersetzen Sie die **Index** -Methode durch Folgendes **Katalog** Aktion, und der Rückgabewert des Inhalts aus der JSON-Datei, die Sie vor kurzem dem Projekt hinzugefügt.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - Katalog Aktion*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Drücken Sie **F5** , führen Sie die Projektmappe, und navigieren Sie zu folgender URL, um den Mocks erstellt Foto-Dienst zu testen: `http://localhost:[port]/photo/gallery` (der Wert [Port] hängt von den aktuellen Port, an die Anwendung gestartet wurde). Die Anforderung an diese URL abrufen soll den Inhalt der **Photos.json** Datei.

    ![Testen des Diensts Mocks erstellt Foto](whats-new-in-aspnet-mvc-4/_static/image20.png "Testen des Diensts Foto Mocks erstellt")

    *Testen des Diensts Foto Mocks erstellt*

In einer echten Implementierung können Sie [ASP.NET Web API](../../../../web-api/index.md) Implementieren des Diensts Fotogalerie. ASP.NET Web-API ist ein Framework, das erleichtert das Erstellen von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Aufgabe 2: Anzeigen der Fotogalerie

In dieser Aufgabe aktualisieren Sie die Homepage, um die Fotogalerie mithilfe des mocked-Diensts in die erste Aufgabe in dieser Übung erstellten anzeigen. Fügen die Modelldateien und die Katalogsichten zu aktualisieren.

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
2. Erstellen der **Foto** -Klasse in der **Modelle** Ordner. Zu diesem Zweck mit der Maustaste auf die **Modelle** Ordner wählen **hinzufügen** , und klicken Sie auf **Klasse**. Legen Sie dann den Namen **Photo.cs** , und klicken Sie auf **hinzufügen**.
3. Fügen Sie die folgenden Elemente auf der **Foto** Klasse.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex02 - Foto Modell*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Öffnen Sie im Ordner **Controller** die Datei **HomeController.cs**.
5. Fügen Sie die folgenden using-Anweisungen hinzu.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - HomeController Using-Direktiven*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Update der **Index** zu verwendende Aktion **"HttpClient"** abgerufen Katalogdaten und verwenden Sie dann die **JavaScriptSerializer** Deserialisierung wird das Modell anzeigen.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex02 - Index-Aktion*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Öffnen der **Index.cshtml** -Datei unter der **Views\Home den** Ordner und Ersetzen Sie alle Inhalte durch den folgenden Code.

    Dieser Code durchläuft die Fotos, die vom Dienst abgerufen und in eine ungeordnete Liste angezeigt.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - Fotoliste*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **Content** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**. Navigieren Sie zu der **Source\Assets\Content** Ordner dieser Anleitung und Hinzufügen der **"Site.CSS" ändern** Datei. Sie müssen den Ersatz zu bestätigen. Wenn Sie haben die **"Site.CSS" ändern** Datei öffnen, müssen Sie bestätigen, um die Datei auch erneut laden.
9. Öffnen Sie den Datei-Explorer, und kopieren Sie die gesamte **Fotos** Ordner befindet sich unter dem **Source\Assets** Ordner dieser Anleitung in den Stammordner des Projekts im Projektmappen-Explorer.
10. Führen Sie die Anwendung aus. Jetzt sollte der Startseite anzeigen von Fotos im Katalog angezeigt werden.

    ![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")

    *Fotogalerie*
11. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Übung 3: Hinzufügen der Unterstützung für mobile Geräte

Eines der wichtigsten Updates in ASP.NET MVC 4 wird die Unterstützung für mobile-Entwicklung. In dieser Übung untersuchen Sie neue ASP.NET MVC 4-Funktionen für mobile Anwendungen durch Erweitern der PhotoGallery-Lösung, die Sie in der vorherigen Übung erstellt haben.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Aufgabe 1: Installieren von jQuery Mobile in einer ASP.NET MVC 4-Anwendung

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex3-MobileSupport/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **Package Manager Console** durch Klicken auf die **Tools** &gt; **Bibliothekspaket-Manager** &gt; **Paket-Manager Konsole** Option des Menüs.

    ![Öffnen das NuGet-Paket-Manager-Konsole](whats-new-in-aspnet-mvc-4/_static/image22.png "der NuGet-Paket-Manager-Konsole öffnen")

    *Öffnen das NuGet-Paket-Manager-Konsole*
3. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Installieren der **jQuery.Mobile.MVC** Paket.

    jQuery Mobile ist eine open Source-Bibliothek zum Erstellen von berührungsoptimiertes Web-Benutzeroberfläche. Das jQuery.Mobile.MVC NuGet-Paket enthält Hilfsprogramme zum Verwenden von jQuery Mobile für eine ASP.NET MVC 4-Anwendung.

    > [!NOTE]
    > Durch den folgenden Befehl ausführen, werden Sie die Bibliothek jQuery.Mobile.MVC von Nuget herunterladen.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Mit diesem Befehl werden installiert, jQuery Mobile und einige Helper-Dateien, einschließlich der folgenden:

    - **Ansichten/freigegeben/\_Layout.Mobile.cshtml**: ein jQuery Mobile-basierten Layout für kleinere Bildschirme optimiert ist. Wenn die Website aus einem mobilen Browser eine Anforderung empfängt, ersetzen Sie das ursprüngliche Layout (\_Layout.cshtml) durch dieses.
    - Eine ansichtumschaltung Komponente: besteht aus den **Ansichten/freigegeben/\_ViewSwitcher.cshtml** Teilansicht und **ViewSwitcherController.cs** Controller. Diese Komponente zeigt einen Link in mobilen Browsern an Benutzer in der Desktopversion von der Seite wechseln können.  
        ![Fotogalerie-Projekts mit Unterstützung für mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie-Projekts mit Unterstützung für mobile")

        *Fotogalerie-Projekts mit Unterstützung für mobile*
4. Registrieren Sie die Mobile Bündel. Öffnen Sie hierzu die **Global.asax.cs** Datei, und fügen Sie die folgende Zeile hinzu.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bündel*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Führen Sie die Anwendung, die mithilfe eines Webbrowsers desktop.
6. Öffnen der **Windows Phone 7 Emulator** befindet sich im **Startmenü | Alle Programme | Windows Phone SDK 7.1 | Windows Phone-Emulator.**
7. Öffnen Sie Internet Explorer, in der Phone-Startbildschirm. Sehen Sie sich die URL, in dem die Anwendung gestartet, und navigieren Sie zu dieser URL mit den Phone-Browser (z. B. `http://localhost:[PortNumber]/`).

    Beachten Sie, dass Ihre Anwendung im Windows Phone-Emulator unterschiedliche aussieht wie die jQuery.Mobile.MVC neue Objekte erstellt hat, im Projekt, das Anzeigen von Ansichten für mobile Geräte optimiert.

    Beachten Sie die Meldung am oberen Rand der Telefon mit den Link, der in der Desktopansicht wechselt. Darüber hinaus die  **\_Layout.Mobile.cshtml** Layouts, das vom Paket erstellt wurde, installiert ist ein anderes Layout einschließlich der in der Anwendung.

    > [!NOTE]
    > Bisher, besteht keine Verbindung zu mobile Ansicht zurückzukehren. Es wird in höheren Versionen enthalten.

    ![Mobile Ansicht der Foto Gallery Startseite](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile Ansicht der Foto-Katalog-Startseite")

    *Mobile Ansicht der Foto-Katalog-Startseite*
8. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Aufgabe 2: Erstellen von mobilen Ansichten

In dieser Aufgabe erstellen Sie eine mobile Version die Indexansicht mit Inhalt, die für eine bessere Appareance auf mobilen Geräten angepasst.

1. Kopieren der **Views\Home\Index.cshtml** anzuzeigen, und fügen Sie ihn auf eine Kopie zu erstellen, benennen Sie die neue Datei um **Index.Mobile.cshtml**.
2. Öffnen die neu erstellte **Index.Mobile.cshtml** anzuzeigen, und Ersetzen Sie die vorhandene &lt;Ul&gt; Tag mit dem folgenden Code. Durch dieses Vorgehen Sie aktualisieren die &lt;Ul&gt; Tag mit jQuery Mobile datenanmerkungen von jQuery mobile Designs zu verwenden.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Beachten Sie Folgendes:
    > 
    > - Die **Data-Role** -Attributsatz zur **Listview** wird die Liste mithilfe der Listview-Formate gerendert.
    > 
    > - Die **Daten Inset** Attributsatz auf "true" wird die Liste mit abgerundeten Rahmen und Rand angezeigt.
    > 
    > - Die **Datenfilter** -Attributsatz zur **"true"** ein Suchfeld generiert.
    > 
    > Erfahren Sie mehr über jQuery Mobile Konventionen in der Projektdokumentation für das: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Drücken Sie **STRG + S** um die Änderungen zu speichern.
4. Wechseln Sie zu der **Windows Phone-Emulator** und den Standort aktualisieren. Beachten Sie das neue Aussehen und Verhalten der Katalogliste sowie das neue Suchfeld befindet sich oben. Geben Sie dann ein Wort in das Suchfeld (z. B. **Tulpen**) So starten Sie eine Suche in der Fotogalerie.

    ![Katalog mithilfe von Listview-Stil mit Filtern](whats-new-in-aspnet-mvc-4/_static/image25.png "Katalog mithilfe von Listview-Stil mit Filtern")

    *Katalog mithilfe von Listview-Stil mit Filtern*

    Kurz gesagt, haben Sie das Rezept Ansicht Mobilizer verwendet, erstellen Sie eine Kopie der Indexansicht mit der &quot;mobile&quot; Suffix. Dieses Suffix zeigt ASP.NET MVC 4, dass jede Anforderung, die von einem mobilen Gerät generiert diese Kopie des Indexes verwendet wird. Darüber hinaus haben Sie die mobile Version von jQuery Mobile verwendet für die Website Aussehen und Verhalten auf mobilen Geräten zu verbessern, die Indexansicht aktualisiert.
5. Wechseln Sie zurück zu Visual Studio und öffnen **Site.Mobile.css** unterhalb der **Content** Ordner.
6. Korrigieren Sie die Positionierung des Titels Foto, zeigen auf der rechten Seite des Bilds zu vereinfachen. Zu diesem Zweck fügen Sie den folgenden Code aus, um die **Site.Mobile.css** Datei.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Drücken Sie **STRG + S** um die Änderungen zu speichern.
8. Wechseln Sie zurück zu den **Windows Phone-Emulator** und den Standort aktualisieren. Beachten Sie, dass der Titel Foto wird jetzt ordnungsgemäß positioniert.

    ![Auf der rechten Seite des Bilds positioniert Titel](whats-new-in-aspnet-mvc-4/_static/image26.png "auf der rechten Seite des Bilds positioniert Titel")

    *Auf der rechten Seite des Bilds positioniert Titel*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Aufgabe 3 - jQuery Mobile-Designs

Jedes Layout und die Widgets im jQuery Mobile ist für ein neues objektorientierte CSS-Framework konzipiert, die mit der eine vollständige einheitliche visuellen Entwurf Design auf Sites und Anwendungen anwenden können.

jQuery Mobile Standard Design enthält 5 Muster, die sich Buchstaben (a, b, C, d e) zur schnellen Bezugnahme.

In dieser Aufgabe aktualisieren Sie die mobile Layout für ein anderes Design als den Standardwert verwenden.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der  **\_Layout.Mobile.cshtml** -Datei im **Views\Shared**.
3. Suchen Sie das Div-Element mit den Data-Role festgelegt &quot;Seite&quot; und Aktualisieren der **Data-Theme** auf &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Drücken Sie **STRG + S** um die Änderungen zu speichern.
5. Aktualisieren Sie den Standort in der **Windows Phone-Emulator** , und beachten Sie das neue Farbschema.

    ![Mobile Layout mit einem anderen Farbschema](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile Layout mit einem anderen Farbschema")

    *Mobile Layout mit einem anderen Farbschema*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Aufgabe 4: Verwenden der Ansichtumschaltung Komponente und den Browser Überschreiben von Funktionen

Eine Konvention für Mobilgeräte optimierte Webseiten besteht darin, einen Link hinzufügen, dessen Text ist etwas wie Desktopansicht oder vollständige standortmodus, in dem Benutzer, die in einer desktop-Version von der Seite wechseln kann. Das jQuery.Mobile.MVC-Paket enthält ein Beispiel für **ansichtumschaltung** -Komponente für diesen Zweck verwendet werden, der  **\_Layout.Mobile.cshtml** anzeigen.

![Link zur Ansicht Desktop wechseln](whats-new-in-aspnet-mvc-4/_static/image28.png "Link, um zur Desktop-Ansicht wechseln")

*Link zum Desktop-Ansicht wechseln*

Ansichtumschaltung verwendet ein neues Feature namens **Browser überschreiben**. Mit diesem Feature können Ihre Anwendung Anforderungen zu behandeln, als ob sie von einem anderen Browser (Benutzer-Agent) als dem gestellt würden, die tatsächlich von kommen.

In dieser Aufgabe untersuchen Sie die beispielimplementierung von einem ansichtumschaltung, indem jQuery.Mobile.MVC und dem neuen Browser Überschreiben von Funktionen in ASP.NET MVC 4 hinzugefügt.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der  **\_Layout.Mobile.cshtml** Ansicht befindet sich unter der **Views\Shared** Ordner, und beachten Sie, dass der ansichtumschaltung Komponente als eine Teilansicht verwiesen wird.

    ![Mobile Layouts mit den Ansichtumschaltung Komponente](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile Layouts mit den Ansichtumschaltung Komponente")

    *Mobile Layouts mit den Ansichtumschaltung Komponente*
3. Öffnen der  **\_ViewSwitcher.cshtml** Teilansicht.

    Die Teilansicht verwendet die neue Methode **ViewContext.HttpContext.GetOverriddenBrowser()** zum Ermitteln des Ursprungs von der webanforderung und zeigen den entsprechenden Link, um entweder mit dem Desktop oder Mobile-Ansichten zu wechseln.

    Die **GetOverridenBrowser** Methode gibt ein **HttpBrowserCapabilitiesBase** -Instanz, die der Benutzer-Agent, die derzeit für die Anforderung festgelegt entspricht (tatsächlichen oder überschrieben). Sie können diesen Wert verwenden, beim Abrufen von Eigenschaften wie z. B. **IsMobileDevice**.

    ![Teilansicht ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher Teilansicht")

    *Teilansicht ViewSwitcher*
4. Öffnen der **ViewSwitcherController.cs** Klasse befindet sich in der **Controller** Ordner. Sehen Sie sich diese SwitchView Aktion wird aufgerufen, indem Sie den Link in der ViewSwitcher-Komponente, und beachten Sie die neuen HttpContext-Methoden.

    - Die **HttpContext.ClearOverridenBrowser()** -Methode entfernt alle außer Kraft gesetzten Benutzer-Agent für die aktuelle Anforderung.
    - Die **HttpContext.SetOverridenBrowser()** Methode überschreibt die Anforderung tatsächlichen Benutzer-Agent-Wert, der mit dem angegebenen Benutzer-Agent.  
        ![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher-Controller")  
*ViewSwitcher-Controller*

        Browser überschreiben ist eine grundlegende Funktion von ASP.NET MVC 4, das ist auch verfügbar, auch wenn Sie nicht das jQuery.Mobile.MVC-Paket installieren. Allerdings diese Funktion wirkt sich auf nur anzeigen, Layout und Speicherortformate der Teilansicht, und er hat keine Auswirkungen auf eine der Funktionen, die vom Request.Browser Objekt abhängig sind.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Aufgabe 5: Hinzufügen der Ansichtumschaltung in der Desktopansicht

In dieser Aufgabe aktualisieren Sie den desktop Layout zum ansichtumschaltung-enthalten. Dadurch können für mobile Benutzer, mobile Ansicht zurückzukehren, beim Durchsuchen von desktop anzeigen.

1. Aktualisieren Sie den Standort in der **Windows Phone-Emulator**.
2. Klicken Sie auf die **Desktopansicht** Link am Anfang des Katalogs. Beachten Sie, dass keine ansichtumschaltung vorliegt, in der desktop können Sie zulassen, dass Sie an die mobile Ansicht zurückgeben.
3. Wechseln Sie zurück zu Visual Studio und Öffnen der  **\_Layout.cshtml** anzeigen.
4. Suchen Sie den anmeldeabschnitt, und fügen Sie einen Aufruf zum Rendern der  **\_ViewSwitcher** Teilansicht unten die  **\_LogOnPartial** Teilansicht. Drücken Sie anschließend die **STRG + S** um die Änderungen zu speichern.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Drücken Sie **STRG + S** um die Änderungen zu speichern.
6. Aktualisieren Sie die Seite in der Windows Phone-Emulator aus, und doppelklicken Sie auf dem Bildschirm, um zu vergrößern. Beachten Sie, die auf der Startseite jetzt zeigt die **Mobile Ansicht** Link, der von mobilen in Desktopansicht wechselt.

    ![Der Darstellung in der Desktopansicht gerendert](whats-new-in-aspnet-mvc-4/_static/image32.png "Ansichtumschaltung in desktop-Ansicht gerendert")

    *In der Desktopansicht gerendert Ansichtumschaltung*
7. Wechseln Sie zur mobilen Ansicht erneut aus, und navigieren Sie zu <strong>zu</strong> Seite (http://localhost[Port]/Home/About). Beachten Sie, dass, selbst wenn Sie eine Ansicht About.Mobile.cshtml erstellt haben, mit der mobilen Layout der Seite "Info" angezeigt wird (\_Layout.Mobile.cshtml).

    ![Zu den Seiten](whats-new-in-aspnet-mvc-4/_static/image33.png "zu Seiten")

    *Zu den Seiten*
8. Schließlich werden öffnen Sie die Website in einem desktop Webbrowser. Beachten Sie, dass keine der vorherigen Updates der Desktopansicht betroffen ist.

    ![PhotoGallery Desktopansicht](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery Desktopansicht")

    *PhotoGallery Desktopansicht*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Aufgabe 6: Erstellen einer neuen Anzeigemodi

Das neue Anzeige Modi Feature ermöglicht es sich um eine Anwendung, die Sichten je nach Browser auszuwählen, die die Anforderung generiert wird. Z. B. ein Desktopbrowser auf der Startseite "angefordert, die Anwendung zurück die **Views\Home\Index.cshtml** Vorlage. Klicken Sie dann, wenn ein mobiler Browser auf der Startseite anfordert, die Anwendung zurück die **Views\Home\Index.mobile.cshtml** Vorlage.

In dieser Aufgabe erstellen Sie ein benutzerdefiniertes Layout für iPhone-Geräte und müssen Anforderungen aus der iPhone-Geräte zu simulieren. Zu diesem Zweck können Sie entweder einen iPhone Emulator/Simulator (z. B. [Electric Mobile Simulator](http://www.electricplum.com/)) oder einen Browser mit Add-Ons anzuzeigen, die den Benutzer-Agent zu ändern. Anweisungen zum Festlegen der Benutzer-Agent-Zeichenfolge in einem Safaribrowser auf einem iPhone zu emulieren, finden Sie unter [Vorgehensweise können Sie vorgeben, es handelt sich um Internet Explorer Safari](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) im Blog von David Alison des.

**Beachten Sie, dass diese Aufgabe optional ist, und Sie im Labor fortsetzen können, ohne Sie auszuführen.**

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
2. Open **Global.asax.cs** und fügen Sie die folgende Anweisung verwenden.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Fügen Sie den folgenden hervorgehobenen Code in die Anwendung\_Start-Methode.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex03 - iPhone "DisplayMode"*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Sie haben ein neues registriert **DefaultDisplayMode mit dem Namen &quot;iPhone&quot;**, innerhalb der statischen **DisplayModeProvider.Instance.Modes** statische Liste verglichen wird Jede eingehende Anforderung. Wenn die eingehende Anforderung die Zeichenfolge enthält &quot;iPhone&quot;, ASP.NET MVC findet die Ansichten enthalten, deren Name die &quot;iPhone&quot; Suffix. Der Parameter 0 gibt an, wie bestimmte der neue Modus ist; in dieser Ansicht ist z. B. genauer als die allgemeinen &quot;.mobile&quot; Regel, die Anfragen von mobilen Geräten entspricht.

Nachdem dieser Code ausgeführt, wenn ein iPhone-Browser eine Anforderung generiert, wird die Anwendung verwendet die **Views\Shared\\_Layout.iPhone.cshtml** Layout Sie in den nächsten Schritten erstellen.

> [!NOTE]
> Auf diese Weise testen Sie die Anforderung für iPhone zu Demonstrationszwecken vereinfacht wurde und möglicherweise nicht funktionieren erwartungsgemäß für jede iPhone User-Agent-Zeichenfolge (zum Beispiel Test Groß-/Kleinschreibung berücksichtigt wird).

4. Erstellen Sie eine Kopie der  **\_Layout.Mobile.cshtml** in der Datei die **Views\Shared** Ordner und benennen Sie die Kopie um &quot; **\_Layout.iPhone.csthml**&quot;.
5. Open  **\_Layout.iPhone.csthml** Sie im vorherigen Schritt erstellt haben.
6. Suchen Sie das Div-Element mit dem Data-Role-Attributsatz **Seite** , und ändern Sie die **Data-Theme** -Attribut auf &quot; **eine**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Sie haben jetzt 3 Layouts in Ihre ASP.NET MVC 4-Anwendung:

1. **\_Layout.cshtml**: Standardlayout Desktopbrowsern verwendet.
2. **\_Layout.Mobile.cshtml**: Standardlayout für mobile Geräte verwendet.
3. **\_Layout.iPhone.cshtml**: bestimmten Layout für iPhone-Geräte, die mit einem anderen Farbschema zur Unterscheidung von \_Layout.mobile.cshtml.
7. Drücken Sie **F5** führen Sie die Anwendung, und suchen den Standort in der **Windows Phone-Emulator**.
8. Öffnen einer **iPhone-Simulator** (finden Sie unter [Anhang C](#AppendixC) Anweisungen zum Installieren und konfigurieren einen iPhone-Simulator), und wechseln Sie zur Website zu. Beachten Sie, dass die betreffende Vorlage jedes Telefon verwendet.

    ![Using-different-Views-for-each-Mobile-Device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Verwenden verschiedene Ansichten für jedes mobile Gerät*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Übung 4: Verwenden von asynchronen Controller

Microsoft .NET Framework 4.5 führt neue Sprachfeatures in c# und Visual Basic um ein neues Fundament für Asynchronie in der .NET-Programmierung bereitzustellen. Diese neue Foundation stellt asynchrone Programmierung, - ähnelt und ungefähr so einfach wie das - synchronen Programmierung. Sie können nun schreiben Sie asynchrone Aktionsmethoden in ASP.NET MVC 4 mithilfe der **AsyncController im Vergleich zum** Klasse. Sie können asynchrone Aktionsmethoden für lang andauernde, nicht CPU-gebundene Anforderungen. Dadurch wird vermieden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird. AsyncController im Vergleich zum Klasse dient normalerweise für lang andauernde Webdienstaufrufe.

In dieser Übung erläutert die Grundlagen des asynchronen Vorgangs in ASP.NET MVC 4. Wenn Sie eine Vertiefung möchten, können Sie die folgenden Artikel auschecken: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Aufgabe 1: Implementieren eines asynchronen Controllers

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex4-Async/Begin/** Ordner. Andernfalls möglicherweise weiterhin mithilfe der **End** Lösung abgerufen, von der vorherigen Übung abschließen.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung müssen Sie einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Öffnen der **HomeController.cs** -Klasse aus den **Controller** Ordner.
3. Fügen Sie die folgende Anweisung verwenden.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Update der **HomeController** Klasse zu vererben **AsyncController im Vergleich zum**. Domänencontroller, auf denen Ableitung AsyncController im Vergleich zum Aktivieren von ASP.NET zur Verarbeitung von asynchronen Anforderungen, und sie können weiterhin Dienst synchrone Aktionsmethoden.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Hinzufügen der **Async** Schlüsselwort, um die **Index** Methode, und stellen sie den Rückgabetyp **Aufgabe&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Die **Async** Schlüsselwort ist eines der .NET Framework 4.5 bietet neue Schlüsselwörter; er weist den Compiler, dass diese Methode asynchronen Code enthält. Ein **Aufgabe** Objekt stellt einen asynchronen Vorgang, die in der Zukunft späteren Zeitpunkt abgeschlossen werden kann.
6. Ersetzen Sie die **Client. GetAsync()** Aufruf mit der vollständigen asynchronen Version "await" Schlüsselwort aus, wie unten dargestellt.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > In der vorherigen Version wurden mithilfe der **Ergebnis** Eigenschaft aus der **Aufgabe** Objekt, das den Thread zu blockieren, bis das Ergebnis (Version von Sync) zurückgegeben wird.
    > 
    > Hinzufügen der **"await"** -Schlüsselwort weist den Compiler an, die vom Methodenaufruf zurückgegebene Aufgabe asynchron zu warten. Dies bedeutet, dass der Rest des Codes als Rückruf ausgeführt wird, nachdem die erwartete Methode abgeschlossen hat. Außerdem ist zu beachten ist, dass Sie nicht benötigen, um den Try / Catch-Block zu ändern, um dieses: die Ausnahmen, die im Hintergrund oder im Vordergrund stattfinden werden weiterhin ohne zusätzliche Arbeit mit einem Handler, die vom Framework bereitgestellten abgefangen werden.
7. Ändern Sie den Code auf die asynchrone Implementierung fortsetzen, durch die Zeilen mit den neuen Code ersetzen, wie unten dargestellt

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Führen Sie die Anwendung aus. Sehen Sie keine größeren Änderungen, aber der Code wird nicht blockiert einen Thread aus dem Threadpool, erstellen eine bessere Nutzung von Serverressourcen und Verbessern der Leistung.

    > [!NOTE]
    > Erfahren Sie mehr über die neuen Funktionen der asynchronen Programmierung im Labor &quot; **asynchrone Programmierung in .NET 4.5 mit c# und Visual Basic** &quot; in der Visual Studio-Training Kit enthalten.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Aufgabe 2: Behandeln von Timeouts mit Abbruchtoken

Asynchrone Aktionsmethoden, die Aufgabeninstanzen zurückgeben können auch Timeouts unterstützen. In dieser Aufgabe aktualisieren Sie die Index-Methodencode eine Timeout-Szenario, das Verwenden eines Abbruchtokens zu behandeln.

1. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.
2. Fügen Sie die folgenden using-Anweisungen die **HomeController.cs** Datei.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aktualisieren Sie die Index-Aktion empfangen einer **CancellationToken** Argument.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Update der **GetAsync** Aufruf an das Abbruchtoken übergeben.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex04 - "SendAsync" mit CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Ergänzen der *Index* Methode mit einer **AsyncTimeout** -Attribut auf 500 Millisekunden festgelegt und ein **HandleError** Attribut zur Verarbeitung konfiguriert  **TaskCanceledException** durch Umleiten an einen **"timedOut"** anzeigen.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex04 - Attribute*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Öffnen der **PhotoController** Klasse und Aktualisieren der **Katalog** Methode, um die Ausführung von 1000 Millisekunden (1 Sekunde) zum Simulieren einer lang ausgeführten Aufgabe zu verzögern.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Öffnen der **"Web.config"** Datei, und aktivieren Sie benutzerdefinierte Fehler, indem Sie das folgende Element hinzufügen.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Erstellen einer neuen Ansicht in **Views\Shared** mit dem Namen **"timedOut"** und verwenden Sie das Standardlayout. Klicken Sie im Projektmappen-Explorer mit der Maustaste die **Views\Shared** Ordner, und wählen **hinzufügen | Ansicht**.

    ![Verwenden verschiedene Ansichten für jedes mobile Gerät](whats-new-in-aspnet-mvc-4/_static/image36.png "verschiedene Ansichten für jedes mobile Gerät verwenden.")

    *Verwenden verschiedene Ansichten für jedes mobile Gerät*
9. Update der **"timedOut"** Inhalte anzuzeigen, wie unten dargestellt.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Führen Sie die Anwendung, und suchen Sie die Stamm-URL. Wie Sie hinzugefügt haben eine **Thread.Sleep** von 1000 Millisekunden, erhalten Sie ein Timeoutfehler von generiert die **AsyncTimeout** Attribut, und abfängt, indem Sie die **HandleError** Das Attribut.

    ![Timeout-Ausnahme behandelt](whats-new-in-aspnet-mvc-4/_static/image37.png "Timeoutausnahme behandelt")

    *Timeoutausnahme behandelt wird*

> [!NOTE]
> Darüber hinaus können Sie die Bereitstellung dieser Anwendung, die Windows Azure-Websites folgenden [Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In dieser praktische Übung-on-Übungseinheit haben Sie einige der neuen Funktionen in ASP.NET MVC 4 residenten beobachtet. Die folgenden Konzepte haben behandelt wurde:

- Nutzen Sie die Erweiterungen der für die ASP.NET MVC-Projekt Vorlagen, einschließlich der neue Projektvorlage für die mobile Anwendung
- Verwenden Sie das HTML5 Viewport-Attribut und der CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern
- Verwenden Sie für progressiven Verbesserungen und zum Erstellen von Web-Touch-optimierte Benutzeroberfläche jQuery Mobile
- Mobile-spezifischen Ansichten erstellen
- Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente
- Erstellen von asynchronen Controller Dank Unterstützung der Aufgabe

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code, den Sie jederzeit griffbereit benötigen. Das Lab-Dokument erfahren Sie, wenn Sie genau, verwenden können wie in der folgenden Abbildung dargestellt.

![Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt](whats-new-in-aspnet-mvc-4/_static/image38.png "Codeausschnitte zum Einfügen von Code in Ihr Projekt mit Visual Studio")

*Verwendung von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihr Projekt*

***Hinzufügen ein Codeausschnitts, die mithilfe der Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo möchten Sie den Code einfügen.
2. Starten Sie den Namen des Ausschnitts (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmenden Namen Ausschnitte.
4. Wählen Sie den richtigen Ausschnitt (oder behalten Sie eingeben, bis der gesamte Ausschnitt Name ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal, um den Ausschnitt an der Cursorposition eingefügt.

![Geben Sie den Namen des Ausschnitts](whats-new-in-aspnet-mvc-4/_static/image39.png "starten Sie den Namen des Ausschnitts eingeben")

*Starten Sie den Namen des Ausschnitts eingeben*

![Drücken Sie Tab, auf den hervorgehobenen Ausschnitt](whats-new-in-aspnet-mvc-4/_static/image40.png "drücken Sie die Tab hervorgehobenen Ausschnitt auswählen")

*Drücken Sie Tab, um den markierten Ausschnitt auszuwählen*

![Drücken Sie erneut die Tab und den Ausschnitt erweitert](whats-new-in-aspnet-mvc-4/_static/image41.png "drücken Sie erneut die Tab und den Ausschnitt erweitert")

*Drücken Sie erneut die Tab und den Ausschnitt erweitert*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)***

1. Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen möchten.
2. Wählen Sie **Ausschnitt einfügen** gefolgt von **eigene Codeausschnitte**.
3. Wählen Sie die relevanten Ausschnitt aus der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](whats-new-in-aspnet-mvc-4/_static/image42.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken](whats-new-in-aspnet-mvc-4/_static/image43.png "den relevanten Ausschnitt aus der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie den relevanten Ausschnitt aus der Liste, indem Sie darauf klicken*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Anhang C: Installieren von WebMatrix 2 und in der iPhone-Simulator

Um Ihre Website in einem simulierten iPhone-Gerät ausführen können Sie die WebMatrix-Erweiterung &quot;Electric Mobile-Simulator für das iPhone&quot;. Darüber hinaus können Sie die gleiche Erweiterung zum Ausführen des Simulators aus Visual Studio 2012 konfigurieren.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Aufgabe 1: Installieren von WebMatrix 2

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; <em>WebMatrix 2</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2 installieren")

    *Installieren von WebMatrix 2*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image50.png "akzeptieren der Lizenzbedingungen")

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image51.png "Installationsstatus")

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation abgeschlossen")

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Aufgabe 2: installieren die iPhone-Simulator-Erweiterung

1. Führen Sie **WebMatrix** und ggf. eine vorhandene Website öffnen oder erstellen Sie ein neues Konto.
2. Klicken Sie auf die **ausführen** Schaltfläche aus der **Startseite** , und wählen Sie im Menüband **Add new**.

    ![Hinzufügen von neuen WebMatrix-Erweiterung](whats-new-in-aspnet-mvc-4/_static/image53.png "neue WebMatrix-Erweiterung hinzufügen")

    *Hinzufügen von neuen WebMatrix-Erweiterung*
3. Wählen Sie **iPhone-Simulator** , und klicken Sie auf **installieren**.

    ![Durchsuchen von WebMatrix Erweiterungen](whats-new-in-aspnet-mvc-4/_static/image54.png "Durchsuchen von WebMatrix-Erweiterungen")

    *Durchsuchen von WebMatrix-Erweiterungen*
4. Klicken Sie in die Paketdetails auf **installieren** dem Fortsetzen der Installation der Erweiterung.

    ![iPhone-Simulator Erweiterung](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone-Simulator-Erweiterung")

    *iPhone-Simulator-Erweiterung*
5. Lesen Sie und akzeptieren Sie die EULA-Erweiterung.

    ![WebMatrix-Erweiterung EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix-Erweiterung EULA")

    *WebMatrix-Erweiterung EULA*
6. Jetzt können Sie Ihre Website von WebMatrix ausführen, mit der iPhone-Simulator-Option.

    ![Führen Sie mithilfe von iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "mit iPhone ausgeführt")

    *Führen Sie mithilfe von iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Aufgabe 3: Konfigurieren von Visual Studio 2012 zum iPhone-Simulator ausführen

1. Öffnen Sie **Visual Studio 2012** und eine beliebige Website öffnen oder erstellen Sie ein neues Projekt.
2. Klicken Sie auf den Pfeil nach unten über die Schaltfläche "ausführen", und wählen Sie **Browserauswahl**.

    ![Browserauswahl](whats-new-in-aspnet-mvc-4/_static/image58.png "Browserauswahl")

    *Browserauswahl*
3. In der &quot;Browserauswahl&quot; Dialogfeld klicken Sie auf **hinzufügen**.
4. In der &quot;Programm hinzufügen&quot; Dialogfeld, verwenden Sie die folgenden Werte:

   - <strong>Programm</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (Aktualisieren Sie den Pfad entsprechend)</em>
   - **Argumente**: &quot;1&quot;
   - **Anzeigename**: iPhone-Simulator

     ![Programm hinzufügen](whats-new-in-aspnet-mvc-4/_static/image59.png "Programm hinzufügen")

     *Hinzufügen eines Programms mit Durchsuchen*
5. Klicken Sie auf **OK** und schließen Sie alle Dialogfelder.
6. Jetzt können Sie Ihre Webanwendungen in der iPhone-Simulator von Visual Studio 2012 ausführen.

    ![Navigieren Sie mit der iPhone-Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browserauswahl iPhone-Simulator")

    *Navigieren Sie mit der iPhone-Simulator*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

In diesem Anhang wird gezeigt, wie eine neue Website aus dem Windows Azure-Verwaltungsportal erstellen und veröffentlichen Sie die Anwendung, die Sie erhalten haben anhand der testumgebung profitieren von der Web Deploy Veröffentlichungsfunktion von Windows Azure bereitgestellt werden.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Sie können mit Windows Azure hosten 10 ASP.NET-Websites kostenlos und skalieren Sie, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** in der Befehlszeile.

    ![Erstellen einer neuen Website](whats-new-in-aspnet-mvc-4/_static/image62.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **berechnen** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Verfügbare URL für die neue Website bereitstellen, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Eine Windows Azure-Website ist der Host für eine Webanwendung, die ausgeführt wird, in der Cloud, die Sie steuern und verwalten können. Die Option Schnellerfassung können Sie eine vollständige Webanwendung, die Windows Azure-Website von außerhalb des Portals bereitstellen. Es umfasst nicht die Schritte zum Einrichten einer Datenbank.

    ![Erstellen eine neue Website, die mit der Schnellerfassung](whats-new-in-aspnet-mvc-4/_static/image63.png "erstellen eine neue Website, die mit der Schnellerfassung")

    *Erstellen eine neue Website, die mit der Schnellerfassung*
4. Warten Sie, bis die neue **Website** wird erstellt.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website funktioniert.

    ![Um die neue Website durchsuchen](whats-new-in-aspnet-mvc-4/_static/image64.png "durchsuchen, um die neue Website")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-aspnet-mvc-4/_static/image65.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-mvc-4/_static/image66.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite der **kurzer Blick** auf die **Herunterladen eines Veröffentlichungsprofils** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die erforderlich sind, eine Verbindung herstellen und die Authentifizierung für alle Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung beim Lesen von veröffentlichungsprofilen zum Automatisieren der Konfiguration dieser Programme für Veröffentlichung von Webanwendungen auf Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image67.png "der Website herunterladen eines Veröffentlichungsprofils")

    *Die Website herunterladen eines Veröffentlichungsprofils*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort ein. In dieser Übung wird weiter Gewusst wie: Verwenden Sie diese Datei zum Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image68.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: konfigurieren den Datenbankserver

Wenn die Anwendung durchführt, verwenden Sie SQL Server Datenbanken Sie einen SQL-Datenbankserver erstellen müssen. Wenn eine einfache Anwendung bereitstellen, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbankserver zum Speichern der Anwendungsdatenbank. Sehen Sie die SQL-Datenbankservern über Ihr Abonnement in Windows Azure-Verwaltungsportal am **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie keinen Server erstellt haben, können Sie erstellen, mit der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie die **Servername und -URL, Administrator-Benutzername und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie er in einer späteren Phase erstellt wird.

    ![SQL-Datenbank-Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL-Datenbank-Dashboard")

    *SQL-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, aus Visual Studio aus diesem Grund Sie die lokale IP-Adresse in der Serverliste des müssen **zulässigen IP-Adressen**. Klicken Sie hierzu auf **konfigurieren**, wählen Sie die IP-Adresse von **aktuelle Client-IP-Adresse** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **IP-Clientadresse** wird zu den zulässigen IP-Adressen hinzugefügt Liste, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Bestätigen von Änderungen*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mithilfe von Web Deploy

1. Wechseln Sie zurück, die ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-mvc-4/_static/image73.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren des Veröffentlichungsprofils](whats-new-in-aspnet-mvc-4/_static/image74.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Überprüfung ist beendet, sobald Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Validate Connection" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-aspnet-mvc-4/_static/image75.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite der **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbank Server-URL unter Verwendung der *Tcp:* Präfix.
   - In **Benutzername** Geben Sie Ihre Administrator Serveranmeldenamen an.
   - In **Kennwort** Geben Sie Ihre Server-Administratorkennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Zielverbindungszeichenfolge](whats-new-in-aspnet-mvc-4/_static/image77.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen des Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-mvc-4/_static/image78.png "erstellen die Datenbank-Zeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie verwenden für die Verbindung mit SQL-Datenbank in Windows Azure ist innerhalb der Verbindung standardmäßig Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Verbindungszeichenfolge zur SQL-Datenbank zeigt](whats-new-in-aspnet-mvc-4/_static/image79.png "Verbindungszeichenfolge zur SQL-Datenbank zeigt")

    *Verbindungszeichenfolge, die auf der SQL-Datenbank*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-mvc-4/_static/image80.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, wird der Standardbrowser veröffentlichten Website geöffnet.

    ![Anwendung in Windows Azure veröffentlicht](whats-new-in-aspnet-mvc-4/_static/image81.png "Veröffentlichung der Anwendung in Windows Azure")

    *Anwendung, die auf Windows Azure veröffentlicht werden soll*
