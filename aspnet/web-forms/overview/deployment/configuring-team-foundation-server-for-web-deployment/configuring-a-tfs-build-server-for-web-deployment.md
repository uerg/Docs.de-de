---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurieren eine TFS-Buildserver für die Webbereitstellung | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema beschreibt das Vorbereiten von einem Team Foundation Server (TFS)-Build-Server zum Erstellen und Bereitstellen Ihrer Lösungen mithilfe von Team Build und der Internet-Informationen...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7f57ad0392a068964bb910fbbaafea105fdbb3d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841340"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurieren eines TFS-Buildservers für die Webbereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie einen Team Foundation Server (TFS)-Build-Server zum Erstellen und Bereitstellen Ihrer Lösungen mithilfe von Team Build und das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) vorbereiten.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Zur Vorbereitung eines Buildservers zum Erstellen und Ihre Lösungen bereitstellen, müssen Sie:

- Installieren Sie und konfigurieren Sie der TFS-Build-Diensts.
- Installieren Sie Visual Studio 2010.
- Installieren Sie Produkte oder Komponenten, die erforderlich sind, um die Projektmappe, ebenso wie Versionen von .NET Framework oder ASP.NET MVC erstellen.
- Installieren Sie Web Deploy 2.0 oder höher.

In diesem Thema erfahren Sie, wie diese Verfahren ausführen, oder zeigen Sie auf andere Ressourcen, in dem sie vorhanden sind. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird angenommen, dass:

