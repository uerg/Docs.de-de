---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: "Hinzufügen von ASP.NET Identity zu einem leeren oder vorhandene Web Forms-Projekt | Microsoft Docs"
author: raquelsa
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET-Anwendung ASP.NET Identity (das neue Mitgliedschaftssystem für ASP.NET) hinzu. Wenn Sie eine neue Web Forms oder MVC erstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/23/2013
ms.topic: article
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: f5783287a26174ddf65bb0eae34c347831d02c47
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Hinzufügen von ASP.NET Identity zu einem leeren oder vorhandene Web Forms-Projekt
====================
durch [Raquel Soares De Almeida](https://github.com/raquelsa)

> Dieses Lernprogramm veranschaulicht das Hinzufügen [ASP.NET Identity](introduction-to-aspnet-identity.md) (das neue Mitgliedschaftssystem für ASP.NET) an eine ASP.NET-Anwendung.
> 
> Wenn Sie ein neues Web Forms- oder MVC-Projekt in Visual Studio-2013 RTM mit einzelnen Konten erstellen, wird Visual Studio alle erforderlichen Pakete installieren und fügen Sie alle erforderlichen Klassen für Sie. In diesem Lernprogramm werden die Schritte zum Hinzufügen von ASP.NET Identity unterstützen das vorhandene Web Forms-Projekt oder ein neues leeres Projekt veranschaulichen. Ich beschreibt alle NuGet-Pakete, die Sie installieren müssen, und Klassen, mit denen, die Sie hinzufügen müssen. Ich werden Beispiel Web Forms besprochen, für neue Benutzer registrieren und Anmelden bei Hervorhebung alle main Eintrag Point-APIs für Benutzer- und Authentifizierung. Dieses Beispiel verwendet die ASP.NET Identity-Standardimplementierung für die Speicherung von SQL die basiert auf Entity Framework. Dieses Lernprogramm wird LocalDB für die SQL-Datenbank verwendet.
> 
> Dieses Lernprogramm wurde von Raquel Soares De Almeida und Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Erste Schritte ASP.NET-Identität

1. Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Klicken Sie auf **neues Projekt** vom Beginn, oder verwenden Sie das Menü und wählen Sie **Datei**, und klicken Sie dann **neues Projekt**.
3. Wählen Sie **Visual C#-i** n den linken Bereich, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Namen für das Projekt "WebFormsIdentity", und klicken Sie dann auf **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
 Beachten Sie, dass die **Authentifizierung ändern** -Schaltfläche deaktiviert ist und keine Unterstützung der Authentifizierung wird in dieser Vorlage bereitgestellt. Die Web Forms, MVC und Web-API-Vorlagen können Sie die authentifizierungsansatz auswählen. Weitere Informationen finden Sie unter [Übersicht der Authentifizierung](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Hinzufügen von Identity-Paketen zu Ihrer App

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie im Textfeld "Suchen" im Dialogfeld "*Identity.E*". Klicken Sie auf installieren, die für dieses Paket.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Beachten Sie, dass dieses Paket die abhängigkeitspakete installiert wird: EntityFramework und Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Hinzufügen von WebForms Benutzer registrieren

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen**, und klicken Sie dann **Webformular**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. In der **Namen für Artikel angeben** Dialogfeld Feld "Name" neue WebForm **registrieren**, und klicken Sie dann auf **OK**
3. Ersetzen Sie das Markup in der generierten *Register.aspx* -Datei mit den folgenden Code. Die codeänderungen werden hervorgehoben.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Dies ist nur eine vereinfachte Version der *Register.aspx* Datei, die erstellt wird, wenn Sie ein neues ASP.NET Web Forms-Projekt erstellen. Das Markup oben Fügt Formularfelder und eine Schaltfläche, um einen neuen Benutzer registrieren.
4. Öffnen der *Register.aspx.cs* -Datei und Ersetzen Sie den Inhalt der Datei durch den folgenden Code:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Der obige Code ist eine vereinfachte Version der *Register.aspx.cs* -Datei, die erstellt wird, wenn Sie ein neues ASP.NET Web Forms-Projekt erstellen.
    > 2. Die *IdentityUser* Klasse ist die Standardimplementierung von EntityFramework der *IUser* Schnittstelle. *IUser* Schnittstelle ist die minimale Schnittstelle für einen Benutzer auf ASP.NET Identity Core.
    > 3. Die *UserStore* Klasse ist die standardmäßige EntityFramework Implementierung eines Benutzerspeichers. Diese Klasse implementiert die ASP.NET Identity Core minimalen Schnittstellen: *IUserStore*, *IUserLoginStore*, *IUserClaimStore* und *IUserRoleStore* .
    > 4. Die *UserManager* Klasse stellt benutzerbezogene APIs, die automatisch vorgenommenen Änderungen zu speichern, wird die *UserStore*.
    > 5. Die *IdentityResult* Klasse stellt das Ergebnis eines identitätsvorgangs dar.
5. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen**, **ASP.NET-Ordner hinzufügen** und dann **App\_Daten**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Öffnen der *"Web.config"* Datei, und fügen Sie einen Verbindungszeichenfolgeneintrag für die Datenbank, die wir verwenden, um das Speichern von Benutzerinformationen. Die Datenbank wird zur Laufzeit von EntityFramework für die Identity-Entitäten erstellt werden. Die Verbindungszeichenfolge ähnelt einer für Sie erstellt werden, wenn Sie ein neues Web Forms-Projekt erstellen. Der hervorgehobene Code zeigt dem Markup hinzugefügt werden soll:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Ersetzen Sie für Visual Studio 2015 oder höher, `(localdb)\v11.0` mit `(localdb)\MSSQLLocalDB` in der Verbindungszeichenfolge.
    
7. Klicken Sie mit der rechten Maustaste auf die Datei *Register.aspx* in Ihrem Projekt, und wählen **als Startseite festlegen**. Drücken Sie STRG + F5 zu erstellen, und führen Sie die Webanwendung. Geben Sie einen neuen Benutzernamen und ein Kennwort, und klicken Sie dann auf **registrieren**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity bietet Unterstützung für die Validierung und in diesem Beispiel können Sie das Standardverhalten für Benutzer und Kennwort überprüfen Validierungssteuerelemente, die von der Identität Hauptpaket stammen. Das Standard-Validierungssteuerelement für Benutzer (`UserValidator`) verfügt über eine Eigenschaft `AllowOnlyAlphanumericUserNames` , besitzt Standardwert `true`. Der Standard-Validierungssteuerelement für das Kennwort (`MinimumLengthValidator`) wird sichergestellt, dass dieses Kennwort hat mindestens 6 Zeichen enthalten. Diese Validierungssteuerelemente werden Eigenschaften auf `UserManager` , überschrieben werden, wenn Sie benutzerdefinierte Validierung haben möchten.

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Überprüfen der Identität der LocalDb-Datenbank und die Tabellen, die vom Entity Framework generierten

1. In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Erweitern Sie **DefaultConnection (WebFormsIdentity)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **AspNetUsers** , und klicken Sie auf **Tabellendaten anzeigen**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurieren der Anwendung für die OWIN-Authentifizierung

Zu diesem Zeitpunkt haben wir Unterstützung für die Erstellung von Benutzern nur hinzugefügt. Jetzt werden wir zur Veranschaulichung, wie wir die Authentifizierung zur Anmeldung eines Benutzers hinzufügen können. ASP.NET Identity verwendet Microsoft OWIN-authentifizierungsmiddleware für die Formularauthentifizierung. Die OWIN-Cookieauthentifizierung ist ein Cookie und Ansprüche basierend Authentifizierungsmechanismus, der von einem Framework gehostet verwendet werden kann [OWIN](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx) oder IIS. Mit diesem Modell können der gleiche Authentifizierungspaketen für mehrere Frameworks wie ASP.NET MVC und Web Forms verwendet werden. Weitere Informationen zum Projekt Katana und zum Ausführen von Middleware in einen Host agnostisch finden Sie unter [Einstieg in das Katana-Projekt](https://msdn.microsoft.com/en-us/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Installieren von Authentifizierungspaketen für Ihre Anwendung

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie im Textfeld "Suchen" im Dialogfeld "*Identity.Owin*". Klicken Sie auf installieren, die für dieses Paket.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Suchen nach Paket ***"Microsoft.owin.Host.systemweb"*** und installieren Sie es.   

    > [!NOTE]
    > Die **Microsoft.Aspnet.Identity.Owin** Paket enthält eine Reihe von OWIN-Erweiterungsklassen zum Verwalten und Konfigurieren der OWIN-authentifizierungsmiddleware von ASP.NET Identity Core-Pakete verwendet wird.  
    > Die **"Microsoft.owin.Host.systemweb"** Paket enthält einen OWIN-Server, der OWIN-basierte Anwendungen zur Ausführung unter IIS mithilfe der ASP.NET-Pipeline für die Anforderung aktiviert. Weitere Informationen finden Sie unter [OWIN-Middleware in der IIS-integrierten Pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Hinzufügen von OWIN-Start und Authentifizierung Konfigurationsklassen

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, klicken Sie auf **hinzufügen**, und klicken Sie dann **neues Element hinzufügen**. Geben Sie im Textfeld "Suchen" im Dialogfeld "*Owin*". Benennen Sie die Klasse "*Start*", und klicken Sie auf **hinzufügen**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Fügen Sie in der Datei Startup.cs des hervorgehobenen Codes zum Konfigurieren der OWIN-Cookieauthentifizierung unten ein.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Diese Klasse enthält die `OwinStartup` Attribut zum Angeben der OWIN-Start-Klasse. Jede OWIN-Anwendung verfügt über eine Autostart-Klasse, in dem Sie Komponenten für die Pipeline der Anwendung angeben. Finden Sie unter [OWIN Start Klasse Erkennung](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) für Weitere Informationen zu diesem Modell.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Hinzufügen von WebForms für die Registrierung und Anmeldung von Benutzern

1. Öffnen der *Register.cs* Datei, und fügen Sie folgenden Code, der der Benutzer bei der Registrierung ist erfolgreich verläuft. Im folgenden werden die Änderungen hervorgehoben.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Da ASP.NET Identity und OWIN Cookieauthentifizierung anspruchsbasierte System sind, erfordert das Framework den app-Entwickler zum Generieren einer ["ClaimsIdentity"](https://msdn.microsoft.com/en-us/library/microsoft.identitymodel.claims.claimsidentity.aspx) für den Benutzer. "ClaimsIdentity" hat Informationen über alle Ansprüche für den Benutzer wie z. B. welche Rollen, die der Benutzer angehört. Sie können auch weitere Ansprüche für den Benutzer zu diesem Zeitpunkt hinzufügen.
    > - Sie können die Benutzer anmelden, mithilfe der AuthenticationManager aus aufrufen und OWIN `SignIn` und in der "ClaimsIdentity" übergeben werden, wie oben gezeigt. Dieser Code wird der Benutzer anmelden und generieren ebenfalls einen Cookie. Dieser Aufruf ist analog zu [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx) verwendet werden, indem Sie die [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) Modul.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste in Ihrem Projekt auf **hinzufügen**, und klicken Sie dann **Webformular**. Nennen Sie das Web Form **Anmeldung**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Ersetzen Sie den Inhalt von der *"Login.aspx"* Datei durch den folgenden Code:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Ersetzen Sie den Inhalt von der *Login.aspx.cs* Datei durch Folgendes:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Die `Page_Load` nun den Status des aktuellen Benutzers gesucht und führt die Aktionen, die basierend auf seiner `Context.User.Identity.IsAuthenticated` Status.  
    >     **Protokolliert in Benutzername angezeigt** : die Microsoft ASP.NET Identity Framework fügte Erweiterungsmethoden auf [System.Security.Principal.IIdentity](https://msdn.microsoft.com/en-us/library/system.security.principal.iidentity.aspx) , mit der Sie zum Abrufen der `UserName` und `UserId` für die angemeldeter Benutzer Diese Erweiterungsmethoden werden definiert, der `Microsoft.AspNet.Identity.Core` Assembly. Diese Erweiterungsmethoden finden sich die Ersetzung für [HttpContext.User.Identity.Name](https://msdn.microsoft.com/en-us/library/system.web.httpcontext.user.aspx) .
    > - SignIn-Methode:   
    >     `This`-Methode ersetzt die vorherige `CreateUser_Click` Methode in diesem Beispiel und nun meldet sich der Benutzer nach der Benutzer erfolgreich erstellt.   
    >  Microsoft owin-Framework fügte Erweiterungsmethoden auf `System.Web.HttpContext` , mit der Sie zum Abrufen eines Verweises auf ein `IOwinContext`. Diese Erweiterungsmethoden werden in definiert `Microsoft.Owin.Host.SystemWeb` Assembly. Die `OwinContext` Klasse macht ein `IAuthenticationManager` -Eigenschaft, die authentifizierungsmiddlewarefunktionen, die für die aktuelle Anforderung verfügbar darstellt.  
    >  Sie können sich Benutzer anmelden, mit der `AuthenticationManager` aus aufrufen und OWIN `SignIn` und übergibt die `ClaimsIdentity` wie oben gezeigt.   
    >  Da ASP.NET Identity und OWIN Cookieauthentifizierung anspruchsbasiertes System befinden, wird das Framework benötigt die app zum Generieren einer `ClaimsIdentity` für den Benutzer.   
    >  Die `ClaimsIdentity` enthält Informationen über alle Ansprüche für den Benutzer, wie z. B. welche Rollen, die der Benutzer angehört. Sie können auch weitere Ansprüche für den Benutzer zu diesem Zeitpunkt hinzufügen.  
    >  Dieser Code wird der Benutzer anmelden und generieren ebenfalls einen Cookie. Dieser Aufruf ist analog zu [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.setauthcookie.aspx) verwendet werden, indem Sie die [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) Modul.
    > - `SignOut`Methode:   
    >  Ruft einen Verweis auf die `AuthenticationManager` aus OWIN und Aufrufe `SignOut`. Dies ist analog zu [FormsAuthentication.SignOut](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthentication.signout.aspx) Methode, mit der die [FormsAuthentication](https://msdn.microsoft.com/en-us/library/system.web.security.formsauthenticationmodule.aspx) Modul.
5. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung. Geben Sie einen neuen Benutzernamen und ein Kennwort, und klicken Sie dann auf **registrieren**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
 Hinweis: an diesem Punkt der neue Benutzer erstellt und angemeldet.
6. Klicken Sie auf **Abmelden** Schaltfläche. Sie werden in das Protokoll im Formular umgeleitet werden.
7. Geben Sie ein ungültiger Benutzername oder Kennwort und klicken Sie auf **melden Sie sich** Schaltfläche.   
 Die `UserManager.Find` Methode Null und die Fehlermeldung zurück: " *Ungültiger Benutzername oder Kennwort* " wird angezeigt.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
