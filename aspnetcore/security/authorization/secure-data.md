---
title: "Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt"
author: rick-anderson
description: "Informationen Sie zum Erstellen einer Razor-Seiten-app mit Benutzerdaten durch Autorisierung geschützt. Enthält SSL, Authentifizierung und Sicherheit, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 6333082a2b2b4f6d3f1ce2afc600b4203a0f5dca
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="93e44-104">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="93e44-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="93e44-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="93e44-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="93e44-106">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Web-app mit Benutzerdaten durch Autorisierung geschützt wird.</span><span class="sxs-lookup"><span data-stu-id="93e44-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="93e44-107">Es zeigt eine Liste von Kontakten, die authentifizierte Benutzer (registrierten) erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="93e44-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="93e44-108">Es gibt drei Sicherheitsgruppen:</span><span class="sxs-lookup"><span data-stu-id="93e44-108">There are three security groups:</span></span>

* <span data-ttu-id="93e44-109">**Registrierte Benutzer** alle genehmigten Daten anzeigen und bearbeiten/löschen können ihre eigenen Daten.</span><span class="sxs-lookup"><span data-stu-id="93e44-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="93e44-110">**Manager** genehmigen oder ablehnen von Kontaktdaten können.</span><span class="sxs-lookup"><span data-stu-id="93e44-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="93e44-111">Nur genehmigte Kontakte werden für Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="93e44-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="93e44-112">**Administratoren** können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="93e44-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="93e44-113">In der folgenden Abbildung, Benutzer Rick (`rick@example.com`) angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="93e44-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="93e44-114">Rick kann nur genehmigte Kontakte anzeigen und **bearbeiten**/**löschen**/**neu erstellen** Links, um seine Kontakte.</span><span class="sxs-lookup"><span data-stu-id="93e44-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="93e44-115">Nur der letzte Datensatz erstellt, von Rick zeigt **bearbeiten** und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="93e44-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="93e44-116">Andere Benutzer sehen den letzten Datensatz, erst, wenn ein Manager oder der Administrator ändert sich der Status "Genehmigt".</span><span class="sxs-lookup"><span data-stu-id="93e44-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Bild beschrieben vor](secure-data/_static/rick.png)

<span data-ttu-id="93e44-118">In der folgenden Abbildung `manager@contoso.com` in und in der Rolle "Manager" signiert ist:</span><span class="sxs-lookup"><span data-stu-id="93e44-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Bild beschrieben vor](secure-data/_static/manager1.png)

<span data-ttu-id="93e44-120">Die folgende Abbildung zeigt den Managern Detailansicht eines Kontakts an:</span><span class="sxs-lookup"><span data-stu-id="93e44-120">The following image shows the managers details view of a contact:</span></span>

![Bild beschrieben vor](secure-data/_static/manager.png)

<span data-ttu-id="93e44-122">Die **genehmigen** und **ablehnen** Schaltflächen sind nur für Manager und Administratoren angezeigt.</span><span class="sxs-lookup"><span data-stu-id="93e44-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="93e44-123">In der folgenden Abbildung `admin@contoso.com` in und in der "Administratoren" angemeldet ist:</span><span class="sxs-lookup"><span data-stu-id="93e44-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Bild beschrieben vor](secure-data/_static/admin.png)

<span data-ttu-id="93e44-125">Der Administrator verfügt über alle Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="93e44-125">The administrator has all privileges.</span></span> <span data-ttu-id="93e44-126">Sie können eine beliebige kontaktieren, Lese-/bearbeiten/löschen und ändern Sie den Status von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="93e44-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="93e44-127">Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="93e44-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="93e44-128">Das Beispiel enthält die folgenden Autorisierung Handler:</span><span class="sxs-lookup"><span data-stu-id="93e44-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="93e44-129">`ContactIsOwnerAuthorizationHandler`: Stellt sicher, dass Benutzer nur ihre Daten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="93e44-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="93e44-130">`ContactManagerAuthorizationHandler`: Ermöglicht es Managern zum Genehmigen oder ablehnen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="93e44-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="93e44-131">`ContactAdministratorsAuthorizationHandler`: Ermöglicht Administratoren zum Genehmigen oder ablehnen von Kontakten und Bearbeiten/Löschen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="93e44-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93e44-132">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="93e44-132">Prerequisites</span></span>