- Sie beginnen mit einem "sauberen" Server-Build Ausführen von Windows Server 2008 R2 Service Pack 1.
- Der Server ist die Domäne mit einer statischen IP-Adresse.
- Sie haben die TFS-Anwendungsebene auf einem separaten Server installiert, wie in beschrieben [webbasierte Unternehmensbereitstellung: Szenarioübersicht](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Wem diese Verfahren werden?

In den meisten Fällen wird ein TFS-Administrator für das Konfigurieren von Buildservern zuständig sein. In einigen Fällen kann das Entwicklerteam den Besitz von bestimmten Buildservern dauern.

## <a name="install-and-configure-the-tfs-build-service"></a>Installieren Sie und konfigurieren Sie der TFS-Build-Diensts

Wenn Sie einen Buildserver konfigurieren, ist Ihre erste Aufgabe zum Installieren und Konfigurieren der TFS-Build-Diensts. Im Rahmen dieses Prozesses müssen Sie:

- Installieren Sie den TFS-Build-Dienst, und Konfigurieren eines Dienstkontos. Alle BuildTasks, einschließlich der Bereitstellung werden ausgeführt, mit der Identität des Build Service-Konto.
- Erstellen Sie eine *Buildcontroller* und einem oder mehreren *build-Agents*. Jeder Buildcontroller verwaltet einen Satz von Build-Agents. Wenn Sie einen Build in die Warteschlange, weist der Buildcontroller die Build-Aufgabe an eine verfügbare Build-Agent. Jede teamprojektauflistung in TFS wird ein einzelner Buildcontroller zugeordnet.
- Konfigurieren Sie einen Ablageordner für die Buildausgaben. Dies ist eine Netzwerkfreigabe. Alle Ausgaben, wie bei Webpaketen für die Bereitstellung zu erstellen, werden an den Ablageordner gesendet.

Die [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) Kapitel auf MSDN enthält alle Ressourcen, die Sie benötigen, um diese Aufgaben ausführen:

- Eine konzeptionelle Übersicht über Team Foundation Build, einschließlich den Builddienst Buildcontroller und Build-Agents finden Sie unter [Grundlegendes zu einem Team Foundation Build-System](https://msdn.microsoft.com/library/dd793166.aspx).
- Informationen zum Installieren und konfigurieren den Build-Dienst finden Sie unter [Konfigurieren eines Buildcomputers](https://msdn.microsoft.com/library/ms181712.aspx).
- Informationen zum Erstellen von Buildcontrollern finden Sie unter [erstellen und Verwenden von einem Buildcontroller](https://msdn.microsoft.com/library/ee330987.aspx).
- Informationen zum Erstellen von Build-Agents finden Sie unter [erstellen und Verwenden von Build-Agents](https://msdn.microsoft.com/library/bb399135.aspx).
- Informationen zum Erstellen und Konfigurieren der Ablageordner, finden Sie unter [Einrichten von Ablageordnern](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Installieren der erforderlichen Produkte und Komponenten

Um dem Build-Server für Ihre Lösungen zu aktivieren, installieren Sie alle Produkte, Komponenten oder Assemblys, die Ihre Lösung erforderlich sind. Bevor Sie alle webplattformkomponenten installieren, sollten Sie Visual Studio 2010 (beliebige Version) auf dem Buildserver installieren. Dadurch wird sichergestellt, dass die Kerndateien von Microsoft Build Engine (MSBuild) Ziel und die Zieldateien Web Publishing Pipeline (WPP) für den Builddienst verfügbar sind. Visual Studio-Installer sollten auch Web Deploy installieren, die Sie benötigen, wenn Sie Web-Pakete als Teil des Buildprozesses bereitstellen möchten.

Die beste Möglichkeit, allgemeine Plattform-Webkomponenten installieren ist die Verwendung der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118). Dadurch wird sichergestellt, dass Sie die neueste Version jedes Produkts installieren, und auch automatisch erkannt und installiert alle erforderlichen Komponenten für jedes Produkt. Im Fall von der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Lösung, sollte Sie den Webplattform-Installer verwenden, um diese Produkte und Komponenten zu installieren:

- **.NET Framework 4.0**. Dies ist erforderlich, um Anwendungen auszuführen, die in dieser Version von .NET Framework erstellt wurden.
- **Webbereitstellungstool 2.1 oder höher**. Dadurch wird Web Deploy (und die zugrunde liegende ausführbare Datei, die MSDeploy.exe) auf dem Server installiert. Im Rahmen dieser Prozess wird es installiert und startet die Web-Agent-Dienst. Dieser Dienst können Sie die Webpakete von einem Remotecomputer bereitstellen.
- **ASP.NET MVC 3**. Dadurch werden die Assemblys, die Sie zum Ausführen von ASP.NET MVC 3-Anwendungen müssen installiert.

**Installieren der erforderlichen Produkte und Komponenten**

1. Installieren Sie Visual Studio 2010. Wenn Sie aufgefordert werden, die zu installierenden Funktionen auswählen, sollten Sie Folgendes hinzufügen:

    1. Alle Programmiersprachen, die Sie benötigen, um zu kompilieren.
    2. Visual Web Developer. Dadurch wird sichergestellt, dass der Buildserver die WPP Ziele hinzugefügt werden.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Klicken Sie nach Abschluss der Installation von Visual Studio 2010 herunterladen und installieren [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (sofern er noch nicht in den Installationsmedien enthalten ist).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1, löst einen Fehler, der MSBuild für die Suche nach der ausführbaren Datei MSDeploy verhindern kann.
3. Herunterladen und Starten der [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).
4. Am oberen Rand der **Web Platform Installer 3.0** Fenster, klicken Sie auf **Produkte**.
5. Klicken Sie auf der linken Seite des Fensters, klicken Sie im Navigationsbereich auf **Frameworks**.
6. In der **Microsoft .NET Framework 4** Zeile, wenn .NET Framework nicht bereits installiert ist, klicken Sie auf **hinzufügen**.

    > [!NOTE]
    > Möglicherweise haben Sie bereits über Windows Update .NET Framework 4.0 installiert. Wenn ein Produkt oder eine Komponente bereits installiert ist, den Webplattform-Installer wird angegeben, dies durch Ersetzen der **hinzufügen** Schaltfläche mit dem Text **installiert**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. In der **ASP.NET MVC 3 (Visual Studio 2010)** auf **hinzufügen**.
8. Klicken Sie im Navigationsbereich auf **Server**.
9. In der **Web Deployment Tool 2.1** auf **hinzufügen**.
10. Klicken Sie auf **Installieren**. Der Webplattform-Installer zeigt Ihnen eine Liste der Produkte&#x2014;zusammen mit verknüpften Abhängigkeiten&#x2014;installiert werden und werden Sie aufgefordert, den Lizenzbedingungen zustimmen.
11. Lesen Sie die Lizenzbedingungen, und wenn Sie den Bedingungen zustimmen, klicken Sie auf **akzeptieren**.
12. Wenn die Installation abgeschlossen ist, klicken Sie auf **Fertig stellen**, und schließen Sie dann die **Web Platform Installer 3.0** Fenster.

> [!NOTE]
> Wenn Ihr Bereitstellungsprozess die Verwendung von Tools wie VSDBCMD.exe oder SQLCMD.exe enthält, müssen Sie sicherstellen, dass diese auf dem Buildserver installiert werden. VSDBCMD.exe ist ein Visual Studio-Tool und wird in der Regel mit dem Server hinzugefügt, bei der Installation von Team Foundation Build. SQLCMD.exe ist ein SQL Server-Tool. Sie können eine eigenständige Version von SQLCMD.exe aus der [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) Seite.


## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt ist der Buildserver mit der Erstellung und Bereitstellung von Webanwendungsprojekten beginnen. Im nächsten Thema, [Erstellen einer Build-Definition, unterstützt Bereitstellung](creating-a-build-definition-that-supports-deployment.md), beschreibt, wie zum Erstellen und konfigurieren eine Builddefinition zum Steuern, wann und wie Ihre Projekte erstellt und bereitgestellt werden.

## <a name="further-reading"></a>Weiterführende Themen

Weitere allgemeine Anleitungen zum Arbeiten mit Team Build, finden Sie unter [Verwalten von Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Zurück](adding-content-to-source-control.md)
> [Weiter](creating-a-build-definition-that-supports-deployment.md)
