---
uid: web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
title: Erstellen Sie eine sichere ASP.NET Web Forms-app mit benutzerregistrierung, e-Mail-Bestätigung und das Kennwort zurücksetzen (c#) | Microsoft Docs
author: Erikre
description: In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Web Forms-Apps mit benutzerregistrierung, e-Mail-Bestätigung und das Kennwort zurückzusetzen, verwenden das ASP.NET Identity-Element...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 0a8d6044-5fab-4213-82d6-5618d5601358
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/security/create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1dc7ace69473b45432fd942b9cf1ba32332cb707
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset-c"></a>Erstellen Sie eine sichere ASP.NET Web Forms-app mit benutzerregistrierung, e-Mail-Bestätigung und das Kennwort zurücksetzen (c#)
====================
by [Erik Reitan](https://github.com/Erikre)

> In diesem Lernprogramm wird gezeigt, wie eine ASP.NET Web Forms-Anwendung mit benutzerregistrierung, e-Mail-Bestätigung und das Kennwort zurückzusetzen, verwenden das Mitgliedschaftssystem ASP.NET Identity zu erstellen. Dieses Lernprogramm basiert auf andersons [MVC-Lernprogramm](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).


## <a name="introduction"></a>Einführung

Dieses Lernprogramm führt Sie durch die erforderlichen Schritte zum Erstellen einer ASP.NET Web Forms-Anwendung mithilfe von Visual Studio und ASP.NET 4.5 zum Erstellen einer sicheren Web Forms-app mit benutzerregistrierung, eine e-Mail-Bestätigung und das Kennwort zurücksetzen.

### <a name="tutorial-tasks-and-information"></a>Lernprogramm Aufgaben und Informationen:

- [Erstellen Sie eine ASP.NET Web Forms-Anwendung](#createWebForms)
- [Verknüpfen mit SendGrid](#SG)
- [E-Mail-Bestätigung vor dem Anmelden](#require)
- [Kennwortwiederherstellung und Zurücksetzen](#reset)
- [Senden von e-Mail-Bestätigungslink](#rsend)
- [Problembehandlung bei der App](#dbg)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="createWebForms"></a>
## <a name="create-an-aspnet-web-forms-app"></a>Erstellen einer ASP.NET Web Forms-App

Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher sowie.

> [!NOTE]
> Warnung: Sie installieren müssen [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.


1. Erstellen eines neuen Projekts (**Datei**  - &gt; **neues Projekt**), und wählen Sie die **ASP.NET-Webanwendung** Vorlage und die aktuelle .NET Framework die Version aus der **neues Projekt** (Dialogfeld).
2. Aus der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web Forms** Vorlage. Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**. Wenn Sie die app in Azure hosten möchten, lassen Sie die **Host in der Cloud** Kontrollkästchen aktiviert.   
 Klicken Sie auf **OK** zum Erstellen eines neuen Projekts.  
    ![Neues ASP.NET-Projekt (Dialogfeld)](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image1.png)
3. Aktivieren Sie Secure Sockets Layer (SSL) für das Projekt ein. Führen Sie die Schritte zur Verfügung, in der **"SSL aktivieren", für das Projekt** Teil der [erste Schritte mit Web Forms Reihe von Lernprogrammen](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms).
4. Führen Sie die app, klicken Sie auf die **registrieren** verknüpfen und einen neuen Benutzer registrieren. Die ausschließliche Überprüfung auf die e-Mail-Adresse zu diesem Zeitpunkt basiert auf der [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut, um sicherzustellen, dass die e-Mail-Adresse wohlgeformt ist. Ändern Sie den Code zum Hinzufügen von e-Mail-Bestätigung. Schließen Sie das Browserfenster.
5. In **Server-Explorer** von Visual Studio (**Ansicht**  - &gt; **Server-Explorer**), navigieren Sie zu **Daten Connections\ DefaultConnection\Tables\AspNetUsers**, mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**. 

    Die folgende Abbildung zeigt die `AspNetUsers` Tabellenschema:

    ![AspNetUsers Tabellenschema](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image2.png)
6. In **Server-Explorer**, mit der rechten Maustaste auf die **AspNetUsers** Tabelle, und wählen Sie **Tabellendaten anzeigen**.  
  
    ![Tabellendaten AspNetUsers](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image3.png)  
 Die e-Mail für den registrierten Benutzer hat zu diesem Zeitpunkt nicht bestätigt.
7. Klicken Sie auf die Zeile, und wählen Sie zum Löschen des Benutzers aus. Sie fügen diese e-Mail erneut im nächsten Schritt hinzu und eine Bestätigungsnachricht an die e-Mail-Adresse senden.

## <a name="email-confirmation"></a>E-Mail-Bestätigung

Es wird empfohlen, die e-Mail-Adresse während der Registrierung eines neuen Benutzers zu überprüfen, sind sie nicht eine andere Person Identität zu bestätigen (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse). Angenommen, Sie ein Diskussionsforum hatten, Sie möchten verhindern `"bob@cpandl.com"` aus der Registrierung als `"joe@contoso.com"`. Ohne e-Mail-Bestätigung `"joe@contoso.com"` konnten unerwünschte e-Mails von Ihrer app abgerufen werden. Angenommen, Bob versehentlich als registriert `"bib@cpandl.com"` und hadn't bemerkt, er wäre kennwortwiederherstellung verwenden, da die app hat seine richtige e-Mail-Nachricht nicht. E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und stellt keinen Schutz vor Spammern bestimmt.

In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie von einer e-Mail, eine SMS oder einen anderen Mechanismus bestätigt wurden. In den folgenden Abschnitten wird e-Mail-Bestätigung aktivieren und ändern Sie den Code, um zu verhindern, dass neu registrierte Benutzer anmelden, bis ihre e-Mails bestätigt wurde. In diesem Lernprogramm verwenden Sie die e-Mail-Dienst SendGrid.

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a>Verknüpfen mit SendGrid

Obwohl dieses Lernprogramm zeigt nur zum Hinzufügen von e-Mail-Benachrichtigung über [SendGrid](http://sendgrid.com/), können Sie e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden (finden Sie unter [zusätzliche Ressourcen](#addRes)).

1. Öffnen Sie in Visual Studio die **Package Manager Console** (**Tools**  - &gt; **NuGet-Paket-Manager**  - &gt; **Package Manager Console**), und geben Sie den folgenden Befehl aus:  
    `Install-Package SendGrid`
2. Wechseln Sie zu der [Azure SendGrid-Registrierungsseite](https://azure.microsoft.com/gallery/store/sendgrid/sendgrid-azure/) und kostenlos SendGrid-Konto registrieren. Sie können auch registrieren Sie sich für ein kostenloses SendGrid-Konto direkt auf [SendGrids-Website](http://www.sendgrid.com).
3. Aus **Projektmappen-Explorer** öffnen Sie die *IdentityConfig.cs* in der Datei die *App\_starten* Ordner, und fügen Sie folgenden Code, um die gelbhervorgehoben`EmailService` Klasse so konfigurieren Sie **SendGrid**:

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample1.cs?highlight=3,5,8-37)]
4. Darüber hinaus fügen Sie die folgenden `using` Anweisungen am Anfang der *IdentityConfig.cs* Datei: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample2.cs?highlight=1-4)]
5. Um dieses Beispiel einfach zu halten, speichern Sie die e-Mail-Dienst-Kontowerte in der `appSettings` Teil der *"Web.config"* Datei. Fügen Sie die folgenden XML-Code in gelb hervorgehoben werden die *"Web.config"* Datei im Stammverzeichnis des Projekts:

    [!code-xml[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample3.xml?highlight=2-5)]

    > [!WARNING]
    > Sicherheit – sensible Daten nie im Quellcode speichern. In diesem Beispiel wird das Konto und die Anmeldeinformationen werden gespeichert der **AppSetting** Teil der *"Web.config"* Datei. Sie können auf Azure sicher diese Werte speichern, auf die **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Registerkarte im Azure-Portal. Weitere Informationen finden Sie im Thema andersons [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](https://go.microsoft.com/fwlink/?LinkId=513141).
6. Fügen Sie die e-Mail-Dienst-Werte, um widerzuspiegeln, dass die Werte der SendGrid-Authentifizierung (Benutzername und Kennwort), damit Sie erfolgreich können Ihre app Senden von e-Mails von. Achten Sie darauf, dass Sie Ihren SendGrid-Kontonamen statt auf die e-Mail-Adresse angegebene SendGrid verwenden.

### <a name="enable-email-confirmation"></a>Aktivieren von e-Mail-Bestätigung

 Um e-Mail-Bestätigung zu aktivieren, müssen Sie den Registrierungscode, der mithilfe der folgenden Schritte ändern.  
 

1. In der *Konto* Ordner die *Register.aspx.cs* CodeBehind und aktualisieren Sie die `CreateUser_Click` Methode, um die folgenden hervorgehobenen Änderungen zu aktivieren: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample4.cs?highlight=9-11)]
2. In **Projektmappen-Explorer**, mit der rechten Maustaste *"default.aspx"* , und wählen Sie **als Startseite festlegen**.
3. Führen Sie die app durch Drücken von **F5.** Nachdem die Seite angezeigt wird, klicken Sie auf die **registrieren** Link, um die Seite "Register" anzuzeigen.
4. Geben Sie Ihre e-Mail- und das Kennwort, und klicken Sie auf die **registrieren** Schaltfläche zum Senden einer e-Mail-Nachricht über SendGrid.  
   Der aktuelle Status des Projekts und Code ermöglicht dem Benutzer anmelden, nachdem sie das Registrierungsformular abgeschlossen, obwohl sie ihrem Konto bestätigt dies nicht getan haben.
5. Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.  
   Nachdem Sie das Registrierungsformular senden, werden Sie angemeldet werden.  
    ![Beispiel-Website - angemeldet](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/_static/image4.png)

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a>E-Mail-Bestätigung vor dem Anmelden

Obwohl Sie das e-Mail-Konto bestätigt haben, würden an diesem Punkt müssen nicht auf klicken auf den Link, den Sie in der e-Mail zur Überprüfung um vollständig angemeldet werden. Im folgenden Abschnitt ändern Sie den Code, der neue Benutzer um eine bestätigte e-Mail-Adresse zu erhalten, bevor sie angemeldet sind (authentifiziert).

1. In **Projektmappen-Explorer** von Visual Studio aktualisiert die `CreateUser_Click` Ereignis in der *Register.aspx.cs* Code-Behind in enthaltenen der *Konten* Ordner mit der folgende hervorgehobene Änderungen vor: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample5.cs?highlight=13-14,17-21)]
2. Update der `LogIn` Methode in der *Login.aspx.cs* CodeBehind mit den folgenden hervorgehobenen Änderungen: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample6.cs?highlight=9-19,45-46)]

### <a name="run-the-application"></a>Ausführen der Anwendung

 Nun, dass Sie den Code, um zu überprüfen, ob die e-Mail-Adresse des Benutzers bestätigt wurde implementiert haben, sehen Sie die Funktionalität sowohl die **registrieren** und **Anmeldung** Seiten. 

1. Löschen Sie alle Konten in der **AspNetUsers** Tabelle, die den e-Mail-Alias enthalten, Sie testen möchten.
2. Führen Sie die app (**F5**) und überprüfen Sie, ob Sie können nicht als ein Benutzer registrieren, wenn Sie Ihre e-Mail-Adresse bestätigt haben.
3. Vor dem bestätigen Ihr neuen Kontos über die e-Mail-Adresse, die gerade übermittelt wurde, versuchen Sie, sich mit dem neuen Konto anzumelden.  
 Sie sehen, dass Sie nicht anmelden können, und dass Sie ein bestätigte e-Mail-Konto benötigen.
4. Nachdem Sie Ihre e-Mail-Adresse bestätigt haben, melden Sie sich an die app.

<a id="reset"></a>
## <a name="password-recovery-and-reset"></a>Kennwortwiederherstellung und Zurücksetzen

1. In Visual Studio, entfernen Sie die Kommentarzeichen aus der `Forgot` Methode in der *Forgot.aspx.cs* Code-Behind in enthaltenen der *Konto* Ordner, damit die Methode angezeigt wird, wie folgt: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample7.cs?highlight=16-18)]
2. Öffnen der *"Login.aspx"* Seite. Ersetzen Sie das Markup gegen Ende der **LoginForm** Abschnitt wie unten markiert: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample8.aspx?highlight=52-53)]
3. Öffnen der *Login.aspx.cs* CodeBehind und kommentieren Sie die folgende Codezeile in gelb hervorgehoben der `Page_Load` Ereignishandler: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample9.cs?highlight=5)]
4. Führen Sie die app durch Drücken von **F5.** Nachdem die Seite angezeigt wird, klicken Sie auf die **melden Sie sich** Link.
5. Klicken Sie auf die **haben Sie Ihr Kennwort vergessen?** Link zum Anzeigen der **"Kennwort vergessen"** Seite.
6. Geben Sie Ihre e-Mail-Adresse ein, und klicken Sie auf die **Absenden** Schaltfläche, um eine e-Mail an Ihre Adresse senden, dem Sie Ihr Kennwort zurücksetzen können.   
   Überprüfen Sie Ihr e-Mail-Konto, und klicken Sie auf den Link zum Anzeigen der **Kennwort zurücksetzen** Seite.
