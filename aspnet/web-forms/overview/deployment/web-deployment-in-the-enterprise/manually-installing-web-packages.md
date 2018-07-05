---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Manuelles Installieren von Webpaketen | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema beschreibt, wie Sie manuell ein Webbereitstellungspakets in IIS (Internetinformationsdienste) importieren. Das Thema erstellen und die Web-Anwendung verpacken...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 7fd8060104ca4e02919a3fbac135edb3e9396c64
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833004"
---
<a name="manually-installing-web-packages"></a>Manuelles Installieren von Webpaketen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt, wie Sie manuell ein Webbereitstellungspakets in IIS (Internetinformationsdienste) importieren.
> 
> Das Thema [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md) beschrieben wie die IIS-Webbereitstellungstool (Web Deploy), in Verbindung mit der Microsoft Build Engine (MSBuild) und das Web Publishing-Pipeline (WPP), können Sie Packen Ihrer Webanwendungsprojekte in eine einzelne Zip-Datei. Diese Datei, die so genannte eines Webbereitstellungspakets (oder einfach ein Bereitstellungspaket) enthält alle Inhalts- und Konfigurationsdatenbanken Informationen, die IIS benötigt, um Ihre Webanwendung auf einem Webserver neu zu erstellen.
> 
> Nachdem Sie einem Webbereitstellungspaket erstellt haben, können Sie es auf einem IIS-Server, auf verschiedene Weise veröffentlichen. In vielen Szenarien sollten Sie nutzen die Integrationspunkte zwischen MSBuild, Apps und Web Deploy zum Erstellen und installieren Web-Pakete Remote, als Teil eines automatisierten oder Schritt für Schritt Build & Deployment-Prozesses. Dieser Prozess wird hier beschrieben [Bereitstellen von Webpaketen](deploying-web-packages.md). Allerdings ist dies nicht immer möglich. Nehmen wir an, dass Sie eine Webanwendung in einer produktionsumgebung Internetzugriff bereitstellen möchten. Aus Sicherheitsgründen ist solche in eine produktionsumgebung auf die mindestens wahrscheinlich hinter einer Firewall in einem Subnetz, das auf dem Buildserver, in einem Umkreisnetzwerk (auch bekannt als DMZ, demilitarisierte Zone und überwachtes Subnetz bezeichnet) getrennt ist. In vielen Fällen werden die produktionsumgebung in einer anderen Domäne oder in einem physisch isolierten Netzwerk.
> 
> In diesen Szenarien kann Ihre einzige Option sein, port des Pakets auf dem Zielserver und manuell in IIS importieren. Obwohl dieser Ansatz, automatisierten Bereitstellung ausschließt, ist es immer noch eine höchst effektive Technik zum Veröffentlichen einer Webanwendung&#x2014;Sie einfach eine einzelne Zip-Datei auf Ihren Webserver kopieren und verwenden Sie einen Assistenten, um Sie bei den Importvorgang zu unterstützen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

## <a name="task-overview"></a>Übersicht über den Task

Sie müssen zum Ausführen dieser Aufgaben auf hoher Ebene, um eines Webbereitstellungspakets in IIS zu importieren:

- Erstellen eines Webbereitstellungspakets mit der MSBuild-Befehlszeile, Team Build oder Visual Studio 2010.
- Kopieren des Pakets an den Ziel-Webserver.
- Verwenden Sie den Assistenten zum Importieren von Paket in IIS-Manager zum Installieren des Pakets, und geben Sie Werte für Variablen wie Verbindungszeichenfolgen und Dienstendpunkte.

In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit den Konzepten von Webpaketen, Web Deploy und die WPP vertraut sind. Weitere Informationen finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).

> [!NOTE]
> In diesem Thema wird am besten verwendet, in Verbindung mit [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Offline-Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), die erklärt, wie die erforderlichen Komponenten installieren und Vorbereiten einer IIS-Website für den Paketimport.


## <a name="create-a-web-deployment-package"></a>Erstellen eines Webbereitstellungspakets

