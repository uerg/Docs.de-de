---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Debuggen von gespeicherten Prozeduren (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Visual Studio Professional und Team System-Editionen können Sie Haltepunkte setzen und springen, um gespeicherte Prozeduren in SQL Server, zu debuggen gespeichert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 67714284c44c7bc9b06dc599601e116b83017e3e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402000"
---
<a name="debugging-stored-procedures-vb"></a>Debuggen von gespeicherten Prozeduren (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) oder [PDF-Datei herunterladen](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Visual Studio Professional und Team System-Editionen können Sie Haltepunkte setzen und springen, um zu gespeicherten Prozeduren in SQL Server, wodurch das Debuggen von gespeicherter Prozeduren, die so einfach wie das Debuggen von Anwendungscode. Dieses Tutorial veranschaulicht das direkte datenbankdebugging und Debuggen der Anwendung von gespeicherten Prozeduren.


## <a name="introduction"></a>Einführung

Visual Studio bietet eine umfassende Debugleistung. Mit wenigen Tastaturanschlägen oder Mausklicks es möglich, verwenden Sie zum Beenden der Ausführung eines Programms, und überprüfen die Übertragung und Haltepunkte. Zusammen mit Debuggen von Anwendungscode, bietet Visual Studio unterstützt das Debuggen von gespeicherter Prozeduren von SQL Server. Genau wie Haltepunkte können, im Code eines ASP.NET Code-Behind-Klasse oder Business Logic Layer-Klasse festgelegt werden, damit auch sie platziert werden können, innerhalb von gespeicherten Prozeduren.

In diesem Tutorial betrachten wir Einzelschritt in gespeicherte Prozeduren im Server-Explorer in Visual Studio auch zum Festlegen von Haltepunkten, die erreicht werden, wenn die gespeicherte Prozedur von der ausgeführten ASP.NET-Anwendung aufgerufen wird.

> [!NOTE]
> Leider gespeicherte Prozeduren können nur werden in das gesprungen und über die Professional und Team-Systeme Versionen von Visual Studio debuggt. Wenn Sie Visual Web Developer oder die standard-Version von Visual Studio verwenden, können Sie Willkommen zusammen zu lesen, durchlaufen wir die erforderlichen Schritte zum Debuggen von gespeicherter Prozeduren, aber Sie werden nicht in der Lage, diese Schritte auf Ihrem Computer replizieren.


## <a name="sql-server-debugging-concepts"></a>Debuggen von SQL Server-Konzepte

Microsoft SQL Server 2005 wurde entworfen, um die Integration in die [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), die Laufzeit, die von allen Assemblys für .NET verwendet wird. Daher unterstützt SQL Server 2005 verwalteten Datenbankobjekte. Das heißt, können Sie Datenbankobjekte wie gespeicherte Prozeduren und benutzerdefinierten Funktionen (UDFs) als Methoden in einer Visual Basic-Klasse erstellen. Dadurch können diese gespeicherten Prozeduren und benutzerdefinierte Funktionen, Funktionen, die in .NET Framework und von Ihren eigenen benutzerdefinierten Klassen nutzen können. Natürlich ist es möglich, bietet SQL Server 2005 auch Unterstützung für T-SQL-Datenbankobjekte.

SQL Server 2005 bietet Unterstützung für Remotedebuggen für T-SQL und verwalteter Datenbankobjekte. Allerdings können diese Objekte über die Editionen Visual Studio 2005 Professional und Team-Systeme nur gedebuggt werden. In diesem Tutorial untersuchen wir Debuggen T-SQL-Datenbankobjekte. Das nachfolgende Lernprogramm untersucht Debuggen verwalteter Datenbankobjekte.

Die [Übersicht über die von T-SQL und CLR-Debuggen in SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) Blogeintrag aus der [CLR-Integration für SQL Server 2005-Team](https://blogs.msdn.com/sqlclr/default.aspx) werden die drei Möglichkeiten zum Debuggen von SQL Server 2005-Objekte aus Visual Studio:

- **Leiten Sie die Datenbank-Debuggen (DDD)** : Klicken Sie in Server-Explorer, die wir einen in T-SQL-Datenbankobjekte wie gespeicherte Prozeduren und benutzerdefinierte Funktionen Einzelschritt. Untersuchen wir DDD in Schritt 1.
- **Debuggen der Anwendung** – wir Festlegen von Haltepunkten in einem Datenbankobjekt und führen Sie dann auf unsere ASP.NET-Anwendung. Wenn das Datenbankobjekt, das ausgeführt wird, wird der Haltepunkt erreicht, und an den Debugger umgedreht Steuerelement. Beachten Sie, dass beim Anwendungsdebuggen wir in ein Datenbankobjekt aus dem Anwendungscode wechseln können. Wir müssen explizit legen Sie Haltepunkte in diesen gespeicherten Prozeduren oder benutzerdefinierte Funktionen möchten wir, in denen den Debugger anhält. Anwendungsdebuggen wird untersucht, in Schritt2 ab.
- **Aus einem SQL Server-Projekt Debuggen** -Editionen von Visual Studio Professional und Team-Systeme sind einen SQL Server-Projekt-Typ, der häufig verwendet wird, um Datenbankobjekte zu erstellen. Wir werden untersuchen, verwenden SQL Server-Projekte und Debuggen ihre Inhalte im nächsten Tutorial.

Visual Studio kann gespeicherte Prozeduren für SQL Server-Instanzen für lokale und remote Debuggen. Eine lokale SQL Server-Instanz ist eine, die auf dem gleichen Computer wie Visual Studio installiert ist. Wenn SQL Server-Datenbank, die Sie verwenden nicht auf dem Entwicklungscomputer befindet, wird er eine remote-Instanz behandelt. Für diese Tutorials haben wir lokale SQL Server-Instanzen verwendet wurden. Debuggen von gespeicherten Prozeduren in SQL Server-Remoteinstanz erfordert weitere Konfigurationsschritte erforderlich als beim Debuggen von gespeicherten Prozeduren in einer lokalen Instanz.

Wenn Sie eine lokale SQL Server-Instanz verwenden, können Sie beginnen Sie mit Schritt 1 und am Ende dieses Tutorials durcharbeiten. Bei Verwendung eine Remoteinstanz von SQL Server werden jedoch, Sie müssen zunächst, um sicherzustellen, dass beim Debuggen auf Ihren Entwicklungscomputer mit einem Windows-Benutzerkonto an, die eine SQL Server-Anmeldung auf der Remoteinstanz angemeldet sind. Moveover, sowohl für diese datenbankanmeldung als auch für die datenbankanmeldung, die von der ausgeführten ASP.NET-Anwendung eine Verbindung mit der Datenbank verwendet, muss Mitglied der `sysadmin` Rolle. Finden Sie das Debuggen von T-SQL-Datenbankobjekte auf Remoteinstanzen im Abschnitt am Ende dieses Tutorials für Weitere Informationen zum Konfigurieren von Visual Studio und SQL Server, um eine remote-Instanz zu debuggen.

Verstehen Sie schließlich, dass debugging-Unterstützung für T-SQL-Datenbankobjekte nicht als Features wie debugging-Unterstützung für .NET-Anwendungen bietet. Z. B. haltepunktbedingungen und-Filter werden nicht unterstützt, nur eine Teilmenge der Debugfenster verfügbar, Sie können nicht verwenden, bearbeiten und fortfahren, das "Direktfenster" nutzlos usw. gerendert wird. Finden Sie unter [Einschränkungen von Debuggerbefehlen und Funktionen](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) für Weitere Informationen.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Schritt 1: Direkte Schrittweises Ausführen einer gespeicherten Prozedur

Visual Studio erleichtert das direkte Debuggen eines Datenbankobjekts. S betrachten, wie Sie mit der direkten Datenbank Debuggen (DDD)-Funktion in Einzelschritten können die `Products_SelectByCategoryID` gespeicherten Prozedur in der Northwind-Datenbank. Wie der Name schon sagt, `Products_SelectByCategoryID` gibt Produktinformationen für eine bestimmte Kategorie; es erstellt wurde die [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet-s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Tutorial. Navigieren Sie zum Server-Explorer, und erweitern Sie den Knoten der Northwind-Datenbank. Als Nächstes Drilldown in den Ordner gespeicherte Prozeduren, mit der rechten Maustaste auf die `Products_SelectByCategoryID` gespeicherte Prozedur aus, und wählen Sie die Option Schritt in gespeicherte Prozedur aus dem Kontextmenü. Dadurch wird den Debugger gestartet.

Da die `Products_SelectByCategoryID` gespeicherte Prozedur erwartet, dass eine `@CategoryID` Eingabeparameter, wir werden aufgefordert, diesen Wert anzugeben. Geben Sie 1 an, die Informationen zu den Getränke zurückgegeben wird.


![Verwenden Sie den Wert 1 für die @CategoryID Parameter](debugging-stored-procedures-vb/_static/image1.png)

**Abbildung 1**: Verwenden Sie den Wert 1 für die `@CategoryID` Parameter


Nach dem Bereitstellen des Werts für die `@CategoryID` Parameter der gespeicherten Prozedur ausgeführt wird. Anstatt bis zum Abschluss ausgeführt, jedoch hält der Debugger die Ausführung bei der ersten Anweisung. Beachten Sie den gelben Pfeil in den Rand, der angibt, der aktuellen Position in der gespeicherten Prozedur. Sie können anzeigen und Bearbeiten von Parameterwerten, die über das Fenster "überwachen" oder mit dem Mauszeiger auf den Parameternamen in der gespeicherten Prozedur.


[![Der Debugger wurde für die erste Anweisung der gespeicherten Prozedur angehalten.](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Abbildung 2**: der Debugger wurde angehalten, für die erste Anweisung der gespeicherten Prozedur ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image4.png))


Um einen über die gespeicherte Prozedur eine Anweisung zu einem Zeitpunkt durchzuführen, klicken Sie auf die Schaltfläche "Step Over" in der Symbolleiste, oder drücken Sie die F10-TASTE. Die `Products_SelectByCategoryID` gespeicherte Prozedur enthält ein einzelnes `SELECT` Anweisung, daher die einzige Anweisung überspringen und die Ausführung der gespeicherten Prozedur abgeschlossen wird drücken F10. Nachdem die gespeicherte Prozedur abgeschlossen wurde, wird die Ausgabe im Ausgabefenster angezeigt, und der Debugger wird beendet.

> [!NOTE]
> T-SQL-debugging erfolgt auf der Anweisungsebene Sie können nicht schrittweise ein `SELECT` Anweisung.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Schritt 2: Konfigurieren der Website für das Debuggen der Anwendung

Beim Debuggen einer gespeicherten Prozedur direkt über den Server-Explorer ist, sind in vielen Szenarien wir weitere Informationen über die gespeicherte Prozedur Debuggen, wenn sie von unserem ASP.NET-Anwendung aufgerufen wird. Wir können Haltepunkte hinzufügen, um eine gespeicherte Prozedur in Visual Studio, und starten Sie die ASP.NET-Anwendung debuggen. Wenn eine gespeicherte Prozedur mit Haltepunkten aus der Anwendung aufgerufen wird, wird die Ausführung am Haltepunkt angehalten, und wir können anzeigen und ändern Sie die Parameterwerte der gespeicherten Prozedur s und durchlaufen Sie die Anweisungen, wie wir in Schritt 1.

Bevor wir mit dem Debuggen von gespeicherten Prozeduren, die aufgerufen wird, von der Anwendung beginnen können, müssen wir die ASP.NET-Webanwendung für die Integration von SQL Server-Debugger anweisen. Starten, indem Sie mit der rechten Maustaste auf den Namen der Website im Projektmappen-Explorer (`ASPNET_Data_Tutorial_74_VB`). Wählen Sie die Eigenschaftenseiten-Option im Kontextmenü, wählen Sie das Element "Startoptionen" auf der linken Seite und aktivieren Sie das SQL Server-Kontrollkästchen im Abschnitt Debugger (siehe Abbildung 3).


[![Aktivieren Sie das SQL Server-Kontrollkästchen in der Anwendung s-Eigenschaftenseiten](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Abbildung 3**: Aktivieren Sie das SQL Server-Kontrollkästchen in der Anwendung-s-Eigenschaftenseiten ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image7.png))


Darüber hinaus müssen wir zum Aktualisieren der Datenbank-Verbindungszeichenfolge, die von der Anwendung verwendet wird, so dass Verbindungspooling deaktiviert ist. Um ein T-SQL-Database-Objekt direkt zu debuggen, suchen Sie das Objekt über den Server-Explorer und dann mit der rechten Maustaste darauf, und Einzelschritt auswählen. Dadurch wird der Debugger gestartet und hält bei der ersten Anweisung des Datenbankobjekts, das an diesem Punkt können Sie durchlaufen die Objekt-s-Anweisungen und anzeigen und ändern die Parameterwerte. In Schritt 1 dieser Ansatz in Einzelschritten wird die  gespeicherte Prozedur. Anwendungsdebuggen können Haltepunkte direkt in die Datenbankobjekte festgelegt werden.

Wenn ein Datenbankobjekt mit Haltepunkten aus einer Clientanwendung (z. B. eine ASP.NET-Webanwendung) aufgerufen wird, stoppt das Programm, wie der Debugger übernimmt.


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Anwendungsdebuggen ist nützlich, weil es deutlicher zeigt, welche Anwendungsaktion führt dazu, dass ein bestimmtes Datenbankobjekt aufgerufen werden.


Es erfordert jedoch ein wenig mehr Konfigurations- und Setupprobleme als direktes Datenbankdebugging. Datenbankobjekte können auch über SQL Server-Projekte gedebuggt werden.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Betrachten wir mithilfe von SQL Server-Projekte, und verwenden sie zum Erstellen und Debuggen von verwalteten Datenbankobjekten im nächsten Tutorial.

Viel Spaß beim Programmieren! Wie in Abbildung 4 dargestellt, wird der Haltepunkt als roter Kreis im Rand angezeigt.


[![Festlegen eines Haltepunkts in der Products_SelectByCategoryID gespeicherten Prozedur](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Abbildung 4**: Festlegen eines Haltepunkts in der `Products_SelectByCategoryID` Stored Procedure ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image10.png))


In der Reihenfolge für eine SQL-Datenbank zu debuggenden Objekts, durch eine Clientanwendung ist es zwingend erforderlich, dass die Datenbank für die Unterstützung des Debuggens der Anwendung konfiguriert werden. Wenn Sie zuerst einen Haltepunkt festlegen, sollten diese Einstellung automatisch aktiviert werden, aber es ist ratsam, überprüfen Sie noch einmal. Mit der rechten Maustaste auf die `NORTHWND.MDF` Knoten im Server-Explorer. Das Kontextmenü sollte aktiviert Menüelement Anwendungsdebugging enthalten.


![Stellen Sie sicher, dass die Anwendung debuggen-Option aktiviert ist](debugging-stored-procedures-vb/_static/image11.png)

**Abbildung 5**: Stellen Sie sicher, dass die Anwendung debuggen-Option aktiviert ist


Mit dem gesetzten Haltepunkt und die Anwendungsdebuggen-Option aktiviert sind wir bereit für die gespeicherte Prozedur, die beim Aufruf von der ASP.NET-Anwendung zu debuggen. Starten Sie den Debugger, indem Sie im Menü Debuggen, und Sie Debuggen starten auswählen, durch Drücken von F5 oder durch Klicken auf die grüne play-Symbol auf der Symbolleiste. Den Debugger zu starten wird und die Website zu starten.

Die `Products_SelectByCategoryID` gespeicherte Prozedur wurde erstellt, der [vorhandene gespeicherte Prozeduren verwenden, für die typisierte DataSet-s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Tutorial. Die entsprechende Webseite (`~/AdvancedDAL/ExistingSprocs.aspx`) enthält eine GridView, die von dieser gespeicherten Prozedur zurückgegebenen Ergebnisse werden angezeigt. Besuchen Sie diese Seite über den Browser aus. Beim Erreichen der Seite, die den Haltepunkt in der `Products_SelectByCategoryID` gespeicherte Prozedur wird erreicht, und Visual Studio die Steuerung zurückgegeben. Genau wie in Schritt 1, können Sie schrittweise Durchlaufen der gespeicherten Prozedur s-Anweisungen und die Ansicht und die Parameterwerte ändern.


[![Die Seite ExistingSprocs.aspx werden zuerst die Getränke angezeigt.](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Abbildung 6**: die `ExistingSprocs.aspx` Seite zeigt zu Beginn der Getränke ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image14.png))


[![Die gespeicherte Prozedur s Haltepunkt wurde erreicht](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Abbildung 7**: die gespeicherte Prozedur s Haltepunkt erreicht wurde ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image17.png))


Als das Fenster "überwachen" in Abbildung 7 zeigt, der Wert des der `@CategoryID` -Parameter ist 1. Grund hierfür ist die `ExistingSprocs.aspx` Seite zeigt anfänglich Produkte in der Kategorie "Getränke", die eine `CategoryID` Wert 1. Wählen Sie eine andere Kategorie aus der Dropdown Liste ein. Dies führt dazu, dass einen Postback und führt erneut die `Products_SelectByCategoryID` gespeicherte Prozedur. Der Haltepunkt erreicht wird, erneut aus, aber dieses Mal die `@CategoryID` s-Parameterwert entspricht dem ausgewählten Dropdown-Listenfeld-Element s `CategoryID`.


[![Wählen Sie eine andere Kategorie aus der Dropdown Liste](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Abbildung 8**: Wählen Sie eine andere Kategorie aus der Dropdown Liste ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image20.png))


[![Die @CategoryID Parameter gibt die Kategorie, die von der Webseite ausgewählt](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Abbildung 9**: die `@CategoryID` Parameter gibt die Kategorie, die von der Webseite ausgewählt ([klicken Sie, um das Bild in voller Größe anzeigen](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Wenn der Breakpoint im der `Products_SelectByCategoryID` gespeicherte Prozedur wird nicht erreicht werden, wenn das Unternehmen Besuchen der `ExistingSprocs.aspx` Seite, stellen Sie sicher, dass das Kontrollkästchen für die SQL Server im Abschnitt "Debugger" der ASP.NET-Anwendung s-Seite "Eigenschaften" markiert ist, wurde der Verbindungs-pooling deaktiviert, und dass die Datenbank s Anwendungsdebugging-Option aktiviert ist. Sollten Sie erneut immer noch Probleme auftreten, starten Sie Visual Studio, und versuchen Sie es erneut.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Debuggen von T-SQL-Datenbankobjekte auf Remoteinstanzen

Debuggen von Datenbankobjekten über Visual Studio ist recht einfach, wenn die SQL Server-Datenbankinstanz auf dem gleichen Computer wie Visual Studio ist. Jedoch SQL Server und Visual Studio, die auf verschiedenen Computern befinden sich ist eine sorgfältige Konfiguration erforderlich, alles ordnungsgemäß funktioniert. Es gibt zwei Hauptaufgaben, die, denen wir mit konfrontiert sind:

- Stellen Sie sicher, dass die Anmeldung bei der Herstellung einer Verbindung mit der Datenbank über ADO.NET verwendet, gehört die `sysadmin` Rolle.
- Stellen Sie sicher, dass das Windows-Benutzerkonto von Visual Studio verwendet werden, auf dem Entwicklungscomputer ein gültiger SQL Server-Anmeldekonto ist, zu dem gehört die `sysadmin` Rolle.

Der erste Schritt ist relativ unkompliziert. Ermitteln Sie zuerst das Benutzerkonto zum Verbinden mit der Datenbank von der ASP.NET-Anwendung ein, und fügen Sie in SQL Server Management Studio, Anmeldekonto an, die `sysadmin` Rolle.

Der zweite Task erfordert, dass die Windows-Benutzerkonto, das Sie verwenden, um das Debuggen der Anwendung eine gültige Anmeldung auf der Remotedatenbank. Allerdings ist es wahrscheinlich an, dass das Windows-Konto, das Sie auf Ihrer Arbeitsstation mit angemeldet nicht um eine gültige Anmeldung auf SQL Server ist. Anstelle von SQL Server Ihr bestimmtes Anmeldekonto hinzugefügt haben, würde eine bessere Wahl sein, einige Windows-Benutzerkonto als das debugging SQL Server-Dienstkonto festgelegt werden soll. Um die Datenbankobjekte einer SQL Server-Remoteinstanz zu debuggen, ausführen Sie anschließend Visual Studio, die mit dieser Windows-Anmeldeinformationen Konto s.

Ein Beispiel sollte Ihnen Dinge. Angenommen, es ein Windows-Konto, das mit dem Namen ist `SQLDebug` innerhalb der Windows-Domäne. Dieses Konto mit der SQL Server-Remoteinstanz als gültigen Anmeldenamen und ein Mitglied hinzugefügt werden müssten die `sysadmin` Rolle. Klicken Sie dann, um die SQL Server-Remoteinstanz in Visual Studio zu debuggen, müssen wir zum Ausführen von Visual Studio als die `SQLDebug` Benutzer. Dies kann durch Abmelden eigenen Arbeitsstation, wieder anmelden, als `SQLDebug`, und klicken Sie dann starten von Visual Studio, jedoch ein einfacherer Ansatz wäre, die Anmeldung bei den eigenen Arbeitsstation mit eigenen Anmeldeinformationen und verwenden Sie dann `runas.exe` zum Starten von Visual Studio als die `SQLDebug` Benutzer. `runas.exe` können eine bestimmte Anwendung unter dem Deckmantel von einem anderen Benutzerkonto ausgeführt werden. Zum Starten von Visual Studio als `SQLDebug`, könnten Sie die folgende Anweisung in der Befehlszeile eingeben:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Eine ausführlichere Erläuterung zu diesem Prozess werden soll, finden Sie unter [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s Leitfaden für Visual Studio und SQL Server, die siebte Ausgabe* sowie [so wird's gemacht: SQL Server-Berechtigungen festlegen für das Debuggen](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Wenn Ihrem Entwicklungscomputer Windows XP Service Pack 2 ausgeführt wird, Sie die Windows-Firewall zum Remotedebuggen konfigurieren müssen. [Wie zu: SQL Server 2005-Debuggen aktivieren](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) Artikel – Anmerkungen zu dieser, dass diese zwei Schritten besteht: (a) auf dem Visual Studio-Hostcomputer, müssen Sie hinzufügen `Devenv.exe` auf die Liste mit Ausnahmen und öffnen Sie die TCP-Anschluss 135; und (b) auf einem Remotecomputer (SQL), müssen Sie öffnen die 135 TCP-port, und fügen `sqlservr.exe` zur Liste Ausnahmen. Wenn die Domänenrichtlinie die Netzwerkkommunikation über IPSec erfordert, müssen Sie die Ports UDP 4500 und UDP 500 öffnen.


## <a name="summary"></a>Zusammenfassung

Zusätzlich zur Bereitstellung von debugging-Unterstützung für .NET-Anwendungscode, bietet Visual Studio auch eine Vielzahl von Debugoptionen für SQL Server 2005. In diesem Tutorial erläutert, zwei dieser Optionen: Direktes Datenbankdebugging und Anwendungsdebuggen. Um ein T-SQL-Database-Objekt direkt zu debuggen, suchen Sie das Objekt über den Server-Explorer und dann mit der rechten Maustaste darauf, und Einzelschritt auswählen. Dadurch wird der Debugger gestartet und hält bei der ersten Anweisung des Datenbankobjekts, das an diesem Punkt können Sie durchlaufen die Objekt-s-Anweisungen und anzeigen und ändern die Parameterwerte. In Schritt 1 dieser Ansatz in Einzelschritten wird die `Products_SelectByCategoryID` gespeicherte Prozedur.

Anwendungsdebuggen können Haltepunkte direkt in die Datenbankobjekte festgelegt werden. Wenn ein Datenbankobjekt mit Haltepunkten aus einer Clientanwendung (z. B. eine ASP.NET-Webanwendung) aufgerufen wird, stoppt das Programm, wie der Debugger übernimmt. Anwendungsdebuggen ist nützlich, weil es deutlicher zeigt, welche Anwendungsaktion führt dazu, dass ein bestimmtes Datenbankobjekt aufgerufen werden. Es erfordert jedoch ein wenig mehr Konfigurations- und Setupprobleme als direktes Datenbankdebugging.

Datenbankobjekte können auch über SQL Server-Projekte gedebuggt werden. Betrachten wir mithilfe von SQL Server-Projekte, und verwenden sie zum Erstellen und Debuggen von verwalteten Datenbankobjekten im nächsten Tutorial.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Zurück](protecting-connection-strings-and-other-configuration-information-vb.md)
> [Weiter](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
