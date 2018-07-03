---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Auslagern und Sortieren von Berichtsdaten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Paginierung und Sortierung sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. In diesem Tutorial werden wir einen ersten Blick auf das Hinzufügen einer Sortierung dauern und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 75a2206bf3db3af8859fe4de58f67135d31bba0f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401921"
---
<a name="paging-and-sorting-report-data-c"></a>Auslagern und Sortieren von Berichtsdaten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) oder [PDF-Datei herunterladen](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Paginierung und Sortierung sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. In diesem Tutorial werden wir einen ersten Blick auf Hinzufügen, Sortieren und paging auf unsere Berichte, die wir dann nach in zukünftigen Lernprogrammen erstellen, nutzen.


## <a name="introduction"></a>Einführung

Paginierung und Sortierung sind zwei sehr allgemeine Funktionen zum Anzeigen von Daten in einer online-Anwendung. Z. B. bei der Suche nach ASP.NET Bücher im Onlinebuchhandel gibt es möglicherweise Hunderte von Büchern, aber der Bericht mit den Suchergebnissen werden nur zehn Übereinstimmungen pro Seite aufgeführt. Darüber hinaus können die Ergebnisse nach Titel, Preis, Seitenanzahl, Name des Autors und So weiter sortiert werden. Während der letzten 23 Tutorials haben Gewusst wie: Erstellen Sie eine Vielzahl von Berichten, einschließlich der Schnittstellen, mit denen hinzufügen, bearbeiten und Löschen von Daten, überprüft es speichern, die nicht erläutert, wie zum Sortieren von Daten und die einzige Beispiele für paging wir Ve gesehen wurden mit DetailsView und FormView -Steuerelemente.

In diesem Tutorial erfahren wir, wie Sortierung und paging auf unsere Berichte, die ausgeführt werden können, durch einfaches aktivieren einige Kontrollkästchen hinzufügen. Leider wird dieser vereinfachten Implementierung hat auch Nachteile haben, die die Sortierung Schnittstelle ein wenig zu wünschen übrig lässt und die Paging-Routinen sind nicht für effizientes Auslagern von großen Resultsets konzipiert. Zukünftige wird wie die Einschränkungen von der Out-von-the-Box Auslagern und Sortieren von Lösungen zu überwinden Tutorials werden.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>Schritt 1: Hinzufügen von Paging und Sortieren von Tutorial Webseiten

Bevor wir in diesem Tutorial beginnen, können Sie zuerst nehmen einen Moment Zeit, um die ASP.NET-Seiten hinzuzufügen, benötigen wir für dieses Lernprogramm sowie die folgenden drei s ein. Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `PagingAndSorting`. Fügen Sie die folgenden fünf ASP.NET-Seiten in diesen Ordner, wenn all diese Einstellungen zu konfigurieren, um die Masterseite verwenden `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![Erstellen Sie einen Ordner PagingAndSorting, und fügen Sie die Tutorial ASP.NET-Seiten](paging-and-sorting-report-data-cs/_static/image1.png)

**Abbildung 1**: Erstellen Sie einen Ordner PagingAndSorting, und fügen Sie die Tutorial ASP.NET-Seiten


Öffnen Sie als Nächstes die `Default.aspx` Seite, und ziehen Sie die `SectionLevelTutorialListing.ascx` Benutzersteuerelement aus der `UserControls` Ordner auf die Entwurfsoberfläche. Dieses Benutzersteuerelement, die wir in den erstellt die [Masterseiten und Sitenavigation](../introduction/master-pages-and-site-navigation-cs.md) Tutorial, listet die Sitemap und zeigt diese Tutorials im aktuellen Abschnitt in einer Liste mit Aufzählungszeichen.


![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](paging-and-sorting-report-data-cs/_static/image2.png)

**Abbildung 2**: Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"


Um die Liste mit Aufzählungszeichen angezeigt, die Paginierung und Sortierung von Tutorials, die wir erstellen, müssen wir diese Siteübersicht hinzufügen. Öffnen der `Web.sitemap` Datei, und fügen Sie das folgende Markup nach dem vom Markup bearbeiten, einfügen und löschen Site Map, Knoten:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten](paging-and-sorting-report-data-cs/_static/image3.png)

**Abbildung 3**: Aktualisieren Sie die Website-Karte, um die neuen ASP.NET-Seiten enthalten


## <a name="step-2-displaying-product-information-in-a-gridview"></a>Schritt 2: Anzeigen von Produktinformationen in einer GridView-Ansicht

Bevor wir tatsächlich implementieren Auslagern und Sortieren von Funktionen, können Sie s, erstellen Sie zunächst eine standardmäßige nicht-Srotable, nicht auslagerbaren GridView, die die Produktinformationen enthält. Diese Aufgabe ist es häufig vor der in dieser tutorialreihe also diese Schritte ausgeführt haben vertraut sein. Öffnen Sie zunächst die `SimplePagingSorting.aspx` Seite, und ziehen Sie ein GridView-Steuerelement aus der Toolbox auf den Designer, Festlegen der `ID` Eigenschaft `Products`. Als Nächstes erstellen Sie eine neue "ObjectDataSource", die die Klasse ProductsBLL s verwendet `GetProducts()` Methode, um alle Produktinformationen, zurückzugeben.


![Abrufen von Informationen zu allen Produkten mithilfe der GetProducts()-Methode](paging-and-sorting-report-data-cs/_static/image4.png)

**Abbildung 4**: Abrufen von Informationen zu allen Produkten mithilfe der GetProducts()-Methode


Da in diesem Bericht kein schreibgeschützter Bericht, der es s ist das "ObjectDataSource"-s zuordnen müssen `Insert()`, `Update()`, oder `Delete()` Methoden, um die entsprechenden `ProductsBLL` -Methoden (keine) daher aus der Dropdown-Liste auswählen, für das UPDATE, INSERT, Registerkarten "" und löschen.


![Wählen Sie die (None) Option in der Dropdown-Liste in der Update-, INSERT- und Löschen von Registerkarten](paging-and-sorting-report-data-cs/_static/image5.png)

**Abbildung 5**: Wählen Sie die (None) Option in der Dropdown-Liste in der Update-, INSERT- und Löschen von Registerkarten


Als Nächstes können Sie s GridView s Felder anpassen, sodass nur die Produktnamen, Lieferanten, Kategorien, Preise und nicht mehr unterstützte Status angezeigt werden. Darüber hinaus können alle Feldebene Formatierungsänderungen vorzunehmen ändert, z. B. zum Anpassen der `HeaderText` Eigenschaften oder formatieren den Preis als Währung. Nach diesen Änderungen sollte Ihre GridView s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Abbildung 6 zeigt unseren Fortschritt bisher ein, wenn Sie über einen Browser angezeigt. Beachten Sie, dass die Seite zeigt eine Liste aller Produkte in einem Bildschirm mit einer jeden Produktname s, Kategorie, Lieferanten, Preis, und nicht mehr Status unterstützte.


[![Jedes der Produkte werden aufgelistet](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Abbildung 6**: jedes der Produkte aufgeführt sind ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>Schritt 3: Hinzufügen von Pagingunterstützung

Auflisten von *alle* Produkte auf einem Bildschirm Informationsflut für den Benutzer zum Durchlesen der das führen kann. Damit werden die Ergebnisse übersichtlicher zu machen, können wir teilen Sie die Daten in kleinere Seiten mit Daten und ermöglicht dem Benutzer, die eine Seite mit Daten zu einem Zeitpunkt durchlaufen. Zum Ausführen dieser einfach das Kontrollkästchen Paging aktivieren aus dem GridView-s-Smarttag (Dadurch wird das GridView-s [ `AllowPaging` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) zu `true`).


[![Aktivieren Sie das Kontrollkästchen aktivieren Paging, zum Hinzufügen von Paging-Unterstützung](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Abbildung 7**: das Kontrollkästchen aktivieren Sie Paging, Paging-Unterstützung hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image11.png))


Aktivieren von Paging begrenzt die Anzahl der Datensätze pro Seite angezeigt wird, und fügt eine *Auslagerungsschnittstelle* an die GridView. Die Auslagerung Standardschnittstelle, dargestellt in Abbildung 7 ist eine Reihe von Seitenzahlen, damit der Benutzer schnell von einer Seite mit Daten in einen anderen zu navigieren. Diese Schnittstelle Paging sollte Ihnen bekannt, wie wir haben gesehen, wenn die DetailsView und FormView-Steuerelemente in den letzten Tutorials Paging-Unterstützung hinzugefügt.

Sowohl die FormView-Steuerelement die DetailsView-Steuerelemente zeigen nur einen einzelnen Datensatz pro Seite. GridView jedoch berät seine [ `PageSize` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) um zu bestimmen, wie viele Datensätze pro Seite angezeigt (diese Eigenschaft hat einen Standardwert von 10).

Diese Schnittstelle GridView, DetailsView oder FormView s Paging kann mithilfe der folgenden Eigenschaften angepasst werden:

- `PagerStyle` Gibt die Formatinformationen für die Paging-Schnittstelle an. Geben Sie die Einstellungen wie können `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`und so weiter.
- `PagerSettings` enthält eine Reihe von Eigenschaften, die die Funktionalität der Auslagerungsschnittstelle anpassen können. `PageButtonCount` gibt die maximale Anzahl der numerischen Seitenzahlen, die in den Paging-Schnittstelle (der Standardwert ist 10) angezeigt; die [ `Mode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) gibt an, wie die Paging-Schnittstelle arbeitet, und kann so festgelegt werden: 

    - `NextPrevious` Zeigt ein Schaltflächen Weiter und zurück, damit der Benutzer eine Seite vorwärts oder rückwärts zu einem Zeitpunkt zu Schritt
    - `NextPreviousFirstLast` Zusätzlich zu die Schaltflächen Weiter und zurück sind auch erste und letzte Schaltflächen enthalten, damit der Benutzer schnell in der ersten oder letzten Seite der Daten verschieben
    - `Numeric` Zeigt eine Reihe von Seitenzahlen, damit der Benutzer sofort auf einer beliebigen Seite wechseln
    - `NumericFirstLast` neben der Seitenzahlen enthält erste und letzte Schaltflächen, damit der Benutzer schnell in der ersten oder letzten Seite der Daten zu verschieben; die ersten/letzten Schaltflächen werden nur angezeigt, wenn alle numerischen Seitenzahlen nicht geeignet

