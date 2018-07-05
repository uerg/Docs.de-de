---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Abfragen von Daten mit dem SqlDataSource-Steuerelement (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials verwendet haben wir das ObjectDataSource-Steuerelement, um die Darstellungsschicht vollständig von der Datenzugriffsschicht zu trennen. Mit diesem Tutor wird gestartet...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 6b9ca474b7a085302d287a223c08abe2fa5336b0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815290"
---
<a name="querying-data-with-the-sqldatasource-control-c"></a>Abfragen von Daten mit dem SqlDataSource-Steuerelement (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) oder [PDF-Datei herunterladen](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> In den vorherigen Tutorials verwendet haben wir das ObjectDataSource-Steuerelement, um die Darstellungsschicht vollständig von der Datenzugriffsschicht zu trennen. In diesem Tutorial beginnen, wird erläutert, wie das SqlDataSource-Steuerelement, für einfache Anwendungen verwendet werden kann, die nicht mit eine strikte Trennung von Darstellung und den Datenzugriff benötigen.


## <a name="introduction"></a>Einführung

Alle in den Tutorials wir haben bisher untersucht eine geschichtete Architektur, die mit Präsentation, Geschäftslogik und Datenzugriff Ebenen verwendet haben. (Data Access Layer, DAL) im ersten Lernprogramm erstellt wurde ([Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md)) und den Geschäftslogikebene in der zweiten ([Erstellen einer Geschäftslogikebene](../introduction/creating-a-business-logic-layer-cs.md)). Beginnend mit der [Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Tutorial wurde erläutert, wie neues s-ObjectDataSource-Steuerelement von ASP.NET 2.0 zu verwenden, um mit der Architektur auf der Darstellungsschicht deklarativ Schnittstelle.

Während alle bisher in den Tutorials die Architektur zum Arbeiten mit Daten verwendet haben, ist es auch möglich, zugreifen, einfügen, aktualisieren und Löschen von Datenbankdaten direkt aus einer ASP.NET-Seite unter Umgehung der Architektur. Auf diese Weise wird der Datenbankabfragen und Geschäftslogik direkt auf der Webseite angezeigt. Bei ausreichend großen oder komplexen Anwendungen ist das Entwerfen, implementieren und verwenden eine geschichtete Architektur äußerst wichtig für den Erfolg, aktualisierbarkeit und wartbarkeit der Anwendung. Entwickeln eine robuste Architektur, kann jedoch nicht erforderlich sein, wenn äußerst einfache, einmalige Anwendungen erstellen.

ASP.NET 2.0 bietet fünf integrierten Datenquellen-Steuerelemente [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), ["ObjectDataSource"](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx), und [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). Dem SqlDataSource-Steuerelement kann verwendet werden, zugreifen auf und Ändern von Daten direkt aus einer relationalen Datenbank, einschließlich Microsoft SQL Server, Microsoft Access, Oracle, MySQL und andere. In diesem Tutorial und die folgenden drei untersuchen wir zum Arbeiten mit dem SqlDataSource-Steuerelement einfügen, aktualisieren und Löschen von Daten untersuchen, wie Sie Abfragen und Datenbank Filtern von Daten sowie wie Sie mit dem SqlDataSource-Steuerelement auf.


![ASP.NET 2.0 umfasst fünf integrierten Datenquellen-Steuerelemente](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Abbildung 1**: ASP.NET 2.0 umfasst fünf integrierten Datenquellen-Steuerelemente


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Vergleichen der ObjectDataSource und SqlDataSource-Steuerelement

Sowohl der ObjectDataSource und SqlDataSource-Steuerelemente sind im Prinzip einfach Proxys für Daten. Siehe die [Anzeigen von Daten mit dem ObjectDataSource-Steuerelement](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) Tutorial dem ObjectDataSource-Steuerelement verfügt über Eigenschaften, die den Objekttyp, die bereitstellt angeben, die Daten und die Methoden aufrufen, um auszuwählen, einfügen, aktualisieren und Löschen von Daten aus dem zugrunde liegenden Objekttyp. Sobald die "ObjectDataSource"-s-Eigenschaften konfiguriert wurden, ein Daten-Websteuerelement wie GridView, DetailsView oder DataList-Steuerelement gebunden werden können auf das Steuerelement, mit der "ObjectDataSource"-s `Select()`, `Insert()`, `Delete()`, und `Update()` Methoden interagieren Sie mit der zugrunde liegenden Architektur.

Dem SqlDataSource-Steuerelement bietet dieselbe Funktionalität, aber für eine relationale Datenbank, anstatt eine Objektbibliothek ausgeführt wird. Mit dem SqlDataSource-Steuerelement wir müssen angeben, die Datenbank-Verbindungszeichenfolge und die Ad-hoc-SQL-Abfragen oder gespeicherte Prozeduren zum Einfügen, aktualisieren, löschen und Abrufen von Daten ausgeführt. Die s SqlDataSource `Select()`, `Insert()`, `Update()`, und `Delete()` Methoden, wenn aufgerufen, eine Verbindung mit der angegebenen Datenbank herstellen, und geben Sie die entsprechenden SQL-Abfrage. Wie das folgende Diagramm veranschaulicht, die diese Methoden führen die Routinearbeit der Verbindung mit einer Datenbank, eine Abfrage und Zurückgeben der Ergebnisse.


![Dem SqlDataSource-Steuerelement dient als Proxy für die Datenbank](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Abbildung 2**: dem SqlDataSource-Steuerelement dient als Proxy für die Datenbank


> [!NOTE]
> In diesem Tutorial konzentrieren wir uns über das Abrufen von Daten aus der Datenbank. In der [einfügen, aktualisieren und Löschen von Daten mit dem SqlDataSource-Steuerelement](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) Tutorial wird gezeigt, wie mit dem SqlDataSource-Steuerelement zur Unterstützung von einfügen, aktualisieren und löschen zu konfigurieren.


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Dem SqlDataSource-Steuerelement und AccessDataSource-Steuerelement

Neben dem SqlDataSource-Steuerelement enthält ASP.NET 2.0 auch einem AccessDataSource-Steuerelement. Diese zwei verschiedene Steuerelemente führen viele Entwickler, die noch nicht mit ASP.NET 2.0 zu vermuten, dass das AccessDataSource-Steuerelement konzipiert ist, funktioniert exklusiv mit Microsoft Access mit dem SqlDataSource-Steuerelement, das ausschließlich mit Microsoft SQL Server funktionieren. Die AccessDataSource konzipiert ist insbesondere bei Microsoft Access, das SqlDataSource-Steuerelement arbeitet es mit *alle* relationale Datenbank, die über .NET zugegriffen werden kann. Dies schließt alle OLE DB oder ODBC-kompatiblen Datenspeichern, z.B. Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL und PostgreSQL neben vielen anderen.

Der einzige Unterschied zwischen den Steuerelementen AccessDataSource und SqlDataSource-Steuerelement ist, wie die Datenbank-Verbindungsinformationen angegeben wird. AccessDataSource-Steuerelement benötigt nur den Pfad zum Access-Datenbankdatei an. Dem SqlDataSource-Steuerelement erfordert dagegen auf eine vollständige Verbindungszeichenfolge.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Schritt 1: Erstellen von Webseiten SqlDataSource-Steuerelement

Bevor wir beginnen, arbeiten Sie direkt mit dem SqlDataSource-Steuerelement mit Daten der Datenbank untersuchen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die folgenden drei benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `SqlDataSource`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![Fügen Sie die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzu.](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Abbildung 3**: die ASP.NET-Seiten für die Lernprogramme SqlDataSource-bezogene hinzufügen


Wie in den anderen Ordnern `Default.aspx` in die `SqlDataSource` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Abbildung 4**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))


Abschließend fügen Sie diese vier Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach dem Hinzufügen von benutzerdefinierten Schaltflächen, zu dem DataList- und Wiederholungssteuerelement `<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Bearbeiten, einfügen und löschen die Lernprogramme.


![Die Sitemap enthält jetzt die Einträge für die Lernprogramme SqlDataSource-Steuerelement](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Abbildung 5**: die Sitemap enthält jetzt Einträge für die Lernprogramme SqlDataSource-Steuerelement


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Schritt 2: Hinzufügen und Konfigurieren von dem SqlDataSource-Steuerelement

Öffnen Sie zunächst die `Querying.aspx` auf der Seite die `SqlDataSource` Ordner und wechseln Sie zur Entwurfsansicht. Ziehen Sie ein SqlDataSource-Steuerelement aus der Toolbox in den Designer und den Satz der `ID` zu `ProductsDataSource`. Wie bei dem ObjectDataSource-Steuerelement, dem SqlDataSource-Steuerelement ist keine gerenderte Ausgabe erzeugt, und daher als ein graues Feld auf der Entwurfsoberfläche angezeigt. Um dem SqlDataSource-Steuerelement zu konfigurieren, klicken Sie auf den Link "Datenquelle konfigurieren" aus dem SqlDataSource-s-Smarttag.


![Klicken Sie auf den Link "Quelle" Daten aus dem SqlDataSource-s-Smarttag konfigurieren](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Abbildung 6**: Klicken Sie auf den Link "Quelle" Daten aus dem SqlDataSource-s-Smarttag konfigurieren


Dadurch wird s SqlDataSource-Steuerelement-konfigurieren von Datenquellen-Assistent. Während Sie die Schritte im Assistenten s über das ObjectDataSource-Steuerelement s unterscheiden zu können, ist das endgültige Ziel identisch sein, um die Details zum Abrufen, einfügen, aktualisieren und Löschen von Daten von der Datenquelle angeben. Dem SqlDataSource-Steuerelement umfasst dies das Angeben der zugrunde liegenden Datenbank zu verwenden und bereitstellen, die Ad-hoc-SQL-Anweisungen oder gespeicherten Prozeduren.

Der erste Assistentenschritt verlangt für die Datenbank. Die Dropdownliste enthält diese Datenbanken finden Sie in der Web Application s `App_Data` Ordner sowie diejenigen, die im Server-Explorer den Knoten "Datenverbindungen" hinzugefügt wurden. Seit wir hinzugefügt haben bereits eine Verbindungszeichenfolge für die `NORTHWIND.MDF` -Datenbank in die `App_Data` Ordner, um unser Projekt s `Web.config` -Datei, die Dropdownliste enthält einen Verweis auf diese Verbindungszeichenfolge `NORTHWINDConnectionString`. Wählen Sie dieses Element aus der Dropdown-Liste aus, und klicken Sie auf Weiter.


![Wählen Sie die NORTHWINDConnectionString aus der Dropdown Liste](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Abbildung 7**: Wählen Sie die `NORTHWINDConnectionString` aus der Dropdown-Liste


Nach dem Auswählen der Datenbank, fragt der Assistent für die Abfrage aus, um Daten zurückzugeben. Wir können entweder angeben, dass die Spalten einer Tabelle oder Sicht zum Zurückgeben oder eine benutzerdefinierte SQL-Anweisung eingeben und kann eine gespeicherte Prozedur angeben. Sie können zwischen dieser Option über der Specify, benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur und Spalten aus einer Tabelle angeben oder Anzeigen von Optionsfeldern.

> [!NOTE]
> In diesem ersten Beispiel können Sie s verwenden, die Spalten von einer Tabelle oder Sicht-Option angeben. Wir später in diesem Lernprogramm zum Assistenten zurückzukehren und Untersuchen der Specify, benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur-Option.


Abbildung 8 zeigt konfigurieren den Bildschirm für die Select-Anweisung, wenn die Spalten angeben, über eine Tabelle oder Sicht Optionsfeld ausgewählt ist. Die Dropdownliste enthält den Satz von Tabellen und Sichten in der Northwind-Datenbank mit der ausgewählten Tabelle oder Ansicht-s-Spalten in der folgenden Kontrollkästchen-Liste angezeigt. In diesem Beispiel können s Zurückgeben der `ProductID`, `ProductName`, und `UnitPrice` Spalten aus der `Products` Tabelle. Wie in Abbildung 8 gezeigt, nachdem die resultierende SQL-Anweisung eine Auswahl treffen diese des Assistenten angezeigt werden, `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.


![Zurückgeben von Daten aus der Tabelle Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Abbildung 8**: Zurückgeben von Daten aus der `Products` Tabelle


Nachdem Sie den Assistenten zum Zurückgeben konfiguriert haben die `ProductID`, `ProductName`, und `UnitPrice` Spalten aus der `Products` Tabelle, klicken Sie auf die Schaltfläche "Weiter". Diese letzten Bildschirm bietet die Möglichkeit, die Ergebnisse der Abfrage aus dem vorherigen Schritt konfiguriert untersuchen. Klicken Sie auf die Schaltfläche "Testen von Abfragen" führt den konfigurierten `SELECT` -Anweisung und zeigt die Ergebnisse in einem Raster.


![Klicken Sie auf die Schaltfläche zum Testen von Abfragen zum Überprüfen der SELECT-Abfrage](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Abbildung 9**: Klicken Sie auf die Schaltfläche zum Testen von Abfragen an die Prüfung Ihrer `SELECT` Abfrage


Klicken Sie auf "Fertig stellen", um den Assistenten zu beenden.

Wie Sie mit dem ObjectDataSource-Steuerelement, SqlDataSource s ordnet der Assistent nur Werte auf die Eigenschaften des Steuerelements s, nämlich die [ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) und [ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) Eigenschaften. Nach Abschluss des Assistenten, sollte Ihre SqlDataSource-Steuerelement s-deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

Die `ConnectionString` Eigenschaft enthält Informationen zur Verbindung mit der Datenbank. Mithilfe dieser Eigenschaft kann einen vollständigen, hartcodierte Verbindungszeichenfolgenwert zugewiesen werden oder auf eine Verbindungszeichenfolge im verweisen `Web.config`. Um einen Verbindungszeichenfolgenwert in der Datei "Web.config" zu verweisen, verwenden Sie die Syntax `<%$ expressionPrefix:expressionValue %>`. In der Regel *ExpressionPrefix* ist ConnectionStrings und *ExpressionValue* ist der Name der Verbindungszeichenfolge in der `Web.config` [ `<connectionStrings>` Abschnitt](https://msdn.microsoft.com/library/bf7sd233.aspx). Allerdings kann die Syntax verwendet werden zu verweisen `<appSettings>` Elemente oder Inhalt aus Ressourcendateien. Finden Sie unter [ASP.NET Ausdrücke Overview](https://msdn.microsoft.com/library/d5bd1tad.aspx) Weitere Informationen zu dieser Syntax.

Die `SelectCommand` Eigenschaft gibt an, die Ad-hoc-SQL-Anweisung oder gespeicherte Prozedur ausgeführt werden, um die Daten zurückgeben.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Schritt 3: Hinzufügen eines Daten-Web-Steuerelements, und binden es an dem SqlDataSource-Steuerelement

Nach dem SqlDataSource-Steuerelement konfiguriert wurde, kann es in einen Daten-Web-Steuerelement, z. B. einem GridView- oder DetailsView gebunden werden. In diesem Tutorial können Sie s, die die Daten in einer GridView anzeigen. Ziehen Sie aus der Toolbox einer GridView-Ansicht auf der Seite, und binden Sie es an der `ProductsDataSource` SqlDataSource-Steuerelement durch Auswählen der Datenquelle aus der Dropdown-Liste in den GridView-s-Smarttag.


[![Hinzufügen einer GridView-Ansicht, und ihn mit dem SqlDataSource-Steuerelement](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Abbildung 10**: Hinzufügen einer GridView-Ansicht, und ihn mit dem SqlDataSource-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))


Nachdem Sie haben das SqlDataSource-Steuerelement aus der Dropdown-Liste in den GridView-s-Smarttag ausgewählt, wird Visual Studio automatisch eine BoundField- oder CheckBoxField hinzufügen an die GridView für jede Spalte zurückgegeben, die für das Datenquellen-Steuerelement. Da es sich bei dem SqlDataSource-Steuerelement gibt drei Spalten Datenbank `ProductID`, `ProductName`, und `UnitPrice` stehen drei Felder in den GridView-Ansicht.

So konfigurieren Sie die GridView-Zuordnungsvorgänge drei in Ruhe BoundFields. Ändern der `ProductName` Feld s `HeaderText` Eigenschaft, um Product Name und die `UnitPrice` Feld s Preis. Formatieren der `UnitPrice` Feld als Währung. Nachdem diese Änderungen vorgenommen wurden, sollte Ihre GridView s deklarative Markup etwa wie folgt aussehen:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Besuchen Sie diese Seite über einen Browser ein. Wie in Abbildung 11 gezeigt, enthält die GridView jedes Produkt s `ProductID`, `ProductName`, und `UnitPrice` Werte.


[![Die GridView zeigt die einzelnen Produkt s ProductID, ProductName und UnitPrice-Werte](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Abbildung 11**: die GridView zeigt jedes Produkt s `ProductID`, `ProductName`, und `UnitPrice` Werte ([klicken Sie, um das Bild in voller Größe anzeigen](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))


Wenn die Seite besucht wird ruft GridView der Datenquellen-Steuerelement s `Select()` Methode. Wenn wir das ObjectDataSource-Steuerelement verwenden, dies bezeichnet die `ProductsBLL` Klasse s `GetProducts()` Methode. Mit dem SqlDataSource-Steuerelement, jedoch die `Select()` Methode richtet eine Verbindung mit der angegebenen Datenbank und die Probleme der `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, in diesem Beispiel). Dem SqlDataSource-Steuerelement gibt die Ergebnisse an die GridView dann erstellen eine Zeile in der GridView für die einzelnen Datenbank Datensätze zurückgegeben zählt, zurück.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Funktionen der integrierten Web- und dem SqlDataSource-Steuerelement

Im Allgemeinen gilt: die Features der websteuerungselemente, paging, sortieren, bearbeiten Daten, löschen, einfügen und So weiter sind spezifisch für das Websteuerelement Daten und hängen nicht von den Datenquellen-Steuerelement verwendet. GridView verwenden, also kann den integrierten paging, sortieren, bearbeiten und Löschen von, ob es ein ObjectDataSource-Steuerelement oder ein SqlDataSource-Steuerelement gebunden ist. Allerdings sind bestimmte Funktionen der Web-Daten vertrauliche, um das Datenquellen-Steuerelement verwendet wird oder die s-Steuerelement für die Datenquellenkonfiguration.

Z. B. in der [effizient Paging durch große Mengen von Daten](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) Tutorial erläutert, wie in der Standardeinstellung die Paginglogik für die Daten Web naiv gibt steuert *alle* Datensätze aus der zugrunde liegenden Data source und zeigt dann nur die entsprechenden Teilmenge der Datensätze im angegebenen Index der aktuellen Seite und die Anzahl der Datensätze pro Seite angezeigt. Dieses Modell ist sehr ineffizient, wenn paging durch ausreichend große Resultsets. Glücklicherweise kann dem ObjectDataSource-Steuerelement konfiguriert werden, um benutzerdefiniertes Paging zu unterstützen, das nur die genaue Teilmenge der anzuzeigenden Datensätze zurückgibt. Das SqlDataSource-Steuerelement, verfügt jedoch nicht über die Eigenschaften für die benutzerdefinierte Paginierung implementieren.

Tritt auf, eine andere Besonderheit mit Paginierung und Sortierung mit dem SqlDataSource-Steuerelement. Standardmäßig können die von einem SqlDataSource-Steuerelement zurückgegebenen Daten ausgelagert oder über die GridView sortiert werden. Um dies zu demonstrieren, überprüfen Sie die Optionen "Aktivieren von Paging und Sortieren aktivieren" im GridView Smarttag s in `Querying.aspx` und stellen Sie sicher, dass dies funktioniert wie erwartet.

Sortieren und paging funktioniert, da es sich bei dem SqlDataSource-Steuerelement in ein DataSet mit flexibler typbindung Ruft Daten in der Datenbank ab. Die Gesamtzahl der Datensätze, die von der Abfrage einen wichtiger Aspekt zum Implementieren von Paging zurückgegebenen kann aus dem DataSet überprüft werden. Darüber hinaus können die DataSet-s-Ergebnisse mit einer "DataView" sortiert werden. Diese Funktionen werden automatisch von dem SqlDataSource-Steuerelement verwendet, wenn die GridView-Anforderungen ausgelagert oder Daten sortierte.

Dem SqlDataSource-Steuerelement kann so konfiguriert werden, dass ein DataReader-Ziel anstelle eines Datasets als ändern zurückgegeben werden, dessen [ `DataSourceMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) aus `DataSet` (Standard), `DataReader`. Mit "DataReader" kann in Situationen bevorzugt wird, wenn die SqlDataSource-s-Ergebnisse am vorhandenen Code übergeben, die ein DataReader-Ziel erwartet. Da "DataReaders" deutlich einfacher Objekte als DataSets sind, bieten sie darüber hinaus eine bessere Leistung. Wenn Sie diese Änderung vornehmen, allerdings kann weder das Websteuerelement Daten sortieren, und die Seite, da es sich bei dem SqlDataSource-Steuerelement kann nicht ermitteln, wie viele Datensätze von der Abfrage zurückgegeben werden, ebenso wie das DataReader-Objekt nicht bieten alle Techniken zum Sortieren der zurückgegebenen Daten.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Schritt 4: Verwenden einer benutzerdefinierten SQL-Anweisung oder gespeicherte Prozedur

Wenn Sie das SqlDataSource-Steuerelement zu konfigurieren, kann die Abfrage verwendet, um Daten zurückzugeben in einem der beiden Methoden als eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur oder Spalten aus einer vorhandenen Tabelle oder Sicht angegeben werden. Wählen Sie in Schritt2, die wir untersucht die Spalten aus der `Products` Tabelle. Können Sie sehen Sie sich mit einer benutzerdefinierten SQL­Anweisung s ein.

Fügen Sie ein anderes GridView-Steuerelement, das `Querying.aspx` Seite, und wählen Sie aus der Dropdown-Liste im Smarttag eine neue Datenquelle zu erstellen. Als Nächstes anzugeben, dass die Daten aus einer Datenbank diese gezogen werden, wird ein neues SqlDataSource-Steuerelement erstellt. Benennen Sie das Steuerelement `ProductsWithCategoryInfoDataSource`.


![Erstellen Sie ein neues SqlDataSource-Steuerelement, das mit dem Namen ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Abbildung 12**: Erstellen Sie ein neues SqlDataSource-Steuerelement, das mit dem Namen `ProductsWithCategoryInfoDataSource`


Im nächste Bildschirm fordert uns um die Datenbank anzugeben. Wählen Sie wie in Abbildung 7 die `NORTHWINDConnectionString` aus der Dropdownliste aus, und klicken Sie auf Weiter. Der Bildschirm für die Select-Anweisung konfigurieren wählen Sie eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur Optionsfeld angeben, und klicken Sie auf Weiter. Dadurch wird den Bildschirm definieren benutzerdefinierte Anweisungen oder gespeicherte Prozeduren angezeigt, die Registerkarten, SELECT, UPDATE, INSERT und DELETE mit der Bezeichnung bietet. Auf jeder Registerkarte können Sie benutzerdefinierte SQL-Anweisung in das Textfeld eingeben, oder wählen Sie eine gespeicherte Prozedur aus der Dropdown Liste. In diesem Tutorial betrachten wir eine benutzerdefinierte SQL­Anweisung eingeben; Im nächste Tutorial enthält ein Beispiel, das eine gespeicherte Prozedur verwendet.


![Geben Sie eine benutzerdefinierte SQL-Anweisung, oder wählen Sie eine gespeicherte Prozedur](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Abbildung 13**: Geben Sie eine benutzerdefinierte SQL-Anweisung, oder wählen Sie eine gespeicherte Prozedur


Die benutzerdefinierte SQL-Anweisung manuell in das Textfeld eingegeben werden kann oder grafisch konstruiert werden kann, indem Sie auf die Schaltfläche "Abfrage-Generator". Über den Abfrage-Generator oder das Textfeld ein, verwenden Sie die folgende Abfrage zurückzugebenden der `ProductID` und `ProductName` Felder aus der `Products` Tabelle mithilfe von einer `JOIN` zum Abrufen des Produkts s `CategoryName` aus der `Categories` Tabelle:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]


![Sie können grafisch die Abfrage mithilfe des Abfrage-Generators erstellen](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Abbildung 14**: Sie grafisch zu erstellen können, dass die Abfrage mithilfe des Abfrage-Generators


Nach dem Angeben der Abfrage an, klicken Sie auf Weiter, um zum Testen von Abfragen Bildschirm zu gelangen. Klicken Sie auf "Fertig stellen", um den SqlDataSource-Steuerelement-Assistenten abzuschließen.

Nach Abschluss des Assistenten an, die GridView müssen drei BoundFields hinzugefügt, Anzeigen von der `ProductID`, `ProductName`, und `CategoryName` Spalten von der Abfrage zurückgegeben und führt die folgenden deklaratives Markup:


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]


[![Das GridView zeigt jedes Produkt s-ID, Name und die zugeordnete Kategoriename](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Abbildung 15**: s der GridView zeigt jede Produkt-ID, Name und Kategorienamen verknüpft ist ([klicken Sie, um das Bild in voller Größe anzeigen](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurde erläutert, wie Abfragen und Anzeigen von Daten, die mit dem SqlDataSource-Steuerelement. Wie dem ObjectDataSource-Steuerelement dient dem SqlDataSource-Steuerelement als Proxy und einen deklarativen Ansatz zum Zugreifen auf Daten bereitstellt. Die Eigenschaften angeben, die Datenbank für die Verbindung und der SQL- `SELECT` zum Ausführen von Abfragen; sie können über das Eigenschaftenfenster oder mit dem Konfigurieren von Datenquellen-Assistenten angegeben werden.

Die `SELECT` Abfragebeispiele in diesem Tutorial wir untersuchten alle Datensätze zurückgegeben, aus der angegebenen Abfrage. Das SqlDataSource-Steuerelement, kann dagegen enthalten eine `WHERE` -Klausel mit Parametern, deren Werte programmgesteuert zugeordnet sind oder automatisch aus einer angegebenen Quelle abgerufen werden. Untersucht das Erstellen und verwenden Sie parametrisierte Abfragen im nächsten Tutorial.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf Daten der relationalen Datenbank](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Übersicht über die SqlDataSource-Steuerelement](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Schnellstart-Tutorials: Dem SqlDataSource-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Die Datei "Web.config" `<connectionStrings>` Element](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Datenbank-Verbindung-Zeichenfolgenreferenz](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Susan Connery Bernadette Leigh und David Suru. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](using-parameterized-queries-with-the-sqldatasource-cs.md)
