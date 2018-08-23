---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Bereitstellen eines bestimmten Builds | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie zum Bereitstellen von Webpaketen und Datenbankskripts aus einem bestimmten vorherigen Build an ein neues Ziel, z. B. ein Staging- oder produktionsumgebung Enviro wird...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: e788f02795fc83ac98c5a0ba307f16b0f506e489
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838108"
---
<a name="deploying-a-specific-build"></a>Bereitstellen eines bestimmten Builds
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Webpakete und die Datenbankskripts aus einem bestimmten vorherigen Build an ein neues Ziel, z. B. ein Staging- oder produktionsumgebung-Umgebung bereitgestellt wird.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Build & Deployment-Prozess von gesteuert wird zwei Projektdateien&#x2014;eine die Buildanweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Einstellungen für Build & Deployment gelten enthält. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Bis jetzt in den Themen in diesem Tutorial Satz konzentriert sich auf die Sie erstellen, Packen und Bereitstellen von Webanwendungen und -Datenbanken als Teil einer einzigen Schritt oder automatisierten Prozess. Aber in einigen allgemeinen Szenarios sollten Sie eine Liste der Builds in einen Ablageordner auswählen, die von Ihnen bereitgestellten Ressourcen. Das heißt, möglicherweise der neueste Build der Build nicht, die, den Sie bereitstellen möchten.

Nehmen Sie continuous Integration (CI) beschrieben, die im vorherigen Thema [Erstellen einer Build-Definition, unterstützt Bereitstellung](creating-a-build-definition-that-supports-deployment.md). Sie haben eine Builddefinition in Team Foundation Server (TFS) 2010 erstellt. Jedes Mal, wenn ein Entwickler Code in TFS eincheckt, wird Team Build erstellen Sie Ihren Code, erstellen Webpakete und die Datenbankskripts als Teil des Buildprozesses, alle Komponententests ausführen und Ihre Ressourcen in einer testumgebung bereitstellen. Abhängig von der Aufbewahrungsrichtlinie, die Sie beim Erstellen der Builddefinition konfiguriert haben, behält TFS eine bestimmte Anzahl von vorherigen Builds.

![](deploying-a-specific-build/_static/image1.png)

Angenommen Sie, Sie haben die Überprüfung ausgeführt und Überprüfungstests für eines der folgenden in Ihrer testumgebung erstellt und Sie Ihre Anwendung in einer Stagingumgebung bereitstellen möchten. In der Zwischenzeit Entwickler möglicherweise neuen Code eingecheckt haben. Sie möchten die Projektmappe neu erstellen und Bereitstellen von in der Stagingumgebung bereit, und nicht den neuesten Build in der Stagingumgebung bereitstellen möchten. Stattdessen möchten Sie diesen Build bereitstellen, den Sie überprüft haben, und überprüft auf dem Testserver.

Um dies zu erreichen, müssen Sie die Microsoft Build Engine (MSBuild) mitteilen, wo Sie finden die Webpakete und die Datenbankskripts, die ein bestimmten Build generiert.

## <a name="overriding-the-outputroot-property"></a>Überschreiben der OutputRoot-Eigenschaft

