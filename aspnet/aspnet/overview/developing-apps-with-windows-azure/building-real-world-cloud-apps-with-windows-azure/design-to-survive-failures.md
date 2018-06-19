---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Fehler (Building Real-World Cloud Apps with Azure) Überleben beim Entwurf | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 01883cb0be3e7c7b5dc8d32b784ccb3a28652f1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874108"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>Fehler (Building Real-World Cloud Apps with Azure) Überleben entwerfen
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Keines der Dinge zu bedenken, bei der Erstellung jeder Art von Anwendung, aber vor allem eine, die in der Cloud ausgeführt werden, in denen viele Leute verwenden, stehen Ihnen handelt es sich um die app entwerfen, damit es ordnungsgemäß Behandeln von Fehlern und weiterhin Wert liefern kann so weit wie möglich ist. Sofern genügend Zeit gegeben, setupbegrüßungsbildschirm Dinge in jeder Umgebung oder eine beliebige Softwaresystem schief geht. Behandlung von Situationen verwendet, in die app bestimmt, wie verärgert Ihre Kunden erhalten soll und wie viel Zeit aufwenden, analysieren und Beheben von Problemen stehen Ihnen.

## <a name="types-of-failures"></a>Fehlertypen

Es gibt zwei grundlegende Kategorien von Fehlern, die auf andere Weise behandeln möchten:

- Vorübergehend, Selbstkorrektur Ausfällen, z. B. vorübergehende Verbindungsprobleme.
- Beständig von Fehlern, die einen Eingriff erfordern.

Für vorübergehende Fehler können Sie implementieren eine wiederholungsrichtlinie, um sicherzustellen, dass die meisten der Zeit, die app schnell und automatisch wiederhergestellt wird. Ihre Kunden möglicherweise geringfügig längere Antwortzeit fest, aber ansonsten sie wirkt sich nicht auf. Wir zeigen einige Möglichkeiten, um diese Fehler im behandelt die [vorübergehender Fehler behandeln Kapitel](transient-fault-handling.md).

