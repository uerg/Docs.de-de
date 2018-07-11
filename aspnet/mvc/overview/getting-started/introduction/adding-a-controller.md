---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 77389bfa4774857eb2a607b0a70e982826312e03
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "38119262"
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut strukturiert, getestet und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- **M** odelle: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- **V** Iews: Vorlagendateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- **C** ontroller: Klassen, die eingehenden Browseranforderungen verarbeiten rufen Modelldaten und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Browser zurückgegeben.

Wir werden all diese Konzepte in dieser tutorialreihe abdecken und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.

> [!NOTE]
> Im vorherigen Schritt des Standard-MVC wurde die Vorlage ausgewählt. Dies erstellt Home-Konto und die Controller verwalten, standardmäßig.

Wir erstellen zunächst eine Controller-Klasse. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und klicken Sie dann auf **hinzufügen**, klicken Sie dann **Controller**.


![](adding-a-controller/_static/image1.png)

In der **Gerüst hinzufügen** Dialogfeld klicken Sie auf **MVC 5 Controller - leer**, und klicken Sie dann auf **hinzufügen**.

![](adding-a-controller/_static/image2.png)  
 

Geben Sie Ihre neuen Controller den Namen "HelloWorldController", und klicken Sie auf **hinzufügen**.

![Controller hinzufügen](adding-a-controller/_static/image3.png)

Beachten Sie, dass **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurde *HelloWorldController.cs* und einen neuen Ordner *Views\HelloWorld*. Der Controller ist in der IDE geöffnet.

![](adding-a-controller/_static/image4.png)

Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Kontroller-Methoden gibt eine Zeichenfolge mit HTML als Beispiel zurück. Der Controller wird mit dem Namen `HelloWorldController` und die erste Methode heißt `Index`. Lassen Sie uns in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird wie im folgenden Screenshot aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie sagte, dass das System nur HTML-Code zurückgeben, und dies der Fall war.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL an. Die URL-Routinglogik auf Standard, die ASP.NET MVC verwendet ein Format wie dieses, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Sie legen Sie das Format für das routing in der *App\_Start/RouteConfig.cs* Datei.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Wenn Sie die Anwendung auszuführen, und Sie keine URL-Segmente geben, wird standardmäßig mit dem Controller "Home", und die Aktionsmethode "Index" angegeben wird, in dem Abschnitt "Defaults" des obigen Codes.

Der erste Teil der URL bestimmt die auszuführende Controllerklasse. Also */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Also */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, die wir mussten nur durchsuchen */HelloWorld* und `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist. Der dritte Teil des URL-Segments (`Parameters`) ist für Routendaten. Weiterleiten von Daten dass später in diesem Tutorial sehen.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Welcome Action-Methode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image6.png)

Lassen Sie uns das Beispiel etwas so ändern, dass Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die c#-Funktion optional-Parameter verwendet, um anzugeben, dass die `numTimes` Parameter sollte der Standardwert 1, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Sicherheitshinweis: Der Code oben verwendet [HttpUtility.HtmlEncode durchführen](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) zum Schützen der Anwendung vor schädlicher Eingaben (über JavaScript). Weitere Informationen finden Sie unter [How to: Protect Against Script Exploits in einer Webanwendung durch Anwenden von HTML-Codierung in Zeichenfolgen](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Die [ASP.NET MVC-modellbindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image7.png)

Im Beispiel oben das URL-Segment ( `Parameters`) nicht verwendet wird, die `name` und `numTimes` als Parameter übergebene [Abfragezeichenfolgen](http://en.wikipedia.org/wiki/Query_string). Das Platzhalterzeichen ? (Fragezeichen) in der obigen URL ist ein Trennzeichen aus, und das die Abfragezeichenfolgen folgen. Das Zeichen &amp; trennt Abfragezeichenfolgen.

Ersetzen Sie die willkommene-Methode, mit dem folgenden Code:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Führen Sie die Anwendung aus, und geben Sie die folgende URL: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Dieses Mal das dritte URL-Segment abgeglichen routenparameters `ID.` der `Welcome` Aktionsmethode enthält einen Parameter (`ID`), die mit der URL-Spezifikation in der `RegisterRoutes` Methode.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

In ASP.NET MVC-Anwendungen ist es üblicher, übergeben Sie Parameter als Routendaten (wie mit der ID oben) als Übergabe als Abfragezeichenfolgen. Sie könnten auch hinzufügen, eine Route, um beide übergeben die `name` und `numtimes` in Parametern wie in der URL-Routendaten. In der *App\_start\routeconfig* Datei, die "Hello"-Route hinzufügen:

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Führen Sie die Anwendung, und navigieren Sie zu `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Viele MVC-Anwendungen auf funktioniert die Standardroute. Erfahren Sie später in diesem Lernprogramm aus, um Daten mithilfe der modellbindung zu übergeben, und ändern Sie die Standardroute für, die keine.

In diesen Beispielen wurde der Controller Ausführen der &quot;VC&quot; -Teil von MVC – d. h. die Ansicht und Controller Arbeit. Der Controller gibt HTML direkt zurück. Normalerweise sollen nicht Controller HTML direkt zurückgeben, da dies sehr mühsam Code wird. Stattdessen wird in der Regel eine separate ansichtsvorlagendatei verwendet, um Hilfe, die HTML-Antwort zu generieren. Sehen wir uns weiter an, wie wir dies erreichen können.

> [!div class="step-by-step"]
> [Zurück](getting-started.md)
> [Weiter](adding-a-view.md)
