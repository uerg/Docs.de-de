---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Erstellen und Packen von Webanwendungsprojekten | Microsoft-Dokumentation
author: jrjlee
description: Wenn Sie ein Webanwendungsprojekt in einer remote-Server-Umgebung bereitstellen möchten, müssen Sie zunächst das Projekt zu erstellen und generieren eine Web-Bereitstellung Packa...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 406b8e6daf47196eb98700efe41e34c02d5682d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833315"
---
<a name="building-and-packaging-web-application-projects"></a>Erstellen und Packen von Webanwendungsprojekten
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie ein Webanwendungsprojekt in einer remote-Server-Umgebung bereitstellen möchten, ist Ihre erste Aufgabe erstellen Sie das Projekt, und Erstellen eines Webbereitstellungspakets an. Dieses Thema beschreibt die Funktionsweise des Buildprozesses für Webanwendungsprojekte. Insbesondere wird erläutert, es:
> 
> - Wie erweitert Web Publishing Pipeline (WPP) während des Buildprozesses Bereitstellungsfunktion enthalten.
> - Wie das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) für Ihre Webanwendung in ein Bereitstellungspaket aktiviert.
> - Zur des erstellungs- und Packvorgangs-Prozess aus, und welche Dateien erstellt werden.


In Visual Studio 2010 wird der Build & Deployment-Prozess für Webanwendungsprojekte von WPP unterstützt. Die WPP stellt einen Satz von Microsoft Build Engine (MSBuild)-Ziele, die erweitern die Funktionalität von MSBuild und ermöglicht die Integration von Web Deploy, bereit. In Visual Studio sehen Sie diese erweiterte Funktionalität für Ihr Webprojekt für die Anwendung auf den Eigenschaftenseiten. Die **Web packen/veröffentlichen** Seite zusammen mit den **SQL packen/veröffentlichen** Seite können Sie konfigurieren, wie das Webanwendungsprojekt für die Bereitstellung verpackt wird, wenn der Buildprozess abgeschlossen ist.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Wie funktioniert die WPP?

Wenn Sie einen Blick auf die Projektdatei für ein C# -Code verwenden-je Webanwendungsprojekt, sehen Sie, dass es sich um zwei TARGETS-Dateien importiert.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Die erste **Import** -Anweisung, die alle Visual C#-Projekte gemeinsam ist. Diese Datei *"Microsoft.CSharp.targets"*, Ziele und Aufgaben, die spezifisch zu Visual c# sind enthält. Z. B. der C#-Compiler (**Csc**) Aufgabe hier aufgerufen wird. Die *"Microsoft.CSharp.targets"* Datei wiederum Importe der *"Microsoft.Common.targets"* Datei. Ziele, die für alle Projekte, wie z. B. gelten definiert **erstellen**, **Rebuild**, **ausführen**, **Kompilieren**, und **bereinigen** . Die zweite **Import** Anweisung ist spezifisch für Webanwendungsprojekte. Die *Microsoft.WebApplication.targets* Datei wiederum Importe der *Microsoft.Web.Publishing.targets* Datei. Die *Microsoft.Web.Publishing.targets* Datei im Wesentlichen *ist* WPP. Ziele, z. B. definiert **Paket** und **MSDeployPublish**, Web Deploy, um die verschiedenen Bereitstellungsaufgaben ausführen, das aufrufen.

Um zu verstehen, wie diese zusätzliche Ziele verwendet werden, in der Contact Manager-beispiellösung öffnen die *Publish.proj* Datei, und sehen Sie sich die **BuildProjects** Ziel.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Dieses Ziel verwendet die **MSBuild** Task, um verschiedene Projekte zu erstellen. Beachten Sie, dass die **DeployOnBuild** und **DeployTarget** Eigenschaften:

- Die **DeployOnBuild = True** Eigenschaft im Wesentlichen bedeutet "Ich möchte ein zusätzliches Ziel ausgeführt werden soll, wenn der Build erfolgreich abgeschlossen wurde."
- Die **DeployTarget** -Eigenschaft gibt den Namen des Ziels ausgeführt wird, wenn sollen die **DeployOnBuild** Eigenschaft **"true"**. In diesem Fall geben Sie, dass Sie MSBuild zum ausführen soll die **Paket** Ziel nach dem Erstellen des Projekts.

Die **Paket** Ziel ist definiert, der *Microsoft.Web.Publishing.targets* Datei. Im Wesentlichen wird dieses Ziel wird die Buildausgabe Ihres Webprojekts für die Anwendung und stellt sich in einem Webbereitstellungspaket, die in einem IIS-Webserver veröffentlicht werden können.

> [!NOTE]
> Eine Projektdatei an (z. B. <em>ContactManager.Mvc.csproj</em>) in Visual Studio 2010 müssen Sie zuerst das Projekt aus der Projektmappe entladen. In der <strong>Projektmappen-Explorer</strong> rechten Maustaste auf den Projektknoten, und klicken Sie dann auf <strong>Projekt entladen</strong>. Mit der rechten Maustaste erneut auf des Projektknotens, und klicken Sie dann auf <strong>bearbeiten</strong><em>[Projektdatei]</em>). Die Projektdatei wird im XML-Rohformat geöffnet. Denken Sie daran, auf das Projekt erneut laden, wenn Sie fertig sind.  
> Weitere Informationen zu MSBuild-Ziele, Aufgaben und <strong>Import</strong> -Anweisungen finden Sie unter [Grundlegendes zur Projektdatei](understanding-the-project-file.md). Eine ausführlichere Einführung in Projektdateien und die Apps, finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Was ist ein Webbereitstellungspaket?

