---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
title: Programmgesteuertes Festlegen der Masterseite (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Festlegen der Masterseite der Inhaltsseite, programmgesteuert über den PreInit-Ereignishandler überprüft.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 7c4a3445-2440-4aee-b9fd-779c05e6abb2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b3fbd95a8088e20382c426fcdcc6a4b6e67d44b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370389"
---
<a name="specifying-the-master-page-programmatically-c"></a>Programmgesteuertes Festlegen der Masterseite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_CS.pdf)

> Festlegen der Masterseite der Inhaltsseite, programmgesteuert über den PreInit-Ereignishandler überprüft.


## <a name="introduction"></a>Einführung

Seit dem ersten Beispiel in [ *erstellen eine standortweite Layout mithilfe von Master Pages*](creating-a-site-wide-layout-using-master-pages-cs.md)werden alle Inhaltsdateien Seiten auf ihrer Masterseite deklarativ über verwiesen die `MasterPageFile` -Attribut in der `@Page`Richtlinie. Beispielsweise die folgenden `@Page` Richtlinie links die Seite Inhalte auf die Masterseite `Site.master`:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample1.aspx)]

Die [ `Page` Klasse](https://msdn.microsoft.com/library/system.web.ui.page.aspx) in die `System.Web.UI` -Namespace umfasst einen [ `MasterPageFile` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) , die den Pfad auf die Masterseite der Inhaltsseite zurückgibt; es ist diese Eigenschaft, die von der festgelegtist`@Page` Richtlinie. Diese Eigenschaft kann auch verwendet werden, können Sie die Masterseite der Inhaltsseite programmgesteuert angeben. Dieser Ansatz ist hilfreich, wenn Sie die Masterseite, die basierend auf externe Faktoren wie der Benutzer Zugriff auf die Seite dynamisch zuweisen möchten.

In diesem Tutorial stellen wir unsere Website, eine zweite Masterseite hinzugefügt und die Masterseite für die Verwendung zur Laufzeit dynamisch zu entscheiden.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Schritt 1: Einen Blick auf den Lebenszyklus der Seite

Wenn eine Anforderung auf dem Webserver für eine ASP.NET-Seite, die eine Inhaltsseite ist eingeht, muss das ASP.NET-Modul fuse der Seite Inhalt von Steuerelementen in die Masterseite die entsprechende ContentPlaceHolder-Steuerelemente. Diese Fusion erstellt eine einzelnes Steuerelement-Hierarchie, die dann durch den Lebenszyklus der Seite "typischen" fortgesetzt werden kann.

Abbildung 1 zeigt diese Fusion. Schritt 1 in Abbildung 1 zeigt die ursprünglichen Inhalt und die Hierarchien der Masterseite-Steuerelement. Steuerelemente auf der Seite werden am Ende des Protokollfragments der PreInit-Phase der Inhalt der entsprechenden ContentPlaceHolder-Steuerelemente auf der Masterseite (Schritt2) hinzugefügt. Nach diesem Fusion dient die Masterseite als Stamm der Steuerelementhierarchie fused. Dieses Steuerelement fused Hierarchie wird dann hinzugefügt, um die Seite, um die abgeschlossene Steuerelementhierarchie (Schritt 3) zu erstellen. Das Ergebnis ist die Steuerelementhierarchie der Seite die fused Steuerelementhierarchie enthält.


[![Der Masterseite und der Inhaltsseite Steuerelement Hierarchien werden zusammen mit Sicherung während der PreInit-Phase](specifying-the-master-page-programmatically-cs/_static/image2.png)](specifying-the-master-page-programmatically-cs/_static/image1.png)

**Abbildung 01**: der Masterseite und der Inhaltsseite Steuerelement Hierarchien werden zusammen mit Sicherung während der PreInit-Phase ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image3.png))


## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Schritt 2: Festlegen der`MasterPageFile`-Eigenschaft im Code

