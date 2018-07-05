---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: Was ist neu in ASP.NET MVC 4 | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, auf Standards basierende Webanwendungen, die mit bewährte Entwurfsmuster und die Leistung von ASP.NET und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8862c4da0d881a6f1084317e08697354c0ae6d48
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374103"
---
# <a name="whats-new-in-aspnet-mvc-4"></a>Neuerungen in ASP.NET MVC 4

durch [Web Camps Team](https://twitter.com/webcamps)

[Herunterladen Sie Web Camps Training Kit](https://aka.ms/webcamps-training-kit)

ASP.NET MVC 4 ist ein Framework zum Erstellen von skalierbaren, auf Standards basierende Webanwendungen mit bewährte Entwurfsmuster und die Leistung von ASP.NET und .NET Framework. Diese neue, vierte Version des Frameworks besteht darin, einfacher Entwicklung von mobilen Webanwendungen.

Zunächst bei der Erstellung eines neuen ASP.NET MVC 4-Projekts besteht jetzt eine mobile Anwendung-Projektvorlage, die Sie verwenden können, um eine eigenständige app speziell für mobile Geräte zu erstellen. Darüber hinaus wird die ASP.NET MVC 4 in jQuery Mobile über ein jQuery.Mobile.MVC NuGet-Paket integriert. jQuery Mobile ist ein HTML5-basierten Framework für die Entwicklung von Web-apps, die mit allen gängigen mobilen Geräteplattformen, einschließlich Windows Phone, iPhone, Android usw. kompatibel sind. Wenn Sie die Spezialisierung benötigen, ermöglicht Sie dienen verschiedene Ansichten für verschiedene Geräte und gerätespezifische Optimierungen jedoch auch ASP.NET MVC 4.

In dieser praktischen Übungseinheit beginnen Sie mit der ASP.NET MVC 4 &quot;Internetanwendung&quot; Projektvorlage aus, um eine Fotogalerie-Anwendung zu erstellen. Sie werden progressiv verbessern, die app mithilfe von jQuery Mobile und neuen Features von ASP.NET MVC 4, um es mit anderen mobilen Geräten und desktop-Webbrowser kompatibel zu machen. Außerdem lernen Sie neuen Code Know-how für die codegenerierung und wie ASP.NET MVC 4 asynchrone Aktionsmethoden schreiben, durch die Unterstützung von Aufgabe vereinfacht&lt;ActionResult&gt; Rückgabetypen.

> [!NOTE]
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [Versionen der Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Das Projekt, das speziell für dieses Lab finden Sie unter [Neuigkeiten in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Profitieren Sie von den Verbesserungen bei der ASP.NET MVC-Projekt-Vorlagen, einschließlich der neuen Projektvorlage für mobile Anwendungen
- Verwenden Sie den Viewport-Attribut für HTML5 und CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern
- Verwenden von jQuery Mobile für progressive Verbesserungen und zur Erstellung von Touch-optimierte Web-Benutzeroberfläche
- Erstellen von Ansichten
- Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente
- Erstellen Sie asynchrone Controller mithilfe des Task-Unterstützung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang B](#AppendixB) Anleitungen zur Installation).
- [ASP.NET MVC 4](../../../mvc4.md) (enthalten in der Microsoft Visual Studio 2012-Installation)
- Windows Phone-Emulator (enthalten der [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))
- Optional: [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) mit **Electric Plum iPhone-Simulator** -Erweiterung (nur für Übung 3 verwendet, um die Webanwendung mit einem iPhone-Simulator durchsuchen)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Setup

In diesem Dokument Lab werden Sie aufgefordert, zum Einfügen von Codeblöcken. Der Einfachheit halber die meisten dieser Code dient als Visual Studio-Codeausschnitten, die Sie von innerhalb von Visual Studio verwenden können, um zu vermeiden, müssen sie manuell hinzufügen.

So installieren Sie die Codeausschnitte

1. Öffnen Sie ein Windows-Explorer-Fenster, und navigieren Sie zu des Labs die **Source\Setup** Ordner.
2. Doppelklicken Sie auf die **"Setup.cmd"** Datei in diesem Ordner die Codeausschnitte für Visual Studio zu installieren.

Wenn Sie nicht mit dem Visual Studio Code Snippets und zu erfahren, wie Sie deren Verwendung vertraut sind, sehen Sie sich im Anhang in diesem Dokument &quot; [Anhang A: Verwenden von Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Neue ASP.NET MVC 4-Projektvorlagen](#Exercise1)
2. [Erstellen der Fotogalerie-Webanwendung](#Exercise2)
3. [Hinzufügen von Unterstützung für Mobile Geräte](#Exercise3)
4. [Verwenden asynchrone Controller](#Exercise4)

> [!NOTE]
> Jede Übung umfasst eine **End** Ordner mit der resultierenden Lösung, die Sie nach Abschluss der Übungen abrufen soll. Sie können diese Lösung als Leitfaden verwenden, bei Bedarf zusätzliche Hilfe bei der die Übungen durcharbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **60 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a>Übung 1: Neue ASP.NET MVC 4-Projektvorlagen

In dieser Übung untersuchen Sie die Verbesserungen in den Vorlagen für ASP.NET MVC 4-Projekt. Zusätzlich zum Internet Application-Vorlage enthält in MVC 3 bereits vorhanden., diese Version nun eine getrennte Vorlage für Mobile Anwendungen. Zunächst sehen Sie sich einige relevante Features der einzelnen Vorlagen. Klicken Sie dann arbeiten Sie auf das rendering Ihrer Seite ordnungsgemäß auf die verschiedenen Plattformen mithilfe des richtigen Ansatzes.

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a>Aufgabe 1: Untersuchen der Internetanwendungsvorlage

1. Open **Visual Studio**.
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual c# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **Registerseite**, wählen Sie einen Standort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**.

    > [!NOTE]
    > Später passen Sie die Registerseite ASP.NET MVC 4-Lösung, die Sie jetzt erstellen.

    ![Erstellen eines neuen Projekts](whats-new-in-aspnet-mvc-4/_static/image1.png "Erstellen eines neuen Projekts")

    *Erstellen eines neuen Projekts*
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** Projektvorlage aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.

    ![Erstellen einer neuen ASP.NET MVC 4 Internetanwendung](whats-new-in-aspnet-mvc-4/_static/image2.png "Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Internet-Anwendung*

    > [!NOTE]
    > Razor-Syntax wurde in ASP.NET MVC 3 eingeführt wurde. Das Ziel ist, die die Anzahl von Zeichen und Tastaturanschläge erforderlich sind in einer Datei, und Aktivieren einer schnell und fließend, die Codierung von Workflows zu minimieren. Razor nutzt vorhandene C#-/ VB (oder andere) Fähigkeiten und bietet eine Vorlage Markupsyntax, die einen tollen HTML-Konstruktion Workflow ermöglicht.
4. Drücken Sie **F5** auf erneuerten Vorlagen anzuzeigen und die Projektmappe auszuführen. Sie können Sie die folgenden Features überprüfen:

    **Modernes-Vorlagen**

    Die Vorlagen haben erneuert wurde, weitere Modern aussehende Formate bereitstellt.

    ![Vorlagen für ASP.NET MVC 4 neu gestaltet](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 neu gestaltet Vorlagen")

    *Vorlagen für ASP.NET MVC 4 neu gestaltet*

    ![Neue Seite "Kontakt"](whats-new-in-aspnet-mvc-4/_static/image4.png "Seite Neuer Kontakt")

    *Neue Seite "Kontakt"*

    **Adaptive Rendering**

    Sehen Sie sich die Größe des Browserfensters, und beachten Sie, wie das Seitenlayout passen diese dynamisch auf die neue Fenstergröße an. Diese Vorlagen mithilfe der adaptiven Renderingtechnik um Desktop- und mobile Plattformen ohne Anpassung ordnungsgemäß zu rendern.

    ![ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.")

    *ASP.NET MVC 4-Projektvorlage in Größen von anderen Browser.*

    **Umfangreichere Benutzeroberfläche mit JavaScript**

    Eine weitere Verbesserung von Standardprojektvorlagen ist die Verwendung von JavaScript, um eine interaktivere JavaScript bereitzustellen. Die Links anmelden und registrieren, die in der Vorlage verwendete beispielhaft wie die jQuery-Überprüfungen zu verwenden, um die Eingabefelder von clientseitigen zu überprüfen.

    ![jQuery-Validierung](whats-new-in-aspnet-mvc-4/_static/image6.png)

    *jQuery-Validierung*

    > [!NOTE]
    > Beachten Sie, dass die beiden in Abschnitten im ersten Abschnitt melden Sie sich, dass kann melden Sie sich mit einem Konto registriert, von der Website und im zweiten Abschnitt, den Sie Altenativelly, melden Sie sich mit der ein anderer Authentifizierungsdienst wie Google (standardmäßig deaktiviert können).
5. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
6. Öffnen Sie die Datei **AuthConfig.cs** befindet sich unter dem **App\_starten** Ordner.
7. Entfernen Sie den Kommentar der letzten Zeile zum Registrieren des Google-Client für *OAuth* Authentifizierung.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > Beachten Sie, dass Sie die Authentifizierung über alle OpenID als auch die OAuth-Dienst wie Facebook, Twitter, Microsoft leicht aktivieren können.
8. Drücken Sie **F5** führen Sie die Projektmappe, und navigieren zur Anmeldeseite.
9. Wählen Sie **Google** Dienst zum Anmelden.

    ![Das Protokoll auswählen im Dienst](whats-new-in-aspnet-mvc-4/_static/image7.png)

    *Das Protokoll auswählen im Dienst*
10. Melden Sie sich mit Ihrem Google-Konto.
11. Ermöglichen Sie den Standort (Localhost) zum Abrufen von Informationen aus Google-Konto an.
12. Abschließend müssen Sie am Standort ordnen Sie das Google-Konto zu registrieren.

   ![Weisen Sie Ihr Google-Konto](whats-new-in-aspnet-mvc-4/_static/image8.png)

   *Verknüpfen Ihr Google-Konto*
13. Schließen Sie den Browser, um den Debugger beenden und zurück zu Visual Studio.
14. Lesen Sie die Projektmappe Auschecken, einige andere neue Features, die in der Projektvorlage von ASP.NET MVC 4 eingeführt wurde.

   ![Der ASP.NET MVC 4-Projekt für Internetanwendungsvorlage](whats-new-in-aspnet-mvc-4/_static/image9.png "der Projekt Internetanwendungsvorlage in ASP.NET MVC 4")

   *Der Projekt Internetanwendungsvorlage in ASP.NET MVC 4*

   - **HTML 5 Markup**

       Durchsuchen Sie die Ansichten der Vorlagen auf das neue Design-Markup zu ermitteln.

       ![Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Neue Vorlage für die Verwendung von Razor und HTML5-Markups About.cshtml.")

       *Neue Vorlage für die Verwendung von Razor und HTML5-Markup ("About.cshtml").*
   - **Aktualisierte JavaScript-Bibliotheken**

       Die ASP.NET MVC 4-Standardvorlage enthält jetzt KnockoutJS, ein JavaScript-MVVM-Framework, das Sie erstellen kann und reaktionsschnelle Web-Anwendungen, die mit JavaScript und HTML. Wie sind in MVC3, jQuery und jQuery UI-Bibliotheken ebenfalls in ASP.NET MVC 4 enthalten.

     > [!NOTE]
     > Erhalten Sie weitere Informationen zur KnockOutJS-Bibliothek unter diesem Link: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/). Erfahren Sie darüber hinaus über jQuery und jQuery UI in [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a>Aufgabe 2: Untersuchen der Mobile-Application-Vorlage

ASP.NET MVC 4 ermöglicht die Entwicklung von Websites für mobile Geräte und Tablets. Diese Vorlage hat die gleiche Anwendungsstruktur als Internet Application-Vorlage (Beachten Sie, dass der controllercode praktisch identisch ist), aber der Stil wurde geändert, um auf mobilen Geräten touchbasierte ordnungsgemäß gerendert.

1. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual c# | Web** Vorlage auf der linken Struktur, und wählen Sie die **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **PhotoGallery.Mobile**, wählen Sie einen Standort (oder übernehmen Sie den Standardwert), wählen Sie &quot;zu Projektmappe hinzufügen&quot; , und klicken Sie auf **OK**.
2. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Mobile-Anwendung** Projektvorlage aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.

    ![Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung](whats-new-in-aspnet-mvc-4/_static/image11.png "Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung")

    *Erstellen einer neuen ASP.NET MVC 4 Mobile Anwendung*
3. Jetzt können Sie in der Lage, untersuchen die Projektmappe, und sehen Sie sich einige der neuen Features eingeführt, durch die ASP.NET MVC 4-Lösungsvorlage für mobile Geräte:

    - **jQuery Mobile-Bibliothek**

        Die Mobile Anwendung-Projektvorlage enthält die mobilen jQuery-Bibliothek, die ist eine open-Source-Bibliothek für mobile Browserkompatibilität. jQuery Mobile gilt progressive Verbesserung für mobile Browser, CSS und JavaScript zu unterstützen. Progressive Verbesserung ermöglicht allen Browsern, die grundlegende Inhalte von einer Webseite angezeigt, während sie nur die leistungsstärksten Browser zum Anzeigen von umfangreichen Inhalten ermöglicht. Die JavaScript- und CSS-Dateien, in dem jQuery Mobile-Stil, können mobile Browser, um den Inhalt in den Bildschirm zu passen, ohne Änderungen im Markup Seite.

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        *jQuery mobile-Bibliothek in der Vorlage*
    - **HTML5-basierte markup**

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        *Mobile Anwendung-Vorlage, die mithilfe von HTML5-Markup (Login.cshtml und "Index.cshtml")*
4. Drücken Sie **F5** um die Projektmappe auszuführen.
5. Öffnen der **Windows Phone 7 Emulator**.
6. Öffnen Sie auf der Startseite des Telefons Internet Explorer. Sehen Sie sich die URL, wobei der desktop-Anwendung gestartet, und navigieren Sie zu dieser URL vom Telefon (z. B. `http://localhost:[PortNumber]/`).
7. Nun können Sie die Anmeldeseite eingeben, oder sehen Sie sich die zu den Seiten. Beachten Sie, dass das Format der Website auf die neue Metro-app für mobile Geräte. Die ASP.NET MVC 4-Projektvorlage wird richtig angezeigt, auf mobilen Geräten, um sicherzustellen, dass alle Elemente der Seite sichtbar und aktiviert sind. Beachten Sie, dass die Links in der Kopfzeile groß genug, um angeklickt oder angetippt werden.

    ![Projekt-Seiten in einem mobilen Gerät](whats-new-in-aspnet-mvc-4/_static/image14.png "Projekt Vorlagenseiten in einem mobilen Gerät")

    *Projekt-Seiten in einem mobilen Gerät*
8. Die neue Vorlage verwendet auch die **Viewport-Meta-Tag**. Die meisten mobilen Browser definieren eine Breite für ein virtuelles Browserfenster oder &quot;Viewport&quot;, dies ist größer als die tatsächliche Breite des mobilen Geräts. Dadurch können mobile Browser, um die gesamte Webseite in die virtuelle Anzeige anzuzeigen. Die **Viewport-Meta-Tag** können Webentwickler legen Sie die Breite, Höhe und die Skalierung des Bereichs Browser auf mobilen Geräten **.** Die ASP.NET MVC 4-Vorlage für Mobile Anwendungen wird des Viewports auf die Gerätebreite (&quot;Breite = Device-Width&quot;) in der Layoutvorlage (*Views\Shared\_Layout.cshtml*), damit alle dem Seiten werden ihre Viewports auf die Bildschirmbreite für Geräte festgelegt haben. Beachten Sie, dass das Viewport-Meta-Tag die Standardansicht für den Browser nicht ändern.
9. Open  **\_Layout.cshtml**befindet sich in der **Ansichten | Freigegebene** Ordner, und einen Kommentar des Viewports Meta-Tags. Führen Sie die Anwendung, wenn nicht bereits geöffnet, und sehen Sie sich die Unterschiede.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

![Die Website nach dem kommentieren des Viewportmetatag](whats-new-in-aspnet-mvc-4/_static/image15.png "der Website nach dem kommentieren des Viewportmetatag")

*Die Website nach dem kommentieren des Viewportmetatag*
10. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
11. Entfernen Sie das Viewportmetatag aus.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a>Aufgabe 3: Adaptive Rendering verwenden

In dieser Aufgabe lernen Sie, eine andere Methode zum Rendern einer Webseite ordnungsgemäß auf mobilen Geräten und Webbrowser zur gleichen Zeit ohne Anpassung. Sie haben bereits die Viewport-Meta-Tag mit einem ähnlichen Zweck verwendet. Nachdem Sie eine weitere leistungsstarke Methode erfüllt: *adaptive Rendering*.

Adaptives Rendering ist eine Technik, die verwendet **CSS3-Medienabfragen** angewendet, die zu einer Seite anpassen. Medienabfragen definieren die Bedingungen in einem Stylesheet, das Gruppieren von CSS-Formatvorlagen unter einer bestimmten Bedingung. Nur, wenn die Bedingung "true" ist, wird das Format für die deklarierte Objekte angewendet.

Flexibilität, die Ihnen durch die adaptive Renderingtechnik kann Anpassungen für die Anzeige der Website auf verschiedenen Geräten. Sie können beliebig viele Formate, wie Sie auf ein einzelnes Stylesheet ohne das Schreiben von Code für den Stil auswählen möchten, definieren. Aus diesem Grund ist es eine sehr praktische Möglichkeit zur Anpassung von Formatvorlagen der Seite, da hierdurch die Menge von doppelt vorhandenem Code und Logik für das Rendern zu. Andererseits, würde Auslastung der Netzwerkbandbreite erhöhen, wie die Größe der CSS-Dateien unwesentlich zunehmen kann.

Verwenden Sie die adaptive Renderingtechnik, Ihre Website werden **ordnungsgemäß, unabhängig vom Browser angezeigt.** Sie sollten erwägen, wenn die zusätzliche Bandbreite Laden relevant ist.

> [!NOTE]
> Ist das grundlegende Format der eine Medienabfrage: @media \[Bereich: alle | handheld | drucken | Projektion | Bildschirm\] ([-Eigenschaft: Wert]... [Eigenschaft: Wert])


Beispiele für Medienabfragen: &gt;  <strong>@media alle und (max. Breite: 1000) und (minimale Breite: 700px) {}:</strong> für alle Lösungen 700px bis 1000.

> <strong>@media Bildschirm und (minimale Breite: 400 px) und (max. Breite: 700px) {…}:</strong> nur für Bildschirme. Die Lösung muss zwischen 400 und 700px sein.
> 
> <strong>@media Handheld und (minimale Breite: 20em), Bildschirm und (minimale Breite: 20em) {…}:</strong> für Handheld-Geräte (Mobile und Geräte) und den Bildschirm. Die minimale Breite muss größer als 20em sein.
> 
> Weitere Informationen hierzu finden Sie auf die [W3C-Website](http://www.w3.org/TR/css3-mediaqueries/).


Sie werden nun untersuchen, wie die adaptive Wiedergabe funktioniert, verbessern die Lesbarkeit von ASP.NET-MVC 4-Standard-Website-Vorlage.

1. Öffnen der **PhotoGallery.sln** Lösung, die Sie bei Aufgabe 1 erstellt haben, und wählen Sie die **Registerseite** Projekt. Drücken Sie **F5** um die Projektmappe auszuführen.
2. Ändern Sie die Breite des Browsers, der Windows festlegen, um die Hälfte oder weniger als ein Viertel der ursprünglichen Größe. Beachten Sie, was geschieht, mit den Elementen in der Kopfzeile: Einige Elemente nicht in den sichtbaren Bereich des Headers angezeigt.
3. Open <strong>"Site.CSS"</strong> -Datei aus Visual Studio-Projektmappen-Explorer befindet sich in <strong>Content</strong> Projektordner. Drücken Sie <strong>STRG + F</strong> integrierte Suche in Visual Studio öffnen und das Schreiben <strong>@media</strong> zum Suchen der <strong>CSS-Medienabfrage</strong>.

    In dieser Vorlage definierte die Media-abfragebedingung funktioniert auf diese Weise: Wenn im Browser auf die Fenstergröße unterschreitet **850 px**, die CSS-Regeln angewendet sind innerhalb dieses Blocks Media definiert.

    ![Suchen der Medienabfrage](whats-new-in-aspnet-mvc-4/_static/image16.png "der Medienabfrage suchen")

    *Suchen der Medienabfrage*
4. Ersetzen Sie den Max-Width-Attribut-Wert, legen Sie in der 850 px mit **10px**, um die adaptive Rendering zu deaktivieren, und drücken Sie **STRG + S** um die Änderungen zu speichern. Zurück zu den Browser, und drücken Sie **STRG + F5** , aktualisieren Sie die Seite mit den Änderungen Sie vorgenommen haben. Beachten Sie die Unterschiede bei der beide Seiten aus, wenn die Breite des Fensters anpassen.

    ![In der linken Seite wird angewendet der @media Formatvorlage in das Recht, den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image17.png "In der linken Seite wird angewendet der @media Formatvorlage in das Recht, den Stil fehlt")

    <em>In der linken Seite wendet die @media Formatvorlage in das Recht, den Stil fehlt</em>

    Jetzt prüfen wir heraus, was auf mobilen Geräten:

    ![In der linken Seite wird angewendet der @media Formatvorlage in das Recht, den Stil ausgelassen wird](whats-new-in-aspnet-mvc-4/_static/image18.png "In der linken Seite wird angewendet der @media Formatvorlage in das Recht, den Stil fehlt")

    <em>In der linken Seite wendet die @media Formatvorlage in das Recht, den Stil fehlt</em>

    Obwohl Sie bemerken, dass die Änderungen beim Rendern der Seite in einem Webbrowser ist nicht sehr wichtig, wenn ein mobiles Gerät verwenden werden die Unterschiede deutlicher. Auf der linken Seite des Bilds sehen wir, dass das benutzerdefinierte Format die Lesbarkeit verbessert.

    Adaptives Rendering kann in vielen Weitere Szenarien erleichtert Ihnen die anzuwendende bedingte Formatierung auf eine Website aus, und Beheben von allgemeinen Problemen bezüglich eine praktische Methode verwendet werden.

    Die Viewport-Meta-Tags und CSS-Medienabfragen sind nicht spezifisch für ASP.NET MVC 4, damit Sie diese Funktionen in einer beliebigen Webanwendung nutzen können.
5. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a>Übung 2: Erstellen der Fotogalerie-Webanwendung

In dieser Übung werden Sie für eine Fotogalerie-Anwendung zum Anzeigen von Fotos arbeiten. Starten Sie mit der ASP.NET MVC 4-Projektvorlage aus, und klicken Sie dann fügen Sie eine Funktion zum Abrufen von Fotos von einem Dienst, und sie auf der Startseite anzeigen.

In der folgenden Übung aktualisieren Sie diese Lösung die Möglichkeit zu verbessern, die sie auf mobilen Geräten angezeigt wird.

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a>Aufgabe 1: Erstellen eines simulierten Foto-Diensts

In dieser Aufgabe erstellen Sie eine Simulation der der Foto-Dienst zum Abrufen des Inhalts, die im Katalog angezeigt werden. Zu diesem Zweck fügen Sie einen neuen Controller hinzu, der einfach eine JSON-Datei mit den Daten von jedem Foto zurückgibt.

1. Open **Visual Studio** Wenn nicht bereits geöffnet.
2. Wählen Sie die **Datei | Neue | Projekt** Menübefehl. In der **neues Projekt** wählen Sie im Dialogfeld die **Visual c# | Web** Vorlage auf der linken Struktur, und wählen Sie **ASP.NET MVC 4-Webanwendung.** Nennen Sie das Projekt **Registerseite**, wählen Sie einen Standort (oder übernehmen Sie den Standardwert), und klicken Sie auf **OK**. Alternativ können Sie Arbeit fortsetzen, aus Ihrem vorhandenen ASP.NET MVC 4 **Internetanwendung** -Lösung von **Übung 1** und den nächsten Schritt überspringen.
3. In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld die **Internetanwendung** Projektvorlage aus, und klicken Sie auf **OK**. Stellen Sie sicher, dass Sie Razor als Ansichtsmodul ausgewählt haben.
4. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **App\_Daten** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**. Navigieren Sie zu der **Source\Assets\App\_Daten** Ordner dieser Übungseinheit und Hinzufügen der **Photos.json** Datei.
5. Erstellen ein neues Controllers mit dem Namen **PhotoController**. Zu diesem Zweck mit der Maustaste auf die **Controller** wechseln Sie zum Ordner **hinzufügen** , und wählen Sie **Controller.** Führen Sie den Namen des Controllers, lassen Sie die **leerer MVC-Controller** Vorlage, und klicken Sie auf **hinzufügen**.

    ![Hinzufügen der PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "der PhotoController hinzufügen")

    *Hinzufügen der PhotoController*
6. Ersetzen Sie die **Index** -Methode durch den folgenden **Katalog** Aktion, und Rückgabe des Inhalts aus der JSON-Datei, die Sie vor kurzem zum Projekt hinzugefügt haben.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - Katalog Aktion*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. Drücken Sie **F5** führen Sie die Projektmappe, und navigieren Sie dann die folgende URL, um die simulierte Foto-Dienst zu testen: `http://localhost:[port]/photo/gallery` (der Wert [Port] hängt von den aktuellen Port, der die Anwendung gestartet wurde). Die Anforderung an diese URL abrufen soll den Inhalt der **Photos.json** Datei.

    ![Testen des Diensts simulierte Foto](whats-new-in-aspnet-mvc-4/_static/image20.png "Testen des Diensts simulierte Foto")

    *Testen des Diensts simulierte Foto*

In einer realen Implementierung können Sie [ASP.NET Web-API](../../../../web-api/index.md) der Fotogalerie-Dienst implementiert wird. ASP.NET Web-API ist ein Framework, das erleichtert die Erstellung von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten zu erreichen. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a>Aufgabe 2: Anzeigen der Fotogalerie

In dieser Aufgabe aktualisieren Sie auf der Startseite, um die Fotogalerie anzuzeigen, mit der simulierte Dienst, den Sie in der ersten Aufgabe dieser Übung erstellt haben. Sie fügen die Modelldateien und die Katalogsichten zu aktualisieren.

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
2. Erstellen der **Foto** -Klasse in der **Modelle** Ordner. Zu diesem Zweck mit der Maustaste auf die **Modelle** Ordner **hinzufügen** , und klicken Sie auf **Klasse**. Legen Sie dann auf den Namen **Photo.cs** , und klicken Sie auf **hinzufügen**.
3. Fügen Sie die folgenden Elemente der **Foto** Klasse.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex02 - Foto Modell*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. Öffnen Sie im Ordner **Controller** die Datei **HomeController.cs**.
5. Fügen Sie die folgenden using-Anweisungen hinzu.

    (Codeausschnitt - *ASP.NET MVC 4 Lab - Ex02 - HomeController Using-Direktiven*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. Update der **Index** Aktion verwendet **"HttpClient"** die Katalogdaten abgerufen werden soll, und verwenden Sie dann die **JavaScriptSerializer** , es an das Ansichtsmodell zu deserialisieren.

    (Codeausschnitt - *Indexaktion für ASP.NET MVC 4-Lab - Ex02 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. Öffnen der **"Index.cshtml"** Datei befindet sich unter dem **Views\Home** Ordner und Ersetzen Sie alle Inhalte mit dem folgenden Code.

    Dieser Code durchläuft alle Fotos aus dem Dienst abgerufen und in eine ungeordnete Liste angezeigt.

    (Codeausschnitt - *Fotoliste für ASP.NET MVC 4-Lab - Ex02 -*)

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. In der **Projektmappen-Explorer**, mit der rechten Maustaste die **Content** Ordner des Projekts, und wählen Sie **hinzufügen | Vorhandenes Element**. Navigieren Sie zu der **Source\Assets\Content** Ordner dieser Übungseinheit und Hinzufügen der **"Site.CSS"** Datei. Sie haben, um dessen Ersetzung zu bestätigen. Wenn Sie haben die **"Site.CSS"** Datei öffnen, müssen Sie bestätigen, dass Sie die Datei auch erneut laden.
9. Öffnen Sie Datei-Explorer, und kopieren Sie das gesamte **Fotos** Ordner befindet sich unter dem **Source\Assets** Ordner dieser Übungseinheit in den Stammordner des Projekts im Projektmappen-Explorer.
10. Führen Sie die Anwendung aus. Jetzt sollte Sie auf der Startseite, und die Fotos im Katalog angezeigt.

    ![Fotogalerie](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotogalerie")

    *Fotogalerie*
11. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a>Übung 3: Hinzufügen von Unterstützung für mobile Geräte

Eines der wichtigsten Updates in ASP.NET MVC 4 ist die Unterstützung für die Entwicklung für mobile Geräte. In dieser Übung untersuchen Sie neue ASP.NET MVC 4-Features für mobile Anwendungen durch die Erweiterung der Registerseite-Projektmappe, die Sie in der vorherigen Übung erstellt haben.

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a>Aufgabe 1: Installieren von jQuery Mobile-Geräte in einer ASP.NET MVC 4-Anwendung

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex3-MobileSupport/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **-Paket-Manager-Konsole** durch Klicken auf die **Tools** &gt; **Bibliothekspaket-Manager** &gt; **-Paket-Manager Konsole** Option des Menüs.

    ![Öffnen die NuGet-Paket-Manager-Konsole](whats-new-in-aspnet-mvc-4/_static/image22.png "der NuGet-Paket-Manager-Konsole öffnen")

    *Öffnen die NuGet-Paket-Manager-Konsole*
3. Führen Sie in der Paket-Manager-Konsole den folgenden Befehl zum Installieren der **jQuery.Mobile.MVC** Paket.

    jQuery Mobile ist ein open-Source-Bibliothek zum Erstellen von Touch-optimierte Web-UI. Das NuGet-Paket jQuery.Mobile.MVC enthält Hilfsprogramme zum Verwenden von jQuery Mobile für eine ASP.NET MVC 4-Anwendung.

    > [!NOTE]
    > Durch den folgenden Befehl ausführen, werden Sie die Bibliothek jQuery.Mobile.MVC aus Nuget herunterladen.

    PM

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    Mit diesem Befehl wird installiert, jQuery Mobile und einige Hilfsprogrammdateien, einschließlich der folgenden:

    - **Views/Shared/\_Layout.Mobile.cshtml**: ein jQuery Mobile-basierten Layout, die für kleinere Bildschirme optimiert ist. Wenn die Website eine Anforderung von einem mobilen Browser empfängt, ersetzen Sie das ursprüngliche Layout (\_Layout.cshtml) durch diese.
    - Eine ansichtumschaltung Komponente: besteht aus den **Views/Shared/\_ViewSwitcher.cshtml** Teilansicht und **ViewSwitcherController.cs** Controller. Diese Komponente wird einen Link in mobilen Browsern von Benutzern in der Desktopversion von der Seite wechseln aktiviert angezeigt.  
        ![Fotogalerie-Projekts mit Unterstützung für mobile](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotogalerie-Projekts mit Unterstützung für mobile")

        *Fotogalerie-Projekts mit Unterstützung für mobile*
4. Registrieren Sie die Mobile Bündel. Zu diesem Zweck öffnen Sie die **"Global.asax.cs"** Datei, und fügen Sie die folgende Zeile hinzu.

    (Codeausschnitt - *Mobile Pakete von ASP.NET MVC 4 Lab - Ex03 - Register*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. Führen Sie die Anwendung, die mit einem desktop-Webbrowser ein.
6. Öffnen der **Windows Phone 7 Emulator** befindet sich in **Menü "Start" | Alle Programme | Windows Phone SDK 7.1 | Windows Phone-Emulator.**
7. Öffnen Sie auf der Startseite des Telefons Internet Explorer. Sehen Sie sich die URL, in dem die Anwendung gestartet, und navigieren Sie zu dieser URL mit dem Phone-Browser (z. B. `http://localhost:[PortNumber]/`).

    Beachten Sie, dass Ihre Anwendung im Windows Phone-Emulator anders aussehen wird, wie die jQuery.Mobile.MVC neue Ressourcen erstellt hat, in Ihrem Projekt, die für Mobilgeräte optimierte Ansichten anzeigen.

    Beachten Sie, dass die Nachricht am Anfang des Telefons, mit den Link, der in der Desktopansicht wechselt. Darüber hinaus die  **\_Layout.Mobile.cshtml** Layout, das vom Paket erstellt wurde, müssen installiert sein, ist ein anderes Layout einschließlich, in der Anwendung.

    > [!NOTE]
    > Es gibt bisher keine Verbindung zur mobilen Ansicht zurückzukehren. Es wird in späteren Versionen enthalten sein.

    ![Mobile Ansicht der Photo Gallery Startseite](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile Ansicht der Photo Gallery-Startseite")

    *Mobile Ansicht der Photo Gallery-Startseite*
8. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a>Aufgabe 2: Erstellen von mobilen Ansichten

In dieser Aufgabe erstellen Sie eine mobile Version die Ansicht "Index" mit Inhalt für eine bessere Appareance auf mobilen Geräten angepasst.

1. Kopieren der **Views\Home\Index.cshtml** anzuzeigen, und fügen Sie ihn in eine Kopie erstellen, benennen Sie die neue Datei zu **"Index.Mobile.cshtml"**.
2. Öffnen die neu erstellte **"Index.Mobile.cshtml"** anzuzeigen, und Ersetzen Sie die vorhandene &lt;Ul&gt; Tag mit dem folgenden Code. Auf diese Weise Sie aktualisieren die &lt;Ul&gt; Tag mit jQuery Mobile datenanmerkungen von jQuery mobile Designs zu verwenden.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > Beachten Sie Folgendes:
    > 
    > - Die **Data-Role** -Attributsatz auf **"ListView"** wird die Liste mit dem Listview-Stile gerendert.
    > 
    > - Die **Data-Inset** Attributsatz auf "true" wird die Liste mit abgerundeten Rahmen und Rand angezeigt.
    > 
    > - Die **Datenfilter** -Attributsatz auf **"true"** ein Suchfeld generiert.
    > 
    > Erfahren Sie mehr über jQuery Mobile-Konventionen in der Projektdokumentation zu: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)
3. Drücken Sie **STRG + S** um die Änderungen zu speichern.
4. Wechseln Sie zu der **Windows Phone-Emulator** und den Standort aktualisieren. Beachten Sie, dass das neue Aussehen und Verhalten der Katalogliste als auch das neue Suchfeld am oberen Rand. Geben Sie dann in das Suchfeld ein Wort (z. B. **Tulpen**) um eine Suche in der Fotogalerie zu starten.

    ![Katalog mithilfe von "ListView"-Stil mit Filterung](whats-new-in-aspnet-mvc-4/_static/image25.png "Katalog mithilfe von \"ListView\"-Stil mit Filterung")

    *Katalog mithilfe von "ListView"-Stil mit Filterung*

    Zusammenfassend lässt sich sagen, haben Sie die Ansicht Mobilizer Anleitung verwendet, erstellen Sie eine Kopie der Ansicht "Index" mit der &quot;mobile&quot; Suffix. Dieses Suffix wird auf ASP.NET MVC 4 angegeben, dass jede Anforderung von einem mobilen Gerät generiert diese Kopie des Index. Darüber hinaus haben Sie die mobile Version von jQuery Mobile verwenden, für die Website Aussehen und Verhalten auf mobilen Geräten zu verbessern, Ansicht "Index" aktualisiert.
5. Wechseln Sie zurück zu Visual Studio und öffnen **Site.Mobile.css** befindet sich unter dem **Content** Ordner.
6. Beheben Sie die Positionierung des Titels Foto, zeigen auf der rechten Seite des Bilds zu erleichtern. Zu diesem Zweck fügen Sie den folgenden Code der **Site.Mobile.css** Datei.

    CSS

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. Drücken Sie **STRG + S** um die Änderungen zu speichern.
8. Wechseln Sie zurück zu den **Windows Phone-Emulator** und den Standort aktualisieren. Beachten Sie, dass der Fototitel jetzt richtig positioniert ist.

    ![Klicken Sie auf der rechten Seite des Bilds positioniert Titel](whats-new-in-aspnet-mvc-4/_static/image26.png "Titel auf der rechten Seite des Bilds positioniert")

    *Klicken Sie auf der rechten Seite des Bilds positioniert Titel*

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a>Aufgabe 3: jQuery Mobile-Designs

Jedes Layout und Widgets in jQuery Mobile wurde um ein neues objektorientierten CSS-Framework entwickelt, die ein Design für die vollständige einheitliche visuelles Design auf Websites und Anwendungen anwenden können.

jQuery Mobile Standard Design enthält 5 Muster, die Buchstaben angegeben sind (a, b, C, d, e) für Kurzübersicht.

In dieser Aufgabe aktualisieren Sie das mobile Layout, um ein anderes Design als den Standardwert zu verwenden.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der  **\_Layout.Mobile.cshtml** Datei im **Views\Shared**.
3. Suchen Sie das Div-Element mit die Data-Rolle festgelegt &quot;Seite&quot; und aktualisieren Sie die **Data-Theme** zu &quot; **e**&quot;.

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. Drücken Sie **STRG + S** um die Änderungen zu speichern.
5. Aktualisieren Sie den Standort in der **Windows Phone-Emulator** und beachten Sie, dass das neue Farbschema.

    ![Mobiles Layout mit einem anderen Farbschema](whats-new-in-aspnet-mvc-4/_static/image27.png "mobiles Layout mit einem anderen Farbschema")

    *Mobiles Layout mit einem anderen Farbschema*

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a>Aufgabe 4: Verwendung der Ansichtumschaltung-Komponente und der Browser Überschreiben von Funktionen

Eine Konvention für Mobilgeräte optimierte Web Pages ist auf einen Link hinzufügen, dessen Text ist etwas wie Desktopansicht oder vollständige standortmodus, in dem Benutzer, die in einer Desktopversion von der Seite wechseln kann. Das jQuery.Mobile.MVC-Paket enthält ein Beispiel für **ansichtumschaltung** Komponente für diesen Zweck verwendet die  **\_Layout.Mobile.cshtml** anzeigen.

![Link zur Desktop-Ansicht wechseln](whats-new-in-aspnet-mvc-4/_static/image28.png "Link zur Desktop-Ansicht wechseln")

*Link zum zur Desktop-Ansicht wechseln*

Ansichtumschaltung verwendet ein neues Feature namens **Browser überschreiben**. Dieses Feature ermöglicht Ihrer Anwendung, die Anforderungen zu behandeln, als ob sie von einem anderen Browser (Benutzer-Agent) als dem gestellt würden, die, denen Sie tatsächlich von stammen.

In dieser Aufgabe untersuchen Sie die beispielimplementierung des eine ansichtumschaltung-jQuery.Mobile.MVC und der neue Browser Überschreiben von Funktionen in ASP.NET MVC 4 hinzugefügt.

1. Wechseln Sie zurück zu Visual Studio.
2. Öffnen der  **\_Layout.Mobile.cshtml** Ansicht befindet sich unter der **Views\Shared** Ordner, und beachten Sie, dass der ansichtumschaltung Komponente, die als Teilansicht verwiesen wird.

    ![Mobiles Layout mit Ansichtumschaltung Komponente](whats-new-in-aspnet-mvc-4/_static/image29.png "mobiles Layout mit Ansichtumschaltung Komponente")

    *Mobiles Layout mit Ansichtumschaltung Komponente*
3. Öffnen der  **\_ViewSwitcher.cshtml** Teilansicht.

    Die Teilansicht verwendet die neue Methode **ViewContext.HttpContext.GetOverriddenBrowser()** bestimmen den Ursprung der webanforderung aus, und zeigen den entsprechenden Link, um entweder auf den Desktop oder Mobile Ansichten zu wechseln.

    Die **GetOverridenBrowser** Methode gibt ein **HttpBrowserCapabilitiesBase** Instanz, die der Benutzer-Agent eingerichtet, für die Anforderung entspricht (tatsächlichen oder überschrieben). Sie können diesen Wert verwenden, um Eigenschaften zu erhalten, wie z. B. **IsMobileDevice**.

    ![Die Teilansicht ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher Teilansicht")

    *ViewSwitcher Teilansicht*
4. Öffnen der **ViewSwitcherController.cs** Klasse befindet sich in der **Controller** Ordner. Sehen Sie sich mit dieser Aktion SwitchView wird aufgerufen, die der Link in der ViewSwitcher-Komponente, und beachten Sie, dass die neuen HttpContext-Methoden.

    - Die **HttpContext.ClearOverridenBrowser()** -Methode entfernt alle außer Kraft gesetzten Benutzer-Agent für die aktuelle Anforderung.
    - Die **HttpContext.SetOverridenBrowser()** methodenüberschreibungen der Anforderung tatsächlichen Benutzer-Agent-Wert mit dem angegebenen Benutzer-Agent.  
        ![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher-Controller")  
*ViewSwitcher-Controller*

        Außerkraftsetzen der Browser ist ein zentrales Feature von ASP.NET MVC 4, das ist auch verfügbar, auch wenn Sie nicht das jQuery.Mobile.MVC-Paket installieren. Jedoch wird dieses Feature wirkt sich auf nur anzeigen, Layout und Speicherortformate der Teilansicht, und es hat keine Auswirkungen auf die Features, die für das Request.Browser-Objekt abhängig sind.

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a>Aufgabe 5: Hinzufügen der Ansichtumschaltung in der Desktopansicht

In dieser Aufgabe aktualisieren Sie die desktop-Layout-ansichtumschaltung enthält. Dadurch können mobile Benutzer zurück zur mobilen Ansicht zu wechseln, beim Desktopansicht zu durchsuchen.

1. Aktualisieren Sie den Standort in der **Windows Phone-Emulator**.
2. Klicken Sie auf die **Desktopansicht** oben auf den Link des Katalogs. Beachten Sie, dass keine ansichtumschaltung vorhanden ist, in der Desktopansicht um zuzulassen, dass Sie für die mobile Ansicht zurückgeben.
3. Wechseln Sie zurück zu Visual Studio und Öffnen der  **\_Layout.cshtml** anzeigen.
4. Suchen Sie den Abschnitt für die Anmeldung, und fügen Sie einen Aufruf zum Rendern der  **\_ViewSwitcher** partielle Ansicht unten die  **\_LogOnPartial** Teilansicht. Drücken Sie dann die **STRG + S** um die Änderungen zu speichern.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. Drücken Sie **STRG + S** um die Änderungen zu speichern.
6. Aktualisieren Sie die Seite im Windows Phone-Emulator aus, und doppelklicken Sie auf dem Bildschirm zum Verkleinern. Beachten Sie, die auf der Startseite jetzt zeigt die **Mobile Ansicht** Link, der von der mobile zur Desktopansicht wechseln.

    ![Der Darstellung in der Desktopansicht gerendert](whats-new-in-aspnet-mvc-4/_static/image32.png "Ansichtumschaltung, die in der Desktopansicht gerendert")

    *In der Desktopansicht gerendert Ansichtumschaltung*
7. Wechseln Sie zur mobilen Ansicht erneut aus, und navigieren Sie zu <strong>zu</strong> Seite (http://localhost[Port]/Home/About). Beachten Sie, dass, selbst wenn Sie eine Ansicht About.Mobile.cshtml erstellt haben, mit das mobile Layout der Seite "Info" angezeigt wird (\_Layout.Mobile.cshtml).

    ![Zu den Seiten](whats-new-in-aspnet-mvc-4/_static/image33.png "zu Seite")

    *Zu den Seiten*
8. Öffnen Sie schließlich die Website in einem desktop-Webbrowser. Beachten Sie, dass keine der vorherigen Updates Desktopansicht betroffen ist.

    ![Desktopansicht Registerseite](whats-new-in-aspnet-mvc-4/_static/image34.png "Registerseite Desktopansicht")

    *Registerseite Desktopansicht*

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a>Aufgabe 6: Erstellen einer neuen Anzeigemodi

Das neue Feature der Anzeige Modi kann es sich um eine Anwendung, die Ansichten, je nach Browser auswählen, die die Anforderung generiert wird. Z. B. wenn ein desktop-Browser auf der Startseite anfordert, die Anwendung gibt die **Views\Home\Index.cshtml** Vorlage. Klicken Sie dann, wenn ein mobiler Browser auf der Startseite anfordert, die Anwendung gibt die **Views\Home\Index.mobile.cshtml** Vorlage.

In dieser Aufgabe erstellen Sie ein benutzerdefiniertes Layout für iPhone-Geräte, und müssen Sie die Anforderungen aus der iPhone-Geräte zu simulieren. Zu diesem Zweck können Sie entweder einen iPhone-Emulator /-Simulator (z. B. [elektrischen Mobile Simulator](http://www.electricplum.com/)) oder einen Browser mit Add-ons, die den Benutzer-Agent zu ändern. Anweisungen zum Festlegen der Zeichenfolge des Benutzer-Agent in einem Safaribrowser auf einem iPhone zu emulieren, finden Sie unter [Vorgehensweise können Sie Safari nehmen, es handelt sich um Internet Explorer](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) im Blog von David Alison des.

**Beachten Sie, dass diese Aufgabe optional ist, und Sie in den Laboren fortsetzen können, ohne Sie auszuführen.**

1. Drücken Sie in Visual Studio **UMSCHALT** + **F5** zum Debuggen der Anwendung zu beenden.
2. Open **"Global.asax.cs"** und fügen Sie die folgenden using-Anweisung.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. Fügen Sie folgenden hervorgehobenen Code hinzu, bei der Anwendung\_Start-Methode.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex03 - iPhone "DisplayMode"*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

Sie haben ein neues registriert **DefaultDisplayMode, die mit dem Namen &quot;iPhone&quot;**, in der statischen **DisplayModeProvider.Instance.Modes** statische Liste, die für abgeglichen werden Jede eingehende Anforderung. Wenn die eingehende Anforderung die Zeichenfolge enthält &quot;iPhone&quot;, ASP.NET MVC findet die Ansichten enthalten, deren Name die &quot;iPhone&quot; Suffix. Der 0-Parameter gibt an, wie bestimmte der neue Modus wird; Diese Ansicht ist z.B. spezifischer als die allgemeinen &quot;.mobile&quot; Regel, die Anfragen von mobilen Geräten entspricht.

Nachdem dieser Code ausgeführt, wenn ein iPhone-Browser eine Anforderung generiert, kann Ihre Anwendung verwendet die **Views\Shared\\_Layout.iPhone.cshtml** Layout, die Sie in den nächsten Schritten erstellen.

> [!NOTE]
> So testen Sie die Anforderung für iPhone zu Demonstrationszwecken vereinfacht wurde, und möglicherweise nicht funktionieren erwartungsgemäß für jede iPhone-Benutzer-Agent-Zeichenfolge (zum Beispiel Test Groß-/Kleinschreibung beachtet wird).

4. Erstellen einer Kopie der  **\_Layout.Mobile.cshtml** Datei die **Views\Shared** Ordner, und benennen Sie die Kopie auf &quot; **\_Layout.iPhone.csthml**&quot;.
5. Open  **\_Layout.iPhone.csthml** Sie im vorherigen Schritt erstellt haben.
6. Suchen Sie das Div-Element mit dem Data-Role-Attribut festgelegt **Seite** , und ändern Sie die **Data-Theme** Attribut &quot; **eine**&quot;.


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

Nun müssen Sie 3 Layouts in Ihrer ASP.NET MVC 4-Anwendung:

1. **\_Layout.cshtml**: Standardlayout für Desktopbrowser verwendet.
2. **\_Layout.Mobile.cshtml**: Standardlayout für mobile Geräte verwendet.
3. **\_Layout.iPhone.cshtml**: bestimmten Layout für die iPhone-Geräte, die über ein anderes Farbschema zur Unterscheidung von \_Layout.mobile.cshtml.
7. Drücken Sie **F5** führen Sie die Anwendung, und suchen den Standort in der **Windows Phone-Emulator**.
8. Öffnen einer **iPhone-Simulator** (finden Sie unter [Anhang C](#AppendixC) Anleitungen zum Installieren und konfigurieren einen iPhone-Simulator), und wechseln Sie zur Website zu. Beachten Sie, dass jedem Telefon die jeweiligen Vorlage verwendet wird.

    ![Using-different-Views-for-each-Mobile-Device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    *Verwenden verschiedene Ansichten für jedes mobile Gerät*

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a>Übung 4: Verwenden von asynchronen Controller

Microsoft .NET Framework 4.5 führt neue Sprachfeatures in c# und Visual Basic eine neue Grundlage für Asynchronie in .NET programmieren bereitstellen. Dieser neue Grundlage ermöglicht die asynchronen Programmierung, - ähnelt und ungefähr so einfach wie das - synchronen Programmierung. Sie können jetzt um asynchrone Aktionsmethoden in ASP.NET MVC 4 mit den **AsyncController** Klasse. Sie können asynchrone Aktionsmethoden für lang andauernde, nicht CPU-gebundene Anforderungen. Dadurch wird vermieden, dass der Webserver aus arbeiten ausführen, während die Anforderung verarbeitet wird. Die Klasse AsyncController wird normalerweise für lang andauernde Aufrufe des Webdiensts verwendet.

In dieser Übung werden die Grundlagen der asynchronen Vorgänge in ASP.NET MVC 4 erläutert. Wenn Sie eingehendere Informationen benötigen, können Sie den folgenden Artikel überprüfen: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a>Aufgabe 1: Implementieren eines asynchronen Controllers

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex4-Async/Anfang/** Ordner. Andernfalls können Sie weiterhin, verwenden die **End** Lösung abgerufen wird, indem Sie der vorherige Übung abschließen können.

   1. Wenn Sie die bereitgestellten geöffnet **beginnen** Lösung, Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Öffnen der **"HomeController.cs"** -Klasse aus der **Controller** Ordner.
3. Fügen Sie die folgenden using-Anweisung.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. Update der **HomeController** Klasse geerbt **AsyncController**. Domänencontroller, auf denen AsyncController abgeleitet ermöglichen es ASP.NET, asynchrone Anforderungen zu verarbeiten, und sie können weiterhin Dienst synchrone Aktionsmethoden.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. Hinzufügen der **Async** Schlüsselwort, um die **Index** Methode und der Rückgabetyp **Aufgabe&lt;ActionResult&gt;**.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > Die **Async** Schlüsselwort ist eines der .NET Framework 4.5 bietet neue Schlüsselwörter; er informiert den Compiler, dass diese Methode von asynchronen Code enthält. Ein **Aufgabe** Objekt stellt einen asynchronen Vorgang, die in der Zukunft zu einem bestimmten Zeitpunkt abgeschlossen werden kann.
6. Ersetzen Sie die **Client. GetAsync()** Aufruf mit dem vollständigen Async-Version mit await-Schlüsselwort wie unten dargestellt.

    (Codeausschnitt - *"getasync" ASP.NET MVC 4-Lab - Ex04 -*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > In der vorherigen Version, wurden Sie verwenden die **Ergebnis** Eigenschaft aus der **Aufgabe** Objekt, das den Thread zu blockieren, bis das Ergebnis (Synchronisierungsversion) zurückgegeben wird.
    > 
    > Hinzufügen der **"await"** -Schlüsselwort weist den Compiler an, um asynchron für den Aufruf der Methode zurückgegebene Aufgabe zu warten. Dies bedeutet, dass der Rest des Codes als Rückruf ausgeführt wird, nur, nachdem die erwartete Methode abgeschlossen wird. Ein weiterer Aspekt zu beachten ist, dass Sie nicht benötigen, um den Try / Catch-Block zu ändern, um dies zu gewährleisten: die Ausnahmen, die im Hintergrund oder im Vordergrund ausgeführt werden noch abgefangen werden, ohne zusätzlichen Aufwand mit einem Handler, die vom Framework bereitgestellt wird.
7. Ändern Sie den Code durch die Zeilen mit dem neuen Code ersetzen, wie unten gezeigt auf die asynchrone Implementierung fortsetzen

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex04 - ReadAsStringAsync*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. Führen Sie die Anwendung aus. Sie sehen keine größeren Änderungen, Ihren Code, blockiert jedoch keinen Thread aus dem Threadpool, sodass eine bessere Nutzung der Serverressourcen und Verbessern der Leistung.

    > [!NOTE]
    > Erfahren Sie mehr über die neuen Funktionen der asynchronen Programmierung in der testumgebung &quot; **asynchrone Programmierung in .NET 4.5 mit c# und Visual Basic** &quot; in das Visual Studio-Training Kit enthalten.

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a>Aufgabe 2: Behandlung von Timeouts mit Abbruchtoken

Asynchrone Aktionsmethoden, die Instanzen der Aufgabe zurückgeben, können auch Timeouts unterstützen. In dieser Aufgabe aktualisieren Sie den Index-Methodencode zum Behandeln eine Timeout-Szenarios mit einem Abbruchtoken.

1. Wechseln Sie zurück zu Visual Studio, und drücken Sie **UMSCHALT + F5** debugging beenden.
2. Fügen Sie die folgenden using-Anweisung die **"HomeController.cs"** Datei.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. Aktualisieren Sie die Index-Aktion zum Empfangen einer **CancellationToken** Argument.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. Update der **"getasync"** Aufruf an das Abbruchtoken zu übergeben.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex04 - SendAsync mit CancellationToken*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. Ergänzen der *Index* -Methode mit einer **AsyncTimeout** -Attributsatz auf 500 Millisekunden und **HandleError** Attribut zur Verarbeitung konfiguriert  **TaskCanceledException** durch Umleiten an einen **"timedOut"** anzeigen.

    (Codeausschnitt - *ASP.NET MVC 4-Lab - Ex04 - Attribute*)

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. Öffnen der **PhotoController** -Klasse, und Aktualisieren der **Katalog** Methode zum Verzögern der Ausführung von 1000 Millisekunden (1 Sekunde), um eine langwierige Aufgabe zu simulieren.

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. Öffnen der **"Web.config"** Datei, und Aktivieren von benutzerdefinierten Fehlern durch das folgende Element hinzufügen.

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. Erstellen Sie eine neue Ansicht im **Views\Shared** mit dem Namen **"timedOut"** und verwenden Sie das Standardlayout. Klicken Sie im Projektmappen-Explorer mit der Maustaste der **Views\Shared** Ordner, und wählen **hinzufügen | Ansicht**.

    ![Mit anderen Ansichten für jedes mobile Gerät](whats-new-in-aspnet-mvc-4/_static/image36.png "mit anderen Ansichten für jedes mobile Gerät")

    *Verwenden verschiedene Ansichten für jedes mobile Gerät*
9. Update der **"timedOut"** Inhalt anzeigen, wie unten dargestellt.

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. Führen Sie die Anwendung, und navigieren Sie zu der Stamm-URL. Wie Sie hinzugefügt haben, eine **Thread.Sleep** von 1000 Millisekunden angegeben, erhalten Sie einen Timeoutfehler, vom der **AsyncTimeout** Attribut, und erfassen Sie mithilfe der **HandleError** -Attribut.

    ![Timeout-Ausnahme behandelt](whats-new-in-aspnet-mvc-4/_static/image37.png "behandelten Timeout-Ausnahme")

    *Timeoutausnahme behandelt wird*

> [!NOTE]
> Darüber hinaus können Sie diese Anwendung für Windows Azure-Websites Folgendes bereitstellen [Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy](#AppendixD).


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

In diesem praktische Übung haben Sie einige der neuen Features in ASP.NET MVC 4 residenten untersucht. Die folgenden Konzepte haben behandelt:

- Profitieren Sie von den Verbesserungen bei der ASP.NET MVC-Projekt-Vorlagen, einschließlich der neuen Projektvorlage für mobile Anwendungen
- Verwenden Sie den Viewport-Attribut für HTML5 und CSS-Medienabfragen, um die Anzeige auf mobilen Geräten zu verbessern
- Verwenden von jQuery Mobile für progressive Verbesserungen und zur Erstellung von Touch-optimierte Web-Benutzeroberfläche
- Erstellen von Ansichten
- Umschalten zwischen mobilen und desktop-Ansichten in der Anwendung mithilfe der ansichtumschaltung Komponente
- Erstellen Sie asynchrone Controller mithilfe des Task-Unterstützung

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Anhang A: Verwenden von Codeausschnitten

Mit Codeausschnitten müssen Sie den Code zur Hand benötigten. Das Lab-Dokument informiert Sie genau wann sie verwendet werden kann wie in der folgenden Abbildung dargestellt.

![Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt](whats-new-in-aspnet-mvc-4/_static/image38.png "mithilfe von Visual Studio-Codeausschnitten, die Code in das Projekt einfügen.")

*Verwenden von Visual Studio-Codeausschnitten zum Einfügen von Code in Ihrem Projekt*

***Hinzufügen ein Codeausschnitts, die über die Tastatur (nur c#)***

1. Platzieren Sie den Cursor, wo Sie möchten den Code einfügen.
2. Starten Sie den codeausschnittsnamen (ohne Leerzeichen oder Bindestriche) eingeben.
3. Beobachten Sie, wie IntelliSense zeigt übereinstimmende Codeausschnitte Namen.
4. Wählen Sie den richtigen Codeausschnitt (oder halten Sie eingeben, bis die gesamte codeausschnittsnamen ausgewählt ist).
5. Drücken Sie die Tab-Taste zweimal auf Einfügen des Codeausschnitts an der Cursorposition ein.

![Geben Sie den Namen des Ausschnitts](whats-new-in-aspnet-mvc-4/_static/image39.png "Geben Sie den Namen des Ausschnitts")

*Geben Sie den Namen des Ausschnitts*

![Tabstopp drücken, um den hervorgehobenen Codeausschnitt auswählen](whats-new-in-aspnet-mvc-4/_static/image40.png "Tab drücken, um den hervorgehobenen Codeausschnitt auswählen")

*Drücken Sie Tabstopp, um den hervorgehobenen Codeausschnitt auswählen*

![Drücken Sie die Tabulatortaste erneut, und der Ausschnitt erweitert](whats-new-in-aspnet-mvc-4/_static/image41.png "drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.")

*Drücken Sie die Tabulatortaste erneut, und der Ausschnitt werden erweitert.*

***Hinzufügen ein Codeausschnitts, die mit der Maus (c#, Visual Basic und XML)***

1. Mit der rechten Maustaste, in dem den Codeausschnitt eingefügt werden soll.
2. Wählen Sie **Ausschnitt einfügen** gefolgt von **Meine Codeausschnitte**.
3. Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken.

![Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten](whats-new-in-aspnet-mvc-4/_static/image42.png "mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten,")

*Mit der rechten Maustaste, in dem Sie den Codeausschnitt einfügen, und wählen Ausschnitt einfügen möchten*

![Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken](whats-new-in-aspnet-mvc-4/_static/image43.png "die relevante Codeausschnitte in der Liste auswählen, indem Sie darauf klicken")

*Wählen Sie die relevante Codeausschnitte in der Liste, indem Sie darauf klicken*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Anhang B: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image45.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image46.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image47.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](whats-new-in-aspnet-mvc-4/_static/image48.png)

    *Visual Studio Express für Web-Kachel*

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a>Anhang C: Installieren von WebMatrix 2 und in der iPhone-Simulator

Für die Ausführung Ihrer Website auf einem simulierten iPhone-Gerät können Sie die WebMatrix-Erweiterung &quot;elektrischen Mobile-Simulator für das iPhone&quot;. Darüber hinaus können Sie die gleiche Erweiterung führen Sie den Simulator in Visual Studio 2012 konfigurieren.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a>Aufgabe 1: Installieren von WebMatrix 2

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>WebMatrix 2</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Installieren von WebMatrix 2")

    *Installieren von WebMatrix 2*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](whats-new-in-aspnet-mvc-4/_static/image50.png "akzeptieren der Lizenzbedingungen")

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](whats-new-in-aspnet-mvc-4/_static/image51.png "Installationsstatus")

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation wurde abgeschlossen")

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a>Aufgabe 2: installieren die iPhone-Simulator-Erweiterung

1. Führen Sie **WebMatrix** und ggf. eine vorhandene Website öffnen oder erstellen Sie eine neue Ressourcengruppe.
2. Klicken Sie auf die **ausführen** Schaltfläche der **Startseite** , und wählen Sie im Menüband **neu hinzufügen**.

    ![Hinzufügen von neuen WebMatrix-Erweiterung](whats-new-in-aspnet-mvc-4/_static/image53.png "neuen WebMatrix-Erweiterung hinzufügen")

    *Hinzufügen von neuen WebMatrix-Erweiterung*
3. Wählen Sie **iPhone-Simulator** , und klicken Sie auf **installieren**.

    ![Durchsuchen die WebMatrix-Erweiterungen](whats-new-in-aspnet-mvc-4/_static/image54.png "Durchsuchen von WebMatrix-Erweiterungen")

    *WebMatrix-Extensions durchsuchen*
4. Klicken Sie auf die Paketdetails auf **installieren** zum Fortsetzen der Installation der Erweiterung.

    ![iPhone-Simulator Erweiterung](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone-Simulator-Erweiterung")

    *iPhone-Simulator-Erweiterung*
5. Lesen und akzeptieren der LIZENZBEDINGUNGEN-Erweiterungs.

    ![WebMatrix-Erweiterung EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix-Erweiterung Endbenutzer-Lizenzvertrag")

    *WebMatrix-Erweiterung Endbenutzer-Lizenzvertrag*
6. Jetzt können Sie Ihre Website von WebMatrix ausführen, mit der iPhone-Simulator-Option.

    ![Führen Sie mit der iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "mit iPhone ausgeführt")

    *Führen Sie mit der iPhone*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a>Aufgabe 3: Konfigurieren von Visual Studio 2012 zum Ausführen der iPhone-Simulator

1. Öffnen Sie **Visual Studio 2012** und jede Website öffnen oder Erstellen eines neuen Projekts.
2. Klicken Sie auf den Pfeil nach unten, von der Schaltfläche "ausführen", und wählen Sie **Browserauswahl**.

    ![Durchsuchen mit](whats-new-in-aspnet-mvc-4/_static/image58.png "Browserauswahl")

    *Browserauswahl*
3. In der &quot;Browserauswahl&quot; Dialogfeld klicken Sie auf **hinzufügen**.
4. In der &quot;Programm hinzufügen&quot; im Dialogfeld die folgenden Werte:

   - <strong>Programm</strong>: C:\Users\*{"CurrentUser"}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (Aktualisieren Sie den Pfad entsprechend)</em>
   - **Argumente**: &quot;1&quot;
   - **Anzeigename des**: iPhone-Simulator

     ![Programm hinzufügen](whats-new-in-aspnet-mvc-4/_static/image59.png "Programm hinzufügen")

     *Fügen Sie Programm mit Durchsuchen hinzu*
5. Klicken Sie auf **OK** und die Dialogfelder zu schließen.
6. Jetzt können Sie in der Lage, Ihre Webanwendungen in der iPhone-Simulator aus Visual Studio 2012 auszuführen.

    ![Navigieren Sie mit der iPhone-Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "durchsuchen mit der iPhone-Simulator")

    *Navigieren Sie mit der iPhone-Simulator*

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Anhang D: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

In diesem Anhang wird veranschaulicht, wie erstellen eine neue Website aus dem Windows Azure-Verwaltungsportal, und Veröffentlichen der Anwendung, die Sie erhalten haben, indem der testumgebung können die Nutzung der Web Deploy-publishing-Funktion von Windows Azure bereitgestellte.

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Aufgabe 1: Erstellen einer neuen Website aus dem Windows Azure-Portal

1. Wechseln Sie zu der [Windows Azure-Verwaltungsportal](https://manage.windowsazure.com/) und melden Sie sich mit den Microsoft-Anmeldeinformationen, die Ihrem Abonnement zugeordnet.

    > [!NOTE]
    > Mit Windows Azure können Sie 10 ASP.NET-Websites kostenlos hosten und dann zu skalieren, wenn Ihr Datenverkehr zunimmt. Sie können registrieren [hier](http://aka.ms/aspnet-hol-azure).

    ![Melden Sie sich bei Windows Azure-Portal](whats-new-in-aspnet-mvc-4/_static/image61.png "melden Sie sich bei Windows Azure-Portal")

    *Melden Sie sich bei Windows Azure-Verwaltungsportal*
2. Klicken Sie auf **neu** auf der Befehlsleiste.

    ![Erstellen einer neuen Website](whats-new-in-aspnet-mvc-4/_static/image62.png "Erstellen einer neuen Website")

    *Erstellen einer neuen Website*
3. Klicken Sie auf **Compute** | **Website**. Wählen Sie dann **Schnellerfassung** Option. Geben Sie eine verfügbare URL für die neue Website, und klicken Sie auf **Website erstellen**.

    > [!NOTE]
    > Ein Windows Azure-Website ist der Host für eine Webanwendung, die in der Cloud, die Sie steuern und verwalten können. Die Option "Schnellerfassung" können Sie eine vollständige Webanwendung auf dem Windows Azure-Website von außerhalb des Portals bereitstellen. Er umfasst keine Informationen zum Einrichten einer Datenbank.

    ![Erstellen einer neuen Website mithilfe der Schnellerfassung](whats-new-in-aspnet-mvc-4/_static/image63.png "Erstellen einer neuen Website mithilfe der Schnellerfassung")

    *Erstellen einer neuen Website mithilfe der Schnellerfassung*
4. Warten Sie, bis die neue **Website** erstellt wird.
5. Nachdem die Website erstellt wurde, klicken Sie auf den Link unter der **URL** Spalte. Überprüfen Sie, dass die neue Website ausgeführt wird.

    ![Navigieren zu der neuen Website](whats-new-in-aspnet-mvc-4/_static/image64.png "auf die neue Website durchsuchen")

    *Um die neue Website durchsuchen*

    ![Website](whats-new-in-aspnet-mvc-4/_static/image65.png "Website ausgeführt wird")

    *Die Website ausgeführt wird*
6. Wechseln Sie zurück zum Portal, und klicken Sie auf den Namen der Website unter der **Namen** Spalte die Verwaltungsseiten angezeigt.

    ![Öffnen die Website-Verwaltungsseiten](whats-new-in-aspnet-mvc-4/_static/image66.png "öffnen die Website-Verwaltungsseiten")

    *Öffnen die Website-Verwaltungsseiten*
7. In der **Dashboard** Seite die **Blick** auf die **Veröffentlichungsprofil herunterladen** Link.

    > [!NOTE]
    > Die *Veröffentlichungsprofil* enthält alle Informationen zum Veröffentlichen einer Webanwendung in einer Windows Azure-Website für die einzelnen aktivierten Veröffentlichungsmethoden erforderlich sind. Das Veröffentlichungsprofil enthält die URLs, Benutzeranmeldeinformationen und datenbankzeichenfolgen, die zum Herstellen einer Verbindung mit und die Authentifizierung bei jedem der Endpunkte für die eine Veröffentlichungsmethode aktiviert ist. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express für Web** und **Microsoft Visual Studio 2012** Unterstützung, die beim Lesen von veröffentlichungsprofilen Automatisieren der Konfiguration von diesen Programmen Veröffentlichung von Webanwendungen in Windows Azure-Websites.

    ![Herunterladen der Website-Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image67.png "herunterladen den Website-Veröffentlichungsprofil")

    *Herunterladen der Website-Veröffentlichungsprofil*
8. Laden Sie das Veröffentlichungsprofil an einem bekannten Speicherort herunter. In dieser Übung wird weiter wie Sie diese Datei verwenden, um das Veröffentlichen einer Webanwendung auf einem Windows Azure-Websites in Visual Studio angezeigt.

    ![Speichern das Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image68.png "speichern das Veröffentlichungsprofil")

    *Speichern das Veröffentlichungsprofil*

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Aufgabe 2: Konfigurieren des Datenbank-Servers

Wenn Ihre Anwendung nutzt SQL Server Datenbanken, die Sie zum Erstellen eines SQL-Datenbank-Servers benötigen. Wenn zum Bereitstellen einer einfachen Anwendung, die keine SQL Server verwendet werden sollen, können Sie diese Aufgabe überspringen.

1. Sie benötigen einen SQL-Datenbank-Server zum Speichern der Datenbank. Sehen Sie die SQL-Datenbank-Server aus Ihrem Abonnement in das Windows Azure-Verwaltungsportal unter **Sql-Datenbanken** | **Server** | **des Servers Dashboard**. Wenn Sie nicht mit eine Server-Instanz verfügen, können Sie erstellen, mithilfe der **hinzufügen** auf der Befehlsleiste auf die Schaltfläche. Notieren Sie sich die **Servername und -URL, Administrator-Anmeldenamen und Kennwort**, wie Sie sie in den nächsten Aufgaben verwenden. Erstellen Sie die Datenbank nicht noch, wie es in einem späteren Zeitpunkt erstellt wird.

    ![SQL Server-Datenbank-Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "Dashboard der SQL-Datenbank-Server")

    *SQL Server-Datenbank-Dashboard*
2. In der nächsten Aufgabe testen Sie die Verbindung mit der Datenbank, in Visual Studio aus diesem Grund Sie Ihre lokale IP-Adresse in der Liste des Servers der einschließen müssen **zulässige IP-Adressen**. Zu diesem Zweck klicken Sie auf **konfigurieren**, wählen Sie die IP-Adresse von **Current Client IP Address** und fügen Sie ihn auf die **IP-Startadresse** und **IP-Endadresse** Textfelder, und klicken Sie auf die ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) Schaltfläche.

    ![Client-IP-Adresse hinzufügen](whats-new-in-aspnet-mvc-4/_static/image71.png)

    *Client-IP-Adresse hinzufügen*
3. Einmal die **Client-IP-Adresse** wird hinzugefügt, um die zulässigen IP-Adressen aufzulisten, klicken Sie auf **speichern** um die Änderungen zu bestätigen.

    ![Bestätigen von Änderungen](whats-new-in-aspnet-mvc-4/_static/image72.png)

    *Bestätigen von Änderungen*

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Aufgabe 3: Veröffentlichen einer ASP.NET MVC 4-Anwendung mit Web Deploy

1. Wechseln Sie zurück, der ASP.NET MVC 4-Projektmappe. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Websiteprojekt, und wählen Sie **veröffentlichen**.

    ![Veröffentlichen der Anwendung](whats-new-in-aspnet-mvc-4/_static/image73.png "Veröffentlichen der Anwendung")

    *Veröffentlichen der Website*
2. Importieren Sie das Veröffentlichungsprofil, die, das Sie in der ersten Aufgabe gespeichert.

    ![Importieren das Veröffentlichungsprofil](whats-new-in-aspnet-mvc-4/_static/image74.png "Importieren des Veröffentlichungsprofils")

    *Importieren Sie das Veröffentlichungsprofil*
3. Klicken Sie auf **überprüft, ob Verbindung**. Klicken Sie nach Abschluss der Überprüfung auf **Weiter**.

    > [!NOTE]
    > Die Überprüfung ist abgeschlossen, wenn Sie sehen, dass ein grünes Häkchen neben der Schaltfläche "Verbindung überprüfen" angezeigt werden.

    ![Überprüfen der Verbindung](whats-new-in-aspnet-mvc-4/_static/image75.png "Überprüfen der Verbindung")

    *Überprüfen der Verbindung*
4. In der **Einstellungen** Seite die **Datenbanken** auf die Schaltfläche neben dem Textfeld für die datenbankverbindung (d. h. **DefaultConnection**).

    ![Web deploy-Konfiguration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy-Konfiguration")

    *Web deploy-Konfiguration*
5. Konfigurieren Sie die Verbindung mit der Datenbank wie folgt:

   - In der **Servernamen** Geben Sie Ihre SQL-Datenbankserver URL mit der *Tcp:* Präfix.
   - In **Benutzernamen** Geben Sie Ihre Server Administrator-Anmeldenamen.
   - In **Kennwort** Geben Sie Ihre Server Administrator-Anmeldekennwort.
   - Geben Sie einen neuen Datenbanknamen ein, z. B.: *MVC4SampleDB*.

     ![Konfigurieren von Ziel-Verbindungszeichenfolge](whats-new-in-aspnet-mvc-4/_static/image77.png "Zielverbindungszeichenfolge konfigurieren")

     *Konfigurieren von Ziel-Verbindungszeichenfolge*
6. Klicken Sie dann auf **OK**. Bei der Aufforderung zum Erstellen der Datenbank auf **Ja**.

    ![Erstellen der Datenbank](whats-new-in-aspnet-mvc-4/_static/image78.png "erstellen die datenbankzeichenfolge")

    *Erstellen der Datenbank*
7. Die Verbindungszeichenfolge, die Sie für die Verbindung mit SQL-Datenbank in Windows Azure verwendet werden, ist innerhalb der Verbindungstyp Standard Textfeld angezeigt. Klicken Sie dann auf **Weiter**.

    ![Auf der SQL-Datenbank-Verbindungszeichenfolge](whats-new-in-aspnet-mvc-4/_static/image79.png "auf SQL-Datenbank-Verbindungszeichenfolge")

    *Auf der SQL-Datenbank-Verbindungszeichenfolge*
8. In der **Vorschau** auf **veröffentlichen**.

    ![Veröffentlichen der Webanwendung](whats-new-in-aspnet-mvc-4/_static/image80.png "Veröffentlichen der Webanwendung")

    *Veröffentlichen der Webanwendung*
9. Sobald der Veröffentlichungsprozess abgeschlossen ist, öffnet Ihr Standardbrowser die veröffentlichte Website.

    ![Anwendung auf Windows Azure veröffentlicht](whats-new-in-aspnet-mvc-4/_static/image81.png "Anwendung auf Windows Azure veröffentlicht")

    *Anwendung in Windows Azure veröffentlicht*
