---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure-Authentifizierung | Microsoft-Dokumentation
author: Rick-Anderson
description: Microsoft ASP.NET-Tools für Windows Azure Active Directory können sie ganz einfach Aktivieren der Authentifizierung für Webanwendungen, die auf Windows Azure-Websites gehosteten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: d46a673b75eba0e058c7e20b12f44caf4e2f4f50
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379792"
---
<a name="windows-azure-authentication"></a>Windows Azure-Authentifizierung
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET-tools für Windows Azure Active Directory-Authentifizierung für auf gehostete Webanwendungen aktivieren problemlos [Windows Azure-Websites](https://www.windowsazure.com/home/features/web-sites/). Sie können Windows Azure-Authentifizierung verwenden, um Office 365-Benutzer aus Ihrer Organisation, Unternehmenskonten, die von Ihrem lokalen Active Directory synchronisiert oder in einer eigenen benutzerdefinierten Windows Azure Active Directory-Domäne erstellten Benutzer zu authentifizieren. Aktivieren der Windows Azure-Authentifizierung konfiguriert die Anwendung zum Authentifizieren von Benutzern, die mit einem einzigen [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Mandanten.
> 
> Die ASP.NET-Authentifizierung für Windows Azure-Tool wird nicht für Webrollen in einem Cloud-Dienst unterstützt, aber wir dazu in einem zukünftigen Release möchten. [Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) in Windows Azure-Webrollen unterstützt wird.
> 
> Ausführliche Informationen zum Einrichten der Synchronisierung zwischen Ihrem lokalen Active Directory und Ihrem Windows Azure Active Directory-Mandanten finden Sie [Verwenden von AD FS 2.0 zur Implementierung und Verwaltung der einmaligen Anmeldung](https://technet.microsoft.com/library/jj205462.aspx).
> 
> Windows Azure Active Directory steht derzeit als eine [Vorschau kostenlos](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Anforderungen:

- Visual Studio 2012 oder [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)
- [Erweiterungen für Visual Studio 2012 für Webtools](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) oder [Erweiterungen für Webtools für Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Microsoft ASP.NET-Tools für Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) oder [Microsoft ASP.NET-Tools für Windows Azure Active Directory – Visual Studio Express 2012 für Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Erstellen einer ASP.NET Web-Anwendung mit Visual Studio 2012

Sie können jede Webanwendung mit Visual Studio 2012 erstellen, in diesem Tutorial verwendet die ASP.NET MVC-Intranet-Vorlage.

1. Erstellen Sie eine neue ASP.NET MVC 4-Intranetanwendung ein, und übernehmen Sie alle Standardeinstellungen. (Es muss In **Tra** net und nicht im **ter** net-Projekt).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Aktivieren von Windows Azure-Authentifizierung (Wenn Sie ein globaler Administrator der Grundsatz sind)

Wenn Sie nicht über einen vorhandenen Windows Azure Active Directory-Mandanten (z. B. über ein vorhandenes Office 365-Konto) verfügen können Sie einen neuen Mandanten erstellen, sich für eine [neues Windows Azure Active Directory-Konto](http://g.microsoftonline.com/0AX00en/5).

1. Wählen Sie aus dem Menü "Projekt" **Windows Azure-Authentifizierung aktivieren**:  
  
   ![](windows-azure-authentication/_static/image2.png)

2. Geben Sie die Domäne für Ihren Windows Azure Active Directory-Mandanten (z.B. "contoso.onmicrosoft.com"), und klicken Sie auf **aktivieren**:

![](windows-azure-authentication/_static/image3.png)

3. In der Webauthentifizierung Dialogfeld melden Sie sich als Administrator für Ihren Windows Azure Active Directory-Mandanten:  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Aktivieren Sie Windows Azure, indem Sie ein nicht-Administrator von der Grundsatz

Wenn Sie nicht über globale Administratorberechtigungen für Ihren Windows Azure Active Directory-Mandanten verfügen, können Sie das Kontrollkästchen für die Bereitstellung der Anwendung deaktivieren.

![](windows-azure-authentication/_static/image6.png)

Das Dialogfeld zeigt die **Domäne**, **Anwendungsprinzipal-Id** und **Antwort-URL** die für die Anwendung mit einer Azure Active Directory-Bereitstellung erforderlich sind Grundsatz. Sie müssen diese Informationen an eine Person weitergeben können, die über ausreichende Berechtigungen zum Bereitstellen der Anwendungs hat. Finden Sie unter[wie einmaliges Anmelden mit Windows Azure Active Directory - ASP.NET-Anwendung implementiert](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) ausführliche Informationen zum Cmdlet verwenden, um den Dienstprinzipal manuell zu erstellen.  
Nachdem die Anwendung erfolgreich bereitgestellt wurde, klicken Sie auf **weiterhin "Web.config" mit den ausgewählten Einstellungen aktualisieren**. Wenn Sie den Vorgang fortsetzen, Entwickeln der Anwendung erfolgen kann, klicken Sie auf Bereitstellung warten **speichern schließen, um die Einstellungen in der Projektdatei**. Das nächste Mal aufrufen von Windows Azure-Authentifizierung aktivieren und deaktivieren Sie das Kontrollkästchen bereitstellende, wird Ihnen die gleichen Einstellungen und klicken Sie auf **Weiter**, klicken Sie dann auf, **gelten diese Einstellungen in "Web.config"**.

1. Warten Sie, während Ihre Anwendung für Windows Azure-Authentifizierung konfiguriert und mit Windows Azure Active Directory bereitgestellt.
2. Nachdem Windows Azure-Authentifizierung für Ihre Anwendung aktiviert wurde, klicken Sie auf **schließen:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Drücken Sie F5, um die Anwendung auszuführen. Sie sollten automatisch zur Anmeldeseite umgeleitet. Verwenden Sie die Anmeldeinformationen für Directory Grundsatz, um die Anmeldung bei der Anwendung...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Da Ihre Anwendung zurzeit ein selbstsigniertes Testzertifikat verwendet erhalten eine Warnung Sie von den Browser, die das Zertifikat nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde.

    Diese Warnung kann ignoriert werden während der lokalen Entwicklung durch Klicken auf **Laden dieser Website fortsetzen:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Sie haben nun erfolgreich Ihre Anwendung mit Windows Azure-Authentifizierung angemeldet!

    ![](windows-azure-authentication/_static/image2.jpg)

Aktivieren von Windows Azure werden Authentifizierung die folgenden Änderungen an Ihrer Anwendung:

- Eine Anti siteübergreifenden Request Forgery ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)))-Klasse ( *App\_Start\AntiXsrfConfig.cs* ) wird dem Projekt hinzugefügt.
- Die NuGet-Pakete `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` zu Ihrem Projekt hinzugefügt wird.
- Windows Identity Foundation-Einstellungen in Ihrer Anwendung werden für das Akzeptieren von Sicherheitstoken aus Ihrem Windows Azure Active Directory-Mandanten konfiguriert werden. Klicken Sie auf das Bild, um zu sehen, eine erweiterte Ansicht die Änderungen an der *"Web.config"* Datei.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Es wird ein Dienstprinzipal für Ihre Anwendung in Ihrem Windows Azure Active Directory-Mandanten bereitgestellt werden.
- HTTPS ist aktiviert.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

Vollständige Anweisungen dazu finden Sie unter [Bereitstellen einer ASP.NET-Webanwendung auf einem Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

Eine Anwendung, die mit Windows Azure-Authentifizierung auf einer Azure-Website zu veröffentlichen:

1. Klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie **veröffentlichen:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Herunterladen Sie und importieren Sie ein Veröffentlichungsprofil für Ihr Azure-Website, über das Dialogfeld "Web veröffentlichen".

    ![](windows-azure-authentication/_static/image4.jpg)
3. Die **Verbindung** Registerkarte zeigt die **Ziel-URL** (die öffentlich zugängliche URL für Ihre Anwendung). Klicken Sie auf **Verbindung überprüfen** auf Ihre Verbindung zu testen:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Wenn Sie auf diese Azure-Website veröffentlicht haben zuvor sollten Sie überprüfen die **entfernen weiterer Dateien am Ziel** Einstellung aus, um sicherzustellen, dass Ihre Anwendung ordnungsgemäß veröffentlicht. Beachten Sie, dass die **Windows Azure-Authentifizierung aktivieren** ist das Kontrollkästchen Ausgewählte Aufträge.  

    ![](windows-azure-authentication/_static/image10.png)
5. Optional: Klicken Sie auf die **Vorschau** Registerkarte auf **Vorschau starten** bereitgestellten die Dateien angezeigt.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Klicken Sie auf **veröffentlichen.**

    Sie werden aufgefordert, die zum Aktivieren von Windows Azure-Authentifizierung für den Zielhost. Klicken Sie auf **aktivieren** um den Vorgang fortzusetzen:

    ![](windows-azure-authentication/_static/image11.png)
7. Geben Sie Ihre Administratoranmeldeinformationen für Ihren Windows Azure Active Directory-Mandanten:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Sobald Ihre Anwendung wurde erfolgreich veröffentlicht wurde, wird ein Browserfenster mit der veröffentlichten Website geöffnet.

    > [!NOTE]
    > Es dauert bis zu fünf Minuten für Ihre Anwendung vollständig mit Windows Azure Active Directory bereitgestellt werden, nach der Aktivierung von Windows Azure-Authentifizierung für den Zielhost (in der Regel müssen weniger). Beim ersten Ausführen der Anwendung Wenn ACS50001 Fehlermeldung: vertrauende Seite mit dem Namen "[Realm]" wurde nicht gefunden, warten Sie einige Minuten, und wiederholen Sie die Anwendung erneut ausführen.
9. Wenn Sie aufgefordert werden, melden Sie sich als Benutzer in Ihrem Verzeichnis:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Sie haben nun erfolgreich angemeldet von Azure-gehosteten Anwendung mithilfe von Windows Azure-Authentifizierung.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Bekannte Probleme

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Rollenbasierte Autorisierung schlägt fehl, wenn Windows Azure-Authentifizierung < O:p >< / O:p >

Windows Azure-Authentifizierung stellt keine derzeit der erforderlichen Rollenanspruch bereit, sodass rollenbasierte Autorisierung ausgeführt werden kann. Die Rolle des authentifizierten Benutzers manuell vom Windows Azure Active Directory. < O:p >< abgerufen werden muss / O:p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Navigieren zu einer Anwendung mit Windows Azure-Authentifizierung wird die Fehlermeldung "die Domäne des angemeldeten Benutzers (live.com) keine entspricht ACS20016 zulässig Domäne dieses STS" < O:p >< / O:p >

Wenn Sie bereits bei einem Microsoft Account (z. B. hotmail.com, live.com, outlook.com) angemeldet sind und Sie versuchen, eine Anwendung zuzugreifen, die Windows Azure-Authentifizierung aktiviert ist können Sie eine Antwort 400-Fehler erhalten, da die Domäne Ihrem Microsoft Account von Windows Azure Active Directory wird nicht erkannt werden. Um bei der Anwendung zu protokollieren, melden Sie sich über Ihr Microsoft Account zuerst. < O:p >< / O:p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Anders als in einer Anwendung mit Windows Azure-Authentifizierung aktiviert und eine X509CertificateValidationMode Protokollierung keine führt Zertifikatüberprüfungsfehler für das Zertifikat accounts.accesscontrol.windows.net < O:p >< / O:p >

Überprüfung des Zertifikats ist nicht erforderlich und sollte deaktiviert bleiben. Der Fingerabdruck des Zertifikats des Ausstellers wird überprüft, von der "wsfederationauthenticationmodule" < O:p >< / O:p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Beim Aktivieren der Windows Azure-Authentifizierung zeigt das Dialogfeld "Web Authentication" den Fehler "ACS20016: die Domäne des angemeldeten Benutzers (" contoso.onmicrosoft.com ") stimmt nicht mit keiner zulässigen Domäne dieses STS überein." < O:p >< / O:p >

Dieser Fehler kann angezeigt werden, wenn Sie zuvor erfolgreich in mit einem anderen Windows Azure Active Directory-Konto innerhalb des gleichen Prozesses des Visual Studio angemeldet haben. Melden Sie sich ab, aus dem angegebenen Konto aus, oder starten Sie Visual Studio neu. Wenn Sie bereits angemeldet und die Option "Keep me angemeldet" ausgewählt, dann Sie möglicherweise Ihren Browser Cookies. < O:p >< löschen müssen / O:p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Die Anforderung ist keine gültige WS-Verbund-Protokollnachricht < O:p >< / O:p >

Dies kann vorkommen, wenn Sie bereits mit einigen anderen Microsoft-ID mit einem der Azure-Dienste angemeldet sind. Verwenden von benutzerdefinierten Browser-Fenster wie InPrivate in Internet Explorer oder Chrome-Inkognito oder deaktivieren Sie alle Cookies. <o:p></o:p>

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Microsoft ASP.NET-Tools für Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure-Features: Identität](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/library/hh967619.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für Ihre Organisation](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für mehrere Organisationen](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Wie Sie einmaliges Anmelden mit Windows Azure Active Directory implementieren.](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Einmaliges Anmelden mit Windows Azure Active Directory: ein tieferer Einblick in](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Verwenden von AD FS 2.0 zu implementieren und Verwalten von einmaliges Anmelden](https://technet.microsoft.com/library/jj205462.aspx)
