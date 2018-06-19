---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3 Application with Razor and unaufdringliches JavaScript | Microsoft Docs
author: microsoft
description: Die Benutzerliste Beispielwebanwendung veranschaulicht, wie einfach es ist zum Erstellen von ASP.NET MVC 3-Anwendungen, die das Razor-Anzeigemodul verwenden. Die Beispiel-Anwendung-s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 9b273f6827cad2078b581d6da7b127198dfddaa5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
ms.locfileid: "30874693"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="f5f0d-104">Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript</span><span class="sxs-lookup"><span data-stu-id="f5f0d-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="f5f0d-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f5f0d-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f5f0d-106">Die Benutzerliste Beispielwebanwendung veranschaulicht, wie einfach es ist zum Erstellen von ASP.NET MVC 3-Anwendungen, die das Razor-Anzeigemodul verwenden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="f5f0d-107">Die beispielanwendung zeigt, wie die neue Razor-Ansichtsmodul mit ASP.NET MVC 3-Version und die Visual Studio 2010 zum Erstellen einer fiktiven Benutzerliste-Websites, die Funktionen, z. B. erstellen, anzeigen, bearbeiten und Löschen von Benutzern enthält.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="f5f0d-108">In diesem Lernprogramm wird beschrieben, die ausgeführt wurden, um die Benutzerliste Beispiel ASP.NET MVC 3-Anwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="f5f0d-109">Visual Studio-Projekt mit c# und VB-Quellcode ist zu diesem Thema steht zur Verfügung: [herunterladen](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="f5f0d-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="f5f0d-110">Wenn Sie Fragen zu diesem Lernprogramm haben, Supportfragen zu den [MVC-Forum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="f5f0d-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="f5f0d-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f5f0d-111">Overview</span></span>

<span data-ttu-id="f5f0d-112">Die Anwendung, die Sie erstellen werden müssen ist eine einfache Liste-Website.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="f5f0d-113">Benutzer können eingeben, anzeigen und Aktualisieren von Benutzerinformationen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-113">Users can enter, view, and update user information.</span></span>

![Beispiel-site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="f5f0d-115">Sie können das abgeschlossene Projekt VB- und C#- [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="f5f0d-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="f5f0d-116">Erstellen der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="f5f0d-116">Creating the Web Application</span></span>

<span data-ttu-id="f5f0d-117">Um das Lernprogramm zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mit der *ASP.NET MVC 3-Webanwendung* Vorlage.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="f5f0d-118">Benennen Sie die Anwendung &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="f5f0d-119">[![Neues MVC 3-Projekt](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="f5f0d-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="f5f0d-120">In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**, wählen Sie das Razor-Ansichtsmodul, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Dialogfeld "neues ASP.NET MVC 3-Projekt"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="f5f0d-122">In diesem Lernprogramm werden Sie nicht den ASP.NET-Mitgliedschaftsanbieter verwenden können Sie die Dateien, die für die zugeordnete Anmeldung und Mitgliedschaft löschen können.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="f5f0d-123">In **Projektmappen-Explorer**, entfernen Sie die folgenden Dateien und Verzeichnisse:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="f5f0d-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="f5f0d-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="f5f0d-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="f5f0d-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="f5f0d-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="f5f0d-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="f5f0d-127">*Views\Account* (und alle Dateien in diesem Verzeichnis)</span><span class="sxs-lookup"><span data-stu-id="f5f0d-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln Exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="f5f0d-129">Bearbeiten der <em> \_Layout.cshtml</em> -Datei und Ersetzen Sie das Markup der `<div>` Element mit dem Namen `logindisplay` mit der Meldung <em> &quot; </em>Anmeldung deaktiviert&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="f5f0d-130">Das folgende Beispiel zeigt das neue Markup:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="f5f0d-131">Hinzufügen des Modells</span><span class="sxs-lookup"><span data-stu-id="f5f0d-131">Adding the Model</span></span>

<span data-ttu-id="f5f0d-132">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner wählen **hinzufügen**, und klicken Sie dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Neue Benutzer Mdl-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="f5f0d-134">Nennen Sie die Klasse `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-134">Name the class `UserModel`.</span></span> <span data-ttu-id="f5f0d-135">Ersetzen Sie den Inhalt von der *UserModel* Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="f5f0d-136">Die `UserModel` Klasse stellt Benutzer dar.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="f5f0d-137">Jeder Member der Klasse mit dem versehen ist die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut aus dem [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="f5f0d-138">Die Attribute in der [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace stellen automatische Client- und serverseitige Validierung für Webanwendungen bereit.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="f5f0d-139">Öffnen der `HomeController` -Klasse und das Hinzufügen einer `using` Direktive, damit Sie Zugriff haben die `UserModel` und `Users` Klassen:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="f5f0d-140">Direkt nach der `HomeController` Deklaration, fügen Sie den folgenden Kommentar und der Verweis auf eine `Users` Klasse:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="f5f0d-141">Die `Users` Klasse ist ein vereinfachtes, in-Memory-Datenspeicher, die Sie in diesem Lernprogramm verwenden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="f5f0d-142">In einer echten Anwendung würden Sie eine Datenbank zum Speichern von Benutzerinformationen verwenden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="f5f0d-143">Die ersten Zeilen von der `HomeController` Datei werden im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="f5f0d-144">Erstellen Sie die Anwendung aus, sodass Benutzermodell für den Gerüstbau-Assistenten im nächsten Schritt verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="f5f0d-145">Erstellen die Standardansicht</span><span class="sxs-lookup"><span data-stu-id="f5f0d-145">Creating the Default View</span></span>

<span data-ttu-id="f5f0d-146">Der nächste Schritt ist zum Hinzufügen einer Aktionsmethode und können die Benutzer anzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="f5f0d-147">Löschen Sie die vorhandene *Views\Home\Index* Datei.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="f5f0d-148">Erstellen Sie ein neues *Index* Datei, die die Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="f5f0d-149">In der `HomeController` Klasse, ersetzen Sie den Inhalt von der `Index` -Methode durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="f5f0d-150">Mit der rechten Maustaste innerhalb der `Index` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Fügen Sie die Ansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="f5f0d-152">Wählen Sie die **eine stark typisierte Ansicht erstellen** Option.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="f5f0d-153">Für **Datenklasse anzeigen**Option **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="f5f0d-154">(Wenn Sie nicht sehen **Mvc3Razor.Models.UserModel** in der **Datenklasse anzeigen** Feld müssen Sie das Projekt zu erstellen.) Stellen Sie sicher, dass das Ansichtsmodul, um festgelegt ist **Razor**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="f5f0d-155">Legen Sie **Inhalt anzeigen** auf **Liste** , und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-155">Set **View content** to **List** and then click **Add**.</span></span>

![Indexansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="f5f0d-157">Die neue Ansicht scaffolds automatisch die Benutzerdaten, die an die `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="f5f0d-158">Überprüfen Sie den neu generierten *Views\Home\Index* Datei.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="f5f0d-159">Die **neu erstellen**, **bearbeiten**, **Details**, und **löschen** Links funktionieren nicht, aber der Rest der Seite ist funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="f5f0d-160">Führen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-160">Run the page.</span></span> <span data-ttu-id="f5f0d-161">Sie sehen eine Liste von Benutzern.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-161">You see a list of users.</span></span>

![Indexseite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="f5f0d-163">Öffnen der *Index.cshtml* -Datei und ersetzen die `ActionLink` Markup für **bearbeiten**, **Details**, und **löschen** durch den folgenden Code :</span><span class="sxs-lookup"><span data-stu-id="f5f0d-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="f5f0d-164">Der Benutzername wird als ID verwendet, zu dem ausgewählten Datensatz in der **bearbeiten**, **Details**, und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="f5f0d-165">Erstellen die Detailansicht</span><span class="sxs-lookup"><span data-stu-id="f5f0d-165">Creating the Details View</span></span>

<span data-ttu-id="f5f0d-166">Der nächste Schritt besteht, zum Hinzufügen einer `Details` Aktionsmethode und Ansicht um Benutzerdetails anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="f5f0d-168">Fügen Sie die folgenden `Details` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="f5f0d-169">Mit der rechten Maustaste innerhalb der `Details` -Methode, und wählen Sie dann <strong>Ansicht hinzufügen</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="f5f0d-170">Überprüfen Sie, ob die <strong>Datenklasse anzeigen</strong> enthält <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="f5f0d-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="f5f0d-171">Legen Sie <strong>Inhalt anzeigen</strong> auf <strong>Details</strong> , und klicken Sie dann auf <strong>hinzufügen</strong>.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Hinzufügen der Detailansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="f5f0d-173">Führen Sie die Anwendung, und wählen Sie einen Link Details.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-173">Run the application and select a details link.</span></span> <span data-ttu-id="f5f0d-174">Der automatische Gerüstbau zeigt jede Eigenschaft im Modell an.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-174">The automatic scaffolding shows each property in the model.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="f5f0d-176">Erstellen die Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="f5f0d-176">Creating the Edit View</span></span>

<span data-ttu-id="f5f0d-177">Fügen Sie die folgenden `Edit` Methode, um den home-Controller.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="f5f0d-178">Fügen Sie eine Ansicht aus, wie in den vorherigen Schritten jedoch festgelegt **Inhalt anzeigen** auf **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Hinzufügen der Bearbeitungsansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="f5f0d-180">Führen Sie die Anwendung, und bearbeiten Sie den ersten und letzten Namen von einem Benutzer.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="f5f0d-181">Wenn Sie eine verletzen `DataAnnotation` Einschränkungen, die auf angewendet wurden die `UserModel` -Klasse, bei der Übermittlung des Formulars sehen Sie Validierungsfehler, die in Servercode erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="f5f0d-182">Angenommen, Sie ändern, dass der Vorname &quot;Ann&quot; auf &quot;ein&quot;, bei der Übermittlung des Formulars die folgende Fehlermeldung wird angezeigt, auf dem Formular:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="f5f0d-183">In diesem Lernprogramm haben Sie den Benutzernamen als primären Schlüssel behandelt.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="f5f0d-184">Aus diesem Grund kann die Name-Eigenschaft für Benutzer nicht geändert werden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="f5f0d-185">In der *Edit.cshtml* Datei, direkt nach der `Html.BeginForm` -Anweisung, legen Sie den Benutzernamen ein, ein ausgeblendetes Feld sein.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="f5f0d-186">Dies bewirkt, dass die Eigenschaft im Modell übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="f5f0d-187">Das folgende Codefragment zeigt die Platzierung der `Hidden` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="f5f0d-188">Ersetzen Sie die `TextBoxFor` und `ValidationMessageFor` Markup für den Benutzernamen mit einer `DisplayFor` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="f5f0d-189">Die `DisplayFor` Methode zeigt die Eigenschaft als nur-Lese Element.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="f5f0d-190">Das folgende Beispiel zeigt das vollständige Markup.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-190">The following example shows the completed markup.</span></span> <span data-ttu-id="f5f0d-191">Die ursprüngliche `TextBoxFor` und `ValidationMessageFor` Aufrufe mit Razor beginnen Kommentar und End-Kommentar Zeichen auskommentiert werden (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="f5f0d-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="f5f0d-192">Die clientseitige Validierung aktivieren</span><span class="sxs-lookup"><span data-stu-id="f5f0d-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="f5f0d-193">Um die clientseitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen und Sie müssen drei JavaScript-Dateien.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="f5f0d-194">Öffnen Sie die Anwendung *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="f5f0d-195">Vergewissern Sie sich `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` festgelegt sind, auf "true" in den Anwendungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="f5f0d-196">Das folgende Fragment aus dem Stammverzeichnis *"Web.config"* -Datei zeigt die richtigen Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="f5f0d-197">Festlegen von `UnobtrusiveJavaScriptEnabled` auf aktiviert unaufdringliches Ajax und unaufdringliche Clientvalidierung "true".</span><span class="sxs-lookup"><span data-stu-id="f5f0d-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="f5f0d-198">Bei Verwendung von unaufdringlichen Überprüfung werden die Gültigkeitsprüfungsregeln verwendet in HTML5-Attribute aktiviert.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="f5f0d-199">HTML5-Attributnamen können nur Kleinbuchstaben, Zahlen und Bindestrichen bestehen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="f5f0d-200">Festlegen von `ClientValidationEnabled` auf "true" ermöglicht die clientseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="f5f0d-201">Durch Festlegen dieser Schlüssel in der Anwendung *"Web.config"* Datei, Sie aktivieren die Clientvalidierung und unaufdringliches JavaScript für die gesamte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="f5f0d-202">Sie können auch aktivieren oder deaktivieren Sie diese Einstellungen in einzelne Sichten oder Controllermethoden, die mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="f5f0d-203">Sie müssen auch einige JavaScript-Dateien in der gerenderten Ansicht enthalten.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="f5f0d-204">Eine einfache Möglichkeit, die in allen Ansichten der JavaScript-Code enthalten ist, zum Hinzufügen der *Views\Shared\\_Layout.cshtml* Datei.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="f5f0d-205">Ersetzen Sie die `<head>` Element von der * \_Layout.cshtml* Datei durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="f5f0d-206">Die ersten beiden jQuery-Skripts, die von der Microsoft Ajax Content Delivery Network (CDN) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="f5f0d-207">Durch das Microsoft Ajax-CDN nutzen, können Sie die erste Treffer Leistung Ihrer Anwendungen bedeutend verbessern.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="f5f0d-208">Führen Sie die Anwendung, und klicken Sie auf einen Bearbeitungslink.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-208">Run the application and click an edit link.</span></span> <span data-ttu-id="f5f0d-209">Zeigen Sie den Quellcode der Seite im Browser.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-209">View the page's source in the browser.</span></span> <span data-ttu-id="f5f0d-210">Die Browserquelle zeigt viele Attribute des Formulars `data-val` (für die datenüberprüfung).</span><span class="sxs-lookup"><span data-stu-id="f5f0d-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="f5f0d-211">Wenn die Clientvalidierung und unaufdringliches JavaScript aktiviert ist, Eingabefelder mit einer Client-Validierungsregel enthalten die `data-val="true"` Attribut unaufdringliche Clientvalidierung auslösen.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="f5f0d-212">Z. B. die `City` Feld im Modell wurde mit ergänzt die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut, das Ergebnisse im HTML-Code im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="f5f0d-213">Für jede Regel Clientvalidierung Element wird, die das Formular enthält ein Attribut hinzugefügt `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="f5f0d-214">Mithilfe der `City` erforderlichen Clientvalidierung diese Regel generiert eine zuvor gezeigten Beispiel der `data-val-required` Attribut und der Meldung &quot;der Ort ist ein Pflichtfeld&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="f5f0d-215">Führen Sie die Anwendung, ein Benutzer bearbeiten und löschen Sie die `City` Feld.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="f5f0d-216">Wenn Sie aus dem Feld tab, sehen Sie eine clientseitige Validierungsfehlermeldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Ort erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="f5f0d-218">Auf ähnliche Weise für jeden Parameter in der Clientvalidierung Regel wird ein Attribut hinzugefügt, die das Formular enthält `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="f5f0d-219">Z. B. die `FirstName` Eigenschaft mit dem versehen ist die [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) -Attribut und gibt eine minimale Länge von 3 und einer maximalen Länge von 8.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="f5f0d-220">Der mit dem Namen Gültigkeitsprüfungsregel `length` wurde der Name des Parameters `max` und der Wert des Parameters 8.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="f5f0d-221">Das folgende Beispiel zeigt den HTML-Code, der generiert wird, für die `FirstName` Feld, wenn Sie ein Benutzer bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="f5f0d-222">Weitere Informationen zu unaufdringliche Clientvalidierung, finden Sie im Eintrag [unaufdringliche Clientvalidierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilsons Blog.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="f5f0d-223">In ASP.NET MVC 3 Beta müssen Sie manchmal zum Senden des Formulars, um die clientseitige Überprüfung zu starten.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="f5f0d-224">Dies kann für die endgültige Version geändert werden.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="f5f0d-225">Erstellen die Erstellungsansicht</span><span class="sxs-lookup"><span data-stu-id="f5f0d-225">Creating the Create View</span></span>

<span data-ttu-id="f5f0d-226">Der nächste Schritt besteht, zum Hinzufügen einer `Create` Aktionsmethode und anzeigen, damit den Benutzer einen neuen Benutzer erstellen können.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="f5f0d-227">Fügen Sie die folgenden `Create` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="f5f0d-228">Fügen Sie eine Ansicht aus, wie in den vorherigen Schritten jedoch festgelegt **Inhalt anzeigen** auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="f5f0d-230">Die Anwendung auszuführen, wählen Sie die **erstellen** verknüpfen, und fügen Sie einen neuen Benutzer hinzu.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="f5f0d-231">Die `Create` Methode automatisch nutzt die clientseitige und serverseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="f5f0d-232">Bei dem Versuch, einen Benutzernamen eingeben, der Leerzeichen, z. B. enthält &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="f5f0d-233">Wenn Sie die Registerkarte Out Feld für den Benutzernamen, die clientseitige Überprüfung ein Fehler (`White space is not allowed`) wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="f5f0d-234">Fügen Sie die Delete-Methode</span><span class="sxs-lookup"><span data-stu-id="f5f0d-234">Add the Delete method</span></span>

<span data-ttu-id="f5f0d-235">Um das Lernprogramm abgeschlossen haben, fügen Sie die folgenden `Delete` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="f5f0d-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="f5f0d-236">Hinzufügen einer `Delete` anzeigen, wie in den vorherigen Schritten festlegen **Inhalt anzeigen** auf **löschen**.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="f5f0d-238">Sie haben nun eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit der Validierung.</span><span class="sxs-lookup"><span data-stu-id="f5f0d-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
