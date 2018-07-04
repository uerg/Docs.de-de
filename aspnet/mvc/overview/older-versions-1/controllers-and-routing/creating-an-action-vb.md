---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Erstellen einer Aktion (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode, um eine Aktion werden.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 64bc75eaccdd71ebff59f34a824c9b6c520a27ef
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367867"
---
<a name="creating-an-action-vb"></a><span data-ttu-id="85006-104">Erstellen einer Aktion (VB)</span><span class="sxs-lookup"><span data-stu-id="85006-104">Creating an Action (VB)</span></span>
====================
<span data-ttu-id="85006-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="85006-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="85006-106">Erfahren Sie, wie Sie ASP.NET MVC-Controller eine neue Aktion hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="85006-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="85006-107">Informationen Sie zu den Anforderungen für eine Methode, um eine Aktion werden.</span><span class="sxs-lookup"><span data-stu-id="85006-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="85006-108">Das Ziel in diesem Tutorial wird beschrieben, wie Sie eine neue Controlleraktion erstellen können.</span><span class="sxs-lookup"><span data-stu-id="85006-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="85006-109">Sie erfahren Sie mehr über die Anforderungen von einer Aktionsmethode.</span><span class="sxs-lookup"><span data-stu-id="85006-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="85006-110">Sie erfahren außerdem, wie Sie verhindern, dass eine Methode als eine Aktion verfügbar gemacht werden.</span><span class="sxs-lookup"><span data-stu-id="85006-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="85006-111">Hinzufügen einer Aktion zu einem Controller</span><span class="sxs-lookup"><span data-stu-id="85006-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="85006-112">Sie können eine neue Aktion auf einen Controller hinzufügen, durch Hinzufügen einer neuen Methode mit dem Controller.</span><span class="sxs-lookup"><span data-stu-id="85006-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="85006-113">Der Controller in Codebeispiel 1 enthält beispielsweise eine Aktion, die mit dem Namen Index() und eine Aktion, die mit dem Namen SayHello().</span><span class="sxs-lookup"><span data-stu-id="85006-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="85006-114">Beide Methoden werden als Aktivitäten verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="85006-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="85006-115">**1 – Controllers\HomeController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="85006-115">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

<span data-ttu-id="85006-116">Um das Universum als Aktion verfügbar gemacht werden, muss eine Methode bestimmte Anforderungen erfüllen:</span><span class="sxs-lookup"><span data-stu-id="85006-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="85006-117">Die Methode muss öffentlich sein.</span><span class="sxs-lookup"><span data-stu-id="85006-117">The method must be public.</span></span>
- <span data-ttu-id="85006-118">Die Methode darf nicht mit einer statischen Methode sein.</span><span class="sxs-lookup"><span data-stu-id="85006-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="85006-119">Die Methode kann nicht über eine Erweiterungsmethode sein.</span><span class="sxs-lookup"><span data-stu-id="85006-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="85006-120">Die Methode kann nicht über einen Konstruktor, Getter oder Setter sein.</span><span class="sxs-lookup"><span data-stu-id="85006-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="85006-121">Die Methode keine offene generische Typen.</span><span class="sxs-lookup"><span data-stu-id="85006-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="85006-122">Die Methode ist keine Methode der Basisklasse Controller.</span><span class="sxs-lookup"><span data-stu-id="85006-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="85006-123">Die Methode darf keine enthalten **Ref** oder **out** Parameter.</span><span class="sxs-lookup"><span data-stu-id="85006-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="85006-124">Beachten Sie, dass es keine Einschränkungen für den Rückgabetyp der eine Controlleraktion gibt.</span><span class="sxs-lookup"><span data-stu-id="85006-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="85006-125">Eine Controlleraktion kann es sich um eine Zeichenfolge, einen datetime-Wert, eine Instanz der Random-Klasse, oder "void" zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="85006-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="85006-126">ASP.NET MVC-Framework konvertiert jeder Rückgabetyp, die nicht auf ein Aktionsergebnis in eine Zeichenfolge ist und die Zeichenfolge an den Browser zu rendern.</span><span class="sxs-lookup"><span data-stu-id="85006-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="85006-127">Wenn Sie eine beliebige Methode, die diese Anforderungen an einen Controller nicht verletzt hinzufügen, wird die Methode als eine Controlleraktion verfügbar gemacht.</span><span class="sxs-lookup"><span data-stu-id="85006-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="85006-128">Achten Sie hier.</span><span class="sxs-lookup"><span data-stu-id="85006-128">Be careful here.</span></span> <span data-ttu-id="85006-129">Eine Controlleraktion kann von jedem Benutzer mit dem Internet verbundenen aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="85006-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="85006-130">Erstellen Sie eine Controlleraktion DeleteMyWebsite() nicht der Fall, z. B.</span><span class="sxs-lookup"><span data-stu-id="85006-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="85006-131">Verhindert, dass eine öffentliche Methode aufgerufen wird</span><span class="sxs-lookup"><span data-stu-id="85006-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="85006-132">Wenn Sie eine öffentliche Methode in einer Controllerklasse zu erstellen müssen, nicht die Methode als eine Controlleraktion verfügbar machen möchten, und Sie verhindern, dass die Methode aufgerufen wird können, mithilfe der &lt;NonAction&gt; Attribut.</span><span class="sxs-lookup"><span data-stu-id="85006-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the &lt;NonAction&gt; attribute.</span></span> <span data-ttu-id="85006-133">Der Controller im Codebeispiel 2 enthält beispielsweise eine öffentliche Methode mit dem Namen CompanySecrets(), die mit ergänzt wird die &lt;NonAction&gt; Attribut.</span><span class="sxs-lookup"><span data-stu-id="85006-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the &lt;NonAction&gt; attribute.</span></span>

<span data-ttu-id="85006-134">**Codebeispiel 2 - Controllers\WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="85006-134">**Listing 2 - Controllers\WorkController.vb**</span></span>

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

<span data-ttu-id="85006-135">Wenn Sie versuchen, die Controlleraktion CompanySecrets() aufrufen, indem Sie die Eingabe /Work/CompanySecrets in die Adressleiste des Browsers klicken Sie dann erhalten die Fehlermeldung in Abbildung 1 Sie.</span><span class="sxs-lookup"><span data-stu-id="85006-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="85006-136">[![Aufrufen einer NonAction-Methode](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="85006-136">[![Invoking a NonAction method](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)</span></span>

<span data-ttu-id="85006-137">**Abbildung 01**: Aufrufen einer Methode NonAction ([klicken Sie, um das Bild in voller Größe anzeigen](creating-an-action-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="85006-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-vb/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="85006-138">[Zurück](creating-a-controller-vb.md)
> [Weiter](aspnet-mvc-controllers-overview-cs.md)</span><span class="sxs-lookup"><span data-stu-id="85006-138">[Previous](creating-a-controller-vb.md)
[Next](aspnet-mvc-controllers-overview-cs.md)</span></span>
