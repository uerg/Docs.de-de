---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Erstellen von gespeicherten Prozeduren und benutzerdefinierten Funktionen mit verwaltetem Code (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Microsoft SQL Server 2005 ist in der .NET Common Language Runtime, die Entwicklern ermöglichen, Erstellen von Datenbankobjekten mithilfe von verwaltetem Code integriert. In diesem Tutorial...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 33ec7cfbb029c1a0e5200eb61aa4e39b02d991f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824415"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Erstellen gespeicherter Prozeduren und benutzerdefinierten Funktionen mit verwaltetem Code (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) oder [PDF-Datei herunterladen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 ist in der .NET Common Language Runtime, die Entwicklern ermöglichen, Erstellen von Datenbankobjekten mithilfe von verwaltetem Code integriert. Dieses Tutorial veranschaulicht das Erstellen von verwalteter gespeicherter Prozeduren und benutzerdefinierte Funktionen, mit dem Visual Basic oder C#-Code verwaltet. Wir sehen auch, wie diese Editionen von Visual Studio ermöglichen Ihnen, diese verwalteten Datenbankobjekte zu debuggen.


## <a name="introduction"></a>Einführung

S für Microsoft SQL Server 2005-Datenbanken verwenden das [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) für das Einfügen, ändern und Abrufen von Daten. Die meisten Datenbanksysteme sind Konstrukte für die Gruppierung einer Reihe von SQL-Anweisungen, die dann als einzelne, wiederverwendbare Einheit ausgeführt werden können. Gespeicherte Prozeduren sind ein Beispiel. Ein weiterer Vorteil ist *User-Defined Functions*(UDFs), ein Konstrukt, das in Schritt 9 genauer untersucht werden.

Im Grunde dient SQL für die Arbeit mit Datasets. Die `SELECT`, `UPDATE`, und `DELETE` Anweisungen grundsätzlich gelten für alle Datensätze in die entsprechende Tabelle und werden nur durch die beschränkt die `WHERE` Klauseln. Es gibt noch viele Sprachfeatures entwickelt, für die Arbeit mit einem Datensatz zu einem Zeitpunkt und zum Bearbeiten von skalaren Daten. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) für eine Gruppe von Datensätzen über jeweils eine Schleife sein können. String-Funktionen zur Zeichenfolgenmanipulation wie `LEFT`, `CHARINDEX`, und `PATINDEX` mit skalaren Daten. SQL enthält auch Anweisungen für die ablaufsteuerung wie `IF` und `WHILE`.

Vor der Microsoft SQL Server 2005 können gespeicherte Prozeduren und benutzerdefinierte Funktionen nur als eine Auflistung von T-SQL-Anweisungen definiert werden. SQL Server 2005, jedoch wurde konzipiert, Integration in die [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), die Laufzeit, die von allen Assemblys für .NET verwendet wird. Daher können die gespeicherten Prozeduren und benutzerdefinierte Funktionen in einer SQL Server 2005-Datenbank erstellt werden mithilfe von verwaltetem Code. D.h., können Sie eine gespeicherte Prozedur oder UDF als eine Methode in einer C#-Klasse erstellen. Dadurch können diese gespeicherten Prozeduren und benutzerdefinierte Funktionen, Funktionen, die in .NET Framework und von Ihren eigenen benutzerdefinierten Klassen nutzen können.

In diesem Lernprogramm wird untersucht gespeichert wie das Erstellen verwalteten Prozeduren und benutzerdefinierte Funktionen und wie sie in unserer Northwind-Datenbank integriert. Lassen Sie s beginnen!

