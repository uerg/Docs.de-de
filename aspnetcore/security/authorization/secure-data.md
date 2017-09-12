---
title: "Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt"
author: rick-anderson
keywords: ASP.NET Core, MVC, Autorisierung, Rollen, Sicherheit, administrator
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: db05ffb585022c3d9512d32da28c54788f97129c
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/12/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a><span data-ttu-id="3215d-103">Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt</span><span class="sxs-lookup"><span data-stu-id="3215d-103">Create an ASP.NET Core app with user data protected by authorization</span></span>

<span data-ttu-id="3215d-104">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Joe Audette](https://twitter.com/joeaudette)</span><span class="sxs-lookup"><span data-stu-id="3215d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Joe Audette](https://twitter.com/joeaudette)</span></span>

<span data-ttu-id="3215d-105">In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer WebApp mit Benutzerdaten durch Autorisierung geschützt wird.</span><span class="sxs-lookup"><span data-stu-id="3215d-105">This tutorial shows how to create a web app with user data protected by authorization.</span></span> <span data-ttu-id="3215d-106">Es zeigt eine Liste von Kontakten, die authentifizierte Benutzer (registrierten) erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="3215d-106">It  displays a list of contacts that authenticated (registered) users have created.</span></span> <span data-ttu-id="3215d-107">Es gibt drei Sicherheitsgruppen:</span><span class="sxs-lookup"><span data-stu-id="3215d-107">There are three security groups:</span></span>

* <span data-ttu-id="3215d-108">Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="3215d-108">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="3215d-109">Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="3215d-109">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="3215d-110">Manager können genehmigen oder ablehnen Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="3215d-110">Managers can approve or reject contact data.</span></span> <span data-ttu-id="3215d-111">Nur genehmigte Kontakte werden für Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3215d-111">Only approved contacts are visible to users.</span></span>
* <span data-ttu-id="3215d-112">Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="3215d-112">Administrators can approve/reject and edit/delete any data.</span></span>

<span data-ttu-id="3215d-113">In der folgenden Abbildung, Benutzer Rick (`rick@example.com`) angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-113">In the following image, user Rick (`rick@example.com`) is signed in.</span></span> <span data-ttu-id="3215d-114">Rick kann nur Benutzeransicht Kontakte genehmigt und seine Kontakte bearbeiten/löschen.</span><span class="sxs-lookup"><span data-stu-id="3215d-114">User Rick can only view approved contacts and edit/delete his contacts.</span></span> <span data-ttu-id="3215d-115">Nur der letzte Datensatz erstellt, indem Rick zeigt bearbeiten und Löschen von Verknüpfungen</span><span class="sxs-lookup"><span data-stu-id="3215d-115">Only the last record, created by Rick, displays edit and delete links</span></span>

![Bild oben beschriebenen](secure-data/_static/rick.png)

<span data-ttu-id="3215d-117">In der folgenden Abbildung `manager@contoso.com` in und in der Rolle "Manager" signiert ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-117">In the following image, `manager@contoso.com` is signed in and in the managers role.</span></span> 

![Bild oben beschriebenen](secure-data/_static/manager1.png)

<span data-ttu-id="3215d-119">Die folgende Abbildung zeigt den Managern Detailansicht eines Kontakts an.</span><span class="sxs-lookup"><span data-stu-id="3215d-119">The following image shows the  managers details view of a contact.</span></span>

![Bild oben beschriebenen](secure-data/_static/manager.png)

<span data-ttu-id="3215d-121">Nur Manager und Administratoren haben die genehmigen und Ablehnen von Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="3215d-121">Only managers and administrators have the approve and reject buttons.</span></span>

<span data-ttu-id="3215d-122">In der folgenden Abbildung `admin@contoso.com` signiert ist, in und in der Administratorrolle sein.</span><span class="sxs-lookup"><span data-stu-id="3215d-122">In the following image, `admin@contoso.com` is signed in and in the administrator’s role.</span></span> 

![Bild oben beschriebenen](secure-data/_static/admin.png)

<span data-ttu-id="3215d-124">Der Administrator verfügt über alle Berechtigungen.</span><span class="sxs-lookup"><span data-stu-id="3215d-124">The administrator has all privileges.</span></span> <span data-ttu-id="3215d-125">Sie können eine beliebige kontaktieren, Lese-/bearbeiten/löschen und ändern Sie den Status von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="3215d-125">She can read/edit/delete any contact and change the status of contacts.</span></span>

<span data-ttu-id="3215d-126">Die app wurde erstellt, indem [Gerüstbau](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) Folgendes `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="3215d-126">The app was created by [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller)  the following `Contact` model:</span></span>

<span data-ttu-id="3215d-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3215d-127">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

<span data-ttu-id="3215d-128">Ein `ContactIsOwnerAuthorizationHandler` Autorisierung-Handler wird sichergestellt, dass Benutzer nur ihre Daten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="3215d-128">A `ContactIsOwnerAuthorizationHandler` authorization handler ensures that a user can only edit their data.</span></span> <span data-ttu-id="3215d-129">Ein `ContactManagerAuthorizationHandler` Autorisierung Handler ermöglicht Manager zum Genehmigen oder ablehnen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="3215d-129">A `ContactManagerAuthorizationHandler` authorization handler allows managers to approve or reject contacts.</span></span>  <span data-ttu-id="3215d-130">Ein `ContactAdministratorsAuthorizationHandler` Autorisierung Handler ermöglicht Administratoren zum Genehmigen oder ablehnen von Kontakten und Bearbeiten/Löschen von Kontakten.</span><span class="sxs-lookup"><span data-stu-id="3215d-130">A `ContactAdministratorsAuthorizationHandler` authorization handler allows administrators to approve or reject contacts and to edit/delete contacts.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3215d-131">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="3215d-131">Prerequisites</span></span>

<span data-ttu-id="3215d-132">Dies ist kein Lernprogramm ab.</span><span class="sxs-lookup"><span data-stu-id="3215d-132">This is not a beginning tutorial.</span></span> <span data-ttu-id="3215d-133">Sie sollten mit vertraut sein:</span><span class="sxs-lookup"><span data-stu-id="3215d-133">You should be familiar with:</span></span>

* [<span data-ttu-id="3215d-134">ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="3215d-134">ASP.NET Core MVC</span></span>](xref:tutorials/first-mvc-app/start-mvc)
* [<span data-ttu-id="3215d-135">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3215d-135">Entity Framework Core</span></span>](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a><span data-ttu-id="3215d-136">Die Starter und abgeschlossene Anwendung</span><span class="sxs-lookup"><span data-stu-id="3215d-136">The starter and completed app</span></span>

<span data-ttu-id="3215d-137">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [abgeschlossen](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span><span class="sxs-lookup"><span data-stu-id="3215d-137">[Download](xref:tutorials/index#how-to-download-a-sample) the [completed](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) app.</span></span> <span data-ttu-id="3215d-138">[Test](#test-the-completed-app) abgeschlossene Anwendung, damit Sie mit der Sicherheitsfunktionen vertraut machen.</span><span class="sxs-lookup"><span data-stu-id="3215d-138">[Test](#test-the-completed-app) the completed app so you become familiar with its security features.</span></span> 

### <a name="the-starter-app"></a><span data-ttu-id="3215d-139">Die Starter-app</span><span class="sxs-lookup"><span data-stu-id="3215d-139">The starter app</span></span>

<span data-ttu-id="3215d-140">Es ist hilfreich, den Code mit dem vollständigen Beispiel verglichen werden soll.</span><span class="sxs-lookup"><span data-stu-id="3215d-140">It's helpful to compare your code with the completed sample.</span></span>

<span data-ttu-id="3215d-141">[Herunterladen](xref:tutorials/index#how-to-download-a-sample) der [Starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span><span class="sxs-lookup"><span data-stu-id="3215d-141">[Download](xref:tutorials/index#how-to-download-a-sample) the [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) app.</span></span> 

<span data-ttu-id="3215d-142">Finden Sie unter [Starter-app erstellen](#create-the-starter-app) , wenn Sie von Grund auf neu erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="3215d-142">See [Create the starter app](#create-the-starter-app) if you'd like to create it from scratch.</span></span>

<span data-ttu-id="3215d-143">Die Datenbank zu aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="3215d-143">Update the database:</span></span>

```none
   dotnet ef database update
```

<span data-ttu-id="3215d-144">Führen Sie die app, tippen Sie auf die **ContactManager** verknüpfen, und überprüfen Sie Sie erstellen, bearbeiten und Löschen eines Kontakts.</span><span class="sxs-lookup"><span data-stu-id="3215d-144">Run the app, tap the **ContactManager** link, and verify you can create, edit, and delete a contact.</span></span>

<span data-ttu-id="3215d-145">Dieses Lernprogramm sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app.</span><span class="sxs-lookup"><span data-stu-id="3215d-145">This tutorial has all the major steps to create the secure user data app.</span></span> <span data-ttu-id="3215d-146">Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="3215d-146">You may find it helpful to refer to the completed project.</span></span>

## <a name="modify-the-app-to-secure-user-data"></a><span data-ttu-id="3215d-147">Ändern Sie die app aus, um Benutzerdaten zu sichern.</span><span class="sxs-lookup"><span data-stu-id="3215d-147">Modify the app to secure user data</span></span>

<span data-ttu-id="3215d-148">In den folgenden Abschnitten sind die wichtigsten Schritte zum Erstellen der sicheren Benutzer Daten app.</span><span class="sxs-lookup"><span data-stu-id="3215d-148">The following sections have all the major steps to create the secure user data app.</span></span> <span data-ttu-id="3215d-149">Sie können es zum Verweisen auf das abgeschlossene Projekt hilfreich sein.</span><span class="sxs-lookup"><span data-stu-id="3215d-149">You may find it helpful to refer to the completed project.</span></span>

### <a name="tie-the-contact-data-to-the-user"></a><span data-ttu-id="3215d-150">Binden Sie die Kontaktdaten für den Benutzer</span><span class="sxs-lookup"><span data-stu-id="3215d-150">Tie the contact data to the user</span></span>

<span data-ttu-id="3215d-151">Verwenden Sie die ASP.NET [Identität](xref:security/authentication/identity) Benutzer-ID, um sicherzustellen, dass Benutzer ihre Daten, aber nicht für andere Benutzerdaten bearbeiten kann.</span><span class="sxs-lookup"><span data-stu-id="3215d-151">Use the ASP.NET [Identity](xref:security/authentication/identity) user ID to ensure users can edit their data, but not other users data.</span></span> <span data-ttu-id="3215d-152">Hinzufügen `OwnerID` auf die `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="3215d-152">Add `OwnerID` to the `Contact` model:</span></span>

<span data-ttu-id="3215d-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span><span class="sxs-lookup"><span data-stu-id="3215d-153">[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]</span></span>

<span data-ttu-id="3215d-154">`OwnerID`ist die ID des Benutzers aus der `AspNetUser` -Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3215d-154">`OwnerID` is the user's ID from the `AspNetUser` table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="3215d-155">Die `Status` Feld bestimmt, ob ein Kontakt allgemeine Benutzer sichtbar ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-155">The `Status` field determines if a contact is viewable by general users.</span></span> 

<span data-ttu-id="3215d-156">Das Gerüst für eine neue Migration erstellen und Aktualisieren der Datenbank:</span><span class="sxs-lookup"><span data-stu-id="3215d-156">Scaffold a new migration and update the database:</span></span>

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a><span data-ttu-id="3215d-157">Erfordern von SSL und authentifizierte Benutzer</span><span class="sxs-lookup"><span data-stu-id="3215d-157">Require SSL and authenticated users</span></span>

<span data-ttu-id="3215d-158">In der `ConfigureServices` Methode der *Startup.cs* hinzufügen. die [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) Autorisierungsfilter:</span><span class="sxs-lookup"><span data-stu-id="3215d-158">In the `ConfigureServices` method of the *Startup.cs* file, add the [RequireHttpsAttribute](https://docs.microsoft.com/aspnet/core/api) authorization filter:</span></span>

<span data-ttu-id="3215d-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span><span class="sxs-lookup"><span data-stu-id="3215d-159">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]</span></span>

<span data-ttu-id="3215d-160">Wenn Sie Visual Studio verwenden, finden Sie unter [Einrichten von IIS Express für SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span><span class="sxs-lookup"><span data-stu-id="3215d-160">If you're using Visual Studio, see [Set up IIS Express for SSL/HTTPS](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps).</span></span> <span data-ttu-id="3215d-161">Zum Umleiten von HTTP-Anforderungen zu HTTPS finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="3215d-161">To redirect HTTP requests to HTTPS, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="3215d-162">Wenn Sie mithilfe von Visual Studio-Code oder auf lokale Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="3215d-162">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="3215d-163">Legen Sie `"LocalTest:skipSSL": true` in der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="3215d-163">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

### <a name="require-authenticated-users"></a><span data-ttu-id="3215d-164">Erfordern Sie authentifizierte Benutzern</span><span class="sxs-lookup"><span data-stu-id="3215d-164">Require authenticated users</span></span>

<span data-ttu-id="3215d-165">Legen Sie die Standardrichtlinie für die Authentifizierung der Benutzer authentifiziert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="3215d-165">Set the default authentication policy to require users to be authenticated.</span></span> <span data-ttu-id="3215d-166">Sie können ablehnen der Authentifizierung auf dem Controller bzw. die Aktionsmethode-Methode, mit der `[AllowAnonymous]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="3215d-166">You can opt out of authentication at the controller or action method with the `[AllowAnonymous]` attribute.</span></span> <span data-ttu-id="3215d-167">Bei diesem Ansatz erfordern neue Domänencontroller hinzugefügt automatisch Authentifizierung, die sicherer ist als der vertrauenden Seite auf neue Domänencontroller enthalten die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="3215d-167">With this approach, any new controllers added will automatically require authentication, which is safer than relying on new controllers to include the `[Authorize]` attribute.</span></span> <span data-ttu-id="3215d-168">Fügen Sie Folgendes an der `ConfigureServices` Methode der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="3215d-168">Add the following to  the `ConfigureServices` method of the *Startup.cs* file:</span></span>

<span data-ttu-id="3215d-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span><span class="sxs-lookup"><span data-stu-id="3215d-169">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]</span></span>

<span data-ttu-id="3215d-170">Hinzufügen `[AllowAnonymous]` zum home-Controller, sodass anonyme Benutzer Informationen über die Website abrufen können, bevor sie registriert werden.</span><span class="sxs-lookup"><span data-stu-id="3215d-170">Add `[AllowAnonymous]` to the home controller so anonymous users can get information about the site before they register.</span></span>

<span data-ttu-id="3215d-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span><span class="sxs-lookup"><span data-stu-id="3215d-171">[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]</span></span>

### <a name="configure-the-test-account"></a><span data-ttu-id="3215d-172">Konfigurieren Sie das Testkonto an</span><span class="sxs-lookup"><span data-stu-id="3215d-172">Configure the test account</span></span>

<span data-ttu-id="3215d-173">Die `SeedData` Klasse zwei Konten "," Administrator "und"-Manager erstellt.</span><span class="sxs-lookup"><span data-stu-id="3215d-173">The `SeedData` class creates two accounts,  administrator and manager.</span></span> <span data-ttu-id="3215d-174">Verwenden der [Secret-Manager-Tool](xref:security/app-secrets) zum Festlegen eines Kennworts für diese Konten.</span><span class="sxs-lookup"><span data-stu-id="3215d-174">Use the [Secret Manager tool](xref:security/app-secrets) to set a password for these accounts.</span></span> <span data-ttu-id="3215d-175">Führen Sie diese aus dem Projektverzeichnis (das Verzeichnis mit *"Program.cs"*).</span><span class="sxs-lookup"><span data-stu-id="3215d-175">Do this from the project directory (the directory containing *Program.cs*).</span></span>

```console
dotnet user-secrets set SeedUserPW <PW>
```

<span data-ttu-id="3215d-176">Update `Configure` Testkennwort verwenden:</span><span class="sxs-lookup"><span data-stu-id="3215d-176">Update `Configure` to use the test password:</span></span>

<span data-ttu-id="3215d-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span><span class="sxs-lookup"><span data-stu-id="3215d-177">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]</span></span>

<span data-ttu-id="3215d-178">Fügen Sie die Administrator-Benutzer-ID und `Status = ContactStatus.Approved` für Kontakte.</span><span class="sxs-lookup"><span data-stu-id="3215d-178">Add the administrator user ID and `Status = ContactStatus.Approved` to the contacts.</span></span> <span data-ttu-id="3215d-179">Fügen Sie nur einen Kontakt angezeigt wird, die Benutzer-ID an alle Kontakte hinzu:</span><span class="sxs-lookup"><span data-stu-id="3215d-179">Only one contact is shown, add the user ID to all contacts:</span></span>

<span data-ttu-id="3215d-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span><span class="sxs-lookup"><span data-stu-id="3215d-180">[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]</span></span>

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a><span data-ttu-id="3215d-181">Erstellen Sie, Besitzer, Manager und Administrator Autorisierung Handler</span><span class="sxs-lookup"><span data-stu-id="3215d-181">Create owner, manager, and administrator authorization handlers</span></span>

<span data-ttu-id="3215d-182">Erstellen einer `ContactIsOwnerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="3215d-182">Create a `ContactIsOwnerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="3215d-183">Die `ContactIsOwnerAuthorizationHandler` werden überprüfen Sie, ob der Benutzer auf die Ressource dient die Ressource gehört.</span><span class="sxs-lookup"><span data-stu-id="3215d-183">The `ContactIsOwnerAuthorizationHandler` will verify the user acting on the resource owns the resource.</span></span>

<span data-ttu-id="3215d-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-184">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]</span></span>

<span data-ttu-id="3215d-185">Die `ContactIsOwnerAuthorizationHandler` Aufrufe `context.Succeed` ist der aktuelle authentifizierte Benutzer der Besitzer des Kontakts.</span><span class="sxs-lookup"><span data-stu-id="3215d-185">The `ContactIsOwnerAuthorizationHandler` calls `context.Succeed` if the current authenticated user is the contact owner.</span></span> <span data-ttu-id="3215d-186">Autorisierung Handler brauchbaren `context.Succeed` Wenn die Anforderungen erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="3215d-186">Authorization handlers generally return `context.Succeed` when the requirements are met.</span></span> <span data-ttu-id="3215d-187">Diese zurückgeben `Task.FromResult(0)` Wenn Anforderungen nicht erfüllt sind.</span><span class="sxs-lookup"><span data-stu-id="3215d-187">They return `Task.FromResult(0)` when requirements are not met.</span></span> <span data-ttu-id="3215d-188">`Task.FromResult(0)`weder Erfolg oder Fehler auftritt, wird dadurch andere Autorisierung Handler ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="3215d-188">`Task.FromResult(0)` is neither success or failure, it allows other authorization handler to run.</span></span> <span data-ttu-id="3215d-189">Wenn Sie explizit fehlschlägt müssen, zurückgeben `context.Fail()`.</span><span class="sxs-lookup"><span data-stu-id="3215d-189">If you need to explicitly fail, return `context.Fail()`.</span></span>

<span data-ttu-id="3215d-190">Wir können Kontakt Besitzer bearbeiten/ihre eigenen Daten löschen, damit wir nicht brauchen, überprüfen Sie den Vorgang im Parameters Anforderung übergeben.</span><span class="sxs-lookup"><span data-stu-id="3215d-190">We allow contact owners to edit/delete their own data, so we don't need to check the operation passed in the requirement parameter.</span></span>

### <a name="create-a-manager-authorization-handler"></a><span data-ttu-id="3215d-191">Erstellen Sie einen Ereignishandler der Manager-Autorisierung</span><span class="sxs-lookup"><span data-stu-id="3215d-191">Create a manager authorization handler</span></span>

<span data-ttu-id="3215d-192">Erstellen einer `ContactManagerAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="3215d-192">Create a `ContactManagerAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="3215d-193">Die `ContactManagerAuthorizationHandler` der Benutzer, die auf die Ressource ist ein Manager überprüft.</span><span class="sxs-lookup"><span data-stu-id="3215d-193">The `ContactManagerAuthorizationHandler` will verify the user acting on the resource is a manager.</span></span> <span data-ttu-id="3215d-194">Nur Manager können genehmigen oder Ablehnen der Änderungen am Inhalt (neue oder geänderte).</span><span class="sxs-lookup"><span data-stu-id="3215d-194">Only managers can approve or reject content changes (new or changed).</span></span>

<span data-ttu-id="3215d-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-195">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]</span></span>

### <a name="create-an-administrator-authorization-handler"></a><span data-ttu-id="3215d-196">Erstellen Sie einen Administrator-Autorisierung-Ereignishandler</span><span class="sxs-lookup"><span data-stu-id="3215d-196">Create an administrator authorization handler</span></span>

<span data-ttu-id="3215d-197">Erstellen einer `ContactAdministratorsAuthorizationHandler` -Klasse in der *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="3215d-197">Create a `ContactAdministratorsAuthorizationHandler` class in the  *Authorization* folder.</span></span> <span data-ttu-id="3215d-198">Die `ContactAdministratorsAuthorizationHandler` der Benutzer, die auf die Ressource ist ein Administrator überprüft.</span><span class="sxs-lookup"><span data-stu-id="3215d-198">The `ContactAdministratorsAuthorizationHandler` will verify the user acting on the resource is a administrator.</span></span> <span data-ttu-id="3215d-199">Administratoren kann alle Vorgänge tun.</span><span class="sxs-lookup"><span data-stu-id="3215d-199">Administrator can do all operations.</span></span>

<span data-ttu-id="3215d-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-200">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]</span></span>

## <a name="register-the-authorization-handlers"></a><span data-ttu-id="3215d-201">Registrieren Sie die Authorization-Handler</span><span class="sxs-lookup"><span data-stu-id="3215d-201">Register the authorization handlers</span></span>

<span data-ttu-id="3215d-202">Verwendung von Entity Framework Core Services müssen registriert werden, für die [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection) mit [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span><span class="sxs-lookup"><span data-stu-id="3215d-202">Services using Entity Framework Core must be registered for [dependency injection](xref:fundamentals/dependency-injection) using [AddScoped](https://docs.microsoft.com/aspnet/core/api).</span></span> <span data-ttu-id="3215d-203">Die `ContactIsOwnerAuthorizationHandler` verwendet ASP.NET Core [Identität](xref:security/authentication/identity), die basiert auf Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="3215d-203">The `ContactIsOwnerAuthorizationHandler` uses ASP.NET Core [Identity](xref:security/authentication/identity), which is built on Entity Framework Core.</span></span> <span data-ttu-id="3215d-204">Registrieren Sie die Handler mit die Auflistung, damit sie verfügbar sind, werden die `ContactsController` über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3215d-204">Register the handlers with the service collection so they will be available to the `ContactsController` through [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="3215d-205">Fügen Sie den folgenden Code am Ende der `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3215d-205">Add the following code to the end of `ConfigureServices`:</span></span>

<span data-ttu-id="3215d-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span><span class="sxs-lookup"><span data-stu-id="3215d-206">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]</span></span>

<span data-ttu-id="3215d-207">`ContactAdministratorsAuthorizationHandler`und `ContactManagerAuthorizationHandler` als Singletons hinzugefügt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="3215d-207">`ContactAdministratorsAuthorizationHandler` and `ContactManagerAuthorizationHandler` are added as singletons.</span></span> <span data-ttu-id="3215d-208">Sie Singletons sind, da sie nicht EF verwenden und alle erforderlichen Informationen in den `Context` Parameter von der `HandleRequirementAsync` Methode.</span><span class="sxs-lookup"><span data-stu-id="3215d-208">They are singletons because they don't use EF and all the information needed is in the `Context` parameter of the `HandleRequirementAsync` method.</span></span>

<span data-ttu-id="3215d-209">Die vollständige `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3215d-209">The complete `ConfigureServices`:</span></span>

<span data-ttu-id="3215d-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span><span class="sxs-lookup"><span data-stu-id="3215d-210">[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]</span></span>

## <a name="update-the-code-to-support-authorization"></a><span data-ttu-id="3215d-211">Aktualisieren Sie den Code, um die Autorisierung zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="3215d-211">Update the code to support authorization</span></span>

<span data-ttu-id="3215d-212">In diesem Abschnitt Aktualisieren Sie den Controller und Ansichten und fügen Sie eine Operations Anforderungen-Klasse.</span><span class="sxs-lookup"><span data-stu-id="3215d-212">In this section, you update the controller and views and add an operations requirements class.</span></span>

### <a name="update-the-contacts-controller"></a><span data-ttu-id="3215d-213">Aktualisieren Sie den Controller Kontakte</span><span class="sxs-lookup"><span data-stu-id="3215d-213">Update the Contacts controller</span></span>

<span data-ttu-id="3215d-214">Update der `ContactsController` Konstruktor:</span><span class="sxs-lookup"><span data-stu-id="3215d-214">Update the `ContactsController` constructor:</span></span>

* <span data-ttu-id="3215d-215">Hinzufügen der `IAuthorizationService` Dienst den Zugriff auf die Autorisierung Handler.</span><span class="sxs-lookup"><span data-stu-id="3215d-215">Add the `IAuthorizationService` service to  access to the authorization handlers.</span></span> 
* <span data-ttu-id="3215d-216">Hinzufügen der `Identity` `UserManager` Dienst:</span><span class="sxs-lookup"><span data-stu-id="3215d-216">Add the `Identity` `UserManager` service:</span></span>

<span data-ttu-id="3215d-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span><span class="sxs-lookup"><span data-stu-id="3215d-217">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]</span></span>

### <a name="add-a-contact-operations-requirements-class"></a><span data-ttu-id="3215d-218">Fügen Sie eine wenden Vorgänge Anforderungen-Klasse</span><span class="sxs-lookup"><span data-stu-id="3215d-218">Add a contact operations requirements class</span></span>

<span data-ttu-id="3215d-219">Hinzufügen der `ContactOperationsRequirements` Klasse, um die *Autorisierung* Ordner.</span><span class="sxs-lookup"><span data-stu-id="3215d-219">Add the `ContactOperationsRequirements` class to the *Authorization* folder.</span></span> <span data-ttu-id="3215d-220">Diese Klasse enthalten, die Anforderungen unserer app unterstützt:</span><span class="sxs-lookup"><span data-stu-id="3215d-220">This class  contain the requirements our app supports:</span></span>

<span data-ttu-id="3215d-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-221">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

### <a name="update-create"></a><span data-ttu-id="3215d-222">Update zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3215d-222">Update Create</span></span>

<span data-ttu-id="3215d-223">Update der `HTTP POST Create` aufzurufende Methode:</span><span class="sxs-lookup"><span data-stu-id="3215d-223">Update the `HTTP POST Create` method to:</span></span>

* <span data-ttu-id="3215d-224">Fügen Sie die Benutzer-ID an den `Contact` Modell.</span><span class="sxs-lookup"><span data-stu-id="3215d-224">Add the user ID to the `Contact` model.</span></span>
* <span data-ttu-id="3215d-225">Rufen Sie den Handler Autorisierung, um sicherzustellen, dass der Benutzer im Besitz des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-225">Call the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="3215d-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="3215d-226">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]</span></span>

### <a name="update-edit"></a><span data-ttu-id="3215d-227">Update bearbeiten</span><span class="sxs-lookup"><span data-stu-id="3215d-227">Update Edit</span></span>

<span data-ttu-id="3215d-228">Aktualisieren Sie sowohl `Edit` Methoden, um den Handler für die Autorisierung verwenden, um zu überprüfen, ob den Benutzer der Besitzer des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-228">Update both `Edit` methods to use the authorization handler to verify the user owns the contact.</span></span> <span data-ttu-id="3215d-229">Da wir Autorisierung Ressource ausführen kann nicht verwendet die `[Authorize]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="3215d-229">Because we are performing resource authorization we cannot use the `[Authorize]` attribute.</span></span> <span data-ttu-id="3215d-230">Wir haben keinen Zugriff auf die Ressource, wenn Attribute ausgewertet werden.</span><span class="sxs-lookup"><span data-stu-id="3215d-230">We don't have access to the resource when attributes are evaluated.</span></span> <span data-ttu-id="3215d-231">Ressourcenbasierter Autorisierung muss unbedingt erforderlich.</span><span class="sxs-lookup"><span data-stu-id="3215d-231">Resource based authorization must be imperative.</span></span> <span data-ttu-id="3215d-232">Überprüfungen müssen ausgeführt werden, sobald wir Zugriff auf die Ressource haben durch Laden in unserer Controller oder durch Laden in den Handler selbst.</span><span class="sxs-lookup"><span data-stu-id="3215d-232">Checks must be performed once we have access to the resource, either by loading it in our controller, or by loading it within the handler itself.</span></span> <span data-ttu-id="3215d-233">Häufig werden Sie durch die Übergabe der Ressourcenschlüssel die Ressource zugreifen.</span><span class="sxs-lookup"><span data-stu-id="3215d-233">Frequently you will access the resource by passing in the resource key.</span></span>

<span data-ttu-id="3215d-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span><span class="sxs-lookup"><span data-stu-id="3215d-234">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]</span></span>

### <a name="update-the-delete-method"></a><span data-ttu-id="3215d-235">Aktualisieren Sie die Delete-Methode</span><span class="sxs-lookup"><span data-stu-id="3215d-235">Update the Delete method</span></span>

<span data-ttu-id="3215d-236">Aktualisieren Sie sowohl `Delete` Methoden, um den Handler für die Autorisierung verwenden, um zu überprüfen, ob den Benutzer der Besitzer des Kontakts ist.</span><span class="sxs-lookup"><span data-stu-id="3215d-236">Update both `Delete` methods to use the authorization handler to verify the user owns the contact.</span></span>

<span data-ttu-id="3215d-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span><span class="sxs-lookup"><span data-stu-id="3215d-237">[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]</span></span>

## <a name="inject-the-authorization-service-into-the-views"></a><span data-ttu-id="3215d-238">Fügen Sie dem Prüfdienst in den Ansichten</span><span class="sxs-lookup"><span data-stu-id="3215d-238">Inject the authorization service into the views</span></span>

<span data-ttu-id="3215d-239">Derzeit wird die Benutzeroberfläche zeigt bearbeiten und Löschen von Verknüpfungen für Daten, die der Benutzer ändern kann.</span><span class="sxs-lookup"><span data-stu-id="3215d-239">Currently the UI shows edit and delete links for data the user cannot modify.</span></span> <span data-ttu-id="3215d-240">Wir beheben, die durch Anwenden des Autorisierung Handlers mit der Ansichten.</span><span class="sxs-lookup"><span data-stu-id="3215d-240">We'll fix that by applying the authorization handler to the views.</span></span>

<span data-ttu-id="3215d-241">Fügen Sie dem Prüfdienst in der *Views/_ViewImports.cshtml* Datei, sodass es auf alle Sichten verfügbar sind:</span><span class="sxs-lookup"><span data-stu-id="3215d-241">Inject the authorization service in the *Views/_ViewImports.cshtml* file so it will be available to all views:</span></span>

<span data-ttu-id="3215d-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="3215d-242">[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]</span></span>

<span data-ttu-id="3215d-243">Update der *Views/Contacts/Index.cshtml* Razor-Ansicht nur anzeigen, die bearbeiten und Löschen von Verknüpfungen für Benutzer, die den Kontakt bearbeiten/löschen können.</span><span class="sxs-lookup"><span data-stu-id="3215d-243">Update the *Views/Contacts/Index.cshtml* Razor view to only display the edit and delete links for users who can edit/delete the contact.</span></span>

<span data-ttu-id="3215d-244">Hinzufügen`@using ContactManager.Authorization;`</span><span class="sxs-lookup"><span data-stu-id="3215d-244">Add `@using ContactManager.Authorization;`</span></span>

<span data-ttu-id="3215d-245">Update der `Edit` und `Delete` verknüpft, sodass sie nur für Benutzer mit Berechtigung zum Bearbeiten und löschen den Kontakt gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="3215d-245">Update the `Edit` and `Delete` links so they are only rendered for users with permission to edit and delete the contact.</span></span>

<span data-ttu-id="3215d-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span><span class="sxs-lookup"><span data-stu-id="3215d-246">[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]</span></span>

<span data-ttu-id="3215d-247">Warnung: Ausblenden von Links von Benutzern, die keine Berechtigung zum Bearbeiten oder löschen Sie Daten jedoch die app nicht gesichert.</span><span class="sxs-lookup"><span data-stu-id="3215d-247">Warning: Hiding links from users that do not have permission to edit or delete data does not secure the app.</span></span> <span data-ttu-id="3215d-248">Durch das Ausblenden Links wird der app mehrere Benutzer angezeigte durch Anzeigen der einzige gültige Links.</span><span class="sxs-lookup"><span data-stu-id="3215d-248">Hiding links makes the app more user friendly by displaying only valid links.</span></span> <span data-ttu-id="3215d-249">Benutzer können die generierten URLs zum Aufrufen bearbeiten und Löschen von Operationen mit Daten, die sie besitzen keine hack.</span><span class="sxs-lookup"><span data-stu-id="3215d-249">Users can hack the generated URLs to invoke edit and delete operations on data they don't own.</span></span>  <span data-ttu-id="3215d-250">Der Controller muss wiederholt werden, dass die zugriffsüberprüfungen schützen.</span><span class="sxs-lookup"><span data-stu-id="3215d-250">The controller must repeat the access checks to be secure.</span></span>

### <a name="update-the-details-view"></a><span data-ttu-id="3215d-251">Die Detailansicht aktualisieren</span><span class="sxs-lookup"><span data-stu-id="3215d-251">Update the Details view</span></span>

<span data-ttu-id="3215d-252">Aktualisieren Sie die Detailansicht, damit Manager genehmigen oder ablehnen von Kontakten können:</span><span class="sxs-lookup"><span data-stu-id="3215d-252">Update the details view so managers can approve or reject contacts:</span></span>

<span data-ttu-id="3215d-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span><span class="sxs-lookup"><span data-stu-id="3215d-253">[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]</span></span>

## <a name="test-the-completed-app"></a><span data-ttu-id="3215d-254">Abgeschlossene Anwendung testen</span><span class="sxs-lookup"><span data-stu-id="3215d-254">Test the completed app</span></span>

<span data-ttu-id="3215d-255">Wenn Sie mithilfe von Visual Studio-Code oder auf lokale Plattform, die ein Testzertifikat für SSL keine testen:</span><span class="sxs-lookup"><span data-stu-id="3215d-255">If you are using Visual Studio Code or testing on local platform that doesn't include a test certificate for SSL:</span></span>

- <span data-ttu-id="3215d-256">Legen Sie `"LocalTest:skipSSL": true` in der *appsettings.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="3215d-256">Set `"LocalTest:skipSSL": true` in the *appsettings.json* file.</span></span>

<span data-ttu-id="3215d-257">Wenn Sie die app ausgeführt haben und Kontakte, löschen Sie alle Datensätze in der `Contact` Tabelle, und starten Sie die app Ausgangswert für die Datenbank neu.</span><span class="sxs-lookup"><span data-stu-id="3215d-257">If you have run the app and have contacts, delete all the records in the `Contact` table and restart the app to seed the database.</span></span> <span data-ttu-id="3215d-258">Wenn Sie Visual Studio verwenden, müssen Sie zum Beenden und starten IIS Express Ausgangswert der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3215d-258">If you are using Visual Studio, you need to exit and restart IIS Express to seed the database.</span></span>

<span data-ttu-id="3215d-259">Registrieren Sie einen Benutzer, um die Kontakte zu durchsuchen.</span><span class="sxs-lookup"><span data-stu-id="3215d-259">Register a user to browse the contacts.</span></span>

<span data-ttu-id="3215d-260">Eine einfache Möglichkeit zum Testen Sie die abgeschlossenen app wird auf drei verschiedenen Browsern (oder inkognito/InPrivate-Versionen) zu starten.</span><span class="sxs-lookup"><span data-stu-id="3215d-260">An easy way to test the completed app is to launch three different browsers (or incognito/InPrivate versions).</span></span> <span data-ttu-id="3215d-261">In einem Browser eingeben, z. B. einen neuen Benutzer registrieren `test@example.com`.</span><span class="sxs-lookup"><span data-stu-id="3215d-261">In one browser, register a new user, for example, `test@example.com`.</span></span> <span data-ttu-id="3215d-262">Melden Sie sich mit einem anderen Benutzer jedem Browser an.</span><span class="sxs-lookup"><span data-stu-id="3215d-262">Sign in to each browser with a different user.</span></span> <span data-ttu-id="3215d-263">Überprüfen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="3215d-263">Verify the following:</span></span>

* <span data-ttu-id="3215d-264">Registrierte Benutzer können alle genehmigten Kontaktdaten anzeigen.</span><span class="sxs-lookup"><span data-stu-id="3215d-264">Registered users can view all the approved contact data.</span></span>
* <span data-ttu-id="3215d-265">Registrierte Benutzer können bearbeiten/ihre eigenen Daten löschen.</span><span class="sxs-lookup"><span data-stu-id="3215d-265">Registered users can edit/delete their own data.</span></span> 
* <span data-ttu-id="3215d-266">Manager können genehmigen oder ablehnen Kontaktdaten.</span><span class="sxs-lookup"><span data-stu-id="3215d-266">Managers can approve or reject contact data.</span></span> <span data-ttu-id="3215d-267">Die `Details` Ansicht zeigt **genehmigen** und **ablehnen** Schaltflächen.</span><span class="sxs-lookup"><span data-stu-id="3215d-267">The `Details` view shows **Approve** and **Reject** buttons.</span></span> 
* <span data-ttu-id="3215d-268">Administratoren können ablehnen/genehmigen und Bearbeiten/Löschen aller Daten.</span><span class="sxs-lookup"><span data-stu-id="3215d-268">Administrators can approve/reject and edit/delete any data.</span></span>

| <span data-ttu-id="3215d-269">Benutzer</span><span class="sxs-lookup"><span data-stu-id="3215d-269">User</span></span>| <span data-ttu-id="3215d-270">Optionen</span><span class="sxs-lookup"><span data-stu-id="3215d-270">Options</span></span> |
| ------------ | ---------|
| test@example.com | <span data-ttu-id="3215d-271">Können bearbeiten/eigene Daten löschen</span><span class="sxs-lookup"><span data-stu-id="3215d-271">Can edit/delete own data</span></span> |
| manager@contoso.com | <span data-ttu-id="3215d-272">Können ablehnen/genehmigen und bearbeiten/löschen, die Daten besitzen</span><span class="sxs-lookup"><span data-stu-id="3215d-272">Can approve/reject and edit/delete own data</span></span>  |
| admin@contoso.com | <span data-ttu-id="3215d-273">Können bearbeiten/löschen und alle Daten genehmigen/ablehnen</span><span class="sxs-lookup"><span data-stu-id="3215d-273">Can edit/delete and approve/reject all data</span></span>|

<span data-ttu-id="3215d-274">Erstellen Sie einen Kontakt im Browser Administratoren.</span><span class="sxs-lookup"><span data-stu-id="3215d-274">Create a contact in the administrators browser.</span></span> <span data-ttu-id="3215d-275">Kopieren Sie die URL zum Löschen und Bearbeiten von den Administrator wenden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="3215d-275">Copy the URL for delete and edit from the administrator contact.</span></span> <span data-ttu-id="3215d-276">Fügen Sie diese Links in den Browser des Testbenutzers zu überprüfen, ob der Testbenutzer diese Vorgänge ausführen kann nicht aus.</span><span class="sxs-lookup"><span data-stu-id="3215d-276">Paste these links into the test user's browser to verify the test user cannot perform these operations.</span></span>

## <a name="create-the-starter-app"></a><span data-ttu-id="3215d-277">Erstellen der Starter-app</span><span class="sxs-lookup"><span data-stu-id="3215d-277">Create the starter app</span></span>

<span data-ttu-id="3215d-278">Befolgen Sie diese Anweisungen zum Erstellen der Starter-app aus.</span><span class="sxs-lookup"><span data-stu-id="3215d-278">Follow these instructions to create the starter app.</span></span>

* <span data-ttu-id="3215d-279">Erstellen einer **ASP.NET-Webanwendung für Core** mit [Visual Studio 2017](https://www.visualstudio.com/) mit dem Namen "ContactManager"</span><span class="sxs-lookup"><span data-stu-id="3215d-279">Create an **ASP.NET Core Web Application** using [Visual Studio 2017](https://www.visualstudio.com/) named "ContactManager"</span></span>

  * <span data-ttu-id="3215d-280">Erstellen Sie die app mit **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="3215d-280">Create the app with **Individual User Accounts**.</span></span>
  * <span data-ttu-id="3215d-281">Nennen Sie sie "ContactManager", damit Ihr Namespace den Namespace verwenden, in dem Beispiel entspricht.</span><span class="sxs-lookup"><span data-stu-id="3215d-281">Name it "ContactManager" so your namespace will match the namespace use in the sample.</span></span>

* <span data-ttu-id="3215d-282">Fügen Sie die folgenden `Contact` Modell:</span><span class="sxs-lookup"><span data-stu-id="3215d-282">Add the following `Contact` model:</span></span>

  <span data-ttu-id="3215d-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="3215d-283">[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]</span></span>

* <span data-ttu-id="3215d-284">Gerüst der `Contact` -Modells mithilfe von Entity Framework Core und die `ApplicationDbContext` -Datenkontext.</span><span class="sxs-lookup"><span data-stu-id="3215d-284">Scaffold the `Contact` model using Entity Framework Core and the `ApplicationDbContext` data context.</span></span> <span data-ttu-id="3215d-285">Übernehmen Sie alle Standardeinstellungen der Gerüstbau.</span><span class="sxs-lookup"><span data-stu-id="3215d-285">Accept all the scaffolding defaults.</span></span> <span data-ttu-id="3215d-286">Mit `ApplicationDbContext` Klasse fügt für den Datenkontext der Contact-Tabelle in der [Identität](xref:security/authentication/identity) Datenbank.</span><span class="sxs-lookup"><span data-stu-id="3215d-286">Using `ApplicationDbContext` for the data context class  puts the contact table in the [Identity](xref:security/authentication/identity) database.</span></span> <span data-ttu-id="3215d-287">Finden Sie unter [Hinzufügen eines Modells](xref:tutorials/first-mvc-app/adding-model) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="3215d-287">See [Adding a model](xref:tutorials/first-mvc-app/adding-model) for more information.</span></span>

* <span data-ttu-id="3215d-288">Update der **ContactManager** Verankern der *Views/Shared/_Layout.cshtml* aus der Datei `asp-controller="Home"` auf `asp-controller="Contacts"` tippen also die **ContactManager** Link Kontakte-Controllers wird aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="3215d-288">Update the **ContactManager** anchor in the *Views/Shared/_Layout.cshtml* file from `asp-controller="Home"` to `asp-controller="Contacts"` so tapping the **ContactManager** link will invoke the Contacts controller.</span></span> <span data-ttu-id="3215d-289">Das ursprüngliche Markup:</span><span class="sxs-lookup"><span data-stu-id="3215d-289">The original markup:</span></span>

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

<span data-ttu-id="3215d-290">Der aktualisierte Markupcode aus:</span><span class="sxs-lookup"><span data-stu-id="3215d-290">The updated markup:</span></span>

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* <span data-ttu-id="3215d-291">Erstellen der anfänglichen Migrations und Aktualisierung der Datenbank</span><span class="sxs-lookup"><span data-stu-id="3215d-291">Scaffold the initial migration and update the database</span></span>

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* <span data-ttu-id="3215d-292">Testen der app durch erstellen, bearbeiten und Löschen eines Kontakts</span><span class="sxs-lookup"><span data-stu-id="3215d-292">Test the app by creating, editing and deleting a contact</span></span>

### <a name="seed-the-database"></a><span data-ttu-id="3215d-293">Ausgangswert für die Datenbank</span><span class="sxs-lookup"><span data-stu-id="3215d-293">Seed the database</span></span>

<span data-ttu-id="3215d-294">Hinzufügen der `SeedData` Klasse, um die *Daten* Ordner.</span><span class="sxs-lookup"><span data-stu-id="3215d-294">Add the `SeedData` class to the *Data* folder.</span></span> <span data-ttu-id="3215d-295">Wenn Sie das Beispiel heruntergeladen haben, können Sie kopieren die *SeedData.cs* Datei wird in der *Daten* Ordner des Startprojekts.</span><span class="sxs-lookup"><span data-stu-id="3215d-295">If you've downloaded the sample, you can copy the *SeedData.cs* file to the *Data* folder of the starter project.</span></span>

<span data-ttu-id="3215d-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-296">[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]</span></span>

<span data-ttu-id="3215d-297">Fügen Sie den hervorgehobenen Code am Ende der `Configure` Methode in der *Startup.cs* Datei:</span><span class="sxs-lookup"><span data-stu-id="3215d-297">Add the highlighted code to the end of the `Configure` method in the *Startup.cs* file:</span></span>

<span data-ttu-id="3215d-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span><span class="sxs-lookup"><span data-stu-id="3215d-298">[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]</span></span>

<span data-ttu-id="3215d-299">Testen Sie, ob die Anwendung die Datenbank mit Anfangsdaten gefüllt.</span><span class="sxs-lookup"><span data-stu-id="3215d-299">Test that the app seeded the database.</span></span> <span data-ttu-id="3215d-300">Die Seed-Methode wird nicht ausgeführt, wenn alle Zeilen in den Kontakt DB vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="3215d-300">The seed method does not run if there are any rows in the contact DB.</span></span>

### <a name="create-a-class-used-in-the-tutorial"></a><span data-ttu-id="3215d-301">Erstellen Sie eine Klasse, die im Lernprogramm verwendet</span><span class="sxs-lookup"><span data-stu-id="3215d-301">Create a class used in the tutorial</span></span>

* <span data-ttu-id="3215d-302">Erstellen Sie einen Ordner mit dem Namen *Autorisierung*.</span><span class="sxs-lookup"><span data-stu-id="3215d-302">Create a folder named *Authorization*.</span></span>
* <span data-ttu-id="3215d-303">Kopieren der *Authorization\ContactOperations.cs* aus dem Download des abgeschlossenen Projekts-Datei ein, oder kopieren Sie den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="3215d-303">Copy the *Authorization\ContactOperations.cs* file from the completed project download, or copy the following code:</span></span>

<span data-ttu-id="3215d-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span><span class="sxs-lookup"><span data-stu-id="3215d-304">[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]</span></span>

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a><span data-ttu-id="3215d-305">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="3215d-305">Additional resources</span></span>

* <span data-ttu-id="3215d-306">[ASP.NET Core Autorisierung Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span><span class="sxs-lookup"><span data-stu-id="3215d-306">[ASP.NET Core Authorization Lab](https://github.com/blowdart/AspNetAuthorizationWorkshop).</span></span> <span data-ttu-id="3215d-307">Diese Übung wird ausführlicher auf den Sicherheitsfeatures, die in diesem Lernprogramm eingeführt.</span><span class="sxs-lookup"><span data-stu-id="3215d-307">This lab goes into more detail on the security features introduced in this tutorial.</span></span>
* [<span data-ttu-id="3215d-308">Autorisierung in ASP.NET Core: einfach, anspruchsbasierte und benutzerdefinierten Rolle</span><span class="sxs-lookup"><span data-stu-id="3215d-308">Authorization in ASP.NET Core : Simple, role, claims-based and custom</span></span>](index.md)
* [<span data-ttu-id="3215d-309">Benutzerdefinierte, richtlinienbasierende Autorisierung</span><span class="sxs-lookup"><span data-stu-id="3215d-309">Custom Policy-Based Authorization</span></span>](policies.md)