Welche Masterseite in dieser Fusion partakes, hängt davon ab, der Wert für die `Page` des Objekts `MasterPageFile` Eigenschaft. Festlegen der `MasterPageFile` -Attribut in der `@Page` -Anweisung hat das Ergebnis der Zuweisung der `Page`des `MasterPageFile` -Eigenschaft während der Initialisierungsphase, die die erste Phase des Lebenszyklus der Seite ist. Wir können diese Eigenschaft auch programmgesteuert festlegen. Allerdings ist es zwingend erforderlich, dass diese Eigenschaft festgelegt werden, bevor die Fusion in Abbildung 1 durchgeführt wird.

Am Anfang der PreInit-Phase der `Page` -Objekt löst die [ `PreInit` Ereignis](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) und ruft seine [ `OnPreInit` Methode](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Um die Masterseite programmgesteuert festzulegen, klicken Sie dann, wir können entweder erstellen einen Ereignishandler für die `PreInit` Ereignis- oder außer Kraft setzen der `OnPreInit` Methode. Sehen wir uns auf beide Ansätze.

Öffnen Sie zunächst `Default.aspx.cs`, die CodeBehind-Klassendatei für unsere Website-Homepage. Hinzufügen eines ereignishandlers für der Seite des `PreInit` Ereignis, indem Sie in den folgenden Code eingeben:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample2.cs)]

Von hier aus können wir Festlegen der `MasterPageFile` Eigenschaft. Aktualisieren Sie den Code, sodass den Wert weist "~ / Site.master" auf die `MasterPageFile` Eigenschaft.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample3.cs)]

Wenn Sie einen Haltepunkt festlegen und starten mit Debuggen Sie, dass sehen immer die `Default.aspx` Seite wird besucht, oder wird jedes Mal, wenn ein Postback auf dieser Seite die `Page_PreInit` -Ereignishandler ausgeführt wird und die `MasterPageFile` Eigenschaft zugewiesen ist "~ / Site.master".

Alternativ können Sie überschreiben die `Page` Klasse `OnPreInit` Methode, und legen die `MasterPageFile` Eigenschaft vorhanden. In diesem Beispiel nicht legen wir die Masterseite in eine bestimmte Seite, sondern von `BasePage`. Denken Sie daran, dass wir eine benutzerdefinierte Basisseite-Klasse erstellt (`BasePage`) zurück in die [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Tutorial. Derzeit `BasePage` überschreibt die `Page` Klasse `OnLoadComplete` -Methode, in dem der Seite wird `Title` -Eigenschaft basierend auf der Sitemap-Daten. Aktualisieren wir `BasePage` überschreiben auch die `OnPreInit` Methode, um die Masterseite programmgesteuert anzugeben.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample4.cs)]

Da alle unsere Inhaltsseiten abgeleitet `BasePage`, alle haben jetzt die Masterseite programmgesteuert zugewiesen. An diesem Punkt die `PreInit` -Ereignishandler in `Default.aspx.cs` überflüssig ist, können Sie ihn zu entfernen.

### <a name="what-about-thepagedirective"></a>Wie sieht die`@Page`Richtlinie?

Ein wenig verwirrend erscheinen möglicherweise ist, die der Inhaltsseiten `MasterPageFile` Eigenschaften sind jetzt an zwei Orten angegeben werden: programmgesteuert in die `BasePage` Klasse `OnPreInit` Methode sowie die `MasterPageFile` -Attribut in jeder Inhaltsseite `@Page` Richtlinie.

Die erste Phase im Lebenszyklus Seite ist der Initialisierungsphase. In dieser Phase der `Page` des Objekts `MasterPageFile` der Wert der Eigenschaft zugewiesen der `MasterPageFile` -Attribut in der `@Page` Richtlinie (falls es vorhanden ist). Die PreInit-Phase folgt der Initialisierungsphase, und es ist soweit, in dem wir programmgesteuert festlegen der `Page` des Objekts `MasterPageFile` und überschrieb den Wert im Bereich von-Eigenschaft der `@Page` Richtlinie. Da festlegt sind die `Page` des Objekts `MasterPageFile` Eigenschaft programmgesteuert, wir könnten Entfernen der `MasterPageFile` -Attribut aus der `@Page` -Anweisung ohne Auswirkungen auf die End-benutzererfahrung. Um sich selbst davon überzeugen möchten, fahren Sie fort, und entfernen Sie die `MasterPageFile` -Attribut aus der `@Page` -Direktive in `Default.aspx` und besuchen Sie dann auf die Seite über einen Browser. Wie zu erwarten ist, ist die Ausgabe identisch, bevor Sie das Attribut entfernt wurde.

