---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interaktion mit der Masterseite der Inhaltsseite (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Untersucht, wie Sie rufen Methoden, Eigenschaften usw. von der Masterseite im Code auf der Inhaltsseite festgelegt.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 59a00305cdcaf41ac0b37649382b9c3dc9ce1b0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838347"
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interaktion mit der Masterseite der Inhaltsseite (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Untersucht, wie Sie rufen Methoden, Eigenschaften usw. von der Masterseite im Code auf der Inhaltsseite festgelegt.


## <a name="introduction"></a>Einführung

Im Laufe der letzten fünf Lernprogramme haben erläutert, wie zum Erstellen einer Masterseite, Inhaltsbereiche definieren, binden ASP.NET-Seiten auf eine Gestaltungsvorlage und seitenspezifische Inhalte definieren. Wenn ein Besucher eine bestimmte Inhaltsseite auf anfordert, werden der Inhalt und Masterseiten Markup zur Laufzeit, was das Rendern von einer einheitlichen Steuerelementhierarchie fused. Aus diesem Grund, wir haben bereits gesehen, einer Möglichkeit, die die Masterseite und eines entsprechenden Inhaltsseiten interagieren können: das Markup für transfuse in die Masterseite ContentPlaceHolder-Steuerelemente die Seite Inhalte beschreibt.

Was ist wir noch untersuchen wie der Masterseite und die Seite "Inhalt" programmgesteuert interagieren können. Neben der Definition des Markups für die Masterseite ContentPlaceHolder-Steuerelemente, eine Inhaltsseite außerdem die Masterseite öffentliche Eigenschaften Werte zuweisen und die öffentlichen Methoden aufrufen. Auf ähnliche Weise kann eine Masterseite mit entsprechenden Inhaltsseiten interagieren. Während das Programminteraktion zwischen einem Master- und Seite seltener auf als die Interaktion zwischen ihren deklarative Markups ist, gibt es viele Szenarien, in denen solche programmgesteuerte Interaktion erforderlich ist.

In diesem Tutorial untersuchen wir an, wie eine Inhaltsseite mit der Masterseite programmgesteuert interagieren kann; im nächsten Tutorial betrachten wir wie die Masterseite auf ähnliche Weise mit der Inhaltsseiten interagieren kann.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Beispiele für die programmgesteuerte Interaktion zwischen einer Inhaltsseite und die Masterseite

Wenn eine bestimmte Region einer Seite für Seite von Seite konfiguriert werden muss, verwenden wir ein ContentPlaceHolder-Steuerelement. Aber was Situationen, in denen die Mehrheit der Seiten, die eine bestimmte Ausgabe, aber eine größere Anzahl von Seiten ausgeben müssen, angepasst werden muss, um etwas anderes anzuzeigen? Ein Beispiel, das wir in untersucht die [ *mehrere ContentPlaceHolder-Steuerelemente und Inhalte Standard* ](multiple-contentplaceholders-and-default-content-vb.md) Tutorial umfasst eine Schnittstelle für die Anmeldung auf jeder Seite angezeigt. Während die meisten Seiten eine Anmeldung Schnittstelle enthalten sollen, sie unterdrückt werden sollen für eine Reihe von Seiten, z. B.: die main-Anmeldeseite (`Login.aspx`); die Seite "Konto erstellen", und die anderen Seiten, die nur für authentifizierte Benutzer zugänglich sind. Die [ *mehrere ContentPlaceHolder-Steuerelemente und Standardinhalt* ](multiple-contentplaceholders-and-default-content-vb.md) Tutorial zeigte das Erstellen, um den standardmäßigen Inhalt für ein ContentPlaceHolder-Objekt auf der Masterseite zu definieren, und klicken Sie dann das in diesen überschreiben Where Seiten der Standardinhalt wurde nicht wollten.

Eine weitere Möglichkeit ist die Erstellung von einer öffentlichen Eigenschaft oder Methode innerhalb der Masterseite, die angibt, ob auf Sie ein- oder Ausblenden der Login-Schnittstelle. Für die Masterseite kann beispielsweise eine öffentliche Eigenschaft mit dem Namen `ShowLoginUI` festgelegt wurde, deren Wert verwendet die `Visible` -Eigenschaft des Steuerelements Anmeldung auf der Masterseite. Diese Inhaltsseiten, in denen die Anmeldebenutzeroberfläche unterdrückt werden sollen. anschließend programmgesteuert legen die `ShowLoginUI` Eigenschaft `False`.

Möglicherweise tritt auf, die am häufigsten auftretende Beispiel des Inhalts und der Interaktion der Masterseite, wenn Daten angezeigt, in die Masterseite muss aktualisiert werden, nachdem eine Aktion auf der Inhaltsseite wiederholt wurde. Beachten Sie, dass eine Masterseite, die einer GridView-Ansicht enthält, die die fünf zuletzt zeigt Datensätze aus einer bestimmten Datenbanktabelle hinzugefügt und, dass eine der entsprechenden Inhaltsseiten enthält eine Schnittstelle für die dieselbe Tabelle neue Datensätze hinzugefügt.

Wenn ein Benutzer auf das Zeichenblatt, um einen neuen Datensatz hinzufügen besucht, sieht sie, dass Datensätze auf der Masterseite angezeigt, das die fünf zuletzt hinzugefügt wurde. Nach dem Füllen die Werte für Spalten des neuen Datensatzes, wie sie das Formular übermittelt. Vorausgesetzt, dass das GridView auf der Masterseite hat seine `EnableViewState` Eigenschaft auf WAHR (Standardeinstellung) festgelegt wurde, dessen Inhalt wird aus dem Ansichtszustand neu geladen, und daher werden die gleichen fünf Datensätze angezeigt, obwohl die Datenbank ein neuer Datensatz hinzugefügt wurde. Dies kann den Benutzer verwirren.

> [!NOTE]
> Auch wenn Sie den GridView Ansichtszustand, damit sie mit der zugrunde liegenden Datenquelle bei jedem Postback bindet erneut deaktiviert, wird diese nicht immer noch den gerade hinzugefügten Eintrag angezeigt, da die Daten gebunden werden, an die GridView weiter oben in den Lebenszyklus der Seite, als wenn die Datab der neue Datensatz hinzugefügt wird ASE.


An, um dieses Problem zu beheben, damit der gerade hinzugefügten Datensatz auf der Masterseite angezeigt wird, die Sie GridView beim Postback anweisen, die GridView, die mit der Datenquelle binden möchten *nach* des neuen Eintrags der Datenbank hinzugefügt wurde. Dies erfordert die Interaktion zwischen dem Inhalt und Masterseiten, da die Schnittstelle für das Hinzufügen des neuen Datensatzes (und dessen Ereignishandler) werden in der Seite Inhalt, aber der GridView, die aktualisiert werden muss auf der Masterseite ist.

Aktualisieren die Masterseite Anzeige von einem Ereignishandler in der Seite Inhalt die häufigsten Anforderungen für Inhalt und die Interaktion der Masterseite ist, sehen wir uns in diesem Thema noch ausführlicher. Der Download für dieses Tutorial enthält eine Microsoft SQL Server 2005 Express Edition-Datenbank, die mit dem Namen `NORTHWIND.MDF` in der Website `App_Data` Ordner. Die Northwind-Datenbank speichert, Produkt, Mitarbeiter und Verkaufsinformationen für ein fiktives Unternehmen, Northwind Traders.

Schritt 1 führt über die fünf zuletzt angezeigt haben Produkte in einer GridView-Ansicht auf der Masterseite hinzugefügt. Schritt 2 erstellt eine Seite zum Hinzufügen neuer Produkte. Schritt 3 dargestellt, wie zum Erstellen von öffentlichen Eigenschaften und Methoden in der Masterseite und Schritt 4 wird veranschaulicht, wie Sie programmgesteuert mit diesen Eigenschaften und Methoden auf der Seite Inhalt.

> [!NOTE]
> In diesem Tutorial werden die Besonderheiten der Arbeit mit Daten in ASP.NET nicht beschäftigen. Die Schritte zum Einrichten der master-Seite zum Anzeigen von Daten und der Inhaltsseite zum Einfügen von Daten sind vollständige, noch die Bereitstellung – locker und. Eine eingehendere Betrachtung anzeigen und Einfügen von Daten sowie zur Verwendung der SqlDataSource-Steuerelement und GridView-Steuerelemente finden Sie in den Ressourcen im Abschnitt Weitere Messwerte am Ende dieses Tutorials.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Schritt 1: Anzeigen von die fünf zuletzt Produkte hinzugefügt, auf der Masterseite

Öffnen Sie die Stammwebsite und fügen Sie eine Bezeichnung und ein GridView-Steuerelement, um die `leftContent` `<div>`. Löschen Sie der Bezeichnung des `Text` Eigenschaftensatz, dessen `EnableViewState` Eigenschaft, um `False`, und die zugehörige `ID` Eigenschaft, um `GridMessage`; legen Sie des GridView `ID` Eigenschaft, um `RecentProducts`. Klicken Sie dann aus dem Designer, erweitern Sie den GridView Smarttag und mit einer neuen Datenquelle bindet, bindet es auch. Dadurch wird der Assistent zum Konfigurieren von Datenquellen gestartet. Da der Northwind-Datenbank der `App_Data` Ordner ist eine Microsoft SQL Server-Datenbank, um ein SqlDataSource-Steuerelement zu erstellen, indem Sie auswählen (siehe Abbildung 1) wählen, geben Sie dem SqlDataSource-Steuerelement Namen `RecentProductsDataSource`.


[![GridView zu binden, um ein SqlDataSource-Steuerelement, das mit dem Namen RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Abbildung 01**: GridView zu binden, um ein SqlDataSource-Steuerelement namens `RecentProductsDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


Im nächste Schritt fordert uns an, welche Datenbank eine Verbindung hergestellt. Wählen Sie die `NORTHWIND.MDF` -Datenbankdatei aus der Dropdown-Liste, und klicken Sie auf Weiter. Da dies beim ersten ist, haben wir diese Datenbank verwendet, bietet der Assistent zum Speichern der Verbindungszeichenfolge in `Web.config`. Lassen Sie sie speichern die Verbindungszeichenfolge, die mit dem Namen `NorthwindConnectionString`.


[![Verbinden Sie mit der Northwind-Datenbank](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Abbildung 02**: Verbinden mit der Northwind-Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


Das Konfigurieren von Datenquellen-Assistent bietet zwei Möglichkeiten, die mit denen wir die Abfrage zum Abrufen von Daten angeben können:

- Durch Angabe eines benutzerdefinierten SQL-Anweisung oder gespeicherte Prozedur, oder
- Durch Auswählen einer Tabelle oder Sicht, und klicken Sie dann angeben der zurückzugebenden Spalten

Da zurückgegeben, dass nur die fünf zuletzt Produkten hinzugefügt werden soll, müssen wir eine benutzerdefinierte SQL­Anweisung angeben. Verwenden Sie die folgenden `SELECT` Abfrage:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Die `TOP 5` Schlüsselwort gibt nur die ersten fünf Datensätze aus der Abfrage. Die `Products` Primärschlüssel der Tabelle, `ProductID`, ist ein `IDENTITY` Spalte, die uns wird sichergestellt, dass jeder neuen Produkts in der Tabelle hinzugefügt, einen höheren Wert als den vorherigen Eintrag hat. Aus diesem Grund Sortieren der Ergebnisse von `ProductID` gibt zurück, die Produkte an, wobei die zuletzt erstellte diejenigen in absteigender Reihenfolge.


[![Die fünf zuletzt hinzugefügte Produkte zurückgeben](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Abbildung 03**: die fünf am häufigsten vor kurzem hinzugefügt Produkte zurückzugeben ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Nach Abschluss des Assistenten, generiert Visual Studio zwei BoundFields für GridView zum Anzeigen der `ProductName` und `UnitPrice` Felder, die aus der Datenbank zurückgegeben. An diesem Punkt sollte die Masterseite deklaratives Markup ähnlich dem folgenden Markup enthalten:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Wie Sie sehen können, die das Markup enthält: das Label-Steuerelement (`GridMessage`); die GridView `RecentProducts`mit zwei BoundFields; und ein SqlDataSource-Steuerelement, das gibt zurück, der fünf Produkte zuletzt hinzugefügt wurde.

Mit diesem GridView, die erstellt und dessen SqlDataSource-Steuerelement konfiguriert haben, besuchen Sie die Website über einen Browser. Wie in Abbildung 4 gezeigt, sehen Sie sich, dass ein Raster in der unteren linken Ecke, die die fünf zuletzt listet Produkte hinzugefügt.


[![Das GridView zeigt die fünf zuletzt hinzugefügte Produkte](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Abbildung 04**: GridView zeigt die fünf am häufigsten vor kurzem hinzugefügt Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> Können Sie, um die Darstellung der GridView zu bereinigen. Einige Vorschläge enthalten die Formatierung der angezeigten `UnitPrice` Wert als eine Währung und mithilfe von Hintergrundfarben und Schriftarten um zu des Datenblatts Darstellung zu verbessern.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Schritt 2: Erstellen einer Inhaltsseite zum Hinzufügen neuer Produkte

Unsere nächste Aufgabe besteht darin, eine Inhaltsseite erstellen, von dem ein Benutzer kann auf ein neues Produkt hinzufügen, der `Products` Tabelle. Fügen Sie eine neue Seite auf die `Admin` Ordner mit dem Namen `AddProduct.aspx`, und bindet, bindet es an der `Site.master` Masterseite. Abbildung 5 zeigt den Projektmappen-Explorer, nachdem auf dieser Seite auf der Website hinzugefügt wurde.


[![Fügen Sie eine neue ASP.NET-Seite, um den Ordner Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Abbildung 05**: Hinzufügen einer neuen ASP.NET-Seite zu den `Admin` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Denken Sie daran, dass in der [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) Lernprogramm eine benutzerdefinierten Basisseite-Klasse, die mit dem Namen erstellten `BasePage` , die den Titel der Seite generiert, wenn es war nicht explizit festgelegt wurde. Wechseln Sie zu der `AddProduct.aspx` Seite des Code-Behind-Klasse und deren abgeleitet `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie abschließend die `Web.sitemap` hinzu, um einen Eintrag in dieser Lektion einzubeziehen. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Steuerelement-ID-Benennungsprobleme Lektion:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Siehe Abbildung 6: das Hinzufügen dieses `<siteMapNode>` Elements in der Liste der Lektionen wiedergegeben wird.

Wechseln Sie zurück zur `AddProduct.aspx`. In das Steuerelement für die `MainContent` ContentPlaceHolder, fügen Sie einem DetailsView-Steuerelement hinzu, und nennen Sie sie `NewProduct`. Binden von DetailsView an ein neues SqlDataSource-Steuerelement, das mit dem Namen `NewProductDataSource`. Wie Sie mit dem SqlDataSource-Steuerelement in Schritt 1, konfigurieren Sie den Assistenten, sodass sie die Northwind-Datenbank verwendet, und wählen eine benutzerdefinierte SQL­Anweisung angeben. Da DetailsView Hinzufügen von Elementen in der Datenbank verwendet wird, müssen wir beide geben eine `SELECT` Anweisung und eine `INSERT` Anweisung. Verwenden Sie die folgenden `SELECT` Abfrage:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Klicken Sie dann auf der Registerkarte einfügen, fügen Sie die folgenden `INSERT` Anweisung:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Finden Sie nach Abschluss des Assistenten unter DetailsViews-Smarttag, und aktivieren Sie das Kontrollkästchen "Einfügen aktivieren". Dadurch werden eine CommandField zur DetailsView mit seiner `ShowInsertButton` -Eigenschaft auf "true" festgelegt. Da diese DetailsView ausschließlich zum Einfügen von Daten verwendet wird, legen Sie die DetailsView `DefaultMode` Eigenschaft `Insert`.

Das ist alles schon! Testen Sie nun auf dieser Seite. Besuchen Sie `AddProduct.aspx` über einen Webbrowser, geben Sie einen Namen und den Preis (siehe Abbildung 6).


[![Hinzufügen eines neuen Produkts in der Datenbank](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Abbildung 06**: Hinzufügen eines neuen Produkts in die Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Klicken Sie nach dem Eingeben der Namen und den Preis für Ihr neues Produkt, auf die Schaltfläche "Insert". Dies bewirkt, dass das Formular, um postback. Beim Postback, dem SqlDataSource-Steuerelement des `INSERT` -Anweisung ausgeführt wird, werden die zwei Parameter mit den Werten der freien Eingabe in DetailsViews zwei TextBox-Steuerelemente aufgefüllt. Es ist leider keine visuelle Bestätigung, die eine Einfügung durchgeführt wurde. Es wäre gut, haben eine Meldung angezeigt wird, bestätigt, dass ein neuer Datensatz hinzugefügt wurde. Ich lassen Sie dieses als Übung für den Leser. Darüber hinaus zeigt nach dem Hinzufügen eines neuen Datensatzes aus der DetailsView GridView auf der Masterseite weiterhin dieselben fünf Datensätze wie vor; Es umfasst nicht den gerade hinzugefügten Datensatz. Untersucht, wie Sie dies in den nächsten Schritten zu beheben.

> [!NOTE]
> Zusätzlich zum Hinzufügen von einer Art von visuellem Feedback, das die Einfügung erfolgreich war, empfehle ich Ihnen auch aktualisieren, Einfügen von DetailsViews-Schnittstelle zur Validierung einschließen. Es gibt derzeit keine Validierung. Wenn ein Benutzer einen ungültigen Wert für eingibt der `UnitPrice` Feld, z. B. "zu viel Leistung beanspruchen," eine Ausnahme wird beim Postback ausgelöst werden, wenn das System versucht, diese Zeichenfolge in einen Dezimalwert zu konvertieren. Weitere Informationen zum Anpassen der Einfügen von Schnittstellen, finden Sie in der [ *Anpassen der Benutzeroberfläche für die Änderung der Daten* Tutorial](https://asp.net/learn/data-access/tutorial-20-vb.aspx) aus meiner [arbeiten mit Daten tutorialreihe](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Schritt 3: Erstellen öffentliche Eigenschaften und Methoden auf der Masterseite

In Schritt 1 hinzugefügten wir ein Label-Steuerelement mit dem Namen `GridMessage` oben auf der Masterseite GridView. Diese Bezeichnung ist vorgesehen, optional eine Meldung angezeigt. Beispielsweise nach dem Hinzufügen eines neuen Datensatzes, der `Products` Tabelle, wir möchten eine Meldung angezeigt, die liest: "*ProductName* in der Datenbank hinzugefügt wurde." Anstatt hartcodieren der Text für diese Bezeichnung auf der Masterseite sollten wir die Nachricht von der Seite Inhalt angepasst werden.

Da das Label-Steuerelement als eine geschützte Member-Variable innerhalb der Masterseite implementiert wird, kann nicht diese direkt von Inhaltsseiten zugegriffen werden. Für die Arbeit mit der Bezeichnung in einer Masterseite der Inhaltsseite (oder einfach alle Websteuerelement in die Masterseite) müssen wir eine öffentliche Eigenschaft in die Masterseite zu erstellen, macht das Websteuerelement oder dient als Proxy mit dem eine seiner Eigenschaften werden kann  der Zugriff auf. Fügen Sie die folgende Syntax, die Masterseite CodeBehind-Klasse, der Bezeichnung des verfügbar zu machen `Text` Eigenschaft:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Wenn ein neuer Datensatz hinzugefügt wird, um die `Products` Tabelle von einer Inhaltsseite der `RecentProducts` GridView, die auf der Masterseite muss mit der zugrunde liegenden Datenquelle zu binden. Zum Binden des GridView-Aufrufs der `DataBind` Methode. Da die GridView auf der Masterseite nicht programmgesteuert zugegriffen werden kann, auf die Inhaltsseiten ist, werden wir müssen eine öffentliche Methode in der Masterseite, die während der Erstellung aufgerufen, bindet die Daten an die GridView. Fügen Sie auf der Masterseite CodeBehind-Klasse die folgende Methode hinzu:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Mit der `GridMessageText` Eigenschaft und `RefreshRecentProductsGrid` Methode vorhanden, jeder Inhaltsseite kann programmgesteuert festlegen oder Lesen Sie den Wert des der `GridMessage` Bezeichnungsfelds `Text` Eigenschaft oder binden Sie die Daten in die `RecentProducts` GridView. Schritt 4 untersucht, wie Sie öffentliche Eigenschaften und Methoden der Masterseite von einer Inhaltsseite zugreifen wird.

> [!NOTE]
> Vergessen Sie nicht, markieren Sie die Eigenschaften und Methoden wie der Masterseite `Public`. Wenn Sie nicht explizit diese Eigenschaften und Methoden als kennzeichnen `Public`, sie können nicht auf der Seite Inhalt zugegriffen werden.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Schritt 4: Aufrufen von öffentlichen Member der Masterseite von einer Inhaltsseite

Nachdem die Masterseite erforderlichen öffentliche Eigenschaften und Methoden verfügt, wir können diese Eigenschaften und Methoden aus Aufrufen der `AddProduct.aspx` Inhaltsseite. Insbesondere müssen wir die Gestaltungsvorlage festgelegt `GridMessageText` -Eigenschaft, und rufen die `RefreshRecentProductsGrid` -Methode auf, nachdem das neue Produkt mit der Datenbank hinzugefügt wurde. Alle ASP.NET Daten-Websteuerelemente auslösen Ereignisse, unmittelbar vor und nach dem Ausführen verschiedener Aufgaben, die Seitenentwickler, die eine programmgesteuerte Aktion vor oder nach der Aufgabe zu erleichtern. Z. B. wenn der Endbenutzer DetailsViews-Einfügen-Schaltfläche klickt, auf postback die DetailsView löst die `ItemInserting` -Ereignis vor dem Einfügen von Workflow ab. Klicken Sie dann den Datensatz in die Datenbank eingefügt. Danach DetailsView löst die `ItemInserted` Ereignis. Erstellen aus diesem Grund für die Arbeit mit der Masterseite nachdem das neue Produkt hinzugefügt wurde, einen Ereignishandler für die DetailsView `ItemInserted` Ereignis.

Es gibt zwei Möglichkeiten, eine Inhaltsseite programmgesteuert mit der die Masterseite kann:

- Mithilfe der `Page.Master` -Eigenschaft, die einen lose typisierten Verweis auf die Masterseite zurückgegeben wird, oder
- Geben Sie Pfad der Seite Masterseite Typ oder die Datei über eine `@MasterType` -Direktive; Dadurch wird eine stark typisierte Eigenschaft automatisch auf die Seite mit dem Namen hinzugefügt `Master`.

Betrachten wir beide Ansätze.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Verwenden die lose typisierte`Page.Master`Eigenschaft

Alle ASP.NET-Webseiten muss abgeleitet werden, aus der `Page` -Klasse, die sich im befindet der `System.Web.UI` Namespace. Die `Page` Klasse enthält eine [ `Master` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , der einen Verweis auf die Masterseite zurückgibt. Wenn die Seite nicht über eine Masterseite verfügt `Master` gibt `Nothing`.

Die `Master` -Eigenschaft gibt ein Objekt des Typs [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (auch im Verzeichnis der `System.Web.UI` Namespace) Dies ist der Basistyp, von dem alle Masterseiten abgeleitet. Aus diesem Grund auf mit öffentlichen Eigenschaften oder Methoden, die in unsere Website, die Masterseite, wir müssen umwandeln, definiert die `MasterPage` Objekt, das von der `Master` Eigenschaft in den entsprechenden Typ. Da wir unsere Masterseitendatei namens `Site.master`, wurde der Code-Behind-Klasse mit dem Namen `Site`. Der folgende code aus diesem Grund Umwandlungen der `Page.Master` Eigenschaft mit einer Instanz von der `Site` Klasse.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Nun, da wir umgewandelt haben die lose typisierte `Page.Master` Eigenschaft auf den Typ des Standorts können wir die Eigenschaften und Methoden, die Website zu verweisen. Wie in Abbildung 7 dargestellt, das die öffentliche Eigenschaft `GridMessageText` wird in der IntelliSense-Dropdownliste angezeigt.


[![IntelliSense zeigt unsere Masterseite öffentliche Eigenschaften und Methoden](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Abbildung 07**: IntelliSense zeigt unsere Masterseite öffentliche Eigenschaften und Methoden ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Wenn Sie Ihre Masterseitendatei namens `MasterPage.master` lautet die Masterseite Code-Behind-Klassenname `MasterPage`. Dies kann zu mehrdeutigen Code führen, bei der Umwandlung vom Typ `System.Web.UI.MasterPage` auf Ihre `MasterPage` Klasse. Kurz gesagt, müssen Sie den vollqualifizierten des Typs, dem Sie umwandeln, das kann etwas schwierig sein, wenn das Websiteprojekt-Modell verwenden. Mein Vorschlag wäre entweder sicherstellen, dass bei der Erstellung Ihrer Masterseite Sie es etwas anderen als Namen `MasterPage.master` oder noch besser ist, erstellen Sie einen stark typisierten Verweis auf die Masterseite.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Erstellen einen stark typisierten Verweis mit dem`@MasterType`Richtlinie

Wenn Sie genau hinsehen werden Sie sehen, dass eine ASP.NET-Seite Code-Behind-Klasse eine partielle Klasse ist (Beachten Sie die `Partial` Schlüsselwort in der Klassendefinition). Partielle Klassen in c# und Visual Basic mit Framework 2.0 eingeführt wurden und im Prinzip für die Member einer Klasse in mehreren Dateien definiert werden können. Die CodeBehind-Klassendatei - `AddProduct.aspx.vb`, Beispiel: enthält den Code, den Sie, die Seitenentwickler erstellen. Neben unserem Code das ASP.NET-Modul erstellt automatisch eine separate Klasse-Datei mit den Eigenschaften, und Ereignishandlern, übersetzen deklarative Markup in der Klassenhierarchie der Seite aus.

Die automatische codegenerierung, die auftritt, wenn eine ASP.NET-Seite besucht wird kontextspezifischen einige recht interessanten und nützliche Möglichkeiten. Im Fall von Masterseiten, wenn wir das ASP.NET-Modul mitteilen, welche Masterseite von unserer Seite verwendet wird, er generiert einen stark typisierten `Master` -Eigenschaft für uns.

Verwenden der [ `@MasterType` Richtlinie](https://msdn.microsoft.com/library/ms228274.aspx) die ASP.NET-Engine für die master Seitentyp der Seite Inhalt informiert. Die `@MasterType` Richtlinie kann entweder der Typname der Masterseite oder der Pfad der Datei akzeptieren. Um anzugeben, dass die `AddProduct.aspx` verwendet `Site.master` als Gestaltungsvorlage, fügen Sie die folgende Anweisung am Anfang `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Diese Anweisung weist die Engine ASP.NET einen stark typisierten Verweis auf die Masterseite über eine Eigenschaft mit dem Namen hinzufügen `Master`. Mit der `@MasterType` -Anweisung direktes Aufrufen der `Site.master` master öffentliche Eigenschaften und Methoden, die direkt über die Seite die `Master` Eigenschaft ohne datentypkonvertierungen.

> [!NOTE]
> Wenn Sie weglassen der `@MasterType` Richtlinie ist die Syntax `Page.Master` und `Master` das gleiche Ergebnis zurückgegeben: ein lose typisiertes Objekt in die Masterseite. Wenn Sie enthalten die `@MasterType` Richtlinie dann `Master` gibt einen stark typisierten Verweis auf die angegebene Masterseite. `Page.Master`, jedoch immer noch einen lose typisierten Verweis zurück. Eine ausführlichere Betrachtung warum dies der Fall ist und wie die `Master` Eigenschaft erstellt wird bei der `@MasterType` Richtlinie enthalten ist, finden Sie unter [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)Blogeintrag [ `@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualisieren die Masterseite, nach dem Hinzufügen eines neuen Produkts

Nachdem wir wissen, wie zum Aufrufen der öffentlichen Eigenschaften und Methoden von einer Inhaltsseite einer Masterseite wir sind bereit zum Aktualisieren der `AddProduct.aspx` Seite, damit die Masterseite aktualisiert wird, nach dem Hinzufügen eines neuen Produkts. Zu Beginn von Schritt 4, die wir erstellt eines ereignishandlers für des DetailsView-Steuerelements `ItemInserting` -Ereignis, das ausgeführt wird, sofort, nachdem das neue Produkt mit der Datenbank hinzugefügt wurde. Fügen Sie zu diesem Ereignishandler den folgenden Code hinzu:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Der obige Code wird sowohl die lose typisierte `Page.Master` Eigenschaft und die stark typisierte `Master` Eigenschaft. Beachten Sie, dass die `GridMessageText` -Eigenschaftensatz auf "*ProductName* ... Raster hinzugefügt" Werte für den gerade hinzugefügten des Produkts können Sie über die `e.Values` Auflistung, wie Sie sehen können, die gerade hinzugefügte `ProductName` Wert erfolgt über `e.Values("ProductName")`.

Abbildung 8 zeigt die `AddProduct.aspx` Seite sofort nach der ein neues Produkt - Scotts Soda - Datenbank hinzugefügt wurde. Beachten Sie, dass der Name des gerade hinzugefügten Produkts in die Masterseite Bezeichnung angegeben ist und die GridView aktualisiert worden sind, um das Produkt und seinem Preis enthalten.


[![Bezeichnung und GridView anzeigen, das gerade hinzugefügte Produkt der Masterseite](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Abbildung 08**: Bezeichnung und GridView die Masterseite wird gezeigt, das Produkt Just-Added ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Zusammenfassung

Im Idealfall eine Masterseite und die Inhaltsseiten sind vollständig voneinander getrennt, und Sie benötigen keine Interaktion. Während der Masterseiten und Inhaltsseiten mit diesem Ziel vor Augen entworfen werden soll, gibt es zahlreiche gängige Szenarien, in denen eine Inhaltsseite mit der Masterseite Schnittstelle muss. Einer der häufigsten Gründe Rechenzentren zu einen bestimmten Teil die Masterseite-Anzeige, die eine Aktion, die auf der Inhaltsseite wiederholt entsprechend aktualisieren.

Die gute Nachricht ist, dass es relativ einfach, um eine programmgesteuerte Interaktion mit der Masterseite Inhaltsseite zu erhalten. Zunächst erstellen öffentliche Eigenschaften oder Methoden auf der Masterseite, die die Funktionalität zu kapseln, die von einer Inhaltsseite aufgerufen werden muss. Klicken Sie dann auf der Inhaltsseite, Zugriff auf Eigenschaften und Methoden der Masterseite über die typenlosen `Page.Master` Eigenschaft oder verwenden Sie die `@MasterType` Direktive, um einen stark typisierten Verweis auf die Masterseite zu erstellen.

Im nächsten Tutorial untersuchen wir auf die programmgesteuerte Interaktion mit einem entsprechenden Inhaltsseiten Masterseite.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET Masterseiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Übergeben von Informationen zwischen Inhalt und Masterseiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in den ASP.NET-Tutorials](../../data-access/index.md)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Zack Jones. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](control-id-naming-in-content-pages-vb.md)
> [Weiter](interacting-with-the-content-page-from-the-master-page-vb.md)
