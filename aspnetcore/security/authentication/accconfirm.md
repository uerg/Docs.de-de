---
title: "Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core"
author: rick-anderson
description: "Veranschaulicht das Erstellen einer ASP.NET Core-Apps mit der e-Mail-Bestätigung und das Kennwort zurücksetzen."
keywords: "ASP.NET Core, das Kennwort zurückzusetzen, e-Mail-Bestätigung, Sicherheit"
ms.author: riande
manager: wpickett
ms.date: 07/19/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/accconfirm
ms.openlocfilehash: aaed75c78a99e59954add959a76a2fd68ea5f3fc
ms.sourcegitcommit: f2fb0b45284e4f8c4a9c422bec790aede7c1f0ac
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/17/2017
---
# <a name="account-confirmation-and-password-recovery-in-aspnet-core"></a><span data-ttu-id="59af4-104">Kontobestätigung und kennwortwiederherstellung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="59af4-104">Account confirmation and password recovery in ASP.NET Core</span></span>

<span data-ttu-id="59af4-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="59af4-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="59af4-106">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Apps mit e-Mail-Bestätigung und das Kennwort zurücksetzen.</span><span class="sxs-lookup"><span data-stu-id="59af4-106">This tutorial shows you how to build an ASP.NET Core app with email confirmation and password reset.</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="59af4-107">Erstellen eines neuen ASP.NET Core-Projekts</span><span class="sxs-lookup"><span data-stu-id="59af4-107">Create a New ASP.NET Core Project</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-108">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-108">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="59af4-109">Dieser Schritt gilt für Visual Studio unter Windows.</span><span class="sxs-lookup"><span data-stu-id="59af4-109">This step applies to Visual Studio on Windows.</span></span> <span data-ttu-id="59af4-110">Finden Sie im nächsten Abschnitt CLI-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="59af4-110">See the next section for CLI instructions.</span></span>

<span data-ttu-id="59af4-111">Das Lernprogramm ist Visual Studio 2017 Preview 2 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="59af4-111">The tutorial requires Visual Studio 2017 Preview 2 or later.</span></span>

* <span data-ttu-id="59af4-112">Erstellen Sie in Visual Studio eine neue Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="59af4-112">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="59af4-113">Wählen Sie **ASP.NET Core 2.0**.</span><span class="sxs-lookup"><span data-stu-id="59af4-113">Select **ASP.NET Core 2.0**.</span></span> <span data-ttu-id="59af4-114">Die folgende Abbildung anzeigen **.NET Core** ausgewählt, aber Sie können auswählen, **.NET Framework**.</span><span class="sxs-lookup"><span data-stu-id="59af4-114">The following image show **.NET Core** selected, but you can select **.NET Framework**.</span></span>
* <span data-ttu-id="59af4-115">Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="59af4-115">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>
* <span data-ttu-id="59af4-116">Behalten Sie den Standardwert **in app-Store Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="59af4-116">Keep the default **Store user accounts in-app**.</span></span>

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/2.png)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="59af4-119">Das Lernprogramm ist Visual Studio 2017 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="59af4-119">The tutorial requires Visual Studio 2017 or later.</span></span>

* <span data-ttu-id="59af4-120">Erstellen Sie in Visual Studio eine neue Webanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="59af4-120">In Visual Studio, create a New Web Application Project.</span></span>
* <span data-ttu-id="59af4-121">Wählen Sie **Authentifizierung ändern** und legen Sie auf **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="59af4-121">Select **Change Authentication** and set to **Individual User Accounts**.</span></span>

![Dialogfeld "Neues Projekt" mit "Einzelne Benutzerkonten Radio" ausgewählt](accconfirm/_static/indiv.png)

---

### <a name="net-core-cli-project-creation-for-macos-and-linux"></a><span data-ttu-id="59af4-123">.NET Core CLI projekterstellung für MacOS und Linux</span><span class="sxs-lookup"><span data-stu-id="59af4-123">.NET Core CLI project creation for macOS and Linux</span></span>

<span data-ttu-id="59af4-124">Wenn Sie die CLI oder SQLite verwenden, führen Sie in einem Befehlsfenster Folgendes:</span><span class="sxs-lookup"><span data-stu-id="59af4-124">If you're using the CLI or SQLite, run the following in a command window:</span></span>

```console
dotnet new mvc --auth Individual
```

* <span data-ttu-id="59af4-125">`--auth Individual`Gibt die Vorlage für die einzelnen Benutzerkonten an.</span><span class="sxs-lookup"><span data-stu-id="59af4-125">`--auth Individual` specifies the Individual User Accounts template.</span></span>
* <span data-ttu-id="59af4-126">Fügen Sie auf Windows, die `-uld` Option.</span><span class="sxs-lookup"><span data-stu-id="59af4-126">On Windows, add the `-uld` option.</span></span> <span data-ttu-id="59af4-127">Die `-uld` Option erstellt eine LocalDB-Verbindungszeichenfolge anstelle einer SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59af4-127">The `-uld` option creates a LocalDB connection string rather than a SQLite DB.</span></span>
* <span data-ttu-id="59af4-128">Führen Sie `new mvc --help` um Hilfe zu diesem Befehl zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="59af4-128">Run `new mvc --help` to get help on this command.</span></span>

