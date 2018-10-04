---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
title: Hinzufügen eines Controllers (VB) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 741259e1-54ac-4f71-b4e8-2bd5560bb950
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: b38f3c4051af426e471568b8a25a471986f07209
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576227"
---
<a name="adding-a-controller-vb"></a>Hinzufügen eines Controllers (VB)
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET steht zur Ergänzung dieses Themas. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C# -Code bevorzugen, wechseln Sie zu der [c#-Version](../cs/adding-a-controller.md) in diesem Tutorial.


MVC steht für *Model-View-Controller*. MVC ist ein Muster für die Entwicklung von Anwendungen so, dass jeder Teil eine separate Aufgabe aufweist:

- Modell: Die Daten für Ihre Anwendung.
- Ansichten: Die Vorlagendateien, die die Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- Controller: Klassen, verarbeiten eingehende URL-Anforderungen an die Anwendung, rufen Modelldaten aus, und geben Sie dann die Vorlagen anzeigen, die eine Antwort an den Client zu rendern.

Wir werden all diese Konzepte in diesem Tutorial behandelt und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.

Erstellen Sie einen neuen Domänencontroller, indem Sie mit der rechten Maustaste die *Controller* Ordner **Projektmappen-Explorer** auswählen und dann **Controller hinzufügen**.

[![AddController](adding-a-controller/_static/image2.png "AddController")](adding-a-controller/_static/image1.png)

Benennen Sie dem neuen Controller &quot;HelloWorldController&quot; , und klicken Sie auf **hinzufügen**.

[![2AddEmptyController](adding-a-controller/_static/image4.png "2AddEmptyController")](adding-a-controller/_static/image3.png)

Beachten Sie, dass **Projektmappen-Explorer** auf der rechten Seite, dass Sie eine neue Datei erstellt wurde mit dem Namen *HelloWorldController.cs* und dass die Datei in der IDE geöffnet ist.

In der neuen `public class HelloWorldController` blockieren, erstellen Sie zwei neue Methoden, die wie im folgenden Code aussehen. Es wird eine Zeichenfolge von HTML direkt vom Controller als Beispiel zurückgeben.

[!code-vb[Main](adding-a-controller/samples/sample1.vb)]

Den Namen Ihres Controllers `HelloWorldController` und die neue Methode heißt `Index`. Führen Sie die Anwendung (drücken Sie F5 oder STRG + F5). Sobald Ihr Browser gestartet wurde, fügen Sie &quot;HelloWorld&quot; auf den Pfad in der Adressleiste. (Auf meinem Computer hat `http://localhost:43246/HelloWorld`) Ihr Browser wird im folgenden Screenshot aussehen. In der oben genannten Methode hat der Code direkt eine Zeichenfolge zurückgegeben. Wir mitgeteilt, dass das System nur HTML-Code zurückgeben, und dies der Fall war.

![](adding-a-controller/_static/image5.png)

ASP.NET MVC ruft verschiedene Controllerklassen (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL an. Die Standardlogik von Zuordnung, die ASP.NET MVC verwendet ein Format wie folgt steuern, welcher Code aufgerufen wird:

`/[Controller]/[ActionName]/[Parameters]`

Der erste Teil der URL bestimmt die auszuführende Controllerklasse. Also */HelloWorld* ordnet die `HelloWorldController` Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Also */HelloWorld/Index* würde dazu führen, dass die `Index` Methode der `HelloWorldController` Klasse ausgeführt. Beachten Sie, die nur werden musste, besuchen */HelloWorld* oben und die `Index` Methode wurde in der Standardeinstellung verwendet. Dies ist, da eine Methode mit dem Namen `Index` ist die Standardmethode, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.

Wechseln Sie zu `http://localhost:xxxx/HelloWorld/Welcome`. Die `Welcome` wird ausgeführt und gibt die Zeichenfolge &quot;Dies ist die Welcome Action-Methode... &quot;. Die standardzuordnung für MVC ist `/[Controller]/[ActionName]/[Parameters]`. Bei dieser URL der Controller ist `HelloWorld` und `Welcome` ist die Methode. Wir haben noch nicht verwendet die `[Parameters]` Teil der URL noch.

![](adding-a-controller/_static/image6.png)

Lassen Sie uns das Beispiel etwas so ändern, dass Parameterinformationen in aus der URL an den Controller übergeben werden können (z. B. */HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4*). Ändern Ihrer `Welcome` Methode, um zwei Parameter enthalten, wie unten dargestellt. Beachten Sie, dass wir die optionalen Parameter das VB-Feature verwendet haben, um anzugeben, dass die `numTimes` Parameter sollte der Standardwert 1, wenn kein Wert für diesen Parameter übergeben wird.

[!code-vb[Main](adding-a-controller/samples/sample2.vb)]

Führen Sie die Anwendung, und navigieren Sie zu `http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4` **.** Sie können versuchen, verschiedene Werte für `name` und `numtimes`. Das System ordnet automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode.

![](adding-a-controller/_static/image7.png)

In diesen beiden Beispielen der Controller gearbeitet hat den VC-Teil von MVC – Dies ist die Ansicht und Controller Arbeit. Der Controller gibt HTML direkt zurück. Wir sollten normalerweise nicht Controller HTML direkt zurückgeben, da dies sehr mühsam Code wird. Stattdessen wird in der Regel eine separate ansichtsvorlagendatei verwendet, um Hilfe, die HTML-Antwort zu generieren. Sehen wir uns, wie wir dies erreichen können.

> [!div class="step-by-step"]
> [Zurück](intro-to-aspnet-mvc-3.md)
> [Weiter](adding-a-view.md)
