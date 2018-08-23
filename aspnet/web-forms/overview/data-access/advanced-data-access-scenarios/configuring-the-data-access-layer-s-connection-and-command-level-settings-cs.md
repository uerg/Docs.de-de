---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Konfigurieren von Einstellungen für die Datenzugriffsebene-Verbindung und Befehlsebene (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Die TableAdapters in einem typisierten DataSet kümmert sich automatisch eine Verbindung mit der Datenbank, Ausgeben von Befehlen und eine "DataTable" mit den Ergebnissen aufgefüllt...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: 142c8e93422ac03d2f2205b6635f88b982b4c9e2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837406"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Konfigurieren von Einstellungen für die Datenzugriffsebene-Verbindung und Befehlsebene (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) oder [PDF-Datei herunterladen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Die TableAdapters in einem typisierten DataSet kümmert sich automatisch eine Verbindung mit der Datenbank, Ausgeben von Befehlen und das Auffüllen einer "DataTable" mit den Ergebnissen. Es gibt Situationen, in denen auf, aber wenn wir diese Informationen selbst, und klicken Sie in diesem Tutorial in Ihrer Verantwortung möchten wir die datenbankeinstellungen für die Verbindung und Befehlsebene im TableAdapter zugreifen erfahren, wie Sie.


## <a name="introduction"></a>Einführung

In der tutorialreihe haben wir typisierter DataSets verwendet, zur Implementierung der Datenzugriffsschicht und Geschäftsobjekten unsere Schichtenarchitektur. Siehe die [ersten Tutorial](../introduction/creating-a-data-access-layer-cs.md), die typisierte DataSet-Zuordnungsvorgänge DataTables als Repositorys für Daten, dienen während die TableAdapters als Wrapper fungiert für die Kommunikation mit der Datenbank abgerufen und die zugrunde liegenden Daten geändert. Die TableAdapters kapseln die Komplexität im Zusammenhang mit der Datenbank und erspart uns zum Schreiben von Code zum Verbinden mit der Datenbank, einen Befehl oder die Ergebnisse in eine DataTable füllen.

Es gibt jedoch Situationen, wenn wir in die Tiefe des TableAdapter burrow und Code schreiben, der direkt mit ADO.NET-Objekten arbeitet müssen. In der [Umschließen von Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Tutorial, z. B. wir Methoden der TableAdapter hinzugefügte für Anfang, Commit und Rollback für Transaktionen von ADO.NET. Diese Methoden verwendet eine interne, manuell erstellte `SqlTransaction` -Objekt, das die TableAdapter zugewiesen wurde `SqlCommand` Objekte.

In diesem Tutorial untersuchen wir die Verbindung und Befehlsebene datenbankeinstellungen im TableAdapter zugreifen auf. Insbesondere fügen wir Funktionen für die `ProductsTableAdapter` , die Zugriff auf die zugrunde liegenden Verbindungszeichenfolge und befehlstimeouteinstellungen.

## <a name="working-with-data-using-adonet"></a>Arbeiten mit Daten, die mithilfe von ADO.NET

Microsoft .NET Framework enthält eine Vielzahl von Klassen, die speziell dazu entwickelt, mit Daten arbeiten. Diese Klassen, die innerhalb der [ `System.Data` Namespace](https://msdn.microsoft.com/library/system.data.aspx), werden als bezeichnet die *ADO.NET* Klassen. Einige der Klassen des wirkungsfelds ADO.NET sind verknüpft, auf einen bestimmten *Datenanbieter*. Sie können einen Datenanbieter als Kommunikationskanal vorstellen, die den Informationsfluss zwischen den Klassen von ADO.NET und den zugrunde liegenden Datenspeicher ermöglicht. Es gibt generalisierten Anbieter, z. B. OLE DB und ODBC sowie Anbieter, die speziell für ein bestimmtes Datenbanksystem konzipiert sind. Z. B. während es bei der Herstellung einer Verbindung mit einer Microsoft SQL Server-Datenbank, die mit dem OLE DB-Anbieter möglich ist, entspricht der SqlClient-Anbieter wesentlich effizienter konzipiert und optimiert, die speziell für SQL Server.

Beim programmgesteuerten Zugriff auf Daten wird häufig das folgende Muster verwendet:

- Herstellen einer Verbindungs mit der Datenbank an.
- Führen Sie einen Befehl.
- Für `SELECT` Abfragen, mit der resultierenden Datensätze zu arbeiten.

Es gibt separate ADO.NET-Klassen zum Durchführen jeder dieser Schritte. Verwenden Sie zum Verbinden mit einer Datenbank mit dem SqlClient-Anbieter z. B. die [ `SqlConnection` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Problem ein `INSERT`, `UPDATE`, `DELETE`, oder `SELECT` Befehl in der Datenbank mithilfe der [ `SqlCommand` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Mit Ausnahme der [Umschließen von Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Tutorial hatten wir nicht auf ein Schreibvorgang auf niedriger Ebene ADO.NET code selbst, da die TableAdapters automatisch generiertem Code erforderlichen Funktionen enthält Verbinden mit der Datenbank, führen Sie Befehle, Abrufen von Daten und füllen Sie die Daten in Datentabellen. Allerdings gibt es möglicherweise vorkommen, dass wir benötigen diese Low-Level-Einstellungen anpassen. Untersuchen wir über die nächsten Schritte wie Sie die ADO.NET-Objekte wird intern verwendet, durch die TableAdapters zu nutzen.

## <a name="step-1-examining-with-the-connection-property"></a>Schritt 1: Untersuchen, mit der Connection-Eigenschaft

Jede TableAdapter-Klasse verfügt über eine `Connection` Eigenschaft, die Datenbank-Verbindungsinformationen angibt. Diese Eigenschaft s-Datentyp und `ConnectionString` Wert werden durch die Auswahl im TableAdapter-Konfigurations-Assistenten bestimmt. Denken Sie daran, dass wenn wir einen TableAdapter zum ersten Mal mit einem typisierten DataSet hinzufügen dieser Assistent uns für die Datenbank erwartet, Quelle (siehe Abbildung 1). In diesem ersten Schritt die Dropdown-Liste enthält diese Datenbanken, die in der Konfigurationsdatei als auch für alle anderen Datenbanken im Server-Explorer s Datenverbindungen angegeben. Wenn die Datenbank, die verwendet werden soll, in der Dropdown-Liste nicht vorhanden ist, kann eine neue datenbankverbindung angegeben werden, indem Sie auf die Schaltfläche "neue Verbindung", und die erforderlichen Verbindungsinformationen bereitgestellt.


[![Der erste Schritt des TableAdapter-Konfigurations-Assistenten](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Abbildung 1**: der erste Schritt des TableAdapter-Konfigurations-Assistenten ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Let s können Sie überprüfen den Code für die TableAdapter `Connection` Eigenschaft. Wie erwähnt in der [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial können wir den automatisch generierten TableAdapter-Code anzeigen, indem Sie das Fenster "Klassenansicht", Drilldown auf die entsprechende Klasse, und doppelklicken Sie dann auf den Namen des Members.

Navigieren Sie zu dem Fenster "Klassenansicht", indem Sie das Menü "Ansicht" und Klassenansicht auswählen (oder durch Eingabe von STRG + UMSCHALT + C). In der oberen Hälfte der das Fenster "Klassenansicht" zeigen die `NorthwindTableAdapters` Namespace, und wählen Sie die `ProductsTableAdapter` Klasse. Dadurch wird angezeigt der `ProductsTableAdapter` s Mitglieder in der unteren Hälfte der Klassenansicht, wie in Abbildung 2 dargestellt. Doppelklicken Sie auf die `Connection` Eigenschaft, um den Code anzuzeigen.


![Doppelklicken Sie auf die-Verbindungseigenschaft in der Klassenansicht den automatisch generierten Code anzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Abbildung 2**: Doppelklicken Sie auf die-Verbindungseigenschaft in der Klassenansicht den automatisch generierten Code anzeigen


Die TableAdapter `Connection` -Eigenschaft und die andere Verbindung-bezogenem Code folgt:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Wenn die TableAdapter-Klasse instanziiert wird, ob die Membervariable `_connection` gleich `null`. Wenn die `Connection` -Eigenschaft zugegriffen wird, es zuerst überprüft, wenn die `_connection` Membervariable instanziiert wurde. Ist dies nicht, die `InitConnection` Methode wird aufgerufen, die instanziiert `_connection` und legt seine `ConnectionString` Eigenschaft, um den Wert der Verbindungszeichenfolge aus dem ersten Schritt des TableAdapter-Konfigurations-Assistenten s angegeben.

Die `Connection` Eigenschaft kann auch zugewiesen werden, um eine `SqlConnection` Objekt. Auf diese Weise wird die neue `SqlConnection` Objekt mit den jeweiligen die TableAdapter `SqlCommand` Objekte.

## <a name="step-2-exposing-connection-level-settings"></a>Schritt 2: Verfügbarmachen von Verbindungsebene-Einstellungen

Die Verbindungsinformationen sollten bleiben innerhalb der TableAdapter gekapselten und nicht auf anderen Ebenen der Architektur der Anwendung zugegriffen werden. Allerdings gibt es möglicherweise Szenarios Wenn die TableAdapter-s-Verbindungsebene Informationen zugegriffen werden kann oder für eine Abfrage, die Benutzer oder die ASP.NET-Seite angepasst werden muss.

S erweitern lassen die `ProductsTableAdapter` in die `Northwind` DataSet sollen eine `ConnectionString` -Eigenschaft, die von der Geschäftslogikebene verwendet werden kann, zu lesen oder ändern die Verbindungszeichenfolge, die von der TableAdapter verwendet.

> [!NOTE]
> Ein *Verbindungszeichenfolge* ist eine Zeichenfolge, die Datenbank-Verbindungsinformationen, z. B. den zu verwendenden Anbieter an, den Speicherort der die Datenbank, die Anmeldeinformationen für die Authentifizierung und die andere Datenbank-bezogene Einstellungen angibt. Eine Liste der Verbindung Zeichenfolgenmuster, die von einer Vielzahl von Datenspeichern und der Anbieter verwendet, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).


Siehe die [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial die typisierte DataSet s automatisch generierte Klassen können erweitert werden, durch die Verwendung von partiellen Klassen. Erstellen Sie zunächst einen neuen Unterordner im Projekt mit dem Namen `ConnectionAndCommandSettings` unterhalb der `~/App_Code/DAL` Ordner.


![Fügen Sie einen Unterordner mit dem Namen ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Abbildung 3**: Fügen Sie einen Unterordner mit dem Namen `ConnectionAndCommandSettings`


Fügen Sie eine neue Klassendatei mit dem Namen `ProductsTableAdapter.ConnectionAndCommandSettings.cs` , und geben Sie den folgenden Code:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Diese partielle Klasse fügt eine `public` Eigenschaft mit dem Namen `ConnectionString` auf die `ProductsTableAdapter` -Klasse, können eine beliebige Ebene zum Lesen oder aktualisieren die Verbindungszeichenfolge für die TableAdapter zugrunde liegende Verbindung.

Mit dieser partiellen Klasse erstellt (und gespeichert), öffnen Sie die `ProductsBLL` Klasse. Navigieren Sie zu einem der vorhandenen Methoden aus, und geben Sie in `Adapter` und drücken Sie dann die Punkt-Taste, um IntelliSense aufzurufen. Daraufhin sollte das neue `ConnectionString` Eigenschaft, die in IntelliSense, was bedeutet, dass Sie programmgesteuert lesen oder passen Sie diesen Wert aus der BLL können verfügbar.

## <a name="exposing-the-entire-connection-object"></a>Das gesamte Verbindungsobjekt verfügbar zu machen

Diese partielle Klasse macht nur eine der Eigenschaften des zugrunde liegenden Verbindungsobjekts: `ConnectionString`. Wenn Sie das gesamte Verbindungsobjekt außerhalb der Grenzen des TableAdapter verfügbar machen möchten, können Sie alternativ dazu ändern die `Connection` Schutzebene s'-Eigenschaft. Die automatisch generierten Code in Schritt 1 wir untersuchten zeigen, mit dem die TableAdapter `Connection` Eigenschaft wird als markiert `internal`, was bedeutet, dass er nur von den Klassen in der gleichen Assembly zugegriffen werden kann. Dies kann geändert werden, jedoch über die TableAdapter `ConnectionModifier` Eigenschaft.

Öffnen der `Northwind` DataSet, klicken Sie auf die `ProductsTableAdatper` im Designer, und navigieren Sie zu dem Fenster "Eigenschaften". Es wird Ihnen die `ConnectionModifier` legen Sie auf den Standardwert `Assembly`. Zu den `Connection` Eigenschaft außerhalb der typisierte DataSet-s-Assembly, Änderung zur Verfügung steht die `ConnectionModifier` Eigenschaft `Public`.


[![Die Verbindung Eigenschaft s Zugriffsebene kann über die ConnectionModifier-Eigenschaft konfiguriert werden](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Abbildung 4**: die `Connection` Eigenschaft s Barrierefreiheit Ebene kann so konfiguriert werden, über die `ConnectionModifier` Eigenschaft ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Speichern Sie das DataSet und wieder die `ProductsBLL` Klasse. Wie zuvor, besuchen Sie eine der vorhandenen Methoden aus, und geben Sie in `Adapter` und drücken Sie dann die Punkt-Taste, um IntelliSense aufzurufen. Die Liste sollte enthalten eine `Connection` -Eigenschaft, was bedeutet, dass Sie jetzt programmgesteuert lesen oder können alle Einstellungen auf Verbindungsebene der BLL zuweisen.

## <a name="step-3-examining-the-command-related-properties"></a>Schritt 3: Untersuchen der befehlsbezogenen Eigenschaften

Ein TableAdapter besteht aus einem hauptspeicherpool für Abfragen, die in der Standardeinstellung automatisch generiert wurde `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Diese Hauptabfrage s `INSERT`, `UPDATE`, und `DELETE` Anweisungen werden in den TableAdapter-s-Code implementiert, als ADO.NET Data Adapterobjekt über die `Adapter` Eigenschaft. Wie Sie mit der `Connection` -Eigenschaft, die `Adapter` eigenschaftsdatentyp s richtet sich nach den Datenanbieter verwendet. Da diese Tutorials den SqlClient-Anbieter verwenden die `Adapter` Eigenschaft ist vom Typ [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Die TableAdapter `Adapter` Eigenschaft verfügt über drei Eigenschaften des Typs `SqlCommand` , die es verwendet, um Problem der `INSERT`, `UPDATE`, und `DELETE` Anweisungen:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Ein `SqlCommand` -Objekt ist verantwortlich für eine bestimmte Abfrage an die Datenbank gesendet und verfügt über Eigenschaften, z. B.: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), enthält die Ad-hoc-SQL-Anweisung oder die gespeicherte Prozedur für die Ausführung und [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), dies ist eine Auflistung von `SqlParameter` Objekte. Wie wir, in gesehen der [Erstellen einer Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Tutorial dieser Befehl Objekte können angepasst werden, über das Eigenschaftenfenster.

Zusätzlich zu der Hauptabfrage des TableAdapter kann eine Variable Anzahl von Methoden enthalten, die beim Aufrufen, senden Sie einen angegebenen Befehl mit der Datenbank. Die Hauptabfrage s Command-Objekt und die Befehlsobjekte für alle zusätzlichen Methoden werden in die TableAdapter gespeichert `CommandCollection` Eigenschaft.

Let s können Sie die vom generierten Code betrachten die `ProductsTableAdapter` in die `Northwind` DataSet für diese beiden Eigenschaften und deren unterstützende Membervariablen und Hilfsmethoden:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Der Code für die `Adapter` und `CommandCollection` Eigenschaften imitiert eng mit der die `Connection` Eigenschaft. Es gibt Membervariablen, die die Objekte, die verwendet werden, durch die Eigenschaften enthalten. Die Eigenschaften `get` Accessoren zu starten, indem überprüft wird, ist die entsprechende Membervariable `null`. Wenn dies der Fall ist, wird eine Initialisierungsmethode aufgerufen, die erstellt eine Instanz der Membervariable und weist den Kern befehlsbezogenen Eigenschaften.

## <a name="step-4-exposing-command-level-settings"></a>Schritt 4: Verfügbarmachen von Befehlsebene-Einstellungen

Im Idealfall sollten die Befehlsebene Informationen in der Datenzugriffsebene gekapselten bleiben. Diese Informationen in anderen Ebenen der Architektur benötigt werden sollte, kann jedoch sie mittels einer partiellen Klasse, wie mit den Einstellungen auf Serverebene verfügbar gemacht werden.

Da der TableAdapter nur eine einzelne hat `Connection` -Eigenschaft, die der Code für das Verfügbarmachen von Einstellungen auf Ebene von Verbindung ist recht einfach. Sind ein etwas komplizierteres Befehlsebene Einstellungen zu ändern, da der TableAdapter mehrere Befehlsobjekte - verfügen, kann ein `InsertCommand`, `UpdateCommand`, und `DeleteCommand`, zusammen mit der eine Variable Anzahl von Befehlsobjekten in die `CommandCollection` Diese Eigenschaft. Beim Befehlsebene Einstellungen zu aktualisieren, müssen diese Einstellungen auf alle Befehlsobjekte weitergegeben werden.

Angenommen Sie, dass gab es bestimmte Abfragen in den TableAdapter, die eine außergewöhnliche viel Zeit zum Ausführen benötigt haben. Mit dem TableAdapter eine dieser Abfragen ausführen, sollten wir das Befehlsobjekt s erhöhen [ `CommandTimeout` Eigenschaft](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Diese Eigenschaft gibt die Anzahl der Sekunden warten, bis der auszuführende Befehl, und ist standardmäßig auf 30.

Zu den `CommandTimeout` Eigenschaft angepasst werden, indem die BLL, fügen Sie die folgenden `public` Methode, um die `ProductsDataTable` mithilfe der partiellen Klasse-Datei in Schritt2 erstellte (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Diese Methode konnte von der BLL oder Darstellungsschicht das Befehlstimeout für alle Befehle Probleme Festlegen von dieser Instanz des TableAdapter aufgerufen werden.

> [!NOTE]
> Die `Adapter` und `CommandCollection` Eigenschaften werden als `private`, d. h., sie können nur von Code in der TableAdapter aus zugegriffen werden. Im Gegensatz zu den `Connection` -Eigenschaft, diese Zugriffsmodifizierer sind nicht konfigurierbar. Aus diesem Grund bei Bedarf auf anderen Ebenen der Architektur Befehlsebene-Eigenschaften verfügbar machen Sie müssen die partielle Klasse Ansatz verwenden erwähnte zu einem `public` Methode oder Eigenschaft, die liest oder schreibt in die `private` Befehlsobjekte.


## <a name="summary"></a>Zusammenfassung

Die TableAdapters innerhalb eines typisierten Datasets dienen, zum Datenzugriff und die Komplexität zu kapseln. Mit TableAdapters können wir keine Gedanken um das Schreiben von Code von ADO.NET zum Verbinden mit der Datenbank, einen Befehl oder die Ergebnisse in eine DataTable füllen. Es wird alle automatisch für uns behandelt.

Allerdings gibt es möglicherweise vorkommen, dass wir die Low-Level ADO.NET Einzelheiten benötigen, z. B. das Ändern der Verbindungszeichenfolge oder die standardmäßigen-Verbindung oder eines Befehl Timeoutwerte anpassen. Der TableAdapter hat automatisch generierter `Connection`, `Adapter`, und `CommandCollection` Eigenschaften, aber diese sind entweder `internal` oder `private`, in der Standardeinstellung. Diese internen Informationen kann verfügbar gemacht werden, durch die Erweiterung des TableAdapter mit partiellen Klassen einzuschließender `public` Methoden oder Eigenschaften. Sie können auch die TableAdapter `Connection` Zugriffsmodifizierer für die Eigenschaft kann so konfiguriert werden, über die TableAdapter `ConnectionModifier` Eigenschaft.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führen Sie Prüfer für dieses Tutorial Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy und Hilton Geisenow wurden. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](working-with-computed-columns-cs.md)
> [Weiter](protecting-connection-strings-and-other-configuration-information-cs.md)
