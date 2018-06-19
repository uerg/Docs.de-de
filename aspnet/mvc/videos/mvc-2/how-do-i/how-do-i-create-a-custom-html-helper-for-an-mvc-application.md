---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: Wie erstelle ich ein benutzerdefiniertes HTML-Hilfsobjekt für eine MVC-Anwendung? | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie eine benutzerdefinierte HtmlHelper erstellen, die in den Standardsatz in einer MVC-Anwendung nicht verfügbar ist. Erste, eine Beispiel-MVC-Applica...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/11/2009
ms.topic: article
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 92faa04e1eefec0852604d51987ddaa9ee58838a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870101"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="394d9-105">Wie erstelle ich ein benutzerdefiniertes HTML-Hilfsobjekt für eine MVC-Anwendung?</span><span class="sxs-lookup"><span data-stu-id="394d9-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="394d9-106">durch [Chris PEL-Spareinlagen](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="394d9-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="394d9-107">In diesem Video zeigt Chris Pels, wie eine benutzerdefinierte HtmlHelper erstellen, die in den Standardsatz in einer MVC-Anwendung nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="394d9-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="394d9-108">Zuerst wird eine Beispiel-MVC-Anwendung mit einem Demo-Controller und Ansicht so testen Sie die benutzerdefinierte HtmlHelper erstellt.</span><span class="sxs-lookup"><span data-stu-id="394d9-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="394d9-109">Als Nächstes wird ein Modul mit einer öffentlichen Funktion erstellt, die die Implementierung des benutzerdefinierten HtmlHelper stellt eine Erweiterungsmethode wird.</span><span class="sxs-lookup"><span data-stu-id="394d9-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="394d9-110">Die benutzerdefinierten Hilfsmethoden ist zum Erstellen von `<img>` tags auf einer Seite, und mehrere eingehende Parameter, einschließlich Id, Url und Alt-Text für das Bildtag empfängt.</span><span class="sxs-lookup"><span data-stu-id="394d9-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="394d9-111">Die Logik wird dann an die Funktion für die Rückgabe des abgeschlossenen hinzugefügt `<img>` Tag mit den angegebenen Informationen.</span><span class="sxs-lookup"><span data-stu-id="394d9-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="394d9-112">Anschließend wird der benutzerdefinierten HtmlHelper auf der Seite "Demo" verwendet, um ein Bild anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="394d9-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="394d9-113">Schließlich wird die benutzerdefinierte HtmlHelper erweitert, um mehrere Konstruktor überschreibt einzubeziehen die Flexibilität ermöglichen, weitere problemlos erstellen unterschiedliche `<img>` Tags.</span><span class="sxs-lookup"><span data-stu-id="394d9-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="394d9-114">&#9654;Sehen Sie sich an (18 Minuten)</span><span class="sxs-lookup"><span data-stu-id="394d9-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="394d9-115">[Zurück](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Weiter](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="394d9-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
