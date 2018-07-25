---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Verwenden der Seitenprüfung in Visual Studio 2012 | Microsoft-Dokumentation
author: rick-anderson
description: In dieser praktischen Übungseinheit werden Sie feststellen, ein neues Tool zum Suchen und Beheben von Problemen der Webseite in Visual Studio – die Seitenprüfung. Die Seitenprüfung ist ein neues tool, dass b...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: ac945a23dc6ef060340320d047f13c8e81057138
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833671"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Verwenden der Seitenprüfung in Visual Studio 2012
====================
durch [Web Camps Team](https://twitter.com/webcamps)

> In dieser praktischen Übungseinheit werden Sie feststellen, ein neues Tool zum Suchen und Beheben von Problemen der Webseite in Visual Studio – die Seitenprüfung.
> 
> Seitenprüfung ist ein neues Tool, das browserdiagnosetools zu Visual Studio und bietet eine integrierte Erfahrung zwischen Browser, ASP.NET und Quellcode. Es rendert eine Webseite (HTML, Web Forms, ASP.NET MVC oder Web Pages) direkt in Visual Studio-IDE und können Sie sowohl den Quellcode und die resultierende Ausgabe zu überprüfen. Der Seitenprüfung können Sie ganz einfach eine Website zu zerlegen und schnell Seiten von Grund auf neu erstellen, um Probleme schnell diagnostizieren.
> 
> Heute haben wir eine Reihe von Web-Frameworks, die flexible und skalierbare Websites rechtzeitig, z. B. ASP.NET MVC und Web Forms zu erstellen. Andererseits, ruft es schwieriger Probleme auf den Seiten zu finden, da die IDE die Designeransicht in der Template-basierten Seiten und dynamische Inhalte nicht unterstützt. Daher müssen diese Websites geöffnet werden, in einem Browser, um festzustellen, wie sie zu einem Benutzer angezeigt werden.
> 
> Webentwickler externe Tools verwenden, um Probleme zu finden, die regelmäßig im Browser ausgeführt wird. Klicken Sie dann, sie zur IDE zurückzukehren, und beginnen. Dies hin-und Aktivität zwischen der IDE, Browser und die Tools zur profilerstellung kann ineffizient sein und in einigen Fällen erfordert eine neue Bereitstellung und den Cache bereinigen jedes Mal, die Sie ein Problem reproduzieren möchten.
> 
> Page Inspector schließt eine Lücke bei der Webentwicklung zwischen dem Client (Browsertools) und dem Server ((ASP.NET und Quellcode-Code) durch das Zusammenführen von das beste aus beiden Welten, die mit einem kombinierten Satz von Features.
> 
> Mit Page Inspector können Sie sehen, welche Elemente in den Quelldateien (einschließlich serverseitigem Code) auf das HTML-Markup im Browser gerendert werden ausgelöst haben. Der Seitenprüfung können Sie das Ändern von CSS-Eigenschaften und DOM-Elementattribute, um die Änderungen sofort im Browser anzuzeigen.
> 
> Dieser praktischen Übungseinheit wird führen Sie durch die Funktionen der Seitenprüfung und zeigen Ihnen, wie Sie sie zum Beheben von Problemen in Webanwendungen verwenden können. **Diese Übungseinheit enthält zwei Übungen, ähnlich wie Flows verwenden, aber als Ziel verschiedene Technologien. Wenn Sie ein ASP.NET MVC-Entwickler sind, führen Sie die Übung eine; Wenn Sie eine WebForms Entwickler folgen Übung zwei sind**.
> 
> Dieses Lab führt Sie durch die Verbesserungen und neue Funktionen, die zuvor beschriebenen durch Anwenden von geringen Änderungen mit einer Beispiel-Webanwendung, die im Quellordner bereitgestellt.
> 
> Alle Beispielcode und Ausschnitte sind im Web Camps Training Kit unter enthalten [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Zerlegen einer Website mit der Seitenprüfung
- Überprüfen und die Vorschau der Änderungen von CSS-Formatvorlagen mit der Seitenprüfung
- Erkennen und Beheben von Problemen in Ihren Webseiten verwenden der Seitenprüfung

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen Folgendes, um diese testumgebung abzuschließen:

- [Microsoft Visual Studio Express 2012 für Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).
- InternetExplorer 9 oder höher

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Dieser praktischen Übungseinheit enthält die folgenden Übungen:

1. [Übung 1: Verwenden der Seitenprüfung in ASP.NET MVC-Projekte](#Exercise1)
2. [Übung 2: Verwenden der Seitenprüfung in WebForms-Projekte](#Exercise2)

> [!NOTE]
> Jede Übung umfasst eine ab Lösung, die im Ordner "Begin" von der Übung, mit dem Sie jede Übung unabhängig von den anderen verfolgen kann. In den Quellcode für eine Übung finden Sie auch einen End-Ordner, die Visual Studio-Projektmappe mit dem Code, die aus der Schritte in der entsprechenden Übung enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie zusätzliche Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


Geschätzte Zeit für diese testumgebung abzuschließen: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Übung 1: Verwenden der Seitenprüfung in ASP.NET MVC-Projekte

In dieser Übung erfahren Sie, wie Sie eine Vorschau anzeigen und Debuggen einer **ASP.NET MVC 4** Lösung **Page Inspector**. Führen Sie zunächst eine kurze Tour durch das Tool Informationen die Funktionen, die im Web Prozess Debuggen zu erleichtern. Klicken Sie dann arbeiten Sie auf einer Webseite, die Formatierung Probleme enthält. Erfahren Sie, wie der Seitenprüfung verwenden, um den Quellcode zu finden, der das Problem generiert, und beheben.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen der Seitenprüfung

In dieser Aufgabe lernen Sie, wie Sie die Seitenprüfung im Kontext eines ASP.NET MVC 4-Projekts zu verwenden, eine Fotogalerie anzeigt.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex1-MVC4/Anfang/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Suchen Sie im Projektmappen-Explorer **"Index.cshtml"** Ansicht der **/Views/Home** Projektordner, der rechten Maustaste darauf und wählen Sie **in Seitenprüfung anzeigen**.

    ![Auswählen einer Datei in der Seitenprüfung Vorschau](using-page-inspector-in-visual-studio-2012/_static/image1.png "Auswählen einer Datei, die in der Seitenprüfung (Vorschau)")

    *Auswählen einer Datei, die in der Seitenprüfung (Vorschau)*
3. Zeigt das Fenster für die Seitenprüfung die */Home/Index* URL zugeordnet, die Quelle anzeigen, die Sie ausgewählt haben.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Den ersten Kontakt mit der Seitenprüfung*

    Das Tool Page Inspector ist in der Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten zusammen mit einem leistungsfähigen HTML-Profiler-Browser. Beachten Sie, dass Sie keine führen Sie die Projektmappe, um Ihre Seiten zu überprüfen.

    > [!NOTE]
    > Wenn die Breite der Browser der Seitenprüfung kleiner als die Breite der Seite geöffnet ist, wird die Seite nicht ordnungsgemäß angezeigt. In diesem Fall die Seitenprüfung passen.
4. Klicken Sie auf die **Dateien** Registerkarte in der Seitenprüfung.

    Sie sehen alle Quelldateien, die auf der Indexseite erstellen. Dieses Feature unterstützt, um alle Elemente auf einen Blick zu identifizieren, insbesondere, wenn Sie mit Teilansichten und Vorlagen arbeiten. Beachten Sie, dass Sie auch alle Dateien öffnen können, wenn Sie auf die Links klicken.

    ![Registerkarte die-Dateien](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Registerkarte "Dateien"*
5. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie das Auswählen eines Elements der Seite, und finden Sie unter den HTML- und Razor-Code.

    ![Umschaltfläche-des-Prüfung-Modus](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Überprüfungsmodus Umschaltfläche*
6. Bewegen Sie im Browser der Seitenprüfung den Mauszeiger über die Seitenelemente. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp wird angezeigt, und die entsprechende Quellmarkup oder den Code in Visual Studio-Editor hervorgehoben ist.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Überprüfungsmodus in Aktion*

    > [!NOTE]
    > Maximieren Sie die Seite Inspektor-Fenster nicht, oder Sie werden nicht in der Lage sind, finden in der Registerkarte "Vorschau" zeigt den Quellcode. Wenn Sie das Element in der Seitenprüfung auf, wenn dieses maximiert ist, der Quellcode der Auswahl wird angezeigt, aber es wird die Seitenprüfung Fenster ausgeblendet.

    Wenn Sie besondere Aufmerksamkeit schenken der **"Index.cshtml"** -Datei, werden Sie feststellen, dass der Teil des Quellcodes, die das ausgewählte Element generiert hervorgehoben ist. Dieses Feature erleichtert die Bearbeitung von langen Quelldateien, die eine direkte und schnelle Möglichkeit zum Zugriff auf den Code bereitstellen.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Untersuchen von Elementen*
7. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche (![wählen Sie die Registerkarte "HTML", um den HTML-Code im Browser Seitenprüfung gerendert anzuzeigen.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Wählen Sie die Registerkarte \"HTML\", um den HTML-Code im Browser Seitenprüfung gerendert anzuzeigen.") ), deaktivieren Sie den Cursor.
8. Wählen Sie die **HTML** Registerkarte zum Anzeigen des HTML-Codes im Browser Seitenprüfung gerendert.
9. Klicken Sie im HTML-Markup suchen Sie das Listenelement mit dem Koala-Link, und wählen Sie sie.

    Beachten Sie, dass wenn Sie den Code auswählen, wird die entsprechende Ausgabe automatisch im Browser hervorgehoben. Diese Funktion ist nützlich, um festzustellen, wie ein HTML-Block auf der Seite gerendert wird.

    ![Auswählen von HTML-Element auf der Seite](using-page-inspector-in-visual-studio-2012/_static/image8.png "auswählen HTML-Element auf der Seite")

    *HTML-Element auf der Seite auswählen*
10. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche aktivieren *Überprüfungsmodus* , und klicken Sie auf der Navigationsleiste auf. Rechts von der HTML-Code, klicken Sie im Bereich "Formate" sehen Sie eine Liste mit den CSS-Formatvorlagen auf das ausgewählte Element angewendet.

    > [!NOTE]
    > Da der Header einen Teil des Layouts Standort ist, wird auch der Seitenprüfung geöffnet \_Layout.cshtml-Datei und markieren Sie das Segment des Codes beeinträchtigt.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Ermittlung von Formatvorlagen und die Quelldateien eines ausgewählten Elements*
11. Zeigen Sie mit dem Mauszeiger von Toggle-Prüfung aktiviert auf nachfolgend der blauen Leiste für die ausgewählte, und klicken Sie auf die halbe Kreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image10.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Formate die **Hintergrundbild** Element unter den **.main-Content** Gruppe. **Deaktivieren Sie** der **Hintergrundbild** und was passiert. Beachten Sie, dass der Browser die Änderungen sofort widerspiegeln wird und der Kreis wird ausgeblendet.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte Seite Inspektor Formatvorlagen anwenden wirken sich nicht auf das ursprüngliche Stylesheet aus. Sie können Stile deaktivieren oder ändern ihre Werte so oft wie Sie möchten, aber sie werden nach der Aktualisierung der Seite wiederhergestellt werden.

    ![Aktivieren und Deaktivieren von CSS-Formatvorlagen](using-page-inspector-in-visual-studio-2012/_static/image11.png "aktivieren und Deaktivieren von CSS-Formatvorlagen")

    *Aktivieren und Deaktivieren von CSS-Formatvorlagen*
13. Klicken Sie nun die "**Ihr Logo hier einfügen**' Text in den Header mit den Überprüfungsmodus.
14. In der **Stile** Registerkarte die **Font-Size** CSS-Attribut unter dem **.site-Titel** Gruppe. Doppelklicken Sie auf den Wert des Attributs ein, und Ersetzen Sie den 2.3 Em-Wert mit **3 Em**, und drücken Sie dann die **EINGABETASTE**. Beachten Sie, dass der Titel größere aussieht.

    ![Ändern von CSS-Werten in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image12.png "Ändern von CSS-Werten in der Seitenprüfung")

    *Ändern von CSS-Werten in der Seitenprüfung*
15. Klicken Sie auf die **Formatvorlagen verfolgen** Registerkarte im rechten Bereich der Seitenprüfung. Dies ist eine alternative Möglichkeit zum finden in alle Stile an, die auf die Auswahl, sortiert nach dem Attributnamen angewendet.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Ablaufverfolgung von CSS-Stilen des ausgewählten Elements*
16. Ein weiteres Feature von der Seitenprüfung ist im Layoutbereich. Verwenden den Überprüfungsmodus, wählen Sie die Navigationsleiste, und klicken Sie dann auf die **Layout** Registerkarte im rechten Bereich. Es wird die genaue Größe des ausgewählten Elements sowie seine Größe Offset, Rand, Auffüllung und Rahmen angezeigt. Beachten Sie, dass Sie auch die Werte aus dieser Sicht ändern können.

    ![Elementlayout in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image14.png "Elementlayout in der Seitenprüfung")

    *Elementlayout in der Seitenprüfung*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Problemen in der Fotogalerie

Wie würden Sie eine diagnose Webseiten-Probleme mit früheren Versionen von Visual Studio? Sie sind Ihnen wahrscheinlich vertraut mit Web debugging-Tools, die außerhalb der Visual Studio-IDE, z. B. Internet Explorer-Entwicklertools oder Firebug ausgeführt. -Browser nutzen nur HTML-Code, Skripts und Formatvorlagen, während eine zugrunde liegende Framework den HTML-Code generiert, die gerendert wird. Aus diesem Grund müssen Sie häufig die gesamte Website ein, um festzustellen, wie Webseiten aussehen bereitstellen.

Wahrscheinlich haben Sie diese Schritte ausgeführt, wenn gewünscht, zum Erkennen und Beheben eines Problems auf der Website:

1. Führen Sie die Projektmappe in Visual Studio, oder stellen Sie die Seite, auf dem Webserver.
2. Öffnen Sie im Browser die Entwicklertools, die Sie verwenden, oder öffnen Sie einfach den Quellcode und die Stile und versucht, das Problem abzugleichen. Um die Dateien zu finden, Sie hätten die &quot;Suche&quot; oder &quot;Suche in Dateien&quot; Funktionen mit dem Namen der Style-Klassen.
3. Sobald der Fehler festgestellt wird, beenden Sie den Webbrowser und dem Server.
4. Löschen Sie den Browsercache.
5. Zurück zu Visual Studio eine Lösung anwenden. Wiederholen Sie die einzelnen Schritte zum Testen.

Wie in ASP.NET MVC 4 keine echten WYSIWYG vorhanden ist, werden die meisten Probleme Stil in einem späteren Zeitpunkt nach dem Ausführen oder das Bereitstellen der Webanwendung erkannt. Bei der Seitenprüfung ist es nun möglich, einer beliebigen Seite Vorschau anzeigen, ohne das Ausführen der Projektmappe.

In dieser Aufgabe Sie verwenden die seitenprüfung und beheben Sie einige Probleme der Fotogalerie-Anwendung.

1. Verwenden der Seitenprüfung, suchen Sie die **registrieren** und **melden Sie sich bei** Links auf der linken Seite des Headers.

    Beachten Sie, dass die Links werden nicht an der erwarteten Stelle auf der rechten Seite angezeigt, und sie, wie eine Liste mit Aufzählungszeichen angezeigt werden. Sie nun die Links, rechts ausgerichtet und neu formatieren sie entsprechend.

    ![Suchen die Registrierung und das Protokoll in Links](using-page-inspector-in-visual-studio-2012/_static/image15.png "suchen die Registrierung und das Protokoll in Links")

    *Suchen die Registrierung und das Protokoll in links*
2. Ein/aus-Überprüfungsmodus ausgewählt haben klicken Sie auf Nähe, aber nicht auf den Link "Register", um den Code zu öffnen.

    Beachten Sie, die der Quellcode der Links sich in befindet der  **\_LoginPartial.cshtml** Datei nicht mit der Datei "Index.cshtml" noch die \_Layout.cshtml, die das platziert sind, können Sie zunächst den Artikel. Sie haben direkt in der Quelldatei platziert wurde.
3. In der **Stile** Registerkarte, zu suchen, und klicken Sie auf die **<section> #login</section>** Element, das den HTML--Container, damit diese Links ist.

    Beachten Sie, dass die **#login** Stil befindet sich automatisch im **"Site.CSS"** nach anklicken. Darüber hinaus ist der Code nun markiert.

    ![Wählen die CSS-Formatvorlagen](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS-Formate auswählen")

    *Wählen die CSS-Formatvorlagen*
4. Heben Sie die auskommentierung der **Text-align** -Attribut in der hervorgehobene Code, durch das Entfernen der öffnenden und schließenden Zeichen, und speichern Sie die **"Site.CSS"** Datei.

    Page Inspector ist über alle anderen Dateien, aus denen die aktuelle Seite, und es kann erkennen, wenn eine dieser Dateien ändern. Sie benachrichtigt, wenn die aktuelle Seite im Browser nicht mit Quelldateien synchronisiert ist.
5. Klicken Sie im Browser der Seitenprüfung auf der Leiste befindet sich unter der Adressleiste ein, um die Seite erneut zu laden.

    ![Die Seite erneut zu laden](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Die Seite erneut zu laden*

    Die Links sind jetzt auf der rechten Seite, aber sie noch sehen, wie eine Liste mit Aufzählungszeichen. Nun, Sie entfernen die Aufzählungszeichen und Links horizontal ausrichten.

    ![Aktualisierte Seite](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Aktualisierte Seite*
6. Mit den Überprüfungsmodus, aktivieren Sie keines der **&lt;li&gt;** Elemente, die enthalten die &quot;registrieren&quot; und &quot;melden Sie sich&quot; Links. Klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element für den Zugriff auf **Styles.css** Code.

    ![Suchen den Stil](using-page-inspector-in-visual-studio-2012/_static/image19.png "styl suchen")

    *Suchen das Format*
7. In **"Style.CSS"**, entfernen Sie den Code für **#login li** Elemente. Formatvorlage aus, die Sie hinzufügen möchten, wird Aufzählungszeichen ausblenden und Anzeigen der Elemente horizontal.

    ![Die Links für die Anmeldung Umformatierung](using-page-inspector-in-visual-studio-2012/_static/image20.png "Umformatierung die Login-Links")

    *Die Links für die Anmeldung Umformatierung*
8. Speichern Sie **"Style.CSS"** Datei, und klicken Sie auf der Leiste befindet sich unter der Adresse um die Seite erneut zu laden. Beachten Sie, dass Links richtig angezeigt werden.

    ![Links rechts ausgerichtet](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links ausgerichtet, auf die rechte Seite")

    *Links, rechts ausgerichtet*
9. Abschließend ändern Sie den headertitel. Klicken mit den Überprüfungsmodus **Ihr Logo hier einfügen** Text "und" GET-Anforderung an den Quellcode, die es generiert.
10. Nachdem Sie sich befinden  **\_Layout.cshtml**, ersetzen Sie "**Ihr Logo hier einfügen**"Text mit"**Fotogalerie**". Speichern und Browser der Seitenprüfung zu aktualisieren.

    ![Zuweisen eines neuen Titels](using-page-inspector-in-visual-studio-2012/_static/image22.png "einen neuen Titel zuweisen")

    *Zuweisen eines neuen Titels*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Seite "Photo Gallery" aktualisiert*
11. Abschließend wählen Sie zum Abschluss der **Registerseite** -Projekt, und drücken Sie **F5** zum Ausführen der app. Sehen Sie sich alles, was die Änderungen wie erwartet funktionieren.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Übung 2: Verwenden der Seitenprüfung in WebForms-Projekte

In dieser Übung lernen Sie, wie Sie eine Vorschau anzeigen und Debuggen einer WebForms-Lösung, die unter Verwendung der Seitenprüfung. Sie führt zunächst eine kurze Tour durch das Tool die Funktionen der Seitenprüfung zu finden, die im Web Prozess Debuggen zu erleichtern. Klicken Sie dann arbeiten Sie auf einer Webseite, die Formatierung Probleme enthält. Erfahren Sie, wie der Seitenprüfung verwenden, um den Quellcode zu finden, der das Problem generiert, und beheben.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen der Seitenprüfung

In dieser Aufgabe lernen Sie, wie Sie die Funktionen der Seitenprüfung im Kontext eines Web Forms-Projekts zu verwenden, eine Fotogalerie anzeigt.

1. Öffnen der **beginnen** Lösung controllerarbeitsverzeichnis **Quelle/Ex2-WebForms/Anfang/** Ordner.

   1. Sie müssen einige fehlende NuGet-Pakete herunterladen. bevor Sie fortfahren. Zu diesem Zweck klicken Sie auf die **Projekt** Menü **NuGet-Pakete verwalten**.
   2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
   3. Abschließend erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

      > [!NOTE]
      > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken in Ihrem Projekt, Versand Verringern der Projektgröße. Mit NuGet Power Tools können werden durch Angabe von Versionen des Pakets in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, die, das Sie das Projekt ausführen, können. Deshalb müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit wird.
2. Suchen Sie im Projektmappen-Explorer **"default.aspx"** Seite der rechten Maustaste darauf und wählen Sie **in Seitenprüfung anzeigen**.

    ![Öffnen "default.aspx" mit der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image24.png "öffnen \"default.aspx\" mit der Seitenprüfung")

    *Öffnen Sie "default.aspx" mit der Seitenprüfung*
3. Das Fenster für die Seitenprüfung zeigt "default.aspx".

    ![Anzeigen von "default.aspx" in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image25.png "\"default.aspx\" in der Seitenprüfung anzeigen")

    *"Default.aspx" in der Seitenprüfung anzeigen*

    Das Tool Page Inspector ist in der Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten Browser, zusammen mit einem leistungsstarke HTML-Profiler, der den ausgewählten Code angezeigt wird. Beachten Sie, dass Sie keine führen Sie die Projektmappe, um Ihre Seiten zu überprüfen.

    > [!NOTE]
    > Wenn die Breite der Browser der Seitenprüfung kleiner als die Breite der Seite geöffnet ist, wird die Seite nicht ordnungsgemäß angezeigt. In diesem Fall die Seitenprüfung passen.
4. Klicken Sie auf die **Dateien** Registerkarte in der Seitenprüfung.

    Sie sehen alle Quelldateien, die die gerenderte Standardseite erstellen. Dies ist ein nützliches Feature, um alle Elemente auf einen Blick zu identifizieren, insbesondere dann, wenn Sie mit Benutzersteuerelementen und Masterseiten arbeiten. Beachten Sie, dass Sie auch auf jede der Dateien navigieren können.

    ![Registerkarte "Dateien"](using-page-inspector-in-visual-studio-2012/_static/image26.png "Registerkarte die Dateien")

    *Registerkarte "Dateien"*
5. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie das Auswählen eines Elements der Seite, und finden Sie in der HTML-Code und ASPX-Quelle.

    ![Überprüfungsmodus Umschaltfläche](using-page-inspector-in-visual-studio-2012/_static/image27.png "Überprüfungsmodus ein/aus-Schaltfläche")

    *Überprüfungsmodus Umschaltfläche*
6. Bewegen Sie im Browser Seitenprüfung den Mauszeiger über die Seitenelemente. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp wird angezeigt, und die entsprechende Quellmarkup oder den Code in Visual Studio-Editor hervorgehoben ist.

    ![Überprüfungsmodus in Aktion](using-page-inspector-in-visual-studio-2012/_static/image28.png "Überprüfungsmodus in Aktion")

    *Überprüfungsmodus in Aktion*

    > [!NOTE]
    > Maximieren Sie die Seite Inspektor-Fenster nicht, oder Sie werden nicht in der Lage sind, finden in der Registerkarte "Vorschau" zeigt den Quellcode. Wenn Sie das Element in der Seitenprüfung auf, wenn dieses maximiert ist, der Quellcode der Auswahl wird angezeigt, aber es wird die Seitenprüfung Fenster ausgeblendet.

    Wenn Sie besondere Aufmerksamkeit schenken **"default.aspx"** -Datei, werden Sie feststellen, dass der Teil des Quellcodes, die das ausgewählte Element generiert hervorgehoben ist. Dieses Feature erleichtert die Edition von langen Quelldateien, die eine direkte und schnelle Möglichkeit zum Zugriff auf den Code bereitstellen.

    ![Überprüfen die Elemente](using-page-inspector-in-visual-studio-2012/_static/image29.png "Elemente untersuchen")

    *Untersuchen von Elementen*
7. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.](using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), befindet sich in der Seitenprüfung Registerkarten, um den Cursor zu deaktivieren.
8. Wählen Sie die **HTML** Registerkarte zum Anzeigen des HTML-Codes im Browser Seitenprüfung gerendert.
9. Suchen Sie in der HTML-Code das Listenelement mit dem Koala-Link, und wählen Sie sie.

    Beachten Sie, dass wenn Sie den Code auswählen, die entsprechende Ausgabe automatisch hervorgehoben Browser ist. Diese Funktion ist nützlich, um festzustellen, wie ein HTML-Block auf der Seite gerendert wird.

    ![Auswählen eines HTML-Elements auf der Seite](using-page-inspector-in-visual-studio-2012/_static/image31.png "ein HTML-Element auf der Seite auswählen")

    *Ein HTML-Element auswählen auf der Seite*
10. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche aktivieren *Überprüfungsmodus* , und klicken Sie auf der Navigationsleiste auf. Rechts von der HTML-Code, klicken Sie im Bereich "Formate" sehen Sie eine Liste mit den CSS-Formatvorlagen auf das ausgewählte Element angewendet.

    > [!NOTE]
    > Da der Header das Layout der Website gehört, der Seitenprüfung außerdem Site.Master-Datei zu öffnen und markieren Sie das Segment des Codes beeinträchtigt.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Ermitteln von Formatvorlagen und die Quelldateien eines ausgewählten Elements")

    *Ermittlung von Formatvorlagen und die Quelldateien eines ausgewählten Elements*
11. Bewegen Sie mit dem Mauszeiger von Toggle-Prüfung aktiviert den Mauszeiger unterhalb der Menüleiste, und klicken Sie auf die leere Halbkreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image33.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Formate die **Hintergrundbild** Element unter den **.main-Content** Gruppe. **Deaktivieren Sie** der **Hintergrundbild** und was passiert. Beachten Sie, dass der Browser die Änderungen sofort widerspiegeln wird und der Kreis wird ausgeblendet.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte Seite Inspektor Formatvorlagen anwenden wirken sich nicht auf das ursprüngliche Stylesheet aus. Sie können Stile deaktivieren oder ändern ihre Werte so oft wie Sie möchten, aber sie werden nach der Aktualisierung der Seite wiederhergestellt werden.

    ![Aktivieren und Deaktivieren von CSS-styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "aktivieren und Deaktivieren von CSS-Formatvorlagen")

    *Aktivieren und Deaktivieren von CSS-Formatvorlagen*
13. Klicken Sie nun die "**Ihre** **Logo hier einfügen"** Text auf den Header mit den Überprüfungsmodus.
14. In der **Stile** Registerkarte die **Font-Size** CSS-Attribut unter dem **.site-Titel** Gruppe. Doppelklicken Sie auf das Attribut nach, um den Wert zu bearbeiten. Ersetzen Sie die 2.3em-Wert mit **3em**, und drücken Sie dann die EINGABETASTE. Beachten Sie, dass der Titel größere aussieht.

    ![Ändern von CSS-Werten in der Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Ändern von CSS-Werten in der Seitenprüfung")

    *Ändern von CSS-Werten in der Seitenprüfung*
15. Klicken Sie auf die **Formatvorlagen verfolgen** Registerkarte im rechten Bereich der Seitenprüfung. Dies ist eine alternative Möglichkeit zum finden in alle Stile an, die auf die Auswahl, sortiert nach dem Attributnamen angewendet.

    ![Ablaufverfolgung von CSS-Stilen des ausgewählten Elements](using-page-inspector-in-visual-studio-2012/_static/image36.png "Ablaufverfolgung von CSS-Stilen des ausgewählten Elements")

    *Ablaufverfolgung von CSS-Stilen des ausgewählten Elements*
16. Ein weiteres Feature von der Seitenprüfung ist im Layoutbereich. Verwenden den Überprüfungsmodus, wählen Sie die Navigationsleiste, und klicken Sie dann auf die **Layout** Registerkarte im rechten Bereich. Es wird die genaue Größe des ausgewählten Elements sowie seine Größe Offset, Rand, Auffüllung und Rahmen angezeigt. Beachten Sie, dass Sie auch die Werte aus dieser Sicht ändern können.

    ![Elementlayout in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image37.png "Elementlayout in der Seitenprüfung")

    *Elementlayout in der Seitenprüfung*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Problemen in der Fotogalerie

Wie würden Sie eine diagnose Webseiten-Probleme mit früheren Versionen von Visual Studio? Sie sind Ihnen wahrscheinlich vertraut mit Web debugging-Tools, die außerhalb der Visual Studio-IDE, z. B. Internet Explorer-Entwicklertools oder Firebug ausgeführt. -Browser nutzen nur HTML-Code, Skripts und Formatvorlagen, während eine zugrunde liegende Framework den HTML-Code generiert, die gerendert wird. Aus diesem Grund müssen Sie häufig die gesamte Website ein, um festzustellen, wie Webseiten aussehen bereitstellen.

Wahrscheinlich haben Sie diese Schritte ausgeführt, wenn gewünscht, zum Erkennen und Beheben eines Problems auf der Website:

1. Führen Sie die Projektmappe in Visual Studio, oder stellen Sie die Seite, auf dem Webserver.
2. Öffnen Sie im Browser die Entwicklertools, die Sie verwenden, oder öffnen Sie einfach den Quellcode und die Stile und versucht, das Problem abzugleichen. Um die Dateien zu finden, Sie hätten die &quot;Suche&quot; oder &quot;Suche in Dateien&quot; Funktionen mit dem Namen der Style-Klassen.
3. Sobald der Fehler festgestellt wird, beenden Sie den Webbrowser und dem Server.
4. Löschen Sie den Browsercache.
5. Zurück zu Visual Studio eine Lösung anwenden. Wiederholen Sie die einzelnen Schritte zum Testen.

Wie es keine echte WYSIWYG in ASP.NET WebForms ist, werden einige Formatprobleme in einem späteren Zeitpunkt nach dem Ausführen oder die Bereitstellung erkannt. Bei der Seitenprüfung ist es nun möglich, einer beliebigen Seite Vorschau anzeigen, ohne das Ausführen der Projektmappe.

In dieser Aufgabe verwenden Sie die seitenprüfung zum Beheben der Probleme der Fotogalerie-Anwendung. In die folgenden Schritte aus Sie erkennen und beheben Sie schnell ein Problem mit der einfachen Stile im Header.

1. Suchen Sie mithilfe der Seite Überprüfung der **registrieren** und **anmelden** Links auf der linken Seite des Headers.

    Beachten Sie, dass der Link nicht an der erwarteten Stelle auf der rechten Seite angezeigt wird. Sie jetzt den Link, um rechts ausgerichtet und entsprechend umzugestalten.

    ![Melden Sie sich links auf der linken Seite positioniert](using-page-inspector-in-visual-studio-2012/_static/image38.png "melden Sie sich links auf der linken Seite positioniert")

    *Link anmelden auf der linken Seite positioniert*
2. Wählen Sie den Link "Anmelden" So öffnen Sie dessen Code mit ein-/ausschalten Überprüfungsmodus ausgewählt.

    Beachten Sie, die der Quellcode der Link sich in befindet der **Site.Master** -Datei nicht in die Seite "default.aspx" wird der Ort können Sie den Artikel an erste Stelle; Sie direkt in der Quelldatei platziert wurden.
3. In der **Stile** Registerkarte, zu suchen, und klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element, das den HTML--Container, damit diese Links ist.

    Beachten Sie, dass die **#login** Stil befindet sich automatisch im **"Site.CSS"** nach anklicken. Darüber hinaus ist der Code nun markiert.

    ![Wählen die CSS-Formatvorlagen](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS-Formate auswählen")

    *Wählen die CSS-Formatvorlagen*
4. Heben Sie die auskommentierung der **Text-align** -Attribut in der hervorgehobene Code, durch das Entfernen der öffnenden und schließenden Zeichen, und speichern Sie die **"Site.CSS"** Datei.

    Page Inspector ist über alle anderen Dateien, aus denen die aktuelle Seite, und es kann erkennen, wenn eine dieser Dateien ändern. Sie benachrichtigt, wenn die aktuelle Seite im Browser nicht mit Quelldateien synchronisiert ist.
5. Klicken Sie im Browser der Seitenprüfung auf der Leiste befindet sich unter der Adressleiste ein, um die Änderungen zu speichern und Laden Sie die Seite.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Die Seite erneut zu laden*

    Die Links sind jetzt auf der rechten Seite, aber sie noch sehen, wie eine Liste mit Aufzählungszeichen. Nun, Sie entfernen die Aufzählungszeichen und Links horizontal ausrichten.

    ![Aktualisierte Seite](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Aktualisierte Seite*
6. Mit den Überprüfungsmodus, aktivieren Sie keines der **&lt;li&gt;** Elemente, die enthalten die &quot;registrieren&quot; und &quot;melden Sie sich&quot; Links. Klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element für den Zugriff auf **Styles.css** Code.

    ![Suchen den Stil](using-page-inspector-in-visual-studio-2012/_static/image42.png "styl suchen")

    *Suchen das Format*
7. In **"Style.CSS"**, entfernen Sie den Code für **#login li** Elemente. Formatvorlage aus, die Sie hinzufügen möchten, wird Aufzählungszeichen ausblenden und Anzeigen der Elemente horizontal.

    ![Die Links für die Anmeldung Umformatierung](using-page-inspector-in-visual-studio-2012/_static/image43.png "Umformatierung die Login-Links")

    *Die Links für die Anmeldung Umformatierung*
8. Speichern Sie **"Style.CSS"** Datei, und klicken Sie auf der Leiste befindet sich unter der Adresse um die Seite erneut zu laden. Beachten Sie, dass Links richtig angezeigt werden.

    ![Links rechts ausgerichtet](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links ausgerichtet, auf die rechte Seite")

    *Links, rechts ausgerichtet*
9. Abschließend ändern Sie den headertitel. Anstatt die Suche nach der "**Ihr Logo hier einfügen"** Text in allen Dateien, die den Überprüfungsmodus verwenden, um auf den Text ein, und erhalten auf den Quellcode, die es generiert.

    ![Suchen den Titel der Website](using-page-inspector-in-visual-studio-2012/_static/image45.png "suchen den Titel der Website")

    *Suchen den Titel der Website*
10. Nachdem Sie sich befinden **Site.Master**, ersetzen Sie die "**Ihr Logo hier einfügen**"Text mit"**Fotogalerie**". Speichern und Browser der Seitenprüfung zu aktualisieren.

    ![Fotogalerie-Seite aktualisiert](using-page-inspector-in-visual-studio-2012/_static/image46.png "Fotogalerie-Seite aktualisiert")

    *Seite "Photo Gallery" aktualisiert*
11. Aktivieren Sie schließlich **F5** zum Ausführen der app der Überprüfung alle Änderungen wie erwartet funktionieren.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Von dieser praktischen Übungseinheit haben Sie gelernt, wie der Seitenprüfung verwenden, um Ihre Webanwendung in der Vorschau anzuzeigen, ohne sich erneut, und führen die Website in einem Browser. Darüber hinaus haben Sie gelernt, wie Sie schnell finden und Fehler zu beheben, indem Sie auf die direkt aus der gerenderten Ausgabe auf den Quellcode zugreifen.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für Web

Sie installieren können **Microsoft Visual Studio Express 2012 für Web** oder einem anderen &quot;Express&quot; Version verwenden, die **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Die folgenden Anweisungen führen Sie über die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen ihn, und suchen Sie nach dem Produkt &quot; <em>Visual Studio Express 2012 für das Web mit Windows Azure SDK</em>&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** gelangen Sie zum Herunterladen und installieren Sie diese zuerst.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Installieren von Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Installieren von Visual Studio Express")

    *Installieren von Visual Studio Express*
4. Lesen Sie die Produkte Lizenzen und Begriffe, und klicken Sie auf **akzeptieren** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess zum Herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Installationsstatus*
6. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![Die Installation wurde abgeschlossen](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Die Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** schreiben zu starten und Bildschirm &quot; **VS Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** die Kachel.

    ![Visual Studio Express für Web-Kachel](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Visual Studio Express für Web-Kachel*
