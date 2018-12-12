---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Continuous Integration und Continuous Delivery (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: f701488025ec84a17651f9abff43f84531d35689
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576247"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Continuous Integration und Continuous Delivery (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Die ersten beiden empfohlen Development-Process-Muster wurden [automatisieren alles](automate-everything.md) und [Quellcodeverwaltung](source-control.md), und das dritte Prozess-Muster kombiniert. Continuous Integration (CI) bedeutet, dass wenn ein Entwickler im Code, um das Quell-Repository eincheckt, automatisch ein Build ausgelöst wird. Continuous Delivery (CD) geht noch einen Schritt weiter: Nachdem ein Build und automatisierte Komponententests erfolgreich sind, Sie automatisch die Anwendung in einer Umgebung, in denen erreichen Sie weitere ausführliche Tests, bereitstellen.

Die Cloud können Sie die Kosten für die Verwaltung einer testumgebung, da Sie nur für die umgebungsressourcen Zahlen so lange Sie Bereiche verwenden. Ihren CD-Prozess kann Sie die testumgebung festgelegt, wenn Sie ihn benötigen, und Sie können die Umgebung Herunterfahren, wenn Sie fertig sind testen.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Continuous Integration und Continuous Delivery-workflow

Im Allgemeinen wird empfohlen, dass Sie continuous Delivery für Ihre Entwicklungs- und Stagingumgebungen ausführen. Die meisten Teams, auch bei Microsoft und erfordern einen manuellen Prozess der Überprüfung und Genehmigung für die produktionsbereitstellung. Für einen in der produktionsumgebung erfolgt die Bereitstellung, Sie möchten sicherstellen, dass es möglicherweise, wenn entscheidende Personen auf das Entwicklungsteam für die Unterstützung oder Zeiten mit geringer Auslastung verfügbar sind. Es gibt jedoch, dass nichts zu verhindern, dass Sie Ihre Entwicklungs- und testumgebungen vollständig automatisieren, sodass alles, was ein Entwickler hat zu tun ist, überprüfen Sie in einer Umgebung und eine Änderung für Tests auf Akzeptanz eingerichtet ist.

Im folgenden Diagramm aus [ein Microsoft Patterns and Practices e-Book Informationen zu continuous Delivery](http://aka.ms/ReleasePipeline) veranschaulicht einen typischen Workflow. Klicken Sie auf das Bild, das er finden Sie unter voller Größe in seinem ursprünglichen Kontext.

[![Continuous Delivery-workflow](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Wie die Cloud kostengünstige CI- und CD ermöglicht

Es ist einfach, diese Prozesse in Azure automatisieren. Da Sie alles, was in der Cloud ausführen, müssen Sie erwerben oder Verwalten von Servern für Ihre Builds oder Ihren testumgebungen. Und Sie nicht warten, bis ein Server für Ihre Tests auf verfügbar sein. Sie können mit jedem Build, die Sie ausführen, richten Sie eine testumgebung in Azure unter Verwendung Ihres Automatisierungsskripts, ausführen Akzeptanztests oder Weitere ausführliche Tests für diese und dann danach einfach beenden sie. Und wenn Sie diesen Server nur für 2 Stunden, 8 Stunden oder einen Tag ausführen, die den Geldbetrag, die Sie dafür bezahlen ist minimal, da Sie nur für den Zeitraum bezahlen, die ein Computer tatsächlich ausgeführt wird. Z. B. erforderlich die Umgebung für die Korrektur, dass die Anwendung im Grunde ungefähr 1 Cent pro Stunde Kosten einer fehlklassifizierung einer Ebene wird von der kostenfreien Version steigt. Im Verlauf eines Monats wird, wenn Sie nur die Umgebung pro Stunde zu einem Zeitpunkt ausgeführt wurde, würde Ihre Testumgebung wahrscheinlich weniger kosten als der Latte Macchiato, den Sie bei Starbucks kaufen.

## <a name="azure-devops-services"></a>Azure DevOps Services 

Azure DevOps-Dienste bietet eine Reihe von Funktionen, die Ihnen bei der Anwendungsentwicklung aus Planen der Bereitstellung helfen.

- Es unterstützt (verteilt) Git- und TFVC (zentralisiert) Datenquellen-Steuerelement.
- Es bietet einen Dienst für elastische Datenbanken erstellen, was bedeutet es dynamisch Buildservern erstellt, wenn sie benötigt werden und sie nach unten, wenn sie fertig sind. Sie können automatisch einen Build starten, wenn jemand Änderungen am Quellcode checkt, und Sie müssen keine zugeordnet haben, und Zahlen Sie für Ihre eigenen Buildserver, die sich im Leerlauf in den meisten Fällen. Der Builddienst ist kostenlos, solange Sie eine bestimmte Anzahl von Builds nicht überschreiten. Wenn Sie erwarten, dass Sie eine große Anzahl von Builds, können Sie ein wenig zusätzliche reservierte Buildservern Zahlen.
- Continuous Delivery für Azure unterstützt.
- Es unterstützt die automatisierte Auslastungstests. Auslastungstests ist entscheidend für eine Cloud-app jedoch häufig vernachlässigt wird, bis es zu spät ist. Auslastungstests simuliert intensiven Gebrauch von einer app durch Tausende von Benutzern, sodass Sie Engpässe ermitteln und den Durchsatz verbessern, bevor Sie die app in der produktionsumgebung veröffentlichen.
- Teamraumzusammenarbeit, die Echtzeit-Kommunikation und Zusammenarbeit für kleine Teams der agilen erleichtert unterstützt.
- Agile-projektverwaltung unterstützt.


Weitere Informationen zu continuous Integration und Continuous Delivery-Funktionen von Azure DevOps-Dienste zu erhalten, finden Sie unter [der Dokumentation zu Azure DevOps](/azure/devops/index).

Wenn Sie für eine sofort einsetzbare Projektmanagement, Zusammenarbeit im Team und Source-Control-Lösung, sehen Sie sich Azure DevOps-Dienste suchen. Melden Sie sich bei [Azure DevOps-Dienste](https://dev.azure.com/).

## <a name="summary"></a>Zusammenfassung

Die ersten drei Cloud-Entwicklungsmuster wurden dazu, wie Sie einen Entwicklungsprozess wiederholbare, zuverlässige und vorhersagbare mit niedrigen Zykluszeit zu implementieren. In der [im nächsten Kapitel](web-development-best-practices.md) beginnen wir, Architektur und coding-Muster betrachten.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter [bereitstellen eine Web-app in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Siehe auch die folgenden Ressourcen:

- [Erstellung einer Versionspipeline mit Team Foundation Server 2012](http://aka.ms/ReleasePipeline). E-Book, praktische Übungseinheiten und Beispielcode von Microsoft Patterns and Practices, bietet eine detaillierte Einführung in continuous Delivery. Behandelt die Verwendung von Visual Studio Lab Management und Release Management für Visual Studio.
- [ALM Rangers-DevOps-Tools und Anleitungen](https://aka.ms/vsarsolutions/). ALM Rangers eingeführt, die DevOps-Workbench Beispiel begleitende Lösung und praktische Anleitungen in Zusammenarbeit mit den Mustern &amp; Methoden Buch *Erstellung einer Versionspipeline mit TFS 2012*, als eine hervorragende Möglichkeit zum Starten lernen die Konzepte von DevOps &amp; Release Management für TFS 2012 und austesten. Die Anleitung wird gezeigt, wie in Umgebungen mit mehreren einmal erstellen und bereitstellen.
- [Tests für fortlaufende Übermittlung mit Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). E-Book von Microsoft Patterns and Practices, erläutert integrieren Sie automatisierte Tests mit continuous Delivery.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Der Quellcode für ein Tool zum Erfassen eines Builds aus TFS (basierend auf einer Bezeichnung), erstellen Sie sie, es gepackt werden, können Personen in der DevOps-Rolle so konfigurieren Sie bestimmte Aspekte des Clouddiensts und per Push in Azure übertragen. Das Tool verfolgt während des Bereitstellungsvorgangs, um Vorgänge auf "zurück auf eine zuvor bereitgestellte Version zurücksetzen" zu aktivieren. Das Tool weist keine externen Abhängigkeiten und kann eigenständige mithilfe der TFS-APIs und das Azure SDK-Funktion.
- [Fortlaufende Bereitstellung: Zuverlässige Software frei, über den Build-, Test- und Automatisierung der Bereitstellung](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Buch von Jez Humble.
- [Veröffentlichen Sie es aus! Entwerfen und Bereitstellen von Software für produktionsbereite](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Buch von Michael T. Nygard.

> [!div class="step-by-step"]
> [Zurück](source-control.md)
> [Weiter](web-development-best-practices.md)