Darüber hinaus die GridView, DetailsView und FormView-Steuerelement alle bieten die `PageIndex` und `PageCount` Eigenschaften, die die aktuelle Seite, die angezeigt wird und die Gesamtzahl der Seiten mit Daten, Serveroptionsparameter anzugeben. Die `PageIndex` -Eigenschaft ist indiziert, beginnend mit 0, was bedeutet, dass beim Anzeigen der ersten Seite des Data `PageIndex` ist gleich 0. `PageCount`, auf der anderen Seite beginnt Zähler bei 1, was bedeutet, dass `PageIndex` ist auf den Werten zwischen 0 und `PageCount - 1`.

Lassen Sie s, die die standarddarstellung der unsere GridView-s-Auslagerungsschnittstelle verbessert werden. Können Sie insbesondere s haben die Paging-Schnittstelle, die mit einem hellgrauen Hintergrund rechtsbündig ausgerichtet. Statt diese Eigenschaften direkt über das GridView-s `PagerStyle` -Eigenschaft können s erstellen Sie eine CSS-Klasse in `Styles.css` mit dem Namen `PagerRowStyle` und weisen Sie dann die `PagerStyle` s `CssClass` Eigenschaft über unser Design. Öffnen Sie zunächst `Styles.css` und Definition der Klasse das folgende CSS hinzufügen:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Öffnen Sie als Nächstes die `GridView.skin` Datei die `DataWebControls` Ordner innerhalb der `App_Themes` Ordner. Wie wir unter den *Masterseiten und Sitenavigation* Lernprogramm, Designmodus Dateien können verwendet werden, um die Standardwerte für die Eigenschaft für ein Steuerelement anzugeben. Aus diesem Grund ergänzen die vorhandenen Einstellungen aus, um die Einstellung der `PagerStyle` s `CssClass` Eigenschaft `PagerRowStyle`. Darüber hinaus können s konfigurieren Sie die Paging-Schnittstelle auf höchstens fünf numerische Schaltflächen mit Anzeigen der `NumericFirstLast` Auslagerungsschnittstelle.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Die Paging-Benutzeroberfläche

