---
uid: web-api/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Dieses Lernprogramm zeigt, wie Abhängigkeiten in der ASP.NET Web API-Controller einfügen. Softwareversionen, die im Lernprogramm Web API 2 Unity-Anwendungsblock verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 7f64cc83e36c80b0ffd53edfc629557c0847b200
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28036514"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="bdbf7-104">Abhängigkeitsinjektion in ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bdbf7-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bdbf7-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bdbf7-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bdbf7-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="bdbf7-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="bdbf7-107">Dieses Lernprogramm zeigt, wie Abhängigkeiten in der ASP.NET Web API-Controller einfügen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bdbf7-108">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="bdbf7-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bdbf7-109">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="bdbf7-109">Web API 2</span></span>
> - [<span data-ttu-id="bdbf7-110">Unity-Anwendungsblock</span><span class="sxs-lookup"><span data-stu-id="bdbf7-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="bdbf7-111">Entity Framework 6 (Version 5 funktioniert auch)</span><span class="sxs-lookup"><span data-stu-id="bdbf7-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="bdbf7-112">Was ist die Abhängigkeitsinjektion?</span><span class="sxs-lookup"><span data-stu-id="bdbf7-112">What is Dependency Injection?</span></span>

<span data-ttu-id="bdbf7-113">Ein *Abhängigkeit* ist jedes Objekt, das ein anderes Objekt ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="bdbf7-114">Angenommen, es ist üblich, definieren Sie eine [Repository](http://martinfowler.com/eaaCatalog/repository.html) , die Zugriff auf Daten verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="bdbf7-115">Wir mit einem Beispiel veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-115">Let's illustrate with an example.</span></span> <span data-ttu-id="bdbf7-116">Zuerst definieren wir ein Domänenmodell:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="bdbf7-117">Hier ist eine einfaches Repository-Klasse, die Elemente in einer Datenbank, die Verwendung von Entity Framework speichert.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="bdbf7-118">Jetzt sehen wir definieren einen Web-API-Controller, der für die GET-Anforderungen unterstützt `Product` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="bdbf7-119">(Ich bin, POST und andere Methoden aus Gründen der Einfachheit verlassen.) Hier wird ein erster Versuch:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="bdbf7-120">Beachten Sie, von denen die Controllerklasse abhängt `ProductRepository`, und wir sind den Controller erstellen lassen die `ProductRepository` Instanz.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="bdbf7-121">Allerdings ist es nicht ratsam, hartcodieren die Abhängigkeit auf diese Weise verschiedene Ursachen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="bdbf7-122">Wenn Sie ersetzen möchten `ProductRepository` mit einer anderen Implementierung müssen Sie auch die Controllerklasse ändern.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="bdbf7-123">Wenn die `ProductRepository` über Abhängigkeiten verfügt, müssen Sie diese in den Controller konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="bdbf7-124">Für ein großes Projekt mit mehreren Domänencontrollern wird der Konfigurationscode in Ihrem Projekt verschoben.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="bdbf7-125">Es ist schwierig, Komponententest, da der Controller fest programmiert, dass die Datenbank abzufragen ist.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="bdbf7-126">Für einen Komponententest sollten Sie Mock oder Stub-Repository verwenden, die nicht mit dem Entwurf Benutzerverzeichnis möglich ist.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="bdbf7-127">Wir können diese Probleme durch *Räumen* das Repository in den Controller.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="bdbf7-128">Gestalten Sie zunächst die `ProductRepository` Klasse in einer Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="bdbf7-129">Geben Sie dann die `IProductRepository` als Konstruktorparameter:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="bdbf7-130">Dieses Beispiel verwendet [Konstruktoreinfügung](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="bdbf7-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="bdbf7-131">Sie können auch *Setter-Injektion*, in dem Sie die Abhängigkeit über einen Setter-Methode oder Eigenschaft festlegen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="bdbf7-132">Aber jetzt ein Problem vorliegt, da Ihre Anwendung keine den Controller direkt erstellt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="bdbf7-133">Web-API erstellt den Controller aus, wenn er die Anforderung weitergeleitet und Web-API weiß nicht, alles zu `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="bdbf7-134">Dies kommt der Web-API-Abhängigkeitskonfliktlöser.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="bdbf7-135">Die Web-API-Abhängigkeitskonfliktlöser</span><span class="sxs-lookup"><span data-stu-id="bdbf7-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="bdbf7-136">Web-API definiert die **IDependencyResolver** Schnittstelle zum Auflösen von Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="bdbf7-137">Hier ist die Definition der Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="bdbf7-138">Die **IDependencyScope** Schnittstelle verfügt über zwei Methoden:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="bdbf7-139">**GetService** erstellt eine Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="bdbf7-140">**"GetServices" auf** erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="bdbf7-141">Die **IDependencyResolver** Methode erbt **IDependencyScope** und fügt die **BeginScope** Methode.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="bdbf7-142">Ich werde über Bereiche weiter unten in diesem Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="bdbf7-143">Wenn die Web-API eine Controllerinstanz erstellt wird, ruft er zuerst **IDependencyResolver.GetService**, und der Typ des Controllers übergeben.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="bdbf7-144">Diese Erweiterbarkeit Hook können Sie den Controller, erstellen Sie alle Abhängigkeiten werden aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="bdbf7-145">Wenn **GetService** gibt null zurück, sieht der Web-API für einen parameterlosen Konstruktor für die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="bdbf7-146">Abhängigkeit Auflösung mit der Unity-Container</span><span class="sxs-lookup"><span data-stu-id="bdbf7-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="bdbf7-147">Obwohl Sie eine vollständige schreiben **IDependencyResolver** Implementierung von Grund auf neu, die Schnittstelle ist, fungiert als Brücke zwischen Web-API und vorhandenen IoC-Container wirklich ausgelegt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="bdbf7-148">Ein IoC-Container ist eine Softwarekomponente, die zum Verwalten von Abhängigkeiten zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="bdbf7-149">Registrieren von Typen mit dem Container, und klicken Sie dann den Container verwenden, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="bdbf7-150">Der Container ermittelt automatisch die Abhängigkeit Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="bdbf7-151">Viele IoC-Container können Sie steuern, z. B. Objektlebensdauer und Bereich.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="bdbf7-152">"IoC" steht für "Inversion des Steuerelements" Dies ist ein allgemeines Muster, in denen ein Framework Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="bdbf7-153">Ein IoC-Container erstellt die Objekte, die "die übliche ablaufsteuerung kehrt".</span><span class="sxs-lookup"><span data-stu-id="bdbf7-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="bdbf7-154">Für dieses Lernprogramm verwenden wir [Unity](https://msdn.microsoft.com/library/ff647202.aspx) aus Microsoft Patterns &amp; Methoden.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="bdbf7-155">(Andere beliebten Bibliotheken enthalten [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), und [StructureMap ](http://docs.structuremap.net/).) NuGet-Paket-Manager können Sie Unity installieren.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="bdbf7-156">Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="bdbf7-157">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="bdbf7-158">Hier ist eine Implementierung von **IDependencyResolver** , ein Unity-Containers einschließt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="bdbf7-159">Wenn die **GetService** Methode einen Typ kann nicht aufgelöst werden, sollte zurückgegeben **null**.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="bdbf7-160">Wenn die **"GetServices" auf** Methode einen Typ kann nicht aufgelöst werden, sollten sie ein leeres Auflistungsobjekt zurück.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="bdbf7-161">Lösen Sie keine Ausnahmen für die unbekannten Typen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="bdbf7-162">Konfigurieren den Abhängigkeitskonfliktlöser</span><span class="sxs-lookup"><span data-stu-id="bdbf7-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="bdbf7-163">Legen Sie den Abhängigkeitskonfliktlöser für die **DependencyResolver** Eigenschaft des globalen **HttpConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="bdbf7-164">Im folgenden Codebeispiel Register der `IProductRepository` Schnittstelle mit Unity, und erstellt dann eine `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="bdbf7-165">Abhängigkeitsbereich und Controller-Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="bdbf7-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="bdbf7-166">Domänencontroller werden pro Anforderung erstellt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-166">Controllers are created per request.</span></span> <span data-ttu-id="bdbf7-167">Zum Verwalten der Objektlebensdauer, **IDependencyResolver** verwendet das Konzept einer *Bereich*.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="bdbf7-168">Der Abhängigkeitskonfliktlöser angefügt, um die **HttpConfiguration** Objekt hat globalen Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="bdbf7-169">Wenn die Web-API einen Controller erstellt, ruft er **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="bdbf7-170">Diese Methode gibt ein **IDependencyScope** , der einen untergeordneten Bereich darstellt.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="bdbf7-171">Web-API ruft dann **GetService** im untergeordneten Bereich den Controller zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="bdbf7-172">Wenn die Anforderung abgeschlossen ist, Web-API-Aufrufe **Dispose** auf untergeordneten Bereich.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="bdbf7-173">Verwenden der **Dispose** Methode, um den Controller Abhängigkeiten zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="bdbf7-174">Zum Implementieren von **BeginScope** richtet sich nach der IoC-Container.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="bdbf7-175">Für Unity und entspricht Bereich ein untergeordneter Container:</span><span class="sxs-lookup"><span data-stu-id="bdbf7-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="bdbf7-176">Die meisten IoC-Container haben ähnliche Entsprechungen.</span><span class="sxs-lookup"><span data-stu-id="bdbf7-176">Most IoC containers have similar equivalents.</span></span>