> [!NOTE]
> Verwaltete Datenbankobjekte bieten einige Vorteile gegenüber ihren SQL-Entsprechungen. Sprache Vielfalt und vertraut sind und die Möglichkeit, Wiederverwenden von vorhandenem Code und Geschäftslogik sind die wichtigsten Vorteile. Verwaltete Datenbankobjekte treten aber wahrscheinlich weniger effizient sein, bei der Verwendung von Datasets, die nicht viel prozedurale Logik beinhalten. Eine ausführlichere Erläuterung zu den Vorteilen der Verwendung von verwalteten Codes im Vergleich zu T-SQL finden Sie in der [Vorteile von verwaltetem Code bei der Erstellung von Datenbankobjekten](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Schritt 1: Verschieben der Northwind-Datenbank von`App_Data`

Alle unsere Tutorials bisher haben verwendet eine Microsoft SQL Server 2005 Express Edition-Datenbankdatei in der Web Application s `App_Data` Ordner. Platzieren Sie die Datenbank in `App_Data` einfachere Verteilung und in diesen Tutorials ausführen, da alle Dateien in einem Verzeichnis wurden und keine zusätzliche Konfigurationsschritte erforderlich benötigt, um das Tutorial zu testen.

In diesem Tutorial jedoch Let s verschieben die Northwind-Datenbank von `App_Data` und registrieren Sie ihn explizit mit der SQL Server 2005 Express Edition-Datenbank-Instanz. Während wir die Schritte, für dieses Tutorial mit der Datenbank in ausführen können der `App_Data` Ordner eine Reihe von den Schritten erfolgen wesentlich durch die Registrierung explizit der Datenbank mit der SQL Server 2005 Express Edition-Datenbank-Instanz.

Der Download für dieses Lernprogramm wurde die zwei Datenbankdateien - `NORTHWND.MDF` und `NORTHWND_log.LDF` – in einen Ordner namens platziert `DataFiles`. Wenn Sie zusammen mit Ihren eigenen Implementierung der Tutorials folgen, schließen Sie Visual Studio, und Verschieben der `NORTHWND.MDF` und `NORTHWND_log.LDF` Dateien von der Website s `App_Data` Ordner in einem Ordner außerhalb der Website. Nachdem Sie die Datenbankdateien in einen anderen Ordner verschoben werden können, dass wir benötigen, um die Northwind-Datenbank mit der SQL Server 2005 Express Edition-Datenbank-Instanz zu registrieren. Dies kann von SQL Server Management Studio erfolgen. Wenn Sie eine nicht - Express-Edition von SQL Server 2005 auf Ihrem Computer installiert haben müssen Sie wahrscheinlich bereits Management Studio installiert. Wenn Sie nur SQL Server 2005 Express Edition auf Ihrem Computer herunterladen und installieren in Ruhe [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Starten Sie SQL Server Management Studio. Wie in Abbildung 1 gezeigt, wird Sie werden aufgefordert, welchen Server Sie zum Herstellen einer Verbindung mit Management Studio gestartet. Geben Sie den Namen des Servers Localhost\SQLExpress, wählen Sie die Windows-Authentifizierung in der Dropdownliste Authentifizierung aus, und klicken Sie auf Verbinden.


![Verbinden Sie mit der entsprechenden Datenbank-Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Abbildung 1**: Verbinden mit der entsprechenden Datenbank-Instanz


Nachdem Sie Ve hergestellt wurde, listet die Objekt-Explorer-Fenster Informationen über die SQL Server 2005 Express Edition-Datenbankinstanz, einschließlich Datenbanken, Sicherheitsinformationen, Management-Optionen, und So weiter.

Wir fügen Sie in die Northwind-Datenbank müssen die `DataFiles` Ordner (oder ganz egal, wo Sie verschoben haben können) mit der SQL Server 2005 Express Edition-Datenbank-Instanz. Mit der rechten Maustaste auf den Ordner "Datenbanken", und wählen Sie die Attach-Option im Kontextmenü. Das Anfügen von Datenbanken-Dialogfeld wird angezeigt. Klicken Sie auf die Schaltfläche "hinzufügen", der Drilldown mit den entsprechenden `NORTHWND.MDF` Datei, und klicken Sie auf OK. An diesem Punkt werden Ihr Bildschirm sollte ähnlich wie in Abbildung 2 aussehen.


[![Verbinden Sie mit der entsprechenden Datenbank-Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Abbildung 2**: Verbinden mit der entsprechenden Datenbank-Instanz ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Beim Verbinden mit der SQL Server 2005 Express Edition-Instanz über Management Studio lässt das Dialogfeld Anfügen von Datenbanken nicht um einen Drilldown in Verzeichnissen nach Benutzer-Profil, wie z. B. Stellen Sie daher sicher, platzieren Sie die `NORTHWND.MDF` und `NORTHWND_log.LDF` Dateien in einem Verzeichnis kein Benutzerprofil.


Klicken Sie auf die Schaltfläche "OK", um die Datenbank anzufügen. Das Anfügen von Datenbanken-Dialogfeld wird geschlossen, und im Objekt-Explorer sollten nun die gerade angefügten Datenbank aufgeführt. Die Chancen stehen die Northwind-Datenbank einen Namen wie weist `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Benennen Sie die Datenbank zur Northwind-Datenbank, indem mit der rechten Maustaste auf die Datenbank und umbenennen.


![Benennen Sie die Datenbank zur Northwind-Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Abbildung 3**: Benennen Sie die Datenbank zur Northwind-Datenbank


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Schritt 2: Erstellen eine neue Projektmappe und SQL Server-Projekt in Visual Studio

Zum Erstellen von verwalteten gespeicherten Prozeduren oder benutzerdefinierte Funktionen in SQL Server 2005 schreiben wir die gespeicherte Prozedur und die UDF-Logik als C#-Code in einer Klasse. Nachdem der Code geschrieben wurde, müssen wir dieser Klasse in eine Assembly zu kompilieren (eine `.dll` Datei), registrieren Sie die Assembly mit SQL Server-Datenbank und erstellen Sie dann in der Datenbank, die auf die entsprechende Methode in zeigt eine gespeicherte Prozedur oder UDF-Objekt die Assembly. Diese Schritte können alle manuell ausgeführt werden. Wir können den Code erstellen, in einen beliebigen Text-Editor, kompilieren Sie ihn über die Befehlszeile mit der C#-Compiler ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), registrieren Sie ihn mit der Datenbank mithilfe der [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) hochladebefehl oder aus der Verwaltung Studio, und fügen Sie die gespeicherte Prozedur oder UDF-Objekt, auf ähnliche Weise. Zum Glück enthalten die Professional und Team-Systeme Versionen von Visual Studio ein SQL Server-Projekt, das diese Aufgaben automatisiert. In diesem Tutorial werden wir die Verwendung des SQL Server-Projekt-Typs um eine verwaltete gespeicherte Prozedur und UDF-Datei zu erstellen.

> [!NOTE]
> Wenn Sie Visual Web Developer oder die Standard Edition von Visual Studio verwenden, müssen Sie stattdessen die manuellen Ansatz zu verwenden. Schritt 13 enthält detaillierte Anweisungen zum Ausführen dieser Schritte manuell. Sollten Sie die Schritte 2 bis 12 lesen, bevor Sie Schritt 13 lesen, da diese Schritte wichtige SQL Server-Konfigurations-Anweisungen, die angewendet werden muss enthalten, unabhängig davon, welche Version von Visual Studio Sie verwenden.


Starten Sie Visual Studio öffnen. Wählen Sie aus dem Menü "Datei" Neues Projekt aus, um das Dialogfeld "Neues Projekt" anzuzeigen (siehe Abbildung 4). Drilldown in der Datenbank-Projekttyp aus, und wählen Sie dann aus den Vorlagen, die auf der rechten Seite aufgeführt wird, um ein neues SQL Server-Projekt zu erstellen. Ich haben sich entschieden, dieses Projekt `ManagedDatabaseConstructs` und platziert es in einer Projektmappe mit dem Namen `Tutorial75`.


[![Erstellen eines neuen SQL Server-Projekts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Abbildung 4**: Erstellen eines neuen SQL Server-Projekts ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Klicken Sie auf die Schaltfläche "OK", klicken Sie im Dialogfeld Neues Projekt auf die Projektmappe und SQL Server-Projekt erstellen.

Ein SQL Server-Projekt ist mit einer bestimmten Datenbank gebunden. Daher werden nach dem Erstellen des neuen SQL Server-Projekts wir sofort aufgefordert, diese Informationen angeben. Abbildung 5 zeigt das Dialogfeld Neuer Datenbankverweis, die sich auf die Northwind-Datenbank zu verweisen, die wir in der SQL Server 2005 Express Edition-Datenbank-Instanz in Schritt 1 registriert gefüllt wurde.


![Ordnen Sie das SQL Server-Projekt mit der Northwind-Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Abbildung 5**: Ordnen Sie das SQL Server-Projekt mit der Northwind-Datenbank


Um die verwaltete gespeicherte Prozeduren und benutzerdefinierte Funktionen wir in diesem Projekt erstellen zu debuggen, muss SQL/CLR-debugging-Unterstützung für die Verbindung aktiviert werden. Wenn ein SQL Server-Projekt eine neue Datenbank zuordnen (wie in Abbildung 5), Visual Studio werden wir gefragt, ob SQL/CLR-Debuggen für die Verbindung aktiviert werden soll (siehe Abbildung 6). Klicken Sie auf "Ja".


![SQL/CLR-Debuggen aktivieren](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Abbildung 6**: SQL/CLR-Debuggen aktivieren


An diesem Punkt wurde das neue SQL Server-Projekt zur Projektmappe hinzugefügt. Es enthält einen Ordner namens `Test Scripts` mit einer Datei mit dem Namen `Test.sql`, die für das Debuggen verwalteter Datenbankobjekte im Projekt erstellt werden. Betrachten wir Debuggen in Schritt 12 fort.

Wir können jetzt die neue verwaltete gespeicherte Prozeduren und benutzerdefinierte Funktionen hinzuzufügen, zu diesem Projekt, aber bevor wir zulassen, e zuerst enthalten unsere vorhandenen Web-App in der Projektmappe. Wählen Sie die Option hinzufügen, und die wählen Sie vorhandene Website aus, über das Menü Datei. Navigieren Sie zu der entsprechenden Website-Ordner, und klicken Sie auf OK. Wie in Abbildung 7 dargestellt, wird dadurch die Lösung enthält zwei Projekte aktualisiert: die Website und die `ManagedDatabaseConstructs` SQL Server-Projekt.


![Im Projektmappen-Explorer enthält jetzt zwei Projekte](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Abbildung 7**: im Projektmappen-Explorer enthält jetzt zwei Projekte


Die `NORTHWNDConnectionString` Wert in `Web.config` derzeit verweist die `NORTHWND.MDF` Datei die `App_Data` Ordner. Da wir diese Datenbank von entfernt `App_Data` und registrieren Sie es explizit in der SQL Server 2005 Express Edition-Datenbank-Instanz muss entsprechend aktualisiert. die `NORTHWNDConnectionString` Wert. Öffnen der `Web.config` -Datei in die Website und die Änderung der `NORTHWNDConnectionString` Wert, sodass die Verbindungszeichenfolge lautet: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Nach dieser Änderung Ihre `<connectionStrings>` im Abschnitt `Web.config` sollte etwa wie folgt aussehen:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Siehe die [vorherigen Lernprogramm](debugging-stored-procedures-cs.md), beim Debuggen einer SQL Server-Objekt von einer Clientanwendung, wie z. B. einer ASP.NET-Website müssen wir Verbindungspooling nicht deaktiviert. Die Verbindungszeichenfolge, die oben gezeigte deaktiviert Verbindungspooling ( `Pooling=false` ). Wenn Sie nicht, zum Debuggen von verwalteten gespeicherten Prozeduren und benutzerdefinierten Funktionen aus der ASP.NET-Website beabsichtigen, aktivieren Sie die Verbindungs-pooling.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Schritt 3: Erstellen eines verwalteten gespeicherten Prozedur

Eine verwaltete gespeicherte Prozedur mit der Datenbank Northwind hinzufügen wir zuerst auf die gespeicherte Prozedur als Methode in der SQL Server-Projekt zu erstellen müssen. Im Projektmappen-Explorer mit der Maustaste auf die `ManagedDatabaseConstructs` Projektname und wählen Sie ein neues Element hinzufügen. Dadurch wird das Dialogfeld "Neues Element hinzufügen" angezeigt, dem die Typen verwalteter Datenbankobjekte aufgelistet, die dem Projekt hinzugefügt werden können. Wie in Abbildung 8 gezeigt, enthält diese gespeicherten Prozeduren und benutzerdefinierten Funktionen, unter anderem an.

Lassen Sie s starten, indem Sie eine gespeicherte Prozedur, die einfach alle Produkte zurückgibt, die eingestellt wurden hinzugefügt. Nennen Sie die neue gespeicherte Prozedurdatei `GetDiscontinuedProducts.cs`.


[![Hinzufügen einer neuen gespeicherten Prozedur, die mit dem Namen GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Abbildung 8**: Hinzufügen einer neuen gespeicherten Prozedur mit dem Namen `GetDiscontinuedProducts.cs` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Dies erstellt eine neue C#-Klassendatei mit dem folgenden Inhalt:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Beachten Sie, die die gespeicherte Prozedur, als implementiert wird eine `static` Methode innerhalb einer `partial` Klassendatei mit dem Namen `StoredProcedures`. Darüber hinaus die `GetDiscontinuedProducts` Methode ergänzt wird, mit der `SqlProcedure attribute`, die kennzeichnet, dass der Methode als eine gespeicherte Prozedur.

Der folgende Code erstellt eine `SqlCommand` -Objekt und legt seine `CommandText` auf eine `SELECT` Abfrage, die alle Spalten aus zurückgibt der `Products` Tabelle für Produkte, deren `Discontinued` Feld 1. Dann wird der Befehl ausgeführt, und sendet die Ergebnisse zurück an die Clientanwendung. Fügen Sie folgenden Code, der `GetDiscontinuedProducts` Methode.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Alle verwalteten Datenbankobjekte haben Zugriff auf eine [ `SqlContext` Objekt](https://msdn.microsoft.com/library/ms131108.aspx) , das den Kontext des Aufrufers darstellt. Die `SqlContext` ermöglicht den Zugriff auf eine [ `SqlPipe` Objekt](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) über seine [ `Pipe` Eigenschaft](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Dies `SqlPipe` Objekt wird verwendet, um Informationen zwischen der SQL Server-Datenbank und die aufrufende Anwendung zu senden. Wie der Name schon sagt, den [ `ExecuteAndSend` Methode](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) führt eine übergegebenen `SqlCommand` -Objekt und die Ergebnisse an die Clientanwendung zurück sendet.

> [!NOTE]
> Verwaltete Datenbankobjekte eignen sich am besten für gespeicherte Prozeduren und benutzerdefinierte Funktionen, die prozedurale Logik statt setbasierte Logik verwenden. Prozedurale Logik umfasst die Arbeit mit Daten pro Zeile für Zeile von oder Arbeiten mit skalaren Daten. Die `GetDiscontinuedProducts` Methode, die wir gerade erstellt, jedoch haben wird keine prozedurale Logik. Aus diesem Grund würden sie idealerweise als gespeicherte T-SQL-Prozedur implementiert werden. Die Implementierung erfolgt als eine verwaltete gespeicherte Prozedur veranschaulicht die erforderlichen Schritte zum Erstellen und Bereitstellen von gespeicherten Prozeduren verwaltet.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Schritt 4: Bereitstellen der verwalteten gespeicherten Prozedur

Mit diesem Code, der abgeschlossen ist können wir sie in der Northwind-Datenbank bereitstellen. Bereitstellen eines SQL Server-Projekts wird der Code in eine Assembly kompiliert, registriert die Assembly mit der Datenbank und erstellt die entsprechenden Objekte in der Datenbank, die sie in die entsprechenden Methoden in der Assembly zu verknüpfen. Der genaue Satz von Tasks, die Bereitstellungsoption ist genauer gesagt in Schritt 13 ausgeschrieben. Mit der rechten Maustaste auf die `ManagedDatabaseConstructs` Projektname im Projektmappen-Explorer, und wählen Sie die Option bereitstellen. Bereitstellung jedoch fehlschlägt, mit dem folgenden Fehler: falsche Syntax in der Nähe von "Extern". Sie müssen möglicherweise den Kompatibilitätsgrad der aktuellen Datenbank auf einen höheren Wert zum Aktivieren dieser Funktion festlegen. Finden Sie Hilfe für die gespeicherte Prozedur `sp_dbcmptlevel`.

Diese Fehlermeldung tritt auf, bei dem Versuch, um die Assembly mit der Northwind-Datenbank zu registrieren. Um eine Assembly mit einer SQL Server 2005-Datenbank zu registrieren, muss der Kompatibilitätsgrad der Datenbank-s auf 90 festgelegt werden. Standardmäßig ist bei neuen SQL Server 2005-Datenbanken einen Kompatibilitätsgrad von 90. Allerdings haben Datenbanken mit Microsoft SQL Server 2000 erstellt einen Standard-Kompatibilitätsgrad von 80. Seit die Northwind-Datenbank zunächst eine Microsoft SQL Server 2000-Datenbank, der Kompatibilitätsgrad derzeit auf 80 festgelegt ist, und muss daher auf 90 erhöht werden, um Datenbankobjekte zu registrieren.

Um der Kompatibilitätsgrad der Datenbank-s zu aktualisieren, öffnen Sie ein neues Abfragefenster in Management Studio, und geben Sie ein:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Klicken Sie auf das Symbol "ausführen" in der Symbolleiste, um die obige Abfrage ausgeführt.


[![Aktualisieren der Northwind-Datenbank-Kompatibilitätsgrad s](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Abbildung 9**: Aktualisieren der Datenbank "Northwind" s-Kompatibilitätsgrad ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


Nach dem Aktualisieren des Kompatibilitätsgrads, erneut bereitstellen Sie in der SQL Server-Projekt. Dieses Mal sollte die Bereitstellung ohne Fehler abgeschlossen werden.

Zurück zu SQL Server Management Studio, mit der rechten Maustaste auf die Northwind-Datenbank im Objekt-Explorer, und wählen Sie aktualisieren. Als Nächstes Drilldown in den Ordner Programmierung, und erweitern Sie dann den Ordner Assemblys. Wie Abbildung 10 zeigt, enthält die Northwind-Datenbank jetzt vom generierten Assembly die `ManagedDatabaseConstructs` Projekt.


![Die ManagedDatabaseConstructs-Assembly ist jetzt mit der Northwind-Datenbank registriert.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Abbildung 10**: die `ManagedDatabaseConstructs` Assembly ist jetzt mit der Northwind-Datenbank registriert.


Erweitern Sie auch den Ordner gespeicherte Prozeduren aus. Es wird Ihnen eine gespeicherte Prozedur namens `GetDiscontinuedProducts`. Diese gespeicherte Prozedur erstellt wurde, durch den Bereitstellungsprozess und verweist auf die `GetDiscontinuedProducts` -Methode in der die `ManagedDatabaseConstructs` Assembly. Bei der `GetDiscontinuedProducts` gespeicherte Prozedur wird ausgeführt, es wiederum führt die `GetDiscontinuedProducts` Methode. Da dies eine verwaltete gespeicherte Prozedur ist es nicht bearbeitet werden über Management Studio (daher das Schlosssymbol neben dem Namen der gespeicherten Prozedur).


![Die GetDiscontinuedProducts gespeicherte Prozedur wird in den Ordner für die gespeicherten Prozeduren aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Abbildung 11**: die `GetDiscontinuedProducts` gespeicherte Prozedur wird in den Ordner für die gespeicherten Prozeduren aufgeführt.


Es ist immer noch eine weitere Hürde, wir haben zu überwinden, bevor die verwaltete gespeicherte Prozedur aufgerufen werden kann: die Datenbank konfiguriert ist, um zu verhindern, dass bei der Ausführung von verwaltetem Code. Überprüfen Sie dies durch ein neues Abfragefenster öffnen und Ausführen der `GetDiscontinuedProducts` gespeicherte Prozedur. Sie erhalten die folgende Fehlermeldung angezeigt: Ausführung von Benutzercode in .NET Framework ist deaktiviert. Aktivieren Sie die Konfigurationsoption für Clr-fähig.

Überprüfen Sie die Konfigurationsinformationen des Northwind-s-Datenbank, geben aus, und führen Sie den Befehl `exec sp_configure` in das Abfragefenster. Dies zeigt, dass es sich bei der Clr-fähig festlegen auf 0 festgelegt ist.


[![Die Clr-fähig Einstellung ist derzeit auf 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Abbildung 12**: die Clr-fähig Einstellung ist derzeit auf 0 ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Beachten Sie, dass jede Konfigurationseinstellung in Abbildung 12 vier Werte, die mit ihm aufgeführt: die minimale und maximale Werte und die Konfiguration und Ausführungswert. Um der Konfigurationswert für die Einstellung für die Clr-fähig zu aktualisieren, führen Sie den folgenden Befehl aus:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Wenn Sie erneut ausführen, die `exec sp_configure` sehen Sie, dass die oben genannte Anweisung der Clr-fähig s Config 1 aktualisiert, aber der Ausführungswert immer noch auf 0 festgelegt ist. Für diese konfigurationsänderung wirksam müssen wir führen die [ `RECONFIGURE` Befehl](https://msdn.microsoft.com/library/ms176069.aspx), auf den aktuellen Konfigurationswert den Ausführungswert festzulegen. Geben Sie einfach `RECONFIGURE` in das Abfragefenster, und klicken Sie auf das Symbol "ausführen" in der Symbolleiste. Wenn das Ausführen `exec sp_configure` jetzt sollten Sie finden Sie unter den Wert 1 für die Clr-fähig Einstellung s-Konfigurationsdatei und führen Sie die Werte.

Mit der Clr-fähig-Konfiguration abgeschlossen ist, sind wir bereit für die verwaltete Ausführung `GetDiscontinuedProducts` gespeicherte Prozedur. Geben Sie im Abfragefenster, und führen Sie den Befehl `exec` `GetDiscontinuedProducts`. Aufrufen der gespeicherten Prozedur führt dazu, dass die entsprechenden verwalteten Code in die `GetDiscontinuedProducts` auszuführende Methode. Dieser Code gibt eine `SELECT` Abfrage alle Produkte zurückgegeben, die nicht mehr und gibt diese Daten an die aufrufende Anwendung, die SQL Server Management Studio in diesem Fall ist. Management Studio empfängt diese Ergebnisse und zeigt sie im Ergebnisfenster.


[![Die gespeicherte Prozedur alle zurückgibt GetDiscontinuedProducts ausgelaufenen Produkte](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Abbildung 13**: die `GetDiscontinuedProducts` gespeicherte Prozedur gibt alle nicht mehr unterstützte Produkte ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Schritt 5: Erstellen verwaltete gespeicherte Prozeduren, die Eingabeparameter akzeptieren

Viele der Abfragen und gespeicherte Prozeduren, die wir, klicken Sie in diesen Tutorials erstellt haben verwendet haben *Parameter*. Z. B. in der [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet-s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Tutorial eine gespeicherte Prozedur namens erstellt `GetProductsByCategoryID` , die einen Eingabeparameter mit dem Namen akzeptiert `@CategoryID`. Die gespeicherte Prozedur dann alle Produkte zurückgegeben, deren `CategoryID` Feld übereinstimmen, den Wert des angegebenen `@CategoryID` Parameter.

Um eine verwaltete gespeicherte Prozedur zu erstellen, die Eingabeparameter akzeptiert, geben Sie einfach die Parameter in der Methodendefinition s ein. Um dies zu veranschaulichen, Let s eine andere verwaltete gespeicherte Prozedur zum Hinzufügen der `ManagedDatabaseConstructs` Projekt mit dem Namen `GetProductsWithPriceLessThan`. Diese verwaltete gespeicherte Prozedur akzeptiert einen Eingabeparameter, die einen Preis angeben, und gibt alle Produkte zurück, dessen `UnitPrice` Feld ist kleiner als der Parameter-s-Wert.

Um das Projekt eine neue gespeicherte Prozedur hinzuzufügen, mit der Maustaste auf die `ManagedDatabaseConstructs` Projektname und wählen Sie eine neue gespeicherte Prozedur hinzufügen. Nennen Sie die Datei `GetProductsWithPriceLessThan.cs`. Dadurch wird zunächst eine neue C#-Klassendatei mit einer Methode, die mit dem Namen erstellt, wie in Schritt 3 beschrieben, `GetProductsWithPriceLessThan` in platziert die `partial` Klasse `StoredProcedures`.

Update der `GetProductsWithPriceLessThan` s Methodendefinition so, dass die It akzeptiert eine [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) Eingabeparameter mit dem Namen `price` und den Code schreiben, auszuführen und die Ergebnisse der Abfrage zurückzugeben:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

Die `GetProductsWithPriceLessThan` Methodendefinition s und Code gleicht die Definition und den Code der `GetDiscontinuedProducts` Methode, die in Schritt 3 erstellt haben. Der einzige Unterschied besteht, die die `GetProductsWithPriceLessThan` -Methode akzeptiert als Eingabe an den Parameter (`price`), wird die `SqlCommand` s-Abfrage enthält einen Parameter (`@MaxPrice`), und ein Parameter hinzugefügt wird die `SqlCommand` s `Parameters` Sammlung ist und der Wert des der `price` Variable.

Nach dem Hinzufügen dieses Codes, erneut bereitstellen Sie in der SQL Server-Projekt. Klicken Sie dann zurück zu SQL Server Management Studio, und aktualisieren Sie den Ordner für gespeicherte Prozeduren. Daraufhin sollte einen neuen Eintrag, `GetProductsWithPriceLessThan`. Geben Sie ein Abfragefenster, und führen Sie den Befehl `exec GetProductsWithPriceLessThan 25`, wird Sie Liste weniger als $25, alle Produkte, wie in Abbildung 14 dargestellt.


[![Produkte unter $25 werden angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Abbildung 14**: Produkte unter $25 werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Schritt 6: Aufrufen der verwalteten gespeicherten Prozedur aus der Datenzugriffsebene

Wir haben an diesem Punkt hinzugefügt der `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherte Prozeduren, die `ManagedDatabaseConstructs` Projekt, und sie mit der Northwind-SQL Server-Datenbank registriert haben. Wir haben diese verwalteten gespeicherten Prozeduren von SQL Server Management Studio auch aufgerufen (siehe Abbildung s 13 und 14). In der Reihenfolge für unsere ASP.NET verwaltete Anwendung für die Verwendung dieser gespeicherte Prozeduren, allerdings müssen wir diese Datenzugriffs- und Geschäftslogikschichten in der Architektur hinzufügen. In diesem Schritt werden wir zwei neue Methoden zum Hinzufügen der `ProductsTableAdapter` in die `NorthwindWithSprocs` typisierte DataSet, das im ursprünglich erstellt wurde die [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet-s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Tutorial. In Schritt 7 werden wir die entsprechende Methoden an die BLL hinzufügen.

Öffnen der `NorthwindWithSprocs` typisierte DataSet in Visual Studio und beginnen Sie, indem Sie eine neue Methode zum Hinzufügen der `ProductsTableAdapter` mit dem Namen `GetDiscontinuedProducts`. Um eine neue Methode einen TableAdapter hinzugefügt haben, mit der rechten Maustaste auf den Namen des TableAdapter s im Designer, und wählen Sie im Kontextmenü die Option "hinzufügen".

> [!NOTE]
> Seit die Northwind-Datenbank aus der `App_Data` Ordner mit der SQL Server 2005 Express Edition-Datenbank-Instanz, es ist zwingend erforderlich, dass die zugehörige Verbindungszeichenfolge in "Web.config" aktualisiert werden, um diese Änderung zu übernehmen. In Schritt2 die aktualisieren erörtert die `NORTHWNDConnectionString` Wert `Web.config`. Wenn Sie vergessen haben, um dieses Update zu machen, klicken Sie dann sehen die Fehlermeldung "Fehler" Abfrage hinzufügen Sie. Keine Verbindung gefunden `NORTHWNDConnectionString` für Objekt `Web.config` in einem Dialogfeld, wenn Sie versuchen, eine neue Methode dem TableAdapter hinzugefügt. Um diesen Fehler zu beheben, klicken Sie auf OK, und fahren Sie mit `Web.config` und aktualisieren Sie die `NORTHWNDConnectionString` -Wert, wie in Schritt2 beschrieben. Wiederholen Sie dann erneut die-Methode dem TableAdapter hinzufügen. Dieses Mal sollte es fehlerfrei funktionieren.


Hinzufügen einer neuen Methode startet den TableAdapter-Abfrage-Konfigurations-Assistenten, in dem wir oft in den letzten Tutorials verwendet haben. Im ersten Schritt fragt uns an, wie der TableAdapter auf die Datenbank zugreifen soll: über eine Ad-hoc-SQL-Anweisung oder einer neuen oder vorhandenen gespeicherten Prozedur. Da wir bereits erstellt und registriert wurden die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur mit der Datenbank, wählen Sie die vorhandene gespeicherte Prozedur-Option, und drücken weiter.


[![Wählen Sie die vorhandene gespeicherte Prozedur-Option verwenden](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Abbildung 15**: Wählen Sie die vorhandene gespeicherte Prozedur Option ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Der nächste Bildschirm fordert uns für die gespeicherte Prozedur, die die Methode aufgerufen wird. Wählen Sie die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur aus der Dropdown-Liste, und klicken Sie weiter.


[![Wählen Sie die GetDiscontinuedProducts verwalteten gespeicherten Prozedur](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Abbildung 16**: Wählen Sie die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Wir werden dann aufgefordert, um anzugeben, ob die gespeicherte Prozedur Zeilen, einen einzelnen Wert oder nichts zurückgibt. Da `GetDiscontinuedProducts` gibt den Satz von Produktzeilen nicht mehr unterstützte, wählen Sie die erste Option (Tabellendaten), und klicken Sie auf Weiter.


[![Wählen Sie die Tabellendaten-Option](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Abbildung 17**: Wählen Sie die tabellarische Daten-Option ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Der letzten Seite des Assistenten kann wir die Datenzugriffsmuster verwendet und die Namen der resultierenden Methoden angeben. Behalten Sie Sie Kontrollkästchen aktiviert und die Namen die Methoden `FillByDiscontinued` und `GetDiscontinuedProducts`. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.


[![Namen der Methoden FillByDiscontinued und GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Abbildung 18**: Benennen Sie die Methoden `FillByDiscontinued` und `GetDiscontinuedProducts` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Wiederholen Sie diese Schritte zum Erstellen von Methoden, die mit dem Namen `FillByPriceLessThan` und `GetProductsWithPriceLessThan` in die `ProductsTableAdapter` für die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur.

Abbildung 19 zeigt einen Screenshot des DataSet-Designer nach dem Hinzufügen der Methoden für die `ProductsTableAdapter` für die `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherte Prozeduren.


[![Die ProductsTableAdapter enthält die neuen Methoden, die in diesem Schritt hinzugefügt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Abbildung 19**: die `ProductsTableAdapter` enthält neue Methoden hinzugefügt, in diesem Schritt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Schritt 7: Hinzufügen von entsprechenden Methoden zu der Geschäftslogikebene

Nachdem wir die Datenzugriffsebene enthält Methoden zum Aufrufen der verwalteten gespeicherten Prozeduren hinzugefügt, die in den Schritte4 und 5 aktualisiert haben, müssen wir entsprechenden Methoden der Geschäftslogikschicht hinzu. Die folgenden beiden Methoden zum Hinzufügen der `ProductsBLLWithSprocs` Klasse:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Beide Methoden einfach die entsprechende DAL-Methode aufrufen und Zurückgeben der `ProductsDataTable` Instanz. Die `DataObjectMethodAttribute` Markup oberhalb jeder Methode bewirkt, dass diese Methoden in der Dropdown-Liste in der Registerkarte "auswählen" den "ObjectDataSource"-s-Konfigurieren von Datenquellen-Assistenten eingeschlossen werden.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Schritt 8: Aufrufen von verwalteten gespeicherten Prozeduren der Darstellungsschicht

Mit der Geschäftslogik und Datenzugriffsschichten erweitert, um Unterstützung für den Aufruf der `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozeduren, können wir jetzt zeigen diese gespeicherten Prozeduren Ergebnisse über eine ASP.NET-Seite.

Öffnen der `ManagedFunctionsAndSprocs.aspx` auf der Seite die `AdvancedDAL` Ordner, und ziehen Sie aus der Toolbox einer GridView-Ansicht auf den Designer. Legen Sie die GridView s `ID` Eigenschaft `DiscontinuedProducts` und von sein Smarttag, binden Sie es an eine neue, mit dem Namen "ObjectDataSource" `DiscontinuedProductsDataSource`. Konfigurieren Sie zum Abrufen der Daten aus dem ObjectDataSource-Steuerelement die `ProductsBLLWithSprocs` Klasse s `GetDiscontinuedProducts` Methode.


[![Konfigurieren von dem ObjectDataSource-Steuerelement zur Verwendung der ProductsBLLWithSprocs-Klasse](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Abbildung 20**: Konfigurieren von dem ObjectDataSource-Steuerelement verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Wählen Sie die GetDiscontinuedProducts-Methode aus der Dropdown-Liste in der Registerkarte "SELECT"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Abbildung 21**: Wählen Sie die `GetDiscontinuedProducts` Methode aus der Dropdown-Liste auf der Registerkarte "auswählen" ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Da dieses Raster, nur Anzeige von Produktinformationen verwendet werden, legen Sie die Dropdownlisten in der Update-, INSERT-, Registerkarten, um (keine) und löschen Sie klicken Sie dann auf "Fertig stellen".

Nach Abschluss des Assistenten, Visual Studio wird automatisch Hinzufügen eines BoundField- oder CheckBoxField für jedes Datenfeld in der `ProductsDataTable`. So entfernen Sie alle diese Felder mit Ausnahme von in Ruhe `ProductName` und `Discontinued`, zeigen Sie mit der Ihre GridView und "ObjectDataSource" s deklaratives Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Nehmen Sie einen Moment Zeit, zum Anzeigen dieser Seite über einen Browser ein. Wenn die Seite besucht wird, die ObjectDataSource ruft die `ProductsBLLWithSprocs` Klasse s `GetDiscontinuedProducts` Methode. Wie wir in Schritt 7 gesehen haben, ruft diese Methode nach unten der DAL-s `ProductsDataTable` Klasse s `GetDiscontinuedProducts` -Methode, die Ruft die `GetDiscontinuedProducts` gespeicherte Prozedur. Diese gespeicherte Prozedur ist eine verwaltete gespeicherte Prozedur aus, und führt den Code, die in Schritt 3, die nicht mehr unterstützte Produkte zurückgeben erstellt wurde.

Die von der verwalteten gespeicherten Prozedur zurückgegebenen Ergebnisse werden in verpackt eine `ProductsDataTable` von der DAL und seitdem wieder an die BLL, die dann sie auf der Darstellungsschicht zurückgibt, wo sie sind an die GridView gebunden und angezeigt. Erwartungsgemäß funktioniert, listet das Raster dieser Produkte, die eingestellt wurden.


[![Die nicht mehr unterstützte Produkte werden aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Abbildung 22**: die nicht mehr unterstützte Produkte aufgeführt sind ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Fügen Sie zur weiteren wird empfohlen ein Textfeld und einer anderen GridView, auf der Seite. Haben Sie diese GridView zeigt die Produkte, die kleiner als die Menge, die in das Textfeld eingegeben werden, durch den Aufruf der `ProductsBLLWithSprocs` Klasse s `GetProductsWithPriceLessThan` Methode.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Schritt 9: Erstellen und Aufrufen von T-SQL-UDFs

Benutzerdefinierte Funktionen oder benutzerdefinierte Funktionen, sind, dass der Nachahmung genau die Semantik der Funktionen in Programmiersprachen zu vergleichen Datenbankobjekte. UDFs können wie eine Funktion in C# geschrieben enthalten eine Variable Anzahl von Eingabeparametern und den Wert eines bestimmten Typs zurück. Eine UDF kann entweder skalaren Daten – eine Zeichenfolge, eine ganze Zahl, und So weiter – oder Tabellendaten zurückgeben. Lassen Sie s, nehmen einen kurzen Blick auf beide Arten von UDFs, beginnend mit einer benutzerdefinierten Funktion, die einen skalaren Datentyp zurückgibt.

Die folgende benutzerdefinierte Funktion berechnet den erwarteten Wert des Bestands für ein bestimmtes Produkt. Dies erfolgt durch aufnehmen in die drei Parameter: den `UnitPrice`, `UnitsInStock`, und `Discontinued` für ein bestimmtes Produkt - Werte und gibt einen Wert vom Typ `money`. Berechnet den erwarteten Wert des Bestands durch Multiplikation der `UnitPrice` durch die `UnitsInStock`. Für nicht mehr unterstützte Elemente wird dieser Wert halbiert.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Nachdem diese UDF in der Datenbank hinzugefügt wurde, können sie über Management Studio gefunden werden, durch das Erweitern der Ordner "Programmierbarkeit" und dann Funktionen, und klicken Sie dann Skalarwert-Funktionen. Es kann verwendet werden, einem `SELECT` Abfrage wie folgt:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Ich habe hinzugefügt haben die `udf_ComputeInventoryValue` UDF mit der Datenbank Northwind Abbildung 23 zeigt die Ausgabe der oben genannten `SELECT` Abfragen, wenn Sie über Management Studio angezeigt. Beachten Sie außerdem, dass die benutzerdefinierte Funktion unter dem Ordner Skalarwert-Funktionen im Objekt-Explorer aufgeführt ist.


[![Jedes Produkt s Inventur Werte wird aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Abbildung 23**: jedes Produkt s Inventory-Werte aufgeführt ist ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Benutzerdefinierte Funktionen können auch tabellarische Daten zurückgeben. Beispielsweise können wir eine benutzerdefinierte Funktion erstellen, die Produkte zurückgibt, die zu einer bestimmten Kategorie gehören:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

Die `udf_GetProductsByCategoryID` UDF akzeptiert eine `@CategoryID` Eingabeparameter und gibt die Ergebnisse der angegebenen `SELECT` Abfrage. Nach der Erstellung kann diese UDF verwiesen werden, der `FROM` (oder `JOIN`)-Klausel einer `SELECT` Abfrage. Im folgende Beispiel würde Zurückgeben der `ProductID`, `ProductName`, und `CategoryID` Werte für die einzelnen von Getränken.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Ich habe hinzugefügt haben die `udf_GetProductsByCategoryID` UDF mit der Datenbank Northwind Abbildung 24 zeigt die Ausgabe der oben genannten `SELECT` Abfragen, wenn Sie über Management Studio angezeigt. Benutzerdefinierte Funktionen, die tabellarische Daten zurückgeben finden Sie im Objekt-Explorer-Ordner für die s Tabellenwert-Funktionen.


[![Die ProductID, ProductName und CategoryID sind für jede trinken aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Abbildung 24**: die `ProductID`, `ProductName`, und `CategoryID` werden für jede trinken aufgelistet ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Weitere Informationen zum Erstellen und Verwenden von benutzerdefinierten Funktionen finden Sie in [Einführung in die User-Defined Functions](http://www.sqlteam.com/item.asp?ItemID=1955). Sehen Sie sich auch [Vorteile und Funktionen von Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Schritt 10: Erstellen eine verwaltete UDF

Die `udf_ComputeInventoryValue` und `udf_GetProductsByCategoryID` UDFs erstellt, die in den obigen Beispielen sind T-SQL-Datenbankobjekte. SQL Server 2005 unterstützt auch die verwaltete benutzerdefinierte Funktionen, die hinzugefügt werden können die `ManagedDatabaseConstructs` Projekt genau wie die verwaltete Prozeduren über die Schritte 3 und 5 gespeicherte. Für diesen Schritt können Sie s implementieren die `udf_ComputeInventoryValue` UDF in verwaltetem Code.

Eine verwaltete UDF zum Hinzufügen der `ManagedDatabaseConstructs` Projekt, mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer aus, und wählen Sie ein neues Element hinzufügen. Wählen Sie die User-Defined-Vorlage im Dialogfeld "Neues Element hinzufügen", und nennen Sie die neue UDF-Datei `udf_ComputeInventoryValue_Managed.cs`.


[![Eine neue verwaltete UDF zum ManagedDatabaseConstructs Projekt hinzufügen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Abbildung 25**: eine neue verwaltete UDF zum Hinzufügen der `ManagedDatabaseConstructs` Projekt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Die Vorlage für eine benutzerdefinierte Funktion erstellt eine `partial` Klasse mit dem Namen `UserDefinedFunctions` mit einer Methode, deren Name den Klassennamen für die s-Datei entspricht (`udf_ComputeInventoryValue_Managed`, in diesem Fall). Diese Methode wird ergänzt, mit der [ `SqlFunction` Attribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), die kennzeichnet, dass der Methode als eine verwaltete UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

Die `udf_ComputeInventoryValue` Methodenrückgabe derzeit eine [ `SqlString` Objekt](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) und nicht alle benötigten Eingabeparameter akzeptiert. Wir müssen die Definition der Methode aktualisieren, damit sie akzeptiert drei Eingabeparameter - `UnitPrice`, `UnitsInStock`, und `Discontinued` - und gibt eine `SqlMoney` Objekt. Die Logik zum Berechnen des Inventur-Werts ist identisch mit der in der T-SQL `udf_ComputeInventoryValue` UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Beachten Sie, dass die Eingabeparameter der UDF-s-Methode von den entsprechenden SQL-Typen: `SqlMoney` für die `UnitPrice` Feld [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) für `UnitsInStock`, und [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) für `Discontinued`. Diese Datentypen entsprechen, die in definierten Typen der `Products` Tabelle: der `UnitPrice` Spalte ist vom Typ `money`, `UnitsInStock` Spalte vom Typ `smallint`, und die `Discontinued` Spalte vom Typ `bit`.

Der Code beginnt mit der Erstellung einer `SqlMoney` Instanz mit dem Namen `inventoryValue` , die einen Wert von 0 zugewiesen ist. Die `Products` Table erlaubt, für die Datenbank `NULL` Werte in der `UnitsInPrice` und `UnitsInStock` Spalten. Daher wir zuerst prüfen um festzustellen, ob diese Werte enthalten müssen `NULL` s, die wir durch die `SqlMoney` s-Objekt [ `IsNull` Eigenschaft](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Wenn beide `UnitPrice` und `UnitsInStock` enthalten nicht`NULL` Werte, berechnen wir die `inventoryValue` das Produkt der beiden sein. Wenn danach `Discontinued` ist "true", und klicken Sie dann wir den Wert halbieren.

> [!NOTE]
> Die `SqlMoney` Objekt kann nur zwei `SqlMoney` Instanzen miteinander multipliziert werden sollen. Es lässt nicht zu einer `SqlMoney` Instanz eine literale Gleitkommazahl multipliziert werden. Aus diesem Grund zu halbieren `inventoryValue` wir Multiplizieren Sie es mit einer neuen `SqlMoney` Instanz, die den Wert 0,5 aufweist.


## <a name="step-11-deploying-the-managed-udf"></a>Schritt 11: Bereitstellen der verwalteten UDFs

Nun, dass die verwaltete benutzerdefinierte Funktion erstellt wurde, können wir sie in der Northwind-Datenbank bereitstellen. Wie in Schritt 4 beschrieben, werden die verwalteten Objekte in einer SQL Server-Datenbankprojekt bereitgestellt, mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und die Bereitstellungsoption aus dem Kontextmenü auswählen.

Nachdem Sie das Projekt bereitgestellt haben, zurück zu SQL Server Management Studio, und aktualisieren Sie den Ordner Skalarwertfunktionen. Sie sollten jetzt zwei Einträge sehen:

- `dbo.udf_ComputeInventoryValue` – der T-SQL-UDF in Schritt 9 erstellt und
- `dbo.udf ComputeInventoryValue_Managed` – das verwaltete UDF erstellt in Schritt 10, die gerade bereitgestellt wurde.

Um diese verwaltete UDF zu testen, führen Sie die folgende Abfrage aus in Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Dieser Befehl verwendet die verwaltete `udf ComputeInventoryValue_Managed` UDF anstelle von T-SQL `udf_ComputeInventoryValue` UDF, aber die Ausgabe ist identisch. Verweisen Sie zurück auf Abbildung 23, um einen Screenshot der UDF-s-Ausgabe anzuzeigen.

## <a name="step-12-debugging-the-managed-database-objects"></a>Schritt 12: Debuggen verwalteter Datenbankobjekte

In der [gespeicherte Prozeduren Debuggen](debugging-stored-procedures-cs.md) Tutorial erläutert die drei Optionen für das Debuggen von SQL Server über Visual Studio: Direktes Datenbankdebugging Anwendungsdebuggen und Debuggen von einem SQL Server-Projekt. Verwaltete Datenbank Objekte können nicht über direktes Datenbankdebugging debuggt werden, sind aber debuggt werden können, von einer Clientanwendung und direkt in der SQL Server-Projekt. Damit das Debuggen funktioniert muss jedoch die SQL Server 2005-Datenbank SQL/CLR-Debuggen zulassen. Bedenken Sie, dass beim ersten Erstellen der `ManagedDatabaseConstructs` Projekt Visual Studio uns gefragt, ob wir wollten SQL/CLR-Debuggen (siehe Abbildung 6 in Schritt2) aktivieren. Diese Einstellung kann geändert werden, mit der rechten Maustaste auf die Datenbank im Server-Explorer-Fenster.


![Stellen Sie sicher, dass die Datenbank SQL/CLR-Debuggen ermöglicht](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Abbildung 26**: Stellen Sie sicher, dass die Datenbank SQL/CLR-Debuggen ermöglicht


Angenommen, wir, zum Debuggen wollten der `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur. Wir zunächst einen Haltepunkt im Code der Festlegen der `GetProductsWithPriceLessThan` Methode.


[![Legen Sie einen Haltepunkt in der GetProductsWithPriceLessThan-Methode](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Abbildung 27**: Festlegen eines Haltepunkts in der `GetProductsWithPriceLessThan` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Lassen Sie s ein erster Blick auf die verwalteten Datenbankobjekte aus dem SQL Server-Projekt debuggen. Da unsere Projektmappe zwei Projekte - enthält die `ManagedDatabaseConstructs` SQL Server-Projekt zusammen mit unserer Website, um aus dem SQL Server-Projekt debuggen, wir anweisen, starten Sie Visual Studio müssen, die `ManagedDatabaseConstructs` SQL-Server-Projekt, wenn wir das Debuggen starten. Mit der rechten Maustaste die `ManagedDatabaseConstructs` Projekt im Projektmappen-Explorer, und wählen Sie die Gruppe als Startprojekt-Option im Kontextmenü.

Wenn die `ManagedDatabaseConstructs` Projekt aus dem Ausführen die SQL-Anweisungen im Debugger gestartet wird die `Test.sql` -Datei, die im befindet der `Test Scripts` Ordner. Z. B. zum Testen der `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur, ersetzen Sie die vorhandene `Test.sql` Dateiinhalt durch die folgende Anweisung, die aufruft der `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur übergeben die `@CategoryID` 14,95 Wert:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Wenn Sie speichern das obige Skript in eingegeben `Test.sql`, starten Sie das Debuggen, indem Sie dem Debugmenü den Ausnahmebefehl und auswählen, Debugging starten oder durch Drücken von F5 oder die grüne play-Symbol auf der Symbolleiste. Dies wird erstellen Sie die Projekte in der Projektmappe, die verwalteten Datenbankobjekte für die Northwind-Datenbank bereitstellen und führen Sie dann die `Test.sql` Skript. An diesem Punkt wird der Haltepunkt erreicht, und wir können durchlaufen die `GetProductsWithPriceLessThan` -Methode, die Werte der Eingabeparameter prüfen und so weiter.


[![Der Haltepunkt in der Methode GetProductsWithPriceLessThan wurde erreicht.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Abbildung 28**: der Haltepunkt in der `GetProductsWithPriceLessThan` -Methode wurde erreicht ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


In der Reihenfolge für eine SQL-Datenbank zu debuggenden Objekts, durch eine Clientanwendung ist es zwingend erforderlich, dass die Datenbank für die Unterstützung des Debuggens der Anwendung konfiguriert werden. Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und stellen Sie sicher, dass die Anwendungsdebuggen-Option aktiviert ist. Darüber hinaus müssen wir so konfigurieren Sie die ASP.NET-Anwendung für die Integration der SQL-Debugger und Verbindungspooling zu deaktivieren. Diese Schritte wurden erläutert im Detail in Schritt2 der [gespeicherte Prozeduren Debuggen](debugging-stored-procedures-cs.md) Tutorial.

Nachdem Sie die ASP.NET-Anwendung und die Datenbank konfiguriert haben, der ASP.NET-Website als Startprojekt festlegen und mit dem Debuggen beginnen. Wenn Sie eine Seite, die der verwalteten Objekte, die ein Haltepunkt verfügt über eine aufruft besuchen, die Anwendung wird angehalten, und Steuerelement ist jetzt über, die im Debugger, in dem können Sie den Code schrittweise durchlaufen, wie in Abbildung 28 dargestellt.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Schritt 13: Manuell verwalteten kompilieren und Bereitstellen von Datenbankobjekten

SQL Server-Projekte erleichtern das Erstellen, kompilieren und Bereitstellen von verwalteten Datenbankobjekten. SQL Server-Projekte sind leider nur in den Editionen Professional und Team-Systeme von Visual Studio verfügbar. Wenn Sie Visual Web Developer oder der Standard Edition von Visual Studio verwenden und die verwalteten Datenbankobjekte verwenden möchten, müssen Sie Sie manuell erstellen und bereitstellen können. Dies umfasst vier Schritte:

1. Erstellen Sie eine Datei, die den Quellcode für das verwaltete Datenbankobjekt enthält,
2. Kompilieren Sie das Objekt in eine Assembly,
3. Registrieren Sie die Assembly mit der SQL Server 2005-Datenbank, und
4. Erstellen Sie ein Datenbankobjekt in SQL-Server, der auf die entsprechende Methode in der Assembly verweist.

Diese Aufgaben zu veranschaulichen, lassen Sie die s-Erstellen eines neuen verwalteten gespeicherten Prozedur, die Produkte zurückgibt, deren `UnitPrice` ist größer als ein angegebener Wert. Erstellen Sie eine neue Datei auf Ihrem Computer, die mit dem Namen `GetProductsWithPriceGreaterThan.cs` , und geben Sie den folgenden Code in die Datei (Sie können Visual Studio, Editor oder einem Text-Editor, um dies zu erreichen):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Dieser Code ist nahezu identisch mit der `GetProductsWithPriceLessThan` Methode, die in Schritt 5 erstellt haben. Die einzigen Unterschiede sind die Methodennamen die `WHERE` -Klausel und den Namen des Parameters in der Abfrage verwendet. In der `GetProductsWithPriceLessThan` -Methode, die `WHERE` Klausel lesen: `WHERE UnitPrice < @MaxPrice`. Hier im `GetProductsWithPriceGreaterThan`, wir verwenden: `WHERE UnitPrice > @MinPrice` .

Nun müssen wir diese Klasse in eine Assembly zu kompilieren. In der Befehlszeile aus, navigieren Sie zu dem Verzeichnis, in dem Sie gespeichert haben, die `GetProductsWithPriceGreaterThan.cs` Datei und Verwenden von c#-Compiler (`csc.exe`) die Klassendatei in eine Assembly zu kompilieren:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Wenn der Ordner mit `csc.exe` in nicht im System s `PATH`, müssen Sie den Pfad, vollständig zu verweisen `%WINDOWS%\Microsoft.NET\Framework\version\`, wie folgt:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Kompilieren Sie GetProductsWithPriceGreaterThan.cs in eine Assembly.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Abbildung 29**: Kompilieren `GetProductsWithPriceGreaterThan.cs` in eine Assembly ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


Die `/t` -Flag gibt an, dass die C#-Klassendatei in eine DLL-Datei (statt einer ausführbaren Datei) kompiliert werden soll. Die `/out` -Flag gibt an, den Namen der resultierenden Assembly.

> [!NOTE]
> Anstatt Kompilieren der `GetProductsWithPriceGreaterThan.cs` Klassendatei, die über die Befehlszeile an, Sie Alternativ können [Visual c# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) oder erstellen Sie ein separates Class Library-Projekt in Visual Studio Standard Edition. S Ren Jacob Lauritsen hat Ihr Postfach bereitgestellt, solche ein Visual c# Express Edition-Projekt mit Code für die `GetProductsWithPriceGreaterThan` gespeicherte Prozedur und die beiden verwalteten gespeicherte Prozeduren und UDF in den Schritten 3, 5 und 10 erstellt. S Ren s Projekt enthält auch die T-SQL-Befehle erforderlich, um den entsprechenden Datenbankobjekten hinzufügen.


Durch den Code in eine Assembly kompiliert wird können wir die Assembly in der SQL Server 2005-Datenbank zu registrieren. Dies erfolgt mithilfe von T-SQL, mit dem Befehl `CREATE ASSEMBLY`, oder über SQL Server Management Studio. Lassen Sie s den Fokus auf mithilfe von Management Studio.

Erweitern Sie in Management Studio im Ordner Programmierbarkeit der Northwind-Datenbank aus. Eines der Unterordner wird Assemblys. Um die Datenbank manuell eine neue Assembly hinzuzufügen, mit der rechten Maustaste auf den Ordner Assemblys, und wählen Sie die neue Assembly aus dem Kontextmenü aus. Diese zeigt die neue Assembly Dialogfeld (siehe Abbildung 30). Klicken Sie auf die Schaltfläche zum Durchsuchen, wählen die `ManuallyCreatedDBObjects.dll` Assembly, die wir gerade kompiliert, und klicken Sie dann auf OK, um die Assembly mit der Datenbank hinzuzufügen. Sie sollte nicht die `ManuallyCreatedDBObjects.dll` Assembly im Objekt-Explorer.


[![Fügen Sie die ManuallyCreatedDBObjects.dll-Assembly in der Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Abbildung 30**: Hinzufügen der `ManuallyCreatedDBObjects.dll` Assembly mit der Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![Die ManuallyCreatedDBObjects.dll finden Sie im Objekt-Explorer](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Abbildung 31**: die `ManuallyCreatedDBObjects.dll` finden Sie im Objekt-Explorer


Während wir die Assembly mit der Datenbank Northwind hinzugefügt haben, müssen wir noch eine gespeicherte Prozedur mit Zuordnen der `GetProductsWithPriceGreaterThan` Methode in der Assembly. Zu diesem Zweck öffnen Sie ein neues Abfragefenster, und führen Sie das folgende Skript aus:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Dadurch wird eine neue gespeicherte Prozedur erstellt, in der Northwind-Datenbank, die mit dem Namen `GetProductsWithPriceGreaterThan` und ordnet es die verwaltete Methode `GetProductsWithPriceGreaterThan` (dieser befindet sich in der Klasse `StoredProcedures`, die in der Assembly ist `ManuallyCreatedDBObjects`).

Nach dem Ausführen des obigen Skripts, aktualisieren Sie den Ordner für gespeicherte Prozeduren im Objekt-Explorer. Daraufhin sollte einen neuer Eintrag der gespeicherten Prozedur - `GetProductsWithPriceGreaterThan` -IValidator.h ein Schlosssymbol angezeigt. Um diese gespeicherte Prozedur zu testen, geben Sie ein, und führen Sie im Abfragefenster das folgende Skript aus:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Wie in Abbildung 32 dargestellt, handelt es sich bei der obigen Befehl zeigt Informationen für diese Produkte mit einem `UnitPrice` 24,95 $ größer.


[![Die ManuallyCreatedDBObjects.dll finden Sie im Objekt-Explorer](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Abbildung 32**: die `ManuallyCreatedDBObjects.dll` finden Sie im Objekt-Explorer ([klicken Sie, um das Bild in voller Größe anzeigen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Zusammenfassung

Microsoft SQL Server 2005 bietet Integration mit der Common Language Runtime (CLR), dem Datenbankobjekte mit verwaltetem Code erstellt werden können. Früher, wird diese Datenbankobjekte können nur mit T-SQL erstellt werden, aber nun können wir diese Objekte mit .NET-Programmiersprachen wie c# erstellen. In diesem Tutorial erstellten verwaltet zwei gespeicherte Prozeduren und eine verwaltete benutzerdefinierte Funktion.

Visual Studio s SQL Server-Projekt-Typ erleichtert das Erstellen, kompilieren und Bereitstellen von verwalteten Datenbankobjekten. Darüber hinaus bietet sie umfassende Debugunterstützung. SQL Server-Projekt-Typen sind jedoch nur in den Editionen Professional und Team-Systeme von Visual Studio verfügbar. Für diejenigen muss mithilfe von Visual Web Developer oder der Standard Edition von Visual Studio, die Erstellung, Kompilierung und Bereitstellung manuell ausgeführt werden wie in Schritt 13 beschrieben.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Vor- und Nachteile von benutzerdefinierten Funktionen](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Erstellen von SQL Server 2005-Objekte in verwaltetem Code](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Erstellen von Triggern, die mithilfe von verwaltetem Code in SQLServer 2005](http://www.15seconds.com/issue/041006.htm)
- [Gewusst wie: Erstellen und führen Sie eine CLR-SQL Server-Prozedur](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Gewusst wie: Erstellen und führen Sie eine CLR-SQL-Server eine benutzerdefinierte Funktion](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Gewusst wie: Bearbeiten der `Test.sql` Skript zum Ausführen von SQL-Objekte](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Einführung in Benutzer, benutzerdefinierte Funktionen](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Verwalteter Code und SQLServer 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Transact-SQL-Referenz](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Exemplarische Vorgehensweise: Erstellen einer gespeicherten Prozedur in verwaltetem Code](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde S Ren Jacob Lauritsen. Neben diesen Artikel, S Ren auch erstellt, das Visual c# Express Edition-Projekt, das in diesem Artikel s Download manuell kompilieren die verwalteten Datenbankobjekte enthalten. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](debugging-stored-procedures-cs.md)
> [Weiter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
