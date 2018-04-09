---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Erstellen von Webanwendungen Offline mit Web Deploy | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie eine Webanwendung für die Dauer einer automatisierten Bereitstellung mithilfe der Internetinformationsdienste (Internet Information Services, IIS) Web Depl offline geschaltet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 511201dc5646340b21023430fa319417f2b53ae2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Erstellen von Webanwendungen Offline mit Web Deploy
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie eine Webanwendung für die Dauer einer automatisierten Bereitstellung über das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) offline erstellt werden. Benutzer, die an die Web-Anwendung durchsuchen werden umgeleitet, um eine *App\_offline.htm* Datei, bis die Bereitstellung abgeschlossen ist.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

In einer Vielzahl von Szenarien sollten Sie eine Webanwendung offline schalten, während Sie zugehöriger Komponenten, wie Datenbanken oder Webdienste ändern. In der Regel in IIS und ASP.NET können Sie zu diesem Zweck platzieren eine Datei namens *App\_offline.htm* im Stammordner der IIS-Website oder Anwendung. Die *App\_offline.htm* Datei ist eine standard-HTML-Datei und enthält in der Regel eine einfache Nachricht mit dem Hinweis an, dass der Standort aufgrund von Wartungsarbeiten vorübergehend nicht verfügbar ist. Während der *App\_offline.htm* Datei im Stammordner der Website vorhanden ist, wird IIS automatisch weitergeleitet wird alle Anforderungen, die Datei. Wenn Sie Updates vorgenommen haben, entfernen Sie die *App\_offline.htm* -Datei und die Website wird fortgesetzt, wie üblich verarbeitet Clientanforderungen.

Wenn Sie Web Deploy zum Ausführen von automatisierter oder einstufiger Bereitstellungen in einer zielumgebung verwenden, möchten Sie möglicherweise integrieren, hinzufügen und Entfernen von der *App\_offline.htm* -Datei in das Verfahren zum Bereitstellen. Zu diesem Zweck müssen Sie diese allgemeinen Aufgaben ausführen:

- In der Projektdatei von Microsoft Build Engine (MSBuild) kopiert werden sollen, verwenden, um den Bereitstellungsprozess zu steuern, erstellen ein MSBuild-Ziel, die eine *App\_offline.htm* Datei auf den Zielserver vor alle Bereitstellungsaufgaben beginnen.
- Fügen Sie einen anderen MSBuild-Ziel, das entfernt die *App\_offline.htm* Datei auf dem Zielserver aus, wenn alle Bereitstellungsaufgaben abgeschlossen sind.
- In das Webanwendungsprojekt erstellen eine *. wpp.targets* -Datei, die garantiert, dass ein *App\_offline.htm* Datei wird in das Bereitstellungspaket hinzugefügt, wenn Sie Web Deploy aufgerufen wird.

