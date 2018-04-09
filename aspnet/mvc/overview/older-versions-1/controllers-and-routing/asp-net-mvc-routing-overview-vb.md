---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC-Routing Overview (VB) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm wird Stephen Walther gezeigt, wie das ASP.NET MVC-Framework Browseranforderungen Controlleraktionen zugeordnet.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 3de0e21552a4aa03aa21f21a4e26028f1475f3e9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-routing-overview-vb"></a>ASP.NET MVC-Routing Overview (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Lernprogramm wird Stephen Walther gezeigt, wie das ASP.NET MVC-Framework Browseranforderungen Controlleraktionen zugeordnet.


In diesem Lernprogramm werden Sie eine Einführung in ein wichtiges Feature von jeder ASP.NET MVC-Anwendung namens *ASP.NET-Routing*. Das ASP.NET-Routing-Modul ist verantwortlich für die Zuordnung von eingehenden Browseranforderungen zu bestimmten Aktionen von MVC-Controller. Am Ende dieses Lernprogramms können Sie verstehen, wie die standardmäßigen Routentabelle Controlleraktionen Anforderungen zugeordnet.

## <a name="using-the-default-route-table"></a>Mithilfe der Standardroute-Tabelle

Wenn Sie eine neue ASP.NET MVC-Anwendung erstellen, wird die Anwendung bereits ASP.NET-Routing Verwendung konfiguriert. ASP.NET-Routing funktioniert Setup an zwei Stellen.

ASP.NET-Routing ist zunächst in der Webkonfigurationsdatei (Datei "Web.config") für Ihre Anwendung aktiviert. Es gibt vier Abschnitte in der Konfigurationsdatei, die für routing relevant sind: Abschnitt system.web.httpModules, Abschnitt system.web.httpHandlers Abschnitt system.webserver.modules und Abschnitt system.webserver.handlers. Achten Sie darauf, dass Sie nicht in diesen Abschnitten gelöscht werden, da ohne diese Abschnitte routing nicht mehr verwendet werden.

Zweitens und wichtiger ist, wird eine Routingtabelle in die Datei "Global.asax" der Anwendung erstellt. Die Datei "Global.asax" ist eine spezielle Datei, die Ereignishandler für die Lebenszyklusereignisse von ASP.NET Anwendung enthält. Die Routingtabelle wird während des Ereignisses zum Starten der Anwendung erstellt.

Die Datei im Codebeispiel 1 enthält die standardmäßige Datei "Global.asax" einer ASP.NET MVC-Anwendung.

**Listing 1 - Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

Wenn eine MVC-Anwendung zuerst gestartet wird, die Anwendung\_Start()-Methode aufgerufen wird. Diese Methode ruft wiederum die RegisterRoutes()-Methode. Die RegisterRoutes()-Methode erstellt die Routentabelle.

Die Standard-Routingtabelle enthält eine einzelne Route (mit dem Namen "Default"). Die Standardroute ordnet das erste Segment einer URL Controllernamen, das zweite Segment der URL zu einem Controlleraktion und das dritte Segment, das einen Parameter namens **Id**.

Stellen Sie sich vor, dass Sie die folgende URL in die Adressleiste des Webbrowsers eingeben:

/Home/Index/3

Die Standardroute ordnet diese URL die folgenden Parameter:

- Controller = Home

- Aktion = Index

- id = 3

Wenn Sie die URL/Home/Index/3 anfordern, wird der folgende Code ausgeführt:

HomeController.Index(3)

Die Standardroute enthält Standardwerte für alle drei Parameter. Wenn Sie einen Domänencontroller nicht angeben, klicken Sie dann den Controller Parameter ist standardmäßig auf den Wert **Home**. Wenn Sie eine Aktion angeben, wird standardmäßig der Action-Parameter auf den Wert **Index**. Schließlich, wenn Sie eine Id angeben, wird standardmäßig der Id-Parameter auf eine leere Zeichenfolge.

Sehen wir uns einige Beispiele, wie die Standardroute URLs Controlleraktionen zugeordnet. Stellen Sie sich vor, dass Sie die folgende URL in der Adressleiste des Browsers eingeben:

/Home

Aufgrund der Standardeinstellung Route Parameterstandardwerte wird diese URL eingeben der Index()-Methode der HomeController-Klasse auflisten verursachen in 2 aufgerufen werden.

**Auflisten von 2 – HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

Auflisten von 2 umfasst HomeController-Klasse eine Methode namens Index(), die einen einzelnen Parameter mit dem Namen ID akzeptiert Die URL/Home bewirkt, dass die Index()-Methode, die mit dem Wert ' Nothing ' als Wert des Id-Parameters aufgerufen werden.

Aufgrund der Art und Weise, dass das MVC-Framework Controlleraktionen aufruft entspricht der URL/Home auch die Index()-Methode der HomeController-Klasse in 3 auflisten.

**Auflisten von 3 – HomeController.vb (Index-Aktion ohne Parameter)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

Die Methode Index() in 3 auflisten akzeptiert keine Parameter. Die URL/Home führt dazu, dass diese Index()-Methode aufgerufen werden. Die URL/Home/Index/3 ruft auch diese Methode (die Id wird ignoriert).

Die URL/Home entspricht auch der Index()-Methode der HomeController-Klasse in 4 auflisten.

**Auflisten von 4 – HomeController.vb (Index-Aktion mit NULL-Werte zulassen-Parameter)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

Auflisten von 4 hat die Index()-Methode einen ganzzahligen Parameter an. Da der Parameter ein NULL-Werte zulassen-Parameter ist (kann der Wert keine Aktion ausgeführt haben), kann die Index() aufgerufen werden, ohne einen Fehler auszulösen.

Schließlich Aufrufen der Methode Index() 5 auflisten, mit der URL/Home führt dazu, dass eine Ausnahme seit dem Id-Parameter *nicht* Parameter NULL-Werte zulässt. Wenn Sie versuchen, das Aufrufen der Methode Index() erhalten Sie den Fehler, die in Abbildung 1 angezeigt.

**Auflisten von 5 – HomeController.vb (Index-Aktion mit der Id-Parameter)**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![Eine Controlleraktion aufrufen, die einen Parameterwert erwartet](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**Abbildung 01**: Aufrufen einer Controlleraktion, die einen Parameterwert erwartet ([klicken Sie hier, um das Bild in voller Größe angezeigt](asp-net-mvc-routing-overview-vb/_static/image2.png))


URL/Home/Index/3 funktioniert andererseits, problemlos mit Index-Controlleraktion 5 auflisten. Die Anforderung /Home/Index/3 bewirkt, dass die Index()-Methode, die mit einem Id-Parameter aufgerufen werden, die den Wert 3 hat.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde eine kurze Einführung in ASP.NET-Routing stellen. Untersucht die Standard-Routingtabelle, die Sie durch eine neue ASP.NET MVC-Anwendung zu erhalten. Sie haben gelernt, wie die Standardroute URLs Controlleraktionen zugeordnet.

> [!div class="step-by-step"]
> [Zurück](creating-an-action-cs.md)
> [Weiter](understanding-action-filters-vb.md)
