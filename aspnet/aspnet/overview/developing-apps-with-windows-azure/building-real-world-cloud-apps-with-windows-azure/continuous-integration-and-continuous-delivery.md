---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Fortlaufende Integration und kontinuierlichen Bereitstellung (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4a5433a7dd70e27b59163822ba427b026c3f4ce0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Fortlaufende Integration und kontinuierlichen Bereitstellung (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Die ersten beiden empfohlen wurden Prozess Entwicklungsmustern [automatisieren alles](automate-everything.md) und [Quellcodeverwaltung](source-control.md), und das dritte Prozess Muster kombiniert. Fortlaufende Integration (CI) bedeutet, dass wenn ein Entwickler Code, um das Quellrepository eincheckt, automatisch ein Build ausgelöst wird. Fortlaufende Übermittlung (CD) akzeptiert einen Schritt weiter: nach einem Build und automatisierte Komponententests erfolgreich sind, Sie automatisch die Anwendung in einer Umgebung, in denen möglich Weitere ausführliche Tests, bereitstellen.

Die Cloud können Sie Minimieren der Kosten eine testumgebung verwalten, da Sie nur für die Umgebung Ressourcen bezahlen solange Sie sie verwenden. CD-Prozess können Sie die testumgebung festlegen, wenn Sie ihn benötigen, und Sie können Sie die Umgebung ausführen, wenn Sie fertig sind testen.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Continuous Integration- und Continuous Delivery-workflow

Im Allgemeinen wird empfohlen, dass Sie die dauerhafte Bereitstellung in der Entwicklung und Stagingumgebungen ausführen. Die meisten Teams, auch bei Microsoft gewonnen – erfordern einen manuellen Prozess für Überprüfung und Genehmigung für die produktionsbereitstellung. Für eine Produktion erfolgt die Bereitstellung, Sie möchten sicherstellen, dass es möglicherweise, wenn wichtige Personen auf das Entwicklungsteam Supportzwecken oder Zeiten mit geringem Datenverkehr verfügbar sind. Es gibt jedoch, dass nichts zu verhindern, dass Sie die Entwicklungs- und testumgebungen vollständig automatisieren, sodass ein Entwickler muss nur eine Änderung und eine Umgebung Einchecken für Akzeptanztests festgelegt ist.

Im folgenden Diagramm aus [ein Microsoft Patterns and Practices e-Book zu kontinuierlichen Bereitstellung](http://aka.ms/ReleasePipeline) veranschaulicht einen typischen Workflow. Klicken Sie auf das Bild zur Anzeige voller Größe in seinem ursprünglichen Kontext.

[![Workflow für fortlaufende Übermittlung](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Wie die Cloud kostengünstige CI und CD ermöglicht

Es ist einfach, diese Prozesse in Azure automatisieren. Da Sie alles in der Cloud ausführen, müssen Sie erwerben oder Verwalten von Servern für Ihre Builds oder Ihre testumgebungen. Und Sie müssen für einen Server, führen Sie Tests auf Protokollpuffers warten. Mit jedem Build, die Sie ausführen, konnte Sie bündelt in Azure mithilfe Ihrer Automatisierungsskript, führen Akzeptanztests oder Weitere ausführliche Tests anhand einer testumgebung, und klicken Sie dann einfach anschließend beenden sie. Und wenn Sie diesen Server nur für 2 Stunden, 8 Stunden oder täglich ausgeführt, des Geldbetrags, in denen sie bezahlen ist minimal, da Sie nur für die Zeit einzurichten, die ein Computer tatsächlich ausgeführt wird. Beispielsweise erforderlichen die Umgebung für die Korrektur Anwendung im Grunde ungefähr 1 Cent pro Stunde Kosten, wenn Sie eine Ebene aus der freien Ebene nach oben. Im Verlauf eines Monats, wenn Sie nur die Umgebung pro Stunde zu einem Zeitpunkt ausgeführt wurde würde Ihre testumgebung wahrscheinlich kleiner als ein Latte Kosten, die Sie Finanzen zu kaufen.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS bietet eine Reihe von Funktionen, die Sie mit der Anwendungsentwicklung aus Planen der Bereitstellung zu unterstützen.

- (Verteilte) Git- und TFVC (zentral) Datenquellen-Steuerelement unterstützt.
- Er bietet eine flexible Builddienst zu, d. h. es dynamisch Buildservern erstellt, wenn sie benötigt werden, und bringt sie nach unten, wenn sie fertig sind. Sie können automatisch starten Sie einen Build, wenn jemand in quellcodeänderungen überprüft, und Sie keine zugeordnet haben und die Zahlen für Ihre eigenen Buildserver, die sich im Leerlauf in den meisten Fällen. Der Builddienst ist kostenlos, solange Sie eine bestimmte Anzahl von Builds nicht überschreiten. Wenn Sie erwarten, führen Sie eine große Anzahl von Builds, können Sie ein wenig zusätzlicher für reservierte Buildservern bezahlen.
- Dauerhafte Bereitstellung in Azure unterstützt.
- Automatisierte Auslastungstests unterstützt. Auslastungstests für eine Cloud-app von entscheidender Bedeutung ist jedoch häufig versäumt wird, bis es zu spät ist. Auslastungstests simuliert starke Nutzung von einer app durch Tausende von Benutzern, sodass Sie Engpässe zu ermitteln und Durchsatz zu verbessern, bevor Sie die app in die Produktion freigeben.
- Teamraumzusammenarbeit, das die Echtzeit-Kommunikation und Zusammenarbeit für kleine Teams der agilen vereinfacht unterstützt.
- Agile Projektmanagement unterstützt.


Weitere Informationen zum fortlaufende Integration und Übermittlung von Funktionen der VSTS finden Sie unter [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Wenn Sie für eine sofort einsetzbare Projektmanagement und Zusammenarbeit im Team Source Control-Lösung Auschecken VSTS suchen. Der Dienst ist für bis zu 5 Benutzer kostenlos, und registrieren können für sie am [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Zusammenfassung

Die ersten drei Cloud Entwicklungsmustern wurden Informationen zum Implementieren einer wiederholbare, zuverlässige und vorhersagbaren Entwicklungsprozesses mit niedrigen Zykluszeit. In der [nächsten Kapitels](web-development-best-practices.md) beginnen wir die Architektur und Codierung Muster betrachten.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter [bereitstellen eine Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Siehe auch die folgenden Ressourcen:

- [Erstellen einer Releasepipeline mit Team Foundation Server 2012](http://aka.ms/ReleasePipeline). E-Book herunterladen, praktische Übungseinheiten und Beispielcode von Microsoft Patterns and Practices, bietet eine ausführliche Einführung in fortlaufende Übermittlung. Deckt die Verwendung von Visual Studio Lab Management und Visual Studio Release Management.
- [Hinweise und ALM Rangers DevOps-Tools](https://aka.ms/vsarsolutions/). ALM Rangers eingeführt, die DevOps-Workbench Companion Beispielprojektmappe und praktische Hinweise in Zusammenarbeit mit den Mustern &amp; Praktiken-Buchs *Erstellen einer Releasepipeline mit TFS 2012*, als eine hervorragende Möglichkeit zum Starten lernen die Konzepte von DevOps &amp; Release Management für TFS 2012 und um die Ringe zu starten. Die Anleitung zeigt, wie einmal erstellen und in mehreren Umgebungen bereitstellen.
- [Tests für fortlaufende Übermittlung mit Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-Book vom Microsoft Patterns and Practices erläutert, wie automatisierte Tests mit kontinuierlichen Bereitstellung integrieren.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Der Quellcode für ein Tool zum Erfassen eines Builds in TFS (basierend auf einer Bezeichnung), erstellen Sie sie verpacken, können eine Person in der Rolle "DevOps" So konfigurieren Sie bestimmte Aspekte des Zertifikats und schieben Sie ihn in Azure. Das Tool verfolgt den Bereitstellungsprozess damit möglichen Vorgänge "auf eine zuvor bereitgestellte Version Rollback". Das Tool weist keine externen Abhängigkeiten und eigenständige mit TFS-APIs und das Azure SDK kann eingesetzt werden.
- [Continuous Delivery: Zuverlässige Software frei über Build-, Test- und Bereitstellungsautomatisierung](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Buch von Jez Humble.
- [Geben Sie es aus! Entwerfen und Bereitstellen von Software produktionsbereite](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Buch von Michael T. Nygard.

>[!div class="step-by-step"]
[Zurück](source-control.md)
[Weiter](web-development-best-practices.md)
