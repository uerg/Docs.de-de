---
uid: web-api/overview/advanced/dependency-injection
title: Abhängigkeitsinjektion in ASP.NET Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial zeigt, wie Abhängigkeiten in Ihre ASP.NET Web-API-Controller eingefügt wird. Die Softwareversionen, die in dem Lernprogramm Web API 2 Unity-Anwendungsblock verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 92ce5eadc7f371540295c1c4279f817dba09f8e3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369171"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a><span data-ttu-id="16e93-104">Abhängigkeitsinjektion in ASP.NET Web-API 2</span><span class="sxs-lookup"><span data-stu-id="16e93-104">Dependency Injection in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="16e93-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16e93-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="16e93-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="16e93-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> <span data-ttu-id="16e93-107">In diesem Tutorial zeigt, wie Abhängigkeiten in Ihre ASP.NET Web-API-Controller eingefügt wird.</span><span class="sxs-lookup"><span data-stu-id="16e93-107">This tutorial shows how to inject dependencies into your ASP.NET Web API controller.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="16e93-108">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="16e93-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="16e93-109">Web-API 2</span><span class="sxs-lookup"><span data-stu-id="16e93-109">Web API 2</span></span>
> - [<span data-ttu-id="16e93-110">Unity-Anwendungsblock</span><span class="sxs-lookup"><span data-stu-id="16e93-110">Unity Application Block</span></span>](https://www.nuget.org/packages/Unity/)
> - <span data-ttu-id="16e93-111">Entity Framework 6 (Version 5 funktioniert auch)</span><span class="sxs-lookup"><span data-stu-id="16e93-111">Entity Framework 6 (version 5 also works)</span></span>


## <a name="what-is-dependency-injection"></a><span data-ttu-id="16e93-112">Was ist Dependency Injection?</span><span class="sxs-lookup"><span data-stu-id="16e93-112">What is Dependency Injection?</span></span>

<span data-ttu-id="16e93-113">Ein *Abhängigkeit* ist jedes Objekt, das ein anderes Objekt ist erforderlich.</span><span class="sxs-lookup"><span data-stu-id="16e93-113">A *dependency* is any object that another object requires.</span></span> <span data-ttu-id="16e93-114">Es ist beispielsweise üblich, definieren Sie eine [Repository](http://martinfowler.com/eaaCatalog/repository.html) , die den Datenzugriff verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="16e93-114">For example, it's common to define a [repository](http://martinfowler.com/eaaCatalog/repository.html) that handles data access.</span></span> <span data-ttu-id="16e93-115">Lassen Sie uns mit einem Beispiel veranschaulichen.</span><span class="sxs-lookup"><span data-stu-id="16e93-115">Let's illustrate with an example.</span></span> <span data-ttu-id="16e93-116">Zuerst definieren wir ein Domänenmodell:</span><span class="sxs-lookup"><span data-stu-id="16e93-116">First, we'll define a domain model:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

<span data-ttu-id="16e93-117">Hier ist eine einfache repositoryklasse, die Elemente in einer Datenbank mithilfe von Entity Framework speichert.</span><span class="sxs-lookup"><span data-stu-id="16e93-117">Here is a simple repository class that stores items in a database, using Entity Framework.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

<span data-ttu-id="16e93-118">Jetzt definieren wir einen Web-API-Controller, der GET-Anforderungen für unterstützt `Product` Entitäten.</span><span class="sxs-lookup"><span data-stu-id="16e93-118">Now let's define a Web API controller that supports GET requests for `Product` entities.</span></span> <span data-ttu-id="16e93-119">(Ich bin, POST und andere Methoden aus Gründen der Einfachheit vorgenommen werden müssen.) Hier ist ein erster Versuch:</span><span class="sxs-lookup"><span data-stu-id="16e93-119">(I'm leaving out POST and other methods for simplicity.) Here is a first attempt:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

<span data-ttu-id="16e93-120">Beachten Sie, von denen die Controller-Klasse abhängt `ProductRepository`, und wir werden den Controller erstellen lassen die `ProductRepository` Instanz.</span><span class="sxs-lookup"><span data-stu-id="16e93-120">Notice that the controller class depends on `ProductRepository`, and we are letting the controller create the `ProductRepository` instance.</span></span> <span data-ttu-id="16e93-121">Allerdings ist es eine schlechte Idee, hartcodieren der Abhängigkeit auf diese Weise können verschiedene Ursachen haben.</span><span class="sxs-lookup"><span data-stu-id="16e93-121">However, it's a bad idea to hard code the dependency in this way, for several reasons.</span></span>

- <span data-ttu-id="16e93-122">Wenn Sie ersetzen möchten `ProductRepository` durch eine andere Implementierung verwenden, müssen Sie auch die Controllerklasse ändern.</span><span class="sxs-lookup"><span data-stu-id="16e93-122">If you want to replace `ProductRepository` with a different implementation, you also need to modify the controller class.</span></span>
- <span data-ttu-id="16e93-123">Wenn die `ProductRepository` über Abhängigkeiten verfügt, müssen Sie diese in den Controller konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="16e93-123">If the `ProductRepository` has dependencies, you must configure these inside the controller.</span></span> <span data-ttu-id="16e93-124">Für ein großes Projekt mit mehreren Controllern wird der Konfigurationscode auf Ihr Projekt verteilt.</span><span class="sxs-lookup"><span data-stu-id="16e93-124">For a large project with multiple controllers, your configuration code becomes scattered across your project.</span></span>
- <span data-ttu-id="16e93-125">Es ist schwierig, Komponententests, da der Controller für die Datenbank abzufragen hartcodiert ist.</span><span class="sxs-lookup"><span data-stu-id="16e93-125">It is hard to unit test, because the controller is hard-coded to query the database.</span></span> <span data-ttu-id="16e93-126">Für einen Komponententest sollten Sie ein Mock oder Stub-Repository, verwenden, die nicht mit dem Entwurf Benutzerverzeichnis möglich ist.</span><span class="sxs-lookup"><span data-stu-id="16e93-126">For a unit test, you should use a mock or stub repository, which is not possible with the currect design.</span></span>

<span data-ttu-id="16e93-127">Wir können diese Probleme durch adressieren *einfügen* das Repository in den Controller.</span><span class="sxs-lookup"><span data-stu-id="16e93-127">We can address these problems by *injecting* the repository into the controller.</span></span> <span data-ttu-id="16e93-128">Gestalten Sie zunächst die `ProductRepository` Klasse in eine Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="16e93-128">First, refactor the `ProductRepository` class into an interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

<span data-ttu-id="16e93-129">Geben Sie dann die `IProductRepository` als Konstruktorparameter:</span><span class="sxs-lookup"><span data-stu-id="16e93-129">Then provide the `IProductRepository` as a constructor parameter:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

<span data-ttu-id="16e93-130">Dieses Beispiel verwendet [Konstruktorinjektion](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span><span class="sxs-lookup"><span data-stu-id="16e93-130">This example uses [constructor injection](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection).</span></span> <span data-ttu-id="16e93-131">Sie können auch *Setter-Injektion*, in dem Sie die Abhängigkeit über einen Setter-Methode oder Eigenschaft festlegen.</span><span class="sxs-lookup"><span data-stu-id="16e93-131">You can also use *setter injection*, where you set the dependency through a setter method or property.</span></span>

<span data-ttu-id="16e93-132">Aber jetzt gibt es kein Problem, da Ihre Anwendung nicht für den Controller direkt erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="16e93-132">But now there is a problem, because your application doesn't create the controller directly.</span></span> <span data-ttu-id="16e93-133">Web-API erstellt den Controller aus, wenn die Anforderung leitet, und die Web-API weiß nicht, alles zu `IProductRepository`.</span><span class="sxs-lookup"><span data-stu-id="16e93-133">Web API creates the controller when it routes the request, and Web API doesn't know anything about `IProductRepository`.</span></span> <span data-ttu-id="16e93-134">Dies kommt der Web-API-Abhängigkeitskonfliktlöser.</span><span class="sxs-lookup"><span data-stu-id="16e93-134">This is where the Web API dependency resolver comes in.</span></span>

## <a name="the-web-api-dependency-resolver"></a><span data-ttu-id="16e93-135">Der Abhängigkeitskonfliktlöser-Web-API</span><span class="sxs-lookup"><span data-stu-id="16e93-135">The Web API Dependency Resolver</span></span>

<span data-ttu-id="16e93-136">Web-API definiert die **IDependencyResolver** Schnittstelle zum Auflösen von Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="16e93-136">Web API defines the **IDependencyResolver** interface for resolving dependencies.</span></span> <span data-ttu-id="16e93-137">Hier ist die Definition der Schnittstelle:</span><span class="sxs-lookup"><span data-stu-id="16e93-137">Here is the definition of the interface:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

<span data-ttu-id="16e93-138">Die **IDependencyScope** Schnittstelle verfügt über zwei Methoden:</span><span class="sxs-lookup"><span data-stu-id="16e93-138">The **IDependencyScope** interface has two methods:</span></span>

- <span data-ttu-id="16e93-139">**"GetService"** erstellt eine Instanz eines Typs.</span><span class="sxs-lookup"><span data-stu-id="16e93-139">**GetService** creates one instance of a type.</span></span>
- <span data-ttu-id="16e93-140">**GetServices** erstellt eine Auflistung von Objekten eines angegebenen Typs.</span><span class="sxs-lookup"><span data-stu-id="16e93-140">**GetServices** creates a collection of objects of a specified type.</span></span>

<span data-ttu-id="16e93-141">Die **IDependencyResolver** Methode erbt **IDependencyScope** und fügt die **BeginScope** Methode.</span><span class="sxs-lookup"><span data-stu-id="16e93-141">The **IDependencyResolver** method inherits **IDependencyScope** and adds the **BeginScope** method.</span></span> <span data-ttu-id="16e93-142">Ich werde später in diesem Tutorial Informationen zu Bereichen.</span><span class="sxs-lookup"><span data-stu-id="16e93-142">I'll talk about scopes later in this tutorial.</span></span>

<span data-ttu-id="16e93-143">Wenn die Web-API-Controller-Instanz erstellt wird, ruft er zuerst **IDependencyResolver.GetService**, und übergeben Sie den Typ des Domänencontrollers.</span><span class="sxs-lookup"><span data-stu-id="16e93-143">When Web API creates a controller instance, it first calls **IDependencyResolver.GetService**, passing in the controller type.</span></span> <span data-ttu-id="16e93-144">Sie können dieser erweiterungsmöglichkeit verwenden, im Controller zu erstellen, alle Abhängigkeiten werden aufgelöst.</span><span class="sxs-lookup"><span data-stu-id="16e93-144">You can use this extensibility hook to create the controller, resolving any dependencies.</span></span> <span data-ttu-id="16e93-145">Wenn **"GetService"** gibt null zurück, Web-API, sucht einen parameterlosen Konstruktor für die Controllerklasse.</span><span class="sxs-lookup"><span data-stu-id="16e93-145">If **GetService** returns null, Web API looks for a parameterless constructor on the controller class.</span></span>

## <a name="dependency-resolution-with-the-unity-container"></a><span data-ttu-id="16e93-146">Abhängigkeitsauflösung mit den Unity-Container</span><span class="sxs-lookup"><span data-stu-id="16e93-146">Dependency Resolution with the Unity Container</span></span>

<span data-ttu-id="16e93-147">Obwohl Sie eine vollständige schreiben könnten **IDependencyResolver** Implementierung von Grund auf, die Schnittstelle ist speziell, fungiert als Brücke zwischen Web-API und vorhandenen IoC-Container.</span><span class="sxs-lookup"><span data-stu-id="16e93-147">Although you could write a complete **IDependencyResolver** implementation from scratch, the interface is really designed to act as bridge between Web API and existing IoC containers.</span></span>

<span data-ttu-id="16e93-148">Ein IoC-Container ist eine Softwarekomponente, die zum Verwalten von Abhängigkeiten zuständig ist.</span><span class="sxs-lookup"><span data-stu-id="16e93-148">An IoC container is a software component that is responsible for managing dependencies.</span></span> <span data-ttu-id="16e93-149">Typen mit dem Container registrieren, und klicken Sie dann den Container verwenden, um Objekte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="16e93-149">You register types with the container, and then use the container to create objects.</span></span> <span data-ttu-id="16e93-150">Der Container ermittelt automatisch die Abhängigkeit Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="16e93-150">The container automatically figures out the dependency relations.</span></span> <span data-ttu-id="16e93-151">Viele IoC-Container können auch steuern, z. B. Objektlebensdauer und Bereich.</span><span class="sxs-lookup"><span data-stu-id="16e93-151">Many IoC containers also allow you to control things like object lifetime and scope.</span></span>

> [!NOTE]
> <span data-ttu-id="16e93-152">"IoC" steht für "Inversion of Control", dies ist ein allgemeines Muster, in denen ein Framework in Anwendungscode aufruft.</span><span class="sxs-lookup"><span data-stu-id="16e93-152">"IoC" stands for "inversion of control", which is a general pattern where a framework calls into application code.</span></span> <span data-ttu-id="16e93-153">Ein IoC-Container erstellt die Objekte, die "die übliche ablaufsteuerung kehrt".</span><span class="sxs-lookup"><span data-stu-id="16e93-153">An IoC container constructs your objects for you, which "inverts" the usual flow of control.</span></span>


<span data-ttu-id="16e93-154">In diesem Tutorial verwenden wir [Unity](https://msdn.microsoft.com/library/ff647202.aspx) aus Microsoft Patterns &amp; Methoden.</span><span class="sxs-lookup"><span data-stu-id="16e93-154">For this tutorial, we'll use [Unity](https://msdn.microsoft.com/library/ff647202.aspx) from Microsoft Patterns &amp; Practices.</span></span> <span data-ttu-id="16e93-155">(Andere beliebten Bibliotheken enthalten [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), und [StructureMap ](http://docs.structuremap.net/).) Sie können NuGet-Paket-Manager verwenden, zum Installieren von Unity.</span><span class="sxs-lookup"><span data-stu-id="16e93-155">(Other popular libraries include [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), and [StructureMap](http://docs.structuremap.net/).) You can use NuGet Package Manager to install Unity.</span></span> <span data-ttu-id="16e93-156">Von der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="16e93-156">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="16e93-157">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="16e93-157">In the Package Manager Console window, type the following command:</span></span>

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

<span data-ttu-id="16e93-158">Hier ist eine Implementierung von **IDependencyResolver** , ein Unity-Containers einschließt.</span><span class="sxs-lookup"><span data-stu-id="16e93-158">Here is an implementation of **IDependencyResolver** that wraps a Unity container.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="16e93-159">Wenn die **"GetService"** Methode ein Typs kann nicht aufgelöst werden, sollte zurückgegeben **null**.</span><span class="sxs-lookup"><span data-stu-id="16e93-159">If the **GetService** method cannot resolve a type, it should return **null**.</span></span> <span data-ttu-id="16e93-160">Wenn die **GetServices** Methode ein Typs kann nicht aufgelöst werden, sollten sie ein leeres Auflistungsobjekt zurück.</span><span class="sxs-lookup"><span data-stu-id="16e93-160">If the **GetServices** method cannot resolve a type, it should return an empty collection object.</span></span> <span data-ttu-id="16e93-161">Ausnahmen Sie keine für unbekannte Typen.</span><span class="sxs-lookup"><span data-stu-id="16e93-161">Don't throw exceptions for unknown types.</span></span>


## <a name="configuring-the-dependency-resolver"></a><span data-ttu-id="16e93-162">Konfigurieren den Abhängigkeitskonfliktlöser</span><span class="sxs-lookup"><span data-stu-id="16e93-162">Configuring the Dependency Resolver</span></span>

<span data-ttu-id="16e93-163">Legen Sie den Abhängigkeitskonfliktlöser für die **DependencyResolver** Eigenschaft des globalen **HttpConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="16e93-163">Set the dependency resolver on the **DependencyResolver** property of the global **HttpConfiguration** object.</span></span>

<span data-ttu-id="16e93-164">Der folgende code registriert das `IProductRepository` sind über Schnittstellen mit Unity und erstellt dann eine `UnityResolver`.</span><span class="sxs-lookup"><span data-stu-id="16e93-164">The following code registers the `IProductRepository` interface with Unity and then creates a `UnityResolver`.</span></span>

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a><span data-ttu-id="16e93-165">Abhängigkeitsbereich und Controller-Lebensdauer</span><span class="sxs-lookup"><span data-stu-id="16e93-165">Dependency Scope and Controller Lifetime</span></span>

<span data-ttu-id="16e93-166">Controller werden pro Anforderung erstellt.</span><span class="sxs-lookup"><span data-stu-id="16e93-166">Controllers are created per request.</span></span> <span data-ttu-id="16e93-167">Zum Verwalten der Lebensdauer von Objekten, **IDependencyResolver** verwendet das Konzept einer *Bereich*.</span><span class="sxs-lookup"><span data-stu-id="16e93-167">To manage object lifetimes, **IDependencyResolver** uses the concept of a *scope*.</span></span>

<span data-ttu-id="16e93-168">Der Abhängigkeitskonfliktlöser angefügt, um die **HttpConfiguration** Objekt hat einen globalen Gültigkeitsbereich.</span><span class="sxs-lookup"><span data-stu-id="16e93-168">The dependency resolver attached to the **HttpConfiguration** object has global scope.</span></span> <span data-ttu-id="16e93-169">Wenn die Web-API einen Controller erstellt, ruft er **BeginScope**.</span><span class="sxs-lookup"><span data-stu-id="16e93-169">When Web API creates a controller, it calls **BeginScope**.</span></span> <span data-ttu-id="16e93-170">Diese Methode gibt ein **IDependencyScope** , einen untergeordneten Bereich darstellt.</span><span class="sxs-lookup"><span data-stu-id="16e93-170">This method returns an **IDependencyScope** that represents a child scope.</span></span>

<span data-ttu-id="16e93-171">Web-API ruft dann **"GetService"** im untergeordneten Bereich den Controller zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="16e93-171">Web API then calls **GetService** on the child scope to create the controller.</span></span> <span data-ttu-id="16e93-172">Wenn die Anforderung abgeschlossen ist, Web-API-Aufrufe **Dispose** für den untergeordneten Bereich.</span><span class="sxs-lookup"><span data-stu-id="16e93-172">When request is complete, Web API calls **Dispose** on the child scope.</span></span> <span data-ttu-id="16e93-173">Verwenden der **Dispose** Methode, der Abhängigkeiten des Controllers zu verwerfen.</span><span class="sxs-lookup"><span data-stu-id="16e93-173">Use the **Dispose** method to dispose of the controller's dependencies.</span></span>

<span data-ttu-id="16e93-174">Implementierung der **BeginScope** hängt von den IoC-Container.</span><span class="sxs-lookup"><span data-stu-id="16e93-174">How you implement **BeginScope** depends on the IoC container.</span></span> <span data-ttu-id="16e93-175">Für Unity entspricht einem untergeordneten Container Bereich:</span><span class="sxs-lookup"><span data-stu-id="16e93-175">For Unity, scope corresponds to a child container:</span></span>

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

<span data-ttu-id="16e93-176">Die meisten IoC-Containern haben ähnliche Entsprechungen.</span><span class="sxs-lookup"><span data-stu-id="16e93-176">Most IoC containers have similar equivalents.</span></span>
