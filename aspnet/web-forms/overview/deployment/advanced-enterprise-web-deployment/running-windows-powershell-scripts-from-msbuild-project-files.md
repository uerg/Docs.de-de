---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ausführen von Windows PowerShell-Skripts von MSBuild-Projektdateien | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Prozesses Build- und Bereitstellungsprozess ausgeführt wird. Sie können ein Skript lokal ausführen (das heißt, auf die b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Ausführen von Windows PowerShell-Skripts von MSBuild-Projektdateien
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie ein Windows PowerShell-Skript als Teil eines Prozesses Build- und Bereitstellungsprozess ausgeführt wird. Sie können ein Skript lokal (das heißt, auf dem Buildserver) oder Remote auf einem Ziel-Webserver oder Datenbankserver wie.
> 
> Es gibt viele Gründe, warum Sie möglicherweise nach der Bereitstellung Windows PowerShell-Skript ausführen möchten. Auf diese Weise können Sie z. B. folgende Vorgänge durchführen:
> 
> - Eine benutzerdefinierte Ereignisquelle zur Registrierung hinzuzufügen.
> - Ein Dateisystemverzeichnis für Uploads zu generieren.
> - Bereinigen Sie Buildverzeichnisse.
> - Schreiben Sie Einträge in eine benutzerdefinierte Protokolldatei.
> - Einladen von Benutzern zu einer neu bereitgestellte Webanwendung-e-Mails zu senden.
> - Erstellen Sie Benutzerkonten mit den entsprechenden Berechtigungen.
> - Konfigurieren der Replikation zwischen SQL Server-Instanzen.
> 
> In diesem Thema erfahren Sie, wie Sie Windows PowerShell-Skripts sowohl lokal als auch Remote über ein benutzerdefiniertes Ziel in einer Microsoft Build Engine (MSBuild)-Projektdatei ausführen.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="task-overview"></a>Übersicht über den Task

Um ein Windows PowerShell-Skript im Rahmen einer automatisierten oder einstufiger Bereitstellung auszuführen, müssen Sie diese allgemeinen Aufgaben ausführen:

- Fügen Sie das Windows PowerShell-Skript zur Projektmappe und zur quellcodeverwaltung hinzu.
- Erstellen Sie einen Befehl, der das Windows PowerShell-Skript aufruft.
- Versehen Sie reservierten XML-Zeichen in Ihrem Befehl mit Escapezeichen.
- Erstellen Sie ein Ziel in der benutzerdefinierte MSBuild-Projektdatei, und verwenden Sie die **Exec** Task zum Ausführen des Befehls.

In diesem Thema erfahren Sie, wie Sie diese Verfahren ausführen. Aufgaben und exemplarische Vorgehensweisen in diesem Thema wird davon ausgegangen, dass Sie bereits mit der MSBuild-Ziele und Eigenschaften vertraut sind und Sie erfahren, wie eine benutzerdefinierte MSBuild-Projektdatei zu verwenden, um einen Prozess Build- und Bereitstellungsprozess Laufwerk. Weitere Informationen finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Erstellen und Hinzufügen von Windows PowerShell-Skripts

Die Aufgaben in diesem Thema verwenden Sie ein Windows PowerShell-Beispielskript, mit dem Namen **LogDeploy.ps1** zum Ausführen von Skripts von MSBuild veranschaulichen. Die **LogDeploy.ps1** Skript enthält eine einfache Funktion, die einen Eintrag einzeilige in eine Protokolldatei schreibt:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


Die **LogDeploy.ps1** Skript akzeptiert zwei Parameter. Der erste Parameter den vollständigen Pfad darstellt, in die Protokolldatei einen Eintrag hinzugefügt werden soll, und der zweite Parameter darstellt, das Bereitstellungsziel aus, dem in der Protokolldatei aufgezeichnet werden sollen. Wenn Sie das Skript ausführen, fügt eine Zeile in die Protokolldatei im folgenden Format hinzu:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Vornehmen der **LogDeploy.ps1** Skript MSBuild zur Verfügung, müssen Sie:

- Das Skript zur quellcodeverwaltung hinzufügen.
- Fügen Sie das Skript, um die Projektmappe in Visual Studio 2010.

Sie müssen die Bereitstellungsskripts mit Ihrer Lösungsinhalt, unabhängig davon, ob das Skript auf dem Buildserver oder auf einem Remotecomputer ausgeführt werden sollen. Eine Möglichkeit ist das Skript in einen Projektmappenordner hinzuzufügen. Da Sie Windows PowerShell-Skript als Teil des Bereitstellungsprozesses verwenden möchten, ist es im Beispiel Contact Manager sinnvoll ist, fügen das Skript in den Projektmappenordner veröffentlichen.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Den Inhalt der Projektmappenordner werden zum Erstellen von Servern als Quellmaterial kopiert. Allerdings bilden sie kein Teil des Projektausgabe.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Ein Windows PowerShell-Skript ausführen, auf dem Buildserver

In einigen Szenarien empfiehlt es sich um Windows PowerShell-Skripts auf dem Computer ausgeführt wird, in dem Ihre Projekte erstellt. Beispielsweise können Sie ein Windows PowerShell-Skript verwenden, bereinigen Sie Buildordner oder die Einträge in eine benutzerdefinierte Protokolldatei zu schreiben.

Im Hinblick auf die Syntax ist ein Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen dasselbe wie das Ausführen von Windows PowerShell-Skript aus einer normalen Eingabeaufforderung. Sie müssen die ausführbare powershell.exe aufrufen und Verwenden der **– Befehl** Switch, geben Sie die Befehle, die Windows PowerShell ausgeführt werden sollen. (In Windows PowerShell v2, können Sie auch die **– Datei** wechseln). Der Befehl sollte dieses Format aufweisen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Zum Beispiel:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Wenn der Pfad für das Skript Leerzeichen enthält, müssen Sie den Dateipfad in einfachen Anführungszeichen, die nach einem kaufmännischen und-Zeichen einschließen. Sie können doppelte Anführungszeichen nicht verwenden, da Sie bereits sie verwendet haben, mit den Befehl eingeschlossen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Es gibt einige zusätzliche Überlegungen beim Aufrufen dieses Befehls von MSBuild. Sie sollten zunächst berücksichtigen die **– NonInteractive** Flag, um sicherzustellen, dass das Skript im stillen Modus ausgeführt wird. Als Nächstes sollten Sie enthalten die **– "ExecutionPolicy"** Flag mit einem Wert des entsprechenden Arguments. Dies gibt an, dass Windows PowerShell wird für Ihr Skript und können Sie die standardausführungsrichtlinie, überschreiben die Ausführung des Skripts möglicherweise verhindert werden, die Ausführungsrichtlinie. Sie können diese Argumentwerte auswählen:

- Der Wert **uneingeschränkt** lässt Windows PowerShell, führen Sie das Skript, unabhängig davon, ob das Skript nicht signiert ist.
- Der Wert **"RemoteSigned"** lässt Windows PowerShell, nicht signierte Skripts auszuführen, die auf dem lokalen Computer erstellt wurden. Allerdings müssen die Skripts, die an anderer Stelle erstellt wurden signiert werden. (In der Praxis können Sie sehr wahrscheinlich nicht zu Windows PowerShell-Skript lokal auf einem Build-Server erstellt haben).
- Der Wert **"AllSigned"** lässt Windows PowerShell, nur signierte Skripts auszuführen.

Ist die standardausführungsrichtlinie **eingeschränkte**, dadurch wird verhindert, dass Windows PowerShell alle Skriptdateien ausführen.

Schließlich müssen Sie alle reservierten XML-Zeichen mit Escapezeichen versehen, die in Windows PowerShell-Befehls auftreten:

- Ersetzen Sie einfache Anführungszeichen mit  **&amp;Apos;**
- Ersetzen von doppelten Anführungszeichen mit  **&amp;Quot;**
- Ersetzen Sie kaufmännische und-Zeichen mit  **&amp;Amp;**

- Wenn Sie diese Änderungen vornehmen, wird der Befehl so aussehen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Innerhalb der benutzerdefinierte MSBuild-Projektdatei können ein neues Ziel erstellen und Verwenden der **Exec** Aufgabe zum Ausführen dieses Befehls:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Beachten Sie, dass in diesem Beispiel:

- Alle Variablen, wie Parameterwerte und den Speicherort der ausführbaren Windows PowerShell-Datei werden als MSBuild-Eigenschaften deklariert.
- Bedingungen sind enthalten, damit Benutzer diese Werte in der Befehlszeile überschreiben können.
- Die **MSDeployComputerName** Eigenschaft an anderer Stelle in der Projektdatei deklariert ist.

Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, wird Windows PowerShell den Befehl und einen Protokolleintrag schreiben, zu der Datei, die Sie angegeben haben.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Ein Windows PowerShell-Skript ausführen auf einem Remotecomputer

Windows PowerShell ist zur Ausführung von Skripts auf Remotecomputern über [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Zu diesem Zweck müssen Sie verwenden die [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) Cmdlet. Dadurch können Sie das Skript auf einem oder mehreren Remotecomputern ausführen, ohne kopieren das Skript auf den Remotecomputern. Ergebnisse werden auf dem lokalen Computer zurückgegeben, in denen Sie das Skript ausgeführt.

> [!NOTE]
> Vor der Verwendung der **Invoke-Command** -Cmdlet zum Ausführen von Windows PowerShell-Skripts auf einem Remotecomputer befindet, müssen Sie einen WinRM-Listener zur Aufnahme von remote-Nachrichten konfigurieren. Hierzu können Sie durch Ausführen des Befehls **Winrm Quickconfig** auf dem Remotecomputer. Weitere Informationen finden Sie unter [Installation und Konfiguration für Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Im Windows PowerShell-Fenster, verwenden Sie diese Syntax zum Ausführen der **LogDeploy.ps1** Skript auf einem Remotecomputer:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Es gibt verschiedene andere Möglichkeiten der Verwendung von **Invoke-Command** mit einem Skript-Datei, aber dieser Ansatz ist vergleichsweise einfach, wenn Sie Parameter angeben und Verwalten von Pfaden mit Leerzeichen müssen.


Wenn Sie dies an einer Eingabeaufforderung ausführen, müssen Sie die ausführbare Windows PowerShell aufrufen und Verwenden der **– Befehl** Parameter, um Ihre Anweisungen bereitzustellen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Wie vor, müssen Sie einige zusätzliche Switches bereitstellen und escape reservierten XML-Zeichen, wenn Sie den Befehl auf MSBuild ausführen:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Zum Schluss wie zuvor können Sie die **Exec** Aufgabe in eine benutzerdefinierte MSBuild-Ziel zum Ausführen des Befehls:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Wenn Sie dieses Ziel als Teil des Buildprozesses ausführen, führen Windows PowerShell das Skript auf dem Computer, die Sie, in angegeben der **– Computername** Argument.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschriebenen Windows PowerShell-Skript aus einer MSBuild-Projektdatei ausführen. Sie können diesen Ansatz verwenden, ein Windows PowerShell-Skript im Rahmen einer automatisierten Prozesses oder eines einstufiger Build- und Bereitstellungsprozess entweder lokal oder auf einem Remotecomputer ausgeführt.

## <a name="further-reading"></a>Weiterführende Themen

Anleitungen zum Signieren von Windows PowerShell-Skripts und Verwalten von Ausführungsrichtlinien finden Sie unter [Ausführen von Windows PowerShell-Skripts](https://technet.microsoft.com/library/ee176949.aspx). Um Hilfe bei der Windows PowerShell-Befehle von einem Remotecomputer ausführen, finden Sie unter [Ausführen von Remotebefehlen](https://technet.microsoft.com/library/dd819505.aspx).

Weitere Informationen finden Sie unter benutzerdefinierte MSBuild-Projektdateien um den Bereitstellungsprozess zu steuern, finden Sie unter [verstehen die Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md) und [Verständnis des Build-Prozesses](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Zurück](taking-web-applications-offline-with-web-deploy.md)
> [Weiter](troubleshooting-the-packaging-process.md)
