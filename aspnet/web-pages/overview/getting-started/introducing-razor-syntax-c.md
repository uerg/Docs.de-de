---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#) | Microsoft-Dokumentation
author: tfitzmac
description: Dieses Kapitel bietet Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax. ASP.NET ist Microsoft Technologie für die dynamische Pa ausgeführt...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 347e5ddbc02866887d3f422ecc291e5e3dfacaaf
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207913"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (c#)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel bietet Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax. ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Webseiten auf Webservern. Dieser Artikel konzentriert sich auf die mit der Programmiersprache c#.
> 
> **Sie lernen Folgendes**:
> 
> - Das obere 8 Programmiertipps für die ersten Schritte mit ASP.NET Web Pages mit Razor-Syntax.
> - Grundlegende Programmierkonzepte kennen, die Sie benötigen.
> - Welche ASP.NET-Servercode: und die Razor-Syntax geht.
>   
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Die besten Tipps für 8 Programmierung

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt benötigen wissen, wie Sie mithilfe der Razor-Syntax ASP.NET-Servercode: schreibe.

> [!NOTE]
> Die Razor-Syntax basiert auf der C#-Programmiersprache, und das ist die Sprache, die am häufigsten mit ASP.NET Web Pages verwendet wird. Die Razor-Syntax unterstützt jedoch auch Visual Basic-Sprache, und alle Elemente, die Sie sehen, dass Sie auch in Visual Basic durchführen können. Weitere Informationen finden Sie im Anhang [Visual Basic-Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).


Weitere Informationen über die meisten dieser Techniken zur Programmierung finden Sie später in diesem Artikel.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Sie fügen Code hinzu, um eine Seite mit dem @-Zeichen

