---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: "Hinzufügen eines Controllers | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: c8f317b2ac133f560461917af1588b7a1fa51c4f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut entworfen, einfach zu testender und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- **M** Odels: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- **V** Iews: Vorlagendateien von der Anwendung zum dynamischen Generieren von HTML-Antworten verwendeten.
- **C** Ontrollers: Klassen, die Behandlung eingehender Browseranforderungen, Abrufen von Daten des Modells, und geben Sie Vorlagen anzeigen, die eine Antwort an den Browser zurückgibt.

Wir werden all diese Konzepte in diesem Lernprogramm Reihe abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.

> [!NOTE]
> Im vorherigen Schritt die Standard-MVC wurde die Vorlage ausgewählt. Dies erstellt Home, Konto und Controller standardmäßig verwalten.

Beginnen wir mit eine Controllerklasse erstellen. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.


![](adding-a-controller/_static/image1.png)

In der **Gerüst hinzufügen** (Dialogfeld), klicken Sie auf **MVC 5-Controller - leere**, und klicken Sie dann auf **hinzufügen**.

![](adding-a-controller/_static/image2.png)  
 

Nennen Sie Ihre neue Controller "HelloWorldController", und klicken Sie auf **hinzufügen**.

![Hinzufügen eines Controllers](adding-a-controller/_static/image3.png)

Beachten Sie im **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurden *HelloWorldController.cs* und einen neuen Ordner *Views\HelloWorld*. Der Controller ist in der IDE geöffnet.

![](adding-a-controller/_static/image4.png)

Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Controllermethoden werden als Beispiel eine HTML-Zeichenfolge zurück. Der Domänencontroller heißt `HelloWorldController` und die erste Methode ist mit dem Namen `Index`. Nehmen wir in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird das folgende Bildschirmfoto aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie mitgeteilt, dass das System auf HTML-Code zurückgeben, und es wurde!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an. Die Standard-URL Routinglogik von ASP.NET MVC verwendet ein Format wie folgt, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Legen Sie das Format für das routing in der *App\_Start/RouteConfig.cs* Datei.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Wenn Sie die Anwendung auszuführen, und Sie keine URL-Segmente geben, wird standardmäßig die "Home" Controller und die Aktionsmethode "Index" im Abschnitt "Standardwerte" des obigen Codes angegeben.

Der erste Teil der URL bestimmt die Controllerklasse ausgeführt. Daher */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, das wir durchsuchen musste */HelloWorld* und die `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist. Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten. Wir sehen Routendaten weiter unten in diesem Lernprogramm.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Willkommensseite Aktionsmethode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image6.png)

Nehmen wir das Beispiel etwas so ändern, dass einige Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die C#-optionalen Parameter-Funktion verwendet, um anzugeben, dass die `numTimes` Parameter sollte auf 1 Standard, falls kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Sicherheitshinweis: Der Code oben verwendet [HttpUtility.HtmlEncode durchführen](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) zum Schützen der Anwendung von böswillige Eingaben (d. h. "JavaScript"). Weitere Informationen finden Sie unter [How to: Protect Against Script Exploits in einer Web-Anwendung durch Anwenden von HTML-Codierung in Zeichenfolgen](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Die [Bindungssystem für ASP.NET MVC-Modell](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image7.png)

Im Beispiel oben das URL-Segment ( `Parameters`) nicht verwendet wird, die `name` und `numTimes` Parameter werden als übergeben [Abfragezeichenfolgen](http://en.wikipedia.org/wiki/Query_string). Das Platzhalterzeichen ? (Fragezeichen) in der obigen URL ein Trennzeichen ist, und befolgen Sie die Abfragezeichenfolgen an Sie. Das Zeichen &amp; trennt Abfragezeichenfolgen.

Ersetzen Sie die Willkommensseite Methode durch den folgenden Code ein:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Führen Sie die Anwendung, und geben Sie die folgende URL:`http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Dieses Mal das dritte URL-Segment abgeglichen routenparameters `ID.` der `Welcome` Aktionsmethode enthält einen Parameter (`ID`), die mit der URL-Spezifikation in der `RegisterRoutes` Methode.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

In ASP.NET MVC-Anwendungen ist es besser in der Regel als Routendaten (z. B. wir mit der ID oben haben) als Übergabe als Abfragezeichenfolgen Parameter zu übergeben. Sie können auch eine Route zum Übergeben von sowohl Hinzufügen der `name` und `numtimes` in Parametern wie in der URL-Routendaten. In der *App\_Start\RouteConfig.cs* Datei, die "Hello"-Route hinzufügen:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Führen Sie die Anwendung, und navigieren Sie zu `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Viele MVC-Anwendungen auf funktioniert die Standardroute. Erfahren Sie weiter unten in diesem Lernprogramm aus, um Daten mit den Modellbinder übergeben, und müssen nicht die Standardroute für diese zu ändern.

In diesen Beispielen wurde der Controller auf diese Weise wurde das &quot;VC&quot; Teil MVC –, also die Ansicht und Controller-Aufgaben. Der Controller ist HTML direkt zurückgeben. In der Regel soll nicht Controller HTML direkt zurückgeben, da, die sehr aufwendig, Code wird. Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren. Sehen wir uns an, wie wir dies weiter.

>[!div class="step-by-step"]
[Zurück](getting-started.md)
[Weiter](adding-a-view.md)
