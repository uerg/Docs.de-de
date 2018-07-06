---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Konfigurieren der Produktionswebanwendung mithilfe der Produktionsdatenbank (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Wie in den vorherigen Tutorials erwähnt, ist es nicht ungewöhnlich, dass Informationen zu den Umgebungen für Entwicklungs- und produktionsumgebungen unterscheiden. Dies ist es...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 952bdafd06143ce24e9e8beb0d6e6343400ffdc6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840699"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Konfigurieren der Produktionswebanwendung mithilfe der Produktionsdatenbank (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Wie in den vorherigen Tutorials erwähnt, ist es nicht ungewöhnlich, dass Informationen zu den Umgebungen für Entwicklungs- und produktionsumgebungen unterscheiden. Dies gilt insbesondere für datengesteuerte Webanwendungen, wie die Datenbank-Verbindungszeichenfolgen zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen unterscheiden. In diesem Tutorial werden Methoden zum Konfigurieren der produktionsumgebung, um die entsprechende Verbindungszeichenfolge im Detail beinhalten behandelt.


## <a name="introduction"></a>Einführung

Datengesteuerte Webanwendungen verwenden eine andere Datenbank aus, wenn in der Entwicklung als bei in der Regel in der Produktion. Für Anwendungen, die von einer Web-Host-Anbieter gehostet und entwickelt wurde, lokal befindet sich die Entwicklungsdatenbank in der Regel auf dem Computer des Entwicklers s während die Produktionsdatenbank auf einem Datenbankserver, auf die Webhosting-Unternehmens-s-Funktion gehostet wird. Bereitstellung einer datengesteuerten Webanwendung umfasst das Kopieren der Entwicklungsdatenbank an den Produktions-Datenbankserver. Im vorherigen Tutorial haben Sie Möglichkeiten zum Durchführen dieses Schritts.

Die Webanwendung verwendet die Informationen in einem *Verbindungszeichenfolge* zum Herstellen einer Verbindung mit der Datenbank. Die Verbindungszeichenfolge, die in der Regel in gespeichert ist `Web.config`, gibt den Namen des Datenbankservers, den Namen der Datenbank, den Sicherheitskontext und andere Informationen. Da die Datenbank, die von der Webanwendung verwendet, hängt davon ab, ob die Webanwendung in den Umgebungen für Entwicklung oder Produktion ausgeführt wird, müssen die Verbindungszeichenfolgen zwischen den beiden Umgebungen unterscheiden.

Es ist nicht ungewöhnlich, dass Informationen zu den Umgebungen für Entwicklungs- und produktionsumgebungen unterscheiden. Die *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* Lernprogramm erläutert Techniken für die Verwaltung der separaten Konfigurationsinformationen zwischen diesen zwei Umgebungen sowie eine kurze Erläuterung Datenbank-Verbindungszeichenfolgen. In diesem Tutorial werden Methoden zum Konfigurieren der produktionsumgebung, um die entsprechende Verbindungszeichenfolge im Detail beinhalten behandelt.

## <a name="examining-the-connection-string-information"></a>Untersuchen die Informationen zur Verbindungszeichenfolge

Die Verbindungszeichenfolge, die von der Webanwendung Book Reviews verwendet befindet sich in der Konfigurationsdatei der Anwendung s `Web.config`. `Web.config` enthält einen speziellen Bereich für das Speichern von Verbindungszeichenfolgen, treffend [ &lt;ConnectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Die `Web.config` Datei für die Website Book Reviews eine Verbindungszeichenfolge, die in diesem Abschnitt, der mit dem Namen definiert hat `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Die Verbindungszeichenfolge - Datenquelle =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True; User Instance = True: besteht aus einer Anzahl von Optionen und Werte, mit Option-Wert-Paare, getrennt durch ein Semikolon und jede Option und dem Wert, der durch ein Gleichheitszeichen getrennt. Die vier Optionen in dieser Verbindungszeichenfolge verwendet werden:

- `Data Source` -Gibt den Speicherort der Datenbank-Server und den Datenbankserver-Instanzname, (sofern vorhanden). Der Wert `.\SQLEXPRESS`, ist ein Beispiel vorhanden ist, einen Datenbank-Server und einen Instanznamen an. Der Punkt gibt an, dass der Datenbankserver auf dem gleichen Computer wie die Anwendung; der Instanzname ist `SQLEXPRESS`.
- `AttachDbFilename` -Gibt den Speicherort der Datenbankdatei. Der Wert enthält, den Platzhalter `|DataDirectory|`, d.h. auf den vollständigen Pfad der Anwendung s aufgelöst `App_Data` Ordner zur Laufzeit.
- `Integrated Security` – Ein boolescher Wert, der angibt, ob eine angegebene Benutzername und Kennwort verwenden, beim Verbinden mit der Datenbank (False) oder die aktuelle Windows Anmeldeinformationen (True).
- `User Instance` -eine Konfigurationsoption, die spezifisch für die SQL Server Express-Editionen, die angibt, ob nicht-Administratoren auf dem lokalen Computer anfügen, und Verbinden mit einer SQL Server Express Edition-Datenbank. Finden Sie unter [SQL Server Express-Benutzerinstanzen](https://msdn.microsoft.com/library/ms254504.aspx) für Weitere Informationen zu dieser Einstellung.
  

Optionen für die zulässige Verbindungszeichenfolge hängt von der Datenbank, die Sie eine Verbindung mit und die [ADO.NET](http://ADO.NET) Datenbankanbieter verwendet wird. Z. B. die Verbindungszeichenfolge für die Verbindung mit einer Microsoft SQL Server Datenbank unterscheidet, die zur Verbindung mit einer Oracle-Datenbank. Ebenso wird beim Herstellen einer Verbindung mit einer Microsoft SQL Server-Datenbank, die mit dem SqlClient-Anbieter eine andere Verbindungszeichenfolge als bei der Verwendung des OLE-DB-Anbieters verwendet.

Sie können manuell mit einer Website wie die Datenbank-Verbindungszeichenfolge erstellen [ConnectionStrings.com](http://www.connectionstrings.com/) als Ressource für welche Optionen verfügbar sind. Ein einfacherer Ansatz ist jedoch auf die Datenbank auf dem Server-Explorer in Visual Studio hinzufügen und dann ziehen Sie die Verbindungszeichenfolge aus dem Fenster "Eigenschaften". Lassen Sie s, die diesem Verfahren zum Erstellen der Verbindungszeichenfolge mit dem Produktions-Datenbankserver verwenden.

Öffnen Sie Visual Studio, und navigieren Sie zum Server Explorer-Fensters (in Visual Web Developer dieses Fenster genannt Datenbank-Explorer angezeigt). Mit der rechten Maustaste auf die Option Data Connections, und wählen Sie im Kontextmenü die Option Verbindung hinzufügen. Dadurch wird der Assistent, der in Abbildung 1 dargestellt. Wählen Sie die entsprechende Datenquelle aus, und klicken Sie auf Weiter.


[![Wählen Sie eine neue Datenbank im Server-Explorer hinzu](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Abbildung 1**: Wählen Sie im Server-Explorer eine neue Datenbank hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


Geben Sie als Nächstes die verschiedenen Datenbank-Verbindungsinformationen (siehe Abbildung 2). Wenn Sie sich mit Ihrem Webhosting registriert, haben sollte sie bereitgestellt haben Informationen zum Verbinden mit der Datenbank – den Namen des Datenbankservers, den Datenbanknamen, den Benutzernamen und ein Kennwort zum Verbinden mit der Datenbank, und so weiter. Klicken Sie nach Eingabe dieser Informationen auf OK, um diesen Assistenten abzuschließen und um die Datenbank auf dem Server-Explorer hinzuzufügen.


[![Geben Sie die Datenbank-Verbindungsinformationen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Abbildung 2**: Geben Sie die Datenbank-Verbindungsinformationen ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


Die Datenbank der Produktion Umgebung sollte jetzt im Server-Explorer aufgeführt werden. Wählen Sie die Datenbank im Server-Explorer, und öffnen Sie das Fenster "Eigenschaften" aus. Dort finden Sie eine Eigenschaft, die durch die Verbindungszeichenfolge der Datenbank-s-Verbindungszeichenfolge. Vorausgesetzt, dass Sie Microsoft SQL Server-Datenbank für Produktions- und dem SqlClient-Anbieter verwenden, sollte Ihre Verbindungszeichenfolge etwa wie folgt aussehen:

<strong>Datenquelle =<em>ServerName</em>; Initial Catalog =<em>DatabaseName</em>; Persist Security Info = True; Benutzer-ID =<em>Benutzername</em>; Kennwort =*Kennwort</strong>*

In denen *ServerName*, *DatabaseName*, *Benutzername*, und *Kennwort* sind mit den Werten für den Namen des Datenbankservers, der Datenbank Namen und den Benutzernamen und Kennwort, die Sie von Ihrem Unternehmen der Web-Host bereitgestellt.

## <a name="deploying-the-book-reviews-web-application"></a>Bereitstellen der Book Reviews-Webanwendung

Des vorherigen Tutorials in Einzelschritten durchlaufen die Entwicklungsdatenbank in der produktionsumgebung kopieren, aber es nicht untersuchen die datengesteuerte Anwendung bereitstellen. Zu diesem Zeitpunkt wird die produktionsumgebung enthält die Datenbank, aber die Version der Anwendung Book Reviews mit statischen Reviews verwendet. Die neue, datengesteuerte Anwendung auf dem Produktionsserver sowie die aktualisierten Konfigurationsinformationen bereitstellen müssen.

Nehmen Sie einen Moment Zeit, um die Anwendung eines datengesteuerten aus der Entwicklungsumgebung für die Produktion bereitzustellen. Dieser Prozess wurde in vorherigen Tutorials ausführlich behandelt. Eine Auffrischung, finden Sie unter den *Bereitstellen Ihrer Website mithilfe von FTP-Client* oder *Bereitstellen Ihrer Website mit Visual Studio* Tutorials. Sie müssen sicherstellen, dass die Verbindungszeichenfolge für die Produktion wird verwendet, in der produktionsumgebung, dies, dass alternative bedeutet `Web.config` Datei muss bereitgestellt werden. Insbesondere diese Änderung `Web.config` Datei s `<connectionStrings>` Element muss die Verbindungszeichenfolge für die Produktion enthalten und sollte etwa wie folgt aussehen:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Beachten Sie, die die Verbindungszeichenfolge in der `<connectionStrings>` Element den gleichen Namen (`ReviewsConnectionString`), aber jetzt enthält die Produktions-Datenbank-Verbindungszeichenfolge anstelle der Verbindungszeichenfolge für die Entwicklung.

Es sei denn, Sie haben einen mehr formalisierten Deployment-Workflow entweder manuell ändern, die `Web.config` Datei, die Verbindungszeichenfolge für die Produktion verwendet werden, vor der Bereitstellung (Denken Sie daran, wieder den sie mit der Verbindungszeichenfolge für die Entwicklung danach) oder verwalten eine separaten `Web.config` -Datei mit der Produktion Umgebung Konfigurationsinformationen, die in der produktionsumgebung im Rahmen des Bereitstellungsprozesses hochgeladen haben, ruft.

> [!NOTE]
> Wenn Sie versehentlich Bereitstellen einer `Web.config` Datei, die Verbindungszeichenfolge für die Entwicklung enthält, und es ein Fehler sein wird, wenn die Anwendung auf Produktions-versucht, mit der Datenbank herstellen. Dieser Fehler in Form einer `SqlException` mit einer Meldung, die meldet, dass der Server nicht gefunden wurde oder nicht zugegriffen werden konnte.


Nachdem der Standort in der produktionsumgebung bereitgestellt wurde, finden Sie auf der Produktionswebsite über Ihren Browser. Sie finden Sie unter, und die gleiche benutzerfreundlichkeit wie profitieren, wenn die datengesteuerte Anwendung lokal ausgeführt. Natürlich beim Besuch der Website auf Produktions-wird die Website von den Produktions-Datenbankserver unterstützt während die Datenbank in der Entwicklung Besuchen der Website in der Entwicklungsumgebung verwendet werden. Abbildung 3 zeigt die *bringen Sie sich ASP.NET 3.5 in 24 Stunden* überprüfen Sie die Seite von der Website in der produktionsumgebung (Beachten Sie die URL in die Adressleiste des Browsers s).


[![Die Data-Driven-Anwendung ist jetzt verfügbar in Produktion!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Abbildung 3**: The Data-Driven-Anwendung ist jetzt verfügbar in Produktion! ([Klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Das Speichern von Verbindungszeichenfolgen in einer separaten Konfigurationsdatei

Ein gängiges Verfahren für die Verwaltung von separate Konfiguration für die Umgebungen für Entwicklungs- und produktionsumgebungen ist, haben Sie zwei Versionen der `Web.config`: eine für die Entwicklungsumgebung und eine für die Produktion. Zum Zeitpunkt der Bereitstellung der entsprechenden `Web.config` Version kopiert werden kann, in der produktionsumgebung bereit. Im Idealfall würde dieser Prozess als Teil des Bereitstellungsworkflows automatisiert werden.

Anstatt zwei Separate `Web.config` Dateien, die Sie ausführen, optional können, geben Sie eine feiner abgestimmte Unterschiede. Die Elemente, aus denen die `Web.config` Datei kann definiert werden, in eine externe Konfigurationsdateien, klicken Sie dann auf die verweist, die `Web.config` Datei. Kurz gesagt können Sie haben eine `Web.config` Datei für die Umgebungen, die eine databaseConnectionStrings.config-Datei verweist auf die die Verbindungszeichenfolgen enthalten würde, die von der Anwendung verwendet und würde für jede Umgebung eindeutig sein. Meiner meinung nach die Trennung der unterschiedlichen Konfigurationsinformationen in separate Dateien einen besseren Überblick über Ihre bietet und einfacher `Web.config` -Datei und vieles mehr klar wird beschrieben, die Konfigurationsunterschiede zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen.

Um dieses Verfahren verwenden, erstellen Sie zunächst einen neuen Ordner in der Webanwendung, die mit dem Namen `ConfigSections`. Als Nächstes fügen Sie zwei Dateien auf diesen neuen Ordner namens "databaseConnectionStrings.dev.config" und "databaseConnectionStrings.production.config hinzu. Kopieren Sie anschließend die `<connectionStrings>` Element `Web.config` in den Dateien databaseConnectionStrings.dev.config und databaseConnectionStrings.production.config und ändern Sie die Verbindungszeichenfolge in der databaseConnectionStrings.production.config Datei, damit sie die Verbindungszeichenfolge für die Produktion gibt. Beispielsweise sollte die Datei databaseConnectionStrings.dev.config enthalten nur die `<connectionStrings>` Element mit einer Verbindungszeichenfolge, die die Entwicklungsdatenbank verweist:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Die Datei databaseConnectionStrings.production.config sollte ebenso einfach enthalten eine `<connectionStrings>` -Element, aber eine, die Verbindungszeichenfolge für die produktionsumgebung hat.

Erstellen Sie eine Kopie der Datei databaseConnectionStrings.dev.config aus, und nennen Sie sie databaseConnectionStrings.config.

> [!NOTE]
> Sie können Namen die Konfigurationsdatei einen anderen Wert als databaseConnectionStrings.config, wenn Sie d, wie z. B. `connectionStrings.config` oder `dbInfo.config`. Achten Sie jedoch zum Benennen der Datei mit einem `.config` Erweiterung als `.config` Dateien sind, wird standardmäßig nicht ausgeführt, von der Engine für ASP.NET. Wenn Sie die Datei einen anderen Namen, z. B. `connectionStrings.txt`, ein Benutzer kann verweisen Sie ihren Browser auf [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) und zeigen Sie den Inhalt der Datei.


An diesem Punkt die `ConfigSections` Ordner sollte drei Dateien (siehe Abbildung 4) enthalten. Die databaseConnectionStrings.dev.config und databaseConnectionStrings.production.config-Dateien enthalten die Verbindungszeichenfolgen für die Entwicklungs- und produktionsumgebungen, bzw. auf. Die databaseConnectionStrings.config-Datei enthält die Verbindungsinformationen für die Zeichenfolge, die von der Webanwendung zur Laufzeit verwendet werden. Daher muss die databaseConnectionStrings.config-Datei identisch mit der databaseConnectionStrings.dev.config-Datei in der Entwicklungsumgebung sein hingegen auf Produktions-databaseConnectionStrings.config Datei identisch sein databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Abbildung 4**: ConfigSections ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


Jetzt müssen wir weisen `Web.config` die databaseConnectionStrings.config-Datei für die Verbindung Zeichenfolgenspeicher zu verwenden. Öffnen Sie `Web.config`, und ersetzen Sie das vorhandene `<connectionStrings>`-Element mit folgendem:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

Die `configSource` Attribut gibt an, einen physischen Pfad relativ zu den `Web.config` Datei. Wenn die externe `.config` Datei befindet sich im selben Verzeichnis wie `Web.config` legen Sie dann auf den Dateinamen, der dieses Attribut die `.config` Datei. Wenn sie s in einem Unterverzeichnis, wie bei databaseConnectionStrings.config, der Fall ist, geben Sie den Unterordner mit, dass ein umgekehrter Schrägstrich den Ordner- und -Namen, z.B. ConfigSections\databaseConnectionStrings.config begrenzen.

Dank dieser Änderung kann die Umgebungen für Entwicklungs- und produktionsumgebungen enthält denselben `Web.config` Datei. Jetzt ist der einzige Unterschied die databaseConnectionStrings.config-Datei. Kopieren Sie die databaseConnectionStrings.production.config-Datei für die Produktion, und benennen Sie sie in databaseConnectionStrings.config. Treten, in der Zukunft Änderungen an der Verbindungszeichenfolge für die Produktion müssen Sie sie in die Datei databaseConnectionStrings.production.config vornehmen, und klicken Sie dann die Datei in die Produktion hochladen databaseConnectionStrings.config umbenennen.

> [!NOTE]
> Sie können angeben, die Informationen für alle `Web.config` Element in einer separaten Datei und Verwenden der `configSource` Attribut auf die Datei aus `Web.config`.


## <a name="summary"></a>Zusammenfassung

Datengesteuerte Anwendungen werden in der Regel unterschiedliche Datenbanken verwenden, in der Entwicklungs- und produktionsumgebungen. Daher müssen die Datenbank-Verbindungszeichenfolgen, die in der Konfiguration der Webanwendung s gespeichert pro Umgebung eindeutig sein. In diesem Tutorial erläutert, wie zum Bestimmen der Produktions-Datenbank-Verbindungszeichenfolge und die Möglichkeiten, um die eindeutige Verbindungs-Informationen in den beiden Umgebungen verwalten.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Verbindungszeichenfolgen und Konfigurationsdateien](https://msdn.microsoft.com/library/ms254494.aspx)
- [Datenbankkonfiguration Zeichenfolgen Informationen @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Verschieben von Einstellungen aus der Datei "Web.config"](http://www.asp101.com/tips/index.asp?id=154)
- [Technische Dokumentation für die &lt;ConnectionStrings&gt; Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-a-database-vb.md)
> [Weiter](configuring-a-website-that-uses-application-services-vb.md)
