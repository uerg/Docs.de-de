---
title: Integrationstests zu legen, die in ASP.NET Core
author: ardalis
description: "So verwenden Sie ASP.NET Core Integrationstests zu legen, um sicherzustellen, dass eine Anwendung Komponenten ordnungsgemäß funktionieren."
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 8b0d741c05a723ad80fe812254c9a500a9fd9204
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="40125-103">Integrationstests zu legen, die in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40125-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="40125-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="40125-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="40125-105">Integrationstests wird sichergestellt, dass eine Anwendung Komponenten ordnungsgemäß funktionieren, wenn es sich bei zusammen assembliert.</span><span class="sxs-lookup"><span data-stu-id="40125-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="40125-106">ASP.NET Core unterstützt Integrationstests zu legen, mit der Komponententest-Frameworks und einem integrierten Test Webhost, der verwendet werden kann, um Mehraufwand von Netzwerk-Anforderungen zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="40125-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="40125-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="40125-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="40125-108">Einführung in die Integrationstests</span><span class="sxs-lookup"><span data-stu-id="40125-108">Introduction to integration testing</span></span>

<span data-ttu-id="40125-109">Integrationstests stellen Sie sicher, dass unterschiedliche Teile einer Anwendung ordnungsgemäß zusammenarbeiten.</span><span class="sxs-lookup"><span data-stu-id="40125-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="40125-110">Im Gegensatz zu [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), Integrationstests häufig Infrastruktur anwendungsaspekten, z. B. eine Datenbank, Dateisystem, Netzwerkressourcen, oder webanforderungen und-Antworten umfassen.</span><span class="sxs-lookup"><span data-stu-id="40125-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="40125-111">Komponententests Fakes oder Pseudoobjekten anstelle dieser Bedenken, aber der Zweck der Integrationstests wird bestätigt, dass das System arbeitet wie erwartet mit diesen Systemen.</span><span class="sxs-lookup"><span data-stu-id="40125-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="40125-112">Integrationstests, tendenziell, weil sie größere Codesegmente Übung und sie von Infrastrukturelemente, abhängig sind erheblich langsamer als Komponententests.</span><span class="sxs-lookup"><span data-stu-id="40125-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="40125-113">Daher ist es eine gute Idee, wie viele Integrationstests einzuschränken, dass Sie schreiben, insbesondere dann, wenn Sie das gleiche Verhalten mit einem Komponententest testen können.</span><span class="sxs-lookup"><span data-stu-id="40125-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="40125-114">Wenn einige Verhaltensweisen getestet werden kann, verwenden einen Komponententest oder einen Integrationstest lieber den Komponententest, da er sein werden fast immer schneller.</span><span class="sxs-lookup"><span data-stu-id="40125-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="40125-115">Möglicherweise müssen Dutzende oder Hunderte von Komponententests mit vielen unterschiedlichen Eingaben jedoch nur eine Handvoll Integrationstests, die die wichtigsten Szenarien abdecken.</span><span class="sxs-lookup"><span data-stu-id="40125-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="40125-116">Testen die Logik innerhalb der eigenen Methoden ist in der Regel die Domäne des Komponententests.</span><span class="sxs-lookup"><span data-stu-id="40125-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="40125-117">Testen die Funktionsweise der Anwendungsstatus innerhalb seiner Frameworks, z. B. ASP.NET Core oder mit einer Datenbank ist, in denen Integrationstests sprachbasierte möglich.</span><span class="sxs-lookup"><span data-stu-id="40125-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="40125-118">Es ist nicht zu viele Integrationstests, um sicherzustellen, dass eine Zeile in die Datenbank schreiben und Lesen Sie es wieder können Sie Ihre dauern.</span><span class="sxs-lookup"><span data-stu-id="40125-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="40125-119">Sie müssen nicht alle möglichen Permutation der vom Datenzugriffscode zu testen – müssen Sie nur testen genug, um erhalten Sie mehr Gewissheit, die die Anwendung ordnungsgemäß funktioniert.</span><span class="sxs-lookup"><span data-stu-id="40125-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="40125-120">Integration testen ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40125-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="40125-121">Um bis zur Integrationstests zu legen zu erhalten, müssen Sie ein Testprojekt erstellen, fügen Sie einen Verweis zum Projekt Web ASP.NET Core und installieren einen Test Runner.</span><span class="sxs-lookup"><span data-stu-id="40125-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="40125-122">Dieser Prozess wird beschrieben, der [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) Dokumentation sowie detaillierte Anweisungen zum Ausführen von Tests und Empfehlungen für die Benennung der Tests und Testklassen.</span><span class="sxs-lookup"><span data-stu-id="40125-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="40125-123">Trennen Sie die Komponententests und Integrationstests mit anderen Projekten.</span><span class="sxs-lookup"><span data-stu-id="40125-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="40125-124">Dadurch wird sichergestellt, dass Sie versehentlich Infrastruktur Bedenken in Komponententests einführen nicht und können Sie problemlos auswählen, welcher Satz von Tests ausführen.</span><span class="sxs-lookup"><span data-stu-id="40125-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="40125-125">Testhost</span><span class="sxs-lookup"><span data-stu-id="40125-125">The Test Host</span></span>

