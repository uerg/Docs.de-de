---
title: Kontobestätigung und kennwortwiederherstellung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen einer ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung.
ms.author: riande
ms.date: 7/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: af3ba8b27148d94c4a905c688b8165f49b9f2927
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095745"
---
::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="aefd1-103">Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die ASP.NET Core 1.1 und Version 2.1.</span><span class="sxs-lookup"><span data-stu-id="aefd1-103">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core 1.1 and 2.1 version.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="aefd1-104">Kontobestätigung und kennwortwiederherstellung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aefd1-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="aefd1-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="aefd1-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="aefd1-106">Dieses Tutorial veranschaulicht das Erstellen eine ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung.</span><span class="sxs-lookup"><span data-stu-id="aefd1-106">This tutorial shows how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="aefd1-107">Dieses Tutorial ist der **nicht** ein Thema ab.</span><span class="sxs-lookup"><span data-stu-id="aefd1-107">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="aefd1-108">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="aefd1-108">You should be familiar with:</span></span>

* [<span data-ttu-id="aefd1-109">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aefd1-109">ASP.NET Core</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="aefd1-110">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="aefd1-110">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="aefd1-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="aefd1-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<!-- see C:/Dropbox/wrk/Code/SendGridConsole/Program.cs -->

## <a name="prerequisites"></a><span data-ttu-id="aefd1-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="aefd1-112">Prerequisites</span></span>

<span data-ttu-id="aefd1-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span><span class="sxs-lookup"><span data-stu-id="aefd1-113">[!INCLUDE [](~/includes/2.1-SDK.md) [](~/includes/2.1-SDK.md)]</span></span>

## <a name="create-a-web--app-and-scaffold-identity"></a><span data-ttu-id="aefd1-114">Erstellen einer Web-app und das Gerüst für Identity</span><span class="sxs-lookup"><span data-stu-id="aefd1-114">Create a web  app and scaffold Identity</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aefd1-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aefd1-115">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="aefd1-116">Erstellen Sie in Visual Studio ein neues **Webanwendung** Projekt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-116">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="aefd1-117">Wählen Sie **ASP.NET Core 2.1**.</span><span class="sxs-lookup"><span data-stu-id="aefd1-117">Select **ASP.NET Core 2.1**.</span></span>
* <span data-ttu-id="aefd1-118">Behalten Sie den Standardwert **Authentifizierung** festgelegt **keine Authentifizierung**.</span><span class="sxs-lookup"><span data-stu-id="aefd1-118">Keep the default **Authentication** set to **No Authentication**.</span></span> <span data-ttu-id="aefd1-119">Authentifizierung wird im nächsten Schritt hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-119">Authentication is added in the next step.</span></span>

<span data-ttu-id="aefd1-120">Im nächsten Schritt:</span><span class="sxs-lookup"><span data-stu-id="aefd1-120">In the next step:</span></span>

* <span data-ttu-id="aefd1-121">Legen Sie auf die Layoutseite *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="aefd1-121">Set the layout page to *~/Pages/Shared/_Layout.cshtml*</span></span>
* <span data-ttu-id="aefd1-122">Wählen Sie *Konto/registrieren*</span><span class="sxs-lookup"><span data-stu-id="aefd1-122">Select *Account/Register*</span></span>
* <span data-ttu-id="aefd1-123">Erstellen Sie ein neues **Datenkontextklasse**</span><span class="sxs-lookup"><span data-stu-id="aefd1-123">Create a new **Data context class**</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="aefd1-124">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="aefd1-124">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet new webapp -o WebPWrecover
cd WebPWrecover
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
dotnet aspnet-codegenerator identity -fi Account.Register -dc WebPWrecover.Models.WebPWrecoverContext
dotnet ef migrations add CreateIdentitySchema
dotnet ef database drop -f
dotnet ef database update
dotnet build
```

<span data-ttu-id="aefd1-125">Führen Sie `dotnet aspnet-codegenerator identity --help` um Hilfe zu den gerüstbautool erhalten.</span><span class="sxs-lookup"><span data-stu-id="aefd1-125">Run `dotnet aspnet-codegenerator identity --help` to get help on the scaffolding tool.</span></span>

------

<span data-ttu-id="aefd1-126">Befolgen Sie die Anweisungen in [Aktivieren der Authentifizierung](xref:security/authentication/scaffold-identity#useauthentication):</span><span class="sxs-lookup"><span data-stu-id="aefd1-126">Follow the instructions in [Enable authentication](xref:security/authentication/scaffold-identity#useauthentication):</span></span>

* <span data-ttu-id="aefd1-127">Hinzufügen `app.UseAuthentication();` auf `Startup.Configure`</span><span class="sxs-lookup"><span data-stu-id="aefd1-127">Add `app.UseAuthentication();` to `Startup.Configure`</span></span>
* <span data-ttu-id="aefd1-128">Hinzufügen `<partial name="_LoginPartial" />` zur Layoutdatei.</span><span class="sxs-lookup"><span data-stu-id="aefd1-128">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="aefd1-129">Testen Sie die Registrierung neuer Benutzer</span><span class="sxs-lookup"><span data-stu-id="aefd1-129">Test new user registration</span></span>

<span data-ttu-id="aefd1-130">Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="aefd1-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="aefd1-131">Die ausschließliche Überprüfung der auf die e-Mail-Adresse an diesem Punkt ist, mit der [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut.</span><span class="sxs-lookup"><span data-stu-id="aefd1-131">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="aefd1-132">Nach der Übermittlung der Registrierungs, werden Sie bei der app angemeldet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-132">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="aefd1-133">Weiter unten in diesem Tutorial wird der Code aktualisiert, damit neue Benutzer können sich nicht anmelden, bis ihre e-Mail-Adresse überprüft wird.</span><span class="sxs-lookup"><span data-stu-id="aefd1-133">Later in the tutorial, the code is updated so new users can't log in until their email is validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="aefd1-134">Abrufen der Identität-Datenbank</span><span class="sxs-lookup"><span data-stu-id="aefd1-134">View the Identity database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aefd1-135">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aefd1-135">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="aefd1-136">Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="aefd1-136">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="aefd1-137">Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer-13)**.</span><span class="sxs-lookup"><span data-stu-id="aefd1-137">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="aefd1-138">Mit der rechten Maustaste auf **Dbo. "Aspnetusers"** > **zeigen Daten**:</span><span class="sxs-lookup"><span data-stu-id="aefd1-138">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Über das Kontextmenü für die Tabelle "aspnetusers" im Objekt-Explorer von SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="aefd1-140">Beachten Sie die Tabelle `EmailConfirmed` Feld `False`.</span><span class="sxs-lookup"><span data-stu-id="aefd1-140">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="aefd1-141">Sie möchten möglicherweise verwenden Sie diese e-Mail erneut im nächsten Schritt, wenn die app eine Bestätigung per e-Mail sendet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-141">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="aefd1-142">Mit der rechten Maustaste auf die Zeile, und wählen **löschen**.</span><span class="sxs-lookup"><span data-stu-id="aefd1-142">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="aefd1-143">Löschen den e-Mail-Alias erleichtert es in den folgenden Schritten.</span><span class="sxs-lookup"><span data-stu-id="aefd1-143">Deleting the email alias makes it easier in the following steps.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="aefd1-144">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="aefd1-144">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="aefd1-145">Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anweisungen zum Anzeigen der SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="aefd1-145">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

------

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="aefd1-146">E-Mail-Bestätigung erforderlich</span><span class="sxs-lookup"><span data-stu-id="aefd1-146">Require email confirmation</span></span>

<span data-ttu-id="aefd1-147">Es ist eine bewährte Methode, die e-Mail-Adresse einer neuen benutzerregistrierung zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-147">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="aefd1-148">E-Mail-Bestätigung können, um zu überprüfen, ob sie sind eine andere Person keinen Identitätswechsel (d. h. sie noch nicht registriert mit einer anderen Person für den e-Mail-Adresse).</span><span class="sxs-lookup"><span data-stu-id="aefd1-148">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="aefd1-149">Angenommen, Sie haben ein Diskussionsforum, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="aefd1-149">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="aefd1-150">Ohne e-Mail-Bestätigung "nolivetto@contoso.com" könnte unerwünschte e-Mail-Adresse aus Ihrer app erhalten.</span><span class="sxs-lookup"><span data-stu-id="aefd1-150">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="aefd1-151">Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" und noch nicht bemerkt, dass die falsche Schreibweise von "Yli".</span><span class="sxs-lookup"><span data-stu-id="aefd1-151">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="aefd1-152">Sie wäre nicht kennwortwiederherstellung zu verwenden, da die app die korrekte e-Mail-Adresse besitzt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-152">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="aefd1-153">E-Mail-Bestätigung enthält nur begrenzten Schutz von Bots.</span><span class="sxs-lookup"><span data-stu-id="aefd1-153">Email confirmation provides limited protection from bots.</span></span> <span data-ttu-id="aefd1-154">E-Mail-Bestätigung stellt keinen Schutz vor böswilligen Benutzern viele e-Mail-Konten bereit.</span><span class="sxs-lookup"><span data-stu-id="aefd1-154">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="aefd1-155">Im Allgemeinen möchten Sie verhindern, dass neue Benutzer keine Daten zu Ihrer Website veröffentlichen, bevor sie eine e-Mail bestätigte haben.</span><span class="sxs-lookup"><span data-stu-id="aefd1-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="aefd1-156">Update *Areas/Identity/IdentityHostingStartup.cs* eine bestätigte e-Mail-Adresse erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="aefd1-156">Update *Areas/Identity/IdentityHostingStartup.cs*  to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/IdentityHostingStartup.cs?name=snippet1&highlight=10-13)]

<span data-ttu-id="aefd1-157">`config.SignIn.RequireConfirmedEmail = true;` verhindert, dass registrierte Benutzer anmelden, bis ihre-e-Mail bestätigt ist.</span><span class="sxs-lookup"><span data-stu-id="aefd1-157">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="aefd1-158">Konfigurieren von e-Mail-Anbieter</span><span class="sxs-lookup"><span data-stu-id="aefd1-158">Configure email provider</span></span>

<span data-ttu-id="aefd1-159">In diesem Tutorial [SendGrid](https://sendgrid.com) wird verwendet, um e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-159">In this tutorial, [SendGrid](https://sendgrid.com) is used to send email.</span></span> <span data-ttu-id="aefd1-160">Sie benötigen eine SendGrid-Konto und einen Schlüssel zum Senden von e-Mails.</span><span class="sxs-lookup"><span data-stu-id="aefd1-160">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="aefd1-161">Sie können andere e-Mail-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-161">You can use other email providers.</span></span> <span data-ttu-id="aefd1-162">ASP.NET Core 2.x enthält `System.Net.Mail`, wodurch Sie zum Senden von e-Mail-Adresse aus Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="aefd1-162">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="aefd1-163">Es wird empfohlen, dass Sie über SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-163">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="aefd1-164">SMTP ist schwierig, sichere und ordnungsgemäß eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-164">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="aefd1-165">Die [optionsmuster](xref:fundamentals/configuration/options) wird verwendet, um die benutzereinstellungen für Konto und den Schlüssel zugreifen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-165">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="aefd1-166">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="aefd1-166">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="aefd1-167">Erstellen Sie eine Klasse, um den Schlüssel für sichere e-Mails abrufen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-167">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="aefd1-168">In diesem Beispiel erstellen *Services/AuthMessageSenderOptions.cs*:</span><span class="sxs-lookup"><span data-stu-id="aefd1-168">For this sample, create *Services/AuthMessageSenderOptions.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/AuthMessageSenderOptions.cs?name=snippet1)]

#### <a name="configure-sendgrid-user-secrets"></a><span data-ttu-id="aefd1-169">Konfigurieren von SendGrid benutzergeheimnisse</span><span class="sxs-lookup"><span data-stu-id="aefd1-169">Configure SendGrid user secrets</span></span>

<span data-ttu-id="aefd1-170">Fügen Sie einen eindeutigen `<UserSecretsId>` Wert der `<PropertyGroup>` -Element der Projektdatei:</span><span class="sxs-lookup"><span data-stu-id="aefd1-170">Add a unique `<UserSecretsId>` value to the `<PropertyGroup>` element of the project file:</span></span>

[!code-xml[](accconfirm/sample/WebPWrecover21/WebPWrecover.csproj?highlight=5)]

<span data-ttu-id="aefd1-171">Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="aefd1-171">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="aefd1-172">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="aefd1-172">For example:</span></span>

```console
C:/WebAppl>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="aefd1-173">Auf Windows, speichert Secret Manager-Schlüssel/Wert-Paare in einem *secrets.json* Datei die `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="aefd1-173">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="aefd1-174">Den Inhalt der *secrets.json* Datei sind nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-174">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="aefd1-175">Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)</span><span class="sxs-lookup"><span data-stu-id="aefd1-175">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="install-sendgrid"></a><span data-ttu-id="aefd1-176">Installieren von SendGrid</span><span class="sxs-lookup"><span data-stu-id="aefd1-176">Install SendGrid</span></span>

<span data-ttu-id="aefd1-177">In diesem Tutorial wird gezeigt, wie Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mails über SMTP und andere Mechanismen senden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-177">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="aefd1-178">Installieren Sie die `SendGrid` NuGet-Paket:</span><span class="sxs-lookup"><span data-stu-id="aefd1-178">Install the `SendGrid` NuGet package:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="aefd1-179">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="aefd1-179">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="aefd1-180">Geben Sie über die Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="aefd1-180">From the Package Manager Console, enter the following command:</span></span>

``` PMC
Install-Package SendGrid
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="aefd1-181">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="aefd1-181">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="aefd1-182">Geben Sie in der Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="aefd1-182">From the console, enter the following command:</span></span>

```cli
dotnet add package SendGrid
```

------

<span data-ttu-id="aefd1-183">Finden Sie unter [kostenlos erste Schritte mit SendGrid](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="aefd1-183">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>
### <a name="implement-iemailsender"></a><span data-ttu-id="aefd1-184">Implementieren von IEmailSender</span><span class="sxs-lookup"><span data-stu-id="aefd1-184">Implement IEmailSender</span></span>

<span data-ttu-id="aefd1-185">Das implementieren `IEmailSender`, erstellen Sie *Services/EmailSender.cs* mit ähnlich dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="aefd1-185">To Implement `IEmailSender`, create *Services/EmailSender.cs* with code similar to the following:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Services/EmailSender.cs)]

