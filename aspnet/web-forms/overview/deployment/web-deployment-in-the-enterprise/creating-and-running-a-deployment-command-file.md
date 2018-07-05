---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Erstellen und Ausführen einer Bereitstellungstyps Befehlsdatei | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie eine Befehlsdatei erstellen, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild) Projektdateien als einen Schritt für Schritt, erneut ausführen...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830301"
---
<a name="creating-and-running-a-deployment-command-file"></a>Erstellen und Ausführen einer Befehlsdatei für die Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellen, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild) Projektdateien als Schritt für Schritt, wiederholbaren Prozess ausgeführt wird.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager](the-contact-manager-solution.md) Lösung&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Verständnis des Prozesses erstellen](understanding-the-build-process.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="process-overview"></a>Prozessübersicht

In diesem Thema erfahren Sie, wie zum Erstellen und Ausführen einer Befehlsdatei, die diese Projektdateien verwendet wird, um eine wiederholbare Bereitstellung für Ihre zielumgebung durchzuführen. Im Wesentlichen die Befehlsdatei muss einfach nur einen MSBuild-Befehl enthält, die:

- Informiert MSBuild, führen Sie die Umgebung agnostisch *Publish.proj* Datei.
- Teilt die *Publish.proj* Datei, welche Datei enthält die umgebungsspezifischen projekteinstellungen und wo Sie sich befindet.

## <a name="create-an-msbuild-command"></a>Erstellen Sie ein MSBuild-Befehl

Siehe [Verständnis des Prozesses erstellen](understanding-the-build-process.md), umgebungsspezifische Projektdatei&#x2014;z. B. *Env-Dev.proj*&#x2014;dient in der Umgebung agnostisch importiert werden sollen *Publish.proj* Datei zum Zeitpunkt der Erstellung. Zusammen bieten diese beiden Dateien einen vollständigen Satz von Anweisungen, die MSBuild anweisen, wie zum Erstellen und Bereitstellen Ihrer Lösung.

Die *Publish.proj* Datei verwendet ein **importieren** Element, um die umgebungsspezifischen-Projektdatei importieren.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Wenn Sie MSBuild.exe zum Erstellen und Bereitstellen von Contact Manager-Lösung verwenden, müssen Sie:

- Führen Sie MSBuild.exe auf die *Publish.proj* Datei.
- Geben Sie den Speicherort der Projektdatei umgebungsspezifische durch Angabe der Parameter für die Befehlszeilen mit dem Namen **TargetEnvPropsFile**.

Zu diesem Zweck sieht der MSBuild-Befehl folgendermaßen aus:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Hier ist es eine einfache Schritt, in einer Bereitstellung für wiederholbare, Schritt für Schritt zu verschieben. Müssen Sie lediglich Ihre MSBuild-Befehl eine CMD-Datei hinzu. In der Projektmappe Contact Manager-Ordner "Publish" umfasst eine Datei namens *veröffentlichen-Dev.cmd* , die dies ist genau.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> Die **/fl** -Schalter weist MSBuild zum Erstellen einer Protokolldatei mit dem Namen *msbuild.log* in das Arbeitsverzeichnis, in denen MSBuild.exe aufgerufen wurde.


Zum Bereitstellen oder die Kontakt-Manager-Lösung erneut bereitzustellen, müssen Sie lediglich ausgeführt wird die *veröffentlichen-Dev.cmd* Datei. Wenn Sie die Datei ausführen, wird MSBuild:

- Alle Projekte in der Projektmappe zu erstellen.
- Generieren Sie bereitstellbare Webpakete für Webanwendungsprojekte.
- Generieren Sie DBSCHEMA und DeployManifest-Dateien für die Datenbankprojekte.
- Stellen Sie die Webpakete an den Webserver bereit.
- Bereitstellen der Datenbank mit dem Datenbankserver her.

## <a name="run-the-deployment"></a>Führen Sie die Bereitstellung

Wenn Sie eine Befehlsdatei für Ihre zielumgebung erstellt haben, sollten Sie die gesamte Bereitstellung abgeschlossen, indem Sie einfach die Datei ausführen können.

**Zum Bereitstellen der Contact Manager-Lösung an Ihre testumgebung**

1. Klicken Sie auf der Entwicklerarbeitsstation Windows Explorer öffnen, und navigieren Sie dann auf den Speicherort der der *veröffentlichen-Dev.cmd* Datei.
2. Doppelklicken Sie auf die Datei aus, um es auszuführen.
3. Wenn ein **Datei öffnen-Sicherheitswarnung** klicken Sie im angezeigten Dialogfeld **ausführen**.
4. Wenn Ihre Konfigurationseinstellungen und die Testserver eingerichtet sind, ordnungsgemäß im Eingabeaufforderungsfenster wird angezeigt ein **Buildvorgang war erfolgreich.** Meldung, wenn MSBuild die Projektdateien Verarbeitung abgeschlossen hat.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Ist dies beim ersten Sie die Lösung für diese Umgebung bereitgestellt haben, müssen Sie das Computerkonto Test Web Server, Hinzufügen der **Db\_Datawriter** und **Db\_Datareader**Rollen auf die **ContactManager** Datenbank. Dieses Verfahren wird beschrieben, [Konfigurieren eines Datenbankservers für die Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Sie müssen nur diese Berechtigungen zuweisen, wenn Sie die Datenbank erstellen. In der Standardeinstellung des Buildprozesses werden nicht neu erstellt die Datenbank bei jeder Bereitstellung&#x2014;stattdessen die vorhandene Datenbank auf dem neuesten Schema verglichen und nur die erforderlichen Änderungen vornehmen. Daher müssen Sie nur diesen Datenbankrollen beim ersten ordnen Sie die Lösung bereitstellen.
6. Öffnen Sie Internet Explorer, und navigieren Sie zu der URL der Contact Manager-Anwendung (z. B. `http://testweb1:85/ContactManager/`).
7. Stellen Sie sicher, dass die Anwendung ordnungsgemäß funktioniert, und Sie können Kontakte hinzufügen.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Schlussbemerkung

Erstellen eine Befehlsdatei, die die MSBuild-Anweisungen enthält, bietet Ihnen eine schnelle und einfache Möglichkeit, erstellen und Bereitstellen von Projektmappen mit mehreren Projekten für ein bestimmtes Ziel-Umgebung. Wenn Sie wiederholte Bereitstellen Ihrer Lösung in mehreren zielumgebungen müssen, können Sie mehrere Befehlsdateien erstellen. Klicken Sie in jeder Befehlsdatei MSBuild-Befehl wird die gleiche universal-Projekt-Datei erstellen, aber es wird eine andere Umgebung spezifischen Projektdatei angeben. Beispielsweise kann eine Befehlsdatei für Entwickler veröffentlichen oder testumgebung dieses MSBuild-Befehl enthalten:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Eine Befehlsdatei in einer Stagingumgebung zu veröffentlichen, könnte dieses MSBuild-Befehl enthalten:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifischen Projektdateien für Ihre eigenen serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Sie können den Buildprozess für jede Umgebung auch anpassen, durch Überschreiben von Eigenschaften oder Festlegen von verschiedenen anderen Schaltern in Ihrem MSBuild-Befehl. Weitere Informationen finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Zurück](deploying-database-projects.md)
> [Weiter](manually-installing-web-packages.md)
