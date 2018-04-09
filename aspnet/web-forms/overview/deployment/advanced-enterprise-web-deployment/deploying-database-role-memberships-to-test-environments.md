---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Bereitstellen von Datenbank-Rollenmitgliedschaften für Testumgebungen | Microsoft Docs
author: jrjlee
description: Dieses Thema beschreibt, wie Benutzerkonten Datenbankrollen als Teil einer Bereitstellung von Projektmappen in einer testumgebung hinzufügen. Beim Bereitstellen einer Lösung mit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 4f635153213b0695d7d4b64d09adefaf8ee8e892
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Bereitstellen von Datenbank-Rollenmitgliedschaften für Testumgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Benutzerkonten Datenbankrollen als Teil einer Bereitstellung von Projektmappen in einer testumgebung hinzufügen.
> 
> Beim Bereitstellen einer Projektmappe ein Datenbankprojekt in einer Staging- oder Produktionsserver-Umgebung mit sollen nicht in der Regel den Entwickler, die das Hinzufügen von Benutzerkonten für Datenbankrollen automatisieren. In den meisten Fällen der Entwickler weiß nicht, welche Benutzerkonten müssen die Datenbankrollen hinzugefügt werden, und diese Anforderungen können jederzeit ändern. Beim Bereitstellen einer Projektmappe mit einem Datenbankprojekt eine Entwicklungs- oder testumgebung, ist die Situation in der Regel sehr unterschiedliche:
> 
> - Die erneut bereitstellen in der Regel der Lösung in regelmäßigen Abständen, häufig mehrmals am Tag.
> - Die Datenbank wird in der Regel bei jeder Bereitstellung neu erstellt das bedeutet, dass der Datenbankbenutzer erstellt und nach jeder Bereitstellung Rollen hinzugefügt werden müssen.
> - Normalerweise hat der Entwickler die vollständige Kontrolle über die zielumgebung für Entwicklungs- oder Testserver.
> 
> In diesem Szenario ist es häufig von Vorteil automatisch Datenbankbenutzer erstellen und Zuweisen von Mitgliedschaften in der Datenbankrolle im Rahmen des Bereitstellungsprozesses.
> 
> Der wichtigste Faktor ist, dass dieser Vorgang angehören soll bedingten basierend auf der zielumgebung. Wenn Sie auf einem Staging- oder in einer produktiven Umgebung bereitstellen, möchten Sie überspringen Sie den Vorgang. Wenn Sie dem Entwickler bereitstellen oder Umgebung testen, sollten Sie Rollenmitgliedschaften ohne weiteren Eingriff bereitzustellen. In diesem Thema wird beschrieben, ein Ansatz besteht darin, die Sie verwenden können, um dieser Herausforderung zu begegnen.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In diesem Thema wird Folgendes vorausgesetzt:

- Verwenden Sie die Teilung Projekt Datei Vorgehensweise bei der Bereitstellung von Projektmappen, wie in beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Sie rufen VSDBCMD aus der Projektdatei, um das Datenbankprojekt bereitstellen, wie in beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Zum Erstellen von Datenbankbenutzern und Rollenmitgliedschaften zuweisen, wenn Sie ein Datenbankprojekt in einer testumgebung bereitstellen, müssen Sie Folgendes ausführen:

- Erstellen Sie ein Transact Structured Query Language (Transact-SQL)-Skript, das die notwendige Änderungen vornimmt.
- Erstellen Sie ein Microsoft Build Engine (MSBuild)-Ziel, das das Hilfsprogramm sqlcmd.exe verwendet, um das SQL-Skript auszuführen.
- Konfigurieren Sie die Projektdateien zum Ziel aufrufen, wenn Sie die Projektmappe in einer testumgebung bereitstellen.

In diesem Thema erfahren Sie, wie Sie jede der folgenden Verfahren ausführen.

## <a name="scripting-the-database-role-memberships"></a>Die Mitgliedschaften in Datenbankrollen Scripting

Können Sie ein Transact-SQL-Skript in viele verschiedene Arten erstellen, und wählen Sie in einem beliebigen Speicherort. Der einfachste Ansatz ist die Erstellung des Skripts innerhalb der Projektmappe in Visual Studio 2010.

**So erstellen ein SQL-Skript**

1. In der **Projektmappen-Explorer** Fenster, erweitern Sie Ihr Datenbankprojekt-Knoten.
2. Mit der rechten Maustaste die **Skripts** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neuer Ordner**.
3. Typ **Test** als Name des Ordners aus, und drücken Sie dann die EINGABETASTE.
4. Mit der rechten Maustaste die **Test** Ordner, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Skript**.
5. In der **neues Element hinzufügen** Dialogfeld Feld, geben Sie dem Skript einen aussagekräftigen Namen (z. B. **AddRoleMemberships.sql**), und klicken Sie dann auf **hinzufügen**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. In der *AddRoleMemberships.sql* Datei, Hinzufügen von Transact-SQL-Anweisungen, die:

    1. Erstellen Sie einen Datenbankbenutzer für die SQL Server-Anmeldung, die auf Ihre Datenbank zugreifen.
    2. Den Datenbankbenutzer und alle erforderlichen Datenbankrollen hinzugefügt.
7. Die Datei sollte diesem ähneln:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Speichern Sie die Datei.

## <a name="executing-the-script-on-the-target-database"></a>Ausführen des Skripts in der Zieldatenbank

