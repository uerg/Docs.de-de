---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Manuelles Installieren von Webpaketen | Microsoft Docs
author: jrjlee
description: Dieses Thema beschreibt, wie Sie manuell ein Webbereitstellungspakets in Internet Information Services (IIS) importieren. Das Thema erstellen und Packen Web enn...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="manually-installing-web-packages"></a>Manuelles Installieren von Webpaketen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie manuell ein Webbereitstellungspakets in Internet Information Services (IIS) importieren.
> 
> Das Thema [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md) beschrieben wie IIS Webbereitstellungstool (Web Deploy) in Verbindung mit dem Microsoft Build Engine (MSBuild) und Web veröffentlichen Pipeline (WPP), können Sie Verpacken Ihrer Webanwendungsprojekte in eine einzelne Zip-Datei. Diese Datei, bekannt als ein Webbereitstellungspaket (oder einfach ein Bereitstellungspaket) enthält alle Inhalts- und Konfigurationsdatenbanken Informationen, die IIS benötigt, um Ihre Webanwendung auf einem Webserver neu zu erstellen.
> 
> Nach der Erstellung eines Webbereitstellungspakets können Sie es auf einem Server mit IIS auf verschiedene Weise veröffentlichen. In einer Vielzahl von Szenarien sollten Sie nutzen die Integrationspunkte zwischen MSBuild, WPP und Web Deploy zum Erstellen und installieren Webpakete Remote im Rahmen einer automatisierten Prozesses oder eines einstufiger Build- und Bereitstellungsprozess. Dieser Prozess wird beschrieben, [Bereitstellen von Webpaketen](deploying-web-packages.md). Allerdings ist dies nicht immer möglich. Angenommen Sie, eine Webanwendung in einer produktionsumgebung Internetzugriff bereitgestellt werden soll. Aus Gründen der Sicherheit wird solche eine produktiven Umgebung auf die sehr am wenigsten wahrscheinlichen hinter einer Firewall in einem Subnetz sein, die aus dem Build-Server in einem Umkreisnetzwerk (auch bekannt als DMZ, demilitarisierte Zone und überwachtes Subnetz bezeichnet) getrennt ist. In vielen Fällen werden die produktionsumgebung in einer anderen Domäne oder in einem physisch isoliertes Netzwerk.
> 
> In diesen Szenarien möglicherweise Ihre einzige Option Portieren das Paket auf dem Zielserver, und importieren sie manuell in IIS. Obwohl dieser Ansatz automatisierte Bereitstellung ausschließt, ist immer noch ein äußerst effektiv Verfahren zum Veröffentlichen einer Webanwendung&#x2014;Sie einfach eine einzelne Zip-Datei auf Ihren Webserver kopieren und ein Assistent führt Sie durch den Importvorgang zu verwenden.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

Sie müssen diese allgemeinen Aufgaben ausführen, um ein Webbereitstellungspaket in IIS importieren:

- Erstellen eines Webbereitstellungspakets mit der MSBuild-Befehlszeile, Team Build oder Visual Studio 2010.
- Kopieren Sie das Paket mit dem Ziel-Webserver.
- Verwenden Sie den Assistenten zum Importieren von Anwendungen Paket im IIS-Manager installieren Sie das Webpaket, und geben Sie Werte für Variablen wie Verbindungszeichenfolgen und Dienstendpunkte.

In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit der grundlegenden Konzepte Webpaketen, Web Deploy und die WPP vertraut sind. Weitere Informationen finden Sie unter [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).

> [!NOTE]
> In diesem Thema wird am besten in Verbindung mit verwendet [konfigurieren Sie einen Webserver für bereitstellen Webveröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), die erklärt, wie die erforderlichen Komponenten installieren und Vorbereiten von einer IIS-Website für das Paket importieren.


## <a name="create-a-web-deployment-package"></a>Erstellen eines Webbereitstellungspakets

Die erste Aufgabe besteht darin ein Webbereitstellungspaket für das Webanwendungsprojekt zu erstellen, die Sie bereitstellen möchten. Sie können eine Vielzahl von Möglichkeiten Webpaketen erstellen.

