---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Bereitstellen von Datenbankrollenmitgliedschaften in Testumgebungen | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema beschreibt, wie Sie Benutzerkonten, Datenbankrollen, die als Teil einer Bereitstellung in einer testumgebung hinzufügen. Beim Bereitstellen einer Lösung mit...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a690d99df7a19c422fb217544ec183c311d1796f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828032"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Bereitstellen von Datenbankrollenmitgliedschaften in Testumgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie Benutzerkonten, Datenbankrollen, die als Teil einer Bereitstellung in einer testumgebung hinzufügen.
> 
> Wenn Sie eine Projektmappe mit einem Datenbankprojekt in einer Umgebung Staging- oder produktionsumgebung bereitstellen, möchten Sie nicht in der Regel den Entwickler das Hinzufügen von Benutzerkonten, Datenbankrollen zu automatisieren. Klicken Sie in den meisten Fällen der Entwickler weiß nicht, welche Benutzerkonten müssen die Datenbankrollen hinzugefügt werden, und diese Anforderungen können sich jederzeit ändern. Beim Bereitstellen einer Projektmappe mit einem Datenbankprojekt eine Entwicklungs- oder testumgebung, unterscheidet sich jedoch die Situation in der Regel Recht:
> 
> - Der Entwickler bereitgestellt erneut Lösung, in der Regel in regelmäßigen Abständen, häufig mehrere Male pro Tag.
> - Die Datenbank wird in der Regel bei jeder Bereitstellung, neu erstellt das bedeutet, dass der Datenbankbenutzer erstellt und nach jeder Bereitstellung Rollen hinzugefügt werden müssen.
> - Der Entwickler hat in der Regel Vollzugriff auf die zielumgebung für Entwicklungs- oder testumgebung.
> 
> In diesem Szenario ist es oft vorteilhaft, automatisch erstellen von Datenbankbenutzern und Zuweisen von Datenbank-Rollenmitgliedschaften im Rahmen des Bereitstellungsprozesses.
> 
> Der wichtigste Faktor ist, dass dieser Vorgang bedingte werden basierend auf der zielumgebung muss. Wenn Sie in ein Staging- oder in einer produktionsumgebung bereitstellen möchten, möchten Sie den Vorgang zu überspringen. Wenn Sie für einen Entwickler bereitstellen oder testumgebung, möchten Sie Rollenmitgliedschaften ohne weiteren Eingriff bereitstellen. Dieses Thema beschreibt eine Möglichkeit, die Sie verwenden können, um diese Herausforderungen zu meistern.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In diesem Thema wird vorausgesetzt, dass:

