---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Teil 1: Übersicht und erstellen das Projekt | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: f0616383fce2e92f7d1a0b63bf840208f7327bf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394060"
---
<a name="part-1-overview-and-creating-the-project"></a><span data-ttu-id="be415-102">Teil 1: Übersicht und Erstellen des Projekts</span><span class="sxs-lookup"><span data-stu-id="be415-102">Part 1: Overview and Creating the Project</span></span>
====================
<span data-ttu-id="be415-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="be415-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="be415-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="be415-104">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

<span data-ttu-id="be415-105">Entitätsframework ist ein Framework für Objekt-relationale Zuordnung.</span><span class="sxs-lookup"><span data-stu-id="be415-105">Entity Framework is an object/relational mapping framework.</span></span> <span data-ttu-id="be415-106">Es ist die Domänenobjekte in Ihrem Code Entitäten in einer relationalen Datenbank zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="be415-106">It maps the domain objects in your code to entities in a relational database.</span></span> <span data-ttu-id="be415-107">Zum größten Teil, müssen Sie keinen der Datenbankschicht zu kümmern, da Entity Framework es für Sie übernimmt.</span><span class="sxs-lookup"><span data-stu-id="be415-107">For the most part, you do not have to worry about the database layer, because Entity Framework takes care of it for you.</span></span> <span data-ttu-id="be415-108">Ihr Code bearbeitet, die Objekte, und Änderungen in einer Datenbank beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="be415-108">Your code manipulates the objects, and changes are persisted to a database.</span></span>

## <a name="about-the-tutorial"></a><span data-ttu-id="be415-109">Über das Tutorial</span><span class="sxs-lookup"><span data-stu-id="be415-109">About the Tutorial</span></span>

<span data-ttu-id="be415-110">In diesem Tutorial erstellen Sie eine einfache Store-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be415-110">In this tutorial, you will create a simple store application.</span></span> <span data-ttu-id="be415-111">Es gibt zwei wesentliche Teile der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="be415-111">There are two main parts to the application.</span></span> <span data-ttu-id="be415-112">Standardbenutzer können Produkte anzeigen, und Aufträge zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="be415-112">Normal users can view products and create orders:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

<span data-ttu-id="be415-113">Administratoren können zu erstellen, löschen oder Bearbeiten von Produkten:</span><span class="sxs-lookup"><span data-stu-id="be415-113">Administrators can create, delete, or edit products:</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a><span data-ttu-id="be415-114">Fähigkeiten, mit denen, die Sie lernen Folgendes</span><span class="sxs-lookup"><span data-stu-id="be415-114">Skills You'll Learn</span></span>

<span data-ttu-id="be415-115">Hier ist Sie lernen Folgendes:</span><span class="sxs-lookup"><span data-stu-id="be415-115">Here's what you'll learn:</span></span>

- <span data-ttu-id="be415-116">Wie Sie Entity Framework mit ASP.NET Web-API zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="be415-116">How to use Entity Framework with ASP.NET Web API.</span></span>
- <span data-ttu-id="be415-117">Gewusst wie "Knockout.js" verwenden, um eine dynamische Clientbenutzeroberfläche zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="be415-117">How to use knockout.js to create a dynamic client UI.</span></span>
- <span data-ttu-id="be415-118">So verwenden Sie die Formularauthentifizierung mit Web-API zum Authentifizieren von Benutzern.</span><span class="sxs-lookup"><span data-stu-id="be415-118">How to use forms authentication with Web API to authenticate users.</span></span>

<span data-ttu-id="be415-119">Obwohl in diesem Tutorial eigenständig ist, empfiehlt es sich, zuerst die folgenden Lernprogramme zu lesen:</span><span class="sxs-lookup"><span data-stu-id="be415-119">Although this tutorial is self-contained, you might want to read the following tutorials first:</span></span>

