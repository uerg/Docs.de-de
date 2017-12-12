---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Mit Page Inspector in Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: "In dieser praktischen Übungseinheit werden Sie feststellen, ein neues Tool zum Suchen und Beheben von Problemen Webseite in Visual Studio - der Seitenprüfung angezeigt werden. Seitenprüfung ist ein neues tool von b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Verwenden der Seitenprüfung in Visual Studio 2012
====================
durch [Web Lager Team](https://twitter.com/webcamps)

> In dieser praktischen Übungseinheit werden Sie feststellen, ein neues Tool zum Suchen und Beheben von Problemen Webseite in Visual Studio - der Seitenprüfung angezeigt werden.
> 
> Seitenprüfung ist ein neues Tool, das Browser-Diagnose-Tools für Visual Studio und bietet eine integrierte Benutzeroberfläche den Browser, ASP.NET und Quellcode. Rendert eine Webseite (HTML, Web Forms, ASP.NET MVC oder Web Pages) direkt in der Visual Studio-IDE, und Sie können Sie den Quellcode einfügen und die resultierende Ausgabe untersuchen. Page Inspector können Sie problemlos zerlegen eine Website schnell Seiten von Grund auf neu erstellen, werden und Probleme schnell zu diagnostizieren.
> 
> Heute haben wir eine Anzahl von Web-Frameworks, die flexible und skalierbare Websites rechtzeitig, z. B. ASP.NET MVC und WebForms zu erstellen. Andererseits, ruft es schwieriger Probleme auf den Seiten zu finden, da die IDE die Designeransicht in Seiten Template-basierten und dynamische Inhalte nicht unterstützt. Aus diesem Grund haben diese Websites geöffnet werden, in einem Browser anzeigen, wie sie einem Benutzer angezeigt werden.
> 
> Webentwickler externe Tools verwenden, suchen Sie nach Problemen, die regelmäßig im Browser ausgeführt wird. Klicken Sie dann zur IDE zurückzukehren und beginnen zu beheben. Dadurch hin-und Aktivität zwischen der IDE, Browser und die Profilerstellungstools kann ineffizient sein und in einigen Fällen erfordert eine neue Bereitstellung und bereinigen jedes Mal, Sie zum Reproduzieren eines Problems möchten, Cache.
> 
> Page Inspector verbindet eine Lücke in der Webentwicklung zwischen dem Client (Browsertools) und dem Server ((ASP.NET und Quellcode-Code), durch das das beste aus beiden Welten bereit, die mit einem kombinierten Satz von Funktionen kombinieren.
> 
> Mit Page Inspector können Sie sehen, welche Elemente in den Quelldateien (einschließlich serverseitigem Code) auf das HTML-Markup im Browser gerendert werden erstellt haben. Page Inspector können Sie die CSS-Eigenschaften und DOM-Elementattribute Änderungen sofort im Browser ändern.
> 
> Dieser praktischen Übungseinheit wird die Seitenprüfung Funktionen schrittweise und zeigen, wie Sie sie zur Behebung von Problemen in Webanwendungen verwenden können. **Diese Übungseinheit enthält zwei Übungen ähnliche Arbeitsabläufe mithilfe jedoch verschiedene Technologien abzielt. Wenn Sie ein ASP.NET MVC-Entwickler sind, führen Sie eine Übung; Wenn Sie eine WebForms-Entwickler folgen Übung zwei sind**.
> 
> Diese Übung führt Sie durch die Optimierungen und neuen Funktionen, die zuvor beschriebenen durch Anwenden von kleinere Änderungen auf eine Beispielwebanwendung im Quellordner bereitgestellt.
> 
> Alle Beispielcode und Codeausschnitte sind im Web Lager Training Kit unter enthalten [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Ziele

In dieser praktischen Übungseinheit erfahren Sie, wie Sie:

- Zerlegen Sie eine Website, die mit Page Inspector
- Überprüfen und die Vorschau der CSS-Stile Änderungen mit der Seitenprüfung
- Erkennen und Beheben von Problemen in Ihren Webseiten, die mit Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Erforderliche Komponenten

Sie benötigen zum Abschließen dieser testumgebung die folgenden Elemente:

- [Microsoft Visual Studio Express 2012 für das Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) oder sogar eine höhere (Lesen [Anhang A](#AppendixA) Anleitungen zur Installation).
- InternetExplorer 9 oder höher

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Übungen

Diese praktische Übungseinheit enthält die folgenden Übungen durcharbeiten:

1. [Übung 1: Verwenden von Seitenprüfung in ASP.NET MVC-Projekte](#Exercise1)
2. [Übung 2: Verwendung Page Inspector in Web Forms-Projekten](#Exercise2)

> [!NOTE]
> Jede Übung wird eine beginnend Lösung, die im Ordner "Begin", die Ihnen ermöglicht, jede Übung unabhängig von den anderen führen die Übung beiliegen. In den Quellcode für eine Übung finden Sie auch einen End-Ordner, die durch den Code, der aus den Schritten in der entsprechenden Übung führt Visual Studio-Projektmappe enthält. Sie können diese Lösungen als Leitfaden verwenden, wenn Sie weitere Hilfe benötigen, wie Sie mithilfe dieser praktischen Übungseinheit arbeiten.


Geschätzte Zeit zum Abschließen dieser testumgebung: **30 Minuten**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Übung 1: Verwenden von Seitenprüfung in ASP.NET MVC-Projekte

In dieser Übung erfahren Sie, wie Sie in der Vorschau anzeigen und Debuggen einer **ASP.NET MVC 4** -Lösung verwenden **Page Inspector**. Führen Sie zuerst eine kurze Lap um das Tool Informationen die Funktionen, die im Web Prozess Debuggen zu vereinfachen. Klicken Sie dann, arbeiten Sie in einer Webseite, die Styling Probleme enthält. Sie erfahren, wie Sie mit Page Inspector um den Quellcode zu finden, der das Problem generiert und Behebung des Problems.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen der Seitenprüfung

In dieser Aufgabe erfahren Sie, wie mit der Seitenprüfung im Kontext eines ASP.NET MVC 4-Projekts, das ein Fotokatalog anzeigt.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex1-MVC4/Begin/** Ordner.

    1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Suchen Sie im Projektmappen-Explorer **Index.cshtml** zeigen an, unter dem **/Ansichten/Start** Projektordner, der rechten Maustaste darauf und wählen Sie **in Seitenprüfung anzeigen**.

    ![Auswählen einer Datei in der Seitenprüfung Vorschau](using-page-inspector-in-visual-studio-2012/_static/image1.png "Auswählen einer Datei, in der Vorschau in Seitenprüfung anzeigen")

    *Auswählen einer Datei, in der Vorschau in Seitenprüfung anzeigen*
3. Zeigt das Fenster für die Seitenprüfung die */Home/Index* URL zugeordnet, mit der Datenquelle anzeigen, die Sie ausgewählt haben.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Den ersten Kontakt mit der Seitenprüfung*

    Das Tool Page Inspector ist in der Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten zusammen mit einem leistungsstarken HTML-Profiler-Browser. Beachten Sie, dass Sie keine führen Sie die Projektmappe, um Ihren Seiten anzuzeigen.

    > [!NOTE]
    > Wenn die Breite des Browsers Page Inspector kleiner als die Breite der Seite geöffnet wird, werden Sie die Seite nicht ordnungsgemäß angezeigt. Wenn dies der Fall ist, passen Sie die Breite der Seitenprüfung.
4. Klicken Sie auf die **Dateien** Registerkarte in der Seitenprüfung.

    Sie sehen die Quelldateien, die die Indexseite verfassen. Diese Funktion hilft, alle Elemente auf einen Blick erkennen, insbesondere bei der Arbeit mit Teilansichten und Vorlagen. Beachten Sie, dass Sie auch alle Dateien öffnen können, wenn Sie auf die Links klicken.

    ![Die Registerkarte als Dateien](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Die Registerkarte "Dateien"*
5. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie wählen Sie ein beliebiges Element von der Seite ", und finden Sie unter der HTML- und Razor-Code.

    ![Ein-/ausschalten-Überprüfung-Modus-Schaltfläche](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Schaltfläche "Überprüfungsmodus umschalten"*
6. Bewegen Sie im Browser Seitenprüfung den Mauszeiger über die Seitenelemente. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp wird angezeigt, und das entsprechende Quellmarkup oder der Code wird in der Visual Studio-Editor hervorgehoben.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Überprüfungsmodus in Aktion*

    > [!NOTE]
    > Maximieren Sie das Page Inspector-Fenster nicht, oder Sie werden nicht in der Lage sind, finden in der Registerkarte "Vorschau" Quellcode anzeigen. Wenn Sie das Element in der Seitenprüfung auf, wenn dieses maximiert ist, der Quellcode der Auswahl erscheint, aber es wird das Fenster Page Inspector ausblenden.

    Wenn Sie Zertifikatsperrinformationen der **Index.cshtml** -Datei werden Sie feststellen, dass der Teil des Quellcodes, die das ausgewählte Element erzeugt hervorgehoben ist. Dieses Feature erleichtert die Bearbeitung der lange Quelldateien, eine direkte und schnelle Möglichkeit zum Zugriff auf den Code bereitstellen.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Überprüfen von Elementen*
7. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche (![wählen Sie die Registerkarte "HTML", um den HTML-Code im Browser Seitenprüfung gerendert anzuzeigen.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Wählen Sie die Registerkarte "HTML", um den HTML-Code im Browser Seitenprüfung gerendert anzuzeigen.") ), deaktivieren Sie den Cursor.
8. Wählen Sie die **HTML** Registerkarte an, den im Browser Seitenprüfung gerenderten HTML-Code angezeigt.
9. Klicken Sie in das HTML-Markup suchen Sie das Listenelement mit dem Link Koala, und wählen Sie sie.

    Beachten Sie, dass wenn Sie den Code auswählen, wird die entsprechende Ausgabe automatisch im Browser hervorgehoben. Diese Funktion ist nützlich, um festzustellen, wie ein HTML-Block auf der Seite gerendert wird.

    ![Auswählen von HTML-Element auf der Seite](using-page-inspector-in-visual-studio-2012/_static/image8.png "auswählen HTML-Element auf der Seite")

    *HTML-Element auf der Seite auswählen*
10. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche aktivieren *Überprüfungsmodus* , und klicken Sie auf der Navigationsleiste. Am rechten Rand der HTML-Code, klicken Sie im Bereich "Formatvorlagen" sehen Sie eine Liste mit den CSS-Formatvorlagen, die auf das ausgewählte Element angewendet.

    > [!NOTE]
    > Da der Header das Layout für die Website gehört, wird auch Page Inspector geöffnet \_Layout.cshtml-Datei und markieren Sie das Codesegment beeinflusst.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Ermittlung von Formatvorlagen und Quelldateien eines ausgewählten Elements*
11. Bewegen Sie mit dem zum ein-/ausschalten Inspektion Zeiger aktiviert den Mauszeiger unterhalb der blauen Leiste für die ausgewählte, und klicken Sie auf die halbe Kreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image10.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Formate die **Hintergrundbild** Element unter den **.main-Content** Gruppe. **Deaktivieren Sie** der **Hintergrundbild** und sehen, was passiert. Beachten Sie, dass der Browser die Änderungen sofort widerspiegelt, und der Kreis wird ausgeblendet.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte "Seite Inspektor Formatvorlagen" vornehmen, wirken sich nicht auf die ursprünglichen Stylesheet aus. Stile deaktivieren können oder ändern Sie ihre Werte so oft wie werden sollen, aber sie werden nach der Aktualisierung der Seite wiederhergestellt werden.

    ![Aktivieren und Deaktivieren von CSS-Stilen](using-page-inspector-in-visual-studio-2012/_static/image11.png "aktivieren und Deaktivieren von CSS-Stilen")

    *Aktivieren und Deaktivieren von CSS-Stilen*
13. Klicken Sie nun auf die "**Ihr Logo hier einfügen**' Text in den Header mit den Überprüfungsmodus.
14. In der **Stile** Registerkarte, suchen Sie nach der **Font-Size** CSS-Attribut unter der **.site Titel** Gruppe. Doppelklicken Sie auf den Wert des Attributs, und Ersetzen Sie den Wert 2.3 Em mit **3 Em**, und drücken Sie dann die **EINGABETASTE**. Beachten Sie, dass der Titel größere aussieht.

    ![Ändern von CSS-Werten in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image12.png "Ändern von CSS-Werten in der Seitenprüfung")

    *Ändern von CSS-Werten in der Seitenprüfung*
15. Klicken Sie auf die **Formatvorlagen** Registerkarte im rechten Bereich der Seitenprüfung befinden. Dies ist eine alternative Möglichkeit, alle Stile zur Auswahl, sortiert nach dem Attributnamen finden Sie unter.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *CSS-Formatvorlagen Verfolgen des ausgewählten Elements*
16. Eine weitere Funktion von Page Inspector ist der Layout-Bereich. Verwenden den Überprüfungsmodus, wählen Sie die Navigationsleiste, und klicken Sie dann auf die **Layout** Registerkarte im rechten Bereich. Sie sehen die genaue Größe des ausgewählten Elements sowie seine Größe Offset, Rand, Auffüllung und Rahmen. Beachten Sie, dass Sie auch die Werte aus dieser Sicht ändern können.

    ![In der Seitenprüfung Elementlayouts](using-page-inspector-in-visual-studio-2012/_static/image14.png "Elementlayouts in der Seitenprüfung")

    *In der Seitenprüfung Elementlayouts*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Formatprobleme in der Fotogalerie

Wie würden Sie Webseiten mit früheren Versionen von Visual Studio Problemdiagnose? Sie sind wahrscheinlich bereits mit Web debugging-Tools, die außerhalb der Visual Studio-IDE, z. B. Internet Explorer-Entwicklertools oder Firebug ausgeführt vertraut sind. Browser nur verstehen, HTML, Skripts und Formatvorlagen, während eine zugrunde liegende Framework den HTML-Code generiert, die gerendert werden. Aus diesem Grund müssen Sie häufig die gesamte Website, um festzustellen, wie Webseiten aussehen bereitstellen.

Diese Schritte hatte wahrscheinlich ausgeführt werden, wenn Sie ermitteln und Beheben eines Problems auf Ihre Website wollten:

1. Führen Sie die Projektmappe in Visual Studio oder Bereitstellen Sie der Seite auf dem Webserver "".
2. Öffnen Sie im Browser die Developer-Tools, die Sie verwenden, oder öffnen Sie einfach den Quellcode und die Stile und versuchen, das Problem zu entsprechen. Um die Dateien zu finden, wo Sie verwendet hätten die &quot;Suche&quot; oder &quot;Suche in Dateien&quot; Funktionen mit dem Namen der Formatvorlage Klassen.
3. Nach Erkennung des Fehlers ist beenden Sie den Webbrowser und dem Server.
4. Löschen Sie den Browsercache.
5. Zurück zu Visual Studio und einen Fix anzuwenden. Wiederholen Sie die einzelnen Schritte zum Testen.

Keine echte WYSIWYG in ASP.NET MVC 4 vorhanden ist, werden die meisten der formatvorlagenprobleme nach ausführen oder die Webanwendung bereitstellen auf einem späteren Zeitpunkt erkannt. Mit der Seitenprüfung ist es nun möglich, eine beliebige Seite Vorschau anzeigen, ohne die Projektmappe auszuführen.

In dieser Aufgabe werden Sie der seitenprüfung verwenden und einige Probleme beheben, die Fotogalerie-Anwendung.

1. Suchen Sie mit Page Inspector, die **registrieren** und **melden Sie sich** Links auf der linken Seite des Headers.

    Beachten Sie, dass die Links werden nicht an der erwarteten Stelle auf der rechten Seite angezeigt, und diese wie eine Aufzählung angezeigt. Sie werden jetzt die Links, rechts ausgerichtet und neu formatieren sie entsprechend.

    ![Suchen die Register und die Protokolldateien im Links](using-page-inspector-in-visual-studio-2012/_static/image15.png "suchen die Register und die Protokolldateien im Links")

    *Suchen die Register und die Protokolldateien im links*
2. Ein-/ausschalten-Überprüfungsmodus ausgewählt haben klicken Sie auf Schließen, aber nicht auf den Link registrieren, um ihren Code zu öffnen.

    Beachten Sie, die der Quellcode der Links sich in befindet der  **\_LoginPartial.cshtml** Datei, nicht die Index.cshtml noch die \_Layout.cshtml, die die Anwendungsmöglichkeiten sind interessante Informationen finden Sie in der ersten Stelle. Sie haben direkt in der richtigen Quelldatei platziert wurde.
3. In der **Stile** Registerkarte, suchen und klicken Sie auf die  **<section> #login</section>**  Element, das die HTML--Container, damit diese Links wird.

    Beachten Sie, dass die **#login** Stil befindet sich im automatisch **"Site.CSS" ändern** nach anklicken. Darüber hinaus wird der Code jetzt hervorgehoben.

    ![Auswählen der CSS-Stilen](using-page-inspector-in-visual-studio-2012/_static/image16.png "CSS-Stilen auswählen")

    *Die CSS-Stilen auswählen*
4. Heben Sie die auskommentierung der **Text-align** Attribut in der hervorgehobene Code, durch das Entfernen der öffnenden und schließenden Zeichen, und speichern Sie die **"Site.CSS" ändern** Datei.

    Page Inspector fähig aller anderen Dateien, aus denen die aktuelle Seite zusammensetzt, und es kann erkennen, wenn eine dieser Dateien ändern. Sie werden benachrichtigt, wenn die aktuelle Seite im Browser nicht mit Quelldateien synchronisiert ist.
5. Klicken Sie im Browser Seitenprüfung auf der Leiste befindet sich unter der Adressleiste ein, um die Seite erneut zu laden.

    ![Erneutes Laden der Seite "](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Erneutes Laden der Seite "*

    Die Links sind jetzt auf der rechten Seite, aber sie weiterhin wie folgt eine Liste mit Aufzählungszeichen aussehen. Jetzt, Sie entfernen die Aufzählungszeichen und horizontal ausgerichtet. die Links.

    ![Seite "aktualisiert"](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Seite "aktualisiert"*
6. Mit den Überprüfungsmodus, aktivieren Sie keines der  **&lt;li&gt;**  Elemente beziehen, enthalten die &quot;registrieren&quot; und &quot;melden Sie sich&quot; Links. Klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element für den Zugriff auf **Styles.css** Code.

    ![Suchen Sie das Format von](using-page-inspector-in-visual-studio-2012/_static/image19.png "suchen das Format")

    *Suchen das Format*
7. In **Style.css**, kommentieren Sie den Code für **#login li** Elemente. Die Formatvorlage, die Sie hinzufügen wird Aufzählungszeichen ausblenden und Anzeigen der Elemente horizontal.

    ![Zuweisen neuer Stile die Anmeldung Links](using-page-inspector-in-visual-studio-2012/_static/image20.png "Zuweisen neuer Stile die Login-Links")

    *Zuweisen neuer Stile die Login-links*
8. Speichern Sie **Style.css** Datei, und klicken Sie auf der Leiste befindet sich unter der Adresse um die Seite erneut zu laden. Beachten Sie, dass die Links ordnungsgemäß angezeigt.

    ![Links, rechts ausgerichtet](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links, rechts ausgerichtet")

    *Links, rechts ausgerichtet*
9. Abschließend ändern Sie den Header-Titel. Verwenden Sie den Überprüfungsmodus auf **Ihr Logo hier einfügen** Text "und" GET-Anforderung an den Quellcode, das sie generiert.
10. Jetzt sind Sie in  **\_Layout.cshtml**, ersetzen Sie "**Ihr Logo hier einfügen**'Text mit'**Fotogalerie**". Speichern und Browser der Seitenprüfung zu aktualisieren.

    ![Zuweisen eines neuen Titels](using-page-inspector-in-visual-studio-2012/_static/image22.png "Zuweisen eines neuen Titels")

    *Zuweisen eines neuen Titels*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Fotogalerie Seite aktualisiert*
11. Schließlich Teilnummern der **PhotoGallery** -Projekt, und drücken Sie **F5** um die app auszuführen. Sehen Sie sich alle Änderungen ordnungsgemäß funktionieren wie erwartet.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Übung 2: Verwendung Page Inspector in Web Forms-Projekten

In dieser Übung erfahren Sie, wie in der Vorschau anzeigen und Debuggen von einer Web Forms-Lösung, die mit Page Inspector. Zuerst führen Sie eine kurze Lap um das Tool Informationen die Page Inspector-Funktionen, die im Web Prozess Debuggen zu vereinfachen. Klicken Sie dann, arbeiten Sie in einer Webseite, die Styling Probleme enthält. Sie erfahren, wie Sie mit Page Inspector um den Quellcode zu finden, der das Problem generiert und Behebung des Problems.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Aufgabe 1: Untersuchen der Seitenprüfung

In dieser Aufgabe erfahren Sie, wie die Page Inspector-Funktionen im Kontext einer Web Forms-Projekt verwenden, das ein Fotokatalog anzeigt.

1. Öffnen der **beginnen** Lösung finden Sie unter **Quell-/Ex2-WebForms/Begin/** Ordner.

    1. Sie müssen einige fehlende NuGet-Pakete herunterladen bevor Sie fortfahren. Klicken Sie hierzu auf die **Projekt** Menü **NuGet-Pakete verwalten**.
    2. In der **NuGet-Pakete verwalten** Dialogfeld klicken Sie auf **wiederherstellen** um das Herunterladen fehlender Pakete.
    3. Schließlich erstellen Sie die Projektmappe, indem Sie auf **erstellen** | **Projektmappe**.

    > [!NOTE]
    > Einer der Vorteile der Verwendung von NuGet ist, dass Sie nicht alle Bibliotheken, die sich in Ihrem Projekt liefern Verringern der Projektgröße. Mit NuGet-Powertools werden durch Angabe der Paketversionen in der Datei "Packages.config" Sie alle erforderlichen Bibliotheken das erstmalige herunterladen, wenn, das Sie das Projekt ausführen, können. Deshalb wird müssen Sie diese Schritte ausgeführt werden, nach dem Öffnen einer vorhandenen Lösung aus dieser Übungseinheit.
2. Suchen Sie im Projektmappen-Explorer **"default.aspx"** Seite der rechten Maustaste darauf und wählen Sie **in Seitenprüfung anzeigen**.

    ![Öffnen "default.aspx" mit der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image24.png "Öffnen von "default.aspx" mit der Seitenprüfung")

    *Öffnen Sie "default.aspx" mit der Seitenprüfung*
3. Die Seite Inspektor-Fenster wird "default.aspx" angezeigt.

    ![Anzeigen von "default.aspx" in der Seitenprüfung](using-page-inspector-in-visual-studio-2012/_static/image25.png ""default.aspx" in Seitenprüfung anzeigen")

    *"Default.aspx" in Seitenprüfung anzeigen*

    Das Tool Page Inspector ist in der Visual Studio-Umgebung integriert. Der Inspektor enthält einen eingebetteten Browser, zusammen mit einem leistungsstarke HTML-Profiler, der anzeigt, den ausgewählten Code. Beachten Sie, dass Sie keine führen Sie die Projektmappe, um Ihren Seiten anzuzeigen.

    > [!NOTE]
    > Wenn die Breite des Browsers Page Inspector kleiner als die Breite der Seite geöffnet wird, werden Sie die Seite nicht ordnungsgemäß angezeigt. Wenn dies der Fall ist, passen Sie die Breite der Seitenprüfung.
4. Klicken Sie auf die **Dateien** Registerkarte in der Seitenprüfung.

    Sie sehen die Quelldateien, die die gerenderte Standardseite erstellen. Dies ist nützlich zum Identifizieren alle Elemente auf einen Blick, insbesondere bei der Arbeit mit Benutzersteuerelemente und Masterseiten. Beachten Sie, dass Sie auch zu einzelnen Dateien navigieren können.

    ![Die Registerkarte "Dateien"](using-page-inspector-in-visual-studio-2012/_static/image26.png "Registerkarte "der Dateien"")

    *Die Registerkarte "Dateien"*
5. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche auf der linken Seite der Registerkarten.

    Mit diesem Tool können Sie wählen Sie ein beliebiges Element von der Seite ", und finden Sie in der HTML-Code und ASPX-Quelle.

    ![Überprüfungsmodus Umschaltfläche](using-page-inspector-in-visual-studio-2012/_static/image27.png "Überprüfungsmodus ein/aus-Schaltfläche")

    *Schaltfläche "Überprüfungsmodus umschalten"*
6. Bewegen Sie im Browser Seitenprüfung den Mauszeiger über die Seitenelemente. Während Sie den Mauszeiger über einen beliebigen Teil der gerenderten Seite bewegen, wird der Elementtyp wird angezeigt, und das entsprechende Quellmarkup oder der Code wird in der Visual Studio-Editor hervorgehoben.

    ![Überprüfungsmodus in Aktion](using-page-inspector-in-visual-studio-2012/_static/image28.png "Überprüfungsmodus in Aktion")

    *Überprüfungsmodus in Aktion*

    > [!NOTE]
    > Maximieren Sie das Page Inspector-Fenster nicht, oder Sie werden nicht in der Lage sind, finden in der Registerkarte "Vorschau" Quellcode anzeigen. Wenn Sie das Element in der Seitenprüfung auf, wenn dieses maximiert ist, der Quellcode der Auswahl erscheint, aber es wird das Fenster Page Inspector ausblenden.

    Wenn Sie Zertifikatsperrinformationen **"default.aspx"** -Datei werden Sie feststellen, dass der Teil des Quellcodes, die das ausgewählte Element erzeugt hervorgehoben ist. Dieses Feature erleichtert die Edition der lange Quelldateien, eine direkte und schnelle Möglichkeit zum Zugriff auf den Code bereitstellen.

    ![Überprüfen die Elemente](using-page-inspector-in-visual-studio-2012/_static/image29.png "Elemente überprüfen")

    *Überprüfen von Elementen*
7. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), befindet sich im Page Inspector-Registerkarten, um den Cursor zu deaktivieren.
8. Wählen Sie die **HTML** Registerkarte an, den im Browser Seitenprüfung gerenderten HTML-Code angezeigt.
9. Suchen Sie in der HTML-Code das Listenelement mit dem Koala-Link, und wählen Sie sie.

    Beachten Sie, dass wenn Sie den Code auswählen, die entsprechende Ausgabe automatisch hervorgehobenen Browser wird. Diese Funktion ist nützlich, um festzustellen, wie ein HTML-Block auf der Seite gerendert wird.

    ![Auswählen eines HTML-Elements auf der Seite](using-page-inspector-in-visual-studio-2012/_static/image31.png "ein HTML-Element auf der Seite auswählen")

    *Auswählen eines HTML-Elements auf der Seite*
10. Klicken Sie auf die **ein-/ausschalten Überprüfungsmodus** Schaltfläche aktivieren *Überprüfungsmodus* , und klicken Sie auf der Navigationsleiste. Am rechten Rand der HTML-Code, klicken Sie im Bereich "Formatvorlagen" sehen Sie eine Liste mit den CSS-Formatvorlagen, die auf das ausgewählte Element angewendet.

    > [!NOTE]
    > Da der Header das Layout für die Website gehört, Page Inspector ebenfalls öffnen Sie die Datei Site.Master und markieren Sie das Codesegment beeinflusst.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "Ermittlung von Formatvorlagen und Quelldateien eines ausgewählten Elements")

    *Ermittlung von Formatvorlagen und Quelldateien eines ausgewählten Elements*
11. Bewegen Sie mit dem zum ein-/ausschalten Inspektion Zeiger aktiviert den Mauszeiger unterhalb der Menüleiste, und klicken Sie auf die leere Halbkreis.

    ![Auswählen eines Elements](using-page-inspector-in-visual-studio-2012/_static/image33.png "Auswählen eines Elements")

    *Auswählen eines Elements*
12. Suchen Sie im Bereich Formate die **Hintergrundbild** Element unter den **.main-Content** Gruppe. **Deaktivieren Sie** der **Hintergrundbild** und sehen, was passiert. Beachten Sie, dass der Browser die Änderungen sofort widerspiegelt, und der Kreis wird ausgeblendet.

    > [!NOTE]
    > Die Änderungen, die Sie auf der Registerkarte "Seite Inspektor Formatvorlagen" vornehmen, wirken sich nicht auf die ursprünglichen Stylesheet aus. Stile deaktivieren können oder ändern Sie ihre Werte so oft wie werden sollen, aber sie werden nach der Aktualisierung der Seite wiederhergestellt werden.

    ![Aktivieren und Deaktivieren von CSS-styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "aktivieren und Deaktivieren von CSS-Stilen")

    *Aktivieren und Deaktivieren von CSS-Stilen*
13. Klicken Sie nun auf die "**Ihrer** **Logo hier einfügen"** Text auf den Header mit den Überprüfungsmodus.
14. In der **Stile** Registerkarte, suchen Sie nach der **Font-Size** CSS-Attribut unter der **.site Titel** Gruppe. Doppelklicken Sie auf das Attribut einmal, um seinen Wert zu bearbeiten. Ersetzen der 2.3em Wert mit **3em**, und drücken Sie dann die EINGABETASTE. Beachten Sie, dass der Titel größere aussieht.

    ![Ändern von CSS-Werten in der Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Ändern von CSS-Werten in der Seitenprüfung")

    *Ändern von CSS-Werten in der Seitenprüfung*
15. Klicken Sie auf die **Formatvorlagen** Registerkarte im rechten Bereich der Seitenprüfung befinden. Dies ist eine alternative Möglichkeit, alle Stile zur Auswahl, sortiert nach dem Attributnamen finden Sie unter.

    ![CSS-Formatvorlagen Verfolgen des ausgewählten Elements](using-page-inspector-in-visual-studio-2012/_static/image36.png "CSS-Formatvorlagen Verfolgen des ausgewählten Elements")

    *CSS-Formatvorlagen Verfolgen des ausgewählten Elements*
16. Eine weitere Funktion von Page Inspector ist der Layout-Bereich. Verwenden den Überprüfungsmodus, wählen Sie die Navigationsleiste, und klicken Sie dann auf die **Layout** Registerkarte im rechten Bereich. Sie sehen die genaue Größe des ausgewählten Elements sowie seine Größe Offset, Rand, Auffüllung und Rahmen. Beachten Sie, dass Sie auch die Werte aus dieser Sicht ändern können.

    ![In der Seitenprüfung Elementlayouts](using-page-inspector-in-visual-studio-2012/_static/image37.png "Elementlayouts in der Seitenprüfung")

    *In der Seitenprüfung Elementlayouts*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Aufgabe 2: Suchen und Beheben von Formatprobleme in der Fotogalerie

Wie würden Sie Webseiten mit früheren Versionen von Visual Studio Problemdiagnose? Sie sind wahrscheinlich bereits mit Web debugging-Tools, die außerhalb der Visual Studio-IDE, z. B. Internet Explorer-Entwicklertools oder Firebug ausgeführt vertraut sind. Browser nur verstehen, HTML, Skripts und Formatvorlagen, während eine zugrunde liegende Framework den HTML-Code generiert, die gerendert werden. Aus diesem Grund müssen Sie häufig die gesamte Website, um festzustellen, wie Webseiten aussehen bereitstellen.

Diese Schritte hatte wahrscheinlich ausgeführt werden, wenn Sie ermitteln und Beheben eines Problems auf Ihre Website wollten:

1. Führen Sie die Projektmappe in Visual Studio oder Bereitstellen Sie der Seite auf dem Webserver "".
2. Öffnen Sie im Browser die Developer-Tools, die Sie verwenden, oder öffnen Sie einfach den Quellcode und die Stile und versuchen, das Problem zu entsprechen. Um die Dateien zu finden, wo Sie verwendet hätten die &quot;Suche&quot; oder &quot;Suche in Dateien&quot; Funktionen mit dem Namen der Formatvorlage Klassen.
3. Nach Erkennung des Fehlers ist beenden Sie den Webbrowser und dem Server.
4. Löschen Sie den Browsercache.
5. Zurück zu Visual Studio und einen Fix anzuwenden. Wiederholen Sie die einzelnen Schritte zum Testen.

Es ist keine echte WYSIWYG in ASP.NET Web Forms, werden einige Formatprobleme in einem späteren Zeitpunkt erkannt, nachdem ausgeführt oder bereitgestellt. Mit der Seitenprüfung ist es nun möglich, eine beliebige Seite Vorschau anzeigen, ohne die Projektmappe auszuführen.

In dieser Aufgabe verwenden Sie die seitenprüfung für einige der Fotogalerie Anwendung Probleme beheben. In den folgenden Schritten erkennt und einige einfache Styling Problem wird in der Kopfzeile schnell beheben.

1. Suchen Sie mithilfe der Seite Prüfung der **registrieren** und **anmelden** Links auf der linken Seite des Headers.

    Beachten Sie, dass der Link nicht an der erwarteten Stelle auf der rechten Seite angezeigt wird. Sie jetzt den Link rechts ausgerichtet und entsprechend umzugestalten.

    ![Melden Sie sich links auf der linken Seite positioniert](using-page-inspector-in-visual-studio-2012/_static/image38.png "melden Sie sich links auf der linken Seite positioniert")

    *Link anmelden, das auf der linken Seite positioniert*
2. Wählen Sie mit ein-/ausschalten-Überprüfungsmodus ausgewählt haben den Link anmelden, um ihren Code zu öffnen.

    Beachten Sie, die der Quellcode Link sich in befindet der **Site.Master** -Datei nicht in die Seite "default.aspx" also die Stelle interessante Informationen finden Sie in der ersten Stelle; Sie direkt in der richtigen Quelldatei platziert wurden.
3. In der **Stile** Registerkarte, suchen und klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element, das die HTML--Container, damit diese Links wird.

    Beachten Sie, dass die **#login** Stil befindet sich im automatisch **"Site.CSS" ändern** nach anklicken. Darüber hinaus wird der Code jetzt hervorgehoben.

    ![Auswählen der CSS-Stilen](using-page-inspector-in-visual-studio-2012/_static/image39.png "CSS-Stilen auswählen")

    *Die CSS-Stilen auswählen*
4. Heben Sie die auskommentierung der **Text-align** Attribut in der hervorgehobene Code, durch das Entfernen der öffnenden und schließenden Zeichen, und speichern Sie die **"Site.CSS" ändern** Datei.

    Page Inspector fähig aller anderen Dateien, aus denen die aktuelle Seite zusammensetzt, und es kann erkennen, wenn eine dieser Dateien ändern. Sie werden benachrichtigt, wenn die aktuelle Seite im Browser nicht mit Quelldateien synchronisiert ist.
5. Klicken Sie im Browser Seitenprüfung auf der Leiste befindet sich unter der Adressleiste ein, um die Änderungen zu speichern und Laden Sie die Seite neu.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Erneutes Laden der Seite "*

    Die Links sind jetzt auf der rechten Seite, aber sie weiterhin wie folgt eine Liste mit Aufzählungszeichen aussehen. Jetzt, Sie entfernen die Aufzählungszeichen und horizontal ausgerichtet. die Links.

    ![Seite "aktualisiert"](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Seite "aktualisiert"*
6. Mit den Überprüfungsmodus, aktivieren Sie keines der  **&lt;li&gt;**  Elemente beziehen, enthalten die &quot;registrieren&quot; und &quot;melden Sie sich&quot; Links. Klicken Sie auf die  **&lt;Abschnitt&gt; #login** Element für den Zugriff auf **Styles.css** Code.

    ![Suchen Sie das Format von](using-page-inspector-in-visual-studio-2012/_static/image42.png "suchen das Format")

    *Suchen das Format*
7. In **Style.css**, kommentieren Sie den Code für **#login li** Elemente. Die Formatvorlage, die Sie hinzufügen wird Aufzählungszeichen ausblenden und Anzeigen der Elemente horizontal.

    ![Zuweisen neuer Stile die Anmeldung Links](using-page-inspector-in-visual-studio-2012/_static/image43.png "Zuweisen neuer Stile die Login-Links")

    *Zuweisen neuer Stile die Login-links*
8. Speichern Sie **Style.css** Datei, und klicken Sie auf der Leiste befindet sich unter der Adresse um die Seite erneut zu laden. Beachten Sie, dass die Links ordnungsgemäß angezeigt.

    ![Links, rechts ausgerichtet](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links, rechts ausgerichtet")

    *Links, rechts ausgerichtet*
9. Abschließend ändern Sie den Header-Titel. Anstelle der Suche nach der "**Ihr Logo hier einfügen"** Text in allen Dateien angezeigt, den Überprüfungsmodus verwenden, um auf den Text und das Abrufen auf den Quellcode, das sie generiert.

    ![Suchen den Titel der Website](using-page-inspector-in-visual-studio-2012/_static/image45.png "suchen den Titel der Website")

    *Suchen den Titel der Website*
10. Jetzt sind Sie in **Site.Master**, ersetzen Sie die "**Ihr Logo hier einfügen**'Text mit'**Fotogalerie**". Speichern und Browser der Seitenprüfung zu aktualisieren.

    ![Fotogalerie Seite aktualisiert](using-page-inspector-in-visual-studio-2012/_static/image46.png "Fotogalerie Seite aktualisiert")

    *Fotogalerie Seite aktualisiert*
11. Drücken Sie zum Schluss **F5** zum Ausführen der app des Auschecken alle Änderungen ordnungsgemäß funktionieren wie erwartet.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Zusammenfassung

Durch diese praktische Übungseinheit haben gelernt, wie die Web-Anwendung in der Vorschau anzeigen, ohne neu und führen die Website in einem Browser mit Page Inspector. Darüber hinaus haben Sie gelernt, wie Sie Schnelles Auffinden und Beheben von Fehlern durch den Zugriff auf direkt aus der gerenderten Ausgabe auf den Quellcode.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Anhang A: Installieren von Visual Studio Express 2012 für das Web

Sie installieren können **Microsoft Visual Studio Express 2012 für das Web** oder ein anderes &quot;Express&quot; Version mithilfe der  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . Die folgenden Anweisungen führen Sie durch die erforderlichen Schritte zum Installieren *Visual Studio Express 2012 für das Web* mit *Microsoft Web Platform Installer*.

1. Wechseln Sie zu [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Auch wenn Sie bereits Webplattform-Installer installiert haben, können Sie öffnen es, und suchen Sie nach dem Produkt &quot; *Visual Studio Express 2012 für das Web mit Windows Azure SDK*&quot;.
2. Klicken Sie auf **jetzt installieren**. Wenn Sie keine **Webplattform-Installer** Sie Informationen zum Herunterladen und installieren Sie diese zuerst umgeleitet werden.
3. Einmal **Webplattform-Installer** geöffnet ist, klicken Sie auf **installieren** um das Setup zu starten.

    ![Visual Studio Express installieren](using-page-inspector-in-visual-studio-2012/_static/image47.png "Visual Studio Express installieren")

    *Installieren Sie Visual Studio Express*
4. Lesen Sie die Produkte, Lizenzen und Begriffe, und klicken Sie auf **ich stimme** um den Vorgang fortzusetzen.

    ![Akzeptieren der Lizenzbedingungen](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Akzeptieren der Lizenzbedingungen*
5. Warten Sie, bis der Prozess herunterladen und die Installation abgeschlossen ist.

    ![Installationsstatus](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Installationsstatus*
6. Nach Abschluss der Installation klicken Sie auf **Fertig stellen**.

    ![Installation wurde abgeschlossen](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Installation wurde abgeschlossen*
7. Klicken Sie auf **beenden** Webplattform-Installer zu schließen.
8. Um Visual Studio Express für Web zu öffnen, wechseln Sie zu der **starten** Startseite ein, und starten Sie das Schreiben von &quot; **Visual Studio Express**&quot;, klicken Sie dann auf die **Visual Studio Express für Web** Kachel.

    ![Visual Studio Express für Web-Kachel](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *Visual Studio Express für Web-Kachel*