## <a name="test-new-user-registration"></a><span data-ttu-id="59af4-129">Testen Sie die Registrierung neuer Benutzer</span><span class="sxs-lookup"><span data-stu-id="59af4-129">Test new user registration</span></span>

<span data-ttu-id="59af4-130">Die app auszuführen, wählen Sie die **registrieren** verknüpfen, und registrieren Sie einen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="59af4-130">Run the app, select the **Register** link, and register a user.</span></span> <span data-ttu-id="59af4-131">Befolgen Sie die Anweisungen zum Ausführen von Entity Framework Core-Migrationen aus.</span><span class="sxs-lookup"><span data-stu-id="59af4-131">Follow the instructions to run Entity Framework Core migrations.</span></span> <span data-ttu-id="59af4-132">An diesem Punkt wird die ausschließliche Überprüfung auf die e-Mail-Adresse mit der [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) Attribut.</span><span class="sxs-lookup"><span data-stu-id="59af4-132">At this  point, the only validation on the email is with the [[EmailAddress]](http://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span> <span data-ttu-id="59af4-133">Nachdem Sie die Registrierung senden, werden Sie in der app angemeldet.</span><span class="sxs-lookup"><span data-stu-id="59af4-133">After you submit the registration, you are logged into the app.</span></span> <span data-ttu-id="59af4-134">Später in diesem Lernprogramm werden wir dies ändern, damit neue Benutzer anmelden können, bis ihre e-Mails überprüft wurde.</span><span class="sxs-lookup"><span data-stu-id="59af4-134">Later in the tutorial, we'll change this so new users cannot log in until their email has been validated.</span></span>

## <a name="view-the-identity-database"></a><span data-ttu-id="59af4-135">Anzeigen der Identity-Datenbank</span><span class="sxs-lookup"><span data-stu-id="59af4-135">View the Identity database</span></span>

# <a name="sql-servertabsql-server"></a>[<span data-ttu-id="59af4-136">SQL Server</span><span class="sxs-lookup"><span data-stu-id="59af4-136">SQL Server</span></span>](#tab/sql-server)

* <span data-ttu-id="59af4-137">Aus der **Ansicht** klicken Sie im Menü **Objekt-Explorer von SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="59af4-137">From the **View** menu, select **SQL Server Object Explorer** (SSOX).</span></span> 
* <span data-ttu-id="59af4-138">Navigieren Sie zu **(Localdb) MSSQLLocalDB (SQLServer 13)**.</span><span class="sxs-lookup"><span data-stu-id="59af4-138">Navigate to **(localdb)MSSQLLocalDB(SQL Server 13)**.</span></span> <span data-ttu-id="59af4-139">Mit der rechten Maustaste auf **Dbo. AspNetUsers** > **zeigen Daten**:</span><span class="sxs-lookup"><span data-stu-id="59af4-139">Right-click on **dbo.AspNetUsers** > **View Data**:</span></span>

![Kontextmenü für AspNetUsers-Tabelle in SQL Server-Objekt-Explorer](accconfirm/_static/ssox.png)

<span data-ttu-id="59af4-141">Beachten Sie die `EmailConfirmed` Feld ist `False`.</span><span class="sxs-lookup"><span data-stu-id="59af4-141">Note the `EmailConfirmed` field is `False`.</span></span>

<span data-ttu-id="59af4-142">Möglicherweise möchten diese e-Mail-Adresse verwenden erneut im nächsten Schritt, wenn die app eine e-Mail zur kaufbestätigung sendet.</span><span class="sxs-lookup"><span data-stu-id="59af4-142">You might want to use this email again in the next step when the app sends a confirmation email.</span></span> <span data-ttu-id="59af4-143">Mit der rechten Maustaste auf die Zeile, und wählen **löschen**.</span><span class="sxs-lookup"><span data-stu-id="59af4-143">Right-click on the row and select **Delete**.</span></span> <span data-ttu-id="59af4-144">Löschen die e-Mail-Adresse alias wird nun erleichtert in den folgenden Schritten.</span><span class="sxs-lookup"><span data-stu-id="59af4-144">Deleting the email alias now will make it easier in the following steps.</span></span>

# <a name="sqlitetabsqlite"></a>[<span data-ttu-id="59af4-145">SQLite</span><span class="sxs-lookup"><span data-stu-id="59af4-145">SQLite</span></span>](#tab/sqlite)

<span data-ttu-id="59af4-146">Finden Sie unter [arbeiten mit SQLite in einem ASP.NET Core MVC-Projekt](xref:tutorials/first-mvc-app-xplat/working-with-sql) Anweisungen zum Anzeigen der SQLite-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="59af4-146">See [Working with SQLite in an ASP.NET Core MVC project](xref:tutorials/first-mvc-app-xplat/working-with-sql) for instructions on how to view the SQLite DB.</span></span> 

---

## <a name="require-ssl-and-setup-iis-express-for-ssl"></a><span data-ttu-id="59af4-147">Erfordern von SSL und Einrichten von IIS Express für SSL</span><span class="sxs-lookup"><span data-stu-id="59af4-147">Require SSL and setup IIS Express for SSL</span></span>

<span data-ttu-id="59af4-148">Finden Sie unter [Erzwingen von SSL](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="59af4-148">See [Enforcing SSL](xref:security/enforcing-ssl).</span></span>

<a name="prevent-login-at-registration"></a>
## <a name="require-email-confirmation"></a><span data-ttu-id="59af4-149">E-Mail-Bestätigung</span><span class="sxs-lookup"><span data-stu-id="59af4-149">Require email confirmation</span></span>

<span data-ttu-id="59af4-150">Es wird empfohlen, die e-Mail-Adresse eines eine neue benutzerregistrierung, um zu überprüfen, sind sie nicht eine andere Person Identität zu bestätigen (d. h., sie noch nicht registriert mit einer anderen Person e-Mail-Adresse).</span><span class="sxs-lookup"><span data-stu-id="59af4-150">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="59af4-151">Angenommen, ein Diskussionsforum mussten, und Sie möchten, um zu verhindern, dass "yli@example.com"aus der Registrierung als"nolivetto@contoso.com."</span><span class="sxs-lookup"><span data-stu-id="59af4-151">Suppose you had a discussion forum, and you wanted to prevent "yli@example.com" from registering as "nolivetto@contoso.com."</span></span> <span data-ttu-id="59af4-152">Ohne e-Mail-Bestätigung "nolivetto@contoso.com" könnte unerwünschte e-Mails aus Ihrer app abrufen.</span><span class="sxs-lookup"><span data-stu-id="59af4-152">Without email confirmation, "nolivetto@contoso.com" could get unwanted email from your app.</span></span> <span data-ttu-id="59af4-153">Angenommen, das der Benutzer versehentlich als registriert "ylo@example.com" hadn't Tippfehler nun der "Yli", wäre sie nicht kennwortwiederherstellung verwenden, da die app ihre korrekte e-Mail-Adresse hat.</span><span class="sxs-lookup"><span data-stu-id="59af4-153">Suppose the user accidentally registered as "ylo@example.com" and hadn't noticed the misspelling of "yli," they wouldn't be able to use password recovery because the app doesn't have their correct email.</span></span> <span data-ttu-id="59af4-154">E-Mail-Bestätigung enthält nur begrenzt Schutz von Bots und stellt keinen Schutz vor bestimmt Spammern Domänenbenutzern viele funktionierenden e-Mail-Aliase, die sie verwenden können, um zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="59af4-154">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers who have many working email aliases they can use to register.</span></span>

<span data-ttu-id="59af4-155">In der Regel möchten verhindern, dass neue Benutzer keine Daten zu Ihrer Website bereitstellen, bevor sie eine bestätigte e-Mail-Adresse aufweisen.</span><span class="sxs-lookup"><span data-stu-id="59af4-155">You generally want to prevent new users from posting any data to your web site before they have a confirmed email.</span></span> 

<span data-ttu-id="59af4-156">Update `ConfigureServices` eine bestätigte e-Mail-Adresse erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="59af4-156">Update `ConfigureServices` to require a confirmed email:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="59af4-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span><span class="sxs-lookup"><span data-stu-id="59af4-158">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=6-9)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="59af4-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span><span class="sxs-lookup"><span data-stu-id="59af4-160">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=13-16)]</span></span>

