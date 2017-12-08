---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: "Hinzufügen eines Controllers | Microsoft Docs"
author: Rick-Anderson
description: "Hinweis: Eine aktualisierte Version dieses Lernprogramms ist hier verfügbar, die ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher zu verfolgen und demo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 69af91401e51470fbc0b67103345325201b06723
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Lernprogramms steht [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist sicherer, viel einfacher, führen und weitere Funktionen veranschaulicht.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut entworfen, einfach zu testender und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- **M** Odels: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- **V** Iews: Vorlagendateien von der Anwendung zum dynamischen Generieren von HTML-Antworten verwendeten.
- **C** Ontrollers: Klassen, die Behandlung eingehender Browseranforderungen, Abrufen von Daten des Modells, und geben Sie Vorlagen anzeigen, die eine Antwort an den Browser zurückgibt.

Wir werden all diese Konzepte in diesem Lernprogramm Reihe abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.

Beginnen wir mit eine Controllerklasse erstellen. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**.

![](adding-a-controller/_static/image1.png)

Nennen Sie Ihre neue Controller &quot;HelloWorldController&quot;. Lassen Sie die Standardvorlage als **leerer MVC-Controller** , und klicken Sie auf **hinzufügen**.

![Hinzufügen eines Controllers](adding-a-controller/_static/image2.png)

Beachten Sie im **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurden *HelloWorldController.cs*. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image3.png)

Ersetzen Sie den Inhalt der Datei durch den folgenden Code ein.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Controllermethoden werden als Beispiel eine HTML-Zeichenfolge zurück. Der Domänencontroller heißt `HelloWorldController` und Sie heißt die erste Methode, die oben genannten `Index`. Nehmen wir in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird das folgende Bildschirmfoto aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie mitgeteilt, dass das System auf HTML-Code zurückgeben, und es wurde!

![](adding-a-controller/_static/image4.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an. Die Standard-URL Routinglogik von ASP.NET MVC verwendet ein Format wie folgt, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die Controllerklasse ausgeführt. Daher */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, das wir durchsuchen musste */HelloWorld* und die `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Willkommensseite Aktionsmethode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image5.png)

Nehmen wir das Beispiel etwas so ändern, dass einige Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die C#-optionalen Parameter-Funktion verwendet, um anzugeben, dass die `numTimes` Parameter sollte auf 1 Standard, falls kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Die [Bindungssystem für ASP.NET MVC-Modell](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image6.png)

In beiden Beispielen wurde der Controller auf diese Weise wurde das &quot;VC&quot; Teil MVC –, also die Ansicht und Controller-Aufgaben. Der Controller ist HTML direkt zurückgeben. In der Regel soll nicht Controller HTML direkt zurückgeben, da, die sehr aufwendig, Code wird. Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren. Sehen wir uns an, wie wir dies weiter.

>[!div class="step-by-step"]
[Zurück](intro-to-aspnet-mvc-4.md)
[Weiter](adding-a-view.md)
