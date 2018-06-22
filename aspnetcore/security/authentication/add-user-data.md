---
title: Hinzufügen, herunterladen und Löschen von benutzerdefinierten Benutzerdaten Identität in einem Projekt auf ASP.NET Core
author: rick-anderson
description: Weitere Informationen Sie zum Hinzufügen von benutzerdefinierten Daten zu Identität in einem Projekt auf ASP.NET Core. Löschen von Daten pro GDPR.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
uid: security/authentication/add-user-data
ms.openlocfilehash: ecd0e6d1c71b24309fab70fbb06af7731463bb0e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271956"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="c14e8-104">Hinzufügen, herunterladen und Löschen von benutzerdefinierten Benutzerdaten Identität in einem Projekt auf ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c14e8-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="c14e8-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c14e8-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c14e8-106">Dieser Artikel zeigt, wie Sie:</span><span class="sxs-lookup"><span data-stu-id="c14e8-106">This article shows how to:</span></span>

* <span data-ttu-id="c14e8-107">Fügen Sie benutzerdefinierte Benutzerdaten an eine ASP.NET Core-Web-app.</span><span class="sxs-lookup"><span data-stu-id="c14e8-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="c14e8-108">Ergänzen Sie die benutzerdefinierten Datenmodell mit der [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) Attribut, damit er automatisch für herunterladen und Löschen verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="c14e8-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="c14e8-109">Machen die Daten heruntergeladen und gelöscht werden kann hilft bei der Erfüllung [GDPR](xref:security/gdpr) Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="c14e8-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="c14e8-110">Project-Beispiels aus einer Razor-Seiten-Web-app erstellt, aber die Anweisungen ähneln sich für eine ASP.NET Core MVC-Web-app.</span><span class="sxs-lookup"><span data-stu-id="c14e8-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="c14e8-111">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c14e8-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c14e8-112">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="c14e8-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="c14e8-113">Erstellen einer Razor-Web-App</span><span class="sxs-lookup"><span data-stu-id="c14e8-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c14e8-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c14e8-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c14e8-115">Klicken Sie in Visual Studio im Menü **Datei** auf **Neu** > **Projekt**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="c14e8-116">Nennen Sie das Projekt **WebApp1** Wenn Sie möchten einen übereinstimmenden Namespace der [Beispiel herunterladen](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) Code.</span><span class="sxs-lookup"><span data-stu-id="c14e8-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="c14e8-117">Wählen Sie **Webanwendung ASP.NET Core** > **OK**</span><span class="sxs-lookup"><span data-stu-id="c14e8-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="c14e8-118">Wählen Sie **ASP.NET Core 2.1** in der Dropdownliste</span><span class="sxs-lookup"><span data-stu-id="c14e8-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="c14e8-119">Wählen Sie **-Webanwendung**  > **OK**</span><span class="sxs-lookup"><span data-stu-id="c14e8-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="c14e8-120">Erstellen Sie das Projekt, und führen Sie es aus.</span><span class="sxs-lookup"><span data-stu-id="c14e8-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c14e8-121">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="c14e8-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="c14e8-122">Führen Sie die Identität scaffolder</span><span class="sxs-lookup"><span data-stu-id="c14e8-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c14e8-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c14e8-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c14e8-124">Von **Projektmappen-Explorer**, mit der rechten Maustaste auf das Projekt > **hinzufügen** > **neues Gerüstelement**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c14e8-125">Im linken Bereich des der **Gerüst hinzufügen** wählen Sie im Dialogfeld **Identität** > **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="c14e8-126">In der **ADD Identität** Dialog, der die folgenden Optionen:</span><span class="sxs-lookup"><span data-stu-id="c14e8-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="c14e8-127">Wählen Sie Ihre vorhandenen Layoutdatei *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c14e8-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="c14e8-128">Wählen Sie die folgenden Dateien überschreiben:</span><span class="sxs-lookup"><span data-stu-id="c14e8-128">Select the following files to override:</span></span>
    * <span data-ttu-id="c14e8-129">**Konto/registrieren**</span><span class="sxs-lookup"><span data-stu-id="c14e8-129">**Account/Register**</span></span>
    * <span data-ttu-id="c14e8-130">**Konto / / Index verwalten**</span><span class="sxs-lookup"><span data-stu-id="c14e8-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="c14e8-131">Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Datenkontextklasse**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="c14e8-132">Akzeptieren Sie den Typ (**WebApp1.Models.WebApp1Context** , wenn Sie das Projekt mit dem Namen **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="c14e8-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="c14e8-133">Wählen Sie die **+** Schaltfläche zum Erstellen eines neuen **Benutzerklasse**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="c14e8-134">Akzeptieren Sie den Typ (**WebApp1User** , wenn Sie das Projekt mit dem Namen **WebApp1**) > **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="c14e8-135">Wählen Sie **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="c14e8-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c14e8-136">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="c14e8-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c14e8-137">Wenn Sie die ASP.NET Scaffolder noch nicht installiert haben, installieren Sie es jetzt ein:</span><span class="sxs-lookup"><span data-stu-id="c14e8-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c14e8-138">Hinzufügen eines Verweises Paket auf [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) der Projektdatei (csproj).</span><span class="sxs-lookup"><span data-stu-id="c14e8-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="c14e8-139">Führen Sie den folgenden Befehl in das Projektverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="c14e8-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="c14e8-140">Führen Sie den folgenden Befehl zum Auflisten von Optionen Scaffolder Identität:</span><span class="sxs-lookup"><span data-stu-id="c14e8-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="c14e8-141">Führen Sie in den Projektordner der Identität Scaffolder aus:</span><span class="sxs-lookup"><span data-stu-id="c14e8-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="c14e8-142">Die Anweisungen im [Migrationen, UseAuthentication und Layout](xref:security/authentication/scaffold-identity#efm) die folgenden Schritte ausführen:</span><span class="sxs-lookup"><span data-stu-id="c14e8-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="c14e8-143">Erstellen Sie eine Migration aus, und Aktualisieren der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="c14e8-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="c14e8-144">Fügen Sie `UseAuthentication` zu `Startup.Configure` hinzu.</span><span class="sxs-lookup"><span data-stu-id="c14e8-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="c14e8-145">Hinzufügen `<partial name="_LoginPartial" />` der Layout-Datei.</span><span class="sxs-lookup"><span data-stu-id="c14e8-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="c14e8-146">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="c14e8-146">Test the app:</span></span>
  * <span data-ttu-id="c14e8-147">Registrieren eines Benutzers</span><span class="sxs-lookup"><span data-stu-id="c14e8-147">Register a user</span></span>
  * <span data-ttu-id="c14e8-148">Wählen Sie den neuen Benutzernamen ein (neben der **Logout** Link).</span><span class="sxs-lookup"><span data-stu-id="c14e8-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="c14e8-149">Sie müssen möglicherweise erweitern das Fenster, oder wählen das Navigationssymbol Balken, um den Benutzernamen und anderen Links anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="c14e8-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="c14e8-150">Wählen Sie die **persönliche Daten** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="c14e8-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="c14e8-151">Wählen Sie die **herunterladen** Schaltfläche und untersucht die *PersonalData.json* Datei.</span><span class="sxs-lookup"><span data-stu-id="c14e8-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="c14e8-152">Testen der **löschen** Schaltfläche, die der angemeldete Benutzer löscht.</span><span class="sxs-lookup"><span data-stu-id="c14e8-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="c14e8-153">Hinzufügen von benutzerdefinierten Benutzerdaten mit der Identity-Datenbank</span><span class="sxs-lookup"><span data-stu-id="c14e8-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="c14e8-154">Update der `IdentityUser` abgeleitete Klasse mit benutzerdefinierten Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="c14e8-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="c14e8-155">Wenn Sie Ihr Projekt WebApp1 genannt, kann die Datei heißt *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="c14e8-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="c14e8-156">Aktualisieren Sie die Datei mit den folgenden Code ein:</span><span class="sxs-lookup"><span data-stu-id="c14e8-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="c14e8-157">Eigenschaften mit ergänzt die [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) Attribut sind:</span><span class="sxs-lookup"><span data-stu-id="c14e8-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="c14e8-158">Gelöscht, sobald die *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor-Seite ruft `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="c14e8-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="c14e8-159">Enthalten in der heruntergeladenen Daten durch die *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor-Seite.</span><span class="sxs-lookup"><span data-stu-id="c14e8-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="c14e8-160">Die Seite Account/Manage/Index.cshtml "Aktualisieren"</span><span class="sxs-lookup"><span data-stu-id="c14e8-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="c14e8-161">Update der `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="c14e8-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="c14e8-162">Update der *Areas/Identity/Pages/Account/Manage/Index.cshtml* mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="c14e8-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="c14e8-163">Die Seite Account/Register.cshtml "Aktualisieren"</span><span class="sxs-lookup"><span data-stu-id="c14e8-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="c14e8-164">Update der `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* mit den folgenden hervorgehobenen Code:</span><span class="sxs-lookup"><span data-stu-id="c14e8-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="c14e8-165">Update der *Areas/Identity/Pages/Account/Register.cshtml* mit dem folgenden hervorgehobenen Markup:</span><span class="sxs-lookup"><span data-stu-id="c14e8-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="c14e8-166">Erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="c14e8-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="c14e8-167">Fügen Sie eine Migration für die benutzerdefinierte Daten hinzu</span><span class="sxs-lookup"><span data-stu-id="c14e8-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c14e8-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c14e8-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c14e8-169">In Visual Studio **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="c14e8-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c14e8-170">.NET Core-CLI</span><span class="sxs-lookup"><span data-stu-id="c14e8-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="c14e8-171">Test erstellen, anzeigen, herunterladen, Löschen von benutzerdefinierten Daten</span><span class="sxs-lookup"><span data-stu-id="c14e8-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="c14e8-172">Testen der App:</span><span class="sxs-lookup"><span data-stu-id="c14e8-172">Test the app:</span></span>

* <span data-ttu-id="c14e8-173">Registrieren Sie einen neuen Benutzer.</span><span class="sxs-lookup"><span data-stu-id="c14e8-173">Register a new user.</span></span>
* <span data-ttu-id="c14e8-174">Zeigen Sie die benutzerdefinierten Benutzerdaten auf der `/Identity/Account/Manage` Seite.</span><span class="sxs-lookup"><span data-stu-id="c14e8-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="c14e8-175">Herunterladen und zeigen Sie die Benutzer persönliche Daten aus der `/Identity/Account/Manage/PersonalData` Seite.</span><span class="sxs-lookup"><span data-stu-id="c14e8-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