In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits eine Projektmappe erstellt haben, die über mindestens ein Webanwendungsprojekt enthält und eine benutzerdefinierte Projektdatei verwenden, um den Bereitstellungsprozess zu steuern, wie in beschrieben [Webbereitstellung in der Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternativ können Sie die [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) Beispielprojektmappe in den Beispielen in diesem Thema folgen.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Hinzufügen der App\_Offline-Datei, um ein Webanwendungsprojekt

Die erste Aufgabe, die Sie durchführen müssen hinzufügen wird ein *App\_offline* Datei zum Webanwendungsprojekt-Anwendung:

- Um zu verhindern, dass die Datei des Entwicklungsprozesses stören (nicht soll Ihre Anwendung dauerhaft offline sein), Sie sollten aufrufen, es etwas anders als *App\_offline.htm*. Angenommen, nennen Sie die Datei *App\_offline template.htm*.
- Um zu verhindern, dass die Datei als bereitzustellende-ist, legen Sie die Buildaktion auf **keine**.

**So fügen Sie eine App hinzu\_offline-Datei, um ein Webanwendungsprojekt**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Webanwendungsprojekts, zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **HTML-Seite**.
4. In der **Namen** geben **App\_offline template.htm**, und klicken Sie dann auf **hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Fügen Sie einige einfache HTML aus, um Benutzer zu informieren, dass die Anwendung nicht verfügbar ist, und speichern Sie die Datei. Verwenden Sie keine serverseitigen Tags (z. B. alle Tags, die mit dem Präfix "Asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. In der **Projektmappen-Explorer** rechten Maustaste auf die neue Datei, und klicken Sie dann auf **Eigenschaften**.
7. In der **Eigenschaften** Fenster, in der **Buildvorgang** Zeile **keine**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Bereitstellen und löschen eine App\_Offlinedatei

Der nächste Schritt ist so ändern Sie Ihre Bereitstellung Logik kopieren Sie die Datei auf den Zielserver zu Beginn des Bereitstellungsprozesses und am Ende entfernt werden.

> [!NOTE]
> Im nächste Verfahren wird davon ausgegangen, dass Sie eine benutzerdefinierte MSBuild-Projektdatei zum Steuern des Verfahren zum Bereitstellen, wie in beschrieben verwenden, [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Wenn Sie direkt von Visual Studio bereitstellen, müssen Sie einen anderen Ansatz zu verwenden. Sayed Ibrahim Hashimi beschreibt einen solchen Ansatz in [zum Ausführen Ihrer App Offline während Webveröffentlichung](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Bereitstellen einer *App\_offline* Datei an eine Ziel-IIS-Website müssen Sie zum Aufrufen MSDeploy.exe mithilfe der [Web Deploy **ContentPath** Anbieter](https://technet.microsoft.com/library/dd569034(WS.10).aspx). Die **ContentPath** Anbieter unterstützt physischen Verzeichnispfade und Pfade in IIS-Website oder Anwendung, wodurch die ideale Option zum Synchronisieren einer Datei zwischen einem Visual Studio-Projektordner und eine IIS-Webanwendung. Um die Datei bereitstellen, sieht der MSDeploy-Befehl folgendermaßen aus:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Um die Datei am Zielstandort am Ende des Bereitstellungsprozesses zu entfernen, sieht der MSDeploy-Befehl folgendermaßen aus:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Um diese Befehle als Teil eines Prozesses Build- und Bereitstellungsprozess automatisieren möchten, müssen Sie diese in Ihre benutzerdefinierte MSBuild-Projektdatei integriert. Das nächste Verfahren beschreibt, wie dies.

**Zum Bereitstellen und löschen eine App\_Offlinedatei**

1. Öffnen Sie in Visual Studio 2010 die MSBuild-Projektdatei, die steuert, das Verfahren zum Bereitstellen. In der [Vorgesetzten Kontakts](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) beispiellösung, dies ist die *Publish.proj* Datei.
2. Im Stammverzeichnis **Projekt** Element, erstellen Sie ein neues **PropertyGroup** Element zum Speichern von Variablen für die *App\_offline* Bereitstellung:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. Die **SourceRoot** Eigenschaft an anderer Stelle definiert ist, der *Publish.proj* Datei. Er gibt den Speicherort des Stammordners für den Quellinhalt relativ zum Pfad aktuellen an&#x2014;also relativ zum Speicherort von der *Publish.proj* Datei.
4. Die **ContentPath** Anbieter akzeptiert keine relative Dateipfade, daher müssen Sie einen absoluten Pfad der Quelldatei abrufen, bevor Sie bereitgestellt werden können. Sie können die [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) Aufgabe dazu.
5. Fügen Sie einen neuen **Ziel** Element mit dem Namen **GetAppOfflineAbsolutePath**. In diesem Ziel verwenden die **ConvertToAbsolutePath** ausführen einen absoluten Pfad zum Abrufen der *App\_offline-Vorlage* Datei im Projektordner.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Dieses Ziel akzeptiert den relativen Pfad zu der *App\_offline Vorlage* Datei im Projektordner und speichert ihn in eine neue Eigenschaft als einen absoluten Dateipfad. Die **des BeforeTargets** Attribut gibt an, dass Sie dieses Ziel ausgeführt wird, bevor die **DeployAppOffline** Ziels, das Sie im nächsten Schritt erstellen.
7. Fügen Sie ein neues Ziel mit dem Namen **DeployAppOffline**. In diesem Ziel den MSDeploy.exe-Befehl, mit denen bereitgestellt aufrufen Ihrer *App\_offline* Datei mit dem Ziel-Webserver.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. In diesem Beispiel wird die **ContactManagerIisPath** Eigenschaft in der Projektdatei an anderer Stelle definiert ist. Dies ist lediglich eine IIS-Anwendungspfad, in der Form *[IIS Websitename] / [Anwendungsname]*. Eine Bedingung in der Ziel-einschließlich ermöglicht es Benutzern, wechseln die *App\_offline* Bereitstellung aktiviert oder deaktiviert Ändern eines Eigenschaftswerts oder einen Befehlszeilenparameter.
9. Fügen Sie ein neues Ziel mit dem Namen **DeleteAppOffline**. In diesem Ziel "Befehl" MSDeploy.exe ", die entfernt werden" aufrufen Ihrer *App\_offline* -Datei aus dem Ziel-Webserver.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Die letzte Aufgabe ist das Aufrufen von diesen neuen Zielen zu geeigneten Zeitpunkten während der Ausführung der Projektdatei. Dies ist auf verschiedene Weise möglich. Z. B. in der *Publish.proj* Datei, die **FullPublishDependsOn** Eigenschaft gibt eine Liste der Ziele, die ausgeführt werden muss in sortieren, wenn die **FullPublish** Standard Ziel aufgerufen wird.
11. Ändern die MSBuild-Projektdatei zum Aufrufen der **DeployAppOffline** und **DeleteAppOffline** Ziele am entsprechenden Punkt in den Veröffentlichungsprozess ein.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Wenn Sie Ihre benutzerdefinierte MSBuild-Projektdatei Ausführen der *App\_offline* Datei wird unmittelbar nach der erfolgreichen Erstellung auf dem Server bereitgestellt werden. Es wird dann vom Server gelöscht werden, nachdem Sie die Bereitstellungsaufgaben abgeschlossen haben.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Hinzufügen der App\_Offlinedatei Bereitstellungspaketen

Abhängig von der Konfiguration Ihrer Bereitstellung vorhandene Inhalte am Ziel IIS-Webanwendung&#x2014;wie die *App\_offline.htm* Datei&#x2014;möglicherweise automatisch gelöscht, wenn Sie ein Web deploy Paket an das Ziel. Um sicherzustellen, dass die *App\_offline.htm* Datei wird so lange für die Dauer der Bereitstellung, müssen Sie die Datei in das Webbereitstellungspaket selbst enthalten darüber hinaus die Datei direkt zu Beginn der Bereitstellung Der Bereitstellungsprozess.

- Wenn Sie die vorherigen Aufgaben in diesem Thema befolgt haben, Sie müssen hinzugefügt haben, die *App\_offline.htm* Datei zum Webanwendungsprojekt-Anwendung unter einem anderen Dateinamen (wir verwendet *App\_ Offline-template.htm*) und haben legen Sie die Buildaktion auf **keine**. Diese Änderungen sind erforderlich, um zu verhindern, dass die Datei aus stören Entwicklung und Debuggen. Daher müssen Sie zum Anpassen des Verpackungsprozesses, um sicherzustellen, dass die *App\_offline.htm* Datei in das Webbereitstellungspaket enthalten ist.

Die Publishing Web Pipeline (WPP) verwendet eine Liste der mit dem Namen **FilesForPackagingFromProject** eine Liste der Dateien zu erstellen, die in das Webbereitstellungspaket aufgenommen werden. Sie können den Inhalt von Ihrer Webpaketen anpassen, indem Sie Ihre eigenen Artikel zu dieser Liste hinzufügen. Zu diesem Zweck müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Erstellen eine benutzerdefinierten Projektdatei mit dem Namen *[Projektname].wpp.targets* im gleichen Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *. wpp.targets* Datei muss sich im gleichen Ordner wie die Anwendung Webprojektdatei wechseln&#x2014;z. B. *ContactManager.Mvc.csproj*&#x2014;anstatt im gleichen Ordner wie die benutzerdefinierten Verwenden Sie zum Steuern der Build- und Bereitstellungsprozess Projektdateien verarbeiten.
2. In der *. wpp.targets* Datei, erstellen Sie ein neues MSBuild-Ziel, die ausgeführt wird *vor* der **CopyAllFilesToSingleFolderForPackage** Ziel. Dies ist das WPP-Ziel, das erstellt eine Liste der Dinge, die in das Paket eingeschlossen werden sollen.
3. Erstellen Sie in das neue Ziel ein **ItemGroup** Element.
4. In der **ItemGroup** Element, Hinzufügen einer **FilesForPackagingFromProject** Element, und geben Sie die *App\_offline.htm* Datei.

Die *. wpp.targets* Datei sollte diesem ähneln:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Dies sind die wichtigsten Punkte beachten, in diesem Beispiel:

- Die **des BeforeTargets** Attribut fügt dieses Ziel in der WPP, indem Sie angeben, die unmittelbar vor der ausgeführt werden soll die **CopyAllFilesToSingleFolderForPackage** Ziel.
- Die **FilesForPackagingFromProject** Element verwendet die **DestinationRelativePath** Metadatenwert umbenennen die Datei aus *App\_offline template.htm* um *App\_offline.htm* zur Liste hinzugefügt wird.

Das nächste Verfahren veranschaulicht das Hinzufügen dieser *. wpp.targets* Datei um ein Webanwendungsprojekt.

**Hinzufügen einer. wpp.targets-Datei in ein Bereitstellungspaket für Web**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Projektknoten der Web-Anwendung (z. B. **ContactManager.Mvc**), zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **XML-Datei** Vorlage.
4. In der **Namen** geben *[Projektname] ***.wpp.targets** (z. B. **ContactManager.Mvc.wpp.targets**), und klicken Sie dann auf **hinzufügen**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Wenn Sie den Stammknoten des Projekts ein neues Element hinzufügen, wird die Datei im gleichen Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows Explorer öffnen.
5. Fügen Sie das MSBuild-Markup, das zuvor beschriebene, in der Datei.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Speichern und schließen Sie die *[Projektname].wpp.targets* Datei.

Das nächste Mal Sie Builds und des Pakets, das Webanwendungsprojekt, WPP erkennt automatisch die *. wpp.targets* Datei. Die *App\_offline template.htm* Datei enthalten sein wird, in der resultierenden Webbereitstellungspaket als *App\_offline.htm*.

> [!NOTE]
> Wenn Ihre Bereitstellung ein Fehler auftritt, die *App\_offline.htm* Datei bleibt an Ort und die Anwendung bleibt offline. Dies ist normalerweise das gewünschte Verhalten. Schalten Sie die Anwendung wieder online können Sie löschen die *App\_offline.htm* Datei von Ihrem Webserver. Sie können auch, wenn Sie Fehler korrigieren, und führen Sie eine erfolgreiche Bereitstellung der *App\_offline.htm* wird entfernt.


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie eine Webanwendung für die Dauer einer Bereitstellung offline durch die Veröffentlichung erstellt werden ein *App\_offline.htm* Datei auf den Zielserver zu Beginn des Bereitstellungsprozesses und entfernen Sie es an den Ende. Es fallen auch zum Einschließen einer *App\_offline.htm* Datei in ein Webbereitstellungspaket.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen über das Packen und Bereitstellungsprozess, finden Sie unter [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Bereitstellung von Paketen](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), und [ Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Wenn Sie Ihre Webanwendungen direkt aus Visual Studio veröffentlichen, anstatt mit benutzerdefinierte MSBuild-Projekt Datei beschriebene Ansatz in diesen Lernprogrammen, müssen Sie einen etwas anderen Ansatz verwenden, um die Anwendung offline, während der Veröffentlichung zu erstellen Prozess. Weitere Informationen finden Sie unter [wie Ihre Web-app während der Veröffentlichung werden](https://go.microsoft.com/?linkid=9805135) (Blogbeitrag).

> [!div class="step-by-step"]
> [Zurück](excluding-files-and-folders-from-deployment.md)
> [Weiter](running-windows-powershell-scripts-from-msbuild-project-files.md)
