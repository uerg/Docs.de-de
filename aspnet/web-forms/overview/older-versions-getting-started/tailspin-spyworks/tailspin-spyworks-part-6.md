---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET-Mitgliedschaft | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 6d01a745ca1428f065f564f676d483ee807eb52e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368472"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="4a95e-104">Teil 6: ASP.NET-Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="4a95e-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="4a95e-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="4a95e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="4a95e-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="4a95e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="4a95e-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="4a95e-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="4a95e-109">Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="4a95e-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="4a95e-110">Verwenden von ASP.NET-Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="4a95e-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="4a95e-111">Klicken Sie auf Sicherheit</span><span class="sxs-lookup"><span data-stu-id="4a95e-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="4a95e-112">Stellen Sie sicher, dass die Formularauthentifizierung verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="4a95e-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="4a95e-113">Verwenden Sie den Link "Benutzer erstellen", um eine Reihe von Benutzern zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="4a95e-114">Abschließend finden Sie in Projektmappen-Explorer, und aktualisieren Sie die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="4a95e-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="4a95e-115">Beachten Sie, dass der ASPNETDB. MDF-Datei in Ordnung wurde erstellt.</span><span class="sxs-lookup"><span data-stu-id="4a95e-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="4a95e-116">Diese Datei enthält die Tabellen, um die ASP.NET Hauptdienste wie Mitgliedschaften unterstützen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="4a95e-117">Jetzt können wir die Implementierung des Auscheckvorgangs beginnen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="4a95e-118">Beginnen Sie mit der Erstellung einer CheckOut.aspx-Seite.</span><span class="sxs-lookup"><span data-stu-id="4a95e-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="4a95e-119">Die Seite "CheckOut.aspx" sollte nur für Benutzer verfügbar sein, die in angemeldet sind, damit wir den Zugriff beschränken wird angemeldeter Benutzer und umleitungs-Benutzer, die auf die Anmeldeseite nicht angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="4a95e-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="4a95e-120">Zu diesem Zweck fügen wir die folgenden für den Konfigurationsabschnitt der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="4a95e-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="4a95e-121">Die Vorlage für ASP.NET Web Forms-Anwendungen automatisch einen Abschnitt für die Authentifizierung zu Ihrer Datei "Web.config" hinzugefügt und die Standardanmeldeseite hergestellt.</span><span class="sxs-lookup"><span data-stu-id="4a95e-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="4a95e-122">Wir müssen ändern, dass "Login.aspx" CodeBehind-Datei, eine anonyme Warenkorb zu migrieren, wenn der Benutzer anmeldet.</span><span class="sxs-lookup"><span data-stu-id="4a95e-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="4a95e-123">Ändern Sie die Seite\_Load-Ereignis wie folgt.</span><span class="sxs-lookup"><span data-stu-id="4a95e-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="4a95e-124">Fügen Sie dann einen "LoggedIn"-Ereignishandler wie folgt auf den Namen der Sitzung auf den neu angemeldeten Benutzer festgelegt, und ändern die temporäre Sitzungs-Id in den Einkaufswagen mit der der Benutzer durch Aufrufen der MigrateCart-Methode in unserer MyShoppingCart-Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="4a95e-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="4a95e-125">(Implementiert in der CS-Datei)</span><span class="sxs-lookup"><span data-stu-id="4a95e-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="4a95e-126">Implementieren Sie die MigrateCart()-Methode wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="4a95e-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="4a95e-127">In checkout.aspx verwenden EntityDataSource und einer GridView-Ansicht in unseren sehen Sie sich die Seite wir ähnlich wie wir in unserer Warenkorb-Seite haben.</span><span class="sxs-lookup"><span data-stu-id="4a95e-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="4a95e-128">Beachten Sie, dass das GridView-Steuerelement einen "den Ondatabound"-Ereignishandler mit dem Namen MyList gibt\_RowDataBound wir implementieren diesen Ereignishandler wie folgt.</span><span class="sxs-lookup"><span data-stu-id="4a95e-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="4a95e-129">Diese Methode hält, eine laufende Summe der Einkaufswagen Einkaufswagen, wie jede Zeile gebunden ist und die untersten Zeile der GridView aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="4a95e-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="4a95e-130">Zu diesem Zeitpunkt haben wir eine "Kritik" Präsentation von der Reihenfolge platziert werden implementiert.</span><span class="sxs-lookup"><span data-stu-id="4a95e-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="4a95e-131">Lassen Sie uns eine leere einkaufswagenszenario behandeln, indem Sie einige Codezeilen hinzufügen, klicken Sie auf unserer Seite\_Load-Ereignis:</span><span class="sxs-lookup"><span data-stu-id="4a95e-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="4a95e-132">Klickt der Benutzer auf die Schaltfläche "Absenden" führt es in der Senden-Schaltfläche klicken Sie auf-Ereignishandler den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="4a95e-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="4a95e-133">Der "fleischanteil" von dem bestellannahmeprozess besteht darin, in der Methode SubmitOrder() unserer MyShoppingCart-Klasse implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="4a95e-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="4a95e-134">SubmitOrder führt Folgendes aus:</span><span class="sxs-lookup"><span data-stu-id="4a95e-134">SubmitOrder will:</span></span>

- <span data-ttu-id="4a95e-135">Nehmen Sie alle Artikel im Warenkorb, und verwenden Sie, um einen neuen Datensatz für den Auftrag und die zugehörigen OrderDetails-Datensätze zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="4a95e-136">Berechnet das Versanddatum.</span><span class="sxs-lookup"><span data-stu-id="4a95e-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="4a95e-137">Deaktivieren Sie den Einkaufswagen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="4a95e-138">Im Rahmen dieser beispielanwendung werden wir ein Lieferdatum berechnen, indem Sie auf das aktuelle Datum zwei Tage hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="4a95e-139">Die Anwendung jetzt ausführen gestattet, den Einkaufswagen Prozess von Anfang bis Ende zu testen.</span><span class="sxs-lookup"><span data-stu-id="4a95e-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4a95e-140">[Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="4a95e-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
