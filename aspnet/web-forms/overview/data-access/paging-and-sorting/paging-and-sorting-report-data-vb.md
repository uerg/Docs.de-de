---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Paging und Sortieren von Berichtsdaten (VB) | Microsoft Docs
author: rick-anderson
description: Paging und Sortieren von sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. In diesem Lernprogramm führen wir einen ersten Blick auf das Hinzufügen einer Sortierung und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: c5e7e110d436caa7b7526eae105fde601367007a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887238"
---
<a name="paging-and-sorting-report-data-vb"></a>Paging und Sortieren von Berichtsdaten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) oder [PDF herunterladen](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Paging und Sortieren von sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. In diesem Lernprogramm führen wir einen ersten Blick auf die Sortierung und paging unsere Berichte, in denen wir dann nach in zukünftigen Lernprogrammen erstellen, hinzufügen.


## <a name="introduction"></a>Einführung

Paging und Sortieren von sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. Beispielsweise bei der Suche nach ASP.NET Bücher Onlinebuchhandel gibt es möglicherweise Hunderte von Büchern, aber der Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Name des Autors und So weiter sortiert werden. Während der letzten 23 Lernprogramme Gewusst wie: Erstellen Sie eine Vielzahl von Berichten, Schnittstellen, mit denen hinzufügen, bearbeiten und Löschen von Daten, einschließlich untersucht haben wir Ve nicht erläutert, wie zum Sortieren von Daten und die einzige Beispiele für paging wir haben gesehen wurden mit den, DetailsView und FormView Steuerelemente.

In diesem Lernprogramm sehen wir, wie Sortierung und Auslagerung unsere Berichte, in denen ausgeführt werden können, indem Sie einfach einige Kontrollkästchen überprüfen hinzugefügt. Leider dieser eher einfache Implementierung hat die Nachteile, die die Sortierung Schnittstelle einem bit zu wünschen übrig lässt und die Paginierung Routinen dienen nicht effizient große Resultsets durchgehen. Zukünftige Lernprogrammen werden wie die Einschränkungen für die Out-of-the-Box paging und Sortieren von Lösungen umgehen untersuchen.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Paging und Sortieren von Tutorial Webseiten

Bevor wir in diesem Lernprogramm beginnen, können Sie s, schalten Sie zuerst einen Moment Zeit, die ASP.NET-Seiten hinzufügen, die wir für dieses Lernprogramm und den nächsten drei benötigen. Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `PagingAndSorting`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, dass alle von ihnen für die Verwendung die Masterseite konfiguriert `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Erstellen Sie einen Ordner PagingAndSorting, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu](paging-and-sorting-report-data-vb/_static/image1.png)

**Abbildung 1**: Erstellen Sie einen Ordner PagingAndSorting, und fügen Sie dem Tutorial ASP.NET-Seiten hinzu


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus den `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in erstellt die [Masterseiten und Websitenavigation](../introduction/master-pages-and-site-navigation-vb.md) Lernprogramm, zählt die Siteübersicht und diese Lernprogramme im aktuellen Abschnitt in einer Aufzählung angezeigt.


![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](paging-and-sorting-report-data-vb/_static/image2.png)

**Abbildung 2**: das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen


Damit haben die Liste mit Aufzählungszeichen angezeigt, das Paging und sortieren die Lernprogramme, die wir erstellen, müssen wir diese Siteübersicht hinzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach dem vom Markup bearbeiten, einfügen und Löschen von Website zuordnen, Knoten:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten](paging-and-sorting-report-data-vb/_static/image3.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neue ASP.NET-Seiten enthalten


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Schritt 2: Anzeigen von Produktinformationen in einer GridView

Bevor wir paging und Sortieren von Funktionen tatsächlich implementieren, können Sie s erstellen Sie zunächst eine standardmäßige nicht-Srotable, nicht auslagerbaren GridView, die die Produktinformationen enthält. Dies ist ein Task wir gibt es oft vor ausgeführt während des gesamten dieser Reihe von Lernprogrammen daher diese Schritte sollten damit vertraut sein. Öffnen Sie zunächst die `SimplePagingSorting.aspx` Seite, und ziehen Sie ein GridView-Steuerelement aus der Toolbox in den Designer, Festlegen seiner `ID` Eigenschaft `Products`. Als Nächstes erstellen Sie eine neue ObjectDataSource, die die Klasse ProductsBLL s verwendet `GetProducts()` Methode, um alle Produktinformationen zurückzugeben.


![Abrufen von Informationen für alle Produkte mit der GetProducts()-Methode](paging-and-sorting-report-data-vb/_static/image4.png)

**Abbildung 4**: Abrufen von Informationen für alle Produkte mit der GetProducts()-Methode


Da dieser Bericht ein schreibgeschützter Bericht, der es s nicht ist das ObjectDataSource-en die Zuordnung müssen `Insert()`, `Update()`, oder `Delete()` Methoden, um die entsprechenden `ProductsBLL` Methoden; daher für das UPDATE, INSERT, aus der Dropdown-Liste auswählen (keine) Registerkarten "" und löschen.


![Wählen Sie (keine)-Option in der Dropdown-Liste in der Update-, INSERT- und Löschen von Registerkarten](paging-and-sorting-report-data-vb/_static/image5.png)

**Abbildung 5**: Wählen Sie (keine)-Option in der Dropdown-Liste in der Update-, INSERT- und Löschen von Registerkarten


Als Nächstes können Sie s GridView s Felder anpassen, sodass nur die Produktnamen, Lieferanten, Kategorien, Preise und nicht mehr unterstützte Status angezeigt werden. Darüber hinaus stellen Feldebene Formatierungen gerne ändert, z. B. zum Anpassen der `HeaderText` Eigenschaften oder die Formatierung des Preis als Währung. Nach der Änderung sollte Ihre GridView s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt. Beachten Sie, dass die Seite zeigt eine Liste aller Produkte in einem Bildschirm, die mit einer einzelnen Produktname s, Kategorie, Lieferanten, Preis, und nicht mehr Status unterstützte.


[![Jede der Produkte aufgeführt sind](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Abbildung 6**: aller Produkte aufgeführt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Schritt 3: Hinzufügen von Unterstützung der Paginierung

Auflisten von *alle* Produkte auf einem Bildschirm an Informationen Überladung für den Benutzer zum Durchlesen der Daten führen kann. Damit die Ergebnisse besser ist, kann die Daten in kleineren Datenseiten zusammensetzen und ermöglicht dem Benutzer die Daten einer Seite zu einem Zeitpunkt durchlaufen werden. Zum Ausführen dieser einfach das Kontrollkästchen Paging aktivieren aus dem GridView-s-Smarttag (Dies legt die GridView s [ `AllowPaging` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) auf `true`).


[![Überprüfen Sie das Paging aktivieren zum Hinzufügen der Unterstützung der Paginierung](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Abbildung 7**: Kontrollkästchen aktivieren Paging zur Unterstützung von Paging hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image11.png))


Aktivieren von Paging begrenzt die Anzahl der Datensätze pro Seite angezeigt, und fügt eine *Paging Schnittstelle* an die GridView. Die Auslagerung Standardschnittstelle, siehe Abbildung 7, ist eine Reihe von Seitenzahlen, sodass der Benutzer schnell von einer Seite der Daten in einen anderen zu navigieren. Diese Schnittstelle Paging aussehen vertraut sind, als es gibt es schon beim Hinzufügen der Unterstützung der Paginierung, DetailsView und FormView-Steuerelemente in den vergangenen Lernprogramme.

Die DetailsView und die FormView-Steuerelemente zeigen nur einen einzelnen Datensatz pro Seite. Die GridView jedoch fragt die [ `PageSize` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) um zu bestimmen, wie viele Datensätze pro Seite angezeigt (diese Eigenschaft standardmäßig einen Wert von 10).

Diese Schnittstelle GridView, DetailsView und FormView s Paging kann mithilfe der folgenden Eigenschaften angepasst werden:

- `PagerStyle` Gibt die Formatinformationen für die Auslagerungsdatei-Schnittstelle an. festlegbaren Einstellungen wie `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`und so weiter.
- `PagerSettings` enthält eine Bevy von Eigenschaften, die die Funktionalität der Schnittstelle Paging anpassen können. `PageButtonCount` gibt die maximale Anzahl der numerischen Seitenzahlen angezeigt, in der Auslagerungsdatei-Schnittstelle (der Standard ist 10); das [ `Mode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) gibt an, wie die Paginierung-Schnittstelle funktioniert und kann so festgelegt werden: 

    - `NextPrevious` Zeigt ein Schaltflächen Weiter und zurück, sodass der Benutzer eine Seite vorwärts oder rückwärts zu einem Zeitpunkt zu Schritt
    - `NextPreviousFirstLast` Zusätzlich zu den Schaltflächen Weiter und zurück sind auch erste und letzte Schaltflächen enthalten, sodass der Benutzer auf der ersten oder letzten Seite der Daten schnell zu verschieben
    - `Numeric` Zeigt eine Reihe von Seitenzahlen, sodass der Benutzer sofort auf einer beliebigen Seite wechseln
    - `NumericFirstLast` Zusätzlich zu den Seitenzahlen umfasst erste und letzte Schaltflächen, sodass der Benutzer auf der ersten oder letzten Seite der Daten schnell zu verschieben. die ersten/letzten Schaltflächen werden nur angezeigt, wenn alle der numerischen Seitenzahlen aufgenommen werden können

Darüber hinaus die GridView, DetailsView und FormView alle bieten die `PageIndex` und `PageCount` Eigenschaften, die der aktuellen Seite angezeigt wird und die Gesamtanzahl von Datenseiten, Serveroptionsparameter anzugeben. Die `PageIndex` -Eigenschaft ist indiziert, beginnend mit 0, was bedeutet, dass beim Anzeigen der ersten Seite des Data `PageIndex` gleich "0" wird. `PageCount`, auf der anderen Seite beginnt Zähler bei 1, was bedeutet, dass `PageIndex` ist beschränkt auf die Werte zwischen 0 und `PageCount - 1`.

Lassen Sie s einen Moment Zeit, die standarddarstellung der unsere GridView s Paging-Schnittstelle zu verbessern. Ermöglichen Sie insbesondere s die Paging-Schnittstelle rechtsbündig mit einem hellen grauen Hintergrund aufweisen. Anstatt das Festlegen dieser Eigenschaften direkt über die GridView s `PagerStyle` -Eigenschaft, Let s erstellen Sie eine CSS-Klasse in `Styles.css` mit dem Namen `PagerRowStyle` und weisen Sie ihm die `PagerStyle` s `CssClass` Eigenschaft über unser Design. Öffnen Sie zunächst `Styles.css` und Klassendefinition für den folgenden CSS-Code hinzufügen:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Öffnen Sie als Nächstes die `GridView.skin` in der Datei die `DataWebControls` Ordner innerhalb der `App_Themes` Ordner. Wie in erläutert die *Masterseiten und Websitenavigation* Lernprogramme, Designmodus Dateien können verwendet werden, um die Standardwerte für die Eigenschaft für ein Websteuerelement anzugeben. Aus diesem Grund ergänzen die vorhandenen Einstellungen auf Einbeziehung der Einstellung der `PagerStyle` s `CssClass` Eigenschaft `PagerRowStyle`. Let s konfigurieren Sie zudem die Auslagerungsschnittstelle, um höchstens fünf numerischen Seitenschaltflächen mit Anzeigen der `NumericFirstLast` Paging-Schnittstelle.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Die Benutzeroberfläche des Paging

Abbildung 8 zeigt die Webseite besucht über einen Browser, nachdem die GridView s Paging aktivieren das Kontrollkästchen aktiviert wurde und die `PagerStyle` und `PagerSettings` Konfigurationen wurden durch die `GridView.skin` Datei. Hinweis nur zehn Datensätze werden angezeigt, und die Paginierung Schnittstelle gibt an, dass wir die erste Seite der Daten anzeigen.


[![Mit Paging aktiviert ist nur eine Teilmenge der Datensätze zu einem Zeitpunkt angezeigt werden](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Abbildung 8**: mit Paging aktiviert, nur eine Teilmenge der Datensätze zu einem Zeitpunkt angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image14.png))


Klickt der Benutzer auf einem der Seitenzahlen in der Auslagerungsdatei-Schnittstelle, ein Postback erfolgt, und die Seite Neuladen angezeigt, die Datensätze der Seite "s" angefordert. Abbildung 9 zeigt die Ergebnisse nach der Aktivierung, um die letzte Seite der Daten anzuzeigen. Beachten Sie, dass die letzte Seite nur einem Datensatz. Dies ist da 81 Datensätze insgesamt acht Seiten des 10 Datensätze pro Seite plus eine Seite mit einem Datensatz zurückgemarshallt resultierende vorliegen.


[![Durch Klicken auf eine Seitenzahl an einen Postback verursacht, und zeigt die entsprechenden Teilmenge von Datensätzen](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Abbildung 9**: durch Klicken auf eine Seitenzahl einen Postback verursacht, und zeigt die entsprechenden Teilmenge der Datensätze ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Paging s serverseitige Workflow

Klickt der Benutzer auf eine Schaltfläche in der Auslagerungsdatei-Schnittstelle, ein Postback erfolgt, und der folgende Workflow der serverseitigen beginnt:

1. Die GridView-s (oder DetailsView oder FormView) `PageIndexChanging` -Ereignis ausgelöst
2. Das ObjectDataSource erneut anfordert *alle* der Daten aus der BLL; die GridView s `PageIndex` und `PageSize` Eigenschaftswerte werden verwendet, um zu bestimmen, was der BLL zurückgegebenen Datensätze in die GridView angezeigt werden müssen
3. Die GridView s `PageIndexChanged` -Ereignis ausgelöst

In Schritt2 fordert erneut das ObjectDataSource alle Daten aus der Datenquelle. Diese Art von Auslagerung wird häufig als bezeichnet *Standardnavigation*, wie er die einheitlicheres Paginierungsverhalten verwendet, wird standardmäßig beim Festlegen der `AllowPaging` Eigenschaft `true`. Hat Standardwert Ruft die Daten Websteuerelement naiv paging alle Datensätze für jede Seite der Daten ab, obwohl nur eine Teilmenge der Datensätze werden gerendert, in der HTML-Code, s, die an den Browser gesendet. Wenn die Daten der Datenbank vom BLL oder ObjectDataSource zwischengespeichert werden, wird die Standardnavigation nicht betriebsbereiten für ausreichend große Resultsets oder Webanwendungen mit vielen gleichzeitigen Benutzern.

In den nächsten Lernprogrammen untersuchen wir zum Implementieren *benutzerdefiniertes Paging*. Insbesondere können das ObjectDataSource abzurufenden, dass nur den genauen Satz von Datensätzen, die für die angeforderte Seite der Daten erforderlichen anweisen, mit benutzerdefinierten Paging. Können sich sicher, wird die Effizienz beim Paging durch große Resultsets in benutzerdefiniertes Paging erheblich verbessert.

> [!NOTE]
> Während die Standardnavigation nicht geeignet ist, wenn ausreichend große Resultsets oder Websites mit vielen gleichzeitigen Benutzern paging, beachten Sie, dass benutzerdefiniertes Paging erfordert weitere Änderungen und der Aufwand verringert, implementiert und nicht so einfach ist wie das Überprüfen ein Kontrollkästchen (als Standardeinstellung die Auslagerungsdatei). Aus diesem Grund möglicherweise Standardnavigation die optimale Wahl für kleine, mit geringem Datenverkehr Websites oder wenn Paging für relativ kleine Resultsets legt fest, wie sie s wesentlich einfacher und schneller zu implementieren.


Wenn bekannt ist, dass wir in unserer Datenbank nie mehr als 100 Produkte haben, wird der minimale Leistungsgewinn durch benutzerdefinierte Paging gefallen hat z. B. wahrscheinlich von der Aufwand zur Implementierung versetzt. Wenn wir einen Tag zu mehreren Tausend oder Zehntausenden von Produkten, haben jedoch möglicherweise *nicht* implementieren benutzerdefiniertes Paging würde die Skalierbarkeit der Anwendung erheblich behindern.

## <a name="step-4-customizing-the-paging-experience"></a>Schritt 4: Anpassen der Auslagerungsdatei

Die Web-Datensteuerelemente bieten eine Reihe von Eigenschaften, die verwendet werden kann, um das Benutzer s Paging zu verbessern. Die `PageCount` -Eigenschaft, z. B. gibt an, wie viele Seiten insgesamt vorhanden sind, während die `PageIndex` Eigenschaft gibt die aktuelle Seite aufgerufen wird, und kann festgelegt werden, um einen Benutzer zu einer bestimmten Seite schnell zu verschieben. Um zu veranschaulichen, wie mit diesen Eigenschaften können nach der erforderlichen Erfahrungsgrad des Benutzers s-Paging verbessert werden, können eine Bezeichnung hinzufügen s-Websteuerelement auf unserer Seite, mit die dem Benutzer auf welcher Seite informiert sie re gerade besuchen, zusammen mit einer DropDownList-Steuerelement, das ermöglicht ihnen, schnell zu einer bestimmten Assistentenseite zu wechseln .

Zunächst ein Websteuerelement Bezeichnung der Seite hinzufügen, legen Sie seine `ID` Eigenschaft `PagingInformation`, und löschen, dessen `Text` Eigenschaft. Als Nächstes erstellen Sie einen Ereignishandler für die GridView s `DataBound` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Dieser Ereignishandler weist die `PagingInformation` Bezeichnung s `Text` Eigenschaft mit einer Meldung informiert die Benutzer die Seite, die gerade besuchen `Products.PageIndex + 1` Out wie viele Seiten insgesamt `Products.PageCount` (wir 1 zum Hinzufügen der `Products.PageIndex` Eigenschaft da `PageIndex` ist indiziert, beginnend mit 0). Ich Zuweisen dieser Bezeichnung s ausgewählt haben `Text` Eigenschaft in der `DataBound` -Ereignishandler im Gegensatz zu der `PageIndexChanged` Ereignishandler da die `DataBound` Ereignis ausgelöst wird, jedes Mal, wenn Daten an die GridView gebunden werden, während die `PageIndexChanged` Ereignishandler nur wird ausgelöst, wenn der Seitenindex geändert wird. Besuchen Sie bei die GridView anfänglich auf der ersten Seite gebundene Daten darstellt, die `PageIndexChanging` Ereignis verfügt über t auslösen (während der `DataBound` Ereignis ist).

Durch diese hinzufügen wird der Benutzer nun angezeigt eine Nachricht, der angibt, welcher Seite, die sie besucht werden und wie viele Gesamtseitenzahl Daten vorhanden sind.


[![Die aktuelle Seitenzahl und die Anzahl der Seiten gesamt werden angezeigt.](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Abbildung 10**: die aktuelle Seitenzahl und die Anzahl der Seiten gesamt werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image20.png))


Zusätzlich zum Label-Steuerelement können Sie auch ein DropDownList-Steuerelement hinzufügen, die die Seitenzahlen der GridView mit ausgewählten aktuell angezeigten Seite listet s ein. Der Grundgedanke besteht, dass der Benutzer schnell aus der aktuellen Seite zu einem anderen springen kann, indem Sie einfach den neuen Index für die Seite aus der Dropdownliste auswählen. Starten, indem Sie im Designer festlegen eine DropDownList hinzugefügt seine `ID` Eigenschaft `PageList` und überprüfen die Option AutoPostBack aktivieren von smart Tag.

Als Nächstes zurück zu den `DataBound` -Ereignishandler und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Dieser Code beginnt durch Deaktivieren der Elemente in der `PageList` DropDownList. Das mag überflüssig, da eine ausreichen t erwarten, dass die Anzahl der Seiten ändern, aber andere Benutzer können das System gleichzeitig zu verwenden, hinzufügen oder entfernen Datensätze aus der `Products` Tabelle. Solche einfügungen oder Löschvorgängen konnte die Anzahl der Seiten, die Daten ändern.

Als Nächstes müssen wir die Seitenzahlen erneut erstellen und das Projekt, das die aktuelle GridView zugeordnet haben `PageIndex` standardmäßig ausgewählt. Erreichen wir mit einer Schleife von 0 bis `PageCount - 1`, Hinzufügen eines neuen `ListItem` in jeder Iteration und die Einstellung der `Selected` Eigenschaft auf "true", wenn Sie der aktuellen Iterationsindex GridView s gleich `PageIndex` Eigenschaft.

Schließlich müssen, erstellen Sie einen Ereignishandler für das DropDownList s `SelectedIndexChanged` Ereignis, das ausgelöst wird, jedes Mal, die der Benutzer auswählen, ein anderes Element aus der Liste. Um Ereignishandler zu erstellen, einfach Doppelklicken Sie auf der DropDownList im Designer, und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Wie in Abbildung 11 gezeigt, ändern Sie lediglich die GridView s `PageIndex` -Eigenschaft bewirkt, dass die Daten erneut an die GridView gebunden werden. In der GridView s `DataBound` Ereignishandler, d. h. der entsprechenden DropDownList `ListItem` ausgewählt ist.


[![Der Benutzer wird automatisch ausgeführt, um die sechste Seite beim Auswählen der Seite 6 Dropdown-Liste-Element](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Abbildung 11**: der Benutzer wird automatisch ausgeführt, um die sechste Seite beim Seite 6 Dropdown-Liste Sie das Element auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Schritt 5: Hinzufügen von bidirektionalen Sortierungsunterstützung

Hinzufügen von bidirektionalen sortierungsunterstützung ist so einfach wie das Hinzufügen der Unterstützung der Paginierung einfach das Kontrollkästchen Sortieren aktivieren aus dem GridView-s-Smarttag (wodurch die GridView s [ `AllowSorting` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) auf `true`). Dies rendert die Header der GridView s Felder als LinkButtons, die beim Klicken auf, dazu führen, dass einen Postback und die Daten nach der Spalte in aufsteigender Reihenfolge sortiert zurück. Die Daten in absteigender Reihenfolge beim erneuten Klicken auf den gleichen Header LinkButton neu sortiert werden.

> [!NOTE]
> Wenn Sie eine benutzerdefinierte Datenzugriffsschicht statt eines typisierten Datasets verwenden, haben Sie keine Möglichkeit Sortieren aktivieren im GridView s Smarttag. Nur GridViews an Datenquellen, die systemeigene Unterstützung für Sortierung gebunden haben, dieses Kontrollkästchen verfügbar. Das typisierte DataSet bietet Out-of-Box-sortierungsunterstützung, da ADO.NET DataTable bietet eine `Sort` Methode, die beim Aufrufen, sortiert die DataTable s DataRows mit den angegebenen Kriterien.


Falls die DAL keinen Wert Objekte, die systemintern sortiert Sortierung unterstützen zurückgibt, müssen Sie konfigurieren das ObjectDataSource um Sortierinformationen an die Geschäftslogikschicht übergeben, die Daten sortiert oder die Daten können, von der DAL. Genauer Gewusst wie: Sortieren von Daten auf die Geschäftslogik und Datenzugriffsebenen Daten in einem späteren Lernprogramm.

Sortierung LinkButtons werden als HTML-Hyperlinks gerendert, dessen aktuelle Farben (für einen nicht besuchter Link und Dunkelrot für ein bereits besuchter Link Blau) mit der Hintergrundfarbe der Kopfzeile miteinander in Konflikt geraten. Stattdessen können s haben alle Header Zeile Links weiß, unabhängig davon, ob sie Ve wurde besucht hat oder nicht. Dies kann durch Hinzufügen der folgenden Optionen, um die `Styles.css` Klasse:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Diese Syntax gibt an, dass weißen Text verwenden, wenn diese Links in einem Element anzeigen, die die HeaderStyle-Klasse verwendet.

Nach der CSS-Code Wenn der Zugriff auf die Seite über einen Browser sollte am Bildschirm ähnelt der Abbildung 12. Abbildung 12 zeigt insbesondere, die Ergebnisse nach dem Preis s-Header auf den Link geklickt wurde.


[![Die Ergebnisse sind nach UnitPrice in aufsteigender Reihenfolge sortiert wurden](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Abbildung 12**: die Ergebnisse haben nach UnitPrice in aufsteigender Reihenfolge sortiert wurden ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Untersuchen die Sortierung Workflow

Alle GridView Felder, die BoundField-, CheckBoxField, TemplateField und usw. haben eine `SortExpression` -Eigenschaft, die der Ausdruck gibt an, die verwendet werden soll, um die Daten zu sortieren, wenn s Feld Sortieren Header Link geklickt wird. Die GridView verfügt auch über eine `SortExpression` Eigenschaft. Beim Sortieren Überschrift LinkButton geklickt wird, weist die GridView Feld s `SortExpression` -Wert an die `SortExpression` Eigenschaft. Als Nächstes die Daten aus der ObjectDataSource erneut abgerufen und entsprechend der GridView s sortiert `SortExpression` Eigenschaft. In der folgenden Liste wird erläutert, die Abfolge der Schritte, die herausstellt, wenn ein Endbenutzer die Daten in einem GridView sortiert werden:

1. Die GridView s [Sorting-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) ausgelöst wird
2. Die GridView s [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) festgelegt ist, um die `SortExpression` des Felds, deren Sortierung Header LinkButton geklickt wurde
3. Das ObjectDataSource werden alle Daten aus der BLL erneut abgerufen und sortiert dann die Daten mithilfe der GridView-s `SortExpression`
4. Die GridView s `PageIndex` Eigenschaft auf 0 zurückgesetzt, was bedeutet, dass beim Sortieren des Benutzers wird zurückgegeben, zur ersten Seite der Daten (vorausgesetzt, Unterstützung der Paginierung implementiert wurde)
5. Die GridView s [ `Sorted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) ausgelöst wird

Wie bei der Standardnavigation, sortieren die Option Standard erneut abgerufen *alle* Datensätze aus der BLL. Bei Verwendung von Sortierung ohne Auslagerung oder bei Verwendung der Sortierung mit Standard-Paging, dort s keine Möglichkeit, diese Beeinträchtigung (nicht genügend Zwischenspeichern von Daten in der Datenbank) zu umgehen. Jedoch, wie es in einem späteren Lernprogramm sehen sie s möglich, Daten effizient zu sortieren, wenn benutzerdefiniertes Paging verwendet.

Beim Binden von einem ObjectDataSource an die GridView über die Dropdownliste in der GridView-s-Smarttag jedes Feld GridView verfügt automatisch über seine `SortExpression` Eigenschaft zugewiesen. der Name des Datenfelds in der `ProductsRow` Klasse. Z. B. die `ProductName` BoundField-s `SortExpression` festgelegt ist, um `ProductName`, wie in der folgenden deklarativem Markup dargestellt:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Ein Feld kann so konfiguriert werden, damit er von gelöscht wird, nicht sortierbar s seine `SortExpression` Eigenschaft (zuweisen auf eine leere Zeichenfolge). Um dies zu veranschaulichen, stellen Sie sich vor, die wir t können unseren Kunden unsere Produkte nach dem Preis sortiert werden soll. Die `UnitPrice` BoundField-s `SortExpression` Eigenschaft von deklarativem Markup oder über die Felder (Dialogfeld) (, zugegriffen werden kann durch Klicken auf den Link "Spalten bearbeiten" in der GridView-s-Smarttag) entfernt werden kann.


![Die Ergebnisse sind nach UnitPrice in aufsteigender Reihenfolge sortiert wurden](paging-and-sorting-report-data-vb/_static/image27.png)

**Abbildung 13**: die Ergebnisse haben anhand der UnitPrice in aufsteigender Reihenfolge sortiert wurden


Einmal die `SortExpression` Eigenschaft entfernt wurde, für die `UnitPrice` BoundField, als Text und nicht als Link, um Benutzer aus der Sortierung der Daten nach dem Preis zu verhindern, wird der Seitenkopf gerendert.


[![Entfernen Sie die SortExpression-Eigenschaft, können Benutzer nicht mehr die Produkte nach dem Preis sortieren](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Abbildung 14**: durch das Entfernen der SortExpression-Eigenschaft, können Benutzer nicht mehr der Produkte von Preis sortieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Programmgesteuertes Sortieren von GridView

Sie können den Inhalt der GridView auch programmgesteuert sortieren, indem Sie mit der GridView s [ `Sort` Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Übergeben Sie einfach die `SortExpression` Wert Sortierungskriterium zusammen mit den [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` oder `Descending`), und die GridView-s-Daten neu sortiert werden.

Stellen Sie sich vor, dass die Ursache, die wir Sortieren nach deaktiviert die `UnitPrice` wurde, weil sich Sorgen, dass unsere Kunden einfach nur die niedrigsten Preis Produkte kaufen würde. Allerdings möchten wir, dass sie die teuersten Produkte zu kaufen, sodass d wir sie in der Lage, die Produkte nach dem Preis, jedoch nur von dem die teuersten Preis den kleinsten sortiert sein wie Ihre.

Durchführung dadurch ein Websteuerelement Schaltfläche hinzugefügt, auf der Seite festlegen seiner `ID` Eigenschaft, um `SortPriceDescending`, und die zugehörige `Text` Eigenschaft sortieren nach dem Preis. Als Nächstes erstellen Sie einen Ereignishandler für die Schaltfläche "s" `Click` Ereignis durch Doppelklicken auf das Schaltflächen-Steuerelement im Designer. Fügen Sie an diesen Ereignishandler den folgenden Code hinzu:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Auf diese Schaltfläche klicken, gibt den Benutzer zur ersten Seite mit den Produkten, sortiert nach dem Preis, aus aufwändigsten auf günstigere (siehe Abbildung 15) zurück.


[![Die Produkte anhand der aufwändigsten klicken auf die Schaltfläche Aufträge auf der geringsten](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Abbildung 15**: die Produkte aus der die teuersten den kleinsten sortiert durch Klicken auf die Schaltfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie paging und Sortieren Funktionen implementieren, wurden beide so einfach wie ein Kontrollkästchen überprüfen! Wenn ein Benutzer sortiert oder durch die Daten Seiten, erweitert ein ähnlicher Workflow:

1. Ein Postback erfolgt.
2. Die Daten Websteuerelement s Ebene vorab-Ereignis ausgelöst (`PageIndexChanging` oder `Sorting`)
3. Alle Daten erneut abgerufen, indem die ObjectDataSource
4. Die Daten Websteuerelement s Ebene nach dem Ereignis ausgelöst wird (`PageIndexChanged` oder `Sorted`)

Implementieren grundlegende Paging und Sortieren ist ein Kinderspiel muss mehr Aufwand zu nutzen, die eine effizientere benutzerdefinierte Paging oder zum Optimieren der Auslagerungsdatei oder Sortierung Schnittstelle ausgeübt. Zukünftige Lernprogrammen werden die folgenden Themen erkunden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](creating-a-customized-sorting-user-interface-cs.md)
> [Weiter](efficiently-paging-through-large-amounts-of-data-vb.md)
