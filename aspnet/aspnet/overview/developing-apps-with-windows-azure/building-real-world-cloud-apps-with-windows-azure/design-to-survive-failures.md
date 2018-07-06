---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Entwurf überstehen von Ausfällen (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 29a430a223fb62d9530ea00a60d458a6dbc5199a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820654"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Ausrichtung des Entwurfs auf das überstehen von Ausfällen (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Eines der Dinge denken Sie beim Erstellen jeder Art von Anwendung, aber vor allem eine, die in der Cloud ausgeführt wird, in denen viele Leute verwenden, müssen Sie die app entwerfen, damit es ordnungsgemäß Behandeln von Fehlern und auszahlen kann ist wie möglich ist. Wenn genügend Zeit, sind der Stand der Dinge, die in jeder Umgebung oder anderen Softwaresystemen schief gehen. Behandlung von Situationen verwendet, in der app bestimmt, wie verärgert Ihre Kunden erhalten und wie viel Zeit verbringen, analysieren und Beheben von Problemen mit Ihnen.

## <a name="types-of-failures"></a>Arten von Fehlern

Es gibt zwei grundlegende Kategorien von Fehlern, die Sie anders behandeln möchten:

- Vorübergehende, selbst heilen Fehler wie z. B. vorübergehende Probleme mit der Netzwerkkonnektivität.
- Diskutierte Fehler, die Eingriffe erfordern.

Für vorübergehende Fehler können Sie implementieren eine wiederholungsrichtlinie, um sicherzustellen, dass ein Großteil der Zeit, stellt die app schnell und automatisch wieder her. Ihre Kunden möglicherweise etwas längere Antwortzeit fest, aber andernfalls sie dadurch nicht beeinflusst. Wir zeigen einige Möglichkeiten, um diese Fehler in behandeln die [Kapitel behandeln vorübergehender Fehler](transient-fault-handling.md).

Für die Umwelt Fehler, können Sie implementieren, Überwachung und Protokollierung von Funktionen, die Sie sofort benachrichtigt, wenn Probleme auftreten, und das erleichtert die Analyse der Grundursache. Wir zeigen einige Möglichkeiten können Sie den Überblick über diese Arten von Fehlern in zu behalten die [Überwachung und Telemetrie Kapitel](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Fehler-Bereich

Sie müssen auch Fehler Bereich – überlegen, ob ein einzelner Computer betroffen ist, einen gesamten Dienst wie Storage, SQL-Datenbank oder einer gesamten Region.

![Fehler-Bereich](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Computerausfälle

Klicken Sie in Azure ein fehlerhaften Server wird automatisch durch eine neue ersetzt, und eine durchdachte Cloud-app wird über diese Art von Fehler schnell und automatisch wiederhergestellt. Zuvor betont wir die Vorteile der Skalierbarkeit von Ebenen statusfreie Web- und einfache Wiederherstellung nach einem fehlerhaften Server ist ein weiterer Vorteil von statusfreiheit. Einfache Wiederherstellung ist auch mit einer der Vorteile des Platform-as-a-Service (PaaS)-Funktionen wie SQL-Datenbank und Azure App Service-Web-Apps. Hardwareausfälle sind zwar selten, aber wenn die Zeitpunkte, dass diese Dienste werden automatisch behandelt; Sie müssen noch nicht zum Schreiben von Code zum Behandeln von Fehlern der Computer, wenn Sie einen dieser Dienste verwenden.

### <a name="service-failures"></a>Dienstfehler

Cloud-apps verwenden normalerweise mehrere Dienste. Z. B. die Fix It-app verwendet den SQL-Datenbank-Dienst, den Storage-Dienst, und die Web-app in Azure App Service bereitgestellt wird. Was wird Ihrer app tun, bei einem einer der Dienste, von denen Sie abhängen Ausfall? Für einige Fehler einen aussagekräftigen Dienst "Es tut uns leid, versuchen Sie es später noch Mal" Nachricht möglicherweise die beste möglich. In vielen Fällen können jedoch besser. Z. B. bei Ihrem Back-End-Datenspeicher nicht ausgeführt wird, können Sie Benutzereingaben akzeptieren, anzeigen "Ihre Anforderung wurde empfangen", und speichern die Eingabe, die an einer beliebigen Stelle else vorübergehend; während der Dienst Sie müssen in diesem Fall funktioniert die Eingabe abzurufen und zu verarbeiten.

Die [Warteschlange-orientierte Arbeit Muster](queue-centric-work-pattern.md) Kapitel zeigt eine Möglichkeit zum Behandeln dieses Szenarios. Die Fix It-app speichert Aufgaben in SQL-Datenbank, aber nicht sein muss, beenden Sie arbeiten, wenn Sie SQL-Datenbank nicht ausgeführt wird. In diesem Kapitel erfahren wir, wie Benutzereingaben für einen Vorgang in einer Warteschlange speichern und diese ein Arbeitsprozess mit die Warteschlange zu lesen und aktualisieren Sie den Vorgang. Wenn SQL nicht verfügbar ist, ist die Fähigkeit zum Erstellen der Fix It-Aufgaben nicht betroffen; der Arbeitsprozess kann warten, und neue Aufgaben zu verarbeiten, wenn der SQL-Datenbank verfügbar ist.

### <a name="region-failures"></a>Ausfälle von schreibregionen

Gesamte Regionen schlägt möglicherweise fehl. Eine Naturkatastrophe könnte ein Rechenzentrum zerstören, es möglicherweise durch eine Meteor vereinfacht zu erhalten, konnte die Zeile "Trunk" in das Datencenter von einem Farmer durch nichts mehr mit einer Tieflöffel usw. ausgeschnitten. Was tun Sie, wenn Ihre app im stricken Rechenzentrum gehostet wird? Es ist möglich, Ihre app in Azure einrichten, auf die in mehreren Regionen gleichzeitig ausgeführt werden, damit bei ein Notfall in einer, Sie weiterhin in einer anderen Region ausführen. Solche Fehler sind äußerst selten auftreten, und die meisten apps nicht über die Aufwand erforderlich, um sicherzustellen, dass durch Fehler dieser Art einen ununterbrochenen Dienst zu springen. Finden Sie im Abschnitt "Ressourcen" am Ende des Kapitels, für die Informationen dazu, wie Sie Ihre Apps auch über einen Fehler für die Region zur Verfügung.

Ein Ziel von Azure ist es, behandeln alle diese Arten von Fehlern, die viel einfacher, und sehen Sie einige Beispiele für Sie, wie wir, die in den folgenden Kapiteln abschneiden!.

## <a name="slas"></a>SLAs

Menschen hören oft Service Level Agreements (SLAs) in der Cloud-Umgebung. Dies sind im Grunde Zusagen, die Unternehmen zur wie zuverlässig der Dienst ist. Eine 99,9 % SLA bedeutet, dass Sie erwarten, den Dienst zu 99,9 % der Zeit ordnungsgemäß funktionieren. Dies ist ein recht häufig angegebener Wert für eine Vereinbarung zum SERVICELEVEL und hört sich wie eine sehr hohe Anzahl, aber nicht erzielt werden könnte, wie viel Zeit nach unten. tatsächlich Endergebnis 1 %. Hier ist eine Tabelle mit wie viel Ausfallzeit verschiedene SLA-Prozentsätze über einem Jahr, Monat und einer Woche beanspruchen.

![SLA-Tabelle](design-to-survive-failures/_static/image2.png)

Daher kann eine 99,9 % SLA bedeutet, dass Ihr Dienst 8,76 Stunden ein Jahr oder 43,2 Minuten pro Monat ausfallen. Das ist mehr außer Betrieb genommen, als die meisten Leute realisieren. So, als Entwickler möchten Sie bedenken, dass eine bestimmte Menge an außer Betrieb genommen werden kann und diese auf eine ordnungsgemäße Weise zu behandeln. An einem bestimmten Punkt wird von Personen Ihre app nutzen, werden, wird ein Dienst nicht verfügbar, und möchten Sie die negative Auswirkungen, die auf den Kunden zu minimieren.

Eine Sache, die Sie eine SLA kennen sollten ist, welcher Zeitrahmen auf verweist: setzt erhalten die Uhr wöchentlich, monatlich oder jährlich zurück? In Azure zurückgesetzt wir die Uhr handelt es sich besser für Sie eine jährliche SLA, da eine jährliche SLA schlechten Monaten ausblenden kann, indem Sie versetzen sie mit einer Reihe von guten Monaten jeden Monat.

Natürlich möchten wir immer so eine bessere Leistung als die Vereinbarung zum SERVICELEVEL tun; Sie werden in der Regel viel kleiner als die ausfallen. Die Zusicherung ist, wenn wir je nach unten für überschreitet den maximal zulässigen Wert außer Betrieb genommen können Sie Geld zurück anfordern können. Geldbeträge erhalten Sie wieder wahrscheinlich wäre nicht vollständig kompensiert Sie für die geschäftlichen Auswirkungen von den Überschuss außer Betrieb genommen, aber dieser Teil der Vereinbarung zum SERVICELEVEL fungiert als durchsetzungsrichtlinie und Ihnen mitteilt, dass wir es sehr ernst zu nehmen.

### <a name="composite-slas"></a>Zusammengesetzte SLAs

Wirkt sich kaum für jeden Dienst mit einer separaten SLA mehrere Dienste in einer app mit einer wichtig zu bedenken, wenn Sie sich die SLAs ansehen. Beispielsweise verwendet die Fix It-app an Azure App Service-Web-Apps, Azure Storage und SQL-Datenbank. Hier sind die SLA-Zahlen, ab dem Datum, an das diesem e-Book im Dezember 2013 geschrieben wird:

![SLA-Website, Speicher, SQL-Datenbank](design-to-survive-failures/_static/image3.png)

Was ist die maximale Ausfallzeit erwarten Sie für die app basierend auf diesen Dienst SLAs? Sie denken möglicherweise, dass es sich bei Ihrem Ausfallzeiten in diesem Fall entspricht der schlechtesten SLA-Prozentsatz oder 99,9 % wäre. Das wäre "true", wenn alle drei Dienste, die immer zur selben Zeit fehlgeschlagen ist, das jedoch nicht notwendigerweise was tatsächlich geschieht. Jeder Dienst kann zu unterschiedlichen Zeitpunkten unabhängig voneinander fehlschlagen, müssen Sie die zusammengesetzte Vereinbarung zum SERVICELEVEL zu berechnen, indem Sie die einzelnen SLA Zahlen multipliziert.

![Zusammengesetzte Vereinbarung zum SERVICELEVEL](design-to-survive-failures/_static/image4.png)

Damit Ihre app konnte Sie nicht nur 43,2 Minuten einen Monat, aber 3 Mal Menge, 108 Minuten pro Monat – und werden immer noch innerhalb der Grenzen des Azure-SLA.

Dieses Problem ist nicht eindeutig, in Azure. Stellen wir tatsächlich die beste Cloud SLAs alle Cloud-Diensts verfügbar, und müssen Sie ähnliche Probleme behandeln müssen, wenn Sie Cloud-Dienste des Anbieters verwenden. Was dies verdeutlicht, ist die Bedeutung der nachzudenken, wie Sie Ihre app um die unvermeidliche Dienstfehler ordnungsgemäß behandelt entwerfen können, da sie häufig genug Ihrer Kunden oder Nutzern beeinträchtigen können möglicherweise.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Cloud-SLAs, die im Vergleich zu Ausfallzeiten Leistungspaket für Unternehmenskunden

Personen sagen manchmal, "In meiner app Enterprise ich nie Probleme haben." Wenn Sie wie viel Ausfallzeit pro Monat Fragen haben sie tatsächlich, sie beispielsweise in der Regel "Passiert nun, es gelegentlich." Und wenn Sie wie oft der Fragen, geben sie, die "Manchmal wir zu sichern oder ein neue Server oder Update-Software installieren müssen." Natürlich zählt, die als außer Betrieb genommen. Die meisten Unternehmens-apps sind, außer sie sind besonders wichtige tatsächlich nach unten für mehr als die Zeitspanne, die durch SLAs Dienst zulässig. Aber wenn es den Server und Ihrer Infrastruktur und Sie verantwortlich dafür und Kontrolle darüber sind, Sie können Sie weniger Enthüllung zu Ausfallzeiten. In einer Cloudumgebung Sie abhängig von einer anderen Person sind, und Sie nicht wissen, was passiert, damit Sie tendenziell mehr sorgen sie erhalten möglicherweise.

Wenn ein Unternehmen erzielt, einen höheren Prozentsatz der Betriebszeit wird als Sie von einer SLA-Cloud erhalten, dazu diese Ausgaben viel Geld für Hardware. Ein Cloud-Dienst kann jedoch noch viel mehr für seine Dienste berechnet werden musste. Stattdessen nutzen Sie kostengünstigen Dienst und Ihre Software so entwerfen, dass die unvermeidliche Fehler lediglich minimalen Unterbrechungen für Ihre Kunden führen. Ihren Job als einen Cloud-app-Designer ist nicht so viele Fehler hinsichtlich der Katastrophe vermeiden zu vermeiden, und Sie dies durch die Konzentration auf Software, die nicht für Hardware. Während der Unternehmens-apps sich bemühen, durchschnittliche Zeit zwischen Ausfällen zu maximieren, sollen minimieren Sie die mittlere Zeit bis zur Wiederherstellung Cloud-apps.

### <a name="not-all-cloud-services-have-slas"></a>Nicht alle Cloud-Dienste verfügen SLAs

Denken Sie auch, dass nicht alle Cloud-Dienst sogar eine SLA hat. Wenn Ihre app einen Dienst mit keine Garantie für die Betriebszeit abhängig ist, können Sie ausfallen wesentlich länger, als Sie sich vorstellen können. Z. B. Wenn Sie sich auf Ihrer Website mit dem Anbieter sozialer Netzwerke wie Facebook oder Twitter aktivieren, überprüfen Sie mit dem Dienstanbieter, um herauszufinden, ob es eine SLA gibt und draußen solcher Routenname vorhanden ist dort kann. Aber wenn der Authentifizierungsdienst nicht erreichbar ist oder nicht die Anzahl von Anforderungen unterstützen, die Sie an sie übergeben, werden Ihre Kunden aus Ihrer app gesperrt. Sie können Tage oder länger ausfallen. Die Ersteller von eine neue app erwartet von mehreren hundert Millionen Downloads und hat eine Abhängigkeit auf Facebook-Authentifizierung – jedoch nicht sprechen Sie mit Facebook vor dem Wechsel von Live- und ermittelte zu spät, die gab es keine SLA für diesen Dienst.

### <a name="not-all-downtime-counts-toward-slas"></a>Nicht alle Ausfallzeiten eingerechnet SLAs

Einige Clouddienste möglicherweise absichtlich als Dienst verweigern, wenn Ihre app, die ihnen übermäßige verwendet. Dies wird als bezeichnet *Drosselung*. Wenn ein Dienst eine SLA verfügt, sollten sie die Bedingungen angeben, unter dem Sie möglicherweise gedrosselt werden, und Ihr app-Design sollte diese Bedingungen zu vermeiden und angemessen zu reagieren, die Drosselung, wenn es auftritt. Z. B. wenn Anforderungen an einen Dienst starten fehlschlägt, wenn Sie eine bestimmte Anzahl pro Sekunde überschreiten, möchten Sie sicherstellen, dass automatische Wiederholungen nicht so schnell weiter ausfallen, dass sie bewirken, die Drosselung dass, um den Vorgang fortzusetzen. Haben wir dann mehr zu sagen zur Drosselung in der [Kapitel behandeln vorübergehender Fehler](transient-fault-handling.md).

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel hat versucht, können Sie feststellen, warum eine reale Cloud-app wurde so konzipiert, dass Fehler ordnungsgemäß zu überstehen. Beginnend mit der [im nächsten Kapitel](monitoring-and-telemetry.md), die übrigen Muster in dieser Serie näher über einige Strategien, Sie dies können:

- Haben Sie gut [Überwachung und Telemetrie](monitoring-and-telemetry.md), damit Sie feststellen, schnell zu Fehlern, die Eingreifen erfordern, und Sie verfügen über ausreichend Informationen zur Problembehebung.
- [Behandeln von vorübergehenden Fehlern](transient-fault-handling.md) durch die Implementierung von Wiederholungslogik intelligent, damit Ihre app automatisch wiederhergestellt wird, wenn er kann und sendet eine Anfrage an [Circuit-Breaker](transient-fault-handling.md#circuitbreakers) Logik ist dies nicht möglich.
- Verwendung [verteilte Zwischenspeicherung](distributed-caching.md) um Durchsatz, Latenz und Verbindung Probleme mit Zugriff auf die Datenbank zu minimieren.
- Implementieren, die lose Kopplung, die über die [warteschlangenorientierte Muster](queue-centric-work-pattern.md), sodass Ihr app-Front-End fortgesetzt werden kann, funktioniert, wenn das Back-End ausgefallen ist.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in späteren Kapiteln in diesem e-Book und den folgenden Ressourcen.

Dokumentation:

- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Die FailSafe-Videoreihe Webseite-Version.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy.
- [Azure technische Dokumentation zur Geschäftskontinuität](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Whitepaper von Patrick Wickline, und Jason Roth.
- [Notfallwiederherstellung und Hochverfügbarkeit für Azure-Anwendungen](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Whitepaper von Hanu Kommalapati, Michael McKeown und Jason Roth.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Finden Sie unter Data Center-Bereitstellung mit mehreren Anweisungen, Circuit-Breaker-Muster.
- [Azure-Support - Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).
- [Die Geschäftskontinuität in Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentation zu SQL-Datenbank hohe Verfügbarkeit und notfallwiederherstellung Wiederherstellungsfunktionen.
- [Hohe Verfügbarkeit und Notfallwiederherstellung für SQLServer auf Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Teil 9-Reihe von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Episoden 1 und 8 wechsle ausführlich die Gründe für das Entwerfen von Cloud-apps Ausfälle überbrücken. Finden Sie auch zur nachverfolgung der Drosselung in Folge 2 49:57, die Diskussion von Schwachstellen und Fehlerzustände in Folge 2 56:05 ab und die Erläuterung von schutzschaltern in der Folge 3 beginnend 40:55 ab.
- [Erstellen von Big: Erfahrungen aus dem Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms spricht über Entwerfen für Fehler, und alles instrumentieren. Ähnlich wie die Failsafe-Serie, aber wird auf Weitere Gewusst-wie-Details.

> [!div class="step-by-step"]
> [Zurück](unstructured-blob-storage.md)
> [Weiter](monitoring-and-telemetry.md)