Ob die `MasterPageFile` Eigenschaft wird festgelegt, über die `@Page` Richtlinie oder programmgesteuert an das Ende der Benutzeroberfläche nicht relevant ist. Allerdings die `MasterPageFile` -Attribut in der `@Page` Richtlinie wird von Visual Studio zur Entwurfszeit verwendet, um der WYSIWYG erzeugen Ansicht im Designer. Wenn Sie zum zurückkehren `Default.aspx` in Visual Studio, und navigieren Sie in den Designer sehen Sie die Meldung "Masterseite-Fehler: die Seite verfügt über Steuerelemente, eine Masterseite erfordern, aber keine angegeben" (siehe Abbildung 2).

Kurz gesagt, Sie behalten möchten die `MasterPageFile` -Attribut in der `@Page` Richtlinie, um eine umfassende während der Entwurfszeit-Funktionen in Visual Studio nutzen zu können.


[![Visual Studio nutzt die @Page MasterPageFile-Attribut für die Richtlinie zum Rendern der Entwurfsansicht](specifying-the-master-page-programmatically-cs/_static/image5.png)](specifying-the-master-page-programmatically-cs/_static/image4.png)

**Abbildung 02**: Visual Studio verwendet die `@Page` -Direktive `MasterPageFile` Attribut zum Rendern der Entwurfsansicht ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image6.png))


## <a name="step-3-creating-an-alternative-master-page"></a>Schritt 3: Erstellen eine Alternative-Masterseite

Da es sich bei einer Inhaltsseite Masterseite programmgesteuert zur Laufzeit festgelegt werden kann ist es möglich, eine bestimmte Masterseite, die externe kriterienbasierten dynamisch zu laden. Diese Funktionalität kann in Situationen nützlich sein, in dem der Website-Layout basierend auf den Benutzer variieren muss. Z. B. können eine Blog-Engine-Webanwendung Benutzer wählen ein Layout für ihren Blog, wobei jedes Layout, die mit einer anderen Masterseite zugeordnet ist. Zur Laufzeit beim ein Besucher eines Benutzers Blog anzeigen, wird müssten die Webanwendung bestimmen den Blog-Layout und die entsprechenden Masterseite dynamisch verknüpft wird, mit der Inhaltsseite.

Betrachten wir eine Gestaltungsvorlage zur Laufzeit basierend auf einigen externen Kriterien dynamisch zu laden. Unsere Website enthält derzeit nur eine Masterseite (`Site.master`). Wir benötigen einen anderen Masterseite zur Veranschaulichung eine Gestaltungsvorlage zur Laufzeit auswählen. Dieser Schritt im erstellen und konfigurieren die neue Masterseite Mittelpunkt. Schritt 4 sucht bestimmt werden, welche Masterseite für die Verwendung zur Laufzeit.

Erstellen Sie eine neue Masterseite in den Stammordner, der mit dem Namen `Alternate.master`. Fügen Sie ein neues Stylesheet auch auf der Website mit dem Namen `AlternateStyles.css`.


[![Fügen Sie ein weiteres Masterseite und CSS-Datei in der Website](specifying-the-master-page-programmatically-cs/_static/image8.png)](specifying-the-master-page-programmatically-cs/_static/image7.png)

**Abbildung 03**: Hinzufügen einer anderen Masterseite und CSS-Datei auf der Website ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image9.png))


