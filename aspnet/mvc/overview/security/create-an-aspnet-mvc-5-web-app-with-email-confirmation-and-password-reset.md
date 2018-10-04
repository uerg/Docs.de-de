---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Erstellen eine sichere ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und kennwortzurücksetzung mithilfe von ASP.NET Identity-Mitgliedschaftssystem zu erstellen. Sie Zertifizierungsstelle...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 02a0153f20e9390a5ab8d4ecb4f73556b339d9a9
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576455"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="7648a-104">Erstellen einer sicheren ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung (c#)</span><span class="sxs-lookup"><span data-stu-id="7648a-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="7648a-105">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="7648a-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="7648a-106">In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Web-app mit e-Mail-Bestätigung und kennwortzurücksetzung mithilfe von ASP.NET Identity-Mitgliedschaftssystem zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7648a-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="7648a-107">Sie können die fertige Anwendung herunterladen [hier](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="7648a-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7648a-108">Der Download enthält Debuggen-Hilfsprogramme, mit denen Sie die Bestätigung per e-Mail und SMS zu testen, ohne das Einrichten einer e-Mail oder SMS-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="7648a-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="7648a-109">In diesem Tutorial wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="7648a-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="7648a-110">Erstellen einer ASP.NET MVC-app</span><span class="sxs-lookup"><span data-stu-id="7648a-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="7648a-111">Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="7648a-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="7648a-112">Installieren Sie [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher.</span><span class="sxs-lookup"><span data-stu-id="7648a-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="7648a-113">Warnung: Sie müssen installieren [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) oder höher, um dieses Lernprogramm abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="7648a-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="7648a-114">Erstellen Sie ein neues ASP.NET Web-Projekt, und wählen Sie die MVC-Vorlage.</span><span class="sxs-lookup"><span data-stu-id="7648a-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="7648a-115">Web Forms unterstützt auch ASP.NET Identity, sodass Sie ähnliche Schritte in einer Web Forms-app ausführen konnte.</span><span class="sxs-lookup"><span data-stu-id="7648a-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="7648a-116">Lassen Sie die Standardauthentifizierung als **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="7648a-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="7648a-117">Wenn Sie die app in Azure hosten möchten, lassen Sie das Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="7648a-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="7648a-118">Später in diesem Tutorial werden wir in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="7648a-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="7648a-119">Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="7648a-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="7648a-120">Legen Sie die [Projekt für die Verwendung von SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="7648a-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="7648a-121">Die app auszuführen, klicken Sie auf die **registrieren** verknüpfen, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="7648a-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="7648a-122">Die ausschließliche Überprüfung der auf die e-Mail-Adresse an diesem Punkt ist, mit der [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut.</span><span class="sxs-lookup"><span data-stu-id="7648a-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="7648a-123">Wechseln Sie in Server-Explorer zu **Daten Connections\DefaultConnection\Tables\AspNetUsers**, mit der rechten Maustaste, und wählen Sie **Tabellendefinition öffnen**.</span><span class="sxs-lookup"><span data-stu-id="7648a-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="7648a-124">Die folgende Abbildung zeigt die `AspNetUsers` Schema:</span><span class="sxs-lookup"><span data-stu-id="7648a-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="7648a-125">Klicken Sie mit der rechten Maustaste auf die **"aspnetusers"** Tabelle, und wählen Sie **Tabellendaten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="7648a-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="7648a-126">Die e-Mail wurde an diesem Punkt nicht bestätigt.</span><span class="sxs-lookup"><span data-stu-id="7648a-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="7648a-127">Klicken Sie auf die Zeile, und wählen Sie löschen.</span><span class="sxs-lookup"><span data-stu-id="7648a-127">Click on the row and select delete.</span></span> <span data-ttu-id="7648a-128">Sie fügen Sie diese e-Mail erneut im nächsten Schritt hinzu, und eine Bestätigung per e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="7648a-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="7648a-129">E-Mail-Bestätigung</span><span class="sxs-lookup"><span data-stu-id="7648a-129">Email confirmation</span></span>

