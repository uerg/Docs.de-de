---
title: "Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt"
description: "Informationen Sie zum Erstellen einer Razor-Seiten-app mit Benutzerdaten durch Autorisierung geschützt. Enthält SSL, Authentifizierung und Sicherheit, ASP.NET Core Identity."
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 01/24/2018
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: ff5c97feca58318d5f4e5b6a6a930c92469602ba
ms.sourcegitcommit: 18ff1fdaa3e1ae204ed6a2ba9351ce8cf1371c85
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="07042-104">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="07042-104">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="07042-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="07042-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="07042-106">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer ASP.NET Core-Web-app mit Benutzerdaten durch Autorisierung geschützt wird.</span><span class="sxs-lookup"><span data-stu-id="07042-106">This tutorial shows how to create an ASP.NET Core web app with user data protected by authorization.</span></span> <span data-ttu-id="07042-107">Es zeigt eine Liste von Kontakten, die authentifizierte Benutzer (registrierten) erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="07042-107">It displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="07042-108">Es gibt drei Sicherheitsgruppen:</span><span class="sxs-lookup"><span data-stu-id="07042-108">There are three security groups:</span></span>

* <span data-ttu-id="07042-109">**Registrierte Benutzer** alle genehmigten Daten anzeigen und bearbeiten/löschen können ihre eigenen Daten.</span><span class="sxs-lookup"><span data-stu-id="07042-109">**Registered users** can view all the approved data and can edit/delete their own data.</span></span>
* <span data-ttu-id="07042-110">**Manager** genehmigen oder ablehnen von Kontaktdaten können.</span><span class="sxs-lookup"><span data-stu-id="07042-110">**Managers** can approve or reject contact data.</span></span> <span data-ttu-id="07042-111">Nur genehmigte Kontakte werden für Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="07042-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="07042-112">**Administratoren** können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="07042-112">**Administrators** can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="07042-113">In der folgenden Abbildung, Benutzer Rick (`rick@example.com`) angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="07042-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="07042-114">Rick kann nur genehmigte Kontakte anzeigen und **bearbeiten**/**löschen**/**neu erstellen** Links, um seine Kontakte.</span><span class="sxs-lookup"><span data-stu-id="07042-114">Rick can only view approved contacts and **Edit**/**Delete**/**Create New** links for his contacts.</span></span> <span data-ttu-id="07042-115">Nur der letzte Datensatz erstellt, von Rick zeigt **bearbeiten** und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="07042-115">Only the last record, created by Rick, displays **Edit** and **Delete** links.</span></span> <span data-ttu-id="07042-116">Andere Benutzer sehen den letzten Datensatz, erst, wenn ein Manager oder der Administrator ändert sich der Status "Genehmigt".</span><span class="sxs-lookup"><span data-stu-id="07042-116">Other users won't see the last record until a manager or administrator changes the status to "Approved".</span></span>

![Bild beschrieben vor](secure-data/_static/rick.png)

<span data-ttu-id="07042-118">In der folgenden Abbildung `manager@contoso.com` in und in der Rolle "Manager" signiert ist:</span><span class="sxs-lookup"><span data-stu-id="07042-118">In the following image, `manager@contoso.com` is signed in and in the managers role:</span></span>

![Bild beschrieben vor](secure-data/_static/manager1.png)

<span data-ttu-id="07042-120">Die folgende Abbildung zeigt den Managern Detailansicht eines Kontakts an:</span><span class="sxs-lookup"><span data-stu-id="07042-120">The following image shows the managers details view of a contact:</span></span>

![Bild beschrieben vor](secure-data/_static/manager.png)

<span data-ttu-id="07042-122">Die **genehmigen** und **ablehnen** Schaltflächen sind nur für Manager und Administratoren angezeigt.</span><span class="sxs-lookup"><span data-stu-id="07042-122">The **Approve** and **Reject** buttons are only displayed for managers and administrators.</span></span>

<span data-ttu-id="07042-123">In der folgenden Abbildung `admin@contoso.com` in und in der "Administratoren" angemeldet ist:</span><span class="sxs-lookup"><span data-stu-id="07042-123">In the following image, `admin@contoso.com` is signed in and in the administrators role:</span></span>

![Bild beschrieben vor](secure-data/_static/admin.png)

