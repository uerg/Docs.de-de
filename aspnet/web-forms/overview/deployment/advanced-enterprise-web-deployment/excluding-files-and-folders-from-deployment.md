---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Ausschließen von Dateien und Ordner von der Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie auszuschließen, können Sie Dateien und Ordner von einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33eecfea86842a93eac705e7823f1eba9519670e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816677"
---
<a name="excluding-files-and-folders-from-deployment"></a>Ausschließen von Dateien und Ordner von der Bereitstellung
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie auszuschließen, können Sie Dateien und Ordner von einem Webbereitstellungspaket beim Erstellen und Packen ein Webanwendungsprojekt.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="overview"></a>Übersicht

Wenn Sie ein Webanwendungsprojekt in Visual Studio 2010 erstellen, können die Web Publishing Pipeline (WPP) dieses Buildprozesses zu erweitern, indem Sie Ihrer kompilierten Webanwendung in einem Webpaket bereitgestellt packen. Sie können dann verwenden die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) auf dieses Webpaket bereitgestellt, um einen remote-IIS-Webserver oder das Webpaket manuell über den IIS-Manager zu importieren. Dieser Prozess der paketerstellung wird erläutert, [erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Wie Sie Kontrolle haben, was in Ihrer Web enthalten ruft gepackt werden? Die projekteinstellungen in Visual Studio über die zugrunde liegende Projektdatei, geben Sie ausreichend Kontrolle für eine Vielzahl von Szenarien. In einigen Fällen möchten jedoch möglicherweise den Inhalt Ihrer Webpakets für bestimmte zielumgebungen anpassen. Beispielsweise empfiehlt es sich um einen Ordner für Protokolldateien einschließen, wenn Sie Ihre Anwendung in einer testumgebung bereitstellen, aber schließen Sie den Ordner aus, wenn Sie die Anwendung in einer Umgebung Staging- oder produktionsumgebung bereitstellen. In diesem Thema zeigt Ihnen, wie Sie dies tun.

## <a name="what-gets-included-by-default"></a>Was ruft standardmäßig enthalten?

Wenn Sie die Projekteigenschaften für Web-Anwendung in Visual Studio konfigurieren die **bereitzustellenden Elemente** auf in der Liste der **Web packen/veröffentlichen** Seite können Sie angeben, was in Ihrer Web-Bereitstellung eingeschlossen werden soll das Paket. Standardmäßig ist dies festgelegt auf **nur Dateien, die zum Ausführen dieser Anwendung benötigt**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Bei der Auswahl **nur Dateien, die zum Ausführen dieser Anwendung benötigt**, WPP wird versucht, um zu bestimmen, welche Dateien auf das Webpaket hinzugefügt werden soll. Dies umfasst Folgendes:

- Alle Buildausgaben die für das Projekt.
- Alle Dateien, die mit einer Buildaktion von markiert **Content**.

> [!NOTE]
> Die Logik, die bestimmt, welche Dateien enthalten, ist in dieser Datei enthalten:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Bestimmte Dateien und Ordner ausschließen

In einigen Fällen sollten eine präzisere Kontrolle Sie über die Dateien und Ordner bereitgestellt werden. Wenn Sie wissen, welche Dateien sollen vor der Zeit, und der Ausschluss gilt für alle zielumgebungen, legen Sie einfach die **Buildvorgang** jeder Datei **keine**.

**Um bestimmte Dateien von der Bereitstellung ausgeschlossen.**

1. In der **Projektmappen-Explorer** rechten Maustaste auf die Datei, und klicken Sie dann auf **Eigenschaften**.
2. In der **Eigenschaften** Fenster in der **Buildvorgang** Zeile **keine**.

Dieser Ansatz ist jedoch nicht immer praktisch. Beispielsweise empfiehlt es sich um die Dateien zu variieren, und Ordner werden gemäß Ihrer zielumgebung und außerhalb von Visual Studio enthalten. Z. B. in der Contact Manager-beispiellösung, sehen Sie sich den Inhalt des Projekts ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- Der Ordner "internen" enthält einige SQL­Skripts, die der Entwickler verwendet wird, erstellen, löschen, und füllen Sie lokale Datenbanken zu Entwicklungszwecken. "Nothing" in diesem Ordner sollte in einer Umgebung Staging- oder produktionsumgebung bereitgestellt werden.
- Ordner "Scripts" enthält mehrere JavaScript-Dateien. Viele dieser Dateien sind ausschließlich zur Unterstützung des Debuggens, oder geben Sie die IntelliSense-Funktionen in Visual Studio enthalten. Einige dieser Dateien sollten nicht auf dem Staging-oder produktionsumgebung bereitgestellt werden. Allerdings empfiehlt es sich, sie in einer testumgebung für Entwickler, die Problembehandlung zu vereinfachen. bereitzustellen.

Obwohl Sie Ihre Projektdateien, um bestimmte Dateien und Ordner ausschließen bearbeiten können, besteht eine einfachere Möglichkeit zur Verfügung. Die WPP umfasst einen Mechanismus, um Dateien und Ordner ausschließen, indem Sie mit dem Namen Elementlisten erstellen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles**. Sie können diesen Mechanismus erweitern, indem Sie diese Listen eigene Elemente hinzufügt. Zu diesem Zweck müssen Sie die folgenden allgemeinen Schritte ausführen:

1. Erstellen Sie eine benutzerdefinierte Projektdatei namens *[Projektname].wpp.targets* im gleichen Ordner wie die Projektdatei.

    > [!NOTE]
    > Die *. wpp.targets* Datei muss im gleichen Ordner wie die Projektdatei Ihre Web-Anwendung&#x2014;z. B. *ContactManager.Mvc.csproj*&#x2014;nicht im gleichen Ordner wie die benutzerdefinierten die Projektdateien, die Sie verwenden, um den Build & Deployment-Prozess zu steuern.
2. In der *. wpp.targets* hinzufügen. eine **ItemGroup** Element.
3. In der **ItemGroup** Element hinzufügen **ExcludeFromPackageFolders** und **ExcludeFromPackageFiles** Elemente, die bestimmte Dateien und Ordner nach Bedarf ausschließen.

Dies ist die grundlegende Struktur dieser *. wpp.targets* Datei:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Beachten Sie, dass jedes Element ein Element-Metadatenelement mit dem Namen enthält **FromTarget**. Dies ist ein optionaler Wert, der den Buildprozess nicht beeinträchtigt. Es dient lediglich, um anzugeben, warum bestimmte Dateien oder Ordner weggelassen wurden, wenn jemand die erstellungsprotokolle überprüft.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Ausschließen von Dateien und Ordner aus einem Webpaket

Im nächste Verfahren veranschaulicht das Hinzufügen einer *. wpp.targets* Datei in ein Webanwendungsprojekt und wie Sie die Datei zu verwenden, um bestimmte Dateien und Ordner aus dem Webpaket ausschließen, wenn Sie Ihr Projekt erstellen.

**Auszuschließende Dateien und Ordner von einem Webbereitstellungspaket**

1. Öffnen Sie die Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster mit der rechten Maustaste des Projektknoten der Web-Anwendung (z. B. **ContactManager.Mvc**), zeigen Sie auf **hinzufügen**, und klicken Sie dann auf **Neues Element**.
3. In der **neues Element hinzufügen** wählen Sie im Dialogfeld die **XML-Datei** Vorlage.
4. In der **Namen** geben *[Projektname] ***.wpp.targets** (z. B. **ContactManager.Mvc.wpp.targets**), und klicken Sie dann auf **hinzufügen**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Wenn Sie auf den Stammknoten eines Projekts ein neues Element hinzufügen, wird die Datei im gleichen Ordner wie die Projektdatei erstellt. Sie können dies überprüfen, indem Sie den Ordner in Windows Explorer öffnen.
5. Fügen Sie in der Datei eine **Projekt** Element und ein **ItemGroup** Element:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Wenn Sie Ordner aus dem Webpaket ausschließen möchten, fügen ein **ExcludeFromPackageFolders** Element, das **ItemGroup** Element:

   1. In der **Include** -Attributs festzulegen, geben Sie eine durch Semikolons getrennte Liste der Ordner, die Sie ausschließen möchten.
   2. In der **FromTarget** Metadata-Element, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Ordner, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Wenn Sie Dateien aus dem Webpaket ausschließen möchten, fügen Sie eine **ExcludeFromPackageFiles** Element, das **ItemGroup** Element:

   1. In der **Include** -Attributs festzulegen, geben Sie eine durch Semikolons getrennte Liste der Dateien, die Sie ausschließen möchten.
   2. In der **FromTarget** Metadata-Element, geben Sie einen aussagekräftigen Wert um anzugeben, warum die Dateien, wie den Namen der ausgeschlossen werden die *. wpp.targets* Datei.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. Die *[Projektname].wpp.targets* Datei sollte nun dieser Struktur ähneln:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Speichern und schließen Sie die *[Projektname].wpp.targets* Datei.

Das nächste Mal Build und das Paket das Webanwendungsprojekt, WPP erkennt automatisch die *. wpp.targets* Datei. Alle Dateien und Ordner aus, die Sie angegeben haben, werden in das Webpaket nicht enthalten sein.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie bestimmte Dateien und Ordner ausschließen, wenn Sie ein Webpaket erstellen, durch Erstellen einer benutzerdefinierten *. wpp.targets* -Datei im gleichen Ordner wie die Projektdatei Ihre Web-Anwendung.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zur Verwendung von benutzerdefinierter Projektdateien von Microsoft Build Engine (MSBuild) um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu den paketerstellungs- und Bereitstellungsprozess, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Konfigurieren von Parametern für die Bereitstellung von Paket](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), und [ Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Zurück](deploying-membership-databases-to-enterprise-environments.md)
> [Weiter](taking-web-applications-offline-with-web-deploy.md)