Ich entwarf haben die `Alternate.master` Masterseite haben den Titel oben auf der Seite zentriert und auf einem dunkelblaue Hintergrund angezeigt. Ich habe Schiffsteil abgesehen von der linken Spalte und verschoben, dass der Inhalt unterhalb der `MainContent` ContentPlaceHolder-Steuerelement, das erstreckt sich jetzt die gesamte Breite der Seite über. Darüber hinaus ich nixed die unsortierte Liste der Lektionen und ersetzt es durch eine horizontale Liste oben `MainContent`. Ich habe auch aktualisiert Schriftarten und Farben verwendet werden, von der Masterseite (und somit die Inhaltsseiten). Abbildung 4 zeigt `Default.aspx` bei Verwendung der `Alternate.master` Masterseite.

> [!NOTE]
> ASP.NET bietet die Möglichkeit, definieren Sie *Designs*. Ein Design ist eine Sammlung von Bildern, CSS-Dateien und Formatvorlagen bezogenen Web Control eigenschaftseinstellungen, die auf einer Seite zur Laufzeit angewendet werden können. Designs sind die beste Wahl, wenn Ihre Website Layouts nur im angezeigten Bilder und durch ihre CSS-Regeln unterscheiden. Wenn die Layouts unterscheiden sich mehr erheblich an, wie die Verwendung von anderen Websteuerelemente oder müssen ein völlig anderes Layout, Sie separate Masterseiten verwenden müssen. Prüfen Sie im Abschnitt Weitere nützliche Informationen am Ende dieses Tutorials für Weitere Informationen zu Designs.


[![Unsere Inhaltsseiten können jetzt ein neues Aussehen und Verhalten verwenden.](specifying-the-master-page-programmatically-cs/_static/image11.png)](specifying-the-master-page-programmatically-cs/_static/image10.png)

**Abbildung 04**: unsere Inhaltsseiten können Sie jetzt ein neues Aussehen und Verhalten ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image12.png))


Wenn die Master- und Inhaltsseiten Markup abgesichert werden, die `MasterPage` Klasse sicher, dass alle Inhalte-Steuerelement in der Seite Inhalt auf ein ContentPlaceHolder-Objekt in die Masterseite verweist. Eine Ausnahme wird ausgelöst, wenn ein Inhaltssteuerelement, das eine nicht existierende ContentPlaceHolder verweist auf gefunden wird. Das heißt, es ist zwingend erforderlich, dass die Masterseite der Inhaltsseite zugewiesen wird, ein ContentPlaceHolder-Objekt für jedes haben Inhaltssteuerelement in der Seite Inhalt.

Die `Site.master` Masterseite enthält vier ContentPlaceHolder-Steuerelemente:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Einige der Inhaltsseiten in unserer Website sind nur ein oder zwei ContentControl-Elemente: andere enthalten ein ContentControl-Element für jede der verfügbaren ContentPlaceHolder-Steuerelemente. Wenn unsere neue Masterseite (`Alternate.master`) kann diese Inhaltsseiten, die Inhaltssteuerelemente für alle von der ContentPlaceHolder-Steuerelemente in Werte zugewiesen werden `Site.master` ist es wichtig, `Alternate.master` auch enthalten die gleichen ContentPlaceHolder-Steuerelemente als `Site.master`.

Zum Abrufen Ihrer `Alternate.master` Masterseite aussehen wie in Data Mining (siehe Abbildung 4) starten, indem Sie definieren die Masterseite Stile in die `AlternateStyles.css` Stylesheet. Fügen Sie die folgenden Regeln in `AlternateStyles.css`:


[!code-css[Main](specifying-the-master-page-programmatically-cs/samples/sample5.css)]

Fügen Sie das folgende deklarative Markup zu `Alternate.master`. Wie Sie sehen können, `Alternate.master` enthält vier ContentPlaceHolder-Steuerelemente, mit dem gleichen `ID` Werte wie die ContentPlaceHolder-Steuerelemente in `Site.master`. Außerdem enthält es ein ScriptManager-Steuerelement, der für die Seiten auf unserer Website erforderlich ist, die ASP.NET AJAX-Framework verwenden.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Testen der neuen Master-Seite

