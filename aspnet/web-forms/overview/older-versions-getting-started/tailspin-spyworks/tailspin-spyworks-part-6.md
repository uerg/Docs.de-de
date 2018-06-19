---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET-Mitgliedschaft | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886809"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="cdc22-104">Teil 6: ASP.NET-Mitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="cdc22-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="cdc22-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="cdc22-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="cdc22-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="cdc22-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="cdc22-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="cdc22-109">Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.</span><span class="sxs-lookup"><span data-stu-id="cdc22-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="cdc22-110">Arbeiten mit ASP.NET Membership</span><span class="sxs-lookup"><span data-stu-id="cdc22-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="cdc22-111">Klicken Sie auf die Sicherheit</span><span class="sxs-lookup"><span data-stu-id="cdc22-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="cdc22-112">Stellen Sie sicher, dass wir die Formularauthentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="cdc22-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="cdc22-113">Verwenden Sie den Link "Create User", um eine Reihe von Benutzern zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="cdc22-114">Wenn fertig sind, finden Sie in Projektmappen-Explorer, und aktualisieren Sie die Ansicht.</span><span class="sxs-lookup"><span data-stu-id="cdc22-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="cdc22-115">Beachten Sie, dass die ASPNETDB. Feine MDF wurde erstellt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="cdc22-116">Diese Datei enthält die Tabellen, um die Hauptdienste in ASP.NET wie Mitgliedschaft zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="cdc22-117">Jetzt können wir die Implementierung des Auscheckvorgangs beginnen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="cdc22-118">Beginnen Sie mit dem Erstellen einer CheckOut.aspx-Seite.</span><span class="sxs-lookup"><span data-stu-id="cdc22-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="cdc22-119">Die Seite "CheckOut.aspx" darf nur für Benutzer verfügbar sein, angemeldet sind, damit wir den Zugriff auf einschränken angemeldeten Benutzer und umleiten, die nicht auf der Anmeldeseite angemeldet sind.</span><span class="sxs-lookup"><span data-stu-id="cdc22-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="cdc22-120">Zu diesem Zweck fügen Sie den folgenden für den Konfigurationsabschnitt der Datei "Web.config" hinzu.</span><span class="sxs-lookup"><span data-stu-id="cdc22-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="cdc22-121">Die Vorlage für ASP.NET Web Forms-Anwendungen automatisch einen Abschnitt für die Authentifizierung in unsere Datei "Web.config" hinzugefügt und die Standardanmeldeseite hergestellt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="cdc22-122">Wir müssen ändern "Login.aspx" CodeBehind-Datei, um eine anonyme Einkaufswagen migriert werden, wenn der Benutzer anmeldet.</span><span class="sxs-lookup"><span data-stu-id="cdc22-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="cdc22-123">Die Seite "ändern"\_Load-Ereignis wie folgt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="cdc22-124">Fügen Sie dann einen "LoggedIn"-Ereignishandler wie folgt den Sitzungsnamen auf neu angemeldeten Benutzers festlegen und ändern die temporäre Sitzungs-Id in den Einkaufswagen Sinn macht, des Benutzers durch Aufrufen der Methode MigrateCart in unsere MyShoppingCart-Klasse.</span><span class="sxs-lookup"><span data-stu-id="cdc22-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="cdc22-125">(Implementiert in der CS-Datei)</span><span class="sxs-lookup"><span data-stu-id="cdc22-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="cdc22-126">Implementieren Sie die Methode MigrateCart() wie folgt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="cdc22-127">In checkout.aspx verwenden EntityDataSource und eine GridView in unserer Auschecken Seite wir fast so wie wir in unserer Warenkorb-Seite hat.</span><span class="sxs-lookup"><span data-stu-id="cdc22-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="cdc22-128">Beachten Sie, dass unsere GridView-Steuerelement einen "den Ondatabound"-Ereignishandler mit dem Namen MyList gibt\_RowDataBound implementieren wir also diesem Ereignishandler wie folgt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="cdc22-129">Diese Methode hält eine laufende Summe der Warenkorb Einkaufswagen wie jede Zeile gebunden ist und die letzte Zeile der GridView aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="cdc22-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="cdc22-130">Zu diesem Zeitpunkt haben wir eine Präsentation "Überprüfen", platziert werden der Reihenfolge implementiert.</span><span class="sxs-lookup"><span data-stu-id="cdc22-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="cdc22-131">Behandeln wir eine leere einkaufswagenszenario durch Hinzufügen von wenigen Codezeilen auf unserer Seite\_Load-Ereignis:</span><span class="sxs-lookup"><span data-stu-id="cdc22-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="cdc22-132">Klickt der Benutzer auf die Schaltfläche "Absenden" führt wir im übermitteln Schaltfläche klicken-Ereignishandler den folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="cdc22-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="cdc22-133">Der "Hauptbestandteil" des Bestellvorgangs Übermittlung wird in der Methode SubmitOrder() unsere MyShoppingCart-Klasse implementiert werden.</span><span class="sxs-lookup"><span data-stu-id="cdc22-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="cdc22-134">SubmitOrder unterstützt wird:</span><span class="sxs-lookup"><span data-stu-id="cdc22-134">SubmitOrder will:</span></span>

- <span data-ttu-id="cdc22-135">Nehmen Sie alle Einzelposten in den Warenkorb, und verwenden Sie, um einen neuen Datensatz für die Reihenfolge und die zugehörigen OrderDetails Datensätze zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="cdc22-136">Versanddatum zu berechnen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="cdc22-137">Deaktivieren Sie den Einkaufswagen Sinn macht.</span><span class="sxs-lookup"><span data-stu-id="cdc22-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="cdc22-138">Im Rahmen dieser beispielanwendung müssen wir ein Lieferdatum berechnen, indem einfach das aktuelle Datum zwei Tage hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="cdc22-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="cdc22-139">Die Anwendung jetzt ausführen lässt, den Einkaufswagen Prozess von Anfang bis Ende zu testen.</span><span class="sxs-lookup"><span data-stu-id="cdc22-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cdc22-140">[Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="cdc22-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
