---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Contact Manager-Lösung | Microsoft-Dokumentation
author: jrjlee
description: In dieser tutorialreihe verwendet eine beispiellösung&#x2014;Contact Manager-Lösung&#x2014;zur Darstellung einer unternehmensweiten-Anwendung mit einer realistischen arbeiten...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: c8044c37738e9d23ca83648a6b571059338dc1a3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835158"
---
<a name="the-contact-manager-solution"></a>Contact Manager-Lösung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dies [Reihe von Tutorials](web-deployment-in-the-enterprise.md) verwendet eine beispiellösung&#x2014;Contact Manager-Lösung&#x2014;um eine unternehmensweite-Anwendung mit einem realistischen Maß an Komplexität darzustellen. In diesem Thema Contact Manager-Lösung führt, werden die Hauptkomponenten der Lösung und identifiziert die Herausforderungen bei der diese Art von Anwendungen für verschiedene Plattformen von Ziel in einer unternehmensumgebung bereitstellen.
> 
> Wie Sie in den Themen in diesen Tutorials durcharbeiten, können Sie Contact Manager-Lösung als eine referenzimplementierung verwenden, die veranschaulicht, wie Sie bestimmte Herausforderungen bei Bereitstellungsszenarios erfüllen können. Im nächsten Thema, [Einstellung Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md), beschreibt, wie Sie herunterladen und Ausführen der Lösung auf der Entwicklerarbeitsstation.


## <a name="solution-overview"></a>Übersicht über die Lösung

Contact Manager-Lösung besteht aus vier einzelne Projekte:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Dies ist ein ASP.NET MVC 3-Webanwendungsprojekt, das den Einstiegspunkt für die Projektmappe darstellt. Es bietet einige Funktionen der einfachen Web-Anwendung, wie Benutzern die Möglichkeit zum Erstellen und Anzeigen von Details für sicherheitskontakt bereitstellen. Die Anwendung verwendet einen Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und eine Datenbank für ASP.NET-Anwendungsdienste zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager.Database**. Dies ist ein Visual Studio-Datenbankprojekt. Das Projekt definiert das Schema für eine Datenbank, speichert Informationen wenden Sie sich an.
- **ContactManager.Service**. Dies ist ein WCF-Webdienstprojekt. Der WCF-Dienst macht ein Endpunkt, der Aufrufern ermöglicht, führen Sie zu erstellen, abrufen, aktualisieren und Löschvorgängen (CRUD) für die **ContactManager** Datenbank. Der Dienst beruht, die **ContactManager** Datenbank und die **ContactManager.Common.dll** Assembly.
- **ContactManager.Common**. Dies ist ein Klassenbibliotheksprojekt. Der WCF-Dienst basiert auf Typen, die in dieser Assembly definiert.

Die Lösung umfasst auch einen Projektmappenordner mit dem Namen veröffentlichen. Enthält verschiedene benutzerdefinierte Projektdateien und Befehlsdateien, die veranschaulichen, wie Sie steuern, und Bearbeiten des Build & Deployment-Prozesses. Diese werden später in diesem Lernprogramm ausführlicher behandelt.

Auf konzeptioneller Ebene passen die Komponenten der Lösung zusammen wie folgt:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Während die ASP.NET MVC 3-Webanwendung dem ASP.NET-Mitgliedschaftsanbieter verwendet wird, werden alle Seiten innerhalb der Webanwendung anonymen Zugriff zulassen. Dies ist natürlich keine realistische Konfiguration. Allerdings ist die Lösung richten Sie auf diese Weise, die es erleichtern Ihnen das Bereitstellen und Testen der Lösung ohne Benutzerkonten und Rollen zu konfigurieren.


## <a name="deployment-challenges"></a>Herausforderungen bei der Bereitstellung

Contact Manager-Lösung veranschaulicht verschiedene Herausforderungen bei der Bereitstellung, die zu einer Vielzahl von Bereitstellungsszenarios gelten:

- Die Lösung besteht aus mehreren abhängigen Projekten. Sie müssen diese Projekte gleichzeitig bereitstellen.
- Verbindungszeichenfolgen und Dienstendpunkten für jede Umgebung aktualisiert werden müssen, und in vielen Fällen diese Informationen werden nicht für den Entwickler verfügbar.
- Bei der Bereitstellung der **ContactManager** Datenbank Staging-und produktionsumgebungen müssen Sie vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten.
- Wenn Sie die Datenbank für ASP.NET-Anwendungsdienste bereitstellen, müssen Sie einige Konfigurationsdaten bereitstellen, aber lassen Sie alle Benutzerdaten für das Konto.
- Die Projekte enthalten einige Dateien und Ordner, die nicht bereitgestellt werden sollen. Sie müssen diese Dateien und Ordner aus dem Bereitstellungsprozess ausgeschlossen.
- Die Lösung muss automatisierte Bereitstellung aus einem Build-Server für Team Foundation Server (TFS) zu unterstützen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema eine allgemeine Übersicht über die Contact Manager-Lösung bereitgestellt und identifiziert einige der inhärenten Herausforderungen bei der Bereitstellung, die zu einer Vielzahl von Bereitstellungsszenarios gelten. Die übrigen Themen in diesem Tutorial werden einige der Techniken, die Sie verwenden können, um diese Herausforderungen zu erfüllen.

Im nächsten Thema, [Einstellung Einrichten der Contact Manager-Lösung](setting-up-the-contact-manager-solution.md), beschreibt, wie Sie herunterladen und Ausführen der Lösung auf der Entwicklerarbeitsstation.

> [!div class="step-by-step"]
> [Zurück](web-deployment-in-the-enterprise.md)
> [Weiter](setting-up-the-contact-manager-solution.md)
