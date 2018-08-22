---
uid: identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
title: Hinzufügen von ASP.NET Identity zu einem leeren oder vorhandenen Web Forms-Projekt | Microsoft-Dokumentation
author: raquelsa
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET-Anwendung ASP.NET Identity (das neue Mitgliedschaftssystem für ASP.NET) hinzufügen. Wenn Sie eine neue Web Forms oder MVC erstellen...
ms.author: riande
ms.date: 10/23/2013
ms.assetid: 1cbc0ed2-5bd6-4b62-8d34-4c193dcd8b25
msc.legacyurl: /identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: 229d6fef5aa9c2384b6d92ec3e3ed7316b69afe0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826535"
---
<a name="adding-aspnet-identity-to-an-empty-or-existing-web-forms-project"></a>Hinzufügen von ASP.NET Identity zu einem leeren oder vorhandenen Web Forms-Projekt
====================
durch [Raquel Soares De Almeida](https://github.com/raquelsa)

> In diesem Tutorial erfahren Sie, wie hinzuzufügende [ASP.NET Identity](introduction-to-aspnet-identity.md) (das neue Mitgliedschaftssystem für ASP.NET) an eine ASP.NET-Anwendung.
> 
> Wenn Sie ein neues Web Forms oder MVC-Projekt in Visual Studio-2013 RTM durch individuelle Konten erstellen, wird Visual Studio installieren aller erforderlichen Pakete und fügen Sie alle erforderlichen Klassen für Sie hinzu. In diesem Tutorial werden die Schritte zum Hinzufügen von ASP.NET Identity-Support zu Ihrem vorhandenen Web Forms-Projekt oder ein neues leeres Projekt veranschaulichen. Umrissen werden alle NuGet-Pakete, die Sie installieren möchten, und Klassen, die Sie hinzufügen müssen. Ich werden Beispiel-Web Forms besprochen, für die Registrierung neuer Benutzer, und markieren Sie alle Einstiegspunkt in main Point-APIs für die benutzerverwaltung und-Authentifizierung anmelden. In diesem Beispiel wird die ASP.NET Identity-Standardimplementierung für SQL-Datenspeicher verwenden, basiert auf Entity Framework. Dieses Tutorial werden wir LocalDB für die SQL-Datenbank verwenden.
> 
> In diesem Tutorial wurde von Raquel Soares De Almeida und Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ).


## <a name="getting-started-aspnet-identity"></a>Erste Schritte ASP.NET Identity

1. Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).
2. Klicken Sie auf **neues Projekt** vom Anfang, oder verwenden Sie das Menü und wählen Sie **Datei**, und klicken Sie dann **neues Projekt**.
3. Wählen Sie **Visual C#-i** n im linken Bereich, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Benennen Sie das Projekt "WebFormsIdentity", und klicken Sie dann auf **OK**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image1.png)
4. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image2.png)  
  
   Beachten Sie, dass die **Authentifizierung ändern** -Schaltfläche deaktiviert ist und keine authentifizierungsunterstützung in dieser Vorlage bereitgestellt. Die Web Forms, MVC und Web-API-Vorlagen können Sie die authentifizierungsansatz wählen. Weitere Informationen finden Sie unter [Übersicht über die der Authentifizierung](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth) .

## <a name="adding-identity-packages-to-your-app"></a>Hinzufügen von Identity-Paketen zu Ihrer App

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie Text "Suchen" im Dialogfeld "*Identity.E*". Klicken Sie für dieses Paket installieren.   
  
![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image3.png)  
  
Beachten Sie, dass dieses Paket die abhängigkeitspakete installiert wird: EntityFramework und Microsoft ASP.NET Identity Core.

## <a name="adding-web-forms-to-register-users"></a>Hinzufügen von Webformularen an das Benutzer registrieren

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen**, und klicken Sie dann **Webformular**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image4.png)
2. In der **Namen für Artikel angeben** Dialogfeld Namen des neuen Webformulars **registrieren**, und klicken Sie dann auf **OK**
3. Ersetzen Sie das Markup in der generierten *Register.aspx* -Datei mit den folgenden Code. Die Codeänderungen werden hervorgehoben.   

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample1.aspx?highlight=9,12-40)]

    > [!NOTE]
    > Dies ist nur eine vereinfachte Version der *Register.aspx* -Datei, die erstellt wird, wenn Sie ein neues ASP.NET Web Forms-Projekt erstellen. Das Markup oben Fügt Formularfelder und eine Schaltfläche, um einen neuen Benutzer zu registrieren.
