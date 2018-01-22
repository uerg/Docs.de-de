---
title: Zweistufige Authentifizierung mit SMS
author: rick-anderson
description: Zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mit ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 0970b7e95b116ceab1d17502d2f8aee9cd821715
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="d99d1-103">Zweistufige Authentifizierung mit SMS</span><span class="sxs-lookup"><span data-stu-id="d99d1-103">Two-factor authentication with SMS</span></span>

<span data-ttu-id="d99d1-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Schweiz-Entwickler](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="d99d1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="d99d1-105">Dieses Tutorial bezieht sich auf ASP.NET Core nur 1.x.</span><span class="sxs-lookup"><span data-stu-id="d99d1-105">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="d99d1-106">Finden Sie unter [aktivieren QR-Code-Generierung für Authentifikator-apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) für ASP.NET Core 2.0 und höher.</span><span class="sxs-lookup"><span data-stu-id="d99d1-106">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="d99d1-107">Dieses Lernprogramm zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mithilfe von SMS.</span><span class="sxs-lookup"><span data-stu-id="d99d1-107">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="d99d1-108">Anweisungen werden bestimmte für [Twilio](https://www.twilio.com/) und [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), aber Sie können andere SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-108">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="d99d1-109">Es wird empfohlen [Kontobestätigung und Kennwortwiederherstellung](accconfirm.md) vor dem Starten dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="d99d1-109">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="d99d1-110">Anzeigen der [abgeschlossenen Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="d99d1-110">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="d99d1-111">[Zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="d99d1-111">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="d99d1-112">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="d99d1-112">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="d99d1-113">Erstellen eine neue ASP.NET Core-Web-app mit dem Namen `Web2FA` mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="d99d1-113">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="d99d1-114">Befolgen Sie die Anweisungen in [Erzwingen von SSL in einer ASP.NET Core app](xref:security/enforcing-ssl) einrichten und SSL erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d99d1-114">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="d99d1-115">Erstellen Sie ein SMS-Konto</span><span class="sxs-lookup"><span data-stu-id="d99d1-115">Create an SMS account</span></span>

<span data-ttu-id="d99d1-116">Erstellen Sie ein SMS-Konto, z. B. von [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="d99d1-116">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="d99d1-117">Zeichnen Sie die Anmeldeinformationen für die Authentifizierung (für Twilio: AccountSid und AuthToken, für ASPSMS: Userkey und Kennwort).</span><span class="sxs-lookup"><span data-stu-id="d99d1-117">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="d99d1-118">Eng zusammenliegen, SMS-Anbieter-Anmeldeinformationen</span><span class="sxs-lookup"><span data-stu-id="d99d1-118">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="d99d1-119">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-119">**Twilio:**</span></span>  
<span data-ttu-id="d99d1-120">Kopieren Sie aus Ihrem Twilio-Konto in der Registerkarte Dashboard der **Konto-SID** und **autorisierungstokens**.</span><span class="sxs-lookup"><span data-stu-id="d99d1-120">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="d99d1-121">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-121">**ASPSMS:**</span></span>  
<span data-ttu-id="d99d1-122">Navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrem **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="d99d1-122">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="d99d1-123">Speichern wir diese Werte mit dem geheimen Schlüssel-Manager-Tool innerhalb der Schlüssel später `SMSAccountIdentification` und `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="d99d1-123">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="d99d1-124">Angeben von "SenderID" / Absender</span><span class="sxs-lookup"><span data-stu-id="d99d1-124">Specifying SenderID / Originator</span></span>

<span data-ttu-id="d99d1-125">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-125">**Twilio:**</span></span>  
<span data-ttu-id="d99d1-126">Kopieren Sie auf der Registerkarte Zahlen Ihrer Twilio **Telefonnummer**.</span><span class="sxs-lookup"><span data-stu-id="d99d1-126">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="d99d1-127">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-127">**ASPSMS:**</span></span>  
<span data-ttu-id="d99d1-128">Klicken Sie im Menü Urheber entsperren entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).</span><span class="sxs-lookup"><span data-stu-id="d99d1-128">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="d99d1-129">Wir werden später speichern den Wert mit dem geheimen Schlüssel-Manager-Tool im Schlüssel `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="d99d1-129">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="d99d1-130">Geben Sie Anmeldeinformationen für den SMS-Dienst</span><span class="sxs-lookup"><span data-stu-id="d99d1-130">Provide credentials for the SMS service</span></span>

<span data-ttu-id="d99d1-131">Wir verwenden die [Optionen Muster](xref:fundamentals/configuration/options) auf die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="d99d1-131">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="d99d1-132">Erstellen Sie eine Klasse zum Abrufen von SMS-Sicherheitsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="d99d1-132">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="d99d1-133">Für dieses Beispiel die `SMSoptions` Klasse wird erstellt, der *Services/SMSoptions.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="d99d1-133">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="d99d1-134">Legen Sie die `SMSAccountIdentification`, `SMSAccountPassword` und `SMSAccountFrom` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="d99d1-134">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="d99d1-135">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="d99d1-135">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="d99d1-136">Fügen Sie das NuGet-Paket für den SMS-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="d99d1-136">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="d99d1-137">Aus der Paket-Manager-Konsole (PMC) ausführen:</span><span class="sxs-lookup"><span data-stu-id="d99d1-137">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="d99d1-138">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-138">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="d99d1-139">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-139">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="d99d1-140">Fügen Sie Code in der *Services/MessageServices.cs* Datei SMS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="d99d1-140">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="d99d1-141">Verwenden Sie die Twilio oder Abschnitt ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="d99d1-141">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="d99d1-142">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-142">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="d99d1-143">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="d99d1-143">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="d99d1-144">Konfigurieren Sie starten auf, wenn verwenden`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="d99d1-144">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="d99d1-145">Hinzufügen `SMSoptions` in dem Dienstcontainer den `ConfigureServices` Methode in der *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="d99d1-145">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="d99d1-146">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="d99d1-146">Enable two-factor authentication</span></span>

<span data-ttu-id="d99d1-147">Öffnen der *Views/Manage/Index.cshtml* Razor-Datei, und entfernen Sie der Kommentar Zeichen (damit kein Markup auskommentiert ist).</span><span class="sxs-lookup"><span data-stu-id="d99d1-147">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="d99d1-148">Melden Sie sich mit einer zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="d99d1-148">Log in with two-factor authentication</span></span>

* <span data-ttu-id="d99d1-149">Die app auszuführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="d99d1-149">Run the app and register a new user</span></span>

![-Webanwendung Register Ansicht öffnen Sie in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="d99d1-151">Tippen Sie auf Ihren Benutzernamen, das aktiviert wird, die `Index` Aktionsmethode im Controller verwalten.</span><span class="sxs-lookup"><span data-stu-id="d99d1-151">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="d99d1-152">Tippen Sie dann auf die Telefonnummer **hinzufügen** Link.</span><span class="sxs-lookup"><span data-stu-id="d99d1-152">Then tap the phone number **Add** link.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa2.png)

* <span data-ttu-id="d99d1-154">Hinzufügen einer Telefonnummer, die den Prüfcode empfangen wird, und tippen Sie auf **senden Prüfcode**.</span><span class="sxs-lookup"><span data-stu-id="d99d1-154">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Seite "Phone Number" hinzufügen](2fa/_static/login2fa3.png)

* <span data-ttu-id="d99d1-156">Sie erhalten eine SMS mit der Überprüfungscode.</span><span class="sxs-lookup"><span data-stu-id="d99d1-156">You will get a text message with the verification code.</span></span> <span data-ttu-id="d99d1-157">Geben Sie ihn, und tippen Sie auf **senden**</span><span class="sxs-lookup"><span data-stu-id="d99d1-157">Enter it and tap **Submit**</span></span>

![Überprüfen Sie die Seite "Phone Number"](2fa/_static/login2fa4.png)

<span data-ttu-id="d99d1-159">Wenn Sie eine Textnachricht nicht erhalten, finden Sie unter Seite für Twilio-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="d99d1-159">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="d99d1-160">Die Ansicht verwalten zeigt, dass Ihre Telefonnummer wurde erfolgreich hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="d99d1-160">The Manage view shows your phone number was added successfully.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa5.png)

