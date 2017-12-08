---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: ASP.NET MVC 5-Anwendung mit SMS und e-Mail-zweistufige Authentifizierung | Microsoft Docs
author: Rick-Anderson
description: "In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung erstellen. Führen Sie erstellen einen sicheren ASP.NET MVC 5-Web-app mit..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="5a014-104">ASP.NET MVC 5-Anwendung mit SMS und e-Mail-zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5a014-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="5a014-105">Durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="5a014-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="5a014-106">In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Web-app mit einer zweistufigen Authentifizierung erstellen.</span><span class="sxs-lookup"><span data-stu-id="5a014-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="5a014-107">Führen Sie [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="5a014-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="5a014-108">Sie können herunterladen die fertigen Anwendung [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="5a014-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="5a014-109">Der Download enthält die debugging-Hilfen, mit denen Sie die e-Mail-Bestätigung und SMS zu testen, ohne eine e-Mail oder SMS-Anbieter einrichten zu müssen.</span><span class="sxs-lookup"><span data-stu-id="5a014-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="5a014-110">Dieses Lernprogramm wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="5a014-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="5a014-111">Erstellen einer ASP.NET MVC-app</span><span class="sxs-lookup"><span data-stu-id="5a014-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="5a014-112">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5a014-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="5a014-113">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="5a014-113">Enable Two-factor authentication</span></span>](#enable2)
- <span data-ttu-id="5a014-114">[Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)</span><span class="sxs-lookup"><span data-stu-id="5a014-114">[Additional Resources](#addRes)</span></span>

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="5a014-115">Erstellen einer ASP.NET MVC-app</span><span class="sxs-lookup"><span data-stu-id="5a014-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="5a014-116">Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="5a014-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="5a014-117">Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.</span><span class="sxs-lookup"><span data-stu-id="5a014-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="5a014-118">Warnung: Führen Sie [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) bevor Sie fortfahren.</span><span class="sxs-lookup"><span data-stu-id="5a014-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="5a014-119">Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="5a014-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="5a014-120">Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="5a014-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="5a014-121">WebForms unterstützt auch ASP.NET Identity, damit Sie ähnliche Schritte in einer Web Forms-app folgen können.</span><span class="sxs-lookup"><span data-stu-id="5a014-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="5a014-122">Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="5a014-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="5a014-123">Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="5a014-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="5a014-124">Später in diesem Lernprogramm wird es in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="5a014-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="5a014-125">Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="5a014-125">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="5a014-126">Legen Sie die [Projekt zur Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="5a014-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="5a014-127">Einrichten von SMS für die zweistufige Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5a014-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="5a014-128">Dieses Lernprogramm enthält Anweisungen für die Verwendung von Twilio oder ASPSMS, aber Sie können andere SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="5a014-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="5a014-129">**Erstellen ein Benutzerkonto mit einem SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="5a014-129">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="5a014-130">Erstellen einer [Twilio](https://www.twilio.com/try-twilio) oder ein [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) Konto.</span><span class="sxs-lookup"><span data-stu-id="5a014-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="5a014-131">**Installieren zusätzlicher Pakete oder Hinzufügen von Dienstverweisen**</span><span class="sxs-lookup"><span data-stu-id="5a014-131">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="5a014-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="5a014-132">Twilio:</span></span>  
 <span data-ttu-id="5a014-133">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="5a014-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="5a014-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="5a014-134">ASPSMS:</span></span>  
 <span data-ttu-id="5a014-135">Die folgenden Dienstverweis muss hinzugefügt werden:</span><span class="sxs-lookup"><span data-stu-id="5a014-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 <span data-ttu-id="5a014-136">Adresse:</span><span class="sxs-lookup"><span data-stu-id="5a014-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="5a014-137">Namespace:</span><span class="sxs-lookup"><span data-stu-id="5a014-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="5a014-138">**Eng zusammenliegen, SMS-Anbieter-Benutzeranmeldeinformationen**</span><span class="sxs-lookup"><span data-stu-id="5a014-138">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="5a014-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="5a014-139">Twilio:</span></span>  
 <span data-ttu-id="5a014-140">Aus der **Dashboard** Registerkarte Ihrem Twilio-Konto Kopie der **Konto-SID** und **autorisierungstokens**.</span><span class="sxs-lookup"><span data-stu-id="5a014-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="5a014-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="5a014-141">ASPSMS:</span></span>  
 <span data-ttu-id="5a014-142">Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrer selbst definierten **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="5a014-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="5a014-143">Speichern wir diese Werte in einem späteren Zeitpunkt die *"Web.config"* -Datei in den Schlüsseln `"SMSAccountIdentification"` und `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="5a014-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="5a014-144">**Angeben von "SenderID" / Absender**</span><span class="sxs-lookup"><span data-stu-id="5a014-144">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="5a014-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="5a014-145">Twilio:</span></span>  
 <span data-ttu-id="5a014-146">Aus der **Zahlen** Registerkarte, kopieren Sie Ihre Twilio-Telefonnummer.</span><span class="sxs-lookup"><span data-stu-id="5a014-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="5a014-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="5a014-147">ASPSMS:</span></span>  
 <span data-ttu-id="5a014-148">Innerhalb der **entsperren Urheber** Menü, entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).</span><span class="sxs-lookup"><span data-stu-id="5a014-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="5a014-149">Wir werden später speichern den Wert in der *"Web.config"* Datei innerhalb des Schlüssels `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="5a014-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="5a014-150">**SMS-Anbieter-Anmeldeinformationen in der app übertragen**</span><span class="sxs-lookup"><span data-stu-id="5a014-150">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="5a014-151">Stellen Sie die Anmeldeinformationen und die Telefonnummer des Absenders der app zur Verfügung.</span><span class="sxs-lookup"><span data-stu-id="5a014-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="5a014-152">Um die Dinge einfach zu halten speichern wir diese Werte in der *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="5a014-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="5a014-153">Wenn wir in Azure bereitstellen, speichern wir die Werte, die sicher in die **Anwendungseinstellungen** Abschnitt auf der Website, Registerkarte "konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="5a014-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="5a014-154">Sicherheit – sensible Daten nie im Quellcode speichern.</span><span class="sxs-lookup"><span data-stu-id="5a014-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="5a014-155">Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="5a014-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="5a014-156">Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="5a014-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="5a014-157">**Implementierung der Datenübertragung an SMS-Anbieter**</span><span class="sxs-lookup"><span data-stu-id="5a014-157">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="5a014-158">Konfigurieren der `SmsService` -Klasse in der *App\_Start\IdentityConfig.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="5a014-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="5a014-159">Je nach verwendeter SMS-Anbieter aktivieren Sie entweder die **Twilio** oder **ASPSMS** Abschnitt:</span><span class="sxs-lookup"><span data-stu-id="5a014-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="5a014-160">Update der *Views\Manage\Index.cshtml* Razor-Ansicht: (Hinweis: nicht nur die Kommentare im vorhandenen Code zu entfernen, verwenden Sie den folgenden Code.)</span><span class="sxs-lookup"><span data-stu-id="5a014-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="5a014-161">Überprüfen Sie die `EnableTwoFactorAuthentication` und `DisableTwoFactorAuthentication` Aktionsmethoden in der `ManageController` haben die[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) Attribut:</span><span class="sxs-lookup"><span data-stu-id="5a014-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="5a014-162">Führen Sie die app, und melden Sie sich mit dem Konto, das Sie zuvor registriert.</span><span class="sxs-lookup"><span data-stu-id="5a014-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="5a014-163">Klicken Sie auf Ihre Benutzer-ID, das aktiviert wird, die `Index` Aktionsmethode in `Manage` Controller.</span><span class="sxs-lookup"><span data-stu-id="5a014-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="5a014-164">Klicken Sie auf Hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5a014-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="5a014-165">Die `AddPhoneNumber` Aktionsmethode zeigt ein Dialogfeld, um eine zehnstellige Nummer eingeben, die SMS-Nachrichten empfangen können.</span><span class="sxs-lookup"><span data-stu-id="5a014-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="5a014-166">In wenigen Sekunden erhalten Sie eine Textnachricht mit den Überprüfungscode.</span><span class="sxs-lookup"><span data-stu-id="5a014-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="5a014-167">Geben Sie ihn, und drücken Sie die **Absenden**.</span><span class="sxs-lookup"><span data-stu-id="5a014-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="5a014-168">Die Ansicht verwalten zeigt, dass Ihre Telefonnummer hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="5a014-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="5a014-169">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="5a014-169">Enable two-factor authentication</span></span>

<span data-ttu-id="5a014-170">In der app generierte Vorlage müssen Sie die Benutzeroberfläche verwenden, um zweistufige Authentifizierung (2FA) zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5a014-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="5a014-171">Klicken Sie auf Ihre Benutzer-ID (e-Mail-Alias) in der Navigationsleiste, um 2FA zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5a014-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="5a014-172">Klicken Sie auf 2FA aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5a014-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="5a014-173">Abmelden, klicken Sie dann-melden Sie sich wieder im.</span><span class="sxs-lookup"><span data-stu-id="5a014-173">Logout, then log back in.</span></span> <span data-ttu-id="5a014-174">Wenn Sie e-Mail aktiviert haben (finden Sie unter meinem [vorherigen Lernprogramm](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), können Sie die SMS oder e-Mail-Nachrichten für 2FA auswählen.</span><span class="sxs-lookup"><span data-stu-id="5a014-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="5a014-175">Vergewissern Sie sich Codepage wird angezeigt, in dem Sie den Code (von SMS oder e-Mail) eingeben können.</span><span class="sxs-lookup"><span data-stu-id="5a014-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="5a014-176">Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA verwenden, um sich anzumelden, wenn mit dem Browser und dem Gerät, in denen Sie das Feld überprüft.</span><span class="sxs-lookup"><span data-stu-id="5a014-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="5a014-177">Als böswillige Benutzer auf Ihr Gerät zugreifen können, 2FA aktivieren, und klicken Sie auf die **Denken Sie daran Browser** wird bieten Ihnen einfachen einschrittigen Kennwortzugriff und gleichzeitig die starke 2FA Schutz für alle Zugriffsvorgänge beibehalten von nicht vertrauenswürdigen Geräten.</span><span class="sxs-lookup"><span data-stu-id="5a014-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="5a014-178">Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="5a014-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="5a014-179">Dieses Lernprogramm enthält eine kurze Einführung in die 2FA auf eine neue ASP.NET MVC-app aktivieren.</span><span class="sxs-lookup"><span data-stu-id="5a014-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="5a014-180">Meine Lernprogramm [zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) ins Detail, auf den Code hinter dem Beispiel wird.</span><span class="sxs-lookup"><span data-stu-id="5a014-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="5a014-181">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5a014-181">Additional Resources</span></span>

- <span data-ttu-id="5a014-182">[Zweistufige Authentifizierung mithilfe von SMS und e-Mails mit ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) wechselt in den Details zu einer zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="5a014-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="5a014-183">Links zu ASP.NET Identity empfohlene Ressourcen</span><span class="sxs-lookup"><span data-stu-id="5a014-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="5a014-184">[Konto zur Bestätigung und Kennwortwiederherstellung mit ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf Wiederherstellung und Konto für die Bestätigung des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="5a014-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="5a014-185">[MVC 5-Anwendung mit Facebook, Twitter, LinkedIn und Google OAuth2 anmelden](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) dieses Lernprogramm veranschaulicht das Schreiben einer ASP.NET MVC 5-Apps mit Facebook und Google OAuth 2 Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="5a014-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="5a014-186">Es wird gezeigt, wie der Identity-Datenbank zusätzliche Daten hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="5a014-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="5a014-187">[Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf Azure-Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="5a014-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="5a014-188">In diesem Lernprogramm fügt Azure-Bereitstellung zum Sichern Ihrer Apps mit Rollen, wie die Mitgliedschafts-API zu verwenden, um Benutzern und Rollen und zusätzliche Sicherheitsfunktionen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="5a014-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="5a014-189">Erstellen einer Google-app für OAuth 2, und verbinden die app zum Projekt</span><span class="sxs-lookup"><span data-stu-id="5a014-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="5a014-190">Erstellen die app in Facebook, und verbinden die app zum Projekt</span><span class="sxs-lookup"><span data-stu-id="5a014-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="5a014-191">Einrichten von SSL in das Projekt</span><span class="sxs-lookup"><span data-stu-id="5a014-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
