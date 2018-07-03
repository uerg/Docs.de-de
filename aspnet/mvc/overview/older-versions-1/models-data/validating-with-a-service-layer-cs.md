---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Überprüfen mit einer Dienstschicht (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Erfahren Sie, wie Sie eine Validierungslogik aus Ihre Controlleraktionen und in einer separaten Dienstschicht zu verschieben. In diesem Tutorial Stephen Walther wird erläutert, wie Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 8f533fca3357d060f072e2e28d05071d636cd9e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362303"
---
<a name="validating-with-a-service-layer-c"></a><span data-ttu-id="527bc-104">Überprüfen mit einer Dienstschicht (c#)</span><span class="sxs-lookup"><span data-stu-id="527bc-104">Validating with a Service Layer (C#)</span></span>
====================
<span data-ttu-id="527bc-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="527bc-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="527bc-106">Erfahren Sie, wie Sie eine Validierungslogik aus Ihre Controlleraktionen und in einer separaten Dienstschicht zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="527bc-106">Learn how to move your validation logic out of your controller actions and into a separate service layer.</span></span> <span data-ttu-id="527bc-107">In diesem Tutorial erläutert Stephen Walther an, wie Sie eine scharfe Trennung der Zuständigkeiten verwalten können, indem isoliert von Ihrer Dienstebene aus Ihrem Controller-Ebene.</span><span class="sxs-lookup"><span data-stu-id="527bc-107">In this tutorial, Stephen Walther explains how you can maintain a sharp separation of concerns by isolating your service layer from your controller layer.</span></span>


<span data-ttu-id="527bc-108">Das Ziel dieses Lernprogramms ist eine Methode zum Ausführen der Validierung in ASP.NET MVC-Anwendungen beschrieben.</span><span class="sxs-lookup"><span data-stu-id="527bc-108">The goal of this tutorial is to describe one method of performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="527bc-109">In diesem Tutorial erfahren Sie, wie Sie eine Validierungslogik aus Ihren Controllern und in einer separaten Dienstschicht zu verschieben.</span><span class="sxs-lookup"><span data-stu-id="527bc-109">In this tutorial, you learn how to move your validation logic out of your controllers and into a separate service layer.</span></span>

## <a name="separating-concerns"></a><span data-ttu-id="527bc-110">Trennen von Concerns</span><span class="sxs-lookup"><span data-stu-id="527bc-110">Separating Concerns</span></span>

<span data-ttu-id="527bc-111">Wenn Sie eine ASP.NET MVC-Anwendung erstellen, sollten Sie Ihre Datenbanklogik nicht in Ihre Controlleraktionen platzieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-111">When you build an ASP.NET MVC application, you should not place your database logic inside your controller actions.</span></span> <span data-ttu-id="527bc-112">Kombinieren Ihre Datenbank und Controller-Logik erschwert Ihre Anwendung im Laufe der Zeit beibehalten.</span><span class="sxs-lookup"><span data-stu-id="527bc-112">Mixing your database and controller logic makes your application more difficult to maintain over time.</span></span> <span data-ttu-id="527bc-113">Es wird empfohlen, dass Sie alle Ihre Datenbanklogik in einer separaten Repository-Ebene platzieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-113">The recommendation is that you place all of your database logic in a separate repository layer.</span></span>

<span data-ttu-id="527bc-114">Liste 1 enthält beispielsweise ein einfaches Repository mit dem Namen der ProductRepository.</span><span class="sxs-lookup"><span data-stu-id="527bc-114">For example, Listing 1 contains a simple repository named the ProductRepository.</span></span> <span data-ttu-id="527bc-115">Die produktrepository enthält alle Datenzugriffscode für die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="527bc-115">The product repository contains all of the data access code for the application.</span></span> <span data-ttu-id="527bc-116">Die Liste enthält auch die IProductRepository-Schnittstelle, die das produktrepository implementiert.</span><span class="sxs-lookup"><span data-stu-id="527bc-116">The listing also includes the IProductRepository interface that the product repository implements.</span></span>

<span data-ttu-id="527bc-117">**Codebeispiel 1 – Models\ProductRepository.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-117">**Listing 1 -- Models\ProductRepository.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

<span data-ttu-id="527bc-118">Der Controller im Codebeispiel 2 verwendet die Repository-Ebene in der Index() und Create() Aktionen.</span><span class="sxs-lookup"><span data-stu-id="527bc-118">The controller in Listing 2 uses the repository layer in both its Index() and Create() actions.</span></span> <span data-ttu-id="527bc-119">Beachten Sie, dass dieser Controller keine Datenbanklogik nicht enthält.</span><span class="sxs-lookup"><span data-stu-id="527bc-119">Notice that this controller does not contain any database logic.</span></span> <span data-ttu-id="527bc-120">Erstellen eine Repository-Ebene können Sie eine saubere Trennung von Zuständigkeiten zu verwalten.</span><span class="sxs-lookup"><span data-stu-id="527bc-120">Creating a repository layer enables you to maintain a clean separation of concerns.</span></span> <span data-ttu-id="527bc-121">Controller für die Anwendung datenflusskontrolllogik verantwortlich sind, und das Repository für die Logik für den Datenzugriff verantwortlich ist.</span><span class="sxs-lookup"><span data-stu-id="527bc-121">Controllers are responsible for application flow control logic and the repository is responsible for data access logic.</span></span>

<span data-ttu-id="527bc-122">**Codebeispiel 2 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-122">**Listing 2 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a><span data-ttu-id="527bc-123">Erstellen eine Dienstschicht</span><span class="sxs-lookup"><span data-stu-id="527bc-123">Creating a Service Layer</span></span>

<span data-ttu-id="527bc-124">Also datenflusskontrolllogik Anwendung gehört, in einem Controller und Logik für den Datenzugriff in einem Repository gehört.</span><span class="sxs-lookup"><span data-stu-id="527bc-124">So, application flow control logic belongs in a controller and data access logic belongs in a repository.</span></span> <span data-ttu-id="527bc-125">In diesem Fall, in dem platziere Sie eine Validierungslogik?</span><span class="sxs-lookup"><span data-stu-id="527bc-125">In that case, where do you put your validation logic?</span></span> <span data-ttu-id="527bc-126">Eine Möglichkeit ist, platzieren Sie eine Validierungslogik in einem *Dienstschicht*.</span><span class="sxs-lookup"><span data-stu-id="527bc-126">One option is to place your validation logic in a *service layer*.</span></span>

<span data-ttu-id="527bc-127">Eine Dienstschicht ist eine zusätzliche Ebene in einer ASP.NET MVC-Anwendung, die Kommunikation zwischen einem Controller und einer Repositoryschicht vermittelt.</span><span class="sxs-lookup"><span data-stu-id="527bc-127">A service layer is an additional layer in an ASP.NET MVC application that mediates communication between a controller and repository layer.</span></span> <span data-ttu-id="527bc-128">Die Dienstschicht enthält Geschäftslogik.</span><span class="sxs-lookup"><span data-stu-id="527bc-128">The service layer contains business logic.</span></span> <span data-ttu-id="527bc-129">Insbesondere enthält sie Validierungslogik.</span><span class="sxs-lookup"><span data-stu-id="527bc-129">In particular, it contains validation logic.</span></span>

<span data-ttu-id="527bc-130">Die Product-Dienstebene in Programmausdruck 3 hat z. B. eine CreateProduct()-Methode.</span><span class="sxs-lookup"><span data-stu-id="527bc-130">For example, the product service layer in Listing 3 has a CreateProduct() method.</span></span> <span data-ttu-id="527bc-131">Die CreateProduct()-Methode ruft die ValidateProduct()-Methode, um ein neues Produkt zu überprüfen, bevor Sie das Produkt in der produktrepository übergeben.</span><span class="sxs-lookup"><span data-stu-id="527bc-131">The CreateProduct() method calls the ValidateProduct() method to validate a new product before passing the product to the product repository.</span></span>

<span data-ttu-id="527bc-132">**Codebeispiel 3 - Models\ProductService.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-132">**Listing 3 - Models\ProductService.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

<span data-ttu-id="527bc-133">Der Product-Controller wurde aktualisiert, in Listing 4 die Dienstschicht anstelle der Repositoryschicht verwenden.</span><span class="sxs-lookup"><span data-stu-id="527bc-133">The Product controller has been updated in Listing 4 to use the service layer instead of the repository layer.</span></span> <span data-ttu-id="527bc-134">Die Controller-Schicht kommuniziert mit der Dienstebene.</span><span class="sxs-lookup"><span data-stu-id="527bc-134">The controller layer talks to the service layer.</span></span> <span data-ttu-id="527bc-135">Die Dienstebene, die mit der Repositoryschicht kommuniziert werden.</span><span class="sxs-lookup"><span data-stu-id="527bc-135">The service layer talks to the repository layer.</span></span> <span data-ttu-id="527bc-136">Jede Ebene hat eine separate Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="527bc-136">Each layer has a separate responsibility.</span></span>

<span data-ttu-id="527bc-137">**Programmausdruck 4 - Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-137">**Listing 4 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

<span data-ttu-id="527bc-138">Beachten Sie, dass der Produktdienst in der Product-Controller-Konstruktor erstellt wurde.</span><span class="sxs-lookup"><span data-stu-id="527bc-138">Notice that the product service is created in the product controller constructor.</span></span> <span data-ttu-id="527bc-139">Wenn der Produktdienst erstellt wird, wird das Modellzustandswörterbuch an den Dienst übergeben.</span><span class="sxs-lookup"><span data-stu-id="527bc-139">When the product service is created, the model state dictionary is passed to the service.</span></span> <span data-ttu-id="527bc-140">Der Produktdienst verwendet den Modellzustand Validierung Fehlermeldungen an den Controller zurückgesendet.</span><span class="sxs-lookup"><span data-stu-id="527bc-140">The product service uses model state to pass validation error messages back to the controller.</span></span>

## <a name="decoupling-the-service-layer"></a><span data-ttu-id="527bc-141">Entkopplung der Dienstebene</span><span class="sxs-lookup"><span data-stu-id="527bc-141">Decoupling the Service Layer</span></span>

<span data-ttu-id="527bc-142">Es konnte nicht den Controller und die Dienstschichten in einer Hinsicht zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-142">We have failed to isolate the controller and service layers in one respect.</span></span> <span data-ttu-id="527bc-143">Der Controller und die Dienstschichten kommunizieren über die Modellstatus.</span><span class="sxs-lookup"><span data-stu-id="527bc-143">The controller and service layers communicate through model state.</span></span> <span data-ttu-id="527bc-144">Anders gesagt: die Dienstschicht weist eine Abhängigkeit auf eine bestimmte Funktion von ASP.NET MVC-Framework.</span><span class="sxs-lookup"><span data-stu-id="527bc-144">In other words, the service layer has a dependency on a particular feature of the ASP.NET MVC framework.</span></span>

<span data-ttu-id="527bc-145">Wir möchten die Dienstschicht aus unserem Controller-Ebene, die so weit wie möglich zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-145">We want to isolate the service layer from our controller layer as much as possible.</span></span> <span data-ttu-id="527bc-146">Theoretisch sollten wir die Dienstschicht mit jeder Art von Anwendung nicht nur eine ASP.NET MVC-Anwendung verwenden können.</span><span class="sxs-lookup"><span data-stu-id="527bc-146">In theory, we should be able to use the service layer with any type of application and not only an ASP.NET MVC application.</span></span> <span data-ttu-id="527bc-147">Beispielsweise sollten wir in Zukunft zum Erstellen einer WPF-Front-End für unsere Anwendung.</span><span class="sxs-lookup"><span data-stu-id="527bc-147">For example, in the future, we might want to build a WPF front-end for our application.</span></span> <span data-ttu-id="527bc-148">Es sollte eine Möglichkeit zum Entfernen der Abhängigkeit von ASP.NET MVC Modellstatus von unserem Dienstebene gefunden.</span><span class="sxs-lookup"><span data-stu-id="527bc-148">We should find a way to remove the dependency on ASP.NET MVC model state from our service layer.</span></span>

<span data-ttu-id="527bc-149">In Listing 5 ist wurde die Dienstschicht aktualisiert, damit er nicht mehr Modellzustand verwendet.</span><span class="sxs-lookup"><span data-stu-id="527bc-149">In Listing 5, the service layer has been updated so that it no longer uses model state.</span></span> <span data-ttu-id="527bc-150">Stattdessen wird jede Klasse, die die IValidationDictionary-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="527bc-150">Instead, it uses any class that implements the IValidationDictionary interface.</span></span>

<span data-ttu-id="527bc-151">**Programmausdruck 5 - Models\ProductService.cs (entkoppelt)**</span><span class="sxs-lookup"><span data-stu-id="527bc-151">**Listing 5 - Models\ProductService.cs (decoupled)**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

<span data-ttu-id="527bc-152">Die IValidationDictionary-Schnittstelle wird im Codebeispiel 6 definiert.</span><span class="sxs-lookup"><span data-stu-id="527bc-152">The IValidationDictionary interface is defined in Listing 6.</span></span> <span data-ttu-id="527bc-153">Diese einfache Schnittstelle verfügt über eine einzelne Methode und eine einzelne Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="527bc-153">This simple interface has a single method and a single property.</span></span>

<span data-ttu-id="527bc-154">**Codebeispiel 6: Models\IValidationDictionary.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-154">**Listing 6 - Models\IValidationDictionary.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

<span data-ttu-id="527bc-155">Die Klasse im Codebeispiel 7, mit dem Namen der ModelStateWrapper-Klasse implementiert die IValidationDictionary-Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="527bc-155">The class in Listing 7, named the ModelStateWrapper class, implements the IValidationDictionary interface.</span></span> <span data-ttu-id="527bc-156">Sie können die ModelStateWrapper-Klasse instanziieren, indem Sie ein Modell State-Wörterbuch an den Konstruktor übergeben.</span><span class="sxs-lookup"><span data-stu-id="527bc-156">You can instantiate the ModelStateWrapper class by passing a model state dictionary to the constructor.</span></span>

<span data-ttu-id="527bc-157">**Auflisten von 7 – Models\ModelStateWrapper.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-157">**Listing 7 - Models\ModelStateWrapper.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

<span data-ttu-id="527bc-158">Schließlich verwendet der aktualisierte Controller im Codebeispiel 8 die ModelStateWrapper beim Erstellen der Dienstschicht in seinem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="527bc-158">Finally, the updated controller in Listing 8 uses the ModelStateWrapper when creating the service layer in its constructor.</span></span>

<span data-ttu-id="527bc-159">**Auflisten von 8 – Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="527bc-159">**Listing 8 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

<span data-ttu-id="527bc-160">Verwenden die IValidationDictionary können Schnittstelle und der ModelStateWrapper-Klasse wir unsere Dienstebene aus unserem Controller Ebene vollständig zu isolieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-160">Using the IValidationDictionary interface and the ModelStateWrapper class enables us to completely isolate our service layer from our controller layer.</span></span> <span data-ttu-id="527bc-161">Die Dienstschicht ist nicht mehr Modellstatus abhängig.</span><span class="sxs-lookup"><span data-stu-id="527bc-161">The service layer is no longer dependent on model state.</span></span> <span data-ttu-id="527bc-162">Sie können jede Klasse übergeben, die um die Dienstebene der IValidationDictionary-Schnittstelle implementiert.</span><span class="sxs-lookup"><span data-stu-id="527bc-162">You can pass any class that implements the IValidationDictionary interface to the service layer.</span></span> <span data-ttu-id="527bc-163">Beispielsweise kann eine WPF-Anwendung die IValidationDictionary-Schnittstelle mit einer einfachen Auflistung-Klasse implementieren.</span><span class="sxs-lookup"><span data-stu-id="527bc-163">For example, a WPF application might implement the IValidationDictionary interface with a simple collection class.</span></span>

## <a name="summary"></a><span data-ttu-id="527bc-164">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="527bc-164">Summary</span></span>

<span data-ttu-id="527bc-165">Das Ziel in diesem Tutorial war ein Ansatz für die Ausführung der Validierung in ASP.NET MVC-Anwendungen erläutert.</span><span class="sxs-lookup"><span data-stu-id="527bc-165">The goal of this tutorial was to discuss one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="527bc-166">In diesem Tutorial haben Sie gelernt, wie alle eine Validierungslogik aus Ihren Controllern und in einer separaten Dienstschicht verschieben.</span><span class="sxs-lookup"><span data-stu-id="527bc-166">In this tutorial, you learned how to move all of your validation logic out of your controllers and into a separate service layer.</span></span> <span data-ttu-id="527bc-167">Außerdem haben Sie gelernt, Ihrer Dienstebene aus Ihrem Controller-Ebene zu isolieren, indem Sie eine Klasse ModelStateWrapper erstellen.</span><span class="sxs-lookup"><span data-stu-id="527bc-167">You also learned how to isolate your service layer from your controller layer by creating a ModelStateWrapper class.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="527bc-168">[Zurück](validating-with-the-idataerrorinfo-interface-cs.md)
> [Weiter](validation-with-the-data-annotation-validators-cs.md)</span><span class="sxs-lookup"><span data-stu-id="527bc-168">[Previous](validating-with-the-idataerrorinfo-interface-cs.md)
[Next](validation-with-the-data-annotation-validators-cs.md)</span></span>
