---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: Erstellen von gespeicherten Prozeduren und benutzerdefinierten Funktionen mit verwaltetem Code (c#) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 wird in der .NET Common Language Runtime ermöglichen Entwicklern das Erstellen von Datenbankobjekten durch verwalteten Code integriert. In diesem Lernprogramm...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 5a860c8ab6ad7ff04de2175900491d532db782d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>Erstellen gespeicherter Prozeduren und benutzerdefinierte Funktionen mit verwaltetem Code (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip) oder [PDF herunterladen](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 wird in der .NET Common Language Runtime ermöglichen Entwicklern das Erstellen von Datenbankobjekten durch verwalteten Code integriert. Dieses Lernprogramm wird gezeigt, wie verwaltete gespeicherte Prozeduren zu erstellen und benutzerdefinierte Funktionen mit Visual Basic oder C#-Code verwaltet. Wir sehen auch, wie diese Editionen von Visual Studio Sie solche verwalteten Datenbankobjekte zu debuggen können.


## <a name="introduction"></a>Einführung

S für Microsoft SQL Server 2005-Datenbanken verwenden den [Transact-Structured Query Language (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) zum Einfügen, ändern und Abrufen von Daten. Die meisten Datenbanksysteme enthalten Konstrukte zum Gruppieren einer Reihe von SQL-Anweisungen, die dann als einzelne, wiederverwendbare Einheit ausgeführt werden können. Gespeicherte Prozeduren sind ein Beispiel. Ein weiterer Vorteil ist *benutzerdefinierte Funktionen*(UDFs), ein Konstrukt, das Untersuchen wir ausführlich in Schritt 9.

Im Kern dient SQL zum Arbeiten mit Datasets. Die `SELECT`, `UPDATE`, und `DELETE` Anweisungen grundsätzlich gelten für alle Datensätze in der entsprechenden Tabelle und sind nur über eingeschränkte durch ihre `WHERE` Klauseln. Es gibt noch zahlreiche Sprachfunktionen zum Arbeiten mit einem Datensatz zu einem Zeitpunkt und zum Bearbeiten von skalaren Daten. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) für eine Gruppe von Datensätzen looped über jeweils einzeln zu ermöglichen. String-Funktionen zur Zeichenfolgenmanipulation wie `LEFT`, `CHARINDEX`, und `PATINDEX` funktionieren mit skalaren Daten. SQL enthält auch Anweisungen für die ablaufsteuerung wie `IF` und `WHILE`.

Vor der Microsoft SQL Server 2005 können gespeicherte Prozeduren und benutzerdefinierten Funktionen nur als eine Auflistung von T-SQL-Anweisungen definiert werden. SQL Server 2005, jedoch wurde entworfen, um Integration mit Bereitstellen der [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), also die Laufzeit verwendet, die von allen .NET Assemblys. Daher können die gespeicherten Prozeduren und benutzerdefinierten Funktionen in einer SQL Server 2005-Datenbank erstellt werden mithilfe von verwaltetem Code. D. h., können Sie eine gespeicherte Prozedur oder benutzerdefinierten Funktion als Methode in einer C#-Klasse erstellen. Dadurch werden diese gespeicherten Prozeduren und benutzerdefinierten Funktionen aus, die die Funktionalität in .NET Framework und eigene benutzerdefinierte Klassen.

In diesem Lernprogramm wird untersucht gespeichert verwalteten Erstellen von Prozeduren und benutzerdefinierte Funktionen und wie diese in die Northwind-Datenbank integriert. Lassen Sie s beginnen!

> [!NOTE]
> Verwaltete Datenbankobjekte bieten einige Vorteile gegenüber dem ihre SQL-Entsprechungen. Sprache Reichhaltigkeit und vertraut sind und der Möglichkeit zur Wiederverwendung vorhandener Code und Logik sind die wichtigsten Vorteile. Aber Datenbankobjekte sind wahrscheinlich weniger effizient sein, bei der Arbeit mit Datasets, die nicht viel prozeduralen Logik beinhalten. Eine ausführlichere Beschreibung über die Vorteile der Verwendung von verwalteten Codes im Vergleich zu T-SQL, sehen Sie sich die [Vorteile von verwaltetem Code bei der Erstellung von Datenbankobjekten](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Schritt 1: Verschieben der Northwind-Datenbank aus`App_Data`

Alle unseren Tutorials bisher haben verwendet eine Microsoft SQL Server 2005 Express Edition-Datenbankdatei in der Web-Anwendung s `App_Data` Ordner. Platzieren Sie die Datenbank im `App_Data` vereinfacht die Verteilung und diese Lernprogramme ausführen, wie alle Dateien in ein Verzeichnis gefunden wurden, und So testen Sie das Lernprogramm keine zusätzliche Konfigurationsschritte erforderlich.

Für dieses Lernprogramm jedoch Let s verschieben die Northwind-Datenbank von `App_Data` und registrieren Sie ihn explizit mit der Instanz des SQL Server 2005 Express Edition-Datenbank. Während es die Schritte, für dieses Lernprogramm mit der Datenbank in ausführen können den `App_Data` Ordner, eine Reihe von den Schritten erfolgen viel einfacher durch das explizite Registrieren der Datenbank mit der Instanz des SQL Server 2005 Express Edition-Datenbank.

Der Download für dieses Lernprogramm hat zwei Datenbankdateien - `NORTHWND.MDF` und `NORTHWND_log.LDF` - platzierten in einem Ordner namens `DataFiles`. Wenn Sie zusammen mit Ihren eigenen Implementierung der Lernprogramme vorgehen, schließen Sie Visual Studio, und Verschieben der `NORTHWND.MDF` und `NORTHWND_log.LDF` Dateien von der Website s `App_Data` Ordner in einen Ordner außerhalb der Website. Nachdem die Datenbankdateien in einen anderen Ordner verschoben wurden müssen wir die Northwind-Datenbank mit der Instanz des SQL Server 2005 Express Edition-Datenbank zu registrieren. Dies kann in SQL Server Management Studio erfolgen. Wenn Sie eine nicht - Express Edition von SQL Server 2005 auf Ihrem Computer installiert haben müssen Sie wahrscheinlich bereits Management Studio installiert. Wenn Sie nur SQL Server 2005 Express Edition auf Ihrem Computer verfügen, Sie nehmen einen Moment Zeit zum Herunterladen und installieren [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Starten Sie SQL Server Management Studio. Wie in Abbildung 1 gezeigt, startet Management Studio, indem Sie gefragt werden, welcher Server für die Verbindung. Geben Sie Localhost\SQLExpress für den Servernamen, wählen Sie die Windows-Authentifizierung in der Dropdownliste Authentifizierung aus, und klicken Sie auf Verbinden.


![Herstellen einer Verbindung mit der entsprechenden Datenbank-Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**Abbildung 1**: Herstellen einer Verbindung mit der entsprechenden Datenbank-Instanz


Nachdem Sie Ve hergestellt ist, werden die Objekt-Explorer-Fenster Informationen über die SQL Server 2005 Express Edition-Datenbankinstanz, einschließlich Datenbanken, Sicherheitsinformationen, Verwaltungsoptionen, und So weiter aufgelistet.

Wir müssen in die Northwind-Datenbank Anfügen der `DataFiles` Ordner (oder ablegen, wo Sie es verschoben haben können) und SQL Server 2005 Express Edition-Datenbankinstanz. Mit der rechten Maustaste auf den Ordner "Datenbanken", und wählen Sie die Attach-Option im Kontextmenü. Hierdurch wird das Dialogfeld Anfügen von Datenbanken angezeigt. Klicken Sie auf die Schaltfläche "hinzufügen", die entsprechende Drilldown `NORTHWND.MDF` Datei, und klicken Sie auf OK. An diesem Punkt sollte Ihr Bildschirm ähnlich wie in Abbildung 2 aussehen.


[![Herstellen einer Verbindung mit der entsprechenden Datenbank-Instanz](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**Abbildung 2**: Herstellen einer Verbindung mit der entsprechenden Datenbank-Instanz ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> Bei der Verbindung der SQL Server 2005 Express Edition-Instanz mithilfe von Management Studio lässt das Dialogfeld Anfügen von Datenbanken nicht um einen Drilldown in Verzeichnissen nach Benutzer Profil, z. B. Arbeitsplatz. Stellen Sie daher sicher an die `NORTHWND.MDF` und `NORTHWND_log.LDF` Dateien in einem Profilverzeichnis nichtbenutzer-.


Klicken Sie auf die Schaltfläche "OK", um die Datenbank anzufügen. Das Anfügen von Datenbanken-Dialogfeld wird geschlossen, und im Objekt-Explorer sollte nun die Datenbank gerade angefügt auflisten. Wahrscheinlich werden der Northwind-Datenbank einen Namen wie hat `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Benennen Sie die Datenbank zur Northwind-Datenbank, indem Sie mit der rechten Maustaste auf die Datenbank und umbenennen.


![Benennen Sie die Datenbank an Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**Abbildung 3**: Benennen Sie die Datenbank an Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Schritt 2: Erstellen eine neue Projektmappe und SQL Server-Projekt in Visual Studio

Zum Erstellen von verwalteten gespeicherten Prozeduren oder benutzerdefinierten Funktionen in SQL Server 2005 werden wir die gespeicherte Prozedur und die UDF-Logik als C#-Code in einer Klasse schreiben. Nachdem der Code geschrieben wurde, müssen wir diese Klasse in eine Assembly kompilieren (eine `.dll` Datei), registrieren Sie die Assembly mit SQL Server-Datenbank und erstellen Sie eine gespeicherte Prozedur oder ein UDF-Objekt in der Datenbank, die auf die entsprechende Methode in verweist die Assembly. Diese Schritte können alle manuell ausgeführt werden. Wir können erstellen Sie den Code in einem beliebigen Text-Editor, kompilieren Sie ihn über die Befehlszeile mit der C#-Compiler ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx)), registrieren Sie ihn für die Datenbank mithilfe der [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) Befehl oder aus der Verwaltung Studio, und fügen Sie die gespeicherte Prozedur oder eine UDF-Objekts auf ähnliche Weise. Glücklicherweise enthalten die Professional und Team-Systeme Versionen von Visual Studio ein SQL Server-Projekt, das diese Aufgaben automatisiert. In diesem Lernprogramm wird von protokollsuchen mit der SQL Server-Projekttyp, erstellen Sie eine verwaltete gespeicherte Prozedur und der UDF.

> [!NOTE]
> Wenn Sie Visual Web Developer oder der Standard Edition von Visual Studio verwenden, müssen Sie stattdessen die manuelle Methode zu verwenden. Schritt 13 enthält detaillierte Anweisungen zum Ausführen dieser Schritte manuell. Ich empfehlen Ihnen, lesen Sie die Schritte 2 bis 12 vor dem Schritt 13 lesen, da diese Schritte wichtige Hinweise zur Konfiguration von SQL Server, die angewendet werden muss enthält, unabhängig davon, welche Version von Visual Studio, die Sie verwenden.


Starten Sie Visual Studio öffnen. Wählen Sie über das Menü Datei, neues Projekt, um das Dialogfeld "Neues Projekt" angezeigt (siehe Abbildung 4). Drilldown zu den Datenbank-Projekttyp, und wählen Sie dann aus den Vorlagen aufgeführt, die auf der rechten Seite, um ein neues SQL Server-Projekt zu erstellen. Ich haben sich entschieden, nennen Sie dieses Projekt `ManagedDatabaseConstructs` und platziert es in einer Projektmappe mit dem Namen `Tutorial75`.


[![Erstellen eines neuen SQL Server-Projekts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**Abbildung 4**: Erstellen eines neuen SQL Server-Projekts ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


Klicken Sie auf die Schaltfläche "OK", klicken Sie im Dialogfeld Neues Projekt zum Erstellen der Projektmappe und SQL Server-Projekt.

Ein SQL Server-Projekt ist mit einer bestimmten Datenbank gebunden. Folglich werden nach dem Erstellen der neuen SQL Server-Projekts wir sofort aufgefordert, diese Informationen angeben. Abbildung 5 zeigt das Dialogfeld neue Datenbankverweis, die auf der Northwind-Datenbank zu verweisen, die wir in der Instanz des SQL Server 2005 Express Edition-Datenbank wieder in Schritt 1 registriert ausgefüllt wurden.


![Ordnen Sie das SQL Server-Projekt mit der Northwind-Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**Abbildung 5**: Ordnen Sie das SQL Server-Projekt mit der Northwind-Datenbank


Um die verwaltete gespeicherte Prozeduren und benutzerdefinierte Funktionen, die innerhalb dieses Projekts erstellen, zu debuggen, müssen wir SQL/CLR-debugging-Unterstützung für die Verbindung zu aktivieren. Wenn ein SQL Server-Projekt eine neue Datenbank zuordnen (wie in Abbildung 5), Visual Studio werden wir gefragt, wenn wir SQL/CLR-Debuggen für die Verbindung aktivieren möchten (siehe Abbildung 6). Klicken Sie auf "Ja".


![SQL/CLR-Debuggen aktivieren](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**Abbildung 6**: SQL/CLR-Debuggen aktivieren


An diesem Punkt wurde das neue SQL Server-Projekt zur Projektmappe hinzugefügt wurde. Sie enthält einen Ordner namens `Test Scripts` mit einer Datei namens `Test.sql`, dient zum Debuggen verwalteter Datenbankobjekte in das Projekt erstellt. Betrachten wir Debuggen in Schritt 12 fort.

Wir können jetzt die neue verwaltete gespeicherte Prozeduren und benutzerdefinierte Funktionen hinzuzufügen, zu diesem Projekt, aber bevor wir lassen einschließen s unsere vorhandene Webanwendung zuerst in der Projektmappe. Wählen Sie die Option "hinzufügen", und wählen Sie die vorhandene Website, über das Menü Datei. Navigieren Sie zu der entsprechenden Websiteordner, und klicken Sie auf OK. Abbildung 7 zeigt ein Beispiel, wird dadurch die Lösung, um die zwei Projekte umfassen aktualisieren: die Website und die `ManagedDatabaseConstructs` SQL Server-Projekt.


![Im Projektmappen-Explorer enthält jetzt zwei Projekte](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**Abbildung 7**: im Projektmappen-Explorer enthält jetzt zwei Projekte


Die `NORTHWNDConnectionString` Wert in `Web.config` derzeit verweist auf die `NORTHWND.MDF` in der Datei die `App_Data` Ordner. Da wir diese Datenbank von entfernt `App_Data` und registrieren Sie es explizit in der Instanz des SQL Server 2005 Express Edition-Datenbank, müssen wir entsprechend aktualisieren der `NORTHWNDConnectionString` Wert. Öffnen der `Web.config` -Datei in die Website und die Änderung der `NORTHWNDConnectionString` Wert so, dass die Verbindungszeichenfolge lautet: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Nach dieser Änderung Ihrer `<connectionStrings>` im Abschnitt `Web.config` sollte etwa wie folgt aussehen:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> Wie in beschrieben die [vorherigen Lernprogramm](debugging-stored-procedures-cs.md), beim Debuggen von SQL Server-Objekt aus einer Clientanwendung, z. B. einer ASP.NET-Website müssen wir Verbindungspooling zu deaktivieren. Die Verbindungszeichenfolge, die oben gezeigte deaktiviert Verbindungspooling ( `Pooling=false` ). Wenn Sie nicht, zum Debuggen von verwalteten gespeicherten Prozeduren und benutzerdefinierten Funktionen aus der ASP.NET-Website beabsichtigen, aktivieren Sie Verbindungspooling.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Schritt 3: Erstellen eine verwaltete gespeicherte Prozedur

Eine verwaltete gespeicherte Prozedur mit der Datenbank Northwind hinzufügen müssen Sie zuerst die gespeicherte Prozedur als eine Methode in der SQL Server-Projekt zu erstellen. Projektmappen-Explorer mit der Maustaste auf die `ManagedDatabaseConstructs` Teamprojektnamen aus, und wählen Sie ein neues Element hinzufügen. Dies wird im Dialogfeld "Neues Element hinzufügen" angezeigt, die Typen verwalteter Datenbankobjekte aufgeführt, die dem Projekt hinzugefügt werden können. Wie in Abbildung 8 gezeigt, enthält diese gespeicherten Prozeduren und benutzerdefinierte Funktionen, o. ä.

Lassen Sie s starten, indem Sie eine gespeicherte Prozedur, die einfach alle Produkte zurückgibt, die eingestellt wurden. Nennen Sie die neue gespeicherte Prozedurdatei `GetDiscontinuedProducts.cs`.


[![Hinzufügen einer neuen gespeicherten Prozedur, die mit dem Namen GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**Abbildung 8**: Hinzufügen einer neuen gespeicherten Prozedur mit dem Namen `GetDiscontinuedProducts.cs` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


Dadurch wird eine neue C#-Klassendatei mit folgendem Inhalt erstellt:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

Beachten Sie, die die gespeicherte Prozedur als implementiert ist eine `static` Methode innerhalb einer `partial` Klassendatei namens `StoredProcedures`. Darüber hinaus die `GetDiscontinuedProducts` Methode ergänzt wird, mit der `SqlProcedure attribute`, die kennzeichnet, dass die Methode als gespeicherte Prozedur.

Der folgende Code erstellt ein `SqlCommand` -Objekt und stellt seine `CommandText` auf eine `SELECT` Abfrage, die alle Spalten aus zurückgibt der `Products` Tabelle für Produkte, deren `Discontinued` Feld gleich 1. Anschließend führt den Befehl und sendet die Ergebnisse an die Clientanwendung zurück. Fügen Sie diesen Code, um die `GetDiscontinuedProducts` Methode.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

Alle verwalteten Datenbankobjekte haben Zugriff auf eine [ `SqlContext` Objekt](https://msdn.microsoft.com/library/ms131108.aspx) , die den Kontext des Aufrufers darstellt. Die `SqlContext` ermöglicht den Zugriff auf eine [ `SqlPipe` Objekt](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) über seine [ `Pipe` Eigenschaft](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Dies `SqlPipe` Objekt wird verwendet, um Informationen zwischen SQL Server-Datenbank und die aufrufende Anwendung Fähre. Wie der Name schon sagt, den [ `ExecuteAndSend` Methode](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) führt eine übergeben im `SqlCommand` -Objekt und sendet die Ergebnisse an die Clientanwendung zurück.

> [!NOTE]
> Verwaltete Datenbankobjekte eignen sich am besten für gespeicherte Prozeduren und benutzerdefinierte Funktionen, die setbasierte Logik, anstatt prozeduralen Logik verwenden. Prozedurale Logik umfasst das Arbeiten mit Sätzen von Daten auf eine Zeile für Zeile Basis von oder Arbeiten mit skalaren Daten. Die `GetDiscontinuedProducts` Methode, die soeben erstellt, jedoch wurde werden keine prozeduralen Logik. Aus diesem Grund würde es Idealerweise als T-SQL-Prozedur implementiert werden. Die Implementierung erfolgt als eine verwaltete gespeicherte Prozedur veranschaulicht die erforderlichen Schritte zum Erstellen und Bereitstellen von gespeicherten Prozeduren verwaltet.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Schritt 4: Bereitstellen von verwalteten gespeicherten Prozedur

Mit dem folgenden Code, der abgeschlossen ist können sie mit der Northwind-Datenbank bereitstellen. Bereitstellen eines SQL Server-Projekts der Code in eine Assembly kompiliert, registriert die Assembly mit der Datenbank und erstellt die entsprechenden Objekte in der Datenbank, in die entsprechenden Methoden in der Assembly verknüpfen. Der exakte Satz von Tasks, die Option bereitstellen, wird genauer gesagt in Schritt 13 ausgeschrieben. Mit der rechten Maustaste auf die `ManagedDatabaseConstructs` Projektname im Projektmappen-Explorer, und wählen Sie die Option bereitstellen. Allerdings wird die Bereitstellung mit dem folgenden Fehler: falsche Syntax in der Nähe von "Extern". Sie müssen möglicherweise den Kompatibilitätsgrad der aktuellen Datenbank einen höheren Wert zum Aktivieren dieser Funktion festlegen. Finden Sie Hilfe für die gespeicherte Prozedur `sp_dbcmptlevel`.

Diese Fehlermeldung tritt auf, wenn bei dem Versuch, die Assembly mit der Northwind-Datenbank zu registrieren. Um eine Assembly mit einer SQL Server 2005-Datenbank zu registrieren, muss der Kompatibilitätsgrad der Datenbank-s auf 90 festgelegt werden. Standardmäßig ist bei neuen SQL Server 2005-Datenbanken einen Kompatibilitätsgrad von 90. Allerdings haben Datenbanken mit Microsoft SQL Server 2000 erstellt einen Standard-Kompatibilitätsgrad von 80. Seit die Northwind-Datenbank zunächst mit einer Microsoft SQL Server 2000-Datenbank war, wird ihr Kompatibilitätsgrad derzeit auf 80 festgelegt ist und muss daher 90 erhöht werden, um Datenbankobjekte zu registrieren.

Um die Datenbank-Kompatibilitätsgrad s zu aktualisieren, öffnen Sie ein neues Abfragefenster in Management Studio, und geben Sie:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

Klicken Sie auf das Symbol "ausführen" in der Symbolleiste auf die vorangehende Abfrage ausführen.


[![Der Kompatibilitätsgrad der Datenbank Northwind s aktualisieren](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**Abbildung 9**: Aktualisieren der Datenbank "Northwind" s-Kompatibilitätsgrad ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


Nach dem Aktualisieren des Kompatibilitätsgrads, erneut bereitstellen Sie in das SQL Server-Projekt. Dieses Mal sollte die Bereitstellung ohne Fehler abgeschlossen.

Zurück zu SQL Server Management Studio, mit der rechten Maustaste auf die Northwind-Datenbank im Objekt-Explorer, und wählen Sie aktualisieren. Als Nächstes Drilldown in den Ordner Programmierbarkeit, und erweitern Sie dann den Ordner Assemblys. Wie in Abbildung 10 gezeigt, enthält die Northwind-Datenbank jetzt die Assembly generiert, indem die `ManagedDatabaseConstructs` Projekt.


![Die ManagedDatabaseConstructs-Assembly ist nun registriert, mit der Northwind-Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**Abbildung 10**: die `ManagedDatabaseConstructs` Assembly ist nun registriert, mit der Northwind-Datenbank


Erweitern Sie auch den Ordner gespeicherte Prozeduren aus. Es wird eine gespeicherte Prozedur namens angezeigt `GetDiscontinuedProducts`. Diese gespeicherte Prozedur erstellt wurde, durch den Bereitstellungsvorgang und verweist auf die `GetDiscontinuedProducts` Methode in der `ManagedDatabaseConstructs` Assembly. Wenn die `GetDiscontinuedProducts` gespeicherte Prozedur ausgeführt wird, davon wiederum führt die `GetDiscontinuedProducts` Methode. Da dies eine verwaltete gespeicherte Prozedur ist nicht bearbeitet werden über Management Studio (also das Schlosssymbol neben dem Namen der gespeicherten Prozedur).


![Die GetDiscontinuedProducts gespeicherte Prozedur ist in den Ordner für die gespeicherten Prozeduren aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**Abbildung 11**: die `GetDiscontinuedProducts` gespeicherte Prozedur wird in den Ordner für die gespeicherten Prozeduren aufgeführt.


Es ist immer noch eine weitere Hürde, wir haben, zu umgehen, bevor die verwaltete gespeicherte Prozedur aufgerufen werden kann: für die Ausführung von verwaltetem Code zu verhindern, dass die Datenbank konfiguriert ist. Dies überprüfen, indem Sie ein neues Abfragefenster öffnen und Ausführen der `GetDiscontinuedProducts` gespeicherten Prozedur. Sie erhalten die folgende Fehlermeldung angezeigt: die Ausführung von Benutzercode in .NET Framework ist deaktiviert. Aktivieren Sie die Konfigurationsoption für Clr-fähig.

Überprüfen Sie die Konfigurationsinformationen der Northwind-Datenbank s, geben aus, und führen Sie den Befehl `exec sp_configure` in das Abfragefenster ein. Dies zeigt, dass die Clr-fähig festlegen auf 0 festgelegt ist.


[![Die Clr-fähig Einstellung ist zurzeit auf 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**Abbildung 12**: der Clr-fähig Einstellung ist zurzeit auf 0 ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


Beachten Sie, dass jede Konfigurationseinstellung in Abbildung 12 vier Werte aufgeführt, die mit ihm hat: die minimale und maximale Werte und die Konfiguration und Ausführungswert. Um den Konfigurationswert für die Clr-fähig-Einstellung zu aktualisieren, führen Sie den folgenden Befehl ein:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

Wenn Sie erneut ausführen, die `exec sp_configure` sehen Sie, dass die obige Anweisung auf 1 den Clr-fähig s Config Einstellungswert aktualisiert, aber der Ausführungswert weiterhin auf 0 festgelegt ist. Damit diese konfigurationsänderung wirksam müssen zum Ausführen der [ `RECONFIGURE` Befehl](https://msdn.microsoft.com/library/ms176069.aspx), auf den aktuellen Konfigurationswert den Ausführungswert festzulegen. Geben Sie einfach `RECONFIGURE` in das Abfragefenster ein, und klicken Sie auf das Symbol "ausführen" in der Symbolleiste. Wenn das Ausführen `exec sp_configure` nun sollten Sie einen Wert von 1 für die Clr-fähig Einstellung s abgebildete sehen und Werte ausführen.

Mit der Clr-fähig-Konfiguration abgeschlossen ist, werden wir für die die verwaltete Ausführung bereit `GetDiscontinuedProducts` gespeicherte Prozedur. Geben Sie in das Abfragefenster ein, und führen Sie den Befehl `exec` `GetDiscontinuedProducts`. Den entsprechenden verwalteten Code im Aufruf der gespeicherten Prozedur bewirkt, dass die `GetDiscontinuedProducts` Methode ausgeführt. Dieser Code stellt eine `SELECT` Abfrage alle Produkte zurückgegeben, die nicht mehr und gibt diese Daten an die aufrufende Anwendung, die SQL Server Management Studio in dieser Instanz ist. Management Studio erhält diese Ergebnisse und zeigt sie im Ergebnisfenster.


[![Die gespeicherte Prozedur alle gibt GetDiscontinuedProducts nicht mehr unterstützte Produkte](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**Abbildung 13**: die `GetDiscontinuedProducts` gespeicherte Prozedur gibt alle nicht mehr unterstützte Produkte ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Schritt 5: Erstellen von verwalteten Prozeduren, die Eingabeparameter akzeptieren

Viele der Abfragen und gespeicherte Prozeduren, in der gesamten Ausführung dieser Lernprogramme erstellt wurde verwendet haben *Parameter*. Z. B. in der [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Lernprogramm wir eine gespeicherte Prozedur namens erstellt `GetProductsByCategoryID` , die einen Eingabeparameter mit dem Namen akzeptiert `@CategoryID`. Die gespeicherte Prozedur wird dann alle Produkte zurückgegeben, deren `CategoryID` Feld übereinstimmen, den Wert des angegebenen `@CategoryID` Parameter.

Geben Sie einfach diese Parameter in der Methodendefinition s, um eine verwaltete gespeicherte Prozedur zu erstellen, die Eingabeparameter akzeptiert. Um dies zu veranschaulichen, Let s eine andere verwaltete gespeicherte Prozedur zum Hinzufügen der `ManagedDatabaseConstructs` Projekt mit dem Namen `GetProductsWithPriceLessThan`. Diese verwaltete gespeicherte Prozedur Eingabeparameter angeben einen Preis akzeptiert und gibt alle Produkte zurück, dessen `UnitPrice` Feld ist kleiner als der Wert des s-Parameters.

Um das Projekt eine neue gespeicherte Prozedur hinzuzufügen, mit der Maustaste auf die `ManagedDatabaseConstructs` Teamprojektnamen aus, und wählen Sie zum Hinzufügen einer neuen gespeicherten Prozedur. Nennen Sie die Datei `GetProductsWithPriceLessThan.cs`. Dadurch wird zunächst eine neue C#-Klassendatei mit einer Methode mit dem Namen erstellt, wie in Schritt 3 veranschaulichte Filtermenü, `GetProductsWithPriceLessThan` Hauptvideos der `partial` Klasse `StoredProcedures`.

Update der `GetProductsWithPriceLessThan` Definition s-Methode, sodass dieser akzeptiert eine [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) Eingabeparameter mit dem Namen `price` und den Code schreiben, auszuführen und die Ergebnisse der Abfrage zurückzugeben:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

Die `GetProductsWithPriceLessThan` Methodendefinition s als auch Code ähnlich der Definition und den Code der `GetDiscontinuedProducts` Methode, die in Schritt 3 erstellt haben. Die einzigen Unterschiede sind, die die `GetProductsWithPriceLessThan` Methode als Eingabeparameter akzeptiert (`price`), die `SqlCommand` s-Abfrage enthält einen Parameter (`@MaxPrice`), und ein Parameter hinzugefügt wird die `SqlCommand` s `Parameters` Auflistung ist und der Wert des der `price` Variable.

Nach dem Hinzufügen dieses Codes erneut bereitstellen Sie in SQL Server-Projekt. Klicken Sie dann zurück zu SQL Server Management Studio, und aktualisieren Sie den Ordner gespeicherte Prozeduren. Daraufhin sollte einen neuen Eintrag `GetProductsWithPriceLessThan`. Geben Sie vom einem Abfragefenster, und führen Sie den Befehl `exec GetProductsWithPriceLessThan 25`, die Liste aller Produkte $25, kleiner wird, wie in Abbildung 14 gezeigt.


[![Unter $25 Produkte angezeigt werden](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**Abbildung 14**: Produkte unter $25 werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Schritt 6: Verwaltete gespeicherte Prozedur von der Datenzugriffsebene aufrufen

Zu diesem Zeitpunkt haben wir hinzugefügt der `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` zu gespeicherte Prozeduren verwaltet die `ManagedDatabaseConstructs` Projekt, und sie mit der Northwind-SQL Server-Datenbank registriert haben. Wir auch diese verwalteten gespeicherten Prozeduren von SQL Server Management Studio aufgerufen (siehe Abbildung s 13 und 14). In der Reihenfolge für unsere ASP.NET verwalteten Anwendung für die Verwendung dieser gespeicherte Prozeduren, allerdings müssen wir ihnen den Datenzugriff und Geschäftsschichten der Logik in der Architektur hinzufügen. In diesem Schritt fügen wir zwei neue Methoden zum der `ProductsTableAdapter` in der `NorthwindWithSprocs` typisierte DataSet, das anfänglich in erstellt wurde die [Erstellen neuer gespeicherter Prozeduren für die typisierte DataSet s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Lernprogramm. In Schritt 7 werden die BLL entsprechende Methoden hinzugefügt.

Öffnen der `NorthwindWithSprocs` typisierte DataSet in Visual Studio und beginnen Sie, indem Sie eine neue Methode zum Hinzufügen der `ProductsTableAdapter` mit dem Namen `GetDiscontinuedProducts`. Um einen TableAdapter eine neue Methode hinzugefügt haben, mit der rechten Maustaste auf den Namen des TableAdapter s in den Designer, und wählen Sie aus dem Kontextmenü die Option der Abfrage hinzufügen.

> [!NOTE]
> Da wir ein Verschieben der Datenbank Northwind aus der `App_Data` Ordner mit der Instanz des SQL Server 2005 Express Edition-Datenbank, es ist unbedingt erforderlich, dass die entsprechende Verbindungszeichenfolge in der Datei "Web.config" aktualisiert werden, damit diese Änderung angezeigt. In Schritt2 besprochen Aktualisieren der `NORTHWNDConnectionString` Wert in `Web.config`. Wenn Sie vergessen haben diese Aktualisierung durchführen, werden Sie die Fehlermeldung "Fehler" zu Abfrage hinzufügen angezeigt. Kann nicht gefunden werden Verbindung `NORTHWNDConnectionString` für Objekt `Web.config` in einem Dialogfeld beim Versuch, eine neue Methode des TableAdapter hinzugefügt. Um diesen Fehler zu beheben, klicken Sie auf OK, und fahren Sie mit `Web.config` und Aktualisieren der `NORTHWNDConnectionString` -Wert wie in Schritt2 beschrieben. Wiederholen Sie dann erneut die-Methode dem TableAdapter hinzufügen. Dieses Mal sollte es ohne Fehler verwendet werden.


Hinzufügen einer neuen Methode startet die TableAdapter-Abfrage-Konfigurations-Assistenten, in dem wir in vergangenen Lernprogramme oft verwendet haben. Der erste Schritt fordert uns an, wie der TableAdapter den Datenbankzugriff sollte: über eine Ad-hoc-SQL-Anweisung oder über eine neue oder vorhandene gespeicherte Prozedur. Da wir bereits erstellt und registriert wurden die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur mit der Datenbank, wählen Sie die vorhandene verwenden gespeicherte Prozedur-Option, und klicken Sie weiter.


[![Wählen Sie die vorhandene gespeicherte Prozedur Option Verwendung](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**Abbildung 15**: Wählen Sie die vorhandene verwenden gespeicherte Prozedur Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


Der nächste Bildschirm fordert uns für die gespeicherte Prozedur, die die Methode aufruft. Wählen Sie die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur aus der Dropdown-Liste, und klicken Sie weiter.


[![Wählen Sie die GetDiscontinuedProducts verwalteten gespeicherten Prozedur](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**Abbildung 16**: Wählen Sie die `GetDiscontinuedProducts` verwaltete gespeicherte Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


Wir werden dann aufgefordert, um anzugeben, ob die gespeicherte Prozedur Zeilen, die einen einzelnen Wert oder nichts zurückgibt. Da `GetDiscontinuedProducts` gibt die Menge zurück, der nicht mehr unterstützte Produktzeilen, wählen Sie die erste Option (Tabular Data), und klicken Sie auf Weiter.


[![Wählen Sie die Tabellendaten-Option](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**Abbildung 17**: Wählen Sie die tabellarischen Daten-Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


Der letzten Seite des Assistenten kann wir die Datenzugriffsmuster verwendet und die Namen der resultierenden Methoden angeben. Lassen Sie die Kontrollkästchen aktiviert und Namen der Methoden `FillByDiscontinued` und `GetDiscontinuedProducts`. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.


[![Name der Methoden FillByDiscontinued und GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**Abbildung 18**: Benennen Sie die Methoden `FillByDiscontinued` und `GetDiscontinuedProducts` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


Wiederholen Sie diese Schritte zum Erstellen von Methoden, die mit dem Namen `FillByPriceLessThan` und `GetProductsWithPriceLessThan` in der `ProductsTableAdapter` für die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur.

Abbildung 19 zeigt einen Screenshot der DataSet-Designer nach dem Hinzufügen der Methoden der `ProductsTableAdapter` für die `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwalteten gespeicherte Prozeduren.


[![Die ProductsTableAdapter enthält die neue Methoden, die in diesem Schritt hinzugefügt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**Abbildung 19**: die `ProductsTableAdapter` enthält neue Methoden hinzugefügt, in diesem Schritt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Schritt 7: Hinzufügen von entsprechenden Methoden auf die Business Logic Layer

Wir der Datenzugriffsebene Einbeziehung von Methoden zum Aufrufen der verwalteten gespeicherten Prozeduren hinzugefügt, die in den Schritte4 und 5 aktualisiert haben, müssen wir die Geschäftslogikschicht entsprechende Methoden hinzufügen. Die folgenden beiden Methoden zum Hinzufügen der `ProductsBLLWithSprocs` Klasse:


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

Beide Methoden einfach die entsprechende DAL-Methode aufrufen und Zurückgeben der `ProductsDataTable` Instanz. Die `DataObjectMethodAttribute` Markup über jede Methode führt dazu, dass diese Methoden, die in der Dropdown-Liste in der Registerkarte "auswählen" das ObjectDataSource s Konfigurieren von Datenquellen-Assistenten eingeschlossen werden.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Schritt 8: Aufrufen der verwalteten gespeicherten Prozeduren aus der Darstellungsschicht

Mit dem Geschäftslogik und die Datenzugriffsebene ergänzt, um Unterstützung für das Aufrufen der `GetDiscontinuedProducts` und `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozeduren können wir jetzt diese anzeigen gespeicherten Prozeduren Ergebnisse über eine ASP.NET-Seite.

Öffnen der `ManagedFunctionsAndSprocs.aspx` auf der Seite der `AdvancedDAL` Ordner, und ziehen Sie aus der Toolbox eine GridView in den Designer. Legen Sie die GridView s `ID` Eigenschaft `DiscontinuedProducts` und aus seinem Smarttag binden Sie es an eine neue ObjectDataSource mit dem Namen `DiscontinuedProductsDataSource`. Konfigurieren der ObjectDataSource zum Abrufen der zugehörigen Daten aus der `ProductsBLLWithSprocs` Klasse s `GetDiscontinuedProducts` Methode.


[![Konfigurieren der ObjectDataSource zur Verwendung der ProductsBLLWithSprocs-Klasse](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**Abbildung 20**: Konfigurieren der ObjectDataSource verwenden die `ProductsBLLWithSprocs` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![Wählen Sie die GetDiscontinuedProducts-Methode aus der Dropdown-Liste auf der Registerkarte "SELECT"](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**Abbildung 21**: Wählen Sie die `GetDiscontinuedProducts` Methode aus der Dropdown-Liste auf der Registerkarte "auswählen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


Da in diesem Raster nur die Produktinformationen anzeigen verwendet wird, legen Sie die Dropdownlisten in der Update-, INSERT- und Löschen von Registerkarten (keine), und klicken Sie dann auf Fertig stellen.

Nach Abschluss des Assistenten für Visual Studio wird automatisch eine BoundField- oder hinzufügen CheckBoxField für jedes Datenfeld in der `ProductsDataTable`. So entfernen Sie alle diese Felder mit Ausnahme von erkundet `ProductName` und `Discontinued`, zeigen Sie an dem die GridView und ObjectDataSource s deklaratives Markup sollte etwa wie folgt aussehen:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser anzuzeigen. Wenn die Seite zugegriffen wird, das ObjectDataSource-Aufrufe der `ProductsBLLWithSprocs` Klasse s `GetDiscontinuedProducts` Methode. Wie in Schritt 7 wurde erläutert, ruft diese Methode nach unten der DAL-s `ProductsDataTable` Klasse s `GetDiscontinuedProducts` -Methode, die Ruft die `GetDiscontinuedProducts` gespeicherte Prozedur. Diese gespeicherte Prozedur ist eine verwaltete gespeicherte Prozedur und führt den Code, das in Schritt 3, die nicht mehr unterstützte Produkte zurückgeben erstellt wurde.

Die von der verwalteten gespeicherten Prozedur zurückgegebenen Ergebnisse sind verpackt in einer `ProductsDataTable` von der DAL, und klicken Sie dann die BLL, das dann diese an die Darstellungsschicht zurückgibt, in dem sie an die GridView gebunden und angezeigt werden, zurückgegeben. Wie erwartet, listet das Raster diese Produkte, die eingestellt wurden.


[![Die nicht mehr unterstützte Produkte aufgeführt sind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**Abbildung 22**: die nicht mehr unterstützte Produkte aufgeführt sind ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


Fügen Sie zur weiteren wird empfohlen ein Textfeld und einer anderen GridView auf der Seite "ein. Haben Sie diesem GridView zeigen die Produkte, die kleiner als die Menge, die in das Textfeld eingegeben werden, durch Aufrufen der `ProductsBLLWithSprocs` Klasse s `GetProductsWithPriceLessThan` Methode.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Schritt 9: Erstellen und Aufrufen von T-SQL-UDFs

Benutzerdefinierte Funktionen oder benutzerdefinierten Funktionen, sind eng verhielte die Semantik der Funktionen in Programmiersprachen zu vergleichen Datenbankobjekte. Wie eine Funktion in c# benutzerdefinierten Funktionen umfassen eine Variable Anzahl von Eingabeparametern und den Wert eines bestimmten Typs zurück. Eine benutzerdefinierte Funktion kann entweder skalaren Daten – eine Zeichenfolge, eine ganze Zahl, und die usw.- oder die Tabellendaten zurückgeben. Lassen Sie s, nehmen einen kurzen Blick auf beide Arten von UDFs, beginnend mit einer benutzerdefinierten Funktion, die einen skalaren Datentyp zurückgibt.

Die folgende benutzerdefinierte Funktion berechnet den geschätzten Wert der Bestand für ein bestimmtes Produkt. Dies erfolgt durch Aufnahme in drei Eingabeparameter - die `UnitPrice`, `UnitsInStock`, und `Discontinued` Werte für ein bestimmtes Produkt - und gibt einen Wert vom Typ `money`. Berechnet den geschätzten Wert für die Inventur durch Multiplikation der `UnitPrice` durch die `UnitsInStock`. Für nicht mehr unterstützte Elemente wird dieser Wert halbiert.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

Sobald diese benutzerdefinierte Funktion mit der Datenbank hinzugefügt wurde, kann er über Management Studio gefunden werden, durch die Programmierbarkeit-Ordner, und klicken Sie dann und dann Skalarwert-Funktionen erweitern. Es kann verwendet werden, einem `SELECT` Abfrage wie folgt:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

Ich hinzugefügt haben die `udf_ComputeInventoryValue` UDF mit der Datenbank Northwind; Abbildung 23 zeigt die Ausgabe der oben genannten `SELECT` abzufragen, wenn über Management Studio angezeigt. Beachten Sie außerdem, dass die UDF im Objekt-Explorer im Ordner Skalarwert-Funktionen aufgeführt ist.


[![Jedes Produkt s Inventur Werte wird aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**Abbildung 23**: jedes Produkt s Inventur Werte enthält ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Benutzerdefinierte Funktionen können auch Tabellendaten zurückgeben. Beispielsweise können wir eine benutzerdefinierte Funktion erstellen, die Produkte zurückgibt, die zu einer bestimmten Kategorie gehören:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

Die `udf_GetProductsByCategoryID` UDF akzeptiert eine `@CategoryID` Eingabeparameter und gibt die Ergebnisse des angegebenen `SELECT` Abfrage. Nach der Erstellung kann diese benutzerdefinierte Funktion verwiesen werden, der `FROM` (oder `JOIN`)-Klausel einer `SELECT` Abfrage. Im folgende Beispiel würde Zurückgeben der `ProductID`, `ProductName`, und `CategoryID` Werte für jede von Getränken.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

Ich hinzugefügt haben die `udf_GetProductsByCategoryID` UDF mit der Datenbank Northwind; Abbildung 24 zeigt die Ausgabe der oben genannten `SELECT` abzufragen, wenn über Management Studio angezeigt. Benutzerdefinierte Funktionen, die Tabellendaten zurückgeben können im Objekt-Explorer s Tabellenwertfunktionen Ordner gefunden werden.


[![Die ProductID, ProductName und CategoryID sind für jede Getränke aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**Abbildung 24**: die `ProductID`, `ProductName`, und `CategoryID` sind für jede Getränke aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> Weitere Informationen zum Erstellen und Verwenden von benutzerdefinierten Funktionen Auschecken [Einführung in benutzerdefinierte Funktionen](http://www.sqlteam.com/item.asp?ItemID=1955). Außerdem sehen Sie sich [vor- und Drawbacks of User-Defined Funktionen](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Schritt 10: Erstellen einer verwalteten UDFs

Die `udf_ComputeInventoryValue` und `udf_GetProductsByCategoryID` UDFs erstellt in den obigen Beispielen sind T-SQL-Datenbankobjekte. SQL Server 2005 unterstützt auch die verwalteten UDFs, die hinzugefügt werden können die `ManagedDatabaseConstructs` Projekt so, wie die verwaltete Prozeduren aus Schritt 3 und 5 gespeicherte. Für diesen Schritt können Sie s implementieren die `udf_ComputeInventoryValue` UDF in verwaltetem Code.

Eine verwaltete UDF zum Hinzufügen der `ManagedDatabaseConstructs` -Projekts mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie ein neues Element hinzufügen. Wählen Sie die benutzerdefinierte Vorlage aus dem Dialogfeld Neues Element hinzufügen, und nennen Sie die neue UDF-Datei `udf_ComputeInventoryValue_Managed.cs`.


[![Fügen Sie eine neue verwaltete UDF ManagedDatabaseConstructs-Projekt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**Abbildung 25**: Hinzufügen einer neuen verwaltet UDF auf die `ManagedDatabaseConstructs` Projekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


Die Vorlage eine benutzerdefinierte Funktion erstellt eine `partial` Klasse mit dem Namen `UserDefinedFunctions` mit einer Methode, deren Name identisch mit der Klassenname für die Datei "s ist" (`udf_ComputeInventoryValue_Managed`, in dieser Instanz). Diese Methode ergänzt wird, mithilfe der [ `SqlFunction` Attribut](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), die kennzeichnet, dass der Methode als eine verwaltete UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

Die `udf_ComputeInventoryValue` Methodenrückgabe derzeit eine [ `SqlString` Objekt](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) und keine Eingabeparameter akzeptiert. Wir müssen die Definition der Methode aktualisieren, sodass es nimmt drei Eingabeparameter - `UnitPrice`, `UnitsInStock`, und `Discontinued` - und gibt ein `SqlMoney` Objekt. Die Logik zum Berechnen des Inventur-Werts ist identisch mit dem in der T-SQL `udf_ComputeInventoryValue` UDF.


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

Beachten Sie, dass die Eingabeparameter der UDF-s-Methode für die entsprechenden SQL-Datentypen: `SqlMoney` für die `UnitPrice` Feld [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) für `UnitsInStock`, und [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) für `Discontinued`. Diese Datentypen entsprechen, die in definierten Typen der `Products` Tabelle: der `UnitPrice` Spalte ist vom Typ `money`, `UnitsInStock` Spalte vom Typ `smallint`, und die `Discontinued` Spalte vom Typ `bit`.

Der Code beginnt mit dem Erstellen einer `SqlMoney` Instanz mit dem Namen `inventoryValue` , die einen Wert von 0 zugewiesen. Die `Products` Tabelle zulässig, für die Datenbank `NULL` Werte in der `UnitsInPrice` und `UnitsInStock` Spalten. Daher wir zuerst prüfen um festzustellen, ob diese Werte enthalten müssen `NULL` s, die wir, über tun die `SqlMoney` s-Objekt [ `IsNull` Eigenschaft](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Wenn beide `UnitPrice` und `UnitsInStock` enthalten nicht -`NULL` Werte, und klicken Sie dann wir berechnen die `inventoryValue` das Produkt der beiden sein. Wenn danach `Discontinued` ist "true", und klicken Sie dann wir den Wert halbiert.

> [!NOTE]
> Die `SqlMoney` Objekt kann nur die zwei `SqlMoney` Instanzen miteinander multipliziert werden soll. Es ist nicht gestattet eine `SqlMoney` Instanz durch ein literal Gleitkommazahl multipliziert werden soll. Aus diesem Grund zum halbiert `inventoryValue` wir Multiplizieren Sie es mit einer neuen `SqlMoney` Instanz, die den Wert 0,5 aufweist.


## <a name="step-11-deploying-the-managed-udf"></a>Schritt 11: Bereitstellen von verwalteten UDF

Nachdem die verwaltete benutzerdefinierte Funktion erstellt wurde, können wir für die Northwind-Datenbank bereitstellen. Wie in Schritt 4 wurde erläutert, werden die verwalteten Objekte in einer SQL Server-Projekt bereitgestellt, indem Sie mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer und im Kontextmenü die Option bereitstellen auswählen.

Nachdem Sie das Projekt bereitgestellt haben, zurück zu SQL Server Management Studio, und aktualisieren Sie den Ordner Skalarwertfunktionen. Nun sollten zwei Einträge angezeigt werden:

- `dbo.udf_ComputeInventoryValue` -die T-SQL-UDF erstellt in Schritt 9 und
- `dbo.udf ComputeInventoryValue_Managed` -die verwaltete benutzerdefinierte Funktion erstellt in Schritt 10, die gerade bereitgestellt wurde.

Um diese verwaltete benutzerdefinierte Funktion zu testen, führen Sie die folgende Abfrage aus in Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

Dieser Befehl verwendet das verwaltete `udf ComputeInventoryValue_Managed` UDF anstelle der T-SQL `udf_ComputeInventoryValue` UDF, aber die Ausgabe ist identisch. Finden Sie einen Screenshot der UDF-s-Ausgabe finden Sie unter Abbildung 23 zurück.

## <a name="step-12-debugging-the-managed-database-objects"></a>Schritt 12: Debuggen verwalteter Datenbankobjekte

In der [Debuggen von gespeicherten Prozeduren](debugging-stored-procedures-cs.md) Lernprogramm wir besprochen haben drei Optionen für das Debuggen von SQL Server über Visual Studio: direkten Datenbankdebuggen, Anwendungsdebugging und Debuggen von einem SQL Server-Projekt. Verwaltete Datenbank Objekte können nicht debuggt werden, über direkten Datenbankdebuggen, jedoch können gedebuggt werden, von einer Clientanwendung und direkt aus dem SQL Server-Projekt. In der Reihenfolge für das Debuggen funktioniert muss jedoch die SQL Server 2005-Datenbank SQL/CLR-Debuggen zulassen. Bedenken Sie, dass beim Erstellen wir zunächst die `ManagedDatabaseConstructs` Projekt Visual Studio haben uns gebeten, ob wir SQL/CLR-Debuggen (siehe Abbildung 6 in Schritt2) aktivieren möchten. Diese Einstellung kann geändert werden, indem Sie mit der rechten Maustaste auf die Datenbank aus dem Server-Explorer-Fenster.


![Stellen Sie sicher, dass die Datenbank SQL/CLR-Debuggen kann](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**Abbildung 26**: Stellen Sie sicher, dass die Datenbank SQL/CLR-Debuggen kann


Angenommen, zum Debuggen der `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur. Wir würden starten, indem Sie einen Haltepunkt im Code von der `GetProductsWithPriceLessThan` Methode.


[![Legen Sie einen Haltepunkt in der GetProductsWithPriceLessThan-Methode](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**Abbildung 27**: Festlegen eines Haltepunkts in der `GetProductsWithPriceLessThan` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


Lassen Sie uns zunächst an die verwalteten Datenbankobjekte aus dem SQL Server-Projekt Debuggen s an. Da unsere Projektmappe zwei Projekte - enthält die `ManagedDatabaseConstructs` SQL Server-Projekt zusammen mit unserer Website - um aus dem SQL Server-Projekt Debuggen Wir weisen Sie Visual Studio zum Starten müssen der `ManagedDatabaseConstructs` SQL Server-Projekt, wenn wir das Debuggen starten. Mit der rechten Maustaste die `ManagedDatabaseConstructs` Projekt im Projektmappen-Explorer, und wählen Sie die Gruppe als Startprojekt-Option im Kontextmenü.

Wenn die `ManagedDatabaseConstructs` Projekt aus dem er die SQL-Anweisungen im führt Debugger gestartet wird die `Test.sql` Datei befindet sich im die `Test Scripts` Ordner. Z. B. zum Testen der `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur ersetzen Sie die vorhandene `Test.sql` Datei Inhalt mit der folgenden Anweisung ruft die `GetProductsWithPriceLessThan` verwaltete gespeicherte Prozedur übergeben der `@CategoryID` Wert 14,95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

Sobald Sie Ve des obigen Skripts in eingegeben `Test.sql`, starten Sie das Debuggen, indem Sie dem Debugmenü den Ausnahmebefehl aufrufen und auswählen, Debugging starten, oder drücken Sie F5, oder das grüne Symbol "wiedergeben" in der Symbolleiste. Dies wird Projekte innerhalb der Projektmappe zu erstellen, Bereitstellen von verwalteten Datenbankobjekte zur Northwind-Datenbank und führen Sie dann die `Test.sql` Skript. An diesem Punkt wird der Haltepunkt erreicht, und wir können schrittweise die `GetProductsWithPriceLessThan` -Methode, die Werte der Eingabeparameter prüfen und so weiter.


[![Der Breakpoint in der Methode GetProductsWithPriceLessThan wurde erreicht.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**Abbildung 28**: der Haltepunkt in der `GetProductsWithPriceLessThan` -Methode wurde erreicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


Damit ein SQL-Datenbankobjekt, das über eine Clientanwendung gedebuggt werden kann ist es unabdinglich, die zum Unterstützen von Anwendungsdebuggen konfiguriert werden. Mit der rechten Maustaste auf die Datenbank im Server-Explorer, und stellen Sie sicher, dass die Option Debuggen der Anwendung aktiviert ist. Darüber hinaus müssen es zum Konfigurieren der ASP.NET-Anwendung für die Integration der SQL-Debugger und Verbindungspooling zu deaktivieren. Diese Schritte in Schritt2 des im Detail besprochen wurden die [Debuggen von gespeicherten Prozeduren](debugging-stored-procedures-cs.md) Lernprogramm.

Nachdem Sie die ASP.NET-Anwendung und Datenbank konfiguriert haben, ASP.NET-Website als Startprojekt festgelegt, und mit dem Debuggen beginnen. Wenn Sie eine Website, die eine der verwalteten Objekte, die ein Haltepunkt verfügt über aufruft besuchen, die Anwendung wird angehalten, und Steuerelement wird aktiviert werden über, die im Debugger, können Sie den Code schrittweise durchlaufen wie in Abbildung 28 dargestellt.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Schritt 13: Manuell verwalteten kompilieren und Bereitstellen von Datenbankobjekten

SQL Server-Projekte erleichtern das Erstellen, kompilieren und Bereitstellen von verwalteten Datenbankobjekten. Leider sind SQL Server-Projekte nur in den Editionen Professional und Team-Systeme von Visual Studio verfügbar. Wenn Sie Visual Web Developer oder der Standard Edition von Visual Studio verwenden und verwalteten Datenbankobjekte verwenden möchten, müssen Sie manuell erstellen und bereitstellen. Dies umfasst vier Schritte:

1. Erstellen Sie eine Datei, die den Quellcode für das verwaltete Datenbankobjekt enthält,
2. Das Objekt in eine Assembly kompilieren
3. Registrieren Sie die Assembly mit der SQL Server 2005-Datenbank, und
4. Erstellen Sie ein Datenbankobjekt in SQL Server, die auf die entsprechende Methode in der Assembly verweist.

Um diese Aufgaben zu veranschaulichen, lassen Sie die s-Erstellen eines neuen verwalteten gespeicherten Prozedur, die Produkte zurückgibt, deren `UnitPrice` größer als ein angegebener Wert ist. Erstellen Sie eine neue Datei auf Ihrem Computer mit dem Namen `GetProductsWithPriceGreaterThan.cs` , und geben Sie den folgenden Code in die Datei (Sie können Visual Studio, Editor oder einem beliebigen Texteditor, um dies zu erreichen):


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

Dieser Code ist fast identisch mit der `GetProductsWithPriceLessThan` Methode, die in Schritt 5 erstellt haben. Die einzigen Unterschiede werden dem Methodennamen, der `WHERE` -Klausel und der Name des Parameters in der Abfrage verwendet. In der `GetProductsWithPriceLessThan` -Methode, die `WHERE` -Klausel lesen: `WHERE UnitPrice < @MaxPrice`. Hier im `GetProductsWithPriceGreaterThan`, wir verwenden: `WHERE UnitPrice > @MinPrice` .

Jetzt müssen wir diese Klasse in eine Assembly kompilieren. Navigieren Sie zu dem Verzeichnis, in dem Sie gespeichert, über die Befehlszeile der `GetProductsWithPriceGreaterThan.cs` Datei und verwenden den C#-Compiler (`csc.exe`) Klassendatei in eine Assembly kompiliert:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

Wenn der Ordner mit `csc.exe` in nicht im System s `PATH`, müssen Sie vollständig des Pfads, verweisen `%WINDOWS%\Microsoft.NET\Framework\version\`, wie folgt:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![Kompilieren Sie GetProductsWithPriceGreaterThan.cs in eine Assembly.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**Abbildung 29**: Kompilieren `GetProductsWithPriceGreaterThan.cs` in einer Assembly ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


Die `/t` -Flag gibt an, dass die C#-Klassendatei in eine DLL (statt eine ausführbare Datei) kompiliert werden soll. Die `/out` -Flag gibt an, den Namen der resultierenden Assembly.

> [!NOTE]
> Anstatt zu kompilieren der `GetProductsWithPriceGreaterThan.cs` Klassendatei über die Befehlszeile Alternativ Sie können [Visual c# Express Edition](https://msdn.microsoft.com/vstudio/express/visualcsharp/) , oder erstellen Sie ein separates Klassenbibliotheksprojekt, in Visual Studio Standard Edition. S Ren Jacob Lauritsen bereitgestellt hat überprüfen solche einem Visual c# Express Edition-Projekt mit Code für die `GetProductsWithPriceGreaterThan` gespeicherte Prozedur und die beiden verwalteten gespeicherte Prozeduren und benutzerdefinierte Funktion, die in Schritt 3, 5 und 10 erstellt. S Ren s Projekt umfasst auch die T-SQL-Befehle, die erforderlich sind, um die entsprechende Datenbankobjekte hinzuzufügen.


Durch den Code in eine Assembly kompiliert wird können wir die Assembly in der SQL Server 2005-Datenbank zu registrieren. Dies erfolgt mithilfe von T-SQL, mit dem Befehl `CREATE ASSEMBLY`, oder über SQL Server Management Studio. Lassen Sie s den Fokus auf mithilfe von Management Studio.

Erweitern Sie in Management Studio im Ordner Programmierbarkeit der Northwind-Datenbank aus. Keines der Unterordner handelt es sich um Assemblys. Um die Datenbank manuell eine neue Assembly hinzuzufügen, mit der rechten Maustaste auf den Ordner Assemblys, und wählen Sie die neue Assembly aus dem Kontextmenü aus. Dieser enthält die neue Assembly Dialogfeld (siehe Abbildung 30). Klicken Sie auf die Schaltfläche zum Durchsuchen, wählen die `ManuallyCreatedDBObjects.dll` Assembly wir gerade kompiliert, und klicken Sie dann auf OK, um die Assembly mit der Datenbank hinzufügen. Sie sollte nicht die `ManuallyCreatedDBObjects.dll` Assembly im Objekt-Explorer.


[![Fügen Sie die ManuallyCreatedDBObjects.dll-Assembly in der Datenbank](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**Abbildung 30**: Hinzufügen der `ManuallyCreatedDBObjects.dll` -Assembly in der Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![Die ManuallyCreatedDBObjects.dll wird im Objekt-Explorer aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**Abbildung 31**: die `ManuallyCreatedDBObjects.dll` wird im Objekt-Explorer aufgeführt


Während es die Assembly mit der Datenbank Northwind hinzugefügt haben, müssen wir noch eine gespeicherte Prozedur mit Zuordnen der `GetProductsWithPriceGreaterThan` Methode in der Assembly. Um dies zu erreichen, öffnen Sie ein neues Abfragefenster, und führen Sie das folgende Skript:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

Dadurch wird eine neue gespeicherte Prozedur erstellt, in der Northwind-Datenbank mit dem Namen `GetProductsWithPriceGreaterThan` und ordnet die verwaltete Methode `GetProductsWithPriceGreaterThan` (Dies ist in der Klasse `StoredProcedures`, die in der Assembly ist `ManuallyCreatedDBObjects`).

Nach dem Ausführen des obigen Skripts, aktualisieren Sie den Ordner gespeicherte Prozeduren im Objekt-Explorer aus. Daraufhin sollte einen neuer Eintrag der gespeicherten Prozedur - `GetProductsWithPriceGreaterThan` -die über ein Schlosssymbol daneben verfügt. Um diese gespeicherte Prozedur testen möchten, geben Sie ein, und führen Sie das folgende Skript in das Abfragefenster ein:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

Wie in Abbildung 32 dargestellt, handelt es sich bei der oben genannten Befehl zeigt Informationen für diese Produkte mit einem `UnitPrice` größer als $24,95.


[![Die ManuallyCreatedDBObjects.dll wird im Objekt-Explorer aufgeführt.](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**Abbildung 32**: die `ManuallyCreatedDBObjects.dll` wird im Objekt-Explorer aufgeführt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>Zusammenfassung

Microsoft SQL Server 2005 bietet Integration mit der Common Language Runtime (CLR), dem Datenbankobjekte mithilfe von verwaltetem Code erstellt werden können. Zuvor diese Datenbankobjekte konnte nur mit T-SQL erstellt werden, aber nun können wir diese Objekte, die mit .NET-Programmiersprachen wie c# erstellen. In diesem Lernprogramm erstellten verwaltet zwei gespeicherte Prozeduren und eine verwaltete benutzerdefinierte Funktion.

Visual Studio s SQL Server-Projekttyp erleichtert das Erstellen, kompilieren und Bereitstellen von verwalteten Datenbankobjekten. Darüber hinaus bietet sie umfassende Debugunterstützung. SQL Server-Projekttypen sind jedoch nur in den Editionen Professional und Team-Systeme von Visual Studio verfügbar. Für diese muss mithilfe von Visual Web Developer oder der Standard Edition von Visual Studio, Erstellung, Kompilierung und Bereitstellungsschritte manuell ausgeführt werden wie wir im Schritt 13 gesehen haben.

Viel Spaß beim Programmieren!

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Vor- und Nachteile von benutzerdefinierten Funktionen](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Erstellen von SQL Server 2005-Objekte in verwaltetem Code](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Erstellen von Triggern, die mithilfe von verwaltetem Code in SQLServer 2005](http://www.15seconds.com/issue/041006.htm)
- [Gewusst wie: Erstellen und führen Sie eine CLR-SQL Server gespeicherte Prozedur](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Gewusst wie: Erstellen und Ausführen eine CLR-SQL Server eine benutzerdefinierte Funktion](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Gewusst wie: Bearbeiten der `Test.sql` Skript zum Ausführen von SQL-Objekte](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Einführung in Benutzer, benutzerdefinierte Funktionen](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Verwalteter Code und SQLServer 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Transact-SQL-Referenz](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [Exemplarische Vorgehensweise: Erstellen einer gespeicherten Prozedur in verwaltetem Code](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde S Ren Jacob Lauritsen. Neben den diesem Artikel, S Ren auch erstellt, das in diesem Artikel s-Download für die verwalteten Datenbankobjekte manuell kompilieren enthaltenen Visual c# Express Edition-Projekt. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](debugging-stored-procedures-cs.md)
> [Weiter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
