---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Hinzufügen eines Controllers (c#) | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Serivice Pack 1, welche i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 963d3bbbadf408d7045c50bfd693069e4097d45d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868294"
---
<a name="adding-a-controller-c"></a>Hinzufügen eines Controllers (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.
> 
> 
> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist zu diesem Thema steht zur Verfügung. [Die C#-Version herunterladen](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) dieses Lernprogramm.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut entworfen und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- Domänencontroller: Klassen, die Behandlung eingehender Anforderungen an die Anwendung, Abrufen von Daten des Modells aus, und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Client zurückgeben.
- Modelle: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- Ansichten:-Vorlagendateien, die von der Anwendung zum dynamischen Generieren von HTML-Antworten verwendeten.

Wir werden all diese Konzepte in diesem Lernprogramm Reihe abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.

Beginnen wir mit eine Controllerklasse erstellen. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Nennen Sie Ihre neue Controller "HelloWorldController". Lassen Sie die Standardvorlage als **leerer Controller** , und klicken Sie auf **hinzufügen**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Beachten Sie im **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurden *HelloWorldController.cs*. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image5.png)

Innerhalb der `public class HelloWorldController` blockieren, erstellen Sie zwei Methoden, die den folgenden Code ähneln. Der Controller wird als Beispiel eine HTML-Zeichenfolge zurück.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Der Domänencontroller heißt `HelloWorldController` und Sie heißt die erste Methode, die oben genannten `Index`. Nehmen wir in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser "HelloWorld" an den Pfad in die Adressleiste ein. (Z. B. in der Abbildung unten, dessen `http://localhost:43246/HelloWorld.`) die Seite im Browser wird das folgende Bildschirmfoto aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie mitgeteilt, dass das System auf HTML-Code zurückgeben, und es wurde!

![](adding-a-controller/_static/image6.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an. Standardlogik für die Zuordnung von ASP.NET MVC verwendet verwendet ein Format wie folgt, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die Controllerklasse ausgeführt. Daher */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, das wir durchsuchen musste */HelloWorld* und die `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge „This is the Welcome action method...“ zurück. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image7.png)

Nehmen wir das Beispiel etwas so ändern, dass einige Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die C#-optionalen Parameter-Funktion verwendet, um anzugeben, dass die `numTimes` Parameter sollte auf 1 Standard, falls kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Das System wird automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode zugeordnet.

![](adding-a-controller/_static/image8.png)

In beiden Beispielen der Controller den "VC" Teil MVC ausführen wurde – d. h. die Ansicht und Controller Arbeit. Der Controller ist HTML direkt zurückgeben. In der Regel soll nicht Controller HTML direkt zurückgeben, da, die sehr aufwendig, Code wird. Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren. Sehen wir uns an, wie wir dies weiter.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