Abbildung 8 zeigt die Webseite, wenn über einen Browser aufgerufen werden soll, nachdem die GridView s Paging aktivieren das Kontrollkästchen markiert ist und die `PagerStyle` und `PagerSettings` Konfigurationen wurden durch die `GridView.skin` Datei. Hinweis wie nur zehn Datensätze werden angezeigt, und die Paging-Schnittstelle gibt an, dass wir die erste Seite der Daten angezeigt werden.


[![Mit Paging aktiviert ist werden nur eine Teilmenge der Datensätze zu einem Zeitpunkt angezeigt](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Abbildung 8**: mit Paging aktiviert, nur eine Teilmenge der Datensätze zu einem Zeitpunkt angezeigt werden ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image14.png))


Klickt der Benutzer auf eine Seite Zahlen in den Paging-Schnittstelle, erfolgt ein Postback aus, und die Seite lädt angezeigt, die Datensätze der Seite "s" angefordert. Abbildung 9 zeigt die Ergebnisse nach der Aktivierung, um die letzte Seite der Daten anzuzeigen. Beachten Sie, dass die letzte Seite nur einem Datensatz; Dies ist da 81 Datensätze vorhanden sind, insgesamt acht Seiten der 10 Datensätze pro Seite plus eine Seite mit einem Datensatz einzelne führt.


