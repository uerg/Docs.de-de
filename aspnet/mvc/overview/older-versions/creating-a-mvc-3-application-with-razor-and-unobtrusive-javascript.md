---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript | Microsoft-Dokumentation
author: microsoft
description: Die Benutzerliste-Beispielwebanwendung veranschaulicht, wie einfach es ist die Erstellung von ASP.NET MVC 3-Anwendungen, die die Razor-Ansichts-Engine verwenden. Die Beispiel-Anwendung-s...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/01/2010
ms.topic: article
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 39ed35c1b7d5c702ffea6908daeac5ca12f1693e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398012"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="a541a-104">Erstellen einer MVC 3-Anwendung mit Razor und Unaufdringlichem JavaScript</span><span class="sxs-lookup"><span data-stu-id="a541a-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="a541a-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a541a-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a541a-106">Die Benutzerliste-Beispielwebanwendung veranschaulicht, wie einfach es ist die Erstellung von ASP.NET MVC 3-Anwendungen, die die Razor-Ansichts-Engine verwenden.</span><span class="sxs-lookup"><span data-stu-id="a541a-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="a541a-107">Die beispielanwendung veranschaulicht, wie die neue Razor-ansichtsengine mit ASP.NET MVC 3-Version und die Visual Studio 2010 zum Erstellen einer fiktiven Benutzerliste-Websites, die Funktionen wie das Erstellen, anzeigen, bearbeiten und Löschen von Benutzern enthält.</span><span class="sxs-lookup"><span data-stu-id="a541a-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="a541a-108">In diesem Tutorial wird beschrieben, die ausgeführt wurden, um die Benutzerliste ASP.NET MVC 3-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="a541a-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="a541a-109">Visual Studio-Projekts mit c# und VB-Quellcode ist zur Ergänzung dieses Themas verfügbar: [herunterladen](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="a541a-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="a541a-110">Wenn Sie Fragen zu diesem Tutorial haben, Posten Sie diese auf die [MVC-Forum](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="a541a-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="a541a-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="a541a-111">Overview</span></span>

<span data-ttu-id="a541a-112">Die Anwendung, die Sie erstellen, ist eine einfache Benutzer-Liste-Website.</span><span class="sxs-lookup"><span data-stu-id="a541a-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="a541a-113">Benutzer können eingeben, anzeigen und Aktualisieren von Benutzerinformationen.</span><span class="sxs-lookup"><span data-stu-id="a541a-113">Users can enter, view, and update user information.</span></span>

![Beispielwebsite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="a541a-115">Sie können das abgeschlossene Projekt VB und c# [hier](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="a541a-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="a541a-116">Erstellen der Webanwendung</span><span class="sxs-lookup"><span data-stu-id="a541a-116">Creating the Web Application</span></span>

<span data-ttu-id="a541a-117">Um das Lernprogramm zu starten, öffnen Sie Visual Studio 2010, und erstellen Sie ein neues Projekt mit der *ASP.NET MVC 3-Webanwendung* Vorlage.</span><span class="sxs-lookup"><span data-stu-id="a541a-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="a541a-118">Nennen Sie die Anwendung &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="a541a-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="a541a-119">[![Neues MVC 3-Projekt](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="a541a-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="a541a-120">In der **neues ASP.NET MVC 3-Projekt** wählen Sie im Dialogfeld **Internetanwendung**, wählen Sie die Razor-Ansichts-Engine, und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="a541a-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Dialogfeld "neues ASP.NET MVC 3-Projekt"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="a541a-122">In diesem Tutorial werden Sie nicht dem ASP.NET-Mitgliedschaftsanbieter verwenden können Sie die Dateien, die für die zugeordnete Anmeldung und Mitgliedschaft löschen können.</span><span class="sxs-lookup"><span data-stu-id="a541a-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="a541a-123">In **Projektmappen-Explorer**, entfernen Sie die folgenden Dateien und Verzeichnisse:</span><span class="sxs-lookup"><span data-stu-id="a541a-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="a541a-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="a541a-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="a541a-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="a541a-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="a541a-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="a541a-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="a541a-127">*Views\Account* (und alle Dateien in dieses Verzeichnis)</span><span class="sxs-lookup"><span data-stu-id="a541a-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln "exp"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="a541a-129">Bearbeiten der  <em>\_Layout.cshtml</em> -Datei und Ersetzen Sie das Markup in der `<div>` Element mit dem Namen `logindisplay` mit der Meldung <em>&quot;</em>Anmeldung deaktiviert&quot;.</span><span class="sxs-lookup"><span data-stu-id="a541a-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="a541a-130">Das folgende Beispiel zeigt das neue Markup:</span><span class="sxs-lookup"><span data-stu-id="a541a-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="a541a-131">Hinzufügen des Modells</span><span class="sxs-lookup"><span data-stu-id="a541a-131">Adding the Model</span></span>

<span data-ttu-id="a541a-132">In **Projektmappen-Explorer**, mit der rechten Maustaste die *Modelle* Ordner **hinzufügen**, und klicken Sie dann auf **Klasse**.</span><span class="sxs-lookup"><span data-stu-id="a541a-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Neue Benutzer Mdl-Klasse](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="a541a-134">Nennen Sie die Klasse `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="a541a-134">Name the class `UserModel`.</span></span> <span data-ttu-id="a541a-135">Ersetzen Sie den Inhalt von der *UserModel* -Datei mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a541a-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="a541a-136">Die `UserModel` Klasse stellt Benutzer dar.</span><span class="sxs-lookup"><span data-stu-id="a541a-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="a541a-137">Jeder Member der Klasse mit dem versehen ist die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut aus dem ["DataAnnotations"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace.</span><span class="sxs-lookup"><span data-stu-id="a541a-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="a541a-138">Die Attribute in der ["DataAnnotations"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) Namespace bieten automatische Client- und serverseitige Validierung für Webanwendungen.</span><span class="sxs-lookup"><span data-stu-id="a541a-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="a541a-139">Öffnen der `HomeController` -Klasse und das Hinzufügen einer `using` Direktive, damit Sie zugreifen können, die `UserModel` und `Users` Klassen:</span><span class="sxs-lookup"><span data-stu-id="a541a-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="a541a-140">Direkt nach der `HomeController` Deklaration, fügen Sie den folgenden Kommentar und der Verweis auf eine `Users` Klasse:</span><span class="sxs-lookup"><span data-stu-id="a541a-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="a541a-141">Die `Users` Klasse ist ein vereinfachtes, die in-Memory-Datenspeicher, die Sie in diesem Tutorial verwenden.</span><span class="sxs-lookup"><span data-stu-id="a541a-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="a541a-142">In einer echten Anwendung würden Sie eine Datenbank verwenden, um Benutzerinformationen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="a541a-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="a541a-143">Die ersten Zeilen der `HomeController` Datei werden im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a541a-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="a541a-144">Erstellen Sie die Anwendung, sodass das Benutzermodell Assistenten für den Gerüstbau im nächsten Schritt zur Verfügung stehen.</span><span class="sxs-lookup"><span data-stu-id="a541a-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="a541a-145">Erstellen die Standardansicht</span><span class="sxs-lookup"><span data-stu-id="a541a-145">Creating the Default View</span></span>

<span data-ttu-id="a541a-146">Der nächste Schritt ist zum Hinzufügen einer Aktionsmethode und können die Benutzer anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a541a-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="a541a-147">Löschen Sie die vorhandenen *Views\Home\Index* Datei.</span><span class="sxs-lookup"><span data-stu-id="a541a-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="a541a-148">Erstellen Sie ein neues *Index* Datei, die die Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a541a-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="a541a-149">In der `HomeController` Klasse, ersetzen Sie den Inhalt von der `Index` Methode durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a541a-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="a541a-150">Klicken Sie auf auf die `Index` -Methode, und klicken Sie dann auf **Ansicht hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a541a-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Ansicht hinzufügen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="a541a-152">Wählen Sie die **eine stark typisierte Ansicht erstellen** Option.</span><span class="sxs-lookup"><span data-stu-id="a541a-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="a541a-153">Für **Datenklasse anzeigen**Option **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="a541a-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="a541a-154">(Wenn Sie nicht sehen **Mvc3Razor.Models.UserModel** in die **Datenklasse anzeigen** Feld müssen Sie das Projekt zu erstellen.) Stellen Sie sicher, dass die Ansichts-Engine, um festgelegt ist **Razor**.</span><span class="sxs-lookup"><span data-stu-id="a541a-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="a541a-155">Legen Sie **Inhalt anzeigen** zu **Liste** , und klicken Sie dann auf **hinzufügen**.</span><span class="sxs-lookup"><span data-stu-id="a541a-155">Set **View content** to **List** and then click **Add**.</span></span>

![Hinzufügen der Ansicht "Index"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="a541a-157">Die neue Ansicht erstellt automatisch das Gerüst der Benutzerdaten, die an die `Index` anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a541a-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="a541a-158">Untersuchen Sie die neu generierte *Views\Home\Index* Datei.</span><span class="sxs-lookup"><span data-stu-id="a541a-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="a541a-159">Die **neu erstellen**, **bearbeiten**, **Details**, und **löschen** Links funktionieren nicht, aber der Rest der Seite ist funktionsfähig.</span><span class="sxs-lookup"><span data-stu-id="a541a-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="a541a-160">Führen Sie die Seite.</span><span class="sxs-lookup"><span data-stu-id="a541a-160">Run the page.</span></span> <span data-ttu-id="a541a-161">Sie sehen eine Liste von Benutzern.</span><span class="sxs-lookup"><span data-stu-id="a541a-161">You see a list of users.</span></span>

![Indexseite](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="a541a-163">Öffnen der *"Index.cshtml"* -Datei und Ersetzen Sie die `ActionLink` Markup für **bearbeiten**, **Details**, und **löschen** durch den folgenden Code :</span><span class="sxs-lookup"><span data-stu-id="a541a-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="a541a-164">Der Benutzername wird als ID verwendet, finden Sie den ausgewählten Datensatz in die **bearbeiten**, **Details**, und **löschen** Links.</span><span class="sxs-lookup"><span data-stu-id="a541a-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="a541a-165">Die Detailansicht erstellen</span><span class="sxs-lookup"><span data-stu-id="a541a-165">Creating the Details View</span></span>

<span data-ttu-id="a541a-166">Der nächste Schritt besteht, zum Hinzufügen einer `Details` Aktionsmethode und anzeigen, um Benutzerinformationen anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a541a-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="a541a-168">Fügen Sie die folgenden `Details` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="a541a-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="a541a-169">Klicken Sie auf auf die `Details` -Methode, und wählen Sie dann <strong>Ansicht hinzufügen</strong>.</span><span class="sxs-lookup"><span data-stu-id="a541a-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="a541a-170">Überprüfen Sie, ob die <strong>Datenklasse anzeigen</strong> enthält <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="a541a-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="a541a-171">Legen Sie <strong>Inhalt anzeigen</strong> zu <strong>Details</strong> , und klicken Sie dann auf <strong>hinzufügen</strong>.</span><span class="sxs-lookup"><span data-stu-id="a541a-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Hinzufügen der Detailansicht](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="a541a-173">Führen Sie die Anwendung, und wählen Sie einen Details-Link.</span><span class="sxs-lookup"><span data-stu-id="a541a-173">Run the application and select a details link.</span></span> <span data-ttu-id="a541a-174">Der automatische Gerüstbau zeigt jede Eigenschaft im Modell.</span><span class="sxs-lookup"><span data-stu-id="a541a-174">The automatic scaffolding shows each property in the model.</span></span>

![Details](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="a541a-176">Erstellen die Bearbeitungsansicht</span><span class="sxs-lookup"><span data-stu-id="a541a-176">Creating the Edit View</span></span>

<span data-ttu-id="a541a-177">Fügen Sie die folgenden `Edit` Methode, um den home-Controller.</span><span class="sxs-lookup"><span data-stu-id="a541a-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="a541a-178">Hinzufügen einer Ansicht wie in den vorherigen Schritten, jedoch festgelegt **Inhalt anzeigen** zu **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="a541a-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Hinzufügen der Ansicht "Bearbeiten"](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="a541a-180">Führen Sie die Anwendung, und bearbeiten Sie den ersten und letzten Namen von einem Benutzer.</span><span class="sxs-lookup"><span data-stu-id="a541a-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="a541a-181">Wenn Sie gegen verstoßen `DataAnnotation` Einschränkungen, die auf angewendet wurden die `UserModel` -Klasse, wenn der Übermittlung des Formulars, sehen Sie Validierungsfehler, die von Server-Code erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="a541a-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="a541a-182">Wenn Sie den ersten Namen ändern, z. B. &quot;Ann&quot; zu &quot;ein&quot;, wenn Sie das Formular übermitteln der folgende Fehler wird angezeigt, auf dem Formular:</span><span class="sxs-lookup"><span data-stu-id="a541a-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="a541a-183">In diesem Tutorial, behandeln Sie den Benutzernamen als Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="a541a-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="a541a-184">Aus diesem Grund kann der benutzernameneigenschaft geändert werden.</span><span class="sxs-lookup"><span data-stu-id="a541a-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="a541a-185">In der *Edit.cshtml* -Datei, direkt nach der `Html.BeginForm` -Anweisung, legen Sie den Benutzernamen ein, ein ausgeblendetes Feld sein.</span><span class="sxs-lookup"><span data-stu-id="a541a-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="a541a-186">Dies bewirkt, dass die Eigenschaft im Modell übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="a541a-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="a541a-187">Das folgende Codefragment zeigt die Platzierung der `Hidden` Anweisung:</span><span class="sxs-lookup"><span data-stu-id="a541a-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="a541a-188">Ersetzen Sie die `TextBoxFor` und `ValidationMessageFor` Markup für den mit dem Namen einer `DisplayFor` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="a541a-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="a541a-189">Die `DisplayFor` Methode zeigt die Eigenschaft als nur-Lese Element.</span><span class="sxs-lookup"><span data-stu-id="a541a-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="a541a-190">Das folgende Beispiel zeigt das vollständige Markup.</span><span class="sxs-lookup"><span data-stu-id="a541a-190">The following example shows the completed markup.</span></span> <span data-ttu-id="a541a-191">Die ursprüngliche `TextBoxFor` und `ValidationMessageFor` Aufrufe mit den Razor-Kommentare beginnen und End-Kommentar Zeichen auskommentiert werden (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="a541a-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="a541a-192">Aktivieren die clientseitige Validierung</span><span class="sxs-lookup"><span data-stu-id="a541a-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="a541a-193">Um die clientseitige Validierung in ASP.NET MVC 3 zu aktivieren, müssen Sie zwei Flags festlegen, und Sie müssen drei JavaScript-Dateien einschließen.</span><span class="sxs-lookup"><span data-stu-id="a541a-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="a541a-194">Öffnen Sie die Anwendung die *"Web.config"* Datei.</span><span class="sxs-lookup"><span data-stu-id="a541a-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="a541a-195">Stellen Sie sicher `that ClientValidationEnabled` und `UnobtrusiveJavaScriptEnabled` festgelegt sind, auf "true" in den Anwendungseinstellungen.</span><span class="sxs-lookup"><span data-stu-id="a541a-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="a541a-196">Das folgende Fragment aus dem Stammverzeichnis *"Web.config"* -Datei veranschaulicht die richtigen Einstellungen:</span><span class="sxs-lookup"><span data-stu-id="a541a-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="a541a-197">Festlegen von `UnobtrusiveJavaScriptEnabled` auf unaufdringliche Ajax ermöglicht und die unaufdringliche Clientvalidierung "true".</span><span class="sxs-lookup"><span data-stu-id="a541a-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="a541a-198">Wenn Sie die unaufdringliche Validierung verwenden, werden die Validierungsregeln in HTML5-Attribute umgewandelt.</span><span class="sxs-lookup"><span data-stu-id="a541a-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="a541a-199">Die Namen der HTML5-Attribute bestehen nur Kleinbuchstaben, Zahlen und Bindestrichen aus.</span><span class="sxs-lookup"><span data-stu-id="a541a-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="a541a-200">Festlegen von `ClientValidationEnabled` auf "true" ermöglicht die clientseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="a541a-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="a541a-201">Durch Festlegen dieser Schlüssel in der Anwendung *"Web.config"* -Datei, Sie aktivieren die Clientvalidierung und unaufdringliches JavaScript für die gesamte Anwendung.</span><span class="sxs-lookup"><span data-stu-id="a541a-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="a541a-202">Sie können auch aktivieren oder deaktivieren diese Einstellungen in einzelne Ansichten oder Controllermethoden, die mit dem folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a541a-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="a541a-203">Sie müssen auch mehrere JavaScript-Dateien in die gerenderte Ansicht einfügen.</span><span class="sxs-lookup"><span data-stu-id="a541a-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="a541a-204">Eine einfache Möglichkeit, den JavaScript-Code in allen Ansichten enthalten ist, zum Hinzufügen der *Views\Shared\\"_Layout.cshtml"* Datei.</span><span class="sxs-lookup"><span data-stu-id="a541a-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="a541a-205">Ersetzen Sie die `<head>` Element der  *\_Layout.cshtml* -Datei mit den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="a541a-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="a541a-206">Die ersten beiden jQuery-Skripts, die von der Microsoft Ajax Content Delivery Network (CDN) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="a541a-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="a541a-207">Durch das Microsoft Ajax CDN nutzen, können Sie die ersten Leistung Ihrer Anwendungen erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="a541a-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="a541a-208">Führen Sie die Anwendung, und klicken Sie auf einen Bearbeitungslink.</span><span class="sxs-lookup"><span data-stu-id="a541a-208">Run the application and click an edit link.</span></span> <span data-ttu-id="a541a-209">Quellcode der Seite im Browser anzeigen.</span><span class="sxs-lookup"><span data-stu-id="a541a-209">View the page's source in the browser.</span></span> <span data-ttu-id="a541a-210">Die Browserquelle zeigt viele Attribute des Formulars `data-val` (zur datenvalidierung).</span><span class="sxs-lookup"><span data-stu-id="a541a-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="a541a-211">Wenn die Clientvalidierung und unaufdringliches JavaScript aktiviert ist, enthalten Felder mit einer Client-Validation-Regel für die Eingabe der `data-val="true"` Attribut unaufdringlichen Validierung auslösen.</span><span class="sxs-lookup"><span data-stu-id="a541a-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="a541a-212">Z. B. die `City` Feld im Modell ergänzt wurde die [erforderlich](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) -Attribut, das Ergebnisse im HTML-Code im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="a541a-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="a541a-213">Für jede Regel Clientvalidierung Element wird, die das Formular enthält ein Attribut hinzugefügt `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="a541a-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="a541a-214">Mithilfe der `City` Feld vorherigen Beispiel lautet der erforderlichen Client-Validation-Regel generiert die `data-val-required` -Attribut und der Meldung &quot;der City-Feld ist erforderlich,&quot;.</span><span class="sxs-lookup"><span data-stu-id="a541a-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="a541a-215">Führen Sie die Anwendung, einen Benutzer zu bearbeiten und löschen die `City` Feld.</span><span class="sxs-lookup"><span data-stu-id="a541a-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="a541a-216">Wenn Sie aus dem Feld tab, sehen Sie eine Fehlermeldung für die clientseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="a541a-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Stadt erforderlich](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="a541a-218">Auf ähnliche Weise für jeden Parameter in der Clientvalidierung Regel wird ein Attribut hinzugefügt, die das Formular enthält `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="a541a-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="a541a-219">Z. B. die `FirstName` -Eigenschaft die Anmerkungen die [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) Attribut, und gibt eine Mindestlänge von 3 und einer maximalen Länge von 8.</span><span class="sxs-lookup"><span data-stu-id="a541a-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="a541a-220">Der mit dem Namen Gültigkeitsprüfungsregel `length` ist der Name des Parameters `max` und den Wert des Parameters 8.</span><span class="sxs-lookup"><span data-stu-id="a541a-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="a541a-221">Das folgende Beispiel zeigt den HTML-Code, der für generiert die `FirstName` Feld, wenn Sie einen Benutzer bearbeiten:</span><span class="sxs-lookup"><span data-stu-id="a541a-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="a541a-222">Weitere Informationen zu der unaufdringlichen Validierung, finden Sie im Eintrag [unaufdringliche Clientvalidierung in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilsons Blog.</span><span class="sxs-lookup"><span data-stu-id="a541a-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="a541a-223">In ASP.NET MVC 3-Beta müssen Sie manchmal zum Senden des Formulars, um die clientseitige Validierung zu starten.</span><span class="sxs-lookup"><span data-stu-id="a541a-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="a541a-224">Dies kann für die endgültige Version geändert werden.</span><span class="sxs-lookup"><span data-stu-id="a541a-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="a541a-225">Erstellen die Ansicht "erstellen"</span><span class="sxs-lookup"><span data-stu-id="a541a-225">Creating the Create View</span></span>

<span data-ttu-id="a541a-226">Der nächste Schritt besteht, zum Hinzufügen einer `Create` Aktionsmethode und anzeigen, damit den Benutzer einen neuen Benutzer erstellen können.</span><span class="sxs-lookup"><span data-stu-id="a541a-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="a541a-227">Fügen Sie die folgenden `Create` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="a541a-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="a541a-228">Hinzufügen einer Ansicht wie in den vorherigen Schritten, jedoch festgelegt **Inhalt anzeigen** zu **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="a541a-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Create View (Ansicht erstellen)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="a541a-230">Die Anwendung auszuführen, wählen Sie die **erstellen** verknüpfen, und fügen Sie einen neuen Benutzer hinzu.</span><span class="sxs-lookup"><span data-stu-id="a541a-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="a541a-231">Die `Create` Methode nutzt automatisch die clientseitige und serverseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="a541a-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="a541a-232">Versucht, einen Benutzernamen eingeben, die Leerzeichen, z. B. enthält &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="a541a-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="a541a-233">Wenn Sie die Registerkarte aus dem Feld für den Benutzernamen, ein Fehler die clientseitige Validierung (`White space is not allowed`) wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a541a-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="a541a-234">Fügen Sie die Delete-Methode</span><span class="sxs-lookup"><span data-stu-id="a541a-234">Add the Delete method</span></span>

<span data-ttu-id="a541a-235">Um das Tutorial abzuschließen, fügen Sie die folgenden `Delete` Methode, um den home-Controller:</span><span class="sxs-lookup"><span data-stu-id="a541a-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="a541a-236">Hinzufügen einer `Delete` anzeigen, wie in den vorherigen Schritten festlegen **Inhalt anzeigen** zu **löschen**.</span><span class="sxs-lookup"><span data-stu-id="a541a-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Ansicht löschen](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="a541a-238">Sie haben nun eine einfache, aber voll funktionsfähige ASP.NET MVC 3-Anwendung mit der Validierung.</span><span class="sxs-lookup"><span data-stu-id="a541a-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
