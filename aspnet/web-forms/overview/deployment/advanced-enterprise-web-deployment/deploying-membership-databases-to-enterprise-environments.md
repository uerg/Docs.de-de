---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Bereitstellen von Datenbankrollenmitgliedschaften in Unternehmensumgebungen | Microsoft-Dokumentation
author: jrjlee
description: Dieser Artikel beschreibt die wichtigsten Überlegungen und Herausforderungen, die Sie zu umgehen, wenn Sie ASP.NET Application Services-Datenbanken (gängiger... bereitstellen müssen
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832683"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Bereitstellen von Datenbankrollenmitgliedschaften in Unternehmensumgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird erläutert, die wichtige Überlegungen und Herausforderungen, die Sie zum Lösen des Problems bereitstellen ASP.NET-Anwendung-Services-Datenbanken (häufig als Datenbankrollenmitgliedschaften bezeichnet) in Test-, Staging- und produktionsumgebungen Umgebungen benötigen. Außerdem wird beschrieben, Ansätze, die Sie verwenden können, um diese Herausforderungen zu erfüllen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Was sind die Probleme, wenn Sie eine Mitgliedschaftsdatenbank bereitstellen?

Wenn Sie eine Strategie für die Bereitstellung für eine Datenbank, entwickeln, ist zunächst, die Sie berücksichtigen müssen in den meisten Fällen welche Daten, die Sie bereitstellen möchten. In einer Entwicklungs- oder Test-Umgebung empfiehlt es sich um Benutzerkontodaten zum vereinfachten testen schnell und einfach bereitzustellen. In einer Umgebung Staging- oder produktionsumgebung ist es sehr unwahrscheinlich, dass Sie Benutzerkontodaten bereitstellen möchten.

Leider führen Datenbankrollenmitgliedschaften ASP.NET einige spezielle Aufgaben, die diese Entscheidung zu treffen, die sehr viel komplizierter:

- Eine Schema only-Bereitstellung wird die Mitgliedschaftsdatenbank in einen nicht betriebsfähigen Zustand belassen. Dies ist, da die Mitgliedschaftsdatenbank einige Konfigurationsdaten enthält (in der **Aspnet\_SchemaVersions** Tabelle), die die Datenbank benötigt, um zu funktionieren. Daher ist es möglich, wenn Sie eine Schema only-Bereitstellung einer Mitgliedschaftsdatenbank ausführen, um Benutzerkontodaten auszuschließen, müssen Sie zum Ausführen eines Skripts nach der Bereitstellung, um die erforderliche Konfigurationsdaten hinzuzufügen.
- Je nachdem, wie Ihre Mitgliedschaftsdatenbank konfiguriert ist kann der Mitgliedschaftsanbieter den Schlüssel des Computers zum Verschlüsseln von Kennwörtern, und speichern sie in der Datenbank verwenden. In diesem Fall werden alle Benutzerdaten für das Konto, die Sie mit der Datenbank bereitstellen kann, die auf dem Zielserver nicht verwendet werden. Aus diesem Grund ist das Bereitstellen von Benutzerkontodaten nicht unterstützt.

## <a name="choosing-a-membership-database-strategy"></a>Auswählen einer Strategie für die Mitgliedschaft in Datenbank

Verwenden Sie diese Richtlinien bei der Auswahl eine Mitgliedschaftsdatenbank in einer Server-unternehmensumgebung bereitstellen:

- Wo immer dies möglich ist, stellen Sie keine Mitgliedschaft Datenbanken. Erstellen Sie stattdessen die Mitgliedschaftsdatenbank manuell auf dem Zielserver für die Datenbank. Wenn Sie Ihr Datenbankschema Mitgliedschaft nicht angepasst haben, können Sie einfach erstellen eine neue in situ an der mit dem Ziel der [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Wenn keine Möglichkeit besteht jedoch eine Mitgliedschaftsdatenbank bereitzustellenden&#x2014;z. B., wenn Sie umfangreiche Änderungen am Datenbankschema vorgenommen haben&#x2014;sollten Sie eine Schema only-Bereitstellung von der Mitgliedschaftsdatenbank auszuschließende Benutzerkontodaten, ausführen und dann Führen Sie ein Skript nach der Bereitstellung, um alle erforderlichen Konfigurationsdaten hinzuzufügen. Zahlreiche allgemeine Anleitungen finden Sie auf diese Ansätze in [Vorgehensweise: Bereitstellen der ASP.NET Membership ohne einschließlich Datenbankbenutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Es ist wichtig zu beachten, dass *das Schema Ihrer Mitgliedschaft-Datenbank ist wahrscheinlich relativ statisch*. Auch wenn Sie die Mitgliedschaftsdatenbank angepasst haben, es ist unwahrscheinlich, müssen Sie zum Aktualisieren des Schemas in regelmäßigen Abständen&#x2014;geht nicht mit der gleichen Häufigkeit als der Code in eine Webanwendung oder ein Datenbankprojekt ändern. Daher sollte nicht müssen Sie automatisierte oder Schritt für Schritt Bereitstellungsprozesse die Mitgliedschaftsdatenbank einschließt.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Verwenden VSDBCMD ein Datenbankschema Mitgliedschaft aktualisieren

Wenn Sie die Struktur Ihrer Mitgliedschaft-Datenbank nach der ersten Bereitstellung ändern, sollten Sie nicht das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) zu verwenden, um die Datenbank erneut bereitzustellen. Der Datenbankfunktionalität für die Bereitstellung in Web Deploy umfasst nicht die Möglichkeit, stellen differenzielle Updates in eine Zieldatenbank&#x2014;löschen Sie stattdessen Web Deploy muss und die Datenbank neu erstellen. Dies bedeutet, dass Sie alle vorhandenen Benutzer-Konto-Daten verlieren, die in der Regel in Staging-oder produktionsumgebungen nicht erwünscht ist.

Die Alternative ist das VSDBCMD-Dienstprogramm verwenden, um das Schema der Zieldatenbank aktualisiert. VSDBCMD enthält zwei wichtige Funktionen. Zunächst können sie das Schema einer vorhandenen Datenbank in eine DBSCHEMA-Datei importieren. Andererseits können sie eine DBSCHEMA-Datei zu einer vorhandenen Datenbank als eine differenzielle Update bereitstellen Dies bedeutet, dass nur, die erforderlichen Änderungen vorgenommen an die Zieldatenbank, die auf dem neuesten Stand zu bringen und Sie keine Daten verlieren.

Sie können diese allgemeinen Schritte verwenden, um ein Datenbankschema Mitgliedschaft zu aktualisieren:

1. Verwenden Sie die VSDBCMD **Import** Aktion aus, um eine DBSCHEMA-Datei für die Quelldatenbank für die Mitgliedschaft zu generieren. Dieses Verfahren wird beschrieben, [Vorgehensweise: Importieren eines Schemas an einer Eingabeaufforderung](https://msdn.microsoft.com/library/dd172135.aspx).
2. Verwenden Sie die VSDBCMD **bereitstellen** Aktion aus, um die DBSCHEMA-Datei für die Zieldatenbank für die Gruppenmitgliedschaft bereitstellen. Dieses Verfahren wird beschrieben, [Befehlszeilenreferenz für VSDBCMD. EXE-Datei ("Bereitstellung" und "Schema importieren")](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben einige der Herausforderungen, die Sie möglicherweise stoßen, wenn Sie ASP.NET Membership-Datenbanken in verschiedenen zielumgebungen bereitstellen möchten. Insbesondere erläutert er, weshalb Schema only-Bereitstellungen die Mitgliedschaftsdatenbank in einen nicht betriebsfähigen Zustand belassen werden, und warum das Bereitstellen von Benutzerkontodaten nicht unterstützt wird. Das Thema präsentiert auch Anleitungen zum Bereitstellen und Bereitstellen von Datenbankrollenmitgliedschaften in verschiedenen Szenarien zu aktualisieren.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Anleitungen und Beispiele zur Verwendung von VSDBCMD finden Sie unter [Befehlszeilenreferenz für VSDBCMD. EXE-Datei ("Bereitstellung" und "Schema importieren")](https://msdn.microsoft.com/library/dd193283.aspx) und [Vorgehensweise: Importieren eines Schemas an einer Eingabeaufforderung](https://msdn.microsoft.com/library/dd172135.aspx). Weitere Informationen zur Verwendung von Aspnet\_regsql.exe zum Erstellen von Datenbanken mit Mitgliedschaft, finden Sie unter [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Eine allgemeine Anleitung zum Bereitstellen von Datenbankrollenmitgliedschaften, finden Sie unter [Vorgehensweise: Bereitstellen der ASP.NET Membership ohne einschließlich Datenbankbenutzerkonten](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Zurück](deploying-database-role-memberships-to-test-environments.md)
> [Weiter](excluding-files-and-folders-from-deployment.md)
