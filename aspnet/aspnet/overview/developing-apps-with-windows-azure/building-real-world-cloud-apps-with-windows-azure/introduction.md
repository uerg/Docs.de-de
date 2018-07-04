---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Erstellen realer Cloud-Apps mit Azure | Microsoft-Dokumentation
author: MikeWasson
description: Dieses e-Book führt Sie durch einen musterbasierten Ansatz zum Erstellen von realitätsgetreuen Cloud-Lösungen. Die Muster gelten für den Entwicklungsprozess als auch ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 15e5c0a0411e3cd9433544e9a09b6373311daf6b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396213"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Erstellen realer Cloud-Apps mit Azure
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Dieses e-Book führt Sie durch einen musterbasierten Ansatz zum Erstellen von realitätsgetreuen Cloud-Lösungen. Die Muster gelten sowohl für den Entwicklungsprozess als auch für die Architektur und Praktiken zur codeerstellung.
> 
> Der Inhalt basiert auf einer Präsentation von Scott Guthrie entwickelt und bereitgestellt durch ihn an die Norwegische Entwickler Conference (NDC) im Juni 2013 ([Teil 1](http://vimeo.com/68215538), [Teil 2](http://vimeo.com/68215602)), und klicken Sie auf der Microsoft Tech Ed Australia in September 2013 ([Teil 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [Teil 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Viele andere](more-patterns-and-guidance.md#acknowledgments) aktualisiert und ergänzt die Inhalte bei der Umsetzung von Video in die Schriftform.


## <a name="intended-audience"></a>Zielgruppe

Entwickler, die wissen, was die Entwicklung für die Cloud in Betracht ziehen eine Verschiebung in die Cloud sind oder noch nicht mit cloud-Entwicklung werden hier eine präzise Übersicht über die wichtigsten Konzepte und Methoden, die sie kennen müssen. Die Konzepte sind mit konkreten Beispielen und jedes Kapitellinks zu anderen Ressourcen Ausführlichere Informationen veranschaulicht. Die Beispiele und Links zu weiteren Ressourcen sind für Microsoft-Frameworks und -Dienste, aber die gezeigten Grundsätze auf andere Webentwicklungs-Frameworks und Cloudumgebungen ebenfalls.

Entwickler, die bereits für die Cloud entwickeln möglicherweise, dass hier Ideen, die helfen ihnen noch erfolgreicher zu sein. Jedes Kapitel in der Reihe kann unabhängig voneinander gelesen werden, sodass Sie auswählen können, und wählen Themen, in denen Sie interessiert sind.

Jeder, der beobachtet Scott Guthries *Building Real World Cloud Apps mit Azure* Präsentations- und weitere Details und die aktualisierten Informationen, die hier findet möchte.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Cloud-Entwicklungsmuster

Dieses e-Book wird erläutert, dreizehn Muster für die Cloudentwicklung empfohlen. "Pattern" wird im weitesten Sinne hier verwendet, um ein empfohlenes Verfahren zum Aktionen bedeuten: optimal entwickeln, Entwerfen und Programmieren von Cloud-apps kennengelernt. Hierbei handelt es sich um wichtige Muster derer Sie "fallen unter der einfache Erfolg" Wenn Sie diese folgen.

- [Automatisieren Sie alles](automate-everything.md).

    - Verwenden Sie Skripts, um die Maximierung der Effizienz und Minimieren von Fehlern in der sich wiederholende Prozesse.
    - Demo: Azure Verwaltungsskripts.
- [Datenquellen-Steuerelement](source-control.md). 

    - Richten Sie Verzweigungsstruktur, die in der quellcodeverwaltung aus, um die DevOps-Workflow zu erleichtern.
    - Demo: Skripts zur quellcodeverwaltung hinzufügen.
    - Demo: Speichern Sie vertrauliche Daten aus der quellcodeverwaltung.
    - Demo: Verwenden Sie Git in Visual Studio.
- [Continuous Integration und Delivery](continuous-integration-and-continuous-delivery.md). 

    - Automatisieren Sie Builds und Continuous Deployment mit jedem Source Control einchecken.
- [Web Development best Practices](web-development-best-practices.md). 

    - Halten Sie die Webebene zustandslos.
    - Demo: Skalierung und automatische Skalierung in Web-Apps in Azure App Service.
    - Vermeiden des Sitzungszustands.
    - Verwenden Sie ein CDN mit Fallback, wenn das CDN nicht verfügbar ist.
    - Verwenden Sie asynchrones Programmiermodell.
    - Demo: Async in ASP.NET MVC und Entity Framework.
- [Einmaliges Anmelden](single-sign-on.md). 

    - Einführung in Azure Active Directory.
    - Demo: Erstellen einer ASP.NET-Apps aus, die Azure Active Directory verwendet.
- [Optionen für die datenspeicherung](data-storage-options.md). 

    - Typen von Datenspeichern.
    - Informationen zum Auswählen des richtigen Datenspeichers.
    - Demo: Azure SQL-Datenbank.
- [Strategien für die Datenpartitionierung](data-partitioning-strategies.md). 

    - Partitionieren von Daten vertikal, horizontal und/oder zu erleichtern die Skalierung von einer relationalen Datenbank.
- [Unstrukturiertes Blob Storage](unstructured-blob-storage.md). 

    - Store Dateien in der Cloud mit dem blobdienst.
    - Demo: Verwendung von Blob Storage in der app zu beheben.
- [Entwurf Ausfälle überbrücken](design-to-survive-failures.md). 

    - Arten von Fehlern.
    - Fehler-Bereich.
    - Grundlegendes zu SLAs.
- [Überwachung und Telemetrie](monitoring-and-telemetry.md). 

    - Warum sollte Sie sowohl eine app Telemetrie erwerben, und Schreiben eigenen Code zum Instrumentieren Ihrer app.
    - Demo: New Relic für Azure
    - Demo: das Protokollieren von Code in der app zu beheben.
    - Demo: Abhängigkeitsinjektion in der app zu beheben.
    - Demo: integrierte Protokollierung-Unterstützung in Azure.
- [Behandlung vorübergehender Fehler](transient-fault-handling.md). 

    - Verwenden Sie smart wiederholen/Backoff-Logik, um die Auswirkungen der vorübergehende Fehler zu vermeiden.
    - Demo: Wiederholung/Backoff in Entity Framework 6.
- [Verteilte Zwischenspeicherung](distributed-caching.md). 

    - Verbessern Sie der Skalierbarkeit und reduzieren Sie Transaktionskosten für die Datenbank, indem Sie Verteiltes Zwischenspeichern.
- [Warteschlangenorientierte Muster](queue-centric-work-pattern.md). 

    - Hohen Verfügbarkeit und verbessern Sie Skalierbarkeit zu, indem Sie die lose Kopplung von Web-und workerebenen.
    - Demo: Azure Storage-Warteschlangen in der app zu beheben.
- [Mehr cloud-app-Muster und Anleitungen](more-patterns-and-guidance.md).
- [Anhang: Fix It-Beispielanwendung](the-fix-it-sample-application.md)

    - Bekannte Probleme
    - Bewährte Methoden
    - So laden Sie herunter, erstellen, ausführen und bereitstellen.

Diese Muster gelten für alle Cloudumgebungen, aber sind, illustrieren sie anhand von Beispielen, die basierend auf der Microsoft-Technologien und Dienste wie Visual Studio, Team Foundation Service, ASP.NET und Azure.

Diese verbleibenden Teil dieses Kapitels werden die Fix It-beispielanwendung und die Web-Apps in Azure App Service-Cloud-Umgebung, die die Fix It-app ausgeführt, in wird eingeführt.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Die Lösung, die it-beispielanwendung

Die meisten Screenshots und in diesem e-Book dargestellten Codebeispiele basieren auf der Fix It-app ursprünglich entwickelt von [Scott Guthrie](https://weblogs.asp.net/scottgu/) empfohlene Cloud-app-Entwicklungsmuster und Vorgehensweisen veranschaulicht.

![Startseite der app beheben](introduction/_static/image1.png)

Die Beispiel-app ist eine einfache Arbeitsaufgabe mitgearbeitet. Wenn Sie einen festen benötigen, erstellen ein Ticket ein und weisen Sie zu einer Person und andere kann melden Sie sich, und finden Sie unter den Tickets zugewiesen an Sie und Tickets zu markieren, als abgeschlossen, wenn die Arbeit abgeschlossen ist.

Es ist ein standard Visual Studio-Webprojekt. Es basiert auf ASP.NET MVC und verwendet eine SQL Server-Datenbank. Sie können in IIS Express lokal ausführen und auf einer Azure-Website in der Cloud ausführen bereitgestellt werden kann. Sie können sich mit Formularauthentifizierung und einer lokalen Datenbank oder Anbieter sozialer Netzwerke wie Google anmelden. (Später auch zeigen wir wie mit einem Active Directory-organisationskonto anmelden.)

![Anmeldeseite](introduction/_static/image2.png)

Sobald Sie angemeldet sind in können Sie ein Ticket erstellen, es einem Benutzer zuweisen und Hochladen einen Überblick darüber, was behoben werden soll.

![Erstellen Sie einen Fix It-task](introduction/_static/image3.png)

![Beheben Sie diese Aufgabe erstellt](introduction/_static/image4.png)

Sie können das Verfolgen von Arbeitsaufgaben, die Sie erstellt haben, finden Sie Tickets, die Sie Details zum Ticket und Mark Elemente zugewiesen werden, als abgeschlossen.

Dies ist eine sehr einfache app hinsichtlich der Funktion, aber Sie erfahren, wie diese erstellt werden, damit sie auf Millionen von Benutzern skaliert werden kann und, wie z. B. Datenbankfehler und verbindungsunterbrechungen robust werden. Außerdem sehen, wie Sie eine automatisierte und agilen Entwicklungsworkflow zu erstellen, wodurch Sie fangen Sie einfach, und stellen die app eine bessere und bessere durch Iteration des Entwicklungszyklus, schnell und effizient.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>In Azure App Service-Web-Apps

Die Cloud-Umgebung für die Fix It-Anwendung verwendet, ist ein Dienst von Azure, dass wir Websites aufrufen. Dieser Dienst ist eine Möglichkeit, Sie können Ihre eigenen Web-app in Azure hosten, ohne virtuelle Computer erstellen und deren Aktualisierung beibehalten, installieren und Konfigurieren von IIS, usw. an. Wir hosten Ihre Website auf den virtuellen Computern und automatisch bereitzustellen, Sicherung und-Wiederherstellung sowie andere Dienste für Sie. Die Websites-Dienst funktioniert mit ASP.NET, Node.js, PHP und Python. Sie können Sie sehr schnell mithilfe von Visual Studio, Web Deploy, FTP, Git oder TFS bereitstellen. Dies ist normalerweise nur ein paar Sekunden zwischen dem Starten einer Bereitstellung und die Zeit, die der Updates über das Internet verfügbar ist. Es ist für den Einstieg kostenlos, und Sie können nach oben skalieren, wenn Ihr Datenverkehr zunimmt.

Hinter den Kulissen bietet Web-Apps in Azure App Service viele der Komponenten der Architektur und Funktionen, die Sie selbst erstellen, wenn Sie eine Website mit IIS auf Ihren eigenen virtuellen Computern hosten möchten müssen. Eine Komponente ist eine End-Bereitstellungspunkt, der automatisch IIS konfiguriert und installiert Ihre Anwendung auf so viele virtuelle Computer, wie Sie Ihre Website ausführen möchten.

![Bereitstellung (Dienst)](introduction/_static/image5.png)

Wenn ein Benutzer der Website klickt, sie nicht die IIS-VMs direkt erreicht, sie durchlaufen [Application Request Routing (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) load balancer. Sie können diese mit Ihren eigenen Servern verwenden, aber dies hat den Vorteil, dass er für Sie automatisch eingerichtet werden. Sie verwenden eine intelligente Heuristik, die Faktoren wie z. B. sitzungsaffinität Warteschlangentiefe in IIS verwendet, und CPU-Auslastung auf jedem Computer, leiten Sie Datenverkehr an die virtuellen Computer, Hosten Ihrer Website.

![ARR-Lastenausgleich](introduction/_static/image6.png)

Wenn ein Computer ausfällt, Azure automatisch per Pullvorgang aus der Rotation, startet eine neue VM-Instanz und beginnt, Datenverkehr an die neue Instanz – alle ohne Downtime für Ihre Anwendung.

![Automatische Wiederherstellung bei Computerfehler](introduction/_static/image7.png)

All dies erfolgt automatisch. Sie müssen lediglich eine Website erstellen und Bereitstellen Ihrer Anwendung, die mit Windows PowerShell, Visual Studio oder das Azure-Verwaltungsportal.

Schnelle und einfache schrittweise dieses Tutorial veranschaulicht, wie erstellen eine Webanwendung in Visual Studio und es auf einer Azure-Website bereitstellen, finden Sie unter [erste Schritte mit Azure und ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Zusammenfassung

Diese Einführung hat eine Liste der Themen, in denen, die das Buch behandelt, Screenshots der beispielanwendung und eine kurze Übersicht über die Web-Apps in Azure App Service-Cloud-Umgebung bereitgestellt werden. Einer der großen Vorzüge der Entwicklung von apps, die in und für die Cloud ist, dass es einfach, wie z. B. eine testumgebung erstellen und Bereitstellen von Code, sich wiederholende Entwicklungsaufgaben automatisieren. Vorgehensweise, die Gegenstand der [im nächsten Kapitel](automate-everything.md).

## <a name="resources"></a>Ressourcen

Weitere Informationen zu den Themen in diesem Kapitel finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Web-Apps in Azure App Service](https://azure.microsoft.com/services/app-service/web/). Portalseite für Azure-Dokumentation zu Web-Apps.
- [Web-Apps, Clouddienste und virtuelle Computer: wann die verwenden?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS, wie in diesem Kapitel dargestellt ist nur eine von drei Methoden, die Web-apps in Azure ausgeführt werden können. Dieser Artikel erläutert die Unterschiede zwischen den drei Arten und bietet Anleitungen zum auswählen, welches Abonnement für Ihr Szenario geeignet ist. Wie andere Websites ist Cloud Services ein PaaS-Feature von Azure. Virtuelle Computer sind eine IaaS-Funktion. Eine Erläuterung von PaaS und IaaS, finden Sie unter den [Datenoptionen](data-storage-options.md#paasiaas) Kapitel.

Videos:

- [Scott Guthrie beginnt bei Schritt 0: Was ist, dass das Azure-Cloud-Betriebssystem?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Websites-Architektur - mit Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Azure-Web Sites-Interna mit Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Nächste](automate-everything.md)