4. Öffnen der *Register.aspx.cs* Datei, und Ersetzen Sie den Inhalt der Datei mit dem folgenden Code:

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample2.cs)]

    > [!NOTE] 
    > 
    > 1. Der obige Code ist eine vereinfachte Version der *Register.aspx.cs* -Datei, die erstellt wird, wenn Sie ein neues ASP.NET Web Forms-Projekt erstellen.
    > 2. Die *"identityuser"* Klasse ist die standardmäßige EntityFramework-Implementierung von der *IUser* Schnittstelle. *IUser* Schnittstelle ist die minimale Schnittstelle für einen Benutzer auf ASP.NET Identity Core.
    > 3. Die *UserStore* Klasse ist die Standardimplementierung von EntityFramework eines Benutzerspeichers. Diese Klasse implementiert die ASP.NET Identity Core minimalen Schnittstellen: *"iuserstore"*, *IUserLoginStore*, *IUserClaimStore* und *IUserRoleStore* .
    > 4. Die *UserManager* Klasse stellt benutzerbezogene APIs, die automatisch Änderungen zum Speichern der *UserStore*.
    > 5. Die *IdentityResult* Klasse stellt das Ergebnis eines identitätsvorgangs dar.
5. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und klicken Sie auf **hinzufügen**, **ASP.NET-Ordner hinzufügen** und dann **App\_Daten**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image5.png)
6. Öffnen der *"Web.config"* -Datei und fügen Sie einen Verbindungszeichenfolgeneintrag für die Datenbank, die wir verwenden, um Benutzerinformationen zu speichern. Die Datenbank wird zur Laufzeit von EntityFramework. für die Identity-Entitäten erstellt werden. Die Verbindungszeichenfolge ähnelt einem für Sie erstellt werden, wenn Sie ein neues Web Forms-Projekt erstellen. Der hervorgehobene Code zeigt das Markup, dass Sie hinzufügen müssen:

    [!code-xml[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample3.xml?highlight=11-14)]
    
    > [!NOTE] 
    > Für Visual Studio 2015 oder höher verwenden, ersetzen Sie dies `(localdb)\v11.0` mit `(localdb)\MSSQLLocalDB` in der Verbindungszeichenfolge.
    
7. Klicken Sie mit der rechten Maustaste auf die Datei *Register.aspx* in Ihr Projekt, und wählen Sie **als Startseite festlegen**. Drücken Sie STRG + F5 zum Erstellen und Ausführen der Webanwendung. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image6.png)  

    > [!NOTE]
    > ASP.NET Identity bietet Unterstützung für die Validierung und in diesem Beispiel können Sie das Standardverhalten für Benutzer und Kennwort überprüfen Validierungssteuerelemente, die von der Identity-Kernpaket stammen. Das Standard-Validierungssteuerelement für den Benutzer (`UserValidator`) verfügt über eine Eigenschaft `AllowOnlyAlphanumericUserNames` aufweist festgelegte Standardwert `true`. Die Standard-Validierungssteuerelement für das Kennwort (`MinimumLengthValidator`) wird sichergestellt, das Kennwort wurde mindestens 6 Zeichen lang sein. Diese Validierungen werden die Eigenschaften auf `UserManager` , kann überschrieben werden, wenn Sie benutzerdefinierte Validierung möchten

## <a name="verifying-the-localdb-identity-database-and-tables-generated-by-entity-framework"></a>Überprüfen der Identität der LocalDb-Datenbank und Tabellen, die von Entitätsframework generierten

1. In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image7.png)
2. Erweitern Sie **DefaultConnection (WebFormsIdentity)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **"aspnetusers"** , und klicken Sie auf **Tabellendaten anzeigen**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image8.png)  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image9.png)

## <a name="configuring-the-application-for-owin-authentication"></a>Konfigurieren der Anwendung für die OWIN-Authentifizierung

