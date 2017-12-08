---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Erstellen von Real-World Cloud Apps with Azure | Microsoft Docs
author: MikeWasson
description: "Diese e-Book führt Sie durch ein Muster basierender Ansatz, Real-World Cloud-Lösungen zu erstellen. Die Muster gelten, für den Entwicklungsprozess sowie im Vergleich zu einem..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5054f932d05fb612a6e18a81274719d7e249b77b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="building-real-world-cloud-apps-with-azure"></a>Real-World Cloud Apps with Azure erstellen
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Diese e-Book führt Sie durch ein Muster basierender Ansatz, Real-World Cloud-Lösungen zu erstellen. Die Muster gelten für den Entwicklungsprozess und für Architektur und Codierungstechniken.
> 
> Der Inhalt basiert auf einer Präsentation von Scott Guthrie entwickelt und von ihm am Norwegisch Entwickler Konferenz (NDC) vom Juni 2013 übermittelt ([Teil 1](http://vimeo.com/68215538), [Teil 2](http://vimeo.com/68215602)), und an Microsoft Tech Eduard Australia in September 2013 ([Teil 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [Teil 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Viele andere](more-patterns-and-guidance.md#acknowledgments) aktualisiert und augmentiert, dass den Inhalt beim aus Video geschriebene Form wechselt.


## <a name="intended-audience"></a>Beabsichtigte Zielgruppe

Entwickler sind interessieren, die für die Cloud entwickeln erwägen einen Wechsel in die Cloud oder mit dem cloud-Entwicklung vertraut sind, werden hier eine präzise Übersicht über die wichtigsten Konzepte und bewährte Methoden für das, die Sie wissen müssen. Konkrete Beispiele und jedes Kapitellinks auf andere Ressourcen für weitere ausführliche Informationen werden die Konzepte veranschaulicht. In den Beispielen und die Links zu weiteren Ressourcen für Microsoft-Frameworks und Dienste sind, aber die dargestellten Prinzipien gelten für andere Web-Entwicklungsframeworks und cloud-Umgebungen sowie.

Entwickler, die bereits für die Cloud entwickeln möglicherweise, dass sie hier Ideen, mit deren Hilfe werden attraktiver machen. Jedes Kapitel in der Reihe kann unabhängig voneinander gelesen werden. daher können Sie auswählen, und wählen Sie die Themen, in denen Sie interessiert sind.

Jeder, der beobachtet wird Guthries *Building Real World Cloud Apps with Azure* Präsentations- und weitere Details und aktualisierte Informationen, die hier findet möchte.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Cloud-Entwicklungsmustern

Diese e-Book erläutert 13 Muster für die Cloudentwicklung empfohlen. "Pattern" wird im weitesten Sinne hier verwendet, um eine empfohlene Methode zum erledigen bedeuten: Wechseln zum Entwerfen, entwickeln und Codierung von Cloud-apps wie am besten. Hierbei handelt es sich um Steuerelementmuster derer Sie "fallen unter die Pit erfolgreich", wenn Sie die Links.

- [Automatisieren Sie alles](automate-everything.md).

    - Verwenden Sie Skripts, um die Maximierung der Effizienz und Minimieren von Fehlern in sich wiederholende Prozesse.
    - Demo: Azure-Verwaltungsskripts.
- [Quellcodeverwaltung](source-control.md). 

    - Richten Sie Verzweigungsstruktur in der quellcodeverwaltung aus, um DevOps-Workflow zu ermöglichen.
    - Demo: Skripts zur quellcodeverwaltung hinzufügen.
    - Demo: behalten Sie vertrauliche Daten aus der quellcodeverwaltung.
    - Demo: Verwendung von Git in Visual Studio.
- [Fortlaufende Integration und Bereitstellung](continuous-integration-and-continuous-delivery.md). 

    - Build- und Bereitstellungsprozess mit jeder Quelle Steuerelement Check-in zu automatisieren.
- [Web Development best Practices](web-development-best-practices.md). 

    - Behalten Sie die Webebene zustandslos.
    - Demo: Skalierung und automatische Skalierung in Web-Apps in Azure App Service.
    - Vermeiden Sie Sitzungsstatus.
    - Verwenden Sie einen CDN.
    - Modell für die asynchrone Programmierung verwenden.
    - Demo: asynchrone in ASP.NET MVC und Entity Framework.
- [Einmaliges Anmelden](single-sign-on.md). 

    - Einführung in Azure Active Directory.
    - Demo: Erstellen einer ASP.NET app, die Azure Active Directory verwendet.
- [Datenspeicheroptionen](data-storage-options.md). 

    - Arten von Datenspeichern.
    - Wie Sie den richtigen Datenspeicher auswählen.
    - Demo: Azure SQL-Datenbank.
- [Strategien für die Datenpartitionierung](data-partitioning-strategies.md). 

    - Partitionieren von Daten, vertikal, horizontal oder beides zu ermöglichen, Skalierung einer relationalen Datenbank.
- [Unstrukturierte Blob-Speicher](unstructured-blob-storage.md). 

    - Speichern Sie Dateien in der Cloud mithilfe des Blob-Diensts.
    - Demo: Verwenden von Blob-Speicher in der app zu beheben.
- [Beim Entwurf Fehler verkraftet](design-to-survive-failures.md). 

    - Typen von Fehlern.
    - Fehler-Bereich.
    - Grundlegendes zu SLAs.
- [Überwachung und Telemetrie](monitoring-and-telemetry.md). 

    - Warum sollten Sie sowohl eine Telemetrie-app zu erwerben, und Schreiben Sie Code zum Instrumentieren Ihrer app.
    - Demo: New Relic für Azure
    - Demo: das Protokollieren von Code in der app zu beheben.
    - Demo: Abhängigkeitsinjektion in der app zu beheben.
    - Demo: protokollierungsunterstützung der integrierte in Azure.
- [Behandlung vorübergehender Fehler](transient-fault-handling.md). 

    - Verwenden Sie intelligenten Wiederholung/backofflogik bereitstellen, um die Auswirkung der vorübergehende Fehler zu verringern.
    - Demo: Wiederholung/Backoff in Entity Framework 6.
- [Verteiltes caching](distributed-caching.md). 

    - Verbessern der Skalierbarkeit und Reduzieren von Transaktionskosten für die Datenbank mithilfe von Verteiltes Zwischenspeichern.
- [Warteschlange anwendungsorientierte Arbeit Muster](queue-centric-work-pattern.md). 

    - Aktivieren Sie hohen Verfügbarkeit und verbessern Sie Skalierbarkeit zu, indem Sie Web- und Workerrollen Ebenen lose Kopplung.
    - Demo: Azure-Speicher-Warteschlangen in der app zu beheben.
- [Mehrere cloud-app-Muster und Anleitungen](more-patterns-and-guidance.md).
- [Anhang: Korrigieren sie Beispielanwendung](the-fix-it-sample-application.md)

    - Bekannte Probleme
    - Bewährte Methoden
    - Zum Herunterladen, erstellen, ausführen und bereitstellen.

Diese Muster gelten für alle Cloudumgebungen, aber diese über Beispiele, die basierend auf der Microsoft-Technologien und Dienste, z. B. Visual Studio, Team Foundation Service, ASP.NET und Azure illustrieren.

Diese weiteren Verlauf dieses Kapitels führt die beispielanwendung korrigieren und die Web-Apps in Azure App Service-Cloud-Umgebung, die die app zu beheben, die in ausgeführt wird.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Die Lösung, die es beispielanwendung

Die meisten Codebeispiele in diesem e-Book und Screenshots basieren auf der korrigieren-app ursprünglich entwickelt von [Scott Guthrie](https://weblogs.asp.net/scottgu/) empfohlene Cloud app Entwicklungsmustern und Methoden veranschaulicht.

![Korrigieren Sie diesen app-Startseite](introduction/_static/image1.png)

Die Beispiel-app ist eine einfache Arbeitsaufgabe System Tickets. Wenn Sie etwas festen benötigen, erstellen ein Ticket ein und weisen eine Person, und andere kann anzumelden und finden Sie unter die Tickets zugewiesen an Sie und Tickets zu markieren, als abgeschlossen, wenn die Arbeit abgeschlossen ist.

Es ist ein standard Visual Studio-Webprojekt. Es basiert auf ASP.NET MVC und verwendet eine SQL Server-Datenbank. Sie können in IIS Express lokal ausführen und kann auf einer Azure-Website, führen Sie in der Cloud bereitgestellt werden. Melden Sie sich mithilfe von Formularauthentifizierung oder einer lokalen Datenbank oder mithilfe eines sozialen Datenanbieters z. B. Google. (Später auch zeigen wir wie ein Active Directory-Unternehmenskonto anmelden.)

![Melden Sie sich auf der Seite](introduction/_static/image2.png)

Sobald Sie angemeldet sind in ein Ticket zu erstellen, weisen Sie es an eine Person und Hochladen einen Überblick über die korrigiert werden sollen.

![Erstellen einer Aufgabe zu beheben](introduction/_static/image3.png)

![Korrigieren Sie diesen Task erstellt](introduction/_static/image4.png)

Sie können Überwachen des Status von Arbeitsaufgaben, die Sie erstellt haben, finden Sie unter Tickets, Ticket "Details anzeigen", und markieren Elemente zugewiesen werden, als abgeschlossen.

Dies ist eine sehr einfache app hinsichtlich der Funktion, aber Sie sehen, wie Sie es erstellen, sodass er auf Millionen von Benutzern skaliert werden kann, und unempfindlich für z. B. Datenbankfehlern und abgebrochene Verbindungen sind wird. Außerdem sehen, wie eine automatisierte und agile-Entwicklungsworkflow erstellt, wodurch Sie zum Starten einfach und Bereitstellen der app eine bessere und eine bessere durch das Durchlaufen des Entwicklungszyklus schnell und effizient.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Web-Apps in Azure App Service

Die Cloud-Umgebung verwendet wird, für die Anwendung zu beheben ist ein Dienst von Azure, dass wir Websites aufrufen. Dieser Dienst ist eine Möglichkeit, hosten Ihre eigenen Web-app in Azure ohne Erstellen virtueller Computer und deren Aktualisierung beibehalten, installieren und Konfigurieren von IIS usw. an. Wir Ihrer Website auf unsere virtuelle Computer hosten und automatisch bereitstellen, Sicherung und-Wiederherstellung sowie andere Dienste für Sie. Die Websites-Dienst funktioniert mit ASP.NET, Node.js, PHP und Python. Sie können Sie sehr schnell mithilfe von Visual Studio, Web Deploy, FTP, Git oder TFS bereitstellen. Es ist in der Regel nur wenige Sekunden zwischen dem Zeitpunkt, die eine Bereitstellung zu starten und die Uhrzeit, die der Updates über das Internet verfügbar ist. Es ist kostenlos, um zu beginnen, und Sie können vertikal skalieren, wenn Ihr Datenverkehr zunimmt.

Hinter den Kulissen bietet Web-Apps in Azure App Service viele Komponenten der Architektur und Funktionen, die Sie selbst erstellen, wenn Sie mit der Hosten einer Website mithilfe von IIS für Ihre eigenen virtuellen Computer wurden müssten. Eine Komponente ist ein Endpunkt für Bereitstellung, der automatisch konfiguriert IIS und installiert die Anwendung auf wie viele virtuelle Computer, wie Sie Ihre Website ausführen möchten.

![Bereitstellungsdienst](introduction/_static/image5.png)

Wenn ein Benutzer der Website trifft, sie nicht die IIS-VMs direkt erreicht, durchlaufen sie [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) für den Lastenausgleich. Sie können diese mit Ihren eigenen Servern verwenden, aber hat der Vorteil ist, dass sie für Sie automatisch die Einrichtung abgeschlossen. Sie verwenden eine intelligente Heuristik, die in Faktoren wie z. B. sitzungsaffinität Warteschlangentiefe in IIS akzeptiert und CPU-Auslastung auf jedem Computer direkten Datenverkehr an den virtuellen Computern, Hosten Ihrer Website.

![ARR-Lastenausgleich](introduction/_static/image6.png)

Wenn ein Computer ausfällt, Azure automatisch zieht es aus der Rotation genommen, eine neue Computerinstanz der virtuellen läuft und startet Weiterleiten des Datenverkehrs an die neue Instanz – alle mit keine Ausfallzeiten für Ihre Anwendung.

![Automatische Wiederherstellung nach Ausfall des Computers](introduction/_static/image7.png)

All dies erfolgt automatisch. Alles, was Sie tun müssen ist eine Website erstellen und Bereitstellen Ihrer Anwendung, die mit Windows PowerShell, Visual Studio oder das Azure-Verwaltungsportal.

Eine schnelle und einfache schrittweises Lernprogramm, das zeigt, wie eine Webanwendung in Visual Studio erstellen und es auf einer Azure-Website bereitstellen, finden Sie unter [erste Schritte mit Azure und ASP.NET](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Zusammenfassung

Diese Einführung ist eine Liste der Themen, die das Buch abgedeckt wird, Screenshots der beispielanwendung und eine kurze Übersicht über die Web-Apps in Azure App Service-Cloud-Umgebung bereitgestellt. Einer der großen Vorzüge der Entwicklung von apps in und für die Cloud ist die einfache wiederkehrende appentwicklungsaufgaben, wie z. B. eine testumgebung erstellen und Bereitstellen von Code zu automatisieren. Vorgehensweise also das Subjekt der [nächsten Kapitels](automate-everything.md).

## <a name="resources"></a>Ressourcen

Weitere Informationen zu den Themen in diesem Kapitel behandelt finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Web-Apps in Azure App Service](https://azure.microsoft.com/en-us/services/app-service/web/). Portalseite zum Azure-Dokumentation zur Web-Apps.
- [Web-Apps, Cloud-Dienste und virtuelle Computer: wann welche verwenden?](https://azure.microsoft.com/en-us/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, wie in diesem Kapitel dargestellt ist nur eine von drei Methoden, die Web-apps in Azure ausgeführt werden können. Dieser Artikel erläutert die Unterschiede zwischen den drei Arten und enthält Hilfestellung zum auswählen, welche für Ihr Szenario geeignet ist. Wie andere Websites ist Cloud-Dienste eine PaaS-Funktion von Azure. Virtuelle Computer sind ein IaaS-Funktion. Eine Erläuterung der PaaS und IaaS, finden Sie unter der [Datenoptionen](data-storage-options.md#paasiaas) Kapitel.

Videos:

- [Scott Guthrie beginnt bei Schritt 0 – was die Azure-Cloud-Betriebssystem ist?](https://azure.microsoft.com/en-us/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Websites-Architektur - mit Stefan Schackow](https://azure.microsoft.com/en-us/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Merkmale der Azure-Websites mit Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

>[!div class="step-by-step"]
[Nächste](automate-everything.md)
