---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-9
title: Hinzufügen eines neuen Elements in der Datenbank | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 0967c29e-e124-4db0-a788-c45d0ff5aff2
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-9
msc.type: authoredcontent
ms.openlocfilehash: 36251ba907a6f580b63f0fded0591c26b6ff879e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818507"
---
<a name="add-a-new-item-to-the-database"></a><span data-ttu-id="d3b4b-102">Hinzufügen eines neuen Elements in der Datenbank</span><span class="sxs-lookup"><span data-stu-id="d3b4b-102">Add a New Item to the Database</span></span>
====================
<span data-ttu-id="d3b4b-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d3b4b-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="d3b4b-104">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="d3b4b-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="d3b4b-105">In diesem Abschnitt fügen Sie die Möglichkeit für Benutzer, um ein neues Buch zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-105">In this section, you will add the ability for users to create a new book.</span></span> <span data-ttu-id="d3b4b-106">Fügen Sie den folgenden Code in "app.js" an das Ansichtsmodell:</span><span class="sxs-lookup"><span data-stu-id="d3b4b-106">In app.js, add the following code to the view model:</span></span>

[!code-javascript[Main](part-9/samples/sample1.js)]

<span data-ttu-id="d3b4b-107">Ersetzen Sie in "Index.cshtml" das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="d3b4b-107">In Index.cshtml, replace the following markup:</span></span>

[!code-html[Main](part-9/samples/sample2.html)]

<span data-ttu-id="d3b4b-108">Durch:</span><span class="sxs-lookup"><span data-stu-id="d3b4b-108">With:</span></span>

[!code-html[Main](part-9/samples/sample3.html)]

<span data-ttu-id="d3b4b-109">Dieses Markup erstellt ein Formular zum Senden eines neuen Autors.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-109">This markup creates a form for submitting a new author.</span></span> <span data-ttu-id="d3b4b-110">Die Werte für den Autor Dropdown-Liste sind eine Datenbindung an die `authors` Observable im Ansichtsmodell.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-110">The values for the author drop-down list are data-bound to the `authors` observable in the view model.</span></span> <span data-ttu-id="d3b4b-111">Für die andere formeingaben die Werte sind eine Datenbindung an die `newBook` -Eigenschaft des Ansichtsmodells.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-111">For the other form inputs, the values are data-bound to the `newBook` property of the view model.</span></span>

<span data-ttu-id="d3b4b-112">Der Submit-Handler für das Formular gebunden ist, um die `addBook` Funktion:</span><span class="sxs-lookup"><span data-stu-id="d3b4b-112">The submit handler on the form is bound to the `addBook` function:</span></span>

[!code-html[Main](part-9/samples/sample4.html)]

<span data-ttu-id="d3b4b-113">Die `addBook` Funktion liest die aktuellen Werten der datengebundenen Formulareingaben für ein jsonobjekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-113">The `addBook` function reads the current values of the data-bound form inputs to create a JSON object.</span></span> <span data-ttu-id="d3b4b-114">Und es sich um das JSON-Objekt, das zurücksendet `/api/books`.</span><span class="sxs-lookup"><span data-stu-id="d3b4b-114">Then it POSTs the JSON object to `/api/books`.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3b4b-115">[Zurück](part-8.md)
> [Weiter](part-10.md)</span><span class="sxs-lookup"><span data-stu-id="d3b4b-115">[Previous](part-8.md)
[Next](part-10.md)</span></span>
