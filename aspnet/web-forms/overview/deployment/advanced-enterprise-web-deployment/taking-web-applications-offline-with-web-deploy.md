---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Offlineschalten von Webanwendungen mit Web bereitstellen | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie eine Webanwendung für die Dauer einer automatisierten Bereitstellung mithilfe der Internetinformationsdienste (Internet Information Services, IIS) Web Depl offline...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: b8dc1ff26bbbe7dee1ee90fd929b2e98de5360db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829514"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Offlineschalten von Webanwendungen mit Web bereitstellen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie eine Webanwendung für die Dauer einer automatisierten Bereitstellung über das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) offline nutzen können. Benutzer, die an die Webanwendung durchsuchen werden umgeleitet, um eine *App\_offline.htm* Datei, bis die Bereitstellung abgeschlossen ist.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In vielen Szenarien sollten Sie eine Webanwendung offline zu schalten, während Sie Änderungen an den zugehörigen Komponenten, wie Datenbanken oder Webdiensten vornehmen. In der Regel in IIS und ASP.NET können Sie dazu platzieren eine Datei namens *App\_offline.htm* im Stammordner der IIS-Website oder Web-Anwendung. Die *App\_offline.htm* -Datei ist eine standard-HTML-Datei und enthält in der Regel eine einfache Nachricht mit dem Hinweis an, dass der Standort aufgrund von Wartungsarbeiten vorübergehend nicht verfügbar ist. Während der *App\_offline.htm* Datei, die im Stammordner der Website vorhanden ist, wird IIS automatisch alle Anforderungen an die Datei umgeleitet. Wenn Sie Updates vorgenommen haben, entfernen Sie die *App\_offline.htm* -Datei und die Website wird fortgesetzt, Verarbeiten von Anforderungen wie gewohnt.

Wenn Sie Web Deploy zum Ausführen von automatisierten oder Schritt für Schritt Bereitstellungen in einer zielumgebung verwenden, möchten Sie möglicherweise integrieren, hinzufügen und Entfernen von der *App\_offline.htm* -Datei in den Bereitstellungsprozess. Zu diesem Zweck müssen Sie diese allgemeinen Aufgaben ausführen:

- In der Projektdatei Microsoft Build Engine (MSBuild) kopiert werden sollen, verwenden, um den Bereitstellungsprozess zu steuern, erstellen ein MSBuild-Ziel, die eine *App\_offline.htm* Datei auf den Zielserver vor alle Bereitstellungsaufgaben beginnen Sie mit.
- Fügen Sie einen anderen MSBuild-Ziel, das entfernt die *App\_offline.htm* Datei auf dem Zielserver, nachdem alle Bereitstellungsaufgaben abgeschlossen sind.
- Erstellen Sie in das Webanwendungsprojekt, eine *. wpp.targets* -Datei, die wird, dass sichergestellt ein *App\_offline.htm* Datei wird in das Bereitstellungspaket hinzugefügt, wenn die Web Deploy aufgerufen wird.

