---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Festlegen von Ordnerberechtigungen | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 0660a464063783406a69caf663036811c8ac818e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802032"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Festlegen von Ordnerberechtigungen
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial legen Sie Berechtigungen für die *Elmah* Ordner in der bereitgestellten Web site, damit die Anwendung die Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mithilfe der Visual Studio Development Server (Cassini) oder IIS Express testen, können Sie die Anwendung unter Ihrer Identität ausgeführt. Sie sind in den meisten Fällen Administrator auf Ihrem Entwicklungscomputer und auf alle Dateien in einem beliebigen Ordner keine Berechtigung haben. Aber wenn eine Anwendung unter IIS ausgeführt wird, wird es unter der Identität, die definiert, die für den Anwendungspool, dem der Standort zugewiesen ist. Dies ist normalerweise ein vom System definierte Konto, das über beschränkte Berechtigungen verfügt. Standardmäßig sie Lese- und Ausführungsberechtigungen für Dateien und Ordner Ihrer Webanwendung, aber keinen Schreibzugriff auf.

Dies ist ein Problem, wenn Ihre Anwendung erstellt oder Updates-Dateien, die ist eine allgemeine in Webanwendungen benötigen. In der Contoso University-Anwendung erstellt Elmah XML-Dateien in die *Elmah* Ordner zum Speichern von Informationen zu Fehlern. Auch wenn Sie etwa Elmah nicht verwenden, kann Ihre Website können Benutzer, die Hochladen von Dateien oder andere Aufgaben, die Daten in einen Ordner auf Ihrer Website zu schreiben.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test fehlerprotokollierung und berichterstellung

Um festzustellen, wie die Anwendung in IIS nicht richtig funktioniert (obwohl es getan habe, wenn Sie ihn in Visual Studio getestet), Sie können dazu führen, dass einen Fehler, der normalerweise von Elmah protokolliert werden sollen, und öffnen Sie das Elmah-Fehlerprotokoll, um die Details anzuzeigen. Wenn Elmah konnte nicht zum Erstellen von XML-Datei und speichern die Fehlerdetails, sehen Sie einen leeren Bericht.

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, und fordern dann eine ungültige URL wie *Studentsxxx.aspx*. Eine vom System generierte Fehler Seite statt der *GenericErrorPage.aspx* Seite, da die `customErrors` Einstellung in der Datei "Web.config" ist "RemoteOnly", und Sie IIS lokal ausgeführt werden:

![HTTP-Fehler 404-Seite](setting-folder-permissions/_static/image1.png)

Führen Sie nun *Elmah.axd* um den Fehlerbericht anzuzeigen. Nachdem Sie sich mit den Anmeldeinformationen des Administratorkontos anmelden (&quot;Admin&quot; und &quot;Devpwd&quot;), eine leere Log-Fehlerseite wird angezeigt, da Elmah konnte nicht zum Erstellen einer XML-Datei in die *Elmah*Ordner:

![Fehler beim Protokoll leer](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Legen Sie die Schreibberechtigung für das Elmah-Ordner

Sie können Berechtigungen für den Ordner manuell festlegen oder können Sie es automatisch im Rahmen des Bereitstellungsprozesses machen. Somit automatische komplexen MSBuild-Code erfordert und da Sie nur dieses der ersten Anmeldung nötig, die Sie bereitstellen müssen, die folgenden Schritte aus wie Sie dies manuell durchführen. (Informationen dazu, wie Sie diesen Teil des Bereitstellungsprozesses zu machen, finden Sie unter [Ordner festlegen von Berechtigungen für die Webveröffentlichung](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi-Blog.)

1. In **Datei-Explorer**, navigieren Sie zu *C:\inetpub\wwwroot\ContosoUniversity*. Mit der rechten Maustaste die *Elmah* Ordner **Eigenschaften**, und wählen Sie dann die **Sicherheit** Registerkarte.
2. Klicken Sie auf **Bearbeiten**.
3. In der **Berechtigungen für Elmah** wählen Sie im Dialogfeld **DefaultAppPool**, und wählen Sie dann die **schreiben** Kontrollkästchen in der **zulassen** Spalte.

    ![Berechtigungen für Ordner "ELMAH"](setting-folder-permissions/_static/image3.png)

    (Wenn Sie nicht sehen **DefaultAppPool** in die **Gruppen-oder Benutzernamen** Liste Sie wahrscheinlich verwendet eine andere Methode als die in diesem Tutorial, um IIS und ASP.NET 4 auf Ihrem Computer einzurichten. In diesem Fall herausfinden Sie, wie die Identität von Anwendungspools, die die Contoso University-Anwendung und Write-Berechtigung erteilen, um diese Identität zugewiesen verwendet wird. Finden Sie die Links zum Anwendungspoolidentitäten am Ende dieses Tutorials.) Klicken Sie auf **OK** in beiden Dialogfeldern.

## <a name="retest-error-logging-and-reporting"></a>Testen der fehlerprotokollierung und berichterstellung

Testen, indem Sie verursacht einen Fehler erneut auf die gleiche Weise (eine ungültige URL-Anforderung), und führen Sie die **Fehlerprotokoll** Seite. Dieses Mal wird der Fehler auf der Seite angezeigt.

![Seite "Fehler" ELMAH](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle Aufgaben zum Abrufen der Contoso University abgeschlossen in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. Im nächsten Tutorial werden Sie die Website öffentlich verfügbar machen, indem sie in Azure bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel war der Grund, warum Elmah kann nicht zum Speichern von Protokolldateien ist, ziemlich offensichtlich. Sie können IIS-Ablaufverfolgung in Fällen verwenden, wo die Ursache des Problems nicht so offensichtlich ist; finden Sie unter [Problembehandlung Fehler bei Anforderungen mithilfe von Ablaufverfolgung in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der Website IIS.net.

Weitere Informationen über das Gewähren von Berechtigungen für die Identitäten des dienstanwendungspools finden Sie unter [Anwendungspoolidentitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sicherer Inhalt in IIS über Dateisystem-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der Website IIS.net.

> [!div class="step-by-step"]
> [Zurück](deploying-to-iis.md)
> [Weiter](deploying-to-production.md)
