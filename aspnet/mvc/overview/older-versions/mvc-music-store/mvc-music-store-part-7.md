---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 7 behandelt die Mitgliedschaft und Autorisierung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 41b17315cbe1f6d93001a736bc24bf003df24061
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393169"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="d0b4d-104">Teil 7: Mitgliedschaft und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="d0b4d-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="d0b4d-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="d0b4d-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="d0b4d-106">Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="d0b4d-107">Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="d0b4d-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="d0b4d-109">Teil 7 behandelt die Mitgliedschaft und Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="d0b4d-110">Unsere Store Manager-Controller ist derzeit für alle Benutzer Zugriff auf unserer Website zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="d0b4d-111">Wir ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="d0b4d-112">Hinzufügen der AccountController und Ansichten</span><span class="sxs-lookup"><span data-stu-id="d0b4d-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="d0b4d-113">Ein Unterschied zwischen der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung und die Vorlage für ASP.NET MVC 3 leere Webanwendung ist, dass die leere Vorlage eine Konto-Controller nicht enthält.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="d0b4d-114">Wir fügen eine Konto-Controller durch Kopieren einige Dateien eine neue ASP.NET MVC-Anwendung, die aus der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung erstellt.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="d0b4d-115">Erstellen einer neuen ASP.NET MVC-Anwendung mithilfe der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung, und kopieren Sie die folgenden Dateien in denselben Verzeichnissen in Ihrem Projekt:</span><span class="sxs-lookup"><span data-stu-id="d0b4d-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="d0b4d-116">Kopieren Sie im Verzeichnis Controller AccountController.cs</span><span class="sxs-lookup"><span data-stu-id="d0b4d-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="d0b4d-117">Kopieren Sie im Verzeichnis Modelle AccountModels</span><span class="sxs-lookup"><span data-stu-id="d0b4d-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="d0b4d-118">Erstellen Sie ein Verzeichnis in das Verzeichnis "Views", und kopieren Sie alle vier Ansichten in</span><span class="sxs-lookup"><span data-stu-id="d0b4d-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="d0b4d-119">Ändern Sie den Namespace für die Controller- und Model-Klassen, damit sie mit MvcMusicStore beginnen.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="d0b4d-120">Die AccountController-Klasse sollten den MvcMusicStore.Controllers-Namespace verwenden, und die AccountModels-Klasse sollte den MvcMusicStore.Models-Namespace verwenden.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="d0b4d-121">*Hinweis: Diese Dateien sind auch im MvcMusicStore-Assets.zip Download aus dem wir unsere Website Design-Dateien am Anfang des Tutorials kopiert haben. Die Mitgliedschaft-Dateien befinden sich im Verzeichnis "Code".*</span><span class="sxs-lookup"><span data-stu-id="d0b4d-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="d0b4d-122">Die aktualisierte Projektmappe sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="d0b4d-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="d0b4d-123">Hinzufügen von einem Administrator mit dem Standort der ASP.NET-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="d0b4d-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="d0b4d-124">Bevor wir auf unserer Website Autorisierung erforderlich ist, müssen wir einen Benutzer mit Zugriff zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="d0b4d-125">Die einfachste Möglichkeit zum Erstellen eines Benutzers ist die Verwendung die integrierten ASP.NET-Konfiguration-Website.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="d0b4d-126">Starten Sie die ASP.NET-Konfiguration-Website, indem Sie auf das Symbol im Projektmappen-Explorer nach.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="d0b4d-127">Dadurch wird eine Konfigurationswebsite.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-127">This launches a configuration website.</span></span> <span data-ttu-id="d0b4d-128">Klicken Sie auf der Registerkarte "Sicherheit" auf dem Startbildschirm ein, und klicken Sie auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="d0b4d-129">Klicken Sie auf den Link "Rollen erstellen oder verwalten".</span><span class="sxs-lookup"><span data-stu-id="d0b4d-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="d0b4d-130">Geben Sie den Rollennamen "Administrator", und klicken Sie auf die Schaltfläche Rolle hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="d0b4d-131">Klicken Sie auf die Schaltfläche "zurück", und klicken Sie dann auf den Link "Benutzer erstellen" auf der linken Seite.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="d0b4d-132">Geben Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen:</span><span class="sxs-lookup"><span data-stu-id="d0b4d-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="d0b4d-133">**Feld**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-133">**Field**</span></span> | <span data-ttu-id="d0b4d-134">**Wert**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="d0b4d-135">**Benutzername**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-135">**User Name**</span></span> | <span data-ttu-id="d0b4d-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="d0b4d-136">Administrator</span></span> |
| <span data-ttu-id="d0b4d-137">**Kennwort**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-137">**Password**</span></span> | <span data-ttu-id="d0b4d-138">password123!</span><span class="sxs-lookup"><span data-stu-id="d0b4d-138">password123!</span></span> |
| <span data-ttu-id="d0b4d-139">**Kennwort bestätigen**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-139">**Confirm Password**</span></span> | <span data-ttu-id="d0b4d-140">password123!</span><span class="sxs-lookup"><span data-stu-id="d0b4d-140">password123!</span></span> |
| <span data-ttu-id="d0b4d-141">**E-Mail**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-141">**E-mail**</span></span> | <span data-ttu-id="d0b4d-142">(eine beliebige e-Mail-Adresse funktioniert)</span><span class="sxs-lookup"><span data-stu-id="d0b4d-142">(any email address will work)</span></span> |
| <span data-ttu-id="d0b4d-143">**Sicherheitsfrage**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-143">**Security Question**</span></span> | <span data-ttu-id="d0b4d-144">(beliebig)</span><span class="sxs-lookup"><span data-stu-id="d0b4d-144">(whatever you like)</span></span> |
| <span data-ttu-id="d0b4d-145">**Sicherheitsantwort**</span><span class="sxs-lookup"><span data-stu-id="d0b4d-145">**Security Answer**</span></span> | <span data-ttu-id="d0b4d-146">(beliebig)</span><span class="sxs-lookup"><span data-stu-id="d0b4d-146">(whatever you like)</span></span> |

