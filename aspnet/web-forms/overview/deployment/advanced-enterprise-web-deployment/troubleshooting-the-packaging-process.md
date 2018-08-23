---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Problembehandlung des Verpackungsprozesses | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Sie ausführliche Informationen über den Prozess der paketerstellung sammeln können, mit der EnablePackageProcessLoggingAndAssert-Eigenschaft in der M...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827629"
---
<a name="troubleshooting-the-packaging-process"></a>Problembehandlung des Verpackungsprozesses
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie ausführliche Informationen über den Prozess der paketerstellung mit erfassen können die **EnablePackageProcessLoggingAndAssert** -Eigenschaft in der Microsoft Build Engine (MSBuild).
> 
> Beim Festlegen der **EnablePackageProcessLoggingAndAssert** Eigenschaft **"true"**, MSBuild wird:
> 
> - Weitere Informationen über den Prozess der paketerstellung und die erstellungsprotokolle hinzugefügt.
> - Melden Sie Fehler unter bestimmten Bedingungen kann z. B., wenn doppelte Dateien in der Liste der paketerstellung gefunden werden.
> - Erstellen Sie ein Protokollverzeichnis in der *ProjectName*\_Paket-Ordner, und verwenden, um Informationen zu den Dateien, Packen Sie sind, aufzuzeichnen.
> 
> Wenn des Verpackungsprozesses tritt ein Fehler auf, oder die Web-Bereitstellungspakete enthalten nicht die Dateien, die Sie erwarten, können diese Informationen Sie beheben Sie den Prozess und die Pinpoint, in dem sind falsche Stand der Dinge.
> 
> > [!NOTE]
> > Die **EnablePackageProcessLoggingAndAssert** Eigenschaft funktioniert nur, wenn Sie dem Projekt mit erstellen die **Debuggen** Konfiguration. Die Eigenschaft wird in anderen Konfigurationen ignoriert.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Grundlegendes zur EnablePackageProcessLoggingAndAssert-Eigenschaft

[Erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) beschrieben, wie das Web Publishing Pipeline (WPP) einen Satz von MSBuild-Ziele bereitstellt, die erweitern die Funktionalität von MSBuild und ermöglicht die Integration in das Web (Internet Information Services, IIS) Webbereitstellungstool (Web Deploy). Wenn Sie ein Webanwendungsprojekt Verpacken, sind Sie WPP Ziele aufrufen.

Viele dieser Ziele WPP einschließen bedingten Logik, die zusätzliche Informationen protokolliert. wenn die **EnablePackageProcessLoggingAndAssert** -Eigenschaftensatz auf **"true"**. Wenn Sie überprüfen, z. B. die **Paket** Ziel, können Sie sehen, dass es eine zusätzliche Protokollverzeichnis erstellt und eine Liste der Dateien in eine Textdatei schreibt, wenn **EnablePackageProcessLoggingAndAssert** entspricht **"true"**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Die WPP Ziele werden definiert, der *Microsoft.Web.Publishing.targets* -Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web". Sie können diese Datei öffnen und überprüfen die Ziele in Visual Studio 2010 oder eines XML-Editors. Achten Sie darauf, dass Sie nicht um den Inhalt der Datei zu ändern.


## <a name="enabling-the-additional-logging"></a>Die zusätzliche Protokollierung aktivieren

Sie können angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** Eigenschaft verschiedene Möglichkeiten, je nachdem, wie Sie Ihr Projekt erstellen.

