---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET Hostingoptionen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET-Webanwendungen in der Regel dienen, erstellt, und in eine lokale Entwicklungsumgebung getestet und für eine Produktions-o-Umgebung bereitgestellt werden müssen...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 12d4e0e4332cf611e304799155b45f5d668da8f6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804186"
---
<a name="aspnet-hosting-options-c"></a>Optionen zum Hosten von ASP.NET (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> ASP.NET-Webanwendungen sind in der Regel entworfen, erstellt und getestet werden, in einer lokalen Entwicklungsumgebung und müssen in einer produktionsumgebung bereitgestellt werden, sobald sie für die Veröffentlichung bereit ist. Dieses Lernprogramm bietet einen Überblick über den Bereitstellungsprozess und dient als Einführung in dieser tutorialreihe.


## <a name="introduction"></a>Einführung

Webanwendungen werden in der Regel entworfen, erstellt und getestet werden, in einer Entwicklungsumgebung, die nur für den Programmierern, die auf der Website zugegriffen werden kann. Sobald die Anwendung veröffentlicht werden bereit ist, wird es in einer produktionsumgebung verschoben, in denen die Website kann von jedermann im Internet zugegriffen werden. Dieses Verfahren der Bereitstellung führt eine Reihe von Herausforderungen:

- Eine produktionsumgebung muss vorhanden und ordnungsgemäß eingerichtet werden, bevor eine ASP.NET-Anwendung bereitgestellt werden kann; Darüber hinaus muss die produktionsumgebung mit den neuesten Sicherheitspatches auf dem neuesten Stand gehalten werden.
- Der richtige Satz von Markup-Dateien, Codedateien und unterstützenden Dateien muss in der produktionsumgebung in der Entwicklungsumgebung kopiert werden. Für datengesteuerte Anwendungen kann dies erfordert, kopieren das Datenbankschema und/oder Daten sowie.
- Es kann Konfigurationsunterschiede zwischen den beiden Umgebungen vorhanden sein. Connection String oder e-Mail-Adresse verwendeten Datenbankservers in der Entwicklungsumgebung werden wahrscheinlich anders als die produktionsumgebung. Das Verhalten der Anwendung kann, in der Umgebung abhängen. Beispielsweise tritt ein Fehler bei der Entwicklung können die Fehlerdetails des auf dem Bildschirm angezeigt werden, jedoch tritt ein Fehler in der Produktion, sollte stattdessen eine benutzerfreundliche Fehlermeldung-Seite angezeigt werden, und die Fehlerdetails per e-Mail gesendet, die Entwickler.

Auslagerung der auf die erste Herausforderung - einrichten und Verwalten einer produktionsumgebung - weitergegeben werden viele Personen und Unternehmen ihre produktionsumgebungen auf *web Hostinganbieter*. Ein Web-hosting-Anbieter ist ein Unternehmen, das die produktionsumgebung in Ihrem Auftrag verwaltet. Es gibt unzählige Web Hosten von Anbietern, jeweils unterschiedliche Preise und Servicelevels. finden Sie im Abschnitt "Suche nach einer Webhostinganbieter" Tipps zum Suchen eines solchen Diensts Anbieters.

Dies ist der erste in einer Reihe von Tutorials, in denen die Schritte zum Bereitstellen einer ASP.NET-Webanwendung in einer produktionsumgebung bereit, die von einem Webhostinganbieter verwaltet betrachten. Im Verlauf des in diesen Tutorials wird untersucht:

- Welche Dateien müssen auf dem Webhostinganbieter bereitgestellt werden.
- Tools für den Bereitstellungsprozess zu optimieren.
- Informationen zum Bereitstellen einer Datenbank.
- Tipps für die Bereitstellung einer Datenbank mit [der Anbieter für SQL-basierten Mitgliedschaften und Rollen](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), zusammen mit den Möglichkeiten, um das Website-Verwaltungstool in einer produktionsumgebung zu imitieren.
- Strategien zum Aktualisieren von der Datenbank in der Produktion nahtlos mit den Änderungen, die während der Entwicklung.
- Techniken zum Protokollieren von Fehlern, die für Produktions- und die Möglichkeiten, um Entwickler zu benachrichtigen, tritt ein Fehler auftreten.

Diese Lernprogramme sind präzise und bieten schrittweise Anleitungen mit zahlreichen Screenshots, die Sie durch den Prozess geführt, visuell ausgerichtet. In diesem ersten Tutorial bietet eine Übersicht über den Bereitstellungsprozess für ASP.NET und Ratschläge auf einen Webhostinganbieter suchen. Fangen wir an!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Eine Übersicht über den Bereitstellungsprozess für ASP.NET

Kurz gesagt, umfasst das Bereitstellen einer ASP.NET-Anwendung die folgenden drei Schritte aus:

1. Konfigurieren Sie die Webanwendung, die Webserver und die Datenbank, in der produktionsumgebung.
2. Synchronisieren Sie die ASP.NET-Seiten, Codedateien, die Assemblys in der `Bin` Ordner und zugehörigen HTML-Unterstützungsdateien, wie CSS- und JavaScript-Dateien.
3. Die Datenbankschema und Daten zu synchronisieren.

Die Konfigurationsinformationen für eine Webanwendung befindet sich normalerweise die `Web.config` Datei, und enthält Datenbank-Verbindungszeichenfolgen, Fehlerbehandlung Kriterien, die die URL-Umschreibung, Regeln und Informationen für e-Mail-Server. Häufig sind diese Informationen für eine Anwendung in der Entwicklung im Vergleich zu der gleichen Anwendung in der Produktion unterschiedlich. Beispielsweise empfiehlt sich beim Entwickeln einer Anwendung es, eine Entwicklungsdatenbank verwenden, sodass Sie nicht für die Produktionsdatenbank testen. Daher unterscheiden sich die Datenbank-Verbindungszeichenfolgen in der Regel zwischen Entwicklungs- und produktionsumgebungen Anwendungen. Aufgrund dieser Unterschiede auch Teil der Bereitstellung von Änderungen an der Webanwendung Konfigurationsinformationen.

Zusätzlich zu den konfigurationsänderungen für Web-Anwendung möglicherweise Schritt 1-Konfiguration für den Webserver und die Datenbank auch zur Folge. Z. B. wenn eine ASP.NET-Seite erstellt oder löscht Dateien aus einem Verzeichnis auf dem Webserver muss die Webserver konfiguriert werden, um diese dateisystemänderungen zuzulassen. Auf ähnliche Weise möglicherweise-Berechtigung oder die Authentication-Einstellungen, die in der Datenbank vorgenommen werden müssen.


Schritt 2 umfasst den Satz von essential ASP.NET-Seiten und unterstützenden Dateien zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen zu synchronisieren. Der bestimmten Satz von ASP zu gewährleisten. NET-bezogene Dateien, die zwischen den beiden Umgebungen synchronisiert werden müssen, hängt von den Typ des Projekts, die Sie in Visual Studio erstellt und ist der Diskussion im nächsten Tutorial [ *bestimmen was Dateien müssen bereitgestellt werden,*](determining-what-files-need-to-be-deployed-cs.md). Die dritte und vierte Tutorials - [ *Bereitstellen Ihrer Website mithilfe von FTP* ](deploying-your-site-using-an-ftp-client-cs.md) und [ *Bereitstellen Ihrer Website mit Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -überprüfen verschiedene Tools und Techniken für die Synchronisierung dieser Dateien.

Beim Erstellen von datengesteuerten Anwendungen gibt es in der Regel zwei Datenbanken, die verwendet wird: eine für Entwicklung und eine für die Produktion. Während der Entwicklung das Schema der Entwicklungsdatenbank kann geändert werden, um neue Tabellen, Spalten, gespeicherten Prozeduren und Trigger enthalten und kann zum Entfernen oder Umbenennen vorhandener Datenbankobjekte geändert werden. Zwischen der Zeit, die diese Änderungen vorgenommen werden und die Zeit, die die Anwendung in der produktionsumgebung bereitgestellt wird, sind die Datenbanken für Entwicklungs- und produktionsumgebungen nicht synchronisiert. Dieser Asynchronität muss während des Bereitstellungsprozesses korrigiert werden. Diese Herausforderungen werden in zukünftigen Lernprogrammen untersucht werden.

## <a name="finding-a-web-host-provider"></a>Suchen nach einem Webhostinganbieter

ASP.NET-Anwendungen können auf einem beliebigen Webserver bereitgestellt werden, mit dem .NET Framework und Internet Information Services (IIS) installiert. Sie können eine Website von Ihrem Computer, hosten, vorausgesetzt, dass Sie eine Breitbandverbindung mit dem Internet und dem Wissen den Router für das Zulassen von eingehenden webanforderungen konfigurieren konnten. Sie können auch einen Standort auf einem Computer in einem Intranet hosten, wie viele Unternehmen der Fall ist. Der Schwerpunkt dieser Lernprogramme, hostet jedoch Ihrer Website mit einem Webhostinganbieter.

> [!NOTE]
> [IIS](https://www.iis.net/) wird von Microsoft für Unternehmen konzipierten Webserver. Es wird mit den nicht-Home-Editionen von Windows, z. B. Windows Server 2008 und bestimmte Versionen von Windows Vista ausgeliefert. Sie müssen sich nicht zum Installieren von IIS zur Bereitstellung von ASP.NET-Anwendungen in einer Entwicklungsumgebung wie Visual Studio den ASP.NET Development Web Server enthält. Allerdings wird der ASP.NET Development Web Server akzeptiert nur lokale Verbindungen und kann daher nicht in einer produktionsumgebung verwendet werden.


Bevor Sie Ihre Website in einem Webhostinganbieter bereitstellen können, müssen Sie zuerst was Unternehmen, Geschäfte zu tätigen entscheiden. Es gibt unzählige Webhostingunternehmen im Marketplace. eine Suche nach "Webhostingunternehmen" gibt mehr als fünf Millionen Ergebnisse zurück. Wie finden Sie die, die für Sie geeignet ist? Ihre bevorzugte Suchmaschine ist ein guter Ausgangspunkt, wie z. B. sind [TopHosts](http://www.tophosts.com/) und [HostCritique](http://www.hostcritique.net/), die verglichen werden soll, und vergleichen Sie verschiedene hosting von Diensten. Ich Rate auch Fragen Ihren Kollegen und Kollegen für Empfehlungen; Sie können auch Empfehlungen an Stellen die [Open Forum hosten](https://forums.asp.net/158.aspx) hier bei der [ASP.NET-Foren](https://forums.asp.net/).

Webhostingunternehmen in der Regel freigegebenen hostingpläne und spezielle hostingpläne. Mit gemeinsam eine einzelne Web Server-Hosts Dutzende, wenn nicht gar Hunderte von anderen Websites zu hosten. Dedizierte Hosting leasen Sie einen Computer aus dem Unternehmen, das Ihre Website und Ihre Website ausschließlich dient. Ein freigegebener hostingplan kann es sich um Unterstützung für ASP.NET Webseiten, die Möglichkeit zum Arbeiten mit Microsoft Access-Datenbanken, 5 GB Speicherplatz auf dem Datenträger und 100 GB an Bandbreite monatlich für den Datenverkehr für 9,95 US-Dollar pro Monat enthalten. Einen anderen freigegebenen hosting-Plan möglicherweise Unterstützung für ASP.NET-Seiten, Zugriff auf Microsoft SQL Server 2008-Datenbankserver, 10 GB Speicherplatz und 250 GB monatliche Bandbreite für den Datenverkehr für 19,95 $ pro Monat enthalten. Dedizierte hostingpläne sind in der Regel mehr Aufwand, Kosten mehrere Hundert Dollar pro Monat, aber bieten eine bessere Leistung und mehr Kontrolle als freigegebene Optionen hosten. Welchen Plan, die Sie auswählen, die von Ihr Budget abhängig ist, wie viel Datenverkehr Ihrer Website empfängt und die Funktionen, die Sie erwarten Sie benötigen.

Zwei wichtige Überlegungen beim Auswählen einer Webhostinganbieter sind Kundendienst und Quality of Service. Wenn Sie eine Frage oder ein Konfigurationsproblem haben, wie lange dauert es über das Problem des Webhosts Helpdesk senden, bis Sie eine Antwort erhalten? Wie reliable Services des Unternehmens sind? Verfügen sie häufig mit Ausfällen der Datenbank aus? Wie oft wechseln ihre e-Mail-Server den Offlinemodus? Sie können immer bitten, ein Unternehmen enthalten Details zu ihrer Verfügbarkeit und Informationen über die Customer-Service-Richtlinie, aber mehr Sicherheit besteht darin, das Feedback der aktuellen und früheren Kunden, Anfragen, Sie über den online-Foren, Newsgroups und Mailinglisten .

> [!NOTE]
> Einige Webhostingunternehmen konzentrieren Sie sich ihr Unternehmen auf eine bestimmte Technologie-Stapel, wie z. B. .NET oder [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** Inux, **ein** Pache, **M** ySQL, und **P** HP), also stellen Sie sicher, dass das Unternehmen, die Sie auswählen, ASP.NET-Anwendungen hostet. Außerdem stellen Sie sicher, dass sie die Version von ASP.NET, die Sie verwenden unterstützen, um Ihre Anwendung zu erstellen. Und wenn Sie eine datengesteuerte Anwendung erstellen, stellen Sie sicher, dass der Web-Host bietet, den gleichen Datenbankserver und die Version, die Sie verwenden.


## <a name="summary"></a>Zusammenfassung

ASP.NET Web-Anwendungen werden in der Regel entworfen, erstellt und in einer lokalen Entwicklungsumgebung getestet. Nachdem Sie eine Version für die Veröffentlichung bereit ist, wird es in einer produktionsumgebung verschoben werden. Es ist, zwar möglich, Hosten von ASP.NET-Websites auf Ihrem eigenen Computer oder auf Servern in Ihrem Unternehmen wählen Sie zum Auslagern, deren hosting auf einen Webhostinganbieter viele Unternehmen und Personen.

Dieser tutorialreihe werden die Schritte zum Bereitstellen einer ASP.NET-Anwendung auf einen Webhostinganbieter, häufig auftretende Probleme zu untersuchen. In diesem Tutorial eine allgemeine Übersicht über den Bereitstellungsprozess von ASP.NET bereitgestellt und Tipps für die Suche nach einer geeigneten Webhostinganbieter hat. Im nächste Tutorial untersucht davon ab, was von ASP.NET in Verbindung stehenden Dateien, in der produktionsumgebung kopiert werden sollen, wenn Sie Ihre Website bereitstellen.

Viel Spaß beim Programmieren!

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Nächste](determining-what-files-need-to-be-deployed-cs.md)