Für beständig Fehlern, können Sie die Überwachung und Protokollierung von Funktionen, die Sie sofort benachrichtigt, wenn Probleme auftreten, und erleichtert, die die Fehlerursachenanalyse implementieren. Wir zeigen einige Möglichkeiten zur Unterstützung von bleiben Sie auf diese Art von Fehlern in der [Kapitel Überwachung und Telemetrie](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Fehler-Bereich

Außerdem müssen Sie Fehler Bereich – überlegen, ob es sich bei ein einzelnen Computer betroffen ist, einen gesamten Dienst z. B. SQL-Datenbank, Speicher oder einer gesamten Region.

![Fehler-Bereich](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Da Computerfehler

In Azure ein fehlerhaften Server wird automatisch durch einen neuen ersetzt, und eine gut entworfene Cloud-app aus dieser Art des Fehlers wiederhergestellt wird, schnell und automatisch. Zuvor wir die Vorteile der eine zustandslose Webebene betont, und Ihnen eine einfache Wiederherstellung von einem ausgefallenen ist ein weiterer Vorteil der zustandsfreiheit. Einfache Wiederherstellung ist auch mit einer der Vorteile von Platform-as-a-Service (PaaS)-Features wie SQL-Datenbank und Azure App Service-Web-Apps. Hardwarefehler sind zwar selten, aber wenn die Zeitpunkte, dass diese Dienste automatisch handhaben; Sie müssen auch Code schreiben, um Computerfehler zu behandeln, wenn Sie einem dieser Dienste verwenden.

### <a name="service-failures"></a>Dienstfehler

Cloud-apps werden normalerweise mehrere Dienste verwenden. Beispielsweise die app zu beheben, verwendet den SQL-Datenbankdienst, der Speicherdienst und die Web-app in Azure App Service bereitgestellt wird. Was wird Ihre app tun, bei einem Ausfall einer der Dienste, richtet sich nach? Für einige Fehler einen aussagekräftigen service "leider, versuchen Sie es später" Nachricht möglicherweise am besten geeignet, die Sie ausführen können. In vielen Szenarien Sie können jedoch eine bessere Leistung. Z. B. bei der Back-End-Datenspeicher ausgefallen ist, können Sie Benutzereingaben akzeptieren, anzeigen "Ihre Anforderung wurde empfangen", und speichern die Eingabe, die an einem beliebigen Ort else vorübergehend; anschließend wieder, funktioniert der Dienst Sie müssen die Eingabe abzurufen und zu verarbeiten.

Die [Warteschlange anwendungsorientierte Arbeit Muster](queue-centric-work-pattern.md) Kapitel zeigt eine Möglichkeit, dieses Szenario zu behandeln. Die app zu beheben Aufgaben in SQL-Datenbank gespeichert, jedoch nicht zu beenden, zu arbeiten, wenn die SQL-Datenbank nicht ausgeführt wird. In diesem Kapitel erfahren wir, wie zum Speichern von Benutzereingaben für eine Aufgabe in einer Warteschlange, und ein Arbeitsprozess mit der die Warteschlange zu lesen und aktualisieren Sie den Vorgang. Wenn SQL nicht ausgeführt wird, ist die Fähigkeit zum Erstellen von Aufgaben korrigieren nicht betroffen; der Arbeitsprozess kann warten, und neue Aufgaben zu verarbeiten, wenn der SQL-Datenbank verfügbar ist.

### <a name="region-failures"></a>Fehler bei der Region

Gesamte Regionen schlägt möglicherweise fehl. Eine Naturkatastrophe könnte ein Rechenzentrum zerstören, es möglicherweise durch eine Meteor vereinfacht erhalten, konnte die Zeile "Trunk" in das Datencenter durch einen durch eine Kuh Tieflöffel usw. Originalvideo ausgeschnitten werden. Was tun Sie, wenn Ihre app im stricken Datencenter gehostet wird? Es ist möglich, richten Sie Ihre app in Azure in mehreren Regionen gleichzeitig ausführen, sodass ein Notfall in einer vorhanden ist, Sie weiter ausgeführt, in einer anderen Region. Solche Fehler sind äußerst selten vorkommen und die meisten apps nicht über die überblicken erforderlich, um sicherzustellen, dass durch Fehler dieser Art eines unterbrechungsfreien Diensts springen. Siehe Abschnitt "Ressourcen" am Ende des Kapitels Informationen dazu, wie Sie Ihre app auch über eine Region Fehler verfügbar bleibt.

Ein Ziel von Azure ist, um alle dieser Arten von Fehlern, die viel einfacher zu machen, und Sie finden einige Beispiele dafür, wie wir, die in den folgenden Kapiteln tun.

## <a name="slas"></a>SLAs

Personen hören häufig zu Vereinbarungen zum Servicelevel (SLAs) in der Cloudumgebung. Grundsätzlich handelt es sich Zusagen, die Unternehmen stellen Informationen zu den Zuverlässigkeit ihres Diensts. 99,9 % SLA bedeutet, dass Sie erwarten, dass den Dienst ordnungsgemäß 99,9 % der Zeit. Dies ist ein relativ häufig angegebener Wert für die Vereinbarung zum SERVICELEVEL und hört sich wie eine sehr große Anzahl, aber Sie bemerken möglicherweise nicht wie viel Zeit nach unten. 1 % läuft tatsächlich auf. Hier ist eine Tabelle, wie viel Ausfallzeit verschiedene SLA-Prozentsätze über ein Jahr, Monat und einer Woche beanspruchen.

![SLA-Tabelle](design-to-survive-failures/_static/image2.png)

Daher möglicherweise ein 99,9 % SLA bedeutet, dass Ihr Dienst 8.76 Stunden ein Jahr oder 43,2 Minuten pro Monat ausgeschaltet. Dies ist mehr außer Betrieb genommen, als die meisten Personen bewusst. So, als Entwickler sollten Sie daran denken, dass eine bestimmte Menge an außer Betrieb genommen werden kann und auf ordnungsgemäße Weise zu behandeln. Irgendwann wird von Personen Ihre app verwenden ein Diensts wird inaktiv geschaltet und möchten die negative Auswirkung, die auf den Kunden zu minimieren.

Dabei sollten Sie die Vereinbarung zum SERVICELEVEL kennen ist, welchem Zeitrahmen auf verweist: ist erhalten die Uhr wöchentlich, monatlich oder jährlich zurückgesetzt? In Azure zurückgesetzt wir die Uhr jeden Monats, der besser für Sie als eine jährliche SLA ist, da eine jährliche SLA schlechten Monaten verbergen, könnte versetzen sie mit einer Reihe von gute Monate.

Natürlich gibt es immer eine bessere Leistung als die SLA führen; Sie müssen in der Regel viel kleiner als die ausgeschaltet. Die Zusage ist, dass wir jemals nach unten für die maximal außer Betrieb genommen sind Sie Geld wieder einholen können. Des Geldbetrags, den Sie wieder wahrscheinlich erhalten würde nicht vollständig kompensieren Sie der Auswirkung der die Überschreitung außer Betrieb genommen, aber dieser Aspekt der SLA fungiert als eine Erzwingungsrichtlinie und lässt Sie wissen, dass wir es ernst akzeptieren.

### <a name="composite-slas"></a>Zusammengesetzte SLAs

Eine wichtige Sache ist zu bedenken, wenn Sie sich SLAs ansehen ist die Auswirkung der Verwendung von mehreren Diensten in einer app mit jedem Dienst müssen eine separate SLA. Beispielsweise verwendet die app zu beheben, Azure App Service-Web-Apps, die Azure-Speicher und SQL-Datenbank. Hier sind die SLA-Nummern ab dem Datum, an das diese e-Book in Dezember 2013 geschrieben wird:

![SLA-Website, Speicher, SQL-Datenbank](design-to-survive-failures/_static/image3.png)

Was ist die maximale außer Betrieb genommen, die Sie erwarten würden für die app, die basierend auf diesen Dienst SLAs? Sie können sich vorstellen, dass die Ausfallzeiten in diesem Fall entspricht der schlechtesten SLA-Prozentsatz oder 99,9 % wäre. Das wäre "true", wenn alle drei Dienste, die immer zur selben Zeit fehlgeschlagen ist, das jedoch nicht notwendigerweise was tatsächlich geschieht. Jeder Dienst kann unabhängig zu unterschiedlichen Zeitpunkten fehlschlagen, müssen Sie die zusammengesetzte SLA zu berechnen, indem Sie die einzelnen SLA Zahlen multipliziert.

![Zusammengesetzte SLA](design-to-survive-failures/_static/image4.png)

Damit Ihre app konnte werden Sie nicht nur 43,2 Minuten eines Monats jedoch 3 Mal dieser Menge, 108 Minuten pro Monat – und werden immer noch innerhalb der Grenzen des Azure-SLA.

Dieses Problem ist nicht in Azure eindeutig. Wir bieten tatsächlich die beste Cloud SLAs für alle Cloud-Dienst verfügbar, und haben ähnliche Probleme zu behandeln, wenn Sie beliebige Anbieter Cloud-Dienste verwenden. Was dies unterstreicht, ist die Bedeutung der überlegt werden, wie Sie Ihrer app die unvermeidliche Dienstfehler sauber entwerfen können, da sie häufig genug passieren können, auf Ihre Kunden oder Benutzer auswirken.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Cloud-SLAs, die im Vergleich zu Enterprise Ausfallzeit Erfahrung

Personen z. manchmal b. "In meinem Unternehmen app ich nie diese Probleme haben." Sie bitten, wie viele Ausfallzeiten in einem Monat besitzen sie tatsächlich, es wird argumentiert in der Regel, "Es gelegentlich passiert." Und wenn Sie Häufigkeit aufgefordert, sie geben, die "Manchmal wir zu sichern oder einen neuen Server oder Update-Software installieren müssen." Natürlich zählt, wie außer Betrieb genommen. Die meisten Unternehmens-apps sind, wenn sie vor allem unternehmenswichtige sind tatsächlich heruntergefahren seit mehr als die Zeitspanne, die von unseren Dienst SLAs zulässig. Aber wenn sie Ihren Server und Ihrer Infrastruktur ist und Sie verantwortlich dafür und Kontrolle darüber sind, meist weniger angst Ausfallzeiten festlegt, zu. In einer Cloudumgebung Sie eine andere Person abhängig sind, und nicht kennen, was passiert, sodass Sie sind tendenziell mehr sorgen sie abrufen können.

Wenn ein Unternehmen erzielt, einen höheren Prozentsatz der Laufzeit wird als Sie aus einer Cloud-SLA erhalten, dazu sie viel Geld für Hardware verbringt. Ein Cloud-Dienst konnte nachholen, jedoch müssen Sie vieles mehr für ihre Dienste in Rechnung zu stellen. Stattdessen eine kostengünstige Service nutzen und Entwerfen Ihrer Software, damit die unvermeidliche Fehler auftreten, minimale Unterbrechung an Ihre Kunden. Ihre Aufgabe als einem Cloud-app-Designer ist nicht so viel zu vermeiden Sie Fehler hinsichtlich einer Katastrophe zu vermeiden, und Sie, die durch die Fokussierung auf Software, die nicht auf Hardware. Während der Unternehmens-apps bemühen, durchschnittliche Zeit zwischen Fehlern zu maximieren, bemühen Cloud-apps, durchschnittliche Zeit bis zur Wiederherstellung zu minimieren.

### <a name="not-all-cloud-services-have-slas"></a>Nicht alle Cloud-Dienste verfügen SLAs

Sein Beachten Sie auch, nicht für jeden Cloud-Dienst selbst eine SLA besitzt. Wenn Ihre app eines Diensts mit keine Garantie Betriebszeit abhängig ist, ist Sie möglicherweise nach unten wesentlich länger, als Sie sich vorstellen können. Beispielsweise, wenn Sie auf Ihrer Website mit einem sozialen Anbieter z. B. Facebook oder Twitter anmelden aktivieren, erkundigen Sie sich der Dienstanbieter, um herauszufinden, ob eine SLA vorhanden ist, und stellen Sie möglicherweise gibt es kein solcher Routenname. Aber wenn der Authentifizierungsdienst ausfällt oder um die Menge der Anfragen zu unterstützen, die Sie es auslösen kann, werden Ihre Kunden aus Ihrer app gesperrt. Sie sind möglicherweise für Tage oder länger ausgeschaltet. Den Ersteller der eine neue app erwartet vieler Hundert Millionen des Downloads und gedauert hat eine Abhängigkeit auf Facebook-Authentifizierung –, aber nicht sprechen Sie mit Facebook gelangen Sie Livedaten und ermittelten zu spät, die es keine SLA für diesen Dienst wurde.

### <a name="not-all-downtime-counts-toward-slas"></a>Nicht alle Ausfallzeiten angerechnet SLAs

Einige Clouddienste können absichtlich Dienst verweigern, wenn Ihre app, die diese stark verwendet. Hierbei spricht *Einschränkung*. Wenn ein Dienst eine SLA verfügt, sollte er die Bedingungen beinhalten, unter dem Sie die eingeschränkt werden können und Entwurfs app sollte vermeiden Sie diese Bedingungen und reagieren entsprechend der Einschränkung, wenn es auftritt. Z. B. sollten Anforderungen an einen Dienst starten fehlschlägt, wenn Sie eine bestimmte Anzahl pro Sekunde überschreitet, Sie sicherstellen, dass automatische Wiederholversuche so schnell passieren nicht, dass sie bewirken, die Einschränkung dass, um den Vorgang fortzusetzen. Lassen wir weitere sagen zur Einschränkung in der [vorübergehender Fehler behandeln Kapitel](transient-fault-handling.md).

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel hat versucht, können Sie erkennen, warum eine reales Cloud-app wurde so konzipiert, dass Fehler ordnungsgemäß verkraftet. Beginnend mit der [nächsten Kapitels](monitoring-and-telemetry.md), die verbleibenden Muster in dieser Serie gehen ausführlicher erläutert einige Strategien, die Sie dazu verwenden können:

- Haben Sie gut [Überwachung und Telemetrie](monitoring-and-telemetry.md), sodass Sie feststellen, schnell zu Fehlern, die einen Eingriff erfordern und Sie verfügt über ausreichende Informationen zur Problembehebung.
- [Behandeln vorübergehender Fehler](transient-fault-handling.md) durch Implementieren von Wiederholungslogik intelligent, damit Ihre app automatisch wiederhergestellt wird, wenn er kann und fragt [Trennschalter](transient-fault-handling.md#circuitbreakers) Logik ist dies nicht möglich.
- Verwendung [verteiltes caching](distributed-caching.md) um Durchsatz, Latenz und Verbindung Probleme mit Zugriff auf die Datenbank zu minimieren.
- Lockere Kopplung über implementieren die [Warteschlange anwendungsorientierte Arbeit Muster](queue-centric-work-pattern.md), sodass die front-End-app verwenden, wenn der Back-End wird fortgesetzt werden kann.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie weiter unten Kapiteln dieser e-Book "und" den folgenden Ressourcen.

Dokumentation:

- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Version der FailSafe-Videoreihe Webseite.
- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy.
- [Azure technische Dokumentation zur Geschäftskontinuität](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Whitepaper von Patrick Wickline, und Jason Roth.
- [Notfallwiederherstellung und hohe Verfügbarkeit für Azure-Anwendungen](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Whitepaper von Michael McKeown Hanu Kommalapati und Jason Roth.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Bereitstellungsanleitung finden Sie unter mehreren Data Center, einen Trennschalter.
- [Azure-Support - Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).
- [Geschäftskontinuität in der Azure SQL-Datenbank](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). Dokumentation zu SQL-Datenbank hohe Verfügbarkeit und notfallwiederherstellung Wiederherstellungsfunktionen.
- [Hohe Verfügbarkeit und Notfallwiederherstellung für SQLServer auf Azure Virtual Machines](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videos:

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Neun zweiteilige Reihe Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Episoden 1 und 8 ein Wechsel in die Gründe für das Entwerfen von Cloud-apps für die Fehler verkraftet im Detail. Siehe auch die Nachbereitung Erläuterung der Einschränkung in der Folge 2 49:57, die Diskussion von Schwachstellen und Fehlerzuständen in Folge 2 beginnenden 56:05 und die Erläuterung der Trennschalter in Folge 3 beginnenden 40:55 ab.
- [Erstellen von Big: Erkenntnisse aus Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms dreht Entwerfen für Fehler und Instrumentieren alles ab. Vergleichbar mit dem Failsafe-Serie, aber wechselt in den Gewusst-wie-Informationen.

> [!div class="step-by-step"]
> [Zurück](unstructured-blob-storage.md)
> [Weiter](monitoring-and-telemetry.md)