* <span data-ttu-id="d99d1-162">Tippen Sie auf **aktivieren** zweistufige Authentifizierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="d99d1-162">Tap **Enable** to enable two-factor authentication.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="d99d1-164">Die zweistufige Authentifizierung testen</span><span class="sxs-lookup"><span data-stu-id="d99d1-164">Test two-factor authentication</span></span>

* <span data-ttu-id="d99d1-165">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="d99d1-165">Log off.</span></span>

* <span data-ttu-id="d99d1-166">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-166">Log in.</span></span>

* <span data-ttu-id="d99d1-167">Das Benutzerkonto verfügt über zweistufige Authentifizierung aktiviert, daher Sie die zweite Stufe der Authentifizierung bereitzustellen müssen.</span><span class="sxs-lookup"><span data-stu-id="d99d1-167">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="d99d1-168">In diesem Lernprogramm haben Sie die Überprüfung per Bürotelefon aktiviert.</span><span class="sxs-lookup"><span data-stu-id="d99d1-168">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="d99d1-169">Die integrierten Vorlagen ermöglichen außerdem das Einrichten von e-Mail als zweiter Faktor.</span><span class="sxs-lookup"><span data-stu-id="d99d1-169">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="d99d1-170">Sie können zusätzliche zweite Faktoren für die Authentifizierung, z. B. QR-Codes einrichten.</span><span class="sxs-lookup"><span data-stu-id="d99d1-170">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="d99d1-171">Tippen Sie auf **übermitteln**.</span><span class="sxs-lookup"><span data-stu-id="d99d1-171">Tap **Submit**.</span></span>

