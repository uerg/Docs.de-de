---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Gewusst wie:] Verwenden Sie der Reponse.Filter-Eigenschaft, um HTML-Code in eine ASP.NET-Seite ersetzen | Microsoft-Dokumentation'
author: rick-anderson
description: In diesem video Chris Pels veranschaulicht, wie die Reponse.Filter-Eigenschaft zum Abfangen und ändern den HTML-Code auf eine Seite gesendet werden. Zuerst wird eine Beispielseite w erstellt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: f871a3969da21a22bfe10e3a85f43db3e0e522e8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382360"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="24f5a-104">[Gewusst wie:] Mithilfe der Reponse.Filter-Eigenschaft, um HTML-Code in eine ASP.NET-Seite zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="24f5a-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="24f5a-105">durch [Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="24f5a-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="24f5a-106">In diesem video Chris Pels veranschaulicht, wie die Reponse.Filter-Eigenschaft zum Abfangen und ändern den HTML-Code auf eine Seite gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="24f5a-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="24f5a-107">Zunächst wird eine Beispielseite mit einigen einfachen Text erstellt.</span><span class="sxs-lookup"><span data-stu-id="24f5a-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="24f5a-108">Anschließend wird eine benutzerdefinierte Stream-Klasse erstellt, der dient, wie der Stream Ersatz für den aktuellen Stream, der an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="24f5a-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="24f5a-109">In dieser Klasse abgeleiteten Streamklasse werden der Inhalt der Seite aus dem Stream verändert, und klicken Sie dann in den Antwortstream geschrieben abgerufen.</span><span class="sxs-lookup"><span data-stu-id="24f5a-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="24f5a-110">In dieser benutzerdefinierte Stream-Klasse wird die Write-Methode angepasst. dazu ersetzen Sie den HTML-Code in den Basis-Datenstrom, und ändern, was an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="24f5a-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="24f5a-111">Schließlich wird der neue Stream-Klasse, die Eigenschaft "Response.Filter" auf der Seite zugewiesen\_Load-Ereignis, und dabei den Mechanismus zum Ändern der Inhalt der Seite bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="24f5a-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="24f5a-112">&#9654;Sehen Sie sich Video (13 Minuten)</span><span class="sxs-lookup"><span data-stu-id="24f5a-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
