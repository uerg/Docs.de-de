---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: "Konfigurieren eine TFS-Buildserver für Web Deploy | Microsoft Docs"
author: jrjlee
description: "Dieses Thema beschreibt das Vorbereiten eines Buildservers der Team Foundation Server (TFS) erstellen und Bereitstellen der Lösungen, die mit Team Build und die Internet-Informationsspeicher..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: de31a9dffb95b863a4ec38b74fd2c6e03f287a7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurieren einen TFS-Build-Server für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt die zum Vorbereiten eines Buildservers der Team Foundation Server (TFS) erstellen und Bereitstellen der Lösungen, die mit Team Build und die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy).


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Zur Vorbereitung der Build-Server zu erstellen und Ihre Lösungen bereitzustellen, benötigen Sie:

- Installieren und Konfigurieren des TFS-Build-Diensts.
- Installieren von Visual Studio 2010.
- Installieren Sie alle Produkte oder Komponenten, die zum Erstellen der Projektmappe, wie Versionen von .NET Framework oder ASP.NET MVC erforderlich sind.
- Installieren Sie Web Deploy 2.0 oder höher.

In diesem Thema wird gezeigt, wie diese Verfahren ausgeführt, oder zeigen Sie auf andere Ressourcen, in dem sie vorhanden sind. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, die aus:

