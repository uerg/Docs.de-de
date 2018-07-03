---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build & Deployment-Prozesses ausgeführt wird. Sie können ein Skript lokal ausführen (das heißt, auf die b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362894"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Ausführen von Windows PowerShell-Skripts aus MSBuild-Projektdateien
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Build & Deployment-Prozesses ausgeführt wird. Sie können ein Skript lokal ausführen, (das heißt, auf dem Buildserver) oder Remote auf einem Ziel-Webserver oder Datenbankserver wie.
> 
> Es gibt viele Gründe, warum Sie möglicherweise einen Windows PowerShell-Skript nach der Bereitstellung ausführen möchten. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:
> 
> - Fügen Sie eine benutzerdefinierte Ereignisquelle in der Registrierung hinzu.
> - Generieren Sie ein Dateisystemverzeichnis für Uploads.
> - Bereinigen Sie Buildverzeichnisse.
> - Geschrieben Sie Einträge in eine benutzerdefinierte Protokolldatei.
> - Einladen von Benutzern zu einer neu bereitgestellten Webanwendung-e-Mails zu senden.
> - Erstellen Sie Benutzerkonten mit den entsprechenden Berechtigungen an.
> - Konfigurieren der Replikation zwischen SQL Server-Instanzen.
> 
> In diesem Thema zeigt Ihnen, wie Sie Windows PowerShell-Skripts in ein benutzerdefiniertes Ziel in einer Projektdatei Microsoft Build Engine (MSBuild) sowohl lokal als auch remote ausführen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Um ein Windows PowerShell-Skript als Teil einer automatisierten oder Schritt für Schritt Bereitstellungsprozess auszuführen, müssen Sie diese allgemeinen Aufgaben ausführen:

- Fügen Sie das Windows PowerShell-Skript zu Ihrer Projektmappe und quellcodeverwaltung.
- Erstellen Sie einen Befehl, der Windows PowerShell-Skript aufruft.
- Versehen Sie reservierten XML-Zeichen in Ihrem Befehl mit Escapezeichen.
- Erstellen eines Ziels in der benutzerdefinierten MSBuild-Projektdatei, und Verwenden der **Exec** Aufgabe, die den Befehl ausgeführt.

In diesem Thema zeigt Sie, wie Sie diese Schritte ausführen. Die Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit MSBuild-Ziele und Eigenschaften vertraut sind und Sie erfahren, wie eine benutzerdefinierte MSBuild-Projektdatei zu verwenden, um einen Build & Deployment-Prozess zu steuern. Weitere Informationen finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Erstellen und Hinzufügen von Windows PowerShell-Skripts

Die Aufgaben in diesem Thema verwenden Sie ein Windows PowerShell-Beispielskript, mit dem Namen **LogDeploy.ps1** zum veranschaulichen des zum Ausführen von Skripts aus MSBuild. Die **LogDeploy.ps1** Skript enthält eine einfache Funktion, die einen Eintrag für einzeilige in eine Protokolldatei schreibt:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


Die **LogDeploy.ps1** Skript akzeptiert zwei Parameter. Der erste Parameter stellt den vollständigen Pfad, in die Protokolldatei zu der Sie einen Eintrag hinzufügen möchten, und der zweite Parameter stellt das Bereitstellungsziel aus, dem in der Protokolldatei aufgezeichnet werden sollen. Wenn Sie das Skript ausführen, fügt es eine Zeile in die Protokolldatei im folgenden Format hinzu:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Zu den **LogDeploy.ps1** Skript MSBuild zur Verfügung, müssen Sie:

- Das Skript zur quellcodeverwaltung hinzufügen.
- Fügen Sie das Skript für Ihre Lösung in Visual Studio 2010.

Sie müssen nicht stellen Sie das Skript mit Ihrer Lösungsinhalt, unabhängig davon, ob das Skript auf dem Buildserver oder auf einem Remotecomputer ausgeführt werden sollen. Eine Möglichkeit ist das Skript einen Projektmappenordner hinzu. Da Sie das Windows PowerShell-Skript im Rahmen des Bereitstellungsprozesses verwenden möchten, ist es im Contact Manager-Beispiel sinnvoll, fügen das Skript in den Projektmappenordner veröffentlichen.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Der Inhalt der Projektmappenordner werden zum Erstellen von Servern als Quellcode kopiert. Allerdings bilden sie kein Teil des Projektausgabe.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Ein Windows PowerShell-Skript ausführen, auf dem Buildserver

In einigen Szenarien möchten möglicherweise Windows PowerShell-Skripts auf dem Computer ausgeführt wird, in dem Ihre Projekte erstellt. Beispielsweise können Sie ein Windows PowerShell-Skript verwenden, die Buildordner bereinigen oder Einträge in eine benutzerdefinierte Protokolldatei geschrieben.

In Bezug auf Syntax ist ein Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen dasselbe wie das Ausführen eines Windows PowerShell-Skripts über eine reguläre Eingabeaufforderung. Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die Windows PowerShell, die ausführbare Datei aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen: Wie zuvor müssen Sie einige zusätzliche Schalter angeben und reservierten XML-Zeichen als Escapezeichen, beim Ausführen des Befehls aus MSBuild: Zum Schluss wie zuvor können Sie die Exec Aufgabe in einem benutzerdefinierten MSBuild-Ziel zum Ausführen des Befehls:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Zum Beispiel:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, führt Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der – Computername Argument. In diesem Thema beschrieben, wie Sie ein Windows PowerShell-Skript über eine MSBuild-Projektdatei ausführen.


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Sie können diesen Ansatz verwenden, um ein Windows PowerShell-Skript, entweder lokal oder auf einem Remotecomputer befindet, als Teil eines automatisierten oder Schritt für Schritt Build & Deployment-Prozesses ausgeführt. Anleitungen zum Signieren von Windows PowerShell-Skripts, und Verwalten von Ausführungsrichtlinien finden Sie unter **Ausführen von Windows PowerShell-Skripts**. Anleitungen zum Ausführen von Windows PowerShell-Befehle von einem Remotecomputer finden Sie unter **Ausführen von Remotebefehlen**. Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter Grundlegendes zur Projektdatei und Verständnis des Prozesses erstellen. Sie können über diese Argumentwerte auswählen:

- Der Wert **uneingeschränkt** ermöglicht Windows PowerShell zum Ausführen des Skripts, unabhängig davon, ob das Skript signiert ist.
- Der Wert **"RemoteSigned"** ermöglicht Windows PowerShell zum Ausführen von nicht signierten Skripts, die auf dem lokalen Computer erstellt wurden. Allerdings müssen Skripts, die an anderer Stelle erstellt wurden, signiert werden. (In der Praxis können Sie sehr wahrscheinlich nicht zu einem Windows PowerShell-Skript lokal auf einem Buildserver erstellt haben).
- Der Wert **"AllSigned"** ermöglicht Windows PowerShell, um nur digital signierte Skripts auszuführen.

Ist die standardausführungsrichtlinie **Restricted**, wodurch verhindert wird, dass Windows PowerShell alle Skriptdateien ausgeführt.

Abschließend müssen Sie reservierten XML-Zeichen mit Escapezeichen versehen, die auftreten, in Ihrem Windows PowerShell-Befehl:

- Ersetzen Sie einzelne Anführungszeichen mit  **&amp;Apos;**
- Ersetzen von doppelten Anführungszeichen mit  **&amp;Quot;**
- Ersetzen Sie kaufmännische und-Zeichen mit  **&amp;Amp;**

- Wenn Sie diese Änderungen vornehmen, wird der Befehl sieht folgendermaßen aus:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Innerhalb der benutzerdefinierten MSBuild-Projektdatei, können Sie ein neues Ziel erstellen und Verwenden der **Exec** Aufgabe zum Ausführen dieses Befehls:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Beachten Sie, dass in diesem Beispiel:

- Alle Variablen werden wie die Parameterwerte und den Speicherort der ausführbaren Datei Windows PowerShell werden als MSBuild-Eigenschaften deklariert.
- Bedingungen sind enthalten, damit Benutzer diese Werte über die Befehlszeile außer Kraft setzen können.
- Die **MSDeployComputerName** Eigenschaft wird an anderer Stelle in der Projektdatei deklariert.

Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, wird Windows PowerShell den Befehl ausführen und einen Protokolleintrag, der die angegebene Datei zu schreiben.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Ein Windows PowerShell-Skript ausführen auf einem Remotecomputer

Windows PowerShell ist in der Lage der Ausführung von Skripts auf Remotecomputern über [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Zu diesem Zweck müssen Sie verwenden die [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) Cmdlet. Dadurch können Sie das Skript für eine oder mehrere Remotecomputer ausführen, ohne das Skript auf den Remotecomputern zu kopieren. Ergebnisse werden auf dem lokalen Computer zurückgegeben, in denen Sie das Skript ausgeführt wurde.

> [!NOTE]
> Vor der Verwendung der **Invoke-Command** -Cmdlet zum Ausführen von Windows PowerShell-Skripts auf einem Remotecomputer befindet, müssen Sie einen WinRM-Listener zum Akzeptieren von remote-Nachrichten konfigurieren. Sie erreichen dies durch Ausführen des Befehls **Winrm Quickconfig** auf dem Remotecomputer. Weitere Informationen finden Sie unter [Installation und Konfiguration für Windows-Remoteverwaltung](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Im Windows PowerShell-Fenster, verwenden Sie diese Syntax zum Ausführen der **LogDeploy.ps1** Skript auf einem Remotecomputer:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Es gibt verschiedene andere Möglichkeiten der Verwendung von **Invoke-Command** zum Ausführen eines Skripts-Datei, aber dieser Ansatz ist vergleichsweise einfach, wenn Sie Parameterwerte bereitstellen und verwalten Sie Pfade mit Leerzeichen müssen.


Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die Windows PowerShell, die ausführbare Datei aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Wie zuvor müssen Sie einige zusätzliche Schalter angeben und reservierten XML-Zeichen als Escapezeichen, beim Ausführen des Befehls aus MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Zum Schluss wie zuvor können Sie die **Exec** Aufgabe in einem benutzerdefinierten MSBuild-Ziel zum Ausführen des Befehls:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Wenn Sie dieses Ziel im Rahmen des Buildprozesses ausführen, führt Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der **– Computername** Argument.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie Sie ein Windows PowerShell-Skript über eine MSBuild-Projektdatei ausführen. Sie können diesen Ansatz verwenden, um ein Windows PowerShell-Skript, entweder lokal oder auf einem Remotecomputer befindet, als Teil eines automatisierten oder Schritt für Schritt Build & Deployment-Prozesses ausgeführt.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Signieren von Windows PowerShell-Skripts, und Verwalten von Ausführungsrichtlinien finden Sie unter [Ausführen von Windows PowerShell-Skripts](https://technet.microsoft.com/library/ee176949.aspx). Anleitungen zum Ausführen von Windows PowerShell-Befehle von einem Remotecomputer finden Sie unter [Ausführen von Remotebefehlen](https://technet.microsoft.com/library/dd819505.aspx).

Weitere Informationen zu benutzerdefinierte MSBuild-Projektdateien verwenden, um den Bereitstellungsprozess zu steuern, finden Sie unter [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Zurück](taking-web-applications-offline-with-web-deploy.md)
> [Weiter](troubleshooting-the-packaging-process.md)