An diesem Punkt haben wir nur Unterstützung für das Erstellen von Benutzern hinzugefügt. Jetzt werden wir veranschaulichen, wie wir die Authentifizierung, damit einen Benutzer für die Anmeldung hinzufügen können. ASP.NET Identity verwendet Microsoft OWIN-authentifizierungsmiddleware für die Formularauthentifizierung. Die OWIN Cookie-Authentifizierung ist ein Cookie und Ansprüche basierend Authentifizierungsmechanismus, der von einem beliebigen Framework gehostet wird, auf verwendet werden kann [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) oder IIS. Bei diesem Modell können die gleichen Authentifizierungspakete für mehrere Frameworks einschließlich ASP.NET MVC und Web Forms verwendet werden. Weitere Informationen zu Katana-Projekt und Ausführen von Middleware in einen Host unabhängig finden Sie unter [erste Schritte mit dem Katana-Projekt](https://msdn.microsoft.com/magazine/dn451439.aspx).

## <a name="installing-authentication-packages-to-your-application"></a>Installieren von Authentifizierungspaketen zu Ihrer Anwendung

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie Text "Suchen" im Dialogfeld "*Identity.Owin*". Klicken Sie für dieses Paket installieren.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image10.png)
2. Suchen nach Paket ***"Microsoft.owin.Host.systemweb"*** und installieren Sie es.   

    > [!NOTE]
    > Die **Microsoft.Aspnet.Identity.Owin** Paket enthält eine Reihe von Klassen der datenverarbeitungserweiterung OWIN zum Verwalten und Konfigurieren der OWIN-authentifizierungsmiddleware von ASP.NET Identity-Core-Pakete genutzt werden soll.  
    > Die **"Microsoft.owin.Host.systemweb"** Paket enthält einen OWIN-Server, die OWIN-basierten Anwendungen unter IIS, die unter Verwendung der ASP.NET-Pipeline für die Anforderung ausgeführt werden können. Weitere Informationen finden Sie unter [OWIN-Middleware in der IIS integrierte Pipeline](../../../aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline.md).

## <a name="adding-owin-startup-and-authentication-configuration-classes"></a>Hinzufügen von OWIN-Startup "und" Authentication Configuration-Klassen

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, klicken Sie auf **hinzufügen**, und klicken Sie dann **neues Element hinzufügen**. Geben Sie Text "Suchen" im Dialogfeld "*Owin*". Nennen Sie die Klasse "*Start*", und klicken Sie auf **hinzufügen**.   
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image11.png)
2. Fügen Sie in der Datei "Startup.cs" hervorgehobenen Code unten OWIN Cookie-Authentifizierung konfigurieren.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample4.cs?highlight=1,3,15-19)]

    > [!NOTE]
    > Diese Klasse enthält die `OwinStartup` Attribut zum Angeben der OWIN-Startup-Klasse. Jede OWIN-Anwendung verfügt über ein Startup-Klasse, in dem Sie Komponenten für die Pipeline der Anwendung angeben. Finden Sie unter [OWIN-Startup-Klasse Erkennung](../../../aspnet/overview/owin-and-katana/owin-startup-class-detection.md) für Weitere Informationen zu diesem Modell.

## <a name="adding-web-forms-for-registering-and-logging-in-users"></a>Hinzufügen von Webformularen für die Registrierung und Anmeldung von Benutzern

1. Öffnen der *Register.cs* -Datei und fügen Sie folgenden Code in dem der Benutzer bei der Registrierung ist erfolgreich, protokolliert. Die Änderungen werden hervorgehoben.

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample5.cs)]

    > [!NOTE] 
    > 
    > - Da ASP.NET Identity und OWIN Cookie Authentication anspruchsbasierte System sind, erfordert das Framework der app-Entwickler zum Generieren einer ["ClaimsIdentity"](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.claimsidentity.aspx) für den Benutzer. "ClaimsIdentity" enthält Informationen dazu, alle Ansprüche für Benutzer z. B. welche Rollen, die der Benutzer gehört. Sie können auch weitere Ansprüche für den Benutzer in dieser Phase hinzufügen.
    > - Sie können den Benutzer anmelden, mithilfe der AuthenticationManager aus aufrufen und OWIN `SignIn` und übergeben Sie in der "ClaimsIdentity", wie oben gezeigt. Dieser Code wird Anmelden des Benutzers und generieren auch einen Cookie. Dieser Aufruf ist analog zu [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) ein, die die [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Modul.
2. In **Projektmappen-Explorer**, mit der rechten Maustaste in Ihrem Projekt auf **hinzufügen**, und klicken Sie dann **Webformular**. Nennen Sie das Web Form **Anmeldung**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image12.png)
3. Ersetzen Sie den Inhalt von der *"Login.aspx"* -Datei mit den folgenden Code:  

    [!code-aspx[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample6.aspx)]
