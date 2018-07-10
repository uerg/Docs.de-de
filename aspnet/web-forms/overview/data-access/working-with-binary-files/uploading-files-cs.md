---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
title: Hochladen von Dateien (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Erfahren Sie, wie Benutzer zum Hochladen von Binärdateien (z. B. Word oder PDF-Dokumente) können zu Ihrer Website, in denen die Server-Dateisystem gespeichert werden kann...
ms.author: aspnetcontent
ms.date: 03/27/2007
ms.assetid: b381b1da-feb3-4776-bc1b-75db53eb90ab
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-cs
msc.type: authoredcontent
ms.openlocfilehash: f420797b7c06b9063b70b784a5b61c7d02162c1d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820074"
---
<a name="uploading-files-c"></a>Hochladen von Dateien (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_CS.exe) oder [PDF-Datei herunterladen](uploading-files-cs/_static/datatutorial54cs1.pdf)

> Erfahren Sie, wie Benutzer zum Hochladen von Binärdateien (z. B. Word oder PDF-Dokumente) können zu Ihrer Website, in dem sie im Dateisystem des Servers oder der Datenbank gespeichert werden können.


## <a name="introduction"></a>Einführung

Alle in den Tutorials wir haben bisher untersucht haben daran gearbeitet, ausschließlich mit Textdaten. Viele Anwendungen müssen jedoch Datenmodelle, die sowohl Text als auch binäre Daten zu erfassen. Eine dating online-Website kann es sich um Benutzer ein Bild des Profils zugeordnet werden soll hochladen können. Eine personalbeschaffungs-Website kann Benutzer ihre fortsetzen als Microsoft Word oder PDF-Dokument hochladen können.

Arbeiten mit Binärdaten Fügt eine neue Reihe von Herausforderungen. Wir müssen entscheiden, wie die binären Daten in der Anwendung gespeichert ist. Die Schnittstelle für das Einfügen neuer Datensätze muss aktualisiert werden, um die Benutzer zum Hochladen einer Datei von ihrem Computer zu ermöglichen und zusätzliche Schritte erforderlich, um anzuzeigen, oder geben Sie, dass eine Methode für das Herunterladen eines Datensatzes s Binärdaten verknüpft ist. In diesem Tutorial und die folgenden drei lernen wir, wie Sie diese Herausforderungen Herausforderung. Am Ende der in diesen Tutorials werden wir eine voll funktionsfähige Anwendung erstellt haben, die ein Bild und PDF-Broschüre jeder Kategorie zuordnet. In diesem bestimmten Tutorial wir verschiedene Techniken zum Speichern von Binärdaten und erfahren Sie, wie Benutzer zum Hochladen einer Datei von ihrem Computer zu aktivieren und es auf das Dateisystem der Web-Server-s gespeichert haben.

> [!NOTE]
> Binäre Daten, die Teil einer Anwendung s-Datenmodells werden manchmal als bezeichnet ein [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), ein Akronym für Binary Large OBject. In diesen Tutorials habe ich entschieden, verwenden Sie die Terminologie binären Daten, obwohl der Begriff BLOB Synonym ist.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Schritt 1: Erstellen über das Arbeiten mit Binärdaten Web Pages

Bevor wir beginnen, um die Herausforderungen beim Hinzufügen von Unterstützung für binäre Daten zu untersuchen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserem Websiteprojekt zu erstellen, die wir für dieses Lernprogramm sowie die folgenden drei benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `BinaryData`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Fügen Sie die ASP.NET-Seiten für die binäre datenbezogenen Lernprogramme](uploading-files-cs/_static/image1.gif)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die binäre datenbezogenen Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `BinaryData` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](uploading-files-cs/_static/image2.gif)](uploading-files-cs/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image2.png))


Abschließend fügen Sie diese Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach dem Enhancing GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-cs/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für das Arbeiten mit Binärdaten Tutorials.


![Die Sitemap enthält jetzt die Einträge für das Arbeiten mit Binärdaten Tutorials](uploading-files-cs/_static/image3.gif)

**Abbildung 3**: die Sitemap enthält nun Einträge für das Arbeiten mit Binärdaten-Lernprogramme


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Schritt 2: Festlegen der Store der binären Daten

Binäre Daten, die das Anwendungsmodell für s Daten zugeordnet ist, können in einer von zwei Stellen gespeichert werden: auf das Dateisystem der Web-s-Server mit einem Verweis auf die Datei, die in der Datenbank gespeichert oder direkt in der Datenbank selbst (siehe Abbildung 4). Jeder Ansatz verfügt über einen eigenen Satz von vor- und Nachteile und benötigt eine ausführlichere Erläuterung.