- Verwenden Sie den Split-Projekt-Datei-Ansatz für die Bereitstellung der Lösung, wie in beschrieben [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Rufen Sie VSDBCMD aus der Projektdatei, um das Datenbankprojekt bereitstellen wie in beschrieben [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Zum Erstellen von Datenbankbenutzern und Rollenmitgliedschaften zuweisen, wenn Sie ein Datenbankprojekt in einer testumgebung bereitstellen, müssen Sie Folgendes ausführen:

- Erstellen Sie ein Transact Structured Query Language (Transact-SQL)-Skript, das die notwendige Änderungen vorgenommen.
- Erstellen Sie ein Ziel von Microsoft Build Engine (MSBuild), die das Hilfsprogramm sqlcmd.exe verwendet, um das SQL-Skript auszuführen.
- Konfigurieren Sie Ihre Projektdateien, um das Ziel aufgerufen wird, wenn Sie Ihre Lösung in einer testumgebung bereitstellen.

In diesem Thema werden Sie zum Durchführen dieser Verfahren erläutert.

## <a name="scripting-the-database-role-memberships"></a>Die Mitgliedschaften in Datenbankrollen-Skripterstellung

Können Sie ein Transact-SQL-Skript in viele verschiedene Arten erstellen, und wählen Sie in einem beliebigen Speicherort. Der einfachste Ansatz ist die Erstellung des Skripts innerhalb der Projektmappe in Visual Studio 2010.

**Erstellen eines SQL-Skripts**

1. In der **Projektmappen-Explorer** Fenster, erweitern Sie Ihr Datenbankprojekt-Knoten.
2. Mit der rechten Maustaste die **Skripts** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neuer Ordner**.
3. Typ **Test** als Name des Ordners aus, und drücken Sie dann die EINGABETASTE.
4. Mit der rechten Maustaste die **Test** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Skript**.
5. Dies ist in der Regel hilfreich, wenn Sie regelmäßig neu, eine Datenbank in einer testumgebung erstellen, aber es in der Regel vermieden werden, sollte Wenn Sie Datenbanken in Staging-oder produktionsumgebung bereitstellen.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. Daher sollten Sie sicherstellen, dass Sie die erforderliche bedingte Logik verwenden, sodass Datenbankbenutzer und Rollenmitgliedschaften nur erstellt werden, wenn es dazu geeignet ist.

    1. Weitere Informationen zur Verwendung von VSDBCMD Datenbankprojekte bereitstellen, finden Sie unter Bereitstellen von Datenbankprojekten.
    2. Anleitungen zum Anpassen von datenbankbereitstellungen für unterschiedliche zielumgebungen finden Sie unter Anpassen von Datenbankbereitstellungen für mehrere Umgebungen.
7. Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter Grundlegendes zur Projektdatei und Verständnis des Prozesses erstellen.

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Speichern Sie die Datei.

## <a name="executing-the-script-on-the-target-database"></a>Ausführen des Skripts in der Zieldatenbank

Im Idealfall würden Sie alle erforderlichen Transact-SQL-Skripts als Teil eines Skripts nach der Bereitstellung ausführen, wenn Sie das Datenbankprojekt bereitstellen. Skripts nach der Bereitstellung nicht allerdings bedingt auf Grundlage Projektmappenkonfigurationen oder Buildeigenschaften Logik ausgeführt werden können. Die Alternative besteht darin, führen Sie die SQL-Skripts direkt von der MSBuild-Projektdatei, durch das Erstellen einer **Ziel** -Element, das ein sqlcmd.exe-Befehl ausgeführt wird. Sie können diesen Befehl verwenden, um Ihr Skript in der Zieldatenbank:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Weitere Informationen zu den Sqlcmd-Befehlszeilenoptionen finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).


Bevor Sie mit diesem Befehl in einem MSBuild-Ziel einbetten, müssen Sie berücksichtigen, unter welchen Bedingungen das Skript ausgeführt werden sollen:

- Die Zieldatenbank muss vorhanden sein, bevor Sie die Rollenmitgliedschaften ändern. Daher müssen Sie zum Ausführen dieses Skripts *nach* der datenbankbereitstellung.
- Sie müssen eine Bedingung enthalten, sodass das Skript nur für testumgebungen ausgeführt wird.
- Wenn Sie eine "Was-wäre-wenn"-Bereitstellung ausführen&#x2014;in anderen Worten: Wenn Sie Bereitstellungsskripts generiert sind, aber nicht tatsächlich werden ausgeführt&#x2014;Sie sollte nicht das SQL-Skript ausführen.

Bei Verwendung in beschriebenen Ansatz der geteilten Projekt Datei [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), wie von der beispiellösung der Contact Manager-veranschaulicht, können Sie die Buildanweisungen für Ihre SQL-Skript folgendermaßen Teilen:

- Alle erforderlichen umgebungsspezifische Eigenschaften zusammen mit der Eigenschaft, die bestimmt, ob Berechtigungen, bereitgestellt in der Projektdatei auf umgebungsspezifische gesendet werden sollen (z. B. *Env-Dev.proj*).
- Das MSBuild-Ziel, zusammen mit allen Eigenschaften, die nicht zwischen zielumgebungen, ändern in der Projektdatei für die universelle gesendet werden sollen (z. B. *Publish.proj*).

In der Projektdatei umgebungsspezifische müssen Sie definieren den Namen des Datenbankservers, den Namen der Zieldatenbank und eine boolesche Eigenschaft, die dem Benutzer, die angeben, ob Rollenmitgliedschaften bereitstellen kann.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


In der Datei universal-Projekt müssen Sie angeben, den Speicherort der ausführbaren Datei "Sqlcmd" und den Speicherort des SQL-Skripts, die Sie ausführen möchten. Diese Eigenschaften bleiben unabhängig von der zielumgebung. Sie müssen außerdem erstellen Sie ein MSBuild-Ziel, um den Sqlcmd-Befehl auszuführen.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Beachten Sie, dass Sie als statische Eigenschaft, die den Speicherort der ausführbaren Datei "Sqlcmd" hinzufügen, wie dies für andere Ziele nützlich sein könnte. Im Gegensatz dazu definieren Sie den Speicherort der Ihr SQL-Skript und die Syntax der Sqlcmd-Befehl als dynamische Eigenschaften in das Ziel, da sie nicht benötigen, bevor das Ziel ausgeführt wird. In diesem Fall die **DeployTestDBPermissions** Ziel wird nur ausgeführt werden, wenn diese Bedingungen erfüllt sind:

- Die **DeployTestDBRoleMemberships** -Eigenschaftensatz auf **"true"**.
- Der Benutzer wurde nicht angegeben. ein **"WhatIf" = "true"** Flag.

Vergessen Sie schließlich nicht, das Ziel aufzurufen. In der *Publish.proj* -Datei, Sie können hierzu durch Hinzufügen des Ziels, um die Liste der Abhängigkeiten für den standardmäßigen **FullPublish** Ziel. Sie müssen sicherstellen, dass die **DeployTestDBPermissions** Ziel wird nicht ausgeführt, bis die **PublishDbPackages** Ziel ausgeführt wurde.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, eine Möglichkeit, die in die Sie hinzufügen können Datenbankbenutzer und Rollenmitgliedschaften als Aktion nach der Bereitstellung, wenn Sie ein Datenbankprojekt bereitstellen. Dies ist in der Regel hilfreich, wenn Sie regelmäßig neu, eine Datenbank in einer testumgebung erstellen, aber es in der Regel vermieden werden, sollte Wenn Sie Datenbanken in Staging-oder produktionsumgebung bereitstellen. Daher sollten Sie sicherstellen, dass Sie die erforderliche bedingte Logik verwenden, sodass Datenbankbenutzer und Rollenmitgliedschaften nur erstellt werden, wenn es dazu geeignet ist.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zur Verwendung von VSDBCMD Datenbankprojekte bereitstellen, finden Sie unter [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Anpassen von datenbankbereitstellungen für unterschiedliche zielumgebungen finden Sie unter [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu den Sqlcmd-Befehlszeilenoptionen finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Zurück](customizing-database-deployments-for-multiple-environments.md)
> [Weiter](deploying-membership-databases-to-enterprise-environments.md)
