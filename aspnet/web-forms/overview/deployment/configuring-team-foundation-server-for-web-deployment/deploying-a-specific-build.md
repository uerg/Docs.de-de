---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Bereitstellen von einem bestimmten Build | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie zum Bereitstellen von Webpaketen und Datenbankskripts aus einem bestimmten vorherigen Build an ein neues Ziel, z. B. ein Staging- oder Produktionsserver Enviro wird...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a>Bereitstellen von einem bestimmten Build
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Web Deploy Packages and Datenbankskripts aus einem bestimmten vorherigen Build an ein neues Ziel, z. B. ein Staging- oder Produktionsserver Umgebung bereitstellen.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md)in die erstellungs-und Bereitstellung von zwei Projektdateien & #x 2014 kontrolliert wird; o Ne mit Buildanweisungen, die für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess gelten. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Bis jetzt in den Themen in diesem Lernprogramm Satz konzentriert sich auf die erstellen, Verpacken und Bereitstellen von Webanwendungen und -Datenbanken als Teil einer einstufiger oder automatisierten Prozess. Jedoch in einigen allgemeinen Szenarios sollten Sie eine Liste von Builds in einem Ablageordner auswählen, die Ressourcen, die Sie bereitstellen. Das heißt, möglicherweise der neueste Build der Build nicht, die, den Sie bereitstellen möchten.

Betrachten Sie das fortlaufende Integration (CI)-Szenario, das im vorherigen Thema beschriebenen [erstellen eine erstellen, unterstützt Definitionsbereitstellung](creating-a-build-definition-that-supports-deployment.md). Sie haben eine Builddefinition in Team Foundation Server (TFS) 2010 erstellt. Jedes Mal, wenn ein Entwickler Code in TFS eincheckt, wird Team Build Code erstellen, Erstellen von Webpaketen und Datenbankskripts als Teil des Buildprozesses ausführen von Komponententests und Ihre Ressourcen in einer testumgebung bereitstellen. Abhängig von der Aufbewahrungsrichtlinie für die Sie konfiguriert werden, wenn Sie die Builddefinition erstellt haben, wird die TFS eine bestimmte Anzahl von vorherigen Builds beibehalten.

![](deploying-a-specific-build/_static/image1.png)

Nehmen Sie nun an Validierungstests anhand eines der folgenden in Ihrer testumgebung erstellt, Sie die Überprüfung ausgeführt haben und jetzt können Sie Ihre Anwendung in einer Stagingumgebung bereitstellen. In der Zwischenzeit können Entwickler möglicherweise neuen Code eingecheckt haben. Sie möchten nicht die Projektmappe neu erstellen und in der Stagingumgebung bereitstellen und nicht den neuesten Build in der Stagingumgebung bereitgestellt werden soll. Sie möchten stattdessen einen bestimmten Build bereitstellen, den Sie überprüft und auf dem Testserver überprüft haben.

Um dies zu erreichen, müssen Sie die Microsoft Build Engine (MSBuild) erkennen, wo Sie finden die Webpaketen und Datenbankskripts, die ein bestimmten Build generiert.

## <a name="overriding-the-outputroot-property"></a>Überschreiben der OutputRoot-Eigenschaft

