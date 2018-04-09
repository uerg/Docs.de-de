---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: Die Kontakt-Manager-Lösung | Microsoft Docs
author: jrjlee
description: Diese Reihe von Lernprogrammen verwendet eine beispiellösung&#x2014;die Projektmappe Contact Manager&#x2014;zur Darstellung einer Enterprise-Skalierung-Anwendung mit einer realistischen Leve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: d7034f800df98747d10401d7e2c7297fea0e46d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="the-contact-manager-solution"></a>Die Kontakt-Manager-Lösung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dies [Reihe von Lernprogrammen](web-deployment-in-the-enterprise.md) verwendet eine beispiellösung&#x2014;die Projektmappe Contact Manager&#x2014;um eine Enterprise-Skalierung-Anwendung mit einer realistischen Maß an Komplexität darzustellen. Dieses Thema führt die Kontakt-Manager-Lösung, beschreibt die wichtigsten Komponenten der Lösung und identifiziert die Herausforderungen bei der Bereitstellung dieser Art von Anwendung an verschiedenen Ziel Plattformen in einer unternehmensumgebung.
> 
> Arbeit durch die Themen in diesen Lernprogrammen können Sie die Projektmappe Contact Manager als eine referenzimplementierung verwenden, die zeigt, wie Sie bestimmte Probleme in Bereitstellungsszenarios erfüllen können. Im nächsten Thema [Einstellung von der Kontakt-Manager-Lösung](setting-up-the-contact-manager-solution.md), herunterladen und Ausführen der Projektmappe auf der Arbeitsstation Developer beschreibt.


## <a name="solution-overview"></a>Lösungsübersicht

Die Kontakt-Manager-Lösung besteht aus vier einzelne Projekte:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Dies ist eine ASP.NET MVC 3-Webanwendungsprojekt, die den Einstiegspunkt für die Projektmappe darstellt. Er bietet einige grundlegende Web Application-Funktionen, wie Benutzern die Möglichkeit zum Erstellen und Anzeigen von Details des Kontakts. Die Anwendung verwendet einen Windows Communication Foundation (WCF)-Dienst zum Verwalten von Kontakten und eine Datenbank für ASP.NET-Anwendungsdienste zum Verwalten der Authentifizierung und Autorisierung.
- **ContactManager.Database**. Dies ist ein Visual Studio-Datenbankprojekt. Das Projekt definiert das Schema für eine Datenbank, speichert Informationen wenden Sie sich an.
- **ContactManager.Service**. Dies ist ein WCF-Webdienstprojekt. Die WCF-Dienst verfügbar gemachten ein Endpunkt, der Aufrufern ermöglicht, führen zu erstellen, abrufen, aktualisieren und Löschvorgänge (CRUD) auf die **ContactManager** Datenbank. Der Dienst basiert auf der **ContactManager** Datenbank und die **ContactManager.Common.dll** Assembly.
- **ContactManager.Common**. Dies ist ein Klassenbibliotheksprojekt. Der WCF-Dienst basiert auf in dieser Assembly definierten Typen.

Die Lösung umfasst auch einen Projektmappenordner, der mit dem Namen veröffentlichen. Dieses enthält verschiedene benutzerdefinierte Projektdateien und Befehlsdateien, die veranschaulichen, wie Sie steuern und die Build- und Bereitstellungsprozess Prozess bearbeiten können. Diese werden weiter unten in diesem Lernprogramm ausführlicher behandelt.

Auf konzeptioneller Ebene passen die Komponenten der Projektmappe zusammen wie folgt:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Während Sie den ASP.NET-Mitgliedschaftsanbieter in ASP.NET MVC 3-Webanwendung verwendet wird, werden alle Seiten innerhalb der Webanwendung anonymen Zugriff zulassen. Dies ist jedoch deutlich keine realistische Konfiguration. Allerdings ist die Lösung einrichten auf diese Weise bereitstellen und testen die Projektmappe ohne Konfiguration der Benutzerkonten und Rollen erleichtern.


## <a name="deployment-challenges"></a>Bereitstellung

Die Kontakt-Manager-Lösung veranschaulicht verschiedene Herausforderung, die zu einer Vielzahl von Bereitstellungsszenarios gemeinsam verwendet werden:

- Die Lösung besteht aus mehreren abhängige Projekte. Sie müssen diese Projekte gleichzeitig bereitstellen.
- Verbindungszeichenfolgen und Dienstendpunkte für jede Umgebung aktualisiert werden müssen, und in vielen Fällen diese Informationen werden für den Entwickler verfügbar.
- Bei der Bereitstellung der **ContactManager** Datenbank für Staging und Produktion Umgebungen müssen Sie vorhandene Daten bei nachfolgenden Bereitstellungen beibehalten.
- Wenn Sie die Datenbank für ASP.NET-Anwendungsdienste bereitstellen, müssen Sie bereitstellen einiger Konfigurationsdaten jedoch weglassen von Benutzerdaten Konto.
- Die Projekte enthalten einige Dateien und Ordner, die nicht bereitgestellt werden sollen. Sie müssen den Bereitstellungsprozess diese Dateien und Ordner ausschließen.
- Die Lösung muss automatisierte Bereitstellung aus einem Build-Server mit Team Foundation Server (TFS) zu unterstützen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema eine allgemeine Übersicht über die Projektmappe Contact Manager bereitgestellt und identifiziert einige der inhärenten Bereitstellung Herausforderungen, die zu einer Vielzahl von Bereitstellungsszenarios gemeinsam verwendet werden. Die übrigen Themen in diesem Lernprogramm werden einige der Methoden, die Sie verwenden können, um diese Auflagen erfüllt werden beschrieben.

Im nächsten Thema [Einstellung von der Kontakt-Manager-Lösung](setting-up-the-contact-manager-solution.md), herunterladen und Ausführen der Projektmappe auf der Arbeitsstation Developer beschreibt.

> [!div class="step-by-step"]
> [Zurück](web-deployment-in-the-enterprise.md)
> [Weiter](setting-up-the-contact-manager-solution.md)
