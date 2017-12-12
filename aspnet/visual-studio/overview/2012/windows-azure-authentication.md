---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure-Authentifizierung | Microsoft Docs
author: Rick-Anderson
description: "Microsoft ASP.NET-Tools für Windows Azure Active Directory vereinfacht die Authentifizierung für Webanwendungen, die auf Windows Azure-Websites gehostet aktivieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1cb38d66bd0373159e54abf822fba9c5829774ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="windows-azure-authentication"></a>Windows Azure-Authentifizierung
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Tools für Microsoft ASP.NET für Windows Azure Active Directory-Authentifizierung für auf gehostete Webanwendungen aktivieren problemlos [Windows Azure-Websites](https://www.windowsazure.com/en-us/home/features/web-sites/). Sie können Windows Azure-Authentifizierung verwenden, um Office 365-Benutzer aus Ihrer Organisation, Unternehmenskonten aus Ihrem lokalen Active Directory synchronisiert oder in Ihre eigene benutzerdefinierte Windows Azure Active Directory-Domäne erstellten Benutzer zu authentifizieren. Aktivieren von Windows Azure-Authentifizierung konfiguriert Ihre Anwendung zum Authentifizieren von Benutzern, die mit einem einzigen [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Mandanten.
> 
> Die ASP.NET-Authentifizierung für Windows Azure-Tool wird nicht für Webrollen in einem Clouddienst unterstützt, aber wir planen die in einer zukünftigen Version dazu. [Windows Identity Foundation](https://msdn.microsoft.com/en-us/library/hh291066(v=VS.110).aspx) (WIF) wird in Windows Azure-Webrollen unterstützt.
> 
> Ausführliche Schritte zur Einrichtung der Synchronisierung zwischen Ihrem lokalen Active Directory und Windows Azure Active Directory-Mandanten finden Sie unter [Verwenden von AD FS 2.0 zur Implementierung und Verwaltung des einmaligen Anmeldens](https://technet.microsoft.com/en-us/library/jj205462.aspx).
> 
> Windows Azure Active Directory steht derzeit als eine [frei vorschaudienst](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).


## <a name="requirements"></a>Anforderungen:

- Visual Studio 2012 oder [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express)
- [Web-Tools-Erweiterungen für Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) oder [Web Tools-Erweiterungen für Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)
- [Tools für Microsoft ASP.NET für Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) oder [Tools für Microsoft ASP.NET für Windows Azure Active Directory – Visual Studio Express 2012 für Web](https://go.microsoft.com/fwlink/?LinkId=282652)

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a>Erstellen einer ASP.NET Web-Anwendung mit Visual Studio 2012

Sie können jede Web-Anwendung mit Visual Studio 2012 erstellen, dieses Lernprogramm verwendet die ASP.NET MVC-Projektvorlage Intranet.

1. Erstellen Sie ein neues ASP.NET MVC 4-Intranetanwendung ein, und übernehmen Sie alle Standardeinstellungen. (Muss er In **Verknü** net und nicht im **eben** net-Projekt).  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a>Aktivieren Sie Windows Azure-Authentifizierung (Wenn Sie ein Globaladministrator von der Grundsatz sind)

Wenn Sie nicht mit einen vorhandenen Windows Azure Active Directory-Mandanten (z. B. über ein vorhandenes Office 365-Konto) verfügen können Sie einen neuen Mandanten erstellen, indem die Anmeldung bei einem [neues Windows Azure Active Directory-Konto](http://g.microsoftonline.com/0AX00en/5).

1. Wählen Sie im Menü Projekt **Windows Azure-Authentifizierung aktivieren**:  
  
 ![](windows-azure-authentication/_static/image2.png)

2. Geben Sie die Domäne für Ihren Windows Azure Active Directory-Mandanten (z. B. "contoso.onmicrosoft.com"), und klicken Sie auf **aktivieren**:

![](windows-azure-authentication/_static/image3.png)

3. In der Webauthentifizierung Dialogfeld Anmeldung als Administrator für Ihren Windows Azure Active Directory-Mandanten:  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a>Aktivieren Sie Windows Azure, indem Sie ohne Administratorrechte für den Grundsatz

Wenn Sie nicht über globale Administratorrechte für Windows Azure Active Directory-Mandanten verfügen, können Sie das Kontrollkästchen für die Bereitstellung der Anwendung deaktivieren.

![](windows-azure-authentication/_static/image6.png)

Das Dialogfeld zeigt die **Domäne**, **Anwendungsprinzipal-Id** und **-Antwort-URL** die für die Anwendung mit einem Azure Active Directory-Bereitstellung erforderlich sind Grundsatz. Sie müssen diese Informationen an eine Person mitteilen, ausreichende Berechtigungen zum Bereitstellen der Anwendung hat. Finden Sie unter[wie einmaliges Anmelden mit Windows Azure Active Directory - ASP.NET-Anwendung implementiert](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) Weitere Informationen zum Cmdlet verwenden, um den Dienstprinzipal manuell erstellen.  
Sobald die Anwendung erfolgreich bereitgestellt wurde, klicken Sie auf **weiterhin "Web.config" mit den ausgewählten Einstellungen aktualisieren**. Wenn Sie den Vorgang fortsetzen, beim Entwickeln der Anwendung durchgeführt werden soll, klicken Sie auf Bereitstellung warten **speichern schließen, um die Einstellungen in der Projektdatei**. Das nächste Mal aufrufen von Windows Azure-Authentifizierung aktivieren und deaktivieren Sie das Kontrollkästchen provisioning, sehen Sie die gleichen Einstellungen und klicken Sie auf **Fortfahren**, klicken Sie dann auf **wenden diese Einstellungen in "Web.config"**.

1. Warten Sie, während die Anwendung für Windows Azure-Authentifizierung konfiguriert ist und mit Windows Azure Active Directory bereitgestellt wurde.
2. Nachdem die Windows Azure-Authentifizierung für Ihre Anwendung aktiviert wurde, klicken Sie auf **schließen:** 

    ![](windows-azure-authentication/_static/image7.png)
3. Drücken Sie F5, um die Anwendung ausgeführt werden. Sie sollten automatisch an die Anmeldeseite umgeleitet abrufen. Verwenden Sie Verzeichnis Grundsatz Benutzeranmeldeinformationen, die Anwendung anzumelden...  

    ![](windows-azure-authentication/_static/image1.jpg)
4. Da Ihre Anwendung derzeit ein selbstsigniertes Testzertifikat verwendet erhalten Sie eine Warnung über den Browser, die das Zertifikat nicht von einer vertrauenswürdigen Zertifizierungsstelle ausgestellt wurde.

    Diese Warnung kann ignoriert werden während der lokalen Entwicklung durch Klicken auf **Laden dieser Website fortsetzen:** 

    ![](windows-azure-authentication/_static/image8.png)
5. Sie haben jetzt erfolgreich auf Ihre Anwendung mit Windows Azure-Authentifizierung angemeldet!

    ![](windows-azure-authentication/_static/image2.jpg)

Aktivieren von Windows Azure werden Authentifizierung die folgenden Änderungen an der Anwendung vorgenommen:

- Ein Anti siteübergreifenden Anforderungsfälschungen ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)))-Klasse ( *App\_Start\AntiXsrfConfig.cs* ) wird dem Projekt hinzugefügt.
- Die NuGet-Pakete `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` wird dem Projekt hinzugefügt.
- Windows Identity Foundation-Einstellungen in der Anwendung werden für das Akzeptieren von Sicherheitstoken aus Ihrem Windows Azure Active Directory-Mandanten konfiguriert werden. Klicken Sie auf das Bild unten, um eine erweiterte Ansicht dieses die geänderte finden Sie unter der *"Web.config"* Datei.  
  
     ![](windows-azure-authentication/_static/image9.png)
- Einem Dienstprinzipal für Ihre Anwendung in Windows Azure Active Directory-Mandanten wird bereitgestellt werden.
- HTTPS ist aktiviert.

## <a name="deploy-the-application-to-windows-azure"></a>Bereitstellen der Anwendung in Windows Azure

Vollständige Anweisungen finden Sie unter [Bereitstellen einer ASP.NET-Webanwendung auf einer Windows Azure-Website](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).

So veröffentlichen Sie eine Anwendung mit Windows Azure-Authentifizierung auf ein Azure-Website

1. Klicken Sie mit der rechten Maustaste auf die Anwendung, und wählen Sie **veröffentlichen:** 

    ![](windows-azure-authentication/_static/image3.jpg)
2. Das Dialogfeld "Web veröffentlichen" herunter, und importieren Sie ein Veröffentlichungsprofil für Ihr Azure-Website.

    ![](windows-azure-authentication/_static/image4.jpg)
3. Die **Verbindung** Registerkarte zeigt die **Ziel-URL** (die öffentliche angezeigte URL für Ihre Anwendung). Klicken Sie auf **Validate Connection** auf Ihre Verbindung zu testen:

    ![](windows-azure-authentication/_static/image5.jpg)
4. Wenn Sie diese Azure-Website veröffentlicht haben zuvor sollten Sie überprüfen die **entfernen weiterer Dateien am Ziel** Einstellung, um sicherzustellen, dass die Anwendung ordnungsgemäß veröffentlicht. Beachten Sie, dass die **Windows Azure-Authentifizierung aktivieren** Kontrollkästchen wird die ausgewählte Aufträge.  

    ![](windows-azure-authentication/_static/image10.png)
5. Optional: Klicken Sie auf die **Vorschau** Registerkarte auf **Vorschau starten** die Dateien bereitgestellt.

    ![](windows-azure-authentication/_static/image6.jpg)
6. Klicken Sie auf **veröffentlichen.**

    Sie werden aufgefordert, die zum Aktivieren von Windows Azure-Authentifizierung für den Zielhost. Klicken Sie auf **aktivieren** um den Vorgang fortzusetzen:

    ![](windows-azure-authentication/_static/image11.png)
7. Geben Sie Ihre Administratoranmeldeinformationen für Windows Azure Active Directory-Mandanten ein:

    ![](windows-azure-authentication/_static/image7.jpg)
8. Sobald die Anwendung erfolgreich veröffentlicht wurde, wird vom Browser auf die veröffentlichten-Website geöffnet.

    > [!NOTE]
    > Dauert möglicherweise bis zu fünf Minuten für Ihre Anwendung vollständig mit Windows Azure Active Directory bereitgestellt werden, nach der Aktivierung von Windows Azure-Authentifizierung für den Zielhost (in der Regel muss weniger). Beim ersten Ausführen der Anwendungsstatus erhalten Sie die Fehlermeldung ACS50001: vertrauende Seite mit dem Namen "[Bereich]" wurde nicht gefunden, warten Sie einige Minuten, und wiederholen Sie dann die Anwendung erneut ausführen.
9. Wenn Sie aufgefordert werden, melden Sie sich als Benutzer in Ihrem Verzeichnis:

    ![](windows-azure-authentication/_static/image8.jpg)
10. Sie haben jetzt erfolgreich angemeldet Ihrer Azure gehostete Anwendung mit Windows Azure-Authentifizierung.  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a>Bekannte Probleme

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a>Rollenbasierte Autorisierung schlägt fehl, wenn es sich bei Windows Azure-Authentifizierung < o: p >< / o: p >

Windows Azure-Authentifizierung stellt keine der erforderlichen Rollenanspruch derzeit bereit, sodass rollenbasierte Autorisierung ausgeführt werden kann. Die Rolle des authentifizierten Benutzers manuell vom Windows Azure Active Directory. < o: p >< abgerufen werden muss / o: p >

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Navigieren zu einer Anwendung mit Windows Azure-Authentifizierung führt zum Fehler "die Domäne des angemeldeten Benutzers (live.com) keine entspricht ACS20016 zugelassen Domäne dieses STS" < o: p >< / o: p >

Wenn Sie bereits einem Microsoft-Account (z. B. hotmail.com, live.com, outlook.com) angemeldet sind und Sie versuchen, eine Anwendung zuzugreifen, die Windows Azure-Authentifizierung aktiviert wurde möglicherweise eine Antwort 400-Fehler abrufen, da die Domäne Ihrer Microsoft-Account wird von Windows Azure Active Directory nicht erkannt. Um die Anwendung zu protokollieren, melden Sie sich über Ihr Microsoft-Account zuerst. < o: p >< / o: p >

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a>Anders als bei einer Anwendung mit Windows Azure-Authentifizierung aktiviert und eine X509CertificateValidationMode anzumelden keine führt zu Zertifikatüberprüfungsfehler für das Zertifikat accounts.accesscontrol.windows.net < o: p >< / o: p >

Die Überprüfung des Zertifikats ist nicht erforderlich und sollte deaktiviert bleiben. Der Fingerabdruck des Zertifikats des Ausstellers wird überprüft, von der "wsfederationauthenticationmodule" < o: p >< / o: p >

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a>Beim Versuch, die Windows Azure-Authentifizierung aktiviert Zeigt das Dialogfeld für die Webauthentifizierung den Fehler "ACS20016: die Domäne des angemeldeten Benutzers (contoso.onmicrosoft.com) stimmt nicht mit keiner zulässigen Domäne dieses STS überein." < o: p >< / o: p >

Dieser Fehler kann angezeigt werden, wenn Sie zuvor erfolgreich in die mit einem anderen Windows Azure Active Directory-Konto aus der gleichen Visual Studio-Prozess angemeldet haben. Melden Sie sich ab, aus dem angegebenen Konto aus, oder starten Sie Visual Studio neu. Wenn Sie zuvor angemeldet und die Option "In angemeldet bleiben" aktiviert sind, müssen Sie möglicherweise Ihren Browser Cookies. < o: p >< deaktivieren / o: p >

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a>ACS20012: Die Anforderung ist keine gültige WS-Verbund-Protokollnachricht < o: p >< / o: p >

Dies kann geschehen, wenn Sie sich mit einigen anderen Microsoft-ID in eines der Azure-Dienste bereits angemeldet sind. Verwenden von benutzerdefinierten Browserfenster wie z. B. in IE InPrivate- oder Inkognito in Chrome oder deaktivieren Sie alle Cookies. < o: p >< / o: p >

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Tools für Microsoft ASP.NET für Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci
- [Windows Azure-Funktionen: Identität](https://docs.microsoft.com/azure/active-directory/)
- [TechNet: Windows Azure Active Directory](https://technet.microsoft.com/en-us/library/hh967619.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für Ihre Organisation](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [Windows Azure Active Directory: Entwickeln von Apps für mehrere Organisationen bereitgestellt werden](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [Gewusst wie: Implementieren Sie einmaliges Anmelden mit Windows Azure Active Directory](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- [Einmaliges Anmelden mit Windows Azure Active Directory: fundierte Einblicke](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci
- [Verwenden von AD FS 2.0 zur Implementierung und Verwaltung des einmaligen Anmeldens](https://technet.microsoft.com/en-us/library/jj205462.aspx)