---

 
```csharp
config.SignIn.RequireConfirmedEmail = true;
```
<span data-ttu-id="59af4-161">Die vorangehende Zeile wird verhindert, dass registrierte Benutzern zur Verfügung protokolliert werden, bis ihre-e-Mail bestätigt ist.</span><span class="sxs-lookup"><span data-stu-id="59af4-161">The preceding line prevents registered users from being logged in until their email is confirmed.</span></span> <span data-ttu-id="59af4-162">Allerdings verhindert dieser Zeile nicht neue Benutzer im protokolliert werden, nachdem sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="59af4-162">However, that line does not prevent new users from being logged in after they register.</span></span> <span data-ttu-id="59af4-163">Der Standardcode meldet einen Benutzer auf, nachdem sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="59af4-163">The default code logs in a user after they register.</span></span> <span data-ttu-id="59af4-164">Nachdem sie sich abmelden, können sie nicht erneut anmelden, bis sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="59af4-164">Once they log out, they won't be able to log in again until they register.</span></span> <span data-ttu-id="59af4-165">Später in diesem Lernprogramm ändern wir der Code so neu registrierte Benutzer sind **nicht** angemeldet.</span><span class="sxs-lookup"><span data-stu-id="59af4-165">Later in the tutorial we'll change the code so newly registered user are **not** logged in.</span></span>

### <a name="configure-email-provider"></a><span data-ttu-id="59af4-166">Konfigurieren von e-Mail-Anbieter</span><span class="sxs-lookup"><span data-stu-id="59af4-166">Configure email provider</span></span>

<span data-ttu-id="59af4-167">In diesem Lernprogramm wird SendGrid zum Senden von e-Mails.</span><span class="sxs-lookup"><span data-stu-id="59af4-167">In this tutorial, SendGrid is used to send email.</span></span> <span data-ttu-id="59af4-168">Sie benötigen ein SendGrid-Konto und ein Schlüssel zum Senden von e-Mail.</span><span class="sxs-lookup"><span data-stu-id="59af4-168">You need a SendGrid account and key to send email.</span></span> <span data-ttu-id="59af4-169">Sie können andere e-Mail-Anbieter verwenden.</span><span class="sxs-lookup"><span data-stu-id="59af4-169">You can use other email providers.</span></span> <span data-ttu-id="59af4-170">ASP.NET Core 2.x enthält `System.Net.Mail`, womit Sie das Senden von e-Mails aus Ihrer app.</span><span class="sxs-lookup"><span data-stu-id="59af4-170">ASP.NET Core 2.x includes `System.Net.Mail`, which allows you to send email from your app.</span></span> <span data-ttu-id="59af4-171">Es wird empfohlen, dass Sie SendGrid oder eine andere e-Mail-Dienst verwenden, um e-Mail zu senden.</span><span class="sxs-lookup"><span data-stu-id="59af4-171">We recommend you use SendGrid or another email service to send email.</span></span>

