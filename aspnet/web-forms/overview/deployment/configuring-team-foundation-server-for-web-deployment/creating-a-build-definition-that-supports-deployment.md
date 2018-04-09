---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Erstellen einer Builddefinition, unterstützt die Bereitstellung | Microsoft Docs
author: jrjlee
description: Wenn Sie alle Arten von Builds in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Teamprojekts zu erstellen. Dieses Thema des...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Erstellen einer Builddefinition, unterstützt die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie alle Arten von Builds in Team Foundation Server (TFS) 2010 ausführen möchten, müssen Sie eine Builddefinition innerhalb des Teamprojekts zu erstellen. Dieses Thema beschreibt, wie eine neue Builddefinition in TFS erstellt und zur Bereitstellung im Rahmen des Buildprozesses in Team Build steuern.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in die erstellungs-und Bereitstellung von gesteuert wird zwei Projektdateien&#x2014;eine Buildanweisungen, die für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess gelten "" enthält. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Eine Builddefinition ist ein Mechanismus, der steuert, wie und wann Builds Teamprojekten in TFS auftreten. Jede Builddefinition angibt:

- Die Dinge, die Sie erstellen z. B. Visual Studio-Projektmappendateien oder benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien, möchten.
- Die Kriterien, die bestimmen, wann ein Build ausgeführt werden soll, platzieren, wie manuelle Trigger, fortlaufende Integration (CI), oder gated-Check-ins.
- Der Speicherort, an den Team Build Buildausgaben, einschließlich bereitstellungsartefakte wie Webpaketen und Datenbankskripts senden soll.
- Die Zeitspanne, die jeden Build beibehalten werden sollen.
- Verschiedene andere Parameter des Buildprozesses.

> [!NOTE]
> Weitere Informationen für alle Builddefinitionen finden Sie unter [Buildprozess definieren](https://msdn.microsoft.com/library/ms181715.aspx).


In diesem Thema wird gezeigt, wie zum Erstellen einer Builddefinition, die Konfigurationselemente, verwendet, damit ein Build ausgelöst wird, wenn ein Entwickler in neue Inhalte überprüft. Wenn der Buildvorgang erfolgreich war, führt der Builddienst eine benutzerdefinierte Projektdatei, um die Lösung in einer testumgebung bereitzustellen.

Wenn Sie einen Build auszulösen, müssen diese Aktionen ausgeführt:

- Team Build sollte zuerst die Projektmappe erstellt werden. Im Rahmen dieses Prozesses wird das Team Build Web veröffentlichen Pipeline (WPP) zum Generieren von Web-Bereitstellungspakete für jede der in der Projektmappe die Webanwendungsprojekte aufgerufen. Teambuild führen auch Komponententests, die der Projektmappe zugeordnet.
- Wenn der Projektmappen-Buildvorgang scheitert, dauert der Teambuild keine weitere Aktion aus. Komponententest von Testfehlern sollte als Buildfehler behandelt werden.
- Bei erfolgreicher Erstellung der Projektmappe sollte das benutzerdefinierte Projektdatei Team Build ausgeführt, die die Bereitstellung der Lösung steuert. Im Rahmen dieses Prozesses Team Build wird aufgerufen, die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy), um die gepackte Webanwendungen für die Ziel-Webserver zu installieren, und es wird aufgerufen, das VSDBCMD.exe-Hilfsprogramm, um das Erstellen einer Datenbank ausführen Skripts auf dem Zielserver für die Datenbank.

Dies veranschaulicht den Prozess:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

Die [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) errorhandling enthält eine benutzerdefinierte MSBuild-Projektdatei *Publish.proj*, die von MSBuild oder Team Build ausgeführt werden können. Wie in beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md), diese Projektdatei definiert die Logik, mit denen die Webpakete und Datenbanken in einer zielumgebung bereitgestellt. Die Datei enthält die Logik, die Gebäude und Verpackungsprozesses ausgelassen, wenn sie in Team Build, lassen nur die Bereitstellungsaufgaben ausführen ausgeführt wird. Dies liegt daran, dass wenn Sie die Bereitstellung auf diese Weise automatisieren, Sie in der Regel möchten sicherstellen, dass das Projekt erfolgreich erstellt und Komponententests schon vorbei, bevor Sie der Bereitstellungsprozess beginnt.

Der nächste Abschnitt erklärt, wie Sie diesen Prozess zu implementieren, indem Sie eine neue Builddefinition erstellen.

