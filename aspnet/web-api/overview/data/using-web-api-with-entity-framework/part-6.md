---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Erstellen Sie den JavaScript-Client | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b0b8ef9bd44bbce5102f2b12717e330f72a9e0c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400933"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="e91f7-102">Erstellen Sie den JavaScript-Client</span><span class="sxs-lookup"><span data-stu-id="e91f7-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="e91f7-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e91f7-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e91f7-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="e91f7-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e91f7-105">In diesem Abschnitt erstellen Sie den Client für die Anwendung, die mit HTML, JavaScript, und die ["Knockout.js"](http://knockoutjs.com/) Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="e91f7-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="e91f7-106">Wir erstellen die Client-app in Phasen:</span><span class="sxs-lookup"><span data-stu-id="e91f7-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="e91f7-107">Zeigt eine Liste von Büchern.</span><span class="sxs-lookup"><span data-stu-id="e91f7-107">Showing a list of books.</span></span>
- <span data-ttu-id="e91f7-108">Eine Book-Details werden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e91f7-108">Showing a book detail.</span></span>
- <span data-ttu-id="e91f7-109">Ein neues Buch wird hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e91f7-109">Adding a new book.</span></span>

<span data-ttu-id="e91f7-110">Die Knockout-Bibliothek wird das Model-View-ViewModel (MVVM)-Muster verwendet:</span><span class="sxs-lookup"><span data-stu-id="e91f7-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="e91f7-111">Die **Modell** ist die serverseitige Darstellung der Daten in der Business-Domäne (in unserem Fall Büchern und Autoren).</span><span class="sxs-lookup"><span data-stu-id="e91f7-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="e91f7-112">Die **Ansicht** spielt die Darstellungsschicht (HTML).</span><span class="sxs-lookup"><span data-stu-id="e91f7-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="e91f7-113">Die **Ansichtsmodell** ist ein JavaScript-Objekt, das die Modelle enthält.</span><span class="sxs-lookup"><span data-stu-id="e91f7-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="e91f7-114">Das Ansichtsmodell ist eine Abstraktion der Code der Benutzeroberfläche.</span><span class="sxs-lookup"><span data-stu-id="e91f7-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="e91f7-115">Es wurde keine Kenntnisse über die HTML-Darstellung.</span><span class="sxs-lookup"><span data-stu-id="e91f7-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="e91f7-116">Stattdessen es repräsentiert die abstrakte Funktionen in der Ansicht, wie z. B. &quot;eine Liste von Büchern&quot;.</span><span class="sxs-lookup"><span data-stu-id="e91f7-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="e91f7-117">Die Ansicht an das Ansichtsmodell datengebunden ist.</span><span class="sxs-lookup"><span data-stu-id="e91f7-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="e91f7-118">Updates an das Ansichtsmodell werden in der Ansicht automatisch wiedergegeben.</span><span class="sxs-lookup"><span data-stu-id="e91f7-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="e91f7-119">Das Ansichtsmodell ruft Ereignisse wie Schaltflächenklicks ebenfalls aus der Ansicht ab.</span><span class="sxs-lookup"><span data-stu-id="e91f7-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="e91f7-120">Dieser Ansatz erleichtert Ihnen so ändern Sie das Layout und die Benutzeroberfläche der app, da Sie die Bindungen ändern können, ohne Code umzuschreiben.</span><span class="sxs-lookup"><span data-stu-id="e91f7-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="e91f7-121">Beispielsweise kann eine Liste von Elementen als veranschaulicht eine `<ul>`, ändern Sie ihn später in einer Tabelle.</span><span class="sxs-lookup"><span data-stu-id="e91f7-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="e91f7-122">Fügen Sie die Knockout-Bibliothek</span><span class="sxs-lookup"><span data-stu-id="e91f7-122">Add the Knockout Library</span></span>

<span data-ttu-id="e91f7-123">In Visual Studio aus der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**.</span><span class="sxs-lookup"><span data-stu-id="e91f7-123">In Visual Studio, from the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="e91f7-124">Wählen Sie dann **-Paket-Manager-Konsole**.</span><span class="sxs-lookup"><span data-stu-id="e91f7-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="e91f7-125">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl aus:</span><span class="sxs-lookup"><span data-stu-id="e91f7-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="e91f7-126">Dieser Befehl fügt die Knockout-Dateien zum Ordner "Scripts" an.</span><span class="sxs-lookup"><span data-stu-id="e91f7-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="e91f7-127">Erstellen des Ansichtsmodells</span><span class="sxs-lookup"><span data-stu-id="e91f7-127">Create the View Model</span></span>

<span data-ttu-id="e91f7-128">Fügen Sie eine JavaScript-Datei mit dem Namen "App.js", um den Ordner "Scripts" hinzu.</span><span class="sxs-lookup"><span data-stu-id="e91f7-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="e91f7-129">(Klicken Sie im Projektmappen-Explorer mit der Maustaste Ordner "Scripts", wählen Sie **hinzufügen**, und wählen Sie dann **JavaScript-Datei**.) Fügen Sie in den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="e91f7-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="e91f7-130">In Knockout die `observable` Klasse ermöglicht die Datenbindung.</span><span class="sxs-lookup"><span data-stu-id="e91f7-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="e91f7-131">Wenn der Inhalt der Observable-Objekt ändern, benachrichtigt das beobachtbare Objekt alle datengebundenen Steuerelemente, damit sie sich selbst aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="e91f7-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="e91f7-132">(Die `observableArray` Klasse ist die Array-Version von *Observable*.) Zunächst hat unser Ansichtsmodell zwei wahrnehmbare Elemente:</span><span class="sxs-lookup"><span data-stu-id="e91f7-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="e91f7-133">`books` enthält die Liste der Bücher.</span><span class="sxs-lookup"><span data-stu-id="e91f7-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="e91f7-134">`error` enthält eine Fehlermeldung angezeigt, wenn ein AJAX-Aufruf ein Fehler auftritt.</span><span class="sxs-lookup"><span data-stu-id="e91f7-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="e91f7-135">Die `getAllBooks` Weise wird einen AJAX-Aufruf, um die Liste der Bücher zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e91f7-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="e91f7-136">Und sie das Ergebnis auf überträgt die `books` Array.</span><span class="sxs-lookup"><span data-stu-id="e91f7-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="e91f7-137">Die `ko.applyBindings` Methode ist Teil der die Knockout-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="e91f7-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="e91f7-138">Es dauert das Ansichtsmodell als Parameter und richtet die Datenbindung.</span><span class="sxs-lookup"><span data-stu-id="e91f7-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="e91f7-139">Fügen Sie ein Skript-Paket hinzu</span><span class="sxs-lookup"><span data-stu-id="e91f7-139">Add a Script Bundle</span></span>

<span data-ttu-id="e91f7-140">Bündelung ist ein Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei gebündelt.</span><span class="sxs-lookup"><span data-stu-id="e91f7-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="e91f7-141">Bündelung reduziert die Anzahl der Anforderungen an den Server, der Seitenladezeit verbessert werden kann.</span><span class="sxs-lookup"><span data-stu-id="e91f7-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="e91f7-142">Öffnen Sie die Datei App\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="e91f7-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="e91f7-143">Fügen Sie den folgenden Code, der RegisterBundles-Methode.</span><span class="sxs-lookup"><span data-stu-id="e91f7-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="e91f7-144">[Zurück](part-5.md)
> [Weiter](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="e91f7-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
