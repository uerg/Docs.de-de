---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: "Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen | Microsoft Docs"
author: jrjlee
description: "Dieses Thema beschreibt, wie Sie die Eigenschaften einer Datenbank zu bestimmten zielumgebungen im Rahmen des Bereitstellungsprozesses anpassen können. Hinweis: Das Thema wird davon ausgegangen th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: f3ca344c2466d9d538f55cd8ff0a5bf5b7bac808
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie die Eigenschaften einer Datenbank zu bestimmten zielumgebungen im Rahmen des Bereitstellungsprozesses anpassen können.
> 
> > [!NOTE]
> > Das Thema wird davon ausgegangen, dass Sie ein Visual Studio 2010-Datenbankprojekt mithilfe von MSBuild.exe und VSDBCMD.exe bereitstellen. Weitere Informationen, warum Sie diesen Ansatz auswählen können, finden Sie unter [Webbereitstellung im Unternehmen](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) und [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Wenn Sie ein Datenbankprojekt an mehrere Ziele bereitstellen, möchten Sie häufig die Datenbank-Bereitstellungseigenschaften für jedes zielumgebung anpassen. Beispielsweise würde in testumgebungen Sie in der Regel die Datenbank bei jeder Bereitstellung neu erstellen hingegen in Staging-oder produktionsumgebung Sie viel wahrscheinlicher damit inkrementelle Updates für Ihre Daten zu erhalten werden würde.
> 
> In einem Visual Studio 2010-Datenbankprojekt sind Einstellungen für eine Bereitstellung in einer Bereitstellungskonfigurationsdatei (.sqldeployment) enthalten. In diesem Thema wird gezeigt, wie umgebungsspezifische bereitstellungskonfigurationsdateien erstellen und angeben, die Sie als Parameter VSDBCMD verwenden möchten.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In diesem Thema wird Folgendes vorausgesetzt:

- Verwenden Sie die Teilung Projekt Datei Vorgehensweise bei der Bereitstellung von Projektmappen, wie in beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Sie rufen VSDBCMD aus der Projektdatei, um das Datenbankprojekt bereitstellen, wie in beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Um ein Bereitstellungssystem zu erstellen, die unterschiedliche Eigenschaften zur datenbankbereitstellung zwischen zielumgebungen unterstützt, müssen Sie Folgendes ausführen:

- Eine Bereitstellungskonfigurationsdatei (.sqldeployment) für jede zielumgebung zu erstellen.
- Erstellen Sie einen VSDBCMD-Befehl, der angibt, die Bereitstellungskonfigurationsdatei als einer Befehlszeilenoption.
- Parametrisieren Sie die VSDBCMD-Befehl in einer Projektdatei Microsoft Build Engine (MSBuild), sodass die VSDBCMD-Optionen für die zielumgebung geeignet sind.

In diesem Thema erfahren Sie, wie Sie jede der folgenden Verfahren ausführen.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Umgebungsspezifische Bereitstellungskonfigurationsdateien erstellen

Standardmäßig enthält ein Datenbankprojekt eine einzelne Bereitstellung-Konfigurationsdatei mit dem Namen *Database.sqldeployment*. Wenn Sie diese Datei in Visual Studio 2010 öffnen, sehen Sie die verschiedenen Bereitstellungsoptionen, die Ihnen zur Verfügung stehen:

- **Bereitstellung Vergleich Sortierung**. Auf diese Weise können Sie entscheiden, ob die datenbanksortierung des Projekts verwendet werden soll (der *Quelle* Sortierung) oder die datenbanksortierung des Zielservers (die *Ziel* Sortierung). In den meisten Fällen sollten Sie die quellsortierung zu verwenden, wenn Sie für eine Entwicklung bereitstellen oder testumgebung. Wenn Sie in einer Staging- oder produktionsumgebung bereitstellen, sollten Sie in der Regel die zielsortierung zur Vermeidung von jegliche Interoperabilitätsprobleme unverändert zu lassen.
- **Datenbankeigenschaften bereitstellen**. Auf diese Weise können Sie auswählen, ob die Datenbankeigenschaften gelten gemäß der *Database.sqlsettings* Datei. Wenn Sie eine Datenbank zum ersten Mal bereitstellen, sollten Sie die Datenbankeigenschaften bereitstellen. Wenn Sie eine vorhandene Datenbank aktualisieren, Eigenschaften sollte bereits vorhanden sein, und Sie sollte nicht erneut bereitstellen müssen.
- **Datenbank immer neu erstellen**. Damit können Sie auswählen, ob die Zieldatenbank erneut zu erstellen, jedes Mal, wenn Sie bereitstellen oder stellen inkrementelle Änderungen an die Zieldatenbank schalten mit Ihrem Schema auf dem neuesten Stand. Wenn die Datenbank neu erstellt wird, verlieren Sie alle Daten in der vorhandenen Datenbank. Daher ist es möglich, Sie sollten in der Regel festlegen, um **"false"** für Bereitstellungen auf Staging-oder produktionsumgebung.
- **Inkrementelle Bereitstellung blockieren, wenn Datenverlust auftreten könnte**. Dadurch können Sie auswählen, ob die Bereitstellung beendet werden soll, wenn eine Änderung am Datenbankschema Datenverlust führen. Sie in der Regel festlegen, um **"true"** für eine Bereitstellung in einer produktionsumgebung, bieten die Möglichkeit, eingreifen und keine wichtigen Daten zu schützen. Wenn Sie festgelegt haben **Datenbank immer neu erstellen** auf **"false"**, diese Einstellung hat keine Auswirkung.
- **Führen Sie die Bereitstellung im Einzelbenutzermodus**. Dies ist nicht in der Regel ein Problem in einer Umgebung für Entwicklungs- oder Testserver. Aber Sie sollten in der Regel festlegen, um **"true"** für Bereitstellungen auf Staging-oder produktionsumgebung. Dadurch wird verhindert, dass Benutzer Änderungen an der Datenbank vorgenommen werden, während die Bereitstellung ausgeführt wird.
- **Sichern Sie die Datenbank vor der Bereitstellung**. Sie in der Regel festlegen, um **"true"** bei der Bereitstellung in einer produktionsumgebung als vorsichtmaßnahme gegen Datenverlust. Möchten Sie möglicherweise auch festgelegt ist, dass **"true"** bei der Bereitstellung in einer Stagingumgebung erforderlich, wenn Ihre Stagingdatenbank eine große Datenmenge enthält.
- **Generieren von DROP-Anweisungen für Objekte, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt sind**. In den meisten Fällen ist dies ein wesentlicher und notwendiger Teil von inkrementellen Änderungen an einer Datenbank vorgenommen werden. Wenn Sie festgelegt haben **Datenbank immer neu erstellen** auf **"false"**, diese Einstellung hat keine Auswirkung.
- **Verwenden Sie ALTER ASSEMBLY-Anweisungen nicht zum Aktualisieren von CLR-Typen**. Diese Einstellung bestimmt, wie SQL Server common Language Runtime (CLR)-Typen auf neuere Assemblyversionen aktualisieren. Dies sollte festgelegt werden, um **"false"** in den meisten Szenarien.

Diese Tabelle zeigt typische Einstellungen für unterschiedliche zielumgebungen. Die Einstellungen variieren, je nachdem Bindungen die Anforderungen möglicherweise.

|  | Entwickler/Test | Staging-Integration | Produktion |
| --- | --- | --- | --- |
| **Bereitstellung Vergleich Sortierung** | Quelle | Ziel | Ziel |
| **Datenbankeigenschaften bereitstellen** | True | Nur beim ersten Mal | Nur beim ersten Mal |
| **Datenbank immer neu erstellen** | True | False | False |
| **Blockieren Sie inkrementelle Bereitstellung, wenn Datenverlust auftreten könnte** | False | Vielleicht | True |
| **Bereitstellungsskript im Einzelbenutzermodus ausführen** | False | True | True |
| **Sichern der Datenbank vor der Bereitstellung** | False | Vielleicht | True |
| **Generieren Sie DROP-Anweisungen für Objekte, die sich in der Zieldatenbank, aber nicht im Datenbankprojekt sind,** | False | True | True |
| **Verwenden Sie ALTER ASSEMBLY-Anweisungen nicht zum Aktualisieren von CLR-Typen** | False | False | False |
  

> [!NOTE]
> Weitere Informationen zu den Eigenschaften für die datenbankbereitstellung und Aspekte der Umgebung, finden Sie unter [eine Übersicht der Datenbankprojekteinstellungen](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [Vorgehensweise: Konfigurieren von Eigenschaften für Bereitstellungsdetails](https://msdn.microsoft.com/library/dd172125.aspx), [ Erstellen und Bereitstellen der Datenbank in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx), und [erstellen und Bereitstellen von Datenbanken in einer Staging- oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx).


Um die Bereitstellung eines Datenbankprojekts an mehrere Ziele zu unterstützen, sollten Sie eine Bereitstellungskonfigurationsdatei für jede zielumgebung erstellen.

**Zum Erstellen einer umgebungsspezifische-Konfigurationsdatei**

1. In Visual Studio 2010 in der **Projektmappen-Explorer** rechten Maustaste auf das Datenbankprojekt, und klicken Sie dann auf **Eigenschaften**.
2. Auf der Projekteigenschaftenseite für die Datenbank auf die **bereitstellen** Registerkarte die **Bereitstellungskonfigurationsdatei** auf **neu**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. In der **neue Bereitstellungskonfigurationsdatei** Dialogfeld Feld, geben Sie der Datei einen aussagekräftigen Namen (z. B. **TestEnvironment.sqldeployment**), und klicken Sie dann auf **speichern**.
4. Auf der *[Dateiname] *** SQLDEPLOYMENT** Seite, legen Sie die Bereitstellungseigenschaften entsprechend die Anforderungen der zielumgebung und speichern Sie die Datei.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Beachten Sie, dass die neue Datei im Ordner "Eigenschaften" auf das Datenbankprojekt hinzugefügt wird.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Die Konfigurationsdatei für die Bereitstellung angeben in VSDBCMD

Wenn Sie Projektmappenkonfigurationen (z. B. Debug- und Release) innerhalb von Visual Studio 2010 verwenden, können Sie jede Konfiguration eine Bereitstellungskonfigurationsdatei zuordnen. Wenn Sie eine bestimmte Konfiguration erstellen, generiert während des Erstellungsprozesses eine spezifische Konfiguration Bereitstellungsmanifestdatei, die konfigurationsspezifischen Bereitstellungskonfigurationsdatei verweist. Allerdings ist eine der wichtigsten Ziele des Ansatzes zur Bereitstellung in diesen Lernprogrammen beschrieben Personen Zugriff auf den Bereitstellungsprozess zu steuern, ohne Verwendung von Visual Studio 2010 und Projektmappenkonfigurationen zu gewähren. Bei dieser Vorgehensweise ist die Projektmappenkonfiguration unabhängig von der zielbereitstellungsumgebung identisch. Die VSDBCMD-Befehlszeilenoptionen können Sie zum Anpassen der Bereitstellung der Datenbank auf ein bestimmtes Ziel-Umgebung Ihre Bereitstellungskonfigurationsdatei anzugeben.

Verwenden Sie zum Angeben einer Konfigurationsdatei für die Bereitstellung in Ihrer VSDBCMD der **p:/DeploymentConfigurationFile** Schalter, und geben Sie den vollständigen Pfad zur Datei. Dadurch werden die Bereitstellungskonfigurationsdatei überschrieben, die das Bereitstellungsmanifest identifiziert. Angenommen, Sie können mit diesem Befehl VSDBCMD Bereitstellen der **ContactManager** Datenbank in einer testumgebung:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Beachten Sie, dass während des Erstellungsprozesses SQLDEPLOYMENT-Datei umbenennen kann, wenn er die Datei in das Ausgabeverzeichnis kopiert.


Bei Verwendung von SQL-Befehlsvariablen in Ihrer SQL-Skripts vor oder nach der Bereitstellung können Sie einen ähnlichen Ansatz, der Bereitstellung eine umgebungsspezifische SQLCMDVARS-Datei zugeordnet. In diesem Fall verwenden Sie die **p:/SqlCommandVariablesFile** Switch SQLCMDVARS-Datei zu identifizieren.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Ein MSBuild-Projektdatei Ausführen den VSDBCMD-Befehl

Sie können einen VSDBCMD-Befehl von einer MSBuild-Projektdatei aufrufen, indem Sie mithilfe einer **Exec** Aufgabe innerhalb einer MSBuild-Ziel. In der einfachsten Form ihn sieht dann folgendermaßen aus:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- In der Praxis stellen Sie die Projektdateien leicht zu lesen und wiederverwenden möchten, sollten Sie zum Erstellen von Eigenschaften, um die verschiedenen Befehlszeilenparameter zu speichern. Dies erleichtert es Benutzern, um Eigenschaftswerte in einer Projektdatei, umgebungsspezifische bereitzustellen oder um Standardwerte in der MSBuild-Befehlszeile zu überschreiben. Bei Verwendung in beschriebenen Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), Sie sollten die Buildanweisungen und die Eigenschaften zwischen den beiden Dateien entsprechend unterteilen:
- Umgebungsspezifische Einstellungen wie der Name der Konfigurationsdatei Bereitstellung, die Datenbank-Verbindungszeichenfolge und den Namen der Zieldatenbank, sollte in der Projektdatei umgebungsspezifische wechseln.
- Das MSBuild-Ziel, das der Befehl VSDBCMD zusammen mit universellen Eigenschaften wie den Speicherort der ausführbaren Datei VSDBCMD ausführt sollte in der universelle Projektdatei eingefügt werden.

Sie sollten auch sicherstellen, dass Sie das Datenbankprojekt erstellen, bevor Sie VSDBCMD aufrufen, damit die DEPLOYMANIFEST-Datei erstellt wurde und einsatzbereit ist. Sehen Sie ein vollständiges Beispiel dieses Ansatzes im Thema [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md), die führt Sie durch die Projektdateien in die [Vorgesetzten Kontakts Beispielprojektmappe](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird beschrieben, wie Sie die Datenbankeigenschaften, die unterschiedliche zielumgebungen beim Bereitstellen von Datenbankprojekten, die mithilfe von MSBuild und VSDBCMD anpassen können. Dieser Ansatz ist hilfreich, wenn Sie Datenbankprojekte als Teil der größeren, unternehmensweite Lösungen bereitstellen müssen. Diese Lösungen werden häufig an mehrere Ziele, wie Sandbox Entwicklungs- oder Testserver Umgebungen, Staging- oder Integration Plattformen und Produktion oder live-Umgebungen bereitgestellt werden. Jede dieser zielumgebungen erfordert in der Regel einen eindeutigen Satz von Eigenschaften für die datenbankbereitstellung.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Bereitstellen von Datenbankprojekten mit VSDBCMD.exe finden Sie unter [Datenbankprojekte bereitstellen](../web-deployment-in-the-enterprise/deploying-database-projects.md). Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Diese Artikel auf MSDN bieten allgemeineren Leitfaden für die Bereitstellung:

- [Eine Übersicht über Datenbankprojekteinstellungen](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Vorgehensweise: Konfigurieren von Eigenschaften für Bereitstellungsdetails](https://msdn.microsoft.com/library/dd172125.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer isolierten Entwicklungsumgebung](https://msdn.microsoft.com/library/dd193409.aspx)
- [Erstellen und Bereitstellen von Datenbanken in einer Staging- oder Produktionsumgebung](https://msdn.microsoft.com/library/dd193413.aspx)

>[!div class="step-by-step"]
[Zurück](performing-a-what-if-deployment.md)
[Weiter](deploying-database-role-memberships-to-test-environments.md)