**Vorgehen 1: Erstellen Sie ein Paket als Teil des Buildprozesses mit Visual Studio**

Sie können das Webanwendungsprojekt zum Erstellen eines Webbereitstellungspakets nach jedem Build durch Konfigurieren der **Web packen/veröffentlichen** Registerkarte auf den Eigenschaftenseiten des Projekts. Dieser Prozess wird beschrieben, [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).

**Vorgehen 2: Erstellen Sie ein Paket als Teil des Buildprozesses mit MSBuild**

Wenn das Webanwendungsprojekt erstellen, mithilfe von MSBuild direkt, entweder durch eine benutzerdefinierte MSBuild-Projektdatei oder über die Befehlszeile können Sie ein Webbereitstellungspaket als Teil des Buildprozesses erstellen, indem Sie einschließlich der **DeployOnBuild = "true"** und **DeployTarget = Paket** Eigenschaften in Ihrem Befehl. Dieser Prozess wird beschrieben, [Verständnis des Build-Prozesses](understanding-the-build-process.md).

**Vorgehensweise 3: Erstellen Sie ein Paket bei Bedarf in Visual Studio**

Sie können jederzeit in Visual Studio 2010 ein Webbereitstellungspaket für ein Webprojekt für die Anwendung erstellen. Klicken Sie hierzu in der **Projektmappen-Explorer** rechten Maustaste auf das Webanwendungsprojekt, und klicken Sie dann auf **Bereitstellungspaket erstellen**.

![](manually-installing-web-packages/_static/image1.png)

**Methode 4: Erstellen Sie ein Paket bei Bedarf über die Befehlszeile**

Sie können ein Webbereitstellungspaket über die Befehlszeile erstellen, durch den Aufruf der **Paket** Ziel für das Webanwendungsprojekt mithilfe von MSBuild. Der Befehl sollte diesem ähneln:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Je nachdem, was Sie Ansatz verwenden, das Endergebnis ist identisch. Die WPP erstellt ein Webbereitstellungspaket als Zip-Datei zusammen mit verschiedenen hilfreiche Ressourcen, in dem Ausgabeordner für Ihr Webprojekt für die Anwendung.

![](manually-installing-web-packages/_static/image2.png)

Wenn Sie beabsichtigen, das Paket manuell zu importieren, müssen Sie die Zip-Datei. Kopieren Sie diese Datei auf dem Ziel-Webserver und den Importvorgang zu starten.

## <a name="import-a-web-package-into-iis"></a>Importieren Sie eine Web-Paket in IIS

Das nächste Verfahren können Sie um ein Webbereitstellungspaket aus dem lokalen Dateisystem in einer IIS-Website zu importieren. Bevor Sie dieses Verfahren ausführen, stellen Sie sicher, dass Sie verfügen:

- Kopiert das Webbereitstellungspaket an den Webserver.
- Konfiguriert ein IIS-Webserver zum Hosten der Anwendung an.

