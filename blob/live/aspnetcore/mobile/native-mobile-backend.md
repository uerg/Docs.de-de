---
title: "Erstellen von Back-End-Diensten für systemeigene Mobile Anwendungen"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3b6a32f2-5af9-4ede-9b7f-17ab300526d0
ms.technology: aspnet
ms.prod: asp.net-core
uid: mobile/native-mobile-backend
ms.openlocfilehash: be1cd9f4fe41f1a79669975cb6a89439cdd9e5c7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="creating-backend-services-for-native-mobile-applications"></a><span data-ttu-id="3d39a-103">Erstellen von Back-End-Diensten für systemeigene Mobile Anwendungen</span><span class="sxs-lookup"><span data-stu-id="3d39a-103">Creating Backend Services for Native Mobile Applications</span></span>

<span data-ttu-id="3d39a-104">Durch [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="3d39a-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3d39a-105">Mobile apps können problemlos mit ASP.NET Core Back-End-Diensten kommunizieren.</span><span class="sxs-lookup"><span data-stu-id="3d39a-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="3d39a-106">Anzeigen oder Herunterladen von Beispielcode für Back-End-Dienste</span><span class="sxs-lookup"><span data-stu-id="3d39a-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="3d39a-107">Die systemeigene Mobile Beispiel-App</span><span class="sxs-lookup"><span data-stu-id="3d39a-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="3d39a-108">Dieses Lernprogramm veranschaulicht das Erstellen von Back-End-Diensten mithilfe von ASP.NET Core MVC zur Unterstützung systemeigener mobiler apps.</span><span class="sxs-lookup"><span data-stu-id="3d39a-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="3d39a-109">Er verwendet die [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) als seine native Client umfasst die separaten native Clients für Android, iOS, Windows Universal und Windows Phone-Geräte.</span><span class="sxs-lookup"><span data-stu-id="3d39a-109">It uses the [Xamarin Forms ToDoRest app](https://developer.xamarin.com/guides/xamarin-forms/web-services/consuming/rest/) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="3d39a-110">Sie können führen Sie die verknüpften Tutorial aus, um die systemeigene app zu erstellen (und die erforderlichen freien Xamarin-Tools zu installieren), sowie der Xamarin-beispiellösung herunterladen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="3d39a-111">Die Xamarin-Beispiel enthält ein ASP.NET Web API 2-Services-Projekt, der diesem Artikel ASP.NET Core app (mit keine Änderungen, die vom Client benötigte) ersetzt.</span><span class="sxs-lookup"><span data-stu-id="3d39a-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Führen Sie Rest-Anwendung auf einem Android-Smartphone ausgeführt wird](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="3d39a-113">Features</span><span class="sxs-lookup"><span data-stu-id="3d39a-113">Features</span></span>

<span data-ttu-id="3d39a-114">Die app ToDoRest unterstützt auflisten, hinzufügen, löschen und Aktualisieren von Aufgaben.</span><span class="sxs-lookup"><span data-stu-id="3d39a-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="3d39a-115">Jedes Element verfügt über eine ID, einen Namen, Hinweise und eine Eigenschaft, die angibt, ob er noch abgeschlossen ist.</span><span class="sxs-lookup"><span data-stu-id="3d39a-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="3d39a-116">Die Hauptansicht der Elemente, wie oben gezeigt Listet jedes Element Namen an und gibt an, ob es mit einem Häkchen erfolgt.</span><span class="sxs-lookup"><span data-stu-id="3d39a-116">The main view of the items, as shown above, lists each item's name and indicates if it is done with a checkmark.</span></span>

<span data-ttu-id="3d39a-117">Tippen Sie auf die `+` Symbol wird ein Element "hinzufügen" geöffnet:</span><span class="sxs-lookup"><span data-stu-id="3d39a-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Dialogfeld "hinzufügen"](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="3d39a-119">Tippen Sie auf ein Element auf dem Bildschirm Hauptliste öffnet ein Dialogfeld "Bearbeiten", in dem des Elements, Hinweise, und "Fertig" ändert Einstellungen geändert werden können oder das Element gelöscht werden kann:</span><span class="sxs-lookup"><span data-stu-id="3d39a-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Element-Dialogfeld "Bearbeiten"](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="3d39a-121">In diesem Beispiel wird standardmäßig am developer.xamarin.com, gehostete Back-End-Dienste verwendet wird, die nur-Lese Vorgänge können konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="3d39a-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="3d39a-122">Um es für die ASP.NET Core-app, die im nächsten Abschnitt auf Ihrem Computer erstellt, selbst zu testen, müssen Sie der app aktualisieren `RestUrl` konstant.</span><span class="sxs-lookup"><span data-stu-id="3d39a-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="3d39a-123">Navigieren Sie zu der `ToDoREST` Projekt, und öffnen Sie die *Constants.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="3d39a-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="3d39a-124">Ersetzen Sie die `RestUrl` mit einer URL, die dem Computer-IP-Adresse (nicht "localhost" oder 127.0.0.1, da diese Adresse aus dem Geräteemulator, nicht von Ihrem Computer verwendet wird).</span><span class="sxs-lookup"><span data-stu-id="3d39a-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="3d39a-125">Schließen Sie die Port-Nummer (5000).</span><span class="sxs-lookup"><span data-stu-id="3d39a-125">Include the port number as well (5000).</span></span> <span data-ttu-id="3d39a-126">Um zu testen, dass Ihre Dienste mit einem Gerät arbeiten, müssen sicherstellen Sie, dass Sie nicht über eine aktive Firewall blockiert den Zugriff auf diesen Port verfügen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="3d39a-127">Erstellen des Projekts ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3d39a-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="3d39a-128">Erstellen Sie eine neue ASP.NET Core-Webanwendung in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3d39a-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="3d39a-129">Wählen Sie die Vorlage für die Web-API und keine Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="3d39a-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="3d39a-130">Nennen Sie das Projekt *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="3d39a-130">Name the project *ToDoApi*.</span></span>

![Dialogfeld "neues ASP.NET Web Application" mit Web-API-Projektvorlage ausgewählt](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="3d39a-132">Die Anwendung, die auf alle Anforderungen an Port 5000 reagieren soll.</span><span class="sxs-lookup"><span data-stu-id="3d39a-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="3d39a-133">Update *"Program.cs"* einschließen `.UseUrls("http://*:5000")` um dies zu erreichen:</span><span class="sxs-lookup"><span data-stu-id="3d39a-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="3d39a-134">Stellen Sie sicher, dass Sie die Anwendung ausführen, sondern direkt hinter der IIS Express, die nicht lokalen Anforderungen standardmäßig ignoriert.</span><span class="sxs-lookup"><span data-stu-id="3d39a-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="3d39a-135">Führen Sie `dotnet run` von einer Befehlszeile aus, oder wählen Sie den Namen des Anwendungsprofils aus dem Dropdownmenü Debugziel in der Symbolleiste von Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3d39a-135">Run `dotnet run` from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="3d39a-136">Fügen Sie eine Modellklasse aus, um die to-do-Elemente darstellen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="3d39a-137">Markierung erforderliche Felder, die mithilfe der `[Required]` Attribut:</span><span class="sxs-lookup"><span data-stu-id="3d39a-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="3d39a-138">Die API-Methoden erfordern in irgendeiner Weise mit Daten arbeiten.</span><span class="sxs-lookup"><span data-stu-id="3d39a-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="3d39a-139">Verwenden Sie den gleichen `IToDoRepository` Schnittstelle das ursprüngliche Xamarin-Beispiel verwendet:</span><span class="sxs-lookup"><span data-stu-id="3d39a-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="3d39a-140">Für dieses Beispiel verwendet die Implementierung nur eine private Auflistung von Elementen:</span><span class="sxs-lookup"><span data-stu-id="3d39a-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="3d39a-141">Konfigurieren Sie die Implementierung in *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3d39a-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="3d39a-142">An diesem Punkt sind Sie bereit für die Erstellung der *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="3d39a-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="3d39a-143">Erfahren Sie mehr über das Erstellen von web-APIs in [Erstellen Ihrer ersten Web-API mit ASP.NET Core MVC und Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3d39a-143">Learn more about creating web APIs in [Building Your First Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="3d39a-144">Erstellen des Controllers</span><span class="sxs-lookup"><span data-stu-id="3d39a-144">Creating the Controller</span></span>

<span data-ttu-id="3d39a-145">Fügen Sie dem Projekt einen neuen Domänencontroller *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="3d39a-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="3d39a-146">Es sollte von Microsoft.AspNetCore.Mvc.Controller erben.</span><span class="sxs-lookup"><span data-stu-id="3d39a-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="3d39a-147">Hinzufügen einer `Route` Attribut, um anzugeben, dass der Controller behandelt Anforderungen für Pfade ab `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="3d39a-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="3d39a-148">Die `[controller]` Token in der Route wird durch den Namen des Controllers ersetzt (Auslassen der `Controller` Suffix), und ist besonders nützlich für globale Routen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="3d39a-149">Erfahren Sie mehr über [routing](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="3d39a-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="3d39a-150">Der Controller benötigt eine `IToDoRepository` zu funktionieren, fordern Sie eine Instanz dieses Typs über den Controller-Konstruktor.</span><span class="sxs-lookup"><span data-stu-id="3d39a-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="3d39a-151">Zur Laufzeit wird diese Instanz mit der Framework-Unterstützung für bereitgestellt werden [Abhängigkeitsinjektion](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="3d39a-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="3d39a-152">Diese API unterstützt vier verschiedene HTTP-Verben zum Ausführen von Vorgängen von CRUD (Create, Read, Update, Delete) für die Datenquelle.</span><span class="sxs-lookup"><span data-stu-id="3d39a-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="3d39a-153">Die einfachste davon ist der Lesevorgang, die eine HTTP-GET-Anforderung entspricht.</span><span class="sxs-lookup"><span data-stu-id="3d39a-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="3d39a-154">Lesen von Elementen</span><span class="sxs-lookup"><span data-stu-id="3d39a-154">Reading Items</span></span>

<span data-ttu-id="3d39a-155">Eine Liste der Elemente anfordern erfolgt mit einer GET-Anforderung an die `List` Methode.</span><span class="sxs-lookup"><span data-stu-id="3d39a-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="3d39a-156">Die `[HttpGet]` -Attribut auf die `List` Methode gibt an, dass diese Aktion nur GET-Anforderungen verarbeiten soll.</span><span class="sxs-lookup"><span data-stu-id="3d39a-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="3d39a-157">Die Route für diese Aktion wird die Route auf dem Controller angegeben.</span><span class="sxs-lookup"><span data-stu-id="3d39a-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="3d39a-158">Sie müssen nicht unbedingt der Aktionsname im Rahmen der Route verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="3d39a-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="3d39a-159">Sie müssen nur sicherstellen, dass jede Aktion eine Route eindeutigen verfügt.</span><span class="sxs-lookup"><span data-stu-id="3d39a-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="3d39a-160">Routing Attribute können auf dem Controller und Methodenebene bestimmte Routen erstellen angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="3d39a-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="3d39a-161">Die `List` Methodenrückgabe eine 200 OK-Antwort-Code und alle der ToDo-Elemente, die als JSON serialisiert.</span><span class="sxs-lookup"><span data-stu-id="3d39a-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="3d39a-162">Sie können testen, die neue API-Methode, die eine Vielzahl von Tools, z. B. mit [Postman](https://www.getpostman.com/docs/), hier dargestellt:</span><span class="sxs-lookup"><span data-stu-id="3d39a-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Postman-Konsole, die mit einer GET-Anforderung für Todoitems und der Antworttext die JSON für drei zurückgegebenen Elementen anzeigen](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="3d39a-164">Erstellen von Elementen</span><span class="sxs-lookup"><span data-stu-id="3d39a-164">Creating Items</span></span>

<span data-ttu-id="3d39a-165">Gemäß der Konvention wird erstellen neue Datenelemente, die dem HTTP-POST-Verb zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="3d39a-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="3d39a-166">Die `Create` Methode verfügt über eine `[HttpPost]` Attribut angewendet wird, und akzeptiert eine `ToDoItem` Instanz.</span><span class="sxs-lookup"><span data-stu-id="3d39a-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="3d39a-167">Da die `item` übergebene Argument im Text des BEITRAGS, dieser Parameter wird mit ergänzt die `[FromBody]` Attribut.</span><span class="sxs-lookup"><span data-stu-id="3d39a-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="3d39a-168">Innerhalb der Methode das Element wird auf Gültigkeit und vorherigen Vorhandensein im Datenspeicher überprüft, und wenn keine Probleme auftreten, wird er hinzugefügt, mit dem Repository.</span><span class="sxs-lookup"><span data-stu-id="3d39a-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it is added using the repository.</span></span> <span data-ttu-id="3d39a-169">Überprüfung `ModelState.IsValid` führt [modellüberprüfung](../mvc/models/validation.md), und in jede API-Methode, das Benutzereingaben akzeptiert, ausgeführt werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d39a-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="3d39a-170">Das Beispiel verwendet eine Enumeration mit Fehlercodes, die an dem mobilen Client übergeben werden:</span><span class="sxs-lookup"><span data-stu-id="3d39a-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="3d39a-171">Testen Sie das Hinzufügen von neuen Elementen, die mithilfe von Postman durch Auswählen der POST-Verb, das das neue Objekt im JSON-Format im Text der Anforderung bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="3d39a-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="3d39a-172">Sie sollten auch hinzufügen, eine Anforderung Header angeben einer `Content-Type` von `application/json`.</span><span class="sxs-lookup"><span data-stu-id="3d39a-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Postman-Konsole, die mit einer POST- und Antwort](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="3d39a-174">Die Methode gibt das neu erstellte Element in der Antwort.</span><span class="sxs-lookup"><span data-stu-id="3d39a-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="3d39a-175">Aktualisieren von Elementen</span><span class="sxs-lookup"><span data-stu-id="3d39a-175">Updating Items</span></span>

<span data-ttu-id="3d39a-176">Zeichnet die Änderung erfolgt mithilfe von HTTP PUT-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="3d39a-177">Abgesehen von dieser Änderung der `Edit` Methode entspricht weitgehend dem `Create`.</span><span class="sxs-lookup"><span data-stu-id="3d39a-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="3d39a-178">Beachten Sie, dass, wenn der Datensatz nicht gefunden wird, wird die `Edit` Aktion zurückgegeben wird ein `NotFound` Antwort (404).</span><span class="sxs-lookup"><span data-stu-id="3d39a-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="3d39a-179">Informationen zum Postman testen, ändern Sie das Verb PUT-Befehl.</span><span class="sxs-lookup"><span data-stu-id="3d39a-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="3d39a-180">Geben Sie die Daten des aktualisierten Objekts im Text der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="3d39a-180">Specify the updated object data in the Body of the request.</span></span>

![Postman-Konsole, die mit einer PUT und-Antwort](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="3d39a-182">Diese Methode gibt ein `NoContent` (204) Antwort bei erfolgreicher Ausführung aus Gründen der Konsistenz mit der bereits vorhandenen-API.</span><span class="sxs-lookup"><span data-stu-id="3d39a-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="3d39a-183">Löschen von Elementen</span><span class="sxs-lookup"><span data-stu-id="3d39a-183">Deleting Items</span></span>

<span data-ttu-id="3d39a-184">Löschen von Datensätzen erfolgt durch, die DELETE-Anforderungen an den Dienst, und übergeben Sie die ID des Elements gelöscht werden soll.</span><span class="sxs-lookup"><span data-stu-id="3d39a-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="3d39a-185">Mit Updates, erhalten diese Anforderungen für Elemente, die nicht vorhandene `NotFound` Antworten.</span><span class="sxs-lookup"><span data-stu-id="3d39a-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="3d39a-186">Eine erfolgreiche Anforderung erhalten, andernfalls ein `NoContent` (204) Antwort.</span><span class="sxs-lookup"><span data-stu-id="3d39a-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[Main](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="3d39a-187">Beachten Sie, dass nichts beim Testen der Delete-Funktionen im Text der Anforderung erforderlich ist.</span><span class="sxs-lookup"><span data-stu-id="3d39a-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Postman-Konsole, die mit einer DELETE- und Antwort](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="3d39a-189">Allgemeine Konventionen für Web-API</span><span class="sxs-lookup"><span data-stu-id="3d39a-189">Common Web API Conventions</span></span>

<span data-ttu-id="3d39a-190">Wie Sie die Back-End-Dienste für Ihre app entwickeln, sollten Sie mit einem konsistenten Satz von Konventionen oder Richtlinien für die Behandlung von querschnittliche Bedenken aufgerufen werden.</span><span class="sxs-lookup"><span data-stu-id="3d39a-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="3d39a-191">Beispielsweise im oben gezeigten Dienst Anforderungen für bestimmte Datensätze, die gefunden wurden empfangen einer `NotFound` Antwort, anstelle eines `BadRequest` Antwort.</span><span class="sxs-lookup"><span data-stu-id="3d39a-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="3d39a-192">Auf ähnliche Weise Befehle, die versucht, diesen Dienst, der gebunden Modelltypen immer überprüft übergeben `ModelState.IsValid` Komponentenmetadatenobjekts durch eine `BadRequest` für ungültige Modelltypen.</span><span class="sxs-lookup"><span data-stu-id="3d39a-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="3d39a-193">Nachdem Sie eine allgemeine Richtlinie für Ihre APIs identifiziert haben, können in der Regel gekapselt werden in einem [Filter](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="3d39a-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="3d39a-194">Erfahren Sie mehr über [zum Kapseln von allgemeinen API-Richtlinien in ASP.NET Core MVC-Anwendungen wie](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="3d39a-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