> [!NOTE]
> Diese Prozedur&#x2014;in dem ein einzelnes automatisierten Prozess erstellt tests und Bereitstellung einer Lösung&#x2014;ist es wahrscheinlich, dass sich am besten für die Bereitstellung, um Umgebungen zu testen werden. Für die Staging-und produktionsumgebungen können Sie viel wahrscheinlicher zu Inhalten von einem vorherigen Build bereitgestellt, die Sie bereits überprüft haben, und in einer testumgebung überprüft werden soll. Dieser Ansatz wird im nächsten Thema beschrieben [Bereitstellen einer bestimmten Build](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Wem diese Prozedur ausgeführt werden?

In der Regel führt ein TFS-Administrator diese Prozedur. In einigen Fällen kann ein Entwickler Teamleiter Verantwortung für die Teamprojektsammlung in TFS dauern. Um eine neue Builddefinition erstellen, müssen Sie ein Mitglied der **Projektauflistungsadministratoren erstellen** Gruppe für die Teamprojektsammlung, die die Projektmappe enthält.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Erstellen Sie eine Builddefinition für CI und Bereitstellung

Die nächste Prozedur beschreibt, wie eine Builddefinition erstellen, die CI auslöst. Wenn der Buildvorgang erfolgreich war, wird die Lösung bereitgestellt mit Logik in eine benutzerdefinierte MSBuild-Projektdatei.

**So erstellen Sie eine Builddefinition für CI und Bereitstellung**

