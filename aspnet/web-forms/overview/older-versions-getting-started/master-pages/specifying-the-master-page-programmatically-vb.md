---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Programmgesteuertes Festlegen der Gestaltungsvorlage (. VB) | Microsoft Docs
author: rick-anderson
description: "Untersucht die Masterseite der Inhaltsseite, programmgesteuert über den Ereignishandler PreInit festlegen."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 090d0777b9d541003c3115d0da7cd974820c2939
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="specifying-the-master-page-programmatically-vb"></a>Programmgesteuertes Festlegen der Gestaltungsvorlage (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Untersucht die Masterseite der Inhaltsseite, programmgesteuert über den Ereignishandler PreInit festlegen.


## <a name="introduction"></a>Einführung

Seit dem ersten Beispiel in [ *erstellen eine standortweite Layout mithilfe von Master Pages*](creating-a-site-wide-layout-using-master-pages-vb.md), werden alle Inhalte Seiten ihre Masterseite deklarativ über verwiesen haben die `MasterPageFile` -Attribut in der `@Page`Richtlinie. Beispielsweise die folgenden `@Page` Richtlinie links die Seite Inhalte der Masterseite `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

Die [ `Page` Klasse](https://msdn.microsoft.com/en-us/library/system.web.ui.page.aspx) in der `System.Web.UI` Namespace beinhaltet eine [ `MasterPageFile` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.page.masterpagefile.aspx) , die gibt den Pfad zur Masterseite der Inhaltsseite; es ist diese Eigenschaft, die von der festgelegtist`@Page` Richtlinie. Diese Eigenschaft kann auch verwendet werden, Masterseite der Inhaltsseite programmgesteuert angegeben. Dieser Ansatz ist hilfreich, wenn Sie die Gestaltungsvorlage basierend auf externe Faktoren wie der Benutzer Zugriff auf die Seite dynamisch zuweisen möchten.

In diesem Lernprogramm werden eine zweite Masterseite auf unserer Website hinzufügen und die Masterseite zur Laufzeit dynamisch zu entscheiden.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Schritt 1: Einen Blick auf der Seite "-Lebenszyklus

Wenn eine Anforderung an den Webserver für eine ASP.NET-Seite, die eine Seite ist eingeht, muss das Modul für ASP.NET den Anzeigezustand der Seite Sicherung Content-Steuerelemente in der master-Seite des entsprechenden ContentPlaceHolder-Steuerelemente. Diese Fusion erstellt eine einziges Steuerelement-Hierarchie, die dann über den Seitenlebenszyklus typische fortgesetzt werden kann.

Abbildung 1 zeigt diese Fusion. Schritt 1 in Abbildung 1 zeigt die anfänglichen Inhalte und die Masterseite Steuerelement Hierarchien. Steuerelemente auf der Seite werden am Ende des Protokollfragments der PreInit-Phase der Inhalt der entsprechenden ContentPlaceHolders in die Masterseite (Schritt2) hinzugefügt. Nach dieser Fusion dient die Gestaltungsvorlage als Stamm der Steuerelementhierarchie mit Sicherung aus. Dieses Steuerelement fused Hierarchie wird dann zum Erzeugen der abgeschlossene Steuerelementhierarchie (Schritt 3) auf der Seite hinzugefügt. Das Ergebnis ist, dass die Hierarchie der Seite mit Sicherung Steuerelementhierarchie enthält.


[![Der Gestaltungsvorlage und Inhaltsseites Steuerelement Hierarchien sind in der Phase PreInit Fused zusammen](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Abbildung 01**: der Gestaltungsvorlage und des Inhaltsseite Steuerelement Hierarchien werden zusammen Fused-Phase der PreInit ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Schritt 2: Festlegen der`MasterPageFile`-Eigenschaft im Code

Welche Gestaltungsvorlage in diese Fusion partakes hängt vom Wert von der `Page` des Objekts `MasterPageFile` Eigenschaft. Festlegen der `MasterPageFile` Attribut in der `@Page` Richtlinie wurde im Endeffekt Zuweisen der `Page`des `MasterPageFile` Eigenschaft während der Initialisierungsphase, die die erste Phase des Lebenszyklus der Seite ist. Wir können diese Eigenschaft auch programmgesteuert festlegen. Allerdings ist es obligatorisch, dass diese Eigenschaft festgelegt werden, bevor die Fusion in Abbildung 1 stattfindet.

Am Anfang der PreInit-Phase der `Page` -Objekt löst die [ `PreInit` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.page.preinit.aspx) und ruft seine [ `OnPreInit` Methode](https://msdn.microsoft.com/en-us/library/system.web.ui.page.onpreinit.aspx). Um die Gestaltungsvorlage programmgesteuert festzulegen, erstellen Sie dann wir können entweder einen Ereignishandler für das `PreInit` Ereignis oder Überschreibung der `OnPreInit` Methode. Betrachten Sie beide Ansätze aus.

Öffnen Sie zunächst `Default.aspx.vb`, die Code-Behind-Klassendatei für unsere Website-Homepage. Fügen Sie einen Ereignishandler für der Seite `PreInit` Ereignis, indem Sie in den folgenden Code eingeben:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Hier legen wir den `MasterPageFile` Eigenschaft. Aktualisieren Sie den Code, sodass den Wert weist "~ / Site.master" auf die `MasterPageFile` Eigenschaft.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Wenn Sie einen Haltepunkt festlegen und das Starten mit Debuggen Sie, dass sehen immer die `Default.aspx` besuchte Webseite oder bei jedem besteht ein Postback auf dieser Seite die `Page_PreInit` -Ereignishandler ausgeführt wird und die `MasterPageFile` Eigenschaft zugewiesen ist "~ / Site.master".

Alternativ können Sie überschreiben die `Page` Klasse `OnPreInit` -Methode, und legen die `MasterPageFile` Eigenschaft vorhanden. In diesem Beispiel nehmen wir nicht festgelegt die Gestaltungsvorlage in einer bestimmten Seite, sondern aus `BasePage`. Beachten Sie, dass wir eine benutzerdefinierte Basisseite-Klasse erstellt (`BasePage`) zurück in die [ *Titel, Meta-Tags und andere HTML-Header angeben, der Gestaltungsvorlage* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) Lernprogramm. Derzeit `BasePage` überschreibt die `Page` Klasse `OnLoadComplete` -Methode, in denen der Seite wird `Title` -Eigenschaft basierend auf der Siteübersichtsdaten. Wir aktualisieren `BasePage` auch überschreiben die `OnPreInit` Methode, um die Gestaltungsvorlage programmgesteuert anzugeben.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Da unsere Inhaltsseiten Ableitung `BasePage`, alle von ihnen jetzt ihre Masterseite programmgesteuert zugewiesen haben. An diesem Punkt der `PreInit` -Ereignishandler in `Default.aspx.vb` überflüssig; wünschen, um ihn zu entfernen.

### <a name="what-about-thepagedirective"></a>Wie sieht die`@Page`Richtlinie?

Was etwas verwirrend sein kann, die die Inhaltsseiten ist `MasterPageFile` Eigenschaften sind jetzt an zwei Stellen angegeben: programmgesteuert in die `BasePage` -Klasse `OnPreInit` Methode sowie über die `MasterPageFile` Attribut in jeder Inhaltsseite `@Page` Richtlinie.

Die erste Phase im Lebenszyklus Seite ist die Initialisierungsphase. In dieser Phase der `Page` des Objekts `MasterPageFile` der Wert der Eigenschaft zugewiesen der `MasterPageFile` Attribut in der `@Page` Richtlinie (Wenn diese Option angegeben wird). Die PreInit-Phase folgt der Initialisierungsphase und es ist hier, in denen es programmgesteuert festgelegt die `Page` des Objekts `MasterPageFile` -Eigenschaft, und überschreiben den Wert zugewiesen wird, aus der `@Page` Richtlinie. Da wir Festlegen der `Page` des Objekts `MasterPageFile` Eigenschaft programmgesteuert wir konnte Entfernen der `MasterPageFile` -Attribut aus der `@Page` ohne Auswirkung auf die Benutzeroberfläche zur Richtlinie. Um sich davon überzeugen möchten, fahren Sie fort, und entfernen Sie die `MasterPageFile` -Attribut aus der `@Page` -Direktive in `Default.aspx` und besuchen dann die Seite über einen Browser. Wie zu erwarten, ist die Ausgabe identisch, bevor Sie das Attribut entfernt wurde.

Ob die `MasterPageFile` Eigenschaftensatz wird über die `@Page` Richtlinie oder programmgesteuert wird der Endbenutzer-Erfahrung nicht relevant. Allerdings die `MasterPageFile` Attribut in der `@Page` Richtlinie wird während der Entwurfszeit von Visual Studio verwendet, um die WYSIWYG erzeugen Ansicht im Designer. Wenn Sie zum zurückkehren `Default.aspx` in Visual Studio, und navigieren Sie in den Designer sehen Sie die Meldung "Gestaltungsvorlage Fehler: die Seite verfügt über Steuerelemente, die einen Verweis für die Gestaltungsvorlage erfordern, jedoch keine angegeben" (siehe Abbildung 2).

Kurz gesagt, müssen Sie lassen die `MasterPageFile` Attribut in der `@Page` Richtlinie eine komfortable zur Entwurfszeit in Visual Studio nutzen.


[![Visual Studio nutzt das @Page Richtlinie MasterPageFile-Attribut zum Rendern der Entwurfsansicht](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Abbildung 02**: Visual Studio verwendet die `@Page` Richtlinie `MasterPageFile` -Attribut zum Rendern der Ansicht Entwurf ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Schritt 3: Erstellen einer alternativen Masterseite

Da eine Inhaltsseite Masterseite programmgesteuert zur Laufzeit festgelegt werden kann, es möglich ist, die eine bestimmte Masterseite basierend auf bestimmte externe Kriterien dynamisch geladen wird. Diese Funktionalität kann in Situationen nützlich sein, in denen die Website-Layout basierend auf den Benutzer variieren muss. Z. B. können eine Blog-Engine-Webanwendung ihre Benutzer zum Wählen ein Layout für ihre Blog, in dem jedes Layout, die eine andere master-Seite zugeordnet ist. Zur Laufzeit beim ein Besucher eines Benutzers Blog verwenden, anzeigen, wird müssen die Webanwendung im Blog Layout bestimmen, und ordnen Sie die entsprechenden Gestaltungsvorlage dynamisch Inhaltsseite.

Sehen wir uns wie eine Masterseite zur Laufzeit basierend auf bestimmte externe Kriterien dynamisch geladen. Unsere Website enthält derzeit nur eine Gestaltungsvorlage (`Site.master`). Wir benötigen eine andere Masterseite auswählen einer Masterseite zur Laufzeit veranschaulicht. Dieser Schritt konzentriert sich auf erstellen und konfigurieren die neue Masterseite. Schritt 4 prüft welche Masterseite zur Laufzeit zu bestimmen.

Erstellen eine neue Masterseite in den Stammordner, der mit dem Namen `Alternate.master`. Fügen Sie ein neues Stylesheet auch auf der Website mit dem Namen `AlternateStyles.css`.


[![Fügen Sie eine andere Gestaltungsvorlage und CSS-Datei wird in der Website](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Abbildung 03**: Hinzufügen einer anderen Gestaltungsvorlage und CSS-Datei auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image9.png))


Ich entworfen haben die `Alternate.master` Masterseite den am oberen Rand der Seite zentriert und in einem Hintergrundthread dunkelblaue angezeigten Titel haben. Ich habe Schiffsteil abgesehen von der linken Spalte und verschoben, dass der Inhalt unter der `MainContent` ContentPlaceHolder-Steuerelement, das sich jetzt die gesamte Breite der Seite. Darüber hinaus ich nixed ungeordnete Lektionen-Liste und ersetzt es mit einer horizontalen Liste oben `MainContent`. Ich habe aktualisiert auch die Schriftarten und Farben, die von der Gestaltungsvorlage (und durch Erweiterung auch dessen Inhaltsseiten) verwendet. Abbildung 4 zeigt `Default.aspx` bei Verwendung der `Alternate.master` Masterseite.

> [!NOTE]
> ASP.NET bietet die Möglichkeit zum Definieren *Designs*. Ein Design ist eine Auflistung der Bilder, CSS-Dateien und Formatvorlagen bezogenen Web Control eigenschaftseinstellungen, die auf einer Seite zur Laufzeit angewendet werden können. Designs sind die Möglichkeit, aufzurufen, wenn Ihre Website Layouts nur im angezeigten Bilder und durch ihre CSS-Regeln unterscheiden sich. Wenn Layouts mehr erheblich unterscheiden, z. B. unterschiedliche Websteuerelemente verwenden oder ein radikal anderes Layout müssen, müssen Sie separate Masterseiten verwenden. Erhalten Sie im Abschnitt Weitere Themen am Ende dieses Lernprogramms für Weitere Informationen zu Designs.


[![Unsere Inhaltsseiten können jetzt ein neues Aussehen und Verhalten verwenden.](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Abbildung 04**: unsere Inhaltsseiten können jetzt ein neues Aussehen und Verhalten ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image12.png))


Wenn der Master "und" Inhaltsseiten Markup abgesichert werden, die `MasterPage` -Klasse überprüft, um sicherzustellen, dass alle Inhalte Steuerelement auf der Inhaltsseite verweist auf eine ContentPlaceHolder auf der Masterseite. Eine Ausnahme wird ausgelöst, wenn ein Inhaltssteuerelement, das eine nicht existierende ContentPlaceHolder verweist gefunden wird. Anders ausgedrückt, ist es imperative darin, dass die Masterseite der Inhaltsseite zugewiesen wird eine ContentPlaceHolder für jede Inhaltssteuerelement in der Seite Inhalt.

Die `Site.master` Masterseite umfasst vier ContentPlaceHolder-Steuerelemente:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Inhaltsseiten auf unserer Website gehören nur ein oder zwei Inhaltssteuerelemente; ein Inhaltssteuerelement sind für jede der ContentPlaceHolders verfügbar. Wenn unsere neue Gestaltungsvorlage (`Alternate.master`) diese Inhaltsseiten, die für alle von der ContentPlaceHolders in Content-Steuerelemente verfügen je zugewiesen sein `Site.master` ist es wichtig, `Alternate.master` umfassen auch die gleichen Steuerelemente verwenden ContentPlaceHolder als `Site.master`.

Zum Abrufen Ihrer `Alternate.master` Masterseite erschließen (siehe Abbildung 4), starten, indem Sie definieren die Gestaltungsvorlage Formatvorlagen auf ähnelt der `AlternateStyles.css` Stylesheet. Fügen Sie die folgenden Regeln in `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

Als Nächstes fügen Sie das folgende deklarative Markup zum `Alternate.master`. Wie Sie sehen können, `Alternate.master` enthält vier ContentPlaceHolder-Steuerelemente mit dem gleichen `ID` Werte wie die ContentPlaceHolder-Steuerelemente in `Site.master`. Darüber hinaus enthält es ein ScriptManager-Steuerelement, der für die Seiten auf unserer Website erforderlich ist, die ASP.NET AJAX-Framework verwenden.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testen die neue Gestaltungsvorlage

So testen Sie dieses neue Masterseite Update der `BasePage` Klasse `OnPreInit` Methode, damit die `MasterPageFile` Eigenschaft der Wert zugewiesen `"~/Alternate.maser"` und besuchen dann die Website. Jeder Seite sollte ohne Fehler mit Ausnahme von zwei funktionsfähig sind: `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx`. Hinzufügen eines Produkts, DetailsView in `~/Admin/AddProduct.aspx` führt zu einem `NullReferenceException` aus der Zeile des Codes, der versucht, die Gestaltungsvorlage festgelegt `GridMessageText` Eigenschaft. Beim Zugriff auf `~/Admin/Products.aspx` ein `InvalidCastException` wird beim Laden der Seite mit der folgenden Meldung ausgelöst: "Cast-Objekt des Typs kann nicht ' ASP.alternate\_master" in den Typ "ASP.site\_master". "

Diese Fehler auftreten, da die `Site.master` Code-Behind-Klasse enthält öffentlichen Ereignisse, Eigenschaften und Methoden, die nicht in definiert sind `Alternate.master`. Der Teil von Markup für diese beiden Seiten haben eine `@MasterType` -Direktive, verweist der `Site.master` Masterseite.


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Auch, DetailsViews `ItemInserted` -Ereignishandler in `~/Admin/AddProduct.aspx` enthält Code, der die lose typisierten wandelt `Page.Master` Eigenschaft, um ein Objekt vom Typ `Site`. Die `@MasterType` -Direktive (auf diese Weise verwendet) und die Umwandlung in die `ItemInserted` Ereignishandler eng verbindet die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten der `Site.master` Masterseite.

Diese enge Kopplung unterbrochen wir haben `Site.master` und `Alternate.master` Ableiten von einer gemeinsamen Basisklasse, die Definitionen für die öffentlichen Member enthält. Danach können wir aktualisieren das `@MasterType` Direktive, um diese allgemeinen Basistyp verweisen.

### <a name="creating-a-custom-base-master-page-class"></a>Erstellen eine benutzerdefinierte Base Master Page-Klasse

Fügen Sie eine neue Klassendatei zu den `App_Code` Ordner mit dem Namen `BaseMasterPage.vb` und mit ihr abgeleitet sein `System.Web.UI.MasterPage`. Müssen wir definieren die `RefreshRecentProductsGrid` Methode und die `GridMessageText` Eigenschaft im `BaseMasterPage`, aber wir können nicht einfach verschieben sie es aus `Site.master` , da diese Member mit Websteuerelementen arbeiten, die für spezifisch sind die `Site.master` Gestaltungsvorlage (die `RecentProducts` GridView und `GridMessage` Bezeichnung).

Wir müssen werden konfiguriert, `BaseMasterPage` so, dass diese Member, es definiert sind sind jedoch tatsächlich von implementiert `BaseMasterPage`des abgeleiteten Klassen (`Site.master` und `Alternate.master`). Diese Art der Vererbung ist möglich, indem markieren Sie die Klasse als `MustInherit` und ihre Member als `MustOverride`. Kurz gesagt, diese Schlüsselwörter hinzufügen, auf die Klassen und Membern zwei kündigt, die `BaseMasterPage` wurde nicht implementiert `RefreshRecentProductsGrid` und `GridMessageText`, allerdings werden von abgeleiteten Klassen.

Wir müssen zudem definieren die `PricesDoubled` Ereignis in `BaseMasterPage` und bieten eine Möglichkeit, durch die abgeleiteten Klassen zum Auslösen des Ereignisses. Das Muster, die in .NET Framework verwendet, um dieses Verhalten zu ermöglichen ist, erstellen ein öffentliches Ereignis in der Basisklasse aus, und fügen Sie eine geschützte, überschreibbare Methode, die mit dem Namen `OnEventName`. Abgeleitete Klassen können anschließend rufen Sie diese Methode zum Auslösen des Ereignisses oder können überschrieben werden, um Code auszuführen, unmittelbar bevor oder nachdem das Ereignis ausgelöst wird.

Update der `BaseMasterPage` Klasse, damit sie den folgenden Code enthält:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

Öffnen Sie als Nächstes die `Site.master` Code-Behind-Klasse, und Ihr abgeleitet sein `BaseMasterPage`. Da `BaseMasterPage` gekennzeichnete Member enthält `MustOverride` müssen wir diese Member im überschreiben `Site.master`. Hinzufügen der `Overrides` Schlüsselwort, um die Methoden- und Eigenschaftendefinitionen. Außerdem aktualisieren Sie den Code, der auslöst, die `PricesDoubled` Ereignis in der `DoublePrice` Schaltfläche `Click` Ereignishandler mit einem Aufruf von der Basisklasse `OnPricesDoubled` Methode.

Nachdem diese Änderungen die `Site.master` Code-Behind-Klasse sollte den folgenden Code enthalten:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

Wir müssen auch aktualisieren `Alternate.master`des Code-Behind-Klasse ableiten `BaseMasterPage` und überschreiben Sie die beiden `MustOverride` Elemente. Da jedoch `Alternate.master` enthält eine GridView, Listen die neuesten Produkte noch eine Bezeichnung, die eine Meldung, nachdem ein neues Produkt angezeigt mit der Datenbank hinzugefügt wird werden, diese Methoden nicht benötigen nichts zu tun.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Verweisen auf der Basis Master Page-Klasse

Wir Anschluss an die `BaseMasterPage` Klasse und unsere zwei Masterseiten erweitert haben, der letzte Schritt ist zum Aktualisieren der `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten, um auf diese gemeinsamen Typ zu verweisen. Ändern Sie zunächst die `@MasterType` -Direktive auf beiden Seiten aus:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

Nach:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

Anstatt das Verweisen auf einen Dateipfad mit der `@MasterType` Eigenschaft verweist auf den Basistyp (`BaseMasterPage`). Folglich die stark typisierte `Master` Eigenschaft, die in beiden Seiten Code-Behind-Klassen verwendet wird jetzt vom Typ `BaseMasterPage` (anstelle von Typ `Site`). Durch diese Änderung festliegen beschäftigt sich erneut `~/Admin/Products.aspx`. Zuvor dies eine Umwandlung hat einen Fehler verursacht, da die Seite für die Verwendung konfiguriert ist die `Alternate.master` Masterseite, aber die `@MasterType` Richtlinie, die auf die verwiesen wird die `Site.master` Datei. Aber jetzt die Seite ohne Fehler gerendert wird. Grund hierfür ist die `Alternate.master` Masterseite umgewandelt werden kann, um ein Objekt des Typs `BaseMasterPage` (da er es erweitert wird).

Es wird eine kleine Änderung, die vorgenommen werden muss `~/Admin/AddProduct.aspx`. Des DetailsView-Steuerelements `ItemInserted` -Ereignishandler wird sowohl die stark typisierte `Master` -Eigenschaft und die lose typisierten `Page.Master` Eigenschaft. Wir die stark typisierte Referenz behoben, wenn wir aktualisiert die `@MasterType` Richtlinie, es wird jedoch weiterhin die lose typisierten Verweis aktualisieren müssen. Ersetzen Sie die folgende Codezeile ein:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Mit den folgenden wandelt die `Page.Master` auf den Basistyp:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Schritt 4: Bestimmen, welche Gestaltungsvorlage Bindung an die Inhaltsseiten

Unsere `BasePage` -Klasse legt derzeit alle Inhaltsseiten `MasterPageFile` Eigenschaften auf einen hartcodierten Wert in der PreInit-Phase des Lebenszyklus der Seite. Wir können diesen Code aus, um die Gestaltungsvorlage nach externen Faktoren basieren aktualisieren. Möglicherweise hängt die Gestaltungsvorlage laden die Voreinstellungen des aktuell angemeldeten Benutzers. In diesem Fall müssen wir Schreiben von Code in der `OnPreInit` Methode in `BasePage` sucht, die die derzeit über Einstellungen des Benutzers Masterseite.

Erstellen wir eine Webseite, die dem Benutzer ermöglicht, wählen die Gestaltungsvorlage verwendet - `Site.master` oder `Alternate.master` - und speichern Sie diese Auswahl in einer Sitzungsvariablen. Starten, indem Sie eine neue Webseite erstellen, in das Stammverzeichnis, das mit dem Namen `ChooseMasterPage.aspx`. Beim Erstellen dieser Seite (oder eine beliebige andere Inhaltsseiten künftig) müssen Sie nicht auf einer Masterseite gebunden werden, da die Gestaltungsvorlage programmgesteuert, in festgelegt wird `BasePage`. Wenn Sie nicht die neue Seite zu einer Masterseite binden enthält jedoch dann deklarative Standardmarkup für die neue Seite ein Web Form und anderen Inhalten, die von der Masterseite bereitgestellt. Sie müssen dieses Markup manuell durch die entsprechenden Inhaltssteuerelemente zu ersetzen. Aus diesem Grund einfacher ich die neue ASP.NET-Seite auf einer Masterseite zu binden.

> [!NOTE]
> Da `Site.master` und `Alternate.master` identisch festgelegt haben ContentPlaceHolder Steuerelemente es spielt keine welche Masterseite Rolle Sie beim Erstellen der neuen Inhaltsseite auswählen. Aus Gründen der Konsistenz ich würde Sie nach Möglichkeit `Site.master`.


[![Der Website eine neue Seite hinzufügen](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Abbildung 05**: Fügen Sie eine neue Seite auf der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image15.png))


Update der `Web.sitemap` Datei, die einen Eintrag in dieser Lektion hinzugefügt. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Master-Seiten und ASP.NET AJAX-Lektion:


[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Vor dem Hinzufügen von Inhalt an die `ChooseMasterPage.aspx` Seite nehmen einen Moment Zeit, zu der Seite Code-Behind-Klasse zu aktualisieren, sodass es abgeleitet `BasePage` (statt `System.Web.UI.Page`). Als Nächstes die Seite ein DropDownList-Steuerelement hinzugefügt haben, legen Sie dessen `ID` Eigenschaft `MasterPageChoice`, und fügen Sie zwei ListItems mit der `Text` Werte von "~ / Site.master" und "~ / Alternate.master".

Die Seite ein Websteuerelement Schaltfläche hinzu, und legen Sie dessen `ID` und `Text` Eigenschaften `SaveLayout` und "Layout-Auswahl speichern", bzw. An diesem Punkt sollte deklarativem Markup Ihrer Seite etwa wie folgt aussehen:


[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Zuerst besucht die Seite müssen wir die Auswahl des Benutzers aktuell ausgewählten Gestaltungsvorlage anzeigen. Erstellen einer `Page_Load` -Ereignishandler folgenden Code hinzu:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

Der obige Code ausgeführt wird, nur auf der ersten Besuch der Seite "(und nicht bei nachfolgenden Postbacks). Er zuerst überprüft, ob der Sitzungsvariablen `MyMasterPage` vorhanden ist. Wenn dies der Fall ist, versucht es, die übereinstimmende "ListItem" im finden die `MasterPageChoice` DropDownList. Wenn eine übereinstimmende "ListItem" gefunden wird, dessen `Selected` -Eigenschaftensatz auf `True`.

Wir benötigen außerdem Code, der die Auswahl des Benutzers in speichert die `MyMasterPage` Sitzungsvariablen. Erstellen Sie einen Ereignishandler für das `SaveLayout` Schaltfläche `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> Nach der Dauer der `Click` -Ereignishandler auf Postback ausführt, die Gestaltungsvorlage bereits ausgewählt wurde. Aus diesem Grund wird nicht die Auswahl des Benutzers Dropdown-Liste wirksam sein, bis die nächste Seite besuchen. Die `Response.Redirect` erzwingt, dass den Browser erneut anfordern `ChooseMasterPage.aspx`.


Mit der `ChooseMasterPage.aspx` Seite abgeschlossen werden, dass unsere letzte Aufgabe haben `BasePage` Zuweisen der `MasterPageFile` Eigenschaft basierend auf den Wert der `MyMasterPage` Sitzungsvariablen. Wenn die Sitzungsvariable nicht festgelegt ist, haben `BasePage` standardmäßig `Site.master`.


[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> Ich den Code, der weist verschoben der `Page` des Objekts `MasterPageFile` Eigenschaft von der `OnPreInit` -Ereignishandler und in zwei separate Methoden. Diese erste Methode `SetMasterPageFile`, weist der `MasterPageFile` -Eigenschaft auf den Wert der zweiten Methode zurückgegebenes `GetMasterPageFileFromSession`. Markiert die `SetMasterPageFile` Methode `Overridable` , damit zukünftige Klassen erweitern `BasePage` können optional außer Kraft setzen, um benutzerdefinierte Logik implementieren, bei Bedarf. Wir sehen ein Beispiel zum Überschreiben `BasePage`des `SetMasterPageFile` Eigenschaft in den nächsten Lernprogrammen.


Mit diesem Code werden, finden Sie auf der `ChooseMasterPage.aspx` Seite. Zu Beginn der `Site.master` Gestaltungsvorlage ist ausgewählt (siehe Abbildung 6), aber der Benutzer kann eine andere Gestaltungsvorlage aus der Dropdown-Liste auswählen.


[![Inhaltsseiten werden mit der Site.master Masterseite angezeigt.](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Abbildung 06**: Inhaltsseiten sind angezeigt, mit der `Site.master` Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image18.png))


[![Inhaltsseiten werden nun angezeigt mit der Alternate.master Gestaltungsvorlage](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Abbildung 07**: Inhaltsseiten werden angezeigt, jetzt unter Verwendung der `Alternate.master` Gestaltungsvorlage ([klicken Sie hier, um das Bild in voller Größe angezeigt](specifying-the-master-page-programmatically-vb/_static/image21.png))


## <a name="summary"></a>Zusammenfassung

Besucht Inhaltsseite sind die Content-Steuerelemente mit seiner Masterseite ContentPlaceHolder Steuerelementen fused. Masterseite der Inhaltsseite ist gekennzeichnet durch die `Page` Klasse `MasterPageFile` -Eigenschaft, die zugewiesen wird die `@Page` Richtlinie `MasterPageFile` Attribut während der Initialisierungsphase. Wie in diesem Lernprogramm wurde gezeigt, können wir einen Wert zuweisen der `MasterPageFile` Eigenschaft so lange wie wir dies vor dem Ende der Phase PreInit durchführen. Wird die Gestaltungsvorlage programmgesteuert angeben, wird die Tür für erweiterte Szenarien, z. B. Dynamisches Binden von einer Inhaltsseite auf einer Masterseite basierend auf externen Faktoren geöffnet.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Lebenszyklusdiagramm für ASP.NET die Seite "](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Übersicht über ASP.NET Seite-Lebenszyklus](https://msdn.microsoft.com/en-us/library/ms178472.aspx)
- [ASP.NET-Designs und Skins (Übersicht)](https://msdn.microsoft.com/en-us/library/ykzx33wh.aspx)
- [Masterseiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [Designs in ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Suchi Banerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](master-pages-and-asp-net-ajax-vb.md)
[Weiter](nested-master-pages-vb.md)
