---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: "Ausschließen von Dateien und Ordner von der Bereitstellung | Microsoft Docs"
author: jrjlee
description: "In diesem Thema wird beschrieben, wie Sie können Dateien und Ordner ausschließen aus einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 80810415bac473a58f60110fb9d08772e0627bd5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="excluding-files-and-folders-from-deployment"></a>Ausschließen von Dateien und Ordner von der Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie können Dateien und Ordner ausschließen aus einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="overview"></a>Übersicht

Wenn Sie ein Webprojekt für die Anwendung in Visual Studio 2010 erstellen, Sie können die Publishing Web Pipeline (WPP) dieses Buildprozesses zu erweitern, indem Sie Ihre kompilierte-Anwendung in ein Webpaket zur Bereitstellung geeignete packen. Sie können die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) dieses Webpaket bereitgestellt auf einem remote-IIS-Webserver verwenden, oder importieren das Paket manuell über den IIS-Manager. Diese Verpackungsprozesses finden [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Wie Sie Einfluss haben, was in Ihrer Website enthalten ruft Paket? Die Einstellungen für Projektdateien in Visual Studio über die zugrunde liegenden Projektdatei bieten ausreichende Kontrolle für eine Vielzahl von Szenarien. In einigen Fällen möchten Sie jedoch den Inhalt des Webpakets auf bestimmte zielumgebungen anzupassen. Sie möchten z. B. einen Ordner für Protokolldateien einschließen, wenn Sie Ihre Anwendung in einer testumgebung bereitstellen, aber schließen Sie den Ordner aus, wenn Sie die Anwendung in einer Staging- oder Produktionsserver Umgebung bereitstellen. In diesem Thema erfahren Sie, wie Sie dies tun.

## <a name="what-gets-included-by-default"></a>Was ruft standardmäßig enthalten?

Wenn Sie die Projekteigenschaften des Web-Anwendung in Visual Studio Konfigurieren der **bereitzustellenden Elemente** auf in der Liste der **Web packen/veröffentlichen** Seite können Sie angeben, was in Ihrer webbereitstellung enthalten sein sollen das Paket. Standardmäßig ist dies festgelegt auf **nur die Ausführung dieser Anwendung erforderliche Dateien**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Bei Auswahl **nur die Ausführung dieser Anwendung erforderliche Dateien**, WPP versucht zu bestimmen, welche Dateien für das Webpaket hinzugefügt werden soll. Dies umfasst Folgendes:

- Alle Builds Ausgaben für das Projekt.
- Alle Dateien mit dem Buildvorgang markiert **Content**.

> [!NOTE]
> In dieser Datei ist die Logik, die bestimmt, welche Dateien sind enthalten:   
> *%ProgramFiles%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Ausschließen bestimmter Dateien und Ordner

In einigen Fällen sollten eine präzisere Kontrolle Sie über die Dateien und Ordner bereitgestellt werden. Wenn Sie wissen, welche mysqlsink auszuschließenden Dateien Zeit, und der Ausschluss bezieht sich auf alle zielumgebungen, können Sie einfach die **Buildvorgang** jeder Datei **keine**.

**Ausschließen bestimmte Dateien von der Bereitstellung**

1. In der **Projektmappen-Explorer** rechten Maustaste auf die Datei, und klicken Sie dann auf **Eigenschaften**.
2. In der **Eigenschaften** Fenster, in der **Buildvorgang** Zeile **keine**.

Dieser Ansatz ist jedoch nicht immer praktisch. Beispielsweise empfiehlt es sich um die Dateien zu verändern und Ordner werden gemäß Ihrer zielumgebung und außerhalb von Visual Studio enthalten. Angenommen, in der Beispielprojektmappe Contact Manager sehen Sie sich den Inhalt des Projekts ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Der interne Ordner enthält einige SQL­Skripts, mit der Entwickler erstellen, löschen und lokale Datenbanken zu Entwicklungszwecken aufzufüllen. "Nothing" in diesem Ordner sollte in einer Staging- oder Produktionsserver Umgebung bereitgestellt werden.
- Der Ordner "Skripts" enthält mehrere JavaScript-Dateien. Viele dieser Dateien sind enthalten ausschließlich zur Unterstützung einer Debuggen oder IntelliSense in Visual Studio zur Verfügung stellen. Einige dieser Dateien sollten nicht auf Staging-oder produktionsumgebung bereitgestellt werden. Allerdings empfiehlt es sich um die Bereitstellung in einer testumgebung Developer, um die Problembehandlung zu vereinfachen.

Obwohl Sie die Projektdateien zum Ausschließen bestimmter Dateien und Ordner bearbeiten können, gibt es einen einfacheren Weg. Die WPP umfasst einen Mechanismus, um Dateien und Ordner ausschließen, indem Sie mit dem Namen Elementlisten erstellen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles**. Sie können diesen Mechanismus erweitern, indem Sie diese Listen eigene Elemente hinzugefügt. Zu diesem Zweck müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Erstellen eine benutzerdefinierten Projektdatei mit dem Namen *[Projektname].wpp.targets* im gleichen Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *. wpp.targets* Datei muss im gleichen Ordner wie die Projektdatei der Web-Anwendung & #x 2014; wechseln z. B. *ContactManager.Mvc.csproj*& #x 2014; statt im selben Ordner wie ein anderer Benutzerdefinierte Projektdateien verwenden Sie, um die Steuerung des Builds und Bereitstellungsprozess.
2. In der *. wpp.targets* Hinzufügen einer **ItemGroup** Element.
3. In der **ItemGroup** Element hinzufügen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles** Elemente, die bestimmte Dateien und Ordner nach Bedarf ausschließen.

Dies ist die grundlegende Struktur dieses *. wpp.targets* Datei:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Beachten Sie, dass jedes Element ein Element-Metadaten-Element mit dem Namen schließt **FromTarget**. Dies ist ein optionaler Wert, der während des Erstellungsprozesses nicht beeinträchtigt wird. es einfach, um anzugeben, warum bestimmte Dateien oder Ordner weggelassen wurden dient, wenn jemand die Buildprotokolle überprüft.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Ausschließen von Dateien und Ordner von einem Webpaket

Das nächste Verfahren veranschaulicht das Hinzufügen einer *. wpp.targets* Datei ein Webanwendungsprojekt und wie die Datei zum Ausschließen bestimmter Dateien und Ordner aus dem Webpaket beim Erstellen des Projekts verwenden.

**Zum Ausschließen von Dateien und Ordnern über ein Webbereitstellungspaket**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Projektknoten der Web-Anwendung (z. B. **ContactManager.Mvc**), zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **XML-Datei** Vorlage.
4. In der **Namen** geben *[Projektname]***. wpp.targets** (z. B. **ContactManager.Mvc.wpp.targets**), und klicken Sie dann auf  **Hinzufügen**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Wenn Sie den Stammknoten des Projekts ein neues Element hinzufügen, wird die Datei im gleichen Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows Explorer öffnen.
5. Fügen Sie in der Datei eine **Projekt** Element und ein **ItemGroup** Element:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Wenn Sie aus dem Webpaket ausschließen möchten, fügen eine **ExcludeFromPackageFolders** Element der **ItemGroup** Element:

    1. In der **Include** -Attribut angegeben wird, geben Sie eine durch Semikolons getrennte Liste der Ordner, die Sie ausschließen möchten.
    2. In der **FromTarget** Metadatenelement, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Ordner, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Wenn Sie Dateien aus dem Webpaket ausschließen möchten, fügen Sie ein **ExcludeFromPackageFiles** Element an der **ItemGroup** Element:

    1. In der **Include** -Attribut angegeben wird, geben Sie eine durch Semikolons getrennte Liste der Dateien, die Sie ausschließen möchten.
    2. In der **FromTarget** Metadatenelement, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Dateien, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Die *[Projektname].wpp.targets* Datei sollte jetzt diesem ähneln:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Speichern und schließen Sie die *[Projektname].wpp.targets* Datei.

Das nächste Mal Sie Builds und des Pakets, das Webanwendungsprojekt, WPP erkennt automatisch die *. wpp.targets* Datei. Alle Dateien und Ordner, die Sie angegeben werden nicht in der Web-Paket enthalten.

## <a name="conclusion"></a>Schlussfolgerung

Dieses Thema beschreibt, wie Sie bestimmte Dateien und Ordner ausschließen, wenn Sie eine Web-Paket erstellen, durch Erstellen einer benutzerdefinierten *. wpp.targets* Datei im gleichen Ordner wie die Projektdatei der Web-Anwendung.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Verwenden von benutzerdefinierter Projektdateien von Microsoft Build Engine (MSBuild) um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen über das Packen und Bereitstellungsprozess, finden Sie unter [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Bereitstellung von Paketen](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), und [ Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

>[!div class="step-by-step"]
[Zurück](deploying-membership-databases-to-enterprise-environments.md)
[Weiter](taking-web-applications-offline-with-web-deploy.md)
