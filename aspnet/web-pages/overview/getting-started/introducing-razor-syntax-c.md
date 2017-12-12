---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#) | Microsoft Docs"
author: tfitzmac
description: "In diesem Kapitel können Sie einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax. ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Web Pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: f054d574026ab6444cc59a126ef9dcdc323f7bff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel können Sie einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax. ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Webseiten auf Webservern. Dieser Artikel konzentriert sich auf die mithilfe der C#-Programmiersprache.
> 
> **Lernen Sie**:
> 
> - Das obere 8 Programmiertipps für die ersten Schritte bei der Programmierung von ASP.NET Web Pages mit Razor-Syntax.
> - Grundlegende Programmierkonzepte erläutert, die Sie benötigen.
> - Welche Server ASP.NET-Code und Razor-Syntax ist zu.
>   
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Der obere 8 Tipps für die Programmierung

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt benötigen wissen Sie beim Einstieg in ASP.NET Servercode mithilfe der Razor-Syntax schreiben.

> [!NOTE]
> Die Razor-Syntax basiert auf der C#-Programmiersprache, und das ist die Sprache, die am häufigsten mit ASP.NET Web Pages verwendet wird. Die Razor-Syntax unterstützt jedoch auch der Sprache Visual Basic, und die übrigen Funktionen, die Sie sehen, dass Sie auch in Visual Basic ausführen können. Weitere Informationen finden Sie im Anhang [Visual Basic-Sprache und-Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).


Sie finden weitere Informationen über die meisten dieser Programmiertechniken später in diesem Artikel.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Fügen Sie Code, um eine Seite mit dem @-Zeichen

Die `@` Inlineausdrücke, einzelne Anweisungsblöcken und mit mehreren Anweisungen Blöcke beginnt:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Dies ist, wie diese Anweisungen aussieht, wenn die Seite in einem Browser ausgeführt wird:

![Razor Bild1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Beim Anzeigen von Inhalt in einer Seite mit den `@` Zeichen, wie in den vorherigen Beispielen ASP.NET HTML-Codierung der Ausgabe. Dies ersetzt reservierte HTML-Zeichen (z. B. `<` und `>` und `&`) mit Codes, mit denen die Zeichen als Zeichen statt als HTML-Tags oder Entitäten interpretiert wird auf einer Webseite angezeigt werden können. Ohne HTML-Codierung, die Ausgabe aus dem Servercode möglicherweise nicht richtig angezeigt und kann eine Seite, um Sicherheitsrisiken machen.
> 
> Wenn Ihr Ziel ist, um HTML-Markup auszugeben, die RFID-Transponder als Markup gerendert wird (z. B. `<p></p>` für einen Absatz oder `<em></em>` Text hervorgehoben), finden Sie im Abschnitt [Kombinieren von Text, Markup und Code in Codeblöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Erfahren Sie mehr über die HTML-Codierung [arbeiten mit Formularen](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Sie einschließen Codeblöcke in Klammern

Ein *Codeblock* enthält eine oder mehrere codeanweisungen und in geschweifte Klammern eingeschlossen ist.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor Bild2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Innerhalb eines Blocks am Ende jeder Anweisung mit einem Semikolon

Jede Anweisung vollständige Code muss in einem Codeblock mit einem Semikolon enden. Inline-Ausdrücke enden nicht mit einem Semikolon.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie können Variablen verwenden, um Werte zu speichern

Sie können Werte in Speichern einer *Variable*, einschließlich Zeichenfolgen, Zahlen und Datumsangaben. Sie erstellen eine neue Variable mit dem `var` Schlüsselwort. Sie können Variablenwerte einfügen, direkt in eine Seite mit `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor bild3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie setzen literale Zeichenfolgenwerte in Anführungszeichen

Ein *Zeichenfolge* ist eine Folge von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge angeben, schließen Sie ihn in doppelte Anführungszeichen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Wenn die Zeichenfolge, die Sie anzeigen möchten, einen umgekehrten Schrägstrich enthält ( `\` ) oder doppelte Anführungszeichen ( `"` ), verwenden eine *wörtliche Zeichenfolgenliteral* , vorangestellt ist die `@` Operator. (In c# ist die \ Zeichen hat besondere Bedeutung, es sei denn, Sie wörtliche Zeichenfolge literal verwenden.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Einbetten von doppelten Anführungszeichen, ein wörtliches Zeichenfolgenliteral verwenden, und wiederholen die Anführungszeichen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

So sieht das Ergebnis der Verwendung in beiden Beispielen auf einer Seite aus:

![Razor bild4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Beachten Sie, dass die `@` Zeichen wird verwendet, um ausführliche zeichenfolgeliterale in c# zu markieren und Markieren von Code in ASP.NET-Seiten.


### <a name="6-code-is-case-sensitive"></a>6. Code wird die Groß-/Kleinschreibung unterschieden

C#-Schlüsselwörter (z. B. `var`, `true`, und `if`) und Variablennamen wird nach Groß-/Kleinschreibung beachtet. Die folgenden Codezeilen erstellen Sie zwei unterschiedliche Variablen `lastName` und`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Wenn Sie eine Variable deklarieren `var lastName = "Smith";` und wenn Sie versuchen, diese Variable auf Ihrer Seite als verweisen `@LastName`, ein Fehler verursacht, weil `LastName` wird nicht erkannt werden.

> [!NOTE]
> In Visual Basic-Schlüsselwörter und Variablen werden *nicht* Groß-/Kleinschreibung beachtet.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Viele der Codierung umfasst Objekte

Ein *Objekt* darstellt, die Sie programmieren können, mit etwas &#8212; eine Seite, ein Textfeld, eine Datei, ein Bild, einer webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz (Datenbankzeile) usw. Objekte verfügen über Eigenschaften, die ihren Merkmalen beschreiben, sodass Sie lesen können, oder Ändern einer &#8212; ein Text-Box-Objekt verfügt über eine `Text` -Eigenschaft (neben anderen), eine Request-Objekt hat eine `Url` -Eigenschaft, um eine e-Mail-Nachricht wurde ein `From` -Eigenschaft und eine Customer-Objekt hat eine `FirstName` Eigenschaft. Objekte verfügen außerdem über Methoden, die die &quot;Verben&quot; ausführen. Beispiele für ein Objekt "Datei" `Save` -Methode, ein Image-Objekt `Rotate` -Methode, und ein e-Mail-Objekt `Send` Methode.

Arbeiten Sie werden häufig mit den `Request` -Objekt, wo Sie Informationen wie die Werte von Textfeldern (Formularfelder) auf der Seite, welche Art von Browser, die URL der Seite, die Identität des Benutzers usw. angefordert. Im folgende Beispiel wird gezeigt, wie Eigenschaften für den Zugriff auf die `Request` -Objekt und das Aufrufen der `MapPath` Methode der `Request` -Objekt, das Sie den absoluten Pfad der Seite auf dem Server erhalten:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, mit der Entscheidungen

Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie ermitteln können, was Sie basierend auf Bedingungen. Die gängigste Methode hierfür ist, mit der `if` Anweisung (und optional `else` Anweisung).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Die Anweisung `if(IsPost)` ist eine schnelle Möglichkeit der Verfassung `if(IsPost == true)`. Zusammen mit `if` Anweisungen, es gibt eine Vielzahl von Methoden zum Testen Bedingungen, wiederholen Sie diesen Schritt Codeblöcke und usw., die beschrieben werden weiter unten in diesem Artikel.

Das Ergebnis in einem Browser angezeigt (nach dem Klicken auf **Absenden**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP-GET und POST-Methoden und die IsPost-Eigenschaft
> 
> Das Protokoll für Webseiten (HTTP) unterstützt eine beschränkte Anzahl von Methoden (Verben), die verwendet werden, um die Anforderungen an den Server zu senden. Die zwei gängigsten sind "GET", die zum Lesen einer Seite verwendet wird, und POST, die verwendet wird, um eine Seite zu senden. Im Allgemeinen zum ersten Mal, das ein Benutzer eine Seite anfordert, GET mithilfe die Seite angefordert. Wenn der Benutzer in einem Formular ausfüllt und klickt dann auf eine Schaltfläche "Absenden", sendet der Browser eine POST-Anforderung an dem Server an.
> 
> Web-Programmierung ist es oft hilfreich zu wissen, ob eine Seite als GET oder POST-Methode angefordert werden, damit Sie wissen, wie auf die Seite zu verarbeiten. In ASP.NET Web Pages, die Sie verwenden die `IsPost` Eigenschaft, um festzustellen, ob eine Anforderung eine Get- oder POST-Methode ist. Wenn die Anforderung einen Beitrag ist der `IsPost` Eigenschaft gibt "true" zurück, und Sie können z. B. das Lesen der Werte von Textfeldern in einem Formular. Viele Sie sehen Beispiele veranschaulichen das Verarbeiten der Seite unterschiedlich je nach dem Wert des `IsPost`.


## <a name="a-simple-code-example"></a>Ein einfaches Codebeispiel

Dieses Verfahren wird gezeigt, wie eine Seite zu erstellen, die grundlegende Programmiertechniken veranschaulicht. Im Beispiel erstellen Sie eine Seite, mit dem Benutzer, die zwei Zahlen eingeben, und fügt sie hinzu und das Ergebnis wird angezeigt.

1. Klicken Sie im Editor, erstellen Sie eine neue Datei, und nennen sie *AddNumbers.cshtml*.
2. Kopieren Sie den folgenden Code und Markup in die Seite, und Ersetzen Sie dabei alle Elemente bereits in der Seite an.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Hier sind einige Punkte beachten:

    - Die `@` beginnt die erste Codeblock auf der Seite, und ihm vorausgeht der `totalMessage` Variable, die am unteren Rand der Seite eingebettet ist.
    - Der Block am oberen Rand der Seite "ist in der geschweiften Klammern eingeschlossen.
    - Beenden alle Zeilen im-Block oben mit einem Semikolon.
    - Die Variablen `total`, `num1`, `num2`, und `totalMessage` mehrere Nummern und eine Zeichenfolge zu speichern.
    - Zugewiesene Wert Literalzeichenfolge der `totalMessage` Variable ist in doppelte Anführungszeichen.
    - Da der Code Groß-/Kleinschreibung beachtet, wenn die `totalMessage` Variablen am unteren Rand der Seite verwendet wird, muss der Namen der Variablen am Anfang genau entsprechen.
    - Der Ausdruck `num1.AsInt() + num2.AsInt()` wird gezeigt, wie zum Arbeiten mit Objekten und Methoden. Die `AsInt` -Methode für jede Variable konvertiert die Zeichenfolge, die von einem Benutzer auf eine Zahl (Integer) eingegeben wird, sodass Sie arithmetische Operationen darauf ausführen können.
    - Die `<form>` Tag enthält eine `method="post"` Attribut. Dies gibt an, wenn der Benutzer klickt auf **hinzufügen**, wird die Seite an den Server mithilfe der HTTP POST-Methode gesendet werden. Wenn die Seite übermittelt wird, die `if(IsPost)` Test True ergibt, die bedingte code ausgeführt wird, das Ergebnis der Addition der Zahlen anzeigen.
3. Speichern Sie die Seite, und führen Sie es in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Geben Sie zwei ganzen Zahlen, und klicken Sie dann auf die **hinzufügen** Schaltfläche. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Grundlegende Programmierkonzepte

Dieser Artikel bietet eine Übersicht über ASP.NET Web programmieren. Es ist nicht für eine gründlichere Überprüfung, nur einen kurzen Überblick über die Programmierkonzepte erläutert, die am häufigsten verwendet werden. Deshalb werden nahezu jede Option, die Sie benötigen, um zu beginnen mit ASP.NET Web Pages behandelt.

Erste jedoch ein wenig technische Hintergrundinformationen.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Die Razor-Syntax, Servercode und ASP.NET

Razor-Syntax ist eine einfache Programmiersyntax zum Einbetten von serverbasiertem Code in einer Webseite. Auf einer Webseite, die die Razor-Syntax verwendet, es gibt zwei Arten von Inhalten: Clientcode Inhalt und Server. Clientinhalt ist die Dinge, die in Webseiten mit sind: HTML-Markup (Elemente), Informationen wie z. B. CSS, Stil vielleicht einige Clientskript, z. B. JavaScript und nur-Text.

Razor-Syntax können Sie diese Clients Inhalte Servercode hinzufügen. Ist es Servercode auf der Seite, führt der Server diesen Code zuerst, bevor die Seite an den Browser gesendet. Auf dem Server ausführen, kann der Code Aufgaben ausführen, die viel komplexer mit Clientinhalt eigenständig verwendet, z. B. den Zugriff auf serverbasierte Datenbanken werden können. Am wichtigsten ist, kann Servercode Client dynamisch Inhalt &#8212;erstellen. Sie können HTML-Markup oder andere Inhalte nebenbei generieren und senden Sie sie an den Browser sowie die statische HTML-Elemente, die die Seite enthalten kann. Aus Sicht des Browsers unterscheidet sich Clients Inhalte, die von Ihr Servercode generiert wird nicht als andere Client-Inhalt. Wie Sie bereits gesehen haben, ist der Code, der erforderlich ist ganz einfach.

ASP.NET-Webseiten, die die Razor-Syntax enthalten spezielle Dateierweiterung aufweisen (*cshtml* oder *vbhtml*). Der Server erkennt diese Erweiterungen, führt den Code, der mit Razor-Syntax markiert ist, und klicken Sie dann die Seite an den Browser sendet.

### <a name="where-does-aspnet-fit-in"></a>Wann ist ASP.NET angebracht?

Razor-Syntax basiert auf eine Technologie von Microsoft ASP.NET,, die wiederum auf Microsoft .NET Framework basiert. .NET Framework ist ein großer, umfassende Programmierframework von Microsoft für nahezu jede Art von Computer-Anwendung entwickeln. ASP.NET ist der Teil von .NET Framework, die speziell für das Erstellen von Webanwendungen konzipiert ist. Entwickler haben ASP.NET verwendet, um vielen der größten und höchsten Datenverkehr Websites in der ganzen Welt erstellt. (Jederzeit finden Sie unter der Dateinamenerweiterung *aspx* als Teil der URL, an einem Standort müssen Sie wissen, dass die Website mithilfe von ASP.NET geschrieben wurde.)

Die Razor-Syntax erhalten Sie die gesamte Leistung von ASP.NET, verwendet jedoch eine vereinfachte Syntax, die einfacher zu erfahren, ob Sie Anfänger sind und Sie produktiv stellt, wenn Sie ein Experte sind. Obwohl diese Syntax einfach zu verwenden ist, bedeutet die Familie Beziehung mit ASP.NET und .NET Framework, wie Ihre Websites komplexer werden, Sie die Leistungsfähigkeit von der größeren Frameworks zur Verfügung haben.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klassen und Instanzen**
> 
> Servercode ASP.NET verwendet die Objekte, die wiederum auf der Idee von Klassen erstellt werden. Die Klasse ist die Definition oder einer Vorlage für ein Objekt. Angenommen, eine Anwendung enthält eine `Customer` Klasse, definiert die Eigenschaften und Methoden, die alle Kundenobjekt benötigt.
> 
> Wenn die Anwendung mit der tatsächlichen Kundeninformationen arbeiten muss, erstellt er eine Instanz von (oder *instanziiert*) ein Customer-Objekt. Jedes einzelner Kunden ist eine separate Instanz von der `Customer` Klasse. Jede Instanz unterstützt die gleichen Eigenschaften und Methoden, aber die Eigenschaftswerte für jede Instanz in der Regel unterschiedlich sind, da jedes Kundenobjekt eindeutig ist. In einer Customer-Objekt das `LastName` Eigenschaft möglicherweise "Smith"; in einem anderen Customer-Objekt, das `LastName` Eigenschaft möglicherweise "Jones."
> 
> Ebenso jeder einzelnen Webseite an Ihrem Standort ist ein `Page` -Objekt, das eine Instanz ist die `Page` Klasse. Eine Schaltfläche auf der Seite ist eine `Button` -Objekt, das eine Instanz von der `Button` -Klasse, und So weiter. Jede Instanz verfügt über eigene Eigenschaften, aber alle basieren auf was in der Klassendefinition für das Objekt angegeben wird.


## <a name="basic-syntax"></a>Allgemeine Syntax

Zuvor haben Sie gesehen ein einfaches Beispiel zum Erstellen einer ASP.NET Web Pages-Seite, und wie Sie HTML-Markup Servercode hinzufügen können. Hier erfahren Sie die Grundlagen zum Schreiben von ASP.NET Servercode mithilfe der Razor-Syntax &#8212; d. h. die Programmierung Sprachregeln.

Wenn Sie bereits Erfahrung mit Programmierung (insbesondere, wenn Sie C, C++, c#, Visual Basic oder JavaScript verwendet haben), werden viele der hier Sie lesen vertraut sein. Sie benötigen wahrscheinlich Kennenlernen nur wie Markup im Servercode hinzugefügt wird *cshtml* Dateien.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Kombinieren von Text, Markup und Code in Codeblöcken

In Server-Codeblöcke möchten Sie häufig Ausgabe Text oder Markup (oder beides) auf der Seite. Wenn ein servercodeblock Text enthält, die sich nicht Code und, stattdessen gerendert werden soll, ist, muss ASP.NET Code, in die Text unterscheiden können. Dafür stehen verschiedene Möglichkeiten zur Verfügung:

- Schließen Sie den Text in ein HTML-Element, z. B. `<p></p>` oder `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn ASP.NET sieht das öffnende HTML-Tag (z. B. `<p>`), alle Elemente einschließlich des-Elements gerendert und seinen Inhalt als an den Browser, darin Codeausdrücke auf dem Server aufgelöst wird.
- Verwenden der `@:` Operator oder die `<text>` Element. Die `@:` gibt eine einzige Codezeile Inhalt, der nur-Text oder HTML-Tags werden nicht übereinstimmende; enthält die `<text>` Element einschließt, mehrere Zeilen ausgegeben. Diese Optionen eignen sich nicht als Teil der Ausgabe ein HTML-Elements gerendert werden sollen.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Wenn Sie mehrere Zeilen Text oder HTML-Tags, die nicht übereinstimmenden ausgeben möchten, können Sie jede Zeile mit voranstellen `@:`, oder Sie können einschließen, die Zeile in einer `<text>` Element. Wie die `@:` Operator`<text>` Tags von ASP.NET zum Identifizieren von Textinhalt verwendet werden und werden nie in der Seitenausgabe gerendert.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Im erste Beispiel wird die im vorherige Beispiel wiederholt aber verwendet ein einzelnes Paar von `<text>` Tags aus, um den Text zum Rendern einschließen. Im zweiten Beispiel die `<text>` und `</text>` Tags einschließen drei Zeilen, die alle einige nicht enthaltene Text und HTML-Tags, die nicht übereinstimmenden haben (`<br />`), sowie der Servercode und übereinstimmende HTML-Tags. Erneut, können Sie auch jede Zeile einzeln mit voranstellen der `@:` Operator; beide Weise funktionieren.

    > [!NOTE]
    > Wenn Sie Text Ausgabe wie in diesem Abschnitt &#8212;dargestellt; ein HTML-Element mit dem `@:` -Operator, oder die `<text>` -Element &#8212; ASP.NET die Ausgabe nicht HTML-codiert. (Wie bereits erwähnt, wird die Ausgabe der Codeausdrücke Server und Server Codeblöcke, die vorangestellt werden von ASP.NET codiert `@`, außer in die Sonderfälle, in diesem Abschnitt erwähnt wurden.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer Anweisung (und außerhalb von einem Zeichenfolgenliteral) wirken sich nicht auf die Anweisung aus:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Ein Zeilenumbruch in einer Anweisung hat keine Auswirkungen auf die Anweisung, und Sie können Anweisungen zur besseren Lesbarkeit umbrochen. Die folgenden Anweisungen entsprechen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Allerdings können keine Sie eine Zeile in der Mitte ein Zeichenfolgenliteral umschließen. Im folgende Beispiel funktioniert nicht:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Es gibt zwei Optionen, um eine lange Zeichenfolge zu kombinieren, die mehrere Zeilen wie der Code oben umbrochen wird. Sie können den Verkettungsoperator (`+`), die Sie später in diesem Artikel sehen. Sie können auch die `@` Zeichen wörtliche Zeichenfolge literal erstellt, wie Sie weiter oben in diesem Artikel gesehen haben. Ausführliche zeichenfolgeliterale kann über mehrere Zeilen umbrochen werden:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Code (und Markup) Kommentare

Kommentare können Sie die Anmerkungen zu dieser selbst oder von anderen Benutzern zu lassen. Sie können auch damit deaktivieren (*kommentieren*) einen Abschnitt des Codes oder die Markuperweiterung, die Sie nicht ausgeführt werden soll, aber auf Ihrer Seite wird beibehalten möchten.

Es unterscheidet sich kommentieren die Syntax für Razor-Code und HTML-Markup. Wie bei allen Razor-Code Razor Kommentare werden verarbeitet (und dann entfernt) auf dem Server, bevor die Seite an den Browser gesendet wird. Aus diesem Grund kann die Kommentierung Razor-Syntax Kommentare in den Code (oder sogar in das Markup) zu konzentrieren, die Sie sehen können, wenn Sie die Datei bearbeiten, aber, die Benutzern nicht angezeigt, selbst im Quellcode für die Seite.

Für ASP.NET Razor-Kommentare, starten Sie den Kommentar mit `@*` und enden mit `*@`. Der Kommentar kann auf eine oder mehrere Zeilen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Hier ist ein Kommentar innerhalb eines Codeblocks:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Hier wird die gleichen Codeblock, mit der Codezeile auskommentiert, damit er keine ausgeführt werden:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

In einem Codeblock können Sie als Alternative zur Verwendung von Razor-Kommentar-Syntax, die Kommentare Syntax der Programmiersprache verwenden, die Sie, die wie z. B. c# verwenden:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In c# einzeilige Kommentare vorangestellt werden die `//` Zeichen und mehrzeilige Kommentare beginnen mit `/*` und enden mit `*/`. (Wie bei der Razor-Kommentare werden C#-Kommentare nicht im Browser gerendert.)

Für Markup wie Sie möglicherweise wissen, können Sie einen HTML-Kommentar erstellen:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Beginnen Sie mit HTML-Kommentare `<!--` Zeichen und enden mit `-->`. HTML-Kommentare können zum umschließen, nicht nur Text, sondern auch alle HTML-Markup, die Sie auf der Seite beibehalten wollen, aber nicht gerendert werden soll. Diese HTML-Kommentar wird den gesamten Inhalt des Tags und die darin enthaltenen Text ausblenden:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Im Gegensatz zu Razor-Kommentare, HTML-Kommentare *sind* auf der Seite gerendert und dem Benutzer können sie durch Anzeigen der Quellcode für die Seite angezeigt.

Razor weist Einschränkungen auf geschachtelten Blöcken von c#. Weitere Informationen finden Sie unter [mit dem Namen C#-Variablen und geschachtelten Blöcke aufgeteilt Code generieren](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variablen

Eine Variable ist ein benanntes Objekt, mit denen Sie Daten zu speichern. Variablen beliebig benennen, aber der Name muss mit einem alphanumerischen Zeichen beginnen und darf keine Leerzeichen oder reservierte Zeichen.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable kann einen bestimmten Datentyp besitzen, der angibt, welche Art von Daten in der Variablen gespeichert ist. Können Sie Zeichenfolgenvariablen, in denen Zeichenfolgenwerte gespeichert haben (z. B. &quot;Hello World&quot;), Ganzzahlvariablen, die ganzzahlige Werte (z. B. 3 oder 79) speichern und Datenvariablen, die Datumswerte in einer Vielzahl von Formaten (z. B. 4/12/2012 oder März 2009 speichern ). Und es gibt viele andere Datentypen, die Sie verwenden können.

Allerdings müssen Sie in der Regel einen Typ für eine Variable angeben. In den meisten Fällen, können ASP.NET herausfinden, welche basierend auf wie die Daten in die Variable verwendet wird. (Gelegentlich müssen Sie einen Typ angeben, sehen Sie Beispiele, in denen dies "true".)

Sie deklarieren eine Variable mit dem `var` Schlüsselwort (sofern Sie nicht möchten, dass für einen Typ angegeben wird) oder mit dem Namen des Typs:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Das folgende Beispiel zeigt einige typischen Verwendungen von Variablen in einer Webseite:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Wenn Sie die vorherigen Beispielen auf einer Seite kombinieren, sehen Sie diese in einem Browser angezeigt:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konvertieren und Testen von Datentypen

Obwohl ASP.NET einen Datentyp in der Regel automatisch ermitteln kann, in einigen Fällen kann es nicht. Aus diesem Grund müssen Sie die ASP.NET unterstützen auf, indem Sie eine explizite Konvertierung durchführen. Auch wenn Sie keine Typen konvertiert haben, ist es manchmal hilfreich, testen, um festzustellen, welche Art von Daten Sie arbeiten können.

Der häufigste Fall ist, Sie eine Zeichenfolge in einen anderen Typ wie z. B. in eine ganze Zahl oder ein Datum zu konvertieren müssen. Das folgende Beispiel zeigt einem typischen Fall, in dem eine Zeichenfolge in eine Zahl konvertiert werden muss.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

– In der Regel eine Benutzereingabe werden Sie als Zeichenfolgen. Auch wenn Sie Benutzer zur Eingabe einer Zahl aufgefordert haben und auch wenn sie eine Ziffer eingegeben haben, wenn Benutzereingaben gesendet wird und Sie im Code lesen, sind die Daten im Zeichenfolgenformat. Aus diesem Grund müssen Sie die Zeichenfolge in eine Zahl konvertieren. Im Beispiel wenn Sie versuchen, die Durchführung von Berechnungen auf den Werten ohne zu konvertieren, führt der folgende Fehler, da ASP.NET die beiden Zeichenfolgen hinzufügen können:

*Typ "Zeichenfolge" in "Int" kann nicht implizit konvertiert werden.*

Um die Werte zu einer ganzen Zahl zu konvertieren, rufen Sie die `AsInt` Methode. Wenn die Konvertierung erfolgreich ist, können Sie dann die Zahlen hinzufügen.

Die folgende Tabelle enthält einige allgemeine Konvertierung und Test-Methoden für die Variablen an.

| **Methode** | **Beschreibung** | **Beispiel** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Konvertiert eine Zeichenfolge, die eine ganze Zahl (z. B. "593") in eine ganze Zahl darstellt. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | Konvertiert eine Zeichenfolge wie &quot;"true"&quot; oder &quot;"false"&quot; zu einem Boolean-Typ. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Gleitkommazahl. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Dezimalzahl. (In ASP.NET ist eine Dezimalzahl genauer als eine Gleitkommazahl.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | Konvertiert eine Zeichenfolge, die einen Wert für Datum und Uhrzeit der ASP.NET darstellt `DateTime` Typ. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | Konvertiert von einem beliebigen anderen Datentyp in eine Zeichenfolge. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder ein Zeichen, die ASP.NET, welche Art von Befehl zum Ausführen in einem Ausdruck mitteilt. Der C#-Sprache (und die Razor-Syntax, die darauf basieren) unterstützt viele Operatoren müssen Sie nur wenige für den Einstieg zu erkennen. In der folgenden Tabelle werden die am häufigsten verwendeten Operatoren zusammengefasst.

| **Operator** | **Beschreibung** | **Beispiele** |
| --- | --- | --- |
| `+` `-` `*` `/` | Mathematische Operatoren, die in numerischen Ausdrücken verwendet. | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | Zuweisung. Weist den Wert auf der rechten Seite einer Anweisung mit dem Objekt auf der linken Seite an. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | Gleichheit. Gibt `true` , wenn die Werte gleich sind. (Beachten Sie den Unterschied zwischen der `=` Operator und die `==` Operator.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | Ungleichheit. Gibt `true` , wenn die Werte nicht gleich sind. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | Kleiner-als, größer-als, kleiner oder gleich und größer oder gleich. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | Verkettung verwendet wird, um Zeichenfolgen zu verknüpfen. ASP.NET ist den Unterschied zwischen diesen Operator und dem Additionsoperator basierend auf den Datentyp des Ausdrucks bekannt. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=` `-=` | Die Inkrement- und Dekrement-Operatoren, die Addition und Subtraktion 1 (bzw.) aus einer Variablen. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | Punkt. Verwendet, um Objekte und ihre Eigenschaften und Methoden zu unterscheiden. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | Klammern. Gruppierungsausdrücke sowie zum Übergeben von Parametern für Methoden verwendet. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | Klammern. Für den Zugriff auf Werte in Arrays oder Auflistungen verwendet. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | Nicht. Kehrt eine `true` Wert `false` und umgekehrt. In der Regel als eine schnelle Möglichkeit zum Testen verwendet `false` (d. h. für nicht `true`). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&` <code>&#124;&#124;</code> | Logisches AND und Bedingungen, die verwendet werden, um die Verknüpfung zusammen. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Arbeiten mit Datei- und Ordnerpfade in Code

Sie werden häufig in Ihrem Code mit der Datei- und Ordnerpfade arbeiten. Hier ist ein Beispiel der physischen Ordnerstruktur für eine Website auf, wie er auf dem Entwicklungscomputer angezeigt werden kann:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Hier sind einige wichtige Details über URLs und Pfade:

- Eine URL beginnt entweder mit einen Domänennamen (`http://www.example.com`) oder einen Servernamen (`http://localhost`, `http://mycomputer`).
- Eine URL entspricht einem physischen Pfad auf einem Hostcomputer. Beispielsweise `http://myserver` entspricht dem Ordner möglicherweise *C:\websites\mywebsite* auf dem Server.
- Ein virtueller Pfad ist die Abkürzung für Pfade im Code ohne den vollständigen Pfad angeben zu müssen. Es umfasst den Teil einer URL, die die Domäne bzw. den Servernamen folgt. Bei Verwendung von virtuellen Pfaden können Sie Ihren Code zu einer anderen Domäne oder Server verschieben, ohne zum Aktualisieren der Pfade.

Hier ist ein Beispiel, um die Unterschiede zu verstehen:

| Vollständige URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Servername | *mycompanyserver* |
| Virtueller Pfad | */HumanResources/CompanyPolicy.htm* |
| Physischer Pfad | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Das virtuelle Stammverzeichnis ist /, wie der Stamm von Laufwerk C: Laufwerk ist \. (Virtuelle Ordnerpfaden verwenden immer Schrägstriche). Der virtuelle Pfad eines Ordners verfügt nicht über den gleichen Namen wie der physische Ordner haben; Es kann ein Alias sein. (Auf Produktionsservern, der virtuelle Pfad selten einen genaue physischen Pfad entspricht.)

Bei der Arbeit mit Dateien und Ordnern im Code müssen Sie manchmal verweisen auf den physischen Pfad und in einigen Fällen einen virtuellen Pfad, je nachdem welche Objekte mit dem Sie arbeiten. ASP.NET bietet Ihnen diese Tools zum Arbeiten mit Datei- und Ordnerpfade in Code: der `Server.MapPath` -Methode, und die `~` Operator und `Href` Methode.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konvertieren von virtuellen und physischen Pfade: die Server.MapPath-Methode

Der `Server.MapPath` Methode konvertiert einen virtuellen Pfad (z. B. */default.cshtml*) in einen absoluten physischen Pfad (z. B. *C:\WebSites\MyWebSiteFolder\default.cshtml*). Mithilfe dieser Methode können Sie jederzeit einen vollständigen physischen Pfad. Ein typisches Beispiel ist beim Lesen oder Schreiben einer Textdatei oder einer Bilddatei auf dem Webserver.

Der absoluten physischen Pfad der Website auf einer hosting-Site-Server in der Regel nicht kennen, damit diese Methode konvertieren des Pfads können Sie kennen, den virtuellen Pfad – auf den entsprechenden Pfad auf dem Server für Sie. Sie den virtuellen Pfad zu einer Datei oder eines Ordners an die Methode übergeben, und gibt den physischen Pfad zurück:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und Href-Methode

In einem *cshtml* oder *vbhtml* -Datei können Sie verweisen, den virtuellen Stamm Pfad mit der `~` Operator. Dies ist sehr nützlich, da Sie Seiten, an einem Standort navigieren können, und alle Links, die sie zu anderen Seiten enthalten nicht unterbrochen. Es ist auch hilfreich, für den Fall, dass Sie Ihre Website jemals an einen anderen Speicherort verschieben. Hier einige Beispiele:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Wenn die Website `http://myserver/myapp`, sieht wie ASP.NET diese Pfade behandelt wird, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade nicht tatsächlich als die Werte der Variablen angezeigt, aber ASP.NET Pfade behandelt, als wäre, was waren ist.)

Sie können die `~` Operator im Servercode (wie oben) und im Markup, wie folgt:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Verwenden Sie im Markup der `~` Operator, um Pfade zu Ressourcen, wie Bilddateien, andere Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, über die Seite "" (Code und Markup) sucht und behebt alle ASP.NET die `~` Verweise auf den entsprechenden Pfad.

## <a name="conditional-logic-and-loops"></a>Bedingter Logik und Schleifen

Servercode für ASP.NET können Sie die Aufgaben, die basierend auf Bedingungen und Schreiben Sie Code, durch die Anweisung wiederholt eine bestimmte Anzahl von Malen (d. h. Code, der eine Schleife ausgeführt wird).

### <a name="testing-conditions"></a>Testen von Bedingungen

Um eine einfache Bedingung zu testen, Sie verwenden, die `if` -Anweisung, die einen Test gibt "true" oder "false" Grundlage Sie angeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Die `if` Schlüsselwort beginnt einen Block. Die tatsächliche Test (Bedingung) wird in Klammern angegeben und gibt "true" oder "false" zurück. Die Anweisungen, die ausgeführt werden, wenn der Test "true" ist, werden in Klammern eingeschlossen. Ein `if` Anweisung zählen eine `else` Datenblock, Anweisungen, die ausgeführt werden, wenn die Bedingung "false" ist:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Sie können mehrere Bedingungen mit hinzufügen ein `else if` blockieren:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

In diesem Beispiel, wenn die erste in der If-Bedingung Block ist nicht "true", die `else if` Bedingung aktiviert ist. Wenn diese Bedingung erfüllt ist, die Anweisungen in der `else if` Block ausgeführt werden. Wenn keine der Bedingungen erfüllt sind, die Anweisungen in der `else` Block ausgeführt werden. Sie können eine beliebige Anzahl von ElseIf hinzufügen blockiert, und schließen Sie dann mit einer `else` Blockieren der &quot;alles andere&quot; Bedingung.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie eine `switch` blockieren:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Der zu testende Wert steht in Klammern (im Beispiel die `weekday` Variable). Jeder einzelne Test verwendet einen `case` -Anweisung, die mit einem Doppelpunkt (:)) endet. Wenn der Wert des einem `case` Anweisung entspricht dem Testwert, der Code in diesem Fall Block wird ausgeführt. Schließen Sie jede Case-Anweisung mit einem `break` Anweisung. (Wenn Sie vergessen, jede Unterbrechung einschließt `case` blockieren, den Code von der nächsten `case` Anweisung wird auch ausgeführt werden.) Ein `switch` Block verfügt oft über eine `default` Anweisung als bei der letzten Fall einen &quot;alles andere&quot; Option, die ausgeführt wird, wenn keine der anderen Fälle zutrifft.

Das Ergebnis der letzten beiden bedingte Blöcke in einem Browser angezeigt:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Durchlaufen von Code

Sie müssen häufig die gleichen Anweisungen wiederholt ausführen. Dazu müssen Sie Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Auflistung von Daten. Wenn Sie wissen, genau wie oft Sie in einer Schleife möchten, können Sie eine `for` Schleife. Diese Art von Schleife ist besonders hilfreich für das Zählen von oben oder nach unten zählen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Die Schleife beginnt mit der `for` Schlüsselwort, gefolgt von drei Anweisungen in Klammern ein, jeweils mit einem Semikolon beendet.

- Innerhalb der Klammern, die erste Anweisung (`var i=10;`) einen Leistungsindikator erstellt und initialisiert es mit 10. Müssen Sie die Namen des Zählers nicht `i` &#8212; Sie können eine beliebige Variable. Wenn die `for` Schleife ausgeführt wird, wird automatisch erhöht.
- Die zweite Anweisung (`i < 21;`) legt die Bedingung wie weit Sie zählen möchten. In diesem Fall möchten Sie es auf ein Maximum von 20 wechseln (d. h. passiert während der Indikator kleiner als 21 befindet).
- Die dritte Anweisung (`i++` ) verwendet eine Inkrementoperator, der einfach angibt, dass der Leistungsindikator hinzugefügt wird bei jeder Ausführung die Schleife 1 aufweisen sollte.

Ist Sie in der geschweiften Klammern der Code, der für jede Iteration der Schleife ausgeführt wird. Das Markup erstellt einen neuen Absatz (`<p>` Element) jedes Mal und fügt eine Zeile an die Ausgabe des Werts eines `i` (Leistungsindikator). Wenn Sie auf dieser Seite ausführen, erstellt das Beispiel 11 Zeilen, die die Ausgabe mit dem Text in jeder Zeile an, mit dem Element angezeigt.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Wenn Sie eine Auflistung oder ein Array verwenden, verwenden Sie häufig eine `foreach` Schleife. Eine Auflistung ist eine Gruppe von ähnliche Objekte, und die `foreach` Schleife können, Sie aus einer Aufgabe für jedes Element in der Auflistung führen. Diese Art der Schleife eignet sich für Auflistungen, da im Gegensatz zu einer `for` Schleifen, Sie müssen keine Erhöhen des Zählerwerts und einzuschränken. Stattdessen die `foreach` Schleife Code wird einfach in der Auflistung fortgesetzt, bis er abgeschlossen ist.

Der folgende Code gibt beispielsweise die Elemente in der `Request.ServerVariables` Auflistung, die ein Objekt, das Informationen über den Webserver enthält. Er verwendet ein `foreac` h-Schleife, um den Namen der einzelnen Elemente anzeigen, indem Sie beim Erstellen eines neuen `<li>` Element in einem HTML-Aufzählung.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Die `foreach` Schlüsselwort durch Klammern gefolgt wird, bei denen Sie Deklarieren einer Variable, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `var item`), gefolgt von den `in` Schlüsselwort, gefolgt von der Auflistung durchlaufen werden sollen. Im Hauptteil der `foreach` Schleife, können Sie das aktuelle Element über die Variable, die Sie zuvor deklarierte zugreifen.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Verwenden Sie zum Erstellen einer allgemeinen Schleife die `while` Anweisung:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Ein `while` Schleife beginnt mit der `while` Schlüsselwort, gefolgt von Klammern geben Sie, wie lange die Schleife fortgesetzt wird (hier für solange `countNum` ist kleiner als 50), klicken Sie dann den Block zu wiederholen. Schleifen in der Regel erhöhen (hinzufügen) oder dekrementiert (subtrahiert) eine Variable oder ein Objekt, das für die Inventur verwendet. Im Beispiel die `+=` Operator fügt 1 bis `countNum` bei jeder Ausführung die Schleife. (Um eine Variable in einer Schleife zu verringern, die nach-unten zählt, verwenden Sie die Dekrement-Operators `-=`).

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Nahezu alles in einer ASP.NET-Website handelt es sich um ein Objekt, einschließlich der Webseite selbst. Dieser Abschnitt beschreibt einige wichtige Objekte, die Sie häufig in Ihrem Code arbeiten mit.

### <a name="page-objects"></a>Seite "-Objekte

Das grundlegendste Objekt in ASP.NET ist die Seite. Sie können die Eigenschaften der Page-Objekt direkt ohne alle qualifizierenden Objekt zugreifen. Der folgende Code Ruft den Dateipfad mit der Seite mit den `Request` Objekt von der Seite ":

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Zum erleichtern deutlich, dass verweisen auf Eigenschaften und Methoden für das Seitenobjekt der aktuellen Optional können Sie das Schlüsselwort `this` zur Darstellung der Page-Objekt in Ihrem Code. Hier wird im vorangehenden Codebeispiel wird mit `this` zur Darstellung der Seite hinzugefügt:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Können Sie Eigenschaften der `Page` Objekt, eine Vielzahl von Informationen, wie z. B. abgerufen werden:

- `Request`. Wie Sie bereits gesehen haben, ist dies eine Auflistung von Informationen über die aktuelle Anforderung, z. B., welche Art von Browser, die URL der Seite, die Identität des Benutzers usw. angefordert.
- `Response`. Dies ist eine Auflistung von Informationen über die Antwort (Seite), die an den Browser gesendet wird, wenn der Code ausgeführt wurde. Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Auflistungsobjekte (Arrays und Wörterbücher)

Ein *Auflistung* ist eine Gruppe von Objekten desselben Typs, z. B. eine Auflistung von `Customer` Objekte aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, wie z. B. die `Request.Files` Auflistung.

Sie werden häufig in Auflistungen mit Daten arbeiten. Zwei allgemeine Auflistungstypen sind die *Array* und *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Auflistung mit ähnlichen Elementen gespeichert werden soll, aber nicht, erstellen Sie eine separate Variable, um jedes Element möchten:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Bei Arrays, Sie deklarieren einen bestimmten Datentyp z. B. `string`, `int`, oder `DateTime`. Um anzugeben, dass die Variable kann ein Array enthalten, Sie Klammern zur Deklaration hinzufügen (z. B. `string[]` oder `int[]`). Sie erreichen die Elemente in einem Array mit ihrer Position (Index) oder mithilfe der `foreach` Anweisung. Array Indizes sind nullbasiert &#8212; Das heißt, das erste Element ist an position 0, das zweite Element ist an Position 1 und So weiter.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Sie können die Anzahl der Elemente in einem Array zu bestimmen, durch Abrufen der `Length` Eigenschaft. Um die Position eines bestimmten Elements im Array (Suchen Sie das Array) abzurufen, verwenden die `Array.IndexOf` Methode. Sie können auch Aktionen wie Reverse den Inhalt eines Arrays (die `Array.Reverse` Methode) oder Sortieren des Inhalts (die `Array.Sort` Methode).

Die Ausgabe von der String-Array-Code in einem Browser angezeigt:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel/Wert-Paaren, in dem Sie den Schlüssel (oder den Namen) festlegen oder Abrufen des entsprechenden Werts eingeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Um ein Wörterbuch zu erstellen, verwenden Sie die `new` Schlüsselwort, um anzugeben, dass Sie ein neues Dictionary-Objekt erstellen. Sie können ein Wörterbuch zuweisen, um eine Variable mit dem `var` Schlüsselwort. Sie geben die Datentypen der Elemente im Wörterbuch mithilfe der spitzen Klammern ( `< >` ). Am Ende der Deklaration müssen Sie ein Klammernpaar, hinzufügen, da es sich tatsächlich um eine Methode handelt, die ein neues Wörterbuch erstellt.

Um Elemente zum Wörterbuch hinzuzufügen, rufen Sie die `Add` Methode der Variablen Wörterbuch (`myScores` in diesem Fall), und klicken Sie dann einen Schlüssel und einen Wert angeben. Alternativ können Sie eckige Klammern verwenden, um den Schlüssel an, und führen Sie eine einfache Zuweisung, wie im folgenden Beispiel:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in Klammern:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

Wie Sie weiter oben in diesem Artikel lesen, können die Objekte, denen Programmierung mit Methoden verfügen. Z. B. eine `Database` Objekt möglicherweise ein `Database.Connect` Methode. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, den Sie an eine Methode übergeben, die Methode zum Abschließen des Tasks zu aktivieren. Betrachten Sie z. B. eine Deklaration für die `Request.MapPath` Methode, die drei Parameter akzeptiert:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Die Zeile wurde umgebrochen, um sie besser lesbar zu machen. Denken Sie daran, dass Sie Zeilenumbrüche aufnehmen können, fast jeden Ort, mit der Ausnahme, die innerhalb von Zeichenfolgen, in Anführungszeichen eingeschlossen werden.)

Diese Methode gibt den physischen Pfad auf dem Server, der entspricht einem angegebenen virtuellen Pfad zurück. Die drei Parameter für die Methode sind `virtualPath`, `baseVirtualDir`, und `allowCrossAppMapping`. (Beachten Sie, dass in der Deklaration der Parameter mit den Datentypen der Daten aufgeführt sind, die sie akzeptieren.) Wenn Sie diese Methode aufrufen, müssen Sie Werte für alle drei Parameter angeben.

Die Razor-Syntax erhalten Sie zwei Optionen zum Übergeben von Parametern an eine Methode: *Positionsparameter* und *benannte Parameter*. Zum Aufrufen einer Methode mit positionelle Parameter übergeben Sie die Parameter in einer festen Reihenfolge, die in der Deklaration der Methode angegeben ist. (Sie würden diese Reihenfolge in der Regel wissen, lesen Sie die Dokumentation für die Methode.) Sie müssen die Reihenfolge einhalten, und Sie können nicht fortfahren, einen der Parameter &#8212; Wenn es sich bei Bedarf können Sie eine leere Zeichenfolge übergeben (`""`) oder `null` für einen positionelle Parameter, die Sie für einen Wert besitzen.

Im folgende Beispiel wird angenommen, Sie haben einen Ordner namens *Skripts* auf Ihrer Website. Der Code Ruft die `Request.MapPath` -Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Anschließend wird den resultierenden zugeordneten Pfad angezeigt.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Wenn eine Methode viele Parameter verfügt, können Sie den Code besser lesbar behalten mithilfe von benannten Parametern. Zum Aufrufen einer Methode, die benannte Parameter verwenden, geben Sie einen Doppelpunkt (:)), und klicken Sie dann den Wert der Name des Parameters folgen. Der Vorteil der benannten Parameter ist, dass Sie sie in beliebiger Reihenfolge übergeben können Sie die gewünschte. (Ein Nachteil ist, dass der Methodenaufruf nicht als compact ist).

Im folgenden Beispiel wird die gleiche Methode wie oben beschrieben, aber verwendet benannte Parameter, um die Werte anzugeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übergeben. Wenn Sie im vorherigen Beispiel und in diesem Beispiel ausführen, müssen sie den gleichen Wert zurückgeben.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-Catch-Anweisungen

Sie müssen häufig Anweisungen in Ihrem Code, die außerhalb Ihrer Kontrolle Gründen fehlschlagen. Zum Beispiel:

- Wenn Ihr Code versucht, erstellen oder eine Datei zuzugreifen, können alle Arten von Fehlern auftreten. Die gewünschte Datei ist möglicherweise nicht vorhanden, wird sie möglicherweise gesperrt, der Code möglicherweise nicht über Berechtigungen verfügen, und so weiter.
- Auf ähnliche Weise, wenn Ihr Code versucht, zum Aktualisieren von Datensätzen in einer Datenbank, treten möglicherweise Berechtigungsprobleme, kann die Verbindung mit der Datenbank gelöscht werden, die zu speichernden Daten möglicherweise ungültige usw.

Programmiertechnisch gesehen Situationen heißen *Ausnahmen*. Wenn im Code eine Ausnahme auftritt, generiert er (auslöst) eine Fehlermeldung angezeigt, die Benutzern bestenfalls störende:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

In Situationen, in denen Code kann Ausnahmen auftreten, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `try/catch` Anweisungen. In der `try` -Anweisung, führen Sie den Code, der Sie eingecheckt haben. Eine oder mehrere `catch` -Anweisungen, die Sie suchen können, für bestimmte Fehler (bestimmte Arten von Ausnahmen), die möglicherweise aufgetreten sind. Sie können beliebig viele einschließen `catch` Anweisungen, wie Sie nach Fehlern zu suchen, die Sie vorhersehen werden müssen.

> [!NOTE]
> Es wird empfohlen, dass Sie, verwenden vermeiden die `Response.Redirect` Methode in `try/catch` Anweisungen, da es einen Ausnahmefehler, auf der Seite verursachen kann.


Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und zeigt dann eine Schaltfläche, die dem Benutzer die Datei öffnen kann. Im Beispiel verwendet absichtlich einen falscher Dateiname, sodass er eine Ausnahme auslöst. Der Code enthält `catch` -Anweisungen für die zwei möglichen Ausnahmen: `FileNotFoundException`, Dies tritt ein, wenn der Dateiname beschädigt ist und `DirectoryNotFoundException`, Dies tritt ein, wenn ASP.NET den Ordner selbst finden kann. (Sie können kommentieren Sie eine Anweisung im Beispiel um anzuzeigen, wie er ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn Ihr Code die Ausnahme behandelt hat nicht, sehen Sie eine Fehlerseite, wie der vorherige Screenshot. Allerdings die `try/catch` Abschnitt wird verhindert, dass den Benutzer sehen diese Arten von Fehlern.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Programmierung mit Visual Basic**


[Anhang: Visual Basic-Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908)


**Referenzdokumentation**


[ASP.NET](https://msdn.microsoft.com/en-us/library/ee532866.aspx)

[C#-Sprache](https://msdn.microsoft.com/en-us/library/kx37x362.aspx)