![Überprüfungscode senden](2fa/_static/login2fa7.png)

* <span data-ttu-id="d99d1-173">Geben Sie den Code, den Sie per SMS-Textnachricht erhalten.</span><span class="sxs-lookup"><span data-stu-id="d99d1-173">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="d99d1-174">Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA für die Anmeldung bei Verwendung der gleichen Geräte und Browser verwenden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-174">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="d99d1-175">2FA aktivieren und dann auf **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihr Gerät haben.</span><span class="sxs-lookup"><span data-stu-id="d99d1-175">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="d99d1-176">Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-176">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="d99d1-177">Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Geräten, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Geräten 2FA durchlaufen muss.</span><span class="sxs-lookup"><span data-stu-id="d99d1-177">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Vergewissern Sie sich anzeigen](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="d99d1-179">Kontosperrung zum Schutz vor Brute-Force-Angriffen</span><span class="sxs-lookup"><span data-stu-id="d99d1-179">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="d99d1-180">Es wird empfohlen, dass Sie kontosperrung mit 2FA verwenden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-180">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="d99d1-181">Sobald ein Benutzer (über ein lokales Konto oder soziale Konto) anmeldet, jedem fehlgeschlagenen Versuch zur 2FA gespeichert ist, und wenn die maximale Versuche (Standard ist 5) wird erreicht, der Benutzer fünf Minuten gesperrt ist (Sie können festlegen, dass die Sperre mit `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="d99d1-181">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="d99d1-182">Der folgende Code konfiguriert Konto für 10 Minuten nach 10 fehlgeschlagenen Versuchen ausgesperrt werden.</span><span class="sxs-lookup"><span data-stu-id="d99d1-182">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
