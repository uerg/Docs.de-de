---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Festlegen von Ordnerberechtigungen - 6 von 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm legen Sie Berechtigungen für die *Elmah* Ordner in der bereitgestellten Web site, damit die Anwendung Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mithilfe von Visual Studio Development Server (Cassini) testen, können Sie die Anwendung unter der Identität ausgeführt. Sie sind sehr wahrscheinlich ein Administrator auf dem Entwicklungscomputer und vollständige dann berechtigt, alle Dateien in einem beliebigen Ordner Schritte ausführen. Wird jedoch eine Anwendung unter IIS ausgeführt wird, ausgeführt unter der Identität, die definiert, die für den Anwendungspool aus, dem der Standort zugewiesen ist. Dies ist normalerweise eine systemdefinierte-Konto, das über beschränkte Berechtigungen verfügt. Standardmäßig verfügt über Lese- und Ausführungsberechtigungen für Ihre Webanwendung Dateien und Ordner, jedoch keinen Schreibzugriff auf.

Dies ist ein Problem, wenn die Anwendung erstellt oder Updates-Dateien, die ist eine häufige in Webanwendungen müssen. In der Anwendung Contoso University Elmah erstellt XML-Dateien in den *Elmah* Ordner, um Details zu Fehlern zu speichern. Auch wenn Sie etwa Elmah nicht verwenden, kann Ihre Website ermöglichen es Benutzern Hochladen von Dateien oder Durchführen weiterer Aufgaben, die Daten in einen Ordner auf Ihrer Website zu schreiben.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testen die Fehler, Protokollierung und-berichterstellung

Um festzustellen, wie die Anwendung in IIS nicht richtig funktioniert (obwohl dies der Fall, wenn Sie ihn in Visual Studio getestet), Sie können dazu führen, dass einen Fehler, der normalerweise von Elmah protokolliert werden würde, und öffnen Sie das Elmah-Fehlerprotokoll, um die Details anzuzeigen. Wenn Elmah konnte nicht zum Erstellen einer XML-Datei und speichern die Fehlerdetails, sehen Sie eine leere Fehlerbericht anzuzeigen.

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, und fordern dann eine ungültige URL wie *Studentsxxx.aspx*. Daraufhin eine vom System generierte Fehlerseite statt der *GenericErrorPage.aspx* Seite, da die `customErrors` Einstellung in der Datei "Web.config" ist "RemoteOnly", und Sie IIS lokal ausgeführt werden:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Führen Sie nun *Elmah.axd* den Fehlerbericht anzeigen. Eine leere Protokoll Fehlerseite wird angezeigt, da Elmah konnte nicht zum Erstellen einer XML-Datei in die *Elmah* Ordner:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Einstellung über die Schreibberechtigung für den Ordner Elmah

Sie können Berechtigungen für Ordner manuell festlegen oder Sie können es automatisch im Rahmen des Bereitstellungsprozesses vornehmen. Somit automatische komplexen MSBuild-Code erfordert, und da Sie nur dazu erstmalig haben Sie bereitstellen, dieses Lernprogramm nur wird gezeigt, wie Sie dies manuell durchführen. (Informationen dazu, wie in diesem Teil des Bereitstellungsprozesses vornehmen, finden Sie unter [Einstellung Ordnerberechtigungen auf Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi-Blog.)

In **Windows­Explorer**, navigieren Sie zu *C:\inetpub\wwwroot\ContosoUniversity*. Mit der rechten Maustaste die *Elmah* Ordner wählen **Eigenschaften**, und wählen Sie dann die **Sicherheit** Registerkarte.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Wenn Sie nicht sehen **DefaultAppPool** in der **Gruppen-oder Benutzernamen** Liste Sie wahrscheinlich verwendet eine andere Methode als die in diesem Lernprogramm, um IIS und ASP.NET 4 auf Ihrem Computer einzurichten. In diesem Fall feststellen Sie, welche Identität vom University Contoso-Anwendung, und über die Schreibberechtigung erteilen dieser Identität zugewiesen, Anwendungspool verwendet wird. Finden Sie unter den Links zur Anwendungspoolidentitäten am Ende dieses Lernprogramms.)

Klicken Sie auf **bearbeiten**. In der **Berechtigungen für Elmah** wählen Sie im Dialogfeld **DefaultAppPool**, und wählen Sie dann die **schreiben** Kontrollkästchen in der **zulassen** Spalte.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Klicken Sie auf **OK** in beiden Dialogfeldern.

## <a name="retesting-error-logging-and-reporting"></a>Ein erneuter Test Fehler Protokollierung und-berichterstellung

Testen, indem Sie verursacht einen Fehler erneut auf die gleiche Weise (eine ungültige URL-Anforderung), und führen Sie die **Fehlerprotokoll** Seite. Dieses Mal wird der Fehler auf der Seite angezeigt.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Sie benötigen Berechtigungen auch schreiben, auf die *App\_Daten* Ordner daran, dass Sie SQL Server Compact-Datenbankdateien in diesem Ordner verfügen und Sie zum Aktualisieren von Daten in diesen Datenbanken können möchten. In diesem Fall Sie haben jedoch keinen zusätzlichen nichts zu tun, da des Bereitstellungsprozesses automatisch über die Schreibberechtigung für setzt die *App\_Daten* Ordner.

Sie haben jetzt alle Aufgaben müssen Sie Contoso University abgeschlossen in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. In den nächsten Lernprogrammen werden Sie die Website öffentlich verfügbar machen, indem sie in einem Hostinganbieter bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel wurde der Grund, warum Elmah Log-Dateien können nicht gespeichert wurde, relativ offensichtlich. Sie können IIS-Protokollierung in Fällen verwenden, wo die Ursache des Problems nicht so offensichtlich ist. finden Sie unter [Troubleshooting Failed Anforderungen Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der Website IIS.net.

Weitere Informationen über das Gewähren von Berechtigungen für die Identitäten des dienstanwendungspools finden Sie unter [Anwendungspoolidentitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sicherer Inhalt in IIS über Dateisystem-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der Website IIS.net.

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[Weiter](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
