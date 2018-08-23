---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Einführung in Debugging ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: tfitzmac
description: Debugging ist der Prozess des Suchens und Behebens von Fehlern in Ihren Codeseiten. In diesem Kapitel erfahren Sie, einige Tools und Techniken zum Debuggen können und Analysieren der...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: da04158c58054a8c0b8e31d3a55bea2dcbae7a05
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832297"
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Einführung in Debugging ASP.NET Web Pages (Razor) Sites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt verschiedene Möglichkeiten zum Debuggen von Seiten auf einer Website für ASP.NET Web Pages (Razor). Debugging ist der Prozess des Suchens und Behebens von Fehlern in Ihren Codeseiten.
> 
> **Sie lernen Folgendes:** 
> 
> - Vorgehensweise beim Anzeigen von Informationen, die dabei hilft, analysieren und Debuggen von Seiten.
> - Gewusst wie: Verwenden Sie das Debuggen in Visual Studio-tools.
>   
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Die `ServerInfo` Helper.
> - `ObjectInfo` -Hilfsprogramm.
>   
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2. Sie können WebMatrix 3 verwenden, aber integrierte Debugger wird nicht unterstützt.


Ein wichtiger Aspekt der Problembehandlung von Fehlern und Problemen im Code werden diese im vornherein zu vermeiden. Sie können dies durch Einfügen von Abschnitte des Codes, die wahrscheinlich Fehler verursachen `try/catch` Blöcke. Weitere Informationen finden Sie im Abschnitt zum Behandeln von Fehlern in [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890).

Die `ServerInfo` Helper ist ein Diagnosetool, das Sie einen Überblick über die Informationen über die Web-Server-Umgebung bietet, die Ihre Seite hostet. Außerdem erfahren Sie, HTTP-Anforderungsinformationen, die gesendet wird, wenn ein Browser die Seite anfordert. Die `ServerInfo` Hilfsprogramm zeigt den Identität des aktuellen Benutzers, der Typ des Browsers, der eine Anforderung gesendet, und so weiter. Diese Art von Informationen können Sie die Behandlung häufig auftretender Probleme.

