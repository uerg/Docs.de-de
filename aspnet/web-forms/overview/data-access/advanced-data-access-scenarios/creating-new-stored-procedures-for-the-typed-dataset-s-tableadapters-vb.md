---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Erstellen von neuen gespeicherten Prozeduren für TableAdapter-Steuerelemente des typisierten DataSet (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In den vorherigen Tutorials haben wir die SQL-Anweisungen in unserem Code erstellt und übergeben die Anweisungen in der Datenbank ausgeführt werden. Ein alternativer Ansatz ist die Verwendung von s...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 601883ea0e0bbea08e564d77eea5ce7d19956cb7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829612"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Erstellen von neuen gespeicherten Prozeduren für TableAdapter-Steuerelemente des typisierten DataSet (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) oder [PDF-Datei herunterladen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> In den vorherigen Tutorials haben wir die SQL-Anweisungen in unserem Code erstellt und übergeben die Anweisungen in der Datenbank ausgeführt werden. Ein alternativer Ansatz ist, können gespeicherte Prozeduren verwenden, wenn die SQL-Anweisungen in der Datenbank vorab definiert. In diesem Tutorial erfahren wir, wie den TableAdapter-Assistenten generiert neue gespeicherte Prozeduren für uns zu haben.


## <a name="introduction"></a>Einführung

Der Data Access Layer (DAL) für diese Tutorials wird typisierter DataSets verwendet. Siehe die [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Tutorial typisierte DataSets bestehen aus stark typisierten DataTables und TableAdapters. Die DataTables stellen die logischen Entitäten dar, in das System während der TableAdapter-Schnittstelle, mit der zugrunde liegenden Datenbank, die Zugriff auf Ihre Daten ausführen. Dies schließt die Datentabellen mit Daten auffüllen, Ausführen von Abfragen, die skalare Daten zurückgeben und einfügen, aktualisieren und Löschen von Datensätzen aus der Datenbank.

Die SQL-Befehle ausgeführt, indem die TableAdapters werden entweder Ad-hoc-SQL-Anweisungen, wie z. B. `SELECT columnList FROM TableName`, oder gespeicherten Prozeduren. Die TableAdapters in unserer Architektur verwenden Sie Ad-hoc-SQL-Anweisungen. Viele Entwickler und Datenbankadministratoren, allerdings bevorzugen gespeicherte Prozeduren über Ad-hoc-SQL-Anweisungen, Sicherheit, wartbarkeit und aktualisierbarkeit Gründen. Andere Benutzer bevorzugen ardently Ad-hoc-SQL-Anweisungen, um ihre Flexibilität zu erzielen. In meiner eigenen Arbeit ich gespeicherte Prozeduren über Ad-hoc-SQL-Anweisungen bevorzugen allerdings möchten die Ad-hoc-SQL-Anweisungen zu verwenden, um den früheren Tutorials zu vereinfachen.

Wenn erleichtert einen TableAdapter definieren oder Hinzufügen neuer Methoden, die TableAdapter-s-Assistenten genauso einfach neue gespeicherte Prozeduren erstellen oder vorhandene gespeicherte Prozeduren verwenden, wie es um die Ad-hoc-SQL-Anweisungen verwenden. In diesem Tutorial betrachten wir, wie der TableAdapter-s-Assistent gespeicherte Prozeduren automatisch generiert werden. Im nächsten Tutorial werden wir konfigurieren die TableAdapter-s-Methoden, um vorhandene oder manuell erstellte gespeicherte Prozeduren verwenden betrachten.

> [!NOTE]
> Finden Sie unter Rob Howard Blogeintrag [Don ' t mit gespeicherten Prozeduren noch?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) und [Frans Bouma](https://weblogs.asp.net/fbouma/) s Blogeintrag [gespeicherte Prozeduren sind schlecht, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) für eine lebhafte Diskussion über die vor- und Nachteile von gespeicherte Prozeduren und Ad-hoc-SQL.


## <a name="stored-procedure-basics"></a>Grundlagen der gespeicherten Prozeduren

Funktionen sind ein Konstrukt, das für alle Programmiersprachen. Eine Funktion ist eine Sammlung von Anweisungen, die ausgeführt werden, wenn die Funktion aufgerufen wird. Funktionen können annehmen von Eingabeparametern und können optional einen Wert zurückgeben. *[Gespeicherte Prozeduren](http://en.wikipedia.org/wiki/Stored_procedure)*  Datenbankkonstrukte, die viele ähnlichkeiten mit Funktionen in Programmiersprachen zu vergleichen gemeinsam sind. Eine gespeicherte Prozedur einen Satz von T-SQL-Anweisungen besteht, die ausgeführt werden, wenn die gespeicherte Prozedur aufgerufen wird. Eine gespeicherte Prozedur kann 0 (null), zu viele Eingabeparameter akzeptieren und kann Skalare Werte als auch Ausgabeparameter zurückgeben oder, in den meisten Fällen-Resultsets aus `SELECT` Abfragen.

> [!NOTE]
> Gespeicherte Prozeduren werden häufig als gespeicherte Prozeduren oder SPs bezeichnet.


Gespeicherte Prozeduren werden erstellt, mit der [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL-Anweisung. Das folgende T-SQL-Skript erstellt z. B. eine gespeicherte Prozedur namens `GetProductsByCategoryID` , die in einem einzelnen Parameter mit dem Namen akzeptiert `@CategoryID` und gibt die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Felder dieser Spalten in der `Products` Tabelle, die über eine entsprechende `CategoryID` Wert:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Nachdem diese gespeicherte Prozedur erstellt wurde, kann sie mithilfe der folgenden Syntax aufgerufen werden:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Im nächsten Tutorial untersuchen wir das Erstellen von gespeicherten Prozeduren über die Visual Studio-IDE. In diesem Tutorial werden jedoch wir den TableAdapter-Assistenten, die die gespeicherten Prozeduren automatisch für uns generieren zu lassen.


Zusätzlich zum Zurückgeben von Daten einfach, werden gespeicherte Prozeduren häufig verwendet, um mehrere Datenbankbefehle innerhalb des Bereichs einer einzelnen Transaktion auszuführen. Eine gespeicherte Prozedur namens `DeleteCategory`, z. B. dauert eine `@CategoryID` Parameter und führen Sie zwei `DELETE` Anweisungen: zuerst eine zum Löschen von verwandten Produkten und ein zweites ein Löschen der angegebenen Kategorie. Mehrere Anweisungen innerhalb einer gespeicherten Prozedur werden *nicht* automatisch umbrochenen innerhalb einer Transaktion. Weitere T-SQL-Befehle ausgegeben werden, um sicherzustellen, dass die gespeicherte Prozedur s, die mehrere Befehle als atomischen Vorgang behandelt werden müssen. Wir sehen, wie Sie eine gespeicherte Prozedur-s-Befehle innerhalb des Bereichs einer Transaktion in nachfolgenden Tutorial zu umschließen.

Wenn Sie gespeicherte Prozeduren in einer Architektur verwenden, rufen die Data Access Layer-s-Methoden, eine Ad-hoc-SQL-Anweisung auszugeben, anstatt eine bestimmte gespeicherte Prozedur. Auf diese Weise den Speicherort der SQL-Anweisungen ausgeführt (in der Datenbank) zentralisiert, anstatt es in die-s-Anwendungsarchitektur definiert. Diese Zentralisierung wohl suchen, analysieren und Optimieren der Abfragen erleichtert und bietet ein besseres Verständnis, wo und wie die Datenbank verwendet wird.

Weitere Informationen zu den Grundlagen der gespeicherten Prozedur finden Sie in den Ressourcen im Abschnitt Weitere nützliche Informationen am Ende dieses Tutorials.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Schritt 1: Erstellen, die erweiterte Data Access Layer Szenarien-Webseiten

Bevor wir unsere Diskussion zum Erstellen einer DAL, die mithilfe von gespeicherten Prozeduren beginnen, können Sie s zuerst können Sie die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für diese und die nächste mehrere Tutorials benötigen. Starten, indem Sie einen neuen Ordner namens hinzufügen `AdvancedDAL`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, um sicherzustellen, ordnen Sie jeder Seite mit den `Site.master` Masterseite:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Fügen Sie die ASP.NET-Seiten für die erweiterte Data Access Layer Szenarien-Lernprogramme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die erweiterte Data Access Layer Szenarien-Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in die `AdvancedDAL` Ordner werden in den Tutorials im Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund fügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Fügen Sie das SectionLevelTutorialListing.ascx-Benutzersteuerelement an "default.aspx"](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Abschließend fügen Sie diese Seiten als Einträge der `Web.sitemap` Datei. Fügen Sie das folgende Markup insbesondere nach dem Arbeiten mit Daten in einem Batch verarbeitet `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Nach der Aktualisierung `Web.sitemap`, können Sie die Lernprogramme-Website über einen Browser anzeigen. Klicken Sie im Menü auf der linken Seite enthält jetzt Elemente für die Tutorials für fortgeschrittene DAL-Szenarien.


![Die Sitemap enthält jetzt die Einträge für die erweiterte DAL-Szenarien-Lernprogramme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Abbildung 3**: die Sitemap enthält jetzt Einträge für die erweiterte DAL-Szenarien-Lernprogramme


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Schritt 2: Konfigurieren ein TableAdapter zum Erstellen von neuen gespeicherten Prozeduren

Zum Demonstrieren der Erstellung einer Datenzugriffsschicht, die gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen verwendet, können s erstellen Sie ein neues typisierte DataSet in den `~/App_Code/DAL` Ordner mit dem Namen `NorthwindWithSprocs.xsd`. Da wir diesen Prozess im Detail in vorherigen Tutorials schrittweise durchlaufen haben, wird durch den folgenden Schritten schnell fortfahren. Wenn Sie Probleme oder benötigen weitere schrittweise Anweisungen erstellen und Konfigurieren eines typisierten Datasets, verweisen zurück auf die [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-vb.md) Tutorial.

Fügen Sie ein neues DataSet für das Projekt mit der rechten Maustaste auf die `DAL` Ordner, wählen neues Element hinzufügen und die DataSet-Vorlage auswählen, wie in Abbildung 4 dargestellt.


[![Fügen Sie ein neues typisiertes DataSet, auf das Projekt mit dem Namen NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Abbildung 4**: Hinzufügen eines neuen typisierten Datasets, auf das Projekt mit dem Namen `NorthwindWithSprocs.xsd` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Dies wird das neue typisierte DataSet zu erstellen, öffnen Sie den Designer, erstellen einen neuen TableAdapter und den TableAdapter-Konfigurations-Assistenten zu starten. Der erste Schritt des TableAdapter-Konfigurations-Assistenten-s fragt, arbeiten mit die Datenbank auszuwählen. Die Verbindungszeichenfolge zur Northwind-Datenbank sollten in der Dropdown-Liste aufgeführt werden. Wählen Sie diese Option aus, und klicken Sie auf Weiter.

Auf diesem Bildschirm weiter können wir, wie der TableAdapter auf die Datenbank zugreifen muss. In vorherigen Tutorials haben wir uns für die erste Option, die SQL-Anweisungen. Für dieses Lernprogramm die zweite Option auswählen, neue gespeicherte Prozeduren erstellen, und klicken Sie auf Weiter.


[![Weisen Sie die TableAdpater zum Erstellen neuer gespeicherter Prozeduren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Abbildung 5**: Weisen Sie die TableAdpater, erstellen Sie neue gespeicherte Prozeduren ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Genau wie bei der Verwendung von Ad-hoc-SQL-Anweisungen, in den folgenden Schritt wir aufgefordert werden, geben Sie die `SELECT` -Anweisung für die Hauptabfrage des TableAdapter s. Aber statt der `SELECT` Anweisung hier eingegeben werden, um eine Ad-hoc-Abfragen direkt ausführen, in der TableAdapter-s-Assistent erstellt eine gespeicherte Prozedur, der diesen `SELECT` Abfrage.

Verwenden Sie die folgenden `SELECT` Abfrage für diese TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Geben Sie die SELECT-Abfrage](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Abbildung 6**: Geben Sie die `SELECT` Abfrage ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> Die obige Abfrage unterscheidet sich geringfügig vom die Hauptabfrage der `ProductsTableAdapter` in die `Northwind` typisierte DataSet. Bedenken Sie, dass die `ProductsTableAdapter` in die `Northwind` typisierte DataSet enthält zwei abhängige Unterabfragen, um wieder den Kategorienamen und den Firmennamen für jede Produktkategorie s und Lieferanten zu versetzen. Im kommenden [Aktualisieren des TableAdapters, verknüpft mit](updating-the-tableadapter-to-use-joins-vb.md) Tutorial betrachten wir das Hinzufügen dieser verwandter Daten dieses TableAdapter.


Nehmen Sie einen Moment Zeit, auf die Schaltfläche "Erweiterte Optionen" klicken. Von hier aus können wir angeben, ob der Assistent auch generieren soll INSERT-, Update- und Delete-Anweisungen für den TableAdapter, ob eine vollständige Parallelität verwendet, und gibt an, ob die Datentabelle nach Einfüge- und updatevorgängen aktualisiert werden sollen. Die generieren Insert, Update und Delete-Anweisungen-Option ist standardmäßig aktiviert. Lassen Sie ausgecheckt werden soll. Für dieses Tutorial für lassen Sie die Verwendung optimistischer Parallelitätsoptionen deaktiviert.

Wenn die gespeicherten Prozeduren, die von den TableAdapter-Assistenten automatisch erstellt, wird es angezeigt, dass die Aktualisierung der Daten-Tabellenoption ignoriert wird. Unabhängig davon, ob dieses Kontrollkästchen aktiviert ist, die sich ergebende INSERT- und Update abgerufen werden gespeicherte Prozeduren den gerade eingefügten oder nur aktualisierte Datensatz, wie wir in Schritt 3 angezeigt wird.


![Lassen Sie die Option aktiviert generieren Insert, Update und Delete-Anweisungen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Abbildung 7**: lassen Sie die Option aktiviert generieren Insert, Update und Delete-Anweisungen


> [!NOTE]
> Wenn die Verwendung optimistischer Parallelität-Option aktiviert ist, wird der Assistent zusätzliche Bedingungen zum Hinzufügen der `WHERE` -Klausel, die verhindern, dass die Daten aktualisiert wird, wenn Änderungen in anderen Feldern vorgenommen wurden. Verweisen zurück auf die [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) Tutorial Weitere Informationen zur Verwendung der TableAdapter s integrierte optimistische Parallelität Funktion.


Nach dem Eingeben der `SELECT` abzufragen und die Bestätigung, dass die generieren Insert, Update und Delete-Anweisungen-Option aktiviert ist, und klicken Sie auf Weiter. Die Namen der gespeicherten Prozeduren, die der Assistent erstellt für Sie auswählen, einfügen, aktualisieren und Löschen von Daten aufgefordert, diesen nächsten Bildschirm in Abbildung 8 dargestellt. Änderungen, die diese gespeicherten Prozeduren Objektnamen `Products_Select`, `Products_Insert`, `Products_Update`, und `Products_Delete`.


[![Benennen Sie die gespeicherten Prozeduren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Abbildung 8**: Benennen Sie gespeicherte Prozeduren ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Um das T-SQL finden Sie in der TableAdapter-Assistenten zum Erstellen von vier gespeicherten Prozeduren verwenden, klicken Sie auf die Schaltfläche "SQL-Skriptvorschau anzeigen". Im Dialogfeld "SQL-Skriptvorschau anzeigen" können Sie das Skript in einer Datei speichern oder in die Zwischenablage kopieren.


![Vorschau der SQL-Skripts verwendet, um die gespeicherten Prozeduren generieren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Abbildung 9**: Vorschau der SQL-Skript verwendet wird, um die gespeicherten Prozeduren generieren


Nach dem Benennen der gespeicherten Prozeduren ein, klicken Sie neben dem entsprechenden Namen die TableAdapter-Methoden. Vergleichbar mit dem bei der Ad-hoc-SQL-Anweisungen verwenden können wir Methoden erstellen, die eine vorhandene DataTable füllen, oder geben Sie einen neuen zurück. Wir können auch angeben, ob der TableAdapter das DB-Direct-Muster für das Einfügen, aktualisieren und Löschen von Datensätzen enthalten soll. Lassen Sie alle drei Kontrollkästchen aktiviert, aber benennen Sie eine DataTable-Methode, um der Rückgabe `GetProducts` (wie in Abbildung 10 gezeigt).


[![Benennen Sie die Methoden Füllung und GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Abbildung 10**: Benennen Sie die Methoden `Fill` und `GetProducts` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Klicken Sie auf Weiter, um eine Zusammenfassung der Schritte finden Sie unter, die der Assistent ausführt. Schließen Sie den Assistenten, indem Sie auf die Schaltfläche "Fertig stellen". Nachdem der Assistent abgeschlossen ist, werden Sie auf der DataSet-s-Designers, der jetzt enthalten soll zurückgegeben werden die `ProductsDataTable`.


[![Der DataSet-s-Designer zeigt die neu hinzugefügte ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Abbildung 11**: der DataSet-s-Designer zeigt die neu hinzugefügte `ProductsDataTable` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Schritt 3: Untersuchen der neu erstellten gespeicherten Prozeduren

Die TableAdapter-Assistenten automatisch verwendet, die in Schritt2 erstellt die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren und Löschen von Daten. Diese gespeicherten Prozeduren angezeigt, oder über Visual Studio im Server-Explorer und Drilldown in den Ordner der Datenbank gespeicherte Prozeduren geändert werden können. Wie in Abbildung 12 gezeigt, handelt es sich bei der Datenbank Northwind enthält vier neue gespeicherte Prozeduren: `Products_Delete`, `Products_Insert`, `Products_Select`, und `Products_Update`.


![Die vier gespeicherten Prozeduren, die in Schritt 2 erstellten finden Sie im Ordner "s-Datenbank gespeicherte Prozeduren"](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Abbildung 12**: die vier gespeicherten Prozeduren, die in Schritt 2 erstellten finden Sie im Ordner "s-Datenbank gespeicherte Prozeduren"


> [!NOTE]
> Wenn Sie den Server-Explorer nicht angezeigt werden, finden Sie unter dem Menü "Ansicht", und wählen Sie die Server-Explorer-Option. Wenn Sie nicht die produktbezogene gespeicherten Prozeduren hinzugefügt, die aus Schritt2 angezeigt werden, aktualisieren, versuchen Sie es mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen.


Zum Anzeigen oder Ändern einer gespeicherten Prozedur, doppelklicken Sie auf den Namen im Server-Explorer oder alternativ mit der rechten Maustaste auf die gespeicherte Prozedur, und wählen Sie öffnen. Abbildung 13 zeigt die `Products_Delete` gespeicherte Prozedur aus, wenn geöffnet.


[![Gespeicherte Prozeduren können geöffnet und in Visual Studio geändert werden](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Abbildung 13**: gespeicherte Prozeduren können geöffnet und geändert aus in Visual Studio ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Der Inhalt sowohl die `Products_Delete` und `Products_Select` gespeicherte Prozeduren sind ziemlich einfach. Die `Products_Insert` und `Products_Update` gespeicherte Prozeduren, auf der anderen Seite rechtfertigen eine Prüfung beide beim Ausführen einer `SELECT` Anweisung nach ihren `INSERT` und `UPDATE` Anweisungen. Z. B. die folgende SQL-Anweisung bildet die `Products_Insert` gespeicherte Prozedur:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Die gespeicherte Prozedur als Eingabeparameter akzeptiert die `Products` Spalten, die von zurückgegeben wurden die `SELECT` Abfrage, die in den TableAdapter-s-Assistenten und diese Werte werden verwendet, eine `INSERT` Anweisung. Nach der `INSERT` -Anweisung eine `SELECT` Abfrage dient zum Zurückgeben der `Products` Spaltenwerte (einschließlich der `ProductID`) des neu hinzugefügten Datensatzes. Diese Aktualisierung-Funktion ist nützlich, beim Hinzufügen eines neuen Datensatzes, der mit dem BatchUpdate-Muster somit automatisch die neu hinzugefügte aktualisiert `ProductRow` Instanzen `ProductID` Eigenschaften mit dem automatisch inkrementierte Werte, die von der Datenbank zugewiesen.

Der folgende Code veranschaulicht diese Funktion. Er enthält eine `ProductsTableAdapter` und `ProductsDataTable` erstellt, die für die `NorthwindWithSprocs` typisierte DataSet. Ein neues Produkt wird in der Datenbank hinzugefügt, durch das Erstellen einer `ProductsRow` Instanz, die Werte angeben und die TableAdapter aufrufen `Update` Methode und übergeben die `ProductsDataTable`. Intern die TableAdapter `Update` Methode listet den `ProductsRow` Instanzen in der übergebenen "DataTable" (in diesem Beispiel wird nur ein – eine, die wir gerade hinzugefügt haben) und führt die entsprechende einfügen, aktualisieren oder delete-Befehl. In diesem Fall die `Products_Insert` gespeicherte Prozedur ausgeführt wird, wird die fügt eines neuen Datensatzes in die `Products` Tabelle, und gibt die Details des neu hinzugefügten Datensatzes zurück. Die `ProductsRow` s-Instanz `ProductID` Wert wird dann aktualisiert. Nach der `Update` -Methode abgeschlossen wurde, können wir auf die neu hinzugefügte Datensatz s zugreifen `ProductID` Wert über die `ProductsRow` s `ProductID` Eigenschaft.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

Die `Products_Update` gespeicherte Prozedur auf ähnliche Weise umfasst eine `SELECT` Anweisung nach der `UPDATE` Anweisung.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Beachten Sie, dass diese gespeicherte Prozedur enthält zwei Eingabeparameter für `ProductID`: `@Original_ProductID` und `@ProductID`. Diese Funktion ermöglicht Szenarien, in dem der primäre Schlüssel geändert werden kann. Beispielsweise kann jedes Mitarbeiterdatensatz in einer Mitarbeiterdatenbank, die Mitarbeiter-s-Sozialversicherungsnummer als ihres Primärschlüssels erstellt verwenden. Um eine Sozialversicherungsnummer der vorhandenen Mitarbeiter s zu ändern, müssen sowohl die neuen Sozialversicherungsnummer und der ursprünglichen angegeben werden. Für die `Products` Tabelle, eine solche Funktion ist nicht erforderlich, da die `ProductID` Spalte ist eine `IDENTITY` Spalte und sollte nie geändert werden. In der Tat die `UPDATE` -Anweisung in der `Products_Update` gespeicherten Prozedur t enthalten die `ProductID` Spalte in seiner Spaltenliste. Daher zwar `@Original_ProductID` werden in der `UPDATE` Anweisung s `WHERE` -Klausel, es ist überflüssig, für die `Products` -Tabelle und könnte durch ersetzt werden die `@ProductID` Parameter. Beim Ändern einer s-Parameter von gespeicherten Prozeduren ist es wichtig, dass die TableAdapter-Methoden, die die gespeicherte Prozedur verwenden, ebenfalls aktualisiert werden.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Schritt 4: Ändern von Parametern für eine gespeicherte Prozedur s, und Aktualisieren der TableAdapter-Steuerelements

Da die `@Original_ProductID` -Parameter überflüssig ist, können Sie s entfernen Sie sie aus der `Products_Update` vollständig gespeicherten Prozedur. Öffnen der `Products_Update` gespeicherte Prozedur Löschen der `@Original_ProductID` Parameter, und aktivieren Sie in der `WHERE` -Klausel der `UPDATE` -Anweisung, Änderung, die den Namen des Parameters verwendet `@Original_ProductID` zu `@ProductID`. Nach diesen Änderungen sollte das T-SQL in der gespeicherten Prozedur wie folgt aussehen:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Um diese Änderungen in der Datenbank zu speichern, klicken Sie auf das Symbol "Speichern" in der Symbolleiste, oder drücken Sie STRG + S aus. An diesem Punkt die `Products_Update` gespeicherte Prozedur wird kein erwartet eine `@Original_ProductID` Eingabeparameter, aber der TableAdapter ist so konfiguriert, dass um diese einen Parameter zu übergeben. Sehen Sie die Parameter, die der TableAdapter sendet an die `Products_Update` die gespeicherte Prozedur den TableAdapter im DataSet-Designer auswählen, soll das Fenster "Eigenschaften" und klicken auf die Auslassungspunkte in der `UpdateCommand` s `Parameters` Auflistung. Dadurch wird das Dialogfeld Parametersammlungs-Editor in Abbildung 14 dargestellt.


![Die Parameter-Auflistungs-Editor Listen die Parameter verwendet, die an die Products_Update gespeicherten Prozedur](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Abbildung 14**: die Parameter-Auflistungs-Editor Listen die Parameter verwendet, die an die `Products_Update` gespeicherten Prozedur


Sie können diesen Parameter hier entfernen, wählen Sie einfach die `@Original_ProductID` Parameter aus der Liste der Elemente, und klicken Sie auf die Schaltfläche "entfernen".

Alternativ können Sie die Parameter für alle Methoden verwendet werden, indem mit der rechten Maustaste auf den TableAdapter im Designer und konfigurieren aktualisieren. Hierdurch wird der TableAdapter-Konfigurations-Assistent, die die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren, auflisten und löschen, zusammen mit den Parametern die gespeicherten Prozeduren erwarten. Wenn Sie auf der Update-Dropdown-Liste klicken, sehen Sie die `Products_Update` gespeicherte Prozeduren erwartet Eingabeparameter, darunter jetzt nicht mehr `@Original_ProductID` (siehe Abbildung 15). Klicken Sie einfach auf "Fertig stellen", um die Parameterauflistung, die von der TableAdapter verwendet automatisch zu aktualisieren.


[![Sie können auch können den TableAdapter-s-Konfigurations-Assistenten Sie seine Methoden Parameter Sammlungen aktualisieren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Abbildung 15**: können Sie alternativ die TableAdapter-Konfigurations-Assistenten zum Aktualisieren von seine Methoden Parameterauflistungen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Schritt 5: Hinzufügen von zusätzlichen TableAdapter-Methoden

Schritt 2 ist dargestellt ist beim Erstellen eines neuen TableAdapter es einfach, die entsprechenden gespeicherten Prozeduren automatisch generiert haben. Dasselbe gilt beim Hinzufügen von zusätzlicher Methods zu einem TableAdapter. Um dies zu veranschaulichen, können Sie s hinzufügen eine `GetProductByProductID(productID)` Methode, um die `ProductsTableAdapter` in Schritt2 erstellt haben. Diese Methode benötigt als Eingabe eine `ProductID` Wert und Zurückgeben von Details zu das angegebene Produkt.

Beginnen Sie mit der rechten Maustaste auf den TableAdapter, und wählen im Kontextmenü der Abfrage hinzufügen.


![Eine neue Abfrage wird dem TableAdapter hinzufügen.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Abbildung 16**: eine neue Abfrage wird dem TableAdapter hinzufügen.


Dies wird im TableAdapter-Abfrage-Konfigurations-Assistenten gestartet, der zuerst aufgefordert, wie der TableAdapter auf die Datenbank zugreifen muss. Um eine neue gespeicherte Prozedur erstellt haben, wählen Sie erstellen eine neue gespeicherte Prozedur aus, und klicken Sie auf Weiter.


[![Wählen Sie dem Erstellen einer neuen gespeicherten Prozedur Option](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Abbildung 17**: Wählen Sie dem Erstellen eine neue gespeicherte Prozedur Option ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


Im nächste Bildschirm fordert uns identifiziert den Typ der Abfrage aus, ob sie eine Gruppe von Zeilen oder einen einzelnen Skalarwert zurückgeben wird, oder führen Sie an, eine `UPDATE`, `INSERT`, oder `DELETE` Anweisung. Da die `GetProductByProductID(productID)` Methode wird eine Zeile zurück, die zurückgibt, Row-Option ausgewählt ist, und drücken Sie die nächsten auswählen lassen.


[![Wählen Sie die wählen die Option-Zeile zurückgibt.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Abbildung 18**: Wählen Sie die wählen die Option-Zeile zurückgibt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


Der nächste Bildschirm zeigt an, die TableAdapter-s-Haupt-Abfrage, die den Namen der gespeicherten Prozedur aufgeführt (`dbo.Products_Select`). Ersetzen Sie den Namen der gespeicherten Prozedur durch den folgenden `SELECT` -Anweisung, die alle von der Produktfeldern für ein angegebenes Produkt zurückgibt:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Ersetzen Sie den Namen der gespeicherten Prozedur durch eine SELECT-Abfrage](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Abbildung 19**: Ersetzen Sie den Namen der gespeicherten-Prozedur mit einem `SELECT` Abfrage ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


Im folgenden Bildschirm fordert Sie auf die Namen der gespeicherten Prozedur, die erstellt werden. Geben Sie den Namen `Products_SelectByProductID` , und klicken Sie auf Weiter.


[![Name der neuen gespeicherten Prozedur Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Abbildung 20**: Benennen Sie die neue gespeicherte Prozedur `Products_SelectByProductID` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


Der letzte Schritt des Assistenten ermöglicht uns, ändern Sie die Namen generiert sowie gibt an, ob die Füllung verwenden ein DataTable-Muster, ein DataTable-Muster oder beides zurück. Für diese Methode, lassen Sie beide Optionen aktiviert, aber die Methoden zum Benennen `FillByProductID` und `GetProductByProductID`. Klicken Sie auf "Weiter", um eine Zusammenfassung der Schritte der Assistent ausführen wird, und klicken Sie dann auf "Fertig stellen", um den Assistenten abzuschließen.


[![Benennen Sie die TableAdapter-s-Methoden in FillByProductID und GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Abbildung 21**: Benennen Sie die TableAdapter-s-Methoden zu `FillByProductID` und `GetProductByProductID` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Nach dem Fertigstellen des Assistenten für TableAdapter eine neue Methode verfügbar ist, hat `GetProductByProductID(productID)` , beim Aufrufen, führt die `Products_SelectByProductID` gespeicherten Prozedur, die gerade erstellt haben. Diese neue gespeicherte Prozedur vom Server-Explorer anzeigen, indem Drilldown in den Ordner gespeicherte Prozeduren, und öffnen in Ruhe `Products_SelectByProductID` (wenn es nicht angezeigt wird, mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie die Aktualisierung).

Beachten Sie, dass die `SelectByProductID` gespeicherte Prozedur nimmt `@ProductID` als Eingabeparameter und führt die `SELECT` -Anweisung, die wir im Assistenten eingegeben haben.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Schritt 6: Erstellen einer Business Logic Layer-Klasse

In der tutorialreihe haben wir danach gestrebt darin eine mehrschichtigen Architektur, in der die Darstellungsschicht aller die Aufrufe an die Geschäftslogikschicht (Business Logic Layer, BLL) vorgenommen. Um diese entwurfsentscheidung entsprechen, müssen wir zunächst eine BLL-Klasse für das neue typisierte DataSet erstellen, bevor wir der Darstellungsschicht Product-Daten zugreifen können.

Erstellen Sie eine neue Klassendatei mit dem Namen `ProductsBLLWithSprocs.vb` in die `~/App_Code/BLL` Ordner und fügen Sie den folgenden Code hinzu:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Dieser Klasse imitiert die `ProductsBLL` Klasse Semantik aus früheren Lernprogrammen, jedoch die `ProductsTableAdapter` und `ProductsDataTable` Objekte aus der `NorthwindWithSprocs` DataSet. Z. B. statt einer `Imports NorthwindTableAdapters` -Anweisung am Anfang der Klassendatei als `ProductsBLL` der Fall ist, die `ProductsBLLWithSprocs` -Klasse `Imports NorthwindWithSprocsTableAdapters`. Ebenso die `ProductsDataTable` und `ProductsRow` Objekte, die in dieser Klasse verwendete Präfix der `NorthwindWithSprocs` Namespace. Die `ProductsBLLWithSprocs` Klasse bietet zwei Methoden für Datenzugriff `GetProducts` und `GetProductByProductID`, sowie Methoden zum Hinzufügen, aktualisieren und löschen Sie eine einzelne Product-Instanz.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit der`NorthwindWithSprocs`DataSet auf der Darstellungsschicht

An diesem Punkt haben wir eine DAL erstellt, die gespeicherte Prozeduren zugreifen auf und ändern die Datenbankdaten der zugrunde liegenden verwendet. Wir haben auch eine rudimentäre BLL mit Methoden zum Abrufen aller Produkte oder ein bestimmtes Produkt sowie Methoden zum Hinzufügen, aktualisieren und Löschen von Produkten erstellt. Um dieses Tutorial abzurunden, Let s erstellen Sie eine ASP.NET-Seite, die die BLL s verwendet `ProductsBLLWithSprocs` -Klasse für das anzeigen, aktualisieren und Löschen von Datensätzen.

Öffnen der `NewSprocs.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie einer GridView-Ansicht aus der Toolbox auf den Designer, und nennen Sie es `Products`. Das GridView s Smarttag auswählen, um die Bindung an eine neue, mit dem Namen "ObjectDataSource" `ProductsDataSource`. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `ProductsBLLWithSprocs` Klasse, wie in Abbildung 22 dargestellt.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLLWithSprocs-Klasse](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Abbildung 22**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


Die Dropdown-Liste in der Registerkarte "SELECT" verfügt über zwei Optionen: `GetProducts` und `GetProductByProductID`. Da wir alle Produkte in den GridView-Ansicht anzeigen möchten, wählen Sie die `GetProducts` Methode. Die Dropdownlisten auf den Registerkarten Update-, INSERT- und DELETE jedes müssen nur eine Methode. Stellen Sie sicher, dass jede dieser Dropdown-Listen die geeignete Methode ausgewählt verfügt, und klicken Sie dann auf "Fertig stellen".

Nachdem das ObjectDataSource-Steuerelement-Assistent abgeschlossen ist, wird Visual Studio BoundFields und eine CheckBoxField GridView für die Product-Datenfelder hinzufügen. Aktivieren der GridView s integrierte bearbeiten und Löschen von Features durch Überprüfen der Bearbeitung aktivieren und löschen aktivieren Optionen, die in das Smarttag vorhanden.


[![Die Seite enthält eine GridView mit bearbeiten und Löschen von-Unterstützung aktiviert](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Abbildung 23**: die Seite enthält eine GridView mit bearbeiten und Löschen von-Unterstützung aktiviert ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Als wir haben erläutert in vorherigen Tutorials nach dem Abschluss des Assistenten "ObjectDataSource" s Visual Studio legt die `OldValuesParameterFormatString` Eigenschaft, um die ursprüngliche\_{0}. Dieser Schritt muss auf den Standardwert zurückgesetzt werden {0} erhalten Sie die Parameter, die von den Methoden in unserer BLL erwartet in der Reihenfolge für die Funktionen zur pfadänderung Daten ordnungsgemäß funktioniert. Aus diesem Grund werden Sie sicher, dass die `OldValuesParameterFormatString` Eigenschaft {0} oder entfernen Sie die Eigenschaft vollständig aus der deklarativen Syntax.

Nach Abschluss des Konfigurieren von Datenquellen-Assistenten, bearbeiten und Löschen von-Unterstützung in den GridView und Zurückgeben von "ObjectDataSource" s einschalten `OldValuesParameterFormatString` -Eigenschaft auf den Standardwert zurück, im deklarativen Markup Ihrer Seite s sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

An diesem Punkt können wir die GridView aufräumen, durch Anpassen der Benutzeroberfläche der Bearbeitung zum Einschließen von Validierung, dass die `CategoryID` und `SupplierID` Spalten Rendern als DropDownList-Steuerelementen und so weiter. Wir könnten auch eine clientseitiger Bestätigung hinzufügen, auf die Schaltfläche "löschen", und sollten Sie die Zeit zum Implementieren dieser Verbesserungen in Anspruch nehmen. Da diese Themen in vorherigen Tutorials behandelt wurde haben, werden jedoch nicht diese wieder hier behandelt.

Unabhängig davon, ob Sie die GridView oder nicht optimiert haben testen Sie die Seite "," s-Kernfunktionen in einem Browser aus. Wie in Abbildung 24 dargestellt, enthält die Seite die Produkte in einer GridView-Ansicht, die pro Zeile bearbeiten und Löschen von Funktionen bereitstellt.


[![Die Produkte können angezeigt, bearbeitet und aus der GridView gelöscht werden](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Abbildung 24**: die Produkte angezeigt werden können, bearbeiteter und aus der GridView gelöscht ([klicken Sie, um das Bild in voller Größe anzeigen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Die TableAdapters in einem typisierten DataSet können Sie Daten aus der Datenbank, die mit Ad-hoc-SQL-Anweisungen oder mithilfe von gespeicherten Prozeduren zugreifen. Beim Arbeiten mit gespeicherten Prozeduren, können entweder vorhandenen gespeicherten Prozeduren verwendet werden, oder der TableAdapter-Assistenten kann angewiesen werden, zum Erstellen von neuen gespeicherten Prozeduren, die auf der Grundlage einer `SELECT` Abfrage. In diesem Tutorial haben wir, wie Sie die gespeicherten Prozeduren, die automatisch für uns erstellt haben.

Während Sie die gespeicherten Prozeduren, die automatisch generierte können Zeit sparen, gibt es bestimmte Fälle, in dem die gespeicherte Prozedur erstellt der Assistent t ausrichten, mit was wir selbst erstellt haben würden. Ein Beispiel ist die `Products_Update` gespeicherte Prozedur, die beide erwartet `@Original_ProductID` und `@ProductID` Eingabeparameter, obwohl die `@Original_ProductID` Parameter war überflüssig.

In vielen Szenarien ist die gespeicherten Prozeduren wurden möglicherweise bereits erstellt, oder erstellen sie manuell, um eine feiner abgestufte Kontrolle über die gespeicherte Prozedur-s-Befehle haben sollen. In beiden Fällen möchten wir weisen Sie den TableAdapter um vorhandene gespeicherte Prozeduren für ihre Methoden zu verwenden. Wir sehen, wie Sie dazu im nächsten Tutorial.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen und Verwalten von gespeicherten Prozeduren](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQLServer-gespeicherte Prozedur-Grundlagen](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Gespeicherte Prozeduren: Eine Übersicht über](http://www.sqlteam.com/item.asp?ItemID=563)
- [Schreiben einer gespeicherten Prozedur](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Geisenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Weiter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
