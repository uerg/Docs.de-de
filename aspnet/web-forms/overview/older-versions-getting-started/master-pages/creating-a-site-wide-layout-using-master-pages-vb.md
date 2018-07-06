---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Erstellen einen websiteweiten Layouts mit Masterseiten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden die Grundlagen der Masterseite angezeigt. Nämlich gibt Masterseiten geben, wie funktioniert die einer Erstellen einer Masterseite, gibt der Platzhalter für Inhalte, wie ein Cr funktioniert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: b47f2d838cef8e43df83d49eecff2bae8553889e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377313"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Erstellen einen websiteweiten Layouts mit Masterseiten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> In diesem Tutorial werden die Grundlagen der Masterseite angezeigt. Nämlich, was die Masterseiten sind, wie erstellt eine Masterseite, welche Inhalte Platzhalter sind, wie erstellt eine ASP.NET-Seite, die wird verwendet, eine Masterseite, wie ändern die Masterseite automatisch in der zugeordneten Inhaltsseiten usw. dargestellt wird.


## <a name="introduction"></a>Einführung

Ein Attribut einer gut entworfenen Website ist ein einheitliches standortweite Seitenlayout. Nehmen Sie beispielsweise die www.asp.net-Website. Zum Zeitpunkt der Erstellung dieses Dokuments hat jede Seite derselben Inhalt oben und unten auf der Seite. Wie in Abbildung 1 gezeigt, zeigt der Anfang jeder Seite ein graues Balkens mit einer Liste von Microsoft Communities. Unter, das Websitelogo, die Liste der Sprachen, in dem die Website übersetzt wurde, und in den Abschnitten Core: Home, erste Schritte, Informationen, Downloads und So weiter. Ebenso enthält den unteren Rand der Seite Informationen zu Werbezwecken auf www.asp.net, eine urheberrechtserklärung und einen Link zu den Datenschutzbestimmungen.