Zum Testen von diesem neuen Updates für die Masterseite der `BasePage` -Klasse `OnPreInit` Methode, damit die `MasterPageFile` Eigenschaft wird der Wert zugewiesen "~ / Alternate.master", und klicken Sie dann die Website besuchen. Jede Seite sollte ohne Fehler mit Ausnahme von zwei funktionieren: `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx`. Hinzufügen eines Produkts zum DetailsView in `~/Admin/AddProduct.aspx` führt zu einer `NullReferenceException` aus der Zeile des Codes, der versucht, die Gestaltungsvorlage festgelegt `GridMessageText` Eigenschaft. Beim Zugriff auf `~/Admin/Products.aspx` ein `InvalidCastException` wird beim Laden der Seite mit der folgenden Meldung ausgelöst: "wandelt ein Objekt vom Typ kann nicht ' ASP.alternate\_master" in den Typ "ASP.site\_master". "

Diese Fehler auftreten, da die `Site.master` Code-Behind-Klasse enthält öffentliche Ereignisse, Eigenschaften und Methoden, die nicht in definiert sind `Alternate.master`. Der Teil von Markup für diese beiden Seiten haben eine `@MasterType` -Direktive, verweist der `Site.master` Masterseite.


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample7.aspx)]

Darüber hinaus DetailsView `ItemInserted` -Ereignishandler in `~/Admin/AddProduct.aspx` enthält Code, der die lose typisierte wandelt `Page.Master` Eigenschaft, um ein Objekt des Typs `Site`. Die `@MasterType` -Direktive (auf diese Weise verwendet) und die Umwandlung in die `ItemInserted` Ereignishandler eng koppelt die `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` auf Seiten der `Site.master` Masterseite.

Diese enge Kopplung unterbrechen können `Site.master` und `Alternate.master` leiten Sie von einer gemeinsamen Basisklasse, die Definitionen für die öffentlichen Member enthält. Danach können wir aktualisieren den `@MasterType` Direktive, um diese allgemeinen Basistyp zu verweisen.

### <a name="creating-a-custom-base-master-page-class"></a>Erstellen eine benutzerdefinierte Base Master-Page-Klasse

Fügen Sie eine neue Klassendatei mit dem `App_Code` Ordner mit dem Namen `BaseMasterPage.cs` und abgeleitet `System.Web.UI.MasterPage`. Müssen wir definieren die `RefreshRecentProductsGrid` Methode und die `GridMessageText` -Eigenschaft in `BaseMasterPage`, aber wir können nicht einfach verschieben sie es aus `Site.master` , da diese Member mit Websteuerelementen arbeiten, die für spezifisch sind die `Site.master` Masterseite (die `RecentProducts` GridView und `GridMessage` Bezeichnung).

Was wir tun müssen werden konfiguriert, `BaseMasterPage` so, dass diese Member werden es definiert, aber eigentlich, indem implementiert sind `BaseMasterPage`abgeleiteten Klassen (`Site.master` und `Alternate.master`). Diese Art der Vererbung ist möglich, durch Markieren der Klasse und ihre Member als `abstract`. Kurz gesagt: Hinzufügen der `abstract` kündigt von Schlüsselwort, um diese zwei Elemente, die `BaseMasterPage` wurde nicht implementiert `RefreshRecentProductsGrid` und `GridMessageText`, allerdings werden von abgeleiteten Klassen.

Wir müssen auch zum Definieren der `PricesDoubled` Ereignis in `BaseMasterPage` und bieten eine Möglichkeit, von den abgeleiteten Klassen zum Auslösen des Ereignisses. Das Muster, die in .NET Framework verwendet, um dieses Verhalten zu vereinfachen ist, erstellen ein öffentliches Ereignis in der Basisklasse aus, und fügen eine geschützte, `virtual` Methode mit dem Namen `OnEventName`. Abgeleitete Klassen können anschließend rufen Sie diese Methode zum Auslösen des Ereignisses oder können überschrieben werden, um Code auszuführen, unmittelbar vor oder nach der das Ereignis ausgelöst wird.

Aktualisieren Ihrer `BaseMasterPage` Klasse, sodass sie den folgenden Code enthält:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample8.cs)]

