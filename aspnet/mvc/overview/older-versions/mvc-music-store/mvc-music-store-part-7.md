---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 7 deckt Mitgliedschaft und Autorisierung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="58e13-104">Teil 7: Mitgliedschaft und Autorisierung</span><span class="sxs-lookup"><span data-stu-id="58e13-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="58e13-105">durch [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="58e13-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="58e13-106">Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.</span><span class="sxs-lookup"><span data-stu-id="58e13-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="58e13-107">Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.</span><span class="sxs-lookup"><span data-stu-id="58e13-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="58e13-108">Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung.</span><span class="sxs-lookup"><span data-stu-id="58e13-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="58e13-109">Teil 7 deckt Mitgliedschaft und Autorisierung.</span><span class="sxs-lookup"><span data-stu-id="58e13-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="58e13-110">Unsere Speicher-Manager-Controller ist aktuell für Personen, die Zugriff auf unserer Website zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="58e13-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="58e13-111">Ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.</span><span class="sxs-lookup"><span data-stu-id="58e13-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="58e13-112">Hinzufügen von AccountController und Ansichten</span><span class="sxs-lookup"><span data-stu-id="58e13-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="58e13-113">Ein Unterschied zwischen der vollständige ASP.NET MVC 3 Web Application-Vorlage und der ASP.NET MVC 3 leere Web Application-Vorlage ist, dass die leere Vorlage eine Konto-Controller enthält.</span><span class="sxs-lookup"><span data-stu-id="58e13-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="58e13-114">Wir werden eine Konto-Controller hinzufügen, durch Kopieren einiger Dateien aus einer ASP.NET MVC-Anwendung mithilfe der vollständige ASP.NET MVC 3 Web Application-Vorlage erstellt.</span><span class="sxs-lookup"><span data-stu-id="58e13-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="58e13-115">Erstellen einer neuen ASP.NET MVC-Anwendung mithilfe der vollständige ASP.NET MVC 3 Web Application-Vorlage, und kopieren Sie die folgenden Dateien in denselben Verzeichnissen im Projekt:</span><span class="sxs-lookup"><span data-stu-id="58e13-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="58e13-116">Kopieren Sie im Verzeichnis Controller AccountController.cs</span><span class="sxs-lookup"><span data-stu-id="58e13-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="58e13-117">Kopieren Sie im Verzeichnis Modelle AccountModels</span><span class="sxs-lookup"><span data-stu-id="58e13-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="58e13-118">Erstellen Sie ein Konto Verzeichnis in das Verzeichnis "Views", und kopieren Sie alle vier Ansichten</span><span class="sxs-lookup"><span data-stu-id="58e13-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="58e13-119">Ändern Sie den Namespace für die Klassen für Controller und das Modell aus, damit sie mit MvcMusicStore beginnen.</span><span class="sxs-lookup"><span data-stu-id="58e13-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="58e13-120">Die AccountController-Klasse sollten den MvcMusicStore.Controllers-Namespace verwenden, und die AccountModels-Klasse sollte den MvcMusicStore.Models-Namespace verwenden.</span><span class="sxs-lookup"><span data-stu-id="58e13-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="58e13-121">*Hinweis: Diese Dateien sind auch verfügbar im MvcMusicStore Assets.zip Download aus dem wir unsere Website Design-Dateien am Anfang des Lernprogramms kopiert. Die Mitgliedschaft-Dateien befinden sich im Verzeichnis Code.*</span><span class="sxs-lookup"><span data-stu-id="58e13-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="58e13-122">Die aktualisierte Lösung sollte wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="58e13-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="58e13-123">Hinzufügen von einem Administrator mit dem Standort der ASP.NET-Konfiguration</span><span class="sxs-lookup"><span data-stu-id="58e13-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="58e13-124">Bevor wir in unserer Website Autorisierung erfordern, müssen wir einen Benutzer mit Zugriff erstellen.</span><span class="sxs-lookup"><span data-stu-id="58e13-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="58e13-125">Die einfachste Möglichkeit zum Erstellen eines Benutzers ist die Verwendung die integrierten ASP.NET-Konfiguration-Website.</span><span class="sxs-lookup"><span data-stu-id="58e13-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="58e13-126">Starten Sie durch Klicken auf das Symbol "im Projektmappen-Explorer nach der ASP.NET-Konfiguration-Website.</span><span class="sxs-lookup"><span data-stu-id="58e13-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="58e13-127">Dadurch wird eine Konfiguration-Website.</span><span class="sxs-lookup"><span data-stu-id="58e13-127">This launches a configuration website.</span></span> <span data-ttu-id="58e13-128">Klicken Sie auf der Registerkarte "Sicherheit" auf dem Startbildschirm aus, und klicken Sie auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.</span><span class="sxs-lookup"><span data-stu-id="58e13-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="58e13-129">Klicken Sie auf den Link "Rollen erstellen oder verwalten".</span><span class="sxs-lookup"><span data-stu-id="58e13-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="58e13-130">Geben Sie den Namen der Rolle "Administrator", und klicken Sie auf die Schaltfläche Rolle hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="58e13-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="58e13-131">Klicken Sie auf die Schaltfläche "zurück", und klicken Sie auf den Benutzer zum Erstellen eines Links auf der linken Seite.</span><span class="sxs-lookup"><span data-stu-id="58e13-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="58e13-132">Füllen Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen ein:</span><span class="sxs-lookup"><span data-stu-id="58e13-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="58e13-133">**Feld**</span><span class="sxs-lookup"><span data-stu-id="58e13-133">**Field**</span></span> | <span data-ttu-id="58e13-134">**Wert**</span><span class="sxs-lookup"><span data-stu-id="58e13-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="58e13-135">**Benutzername**</span><span class="sxs-lookup"><span data-stu-id="58e13-135">**User Name**</span></span> | <span data-ttu-id="58e13-136">Administrator</span><span class="sxs-lookup"><span data-stu-id="58e13-136">Administrator</span></span> |
| <span data-ttu-id="58e13-137">**Kennwort**</span><span class="sxs-lookup"><span data-stu-id="58e13-137">**Password**</span></span> | <span data-ttu-id="58e13-138">password123!</span><span class="sxs-lookup"><span data-stu-id="58e13-138">password123!</span></span> |
| <span data-ttu-id="58e13-139">**Kennwort bestätigen**</span><span class="sxs-lookup"><span data-stu-id="58e13-139">**Confirm Password**</span></span> | <span data-ttu-id="58e13-140">password123!</span><span class="sxs-lookup"><span data-stu-id="58e13-140">password123!</span></span> |
| <span data-ttu-id="58e13-141">**E-Mail**</span><span class="sxs-lookup"><span data-stu-id="58e13-141">**E-mail**</span></span> | <span data-ttu-id="58e13-142">(eine beliebige e-Mail-Adresse funktioniert)</span><span class="sxs-lookup"><span data-stu-id="58e13-142">(any email address will work)</span></span> |
| <span data-ttu-id="58e13-143">**Sicherheitsfrage**</span><span class="sxs-lookup"><span data-stu-id="58e13-143">**Security Question**</span></span> | <span data-ttu-id="58e13-144">(beliebig)</span><span class="sxs-lookup"><span data-stu-id="58e13-144">(whatever you like)</span></span> |
| <span data-ttu-id="58e13-145">**Sicherheitsantwort**</span><span class="sxs-lookup"><span data-stu-id="58e13-145">**Security Answer**</span></span> | <span data-ttu-id="58e13-146">(beliebig)</span><span class="sxs-lookup"><span data-stu-id="58e13-146">(whatever you like)</span></span> |