[![Die Website www.asp.net setzt eines konsistenten Aussehens und Verhaltens für alle Seiten](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Abbildung 01</strong>: die www.asp.net Website verwendet wird, ein konsistentes Aussehen und können für alle Seiten ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Ein weiteres Attribut einer gut entworfenen Website ist die Leichtigkeit, mit der des Standorts Darstellung geändert werden kann. Abbildung 1 zeigt die Startseite www.asp.net ab März 2008, aber zwischen jetzt und in diesem Lernprogramm für die Veröffentlichung, das Aussehen und Verhalten möglicherweise geändert. Vielleicht werden die Menüelementen am oberen Rand erweitert, um einen neuen Abschnitt für das MVC-Framework enthalten. Oder vielleicht ein brandneues Design mit verschiedenen Farben, Schriftarten und Layout unveiled. Anwenden von Änderungen auf die gesamte Website sollte eine schnelle und einfache Verfahren, das Ändern von Tausenden von Webseiten, aus denen die Website nicht erfordert.

Erstellen einer Vorlage für die gesamte Website in ASP.NET kann mithilfe des *Masterseiten*. Kurz gesagt, eine Masterseite ist eine besondere Art von angezeigt, die das Markup definiert, die auf alle allgemeinen *Inhaltsseiten* sowie die Regionen, die auf einer Inhaltsseite-Seite-an-Content-Basis anpassbar sind. (Eine Inhaltsseite ist eine ASP.NET-Seite, die auf die Masterseite gebunden ist.) Sobald Layout oder Formatieren einer Masterseite geändert wird, alle seine Inhaltsseiten Ausgabe wird auch sofort aktualisiert, wodurch standortweite Darstellung Änderungen so einfach wie das Aktualisieren und Bereitstellen einer einzelnen Datei (d. h., die Masterseite) angewendet.

Dies ist im ersten Tutorial einer Reihe von Tutorials, in die Verwendung von Masterseiten untersucht. Im Verlauf dieser tutorialreihe wir:

- Erstellen von Masterseiten und ihre zugeordneten Inhaltsseiten zu untersuchen,
- Eine Vielzahl von Tipps, Tricks und fallen zu besprechen,
- Identifizieren Sie häufige Masterseite Probleme und untersuchen Sie der umgehungen,
- Erfahren Sie, wie auf der Masterseite von einer Inhaltsseite und a-umgekehrt,
- Erfahren Sie, wie eine Inhaltsseite Masterseite zur Laufzeit angeben und
- Andere erweiterte Themen der Masterseite.

Diese Lernprogramme sind darauf ausgerichtet, präzise und enthalten schrittweise Anleitungen mit zahlreichen Screenshots, die Sie visuell durch den Prozess geführt. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und umfasst ein Download, der den vollständigen Code verwendet.

In diesem ersten Lernprogramm beginnt mit einem Blick auf die Grundlagen der Masterseite. Wir erläutern die Funktionsweise von Masterseiten, sehen Sie sich das Erstellen einer Masterseite und zugeordneten Inhaltsseiten, die mit Visual Web Developer, und wie Änderungen an einer Masterseite sofort auf die Inhaltsseiten wiedergegeben werden. Fangen wir an!

## <a name="understanding-how-master-pages-work"></a>Grundlegendes zur Funktionsweise von Masterseiten

Erstellen einer Website mit einem konsistenten standortweite Seitenlayout erfordert, dass es sich bei jeder Webseite allgemeine Formatierungsmarkup zusätzlich zu den benutzerdefinierten Inhalt auszugeben. Z. B. während der einzelnen Tutorial oder das Forum Beiträge auf www.asp.net haben ihre eigenen eindeutigen Inhalt, jede dieser Seiten auch rendern eine Reihe von allgemeinen `<div>` -Elemente, die obersten Ebene Abschnitt Links anzeigen: Home, erste Schritte, Weitere Informationen und So weiter.

Es gibt eine Vielzahl von Techniken zum Erstellen von Webseiten mit eines konsistenten Aussehens und Verhaltens. Ein naiver Ansatz besteht darin, einfach kopieren und fügen das allgemeine Layout-Markup in alle Webseiten, aber dieser Ansatz bietet eine Reihe von Nachteilen. Jedes Mal eine neue Seite erstellt wird, müssen Sie zunächst einmal kopieren und fügen den freigegebenen Inhalt auf der Seite. Wie Kopieren und Einfügen sind reif für Fehler, da Sie nur eine Teilmenge der freigegebenen Markup versehentlich in eine neue Seite kopieren. Und um es abzurunden, auf diese Weise können und Ersetzen Sie die vorhandene standortweite Darstellung durch eine neue Ressourcengruppe, die eine echte umfangreichen, da jede einzelne Seite am Standort bearbeitet werden muss, um das neue Aussehen und Verhalten zu verwenden.

Vor dem ASP.NET, Version 2.0, Seite Entwickler häufig platziert allgemeine Markup im [Benutzersteuerelemente](https://msdn.microsoft.com/library/y6wb1a0e.aspx) , und klicken Sie dann diese Benutzersteuerelemente auf jeder Seite hinzugefügt. Dieser Ansatz erforderlich, dass der Seitenentwickler Denken Sie daran, jede neue Seite manuell die Steuerelemente hinzufügen, jedoch für einfacher standortweite Änderungen zulässig, da beim Aktualisieren der allgemeinen Markup benötigt nur die Steuerelemente geändert werden. Leider können Visual Studio .NET 2002 und 2003 – die Versionen von Visual Studio zum Erstellen von ASP.NET 1.x-Anwendungen - Steuerelemente in der Entwurfsansicht als graue Felder dargestellt verwendet werden. Daher Seitenentwickler, die mit diesem Ansatz keine WYSIWYG-Entwurfszeit-Umgebung nutzen.

Die Mängel bei der Verwendung der Steuerelemente in ASP.NET Version 2.0 und Visual Studio 2005 behandelt wurden, mit der Einführung von *Masterseiten*. Eine Masterseite ist eine besondere Art von angezeigt, die sowohl das standortweite Markup definiert und die *Regionen* bei verknüpften *Inhaltsseiten* definieren Sie ihre benutzerdefinierten Markups. Wie in Schritt 1, werden diese Bereiche von ContentPlaceHolder-Steuerelemente definiert. Das ContentPlaceHolder-Steuerelement gibt einfach an eine Position in der Steuerelementhierarchie der Masterseite, in dem benutzerdefinierter Inhalt von einer Inhaltsseite eingefügt werden können.

> [!NOTE]
> Die grundlegenden Konzepte und Funktionalität von Masterseiten wurde nicht geändert, seit ASP.NET-Version 2.0. Visual Studio 2008 bietet jedoch zur Entwurfszeit Unterstützung für verschachtelte Gestaltungsvorlagen, ein Feature, das in Visual Studio 2005 gefehlt. Betrachten wir mit geschachtelten Masterseiten in einem späteren Tutorial.


Abbildung 2 zeigt, wie die Masterseite für www.asp.net aussehen könnte. Beachten Sie, dass die Masterseite der allgemeinen websiteweiten Layouts - Markup an die oberen, unteren und rechten Rand jeder Seite – sowie ein ContentPlaceHolder-Objekt in der Mitte links, definiert, wo sich die Inhalte für jede einzelne Webseite befindet.


![Eine Masterseite definiert das Layout der Website-Ebene sowie der Regionen, die bearbeitet werden pro Seite Inhalt vom Seiteninhalt](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Abbildung 02**: eine Masterseite definiert das Layout der Website-Ebene sowie der Regionen, die bearbeitet werden pro Seite Inhalt vom Seiteninhalt


Nachdem eine Masterseite definiert wurde kann es zu ASP.NET-Seiten durch die Teilstriche eines Kontrollkästchens gebunden werden. Diese - wird aufgerufen, Inhaltsseiten - ASP.NET-Seiten enthalten ein ContentControl-Element für jede der Masterseite ContentPlaceHolder-Steuerelemente. Bei die Inhaltsseite über einen Browser zugegriffen wird ist das ASP.NET-Modul erstellt die Steuerelementhierarchie der Masterseite und Steuerelementhierarchie der Seite Inhalt in den entsprechenden Stellen einfügt. Dieser kombinierte Steuerelementhierarchie gerendert wird, und die resultierende HTML an den Browser des Benutzers zurückgegeben. Daher gibt die Seite Inhalte auf, die allgemeine Markup in die Masterseite außerhalb der ContentPlaceHolder-Steuerelemente definiert und das seitenspezifische-Markup, das in eine eigene ContentControl-Elemente definiert. Abbildung 3 veranschaulicht dieses Konzept.


[![Die angeforderte Seite Markup wird in die Masterseite Fused.](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Abbildung 03**: die angeforderte Seite Markup wird in die Masterseite Fused ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Nun, da wir die Funktionsweise von Masterseiten erläutert haben, werfen wir einen Blick auf das Erstellen einer Masterseite und zugeordneten Inhaltsseiten, die mit Visual Web Developer.

> [!NOTE]
> Um die größtmögliche Zielgruppe erreichen, wir, in der gesamten dieser Reihe von Lernprogrammen erstellen, ASP.NET-Website erstellt werden Microsofts kostenlose Version von Visual Studio 2008 mit ASP.NET 3.5 [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Wenn Sie noch nicht keine, ein Upgrade auf ASP.NET 3.5 Sorge – gut in diesen Tutorials arbeiten vorgestellten Konzepte genauso mit ASP.NET 2.0 und Visual Studio 2005. Einige demoanwendungen möglicherweise jedoch neue Funktionen in .NET Framework, Version 3.5 verwenden. Wenn 3.5-spezifischen Funktionen verwendet werden, enthalten eine Anmerkung, die erläutert, wie Sie ähnliche Funktionalität in Version 2.0 implementieren. Beachten Sie, die die für demoanwendungen verfügbar von jedes Tutorial Ziel .NET Framework, Version 3.5 herunterladen Was führt eine `Web.config` Datei, die 3.5-spezifische Konfigurationselemente enthält. Kurz gesagt, wenn Sie noch so installieren Sie .NET 3.5 auf Ihrem Computer klicken Sie dann die herunterladbare Webanwendung funktioniert nicht ohne Sie zuerst die 3.5-spezifische Markup aus `Web.config`. Finden Sie unter [Analyse ASP.NET Version 3.5 die `Web.config` Datei](http://www.4guysfromrolla.com/articles/121207-1.aspx) für Weitere Informationen zu diesem Thema.


## <a name="step-1-creating-a-master-page"></a>Schritt 1: Erstellen einer Masterseite

Wir erstellen und Verwenden von Master- und Inhaltsseiten untersuchen können, benötigen wir aber zunächst eine ASP.NET-Website. Zunächst erstellen eine neue Datei systembasierte ASP.NET-Website. Um dies zu erreichen, starten Sie Visual Web Developer finden Sie unter dem Menü "Datei" und wählen Sie die neue Website, die das Dialogfeld "neue Website" angezeigt (siehe Abbildung 4). Wählen Sie die Vorlage für ASP.NET Web Site, legen Sie die Dropdownliste den Speicherort auf File System, wählen Sie einen Ordner aus, um die Website zu platzieren und Festlegen der Sprache Visual Basic. Hiermit wird eine neue Website mit einer `Default.aspx` ASP.NET-Seite ein `App_Data` Ordner, und ein `Web.config` Datei.

> [!NOTE]
> Visual Studio unterstützt zwei Modi des Projektmanagements: Websiteprojekten und Webanwendungsprojekten. Web Site Projects fehlt eine Projektdatei, während Web Application Projects die Projektarchitektur in Visual Studio .NET 2002/2003 imitieren-sie enthalten eine Projektdatei, und Kompilieren von Quellcode des Projekts in eine einzelne Assembly, die in platziert ist die `/bin` Ordner. Visual Studio 2005 Projekt anfangs nur unterstützte Website, obwohl das Webanwendungsprojekt-Modell mit Service Pack 1 wieder eingeführt wurde. Visual Studio 2008 bietet beide Projektmodelle. Die Visual Web Developer 2005 und 2008-Editionen unterstützen jedoch nur von Websiteprojekten. Für meinen Demos in diesem Tutorial verwende ich das Websiteprojekt-Modell. Wenn Sie eine nicht-Express-Edition verwenden und, verwenden Sie möchten die [Webanwendungsprojekt-Modell](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) stattdessen gerne tun, aber beachten Sie, dass es möglicherweise einige Diskrepanzen zwischen der Anzeige auf Ihrem Bildschirm und welche Schritte Sie müssen im Vergleich zu den dargestellten bildschirmabbildungen und Anweisungen, die in diesen Tutorials.


[![Erstellen einer neuen System-basierte-Website](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Abbildung 04**: Erstellen einer Website New File System-Based ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Fügen Sie eine Masterseite an die Website in das Stammverzeichnis von mit der rechten Maustaste auf den Projektnamen, wählen neues Element hinzufügen und Auswählen der Masterseitenvorlage. Beachten Sie, dass mit der Erweiterung Masterseiten enden `.master`. Nennen Sie diese neue Masterseite `Site.master` , und klicken Sie auf Hinzufügen.


[![Fügen Sie eine Master-Seite mit dem Namen "Site.Master" auf der Website](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Abbildung 05**: Fügen Sie eine Master-Seite mit dem Namen `Site.master` auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Hinzufügen einer neuen Masterseitendatei durch Visual Web Developer erstellt eine Masterseite mit den folgenden deklarativem Markup:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

Die erste Zeile im deklarativen Markup ist die [ `@Master` Richtlinie](https://msdn.microsoft.com/library/ms228176.aspx). Die `@Master` Richtlinie ähnelt der [ `@Page` Richtlinie](https://msdn.microsoft.com/library/ydy4x04a.aspx) , das in ASP.NET-Seiten angezeigt wird. Es definiert die serverseitigen Sprache (VB) und Informationen über den Speicherort und die Vererbung von der Masterseite Code-Behind-Klasse.

Die `DOCTYPE` und deklarativen Markup der Seite angezeigt wird, unter dem `@Master` Richtlinie. Die Seite enthält die statischen HTML-Daten zusammen mit vier serverseitige Steuerelemente:

- **Ein Webformular (die `<form runat="server">`)** : Da alle ASP.NET-Seiten in der Regel haben ein Web Form - und, da die Masterseite Websteuerelemente enthalten kann, die innerhalb eines Webformulars – müssen unbedingt das Web Form Ihrer Masterseite (statt ein Web Form hinzufügen, für die e hinzufügen ACH-Inhaltsseite).
- **Ein ContentPlaceHolder-Steuerelement namens `ContentPlaceHolder1`**  -dieses ContentPlaceHolder-Steuerelement angezeigt wird, innerhalb des Web Forms und dient als der Region für die Benutzeroberfläche der Seite Inhalt.
- **Ein serverseitiges `<head>` Element** : die `<head>` Element verfügt über die `runat="server"` Attribut, das über den serverseitigen Code zugänglich machen. Die `<head>` Element auf diese Weise implementiert wird, damit der Titel der Seite und andere `<head>`-bezogene Markup hinzugefügt oder programmgesteuert angepasst werden kann. Z. B. Festlegen einer ASP.NET-Seite `Title` eigenschaftenänderungen die `<title>` Element gerendert wird, durch die `<head>` -Steuerelement.
- **Ein ContentPlaceHolder-Steuerelement namens `head`**  -dieses ContentPlaceHolder-Steuerelement angezeigt wird, innerhalb der `<head>` Server steuern und können verwendet werden, um Inhalt deklarativ hinzufügen, die `<head>` Element.

Diese deklarativen Masterseite Standardmarkup dient als Ausgangspunkt für eigene Masterseiten entwerfen. Können Sie den HTML-Code bearbeiten oder zusätzliche Websteuerelemente oder ContentPlaceHolder-Steuerelemente auf die Masterseite hinzufügen.

> [!NOTE]
> Master-Seiten-Schnellstart-Lernprogramme


### <a name="creating-a-simple-site-layout"></a>Der Autor

Scott Mitchell, Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt `Default.aspx` Sams Teach selbst ASP.NET 3.5 in 24 Stunden.


[![Besonderen Dank an](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Meine zukünftigen MSDN-Artikeln überprüfen möchten?


Wenn dies der Fall ist, löschen Sie mir eine Linie an `Site.master`  .

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Die Masterseite-Layout wird definiert, mit einer Reihe von `<div>` HTML-Elemente. Die `topContent` `<div>` das Markup enthält, die am oberen Rand jeder Seite wird angezeigt, während die `mainContent`, `leftContent`, und `footerContent` `<div>` s werden verwendet, um den Inhalt der Seite, der linken Spalte und die "unterstützt von Microsoft anzeigen ASP.NET"Symbol bzw. Neben dem Hinzufügen dieser `<div>` Elemente, habe ich auch umbenannt der `ID` -Eigenschaft des Steuerelements aus primären ContentPlaceHolder `ContentPlaceHolder1` zu `MainContent`.

Die Formatierung und Layout-Regeln für diese ausgewählten `<div>` Elemente wird ausgeschrieben, der [Cascading Stylesheet (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) Datei `Styles.css`, über angegeben wird ein `<link>` Element in der Masterseite der `<head>`Element. Diese verschiedenen Regeln definieren, das Aussehen und Verhalten der einzelnen `<div>` Element oben notiert haben. Z. B. die `topContent` `<div>` -Element, das den Text der "Master-Seiten-Lernprogramme" und den Link angezeigt wird, hat die Formatierungsregeln im angegebenen `Styles.css` wie folgt:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Wenn Sie auf Ihrem Computer befolgt werden, müssen Sie dieses Tutorial den Codedownload herunterladen und Hinzufügen der `Styles.css` Datei zum Projekt. Auf ähnliche Weise, Sie müssen auch einen Ordner mit dem Namen erstellen `Images` , und kopieren Sie das Symbol "Unterstützt von Microsoft ASP.NET" aus der heruntergeladenen demowebsite zu Ihrem Projekt.

> [!NOTE]
> Eine Erörterung der CSS-Formatierung Webseite sprengen den Rahmen dieses Artikels aus. Weitere Informationen zu CSS, sehen Sie sich die [CSS-Tutorials](http://www.w3schools.com/css/default.asp) am [W3Schools.com](http://www.w3schools.com/). Außerdem sollten Sie dieses Tutorial den Codedownload herunterladen und Testen Sie die CSS-Einstellungen in `Styles.css` die Auswirkungen der verschiedenen Regeln zur Formatierung angezeigt.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Erstellen einer Masterseite mit einer vorhandenen Entwurfsvorlage für den

Im Laufe der Jahre habe ich eine Anzahl von ASP.NET Web-Anwendungen für kleine-mittelgroße Unternehmen entwickelt. Einige meiner Kunden hatten eines vorhandene Website-Layouts, die, das Sie verwenden möchten; andere eingestellt zuständigen Grafikdesigner. Ein paar wie mich, um das Layout der Website zu entwerfen. Wie Sie von Abbildung 6 erkennen können, empfiehlt Multitaskings auf dem einen Programmierer zum Entwerfen einer Website Layout in der Regel als mit Ihren Steuerberater eine Operation am offenen Herzen ausgeführt werden, während es sich bei Ihrem Arzt besprechen steuern ist.

Glücklicherweise stehen innumerous Websites mit kostenlosen HTML-Design-Vorlagen – Google mehr als sechs Millionen Ergebnisse zurückgegeben, für den Suchbegriff "kostenlose Websitevorlagen". Eine der besten aufgelistet ist [OpenDesigns.org](http://opendesigns.org/). Sobald Sie eine Vorlage für die Website, die Sie möchten die CSS-Dateien und Images Ihrer Websiteprojekt hinzufügen und Integrieren der Vorlage HTML auf Ihrer Masterseite gefunden.

> [!NOTE]
> Microsoft bietet auch eine Reihe von [kostenlose ASP.NET Start Kit Entwurfsvorlagen](https://msdn.microsoft.com/asp.net/aa336613.aspx) , die in Visual Studio im Dialogfeld Neue Website integriert.


## <a name="step-2-creating-associated-content-pages"></a>Schritt 2: Erstellen von Inhaltsseiten verknüpft

Mit der Masterseite erstellt wird können wir zum Erstellen von ASP.NET-Seiten, die auf die Masterseite gebunden sind. Diese Seiten werden als bezeichnet *Inhaltsseiten*.

Wir fügen Sie eine neue ASP.NET-Seite auf das Projekt, und binden Sie es an der `Site.master` Masterseite. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie die Option neues Element hinzufügen. Wählen Sie die Web Form-Vorlage aus, geben Sie den Namen `About.aspx`, und überprüfen Sie dann das Kontrollkästchen "Masterseite auswählen" aus, wie in Abbildung 7 dargestellt. Auf diese Weise wird in diesem Dialogfeld Wählen Sie die Masterseite im Feld aus, in dem Sie die Masterseite mit auswählen können (siehe Abbildung 8).

> [!NOTE]
> Wenn Sie Ihre ASP.NET-Website das Webanwendungsprojekt-Modell verwenden, anstatt das Websiteprojekt-Modell erstellt haben, sehen Sie nicht das Kontrollkästchen "Masterseite auswählen", in das Dialogfeld "Neues Element hinzufügen" in Abbildung 7 dargestellt. Seite mithilfe des Webanwendungsprojekts Modellieren Sie müssen die Inhalte Webformularvorlage anstelle der Web Form-Vorlage auswählen, um einen Inhalt zu erstellen. Nach dem Auswählen der Vorlage Webinhaltsformular, und klicken Sie auf Hinzufügen, wählen Sie den gleichen eine Masterseite, die in Abbildung 8 dargestellte Dialogfeld angezeigt wird.


[![Fügen Sie eine neue Seite hinzu.](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Abbildung 07**: Fügen Sie eine neue Seite hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Wählen Sie die Site.master-Masterseite](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Abbildung 08**: Wählen Sie die `Site.master` Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Wie in der folgende deklarative Markup wird gezeigt, eine neue Seite enthält eine `@Page` Anweisung, die Punkte in der Master Seite und ein ContentControl-Element für jedes der Masterseite ContentPlaceHolder-Steuerelemente sichern.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> Im Abschnitt "Erstellen einer einfachen Website Layout" in Schritt 1 habe ich umbenannt `ContentPlaceHolder1` zu `MainContent`. Wenn Sie nicht diese ContentPlaceHolder-Steuerelement umbenennen, `ID` auf die gleiche Weise, Ihre Inhaltsseite deklaratives Markup wird unterscheiden sich geringfügig aus dem Markup oben. Nämlich des zweite Inhaltssteuerelements des `ContentPlaceHolderID` spiegeln die `ID` des entsprechenden ContentPlaceHolder, die in Ihrer Masterseite steuern.


Beim Rendern von einer Inhaltsseite, muss das ASP.NET-Modul der Seite fuse Inhaltssteuerelemente mit der Masterseite ContentPlaceHolder-Steuerelemente. Die ASP.NET-Engine bestimmt die Masterseite der Inhaltsseite, von der `@Page` -Direktive `MasterPageFile` Attribut. Wie das obenstehende Markup zeigt, ist diese Inhaltsseite an gebunden `~/Site.master`.

Da die Masterseite zwei ContentPlaceHolder-Steuerelemente - verfügt über `head` und `MainContent` -Visual Web Developer generiert zwei Inhaltssteuerelemente. Jedes Inhaltssteuerelement verweist auf ein bestimmtes ContentPlaceHolder-Objekt über dessen `ContentPlaceHolderID` Eigenschaft.

Masterseiten, in denen über vorherige standortweite Vorlage Techniken bringen ist ihre Unterstützung während der Entwurfszeit. Abbildung 9 zeigt die `About.aspx` Inhaltsseite, wenn Sie über Visual Web Developer-Entwurfsansicht angezeigt. Beachten Sie, dass während der Inhalt der Masterseite angezeigt wird, abgeblendet ist und kann nicht geändert werden. Die Inhaltssteuerelemente, die für die Masterseite ContentPlaceHolder-Steuerelemente können, jedoch bearbeitet werden. Und nur verwendet werden, wie Sie mit einer beliebigen anderen ASP.NET-Seite durch Hinzufügen von Web-Steuerelementen über die Ansichten Quelltext- oder Entwurfssicht der Inhaltsseite Schnittstelle erstellen können.


[![Die Inhaltsseite die Entwurfsansicht zeigt sowohl den Seiteninhalt der seitenspezifische und Master](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Abbildung 09**: die Seite Inhalt des Design View zeigt sowohl den seitenspezifischen und Inhalt der Master-Seite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Hinzufügen von Markup und Websteuerelemente auf der Seite Inhalt

Einige Inhalte für erstellen in Ruhe die `About.aspx` Seite. Sie können finden Sie in Abbildung 10: ich eine Überschrift "Über die Author" und eine Reihe von Absätze mit Text eingegeben, aber gerne Websteuerelemente hinzuzufügen. Besuchen Sie nach dem Erstellen dieser Schnittstelle, die `About.aspx` Seite über einen Browser.


[![Besuchen Sie die Seite About.aspx über einen Browser](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Abbildung 10**: finden Sie auf die `About.aspx` Seite über ein Browser ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


Es ist wichtig zu verstehen, dass auf der Seite für den angeforderten Inhalt und der zugeordneten Masterseite mit Sicherung und als Ganzes vollständig auf dem Webserver gerendert. Die vom Browser des Benutzers wird den resultierenden fused HTML-Code gesendet. Um dies zu überprüfen, zeigen Sie den HTML-Code, der vom Browser empfangen werden, indem Sie das Menü "Ansicht" und Quelle auswählen. Beachten Sie, dass keine Rahmen oder eine beliebige andere Techniken für die Anzeige von zwei verschiedenen Webseiten in einem einzelnen Fenster vorhanden sind.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Binden eine Masterseite an eine vorhandene ASP.NET-Seite

Wie wir in diesem Schritt gesehen haben, ist das eine neue Seite hinzufügen, um eine ASP.NET-Webanwendung so einfach wie das Aktivieren des Kontrollkästchens "Masterseite auswählen" und die Masterseite auswählen. Es ist leider nicht so einfach, konvertieren eine vorhandene ASP.NET-Seite in einer Masterseite.

Um eine Masterseite an eine vorhandene ASP.NET-Seite zu binden, müssen Sie die folgenden Schritte ausführen:

1. Hinzufügen der `MasterPageFile` -Attribut auf der ASP.NET-Seite `@Page` verweisen auf die entsprechende Seite für die master-Direktive.
2. Hinzufügen von Inhaltssteuerelementen für jede der ContentPlaceHolder-Steuerelemente auf der Masterseite.
3. Selektiv schneiden Sie, und fügen Sie die ASP.NET-Seite vorhandenen Inhalte in den entsprechenden Inhaltssteuerelementen. Ich sage "selektiv" hier, da ASP.NET Seite wahrscheinlich enthält Markup, das bereits von der Masterseite, wie z. B. ausgedrückt wird die `DOCTYPE`, `<html>` Element, und das Web Form.

Schrittweise Anweisungen zu diesem Prozess zusammen mit Screenshots finden Sie in [Scott Guthrie](https://weblogs.asp.net/scottgu/)des [unter Verwendung der Masterseiten und Sitenavigation](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) Tutorial. Die "Update `Default.aspx` und `DataSample.aspx` mit der Masterseite" Abschnitt werden die folgenden Schritte aus.

Da es viel einfacher ist, neue Inhaltsseiten zu erstellen, statt vorhandene ASP.NET-Seiten in Inhaltsseiten zu konvertieren, empfehle ich, dass bei jedem erstellen eine neue ASP.NET-Website eine Masterseite der Website hinzufügen. Binden Sie alle ASP.NET-Seiten auf diese Masterseite. Keine Sorge, wenn die anfängliche Masterseite sehr einfach oder einfache ist; Sie können die Masterseite später aktualisieren.

> [!NOTE]
> Wenn Sie eine neue ASP.NET-Anwendung erstellen, fügt Visual Web Developer eine `Default.aspx` Seite, die nicht an eine Masterseite gebunden ist. Wenn Sie die Methode konvertieren eine vorhandene ASP.NET-Seite in einer Inhaltsseite möchten, fahren Sie fort, und zu diesem Zweck mit `Default.aspx`. Alternativ können Sie löschen `Default.aspx` , und klicken Sie dann wieder hinzu, aber dieses Mal mit dem Aktivieren des Kontrollkästchens "Masterseite auswählen".


## <a name="step-3-updating-the-master-pages-markup"></a>Schritt 3: Aktualisieren der Masterseite Markup

Einer der Hauptvorteile von Masterseiten ist, dass eine einzige masterdseite definiert das allgemeine Layout für zahlreiche Seiten auf der Website verwendet werden kann. Und benötigt deshalb das Aktualisieren des Standorts Aussehen und Verhalten Aktualisieren einer einzelnen Datei - Masterseite.

Um dieses Verhalten zu veranschaulichen, aktualisieren wir unsere Masterseite Einbeziehung des aktuellen Datums im am oberen Rand der linken Spalte ein. Fügen Sie eine Bezeichnung mit dem Namen `DateDisplay` auf die `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Als Nächstes erstellen Sie eine `Page_Load` -Ereignishandler für den Master Seite, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

Der Code oben legt fest, der Bezeichnung `Text` Eigenschaft, um das aktuelle Datum und die Uhrzeit formatiert, als der Tag der Woche, die den Namen des Monats, und die zweistellige Tagesangabe (siehe Abbildung 11). Rufen Sie eine der Inhaltsseiten für die durch diese Änderung. Wie in Abbildung 11 gezeigt, wird das daraus resultierende Markup wird sofort aktualisiert, um die Änderung auf die Masterseite einschließen.


[![Die Änderungen an der Masterseite werden berücksichtigt beim Anzeigen der Inhaltsseite](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Abbildung 11**: die Änderungen an der Masterseite werden berücksichtigt beim Anzeigen der Inhaltsseite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Wie in diesem Beispiel wird veranschaulicht, können Masterseiten serverseitige Web-Steuerelemente, Code und Ereignishandler enthalten.


## <a name="summary"></a>Zusammenfassung

Mit Masterseiten können ASP.NET-Entwickler, ein konsistentes Layout für die gesamte Website zu entwerfen, das problemlos aktualisierbar ist. Erstellen von Masterseiten und ihre zugeordneten Inhaltsseiten ist so einfach wie das Erstellen von standardmäßiger ASP.NET-Seiten, da Visual Web Developer umfassende entwurfszeitunterstützung bietet.

Die Masterseite-Beispiel in diesem Tutorial erstellten mussten zwei ContentPlaceHolder-Steuerelemente, Head "und" MainContent. Wir angegeben nur-Markup für das Steuerelement MainContent ContentPlaceHolder unsere Inhalte auf der Seite jedoch. Im nächsten Tutorial betrachten wir unter Verwendung mehrerer Inhalts Steuerelemente auf der Seite Inhalt. Außerdem erfahren Sie, wie zum Definieren von Standard-Markup für Inhalt Steuerelemente innerhalb der Masterseite, sowie zum Wechseln zwischen der Verwendung der standardmäßigen Markup in der Masterdatenbank Seite und Bereitstellen von benutzerdefinierten Markup auf der Seite Inhalt definiert.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET für Designer: kostenlose Entwurfsvorlagen und Anleitungen zum Erstellen von ASP.NET-Websites mit Webstandards](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Übersicht über ASP.NET-Masterseiten](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutorials für Cascading Stylesheets (CSS)](http://www.w3schools.com/css/default.asp)
- [Der Titel der Seite festzulegen dynamisch](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Masterseiten in ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Master-Seiten-Schnellstart-Lernprogramme](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](nested-master-pages-cs.md)
> [Weiter](multiple-contentplaceholders-and-default-content-vb.md)