<span data-ttu-id="07042-125">Der Administrator verfügt über alle Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="07042-125">The administrator has all privileges.</span></span> <span data-ttu-id="07042-126">Sie können eine beliebige kontaktieren, Lese-/bearbeiten/löschen und ändern Sie den Status von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="07042-126">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="07042-127">Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="07042-127">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) the following `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

<span data-ttu-id="07042-128">Das Beispiel enthält die folgenden Autorisierung Handler:</span><span class="sxs-lookup"><span data-stu-id="07042-128">The sample contains the following authorization handlers:</span></span>

* <span data-ttu-id="07042-129">`ContactIsOwnerAuthorizationHandler`: Stellt sicher, dass Benutzer nur ihre Daten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="07042-129">`ContactIsOwnerAuthorizationHandler`: Ensures that a user can only edit their data.</span></span>
* <span data-ttu-id="07042-130">`ContactManagerAuthorizationHandler`: Ermöglicht es Managern zum Genehmigen oder ablehnen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="07042-130">`ContactManagerAuthorizationHandler`: Allows managers to approve or reject contacts.</span></span>
* <span data-ttu-id="07042-131">`ContactAdministratorsAuthorizationHandler`: Ermöglicht Administratoren zum Genehmigen oder ablehnen von Kontakten und Bearbeiten/Löschen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="07042-131">`ContactAdministratorsAuthorizationHandler`: Allows administrators to approve or reject contacts and to edit/delete contacts.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="07042-132">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="07042-132">Prerequisites</span></span>

<span data-ttu-id="07042-133">In diesem Lernprogramm wird erweitert.</span><span class="sxs-lookup"><span data-stu-id="07042-133">This tutorial is advanced.</span></span> <span data-ttu-id="07042-134">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="07042-134">You should be familiar with:</span></span>

* [<span data-ttu-id="07042-135">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="07042-135">ASP.NET Core</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="07042-136">Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="07042-136">Authentication</span></span>](xref:security/authentication/index)
* [<span data-ttu-id="07042-137">Kontobestätigung und Kennwortwiederherstellung</span><span class="sxs-lookup"><span data-stu-id="07042-137">Account Confirmation and Password Recovery</span></span>](xref:security/authentication/accconfirm)
* [<span data-ttu-id="07042-138">Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07042-138">Authorization</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="07042-139">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="07042-139">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

<span data-ttu-id="07042-140">Die Version 1.1 von ASP.NET Core dieses Lernprogramm ist [dies](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) Ordner.</span><span class="sxs-lookup"><span data-stu-id="07042-140">The ASP.NET Core 1.1 version of this tutorial is in [this](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) folder.</span></span> <span data-ttu-id="07042-141">Die 1.1 ASP.NET Core-Beispiel in ist der [Beispiele](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span><span class="sxs-lookup"><span data-stu-id="07042-141">The 1.1 ASP.NET Core sample is in the [samples](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).</span></span>

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="07042-142">Die Starter und abgeschlossene Anwendung</span><span class="sxs-lookup"><span data-stu-id="07042-142">The starter and completed app</span></span>

<span data-ttu-id="07042-143">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span><span class="sxs-lookup"><span data-stu-id="07042-143">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) app.</span></span> <span data-ttu-id="07042-144">[Test](#test-the-completed-app) abgeschlossene Anwendung, damit Sie mit der Sicherheitsfunktionen vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="07042-144">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span>

### <a name="the-starter-app"></a><span data-ttu-id="07042-145">Die Starter-app</span><span class="sxs-lookup"><span data-stu-id="07042-145">The starter app</span></span>

<span data-ttu-id="07042-146">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span><span class="sxs-lookup"><span data-stu-id="07042-146">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) app.</span></span>

<span data-ttu-id="07042-147">Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und überprüfen Sie Sie erstellen, bearbeiten und Löschen eines Kontakts.</span><span class="sxs-lookup"><span data-stu-id="07042-147">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

## <a name="secure-user-data"></a><span data-ttu-id="07042-148">Sichern von Benutzerdaten</span><span class="sxs-lookup"><span data-stu-id="07042-148">Secure user data</span></span>

<span data-ttu-id="07042-149">In den folgenden Abschnitten sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app.</span><span class="sxs-lookup"><span data-stu-id="07042-149">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="07042-150">Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="07042-150">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="07042-151">Binden Sie die Kontaktdaten für den Benutzer</span><span class="sxs-lookup"><span data-stu-id="07042-151">Tie the contact data to the user</span></span>

<span data-ttu-id="07042-152">Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="07042-152">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="07042-153">Hinzufügen `OwnerID` und `ContactStatus` auf die `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="07042-153">Add `OwnerID` and `ContactStatus` to the `Contact` model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

<span data-ttu-id="07042-154">`OwnerID`ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="07042-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="07042-155">Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="07042-155">The `Status` field determines if a contact is viewable by general users.</span></span>

<span data-ttu-id="07042-156">Erstellen Sie eine neue Migration und Aktualisierung der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="07042-156">Create a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="07042-157">Erfordern von SSL und authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="07042-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="07042-158">Hinzufügen [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) auf `Startup`:</span><span class="sxs-lookup"><span data-stu-id="07042-158">Add [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) to `Startup`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_env)]

