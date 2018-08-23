---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Hinzufügen eines Controllers | Microsoft-Dokumentation
author: shanselman
description: Eine aktualisierte Version, wenn in diesem Tutorial verwendet Visual Studio 2013 verfügbar ist. Das neue Tutorial verwendet ASP.NET MVC 5, bietet viele Verbesserungen für t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 856b7d98ac8bd30982d81b0609bb9c1288e07e49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832785"
---
<a name="adding-a-controller"></a>Hinzufügen eines Controllers
====================
durch [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Eine aktualisierte Version ist in diesem Tutorial verfügbaren [hier](../../getting-started/introduction/getting-started.md) mit [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). Das neue Tutorial verwendet ASP.NET MVC 5, die viele Verbesserungen für dieses Tutorial bietet.
> 
> 
> Dies ist ein Tutorial für Anfänger, die die Grundlagen von ASP.NET MVC eingeführt werden. Sie erstellen eine einfache Webanwendung, die aus einer Datenbank liest und schreibt. Besuchen Sie die [ASP.NET MVC-Informationscenter](../../../index.md) anderen ASP.NET MVC anhand von Tutorials und Beispiele finden.


MVC ist die Abkürzung für Modell, Ansicht und Controller. MVC ist ein Muster für die Entwicklung von Anwendungen, sodass jeder Teil verfügt über eine Aufgabe, die sich von einem anderen unterscheidet.

- Modell: Die Daten Ihrer Anwendung
- Ansichten: Die Vorlagendateien, die die Anwendung verwendet, um HTML-Antworten dynamisch zu generieren.
- Domänencontroller: Klassen, die eingehende URL-Anforderungen an die Anwendung verarbeiten rufen Modelldaten, und geben Sie Vorlagen anzeigen, die eine Antwort zurück an den Client zu rendern

Wir werden all diese Konzepte in diesem Tutorial behandelt und gezeigt, wie Sie diese verwenden, um eine Anwendung zu erstellen.

Erstellen wir einen neuen Domänencontroller, indem Sie mit der rechten Maustaste in den Projektmappen-Explorer den Ordner "Controllers", und Auswählen von Controller hinzufügen.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Benennen Sie dem neuen Controller "HelloWorldController", und klicken Sie auf Hinzufügen.

[![Controller-Dialogfeld "hinzufügen"](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Beachten Sie, dass im Projektmappen-Explorer auf der rechten Seite, die eine neue Datei namens HelloWorldController.cs erstellt wurde und diese Datei ist jetzt geöffnet, der **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Erstellen Sie zwei neue Methoden, die in die neue öffentliche Klasse HelloWorldController wie folgt aussehen. Es wird eine Zeichenfolge von HTML direkt über den Controller als Beispiel zurückgeben.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

Den Namen Ihres Controllers HelloWorldController, und die neue Methode Index aufgerufen. Führen Sie die Anwendung erneut aus, wie zuvor (klicken Sie auf die Schaltfläche für Wiedergabe oder drücken Sie F5, um Sie zu diesem Zweck). Sobald Ihr Browser gestartet wurde, ändern Sie den Pfad in der Adressleiste den `http://localhost:xx/HelloWorld` , wobei Xx ist, was auf Ihrem Computer Zahl wurde ausgewählt. Ihr Browser sollte nun wie im folgenden Screenshot aussehen. In unserem oben genannten Methode zurückgegeben, wir übergeben eine Zeichenfolge in eine Methode namens "Content" an. Wir erklärten gibt das System nur HTML-Code zurück, und dies der Fall war.

ASP.NET MVC ruft Controllerklassen verschiedene (und darin enthaltene unterschiedliche Aktionsmethoden) abhängig von der eingehenden URL. Die Standardlogik von Zuordnung, die ASP.NET MVC verwendet ein Format wie dieses um zu steuern, welcher Code ausgeführt wird:

/ [Controller] / [ActionName] / [Parameter]

Der erste Teil der URL bestimmt die auszuführende Controllerklasse. Also wird die Klasse HelloWorldController /HelloWorld zugeordnet. Der zweite Teil der URL bestimmt die Aktionsmethode in der Klasse ausgeführt. Daher würde /HelloWorld/Index der Index()-Methode der Klasse HelloWorldcontroller ausgeführt. Beachten Sie, dass nur werden musste, besuchen die oben genannten /HelloWorld und die Methode, die Index impliziert wurde. Das liegt eine Methode namens "Index" die Standardmethode ist, die für einen Controller aufgerufen wird, wenn eine nicht explizit angegeben ist.

[![Dies ist mein Standardaktion](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Nun sehen wir uns `http://localhost:xx/HelloWorld/Welcome.` nun unsere Willkommen-Methode ausgeführt und die HTML-Zeichenfolge zurückgegeben wurde.

In diesem Fall / [Controller] / [ActionName] / [Parameter], damit der Controller "HelloWorld", und Willkommen bei der in diesem Fall ist die Methode. Wir dies nicht getan haben Parameter noch nicht fertig.

[![Dies ist die Welcome Action-Methode](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Wir unser Beispiel etwas so ändern, dass einige Informationen in von der URL zum Controller, z. B. wie folgt übergeben werden können: / HelloWorld/Welcome? Name = Scott&amp;Numtimes = 4. Ändern Sie die Willkommensseite enthält zwei Parameter und aktualisieren sie wie unten. Beachten Sie, dass wir die C#-optionalen Parameter-Funktion verwendet haben, um anzugeben, dass der Parameter NumTimes auf 1 standardmäßig sollte, wenn sie nicht übergeben wird.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Führen Sie die Anwendung, und besuchen Sie `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` den Wert der Name und Numtimes ändern, wie Sie möchten. Das System wird automatisch die benannten Parameter aus der Abfragezeichenfolge in der Adressleiste den Parametern der Methode zugeordnet.

In diesen beiden Beispielen wird der Controller wurde die ganze Arbeit und verfügt über wurde zurückgeben HTML direkt. Normalerweise nicht sollten unsere Controller direkt - HTML zurückgeben, da, die im Endeffekt sehr mühsam Code. Stattdessen in der Regel verwenden eine separate ansichtsvorlagendatei wir zur Generierung der HTML-Antwort. Sehen wir uns, wie wir dies erreichen können. Schließen Sie Ihren Browser, und zur IDE zurückzukehren.

> [!div class="step-by-step"]
> [Zurück](getting-started-with-mvc-part1.md)
> [Weiter](getting-started-with-mvc-part3.md)
