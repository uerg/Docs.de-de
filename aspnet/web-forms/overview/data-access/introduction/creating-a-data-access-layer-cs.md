---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Erstellen einer Datenzugriffsschicht (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wir ganz von vorn beginnen und die Daten Datenzugriffsebene (DAL), verwenden typisierte DataSets, Zugriff auf die Informationen in einer Datenbank zu erstellen.
ms.author: riande
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 4a6942df142ef40b92a8461afbc5f61fdfbde4ba
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312395"
---
<a name="creating-a-data-access-layer-c"></a>Erstellen einer Datenzugriffsschicht (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> In diesem Tutorial wir ganz von vorn beginnen und die Daten Datenzugriffsebene (DAL), verwenden typisierte DataSets, Zugriff auf die Informationen in einer Datenbank zu erstellen.


## <a name="introduction"></a>Einführung

Unser Leben ausgetauschten als Webentwickler mit Daten arbeiten. Wir erstellen Datenbanken zum Speichern der Daten und Code zum Abrufen und bearbeiten, und die Webseiten zum Sammeln und Zusammenfassen der Daten. Dies ist das erste Tutorial in einer langen Reihe, die Techniken zum Implementieren von gemeinsamen Mustern in ASP.NET 2.0 vorgestellt wird. Wir beginnen mit dem Erstellen einer [Softwarearchitektur](http://en.wikipedia.org/wiki/Software_architecture) besteht aus der eine Daten Datenzugriffsebene (DAL) mithilfe von typisierten DataSets, eine Geschäftslogikschicht (Business Logic Layer, BLL) erzwingt, dass benutzerdefinierte Geschäftsregeln und eine Darstellungsschicht ASP.NET besteht aus Seiten, Verwenden Sie ein gebräuchliches Seitenlayout. Sobald diese Back-End-Grundlagen in die berichterstellung, weiter angeordnet wurde, zeigt, wie Sie angezeigt werden, zusammenfassen, sammeln und Überprüfen von Daten aus einer Webanwendung. Diese Lernprogramme sind darauf ausgerichtet, präzise und enthalten schrittweise Anleitungen mit zahlreichen Screenshots, die Sie visuell durch den Prozess geführt. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und umfasst ein Download, der den vollständigen Code verwendet. (In diesem ersten Tutorial ist ziemlich umfassend, aber der Rest wird angezeigt, in Blöcken von viel mehr verständlichste.)

Für diese Tutorials wir verwenden eine Microsoft SQL Server 2005 Express Edition-Version der Northwind-Datenbank gespeichert, der **App\_Daten** Verzeichnis. Zusätzlich zu der Datei die **App\_Daten** Ordner enthält auch die SQL-Skripts zum Erstellen der Datenbank, für den Fall, dass Sie eine andere Datenbank-Version verwenden möchten. Diese Skripts kann auch sein [direkt von Microsoft heruntergeladen](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), wenn Sie möchten. Wenn Sie eine andere SQL Server-Version der Northwind-Datenbank verwenden, müssen Sie aktualisieren die **NORTHWNDConnectionString** festlegen in der Anwendung **"Web.config"** Datei. Die Webanwendung wurde mithilfe von Visual Studio 2005 Professional Edition als Datei System-basierte-Website-Projekt erstellt. Alle in den Tutorials funktionieren jedoch gleichermaßen gut mit der kostenlosen Version von Visual Studio 2005 [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
In diesem Tutorial wir ganz von vorn beginnen und die Data Access Layer (DAL), gefolgt von der Geschäftslogikschicht (Business Logic Layer, BLL) im zweiten Lernprogramm erstellen und arbeiten auf "Seitenlayout" und Navigation in der dritten erstellen. Die Lernprogramme, die nach dem Erstellen der dritte Parameter auf Grundlage des in der ersten drei festgelegt. Wir haben viel in diesem ersten Tutorial behandelt, so starten Sie Visual Studio, und lassen Sie uns loslegen!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Schritt 1: Erstellen eines Webprojekts, und Verbinden mit der Datenbank

Bevor wir unsere Data Access Layer (DAL) erstellen können, müssen wir eine Website erstellen und Einrichten der Datenbank. Zunächst erstellen eine neue Datei systembasierte ASP.NET-Website. Um dies zu erreichen, finden Sie unter dem Menü "Datei" aus, und wählen Sie die neue Website, die das Dialogfeld Neue Website anzuzeigen. Wählen Sie die Vorlage für ASP.NET Web Site, legen Sie die Dropdownliste den Speicherort auf File System, wählen Sie einen Ordner aus, um die Website zu platzieren und Festlegen der Sprache c#.


[![Erstellen einer neuen System-basierte-Website](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Abbildung 1**: Erstellen einer Website New File System-Based ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image3.png))


Hiermit wird eine neue Website mit einer **"default.aspx"** ASP.NET-Seite und **App\_Daten** Ordner.

Im nächste Schritt werden mit der Website erstellt haben einen Verweis auf die Datenbank im Server-Explorer von Visual Studio hinzufügen. Durch Hinzufügen einer Datenbank zu Server-Explorer können Sie Tabellen, gespeicherte Prozeduren, Sichten und usw. alle Angaben stammen aus Visual Studio hinzufügen. Sie können auch Tabellendaten anzeigen oder erstellen eigene Abfragen entweder manuell oder grafisch über den Abfrage-Generator. Darüber hinaus beim Erstellen wir die typisierte DataSets für die DAL müssen zeigen Visual Studio mit der Datenbank wir aus denen typisierten DataSets erstellt werden soll. Während wir diese Verbindungsinformationen zu diesem Zeitpunkt bereitstellen können, füllt Visual Studio automatisch eine Dropdown-Liste der Datenbanken im Server-Explorer bereits registriert.

Die Schritte für den Server-Explorer mit die Northwind-Datenbank hinzugefügt, hängen davon ab, ob Sie in die SQL Server 2005 Express Edition-Datenbank verwenden möchten die **App\_Daten** Ordner oder wenn Sie eine Microsoft SQL Server 2000 oder 2005 haben Datenbank-Server-Setup, die Sie verwenden möchten.

## <a name="using-a-database-in-theappdatafolder"></a>Verwenden eine Datenbank in TheApp\_DataFolder

Wenn Sie verfügen nicht über eine SQL Server 2000 oder 2005-Datenbankserver für die Verbindung, oder Sie einfach zu vermeiden, dass die Datenbank einen Datenbank-Server hinzufügen möchten, möchten, können Sie die SQL Server 2005 Express Edition-Version der Northwind-Datenbank verwenden, die in der heruntergeladenen Website befindet. e's **App\_Daten** Ordner (**NORTHWND. MDF-Datei**).

Eine Datenbank platziert werden, der **App\_Daten** Ordner wird im Server-Explorer automatisch hinzugefügt. Vorausgesetzt, dass Sie SQL Server 2005 Express Edition auf Ihrem Computer installiert haben, sollten Sie einen Knoten namens NORTHWND sehen. MDF-Datei im Server-Explorer, die Sie erweitern können, und untersuchen die Tabellen, Ansichten, gespeicherten Prozeduren, und So weiter (siehe Abbildung 2).

Die **App\_Daten** Ordner kann auch Microsoft Access enthalten **MDB** -Dateien, die, wie sein Gegenstück in SQL Server im Server-Explorer automatisch hinzugefügt werden. Wenn Sie nicht die SQL Server-Optionen verwenden möchten, können Sie immer [Laden Sie eine Microsoft Access-Version der Northwind-Datenbankdatei](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) und drop in die **App\_Daten** Verzeichnis. Merken Sie jedoch, die nicht von Access-Datenbanken als Funktionsumfang wie SQL Server, und werden nicht in Website-Szenarien verwendet werden soll. Darüber hinaus werden eine Reihe von 35-Lernprogramme bestimmte Funktionen auf Datenbankebene verwendet, die durch den Zugriff nicht unterstützt werden.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Herstellen einer Verbindung mit der Datenbank auf einem Microsoft SQL Server 2000 oder 2005-Datenbankserver

Alternativ können Sie mit einer Northwind-Datenbank auf einem Datenbankserver installiert verbinden. Wenn der Datenbankserver nicht bereits über die installierte Northwind-Datenbank verfügt, zunächst müssen Sie hinzufügen es mit Datenbank-Server durch Ausführen des Installations-Skripts, die in diesem Tutorial herunterladen oder von [Herunterladen der SQL Server 2000-Version von Northwind und das Installationsskript](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) direkt aus der Microsoft-Website.

Nachdem Sie die Datenbank installiert haben, wechseln Sie zum Server-Explorer in Visual Studio, mit der Maustaste, auf den Knoten "Datenverbindungen" aus, und wählen die Verbindung hinzufügen. Wenn Sie nicht sehen, dass der Server-Explorer navigieren / Server-Explorer oder Strg + Alt + S drücken. Hierdurch wird das Dialogfeld "Verbindung hinzufügen", dort Sie den Server für die Verbindung angeben können, die Authentifizierungsinformationen, und der Name der Datenbank. Nachdem Sie erfolgreich die Datenbankverbindungsinformationen konfiguriert und auf die Schaltfläche "OK" geklickt haben, wird die Datenbank als Knoten unter dem Knoten "Datenverbindungen" hinzugefügt werden. Sie können den Datenbankknoten, um die Tabellen, Sichten, gespeicherte Prozeduren und So weiter zu untersuchen, erweitern.


![Hinzufügen einer Verbindung mit Ihrem Datenbankserver Northwind-Datenbank](creating-a-data-access-layer-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer Verbindung mit Ihrem Datenbankserver Northwind-Datenbank


## <a name="step-2-creating-the-data-access-layer"></a>Schritt 2: Erstellen der Datenzugriffsebene

Bei der Arbeit mit ist eine Option für die Daten der Logik Daten direkt in der Darstellungsschicht (in einer Webanwendung, die ASP.NET Seiten bilden die Darstellungsschicht) einbetten. Dies dauert Form ADO.NET-Code schreiben, in die ASP.NET-Seite Codeteil oder das SqlDataSource-Steuerelement aus dem Markup-Bereich verwenden. In beiden Fällen koppelt dieser Ansatz eng an die Datenzugriffslogik der Darstellungsschicht. Die empfohlene Vorgehensweise, ist jedoch die Datenzugriffslogik von der Darstellungsschicht zu trennen. Diese separate Ebene wird als der Data Access Layer, DAL kurz, bezeichnet und wird in der Regel als ein separates Klassenbibliotheksprojekt implementiert. Die Vorteile dieser Schichtenarchitektur sind gut dokumentiert (siehe Abschnitt "Weitere Literatur" am Ende dieses Tutorials für Informationen zu diesen Vorteilen) und ist der Ansatz, die wir in dieser Serie übernehmen.

Sämtlicher Code, der für die zugrunde liegende Datenquelle z. B. das Erstellen einer Verbindungs mit der Datenbank ausgeben **wählen**, **einfügen**, **UPDATE**, und  **Löschen Sie** Befehle usw. sollten sich in der DAL befinden. Die Darstellungsschicht alle Verweise auf solche Datenzugriffscode sollte nicht enthalten, jedoch sollten Sie stattdessen Aufrufe in die DAL für alle Anforderungen. Datenzugriffsschichten enthalten normalerweise die Methoden für den Zugriff auf die zugrunde liegenden Datenbankdaten. Die Northwind-Datenbank verfügt beispielsweise über **Produkte** und **Kategorien** Tabellen, mit denen die Produkte für den Verkauf und die Kategorien, zu dem sie gehören. In der DAL verfügen wir Methoden wie z.B.:

- **GetCategories(),** wodurch Informationen für alle Kategorien
- **GetProducts()**, wodurch Informationen für alle Produkte
- **GetProductsByCategoryID (*CategoryID*)**, wodurch alle Produkte, die eine angegebene Kategorie angehören,
- **GetProductByProductID (*"ProductID"*)**, die Informationen zu einem bestimmten Produkt zurück

Diese Methoden, wenn aufgerufen, werden eine Verbindung mit der Datenbank herstellen, geben Sie die entsprechende Abfrage und die Ergebnisse zurückgeben. Es ist wichtig, wie wir diese Ergebnisse zurückgeben. Diese Methoden können einfach zurückgeben, ein DataSet oder eine DataReader, die von der Datenbankabfrage gefüllt, aber im Idealfall diese Ergebnisse zurückgegeben werden sollen mit *stark typisierte Objekte*. Ein stark typisiertes Objekt ist fest, deren Schema zum Zeitpunkt der Kompilierung definiert wird, während das Gegenteil, ein lose typisiertes Objekt, einer ist, deren Schema erst zur Laufzeit nicht bekannt ist.

Beispielsweise sind den DataReader und DataSet (standardmäßig) lose typisierte Objekte, da ihr Schema durch die von der Datenbankabfrage verwendet, um sie aufzufüllen zurückgegebenen Spalten definiert wird. Auf eine bestimmte Spalte einer lose typisierte DataTable wir verwenden eine Syntax wie müssen:  <strong><em>DataTable</em>. Zeilen [<em>Index</em>] ["<em>ColumnName</em>"]</strong>. Der DataTable lose typbindung, in diesem Beispiel wird durch die Tatsache zu verzeichnen, die den Spaltennamen, die mit einer Zeichenfolge oder Ordinalindex zugreifen müssen. Eine stark typisierte DataTable andererseits, müssen alle zugehörigen Spalten, die als Eigenschaften implementiert im Code, der aussieht wie resultierende:  <strong><em>DataTable</em>. Zeilen [<em>Index</em>]. *ColumnName</strong>*.

Um stark typisierte Objekte zurückzugeben, können Entwickler eigene benutzerdefinierte Business-Objekte erstellen oder Verwenden von typisierten DataSets. Ein Geschäftsobjekt wird vom Entwickler implementiert, wie eine Klasse, deren Eigenschaften in der Regel die Spalten der zugrunde liegenden Datenbanktabelle das Geschäftsobjekt, das entsprechend, darstellt. Eine typisierte DataSet ist eine Klasse, die von Visual Studio auf der Grundlage eines Datenbankschemas und, deren Mitglieder fester typbindung gemäß diesem Schema sind für Sie generiert. Das typisierte DataSet selbst besteht aus Klassen, die ADO.NET DataSet-, DataTable- und DataRow-Klassen zu erweitern. Zusätzlich zu stark typisierte DataTable umfassen typisierter DataSets nun auch TableAdapters, die Klassen mit Methoden zum Auffüllen Datasetss, DataTables und das Weitergeben von Änderungen am vorhandenen DataTables an die Datenbank sind.

> [!NOTE]
> Weitere Informationen zu den vor- und Nachteile der Verwendung von typisierten DataSets im Vergleich zu benutzerdefinierten Geschäftsobjekte, finden Sie unter [Datenschichtenkomponenten entwerfen und die Übergabe über Datenebenen](https://msdn.microsoft.com/library/ms978496.aspx).


Wir verwenden die stark typisierte DataSets für diese Tutorials Architektur. Abbildung 3 zeigt den Workflow zwischen den verschiedenen Ebenen einer Anwendung, die typisierte DataSets verwendet.


[![Alle Data Access Code wird an die DAL verwiesen.](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Abbildung 3**: alle Data Access Code verwiesen wird, an die DAL ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Erstellen eines typisierten Datasets und die Tabellenadapter

Um zu beginnen, erstellen die DAL, zunächst das Projekt ein typisiertes DataSet hinzugefügt. Zu diesem Zweck mit der rechten Maustaste auf den Projektknoten im Projektmappen-Explorer, und wählen Sie ein neues Element hinzufügen. Wählen Sie die Option für die Datasets aus der Liste der Vorlagen, und nennen Sie sie **Northwind.xsd**.


[![Wählen Sie Ihr Projekt ein neues DataSet hinzu](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Abbildung 4**: Wählen Sie Ihr Projekt ein neues DataSet hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image10.png))


Nach dem Klicken auf "Add", wenn Sie aufgefordert werden, um das DataSet, das Hinzufügen der **App\_Code** Ordner, wählen Sie "Ja". Klicken Sie dann der Designer für typisierte Datasets angezeigt, und den TableAdapter-Konfigurations-Assistenten startet, sodass Sie Ihre erste TableAdapter mit dem typisierten DataSet hinzufügen.

Eine typisierte DataSet dient als stark typisierte Auflistung von Daten; Es besteht aus stark typisierte DataTable-Instanzen, von denen jeder wiederum stark typisierte DataRow-Instanzen besteht. Wir erstellen eine stark typisierte DataTable für jede von der zugrunde liegenden Datenbanktabellen, die in dieser Reihe von Tutorials arbeiten müssen. Beginnen wir mit dem Erstellen einer DataTable für die **Produkte** Tabelle.

Bedenken Sie, dass stark typisierte DataTable keine Informationen zum Zugriff auf Daten aus ihren zugrunde liegenden Datenbanktabelle beinhalten. Um die Daten zum Füllen der DataTable abzurufen, verwenden wir eine TableAdapter-Klasse, die Funktionen, wie unsere Data Access Layer. Für unsere **Produkte** DataTable des TableAdapter enthält die Methoden **GetProducts()**, **GetProductByCategoryID (*CategoryID*)** und so weiter, die wir von der Darstellungsschicht aufrufen müssen. Der DataTable Rolle besteht darin, wie die stark typisierte Objekte, die zum Übergeben von Daten zwischen den Ebenen dienen.

Zunächst, dass den TableAdapter-Konfigurations-Assistenten dazu aufgefordert, nach welcher Datenbank Sie arbeiten mit auswählen. Die Dropdown-Liste zeigt die Datenbanken im Server-Explorer. Wenn Sie den Server-Explorer nicht die Datenbank Northwind hinzugefügt haben, können Sie die Schaltfläche neue Verbindung zu diesem Zeitpunkt zu diesem Zweck klicken.


[![Wählen Sie die Northwind-Datenbank aus der Dropdown-Liste](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Abbildung 5**: Wählen Sie die Northwind-Datenbank aus der Dropdown-Liste ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image13.png))


Nachdem Sie die Datenbank auswählen und auf Weiter klicken, werden Sie aufgefordert, wenn Sie die Verbindungszeichenfolge in speichern möchten die **"Web.config"** Datei. Durch Speichern der Verbindungszeichenfolge müssen Sie vermeiden, dass es schwer codiert in den TableAdapter-Klassen, die Dinge vereinfacht, wenn die Informationen zur Verbindungszeichenfolge in der Zukunft ändern. Wenn Sie sich entscheiden, um die Verbindungszeichenfolge in der Konfigurationsdatei zu speichern. es befindet sich der **&lt;ConnectionStrings&gt;** Abschnitt möglich [optional verschlüsselte](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) für verbessert Sicherheit oder geänderte später über die neuen ASP.NET 2.0-Eigenschaftenseite innerhalb der IIS GUI-Verwaltungstool, handelt es sich besser für Administratoren ist.


[![Speichern Sie die Verbindungszeichenfolge in "Web.config"](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Abbildung 6**: Speichern der Verbindungszeichenfolge für **"Web.config"** ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image16.png))


Als Nächstes müssen wir das Schema für die erste stark typisierte DataTable zu definieren, und geben Sie die erste Methode für unsere TableAdapter zu verwenden, wenn die stark typisierte DataSet zu füllen. Gleichzeitig werden diese beiden Schritte durchgeführt, durch das Erstellen einer Abfrage, die den Spalten aus der Tabelle zurückgegeben werden, dass in unserem DataTable wiedergegeben werden soll. Am Ende des Assistenten erhalten wir einen Methodennamen an dieser Abfrage. Nach, die durchgeführt wird, kann diese Methode auf unsere Darstellungsschicht aufgerufen werden. Die Methode wird die definierte Abfrage auszuführen und eine stark typisierte DataTable füllen.

Informationen zum Einstieg in die Definition der SQL-Abfrage müssen wir zuerst angeben, wie soll den TableAdapter zum Ausstellen von der Abfrage. Wir können eine Ad-hoc-SQL-Anweisung verwenden, erstellen eine neue gespeicherte Prozedur oder eine vorhandene gespeicherte Prozedur verwenden. Für diese Tutorials verwenden wir die Ad-hoc-SQL-Anweisungen. Finden Sie unter [Brian Noyes](http://briannoyes.net/)des Artikel [Erstellen einer Datenzugriffsschicht mit der DataSet-Designer von Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) ein Beispiel für die Verwendung von gespeicherten Prozeduren.


[![Daten Sie die mithilfe einer Ad-hoc-SQL-Anweisung](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Abbildung 7**: Abfragen der Daten, die mit Ad-hoc-SQL-Anweisungen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image19.png))


An diesem Punkt können wir in der SQL-Abfrage von hand eingeben. Wenn Sie die erste Methode in der TableAdapter erstellen sollen in der Regel, die Abfrage diese Spalten zurück, die in die entsprechende DataTable ausgedrückt werden müssen. Wir erreichen dies durch Erstellen einer Abfrage, die alle Spalten und alle Zeilen aus, gibt die **Produkte** Tabelle:


[![Geben Sie die SQL-Abfrage in das Textfeld ein](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Abbildung 8**: Geben Sie die SQL-Abfrage in das Textfeld ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image22.png))


Klicken Sie alternativ verwenden Sie den Abfrage-Generator, und erstellen Sie die Abfrage grafisch zu, wie in Abbildung 9 gezeigt.


[![Erstellen Sie die Abfrage grafisch dargestellt, über den Abfrage-Editor](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Abbildung 9**: Erstellen der Abfrage grafisch dargestellt, über den Abfrage-Editor ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image25.png))


Nach dem Erstellen der Abfrage, aber bevor Sie fortfahren mit dem nächsten Bildschirm klicken Sie auf die Schaltfläche "Erweiterte Optionen". In Projekten auf Website ist "Generate INSERT-, Update- und Delete-Anweisungen die einzige erweiterte Optionen, die standardmäßig aktiviert; Wenn Sie diesen Assistenten aus einer Klassenbibliothek oder ein Windows-Projekt ausführen wird auch die Option "Vollständigen Parallelität verwenden" ausgewählt werden. Lassen Sie die Option "Verwenden von optimistischer Parallelität" jetzt deaktiviert. In zukünftigen Lernprogrammen untersuchen wir die vollständigen Parallelität.


[![Wählen Sie nur die generieren Insert, Update und Delete-Anweisungen Option](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Abbildung 10**: Wählen Sie nur die generieren Insert, Update und Delete-Anweisungen Option ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image28.png))


Klicken Sie nachdem Sie überprüft haben, die erweiterten Optionen auf Weiter, um mit dem letzten Bildschirm fortfahren. Hier sind wir dazu aufgefordert, wählen Sie die Methoden des TableAdapter hinzu. Es gibt zwei Muster zum Auffüllen von Daten:

- **DataTable füllen** bei diesem Ansatz wird eine Methode erstellt, die in einer Datentabelle als Parameter akzeptiert und füllt anhand der Ergebnisse der Abfrage. Die ADO.NET DataAdapter-Klasse, z. B. implementiert dieses Muster mit der **Fill()** Methode.
- **DataTable zurückgeben** bei diesem Ansatz die Methode erstellt und füllt die DataTable für Sie aus, und gibt zurück, wie die Methoden Wert zurückgeben.

Sie können den TableAdapter, implementieren Sie eine oder beide dieser Muster verwenden. Sie können auch die Methoden, die hier bereitgestellten umbenennen. Lassen Sie beide Kontrollkästchen aktiviert ist, lassen Sie uns, obwohl wir nur das zweite Muster für die gesamte in diesen Tutorials verwenden. Darüber hinaus wir benennen die Recht generischen **GetData** Methode, um **GetProducts**.

Wenn dieses Kontrollkästchen aktiviert, erstellt das letzte Kontrollkästchen "GenerateDBDirectMethods," **Insert()**, **Update()**, und **Delete()** Methoden für den TableAdapter. Wenn Sie diese Option deaktiviert lassen, müssen alle Updates über die TableAdapter Sole **Update()** Methode, die in typisierten Datasets, einer "DataTable", einer einzelnen DataRow oder ein Array von DataRows verwendet. (Haben deaktiviert die "Generate INSERT-, Update- und Delete-Anweisungen" option die erweiterten Eigenschaften in Abbildung 9 dieses Kontrollkästchen, die Einstellung hat keine Auswirkungen.) Wir lassen Sie dieses Kontrollkästchen aktiviert ist.


[![Ändern Sie den Methodennamen von GetData auf GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Abbildung 11**: Ändern Sie den Methodennamen aus **GetData** zu **GetProducts** ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image31.png))


Schließen Sie den Assistenten, indem Sie auf "Fertig stellen". Nach dem Schließen des Assistenten werden wir dem DataSet-Designer zurückgegeben, die DataTable anzeigt, dass wir gerade erstellt haben. Sehen Sie die Liste der Spalten in der **Produkte** DataTable (**"ProductID"**, **ProductName**und so weiter), sowie die Methoden von der  **ProductsTableAdapter** (**Fill()** und **GetProducts()**).


[![Die Produkte DataTable und ProductsTableAdapter wurden für das typisierte DataSet hinzugefügt](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Abbildung 12**: die **Produkte** DataTable und **ProductsTableAdapter** das typisierte DataSet hinzugefügt wurden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image34.png))


An diesem Punkt haben wir eine typisierte DataSet mit einer einzelnen Datentabelle (**Northwind.Products**) und eine stark typisierte DataAdapter-Klasse (**NorthwindTableAdapters.ProductsTableAdapter**) mit einem  **GetProducts()** Methode. Diese Objekte können verwendet werden, Code wie eine Liste aller Produkte auf:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Dieser Code war nicht erforderlich, eine Data Access spezifischen Code schreiben. Wir mussten keine Klassen von ADO.NET zu instanziieren, vorliegenden nicht zum Verweisen auf alle Verbindungszeichenfolgen, SQL-Abfragen oder gespeicherte Prozeduren. Stattdessen stellt der TableAdapter auf niedriger Ebene Datenzugriffscode für uns.

Jedes Objekt, das in diesem Beispiel verwendeten ist auch eine stark typisierte, ermöglicht Visual Studio IntelliSense und die typüberprüfung zur Kompilierzeit angeben. Und alle Datentabellen, die dem TableAdapter vom besten auf Daten der ASP.NET Web-Steuerelemente, z. B. GridView, DetailsView, DropDownList, "CheckBoxList" und verschiedene andere gebunden werden können. Das folgende Beispiel veranschaulicht das Binden von zurückgegebene DataTable der **GetProducts()** Methode, um nur eine mangelnder drei Codezeilen in einer GridView-Ansicht der **Seite\_Load** -Ereignishandler.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Die Liste der Produkte wird in einer GridView-Ansicht angezeigt.](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Abbildung 13**: die Liste der Produkte in einer GridView-Ansicht angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image37.png))


In diesem Beispiel erforderlich, dass wir in unserer ASP.NET-Seite drei Codezeilen schreiben **Seite\_Load** -Ereignishandler, in Zukunft Lernprogramme, die untersucht, wie Sie mit dem ObjectDataSource-Steuerelement deklarativ Abrufen der Daten aus der DAL. Mit dem ObjectDataSource-Steuerelement, wenn wir müssen keinen Code zu schreiben und erhalten Unterstützung paging und Sortierung!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Schritt 3: Hinzufügen von Methoden an die Datenzugriffsebene parametrisiert

An diesem Punkt unsere **ProductsTableAdapter** -Klasse, die jedoch eine Methode verfügt über **GetProducts()**, die alle Produkte in der Datenbank zurückgegeben. Wird mit allen Produkten arbeiten sicherlich nützlich ist, gibt es Zeiten, wenn wir Informationen zu einem bestimmten Produkt oder alle Produkte, die zu einer bestimmten Kategorie gehören abrufen möchten. Um solche Funktionen unserer Data Access Layer hinzuzufügen können wir dem TableAdapter parametrisierte Methoden hinzufügen.

Fügen Sie der **GetProductsByCategoryID (*CategoryID*)** Methode. Zum Hinzufügen einer neuen Methode an die DAL, wechseln Sie zurück zur DataSet-Designer mit der Maustaste in den **ProductsTableAdapter** aus, und wählen Sie die Abfrage hinzufügen.


![Mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen](creating-a-data-access-layer-cs/_static/image38.png)

**Abbildung 14**: mit der rechten Maustaste auf den TableAdapter, und wählen Sie Abfrage hinzufügen


Wir werden zunächst dazu aufgefordert, zu der gibt an, ob der Zugriff auf die Datenbank mit einer Ad-hoc-SQL-Anweisung oder einer neuen oder vorhandenen gespeicherten Prozedur werden sollten. Wählen wir eine Ad-hoc-SQL-Anweisung erneut aus. Als Nächstes werden wir gefragt, welche Art von SQL-Abfrage wir gerne verwenden würden. Da wir alle Produkte zurückzugeben, die eine angegebene Kategorie angehören, möchten, möchten wir Schreiben einer **wählen** -Anweisung die Zeilen zurückgibt.


[![Wählen Sie eine SELECT-Anweisung zu erstellen, die Zeilen zurückgibt](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Abbildung 15**: Wählen Sie zum Erstellen einer **wählen** -Anweisung die Zeilen zurück ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image41.png))


Der nächste Schritt ist die SQL-Abfrage verwendet, um Zugriff auf die Daten zu definieren. Da wir nur solche Produkte zurück, die zu einer bestimmten Kategorie gehören möchten, verwenden ich die gleiche <strong>wählen</strong> -Anweisung vom <strong>GetProducts()</strong>, aber fügen Sie die folgenden <strong>, in denen</strong> Klausel: <strong>, in denen CategoryID = @CategoryID</strong> . Die <strong>@CategoryID</strong> Parameter gibt an, die dem TableAdapter-Assistenten, dass die Methode, die wir erstellen einen Eingabeparameter des entsprechenden Typs (d. h., ein NULL-Werte zulässt Integer) erforderlich sind.


[![Geben Sie eine Abfrage aus, um nur die Produkte in einer angegebenen Kategorie zurückzugeben](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Abbildung 16**: Geben Sie eine Abfrage nur Zurückgeben von Produkten in eine angegebene Kategorie ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image44.png))


Im letzten Schritt, denen wir wählen können, die auf Daten, Muster zugreifen zu verwenden, sowie die Namen der generierten Methoden anpassen. Für das Muster Füllung ändern wir den Namen in <strong>FillByCategoryID</strong> und für die Rückgabe eine "DataTable" Muster zurückgeben (der <strong>erhalten*X</strong>*  Methoden), verwenden wir  <strong>GetProductsByCategoryID</strong>.


[![Wählen Sie die Namen für die TableAdapter-Methoden](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Abbildung 17**: Wählen Sie die Namen für die TableAdapter-Methoden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image47.png))


Nach Abschluss des Assistenten enthält die DataSet-Designer die neuen TableAdapter-Methoden.


![Die Produkte können jetzt werden nach Kategorie abgefragt](creating-a-data-access-layer-cs/_static/image48.png)

**Abbildung 18**: die Produkte können jetzt nach Kategorie abgefragt werden


Ruhe Hinzufügen einer **GetProductByProductID (*"ProductID"*)** Methode, die mit derselben Technik.

Diese parametrisierten Abfragen können direkt aus dem DataSet-Designer getestet werden. Mit der rechten Maustaste auf die Methode in der TableAdapter aus, und wählen Sie die Vorschaudaten. Geben Sie anschließend die Werte für die Parameter verwenden, und klicken Sie auf Vorschau.


[![Diese Produkte gehören zur Kategorie Getränke werden angezeigt.](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Abbildung 19**: Diese Produkte gehören zur Kategorie Getränke werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image51.png))


Mit der **GetProductsByCategoryID (*CategoryID*)** -Methode in der DAL, können jetzt erstellen wir eine ASP.NET-Seite, die nur die Produkte in einer angegebenen Kategorie anzeigt. Im folgenden Beispiel werden alle Produkte, die in der Kategorie "Getränke" sind mit einem **CategoryID** 1.

Beverages.ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Diese Produkte in der Kategorie "Getränke" werden angezeigt.](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Abbildung 20**: die Produkte in der Kategorie "Getränke" werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Schritt 4: Einfügen, aktualisieren und Löschen von Daten

Es gibt zwei Muster, die häufig zum Einfügen, aktualisieren und Löschen von Daten verwendet. Das erste Muster für die die direkte datenbankmuster aufgerufen werden, wenn aufgerufen, umfasst das Erstellen von Methoden, die Problem ein **einfügen**, **UPDATE**, oder **löschen** Befehl das Datenbank, die für ein einzelner Datensatz ausgeführt wird. Solche Methoden werden in der Regel in einer Reihe von skalaren Werten (ganze Zahlen, Zeichenfolgen, boolesche Werte, Datums-/Uhrzeitangaben und So weiter), die entsprechen den Werten, einfügen, aktualisieren oder Löschen von übergeben. Z. B. mit diesem Muster für die **Produkte** Tabelle, die die Delete-Methode in einen ganzzahligen Parameter an, dauern würde, der angibt, die **"ProductID"** des Datensatzes zu löschen, während die Insert-Methode dauern würde, ein Zeichenfolge für die **ProductName**, decimal für die **UnitPrice**, eine ganze Zahl für die **UnitsOnStock**und so weiter.


[![Jede INSERT-, Update- und Delete-Anforderung wird die Datenbank sofort gesendet.](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Abbildung 21**: jede INSERT-, Update- und Delete-Anforderung an die Datenbank sofort gesendet ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image57.png))


Das andere Muster, bezeichnet als Batch Muster aktualisieren, besteht darin, einen gesamten Datasets, DataTable oder Sammlung von DataRows in einem Methodenaufruf zu aktualisieren. Mit diesem Muster Entwickler löscht, einfügt, und ändert die DataRows in einer "DataTable" und übergibt dann diese DataRows oder DataTable in eine Updatemethode. Klicken Sie dann diese Methode listet DataRows übergeben, die bestimmt, ob sie geändert, hinzugefügt, gelöscht oder wurde haben (über die DataRow [RowState-Eigenschaft](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) Wert), und gibt die entsprechende Datenbank-Anforderung für jeden Datensatz.


[![Alle Änderungen werden mit der Datenbank synchronisiert, wenn die Update-Methode aufgerufen wird](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Abbildung 22**: Alle Änderungen werden mit der Datenbank synchronisiert, wenn die Update-Methode aufgerufen wird ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image60.png))


Der TableAdapter updatemusters Batch wird standardmäßig verwendet, unterstützt aber auch das DB-direct-Muster. Da wir die Option "Generate INSERT-, Update- und Delete-Anweisungen in den erweiterten Eigenschaften, beim Erstellen von unserem TableAdapter und ausgewählt die **ProductsTableAdapter** enthält ein **Update()** -Methode die das Batch-Update-Muster implementiert. Insbesondere der TableAdapter enthält ein **Update()** -Methode, die das typisierte DataSet, eine stark typisierte DataTable oder ein oder mehrere DataRows übergeben werden kann. Wenn Sie bleibt das Kontrollkästchen "GenerateDBDirectMethods" überprüft beim ersten Erstellung des TableAdapter das DB-direct-Muster auch über implementiert wird **Insert()**, **Update()**, und **Delete()**  Methoden.

Beide Änderungen Datenmustern Verwenden des TableAdapter **InsertCommand**, **UpdateCommand**, und **DeleteCommand** Eigenschaften zum Ausgeben ihrer **einfügen** , **UPDATE**, und **löschen** Befehle aus, um die Datenbank. Können Sie überprüfen und Ändern der **InsertCommand**, **UpdateCommand**, und **DeleteCommand** Eigenschaften, indem Sie durch Klicken auf den TableAdapter im DataSet-Designer, und klicken Sie dann um das Eigenschaftenfenster. (Stellen Sie sicher, dass Sie ausgewählt haben, den TableAdapter, und dass die **ProductsTableAdapter** Objekt wird in der Dropdown-Liste im Fenster Eigenschaften ausgewählt.)


[![Der TableAdapter hat InsertCommand, UpdateCommand und DeleteCommand-Eigenschaften](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Abbildung 23**: der TableAdapter hat **InsertCommand**, **UpdateCommand**, und **DeleteCommand** Eigenschaften ([klicken Sie auf die in voller Größe anzeigen Image](creating-a-data-access-layer-cs/_static/image63.png))


Um zu überprüfen oder ändern Sie diesen Befehl Datenbankeigenschaften, klicken Sie auf die **CommandText** untergeordnete Eigenschaft, die in den Abfrage-Generator angezeigt wird.


[![Konfigurieren Sie die INSERT-, Update- und DELETE-Anweisungen im Abfrage-Generator](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Abbildung 24**: Konfigurieren der **einfügen**, **UPDATE**, und **löschen** Anweisungen im Abfrage-Generator ([klicken Sie, um das Bild in voller Größeanzeigen](creating-a-data-access-layer-cs/_static/image66.png))


Im folgenden Codebeispiel wird veranschaulicht, wie das Batch-Update-Muster verwendet wird, auf den Preis aller Produkte, die nicht eingestellt werden, und deren 25 Einheiten auf Lager oder weniger, das doppelte wird:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Der folgende Code veranschaulicht, wie die DB-direct-Muster verwenden, um programmgesteuert ein bestimmtes Produkt zu löschen und aktualisieren Sie eine aus, und klicken Sie dann ein neues Konto hinzufügen:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Erstellen von benutzerdefinierten INSERT-, Update- und Delete-Methoden

Die **Insert()**, **Update()**, und **Delete()** Methoden, die von der DB-direct-Methode erstellt können ein bisschen aufwändig sein, insbesondere bei Tabellen mit vielen Spalten. Betrachten das vorherige Codebeispiel, ohne IntelliSenses unterstützen, es ist nicht besonders klar, was **Produkte** Tabellenspalte zugeordnet, jeder Eingabeparameter für die **Update()** und **Insert()**  Methoden. Gibt es möglicherweise vorkommen, dass wir nur möchten Aktualisieren einer einzelnen Spalte oder zwei aus, oder möchten eine benutzerdefinierte **Insert()** -Methode, die vielleicht wird den Wert des neu eingefügten Datensatzes zurückgeben **Identität** (automatisch inkrementierten) Feld.

Um eine solche benutzerdefinierte Methode erstellen, geben Sie an der DataSet-Designer zurück. Mit der rechten Maustaste auf den TableAdapter, und wählen Sie die Abfrage hinzufügen, Rückgabe an den TableAdapter-Assistenten aus. Geben wir den Typ der Abfrage zu erstellen, auf dem zweiten Bildschirm. Erstellen wir eine Methode, ein neues Produkt hinzugefügt und gibt anschließend den Wert von des neu hinzugefügten Datensatzes **"ProductID"**. Aus diesem Grund erstellen Sie wahlweise eine **einfügen** Abfrage.


[![Erstellen Sie eine Methode, um die Products-Tabelle eine neue Zeile hinzuzufügen](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Abbildung 25**: Erstellen Sie eine Methode, um eine neue Zeile hinzuzufügen der **Produkte** Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image69.png))


Auf dem nächsten Bildschirm die **InsertCommand**des **CommandText** angezeigt wird. Erweitern Sie diese Abfrage durch Hinzufügen von **Bereich auswählen\_IDENTITY()** am Ende der Abfrage, wodurch den letzten Identitätswert eingefügte ein **Identität** Spalte im selben Bereich. (Finden Sie unter den [technische Dokumentation](https://msdn.microsoft.com/library/ms190315.aspx) für Weitere Informationen zu **Bereich\_IDENTITY()** und aus welchem Grund Sie wahrscheinlich möchten [verwenden Bereich\_IDENTITY() statt @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Stellen Sie sicher, dass Sie am Ende der **einfügen** -Anweisung mit einem Semikolon vor dem Hinzufügen der **wählen** Anweisung.


[![Erweitern Sie die Abfrage, um die SCOPE_IDENTITY()-Funktion-Wert zurückgeben](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Abbildung 26**: Erweitern Sie die Abfrage auf die Rückgabe der **Bereich\_IDENTITY()** Wert ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image72.png))


Benennen Sie abschließend die neue Methode **InsertProduct**.


[![Legen Sie den neuen Methodennamen InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Abbildung 27**: Legen Sie den Namen der neuen Methode **InsertProduct** ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image75.png))


Wenn Sie zurück zu DataSet-Designer sehen Sie, dass die **ProductsTableAdapter** enthält eine neue Methode **InsertProduct**. Wenn diese neue Methode einen Parameter für jede Spalte besitzt die **Produkte** Tabelle, die Chancen stehen, haben Sie vergessen, beenden die **einfügen** -Anweisung mit einem Semikolon. Konfigurieren der **InsertProduct** Methode und sicherzustellen, dass Sie eine durch Semikolons als Trennzeichen das **einfügen** und **wählen** Anweisungen.

Fügen Sie in der Standardeinstellung Methoden Problem keine Methoden, was bedeutet, dass sie die Anzahl der betroffenen Zeilen zurück. Allerdings möchten wir die **InsertProduct** Methode, um den Rückgabewert von der Abfrage nicht die Anzahl der betroffenen Zeilen zurückzugeben. Passen Sie zu diesem Zweck die **InsertProduct** Methode **ExecuteMode** Eigenschaft, um **Skalar**.


[![Ändern Sie die ExecuteMode-Eigenschaft an skalaren](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Abbildung 28**: Ändern der **ExecuteMode** Eigenschaft **Skalar** ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image78.png))


Der folgende code zeigt, die diese neue **InsertProduct** -Methode in Aktion:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Schritt 5: Abschließen der Datenzugriffsschicht

Beachten Sie, dass der **ProductsTableAdapters** Klasse gibt die **CategoryID** und **SupplierID** Werte aus der **Produkte** Tabelle jedoch enthält nicht die **"CategoryName"** Spalte aus der **Kategorien** Tabelle oder die **CompanyName** Spalte aus der **Lieferanten**Tabelle, obwohl dies wahrscheinlich die Spalten wir angezeigt sind, wenn Produktinformationen angezeigt werden soll. Können wir die Methode des TableAdapter anfänglichen, verbessern **GetProducts()**, um beide enthalten den **"CategoryName"** und **CompanyName** Spaltenwerte, das aktualisiert die stark typisierte DataTable auch diese neuen Spalten eingeschlossen werden soll.

Dies kann ein Problem darstellen, jedoch als dem TableAdapter-Methoden zum Einfügen, aktualisieren, und Löschen von Daten aus dieser Methode für erste basieren. Zum Glück, die automatisch generierten Methoden für einfügen, aktualisieren und Löschen von nicht betroffen Unterabfragen in den **wählen** Klausel. Durch sorgen, dass unsere Abfragen hinzufügen **Kategorien** und **Lieferanten** als Unterabfragen, statt **JOIN** s, wir werden zu vermeiden, dass diese Methoden zum Ändern von Daten zu überarbeiten. Mit der rechten Maustaste auf die **GetProducts()** -Methode in der die **ProductsTableAdapter** und wählen Sie konfigurieren. Passen Sie dann die **wählen** Klausel, damit sie aussieht wie:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aktualisieren Sie die SELECT-Anweisung für die GetProducts()-Methode](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Abbildung 29**: Aktualisieren der **wählen** -Anweisung für die **GetProducts()** Methode ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image81.png))


Nach der Aktualisierung der **GetProducts()** Methode verwenden Sie diese neue Abfrage, die die DataTable umfasst zwei neue Spalten: **"CategoryName"** und **Lieferantenname**.


![DataTable-Produkte weist zwei neue Spalten](creating-a-data-access-layer-cs/_static/image82.png)

**Abbildung 30**: die **Produkte** wurde, verfügt über zwei neue Spalten


Aktualisieren in Ruhe die **wählen** -Klausel in der **GetProductsByCategoryID (*CategoryID*)** -Methode.

Wenn Sie aktualisieren die **GetProducts()** **wählen** mit **JOIN** Syntax der DataSet-Designer wird nicht in der Lage, die Methoden zum Einfügen, aktualisieren und Löschen von automatisch generieren Datenbankdaten, die mit dem direkten DB-Muster. Stattdessen müssen Sie manuell viel erstellen wie mit der **InsertProduct** Methode weiter oben in diesem Tutorial. Darüber hinaus manuell müssen Sie geben die **InsertCommand**, **UpdateCommand**, und **DeleteCommand** Eigenschaftswerte, wenn Sie den Batch, aktualisieren die Muster verwenden möchten.

## <a name="adding-the-remaining-tableadapters"></a>Die verbleibenden TableAdapter hinzufügen

Bis jetzt haben wir nur erläutert, mit der Verwendung von einem einzelnen TableAdapter für eine einzelne Datenbanktabelle. Die Northwind-Datenbank enthält jedoch mehrere verknüpfte Tabellen, die wir zum Arbeiten benötigen mit in unserer Webanwendung. Eine typisierte DataSet mehrere darf im Zusammenhang mit Datentabellen. Zum Abschließen der DAL müssen wir daher DataTables für die anderen Tabellen hinzufügen, die wir in diesen Tutorials verwenden. Um einen neuen TableAdapter typisiertes DataSet hinzuzufügen, öffnen Sie den DataSet-Designer, mit der rechten Maustaste im Designer und wählen Add / TableAdapter. Dies erstellt eine neue DataTable und TableAdapter und führen Sie durch den Assistenten, die, den wir zuvor in diesem Tutorial untersucht.

Erstellen Sie die folgenden Methoden, die mithilfe der folgenden Abfragen und TableAdapters einige Minuten dauern. Beachten Sie, dass die Abfragen in der **ProductsTableAdapter** enthalten die Unterabfragen, um die Kategorie und Lieferanten des Produkts-Namen abrufen. Darüber hinaus, wenn Sie folgen regelmäßig, Sie haben bereits hinzugefügt der **ProductsTableAdapter** Klasse **GetProducts()** und **GetProductsByCategoryID (*CategoryID* )** Methoden.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![Der DataSet-Designer, nachdem die vier TableAdapter hinzugefügt wurden](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Abbildung 31**: der DataSet-Designer nach der vier TableAdapters wurden hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Hinzufügen von benutzerdefiniertem Code mit der Datenzugriffsschicht

Die TableAdapters und Datentabellen, die das typisierte DataSet hinzugefügt wird als eine XML-Schemadefinitionsdatei (**Northwind.xsd**). Sie können diese Schemainformationen anzeigen, indem Sie mit der rechten Maustaste auf die **Northwind.xsd** Datei im Projektmappen-Explorer, und wählen Sie Code anzeigen.


[![Die XML-Schema (XSD-Datei) für die Employees typisiertes DataSet](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Abbildung 32**: The XML Schema Definition (XSD)-Datei für das Employees typisierten DataSet ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image88.png))


Diese Schemainformationen wird zur Entwurfszeit in c# oder Visual Basic-Code übersetzt, bei der Kompilierung oder zur Laufzeit (falls erforderlich), wodurch Sie es mit dem Debugger schrittweise durchlaufen können. Zum Anzeigen dieser automatisch generierter Code ausfallen, der Klassenansicht und einen Drilldown für die TableAdapter oder typisierte DataSet-Klassen. Wenn Sie die Klassenansicht, auf dem Bildschirm nicht angezeigt wird, finden Sie unter dem Menü "Ansicht" und wählen Sie ihn von dort aus, oder drücken Sie STRG + UMSCHALT + C. In der Klassenansicht sehen Sie die Eigenschaften, Methoden und Ereignisse von typisierte DataSet und TableAdapter-Klassen. Um den Code für eine bestimmte Methode anzuzeigen, doppelklicken Sie auf den Methodennamen in der Klassenansicht oder mit der rechten Maustaste darauf, und Gehe zu Definition auswählen.


![Überprüfen Sie den automatisch generierten Code durch Auswählen von Go-Definition aus der Klassenansicht](creating-a-data-access-layer-cs/_static/image89.png)

**Abbildung 33**: Überprüfen Sie den automatisch generierten Code durch Auswählen von Go-Definition aus der Klassenansicht


Während der automatisch generierten Code eine große Zeitersparnis sein kann, wird der Code ist häufig sehr allgemein angelegt und muss angepasst werden, um die speziellen Anforderungen einer Anwendung zu erfüllen. Das Risiko der Erweiterung von automatisch generiertem Code ist jedoch, dass das Tool, das den Code generiert hat, dass es Zeit, zu "neu generieren", und Ihre Anpassungen überschreiben ist ggf. ein. Mit .NET 2.0 neue Teilklasse-Konzept ist es einfach, eine Klasse auf mehrere Dateien aufgeteilt. Dadurch kann wir unseren eigenen Methoden, Eigenschaften und Ereignisse, die automatisch generierten Klassen hinzufügen, ohne Informationen zu Visual Studio, unserer Anpassungen zu überschreiben.

Um die DAL anpassen zu demonstrieren, fügen Sie eine **GetProducts()** Methode, um die **SuppliersRow** Klasse. Die **SuppliersRow** Klasse stellt einen einzelnen Datensatz in die **Lieferanten** Tabelle; jeder Lieferant können Anbieter 0 (null) viele Produkte, damit **GetProducts()** gibt diese zurück Produkte, des angegebenen Lieferanten. Zum Ausführen, die dadurch erstellt einer neue Klassendatei in der **App\_Code** Ordner mit dem Namen **SuppliersRow.cs** und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Diese partielle Klasse weist den Compiler, die bei Erstellen der **Northwind.SuppliersRow** Klasse einbeziehen, die **GetProducts()** Methode, die soeben definiert. Wenn Sie Ihr Projekt erstellen und dann in die Klassenansicht zurück sehen Sie **GetProducts()** jetzt als eine Methode aufgeführt **Northwind.SuppliersRow**.


![Die GetProducts()-Methode ist jetzt Teil der Northwind.SuppliersRow-Klasse](creating-a-data-access-layer-cs/_static/image90.png)

**Abbildung 34**: die **GetProducts()** Methode ist jetzt Teil der **Northwind.SuppliersRow** Klasse


Die **GetProducts()** Methode kann jetzt verwendet werden, zum Auflisten der Satz von Produkten für einen bestimmten Lieferanten, wie im folgenden Code gezeigt:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Diese Daten können auch in einer ASP angezeigt werden. NET Daten, die Websteuerelemente. Die folgende Seite verwendet ein GridView-Steuerelement mit zwei Feldern:

- Eine BoundField-, die den Namen der einzelnen Lieferanten anzeigt und
- Ein TemplateField, die ein BulletedList-Steuerelement enthält, die von der zurückgegebenen Ergebnisse gebunden ist die **GetProducts()** Methode für jeden Lieferanten.

Vorgehensweise beim Anzeigen von solchen Master / Detail-Berichte in zukünftigen Lernprogrammen untersucht. Jetzt in diesem Beispiel soll veranschaulichen, verwenden die benutzerdefinierte Methode hinzugefügt, die **Northwind.SuppliersRow** Klasse.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Unternehmensname des Lieferanten ist in der linken Spalte, deren Produkte in der rechten Seite aufgeführt.](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Abbildung 35**: Unternehmensname für die Lieferanten finden Sie in der linken Spalte, deren Produkte in der rechten Seite ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Zusammenfassung

Wenn das Erstellen einer Webanwendung, erstellen die DAL sollte einer der ersten Arbeitsschritte, bevor Sie mit dem Erstellen Ihrer Darstellungsebene beginnen. Mit Visual Studio ist das Erstellen einer DAL, die auf typisierten DataSets basiert eine Aufgabe, die innerhalb von 10 bis 15 Minuten ausgeführt werden kann, ohne eine einzige Zeile Code schreiben zu müssen. Die Lernprogramme, die für die Zukunft werden auf diesem DAL aufbauen. In der [nächsten Tutorial](creating-a-business-logic-layer-cs.md) wir definieren eine Reihe von Geschäftsregeln und erfahren Sie, wie sie in einer separaten Geschäftslogikebene implementiert.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Erstellen einer DAL, die mithilfe von stark typisierten TableAdapters und Datentabellen in Visual Studio 2005 und ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Entwerfen von Datenebenenkomponenten und Weitergabe von Daten über die Tarife](https://msdn.microsoft.com/library/ms978496.aspx)
- [Erstellen einer Datenzugriffsschicht mit der DataSet-Designer von Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Verschlüsseln von Konfigurationsinformationen in ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Übersicht über TableAdapters](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Arbeiten mit einem typisierten DataSet](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Mithilfe von stark typisierten Datenzugriff in Visual Studio 2005 und ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Erweitern der TableAdapter-Methoden](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Abrufen von skalaren Daten aus einer gespeicherten Prozedur](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>In den Themen in diesem Tutorial-Videotraining

- [Datenzugriffsschichten in ASP.NET-Anwendungen](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Gewusst wie: Binden Sie ein Dataset manuell an ein Datenraster](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Wie für die Arbeit mit Datasets und Filtern aus einer ASP-Anwendung](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez und Carlos Santos. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Nächste](creating-a-business-logic-layer-cs.md)
