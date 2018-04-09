---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: Abfragen von Daten mit SqlDataSource-Steuerelement (VB) | Microsoft Docs
author: rick-anderson
description: In den vorherigen Lernprogrammen verwendet wurde das ObjectDataSource-Steuerelement, um die Darstellungsschicht von der Datenzugriffsschicht vollständig zu trennen. Mit diesem Tutor wird gestartet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6f886ca85a2a4dea5daeff109370bedc1a3f7265
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>Abfragen von Daten mit SqlDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe) oder [PDF herunterladen](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> In den vorherigen Lernprogrammen verwendet wurde das ObjectDataSource-Steuerelement, um die Darstellungsschicht von der Datenzugriffsschicht vollständig zu trennen. Mit diesem Lernprogramm beginnen, lernen wir, wie die SqlDataSource-Steuerelement für einfache Anwendungen verwendet werden kann, die keine solche eine strikte Trennung von Darstellung und Datenzugriff erforderlich sind.


## <a name="introduction"></a>Einführung

Alle Lernprogramme wir haben bisher untersucht eine mehrstufige Architektur, die Präsentation, Geschäftslogik und Datenzugriff Ebenen besteht verwendet haben. (Data Access Layer, DAL) wurde so konzipiert, im ersten Lernprogramm ([erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md)) und der Business Logic Layer in der zweiten ([erstellen eine Geschäftslogikschicht](../introduction/creating-a-business-logic-layer-vb.md)). Beginnend mit der [Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) Lernprogramm wurde erläutert, wie neue s-ObjectDataSource-Steuerelement von ASP.NET 2.0 zu verwenden, um deklarativ eine Schnittstelle mit der Architektur von der Darstellungsschicht.

Während alle bisher die Lernprogramme die Architektur zum Arbeiten mit Daten verwendet haben, kann es auch zugreifen, einfügen, aktualisieren und Löschen von Datenbankdaten direkt von einer ASP.NET-Seite unter Umgehung der Architektur. Auf diese Weise wird der angegebenen Datenbankabfragen und Geschäftslogik direkt auf der Webseite. Ausreichend große oder komplexe Anwendungen ist es äußerst wichtig für Erfolg, aktualisierbarkeit und verwaltbarkeit der Anwendung entwerfen, implementieren und Verwenden einer mehrstufigen Architektur. Entwickeln eine robuste Architektur, kann jedoch nicht erforderlich sein, wenn äußerst einfache, einmalige Anwendungen zu erstellen.

ASP.NET 2.0 verfügt über fünf integrierte Datenquellensteuerelementen [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Zugreifen auf und Ändern von Daten direkt aus einer relationalen Datenbank, einschließlich Microsoft SQL Server, Microsoft Access, Oracle, MySQL usw., kann die SqlDataSource verwendet werden. In diesem Lernprogramm und den nächsten drei untersuchen wir zum Arbeiten mit SqlDataSource-Steuerelement einfügen, aktualisieren und Löschen von Daten zum Abfragen und Filter-Datenbankdaten sowie zum Verwenden der SqlDataSource zu untersuchen.


![ASP.NET 2.0 enthält fünf integrierte Datenquellensteuerelemente](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**Abbildung 1**: ASP.NET 2.0 enthält fünf integrierte Datenquellensteuerelemente


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Vergleichen der ObjectDataSource und SqlDataSource

Sowohl die SqlDataSource das ObjectDataSource-Steuerelemente sind im Prinzip einfach Proxys auf Daten. Entsprechend der Anleitung unter dem [Anzeigen von Daten mit der ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md) Lernprogramm, das ObjectDataSource verfügt über Eigenschaften, die den Objekttyp an, die bereitstellt, die Daten und die Methoden aufrufen, um auszuwählen, einfügen, aktualisieren und Löschen von Daten aus der zugrunde liegenden Objekttyp. Sobald das ObjectDataSource-s-Eigenschaften konfiguriert wurden, können Daten wie eine GridView, DetailsView oder DataList-Websteuerelement gebunden werden, an das Steuerelement, mit dem ObjectDataSource-s `Select()`, `Insert()`, `Delete()`, und `Update()` Methoden interagieren Sie mit der zugrunde liegenden Architektur.

Die ': SqlDataSource bietet die gleiche Funktionalität aber arbeitet mit einer relationalen Datenbank anstatt auf eine Objektbibliothek. Mit dem SqlDataSource wir müssen angeben, die Datenbank-Verbindungszeichenfolge und der Ad-hoc-SQL-Abfragen oder gespeicherte Prozeduren zum Einfügen, aktualisieren, löschen und Abrufen von Daten ausgeführt. Die ': SqlDataSource s `Select()`, `Insert()`, `Update()`, und `Delete()` Methoden, wenn der Aufruf erfolgte, Herstellen einer Verbindung mit der angegebenen Datenbank, und geben Sie die entsprechenden SQL-Abfrage. Wie das folgende Diagramm veranschaulicht, die diese Methoden führen Sie die schwierige Arbeit, Herstellen einer Verbindung mit einer Datenbank Ausgeben einer Abfrage und Zurückgeben der Ergebnisse.


![Die ': SqlDataSource fungiert als Proxy für die Datenbank](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**Abbildung 2**: die ': SqlDataSource fungiert als Proxy für die Datenbank


> [!NOTE]
> In diesem Lernprogramm konzentrieren wir uns über das Abrufen von Daten aus der Datenbank. In der [einfügen, aktualisieren und Löschen des-Daten mit SqlDataSource-Steuerelement](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md) Tutorial, sehen wir die SqlDataSource zur Unterstützung von einfügen, aktualisieren und Löschen von konfigurieren.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Dem SqlDataSource und AccessDataSource-Steuerelement

Neben den SqlDataSource-Steuerelement enthält ASP.NET 2.0 auch einem AccessDataSource-Steuerelement. Diese zwei verschiedene Steuerelemente führen viele Entwickler neue in ASP.NET 2.0 zu vermuten, dass das AccessDataSource-Steuerelement für die Verwendung konzipiert ist ausschließlich mit Microsoft Access mit dem SqlDataSource-Steuerelement, das ausschließlich mit Microsoft SQL Server funktionieren. Während der AccessDataSource arbeiten mit Microsoft Access speziell konzipiert ist, funktioniert die SqlDataSource-Steuerelement mit *alle* relationale Datenbank, die über .NET zugegriffen werden kann. Dies schließt alle OLE DB oder ODBC-kompatible Datenspeicher wie z. B. Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL und PostgreSQL, u. a. viele.

Der einzige Unterschied zwischen den Steuerelementen AccessDataSource und SqlDataSource ist wie die Datenbank-Verbindungsinformationen angegeben wird. AccessDataSource-Steuerelement benötigt nur den Pfad zum Access-Datenbankdatei an. Die SqlDataSource erfordert andererseits, eine vollständige Verbindungszeichenfolge.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Schritt 1: Erstellen der Webseiten ': SqlDataSource

Bevor wir beginnen, arbeiten Sie direkt mit der Datenbankdaten mit SqlDataSource-Steuerelement zu untersuchen, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und den nächsten drei benötigen. Starten, indem Sie einen neuen Ordner namens `SqlDataSource`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**Abbildung 3**: Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu


Wie in den anderen Ordnern `Default.aspx` in den `SqlDataSource` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**Abbildung 4**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


Abschließend fügen Sie diese vier Seiten als Einträge an die `Web.sitemap` Datei. Insbesondere das folgende Markup nach dem Hinzufügen von benutzerdefinierten Schaltflächen hinzufügen, auf das DataList und Repeater `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun Elemente für die Bearbeitung, einfügen und Löschen von Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die ': SqlDataSource-Lernprogramme](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**Abbildung 5**: die Siteübersicht enthält jetzt die Einträge für die ': SqlDataSource-Lernprogramme


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Schritt 2: Hinzufügen und konfigurieren die SqlDataSource-Steuerelement

Öffnen Sie zunächst die `Querying.aspx` auf der Seite der `SqlDataSource` Ordner und zur Entwurfsansicht zu wechseln. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox auf die Designer, und legen seine `ID` auf `ProductsDataSource`. Wie bei der ObjectDataSource die SqlDataSource gerenderte Ausgabe erstellt und daher als eine "graues Feld" auf der Entwurfsoberfläche angezeigt. Um die SqlDataSource zu konfigurieren, klicken Sie auf den Link "Datenquelle konfigurieren" aus dem SqlDataSource-s-Smarttag.


![Klicken Sie auf die Datenquellenlink aus dem Smarttag des SqlDataSource s konfigurieren](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**Abbildung 6**: Klicken Sie auf die Datenquellenlink aus dem Smarttag des SqlDataSource s konfigurieren


Daraufhin wird die SqlDataSource-Steuerelement-Assistent zum Konfigurieren von Datenquellen der s. Während die Schritte im Assistenten s über das ObjectDataSource-Steuerelement s unterscheidet, ist das Ziel, geben Sie die Details zum Abrufen, einfügen, aktualisieren und Löschen von Daten durch die Datenquelle identisch. Dies umfasst die SqlDataSource angeben der zugrunde liegenden Datenbank verwendet und die Angabe der Ad-hoc-SQL-Anweisungen oder gespeicherte Prozeduren.

Der erste Assistentenschritt werden uns aufgefordert, für die Datenbank. Die Dropdownliste enthält diese Datenbanken finden in der Web-Anwendung s `App_Data` Ordner auch solche, die die Daten-Verbindungsknotens im Server-Explorer hinzugefügt wurden. Seit wir hinzugefügt gibt es bereits eine Verbindungszeichenfolge für die `NORTHWIND.MDF` -Datenbank in die `App_Data` Ordner unsere Projekt s `Web.config` Datei, die Dropdownliste enthält einen Verweis auf diese Verbindungszeichenfolge `NORTHWINDConnectionString`. Wählen Sie dieses Element aus der Dropdown-Liste, und klicken Sie auf Weiter.


![Wählen Sie die NORTHWINDConnectionString aus der Dropdown Liste](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**Abbildung 7**: Wählen Sie die `NORTHWINDConnectionString` aus der Dropdown-Liste


Nach dem Auswählen der Datenbank, fragt Sie der Assistent für die Abfrage Daten zurückgibt. Wir können entweder die Spalten einer Tabelle oder Sicht zum Zurückgeben oder geben entweder eine benutzerdefinierte SQL­Anweisung oder einer gespeicherten Prozedur angeben. Sie können Umschalten zwischen diese Entscheidung über die angeben, eine benutzerdefinierte SQL­Anweisung oder gespeicherte Prozedur und Spalten aus einer Tabelle angeben oder Anzeigen von Optionsfeldern.

> [!NOTE]
> In diesem ersten Beispiel können Sie s, verwenden Sie die Spalten von einer Tabelle oder Sicht-Option angeben. Wir später in diesem Lernprogramm zum Assistenten zurückkehren und untersuchen angeben, eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Option.


Abbildung 8 zeigt konfigurieren den Bildschirm Select-Anweisung, wenn die Spalten angeben, über ein Optionsfeld Tabelle oder Sicht aktiviert ist. Die Dropdownliste enthält den Satz von Tabellen und Sichten in der Datenbank Northwind mit der ausgewählten Tabelle oder Sicht s Spalten in der nachfolgenden Liste mit Kontrollkästchen angezeigt. In diesem Beispiel können s Zurückgeben der `ProductID`, `ProductName`, und `UnitPrice` Spalten aus der `Products` Tabelle. Wie in Abbildung 8 gezeigt, nachdem eine Auswahl treffen diese des Assistenten die resultierende SQL-Anweisung zeigt, `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Zurückgeben von Daten aus der Products-Tabelle](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**Abbildung 8**: Zurückgeben von Daten aus der `Products` Tabelle


Nach der Konfiguration des Assistenten werden die `ProductID`, `ProductName`, und `UnitPrice` Spalten aus der `Products` Tabelle, klicken Sie auf die Schaltfläche "Weiter". Dieser letzten Bildschirm bietet die Möglichkeit zum Untersuchen der Ergebnisse der Abfrage aus dem vorherigen Schritt konfiguriert. Klicken auf die Schaltfläche "Testabfrage" führt die konfigurierte `SELECT` -Anweisung und zeigt die Ergebnisse in einem Raster.


![Klicken Sie auf die Schaltfläche "Abfrage testen", um die SELECT-Abfrage zu überprüfen](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**Abbildung 9**: Klicken Sie auf die Schaltfläche "Abfrage testen", überprüfen Sie Ihre `SELECT` Abfrage


Klicken Sie auf "Fertig stellen", um den Assistenten zu beenden.

Wie Sie mit der ObjectDataSource SqlDataSource-s-Assistenten nur Werte der Steuerelementeigenschaften s, nämlich zuweist der [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) und [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) Eigenschaften. Nach Abschluss des Assistenten, sollte SqlDataSource-Steuerelement s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

Die `ConnectionString` Eigenschaft enthält Informationen zum Verbinden mit der Datenbank. Diese Eigenschaft kann einen Wert vollständig, hartcodierte Verbindungszeichenfolge zugewiesen werden oder auf eine Verbindungszeichenfolge in verweisen `Web.config`. Um eine verbindungszeichenwert in "Web.config" zu verweisen, verwenden Sie die Syntax `<%$ expressionPrefix:expressionValue %>`. In der Regel *ExpressionPrefix* ist ConnectionStrings und *ExpressionValue* ist der Name der Verbindungszeichenfolge in der `Web.config` [ `<connectionStrings>` Abschnitt](https://msdn.microsoft.com/library/bf7sd233.aspx). Die Syntax kann jedoch verwendet werden, Verweis `<appSettings>` Elemente oder Inhalte aus Ressourcendateien. Finden Sie unter [Übersicht über ASP.NET-Ausdrücke](https://msdn.microsoft.com/library/d5bd1tad.aspx) für Weitere Informationen über diese Syntax.

Die `SelectCommand` Eigenschaft gibt an, die Ad-hoc-SQL-Anweisung oder gespeicherte Prozedur ausgeführt wird, um die Daten zurückzugeben.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Schritt 3: Hinzufügen eines Daten-Web-Steuerelements, und binden es an die ': SqlDataSource

Nachdem die SqlDataSource konfiguriert wurde, können sie mit einer Datenquelle, z. B. eine GridView oder DetailsView-Websteuerelement gebunden werden. Können Sie für dieses Lernprogramm s, die die Daten in einem GridView anzuzeigen. Eine GridView aus der Toolbox auf die Seite ziehen und binden Sie es an die `ProductsDataSource` SqlDataSource durch Auswählen der Datenquelle aus der Dropdown-Liste im GridView s Smarttag.


[![Hinzufügen einer GridView und Binden an das SqlDataSource-Steuerelement](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**Abbildung 10**: hinzufügen eine GridView und Binden an das SqlDataSource-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


Nachdem Sie Ve SqlDataSource-Steuerelement aus der Dropdown-Liste in das Smarttag für GridView s ausgewählt, wird Visual Studio automatisch eine BoundField- oder CheckBoxField hinzufügen an die GridView für jede der Datenquellen-Steuerelements zurückgegebenen Spalten. Da die ': SqlDataSource gibt drei Spalten Datenbank `ProductID`, `ProductName`, und `UnitPrice` drei Felder in die GridView vorhanden sind.

So konfigurieren Sie die GridView-Zuordnungsvorgänge drei erkundet BoundFields. Ändern der `ProductName` Feld s `HeaderText` Product Name-Eigenschaft und die `UnitPrice` Feld s zum Preis. Formatieren Sie auch die `UnitPrice` Feld als Währung. Nachdem diese Änderungen vorgenommen wurden, sollte Ihre GridView s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

Besuchen Sie diese Seite über einen Browser. Wie Abbildung 11 zeigt, enthält die GridView jedes Produkt s `ProductID`, `ProductName`, und `UnitPrice` Werte.


[![Zeigt die GridView, jedes Produkt s ProductID, ProductName und UnitPrice-Werte](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**Abbildung 11**: die GridView zeigt jedes Produkt s `ProductID`, `ProductName`, und `UnitPrice` Werte ([klicken Sie hier, um das Bild in voller Größe angezeigt](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


Besucht die Seite ruft die GridView seine Datenquellensteuerelement s `Select()` Methode. Wenn wir das ObjectDataSource-Steuerelement verwendeten, bezeichnet dies die `ProductsBLL` Klasse s `GetProducts()` Methode. Mit SqlDataSource, jedoch die `Select()` Methode stellt eine Verbindung mit der angegebenen Datenbank und Probleme der `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, in diesem Beispiel). Die ': SqlDataSource gibt ihre Ergebnisse klicken Sie dann die GridView auflistet, erstellen eine Zeile in die GridView für jeden Datensatz der Datenbank zurückgegeben.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Die integrierte Features von Web und SqlDataSource-Steuerelement

Im Allgemeinen gilt: die Funktionen, die inhärente an den Daten, die websteuerungselemente, paging, sortieren, bearbeiten, löschen, einfügen und So weiter sind spezifisch für das Websteuerelement Daten und hängen nicht von der Datenquellen-Steuerelements verwendet. D. h. kann die GridView nutzen, den integrierten paging, sortieren, bearbeiten und Löschen von, ob er an einem ObjectDataSource oder SqlDataSource gebunden ist. Allerdings sind bestimmte Funktionen der Web-Daten vertrauliche, Datenquellen-Steuerelements verwendet wird oder das Steuerelement s Datenquellenkonfiguration.

Beispielsweise ist in der [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) Lernprogramm besprochen, wie, wird standardmäßig die Auslagerung Logik für die Daten Web naiv gibt Steuerelemente *alle* Datensätze aus der zugrunde liegenden Data source und zeigt dann nur die entsprechenden Teilmenge der Datensätze im angegebenen Index der aktuellen Seite und die Anzahl der Datensätze pro Seite angezeigt. Dieses Modell ist sehr effizient, wenn paging durch ausreichend große Resultsets. Glücklicherweise kann das ObjectDataSource konfiguriert werden, um benutzerdefinierte Paging zu unterstützen, das nur die präzise Teilmenge der Datensätze zurückgibt. SqlDataSource-Steuerelement verfügt jedoch nicht über die Eigenschaften für das benutzerdefiniertes Paging implementieren.

Tritt auf, eine andere Besonderheit mit paging und Sortieren mit der SqlDataSource. Standardmäßig können von einem SqlDataSource zurückgegebenen Daten ausgelagert oder über die GridView sortiert werden. Um dies zu demonstrieren, überprüfen Sie die Optionen "Aktivieren von Paging und Sortieren aktivieren" im GridView Smarttag s in `Querying.aspx` und stellen Sie sicher, dass diese erwartungsgemäß funktioniert.

Sortieren und paging funktioniert, da die ': SqlDataSource in einer lose typisierten DataSet Daten in der Datenbank abruft. Die Gesamtzahl der Datensätze, die von der Abfrage zurückgegebenen einen wesentlichen Aspekt Implementieren von Paging kann aus dem DataSet ermittelt werden. Darüber hinaus können die DataSet-s-Ergebnisse mit einer "DataView" sortiert werden. Diese Funktionen werden automatisch durch die ': SqlDataSource verwendet, wenn Anforderungen GridView ausgelagert oder Daten sortierte.

Die SqlDataSource kann konfiguriert werden, zum Zurückgeben eines "DataReader" anstelle eines Datasets durch Ändern der [ `DataSourceMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) aus `DataSet` (Standard) zu `DataReader`. Mithilfe ein "DataReader" kann in Situationen bevorzugt wird, wenn SqlDataSource-s-Ergebnisse am vorhandenen Code übergeben, die ein "DataReader" erwartet. Da DataReaders erheblich einfacher Objekte als DataSets sind, bieten sie darüber hinaus eine bessere Leistung. Wenn Sie diese Änderung vornehmen, jedoch das Websteuerelement Daten kann weder sortieren noch Seite, da die ': SqlDataSource kann nicht ermitteln, wie viele Datensätze von der Abfrage zurückgegeben werden, ebenso wie nicht DataReader bieten alle Verfahren für die zurückgegebenen Daten zu sortieren.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Schritt 4: Verwenden einer benutzerdefinierten SQL-Anweisung oder gespeicherten Prozedur

Beim Konfigurieren der SqlDataSource-Steuerelement kann zum Zurückgeben von Daten verwendete Abfrage in einem der beiden Ansätze als eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur oder als Spalten aus einer vorhandenen Tabelle oder Sicht angegeben werden. Wählen Sie in Schritt2 untersucht Spalten aus der `Products` Tabelle. Können Sie sehen Sie sich mit einer benutzerdefinierten SQL-Anweisung s an.

Fügen Sie ein anderes GridView-Steuerelement an die `Querying.aspx` Seite und wählen Sie aus der Dropdown-Liste im Smarttag eine neue Datenquelle zu erstellen. Als Nächstes anzugeben, dass die Daten aus einer Datenbank dies gezogen werden, wird ein neues SqlDataSource-Steuerelement erstellt. Benennen Sie das Steuerelement `ProductsWithCategoryInfoDataSource`.


![Erstellen Sie ein neues SqlDataSource-Steuerelement mit dem Namen ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**Abbildung 12**: Erstellen Sie ein neues SqlDataSource-Steuerelement mit dem Namen `ProductsWithCategoryInfoDataSource`


Der nächste Bildschirm fordert uns auf die Datenbank anzugeben. Wählen Sie wie in Abbildung 7 zurück, die `NORTHWINDConnectionString` aus der Dropdown-Liste, und klicken Sie auf Weiter. Konfigurieren Sie den Bildschirm Select-Anweisung wählen Sie angeben aus, eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur Optionsfeld aus, und klicken Sie auf Weiter. Hierdurch wird der Bildschirm definieren benutzerdefinierte Anweisungen oder gespeicherte Prozeduren, die Registerkarten, die mit der Bezeichnung SELECT, UPDATE, INSERT und DELETE bietet angezeigt. Auf jeder Registerkarte können Sie eine benutzerdefinierte SQL­Anweisung in das Textfeld eingeben oder wählen Sie eine gespeicherte Prozedur aus der Dropdown Liste. In diesem Lernprogramm betrachten wir eine benutzerdefinierte SQL­Anweisung eingeben; das nächste Lernprogramm enthält ein Beispiel, eine gespeicherte Prozedur verwendet.


![Geben Sie eine benutzerdefinierte SQL-Anweisung oder wählen eine gespeicherte Prozedur aus](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**Abbildung 13**: Geben Sie eine benutzerdefinierte SQL-Anweisung oder eine gespeicherte Prozedur auswählen


Benutzerdefinierte SQL-Anweisung manuell in das Textfeld eingegeben werden kann oder grafisch konstruiert werden kann, indem Sie auf die Schaltfläche Abfrage-Generator. Aus den Abfrage-Generator oder das Textfeld ein, verwenden Sie die folgende Abfrage zurückzugebenden der `ProductID` und `ProductName` Felder aus der `Products` -Tabelle und verwendet dabei eine `JOIN` zum Abrufen des Produkts s `CategoryName` aus der `Categories` Tabelle:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![Sie können grafisch die Abfrage mithilfe der Abfrage-Generator erstellen](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**Abbildung 14**: Sie grafisch erstellen können, dass die Abfrage mit dem Abfrage-Generator


Nach dem Angeben der Abfrage an, klicken Sie auf Weiter, um zum Bildschirm Testabfrage zu fortfahren. Klicken Sie auf "Fertig stellen", um die SqlDataSource-Assistenten zu beenden.

Nach Abschluss des Assistenten müssen die GridView drei BoundFields hinzugefügt, Anzeigen der `ProductID`, `ProductName`, und `CategoryName` Spalten aus der Abfrage zurückgegeben, und führt die folgenden deklarativem Markup:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![Die GridView zeigt jede s Productid, Name und die zugeordnete Kategorienamen](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**Abbildung 15**: die GridView zeigt jede Produkt-s-ID, der Name und der Kategoriename verknüpft sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben wir gesehen, wie Abfragen und Anzeigen von Daten mit SqlDataSource-Steuerelement. Wie das ObjectDataSource dient die SqlDataSource als einem Proxy verwenden, stellt es einen deklarativen Ansatz für den Zugriff auf Daten aus. Seine Eigenschaften angeben, die Datenbank für die Verbindung und der SQL- `SELECT` zum Ausführen von Abfragen; sie können über das Fenster "Eigenschaften" oder mit der Datenquelle konfigurieren-Assistenten angegeben werden.

Die `SELECT` Abfragebeispiele in diesem Lernprogramm werden untersucht alle Datensätze aus der angegebenen Abfrage zurückgegeben. SqlDataSource-Steuerelement kann jedoch enthalten eine `WHERE` -Klausel mit Parametern, deren Werte programmgesteuert zugeordnet sind, oder automatisch aus einer bestimmten Quelle abgerufen werden. Untersucht, wie zum Erstellen und verwenden Sie parametrisierte Abfragen in den nächsten Lernprogrammen!

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf Daten der relationalen Datenbank](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource-Steuerelement (Übersicht)](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [ASP.NET Schnellstart-Lernprogrammen: SqlDataSource-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Die Datei "Web.config" `<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Datenbankverweis Connection-Zeichenfolge](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Susan Connery Bernadette Leigh und David Suru. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [Weiter](using-parameterized-queries-with-the-sqldatasource-vb.md)
