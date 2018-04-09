---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Hinzufügen eines neuen Elements in der Datenbank | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 5845c092c4d7aee12b33b3f0a49c0e944c0fb9aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="8f0fd-102">Hinzufügen eines neuen Elements in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="8f0fd-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="8f0fd-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8f0fd-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="8f0fd-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="8f0fd-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="8f0fd-105">In diesem Abschnitt fügen Sie die Möglichkeit für Benutzer zum Erstellen eines neuen Buchs.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="8f0fd-106">Fügen Sie den folgenden Code in "App.js" um das Modell anzeigen:</span><span class="sxs-lookup"><span data-stu-id="8f0fd-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="8f0fd-107">Ersetzen Sie im Index.cshtml das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="8f0fd-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="8f0fd-108">Durch:</span><span class="sxs-lookup"><span data-stu-id="8f0fd-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="8f0fd-109">Dieses Markup erstellt ein Formular zum Senden der Autors eines neuen.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="8f0fd-110">Die Werte für den Autor Dropdown-Liste werden einer Datenbindung zu den `authors` Observable im Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="8f0fd-111">Für die andere Formulareingaben die Werte werden einer Datenbindung zu den `newBook` Eigenschaft des Modells anzeigen.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="8f0fd-112">Der Submit-Ereignishandler auf dem Formular gebunden ist die `addBook` Funktion:</span><span class="sxs-lookup"><span data-stu-id="8f0fd-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="8f0fd-113">Die `addBook` Funktion liest die aktuellen Werten der datengebundenen Formulareingaben für ein JSON-Objekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="8f0fd-114">Und es das JSON-Objekt, das zurückgesendet `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="8f0fd-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8f0fd-115">[Zurück](part-8.md)
> [Weiter](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="8f0fd-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
