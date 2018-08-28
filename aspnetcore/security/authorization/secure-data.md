---
title: Erstellen einer ASP.NET Core-app mit Benutzerdaten, die durch Autorisierung geschützt sind
author: rick-anderson
description: Informationen Sie zum Erstellen einer Razor-Seiten-app mit Benutzerdaten, die durch Autorisierung geschützt sind. Enthält, HTTPS, Authentifizierung und Sicherheit, ASP.NET Core Identity.
ms.author: riande
ms.date: 7/24/2018
uid: security/authorization/secure-data
ms.openlocfilehash: ba59e8d6243965188397c4ba7a130eec42acfb91
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055879"
---
::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="94d8d-104">Finden Sie unter [dieses PDF-Dokument](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) für die ASP.NET Core MVC-Version.</span><span class="sxs-lookup"><span data-stu-id="94d8d-104">See [this PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) for the ASP.NET Core MVC version.</span></span> <span data-ttu-id="94d8d-105">Die ASP.NET Core 1.1-Version dieses Tutorials wird [dies](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) Ordner.</span><span class="sxs-lookup"><span data-stu-id="94d8d-105">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="94d8d-106">Die ASP.NET Core-Beispiel befindet sich in 1.1 die [Beispiele](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="94d8d-106">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="94d8d-107">Finden Sie unter den [Diese PDF-Datei] ()https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span><span class="sxs-lookup"><span data-stu-id="94d8d-107">See the [this pdf] (https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_July16_18.pdf)</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="94d8d-108">Erstellen einer ASP.NET Core-app mit Benutzerdaten, die durch Autorisierung geschützt sind</span><span class="sxs-lookup"><span data-stu-id="94d8d-108">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="94d8d-109">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="94d8d-109">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="94d8d-110">In diesem Tutorial veranschaulicht das Erstellen einer ASP.NET Core-Web-Apps mit Benutzerdaten, die durch Autorisierung geschützt sind.</span><span class="sxs-lookup"><span data-stu-id="94d8d-110">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="94d8d-111">Es zeigt eine Liste von Kontakten, die (registrierter) Benutzer authentifiziert haben.</span><span class="sxs-lookup"><span data-stu-id="94d8d-111">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="94d8d-112">Es gibt drei Sicherheitsgruppen:</span><span class="sxs-lookup"><span data-stu-id="94d8d-112">There are three security groups:</span></span>

* <span data-ttu-id="94d8d-113">**Registrierte Benutzer** können alle genehmigten Daten anzeigen und bearbeiten und löschen können ihre eigenen Daten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-113">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="94d8d-114">**Manager** genehmigen oder ablehnen von Kontaktdaten können.</span><span class="sxs-lookup"><span data-stu-id="94d8d-114">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="94d8d-115">Nur genehmigte Kontakte für Benutzer sichtbar sind.</span><span class="sxs-lookup"><span data-stu-id="94d8d-115">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="94d8d-116">**Administratoren** können genehmigen/ablehnen und alle Daten bearbeiten und löschen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-116">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="94d8d-117">In der folgenden Abbildung wird der Benutzer Rick (`rick@example.com`) angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="94d8d-117">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="94d8d-118">Rick kann nur genehmigte Kontakte anzeigen und **bearbeiten**/**löschen**/**neu erstellen** Links, um seine Kontakte.</span><span class="sxs-lookup"><span data-stu-id="94d8d-118">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="94d8d-119">Nur der letzte Datensatz erstellt, von Rick, zeigt **bearbeiten** und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="94d8d-119">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="94d8d-120">Andere Benutzer werden der letzte Eintrag nicht angezeigt, bis Manager oder Administratoren den Status "Genehmigt" annimmt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-120">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Abbildung beschrieben vor](secure-data/_static/rick.png)

<span data-ttu-id="94d8d-122">In der folgenden Abbildung `manager@contoso.com` ist signiert, im und in der Rolle "Managers":</span><span class="sxs-lookup"><span data-stu-id="94d8d-122">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Abbildung beschrieben vor](secure-data/_static/manager1.png)

<span data-ttu-id="94d8d-124">Die folgende Abbildung zeigt die Manager Detailansicht eines Kontakts an:</span><span class="sxs-lookup"><span data-stu-id="94d8d-124">The following image shows the managers details view of a contact:</span></span>