- [<span data-ttu-id="be415-120">Ihre erste ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="be415-120">Your First ASP.NET Web API</span></span>](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [<span data-ttu-id="be415-121">Erstellen einer Webs-API, die unterstützt CRUD-Vorgänge</span><span class="sxs-lookup"><span data-stu-id="be415-121">Creating a Web API that Supports CRUD Operations</span></span>](../creating-a-web-api-that-supports-crud-operations.md)

<span data-ttu-id="be415-122">Sie benötigen Kenntnisse des [ASP.NET MVC](../../../../mvc/index.md) ist auch hilfreich.</span><span class="sxs-lookup"><span data-stu-id="be415-122">Some knowledge of [ASP.NET MVC](../../../../mvc/index.md) is also helpful.</span></span>

## <a name="overview"></a><span data-ttu-id="be415-123">Übersicht</span><span class="sxs-lookup"><span data-stu-id="be415-123">Overview</span></span>

<span data-ttu-id="be415-124">Auf einer hohen Ebene sieht der Architektur der Anwendung zur Verfügung:</span><span class="sxs-lookup"><span data-stu-id="be415-124">At a high level, here is the architecture of the application:</span></span>

- <span data-ttu-id="be415-125">ASP.NET MVC generiert die HTML-Seiten für den Client.</span><span class="sxs-lookup"><span data-stu-id="be415-125">ASP.NET MVC generates the HTML pages for the client.</span></span>
- <span data-ttu-id="be415-126">ASP.NET Web-API macht die CRUD-Vorgänge für die Daten ("Produkte" und "Orders") verfügbar.</span><span class="sxs-lookup"><span data-stu-id="be415-126">ASP.NET Web API exposes CRUD operations on the data (products and orders).</span></span>
- <span data-ttu-id="be415-127">Entitätsframework übersetzt der c#-Modelle, die von Web-API verwendet werden, in der Datenbankentitäten.</span><span class="sxs-lookup"><span data-stu-id="be415-127">Entity Framework translates the C# models used by Web API into database entities.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

<span data-ttu-id="be415-128">Das folgende Diagramm zeigt, wie die Domänenobjekte auf verschiedenen Ebenen der Anwendung dargestellt werden: die Datenbankebene, das Objektmodell und schließlich das Wire-Format, die zum Übertragen von Daten an den Client über HTTP verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="be415-128">The following diagram shows how the domain objects are represented at various layers of the application: The database layer, the object model, and finally the wire format, which is used to transmit data to the client via HTTP.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a><span data-ttu-id="be415-129">Visual Studio-Projekt erstellen</span><span class="sxs-lookup"><span data-stu-id="be415-129">Create the Visual Studio Project</span></span>

<span data-ttu-id="be415-130">Sie können das Tutorial-Projekt mit Visual Web Developer Express oder die Vollversion von Visual Studio erstellen.</span><span class="sxs-lookup"><span data-stu-id="be415-130">You can create the tutorial project using either Visual Web Developer Express or the full version of Visual Studio.</span></span>

<span data-ttu-id="be415-131">Von der **starten** auf **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="be415-131">From the **Start** page, click **New Project**.</span></span>

<span data-ttu-id="be415-132">In der **Vorlagen** wählen Sie im Bereich **installierte Vorlagen** und erweitern Sie die **Visual C#-** Knoten.</span><span class="sxs-lookup"><span data-stu-id="be415-132">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="be415-133">Klicken Sie unter **Visual C#-** Option **Web**.</span><span class="sxs-lookup"><span data-stu-id="be415-133">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="be415-134">Wählen Sie in der Liste der Projektvorlagen das Projekt **ASP.NET MVC 4-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="be415-134">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="be415-135">Nennen Sie das Projekt "ProductStore", und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="be415-135">Name the project "ProductStore" and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

<span data-ttu-id="be415-136">In der **neues ASP.NET MVC 4-Projekt** wählen Sie im Dialogfeld **Internetanwendung** , und klicken Sie auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="be415-136">In the **New ASP.NET MVC 4 Project** dialog, select **Internet Application** and click **OK**.</span></span>

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

<span data-ttu-id="be415-137">Die Vorlage "Internetanwendung" erstellt eine ASP.NET MVC-Anwendung, die Formularauthentifizierung unterstützt.</span><span class="sxs-lookup"><span data-stu-id="be415-137">The "Internet Application" template creates an ASP.NET MVC application that supports forms authentication.</span></span> <span data-ttu-id="be415-138">Wenn Sie die Anwendung jetzt ausführen, verfügt sie bereits über einige Features:</span><span class="sxs-lookup"><span data-stu-id="be415-138">If you run the application now, it already has some features:</span></span>

- <span data-ttu-id="be415-139">Neue Benutzer können auf den Link "Register" in der oberen rechten Ecke registrieren.</span><span class="sxs-lookup"><span data-stu-id="be415-139">New users can register by clicking the "Register" link in the upper right corner.</span></span>
- <span data-ttu-id="be415-140">Registrierte Benutzer können auf den Link "Anmelden" anmelden.</span><span class="sxs-lookup"><span data-stu-id="be415-140">Registered users can log in by clicking the "Log in" link.</span></span>

<span data-ttu-id="be415-141">Informationen zur Mitgliedschaft werden in einer Datenbank beibehalten, der automatisch erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="be415-141">Membership information is persisted in a database that gets created automatically.</span></span> <span data-ttu-id="be415-142">Weitere Informationen zur Formularauthentifizierung in ASP.NET MVC finden Sie unter [Exemplarische Vorgehensweise: Verwenden der Formularauthentifizierung in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span><span class="sxs-lookup"><span data-stu-id="be415-142">For more information about forms authentication in ASP.NET MVC, see [Walkthrough: Using Forms Authentication in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).</span></span>

## <a name="update-the-css-file"></a><span data-ttu-id="be415-143">Aktualisieren Sie die CSS-Datei</span><span class="sxs-lookup"><span data-stu-id="be415-143">Update the CSS File</span></span>

<span data-ttu-id="be415-144">Dieser Schritt ist kosmetischer Natur, aber machen die Seiten, wie die früheren Screenshots gerendert wird.</span><span class="sxs-lookup"><span data-stu-id="be415-144">This step is cosmetic, but it will make the pages render like the earlier screen shots.</span></span>

<span data-ttu-id="be415-145">Klicken Sie im Projektmappen-Explorer erweitern Sie den Ordner "Content", und öffnen Sie die Datei mit dem Namen "Site.CSS".</span><span class="sxs-lookup"><span data-stu-id="be415-145">In Solution Explorer, expand the Content folder and open the file named Site.css.</span></span> <span data-ttu-id="be415-146">Fügen Sie die folgenden CSS-Stile hinzu:</span><span class="sxs-lookup"><span data-stu-id="be415-146">Add the following CSS styles:</span></span>

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [<span data-ttu-id="be415-147">Nächste</span><span class="sxs-lookup"><span data-stu-id="be415-147">Next</span></span>](using-web-api-with-entity-framework-part-2.md)
