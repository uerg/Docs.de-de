---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 81d5107c7e25a10218d13175c47913c94e9ab3fd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827851"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial legen Sie Berechtigungen für die *Elmah* Ordner in der bereitgestellten Web site, damit die Anwendung die Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mit Visual Studio Development Server (Cassini) testen, können Sie die Anwendung unter Ihrer Identität ausgeführt. Sie sind in den meisten Fällen Administrator auf Ihrem Entwicklungscomputer und auf alle Dateien in einem beliebigen Ordner keine Berechtigung haben. Aber wenn eine Anwendung unter IIS ausgeführt wird, wird es unter der Identität, die definiert, die für den Anwendungspool, dem der Standort zugewiesen ist. Dies ist normalerweise ein vom System definierte Konto, das über beschränkte Berechtigungen verfügt. Standardmäßig sie Lese- und Ausführungsberechtigungen für Dateien und Ordner Ihrer Webanwendung, aber keinen Schreibzugriff auf.

Dies ist ein Problem, wenn Ihre Anwendung erstellt oder Updates-Dateien, die ist eine allgemeine in Webanwendungen benötigen. In der Contoso University-Anwendung erstellt Elmah XML-Dateien in die *Elmah* Ordner zum Speichern von Informationen zu Fehlern. Auch wenn Sie etwa Elmah nicht verwenden, kann Ihre Website können Benutzer, die Hochladen von Dateien oder andere Aufgaben, die Daten in einen Ordner auf Ihrer Website zu schreiben.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testen die Fehler, Protokollierung und Berichterstellung

Um festzustellen, wie die Anwendung in IIS nicht richtig funktioniert (obwohl es getan habe, wenn Sie ihn in Visual Studio getestet), Sie können dazu führen, dass einen Fehler, der normalerweise von Elmah protokolliert werden sollen, und öffnen Sie das Elmah-Fehlerprotokoll, um die Details anzuzeigen. Wenn Elmah konnte nicht zum Erstellen von XML-Datei und speichern die Fehlerdetails, sehen Sie einen leeren Bericht.

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, und fordern dann eine ungültige URL wie *Studentsxxx.aspx*. Eine vom System generierte Fehler Seite statt der *GenericErrorPage.aspx* Seite, da die `customErrors` Einstellung in der Datei "Web.config" ist "RemoteOnly", und Sie IIS lokal ausgeführt werden:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Führen Sie nun *Elmah.axd* um den Fehlerbericht anzuzeigen. Eine leere Log-Fehlerseite wird angezeigt, da Elmah konnte nicht zum Erstellen einer XML-Datei in die *Elmah* Ordner:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Einstellung Schreibberechtigung für den Elmah-Ordner

Sie können Berechtigungen für den Ordner manuell festlegen oder können Sie es automatisch im Rahmen des Bereitstellungsprozesses machen. Somit automatisch komplexen MSBuild-Code erfordert und da Sie müssen nur dazu beim ersten Sie bereitstellen, in diesem Tutorial nur gezeigt, wie dies manuell tun. (Informationen dazu, wie Sie diesen Teil des Bereitstellungsprozesses zu machen, finden Sie unter [Ordner festlegen von Berechtigungen für die Webveröffentlichung](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi-Blog.)

In **Windows Explorer**, navigieren Sie zu *C:\inetpub\wwwroot\ContosoUniversity*. Mit der rechten Maustaste die *Elmah* Ordner **Eigenschaften**, und wählen Sie dann die **Sicherheit** Registerkarte.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Wenn Sie nicht sehen **DefaultAppPool** in die **Gruppen-oder Benutzernamen** Liste Sie wahrscheinlich verwendet eine andere Methode als die in diesem Tutorial, um IIS und ASP.NET 4 auf Ihrem Computer einzurichten. In diesem Fall herausfinden Sie, wie die Identität von Anwendungspools, die die Contoso University-Anwendung und Write-Berechtigung erteilen, um diese Identität zugewiesen verwendet wird. Finden Sie die Links zum Anwendungspoolidentitäten am Ende dieses Tutorials.)

Klicken Sie auf **Bearbeiten**. In der **Berechtigungen für Elmah** wählen Sie im Dialogfeld **DefaultAppPool**, und wählen Sie dann die **schreiben** Kontrollkästchen in der **zulassen** Spalte.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Klicken Sie auf **OK** in beiden Dialogfeldern.

## <a name="retesting-error-logging-and-reporting"></a>Einen erneuten Test Fehler Protokollierung und Berichterstellung

Testen, indem Sie verursacht einen Fehler erneut auf die gleiche Weise (eine ungültige URL-Anforderung), und führen Sie die **Fehlerprotokoll** Seite. Dieses Mal wird der Fehler auf der Seite angezeigt.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Sie müssen auch auf Schreibberechtigungen verfügen die *App\_Daten* Ordner da haben Sie die SQL Server Compact-Datenbank-Dateien in diesem Ordner, und Sie Daten in diesen Datenbanken zu aktualisieren möchten. In diesem Fall jedoch, Sie müssen zusätzliche Schritte auszuführen, weil während des Bereitstellungsvorgangs automatisch die Schreibberechtigung für festgelegt die *App\_Daten* Ordner.

Sie haben jetzt alle Aufgaben zum Abrufen der Contoso University abgeschlossen in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. Im nächsten Tutorial werden Sie die Website öffentlich verfügbar machen, indem in einem Hostinganbieter bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel war der Grund, warum Elmah kann nicht zum Speichern von Protokolldateien ist, ziemlich offensichtlich. Sie können IIS-Ablaufverfolgung in Fällen verwenden, wo die Ursache des Problems nicht so offensichtlich ist; finden Sie unter [Problembehandlung Fehler bei Anforderungen mithilfe von Ablaufverfolgung in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der Website IIS.net.

Weitere Informationen über das Gewähren von Berechtigungen für die Identitäten des dienstanwendungspools finden Sie unter [Anwendungspoolidentitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sicherer Inhalt in IIS über Dateisystem-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der Website IIS.net.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
