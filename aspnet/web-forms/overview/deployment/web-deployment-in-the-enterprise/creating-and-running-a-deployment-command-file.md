---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Erstellen und Ausführen einer Bereitstellung Befehlsdatei | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellt, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild)-Projektdateien in einem einzelnen Schritt erneut ausführen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>Erstellen und Ausführen einer Befehlsdatei Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie eine Befehlsdatei erstellt, mit denen Sie eine Bereitstellung mithilfe von Microsoft Build Engine (MSBuild)-Projektdateien einstufiger, wiederholbare ausgeführt wird.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Vorgesetzten Kontakts](the-contact-manager-solution.md) Lösung & #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [Verständnis des Build-Prozesses](understanding-the-build-process.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="process-overview"></a>Übersicht über das

In diesem Thema erfahren Sie, wie zum Erstellen und Ausführen einer Befehlsdatei, die diese Projektdateien verwendet wird, um eine wiederholbare Bereitstellungen in Ihrer zielumgebung auszuführen. Im Wesentlichen die Befehlsdatei einfach muss einen MSBuild-Befehl enthalten, die:

- Weist MSBuild an, führen Sie die Umgebung Unabhängigkeit *Publish.proj* Datei.
- Teilt die *Publish.proj* Datei, die sich die Datei enthält den umgebungsspezifische projekteinstellungen und wo sie suchen.

## <a name="create-an-msbuild-command"></a>Erstellen Sie einen MSBuild-Befehl

Wie in beschrieben [Verständnis des Build-Prozesses](understanding-the-build-process.md), die Projektdatei umgebungsspezifische & #x 2014; z. B. *Env Dev.proj*& #x 2014; dient für den Import in die Unabhängigkeit von der Umgebung *Publish.proj* Datei während des Buildvorgangs. In Kombination bieten zwei dieser Dateien einen vollständigen Satz von Anweisungen, die MSBuild Bereitstellen von Informationen zum Erstellen und Bereitstellen der Projektmappe.

Die *Publish.proj* Datei verwendet eine **importieren** Element, um die Umgebung spezifischen-Projektdatei importieren.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Bei Verwendung von MSBuild.exe zum Erstellen und Bereitstellen der Projektmappe Contact Manager müssen Sie daher auf:

- MSBuild.exe ausführen, auf die *Publish.proj* Datei.
- Geben Sie den Speicherort der Projektdatei umgebungsspezifische durch Angabe eines Befehlszeilenparameters namens **TargetEnvPropsFile**.

Zu diesem Zweck sieht die MSBuild-Befehl folgendermaßen aus:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Hier ist es ein einfacher Schritt zu einer Bereitstellung wiederholbare, Schritt für Schritt zu verschieben. Alles, was Sie tun müssen MSBuild-Befehl eine CMD-Datei hinzugefügt werden. In der Projektmappe Contact Manager Veröffentlichungsordner enthält eine Datei namens *veröffentlichen Dev.cmd* genau dies ausführt.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> Die **/fl** -Schalter wird MSBuild zum Erstellen einer Protokolldatei mit dem Namen *msbuild.log* in das Arbeitsverzeichnis an, in dem MSBuild.exe aufgerufen wurde.


Um bereitzustellen, oder stellen Sie die Kontakt-Manager-Lösung, müssen Sie lediglich führen die *veröffentlichen Dev.cmd* Datei. Wenn Sie die Datei ausführen, sehen MSBuild:

- Alle Projekte in der Projektmappe zu erstellen.
- Bereitstellbare Webpaketen für die Anwendung Webprojekte zu generieren.
- Generieren Sie DBSCHEMA und DeployManifest-Dateien für die Datenbankprojekte.
- Bereitstellen Sie die Webpakete an den Webserver.
- Bereitstellen Sie die Datenbank auf dem Datenbankserver.

## <a name="run-the-deployment"></a>Führen Sie die Bereitstellung

Wenn Sie eine Befehlsdatei für Ihre zielumgebung erstellt haben, sollten Sie einfach mit der Datei die gesamte Bereitstellung abschließen können.

**Die Kontakt-Manager-Lösung an Ihre testumgebung bereitstellen**

1. Auf der Arbeitsstation Developer öffnen Sie Windows Explorer, und navigieren Sie zum Speicherort von der *veröffentlichen Dev.cmd* Datei.
2. Doppelklicken Sie auf die Datei, um es auszuführen.
3. Wenn ein **Datei öffnen-Sicherheitswarnung** Dialogfeld angezeigt wird, klicken Sie auf **ausführen**.
4. Wenn Ihre Konfigurationseinstellungen und Testserver eingerichtet sind, ordnungsgemäß im Eingabeaufforderungsfenster zeigt eine **der Buildvorgang war erfolgreich** Meldung, wenn MSBuild die Verarbeitung der Projektdateien abgeschlossen hat.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Wenn dies der erste Mal ist, haben Sie die Lösung für diese Umgebung bereitgestellt, müssen Sie die Test-Web--Servercomputer zum Hinzufügen der **Db\_Datawriter** und **Db\_Datareader**Rollen auf die **ContactManager** Datenbank. Hierin wird beschrieben, [Konfigurieren eines Datenbankservers für Webveröffentlichung bereitstellen](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Sie müssen nur dieser Berechtigungen zuweisen, wenn Sie die Datenbank erstellen. Standardmäßig die Datenbank auf jeder Bereitstellung & #x 2014, während des Erstellungsprozesses wird nicht neu erstellt; stattdessen wird die vorhandene Datenbank auf das neueste Schema vergleichen und nur die erforderlichen Änderungen vornehmen. Daher müssen Sie nur diesen Datenbankrollen erstmalig ordnen Sie die Projektmappe bereitstellen.
6. Öffnen Sie Internet Explorer, und navigieren Sie zu der URL der Kontakt-Manager-Anwendung (z. B. `http://testweb1:85/ContactManager/`).
7. Stellen Sie sicher, dass die Anwendung erwartungsgemäß funktioniert, und Sie die Kontakte hinzufügen können.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Schlussbemerkung

Erstellen eine Befehlsdatei mit der MSBuild-Anweisungen, bietet Ihnen eine schnelle und einfache Möglichkeit der Erstellung und Bereitstellung einer Lösung mit mehreren Projekten in einer bestimmten zielumgebung. Wenn Sie Ihre Lösung in mehreren zielumgebungen bereitstellen müssen, können Sie mehrere Befehlsdateien erstellen. In jeder Datei die MSBuild-Befehl wird die gleiche universal-Projektdatei erstellt, aber es wird eine andere umgebungsspezifische Projektdatei angeben. Beispielsweise könnte eine Befehlsdatei ein Entwickler veröffentlichen oder testumgebung dieses MSBuild-Befehl enthalten:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Eine Befehlsdatei zum Veröffentlichen in einer Stagingumgebung könnte diese MSBuild-Befehl enthalten:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifische-Projektdateien für eigene Server-Umgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Sie können den Buildprozess für jede Umgebung auch anpassen, indem Überschreiben von Eigenschaften oder das Festlegen von verschiedenen anderen Schaltern in MSBuild-Befehl. Weitere Informationen finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).

>[!div class="step-by-step"]
[Zurück](deploying-database-projects.md)
[Weiter](manually-installing-web-packages.md)