Navigieren Sie anschließend auf die `Site.master` CodeBehind-Klasse und deren abgeleitet `BaseMasterPage`. Da `BaseMasterPage` ist `abstract` müssen wir außer Kraft `abstract` Elemente hier in `Site.master`. Hinzufügen der `override` Schlüsselwort, um die Methoden- und Eigenschaftendefinitionen. Außerdem aktualisieren Sie den Code, der auslöst, die `PricesDoubled` Ereignis in der `DoublePrice` Schaltfläche `Click` -Ereignishandler durch einen Aufruf der Basisklasse `OnPricesDoubled` Methode.

Nachdem diese Änderungen die `Site.master` Code-Behind-Klasse sollte den folgenden Code enthalten:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample9.cs)]

Wir müssen auch aktualisieren `Alternate.master`des Code-Behind-Klasse für die Ableitung `BaseMasterPage` und überschreiben Sie die beiden `abstract` Member. Aber da `Alternate.master` enthält keiner GridView-Ansicht, dass Listen, die die neuesten Produkte noch eine Bezeichnung, die eine Meldung, nachdem ein neues Produkt angezeigt in der Datenbank hinzugefügt wird, diese Methoden nicht nichts weiter tun müssen.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample10.cs)]

### <a name="referencing-the-base-master-page-class"></a>Verweisen Sie auf der Basis-Master-Page-Klasse

Nun, wir haben die `BaseMasterPage` Klasse und unsere zwei Masterseiten erweitern, der letzte Schritt ist zum Aktualisieren der `~/Admin/AddProduct.aspx` und `~/Admin/Products.aspx` Seiten, um auf diesen gemeinsamen Typ zu verweisen. Ändern Sie zunächst die `@MasterType` -Direktive auf beiden Seiten aus:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample11.aspx)]

Nach:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample12.aspx)]

Anstatt das Verweisen auf einen Dateipfad der `@MasterType` Eigenschaft verweist jetzt auf den Basistyp (`BaseMasterPage`). Daher die stark typisierte `Master` -Eigenschaft, die in beide Seiten einer CodeBehind-Klassen verwendet, ist jetzt vom Typ `BaseMasterPage` (anstelle des Typs `Site`). Durch diese Änderung an Stelle noch einmal `~/Admin/Products.aspx`. Zuvor, Dies führte zu einem Fehler umwandeln, da die Seite für die Verwendung konfiguriert ist die `Alternate.master` Masterseite, aber die `@MasterType` Richtlinie, die auf die verwiesen wird die `Site.master` Datei. Aber jetzt die Seite ohne Fehler gerendert. Grund hierfür ist die `Alternate.master` Masterseite umgewandelt werden kann, um ein Objekt des Typs `BaseMasterPage` (da es erweitert).

Es ist eine kleine Änderung, die vorgenommen werden muss `~/Admin/AddProduct.aspx`. Des DetailsView-Steuerelements `ItemInserted` -Ereignishandler wird sowohl die stark typisierte `Master` Eigenschaft und die lose typisierte `Page.Master` Eigenschaft. Wir haben den stark typisierten Verweis behoben, wenn wir aktualisiert die `@MasterType` Richtlinie, aber wir benötigen den lose typisierten Verweis aktualisieren. Ersetzen Sie die folgende Codezeile ein:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample13.cs)]

Durch Folgendes, wandelt die `Page.Master` , die dem Basistyp entspricht:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample14.cs)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Schritt 4: Bestimmen, welche Seite "Master" an das Bindung hergestellt, auf die Inhaltsseiten

Unsere `BasePage` -Klasse legt derzeit alle Inhaltsseiten `MasterPageFile` Eigenschaften, die einen hartcodierten Wert in der PreInit-Phase des Lebenszyklus der Seite. Wir können diesen Code, um die Masterseite als Grundlage für einige externe Faktor aktualisieren. Die Masterseite laden hängt möglicherweise die Einstellungen des aktuell angemeldeten Benutzers. In diesem Fall müssten wir Code schreiben, der `OnPreInit` -Methode in der `BasePage` sucht, die derzeit das Besuchen Masterseite benutzereinstellungen.