[![Binäre Daten können im Dateisystem oder direkt in der Datenbank gespeichert werden](uploading-files-cs/_static/image4.gif)](uploading-files-cs/_static/image3.png)

**Abbildung 4**: Binärdaten können im Dateisystem oder direkt in der Datenbank gespeichert werden ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image4.png))


Angenommen Sie, wir wollten, erweitern Sie die Northwind-Datenbank, um ein Bild jedes Produkts zuzuordnen. Eine Möglichkeit wäre diese Bilddateien auf das Dateisystem der Web-s-Server zu speichern, und zeichnen Sie den Pfad in der `Products` Tabelle. Bei diesem Ansatz fügen wir d eine `ImagePath` Spalte, um die `Products` Tabelle vom Typ `varchar(200)`, vielleicht. Wenn ein Benutzer ein Bild für Chai hochgeladen werden, kann dieses Bild gespeichert werden, auf das Dateisystem der Web-s-Server am `~/Images/Tea.jpg`, wobei `~` physischen Pfad der Anwendung s darstellt. D. h., wenn die Website den physischen Pfad als Stamm ist `C:\Websites\Northwind\`, `~/Images/Tea.jpg` wäre entspricht `C:\Websites\Northwind\Images\Tea.jpg`. Nach dem Hochladen der Bilddatei, aktualisieren wir d den Chai Datensatz in die `Products` Tabelle, damit die `ImagePath` Spalte verwiesen wird, den Pfad des neuen Images. Wir können `~/Images/Tea.jpg` oder nur `Tea.jpg` Wenn wir uns entschieden, dass alle Produktbilder in der Anwendung s abgelegt werden `Images` Ordner.

Die Hauptvorteile von binären Daten im Dateisystem gespeichert sind:

- **Einfache Implementierung** wie wir in Kürze sehen werden, speichern und Abrufen von Binärdaten, die direkt in der Datenbank gespeicherten umfasst ein wenig mehr Code als beim Arbeiten mit Daten über das Dateisystem. Darüber hinaus müssen in der Reihenfolge für einen Benutzer zum Anzeigen oder Herunterladen von Binärdaten sie eine URL für die Daten angezeigt werden. Wenn die Daten auf s-Dateisystem der Web-Server befindet, ist die URL einfach. Wenn die Daten in der Datenbank gespeichert ist, jedoch eine Webseite muss erstellt werden, die abgerufen werden und die Daten aus der Datenbank zurückzugeben.
- **Größere Zugriff auf die Binärdaten** die binären Daten müssen möglicherweise mit anderen Diensten oder Anwendungen, für solche, die die Daten aus der Datenbank abrufen, kann nicht zugegriffen werden. Beispielsweise jedes Produkt zugeordneten Bilder auch über für Benutzer verfügbar sein müssen möglicherweise [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), in diesem Fall d soll die binären Daten im Dateisystem zu speichern.
- **Leistung** Wenn die binären Daten im Dateisystem gespeichert ist, wird die Nachfrage und Netzwerk-Überlastung zwischen der Datenbank-Server und Webserver werden kleiner als die binären Daten direkt in der Datenbank gespeichert ist.

Der Hauptnachteil der binäre Daten im Dateisystem gespeichert ist, dass es sich um die Daten aus der Datenbank entkoppelt. Wenn ein Datensatz gelöscht wird, aus der `Products` Tabelle, die zugehörige Datei im Dateisystem der Web-Server s werden nicht automatisch gelöscht. Schreiben wir, dass zusätzlicher Code So löschen Sie die Datei oder das Dateisystem mit nicht verwendeten, verwaiste Dateien überladen werden wird. Darüber hinaus müssen bei der Sicherung der Datenbank wir damit Sicherungen der zugeordneten Binärdaten im Dateisystem, ebenfalls sicherstellen. Verschieben der Datenbank auf einem anderen Standort oder ähnliche Server führt zu Problemen.

Binäre Daten können auch direkt in einer Microsoft SQL Server 2005-Datenbank gespeichert werden, erstellen Sie eine Spalte vom Typ `varbinary`. Wie kann mit anderen Datentypen variabler Länge Angabe eine maximale Länge der binären Daten, die gespeichert werden können in dieser Spalte. Beispielsweise, reservieren höchstens 5.000 Bytes verwenden `varbinary(5000)`; `varbinary(MAX)` ermöglicht die maximale Speichergröße, etwa 2 GB.

Der Hauptvorteil von binären Daten direkt in der Datenbank gespeichert ist, die enge Kopplung zwischen den binären Daten und den Datenbank-Datensatz. Dies vereinfacht deutlich Datenbankverwaltungsaufgaben, z. B. Sicherungen oder die Datenbank auf einem anderen Standort oder den Server verschieben. Darüber hinaus löscht das Löschen eines Datensatzes automatisch die entsprechenden Binärdaten. Es gibt auch weitere geringfügige Vorteile die binären Daten in der Datenbank gespeichert. Finden Sie unter [Speichern von binären Dateien direkt in die Datenbank mithilfe von ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) für eine ausführlichere Erläuterung.

> [!NOTE]
> In Microsoft SQL Server 2000 und früheren Versionen die `varbinary` -Datentyp aufwies, einem maximalen Grenzwert von 8.000 Bytes. Zum Speichern von bis zu 2 GB binärer Daten der [ `image` Datentyp](https://msdn.microsoft.com/library/ms187993.aspx) stattdessen verwendet werden muss. Durch das Hinzufügen von `MAX` in SQL Server 2005, jedoch die `image` -Datentyp ist veraltet. Es s noch Gründen der Abwärtskompatibilität unterstützt, aber Microsoft hat angekündigt, die die `image` -Datentyp in einer zukünftigen Version von SQL Server entfernt werden.


Bei Verwendung mit einem älteren Data Model können Sie sehen die `image` -Datentyp. Die Datenbank "Northwind" s `Categories` Tabelle besitzt eine `Picture` Spalte, die verwendet werden kann, um die binären Daten der Bilddatei für die Kategorie zu speichern. Da die Northwind-Datenbank ihren Wurzeln in Microsoft Access und früheren Versionen von SQL Server verfügt, ist diese Spalte vom Typ `image`.

Für dieses Lernprogramm sowie die folgenden drei verwenden wir beide Ansätze. Die `Categories` Tabelle verfügt bereits über eine `Picture` Spalte zum Speichern von Inhalt im Binärformat eines Bilds für die Kategorie. Wir fügen eine weitere Spalte, `BrochurePath`, um einen Pfad zu einer PDF-Datei im Dateisystem der Web-s-Server zu speichern, die verwendet werden können, um einen drucken hochwertigen, ansprechenden Überblick über die Kategorie bereitzustellen.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Schritt 3: Hinzufügen der`BrochurePath`Spalte, um die`Categories`Tabelle

Die Categories-Tabelle ist derzeit nur vier Spalten: `CategoryID`, `CategoryName`, `Description`, und `Picture`. Zusätzlich zu diesen Feldern ausführen müssen wir ein neues Gerät hinzufügen, das auf die Kategorie s Broschüre verweist (falls vorhanden). Wechseln Sie zum Hinzufügen dieser Spalte, um im Server-Explorer einen Drilldown in die Tabellen, mit der rechten Maustaste auf die `Categories` Tabelle, und wählen Sie die Tabellendefinition öffnen (siehe Abbildung 5). Wenn Sie den Server-Explorer nicht angezeigt werden, machen sie Sie durch Auswahl der Server-Explorer-Option im Menü Ansicht, oder drücken Sie Strg + Alt + S ein.

Fügen Sie einen neuen `varchar(200)` Spalte, um die `Categories` Tabelle mit der Bezeichnung `BrochurePath` und ermöglicht `NULL` s, und klicken Sie auf das Symbol "Speichern" (oder drücken Sie STRG + S).


[![Der Categories-Tabelle eine BrochurePath Spalte hinzufügen](uploading-files-cs/_static/image5.gif)](uploading-files-cs/_static/image5.png)

**Abbildung 5**: Hinzufügen einer `BrochurePath` Spalte, um die `Categories` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Schritt 4: Aktualisieren der Architektur für die Verwendung der`Picture`und`BrochurePath`Spalten

Die `CategoriesDataTable` in die (Data Access Layer, DAL) verfügt derzeit über vier `DataColumn` s definiert: `CategoryID`, `CategoryName`, `Description`, und `NumberOfProducts`. Wenn wir ursprünglich entworfen, dieser Datentabelle in der [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial die `CategoriesDataTable` mussten nur die ersten drei Spalten; die `NumberOfProducts` Spalte wurde hinzugefügt, die [Master-/Detailberichten mithilfe einer mit Aufzählungszeichen Liste der Masterdatensätze und einem DataList-Details Steuerelement](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) Tutorial.

Siehe *Erstellen einer Datenzugriffsschicht*, die Datentabellen in typisierten Datasets bilden die Geschäftsobjekte. Die TableAdapters sind verantwortlich für die Kommunikation mit der Datenbank und die Geschäftsobjekte mit den Abfrageergebnissen auffüllen. Die `CategoriesDataTable` werden ausgefüllt, indem die `CategoriesTableAdapter`, die über drei Datenabrufmethoden verfügt:

- `GetCategories()` führt die Hauptabfrage des TableAdapter s aus und gibt die `CategoryID`, `CategoryName`, und `Description` Felder aller Datensätze in der `Categories` Tabelle. Die Hauptabfrage wird verwendet, indem Sie den automatisch generierten `Insert` und `Update` Methoden.
- `GetCategoryByCategoryID(categoryID)` Gibt die `CategoryID`, `CategoryName`, und `Description` Felder der Kategorie, deren `CategoryID` gleich *CategoryID*.
- `GetCategoriesAndNumberOfProducts()` -Gibt die `CategoryID`, `CategoryName`, und `Description` Felder für alle Datensätze in der `Categories` Tabelle. Auch wird eine Unterabfrage verwendet, um die Anzahl der Produkte verknüpft ist, wobei jede Kategorie zurückzugeben.

Beachten Sie, dass keine dieser Abfragen zurückgeben der `Categories` s'-Tabelle `Picture` oder `BrochurePath` Spalten noch wird die `CategoriesDataTable` bieten `DataColumn` s für diese Felder. Für die Arbeit mit dem Bild und `BrochurePath` Eigenschaften, wir müssen zunächst zum Hinzufügen der `CategoriesDataTable` und aktualisieren Sie dann die `CategoriesTableAdapter` Klasse, um diese Spalten zurück.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Hinzufügen der`Picture`und`BrochurePath``DataColumn` s

Starten, indem diese beiden Spalten zum Hinzufügen der `CategoriesDataTable`. Mit der rechten Maustaste auf die `CategoriesDataTable` s-Header, wählen Sie aus dem Kontextmenü hinzufügen, und wählen Sie dann die Option Spalte. Hiermit wird ein neues `DataColumn` in der Datentabelle, die mit dem Namen `Column1`. Benennen Sie diese Spalte in `Picture`. Legen Sie im Eigenschaftenfenster die `DataColumn` s `DataType` Eigenschaft `System.Byte[]` (Dies ist dabei nicht um eine Option in der Dropdown-Liste, müssen Sie sie eingeben).


[![Erstellen Sie eine DataColumn mit dem Namen Bild ist, deren Datentyp System.Byte](uploading-files-cs/_static/image6.gif)](uploading-files-cs/_static/image7.png)

**Abbildung 6**: Erstellen einer `DataColumn` benannte `Picture` , deren `DataType` ist `System.Byte[]` ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image8.png))


Fügen Sie ein weiteres `DataColumn` der Datentabelle, nennen Sie es `BrochurePath` über das standardmäßige `DataType` Wert (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zurückgeben der`Picture`und`BrochurePath`Werte aus dem TableAdapter

Mit diesen beiden `DataColumn` s hinzugefügt, die `CategoriesDataTable`, wir erneut bereit, die zum Aktualisieren der `CategoriesTableAdapter`. Es wäre möglich gewesen, beide dieser Spaltenwerte in der Hauptabfrage des TableAdapter zurückgegeben, aber dies würde zurückbringen die binären Daten jedes Mal die `GetCategories()` Methode wurde aufgerufen. Let s aktualisieren Sie stattdessen die Hauptabfrage der TableAdapter rüstet `BrochurePath` , und erstellen Sie eine weitere Daten abrufen-Methode, die eine bestimmte Kategorie s zurückgibt `Picture` Spalte.

Um die Hauptabfrage des TableAdapter zu aktualisieren, mit der Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie im Kontextmenü die Option konfigurieren. Dadurch wird die Tabelle Konfigurations-Assistenten, die wir in einer Reihe von früheren Tutorials gesehen haben. Aktualisieren Sie die Abfrage rüstet die `BrochurePath` , und klicken Sie auf "Fertig stellen".


[![Aktualisieren der Liste der Spalten in der SELECT-Anweisung auch BrochurePath zurückgeben](uploading-files-cs/_static/image7.gif)](uploading-files-cs/_static/image9.png)

**Abbildung 7**: Aktualisieren der Liste der Spalten in der `SELECT` Anweisung, um auch zurückzugeben `BrochurePath` ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image10.png))


Wenn Sie Ad-hoc-SQL-Anweisungen für den TableAdapter verwenden, Aktualisieren der Liste der Spalten in der Hauptabfrage die Liste der Spalten für alle aktualisiert die `SELECT` Abfragemethoden im TableAdapter. Das bedeutet, dass die `GetCategoryByCategoryID(categoryID)` Methode wurde aktualisiert, um das Zurückgeben der `BrochurePath` Spalte, die möglicherweise, was wir es beabsichtigt haben. Es jedoch in die Liste der Spalten auch aktualisiert die `GetCategoriesAndNumberOfProducts()` -Methode, entfernen Sie die Unterabfrage, die die Anzahl der Produkte für die einzelnen Kategorien gibt! Aus diesem Grund müssen wir diese Methode s aktualisieren `SELECT` Abfrage. Mit der rechten Maustaste auf die `GetCategoriesAndNumberOfProducts()` Methode konfigurieren auswählen und Wiederherstellen der `SELECT` Abfrage zurück an den ursprünglichen Wert:


[!code-sql[Main](uploading-files-cs/samples/sample2.sql)]

Als Nächstes erstellen Sie eine neue TableAdapter-Methode, die eine bestimmte Kategorie s zurückgibt `Picture` Spaltenwert. Mit der rechten Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie die Option "Abfrage hinzufügen" zum TableAdapter-Konfigurations-Assistenten zu starten. Der erste Schritt des Assistenten werden wir gefragt, wenn Sie möchten zum Abfragen von Daten mit Ad-hoc-SQL-Anweisungen, eine neue, gespeicherte Prozedur bzw. eine vorhandene Ressourcengruppe. Wählen Sie die SQL-Anweisungen, und klicken Sie auf Weiter. Da wir eine Zeile zurückgegeben wird, wählen Sie die wählen die Option der Zeilen im zweiten Schritt zurückgibt.


[![Wählen Sie die SQL-Anweisungen-Option](uploading-files-cs/_static/image8.gif)](uploading-files-cs/_static/image11.png)

**Abbildung 8**: Wählen Sie die SQL-Anweisungen-Option ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image12.png))


[![Da der Abfrage einen Datensatz aus der Categories-Tabelle zurückgegeben wird, wählen Sie wählen Sie die Zeilen zurückgibt](uploading-files-cs/_static/image9.gif)](uploading-files-cs/_static/image13.png)

**Abbildung 9**: Da die Abfrage einen Datensatz aus der Tabelle "Categories", wählen Sie die Option zurückgeben wird, die Zeilen zurückgibt ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image14.png))


Klicken Sie im dritten Schritt geben Sie folgende SQL-Abfrage, und klicken Sie auf Weiter:


[!code-sql[Main](uploading-files-cs/samples/sample3.sql)]

Der letzte Schritt ist, um den Namen für die neue Methode auszuwählen. Verwendung `FillCategoryWithBinaryDataByCategoryID` und `GetCategoryWithBinaryDataByCategoryID` für die Füllung einer DataTable und Rückgabe eine "DataTable"-Mustern bzw. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Wählen Sie die Namen für die TableAdapter-s-Methoden](uploading-files-cs/_static/image10.gif)](uploading-files-cs/_static/image15.png)

**Abbildung 10**: Wählen Sie die Namen für die TableAdapter-Methoden ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image16.png))


> [!NOTE]
> Nach Abschluss der Tabelle Abfragen Konfigurations-Assistenten möglicherweise ein Dialogfeld, das Sie darüber informiert, dass der neue Befehlstext aus dem Schema der Hauptabfrage unterschiedliche Daten mit Schema zurückgibt. Kurz gesagt: der Assistent wird hingewiesen werden, die die Hauptabfrage des TableAdapter s `GetCategories()` gibt ein anderes Schema als dem wir gerade erstellt haben. Aber wir möchten, damit Sie diese Meldung ignorieren können.


Beachten Sie außerdem, dass, wenn Sie Ad-hoc-SQL-Anweisungen verwenden und mithilfe des Assistenten zum Ändern der TableAdapter Hauptabfrage s zu einem späteren Zeitpunkt in-Time, geändert wird die `GetCategoryWithBinaryDataByCategoryID` s-Methode `SELECT` Anweisung s Spaltenliste sollen nur die Spalten aus der Hauptabfrage (d. h. er entfernt die `Picture` Spalte aus der Abfrage). Zurückgeben die Liste der Spalten manuell aktualisieren müssen die `Picture` Spalte, ähnlich wie die `GetCategoriesAndNumberOfProducts()` Methode weiter oben in diesem Schritt.

Nach dem Hinzufügen der beiden `DataColumn` s, um die `CategoriesDataTable` und `GetCategoryWithBinaryDataByCategoryID` Methode, um die `CategoriesTableAdapter`, diese Klassen im typisierten DataSet-Designer sollte wie im Screenshot in Abbildung 11 aussehen.


![Der DataSet-Designer enthält die neue Spalten und die Methode](uploading-files-cs/_static/image11.gif)

**Abbildung 11**: der DataSet-Designer enthält die neue Spalten und die Methode


## <a name="updating-the-business-logic-layer-bll"></a>Aktualisieren die Geschäftslogikschicht (BLL)

Mit der DAL aktualisiert, übrig bleibt, erweitern Sie die Geschäftslogikschicht (Business Logic Layer, BLL) enthält eine Methode für das neue `CategoriesTableAdapter` Methode. Fügen Sie die folgende Methode der `CategoriesBLL` Klasse:


[!code-csharp[Main](uploading-files-cs/samples/sample4.cs)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Schritt 5: Hochladen einer Datei vom Client an den Webserver

Beim Sammeln von Binärdaten werden häufig durch einen Endbenutzer diese Daten bereitgestellt. Um diese Informationen erfassen zu können, muss der Benutzer eine Datei von ihrem Computer an den Webserver hochladen können. Die hochgeladenen Daten müssen mit dem Datenmodell, das Speichern der Datei auf das Dateisystem der Web-Server-s und Hinzufügen eines Pfads zur Datei in der Datenbank oder den binären Inhalt direkt in der Datenbank schreiben kann bedeuten, integriert werden. In diesem Schritt untersuchen wir an, wie einen Benutzer zum Hochladen von Dateien von ihrem Computer, auf dem Server zu ermöglichen. Im nächsten Tutorial werden wir unsere Aufmerksamkeit zum Integrieren der hochgeladenen Datei in Datenmodell aktivieren.

ASP.NET 2.0 neues [FileUpload-Websteuerelement](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) bietet einen Mechanismus für Benutzer auf eine Datei von ihrem Computer an den Webserver zu senden. Der FileUpload-Steuerelement wird als ein `<input>` Element, dessen `type` -Attribut auf die Datei, die in Browsern, wie ein Textfeld mit einer Schaltfläche zum Durchsuchen angezeigt festgelegt ist. Durch Klicken auf die Schaltfläche zum Durchsuchen wird ein Dialogfeld, in dem der Benutzer eine Datei auswählen kann. Wenn das Formular erneut gesendet wird, werden den Inhalt der ausgewählten Datei s zusammen mit dem Postback gesendet. Auf der Serverseite ist die Informationen über die hochgeladene Datei über die Eigenschaften "FileUpload"-Steuerelement zugänglich.

Öffnen Sie zum Hochladen von Dateien zu veranschaulichen, die `FileUpload.aspx` auf der Seite die `BinaryData` , ein "FileUpload"-Steuerelement aus der Toolbox in den Designer ziehen, und legen Sie das Steuerelement s `ID` Eigenschaft `UploadTest`. Fügen Sie eine Schaltfläche Websteuerelement Festlegen seiner `ID` und `Text` Eigenschaften `UploadButton` und Laden Sie die ausgewählte Datei bzw. hoch. Setzen Sie ein Label-Steuerelement unterhalb der Schaltfläche, löschen Sie schließlich die `Text` Eigenschaft, und legen seine `ID` Eigenschaft `UploadDetails`.


[![Hinzufügen einer FileUpload-Serversteuerelements in die ASP.NET-Seite](uploading-files-cs/_static/image12.gif)](uploading-files-cs/_static/image17.png)

**Abbildung 12**: Hinzufügen einer FileUpload-Serversteuerelements in die ASP.NET-Seite ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image18.png))


Abbildung 13 zeigt diese Seite, wenn Sie über einen Browser angezeigt. Beachten Sie, dass Sie auf die Schaltfläche zum Durchsuchen ein Dialogfeld zur Dateiauswahl, öffnet der Benutzer zum Auswählen einer Datei von ihrem Computer. Nachdem eine Datei ausgewählt wurde, führt dazu, dass Sie auf die Schaltfläche für die ausgewählte Datei hochladen einen Postback aus, der die ausgewählte Datei s binären Inhalt an den Webserver sendet.


[![Der Benutzer kann eine Datei zum Hochladen von ihrem Computer an den Server auswählen.](uploading-files-cs/_static/image13.gif)](uploading-files-cs/_static/image19.png)

**Abbildung 13**: der Benutzer kann eine Datei zum Hochladen auswählen, von dem Computer, auf dem Server ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image20.png))


Beim Postback die hochgeladene Datei im Dateisystem gespeichert werden kann, oder die binäre Daten mit gearbeitet werden können, direkt über einen Stream. In diesem Beispiel können Sie s, erstellen Sie eine `~/Brochures` Ordner und speichern Sie die hochgeladene Datei. Starten Sie durch Hinzufügen der `Brochures` Ordner an den Standort als Unterordner des Stammverzeichnisses. Als Nächstes erstellen Sie einen Ereignishandler für die `UploadButton` s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](uploading-files-cs/samples/sample5.cs)]

Das "FileUpload"-Steuerelement bietet eine Vielzahl von Eigenschaften für die Arbeit mit der hochgeladenen Daten. Z. B. die [ `HasFile` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) gibt an, ob eine Datei, vom Benutzer hochgeladen wurde während der [ `FileBytes` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) bietet Zugriff auf die hochgeladenen Binärdaten als ein Array von Bytes. Die `Click` Ereignishandler startet, indem Sie sicherstellen, dass eine Datei hochgeladen wurde. Wenn eine Datei hochgeladen wurde, zeigt die Bezeichnung der Name der hochgeladenen Datei, die Größe in Bytes und dem Inhaltstyp an.

> [!NOTE]
> Um sicherzustellen, dass der Benutzer eine Datei hochlädt, sehen Sie sich, die `HasFile` Eigenschaft und eine Warnung angezeigt, wenn es s `false`, oder Sie können die [RequiredFieldValidator-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) stattdessen.


Die "FileUpload" s `SaveAs(filePath)` speichert die hochgeladene Datei im angegebenen *"FilePath"*. *"FilePath"* muss eine *physischer Pfad* (`C:\Websites\Brochures\SomeFile.pdf`) anstelle eines *virtuellen* *Pfad* (`/Brochures/SomeFile.pdf`). Die [ `Server.MapPath(virtPath)` Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) akzeptiert einen virtuellen Pfad und den zugehörigen physischen Pfad zurückgegeben. Hier ist der virtuelle Pfad ist `~/Brochures/fileName`, wobei *FileName* ist der Name der hochgeladenen Datei. Finden Sie unter [mithilfe von Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) für Weitere Informationen zur virtuellen und physischen Pfade und die Verwendung von `Server.MapPath`.

Nach Abschluss der `Click` -Ereignishandler können Sie die Seite in einem Browser zu testen. Klicken Sie auf die Schaltfläche zum Durchsuchen, und wählen Sie eine Datei auf Ihrer Festplatte, und klicken Sie dann auf die Schaltfläche "ausgewählten Datei hochladen". Das Postback sendet den Inhalt der ausgewählten Datei an den Webserver, der anschließend Informationen über die Datei vor dem Speichern auf angezeigt wird der `~/Brochures` Ordner. Klicken Sie nach dem Hochladen der das zurück zu Visual Studio, und klicken Sie auf die Schaltfläche "Aktualisieren" im Projektmappen-Explorer. Daraufhin sollte die Datei, die Sie gerade hochgeladen, im Ordner "~/Brochures haben"!


[![Die Datei EvolutionValley.jpg wurde an den Webserver hochgeladen](uploading-files-cs/_static/image14.gif)](uploading-files-cs/_static/image21.png)

**Abbildung 14**: die Datei `EvolutionValley.jpg` hochgeladen wurde an den Webserver ([klicken Sie, um das Bild in voller Größe anzeigen](uploading-files-cs/_static/image22.png))


![EvolutionValley.jpg wurde in den Ordner ~/Brochures gespeichert.](uploading-files-cs/_static/image15.gif)

**Abbildung 15**: `EvolutionValley.jpg` wurde gespeichert, um die `~/Brochures` Ordner


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Die feinheiten beim Speichern der hochgeladener Dateien im Dateisystem

Es gibt einige Besonderheiten, die beim Speichern der Hochladen von Dateien in das Dateisystem der Web-Server-s berücksichtigt werden müssen. Zunächst wird dort s das Problem der Sicherheit. Um eine Datei im Dateisystem zu speichern, muss der Sicherheitskontext an, unter dem die ASP.NET-Seite ausgeführt wird, Schreibberechtigungen verfügen. Der ASP.NET Development Web Server, die im Kontext des aktuellen Benutzerkontos ausgeführt wird. Wenn Sie Microsoft-s (Internet Information Services, IIS) als Webserver verwenden, hängt von der Sicherheitskontext Version von IIS und die Konfiguration.

Eine weitere Herausforderung des Speicherns von Dateien im Dateisystem dreht sich um die Benennung der Dateien. Derzeit auf unserer Seite speichert alle hochgeladenen Dateien, die `~/Brochures` Verzeichnis mit dem gleichen Namen wie die Datei auf dem Clientcomputer s verfügen. Wenn Benutzer A eine Broschüre mit dem Namen hochlädt `Brochure.pdf`, wird die Datei gespeichert werden, als `~/Brochure/Brochure.pdf`. Aber was geschieht, wenn einige Zeit später Benutzer B eine andere Broschüre-Datei hochgeladen wird, die zufällig den gleichen Dateinamen haben (`Brochure.pdf`)? Durch den Code haben wir nun, Benutzer, die eine s-Datei mit User B s Upload überschrieben wird.

Es gibt eine Reihe von Techniken zum Lösen von Namenskonflikten. Eine Möglichkeit ist, wenn keine Hochladen einer Datei aus, wenn eine mit dem gleichen Namen bereits vorhanden. Bei diesem Ansatz, wenn Benutzer B versucht, zum Hochladen einer Datei mit dem Namen `Brochure.pdf`, würde das System nicht speichern Sie ihre Datei und zeigt stattdessen eine Nachricht an Benutzer B zum Umbenennen der Datei, und versuchen Sie es erneut. Speichern Sie die Datei, die über einen eindeutigen Dateinamen, die möglicherweise ein anderer Ansatz ist eine [global eindeutigen Bezeichner (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) oder den Wert aus der entsprechenden Datenbank-Datensatz s Primärschlüsselspalte(n) (vorausgesetzt, dass der Upload zugeordnet ist ein bestimmte Zeile in das Datenmodell). Im nächsten Tutorial werden wir diese Optionen ausführlicher erläutert.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Herausforderungen mit sehr großen Mengen von Binärdaten

In diesen Tutorials wird davon ausgegangen, dass die binären Daten erfasst mittlere Größe ist. Arbeiten mit sehr großen Datenmengen binärdatendateien gespeichert, die mehrere Megabyte oder mehr führt neue Herausforderungen, die in diesen Tutorials nicht eingegangen sind. Beispielsweise standardmäßig ASP.NET lehnt Uploads von mehr als 4 MB, auch wenn dies über konfiguriert werden, kann die [ `<httpRuntime>` Element](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`. Die größenbeschränkungen eine eigene Datei hochladen, wird von IIS zu erzwingt. Finden Sie unter [IIS hochladen Dateigröße](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) für Weitere Informationen. Darüber hinaus kann die Zeit zum Hochladen großer Dateien standardmäßig 110 Sekunden überschreiten, die ASP.NET eine Anforderung gewartet wird. Es gibt auch Arbeitsspeicher- und Leistungsproblemen Probleme, die beim Arbeiten mit großen Dateien auftreten.

