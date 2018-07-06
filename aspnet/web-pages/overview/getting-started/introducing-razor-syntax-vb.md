---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic) | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Anhang enthält eine Übersicht der mit ASP.NET Web Pages-Programmierung in Visual Basic mit Razor-Syntax.
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 72f995e62141df4e8f4cd082b4873d82067af8c1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816547"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel bietet Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages mit der Razor-Syntax und Visual Basic. ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Webseiten auf Webservern.
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


Die meisten Beispiele für die Verwendung von ASP.NET Web Pages mit Razor-Syntax Verwenden von c#. Die Razor-Syntax unterstützt aber auch Visual Basic. Um eine ASP.NET-Webseite in Visual Basic zu programmieren, erstellen Sie eine Webseite mit einem *vbhtml* Dateierweiterung, und fügen Sie Visual Basic-Code hinzu. Dieser Artikel bietet Ihnen einen Überblick über die Arbeit mit Visual Basic-Sprache und Syntax zum Erstellen von ASP.NET Web Pages.

> [!NOTE]
> Die Standardvorlagen für die Website für Microsoft WebMatrix (**Bäckerei**, **Fotogalerie**, und **Starter Site**usw.) in c# und Visual Basic-Versionen verfügbar sind. Sie können die Visual Basic-Vorlagen von als NuGet-Pakete installieren. Websitevorlagen sind im Stammordner Ihrer Website in einen Ordner namens installiert *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Die besten Tipps für 8 Programmierung

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt benötigen wissen, wie Sie mithilfe der Razor-Syntax ASP.NET-Servercode: schreibe.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Sie fügen Code hinzu, um eine Seite mit dem @-Zeichen