<span data-ttu-id="7648a-130">Es hat sich bewährt, bestätigen die e-Mail-Adresse einer neuen benutzerregistrierung, um zu überprüfen, ob sie keine Identität einer anderen Person (d. h. sie noch nicht registriert mit einer anderen Person für den e-Mail-Adresse).</span><span class="sxs-lookup"><span data-stu-id="7648a-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7648a-131">Angenommen, Sie ein Diskussionsforum hatten, Sie möchten zu verhindern, dass `"bob@example.com"` aus der Registrierung als `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="7648a-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="7648a-132">Ohne e-Mail-Bestätigung `"joe@contoso.com"` unerwünschte e-Mail-Adresse Ihrer app abrufen konnte.</span><span class="sxs-lookup"><span data-stu-id="7648a-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="7648a-133">Angenommen, das Bob versehentlich als registriert `"bib@example.com"` und noch nicht bemerkt, er wäre Kennwort wiederherstellen zu verwenden, da die app nicht seine richtige e-Mail-Nachricht nicht.</span><span class="sxs-lookup"><span data-stu-id="7648a-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="7648a-134">E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und nicht bieten Schutz vor bestimmt Spammern, sie haben viele funktionierenden e-Mail-Aliase, die sie verwenden können, um zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="7648a-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="7648a-135">Im Allgemeinen möchten Sie verhindern, dass neue Benutzer keine Daten zu Ihrer Website veröffentlichen, bevor sie per e-Mail, eine SMS oder einen anderen Mechanismus bestätigt wurden.</span><span class="sxs-lookup"><span data-stu-id="7648a-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="7648a-136">In den folgenden Abschnitten werden wir Aktivieren von e-Mail-Bestätigung und ändern Sie den Code, um zu verhindern, dass die neu registrierte Benutzer anmelden, bis ihre e-Mail-Adresse bestätigt wurde.</span><span class="sxs-lookup"><span data-stu-id="7648a-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="7648a-137">Verknüpfen mit SendGrid</span><span class="sxs-lookup"><span data-stu-id="7648a-137">Hook up SendGrid</span></span>

<span data-ttu-id="7648a-138">Obwohl dieses Tutorial nur das zeigt Hinzufügen von e-Mail-Benachrichtigung über [SendGrid](http://sendgrid.com/), senden Sie eine e-Mail mit SMTP und andere Mechanismen (finden Sie unter [Zusatzressourcen](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="7648a-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="7648a-139">Geben Sie in der Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="7648a-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="7648a-140">Wechseln Sie zu der [Anmeldeseite von Azure-SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) und registrieren Sie sich für ein kostenloses SendGrid-Konto.</span><span class="sxs-lookup"><span data-stu-id="7648a-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="7648a-141">Konfigurieren von SendGrid durch Hinzufügen von Code ähnlich dem folgenden in *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="7648a-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="7648a-142">Sie müssen beim Hinzufügen der Folgendes enthält:</span><span class="sxs-lookup"><span data-stu-id="7648a-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="7648a-143">In diesem Beispiel der Einfachheit halber speichern wir die app-Einstellungen in der *"Web.config"* Datei:</span><span class="sxs-lookup"><span data-stu-id="7648a-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="7648a-144">Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern.</span><span class="sxs-lookup"><span data-stu-id="7648a-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="7648a-145">Das Konto und die Anmeldeinformationen werden in der AppSetting gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7648a-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="7648a-146">In Azure können sicher gespeichert werden diese Werte auf die **[konfigurieren](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Registerkarte im Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="7648a-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="7648a-147">Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="7648a-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="7648a-148">Aktivieren von e-Mail-Bestätigung im Kontocontroller</span><span class="sxs-lookup"><span data-stu-id="7648a-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="7648a-149">Überprüfen Sie die *Views\Account\ConfirmEmail.cshtml* Datei hat die richtigen Razor-Syntax.</span><span class="sxs-lookup"><span data-stu-id="7648a-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="7648a-150">(Der @-Zeichen in der ersten Zeile möglicherweise nicht vorhanden.</span><span class="sxs-lookup"><span data-stu-id="7648a-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="7648a-151">)</span><span class="sxs-lookup"><span data-stu-id="7648a-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="7648a-152">Führen Sie die app aus, und klicken Sie auf den Link "Register".</span><span class="sxs-lookup"><span data-stu-id="7648a-152">Run the app and click the Register link.</span></span> <span data-ttu-id="7648a-153">Nachdem Sie das Registrierungsformular senden, sind Sie angemeldet.</span><span class="sxs-lookup"><span data-stu-id="7648a-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="7648a-154">Überprüfen Sie Ihre e-Mail-Konto, und klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="7648a-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="7648a-155">E-Mail-Bestätigung vor der Anmeldung verlangen</span><span class="sxs-lookup"><span data-stu-id="7648a-155">Require email confirmation before log in</span></span>

<span data-ttu-id="7648a-156">Derzeit verwendet werden, wenn ein Benutzer das Registrierungsformular abgeschlossen ist, sind sie angemeldet.</span><span class="sxs-lookup"><span data-stu-id="7648a-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="7648a-157">Im Allgemeinen möchten Sie ihre e-Mail-Adresse zu bestätigen, bevor Sie diese Anmeldung.</span><span class="sxs-lookup"><span data-stu-id="7648a-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="7648a-158">Im folgenden Abschnitt ändern wir den Code, um neue Benutzer, um eine bestätigte e-Mail-Adresse haben, bevor sie angemeldet sind (authentifiziert) erforderlich.</span><span class="sxs-lookup"><span data-stu-id="7648a-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="7648a-159">Update der `HttpPost Register` -Methode mit dem folgenden hervorgehobenen Änderungen vor:</span><span class="sxs-lookup"><span data-stu-id="7648a-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="7648a-160">Durch Auskommentieren der `SignInAsync` -Methode, der Benutzer wird nicht durch die Registrierung angemeldet werden.</span><span class="sxs-lookup"><span data-stu-id="7648a-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="7648a-161">Die `TempData["ViewBagLink"] = callbackUrl;` Zeile kann nicht verwendet werden [Debuggen Sie die Anwendung](#dbg) und Testen Sie die Registrierung ohne e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="7648a-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="7648a-162">`ViewBag.Message` wird verwendet, die Confirm-Anweisungen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7648a-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="7648a-163">Die [Beispiel herunterladen](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) enthält Code, um e-Mail-Bestätigung zu testen, ohne das Einrichten von e-Mail-Adresse und kann auch zum Debuggen der Anwendung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="7648a-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="7648a-164">Erstellen Sie eine `Views\Shared\Info.cshtml` Datei, und fügen Sie das folgende Razor-Markup hinzu:</span><span class="sxs-lookup"><span data-stu-id="7648a-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="7648a-165">Hinzufügen der [Authorize-Attribut](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) auf die `Contact` Aktionsmethode des Home-Controllers.</span><span class="sxs-lookup"><span data-stu-id="7648a-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="7648a-166">Klicken Sie auf die **wenden Sie sich an** Link, um zu überprüfen, ob anonyme Benutzer keinen Zugriff haben und authentifizierte Benutzer Zugriff haben.</span><span class="sxs-lookup"><span data-stu-id="7648a-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="7648a-167">Sie müssen auch aktualisieren die `HttpPost Login` Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="7648a-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="7648a-168">Update der *Views\Shared\Error.cshtml* können Sie die Fehlermeldung anzeigen:</span><span class="sxs-lookup"><span data-stu-id="7648a-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="7648a-169">Löschen von Konten in der **"aspnetusers"** Tabelle, die den e-Mail-Alias enthalten, Sie testen möchten.</span><span class="sxs-lookup"><span data-stu-id="7648a-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="7648a-170">Führen Sie die app aus, und stellen Sie sicher, dass Sie können sich nicht anmelden, bis Sie Ihre e-Mail-Adresse bestätigt haben.</span><span class="sxs-lookup"><span data-stu-id="7648a-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="7648a-171">Nachdem Sie Ihre e-Mail-Adresse bestätigt haben, klicken Sie auf die **wenden Sie sich an** Link.</span><span class="sxs-lookup"><span data-stu-id="7648a-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="7648a-172">Zurücksetzen des Kennworts/Wiederherstellung</span><span class="sxs-lookup"><span data-stu-id="7648a-172">Password recovery/reset</span></span>

<span data-ttu-id="7648a-173">Entfernen Sie die Kommentarzeichen aus der `HttpPost ForgotPassword` Aktionsmethode im Kontocontroller-:</span><span class="sxs-lookup"><span data-stu-id="7648a-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="7648a-174">Entfernen Sie die Kommentarzeichen aus der `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in die *Views\Account\Login.cshtml* Razor-Ansichtsdatei:</span><span class="sxs-lookup"><span data-stu-id="7648a-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="7648a-175">Die Anmeldeseite geöffnet wird nun einen Link zum Zurücksetzen des Kennworts angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7648a-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="7648a-176">Bestätigungslink erneut-e-Mail</span><span class="sxs-lookup"><span data-stu-id="7648a-176">Resend email confirmation link</span></span>

