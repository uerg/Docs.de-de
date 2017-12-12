---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Problembehandlung des Verpackungsprozesses | Microsoft Docs
author: jrjlee
description: "In diesem Thema wird beschrieben, wie Sie ausführliche Informationen zu des Verpackungsprozesses erfassen können, mit der EnablePackageProcessLoggingAndAssert-Eigenschaft in der M..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-the-packaging-process"></a>Problembehandlung des Verpackungsprozesses
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie Sie die ausführliche Informationen zu des Verpackungsprozesses mit Sammeln der **EnablePackageProcessLoggingAndAssert** Eigenschaft in der Microsoft Build Engine (MSBuild).
> 
> Beim Festlegen der **EnablePackageProcessLoggingAndAssert** Eigenschaft **"true"**, MSBuild wird:
> 
> - Fügen Sie zusätzliche Informationen zu des Verpackungsprozesses zu Build-Protokollen.
> - Protokollieren Sie Fehler unter bestimmten Umständen kann z. B., wenn Duplikate in der Liste der Verpackung gefunden werden.
> - Erstellen Sie ein Protokollverzeichnis in der *Projektname*\_Ordner Verpacken und verwenden, um Datensätze Informationen zu den Dateien, die Sie verpacken möchten.
> 
> Wenn des Verpackungsprozesses fehlschlägt, oder Ihre Web-Bereitstellungspakete enthalten nicht die Dateien, die Sie erwarten, können diese Informationen Sie Probleme bei Prozess und Pinpoint sind Dinge falschen wo.
> 
> > [!NOTE]
> > Die **EnablePackageProcessLoggingAndAssert** Eigenschaft funktioniert nur, wenn Sie das Projekt mithilfe Erstellen der **Debuggen** Konfiguration. In anderen Konfigurationen ist die Eigenschaft ignoriert.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Diese Reihe von Lernprogrammen verwendet eine Beispielprojektmappe & #x 2014; die [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; zum Darstellen einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf der Teilung Datei Herangehensweise beschrieben [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird Projekt zwei Dateien & #x 2014; eine enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Grundlegendes zur EnablePackageProcessLoggingAndAssert-Eigenschaft

[Erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) beschrieben, wie die Publishing Web Pipeline (WPP) für einen Satz von MSBuild-Ziele bereitstellt, die die Funktionalität von MSBuild erweitern und ermöglicht die Integration in die Webserver (Internet Information Services, IIS) Webbereitstellungstool (Web Deploy). Wenn Sie ein Webanwendungsprojekt Verpacken, sind Sie WPP Ziele aufgerufen wird.

Viele dieser Ziele WPP einschließen bedingten Logik, die zusätzliche Informationen protokolliert bei der **EnablePackageProcessLoggingAndAssert** -Eigenschaftensatz auf **"true"**. Wenn Sie überprüfen, z. B. die **Paket** Ziel, können Sie sehen, dass eine zusätzliche Protokollverzeichnis erstellt und eine Liste der Dateien in eine Textdatei schreibt, wenn **EnablePackageProcessLoggingAndAssert** gleich **"true"**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Die WPP Ziele werden definiert, der *Microsoft.Web.Publishing.targets* Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web". Sie können die folgende Datei öffnen und überprüfen die Ziele in Visual Studio 2010 oder eines XML-Editors. Sorgen Sie nicht, um den Inhalt der Datei nicht ändern.


## <a name="enabling-the-additional-logging"></a>Zusätzliche Protokollierung aktivieren

Sie können angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** Eigenschaft in verschiedenen Möglichkeiten, je nachdem, wie Sie das Projekt erstellen.

