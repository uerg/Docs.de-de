---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: Konfigurieren der Produktions-Webanwendung mithilfe die Produktionsdatenbank (VB) | Microsoft Docs
author: rick-anderson
description: "Wie in früheren Lernprogrammen erläutert wird, ist es nicht ungewöhnlich, dass Informationen zu den Entwicklungs- und produktionsumgebungen Umgebungen unterscheiden. Dies ist es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 60ef1f93efea777e9309ad8c664a2c6645f1ce80
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>Konfigurieren der Produktions-Webanwendung mithilfe die Produktionsdatenbank (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> Wie in früheren Lernprogrammen erläutert wird, ist es nicht ungewöhnlich, dass Informationen zu den Entwicklungs- und produktionsumgebungen Umgebungen unterscheiden. Dies gilt insbesondere für datengesteuerte Webanwendungen wie die Datenbank-Verbindungszeichenfolgen zwischen der Entwicklungs- und produktionsumgebungen unterscheiden. In diesem Lernprogramm wird erklärt, Möglichkeiten zum Konfigurieren der produktionsumgebung, um die entsprechende Verbindungszeichenfolge ausführlicher enthalten.


## <a name="introduction"></a>Einführung

Datengesteuerte Webanwendungen verwenden eine andere Datenbank aus, wenn bei der Entwicklung als bei in der Regel in der Produktion. Für Anwendungen, die von einem Webhostinganbieter gehostet und lokal entwickelt wurde befindet sich die Entwicklungsdatenbank in der Regel auf dem Computer des Entwicklers s während die Produktionsdatenbank auf einem Datenbankserver an die Webhosting-s-Funktion für Unternehmen gehostet wird. Bereitstellen einer datengesteuerte Webanwendung umfasst das Kopieren der Entwicklungsdatenbank an den Produktions-Datenbankserver. Im vorherigen Lernprogramm haben wir Möglichkeiten, um diesen Schritt auszuführen.

Die Webanwendung verwendet die Informationen in einem *Verbindungszeichenfolge* zum Herstellen einer Verbindung mit der Datenbank. Die Verbindungszeichenfolge, die in der Regel in gespeichert ist `Web.config`, gibt den Namen des Datenbankservers, der Name der Datenbank, der Sicherheitskontext und andere Informationen. Da die Datenbank, die von der Webanwendung verwendet, hängt davon ab, ob die Webanwendung in der Entwicklung oder Produktion Umgebungen ausgeführt wird, müssen die Verbindungszeichenfolgen für die Verbindung zwischen den beiden Umgebungen unterscheiden.

Es ist nicht ungewöhnlich, dass Informationen zu den Entwicklungs- und produktionsumgebungen Umgebungen unterscheiden. Die *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* Lernprogramm erläutert Techniken für die Verwaltung von separaten Konfigurationsinformationen zwischen diesen beiden Umgebungen sowie eine kurze Erläuterung auf Datenbank-Verbindungszeichenfolgen. In diesem Lernprogramm wird erklärt, Möglichkeiten zum Konfigurieren der produktionsumgebung, um die entsprechende Verbindungszeichenfolge ausführlicher enthalten.

## <a name="examining-the-connection-string-information"></a>Untersuchen die Verbindungszeichenfolgeninformationen

Von der Webanwendung Buch Reviews verwendete Verbindungszeichenfolge befindet sich in der Konfigurationsdatei der Anwendung-s `Web.config`. `Web.config`enthält einen speziellen Bereich für das Speichern von Verbindungszeichenfolgen, geeignet benannt, [ &lt;ConnectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). Die `Web.config` -Konfigurationsdatei für das Buch Reviews-Website eine Verbindungszeichenfolge, die in diesem Abschnitt mit dem Namen definiert ist `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

Die Verbindungszeichenfolge - Datenquelle =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True; User Instance = True - besteht aus einer Reihe von Optionen und Werte, mit Option/Wert-Paaren, die getrennt durch ein Semikolon und jede Option und dem Wert, der durch ein Gleichheitszeichen getrennt. Die vier Optionen in dieser Verbindungszeichenfolge verwendet werden:

- `Data Source`-Gibt den Speicherort des Datenbankservers und der Datenbankserver-Instanzname (falls vorhanden). Der Wert `.\SQLEXPRESS`, ist ein Beispiel, in dem es ist ein Datenbankserver und einen Instanznamen an. Der Punkt gibt an, dass der Datenbankserver auf dem gleichen Computer wie die Anwendung; der Instanzname `SQLEXPRESS`.
- `AttachDbFilename`-Gibt den Speicherort der Datenbankdatei. Der Wert enthält den Platzhalter `|DataDirectory|`, also in den vollständigen Pfad der Anwendung s aufgelöst `App_Data` Ordner zur Laufzeit.
- `Integrated Security`– Ein boolescher Wert, der angibt, ob einen angegebene Benutzername/Kennwort verwendet wird, beim Verbinden mit der Datenbank ("false") oder der aktuelle Windows-Anmeldeinformationen ("true").
- `User Instance`-eine Konfigurationsoption, die spezifisch für die SQL Server Express-Edition, der angibt, ob nicht-Administratoren auf dem lokalen Computer Anfügen und Herstellen einer Verbindung mit einer SQL Server Express Edition-Datenbank erlauben. Finden Sie unter [SQL Server Express-Benutzerinstanzen](https://msdn.microsoft.com/library/ms254504.aspx) für Weitere Informationen zu dieser Einstellung.
  

Optionen für die zulässige Verbindungszeichenfolge hängt von der Datenbank, die Sie eine Verbindung mit und die [ADO.NET](http://ADO.NET) Datenbankanbieter verwendet wird. Z. B. die Verbindungszeichenfolge zum Herstellen einer Verbindung mit einer Microsoft SQL Server Datenbank unterscheidet, die zur Verbindung mit einer Oracle-Datenbank. Ebenso verwendet, Herstellen einer Verbindung mit einer Microsoft SQL Server-Datenbank mit dem SqlClient-Anbieter eine andere Verbindungszeichenfolge als bei Verwendung des OLE DB-Anbieters.

Sie können die Datenbank-Verbindungszeichenfolge erstellen, indem manuell mit einem Standort wie [ConnectionStrings.com](http://www.connectionstrings.com/) als eine Ressource, für welche Optionen verfügbar sind. Ein einfacher Ansatz ist jedoch auf die Datenbank auf dem Server-Explorer in Visual Studio hinzufügen und dann auf die Verbindungszeichenfolge im Eigenschaftenfenster. Lassen Sie s, die diesem Verfahren zum Erstellen der Verbindungszeichenfolge mit dem Produktions-Datenbankserver verwenden.

Öffnen Sie Visual Studio, und navigieren Sie zum Server-Explorer-Fenster (in Visual Web Developer dieses Fenster wird aufgerufen, Datenbank-Explorer). Mit der rechten Maustaste auf die Option Data Connections, und wählen Sie aus dem Kontextmenü die Option Verbindung hinzufügen. Daraufhin wird der Assistent in Abbildung 1 dargestellt. Wählen Sie die entsprechende Datenquelle, und klicken Sie auf Weiter.


[![Wählen Sie im Server-Explorer eine neue Datenbank hinzu](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**Abbildung 1**: Wählen Sie im Server-Explorer eine neue Datenbank hinzu ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


Geben Sie anschließend die verschiedenen Datenbank-Verbindungsinformationen (siehe Abbildung 2). Wenn Sie sich mit Ihrem Webhosting registriert, sollten sie bereitgestellt haben Informationen zum Herstellen einer Verbindung mit der Datenbank - Name des Datenbankservers, der den Datenbanknamen, der Benutzername und Kennwort, mit dem Herstellen einer Verbindung mit der Datenbank, und so weiter. Klicken Sie auf OK, um diesen Assistenten ausführen und den Server-Explorer die Datenbank hinzu, nachdem Sie diese Informationen eingegeben haben.


[![Geben Sie den Datenbank-Verbindungsinformationen](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**Abbildung 2**: Geben Sie den Datenbank-Verbindungsinformationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


Die Umgebung Produktionsdatenbank sollte jetzt im Server-Explorer aufgeführt. Wählen Sie die Datenbank im Server-Explorer, und wechseln Sie zum Fenster Eigenschaften. Dort finden Sie eine Eigenschaft mit dem Namen Verbindungszeichenfolge mit der Datenbank-s-Verbindungszeichenfolge. Vorausgesetzt, dass Sie eine Microsoft SQL Server-Datenbank auf Produktions- und dem SqlClient-Anbieter verwenden, sollte Ihre Verbindungszeichenfolge ähnlich der folgenden aussehen:

**Datenquelle =*ServerName*; Initial Catalog =*DatabaseName*; Persist Security Info = True; Benutzer-ID =*Benutzername*; Kennwort = * Kennwort***

Wobei *ServerName*, *DatabaseName*, *Benutzername*, und *Kennwort* sind mit den Werten für den Datenbank-Servernamen der Datenbank Namen und den Benutzernamen und Kennwort, die Ihnen von Ihrem Web Host Unternehmen bereitgestellt.

## <a name="deploying-the-book-reviews-web-application"></a>Bereitstellen der Buch Reviews Web-Anwendung

Das vorherigen Lernprogramm in Einzelschritten durchlaufen die Entwicklungsdatenbank in der produktionsumgebung kopieren, jedoch nicht untersuchen die datengesteuerte Anwendung bereitzustellen. Zu diesem Zeitpunkt wird die produktionsumgebung enthält die Datenbank, aber die Version der Anwendung überprüft Buch mit statischen Reviews verwendet. Wir müssen die neue, datengesteuerte Anwendung auf dem Produktionsserver sowie die aktualisierten Konfigurationsinformationen bereitstellen.

Nehmen Sie einen Moment Zeit, um die Anwendung von datengesteuerten aus der Entwicklungsumgebung bis hin zur Produktion bereitzustellen. Dieser Prozess wurde in den vorherigen Lernprogrammen in ausführlich beschrieben. Aufzufrischen, finden Sie unter der *Bereitstellung Ihrer Website mithilfe von FTP-Client* oder *Bereitstellen Ihrer Website mit Visual Studio* Lernprogramme. Sie müssen sicherstellen, dass die Verbindungszeichenfolge für die Produktion wird verwendet, in der produktionsumgebung, d. einen alternativen h. `Web.config` Datei muss bereitgestellt werden. Insbesondere dies geändert `Web.config` Datei "s" `<connectionStrings>` Element muss die Verbindungszeichenfolge für die Produktion enthalten und sollte etwa wie folgt aussehen:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

Beachten Sie, die die Verbindungszeichenfolge in der `<connectionStrings>` Element den gleichen Namen (`ReviewsConnectionString`), aber enthält nun die Verbindungszeichenfolge für die Produktion anstelle einer Verbindungszeichenfolge für die Entwicklung.

Wenn Sie eine weitere formalisierter Bereitstellungsworkflow verfügen, manuell ändern Sie entweder die `Web.config` Datei, die Verbindungszeichenfolge für die Produktion zu verwenden, vor der Bereitstellung (Denken Sie daran, diese wieder in die Verbindungszeichenfolge für die Entwicklung mit wiederherstellen im Anschluss daran) oder verwalten einen separaten `Web.config` Datei mit den Konfigurationsinformationen Produktions-Umgebung, die im Rahmen des Bereitstellungsprozesses in die produktionsumgebung hochgeladen ruft.

> [!NOTE]
> Wenn Sie versehentlich Bereitstellen einer `Web.config` Datei, die Verbindungszeichenfolge für die Entwicklung enthält, und es ein Fehler sein wird, wenn die Anwendung in Produktion versucht, mit der Datenbank herstellen. Dieser Fehler als Manifeste eine `SqlException` mit einer Meldung meldet, dass der Server nicht gefunden wurde oder nicht zugegriffen werden konnte.


Sobald der Standort bis hin zur Produktion bereitgestellt wurde, finden Sie auf der Produktionsstandort über Ihren Browser. Sie finden Sie unter, und genießen Sie die gleiche benutzererfahrung als wenn die datengesteuerte Anwendung lokal ausgeführt. Natürlich beim Besuch der Website für die Produktion die Website durch die Produktions-Datenbankserver mit Strom versorgt während die Datenbank Zugriff auf die Website in der Entwicklungsumgebung in der Entwicklung verwendet werden. Abbildung 3 zeigt die *Schulen selbst ASP.NET 3.5 in 24 Stunden* Seite von der Website in der produktionsumgebung (Beachten Sie die URL in die Adressleiste des Browsers s) "Überprüfen".


[![Datengesteuerte Anwendung ist jetzt verfügbar in Produktion!](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**Abbildung 3**: The Data-Driven-Anwendung ist jetzt verfügbar in Produktion! ([Klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Das Speichern von Verbindungszeichenfolgen in eine Separate Konfigurationsdatei

Ein gängiges Verfahren zum Verwalten von separaten Konfigurationsinformationen für die Entwicklung und Produktion Umgebungen ist, haben Sie zwei Versionen der `Web.config`: eine für die Entwicklungsumgebung und eine für die Produktion. Zum Zeitpunkt der Bereitstellung der entsprechenden `Web.config` Version in der produktionsumgebung kopiert werden. Dieser Vorgang würde idealerweise als Teil des Workflows für die Bereitstellung automatisiert werden.

Anstelle von zwei separaten verwalten `Web.config` Dateien, die Sie ausführen, optional können, geben Sie eine detailliertere Unterschiede. Die Elemente, aus denen die `Web.config` Datei kann definiert werden, in eine externe Konfigurationsdateien, klicken Sie dann auf die verweist, die `Web.config` Datei. Kurz gesagt können Sie haben eine `Web.config` -Konfigurationsdatei für beide Umgebungen, die eine Datei databaseConnectionStrings.config verweist auf die die Verbindungszeichenfolgen enthalten, würde von der Anwendung verwendet und würde für jede Umgebung eindeutig sein. Suchen, die durch die Trennung der unterschiedlichen Konfiguration in separate Dateien einen besseren Überblick über Ihre bereitstellt und einfachere `Web.config` Datei und vieles mehr deutlich erläutert die Konfigurationsunterschiede zwischen der Entwicklung und Produktion.

Um dieses Verfahren verwenden, zu starten, indem Sie einen neuen Ordner erstellen, in der Webanwendung mit dem Namen `ConfigSections`. Fügen Sie anschließend zwei Dateien in diesen neuen Ordner mit dem Namen databaseConnectionStrings.dev.config und databaseConnectionStrings.production.config. Kopieren Sie anschließend die `<connectionStrings>` Element aus `Web.config` in den Dateien databaseConnectionStrings.dev.config und databaseConnectionStrings.production.config und ändern Sie die Verbindungszeichenfolge in der databaseConnectionStrings.production.config Datei, sodass sie die Verbindungszeichenfolge für die Produktion gibt. Beispielsweise sollte die Datei databaseConnectionStrings.dev.config enthalten nur die `<connectionStrings>` Element mit einer Verbindungszeichenfolge, die die Entwicklungsdatenbank verweist:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

Auf ähnliche Weise die databaseConnectionStrings.production.config Datei dürfen nur ein `<connectionStrings>` -Element, aber eine, die Verbindungszeichenfolge für die Produktion verfügt.

Erstellen Sie eine Kopie der Datei databaseConnectionStrings.dev.config, und nennen Sie sie databaseConnectionStrings.config.

> [!NOTE]
> Sie können Namen die Konfigurationsdatei einen anderen Wert als databaseConnectionStrings.config, wenn Sie d, wie z. B. `connectionStrings.config` oder `dbInfo.config`. Allerdings werden Sie sicher, dass der name der Datei mit einem `.config` Erweiterung als `.config` Dateien, werden standardmäßig nicht verarbeitet werden vom Modul ASP.NET. Wenn Sie die Datei einen anderen Namen, z. B. `connectionStrings.txt`, zeigen Sie ein Benutzer konnte ihren Browser zu [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) und zeigen Sie den Inhalt der Datei!


An diesem Punkt der `ConfigSections` Ordner sollte drei Dateien (siehe Abbildung 4) enthalten. Die databaseConnectionStrings.dev.config und databaseConnectionStrings.production.config-Dateien enthalten die Verbindungszeichenfolgen für die Entwicklung und Produktion Umgebungen bzw. aus. Die databaseConnectionStrings.config-Datei enthält die Verbindungszeichenfolgen-Informationen, die von der Webanwendung zur Laufzeit verwendet werden. Daher sollte die Datei databaseConnectionStrings.config identisch mit der databaseConnectionStrings.dev.config-Datei in der Entwicklungsumgebung sein, während für Produktions-databaseConnectionStrings.config-Datei identisch mit sollten databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**Abbildung 4**: "configSections" ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


Jetzt müssen wir weisen `Web.config` die databaseConnectionStrings.config-Datei für die Verbindung Zeichenfolgenspeicher zu verwenden. Open `Web.config` und Ersetzen Sie die vorhandene `<connectionStrings>` Element durch Folgendes:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

Die `configSource` Attribut gibt an, relativ zu einem physischen Pfad der `Web.config` Datei. Wenn die externe `.config` Datei befindet sich im selben Verzeichnis wie `Web.config` legen Sie dieses Attribut auf den Dateinamen, der die `.config` Datei. Wenn sie s in einem Unterverzeichnis wie bei databaseConnectionStrings.config, der Fall ist, geben Sie den Unterordner mit einem umgekehrten Schrägstrich zur Begrenzung von Ordner- und Namen, z. B. ConfigSections\databaseConnectionStrings.config.

Dank dieser Änderung kann die Entwicklungs- und produktionsumgebungen Umgebungen enthält denselben `Web.config` Datei. Jetzt ist der einzige Unterschied die databaseConnectionStrings.config-Datei. Kopieren Sie die Datei databaseConnectionStrings.production.config bis hin zur Produktion, und benennen Sie sie in databaseConnectionStrings.config. Wenn in der Zukunft Änderungen an der Verbindungszeichenfolge für die Produktion stehen müssen Sie nehmen sie an der databaseConnectionStrings.production.config-Datei, und klicken Sie dann diese Datei hochladen, in die Produktion databaseConnectionStrings.config umbenennen.

> [!NOTE]
> Sie können angeben, die Informationen für alle `Web.config` Element in einer separaten Datei und verwenden Sie die `configSource` Attribut, um auf diese Datei heraus verweisen `Web.config`.


## <a name="summary"></a>Zusammenfassung

Datengesteuerte Anwendungen verwenden in der Regel verschiedene Datenbanken in der Entwicklungs- und produktionsumgebungen. Folglich müssen die Datenbank-Verbindungszeichenfolgen, die in der Konfiguration der Webanwendung s gespeichert pro Umgebung eindeutig sein. In diesem Lernprogramm erläutert wir, wie die Datenbankverbindungszeichenfolge Produktions- und Methoden zum Verwalten der eindeutigen Verbindungszeichenfolgen-Informationen in den beiden Umgebungen bestimmt.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Verbindungszeichenfolgen und Konfigurationsdateien](https://msdn.microsoft.com/library/ms254494.aspx)
- [Datenbankkonfiguration Zeichenfolgen Informationen @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Verschieben Sie die Datei "Web.config" mit Standardeinstellungen](http://www.asp101.com/tips/index.asp?id=154)
- [Technische Dokumentation für die &lt;ConnectionStrings&gt; Element](https://msdn.microsoft.com/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[Zurück](deploying-a-database-vb.md)
[Weiter](configuring-a-website-that-uses-application-services-vb.md)
