---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Mehrere ContentPlaceHolders und Standardinhalt (VB) | Microsoft Docs
author: rick-anderson
description: "Überprüft, wie einer Masterseite mehrere Content Platzhaltern hinzugefügt sowie zum Angeben der Standardinhalt in den Inhalten Platzhaltern."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: ccb65f0b2f16e0c7a67787f7dfab14303daeca1d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Mehrere ContentPlaceHolders und Standardinhalt (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Überprüft, wie einer Masterseite mehrere Content Platzhaltern hinzugefügt sowie zum Angeben der Standardinhalt in den Inhalten Platzhaltern.


## <a name="introduction"></a>Einführung

In dem vorherigen Lernprogramm untersucht wie Masterseiten aktivieren ASP.NET-Entwickler, ein konsistentes Layout für die gesamte Website erstellen. Masterseiten definieren das Markup, die alle seine Inhaltsseiten und Regionen, die auf Basis von Seite angepasst werden. Im vorherigen Lernprogramm erstellt wir eine einfache Gestaltungsvorlage (`Site.master`) und zwei Inhaltsseiten (`Default.aspx` und `About.aspx`). Unsere Masterseite umfasste zwei ContentPlaceHolders mit dem Namen `head` und `MainContent`, die diese befanden sich in der `<head>` Element- und Web Form bzw. Während der Inhaltsseiten jedes zwei Inhaltssteuerelemente hatten, nur angegebenen Markup für die entspricht `MainContent`.

Wie durch die beiden ContentPlaceHolder-Steuerelemente in belegt `Site.master`, eine Masterseite kann mehrere ContentPlaceHolders enthalten. Dazu kann die Gestaltungsvorlage Standardmarkup für ContentPlaceHolder-Steuerelemente angeben. Inhaltsseite, kann dann optional angeben sein eigenes Markup oder das Standardmarkup. In diesem Lernprogramm werden mithilfe mehrerer Inhaltssteuerelemente in die Masterseite betrachten und erfahren Sie, wie in den Steuerelementen ContentPlaceHolder Standardmarkup definieren.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Schritt 1: Hinzufügen von zusätzlichen ContentPlaceHolder-Steuerelementen auf der Seite "Master"

Viele Websitedesigns enthalten mehrere Bereiche auf dem Bildschirm, die auf Basis von Seite angepasst werden. `Site.master`, die Gestaltungsvorlage in dem vorherigen Lernprogramm erstellten enthält eine einzelne ContentPlaceHolder innerhalb der Webformular mit dem Namen `MainContent`. Insbesondere diese ContentPlaceHolder befindet sich in der `mainContent` `<div>` Element.

Abbildung 1 zeigt `Default.aspx` über einen Browser angezeigt. Die Region im rote eingekreisten ist das seitenspezifische Markup entspricht `MainContent`.


[![Die Eingekreiste Region zeigt den Bereich auf Basis von Seite derzeit anpassbare](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Abbildung 01**: die Region mit Kreis zeigt der Bereich als derzeit anpassbare auf Basis von Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Stellen Sie sich vor, dass zusätzlich zu der Region in Abbildung 1 dargestellt, ebenfalls seitenspezifische Elemente der linken Spalte unterhalb der Lektionen und Neuigkeiten hinzufügen müssen Abschnitte. Um dies zu erreichen, fügen wir ein weiteres ContentPlaceHolder-Steuerelement der Masterseite. Um nachzuvollziehen, öffnen Sie die `Site.master` Masterseite in Visual Web Developer und ziehen Sie dann ein ContentPlaceHolder-Steuerelement aus der Toolbox in den Designer nach dem News-Abschnitt. Legen Sie die ContentPlaceHolder `ID` auf `LeftColumnContent`.


[![Hinzufügen eines ContentPlaceHolder-Steuerelements auf der linken Spalte der Gestaltungsvorlage](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Abbildung 02**: Hinzufügen eines ContentPlaceHolder-Steuerelements auf der Masterseite linken Spalte ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Durch das Hinzufügen der `LeftColumnContent` ContentPlaceHolder der Masterseite, wir können definieren Inhalt für diese Region auf Basis von Seite einschließlich einer Inhalte steuern auf der Seite, deren `ContentPlaceHolderID` auf festgelegt ist `LeftColumnContent`. Untersuchen wir dabei in Schritt2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Schritt 2: Definieren von Inhalt für die neue ContentPlaceHolder in Inhaltsseiten

Wenn die Website eine neue Seite hinzufügen, erstellt Visual Web Developer automatisch einen Inhalt Steuerelement auf der Seite für jede ContentPlaceHolder in der ausgewählten Masterseite. Hinzugefügt haben einen der `LeftColumnContent` ContentPlaceHolder unsere Masterseite in Schritt 1, neue ASP.NET Seiten wird jetzt haben Sie drei Content-Steuerelemente.

Um dies zu veranschaulichen, fügen Sie eine neue Seite in das Stammverzeichnis, das mit dem Namen `MultipleContentPlaceHolders.aspx` , gebunden ist, um die `Site.master` Masterseite. Visual Web Developer erstellt diese Seite mit den folgenden deklarativem Markup:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Geben Sie einige Inhalte in das Inhaltssteuerelement verweisen auf die `MainContent` ContentPlaceHolders (`Content2`). Fügen Sie das folgende Markup zum Rendern der `Content3` Inhaltssteuerelement (die Verweise der `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Nach dem Hinzufügen dieses Markup, finden Sie auf der Seite über einen Browser. Wie in Abbildung 3 gezeigt, wird das Markup eingefügt, der `Content3` Inhaltssteuerelement wird angezeigt, in der linken Spalte unterhalb der News-Abschnitt (rot markiert). In das Markup eingefügt `Content2` wird im rechten Teil der Seite "(blau markiert)" angezeigt.


[![Die linke Spalte enthält nun seitenspezifische Inhalt unter der News-Abschnitt](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Abbildung 03**: die linke Spalte jetzt enthält seitenspezifische unterhalb der News Inhaltsabschnitt ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definieren von Inhalt in vorhandenen Inhaltsseiten

Erstellen eine neue Seite automatisch umfasst das ContentPlaceHolder-Steuerelement, die in Schritt 1 hinzugefügt wurden. Aber unsere zwei vorhandenen Inhaltsseiten - `About.aspx` und `Default.aspx` -besitzen kein Steuerelement für die `LeftColumnContent` ContentPlaceHolder. Zum Angeben von Inhalt für dieses ContentPlaceHolder diese zwei vorhandene Seiten müssen wir uns fügen Sie ein Inhaltssteuerelement hinzu.

Im Gegensatz zu den meisten ASP.NET Web-Steuerelemente schließt Visual Web Developer-Toolbox nicht auf ein Inhaltssteuerelement-Element zu. Wir können manuell in das Inhaltssteuerelement deklarativem Markup eingeben, in der Datenquellensicht an, aber ein einfacher und schneller Ansatz ist die Verwendung die Entwurfsansicht. Öffnen der `About.aspx` Seite, und wechseln Sie zur Entwurfsansicht. Wie in Abbildung 4 dargestellt, die `LeftColumnContent` ContentPlaceHolder wird in der Entwurfsansicht angezeigt; Wenn Sie den Mauszeiger darüber, liest des angezeigte Titels: "LeftColumnContent (Master)." Die Aufnahme von "Meister" im Titel Gibt an, dass keine Inhaltssteuerelement, das auf der Seite für diese ContentPlaceHolder definiert. Wenn Sie vorhanden sind, die ein Steuerelement für die ContentPlaceHolder, wie im Fall für `MainContent`, der Titel wird gelesen: "*ContentPlaceHolderID* (Benutzerdefiniert)."

Hinzufügen ein Inhaltssteuerelements für die `LeftColumnContent` ContentPlaceHolder auf `About.aspx`, erweitern Sie die ContentPlaceHolder Smarttag, und klicken Sie auf den Link erstellen benutzerdefinierte Content.


[![Die Entwurfsansicht für About.aspx zeigt LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Abbildung 04**: die Entwurfsansicht für `About.aspx` zeigt die `LeftColumnContent` ContentPlaceHolder ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Durch Klicken auf das Erstellen benutzerdefinierter Inhaltslink generiert die erforderlichen Inhaltssteuerelement in der Seite und legt seine `ContentPlaceHolderID` Eigenschaft, um die ContentPlaceHolder `ID`. Z. B. durch Klicken auf das Erstellen benutzerdefinierter inhaltsverknüpfung `LeftColumnContent` Region in `About.aspx` fügt der Seite das folgende deklaratives Markup hinzu:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Das Auslassen von Inhaltssteuerelementen

ASP.NET ist nicht erforderlich, alle Inhaltsseiten Content-Steuerelemente für jede ContentPlaceHolder definiert, in die Masterseite einschließen. Wenn ein Inhaltssteuerelement weggelassen wird, verwendet das Modul für ASP.NET das Markup, das in die Masterseite ContentPlaceHolder definiert. Dieses Markup wird als die ContentPlaceHolder bezeichnet *Standardinhalt* und eignet sich für Szenarien, in denen der Inhalt für ein Bereich für die Mehrzahl der Seiten, ist jedoch muss bei einer kleinen Anzahl von Seiten angepasst werden. Schritt 3 untersucht Angabe Standardinhalt der Masterseite.

Derzeit `Default.aspx` enthält zwei Content-Steuerelemente für die `head` und `MainContent` ContentPlaceHolders; er verfügt nicht über ein Steuerelement für `LeftColumnContent`. Daher, wenn `Default.aspx` gerendert wird die `LeftColumnContent` ContentPlaceHolders Standardinhalt verwendet wird. Da wir noch Standardinhalt für diese ContentPlaceHolder definiert haben, wird letztendlich an, dass kein Markup für diese Region ausgegeben wird. Um dieses Verhalten zu überprüfen, besuchen Sie `Default.aspx` über einen Browser. Wie in Abbildung 5 gezeigt, wird kein Markup in der linken Spalte unterhalb der Abschnitt News ausgegeben.


[![Für die LeftColumnContent ContentPlaceHolder wird kein Inhalt dargestellt.](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Abbildung 05**: No Content wird gerendert, für die `LeftColumnContent` ContentPlaceHolder ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Schritt 3: Angeben von Standardinhalt der Gestaltungsvorlage

Einige Websitedesigns umfassen einen Bereich zurück, deren Inhalt für alle Seiten der Website mit Ausnahme von einem oder zwei Ausnahmen identisch ist. Erwägen Sie eine Website, die Benutzerkonten unterstützt. Eine solche Website erfordert eine Anmeldeseite, in denen Besucher ihrer Anmeldeinformationen anmelden bei der Website eingeben können. Um den Anmeldevorgang zu beschleunigen, können die Website-Designer Benutzername und Kennwort Textfelder in der oberen linken Ecke von jeder Seite, um Benutzern das anmelden, ohne explizit die Login-Seite besuchen ermöglichen enthalten. Während diese Textfelder Benutzername und Kennwort in die meisten Seiten hilfreich sind, sind sie redundante auf der Anmeldungsseite, in die Textfelder ein, für die Anmeldeinformationen des Benutzers bereits enthält.

Um die Implementierung dieses Entwurfs konnte ein ContentPlaceHolder-Steuerelement in der oberen linken Ecke der Masterseite erstellt werden. Jede Seite, die erforderlich, um den Textfeldern Benutzername und Kennwort in der oberen linken Ecke angezeigt würde erstellen ein Inhaltssteuerelement für diese ContentPlaceHolder und fügen die erforderliche Schnittstelle. Die Anmeldeseite, auf der anderen Seite würde entweder Hinzufügen eines Inhaltssteuerelements für diese ContentPlaceHolder weggelassen, da einen Inhalt erstellen Steuerelement kein Markup definiert. Der Nachteil dieses Ansatzes ist, dass wir denken Sie daran, die Textfeldern Benutzername und Kennwort auf jeder Seite hinzufügen, die an den Standort (mit Ausnahme der Anmeldeseite) hinzugefügt werden. Dies ist für Trouble gefragt werden. Wir sind wahrscheinlich vergessen, die diese Textfelder auf einer Seite oder zwei hinzuzufügen oder schlimmer, möglicherweise nicht implementiert die Schnittstelle ordnungsgemäß (z. B. statt zwei nur ein Textfeld hinzufügen).

Eine bessere Lösung wird der Benutzername und Kennwort Textfelder als Standardinhalt der ContentPlaceHolder definiert. Auf diese Weise wir brauchen nur diese Standardinhalt in diese Seiten zu überschreiben, die nicht die Textfeldern Benutzername und Kennwort angezeigt werden (die Anmeldeseite, z. B.). Implementieren Sie zum Angeben von Standardinhalt für ein Steuerelement ContentPlaceHolder zu veranschaulichen, nehmen wir das Szenario, das soeben erläuterten.

> [!NOTE]
> Im weiteren Verlauf dieses Lernprogramms aktualisiert unsere Website, um eine Schnittstelle für die Anmeldung in der linken Spalte für alle Seiten, sondern die Anmeldeseite eingeschlossen werden sollen. In diesem Lernprogramm wird jedoch nicht zum Konfigurieren der Website zur Unterstützung von Benutzerkonten untersuchen. Weitere Informationen zu diesem Thema finden Sie in meinem [formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) Lernprogramme.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Hinzufügen einer ContentPlaceHolder und seine Standardinhalt angeben

Öffnen der `Site.master` Masterseite und fügen Sie das folgende Markup in die linke Spalte zwischen die `DateDisplay` Abschnitt Bezeichnung und Lektionen:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Nach dem Hinzufügen dieses Markup sollte die Masterseite Entwurfsansicht Abbildung 6 ähneln.


[![Die Seite "Master" enthält ein Anmeldesteuerelement](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Abbildung 06**: die Seite "Master" enthält ein Login-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Diese ContentPlaceHolder `QuickLoginUI`, hat ein Websteuerelement für Anmeldung als Standardinhalt. Das Steuerelement für die Anmeldung wird eine Benutzeroberfläche, die der Benutzer ihren Benutzernamen und ein gültiges Kennwort sowie eine Schaltfläche "Anmelden" aufgefordert wird, angezeigt. Wenn Sie auf die Schaltfläche "Anmelden", überprüft das Steuerelement für die Anmeldung intern die Anmeldeinformationen des Benutzers für die Mitgliedschaft-API. Um dieses Steuerelement für die Anmeldung in der Praxis verwenden zu können, müssen Sie dann Ihr Standort für die Verwendung von Mitgliedschaft zu konfigurieren. In diesem Thema ist nicht Gegenstand dieses Lernprogramm; Verweisen auf meine [formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) Lernprogramme für Weitere Informationen zum Erstellen einer Webanwendung, die Benutzerkonten unterstützt.

Wahlweise können Sie die Verhalten oder die Darstellung des Steuerelements für die Anmeldung anzupassen. Ich habe zwei Eigenschaften festgelegt: `TitleText` und `FailureAction`. Die `TitleText` Eigenschaftenwert, der Standardeinstellungen auf "Anmelden" wird am oberen Rand des Steuerelements-Benutzeroberfläche angezeigt. Ich, damit sie den Text "Anmelden" als zeigt diese Eigenschaft festgelegt haben eine `<h3>` Element. Die `FailureAction` Eigenschaft gibt an, was zu tun, wenn der Benutzer Anmeldeinformationen ungültig sind. Wird standardmäßig auf den Wert `Refresh`, belässt die Benutzer auf derselben Seite und zeigt eine Fehlermeldung in das Steuerelement für die Anmeldung an. Ich es geändert haben `RedirectToLoginPage`, der den Benutzer auf die Anmeldeseite im Falle ungültige Anmeldeinformationen sendet. Ich möchte den Benutzer auf die Anmeldeseite zu senden, wenn ein Benutzer versucht bei der Anmeldung von einigen anderen Seite, aber ein Fehler auftritt, da die Anmeldeseite enthalten kann, zusätzliche Anweisungen und Optionen, die nicht in der linken Spalte passen würde. Beispielsweise enthalten unter Umständen Anmeldungsseite abzurufenden ein Kennwort vergessen haben oder ein neues Konto zu erstellen.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Erstellen die Seite "Login", und überschreiben den Standardinhalt

Im nächsten Schritt werden mit der Masterseite abgeschlossen die Anmeldeseite zu erstellen. Hinzufügen eine ASP.NET-Seite mit dem Namen Ihrer Website-Stammverzeichnis `Login.aspx`, binden an die `Site.master` Masterseite. Auf diese Weise wird eine Seite mit vier Content-Steuerelemente erstellen, die jeweils einen für die ContentPlaceHolders in definierten `Site.master`.

Hinzufügen eines Steuerelements Login, um die `MainContent` Inhaltssteuerelement. Ebenso wünschen, Hinzufügen von Inhalt der `LeftColumnContent` Region. Allerdings stellen Sie sicher, lassen Sie das Inhaltssteuerelement für die `QuickLoginUI` ContentPlaceHolder leer. Dies wird sicherstellen, dass die Anmeldung Steuerelement nicht in der linken Spalte der Anmeldeseite angezeigt wird.

Nach dem Definieren des Inhalts für die `MainContent` und `LeftColumnContent` Regionen, Ihrer Anmeldeseite deklarativem Markup sollte ähnlich der folgenden aussehen:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Abbildung 7 zeigt diese Seite, wenn Sie über einen Browser angezeigt. Da auf dieser Seite ein Steuerelement für gibt die `QuickLoginUI` ContentPlaceHolder, dabei den Standardinhalt der Masterseite angegeben wird. Im Endeffekt ist, dass das Steuerelement für die Anmeldung im Entwurf der Masterseite angezeigt Ansicht (siehe Abbildung 6) nicht auf dieser Seite gerendert wird.


[![Die Anmeldeseite Represses Standardinhalt der QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Abbildung 07**: die Anmeldung Seite Represses der `QuickLoginUI` ContentPlaceHolders Standard Inhalt ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Verwenden den Standardinhalt in neue Seiten

Wir möchten das Steuerelement für die Anmeldung in der linken Spalte für alle Seiten mit Ausnahme der Anmeldeseite anzeigen. Um dies zu erreichen, sollten alle Inhaltsseiten mit Ausnahme der Anmeldeseite ein Inhaltssteuerelement für weglassen der `QuickLoginUI` ContentPlaceHolder. Durch das Auslassen eines Inhaltssteuerelements, wird stattdessen der ContentPlaceHolder Standardinhalt verwendet werden.

Unsere vorhandenen Inhaltsseiten - `Default.aspx`, `About.aspx`, und `MultipleContentPlaceHolders.aspx` -verwenden Sie ein Inhaltssteuerelement für keine `QuickLoginUI` , da sie erstellt wurden, bevor wir das entsprechende Steuerelement ContentPlaceHolder Masterseite hinzugefügt. Daher müssen diese vorhandenen Seiten nicht aktualisiert werden. Neue Seiten, die zu der Website hinzugefügt enthalten jedoch ein Steuerelement für die `QuickLoginUI` ContentPlaceHolder, standardmäßig. Daher Denken Sie daran, diese zu entfernen, müssen jedes Mal, die wir eine neue Seite hinzufügen (es sei denn, wir die ContentPlaceHolder Standardinhalt, außer Kraft setzen im Fall der Anmeldeseite möchten) für Inhaltssteuerelemente.

Um das Inhaltssteuerelement zu entfernen, Sie können entweder manuell löschen Sie seine deklarative Markup aus der Datenquellensicht an oder wählen Sie in der Entwurfsansicht den Standardwert des Masters Inhaltslink aus seinem Smarttag. Beide Vorgehensweisen entfernt das Inhaltssteuerelement aus der Seite "und erzeugt dieselbe Auswirkung net.

Abbildung 8 zeigt `Default.aspx` über einen Browser angezeigt. Bedenken Sie, dass `Default.aspx` hat nur zwei Inhaltssteuerelemente in seiner deklarativem Markup - eines für angegebene `head` und eine für `MainContent`. Daher ist die Standardeinstellung für die `LeftColumnContent` und `QuickLoginUI` ContentPlaceHolders werden angezeigt.


[![Der Standard Inhalte für die LeftColumnContent und QuickLoginUI ContentPlaceHolders werden angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Abbildung 08**: The Default Inhalte für die `LeftColumnContent` und `QuickLoginUI` ContentPlaceHolders werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Zusammenfassung

Das ASP.NET Masterseite ermöglicht für eine beliebige Anzahl von ContentPlaceHolders in die Masterseite. Was mehr ist, enthalten ContentPlaceHolders Standardinhalt, die in die Groß-/Kleinschreibung ausgegeben wird, ergibt sich keine entsprechende Inhaltssteuerelement in der Seite Inhalt. In diesem Lernprogramm haben wir gesehen zum zusätzlichen ContentPlaceHolder-Steuerelemente in die Masterseite einschließen und definieren Sie Inhaltssteuerelemente für diese neue ContentPlaceHolders in neuen und vorhandenen ASP.NET-Seiten. Wir haben uns auch an Standardeinstellung angeben von Inhalten in einem ContentPlaceHolder eignet sich in Szenarien, in denen nur eine Minderheit der Seiten-Anforderungen, die andernfalls anpassen Inhalt innerhalb einer bestimmten Region standardisiert.

In den nächsten Lernprogrammen untersucht die `head` ContentPlaceHolder finden Sie detaillierte Informationen angezeigt, wie deklarativ und programmgesteuert über den Titel, Meta-Tags und andere HTML-Header auf Grundlage von Seite definiert.

Viel Spaß beim Programmieren!

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Suchi Banerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Zurück](creating-a-site-wide-layout-using-master-pages-vb.md)
[Weiter](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
