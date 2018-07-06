---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Erstellen eine Builddefinition, unterstützt die Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie jede Art von Build in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Teamprojekts erstellen. Dieses Thema des...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 18f88cff032bd0694ef98f0b19849f0edf1681ee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827387"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Erstellen eine Builddefinition, unterstützt die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie jede Art von Build in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Teamprojekts erstellen. In diesem Thema wird beschrieben, wie Sie eine neue Builddefinition in TFS erstellen und wie Sie Web-Bereitstellung im Rahmen des Buildprozesses in Team Build steuern.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Build & Deployment-Prozess von gesteuert wird zwei Projektdateien&#x2014;eine die Buildanweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Einstellungen für Build & Deployment gelten enthält. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Eine Builddefinition ist ein Mechanismus, der steuert, wie und wann Builds für Teamprojekte in TFS auftreten. Es gibt jede Builddefinition an:

- Die Dinge, die Sie erstellen wie Visual Studio-Projektmappendateien oder benutzerdefiniertes Microsoft Build Engine (MSBuild)-Projektdateien, möchten.
- Die Kriterien, die bestimmen, wann ein Build ausgeführt werden soll, platzieren, wie der manuelle Trigger, continuous Integration (CI), oder gated-Check-ins.
- Der Speicherort, an den Team Build Buildausgaben, einschließlich der bereitstellungsartefakte wie Web-Pakete und Skripts senden sollen.
- Die Zeitspanne, die jeden Build beibehalten werden sollen.
- Verschiedene weitere Parameter des Buildprozesses.

> [!NOTE]
> Weitere Informationen zum Build-Definitionen, finden Sie unter [definieren Ihres Buildprozesses](https://msdn.microsoft.com/library/ms181715.aspx).


In diesem Thema erfahren Sie, wie die Erstellung eine Builddefinition, die Continuous Integration, verwendet, damit ein Build ausgelöst wird, wenn ein Entwickler in neue Inhalte prüft. Wenn der Build erfolgreich ist, führt der Build-Dienst eine benutzerdefinierte Projektdatei, um die Lösung in einer testumgebung bereitzustellen.

Wenn Sie einen Build auszulösen, müssen diese Aktionen ausgeführt:

- Team Build sollten zunächst die Projektmappe erstellt werden. Im Rahmen dieses Prozesses wird das Web Publishing Pipeline (WPP) zur Bereitstellung von Webpaketen für jedes der Webanwendungsprojekte in der Projektmappe generiert Team Build aufgerufen werden. Teambuild führt auch keine Komponententests, die der Projektmappe zugeordnet.
- Wenn die Erstellung der Projektmappe ein Fehler auftritt, sollte das Team Build keine weiteren Maßnahmen ergreifen. Skriptkomponententest sollten als Buildfehler behandelt werden.
- Bei erfolgreicher Erstellung der Projektmappe sollte das benutzerdefinierte Projektdatei Team Build ausgeführt, die die Bereitstellung der Lösung steuert. Im Rahmen dieses Prozesses Team Build wird aufgerufen, die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) zum Installieren der verpackten Web-Anwendungen auf die Ziel-Webserver, und es wird aufgerufen, das VSDBCMD.exe-Dienstprogramm zum Erstellen der Datenbank ausführen. die Skripts auf dem Zielserver für die Datenbank.