Im Idealfall würden Sie alle erforderlichen Transact-SQL-Skripts als Teil eines Skripts nach der Bereitstellung ausführen, wenn Sie das Datenbankprojekt bereitstellen. Allerdings zulassen nicht Skripts nach der Bereitstellung Logik bedingt auf Grundlage Projektmappenkonfigurationen oder Buildeigenschaften Ausführung. Die Alternative besteht darin, durch das Erstellen der SQL-Skripts direkt aus der MSBuild-Projektdatei Ausführen einer **Ziel** Element, das einen sqlcmd.exe-Befehl ausführt. Mit diesem Befehl können Sie das Skript in der Zieldatenbank ausführen:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen von "Sqlcmd", finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).


Bevor Sie diesen Befehl in einer MSBuild-Ziel einbetten, müssen Sie berücksichtigen, unter welchen Bedingungen das Skript ausgeführt werden soll:

- Die Zieldatenbank muss vorhanden sein, bevor Sie die Rollenmitgliedschaften ändern. Daher müssen Sie dieses Skript ausführen *nach* der datenbankbereitstellung.
- Sie müssen eine Bedingung enthalten, sodass das Skript nur für testumgebungen ausgeführt wird.
- Wenn Sie eine "Was-wäre-wenn" Bereitstellung ausführen&#x2014;heißt, wenn Sie Bereitstellungsskripts generieren aber nicht tatsächlich ressourcenaufwändig&#x2014;Sie darf nicht das SQL-Skript ausführen.

Bei Verwendung in beschriebenen Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), wie der Kontakt-Manager-Beispielprojektmappe zeigt, können Sie in den erstellungsanweisungen aufteilen, für das SQL-Skript wie folgt:

- Alle erforderlichen umgebungsspezifische Eigenschaften zusammen mit der Eigenschaft, die bestimmt, ob Berechtigungen bereitstellen sollten in der Projektdatei umgebungsspezifische wechseln (z. B. *Env Dev.proj*).
- Das MSBuild-Ziel selbst sowie alle Eigenschaften, die nicht zwischen zielumgebungen, ändern sollte in der universelle Projektdatei wechseln (z. B. *Publish.proj*).

In der Projektdatei umgebungsspezifische müssen Sie definieren den Namen des Datenbankservers, den Namen der Zieldatenbank und eine boolesche Eigenschaft, die dem Benutzer, die angeben, ob Sie Rollenmitgliedschaften bereitstellen kann.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


In der Datei universelles Projekt müssen Sie geben den Speicherort der des ausführbaren "Sqlcmd" und den Speicherort des SQL-Skripts, die Sie ausführen möchten. Diese Eigenschaften bleiben unabhängig von der zielumgebung verbunden. Sie müssen außerdem eine MSBuild-Ziel zum Ausführen von Sqlcmd-Befehl zu erstellen.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Beachten Sie, dass Sie den Speicherort der ausführbaren SQLCMD als eine statische Eigenschaft hinzufügen, wie dies nützlich, um andere Ziele werden konnte. Im Gegensatz dazu definieren Sie den Speicherort der SQL-Skript und die Syntax der Sqlcmd-Befehl als dynamische Eigenschaften in das Ziel, da sie nicht erforderlich sind, bevor das Ziel ausgeführt wird. In diesem Fall die **DeployTestDBPermissions** Ziel wird nur ausgeführt werden, wenn diese Bedingungen erfüllt sind:

- Die **DeployTestDBRoleMemberships** -Eigenschaftensatz auf **"true"**.
- Der Benutzer wurde nicht angegeben. ein **"WhatIf" = "true"** Flag.

Schließlich, vergessen Sie nicht, die Ziel aufzurufen. In der *Publish.proj* Datei hierzu können Sie durch Hinzufügen des Ziels, um die Liste der Abhängigkeiten für den standardmäßigen **FullPublish** Ziel. Sie müssen sicherstellen, dass die **DeployTestDBPermissions** Ziel nicht ausgeführt, bis die **PublishDbPackages** Ziel ausgeführt wurde.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben eine Möglichkeit, in dem Sie hinzufügen können Datenbankbenutzer und Rollenmitgliedschaften als eine Aktion nach der Bereitstellung, wenn Sie ein Datenbankprojekt bereitstellen. Dies ist in der Regel hilfreich, wenn Sie regelmäßig neu, eine Datenbank in einer testumgebung erstellen, aber es in der Regel vermieden werden, sollte Wenn Sie Datenbanken auf Staging-oder produktionsumgebung bereitstellen. Daher sollten Sie sicherstellen, dass Sie die erforderliche bedingte Logik verwenden, sodass Datenbankbenutzer und Rollenmitgliedschaften nur erstellt werden, wenn er dazu geeignet ist.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zur Verwendung von VSDBCMD Datenbankprojekte bereitstellen, finden Sie unter [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md). Anleitungen zum Anpassen von Datenbank-Bereitstellungen für unterschiedliche zielumgebungen finden Sie unter [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](customizing-database-deployments-for-multiple-environments.md). Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu Befehlszeilenoptionen von "Sqlcmd", finden Sie unter [Hilfsprogramms "Sqlcmd"](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Zurück](customizing-database-deployments-for-multiple-environments.md)
> [Weiter](deploying-membership-databases-to-enterprise-environments.md)