4. Ersetzen Sie den Inhalt von der *Login.aspx.cs* Datei durch Folgendes:  

    [!code-csharp[Main](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/samples/sample7.cs)]

    > [!NOTE] 
    > 
    > - Die `Page_Load` jetzt überprüft den Status des aktuellen Benutzers und führt die Aktionen, die basierend auf dessen `Context.User.Identity.IsAuthenticated` Status.  
    >     **Logged in Benutzernamen anzeigen** : wurde die Microsoft ASP.NET Identity Framework Erweiterungsmethoden bereit, die hinzugefügt, auf [System.Security.Principal.IIdentity](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx) , mit der Sie zum Abrufen der `UserName` und `UserId` für die angemeldeter Benutzer Diese Erweiterungsmethoden definiert sind, der `Microsoft.AspNet.Identity.Core` Assembly. Diese Erweiterungsmethoden finden sich die Ersetzung für [HttpContext.User.Identity.Name](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx) .
    > - SignIn-Methode:   
    >     `This` Methode ersetzt die vorherige `CreateUser_Click` -Methode in diesem Beispiel und jetzt meldet sich der Benutzer nach erfolgreicher Erstellung des Benutzers.   
    >  Wurde das Microsoft-OWIN-Framework Erweiterungsmethoden bereit, die hinzugefügt, auf `System.Web.HttpContext` , mit der Sie zum Abrufen eines Verweises auf ein `IOwinContext`. Diese Erweiterungsmethoden definiert sind, im `Microsoft.Owin.Host.SystemWeb` Assembly. Die `OwinContext` -Klasse macht ein `IAuthenticationManager` -Eigenschaft, die authentifizierungsmiddlewarefunktionen, die auf die aktuelle Anforderung darstellt.  
    >  Sie können den Benutzer anmelden, mit der `AuthenticationManager` aus aufrufen und OWIN `SignIn` und übergeben Sie die `ClaimsIdentity` wie oben gezeigt.   
    >  Da ASP.NET Identity und OWIN Cookie Authentication anspruchsbasiertes System sind, wird das Framework erfordert die app zum Generieren einer `ClaimsIdentity` für den Benutzer.   
    >  Die `ClaimsIdentity` enthält Informationen über alle Ansprüche für den Benutzer, wie z. B. welche Rollen, die der Benutzer angehört. Sie können auch weitere Ansprüche für den Benutzer in dieser Phase hinzufügen.  
    >  Dieser Code wird Anmelden des Benutzers und generieren auch einen Cookie. Dieser Aufruf ist analog zu [FormAuthentication.SetAuthCookie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) ein, die die [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Modul.
    > - `SignOut` Methode:   
    >  Ruft einen Verweis auf die `AuthenticationManager` von OWIN und Aufrufe `SignOut`. Dies ist vergleichbar mit [FormsAuthentication.SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) Methode fest, mit der [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) Modul.
5. Drücken Sie **STRG + F5** erstellen und Ausführen der Webanwendung. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image13.png)  
   Hinweis: an diesem Punkt der neue Benutzer erstellt und angemeldet.
6. Klicken Sie auf **Abmelden** Schaltfläche. Sie werden in das Protokoll im Formular umgeleitet werden.
7. Geben Sie ein ungültiger Benutzername oder Kennwort und klicken Sie auf **melden Sie sich bei** Schaltfläche.   
   Die `UserManager.Find` Methode wird Null zurückgegeben und die Fehlermeldung angezeigt: " *Ungültiger Benutzername oder Kennwort* " wird angezeigt.  
  
    ![](adding-aspnet-identity-to-an-empty-or-existing-web-forms-project/_static/image14.png)