<span data-ttu-id="58e13-147">*Hinweis: Sie können natürlich alle Kennwort verwenden, Sie möchten. Die standardsicherheitseinstellungen für die ein Kennwort erfordern ein Kennwort, 7 Zeichen lang ist und ein nicht alphanumerisches Zeichen enthält.*</span><span class="sxs-lookup"><span data-stu-id="58e13-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="58e13-148">Wählen Sie die Rolle "Administrator" für diesen Benutzer aus, und klicken Sie auf die Schaltfläche "Create User".</span><span class="sxs-lookup"><span data-stu-id="58e13-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="58e13-149">Nun sehen Sie eine Meldung, dass der Benutzer erfolgreich erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="58e13-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="58e13-150">Sie können jetzt das Browserfenster schließen.</span><span class="sxs-lookup"><span data-stu-id="58e13-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="58e13-151">Rollenbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="58e13-151">Role-based Authorization</span></span>

<span data-ttu-id="58e13-152">Jetzt können wir Beschränken des Zugriffs auf die StoreManagerController mit dem Attribut [Authorize] Gibt an, dass der Benutzer der Rolle Administrator eingreifen Controller in der Klasse den Zugriff auf sein muss.</span><span class="sxs-lookup"><span data-stu-id="58e13-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="58e13-153">*Hinweis: Das Attribut [Authorize] kann für bestimmte Aktionsmethoden sowie auf Klassenebene Controller platziert werden.*</span><span class="sxs-lookup"><span data-stu-id="58e13-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="58e13-154">Jetzt das Navigieren zu /StoreManager wird ein Dialogfeld "Anmelden" geöffnet:</span><span class="sxs-lookup"><span data-stu-id="58e13-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="58e13-155">Nach der Anmeldung mit unserem neuen Administratorkonto können wir fahren Sie mit dem Album bearbeiten-Bildschirm als vor.</span><span class="sxs-lookup"><span data-stu-id="58e13-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="58e13-156">[Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="58e13-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