![Abbildung beschrieben vor](secure-data/_static/manager.png)

<span data-ttu-id="94d8d-126">Die **genehmigen** und **ablehnen** Schaltflächen sind nur für Manager und Administratoren angezeigt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-126">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="94d8d-127">In der folgenden Abbildung `admin@contoso.com` in und in der Rolle "Administratoren" angemeldet ist:</span><span class="sxs-lookup"><span data-stu-id="94d8d-127">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Abbildung beschrieben vor](secure-data/_static/admin.png)

<span data-ttu-id="94d8d-129">Der Administrator muss alle Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-129">The administrator has all privileges.</span></span> <span data-ttu-id="94d8d-130">Sie können lesen, bearbeiten und löschen Sie einen beliebigen Kontakt, und ändern Sie den Status von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-130">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="94d8d-131">Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="94d8d-131">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet)]

<span data-ttu-id="94d8d-132">Das Beispiel enthält die folgenden Handler für die Autorisierung:</span><span class="sxs-lookup"><span data-stu-id="94d8d-132">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="94d8d-133">`ContactIsOwnerAuthorizationHandler`: Stellt sicher, dass ein Benutzer nur ihre Daten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="94d8d-133">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="94d8d-134">`ContactManagerAuthorizationHandler`: Ermöglicht es Managern zum Genehmigen oder ablehnen von Kontakten aus.</span><span class="sxs-lookup"><span data-stu-id="94d8d-134">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="94d8d-135">`ContactAdministratorsAuthorizationHandler`: Ermöglicht es Administratoren, die zum Genehmigen oder ablehnen von Kontakten und zu bearbeiten und Löschen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-135">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="94d8d-136">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="94d8d-136">Prerequisites</span></span>

<span data-ttu-id="94d8d-137">In diesem Tutorial wird verschoben.</span><span class="sxs-lookup"><span data-stu-id="94d8d-137">This tutorial is advanced.</span></span> <span data-ttu-id="94d8d-138">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="94d8d-138">You should be familiar with:</span></span>

* [<span data-ttu-id="94d8d-139">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="94d8d-139">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="94d8d-140">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="94d8d-140">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="94d8d-141">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="94d8d-141">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="94d8d-142">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="94d8d-142">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="94d8d-143">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="94d8d-143">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="94d8d-144">Der Download-Code für dieses Tutorial ist ASP.NET Core-2.2-Preview 1 oder höher erforderlich.</span><span class="sxs-lookup"><span data-stu-id="94d8d-144">The download code as for this tutorial requires ASP.NET Core 2.2 preview 1 or later.</span></span> <span data-ttu-id="94d8d-145">Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) für das Problem zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-145">See [this GitHub issue](https://github.com/aspnet/Identity/issues/1813#issuecomment-394543909) for a work-around.</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="94d8d-146">Die Starter und die fertige app</span><span class="sxs-lookup"><span data-stu-id="94d8d-146">The starter and completed app</span></span>

