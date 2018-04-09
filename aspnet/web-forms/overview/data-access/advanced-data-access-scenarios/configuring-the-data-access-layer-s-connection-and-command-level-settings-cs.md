---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: Konfigurieren von Einstellungen für die Datenzugriffsebene-Verbindung und Befehlsebene (c#) | Microsoft Docs
author: rick-anderson
description: Die TableAdapters innerhalb eines typisierten Datasets kümmern automatisch Befehle ausgibt, Herstellen einer Verbindung mit der Datenbank und Auffüllen einer DataTable mit den Ergebnissen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f221fd792956fc21cb6eb5834299d3c5bfa80d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>Konfigurieren von Einstellungen für die Datenzugriffsebene-Verbindung und Befehlsebene (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip) oder [PDF herunterladen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> Die TableAdapters innerhalb eines typisierten Datasets kümmern automatisch Befehle ausgibt, Herstellen einer Verbindung mit der Datenbank und Auffüllen einer DataTable mit den Ergebnissen. Es gibt Situationen jedoch wenn wir kümmern Teil dieser detaillierten Informationen selbst, und klicken Sie in diesem Lernprogramm möchten wir die datenbankeinstellungen für die Verbindung und Befehlsebene im TableAdapter zugreifen erfahren, wie Sie.


## <a name="introduction"></a>Einführung

In der gesamten Reihe von Lernprogrammen haben wir typisierter DataSets verwendet, um die Datenzugriffsebene und Geschäftsobjekten unsere geschichteter Architektur zu implementieren. Entsprechend der Anleitung unter dem [ersten Lernprogramm](../introduction/creating-a-data-access-layer-cs.md), die typisierte DataSet s DataTables als Repositorys für Daten, dienen während die TableAdapters als Wrapper fungieren für die Kommunikation mit der Datenbank abrufen und Ändern der zugrunde liegenden Daten. Die TableAdapters kapseln die Komplexität im Zusammenhang mit der Datenbank und erspart uns zum Schreiben von Code zum Herstellen einer Verbindung mit der Datenbank, einen Befehl oder die Ergebnisse in einer "DataTable" auffüllen.

Es gibt vorkommen, allerdings, dass wir müssen in die Tiefen der TableAdapter burrow und Code schreiben, der direkt mit den Objekten ADO.NET funktioniert. In der [Umbruch Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm, z. B. wir Methoden der TableAdapter hinzugefügte für Start-, Commit und ADO.NET Transaktionen ein Rollback. Diese Methoden verwendet eine interne manuell erstellter `SqlTransaction` -Objekt, das die TableAdapter zugewiesen wurde `SqlCommand` Objekte.

In diesem Lernprogramm werden die datenbankeinstellungen für die Verbindung und Befehlsebene im TableAdapter Zugriff auf untersucht. Insbesondere fügen wir der Funktionalität der `ProductsTableAdapter` , ermöglicht den Zugriff auf die zugrunde liegenden Verbindungszeichenfolge und befehlstimeouteinstellungen.

## <a name="working-with-data-using-adonet"></a>Arbeiten mit Daten, die mithilfe von ADO.NET

Microsoft .NET Framework enthält eine Vielzahl von Klassen, die speziell für die Arbeit mit Daten konzipiert. Diese Klassen, die innerhalb der [ `System.Data` Namespace](https://msdn.microsoft.com/library/system.data.aspx), werden als bezeichnet den *ADO.NET* Klassen. Einige der Klassen des wirkungsfelds ADO.NET gebunden sind, zu einem bestimmten *Datenanbieter*. Sie können einen Datenanbieter als Kommunikationskanal vorstellen, die den Informationsfluss zwischen den ADO.NET-Klassen und dem zugrunde liegenden Datenspeicher ermöglicht. Es sind generalisierte Anbieter, z. B. OLE DB und ODBC sowie Anbieter, die speziell für eine bestimmte Datenbank-Managementsystem ausgelegt sind. Angenommen, während es bei der Herstellung einer Verbindung mit einer Microsoft SQL Server-Datenbank, die mithilfe der OLE DB-Anbieter möglich ist, ist der SqlClient-Anbieter wesentlich effizienter wurde entworfen und optimiert, die speziell für SQL Server.

Beim programmgesteuerten Zugriff auf Daten wird das folgende Muster häufig verwendet:

- Herstellen einer Verbindungs mit der Datenbank an.
- Geben Sie einen Befehl.
- Für `SELECT` Abfragen, mit der resultierenden Datensätzen arbeiten.

Es gibt separate ADO.NET-Klassen zum Durchführen jeder dieser Schritte. Um eine Verbindung mit einer Datenbank mithilfe des SqlClient-Anbieters herstellen zu können, verwenden Sie z. B. die [ `SqlConnection` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Um die Umgehung ein `INSERT`, `UPDATE`, `DELETE`, oder `SELECT` Befehl in der Datenbank verwenden die [ `SqlCommand` Klasse](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Mit Ausnahme der [Umbruch Datenbankänderungen innerhalb einer Transaktion](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) Lernprogramm, wir haben nicht musste ein Schreibvorgang auf niedriger Ebene ADO.NET code uns, da die TableAdapters automatisch generiertem Code erforderlichen Funktionen enthält Herstellen einer Verbindung mit der Datenbank, Befehle, Abrufen von Daten und die Daten in Datentabellen aufzufüllen. Allerdings gibt es möglicherweise vorkommen, dass wir diese Low-Level-Einstellungen anpassen müssen. Über die nächsten Schritte werden wie in ADO.NET-Objekte, die intern verwendet, durch die TableAdapters tippen untersucht.

## <a name="step-1-examining-with-the-connection-property"></a>Schritt 1: Untersuchen, mit der Connection-Eigenschaft

Jede TableAdapter-Klasse verfügt über eine `Connection` Eigenschaft, die Verbindungsinformationen für die Datenbank angibt. Diese Eigenschaft s-Datentyp und `ConnectionString` Wert werden von der Auswahl im TableAdapter-Konfigurations-Assistenten bestimmt. Beachten Sie, dass wir zuerst einen TableAdapter eine typisierte DataSet hinzufügen dieser Assistent uns für die Datenbank fordert source (siehe Abbildung 1). Die Dropdown Liste in diesem ersten Schritt enthält die in der Konfigurationsdatei als auch für alle anderen Datenbanken im Server-Explorer-s-Datenverbindungen angegebenen Datenbanken. Wenn die Datenbank aus, die verwendet werden soll, in der Dropdown-Liste nicht vorhanden ist, kann eine neue datenbankverbindung angegeben werden, durch Klicken auf die Schaltfläche neue Verbindung und die erforderlichen Verbindungsinformationen bereitstellen.


[![Der erste Schritt des TableAdapter-Konfigurations-Assistenten](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**Abbildung 1**: der erste Schritt des TableAdapter-Konfigurations-Assistenten ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


Let s erkundet, überprüfen den Code für die TableAdapter `Connection` Eigenschaft. Wie in notiert die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm können wir den automatisch generierten TableAdapter-Code anzeigen, indem Begriff, das Fenster "Klasse", bis zu der entsprechenden Klasse, und doppelklicken Sie dann auf den Elementnamen.

Navigieren Sie zu dem Fenster "Klasse", indem Sie im Menü "Ansicht" aufrufen und Auswählen der Klassenansicht (oder durch Eingabe von STRG + UMSCHALT + C). In der oberen Hälfte des Fensters Klassenansicht, einen Drilldown nach unten, um die `NorthwindTableAdapters` Namespace, und wählen Sie die `ProductsTableAdapter` Klasse. Dies zeigt die `ProductsTableAdapter` s Mitglieder in der unteren Hälfte der Klassenansicht, wie in Abbildung 2 dargestellt. Doppelklicken Sie auf die `Connection` Eigenschaft, um ihren Code zu sehen.


![Doppelklicken Sie auf die-Verbindungseigenschaft in der Klassenansicht den automatisch generierten Code anzeigen](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**Abbildung 2**: Doppelklicken Sie auf die-Verbindungseigenschaft in der Klassenansicht den automatisch generierten Code anzeigen


Die TableAdapter `Connection` Eigenschaft und jedes andere verbindungsrelevante Code folgt:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

Wenn die TableAdapter-Klasse instanziiert wird, die Membervariable `_connection` gleich `null`. Wenn die `Connection` -Eigenschaft zugegriffen wird, prüft zunächst finden Sie unter der `_connection` Membervariable instanziiert wurde. Wenn dies nicht der Fall, ist die `InitConnection` Methode wird aufgerufen, der instanziiert `_connection` und legt sie fest seine `ConnectionString` Eigenschaft, um den Wert der Verbindungszeichenfolge aus der erste Schritt des TableAdapter-Konfigurations-Assistenten s angegeben.

Die `Connection` Eigenschaft kann auch zugewiesen werden, um eine `SqlConnection` Objekt. Auf diese Weise wird die neue `SqlConnection` Objekt mit den jeweiligen die TableAdapter `SqlCommand` Objekte.

## <a name="step-2-exposing-connection-level-settings"></a>Schritt 2: Verfügbarmachen von Einstellungen auf Verbindungsebene

Die Verbindungsinformationen sollten innerhalb des TableAdapter gekapselten bleiben und nicht auf anderen Ebenen in der Anwendungsarchitektur zugegriffen werden. Allerdings gibt es möglicherweise Szenarien, wenn die TableAdapter s Verbindungsebene Informationen zugegriffen werden kann oder für eine Abfrage, die Benutzer oder die ASP.NET-Seite anpassbar sein muss.

S erweitern lassen die `ProductsTableAdapter` in der `Northwind` DataSet einbezogen eine `ConnectionString` -Eigenschaft, die durch den Business Logic Layer zum Lesen oder Ändern der Verbindungszeichenfolge verwendet, die für die TableAdapter verwendet werden kann.

> [!NOTE]
> Ein *Verbindungszeichenfolge* ist eine Zeichenfolge, die Datenbank-Verbindungsinformationen, z. B. die zu verwendenden Anbieter an, den Speicherort der Datenbank, Anmeldeinformationen für die Authentifizierung und andere Datenbank-bezogene Einstellungen angibt. Eine Liste der Verbindung Zeichenfolgemuster durch eine Vielzahl von Datenspeichern und Anbietern verwendet, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).


Entsprechend der Anleitung unter dem [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm, die typisierte DataSet s automatisch generierten Klassen können erweitert werden, durch die Verwendung von partiellen Klassen. Erstellen Sie zunächst einen neuen Unterordner im Projekt mit dem Namen `ConnectionAndCommandSettings` unterhalb der `~/App_Code/DAL` Ordner.


![Fügen Sie einen Unterordner mit dem Namen ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**Abbildung 3**: Fügen Sie einen Unterordner mit dem Namen `ConnectionAndCommandSettings`


Fügen Sie eine neue Klassendatei mit dem Namen `ProductsTableAdapter.ConnectionAndCommandSettings.cs` , und geben Sie den folgenden Code:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

Diese partielle Klasse fügt eine `public` Eigenschaft mit dem Namen `ConnectionString` auf die `ProductsTableAdapter` Klasse, die eine beliebige Ebene beim Lesen oder aktualisieren die Verbindungszeichenfolge für die zugrunde liegende Verbindung TableAdapter ermöglicht.

Mit dieser partiellen Klasse erstellt (und gespeichert), öffnen Sie die `ProductsBLL` Klasse. Wechseln Sie zu einer der vorhandenen Methoden aus, und geben Sie in `Adapter` , und klicken Sie dann die Period-Taste drücken, um IntelliSense aufzurufen. Daraufhin sollte das neue `ConnectionString` Eigenschaft, die in IntelliSense, was bedeutet, dass Sie programmgesteuert lesen oder passen Sie diesen Wert aus der BLL können verfügbar.

## <a name="exposing-the-entire-connection-object"></a>Verfügbarmachen von dem gesamten Verbindungsobjekt

Diese partielle Klasse macht nur eine der Eigenschaften des zugrunde liegenden Verbindungsobjekts: `ConnectionString`. Wenn Sie das gesamte Verbindungsobjekt außerhalb der Grenzen des TableAdapter verfügbar machen möchten, können Sie alternativ ändern die `Connection` Schutzebene s'-Eigenschaft. Die automatisch generierten Code, der in Schritt 1 untersucht ergab, dass die TableAdapter `Connection` Eigenschaft markiert ist, als `internal`, was bedeutet, dass er nur von Klassen in der gleichen Assembly zugegriffen werden kann. Dies kann geändert werden, jedoch über die TableAdapter `ConnectionModifier` Eigenschaft.

Öffnen der `Northwind` DataSet, klicken Sie auf die `ProductsTableAdatper` im Designer, und navigieren Sie zum Fenster Eigenschaften. Es wird daraufhin die `ConnectionModifier` auf den Standardwert festgelegt `Assembly`. Vornehmen der `Connection` Eigenschaft verfügbar ist, außerhalb der typisierte DataSet s Assembly ändern die `ConnectionModifier` Eigenschaft `Public`.


[![Die Eigenschaft s Zugriff auf Verbindungsebene kann über die Eigenschaft ConnectionModifier konfiguriert werden](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**Abbildung 4**: die `Connection` Eigenschaft s Eingabehilfen Ebene kann so konfiguriert werden, über die `ConnectionModifier` Eigenschaft ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


Speichern Sie das DataSet, und fahren Sie dann mit der `ProductsBLL` Klasse. Bevor, wechseln Sie zu einer der vorhandenen Methoden aus, und geben Sie im `Adapter` , und klicken Sie dann die Period-Taste drücken, um IntelliSense aufzurufen. Die Liste enthalten soll eine `Connection` -Eigenschaft, was bedeutet, dass Sie können nun programmgesteuert lesen oder alle Einstellungen auf Verbindungsebene aus der BLL zugewiesen werden soll.

## <a name="step-3-examining-the-command-related-properties"></a>Schritt 3: Untersuchen der Befehle bezogenen Eigenschaften

Ein TableAdapter besteht aus einer hauptspeicherpool für Abfragen, die standardmäßig automatisch generierten hat `INSERT`, `UPDATE`, und `DELETE` Anweisungen. Diese Hauptabfrage s `INSERT`, `UPDATE`, und `DELETE` Anweisungen werden in den TableAdapter-s-Code implementiert, als ADO.NET Data Adapterobjekt über die `Adapter` Eigenschaft. LIKE mit seiner `Connection` -Eigenschaft, die `Adapter` eigenschaftsdatentyp s richtet sich nach den verwendeten Datenanbieter. Da diese Lernprogramme den SqlClient-Anbieter verwenden die `Adapter` -Eigenschaft ist vom Typ [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

Die TableAdapter `Adapter` Eigenschaft verfügt über drei Eigenschaften des Typs `SqlCommand` , die verwendet wird, um Problem der `INSERT`, `UPDATE`, und `DELETE` Anweisungen:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Ein `SqlCommand` -Objekt ist verantwortlich für eine bestimmte Abfrage an die Datenbank gesendet und verfügt über Eigenschaften, z. B.: [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), enthält die Ad-hoc-SQL-Anweisung oder gespeicherte Prozedur für die Ausführung und [ `Parameters` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), dies ist eine Auflistung von `SqlParameter` Objekte. Wie wir gesehen, wieder in haben die [erstellen eine Datenzugriffsschicht](../introduction/creating-a-data-access-layer-cs.md) Lernprogramm diesen Befehl Objekte über das Fenster "Eigenschaften" angepasst werden können.

Neben der Hauptabfrage des TableAdapter eine Variable Anzahl von Methoden enthalten kann, wenn aufgerufen, verteilen Sie einen angegebenen Befehl aus, um die Datenbank. Die Hauptabfrage s Command-Objekt und die Befehlsobjekte für alle zusätzlichen Methoden werden in die TableAdapter gespeichert `CommandCollection` Eigenschaft.

Let s nehmen einen Moment Zeit, die vom generierten Code betrachten die `ProductsTableAdapter` in die `Northwind` DataSet für diese beiden Eigenschaften und deren unterstützende Membervariablen und Hilfsmethoden:


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

Der Code für die `Adapter` und `CommandCollection` Eigenschaften imitiert eng mit der die `Connection` Eigenschaft. Es gibt Membervariablen, die von den Eigenschaften verwendeten Objekte enthalten. Die Eigenschaften `get` Accessoren starten, indem überprüft wird, um festzustellen, ob die entsprechende Membervariable wird `null`. Wenn dies der Fall ist, wird eine Initialisierungsmethode aufgerufen, die erstellt eine Instanz der Membervariable und weist den Kern Befehle bezogenen Eigenschaften.

## <a name="step-4-exposing-command-level-settings"></a>Schritt 4: Verfügbarmachen von Befehlsebene Einstellungen

Im Idealfall sollte die Befehlsebene Informationen innerhalb der Datenzugriffsebene gekapselten bleiben. Diese Informationen in anderen Ebenen der Architektur benötigt werden sollte, kann allerdings es über eine partielle Klasse, wie mit den Einstellungen auf Verbindungsebene verfügbar gemacht werden.

Da TableAdapter nur eine einzelne hat `Connection` -Eigenschaft, die der Code für das Verfügbarmachen von Einstellungen auf Verbindungsebene ist recht einfach. Folgende Dinge sind etwas komplizierter, wenn Befehlsebene Einstellungen zu ändern, da die TableAdapter mehrere Befehlsobjekte - verfügen kann ein `InsertCommand`, `UpdateCommand`, und `DeleteCommand`, zusammen mit der eine Variable Anzahl von Befehlsobjekten in der `CommandCollection` Diese Eigenschaft. Beim Befehlsebene Einstellungen zu aktualisieren, müssen diese Einstellungen auf alle Befehlsobjekte weitergegeben werden.

Angenommen Sie, dass bestimmte Abfragen gab es in den TableAdapter, die eine außergewöhnliche viel Zeit zum Ausführen ausgeführt hat. TableAdapter mit einen solcher Abfragen ausführen, sollten wir das Befehlsobjekt s erhöhen [ `CommandTimeout` Eigenschaft](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Diese Eigenschaft gibt die Anzahl der Sekunden für die Ausführung des Befehls, und wird standardmäßig auf 30.

Ermöglicht die `CommandTimeout` -Eigenschaft angepasst werden, indem die BLL fügen Sie die folgenden `public` Methode, um die `ProductsDataTable` mithilfe der partiellen Klassendatei in Schritt2 erstellt wurde (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

Diese Methode konnte aus dem BLL oder der Darstellungsschicht festzulegende das Befehlstimeout für alle Befehle Probleme von dieser Instanz des TableAdapter aufgerufen werden.

> [!NOTE]
> Die `Adapter` und `CommandCollection` Eigenschaften werden als `private`, d. h., sie können nur von Code in den TableAdapter aus zugegriffen werden. Im Gegensatz zu den `Connection` -Eigenschaft, diese Zugriffsmodifizierer sind nicht konfigurierbar. Aus diesem Grund Wenn Sie zu anderen Ebenen in der Architektur Befehlsebene-Eigenschaften verfügbar machen müssen müssen Sie verwenden die partielle Klasse Ansatz weiter oben erläuterten, Bereitstellen einer `public` -Methode oder Eigenschaft, die Lese- und Schreibvorgänge, um die `private` Befehlsobjekten.


## <a name="summary"></a>Zusammenfassung

Die TableAdapters innerhalb eines typisierten Datasets dienen zum Kapseln von Daten Zugriffsdetails und Komplexität. Mithilfe von TableAdapters, müssen wir keine Gedanken machen, Schreiben von Code von ADO.NET zum Herstellen einer Verbindung mit der Datenbank, einen Befehl oder die Ergebnisse in einer "DataTable" auffüllen. Es werden alle automatisch für uns behandelt.

Allerdings gibt es möglicherweise vorkommen, dass wir die Low-Level ADO.NET Einzelheiten, z. B. das Ändern der Verbindungszeichenfolge oder die Verbindung oder Befehl Timeout Standardwerte anpassen müssen. Der TableAdapter wurde automatisch generierten `Connection`, `Adapter`, und `CommandCollection` Eigenschaften, doch diese sind entweder `internal` oder `private`, standardmäßig. Diese internen Informationen kann verfügbar gemacht werden, durch die Erweiterung des TableAdapters mit partiellen Klassen einzuschließender `public` Methoden oder Eigenschaften. Alternativ können Sie die TableAdapter `Connection` Zugriffsmodifizierer für die Eigenschaft kann konfiguriert werden, über die TableAdapter `ConnectionModifier` Eigenschaft.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Führen Sie Prüfer für dieses Lernprogramm Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy und Hilton Geisenow wurden. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](working-with-computed-columns-cs.md)
> [Weiter](protecting-connection-strings-and-other-configuration-information-cs.md)