Beim Erstellen und ein Webanwendungsprojekts, Bereitstellen entweder mithilfe von Visual Studio 2010 oder direkt mithilfe von MSBuild, ist das Endergebnis in der Regel eine *Webbereitstellungspaket*. Das Webbereitstellungspaket ist eine ZIP-Datei. Enthält alle Informationen, von denen IIS und Web Deploy benötigen, um Ihre Webanwendung neu erstellen einschließlich:

- Die kompilierte Ausgabe Ihrer Web-Anwendung, einschließlich der Inhalte, Ressourcendateien, Konfigurationsdateien, JavaScript und cascading Stylesheets (CSS) Ressourcen, und So weiter.
- Assemblys, die für Ihr Webprojekt für die Anwendung sowie für alle Projekte in Ihrer Lösung, auf die verwiesen wird.
- SQL-Skripts zum Generieren von Datenbanken, die Sie mit der Webanwendung bereitstellen.

Nachdem das Webbereitstellungspaket generiert wurde, können Sie sie in einem IIS-Webserver auf verschiedene Weise veröffentlichen. Beispielsweise können Sie es bereitstellen per Remotezugriff von der Web Deploy Remote-Agents-Dienst oder die WebHandler bereitstellen auf dem Ziel-Webserver als Ziel haben, oder Sie können IIS-Manager verwenden, um das Paket auf dem Zielserver manuell zu importieren. Weitere Informationen zu diesen Vorgehensweisen zur Bereitstellung finden Sie unter [Entscheidung zur Webbereitstellung rechts](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Wie funktioniert der Buildprozess?

Dies zeigt, was geschieht, wenn Sie erstellen und ein Webanwendungsprojekts Verpacken:

![](building-and-packaging-web-application-projects/_static/image2.png)

Wenn Sie ein Webanwendungsprojekt erstellen, wird während des Buildprozesses werden eine Datei namens *[Projektname]. "SourceManifest.xml"*. Zusammen mit der Projektdatei und die Ausgabe des Buildvorgangs dies *. "SourceManifest.xml"* Datei weist Web Deploy benötigte in das Webbereitstellungspaket eingeschlossen werden sollen. Verwenden diese Eingaben, Web Deploy einem Webbereitstellungspaket, der mit dem Namen generiert *[Projektname] ZIP*.

Neben das Webbereitstellungspaket generiert der Buildprozess zwei Dateien, mit die Sie das Paket verwenden können:

- Die *. "Deploy.cmd"* -Datei enthält einen Satz von parametrisierten Web Deploy (MSDeploy.exe) Befehlen, die Ihr Webbereitstellungspaket auf einem remote-IIS-Webserver zu veröffentlichen. Ausführen der *. "Deploy.cmd"* -Datei mit entsprechenden Parametern in der Regel stellt eine schnellere und einfachere alternative zum manuellen Erstellen der MSDeploy.exe Befehlen selbst.
- Die *"SetParameters.xml"* Datei enthält einen Satz von Parameterwerten, die den MSDeploy.exe-Befehl. Dazu gehören Eigenschaften wie den Namen der IIS-Webanwendung auf die Sie zur Bereitstellung des Pakets, das die Werte der Dienstendpunkte und Verbindungszeichenfolgen definiert, der *"Web.config"* -Datei und jede Eigenschaft eines anwendungsbereitstellungstyps Werte werden auf den Eigenschaftenseiten des Projekts definiert.

Die *"SetParameters.xml"* Datei ist der Schlüssel für die Verwaltung des Bereitstellungsprozesses. Diese Datei wird entsprechend den Inhalten der Webanwendungsprojekt dynamisch generiert. Z. B., wenn Sie eine Verbindungszeichenfolge zum Hinzufügen Ihrer *"Web.config"* -Datei des Buildprozesses automatisch erkennt die Verbindungszeichenfolge, parametrisieren Sie die Bereitstellung entsprechend, und erstellen Sie einen Eintrag in der  *"SetParameters.xml"* Datei, sodass Sie die Verbindungszeichenfolge im Rahmen des Bereitstellungsprozesses ändern können. Im nächsten Thema, [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md), erläutert die Rolle der Datei im Detail und beschreibt die verschiedenen Möglichkeiten, die in der können Sie es während der Erstellung und Bereitstellung ändern.

> [!NOTE]
> In Visual Studio 2010 unterstützt die WPP nicht Vorkompilieren der Seiten in einer Webanwendung vor dem Packen. Die nächste Version von Visual Studio und die WPP umfasst die Möglichkeit zum Vorkompilieren einer Webanwendung als paketerstellung-Option.


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird eine Übersicht über die Build- und Verpackungsprozess für Webanwendungsprojekte in Visual Studio 2010 bereitgestellt. Es wurde beschrieben, wie die WPP Web Deploy-Befehle aus MSBuild aufrufen kann, und er erläutert, wie die erstellungs- und Packvorgangs-Prozess.

Nachdem Sie einem Webbereitstellungspaket erstellt haben, werden im nächste Schritt bereitstellen. Weitere Informationen hierzu finden Sie unter [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md) und [Bereitstellen von Webpaketen](deploying-web-packages.md).

## <a name="further-reading"></a>Weiterführende Themen

Die nächsten Themen in diesem Tutorial [Konfigurieren von Parametern für die Bereitstellung von Paket](configuring-parameters-for-web-package-deployment.md) und [Bereitstellen von Webpaketen](deploying-web-packages.md), bieten eine Anleitung zur Verwendung des Webpakets Sie erstellt haben. Das letzte Tutorial dieser Reihe [erweiterte webbasierte Unternehmensbereitstellung](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), enthält Sie Anleitungen zum Anpassen und Problembehandlung des Verpackungsprozesses.

Eine ausführlichere Einführung in Projektdateien und die Apps, finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-build-process.md)
> [Weiter](configuring-parameters-for-web-package-deployment.md)