<span data-ttu-id="07042-159">In der `ConfigureServices` Methode der *Startup.cs* hinzufügen. die [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) Autorisierungsfilter:</span><span class="sxs-lookup"><span data-stu-id="07042-159">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) authorization filter:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=19-)]

<span data-ttu-id="07042-160">Wenn Sie Visual Studio verwenden, aktivieren Sie SSL.</span><span class="sxs-lookup"><span data-stu-id="07042-160">If you're using Visual Studio, enable SSL.</span></span>

<span data-ttu-id="07042-161">Zum Umleiten von HTTP-Anforderungen zu HTTPS finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="07042-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="07042-162">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="07042-162">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

  <span data-ttu-id="07042-163">Legen Sie `"LocalTest:skipSSL": true` in der *"appSettings". Developement.JSON* Datei.</span><span class="sxs-lookup"><span data-stu-id="07042-163">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="07042-164">Erfordern Sie authentifizierte Benutzern</span><span class="sxs-lookup"><span data-stu-id="07042-164">Require authenticated users</span></span>

<span data-ttu-id="07042-165">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="07042-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="07042-166">Sie können die Authentifizierung auf Methodenebene Razor-Seite, Controller oder Aktionen mit abwählen der `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="07042-166">You can opt out of authentication at the Razor Page, controller, or action method level with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="07042-167">Festlegen der Standardrichtlinie für die Authentifizierung Benutzer authentifiziert werden müssen schützt neu hinzugefügten Razor-Seiten und Controllern aus.</span><span class="sxs-lookup"><span data-stu-id="07042-167">Setting the default authentication policy to require users to be authenticated protects newly added Razor Pages and controllers.</span></span> <span data-ttu-id="07042-168">Mit Authentifizierung erforderlich, in der Standardeinstellung sicherer ist als der vertrauenden Seite auf das neue Domänencontroller und Razor-Seiten enthalten die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="07042-168">Having authentication required by default is safer than relying on new controllers and Razor Pages to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="07042-169">Fügen Sie Folgendes an der `ConfigureServices` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="07042-169">Add the following to the `ConfigureServices` method of the *Startup.cs* file:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=31-)]

<span data-ttu-id="07042-170">Hinzufügen [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) auf den Index zu, und wenden Sie sich an Seiten, sodass anonyme Benutzer Informationen über die Website abrufen können, bevor sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="07042-170">Add [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) to the Index, About, and Contact pages so anonymous users can get information about the site before they register.</span></span> 

