---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Debuggen von gespeicherten Prozeduren (c#) | Microsoft Docs
author: rick-anderson
description: "Editionen von Visual Studio Professional und Team System ermöglichen es Ihnen, Haltepunkte setzen und Schritt gespeicherten Prozeduren in SQL Server, wodurch das Debuggen von gespeicherten..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: eda544d72fe3449c8d701fc579f2f26d37090f24
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="debugging-stored-procedures-c"></a>Debuggen von gespeicherten Prozeduren (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) oder [PDF herunterladen](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Visual Studio Professional und Team System Edition können Sie Haltepunkte setzen und Schritt gespeicherten Prozeduren in SQL Server, Debuggen gespeicherter Prozeduren genauso einfach wie das Debuggen von Anwendungscode vornehmen. Dieses Lernprogramm veranschaulicht direkten Datenbankdebuggen und die Anwendungsdebuggen von gespeicherten Prozeduren.


## <a name="introduction"></a>Einführung

Visual Studio bietet eine umfassende Debugleistung. Mit wenigen Tastaturanschlägen oder Mausklicks es s Haltepunkte zum Beenden der Ausführung eines Programms, und untersuchen und Fluss verwendet. Zusammen mit Debuggen Anwendungscode, bietet Visual Studio unterstützt das Debuggen von gespeicherter Prozeduren von SQL Server. Wie alle Haltepunkte im Code eines ASP.NET Code-Behind-Klasse oder Business Logic Layer-Klasse festgelegt werden, können damit zu sie platziert werden innerhalb von gespeicherten Prozeduren.

In diesem Lernprogramm betrachten wir schrittweise Ausführung von gespeicherten Prozeduren im Server-Explorer in Visual Studio ebenfalls wie Festlegen von Haltepunkten, die erreicht werden, wenn die gespeicherte Prozedur von der ausgeführten ASP.NET-Anwendung aufgerufen wird.

> [!NOTE]
> Leider können gespeicherte Prozeduren nur werden in Einzelschritten und über die Professional und Team-Systeme Versionen von Visual Studio debuggen. Wenn Sie Visual Web Developer oder die standard-Version von Visual Studio verwenden, können Sie Willkommen entlang zu lesen, wie wir die erforderlichen Schritte zum Debuggen von gespeicherter Prozeduren durchlaufen verwendet, aber Sie werden nicht in der Lage, diese Schritte auf dem Computer zu replizieren.


## <a name="sql-server-debugging-concepts"></a>Konzepte von SQL Server-Debuggen

Microsoft SQL Server 2005 wurde entworfen, um die Integration mit Bereitstellen der [Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx), also die Laufzeit verwendet, die von allen .NET Assemblys. Daher unterstützt SQL Server 2005 verwalteten Datenbankobjekte. Sie können also Datenbankobjekte wie gespeicherte Prozeduren und benutzerdefinierte Funktionen (UDFs) als Methoden in einer C#-Klasse erstellen. Dadurch werden diese gespeicherten Prozeduren und benutzerdefinierten Funktionen aus, die die Funktionalität in .NET Framework und eigene benutzerdefinierte Klassen. Natürlich müssen bietet SQL Server 2005 auch Unterstützung für T-SQL-Datenbankobjekte.

SQL Server 2005 bietet Unterstützung für T-SQL und verwalteter Datenbankobjekte debugging. Diese Objekte können jedoch nur über Visual Studio 2005 Professional und Systeme Team Edition gedebuggt werden. In diesem Lernprogramm werden Debuggen T-SQL-Datenbankobjekte untersucht. Prüft, dass das nachfolgende Lernprogramm Debuggen verwalteter Datenbankobjekte.

Die [Übersicht über die von T-SQL und CLR-Debuggen in SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) Blogeintrag aus der [CLR-Integration von SQL Server 2005-Team](https://blogs.msdn.com/sqlclr/default.aspx) werden die drei Möglichkeiten zum Debuggen von SQL Server 2005-Objekte aus Visual Studio hervorgehoben:

- **Leiten Sie Datenbank-Debuggen (DDD)** – klicken Sie in Server-Explorer, die wir einen in eine beliebige T-SQL-Datenbankobjekt, z. B. gespeicherte Prozeduren und benutzerdefinierte Funktionen Einzelschritt. Untersuchen wir DDD in Schritt 1.
- **Das Debuggen der Anwendung** -wir können Haltepunkte innerhalb eines Datenbankobjekts und führen Sie anschließend unsere ASP.NET-Anwendung. Wenn das Datenbankobjekt ausgeführt wird, wird der Haltepunkt erreicht und Steuerelement Supportgruppen für den Debugger. Beachten Sie, dass beim Anwendungsdebuggen wird in ein Datenbankobjekt aus Anwendungscode wechseln können. Wir müssen explizit legen Sie Haltepunkte in diese gespeicherten Prozeduren oder benutzerdefinierten Funktionen, möchten wir den Debugger zu beenden. Anwendungsdebuggen ab, die in Schritt2 untersucht.
- **Aus einem SQL Server-Projekt Debuggen** -Editionen von Visual Studio Professional und Team-Systeme sind einen SQL Server-Projekt-Typ, der häufig verwendet wird, um Datenbankobjekte zu erstellen. Wir untersuchen mithilfe von SQL Server-Projekte und ihre Inhalte in den nächsten Lernprogrammen Debuggen.

Visual Studio debuggen kann gespeicherte Prozeduren auf lokalen und remote-SQL Server-Instanzen. Eine lokale SQL Server-Instanz ist eine, die auf demselben Computer wie Visual Studio installiert ist. Wenn Ihnen verwendeten SQL Server-Datenbank nicht auf dem Entwicklungscomputer befindet, wird es eine Remoteinstanz angesehen. Für diesen Lernprogrammen haben wir lokale SQL Server-Instanzen verwendet. Debuggen gespeicherter Prozeduren auf einer Remoteinstanz des SQL Server erfordert mehrere Konfigurationsschritte als beim Debuggen von gespeicherten Prozeduren auf einer lokalen Instanz.

Wenn Sie eine lokale SQL Server-Instanz verwenden, können Sie mit Schritt 1 beginnen und Durcharbeiten dieses Lernprogramms bis zum Ende. Bei Verwendung eine Remoteinstanz von SQL Server werden jedoch, Sie müssen zunächst, um sicherzustellen, dass während des Debuggens werden protokolliert, auf dem Entwicklungscomputer mit einem Windows-Benutzerkonto, das SQL Server-Anmeldung auf der Remoteinstanz verfügt. Moveover, diese datenbankanmeldung und die Datenbank-Anmeldenamen, die von der ausgeführten ASP.NET-Anwendung eine Verbindung mit der Datenbank verwendet, muss Mitglied der `sysadmin` Rolle. Finden Sie das Debuggen von T-SQL-Datenbankobjekte auf Remoteinstanzen im Abschnitt am Ende dieses Lernprogramms für Weitere Informationen zum Konfigurieren von Visual Studio und SQL Server, um eine Remoteinstanz zu debuggen.

Schließlich verstehen Sie, dass die debugging-Unterstützung für T-SQL-Datenbankobjekte nicht als Funktion als debugging-Unterstützung für .NET-Anwendungen umfangreiche ist. Beispielsweise breakpointbedingungen und Filter werden nicht unterstützt, nur ein Teil der Debugfenster verfügbar sind, können keine bearbeiten und fortfahren, das "Direktfenster" nutzlos usw. gerendert wird. Finden Sie unter [Einschränkungen Debuggerbefehle und den Funktionen](https://msdn.microsoft.com/en-us/library/ms165035(VS.80).aspx) für Weitere Informationen.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Schritt 1: Direkt einem Einzelschritt, in einer gespeicherten Prozedur

Visual Studio erleichtert das direkt Debuggen eines Datenbankobjekts. Können s betrachten, wie die direkte Datenbank Debuggen (DDD)-Funktion verwenden, um einen Einzelschritt in die `Products_SelectByCategoryID` gespeicherte Prozedur in der Northwind-Datenbank. Wie der Name schon sagt, `Products_SelectByCategoryID` gibt Produktinformationen für eine bestimmte Kategorie; es erstellt wurde, der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Lernprogramm. Navigieren Sie zu dem Server-Explorer starten Sie, und erweitern Sie den Knoten der Northwind-Datenbank. Als Nächstes Drilldown in den Ordner gespeicherte Prozeduren, mit der rechten Maustaste auf die `Products_SelectByCategoryID` gespeicherte Prozedur, und wählen Sie die Option Schritt in gespeicherte Prozedur aus dem Kontextmenü. Dadurch wird den Debugger gestartet.

Da die `Products_SelectByCategoryID` gespeicherte Prozedur erwartet eine `@CategoryID` Eingabeparameter, wir werden aufgefordert, diesen Wert anzugeben. Geben Sie 1, die Informationen zu den Getränke zurückgegeben wird.


![Verwenden Sie den Wert 1 für die @CategoryID Parameter](debugging-stored-procedures-cs/_static/image1.png)

**Abbildung 1**: Verwenden Sie den Wert 1 für die `@CategoryID` Parameter


Nach der Angabe des Werts für die `@CategoryID` Parameter, die gespeicherte Prozedur ausgeführt wird. Anstatt bis zum Abschluss ausgeführt, jedoch hält der Debugger die Ausführung bei der ersten Anweisung. Beachten Sie, den gelben Pfeil auf den Rand, der angibt, der aktuellen Position in der gespeicherten Prozedur. Sie können anzeigen und Bearbeiten von Parameterwerten, die über das Fenster "überwachen" oder der Name des Parameters in der gespeicherten Prozedur mit der Maus.


[![Der Debugger wurde auf die erste Anweisung der gespeicherten Prozedur angehalten.](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Abbildung 2**: der Debugger wurde angehalten, auf die erste Anweisung der gespeicherten Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image4.png))


Um einen über die gespeicherte Prozedur eine Anweisung zu einem Zeitpunkt durchzuführen, klicken Sie auf die Schaltfläche "Prozedurschritt" in der Symbolleiste, oder drücken Sie die F10-TASTE. Die `Products_SelectByCategoryID` gespeicherte Prozedur enthält ein einziges `SELECT` Anweisung, damit für die einzelne Anweisung drücken F10 einen Prozedurschritt wird und schließen Sie die Ausführung der gespeicherten Prozedur. Nachdem die gespeicherte Prozedur abgeschlossen wurde, wird seine Ausgabe im Ausgabefenster angezeigt, und der Debugger wird beendet.

> [!NOTE]
> T-SQL-Debuggen auftritt auf der Anweisungsebene; Sie können nicht in einem Einzelschritt einer `SELECT` Anweisung.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Schritt 2: Konfigurieren der Website für das Debuggen der Anwendung

Beim Debuggen einer gespeicherten Prozedur direkt über den Server-Explorer praktisch ist, sind in vielen Szenarien wir mehr interessiert die gespeicherte Prozedur Debuggen, wenn sie von unserem ASP.NET-Anwendung aufgerufen wird. Wir können Haltepunkte hinzufügen, um eine gespeicherte Prozedur in Visual Studio, und starten Sie die ASP.NET-Anwendung debuggen. Wenn eine gespeicherte Prozedur mit Haltepunkten aus der Anwendung aufgerufen wird, wird die Ausführung am Haltepunkt angehalten, und wir sehen und ändern Sie die Parameterwerte der gespeicherten Prozedur s und die Anweisungen schrittweise, wie in Schritt 1 Wir haben können.

Bevor wir mit dem Debuggen von gespeicherter Prozeduren, die von der Anwendung aufgerufen beginnen können, müssen wir weisen Sie die ASP.NET-Webanwendung für die Integration von SQL Server-Debugger an. Starten, indem Sie mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer (`ASPNET_Data_Tutorial_74_CS`). Wählen Sie die Eigenschaftenseiten-Option im Kontextmenü, wählen Sie das Element "Startoptionen" auf der linken Seite, und aktivieren Sie das SQL Server-Kontrollkästchen im Abschnitt Debugger (siehe Abbildung 3).


[![Aktivieren Sie das SQL Server-Kontrollkästchen in der Anwendung s-Eigenschaftenseiten](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Abbildung 3**: Aktivieren Sie das SQL Server-Kontrollkästchen in der Anwendung-s-Eigenschaftenseiten ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image7.png))


Darüber hinaus müssen Sie aktualisieren die Datenbank-Verbindungszeichenfolge, die von der Anwendung verwendet werden, sodass das Verbindungspooling deaktiviert ist. Wenn eine Verbindung mit einer Datenbank geschlossen wird, den entsprechenden `SqlConnection` Objekt befindet sich in einem Pool verfügbarer Verbindungen. Beim Herstellen einer Verbindungs mit einer Datenbank, einem verfügbaren Verbindungsobjekt aus diesem Pool abgerufen werden können statt erstellen und eine neue Verbindung herstellen müssen. Diese pooling von Verbindungsobjekten ist eine leistungsverbesserung und ist standardmäßig aktiviert. Allerdings beim Debuggen möchten, dass wir deaktivieren Verbindungspooling, da der Debuginfrastruktur nicht ordnungsgemäß wiederhergestellt ist, bei der Arbeit mit einer Verbindung, die aus dem Pool erstellt wurde.

Deaktiviertes Verbindungspooling, aktualisieren Sie die `NORTHWNDConnectionString` in `Web.config` , damit sie die Einstellung enthält `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Wenn Sie fertig sind Debuggen von SQL Server über die ASP.NET-Anwendung unbedingt reinstate Verbindungspooling durch das Entfernen der `Pooling` aus der Verbindungszeichenfolge festlegen (oder durch Festlegen auf `Pooling=true` ).


An diesem Punkt wurde die ASP.NET-Anwendung konfiguriert, um Visual Studio zum Debuggen von SQL Server-Datenbankobjekte, die beim Aufrufen durch die Webanwendung zu ermöglichen. Jetzt bleibt lediglich um eine gespeicherte Prozedur einen Haltepunkt hinzu, und mit dem Debuggen beginnen!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Schritt 3: Hinzufügen eines Haltepunkts und Debuggen

Öffnen der `Products_SelectByCategoryID` gespeicherte Prozedur, und legen Sie einen Haltepunkt am Anfang der `SELECT` Anweisung, indem Sie in den Rand bei der entsprechenden Stelle oder platzieren Sie den Cursor am Anfang der `SELECT` -Anweisung und drücken F9. Abbildung 4 zeigt, wird der Haltepunkt als ein roter Kreis auf den Rand angezeigt.


[![Legen Sie einen Haltepunkt in der Products_SelectByCategoryID gespeicherte Prozedur](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Abbildung 4**: Festlegen eines Haltepunkts in der `Products_SelectByCategoryID` gespeicherte Prozedur ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image10.png))


Damit ein SQL-Datenbankobjekt, das über eine Clientanwendung gedebuggt werden kann ist es unabdinglich, die zum Unterstützen von Anwendungsdebuggen konfiguriert werden. Wenn Sie zuerst einen Haltepunkt festlegen, sollten diese Einstellung automatisch aktiviert werden, aber es ist unerlässlich, doppelklicken Sie auf. Mit der rechten Maustaste auf die `NORTHWND.MDF` Knoten im Server-Explorer. Im Kontextmenü den Befehl sollte eine aktivierte Menüelement Anwendungsdebugging enthalten.


![Stellen Sie sicher, dass die Anwendung debuggen-Option aktiviert ist](debugging-stored-procedures-cs/_static/image11.png)

**Abbildung 5**: Stellen Sie sicher, dass die Anwendung debuggen-Option aktiviert ist


Mit der Haltepunkt festgelegt und die Anwendungsdebuggen-Option aktiviert können die gespeicherte Prozedur, die beim Aufrufen durch die ASP.NET-Anwendung debuggen. Starten Sie den Debugger, navigieren Sie zu dem Debugmenü den Ausnahmebefehl und Symbol auf der Symbolleiste auswählen Debuggen starten, drücken Sie F5, oder indem Sie auf die grüne "wiedergeben". Der Debugger zu starten, wird und starten Sie die Website.

Die `Products_SelectByCategoryID` gespeicherte Prozedur erstellt wurde, der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) Lernprogramm. Die entsprechenden Webseite (`~/AdvancedDAL/ExistingSprocs.aspx`) enthält eine GridView, die von dieser gespeicherten Prozedur zurückgegebenen Ergebnisse werden angezeigt. Besuchen Sie diese Seite über den Browser. Beim Erreichen der Seite ", der den Haltepunkt in der `Products_SelectByCategoryID` gespeicherte Prozedur erreicht, Visual Studio die Kontrolle zurück. Genau wie in Schritt 1, können Sie schrittweise Ausführen der gespeicherten Prozedur s-Anweisungen, und überprüfen und ändern die Parameterwerte.


[![Die Seite "ExistingSprocs.aspx" zeigt zunächst die Getränke](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Abbildung 6**: die `ExistingSprocs.aspx` Seite zeigt zunächst die Getränke ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image14.png))


[![Die gespeicherte Prozedur s Haltepunkt erreicht](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Abbildung 7**: die gespeicherte Prozedur s Haltepunkt erreicht wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image17.png))


Als das Überwachungsfenster in Abbildung 7 zeigt, der Wert, der die `@CategoryID` Parameter ist 1. Grund hierfür ist die `ExistingSprocs.aspx` Seite zeigt anfänglich Produkte in der Kategorie "Getränke", besitzt eine `CategoryID` Wert 1. Wählen Sie eine andere Kategorie aus der Dropdownliste aus. Auf diese Weise einen Postback verursacht und führt erneut die `Products_SelectByCategoryID` gespeicherte Prozedur. Der Haltepunkt erreicht wird, erneut, diesmal jedoch die `@CategoryID` s-Parameterwert wiedergibt der ausgewählten Dropdown-Listenelement s `CategoryID`.


[![Wählen Sie eine andere Kategorie aus der Dropdown Liste](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Abbildung 8**: Wählen Sie eine andere Kategorie aus der Dropdown Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image20.png))


[![Die @CategoryID Parameter gibt die Kategorie auf der Webseite ausgewählt](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Abbildung 9**: die `@CategoryID` Parameter gibt die Kategorie, die von der Webseite ausgewählt ([klicken Sie hier, um das Bild in voller Größe angezeigt](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Wenn der Breakpoint in der `Products_SelectByCategoryID` gespeicherte Prozedur wird nicht erreicht, beim Zugriff auf die `ExistingSprocs.aspx` Seite, stellen Sie sicher, dass das SQL Server-Kontrollkästchen im Abschnitt Debugger der ASP.NET-Anwendung s Eigenschaftenseite geprüft wurde, dass Verbindungspooling wurde deaktiviert, und dass die Datenbank s Anwendungsdebugging-Option aktiviert ist. Wenn Sie immer noch Probleme auftreten, starten Sie Visual Studio und versuchen Sie es erneut.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debuggen von T-SQL-Datenbankobjekte auf Remote-Instanzen

Debuggen von Datenbankobjekten über Visual Studio ist recht einfach, wenn die SQL Server-Datenbankinstanz auf dem gleichen Computer wie Visual Studio ist. Jedoch wenn SQL Server und Visual Studio auf unterschiedlichen Computern befinden wird dann einige eine sorgfältige Konfiguration erforderlich, um alles ordnungsgemäß funktioniert. Es gibt zwei Kernaufgaben, denen es ausgesetzt sind:

- Stellen Sie sicher, dass der Anmeldename zum Verbinden mit der Datenbank über ADO.NET gehört die `sysadmin` Rolle.
- Stellen Sie sicher, dass die Windows-Benutzerkonto, das von Visual Studio auf dem Entwicklungscomputer verwendet eine gültige SQL Server-Anmeldekonto ist, zu der gehört die `sysadmin` Rolle.

Der erste Schritt ist relativ unkompliziert. Zunächst Identifizieren des Benutzerkontos, das zum Verbinden mit der Datenbank von der ASP.NET-Anwendung aus, und fügen Sie dann in SQL Server Management Studio, Anmeldekonto an, die `sysadmin` Rolle.

Die zweite Aufgabe erfordert, dass die Windows-Benutzerkonto, mit dem Debuggen der Anwendung, eine gültige Anmeldung auf die Remotedatenbank. Allerdings sind die Chancen, dass die Windows-Konto, das auf Ihre Arbeitsstation mit dem Sie angemeldet, auf, eine gültige Anmeldung auf SQL Server ist. Statt SQL Server die bestimmten Anmeldekonto hinzugefügt, wäre eine bessere Wahl, einige Windows-Benutzerkonto als das debugging SQL Server-Dienstkonto festlegen. Um die Datenbankobjekte von einer SQL Server-Remoteinstanz zu debuggen, würden Sie dann Visual Studio unter Verwendung dieser Windows-Konto s Anmeldeinformationen ausführen.

Ein Beispiel sollte Dinge verdeutlichen. Angenommen, es ein Windows-Konto mit dem Namen ist `SQLDebug` innerhalb der Windows-Domäne. Dieses Konto auf die SQL Server-Remoteinstanz als eine gültige Anmeldung und als Mitglied hinzugefügt werden müssten die `sysadmin` Rolle. Klicken Sie dann müssten wir um remote SQL Server-Instanz von Visual Studio zu debuggen, Ausführen von Visual Studio als die `SQLDebug` Benutzer. Dies könnte durch Protokollierung aus unserem Arbeitsstation wieder anmelden, als `SQLDebug`, und starten Sie dann Visual Studio, jedoch ein einfacherer Ansatz wäre, sich unsere Arbeitsstation mit eigenen Anmeldeinformationen anmelden, und verwenden Sie dann `runas.exe` zum Starten von Visual Studio als die `SQLDebug` Benutzer. `runas.exe`ermöglicht es eine bestimmte Anwendung unter dem Deckmantel von einem anderen Benutzerkonto ausgeführt werden. Zum Starten von Visual Studio als `SQLDebug`, könnten Sie die folgende Anweisung in der Befehlszeile eingeben:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Eine ausführlichere Erklärung zu diesem Vorgang finden Sie unter [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Leitfaden zu Visual Studio und SQL Server, siebten Edition* sowie [Vorgehensweise: Festlegen von SQL Server-Berechtigungen für das Debuggen](https://msdn.microsoft.com/en-us/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Wenn der Entwicklungscomputer mit Windows XP Service Pack 2 ausgeführt wird, müssen Sie konfigurieren Sie die Windows-Firewall, um Remotedebugging zu ermöglichen. [Die Art und Weise an: SQL Server 2005-Debuggen aktivieren](https://msdn.microsoft.com/en-us/library/s0fk6z6e(VS.80).aspx) Artikel Hinweise, dass diese beiden Schritte umfasst: (a) auf dem Visual Studio-Hostcomputer müssen hinzufügen `Devenv.exe` zur Liste der Ausnahmen und öffnen Sie die TCP-Anschluss 135; und (b) auf dem Remotecomputer (SQL), müssen Sie öffnen die 135 TCP-port und fügen `sqlservr.exe` der Ausnahmenliste. Wenn die Domänenrichtlinie eine Netzwerkkommunikation über IPSec erfordert, müssen Sie die Ports UDP 4500 und UDP 500 öffnen.


## <a name="summary"></a>Zusammenfassung

Zusätzlich zur Bereitstellung von debugging-Unterstützung für .NET-Anwendungscode, bietet Visual Studio auch eine Vielzahl von Debugoptionen für SQL Server 2005. In diesem Lernprogramm erläutert, zwei Optionen: direkten Datenbankdebuggen und die zu Anwendungsdebuggen. Um ein T-SQL-Datenbankobjekt direkt Debuggen zu können, suchen Sie das Objekt über den Server-Explorer mit der rechten Maustaste darauf, und Einzelschritt auswählen. Dadurch wird der Debugger gestartet und hält bei der ersten Anweisung in das Datenbankobjekt, das an diesem, das Punkt können Sie schrittweise Durchlaufen der Objekt-s-Anweisungen, und überprüfen und Ändern von Parameterwerten an. In Schritt 1 wird dieser Ansatz mit den Einzelschritten der `Products_SelectByCategoryID` gespeicherten Prozedur.

Anwendungsdebuggen kann Haltepunkte direkt innerhalb der Datenbankobjekte festgelegt werden. Wenn ein Datenbankobjekt mit Haltepunkten aus einer Clientanwendung (z. B. eine ASP.NET-Webanwendung) aufgerufen wird, hält das Programm an, wie der Debugger übernimmt. Anwendungsdebugging ist nützlich, da deutlicher angezeigt Maßnahme Anwendung bewirkt, dass ein bestimmtes Datenbankobjekt aufgerufen werden. Dies erfordert allerdings mehr Konfiguration und Installation als direkten Datenbankdebuggen.

Datenbankobjekte können auch über SQL Server-Projekte gedebuggt werden. Betrachten wir mithilfe von SQL Server-Projekte, und verwenden diese zum Erstellen und Debuggen verwalteter Datenbankobjekte in den nächsten Lernprogrammen.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Zurück](protecting-connection-strings-and-other-configuration-information-cs.md)
[Weiter](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