Erstellen Sie eine Webseite, die dem Benutzer ermöglicht, wählen Sie die Masterseite zu verwenden – wir `Site.master` oder `Alternate.master` –, und speichern Sie diese Option in einer Sitzungsvariablen. Zunächst erstellen Sie eine neue Webseite in das Stammverzeichnis, das mit dem Namen `ChooseMasterPage.aspx`. Beim Erstellen von dieser Seite (oder jeder anderen Inhaltsseiten ab sofort) müssen Sie nicht auf eine Gestaltungsvorlage gebunden werden, da die Masterseite programmgesteuert, in festgelegt wird `BasePage`. Wenn Sie keine neue Seite auf eine Gestaltungsvorlage Bindung durchführen enthält jedoch dann deklarativen Markup für die neue Seite standardmäßig ein Webformular und anderen Inhalten, die von der Masterseite angegeben. Sie müssen dieses Markup manuell mit den entsprechenden Inhaltssteuerelementen zu ersetzen. Aus diesem Grund finde ich es einfacher, die neue Seite mit ASP.NET auf eine Gestaltungsvorlage zu binden.

> [!NOTE]
> Da `Site.master` und `Alternate.master` haben den gleichen Satz ContentPlaceHolder-Steuerelemente es spielt keine welche Masterseite Rolle Sie auswählen, wenn die neue Seite zu erstellen. Aus Gründen der Konsistenz würde ich empfehlen, mit `Site.master`.


[![Fügen Sie auf der Website eine neue Seite hinzu](specifying-the-master-page-programmatically-cs/_static/image14.png)](specifying-the-master-page-programmatically-cs/_static/image13.png)

**Abbildung 05**: Fügen Sie auf der Website eine neue Seite hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image15.png))


Update der `Web.sitemap` hinzu, um einen Eintrag in dieser Lektion einzubeziehen. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Masterseiten und ASP.NET AJAX-Lektion:


[!code-xml[Main](specifying-the-master-page-programmatically-cs/samples/sample15.xml)]

Vor dem Hinzufügen von Inhalt, der `ChooseMasterPage.aspx` Seite können Sie die CodeBehind-Klasse der Seite zu aktualisieren, sodass es abgeleitet `BasePage` (statt `System.Web.UI.Page`). Als Nächstes fügen Sie einem DropDownList-Steuerelement auf der Seite hinzu, legen Sie dessen `ID` Eigenschaft `MasterPageChoice`, und fügen Sie zwei ListItems mit der `Text` Werte von "~ / Site.master" und "~ / Alternate.master".

Fügen Sie ein Steuerelement Schaltfläche auf der Seite, und legen Sie dessen `ID` und `Text` Eigenschaften `SaveLayout` und "Layout-Auswahl speichern", bzw. An diesem Punkt sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:


[!code-aspx[Main](specifying-the-master-page-programmatically-cs/samples/sample16.aspx)]

Wenn Sie zuerst die Seite besucht wird müssen wir die Auswahl des Benutzers aktuell ausgewählten Masterseite angezeigt. Erstellen Sie eine `Page_Load` -Ereignishandler, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample17.cs)]

Der obige Code führt nur auf den ersten Besuch der Seite (und nicht bei nachfolgenden Postbacks). Es zuerst überprüft, wenn die Sitzungsvariable `MyMasterPage` vorhanden ist. Wenn dies der Fall ist, versucht wird, finden Sie in das entsprechende ListItem der `MasterPageChoice` DropDownList. Wenn eine übereinstimmende ListItem gefunden wird, dessen `Selected` -Eigenschaftensatz auf `true`.

