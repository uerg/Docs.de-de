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
<a name="how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page"></a>[Gewusst wie:] Verwenden Sie die Reponse.Filter-Eigenschaft, um HTML-Code in einer ASP.NET-Seite zu ersetzen
====================
durch [Chris PEL-Spareinlagen](https://twitter.com/chrispels)

In diesem video Chris Pels zeigt, wie die Eigenschaft Reponse.Filter abfangen und ändern den HTML-Code an eine Seite gesendet werden. Zuerst wird eine Beispielseite mit einigen einfachen Text erstellt. Anschließend wird eine benutzerdefinierte Streamklasse erstellt dient als Ersatz-Datenstrom für den aktuellen Stream, der an den Browser des Benutzers gesendet wird. In dieser benutzerdefinierten Stream-Klasse werden der Inhalt der Seite aus dem Stream, geändert, und klicken Sie dann in den Datenstrom geschrieben abgerufen. In dieser benutzerdefinierten Stream-Klasse wird die Write-Methode angepasst ersetzen Sie den HTML-Code in der Antwort basisdatenstrom so zu ändern, was an den Browser des Benutzers gesendet wird. Schließlich wird das neue Stream-Klasse für die Eigenschaft "Response.Filter" auf der Seite zugewiesen\_Load-Ereignis, wodurch den Mechanismus zum Ändern des Seiteninhalts bereitstellen.

[&#9654; Sehen Sie sich an (13 Minuten)](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-use-the-reponsefilter-property-to-replace-html-in-an-aspnet-page)
