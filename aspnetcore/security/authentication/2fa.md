---
title: Zweistufige Authentifizierung mit SMS in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Einrichten der zweistufigen Authentifizierung (2FA) mit einer app ASP.NET Core.
manager: wpickett
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 20f00c2307e140d81e716304c96a143340d934d0
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="57f0b-103">Zweistufige Authentifizierung mit SMS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="57f0b-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="57f0b-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Schweiz-Entwickler](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="57f0b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="57f0b-105">Finden Sie unter [aktivieren QR-Code-Generierung für Authentifikator-apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) für ASP.NET Core 2.0 und höher.</span><span class="sxs-lookup"><span data-stu-id="57f0b-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="57f0b-106">Dieses Lernprogramm zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mithilfe von SMS.</span><span class="sxs-lookup"><span data-stu-id="57f0b-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="57f0b-107">Anweisungen werden bestimmte für [Twilio](https://www.twilio.com/) und [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), aber Sie können andere SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="57f0b-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="57f0b-108">Es wird empfohlen [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm) vor dem Starten dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="57f0b-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="57f0b-109">Anzeigen der [abgeschlossenen Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="57f0b-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="57f0b-110">[Zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="57f0b-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="57f0b-111">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="57f0b-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="57f0b-112">Erstellen eine neue ASP.NET Core-Web-app mit dem Namen `Web2FA` mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="57f0b-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="57f0b-113">Befolgen Sie die Anweisungen in [Erzwingen von SSL in einer ASP.NET Core app](xref:security/enforcing-ssl) einrichten und SSL erforderlich.</span><span class="sxs-lookup"><span data-stu-id="57f0b-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="57f0b-114">Erstellen Sie ein SMS-Konto</span><span class="sxs-lookup"><span data-stu-id="57f0b-114">Create an SMS account</span></span>

<span data-ttu-id="57f0b-115">Erstellen Sie ein SMS-Konto, z. B. von [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="57f0b-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="57f0b-116">Zeichnen Sie die Anmeldeinformationen für die Authentifizierung (für Twilio: AccountSid und AuthToken, für ASPSMS: Userkey und Kennwort).</span><span class="sxs-lookup"><span data-stu-id="57f0b-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="57f0b-117">Eng zusammenliegen, SMS-Anbieter-Anmeldeinformationen</span><span class="sxs-lookup"><span data-stu-id="57f0b-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="57f0b-118">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-118">**Twilio:**</span></span>  
<span data-ttu-id="57f0b-119">Kopieren Sie aus Ihrem Twilio-Konto in der Registerkarte Dashboard der **Konto-SID** und **autorisierungstokens**.</span><span class="sxs-lookup"><span data-stu-id="57f0b-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="57f0b-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-120">**ASPSMS:**</span></span>  
<span data-ttu-id="57f0b-121">Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrem **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="57f0b-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="57f0b-122">Speichern wir diese Werte mit dem geheimen Schlüssel-Manager-Tool innerhalb der Schlüssel später `SMSAccountIdentification` und `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="57f0b-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="57f0b-123">Angeben von "SenderID" / Absender</span><span class="sxs-lookup"><span data-stu-id="57f0b-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="57f0b-124">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-124">**Twilio:**</span></span>  
<span data-ttu-id="57f0b-125">Kopieren Sie auf der Registerkarte Zahlen Ihrer Twilio **Telefonnummer**.</span><span class="sxs-lookup"><span data-stu-id="57f0b-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="57f0b-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-126">**ASPSMS:**</span></span>  
<span data-ttu-id="57f0b-127">Klicken Sie im Menü Urheber entsperren entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).</span><span class="sxs-lookup"><span data-stu-id="57f0b-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="57f0b-128">Wir werden später speichern den Wert mit dem geheimen Schlüssel-Manager-Tool im Schlüssel `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="57f0b-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="57f0b-129">Geben Sie Anmeldeinformationen für den SMS-Dienst</span><span class="sxs-lookup"><span data-stu-id="57f0b-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="57f0b-130">Wir verwenden die [Optionen Muster](xref:fundamentals/configuration/options) auf die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="57f0b-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="57f0b-131">Erstellen Sie eine Klasse zum Abrufen von SMS-Sicherheitsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="57f0b-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="57f0b-132">Für dieses Beispiel die `SMSoptions` Klasse wird erstellt, der *Services/SMSoptions.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="57f0b-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="57f0b-133">Legen Sie die `SMSAccountIdentification`, `SMSAccountPassword` und `SMSAccountFrom` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="57f0b-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="57f0b-134">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="57f0b-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="57f0b-135">Fügen Sie das NuGet-Paket für den SMS-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="57f0b-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="57f0b-136">Aus der Paket-Manager-Konsole (PMC) ausführen:</span><span class="sxs-lookup"><span data-stu-id="57f0b-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="57f0b-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="57f0b-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="57f0b-139">Fügen Sie Code in der *Services/MessageServices.cs* Datei SMS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="57f0b-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="57f0b-140">Verwenden Sie die Twilio oder Abschnitt ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="57f0b-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="57f0b-141">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="57f0b-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="57f0b-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="57f0b-143">Konfigurieren Sie starten auf, wenn verwenden `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="57f0b-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="57f0b-144">Hinzufügen `SMSoptions` in dem Dienstcontainer den `ConfigureServices` Methode in der *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="57f0b-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="57f0b-145">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="57f0b-145">Enable two-factor authentication</span></span>

<span data-ttu-id="57f0b-146">Öffnen der *Views/Manage/Index.cshtml* Razor-Datei, und entfernen Sie der Kommentar Zeichen (damit kein Markup auskommentiert ist).</span><span class="sxs-lookup"><span data-stu-id="57f0b-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="57f0b-147">Melden Sie sich mit einer zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="57f0b-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="57f0b-148">Die app auszuführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="57f0b-148">Run the app and register a new user</span></span>

![-Webanwendung Register Ansicht öffnen Sie in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="57f0b-150">Tippen Sie auf Ihren Benutzernamen, das aktiviert wird, die `Index` Aktionsmethode im Controller verwalten.</span><span class="sxs-lookup"><span data-stu-id="57f0b-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="57f0b-151">Tippen Sie dann auf die Telefonnummer **hinzufügen** Link.</span><span class="sxs-lookup"><span data-stu-id="57f0b-151">Then tap the phone number **Add** link.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa2.png)

* <span data-ttu-id="57f0b-153">Hinzufügen einer Telefonnummer, die den Prüfcode empfangen wird, und tippen Sie auf **senden Prüfcode**.</span><span class="sxs-lookup"><span data-stu-id="57f0b-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Seite "Phone Number" hinzufügen](2fa/_static/login2fa3.png)

* <span data-ttu-id="57f0b-155">Sie erhalten eine SMS mit der Überprüfungscode.</span><span class="sxs-lookup"><span data-stu-id="57f0b-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="57f0b-156">Geben Sie ihn, und tippen Sie auf **senden**</span><span class="sxs-lookup"><span data-stu-id="57f0b-156">Enter it and tap **Submit**</span></span>

![Überprüfen Sie die Seite "Phone Number"](2fa/_static/login2fa4.png)

<span data-ttu-id="57f0b-158">Wenn Sie eine Textnachricht nicht erhalten, finden Sie unter Seite für Twilio-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="57f0b-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="57f0b-159">Die Ansicht verwalten zeigt, dass Ihre Telefonnummer wurde erfolgreich hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="57f0b-159">The Manage view shows your phone number was added successfully.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa5.png)

* <span data-ttu-id="57f0b-161">Tippen Sie auf **aktivieren** zweistufige Authentifizierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="57f0b-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="57f0b-163">Die zweistufige Authentifizierung testen</span><span class="sxs-lookup"><span data-stu-id="57f0b-163">Test two-factor authentication</span></span>

* <span data-ttu-id="57f0b-164">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="57f0b-164">Log off.</span></span>

* <span data-ttu-id="57f0b-165">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="57f0b-165">Log in.</span></span>

* <span data-ttu-id="57f0b-166">Das Benutzerkonto verfügt über zweistufige Authentifizierung aktiviert, daher Sie die zweite Stufe der Authentifizierung bereitzustellen müssen.</span><span class="sxs-lookup"><span data-stu-id="57f0b-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="57f0b-167">In diesem Lernprogramm haben Sie die Überprüfung per Bürotelefon aktiviert.</span><span class="sxs-lookup"><span data-stu-id="57f0b-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="57f0b-168">Die integrierten Vorlagen ermöglichen außerdem das Einrichten von e-Mail als zweiter Faktor.</span><span class="sxs-lookup"><span data-stu-id="57f0b-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="57f0b-169">Sie können zusätzliche zweite Faktoren für die Authentifizierung, z. B. QR-Codes einrichten.</span><span class="sxs-lookup"><span data-stu-id="57f0b-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="57f0b-170">Tippen Sie auf **übermitteln**.</span><span class="sxs-lookup"><span data-stu-id="57f0b-170">Tap **Submit**.</span></span>

![Überprüfungscode senden](2fa/_static/login2fa7.png)

* <span data-ttu-id="57f0b-172">Geben Sie den Code, den Sie per SMS-Textnachricht erhalten.</span><span class="sxs-lookup"><span data-stu-id="57f0b-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="57f0b-173">Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA für die Anmeldung bei Verwendung der gleichen Geräte und Browser verwenden.</span><span class="sxs-lookup"><span data-stu-id="57f0b-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="57f0b-174">2FA aktivieren und dann auf **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihr Gerät haben.</span><span class="sxs-lookup"><span data-stu-id="57f0b-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="57f0b-175">Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="57f0b-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="57f0b-176">Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Geräten, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Geräten 2FA durchlaufen muss.</span><span class="sxs-lookup"><span data-stu-id="57f0b-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Vergewissern Sie sich anzeigen](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="57f0b-178">Kontosperrung zum Schutz vor Brute-Force-Angriffen</span><span class="sxs-lookup"><span data-stu-id="57f0b-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="57f0b-179">Kontosperrung wird bei 2FA empfohlen.</span><span class="sxs-lookup"><span data-stu-id="57f0b-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="57f0b-180">Sobald ein Benutzer über ein lokales Konto oder soziale Konto anmeldet, wird jedem fehlgeschlagenen Versuch zur 2FA gespeichert.</span><span class="sxs-lookup"><span data-stu-id="57f0b-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="57f0b-181">Wenn die maximale zugriffsversuchsfehlern erreicht ist, wird der Benutzer gesperrt (Standardeinstellung: 5-Minuten-Sperre nach 5 Zugriffsversuchen fehlgeschlagen).</span><span class="sxs-lookup"><span data-stu-id="57f0b-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="57f0b-182">Eine erfolgreiche Authentifizierung setzt die Anzahl der fehlgeschlagenen Zugriffe Versuche und setzt die Uhr.</span><span class="sxs-lookup"><span data-stu-id="57f0b-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="57f0b-183">Die maximale Anzahl fehlgeschlagener Zugriffsversuche und Dauer der Sperrung kann festgelegt werden, mit [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) und [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="57f0b-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="57f0b-184">Der folgende Code konfiguriert die kontosperrung für 10 Minuten nach 10 Zugriffsversuche nicht:</span><span class="sxs-lookup"><span data-stu-id="57f0b-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="57f0b-185">Überprüfen Sie, ob [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) legt `lockoutOnFailure` auf `true`:</span><span class="sxs-lookup"><span data-stu-id="57f0b-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
