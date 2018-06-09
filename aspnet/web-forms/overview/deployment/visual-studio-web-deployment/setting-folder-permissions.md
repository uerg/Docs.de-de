---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Festlegen von Ordnerberechtigungen | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 7efe267975835e889950983126088f1b637c28fb
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "30890397"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Festlegen von Ordnerberechtigungen
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm legen Sie Berechtigungen für die *Elmah* Ordner in der bereitgestellten Web site, damit die Anwendung Protokolldateien in diesem Ordner erstellen kann.

Wenn Sie eine Webanwendung in Visual Studio mithilfe der Visual Studio Development Server (Cassini) oder IIS Express zu testen, führt die Anwendung unter der Identität. Sie sind sehr wahrscheinlich ein Administrator auf dem Entwicklungscomputer und vollständige dann berechtigt, alle Dateien in einem beliebigen Ordner Schritte ausführen. Wird jedoch eine Anwendung unter IIS ausgeführt wird, ausgeführt unter der Identität, die definiert, die für den Anwendungspool aus, dem der Standort zugewiesen ist. Dies ist normalerweise eine systemdefinierte-Konto, das über beschränkte Berechtigungen verfügt. Standardmäßig verfügt über Lese- und Ausführungsberechtigungen für Ihre Webanwendung Dateien und Ordner, jedoch keinen Schreibzugriff auf.

Dies ist ein Problem, wenn die Anwendung erstellt oder Updates-Dateien, die ist eine häufige in Webanwendungen müssen. In der Anwendung Contoso University Elmah erstellt XML-Dateien in den *Elmah* Ordner, um Details zu Fehlern zu speichern. Auch wenn Sie etwa Elmah nicht verwenden, kann Ihre Website ermöglichen es Benutzern Hochladen von Dateien oder Durchführen weiterer Aufgaben, die Daten in einen Ordner auf Ihrer Website zu schreiben.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test-fehlerprotokollierung und berichterstellung

Um festzustellen, wie die Anwendung in IIS nicht richtig funktioniert (obwohl dies der Fall, wenn Sie ihn in Visual Studio getestet), Sie können dazu führen, dass einen Fehler, der normalerweise von Elmah protokolliert werden würde, und öffnen Sie das Elmah-Fehlerprotokoll, um die Details anzuzeigen. Wenn Elmah konnte nicht zum Erstellen einer XML-Datei und speichern die Fehlerdetails, sehen Sie eine leere Fehlerbericht anzuzeigen.

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, und fordern dann eine ungültige URL wie *Studentsxxx.aspx*. Daraufhin eine vom System generierte Fehlerseite statt der *GenericErrorPage.aspx* Seite, da die `customErrors` Einstellung in der Datei "Web.config" ist "RemoteOnly", und Sie IIS lokal ausgeführt werden:

![HTTP 404-Fehler (Seite)](setting-folder-permissions/_static/image1.png)

Führen Sie nun *Elmah.axd* den Fehlerbericht anzeigen. Nachdem Sie sich mit den Anmeldeinformationen des Administratorkontos anzumelden (&quot;Admin&quot; und &quot;Devpwd&quot;), eine leere Protokoll Fehlerseite wird angezeigt, da Elmah konnte nicht zum Erstellen einer XML-Datei in die *Elmah*Ordner:

![Fehler beim Protokoll leer](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Legen Sie über die Schreibberechtigung für den Elmah-Ordner

Sie können Berechtigungen für Ordner manuell festlegen oder Sie können es automatisch im Rahmen des Bereitstellungsprozesses vornehmen. Somit automatische komplexen MSBuild-Code erfordert, und da Sie nur dazu erstmalig, die Sie bereitstellen müssen, die folgenden Schritte aus wie Sie dies manuell durchführen. (Informationen dazu, wie in diesem Teil des Bereitstellungsprozesses vornehmen, finden Sie unter [Einstellung Ordnerberechtigungen auf Web Publish](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) Sayed Hashimi-Blog.)

1. In **Datei-Explorer**, navigieren Sie zu *C:\inetpub\wwwroot\ContosoUniversity*. Mit der rechten Maustaste die *Elmah* Ordner wählen **Eigenschaften**, und wählen Sie dann die **Sicherheit** Registerkarte.
2. Klicken Sie auf **Bearbeiten**.
3. In der **Berechtigungen für Elmah** wählen Sie im Dialogfeld **DefaultAppPool**, und wählen Sie dann die **schreiben** Kontrollkästchen in der **zulassen** Spalte.

    ![Berechtigungen für Ordner ELMAH](setting-folder-permissions/_static/image3.png)

    (Wenn Sie nicht sehen **DefaultAppPool** in der **Gruppen-oder Benutzernamen** Liste Sie wahrscheinlich verwendet eine andere Methode als die in diesem Lernprogramm, um IIS und ASP.NET 4 auf Ihrem Computer einzurichten. In diesem Fall feststellen Sie, welche Identität vom University Contoso-Anwendung, und über die Schreibberechtigung erteilen dieser Identität zugewiesen, Anwendungspool verwendet wird. Finden Sie unter den Links zur Anwendungspoolidentitäten am Ende dieses Lernprogramms.) Klicken Sie auf **OK** in beiden Dialogfeldern.

## <a name="retest-error-logging-and-reporting"></a>Testen der fehlerprotokollierung und berichterstellung

Testen, indem Sie verursacht einen Fehler erneut auf die gleiche Weise (eine ungültige URL-Anforderung), und führen Sie die **Fehlerprotokoll** Seite. Dieses Mal wird der Fehler auf der Seite angezeigt.

![ELMAH Protokoll Fehlerseite](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle Aufgaben müssen Sie Contoso University abgeschlossen in IIS auf dem lokalen Computer ordnungsgemäß funktioniert. In den nächsten Lernprogrammen werden Sie die Website öffentlich verfügbar machen, indem sie in Azure bereitstellen.

## <a name="more-information"></a>Weitere Informationen

In diesem Beispiel wurde der Grund, warum Elmah Log-Dateien können nicht gespeichert wurde, relativ offensichtlich. Sie können IIS-Protokollierung in Fällen verwenden, wo die Ursache des Problems nicht so offensichtlich ist. finden Sie unter [Troubleshooting Failed Anforderungen Using Tracing in IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) auf der Website IIS.net.

Weitere Informationen über das Gewähren von Berechtigungen für die Identitäten des dienstanwendungspools finden Sie unter [Anwendungspoolidentitäten](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) und [sicherer Inhalt in IIS über Dateisystem-ACLs](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) auf der Website IIS.net.

> [!div class="step-by-step"]
> [Zurück](deploying-to-iis.md)
> [Weiter](deploying-to-production.md)
