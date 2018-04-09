---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Erstellen und Verpacken Webanwendungsprojekte | Microsoft Docs
author: jrjlee
description: Wenn Sie ein Webprojekt für die Anwendung in einer remote-Server bereitstellen möchten, ist die erste Aufgabe erstellen Sie das Projekt, und generieren eine Web-Bereitstellung Packa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="building-and-packaging-web-application-projects"></a>Erstellen und Verpacken Webanwendungsprojekte
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Wenn Sie ein Webprojekt für die Anwendung in einer remote-Server bereitstellen möchten, ist die erste Aufgabe zum Erstellen des Projekts und zum Generieren eines Webbereitstellungspakets. Dieses Thema beschreibt die Funktionsweise des Buildprozesses für Webanwendungsprojekte. Insbesondere wird erklärt, aus:
> 
> - Wie die Publishing Web Pipeline (WPP) des Buildprozesses Bereitstellung Funktionen erweitert.
> - Wie die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) für Ihre Webanwendung in ein Bereitstellungspaket aktiviert.
> - Wie der Build und Verpacken-Prozess, und welche Dateien erstellt werden.


In Visual Studio 2010 wird der Prozess Build- und Bereitstellungsprozess für Webanwendungsprojekte von WPP unterstützt. Die WPP bietet eine Reihe von Microsoft Build Engine (MSBuild)-Ziele, die die Funktionalität von MSBuild erweitern, und aktivieren Sie ihn für die Integration von Web Deploy. In Visual Studio können Sie diese erweiterte Funktionalität auf den Eigenschaftenseiten für Ihr Webprojekt für die Anwendung finden Sie unter. Die **Web packen/veröffentlichen** Seite zusammen mit den **SQL packen/veröffentlichen** Seite können Sie konfigurieren, wie das Webprojekt für die Anwendung für die Bereitstellung verpackt wird, wenn der Buildprozess abgeschlossen ist.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Wie funktioniert die WPP?

Wenn Sie einen Blick auf die Projektdatei für ein C#-werfen-Basis Webanwendungsprojekt, sehen Sie, dass es zwei TARGETS-Dateien importiert.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


Die erste **Import** Anweisung gilt für alle Visual C#-Projekten. Diese Datei *Microsoft.CSharp.targets*, Ziele und Aufgaben, die bestimmten in Visual c# enthält. Z. B. den C#-Compiler (**Csc**) Aufgabe hier aufgerufen wird. Die *Microsoft.CSharp.targets* Datei wiederum Importe der *Microsoft.Common.targets* Datei. Ziele, die für alle Projekte, z. B. sind definiert **erstellen**, **Rebuild**, **ausführen**, **Kompilieren**, und **bereinigen** . Die zweite **Import** Anweisung gilt nur für Webanwendungsprojekte. Die *Microsoft.WebApplication.targets* Datei wiederum Importe der *Microsoft.Web.Publishing.targets* Datei. Die *Microsoft.Web.Publishing.targets* Datei im Wesentlichen *ist* WPP. Es definiert ausgerichtet ist, z. B. **Paket** und **MSDeployPublish**, die invoke Web Deploy, um die verschiedenen Bereitstellungsaufgaben abzuschließen.

Um zu verstehen, wie diese zusätzliche Ziele verwendet werden, in der Beispielprojektmappe Kontakt-Manager öffnen die *Publish.proj* Datei, und sehen Sie sich die **BuildProjects** Ziel.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Dieses Ziel verwendet die **MSBuild** Aufgabe zum Erstellen von verschiedenen Projekten. Beachten Sie, dass die **DeployOnBuild** und **DeployTarget** Eigenschaften:

- Die **DeployOnBuild = "true"** Eigenschaft im Wesentlichen bedeutet "Ich möchte ein zusätzliches Ziel ausgeführt werden, wenn der Build erfolgreich abgeschlossen wurde."
- Die **DeployTarget** Eigenschaft identifiziert den Namen des Ziels ausgeführt wird, wenn sollen die **DeployOnBuild** -Eigenschaft gleich **"true"**. In diesem Fall geben Sie, dass Sie MSBuild ausgeführt werden soll die **Paket** Ziel nach dem Erstellen des Projekts.

Die **Paket** Ziel ist definiert, der *Microsoft.Web.Publishing.targets* Datei. Dieses Ziel wird im Wesentlichen nimmt die Buildausgabe des Webanwendungsprojekts und verwandelt sie in ein Webbereitstellungspaket, die in einem IIS-Webserver veröffentlicht werden können.

> [!NOTE]
> So zeigen Sie eine Projektdatei an (z. B. <em>ContactManager.Mvc.csproj</em>) in Visual Studio 2010 müssen Sie zunächst das Projekt aus der Projektmappe entladen. In der <strong>Projektmappen-Explorer</strong> rechten Maustaste auf den Projektknoten, und klicken Sie dann auf <strong>Projekt entladen</strong>. Mit der rechten Maustaste erneut auf des Projektknotens, und klicken Sie dann auf <strong>bearbeiten</strong><em>[-Projektdatei]</em>). Die Projektdatei wird in seiner Rohform XML geöffnet. Denken Sie daran, um das Projekt erneut laden, wenn Sie fertig sind.  
> Weitere Informationen zu MSBuild-Ziele, Aufgaben und <strong>Import</strong> -Anweisungen finden Sie unter [verstehen die Projektdatei](understanding-the-project-file.md). Eine ausführliche Einführung in die Projektdateien und die WPP finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi und William Bartholomew, ISBN-Nummer: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Was ist ein Webbereitstellungspaket?

