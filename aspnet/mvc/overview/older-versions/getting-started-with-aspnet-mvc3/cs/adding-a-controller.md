---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
title: Hinzufügen eines Controllers (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Serivice Pack 1, was, wie ich...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 0b8c56b5-fdf3-42dd-a866-98fbe0ab78a0
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b31cf3bdf18c144d2735973119367b01de0353fe
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833984"
---
<a name="adding-a-controller-c"></a>Hinzufügen eines Controllers (c#)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.
> 
> 
> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen, die gut strukturiert und leicht zu warten sind. MVC-basierten Anwendungen enthalten:

- Domänencontroller: Klassen, die Behandlung eingehender Anforderungen an die Anwendung, rufen Modelldaten aus, und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Client zurückgeben.
- Modelle: Klassen, die die Daten der Anwendung darstellen, und, die Validierungslogik zum Erzwingen von Geschäftsregeln für diese Daten verwenden.
- Ansichten:-Vorlagendateien, die von der Anwendung verwendeten HTML-Antworten dynamisch zu generieren.

Wir werden all diese Konzepte in dieser tutorialreihe abdecken und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.

Wir erstellen zunächst eine Controller-Klasse. In **Projektmappen-Explorer**, mit der rechten Maustaste die *Controller* Ordner, und wählen Sie dann **Controller hinzufügen**.

[![](adding-a-controller/_static/image2.png)](adding-a-controller/_static/image1.png)

Benennen Sie dem neuen Controller "HelloWorldController". Lassen Sie die Standardvorlage als **leeren Controller** , und klicken Sie auf **hinzufügen**.

[![AddHelloWorldController](adding-a-controller/_static/image4.png)](adding-a-controller/_static/image3.png)

Beachten Sie, dass **Projektmappen-Explorer** , dass eine neue Datei benannte erstellt wurde *HelloWorldController.cs*. Die Datei ist in der IDE geöffnet.

![](adding-a-controller/_static/image5.png)

In der `public class HelloWorldController` blockieren, erstellen Sie zwei Methoden, die wie im folgenden Code aussehen. Der Controller gibt eine Zeichenfolge mit HTML als Beispiel zurück.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Den Namen Ihres Controllers `HelloWorldController` und die erste Methode, die oben genannten heißt `Index`. Lassen Sie uns in einem Browser aufrufen. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Fügen Sie im Browser "HelloWorld" an den Pfad in die Adressleiste ein. (Z. B. in der Abbildung unten, dessen `http://localhost:43246/HelloWorld.`) die Seite im Browser wird wie im folgenden Screenshot aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Sie sagte, dass das System nur HTML-Code zurückgeben, und dies der Fall war.

![](adding-a-controller/_static/image6.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL an. Die Standardlogik von Zuordnung, die ASP.NET MVC verwendet ein Format wie dieses, um zu bestimmen, welcher Code zum Aufrufen:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controllerklasse. Also */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Also */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, die wir mussten nur durchsuchen */HelloWorld* und `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome`-Methode wird ausgeführt und gibt die Zeichenfolge „This is the Welcome action method...“ zurück. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL ist `HelloWorld` der Controller und `Welcome` die Aktionsmethode. Sie haben den Teil `[Parameters]` der URL noch nicht verwendet.

![](adding-a-controller/_static/image7.png)

Lassen Sie uns das Beispiel etwas so ändern, dass Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass der Code die c#-Funktion optional-Parameter verwendet, um anzugeben, dass die `numTimes` Parameter sollte der Standardwert 1, wenn kein Wert für diesen Parameter übergeben wird.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs)]

Führen Sie die Anwendung, und navigieren Sie zu der Beispiel-URL (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4)`. Sie können versuchen, verschiedene Werte für `name` und `numtimes` in der URL. Das System ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image8.png)

In diesen beiden Beispielen der Controller gearbeitet hat "den VC"-Teil von MVC, d. h. die Ansicht und Controller Arbeit. Der Controller gibt HTML direkt zurück. Normalerweise sollen nicht Controller HTML direkt zurückgeben, da dies sehr mühsam Code wird. Stattdessen wird in der Regel eine separate ansichtsvorlagendatei verwendet, um Hilfe, die HTML-Antwort zu generieren. Sehen wir uns weiter an, wie wir dies erreichen können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