- Beginnen Sie mit einem "sauberen" Server-Build, Windows Server 2008 R2 Service Pack 1 ausführen.
- Der Server ist die Domäne mit einer statischen IP-Adresse.
- Sie haben die TFS-Anwendungsebene auf einem separaten Server installiert, wie in beschrieben [Web Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren ausgeführt werden?

In den meisten Fällen wird ein TFS-Administrator für das Konfigurieren von Buildservern verantwortlich sein. In einigen Fällen kann das Team Developer den Besitz des bestimmten Buildservern dauern.

## <a name="install-and-configure-the-tfs-build-service"></a>Installieren und Konfigurieren des TFS-Build-Diensts

Wenn Sie einen Build-Server konfigurieren, ist die erste Aufgabe zum Installieren und Konfigurieren des TFS-Build-Diensts. Im Rahmen dieses Vorgangs müssen Sie:

- Die TFS-Build-Dienst installieren und Konfigurieren eines Dienstkontos. Alle Buildaufgaben, einschließlich der Bereitstellung, werden mit der Identität des Build-Dienstkontos ausgeführt.
- Erstellen einer *Buildcontroller* und eine oder mehrere *build-Agents*. Jeder Buildcontroller verwaltet einen Satz von Build-Agents. Wenn Sie einen Build in die Warteschlange, weist der Buildcontroller die Build-Aufgabe an einen verfügbaren Build-Agent an. Jede Teamprojektsammlung in TFS wird eine einzelne Buildcontroller zugeordnet.
- Konfigurieren Sie einen Ablageordner für die Ausgaben des builddebugvorgangs. Dies ist eine Netzwerkfreigabe. Alle Ausgaben, z. B. Web-Bereitstellungspakete "erstellen", an den Ablageordner gesendet werden.

Die [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) Kapitel auf MSDN enthält alle Ressourcen, die Sie benötigen, um diese Aufgaben ausführen:

- Eine konzeptionelle Übersicht über Team Foundation Build, einschließlich der Builddienst Buildcontroller und Build-Agents finden Sie unter [Grundlegendes zu einem Team Foundation Build-System](https://msdn.microsoft.com/library/dd793166.aspx).
- Informationen zum Installieren und Konfigurieren des Builddiensts finden Sie unter [Konfigurieren eines Buildcomputers](https://msdn.microsoft.com/library/ms181712.aspx).
- Informationen zum Erstellen von Buildcontroller finden Sie unter [erstellen und Arbeiten mit einem Controller erstellen](https://msdn.microsoft.com/library/ee330987.aspx).
- Informationen zum Erstellen von Build-Agents finden Sie unter [erstellen und Verwenden von Build-Agents](https://msdn.microsoft.com/library/bb399135.aspx).
- Informationen zum Erstellen und Konfigurieren von Ablageordnern finden Sie unter [Einrichten von Ablageordnern](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installieren der erforderlichen Produkte und Komponenten

Um dem Build-Server zum Erstellen der Lösungen zu aktivieren, müssen Sie installieren alle Produkte, Komponenten oder Assemblys, die Ihre Lösung erforderlich sind. Bevor Sie alle webplattformkomponenten installieren, sollten Sie Visual Studio 2010 (beliebige Version) auf dem Buildserver installieren. Dadurch wird sichergestellt, dass die Kerndateien von Microsoft Build Engine (MSBuild) Ziel und die Zieldateien Web veröffentlichen Pipeline (WPP) für den Builddienst verfügbar sind. Visual Studio-Installer muss auch Web Deploy installieren müssen, wenn Sie als Teil des Buildprozesses Webpaketen bereitstellen möchten.

Die beste Möglichkeit, allgemeine webplattformkomponenten installieren ist die Verwendung der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118). Dadurch wird sichergestellt, dass Sie die neueste Version jedes Produkts installieren, und es auch automatisch erkannt und installiert alle erforderlichen Komponenten für jedes Produkt. Im Fall von der [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung sollten Sie den Webplattform-Installer verwenden, um diese Produkte und Komponenten zu installieren:

- **.NET Framework 4.0**. Dies ist erforderlich, um Anwendungen auszuführen, die für diese Version von .NET Framework erstellt wurden.
- **Webbereitstellungstool 2.1 oder höher Web**. Hiermit werden Web Deploy (und die zugrunde liegende ausführbare Datei, MSDeploy.exe) auf dem Server installiert. Im Rahmen dieses Prozesses wird installiert, und der Webbereitstellungs-Agent-Dienst startet. Dieser Dienst können Sie Webpakete von einem Remotecomputer bereitstellen.
- **ASP.NET MVC 3**. Hiermit werden die Assemblys, die Sie zum Ausführen von ASP.NET MVC 3-Anwendungen müssen installiert.

**Zum Installieren der erforderlichen Produkte und Komponenten**

1. Installieren von Visual Studio 2010. Wenn Sie aufgefordert werden, die zu installierenden Funktionen auswählen, sollten Sie Folgendes einschließen:

    1. Alle Programmiersprachen, die Sie benötigen, um zu kompilieren.
    2. Visual Web Developer. Dadurch wird sichergestellt, dass die WPP Ziele mit dem Buildserver hinzugefügt werden.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Klicken Sie nach Abschluss der Installation von Visual Studio 2010 herunterladen und installieren [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (sofern er nicht bereits in dem Installationsmedium enthalten ist).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 löst einen Fehler, der MSBuild für die Suche nach der ausführbaren MSDeploy verhindern kann.
3. Herunterladen und Starten der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).
4. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
5. Klicken Sie auf der linken Seite des Fensters klicken Sie im Navigationsbereich auf **Frameworks**.
6. In der **Microsoft .NET Framework 4** Zeile, wenn .NET Framework nicht bereits installiert ist, klicken Sie auf **hinzufügen**.

    > [!NOTE]
    > Sie können bereits .NET Framework 4.0 über Windows Update installiert haben. Wenn ein Produkt oder eine Komponente bereits installiert ist, den Webplattform-Installer wird angegeben, das durch Ersetzen der **hinzufügen** Schaltfläche mit dem Text **installiert**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. In der **ASP.NET MVC 3 (Visual Studio 2010)** auf **hinzufügen**.
8. Klicken Sie im Navigationsbereich auf **Server**.
9. In der **Web Bereitstellung Tool 2.1** auf **hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer erfahren Sie, eine Liste der Produkte & #x 2014; sowie alle zugehörigen Abhängigkeiten Ko & #x 2014; installiert werden und werden Sie aufgefordert, die Lizenzbedingungen zu akzeptieren.
11. Überprüfen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **ich stimme**.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

> [!NOTE]
> Wenn das Verfahren zum Bereitstellen der Verwendung von Tools wie VSDBCMD.exe oder SQLCMD.exe enthält, müssen Sie sicherstellen, dass diese auf dem Buildserver installiert werden. VSDBCMD.exe ist ein Tool für Visual Studio und wird in der Regel mit dem Server hinzugefügt, bei der Installation von Team Foundation Build. SQLCMD.exe handelt es sich um eine SQL Server-Tool. Sie können eine eigenständige Version des von SQLCMD.exe Herunterladen der [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) Seite.


## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt ist Ihrem Build-Server zum Starten der Erstellung und Bereitstellung von Webanwendungsprojekten bereit. Im nächsten Thema [erstellen eine erstellen, unterstützt Definitionsbereitstellung](creating-a-build-definition-that-supports-deployment.md), beschreibt, wie zum Erstellen und konfigurieren eine Builddefinition, um zu steuern, wann und wie Ihre Projekte erstellt und bereitgestellt werden.

## <a name="further-reading"></a>Weiterführende Themen

Weitere allgemeine Anleitungen zum Arbeiten mit Team Build finden Sie unter [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

>[!div class="step-by-step"]
[Zurück](adding-content-to-source-control.md)
[Weiter](creating-a-build-definition-that-supports-deployment.md)