In der [Beispielprojektmappe](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* Datei deklariert eine Eigenschaft namens **OutputRoot**. Wie der Name schon sagt, ist dies der Stammordner, der alles enthält, die während des Buildprozesses werden. In der *Publish.proj* -Datei, Sie können sehen, die die **OutputRoot** Eigenschaft bezieht sich auf das Stammverzeichnis für alle Bereitstellungsressourcen.

> [!NOTE]
> **OutputRoot** ist eine häufig verwendete Eigenschaftenname. Visual c# und Visual Basic-Projektdateien deklarieren auch diese Eigenschaft zum Speichern des Stammverzeichnis für alle Ausgaben des builddebugvorgangs.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Wenn Sie möchten die Projektdatei in Webpakete und Skripts von einem anderen Speicherort Datenbank&#x2014;wie die Ausgaben der eine früheren TFS-Build&#x2014;müssen Sie einfach überschreiben die **OutputRoot** Eigenschaft. Sie sollten den Wert der Eigenschaft in den entsprechenden Buildordner auf dem Team Build-Server festlegen. Wenn Sie MSBuild von der Befehlszeile aus ausgeführt haben, geben Sie einen Wert für **OutputRoot** als Befehlszeilenargument:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


In der Praxis jedoch, Sie würden auch überspringen möchten die **erstellen** Ziel&#x2014;wäre es ziemlich sinnlos, bei der Erstellung Ihrer Lösung, wenn Sie nicht planen, verwenden Sie die Buildausgaben. Hierzu können Sie geben die Ziele, die Sie über die Befehlszeile ausführen möchten:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Allerdings in den meisten Fällen sollten Sie Ihrer Bereitstellungslogik in einer TFS-Builddefinition zu erstellen. Dadurch können Benutzer mit der **Builds zur Warteschlange** Berechtigung für die Bereitstellung über alle Visual Studio-Installation mit einer Verbindung mit dem TFS-Server ausgelöst.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Erstellen eine Builddefinition zum Bereitstellen von bestimmten erstellt

Im nächste Verfahren wird beschrieben, wie Sie eine Builddefinition erstellen, die Benutzern, lösen Sie Bereitstellungen in einer Stagingumgebung bereit, mit einem einzigen Befehl ermöglicht.

In diesem Fall nicht sollen die Builddefinition, etwas erstellen&#x2014;möchten Sie Sie einfach die Bereitstellungslogik in der Projektdatei, benutzerdefinierte ausführen. Die *Publish.proj* -Datei enthält die bedingten Logik, die überspringt die **erstellen** verwenden, wenn die Datei in Team Build ausgeführt wird. Dies geschieht durch die Auswertung der integrierten **BuildingInTeamBuild** -Eigenschaft, die automatisch, um festgelegt wird **"true"** , wenn Sie die Projektdatei in Team Build ausführen. Daher können Sie überspringen den Buildprozess und führen Sie einfach die Projektdatei, um einen vorhandenen Build bereitzustellen.

**Zum Erstellen einer Builddefinition zum manuellen Auslösen der Bereitstellung**

1. In Visual Studio 2010 in der **Team Explorer** Fenster Erweitern Ihres Teamprojektknotens, mit der rechten Maustaste **erstellt**, und klicken Sie dann auf **neue Builddefinition**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Auf der **allgemeine** Registerkarte, benennen Sie der Builddefinition (z. B. **DeployToStaging**) und eine optionale Beschreibung.
3. Auf der **Trigger** Registerkarte **manuell –-Check-ins werden nicht ausgelöst, einen neuen Build**.
4. Auf der **Build-Standardwerte** Registerkarte die **Kopieren der Buildausgabe in den folgenden Ablageordner** geben den Pfad der Universal Naming Convention (UNC) des Ordners "Drop" (z. B.  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Auf der **Prozess** Registerkarte die **Buildprozessdatei** Dropdown-Liste belassen **DefaultTemplate.xaml** ausgewählten. Dies ist eine von der Standard-Buildprozessvorlagen, die zu allen neuen Teamprojekten hinzugefügt werden.
6. In der **Buildprozessparameter** Tabelle, klicken Sie in der **zu erstellende Elemente** Zeile, und klicken Sie dann auf die **Auslassungspunkte** Schaltfläche.

    ![](deploying-a-specific-build/_static/image4.png)
7. In der **zu erstellende Elemente** Dialogfeld klicken Sie auf **hinzufügen**.
8. In der **Elemente des Typs** Dropdownliste **MSBuild-Projektdateien**.
9. Navigieren Sie zum Speicherort der benutzerdefinierten Datei mit dem Sie den Bereitstellungsprozess zu steuern, wählen Sie die Datei, und klicken Sie dann auf **OK**.

    ![](deploying-a-specific-build/_static/image5.png)
10. In der **zu erstellende Elemente** Dialogfeld klicken Sie auf **OK**.
11. In der **Buildprozessparameter** Tabelle, erweitern Sie die **erweitert** Abschnitt.
12. In der **MSBuild-Argumente** Zeile, geben Sie den Speicherort Ihrer umgebungsspezifische-Projektdatei, und fügen einen Platzhalter für den Speicherort des Ordners "Build" hinzu:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Müssen Sie Sie überschreiben die **OutputRoot** Wert jedes Mal, wenn Sie einen Build in die Warteschlange. Dies wird im nächsten Verfahren behandelt.
13. Klicken Sie auf **Speichern**.

Wenn Sie einen Build auszulösen, müssen Sie zum Aktualisieren der **OutputRoot** -Eigenschaft auf den Build Sie bereitstellen möchten.

**Bereitstellen ein bestimmtes Builds aus einer Builddefinition**

1. In der **Team Explorer** rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](deploying-a-specific-build/_static/image7.png)
2. In der **Build zur Warteschlange hinzufügen** Dialogfeld auf die **Parameter** Registerkarte, erweitern Sie die **erweitert** Abschnitt.
3. In der **MSBuild-Argumente** Zeile, ersetzen Sie den Wert der **OutputRoot** Eigenschaft mit dem Speicherort des Ordners "Build". Zum Beispiel:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Achten Sie darauf, dass Sie einen nachgestellten Schrägstrich am Ende den Pfad zu Ihrem Ordner "Build" verwenden.
4. Klicken Sie auf **Warteschlange**.

Wenn Sie den Build zur Warteschlange, die Projektdatei die Datenbankskripts bereitstellen wird und Web-auf dem Ablageordner erstellen Sie Pakete in der **OutputRoot** Eigenschaft.

## <a name="conclusion"></a>Schlussbemerkung

Zum Veröffentlichen der Bereitstellungsressourcen wie Web-Pakete und Skripts aus einer vorherigen bestimmten erstellen mit dem Bereitstellungsmodell des Split-Projekt-Datei in diesem Thema beschrieben. Es wurde erklärt, wie zum Überschreiben der **OutputRoot** -Eigenschaft und wie Sie die Bereitstellungslogik in einem TFS Integrieren von build-Definitionen.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/library/ms181716.aspx) und [definieren Ihres Buildprozesses](https://msdn.microsoft.com/library/ms181715.aspx). Weitere Anleitungen zum Abfragen von Builds, finden Sie unter [einen Build zur Warteschlange](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Zurück](creating-a-build-definition-that-supports-deployment.md)
> [Weiter](configuring-permissions-for-team-build-deployment.md)
