---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Bereitstellen von Datenbanken Mitgliedschaft in Enterprise-Umgebungen | Microsoft Docs
author: jrjlee
description: Dieser Artikel beschreibt die wichtigsten Überlegungen und Herausforderungen, die Sie benötigen, zu umgehen, wenn Sie ASP.NET Anwendung-Services-Datenbanken (üblicher... bereitstellen
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Bereitstellen von Datenbanken Mitgliedschaft in Enterprise-Umgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird erläutert, die wichtige Überlegungen und Herausforderungen, die Sie vermeiden, wenn Sie ASP.NET-Anwendung Datenbanken (häufiger Mitgliedschaft Datenbanken genannt) in Test-, Staging- oder produktionsumgebung Umgebungen für Bereitstellungsdienste benötigen. Es beschreibt auch Ansätze, die Sie verwenden können, um diese Auflagen erfüllt werden.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Was sind die Probleme, wenn Sie eine Mitgliedschaftsdatenbank bereitstellen?

Wenn Sie eine Bereitstellungsstrategie für eine Datenbank auszutauschen, wird im ersten Schritt müssen Sie berücksichtigen in den meisten Fällen welche Daten, die Sie bereitstellen möchten. In einer Umgebung Entwicklungs- oder Testserver können Sie bereitzustellende Benutzerkontodaten zum Testen der schnell und einfach zu vereinfachen. In einer Staging- oder Produktionsserver Umgebung ist es sehr unwahrscheinlich, dass Sie Kontodaten für Benutzer bereitstellen möchten.

Leider wurde für ASP.NET Membership Datenbanken bestimmten Herausforderungen, die diese Entscheidung zu treffen, die viel komplexere eingeführt:

- Eine Schema only-Bereitstellung wird die Mitgliedschaftsdatenbank in einen nicht betriebsfähigen Zustand lassen. Dies ist, da die Mitgliedschaftsdatenbank einige Konfigurationsdaten enthält (in der **Aspnet\_SchemaVersions** Tabelle), die die Datenbank erforderlich, um zu funktionieren. Wenn Sie eine Schema only Bereitstellung einer Mitgliedschaftsdatenbank ausführen, um Benutzerkontodaten ausschließen, müssen Sie daher führen Sie ein Skript nach der Bereitstellung, um die notwendige Konfigurationsdaten hinzuzufügen.
- Abhängig von der Konfiguration der Mitgliedschaftsdatenbank kann der Mitgliedschaftsanbieter den Computerschlüssel verwenden, um Kennwörter verschlüsselt und in der Datenbank zu speichern. In diesem Fall werden keine Benutzerdaten für das Konto, die Sie, mit der Datenbank bereitstellen auf dem Zielserver unbrauchbar werden. Aus diesem Grund ist das Bereitstellen von Benutzerdaten-Konto nicht unterstütztes Szenario.

## <a name="choosing-a-membership-database-strategy"></a>Auswählen einer Strategie für die Datenbank Mitgliedschaft

Verwenden Sie diese Richtlinien, wenn Sie eine Mitgliedschaftsdatenbank in einer Server-unternehmensumgebung bereitstellen auswählen:

- Nach Möglichkeit sollten Sie die Mitgliedschaft Datenbanken nicht bereitstellen. Erstellen Sie stattdessen die Mitgliedschaftsdatenbank manuell auf dem Zielserver für die Datenbank. Wenn Sie das Datenbankschema Mitgliedschaft angepasst haben, können Sie einfach erstellen ein neues Konto in situ auf dem Ziel mithilfe der [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Wenn keine Möglichkeit besteht jedoch zum Bereitstellen einer Mitgliedschaftsdatenbank&#x2014;z. B., wenn Sie eine umfangreiche Änderungen am Datenbankschema vorgenommen haben&#x2014;sollten Sie eine Schema only Bereitstellung der Mitgliedschaftsdatenbank auszuschließende Benutzerkontodaten, ausführen und dann Führen Sie ein Skript nach der Bereitstellung erforderlichen Konfigurationsdaten hinzufügen. Umfassende Anleitung finden Sie auf diese Ansätze in [Vorgehensweise: Bereitstellen der ASP.NET Membership ohne einschließlich Datenbankbenutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Es ist wichtig zu beachten, dass *das Schema der Mitgliedschaftsdatenbank ist wahrscheinlich relativ statisch sein*. Auch wenn Sie die Mitgliedschaftsdatenbank angepasst haben, es ist sehr unwahrscheinlich, müssen Sie zum Aktualisieren des Schemas in regelmäßigen Abständen&#x2014;es ist nicht mit der gleichen Häufigkeit wie der Code in einer Webanwendung oder ein Datenbankprojekt ändern möchten. Daher müssen Sie darf kein automatisierte oder einstufiger-bereitstellungstechnologien die Mitgliedschaftsdatenbank einschließt.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Verwenden VSDBCMD ein Datenbankschema Mitgliedschaft aktualisieren

Wenn Sie die Struktur Ihrer Mitgliedschaftsdatenbank nach der ersten Bereitstellung ändern, möchten Sie möglicherweise nicht die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zu verwenden, um die Datenbank erneut bereitzustellen. Die Datenbank neue Bereitstellungsfunktion in Web Deploy ist nicht die Möglichkeit, stellen differenzielle Updates zu einer Zieldatenbank enthalten&#x2014;stattdessen löschen und Neuerstellen die Datenbank muss Web Deploy. Dies bedeutet, dass alle vorhandenen Benutzer Kontodaten verloren gehen, die wodurch die in der Regel in der Staging-oder produktionsumgebung nicht erwünscht ist.

Die Alternative ist die Verwendung des Hilfsprogramms VSDBCMD um das Schema der Zieldatenbank zu aktualisieren. VSDBCMD umfasst zwei wichtige Funktionen. Zunächst können sie das Schema einer vorhandenen Datenbank in eine DBSCHEMA-Datei importieren. Zweitens können sie eine DBSCHEMA-Datei zu einer vorhandenen Datenbank als eine differenzielle Updates bereitstellen d. h. es werden nur Änderungen erforderlich, um die Zieldatenbank, die auf dem neuesten Stand zu bringen und Sie verlieren Sie keine Daten.

Sie können diese allgemeinen Schritte aus verwenden, um ein Datenbankschema Mitgliedschaft aktualisieren:

1. Verwenden Sie die VSDBCMD **Import** Aktion aus, um eine DBSCHEMA-Datei für die Quelldatenbank für die Mitgliedschaft zu generieren. Hierin wird beschrieben, [Vorgehensweise: Importieren eines Schemas von einer Eingabeaufforderung](https://msdn.microsoft.com/library/dd172135.aspx).
2. Verwenden Sie die VSDBCMD **bereitstellen** Aktion aus, um die DBSCHEMA-Datei für Ihre Mitgliedschaft Zieldatenbank bereitstellen. Hierin wird beschrieben, [Command-Line Reference for VSDBCMD. EXE-Datei (Bereitstellung und Schemaimport)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschriebenen einige der Herausforderungen, die Sie möglicherweise stoßen, wenn Sie ASP.NET Membership-Datenbanken in verschiedenen zielumgebungen bereitstellen müssen. Insbesondere erläutert es Warum Schema only-Bereitstellungen die Mitgliedschaftsdatenbank in einen nicht betriebsfähigen Zustand belassen werden, und warum Bereitstellen von Benutzerdaten-Konto nicht unterstützt wird. Das Thema präsentiert auch Anleitungen zum Bereitstellen, bereitstellen und Aktualisieren von Mitgliedschaft Datenbanken in verschiedenen Szenarien.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Hinweise und Beispiele zur Verwendung von VSDBCMD finden Sie unter [Command-Line Reference for VSDBCMD. EXE-Datei (Bereitstellung und Schemaimport)](https://msdn.microsoft.com/library/dd193283.aspx) und [Vorgehensweise: Importieren eines Schemas von einer Eingabeaufforderung](https://msdn.microsoft.com/library/dd172135.aspx). Weitere Informationen zur Verwendung von Aspnet\_regsql.exe zum Erstellen von Datenbanken von Mitgliedschaft, finden Sie unter [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Allgemeineren Leitfaden zum Bereitstellen von Mitgliedschaft Datenbanken finden Sie unter [Vorgehensweise: Bereitstellen der ASP.NET Membership ohne einschließlich Datenbankbenutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Zurück](deploying-database-role-memberships-to-test-environments.md)
> [Weiter](excluding-files-and-folders-from-deployment.md)