1. In Visual Studio 2010 in der **Team Explorer** Fenster Erweitern des Teamprojektknotens der rechten Maustaste auf **Builds**, und klicken Sie dann auf **neue Builddefinition**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Auf der **allgemeine** Registerkarte, benennen Sie der Builddefinition (z. B. **DeployToTest**) und eine optionale Beschreibung.
3. Auf der **Trigger** Registerkarte, wählen Sie die Kriterien, die auf dem Sie einen neuen Build auslösen möchten. Wählen Sie z. B. wenn die Projektmappe erstellt und in der testumgebung bereitstellen, jedes Mal, wenn ein Entwickler im neuen Code überprüft werden sollen, **Continuous Integration**.
4. Auf der **Build-Standardwerte** Registerkarte die **Kopie Buildausgabe in den folgenden Ablageordner** Feld, geben Sie den Pfad (UNC = Universal Naming Convention) des Ordners "Drop" (z. B.  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Diese Dateiablage-Speicherort speichert mehrere Builds, abhängig von der Aufbewahrungsrichtlinie, die Sie konfigurieren. Bei Bereitstellungselemente aus einem bestimmten Build in einer Staging- oder Produktion veröffentlichen soll, ist dies die, in dem sie suchen müssen.
5. Auf der **Prozess** Registerkarte die **Buildprozessdatei** verlassen der Dropdown-Liste **DefaultTemplate.xaml** ausgewählten. Dies ist eine der Build Standardprozessvorlagen, die auf alle neuen Teamprojekten hinzugefügt werden.
6. In der **Buildprozessparameter** Tabelle, klicken Sie in der **zu erstellende Dokumente** Zeile, und klicken Sie dann auf die **Auslassungszeichen** Schaltfläche.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. In der **zu erstellende Dokumente** (Dialogfeld), klicken Sie auf **hinzufügen**.
8. Navigieren Sie zum Speicherort der Projektmappendatei, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. In der **zu erstellende Dokumente** (Dialogfeld), klicken Sie auf **hinzufügen**.
10. In der **Elemente des Typs** Dropdownliste **MSBuild-Projektdateien**.
11. Navigieren Sie zum Speicherort der benutzerdefinierten Projektdatei, mit denen Sie steuern des Bereitstellungsprozesses, wählen Sie die Datei, und klicken Sie dann auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. Die **zu erstellende Dokumente** Dialogfeld sollte jetzt zwei Elemente anzeigen. Klicken Sie auf **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Auf der **Prozess** Registerkarte die **Buildprozessparameter** table, erweitern Sie die **erweitert** Abschnitt.
14. In der **MSBuild-Argumente** Zeile, fügen Sie MSBuild Befehlszeilenargumente, die *entweder* erfordert der Items zu erstellen. Im Szenario Projektmappe Contact Manager sind diese Argumente erforderlich:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. In diesem Beispiel:

    1. Die **DeployOnBuild = "true"** und **DeployTarget = Paket** Argumente sind erforderlich, wenn Sie die Projektmappe Contact Manager erstellen. Dies weist MSBuild Web Bereitstellungspakete erstellen nach der Erstellung jedes Webanwendungsprojekt, wie in beschrieben [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. Die **TargetEnvPropsFile** Argument ist erforderlich, bei der Erstellung der *Publish.proj* Datei. Diese Eigenschaft gibt den Speicherort der Konfigurationsdatei umgebungsspezifische an, wie in beschrieben [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Auf der **Aufbewahrungsrichtlinie** Registerkarte, konfigurieren, wie viele jedes Typs erstellt, nach Bedarf beibehalten möchten.
17. Klicken Sie auf **Speichern**.

## <a name="queue-a-build"></a>Hinzufügen eines Builds zur Warteschlange

An diesem Punkt haben Sie mindestens eine neue Builddefinition erstellt. Der Build-Prozesses, die Sie definiert wird nun gemäß der Trigger ausgeführt, die Sie in der Builddefinition angegeben.

Wenn Sie die Builddefinition CI Verwendung konfiguriert haben, können Sie die Builddefinition auf zwei Arten testen:

- Checken Sie einige Inhalte mit dem Teamprojekt, einen automatische Build auszulösen.
- Die Warteschlange eines Builds manuell.

**Um manuell einen Build in die Warteschlange**

1. In der **Team Explorer** rechten Maustaste auf die Builddefinition, und klicken Sie dann auf **neuen Build in Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. In der **Build zur Warteschlange hinzufügen** (Dialogfeld), überprüfen Sie die Buildeigenschaften, und klicken Sie dann auf **Warteschlange**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

So überprüfen den Status und das Ergebnis eines Builds&#x2014;unabhängig davon, ob er manuell oder automatisch ausgelöst wurde&#x2014;Doppelklicken Sie auf die Builddefinition ist jetzt in der **Team Explorer** Fenster. Dies öffnet einen **Build Explorer** Registerkarte.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Von hier aus können Sie fehlerhafte Builds Probleme beheben. Wenn Sie einen einzelnen Build doppelklicken, können Sie Zusammenfassungsinformationen anzeigen und klicken Sie auf die ausführliche Protokolldateien.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Diese Informationen können Sie die Problembehandlung bei fehlerhaften Builds und Probleme zu beheben, bevor Sie, einen anderen Build versuchen.

> [!NOTE]
> Builds, die Bereitstellung Logik ausgeführt werden wahrscheinlich fehl, bis Sie die Build-Server alle in der zielumgebung erforderlichen Berechtigungen erteilt haben. Weitere Informationen finden Sie unter [Konfigurieren von Berechtigungen für die Team Build-Bereitstellung](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Überwachen des Buildprozesses

TFS bietet eine Breite Palette von Funktionen, um während des Erstellungsprozesses überwachen. TFS können z. B. per e-Mail oder Warnungen in Ihrer Infobereich der Taskleiste angezeigt wird, wenn ein Build abgeschlossen wurde. Weitere Informationen finden Sie unter [ausführen und Überwachen von Builds](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschriebene Vorgehensweise: erstellen eine Builddefinition in TFS. Die Builddefinition wurde für CI, konfiguriert, damit während des Erstellungsprozesses ausgeführt werden kann, wenn ein Entwickler Inhalt mit dem Teamprojekt eincheckt. Die Builddefinition führt eine benutzerdefinierte MSBuild-Projektdatei, um Web Deploy Packages and Datenbankskripts in einer zielumgebung für den Server bereitstellen.

In der Reihenfolge für eine automatische Bereitstellung im Rahmen des Buildprozesses erfolgreich ist müssen Sie die entsprechenden Berechtigungen für das Build-Dienstkonto auf dem Ziel-Webserver und den Zielserver für die Datenbank zu gewähren. Das letzte Thema in diesem Lernprogramm [Konfigurieren von Berechtigungen für die Team Build Bereitstellung](configuring-permissions-for-team-build-deployment.md), beschreibt, wie Sie zu identifizieren und konfigurieren Sie die Berechtigungen für die automatisierte Bereitstellung von einem Team Build-Server erforderlich.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Builddefinitionen finden Sie unter [Erstellen einer grundlegenden Builddefinition](https://msdn.microsoft.com/library/ms181716.aspx) und [Buildprozess definieren](https://msdn.microsoft.com/library/ms181715.aspx). Weitere Anleitungen auf queuing Builds finden Sie unter [einen Build zur Warteschlange](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Zurück](configuring-a-tfs-build-server-for-web-deployment.md)
> [Weiter](deploying-a-specific-build.md)
