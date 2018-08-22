---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 0267d31c-892f-49a1-9e7a-3ae8cc12b2ca
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 772f3c87d6b73b324164bf9619d332a9c01d7476
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829503"
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut strukturiert, getestet und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- **M** odelle: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- **V** Iews: Vorlagendateien, die Ihre Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- **C** ontroller: Klassen, die eingehenden Browseranforderungen verarbeiten rufen Modelldaten und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Browser zurückgegeben.

Wir werden all diese Konzepte in dieser tutorialreihe abdecken und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.

Wir erstellen zunächst eine Controller-Klasse. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**.

![](adding-a-controller/_static/image1.png)

Benennen Sie dem neuen Controller &quot;HelloWorldController&quot;. Lassen Sie die Standardvorlage als **leerer MVC-Controller** , und klicken Sie auf **hinzufügen**.

![Controller hinzufügen](adding-a-controller/_static/image2.png)

Beachten Sie, dass **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurde *HelloWorldController.cs*. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image3.png)

Ersetzen Sie den Inhalt der Datei mit dem folgenden Code ein.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Die Kontroller-Methoden gibt eine Zeichenfolge mit HTML als Beispiel zurück. Der Controller wird mit dem Namen `HelloWorldController` und die erste Methode, die oben genannten heißt `Index`. Lassen Sie uns in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Z. B. in der Abbildung unten, dessen `http://localhost:1234/HelloWorld.`) die Seite im Browser wird wie im folgenden Screenshot aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie sagte, dass das System nur HTML-Code zurückgeben, und dies der Fall war.

![](adding-a-controller/_static/image4.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL an. Die URL-Routinglogik auf Standard, die ASP.NET MVC verwendet ein Format wie dieses, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controllerklasse. Also */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Also */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, die wir mussten nur durchsuchen */HelloWorld* und `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Welcome Action-Methode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image5.png)

Lassen Sie uns das Beispiel etwas so ändern, dass Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die c#-Funktion optional-Parameter verwendet, um anzugeben, dass die `numTimes` Parameter sollte der Standardwert 1, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Die [ASP.NET MVC-modellbindungssystem](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image6.png)

In diesen beiden Beispielen wurde der Controller Ausführen der &quot;VC&quot; -Teil von MVC – d. h. die Ansicht und Controller Arbeit. Der Controller gibt HTML direkt zurück. Normalerweise sollen nicht Controller HTML direkt zurückgeben, da dies sehr mühsam Code wird. Stattdessen wird in der Regel eine separate ansichtsvorlagendatei verwendet, um Hilfe, die HTML-Antwort zu generieren. Sehen wir uns weiter an, wie wir dies erreichen können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-4.md)
> [Weiter](adding-a-view.md)