[![Durch Klicken auf eine Seitenzahl ein Postback auslöst, und zeigt einen entsprechenden Teil der Datensätze](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Abbildung 9**: durch Klicken auf eine Seitenzahl ein Postback auslöst, und zeigt die entsprechenden Teilmenge der Datensätze ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Paging s serverseitige Workflow

Klickt der Endbenutzer auf eine Schaltfläche in der Auslagerungsdatei-Schnittstelle, ein Postback erfolgt und im folgende serverseitigen-Workflow beginnt:

1. Das GridView-s (oder DetailsView oder FormView) `PageIndexChanging` -Ereignis ausgelöst
2. Dem ObjectDataSource-Steuerelement erneut anfordert *alle* der Daten aus der BLL; das GridView-s `PageIndex` und `PageSize` Eigenschaftswerte werden verwendet, um zu bestimmen, was der BLL zurückgegebenen Datensätze in den GridView-Ansicht angezeigt werden müssen
3. Das GridView-s `PageIndexChanged` -Ereignis ausgelöst

In Schritt2 fordert erneut dem ObjectDataSource-Steuerelement alle Daten aus der Datenquelle. Diese Art von Auslagerung wird häufig als bezeichnet *Standardpaging*, wie sie s das auslagerungsverhalten verwendet standardmäßig beim Festlegen der `AllowPaging` Eigenschaft `true`. Standardwert ruft naiv Auslagern von der Daten-Websteuerelement alle Datensätze für jede Seite der Daten, obwohl nur eine Teilmenge der Datensätze sind tatsächlich dargestellt in der HTML-Code, s, die an den Browser gesendet. Wenn die Daten für die von der BLL oder "ObjectDataSource" zwischengespeichert werden, ist das Standardpaging betriebsbereiten bei ausreichend großen Resultsets oder für Webanwendungen mit vielen gleichzeitigen Benutzern.

Im nächsten Tutorial untersuchen wir implementieren *benutzerdefiniertes Paging*. Mit benutzerdefiniertem Paging können Sie insbesondere dem ObjectDataSource-Steuerelement den genauen Satz von Datensätzen, die für die angeforderte Seite der Daten erforderlich sind nur abrufen anweisen. Sich sicher vorstellen können, wird die Effizienz von Paging für große Resultsets in benutzerdefiniertes Paging erheblich verbessert.

> [!NOTE]
> Während das Standardpaging nicht geeignet ist, wenn ausreichend große Resultsets oder Websites mit vielen gleichzeitigen Benutzern Auslagerung, beachten Sie, dass benutzerdefinierte Paginierung erfordert weitere Änderungen und Aufwand zum Implementieren und nicht so einfach ist wie das Aktivieren eines Kontrollkästchens, (wie der Standardwert ist Paging) auf. Aus diesem Grund das Standardpaging ist möglicherweise die ideale Wahl für kleine, mit geringer Auslastung Websites oder wenn Paging für relativ kleine Resultsets legt fest, wie sie s sehr viel einfacher und schneller zu implementieren.


Wenn wir wissen, dass wir in unserer Datenbank nie mehr als 100 Produkte haben, ist der minimale Leistungsgewinn zusammenarbeitsmöglichkeiten benutzerdefiniertes Paging z. B. wahrscheinlich von den für ihre Implementierung erforderliche Aufwand versetzt. Wenn Sie jedoch, wir eines Tages mehreren Tausend oder zehntausend von Tausenden von Produkten, haben möglicherweise *nicht* benutzerdefinierte Paginierung implementieren würde erheblich behindern die Skalierbarkeit der Anwendung.

## <a name="step-4-customizing-the-paging-experience"></a>Schritt 4: Anpassen der Paginierung

Die datenwebsteuerelementen bieten eine Reihe von Eigenschaften, die verwendet werden können, um die Benutzer s Paging zu verbessern. Die `PageCount` -Eigenschaft, z. B. gibt an, wie viele Seiten gesamte vorhanden sind, während die `PageIndex` Eigenschaft gibt an der aktuellen Seite aufgerufen wird, und kann festgelegt werden, um einen Benutzer zu einer bestimmten Seite schnell zu verschieben. Um zu veranschaulichen, wie Sie mit diesen Eigenschaften können nach der Paginierung benutzerfreundlichkeit s verbessert werden, können eine Bezeichnung hinzufügen s Web Steuern auf unserer Seite, die die Benutzer darüber informiert, welche Seite wird sie erneut derzeit das Unternehmen besuchen, zusammen mit einem DropDownList-Steuerelement, das schnell zu einer Seite zu springen ermöglicht .

Zunächst ein Label-Steuerelement auf Ihrer Seite hinzufügen, legen Sie seine `ID` Eigenschaft `PagingInformation`, und deaktivieren Sie die `Text` Eigenschaft. Als Nächstes erstellen Sie einen Ereignishandler für das GridView-s `DataBound` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Dieser Ereignishandler weist die `PagingInformation` Bezeichnung s `Text` Eigenschaft, um eine Nachricht informiert den Benutzer die Seite, die sie gerade besuchen `Products.PageIndex + 1` aus wie vielen Gesamtseitenzahl `Products.PageCount` (Vertriebsdatenpunkt auf 1 fest, um die `Products.PageIndex` Eigenschaft da `PageIndex` wird indiziert, beginnend mit 0). Ich habe der Zuweisen dieser Bezeichnung s `Text` -Eigenschaft in der `DataBound` -Ereignishandler im Gegensatz zu den `PageIndexChanged` -Ereignishandler da die `DataBound` Ereignis ausgelöst wird, jedes Mal, wenn Daten an die GridView gebunden werden, während die `PageIndexChanged` Ereignishandler nur wird ausgelöst, wenn der Seitenindex geändert wird. Besuchen Sie die Wenn GridView zunächst Daten auf der ersten Seite gebunden ist, die `PageIndexChanging` Ereignis t ausgelöst (während der `DataBound` -Ereignis erledigt).

Durch diese hinzufügen wird der Benutzer jetzt eine Meldung angezeigt, der angibt, welcher Seite, die sie besuchen und wie viele Gesamtseitenzahl Daten vorhanden sind.


[![Die aktuelle Seitenzahl und die Anzahl der Seiten gesamt werden angezeigt.](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Abbildung 10**: die aktuelle Seitenzahl und die gesamte Anzahl der Seiten werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image20.png))


Können Sie neben das Bezeichnungssteuerelement s, die auch ein DropDownList-Steuerelement hinzufügen, die die Seitennummern in den GridView-Ansicht mit der aktuell angezeigten Seite ausgewählt werden aufgelistet. Die Idee dabei ist, dass der Benutzer schnell von der aktuellen Seite wechseln kann indem Sie einfach den neuen Index für die Seite aus der Dropdownliste auswählen. Starten, indem Sie das Hinzufügen von einem DropDownList-Steuerelement, zum Festlegen der `ID` Eigenschaft `PageList` und überprüfen die Option "AutoPostBack aktivieren" smart Tag.

Als Nächstes zurück zu den `DataBound` -Ereignishandler, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Dieser Code beginnt, durch die Elemente im beseitigen der `PageList` DropDownList. Dies mag überflüssig, da eine ausreichen t erwarten, dass die Anzahl der Seiten ändern, aber anderen Benutzern können das System gleichzeitig zu verwenden, hinzufügen oder entfernen Datensätze aus der `Products` Tabelle. Die Anzahl der Seiten mit Daten können geändert werden solche einfügungen oder löschungen.

Als Nächstes müssen wir erneut, die Seitenzahlen zu erstellen und verwenden diejenige, die an die aktuelle GridView ordnet `PageIndex` standardmäßig ausgewählt. Wir erreichen dies mit einer Schleife von 0 bis `PageCount - 1`, Hinzufügen eines neuen `ListItem` in jeder Iteration und die Einstellung der `Selected` Eigenschaft auf "true", der aktuellen Iterationsindex GridView s gleich `PageIndex` Eigenschaft.

Abschließend müssen wir erstellen einen Ereignishandler für das DropDownList-s `SelectedIndexChanged` Ereignis, das ausgelöst wird jedes Mal, die der Benutzer wählen Sie ein anderes Element aus der Liste. Um diesen Ereignishandler zu erstellen, einfach Doppelklicken Sie auf der Dropdownliste im Designer, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Wie in Abbildung 11 gezeigt, ändern Sie lediglich die GridView s `PageIndex` -Eigenschaft bewirkt, dass die Daten erneut an die GridView gebunden werden. In den GridView-s `DataBound` -Ereignishandler der entsprechenden Dropdownliste `ListItem` ausgewählt ist.


[![Der Benutzer ist die sechste Seite beim Auswählen der Element "Page" 6 Dropdown-Liste automatisch ausgeführt.](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Abbildung 11**: der Benutzer ist die sechste Seite beim Auswählen der Element "Page" 6 Dropdown-Liste automatisch ausgeführt ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>Schritt 5: Hinzufügen von Unterstützung der Datenquellensortierung bidirektionale

Überprüfen Sie die Option "Sortieren aktivieren" das GridView-s-Smarttag einfach hinzufügen der Unterstützung der datenquellensortierung bidirektionale ist so einfach wie das Hinzufügen von pagingunterstützung (wodurch das GridView-s [ `AllowSorting` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) zu `true`). Dies rendert die Header der GridView s Felder als LinkButtons, die beim Klicken auf, dazu führen, dass einen Postback aus, und die Daten nach der Spalte in aufsteigender Reihenfolge sortiert zurück. Die Daten in absteigender Reihenfolge klicken Sie erneut auf den gleichen Header LinkButton neu sortiert werden.

> [!NOTE]
> Wenn Sie eine benutzerdefinierte Datenzugriffsschicht anstelle eines typisierten Datasets verwenden, Sie keine Option zum Sortieren Aktivieren der GridView-s-Smarttag möglicherweise. Nur GridViews an Datenquellen, die native sortieren Unterstützung gebunden haben, dieses Kontrollkästchen, die zur Verfügung. Das typisierte DataSet bietet Unterstützung der datenquellensortierung Out-of-the-Box aus, da ADO.NET DataTable bietet eine `Sort` Methode, wenn aufgerufen, sortiert die DataTable s DataRows anhand der angegebenen Kriterien.


Wenn die DAL kein, Objekte, die nativ sortiert Sortierung unterstützen zurück, müssen Sie konfigurieren die "ObjectDataSource" zum Übergeben von Sortierinformationen an die Business Logic Layer, die die Daten sortieren oder die Daten können, von der DAL. Wir werden das Sortieren von Daten auf die Geschäftslogik und Datenzugriffsschichten in einem späteren Tutorial untersuchen.

Die Sortierung LinkButtons werden als HTML-Links gerendert, deren aktuellen Farben (für einen nicht besuchter Link und Dunkelrot für ein bereits besuchter Link Blau) mit der Hintergrundfarbe der Kopfzeile miteinander in Konflikt geraten. Stattdessen können s haben alle Header-Zeile-Links, die weiß, unabhängig davon, ob sie Ve wurde aufgerufen oder nicht. Dies kann erreicht werden, indem Sie Folgendes hinzufügen der `Styles.css` Klasse:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Diese Syntax gibt an, um die weißen Text verwenden, wenn diese Hyperlinks in einem Element anzeigen, die die HeaderStyle-Klasse verwendet.

Nach dem Hinzufügen dieses CSS beim Besuch der Seite über einen Browser sollte am Bildschirm ähnelt Abbildung 12. Abbildung 12 zeigt insbesondere, die Ergebnisse nach dem Preis s-Header auf den Link geklickt wurde.


[![Die Ergebnisse sind nach UnitPrice in aufsteigender Reihenfolge sortiert wurden](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Abbildung 12**: die Ergebnisse haben nach UnitPrice in aufsteigender Reihenfolge sortiert wurden ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Untersuchen die Sortierung Workflow

Alle GridView Felder, die BoundField-, CheckBoxField, TemplateField und usw. haben eine `SortExpression` -Eigenschaft, die der Ausdruck gibt an, die verwendet werden soll, die Daten sortiert werden soll, wenn dieses Feld s Header sortierlink geklickt wird. Das GridView hat auch eine `SortExpression` Eigenschaft. Wenn Sortierung Header LinkButton geklickt wird, weist die GridView, Feld s `SortExpression` -Wert in seine `SortExpression` Eigenschaft. Als Nächstes die Daten aus dem ObjectDataSource-Steuerelement erneut abgerufen und entsprechend der GridView-s `SortExpression` Eigenschaft. Die folgende Liste enthält die Abfolge der Schritte, die sich herausstellt, wenn ein Endbenutzer die Daten in einer GridView-Ansicht sortiert werden:

1. Das GridView-s [Sorting-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) ausgelöst wird
2. Das GridView-s [ `SortExpression` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) nastaven NA hodnotu der `SortExpression` des Felds, deren Sortierung Header LinkButton geklickt wurde
3. Die ObjectDataSource ruft alle Daten aus der BLL erneut ab und sortiert dann die Daten mithilfe der GridView-s `SortExpression`
4. Das GridView-s `PageIndex` Eigenschaft auf 0 zurückgesetzt, was bedeutet, dass beim Sortieren des Benutzers wird zurückgegeben, zur ersten Seite der Daten (vorausgesetzt, Unterstützung der Paginierung implementiert wurde)
5. Das GridView-s [ `Sorted` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) ausgelöst wird

Wie Sie mit Standardpaging, sortieren die Option standardmäßig erneut abgerufen *alle* Datensätze aus der Geschäftslogikschicht. Sortierung ohne Paging oder bei Verwendung der Sortierung mit standardmäßig Paging, dort s keine Möglichkeit, diese Leistung erreicht (ohne das Zwischenspeichern von Daten in der Datenbank) zu umgehen. Jedoch, in einem späteren Tutorial sehen es möglich, Daten effizient zu sortieren, wenn benutzerdefiniertes Paging zu verwenden.

Beim Binden von einem ObjectDataSource-Steuerelement an die GridView in der Dropdown-Liste in den GridView-s-Smarttag jedes GridView-Feld verfügt automatisch über die `SortExpression` -Eigenschaft zugewiesen. auf den Namen des Datenfelds in der `ProductsRow` Klasse. Z. B. die `ProductName` BoundField-s `SortExpression` nastaven NA hodnotu `ProductName`, wie im folgenden deklarativen Markup dargestellt:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Ein Feld kann so konfiguriert werden, damit sie s nicht sortierbar durch Beseitigen der `SortExpression` Eigenschaft (eine leere Zeichenfolge zuweisen). Um dies zu veranschaulichen, stellen Sie sich vor, die wir nihnen t können unsere Kunden, unsere Produkte nach Preis sortiert werden soll. Die `UnitPrice` BoundField-s `SortExpression` Eigenschaft entfernt werden kann, oder aus dem deklarativen Markup als auch über die Felder (Dialogfeld) (das durch Klicken auf den Link "Spalten bearbeiten" in das GridView-s-Smarttag).


![Die Ergebnisse sind nach UnitPrice in aufsteigender Reihenfolge sortiert wurden](paging-and-sorting-report-data-cs/_static/image27.png)

**Abbildung 13**: die Ergebnisse haben UnitPrice in aufsteigender Reihenfolge sortiert wurden


Nach der `SortExpression` Eigenschaft wurde für entfernt die `UnitPrice` BoundField, als Text und nicht als einen Link an, wodurch verhindert, dass Benutzer die Daten nach Preis sortiert, wird der Header gerendert.


[![Entfernen Sie die SortExpression-Eigenschaft, können Benutzer nicht mehr die Produkte nach Preis sortieren](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Abbildung 14**: durch das Entfernen der SortExpression-Eigenschaft, können Benutzer nicht mehr der Produkte von Preis sortieren ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Programmgesteuertes Sortieren von GridView

Sie können auch Sortieren des Inhalts der GridView programmgesteuert mithilfe der GridView-s [ `Sort` Methode](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Übergeben Sie einfach die `SortExpression` Wert Sortierungskriterium zusammen mit den [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` oder `Descending`), und die GridView-s-Daten werden erneut sortiert werden.

Stellen Sie sich vor, dass der Grund, die wir Sortieren nach deaktiviert die `UnitPrice` wurde, weil bedenken, dass unsere Kunden einfach nur die kostengünstigste Produkte kaufen würde. Allerdings möchten wir empfehlen, zu der teuersten Produkte zu kaufen, damit wir d zu die Produkten nach Preis, jedoch nur von dem die teuersten Preis zu sortieren wie.

Erreichen dies hinzufügen ein Websteuerelements von Schaltfläche auf der Seite legen Sie seine `ID` Eigenschaft, um `SortPriceDescending`, und die zugehörige `Text` Eigenschaft sortieren nach Preis. Als Nächstes erstellen Sie einen Ereignishandler für die Schaltfläche "s" `Click` Ereignis durch Doppelklicken auf das Schaltflächen-Steuerelement im Designer. Fügen Sie diesem Ereignishandler den folgenden Code hinzu:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Auf diese Schaltfläche klicken, gibt den Benutzer auf die erste Seite mit den Produkten, sortiert nach Preis, teuersten am günstigsten ist (siehe Abbildung 15) zurück.


[![Klicken auf die Schaltfläche ordnet die Produkte aus dem die teuersten zur ältesten](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Abbildung 15**: Klicken auf die Schaltfläche die Produkte aus der teuerste zur ältesten Aufträge ([klicken Sie, um das Bild in voller Größe anzeigen](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial erläutert, wie standardmäßige Auslagern und Sortieren von Funktionen implementiert wurden beide so einfach wie das Aktivieren eines Kontrollkästchens! Wenn ein Benutzer sortiert oder durch die Daten Seiten, erweitert ein ähnlichen Workflow:

1. Ein Postback Sicherheitsvorschriften
2. Die Daten-Websteuerelement s Ebene vorab-Ereignis ausgelöst (`PageIndexChanging` oder `Sorting`)
3. Alle Daten erneut abgerufen, indem dem ObjectDataSource-Steuerelement
4. Die Daten-Websteuerelement s Ebene nach dem Ereignis wird ausgelöst (`PageIndexChanged` oder `Sorted`)

Beim Implementieren von grundlegenden Paginierung und Sortierung zum Kinderspiel wird muss mehr Aufwand, der eine effizientere benutzerdefinierte Paging verwendet oder weiter verbessern, die Auslagerungsdatei oder Sortierung-Schnittstelle ausgeübt. Zukünftige Tutorials werden diese Themen erkunden.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Nächste](efficiently-paging-through-large-amounts-of-data-cs.md)