<span data-ttu-id="93e44-133">In diesem Lernprogramm wird erweitert.</span><span class="sxs-lookup"><span data-stu-id="93e44-133">This tutorial is advanced.</span></span> <span data-ttu-id="93e44-134">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="93e44-134">You should be familiar with:</span></span>

* [<span data-ttu-id="93e44-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93e44-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="93e44-136">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="93e44-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="93e44-137">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="93e44-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="93e44-138">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="93e44-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="93e44-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="93e44-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="93e44-140">Finden Sie unter [diese PDF-Datei](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für ASP.NET Core MVC-Version.</span><span class="sxs-lookup"><span data-stu-id="93e44-140">See [this PDF file](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="93e44-141">Die Version 1.1 von ASP.NET Core dieses Lernprogramm ist [dies](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) Ordner.</span><span class="sxs-lookup"><span data-stu-id="93e44-141">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="93e44-142">Die 1.1 ASP.NET Core-Beispiel in ist der [Beispiele](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="93e44-142">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="93e44-143">Die Starter und abgeschlossene Anwendung</span><span class="sxs-lookup"><span data-stu-id="93e44-143">The starter and completed app</span></span>

<span data-ttu-id="93e44-144">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="93e44-144">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="93e44-145">[Test](#test-the-completed-app) abgeschlossene Anwendung, damit Sie mit der Sicherheitsfunktionen vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="93e44-145">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="93e44-146">Die Starter-app</span><span class="sxs-lookup"><span data-stu-id="93e44-146">The starter app</span></span>

<span data-ttu-id="93e44-147">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="93e44-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="93e44-148">Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und überprüfen Sie Sie erstellen, bearbeiten und Löschen eines Kontakts.</span><span class="sxs-lookup"><span data-stu-id="93e44-148">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="93e44-149">Sichern von Benutzerdaten</span><span class="sxs-lookup"><span data-stu-id="93e44-149">Secure user data</span></span>

<span data-ttu-id="93e44-150">In den folgenden Abschnitten sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app.</span><span class="sxs-lookup"><span data-stu-id="93e44-150">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="93e44-151">Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="93e44-151">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="93e44-152">Binden Sie die Kontaktdaten für den Benutzer</span><span class="sxs-lookup"><span data-stu-id="93e44-152">Tie the contact data to the user</span></span>

<span data-ttu-id="93e44-153">Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="93e44-153">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="93e44-154">Hinzufügen `OwnerID` und `ContactStatus` auf die `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="93e44-154">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="93e44-155">`OwnerID`ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="93e44-155">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="93e44-156">Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="93e44-156">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="93e44-157">Erstellen Sie eine neue Migration und Aktualisierung der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="93e44-157">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="93e44-158">Erfordern von SSL und authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="93e44-158">Require SSL and authenticated users</span></span>

<span data-ttu-id="93e44-159">Hinzufügen [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) auf `Startup`:</span><span class="sxs-lookup"><span data-stu-id="93e44-159">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="93e44-160">In der `ConfigureServices` Methode der *Startup.cs* hinzufügen. die [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Autorisierungsfilter:</span><span class="sxs-lookup"><span data-stu-id="93e44-160">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-999)]

<span data-ttu-id="93e44-161">Wenn Sie Visual Studio verwenden, aktivieren Sie SSL.</span><span class="sxs-lookup"><span data-stu-id="93e44-161">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="93e44-162">Zum Umleiten von HTTP-Anforderungen zu HTTPS finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="93e44-162">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="93e44-163">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="93e44-163">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="93e44-164">Legen Sie `"LocalTest:skipSSL": true` in der *"appSettings". Developement.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="93e44-164">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="93e44-165">Erfordern Sie authentifizierte Benutzern</span><span class="sxs-lookup"><span data-stu-id="93e44-165">Require authenticated users</span></span>

<span data-ttu-id="93e44-166">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="93e44-166">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="93e44-167">Sie können die Authentifizierung auf Methodenebene Razor-Seite, Controller oder Aktionen mit abwählen der `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="93e44-167">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="93e44-168">Festlegen der Standardrichtlinie für die Authentifizierung Benutzer authentifiziert werden müssen schützt neu hinzugefügten Razor-Seiten und Controllern aus.</span><span class="sxs-lookup"><span data-stu-id="93e44-168">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="93e44-169">Mit Authentifizierung erforderlich, in der Standardeinstellung sicherer ist als der vertrauenden Seite auf das neue Domänencontroller und Razor-Seiten enthalten die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="93e44-169">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="93e44-170">Fügen Sie Folgendes an der `ConfigureServices` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="93e44-170">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-999)]

<span data-ttu-id="93e44-171">Hinzufügen [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) auf den Index zu, und wenden Sie sich an Seiten, sodass anonyme Benutzer Informationen über die Website abrufen können, bevor sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="93e44-171">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="93e44-172">Hinzufügen `[AllowAnonymous]` auf die [LoginModel und RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="93e44-172">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="93e44-173">Konfigurieren Sie das Testkonto an</span><span class="sxs-lookup"><span data-stu-id="93e44-173">Configure the test account</span></span>

<span data-ttu-id="93e44-174">Die `SeedData` Klasse erstellt zwei Konten: Administrator und den Manager.</span><span class="sxs-lookup"><span data-stu-id="93e44-174">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="93e44-175">Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Festlegen eines Kennworts für diese Konten.</span><span class="sxs-lookup"><span data-stu-id="93e44-175">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="93e44-176">Legen Sie das Kennwort aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*):</span><span class="sxs-lookup"><span data-stu-id="93e44-176">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="93e44-177">Update `Main` Testkennwort verwenden:</span><span class="sxs-lookup"><span data-stu-id="93e44-177">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="93e44-178">Erstellen Sie die Testkonten und aktualisieren die Kontakte</span><span class="sxs-lookup"><span data-stu-id="93e44-178">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="93e44-179">Update der `Initialize` Methode in der `SeedData` Klasse, um die Testkonten zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="93e44-179">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="93e44-180">Fügen Sie die Administrator-Benutzer-ID und `ContactStatus` für Kontakte.</span><span class="sxs-lookup"><span data-stu-id="93e44-180">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="93e44-181">Stellen Sie einen der Kontakte "Übermittelt" und eine "abgelehnt".</span><span class="sxs-lookup"><span data-stu-id="93e44-181">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="93e44-182">Fügen Sie die Benutzer-ID und den Status für alle Kontakte hinzu.</span><span class="sxs-lookup"><span data-stu-id="93e44-182">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="93e44-183">Nur ein Kontakt wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="93e44-183">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="93e44-184">Erstellen Sie, Besitzer, Manager und Administrator Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="93e44-184">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="93e44-185">Erstellen einer `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="93e44-185">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="93e44-186">Die `ContactIsOwnerAuthorizationHandler` stellt sicher, dass der Benutzer, die auf einer Ressource fungiert die Ressource besitzt.</span><span class="sxs-lookup"><span data-stu-id="93e44-186">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="93e44-187">Die `ContactIsOwnerAuthorizationHandler` Aufrufe [Kontext. Erfolgreich](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts.</span><span class="sxs-lookup"><span data-stu-id="93e44-187">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="93e44-188">Autorisierung Handler in der Regel:</span><span class="sxs-lookup"><span data-stu-id="93e44-188">Authorization handlers generally:</span></span>

* <span data-ttu-id="93e44-189">Zurückgeben `context.Succeed` Wenn die Anforderungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="93e44-189">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="93e44-190">Zurückgeben `Task.CompletedTask` bei ist nicht erfüllt.</span><span class="sxs-lookup"><span data-stu-id="93e44-190">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="93e44-191">`Task.CompletedTask`ist weder Erfolg oder Misserfolg&mdash;können andere Autorisierung Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="93e44-191">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="93e44-192">Wenn Sie explizit fehlschlägt müssen, zurückgeben [Kontext. Fehlschlagen](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="93e44-192">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="93e44-193">Die app ermöglicht Kontakt Besitzer zu bearbeiten, löschen, erstellen ihre eigenen Daten an.</span><span class="sxs-lookup"><span data-stu-id="93e44-193">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="93e44-194">`ContactIsOwnerAuthorizationHandler`Überprüfen Sie den Vorgang im Parameters Anforderung übergeben muss nicht.</span><span class="sxs-lookup"><span data-stu-id="93e44-194">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="93e44-195">Erstellen Sie einen Ereignishandler der Manager-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="93e44-195">Create a manager authorization handler</span></span>

<span data-ttu-id="93e44-196">Erstellen einer `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="93e44-196">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="93e44-197">Die `ContactManagerAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Manager.</span><span class="sxs-lookup"><span data-stu-id="93e44-197">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="93e44-198">Nur Manager können genehmigen oder Ablehnen der Änderungen am Inhalt (neue oder geänderte).</span><span class="sxs-lookup"><span data-stu-id="93e44-198">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="93e44-199">Erstellen Sie einen Administrator-Autorisierung-Ereignishandler</span><span class="sxs-lookup"><span data-stu-id="93e44-199">Create an administrator authorization handler</span></span>

<span data-ttu-id="93e44-200">Erstellen einer `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="93e44-200">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="93e44-201">Die `ContactAdministratorsAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Administrator.</span><span class="sxs-lookup"><span data-stu-id="93e44-201">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="93e44-202">Administratoren kann alle Vorgänge tun.</span><span class="sxs-lookup"><span data-stu-id="93e44-202">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="93e44-203">Registrieren Sie die Authorization-Handler</span><span class="sxs-lookup"><span data-stu-id="93e44-203">Register the authorization handlers</span></span>

<span data-ttu-id="93e44-204">Verwendung von Entity Framework Core Services müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="93e44-204">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="93e44-205">Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="93e44-205">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="93e44-206">Damit sie verfügbar sind, registrieren Sie die Handler mit die Auflistung der `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="93e44-206">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="93e44-207">Fügen Sie den folgenden Code am Ende der `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="93e44-207">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

<span data-ttu-id="93e44-208">`ContactAdministratorsAuthorizationHandler`und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="93e44-208">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="93e44-209">Sie Singletons sind, da sie nicht EF verwenden und alle erforderlichen Informationen in den `Context` Parameter von der `HandleRequirementAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="93e44-209">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="93e44-210">Support-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="93e44-210">Support authorization</span></span>

<span data-ttu-id="93e44-211">In diesem Abschnitt Aktualisieren der Razor-Seiten und fügen Sie eine Operations Anforderungen-Klasse.</span><span class="sxs-lookup"><span data-stu-id="93e44-211">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="93e44-212">Überprüfen Sie die Kontaktinformationen Vorgänge Anforderungen-Klasse</span><span class="sxs-lookup"><span data-stu-id="93e44-212">Review the contact operations requirements class</span></span>

<span data-ttu-id="93e44-213">Überprüfen Sie die `ContactOperations` Klasse.</span><span class="sxs-lookup"><span data-stu-id="93e44-213">Review the `ContactOperations` class.</span></span> <span data-ttu-id="93e44-214">Diese Klasse enthält die Anforderungen der app unterstützt:</span><span class="sxs-lookup"><span data-stu-id="93e44-214">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="93e44-215">Erstellen Sie eine Basisklasse für die Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="93e44-215">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="93e44-216">Erstellen Sie eine Basisklasse, die in den Kontakten Razor-Seiten verwendeten Dienste enthält.</span><span class="sxs-lookup"><span data-stu-id="93e44-216">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="93e44-217">Die Basisklasse setzt diese Initialisierungscode an einem zentralen Ort:</span><span class="sxs-lookup"><span data-stu-id="93e44-217">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="93e44-218">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="93e44-218">The preceding code:</span></span>

* <span data-ttu-id="93e44-219">Fügt der `IAuthorizationService` Dienst den Zugriff auf die Autorisierung Handler.</span><span class="sxs-lookup"><span data-stu-id="93e44-219">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="93e44-220">Fügt die Identität `UserManager` Dienst.</span><span class="sxs-lookup"><span data-stu-id="93e44-220">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="93e44-221">Fügen Sie die `ApplicationDbContext` hinzu.</span><span class="sxs-lookup"><span data-stu-id="93e44-221">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="93e44-222">Aktualisieren der CreateModel</span><span class="sxs-lookup"><span data-stu-id="93e44-222">Update the CreateModel</span></span>

<span data-ttu-id="93e44-223">Aktualisieren den erstellen Seite Modell Konstruktor verwendet den `DI_BasePageModel` Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="93e44-223">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="93e44-224">Update der `CreateModel.OnPostAsync` aufzurufende Methode:</span><span class="sxs-lookup"><span data-stu-id="93e44-224">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="93e44-225">Fügen Sie die Benutzer-ID an den `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="93e44-225">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="93e44-226">Rufen Sie den Handler Autorisierung, um sicherzustellen, dass der Benutzer die Berechtigung zum Erstellen von Kontakten verfügt.</span><span class="sxs-lookup"><span data-stu-id="93e44-226">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="93e44-227">Aktualisieren Sie die IndexModel</span><span class="sxs-lookup"><span data-stu-id="93e44-227">Update the IndexModel</span></span>

<span data-ttu-id="93e44-228">Update der `OnGetAsync` Methode, sodass nur genehmigte Kontakte für allgemeine Benutzer angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="93e44-228">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="93e44-229">Aktualisieren Sie die EditModel</span><span class="sxs-lookup"><span data-stu-id="93e44-229">Update the EditModel</span></span>

<span data-ttu-id="93e44-230">Fügen Sie einen Handler Autorisierung, um sicherzustellen, dass der Benutzer im Besitz des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="93e44-230">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="93e44-231">Da Ressource Autorisierung überprüft wird, die `[Authorize]` Attribut ist nicht ausreichend.</span><span class="sxs-lookup"><span data-stu-id="93e44-231">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="93e44-232">Die app hat keinen Zugriff auf die Ressource, wenn Attribute ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="93e44-232">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="93e44-233">Autorisierung ressourcenbasierte muss zwingend erforderlich.</span><span class="sxs-lookup"><span data-stu-id="93e44-233">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="93e44-234">Überprüfungen müssen ausgeführt werden, sobald die app Zugriff auf die Ressource durch Laden in das Modell oder durch Laden in den Handler selbst hat.</span><span class="sxs-lookup"><span data-stu-id="93e44-234">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="93e44-235">Durch Übergeben der Ressourcenschlüssel greifen Sie häufig die Ressource.</span><span class="sxs-lookup"><span data-stu-id="93e44-235">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="93e44-236">Aktualisieren Sie die DeleteModel</span><span class="sxs-lookup"><span data-stu-id="93e44-236">Update the DeleteModel</span></span>

<span data-ttu-id="93e44-237">Aktualisieren Sie das Modell löschen Seite, um die Autorisierung Handler zu verwenden, um sicherzustellen, dass der Benutzer über die Löschberechtigung für den Kontakt verfügt.</span><span class="sxs-lookup"><span data-stu-id="93e44-237">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="93e44-238">Fügen Sie dem Prüfdienst in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="93e44-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="93e44-239">Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Daten, die der Benutzer ändern kann.</span><span class="sxs-lookup"><span data-stu-id="93e44-239">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="93e44-240">Die Benutzeroberfläche ist durch Anwenden des Autorisierung Handlers, die den Ansichten fest.</span><span class="sxs-lookup"><span data-stu-id="93e44-240">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="93e44-241">Fügen Sie dem Prüfdienst in der *Views/_ViewImports.cshtml* Datei, damit er für alle Ansichten verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="93e44-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="93e44-242">Das vorhergehende Markup fügt mehrere `using` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="93e44-242">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="93e44-243">Update der **bearbeiten** und **löschen** verknüpft *Pages/Contacts/Index.cshtml* , sodass sie nur für Benutzer mit den entsprechenden Berechtigungen gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="93e44-243">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> <span data-ttu-id="93e44-244">Ausblenden von Links von Benutzern, die keine Berechtigung zum Ändern von Daten haben Sichern nicht die app.</span><span class="sxs-lookup"><span data-stu-id="93e44-244">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="93e44-245">Durch das Ausblenden Links wird die app durch Anzeigen der einzige gültige Links Benutzerfreundlicher.</span><span class="sxs-lookup"><span data-stu-id="93e44-245">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="93e44-246">Benutzer können die generierten URLs zum Aufrufen bearbeiten und Löschen von Operationen mit Daten, die sie besitzen keine hack.</span><span class="sxs-lookup"><span data-stu-id="93e44-246">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="93e44-247">Der Razor-Seite oder ein Domänencontroller muss zugriffsüberprüfungen zum Schutz der Daten erzwingen.</span><span class="sxs-lookup"><span data-stu-id="93e44-247">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="93e44-248">Updatedetails</span><span class="sxs-lookup"><span data-stu-id="93e44-248">Update Details</span></span>

<span data-ttu-id="93e44-249">Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:</span><span class="sxs-lookup"><span data-stu-id="93e44-249">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

<span data-ttu-id="93e44-250">Aktualisieren Sie das Modell Details:</span><span class="sxs-lookup"><span data-stu-id="93e44-250">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="93e44-251">Abgeschlossene Anwendung testen</span><span class="sxs-lookup"><span data-stu-id="93e44-251">Test the completed app</span></span>

<span data-ttu-id="93e44-252">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="93e44-252">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="93e44-253">Legen Sie `"LocalTest:skipSSL": true` in der *"appSettings". Developement.JSON* Datei, um die SSL-Anforderung zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="93e44-253">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="93e44-254">Überspringen Sie SSL nur auf einem Entwicklungscomputer.</span><span class="sxs-lookup"><span data-stu-id="93e44-254">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="93e44-255">Wenn die app Kontakte besitzt:</span><span class="sxs-lookup"><span data-stu-id="93e44-255">If the app has contacts:</span></span>

* <span data-ttu-id="93e44-256">Löschen aller Datensätze in der `Contact` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="93e44-256">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="93e44-257">Starten Sie die app Ausgangswert für die Datenbank neu.</span><span class="sxs-lookup"><span data-stu-id="93e44-257">Restart the app to seed the database.</span></span>

<span data-ttu-id="93e44-258">Registrieren Sie einen Benutzer zum Durchsuchen der Kontakte.</span><span class="sxs-lookup"><span data-stu-id="93e44-258">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="93e44-259">Eine einfache Möglichkeit zum Testen Sie die abgeschlossenen app wird auf drei verschiedenen Browsern (oder inkognito/InPrivate-Versionen) zu starten.</span><span class="sxs-lookup"><span data-stu-id="93e44-259">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="93e44-260">In einem Browser eingeben, einen neuen Benutzer registrieren (z. B. `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="93e44-260">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="93e44-261">Melden Sie sich mit einem anderen Benutzer jedem Browser an.</span><span class="sxs-lookup"><span data-stu-id="93e44-261">Sign in to each browser with a different user.</span></span> <span data-ttu-id="93e44-262">Überprüfen Sie die folgenden Vorgänge aus:</span><span class="sxs-lookup"><span data-stu-id="93e44-262">Verify the following operations:</span></span>

* <span data-ttu-id="93e44-263">Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="93e44-263">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="93e44-264">Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="93e44-264">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="93e44-265">Manager können genehmigen oder ablehnen Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="93e44-265">Managers can approve or reject contact data.</span></span> <span data-ttu-id="93e44-266">Die `Details` Ansicht zeigt **genehmigen** und **ablehnen** Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="93e44-266">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="93e44-267">Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="93e44-267">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="93e44-268">Benutzer</span><span class="sxs-lookup"><span data-stu-id="93e44-268">User</span></span>| <span data-ttu-id="93e44-269">Optionen</span><span class="sxs-lookup"><span data-stu-id="93e44-269">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="93e44-270">Können bearbeiten/eigene Daten löschen</span><span class="sxs-lookup"><span data-stu-id="93e44-270">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="93e44-271">Können ablehnen/genehmigen und bearbeiten/löschen, die Daten besitzen</span><span class="sxs-lookup"><span data-stu-id="93e44-271">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="93e44-272">Können bearbeiten/löschen und alle Daten genehmigen/ablehnen</span><span class="sxs-lookup"><span data-stu-id="93e44-272">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="93e44-273">Erstellen eines Kontakts in der Administrator-Browser.</span><span class="sxs-lookup"><span data-stu-id="93e44-273">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="93e44-274">Kopieren Sie die URL zum Löschen und Bearbeiten von den Administrator wenden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="93e44-274">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="93e44-275">Fügen Sie diese Links in den Browser des Testbenutzers zu überprüfen, ob der Testbenutzer diese Vorgänge ausführen kann nicht aus.</span><span class="sxs-lookup"><span data-stu-id="93e44-275">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="93e44-276">Erstellen der Starter-app</span><span class="sxs-lookup"><span data-stu-id="93e44-276">Create the starter app</span></span>

* <span data-ttu-id="93e44-277">Erstellen Sie eine Razor-Seiten-app mit dem Namen "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="93e44-277">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="93e44-278">Erstellen Sie die app mit **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="93e44-278">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="93e44-279">Nennen Sie sie "ContactManager", sodass Ihr Namespace den Namespace, die im Beispiel verwendeten entspricht.</span><span class="sxs-lookup"><span data-stu-id="93e44-279">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="93e44-280">`-uld`Gibt an, anstelle von SQLite LocalDB</span><span class="sxs-lookup"><span data-stu-id="93e44-280">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="93e44-281">Fügen Sie die folgenden `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="93e44-281">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="93e44-282">Gerüst der `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="93e44-282">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="93e44-283">Update der **ContactManager** Verankern der *Pages/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="93e44-283">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="93e44-284">Erstellen der anfänglichen Migrations und die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="93e44-284">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="93e44-285">Testen der app durch erstellen, bearbeiten und Löschen eines Kontakts</span><span class="sxs-lookup"><span data-stu-id="93e44-285">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="93e44-286">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="93e44-286">Seed the database</span></span>

<span data-ttu-id="93e44-287">Hinzufügen der `SeedData` Klasse, um die *Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="93e44-287">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="93e44-288">Wenn Sie das Beispiel heruntergeladen haben, können Sie kopieren die *SeedData.cs* Datei wird in der *Daten* Ordner des Startprojekts.</span><span class="sxs-lookup"><span data-stu-id="93e44-288">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="93e44-289">Rufen Sie `SeedData.Initialize` aus `Main`:</span><span class="sxs-lookup"><span data-stu-id="93e44-289">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="93e44-290">Testen Sie, ob die Anwendung die Datenbank mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="93e44-290">Test that the app seeded the database.</span></span> <span data-ttu-id="93e44-291">Wenn alle Zeilen in den Kontakt DB vorhanden sind, nicht der Seed-Methode ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="93e44-291">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="93e44-292">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="93e44-292">Additional resources</span></span>

* <span data-ttu-id="93e44-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="93e44-293">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="93e44-294">Diese Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.</span><span class="sxs-lookup"><span data-stu-id="93e44-294">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="93e44-295">Autorisierung in ASP.NET Core: einfach, anspruchsbasierte und benutzerdefinierten Rolle</span><span class="sxs-lookup"><span data-stu-id="93e44-295">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="93e44-296">Benutzerdefinierte, richtlinienbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="93e44-296">Custom policy-based authorization</span></span>](xref:security/authorization/policies)