Die erste Aufgabe ist die Erstellung ein Webbereitstellungspakets für das Webprojekt für die Anwendung, die Sie bereitstellen möchten. Sie können Web-Pakete in einer Vielzahl von Möglichkeiten erstellen.

**Ansatz 1: Erstellen Sie ein Paket als Teil des Buildprozesses mit Visual Studio**

Sie können konfigurieren, dass das Webanwendungsprojekt, zum Erstellen eines Webbereitstellungspakets nach jedem Build über die **Web packen/veröffentlichen** Registerkarte auf den Eigenschaftenseiten des Projekts. Dieser Prozess wird hier beschrieben [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).

**Ansatz 2: Erstellen Sie ein Paket als Teil des Buildprozesses mit MSBuild**

Wenn Sie das Webanwendungsprojekt mithilfe von MSBuild direkt, entweder über eine benutzerdefinierte MSBuild-Projektdatei oder über die Befehlszeile erstellen können Sie einem Webbereitstellungspaket als Teil des Buildprozesses erstellen, durch Einschließen der **DeployOnBuild = True** und **DeployTarget = Paket** Eigenschaften im Befehl. Dieser Prozess wird hier beschrieben [Verständnis des Prozesses erstellen](understanding-the-build-process.md).

**Methode 3: Erstellen Sie ein Paket bei Bedarf in Visual Studio**

Sie können einem Webbereitstellungspaket für ein Webprojekt für die Anwendung zu einem beliebigen Zeitpunkt in Visual Studio 2010 erstellen. Klicken Sie hierzu in der **Projektmappen-Explorer** rechten Maustaste auf das Webanwendungsprojekt, und klicken Sie dann auf **Bereitstellungspaket erstellen**.

![](manually-installing-web-packages/_static/image1.png)

**Methode 4: Erstellen Sie ein Paket bei Bedarf über die Befehlszeile**

Sie können einem Webbereitstellungspaket über die Befehlszeile erstellen, durch den Aufruf der **Paket** Ziel auf das Webanwendungsprojekt mithilfe von MSBuild. Der Befehl sieht folgendermaßen aus:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Je nachdem, was Sie Ansatz verwenden, das Endergebnis ist identisch. Die WPP wird einem Webbereitstellungspaket als Zipdatei zusammen mit verschiedenen unterstützenden Ressourcen, die in den Ausgabeordner für das Webanwendungsprojekt erstellt.

![](manually-installing-web-packages/_static/image2.png)

Wenn Sie planen, das Webpaket manuell zu importieren, müssen Sie die Zip-Datei. Kopieren Sie diese Datei auf Ihren Zielwebserver und können Sie den Importvorgang zu starten.

## <a name="import-a-web-package-into-iis"></a>Importieren Sie eine Webpaket in IIS

Sie können im nächste Verfahren verwenden, einem Webbereitstellungspaket aus dem lokalen Dateisystem in einer IIS-Website zu importieren. Bevor Sie dieses Verfahren ausführen, stellen Sie sicher, dass Sie haben:

- Kopiert das Webbereitstellungspaket an den Webserver.
- Konfiguriert ein IIS-Webserver zum Hosten Ihrer Anwendung.