Wenn Sie Ihr Projekt über die Befehlszeile erstellen, können Sie angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** Eigenschaft als Befehlszeilenargument:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Wenn Sie eine Datei für benutzerdefinierte Projektvorlagen zum Erstellen von Projekten verwenden, können Sie einschließen der **EnablePackageProcessLoggingAndAssert** Wert in der **Eigenschaften** Attribut der **MSBuild**Aufgabe:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Wenn Sie eine Builddefinition für Team Foundation Server (TFS) zum Erstellen von Projekten verwenden, können Sie angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** -Eigenschaft in der **MSBuild-Argumente** Zeile:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Weitere Informationen zum Erstellen und Konfigurieren von Build-Definitionen finden Sie unter [Erstellen einer Build-Definition, unterstützt Bereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Auch wenn das Paket in jedem Build angezeigt werden sollen, können Sie ändern die Projektdatei für das Webanwendungsprojekt, legen Sie die **EnablePackageProcessLoggingAndAssert** Eigenschaft **"true"**. Sie sollten die Eigenschaft hinzufügen, mit dem ersten **PropertyGroup** Element innerhalb der CSPROJ- oder VBPROJ-Datei.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Protokolldateien

Wenn Sie zu erstellen und Packen ein Webanwendungsprojekts mit **EnablePackageProcessLoggingAndAssert** festgelegt **"true"**, MSBuild erstellt einen zusätzlichen Ordner, die benannte Protokolldatei, in der *ProjectName* \_Paketordner. Ordner "Log" enthält die verschiedenen Dateien:

![](troubleshooting-the-packaging-process/_static/image2.png)

Die Liste der Dateien, die Sie sehen, variieren je die Dinge in Ihrem Projekt und Build-Prozess. Diese Dateien werden jedoch in der Regel verwendet, um die Liste der Dateien, die die WPP erfasst für das Verpacken, die in verschiedenen Phasen des Prozesses aufzuzeichnen:

- Die *PreExcludePipelineCollectFilesPhaseFileList.txt* Datei enthält die Dateien, die MSBuild erfasst, für das Verpacken, bevor alle Dateien, die angegeben werden, für den Ausschluss entfernt werden.
- Die *AfterExcludeFilesFilesList.txt* -Datei enthält die Liste der geänderten Dateien aus, nachdem alle Dateien, die angegeben werden, für den Ausschluss entfernt wurden.

    > [!NOTE]
    > Weitere Informationen zum Ausschließen von Dateien und Ordner von der des Verpackungsprozesses, finden Sie unter [Ausschließen von Dateien und Ordner von der Bereitstellung](excluding-files-and-folders-from-deployment.md).
- Die *AfterTransformWebConfig.txt* Datei aufgeführt, die für die paketerstellung nach jedem gesammelten Dateien *"Web.config"* Transformationen ausgeführt wurden. In dieser Liste alle konfigurationsspezifischen *"Web.config"* transformieren Sie die Dateien, wie z. B. *"Web.Debug.config"* und *Datei "Web.Release.config"*, aus der Liste der Dateien ausgeschlossen sind Verpacken. Ein einzelnes transformiert *"Web.config"* ist an ihrer Stelle enthalten.
- Die *PostAutoParameterizationWebConfigConnectionStrings.txt* -Datei enthält die Liste der Dateien nach dem die Verbindungszeichenfolgen in der *"Web.config"* Datei haben parametrisiert wurde. Dies ist der Prozess, mit dem Sie die Verbindungszeichenfolgen mit den richtigen Einstellungen für Ihre zielumgebung ersetzen, wenn Sie das Paket bereitstellen.
- Die *Prepackage.txt* Datei enthält die abgeschlossene vor dem Erstellen-Liste von Dateien, die im Paket enthalten sein.

> [!NOTE]
> Die Namen der zusätzlichen Protokolldateien entsprechen in der Regel WPP Ziele. Sie können diese Ziele überprüfen, anhand der *Microsoft.Web.Publishing.targets* -Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web".


Wenn der Inhalt Ihrer Webpakets nicht Ihren Erwartungen entsprechen, möglich überprüfen diese Dateien eine gute Möglichkeit, zu identifizieren, an welchem Punkt im Prozess was falsch ist ein Fehler aufgetreten.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschriebenen Informationen zur Verwendung der **EnablePackageProcessLoggingAndAssert** -Eigenschaft in MSBuild des Verpackungsprozesses zu behandeln. Es erläutert die verschiedenen Möglichkeiten, in dem Sie den Wert der Eigenschaft für den Buildprozess angeben können, und es wurde beschrieben, die zusätzliche Informationen, die aufgezeichnet wird, wenn Sie die Eigenschaft, um festlegen **"true"**.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu WPP und wie des Verpackungsprozesses verwaltet, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Anleitungen dazu, wie Sie bestimmte Dateien und Ordner von Web-Bereitstellungspakete ausschließen, finden Sie unter [Ausschließen von Dateien und Ordner von der Bereitstellung](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Vorherige](running-windows-powershell-scripts-from-msbuild-project-files.md)
