---
title: Kontobestätigung und kennwortwiederherstellung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen einer ASP.NET Core-Apps mit der e-Mail-Bestätigung und das Kennwort zurücksetzen.
manager: wpickett
ms.author: riande
ms.date: 2/11/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/accconfirm
ms.openlocfilehash: b6dbe234973431448c18d3cc82a6ac98d4f53a3b
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34730450"
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="ea7f5-103">Kontobestätigung und kennwortwiederherstellung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea7f5-103">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="ea7f5-104">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="ea7f5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="ea7f5-105">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Apps mit e-Mail-Bestätigung und das Kennwort zurücksetzen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-105">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span> <span data-ttu-id="ea7f5-106">Dieses Lernprogramm ist **nicht** ein Thema ab.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-106">This tutorial is **not** a beginning topic.</span></span> <span data-ttu-id="ea7f5-107">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-107">You should be familiar with:</span></span>

* [<span data-ttu-id="ea7f5-108">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea7f5-108">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="ea7f5-109">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="ea7f5-109">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="ea7f5-110">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="ea7f5-110">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="ea7f5-111">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="ea7f5-111">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="ea7f5-112">Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die ASP.NET Core MVC 1.1 "und" 2.x-Versionen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-112">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC 1.1 and 2.x versions.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ea7f5-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="ea7f5-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

## <a name="create-a-new-aspnet-core-project-with-the-net-core-cli"></a><span data-ttu-id="ea7f5-114">Erstellen Sie ein neues ASP.NET Core-Projekt mit der .NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="ea7f5-114">Create a new ASP.NET Core project with the .NET Core CLI</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ea7f5-115">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-115">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor --auth Individual -o WebPWrecover
cd WebPWrecover
```

::: moniker-end

* <span data-ttu-id="ea7f5-116">`--auth Individual` Gibt die Projektvorlage der einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-116">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="ea7f5-117">Fügen Sie auf Windows, die `-uld` Option.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-117">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="ea7f5-118">Es gibt an, dass LocalDB anstelle von SQLite verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-118">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="ea7f5-119">Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-119">Run `new mvc --help` to get help on this command.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ea7f5-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ea7f5-121">Wenn Sie die CLI oder SQLite verwenden, führen Sie in einem Befehlsfenster Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-121">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="ea7f5-122">`--auth Individual` Gibt die Projektvorlage der einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-122">`--auth Individual` specifies the Individual User Accounts project template.</span></span>
* <span data-ttu-id="ea7f5-123">Fügen Sie auf Windows, die `-uld` Option.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-123">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="ea7f5-124">Es gibt an, dass LocalDB anstelle von SQLite verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-124">It specifies LocalDB should be used instead of SQLite.</span></span>
* <span data-ttu-id="ea7f5-125">Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-125">Run `new mvc --help` to get help on this command.</span></span>

---

<span data-ttu-id="ea7f5-126">Alternativ können Sie ein neues ASP.NET Core-Projekt mit Visual Studio erstellen:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-126">Alternatively, you can create a new ASP.NET Core project with Visual Studio:</span></span>

* <span data-ttu-id="ea7f5-127">Erstellen Sie in Visual Studio ein neues **Webanwendung** Projekt.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-127">In Visual Studio, create a new **Web Application** project.</span></span>
* <span data-ttu-id="ea7f5-128">Wählen Sie **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-128">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="ea7f5-129">**.NET Core** in der folgenden Abbildung ausgewählt ist, aber Sie können auswählen, **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-129">**.NET Core** is selected in the following image, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="ea7f5-130">Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-130">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="ea7f5-131">Behalten Sie den Standardwert **in app-Store Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-131">Keep the default **Store user accounts in-app**.</span></span>

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

## <a name="test-new-user-registration"></a><span data-ttu-id="ea7f5-133">Testen Sie die Registrierung neuer Benutzer</span><span class="sxs-lookup"><span data-stu-id="ea7f5-133">Test new user registration</span></span>

<span data-ttu-id="ea7f5-134">Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-134">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="ea7f5-135">Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-135">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="ea7f5-136">An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) Attribut.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-136">At this point, the only validation on the email is with the [[EmailAddress]](/dotnet/api/system.componentmodel.dataannotations.emailaddressattribute) attribute.</span></span> <span data-ttu-id="ea7f5-137">Nach dem Senden der Registrierungs, sind Sie in der app angemeldet.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-137">After submitting the registration, you are logged into the app.</span></span> <span data-ttu-id="ea7f5-138">Später in diesem Lernprogramm wird der Code aktualisiert, damit neue Benutzer anmelden können, bis ihre e-Mails überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-138">Later in the tutorial, the code is updated so new users can't log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="ea7f5-139">Anzeigen der Identity-Datenbank</span><span class="sxs-lookup"><span data-stu-id="ea7f5-139">View the Identity database</span></span>

<span data-ttu-id="ea7f5-140">Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anleitungen zur Anzeige der SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-140">See [Work with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite database.</span></span>

<span data-ttu-id="ea7f5-141">Für Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-141">For Visual Studio:</span></span>

* <span data-ttu-id="ea7f5-142">Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-142">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span>
* <span data-ttu-id="ea7f5-143">Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer 13)**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-143">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="ea7f5-144">Mit der rechten Maustaste auf **Dbo. AspNetUsers** > **zeigen Daten**:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-144">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Kontextmenü für AspNetUsers-Tabelle in SQL Server-Objekt-Explorer](accconfirm/_static/ssox.png)

<span data-ttu-id="ea7f5-146">Beachten Sie der Tabelle `EmailConfirmed` Feld ist `False`.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-146">Note the table's `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="ea7f5-147">Möglicherweise möchten diese e-Mail-Adresse verwenden erneut im nächsten Schritt, wenn die app eine e-Mail zur kaufbestätigung sendet.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-147">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="ea7f5-148">Mit der rechten Maustaste auf die Zeile, und wählen **löschen**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-148">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="ea7f5-149">Löschen den e-Mail-Alias erleichtert es in den folgenden Schritten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-149">Deleting the email alias makes it easier in the following steps.</span></span>

---

## <a name="require-https"></a><span data-ttu-id="ea7f5-150">HTTPS erforderlich</span><span class="sxs-lookup"><span data-stu-id="ea7f5-150">Require HTTPS</span></span>

<span data-ttu-id="ea7f5-151">Finden Sie unter [HTTPS erforderlich](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-151">See [Require HTTPS](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="ea7f5-152">E-Mail-Bestätigung</span><span class="sxs-lookup"><span data-stu-id="ea7f5-152">Require email confirmation</span></span>

<span data-ttu-id="ea7f5-153">Es wird empfohlen, die e-Mail-Adresse eines eine neue benutzerregistrierung zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-153">It's a best practice to confirm the email of a new user registration.</span></span> <span data-ttu-id="ea7f5-154">E-Mail-Bestätigung hilft Ihnen bei der überprüfen, haben sie keinen Identitätswechsel eine andere Person (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-154">Email confirmation helps to verify they're not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="ea7f5-155">Angenommen, ein Diskussionsforum mussten, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com".</span><span class="sxs-lookup"><span data-stu-id="ea7f5-155">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com".</span></span> <span data-ttu-id="ea7f5-156">Ohne e-Mail-Bestätigung "nolivetto@contoso.com" erhalten Sie unerwünschten e-Mail konnte aus Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-156">Without email confirmation, "nolivetto@contoso.com" could receive unwanted email from your app.</span></span> <span data-ttu-id="ea7f5-157">Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" und hadn't bemerkt, dass die falsche Schreibweise des "Yli".</span><span class="sxs-lookup"><span data-stu-id="ea7f5-157">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli".</span></span> <span data-ttu-id="ea7f5-158">Sie wäre nicht kennwortwiederherstellung verwenden, da die app ihre korrekte e-Mail-Adresse hat.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-158">They wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="ea7f5-159">E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-159">Email confirmation provides only limited protection from bots.</span></span> <span data-ttu-id="ea7f5-160">E-Mail-Bestätigung stellt keinen Schutz vor böswilligen Benutzern viele e-Mail-Konten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-160">Email confirmation doesn't provide protection from malicious users with many email accounts.</span></span>

<span data-ttu-id="ea7f5-161">In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie eine bestätigte e-Mail-Adresse aufweisen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-161">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span>

<span data-ttu-id="ea7f5-162">Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-162">Update `ConfigureServices` to require a confirmed email:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet1&highlight=12-17)]

<span data-ttu-id="ea7f5-163">`config.SignIn.RequireConfirmedEmail = true;` verhindert, dass registrierte Benutzer anmelden, bis ihre-e-Mail bestätigt ist.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-163">`config.SignIn.RequireConfirmedEmail = true;` prevents registered users from logging in until their email is confirmed.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="ea7f5-164">Konfigurieren von e-Mail-Anbieter</span><span class="sxs-lookup"><span data-stu-id="ea7f5-164">Configure email provider</span></span>

<span data-ttu-id="ea7f5-165">In diesem Lernprogramm wird SendGrid zum Senden von e-Mails.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-165">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="ea7f5-166">Sie benötigen ein SendGrid-Konto und ein Schlüssel zum Senden von e-Mail.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-166">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="ea7f5-167">Sie können andere e-Mail-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-167">You can use other email providers.</span></span> <span data-ttu-id="ea7f5-168">ASP.NET Core 2.x enthält `System.Net.Mail`, womit Sie das Senden von e-Mails aus Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-168">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="ea7f5-169">Es wird empfohlen, dass Sie SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-169">We recommend you use SendGrid or another email service to send email.</span></span> <span data-ttu-id="ea7f5-170">SMTP ist schwer zu schützen und ordnungsgemäß eingerichtet wurden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-170">SMTP is difficult to secure and set up correctly.</span></span>

<span data-ttu-id="ea7f5-171">Die [Optionen Muster](xref:fundamentals/configuration/options) wird verwendet, um die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-171">The [Options pattern](xref:fundamentals/configuration/options) is used to access the user account and key settings.</span></span> <span data-ttu-id="ea7f5-172">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-172">For more information, see [configuration](xref:fundamentals/configuration/index).</span></span>

<span data-ttu-id="ea7f5-173">Erstellen Sie eine Klasse, um den e-Mail-Schlüssel abzurufen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-173">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="ea7f5-174">Für dieses Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-174">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/AuthMessageSenderOptions.cs?name=snippet1)]

