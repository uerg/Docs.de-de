---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Mehrere ContentPlaceHolder-Steuerelemente und Standardinhalt (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Untersucht, wie eine Masterseite mehrere Content Platzhalter hinzugefügt sowie Standardinhalt die Content Platzhalter angeben.
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: 98cb78e03f9f7aff4a36625416188aba04512f6a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839699"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Mehrere ContentPlaceHolder-Steuerelemente und Standardinhalt (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Untersucht, wie eine Masterseite mehrere Content Platzhalter hinzugefügt sowie Standardinhalt die Content Platzhalter angeben.


## <a name="introduction"></a>Einführung

Im vorherigen Tutorial wurde beschrieben, wie Masterseiten aktivieren ASP.NET-Entwicklern ein einheitliches Layout für die gesamte Website zu erstellen. Masterseiten definieren Markup, das in allen die Inhaltsseiten und Regionen, die pro Seite von Seite angepasst werden. Im vorherigen Tutorial erstellt es eine einfache Masterseite (`Site.master`) und zwei Inhaltsseiten (`Default.aspx` und `About.aspx`). Unsere Masterseite Bestand aus zwei ContentPlaceHolder-Steuerelemente, die mit dem Namen `head` und `MainContent`, wurden die befindet, der `<head>` Element und das Web Form bzw. Während die Inhaltsseiten jedes zwei Inhaltssteuerelemente hatten, nur angegebene Markup für den Endpunkt für `MainContent`.

Wie durch die beiden ContentPlaceHolder-Steuerelemente in belegt `Site.master`, eine Masterseite möglicherweise mehrere ContentPlaceHolder-Steuerelemente enthalten. Die Masterseite kann, Standard-Markup für die ContentPlaceHolder-Steuerelemente angeben. Eine Inhaltsseite, kann dann optional ein eigenes Markup angeben oder das Standardmarkup. In diesem Tutorial haben wir sehen Sie sich mit mehreren ContentControl-Elemente auf der Masterseite und Informationen zum Standard-Markup in die ContentPlaceHolder-Steuerelemente zu definieren.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Schritt 1: Hinzufügen von zusätzlichen ContentPlaceHolder-Steuerelemente auf der Masterseite

Viele Entwürfe der Website enthält mehrere Bereiche auf dem Bildschirm, die pro Seite von Seite angepasst werden. `Site.master`, die Masterseite, die wir im vorherigen Tutorial erstellt haben, enthält ein einzelnes ContentPlaceHolder-Objekt in das Webformular mit dem Namen `MainContent`. Insbesondere diese ContentPlaceHolder befindet sich in der `mainContent` `<div>` Element.

Abbildung 1 zeigt `Default.aspx` über einen Browser angezeigt. Die Region, die rot markiert ist, die seitenspezifische-Markup für `MainContent`.


[![Der Eingekreiste Bereich zeigt den Bereich pro Seite von Seite derzeit anpassbare](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Abbildung 01**: die Region mit Kreis zeigt der Bereich als derzeit anpassbare pro Seite von Seite ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Stellen Sie sich vor, dass neben der Region, die in Abbildung 1 dargestellt, wir auch seitenspezifische-Elemente auf der linken Spalte unter die Lektionen und Nachrichten hinzuzufügen müssen Abschnitte. Zu diesem Zweck fügen wir ein weiteres ContentPlaceHolder-Steuerelement hinzu, auf die Masterseite. Um folgen zu können, öffnen Sie die `Site.master` Masterseite in Visual Web Developer, und ziehen Sie dann ein ContentPlaceHolder-Steuerelement aus der Toolbox in den Designer nach dem Abschnitt "News". Legen Sie die ContentPlaceHolder `ID` zu `LeftColumnContent`.


[![Ein ContentPlaceHolder-Steuerelement auf der linken Spalte der Masterseite hinzufügen](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Abbildung 02**: ein ContentPlaceHolder-Steuerelement auf der Masterseite linke Spalte hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Durch das Hinzufügen der `LeftColumnContent` ContentPlaceHolder der Masterseite, wir können definieren, Inhalt für diese Region pro Seite von Seite indem einen Inhalt Steuerelemente auf der Seite, deren `ContentPlaceHolderID` nastaven NA hodnotu `LeftColumnContent`. Untersuchen wir diesen Prozess in Schritt2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Schritt 2: Definieren von Inhalt für die neue ContentPlaceHolder in die Inhaltsseiten

Wenn Sie eine neue Seite auf der Website hinzufügen, erstellt Visual Web Developer automatisch einen Inhalt Steuerelement auf der Seite für die einzelnen ContentPlaceHolder in der ausgewählten Masterseite. Hinzugefügt müssen eine der `LeftColumnContent` ContentPlaceHolder unserer master-Seite in Schritt 1, neue ASP.NET Pages wird jetzt haben Sie drei ContentControl-Elemente.

Um dies zu veranschaulichen, fügen Sie eine neue Seite in das Stammverzeichnis, das mit dem Namen `MultipleContentPlaceHolders.aspx` , gebunden ist, um die `Site.master` Masterseite. Mit dem folgenden deklaratives Markup, erstellt Visual Web Developer auf dieser Seite:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Geben Sie einige Inhalte in das Inhaltssteuerelement verweisen auf die `MainContent` ContentPlaceHolder-Steuerelemente (`Content2`). Fügen Sie das folgende Markup zu der `Content3` Inhaltssteuerelement (das auf die `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Nach dem Hinzufügen dieses Markup, finden Sie auf der Seite über einen Browser. Wie in Abbildung 3 gezeigt, wird das Markup in platziert die `Content3` Inhaltssteuerelement wird angezeigt, in der linken Spalte unter dem Abschnitt "News" (rot markiert). Das Markup in platziert `Content2` wird rechts auf der Seite (blau markiert) angezeigt.


[![Die linke Spalte enthält jetzt seitenspezifische Inhalte unter dem Abschnitt "News"](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Abbildung 03**: die linke Spalte jetzt enthält seitenspezifische Inhalt unter der Abschnitt "News" ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definieren von Inhalt in bestehenden Inhaltsseiten

Erstellen eine neue Seite automatisch umfasst das ContentPlaceHolder-Steuerelement, das in Schritt 1 hinzugefügt wurde. Aber unsere zwei vorhandenen Inhaltsseiten - `About.aspx` und `Default.aspx` -ein ContentControl-Element für keinen der `LeftColumnContent` ContentPlaceHolder. Zum Angeben von Inhalt für diese ContentPlaceHolder in diese beiden vorhandenen Seiten müssen wir ein Inhaltssteuerelement selbst hinzufügen.

Im Gegensatz zu den meisten Steuerelementen für ASP.NET Web schließt Visual Web Developer-Toolbox kein Inhaltssteuerelement-Element. Wir können manuell in das Inhaltssteuerelement deklarativem Markup eingeben, in der Datenquellensicht an, aber ein schneller und einfacher Ansatz ist die Verwendung von der Entwurfsansicht. Öffnen der `About.aspx` Seite, und wechseln Sie zur Entwurfsansicht. Wie in Abbildung 4 dargestellt, die `LeftColumnContent` ContentPlaceHolder wird in der Entwurfsansicht angezeigt, wenn Sie den Mauszeiger darüber, liest des angezeigte Titels: "LeftColumnContent (Master)". Die Aufnahme der "Master" im Titel Gibt an, dass keine Inhaltssteuerelement, das auf der Seite für diese ContentPlaceHolder definiert. Wenn ein ContentControl-Element, für die ContentPlaceHolder, wie im Fall für vorhanden `MainContent`, der Titel wird gelesen: "*ContentPlaceHolderID* (Benutzerdefiniert)."

Hinzufügen ein Content-Steuerelements für die `LeftColumnContent` ContentPlaceHolder zu `About.aspx`, erweitern Sie ContentPlaceHolders-Smarttag, und klicken Sie auf den Link erstellen Sie benutzerdefinierte Content.


[![Die Entwurfsansicht für About.aspx zeigt die LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Abbildung 04**: die Entwurfsansicht für `About.aspx` zeigt die `LeftColumnContent` ContentPlaceHolder ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Auf den Hyperlink benutzerdefinierten Inhalt erstellen, generiert die erforderlichen Inhaltssteuerelement auf der Seite und legt seine `ContentPlaceHolderID` Eigenschaft, um die ContentPlaceHolder `ID`. Klicken Sie beispielsweise den Link, um benutzerdefinierten Inhalt erstellen `LeftColumnContent` -Region in `About.aspx` folgende deklarative Markup auf der Seite hinzugefügt:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>Das Auslassen von Inhaltssteuerelementen

ASP.NET erforderlich nicht, dass alle Inhaltsseiten für jede in der Masterseite definierte ContentPlaceHolder Inhaltssteuerelemente enthalten sind. Wenn ein ContentControl-Element ausgelassen wird, verwendet das ASP.NET-Modul das Markup in der ContentPlaceHolder auf der Masterseite definiert. Dieses Markup wird als die ContentPlaceHolder bezeichnet *Inhalt standardmäßig* und ist nützlich in Szenarien, in dem der Inhalt für eine Region für die meisten Seiten, üblich ist, muss jedoch für eine kleine Anzahl von Seiten angepasst werden. Schritt 3 untersucht die Angabe der Standardinhalt der Masterseite.

Derzeit `Default.aspx` enthält zwei Inhaltssteuerelemente für die `head` und `MainContent` ContentPlaceHolder-Steuerelemente, er verfügt nicht über ein ContentControl-Element für `LeftColumnContent`. Daher, wenn `Default.aspx` wird gerendert, die `LeftColumnContent` ContentPlaceHolder Standardinhalt wird verwendet. Da wir noch standardmäßigen Inhalt für diese ContentPlaceHolder definieren müssen, ist das Endergebnis, dass kein Markup für diese Region ausgegeben wird. Um dieses Verhalten zu überprüfen, finden Sie unter `Default.aspx` über einen Browser. Wie in Abbildung 5 gezeigt, wird kein Markup in der linken Spalte unter dem Abschnitt "News" ausgegeben.


[![Kein Inhalt ist für die LeftColumnContent ContentPlaceHolder gerendert.](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Abbildung 05**: kein Inhalt für gerendert wird die `LeftColumnContent` ContentPlaceHolder ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Schritt 3: Angeben der standardmäßigen Inhalt auf der Masterseite

Einige Websitedesigns gehören eine Region an, deren Inhalt für alle Seiten der Website, abgesehen von ein oder zwei Ausnahmen identisch ist. Erwägen Sie eine Website, die Benutzerkonten unterstützt. Ein solcher Standort erfordert eine Anmeldeseite, in denen Besucher ihre Anmeldeinformationen zum Signieren der Website eingegeben werden können. Um den Anmeldevorgang zu beschleunigen, können die Website-Designer Benutzername und Kennwort Textfelder in der oberen linken Ecke jeder Seite können Benutzer sich anmelden, ohne explizit für die Anmeldeseite finden Sie unter enthalten. Diese Textfelder Benutzername und Kennwort in den meisten Seiten hilfreich sind, sind redundante in die Anmeldeseite, in die Textfelder für die Anmeldeinformationen des Benutzers bereits enthält.

Um diesen Entwurf zu implementieren, können Sie ein ContentPlaceHolder-Steuerelement in der oberen linken Ecke der Masterseite erstellen. Jede Seite, die erforderlich, um die Textfelder "Benutzername und Kennwort" ein in der oberen linken Ecke angezeigt würde erstellen ein ContentControl-Element für diese ContentPlaceHolder und fügen die erforderliche Schnittstelle. Die Anmeldeseite, auf der anderen Seite würde entweder ein ContentControl-Element für dieses ContentPlaceHolder hinzufügen lassen, oder erstellen Sie würden einen Inhalt Steuerelement kein Markup definiert. Der Nachteil dieses Ansatzes ist, denken Sie daran, die Textfelder "Benutzername und Kennwort" ein zu jeder Seite hinzuzufügen, die an den Standort (mit Ausnahme der Anmeldeseite) hinzugefügt werden müssen. Dies ist zwangsläufig zu Problemen. Wir wahrscheinlich vergessen, die diese Textfelder auf einer Seite oder zwei hinzuzufügen oder, schlimmer noch, wir möglicherweise implementiert die Schnittstelle nicht ordnungsgemäß (möglicherweise nur ein Textfeld anstelle von zwei hinzufügen).

Eine bessere Lösung ist die Textfelder "Benutzername und Kennwort" ein als ContentPlaceHolders-Standardinhalt definieren. Auf diese Weise müssen wir nur dieses Standardinhalt in diese Seiten zu überschreiben, die nicht die Textfelder "Benutzername und Kennwort" ein angezeigt (die Anmeldeseite, zum Beispiel). Zum Angeben von Standardinhalt für ein ContentPlaceHolder-Steuerelement zu veranschaulichen, die gerade beschriebenen Szenarios implementieren.

> [!NOTE]
> Der Rest dieses Tutorials aktualisiert unsere Website, um eine Schnittstelle für die Anmeldung in der linken Spalte für alle Seiten, sondern die Anmeldeseite enthalten. In diesem Tutorial untersucht jedoch nicht die Website zur Unterstützung von Benutzerkonten zu konfigurieren. Weitere Informationen zu diesem Thema finden Sie in meinem [Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) Tutorials.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Ein ContentPlaceHolder-Objekt und geben Sie den Standardinhalt

Öffnen der `Site.master` Masterseite, und fügen Sie das folgende Markup auf der linken Spalte zwischen die `DateDisplay` Abschnitt Bezeichnung und Lektionen:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Nach dem Hinzufügen dieses Markup sollte die Entwurfsansicht für die Masterseite in Abbildung 6 ähneln.


[![Die Master-Seite enthält ein Anmeldesteuerelement](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Abbildung 06**: die Masterseite enthält ein Anmeldesteuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Diese ContentPlaceHolder, `QuickLoginUI`, verfügt über ein Login-Steuerelement als Inhalt standardmäßig. Das Anmeldesteuerelement zeigt eine Benutzeroberfläche, die der Benutzer seinen Benutzernamen und Kennwort sowie eine Schaltfläche "Anmelden" auffordert. Nach dem Klicken auf die Schaltfläche "Anmelden", überprüft das Anmeldesteuerelement intern die Anmeldeinformationen des Benutzers anhand der Membership-API. Um dieses Steuerelement für die Anmeldung in der Praxis verwenden zu können, müssen Sie Sie dann Ihren Standort Gruppenmitgliedschaft konfigurieren. In diesem Thema ist nicht Gegenstand dieses Lernprogramms; finden Sie in meinem [Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) Lernprogramme für Weitere Informationen zum Erstellen einer Web-Anwendung, die Benutzerkonten unterstützt.

Des Anmeldesteuerelement Verhalten oder die Darstellung anpassen können. Ich habe zwei seiner Eigenschaften eingerichtet: `TitleText` und `FailureAction`. Die `TitleText` Eigenschaftswert, der Standardeinstellungen auf "Log In" wird am oberen Rand des Steuerelements-Benutzeroberfläche angezeigt. Ich habe diese Eigenschaft eingerichtet werden, damit es den Text "Sign In" als anzeigt ein `<h3>` Element. Die `FailureAction` Eigenschaft gibt an, was zu tun ist, wenn die Anmeldeinformationen des Benutzers ungültig sind. Wird standardmäßig der Wert `Refresh`, belässt den Benutzer auf derselben Seite und zeigt eine Fehlermeldung, in dem Login-Steuerelement. Ich habe es geändert `RedirectToLoginPage`, der den Benutzer auf die Anmeldeseite im Fall von ungültigen Anmeldeinformationen sendet. Ich ziehe den Benutzer zur Anmeldeseite senden, wenn ein Benutzer versucht, für die Anmeldung in eine andere Seite jedoch ein Fehler auftritt, da die Anmeldeseite enthalten kann, zusätzliche Anweisungen und Optionen, die nicht einfach in der linken Spalte passt. Beispielsweise enthalten unter Umständen die Anmeldeseite um ein vergessenes Kennwort abzurufen oder ein neues Konto erstellen.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Erstellen die Anmeldeseite, und überschreiben den standardmäßigen Inhalt

Im nächsten Schritt werden mit die Masterseite abgeschlossen die Anmeldeseite erstellen. Hinzufügen eine ASP.NET-Seite mit dem Namen Ihrer Website-Stammverzeichnis `Login.aspx`, binden an die `Site.master` Masterseite. Auf diese Weise wird eine Seite mit vier Content-Steuerelementen erstellt, eine für jede der ContentPlaceHolder-Steuerelemente in definierten `Site.master`.

Fügen Sie ein Anmeldesteuerelement, um die `MainContent` Inhaltssteuerelement. Ebenso können Sie Inhalte zum Hinzufügen der `LeftColumnContent` Region. Allerdings stellen Sie sicher, dass das Steuerelement für die `QuickLoginUI` ContentPlaceHolder leer. Dadurch wird sichergestellt, dass die Anmeldung Steuerelement nicht in der linken Spalte der Anmeldeseite angezeigt wird.

Nach dem Definieren des Inhalts für die `MainContent` und `LeftColumnContent` Regionen deklaratives Markup für die Anmeldeseite sollte etwa wie folgt aussehen:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Abbildung 7 zeigt diese Seite, wenn Sie über einen Browser angezeigt. Da diese Seite gibt an, ein ContentControl-Element für die `QuickLoginUI` ContentPlaceHolder, er überschreibt den standardmäßigen Inhalt der Masterseite angegeben. Das Endergebnis ist, dass die Login-Steuerelement in die Masterseite Design angezeigt, die Ansicht (siehe Abbildung 6) nicht auf dieser Seite gerendert wird.


[![Die Anmeldeseite Represses Standardinhalt der QuickLoginUI ContentPlaceHolder des](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Abbildung 07**: die Anmeldung Seite Represses der `QuickLoginUI` ContentPlaceHolder Standard Content ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Verwenden den standardmäßigen Inhalt in neue Seiten

Wir möchten das Steuerelement für die Anmeldung in der linken Spalte für alle Seiten mit Ausnahme der Anmeldeseite angezeigt. Um dies zu erreichen, sollten alle Inhaltsseiten mit Ausnahme der Anmeldeseite ein ContentControl-Element für weglassen der `QuickLoginUI` ContentPlaceHolder. Durch Auslassen eines Inhaltssteuerelements, wird stattdessen ContentPlaceHolders-Standardinhalt verwendet werden.

Unsere bestehenden Inhaltsseiten - `Default.aspx`, `About.aspx`, und `MultipleContentPlaceHolders.aspx` -verwenden Sie ein ContentControl-Element für die keine `QuickLoginUI` weil sie erstellt wurden, bevor wir die Masterseite, ContentPlaceHolder-Steuerelement hinzugefügt. Daher müssen diese vorhandenen Seiten nicht aktualisiert werden. Allerdings enthalten neue Seiten, die auf der Website hinzugefügt, ein ContentControl-Element für die `QuickLoginUI` ContentPlaceHolder, in der Standardeinstellung. Aus diesem Grund müssen wir denken Sie daran, entfernen Sie diese Inhaltssteuerelemente jedes Mal, die wir fügen eine neue Seite hinzu, (es sei denn, wir ContentPlaceHolders-Standardinhalt, außer Kraft setzen wie bei der Anmeldeseite möchten).

Um das Steuerelement zu entfernen, Sie können entweder manuell löschen der deklarative Markup aus der Datenquellensicht an oder wählen Sie in die Entwurfsansicht zu sehen, die standardmäßig auf die Master Inhaltslink aus sein Smarttag. Beide Vorgehensweisen entfernt das Steuerelement der Seite auf, und erzeugt, die gleich net wirksam.

Abbildung 8 zeigt `Default.aspx` über einen Browser angezeigt. Zur Erinnerung: `Default.aspx` hat nur zwei Inhaltssteuerelemente in seiner deklarativen Markup - eines für angegebene `head` und eine für `MainContent`. Als Ergebnis wird der Standardwert für die `LeftColumnContent` und `QuickLoginUI` ContentPlaceHolder-Steuerelemente werden angezeigt.


[![Der standardmäßige Inhalt für die LeftColumnContent und QuickLoginUI ContentPlaceHolders werden angezeigt](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Abbildung 08**: der standardmäßige Inhalt der `LeftColumnContent` und `QuickLoginUI` ContentPlaceHolder-Steuerelemente werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Zusammenfassung

Eine beliebige Anzahl von ContentPlaceHolder-Steuerelemente auf der Masterseite ermöglicht die Masterseite ASP.NET-Modell. Ist ContentPlaceHolder-Steuerelemente enthalten Standardinhalt, der im Fall ausgegeben wird, dass es kein entsprechendes ist Inhaltssteuerelement in der Seite Inhalt. In diesem Tutorial haben wir gesehen wie zusätzliche ContentPlaceHolder-Steuerelemente in die Masterseite einschließen und definieren Sie ContentControl-Elemente für diese neue ContentPlaceHolder-Steuerelemente in neue und vorhandene ASP.NET-Seiten. Wir haben uns auch an das Angeben von Standardwerten Inhalte in ein ContentPlaceHolder-Objekt, was nützlich in Szenarien, in denen nur eine Minderheit der Seiten anpassen, die andernfalls muss die Inhalte in einer bestimmten Region standardisiert, ist.

Im nächsten Tutorial untersucht die `head` ContentPlaceHolder im Detail, wie deklaratives und Programmgesteuertes den Titel, Meta-Tags und anderer HTML-Header pro Seite von Seite zu definieren.

Viel Spaß beim Programmieren!

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Suchi Banerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Weiter](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