Dies verdeutlicht den Prozess:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Die [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe enthält eine benutzerdefinierte MSBuild-Projektdatei *Publish.proj*, die von MSBuild oder Team Build ausgeführt werden können. Siehe [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md), diese Projektdatei definiert die Logik, die die Web-Pakete und Datenbanken in einer zielumgebung bereitgestellt. Die Datei enthält die Logik, die die Erstellung und Verpackungsprozesses ausgelassen, wenn sie in Team Build, verlassen einfach die Bereitstellungsaufgaben ausführen ausgeführt wird. Dies liegt daran, dass wenn Sie die Bereitstellung auf diese Weise automatisieren, Sie in der Regel sicherstellen sollten, dass das Projekt erfolgreich erstellt und übergibt alle Komponententests, bevor der Bereitstellungsprozess beginnt.

Im nächste Abschnitt wird erläutert, wie Sie diesen Prozess zu implementieren, indem Sie eine neue Builddefinition erstellen.

> [!NOTE]
> Diese Prozedur&#x2014;in der ein einzelnes automatisierten Prozess erstellt wird, tests, und stellt eine Lösung bereit,&#x2014;ist wahrscheinlich sich am besten für die Bereitstellung in Umgebungen zu testen. Für die Staging-und produktionsumgebungen können Sie viel eher Inhalte aus einem vorherigen Build bereitgestellt, die Sie bereits überprüft haben, und in einer testumgebung überprüft werden soll. Dieser Ansatz wird im nächsten Thema beschrieben [Bereitstellen von einem bestimmten Build](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Mit dieser Prozedur werden ausgeführt?

In der Regel führt ein TFS-Administrator, dieses Verfahren. In einigen Fällen kann ein Entwickler-Leiter Verantwortung für die teamprojektauflistung in TFS dauern. Um eine neue Builddefinition erstellen, müssen Sie Mitglied der **Buildadministratoren für Projektsammlung** für die Teamprojektsammlung, die die Projektmappe enthält.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Erstellen einer Builddefinition für CI und Bereitstellung

Im nächste Verfahren wird beschrieben, wie Sie eine Builddefinition erstellen, die CI-Trigger. Wenn der Build erfolgreich ist, wird die Lösung bereitgestellt, mit der Logik in einer benutzerdefinierten MSBuild-Projektdatei.

**So erstellen eine Builddefinition für CI und Bereitstellung**

1. In Visual Studio 2010 in der **Team Explorer** Fenster Erweitern Ihres Teamprojektknotens, mit der rechten Maustaste **erstellt**, und klicken Sie dann auf **neue Builddefinition**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Auf der **allgemeine** Registerkarte, benennen Sie der Builddefinition (z. B. **DeployToTest**) und eine optionale Beschreibung.
3. Auf der **Trigger** Registerkarte, wählen Sie die Kriterien an, auf denen einen neuen Build ausgelöst werden sollen. Wählen Sie z. B. Wenn Sie verwenden möchten, erstellen Sie die Projektmappe, und in die testumgebung bereit, sobald ein Entwickler im neuen Code eincheckt, **Continuous Integration**.
4. Auf der **Build-Standardwerte** Registerkarte die **Kopieren der Buildausgabe in den folgenden Ablageordner** geben den Pfad der Universal Naming Convention (UNC) des Ordners "Drop" (z. B.  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Dieses Ablageorts speichert mehrere Builds, abhängig von der Aufbewahrungsrichtlinie, die Sie konfigurieren. Wenn Sie die bereitstellungsartefakte aus einem bestimmten Build in einer Umgebung Staging- oder produktionsumgebung veröffentlichen möchten, ist dies in der Sie diese finden.
5. Auf der **Prozess** Registerkarte die **Buildprozessdatei** Dropdown-Liste belassen **DefaultTemplate.xaml** ausgewählten. Dies ist eine von der Standard-Buildprozessvorlagen, die zu allen neuen Teamprojekten hinzugefügt werden.
6. In der **Buildprozessparameter** Tabelle, klicken Sie in der **zu erstellende Elemente** Zeile, und klicken Sie dann auf die **Auslassungspunkte** Schaltfläche.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. In der **zu erstellende Elemente** Dialogfeld klicken Sie auf **hinzufügen**.
8. Navigieren Sie zum Speicherort Ihrer Lösungsdatei, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. In der **zu erstellende Elemente** Dialogfeld klicken Sie auf **hinzufügen**.
10. In der **Elemente des Typs** Dropdownliste **MSBuild-Projektdateien**.
11. Navigieren Sie zum Speicherort der benutzerdefinierten Datei mit dem Sie den Bereitstellungsprozess zu steuern, wählen Sie die Datei, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Die **zu erstellende Elemente** Dialogfeld sollte nun zwei Elemente angezeigt werden. Klicken Sie auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Auf der **Prozess** Registerkarte die **Buildprozessparameter** Tabelle, erweitern Sie die **erweitert** Abschnitt.
14. In der **MSBuild-Argumente** Zeile, die alle MSBuild-Befehlszeilenargumente hinzufügen, die *entweder* Elemente erstellen zu müssen. In diesem Lösungsszenario Contact Manager-sind diese Argumente erforderlich:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. In diesem Beispiel:

    1. Die **DeployOnBuild = True** und **DeployTarget = Paket** Argumente sind erforderlich, wenn Sie die Projektmappe Contact Manager erstellen. Dies weist die MSBuild-Bereitstellungspakete erstellen nach der Erstellung der einzelnen Webanwendungsprojekt, siehe [erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. Die **TargetEnvPropsFile** Argument ist erforderlich, bei der Erstellung der *Publish.proj* Datei. Diese Eigenschaft gibt den Speicherort der Datei umgebungsspezifischer Konfiguration an, wie in beschrieben [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Auf der **Aufbewahrungsrichtlinie** Konfigurieren jedes Typs wie viele builds nach Bedarf beibehalten möchten.
17. Klicken Sie auf **Speichern**.

## <a name="queue-a-build"></a>Hinzufügen eines Builds zur Warteschlange

An diesem Punkt haben Sie mindestens eine neue Builddefinition erstellt. Der Buildprozess, die, den Sie definiert, wird nun gemäß der Trigger ausgeführt, die Sie in der Builddefinition angegeben haben.

Wenn Sie die Builddefinition, um CI verwenden konfiguriert haben, können Sie die Builddefinition auf zwei Arten testen:

- Überprüfen Sie in Teil des Inhalts mit dem Teamprojekt einen automatische Build ausgelöst.
- Einen Build manuell zur Warteschlange.

**Um einen Build manuell in die Warteschlange**

1. In der **Team Explorer** rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. In der **Build zur Warteschlange hinzufügen** (Dialogfeld), überprüfen Sie die Buildeigenschaften, und klicken Sie dann auf **Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Überprüfen Sie den Status und das Ergebnis eines Builds&#x2014;unabhängig davon, ob es manuell oder automatisch ausgelöst wird,&#x2014;Doppelklicken Sie auf die Builddefinition in die **Team Explorer** Fenster. Dadurch wird geöffnet, eine **Build Explorer** Registerkarte.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Von hier aus können Sie die fehlerhaften Builds beheben. Wenn Sie einen der Builds doppelklicken, können Sie Informationen anzeigen und klicken Sie, um ausführliche Protokolldateien.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Sie können diese Informationen zu fehlerhaften Builds Probleme zu beheben, bevor Sie, ein anderer Build versuchen verwenden.

> [!NOTE]
> Builds, durch die von Bereitstellungslogik ausführen, werden wahrscheinlich erst, wenn Sie dem Build-Server in der zielumgebung erforderlichen Berechtigungen erteilt haben. Weitere Informationen finden Sie unter [Konfigurieren von Berechtigungen für Team Build Deployment](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Überwachen des Buildprozesses

TFS bietet eine Vielzahl von Funktionen für den Buildprozess zu überwachen. Beispielsweise können TFS senden Ihnen eine e-Mail oder Warnungen in Ihre Infobereich der Taskleiste angezeigt wird, wenn ein Build abgeschlossen wurde. Weitere Informationen finden Sie unter [ausführen und Überwachen von Builds](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie eine Builddefinition in TFS zu erstellen. Die Builddefinition ist für Continuous Integration, konfiguriert, damit der Buildprozess ausgeführt wird, wenn ein Entwickler im Inhalt mit dem Teamprojekt eincheckt. Die Builddefinition führt eine benutzerdefinierte MSBuild-Projektdatei zum Bereitstellen von Webpaketen und Datenbankskripts in einer zielumgebung für die Server aus.

In der Reihenfolge für eine automatische Bereitstellung im Rahmen des Buildvorgangs erfolgreich ist müssen Sie die entsprechenden Berechtigungen für das Build-Dienstkonto auf die Ziel-Webserver und den Zielserver für die Datenbank zu gewähren. Das letzte Thema in diesem Tutorial [Konfigurieren von Berechtigungen für Team Build Deployment](configuring-permissions-for-team-build-deployment.md), beschreibt, wie Sie ermitteln und konfigurieren Sie die erforderlichen Berechtigungen für die automatisierte Bereitstellung von einem Team Build-Server.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/library/ms181716.aspx) und [definieren Ihres Buildprozesses](https://msdn.microsoft.com/library/ms181715.aspx). Weitere Anleitungen zum Abfragen von Builds, finden Sie unter [einen Build zur Warteschlange](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-a-tfs-build-server-for-web-deployment.md)
> [Weiter](deploying-a-specific-build.md)
