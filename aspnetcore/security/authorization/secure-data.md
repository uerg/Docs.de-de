---
title: Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt
author: rick-anderson
description: Informationen Sie zum Erstellen einer Razor-Seiten-app mit Benutzerdaten durch Autorisierung geschützt. Umfasst HTTPS, Authentifizierung und Sicherheit, ASP.NET Core Identity.
ms.author: riande
ms.date: 01/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: 32ced054ba559e66de4a300137d56b7c81a4149b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276379"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="40f44-104">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="40f44-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="40f44-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="40f44-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="40f44-106">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Web-app mit Benutzerdaten durch Autorisierung geschützt wird.</span><span class="sxs-lookup"><span data-stu-id="40f44-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="40f44-107">Es zeigt eine Liste von Kontakten, die authentifizierte Benutzer (registrierten) erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="40f44-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="40f44-108">Es gibt drei Sicherheitsgruppen:</span><span class="sxs-lookup"><span data-stu-id="40f44-108">There are three security groups:</span></span>

* <span data-ttu-id="40f44-109">**Registrierte Benutzer** alle genehmigten Daten anzeigen und bearbeiten/löschen können ihre eigenen Daten.</span><span class="sxs-lookup"><span data-stu-id="40f44-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="40f44-110">**Manager** genehmigen oder ablehnen von Kontaktdaten können.</span><span class="sxs-lookup"><span data-stu-id="40f44-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="40f44-111">Nur genehmigte Kontakte werden für Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="40f44-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="40f44-112">**Administratoren** können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="40f44-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="40f44-113">In der folgenden Abbildung, Benutzer Rick (`rick@example.com`) angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="40f44-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="40f44-114">Rick kann nur genehmigte Kontakte anzeigen und **bearbeiten**/**löschen**/**neu erstellen** Links, um seine Kontakte.</span><span class="sxs-lookup"><span data-stu-id="40f44-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="40f44-115">Nur der letzte Datensatz erstellt, von Rick zeigt **bearbeiten** und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="40f44-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="40f44-116">Andere Benutzer sehen den letzten Datensatz, erst, wenn ein Manager oder der Administrator ändert sich der Status "Genehmigt".</span><span class="sxs-lookup"><span data-stu-id="40f44-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Bild beschrieben vor](secure-data/_static/rick.png)

<span data-ttu-id="40f44-118">In der folgenden Abbildung `manager@contoso.com` in und in der Rolle "Manager" signiert ist:</span><span class="sxs-lookup"><span data-stu-id="40f44-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Bild beschrieben vor](secure-data/_static/manager1.png)

<span data-ttu-id="40f44-120">Die folgende Abbildung zeigt den Managern Detailansicht eines Kontakts an:</span><span class="sxs-lookup"><span data-stu-id="40f44-120">The following image shows the managers details view of a contact:</span></span>

![Bild beschrieben vor](secure-data/_static/manager.png)

<span data-ttu-id="40f44-122">Die **genehmigen** und **ablehnen** Schaltflächen sind nur für Manager und Administratoren angezeigt.</span><span class="sxs-lookup"><span data-stu-id="40f44-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="40f44-123">In der folgenden Abbildung `admin@contoso.com` in und in der "Administratoren" angemeldet ist:</span><span class="sxs-lookup"><span data-stu-id="40f44-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Bild beschrieben vor](secure-data/_static/admin.png)

<span data-ttu-id="40f44-125">Der Administrator verfügt über alle Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="40f44-125">The administrator has all privileges.</span></span> <span data-ttu-id="40f44-126">Sie können eine beliebige kontaktieren, Lese-/bearbeiten/löschen und ändern Sie den Status von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="40f44-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="40f44-127">Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="40f44-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="40f44-128">Das Beispiel enthält die folgenden Autorisierung Handler:</span><span class="sxs-lookup"><span data-stu-id="40f44-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="40f44-129">`ContactIsOwnerAuthorizationHandler`: Stellt sicher, dass Benutzer nur ihre Daten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="40f44-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="40f44-130">`ContactManagerAuthorizationHandler`: Ermöglicht es Managern zum Genehmigen oder ablehnen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="40f44-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="40f44-131">`ContactAdministratorsAuthorizationHandler`: Ermöglicht Administratoren zum Genehmigen oder ablehnen von Kontakten und Bearbeiten/Löschen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="40f44-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40f44-132">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="40f44-132">Prerequisites</span></span>

