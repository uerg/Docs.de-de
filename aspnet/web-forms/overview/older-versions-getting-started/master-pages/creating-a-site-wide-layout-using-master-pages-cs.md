---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: Erstellen eines standortweite Layouts mit Masterseiten (c#) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm werden die Grundlagen der Masterseite angezeigt. Nämlich, was Masterseiten, sind wie zu, ist eine Erstellen einer Masterseite, worauf Content Platzhaltern, wie ein Cr ist..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e3507acda4958fa7caa4a22fec7603deaec73c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-site-wide-layout-using-master-pages-c"></a>Erstellen eines standortweite Layouts mit Masterseiten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> In diesem Lernprogramm werden die Grundlagen der Masterseite angezeigt. Nämlich, was sind Masterseiten, wie erstellt einer Masterseite was Content Platzhalter sind, wie eine erstellt eine ASP.NET-Seite auf, die mithilfe eine Masterseite, wie die Änderung von die Masterseite automatisch in seine zugeordneten Inhaltsseiten usw. wiedergegeben werden.


## <a name="introduction"></a>Einführung

Ein Attribut einer gut entworfenen Website wird eine konsistente standortweite Seitenlayout. Nehmen Sie z. B. der www.asp.net-Website. Zum Zeitpunkt der Verfassung dieses Artikels hat jeder Seite der gleichen Inhalt am oberen und unteren Rand der Seite. Wie in Abbildung 1 gezeigt, wird ganz oben Rand jeder Seite einen grauen Balken mit einer Liste von Microsoft Communities angezeigt. Unter, d. h. das Websitelogo, die Liste der Sprachen, in dem die Website übersetzt wurden, und in den Abschnitten Core: Home, erste Schritte, informieren Sie sich, Downloads usw. Entsprechend enthält den unteren Rand der Seite Informationen zu Werbung auf www.asp.net, eine urheberrechtserklärung und einen Link zu den Datenschutzbestimmungen.


[![Die Website www.asp.net verwendet ein einheitliches Erscheinungsbild für alle Seiten](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

**Abbildung 01**: die www.asp.net Website verwendet wird, ein konsistentes Aussehen und über alle Seiten den Eindruck ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))


Ein anderes Attribut einer gut entworfenen Website ist die Einfachheit, mit der Darstellung des Standorts geändert werden kann. Abbildung 1 zeigt die Startseite der www.asp.net ab März 2008, aber zwischen sofort aus, und Veröffentlichung in diesem Lernprogramm, das Aussehen und Verhalten möglicherweise geändert. Möglicherweise werden die Menüelemente entlang der Oberkante erweitert, um einen neuen Abschnitt für das MVC-Framework umfassen. Oder vielleicht ein brandneues Design mit verschiedenen Farben, Schriftarten und Layout unveiled werden. Solche Änderungen anwenden, um den gesamten Standort sollte ein schneller und einfacher Prozess sein, der Ändern von Tausenden von Webseiten, aus denen die Website nicht erforderlich ist.

Erstellen eine Vorlage mit der standortweiten in ASP.NET kann mithilfe des *Masterseiten*. Kurz gesagt, eine Masterseite ist eine besondere Art von ASP.NET-Seite, die das Markup definiert, die gemeinsam von allen *Inhaltsseiten* sowie der Regionen, die auf Basis von Seiteninhalt Inhaltsseite angepasst werden. (Eine Inhaltsseite ist eine ASP.NET-Seite auf, die an die Masterseite gebunden ist.) Wenn Layout oder Formatieren von einer Masterseite geändert wird, alle seine Inhaltsseiten Ausgabe wird ebenso sofort aktualisiert, wodurch standortweite Darstellung Änderungen so einfach wie das Aktualisieren und Bereitstellen von einer einzelnen Datei (d. h., der Gestaltungsvorlage) angewendet.

Dies ist das erste Lernprogramm in einer Reihe von Lernprogrammen, die mit Masterseiten untersuchen. Im Verlauf dieser Reihe von Lernprogrammen wir:

- Überprüfen Sie, Gestaltungsvorlagen und ihre zugeordneten Inhaltsseiten erstellen,
- Eine Vielzahl von Tipps, Tricks und Traps zu besprechen,
- Identifizieren Sie der häufigsten Fehlerquellen der Gestaltungsvorlage und untersuchen Sie problemumgehungen zu,
- Finden Sie unter der Masterseite von einer Inhaltsseite und umgekehrt ein Zugriff auf
- Informationen zum Angeben einer Inhaltsseite Masterseite zur Laufzeit und
- Andere erweiterte Themen Masterseite.

Diese Lernprogramme sind darauf ausgerichtet, um präzise sein, und geben Sie schrittweise Anweisungen mit Screenshots, Sie durch den Prozess visuell zu durchlaufen. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und enthält einen Download der den vollständigen Code verwendet.

In diesem ersten Lernprogramm beginnt mit einem Blick auf die Grundlagen der Masterseite. Wir erörtern, wie von Masterseiten, sehen Sie sich das Erstellen einer Masterseite und zugehörigen Content-Seiten, die mit Visual Web Developer und finden Sie unter wie Änderungen an einer Masterseite sofort in seiner Inhaltsseiten widergespiegelt werden. Fangen wir an!

## <a name="understanding-how-master-pages-work"></a>Grundlegendes zur Funktionsweise von Masterseiten

Erstellen einer Website mit einem konsistenten standortweite Seitenlayout erfordert, dass jede Webseite allgemeine Formatierung Markup zusätzlich zu den benutzerdefinierten Inhalt ausgeben. Z. B. während jedes Lernprogramm oder Forum Post für www.asp.net haben ihre eigenen eindeutigen Inhalte, jede dieser Seiten auch Rendern einer Reihe von allgemeinen `<div>` -Elemente, die auf der obersten Ebene Abschnitt Links anzeigen: Home, erste Schritte, erfahren Sie mehr und So weiter.

Es gibt eine Vielzahl von Techniken für die Erstellung von Webseiten mit ein einheitliches Erscheinungsbild. Ein naive-Ansatz ist, kopieren Sie einfach, und fügen das allgemeine Layout Markup in alle Webseiten, aber dieser Ansatz bietet eine Reihe von Nachteile. Zunächst jedes Mal, wenn eine neue Seite erstellt wird, müssen Sie daran denken, kopieren und fügen den freigegebenen Inhalt in die Seite. Solche kopieren und Einfügen sind reif für Fehler, wie Sie nur eine Teilmenge der freigegebenen Markup in einer neuen Seite versehentlich kopieren können. Und um es top deaktivieren, dieser Ansatz stellt ersetzen die vorhandene standortweite Darstellung mit einer neuen ein echten umfangreichen, da jede einzelne Seite in der Website bearbeitet werden muss, um das neue Aussehen und Verhalten zu verwenden.

Vor dem ASP.NET, Version 2.0, Seite Entwickler häufig platziert allgemeine Markup in [Benutzersteuerelemente](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx) und dann diese Benutzersteuerelemente auf jeder Seite hinzugefügt. Dieser Ansatz erforderlich, dass der Entwickler der Seite Denken Sie daran, jede neue Seite manuell die Benutzersteuerelemente hinzufügen, jedoch für einfacher standortweite Änderungen zulässig, da bei der Aktualisierung der allgemeinen Markup benötigt nur die Benutzersteuerelemente geändert werden. Leider verwendet Visual Studio .NET 2002 und 2003 – die Versionen von Visual Studio zum Erstellen von ASP.NET 1.x Anwendungen - Benutzersteuerelemente in der Entwurfsansicht als graue Felder dargestellt. Seitenentwickler, die bei diesem Ansatz daher keine WYSIWYG-Entwurfszeit Umgebung nutzen.

Die Nachteile der Verwendung von Benutzersteuerelementen wurden in ASP.NET Version 2.0 und Visual Studio 2005 behandelt, durch die Einführung des *Masterseiten*. Eine Masterseite ist eine besondere Art von ASP.NET-Seite, das das standortweite-Markup definiert und die *Regionen* bei verknüpften *Inhaltsseiten* definieren Sie ihre benutzerdefinierte Markup. Da wir in Schritt 1 angezeigt wird, werden diese Bereiche von ContentPlaceHolder-Steuerelementen definiert. Das Steuerelement ContentPlaceHolder bezeichnet einfach eine Position in der Masterseite Hierarchie, in dem benutzerdefinierter Inhalt von einer Inhaltsseite eingefügt werden kann.

> [!NOTE]
> Die grundlegenden Konzepte und Funktionen von Masterseiten wurde nicht geändert, seit ASP.NET Version 2.0. Visual Studio 2008 bietet jedoch zur Entwurfszeit Unterstützung für verschachtelte Gestaltungsvorlagen, ein Feature, das in Visual Studio 2005 gefehlt. Betrachten wir verschachtelte Gestaltungsvorlagen in einem späteren Lernprogramm verwenden.


Abbildung 2 zeigt, wie die Gestaltungsvorlage für www.asp.net aussehen könnte. Beachten Sie, dass die Gestaltungsvorlage der allgemeinen standortweite Layout - Markup am oberen, unteren und rechten Rand jeder Seite - sowie eine ContentPlaceHolder in der Mitte links definiert, wo sich die eindeutige Inhalt für jede einzelne Webseite befindet.


![Eine Gestaltungsvorlage definiert das Layout standortweite sowie der Regionen, die bearbeitet werden regelmäßig Seite Inhalt von Seiteninhalt](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**Abbildung 02**: Masterseite definiert das Layout standortweite sowie der Regionen, die bearbeitet werden pro Seite Inhalt von Seiteninhalt


Nach eine Masterseite definiert wurde, kann es an neue ASP.NET-Seiten, bis der Takt des ein Kontrollkästchen gebunden werden. Diese - Inhaltsseiten aufgerufen - ASP.NET-Seiten enthalten ein Inhaltssteuerelement für jede der Masterseite ContentPlaceHolder-Steuerelemente. Wenn Inhaltsseite über einen Browser zugegriffen wird das ASP.NET-Modul Steuerelementhierarchie der Masterseite und fügt der Inhaltsseite Steuerelementhierarchie in den entsprechenden Stellen. Diese Steuerelementhierarchie kombinierten gerendert wird, und der resultierende HTML-Code an die Endbenutzer-Browser zurückgegeben. Folglich gibt Inhaltsseite das allgemeine Markup, das auf der Masterseite außerhalb der Steuerelemente ContentPlaceHolder definiert und in einem eigenen Inhaltssteuerelemente definiert seitenspezifische Markup. Abbildung 3 veranschaulicht dieses Konzept.


[![Die angeforderte Seite Markup wird in der Master-Seite Sicherung.](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**Abbildung 03**: die angeforderte Seite Markup wird in der Master-Seite Fused ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))


Nun, da wir haben erläutert, wie von Masterseiten, werfen wir einen Blick auf das Erstellen einer Masterseite und zugehörigen Content-Seiten, die mit Visual Web Developer.

> [!NOTE]
> Um die größtmögliche Zielgruppe erreichen, wir, in der gesamten dieser Reihe von Lernprogrammen erstellen, ASP.NET-Website erstellt werden Microsofts kostenlose Version von Visual Studio 2008 mit ASP.NET 3.5 [Visual Web Developer 2008.](https://www.microsoft.com/express/vwd/). Wenn Sie noch nicht auf ASP.NET 3.5 aktualisiert keine Sorge - gut in diese Lernprogramme arbeiten erörterten Konzepte genauso mit ASP.NET 2.0 und Visual Studio 2005. Allerdings können einige Demo-Anwendungen neue Funktionen in .NET Framework, Version 3.5 verwenden; Bei der 3.5-spezifischen Funktionen verwendet werden, umfassen ich eine Anmerkung, die erläutert, wie ähnliche Funktionalität wie in Version 2.0 implementiert. Führen Sie bedenken, die die Demo für Anwendungen verfügbar von jedes Lernprogramm Ziel .NET Framework, Version 3.5, herunterladen Vortäuschen einer `Web.config` -Datei, umfasst der 3.5-spezifischen Konfigurationselementen und Verweise auf 3.5-spezifischen Namespaces in, der `using` Anweisungen in Code-Behind-Klassen ASP.NET-Seiten. Kurz gesagt, wenn Sie noch installieren Sie .NET 3.5 auf dem Computer dann zum Herunterladen der Anwendung funktioniert nicht ohne die zuvor entfernt der 3.5-spezifischer Markups aus `Web.config`. Finden Sie unter [unterzogen, wodurch ASP.NET Version 3.5 des `Web.config` Datei](http://www.4guysfromrolla.com/articles/121207-1.aspx) für Weitere Informationen zu diesem Thema. Sie müssen auch zum Entfernen der `using` Anweisungen, die 3.5-spezifischen Namespaces verweisen.


## <a name="step-1-creating-a-master-page"></a>Schritt 1: Erstellen einer Masterseite

Bevor wir das Erstellen und Verwenden von Master- und Inhaltsseiten untersuchen können, benötigen wir zunächst eine ASP.NET-Website. Durch das Erstellen einer neuen systembasierten ASP.NET-Website starten. Um dies zu erreichen, starten Sie Visual Web Developer wechseln Sie zum Menü Datei und klicken Sie neue Website, die das Dialogfeld "neue Website" angezeigt (siehe Abbildung 4). Auswählen der ASP.NET Web-Websitevorlage, legen Sie die Dropdown-Liste im Dateisystem, wählen Sie einen Ordner auf der Website platzieren und Festlegen der Sprache c#. Dies erstellt eine neue Website mit einem `Default.aspx` ASP.NET-Seite ein `App_Data` Ordner, und ein `Web.config` Datei.

> [!NOTE]
> Visual Studio unterstützt zwei Bereitstellungsmodi Projektmanagement: Websiteprojekte und Webanwendungsprojekte. Websiteprojekte fehlender eine Projektdatei aus, wohingegen Webanwendungsprojekte der Projektarchitektur in Visual Studio .NET 2002/2003 imitieren – sie enthalten eine Projektdatei, und Kompilieren von Quellcode des Projekts in einer einzelnen Assembly in der befindet der `/bin` Ordner. Visual Studio 2005 anfangs nur unterstützte Website Projekte, obwohl die [Webanwendungsprojekt Modell](https://msdn.microsoft.com/en-us/library/aa730880(vs.80).aspx) wurde wieder eingeführt, mit Service Pack 1; Visual Studio 2008 bietet beider Projektmodelle. Visual Web Developer 2005 und 2008-Editionen unterstützen allerdings nur Websiteprojekte. Ich verwende das Websiteprojekt-Modell für meine Demos in diesem Lernprogramm Reihe. Wenn Sie eine nicht-Express Edition verwenden und stattdessen das Webanwendungsprojekt-Modell verwenden möchten, können Sie dies tun, aber beachten Sie, dass es möglicherweise einige Diskrepanzen zwischen sehen Sie, auf dem Bildschirm und die Schritte, die Sie, im Vergleich zu den Screenshots dargestellt und Instructio ausführen müssen NS, die in diesen Lernprogrammen bereitgestellt werden.


[![Erstellen Sie eine neue Website von Datei-basierten System](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**Abbildung 04**: Erstellen einer Website New File System-Based ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))


Fügen Sie einer Masterseite an den Standort im Stammverzeichnis von mit der rechten Maustaste auf den Projektnamen, neues Element hinzufügen auswählen und die Gestaltungsvorlage-Vorlage auswählen. Beachten Sie, dass mit der Erweiterung Masterseiten enden `.master`. Nennen Sie diese neue Masterseite `Site.master` , und klicken Sie auf Hinzufügen.


[![Fügen Sie eine Master-Seite mit dem Namen Site.master auf der Website](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**Abbildung 05**: Fügen Sie eine Master Seite benannte `Site.master` auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))


Hinzufügen einer neuen Masterseitendatei über Visual Web Developer erstellt eine Masterseite mit den folgenden deklarativem Markup:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

Die erste Zeile im deklarativen Markup ist die [ `@Master` Richtlinie](https://msdn.microsoft.com/en-us/library/ms228176.aspx). Die `@Master` Richtlinie ähnelt der [ `@Page` Richtlinie](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx) , in ASP.NET-Seiten angezeigt wird. Definiert die serverseitige-Sprache (c#) und Informationen über den Speicherort und die Vererbung von der Masterseite Code-Behind-Klasse.

Die `DOCTYPE` und deklaratives Markup der Seite wird angezeigt, darunter die `@Master` Richtlinie. Die Seite enthält statische HTML zusammen mit vier serverseitige Steuerelemente:

- **Ein Web Form (die `<form runat="server">`)** : Da alle ASP.NET-Seiten in der Regel haben ein Web Form - und da die Gestaltungsvorlage Websteuerelemente enthalten kann, die in einem Web Form - angezeigt werden, muss unbedingt das Web Form der Gestaltungsvorlage (hinzufügen, sondern ein Webformular mit e hinzufügen ACH Inhaltsseite).
- **Ein ContentPlaceHolder-Steuerelement namens `ContentPlaceHolder1`**  -dieses ContentPlaceHolder-Steuerelement wird in das Web Form und dient als die Region für die Benutzeroberfläche der Inhaltsseite.
- **Ein serverseitiger `<head>` Element** – der `<head>` Element verfügt über die `runat="server"` -Attribut über serverseitigen Code verfügbar zu machen. Die `<head>` Element wird auf diese Weise implementiert, damit den Titel der Seite und andere `<head>`-bezogene Markup hinzugefügt oder programmgesteuert angepasst werden kann. Z. B. Festlegen einer ASP.NET-Seite `Title` eigenschaftenänderungen der `<title>` Element gerendert wird, indem Sie die `<head>` Serversteuerelement.
- **Ein ContentPlaceHolder-Steuerelement namens `head`**  -dieses ContentPlaceHolder-Steuerelement angezeigt wird, innerhalb der `<head>` Server gesteuert und können verwendet werden, um deklarativ Inhalt hinzuzufügen der `<head>` Element.

Diese Standardeinstellung Masterseite deklarativen Markup dient als Ausgangspunkt für eigene Masterseiten entwerfen. Gerne mit den HTML-Code bearbeiten oder Hinzufügen von zusätzliche Websteuerelemente oder ContentPlaceHolders der Masterseite.

> [!NOTE]
> Beim Entwerfen einer Masterseite stellen sicher, dass die Gestaltungsvorlage enthält ein Web Form, und dieser über mindestens ein ContentPlaceHolder-Steuerelement angezeigt wird, in diesem Web Form.


### <a name="creating-a-simple-site-layout"></a>Erstellen eines einfachen Website Layouts

Schauen wir uns `Site.master`des deklarative Standardmarkup ein Website-Layout erstellt, in dem alle Seiten gemeinsam nutzen: eine allgemeine Header; eine linken Spalte mit Navigation, News und andere standortweite Inhalte; und eine Fußzeile, in dem das Symbol "Unterstützung von Microsoft ASP.NET" angezeigt. Abbildung 6 zeigt das Endergebnis der Masterseite auf, wenn eine der entsprechenden Inhaltsseiten über einen Browser angezeigt wird. Die rote Kreis Region in Abbildung 6 bezieht sich auf der Seite aufgerufen wird (`Default.aspx`); der übrige Inhalt ist in der Masterseite definiert und daher konsistente über alle Inhaltsseiten.


[![Die Gestaltungsvorlage definiert das Markup für die oben, links und unteren Teile](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**Abbildung 06**: die Gestaltungsvorlage definiert das Markup für die oben, links und unteren Teile ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))


Um das Website-Layout, das in Abbildung 6 dargestellten zu erreichen, starten Sie mit dem Aktualisieren der `Site.master` Masterseite, sodass sie die folgenden deklarative Markup enthält:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

Die Gestaltungsvorlage Layout ist definiert eine Reihe von `<div>` HTML-Elementen. Die `topContent` `<div>` das Markup enthält, die am oberen Rand jeder Seite wird angezeigt, während die `mainContent`, `leftContent`, und `footerContent` `<div>` s werden verwendet, um den Inhalt der Seite, der linken Spalte und die "Powered by Microsoft anzeigen ASP.NET"-Symbol, bzw. Zusätzlich zum Hinzufügen von diese `<div>` Elemente, die ich auch umbenannt der `ID` Eigenschaft des Steuerelements aus primären ContentPlaceHolder `ContentPlaceHolder1` auf `MainContent`.

Die Formatierung und Layout-Regeln für die ausgewählten `<div>` Elemente wird ausgeschrieben, der [Cascading Stylesheet (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) Datei `Styles.css`, der angegeben wird, über eine &lt;Link&gt; Element in der der Masterseite &lt;Head&gt; Element. Diese verschiedenen Regeln definieren das Aussehen und Verhalten der einzelnen `<div>` Element wie oben beschrieben. Z. B. die `topContent` `<div>` -Element, das den Text "Master Pages-Lernprogramme" und den Link angezeigt wird, hat die Formatierungsregeln, mit denen im angegebenen `Styles.css` wie folgt:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

Wenn Sie auf Ihrem Computer zusammen vorgehen, müssen Sie zugehörige Code für dieses Lernprogramm herunterzuladen und Hinzufügen der `Styles.css` Datei zum Projekt. Auf ähnliche Weise auch müssen Sie einen Ordner namens Images erstellen und das Symbol "Von Microsoft ASP.NET unterstützt" aus der heruntergeladenen Demo-Website in Ihr Projekt kopieren.

> [!NOTE]
> Eine Erörterung von CSS und die Formatierung der Webseite ist, würde den Rahmen dieses Artikels sprengen. Weitere Informationen zu CSS, sehen Sie sich die [CSS-Lernprogramme](http://www.w3schools.com/css/default.asp) am [W3Schools.com](http://www.w3schools.com/). Auch sollten Sie zum zugehörigen Code für dieses Lernprogramm herunterladen und mit den CSS-Einstellungen im wiedergeben `Styles.css` um die Auswirkungen der verschiedenen Formatierungsregeln anzuzeigen.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Erstellen einer Masterseite mithilfe einer vorhandenen Entwurfsvorlage für den

Im Laufe der Jahre habe ich eine Anzahl von ASP.NET-Webanwendungen für kleine-mittelständische Unternehmen erstellt. Einige der Clientcomputer musste eines Layouts für vorhandene Website, das sie verwenden möchten. andere eingestellt zuständigen Grafikdesigner. Ein paar anvertraut anzeigen, um das Layout der Website zu entwerfen. Wie Sie Abbildung 6 erkennen können, empfiehlt sich des Multitaskings auf dem Programmierer zum Entwerfen einer Website Layout in der Regel als mit Ihrem Rechner open-heart surgery ausführen, während Ihre Doctor Ihre steuern verfügt.

Glücklicherweise stehen innumerous Websites mit freien HTML Entwurfsvorlagen - Google mehr als sechs Millionen Ergebnisse zurückgegeben, für den Suchbegriff "freien Websitevorlagen." Eine der besten aufgelistet ist [OpenDesigns.org](http://opendesigns.org/). Sobald Sie eine Vorlage für die Website, die Ihnen gefällt gefunden, fügen Sie die CSS-Dateien und Images dem Websiteprojekt hinzu und integrieren Sie die Vorlage HTML in der Masterseite.

> [!NOTE]
> Microsoft bietet auch eine Reihe von [Freigeben von Vorlagen für ASP.NET Entwurf Start Kit](https://msdn.microsoft.com/en-us/asp.net/aa336613.aspx) , die in das Dialogfeld "neue Website" in Visual Studio integrieren.


## <a name="step-2-creating-associated-content-pages"></a>Schritt 2: Erstellen verbundenen Inhaltsseiten

Mit der master-Seite erstellt können wir beim Starten der Erstellung von ASP.NET-Seiten, die der Masterseite gebunden sind. Solche Seiten werden als bezeichnet *Inhaltsseiten*.

Nehmen wir das Projekt eine neue ASP.NET-Seite hinzu, und binden Sie es an die `Site.master` Masterseite. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie die Option neues Element hinzufügen. Wählen Sie das Web Form-Vorlage aus, geben Sie den Namen `About.aspx`, und klicken Sie dann das Kontrollkästchen "Masterseite auswählen" wie in Abbildung 7 dargestellt. Auf diese Weise zeigt die SELECT-Anweisung eine Gestaltungsvorlage Dialogfeld im Feld aus, in dem Sie die Gestaltungsvorlage verwendet auswählen können (siehe Abbildung 8).

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website unter Verwendung des Webanwendungsprojekt-Modells anstelle des Modells für die Website-Projekt erstellt haben, sehen Sie nicht das Kontrollkästchen "Masterseite auswählen" im Dialogfeld "Neues Element hinzufügen", siehe Abbildung 7. Um ein Inhalt erstellen muss Seite, wenn das Webanwendungsprojekt mithilfe Modellieren Sie die Vorlage Inhalte Webformular anstelle der Web Form-Vorlage auswählen. Nach Auswahl der Web Content Form-Vorlage, und klicken Sie auf Hinzufügen, wählen Sie das gleiche einer Masterseite in Abbildung 8 gezeigte Dialogfeld angezeigt wird.


[![Fügen Sie eine neue Seite hinzu.](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**Abbildung 07**: Fügen Sie eine neue Seite hinzu ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))


[![Wählen Sie die Site.master Gestaltungsvorlage](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**Abbildung 08**: Wählen Sie die `Site.master` Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))


Wie in der folgenden deklarative Markup gezeigt, eine neue Seite enthält eine `@Page` Direktive, die Punkte mit seinem Master Seite und auf ein Inhaltssteuerelement für jedes der Masterseite ContentPlaceHolder Steuerelemente erstellt werden.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> Im Abschnitt "Erstellen einer einfachen Website Layout" in Schritt 1 nach dem Umbenennen `ContentPlaceHolder1` auf `MainContent`. Wenn Sie dieses ContentPlaceHolder-Steuerelement nicht umbenennen, `ID` auf die gleiche Weise Ihre Inhaltsseite deklarativem Markup wird unterscheiden sich geringfügig von der oben gezeigten Markuperweiterung. Nämlich des zweite Inhaltssteuerelements des `ContentPlaceHolderID` wider der `ID` des entsprechenden ContentPlaceHolder steuern, auf der Masterseite.


Bei eine Inhaltsseite zu rendern, muss das Modul für ASP.NET den Anzeigezustand der Seite Sicherung Inhaltssteuerelemente mit seiner Masterseite ContentPlaceHolder-Steuerelemente. Das Modul für ASP.NET bestimmt Masterseite der Inhaltsseite, von der `@Page` Richtlinie `MasterPageFile` Attribut. Wie die oben genannten Markup zeigt, an diese Inhaltsseite gebunden `~/Site.master`.

Da die Masterseite zwei ContentPlaceHolder Steuerelemente – verfügt über `head` und `MainContent` -Visual Web Developer generiert zwei Content-Steuerelemente. Jedes Inhaltssteuerelement verweist auf einen bestimmten ContentPlaceHolder über seine `ContentPlaceHolderID` Eigenschaft.

Masterseiten, in denen über vorherige standortweite Vorlage Techniken verwendet werden, mit ihrer Unterstützung zur Entwurfszeit. Abbildung 9 zeigt die `About.aspx` Inhaltsseite, wenn durch Visual Web Developer-Entwurfsansicht angezeigt. Beachten Sie, dass während der Inhalt der Masterseite angezeigt wird, abgeblendet ist und nicht geändert werden. Die Inhaltssteuerelemente, die für die Gestaltungsvorlage ContentPlaceHolders gibt jedoch bearbeitet werden. Und wie Sie mit anderen ASP.NET-Seite durch Hinzufügen von Web-Steuerelementen in der Quelltext- oder Entwurfssicht Ansichten der Inhaltsseite-Schnittstelle erstellen können.


[![Der Inhaltsseite die Entwurfsansicht zeigt den seitenspezifische und Master Seiteninhalt](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**Abbildung 09**: der Inhaltsseite Entwurf Ansicht zeigt sowohl den seitenspezifische und Inhalt der Master-Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Hinzufügen von Markup und Websteuerelemente zu Inhaltsseite

Erkundet Manche Inhalte für die Erstellung der `About.aspx` Seite. Sie können finden Sie in Abbildung 10: ich eine Überschrift "Über die Author" und eine Reihe von Absätzen Text eingegeben, jedoch gerne auch Web-Steuerelemente hinzufügen. Nach dem Erstellen dieser Schnittstelle, besuchen Sie die `About.aspx` Seite über einen Browser.


[![Besuchen Sie die Seite About.aspx über einen Browser](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**Abbildung 10**: finden Sie auf der `About.aspx` Seite über Browser ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))


Es ist wichtig zu verstehen, dass die angeforderte Seite Inhalt und seine zugeordneten Masterseite fused und als Ganzes vollständig auf dem Webserver gerendert. Der Endbenutzer-Browser wird den resultierenden mit HTML-Code gesendet. Um dies zu überprüfen, zeigen Sie den HTML-Code, der vom Browser empfangen werden, indem Sie im Menü "Ansicht" und Quelle auswählen. Beachten Sie, dass keine Rahmen oder eine beliebige andere speziellen Techniken zum Anzeigen von zwei verschiedenen Webseiten in einem einzelnen Fenster vorhanden sind.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Binden von einer Masterseite an eine ASP.NET-Seite

Wie wir in diesem Schritt gesehen haben, ist die ASP.NET-Webanwendung eine neue Seite hinzugefügt genauso einfach wie das Aktivieren des Kontrollkästchens "Masterseite auswählen", und entnehmen der Masterseite. Es ist leider nicht so einfach, konvertieren eine ASP.NET-Seite in einer Masterseite.

Um einer Masterseite an eine ASP.NET-Seite zu binden, müssen Sie die folgenden Schritte ausführen:

1. Hinzufügen der `MasterPageFile` -Attribut auf der ASP.NET-Seite `@Page` Richtlinie, die auf die entsprechenden Gestaltungsvorlage.
2. Hinzufügen von Inhaltssteuerelementen für jede der ContentPlaceHolders in die Masterseite.
3. Selektiv schneiden Sie, und fügen Sie der ASP.NET-Seite vorhandenen Inhalte in den entsprechenden Content-Steuerelemente. Ich vorgebe "selektiv" hier da ASP.NET Seite wahrscheinlich enthält Markup, das bereits von der Masterseite zurückgegriffen wird, wie z. B. ist der `DOCTYPE`, `<html>` -Element, und das Web Form.

Eine schrittweise Anleitung für diesen Prozess zusammen mit Screenshots finden Sie [Scott Guthrie](https://weblogs.asp.net/scottgu/)des [Masterseiten verwenden und Websitenavigation](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx) Lernprogramm. Die "Update `Default.aspx` und `DataSample.aspx` der Gestaltungsvorlage verwendet" Abschnitt sind die folgenden Schritte aus.

Da es viel einfacher ist, neue Inhaltsseiten erstellen, statt vorhandene ASP.NET-Seiten in Inhaltsseiten zu konvertieren, empfehlen wir, dass bei jedem Erstellen einer neuen ASP.NET-Website eine Masterseite an den Standort hinzufügen. Binden Sie alle neue ASP.NET-Seiten an diese Masterseite. Machen Sie sich keine Gedanken Sie, wenn die anfängliche Gestaltungsvorlage sehr einfach oder plain ist; Sie können die Gestaltungsvorlage später aktualisieren.

> [!NOTE]
> Wenn Sie eine neue ASP.NET-Anwendung zu erstellen, fügt Visual Web Developer eine `Default.aspx` Seite, die nicht an einer Masterseite gebunden ist. Wenn Sie zum Konvertieren einer vorhandenen ASP.NET-Seite in einer Inhaltsseite üben möchten, fahren Sie fort, und holen Sie dies mit `Default.aspx`. Alternativ können Sie löschen `Default.aspx` und anschließend erneut hinzufügen, aber dieses Mal überprüfen das Kontrollkästchen "Masterseite auswählen".


## <a name="step-3-updating-the-master-pages-markup"></a>Schritt 3: Aktualisieren der Gestaltungsvorlage Markup

Einer der Hauptvorteile von Masterseiten ist, dass eine einzelne Masterseite verwendet werden kann, um das allgemeine Layout für zahlreiche Seiten auf der Website zu definieren. Aktualisieren des Standorts Aussehen und Verhalten erfordert daher eine einzelne Datei - Masterseite aktualisieren.

Um dieses Verhalten zu veranschaulichen, aktualisieren wir unsere Masterseite Einbeziehung des aktuellen Datums im am oberen Rand der linken Spalte ein. Fügen Sie eine Bezeichnung mit dem Namen `DateDisplay` auf die `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

Als Nächstes erstellen Sie eine `Page_Load` -Ereignishandler für die Master Seite, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

Der obige Code legt der Bezeichnung `Text` Eigenschaft, um das aktuelle Datum und die Uhrzeit formatiert den Tag der Woche, die den Namen des Monats und die zweistellige Tagesangabe (siehe Abbildung 11). Rufen Sie mit dieser Änderung eines Ihrer Inhaltsseiten aus. Wie Abbildung 11 zeigt, wird das resultierende Markup sofort aktualisiert, um die Änderung der Masterseite einschließen.


[![Die Änderungen an der Masterseite werden berücksichtigt beim Anzeigen der Inhaltsseite](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**Abbildung 11**: die Änderungen an der Gestaltungsvorlage sind berücksichtigt beim Anzeigen der Inhaltsseite ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))


> [!NOTE]
> Wie in diesem Beispiel wird verdeutlicht, können Masterseiten serverseitige Web Controls, Code und Ereignishandler enthalten.


## <a name="summary"></a>Zusammenfassung

Masterseiten ermöglichen Entwicklern das ASP.NET ein konsistentes Layout für die gesamte Website entwerfen, das problemlos aktualisierbar ist. Erstellen, Gestaltungsvorlagen und ihre zugeordneten Inhaltsseiten ist so einfach wie das standardmäßige ASP.NET-Seiten erstellen, wie Visual Web Developer umfangreiche zur Entwurfszeit Unterstützung bietet.

Die Masterseite-Beispiel in diesem Lernprogramm erstellten hatte zwei ContentPlaceHolder Steuerelemente `head` und `MainContent`. Wir nur angegebene Markup für die `MainContent` ContentPlaceHolder jedoch in unserer Inhaltsseite steuern. In den nächsten Lernprogrammen untersuchen wir mehrere Inhalt mithilfe der Steuerelemente auf der Inhaltsseite. Wir sehen ferner zum Definieren der Standardeinstellung Markup für den Inhalt steuert innerhalb der Masterseite ebenfalls wie Umschalten zwischen der Verwendung des standardmäßigen Markup in der Masterdatenbank Seiten- und Bereitstellen von benutzerdefinierten Markup der Seite Inhalt definiert.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET für den Designer: Entwurfsvorlagen und Anleitungen zum Erstellen von ASP.NET-Websites mit Webstandards frei](https://msdn.microsoft.com/en-us/asp.net/aa336602.aspx)
- [Übersicht über ASP.NET Master-Seiten](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [Lernprogramme für Cascading Stylesheets (CSS)](http://www.w3schools.com/css/default.asp)
- [Titel der Seite festlegen dynamisch](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Masterseiten in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Masterseiten Schnellstart-Lernprogrammen](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Nächste](multiple-contentplaceholders-and-default-content-cs.md)