### <a name="configure-startup-to-support-email"></a><span data-ttu-id="aefd1-186">Konfigurieren Sie beim Start zur Unterstützung von e-Mail-Adresse</span><span class="sxs-lookup"><span data-stu-id="aefd1-186">Configure startup to support email</span></span>

<span data-ttu-id="aefd1-187">Fügen Sie den folgenden Code der `ConfigureServices` -Methode in der die *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="aefd1-187">Add the following code to the `ConfigureServices` method in the *Startup.cs* file:</span></span>

* <span data-ttu-id="aefd1-188">Hinzufügen `EmailSender` als singletondienst.</span><span class="sxs-lookup"><span data-stu-id="aefd1-188">Add `EmailSender` as a singleton service.</span></span>
* <span data-ttu-id="aefd1-189">Registrieren der `AuthMessageSenderOptions` konfigurationsinstanz.</span><span class="sxs-lookup"><span data-stu-id="aefd1-189">Register the `AuthMessageSenderOptions` configuration instance.</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Startup.cs?name=snippet2&highlight=12-99)]

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="aefd1-190">Konto-Bestätigung und -Wiederherstellung aktivieren</span><span class="sxs-lookup"><span data-stu-id="aefd1-190">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="aefd1-191">Die Vorlage weist den Code für die Konto-Bestätigung und -Wiederherstellung.</span><span class="sxs-lookup"><span data-stu-id="aefd1-191">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="aefd1-192">Suchen der `OnPostAsync` -Methode in der *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="aefd1-192">Find the `OnPostAsync` method in *Areas/Identity/Pages/Account/Register.cshtml.cs*.</span></span>

