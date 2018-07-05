---
title: Kontobestätigung und kennwortwiederherstellung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen einer ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung.
ms.author: riande
ms.date: 2/11/2018
uid: security/authentication/accconfirm
ms.openlocfilehash: 12265903f60ff6d62befc445434db025c244c178
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803271"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="7124c-103">Kontobestätigung und kennwortwiederherstellung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7124c-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="7124c-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="7124c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="7124c-105">In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET Core-app mit e-Mail-Bestätigung und kennwortzurücksetzung erstellen.</span><span class="sxs-lookup"><span data-stu-id="7124c-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="7124c-106">Dieses Tutorial ist der **nicht** ein Thema ab.</span><span class="sxs-lookup"><span data-stu-id="7124c-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="7124c-107">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="7124c-107">You should be familiar with:</span></span>

* [<span data-ttu-id="7124c-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7124c-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="7124c-109">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="7124c-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="7124c-110">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7124c-110">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="7124c-111">Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die Versionen 1.1 von ASP.NET Core MVC und 2.x.</span><span class="sxs-lookup"><span data-stu-id="7124c-111">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7124c-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="7124c-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="7124c-113">Erstellen Sie ein neues ASP.NET Core-Projekt mit .NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="7124c-113">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7124c-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7124c-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="7124c-115">`--auth Individual` Gibt an, die einzelne Benutzerkonten-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="7124c-115">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="7124c-116">Fügen Sie auf Windows, die `-uld` Option.</span><span class="sxs-lookup"><span data-stu-id="7124c-116">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="7124c-117">Es gibt an, dass LocalDB statt SQLite verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7124c-117">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="7124c-118">Führen Sie `new mvc --help` um Hilfe zu diesem Befehl erhalten.</span><span class="sxs-lookup"><span data-stu-id="7124c-118">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7124c-119">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7124c-119">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7124c-120">Wenn Sie die CLI oder SQLite verwenden, führen Sie Folgendes in einem Befehlsfenster ein:</span><span class="sxs-lookup"><span data-stu-id="7124c-120">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="7124c-121">`--auth Individual` Gibt an, die einzelne Benutzerkonten-Projektvorlage.</span><span class="sxs-lookup"><span data-stu-id="7124c-121">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="7124c-122">Fügen Sie auf Windows, die `-uld` Option.</span><span class="sxs-lookup"><span data-stu-id="7124c-122">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="7124c-123">Es gibt an, dass LocalDB statt SQLite verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="7124c-123">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="7124c-124">Führen Sie `new mvc --help` um Hilfe zu diesem Befehl erhalten.</span><span class="sxs-lookup"><span data-stu-id="7124c-124">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="7124c-125">Alternativ können Sie ein neues ASP.NET Core-Projekt mit Visual Studio erstellen:</span><span class="sxs-lookup"><span data-stu-id="7124c-125">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="7124c-126">Erstellen Sie in Visual Studio ein neues **Webanwendung** Projekt.</span><span class="sxs-lookup"><span data-stu-id="7124c-126">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="7124c-127">Wählen Sie **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="7124c-127">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="7124c-128">**.NET Core** ausgewählt ist, in der folgenden Abbildung, Sie können jedoch **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="7124c-128">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="7124c-129">Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="7124c-129">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="7124c-130">Behalten Sie den Standardwert **in-app-Store Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="7124c-130">Keep the default **Store user accounts in-app**.</span></span>

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="7124c-132">Testen Sie die Registrierung neuer Benutzer</span><span class="sxs-lookup"><span data-stu-id="7124c-132">Test new user registration</span></span>

<span data-ttu-id="7124c-133">Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="7124c-133">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="7124c-134">Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus.</span><span class="sxs-lookup"><span data-stu-id="7124c-134">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="7124c-135">Die ausschließliche Überprüfung der auf die e-Mail-Adresse an diesem Punkt ist, mit der [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut.</span><span class="sxs-lookup"><span data-stu-id="7124c-135">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="7124c-136">Nach der Übermittlung der Registrierungs, werden Sie bei der app angemeldet.</span><span class="sxs-lookup"><span data-stu-id="7124c-136">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="7124c-137">Weiter unten in diesem Tutorial wird der Code aktualisiert, damit neue Benutzer können sich nicht anmelden, bis ihre e-Mail-Adresse überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="7124c-137">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="7124c-138">Abrufen der Identität-Datenbank</span><span class="sxs-lookup"><span data-stu-id="7124c-138">View the Identity database</span></span>

<span data-ttu-id="7124c-139">Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anweisungen zum Anzeigen der SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="7124c-139">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="7124c-140">Für Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7124c-140">For Visual Studio:</span></span>

* <span data-ttu-id="7124c-141">Von der **Ansicht** , wählen Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="7124c-141">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="7124c-142">Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer-13)**.</span><span class="sxs-lookup"><span data-stu-id="7124c-142">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="7124c-143">Mit der rechten Maustaste auf **Dbo. "Aspnetusers"** > **zeigen Daten**:</span><span class="sxs-lookup"><span data-stu-id="7124c-143">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Über das Kontextmenü für die Tabelle "aspnetusers" im Objekt-Explorer von SQL Server](accconfirm/_static/ssox.png)