7. Auf der **Kennwort zurücksetzen** geben Ihre e-Mail, Kennwort und bestätigte Kennwort. Drücken Sie anschließend die **zurücksetzen** Schaltfläche.  
   Wenn Sie Ihr Kennwort erfolgreich Zurücksetzen der **Kennwort geändert** Seite wird angezeigt. Nun können Sie sich mit Ihrem neuen Kennwort anmelden.

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a>Senden von e-Mail-Bestätigungslink

Sobald ein Benutzer ein neues lokales Konto erstellt, werden sie einen Link zur Bestätigung per e-Mail gesendet, den sie für erforderlich sind, verwenden, bevor sie sich anmelden können. Wenn der Benutzer versehentlich der e-Mail zur kaufbestätigung löscht oder die e-Mail-Adresse nie eingeht, benötigen sie den Link zur Bestätigung erneut gesendet. Die folgenden codeänderungen zeigen, wie dies zu ermöglichen.

1. Öffnen Sie in Visual Studio die **Login.aspx.cs** CodeBehind und fügen Sie den folgenden Ereignishandler nach der `LogIn` Ereignishandler:   

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample10.cs)]
2. Ändern der `LogIn` -Ereignishandler in der *Login.aspx.cs* CodeBehind durch Ändern des Codes in gelb hervorgehoben werden wie folgt: 

    [!code-csharp[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample11.cs?highlight=15-17)]