In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits eine Lösung erstellt haben, die mindestens ein Webanwendungsprojekt enthält und Sie eine benutzerdefinierte Projektdatei verwenden, um den Bereitstellungsprozess zu steuern, wie in beschrieben [Webbereitstellung in der Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternativ können Sie die [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe zu den Beispielen im Thema zu folgen.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Hinzufügen einer App\_Offline-Datei in ein Webanwendungsprojekt

Die erste Aufgabe, die erforderlich ist, Hinzufügen einer *App\_offline* Datei zu Ihrem Webprojekt für die Anwendung:

- Um zu verhindern, dass die Datei in den Entwicklungsprozess stören (nicht soll Ihre Anwendung dauerhaft offline sein), rufen Sie es etwas anders als *App\_offline.htm*. Sie können z. B. benennen Sie die Datei *App\_offline template.htm*.
- Um zu verhindern, dass die Datei als bereitgestellt – ist, sollten Sie die Buildaktion auf festlegen **keine**.

**Hinzufügen eine App\_offline-Datei in ein Webanwendungsprojekt**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste in des Webanwendungsprojekts, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **HTML-Seite**.
4. In der **Namen** geben **App\_offline template.htm**, und klicken Sie dann auf **hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Hinzufügen von einfacher HTML-Code um Benutzer zu informieren, dass die Anwendung nicht verfügbar ist, und speichern Sie die Datei. Schließen Sie keine serverseitigen Tags (z. B. alle Tags, die mit dem Präfix "Asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. In der **Projektmappen-Explorer** rechten Maustaste auf die neue Datei, und klicken Sie dann auf **Eigenschaften**.
7. In der **Eigenschaften** Fenster in der **Buildvorgang** Zeile **keine**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Bereitstellen und Löschen einer App\_Offline-Datei

Der nächste Schritt ist zum Ändern Ihrer Bereitstellungslogik kopieren Sie die Datei auf den Zielserver am Anfang des Bereitstellungsprozesses und am Ende entfernt werden.

> [!NOTE]
> Im nächste Verfahren wird davon ausgegangen, dass Sie eine benutzerdefinierte MSBuild-Projektdatei verwenden, um den Bereitstellungsprozess steuern, wie in beschrieben [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Wenn Sie direkt von Visual Studio bereitstellen, müssen Sie einen anderen Ansatz zu verwenden. Sayed Ibrahim Hashimi beschreibt einen solchen Ansatz in [Vorgehensweise nehmen Ihre App Offline während der Webveröffentlichung](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Bereitstellen einer *App\_offline* Datei mit einer Ziel-IIS-Website, müssen Sie MSDeploy.exe mit Aufrufen der [Web Deploy **ContentPath** Anbieter](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Die **ContentPath** -Anbieter unterstützt sowohl physischen Verzeichnispfade und Pfade in IIS-Website oder Anwendung, wodurch die ideale Wahl für die Synchronisierung von einer Datei zwischen einem Visual Studio-Projektordner und einer IIS-Webanwendung. Um die Datei bereitstellen, sieht der MSDeploy-Befehl folgendermaßen aus:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Um die Datei aus dem Zielstandort am Ende des Bereitstellungsprozesses zu entfernen, sieht der MSDeploy-Befehl folgendermaßen aus:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Um diese Befehle als Teil von einem Build & Deployment-Prozess zu automatisieren, müssen Sie sie in der benutzerdefinierten MSBuild-Projektdatei zu integrieren. Im nächste Verfahren wird beschrieben, wie zu diesem Zweck wird.

**Beim Bereitstellen und löschen eine App\_offline-Datei**

1. Öffnen Sie in Visual Studio 2010 die MSBuild-Projektdatei, die den Bereitstellungsprozess steuert. In der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) beispiellösung, dies ist die *Publish.proj* Datei.
2. Im Stammverzeichnis **Projekt** -Element, erstellen Sie ein neues **PropertyGroup** Element zum Speichern von Variablen für die *App\_offline* Bereitstellung:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Die **SourceRoot** Eigenschaft wird an anderer Stelle definiert, der *Publish.proj* Datei. Gibt den Speicherort des Stammordners für den Quellinhalt relativ zu den aktuellen Pfad&#x2014;in anderen Worten, relativ zum Speicherort von der *Publish.proj* Datei.
4. Die **ContentPath** Anbieter akzeptiert keine relativen Pfade, daher müssen Sie einen absoluten Pfad Ihrer Quelldatei zu erhalten, bevor Sie bereitgestellt werden können. Sie können die [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) Aufgabe dazu.
5. Fügen Sie einen neuen **Ziel** Element mit dem Namen **GetAppOfflineAbsolutePath**. Innerhalb dieses Ziel verwenden das **ConvertToAbsolutePath** ausführen einen absoluten Pfad zum Abrufen der *App\_offline-Template* Datei in Ihrem Projektordner.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Dieses Ziel verwendet, den relativen Pfad zu der *App\_offline-Template* Datei im Projektordner und speichert es in eine neue Eigenschaft als einen absoluten Dateipfad. Die **BeforeTargets** Attribut gibt an, dass es sich bei diesem Ziel vor ausgeführt werden sollen die **DeployAppOffline** Ziel, die Sie im nächsten Schritt erstellen.
7. Fügen Sie ein neues Ziel, die mit dem Namen **DeployAppOffline**. Rufen Sie den MSDeploy.exe-Befehl, das bereitgestellt wird, innerhalb dieses Ziel Ihrer *App\_offline* Datei auf den Zielserver für das Web.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In diesem Beispiel die **ContactManagerIisPath** Eigenschaft wird an anderer Stelle in der Projektdatei definiert. Dies ist einfach ein IIS-Anwendungspfad, in der Form *[IIS-Website-Name] / [Anwendungsname]*. Eine Bedingung in der Ziel-einschließlich ermöglicht Benutzern, wechseln die *App\_offline* Bereitstellung aktivieren oder deaktivieren, durch Ändern eines Eigenschaftswerts oder ein Befehlszeilen-Parameter.
9. Fügen Sie ein neues Ziel, die mit dem Namen **DeleteAppOffline**. In diesem Ziel, rufen Sie den MSDeploy.exe-Befehl, der entfernt Ihre *App\_offline* -Datei aus dem Zielwebserver.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Die letzte Aufgabe besteht darin, rufen Sie diese neue Ziele zu geeigneten Zeitpunkten während der Ausführung Ihrer Projektdatei. Dies ist auf verschiedene Weise möglich. Z. B. in der *Publish.proj* -Datei, die **FullPublishDependsOn** Eigenschaft gibt eine Liste der Ziele, die ausgeführt werden muss, in Reihenfolge bei der **FullPublish** Standard Ziel wird aufgerufen.
11. Ändern die MSBuild-Projektdatei zum Aufrufen der **DeployAppOffline** und **DeleteAppOffline** Ziele zu geeigneten Zeitpunkten in den Veröffentlichungsprozess.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Beim Ausführen der benutzerdefinierten MSBuild-Projektdatei, die *App\_offline* Datei wird unmittelbar nach einem erfolgreichen Build auf dem Server bereitgestellt werden. Es wird dann vom Server gelöscht werden, nachdem alle Bereitstellungsaufgaben abgeschlossen wurden.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Hinzufügen einer App\_Offline-Datei zu Bereitstellungspaketen

Je nachdem, wie Sie Ihre Bereitstellung konfigurieren, werden alle vorhandenen Inhalte am Ziel IIS Webanwendung&#x2014;wie die *App\_offline.htm* Datei&#x2014;möglicherweise automatisch gelöscht, wenn Sie ein Web deploy Paket an das Ziel. Um sicherzustellen, dass die *App\_offline.htm* Datei bleibt für die Dauer der Bereitstellung vorhanden, müssen Sie die Datei in das Webbereitstellungspaket selbst enthalten darüber hinaus die Datei direkt zu Beginn der Bereitstellung Der Bereitstellungsprozess.

- Wenn Sie die vorherigen Aufgaben in diesem Thema ausgeführt haben, Sie werde hinzugefügt haben, die *App\_offline.htm* Datei zu Ihrem Webprojekt für die Anwendung unter einem anderen Dateinamen (verwendet *App\_ Offline-template.htm*) und haben legen Sie die Buildaktion auf **keine**. Diese Änderungen sind erforderlich, um zu verhindern, dass die Datei aus beeinträchtigen, bei der Entwicklung und Debuggen. Daher müssen Sie zum Anpassen des Verpackungsprozesses, um sicherzustellen, dass die *App\_offline.htm* Datei in das Webbereitstellungspaket enthalten ist.

Die Web Publishing Pipeline (WPP) verwendet eine Elementliste mit dem Namen **FilesForPackagingFromProject** um eine Liste der Dateien zu erstellen, das in das Webbereitstellungspaket aufgenommen werden soll. Sie können den Inhalt Ihrer Web-Pakete anpassen, indem Sie Ihre eigenen Artikel zu dieser Liste hinzufügen. Zu diesem Zweck müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Erstellen Sie eine benutzerdefinierte Projektdatei namens *[Projektname].wpp.targets* im gleichen Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *. wpp.targets* Datei muss im gleichen Ordner wie die Projektdatei Ihre Web-Anwendung&#x2014;z. B. *ContactManager.Mvc.csproj*&#x2014;nicht im gleichen Ordner wie die benutzerdefinierten die Projektdateien, die Sie verwenden, um den Build & Deployment-Prozess zu steuern.
2. In der *. wpp.targets* Datei, erstellen Sie ein neues MSBuild-Ziel, die ausgeführt wird *vor* der **CopyAllFilesToSingleFolderForPackage** Ziel. Dies ist die WPP-Ziel, das eine Liste der Dinge, die im Paket enthalten erstellt.
3. Erstellen Sie in das neue Ziel, eine **ItemGroup** Element.
4. In der **ItemGroup** -Element, Hinzufügen einer **FilesForPackagingFromProject** Element aus, und geben Sie die *App\_offline.htm* Datei.

Die *. wpp.targets* Datei sollte diesem ähneln:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Dies sind die wichtigsten Punkte der Hinweis in diesem Beispiel:

- Die **BeforeTargets** fügt Attribut dieses Ziel in die WPP vom angeben, die sie unmittelbar vor ausgeführt werden, sollte die **CopyAllFilesToSingleFolderForPackage** Ziel.
- Die **FilesForPackagingFromProject** Element verwendet die **DestinationRelativePath** Metadatenwert umbenennen die Datei von *App\_offline template.htm* um *App\_offline.htm* zur Liste hinzugefügt wird.

Im nächste Verfahren erfahren Sie, wie dies hinzufügen *. wpp.targets* Datei in ein Webanwendungsprojekt.

**Hinzufügen einer. wpp.targets-Datei auf einem Webbereitstellungspaket**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Projektknoten der Web-Anwendung (z. B. **ContactManager.Mvc**), zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **XML-Datei** Vorlage.
4. In der **Namen** geben *[Projektname] ***.wpp.targets** (z. B. **ContactManager.Mvc.wpp.targets**), und klicken Sie dann auf **hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Wenn Sie auf den Stammknoten eines Projekts ein neues Element hinzufügen, wird die Datei im gleichen Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows Explorer öffnen.
5. Fügen Sie in der Datei die MSBuild-Markup, das zuvor beschriebene hinzu.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Speichern und schließen Sie die *[Projektname].wpp.targets* Datei.

Das nächste Mal Build und das Paket das Webanwendungsprojekt, WPP erkennt automatisch die *. wpp.targets* Datei. Die *App\_offline template.htm* Datei enthält das resultierende Webbereitstellungspaket als *App\_offline.htm*.

> [!NOTE]
> Wenn die Bereitstellung ein Fehler auftritt, die *App\_offline.htm* bleibt die Datei vorhanden und die Anwendung bleibt offline. Dies ist normalerweise das gewünschte Verhalten. Um Ihre Anwendung zu bringen wieder online, können Sie löschen die *App\_offline.htm* Datei von Ihrem Webserver. Sie können auch, wenn Sie alle Fehler zu beheben, und führen eine erfolgreiche Bereitstellung, die *App\_offline.htm* Datei wird entfernt.


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie eine Webanwendung für die Dauer einer Bereitstellung offline durch die Veröffentlichung einer *App\_offline.htm* Datei auf den Zielserver am Anfang des Bereitstellungsprozesses, und entfernen Sie sie auf der das Ende. Er erläutert auch, wie Sie enthalten eine *App\_offline.htm* -Datei in einem Webbereitstellungspaket.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den paketerstellungs- und Bereitstellungsprozess, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Bereitstellung von Paket](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), und [ Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Wenn Sie Ihre Webanwendungen direkt aus Visual Studio veröffentlichen, anstatt mit den benutzerdefinierten MSBuild-Projekt-Datei beschriebenen Ansatz in diesen Lernprogrammen, Sie müssen einen etwas anderen Ansatz zu verwenden, um Ihre Anwendung offline schalten, während der Veröffentlichung der Prozess. Weitere Informationen finden Sie unter [wie Sie Ihre Web-app während der Veröffentlichung](https://go.microsoft.com/?linkid=9805135) (Blogbeitrag).

> [!div class="step-by-step"]
> [Zurück](excluding-files-and-folders-from-deployment.md)
> [Weiter](running-windows-powershell-scripts-from-msbuild-project-files.md)