[!code-csharp[Main](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

<span data-ttu-id="07042-171">Hinzufügen `[AllowAnonymous]` auf die [LoginModel und RegisterModel](https://github.com/aspnet/templating/issues/238).</span><span class="sxs-lookup"><span data-stu-id="07042-171">Add `[AllowAnonymous]` to the [LoginModel and RegisterModel](https://github.com/aspnet/templating/issues/238).</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="07042-172">Konfigurieren Sie das Testkonto an</span><span class="sxs-lookup"><span data-stu-id="07042-172">Configure the test account</span></span>

<span data-ttu-id="07042-173">Die `SeedData` Klasse erstellt zwei Konten: Administrator und den Manager.</span><span class="sxs-lookup"><span data-stu-id="07042-173">The `SeedData` class creates two accounts: administrator and manager.</span></span> <span data-ttu-id="07042-174">Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Festlegen eines Kennworts für diese Konten.</span><span class="sxs-lookup"><span data-stu-id="07042-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="07042-175">Legen Sie das Kennwort aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*):</span><span class="sxs-lookup"><span data-stu-id="07042-175">Set the password from the project directory (the directory containing *Program.cs*):</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="07042-176">Update `Main` Testkennwort verwenden:</span><span class="sxs-lookup"><span data-stu-id="07042-176">Update `Main` to use the test password:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a><span data-ttu-id="07042-177">Erstellen Sie die Testkonten und aktualisieren die Kontakte</span><span class="sxs-lookup"><span data-stu-id="07042-177">Create the test accounts and update the contacts</span></span>

<span data-ttu-id="07042-178">Update der `Initialize` Methode in der `SeedData` Klasse, um die Testkonten zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="07042-178">Update the `Initialize` method in the `SeedData` class to create the test accounts:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

<span data-ttu-id="07042-179">Fügen Sie die Administrator-Benutzer-ID und `ContactStatus` für Kontakte.</span><span class="sxs-lookup"><span data-stu-id="07042-179">Add the administrator user ID and `ContactStatus` to the contacts.</span></span> <span data-ttu-id="07042-180">Stellen Sie einen der Kontakte "Übermittelt" und eine "abgelehnt".</span><span class="sxs-lookup"><span data-stu-id="07042-180">Make one of the contacts "Submitted" and one "Rejected".</span></span> <span data-ttu-id="07042-181">Fügen Sie die Benutzer-ID und den Status für alle Kontakte hinzu.</span><span class="sxs-lookup"><span data-stu-id="07042-181">Add the user ID and status to all the contacts.</span></span> <span data-ttu-id="07042-182">Nur ein Kontakt wird angezeigt:</span><span class="sxs-lookup"><span data-stu-id="07042-182">Only one contact is shown:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="07042-183">Erstellen Sie, Besitzer, Manager und Administrator Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="07042-183">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="07042-184">Erstellen einer `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="07042-184">Create a `ContactIsOwnerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="07042-185">Die `ContactIsOwnerAuthorizationHandler` stellt sicher, dass der Benutzer, die auf einer Ressource fungiert die Ressource besitzt.</span><span class="sxs-lookup"><span data-stu-id="07042-185">The `ContactIsOwnerAuthorizationHandler` verifies that the user acting on a resource owns the resource.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

<span data-ttu-id="07042-186">Die `ContactIsOwnerAuthorizationHandler` Aufrufe [Kontext. Erfolgreich](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts.</span><span class="sxs-lookup"><span data-stu-id="07042-186">The `ContactIsOwnerAuthorizationHandler` calls [context.Succeed](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="07042-187">Autorisierung Handler in der Regel:</span><span class="sxs-lookup"><span data-stu-id="07042-187">Authorization handlers generally:</span></span>

* <span data-ttu-id="07042-188">Zurückgeben `context.Succeed` Wenn die Anforderungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="07042-188">Return `context.Succeed` when the requirements are met.</span></span>
* <span data-ttu-id="07042-189">Zurückgeben `Task.CompletedTask` bei ist nicht erfüllt.</span><span class="sxs-lookup"><span data-stu-id="07042-189">Return `Task.CompletedTask` when requirements aren't met.</span></span> <span data-ttu-id="07042-190">`Task.CompletedTask`ist weder Erfolg oder Misserfolg&mdash;können andere Autorisierung Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="07042-190">`Task.CompletedTask` is neither success or failure&mdash;it allows other authorization handlers to run.</span></span>

<span data-ttu-id="07042-191">Wenn Sie explizit fehlschlägt müssen, zurückgeben [Kontext. Fehlschlagen](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span><span class="sxs-lookup"><span data-stu-id="07042-191">If you need to explicitly fail, return [context.Fail](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).</span></span>

<span data-ttu-id="07042-192">Die app ermöglicht Kontakt Besitzer zu bearbeiten, löschen, erstellen ihre eigenen Daten an.</span><span class="sxs-lookup"><span data-stu-id="07042-192">The app allows contact owners to edit/delete/create their own data.</span></span> <span data-ttu-id="07042-193">`ContactIsOwnerAuthorizationHandler`Überprüfen Sie den Vorgang im Parameters Anforderung übergeben muss nicht.</span><span class="sxs-lookup"><span data-stu-id="07042-193">`ContactIsOwnerAuthorizationHandler` doesn't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="07042-194">Erstellen Sie einen Ereignishandler der Manager-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07042-194">Create a manager authorization handler</span></span>

<span data-ttu-id="07042-195">Erstellen einer `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="07042-195">Create a `ContactManagerAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="07042-196">Die `ContactManagerAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Manager.</span><span class="sxs-lookup"><span data-stu-id="07042-196">The `ContactManagerAuthorizationHandler` verifies the user acting on the resource is a manager.</span></span> <span data-ttu-id="07042-197">Nur Manager können genehmigen oder Ablehnen der Änderungen am Inhalt (neue oder geänderte).</span><span class="sxs-lookup"><span data-stu-id="07042-197">Only managers can approve or reject content changes (new or changed).</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="07042-198">Erstellen Sie einen Administrator-Autorisierung-Ereignishandler</span><span class="sxs-lookup"><span data-stu-id="07042-198">Create an administrator authorization handler</span></span>

<span data-ttu-id="07042-199">Erstellen einer `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="07042-199">Create a `ContactAdministratorsAuthorizationHandler` class in the *Authorization* folder.</span></span> <span data-ttu-id="07042-200">Die `ContactAdministratorsAuthorizationHandler` überprüft, ob der Benutzer, die auf die Ressource ist ein Administrator.</span><span class="sxs-lookup"><span data-stu-id="07042-200">The `ContactAdministratorsAuthorizationHandler` verifies the user acting on the resource is an administrator.</span></span> <span data-ttu-id="07042-201">Administratoren kann alle Vorgänge tun.</span><span class="sxs-lookup"><span data-stu-id="07042-201">Administrator can do all operations.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="07042-202">Registrieren Sie die Authorization-Handler</span><span class="sxs-lookup"><span data-stu-id="07042-202">Register the authorization handlers</span></span>

<span data-ttu-id="07042-203">Verwendung von Entity Framework Core Services müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span><span class="sxs-lookup"><span data-stu-id="07042-203">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions).</span></span> <span data-ttu-id="07042-204">Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="07042-204">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="07042-205">Damit sie verfügbar sind, registrieren Sie die Handler mit die Auflistung der `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="07042-205">Register the handlers with the service collection so they're available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="07042-206">Fügen Sie den folgenden Code am Ende der `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="07042-206">Add the following code to the end of `ConfigureServices`:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-)]

<span data-ttu-id="07042-207">`ContactAdministratorsAuthorizationHandler`und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="07042-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="07042-208">Sie Singletons sind, da sie nicht EF verwenden und alle erforderlichen Informationen in den `Context` Parameter von der `HandleRequirementAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="07042-208">They're singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

## <a name="support-authorization"></a><span data-ttu-id="07042-209">Support-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07042-209">Support authorization</span></span>

<span data-ttu-id="07042-210">In diesem Abschnitt Aktualisieren der Razor-Seiten und fügen Sie eine Operations Anforderungen-Klasse.</span><span class="sxs-lookup"><span data-stu-id="07042-210">In this section, you update the Razor Pages and add an operations requirements class.</span></span>

### <a name="review-the-contact-operations-requirements-class"></a><span data-ttu-id="07042-211">Überprüfen Sie die Kontaktinformationen Vorgänge Anforderungen-Klasse</span><span class="sxs-lookup"><span data-stu-id="07042-211">Review the contact operations requirements class</span></span>

<span data-ttu-id="07042-212">Überprüfen Sie die `ContactOperations` Klasse.</span><span class="sxs-lookup"><span data-stu-id="07042-212">Review the `ContactOperations` class.</span></span> <span data-ttu-id="07042-213">Diese Klasse enthält die Anforderungen der app unterstützt:</span><span class="sxs-lookup"><span data-stu-id="07042-213">This class contains the requirements the app supports:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a><span data-ttu-id="07042-214">Erstellen Sie eine Basisklasse für die Razor-Seiten</span><span class="sxs-lookup"><span data-stu-id="07042-214">Create a base class for the Razor Pages</span></span>

<span data-ttu-id="07042-215">Erstellen Sie eine Basisklasse, die in den Kontakten Razor-Seiten verwendeten Dienste enthält.</span><span class="sxs-lookup"><span data-stu-id="07042-215">Create a base class that contains the services used in the contacts Razor Pages.</span></span> <span data-ttu-id="07042-216">Die Basisklasse setzt diese Initialisierungscode an einem zentralen Ort:</span><span class="sxs-lookup"><span data-stu-id="07042-216">The base class puts that initialization code in one location:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

<span data-ttu-id="07042-217">Der vorangehende Code:</span><span class="sxs-lookup"><span data-stu-id="07042-217">The preceding code:</span></span>

* <span data-ttu-id="07042-218">Fügt der `IAuthorizationService` Dienst den Zugriff auf die Autorisierung Handler.</span><span class="sxs-lookup"><span data-stu-id="07042-218">Adds the `IAuthorizationService` service to access to the authorization handlers.</span></span>
* <span data-ttu-id="07042-219">Fügt die Identität `UserManager` Dienst.</span><span class="sxs-lookup"><span data-stu-id="07042-219">Adds the Identity `UserManager` service.</span></span>
* <span data-ttu-id="07042-220">Fügen Sie die `ApplicationDbContext` hinzu.</span><span class="sxs-lookup"><span data-stu-id="07042-220">Add the `ApplicationDbContext`.</span></span>

### <a name="update-the-createmodel"></a><span data-ttu-id="07042-221">Aktualisieren der CreateModel</span><span class="sxs-lookup"><span data-stu-id="07042-221">Update the CreateModel</span></span>

<span data-ttu-id="07042-222">Aktualisieren den erstellen Seite Modell Konstruktor verwendet den `DI_BasePageModel` Basisklasse:</span><span class="sxs-lookup"><span data-stu-id="07042-222">Update the create page model constructor to use the `DI_BasePageModel` base class:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

<span data-ttu-id="07042-223">Update der `CreateModel.OnPostAsync` aufzurufende Methode:</span><span class="sxs-lookup"><span data-stu-id="07042-223">Update the `CreateModel.OnPostAsync` method to:</span></span>

* <span data-ttu-id="07042-224">Fügen Sie die Benutzer-ID an den `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="07042-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="07042-225">Rufen Sie den Handler Autorisierung, um sicherzustellen, dass der Benutzer die Berechtigung zum Erstellen von Kontakten verfügt.</span><span class="sxs-lookup"><span data-stu-id="07042-225">Call the authorization handler to verify the user has permission to create contacts.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a><span data-ttu-id="07042-226">Aktualisieren Sie die IndexModel</span><span class="sxs-lookup"><span data-stu-id="07042-226">Update the IndexModel</span></span>

<span data-ttu-id="07042-227">Update der `OnGetAsync` Methode, sodass nur genehmigte Kontakte für allgemeine Benutzer angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="07042-227">Update the `OnGetAsync` method so only approved contacts are shown to general users:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a><span data-ttu-id="07042-228">Aktualisieren Sie die EditModel</span><span class="sxs-lookup"><span data-stu-id="07042-228">Update the EditModel</span></span>

<span data-ttu-id="07042-229">Fügen Sie einen Handler Autorisierung, um sicherzustellen, dass der Benutzer im Besitz des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="07042-229">Add an authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="07042-230">Da Ressource Autorisierung überprüft wird, die `[Authorize]` Attribut ist nicht ausreichend.</span><span class="sxs-lookup"><span data-stu-id="07042-230">Because resource authorization is being validated, the `[Authorize]` attribute is not enough.</span></span> <span data-ttu-id="07042-231">Die app hat keinen Zugriff auf die Ressource, wenn Attribute ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="07042-231">The app doesn't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="07042-232">Autorisierung ressourcenbasierte muss zwingend erforderlich.</span><span class="sxs-lookup"><span data-stu-id="07042-232">Resource-based authorization must be imperative.</span></span> <span data-ttu-id="07042-233">Überprüfungen müssen ausgeführt werden, sobald die app Zugriff auf die Ressource durch Laden in das Modell oder durch Laden in den Handler selbst hat.</span><span class="sxs-lookup"><span data-stu-id="07042-233">Checks must be performed once the app has access to the resource, either by loading it in the page model or by loading it within the handler itself.</span></span> <span data-ttu-id="07042-234">Durch Übergeben der Ressourcenschlüssel greifen Sie häufig die Ressource.</span><span class="sxs-lookup"><span data-stu-id="07042-234">You frequently access the resource by passing in the resource key.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a><span data-ttu-id="07042-235">Aktualisieren Sie die DeleteModel</span><span class="sxs-lookup"><span data-stu-id="07042-235">Update the DeleteModel</span></span>

<span data-ttu-id="07042-236">Aktualisieren Sie das Modell löschen Seite, um die Autorisierung Handler zu verwenden, um sicherzustellen, dass der Benutzer über die Löschberechtigung für den Kontakt verfügt.</span><span class="sxs-lookup"><span data-stu-id="07042-236">Update the delete page model to use the authorization handler to verify the user has delete permission on the contact.</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="07042-237">Fügen Sie dem Prüfdienst in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="07042-237">Inject the authorization service into the views</span></span>

<span data-ttu-id="07042-238">Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Daten, die der Benutzer ändern kann.</span><span class="sxs-lookup"><span data-stu-id="07042-238">Currently, the UI shows edit and delete links for data the user can't modify.</span></span> <span data-ttu-id="07042-239">Die Benutzeroberfläche ist durch Anwenden des Autorisierung Handlers, die den Ansichten fest.</span><span class="sxs-lookup"><span data-stu-id="07042-239">The UI is fixed by applying the authorization handler to the views.</span></span>

<span data-ttu-id="07042-240">Fügen Sie dem Prüfdienst in der *Views/_ViewImports.cshtml* Datei, damit er für alle Ansichten verfügbar ist:</span><span class="sxs-lookup"><span data-stu-id="07042-240">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it's available to all views:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

<span data-ttu-id="07042-241">Das vorhergehende Markup fügt mehrere `using` Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="07042-241">The preceding markup adds several `using` statements.</span></span>

<span data-ttu-id="07042-242">Update der **bearbeiten** und **löschen** verknüpft *Pages/Contacts/Index.cshtml* , sodass sie nur für Benutzer mit den entsprechenden Berechtigungen gerendert werden:</span><span class="sxs-lookup"><span data-stu-id="07042-242">Update the **Edit** and **Delete** links in *Pages/Contacts/Index.cshtml* so they're only rendered for users with the appropriate permissions:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-)]

> [!WARNING]
> <span data-ttu-id="07042-243">Ausblenden von Links von Benutzern, die keine Berechtigung zum Ändern von Daten haben Sichern nicht die app.</span><span class="sxs-lookup"><span data-stu-id="07042-243">Hiding links from users that don't have permission to change data doesn't secure the app.</span></span> <span data-ttu-id="07042-244">Durch das Ausblenden Links wird die app durch Anzeigen der einzige gültige Links Benutzerfreundlicher.</span><span class="sxs-lookup"><span data-stu-id="07042-244">Hiding links makes the app more user-friendly by displaying only valid links.</span></span> <span data-ttu-id="07042-245">Benutzer können die generierten URLs zum Aufrufen bearbeiten und Löschen von Operationen mit Daten, die sie besitzen keine hack.</span><span class="sxs-lookup"><span data-stu-id="07042-245">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span> <span data-ttu-id="07042-246">Der Razor-Seite oder ein Domänencontroller muss zugriffsüberprüfungen zum Schutz der Daten erzwingen.</span><span class="sxs-lookup"><span data-stu-id="07042-246">The Razor Page or controller must enforce access checks to secure the data.</span></span>

### <a name="update-details"></a><span data-ttu-id="07042-247">Updatedetails</span><span class="sxs-lookup"><span data-stu-id="07042-247">Update Details</span></span>

<span data-ttu-id="07042-248">Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:</span><span class="sxs-lookup"><span data-stu-id="07042-248">Update the details view so managers can approve or reject contacts:</span></span>

[!code-cshtml[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-)]

<span data-ttu-id="07042-249">Aktualisieren Sie das Modell Details:</span><span class="sxs-lookup"><span data-stu-id="07042-249">Update the details page model:</span></span>

[!code-csharp[Main](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a><span data-ttu-id="07042-250">Abgeschlossene Anwendung testen</span><span class="sxs-lookup"><span data-stu-id="07042-250">Test the completed app</span></span>

<span data-ttu-id="07042-251">Wenn Sie mithilfe von Visual Studio-Code oder auf einer lokalen-Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="07042-251">If you're using Visual Studio Code or testing on a local platform that doesn't include a test certificate for SSL:</span></span>

* <span data-ttu-id="07042-252">Legen Sie `"LocalTest:skipSSL": true` in der *"appSettings". Developement.JSON* Datei, um die SSL-Anforderung zu überspringen.</span><span class="sxs-lookup"><span data-stu-id="07042-252">Set `"LocalTest:skipSSL": true` in the *appsettings.Developement.json* file to skip the SSL requirement.</span></span> <span data-ttu-id="07042-253">Überspringen Sie SSL nur auf einem Entwicklungscomputer.</span><span class="sxs-lookup"><span data-stu-id="07042-253">Skip SSL only on a development machine.</span></span>

<span data-ttu-id="07042-254">Wenn die app Kontakte besitzt:</span><span class="sxs-lookup"><span data-stu-id="07042-254">If the app has contacts:</span></span>

* <span data-ttu-id="07042-255">Löschen aller Datensätze in der `Contact` Tabelle.</span><span class="sxs-lookup"><span data-stu-id="07042-255">Delete all the records in the `Contact` table.</span></span>
* <span data-ttu-id="07042-256">Starten Sie die app Ausgangswert für die Datenbank neu.</span><span class="sxs-lookup"><span data-stu-id="07042-256">Restart the app to seed the database.</span></span>

<span data-ttu-id="07042-257">Registrieren Sie einen Benutzer zum Durchsuchen der Kontakte.</span><span class="sxs-lookup"><span data-stu-id="07042-257">Register a user for browsing the contacts.</span></span>

<span data-ttu-id="07042-258">Eine einfache Möglichkeit zum Testen Sie die abgeschlossenen app wird auf drei verschiedenen Browsern (oder inkognito/InPrivate-Versionen) zu starten.</span><span class="sxs-lookup"><span data-stu-id="07042-258">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="07042-259">In einem Browser eingeben, einen neuen Benutzer registrieren (z. B. `test@example.com`).</span><span class="sxs-lookup"><span data-stu-id="07042-259">In one browser, register a new user (for example, `test@example.com`).</span></span> <span data-ttu-id="07042-260">Melden Sie sich mit einem anderen Benutzer jedem Browser an.</span><span class="sxs-lookup"><span data-stu-id="07042-260">Sign in to each browser with a different user.</span></span> <span data-ttu-id="07042-261">Überprüfen Sie die folgenden Vorgänge aus:</span><span class="sxs-lookup"><span data-stu-id="07042-261">Verify the following operations:</span></span>

* <span data-ttu-id="07042-262">Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="07042-262">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="07042-263">Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="07042-263">Registered users can edit/delete their own data.</span></span>
* <span data-ttu-id="07042-264">Manager können genehmigen oder ablehnen Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="07042-264">Managers can approve or reject contact data.</span></span> <span data-ttu-id="07042-265">Die `Details` Ansicht zeigt **genehmigen** und **ablehnen** Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="07042-265">The `Details` view shows **Approve** and **Reject** buttons.</span></span>
* <span data-ttu-id="07042-266">Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="07042-266">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="07042-267">Benutzer</span><span class="sxs-lookup"><span data-stu-id="07042-267">User</span></span>| <span data-ttu-id="07042-268">Optionen</span><span class="sxs-lookup"><span data-stu-id="07042-268">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="07042-269">Können bearbeiten/eigene Daten löschen</span><span class="sxs-lookup"><span data-stu-id="07042-269">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="07042-270">Können ablehnen/genehmigen und bearbeiten/löschen, die Daten besitzen</span><span class="sxs-lookup"><span data-stu-id="07042-270">Can approve/reject and edit/delete own data</span></span> |
| admin@contoso.com | <span data-ttu-id="07042-271">Können bearbeiten/löschen und alle Daten genehmigen/ablehnen</span><span class="sxs-lookup"><span data-stu-id="07042-271">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="07042-272">Erstellen eines Kontakts in der Administrator-Browser.</span><span class="sxs-lookup"><span data-stu-id="07042-272">Create a contact in the administrator's browser.</span></span> <span data-ttu-id="07042-273">Kopieren Sie die URL zum Löschen und Bearbeiten von den Administrator wenden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="07042-273">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="07042-274">Fügen Sie diese Links in den Browser des Testbenutzers zu überprüfen, ob der Testbenutzer diese Vorgänge ausführen kann nicht aus.</span><span class="sxs-lookup"><span data-stu-id="07042-274">Paste these links into the test user's browser to verify the test user can't perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="07042-275">Erstellen der Starter-app</span><span class="sxs-lookup"><span data-stu-id="07042-275">Create the starter app</span></span>

* <span data-ttu-id="07042-276">Erstellen Sie eine Razor-Seiten-app mit dem Namen "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="07042-276">Create a Razor Pages app named "ContactManager"</span></span>

  * <span data-ttu-id="07042-277">Erstellen Sie die app mit **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="07042-277">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="07042-278">Nennen Sie sie "ContactManager", sodass Ihr Namespace den Namespace, die im Beispiel verwendeten entspricht.</span><span class="sxs-lookup"><span data-stu-id="07042-278">Name it "ContactManager" so your namespace matches the namespace used in the sample.</span></span>

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * <span data-ttu-id="07042-279">`-uld`Gibt an, anstelle von SQLite LocalDB</span><span class="sxs-lookup"><span data-stu-id="07042-279">`-uld` specifies LocalDB instead of SQLite</span></span>

* <span data-ttu-id="07042-280">Fügen Sie die folgenden `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="07042-280">Add the following `Contact` model:</span></span>

  [!code-csharp[Main](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* <span data-ttu-id="07042-281">Gerüst der `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="07042-281">Scaffold the `Contact` model:</span></span>

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* <span data-ttu-id="07042-282">Update der **ContactManager** Verankern der *Pages/_Layout.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="07042-282">Update the **ContactManager** anchor in the *Pages/_Layout.cshtml* file:</span></span>

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* <span data-ttu-id="07042-283">Erstellen der anfänglichen Migrations und die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="07042-283">Scaffold the initial migration and update the database:</span></span>

```console
dotnet ef migrations add initial
dotnet ef database update
```

* <span data-ttu-id="07042-284">Testen der app durch erstellen, bearbeiten und Löschen eines Kontakts</span><span class="sxs-lookup"><span data-stu-id="07042-284">Test the app by creating, editing, and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="07042-285">Ausführen eines Seedings für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="07042-285">Seed the database</span></span>

<span data-ttu-id="07042-286">Hinzufügen der `SeedData` Klasse, um die *Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="07042-286">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="07042-287">Wenn Sie das Beispiel heruntergeladen haben, können Sie kopieren die *SeedData.cs* Datei wird in der *Daten* Ordner des Startprojekts.</span><span class="sxs-lookup"><span data-stu-id="07042-287">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="07042-288">Rufen Sie `SeedData.Initialize` aus `Main`:</span><span class="sxs-lookup"><span data-stu-id="07042-288">Call `SeedData.Initialize` from `Main`:</span></span>

[!code-csharp[Main](secure-data/samples/starter2/Program.cs?name=snippet)]

<span data-ttu-id="07042-289">Testen Sie, ob die Anwendung die Datenbank mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="07042-289">Test that the app seeded the database.</span></span> <span data-ttu-id="07042-290">Wenn alle Zeilen in den Kontakt DB vorhanden sind, nicht der Seed-Methode ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="07042-290">If there are any rows in the contact DB, the seed method doesn't run.</span></span>

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a><span data-ttu-id="07042-291">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="07042-291">Additional resources</span></span>

* <span data-ttu-id="07042-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="07042-292">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="07042-293">Diese Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.</span><span class="sxs-lookup"><span data-stu-id="07042-293">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="07042-294">Autorisierung in ASP.NET Core: einfach, anspruchsbasierte und benutzerdefinierten Rolle</span><span class="sxs-lookup"><span data-stu-id="07042-294">Authorization in ASP.NET Core: Simple, role, claims-based, and custom</span></span>](xref:security/authorization/index)
* [<span data-ttu-id="07042-295">Benutzerdefinierte, richtlinienbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="07042-295">Custom policy-based authorization</span></span>](xref:security/authorization/policies)