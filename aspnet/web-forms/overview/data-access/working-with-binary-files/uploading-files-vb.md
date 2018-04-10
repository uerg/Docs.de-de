---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Hochladen von Dateien (VB) | Microsoft Docs
author: rick-anderson
description: Erfahren Sie, wie Benutzer zum Hochladen von binären Dateien (z. B. Word oder PDF-Dokumente) können mit der Website, wo sie im Dateisystem des Servers gespeichert werden können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/27/2007
ms.topic: article
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: fbc4aaf80ac7e0f960e140b492055fe35cd2b6ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="uploading-files-vb"></a>Hochladen von Dateien (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) oder [PDF herunterladen](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Erfahren Sie, wie Benutzer zum Hochladen von binären Dateien (z. B. Word oder PDF-Dokumente) können mit der Website, wo sie im Dateisystem des Servers oder der Datenbank gespeichert werden können.


## <a name="introduction"></a>Einführung

Alle Lernprogramme wir haben bisher untersucht ausschließlich mit Textdaten gearbeitet haben. Viele Anwendungen müssen jedoch Datenmodelle, die Text und Binärdaten erfassen. Eine online-dating-Website können Benutzer zum Hochladen eines Bildes, das ihrem Profil zugeordnet. Eine Personalbeschaffungsstatus Website kann Benutzer ihrer fortsetzen als Microsoft Word oder PDF-Dokument hochladen können.

Arbeiten mit Binärdaten Fügt einen neuen Satz von Aufgaben hinzu. Wir müssen entscheiden, wie die binären Daten in der Anwendung gespeichert ist. Die Schnittstelle für das Einfügen neuer Datensätze muss aktualisiert werden, damit der Benutzer eine Datei von ihrem Computer hochladen kann, und zusätzliche Schritte erforderlich, zum Anzeigen oder angeben, dass eine Möglichkeit zum Herunterladen eines Datensatzes s Binärdaten verknüpft sind. In diesem Lernprogramm und den nächsten drei müssen wie dieser Herausforderung hurdle untersucht. Am Ende der Ausführung dieser Lernprogramme müssen wir eine voll funktionsfähige Anwendung erstellt haben, die jeder Kategorie ein Bild und PDF Broschüren zuordnet. In diesem Lernprogramm fügen wir sehen uns verschiedene Techniken zum Speichern von Binärdaten und untersuchen Sie zum Aktivieren der Benutzer zum Hochladen einer Datei von ihrem Computer und sind im Web Server s-Dateisystem gespeichert.

> [!NOTE]
> Binäre Daten, die Teil einer Anwendung s-Datenmodell ist werden manchmal als bezeichnet eine [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), ein Akronym für Binary Large OBject. In diesen Lernprogrammen haben ich die Terminologie Binärdaten verwenden, obwohl der Begriff BLOB gleichbedeutend ist.


## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Schritt 1: Erstellen der Arbeiten mit Binärdaten Webseiten

Bevor wir beginnen, die Probleme beim Hinzufügen der Unterstützung für binäre Daten untersuchen, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für dieses Lernprogramm und den nächsten drei benötigen. Starten, indem Sie einen neuen Ordner namens `BinaryData`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`


![Fügen Sie die ASP.NET-Seiten für die binäre datenbezogene-Lernprogramme](uploading-files-vb/_static/image1.gif)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die binäre datenbezogene-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `BinaryData` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image2.png))


Abschließend fügen Sie diese Seiten als Einträge an die `Web.sitemap` Datei. Fügen Sie insbesondere das folgende Markup nach dem Enhancing GridView `<siteMapNode>`:


[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält die Elemente jetzt für das Arbeiten mit Binärdaten Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für das Arbeiten mit Binärdaten-Lernprogramme](uploading-files-vb/_static/image3.gif)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge für das Arbeiten mit Binärdaten-Lernprogramme


## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Schritt 2: Die Entscheidung, wo die binären Daten zu speichern.

Binäre Daten, die das Anwendungsmodell für s Daten zugeordnet ist, können in einer von zwei Stellen gespeichert werden: im Dateisystem des Web-Server s mit einem Verweis auf die Datei, die in der Datenbank gespeichert oder direkt in die Datenbank selbst (siehe Abbildung 4). Jeder Ansatz verfügt über einen eigenen Satz von vor- und Nachteile und verdient eine ausführlichere Erläuterung.


[![Binärdaten können im Dateisystem oder direkt in der Datenbank gespeichert werden](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Abbildung 4**: Binärdaten können im Dateisystem oder direkt in der Datenbank gespeichert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image4.png))


Angenommen Sie, wir, erweitern die Northwind-Datenbank, um ein Bild mit den einzelnen Produkten zuzuordnen möchten. Eine Möglichkeit wäre diese Bilddateien im Web Server s-Dateisystem zu speichern, und notieren den Pfad in der `Products` Tabelle. Bei diesem Ansatz fügen wir d ein `ImagePath` Spalte, um die `Products` Tabelle vom Typ `varchar(200)`, vielleicht. Wenn ein Benutzer ein Bild für Chai hochgeladen, möglicherweise das Bild gespeichert werden, im Dateisystem des Web-Server s an `~/Images/Tea.jpg`, wobei `~` der physischen s Anwendungspfad darstellt. D. h., wenn der physische Pfad der Website Stamm ist `C:\Websites\Northwind\`, `~/Images/Tea.jpg` entspräche `C:\Websites\Northwind\Images\Tea.jpg`. Nach dem Hochladen der Bilddatei, aktualisieren wir d den Chai Datensatz in der `Products` Tabelle, damit seine `ImagePath` Spalte verwiesen wird, den Pfad des neuen Images. Verwenden wir `~/Images/Tea.jpg` oder einfach `Tea.jpg` Wenn wir uns entschlossen haben, dass alle Produktbilder in der Anwendung s befinden würde `Images` Ordner.

Die Hauptvorteile von binären Daten im Dateisystem gespeichert werden:

- **Einfache Implementierung** in Kürze eingehendem, speichern und Abrufen von Binärdaten, die direkt in der Datenbank gespeichert umfasst ein wenig mehr Code als beim Arbeiten mit Daten über das Dateisystem. Darüber hinaus müssen in der Reihenfolge für einen Benutzer zum Anzeigen oder Herunterladen von Binärdaten ihnen mit einer URL, um die Daten angezeigt. Wenn die Daten im Web Server s-Dateisystem befinden, lautet die URL unkompliziert. Wenn die Daten in der Datenbank gespeichert ist, jedoch eine Webseite muss erstellt werden, die abgerufen werden und die Daten aus der Datenbank zurück.
- **Umfassenderen Zugriff auf die Binärdaten** die binären Daten möglicherweise für andere Dienste oder Anwendungen, Argumente, die die Daten aus der Datenbank abrufen kann nicht zugegriffen werden. Angenommen, die Bilder, die mit den einzelnen Produkten verknüpft sind auch über für Benutzer verfügbar sein müssen möglicherweise [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), in diesem Fall werden wir d die binären Daten im Dateisystem speichern soll.
- **Leistung** Wenn die Binärdaten im Dateisystem gespeichert ist, wird die Überlastung bei Bedarf und das Netzwerk zwischen dem Datenbankserver und den Webserver werden kleiner, wenn die binären Daten direkt in der Datenbank gespeichert.

Der wichtigste Nachteil von Binärdaten im Dateisystem gespeichert ist, dass sie die Daten aus der Datenbank entkoppelt. Wenn ein Datensatz gelöscht wird, aus der `Products` Tabelle, die zugehörige Datei im Web Server s-Dateisystem werden nicht automatisch gelöscht. Wir müssen schreiben, dass zusätzlicher Code So löschen Sie die Datei oder das Dateisystem mit Dateien, die nicht verwendete, verwaiste Modellabfragen wird. Darüber hinaus muss beim Sichern der Datenbank, wir stellen Sicherungen der zugeordneten Binärdaten im Dateisystem als auch sicherstellen. Verschieben der Datenbank auf einem anderen Standort oder Server Posen ähnliche Probleme.

Binärdaten können auch direkt in einer Microsoft SQL Server 2005-Datenbank gespeichert werden, durch das Erstellen einer Spalteninhalts vom Typ `varbinary`. Wie kann mit anderen Datentypen variabler Länge Angabe eine maximale Länge der binären Daten, die gespeichert werden können in dieser Spalte. Beispielsweise reserviert darf höchstens 5.000 Bytes verwenden `varbinary(5000)`; `varbinary(MAX)` ermöglicht die maximale Speichergröße ungefähr 2 GB.

Der Hauptvorteil des Speicherns von binären Daten direkt in der Datenbank ist die enge Kopplung zwischen den binären Daten und den Datenbank-Datensatz. Dies vereinfacht Datenbankverwaltungsaufgaben wie Sicherungen oder auf einem anderen Standort oder den Server verschoben wird. Darüber hinaus löscht das Löschen eines Datensatzes automatisch die entsprechenden Binärdaten. Es sind auch weitere feine Vorteile des Speicherns von Binärdaten in der Datenbank. Finden Sie unter [Speichern von binären Dateien direkt in die Datenbank mithilfe von ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) für eine ausführliche Erläuterung.

> [!NOTE]
> In Microsoft SQL Server 2000 und früheren Versionen der `varbinary` -Datentyp aufwies einem maximalen Grenzwert von 8.000 Bytes. Zum Speichern von bis zu 2 GB Binärdaten der [ `image` Datentyp](https://msdn.microsoft.com/library/ms187993.aspx) stattdessen verwendet werden muss. Durch das Hinzufügen von `MAX` in SQL Server 2005, jedoch die `image` -Datentyp ist veraltet. Es s weiterhin Gründen der Abwärtskompatibilität unterstützt, aber Microsoft hat angekündigt, die die `image` -Datentyp wird in einer zukünftigen Version von SQL Server entfernt.


Bei Verwendung mit einem älteren Data Model wird möglicherweise die `image` -Datentyp. Die Datenbank "Northwind" s `Categories` Tabelle besitzt eine `Picture` Spalte, die verwendet werden kann, um die binären Daten der Bilddatei für die Kategorie zu speichern. Da die Northwind-Datenbank die Stämme in Microsoft Access und früheren Versionen von SQL Server enthält, ist diese Spalte vom Typ `image`.

Für dieses Lernprogramm sowie die nächsten drei verwenden wir beide Ansätze. Die `Categories` Tabelle verfügt bereits über eine `Picture` Spalte zum Speichern von Inhalt im Binärformat eines Bilds für die Kategorie. Fügen wir eine zusätzliche Spalte `BrochurePath`, um einen Pfad zu einer PDF-Datei im Web Server s-Dateisystem zu speichern, die verwendet werden kann, um einen Druck, verfeinerte Überblick über die Kategorie bereitzustellen.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Schritt 3: Hinzufügen der`BrochurePath`Spalte die`Categories`Tabelle

Die Categories-Tabelle hat derzeit nur vier Spalten: `CategoryID`, `CategoryName`, `Description`, und `Picture`. Zusätzlich zu diesen Feldern ausführen müssen wir ein neues Konto hinzufügen, das auf die Kategorie s Broschüren verweisen wird, (falls vorhanden). Wechseln Sie zum Hinzufügen dieser Spalte auf dem Server-Explorer, ein Drilldown in die Tabellen, mit der rechten Maustaste auf die `Categories` -Tabelle und wählen Sie die Tabellendefinition öffnen (siehe Abbildung 5). Wenn Sie den Server-Explorer nicht angezeigt werden, bringen Sie, wenn der Server-Explorer-Option auswählen, aus dem Menü "Ansicht", oder drücken Sie Strg + Alt + S.

Fügen Sie einen neuen `varchar(200)` Spalte, um die `Categories` Tabelle mit der Bezeichnung `BrochurePath` und ermöglicht `NULL` s, und klicken Sie auf das Symbol "Speichern" (oder drücken Sie STRG + S).


[![Der Categories-Tabelle eine Spalte BrochurePath hinzufügen](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Abbildung 5**: Hinzufügen einer `BrochurePath` Spalte, um die `Categories` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image6.png))


## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Schritt 4: Aktualisieren der Architektur zum Verwenden der`Picture`und`BrochurePath`Spalten

Die `CategoriesDataTable` im Data Access Layer (DAL) verfügt derzeit über vier `DataColumn` s definiert: `CategoryID`, `CategoryName`, `Description`, und `NumberOfProducts`. Wenn wir diese DataTable in ursprünglich entworfen der [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Lernprogramm die `CategoriesDataTable` hatte nur die ersten drei Spalten; das `NumberOfProducts` Spalte wurde hinzugefügt, die [Master/Detail mithilfe einer Aufzählung Liste der Master-Datensätze mit Details DataList](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) Lernprogramm.

Entsprechend der Anleitung unter *erstellen eine Datenzugriffsschicht*, die Datentabellen in das typisierte DataSet bilden die Business-Objekte. Die TableAdapters sind verantwortlich für die Kommunikation mit der Datenbank und die Business-Objekte mit den Abfrageergebnissen aufgefüllt. Die `CategoriesDataTable` werden ausgefüllt, indem die `CategoriesTableAdapter`, die über drei Datenabrufmethoden verfügt:

- `GetCategories()` führt die Hauptabfrage des TableAdapter s aus und gibt die `CategoryID`, `CategoryName`, und `Description` Felder aller Datensätze in der `Categories` Tabelle. Die Hauptabfrage wird verwendet, durch die automatisch generierte `Insert` und `Update` Methoden.
- `GetCategoryByCategoryID(categoryID)` Gibt die `CategoryID`, `CategoryName`, und `Description` Felder der Kategorie, deren `CategoryID` gleich *CategoryID*.
- `GetCategoriesAndNumberOfProducts()` -Gibt die `CategoryID`, `CategoryName`, und `Description` Felder für alle Datensätze in der `Categories` Tabelle. Außerdem wird eine Unterabfrage verwendet, zum Zurückgeben der Anzahl der Produkte jeder Kategorie zugeordnet ist.

Beachten Sie, dass keine der folgenden zurückgeben Abfragen der `Categories` Tabelle s `Picture` oder `BrochurePath` Spalten; noch ist die `CategoriesDataTable` bieten `DataColumn` s für diese Felder. Für die Arbeit mit dem Bild und `BrochurePath` Eigenschaften, müssen wir zuerst sie zum Hinzufügen der `CategoriesDataTable` und aktualisieren Sie dann die `CategoriesTableAdapter` Klasse, um diese Spalten zurück.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Hinzufügen der`Picture`und`BrochurePath``DataColumn` s

Starten, indem Sie diese beiden Spalten zum Hinzufügen der `CategoriesDataTable`. Mit der rechten Maustaste auf die `CategoriesDataTable` s-Header, wählen Sie aus dem Kontextmenü hinzufügen, und wählen Sie dann die Column-Option. Dies erstellt eine neue `DataColumn` in der mit dem Namen "DataTable" `Column1`. Benennen Sie diese Spalte in `Picture`. Legen Sie im Fenster Eigenschaften die `DataColumn` s `DataType` Eigenschaft `System.Byte[]` (Dies ist eine Option in der Dropdown-Liste, müssen Sie sie eingeben).


[![Erstellen Sie eine DataColumn mit dem Namen Bild ist, deren Datentyp System.Byte]](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Abbildung 6**: Erstellen einer `DataColumn` benannte `Picture` , deren `DataType` ist `System.Byte[]` ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image8.png))


Fügen Sie einen anderen `DataColumn` der Datentabelle benennen `BrochurePath` unter Verwendung des standardmäßigen `DataType` Wert (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Zurückgeben der`Picture`und`BrochurePath`Werte aus dem TableAdapter

Mit diesen beiden `DataColumn` s hinzugefügt, um die `CategoriesDataTable`, wir re kann jetzt aktualisiert die `CategoriesTableAdapter`. Wir konnten beide Spaltenwerte in der Hauptabfrage des TableAdapter zurückgegeben haben, aber dies würde zurückholen die binären Daten jedes Mal die `GetCategories()` Methode wurde aufgerufen. Let s aktualisieren Sie stattdessen die Hauptabfrage TableAdapter wieder zurückbekommen `BrochurePath` , und erstellen Sie eine zusätzliche Daten-Abruf-Methode, die eine bestimmte Kategorie s zurückgibt `Picture` Spalte.

Um die Hauptabfrage des TableAdapter zu aktualisieren, mit der Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie aus dem Kontextmenü die Option konfigurieren. Daraufhin wird die Tabelle Konfigurationsassistenten für Datenadapter, die wir in einer Reihe von vergangenen Lernprogramme gesehen haben. Aktualisieren Sie die Abfrage aus, um wieder die `BrochurePath` , und klicken Sie auf ' Fertig stellen '.


[![Aktualisieren der Liste der Spalten in der SELECT-Anweisung auch BrochurePath zurückgeben](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Abbildung 7**: Aktualisieren der Liste der Spalten in der `SELECT` -Anweisung auch zurückgegeben `BrochurePath` ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image10.png))


Bei Verwendung der Ad-hoc-SQL-Anweisungen für die TableAdapter Aktualisieren der Liste der Spalten in der Hauptabfrage die Spaltenliste für alle updates der `SELECT` Abfragemethoden im TableAdapter. Bedeutet, dass die `GetCategoryByCategoryID(categoryID)` Methode wurde aktualisiert und Zurückgeben der `BrochurePath` Spalte, die möglicherweise beabsichtigt wir. Allerdings es auch in die Spaltenliste aktualisiert die `GetCategoriesAndNumberOfProducts()` -Methode, entfernen Sie die Unterabfrage, die die Anzahl der Produkte für die einzelnen Kategorien zurückgibt! Aus diesem Grund müssen wir diese Methode s aktualisieren `SELECT` Abfrage. Mit der rechten Maustaste auf die `GetCategoriesAndNumberOfProducts()` -Methode, wählen Sie konfigurieren und Zurücksetzen der `SELECT` Abfrage zurück an den ursprünglichen Wert:


[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

Als Nächstes erstellen Sie eine neue TableAdapter-Methode, die eine bestimmte Kategorie s zurückgibt `Picture` Spaltenwert. Mit der rechten Maustaste auf die `CategoriesTableAdapter` s-Header, und wählen Sie die Abfrage hinzufügen-Option aus, um die TableAdapter-Abfragekonfigurations-Assistent zu starten. Der erste Schritt des Assistenten werden wir gefragt, wenn Sie zum Abfragen von Daten eine Ad-hoc-SQL-Anweisung verwenden möchten, ein neues, gespeicherte Prozedur bzw. eine vorhandene. Wählen Sie die SQL-Anweisungen, und klicken Sie auf Weiter. Da es eine Zeile zurückgeben, wählen Sie wählen die Option Zeilen aus der zweite Schritt zurückgibt.


[![Wählen Sie die SQL-Anweisungen Option](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Abbildung 8**: Wählen Sie die SQL-Anweisungen Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image12.png))


[![Da die Abfrage einen Datensatz aus der Categories-Tabelle zurückgibt, wählen Sie wählen die Zeilen zurückgibt](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Abbildung 9**: Da der Abfrage einen Datensatz aus der Categories-Tabelle auswählen zurückgegeben werden, die Zeilen zurückgibt ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image14.png))


Geben Sie im dritten Schritt die folgende SQL-Abfrage, und klicken Sie auf Weiter:


[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

Der letzte Schritt besteht darin den Namen für die neue Methode auswählen. Verwendung `FillCategoryWithBinaryDataByCategoryID` und `GetCategoryWithBinaryDataByCategoryID` für das Füllen einer DataTable und der Rückgabewert eine "DataTable" patterns, bzw. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Wählen Sie den Namen für die TableAdapter-s-Methoden](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Abbildung 10**: Wählen Sie den Namen für die TableAdapter-Methoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image16.png))


> [!NOTE]
> Nach Abschluss der Tabelle Adapter Abfragekonfigurations-Assistent möglicherweise ein Dialogfeld darüber informiert, dass der neue Befehlstext Daten mit Schema aus dem Schema der Hauptabfrage unterschiedliche zurückgibt. Kurz gesagt, der Assistent ist beachten, dass der Hauptabfrage des TableAdapter s `GetCategories()` gibt ein anderes Schema als dem soeben erstellt wurde. Diese ist jedoch was wir möchten, damit Sie diese Meldung ignorieren können.


Darüber hinaus sollten Sie bedenken, wenn Sie Ad-hoc-SQL-Anweisungen verwenden und mit dem Assistenten zum Zeitpunkt die TableAdapter Hauptabfrage s zu einem späteren Zeitpunkt ändern, ändern wird der `GetCategoryWithBinaryDataByCategoryID` s-Methode `SELECT` Anweisung s Spaltenliste enthalten nur die Spalten aus der Hauptabfrage (d. h. er entfernt die `Picture` Spalte aus der Abfrage). Die Spaltenliste zurückzugebenden manuell aktualisieren müssen die `Picture` Spalte ähnelt, was haben wir mit der `GetCategoriesAndNumberOfProducts()` Methode weiter oben in diesem Schritt.

Nach dem Hinzufügen der beiden `DataColumn` s, um die `CategoriesDataTable` und `GetCategoryWithBinaryDataByCategoryID` Methode, um die `CategoriesTableAdapter`, sollte diese Klassen in der typisierten DataSet-Designer wie im Screenshot in Abbildung 11 aussehen.


![Der DataSet-Designer enthält die neue Spalten und -Methode](uploading-files-vb/_static/image11.gif)

**Abbildung 11**: der DataSet-Designer enthält die neue Spalten und -Methode


## <a name="updating-the-business-logic-layer-bll"></a>Aktualisieren die Logik der Geschäftsebene (BLL)

Mit der DAL aktualisiert, übrig bleibt Business Logic Layer (BLL) Einbeziehung eine Methode für die neue Erweiterung `CategoriesTableAdapter` Methode. Fügen Sie die folgende Methode, die `CategoriesBLL` Klasse:


[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Schritt 5: Hochladen einer Datei vom Client an den Webserver

Bei der binäre Daten zu erfassen und werden häufig durch einen Endbenutzer diese Daten bereitgestellt. Wenn diese Informationen erfassen möchten, muss der Benutzer eine Datei von ihrem Computer an den Webserver hochladen können. Die hochgeladenen Daten müssen mit dem Datenmodell integriert werden, wodurch das bedeuten, die Datei dass kann auf die Web Server s Dateisystem und Hinzufügen eines Pfads zur Datei in der Datenbank, oder schreiben den binären Inhalt direkt in der Datenbank speichern. In diesem Schritt untersuchen wir an, wie einen Benutzer zum Hochladen von Dateien von ihrem Computer an den Server zu ermöglichen. In den nächsten Lernprogrammen aktivieren wir unsere Aufmerksamkeit auf das Datenmodell die hochgeladene Datei integrieren.

ASP.NET 2.0 neue s [FileUpload-Websteuerelement](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) bietet einen Mechanismus für Benutzer, eine Datei von ihrem Computer an den Webserver zu senden. Das FileUpload-Steuerelement gerendert wird, als ein `<input>` Element, dessen `type` -Attribut auf die Datei, die in Browsern, wie ein Textfeld mit einer Schaltfläche "Durchsuchen angezeigt" festgelegt ist. Durch Klicken auf die Schaltfläche zum Durchsuchen wird ein Dialogfeld, in dem der Benutzer eine Datei auswählen kann. Wenn das Formular zurückgesendet wird, werden den Inhalt der ausgewählten Datei s sowie das Postback gesendet. Auf der Serverseite erfolgt Informationen über die hochgeladene Datei Zugriff über die FileUpload s Steuerelementeigenschaften.

Öffnen Sie zum Hochladen von Dateien zu veranschaulichen, die `FileUpload.aspx` auf der Seite der `BinaryData` Ordner ein FileUpload-Steuerelement aus der Toolbox in den Designer ziehen, und legen Sie das Steuerelement s `ID` Eigenschaft `UploadTest`. Als Nächstes fügen Sie eine Schaltfläche Websteuerelement Festlegen seiner `ID` und `Text` Eigenschaften `UploadButton` und die ausgewählte Datei bzw. hochladen. Setzen Sie schließlich ein Bezeichnung-Websteuerelement unterhalb der Schaltfläche Löschen seine `Text` Eigenschaft, und legen seine `ID` Eigenschaft `UploadDetails`.


[![Hinzufügen eines FileUpload-Steuerelements auf der ASP.NET-Seite](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Abbildung 12**: Hinzufügen eines FileUpload-Steuerelements auf der ASP.NET-Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image18.png))


Abbildung 13 zeigt diese Seite, wenn Sie über einen Browser angezeigt. Beachten Sie, dass Sie auf die Schaltfläche zum Durchsuchen von einem Dialogfeld zur Dateiauswahl, bringt Dadurch kann der Benutzer eine Datei von ihrem Computer auswählen. Nachdem eine Datei ausgewählt wurde, führt dazu, dass Sie auf die Schaltfläche für die ausgewählte Datei hochladen ein Postbacks, das die ausgewählte Datei s binären Inhalt an den Webserver sendet.


[![Der Benutzer kann eine Datei zum Hochladen von ihrem Computer an den Server auswählen.](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Abbildung 13**: der Benutzer kann eine Datei zum Hochladen von ihrem Computer an den Server auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image20.png))


Klicken Sie auf Postback handeln die hochgeladene Datei im Dateisystem gespeichert werden kann, oder die Binärdaten mit umgangen werden können, direkt über einen Stream. In diesem Beispiel können Sie s erstellen eine `~/Brochures` Ordner, und speichern Sie die hochgeladene Datei. Starten Sie durch Hinzufügen der `Brochures` Ordner an den Standort als Unterordner des Stammverzeichnisses. Als Nächstes erstellen Sie einen Ereignishandler für das `UploadButton` s `Click` Ereignis und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

Das Steuerelement FileUpload bietet eine Vielzahl von Eigenschaften für die Arbeit mit der hochgeladenen Daten. Für die Instanz, die [ `HasFile` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) gibt an, ob eine Datei, durch den Benutzer hochgeladen wurde während der [ `FileBytes` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) ermöglicht den Zugriff auf die hochgeladenen Binärdaten als Array von Bytes. Die `Click` Ereignishandler gestartet wird, indem Sie sicherstellen, dass eine Datei hochgeladen wurde. Wenn eine Datei hochgeladen wurde, zeigt die Bezeichnung der Name der hochgeladenen Datei, seiner Größe in Bytes und dem Inhaltstyp an.

> [!NOTE]
> Um sicherzustellen, dass der Benutzer eine Datei hochlädt, sehen Sie sich, die `HasFile` Eigenschaft und eine Warnmeldung angezeigt, wenn es s `False`, oder Sie können die [RequiredFieldValidator-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) stattdessen.


Die FileUpload s `SaveAs(filePath)` speichert die hochgeladene Datei im angegebenen *FilePath*. *FilePath* muss ein *physischen Pfad* (`C:\Websites\Brochures\SomeFile.pdf`) anstelle eines *virtuellen* *Pfad* (`/Brochures/SomeFile.pdf`). Die [ `Server.MapPath(virtPath)` Methode](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) akzeptiert einen virtuellen Pfad und den entsprechenden physikalischen Pfad zurückgegeben. Hier wird der virtuelle Pfad `~/Brochures/fileName`, wobei *FileName* ist der Name der hochgeladenen Datei. Finden Sie unter [mithilfe Server.MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) für Weitere Informationen zum virtuellen und physischen Pfade und Verwenden von `Server.MapPath`.

Nach Abschluss der `Click` -Ereignishandler können Sie die Seite in einem Browser zu testen. Klicken Sie auf die Schaltfläche zum Durchsuchen, und wählen Sie eine Datei von der Festplatte, und klicken Sie dann auf die Schaltfläche "ausgewählte Datei hochladen". Das Postback sendet den Inhalt der ausgewählten Datei an den Webserver, der dann Informationen zur Datei, und speichern Sie es angezeigt wird der `~/Brochures` Ordner. Nach dem Hochladen der das, kehren Sie zu Visual Studio zurück, und klicken Sie auf die Schaltfläche "Aktualisieren" im Projektmappen-Explorer. Sie sollte die Datei, die Sie soeben im Ordner "~/Brochures" hochgeladen.


[![Die Datei EvolutionValley.jpg wurde an den Webserver hochgeladen.](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Abbildung 14**: die Datei `EvolutionValley.jpg` hochgeladen wurde an den Webserver ([klicken Sie hier, um das Bild in voller Größe angezeigt](uploading-files-vb/_static/image22.png))


![EvolutionValley.jpg wurde der Ordner "~/Brochures" gespeichert.](uploading-files-vb/_static/image15.gif)

**Abbildung 15**: `EvolutionValley.jpg` gespeichert wurde, um die `~/Brochures` Ordner


## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Besonderheiten beim Speichern von hochgeladener Dateien im Dateisystem

Es gibt einige Besonderheiten, die behoben werden müssen, beim Hochladen von Dateien die Web Server s im Dateisystem speichern. Zunächst wird dort s das Problem der Sicherheit. Um eine Datei im Dateisystem zu speichern, muss der Sicherheitskontext an, unter dem die ASP.NET-Seite ausgeführt wird, über Schreibberechtigungen verfügen. Der ASP.NET Development Web Server im Kontext Ihres aktuellen Benutzerkontos ausgeführt. Bei Verwendung von Microsoft-s (Internet Information Services, IIS) wie der Webserver hängt von der Sicherheitskontext Version von IIS und seiner Konfiguration.

Eine weitere Herausforderung Dateien im Dateisystem speichern Funktionstüchtigkeit benennen die Dateien. Derzeit unserer Seite speichert alle hochgeladenen Dateien, die `~/Brochures` Verzeichnis mit dem gleichen Namen wie die Datei auf dem Clientcomputer s verfügen. Wenn Benutzer A eine Broschüren mit dem Namen hochlädt `Brochure.pdf`, wird die Datei gespeichert werden, als `~/Brochure/Brochure.pdf`. Aber was geschieht, wenn einige Zeit höher User B eine unterschiedliche Broschüren-Datei hochgeladen wird, die zufällig zur gleiche Dateinamen haben (`Brochure.pdf`)? Durch den Code haben wir nun, Benutzer, die mit User B s Upload eine s-Datei überschrieben wird.

Es gibt diverse Techniken zum Lösen von Namenskonflikten. Eine Möglichkeit besteht darin verbieten Hochladen einer Datei, wenn eine mit dem gleichen Namen bereits vorhanden. Bei diesem Ansatz, wenn Benutzer B versucht zum Hochladen einer Datei mit dem Namen `Brochure.pdf`, würde das System nicht speichern Sie ihre Datei und zeigt stattdessen eine Meldung User B, um die Datei umbenennen und versuchen Sie es erneut. Speichern Sie die Datei mit einem eindeutigen Dateinamen, die sein könnte ein anderer Ansatz ist eine [global eindeutigen Bezeichner (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) oder den Wert aus der entsprechenden Datenbank-Datensatz s Primärschlüsselspalte(n) (vorausgesetzt, dass der Upload zugeordnet ist ein bestimmte Zeile in das Datenmodell). In den nächsten Lernprogrammen werden diese Optionen ausführlich untersucht.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Herausforderungen bei der Platzierung große Mengen von Binärdaten

Diese Lernprogramme setzen voraus, dass die binären Daten erfasst Größe ist. Arbeiten mit sehr großen Datenmengen binärdatendateien gespeichert, die mehrere MB oder größer stellt neue Herausforderung dar, die Gegenstand des diese Lernprogramme sind. Beispielsweise standardmäßig ASP.NET lehnt Uploads von mehr als 4 MB, obwohl dies über konfiguriert werden, kann die [ `<httpRuntime>` Element](https://msdn.microsoft.com/library/e1f13641.aspx) in `Web.config`. IIS erzwingt einen eigenen größenbeschränkungen der Datei hochladen zu. Finden Sie unter [IIS hochladen Dateigröße](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) für Weitere Informationen. Darüber hinaus kann die Zeit zum Hochladen großer Dateien standardmäßig 110 Seconds übersteigen, die ASP.NET für eine Anforderung gewartet wird. Es gibt auch Arbeitsspeicher- und Leistungsproblemen Probleme, die beim Arbeiten mit großen Dateien auftreten.

Das FileUpload-Steuerelement ist für große Dateiuploads alleine nicht durchführbar. Wie der Inhalt der Datei s an den Server gesendet werden, muss der Endbenutzer geduldig ohne eine Bestätigung warten Sie, dass ihre Upload voranschreitet. Dies ist nicht so viele ein Problem beim Umgang mit kleinere Dateien, die in ein paar Sekunden hochgeladen werden können, kann jedoch ein Problem beim Umgang mit großen Dateien, die zum Hochladen Minuten dauern. Es gibt eine Vielzahl von Drittanbieter-Datei hochladen-Steuerelemente, die für die Behandlung von großen Uploads besser geeignet sind, und viele von diesen Anbietern bieten Statusanzeigen und ActiveX-Manager hochladen, die eine weitaus ansprechende Benutzeroberfläche darstellen.

Wenn die Anwendung große Dateien verarbeiten muss, müssen Sie sorgfältig untersuchen die Herausforderungen und finden Sie geeignete Lösungen für Ihren jeweiligen Anforderungen.

## <a name="summary"></a>Zusammenfassung

Erstellen einer Anwendung, die zum Aufzeichnen von binären Daten benötigt, führt eine Reihe von Herausforderungen. In diesem Lernprogramm wir die ersten beiden untersucht: entscheiden, wo die binären Daten gespeichert und ermöglichen einen Benutzer zum Hochladen von Inhalt im Binärformat über eine Webseite. Über die nächsten drei Lernprogramme sehen wir, wie die hochgeladenen Daten mit einem Datensatz in der Datenbank zugeordnet und wie die Binärdaten zusammen mit deren Textdatenfelder angezeigt.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Verwenden von Datentypen mit umfangreichen Werten](https://msdn.microsoft.com/library/ms178158.aspx)
- [FileUpload-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Die ASP.NET 2.0 FileUpload-Webserversteuerelement](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [Die dunkle Seite des Dateiuploads](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und Bernadette Leigh. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](updating-and-deleting-existing-binary-data-cs.md)
> [Weiter](displaying-binary-data-in-the-data-web-controls-vb.md)