Beim Erstellen und Bereitstellen von einem Webanwendungsprojekt mithilfe von Visual Studio 2010 oder direkt mithilfe von MSBuild, ist das Endergebnis in der Regel eine *Webbereitstellungspaket*. Webbereitstellungspaket ist eine ZIP-Datei. Es enthält alles, was, von denen IIS und Web Deploy benötigen, um Ihre Webanwendung neu erstellen einschließlich:

- Die kompilierte Ausgabe Ihrer Web-Anwendung, einschließlich Inhalt, Ressourcendateien, Konfigurationsdateien, JavaScript und cascading Stylesheets (CSS) Ressourcen und So weiter.
- Assemblys, die für Ihr Webprojekt für die Anwendung sowie für alle Projekte innerhalb einer Projektmappe, auf die verwiesen wird.
- SQL-Skripts Datenbanken generieren, die Sie mit der Webanwendung bereitstellen.

Sobald das Webbereitstellungspaket generiert wurde, können Sie es in einem IIS-Webserver auf verschiedene Weise veröffentlichen. Beispielsweise können Remote durch Angeben der Web Deploy Remote-Agents-Dienst oder Bereitstellung-Ereignishandler auf dem Zielserver bereitstellen oder können Sie IIS-Manager auf das Paket auf dem Ziel-Webserver manuell importieren. Weitere Informationen zu diesen Vorgehensweisen zur Bereitstellung finden Sie unter [Auswählen der rechts Ansatz für die Webbereitstellung](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Funktionsweise des Buildprozesses

Dies zeigt, was geschieht, wenn Sie erstellen und Packen Sie ein Webanwendungsprojekt:

![](building-and-packaging-web-application-projects/_static/image2.png)

Wenn Sie ein Webanwendungsprojekt erstellen, wird während des Erstellungsprozesses generiert eine Datei namens *[Projektname]. "SourceManifest.xml"*. Zusammen mit der Projektdatei und die Buildausgabe dies *. "SourceManifest.xml"* Datei weist Web Deploy-er muss in das Webbereitstellungspaket eingeschlossen werden sollen. Verwenden diese Eingaben, Web Deploy ein Webbereitstellungspakets mit dem Namen generiert *[Projektname] ZIP*.

Neben den Webbereitstellungspaket generiert während des Erstellungsprozesses zwei Dateien, mit die Sie das Paket verwenden können:

- Die *. deploy.cmd* -Datei enthält einen Satz von parametrisierten Web Deploy (MSDeploy.exe)-Befehlen, die Ihre Webbereitstellungspaket auf einem remote-IIS-Webserver veröffentlichen. Ausführen der *. deploy.cmd* -Datei mit entsprechenden Parametern in der Regel stellt eine schnellere und einfachere alternative zum Erstellen manuell die MSDeploy.exe Befehlen selbst.
- Die *SetParameters.xml* Datei enthält einen Satz von Parameterwerten an den MSDeploy.exe-Befehl. Zu diesen Werten zählen Eigenschaften, z. B. den Namen des IIS-Webanwendung, zu dem das Paket, das die Werte der Dienstendpunkte bereitgestellt werden soll, und Verbindungszeichenfolgen definiert, in, der *"Web.config"* Datei und jede Eigenschaft eines anwendungsbereitstellungstyps Werte, die auf den Eigenschaftenseiten des Projekts definiert.

Die *SetParameters.xml* Datei ist entscheidend, verwalten den Bereitstellungsprozess. Diese Datei wird entsprechend den Inhalten der das Webanwendungsprojekt dynamisch generiert. Angenommen, Sie fügen eine Verbindungszeichenfolge für Ihre *"Web.config"* Datei während des Erstellungsprozesses wird automatisch erkennen die Verbindungszeichenfolge, parametrisieren Sie die Bereitstellung entsprechend, und erstellen Sie einen Eintrag in der  *SetParameters.xml* Datei, die im Rahmen des Bereitstellungsprozesses die Verbindungszeichenfolge ändern können. Im nächsten Thema [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md), wird die Rolle dieser Datei im Detail erläutert, und beschreibt die verschiedenen Möglichkeiten, in dem können Sie es während der Erstellung und Bereitstellung ändern.

> [!NOTE]
> In Visual Studio 2010 unterstützt die WPP nicht das Vorkompilieren von Seiten in einer Web-Anwendung vor dem Packen. Die nächste Version von Visual Studio und die WPP umfasst die Möglichkeit zum Vorkompilieren einer Web-Anwendung als eine Verpackung fest.


## <a name="conclusion"></a>Schlussbemerkung

Dieses Thema liefert einen Überblick über die Build- und Verpackungsprozesses für Webanwendungsprojekte in Visual Studio 2010. Es beschrieben, wie die WPP Web Deploy-Befehle von MSBuild aufgerufen werden können, und diese erläutert, wie der Build und Verpacken-Prozess.

Nach der Erstellung eines Webbereitstellungspakets umfasst der nächste Schritt die Bereitstellung. Weitere Informationen dazu finden Sie unter [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md) und [Bereitstellen von Webpaketen](deploying-web-packages.md).

## <a name="further-reading"></a>Weiterführende Themen

Die nächsten Themen in diesem Lernprogramm [Konfigurieren von Parametern für die Bereitstellung von Paketen](configuring-parameters-for-web-package-deployment.md) und [Bereitstellen von Webpaketen](deploying-web-packages.md), bieten eine Anleitung zur Verwendung des Webpakets Sie erstellt haben. Die endgültige Lernprogramm dieser Reihe [Enterprise Webbereitstellung erweiterter](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), bietet Anleitungen zum Anpassen und Problembehandlung des Verpackungsprozesses.

Eine ausführliche Einführung in die Projektdateien und die WPP finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi und William Bartholomew, ISBN-Nummer: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-build-process.md)
> [Weiter](configuring-parameters-for-web-package-deployment.md)