<span data-ttu-id="d0b4d-147">*Hinweis: Sie können natürlich alle Kennwort verwenden, die Sie möchten. Die standardsicherheitseinstellungen für das Kennwort müssen es sich um ein Kennwort, das 7 Zeichen lang und enthält ein nicht alphanumerisches Zeichen.*</span><span class="sxs-lookup"><span data-stu-id="d0b4d-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="d0b4d-148">Wählen Sie die Rolle "Administrator" für diesen Benutzer aus, und klicken Sie auf die Schaltfläche "Benutzer erstellen".</span><span class="sxs-lookup"><span data-stu-id="d0b4d-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="d0b4d-149">An diesem Punkt sehen Sie eine Meldung angezeigt, dass der Benutzer erfolgreich erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="d0b4d-150">Sie können das Browserfenster jetzt schließen.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="d0b4d-151">Rollenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="d0b4d-151">Role-based Authorization</span></span>

<span data-ttu-id="d0b4d-152">Jetzt können wir Beschränken des Zugriffs auf die StoreManagerController mit dem [Authorize]-Attribut gibt an, dass der Benutzer muss in jeder beliebigen Controlleraktion in der Klasse den Zugriff auf die Rolle "Administrator" sein.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="d0b4d-153">*Hinweis: Die [Authorize]-Attribut kann sowohl auf bestimmte Methoden als auch auf Klassenebene Controller platziert werden.*</span><span class="sxs-lookup"><span data-stu-id="d0b4d-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="d0b4d-154">Nun Navigieren zu /StoreManager wird ein Dialogfeld Anmelden:</span><span class="sxs-lookup"><span data-stu-id="d0b4d-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="d0b4d-155">Nach der Anmeldung mit unserem neuen Administrator-Konto können wir auf dem Bildschirm Album bearbeiten als vor dem wechseln.</span><span class="sxs-lookup"><span data-stu-id="d0b4d-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d0b4d-156">[Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="d0b4d-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
