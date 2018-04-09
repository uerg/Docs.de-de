---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Hinzufügen eines Controllers (VB) | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 9a433083c31c7929f7599e52800c887f301d7727
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller-vb"></a>Hinzufügen eines Controllers (VB)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/adding-a-controller.md) dieses Lernprogramm.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen so, dass jeder Teil eine separate Verantwortung aufweist:

- Modell: Die Daten für Ihre Anwendung.
- Ansichten: Die Vorlagendateien wird Ihre Anwendung verwenden zum dynamischen Generieren von HTML-Antworten.
- Domänencontroller: Klassen, verarbeiten eingehende URL-Anforderungen an die Anwendung, rufen Modelldaten aus, und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Client zu rendern.

Wir werden all diese Konzepte in diesem Lernprogramm abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.

Erstellen Sie einen neuen Domänencontroller, indem Sie mit der rechten Maustaste die *Controller* Ordner **Projektmappen-Explorer** auswählen und dann **Controller hinzufügen**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Nennen Sie Ihre neue Controller &quot;HelloWorldController&quot; , und klicken Sie auf **hinzufügen**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Beachten Sie im **Projektmappen-Explorer** auf der rechten Seite, dass Sie eine neue Datei erstellt wurde mit dem Namen *HelloWorldController.cs* und dass die Datei in der IDE geöffnet ist.

In der neuen `public class HelloWorldController` blockieren, erstellen Sie zwei neue Methoden, die den folgenden Code ähneln. Wir müssen eine HTML-Zeichenfolge als Beispiel direkt auf dem Controller zurück.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Der Domänencontroller heißt `HelloWorldController` und die neue Methode lautet `Index`. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Sobald Ihr Browser gestartet wurde, fügen &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Auf meinem Computer kann `http://localhost:43246/HelloWorld`) Ihr Browser wird der folgende Screenshot aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Wir mitgeteilt, dass das System auf HTML-Code zurückgeben, und es wurde!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an. Standardlogik für die Zuordnung von ASP.NET MVC verwendet verwendet ein Format wie folgt steuern, welcher Code aufgerufen wird:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die Controllerklasse ausgeführt. Daher */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, das wir besuchen musste */HelloWorld* oben und der `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Willkommensseite Aktionsmethode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Für diese URL wird der Controller `HelloWorld` und `Welcome` ist die Methode. Wir noch nicht verwendet die `[Parameters]` die URL noch Teil.

![](adding-a-controller/_static/image6.png)

Nehmen wir das Beispiel etwas so ändern, dass in einigen Parameterinformationen aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass wir die VB-optionalen Parameter-Funktion verwendet haben, um anzugeben, dass die `numTimes` Parameter sollte auf 1 Standard, falls kein Wert für diesen Parameter übergeben wird.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Sie können versuchen, verschiedene Werte für `name` und `numtimes`. Das System wird automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode zugeordnet.

![](adding-a-controller/_static/image7.png)

In beiden Beispielen der Controller die VC-Teil des MVC ausführen wurde – das ist die Ansicht und Controller-Aufgaben. Der Controller ist HTML direkt zurückgeben. Wir möchten normalerweise nicht Controller HTML direkt zurückgeben, da, die sehr aufwendig, Code wird. Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren. Sehen wir uns, wie wir dies durchführen können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