Das Steuerelement "FileUpload" ist für große Dateiuploads unpraktisch. Wie s den Inhalt der Datei an den Server zurückgesendet wird, sind, muss der Endbenutzer geduldig ohne eine Bestätigung warten, bis dieser den Fortschritt ihrer hochladen. Dies ist nicht so sehr ein Problem, bei kleineren Dateien, die in wenigen Sekunden hochgeladen werden können, jedoch kann ein Problem sein, wenn mit größeren Dateien geht, die den hochzuladenden länger dauern kann. Gibt eine Vielzahl von Drittanbieter-Datei hochladen-Steuerelemente, die für die Behandlung großer Uploads besser geeignet sind, und viele von diesen Anbietern angeben, dass Statusanzeigen und ActiveX-Manager hochladen, die ein viel besseres Benutzererlebnis darstellen.

Wenn Ihre Anwendung muss sich um große Dateien zu verarbeiten, müssen Sie sorgfältig untersuchen, die Herausforderungen, und suchen geeignete Lösungen für Ihre besonderen Bedürfnisse.

## <a name="summary"></a>Zusammenfassung

Erstellen einer Anwendung, die zum Erfassen von binärer Daten benötigt, führt eine Reihe von Herausforderungen. In diesem Tutorial haben wir untersucht, die ersten beiden: Entscheidung, wo sich die binären Daten speichern, und da der Benutzer zum Hochladen von Inhalt im Binärformat über eine Webseite. Über die nächsten drei Tutorials sehen wir, einen Datensatz in der Datenbank die hochgeladenen Daten zugeordnet werden soll und wie die binären Daten zusammen mit der Textfelder für die Daten angezeigt.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Verwenden von Datentypen mit umfangreichen Werten](https://msdn.microsoft.com/library/ms178158.aspx)
- ["FileUpload"-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [ASP.NET 2.0 FileUpload-Serversteuerelements](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Die dunkle Seite des Dateiuploads](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und Bernadette Leigh. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](displaying-binary-data-in-the-data-web-controls-cs.md)