<span data-ttu-id="aefd1-193">Verhindern Sie, dass neu registrierte Benutzern automatisch durch die Sie die folgende Zeile Kommentierung angemeldet werden:</span><span class="sxs-lookup"><span data-stu-id="aefd1-193">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="aefd1-194">Die complete-Methode wird mit der geänderten Zeile hervorgehoben dargestellt:</span><span class="sxs-lookup"><span data-stu-id="aefd1-194">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover21/Areas/Identity/Pages/Account/Register.cshtml.cs?highlight=22&name=snippet_Register)]

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="aefd1-195">Registrieren und bestätigen Sie die e-Mail-Adresse und die kennwortzurücksetzung</span><span class="sxs-lookup"><span data-stu-id="aefd1-195">Register, confirm email, and reset password</span></span>

<span data-ttu-id="aefd1-196">Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-196">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="aefd1-197">Die app ausführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="aefd1-197">Run the app and register a new user</span></span>

  ![Web-Anwendungsansicht Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="aefd1-199">Überprüfen Sie Ihre e-Mail-Adresse für die der Link für die Bestätigung.</span><span class="sxs-lookup"><span data-stu-id="aefd1-199">Check your email for the account confirmation link.</span></span> <span data-ttu-id="aefd1-200">Finden Sie unter [Debuggen-e-Mail](#debug) sollten Sie die e-Mail nicht erhalten.</span><span class="sxs-lookup"><span data-stu-id="aefd1-200">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="aefd1-201">Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-201">Click the link to confirm your email.</span></span>
* <span data-ttu-id="aefd1-202">Melden Sie sich mit Ihren e-Mail-Adresse und Ihr Kennwort.</span><span class="sxs-lookup"><span data-stu-id="aefd1-202">Log in with your email and password.</span></span>
* <span data-ttu-id="aefd1-203">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="aefd1-203">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="aefd1-204">Anzeigen der Seite "verwalten"</span><span class="sxs-lookup"><span data-stu-id="aefd1-204">View the manage page</span></span>

<span data-ttu-id="aefd1-205">Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzername](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="aefd1-205">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="aefd1-206">Möglicherweise müssen Sie die Navigationsleiste, um Benutzername zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="aefd1-206">You might need to expand the navbar to see user name.</span></span>

![Navigationsleiste](accconfirm/_static/x.png)

<span data-ttu-id="aefd1-208">Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-208">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="aefd1-209">Die **-e-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mail-Adresse wurde bestätigt.</span><span class="sxs-lookup"><span data-stu-id="aefd1-209">The **Email** shows a check box indicating the email has been confirmed.</span></span>

### <a name="test-password-reset"></a><span data-ttu-id="aefd1-210">Test Zurücksetzen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="aefd1-210">Test password reset</span></span>

* <span data-ttu-id="aefd1-211">Wenn Sie angemeldet sind, wählen Sie **Logout**.</span><span class="sxs-lookup"><span data-stu-id="aefd1-211">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="aefd1-212">Wählen Sie die **melden Sie sich bei** verknüpfen, und wählen Sie die **haben Ihr Kennwort vergessen?** Link.</span><span class="sxs-lookup"><span data-stu-id="aefd1-212">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="aefd1-213">Geben Sie die e-Mail-Adresse, die Sie verwendet, um das Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="aefd1-213">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="aefd1-214">Es wird eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts gesendet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-214">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="aefd1-215">Überprüfen Sie Ihre e-Mail-Adresse ein, und klicken Sie auf den Link zum Zurücksetzen Ihres Kennworts.</span><span class="sxs-lookup"><span data-stu-id="aefd1-215">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="aefd1-216">Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie sich mit Ihrem e-Mail-Adresse und das neue Kennwort anmelden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-216">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="aefd1-217">Debuggen von e-Mail-Adresse</span><span class="sxs-lookup"><span data-stu-id="aefd1-217">Debug email</span></span>

<span data-ttu-id="aefd1-218">Wenn Sie e-Mail-Adresse funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="aefd1-218">If you can't get email working:</span></span>

* <span data-ttu-id="aefd1-219">Festlegen eines Haltepunkts im `EmailSender.Execute` überprüfen `SendGridClient.SendEmailAsync` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="aefd1-219">Set a breakpoint in `EmailSender.Execute` to verify `SendGridClient.SendEmailAsync` is called.</span></span>
* <span data-ttu-id="aefd1-220">Erstellen Sie eine [Konsolen-app zum Senden von e-Mails](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) mit ähnlichen Code `EmailSender.Execute`.</span><span class="sxs-lookup"><span data-stu-id="aefd1-220">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html) using similar code to `EmailSender.Execute`.</span></span>
* <span data-ttu-id="aefd1-221">Überprüfen Sie die [-e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.</span><span class="sxs-lookup"><span data-stu-id="aefd1-221">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="aefd1-222">Überprüfen Sie Ihren Spam-Ordner.</span><span class="sxs-lookup"><span data-stu-id="aefd1-222">Check your spam folder.</span></span>
* <span data-ttu-id="aefd1-223">Versuchen Sie es einer anderen e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail usw.)</span><span class="sxs-lookup"><span data-stu-id="aefd1-223">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="aefd1-224">Versuchen Sie es an andere e-Mail-Konten gesendet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-224">Try sending to different email accounts.</span></span>

<span data-ttu-id="aefd1-225">**Eine bewährte Sicherheitsmethode** besteht darin, **nicht** produktionsgeheimnisse in Test- und entwicklungsumgebungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-225">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="aefd1-226">Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-Geheimnisse Anwendungseinstellungen im Azure-Web-App-Portal festlegen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-226">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="aefd1-227">Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="aefd1-227">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="aefd1-228">Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung</span><span class="sxs-lookup"><span data-stu-id="aefd1-228">Combine social and local login accounts</span></span>

<span data-ttu-id="aefd1-229">Um diesen Abschnitt abgeschlossen haben, müssen Sie zuerst einen externer Authentifizierungsanbieter aktivieren.</span><span class="sxs-lookup"><span data-stu-id="aefd1-229">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="aefd1-230">Finden Sie unter [Facebook, Google und externe Anbieter Authentifizierung](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="aefd1-230">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="aefd1-231">Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link.</span><span class="sxs-lookup"><span data-stu-id="aefd1-231">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="aefd1-232">In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird zuerst als eine lokale Anmeldung; erstellt, Sie können jedoch zunächst erstellen Sie das Konto als Anmeldedaten eines sozialen Netzwerks, und fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="aefd1-232">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web-Anwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

<span data-ttu-id="aefd1-234">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="aefd1-234">Click on the **Manage** link.</span></span> <span data-ttu-id="aefd1-235">Beachten Sie die 0 externe (Anmeldungen per sozialem Netzwerk) mit diesem Konto verknüpft.</span><span class="sxs-lookup"><span data-stu-id="aefd1-235">Note the 0 external (social logins) associated with this account.</span></span>

![Verwalten von anzeigen](accconfirm/_static/manage.png)

<span data-ttu-id="aefd1-237">Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="aefd1-237">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="aefd1-238">In der folgenden Abbildung ist die Facebook dem externen Authentifizierungsanbieter:</span><span class="sxs-lookup"><span data-stu-id="aefd1-238">In the following image, Facebook is the external authentication provider:</span></span>

![Verwalten Sie Ihre Facebook auflisten externer Anmeldungen-Ansicht](accconfirm/_static/fb.png)

<span data-ttu-id="aefd1-240">Die beiden Konten wurden kombiniert.</span><span class="sxs-lookup"><span data-stu-id="aefd1-240">The two accounts have been combined.</span></span> <span data-ttu-id="aefd1-241">Sie können mit beiden Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="aefd1-241">You are able to log on with either account.</span></span> <span data-ttu-id="aefd1-242">Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls ihre sozialen Authentifizierungsdiensts ausgefallen ist oder eher sie den Zugriff auf ihre Konten sozialer Netzwerke verlieren.</span><span class="sxs-lookup"><span data-stu-id="aefd1-242">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="aefd1-243">Kontobestätigung zu aktivieren, nachdem Sie einen Standort der Benutzer enthält</span><span class="sxs-lookup"><span data-stu-id="aefd1-243">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="aefd1-244">Aktivierung kontobestätigung auf einer Website für Benutzer, alle vorhandenen Benutzer sperren.</span><span class="sxs-lookup"><span data-stu-id="aefd1-244">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="aefd1-245">Vorhandene Benutzer werden gesperrt, da es sich bei ihrem Konto bestätigt werden nicht.</span><span class="sxs-lookup"><span data-stu-id="aefd1-245">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="aefd1-246">Um vorhandene Benutzersperre zu umgehen, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="aefd1-246">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="aefd1-247">Ändern der Datenbank markieren Sie alle vorhandenen Benutzer als bestätigt wird.</span><span class="sxs-lookup"><span data-stu-id="aefd1-247">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="aefd1-248">Bestätigen Sie die vorhandenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="aefd1-248">Confirm exiting users.</span></span> <span data-ttu-id="aefd1-249">Z. B. Batch-Send-e-Mails mit Bestätigung Links.</span><span class="sxs-lookup"><span data-stu-id="aefd1-249">For example, batch-send emails with confirmation links.</span></span>

::: moniker-end