Wenn Sie das Projekt über die Befehlszeile erstellen, können Sie angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** Eigenschaft als Befehlszeilenargument:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Wenn Sie eine Datei für benutzerdefinierte Projektvorlagen zum Erstellen von Projekten verwenden, können Sie enthalten die **EnablePackageProcessLoggingAndAssert** Wert in der **Eigenschaften** Attribut des der **MSBuild**Aufgabe:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Wenn Sie eine Builddefinition für Team Foundation Server (TFS) zum Erstellen von Projekten verwenden, können Sie angeben, einen Wert für die **EnablePackageProcessLoggingAndAssert** Eigenschaft in der **MSBuild-Argumente** Zeile:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Weitere Informationen zum Erstellen und Konfigurieren von Builddefinitionen finden Sie unter [erstellen eine erstellen, unterstützt Definitionsbereitstellung](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Auch wenn Sie das Paket in jedem Build einschließen möchten, können Sie ändern die Projektdatei für das Webprojekt für die Anwendung Festlegen der **EnablePackageProcessLoggingAndAssert** Eigenschaft **"true"**. Fügen Sie die Eigenschaft mit dem ersten **PropertyGroup** Element innerhalb der CSPROJ- oder VBPROJ-Datei.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Protokolldateien

Wenn Sie erstellen und Packen Sie ein Webanwendungsprojekt mit **EnablePackageProcessLoggingAndAssert** festgelegt **"true"**, MSBuild erstellt einen zusätzlichen Ordner, die mit dem Namen anmelden die *Projektname* \_Paketordner. Der Protokollordner enthält verschiedene Dateien:

![](troubleshooting-the-packaging-process/_static/image2.png)

Die Liste der Dateien, die Sie sehen, hängt die Schritte in Ihrem Projekt und Build-Prozess ab. Diese Dateien werden jedoch in der Regel verwendet, um die Liste der Dateien, die die WPP erfasst für Verpackung, die in verschiedenen Phasen des Prozesses aufzuzeichnen:

- Die *PreExcludePipelineCollectFilesPhaseFileList.txt* Datei listet die Dateien, die MSBuild erfasst, für das Verpacken, bevor alle Dateien angegeben sind, für den Ausschluss entfernt werden.
- Die *AfterExcludeFilesFilesList.txt* Datei enthält die Liste der geänderten Datei, nach der alle Dateien angegeben sind, für den Ausschluss entfernt werden.

    > [!NOTE]
    > Weitere Informationen zum Ausschließen von Dateien und Ordnern aus des Verpackungsprozesses finden Sie unter [Ausschließen von Dateien und Ordnern über Bereitstellung](excluding-files-and-folders-from-deployment.md).
- Die *AfterTransformWebConfig.txt* Datei listet die Dateien, die für Verpackung nach allen erfassten *"Web.config"* Transformationen ausgeführt wurden. In dieser Liste können alle konfigurationsspezifischen *"Web.config"* Dateien, wie z. B. transformieren *"Web.Debug.config"* und *Web.Release.config*, aus der Liste der Dateien für ausgeschlossen werden Verpacken. Ein einzelnes transformiert *"Web.config"* an ihrer Stelle enthalten ist.
- Die *PostAutoParameterizationWebConfigConnectionStrings.txt* -Datei enthält die Liste der Dateien nach der Verbindungszeichenfolgen in der *"Web.config"* Datei haben parametrisiert wurde. Dies ist der Prozess, mit dem Sie die Verbindungszeichenfolgen mit den richtigen Einstellungen für Ihre zielumgebung ersetzen, wenn Sie das Paket bereitstellen.
- Die *Prepackage.txt* -Datei enthält die abgeschlossene Präbuild-Liste der Dateien, die in das Paket eingeschlossen werden.

> [!NOTE]
> Die Namen der zusätzlichen Protokolldateien entsprechen in der Regel WPP Ziele. Sie können diese Ziele überprüfen, indem Sie untersuchen die *Microsoft.Web.Publishing.targets* Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web".


Wenn der Inhalt des Webpakets nicht Ihren Erwartungen entsprechen, kann überprüfen diese Dateien eine gute Möglichkeit, an welchem Punkt im Prozess Dinge ein aufgetretenes Problem zu identifizieren.

## <a name="conclusion"></a>Schlussfolgerung

In diesem Thema beschrieben, wie Sie verwenden, können die **EnablePackageProcessLoggingAndAssert** -Eigenschaft in MSBuild des Verpackungsprozesses zu beheben. Es erläutert die verschiedenen Möglichkeiten, in dem Sie den Eigenschaftswert angibt, die während des Erstellungsprozesses angeben können, und es beschrieben die zusätzliche Informationen, die aufgezeichnet werden, wenn Sie die Eigenschaft, um festlegen **"true"**.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Weitere Informationen zu den WPP und wie des Verpackungsprozesses verwaltet, finden Sie unter [erstellen und Packen Webanwendungsprojekte](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Anleitung zur Verwendung von Web-Bereitstellungspakete bestimmte Dateien und Ordner ausschließen, finden Sie unter [Ausschließen von Dateien und Ordnern über Bereitstellung](excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Zurück](running-windows-powershell-scripts-from-msbuild-project-files.md)