3. Update der *"Login.aspx"* Seite durch Hinzufügen des Codes in gelb hervorgehoben werden wie folgt: 

    [!code-aspx[Main](create-a-secure-aspnet-web-forms-app-with-user-registration-email-confirmation-and-password-reset/samples/sample12.aspx?highlight=45-46)]
4. Löschen Sie alle Konten in der **AspNetUsers** Tabelle, die den e-Mail-Alias enthalten, Sie testen möchten.
5. Führen Sie die app (**F5**), und registrieren Sie Ihre e-Mail-Adresse.
6. Vor dem bestätigen Ihr neuen Kontos über die e-Mail-Adresse, die gerade übermittelt wurde, versuchen Sie, sich mit dem neuen Konto anzumelden.  
   Sie sehen, dass Sie nicht anmelden können, und dass Sie ein bestätigte e-Mail-Konto benötigen. Darüber hinaus können Sie jetzt eine bestätigungsmeldung an Ihrem e-Mail-Konto erneut senden.
7. Geben Sie Ihre e-Mail-Adresse und das Kennwort ein, drücken Sie dann die **Bestätigung senden** Schaltfläche.
8. Nachdem Sie Ihre e-Mail-Adresse auf Grundlage der neu gesendeten e-Mail-Nachricht bestätigt haben, melden Sie sich an die app.

<a id="dbg"></a>
## <a name="troubleshooting-the-app"></a>Problembehandlung bei der App

Wenn Sie eine e-Mail mit den Link, um Ihre Anmeldeinformationen überprüfen nicht erhalten:

- Überprüfen Sie Ihren Junk oder Spam-Ordner.
- Melden Sie sich bei Ihrem SendGrid-Konto, und klicken Sie auf die [-e-Mail von aktivitätsverknüpfungen](https://sendgrid.com/logs/index).
- Stellen Sie sicher, dass Sie Ihren SendGrid-Benutzernamen Konto als verwendet eine *"Web.config"* Wert anstelle der e-Mail-Adresse für Ihr SendGrid-Konto.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Links zu ASP.NET Identity empfohlene Ressourcen](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- [Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [ASP.NET Web Forms Reihe von Lernprogrammen - Hinzufügen einer OAuth 2.0-Anbieter](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#OAuthWebForms)
- [Bereitstellen von Mitgliedschaft, OAuth und SQL-Datenbank eine sichere ASP.NET Web Forms-App in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)
- [ASP.NET Web Forms Reihe von Lernprogrammen - SSL für das Projekt aktivieren](../getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal.md#SSLWebForms)