Die `@` Inlineausdrücke, einzelnen Anweisung ausgeführt und mit mehreren Anweisungen Blöcke beginnt:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Wenn Sie Inhalt anzeigen, auf eine Seite mit den `@` Zeichen, wie in den obigen Beispielen ASP.NET HTML-Codierung der Ausgabe. Dies ersetzt die reservierte HTML-Zeichen (z. B. `<` und `>` und `&`) mit Codes, mit denen die Zeichen als Zeichen statt als HTML-Tags oder Entitäten interpretiert wird auf einer Webseite angezeigt werden können. Ohne HTML-Codierung, die aus Ihrem Code möglicherweise nicht richtig angezeigt, und möglicherweise eine Seite, um Sicherheitsrisiken ausgesetzt.
> 
> Wenn Ihr Ziel ist, um HTML-Markup auszugeben, die Tags als Markup rendert (z. B. `<p></p>` für einen Absatz oder `<em></em>` Text hervorgehoben), finden Sie im Abschnitt [Kombinieren von Text, Markup und Code in Codeblöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Erfahren Sie mehr über die HTML-Codierung [arbeiten mit HTML-Formularen in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Schließen Sie Codeblöcke mit Code... End-Code

Ein Codeblock enthält eine oder mehrere codeanweisungen und wird mit den Schlüsselwörtern eingeschlossen `Code` und `End Code`. Platzieren Sie das öffnende `Code` Schlüsselwort unmittelbar nach der `@` Zeichen &#8212; dazwischen können keine Leerzeichen vorhanden sein.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. In einem Block am Ende jeder Anweisung mit einem Zeilenumbruch

In einem Visual Basic-Codeblock endet jeder Anweisung ein Zeilenumbruch eingefügt. (Später in diesem Artikel wird eine Möglichkeit, eine lange codeanweisung in mehrere Zeilen umbrochen werden soll, falls erforderlich angezeigt.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie verwenden Variablen zum Speichern von Werten

Sie können Werte in speichern eine *Variable*, einschließlich Zeichenfolgen, Zahlen und Datumsangaben. Sie erstellen eine neue Variable mit dem `Dim` Schlüsselwort. Sie können Werte direkt in eine Seite mit einfügen `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie einschließen literale Zeichenfolgenwerte in Anführungszeichen

Ein *Zeichenfolge* ist eine Folge von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge angeben, schließen Sie ihn in doppelte Anführungszeichen ein:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Fügen Sie zum Einbetten von doppelten Anführungszeichen in einen Zeichenfolgenwert, zwei doppelte Anführungszeichen ein. Das doppelte Anführungszeichen in die Seitenausgabe einmal enthalten sein sollen, geben Sie ihn als `""` innerhalb der Anführungszeichen-Zeichenfolge ein, und wenn es zweimal angezeigt werden soll, geben Sie ihn als `""""` innerhalb der Zeichenfolge in Anführungszeichen.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic-Code ist nicht in der Groß-/Kleinschreibung beachten

Visual Basic-Sprache ist nicht in der Groß-/Kleinschreibung beachtet. Programmierung Schlüsselwörter (z. B. `Dim`, `If`, und `True`) und Variablennamen (wie `myString`, oder `subTotal`) in jedem Fall geschrieben werden können.

Die folgenden Codezeilen die Variable einen Wert zuweisen `lastname` mit Kleinbuchstaben benennen, und dann den Wert den Variablen auf der Seite mit einem Großbuchstaben Namen ausgeben.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Großteil Ihrer Programmierung umfasst das Arbeiten mit Objekten

Ein Objekt darstellt, eine Sache, die Sie programmieren können, mit &#8212; eine Seite, ein Textfeld, eine Datei, ein Bild, eine webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz (Datenbankzeile) usw. Objekte verfügen über Eigenschaften, die ihren Merkmalen beschreiben &#8212; ein Textfeld-Objekt verfügt über eine `Text` -Eigenschaft, ein Anforderungsobjekt verfügt über eine `Url` -Eigenschaft, die eine e-Mail-Nachricht verfügt über eine `From` -Eigenschaft, und ein Kundenobjekt verfügt über eine `FirstName` Diese Eigenschaft. Objekte verfügen außerdem über Methoden, die die &quot;Verben&quot; ausführen. Beispiele hierfür sind ein Dateiobjekt `Save` -Methode, ein Bildobjekt `Rotate` -Methode und eine e-Mail-Adresse des Objekts `Send` Methode.

Sie arbeiten häufig mit der `Request` -Objekt, das Sie Informationen wie die Werte des Formulars erhalten Felder auf der Seite (Textfelder usw.), welche Art von Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. vorgenommen. Dieses Beispiel veranschaulicht den Zugriff auf Eigenschaften von den `Request` -Objekt und das Aufrufen von der `MapPath` -Methode der der `Request` -Objekt, das Sie den absoluten Pfad der Seite auf dem Server erhalten:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, die Entscheidungen trifft

Ein wichtiges Feature von dynamischen Webseiten ist, dass auf Sie bestimmen können, was zu tun ist basierend auf Bedingungen. Die gängigste Methode dazu ist die `If` Anweisung (und optional `Else` Anweisung).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Die Anweisung `If IsPost` ist eine schnelle Möglichkeit der Verfassung `If IsPost = True`. Zusammen mit `If` Anweisungen, es gibt eine Vielzahl von Möglichkeiten zum Testen von Bedingungen, wiederholen Sie Codeblöcke und usw., die sind weiter unten in diesem Artikel beschrieben.

Das Ergebnis in einem Browser angezeigt (nach dem Klicken auf **senden**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET und POST-Methoden und die IsPost-Eigenschaft**
> 
> Das Protokoll für Webseiten (HTTP) unterstützt eine sehr begrenzte Anzahl von Methoden (&quot;Verben&quot;), mit denen die Anforderungen an den Server zu senden. Die zwei häufigsten Gründe sind GET, die zum Lesen einer Seite verwendet wird, und POST, das verwendet wird, um eine Seite zu übermitteln. Zum ersten Mal, das ein Benutzer eine Seite anfordert, wird im Allgemeinen unter Verwendung von GET eine Seite angefordert. Wenn der Benutzer in einem Formular ausgefüllt, und klickt dann auf **senden**, sendet der Browser eine POST-Anforderung an dem Server.
> 
> In Web-Programmierung ist es oft hilfreich zu wissen, ob eine Seite als GET oder POST angefordert wird, damit Sie wissen, wie auf die Seite zu verarbeiten. In ASP.NET Web Pages, können Sie die `IsPost` Eigenschaft, um festzustellen, ob eine Anforderung eine Get- oder POST ist. Wenn die Anforderung eine POST-ist der `IsPost` Eigenschaft "true" zurück, und Sie können Aktionen wie lesen die Werte von Textfeldern in einem Formular. Viele Beispiele, die Sie sehen, in dem Sie zeigen, wie zum Verarbeiten der Seite hängt der Wert des `IsPost`.


## <a name="a-simple-code-example"></a>Ein einfaches Codebeispiel

Dieses Verfahren zeigt, wie Sie eine Seite erstellen, die die grundlegenden Programmiertechniken veranschaulicht. Im Beispiel erstellen Sie eine Seite, mit dem Benutzer, die zwei Zahlen eingeben, dann fügt sie hinzu, und das Ergebnis wird angezeigt.

1. Klicken Sie in Ihrem Editor, erstellen Sie eine neue Datei, und nennen sie *AddNumbers.vbhtml*.
2. Kopieren Sie den folgenden Code und Markup auf der Seite, und Ersetzen Sie dabei alle Elemente bereits in der Seite an.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Es gibt einige Dinge beachten Sie:

    - Die `@` Zeichen beginnt des erste Codeblocks auf der Seite, und ihm vorausgeht der `totalMessage` Variable im unteren Bereich eingebettet.
    - Der Block am oberen Rand der Seite "steht in `Code...End Code`.
    - Die Variablen `total`, `num1`, `num2`, und `totalMessage` mehrere Zahlen und einer Zeichenfolge zu speichern.
    - Der Zeichenfolgenliteral-Wert, der zugewiesen der `totalMessage` Variable ist in doppelte Anführungszeichen.
    - Da Visual Basic-Code nicht wenn Groß-/Kleinschreibung beachtet, ist die `totalMessage` Variable, die am unteren Rand der Seite verwendet wird, muss der Name nur die Schreibweise der Variablendeklaration am oberen Rand der Seite übereinstimmen. Die Groß-/Kleinschreibung spielt keine Rolle.
    - Der Ausdruck `num1.AsInt()`  +  `num2.AsInt()` für die Arbeit mit Objekten und Methoden veranschaulicht. Die `AsInt` Methode für jede Variable konvertiert die Zeichenfolge, die von einem Benutzer eingegeben wird, auf eine ganze Zahl (Integer), die hinzugefügt werden können.
    - Die `<form>` Tag enthält eine `method="post"` Attribut. Dies gibt an, dass wenn der Benutzer klickt **hinzufügen**, die Seite an den Server mithilfe der HTTP-POST-Methode gesendet. Wenn die Seite gesendet wird, den Code `If IsPost` ergibt "true" und der bedingte code ausgeführt wird, und das Ergebnis der Addition der Zahlen angezeigt.
3. Speichern Sie die Seite, und führen Sie sie in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Geben Sie zwei ganzen Zahlen, und klicken Sie dann auf die **hinzufügen** Schaltfläche.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic-Sprache und Syntax

Zuvor haben Sie ein einfaches Beispiel einer ASP.NET-Webseite zu erstellen, und wie Sie HTML-Markup Servercode hinzufügen können. Hier erfahren Sie die Grundlagen der Verwendung von Visual Basic zum Schreiben von ASP.NET-Code-Server mithilfe der Razor-Syntax &#8212; , also die programming Language-Regeln.

Wenn Sie bereits über Erfahrung mit Programmierung (insbesondere, wenn Sie C#, C++, c#, Visual Basic oder JavaScript verwendet haben), werden viele der hier Sie lesen vertraut sein. Sie müssen wahrscheinlich machen Sie sich nur mit Markup wie WebMatrix-Code hinzugefügt wird *vbhtml* Dateien.

### <a id="BM_CombiningTextMarkupAndCode"></a>  Kombinieren von Text, Markup und Code in Codeblöcken

In Server-Codeblöcken sollten Sie häufig zur Ausgabe von Text und Markup auf der Seite. Wenn ein Server-Codeblock Text enthält, die nicht Code und, stattdessen gerendert werden soll, wie, muss ASP.NET Text von Code zu unterscheiden können. Dafür stehen verschiedene Möglichkeiten zur Verfügung:

- Schließen Sie den Text in einem HTML-Block-Element wie `<p></p>` oder `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn erkennt ASP.NET das öffnende HTML-Tag (z. B. `<p>`), rendert alles, was das Element und dessen Inhalt als für den Browser (und löst die Servercode Ausdrücke) ist.

- Verwenden der `@:` Operator oder die `<text>` Element. Die `@:` Ausgaben eine einzelne Zeile des Inhalts, der nur-Text oder HTML-Tags für nicht übereinstimmenden; enthält die `<text>` Element einschließt, mehrere Zeilen ausgegeben. Diese Optionen sind nützlich, wenn Sie nicht, ein HTML-Element als Teil der Ausgabe gerendert möchten.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Im folgenden Beispiel wird das vorherige Beispiel wiederholt, aber verwendet ein einzelnes Paar von `<text>` Tags aus, um den Text Rendern einschließen.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Im folgenden Beispiel die `<text>` und `</text>` Tags einschließen drei Zeilen, die jeweils einige nicht enthaltene Text und HTML-Tags, die nicht übereinstimmenden sind (`<br />`), sowie der Servercode und HTML-Tags, die übereinstimmende. In diesem Fall können Sie auch jede Zeile einzeln mit voranstellen der `@:` Operator; beide Weise funktionieren.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Wenn Sie Text ausgeben, wie in diesem Abschnitt gezeigt &#8212; mithilfe eines HTML-Elements, die `@:` -Operator, oder die `<text>` Element &#8212; ASP.NET die Ausgabe nicht HTML-Codierung. (Wie bereits erwähnt, ASP.NET codiert die Ausgabe von Codeausdrücken Server und Server-Codeblöcke, die vor `@`, außer in die Sonderfälle, in diesem Abschnitt erwähnt wurden.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer Anweisung (und außerhalb eines Zeichenfolgenliterals) wirken sich nicht auf die Anweisung aus:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Lange Anweisungen Aufteilen in mehrere Zeilen

Sie können eine lange codeanweisung in mehrere Zeilen aufteilen, indem Sie mit dem Unterstrich `_` (die in Visual Basic den Namen der *Fortsetzungszeichen*) nach jeder Zeile des Codes. Um eine Anweisung auf die nächste Zeile unterbrechen, am Ende der Zeile fügen Sie ein Leerzeichen, und klicken Sie dann das Fortsetzungszeichen hinzu. Die Anweisung in der nächsten Zeile fortgesetzt werden. Sie können die Anweisungen auf so viele Zeilen, wie Sie benötigen, um die Lesbarkeit zu verbessern umschließen. Die folgenden Anweisungen sind identisch:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Sie können keine jedoch eine Zeile in der Mitte eines Zeichenfolgenliterals umschließen. Das folgende Beispiel funktioniert nicht:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Um eine lange Zeichenfolge zu kombinieren, die in mehrere Zeilen wie im obigen Code umschließt, müssten Sie verwenden die *Verkettungsoperator* (`&`), die Sie später in diesem Artikel sehen werden.

### <a name="code-comments"></a>Codekommentare

Kommentare können Sie Notizen für sich selbst oder andere zu verlassen. Razor-Syntax Kommentare vorangestellt `@*` und enden mit `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Innerhalb der Codeblöcke können Sie die Kommentare der Razor-Syntax verwenden, oder Sie können normale Visual Basic-Kommentarzeichen, wird ein einfaches Anführungszeichen (`'`) mit dem Präfix für jede Zeile.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variablen

Eine Variable ist ein benanntes Objekt, das Sie verwenden, um Daten zu speichern. Sie können Variablen Namen, aber der Name muss mit einem alphabetischen Zeichen beginnen, und es darf keine Leerzeichen oder reservierte Zeichen enthalten. Wie Sie zuvor gesehen haben, ist die Groß-/Kleinschreibung der Buchstaben im Namen einer Variablen in Visual Basic unerheblich.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable haben einen bestimmten Datentyp, der angibt, welche Art von Daten in der Variablen gespeichert ist. Können Sie Zeichenfolgenvariablen, in denen Zeichenfolgenwerte gespeichert haben (z. B. &quot;Hallo Welt&quot;), Ganzzahl-Variablen, die ganzzahlige Werte (z. B. 3 oder 79) speichern, und die Datenvariablen, die Datumswerte in einer Vielzahl von Formaten (z. B. 4/12/2012 oder März 2009 speichern ). Und es gibt viele andere Datentypen, die Sie verwenden können.

Allerdings müssen Sie einen Typ für eine Variable angeben. In den meisten Fällen kann ASP.NET ermitteln den Typ basierend auf wie die Daten in der Variablen verwendet wird. (Gelegentlich müssen Sie einen Typ angeben, sehen Sie Beispiele, in denen dies "true".)

Um eine Variable zu deklarieren, ohne Angabe eines Typs verwenden `Dim` sowie den Namen der Variablen (z. B. `Dim myVar`). Um eine Variable mit einem Typ zu deklarieren, verwenden `Dim` sowie den Variablennamen ein, gefolgt von `As` , und klicken Sie dann den Namen (z. B. `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Das folgende Beispiel zeigt einige Inlineausdrücke, die die Variablen in einer Webseite verwenden.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konvertieren und Testen von Datentypen

Obwohl ASP.NET einen Datentyp in der Regel automatisch ermitteln kann, manchmal kann es nicht. Aus diesem Grund müssen Sie ASP.NET weiterhelfen, indem Sie eine explizite Konvertierung ausführen. Auch wenn Sie keine Typen zu konvertieren, ist es manchmal hilfreich, testen, welche Art von Daten Sie verwenden können.

Der häufigste Fall ist, dass Sie zum Konvertieren von einer Zeichenfolge in einen anderen Typ, z. B. auf eine ganze Zahl oder Datum. Das folgende Beispiel zeigt einem typischen Fall, in dem Sie eine Zeichenfolge in eine Zahl konvertieren müssen.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Als Faustregel gilt wird der Benutzereingabe für Sie als Zeichenfolgen bereitgestellt. Auch wenn Sie, den Benutzer aufgefordert haben, eine Zahl einzugeben, und auch wenn sie bei der Eingabe des Benutzers gesendet wird, und Sie finden es im Code eine Ziffer eingegeben haben, die Daten im Zeichenfolgenformat werden. Aus diesem Grund müssen Sie die Zeichenfolge in eine Zahl konvertieren. Im Beispiel wenn Sie versuchen, die Durchführung von Berechnungen auf den Werten, ohne zu konvertieren, führt der folgende Fehler, da zwei Zeichenfolgen von ASP.NET hinzugefügt werden können:

`Cannot implicitly convert type 'string' to 'int'.`

Um die Werte in ganzen Zahlen zu konvertieren, rufen Sie die `AsInt` Methode. Wenn die Konvertierung erfolgreich ist, können Sie dann die Zahlen hinzufügen.

Die folgende Tabelle enthält einige allgemeine Konvertierung und Test-Methoden für die Variablen an.


::: Zeile:::::: Spalte::: <strong>Methode</strong> ::: Spalte-End-:::::: Spalte::: <strong>Beschreibung</strong> ::: Spalte-End-:::::: Spalte::: <strong>Beispiel</strong> ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AsInt(), IsInt()` ::: Spalte-End-:::::: Spalte::: konvertiert eine Zeichenfolge, die eine ganze Zahl darstellt (z. B. &quot;593&quot;) in eine ganze Zahl.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AsBool(), IsBool()` ::: Spalte-End-:::::: Spalte::: konvertiert eine Zeichenfolge wie &quot;"true"&quot; oder &quot;"false"&quot; auf einen booleschen Typ.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AsFloat(), IsFloat()` ::: Spalte-End-:::::: Spalte::: konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Gleitkommazahl.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AsDecimal(), IsDecimal()` ::: Spalte-End-:::::: Spalte::: konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Dezimalzahl. (In ASP.NET ist eine Dezimalzahl genauer als eine Gleitkommazahl.) ::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AsDateTime(), IsDateTime()` ::: Spalte-End-:::::: Spalte::: konvertiert eine Zeichenfolge, die einen Wert für Datum und Uhrzeit der ASP.NET darstellt `DateTime` Typ.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `ToString()` ::: Spalte-End-:::::: Spalte::: einen anderen Datentyp in eine Zeichenfolge konvertiert.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    ::: Spalte-End-:::::: Zeilenende:::


## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder das Zeichen, das ASP.NET darüber informiert, welche Art von Befehl aus, um in einem Ausdruck durchgeführt werden. Visual Basic unterstützt viele Operatoren, aber Sie müssen nur wenige zum Entwickeln von ASP.NET Web Pages erkennen. In der folgende Tabelle werden die am häufigsten verwendeten Operatoren zusammengefasst.


::: Zeile:::::: Spalte::: <strong>Operator</strong> ::: Spalte-End-:::::: Spalte::: <strong>Beschreibung</strong> ::: Spalte-End-:::::: Spalte::: <strong>Beispiele</strong> ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `+ - * /` ::: Spalte-End-:::::: Spalte::: mathematische Operatoren, die in numerischen Ausdrücken verwendet.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `=` ::: Spalte-End-:::::: Spalte::: Zuweisungs- und Gleichheit. Je nach Kontext entweder weist den Wert auf der rechten Seite einer Anweisung auf das Objekt auf der linken Seite oder die Werte auf Gleichheit überprüft.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `<>` ::: Spalte-End-:::::: Spalte::: Ungleichheit. Gibt `True` , wenn die Werte nicht gleich sind.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `< > <= >=` ::: Spalte-End-:::::: Spalte::: kleiner, größer als, kleiner als oder gleich und größer oder gleich.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `&` ::: Spalte-End-:::::: Spalte::: Verkettung, die zum Verketten von Zeichenfolgen verwendet wird.
::: Spalte-End-:::::: Spalte::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `+= -=` ::: Spalte-End-:::::: Spalte::: die Inkrement- und Dekrement-Operatoren, die Addition und Subtraktion 1 (bzw.) aus einer Variablen.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `.` ::: Spalte-End-:::::: Spalte::: Punkt. Verwendet, um Objekte und deren Eigenschaften und Methoden zu unterscheiden.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `()` ::: Spalte-End-:::::: Spalte::: Klammern. Um Ausdrücke zu gruppieren, verwendet, zum Übergeben von Parametern zu Methoden und den Zugriff auf Member des Arrays und Auflistungen.
::: Spalte-End-:::::: Spalte::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `Not` ::: Spalte-End-:::::: Spalte::: nicht. Kehrt einen true-Wert auf "false" und umgekehrt. In der Regel als eine schnelle Möglichkeit zum Testen verwendet `False` (d. h. für nicht `True`).
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    ::: Spalte-End-:::::: Zeilenende:::
* * *
::: Zeile:::::: Spalte::: `AndAlso OrElse` ::: Spalte-End-:::::: Spalte::: logische und und zusammen Bedingungen, die verwendet werden, um zu verknüpfen.
::: Spalte-End-:::::: Spalte::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    ::: Spalte-End-:::::: Zeilenende:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und Href-Methode

In einer *.cshtml* oder *vbhtml* -Datei können Sie verweisen, den virtuellen Stamm-Pfad mit der `~` Operator. Dies ist sehr praktisch, da Sie Seiten, an einem Standort navigieren können, und alle Links zu anderen Seiten enthaltenen nicht unterbrochen. Es ist auch praktisch, für den Fall, dass Sie Ihre Website immer an einem anderen Speicherort verschieben. Hier einige Beispiele:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Wenn die Website ist `http://myserver/myapp`, sieht wie ASP.NET diese Pfade behandelt wird, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade nicht tatsächlich als die Werte der Variablen angezeigt, aber ASP.NET die Pfade behandelt, als ob einstellungsänderungen ist.)

Sie können die `~` Operator sowohl im Server-Code (wie oben erläutert) und im Markup wie folgt:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Im Markup, das Sie verwenden die `~` Operator, um Pfade zu den Ressourcen, wie Bilddateien, andere Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, durchsucht Sie die Seite (Code und Markup) und behebt alle ASP.NET die `~` Verweise auf den entsprechenden Pfad.

## <a name="conditional-logic-and-loops"></a>Bedingungen und Schleifen

ASP.NET Server-Code können Sie die Aufgaben, die basierend auf Bedingungen und Schreiben Sie Code, die Anweisungen wiederholen, die also eine bestimmte Anzahl von Malen zu code, der eine Schleife ausgeführt wird).

### <a name="testing-conditions"></a>Testen von Bedingungen

Um eine einfache Bedingung zu testen, Sie verwenden, die `If...Then` -Anweisung, die gibt `True` oder `False` basierend auf einen Test, die Sie angeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Die `If` Schlüsselwort beginnt einen Block. Der eigentliche Test (Bedingung) folgt die `If` -Schlüsselwort und gibt "true" oder "false". Die `If` Anweisung endet mit `Then`. Die Anweisungen, die ausgeführt werden, wenn der Test "true" ist eingeschlossen sind `If` und `End If`. Ein `If` -Anweisung kann enthalten eine `Else` Block, der angibt, Anweisungen, die ausgeführt werden, wenn die Bedingung "false" ist:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Wenn ein `If` Anweisung beginnt einen Codeblock, müssen Sie nicht mit der normalen `Code...End Code` Anweisungen, um die Blöcke einzuschließen. Sie können einfach hinzufügen `@` an den Block, und es funktioniert. Dieser Ansatz funktioniert für `If` sowie andere Visual Basic-Schlüsselwörter, die von Codeblöcken, einschließlich eingehalten werden Programmierung `For`, `For Each`, `Do While`usw.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Sie können mehrere Bedingungen, die mit einem oder mehreren hinzufügen `ElseIf` Blöcken:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

In diesem Beispiel, wenn in der ersten Bedingung die `If` Block ist nicht "true", die `ElseIf` Bedingung aktiviert ist. Wenn diese Bedingung erfüllt ist, die Anweisungen in der `ElseIf` Block ausgeführt werden. Wenn keine der Bedingungen erfüllt sind, die Anweisungen in der `Else` Block ausgeführt werden. Sie können eine beliebige Anzahl von hinzufügen `ElseIf` blockiert, und schließen Sie dann mit einer `Else` als blockiert die &quot;alles&quot; Bedingung.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie eine `Select Case` blockieren:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Der zu testende Wert wird in Klammern angegeben (in dem Beispiel wird die Variable Wochentag). Jeder einzelne Test verwendet einen `Case` -Anweisung, die einen Wert enthält. Wenn der Wert des einem `Case` Anweisung entspricht dem Testwert, der Code in diesem `Case` Block wird ausgeführt.

Das Ergebnis der letzten beiden bedingte Blöcke in einem Browser angezeigt:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Schleifen-code

Sie müssen häufig die gleichen Anweisungen mehrmals ausgeführt werden. Dazu müssen Sie Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Auflistung von Daten. Wenn Sie wissen, wie oft Sie möchten eine Schleife, können Sie eine `For` Schleife. Diese Art von Schleife eignet sich besonders für zugewiesen, oder nach unten zählen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Die Schleife beginnt mit der `For` Schlüsselwort, gefolgt von drei Elementen:

- Unmittelbar nach der `For` -Anweisung, Sie deklarieren eine Zählervariable (müssen Sie nicht mit `Dim`), und klicken Sie dann den Bereich angeben, wie in `i = 10 to 20`. Dies bedeutet, dass die Variable `i` beginnen bei 10 zu zählen und weiterhin bis 20 (einschließlich) erreicht wird.
- Zwischen der `For` und `Next` Anweisungen ist der Inhalt des Blocks. Dies kann eine oder mehrere Code-Anweisungen, die ausgeführt werden mit der foreach-Schleife enthalten.
- Die `Next i` Anweisung wird die Schleife beendet. Es erhöht den Zähler und die nächste Iteration der Schleife beginnt.

Die Zeile des Codes zwischen der `For` und `Next` Zeilen enthält den Code, der für jede Iteration der Schleife ausgeführt wird. Das Markup erstellt einen neuen Absatz (`<p>` Element) jedes Mal, und fügt eine Zeile in die Ausgabe, sodass der Wert i (den Zähler). Wenn Sie diese Seite ausführen, wird im Beispiel 11 Zeilen anzeigen der Ausgabe, mit dem Text in jeder Zeile an, der angibt, der Elementnummer erstellt.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Wenn Sie mit einer Auflistung oder ein Array arbeiten, verwenden Sie häufig eine `For Each` Schleife. Eine Auflistung ist eine Gruppe ähnlicher Objekte und die `For Each` Schleife können Sie eine Aufgabe für jedes Element in der Auflistung durchführen. Dieser Art von Schleife eignet sich für Auflistungen, da im Gegensatz zu einem `For` Schleife, Sie müssen keine Erhöhen des Zählerwerts oder ein Limit festlegen. Stattdessen die `For Each` Schleifen-Code wird einfach in der Auflistung fortgesetzt, bis er abgeschlossen ist.

In diesem Beispiel gibt die Elemente in der `Request.ServerVariables` Sammlung (mit Informationen über den Webserver). Er verwendet eine `For Each` Schleife, um den Namen der einzelnen Elemente anzeigen, indem Sie beim Erstellen eines neuen `<li>` Element in einer HTML-Aufzählung.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Die `For Each` Schlüsselwort folgt durch eine Variable, die ein einzelnes Element in der Auflistung darstellt (beispielsweise `myItem`), gefolgt von der `In` Schlüsselwort, gefolgt von der Auflistung durchlaufen werden sollen. Im Hauptteil der `For Each` Schleife können Sie das aktuelle Element, das mit der Variablen, die Sie zuvor deklarierten zugreifen.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Verwenden Sie zum Erstellen einer allgemeinen Schleife die `Do While` Anweisung:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Diese Schleife beginnt mit der `Do While` Schlüsselwort, gefolgt von einer Bedingung, gefolgt vom Block wiederholt werden soll. Schleifen in der Regel erhöhen (hinzugefügt) oder zu verringern (subtrahiert) eine Variable oder ein Objekt, das für die Inventur verwendet. Im Beispiel die `+=` Operator fügt 1 auf den Wert einer Variable jedes Mal die Schleife ausgeführt wird. (Um eine Variable in einer Schleife zu verringern, die wird nach unten gezählt, verwenden Sie des Dekrementoperators `-=`.)

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Fast alles in einer ASP.NET-Website ist ein Objekt, einschließlich der Webseite selbst. Dieser Abschnitt beschreibt einige wichtige Objekte, die Sie häufig in Ihrem Code verwenden werden.

### <a name="page-objects"></a>Page-Objekte

Die meisten grundlegenden Objekts in ASP.NET ist die Seite. Sie können die Eigenschaften der Page-Objekt, ohne alle berechtigten Objekte direkt zugreifen. Der folgende Code Ruft den Dateipfad der Seite mit den `Request` Objekt der Seite:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Können Sie Eigenschaften der `Page` Objekt um eine Vielzahl von Informationen, wie z. B. zu erhalten:

- `Request` Wie Sie bereits gesehen haben, ist dies eine Auflistung von Informationen über die aktuelle Anforderung, einschließlich welche Art von Browser die Anforderung, die URL der Seite, die Identität des Benutzers usw. vorgenommen.
- `Response` Dies ist eine Auflistung von Informationen über die Antwort (Seite), die an den Browser gesendet wird, wenn der Code ausgeführt wurde. Beispielsweise können Sie diese Eigenschaft, um Informationen in die Antwort zu schreiben.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Von Auflistungsobjekten (Arrays und Dictionarys)

Eine Auflistung ist eine Gruppe von Objekten desselben Typs, z. B. eine Auflistung von `Customer` Objekte aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, z.B. die `Request.Files` Auflistung.

Sie werden häufig in Sammlungen mit Daten arbeiten. Zwei allgemeine Auflistungstypen sind die *Array* und *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Sammlung mit ähnlichen Elementen gespeichert werden soll, aber nicht, erstellen Sie eine separate Variable für jedes Element möchten:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Mit Arrays, Sie deklarieren einen bestimmten Datentyp, z. B. `String`, `Integer`, oder `DateTime`. Um anzugeben, dass die Variable darf ein Array, das Sie Klammern hinzufügen, um den Variablennamen in der Deklaration (z. B. `Dim myVar() As String`). Es stehen die Elemente in einem Array mit ihrer Position (Index) oder mithilfe der `For Each` Anweisung. Arrayindizes sind nullbasiert &#8212; , also das erste Element ist an position 0, das zweite Element ist an Position 1 und So weiter.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Sie können die Anzahl der Elemente in einem Array zu bestimmen, durch Abrufen der `Length` Eigenschaft. Die Position eines bestimmten Elements im Array abzurufen (d. h. das Array zu suchen), verwenden die `Array.IndexOf` Methode. Sie können auch Aktionen wie Reverse den Inhalt eines Arrays (die `Array.Reverse` Methode) oder den Inhalt zu sortieren (die `Array.Sort` Methode).

Die Ausgabe von der Zeichenfolgen-Array-Code in einem Browser angezeigt:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel/Wert-Paare, in dem Sie den Schlüssel (oder Name) festlegen oder Abrufen des entsprechenden Werts angeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Um ein Wörterbuch zu erstellen, verwenden Sie die `New` Schlüsselwort, um anzugeben, dass Sie ein neues erstellen `Dictionary` Objekt. Sie können ein Wörterbuch zuweisen, um eine Variable mit dem `Dim` Schlüsselwort. Sie zeigen die Daten der Elemente in das Wörterbuch, das die Verwendung von Klammern ( `( )` ). Am Ende der Deklaration müssen Sie ein weiteres Paar Klammern hinzufügen, da es sich eigentlich eine Methode handelt, die ein neues Wörterbuch erstellt.

Um Elemente zum Wörterbuch hinzuzufügen, rufen Sie die `Add` Methode der "Dictionary"-Variable (`myScores` in diesem Fall), und geben Sie einen Schlüssel und Wert. Alternativ können Sie Klammern verwenden, um den Schlüssel angeben, und führen eine einfache Zuweisung, wie im folgenden Beispiel:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Um einen Wert aus dem Wörterbuch abzurufen, geben Sie den Schlüssel in Klammern ein:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

In diesem Artikel haben Sie gesehen, verfügen über die Objekte, denen Sie Programmieren mit Methoden aus. Z. B. eine `Database` Objekt möglicherweise eine `Database.Connect` Methode. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, der Sie an eine Methode übergeben, um die Methode zum Abschließen des Tasks zu aktivieren. Betrachten Sie z. B. eine Deklaration für die `Request.MapPath` -Methode, die drei Parameter akzeptiert:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Diese Methode gibt den physischen Pfad auf dem Server, der entspricht einem angegebenen virtuellen Pfad zurück. Die drei Parameter für die Methode sind `virtualPath`, `baseVirtualDir`, und `allowCrossAppMapping`. (Beachten Sie, dass in der Deklaration, die Parameter mit den Datentypen der Daten aufgeführt sind, die eingegeben werden.) Wenn Sie diese Methode aufrufen, müssen Sie Werte für alle drei Parameter angeben.

Wenn Sie Visual Basic mit Razor-Syntax verwenden, haben Sie zwei Möglichkeiten zum Übergeben von Parametern an eine Methode: *Positionsparameter* oder *benannte Parameter*. Um eine Methode mithilfe von positionelle Parameter aufzurufen, übergeben Sie die Parameter in einem strikter Reihenfolge, der in der Deklaration der Methode angegeben ist. (Sie würden diese Reihenfolge in der Regel kennen, lesen Sie die Dokumentation für die Methode.) Führen Sie die Reihenfolge, und Sie können einen der Parameter nicht überspringen &#8212; erforderlich, eine leere Zeichenfolge übergeben (`""`) oder null, ein Positionsparameter, die einen Wert für nicht verfügbar.

Im folgende Beispiel wird angenommen, Sie haben einen Ordner namens *Skripts* auf Ihrer Website. Der Code Ruft die `Request.MapPath` -Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Dann wird den resultierenden zugeordneten Pfad angezeigt.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Wenn viele Parameter für eine Methode vorhanden sind, können Sie Ihren Code übersichtlicher und lesbarer beibehalten, durch die Verwendung benannter Parameter. Um eine Methode, die mit benannten Parametern aufzurufen, geben Sie den Parameternamen, gefolgt von `:=` und geben Sie dann den Wert. Ein Vorteil von benannten Parametern ist, dass Sie sie in beliebiger Reihenfolge hinzufügen können Sie die gewünschten. (Ein Nachteil ist, dass der Methodenaufruf nicht so kompakt ist).

Im folgenden Beispiel wird die gleiche Methode wie oben beschrieben, aber verwendet benannte Parameter, die Werte anzugeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übergeben. Wenn Sie im vorherigen Beispiel und in diesem Beispiel ausführen, werden sie jedoch den gleichen Wert zurück.

## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-Catch-Anweisungen

Sie müssen oft Anweisungen in Ihrem Code, die außerhalb Ihrer Kontrolle Gründen fehlschlagen kann. Zum Beispiel:

- Wenn Ihr Code versucht, öffnen, erstellen, lesen oder Schreiben einer Datei, alle möglichen Fehler auftreten können. Die gewünschte Datei nicht vorhanden sind, wird sie möglicherweise gesperrt, der Code möglicherweise nicht über Berechtigungen verfügen, und so weiter.
- Auf ähnliche Weise Wenn Ihr Code versucht, die Datensätze in einer Datenbank zu aktualisieren, können Berechtigungsprobleme vorhanden sein, kann die Verbindung mit der Datenbank gelöscht werden, die zu speichernden Daten möglicherweise ungültig und so weiter.

Programmiertechnisch ausgedrückt, werden diese Situationen aufgerufen *Ausnahmen*. Wenn Ihr Code eine Ausnahme auftritt, generiert er (auslöst) eine Fehlermeldung angezeigt, d. h., im besten Fall es ziemlich ärgerlich, für Benutzer.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

In Situationen, in denen Ihr Code kann Ausnahmen auftreten, und um Fehler Nachrichten dieses Typs zu vermeiden, können Sie `Try/Catch` Anweisungen. In der `Try` -Anweisung, führen Sie den Code, die Sie überprüfen können. In einer oder mehreren `Catch` -Anweisungen, sehen Sie nach bestimmten Fehlern (bestimmte Arten von Ausnahmen), die aufgetreten sind. Sie können beliebig viele einschließen `Catch` möchte, dass die Anweisungen, wie Sie nach Fehlern zu suchen, die Sie erwarten können.

> [!NOTE]
> Es wird empfohlen, dass Sie, verwenden vermeiden die `Response.Redirect` -Methode in der `Try/Catch` -Anweisungen, da dies eine Ausnahme auf Ihrer Seite führen kann.


Das folgende Beispiel zeigt eine Seite, die auf der ersten Anforderung eine Textdatei erstellt und zeigt dann eine Schaltfläche der Benutzer die Datei zu öffnen. Im Beispiel verwendet absichtlich einen ungültigen Dateinamen ein, damit es eine Ausnahme ausgelöst wird. Der Code enthält `Catch` -Anweisungen für die zwei möglichen Ausnahmen: `FileNotFoundException`, Dies tritt ein, wenn der Dateiname ungültig, ist und `DirectoryNotFoundException`, Dies tritt ein, wenn ASP.NET nicht den Ordner selbst finden können. (Sie können die auskommentierung Aufheben einer Anweisung im Beispiel um zu sehen, wie sie ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn Ihr Code die Ausnahme behandelt hat nicht, sehen Sie eine Fehlerseite angezeigt, wie im vorherigen Screenshot. Allerdings die `Try/Catch` Abschnitt hilft zu verhindern, dass den Benutzer diese Arten von Fehlern angezeigt.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

### <a name="reference-documentation"></a>Referenzdokumentation

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic-Sprache](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