Wir benötigen außerdem Code, der die Auswahl des Benutzers in speichert die `MyMasterPage` Sitzungsvariablen. Erstellen Sie einen Ereignishandler für die `SaveLayout` Schaltfläche `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample18.cs)]

> [!NOTE]
> Mit der Zeit die `Click` Ereignishandler beim Postback ausführt, wird die Masterseite wurde bereits ausgewählt. Aus diesem Grund nicht die Auswahl des Benutzers Dropdown-Listenfeld in Kraft, bis die nächste Seite finden Sie unter. Die `Response.Redirect` erzwingt, dass den Browser erneut anfordern `ChooseMasterPage.aspx`.


Mit der `ChooseMasterPage.aspx` Seite vollständige unsere letzte Aufgabe ist, damit `BasePage` weisen die `MasterPageFile` -Eigenschaft basierend auf den Wert des der `MyMasterPage` Sitzungsvariablen. Wenn die Sitzung-Variable nicht festgelegt ist, haben `BasePage` standardmäßig `Site.master`.


[!code-csharp[Main](specifying-the-master-page-programmatically-cs/samples/sample19.cs)]

> [!NOTE]
> Ich habe den Code, der weist verschoben der `Page` des Objekts `MasterPageFile` Eigenschaft von der `OnPreInit` -Ereignishandler und in zwei separate Methoden. Dieser erste Methode `SetMasterPageFile`, weist der `MasterPageFile` Eigenschaft, um den Rückgabewert von der zweiten Methode `GetMasterPageFileFromSession`. Mir die `SetMasterPageFile` Methode `virtual` , damit zukünftige Klassen erweitern `BasePage` kann optional außer Kraft setzen, um die benutzerdefinierte Logik implementieren, falls erforderlich. Wir sehen uns ein Beispiel zum Überschreiben `BasePage`des `SetMasterPageFile` Eigenschaft im nächsten Tutorial.


Mit diesem Code werden, finden Sie auf die `ChooseMasterPage.aspx` Seite. Zunächst die `Site.master` master Seite ausgewählt ist (siehe Abbildung 6), aber der Benutzer kann eine andere Masterseite aus der Dropdown-Liste auswählen.


[![Inhaltsseiten werden mithilfe der Site.master-Masterseite angezeigt.](specifying-the-master-page-programmatically-cs/_static/image17.png)](specifying-the-master-page-programmatically-cs/_static/image16.png)

**Abbildung 06**: Inhaltsseiten werden angezeigt, unter Verwendung der `Site.master` Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image18.png))


[![Inhaltsseiten werden mithilfe der Alternate.master Masterseite jetzt angezeigt.](specifying-the-master-page-programmatically-cs/_static/image20.png)](specifying-the-master-page-programmatically-cs/_static/image19.png)

**Abbildung 07**: Inhaltsseiten werden angezeigt, jetzt unter Verwendung der `Alternate.master` Masterseite ([klicken Sie, um das Bild in voller Größe anzeigen](specifying-the-master-page-programmatically-cs/_static/image21.png))


## <a name="summary"></a>Zusammenfassung

Wenn eine Inhaltsseite aufgerufen wird, werden die Inhaltssteuerelemente mit der Masterseite ContentPlaceHolder-Steuerelemente mit Sicherung. Masterseite der Inhaltsseite ist gekennzeichnet durch die `Page` Klasse `MasterPageFile` -Eigenschaft, die zugewiesen ist die `@Page` -Direktive `MasterPageFile` Attribut während der Initialisierungsphase. Wie in diesem Tutorial wurde gezeigt, können wir einen Wert zuweisen der `MasterPageFile` Eigenschaft solange wir dazu vor dem Ende der PreInit-Phase. Können programmgesteuert auf die Masterseite angegeben wird, öffnet die Tür für einige erweiterte Szenarien, z. B. Dynamisches Binden von einer Inhaltsseite auf eine Gestaltungsvorlage basierend auf externen Faktoren.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Lebenszyklusdiagramm für ASP.NET die Seite](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Übersicht über ASP.NET Seite-Lebenszyklus](https://msdn.microsoft.com/library/ms178472.aspx)
- [ASP.NET-Designs und Skins (Übersicht)](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Masterseiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [Designs in ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Suchi Banerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-pages-and-asp-net-ajax-cs.md)
> [Weiter](nested-master-pages-cs.md)
