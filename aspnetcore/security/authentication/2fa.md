---
title: Zweistufige Authentifizierung mit SMS in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Einrichten der zweistufigen Authentifizierung (2FA) mit einer app ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 0308b05ebcda1af7f6850549d7a33f1df1a912a0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089983"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="f1c6d-103">Zweistufige Authentifizierung mit SMS in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f1c6d-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="f1c6d-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Schweiz-Entwickler](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="f1c6d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

 <span data-ttu-id="f1c6d-105">Zwei Faktor-Authentifizierung (2FA) Authentifikator-apps, mit der eine zeitbasierte zum einmaligen Kennwort Algorithmus (TOTP), sind die empfohlene Vorgehensweise für 2FA Branche.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="f1c6d-106">2FA TOTP mit SMS 2FA bevorzugt wird.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="f1c6d-107">Weitere Informationen finden Sie unter [aktivieren QR-Code-Generierung für TOTP Authentifikator-apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) für ASP.NET Core 2.0 und höher.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="f1c6d-108">Dieses Lernprogramm zeigt, wie zum Einrichten der zweistufigen Authentifizierung (2FA) mithilfe von SMS.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="f1c6d-109">Anweisungen werden bestimmte für [Twilio](https://www.twilio.com/) und [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), aber Sie können andere SMS-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="f1c6d-110">Es wird empfohlen [Kontobestätigung und Kennwortwiederherstellung](xref:security/authentication/accconfirm) vor dem Starten dieses Lernprogramms.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="f1c6d-111">Anzeigen der [abgeschlossenen Beispiel](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="f1c6d-112">[Zum Herunterladen von](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="f1c6d-113">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="f1c6d-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="f1c6d-114">Erstellen eine neue ASP.NET Core-Web-app mit dem Namen `Web2FA` mit einzelnen Benutzerkonten.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="f1c6d-115">Befolgen Sie die Anweisungen in [Erzwingen von SSL in einer ASP.NET Core app](xref:security/enforcing-ssl) einrichten und SSL erforderlich.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="f1c6d-116">Erstellen Sie ein SMS-Konto</span><span class="sxs-lookup"><span data-stu-id="f1c6d-116">Create an SMS account</span></span>

<span data-ttu-id="f1c6d-117">Erstellen Sie ein SMS-Konto, z. B. von [Twilio](https://www.twilio.com/) oder [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="f1c6d-118">Zeichnen Sie die Anmeldeinformationen für die Authentifizierung (für Twilio: AccountSid und AuthToken, für ASPSMS: Userkey und Kennwort).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="f1c6d-119">Eng zusammenliegen, SMS-Anbieter-Anmeldeinformationen</span><span class="sxs-lookup"><span data-stu-id="f1c6d-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="f1c6d-120">**Twilio:** aus Ihrem Twilio-Konto in der Registerkarte Dashboard Kopieren der **Konto-SID** und **autorisierungstokens**.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="f1c6d-121">**ASPSMS:** navigieren Sie zu Ihrer kontoeinstellungen **Userkey** und kopieren Sie sie zusammen mit Ihrem **Kennwort**.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="f1c6d-122">Speichern wir diese Werte mit dem geheimen Schlüssel-Manager-Tool innerhalb der Schlüssel später `SMSAccountIdentification` und `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="f1c6d-123">Angeben von "SenderID" / Absender</span><span class="sxs-lookup"><span data-stu-id="f1c6d-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="f1c6d-124">**Twilio:** aus der Registerkarte ' Zahlen ' Kopieren Ihrer Twilio **Telefonnummer**.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="f1c6d-125">**ASPSMS:** im entsperren Urheber-Menü, entsperren Sie eine oder mehrere Urheber, oder wählen Sie eine alphanumerische Absender (von allen Netzwerken nicht unterstützt).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="f1c6d-126">Wir werden später speichern den Wert mit dem geheimen Schlüssel-Manager-Tool im Schlüssel `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="f1c6d-127">Geben Sie Anmeldeinformationen für den SMS-Dienst</span><span class="sxs-lookup"><span data-stu-id="f1c6d-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="f1c6d-128">Wir verwenden die [Optionen Muster](xref:fundamentals/configuration/options) auf die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="f1c6d-129">Erstellen Sie eine Klasse zum Abrufen von SMS-Sicherheitsschlüssel.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="f1c6d-130">Für dieses Beispiel die `SMSoptions` Klasse wird erstellt, der *Services/SMSoptions.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="f1c6d-131">Legen Sie die `SMSAccountIdentification`, `SMSAccountPassword` und `SMSAccountFrom` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="f1c6d-132">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="f1c6d-133">Fügen Sie das NuGet-Paket für den SMS-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="f1c6d-134">Aus der Paket-Manager-Konsole (PMC) ausführen:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="f1c6d-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="f1c6d-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="f1c6d-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="f1c6d-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="f1c6d-137">Fügen Sie Code in der *Services/MessageServices.cs* Datei SMS zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="f1c6d-138">Verwenden Sie die Twilio oder Abschnitt ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="f1c6d-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="f1c6d-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="f1c6d-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="f1c6d-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="f1c6d-141">Konfigurieren Sie starten auf, wenn verwenden `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="f1c6d-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="f1c6d-142">Hinzufügen `SMSoptions` in dem Dienstcontainer den `ConfigureServices` Methode in der *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="f1c6d-143">Zweistufige Authentifizierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="f1c6d-143">Enable two-factor authentication</span></span>

<span data-ttu-id="f1c6d-144">Öffnen der *Views/Manage/Index.cshtml* Razor-Datei, und entfernen Sie der Kommentar Zeichen (damit kein Markup auskommentiert ist).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="f1c6d-145">Melden Sie sich mit einer zweistufigen Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="f1c6d-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="f1c6d-146">Die app auszuführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="f1c6d-146">Run the app and register a new user</span></span>

![-Webanwendung Register Ansicht öffnen Sie in Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="f1c6d-148">Tippen Sie auf Ihren Benutzernamen, das aktiviert wird, die `Index` Aktionsmethode im Controller verwalten.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="f1c6d-149">Tippen Sie dann auf die Telefonnummer **hinzufügen** Link.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-149">Then tap the phone number **Add** link.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa2.png)

* <span data-ttu-id="f1c6d-151">Hinzufügen einer Telefonnummer, die den Prüfcode empfangen wird, und tippen Sie auf **senden Prüfcode**.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Seite "Phone Number" hinzufügen](2fa/_static/login2fa3.png)

* <span data-ttu-id="f1c6d-153">Sie erhalten eine SMS mit der Überprüfungscode.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="f1c6d-154">Geben Sie ihn, und tippen Sie auf **senden**</span><span class="sxs-lookup"><span data-stu-id="f1c6d-154">Enter it and tap **Submit**</span></span>

![Überprüfen Sie die Seite "Phone Number"](2fa/_static/login2fa4.png)

<span data-ttu-id="f1c6d-156">Wenn Sie eine Textnachricht nicht erhalten, finden Sie unter Seite für Twilio-Protokoll.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="f1c6d-157">Die Ansicht verwalten zeigt, dass Ihre Telefonnummer wurde erfolgreich hinzugefügt wurde.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-157">The Manage view shows your phone number was added successfully.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa5.png)

* <span data-ttu-id="f1c6d-159">Tippen Sie auf **aktivieren** zweistufige Authentifizierung zu aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Verwalten von anzeigen](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="f1c6d-161">Die zweistufige Authentifizierung testen</span><span class="sxs-lookup"><span data-stu-id="f1c6d-161">Test two-factor authentication</span></span>

* <span data-ttu-id="f1c6d-162">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-162">Log off.</span></span>

* <span data-ttu-id="f1c6d-163">Anmelden.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-163">Log in.</span></span>

* <span data-ttu-id="f1c6d-164">Das Benutzerkonto verfügt über zweistufige Authentifizierung aktiviert, daher Sie die zweite Stufe der Authentifizierung bereitzustellen müssen.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="f1c6d-165">In diesem Lernprogramm haben Sie die Überprüfung per Bürotelefon aktiviert.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="f1c6d-166">Die integrierten Vorlagen ermöglichen außerdem das Einrichten von e-Mail als zweiter Faktor.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="f1c6d-167">Sie können zusätzliche zweite Faktoren für die Authentifizierung, z. B. QR-Codes einrichten.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="f1c6d-168">Tippen Sie auf **übermitteln**.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-168">Tap **Submit**.</span></span>

![Überprüfungscode senden](2fa/_static/login2fa7.png)

* <span data-ttu-id="f1c6d-170">Geben Sie den Code, den Sie per SMS-Textnachricht erhalten.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="f1c6d-171">Durch Klicken auf die **Denken Sie daran Browser** Kontrollkästchen nehmen Sie nicht zu 2FA für die Anmeldung bei Verwendung der gleichen Geräte und Browser verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="f1c6d-172">2FA aktivieren und dann auf **Denken Sie daran Browser** werden Ihnen starken 2FA Schutz vor böswilligen Benutzern, die auf Ihr Konto zugreifen möchten, solange sie keinen Zugriff auf Ihr Gerät haben.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="f1c6d-173">Sie können auf einem privaten Gerät so vorgehen, die Sie regelmäßig verwenden.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="f1c6d-174">Durch Festlegen von **Denken Sie daran Browser**, erhalten Sie die zusätzliche Sicherheit des 2FA von Geräten, die Sie nicht regelmäßig verwenden und erhalten Sie die Vorteile nicht auf Ihren eigenen Geräten 2FA durchlaufen muss.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Vergewissern Sie sich anzeigen](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="f1c6d-176">Kontosperrung zum Schutz vor Brute-Force-Angriffen</span><span class="sxs-lookup"><span data-stu-id="f1c6d-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="f1c6d-177">Kontosperrung wird bei 2FA empfohlen.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="f1c6d-178">Sobald ein Benutzer über ein lokales Konto oder soziale Konto anmeldet, wird jedem fehlgeschlagenen Versuch zur 2FA gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="f1c6d-179">Wenn die maximale zugriffsversuchsfehlern erreicht ist, wird der Benutzer gesperrt (Standardeinstellung: 5-Minuten-Sperre nach 5 Zugriffsversuchen fehlgeschlagen).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="f1c6d-180">Eine erfolgreiche Authentifizierung setzt die Anzahl der fehlgeschlagenen Zugriffe Versuche und setzt die Uhr.</span><span class="sxs-lookup"><span data-stu-id="f1c6d-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="f1c6d-181">Die maximale Anzahl fehlgeschlagener Zugriffsversuche und Dauer der Sperrung kann festgelegt werden, mit [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) und [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="f1c6d-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="f1c6d-182">Der folgende Code konfiguriert die kontosperrung für 10 Minuten nach 10 Zugriffsversuche nicht:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="f1c6d-183">Überprüfen Sie, ob [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) legt `lockoutOnFailure` auf `true`:</span><span class="sxs-lookup"><span data-stu-id="f1c6d-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
