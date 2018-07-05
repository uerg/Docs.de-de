---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Anpassen von Datenbankbereitstellungen für mehrere Umgebungen | Microsoft-Dokumentation
author: jrjlee
description: 'In diesem Thema wird beschrieben, wie Sie die Eigenschaften einer Datenbank zu bestimmten zielumgebungen im Rahmen des Bereitstellungsprozesses anpassen können. Hinweis: Das Thema wird davon ausgegangen te...'
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 2ecd73e4eb3cb00545b5ba090ac5f8428586941b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804292"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Anpassen von Datenbankbereitstellungen für mehrere Umgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie die Eigenschaften einer Datenbank zu bestimmten zielumgebungen im Rahmen des Bereitstellungsprozesses anpassen können.
> 
> > [!NOTE]
> > Das Thema wird davon ausgegangen, dass Sie ein Visual Studio 2010-Datenbankprojekt mithilfe von MSBuild.exe und VSDBCMD.exe bereitstellen möchten. Weitere Informationen dazu, warum Sie diesen Ansatz entscheiden können, finden Sie unter [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) und [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Wenn Sie ein Datenbankprojekt an mehrere Ziele bereitstellen, werden Sie häufig Eigenschaften zur datenbankbereitstellung für jede zielumgebung anpassen möchten. Beispielsweise würde in testumgebungen Sie in der Regel die Datenbank bei jeder Bereitstellung, neu erstellen in Staging-oder produktionsumgebung Sie damit inkrementelle Updates für Ihre Daten zu erhalten sehr viel wahrscheinlicher wäre.
> 
> In einem Visual Studio 2010-Datenbankprojekt sind die bereitstellungseinstellungen in einer Bereitstellungskonfigurationsdatei (.sqldeployment) enthalten. In diesem Thema erfahren Sie, wie zum Erstellen von umgebungsspezifische bereitstellungs-Konfigurationsdateien, und geben Sie den Namespace, die, den Sie als Parameter VSDBCMD verwenden möchten.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In diesem Thema wird vorausgesetzt, dass:

- Verwenden Sie den Split-Projekt-Datei-Ansatz für die Bereitstellung der Lösung, wie in beschrieben [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Rufen Sie VSDBCMD aus der Projektdatei, um das Datenbankprojekt bereitstellen wie in beschrieben [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Um ein Bereitstellungssystem zu erstellen, die unterstützt wird, variieren die Eigenschaften für die datenbankbereitstellung zwischen zielumgebungen, müssen Sie Folgendes ausführen:

- Erstellen einer Bereitstellungskonfigurationsdatei (.sqldeployment) für jede zielumgebung an.
- Erstellen Sie einen VSDBCMD-Befehl, der angibt, die Bereitstellungskonfigurationsdatei als eines Befehlszeilenschalters.
- Parametrisieren Sie die VSDBCMD-Befehl in einer Projektdatei Microsoft Build Engine (MSBuild), sodass die VSDBCMD-Optionen für die zielumgebung geeignet sind.

In diesem Thema werden Sie zum Durchführen dieser Verfahren erläutert.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Erstellen die umgebungsspezifische Konfiguration Bereitstellungsdateien

Standardmäßig enthält ein Datenbankprojekt mit dem Namen eine einzelnen Bereitstellung-Konfigurationsdatei *Database.sqldeployment*. Wenn Sie diese Datei in Visual Studio 2010 öffnen, sehen Sie die verschiedenen Bereitstellungsoptionen, die Ihnen zur Verfügung stehen:

- **Vergleich von Bereitstellungssortierreihenfolge**. Dadurch können Sie entscheiden, ob die Sortierung der Datenbank des Projekts verwenden (die *Quelle* Sortierung) oder die Sortierung der Datenbank des Zielservers (die *Ziel* Sortierung). In den meisten Fällen sollten Sie die quellsortierung zu verwenden, wenn Sie für eine Entwicklung bereitstellen oder testumgebung. Wenn Sie in einer Staging- oder produktionsumgebung bereitstellen, sollten Sie in der Regel die zielsortierung zur Vermeidung von jegliche Interoperabilitätsprobleme unverändert lassen.
- **Datenbankeigenschaften bereitstellen**. Dadurch können Sie auswählen, ob die Eigenschaften, anzuwendende gemäß der *Database.sqlsettings* Datei. Wenn Sie eine Datenbank zum ersten Mal bereitstellen, sollten Sie die Datenbankeigenschaften bereitstellen. Wenn Sie eine vorhandene Datenbank aktualisieren, die Eigenschaften sollte bereits eingerichtet sein, und Sie sollte nicht erneut bereitstellen müssen.
- **Datenbank immer neu erstellen**. So können Sie auswählen, ob die Zieldatenbank erneut zu erstellen, jedes Mal, wenn Sie bereitstellen, oder stellen inkrementelle Änderungen an die Zieldatenbank zu bringen. mit Ihr Schema auf dem neuesten Stand. Wenn Sie die Datenbank neu erstellt, verlieren Sie alle Daten in der vorhandenen Datenbank. Daher ist es möglich, Sie in der Regel legen Sie hier **"false"** für Bereitstellungen in Staging-oder produktionsumgebungen.
- **Inkrementelle Bereitstellung blockieren, wenn Datenverlust auftreten könnte**. Dadurch können Sie auswählen, ob die Bereitstellung beendet werden soll, wenn eine Änderung am Datenbankschema Datenverlust verursacht werden. Sie in der Regel legen Sie diese auf **"true"** für eine Bereitstellung in einer produktionsumgebung bereit, um die Möglichkeit, eingreifen und schützen wichtigen Daten ermöglichen. Wenn Sie festgelegt haben **Datenbank immer neu erstellen** zu **"false"**, diese Einstellung hat keine Auswirkungen.
- **Führen Sie die Bereitstellung in den Einzelbenutzermodus**. Dies ist nicht in der Regel ein Problem in Entwicklungs- oder testumgebungen. Allerdings sollten Sie in der Regel dadurch auf festlegen **"true"** für Bereitstellungen in Staging-oder produktionsumgebungen. Dadurch wird verhindert, dass Benutzer Änderungen an der Datenbank, während die Bereitstellung ausgeführt wird.
- **Sichern Sie die Datenbank vor der Bereitstellung**. Sie in der Regel legen Sie diese auf **"true"** bei der Bereitstellung in einer produktionsumgebung als vorsichtmaßnahme gegen Datenverlust. Sie möchten auch festgelegt ist, dass **"true"** bei der Bereitstellung in einer Stagingumgebung, wenn die staging-Datenbank eine große Datenmenge enthält.
- **Generieren von DROP-Anweisungen für Objekte, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt**. In den meisten Fällen ist dies ein wesentlicher und notwendiger Teil inkrementelle Änderungen an einer Datenbank. Wenn Sie festgelegt haben **Datenbank immer neu erstellen** zu **"false"**, diese Einstellung hat keine Auswirkungen.
- **Verwenden Sie ALTER ASSEMBLY-Anweisungen zum Aktualisieren von CLR-Typen nicht**. Diese Einstellung bestimmt, wie SQL Server common Language Runtime (CLR)-Typen auf neuere Assemblyversionen aktualisieren. Dies sollte festgelegt werden, um **"false"** in den meisten Szenarien.

Diese Tabelle zeigt typische Einstellungen für unterschiedliche zielumgebungen an. Allerdings können die Einstellungen abhängig von den genauen Anforderungen abweichen.

|  | Entwickler/Test | Staging/Integration | Produktion |
| --- | --- | --- | --- |
| **Vergleich von Bereitstellungssortierreihenfolge** | Quelle | Ziel | Ziel |
| **Datenbankeigenschaften bereitstellen** | True | Nur beim ersten Mal | Nur beim ersten Mal |
| **Datenbank immer neu erstellen** | True | False | False |
| **Blockieren Sie inkrementelle Bereitstellung, wenn Datenverlust auftreten könnte** | False | Vielleicht | True |
| **Bereitstellungsskript im Einzelbenutzermodus ausführen** | False | True | True |
| **Sichern Sie die Datenbank vor der Bereitstellung** | False | Vielleicht | True |
| **Generieren Sie DROP-Anweisungen für Objekte, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt,** | False | True | True |
| **Verwenden Sie ALTER ASSEMBLY-Anweisungen nicht zum Aktualisieren von CLR-Typen** | False | False | False |
  

> [!NOTE]
> Weitere Informationen zu den Eigenschaften für die datenbankbereitstellung und Überlegungen zur quellumgebung, finden Sie unter [An Overview of Database Project Settings](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [Vorgehensweise: Konfigurieren Sie die Eigenschaften für die Bereitstellungsdetails](https://msdn.microsoft.com/library/dd172125.aspx), [ Erstellen und Bereitstellen von Datenbank in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx), und [erstellen und Bereitstellen von Datenbanken in einer Staging- oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx).


Um die Bereitstellung eines Datenbankprojekts an mehrere Ziele zu unterstützen, sollten Sie eine Bereitstellungskonfigurationsdatei für jede zielumgebung erstellen.

**So erstellen Sie eine Datei umgebungsspezifischer Konfiguration**

1. In Visual Studio 2010 in der **Projektmappen-Explorer** rechten Maustaste auf das Datenbankprojekt, und klicken Sie dann auf **Eigenschaften**.
2. Auf der Eigenschaftenseite der Datenbank-Projekt auf die **bereitstellen** Registerkarte die **Bereitstellungskonfigurationsdatei** auf **neu**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. In der **neue Bereitstellungskonfigurationsdatei** Dialogfeld gewähren Sie der Datei einen aussagekräftigen Namen (z. B. **TestEnvironment.sqldeployment**), und klicken Sie dann auf **speichern**.
4. Auf der *[Dateiname] *** SQLDEPLOYMENT** Seite, die Eigenschaften der Bereitstellung entsprechend die Anforderungen Ihrer zielumgebung festlegen und speichern Sie die Datei.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Beachten Sie, dass die neue Datei im Ordner "Eigenschaften" im Datenbankprojekt hinzugefügt wird.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Angeben der Konfigurationsdatei der Bereitstellung in VSDBCMD

Wenn Sie Konfigurationen für Projektmappenbuilds (z. B. Debug und Release) in Visual Studio 2010 verwenden, können Sie jede Konfiguration eine Bereitstellungskonfigurationsdatei zuordnen. Wenn Sie eine bestimmte Konfiguration erstellen, generiert des Buildprozesses eine konfigurationsspezifischen Bereitstellungsmanifestdatei, die auf die konfigurationsspezifischen Bereitstellungskonfigurationsdatei verweist. Allerdings ist eines der wichtigsten Ziele des Ansatzes zur Bereitstellung, die in diesen Tutorials beschrieben, Personen, die Möglichkeit, den Bereitstellungsprozess zu steuern, ohne Verwendung von Visual Studio 2010 und Projektmappenkonfigurationen gewähren. Bei diesem Ansatz ist die Konfiguration der Projektmappe unabhängig von der zielumgebung für die Bereitstellung an. Um die Bereitstellung der Datenbank in eine bestimmte zielumgebung anzupassen, können Sie die Befehlszeilenoptionen VSDBCMD an der Konfigurationsdatei der Bereitstellung.

Verwenden Sie zum Angeben einer Konfigurationsdatei für die Bereitstellung in Ihrer VSDBCMD der **p:/DeploymentConfigurationFile** Schalter, und geben Sie den vollständigen Pfad der Datei. Dadurch werden die Bereitstellungskonfigurationsdatei überschrieben, die das Bereitstellungsmanifest identifiziert. Beispielsweise können mit diesem Befehl VSDBCMD zum Bereitstellen der **ContactManager** Datenbank in einer testumgebung:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Beachten Sie, dass der Buildprozess Ihre SQLDEPLOYMENT-Datei umbenennen kann, wenn sie die Datei in das Ausgabeverzeichnis kopiert.


Wenn Sie SQL-Befehlsvariablen in Ihrer SQL-Skripts vor oder nach der Bereitstellung verwenden, können Sie einen ähnlichen Ansatz, der Bereitstellung eine umgebungsspezifischen SQLCMDVARS-Datei zugeordnet. In diesem Fall verwenden Sie die **p:/SqlCommandVariablesFile** wechseln, um die SQLCMDVARS-Datei zu identifizieren.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>MSBuild-Projektdatei Ausführen den VSDBCMD-Befehl

Sie können einen VSDBCMD-Befehl aus einer MSBuild-Projektdatei aufrufen, indem Sie mit einem **Exec** Tasks in einem MSBuild-Ziel. In seiner einfachsten Form würde es folgendermaßen aussehen:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- In der Praxis um Ihre Projektdateien vereinfachen lesen und wiederverwenden möchten, sollten Sie zum Erstellen von Eigenschaften zum Speichern der verschiedenen Befehlszeilenparameter. Dies erleichtert es für Benutzer, um Eigenschaftswerte in einer Projektdatei, umgebungsspezifische zu bereitzustellen oder um Standardwerte aus der MSBuild-Befehlszeile außer Kraft setzen. Bei Verwendung in beschriebenen Ansatz der geteilten Projekt Datei [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), Sie sollten Ihre einrichtungsanweisungen und die Eigenschaften, die zwischen den beiden Dateien entsprechend unterteilen:
- Umgebungsspezifische Einstellungen wie der Name der Bereitstellung der Konfigurationsdatei, die Datenbank-Verbindungszeichenfolge und den Namen der Zieldatenbank, sollte in der Projektdatei auf umgebungsspezifische eingefügt werden.
- Das MSBuild-Ziel, das der VSDBCMD-Befehl, zusammen mit der universal Eigenschaften wie den Speicherort der ausführbaren Datei VSDBCMD führt sollte in der universelle Projektdatei eingefügt werden.

Sie sollten auch sicherstellen, dass Sie das Datenbankprojekt erstellen, bevor Sie VSDBCMD aufrufen, sodass die DEPLOYMANIFEST-Datei erstellt wurde und einsatzbereit ist. Sehen Sie ein vollständiges Beispiel dieses Ansatzes im Thema [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md), führt Sie durch die Projektdateien in der [Contact Manager-beispiellösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird beschrieben, wie Sie die Datenbankeigenschaften, die unterschiedliche zielumgebungen beim Bereitstellen von Datenbankprojekten, die mithilfe von MSBuild und VSDBCMD anpassen können. Dieser Ansatz ist nützlich, wenn Sie Projekte als Teil des größeren, unternehmensweite Lösungen bereitstellen müssen. Diese Lösungen werden häufig an mehrere Ziele, z. B. Sandbox Entwicklungs- oder testumgebungen "," Staging "oder" Integration von Plattformen, "und" Produktion "oder" live-Umgebungen bereitgestellt. Jede dieser zielumgebungen erfordert in der Regel einen eindeutigen Satz von Eigenschaften für die datenbankbereitstellung.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Bereitstellen von Datenbankprojekten VSDBCMD.exe verwenden, finden Sie unter [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Diese Artikel auf MSDN enthalten allgemeineren Leitfaden für die Bereitstellung der Datenbank:

- [Eine Übersicht über Datenbankprojekteinstellungen](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Vorgehensweise: Konfigurieren von Eigenschaften für die Bereitstellungsdetails](https://msdn.microsoft.com/library/dd172125.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer Staging- oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Zurück](performing-a-what-if-deployment.md)
> [Weiter](deploying-database-role-memberships-to-test-environments.md)