Weitere Informationen zum Konfigurieren eines IIS-Webservers zur Unterstützung von Web-Bereitstellungspakete finden Sie unter [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Offline-Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**Um einem Webbereitstellungspaket mit IIS-Manager zu importieren.**

1. Im IIS-Manager in der **Verbindungen** Bereich mit der rechten Maustaste in der IIS-Website, zeigen Sie auf **bereitstellen**, und klicken Sie dann auf **Anwendung importieren**.

    ![](manually-installing-web-packages/_static/image3.png)
2. Im Import Application-Paket-Assistenten auf die **wählen Sie das Paket** Seite navigieren Sie zum Speicherort Ihrer Web-Bereitstellungspaket, und klicken Sie dann auf **Weiter**.
3. Auf der **wählen Sie den Inhalt des Pakets** Seite, deaktivieren Sie alle Inhalte, die Sie nicht benötigen, und klicken Sie dann auf **Weiter**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > In vielen Fällen können Sie nicht alles importieren möchten, die mit einem Webbereitstellungspaket geliefert wird. Sie möchten beispielsweise nicht zulassen Web Deploy, um die zugeordnete Datenbank zu ersetzen.  
    > Die **Berechtigungen** Einträge Festlegen von Berechtigungen auf dem Zielsystem-Datei, um sicherzustellen, dass die Identität des Anwendungspools den physischen Ordner zugreifen kann, in dem der Inhalt der Website gespeichert. Darüber hinaus wird der Benutzer die anonyme Authentifizierung lesen gewährt Berechtigung in den Ordner, damit die Anwendung, die Typdateien Multipurpose Internet Mail Extensions (MIME) verarbeiten kann. Falls gewünscht, können Sie diese Einträge entfernen und konfigurieren die Berechtigungen manuell.
4. Auf der **Anwendungspaketinformationen** Seite, geben die angeforderten Informationen.

    ![](manually-installing-web-packages/_static/image5.png)
5. Wenn Sie ein Webpaket erstellen, wird die WPP analysiert die Konfigurationsdatei für Ihre Anwendung und erkennt alle Variablen, wie Verbindungszeichenfolgen und Dienstendpunkte. In diesem Fall gilt Folgendes:

    1. **Anwendungspfad** ist der IIS-Pfad, in dem Sie Ihre Anwendung installieren möchten. Diese Einstellung ist für alle Pakete, die die WPP erstellt.
    2. **ContactService Dienstendpunktadresse** ist die Adresse, die die Anwendung für die Kommunikation mit den bereitgestellten WCF-Dienst verwenden soll. Diese Einstellung entspricht einem Eintrag in der *"Web.config"* Datei.
    3. Die erste **Verbindungszeichenfolge** Einstellung ist die Verbindungszeichenfolge, die Web Deploy verwenden soll, zum Bereitstellen der Datenbank, die der Anwendung (in diesem Fall einer ASP.NET-Mitgliedschaftsdatenbank) zugeordnet. Diese Einstellung entspricht der Einstellung für die **SQL packen/veröffentlichen** Registerkarte in Visual Studio.
    4. Die zweite **Verbindungszeichenfolge** Einstellung ist die Verbindungszeichenfolge, die die Anwendung tatsächlich verwendet, um mit der Datenbank zu kommunizieren, wenn er ausgeführt wird. Dies entspricht einer Verbindungszeichenfolgeneintrag in die *"Web.config"* Datei.

        > [!NOTE]
        > Weitere Informationen dazu, woher diese Parameter stammen, finden Sie unter [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md).
6. Klicken Sie auf **Weiter**.
7. Ist dies nicht der erstmaligen Bereitstellung der Anwendung auf diese Website haben, werden Sie aufgefordert werden, um anzugeben, ob Sie alle vorhandenen Inhalte vor der Installation löschen möchten. Wählen Sie die Option, die für Ihre Anforderungen geeignet ist, und klicken Sie dann auf **Weiter**.

    ![](manually-installing-web-packages/_static/image6.png)
8. Wenn Sie IIS installieren des Pakets abgeschlossen ist, klicken Sie auf **Fertig stellen**.

    ![](manually-installing-web-packages/_static/image7.png)

An diesem Punkt haben Sie Ihre Webanwendung in IIS erfolgreich veröffentlicht.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie ein Webbereitstellungspakets in einer IIS-Website mit IIS-Manager zu importieren. Dieser Ansatz für die Veröffentlichung von Web-Anwendung ist geeignet, wenn Sicherheit oder Infrastruktur-Einschränkungen nicht möglich oder unerwünscht Remotebereitstellung vornehmen.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Konfigurieren eines IIS-Webservers zum manuellen Importieren einer Web-Paket zu unterstützen, finden Sie unter [Konfigurieren eines Webservers für Bereitstellen von Webpublishing (Offline-Bereitstellung)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Weitere allgemeine Anleitungen zum Bereitstellen von Webpaketen finden Sie [Exemplarische Vorgehensweise: Bereitstellen einer Web Application-Projekt mithilfe eines Webbereitstellungspakets (Teil 1 von 4)](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Vorherige](creating-and-running-a-deployment-command-file.md)
