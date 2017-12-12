---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET Hostingoptionen (c#) | Microsoft Docs
author: rick-anderson
description: "ASP.NET-Webanwendungen in der Regel dienen, erstellt, und in einer lokalen Entwicklungsumgebung getestet und in einer produktionsumgebung Umgebung o bereitgestellt werden müssen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 66431cadac6011bbf247b24a08b3aacec928a715
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-hosting-options-c"></a>ASP.NET Hostingoptionen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> ASP.NET-Webanwendungen sind in der Regel entworfen, erstellt und getestet, die in einer lokalen Entwicklungsumgebung und müssen in einer produktiven Umgebung bereitgestellt werden, sobald er zur Freigabe bereit ist. Dieses Lernprogramm bietet einen allgemeinen Überblick über den Bereitstellungsprozess und dient als eine Einführung in dieser Reihe von Lernprogrammen.


## <a name="introduction"></a>Einführung

Webanwendungen werden in der Regel entworfen, erstellt und getestet werden, in einer Entwicklungsumgebung, die nur für die Programmierer arbeiten auf der Website verfügbar ist. Sobald die Anwendung freigegeben ist, wird es in einer produktiven Umgebung verschoben, in dem die Website kann von jedermann im Internet zugegriffen werden. Der Bereitstellungsprozess führt eine Reihe von Herausforderungen:

- Eine produktiven Umgebung muss vorhanden und ordnungsgemäß eingerichtet werden, bevor eine ASP.NET-Anwendung bereitgestellt werden kann. Darüber hinaus muss die produktionsumgebung über die aktuellsten Sicherheitspatches auf dem neuesten Stand gehalten werden.
- Der richtige Satz Markupdateien, Codedateien und Unterstützungsdateien muss in der Entwicklungsumgebung in die produktionsumgebung kopiert werden. Für datengesteuerte Anwendungen kann dies erfordert, kopieren das Datenbankschema und/oder Daten ebenfalls.
- Es kann Konfigurationsunterschiede zwischen den beiden Umgebungen vorhanden sein. Die Verbindung Zeichenfolgen- oder e-Mail-Datenbankserver in der Entwicklungsumgebung verwendet werden wahrscheinlich anders als die produktionsumgebung. Dazu kann das Verhalten der Anwendung für die Umgebung abhängen. Beispielsweise tritt ein Fehler bei der Entwicklung können Details der Fehler auf dem Bildschirm angezeigt werden, jedoch tritt ein Fehler in der Produktion, sollte stattdessen eine benutzerfreundliche Fehlerseite angezeigt werden, und die Fehlerdetails per e-Mail an die Entwickler.

Um die erste Abfrage angeboten - Einrichtung und Wartung einer produktiven Umgebung - empfiehlt viele Einzelpersonen und Unternehmen Auslagern produktionsumgebungen zu *web Hostinganbieter*. Ein Web-hosting-Anbieter ist ein Unternehmen, das die produktionsumgebung in Ihrem Auftrag verwaltet. Es gibt unzählige Web Hosten von Anbietern, jeweils mit unterschiedlichen Preise und Dienststufen. finden Sie im Abschnitt "Suchen nach einem Webhostinganbieter" Tipps zum Suchen von solchen eines Dienstanbieters.

Dies ist die erste Aufgabe in einer Reihe von Lernprogrammen, die die Schritte zum Bereitstellen einer ASP.NET-Webanwendung in einer produktionsumgebung, die von einem Webhostinganbieter verwaltet betrachten. Im Verlauf der Ausführung dieser Lernprogramme untersuchen wir:

- Welche Dateien müssen auf dem Webhostinganbieter bereitgestellt werden.
- Tools für die Optimierung des Bereitstellungsprozess.
- Zum Bereitstellen einer Datenbank.
- Tipps zum Bereitstellen einer Datenbank, die verwendet [der Anbieter für SQL-basierten Mitgliedschaft und Rollen](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), zusammen mit den Möglichkeiten Websiteverwaltungs-Tool in einer produktionsumgebung zu imitieren.
- Strategien für die Aktualisierung reibungslos mit den Änderungen, die während der Entwicklung der Datenbank in der Produktion.
- Techniken zum Protokollieren von Fehlern, die auftreten, auf die Produktion und Methoden zum Entwickler zu benachrichtigen, wenn ein Fehler auftritt.

Diese Lernprogramme sind darauf ausgerichtet, präziser sein und schrittweise Anweisungen mit Screenshots, Sie durch den Prozess durchlaufen, visuell bereitzustellen. In diesem ersten Lernprogramm bietet einen Überblick über den Bereitstellungsprozess für ASP.NET und Hinweise zu erhalten, auf einen Webhostinganbieter suchen. Fangen wir an!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Eine Übersicht über den Bereitstellungsprozess für ASP.NET

Kurz gesagt, umfasst die Bereitstellung einer ASP.NET-Anwendung die folgenden drei Schritte:

1. Konfigurieren Sie die Webanwendung, Webserver und Datenbank in der produktionsumgebung ein.
2. Synchronisieren Sie die ASP.NET-Seiten, Codedateien, die Assemblys in der `Bin` Ordner und zugehörigen HTML-Unterstützungsdateien, wie CSS und JavaScript-Dateien.
3. Synchronisieren Sie das Datenbankschema und/oder Daten.

Die Konfigurationsinformationen für eine Webanwendung befindet sich in der Regel der `Web.config` Datei und schließt Datenbankverbindungszeichenfolgen, Fehlerbehandlung Kriterien Regeln von URLs und E-mail-Serverinformationen. Häufig sind diese Informationen für eine Anwendung in der Entwicklung im Vergleich zu der gleichen Anwendung in der Produktion unterschiedlich. Beispielsweise sollte beim Entwickeln einer Anwendung eine Entwicklungsdatenbank verwenden, sodass Sie nicht gegen die Produktionsdatenbank testen. Folglich unterscheiden sich die Datenbank-Verbindungszeichenfolgen in der Regel zwischen Entwicklungs- und produktionsumgebungen Anwendungen. Aufgrund dieser Unterschiede auch Teil der Bereitstellung Änderungen an die Webanwendung-Konfigurationsinformationen.

Zusätzlich zu den konfigurationsänderungen für Web-Anwendung möglicherweise in Schritt 1 auch Konfiguration für die Webserver und Datenbank gelten. Z. B. wenn eine ASP.NET-Seite erstellt oder löscht Dateien aus einem Verzeichnis auf dem Webserver muss der Webserver konfiguriert werden, um diese dateisystemänderungen zuzulassen. Möglicherweise gibt es ebenso-Berechtigung oder die Authentication-Einstellungen, die an der Datenbank vorgenommen werden müssen.


Schritt 2 umfasst den Satz von wesentlichen ASP.NET-Seiten und Unterstützungsdateien zwischen der Entwicklungs- und produktionsumgebungen zu synchronisieren. Der bestimmten Satz von ASP. NET-bezogene Dateien, zwischen den beiden Umgebungen synchronisiert werden müssen, hängt vom Typ des Projekts, die Sie in Visual Studio erstellt und ist die Erläuterung in den nächsten Lernprogrammen [ *bestimmen was-Dateien müssen bereitgestellt werden,*](determining-what-files-need-to-be-deployed-cs.md). Die dritte und vierte Lernprogramme - [ *Bereitstellen Ihrer Website mithilfe von FTP* ](deploying-your-site-using-an-ftp-client-cs.md) und [ *Bereitstellen Ihrer Website mit Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -untersuchen andere Tools und Techniken zum Synchronisieren von Dateien.

Beim Erstellen eines datengesteuerten Anwendungen bestehen in der Regel zwei Datenbanken, die verwendet wird: eine für Entwicklung und eine für die Produktion. Während der Entwicklung der Entwicklungsdatenbank Schema möglicherweise geändert werden, um neue Tabellen, Spalten, gespeicherten Prozeduren und Triggern enthalten, oder zum Entfernen oder Umbenennen von vorhandenen Datenbankobjekte geändert werden kann. Zwischen dem Zeitpunkt, den diese Änderungen vorgenommen werden und die Zeit, die die Anwendung bis hin zur Produktion bereitgestellt wird, sind die Entwicklungs- und produktionsumgebungen Datenbanken nicht synchronisiert. Diese Asynchronie muss während des Bereitstellungsvorgangs behoben werden. Diese Probleme werden in zukünftigen Lernprogrammen untersucht werden.

## <a name="finding-a-web-host-provider"></a>Suchen nach einem Webhostinganbieter

ASP.NET-Anwendungen können auf einen beliebigen Webserver bereitgestellt werden, die .NET Framework und Internet Information Services (IIS) installiert wurde. Sie konnte einen Standort aus Ihrem PC hosten, vorausgesetzt, dass Sie zum Konfigurieren des Routers zum Zulassen von eingehenden webanforderungen eine Breitbandverbindung mit dem Internet und dem laufenden hatten. Sie können auch einen Standort von einem Computer in einem Intranet hosten, ebenso viele Unternehmen. Der Fokus mit diesen Lernprogrammen wird jedoch Ihre Website mit einem Webhostinganbieter gehostet werden.

> [!NOTE]
> [IIS](https://www.iis.net/) wird von Microsoft stammende Webserver für Enterprise-Grade. Es ist im Lieferumfang der nicht-Home Editions von Windows, z. B. Windows Server 2008 und bestimmten Editionen von Windows Vista enthalten. Sie müssen nicht zum Installieren von IIS zur Verarbeitung von ASP.NET-Anwendungen in einer Entwicklungsumgebung wie Visual Studio den ASP.NET Development Web Server enthält. Allerdings den ASP.NET Development Web Server akzeptiert nur lokale Verbindungen und kann daher nicht in einer produktiven Umgebung verwendet werden.


Bevor Sie Ihre Website auf einen Webhostinganbieter bereitstellen können, müssen Sie zuerst Unternehmen Geschäfte mit entscheiden. Es gibt unzählige Webhostingunternehmen im Marketplace. eine Suche nach "Webhostinganbieter" gibt mehr als fünf Millionen Ergebnisse zurück. Wie finden Sie die, die für Sie geeignet ist? Ihre bevorzugte Suchmaschine ist ein guter Ausgangspunkt, wie z. B. Websites sind [TopHosts](http://www.tophosts.com/) und [HostCritique](http://www.hostcritique.net/), dem vergleichen und vergleichen Sie verschiedene hosting-Dienste. Ich darauf hinzuweisen, bitten Ihren Kollegen und Kollegen für Empfehlungen; Sie können auch Fragen, um Empfehlungen zur der [öffnen Forum Hosting](https://forums.asp.net/158.aspx) hier auf die [ASP.NET Foren](https://forums.asp.net/).

Webhostingunternehmen in der Regel freigegebenen hosting Pläne und spezielle Hosten von Plänen. Gemeinsam mit einem einzelnen Server Webhosts Dutzende, wenn nicht gar Hunderte von anderen Websites hosten wird. Mit dedizierten hosting lease Sie einen Computer aus dem Unternehmen, das Ihre Website und Ihre Website allein dient. Eine freigegebene hostingplan gehören Unterstützung für die ASP.NET-Seiten, die Möglichkeit zur Arbeit mit Microsoft Access-Datenbanken, 5 GB Speicherplatz und 100 GB an Bandbreite beim monatlich ausgehenden Datenverkehr für 9.95 $ pro Monat. Eine andere freigegebene hostingplan gehören Unterstützung für die ASP.NET-Seiten, den Zugriff auf Microsoft SQL Server 2008-Datenbankserver, 10 GB Speicherplatz und 250 GB an Bandbreite beim monatlich ausgehenden Datenverkehr für 19,95 $ pro Monat. Dedizierte Pläne zum Hosten von Hosten in der Regel viel teurer, kostenaufwändiger mehrere Hundert Dollar pro Monat allerdings bieten eine bessere Leistung und mehr Kontrolle als freigegebene Optionen. Welchen Plan Sie wählen das Budget für die abhängt, wie viel Datenverkehr Ihrer Website empfängt und die Funktionen, die Sie erwarten Sie benötigen.

Zwei wichtige Aspekte bei der Auswahl einer Webhostinganbieter sind Kundendienst und Quality of Service. Wenn Sie eine Frage oder ein Konfigurationsproblem haben, wie lange dauert es über Ihr Problem auf dem Webhost Helpdesk übermitteln, bis Sie eine Antwort erhalten? Zuverlässigkeit des Unternehmens Dienste sind? Haben sie häufig Datenbank Ausfälle? Wie oft werden ihre e-Mail-Server offline geschaltet? Sie können ein Unternehmen enthalten Informationen zu ihren Betriebszeit sowie Informationen über die Customer-Service-Richtlinie immer nachfragen, aber mehr bombensichere besteht darin, das Feedback von Kunden über aktuelle und vergangene Anfragen, wozu Sie über den online-Foren, Newsgroups und e-Mails verwenden können sich.

> [!NOTE]
> Einige Webhostingunternehmen konzentrieren ihres Unternehmens auf eine bestimmte Technologie, wie z. B. .NET oder [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** Inux, **ein** Pache, **M** ySQL, und **P** HP), achten Sie darauf, dass das Unternehmen, die Auswahl von ASP.NET-Anwendungen hostet. Außerdem stellen Sie sicher, dass sie die Version von ASP.NET, die Sie verwenden unterstützen, um die Anwendung erstellen. Und wenn Sie eine datengesteuerte Anwendung erstellen, stellen Sie sicher, dass die Webhost bietet den gleichen Datenbankserver und die Version, die Sie verwenden.


## <a name="summary"></a>Zusammenfassung

ASP.NET-Webanwendungen sind in der Regel entworfen, erstellt und in einer lokalen Entwicklungsumgebung getestet. Sobald eine Version zur Freigabe bereit ist, wird es in einer produktiven Umgebung verschoben. Während es zum Hosten von ASP.NET-Websites auf Ihrem eigenen Computer oder auf Servern in Ihrem Unternehmen möglich ist, wählen viele Unternehmen und Personen überlassen, deren hosting auf einen Webhostinganbieter.

Diese Reihe von Lernprogrammen untersucht die Schritte zum Bereitstellen einer ASP.NET-Anwendung auf einen Webhostinganbieter gängige Probleme zu untersuchen. Dieses Lernprogramm angeboten einen allgemeinen Überblick über den Bereitstellungsprozess für ASP.NET und Tipps für die Suche nach einer geeigneten Webhostinganbieter gegeben. Prüft, dass der nächste Lernprogrammen was ASP.NET-Dateien in der produktionsumgebung kopiert werden, wenn Sie Ihre Website bereitstellen müssen.

Viel Spaß beim Programmieren!

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Nächste](determining-what-files-need-to-be-deployed-cs.md)