In der [-Beispielprojektmappe](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* deklariert eine Eigenschaft namens **OutputRoot**. Wie der Name schon sagt, ist dies der Stammordner, der alles enthält, die während des Erstellungsprozesses generiert. In der *Publish.proj* -Datei ausführen, sehen Sie, den **OutputRoot** Eigenschaft bezieht sich auf das Stammverzeichnis für alle Ressourcen zur Bereitstellung.

> [!NOTE]
> **OutputRoot** ist eine häufig verwendete Eigenschaftsname. Visual c# und Visual Basic-Projektdateien deklarieren auch diese Eigenschaft auf das Stammverzeichnis für alle Buildausgaben zu speichern.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Die Projektdatei, die zum Bereitstellen von Webpaketen und Datenbankskripts von einem anderen Speicherort & #x 2014; z. B. die Ausgaben eines vorherigen TFS-Build & #x 2014; gegebenenfalls müssen Sie lediglich überschreiben die **OutputRoot** Eigenschaft. Sie sollten den Wert der Eigenschaft auf den relevanten Buildordner auf dem Team Build-Server festlegen. Wenn Sie MSBuild über die Befehlszeile ausführen, geben Sie einen Wert für **OutputRoot** als Befehlszeilenargument:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


In der Praxis jedoch, Sie würden auch überspringen möchten die **erstellen** Ziel & #x 2014; es ist kein Punkt in der Projektmappe erstellen, wenn Sie nicht die Ausgaben des builddebugvorgangs verwenden möchten. Sie können dazu durch Hinzufügen von Zielen, die Sie über die Befehlszeile ausführen möchten:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Allerdings in den meisten Fällen sollten Sie die Logik für die Bereitstellung in einer Builddefinition TFS integrieren. Dadurch können Benutzer mit der **Builds zur Warteschlange** über die Berechtigung zum Auslösen der Bereitstellung über alle Visual Studio-Installation mit einer Verbindung mit dem TFS-Server.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Erstellen eine Builddefinition zum Bereitstellen von bestimmten Builds

Die nächste Prozedur beschreibt, wie eine Builddefinition erstellen, die Benutzern, die Trigger-Bereitstellungen in einer Stagingumgebung mit einem einzigen Befehl ermöglicht.

In diesem Fall nicht die Builddefinition nichts erstellen soll & #x 2014; Sie nur er die Logik für die Bereitstellung in der Projektdatei, benutzerdefinierte ausgeführt werden soll. Die *Publish.proj* -Datei enthält, bedingten Logik, die überspringt die **erstellen** abzielen, wenn die Datei im Team Build ausgeführt wird. Dies geschieht durch das Auswerten der integriertes **BuildingInTeamBuild** -Eigenschaft, die automatisch, dass festgelegt ist **"true"** Wenn Sie die Projektdatei in Team Build ausführen. Folglich können während des Erstellungsprozesses überspringen und führen Sie Folgendes aus der Projektdatei, um einen vorhandenen Build bereitstellen.

**So erstellen eine Builddefinition, um die Bereitstellung manuell auslösen**

1. In Visual Studio 2010 in der **Team Explorer** Fenster Erweitern des Teamprojektknotens der rechten Maustaste auf **Builds**, und klicken Sie dann auf **neue Builddefinition**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Auf der **allgemeine** Registerkarte, benennen Sie der Builddefinition (z. B. **DeployToStaging**) und eine optionale Beschreibung.
3. Auf der **Trigger** Registerkarte **manuell – Eincheckvorgänge lösen keinen neuen Build**.
4. Auf der **Build-Standardwerte** Registerkarte die **Kopie Buildausgabe in den folgenden Ablageordner** Feld, geben Sie den Pfad (UNC = Universal Naming Convention) des Ordners "Drop" (z. B.  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Auf der **Prozess** Registerkarte die **Buildprozessdatei** verlassen der Dropdown-Liste **DefaultTemplate.xaml** ausgewählten. Dies ist eine der Build Standardprozessvorlagen, die auf alle neuen Teamprojekten hinzugefügt werden.
6. In der **Buildprozessparameter** Tabelle, klicken Sie in der **zu erstellende Dokumente** Zeile, und klicken Sie dann auf die **Auslassungszeichen** Schaltfläche.

    ![](deploying-a-specific-build/_static/image4.png)
7. In der **zu erstellende Dokumente** (Dialogfeld), klicken Sie auf **hinzufügen**.
8. In der **Elemente des Typs** Dropdownliste **MSBuild-Projektdateien**.
9. Navigieren Sie zum Speicherort der benutzerdefinierten Projektdatei, mit denen Sie steuern des Bereitstellungsprozesses, wählen Sie die Datei, und klicken Sie dann auf **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. In der **zu erstellende Dokumente** (Dialogfeld), klicken Sie auf **OK**.
11. In der **Buildprozessparameter** table, erweitern Sie die **erweitert** Abschnitt.
12. In der **MSBuild-Argumente** Zeile, geben Sie den Speicherort der Projektdatei umgebungsspezifische aus, und fügen Sie einen Platzhalter für den Speicherort des Ordners "Build" hinzu:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Sie müssen außer Kraft setzen die **OutputRoot** Wert jedes Mal, wenn Sie einen Build in die Warteschlange. Dieser Vorgang wird in der nächsten Prozedur beschrieben.
13. Klicken Sie auf **Speichern**.

Wenn Sie einen Build auszulösen, müssen Sie beim Aktualisieren der **OutputRoot** -Eigenschaft auf den Build Sie bereitstellen möchten.

**Bereitstellen von einem bestimmten Build aus einer Builddefinition**

1. In der **Team Explorer** rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](deploying-a-specific-build/_static/image7.png)
2. In der **Build zur Warteschlange hinzufügen** Dialogfeld auf die **Parameter** Registerkarte, erweitern Sie die **erweitert** Abschnitt.
3. In der **MSBuild-Argumente** Zeile, ersetzen Sie den Wert, der die **OutputRoot** Eigenschaft durch den Speicherort des Ordners "Build". Zum Beispiel:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Achten Sie darauf, dass Sie einen nachgestellten Schrägstrich am Ende der Pfad zu Ihrem Ordner "Builds".
4. Klicken Sie auf **Warteschlange**.

Bei der Sie den Build zur Warteschlange hinzu, die Projektdatei wird die Datenbankskripts bereitstellen und Web Paketen aus Buildablageordners Sie in der **OutputRoot** Eigenschaft.

## <a name="conclusion"></a>Schlussfolgerung

In diesem Thema beschrieben, wie die Bereitstellungsressourcen wie Webpaketen und Datenbankskripts zu veröffentlichen, die von einem bestimmten vorherigen erstellen mithilfe der Split-Datei projektbereitstellungsmodells. Es wird erläutert, wie zum Überschreiben der **OutputRoot** Eigenschaft und wie Sie die Logik für die Bereitstellung in einem TFS integrieren Builddefinition.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/en-us/library/ms181716.aspx) und [Buildprozess definieren](https://msdn.microsoft.com/en-us/library/ms181715.aspx). Weitere Anleitungen auf queuing Builds finden Sie unter [einen Build zur Warteschlange](https://msdn.microsoft.com/en-us/library/ms181722.aspx).

>[!div class="step-by-step"]
[Zurück](creating-a-build-definition-that-supports-deployment.md)
[Weiter](configuring-permissions-for-team-build-deployment.md)