Weitere Informationen zum Konfigurieren eines IIS-Webservers zur Unterstützung von Web-Bereitstellungspakete finden Sie unter [konfigurieren Sie einen Webserver für bereitstellen Webveröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**So importieren Sie ein Webbereitstellungspaket mit IIS-Manager**

1. Im IIS-Manager in der **Verbindungen** Bereich mit der rechten Maustaste in der IIS-Website, zeigen Sie auf **bereitstellen**, und klicken Sie dann auf **Anwendung importieren**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Im Anwendungs-Paket-Assistenten importieren auf die **wählen Sie das Paket** Seite navigieren Sie zum Speicherort der Ihre Web-Bereitstellungspaket, und klicken Sie dann auf **Weiter**.
3. Auf der **wählen Sie den Inhalt des Pakets** Seite, deaktivieren Sie alle Inhalte, die Sie nicht benötigen, und klicken Sie dann auf **Weiter**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In vielen Fällen möchten Sie möglicherweise nicht alles, was zu importieren, die im Lieferumfang eines Webbereitstellungspakets. Sie möchten z. B. nicht zulassen Web Deploy, um die zugeordnete Datenbank zu ersetzen.  
    > Die **gewähren** Einträge Festlegen von Berechtigungen auf dem Zielsystem-Datei, um sicherzustellen, dass die Identität des Anwendungspools den physischen Ordner zugreifen kann, in dem der Websiteinhalt gespeichert. Darüber hinaus wird der Benutzer für die anonyme Authentifizierung für den Ordner erteilt, damit die Anwendung Multipurpose Internet Mail Extensions (MIME) Typdateien dienen können gewährt gelesen. Falls gewünscht, können Sie diese Einträge zu entfernen und konfigurieren die Berechtigungen manuell.
4. Auf der **Anwendungspaketinformationen** Seite, stellen die angeforderten Informationen.

    ![](manually-installing-web-packages/_static/image5.png)
5. Wenn Sie eine Web-Paket erstellen, wird WPP analysiert die Konfigurationsdatei für Ihre Anwendung und erkennt alle Variablen, wie z. B. Verbindungszeichenfolgen und Dienstendpunkte. In diesem Fall gilt Folgendes:

    1. **Anwendungspfad** ist der IIS-Pfad, in dem die Anwendung installiert werden soll. Diese Einstellung ist für alle Pakete, die die WPP erstellt.
    2. **ContactService Dienstendpunktadresse** ist die Adresse, die die Anwendung für die Kommunikation mit den bereitgestellten WCF-Dienst verwendet werden sollen. Diese Einstellung entspricht keinem Eintrag in der *"Web.config"* Datei.
    3. Die erste **Verbindungszeichenfolge** Einstellung wird die Verbindungszeichenfolge, die Web Deploy verwenden soll, zum Bereitstellen der Datenbank, die der Anwendung (in diesem Fall einer ASP.NET-Mitgliedschaftsdatenbank) zugeordnet. Diese Einstellung entspricht der Einstellung für die **SQL packen/veröffentlichen** Registerkarte in Visual Studio.
    4. Die zweite **Verbindungszeichenfolge** Einstellung wird die Verbindungszeichenfolge, mit der die Anwendung tatsächlich mit der Datenbank zu kommunizieren, wenn er ausgeführt wird. Dies entspricht einer Verbindungszeichenfolgeneintrag in der *"Web.config"* Datei.

        > [!NOTE]
        > Weitere Informationen, woher diese Parameter stammen, finden Sie unter [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md).
6. Klicken Sie auf **Weiter**.
7. Ist dies nicht der ersten Mal mit dem Sie die Anwendung auf diese Website bereitgestellt haben, werden Sie aufgefordert werden, um anzugeben, ob Sie alle vorhandenen Inhalte vor der Installation löschen möchten. Wählen Sie die Option, die für Ihre Anforderungen geeignet ist, und klicken Sie dann auf **Weiter**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Wenn Sie IIS installieren des Pakets abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![](manually-installing-web-packages/_static/image7.png)

An diesem Punkt haben Sie Ihre Webanwendungen in IIS erfolgreich veröffentlicht.

## <a name="conclusion"></a>Schlussbemerkung

Dieses Thema beschreibt, wie Sie ein Webbereitstellungspaket in einer IIS-Website mit dem IIS-Manager zu importieren. Diese Vorgehensweise bei der Veröffentlichung von Webdienst-Anwendung ist geeignet, wenn Sicherheits- oder Infrastruktur Einschränkungen nicht möglich oder unerwünscht Remotebereitstellung vornehmen.

## <a name="further-reading"></a>Weiterführende Themen

Anleitung zum Konfigurieren eines IIS-Webservers zum manuellen Importieren einer Web-Paket zu unterstützen, finden Sie unter [konfigurieren Sie einen Webserver für bereitstellen Webveröffentlichung (Offline Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Weitere allgemeine Anleitungen zum Bereitstellen von Webpaketen finden Sie unter [Exemplarische Vorgehensweise: Bereitstellen einer Web Application Project Using ein Webbereitstellungspaket (Teil 1 von 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Vorherige](creating-and-running-a-deployment-command-file.md)
