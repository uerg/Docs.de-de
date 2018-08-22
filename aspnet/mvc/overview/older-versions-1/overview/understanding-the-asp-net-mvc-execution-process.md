---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Grundlegendes zu den ASP.NET MVC-Ausführungsprozess | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie ASP.NET MVC-Framework eine schrittweise Browseranforderung verarbeitet.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 3a3bcf4b78e3b19fb4d293f67b68c3a790d05221
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830296"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Grundlegendes zu den ASP.NET MVC-Ausführungsprozess
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie ASP.NET MVC-Framework eine schrittweise Browseranforderung verarbeitet.


Anforderungen an eine ASP.NET-MVC-basierte Webanwendung zunächst pass-through-die **UrlRoutingModule fest** Objekt, das ein HTTP-Modul ist. Dieses Modul analysiert die Anforderung und führt die Routenauswahl aus. Die **UrlRoutingModule fest** -Objekt wählt das erste Routenobjekt aus, das der aktuellen Anforderung übereinstimmt. (Ein Routenobjekt ist eine Klasse, die implementiert **RouteBase**, und ist in der Regel eine Instanz der **Route** Klasse.) Wenn keine übereinstimmende Route gefunden, die **UrlRoutingModule fest** Objekt hat keine Auswirkungen, und gibt die Anforderung, die auf die reguläre ASP.NET- oder IIS-Anforderung verarbeiteten zurückgreifen.

Aus dem ausgewählten **Route** -Objekt, das **UrlRoutingModule fest** -Objekt abruft der **IRouteHandler** -Objekt, das zugeordnet ist die **Route**Objekt. In der Regel in einer MVC-Anwendung, dies wird jeweils eine Instanz des **MvcRouteHandler**. Die **IRouteHandler** Instanz erstellt ein **IHttpHandler** -Objekt und übergibt es die **IHttpContext** Objekt. In der Standardeinstellung die **IHttpHandler** -Instanz für MVC ist die **MvcHandler** Objekt. Die **MvcHandler** -Objekt wählt dann den Controller, der die Anforderung letztlich behandelt.

> [!NOTE]
> Wenn Sie eine ASP.NET MVC-Web-Anwendung in IIS 7.0 ausgeführt wird, ist keine Dateinamenerweiterung für MVC-Projekten erforderlich. In IIS 6.0 erfordert der Handler jedoch, dass Sie die ISAPI-DLL von ASP.NET die Dateinamenerweiterung .mvc zuordnen.


Das Modul und die Ereignishandler sind die Einstiegspunkte für ASP.NET MVC-Framework. Sie können die folgenden Aktionen ausführen:

- Auswählen des richtigen Controllers in einer MVC-Webanwendung.
- Abrufen einer bestimmten Controllerinstanz.
- Aufrufen des Controllers **Execute** Methode.

Im folgenden werden die verschiedenen Ausführungsstufen für ein MVC-Webprojekt aufgeführt:

- Empfangen der ersten Anforderung für die Anwendung 

    - In der Datei "Global.asax" **Route** Objekte werden hinzugefügt, um die **RouteTable** Objekt.
- Führen Sie routing 

    - Die **UrlRoutingModule fest** Modul verwendet das erste übereinstimmende **Route** -Objekt in der **RouteTable** zu erstellende Auflistung der **RouteData** -Objekt, das er zum Erstellen verwendet einer **RequestContext** (**IHttpContext**) Objekt.
- Erstellen von MVC-anforderungshandlers 

    - Die **MvcRouteHandler** Objekt erstellt eine Instanz der **MvcHandler** -Klasse und übergibt sie der **RequestContext** Instanz.
- Erstellen Sie controller 

    - Der **MvcHandler** -Objekt verwendet die **RequestContext** Instanz zum Identifizieren der **IControllerFactory** Objekt (i. d. r. eine Instanz von der  **DefaultControllerFactory** Klasse) zum Erstellen von die Controllerinstanz.
- Execute-Controller – die **MvcHandler** Instanz ruft den Controller s **Execute** Methode. |
- Aktion aufrufen 

    - Die meisten Controller erben von der **Controller** Basisklasse. Für Controller, die dazu die **ControllerActionInvoker** Objekt, mit dem Controller zugeordnet ist, bestimmt, welche Aktionsmethode der Controllerklasse, die aufgerufen und ruft dann die Methode.
- Ausführen des Ergebnisses 

    - Eine typische Aktionsmethode möglicherweise eine Benutzereingabe empfangen, bereiten Sie die entsprechenden Antwortdaten vor und führen Sie dann das Ergebnis, indem ein Ergebnistyp zurückgegeben. Die integrierten Ergebnistypen, die ausgeführt werden können, umfassen Folgendes: **ViewResult** (die rendert eine Ansicht und ist der häufigsten verwendete Ergebnistyp), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, und **EmptyResult**.
