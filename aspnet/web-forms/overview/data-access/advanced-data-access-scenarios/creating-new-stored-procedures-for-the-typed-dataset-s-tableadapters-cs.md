---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Erstellen neuer gespeicherte Prozeduren für das typisierte DataSet TableAdapters (c#) | Microsoft Docs
author: rick-anderson
description: In früheren Lernprogrammen haben wir die SQL-Anweisungen in der getesteten Codes erstellt und übergeben die Anweisungen in der Datenbank ausgeführt werden. Ein alternativer Ansatz ist die Verwendung von s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 3baff73d63cbc96a1a9f2222d077035cdd9a68f7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879295"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Erstellen neuer gespeicherte Prozeduren für das typisierte DataSet TableAdapters (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) oder [PDF herunterladen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> In früheren Lernprogrammen haben wir die SQL-Anweisungen in der getesteten Codes erstellt und übergeben die Anweisungen in der Datenbank ausgeführt werden. Ein alternativer Ansatz werden gespeicherte Prozeduren verwenden, in dem die SQL-Anweisungen in der Datenbank vorab definiert. In diesem Lernprogramm erfahren wir haben dem TableAdapter-Assistenten neue gespeicherte Prozeduren automatisch generiert.


## <a name="introduction"></a>Einführung

Das Data Access Layer (DAL) für diese Lernprogramme werden typisierte DataSets verwendet. Wie in beschrieben die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm typisierte DataSets bestehen aus DataTables stark typisierte und TableAdapters. Die DataTables stellen logischen Entitäten dar, in das System während der TableAdapter-Schnittstelle, mit der zugrunde liegenden Datenbank, die Daten Zugriff Aufgaben auszuführen. Dies schließt die Datentabellen mit Daten auffüllen, Ausführen von Abfragen, die skalare Daten zurückgeben und einfügen, aktualisieren und Löschen von Datensätzen aus der Datenbank.

Die SQL-Befehle ausgeführt, indem die TableAdapters werden entweder Ad-hoc-SQL-Anweisungen, wie z. B. `SELECT columnList FROM TableName`, oder gespeicherten Prozeduren. Die TableAdapters in unserer Architektur verwendet Ad-hoc-SQL-Anweisungen. Viele Entwickler und Datenbankadministratoren bevorzugen jedoch gespeicherte Prozeduren über Ad-hoc-SQL-Anweisungen für aus Gründen der Sicherheit, Verwaltbarkeit und aktualisierbar. Andere lieber ardently Ad-hoc-SQL-Anweisungen für die Flexibilität. Eigene Arbeit ich begünstigen gespeicherte Prozeduren über Ad-hoc-SQL-Anweisungen jedoch ausgewählt haben, um Ad-hoc-SQL-Anweisungen zu verwenden, um die früheren Lernprogramme zu vereinfachen.

Wenn erleichtert einen TableAdapter definieren oder Hinzufügen von neuen Methoden, die TableAdapter-s-Assistent genauso einfach neue gespeicherte Prozeduren erstellen oder vorhandene gespeicherte Prozeduren verwenden, wie es zur Verwendung von Ad-hoc-SQL-Anweisungen. In diesem Lernprogramm untersuchen wir wie der TableAdapter-s-Assistent gespeicherte Prozeduren automatisch generiert. In den nächsten Lernprogrammen werden wir die TableAdapter-s-Methoden, um vorhandene oder manuell erstellte gespeicherte Prozeduren konfigurieren betrachten.

> [!NOTE]
> Finden Sie unter Rob Howards Blog-Eintrag [Don't mit gespeicherten Prozeduren noch?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) und [Frans Bouma](https://weblogs.asp.net/fbouma/) s Blogeintrag [gespeicherte Prozeduren sind fehlerhaft, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) für eine lebhafte Diskussion über die vor- und Nachteile der gespeicherte Prozeduren und Ad-hoc-SQL.


## <a name="stored-procedure-basics"></a>Grundlagen der gespeicherten Prozeduren

Funktionen sind ein Konstrukt, das häufig für alle Programmiersprachen. Eine Funktion ist eine Auflistung von Anweisungen, die ausgeführt werden, wenn die Funktion aufgerufen wird. Funktionen können annehmen von Eingabeparametern und können optional einen Wert zurückgeben. *[Gespeicherte Prozeduren](http://en.wikipedia.org/wiki/Stored_procedure)*  Datenbankkonstrukte, die viele ähnlichkeiten mit Funktionen in Programmiersprachen zu vergleichen gemeinsam sind. Eine gespeicherte Prozedur besteht aus einem Satz von T-SQL-Anweisungen, die ausgeführt werden, wenn die gespeicherte Prozedur aufgerufen wird. Eine gespeicherte Prozedur kann 0 (null), um viele Eingabeparameter akzeptieren und kann die Skalare Werte als auch Ausgabeparameter zurückgeben oder, in den meisten Fällen-Resultsets aus `SELECT` Abfragen.

> [!NOTE]
> Gespeicherte Prozeduren werden häufig als gespeicherte Prozeduren oder SPs bezeichnet.


Gespeicherte Prozeduren werden erstellt, mit der [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL-Anweisung. Das folgende T-SQL-Skript erstellt z. B. eine gespeicherte Prozedur namens `GetProductsByCategoryID` nimmt einen einzelnen Parameter mit dem Namen `@CategoryID` und gibt die `ProductID`, `ProductName`, `UnitPrice`, und `Discontinued` Felder dieser Spalten in der `Products` Tabelle, die entsprechende `CategoryID` Wert:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Nachdem diese gespeicherte Prozedur erstellt wurde, kann er mithilfe der folgenden Syntax aufgerufen werden:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> In den nächsten Lernprogrammen untersuchen wir das Erstellen von gespeicherten Prozeduren über die Visual Studio-IDE. In diesem Lernprogramm werden allerdings wir zu den TableAdapter-Assistenten, die die gespeicherten Prozeduren für uns automatisch generieren lassen.


Zusätzlich zum Zurückgeben von Daten einfach, werden gespeicherte Prozeduren häufig verwendet, um mehrere Datenbankbefehle innerhalb des Bereichs einer einzelnen Transaktion auszuführen. Eine gespeicherte Prozedur namens `DeleteCategory`, z. B. dauern eine `@CategoryID` Parameter, und führen Sie zwei `DELETE` Anweisungen: zuerst eine verwandte Produkte und einen zweiten durch das Löschen der angegebenen Kategorie zu löschen. Mehrere Anweisungen innerhalb einer gespeicherten Prozedur werden *nicht* automatisch umschlossene innerhalb einer Transaktion. Weitere T-SQL-Befehle ausgegeben werden, um sicherzustellen, dass die gespeicherte Prozedur s, die mehrere Befehle als atomarer Vorgang behandelt werden müssen. Wir sehen, dass eine gespeicherte Prozedur s Befehle innerhalb des Bereichs einer Transaktion in das nachfolgende Lernprogramm Wrappen von.

Bei Verwendung von gespeicherten Prozeduren in einer Architektur Aufrufen der Datenzugriffsebene-s-Methoden, eine bestimmte gespeicherte Prozedur, statt eine Ad-hoc-SQL-Anweisung auszugeben. Auf diese Weise den Speicherort der SQL-Anweisungen (in der Datenbank) ausgeführten zentralisiert, anstatt es in der Anwendungsarchitektur s definiert. Diese Zentralisierung wohl erleichtert es, suchen, analysieren und Optimieren der Abfragen und bietet ein besseres Verständnis hinsichtlich, wo und wie die Datenbank verwendet wird.

Weitere Informationen zu den Grundlagen der gespeicherten Prozedur wenden Sie sich an die Ressourcen im Abschnitt Weitere Themen am Ende dieses Lernprogramms.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Schritt 1: Erstellen der erweiterte Data Access Layer Szenarien Webseiten

Bevor wir unsere Erläuterung zum Erstellen einer DAL, die mithilfe von gespeicherten Prozeduren beginnen, können Sie s, schalten Sie zuerst einen Moment Zeit, um die ASP.NET-Seiten in unserer Websiteprojekt zu erstellen, die wir für diese und die nächste mehrere Lernprogramme benötigen. Starten, indem Sie einen neuen Ordner namens `AdvancedDAL`. Fügen Sie die folgenden ASP.NET-Seiten in diesen Ordner, und dabei sicherstellen, ordnen Sie jede Seite mit den `Site.master` Gestaltungsvorlage:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Fügen Sie die ASP.NET-Seiten für die erweiterte Data Access Layer Szenarien Lernprogramme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Abbildung 1**: Fügen Sie die ASP.NET-Seiten für die erweiterte Data Access Layer Szenarien Lernprogramme


Wie in den anderen Ordnern `Default.aspx` in den `AdvancedDAL` Ordner werden die Lernprogramme in einem Abschnitt aufgelistet. Bedenken Sie, dass die `SectionLevelTutorialListing.ascx` Benutzersteuerelement stellt diese Funktionalität bereit. Aus diesem Grund Hinzufügen dieses Benutzersteuerelement zu `Default.aspx` durch Ziehen aus dem Projektmappen-Explorer auf die Seite s Entwurfsansicht.


[![Das Benutzersteuerelement SectionLevelTutorialListing.ascx "default.aspx" hinzufügen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Abbildung 2**: Hinzufügen der `SectionLevelTutorialListing.ascx` Benutzersteuerelement `Default.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Abschließend fügen Sie diese Seiten als Einträge an die `Web.sitemap` Datei. Fügen Sie insbesondere das folgende Markup nach dem Arbeiten mit Daten in einem Batch verarbeitet `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Nach der Aktualisierung `Web.sitemap`, nehmen einen Moment Zeit, um die Lernprogramme-Website über einen Browser anzuzeigen. Klicken Sie im Menü auf der linken Seite enthält nun die Elemente für die erweiterte DAL Szenarien Lernprogramme.


![Die Siteübersicht enthält nun die Einträge für die erweiterte DAL-Szenarien-Lernprogramme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Abbildung 3**: die Siteübersicht enthält jetzt die Einträge für die erweiterte DAL-Szenarien-Lernprogramme


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Schritt 2: Konfigurieren eines TableAdapters zum Erstellen neuer gespeicherte Prozeduren

Zum Demonstrieren der Erstellung eine Datenzugriffsschicht, die gespeicherte Prozeduren anstelle von Ad-hoc-SQL-Anweisungen verwendet, können s Erstellen eines neuen typisierten Datasets in der `~/App_Code/DAL` Ordner mit dem Namen `NorthwindWithSprocs.xsd`. Da wir diesen Prozess im Detail in vorherigen Lernprogrammen gewechselt sind, wird es durch die Schritte hier schnell fortgesetzt. Wenn Sie sich Festfahren oder weitere schrittweise Anweisungen zum Erstellen und konfigurieren eine typisierte DataSet benötigen, verweisen Sie zurück auf die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm.

Fügen Sie ein neues DataSet zum Projekt, indem Sie mit der rechten Maustaste auf die `DAL` Ordner, neues Element hinzufügen auswählen und die DataSet-Vorlage auswählen, wie in Abbildung 4 dargestellt.


[![Fügen Sie ein neues typisiertes DataSet, das Projekt mit dem Namen NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Abbildung 4**: Hinzufügen eines neuen typisierten Datasets auf das Projekt mit dem Namen `NorthwindWithSprocs.xsd` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Dies wird der neue typisierte DataSet erstellen, öffnen Sie ihren Designer einen neuen TableAdapter erstellen und den TableAdapter-Konfigurations-Assistenten zu starten. Der erste Schritt des TableAdapter-Konfigurations-Assistenten-s fordert die Remotedatenbank mit auszuwählen. Die Verbindungszeichenfolge zur Northwind-Datenbank sollten in der Dropdown-Liste aufgeführt werden. Wählen Sie diese Option aus, und klicken Sie auf Weiter.

Aus diesem nächsten Bildschirm können wir auswählen, wie der TableAdapter auf die Datenbank zugreifen soll. In vorherigen Lernprogrammen ausgewählt wird die erste Option, die SQL-Anweisungen. Für dieses Lernprogramm die zweite Option auswählen, neue gespeicherte Prozeduren erstellen, und klicken Sie auf Weiter.


[![Weisen Sie die TableAdpater zum Erstellen neuer gespeicherter Prozeduren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Abbildung 5**: Weisen Sie die TableAdpater, erstellen Sie neue gespeicherte Prozeduren ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Genau wie bei der Verwendung von Ad-hoc-SQL-Anweisungen, in den folgenden Schritt wird gebeten werden, die `SELECT` -Anweisung für die Hauptabfrage des TableAdapter s. Aber statt der `SELECT` Anweisung hier eingegeben werden, um eine Ad-hoc-Abfrage direkt ausführen, in der TableAdapter-s-Assistent erstellt eine gespeicherte Prozedur, die dieses Objekt enthält `SELECT` Abfrage.

Verwenden Sie die folgenden `SELECT` Abfrage für diese TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Geben Sie die SELECT-Abfrage](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Abbildung 6**: Geben Sie die `SELECT` Abfrage ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Die obige Abfrage unterscheidet sich geringfügig von der Hauptabfrage des der `ProductsTableAdapter` in die `Northwind` typisierte DataSet. Bedenken Sie, dass die `ProductsTableAdapter` in die `Northwind` typisierte DataSet enthält zwei abhängige Unterabfragen, um die Kategorienamen und den Firmennamen für jede Produktkategorie s und Lieferanten wiederherzustellen. In den kommenden [Aktualisieren des TableAdapters verwenden verknüpft](updating-the-tableadapter-to-use-joins-cs.md) Lernprogramm betrachten wir das Hinzufügen dieser-bezogene Daten dieser TableAdapter.


Nehmen Sie einen Moment Zeit, auf die Schaltfläche "Erweiterte Optionen" klicken. Hier können wir angeben, ob der Assistent auch generieren soll INSERT-, Update- und Delete-Anweisungen für den TableAdapter, soll eine vollständige Parallelität verwendet und gibt an, ob die Datentabelle nach Inserts und Updates aktualisiert werden sollen. Die generieren Insert, Update und Delete-Anweisungen-Option ist standardmäßig aktiviert. Lassen sie dieses Kontrollkästchen aktiviert. Lassen Sie für dieses Lernprogramm die Verwendung durch vollständige Parallelitätsoptionen deaktiviert.

Die gespeicherten Prozeduren, die automatisch durch den TableAdapter-Assistenten erstellt haben, wird es angezeigt, dass die Aktualisierung der Daten-Tabellenoption ignoriert wird. Unabhängig davon, ob dieses Kontrollkästchen aktiviert ist, die sich ergebende INSERT- und Update abgerufen werden gespeicherte Prozeduren des Datensatzes gerade eingefügten oder gerade aktualisiert, da wir in Schritt 3 angezeigt wird.


![Lassen Sie die Option aktiviert generieren Insert, Update und Delete-Anweisungen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Abbildung 7**: lassen Sie die Option aktiviert generieren Insert, Update und Delete-Anweisungen


> [!NOTE]
> Wenn die Option zum Verwenden einer Verletzung der vollständigen Parallelität aktiviert ist, wird der Assistent zusätzliche Bedingungen zum Hinzufügen der `WHERE` -Klausel, die verhindert, dass die Daten aktualisiert werden, wenn Änderungen festgestellt wurden, in anderen Feldern. Verweisen auf die [optimistische Parallelität implementieren](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) Lernprogramm für Weitere Informationen zur Verwendung der integrierten vollständige Parallelität TableAdapter-s-Control-Feature.


Nach der Eingabe der `SELECT` abzufragen, und bestätigen, dass das Generieren von Insert, Update und Delete-Anweisungen-Option aktiviert ist, und klicken Sie auf Weiter. Diese nächsten Bildschirm in Abbildung 8 gezeigt werden aufgefordert, die Namen der gespeicherten Prozeduren, die der Assistent erstellt für auswählen, einfügen, aktualisieren und Löschen von Daten. Diese gespeicherten Prozeduren Namen ändern `Products_Select`, `Products_Insert`, `Products_Update`, und `Products_Delete`.


[![Benennen Sie die gespeicherten Prozeduren](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Abbildung 8**: Benennen Sie gespeicherte Prozeduren ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Um die T-SQL finden Sie unter der TableAdapter-Assistenten zum Erstellen von vier gespeicherten Prozeduren verwenden, klicken Sie auf Vorschau SQL-Skript. Über das Dialogfeld SQL-Skriptvorschau anzeigen können Sie das Skript in einer Datei speichern oder in die Zwischenablage kopieren.


![Anzeigen Sie das SQL-Skript verwendet, um die gespeicherten Prozeduren Generieren der Vorschau](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Abbildung 9**: Vorschau der SQL-Skript verwendet wird, um die gespeicherten Prozeduren generieren


Nach dem Benennen der gespeicherten Prozeduren, klicken Sie neben dem entsprechenden Namen die TableAdapter-Methoden. Wir können genau wie beim Verwenden von Ad-hoc-SQL-Anweisungen Methoden erstellen, die eine vorhandene DataTable füllen oder einen neuen zurückgeben. Es können auch angeben, ob die TableAdapter das DB-Direct-Muster für das Einfügen, aktualisieren und Löschen von Datensätzen enthalten soll. Belassen Sie alle drei Kontrollkästchen aktiviert, aber benennen Sie eine DataTable-Methode, um der Rückgabe `GetProducts` (siehe Abbildung 10).


[![Nennen Sie die Methoden Füllung und GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Abbildung 10**: Benennen Sie die Methoden `Fill` und `GetProducts` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Klicken Sie auf Weiter, um eine Zusammenfassung der Schritte angezeigt, die der Assistent ausführt. Schließen Sie den Assistenten, indem Sie auf "Fertig stellen". Nachdem der Assistent abgeschlossen wurde, werden Sie mit dem DataSet-s-Designer, der jetzt einschließen soll zurückgegeben werden die `ProductsDataTable`.


[![Der DataSet-s-Designer zeigt die neu hinzugefügte ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Abbildung 11**: der DataSet-s-Designer zeigt die neu hinzugefügte `ProductsDataTable` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Schritt 3: Untersuchen der neu erstellten gespeicherten Prozeduren

Der TableAdapter-Assistenten automatisch verwendet, die in Schritt2 erstellt die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren und Löschen von Daten. Diese gespeicherten Prozeduren können angezeigt oder über Visual Studio geändert werden, indem Sie den Server-Explorer aufrufen und Drilldowns in der Datenbank gespeicherte Prozeduren Ordner. Wie Abbildung 12 zeigt, wird die Northwind-Datenbank enthält vier neue gespeicherte Prozeduren: `Products_Delete`, `Products_Insert`, `Products_Select`, und `Products_Update`.


![Die vier gespeicherten Prozeduren, die in Schritt 2 erstellten finden Sie im Ordner "s-Datenbank gespeicherten Prozeduren"](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Abbildung 12**: die vier gespeicherten Prozeduren, die in Schritt 2 erstellten finden Sie im Ordner "s-Datenbank gespeicherten Prozeduren"


> [!NOTE]
> Wenn Sie den Server-Explorer nicht angezeigt werden, wechseln Sie zum Menü "Ansicht", und wählen Sie die Server-Explorer aus. Wenn Sie nicht die produktbezogene gespeicherten Prozeduren hinzugefügt, die aus Schritt2 angezeigt werden, zu aktualisieren versuchen Sie es mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren und auswählen.


Zum Anzeigen oder Ändern einer gespeicherten Prozedur, doppelklicken Sie auf seinen Namen im Server-Explorer oder Alternativ können Sie mit der rechten Maustaste auf die gespeicherte Prozedur, und wählen Sie öffnen. Abbildung 13 zeigt die `Products_Delete` gespeicherte Prozedur aus, wenn geöffnet.


[![Gespeicherte Prozeduren können geöffnet und in Visual Studio geändert werden](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Abbildung 13**: gespeicherte Prozeduren können geöffnet und werden geändert von innerhalb von Visual Studio ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Den Inhalt beider der `Products_Delete` und `Products_Select` gespeicherte Prozeduren sind sehr einfach. Die `Products_Insert` und `Products_Update` gespeicherte Prozeduren werden andererseits, sollten eine genauere Kontrolle beide beim Ausführen der eine `SELECT` Anweisung nach deren `INSERT` und `UPDATE` Anweisungen. Z. B. die folgende SQL-Anweisung bildet die `Products_Insert` gespeicherte Prozedur:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Die gespeicherte Prozedur als Eingabeparameter akzeptiert die `Products` Spalten, die von zurückgegeben wurden die `SELECT` Abfrage angegeben, in dem TableAdapter-s-Assistenten und diese Werte werden verwendet, eine `INSERT` Anweisung. Nach der `INSERT` -Anweisung eine `SELECT` Abfrage wird zum Zurückgeben der `Products` Spaltenwerte (einschließlich der `ProductID`) des neu hinzugefügten Eintrags. Diese Aktualisierung-Funktion ist nützlich, beim Hinzufügen eines neuen Datensatzes automatisch mit dem BatchUpdate Muster da sie die neu hinzugefügte aktualisiert `ProductRow` Instanzen `ProductID` Eigenschaften mit der automatisch inkrementierende Werte, die von der Datenbank zugewiesen.

Der folgende Code veranschaulicht diese Funktion. Er enthält eine `ProductsTableAdapter` und `ProductsDataTable` erstellt, die für die `NorthwindWithSprocs` typisierte DataSet. Ein neues Produkt wird in der Datenbank hinzugefügt, durch das Erstellen einer `ProductsRow` Instanz, dessen Werte angeben und die TableAdapter aufrufen `Update` -Methode auf und übergibt die `ProductsDataTable`. Intern die TableAdapter `Update` Methode zählt die `ProductsRow` Instanzen in der übergebene "DataTable" (in diesem Beispiel gibt es nur einmal ist – wir soeben hinzugefügt), und führt die entsprechenden insert, update oder delete-Befehl. In diesem Fall die `Products_Insert` gespeicherte Prozedur wird ausgeführt, wodurch einen neuen Datensatz hinzugefügt der `Products` Tabelle und gibt die Details des neu hinzugefügten Datensatzes zurück. Die `ProductsRow` Instanz s `ProductID` Wert wird dann aktualisiert. Nach der `Update` Methode abgeschlossen wurde, können wir auf die neu hinzugefügten Datensatz s zugreifen `ProductID` Wert über die `ProductsRow` s `ProductID` Eigenschaft.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

Die `Products_Update` gespeicherte Prozedur auf ähnliche Weise umfasst eine `SELECT` Anweisung nach seiner `UPDATE` Anweisung.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Beachten Sie, dass diese gespeicherte Prozedur umfasst zwei Eingabeparameter für `ProductID`: `@Original_ProductID` und `@ProductID`. Diese Funktionalität ermöglicht Szenarien, in denen der Primärschlüssel geändert werden kann. In einer Mitarbeiterdatenbank möglicherweise jede Mitarbeiterdatensatz z. B. die Employee-s-Sozialversicherungsnummer als ihres Primärschlüssels erstellt verwenden. Um eine Sozialversicherungsnummer der vorhandenen Mitarbeiter s zu ändern, müssen die neue Social Security Number und ursprünglichen Zeichensatz angegeben werden. Für die `Products` Tabelle, eine solche Funktion ist nicht erforderlich, da die `ProductID` Spalte ist eine `IDENTITY` Spalte und sollte nie geändert werden. In der Tat die `UPDATE` -Anweisung in der `Products_Update` gespeicherte Prozedur verfügt über t enthalten die `ProductID` Spalte in der Spaltenliste. Deshalb while `@Original_ProductID` werden in der `UPDATE` Anweisung s `WHERE` -Klausel, er ist überflüssig, für die `Products` -Tabelle und konnten ersetzt werden, indem die `@ProductID` Parameter. Beim Ändern einer s-Parameter von gespeicherten Prozeduren ist es wichtig, dass die TableAdapter-Methoden, die diese gespeicherte Prozedur verwenden, ebenfalls aktualisiert werden.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Schritt 4: Ändern eine s-Parameter von gespeicherten Prozeduren und Aktualisieren des TableAdapters

Da die `@Original_ProductID` Parameter ist überflüssig, Let s entfernen Sie es aus der `Products_Update` vollständig gespeicherten Prozedur. Öffnen der `Products_Update` gespeicherte Prozedur Löschen der `@Original_ProductID` Parameter, und in der `WHERE` -Klausel der der `UPDATE` -Anweisung, Änderung, die der Namen des Parameters verwendet wird, aus `@Original_ProductID` auf `@ProductID`. Nachdem diese Änderungen vorgenommen wurden, sollte die T-SQL in der gespeicherten Prozedur wie folgt aussehen:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Um diese Änderungen in der Datenbank zu speichern, klicken Sie auf das Symbol "Speichern" in der Symbolleiste, oder drücken Sie STRG + S. An diesem Punkt der `Products_Update` gespeicherte Prozedur erwartet kein `@Original_ProductID` input-Parameter, aber die TableAdapter ist so konfiguriert, dass um solche Parameter zu übergeben. Sehen Sie die Parameter, die der TableAdapter sendet an die `Products_Update` TableAdapter im DataSet-Designer auswählen, Begriff, das Eigenschaftenfenster, und klicken auf die Auslassungspunkte in die gespeicherte Prozedur die `UpdateCommand` s `Parameters` Auflistung. Daraufhin wird das Dialogfeld Editor für die Parameters-Auflistung in Abbildung 14 gezeigt.


![Die Parameter-Auflistungs-Editor-Listen die verwendeten Parameter übergeben wird, auf die Products_Update gespeicherte Prozedur](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Abbildung 14**: die Parameter-Auflistungs-Editor-Listen die verwendeten Parameter übergeben wird, um die `Products_Update` gespeicherte Prozedur


Sie können diesen Parameter hier entfernen, wählen Sie einfach die `@Original_ProductID` Parameter aus der Liste der Elemente, und klicken auf die Schaltfläche "entfernen".

Alternativ können Sie die Parameter für alle Methoden verwendet werden, indem Sie mit der rechten Maustaste auf den TableAdapter im Designer und konfigurieren aktualisieren. Hierdurch erscheint der TableAdapter-Konfigurations-Assistent, die gespeicherten Prozeduren zum auswählen, einfügen, aktualisieren, auflisten und löschen, zusammen mit den Parametern die gespeicherten Prozeduren erwarten. Wenn Sie auf die Dropdownliste Update klicken, sehen Sie die `Products_Update` gespeicherte Prozeduren erwartet Eingabeparameter, darunter jetzt nicht mehr `@Original_ProductID` (siehe Abbildung 15). Klicken Sie einfach auf "Fertig stellen", um die Parameters-Auflistung, die von den TableAdapter verwendet automatisch zu aktualisieren.


[![Sie können alternativ die TableAdapter s mithilfe des Assistenten zum Aktualisieren Sie ihren Methoden Parameters-Auflistungen](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Abbildung 15**: können Sie alternativ die TableAdapter-Konfigurations-Assistenten zum Aktualisieren seiner Methoden Parameter Sammlungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Schritt 5: Hinzufügen von zusätzlichen TableAdapter-Methoden

Schritt 2 dargestellt ist für die Erstellung ein neues TableAdapter es einfach, haben die entsprechenden gespeicherten Prozeduren automatisch generiert. Dasselbe gilt beim Hinzufügen von zusätzlicher Methoden zu einem TableAdapter. Um dies zu veranschaulichen, können Sie s fügen eine `GetProductByProductID(productID)` Methode, um die `ProductsTableAdapter` in Schritt2 erstellt haben. Diese Methode dauert als Eingabe eine `ProductID` Wert und Zurückgeben von Details über das angegebene Produkt.

Starten Sie, indem Sie mit der rechten Maustaste auf den TableAdapter und Abfrage hinzufügen im Kontextmenü auswählen.


![Fügen Sie dem TableAdapter eine neue Abfrage hinzu](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Abbildung 16**: hinzufügen eine neue Abfrage dem TableAdapter


Dadurch wird der TableAdapter-Abfrage-Konfigurations-Assistent gestartet, der zuerst aufgefordert, wie der TableAdapter für die Datenbank zugreifen, sollten. Um eine neue gespeicherte Prozedur erstellt haben, wählen Sie erstellen eine neue gespeicherte Prozedur aus, und klicken Sie auf Weiter.


[![Wählen Sie dem Erstellen einer neuen gespeicherten Prozedur Option](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Abbildung 17**: Wählen Sie dem Erstellen eine neue gespeicherte Prozedur Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Der nächste Bildschirm fordert uns identifiziert den Typ der Abfrage ausgeführt wird, ob es einen Satz von Zeilen oder einen einzelnen skalaren Wert zurückgeben, oder führen, ein `UPDATE`, `INSERT`, oder `DELETE` Anweisung. Da die `GetProductByProductID(productID)` Methode eine Zeile zurückgeben, lassen Sie wählen die Row-Option ausgewählt ist, und klicken Sie weiter zurückgibt.


[![Wählen Sie wählen die Option-Zeile zurückgibt.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Abbildung 18**: Wählen Sie wählen die Option-Zeile zurückgibt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Im nächste Bildschirm wird angezeigt, die TableAdapter-s-Haupt-Abfrage, die nur den Namen der gespeicherten Prozedur führt (`dbo.Products_Select`). Ersetzen Sie den Namen der gespeicherten Prozedur durch Folgendes `SELECT` -Anweisung, die alle von der Produktfeldern für ein angegebenes Produkt zurückgibt:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Ersetzen Sie den Namen der gespeicherten Prozedur durch eine SELECT-Abfrage](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Abbildung 19**: Ersetzen Sie den Namen der gespeicherten-Prozedur mit einem `SELECT` Abfrage ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Im folgenden Bildschirm aufgefordert, die Namen der gespeicherten Prozedur, die erstellt werden soll. Geben Sie den Namen `Products_SelectByProductID` , und klicken Sie auf Weiter.


[![Name der neuen gespeicherten Prozedur Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Abbildung 20**: Benennen Sie die neue gespeicherte Prozedur `Products_SelectByProductID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Der letzte Schritt des Assistenten ermöglicht es, ändern Sie die Methode Namen generiert sowie gibt an, ob die Füllung verwenden ein DataTable-Muster, ein DataTable-Muster oder beides zurück. Belassen Sie für diese Methode beide Optionen aktiviert, aber die Methoden zum Benennen `FillByProductID` und `GetProductByProductID`. Klicken Sie auf Weiter, um eine Zusammenfassung der Schritte anzuzeigen, der Assistent ausführen wird, und klicken Sie dann auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Benennen Sie die TableAdapter-s-Methoden FillByProductID und GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Abbildung 21**: Benennen Sie die TableAdapter-s-Methoden zum `FillByProductID` und `GetProductByProductID` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Nach Abschluss des Assistenten für TableAdapter eine neue Methode verfügbar ist, hat `GetProductByProductID(productID)` , die beim Aufrufen, führt der `Products_SelectByProductID` gespeicherten Prozedur, die gerade erstellt wurde. Nehmen Sie einen Moment Zeit, um diese neue gespeicherte Prozedur im Server-Explorer anzeigen, indem Sie Drilldowns in den Ordner gespeicherte Prozeduren, und öffnen `Products_SelectByProductID` (sofern Sie nicht angezeigt, mit der rechten Maustaste auf den Ordner gespeicherte Prozeduren, und wählen Sie die Aktualisierung).

Beachten Sie, dass die `SelectByProductID` gespeicherte Prozedur nimmt `@ProductID` als Eingabeparameter und führt die `SELECT` -Anweisung, die wir im Assistenten eingegeben haben.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Schritt 6: Erstellen einer Business Logic Layer-Klasse

In der gesamten Reihe von Lernprogrammen haben wir strived darin eine mehrstufige Architektur, in der die Darstellungsschicht alle seine Aufrufe an Business Logic Layer (BLL) vorgenommen. Um diesen Entwurf entscheiden entsprechen, müssen wir zunächst eine BLL-Klasse für das neue typisierte DataSet erstellen, bevor wir die Darstellungsschicht Produktdaten zugreifen können.

Erstellen Sie eine neue Klassendatei mit dem Namen `ProductsBLLWithSprocs.cs` in den `~/App_Code/BLL` Ordner und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Diese Klasse imitiert die `ProductsBLL` -Klasse Semantik aus früheren Lernprogramme, jedoch mit der `ProductsTableAdapter` und `ProductsDataTable` Objekte aus der `NorthwindWithSprocs` DataSet. Angenommen, anstatt eine `using NorthwindTableAdapters` -Anweisung am Anfang der Klassendatei als `ProductsBLL` der Fall ist, die `ProductsBLLWithSprocs` -Klasse verwendet `using NorthwindWithSprocsTableAdapters`. Entsprechend der `ProductsDataTable` und `ProductsRow` vorangestellt-Objekten, die in dieser Klasse verwendet die `NorthwindWithSprocs` Namespace. Die `ProductsBLLWithSprocs` Klasse bietet zwei Methoden, `GetProducts` und `GetProductByProductID`, und Methoden zum Hinzufügen, aktualisieren und löschen Sie eine einzelne Product-Instanz.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Schritt 7: Arbeiten mit der`NorthwindWithSprocs`DataSet aus der Darstellungsschicht

Zu diesem Zeitpunkt haben wir eine DAL erstellt, die gespeicherte Prozeduren zum Zugreifen auf und Ändern von Daten in der zugrunde liegenden Datenbank verwendet. Wir haben auch eine rudimentäre BLL mit Methoden zum Abrufen aller Produkte oder ein bestimmtes Produkt zusammen mit Methoden zum Hinzufügen, aktualisieren und Löschen von Produkten erstellt. Um dieses Lernprogramm zu runden, Let s erstellen eine ASP.NET-Seite, die die BLL s verwendet `ProductsBLLWithSprocs` Klasse zum Anzeigen, aktualisieren und Löschen von Datensätzen.

Öffnen der `NewSprocs.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie eine GridView aus der Toolbox in den Designer, und benennen es `Products`. Aus der GridView s Smarttag auswählen, um die Bindung an eine neue ObjectDataSource mit dem Namen `ProductsDataSource`. Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` -Klasse, wie in Abbildung 22 dargestellt.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLLWithSprocs-Klasse](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Abbildung 22**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


Die Dropdownliste in der Registerkarte "SELECT" verfügt über zwei Optionen: `GetProducts` und `GetProductByProductID`. Da wir alle Produkte in der GridView anzeigen möchten, wählen Sie die `GetProducts` Methode. Die Dropdownlisten mit den Registerkarten Update-, INSERT- und DELETE müssen nur eine Methode. Stellen Sie sicher, dass jede dieser Dropdown-Listen die geeignete Methode ausgewählt wurde, und klicken Sie dann auf ' Fertig stellen '.

Nachdem das ObjectDataSource-Assistent abgeschlossen ist, wird Visual Studio BoundFields und eine CheckBoxField GridView für die Product-Datenfelder hinzufügen. Aktivieren der GridView s integrierte bearbeiten und Löschen von Funktionen durch Überprüfen der Bearbeiten aktivieren und löschen aktivieren Optionen in den smart Tag vorhanden.


[![Diese Seite enthält eine GridView mit bearbeiten und Löschen von Unterstützung aktiviert](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Abbildung 23**: die Seite enthält eine GridView mit bearbeiten und Löschen von Unterstützung aktiviert ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Als wir haben erläutert in vorherigen Lernprogrammen, nach dem Abschluss des Assistenten s ObjectDataSource Visual Studio legt die `OldValuesParameterFormatString` Eigenschaft ursprünglichen\_{0}. Dies muss zurückgesetzt werden, auf den Standardwert von {0} in der Reihenfolge für die Änderung-Funktionen ausgeführt, ordnungsgemäß anhand der Parameter, die von den Methoden in unserer BLL erwartet. Aus diesem Grund werden Sie sicher, dass die `OldValuesParameterFormatString` Eigenschaft, um {0} oder entfernen Sie die Eigenschaft vollständig vom deklarative Syntax.

Nach Abschluss des Konfigurieren von Datenquellen-Assistenten, bearbeiten und löschen-Unterstützung in der GridView und Zurückgeben der ObjectDataSource s einschalten `OldValuesParameterFormatString` -Eigenschaft auf den Standardwert, Ihrer deklarativen Markup der Seite "s" sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

Zu diesem Zeitpunkt konnten wir die GridView aufräumen, durch Anpassen der Benutzeroberfläche der Bearbeitung Einbeziehung Überprüfung, mit der `CategoryID` und `SupplierID` Spalten als DropDownLists rendern, und so weiter. Wir konnten die Schaltfläche "löschen" auch eine Bestätigung für die clientseitige hinzu, und überzeugen Sie sich auf die Zeit zum implementieren diese Erweiterungen in Anspruch nehmen. Seit der vorherigen Lernprogrammen in diesen Themen behandelt wurde, wird jedoch nicht diese erneut hier beschrieben.

Unabhängig davon, ob Sie die GridView oder nicht optimiert haben testen Sie die Seite "s"-Kernfunktionen in einem Browser aus. Wie in Abbildung 24 gezeigt, enthält die Seite Produkte in eine GridView, die pro Zeile bearbeiten und Löschen von Funktionen bereitstellt.


[![Die Produkte können angezeigt, bearbeitet und aus der GridView gelöscht werden](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Abbildung 24**: die Produkte können angezeigt werden, bearbeitet und aus der GridView gelöscht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Zusammenfassung

Die TableAdapters in einem typisierten DataSet können Sie Daten aus der Datenbank mithilfe von Ad-hoc-SQL-Anweisungen oder gespeicherte Prozeduren zugreifen. Beim Arbeiten mit gespeicherten Prozeduren, können entweder vorhandenen gespeicherten Prozeduren verwendet werden oder kann der TableAdapter-Assistenten zum Erstellen neuer angewiesen werden gespeicherte Prozeduren, die auf der Grundlage einer `SELECT` Abfrage. In diesem Lernprogramm wurde wir untersucht, wie Sie die gespeicherten Prozeduren, die automatisch für uns erstellt haben.

Während Sie eine der gespeicherten Prozeduren, die automatisch generierte hilft Zeit zu sparen, gibt es bestimmte Fälle, in dem die gespeicherte Prozedur erstellt, indem der Assistent verfügt über t was wir unser eigenes erstellt haben würden ausgerichtet. Ein Beispiel ist die `Products_Update` gespeicherte Prozedur, die beide erwartet `@Original_ProductID` und `@ProductID` Eingabeparameter, obwohl die `@Original_ProductID` Parameter war überflüssig.

In vielen Szenarien die gespeicherten Prozeduren wurden möglicherweise bereits erstellt, oder wir diese manuell, um eine feinere Kontrolle über die gespeicherte Prozedur s Befehle haben erstellen möchten. In jedem Fall möchten wir anweisen des TableAdapter bereits vorhandener gespeicherte Prozeduren für dessen Methoden zu verwenden. Wir sehen, wie dies in den nächsten Lernprogrammen zu erreichen.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen und Verwalten von gespeicherten Prozeduren](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQLServer gespeicherte Prozedur-Grundlagen](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Gespeicherte Prozeduren: Übersicht](http://www.sqlteam.com/item.asp?ItemID=563)
- [Schreiben Sie eine gespeicherte Prozedur](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Geisenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
