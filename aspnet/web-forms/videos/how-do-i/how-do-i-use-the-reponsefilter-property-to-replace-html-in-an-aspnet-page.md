---
uid: web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
title: '[Gewusst wie:] Mithilfe der Eigenschaft Reponse.Filter HTML auf einer ASP.NET-Seite ersetzen | Microsoft Docs'
author: rick-anderson
description: In diesem video Chris Pels zeigt, wie die Eigenschaft Reponse.Filter abfangen und ändern den HTML-Code an eine Seite gesendet werden. Zuerst wird eine Beispielseite w erstellt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/29/2009
ms.topic: article
ms.assetid: 3e5ae74a-9798-47d8-a2b3-0d8ad42dd4bc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page
msc.type: video
ms.openlocfilehash: 3e7c1f2a947d185d7c2a01deb75e42c92ba914c3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26525529"
---
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a><span data-ttu-id="8d46d-104">[Gewusst wie:] Verwenden Sie die Reponse.Filter-Eigenschaft, um HTML-Code in einer ASP.NET-Seite zu ersetzen</span><span class="sxs-lookup"><span data-stu-id="8d46d-104">[How Do I:] Use the Reponse.Filter Property to Replace HTML in an ASP.NET Page</span></span>
====================
<span data-ttu-id="8d46d-105">durch [Chris PEL-Spareinlagen](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="8d46d-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="8d46d-106">In diesem video Chris Pels zeigt, wie die Eigenschaft Reponse.Filter abfangen und ändern den HTML-Code an eine Seite gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="8d46d-106">In this video Chris Pels shows how to use the Reponse.Filter property to intercept and alter the HTML being sent to a page.</span></span> <span data-ttu-id="8d46d-107">Zuerst wird eine Beispielseite mit einigen einfachen Text erstellt.</span><span class="sxs-lookup"><span data-stu-id="8d46d-107">First, a sample page is created with some simple text.</span></span> <span data-ttu-id="8d46d-108">Anschließend wird eine benutzerdefinierte Streamklasse erstellt dient als Ersatz-Datenstrom für den aktuellen Stream, der an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="8d46d-108">Then, a custom Stream class is created which serves as the replacement stream for the current stream being sent to the user's browser.</span></span> <span data-ttu-id="8d46d-109">In dieser benutzerdefinierten Stream-Klasse werden der Inhalt der Seite aus dem Stream, geändert, und klicken Sie dann in den Datenstrom geschrieben abgerufen.</span><span class="sxs-lookup"><span data-stu-id="8d46d-109">In that custom stream class the contents of the page are retrieved from the stream, altered, and then written out to the response stream.</span></span> <span data-ttu-id="8d46d-110">In dieser benutzerdefinierten Stream-Klasse wird die Write-Methode angepasst ersetzen Sie den HTML-Code in der Antwort basisdatenstrom so zu ändern, was an den Browser des Benutzers gesendet wird.</span><span class="sxs-lookup"><span data-stu-id="8d46d-110">In this custom Stream class the Write method is customized to replace the HTML in the base Response stream, thereby altering what is sent to the user's browser.</span></span> <span data-ttu-id="8d46d-111">Schließlich wird das neue Stream-Klasse für die Eigenschaft "Response.Filter" auf der Seite zugewiesen\_Load-Ereignis, wodurch den Mechanismus zum Ändern des Seiteninhalts bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="8d46d-111">Finally, the new stream class is assigned to the Response.Filter property in the Page\_Load event, thereby, providing the mechanism for altering the page content.</span></span>

[<span data-ttu-id="8d46d-112">&#9654; Sehen Sie sich an (13 Minuten)</span><span class="sxs-lookup"><span data-stu-id="8d46d-112">&#9654; Watch video (13 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