<span data-ttu-id="7124c-145">Beachten Sie die Tabelle `EmailConfirmed` Feld `False`.</span><span class="sxs-lookup"><span data-stu-id="7124c-145">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="7124c-146">Sie möchten möglicherweise verwenden Sie diese e-Mail erneut im nächsten Schritt, wenn die app eine Bestätigung per e-Mail sendet.</span><span class="sxs-lookup"><span data-stu-id="7124c-146">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="7124c-147">Mit der rechten Maustaste auf die Zeile, und wählen **löschen**.</span><span class="sxs-lookup"><span data-stu-id="7124c-147">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="7124c-148">Löschen den e-Mail-Alias erleichtert es in den folgenden Schritten.</span><span class="sxs-lookup"><span data-stu-id="7124c-148">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="7124c-149">Anforderung von HTTPS</span><span class="sxs-lookup"><span data-stu-id="7124c-149">Require HTTPS</span></span>

<span data-ttu-id="7124c-150">Finden Sie unter [verbindliche Verwendung von HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="7124c-150">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="7124c-151">E-Mail-Bestätigung erforderlich</span><span class="sxs-lookup"><span data-stu-id="7124c-151">Require email confirmation</span></span>

<span data-ttu-id="7124c-152">Es ist eine bewährte Methode, die e-Mail-Adresse einer neuen benutzerregistrierung zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="7124c-152">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="7124c-153">E-Mail-Bestätigung können, um zu überprüfen, ob sie sind eine andere Person keinen Identitätswechsel (d. h. sie noch nicht registriert mit einer anderen Person für den e-Mail-Adresse).</span><span class="sxs-lookup"><span data-stu-id="7124c-153">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="7124c-154">Angenommen, Sie haben ein Diskussionsforum, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="7124c-154">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="7124c-155">Ohne e-Mail-Bestätigung "nolivetto@contoso.com" könnte unerwünschte e-Mail-Adresse aus Ihrer app erhalten.</span><span class="sxs-lookup"><span data-stu-id="7124c-155">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="7124c-156">Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" und noch nicht bemerkt, dass die falsche Schreibweise von "Yli".</span><span class="sxs-lookup"><span data-stu-id="7124c-156">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="7124c-157">Sie wäre nicht kennwortwiederherstellung zu verwenden, da die app die korrekte e-Mail-Adresse besitzt.</span><span class="sxs-lookup"><span data-stu-id="7124c-157">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="7124c-158">E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots.</span><span class="sxs-lookup"><span data-stu-id="7124c-158">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="7124c-159">E-Mail-Bestätigung stellt keinen Schutz vor böswilligen Benutzern viele e-Mail-Konten bereit.</span><span class="sxs-lookup"><span data-stu-id="7124c-159">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="7124c-160">Im Allgemeinen möchten Sie verhindern, dass neue Benutzer keine Daten zu Ihrer Website veröffentlichen, bevor sie eine e-Mail bestätigte haben.</span><span class="sxs-lookup"><span data-stu-id="7124c-160">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="7124c-161">Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="7124c-161">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="7124c-162">`config.SignIn.RequireConfirmedEmail = true;` verhindert, dass registrierte Benutzer anmelden, bis ihre-e-Mail bestätigt ist.</span><span class="sxs-lookup"><span data-stu-id="7124c-162">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="7124c-163">Konfigurieren von e-Mail-Anbieter</span><span class="sxs-lookup"><span data-stu-id="7124c-163">Configure email provider</span></span>

<span data-ttu-id="7124c-164">In diesem Tutorial wird SendGrid zum Senden von e-Mails verwendet.</span><span class="sxs-lookup"><span data-stu-id="7124c-164">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="7124c-165">Sie benötigen eine SendGrid-Konto und einen Schlüssel zum Senden von e-Mails.</span><span class="sxs-lookup"><span data-stu-id="7124c-165">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="7124c-166">Sie können andere e-Mail-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="7124c-166">You can use other email providers.</span></span> <span data-ttu-id="7124c-167">ASP.NET Core 2.x enthält `System.Net.Mail`, wodurch Sie zum Senden von e-Mail-Adresse aus Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="7124c-167">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="7124c-168">Es wird empfohlen, dass Sie über SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="7124c-168">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="7124c-169">SMTP ist schwierig, sichere und ordnungsgemäß eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="7124c-169">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="7124c-170">Die [optionsmuster](xref:fundamentals/configuration/options) wird verwendet, um die benutzereinstellungen für Konto und den Schlüssel zugreifen.</span><span class="sxs-lookup"><span data-stu-id="7124c-170">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="7124c-171">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7124c-171">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="7124c-172">Erstellen Sie eine Klasse, um den Schlüssel für sichere e-Mails abrufen.</span><span class="sxs-lookup"><span data-stu-id="7124c-172">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="7124c-173">In diesem Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="7124c-173">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="7124c-174">Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="7124c-174">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="7124c-175">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="7124c-175">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="7124c-176">Auf Windows, speichert Secret Manager-Schlüssel/Wert-Paare in einem *secrets.json* Datei die `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="7124c-176">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="7124c-177">Den Inhalt der *secrets.json* Datei sind nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="7124c-177">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="7124c-178">Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)</span><span class="sxs-lookup"><span data-stu-id="7124c-178">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="7124c-179">Konfigurieren Sie starten, um AuthMessageSenderOptions zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7124c-179">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="7124c-180">Hinzufügen `AuthMessageSenderOptions` zum Dienstcontainer am Ende der `ConfigureServices` -Methode in der die *"Startup.cs"* Datei:</span><span class="sxs-lookup"><span data-stu-id="7124c-180">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7124c-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7124c-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7124c-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7124c-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="7124c-183">Konfigurieren Sie die AuthMessageSender-Klasse</span><span class="sxs-lookup"><span data-stu-id="7124c-183">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="7124c-184">In diesem Tutorial wird gezeigt, wie Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mails über SMTP und andere Mechanismen senden.</span><span class="sxs-lookup"><span data-stu-id="7124c-184">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="7124c-185">Installieren Sie die `SendGrid` NuGet-Paket:</span><span class="sxs-lookup"><span data-stu-id="7124c-185">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="7124c-186">Von der Befehlszeile aus:</span><span class="sxs-lookup"><span data-stu-id="7124c-186">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="7124c-187">Geben Sie über die Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="7124c-187">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="7124c-188">Finden Sie unter [kostenlos erste Schritte mit SendGrid](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="7124c-188">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="7124c-189">Konfigurieren von SendGrid</span><span class="sxs-lookup"><span data-stu-id="7124c-189">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7124c-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7124c-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7124c-191">Zum Konfigurieren von SendGrid fügen Sie Code wie in der folgenden *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="7124c-191">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7124c-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7124c-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="7124c-193">Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="7124c-193">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="7124c-194">Konto-Bestätigung und -Wiederherstellung aktivieren</span><span class="sxs-lookup"><span data-stu-id="7124c-194">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="7124c-195">Die Vorlage weist den Code für die Konto-Bestätigung und -Wiederherstellung.</span><span class="sxs-lookup"><span data-stu-id="7124c-195">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="7124c-196">Suchen der `OnPostAsync` -Methode in der *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="7124c-196">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7124c-197">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7124c-197">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7124c-198">Verhindern Sie, dass neu registrierte Benutzern automatisch durch die Sie die folgende Zeile Kommentierung angemeldet werden:</span><span class="sxs-lookup"><span data-stu-id="7124c-198">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7124c-199">Die complete-Methode wird mit der geänderten Zeile hervorgehoben dargestellt:</span><span class="sxs-lookup"><span data-stu-id="7124c-199">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7124c-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7124c-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7124c-201">Um kontobestätigung zu aktivieren, entfernen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="7124c-201">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="7124c-202">**Hinweis:** Code verhindert, dass einen neu registrierten Benutzer werden automatisch angemeldet, indem Sie die folgende Zeile Kommentierung:</span><span class="sxs-lookup"><span data-stu-id="7124c-202">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="7124c-203">Kennwortwiederherstellung aktivieren, indem Sie die Kommentierung des Codes in die `ForgotPassword` Aktion *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="7124c-203">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="7124c-204">Kommentieren Sie das Formularelement in *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7124c-204">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="7124c-205">Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` -Element, das einen Link zu diesem Artikel enthält.</span><span class="sxs-lookup"><span data-stu-id="7124c-205">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="7124c-206">Registrieren und bestätigen Sie die e-Mail-Adresse und die kennwortzurücksetzung</span><span class="sxs-lookup"><span data-stu-id="7124c-206">Register, confirm email, and reset password</span></span>

<span data-ttu-id="7124c-207">Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.</span><span class="sxs-lookup"><span data-stu-id="7124c-207">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="7124c-208">Die app ausführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="7124c-208">Run the app and register a new user</span></span>

  ![Web-Anwendungsansicht Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="7124c-210">Überprüfen Sie Ihre e-Mail-Adresse für die der Link für die Bestätigung.</span><span class="sxs-lookup"><span data-stu-id="7124c-210">Check your email for the account confirmation link.</span></span> <span data-ttu-id="7124c-211">Finden Sie unter [Debuggen-e-Mail](#debug) sollten Sie die e-Mail nicht erhalten.</span><span class="sxs-lookup"><span data-stu-id="7124c-211">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="7124c-212">Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="7124c-212">Click the link to confirm your email.</span></span>
* <span data-ttu-id="7124c-213">Melden Sie sich mit Ihren e-Mail-Adresse und Ihr Kennwort.</span><span class="sxs-lookup"><span data-stu-id="7124c-213">Log in with your email and password.</span></span>
* <span data-ttu-id="7124c-214">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="7124c-214">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="7124c-215">Anzeigen der Seite "verwalten"</span><span class="sxs-lookup"><span data-stu-id="7124c-215">View the manage page</span></span>

<span data-ttu-id="7124c-216">Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzername](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="7124c-216">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="7124c-217">Möglicherweise müssen Sie die Navigationsleiste, um Benutzername zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="7124c-217">You might need to expand the navbar to see user name.</span></span>

![Navigationsleiste](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7124c-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7124c-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="7124c-220">Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="7124c-220">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="7124c-221">Die **-e-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mail-Adresse wurde bestätigt.</span><span class="sxs-lookup"><span data-stu-id="7124c-221">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Seite "verwalten"](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7124c-223">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7124c-223">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="7124c-224">Dies wird später in diesem Tutorial erwähnt.</span><span class="sxs-lookup"><span data-stu-id="7124c-224">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="7124c-225">![Seite "verwalten"](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="7124c-225">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="7124c-226">Test Zurücksetzen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="7124c-226">Test password reset</span></span>

* <span data-ttu-id="7124c-227">Wenn Sie angemeldet sind, wählen Sie **Logout**.</span><span class="sxs-lookup"><span data-stu-id="7124c-227">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="7124c-228">Wählen Sie die **melden Sie sich bei** verknüpfen, und wählen Sie die **haben Ihr Kennwort vergessen?** Link.</span><span class="sxs-lookup"><span data-stu-id="7124c-228">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="7124c-229">Geben Sie die e-Mail-Adresse, die Sie verwendet, um das Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="7124c-229">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="7124c-230">Es wird eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts gesendet.</span><span class="sxs-lookup"><span data-stu-id="7124c-230">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="7124c-231">Überprüfen Sie Ihre e-Mail-Adresse ein, und klicken Sie auf den Link zum Zurücksetzen Ihres Kennworts.</span><span class="sxs-lookup"><span data-stu-id="7124c-231">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="7124c-232">Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie sich mit Ihrem e-Mail-Adresse und das neue Kennwort anmelden.</span><span class="sxs-lookup"><span data-stu-id="7124c-232">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="7124c-233">Debuggen von e-Mail-Adresse</span><span class="sxs-lookup"><span data-stu-id="7124c-233">Debug email</span></span>

<span data-ttu-id="7124c-234">Wenn Sie e-Mail-Adresse funktioniert nicht:</span><span class="sxs-lookup"><span data-stu-id="7124c-234">If you can't get email working:</span></span>

* <span data-ttu-id="7124c-235">Erstellen Sie eine [Konsolen-app zum Senden von e-Mails](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="7124c-235">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="7124c-236">Überprüfen Sie die [-e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.</span><span class="sxs-lookup"><span data-stu-id="7124c-236">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="7124c-237">Überprüfen Sie Ihren Spam-Ordner.</span><span class="sxs-lookup"><span data-stu-id="7124c-237">Check your spam folder.</span></span>
* <span data-ttu-id="7124c-238">Versuchen Sie es einer anderen e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail usw.)</span><span class="sxs-lookup"><span data-stu-id="7124c-238">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="7124c-239">Versuchen Sie es an andere e-Mail-Konten gesendet.</span><span class="sxs-lookup"><span data-stu-id="7124c-239">Try sending to different email accounts.</span></span>

<span data-ttu-id="7124c-240">**Eine bewährte Sicherheitsmethode** besteht darin, **nicht** produktionsgeheimnisse in Test- und entwicklungsumgebungen verwendet.</span><span class="sxs-lookup"><span data-stu-id="7124c-240">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="7124c-241">Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-Geheimnisse Anwendungseinstellungen im Azure-Web-App-Portal festlegen.</span><span class="sxs-lookup"><span data-stu-id="7124c-241">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="7124c-242">Das Konfigurationssystem ist zum Lesen von Schlüsseln aus Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="7124c-242">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="7124c-243">Kombinieren Sie Konten für soziale Netzwerke und lokale Anmeldung</span><span class="sxs-lookup"><span data-stu-id="7124c-243">Combine social and local login accounts</span></span>

<span data-ttu-id="7124c-244">Um diesen Abschnitt abgeschlossen haben, müssen Sie zuerst einen externer Authentifizierungsanbieter aktivieren.</span><span class="sxs-lookup"><span data-stu-id="7124c-244">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="7124c-245">Finden Sie unter [Facebook, Google und externe Anbieter Authentifizierung](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="7124c-245">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="7124c-246">Sie können lokale als auch social kombinieren, durch Klicken auf Ihre e-Mail-Link.</span><span class="sxs-lookup"><span data-stu-id="7124c-246">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="7124c-247">In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird zuerst als eine lokale Anmeldung; erstellt, Sie können jedoch zunächst erstellen Sie das Konto als Anmeldedaten eines sozialen Netzwerks, und fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="7124c-247">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![Web-Anwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

<span data-ttu-id="7124c-249">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="7124c-249">Click on the **Manage** link.</span></span> <span data-ttu-id="7124c-250">Beachten Sie die 0 externe (Anmeldungen per sozialem Netzwerk) mit diesem Konto verknüpft.</span><span class="sxs-lookup"><span data-stu-id="7124c-250">Note the 0 external (social logins) associated with this account.</span></span>

![Verwalten von anzeigen](accconfirm/_static/manage.png)

<span data-ttu-id="7124c-252">Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="7124c-252">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="7124c-253">In der folgenden Abbildung ist die Facebook dem externen Authentifizierungsanbieter:</span><span class="sxs-lookup"><span data-stu-id="7124c-253">In the following image, Facebook is the external authentication provider:</span></span>

![Verwalten Sie Ihre Facebook auflisten externer Anmeldungen-Ansicht](accconfirm/_static/fb.png)

<span data-ttu-id="7124c-255">Die beiden Konten wurden kombiniert.</span><span class="sxs-lookup"><span data-stu-id="7124c-255">The two accounts have been combined.</span></span> <span data-ttu-id="7124c-256">Sie können mit beiden Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="7124c-256">You are able to log on with either account.</span></span> <span data-ttu-id="7124c-257">Sie sollten Ihre Benutzer lokale Konten hinzufügen, falls ihre sozialen Authentifizierungsdiensts ausgefallen ist oder eher sie den Zugriff auf ihre Konten sozialer Netzwerke verlieren.</span><span class="sxs-lookup"><span data-stu-id="7124c-257">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="7124c-258">Kontobestätigung zu aktivieren, nachdem Sie einen Standort der Benutzer enthält</span><span class="sxs-lookup"><span data-stu-id="7124c-258">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="7124c-259">Aktivierung kontobestätigung auf einer Website für Benutzer, alle vorhandenen Benutzer sperren.</span><span class="sxs-lookup"><span data-stu-id="7124c-259">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="7124c-260">Vorhandene Benutzer werden gesperrt, da es sich bei ihrem Konto bestätigt werden nicht.</span><span class="sxs-lookup"><span data-stu-id="7124c-260">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="7124c-261">Um vorhandene Benutzersperre zu umgehen, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="7124c-261">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="7124c-262">Ändern der Datenbank markieren Sie alle vorhandenen Benutzer als bestätigt wird.</span><span class="sxs-lookup"><span data-stu-id="7124c-262">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="7124c-263">Bestätigen Sie die vorhandenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="7124c-263">Confirm exiting users.</span></span> <span data-ttu-id="7124c-264">Z. B. Batch-Send-e-Mails mit Bestätigung Links.</span><span class="sxs-lookup"><span data-stu-id="7124c-264">For example, batch-send emails with confirmation links.</span></span>