<span data-ttu-id="7648a-177">Sobald ein Benutzer ein neues lokales Konto erstellt wird, werden sie einen Link zur Bestätigung per e-Mail gesendet, die, den Sie für erforderlich sind, verwenden, bevor sie sich anmelden können.</span><span class="sxs-lookup"><span data-stu-id="7648a-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="7648a-178">Wenn der Benutzer versehentlich die Bestätigungs-e-Mail löscht oder die e-Mail nicht ankommt, benötigen sie den Link zur Bestätigung erneut gesendet.</span><span class="sxs-lookup"><span data-stu-id="7648a-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="7648a-179">Die folgenden codeänderungen zeigen, wie dies zu ermöglichen.</span><span class="sxs-lookup"><span data-stu-id="7648a-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="7648a-180">Fügen Sie die folgende Hilfsmethode zum unteren Rand der *controllers\accountcontroller* Datei:</span><span class="sxs-lookup"><span data-stu-id="7648a-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="7648a-181">Aktualisieren Sie die Register-Methode, um die neue Hilfe verwenden:</span><span class="sxs-lookup"><span data-stu-id="7648a-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="7648a-182">Aktualisieren der Login-Methode, um das Kennwort erneut zu senden, wenn das Benutzerkonto nicht bestätigt wurde:</span><span class="sxs-lookup"><span data-stu-id="7648a-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7648a-183">Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung</span><span class="sxs-lookup"><span data-stu-id="7648a-183">Combine social and local login accounts</span></span>

