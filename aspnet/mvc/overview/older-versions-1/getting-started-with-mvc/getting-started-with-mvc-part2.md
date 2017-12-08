---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: "Hinzufügen eines Controllers | Microsoft Docs"
author: shanselman
description: "Eine aktualisierte Version dieses Lernprogramm hier mit Visual Studio 2013 verfügbar ist. Das neue Lernprogramm verwendet ASP.NET MVC 5, zahlreiche Verbesserungen über t bietet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 93a362cf83d39b29fcba3f2dee0c28257805a89e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
durch [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version ist in diesem Lernprogramm verfügbar [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Lernprogramm verwendet ASP.NET MVC 5, viele Verbesserungen über dieses Lernprogramms bereitstellt.
> 
> 
> Dies ist ein Anfänger-Lernprogramm, das die Grundlagen von ASP.NET MVC. Erstellen Sie eine einfache Web-Anwendung, die Lese- und Schreibvorgänge aus einer Datenbank. Besuchen Sie die [ASP.NET MVC-Trainingscenter](../../../index.md) andere ASP.NET-MVC Lernprogramme und Beispiele finden.


MVC steht für Modell, Ansicht und Controller verwendet wird. MVC ist ein Muster für die Entwicklung von Anwendungen so, dass jeder Teil Zuständigkeit aufweist, die von einem anderen unterscheidet.

- Modell: Die Daten der Anwendung
- Ansichten: Die Vorlagendateien wird Ihre Anwendung verwenden zum dynamischen Generieren von HTML-Antworten.
- Domänencontroller: Klassen, die eingehende URL-Anforderungen an die Anwendung verarbeiten Modelldaten abzurufen, und geben Sie Vorlagen anzeigen, die eine Antwort zurück an den Client zu rendern

Wir werden all diese Konzepte in diesem Lernprogramm abdecken und erläutert deren Verwendung zum Erstellen einer Anwendung.

Erstellen wir einen neuen Domänencontroller, indem Sie den Ordner "Controller" im Projektmappen-Explorer mit der rechten Maustaste und Auswählen von Controller hinzufügen.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Nennen Sie Ihre neue Controller "HelloWorldController", und klicken Sie auf Hinzufügen.

[![Controller-Dialogfeld "hinzufügen"](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Beachten Sie im Projektmappen-Explorer auf das Recht, das für Sie aufgerufen HelloWorldController.cs eine neue Datei erstellt wurde, und diese Datei ist jetzt geöffnet, der **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Erstellen Sie zwei neue Methoden, die in die neue öffentliche Klasse HelloWorldController sieht wie folgt aus. Wir müssen eine HTML-Zeichenfolge als Beispiel direkt aus unserem Controller zurück.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Der Domänencontroller heißt HelloWorldController, und die neue Methode Index aufgerufen. Führen Sie die Anwendung erneut aus, wie zuvor (auf die Wiedergabeschaltfläche klicken, oder drücken Sie F5, um Sie zu diesem Zweck). Sobald Ihr Browser gestartet wurde, ändern Sie den Pfad in der Adressleiste `http://localhost:xx/HelloWorld` , wobei Xx ist jeder beliebigen Computers Zahl ausgewählt wurde. Jetzt sollte Ihr Browser wie der folgende Screenshot aussehen. In unserem oben genannten Methode zurückgegeben, wird eine Zeichenfolge an eine Methode namens "Content" übergeben Wir klärten nur einige HTML zurück, und es wurde ein!

ASP.NET MVC ruft verschiedene Controllerklassen (und andere Aktionsmethoden darin enthaltene) abhängig von der eingehenden URL an. Standardlogik für die Zuordnung von ASP.NET MVC verwendet verwendet ein Format wie folgt steuern, welcher Code ausgeführt wird:

/ [Controller] / [Aktionsname] / [Parameter]

Der erste Teil der URL bestimmt die Controllerklasse ausgeführt. So ordnet /HelloWorld HelloWorldController-Klasse. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher würde /HelloWorld/Index die Index()-Methode der Klasse HelloWorldcontroller ausgeführt. Beachten Sie, dass es musste nur /HelloWorld oben besuchen und die Methode, die Index impliziert wurde. Dies ist eine Methode namens "Index" Standardmethode besteht darin, die auf einem Domänencontroller aufgerufen wird, wenn nicht explizit angegeben ist.

[![Dies ist mein Standardaktion](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Jetzt sehen wir finden Sie auf `http://localhost:xx/HelloWorld/Welcome.` jetzt unsere Willkommen Methode ausgeführt und die HTML-Zeichenfolge zurückgegeben wurde.

In diesem Fall / [Controller] / [Aktionsname] / [Parameter] Controller ist HelloWorld und Willkommensseite ist die Methode in diesem Fall. Wir noch nicht Parameter noch nicht geschehen.

[![Dies ist der Willkommen Action-Methode](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Nehmen wir unser Beispiel etwas so ändern, dass wir einige Informationen in aus der URL zum unsere Controller, z. B. wie folgt übergeben: / HelloWorld/Willkommen? Name = Scott&amp;Numtimes = 4. Ändern Sie die Willkommensseite um zwei Parameter und es wie im folgenden Update einzuschließen. Beachten Sie, dass wir die C#-optionalen Parameter-Funktion verwendet haben, um anzugeben, dass der Parameter NumTimes auf 1 standardmäßig sollte, wenn er nicht übergeben wird.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Führen Sie die Anwendung, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` ändern den Wert von Name und Numtimes wie gewünscht. Das System wird automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode zugeordnet.

In beiden Beispielen der Controller wurde auf diese Weise die gesamte Arbeit, und verfügt über wurde zurückgeben HTML direkt. Normalerweise nicht Being unsere Controller HTML direkt - zurückgeben, da sehr aufwendig, um Code zu endet. Wir müssen stattdessen in der Regel eine separate Ansicht Vorlagendatei verwenden, können Sie die HTML-Antwort zu generieren. Sehen wir uns, wie wir dies durchführen können. Schließen Sie Ihren Browser, und zur IDE zurückzukehren.

>[!div class="step-by-step"]
[Zurück](getting-started-with-mvc-part1.md)
[Weiter](getting-started-with-mvc-part3.md)