Die `@` Inlineausdrücke, einzelne Anweisungsblöcken und mit mehreren Anweisungen Blöcke beginnt:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Dies ist, wie diese Anweisungen aussehen, wenn die Seite in einem Browser ausgeführt wird:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Wenn Sie Inhalt anzeigen, auf eine Seite mit den `@` Zeichen, wie in den obigen Beispielen ASP.NET HTML-Codierung der Ausgabe. Dies ersetzt die reservierte HTML-Zeichen (z. B. `<` und `>` und `&`) mit Codes, mit denen die Zeichen als Zeichen statt als HTML-Tags oder Entitäten interpretiert wird auf einer Webseite angezeigt werden können. Ohne HTML-Codierung, die aus Ihrem Code möglicherweise nicht richtig angezeigt, und möglicherweise eine Seite, um Sicherheitsrisiken ausgesetzt.
> 
> Wenn Ihr Ziel ist, um HTML-Markup auszugeben, die Tags als Markup rendert (z. B. `<p></p>` für einen Absatz oder `<em></em>` Text hervorgehoben), finden Sie im Abschnitt [Kombinieren von Text, Markup und Code in Codeblöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Erfahren Sie mehr über die HTML-Codierung [arbeiten mit Formularen](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Schließen Sie Codeblöcke in geschweifte Klammern

Ein *Codeblock* enthält eine oder mehrere codeanweisungen und in geschweifte Klammern eingeschlossen ist.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. In einem Block am Ende jeder Anweisung mit einem Semikolon

In einem Codeblock muss jede vollständige Code-Anweisung mit einem Semikolon enden. Inline-Ausdrücke enden nicht mit einem Semikolon.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie verwenden Variablen zum Speichern von Werten

Sie können Werte in speichern eine *Variable*, einschließlich Zeichenfolgen, Zahlen und Datumsangaben. Sie erstellen eine neue Variable mit dem `var` Schlüsselwort. Sie können Werte direkt in eine Seite mit einfügen `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie einschließen literale Zeichenfolgenwerte in Anführungszeichen

Ein *Zeichenfolge* ist eine Folge von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge angeben, schließen Sie ihn in doppelte Anführungszeichen ein:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Wenn die Zeichenfolge, die Sie anzeigen möchten, einen umgekehrten Schrägstrich enthält ( `\` ) oder doppelte Anführungszeichen ( `"` ), verwenden Sie eine *ausführliches Zeichenfolgenliteral* , enthält das Präfix der `@` Operator. (In C# geschrieben, die \ Zeichen eine besonderen Bedeutung hat, es sei denn, Sie ein ausführliches Zeichenfolgenliteral verwenden.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Klicken Sie zum Einbetten von doppelten Anführungszeichen verwenden ein ausführliches Zeichenfolgenliteral, und wiederholen Sie die Anführungszeichen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

So sieht das Ergebnis der Verwendung von diesen beiden Beispielen auf einer Seite aus:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Beachten Sie, dass die `@` Zeichen wird verwendet, markieren Sie wörtliche Zeichenfolgenliterale in C# geschrieben und Markieren von Code in ASP.NET-Seiten.


### <a name="6-code-is-case-sensitive"></a>6. Code wird die Groß-/Kleinschreibung beachten

C#-Schlüsselwörter (z. B. `var`, `true`, und `if`) und Namen wird die Groß-/Kleinschreibung beachtet. Die folgenden Codezeilen erstellt zwei unterschiedliche Variablen `lastName` und `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Wenn Sie eine Variable deklarieren `var lastName = "Smith";` und wenn Sie versuchen, diese Variable auf der Seite als verweisen `@LastName`, ein Fehler ausgegeben, da `LastName` wird nicht erkannt werden.

> [!NOTE]
> In Visual Basic-Schlüsselwörter und Variablen werden *nicht* Groß-/Kleinschreibung beachtet.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Die Codierung ein Großteil beinhaltet Objekte

Ein *Objekt* stellt eine Sache, die Sie programmieren können, mit &#8212; eine Seite, ein Textfeld, eine Datei, ein Bild, eine webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz (Datenbankzeile) usw. Objekte verfügen über Eigenschaften, die ihre Merkmale zu beschreiben, und Sie lesen oder ändern können &#8212; ein Textfeld-Objekt verfügt über eine `Text` -Eigenschaft (unter anderem) eine Request-Objekt verfügt über eine `Url` -Eigenschaft, die eine e-Mail-Nachricht verfügt über eine `From` -Eigenschaft und ein Kundenobjekt verfügt über eine `FirstName` Eigenschaft. Objekte verfügen außerdem über Methoden, die die &quot;Verben&quot; ausführen. Beispiele hierfür sind ein Dateiobjekt `Save` -Methode, ein Bildobjekt `Rotate` -Methode und eine e-Mail-Adresse des Objekts `Send` Methode.

Sie arbeiten häufig mit der `Request` Objekt, wo Sie Informationen wie die Werte von Textfeldern (Felder) auf der Seite Art der Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. gesendet. Das folgende Beispiel zeigt, wie Sie den Zugriff auf Eigenschaften von den `Request` -Objekt und das Aufrufen von der `MapPath` -Methode der der `Request` -Objekt, das Sie den absoluten Pfad der Seite auf dem Server erhalten:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, die Entscheidungen trifft

Ein wichtiges Feature von dynamischen Webseiten ist, dass auf Sie bestimmen können, was zu tun ist basierend auf Bedingungen. Die gängigste Methode dazu ist die `if` Anweisung (und optional `else` Anweisung).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

Die Anweisung `if(IsPost)` ist eine schnelle Möglichkeit der Verfassung `if(IsPost == true)`. Zusammen mit `if` Anweisungen, es gibt eine Vielzahl von Möglichkeiten zum Testen von Bedingungen, wiederholen Sie Codeblöcke und usw., die sind weiter unten in diesem Artikel beschrieben.

Das Ergebnis in einem Browser angezeigt (nach dem Klicken auf **senden**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET und POST-Methoden und die IsPost-Eigenschaft
> 
> Das Protokoll für Webseiten (HTTP) unterstützt eine sehr begrenzte Anzahl von Methoden (Verben), die verwendet werden, um Anforderungen an den Server zu senden. Die zwei häufigsten Gründe sind GET, die zum Lesen einer Seite verwendet wird, und POST, das verwendet wird, um eine Seite zu übermitteln. Zum ersten Mal, das ein Benutzer eine Seite anfordert, wird im Allgemeinen unter Verwendung von GET eine Seite angefordert. Wenn der Benutzer in einem Formular ausgefüllt und klickt dann auf eine Schaltfläche "Senden", sendet der Browser eine POST-Anforderung an dem Server an.
> 
> In Web-Programmierung ist es oft hilfreich zu wissen, ob eine Seite als GET oder POST angefordert wird, damit Sie wissen, wie auf die Seite zu verarbeiten. In ASP.NET Web Pages, können Sie die `IsPost` Eigenschaft, um festzustellen, ob eine Anforderung eine Get- oder POST ist. Wenn die Anforderung eine POST-ist der `IsPost` Eigenschaft "true" zurück, und Sie können Aktionen wie lesen die Werte von Textfeldern in einem Formular. Viele Beispiele, die Sie sehen, in dem Sie zeigen, wie zum Verarbeiten der Seite hängt der Wert des `IsPost`.


## <a name="a-simple-code-example"></a>Ein einfaches Codebeispiel

Dieses Verfahren zeigt, wie Sie eine Seite erstellen, die die grundlegenden Programmiertechniken veranschaulicht. Im Beispiel erstellen Sie eine Seite, mit dem Benutzer, die zwei Zahlen eingeben, dann fügt sie hinzu, und das Ergebnis wird angezeigt.

1. Klicken Sie in Ihrem Editor, erstellen Sie eine neue Datei, und nennen sie *AddNumbers.cshtml*.
2. Kopieren Sie den folgenden Code und Markup auf der Seite, und Ersetzen Sie dabei alle Elemente bereits in der Seite an.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Es gibt einige Dinge beachten Sie:

    - Die `@` Zeichen beginnt des erste Codeblocks auf der Seite, und ihm vorausgeht der `totalMessage` Variable, die am unteren Rand der Seite eingebettet ist.
    - Der Block am oberen Rand der Seite wird in Klammern eingeschlossen.
    - Am Anfang des Blocks enden alle Zeilen mit einem Semikolon.
    - Die Variablen `total`, `num1`, `num2`, und `totalMessage` mehrere Zahlen und einer Zeichenfolge zu speichern.
    - Der Zeichenfolgenliteral-Wert, der zugewiesen der `totalMessage` Variable ist in doppelte Anführungszeichen.
    - Da der Code beim Groß-/Kleinschreibung beachtet, ist die `totalMessage` Variable, die am unteren Rand der Seite verwendet wird, muss der Name der Variablen am Anfang genau entsprechen.
    - Der Ausdruck `num1.AsInt() + num2.AsInt()` für die Arbeit mit Objekten und Methoden veranschaulicht. Die `AsInt` Methode für jede Variable konvertiert die Zeichenfolge, die von einem Benutzer in eine Zahl (Integer) eingegeben wird, sodass Sie arithmetische Operationen darauf ausführen können.
    - Die `<form>` Tag enthält eine `method="post"` Attribut. Dies gibt an, dass wenn der Benutzer klickt **hinzufügen**, die Seite an den Server mithilfe der HTTP-POST-Methode gesendet. Wenn die Seite gesendet wird, die `if(IsPost)` Test True ergibt, der bedingte code ausgeführt wird, und das Ergebnis der Addition der Zahlen angezeigt.
3. Speichern Sie die Seite, und führen Sie sie in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Geben Sie zwei ganzen Zahlen, und klicken Sie dann auf die **hinzufügen** Schaltfläche. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Grundlegende Programmierkonzepte

Dieser Artikel bietet eine Übersicht über ASP.NET Web-Programmierung. Es ist nicht für eine vollständige Prüfung, nur eine kurze Führung zusammengestellt über die Programmierkonzepte kennen, die Sie am häufigsten verwenden. Dennoch werden fast alles, was benötigen Sie für den Einstieg mit ASP.NET Web Pages behandelt.

Aber zuerst, eine kleine technischen Hintergrund.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>Die Razor-Syntax, Servercode und ASP.NET

Razor-Syntax ist eine einfache Programmiersyntax zum Einbetten von serverbasiertem Code in einer Webseite. Auf einer Webseite, die die Razor-Syntax verwendet, es gibt zwei Arten von Inhalten: Clientcode Inhalte und Server. Clientinhalt ist die Dinge, die Sie, um auf Webseiten verwendet werden: HTML-Markup (Elemente), Informationen zum Schriftschnitt wie CSS, vielleicht einige Clientskripts wie JavaScript und nur-Text.

Razor-Syntax können Sie diese Clients Inhalte Servercode hinzufügen. Liegt Servercode auf der Seite, führt der Server diesen Code zuerst, bevor die Seite an den Browser gesendet. Auf dem Server ausführen, kann der Code Aufgaben ausführen, die mit Clientinhalt alleine, wie Sie den Zugriff auf serverbasierte Datenbanken sehr viel komplizierter sein können. Der wichtigste Servercode kann Clientinhalt dynamisch erstellen &#8212; können sie HTML-Markup oder andere Inhalte nebenbei generieren und senden Sie sie an den Browser sowie die statische HTML-Elemente, die die Seite enthalten können. Aus Sicht des Browsers unterscheidet sich Clients Inhalte, die von Ihrem Code generiert wird nicht als jeder andere Client-Inhalte. Wie Sie bereits gesehen haben, befindet sich der Code, der erforderlich ist recht einfach.

ASP.NET-Webseiten, die die Razor-Syntax enthalten haben eine besondere Dateinamenerweiterung (*.cshtml* oder *vbhtml*). Der Server erkennt diese Erweiterungen, führt den Code, der mit Razor-Syntax markiert ist, und klicken Sie dann die Seite an den Browser sendet.

### <a name="where-does-aspnet-fit-in"></a>Wo ist ASP.NET angebracht?

Razor-Syntax basiert auf einer Technologie von Microsoft, die Namen ASP.NET, die wiederum auf Microsoft .NET Framework basiert. Der.NET Framework ist ein großes, umfassende Programmierframework von Microsoft für die Entwicklung von praktisch jeder Art von Computer-Anwendung. ASP.NET ist der Teil von .NET Framework, die speziell zum Erstellen von Webanwendungen konzipiert ist. Entwickler haben die ASP.NET verwendet, um viele der größten und höchsten Datenverkehr Websites in der ganzen Welt zu erstellen. (Jedes Mal, die die Dateinamenerweiterung daraufhin *aspx* als Teil der URL an einem Standort, wissen Sie, dass die Website mit ASP.NET geschrieben wurde.)

Die Razor-Syntax bietet Ihnen die Leistungsfähigkeit der ASP.NET, aber verwenden eine vereinfachte Syntax, der leichter erfahren Sie, ob Sie Anfänger sind und Sie produktiver stellt, wenn Sie ein Experte sind. Obwohl diese Syntax einfach zu verwenden ist, bedeutet die Familie Beziehung zu ASP.NET und .NET Framework, wie Ihre Websites komplexer werden, Sie die Leistungsfähigkeit der größeren Frameworks zur Verfügung haben.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Klassen und Instanzen**
> 
> ASP.NET-Servercode: verwendet Objekte, die wiederum auf der Idee von Klassen erstellt werden. Die Klasse ist die Definition oder eine Vorlage für ein Objekt. Beispielsweise kann eine Anwendung enthalten eine `Customer` Klasse, die definiert, die Eigenschaften und Methoden, die alle Customer-Objekt.
> 
> Wenn die Anwendung mit tatsächlichen Kundeninformationen arbeiten muss, erstellt er eine Instanz von (oder *instanziiert*) eine Customer-Objekt. Jeder einzelne Kunde wird eine separate Instanz von der `Customer` Klasse. Jede Instanz unterstützt die gleichen Eigenschaften und Methoden, aber die Eigenschaftswerte für die einzelnen Instanzen unterscheiden sich in der Regel, da jedes Kundenobjekt eindeutig ist. In einer Customer-Objekt das `LastName` Eigenschaft möglicherweise "Smith"; in einer anderen Customer-Objekt, das `LastName` Eigenschaft möglicherweise "Jones" ab.
> 
> Jeder einzelne Webseite auf Ihrer Website auf ähnliche Weise wird eine `Page` -Objekt, das eine Instanz von der `Page` Klasse. Eine Schaltfläche auf der Seite ist eine `Button` -Objekt, das eine Instanz von der `Button` -Klasse, und So weiter. Jede Instanz verfügt über eigene Eigenschaften, aber alle basieren auf den in der Definition der Klasse des Objekts angegebenen.


## <a name="basic-syntax"></a>Allgemeine Syntax

Zuvor haben Sie ein einfaches Beispiel zum Erstellen einer ASP.NET Web Pages-Seite, und wie Sie HTML-Markup Servercode hinzufügen können. Hier erfahren Sie die Grundlagen zum Schreiben von ASP.NET-Code-Server mithilfe der Razor-Syntax &#8212; , also die programming Language-Regeln.

Wenn Sie bereits über Erfahrung mit Programmierung (insbesondere, wenn Sie C#, C++, c#, Visual Basic oder JavaScript verwendet haben), werden viele der hier Sie lesen vertraut sein. Sie müssen wahrscheinlich machen Sie sich nur mit Markup wie Server-Code hinzugefügt wird *.cshtml* Dateien.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>Kombinieren von Text, Markup und Code in Codeblöcken

In Server-Codeblöcken möchten Sie häufig Ausgabe Text oder Markup (oder beides) auf der Seite. Wenn ein Server-Codeblock Text enthält, die nicht Code und, stattdessen gerendert werden soll, wie, muss ASP.NET Text von Code zu unterscheiden können. Dafür stehen verschiedene Möglichkeiten zur Verfügung:

- Schließen Sie den Text in einem HTML-Element, z. B. `<p></p>` oder `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn erkennt ASP.NET das öffnende HTML-Tag (z. B. `<p>`), sondert rendert alle Elemente einschließlich des-Elements und dessen Inhalt als wird an den Browser, und Beheben von Servercode Ausdrücke, wie es geht.
- Verwenden der `@:` Operator oder die `<text>` Element. Die `@:` Ausgaben eine einzelne Zeile des Inhalts, der nur-Text oder HTML-Tags für nicht übereinstimmenden; enthält die `<text>` Element einschließt, mehrere Zeilen ausgegeben. Diese Optionen sind nützlich, wenn Sie nicht, ein HTML-Element als Teil der Ausgabe gerendert möchten.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Wenn Sie mehrere Zeilen Text oder HTML-Tags, die nicht übereinstimmenden ausgeben möchten, können Sie jede Zeile mit dem voranstellen `@:`, oder Sie können einschließen, die Zeile in einer `<text>` Element. Wie die `@:` Operator`<text>` Tags werden von ASP.NET zum Identifizieren von Textinhalt verwendet und nie in der Seitenausgabe gerendert werden.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    Im erste Beispiel wird das vorherige Beispiel wiederholt, aber verwendet ein einzelnes Paar von `<text>` Tags aus, um den Text Rendern einschließen. Im zweiten Beispiel die `<text>` und `</text>` Tags einschließen drei Zeilen, die jeweils einige nicht enthaltene Text und HTML-Tags, die nicht übereinstimmenden sind (`<br />`), sowie der Servercode und HTML-Tags, die übereinstimmende. In diesem Fall können Sie auch jede Zeile einzeln mit voranstellen der `@:` Operator; beide Weise funktionieren.

    > [!NOTE]
    > Wenn Sie Text ausgeben, wie in diesem Abschnitt gezeigt &#8212; mithilfe eines HTML-Elements, die `@:` -Operator, oder die `<text>` Element &#8212; ASP.NET die Ausgabe nicht HTML-Codierung. (Wie bereits erwähnt, ASP.NET codiert die Ausgabe von Codeausdrücken Server und Server-Codeblöcke, die vor `@`, außer in die Sonderfälle, in diesem Abschnitt erwähnt wurden.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer Anweisung (und außerhalb eines Zeichenfolgenliterals) wirken sich nicht auf die Anweisung aus:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Ein Zeilenumbruch in einer Anweisung hat keine Auswirkungen auf die Anweisung aus, und können Sie Anweisungen zur besseren Lesbarkeit umbrochen. Die folgenden Anweisungen sind identisch:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

Sie können keine jedoch eine Zeile in der Mitte eines Zeichenfolgenliterals umschließen. Das folgende Beispiel funktioniert nicht:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Es gibt zwei Optionen, um eine lange Zeichenfolge zu kombinieren, die in mehrere Zeilen wie im obigen Code umschließt. Sie können den Operator für Verkettungen verwenden (`+`), die Sie später in diesem Artikel sehen werden. Sie können auch die `@` Zeichen, das eine wörtliche Zeichenfolge literal erstellt, wie Sie weiter oben in diesem Artikel gesehen haben. Wörtliche Zeichenfolgenliterale können Sie über mehrere Zeilen unterteilen:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Code (und Markup) Kommentare

Kommentare können Sie Notizen für sich selbst oder andere zu verlassen. Sie ermöglichen auch deaktivieren (*auskommentieren*) einen Abschnitt des Codes oder Markups, die Sie nicht ausführen möchten, jedoch auf Ihrer Seite vorerst beibehalten möchten.

Es gibt verschiedene kommentieren die Syntax für Razor-Code und HTML-Markup. Wie bei allen Razor-Code werden Razor-Kommentare verarbeitet (und anschließend entfernt) auf dem Server, bevor die Seite an den Browser gesendet wird. Aus diesem Grund kann die Kommentierung Razor-Syntax Sie Kommentare im Code (oder sogar in das Markup) bereitstellen, die Sie sehen können, wenn Sie die Datei bearbeiten, aber Benutzern nicht angezeigt wird, auch in den Quellcode der Seite.

Für ASP.NET Razor-Kommentare, starten Sie den Kommentar mit `@*` und enden mit `*@`. Der Kommentar kann auf eine oder mehrere Zeilen sein:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Hier ist ein Kommentar in einem Codeblock:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Hier ist der gleiche Codeblock, mit der Zeile des Codes auskommentiert, damit er nicht ausgeführt wird:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

In einem Codeblock können Sie als Alternative zur Verwendung von Razor-Kommentar-Syntax, die Kommentierung Syntax der Programmiersprache verwenden, die Sie, die wie z. B. c# verwenden:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

In c# einzeilige Kommentare vorangestellt sind, wird die `//` Zeichen und mehrzeilige Kommentare beginnen mit `/*` und enden mit `*/`. (Wie bei der Razor-Kommentare werden C#-Kommentare nicht an den Browser gerendert.)

Für Markup wie Sie wahrscheinlich wissen, können Sie einen HTML-Kommentar erstellen:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

HTML-Kommentare beginnen mit `<!--` Zeichen und enden mit `-->`. Sie können HTML-Kommentare verwenden, um umgeben, nicht nur Text, sondern auch für jedes HTML-Markup, die Sie auf der Seite beibehalten möchten, aber nicht gerendert werden soll. Diese HTML-Kommentar wird den gesamten Inhalt des Tags und den darin enthaltenen Text ausblenden:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Im Gegensatz zu Razor-Kommentare, HTML-Kommentaren *sind* auf der Seite gerendert und der Benutzer kann den Quellcode der Seite anzeigen.

Razor kann Einschränkungen auf geschachtelte Blöcke von c#. Weitere Informationen finden Sie unter [mit dem Namen C#-Variablen und geschachtelte Blöcke unterteilt Code generieren](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variablen

Eine Variable ist ein benanntes Objekt, das Sie verwenden, um Daten zu speichern. Sie können Variablen Namen, aber der Name muss mit einem alphabetischen Zeichen beginnen, und es darf keine Leerzeichen oder reservierte Zeichen enthalten.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable haben einen bestimmten Datentyp, der angibt, welche Art von Daten in der Variablen gespeichert ist. Können Sie Zeichenfolgenvariablen, in denen Zeichenfolgenwerte gespeichert haben (z. B. &quot;Hallo Welt&quot;), Ganzzahl-Variablen, die ganzzahlige Werte (z. B. 3 oder 79) speichern, und die Datenvariablen, die Datumswerte in einer Vielzahl von Formaten (z. B. 4/12/2012 oder März 2009 speichern ). Und es gibt viele andere Datentypen, die Sie verwenden können.

Allerdings müssen Sie in der Regel einen Typ für eine Variable angeben. In den meisten Fällen, kann ASP.NET ermitteln den Typ basierend auf wie die Daten in der Variablen verwendet wird. (Gelegentlich müssen Sie einen Typ angeben, sehen Sie Beispiele, in denen dies "true".)

Sie deklarieren eine Variable mit dem `var` Schlüsselwort (sofern Sie nicht möchten, dass für einen Typ angegeben wird) oder mit dem Namen des Typs:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

Das folgende Beispiel zeigt einige typischen Verwendungen von Variablen in einer Webseite ein:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Wenn Sie die vorherigen Beispielen auf einer Seite kombinieren, sehen Sie dies in einem Browser angezeigt:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konvertieren und Testen von Datentypen

Obwohl ASP.NET einen Datentyp in der Regel automatisch ermitteln kann, manchmal kann es nicht. Aus diesem Grund müssen Sie ASP.NET weiterhelfen, indem Sie eine explizite Konvertierung ausführen. Auch wenn Sie keine Typen zu konvertieren, ist es manchmal hilfreich, testen, welche Art von Daten Sie verwenden können.

Der häufigste Fall ist, dass Sie zum Konvertieren von einer Zeichenfolge in einen anderen Typ, z. B. auf eine ganze Zahl oder Datum. Das folgende Beispiel zeigt einem typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Als Faustregel gilt wird der Benutzereingabe für Sie als Zeichenfolgen bereitgestellt. Auch wenn Sie haben die Benutzer eine Zahl eingeben aufgefordert, und auch wenn sie bei der Eingabe des Benutzers gesendet wird, und Sie finden es im Code eine Ziffer eingegeben haben, die Daten im Zeichenfolgenformat werden. Aus diesem Grund müssen Sie die Zeichenfolge in eine Zahl konvertieren. Im Beispiel wenn Sie versuchen, die Durchführung von Berechnungen auf den Werten, ohne zu konvertieren, führt der folgende Fehler, da zwei Zeichenfolgen von ASP.NET hinzugefügt werden können:

*Typ "String" in "Int" kann nicht implizit konvertiert werden.*

Um die Werte in ganzen Zahlen zu konvertieren, rufen Sie die `AsInt` Methode. Wenn die Konvertierung erfolgreich ist, können Sie dann die Zahlen hinzufügen.

Die folgende Tabelle enthält einige allgemeine Konvertierung und Test-Methoden für die Variablen an.

:::row:::
    :::column:::
        <strong>Method</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Example</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        Converts a string that represents a whole number (like "593") to an integer.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number. (In ASP.NET, a decimal number is more precise than a floating-point number.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        Converts a string that represents a date and time value to the ASP.NET `DateTime` type.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        Converts any other data type to a string.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    :::column-end:::
:::row-end:::

## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder das Zeichen, das ASP.NET darüber informiert, welche Art von Befehl aus, um in einem Ausdruck durchgeführt werden. Der C#-Sprache (und die Razor-Syntax, die darauf basieren) unterstützt viele Operatoren müssen Sie nur ein Paar für den Einstieg zu erkennen. In der folgende Tabelle werden die am häufigsten verwendeten Operatoren zusammengefasst.


:::row:::
    :::column:::
        <strong>Operator</strong>
    :::column-end:::
    :::column:::
        <strong>Description</strong>
    :::column-end:::
    :::column:::
        <strong>Examples</strong>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+` `-` `*` `/`
    :::column-end:::
    :::column:::
        Math operators used in numerical expressions.
    :::column-end:::
    :::column:::
        [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        Assignment. Assigns the value on the right side of a statement to the object on the left side.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `==`
    :::column-end:::
    :::column:::
        Equality. Returns `true` if the values are equal. (Notice the distinction between the `=` operator and the `==` operator.)
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!=`
    :::column-end:::
    :::column:::
        Inequality. Returns `true` if the values are not equal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+`
    :::column-end:::
    :::column:::
        Concatenation, which is used to join strings. ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+=` `-=`
    :::column-end:::
    :::column:::
        The increment and decrement operators, which add and subtract 1 (respectively) from a variable.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        Dot. Used to distinguish objects and their properties and methods.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        Parentheses. Used to group expressions and to pass parameters to methods.
    :::column-end:::
    :::column:::
        [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `[]`
    :::column-end:::
    :::column:::
        Brackets. Used for accessing values in arrays or collections.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `!`
    :::column-end:::
    :::column:::
        Not. Reverses a `true` value to `false` and vice versa. Typically used as a shorthand way to test for `false` (that is, for not `true`).
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&&` <code>&#124;&#124;</code>
    :::column-end:::
    :::column:::
        Logical AND and OR, which are used to link conditions together.
    :::column-end:::
    :::column:::
        [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    :::column-end:::
:::row-end:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Arbeiten mit Datei- und Ordnerpfade in Code

Sie werden häufig mit Datei- und Ordnerpfade in Ihrem Code arbeiten. Hier ist ein Beispiel der physischen Ordnerstruktur für eine Website auf, wie es auf Ihrem Entwicklungscomputer angezeigt werden kann:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Hier sind einige wichtigen Informationen über URLs und Pfade:

- Eine URL beginnt entweder mit einen Domänennamen (`http://www.example.com`) oder einen Servernamen (`http://localhost`, `http://mycomputer`).
- Eine URL entspricht einem physischen Pfad auf einem Hostcomputer. Z. B. `http://myserver` möglicherweise zu dem Ordner entsprechen *C:\websites\mywebsite* auf dem Server.
- Ein virtueller Pfad ist die Abkürzung, um Pfade im Code darzustellen, ohne den vollständigen Pfad angeben zu müssen. Es umfasst den Teil einer URL, die der Domäne oder Server Name folgt. Wenn Sie die virtuellen Pfade verwenden, können Sie Ihren Code zu einer anderen Domäne oder Server verschieben, ohne die Pfade aktualisieren zu müssen.

Hier ist ein Beispiel hilft Ihnen die Unterschiede zu verstehen:

| Vollständige URL | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Servername | *mycompanyserver* |
| Virtueller Pfad | */humanresources/CompanyPolicy.htm* |
| Physischer Pfad | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Das virtuelle Stammverzeichnis ist /, genau wie der Stamm von Laufwerk C: Laufwerk \. (Virtuelle Ordnerpfade verwenden immer Schrägstriche.) Der virtuelle Pfad eines Ordners muss nicht den gleichen Namen wie der physische Ordner haben; Es kann ein Alias sein. (Auf Produktionsservern entspricht der virtuelle Pfad nur selten einen genaue physischen Pfad.)

Bei der Arbeit mit Dateien und Ordner im Code, müssen Sie gelegentlich verweisen auf den physischen Pfad und manchmal auf einen virtuellen Pfad, je nachdem welche Objekte, die mit dem Sie arbeiten. ASP.NET bietet Ihnen diese Tools zum Arbeiten mit Datei- und Ordnerpfade im Code: die `Server.MapPath` -Methode, und die `~` Operator und `Href` Methode.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konvertieren von virtuellen und physischen Pfade: die Server.MapPath-Methode

Die `Server.MapPath` Methode konvertiert einen virtuellen Pfad (z. B. */default.cshtml*), ein absoluter physischer Pfad (z. B. *C:\WebSites\MyWebSiteFolder\default.cshtml*). Sie verwenden diese Methode können Sie jederzeit einen vollständigen physischen Pfad. Ein typisches Beispiel ist beim Lesen oder Schreiben einer Text- oder Image-Datei auf dem Webserver.

Sie können die absoluten physischen Pfad der Website auf einer hosting-Site-Server in der Regel nicht kennen, damit diese Methode konvertieren des Pfads können Sie wissen – der virtuelle Pfad, in den entsprechenden Pfad auf dem Server für Sie. Sie den virtuellen Pfad zu einer Datei oder Ordner für die Methode übergeben, und es gibt den physischen Pfad:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und Href-Methode

In einer *.cshtml* oder *vbhtml* -Datei können Sie verweisen, den virtuellen Stamm-Pfad mit der `~` Operator. Dies ist sehr praktisch, da Sie Seiten, an einem Standort navigieren können, und alle Links zu anderen Seiten enthaltenen nicht unterbrochen. Es ist auch praktisch, für den Fall, dass Sie Ihre Website immer an einem anderen Speicherort verschieben. Hier einige Beispiele:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Wenn die Website ist `http://myserver/myapp`, sieht wie ASP.NET diese Pfade behandelt wird, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade nicht tatsächlich als die Werte der Variablen angezeigt, aber ASP.NET die Pfade behandelt, als ob einstellungsänderungen ist.)

Sie können die `~` Operator sowohl im Server-Code (wie oben erläutert) und im Markup wie folgt:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Im Markup, das Sie verwenden die `~` Operator, um Pfade zu den Ressourcen, wie Bilddateien, andere Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, durchsucht Sie die Seite (Code und Markup) und behebt alle ASP.NET die `~` Verweise auf den entsprechenden Pfad.

## <a name="conditional-logic-and-loops"></a>Bedingungen und Schleifen

ASP.NET Server-Code können Sie die Aufgaben, die basierend auf Bedingungen, und Schreiben von Code, der Anweisungen wird wiederholt, eine bestimmte Anzahl von Malen (d. h. Code, der eine Schleife ausgeführt wird).

### <a name="testing-conditions"></a>Testen von Bedingungen

Um eine einfache Bedingung zu testen, Sie verwenden, die `if` -Anweisung, die einen Test gibt "true" oder "false" Grundlage Sie angeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

Die `if` Schlüsselwort beginnt einen Block. Der eigentliche Test (Bedingung) wird in Klammern angegeben und gibt "true" oder "false" zurück. Die Anweisungen, die ausgeführt werden, wenn der Test "true" ist, werden in geschweifte Klammern eingeschlossen. Ein `if` -Anweisung kann enthalten eine `else` Block, der angibt, Anweisungen, die ausgeführt werden, wenn die Bedingung "false" ist:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Sie können mehrere Bedingungen mit Hinzufügen einer `else if` blockieren:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

In diesem Beispiel, wenn die erste in der If-Bedingung Block ist nicht "true", die `else if` Bedingung aktiviert ist. Wenn diese Bedingung erfüllt ist, die Anweisungen in der `else if` Block ausgeführt werden. Wenn keine der Bedingungen erfüllt sind, die Anweisungen in der `else` Block ausgeführt werden. Sie können eine beliebige Anzahl von ElseIf hinzufügen blockiert, und schließen Sie dann mit einer `else` als blockiert die &quot;alles&quot; Bedingung.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie eine `switch` blockieren:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

Der zu testende Wert wird in Klammern angegeben (im Beispiel die `weekday` Variable). Jeder einzelne Test verwendet einen `case` -Anweisung, die mit einem Doppelpunkt (:).) endet. Wenn der Wert des einem `case` Anweisung entspricht dem Testwert, der Code in diesen Fall Block wird ausgeführt. Schließen Sie jede Case-Anweisung mit einem `break` Anweisung. (Wenn Sie vergessen, jede Unterbrechung einschließt `case` blockieren, den Code aus der nächsten `case` Anweisung auch ausgeführt werden.) Ein `switch` Block verfügt oft über eine `default` -Anweisung als im letzten Fall für eine &quot;alles&quot; Option aus, die ausgeführt wird, wenn keine anderen Fällen erfüllt sind.

Das Ergebnis der letzten beiden bedingte Blöcke in einem Browser angezeigt:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Schleifen-Code

Sie müssen häufig die gleichen Anweisungen mehrmals ausgeführt werden. Dazu müssen Sie Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Auflistung von Daten. Wenn Sie wissen, wie oft Sie möchten eine Schleife, können Sie eine `for` Schleife. Diese Art von Schleife eignet sich besonders für zugewiesen, oder nach unten zählen:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

Die Schleife beginnt mit der `for` Schlüsselwort, gefolgt von drei Anweisungen in Klammern ein, jeweils mit einem Semikolon abgeschlossen.

- Innerhalb der Klammern, die erste Anweisung (`var i=10;`) einen Leistungsindikator erstellt und initialisiert es mit 10. Sie müssen keine benennen Sie den Zähler `i` &#8212; können Sie eine beliebige Variable. Wenn die `for` Schleife ausgeführt wird, wird der Indikator wird automatisch aktualisiert.
- Die zweite Anweisung (`i < 21;`) legt die Bedingung für die wie weit Sie zählen möchten. In diesem Fall möchten Sie ihn an, auf ein Maximum von 20 (d. h. weiternutzen während der Zähler auf weniger als 21 ist).
- Die dritte Anweisung (`i++` ) Inkrement-Operators, der einfach gibt an, dass der Zähler sollte 1 hinzugefügt wird bei jeder Ausführung die Schleife verwendet.

Innerhalb der geschweiften Klammern ist Sie der Code, der für jede Iteration der Schleife ausgeführt wird. Das Markup erstellt einen neuen Absatz (`<p>` Element) jedes Mal, und fügt eine Zeile in die Ausgabe, sodass der Wert `i` (den Zähler). Wenn Sie diese Seite ausführen, wird im Beispiel 11 Zeilen anzeigen der Ausgabe, mit dem Text in jeder Zeile an, der angibt, der Elementnummer erstellt.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Wenn Sie mit einer Auflistung oder ein Array arbeiten, verwenden Sie häufig eine `foreach` Schleife. Eine Auflistung ist eine Gruppe ähnlicher Objekte und die `foreach` Schleife können Sie eine Aufgabe für jedes Element in der Auflistung durchführen. Dieser Art von Schleife eignet sich für Auflistungen, da im Gegensatz zu einem `for` Schleife, Sie müssen keine Erhöhen des Zählerwerts oder ein Limit festlegen. Stattdessen die `foreach` Schleifen-Code wird einfach in der Auflistung fortgesetzt, bis er abgeschlossen ist.

Der folgende Code gibt beispielsweise die Elemente in der `Request.ServerVariables` Auflistung, die ein Objekt, das Informationen über den Webserver enthält. Er verwendet eine `foreac` h-Schleife, um den Namen der einzelnen Elemente anzuzeigen, durch Erstellen eines neuen `<li>` Element in einer HTML-Aufzählung.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

Die `foreach` Schlüsselwort ist, gefolgt von Klammern deklarieren, in dem Sie eine Variable, die ein einzelnes Element in der Auflistung darstellt (beispielsweise `var item`), gefolgt von der `in` Schlüsselwort, gefolgt von der Auflistung durchlaufen werden sollen. Im Hauptteil der `foreach` Schleife können Sie das aktuelle Element, das mit der Variablen, die Sie zuvor deklarierten zugreifen.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Verwenden Sie zum Erstellen einer allgemeinen Schleife die `while` Anweisung:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Ein `while` Schleife beginnt mit der `while` Schlüsselwort, gefolgt von Klammern für die Sie angeben, wie lange die Schleife fortgesetzt wird (hier, um so lange `countNum` beträgt weniger als 50), klicken Sie dann den Block wiederholt werden soll. Schleifen in der Regel erhöhen (hinzugefügt) oder zu verringern (subtrahiert) eine Variable oder ein Objekt, das für die Inventur verwendet. Im Beispiel die `+=` Operator addiert 1 zu `countNum` bei jeder Ausführung die Schleife. (Um eine Variable in einer Schleife zu verringern, die wird nach unten gezählt, verwenden Sie des Dekrementoperators `-=`).

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Fast alles in einer ASP.NET-Website ist ein Objekt, einschließlich der Webseite selbst. Dieser Abschnitt beschreibt einige wichtige Objekte, die Sie häufig in Ihrem Code verwenden werden.

### <a name="page-objects"></a>Page-Objekte

Die meisten grundlegenden Objekts in ASP.NET ist die Seite. Sie können die Eigenschaften der Page-Objekt, ohne alle berechtigten Objekte direkt zugreifen. Der folgende Code Ruft den Dateipfad der Seite mit den `Request` Objekt der Seite:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Um es zu machen klar, dass Sie auf Eigenschaften und Methoden für das Seitenobjekt der aktuellen verweisen, optional können Sie das Schlüsselwort `this` das Page-Objekt in Ihrem Code darstellen. Hier ist das vorherige Codebeispiel, bei dem `this` zur Darstellung der Seite hinzugefügt:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Können Sie Eigenschaften der `Page` Objekt um eine Vielzahl von Informationen, wie z. B. zu erhalten:

- `Request`. Wie Sie bereits gesehen haben, ist dies eine Auflistung von Informationen über die aktuelle Anforderung, einschließlich welche Art von Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. vorgenommen.
- `Response`. Dies ist eine Auflistung von Informationen über die Antwort (Seite), die an den Browser gesendet wird, wenn der Code ausgeführt wurde. Beispielsweise können Sie diese Eigenschaft, um Informationen in die Antwort zu schreiben. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Von Auflistungsobjekten (Arrays und Dictionarys)

Ein *Auflistung* ist eine Gruppe von Objekten desselben Typs, z. B. eine Auflistung von `Customer` Objekte aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, z.B. die `Request.Files` Auflistung.

Sie werden häufig in Sammlungen mit Daten arbeiten. Zwei allgemeine Auflistungstypen sind die *Array* und *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Sammlung mit ähnlichen Elementen gespeichert werden soll, aber nicht, erstellen Sie eine separate Variable für jedes Element möchten:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Mit Arrays, Sie deklarieren einen bestimmten Datentyp, z. B. `string`, `int`, oder `DateTime`. Um anzugeben, dass die Variable darf ein Array, das Sie Klammern zur Deklaration hinzufügen (z. B. `string[]` oder `int[]`). Es stehen die Elemente in einem Array mit ihrer Position (Index) oder mithilfe der `foreach` Anweisung. Arrayindizes sind nullbasiert &#8212; , also das erste Element ist an position 0, das zweite Element ist an Position 1 und So weiter.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Sie können die Anzahl der Elemente in einem Array zu bestimmen, durch Abrufen der `Length` Eigenschaft. Rufen Sie die Position eines bestimmten Elements im Array (und das Array zu suchen) mit der `Array.IndexOf` Methode. Sie können auch Aktionen wie Reverse den Inhalt eines Arrays (die `Array.Reverse` Methode) oder den Inhalt zu sortieren (die `Array.Sort` Methode).

Die Ausgabe von der Zeichenfolgen-Array-Code in einem Browser angezeigt:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel/Wert-Paare, in dem Sie den Schlüssel (oder Name) festlegen oder Abrufen des entsprechenden Werts angeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Um ein Wörterbuch zu erstellen, verwenden Sie die `new` Schlüsselwort, um anzugeben, dass Sie ein neues Wörterbuchobjekt erstellen. Sie können ein Wörterbuch zuweisen, um eine Variable mit dem `var` Schlüsselwort. Sie zeigen die Daten der Elemente in das Wörterbuch, das mithilfe der spitzen Klammern ( `< >` ). Am Ende der Deklaration müssen Sie ein Paar von Klammern, hinzufügen, da es sich eigentlich eine Methode handelt, die ein neues Wörterbuch erstellt.

Um Elemente zum Wörterbuch hinzuzufügen, rufen Sie die `Add` Methode der "Dictionary"-Variable (`myScores` in diesem Fall), und geben Sie einen Schlüssel und Wert. Alternativ können Sie eckige Klammern verwenden, um den Schlüssel angeben, und führen eine einfache Zuweisung, wie im folgenden Beispiel:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Um einen Wert aus dem Wörterbuch abzurufen, geben Sie den Schlüssel in Klammern ein:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

Wenn Sie zuvor in diesem Artikel lesen, können die Objekte, denen Sie Programmieren mit Methoden verfügen. Z. B. eine `Database` Objekt möglicherweise eine `Database.Connect` Methode. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, der Sie an eine Methode übergeben, um die Methode zum Abschließen des Tasks zu aktivieren. Betrachten Sie z. B. eine Deklaration für die `Request.MapPath` -Methode, die drei Parameter akzeptiert:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(Die Zeile wurde umgebrochen, um sie besser lesbar zu machen. Beachten Sie, dass Sie Zeilenumbrüche einfügen können, fast jeden Ort, mit der Ausnahme, die innerhalb von Zeichenfolgen, in Anführungszeichen eingeschlossen sind.)

Diese Methode gibt den physischen Pfad auf dem Server, der entspricht einem angegebenen virtuellen Pfad zurück. Die drei Parameter für die Methode sind `virtualPath`, `baseVirtualDir`, und `allowCrossAppMapping`. (Beachten Sie, dass in der Deklaration, die Parameter mit den Datentypen der Daten aufgeführt sind, die eingegeben werden.) Wenn Sie diese Methode aufrufen, müssen Sie Werte für alle drei Parameter angeben.

Die Razor-Syntax bietet Ihnen zwei Optionen zum Übergeben von Parametern an eine Methode: *Positionsparameter* und *benannte Parameter*. Um eine Methode mithilfe von positionelle Parameter aufzurufen, übergeben Sie die Parameter in einem strikter Reihenfolge, der in der Deklaration der Methode angegeben ist. (Sie würden diese Reihenfolge in der Regel kennen, lesen Sie die Dokumentation für die Methode.) Führen Sie die Reihenfolge, und Sie können einen der Parameter nicht überspringen &#8212; erforderlich, eine leere Zeichenfolge übergeben (`""`) oder `null` für einen positionelle Parameter, die einen Wert für nicht verfügbar.

Im folgende Beispiel wird angenommen, Sie haben einen Ordner namens *Skripts* auf Ihrer Website. Der Code Ruft die `Request.MapPath` -Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Dann wird den resultierenden zugeordneten Pfad angezeigt.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Wenn eine Methode über viele Parameter verfügt, können Sie Ihren Code besser lesbar behalten mit benannten Parametern. Um eine Methode, die mit benannten Parametern aufzurufen, geben Sie an, gefolgt von der Parameternamen von einem Doppelpunkt (:), und klicken Sie dann den Wert. Der Vorteil für benannte Parameter ist, dass Sie sie in beliebiger Reihenfolge übergeben können, werden sollen. (Ein Nachteil ist, dass der Methodenaufruf nicht so kompakt ist).

Im folgenden Beispiel wird die gleiche Methode wie oben beschrieben, aber verwendet benannte Parameter, die Werte anzugeben:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übergeben. Wenn Sie im vorherigen Beispiel und in diesem Beispiel ausführen, werden sie jedoch den gleichen Wert zurück.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-Catch-Anweisungen

Sie müssen oft Anweisungen in Ihrem Code, die außerhalb Ihrer Kontrolle Gründen fehlschlagen kann. Zum Beispiel:

- Wenn Ihr Code versucht, erstellen oder auf eine Datei zugreifen, können alle möglichen Fehler auftreten. Die gewünschte Datei nicht vorhanden sind, wird sie möglicherweise gesperrt, der Code möglicherweise nicht über Berechtigungen verfügen, und so weiter.
- Auf ähnliche Weise Wenn Ihr Code versucht, die Datensätze in einer Datenbank zu aktualisieren, können Berechtigungsprobleme vorhanden sein, kann die Verbindung mit der Datenbank gelöscht werden, die zu speichernden Daten möglicherweise ungültig und so weiter.

Programmiertechnisch ausgedrückt, werden diese Situationen aufgerufen *Ausnahmen*. Wenn Ihr Code eine Ausnahme auftritt, generiert er (auslöst) eine Fehlermeldung angezeigt, die es im besten Fall ziemlich ärgerlich, für Benutzer:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

In Situationen, in denen Ihr Code kann Ausnahmen auftreten, und um Fehler Nachrichten dieses Typs zu vermeiden, können Sie `try/catch` Anweisungen. In der `try` -Anweisung, führen Sie den Code, die Sie überprüfen können. In einer oder mehreren `catch` -Anweisungen, sehen Sie nach bestimmten Fehlern (bestimmte Arten von Ausnahmen), die aufgetreten sind. Sie können beliebig viele einschließen `catch` Anweisungen, wie Sie nach Fehlern zu suchen, die Sie planen müssen.

> [!NOTE]
> Es wird empfohlen, dass Sie, verwenden vermeiden die `Response.Redirect` -Methode in der `try/catch` -Anweisungen, da dies eine Ausnahme auf Ihrer Seite führen kann.


Das folgende Beispiel zeigt eine Seite, die auf der ersten Anforderung eine Textdatei erstellt und zeigt dann eine Schaltfläche der Benutzer die Datei zu öffnen. Im Beispiel verwendet absichtlich einen ungültigen Dateinamen ein, damit es eine Ausnahme ausgelöst wird. Der Code enthält `catch` -Anweisungen für die zwei möglichen Ausnahmen: `FileNotFoundException`, Dies tritt ein, wenn der Dateiname ungültig, ist und `DirectoryNotFoundException`, Dies tritt ein, wenn ASP.NET nicht den Ordner selbst finden können. (Sie können die auskommentierung Aufheben einer Anweisung im Beispiel um zu sehen, wie sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn Ihr Code die Ausnahme behandelt hat nicht, sehen Sie eine Fehlerseite angezeigt, wie im vorherigen Screenshot. Allerdings die `try/catch` Abschnitt hilft zu verhindern, dass den Benutzer diese Arten von Fehlern angezeigt.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

**Programmierung mit Visual Basic**


[Anhang: Visual Basic-Sprache und Syntax](https://go.microsoft.com/fwlink/?LinkId=202908)


**Referenzdokumentation**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[C#-Sprache](https://msdn.microsoft.com/library/kx37x362.aspx)
