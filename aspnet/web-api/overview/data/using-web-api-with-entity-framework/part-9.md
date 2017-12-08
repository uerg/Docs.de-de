---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: "Hinzufügen eines neuen Elements in der Datenbank | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: d33355b1bd286513958f71ce5521942a6cbb584f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="e3511-102">Hinzufügen eines neuen Elements in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="e3511-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="e3511-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3511-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e3511-104">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="e3511-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="e3511-105">In diesem Abschnitt fügen Sie die Möglichkeit für Benutzer zum Erstellen eines neuen Buchs.</span><span class="sxs-lookup"><span data-stu-id="e3511-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="e3511-106">Fügen Sie den folgenden Code in "App.js" um das Modell anzeigen:</span><span class="sxs-lookup"><span data-stu-id="e3511-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="e3511-107">Ersetzen Sie im Index.cshtml das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="e3511-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="e3511-108">Mit:</span><span class="sxs-lookup"><span data-stu-id="e3511-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="e3511-109">Dieses Markup erstellt ein Formular zum Senden der Autors eines neuen.</span><span class="sxs-lookup"><span data-stu-id="e3511-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="e3511-110">Die Werte für den Autor Dropdown-Liste werden einer Datenbindung zu den `authors` Observable im Modell anzeigen.</span><span class="sxs-lookup"><span data-stu-id="e3511-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="e3511-111">Für die andere Formulareingaben die Werte werden einer Datenbindung zu den `newBook` Eigenschaft des Modells anzeigen.</span><span class="sxs-lookup"><span data-stu-id="e3511-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="e3511-112">Der Submit-Ereignishandler auf dem Formular gebunden ist die `addBook` Funktion:</span><span class="sxs-lookup"><span data-stu-id="e3511-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="e3511-113">Die `addBook` Funktion liest die aktuellen Werten der datengebundenen Formulareingaben für ein JSON-Objekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e3511-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="e3511-114">Und es das JSON-Objekt, das zurückgesendet `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="e3511-114">Then it POSTs the JSON object to `/api/books`.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e3511-115">[Zurück](part-8.md)
[Weiter](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="e3511-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
