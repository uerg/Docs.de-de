---
uid: mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
title: 'Vorgehensweise erstellen: ein benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung? | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes HtmlHelper zu erstellen, die in den Standardsatz in einer MVC-Anwendung nicht verfügbar ist. Erste, eine Beispiel-MVC-Anwendung...
ms.author: riande
ms.date: 12/11/2009
ms.assetid: 58b5eb15-4160-4ce2-ae70-6ba94262ea73
msc.legacyurl: /mvc/videos/mvc-2/how-do-i/how-do-i-create-a-custom-html-helper-for-an-mvc-application
msc.type: video
ms.openlocfilehash: 4061c06cfeab2278e5732295b034f81f7995c2a4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833928"
---
<a name="how-do-i-create-a-custom-html-helper-for-an-mvc-application"></a><span data-ttu-id="79ba2-105">Vorgehensweise erstellen: ein benutzerdefinierten HTML-Hilfsprogramms für eine MVC-Anwendung?</span><span class="sxs-lookup"><span data-stu-id="79ba2-105">How Do I: Create a Custom HTML Helper for an MVC Application?</span></span>
====================
<span data-ttu-id="79ba2-106">durch [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="79ba2-106">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="79ba2-107">In diesem Video zeigt Chris Pels, wie ein benutzerdefiniertes HtmlHelper zu erstellen, die in den Standardsatz in einer MVC-Anwendung nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="79ba2-107">In this video Chris Pels shows how to create a custom HtmlHelper that is not available in the standard set in an MVC application.</span></span> <span data-ttu-id="79ba2-108">Zunächst wird eine MVC-Anwendung mit einer Demo-Controller und eine Ansicht zum Testen der benutzerdefinierten HtmlHelper erstellt.</span><span class="sxs-lookup"><span data-stu-id="79ba2-108">First, a sample MVC application is created with a demo controller and view to test the custom HtmlHelper.</span></span> <span data-ttu-id="79ba2-109">Als Nächstes wird ein Modul mit einer öffentlichen Funktion erstellt, die eine Erweiterungsmethode ist, die die Implementierung von benutzerdefinierten HtmlHelper darstellt.</span><span class="sxs-lookup"><span data-stu-id="79ba2-109">Next, a module is created with a public function that is an extension method which represents the implementation of the custom HtmlHelper.</span></span> <span data-ttu-id="79ba2-110">Die benutzerdefinierten Hilfsmethoden ist zum Erstellen von `<img>` tags auf einer Seite, und mehrere eingehende Parameter, einschließlich Id, Url und Alt-Text für das Image-Tag empfängt.</span><span class="sxs-lookup"><span data-stu-id="79ba2-110">The custom helper is for creating `<img>` tags in a page and receives several inbound parameters including the id, url, and alt text for the image tag.</span></span> <span data-ttu-id="79ba2-111">Die Logik wird dann an die Funktion für die Rückgabe der abgeschlossenen hinzugefügt `<img>` Tag mit den angegebenen Informationen.</span><span class="sxs-lookup"><span data-stu-id="79ba2-111">The logic is then added to the function for returning the completed `<img>` tag with the specified information.</span></span> <span data-ttu-id="79ba2-112">Anschließend wird benutzerdefiniertes HtmlHelper auf der Demoseite verwendet, um ein Bild anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="79ba2-112">Then the custom HtmlHelper is used on the demo page to display an image.</span></span> <span data-ttu-id="79ba2-113">Schließlich wird benutzerdefiniertes HtmlHelper erweitert, um mehrere Konstruktor Außerkraftsetzungen enthalten, die Flexibilität ermöglichen, weitere leichte Erstellen von anderen `<img>` Tags.</span><span class="sxs-lookup"><span data-stu-id="79ba2-113">Finally, the custom HtmlHelper is expanded to include multiple constructor overrides which provide flexibility for more easily creating different `<img>` tags.</span></span>

[<span data-ttu-id="79ba2-114">&#9654;Sehen Sie sich Video (18 Minuten)</span><span class="sxs-lookup"><span data-stu-id="79ba2-114">&#9654; Watch video (18 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-create-a-custom-html-helper-for-an-mvc-application)

> [!div class="step-by-step"]
> <span data-ttu-id="79ba2-115">[Zurück](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
> [Weiter](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="79ba2-115">[Previous](how-do-i-implement-view-models-to-manage-data-for-aspnet-mvc-views.md)
[Next](how-do-i-work-with-model-binders-in-an-mvc-application.md)</span></span>
