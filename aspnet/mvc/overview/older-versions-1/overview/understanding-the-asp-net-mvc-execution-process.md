---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Grundlegendes zu ASP.NET MVC-Ausführungsprozess | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie das ASP.NET MVC-Framework eine Browseranforderung schrittweise verarbeitet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 5837c6e49709d6b86ee52cd88ffd4759c1850544
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500729"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Grundlegendes zu ASP.NET MVC-Ausführungsprozess
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie das ASP.NET MVC-Framework eine Browseranforderung schrittweise verarbeitet.


Anforderungen an eine ASP.NET-MVC-basierte Webanwendung zuerst pass-through-die **UrlRoutingModule fest** Objekt, das ein HTTP-Modul ist. Dieses Modul analysiert die Anforderung und führt die Routenauswahl aus. Die **UrlRoutingModule fest** Objekt wählt das erste Routenobjekt aus, das der aktuellen Anforderung übereinstimmt. (Ein Routenobjekt ist eine Klasse, die implementiert **RouteBase**, und ist in der Regel eine Instanz der **Route** Klasse.) Wenn keine übereinstimmende Route gefunden, die **UrlRoutingModule fest** Objekt hat keine Auswirkung, und die Anforderung an die reguläre ASP.NET- oder IIS-Anforderung verarbeitetes zurückgreifen können.

Aus dem ausgewählten **Route** -Objekt, das **UrlRoutingModule fest** -Objekt abruft der **IRouteHandler** -Objekt, das zugeordnet ist die **Route**Objekt. In der Regel in einer MVC-Anwendung, dies wird jeweils eine Instanz des **MvcRouteHandler**. Die **IRouteHandler** Instanz erstellt ein **IHttpHandler** -Objekt und übergibt es die **IHttpContext** Objekt. Wird standardmäßig die **IHttpHandler** -Instanz für MVC ist der **MvcHandler** Objekt. Die **MvcHandler** Objekt wählt dann den Controller, der die Anforderung letztlich behandelt.

> [!NOTE]
> Wenn eine ASP.NET-MVC-Webanwendung in IIS 7.0 ausgeführt wird, ist keine Dateinamenerweiterung für MVC-Projekte erforderlich. In IIS 6.0 erfordert der Handler jedoch, dass Sie die ISAPI-DLL von ASP.NET die Dateinamenerweiterung .mvc zuordnen.


Das Modul und die Ereignishandler sind die Einstiegspunkte für ASP.NET MVC-Framework. Sie können die folgenden Aktionen ausführen:

- Auswählen des richtigen Controllers in einer MVC-Webanwendung.
- Abrufen einer bestimmten Controllerinstanz.
- Rufen Sie des Controllers **Execute** Methode.

Im folgenden werden die verschiedenen Ausführungsstufen für ein MVC-Webprojekt aufgeführt:

- Empfangen der ersten Anforderung für die Anwendung 

    - In der Datei "Global.asax" **Route** Objekte werden hinzugefügt, um die **RouteTable** Objekt.
- Routing ausführen 

    - Die **UrlRoutingModule fest** -Modul verwendet die erste übereinstimmende **Route** Objekt in der **RouteTable** Auflistung zum Erstellen der **RouteData** -Objekt, das es dann zum Erstellen verwendet einer **RequestContext** (**IHttpContext**) Objekt.
- Erstellen von MVC-anforderungshandlers 

    - Die **MvcRouteHandler** Objekt erstellt eine Instanz der **MvcHandler** -Klasse und übergibt sie die **RequestContext** Instanz.
- Erstellen in der Regel 

    - Die **MvcHandler** -Objekt verwendet die **RequestContext** Instanz zum Identifizieren der **IControllerFactory** Objekt (i. d. r. eine Instanz von der  **DefaultControllerFactory** Klasse) auf die Controllerinstanz erstellt.
- Execute-Controller: die **MvcHandler** -Instanz aufruft, den Controller s **Execute** Methode. |
- Aktion aufrufen 

    - Die meisten Domänencontroller Vererben der **Controller** Basisklasse. Für Domänencontroller, auf denen dazu die **ControllerActionInvoker** Objekt, mit dem Controller zugeordnet ist, bestimmt Aktionsmethode der Controllerklasse aufrufen, und ruft diese Methode anschließend.
- Ausführen des Ergebnisses 

    - Eine typische Aktionsmethode möglicherweise eine Benutzereingabe empfangen, bereiten Sie die entsprechenden Antwortdaten vor und führen Sie dann das Ergebnis, indem ein Ergebnistyp zurückgegeben. Die integrierten Ergebnistypen, die ausgeführt werden können, umfassen Folgendes: **ViewResult** (die rendert eine Ansicht und ist der häufigsten verwendete Ergebnistyp), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, und **EmptyResult**.