<span data-ttu-id="ea7f5-175">Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-175">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ea7f5-176">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-176">For example:</span></span>

```console
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="ea7f5-177">Unter Windows, geheime Schlüssel-Manager speichert Schlüssel/Wert-Paare in ein *secrets.json* in der Datei die `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-177">On Windows, Secret Manager stores keys/value pairs in a *secrets.json* file in the `%APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId>` directory.</span></span>

<span data-ttu-id="ea7f5-178">Der Inhalt der *secrets.json* Datei sind nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-178">The contents of the *secrets.json* file aren't encrypted.</span></span> <span data-ttu-id="ea7f5-179">Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)</span><span class="sxs-lookup"><span data-stu-id="ea7f5-179">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

 ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="ea7f5-180">Konfigurieren Sie Start AuthMessageSenderOptions Verwendung</span><span class="sxs-lookup"><span data-stu-id="ea7f5-180">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="ea7f5-181">Hinzufügen `AuthMessageSenderOptions` dem Dienstcontainer am Ende der `ConfigureServices` Methode in der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-181">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ea7f5-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](accconfirm/sample/WebPWrecover/Startup.cs?name=snippet2&highlight=28)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ea7f5-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

[!code-csharp[](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="ea7f5-184">Konfigurieren Sie die AuthMessageSender-Klasse</span><span class="sxs-lookup"><span data-stu-id="ea7f5-184">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="ea7f5-185">Dieses Lernprogramm veranschaulicht das Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-185">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

<span data-ttu-id="ea7f5-186">Installieren der `SendGrid` NuGet-Paket:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-186">Install the `SendGrid` NuGet package:</span></span>

* <span data-ttu-id="ea7f5-187">Über die Befehlszeile:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-187">From the command line:</span></span>

    `dotnet add package SendGrid`

* <span data-ttu-id="ea7f5-188">Der Paket-Manager-Konsole geben Sie den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-188">From the Package Manager Console, enter the following command:</span></span>

  `Install-Package SendGrid`

<span data-ttu-id="ea7f5-189">Finden Sie unter [erste Schritte mit SendGrid kostenlos](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto registrieren.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-189">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="ea7f5-190">Konfigurieren von SendGrid</span><span class="sxs-lookup"><span data-stu-id="ea7f5-190">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ea7f5-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ea7f5-192">Um die SendGrid zu konfigurieren, fügen Sie Code ähnlich dem folgenden in *Services/EmailSender.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-192">To configure SendGrid, add code similar to the following in *Services/EmailSender.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Services/EmailSender.cs)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ea7f5-193">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-193">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="ea7f5-194">Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-194">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Services/MessageServices.cs)]

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="ea7f5-195">Konto bestätigen und Kennwort Wiederherstellung aktivieren</span><span class="sxs-lookup"><span data-stu-id="ea7f5-195">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="ea7f5-196">Die Vorlage, den Code für die Wiederherstellung für Konto bestätigen und das Kennwort hat.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-196">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="ea7f5-197">Suchen der `OnPostAsync` Methode im *Pages/Account/Register.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-197">Find the `OnPostAsync` method in *Pages/Account/Register.cshtml.cs*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ea7f5-198">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-198">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="ea7f5-199">Verhindern Sie, dass neu registrierte Benutzern zur Verfügung wird automatisch durch die folgende Zeile auskommentiert angemeldet werden:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-199">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ea7f5-200">Die vollständige Methode ist mit der geänderten Zeile hervorgehoben dargestellt:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-200">The complete method is shown with the changed line highlighted:</span></span>

[!code-csharp[](accconfirm/sample/WebPWrecover/Pages/Account/Register.cshtml.cs?highlight=16&name=snippet_Register)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ea7f5-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="ea7f5-202">Zum Aktivieren der kontobestätigung kommentieren Sie den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-202">To enable account confirmation, uncomment the following code:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]

<span data-ttu-id="ea7f5-203">**Hinweis:** Code verhindert, dass einen neu registrierten Benutzer wird automatisch durch die folgende Zeile auskommentiert angemeldet:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-203">**Note:** The code is preventing a newly registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="ea7f5-204">Kennwortwiederherstellung aktivieren, indem Sie den Code in Auskommentieren der `ForgotPassword` Aktion *Controllers/AccountController.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-204">Enable password recovery by uncommenting the code in the `ForgotPassword` action of *Controllers/AccountController.cs*:</span></span>

[!code-csharp[](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]

<span data-ttu-id="ea7f5-205">Kommentieren Sie die Form-Elements im *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-205">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="ea7f5-206">Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` Element, das einen Link zu dieser Artikel enthält.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-206">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element, which contains a link to this article.</span></span>

[!code-cshtml[](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="ea7f5-207">Registrieren, e-Mails zu bestätigen und Kennwort zurücksetzen</span><span class="sxs-lookup"><span data-stu-id="ea7f5-207">Register, confirm email, and reset password</span></span>

<span data-ttu-id="ea7f5-208">Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-208">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="ea7f5-209">Die app auszuführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="ea7f5-209">Run the app and register a new user</span></span>

  ![Webansicht Anwendung Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="ea7f5-211">Suchen Sie nach der Link für die Bestätigung Ihrer e-Mail.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-211">Check your email for the account confirmation link.</span></span> <span data-ttu-id="ea7f5-212">Finden Sie unter [Debuggen e-Mail](#debug) , wenn Sie die e-Mail nicht erhalten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-212">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="ea7f5-213">Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-213">Click the link to confirm your email.</span></span>
* <span data-ttu-id="ea7f5-214">Melden Sie sich mit Ihren e-Mail- und das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-214">Log in with your email and password.</span></span>
* <span data-ttu-id="ea7f5-215">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-215">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="ea7f5-216">Anzeigen der Seite "verwalten"</span><span class="sxs-lookup"><span data-stu-id="ea7f5-216">View the manage page</span></span>

<span data-ttu-id="ea7f5-217">Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzernamen](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="ea7f5-217">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="ea7f5-218">Möglicherweise müssen Sie die Navigationsleiste, um Benutzername erweitern.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-218">You might need to expand the navbar to see user name.</span></span>

![Navigationsleiste](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ea7f5-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ea7f5-221">Die Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-221">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="ea7f5-222">Die **E-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mails bestätigt wurde.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-222">The **Email** shows a check box indicating the email has been confirmed.</span></span>

![Seite "verwalten"](accconfirm/_static/rick2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ea7f5-224">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ea7f5-224">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ea7f5-225">Dies wird später in diesem Lernprogramm erwähnt.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-225">This is mentioned later in the tutorial.</span></span>
<span data-ttu-id="ea7f5-226">![Seite "verwalten"](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="ea7f5-226">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="ea7f5-227">Test Zurücksetzen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="ea7f5-227">Test password reset</span></span>

* <span data-ttu-id="ea7f5-228">Wenn Sie angemeldet sind, wählen Sie **Logout**.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-228">If you're logged in, select **Logout**.</span></span>
* <span data-ttu-id="ea7f5-229">Wählen Sie die **melden Sie sich** verknüpfen, und wählen Sie die **haben Sie Ihr Kennwort vergessen?** Link.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-229">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="ea7f5-230">Geben Sie die e-Mail, die Sie verwendet, um das Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-230">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="ea7f5-231">Eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-231">An email with a link to reset your password is sent.</span></span> <span data-ttu-id="ea7f5-232">Überprüfen Sie Ihre e-Mail-Adresse, und klicken Sie auf den Link, um Ihr Kennwort zurückzusetzen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-232">Check your email and click the link to reset your password.</span></span> <span data-ttu-id="ea7f5-233">Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie sich mit Ihrer e-Mail- und das neue Kennwort anmelden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-233">After your password has been successfully reset, you can log in with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="ea7f5-234">Debuggen-e-Mail</span><span class="sxs-lookup"><span data-stu-id="ea7f5-234">Debug email</span></span>

<span data-ttu-id="ea7f5-235">Wenn Sie e-Mail-nicht funktioniert:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-235">If you can't get email working:</span></span>

* <span data-ttu-id="ea7f5-236">Erstellen einer [Konsolen-app zum Senden von e-Mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-236">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="ea7f5-237">Überprüfen Sie die [e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-237">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="ea7f5-238">Überprüfen Sie Ihre Spam-Ordner.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-238">Check your spam folder.</span></span>
* <span data-ttu-id="ea7f5-239">Wiederholen Sie dann eine andere e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail, usw.).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-239">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="ea7f5-240">Wiederholen Sie den senden an andere e-Mail-Konten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-240">Try sending to different email accounts.</span></span>

<span data-ttu-id="ea7f5-241">**Eine bewährte Sicherheitsmethode** entspricht **nicht** Produktion geheime Schlüssel in Test- und verwenden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-241">**A security best practice** is to **not** use production secrets in test and development.</span></span> <span data-ttu-id="ea7f5-242">Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-242">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="ea7f5-243">Das Konfigurationssystem wird beim Lesen von Schlüsseln von Umgebungsvariablen eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-243">The configuration system is set up to read keys from environment variables.</span></span>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="ea7f5-244">Kombinieren von sozialen und lokalen Anmeldekonten</span><span class="sxs-lookup"><span data-stu-id="ea7f5-244">Combine social and local login accounts</span></span>

<span data-ttu-id="ea7f5-245">Um in diesem Abschnitt abzuschließen, müssen Sie zunächst ein externen Authentifizierungsanbieters aktivieren.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-245">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="ea7f5-246">Finden Sie unter [Facebook, Google, und die externe Anbieter Authentifizierung](xref:security/authentication/social/index).</span><span class="sxs-lookup"><span data-stu-id="ea7f5-246">See [Facebook, Google, and external provider authentication](xref:security/authentication/social/index).</span></span>

<span data-ttu-id="ea7f5-247">Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-247">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="ea7f5-248">In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird als eine lokale Anmeldung; zuerst erstellt jedoch zunächst erstellen Sie das Konto als sozialen Anmeldung werden können, und fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-248">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![-Webanwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

<span data-ttu-id="ea7f5-250">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-250">Click on the **Manage** link.</span></span> <span data-ttu-id="ea7f5-251">Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-251">Note the 0 external (social logins) associated with this account.</span></span>

![Verwalten von anzeigen](accconfirm/_static/manage.png)

<span data-ttu-id="ea7f5-253">Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-253">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="ea7f5-254">In der folgenden Abbildung wird die Facebook dem externen Authentifizierungsanbieter:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-254">In the following image, Facebook is the external authentication provider:</span></span>

![Verwalten Sie externer Anmeldungen Ansicht Facebook auflisten](accconfirm/_static/fb.png)

<span data-ttu-id="ea7f5-256">Die beiden Konten wurden kombiniert.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-256">The two accounts have been combined.</span></span> <span data-ttu-id="ea7f5-257">Sie können mit entweder Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-257">You are able to log on with either account.</span></span> <span data-ttu-id="ea7f5-258">Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls ihre sozialen Anmeldung Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-258">You might want your users to add local accounts in case their social login authentication service is down, or more likely they've lost access to their social account.</span></span>

## <a name="enable-account-confirmation-after-a-site-has-users"></a><span data-ttu-id="ea7f5-259">Aktivieren Sie kontobestätigung, nachdem an einem Standort Benutzer gelten.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-259">Enable account confirmation after a site has users</span></span>

<span data-ttu-id="ea7f5-260">Aktivieren des kontobestätigung an einem Standort mit Benutzern sperrt alle vorhandenen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-260">Enabling account confirmation on a site with users locks out all the existing users.</span></span> <span data-ttu-id="ea7f5-261">Vorhandene Benutzer werden gesperrt, da ihre Konten bestätigt werden nicht.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-261">Existing users are locked out because their accounts aren't confirmed.</span></span> <span data-ttu-id="ea7f5-262">Um vorhandene Benutzersperre zu umgehen, verwenden Sie einen der folgenden Ansätze:</span><span class="sxs-lookup"><span data-stu-id="ea7f5-262">To work around existing user lockout, use one of the following approaches:</span></span>

* <span data-ttu-id="ea7f5-263">Aktualisieren Sie die Datenbank, um markieren Sie alle vorhandenen Benutzer als bestätigt wird.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-263">Update the database to mark all existing users as being confirmed.</span></span>
* <span data-ttu-id="ea7f5-264">Bestätigen Sie vorhandene Benutzer.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-264">Confirm exiting users.</span></span> <span data-ttu-id="ea7f5-265">Beispielsweise Batch-Send-e-Mails mit Bestätigung Links.</span><span class="sxs-lookup"><span data-stu-id="ea7f5-265">For example, batch-send emails with confirmation links.</span></span>
