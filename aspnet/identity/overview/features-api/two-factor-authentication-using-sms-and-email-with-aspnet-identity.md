---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity | Microsoft Docs
author: HaoK
description: In diesem Lernprogramm erfahren Sie, wie zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-eingerichtet. In diesem Artikel von Rick Anderson geschrieben wurde ( @RickAndMSFT ), Pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="dc7f9-104">Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="dc7f9-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="dc7f9-105">durch [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="dc7f9-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="dc7f9-106">In diesem Lernprogramm erfahren Sie, wie zweistufige Authentifizierung (2FA) mithilfe von SMS und e-Mail-eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="dc7f9-107">In diesem Artikel von Rick Anderson geschrieben wurde ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung und Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="dc7f9-108">Das NuGet-Beispiel wurde in erster Linie durch Hao Kung geschrieben.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="dc7f9-109">In diesem Thema werden die folgenden Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-109">This topic covers the following:</span></span>

- [<span data-ttu-id="dc7f9-110">Erstellen des Beispiels Identität</span><span class="sxs-lookup"><span data-stu-id="dc7f9-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="dc7f9-111">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="dc7f9-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="dc7f9-112">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="dc7f9-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="dc7f9-113">Vorgehensweise: Registrieren Sie einen Multi-Factor Authentication-Anbieter</span><span class="sxs-lookup"><span data-stu-id="dc7f9-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="dc7f9-114">Kombinieren von sozialen und lokalen Anmeldekonten</span><span class="sxs-lookup"><span data-stu-id="dc7f9-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="dc7f9-115">Kontosperrung vor Brute-Force-Angriffen</span><span class="sxs-lookup"><span data-stu-id="dc7f9-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="dc7f9-116">Erstellen des Beispiels Identität</span><span class="sxs-lookup"><span data-stu-id="dc7f9-116">Building the Identity sample</span></span>

<span data-ttu-id="dc7f9-117">NuGet verwenden Sie in diesem Abschnitt ein Beispiel herunterladen möchten, die wir mit verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="dc7f9-118">Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="dc7f9-119">Installieren von Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="dc7f9-120">Warnung: Sie müssen Visual Studio installieren [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="dc7f9-121">Erstellen Sie ein neues ***leere*** ASP.NET-Webprojekt.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="dc7f9-122">Geben Sie den folgenden, in der Paket-Manager-Konsole die folgenden Befehle:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 <span data-ttu-id="dc7f9-123">In diesem Lernprogramm verwenden wir [SendGrid](http://sendgrid.com/) das Senden von e-Mails und [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) für Sms-Texte.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="dc7f9-124">Die `Identity.Samples` Paket wird installiert, den wir arbeiten mit Code.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="dc7f9-125">Legen Sie die [Projekt zur Verwendung von SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="dc7f9-126">*Optionale*: befolgen Sie die Anweisungen in meinem [e-Mail-Bestätigung Lernprogramm](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid einbinden und führen Sie die app und registrieren ein e-Mail-Konto.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="dc7f9-127">* Optional: * Demo e-Mail-Link-Bestätigungscode aus dem Beispiel entfernen (die `ViewBag.Link` Code in die Konto-Controller.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="dc7f9-128">Finden Sie unter der `DisplayEmail` und `ForgotPasswordConfirmation` Aktionsmethoden und Razor-Ansichten).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="dc7f9-129">* Optional: * Entfernen der `ViewBag.Status` Code aus verwalten und Konto für Controller und die *Views\Account\VerifyCode.cshtml* und *Views\Manage\VerifyPhoneNumber.cshtml* Razor-Ansichten.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-129">*Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="dc7f9-130">Alternativ können Sie behalten die `ViewBag.Status` So testen Sie die Funktionsweise dieser app lokal ohne zu verknüpfen und Senden von e-Mail und SMS-Nachrichten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="dc7f9-131">Warnung: Wenn Sie die Sicherheitseinstellungen in diesem Beispiel ändern, Produktionen apps müssen einer sicherheitsüberprüfung unterzogen werden, die explizit, die Änderungen vorgenommen aufruft.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="dc7f9-132">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="dc7f9-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="dc7f9-133">Dieses Lernprogramm enthält Anweisungen für die Verwendung von Twilio oder ASPSMS, aber Sie können andere SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="dc7f9-134">**Erstellen ein Benutzerkonto mit einem SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-134">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="dc7f9-135">Erstellen einer [Twilio](https://www.twilio.com/try-twilio) oder ein [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) Konto.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="dc7f9-136">**Installieren zusätzlicher Pakete oder Hinzufügen von Dienstverweisen**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-136">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="dc7f9-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-137">Twilio:</span></span>  
 <span data-ttu-id="dc7f9-138">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="dc7f9-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-139">ASPSMS:</span></span>  
 <span data-ttu-id="dc7f9-140">Die folgenden Dienstverweis muss hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 <span data-ttu-id="dc7f9-141">Adresse:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="dc7f9-142">Namespace:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="dc7f9-143">**Eng zusammenliegen, SMS-Anbieter-Benutzeranmeldeinformationen**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-143">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="dc7f9-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-144">Twilio:</span></span>  
 <span data-ttu-id="dc7f9-145">Aus der **Dashboard** Registerkarte Ihrem Twilio-Konto Kopie der **Konto-SID** und **autorisierungstokens**.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="dc7f9-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-146">ASPSMS:</span></span>  
 <span data-ttu-id="dc7f9-147">Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrer selbst definierten **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="dc7f9-148">Wir werden diese Werte später gespeichert, in der Variablen `SMSAccountIdentification` und `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="dc7f9-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="dc7f9-149">**Angeben von "SenderID" / Absender**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-149">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="dc7f9-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-150">Twilio:</span></span>  
 <span data-ttu-id="dc7f9-151">Aus der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="dc7f9-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-152">ASPSMS:</span></span>  
 <span data-ttu-id="dc7f9-153">Innerhalb der **entsperren Urheber** Menü, entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="dc7f9-154">Wir werden diesen Wert später gespeichert, in der Variablen `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="dc7f9-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="dc7f9-155">**SMS-Anbieter-Anmeldeinformationen in der app übertragen**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-155">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="dc7f9-156">Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders an die app zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="dc7f9-157">Sicherheit – sensible Daten nie im Quellcode speichern.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="dc7f9-158">Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="dc7f9-159">Finden Sie unter der Jon Atten [ASP.NET-MVC: Beibehalten von privaten Einstellungen außerhalb des Datenquellen-Steuerelements](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="dc7f9-160">**Implementierung der Datenübertragung an SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="dc7f9-160">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="dc7f9-161">Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="dc7f9-162">Je nach verwendeter SMS-Anbieter aktivieren Sie entweder die **Twilio** oder **ASPSMS** Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="dc7f9-163">Führen Sie die app, und melden Sie sich mit dem Konto, das Sie zuvor registriert.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="dc7f9-164">Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode in `Manage` Controller.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="dc7f9-165">Klicken Sie auf Hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="dc7f9-166">In wenigen Sekunden erhalten Sie eine Textnachricht mit den Überprüfungscode.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="dc7f9-167">Geben Sie ihn, und drücken Sie die **Absenden**.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="dc7f9-168">Die Ansicht verwalten zeigt, dass Ihre Telefonnummer hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="dc7f9-169">Überprüfen Sie den code</span><span class="sxs-lookup"><span data-stu-id="dc7f9-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="dc7f9-170">Die `Index` Aktionsmethode in `Manage` Controller legt die Statusmeldung basierend auf der vorherigen Aktion und enthält Links, um Ihr lokales Kennwort ändern oder Hinzufügen eines lokalen Kontos.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="dc7f9-171">Die `Index` Methode zeigt auch den Zustand oder die 2FA phone Anzahl, externer Anmeldungen, 2FA aktiviert, und zu behalten, 2FA-Methode für diesen Browser (Dies wird weiter unten erläutert).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="dc7f9-172">Durch Klicken auf Ihre Benutzer-ID (e-Mail) in der Titelleiste, keine Nachricht übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="dc7f9-173">Durch Klicken auf die **Telefonnummer: Entfernen** verknüpfen übergibt `Message=RemovePhoneSuccess` als Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="dc7f9-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="dc7f9-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="dc7f9-175">Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld, um eine zehnstellige Nummer eingeben, die SMS-Nachrichten empfangen können.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="dc7f9-176">Durch Klicken auf die **senden Prüfcode** Schaltfläche sendet die Telefonnummer an den HTTP-POST `AddPhoneNumber` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="dc7f9-177">Die `GenerateChangePhoneNumberTokenAsync` Methode generiert das Sicherheitstoken, die in der SMS-Nachricht festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="dc7f9-178">Wenn der SMS-Dienst konfiguriert wurde, wird das Token gesendet, als die Zeichenfolge &quot;Ihr Sicherheitscode lautet &lt;token&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="dc7f9-179">Die `SmsService.SendAsync` aufzurufende Methode asynchron aufgerufen wird, und klicken Sie dann auf die app umgeleitet wird die `VerifyPhoneNumber` Aktionsmethode (in der das folgende Dialogfeld angezeigt), in dem Sie den Überprüfungscode eingeben können.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="dc7f9-180">Nachdem Sie geben Sie den Code, und klicken Sie auf Absenden, der Code an die HTTP-POST gesendet wird `VerifyPhoneNumber` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="dc7f9-181">Die `ChangePhoneNumberAsync` Methode überprüft die bereitgestellten Sicherheitscode.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="dc7f9-182">Wenn der Code korrekt ist, wird die Telefonnummer hinzugefügt der `PhoneNumber` Feld der `AspNetUsers` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="dc7f9-183">Wenn dieser Aufruf erfolgreich ist, wird die `SignInAsync` -Methode aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="dc7f9-184">Die `isPersistent` Parameter legt fest, ob die authentifizierungssitzung über mehrere Anforderungen hinweg persistent gespeichert wird.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="dc7f9-185">Wenn Sie Ihrem Sicherheitsprofil ändern, ein neuen sicherheitsstempel generiert und gespeichert, der `SecurityStamp` Feld der *AspNetUsers* Tabelle.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="dc7f9-186">Beachten Sie, dass die `SecurityStamp` Feld unterscheidet sich das Sicherheitscookie.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="dc7f9-187">Das Sicherheitscookie befindet sich nicht in der `AspNetUsers` Tabelle (oder an anderer Stelle in der Identity-Datenbank).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="dc7f9-188">Das Cookie Sicherheitstoken ist selbstsigniert mit [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) und wird erstellt, mit der `UserId, SecurityStamp` und Zeitinformationen Ablauf.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="dc7f9-189">Die Cookie-Middleware überprüft das Cookie bei jeder Anforderung.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="dc7f9-190">Die `SecurityStampValidator` Methode in der `Startup` Klasse trifft die-Datenbank und in regelmäßigen Abständen, überprüft der sicherheitsstempel wie angegeben mit der `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="dc7f9-191">Dies geschieht nur alle 30 Minuten (in unserem Beispiel), es sei denn, Sie Ihrem Sicherheitsprofil ändern.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="dc7f9-192">Das Intervall von 30 Minuten wurde gewählt, um Roundtrips zur Datenbank zu minimieren.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="dc7f9-193">Die `SignInAsync` Methode muss aufgerufen werden, wenn das Sicherheitsprofil geändert wurde.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="dc7f9-194">Wenn das Sicherheitsprofil ändert, wird die Datenbank Updates der `SecurityStamp` Feld, und ohne Aufruf der `SignInAsync` Methode Sie angemeldet bleiben würde *nur* bis das nächste Mal die OWIN-Pipeline erreicht die Datenbank (der `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="dc7f9-195">Können Sie testen, indem Sie ändern die `SignInAsync` Methode sofort zurück, und Festlegen des Cookies `validateInterval` Eigenschaft von 30 Minuten auf 5 Sekunden:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="dc7f9-196">Mit den oben genannten Methodencode ändert, können Sie Ihrem Sicherheitsprofil ändern (z. B. durch Ändern des Status von **zwei Faktor aktiviert**) und Sie werden abgemeldet in 5 Sekunden bei der `SecurityStampValidator.OnValidateIdentity` Methode schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="dc7f9-197">Entfernen Sie die Zeile zurück, in der `SignInAsync` -Methode, stellen Sie ein anderes Sicherheitsprofil für ändern und Sie nicht werden abgemeldet. Die `SignInAsync` Methode generiert einen neuen Cookie.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="dc7f9-198">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="dc7f9-198">Enable two-factor authentication</span></span>

<span data-ttu-id="dc7f9-199">In der Beispiel-app müssen Sie die Benutzeroberfläche verwenden, um zweistufige Authentifizierung (2FA) zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="dc7f9-200">Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um 2FA zu aktivieren.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="dc7f9-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="dc7f9-201">Klicken Sie auf 2FA aktivieren.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="dc7f9-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="dc7f9-202">Melden Sie sich ab und dann wieder anzumelden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-202">Log out, then log back in.</span></span> <span data-ttu-id="dc7f9-203">Wenn Sie e-Mail aktiviert haben (finden Sie unter meinem [vorherigen Lernprogramm](account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Nachrichten für 2FA auswählen.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="dc7f9-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="dc7f9-204">Vergewissern Sie sich Codepage wird angezeigt, in dem Sie den Code (von SMS oder e-Mail) eingeben können.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="dc7f9-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="dc7f9-205">Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA zu verwenden, um mit diesem Computer und den Browser anmelden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="dc7f9-206">2FA aktivieren und dann auf die **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihren Computer haben.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="dc7f9-207">Sie können auf einem Computer aus privaten so vorgehen, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="dc7f9-208">Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Computern, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Computern 2FA durchlaufen muss.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="dc7f9-209">Vorgehensweise: Registrieren Sie einen Multi-Factor Authentication-Anbieter</span><span class="sxs-lookup"><span data-stu-id="dc7f9-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="dc7f9-210">Wenn Sie ein neues MVC-Projekt, erstellen die *IdentityConfig.cs* Datei enthält den folgenden Code aus, um einen Multi-Factor Authentication-Anbieter zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="dc7f9-211">Fügen Sie eine Telefonnummer für 2FA hinzu.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="dc7f9-212">Die `AddPhoneNumber` Aktionsmethode in der `Manage` Controller wird ein Sicherheitstoken generiert und sendet es an das Telefon Zahl Sie bereitgestellt haben.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="dc7f9-213">Nach dem Senden des Tokens, leitet sie an der `VerifyPhoneNumber` Aktionsmethode, in dem Sie den Code so registrieren Sie SMS für 2FA eingeben können.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="dc7f9-214">SMS-2FA wird nicht verwendet werden, bis Sie die Telefonnummer überprüft haben.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="dc7f9-215">2FA aktivieren</span><span class="sxs-lookup"><span data-stu-id="dc7f9-215">Enabling 2FA</span></span>

<span data-ttu-id="dc7f9-216">Die `EnableTFA` Aktionsmethode ermöglicht 2FA:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="dc7f9-217">Beachten Sie die `SignInAsync` muss aufgerufen werden, da aktivieren 2FA eine Änderung auf das Sicherheitsprofil handelt.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="dc7f9-218">Wenn 2FA aktiviert ist, muss der Benutzer mit 2FA anmelden, verwenden die 2FA-Ansätze, die sie registriert haben, (SMS und e-Mail im Beispiel).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="dc7f9-219">Sie können weitere 2FA Anbieter z. B. QR-Code-Generatoren hinzufügen, oder Schreiben Sie besitzen (finden Sie unter [mithilfe von Google Authenticator mit ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="dc7f9-220">Die 2FA Codes sind mit generiert [Algorithmus zum einmaligen zeitbasierte](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) und Codes sechs Minuten lang gültig sind.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="dc7f9-221">Wenn Sie den Code eingeben, mehr als sechs Minuten dauern, erhalten Sie eine Fehlermeldung von ungültigem Code.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="dc7f9-222">Kombinieren von sozialen und lokalen Anmeldekonten</span><span class="sxs-lookup"><span data-stu-id="dc7f9-222">Combine social and local login accounts</span></span>

<span data-ttu-id="dc7f9-223">Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="dc7f9-224">In der folgenden Reihenfolge &quot; RickAndMSFT@gmail.com &quot; wird zuerst als eine lokale Anmeldung erstellt, aber Sie können erstellen Sie das Konto als sozialen Protokoll zuerst und anschließend fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="dc7f9-225">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-225">Click on the **Manage** link.</span></span> <span data-ttu-id="dc7f9-226">Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="dc7f9-227">Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="dc7f9-228">Die beiden Konten wurden kombiniert, Sie können mit entweder Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="dc7f9-229">Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls der Anmeldung sozialen Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="dc7f9-230">In der folgenden Abbildung ist Tom soziales Netzwerk anzumelden (die daraufhin aus der **externer Anmeldungen: 1** auf der Seite angezeigt).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="dc7f9-231">Durch Klicken auf **ein Kennwort** bietet die Möglichkeit, fügen ein lokales Protokoll auf dem gleichen Konto zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="dc7f9-232">Kontosperrung vor Brute-Force-Angriffen</span><span class="sxs-lookup"><span data-stu-id="dc7f9-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="dc7f9-233">Benutzersperre aktivieren, um die Konten in Ihrer app von Wörterbuchangriffe zu schützen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="dc7f9-234">Im folgenden code in die `ApplicationUserManager Create` Methode konfiguriert gesperrt war:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="dc7f9-235">Der obige Code ermöglicht Sperre nur für die zweistufige Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="dc7f9-236">Während Sie gesperrt war für Anmeldungen, indem ändern aktivieren können `shouldLockout` auf "true" in der `Login` -Methode des kontocontrollers, sollten Sie nicht gesperrt war für Anmeldungen aktivieren, da es das Konto anfällig macht [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) Anmeldung Angriffe.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="dc7f9-237">Im Beispielcode, Sperrung deaktiviert ist, für das Administratorkonto in erstellt die `ApplicationDbInitializer Seed` Methode:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="dc7f9-238">Dass der Benutzer ein überprüfte e-Mail-Konto muss</span><span class="sxs-lookup"><span data-stu-id="dc7f9-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="dc7f9-239">Der folgende Code muss ein Benutzer eine überprüfte e-Mail-Konto haben, bevor sie sich anmelden können:</span><span class="sxs-lookup"><span data-stu-id="dc7f9-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="dc7f9-240">Wie SignInManager 2FA Anforderung prüft, ob</span><span class="sxs-lookup"><span data-stu-id="dc7f9-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="dc7f9-241">Sowohl der lokalen Anmeldung, und soziale anmelden überprüfen, um festzustellen, ob 2FA aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="dc7f9-242">Wenn 2FA aktiviert ist, die `SignInManager` Anmeldung Methodenrückgabe `SignInStatus.RequiresVerification`, und der Benutzer umgeleitet werden die `SendCode` Aktionsmethode, geben Sie den Code, um das Protokoll in der Sequenz abschließen müssen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="dc7f9-243">Wenn der Benutzer RememberMe hat für das lokale Benutzer-Cookie festgelegt ist die `SignInManager` zurück `SignInStatus.Success` und er keinen 2FA durchlaufen.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="dc7f9-244">Der folgende code zeigt die `SendCode` Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="dc7f9-245">Ein [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) wird erstellt, mit allen 2FA-Methoden, die für den Benutzer aktiviert.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-245">A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="dc7f9-246">Die [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) wird zum Übergeben der [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) Hilfsfunktion, die dem Benutzer ermöglicht, wählen Sie den Ansatz 2FA (in der Regel die e-Mail- und SMS).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-246">The [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="dc7f9-247">Sobald der Benutzer die 2FA-Methode, sendet der `HTTP POST SendCode` Aktionsmethode aufgerufen wird, die `SignInManager` sendet, die an den 2FA-Code, und der Benutzer umgeleitet werden die `VerifyCode` Aktionsmethode, die in dem sie den Code zum Abschließen der Anmeldung eingeben können.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="dc7f9-248">2FA Sperre</span><span class="sxs-lookup"><span data-stu-id="dc7f9-248">2FA Lockout</span></span>

<span data-ttu-id="dc7f9-249">Obwohl Sie die kontosperrung auf Anmeldung Versuch Kennwortfehler festlegen können, wird dieser Ansatz Ihrer Anmeldename anfällig für [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) sperren.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="dc7f9-250">Es wird empfohlen, dass Sie nur mit 2FA kontosperrung verwenden.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="dc7f9-251">Wenn die `ApplicationUserManager` wird erstellt, legt der Beispielcode 2FA Sperre und `MaxFailedAccessAttemptsBeforeLockout` bis fünf.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="dc7f9-252">Sobald ein Benutzer (über lokales Konto oder soziale Konto) anmeldet, jedem fehlgeschlagenen Versuch zur 2FA gespeichert ist, und wenn die maximalen Versuche erreicht ist, der Benutzer fünf Minuten gesperrt ist (Sie können festlegen, dass die Sperre mit `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="dc7f9-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="dc7f9-253">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="dc7f9-253">Additional Resources</span></span>

- <span data-ttu-id="dc7f9-254">[ASP.NET Identity empfohlene Ressourcen](../getting-started/aspnet-identity-recommended-resources.md) vollständige Liste der Identity-Blogs, Videos, Lernprogramme und gute daher verknüpft.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="dc7f9-255">[MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) zeigt außerdem das Hinzufügen von Profilinformationen zur Users-Tabelle.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="dc7f9-256">[ASP.NET MVC und Identität 2.0: Kenntnisse der Grundlagen](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) von John Atten.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="dc7f9-257">Kontobestätigung und Kennwortwiederherstellung mit ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="dc7f9-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="dc7f9-258">Einführung in ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="dc7f9-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="dc7f9-259">[Ankündigung der RTM-Version von ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) von Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="dc7f9-260">[Identität für ASP.NET 2.0: Einrichten von Kontovalidierung und zweistufige Autorisierung](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) von John Atten.</span><span class="sxs-lookup"><span data-stu-id="dc7f9-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