1. Erstellen einer neuen Webseite mit dem Namen *ServerInfo.cshtml*.
2. Am Ende der Seite wird nur vor dem schließenden `</body>` markieren, fügen Sie `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Sie können hinzufügen, die `ServerInfo` Code an einer beliebigen Stelle auf der Seite. Aber es am Ende hinzugefügt werden seine Ausgabe getrennt von Ihrer anderen Seiteninhalt, wodurch es einfacher zu lesen.

    > [!NOTE] 
    > 
    > **Wichtige** Sie sollten alle Diagnosecode von Ihren Webseiten entfernen, bevor Sie Webseiten auf einem Produktionsserver verschieben. Dies gilt für die `ServerInfo` Helper als auch die sonstige diagnostischen Verfahrensweisen in diesem Artikel, bei denen Code einer Seite hinzufügen. Sie möchten nicht Besucher Ihrer Website, um Informationen zu Ihrem Servernamen, Benutzernamen, Pfade auf Ihrem Server, und klicken Sie auf ähnliche Informationen anzuzeigen, da diese Art von Informationen für Personen mit böswilligen Absichten nützlich sein könnten.
3. Speichern Sie die Seite, und führen Sie sie in einem Browser.

    ![Debuggen von-1](introduction-to-debugging/_static/image1.jpg)

    Die `ServerInfo` Hilfsprogramm zeigt vier Tabellen mit Informationen auf der Seite:

   - Konfiguration des Servers. Dieser Abschnitt enthält Informationen zum hosting-Webserver, einschließlich der Name des Computers, der Version von ASP.NET ausführen, den Domänennamen und die Serverzeit an.
   - ASP.NET Server-Variablen. Dieser Abschnitt enthält Details über die viele Details für HTTP-Protokoll (aufgerufene HTTP-Variablen) und Werte, sind Teil jeder Anforderung der Webseite.
   - Informationen zu HTTP-Laufzeit. Dieser Abschnitt enthält details zu, die die Version von Microsoft .NET Framework, die Ihre Webseite unter ausgeführt wird, den Pfad, der Details zu den Cache, und So weiter. (Wie Sie in haben [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages mit der Razor-Syntax werden auf Grundlage von Microsoft ASP.NET Web-Server-Technologie, die selbst auf eine umfangreiche Software erstellt wird Entwicklungsbibliothek mit dem Namen .NET Framework.)
   - Umgebungsvariablen. Dieser Abschnitt enthält eine Liste mit allen lokalen Umgebungsvariablen und deren Werte auf dem Webserver.

     Eine vollständige Beschreibung der Informationen zur Server und die Anforderung ist nicht Gegenstand dieses Artikels, aber Sie sehen, dass die `ServerInfo` Helper eine Vielzahl von Diagnoseinformationen zurückgegeben. Weitere Informationen zu den Werten, die `ServerInfo` zurückgibt, finden Sie unter [Umgebungsvariablen erkannt](https://technet.microsoft.com/library/dd560744(WS.10).aspx) auf der Microsoft TechNet-Website und [IIS-Servervariablen](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) auf der MSDN-Website.

## <a name="embedding-output-expressions-to-display-page-values"></a>Einbetten von Ausgabe-Ausdrücke zum Anzeigen von Seite

Eine weitere Möglichkeit, festzustellen, was in Ihrem Code passiert ist, Ausgabe-Ausdrücke auf der Seite einzubetten. Wie Sie wissen, können Sie den Wert einer Variablen direkt ausgeben, indem Sie etwa hinzufügen `@myVariable` oder `@(subTotal * 12)` auf der Seite. Für das Debuggen, können Sie diese Ausgabe-Ausdrücke an strategischen Stellen im Code platzieren. Dadurch können Sie den Wert des Schlüssels Variablen oder das Ergebnis von Berechnungen anzeigen, wenn Ihre Seite ausgeführt wird. Wenn Sie fertig sind Debuggen, können Sie die Ausdrücke zu entfernen oder kommentieren Sie diese aus. Diese Prozedur zeigt eine typische Herangehensweise an die eingebettete Ausdrücke verwenden, um eine Seite zu debuggen.

1. Erstellen Sie eine neue WebMatrix-Seite mit dem Namen *OutputExpression.cshtml*.
2. Ersetzen Sie die Seite mit folgendem Inhalt:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Im Beispiel wird eine `switch` Anweisung, um die Überprüfung des Werts der `weekday` Variable, und klicken Sie dann eine andere Ausgabe-je nach den Tag der Woche Nachricht anzeigen. Im Beispiel die `if` -Block innerhalb der erste Codeblock nach dem Zufallsprinzip ändert sich der Tag der Woche durch Hinzufügen von einem Tag auf den aktuellen Wochentag-Wert. Dies ist ein Fehler, die zur Veranschaulichung eingeführt.
3. Speichern Sie die Seite, und führen Sie sie in einem Browser.

    Die Seite zeigt die Meldung für den falschen Tag der Woche. Der Tag der Woche es eigentlich ist, wird die Nachricht für einen Tag zu einem späteren Zeitpunkt angezeigt werden. Obwohl in diesem Fall Sie wissen, warum die Nachricht deaktiviert ist (da der Code den falsche Tageswert absichtlich legt fest), in der Praxis ist es oft schwer zu wissen, in denen Fehler im Code Stand der Dinge ist. Zum Debuggen, müssen Sie herausfinden, was auf den Wert der wichtigsten Objekte und Variablen wie z. B. passiert `weekday`.
4. Ausgabe-Ausdrücke hinzufügen, durch Einfügen von `@weekday` wie an zwei Stellen angegeben werden, indem Kommentare im Code gezeigt. Diese Ausgabe-Ausdrücke werden die Werte der Variablen an diesem Punkt in die Ausführung des Codes angezeigt.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Speichern Sie, und führen Sie die Seite in einem Browser.

    Die Seite zeigt die tatsächlichen Tag der Woche zuerst, und klicken Sie dann die aktualisierte Tag der Woche, die sich ergibt aus dem Hinzufügen von einem Tag, und klicken Sie dann die resultierende Nachricht aus der `switch` Anweisung. Die Ausgabe aus den beiden Variablen Ausdrücken (`@weekday`) ohne Leerzeichen zwischen den Tagen hat, da Sie die HTML-Elemente hinzugefügt haben `<p>` Tags aus, um die Ausgabe die Ausdrücke werden nur zu Testzwecken.

    ![Debuggen-2](introduction-to-debugging/_static/image2.jpg)

    Nun Sie sehen können, wo der Fehler ist. Wenn Sie die ersten Anzeigen der `weekday` Variable im Code, zeigt es den richtigen Tag. Wenn Sie ihn anzeigen beim zweiten Mal wird nach der `if` -block in den Code, der Tag, die von einem deaktiviert ist. Damit Sie wissen, dass zwischen der ersten und zweiten Darstellung der Wochentag Variablen ist ein Problem aufgetreten ist. Würde dies eines echten Fehlers, hilft diese Art von Ansatz Ihnen bei den Speicherort des Codes einzugrenzen, die das Problem verursacht.
6. Beheben Sie den Code auf der Seite, indem Sie entfernen die zwei Ausgabe-Ausdrücke, die Sie hinzugefügt haben, und entfernen den Code, der den Tag der Woche geändert. Der verbleibende, vollständige Codeblock sieht wie im folgenden Beispiel aus:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Führen Sie die Seite in einem Browser. Dieses Mal sehen Sie die richtige Nachricht, die für den aktuellen Tag der Woche angezeigt.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Verwenden zum Anzeigen von Objektwerten der ObjectInfo-Hilfsmethode

Die `ObjectInfo` Hilfsprogramm zeigt den Typ und den Wert des jeweiligen Objekts, das Sie übergeben. Können Sie sie an den Wert der Variablen und Objekte in Ihrem Code (wie Sie mit Ausdrücken der Ausgabe im vorherigen Beispiel), außerdem sehen Sie Daten, die Informationen über das Objekt geben.

1. Öffnen Sie die Datei mit dem Namen *OutputExpression.cshtml* , die Sie zuvor erstellt haben.
2. Ersetzen Sie alle Code auf der Seite mit den folgenden Codeblock:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Speichern Sie, und führen Sie die Seite in einem Browser.

    ![Debuggen-4](introduction-to-debugging/_static/image3.jpg)

    In diesem Beispiel die `ObjectInfo` Hilfsprogramm zeigt zwei Elemente:

   - Der Typ. Für die erste Variable, der Typ ist `DayOfWeek`. Für die zweite Variable ist der Typ `String`.
   - Der Wert. In diesem Fall, da Sie bereits über den Wert der Grußformel bei der Variablen auf der Seite anzuzeigen, der Wert wird erneut angezeigt, wenn Sie die Variable übergeben `ObjectInfo`.

     Für komplexe Objekte die `ObjectInfo` Helper kann mehr Informationen anzeigen &#8212; im Grunde genommen können sie anzeigen, die Typen und Werte aller Eigenschaften eines Objekts.

## <a name="using-debugging-tools-in-visual-studio"></a>Verwenden Tools zum Debuggen in Visual Studio

Verwenden Sie für eine umfassende Debugleistung, Visual Studio 2013 oder die kostenlose [Visual Studio Express 2013 für Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Mit Visual Studio können Sie einen Haltepunkt im Code in der Befehlszeile festlegen, die Sie untersuchen möchten.

![Haltepunkt festlegen](introduction-to-debugging/_static/image1.png)

Wenn Sie die Website testen, wird der ausgeführte Code die Ausführung am Haltepunkt angehalten.

![Haltepunkt erreicht](introduction-to-debugging/_static/image2.png)

Sie können die aktuellen Werte der Variablen, und durchlaufen Sie den Code Zeile für Zeile überprüfen.

![Werte](introduction-to-debugging/_static/image3.png)

Weitere Informationen zur Verwendung von integrierten Debuggers in Visual Studio zum Debuggen von ASP.NET Razor-Seiten finden Sie unter [Programming ASP.NET Web Pages (Razor) mithilfe von Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Programmieren von ASP.NET-Webseiten (Razor) mithilfe von Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [IIS-Servervariablen](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Erkannt von Umgebungsvariablen](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