<span data-ttu-id="40f44-133">In diesem Lernprogramm wird erweitert.</span><span class="sxs-lookup"><span data-stu-id="40f44-133">This tutorial is advanced.</span></span> <span data-ttu-id="40f44-134">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="40f44-134">You should be familiar with:</span></span>

* [<span data-ttu-id="40f44-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40f44-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="40f44-136">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="40f44-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="40f44-137">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="40f44-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="40f44-138">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="40f44-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="40f44-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="40f44-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="40f44-140">Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für ASP.NET Core MVC-Version.</span><span class="sxs-lookup"><span data-stu-id="40f44-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="40f44-141">Die Version 1.1 von ASP.NET Core dieses Lernprogramm ist [dies](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) Ordner.</span><span class="sxs-lookup"><span data-stu-id="40f44-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="40f44-142">Die 1.1 ASP.NET Core-Beispiel in ist der [Beispiele](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="40f44-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="40f44-143">Die Starter und abgeschlossene Anwendung</span><span class="sxs-lookup"><span data-stu-id="40f44-143">The starter and completed app</span></span>

<span data-ttu-id="40f44-144">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="40f44-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="40f44-145">[Test](#test-the-completed-app) abgeschlossene Anwendung, damit Sie mit der Sicherheitsfunktionen vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="40f44-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="40f44-146">Die Starter-app</span><span class="sxs-lookup"><span data-stu-id="40f44-146">The starter app</span></span>

<span data-ttu-id="40f44-147">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="40f44-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="40f44-148">Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und überprüfen Sie Sie erstellen, bearbeiten und Löschen eines Kontakts.</span><span class="sxs-lookup"><span data-stu-id="40f44-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="40f44-149">Sichern von Benutzerdaten</span><span class="sxs-lookup"><span data-stu-id="40f44-149">Secure user data</span></span>

<span data-ttu-id="40f44-150">In den folgenden Abschnitten sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app.</span><span class="sxs-lookup"><span data-stu-id="40f44-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="40f44-151">Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="40f44-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="40f44-152">Binden Sie die Kontaktdaten für den Benutzer</span><span class="sxs-lookup"><span data-stu-id="40f44-152">Tie the contact data to the user</span></span>

<span data-ttu-id="40f44-153">Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="40f44-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="40f44-154">Hinzufügen `OwnerID` und `ContactStatus` auf die `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="40f44-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="40f44-155">`OwnerID` ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="40f44-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="40f44-156">Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="40f44-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="40f44-157">Erstellen Sie eine neue Migration und Aktualisierung der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="40f44-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a><span data-ttu-id="40f44-158">Erfordern Sie, HTTPS und authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="40f44-158">Require HTTPS and authenticated users</span></span>

<span data-ttu-id="40f44-159">Hinzufügen [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) auf `Startup`:</span><span class="sxs-lookup"><span data-stu-id="40f44-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="40f44-160">In der `ConfigureServices` Methode der *Startup.cs* hinzufügen. die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Autorisierungsfilter:</span><span class="sxs-lookup"><span data-stu-id="40f44-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

<span data-ttu-id="40f44-161">Wenn Sie Visual Studio verwenden, aktivieren Sie HTTPS.</span><span class="sxs-lookup"><span data-stu-id="40f44-161">If you're using Visual Studio, enable HTTPS.</span></span>

<span data-ttu-id="40f44-162">Zum Umleiten von HTTP-Anforderungen zu HTTPS finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="40f44-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="40f44-163">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für HTTPS keine testen:</span><span class="sxs-lookup"><span data-stu-id="40f44-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

  <span data-ttu-id="40f44-164">Legen Sie `"LocalTest:skipHTTPS": true` in der *"appSettings". Developement.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="40f44-164">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="40f44-165">Erfordern Sie authentifizierte Benutzern</span><span class="sxs-lookup"><span data-stu-id="40f44-165">Require authenticated users</span></span>

<span data-ttu-id="40f44-166">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="40f44-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="40f44-167">Sie können die Authentifizierung auf Methodenebene Razor-Seite, Controller oder Aktionen mit abwählen der `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="40f44-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="40f44-168">Festlegen der Standardrichtlinie für die Authentifizierung Benutzer authentifiziert werden müssen schützt neu hinzugefügten Razor-Seiten und Controllern aus.</span><span class="sxs-lookup"><span data-stu-id="40f44-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="40f44-169">Mit Authentifizierung erforderlich, in der Standardeinstellung sicherer ist als der vertrauenden Seite auf das neue Domänencontroller und Razor-Seiten enthalten die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="40f44-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> 

<span data-ttu-id="40f44-170">Mit der Anforderung an alle Benutzer authentifiziert, die die [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) und [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) Aufrufe sind nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="40f44-170">With the requirement of all users authenticated, the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) and [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) calls are not required.</span></span>

<span data-ttu-id="40f44-171">Update `ConfigureServices` mit folgenden Änderungen:</span><span class="sxs-lookup"><span data-stu-id="40f44-171">Update `ConfigureServices` with the following changes:</span></span>

* <span data-ttu-id="40f44-172">Kommentieren Sie `AuthorizeFolder` und `AuthorizePage`.</span><span class="sxs-lookup"><span data-stu-id="40f44-172">Comment out `AuthorizeFolder` and `AuthorizePage`.</span></span>
* <span data-ttu-id="40f44-173">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="40f44-173">Set the default authentication policy to require users to be authenticated.</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

<span data-ttu-id="40f44-174">Hinzufügen [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) auf den Index zu, und wenden Sie sich an Seiten, sodass anonyme Benutzer Informationen über die Website abrufen können, bevor sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="40f44-174">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="40f44-175">Hinzufügen `[AllowAnonymous]` auf die [LoginModel und RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="40f44-175">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="40f44-176">Konfigurieren Sie das Testkonto an</span><span class="sxs-lookup"><span data-stu-id="40f44-176">Configure the test account</span></span>

<span data-ttu-id="40f44-177">Die `SeedData` Klasse erstellt zwei Konten: Administrator und den Manager.</span><span class="sxs-lookup"><span data-stu-id="40f44-177">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="40f44-178">Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Festlegen eines Kennworts für diese Konten.</span><span class="sxs-lookup"><span data-stu-id="40f44-178">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="40f44-179">Legen Sie das Kennwort aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*):</span><span class="sxs-lookup"><span data-stu-id="40f44-179">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="40f44-180">Wenn Sie ein sicheres Kennwort verwenden, eine Ausnahme wird ausgelöst, wenn `SeedData.Initialize` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="40f44-180">If you don't use a strong password, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="40f44-181">Update `Main` Testkennwort verwenden:</span><span class="sxs-lookup"><span data-stu-id="40f44-181">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="40f44-182">Erstellen Sie die Testkonten und aktualisieren die Kontakte</span><span class="sxs-lookup"><span data-stu-id="40f44-182">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="40f44-183">Update der `Initialize` Methode in der `SeedData` Klasse, um die Testkonten zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="40f44-183">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="40f44-184">Fügen Sie die Administrator-Benutzer-ID und `ContactStatus` für Kontakte.</span><span class="sxs-lookup"><span data-stu-id="40f44-184">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="40f44-185">Stellen Sie einen der Kontakte "Übermittelt" und eine "abgelehnt".</span><span class="sxs-lookup"><span data-stu-id="40f44-185">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="40f44-186">Fügen Sie die Benutzer-ID und den Status für alle Kontakte hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f44-186">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="40f44-187">Nur ein Kontakt wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="40f44-187">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="40f44-188">Erstellen Sie, Besitzer, Manager und Administrator Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="40f44-188">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="40f44-189">Erstellen einer `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="40f44-189">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="40f44-190">Die `ContactIsOwnerAuthorizationHandler` stellt sicher, dass der Benutzer, die auf einer Ressource fungiert die Ressource besitzt.</span><span class="sxs-lookup"><span data-stu-id="40f44-190">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="40f44-191">Die `ContactIsOwnerAuthorizationHandler` Aufrufe [Kontext. Erfolgreich](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts.</span><span class="sxs-lookup"><span data-stu-id="40f44-191">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="40f44-192">Autorisierung Handler in der Regel:</span><span class="sxs-lookup"><span data-stu-id="40f44-192">Authorization handlers generally:</span></span>

* <span data-ttu-id="40f44-193">Zurückgeben `context.Succeed` Wenn die Anforderungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="40f44-193">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="40f44-194">Zurückgeben `Task.CompletedTask` bei ist nicht erfüllt.</span><span class="sxs-lookup"><span data-stu-id="40f44-194">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="40f44-195">`Task.CompletedTask` ist weder Erfolg oder Misserfolg&mdash;können andere Autorisierung Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="40f44-195">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="40f44-196">Wenn Sie explizit fehlschlägt müssen, zurückgeben [Kontext. Fehlschlagen](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="40f44-196">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="40f44-197">Die app ermöglicht Kontakt Besitzer zu bearbeiten, löschen, erstellen ihre eigenen Daten an.</span><span class="sxs-lookup"><span data-stu-id="40f44-197">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="40f44-198">`ContactIsOwnerAuthorizationHandler` Überprüfen Sie den Vorgang im Parameters Anforderung übergeben muss nicht.</span><span class="sxs-lookup"><span data-stu-id="40f44-198">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="40f44-199">Erstellen Sie einen Ereignishandler der Manager-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="40f44-199">Create a manager authorization handler</span></span>

<span data-ttu-id="40f44-200">Erstellen einer `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="40f44-200">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="40f44-201">Die `ContactManagerAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Manager.</span><span class="sxs-lookup"><span data-stu-id="40f44-201">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="40f44-202">Nur Manager können genehmigen oder Ablehnen der Änderungen am Inhalt (neue oder geänderte).</span><span class="sxs-lookup"><span data-stu-id="40f44-202">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="40f44-203">Erstellen Sie einen Administrator-Autorisierung-Ereignishandler</span><span class="sxs-lookup"><span data-stu-id="40f44-203">Create an administrator authorization handler</span></span>

<span data-ttu-id="40f44-204">Erstellen einer `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="40f44-204">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="40f44-205">Die `ContactAdministratorsAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Administrator.</span><span class="sxs-lookup"><span data-stu-id="40f44-205">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="40f44-206">Administratoren kann alle Vorgänge tun.</span><span class="sxs-lookup"><span data-stu-id="40f44-206">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="40f44-207">Registrieren Sie die Authorization-Handler</span><span class="sxs-lookup"><span data-stu-id="40f44-207">Register the authorization handlers</span></span>

<span data-ttu-id="40f44-208">Verwendung von Entity Framework Core Services müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="40f44-208">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="40f44-209">Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="40f44-209">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="40f44-210">Damit sie verfügbar sind, registrieren Sie die Handler mit die Auflistung der `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="40f44-210">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="40f44-211">Fügen Sie den folgenden Code am Ende der `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="40f44-211">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="40f44-212">`ContactAdministratorsAuthorizationHandler` und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="40f44-212">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="40f44-213">Sie Singletons sind, da sie nicht EF verwenden und alle erforderlichen Informationen in den `Context` Parameter von der `HandleRequirementAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="40f44-213">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="40f44-214">Support-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="40f44-214">Support authorization</span></span>

<span data-ttu-id="40f44-215">In diesem Abschnitt Aktualisieren der Razor-Seiten und fügen Sie eine Operations Anforderungen-Klasse.</span><span class="sxs-lookup"><span data-stu-id="40f44-215">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="40f44-216">Überprüfen Sie die Kontaktinformationen Vorgänge Anforderungen-Klasse</span><span class="sxs-lookup"><span data-stu-id="40f44-216">Review the contact operations requirements class</span></span>

<span data-ttu-id="40f44-217">Überprüfen Sie die `ContactOperations` Klasse.</span><span class="sxs-lookup"><span data-stu-id="40f44-217">Review the `ContactOperations` class.</span></span> <span data-ttu-id="40f44-218">Diese Klasse enthält die Anforderungen der app unterstützt:</span><span class="sxs-lookup"><span data-stu-id="40f44-218">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="40f44-219">Erstellen Sie eine Basisklasse für die Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="40f44-219">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="40f44-220">Erstellen Sie eine Basisklasse, die in den Kontakten Razor-Seiten verwendeten Dienste enthält.</span><span class="sxs-lookup"><span data-stu-id="40f44-220">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="40f44-221">Die Basisklasse setzt diese Initialisierungscode an einem zentralen Ort:</span><span class="sxs-lookup"><span data-stu-id="40f44-221">The base class puts that initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="40f44-222">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="40f44-222">The preceding code:</span></span>

* <span data-ttu-id="40f44-223">Fügt der `IAuthorizationService` Dienst den Zugriff auf die Autorisierung Handler.</span><span class="sxs-lookup"><span data-stu-id="40f44-223">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="40f44-224">Fügt die Identität `UserManager` Dienst.</span><span class="sxs-lookup"><span data-stu-id="40f44-224">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="40f44-225">Fügen Sie die `ApplicationDbContext` hinzu.</span><span class="sxs-lookup"><span data-stu-id="40f44-225">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="40f44-226">Aktualisieren der CreateModel</span><span class="sxs-lookup"><span data-stu-id="40f44-226">Update the CreateModel</span></span>

<span data-ttu-id="40f44-227">Aktualisieren den erstellen Seite Modell Konstruktor verwendet den `DI_BasePageModel` Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="40f44-227">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="40f44-228">Update der `CreateModel.OnPostAsync` aufzurufende Methode:</span><span class="sxs-lookup"><span data-stu-id="40f44-228">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="40f44-229">Fügen Sie die Benutzer-ID an den `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="40f44-229">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="40f44-230">Rufen Sie den Handler Autorisierung, um sicherzustellen, dass der Benutzer die Berechtigung zum Erstellen von Kontakten verfügt.</span><span class="sxs-lookup"><span data-stu-id="40f44-230">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="40f44-231">Aktualisieren Sie die IndexModel</span><span class="sxs-lookup"><span data-stu-id="40f44-231">Update the IndexModel</span></span>

<span data-ttu-id="40f44-232">Update der `OnGetAsync` Methode, sodass nur genehmigte Kontakte für allgemeine Benutzer angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="40f44-232">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="40f44-233">Aktualisieren Sie die EditModel</span><span class="sxs-lookup"><span data-stu-id="40f44-233">Update the EditModel</span></span>

<span data-ttu-id="40f44-234">Fügen Sie einen Handler Autorisierung, um sicherzustellen, dass der Benutzer im Besitz des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="40f44-234">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="40f44-235">Da Ressource Autorisierung überprüft wird, die `[Authorize]` Attribut ist nicht ausreichend.</span><span class="sxs-lookup"><span data-stu-id="40f44-235">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="40f44-236">Die app hat keinen Zugriff auf die Ressource, wenn Attribute ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="40f44-236">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="40f44-237">Autorisierung ressourcenbasierte muss zwingend erforderlich.</span><span class="sxs-lookup"><span data-stu-id="40f44-237">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="40f44-238">Überprüfungen müssen ausgeführt werden, sobald die app Zugriff auf die Ressource durch Laden in das Modell oder durch Laden in den Handler selbst hat.</span><span class="sxs-lookup"><span data-stu-id="40f44-238">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="40f44-239">Durch Übergeben der Ressourcenschlüssel greifen Sie häufig die Ressource.</span><span class="sxs-lookup"><span data-stu-id="40f44-239">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="40f44-240">Aktualisieren Sie die DeleteModel</span><span class="sxs-lookup"><span data-stu-id="40f44-240">Update the DeleteModel</span></span>

<span data-ttu-id="40f44-241">Aktualisieren Sie das Modell löschen Seite, um die Autorisierung Handler zu verwenden, um sicherzustellen, dass der Benutzer über die Löschberechtigung für den Kontakt verfügt.</span><span class="sxs-lookup"><span data-stu-id="40f44-241">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="40f44-242">Fügen Sie dem Prüfdienst in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="40f44-242">Inject the authorization service into the views</span></span>

<span data-ttu-id="40f44-243">Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Daten, die der Benutzer ändern kann.</span><span class="sxs-lookup"><span data-stu-id="40f44-243">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="40f44-244">Die Benutzeroberfläche ist durch Anwenden des Autorisierung Handlers, die den Ansichten fest.</span><span class="sxs-lookup"><span data-stu-id="40f44-244">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="40f44-245">Fügen Sie dem Prüfdienst in der *Views/_ViewImports.cshtml* Datei, damit er für alle Ansichten verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="40f44-245">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="40f44-246">Das vorhergehende Markup fügt mehrere `using` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="40f44-246">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="40f44-247">Update der **bearbeiten** und **löschen** verknüpft *Pages/Contacts/Index.cshtml* , sodass sie nur für Benutzer mit den entsprechenden Berechtigungen gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="40f44-247">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="40f44-248">Ausblenden von Links von Benutzern, die keine Berechtigung zum Ändern von Daten haben Sichern nicht die app.</span><span class="sxs-lookup"><span data-stu-id="40f44-248">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="40f44-249">Durch das Ausblenden Links wird die app durch Anzeigen der einzige gültige Links Benutzerfreundlicher.</span><span class="sxs-lookup"><span data-stu-id="40f44-249">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="40f44-250">Benutzer können die generierten URLs zum Aufrufen bearbeiten und Löschen von Operationen mit Daten, die sie besitzen keine hack.</span><span class="sxs-lookup"><span data-stu-id="40f44-250">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="40f44-251">Der Razor-Seite oder ein Domänencontroller muss zugriffsüberprüfungen zum Schutz der Daten erzwingen.</span><span class="sxs-lookup"><span data-stu-id="40f44-251">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="40f44-252">Updatedetails</span><span class="sxs-lookup"><span data-stu-id="40f44-252">Update Details</span></span>

<span data-ttu-id="40f44-253">Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:</span><span class="sxs-lookup"><span data-stu-id="40f44-253">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="40f44-254">Aktualisieren Sie das Modell Details:</span><span class="sxs-lookup"><span data-stu-id="40f44-254">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="40f44-255">Abgeschlossene Anwendung testen</span><span class="sxs-lookup"><span data-stu-id="40f44-255">Test the completed app</span></span>

<span data-ttu-id="40f44-256">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für HTTPS keine testen:</span><span class="sxs-lookup"><span data-stu-id="40f44-256">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for HTTPS:</span></span>

* <span data-ttu-id="40f44-257">Legen Sie `"LocalTest:skipHTTPS": true` in der *"appSettings". Developement.JSON* Datei überspringen Sie die HTTPS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="40f44-257">Set `"LocalTest:skipHTTPS": true` in the *appsettings.Developement.json* file to skip the HTTPS requirement.</span></span> <span data-ttu-id="40f44-258">Skip HTTPS nur auf einem Entwicklungscomputer.</span><span class="sxs-lookup"><span data-stu-id="40f44-258">Skip HTTPS only on a development machine.</span></span>

<span data-ttu-id="40f44-259">Wenn die app Kontakte besitzt:</span><span class="sxs-lookup"><span data-stu-id="40f44-259">If the app has contacts:</span></span>

* <span data-ttu-id="40f44-260">Löschen aller Datensätze in der `Contact` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="40f44-260">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="40f44-261">Starten Sie die app Ausgangswert für die Datenbank neu.</span><span class="sxs-lookup"><span data-stu-id="40f44-261">Restart the app to seed the database.</span></span>

<span data-ttu-id="40f44-262">Registrieren Sie einen Benutzer zum Durchsuchen der Kontakte.</span><span class="sxs-lookup"><span data-stu-id="40f44-262">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="40f44-263">Eine einfache Möglichkeit zum Testen Sie die abgeschlossenen app wird auf drei verschiedenen Browsern (oder inkognito/InPrivate-Versionen) zu starten.</span><span class="sxs-lookup"><span data-stu-id="40f44-263">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="40f44-264">In einem Browser eingeben, einen neuen Benutzer registrieren (z. B. `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="40f44-264">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="40f44-265">Melden Sie sich mit einem anderen Benutzer jedem Browser an.</span><span class="sxs-lookup"><span data-stu-id="40f44-265">Sign in to each browser with a different user.</span></span> <span data-ttu-id="40f44-266">Überprüfen Sie die folgenden Vorgänge aus:</span><span class="sxs-lookup"><span data-stu-id="40f44-266">Verify the following operations:</span></span>

* <span data-ttu-id="40f44-267">Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="40f44-267">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="40f44-268">Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="40f44-268">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="40f44-269">Manager können genehmigen oder ablehnen Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="40f44-269">Managers can approve or reject contact data.</span></span> <span data-ttu-id="40f44-270">Die `Details` Ansicht zeigt **genehmigen** und **ablehnen** Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="40f44-270">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="40f44-271">Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="40f44-271">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="40f44-272">Benutzer</span><span class="sxs-lookup"><span data-stu-id="40f44-272">User</span></span>| <span data-ttu-id="40f44-273">Optionen</span><span class="sxs-lookup"><span data-stu-id="40f44-273">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="40f44-274">Können bearbeiten/eigene Daten löschen</span><span class="sxs-lookup"><span data-stu-id="40f44-274">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="40f44-275">Können ablehnen/genehmigen und bearbeiten/löschen, die Daten besitzen</span><span class="sxs-lookup"><span data-stu-id="40f44-275">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="40f44-276">Können bearbeiten/löschen und alle Daten genehmigen/ablehnen</span><span class="sxs-lookup"><span data-stu-id="40f44-276">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="40f44-277">Erstellen eines Kontakts in der Administrator-Browser.</span><span class="sxs-lookup"><span data-stu-id="40f44-277">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="40f44-278">Kopieren Sie die URL zum Löschen und Bearbeiten von den Administrator wenden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="40f44-278">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="40f44-279">Fügen Sie diese Links in den Browser des Testbenutzers zu überprüfen, ob der Testbenutzer diese Vorgänge ausführen kann nicht aus.</span><span class="sxs-lookup"><span data-stu-id="40f44-279">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="40f44-280">Erstellen der Starter-app</span><span class="sxs-lookup"><span data-stu-id="40f44-280">Create the starter app</span></span>

* <span data-ttu-id="40f44-281">Erstellen Sie eine Razor-Seiten-app mit dem Namen "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="40f44-281">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="40f44-282">Erstellen Sie die app mit **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="40f44-282">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="40f44-283">Nennen Sie sie "ContactManager", sodass Ihr Namespace den Namespace, die im Beispiel verwendeten entspricht.</span><span class="sxs-lookup"><span data-stu-id="40f44-283">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

::: moniker range=">= aspnetcore-2.1"

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

  [!INCLUDE[](~/includes/webapp-alias-notice.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

::: moniker-end

  * <span data-ttu-id="40f44-285">`-uld` Gibt an, anstelle von SQLite LocalDB</span><span class="sxs-lookup"><span data-stu-id="40f44-285">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="40f44-286">Fügen Sie die folgenden `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="40f44-286">Add the following `Contact` model:</span></span>

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="40f44-287">Gerüst der `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="40f44-287">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="40f44-288">Update der **ContactManager** Verankern der *Pages/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="40f44-288">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="40f44-289">Erstellen der anfänglichen Migrations und die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="40f44-289">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="40f44-290">Testen der app durch erstellen, bearbeiten und Löschen eines Kontakts</span><span class="sxs-lookup"><span data-stu-id="40f44-290">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="40f44-291">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="40f44-291">Seed the database</span></span>

<span data-ttu-id="40f44-292">Hinzufügen der `SeedData` Klasse, um die *Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="40f44-292">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="40f44-293">Wenn Sie das Beispiel heruntergeladen haben, können Sie kopieren die *SeedData.cs* Datei wird in der *Daten* Ordner des Startprojekts.</span><span class="sxs-lookup"><span data-stu-id="40f44-293">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="40f44-294">Rufen Sie `SeedData.Initialize` aus `Main`:</span><span class="sxs-lookup"><span data-stu-id="40f44-294">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="40f44-295">Testen Sie, ob die Anwendung die Datenbank mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="40f44-295">Test that the app seeded the database.</span></span> <span data-ttu-id="40f44-296">Wenn alle Zeilen in den Kontakt DB vorhanden sind, nicht der Seed-Methode ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="40f44-296">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="40f44-297">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="40f44-297">Additional resources</span></span>

* <span data-ttu-id="40f44-298">[ASP.NET Core Autorisierung Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="40f44-298">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="40f44-299">Diese Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.</span><span class="sxs-lookup"><span data-stu-id="40f44-299">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="40f44-300">Autorisierung in ASP.NET Core: einfach, anspruchsbasierte und benutzerdefinierten Rolle</span><span class="sxs-lookup"><span data-stu-id="40f44-300">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="40f44-301">Benutzerdefinierte, richtlinienbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="40f44-301">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