<span data-ttu-id="7648a-184">Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link.</span><span class="sxs-lookup"><span data-stu-id="7648a-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7648a-185">In der folgenden Reihenfolge **RickAndMSFT@gmail.com** wird zuerst als einen lokalen Benutzernamen erstellt, aber Sie können das Konto als eine soziale Anmeldung erste erstellen und dann fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="7648a-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="7648a-186">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="7648a-186">Click on the **Manage** link.</span></span> <span data-ttu-id="7648a-187">Beachten Sie die **externer Anmeldungen: 0** mit diesem Konto verknüpft.</span><span class="sxs-lookup"><span data-stu-id="7648a-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="7648a-188">Klicken Sie auf den Link zu einem anderen Protokoll im Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="7648a-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="7648a-189">Wurden die beiden Konten zusammengefasst, die mit Konten anmelden können.</span><span class="sxs-lookup"><span data-stu-id="7648a-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="7648a-190">Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls der sozialen Anmeldung Authentication-Dienst nicht unterbrochen ist oder wahrscheinlich haben sie Zugriff auf ihre Konten sozialer Netzwerke verloren.</span><span class="sxs-lookup"><span data-stu-id="7648a-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="7648a-191">In der folgenden Abbildung, Tom ist eine soziale Anmeldung (den Sie können die **externer Anmeldungen: 1** auf der Seite angezeigt).</span><span class="sxs-lookup"><span data-stu-id="7648a-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="7648a-192">Durch Klicken auf **wählen Sie ein Kennwort** ermöglicht es Ihnen, ein lokales Protokoll hinzufügen, mit dem gleichen Konto verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="7648a-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="7648a-193">E-Mail-Bestätigung ausführlicher</span><span class="sxs-lookup"><span data-stu-id="7648a-193">Email confirmation in more depth</span></span>

<span data-ttu-id="7648a-194">Meine Tutorial [Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wechselt in diesem Thema mit weiteren Details.</span><span class="sxs-lookup"><span data-stu-id="7648a-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="7648a-195">Debuggen der app</span><span class="sxs-lookup"><span data-stu-id="7648a-195">Debugging the app</span></span>

<span data-ttu-id="7648a-196">Wenn Sie eine e-Mail mit der Link nicht erhalten:</span><span class="sxs-lookup"><span data-stu-id="7648a-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="7648a-197">Überprüfen Sie Ihren Junk-e- bzw. Spam-Ordner.</span><span class="sxs-lookup"><span data-stu-id="7648a-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="7648a-198">Melden Sie sich bei Ihrem SendGrid-Konto, und klicken Sie auf die [-e-Mail-Aktivitätslink](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="7648a-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="7648a-199">Um den Link für die Überprüfung ohne e-Mail-Konto testen möchten, laden die [abgeschlossene Beispiel](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="7648a-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="7648a-200">Der Link zur Bestätigung und Bestätigungscodes werden auf der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="7648a-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="7648a-201">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7648a-201">Additional Resources</span></span>

- [<span data-ttu-id="7648a-202">Links zu ASP.NET Identity – empfohlene Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7648a-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="7648a-203">[Kontobestätigung und Kennwortwiederherstellung in ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) wird ausführlicher auf die Wiederherstellungs- und Konto die Bestätigung des Kennworts.</span><span class="sxs-lookup"><span data-stu-id="7648a-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="7648a-204">[MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) in diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-app mit Facebook und Google OAuth 2-Autorisierung zu schreiben.</span><span class="sxs-lookup"><span data-stu-id="7648a-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="7648a-205">Außerdem wird veranschaulicht, mit der Identity-Datenbank zusätzliche Daten hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7648a-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="7648a-206">[Bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="7648a-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="7648a-207">In diesem Tutorial wird Azure-Bereitstellung hinzugefügt. So sichern Sie Ihre app mit Rollen, wie Sie die Mitgliedschafts-API zu verwenden, um zusätzliche Sicherheitsfeatures, Benutzer und Rollen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="7648a-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="7648a-208">Beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt</span><span class="sxs-lookup"><span data-stu-id="7648a-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="7648a-209">Erstellen die app in Facebook, und verbinden die app auf das Projekt</span><span class="sxs-lookup"><span data-stu-id="7648a-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="7648a-210">Einrichten von SSL im Projekt</span><span class="sxs-lookup"><span data-stu-id="7648a-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