<span data-ttu-id="94d8d-147">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="94d8d-147">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="94d8d-148">[Test](#test-the-completed-app) die fertige app, damit Sie mit den Security-Features vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-148">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="94d8d-149">Das Starter-app</span><span class="sxs-lookup"><span data-stu-id="94d8d-149">The starter app</span></span>

<span data-ttu-id="94d8d-150">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="94d8d-150">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="94d8d-151">Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und stellen Sie sicher, Sie erstellen, bearbeiten und Löschen eines Kontakts.</span><span class="sxs-lookup"><span data-stu-id="94d8d-151">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="94d8d-152">Schützen von Benutzerdaten</span><span class="sxs-lookup"><span data-stu-id="94d8d-152">Secure user data</span></span>

<span data-ttu-id="94d8d-153">Die folgenden Abschnitte enthalten die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten-app.</span><span class="sxs-lookup"><span data-stu-id="94d8d-153">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="94d8d-154">Möglicherweise finden Sie es hilfreich sein, auf das abgeschlossene Projekt zu verweisen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-154">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="94d8d-155">Verknüpfen Sie die Kontaktdaten für den Benutzer</span><span class="sxs-lookup"><span data-stu-id="94d8d-155">Tie the contact data to the user</span></span>

<span data-ttu-id="94d8d-156">Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="94d8d-156">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="94d8d-157">Hinzufügen `OwnerID` und `ContactStatus` auf die `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="94d8d-157">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

<span data-ttu-id="94d8d-158">`OwnerID` ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="94d8d-158">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="94d8d-159">Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="94d8d-159">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="94d8d-160">Erstellen Sie eine neue Migration, und aktualisieren Sie die Datenbank:</span><span class="sxs-lookup"><span data-stu-id="94d8d-160">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a><span data-ttu-id="94d8d-161">Fügen Sie Rollendienste hinzu, die Identität</span><span class="sxs-lookup"><span data-stu-id="94d8d-161">Add Role services to Identity</span></span>

<span data-ttu-id="94d8d-162">Fügen Sie [Rollen](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) Rollendienste hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="94d8d-162">Append [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) to add Role services:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a><span data-ttu-id="94d8d-163">Authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="94d8d-163">Require authenticated users</span></span>

<span data-ttu-id="94d8d-164">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen:</span><span class="sxs-lookup"><span data-stu-id="94d8d-164">Set the default authentication policy to require users to be authenticated:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 <span data-ttu-id="94d8d-165">Sie können die Authentifizierung auf der Ebene der Razor Page, Controller und Aktion mit Deaktivieren der `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="94d8d-165">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="94d8d-166">Einstellung die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen schützt das neu hinzugefügte Razor-Seiten und Controller.</span><span class="sxs-lookup"><span data-stu-id="94d8d-166">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="94d8d-167">Mit der Authentifizierung erforderlich, die in der Standardeinstellung sicherer ist als die auf neue Domänencontroller und Razor-Seiten enthalten die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="94d8d-167">Having authentication required by default is more secure than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span>

<span data-ttu-id="94d8d-168">Hinzufügen ["AllowAnonymous"](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) auf den Index, Info "und" Wenden Sie sich an Seiten, damit anonyme Benutzer Informationen über die Website nutzen können, bevor sie sich registriert haben.</span><span class="sxs-lookup"><span data-stu-id="94d8d-168">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a><span data-ttu-id="94d8d-169">Konfigurieren des Testkontos</span><span class="sxs-lookup"><span data-stu-id="94d8d-169">Configure the test account</span></span>

<span data-ttu-id="94d8d-170">Die `SeedData` Klasse erstellt zwei Konten: Administrator und Manager.</span><span class="sxs-lookup"><span data-stu-id="94d8d-170">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="94d8d-171">Verwenden der [Secret Manager-Tool](xref:security/app-secrets) , ein Kennwort für diese Konten festzulegen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-171">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="94d8d-172">Legen Sie das Kennwort aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*):</span><span class="sxs-lookup"><span data-stu-id="94d8d-172">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="94d8d-173">Wenn Sie ein sicheres Kennwort nicht angegeben ist, eine Ausnahme wird ausgelöst, wenn `SeedData.Initialize` aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="94d8d-173">If a strong password is not specified, an exception is thrown when `SeedData.Initialize` is called.</span></span>

<span data-ttu-id="94d8d-174">Update `Main` Testkennwort verwenden:</span><span class="sxs-lookup"><span data-stu-id="94d8d-174">Update `Main` to use the test password:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="94d8d-175">Erstellen Sie die Testkonten und aktualisieren Kontakte</span><span class="sxs-lookup"><span data-stu-id="94d8d-175">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="94d8d-176">Update der `Initialize` -Methode in der die `SeedData` Klasse, um die Testkonten zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="94d8d-176">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="94d8d-177">Fügen Sie die Administrator-Benutzer-ID und `ContactStatus` für die Contacts.</span><span class="sxs-lookup"><span data-stu-id="94d8d-177">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="94d8d-178">Stellen Sie einen der Kontakte für "Gesendet" und eine "abgelehnt".</span><span class="sxs-lookup"><span data-stu-id="94d8d-178">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="94d8d-179">Fügen Sie die Benutzer-ID und den Status an alle Kontakte hinzu.</span><span class="sxs-lookup"><span data-stu-id="94d8d-179">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="94d8d-180">Nur ein Kontakt wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="94d8d-180">Only one contact is shown:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="94d8d-181">Besitzer, Manager und Administratoren Autorisierung Handler zu erstellen</span><span class="sxs-lookup"><span data-stu-id="94d8d-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="94d8d-182">Erstellen Sie eine `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="94d8d-182">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="94d8d-183">Die `ContactIsOwnerAuthorizationHandler` stellt sicher, dass der Benutzer, die auf eine Ressource, das die Ressource besitzt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-183">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="94d8d-184">Die `ContactIsOwnerAuthorizationHandler` Aufrufe [Kontext. Erfolgreiche](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts.</span><span class="sxs-lookup"><span data-stu-id="94d8d-184">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="94d8d-185">Autorisierung Handler in der Regel:</span><span class="sxs-lookup"><span data-stu-id="94d8d-185">Authorization handlers generally:</span></span>

* <span data-ttu-id="94d8d-186">Zurückgeben `context.Succeed` Wenn die Anforderungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="94d8d-186">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="94d8d-187">Zurückgeben `Task.CompletedTask` Wenn sind nicht erfüllt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-187">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="94d8d-188">`Task.CompletedTask` ist weder Erfolg oder Misserfolg&mdash;dadurch, dass andere Handler Autorisierung ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-188">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="94d8d-189">Wenn Sie explizit ausführen müssen, zurückgeben [Kontext. Fehler](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="94d8d-189">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="94d8d-190">Der app können Besitzer von Kontaktinformationen bearbeiten/löschen/erstellen ihre eigenen Daten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-190">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="94d8d-191">`ContactIsOwnerAuthorizationHandler` Überprüfen Sie den Vorgang, der im Parameter "Anforderung" übergeben muss nicht.</span><span class="sxs-lookup"><span data-stu-id="94d8d-191">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="94d8d-192">Erstellen Sie einen Ereignishandler der Manager-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="94d8d-192">Create a manager authorization handler</span></span>

<span data-ttu-id="94d8d-193">Erstellen Sie eine `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="94d8d-193">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="94d8d-194">Die `ContactManagerAuthorizationHandler` überprüft, ob sich der Benutzer, die auf die Ressource eines Managers.</span><span class="sxs-lookup"><span data-stu-id="94d8d-194">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="94d8d-195">Nur Manager können genehmigen oder ablehnen von Änderungen am Inhalt (neue oder geänderte).</span><span class="sxs-lookup"><span data-stu-id="94d8d-195">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="94d8d-196">Erstellen Sie ein Administrator autorisierungshandler</span><span class="sxs-lookup"><span data-stu-id="94d8d-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="94d8d-197">Erstellen Sie eine `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="94d8d-197">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="94d8d-198">Die `ContactAdministratorsAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Administrator.</span><span class="sxs-lookup"><span data-stu-id="94d8d-198">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="94d8d-199">Administrator möglich, alle Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="94d8d-199">Administrator can do all operations.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="94d8d-200">Registrieren Sie die Autorisierung-Handler</span><span class="sxs-lookup"><span data-stu-id="94d8d-200">Register the authorization handlers</span></span>

<span data-ttu-id="94d8d-201">Dienste, die mit Entity Framework Core müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="94d8d-201">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="94d8d-202">Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="94d8d-202">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="94d8d-203">Registrieren Sie die Handler mit der Sammlung von Diensten, damit sie verfügbar sind die `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="94d8d-203">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="94d8d-204">Fügen Sie den folgenden Code am Ende der `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="94d8d-204">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

<span data-ttu-id="94d8d-205">`ContactAdministratorsAuthorizationHandler` und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-205">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="94d8d-206">Sie Singletons sind, da sie nicht die EF verwenden und alle erforderlichen Informationen in den `Context` Parameter, der die `HandleRequirementAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="94d8d-206">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="94d8d-207">Unterstützung für Autorisierung</span><span class="sxs-lookup"><span data-stu-id="94d8d-207">Support authorization</span></span>

<span data-ttu-id="94d8d-208">In diesem Abschnitt Aktualisieren Sie die Razor-Seiten und ein Operations-Anforderungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-208">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="94d8d-209">Überprüfen Sie die Kontaktinformationen Vorgänge Anforderungen-Klasse</span><span class="sxs-lookup"><span data-stu-id="94d8d-209">Review the contact operations requirements class</span></span>

<span data-ttu-id="94d8d-210">Überprüfen Sie die `ContactOperations` Klasse.</span><span class="sxs-lookup"><span data-stu-id="94d8d-210">Review the `ContactOperations` class.</span></span> <span data-ttu-id="94d8d-211">Diese Klasse enthält die Anforderungen der app unterstützt:</span><span class="sxs-lookup"><span data-stu-id="94d8d-211">This class contains the requirements the app supports:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a><span data-ttu-id="94d8d-212">Erstellen Sie eine Basisklasse für die Kontakte-Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="94d8d-212">Create a base class for the Contacts Razor Pages</span></span>

<span data-ttu-id="94d8d-213">Erstellen Sie eine Basisklasse, die die in den Kontakten Razor-Seiten verwendeten Dienste enthält.</span><span class="sxs-lookup"><span data-stu-id="94d8d-213">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="94d8d-214">Die Basisklasse der Klasse wird den Initialisierungscode an einem Ort:</span><span class="sxs-lookup"><span data-stu-id="94d8d-214">The base class puts the initialization code in one location:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="94d8d-215">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="94d8d-215">The preceding code:</span></span>

* <span data-ttu-id="94d8d-216">Fügt der `IAuthorizationService` Dienst den Zugriff auf die Handler für die Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="94d8d-216">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="94d8d-217">Fügt die Identität `UserManager` Service.</span><span class="sxs-lookup"><span data-stu-id="94d8d-217">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="94d8d-218">Fügen Sie die `ApplicationDbContext` hinzu.</span><span class="sxs-lookup"><span data-stu-id="94d8d-218">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="94d8d-219">Aktualisieren der CreateModel</span><span class="sxs-lookup"><span data-stu-id="94d8d-219">Update the CreateModel</span></span>

<span data-ttu-id="94d8d-220">Aktualisieren den erstellen-Page-Modell-Konstruktor verwendet die `DI_BasePageModel` Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="94d8d-220">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="94d8d-221">Update der `CreateModel.OnPostAsync` Methode, um:</span><span class="sxs-lookup"><span data-stu-id="94d8d-221">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="94d8d-222">Hinzufügen der Benutzer-ID an den `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="94d8d-222">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="94d8d-223">Rufen Sie die autorisierungshandler, um sicherzustellen, dass der Benutzer die Berechtigung zum Erstellen von Kontakten verfügt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-223">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="94d8d-224">Aktualisieren Sie die IndexModel</span><span class="sxs-lookup"><span data-stu-id="94d8d-224">Update the IndexModel</span></span>

<span data-ttu-id="94d8d-225">Update der `OnGetAsync` Methode, sodass nur genehmigte Kontakte für allgemeine Benutzer angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="94d8d-225">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="94d8d-226">Aktualisieren Sie die EditModel</span><span class="sxs-lookup"><span data-stu-id="94d8d-226">Update the EditModel</span></span>

<span data-ttu-id="94d8d-227">Fügen Sie einem autorisierungshandler, um sicherzustellen, dass der Benutzer ist Besitzer der den Kontakt hinzu.</span><span class="sxs-lookup"><span data-stu-id="94d8d-227">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="94d8d-228">Da die Ressource Autorisierung überprüft wird, die `[Authorize]` -Attribut ist nicht ausreichend.</span><span class="sxs-lookup"><span data-stu-id="94d8d-228">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="94d8d-229">Die app hat keinen Zugriff auf die Ressource, wenn die Attribute ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="94d8d-229">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="94d8d-230">Ressourcenbasierte Autorisierung muss zwingend erforderlich.</span><span class="sxs-lookup"><span data-stu-id="94d8d-230">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="94d8d-231">Überprüfungen müssen ausgeführt werden, sobald die app Zugriff auf die Ressource, oder durch Laden in das Modell als auch innerhalb des Handlers selbst laden.</span><span class="sxs-lookup"><span data-stu-id="94d8d-231">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="94d8d-232">Sie werden häufig die Ressource durch die Übergabe der Ressourcenschlüssel zugreifen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-232">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="94d8d-233">Aktualisieren Sie die DeleteModel</span><span class="sxs-lookup"><span data-stu-id="94d8d-233">Update the DeleteModel</span></span>

<span data-ttu-id="94d8d-234">Aktualisieren Sie das Seitenmodell "löschen" um den autorisierungshandler für die zu verwenden, um sicherzustellen, dass der Benutzer über die Löschberechtigung für den Kontakt verfügt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-234">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="94d8d-235">Fügen Sie dem Prüfdienst in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="94d8d-235">Inject the authorization service into the views</span></span>

<span data-ttu-id="94d8d-236">Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Kontakte, die der Benutzer nicht ändern kann.</span><span class="sxs-lookup"><span data-stu-id="94d8d-236">Currently, the UI shows edit and delete links for contacts the user can't modify.</span></span>

<span data-ttu-id="94d8d-237">Einfügen des autorisierungsdiensts in die *Views/_viewimports.cshtml* Datei, sodass sie für alle Ansichten verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="94d8d-237">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

<span data-ttu-id="94d8d-238">Das vorhergehende Markup fügt mehrere `using` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-238">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="94d8d-239">Update der **bearbeiten** und **löschen** verknüpft *Pages/Contacts/Index.cshtml* , sodass sie nur für Benutzer mit den entsprechenden Berechtigungen gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="94d8d-239">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> <span data-ttu-id="94d8d-240">Ausblenden von Links von Benutzern, die keine Berechtigung zum Ändern von Daten haben nicht die app zu sichern.</span><span class="sxs-lookup"><span data-stu-id="94d8d-240">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="94d8d-241">Ausblenden von Links wird die app durch Anzeigen der einzige gültige Links Benutzerfreundlicher.</span><span class="sxs-lookup"><span data-stu-id="94d8d-241">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="94d8d-242">Benutzer können die generierten URLs zum Aufrufen von bearbeiten und Löschen von Vorgänge für Daten, die sie besitzen keine hack.</span><span class="sxs-lookup"><span data-stu-id="94d8d-242">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="94d8d-243">Der Razor Page oder einen Controller muss zugriffsüberprüfungen zum Schutz der Daten erzwingen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-243">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="94d8d-244">Updatedetails</span><span class="sxs-lookup"><span data-stu-id="94d8d-244">Update Details</span></span>

<span data-ttu-id="94d8d-245">Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:</span><span class="sxs-lookup"><span data-stu-id="94d8d-245">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

<span data-ttu-id="94d8d-246">Aktualisieren Sie das Seitenmodell Details an:</span><span class="sxs-lookup"><span data-stu-id="94d8d-246">Update the details page model:</span></span>

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="94d8d-247">Testen Sie die fertige app</span><span class="sxs-lookup"><span data-stu-id="94d8d-247">Test the completed app</span></span>

<span data-ttu-id="94d8d-248">Wenn die app Kontakte hat:</span><span class="sxs-lookup"><span data-stu-id="94d8d-248">If the app has contacts:</span></span>

* <span data-ttu-id="94d8d-249">Löschen Sie alle Datensätze in der `Contact` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="94d8d-249">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="94d8d-250">Starten Sie die app zum Seeding der Datenbank neu.</span><span class="sxs-lookup"><span data-stu-id="94d8d-250">Restart the app to seed the database.</span></span>

<span data-ttu-id="94d8d-251">Registrieren Sie einen Benutzer, für das Durchsuchen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-251">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="94d8d-252">Eine einfache Möglichkeit zum Testen der fertigen app wird auf drei verschiedene Browser (oder inkognito/InPrivate-Versionen) zu starten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-252">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="94d8d-253">Registrieren Sie einen neuen Benutzer in einem Browser (z. B. `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="94d8d-253">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="94d8d-254">Melden Sie sich an jeden Browser mit einem anderen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="94d8d-254">Sign in to each browser with a different user.</span></span> <span data-ttu-id="94d8d-255">Überprüfen Sie die folgenden Vorgänge aus:</span><span class="sxs-lookup"><span data-stu-id="94d8d-255">Verify the following operations:</span></span>

* <span data-ttu-id="94d8d-256">Registrierte Benutzer können die genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-256">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="94d8d-257">Registrierte Benutzer können bearbeiten und ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-257">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="94d8d-258">Manager können genehmigen oder ablehnen von Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="94d8d-258">Managers can approve or reject contact data.</span></span> <span data-ttu-id="94d8d-259">Die `Details` anzeigen zeigt **genehmigen** und **ablehnen** Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-259">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="94d8d-260">Administratoren können genehmigen/ablehnen und alle Daten bearbeiten und löschen.</span><span class="sxs-lookup"><span data-stu-id="94d8d-260">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="94d8d-261">Benutzer</span><span class="sxs-lookup"><span data-stu-id="94d8d-261">User</span></span>| <span data-ttu-id="94d8d-262">Optionen</span><span class="sxs-lookup"><span data-stu-id="94d8d-262">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="94d8d-263">Kann bearbeiten oder eigene Daten löschen</span><span class="sxs-lookup"><span data-stu-id="94d8d-263">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="94d8d-264">Können genehmigen/ablehnen "und" Bearbeiten und löschen, die Daten besitzen</span><span class="sxs-lookup"><span data-stu-id="94d8d-264">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="94d8d-265">Kann bearbeiten und löschen und alle Daten genehmigen/ablehnen</span><span class="sxs-lookup"><span data-stu-id="94d8d-265">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="94d8d-266">Erstellen Sie einen Kontakt in der Administrator-Browser.</span><span class="sxs-lookup"><span data-stu-id="94d8d-266">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="94d8d-267">Kopieren Sie die URL für das Löschen und Bearbeiten von den Administrator wenden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="94d8d-267">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="94d8d-268">Fügen Sie diesen Links, in den Browser des Testbenutzers zu überprüfen, ob die Testbenutzers diese Vorgänge kann nicht ein.</span><span class="sxs-lookup"><span data-stu-id="94d8d-268">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="94d8d-269">Die Starter-app erstellen</span><span class="sxs-lookup"><span data-stu-id="94d8d-269">Create the starter app</span></span>

* <span data-ttu-id="94d8d-270">Erstellen einer Razor Pages-app, die mit dem Namen "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="94d8d-270">Create a Razor Pages app named "ContactManager"</span></span>
   * <span data-ttu-id="94d8d-271">Erstellen Sie die app mit **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="94d8d-271">Create the app with **Individual User Accounts**.</span></span>
   * <span data-ttu-id="94d8d-272">Nennen Sie ihn "ContactManager", sodass der Namespace den im Beispiel verwendeten Namespace entspricht.</span><span class="sxs-lookup"><span data-stu-id="94d8d-272">Name it "ContactManager" so the namespace matches the namespace used in the sample.</span></span>
   * <span data-ttu-id="94d8d-273">`-uld` Gibt an, LocalDB anstelle von SQLite</span><span class="sxs-lookup"><span data-stu-id="94d8d-273">`-uld` specifies LocalDB instead of SQLite</span></span>

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* <span data-ttu-id="94d8d-274">Hinzufügen *Models\Contact.cs*:</span><span class="sxs-lookup"><span data-stu-id="94d8d-274">Add *Models\Contact.cs*:</span></span>

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="94d8d-275">Gerüst der `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="94d8d-275">Scaffold the `Contact` model.</span></span>
* <span data-ttu-id="94d8d-276">Erstellen Sie der anfänglichen Migration und aktualisieren Sie die Datenbank zu:</span><span class="sxs-lookup"><span data-stu-id="94d8d-276">Create initial migration and update the database:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="94d8d-277">Update der **ContactManager** verankert werden der *Pages/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="94d8d-277">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="94d8d-278">Testen der app, indem erstellen, bearbeiten und Löschen eines Kontakts</span><span class="sxs-lookup"><span data-stu-id="94d8d-278">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="94d8d-279">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="94d8d-279">Seed the database</span></span>

<span data-ttu-id="94d8d-280">Hinzufügen der [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) -Klasse auf die *Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="94d8d-280">Add the [SeedData](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) class to the *Data* folder.</span></span>

<span data-ttu-id="94d8d-281">Rufen Sie `SeedData.Initialize` aus `Main`:</span><span class="sxs-lookup"><span data-stu-id="94d8d-281">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

<span data-ttu-id="94d8d-282">Testen Sie, dass die app die Datenbank mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-282">Test that the app seeded the database.</span></span> <span data-ttu-id="94d8d-283">Wenn alle Zeilen in der DB-Kontakt vorhanden sind, wird die Seed-Methode nicht ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-283">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="94d8d-284">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="94d8d-284">Additional resources</span></span>

* <span data-ttu-id="94d8d-285">[ASP.NET Core-Autorisierung Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="94d8d-285">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="94d8d-286">Dieser Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.</span><span class="sxs-lookup"><span data-stu-id="94d8d-286">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="94d8d-287">Autorisierung in ASP.NET Core: einfach, Rollen-, Claims-basierte und benutzerdefinierte</span><span class="sxs-lookup"><span data-stu-id="94d8d-287">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="94d8d-288">Benutzerdefinierte, richtlinienbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="94d8d-288">Custom policy-based authorization</span></span>](xref:security/authorization/policies)

::: moniker-end