<span data-ttu-id="40125-126">ASP.NET Core umfasst einen Testhost, der Integration Testprojekte hinzugefügt werden kann und zum Hosten von verwendeten ASP.NET Core Anwendungen bedient Test fordert, ohne die Notwendigkeit einer echten Webhost.</span><span class="sxs-lookup"><span data-stu-id="40125-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="40125-127">Das bereitgestellte Beispiel enthält ein Integrationstestprojekt die konfiguriert wurde, verwenden Sie [xUnit](https://xunit.github.io) und dem Host zu testen.</span><span class="sxs-lookup"><span data-stu-id="40125-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="40125-128">Er verwendet die `Microsoft.AspNetCore.TestHost` NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="40125-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="40125-129">Einmal die `Microsoft.AspNetCore.TestHost` Paket im Projekt enthalten ist, können zum Erstellen und Konfigurieren einer `TestServer` in den Tests.</span><span class="sxs-lookup"><span data-stu-id="40125-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="40125-130">Der folgende Test zeigt, wie sicherzustellen, dass eine Anforderung auf den Stamm eines Standorts "Hello World!" zurückgibt</span><span class="sxs-lookup"><span data-stu-id="40125-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="40125-131">und sollte ausgeführt wurde erfolgreich gegen die Standardeinstellung ASP.NET Core leere Web-Vorlage, die von Visual Studio erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="40125-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="40125-132">Bei diesem Test wird das Anordnen Act Assert-Muster verwendet.</span><span class="sxs-lookup"><span data-stu-id="40125-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="40125-133">Der Diagrammfelder Schritt erfolgt in der Konstruktor, der eine Instanz erstellt `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="40125-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="40125-134">Ein konfiguriertes `WebHostBuilder` verwendet werden, zum Erstellen einer `TestHost`; in diesem Beispiel die `Configure` Methode aus dem System unter dem Test (SUT) `Startup` Klasse wird zum Übergeben der `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="40125-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="40125-135">Diese Methode wird zum Konfigurieren der Anforderungspipeline, der die `TestServer` identisch mit der Konfiguration des Servers SUT würde.</span><span class="sxs-lookup"><span data-stu-id="40125-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="40125-136">In der Act-Teil des Tests wird eine Anforderung an die `TestServer` -Instanz für den Pfad "/" und die Antwort wird zurück in eine Zeichenfolge gelesen.</span><span class="sxs-lookup"><span data-stu-id="40125-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="40125-137">Diese Zeichenfolge wird mit der erwarteten Zeichenfolge "Hello World!" verglichen.</span><span class="sxs-lookup"><span data-stu-id="40125-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="40125-138">Wenn sie übereinstimmen, wird der Test erfolgreich; Andernfalls schlägt fehl.</span><span class="sxs-lookup"><span data-stu-id="40125-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="40125-139">Jetzt können Sie einige zusätzliche Integrationstests, um sicherzustellen, dass es sich bei der es sich um Primzahlen Prüfen der Funktionalität über die Webanwendung funktioniert hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="40125-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="40125-140">Beachten Sie, dass Sie nicht wirklich versuchen So testen Sie die Richtigkeit von Primzahl Checker mit diesen Tests jedoch statt Erwartungen die Webanwendung ausführt.</span><span class="sxs-lookup"><span data-stu-id="40125-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="40125-141">Sie haben bereits Coverage für Komponententests, die Ihnen vertrauen, in sofortigen `PrimeService`, wie Sie hier sehen können:</span><span class="sxs-lookup"><span data-stu-id="40125-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Test-Explorer](integration-testing/_static/test-explorer.png)

<span data-ttu-id="40125-143">Weitere Informationen finden Sie Informationen zu den Komponententests in der [UnitTests](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) Artikel.</span><span class="sxs-lookup"><span data-stu-id="40125-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="40125-144">Integrationstests zu legen, Mvc Razor /</span><span class="sxs-lookup"><span data-stu-id="40125-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="40125-145">Testprojekte, die Razor-Ansichten enthalten erfordern `<PreserveCompilationContext>` festgelegt werden, um "true" in der *csproj* Datei:</span><span class="sxs-lookup"><span data-stu-id="40125-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="40125-146">Projekte, die dieses Element fehlt, werden eine Fehlermeldung ähnlich der folgenden generiert:</span><span class="sxs-lookup"><span data-stu-id="40125-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="40125-147">Umgestaltung Middleware verwenden</span><span class="sxs-lookup"><span data-stu-id="40125-147">Refactoring to use middleware</span></span>

<span data-ttu-id="40125-148">Umgestaltung versteht man das Ändern einer Anwendung Code aus, um den Entwurf zu verbessern, ohne dessen Verhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="40125-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="40125-149">Es sollte idealerweise ausgeführt werden, wenn es eine Sammlung ist von Tests, übergeben, da diese Hilfe stellen Sie sicher, dass das System Verhalten vor und nach der Änderung des unverändert.</span><span class="sxs-lookup"><span data-stu-id="40125-149">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="40125-150">Betrachten die Möglichkeit, die in der die Logik überprüft es sich um Primzahlen wird in der Web-Anwendungsverzeichnis implementiert `Configure` Methode finden Sie unter:</span><span class="sxs-lookup"><span data-stu-id="40125-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="40125-151">Dieser Code funktioniert allerdings handelt es sich zurückgelegt wie möchten Sie diese Art von Funktion implementieren, in einer ASP.NET Core-Anwendung auch so einfach, weil es sich handelt.</span><span class="sxs-lookup"><span data-stu-id="40125-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="40125-152">Angenommen, was die `Configure` Methode würde wie folgt aussehen, ggf. viel Code hinzugefügt, jedes Mal, wenn Sie einen anderen URL-Endpunkt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="40125-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="40125-153">Eine Option zu berücksichtigen ist das Hinzufügen von [MVC](xref:mvc/overview) an der Anwendung und Erstellen eines Domänencontrollers, auf die wesentlichen Überprüfung behandeln.</span><span class="sxs-lookup"><span data-stu-id="40125-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="40125-154">Allerdings muss die vorausgesetzt, dass Sie derzeit keine anderen MVC Funktionalität, einem bit overkill.</span><span class="sxs-lookup"><span data-stu-id="40125-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="40125-155">Sie können jedoch von ASP.NET Core profitieren [Middleware](xref:fundamentals/middleware), die hilft uns zu kapseln die Primzahlen Logik in eine eigene Klasse wird überprüft und eine bessere Leistung erzielen [Trennung von Anliegen](http://deviq.com/separation-of-concerns/) in der `Configure` Methode.</span><span class="sxs-lookup"><span data-stu-id="40125-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="40125-156">Der Pfad darf die Middleware verwendet als Parameter angegeben werden, damit die Middleware-Klasse erwartet werden sollen eine `RequestDelegate` und eine `PrimeCheckerOptions` Instanz in seinem Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="40125-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="40125-157">Wenn der Pfad der Anforderung nicht übereinstimmt, was diese Middleware ist so konfiguriert, dass Sie davon ausgehen dass, Sie einfach die nächste Middleware in der Kette aufrufen und keine weiteren Aktionen.</span><span class="sxs-lookup"><span data-stu-id="40125-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="40125-158">Der Rest des Codes Implementierung, die im war `Configure` ist jetzt der `Invoke` Methode.</span><span class="sxs-lookup"><span data-stu-id="40125-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="40125-159">Da die Middleware hängt die `PrimeService` Service, Sie können auch eine Instanz dieses Diensts mit dem Konstruktor anfordern.</span><span class="sxs-lookup"><span data-stu-id="40125-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="40125-160">Das Framework bietet dieser Dienst über [Abhängigkeitsinjektion](xref:fundamentals/dependency-injection), sofern es konfiguriert wurde, z. B. in `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="40125-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="40125-161">Da diese Middleware fungiert als Endpunkt in der Anforderung Delegatenkette auf, wenn ihr Pfad übereinstimmt, wird es nicht aufgerufen, `_next.Invoke` Wenn diese Middleware verarbeitet die Anforderung.</span><span class="sxs-lookup"><span data-stu-id="40125-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="40125-162">Mit diesem Middleware eingesetzt sind und einige nützliche Erweiterungsmethoden erstellt, um die Lesbarkeit zu konfigurieren, die umgestalteten `Configure` -Methode sieht folgendermaßen aus:</span><span class="sxs-lookup"><span data-stu-id="40125-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="40125-163">Folgende dieses refactoring sind Sie sicher, dass die Webanwendung weiterhin wie zuvor funktioniert, da die Integrationstests erfolgreich alle übergeben werden.</span><span class="sxs-lookup"><span data-stu-id="40125-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="40125-164">Es ist eine gute Idee, übertragen Sie die Änderungen zur quellcodeverwaltung, nachdem Sie eine Umgestaltung abschließen und die Tests bestanden.</span><span class="sxs-lookup"><span data-stu-id="40125-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="40125-165">Wenn Sie die testgesteuerte Entwicklung, üben sind [erwägen der Commit für die Red-Green-Refactor Zyklus](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="40125-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="40125-166">Ressourcen</span><span class="sxs-lookup"><span data-stu-id="40125-166">Resources</span></span>

* [<span data-ttu-id="40125-167">Komponententests</span><span class="sxs-lookup"><span data-stu-id="40125-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="40125-168">Middleware</span><span class="sxs-lookup"><span data-stu-id="40125-168">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="40125-169">Testen von Controllern</span><span class="sxs-lookup"><span data-stu-id="40125-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
