---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Erstellen eine Datenzugriffsschicht (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm wir ganz von vorn starten und die Daten Zugriff Layer (DAL), mithilfe von typisierten DataSets Zugriff auf die Informationen in einer Datenbank zu erstellen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: 5cf1a430d6fe94174a877beb04b930409bdbf084
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-data-access-layer-vb"></a>Erstellen eine Datenzugriffsschicht (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) oder [PDF herunterladen](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> In diesem Lernprogramm wir ganz von vorn starten und die Daten Zugriff Layer (DAL), mithilfe von typisierten DataSets Zugriff auf die Informationen in einer Datenbank zu erstellen.


## <a name="introduction"></a>Einführung

Unser Leben ausgetauschten als Webentwickler arbeiten mit Daten. Wir erstellen Datenbanken zum Speichern der Daten und Code zum Abrufen und bearbeiten, und die Webseiten zu sammeln und zusammenfassen. Dies ist eine lange Reihe, die Techniken zum Implementieren von ASP.NET 2.0 diese allgemeine Muster zu untersuchen, werden im ersten Lernprogramm. Wir beginnen mit dem Erstellen einer [Softwarearchitektur](http://en.wikipedia.org/wiki/Software_architecture) zusammengesetzt von einer Daten Zugriff-Konzept (DAL) mit typisierter DataSets Geschäftslogikschicht (Business Logic Layer, BLL) erzwingt, dass benutzerdefinierte Geschäftsregeln und eine ASP.NET besteht Darstellungsschicht Codepages von Teilen Sie ein gebräuchliches Seitenlayout. Sobald diese Back-End-Grundlagen wechseln in die Berichterstattung, müssen angeordnet wurden, mit dem angezeigt wird, zusammenfassen Sie, sammeln Sie und überprüfen Sie die Daten aus einer Webanwendung. Diese Lernprogramme sind darauf ausgerichtet, um präzise sein, und geben Sie schrittweise Anweisungen mit Screenshots, Sie durch den Prozess visuell zu durchlaufen. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und enthält einen Download der den vollständigen Code verwendet. (Dieses ersten Lernprogramm ziemlich umfassend ist, aber der Rest in viel verständlichste Segmente dargestellt sind.)

Für diese Lernprogramme wir müssen verwenden Sie eine Microsoft SQL Server 2005 Express Edition Version der Northwind-Datenbank eingefügt werden, dem `App_Data` Verzeichnis. Zusätzlich zu der Datei die `App_Data` Ordner enthält auch die SQL-Skripts zum Erstellen der Datenbank, für den Fall, dass Sie eine andere Datenbank-Version verwenden möchten. Diese Skripts können außerdem werden [direkt von Microsoft heruntergeladen](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), wenn Sie lieber möchten. Wenn Sie eine andere SQL Server-Version der Northwind-Datenbank verwenden, müssen Sie beim Aktualisieren der `NORTHWNDConnectionString` festlegen, in der Anwendungsverzeichnis `Web.config` Datei. Die Webanwendung wurde mithilfe von Visual Studio 2005 Professional Edition als Datei systembasierten Websiteprojekt erstellt. Allerdings alle der Lernprogramme funktioniert ebenso mit die kostenlose Version von Visual Studio 2005 [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

In diesem Lernprogramm wir ganz von vorn starten und erstellen Sie die Data Access Layer (DAL), gefolgt von erstellen die [(Business Logic Layer, BLL)](creating-a-business-logic-layer-vb.md) in der zweiten Lernprogramm und die Arbeiten an [Seite Layout und die Navigation](master-pages-and-site-navigation-vb.md) im dritten. Die Lernprogramme, nachdem das dritte Arbeitsblatt auf Grundlage aufbauen wird gemäß der ersten drei ab. Wir haben viele abdecken, die in diesem ersten Lernprogramm, Einrichten von Visual Studio so auslösen und los geht!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Schritt 1: Erstellen ein Webprojekt und Herstellen einer Verbindung mit der Datenbank

Bevor wir unsere Data Access Layer (DAL) erstellen können, müssen wir zuerst eine Website erstellen, und setup die Datenbank. Starten Sie durch das Erstellen einer neuen Datei systembasierten ASP.NET-Website. Um dies zu erreichen, wechseln Sie zum Menü Datei, und wählen Sie die neue Website, die das Dialogfeld "neue Website" anzuzeigen. Wählen Sie die Vorlage ASP.NET-Website, legen Sie die Dropdown-Liste im Dateisystem, wählen Sie einen Ordner auf der Website platzieren und Festlegen der Sprache Visual Basic.


[![Erstellen Sie eine neue Website von Datei-basierten System](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Abbildung 1**: Erstellen einer Website New File System-Based ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image3.png))


Dies erstellt eine neue Website mit einem `Default.aspx` ASP.NET-Seite ein `App_Data` Ordner, und ein `Web.config` Datei.

Mit der Website erstellt wird als Nächstes einen Verweis auf die Datenbank im Server-Explorer von Visual Studio hinzufügen. Durch Hinzufügen einer Datenbank zu Server-Explorer können Sie Tabellen, gespeicherte Prozeduren, Sichten und für alle von innerhalb von Visual Studio hinzufügen. Sie können auch Tabellendaten anzeigen oder erstellen eigene Abfragen entweder manuell oder grafisch über den Abfrage-Generator. Darüber hinaus, wenn wir die typisierte DataSets für die DAL erstellen müssen zu Punkt Visual Studio und der Datenbank wir aus denen typisierten DataSets erstellt werden soll. Während es diese Verbindungsinformationen zu diesem Zeitpunkt bereitstellen kann, füllt Visual Studio automatisch eine Dropdown-Liste der Datenbanken im Server-Explorer bereits registriert.

Notwendigen Schritte zum Hinzufügen der Northwind-Datenbank auf dem Server-Explorer, hängen davon ab, ob Sie in die SQL Server 2005 Express Edition-Datenbank verwenden möchten die `App_Data` Ordner oder haben eine Microsoft SQL Server 2000 oder 2005-Datenbank-Server-Setup, die Sie verwenden möchten. Stattdessen.

## <a name="using-a-database-in-theappdatafolder"></a>Verwenden einer Datenbank in der`App_Data`Ordner

Wenn Sie verfügen nicht über eine SQL Server 2000 oder 2005-Datenbankserver für die Verbindung, oder Sie einfach zu vermeiden, dass die Datenbank mit einem Datenbankserver hinzufügen möchten, möchten, können Sie die SQL Server 2005 Express Edition-Version der Northwind-Datenbank verwenden, die in der heruntergeladenen Websit befindet e's `App_Data` Ordner (`NORTHWND.MDF`).

In eine Datenbank platziert die `App_Data` Ordner wird automatisch auf dem Server-Explorer hinzugefügt. Voraus, dass Sie SQL Server 2005 Express Edition auf Ihrem Computer installiert sind, sollten Sie einen Knoten mit dem Namen NORTHWND sehen. MDF-Datei im Server-Explorer, die Sie erweitern können, und untersuchen die Tabellen, Sichten, gespeicherte Prozedur usw. (siehe Abbildung 2).

Die `App_Data` Ordner kann auch Microsoft Access halten `.mdb` -Dateien, die, wie ihre äquivalente SQL-Server im Server-Explorer automatisch hinzugefügt werden. Wenn Sie die SQL Server-Optionen verwenden möchten, können Sie immer [herunterladen eine Microsoft Access-Version der Northwind-Datenbankdatei](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) und legen Sie die `App_Data` Verzeichnis. Bedenken Sie, allerdings, die als Access-Datenbanken sind nicht Funktionsumfang wie SQL Server, und werden nicht im Website-Szenarien verwendet werden soll. Darüber hinaus wird eine Reihe von 35 +-Lernprogramme bestimmte Funktionen auf Datenbankebene nutzen, die von Access unterstützt werden.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Herstellen einer Verbindung mit der Datenbank in einer Microsoft SQL Server 2000 oder 2005-Datenbankserver

Alternativ können Sie mit einer Northwind-Datenbank auf einem Datenbankserver installiert verbinden. Wenn der Datenbankserver nicht bereits die Northwind-Datenbank installiert haben, Sie zuerst müssen fügen Sie ihn zum Datenbankserver durch Ausführen des Skripts Installation enthalten, in diesem Lernprogramm herunterladen oder von [Herunterladen der SQL Server 2000-Version der Northwind-Datenbank und Installationsskript](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direkt aus der Microsoft-Website.

Nachdem Sie die Datenbank installiert haben, wechseln Sie auf dem Server-Explorer in Visual Studio mit der Maustaste, klicken Sie auf der Datenverbindungen-Knoten, und wählen Verbindung hinzufügen. Wenn Sie den Server-Explorer, wechseln zur Ansicht sehen / Server-Explorer oder Strg + Alt + S klicken. Hierdurch erscheint das Dialogfeld "Verbindung hinzufügen", dort Sie den Server für die Verbindung angeben können, die Informationen zur Benutzerauthentifizierung und den Datenbanknamen. Sobald Sie erfolgreich die Verbindungsinformationen für die Datenbank konfiguriert und auf die Schaltfläche "OK" geklickt haben, wird die Datenbank als Knoten unterhalb des Knotens Datenverbindungen hinzugefügt werden. Sie können den Datenbankknoten aus, um die Tabellen, Sichten, gespeicherte Prozeduren usw. untersuchen erweitern.


![Fügen Sie eine Verbindung mit dem Datenbankserver Northwind-Datenbank](creating-a-data-access-layer-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie eine Verbindung mit dem Datenbankserver Northwind-Datenbank


## <a name="step-2-creating-the-data-access-layer"></a>Schritt 2: Erstellen der Datenzugriffsebene

Beim Arbeiten mit ist eine Option für die Daten die Logik datenbezogene direkt in der Darstellungsschicht (in einer Web-Anwendung, die ASP.NET Seiten zusammensetzt Darstellungsschicht) einbetten. Dies kann der ADO.NET-Code im Abschnitt mit der ASP.NET-Seite Code schreiben oder mithilfe der SqlDataSource-Steuerelement aus dem Markup Teil handeln. In beiden Fällen koppelt dieser Ansatz eng an die Datenzugriffslogik Darstellungsschicht. Die empfohlene Vorgehensweise ist allerdings die Datenzugriffslogik von der Darstellungsschicht getrennt. Diese separaten Schicht wird als Data Access Layer, DAL kurz, bezeichnet und wird in der Regel als ein separates Klassenbibliotheksprojekt implementiert. Die Vorteile dieser Ebenen Architektur sind dokumentiert (siehe Abschnitt "Weitere Messwerte" am Ende dieses Lernprogramms Informationen über diese Vorteile) und ist der Ansatz, wir in dieser Serie nehmen.

Der gesamte Code, der speziell für die zugrunde liegende Datenquelle z. B. das Erstellen einer Verbindungs mit der Datenbank ist ausstellen `SELECT`, `INSERT`, `UPDATE`, und `DELETE` Befehle usw., sollten in der DAL befinden. Die Darstellungsschicht dürfen nicht für alle Verweise auf solche Datenzugriffscode, aber Sie sollten stattdessen Aufrufe an die DAL für alle datenanforderungen durchführen. Datenzugriffsebene enthalten i. d. r. Methoden für den Zugriff auf Daten in der zugrunde liegenden Datenbank. Die Northwind-Datenbank hat, z. B. `Products` und `Categories` Tabellen, mit denen die Produkte für den Verkauf und die Kategorien, zu denen sie gehören. In unserem DAL haben wir Methoden wie z. B.:

- `GetCategories(),` die gibt Informationen über alle Kategorien zurück.
- `GetProducts()`, die Informationen für alle Produkte zurück
- `GetProductsByCategoryID(categoryID)`, das alle Produkte, die eine angegebene Kategorie angehören, zurück
- `GetProductByProductID(productID)`, die Informationen zu einem bestimmten Produkt zurück

Beim Aufrufen dieser Methoden werden eine Verbindung mit der Datenbank herstellen, geben Sie die entsprechende Abfrage und Zurückgeben der Ergebnisse. Es ist wichtig, wie wir diese Ergebnisse zurückgeben. Diese Methoden einfach ein DataSet oder DataReader ausgefüllt, indem der Datenbankabfrage zurückgeben, aber idealerweise diese Ergebnisse zurückgegeben werden soll mit *stark typisierten Objekten*. Ein stark typisiertes Objekt ist, deren Schema starr zum Zeitpunkt der Kompilierung definiert ist, während das Gegenteil, ein Objekt lose typisierten eines ist, deren Schema nicht bis zur Laufzeit bekannt ist.

Beispielsweise sind das DataReader-Objekt und das DataSet (standardmäßig) lose typisierte Objekte, da ihr Schema nach den Spalten, die von der Datenbankabfrage verwendet, um sie aufzufüllen zurückgegebenen definiert ist. Wir benötigen, mithilfe von Syntax wie eine bestimmte Spalte aus einer "DataTable lose typisierte" zugreifen: `DataTable.Rows(index)("columnName")`. Die DataTable lose Typisierung, in diesem Beispiel wird durch die Tatsache ausgestellt, die wir den Namen der Spalte mit einer Zeichenfolge oder Ordinalindex zugreifen müssen. Eine stark typisierte DataTable andererseits, müssen alle zugehörigen Spalten, die als Eigenschaften implementiert resultierenden in Code, der aussieht: `DataTable.Rows(index).columnName`.

Um stark typisierte Objekte zurückzugeben, können Entwickler ihre eigenen benutzerdefinierten Geschäftsobjekte erstellen oder typisierter DataSets verwenden. Ein Geschäftsobjekt wird vom Entwickler implementiert, wie eine Klasse, deren Eigenschaften in der Regel die Spalten der zugrunde liegenden Datenbanktabelle das Geschäftsobjekt widerspiegeln, darstellt. Ein typisiertes DataSet ist eine Klasse, die für Sie von Visual Studio generiert auf Grundlage eines Datenbankschemas und, deren Mitglieder gemäß diesem Schema stark typisiert sind. Das typisierte DataSet selbst besteht aus Klassen, die ADO.NET DataSet und DataTable DataRow Klassen erweitern. Zusätzlich zu den stark typisierten DataTables umfassen typisierter DataSets jetzt auch TableAdapters, die Klassen mit Methoden zum Auffüllen Datasetss Datentabellen und das Weitergeben von Änderungen in die DataTables wieder in der Datenbank sind.

> [!NOTE]
> Weitere Informationen zu den vor- und Nachteile der Verwendung typisierter DataSets im Vergleich zu benutzerdefinierten Geschäftsobjekte, finden Sie unter [Datenebenenkomponenten entwerfen und die Übergabe über Datenebenen](https://msdn.microsoft.com/library/ms978496.aspx).


Wir werden stark typisierte DataSets für diese Lernprogramme-Architektur verwenden. Abbildung 3 zeigt die Arbeitsabläufe zwischen den verschiedenen Ebenen einer Anwendung, die typisierte DataSets verwendet.


[![Alle Datenzugriffscode ist DAL Standardklasse zugeordnet.](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Abbildung 3**: alle Datenzugriffscode ist bisher DAL ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Erstellen eines typisierten Datasets und Tabellenadapters

Um zu beginnen, erstellen unsere DAL, zunächst unsere Projekt ein typisiertes DataSet hinzugefügt werden. Um dies zu erreichen, mit der rechten Maustaste auf den Projektknoten im Projektmappen-Explorer, und wählen Sie ein neues Element hinzufügen. Wählen Sie aus der Liste der Vorlagen die Option für das DataSet, und nennen Sie sie `Northwind.xsd`.


[![Wählen Sie das Projekt ein neues DataSet hinzu](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Abbildung 4**: Wählen Sie Ihr Projekt ein neues DataSet hinzu ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image10.png))


Nach dem Klicken auf Hinzufügen, bei der Aufforderung zum DataSet zum Hinzufügen der `App_Code` Ordner, wählen Sie "Ja". Der Designer für das typisierte DataSet wird dann angezeigt werden, und der TableAdapter-Konfigurations-Assistent wird gestartet, sodass Sie Ihre erste TableAdapter mit dem typisierten DataSet hinzufügen.

Ein typisiertes DataSet dient als eine stark typisierte Auflistung von Daten; Es besteht von stark typisierten DataTable-Instanzen, von denen jeder wiederum von stark typisierten DataRow-Instanzen besteht. Es wird eine stark typisierte DataTable für jede der zugrunde liegenden Datenbanktabellen erstellt, die wir zur Bearbeitung dieser Reihe Lernprogramme benötigen. Beginnen wir mit dem Erstellen einer "DataTable" für die `Products` Tabelle.

Bedenken Sie, dass stark typisierte DataTables keine Informationen zum Zugriff auf Daten aus ihren zugrunde liegenden Datenbanktabelle enthalten. Um die Daten zum Auffüllen der Datentabelle abzurufen, verwenden wir eine TableAdapter-Klasse, die als unsere Datenzugriffsebene fungiert. Für unsere `Products` DataTable, in der TableAdapter enthält die Methoden `GetProducts()`, `GetProductByCategoryID(categoryID)`usw., die wir von der Darstellungsschicht aufrufen müssen. Die DataTable-Rolle besteht darin, dienen als stark typisierte Objekte verwendet, um Daten zwischen den Ebenen zu übergeben.

Der TableAdapter-Konfigurations-Assistent beginnt, indem Sie dazu aufgefordert, die Datenbank zur Bearbeitung auswählen. Die Dropdownliste zeigt diese Datenbanken im Server-Explorer. Wenn Sie die Northwind-Datenbank nicht im Server-Explorer hinzugefügt haben, können Sie die Schaltfläche neue Verbindung zu diesem Zeitpunkt zu diesem Zweck klicken.


[![Wählen Sie die Northwind-Datenbank aus der Dropdown-Liste](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Abbildung 5**: Wählen Sie die Northwind-Datenbank aus der Dropdown-Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image13.png))


Nach dem Auswählen der Datenbank, und klicken Sie auf Weiter, Sie werden gefragt, ob Sie in die Verbindungszeichenfolge speichern möchten die `Web.config` Datei. Durch das Speichern der Verbindungszeichenfolge müssen Sie vermeiden, dass es schwer codierten in den TableAdapter-Klassen Dinge zu vereinfachen, wenn sich die Verbindungszeichenfolgeninformationen in der Zukunft ändern. Wenn Sie sich entscheiden, um die Verbindungszeichenfolge in der Konfigurationsdatei speichern gespeichert ist der `<connectionStrings>` Bereich, kann [optional verschlüsselte](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) für Sicherheit verbesserte oder später über die neue ASP.NET 2.0-Eigenschaftenseite in geändert die IIS-GUI Administratortool, die für Administratoren besser geeignet ist.


[![Speichern Sie die Verbindungszeichenfolge in "Web.config"](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Abbildung 6**: Speichern der Verbindungszeichenfolge für `Web.config` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image16.png))


Als Nächstes müssen wir das Schema für die erste stark typisierte DataTable definiert, und geben Sie die erste Methode für unsere TableAdapter zu verwenden, wenn das stark typisierte DataSet zu füllen. Diese beiden Schritte aktivitätsdesignerattribute gleichzeitig durch Erstellen einer Abfrage, die den Spalten aus der Tabelle zurückgibt, dass wir in unserer "DataTable" wiedergegeben werden soll. Am Ende des Assistenten erhalten wir einen Methodennamen an dieser Abfrage. Sobald, erfüllt ist, kann diese Methode aus unserer Darstellungsschicht aufgerufen werden. Die Methode wird die definierte Abfrage auszuführen und Auffüllen eine stark typisierten "DataTable".

Zunächst definieren die SQL-Abfrage müssen wir zuerst angeben, wie der TableAdapter in der Abfrage ausgeben soll. Wir können eine Ad-hoc-SQL-Anweisung verwenden, Erstellen einer neue gespeicherten Prozedur oder eine vorhandene gespeicherte Prozedur verwenden. Für diese Lernprogramme verwenden wir Ad-hoc-SQL-Anweisungen. Finden Sie unter [Brian Noyes](http://briannoyes.net/)der Artikel, [erstellen Sie eine Datenzugriffsschicht mit DataSet-Designer von Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) ein Beispiel der Verwendung gespeicherter Prozeduren.


[![Fragen Sie die Daten mithilfe einer Ad-hoc-SQL-Anweisung](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Abbildung 7**: Abfragen der Daten, die mit Ad-hoc-SQL-Anweisungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image19.png))


An diesem Punkt können wir in der SQL-Abfrage manuell eingeben. Beim Erstellen der ersten Methode im TableAdapter benötigen Sie in der Regel die Abfrage Spalten zurückgeben, die in die entsprechende "DataTable" ausgedrückt werden. Wir erreichen dies durch Erstellen einer Abfrage, die alle Spalten und alle Zeilen von gibt die `Products` Tabelle:


[![Geben Sie die SQL-Abfrage in das Textfeld](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Abbildung 8**: Geben Sie die SQL-Abfrage in das Textfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image22.png))


Alternativ können Sie mithilfe des Abfrage-Generators und die Abfrage grafisch zu erstellen, wie in Abbildung 9 gezeigt.


[![Erstellen Sie die Abfrage grafisch dar, über den Abfrage-Editor](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Abbildung 9**: Erstellen der Abfrage grafisch dar, über den Abfrage-Editor ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image25.png))


Nach dem Erstellen der Abfrage, jedoch vor dem Verschieben auf dem nächsten Bildschirm klicken Sie auf die Schaltfläche "Erweiterte Optionen". In Websiteprojekten ist "generieren INSERT-, Update- und Delete-Anweisungen" die einzige erweiterte Optionen standardmäßig aktiviert; Wenn Sie diesen Assistenten, aus einer Klassenbibliothek oder ein Windows-Projekt ausführen wird auch die Option "Vollständige Parallelität verwenden" ausgewählt werden. Lassen Sie die Option "Vollständige Parallelität verwenden" jetzt deaktiviert. In zukünftigen Lernprogrammen untersuchen wir die vollständigen Parallelität.


[![Wählen Sie nur die generieren Insert, Update und Delete-Anweisungen Option](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Abbildung 10**: Wählen Sie nur die generieren Insert, Update und Delete-Anweisungen Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image28.png))


Klicken Sie nach der Überprüfung der erweiterten Optionen an auf Weiter, um zur letzten Bildschirm zu gelangen. Hier werden wir gefragt, welche Methoden zum Hinzufügen von dem TableAdapter auswählen. Es gibt zwei Muster für das Auffüllen von Daten:

- **DataTable füllen** bei diesem Ansatz wird eine Methode erstellt, die in einer "DataTable" als Parameter akzeptiert und füllt sie basierend auf den Ergebnissen der Abfrage. Das ADO.NET DataAdapter-Klasse, z. B. implementiert dieses Muster mit seiner `Fill()` Methode.
- **Zurückgeben eine "DataTable"** bei diesem Ansatz die Methode erstellt und füllt die DataTable für Sie, und gibt zurück, wie die Methoden Wert zurückgeben.

Sie können den TableAdapter eine oder beide dieser Muster zu implementieren. Sie können die hier bereitgestellten Methoden auch umbenennen. Lassen Sie beide Kontrollkästchen aktiviert, sehen wir, obwohl es nur das zweite Schema in der gesamten diese Lernprogramme verwenden. Darüber hinaus wir benennen die vielmehr generische `GetData` aufzurufende Methode `GetProducts`.

Wenn dieses Kontrollkästchen aktiviert, erstellt das letzte Kontrollkästchen "GenerateDBDirectMethods," `Insert()`, `Update()`, und `Delete()` Methoden für den TableAdapter. Wenn Sie diese Option deaktiviert lassen, müssen alle Updates über den TableAdapter alleinige `Update()` Methode, die in das typisierte DataSet, einer "DataTable", eine einzelne DataRow oder ein Array von DataRows akzeptiert. (Wenn Sie haben deaktiviert die "generieren INSERT-, Update- und Delete-Anweisungen"-Option von den erweiterten Eigenschaften in Abbildung 9 dieses Kontrollkästchen Einstellung hat keine Auswirkungen.) Lassen Sie uns dieses Kontrollkästchen aktiviert ist.


[![Ändern Sie den Methodennamen aus GetData in GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Abbildung 11**: Ändern Sie den Namen der Methode aus `GetData` auf `GetProducts` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image31.png))


Schließen Sie den Assistenten, indem Sie auf ' Fertig stellen '. Wenn der Assistent geschlossen werden wir in den DataSet-Designer zurückgegeben, die DataTable zeigt, dass wir gerade erstellt haben. Sehen Sie die Liste der Spalten in der `Products` DataTable (`ProductID`, `ProductName`usw.), sowie die Methoden von der `ProductsTableAdapter` (`Fill()` und `GetProducts()`).


[![Die Produkte DataTable und ProductsTableAdapter wurden mit dem typisierten DataSet hinzugefügt](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Abbildung 12**: die `Products` DataTable und `ProductsTableAdapter` wurden hinzugefügt, die typisierte DataSet ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image34.png))


An diesem Punkt haben wir eine typisierte DataSet mit einer einzelnen "DataTable" (`Northwind.Products`) und eine stark typisierte DataAdapter-Klasse (`NorthwindTableAdapters.ProductsTableAdapter`) mit einer `GetProducts()` Methode. Diese Objekte können verwendet werden, eine Liste aller Produkte aus Code wie den Zugriff auf:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Dieser Code war uns ein Bit Data Access-spezifischen Code schreiben nicht erforderlich. Wir verfügte nicht über alle ADO.NET-Klassen instanziieren, wir haben nicht zum Verweisen auf alle Verbindungszeichenfolgen, die SQL-Abfragen oder gespeicherte Prozeduren. Stattdessen stellt der TableAdapter auf niedriger Ebene Datenzugriffscode für uns.

Jedes Objekt, das in diesem Beispiel verwendete ist auch eine stark typisierte, sodass Visual Studio IntelliSense und die typüberprüfung zur Kompilierzeit angeben. Und am besten geeignet für alle Datentabellen, die dem TableAdapter zurückgegebenes für ASP.NET Data Web Controls, z. B. die GridView, DetailsView DropDownList, CheckBoxList und einigen weiteren Bildschirmen gebunden werden kann. Das folgende Beispiel veranschaulicht das Binden von zurückgegebene DataTable der `GetProducts()` Methode, um eine GridView in nur einer mangelnder drei Codezeilen innerhalb der `Page_Load` -Ereignishandler.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]


[![Die Liste der Produkte wird in einem GridView angezeigt.](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Abbildung 13**: die Liste der Produkte in einer GridView angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image37.png))


Obwohl in diesem Beispiel erforderlich, dass wir in unserer ASP.NET-Seite drei Codezeilen schreiben `Page_Load` Ereignishandler, in zukünftigen Lernprogrammen untersucht, wie Sie das ObjectDataSource deklarativ Abrufen der Daten von der DAL. Mit der ObjectDataSource wir müssen keinen Code schreiben und erhalten Unterstützung paging und sortieren!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Schritt 3: Hinzufügen von Methoden, um die Datenzugriffsebene parametrisiert

An diesem Punkt unsere `ProductsTableAdapter` -Klasse, die jedoch eine Methode verfügt über `GetProducts()`, womit alle Produkte in der Datenbank zurückgegeben. Wird mit allen Produkten arbeiten definitiv ist, zwar hilfreich gibt es Situationen wir müssen Abrufen von Informationen zu einem bestimmten Produkt oder alle Produkte, die einer bestimmten Kategorie angehören. Um solche Funktionen unserer Datenzugriffsebene hinzuzufügen können wir dem TableAdapter parametrisierte Methoden hinzufügen.

Fügen Sie die `GetProductsByCategoryID(categoryID)` Methode. Hinzufügen eine neue Methode zu DAL, zurück zu DataSet-Designer mit der Maustaste in den `ProductsTableAdapter` Abschnitt, und wählen Sie die Abfrage hinzufügen.


![Mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen](creating-a-data-access-layer-vb/_static/image38.png)

**Abbildung 14**: mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen


Wir gefragt werden zuerst, ob wir Zugriff auf die Datenbank mit einer Ad-hoc-SQL-Anweisung oder einer neuen oder vorhandenen gespeicherten Prozedur möchten. Wir möchten, verwenden Sie eine Ad-hoc-SQL-Anweisung erneut aus. Als Nächstes werden wir gefragt, welche Art von SQL-Abfrage wir verwenden möchten. Da alle Produkte zurückgegeben, die eine angegebene Kategorie angehören, soll, möchten wir Schreiben einer `SELECT` Anweisung die Zeilen zurückgibt.


[![Wählen Sie eine SELECT-Anweisung erstellen, die Zeilen zurückgibt](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Abbildung 15**: Wählen Sie zum Erstellen einer `SELECT` -Anweisung die Zeilen zurück ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image41.png))


Der nächste Schritt ist die SQL-Abfrage verwendet, um den Datenzugriff zu definieren. Da wir möchten nur solche Produkte zurück, die einer bestimmten Kategorie angehören, ich verwenden die gleiche `SELECT` -Anweisung vom `GetProducts()`, aber fügen Sie die folgenden `WHERE` -Klausel: `WHERE CategoryID = @CategoryID`. Die `@CategoryID` Parameter gibt an, die dem TableAdapter-Assistenten, dass die Methode, die wir erstellen, bei einen Eingabeparameter mit dem entsprechenden Typ (d. h., eine NULL-Werte zulässt Ganzzahl).


[![Geben Sie eine Abfrage aus, um nur die Produkte in einer angegebenen Kategorie zurückgeben](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Abbildung 16**: Geben Sie eine Abfrage nur Zurückgeben von Produkten in eine angegebene Kategorie ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image44.png))


Im letzten Schritt, die wir ausgewählt, können die Daten, Muster zugreifen zu verwenden, sowie die Namen der generierten Methoden anpassen. Nach dem Muster Füllung ändern wir den Namen in `FillByCategoryID` und für die Rückgabe eine "DataTable" Muster zurück (die `GetX` Methoden), nutzen wir `GetProductsByCategoryID`.


[![Wählen Sie den Namen für die TableAdapter-Methoden](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Abbildung 17**: Wählen Sie den Namen für die TableAdapter-Methoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image47.png))


Nach Abschluss des Assistenten enthält die DataSet-Designer die neuen TableAdapter-Methoden.


![Die Produkte können jetzt werden nach Kategorie abgefragt](creating-a-data-access-layer-vb/_static/image48.png)

**Abbildung 18**: die Produkte können jetzt nach Kategorie abgefragt werden


Hinzuzufügende erkundet eine `GetProductByProductID(productID)` Methode, die das gleiche Verfahren verwenden.

Diese parametrisierten Abfragen können direkt aus dem DataSet-Designer getestet werden. Mit der rechten Maustaste auf die Methode im TableAdapter, und wählen Sie Vorschaudaten. Geben Sie anschließend die Werte für die Parameter, und klicken Sie auf Vorschau.


[![Diese Produkte gehören, der Kategorie "Getränke" werden angezeigt.](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Abbildung 19**: die Produkte zu gehörenden der Kategorie "Getränke" werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image51.png))


Mit der `GetProductsByCategoryID(categoryID)` -Methode in unserer DAL, wir können jetzt eine ASP.NET-Seite auf, die nur solche Produkte in einer angegebenen Kategorie anzeigt erstellen. Das folgende Beispiel zeigt alle Produkte, die in der Kategorie "Getränke" sind mit einem `CategoryID` 1.

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]


[![Diese Produkte in der Kategorie "Getränke" werden angezeigt.](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Abbildung 20**: solche Produkte in der Kategorie "Getränke" angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Schritt 4: Einfügen, aktualisieren und Löschen von Daten

Es gibt zwei Muster, die im Allgemeinen zum Einfügen, aktualisieren und Löschen von Daten. Das erste Muster für die ich das Datenbank-direct-Muster heraus aufgerufen wird, umfasst das Erstellen von Methoden, die beim Aufrufen, Problem ein `INSERT`, `UPDATE`, oder `DELETE` -Befehls auf die Datenbank, die für eine einzelne Datenbank-Datensatzes ausgeführt wird. Solche Methoden werden in der Regel in einer Reihe von skalaren Werten (ganze Zahlen, Zeichenfolgen, boolesche Werte, datetimes-Werte usw.), die entsprechen den Werten einfügen, aktualisieren oder löschen übergeben. Z. B. mit diesem Muster für die `Products` Tabelle würde die Delete-Methode in einen ganzzahligen Parameter an, der angibt, die `ProductID` des Datensatzes zu löschen, während die Insert-Methode in einer Zeichenfolge zum dauern würde, die `ProductName`, einen Dezimalwert für die `UnitPrice`, eine ganze Zahl für die `UnitsOnStock`und so weiter.


[![Jede INSERT-, Update- und anfordern löschen wird an die Datenbank sofort gesendet.](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Abbildung 21**: jede einfügen, aktualisieren und löschen anfordern wird an die Datenbank sofort gesendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image57.png))


Andere Muster, die ich verweisen müssen Muster als Batch zu aktualisieren, ist eine gesamte DataSet, DataTable oder Auflistung von DataRows in einem Methodenaufruf zu aktualisieren. Mit diesem Muster ein Entwickler löscht, einfügt, und ändert die DataRows in einer "DataTable" und übergibt dann diese DataRows oder eine Datentabelle in einer Update-Methode. Klicken Sie dann diese Methode zählt die DataRows übergeben, legt fest, ob geändert, hinzugefügt, gelöscht oder wurden haben (über die DataRow [RowState-Eigenschaft](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) Wert), und gibt die entsprechende Datenbank-Anforderung für jeden Datensatz.


[![Alle Änderungen werden mit der Datenbank synchronisiert, wenn die Updatemethode aufgerufen wird](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Abbildung 22**: Alle Änderungen werden mit der Datenbank synchronisiert, wenn die Updatemethode aufgerufen wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image60.png))


Der TableAdapter updatemusters Batch wird standardmäßig verwendet, unterstützt aber auch das direkte DB-Muster. Da wir die Option "generieren INSERT-, Update- und Delete-Anweisungen aus den erweiterten Eigenschaften ausgewählt, wenn unsere TableAdapter erstellen die `ProductsTableAdapter` enthält eine `Update()` -Methode, die dem Batch-Update-Muster implementiert. Insbesondere der TableAdapter enthält eine `Update()` Methode, die das typisierte DataSet, einer stark typisierten "DataTable" oder eine oder mehrere DataRows übergeben werden kann. Wenn Sie nach links "GenerateDBDirectMethods" aktiviert beim ersten Erstellung des TableAdapter das direkte DB-Muster auch über implementiert werden `Insert()`, `Update()`, und `Delete()` Methoden.

Beide Änderung Datenmuster Verwenden des TableAdapter `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften zum Ausstellen von ihren `INSERT`, `UPDATE`, und `DELETE` Befehle in der Datenbank. Können Sie überprüfen und Ändern der `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften, die über durch Klicken auf den TableAdapter im DataSet-Designer, und klicken Sie dann das Eigenschaftenfenster möchten. (Stellen Sie sicher, Sie haben ausgewählt, die TableAdapter, und dass die `ProductsTableAdapter` Objekt ist dasjenige, das in der Dropdown-Liste im Eigenschaftenfenster ausgewählt.)


[![Der TableAdapter weist InsertCommand, UpdateCommand und DeleteCommand-Eigenschaften](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Abbildung 23**: hat die TableAdapter `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaften ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image63.png))


Um zu überprüfen oder ändern Sie diesen Befehl Datenbankeigenschaften, klicken Sie auf die `CommandText` untergeordnete Eigenschaft, die Abfrage-Generator angezeigt wird.


[![Konfigurieren Sie die INSERT-, Update- und DELETE-Anweisungen im Abfrage-Generator](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Abbildung 24**: Konfigurieren der `INSERT`, `UPDATE`, und `DELETE` Anweisungen im Abfrage-Generator ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image66.png))


Im folgenden Codebeispiel wird veranschaulicht, mit der der Batch-updatemuster Doppelklicken den Preis aller Produkte, die nicht eingestellt werden, und deren 25 Einheiten im Lager oder weniger:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

Der folgende Code veranschaulicht, wie die DB-direct-Muster verwenden, um programmgesteuert löschen Sie ein bestimmtes Produkt, und aktualisieren Sie eine, und klicken Sie dann ein neues Konto hinzufügen:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Erstellen von benutzerdefinierten INSERT-, Update- und Delete-Methode

Die `Insert()`, `Update()`, und `Delete()` Methoden, die mit der direkten DB-Methode erstellte können etwas aufwändig sein, insbesondere bei Tabellen mit vielen Spalten. Im vorherigen Codebeispiel ansehen, ohne IntelliSenses unterstützen, es ist nicht besonders klar, welche `Products` Tabellenspalte ordnet jeden Eingabeparameter für das `Update()` und `Insert()` Methoden. Möglicherweise Situationen wird nur eine einzelne Spalte oder zwei aktualisieren oder möchten eine benutzerdefinierte `Insert()` -Methode, die ggf. wird Rückgabe des Werts der des neu eingefügten Datensatzes `IDENTITY` (automatisch inkrementierten)-Feld.

Um eine solche benutzerdefinierte Methode erstellen, geben Sie an der DataSet-Designer zurück. Mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen, an dem TableAdapter-Assistenten zurückgegeben. Auf dem zweiten Bildschirm kann der Typ der Abfrage erstellt angezeigt werden. Erstellen wir eine Methode, fügt ein neues Produkt hinzu und gibt anschließend den Wert der neu hinzugefügten Datensatzabschnitt `ProductID`. Aus diesem Grund erstellen Sie wahlweise eine `INSERT` Abfrage.


[![Erstellen Sie eine Methode, um die Products-Tabelle eine neue Zeile hinzuzufügen](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Abbildung 25**: Erstellen Sie eine Methode zum Hinzufügen einer Zeile neu, um die `Products` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image69.png))


Auf dem nächsten Bildschirm die `InsertCommand`des `CommandText` angezeigt wird. Erweitern Sie diese Abfrage durch Hinzufügen von `SELECT SCOPE_IDENTITY()` am Ende der Abfrage, die den letzten Identitätswert eingefügt zurückgegeben wird ein `IDENTITY` Spalte im selben Bereich. (Finden Sie unter der [technische Dokumentation](https://msdn.microsoft.com/library/ms190315.aspx) Weitere Informationen zu `SCOPE_IDENTITY()` und aus welchem Grund Sie wahrscheinlich möchten [Bereich\_IDENTITY() statt @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Stellen Sie sicher, dass Sie am Ende der `INSERT` -Anweisung mit einem Semikolon vor dem Hinzufügen der `SELECT` Anweisung.


[![Erweitern Sie die Abfrage aus, um den SCOPE_IDENTITY() Wert zurückzugeben](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Abbildung 26**: Erweitern Sie die Abfrage zum Zurückgeben der `SCOPE_IDENTITY()` Wert ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image72.png))


Benennen Sie abschließend die neue Methode `InsertProduct`.


[![Legen Sie den neuen Methodennamen auf InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Abbildung 27**: Legen Sie den neuen Namen der Methode auf `InsertProduct` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image75.png))


Wenn Sie zurück in den DataSet-Designer wird angezeigt, die `ProductsTableAdapter` enthält eine neue Methode `InsertProduct`. Wenn dieser neuen Methode einen Parameter für jede Spalte besitzt die `Products` Tabelle wahrscheinlich werden Sie haben vergessen, beenden die `INSERT` -Anweisung mit einem Semikolon. Konfigurieren der `InsertProduct` Methode und sicherzustellen, dass Sie über ein Semikolon begrenzt die `INSERT` und `SELECT` Anweisungen.

Standardmäßig fügen Sie Methoden Problem nicht-Abfragemethoden, was bedeutet, dass sie die Anzahl der betroffenen Zeilen zurück. Wir möchten jedoch die `InsertProduct` Methode, um den Rückgabewert von der Abfrage nicht die Anzahl der betroffenen Zeilen zurückzugeben. Um dies zu erreichen, passen Sie die `InsertProduct` Methode `ExecuteMode` Eigenschaft, um `Scalar`.


[![Ändern Sie die ExecuteMode-Eigenschaft an skalaren](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Abbildung 28**: Ändern der `ExecuteMode` Eigenschaft `Scalar` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image78.png))


Der folgende code veranschaulicht diese neue `InsertProduct` Methode in Aktion:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Schritt 5: Abschließen der Datenzugriffsebene

Beachten Sie, dass die `ProductsTableAdapters` zurück die `CategoryID` und `SupplierID` Werte aus der `Products` , aber es ist nicht enthalten die `CategoryName` Spalte aus der `Categories` Tabelle oder die `CompanyName` Spalte aus der `Suppliers` Tabelle, obwohl es sich wahrscheinlich die Spalten handelt, die wir beim Anzeigen von Produktinformationen anzeigen möchten. Wir können ursprüngliche Methode des TableAdapter, ergänzen `GetProducts()`, zu beiden enthalten die `CategoryName` und `CompanyName` Spaltenwerte, die stark typisierte DataTable Einbeziehung auch diese neue Spalten aktualisiert werden.

Dies kann ein Problem darstellen, jedoch als dem TableAdapter-Methoden zum Einfügen, aktualisieren, und Löschen von Daten aus dieser Methode für erste basieren. Glücklicherweise, die automatisch generierte Methoden für das Einfügen, aktualisieren und Löschen von nicht sind betroffen Unterabfragen in den `SELECT` Klausel. Achten Sie darauf, um unsere Abfragen, hinzufügen, indem Sie `Categories` und `Suppliers` als Unterabfragen, anstatt `JOIN` s, wir müssen zu vermeiden, dass diese Methoden zum Ändern von Daten zu überarbeiten. Mit der rechten Maustaste auf die `GetProducts()` Methode in der `ProductsTableAdapter` und Auswählen von konfigurieren. Passen Sie dann die `SELECT` Klausel, damit sie aussieht wie:

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]


[![Aktualisieren Sie die SELECT-Anweisung für die GetProducts()-Methode](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Abbildung 29**: Aktualisieren der `SELECT` -Anweisung für die `GetProducts()` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image81.png))


Nach der Aktualisierung der `GetProducts()` Methode, um mithilfe dieser neuen Abfrage DataTable enthält zwei neue Spalten: `CategoryName` und `SupplierName`.


![DataTable Produkte weist zwei neue Spalten](creating-a-data-access-layer-vb/_static/image82.png)

**Abbildung 30**: die `Products` DataTable verfügt über zwei neue Spalten


Erkundet zum Aktualisieren der `SELECT` -Klausel in der `GetProductsByCategoryID(categoryID)` Methode ebenfalls.

Wenn Sie aktualisieren die `GetProducts()` `SELECT` mit `JOIN` Syntax der DataSet-Designer kann nicht Automatisches Generieren von Methoden zum Einfügen, aktualisieren und Löschen von Datenbankdaten, die mit dem DB-direkte Muster. Stattdessen müssen manuell viel Erstellungsvorgang wie mit der `InsertProduct` Methode weiter oben in diesem Lernprogramm. Darüber hinaus manuell haben, geben Sie die `InsertCommand`, `UpdateCommand`, und `DeleteCommand` Eigenschaftswerte, wenn Sie den Batch aktualisieren Muster verwenden möchten.

## <a name="adding-the-remaining-tableadapters"></a>Die verbleibenden TableAdapters hinzufügen

Bis jetzt wurde nur mit einem einzelnen TableAdapter für eine einzelne Datenbanktabelle arbeiten erläutert. Die Northwind-Datenbank enthält jedoch mehrere verwandte Tabellen, die wir zur Bearbeitung in der vorliegenden Webanwendung müssen. Ein typisiertes DataSet mehrere darf im Zusammenhang mit Datentabellen. Unsere DAL Fertigstellung müssen wir daher DataTables für die anderen Tabellen hinzufügen, die wir in diesen Lernprogrammen verwenden. Um einen neuen TableAdapter ein typisiertes DataSet hinzuzufügen, öffnen Sie den DataSet-Designer, mit der rechten Maustaste im Designer und wählen Sie hinzufügen / TableAdapter. Dadurch wird ein neues DataTable und einen TableAdapter erstellen und führt Sie durch den Assistenten, den wir zuvor in diesem Lernprogramm untersucht.

Erstellen Sie die folgenden Methoden, die mithilfe der folgenden Abfragen und TableAdapters einige Minuten dauern. Beachten Sie, dass die Abfragen in der `ProductsTableAdapter` enthalten die Unterabfragen, um Namen für jedes Produkt Kategorie und Lieferanten zu ziehen. Darüber hinaus, wenn Sie bislang befolgt haben zusammen, Sie haben bereits hinzugefügt der `ProductsTableAdapter` Klasse `GetProducts()` und `GetProductsByCategoryID(categoryID)` Methoden.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]


[![Der DataSet-Designer, nachdem die vier TableAdapter hinzugefügt wurden](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Abbildung 31**: der DataSet-Designer nach der vier TableAdapters wurden hinzugefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Hinzufügen von benutzerdefiniertem Code, der DAL

Die TableAdapters und die DataTables für die typisierte DataSet hinzugefügt werden, ausgedrückt als eine XML-Schemadefinition-Datei (`Northwind.xsd`). Sie können diese Schemainformationen anzeigen, indem Sie mit der rechten Maustaste auf die `Northwind.xsd` Datei im Projektmappen-Explorer, und wählen Sie Code anzeigen.


[![Die XML-Schemadefinition (XSD) Schemadatei für die Employees typisiertes DataSet](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Abbildung 32**: die XML-Schemadefinition (XSD)-Datei für die Employees typisierte DataSet ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image88.png))


Diese Schemainformationen wird zur Entwurfszeit in c# oder Visual Basic-Code übersetzt, bei der Kompilierung oder zur Laufzeit (falls erforderlich), an welchem Punkt Sie es mit dem Debugger schrittweise durchlaufen können. Zum Anzeigen dieser automatisch generiertem Code ausfallen, der Klassenansicht und einen Drilldown für die TableAdapter oder typisierte DataSet-Klassen. Wenn die Klassenansicht auf dem Bildschirm nicht angezeigt wird, wechseln Sie zum Menü "Ansicht" und wählen Sie es von dort aus, oder drücken Sie STRG + UMSCHALT + C. Aus der Klassenansicht sehen Sie die Eigenschaften, Methoden und Ereignisse, die typisierte DataSet und TableAdapter-Klassen. Um den Code für eine bestimmte Methode anzuzeigen, doppelklicken Sie auf den Methodennamen in der Klassenansicht oder mit der rechten Maustaste darauf, und wählen Sie Gehe zu Definition.


![Überprüfen Sie den automatisch generierten Code wechseln Sie zur Definition aus der Klassenansicht auswählen](creating-a-data-access-layer-vb/_static/image89.png)

**Abbildung 33**: Überprüfen Sie den automatisch generierten Code wechseln Sie zur Definition aus der Klassenansicht auswählen


Während der automatisch generierten Code eine hervorragende Zeitersparnis sein kann, wird der Code ist häufig sehr generisch und muss angepasst werden, um die speziellen Anforderungen einer Anwendung zu erfüllen. Erweitern von automatisch generierten Code das Risiko ist jedoch, dass das Tool, das Generieren des Codes, dass es ist Zeit ggf., "erneut generieren", und überschreiben Ihre Anpassungen. Mit .NET 2.0 neue Teilklasse Konzept ist es einfach zu eine Klasse auf mehrere Dateien aufgeteilt. Dadurch kann wir unsere eigene Methoden, Eigenschaften und Ereignisse, die automatisch generierten Klassen hinzufügen, ohne Visual Studio überschreiben unsere Anpassungen kümmern.

Um zum Anpassen der DAL zu demonstrieren, fügen Sie nun eine `GetProducts()` Methode, um die `SuppliersRow` Klasse. Die `SuppliersRow` Klasse stellt einen einzelnen Datensatz in der `Suppliers` Tabelle; jeder Lieferant können Anbieter 0 (null) für viele Produkte damit `GetProducts()` diese Produkte von den angegebenen Lieferanten zurück. Erstellen Sie erreichen eine neue Klassendatei in der `App_Code` Ordner mit dem Namen `SuppliersRow.vb` und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Diese partielle Klasse weist den Compiler, die bei Erstellen der `Northwind.SuppliersRow` Klasse einbeziehen, die `GetProducts()` Methode, die soeben definiert. Wenn Sie Ihr Projekt erstellen, und Sie dann mit der Klassenansicht fahren sehen Sie `GetProducts()` jetzt als eine Methode aufgeführt `Northwind.SuppliersRow`.


![Die Methode GetProducts() ist jetzt Teil der Northwind.SuppliersRow-Klasse](creating-a-data-access-layer-vb/_static/image90.png)

**Abbildung 34**: die `GetProducts()` Methode ist jetzt Teil der `Northwind.SuppliersRow` Klasse


Die `GetProducts()` Methode kann jetzt verwendet werden, um von Produkten für einen bestimmten Lieferanten, wie im folgenden Code dargestellt ist, aufgezählt:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Diese Daten können auch in einem ASP angezeigt werden. NET Daten Websteuerelemente. Die folgende Seite verwendet eine GridView-Steuerelements mit zwei Felder:

- Eine, die den Namen der einzelnen Lieferanten zeigt BoundField- und
- Ein TemplateField, die ein BulletedList-Steuerelement enthält, die von der zurückgegebenen Ergebnisse gebunden ist die `GetProducts()` Methode für jeden Lieferanten.

Vorgehensweise beim Anzeigen von solchen Master / Detail-Berichte in zukünftigen Lernprogrammen untersucht. Vorerst-dieses Beispiel wurde entworfen, um zu veranschaulichen, verwenden die benutzerdefinierte Methode hinzugefügt der `Northwind.SuppliersRow` Klasse.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]


[![Der Lieferant Firmenname wird in der linken Spalte, deren Produkte in der rechten Seite aufgeführt.](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Abbildung 35**: die Lieferanten Firmenname wird in der linken Spalte, deren Produkte in der rechten Ecke aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-data-access-layer-vb/_static/image93.png))


## <a name="summary"></a>Zusammenfassung

Beim Erstellen einer Webanwendung, die die DAL erstellen sollte einer der ersten Schritte, bevor Sie mit dem Erstellen der Darstellungsschicht beginnen. Mit Visual Studio ist eine DAL basierend auf typisierte DataSets erstellt eine Aufgabe, die in 10 bis 15 Minuten erreicht werden kann, ohne eine Codezeile schreiben zu müssen. Die Lernprogramme, die Hostdaten werden auf diesem DAL aufbauen. In der [nächsten Lernprogramm](creating-a-business-logic-layer-vb.md) wir definieren eine Reihe von Geschäftsregeln und finden Sie unter wie sie in eine separate Geschäftslogikschicht implementiert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen eine DAL, die mithilfe von stark typisierten TableAdapters und Datentabellen in Visual Studio 2005 und ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Entwerfen von Datenebenenkomponenten und übergeben von Daten mithilfe von Ebenen](https://msdn.microsoft.com/library/ms978496.aspx)
- [Erstellen Sie eine Datenzugriffsschicht mit DataSet-Designer von Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2.0 Anwendungen](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Übersicht über TableAdapters](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Arbeiten mit einem typisierten DataSet](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Mithilfe von stark typisierten Datenzugriff in Visual Studio 2005 und ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Gewusst wie: Erweitern Sie diejenigen TableAdapter-Methoden](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Lehrvideos auf die Themen in diesem Lernprogramm

- [Datenzugriffsschichten in ASP.NET-Anwendungen](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Wie Sie ein Dataset manuell an ein DataGrid-Steuerelement zu binden](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Wie Arbeiten mit Datasets und Filter aus einer ASP-Anwendung](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez und Carlos Santos. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](master-pages-and-site-navigation-cs.md)
> [Weiter](creating-a-business-logic-layer-vb.md)