<span data-ttu-id="59af4-172">Die [Optionen Muster](xref:fundamentals/configuration#options-config-objects) wird verwendet, um die Benutzer Konto- und Schlüsselauthentifizierung Einstellungen zugreifen.</span><span class="sxs-lookup"><span data-stu-id="59af4-172">The [Options pattern](xref:fundamentals/configuration#options-config-objects) is used to access the user account and key settings.</span></span> <span data-ttu-id="59af4-173">Weitere Informationen finden Sie unter [Konfiguration](xref:fundamentals/configuration#fundamentals-configuration).</span><span class="sxs-lookup"><span data-stu-id="59af4-173">For more information, see [configuration](xref:fundamentals/configuration#fundamentals-configuration).</span></span>

<span data-ttu-id="59af4-174">Erstellen Sie eine Klasse, um den e-Mail-Schlüssel abzurufen.</span><span class="sxs-lookup"><span data-stu-id="59af4-174">Create a class to fetch the secure email key.</span></span> <span data-ttu-id="59af4-175">Für dieses Beispiel die `AuthMessageSenderOptions` Klasse wird erstellt, der *Services/AuthMessageSenderOptions.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="59af4-175">For this sample, the `AuthMessageSenderOptions` class is created in the *Services/AuthMessageSenderOptions.cs* file.</span></span>

<span data-ttu-id="59af4-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="59af4-176">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/AuthMessageSenderOptions.cs?name=snippet1)]</span></span>

<span data-ttu-id="59af4-177">Legen Sie die `SendGridUser` und `SendGridKey` mit der [Secret-Manager-Tool](../app-secrets.md).</span><span class="sxs-lookup"><span data-stu-id="59af4-177">Set the `SendGridUser` and `SendGridKey` with the [secret-manager tool](../app-secrets.md).</span></span> <span data-ttu-id="59af4-178">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="59af4-178">For example:</span></span>

```none
C:\WebAppl\src\WebApp1>dotnet user-secrets set SendGridUser RickAndMSFT
info: Successfully saved SendGridUser = RickAndMSFT to the secret store.
```

<span data-ttu-id="59af4-179">Unter Windows, geheimen-Manager speichert die Schlüssel/Wert-Paare in einem *secrets.json* Datei im Verzeichnis %APPDATA%/Microsoft/UserSecrets/ < WebAppName UserSecretsId >.</span><span class="sxs-lookup"><span data-stu-id="59af4-179">On Windows, Secret Manager stores your keys/value pairs in a *secrets.json* file in the %APPDATA%/Microsoft/UserSecrets/<WebAppName-userSecretsId> directory.</span></span>

<span data-ttu-id="59af4-180">Der Inhalt der *secrets.json* Datei sind nicht verschlüsselt.</span><span class="sxs-lookup"><span data-stu-id="59af4-180">The contents of the *secrets.json* file are not encrypted.</span></span> <span data-ttu-id="59af4-181">Die *secrets.json* Datei wird unten gezeigt (die `SendGridKey` Wert entfernt wurde.)</span><span class="sxs-lookup"><span data-stu-id="59af4-181">The *secrets.json* file is shown below (the `SendGridKey` value has been removed.)</span></span>

  ```json
  {
    "SendGridUser": "RickAndMSFT",
    "SendGridKey": "<key removed>"
  }
  ```

### <a name="configure-startup-to-use-authmessagesenderoptions"></a><span data-ttu-id="59af4-182">Konfigurieren Sie Start AuthMessageSenderOptions Verwendung</span><span class="sxs-lookup"><span data-stu-id="59af4-182">Configure startup to use AuthMessageSenderOptions</span></span>

<span data-ttu-id="59af4-183">Hinzufügen `AuthMessageSenderOptions` dem Dienstcontainer am Ende der `ConfigureServices` Methode in der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="59af4-183">Add `AuthMessageSenderOptions` to the service container at the end of the `ConfigureServices` method in the *Startup.cs* file:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-184">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-184">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="59af4-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span><span class="sxs-lookup"><span data-stu-id="59af4-185">[!code-csharp[Main](accconfirm/sample/WebPW/Startup.cs?name=snippet1&highlight=18)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
<span data-ttu-id="59af4-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span><span class="sxs-lookup"><span data-stu-id="59af4-187">[!code-csharp[Main](accconfirm/sample/WebApp1/Startup.cs?name=snippet1&highlight=26)]</span></span>

---

### <a name="configure-the-authmessagesender-class"></a><span data-ttu-id="59af4-188">Konfigurieren Sie die AuthMessageSender-Klasse</span><span class="sxs-lookup"><span data-stu-id="59af4-188">Configure the AuthMessageSender class</span></span>

<span data-ttu-id="59af4-189">Dieses Lernprogramm veranschaulicht das Hinzufügen von e-Mail-Benachrichtigungen über [SendGrid](https://sendgrid.com/), aber Sie können e-Mail-Nachrichten mithilfe von SMTP und andere Mechanismen senden.</span><span class="sxs-lookup"><span data-stu-id="59af4-189">This tutorial shows how to add email notifications through [SendGrid](https://sendgrid.com/), but you can send email using SMTP and other mechanisms.</span></span>

* <span data-ttu-id="59af4-190">Installieren der `SendGrid` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="59af4-190">Install the `SendGrid` NuGet package.</span></span> <span data-ttu-id="59af4-191">Aus der Paket-Manager-Konsole, geben Sie den folgenden den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="59af4-191">From the Package Manager Console,  enter the following the following command:</span></span>

  `Install-Package SendGrid`

* <span data-ttu-id="59af4-192">Finden Sie unter [erste Schritte mit SendGrid kostenlos](https://sendgrid.com/free/) für ein kostenloses SendGrid-Konto registrieren.</span><span class="sxs-lookup"><span data-stu-id="59af4-192">See [Get Started with SendGrid for Free](https://sendgrid.com/free/) to register for a free SendGrid account.</span></span>

#### <a name="configure-sendgrid"></a><span data-ttu-id="59af4-193">Konfigurieren von SendGrid</span><span class="sxs-lookup"><span data-stu-id="59af4-193">Configure SendGrid</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-194">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-194">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="59af4-195">Fügen Sie Code in *Services/EmailSender.cs* ähnlich der folgenden SendGrid konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="59af4-195">Add code in *Services/EmailSender.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="59af4-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span><span class="sxs-lookup"><span data-stu-id="59af4-196">[!code-csharp[Main](accconfirm/sample/WebPW/Services/EmailSender.cs)]</span></span>


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
* <span data-ttu-id="59af4-198">Fügen Sie Code in *Services/MessageServices.cs* ähnlich der folgenden SendGrid konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="59af4-198">Add code in *Services/MessageServices.cs* similar to the following to configure SendGrid:</span></span>

<span data-ttu-id="59af4-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span><span class="sxs-lookup"><span data-stu-id="59af4-199">[!code-csharp[Main](accconfirm/sample/WebApp1/Services/MessageServices.cs)]</span></span>

---

## <a name="enable-account-confirmation-and-password-recovery"></a><span data-ttu-id="59af4-200">Konto bestätigen und Kennwort Wiederherstellung aktivieren</span><span class="sxs-lookup"><span data-stu-id="59af4-200">Enable account confirmation and password recovery</span></span>

<span data-ttu-id="59af4-201">Die Vorlage, den Code für die Wiederherstellung für Konto bestätigen und das Kennwort hat.</span><span class="sxs-lookup"><span data-stu-id="59af4-201">The template has the code for account confirmation and password recovery.</span></span> <span data-ttu-id="59af4-202">Suchen der `[HttpPost] Register` Methode in der *AccountController.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="59af4-202">Find the `[HttpPost] Register` method in the  *AccountController.cs* file.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="59af4-204">Verhindern Sie, dass neu registrierte Benutzern zur Verfügung wird automatisch durch die folgende Zeile auskommentiert angemeldet werden:</span><span class="sxs-lookup"><span data-stu-id="59af4-204">Prevent newly registered users from being automatically logged on by commenting out the following line:</span></span>

```csharp 
await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="59af4-205">Die vollständige Methode ist mit der geänderten Zeile hervorgehoben dargestellt:</span><span class="sxs-lookup"><span data-stu-id="59af4-205">The complete method is shown with the changed line highlighted:</span></span>

<span data-ttu-id="59af4-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="59af4-206">[!code-csharp[Main](accconfirm/sample/WebPW/Controllers/AccountController.cs?highlight=19&name=snippet_Register)]</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-207">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-207">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="59af4-208">Kommentieren Sie den Code zum Aktivieren der kontobestätigung.</span><span class="sxs-lookup"><span data-stu-id="59af4-208">Uncomment the code to enable account confirmation.</span></span>

<span data-ttu-id="59af4-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span><span class="sxs-lookup"><span data-stu-id="59af4-209">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=16-25&name=snippet_Register)]</span></span>

<span data-ttu-id="59af4-210">Hinweis: Wir haben auch einen neu registrierter Benutzer verhindert, dass automatisch durch die folgende Zeile auskommentiert angemeldet sein:</span><span class="sxs-lookup"><span data-stu-id="59af4-210">Note: We're also preventing a newly-registered user from being automatically logged on by commenting out the following line:</span></span>

```csharp 
//await _signInManager.SignInAsync(user, isPersistent: false);
```

<span data-ttu-id="59af4-211">Kennwortwiederherstellung aktivieren, indem Sie den Code in Auskommentieren der `ForgotPassword` Aktion in der *Controllers/AccountController.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="59af4-211">Enable password recovery by uncommenting the code in the `ForgotPassword` action in the *Controllers/AccountController.cs* file.</span></span>

<span data-ttu-id="59af4-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span><span class="sxs-lookup"><span data-stu-id="59af4-212">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=17-23&name=snippet_ForgotPassword)]</span></span>

<span data-ttu-id="59af4-213">Kommentieren Sie die Form-Elements im *Views/Account/ForgotPassword.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="59af4-213">Uncomment the form element in *Views/Account/ForgotPassword.cshtml*.</span></span> <span data-ttu-id="59af4-214">Möglicherweise möchten Sie entfernen die `<p> For more information on how to enable reset password ... </p>` Element, das einen Link zu dieser Artikel enthält.</span><span class="sxs-lookup"><span data-stu-id="59af4-214">You might want to remove the `<p> For more information on how to enable reset password ... </p>` element which contains a link to this article.</span></span>

<span data-ttu-id="59af4-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span><span class="sxs-lookup"><span data-stu-id="59af4-215">[!code-html[Main](accconfirm/sample/WebApp1/Views/Account/ForgotPassword.cshtml?highlight=7-10,12,28)]</span></span>

---

## <a name="register-confirm-email-and-reset-password"></a><span data-ttu-id="59af4-216">Registrieren, e-Mails zu bestätigen und Kennwort zurücksetzen</span><span class="sxs-lookup"><span data-stu-id="59af4-216">Register, confirm email, and reset password</span></span>

<span data-ttu-id="59af4-217">Führen Sie die Web-app, und Testen Sie die kontobestätigung und das Kennwort eine Wiederherstellung durchführen.</span><span class="sxs-lookup"><span data-stu-id="59af4-217">Run the web app, and test the account confirmation and password recovery flow.</span></span>

* <span data-ttu-id="59af4-218">Die app auszuführen und einen neuen Benutzer registrieren</span><span class="sxs-lookup"><span data-stu-id="59af4-218">Run the app and register a new user</span></span>

 ![Webansicht Anwendung Konto registrieren](accconfirm/_static/loginaccconfirm1.png)

* <span data-ttu-id="59af4-220">Suchen Sie nach der Link für die Bestätigung Ihrer e-Mail.</span><span class="sxs-lookup"><span data-stu-id="59af4-220">Check your email for the account confirmation link.</span></span> <span data-ttu-id="59af4-221">Finden Sie unter [Debuggen e-Mail](#debug) , wenn Sie die e-Mail nicht erhalten.</span><span class="sxs-lookup"><span data-stu-id="59af4-221">See [Debug email](#debug) if you don't get the email.</span></span>
* <span data-ttu-id="59af4-222">Klicken Sie auf den Link, um Ihre e-Mail-Adresse zu bestätigen.</span><span class="sxs-lookup"><span data-stu-id="59af4-222">Click the link to confirm your email.</span></span>
* <span data-ttu-id="59af4-223">Melden Sie sich mit Ihren e-Mail- und das Kennwort an.</span><span class="sxs-lookup"><span data-stu-id="59af4-223">Log in with your email and password.</span></span>
* <span data-ttu-id="59af4-224">Melden Sie sich ab.</span><span class="sxs-lookup"><span data-stu-id="59af4-224">Log off.</span></span>

### <a name="view-the-manage-page"></a><span data-ttu-id="59af4-225">Anzeigen der Seite "verwalten"</span><span class="sxs-lookup"><span data-stu-id="59af4-225">View the manage page</span></span>

<span data-ttu-id="59af4-226">Wählen Sie Ihren Benutzernamen im Browser: ![Browserfenster mit Benutzernamen](accconfirm/_static/un.png)</span><span class="sxs-lookup"><span data-stu-id="59af4-226">Select your user name in the browser: ![browser window with user name](accconfirm/_static/un.png)</span></span>

<span data-ttu-id="59af4-227">Möglicherweise müssen Sie die Navigationsleiste, um Benutzername erweitern.</span><span class="sxs-lookup"><span data-stu-id="59af4-227">You might need to expand the navbar to see user name.</span></span>

![Navigationsleiste](accconfirm/_static/x.png)

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="59af4-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="59af4-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="59af4-230">Die Seite "verwalten" wird angezeigt, mit der **Profil** Registerkarte ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="59af4-230">The manage page is displayed with the **Profile** tab selected.</span></span> <span data-ttu-id="59af4-231">Die **E-Mail** zeigt ein Kontrollkästchen, der angibt, der e-Mails bestätigt wurde.</span><span class="sxs-lookup"><span data-stu-id="59af4-231">The **Email** shows a check box indicating the email has been confirmed.</span></span> 

![Seite "verwalten"](accconfirm/_static/rick2.png)


# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="59af4-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="59af4-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="59af4-234">Dies wird auf dieser Seite später in diesem Lernprogramm geht.</span><span class="sxs-lookup"><span data-stu-id="59af4-234">We'll talk about this page later in the tutorial.</span></span>
<span data-ttu-id="59af4-235">![Seite "verwalten"](accconfirm/_static/rick2.png)</span><span class="sxs-lookup"><span data-stu-id="59af4-235">![manage page](accconfirm/_static/rick2.png)</span></span>

---

### <a name="test-password-reset"></a><span data-ttu-id="59af4-236">Test Zurücksetzen des Kennworts</span><span class="sxs-lookup"><span data-stu-id="59af4-236">Test password reset</span></span>

* <span data-ttu-id="59af4-237">Wenn Sie angemeldet sind, wählen Sie **Logout**.</span><span class="sxs-lookup"><span data-stu-id="59af4-237">If you're logged in, select **Logout**.</span></span>  
* <span data-ttu-id="59af4-238">Wählen Sie die **melden Sie sich** verknüpfen, und wählen Sie die **haben Sie Ihr Kennwort vergessen?** Link.</span><span class="sxs-lookup"><span data-stu-id="59af4-238">Select the **Log in** link and select the **Forgot your password?** link.</span></span>
* <span data-ttu-id="59af4-239">Geben Sie die e-Mail, die Sie verwendet, um das Konto zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="59af4-239">Enter the email you used to register the account.</span></span>
* <span data-ttu-id="59af4-240">Eine e-Mail mit einem Link zum Zurücksetzen Ihres Kennworts werden gesendet.</span><span class="sxs-lookup"><span data-stu-id="59af4-240">An email with a link to reset your password will be sent.</span></span> <span data-ttu-id="59af4-241">Überprüfen Sie Ihre e-Mail-Adresse, und klicken Sie auf den Link, um Ihr Kennwort zurückzusetzen.</span><span class="sxs-lookup"><span data-stu-id="59af4-241">Check your email and click the link to reset your password.</span></span>  <span data-ttu-id="59af4-242">Nachdem Sie Ihr Kennwort erfolgreich zurückgesetzt wurde, können Sie mit Ihrem e-Mail- und das neue Kennwort anmelden.</span><span class="sxs-lookup"><span data-stu-id="59af4-242">After your password has been successfully reset, you can login with your email and new password.</span></span>

<a name="debug"></a>

### <a name="debug-email"></a><span data-ttu-id="59af4-243">Debuggen-e-Mail</span><span class="sxs-lookup"><span data-stu-id="59af4-243">Debug email</span></span>

<span data-ttu-id="59af4-244">Wenn Sie e-Mail-nicht funktioniert:</span><span class="sxs-lookup"><span data-stu-id="59af4-244">If you can't get email working:</span></span>

* <span data-ttu-id="59af4-245">Überprüfen Sie die [e-Mail-Aktivität](https://sendgrid.com/docs/User_Guide/email_activity.html) Seite.</span><span class="sxs-lookup"><span data-stu-id="59af4-245">Review the [Email Activity](https://sendgrid.com/docs/User_Guide/email_activity.html) page.</span></span>
* <span data-ttu-id="59af4-246">Überprüfen Sie Ihre Spam-Ordner.</span><span class="sxs-lookup"><span data-stu-id="59af4-246">Check your spam folder.</span></span>
* <span data-ttu-id="59af4-247">Wiederholen Sie dann eine andere e-Mail-Alias für einen anderen e-Mail-Anbieter (Microsoft, Yahoo, Gmail, usw.).</span><span class="sxs-lookup"><span data-stu-id="59af4-247">Try another email alias on a different email provider (Microsoft, Yahoo, Gmail, etc.)</span></span>
* <span data-ttu-id="59af4-248">Erstellen einer [Konsolen-app zum Senden von e-Mail](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span><span class="sxs-lookup"><span data-stu-id="59af4-248">Create a [console app to send email](https://sendgrid.com/docs/Integrate/Code_Examples/v2_Mail/csharp.html).</span></span>
* <span data-ttu-id="59af4-249">Wiederholen Sie den senden an andere e-Mail-Konten.</span><span class="sxs-lookup"><span data-stu-id="59af4-249">Try sending to different email accounts.</span></span>

<span data-ttu-id="59af4-250">**Hinweis:** eine bewährte Sicherheitsmethode Produktion geheime Schlüssel in Test- und nicht zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="59af4-250">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="59af4-251">Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen.</span><span class="sxs-lookup"><span data-stu-id="59af4-251">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="59af4-252">Das Konfigurationssystem wird Setup beim Lesen von Schlüsseln von Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="59af4-252">The configuration system is setup to read keys from environment variables.</span></span>

## <a name="prevent-login-at-registration"></a><span data-ttu-id="59af4-253">Zu verhindern, dass die Anmeldung bei der Registrierung</span><span class="sxs-lookup"><span data-stu-id="59af4-253">Prevent login at registration</span></span>

<span data-ttu-id="59af4-254">Mit den aktuellen Vorlagen, sobald ein Benutzer das Registrierungsformular abgeschlossen ist. sie angemeldet sind (authentifiziert).</span><span class="sxs-lookup"><span data-stu-id="59af4-254">With the current templates, once a user completes the registration form, they are logged in (authenticated).</span></span> <span data-ttu-id="59af4-255">In der Regel möchten Sie ihre e-Mails zu überprüfen, bevor im Clientbrowser.</span><span class="sxs-lookup"><span data-stu-id="59af4-255">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="59af4-256">Im folgenden Abschnitt werden wir ändern Sie den Code erfordern neue Benutzer haben eine bestätigte e-Mail-Adresse aus, bevor sie angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="59af4-256">In the section below, we will modify the code to require new users have a confirmed email before they are logged in.</span></span> <span data-ttu-id="59af4-257">Update der `[HttpPost] Login` Aktion in der *AccountController.cs* Datei mit den folgenden hervorgehobenen Änderungen.</span><span class="sxs-lookup"><span data-stu-id="59af4-257">Update the `[HttpPost] Login` action in the *AccountController.cs* file with the following highlighted changes.</span></span>

<span data-ttu-id="59af4-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span><span class="sxs-lookup"><span data-stu-id="59af4-258">[!code-csharp[Main](accconfirm/sample/WebApp1/Controllers/AccountController.cs?highlight=11-21&name=snippet_Login)]</span></span>

<span data-ttu-id="59af4-259">**Hinweis:** eine bewährte Sicherheitsmethode Produktion geheime Schlüssel in Test- und nicht zu verwenden ist.</span><span class="sxs-lookup"><span data-stu-id="59af4-259">**Note:** A security best practice is to not use production secrets in test and development.</span></span> <span data-ttu-id="59af4-260">Wenn Sie die app in Azure veröffentlichen, können Sie die SendGrid-geheimen als Anwendungseinstellungen im Azure-Web-App-Portal festlegen.</span><span class="sxs-lookup"><span data-stu-id="59af4-260">If you publish the app to Azure, you can set the SendGrid secrets as application settings in the Azure Web App portal.</span></span> <span data-ttu-id="59af4-261">Das Konfigurationssystem wird Setup beim Lesen von Schlüsseln von Umgebungsvariablen.</span><span class="sxs-lookup"><span data-stu-id="59af4-261">The configuration system is setup to read keys from environment variables.</span></span>


## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="59af4-262">Kombinieren von sozialen und lokalen Anmeldekonten</span><span class="sxs-lookup"><span data-stu-id="59af4-262">Combine social and local login accounts</span></span>

<span data-ttu-id="59af4-263">Hinweis: Dieser Abschnitt gilt nur für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="59af4-263">Note: This section applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="59af4-264">Für ASP.NET Core 2.x, finden Sie unter [dies](https://github.com/aspnet/Docs/issues/3753) Problem.</span><span class="sxs-lookup"><span data-stu-id="59af4-264">For ASP.NET Core 2.x, see [this](https://github.com/aspnet/Docs/issues/3753) issue.</span></span>

<span data-ttu-id="59af4-265">Um in diesem Abschnitt abzuschließen, müssen Sie zunächst ein externen Authentifizierungsanbieters aktivieren.</span><span class="sxs-lookup"><span data-stu-id="59af4-265">To complete this section, you must first enable an external authentication provider.</span></span> <span data-ttu-id="59af4-266">Finden Sie unter [Aktivieren der Authentifizierung mithilfe von Facebook, Google und anderen externen Anbietern](social/index.md).</span><span class="sxs-lookup"><span data-stu-id="59af4-266">See [Enabling authentication using Facebook, Google and other external providers](social/index.md).</span></span>

<span data-ttu-id="59af4-267">Lokale und soziale Konten können durch Klicken auf die e-Mail-Link kombiniert werden.</span><span class="sxs-lookup"><span data-stu-id="59af4-267">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="59af4-268">In der folgenden Reihenfolge "RickAndMSFT@gmail.com" wird als eine lokale Anmeldung; zuerst erstellt jedoch zunächst erstellen Sie das Konto als sozialen Anmeldung werden können, und fügen Sie eine lokale Anmeldung hinzu.</span><span class="sxs-lookup"><span data-stu-id="59af4-268">In the following sequence, "RickAndMSFT@gmail.com" is first created as a local login; however, you can create the account as a social login first, then add a local login.</span></span>

![-Webanwendung: RickAndMSFT@gmail.com authentifizierte Benutzer](accconfirm/_static/rick.png)

<span data-ttu-id="59af4-270">Klicken Sie auf die **verwalten** Link.</span><span class="sxs-lookup"><span data-stu-id="59af4-270">Click on the **Manage** link.</span></span> <span data-ttu-id="59af4-271">Beachten Sie die externen 0 (social Anmeldungen) diesem Konto zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="59af4-271">Note the 0 external (social logins) associated with this account.</span></span>

![Verwalten von anzeigen](accconfirm/_static/manage.png)

<span data-ttu-id="59af4-273">Klicken Sie auf den Link, um einen anderen Anmeldenamen-Dienst, und akzeptieren Sie die app-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="59af4-273">Click the link to another login service and accept the app requests.</span></span> <span data-ttu-id="59af4-274">In der folgenden Abbildung wird die Facebook dem externen Authentifizierungsanbieter:</span><span class="sxs-lookup"><span data-stu-id="59af4-274">In the image below, Facebook is the external authentication provider:</span></span>

![Verwalten Sie externer Anmeldungen Ansicht Facebook auflisten](accconfirm/_static/fb.png)

<span data-ttu-id="59af4-276">Die beiden Konten wurden kombiniert.</span><span class="sxs-lookup"><span data-stu-id="59af4-276">The two accounts have been combined.</span></span> <span data-ttu-id="59af4-277">Sie werden können mit entweder Konto anmelden.</span><span class="sxs-lookup"><span data-stu-id="59af4-277">You will be able to log on with either account.</span></span> <span data-ttu-id="59af4-278">Sie sollten Ihren Benutzern lokale Konten hinzufügen, falls der Anmeldung sozialen Authentication-Dienst ausgefallen ist oder wahrscheinlicher haben sie Zugriff auf ihr Konto sozialen verloren.</span><span class="sxs-lookup"><span data-stu-id="59af4-278">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>
