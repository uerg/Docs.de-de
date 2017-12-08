---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interaktion mit der Masterseite von der Inhaltsseite (VB) | Microsoft Docs
author: rick-anderson
description: Untersucht, wie Methoden aufrufen, legen Eigenschaften, die von der Seite "Master" usw., aus Code Inhaltsseite.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f5cb1712922c355c99bde9f8252dc84f1f590ec
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interaktion mit der Masterseite von der Inhaltsseite (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Untersucht, wie Methoden aufrufen, legen Eigenschaften, die von der Seite "Master" usw., aus Code Inhaltsseite.


## <a name="introduction"></a>Einführung

Im Verlauf der letzten fünf Lernprogramme haben erläutert, wie Erstellen einer Masterseite Inhaltsbereiche definieren, Binden von ASP.NET-Seiten auf einer Masterseite und seitenspezifische Inhalt definieren. Wenn ein Besucher eine bestimmte Seite Inhalte anfordert, werden der Inhalt und Masterseiten Markup zur Laufzeit, wodurch das Rendern einer Hierarchie einheitliche Steuerung fused. Aus diesem Grund haben bereits gesehen einer Möglichkeit, die der Masterseite und eines seiner Inhaltsseiten interagieren können: die Seite Inhalte werden das Markup zum Rendern in die Masterseite ContentPlaceHolder Steuerelemente transfuse.

Was wir noch nicht untersuchen ist wie der Masterseite und Inhaltsseite programmgesteuert interagieren können. Zusätzlich zum Definieren von des Markups für die Gestaltungsvorlage ContentPlaceHolder-Steuerelemente, eine Inhaltsseite auch die Masterseite öffentlichen Eigenschaften Werte zuweisen und Aufrufen ihrer öffentlichen Methoden. Auf ähnliche Weise kann eine Gestaltungsvorlage mit seiner Inhaltsseiten interagieren. Während programmgesteuerte Interaktion zwischen Master- und Seite weniger gebräuchlich als die Interaktion zwischen ihren deklarativen Markups ist, gibt es viele Szenarien, in denen solche programmgesteuerte Interaktion erforderlich ist.

In diesem Lernprogramm wird untersucht, wie eine Inhaltsseite programmgesteuert mit Gestaltungsvorlage interagieren kann; in den nächsten Lernprogrammen betrachten wir wie die Gestaltungsvorlage auf ähnliche Weise mit seiner Inhaltsseiten interagieren kann.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Beispiele für die programmgesteuerte Interaktion zwischen einer Inhaltsseite und der Gestaltungsvorlage

Wenn eine bestimmte Region einer Seite von Seite konfiguriert werden muss, verwenden wir ein ContentPlaceHolder-Steuerelement. Aber was müssen Situationen, in denen die meisten der Seiten, die eine bestimmte Ausgabe, aber eine größere Anzahl von Seiten ausgeben müssen, so anzupassen, um etwas anderes anzuzeigen? Ein solches Beispiel, das wir im untersucht die [ *Inhalt standardmäßig und mehrere ContentPlaceHolders* ](multiple-contentplaceholders-and-default-content-vb.md) Lernprogramm umfasst eine Schnittstelle für die Anmeldung auf jeder Seite angezeigt. Während die meisten Seiten eine Anmeldeseite enthalten sollen, sie unterdrückt werden sollen für eine Handvoll Seiten, z. B.: der Haupt-Anmeldeseite (`Login.aspx`); die Seite "Konto erstellen", und die anderen Seiten, die nur für authentifizierte Benutzer verfügbar sind. Die [ *mehrere ContentPlaceHolders und Standardinhalt* ](multiple-contentplaceholders-and-default-content-vb.md) Lernprogramm wurde gezeigt, wie den Standardinhalt für einen ContentPlaceHolder in die Masterseite definieren und dann das in diesen überschreiben Where der Standardinhalt wurde nicht erwünscht.

Eine andere Möglichkeit ist die Erstellung einer öffentlichen Eigenschaft oder Methode innerhalb der Masterseite, die angibt, ob ein- oder Ausblenden der Anmelde-Schnittstelle. Die Gestaltungsvorlage kann z. B. eine öffentliche Eigenschaft mit dem Namen enthalten `ShowLoginUI` , dessen Wert wurde zum Festlegen der `Visible` Eigenschaft des Steuerelements Anmeldung auf der Masterseite. Diese Inhaltsseiten, in denen die Anmeldebenutzeroberfläche unterdrückt werden sollen. anschließend programmgesteuert legen die `ShowLoginUI` Eigenschaft `False`.

Das häufigste Beispiel des Inhalts und der Gestaltungsvorlage Interaktion tritt möglicherweise auf, wenn es sich bei Daten angezeigt, in der Gestaltungsvorlage muss aktualisiert werden, nachdem eine Aktion auf der Inhaltsseite vergangen ist. Beachten Sie, dass eine Masterseite mit GridView, die die fünf zuletzt zeigt Datensätze aus einer bestimmten Datenbanktabelle hinzugefügt und, dass eine der entsprechenden Inhaltsseiten umfasst eine Schnittstelle für neue Datensätze zu derselben Tabelle hinzufügen.

Wenn ein Benutzer auf die Seite, um einen neuen Datensatz hinzufügen besucht, sieht er, dass die fünf zuletzt auf der Seite "master" angezeigten Datensätze hinzugefügt. Nach dem Füllen die Werte für Spalten des neuen Datensatzes, wie sie das Formular übermittelt. Vorausgesetzt, dass die GridView in die Masterseite hat seine `EnableViewState` -Eigenschaft auf True (Standardwert) festgelegt, dessen Inhalt aus dem Ansichtszustand geladen wird und daher werden fünf dieselben Datensätze angezeigt, obwohl die Datenbank ein neuerer Datensatz hinzugefügt wurde. Dies kann den Benutzer verwirrend sein.

> [!NOTE]
> Auch wenn Sie so, dass er auf die zugrunde liegenden Datenquelle auf jedes Postback bindet die GridView Ansichtszustand deaktivieren, wird nicht er weiterhin den gerade hinzugefügten Eintrag angezeigt werden, weil die Daten gebunden werden, an die GridView weiter oben in den Lebenszyklus der Seite, als wenn der neue Datensatz der Datab hinzugefügt wird ASE.


Um dieses Problem zu beheben, damit der gerade hinzugefügten Datensatz in die Masterseite angezeigt wird GridView Postback wir die GridView erneut an die Datenquelle binden anweisen müssen des *nach* des neuen Datensatzes der Datenbank hinzugefügt wurde. Dies erfordert die Interaktion zwischen dem Inhalt und Masterseiten, auf der Masterseite ist die Schnittstelle für das Hinzufügen des neuen Datensatzes (und dessen Ereignishandler) befinden sich in der Inhaltsseite, aber die GridView, die aktualisiert werden muss.

Da die Gestaltungsvorlage Anzeige von einem Ereignishandler auf der Inhaltsseite Aktualisieren eines der am häufigsten verwendeten Anforderungen für Inhalt und die Interaktion der Masterseite ist, lassen Sie uns in diesem Thema im Detail. Der Download für dieses Lernprogramm enthält eine Microsoft SQL Server 2005 Express Edition-Datenbank mit dem Namen `NORTHWIND.MDF` auf der Website `App_Data` Ordner. Die Northwind-Datenbank speichert, Product, Mitarbeiter und Verkaufsinformationen für ein fiktives Unternehmen, Northwind Traders.

Schritt 1 die fünf zuletzt anzeigen schrittweise hinzugefügt Produkte in einer GridView in die Masterseite. Schritt 2 erstellt eine Seite für das Hinzufügen neuer Produkte. Schritt 3 prüft zum Erstellen von öffentlichen Eigenschaften und Methoden in der Masterseite und Schritt 4 wird veranschaulicht, wie mit diesen Eigenschaften und Methoden aus der Inhaltsseite programmgesteuert Schnittstelle.

> [!NOTE]
> Dieses Lernprogramm ist in der Besonderheiten bei der Arbeit mit Daten in ASP.NET nicht eingegangen. Die Schritte zum Einrichten der Masterseite zum Anzeigen von Daten und der Inhaltsseite zum Einfügen von Daten sind abgeschlossen wurde, noch breezy. Eine eingehendere Betrachtung anzeigen und Einfügen von Daten und Verwenden der SqlDataSource und GridView-Steuerelemente finden Sie in den Ressourcen im Abschnitt Weitere Messwerte am Ende dieses Lernprogramms.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Schritt 1: Anzeigen von die fünf zuletzt Produkte hinzugefügt, der Gestaltungsvorlage

Öffnen Sie die Stammwebsite, und fügen Sie eine Bezeichnung und eines GridView-Steuerelements an die `leftContent` `<div>`. Deaktivieren Sie der Bezeichnung `Text` Eigenschaftensatz, dessen `EnableViewState` Eigenschaft, um `False`, und die zugehörige `ID` Eigenschaft, um `GridMessage`; legen Sie der GridView `ID` Eigenschaft, um `RecentProducts`. Als Nächstes Designer erweitern Sie die GridView-Smarttag, und binden Sie es an eine neue Datenquelle. Das Konfigurieren von Datenquellen-Assistent wird gestartet. Da die Northwind-Datenbank der `App_Data` Ordner ist eine Microsoft SQL Server-Datenbank entscheiden, erstellen ein SqlDataSource dazu (siehe Abbildung 1); benennen Sie die SqlDataSource `RecentProductsDataSource`.


[![Die GridView zu binden, um ein SqlDataSource-Steuerelement mit dem Namen RecentProductsDataSource](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Abbildung 01**: die GridView zu binden, um ein SqlDataSource-Steuerelement namens `RecentProductsDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


Als Nächstes fordert uns um anzugeben, welche Datenbank eine Verbindung hergestellt. Wählen Sie die `NORTHWIND.MDF` Datenbankdatei aus der Dropdown-Liste, und klicken Sie auf Weiter. Da dies das erste Mal wir diese Datenbank verwendet ist haben, bietet der Assistent zum Speichern der Verbindungszeichenfolge in `Web.config`. Haben sie die Verbindungszeichenfolge mit dem Namen speichern `NorthwindConnectionString`.


[![Herstellen einer Verbindung mit der Northwind-Datenbank](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Abbildung 02**: Herstellen einer Verbindung mit der Northwind-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


Konfigurieren von Datenquellen-Assistent bietet zwei bedeutet, dass nach denen wir zum Abrufen von Daten verwendete Abfrage angeben können:

- Durch Angabe eines benutzerdefinierten SQL-Anweisung oder gespeicherte Prozedur oder
- Durch Auswählen einer Tabelle oder Sicht und gebe ich die Spalten zurückgegeben

Da zurückgeben, dass nur die fünf zuletzt Produkte hinzugefügt werden soll, müssen wir eine benutzerdefinierte SQL­Anweisung angeben. Verwenden Sie die folgenden `SELECT` Abfrage:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

Die `TOP 5` Schlüsselwort gibt nur die ersten fünf Datensätze aus der Abfrage zurück. Die `Products` Primärschlüssel der Tabelle, `ProductID`, ist ein `IDENTITY` Spalte, die uns wird sichergestellt, dass jedes neue Produkt, die der Tabelle hinzugefügt, einen höheren Wert als der vorherige Wert hat. Aus diesem Grund Sortieren der Ergebnisse von `ProductID` in absteigender Reihenfolge der Produkte, die beginnend mit den zuletzt erstellten zurückgegeben.


[![Die fünf zuletzt hinzugefügte Produkte zurückgeben](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Abbildung 03**: die fünf am häufigsten vor kurzem hinzugefügt Produkte zurückgeben ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Nach Abschluss des Assistenten, generiert Visual Studio zwei BoundFields für GridView zum Anzeigen der `ProductName` und `UnitPrice` Felder aus der Datenbank zurückgegeben. An diesem Punkt sollte die Masterseite deklarativem Markup Markup ähnlich der folgenden enthalten:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Wie Sie sehen können, die das Markup enthält: das Label-Steuerelement (`GridMessage`); die GridView `RecentProducts`mit zwei BoundFields; und ein SqlDataSource-Steuerelement, die fünf zuletzt hinzugefügten Produkte zurückgibt.

Mit diesem GridView erstellt und dessen SqlDataSource-Steuerelement, das so konfiguriert ist, besuchen Sie die Website über einen Browser. Wie in Abbildung 4 gezeigt, sehen Sie sich, dass ein Raster in der unteren linken Ecke, die die fünf zuletzt listet Produkte hinzugefügt.


[![Die GridView zeigt die fünf zuletzt hinzugefügte Produkte](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Abbildung 04**: GridView zeigt die fünf am häufigsten vor kurzem hinzugefügt Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> Wahlweise können die Darstellung der GridView bereinigen. Einige Vorschläge enthalten die angezeigten Formatierung `UnitPrice` Wert als Währung und mithilfe von Hintergrundfarben und Schriftarten zum Optimieren der Darstellung des Datenblatts.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Schritt 2: Erstellen einer Inhaltsseite zum Hinzufügen neuer Produkte

Unsere nächste Aufgabe ist die Erstellung eine Inhaltsseite, in dem ein Benutzer ein neues Produkt hinzufügen kann, die `Products` Tabelle. Fügen Sie eine neue Seite, die `Admin` Ordner mit dem Namen `AddProduct.aspx`, sodass Sie sicher, dass so binden Sie es der `Site.master` Masterseite. Abbildung 5 zeigt der Projektmappen-Explorer aus, nachdem dieser Seite auf der Website hinzugefügt wurde.


[![Fügen Sie eine neue ASP.NET-Seite, um den Ordner Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Abbildung 05**: Fügen Sie eine neue ASP.NET-Seite, um die `Admin` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Denken Sie daran, dass in der [ *Titel, Meta-Tags und andere HTML-Header angeben, der Gestaltungsvorlage* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) Lernprogramm wir eine benutzerdefinierte Basisseite-Klasse, die mit dem Namen erstellt `BasePage` , die den Titel der Seite generiert, wenn es war nicht explizit festgelegt wurde. Wechseln Sie zu der `AddProduct.aspx` Seite des Code-Behind-Klasse, und Ihr abgeleitet sein `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie schließlich die `Web.sitemap` Datei, die einen Eintrag in dieser Lektion hinzugefügt. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für die Steuerelement-ID Naming Probleme Lektion:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Wie in Abbildung 6: das Hinzufügen dieses gezeigt `<siteMapNode>` Elements in der Liste Lektionen wiedergegeben wird.

Zurück zu `AddProduct.aspx`. Im Inhaltssteuerelement für die `MainContent` ContentPlaceHolder, DetailsView-Steuerelement hinzufügen, und nennen Sie sie `NewProduct`. Binden von DetailsView an ein neues SqlDataSource-Steuerelement mit dem Namen `NewProductDataSource`. Wie Sie mit der SqlDataSource in Schritt 1, konfigurieren Sie den Assistenten so, dass sie die Northwind-Datenbank verwendet, und wählen Sie eine benutzerdefinierte SQL­Anweisung angeben. Da DetailsView Hinzufügen von Elementen in der Datenbank verwendet wird, müssen wir beide geben eine `SELECT` Anweisung und eine `INSERT` Anweisung. Verwenden Sie die folgenden `SELECT` Abfrage:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Fügen Sie dann aus der Registerkarte "Einfügen" die folgenden `INSERT` Anweisung:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Nach Abschluss des Assistenten wechseln Sie zu der DetailsView Smarttag, und aktivieren Sie das Kontrollkästchen "Einfügen aktivieren". Dadurch wird eine CommandField, DetailsView mit seiner `ShowInsertButton` -Eigenschaft auf "true" festgelegt. Festgelegt werden, da diese DetailsView ausschließlich zum Einfügen von Daten verwendet werden, DetailsView des `DefaultMode` Eigenschaft `Insert`.

Das ist alles vorhanden ist! Testen Sie nun auf dieser Seite. Besuchen Sie `AddProduct.aspx` über einen Webbrowser, und geben Sie einen Namen und den Preis (siehe Abbildung 6).


[![Hinzufügen eines neuen Produkts in der Datenbank](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Abbildung 06**: Hinzufügen eines neuen Produkts in die Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Klicken Sie nach der Eingabe in den Namen und den Preis für das neue Produkt auf die Schaltfläche einfügen. Dies bewirkt, dass das Formular für das postback. Beim Postback SqlDataSource-Steuerelement des `INSERT` -Anweisung ausgeführt wird; die zwei Parameter mit den eingegebenen Werten in der DetailsView zwei TextBox-Steuerelemente aufgefüllt werden. Leider ist keine visuelle Bestätigung, die eine Einfügung vorgenommen wurde. Es wäre schön, weisen eine Meldung angezeigt wird, bestätigen, dass ein neuer Datensatz hinzugefügt wurde. Ich lassen Sie dieses als Übung für den Leser. Darüber hinaus wird gezeigt, nach dem Hinzufügen eines neuen Datensatzes aus der DetailsView GridView in die Masterseite weiterhin dieselben fünf Datensätze wie vor: Es umfasst nicht den gerade hinzugefügten Datensatz. Untersuchen wir wie dies in den nächsten Schritten zu beheben.

> [!NOTE]
> Zusätzlich zum Hinzufügen von eine Form der visuelles Feedback, das die Einfügung erfolgreich war, empfehle ich Sie auch aktualisieren, Einfügen von der DetailsView-Schnittstelle bereit, eine Validierung beinhalten. Es gibt derzeit keine Validierung. Wenn ein Benutzer für einen ungültigen Wert eingibt der `UnitPrice` Feld, z. B. "zu viel Leistung beanspruchen," eine Ausnahme wird beim Postback ausgelöst werden, wenn das System versucht, diese Zeichenfolge in eine Dezimalzahl umwandeln. Weitere Informationen zum Anpassen der einfügen-Schnittstelle, finden Sie unter der [ *Anpassen der Benutzeroberfläche für die Änderung der Daten* Lernprogramm](https://asp.net/learn/data-access/tutorial-20-vb.aspx) aus meiner [arbeiten mit Tutorial Datenreihe](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Schritt 3: Erstellen von öffentlichen Eigenschaften und Methoden in der Gestaltungsvorlage

In Schritt 1 hinzugefügten wir ein Bezeichnung-Websteuerelement, der mit dem Namen `GridMessage` über die GridView in die Masterseite. Diese Beschriftung dient auch eine Meldung anzeigen. Beispielsweise nach dem Hinzufügen eines neuen Datensatzes, der `Products` Tabelle wir möglicherweise eine Meldung anzeigen möchten, die gelesen: "*ProductName* der Datenbank hinzugefügt wurde." Anstatt hartcodieren der Text für diese Bezeichnung in die Masterseite sollten wir die Nachricht von der Seite Inhalt anpassbar sein.

Da das Label-Steuerelement als Variable geschützter Member innerhalb der Masterseite implementiert wird, die direkt von Inhaltsseiten zugegriffen werden kann. Für die Arbeit mit der Bezeichnung innerhalb einer Masterseite von der Seite Inhalt (oder zu alle Websteuerelement in die Masterseite) muss eine öffentliche Eigenschaft in die Masterseite erstellt werden, die das Websteuerelement macht oder fungiert als Proxy mit dem eine seiner Eigenschaften werden kann  der Zugriff auf. Fügen Sie die folgende Syntax der Masterseite Code-Behind-Klasse, der Bezeichnung verfügbar zu machen `Text` Eigenschaft:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Wenn ein neuer Datensatz hinzugefügt wird, um die `Products` Tabelle aus einer Inhaltsseite der `RecentProducts` GridView in die Masterseite muss erneut an die zugrunde liegenden Datenquelle binden. Zum Binden des GridView-Aufrufs seine `DataBind` Methode. Da die GridView in die Masterseite nicht programmgesteuert zugegriffen werden kann, um die Inhaltsseiten ist, wir müssen eine öffentliche Methode in die Masterseite erstellen, das beim aufgerufen, bindet die Daten an die GridView. Fügen Sie die folgende Methode, um die Gestaltungsvorlage Code-Behind-Klasse:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Mit der `GridMessageText` Eigenschaft und `RefreshRecentProductsGrid` Methode vorhanden, jeder Inhaltsseite kann programmgesteuert festgelegt oder Lesen Sie den Wert des der `GridMessage` Bezeichnungsfelds `Text` Eigenschaft oder binden Sie die Daten der `RecentProducts` GridView. Schritt 4 untersucht, wie auf einer Inhaltsseite der Masterseite öffentlichen Eigenschaften und Methoden zuzugreifen.

> [!NOTE]
> Vergessen Sie nicht, markieren Sie die Eigenschaften und Methoden wie der Gestaltungsvorlage `Public`. Wenn nicht explizit dieser Eigenschaften und Methoden als bezeichnen `Public`, sie können nicht aus der Seite Inhalt zugegriffen werden.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Schritt 4: Aufrufen der Gestaltungsvorlage öffentliche Member aus einer Inhaltsseite

Nun, da die Gestaltungsvorlage erforderlichen öffentlichen Eigenschaften und Methoden verfügt, können wir bereit, um diese Eigenschaften und Methoden aufzurufen der `AddProduct.aspx` Inhaltsseite. Insbesondere müssen wir die Gestaltungsvorlage festgelegt `GridMessageText` -Eigenschaft, und rufen die `RefreshRecentProductsGrid` -Methode auf, nachdem das neue Produkt mit der Datenbank hinzugefügt wurde. Alle der ASP.NET Web Datensteuerelemente auslösen Ereignisse, unmittelbar vor und nach dem Ausführen verschiedener Aufgaben, die Seitenentwicklern die programmgesteuerte Maßnahmen entweder vor oder nach der Aufgabe zu erleichtern. Angenommen, wenn der Endbenutzer die DetailsView Insert Schaltfläche klickt, auf postback der DetailsView löst die `ItemInserting` Ereignis vor dem Einfügen von Workflows. Dann wird den Datensatz in die Datenbank eingefügt. Danach, DetailsView löst die `ItemInserted` Ereignis. Aus diesem Grund, um die Gestaltungsvorlage arbeiten möchten, nachdem das neue Produkt hinzugefügt wurde, für die DetailsView einen Ereignishandler erstellen `ItemInserted` Ereignis.

Es gibt zwei Möglichkeiten, die eine Inhaltsseite programmgesteuert mit Gestaltungsvorlage Schnittstelle kann ein:

- Mithilfe der `Page.Master` Eigenschaft, die einen lose typisierten Verweis auf die Gestaltungsvorlage zurückgibt, oder
- Geben Sie die Seite Masterseite Typ oder ein Dateipfad über eine `@MasterType` Richtlinie; dies Fügt eine stark typisierte Eigenschaft automatisch auf der Seite mit dem Namen `Master`.

Betrachten wir die beiden Ansätzen.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Mithilfe der lose typisierten`Page.Master`Eigenschaft

Alle ASP.NET-Webseiten leiten sich aus der `Page` -Klasse, befindet sich im die `System.Web.UI` Namespace. Die `Page` Klasse enthält eine [ `Master` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.page.master.aspx) , die einen Verweis auf die Masterseite zurückgibt. Wenn die Seite keine Masterseite `Master` gibt `Nothing`.

Die `Master` Eigenschaft gibt ein Objekt vom Typ [ `MasterPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.masterpage.aspx) (befindet sich auch der `System.Web.UI` Namespace) ist der Basistyp, von dem alle Masterseiten abgeleitet. Aus diesem Grund auf mit öffentlichen Eigenschaften oder Methoden, die auf unserer Website Masterseite müssen wir umgewandelt definiert die `MasterPage` Objekt, das von der `Master` Eigenschaft in den entsprechenden Typ. Da wir unsere Masterseitendatei mit dem Namen `Site.master`, wurde die Code-Behind-Klasse mit dem Namen `Site`. Im folgenden code aus diesem Grund Umwandlungen der `Page.Master` Eigenschaft zu einer Instanz von der `Site` Klasse.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Nun mit dem umgewandelte die lose typisierten `Page.Master` Eigenschaft auf den "Standort" können wir verweisen, die Eigenschaften und Methoden, die spezifisch für den Standort. Wie Abbildung 7 zeigt ein Beispiel, das die öffentliche Eigenschaft `GridMessageText` wird in der IntelliSense-Dropdownliste angezeigt.


[![IntelliSense zeigt unseren Gestaltungsvorlage öffentlichen Eigenschaften und Methoden](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Abbildung 07**: zeigt IntelliSense unsere Gestaltungsvorlage öffentlichen Eigenschaften und Methoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Wenn Sie Ihre Masterseitendatei genannt `MasterPage.master` handelt, die Gestaltungsvorlage Code-Behind-Klassenname `MasterPage`. Kann dies zu mehrdeutigen Code bei der Umwandlung vom Typ `System.Web.UI.MasterPage` auf Ihre `MasterPage` Klasse. Kurz gesagt, müssen Sie zur vollständigen Qualifizierung den Typ, dem Sie umwandeln, der kann ein bisschen sein, wenn das Modell der Website-Projekt verwenden. Ich schlage wäre entweder sicherstellen, dass bei der Erstellung der Masterseite Sie es etwas anderen als Namen `MasterPage.master` oder besser noch, erstellen Sie einen stark typisierten Verweis auf die Gestaltungsvorlage.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Erstellen einen stark typisierten Verweis mit der`@MasterType`Richtlinie

Bei näherer Betrachtung können Sie sehen, dass eine ASP.NET-Seite Code-Behind-Klasse eine partielle Klasse ist (Beachten Sie die `Partial` Schlüsselwort in der Klassendefinition). Partielle Klassen in c# und Visual Basic.NET Framework 2.0 eingeführt wurden und kurz gesagt, ermöglichen die Member einer Klasse auf mehrere Dateien definiert werden. Die Code-Behind-Klassendatei - `AddProduct.aspx.vb`, z. B. - enthält den Code, den Sie, der Entwickler der Seite erstellen. Zusätzlich zu den getesteten Codes das Modul für ASP.NET erstellt automatisch eine separate Klassendatei mit Eigenschaften und Ereignishandler, deklarative Markup in Klassenhierarchie der Seite übersetzen.

Die automatische codegenerierung, die Fehler tritt bei eine ASP.NET-Seite besuchten kontextspezifischen einige vielmehr interessanten und nützliche Möglichkeiten. Im Fall von Masterseiten, wenn wir das Modul für ASP.NET Teilen von unseren Inhaltsseite welche Gestaltungsvorlage verwendet wird generiert er einen stark typisierten `Master` -Eigenschaft für uns.

Verwenden der [ `@MasterType` Richtlinie](https://msdn.microsoft.com/en-us/library/ms228274.aspx) des ASP.NET-Moduls für die master Seitentyp der Seite Inhalt informiert. Die `@MasterType` Richtlinie kann entweder der Typname der Masterseite oder des Dateipfads akzeptieren. Um anzugeben, dass die `AddProduct.aspx` Seite verwendet `Site.master` als Gestaltungsvorlage, fügen Sie die folgende Direktive am Anfang `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Diese Direktive weist das ASP.NET-Modul um einen stark typisierten Verweis auf die Gestaltungsvorlage über eine Eigenschaft mit dem Namen hinzufügen `Master`. Mit der `@MasterType` Richtlinie vorhanden, wir können Aufrufen der `Site.master` "master" öffentlichen Eigenschaften und Methoden, die direkt über der Seite der `Master` Eigenschaft ohne alle Umwandlungen.

> [!NOTE]
> Wenn Sie weglassen der `@MasterType` Richtlinie, die Syntax `Page.Master` und `Master` dasselbe zurück: eine lose typisierten Objekt auf der Masterseite. Wenn Sie enthalten die `@MasterType` Richtlinie dann `Master` gibt einen stark typisierten Verweis auf die angegebene Masterseite. `Page.Master`, jedoch weiterhin einen lose typisierten Verweis zurückgegeben. Ein tiefer gehendes Betrachtung verdeutlichen, warum dies der Fall ist und wie die `Master` Eigenschaft erstellt wird bei der `@MasterType` Richtlinie enthalten ist, finden Sie unter [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)des Blogeintrag [ `@MasterType` in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualisieren die Seite "Master" nach dem Hinzufügen eines neuen Produkts

Nun, da wir wissen, wie einer Masterseite öffentlichen Eigenschaften und Methoden aus einer Inhaltsseite aufrufen, können wir zur Aktualisierung der `AddProduct.aspx` Seite, damit die Gestaltungsvorlage aktualisiert wird, nach dem Hinzufügen eines neuen Produkts. Am Anfang des Schritt 4 erstellten einen Ereignishandler für des DetailsView-Steuerelements `ItemInserting` Ereignis, das ausgeführt wird, sofort, nachdem das neue Produkt mit der Datenbank hinzugefügt wurde. Fügen Sie zu diesem Ereignishandler den folgenden Code hinzu:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

Der Code oben verwendet sowohl die lose typisierten `Page.Master` -Eigenschaft und die stark typisierte `Master` Eigenschaft. Beachten Sie, dass die `GridMessageText` -Eigenschaftensatz auf "*ProductName* Raster... hinzugefügt" Das gerade hinzugefügte Produkt Werte ist möglich, über die `e.Values` Auflistung, wie Sie sehen können, der gerade hinzugefügten `ProductName` Wert erfolgt über `e.Values("ProductName")`.

Abbildung 8 zeigt die `AddProduct.aspx` Seite unmittelbar nach einem neuen Produkt - Scotts Soda - Datenbank hinzugefügt wurde. Beachten Sie, dass der gerade hinzugefügten Produktnamen in der Masterseite Bezeichnung angegeben ist und die GridView aktualisiert worden sind, um das Produkt und ihren Preis einzuschließen.


[![Die Gestaltungsvorlage Bezeichnung und GridView anzeigen, das gerade hinzugefügte Produkt](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Abbildung 08**: die Gestaltungsvorlage Bezeichnung und GridView Anzeigen des Produkts Just-Added ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Zusammenfassung

Im Idealfall einer Masterseite und die Inhaltsseiten vollständig voneinander getrennt sind, und Sie erfordern keine Ebene Interaktion. Während Gestaltungsvorlagen und Inhaltsseiten mit diesem Ziel verwenden, beachten Sie gestaltet werden soll, gibt es diverse häufig vorkommende Szenarien, in denen Inhaltsseite mit Gestaltungsvorlage Schnittstelle muss. Einer der häufigsten Gründe dreht sich alles um Aktualisieren eines bestimmten Teils der Gestaltungsvorlage Anzeige basierend auf eine Aktion, die auf der Inhaltsseite vergangen.

Die gute Nachricht ist, dass es relativ einfach zu eine Inhaltsseite programmgesteuerte Interaktion mit Gestaltungsvorlage haben. Starten Sie durch das Erstellen von öffentlichen Eigenschaften oder Methoden in der master-Seite, die die Funktionalität zu kapseln, die von einer Inhaltsseite aufgerufen werden muss. Klicken Sie dann auf der Inhaltsseite Zugriff auf der Gestaltungsvorlage Eigenschaften und Methoden über den typenlosen `Page.Master` Eigenschaft oder mithilfe der `@MasterType` Direktive, um einen stark typisierten Verweis auf die Gestaltungsvorlage erstellen.

In den nächsten Lernprogrammen untersuchen wir wie die programmgesteuerte Interaktion mit einem entsprechenden Inhaltsseiten Gestaltungsvorlage.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET Masterseiten: Tipps, Tricks und Traps](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType`in ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Übergeben von Informationen zwischen dem Inhalt und Masterseiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in ASP.NET-Lernprogramme](../../data-access/index.md)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Zack Jones. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](control-id-naming-in-content-pages-vb.md)
[Weiter](interacting-with-the-content-page-from-the-master-page-vb.md)
