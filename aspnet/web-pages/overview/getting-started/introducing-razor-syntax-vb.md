---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: "Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic) | Microsoft Docs"
author: tfitzmac
description: "In diesem Anhang werden Ihnen einen Überblick über die Programmierung mit ASP.NET Web Pages in Visual Basic mit der Razor-Syntax."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 8f5d223a5944d8adb9fe65c89e87829d18d1c7ee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax (Visual Basic)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel können Sie einen Überblick über die Programmierung mit ASP.NET Web Pages mit Razor-Syntax und Visual Basic. ASP.NET ist die Technologie von Microsoft für die Ausführung von dynamischen Webseiten auf Webservern.
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


Die meisten Beispiele für die Verwendung von ASP.NET Web Pages mit Razor-Syntax verwenden c#. Die Razor-Syntax unterstützt aber auch Visual Basic. Um eine ASP.NET-Webseite in Visual Basic programmieren, erstellen Sie eine Webseite mit einem *vbhtml* Erweiterung, und klicken Sie dann Visual Basic-Code hinzufügen. Dieser Artikel bietet Ihnen einen Überblick über das Arbeiten mit der Sprache Visual Basic und die Syntax zum Erstellen von ASP.NET Web Pages.

> [!NOTE]
> Die Standardvorlagen für die Website für Microsoft WebMatrix (**Bakery**, **Fotogalerie**, und **Starter Site**usw.) in c# und Visual Basic-Versionen verfügbar sind. Sie können die Visual Basic-Vorlagen von als NuGet-Pakete installieren. Websitevorlagen installiert sind, im Stammordner der Website in einem Ordner namens *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Der obere 8 Tipps für die Programmierung

Dieser Abschnitt enthält einige Tipps, die Sie unbedingt benötigen wissen Sie beim Einstieg in ASP.NET Servercode mithilfe der Razor-Syntax schreiben.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Fügen Sie Code, um eine Seite mit dem @-Zeichen

Die `@` Inlineausdrücke Single-Anweisungsblöcken und mit mehreren Anweisungen Blöcke beginnt:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **HTML-Codierung**
> 
> Beim Anzeigen von Inhalt in einer Seite mit den `@` Zeichen, wie in den vorherigen Beispielen ASP.NET HTML-Codierung der Ausgabe. Dies ersetzt reservierte HTML-Zeichen (z. B. `<` und `>` und `&`) mit Codes, mit denen die Zeichen als Zeichen statt als HTML-Tags oder Entitäten interpretiert wird auf einer Webseite angezeigt werden können. Ohne HTML-Codierung, die Ausgabe aus dem Servercode möglicherweise nicht richtig angezeigt und kann eine Seite, um Sicherheitsrisiken machen.
> 
> Wenn Ihr Ziel ist, um HTML-Markup auszugeben, die RFID-Transponder als Markup gerendert wird (z. B. `<p></p>` für einen Absatz oder `<em></em>` Text hervorgehoben), finden Sie im Abschnitt [Kombinieren von Text, Markup und Code in Codeblöcken](#BM_CombiningTextMarkupAndCode) weiter unten in diesem Artikel.
> 
> Erfahren Sie mehr über die HTML-Codierung [arbeiten mit HTML-Formularen in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Sie setzen die Codeblöcke mit Code... End-Code

Ein Codeblock enthält eine oder mehrere codeanweisungen und wird mit den Schlüsselwörtern eingeschlossen `Code` und `End Code`. Platzieren Sie das öffnende `Code` Schlüsselwort unmittelbar nach der `@` Zeichen &#8212; es kann nicht Leerzeichen dazwischen.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Innerhalb eines Blocks am Ende jeder codeanweisung mit Zeilenumbrüchen

In einem Visual Basic-Code-Block beendet jede Anweisung mit einem Zeilenumbruch. (Später in diesem Artikel wird eine Möglichkeit, eine lange codeanweisung in mehrere Zeilen umbrochen werden soll, falls erforderlich angezeigt.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Sie können Variablen verwenden, um Werte zu speichern

Sie können Werte in Speichern einer *Variable*, einschließlich Zeichenfolgen, Zahlen und Datumsangaben. Sie erstellen eine neue Variable mit dem `Dim` Schlüsselwort. Sie können Variablenwerte einfügen, direkt in eine Seite mit `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Sie setzen literale Zeichenfolgenwerte in Anführungszeichen

Ein *Zeichenfolge* ist eine Folge von Zeichen, die als Text behandelt werden. Um eine Zeichenfolge angeben, schließen Sie ihn in doppelte Anführungszeichen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Fügen Sie zum Einbetten von Anführungszeichen in einen Zeichenfolgenwert, zwei doppelte Anführungszeichen ein. Wenn Sie das doppelte Anführungszeichen einmal in der Seitenausgabe angezeigt werden soll, geben Sie ihn als `""` innerhalb der Anführungszeichen-Zeichenfolge ein, und wenn es zweimal angezeigt werden sollen, geben Sie ihn als `""""` innerhalb der Zeichenfolge in Anführungszeichen.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Visual Basic-Code ist nicht in der Groß-/Kleinschreibung unterschieden

Es ist nicht die Sprache Visual Basic Groß-/Kleinschreibung beachtet. Programmierung Schlüsselwörter (z. B. `Dim`, `If`, und `True`) und Variablennamen (z. B. `myString`, oder `subTotal`) können in jedem Fall geschrieben werden.

Die folgenden Codezeilen die Variable einen Wert zuweisen `lastname` mit einem Kleinbuchstaben benennen, und klicken Sie dann den Wert den Variablen auf der Seite mit einem Großbuchstaben Namen ausgeben.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Viele der Codierung umfasst das Arbeiten mit Objekten

Ein Objekt darstellt, die Sie programmieren können, mit etwas &#8212; eine Seite, ein Textfeld, eine Datei, ein Bild, einer webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz (Datenbankzeile) usw. Objekte verfügen über Eigenschaften, die beschreiben, ihre Eigenschaften &#8212; ein Text-Box-Objekt verfügt über eine `Text` Eigenschaft Request-Objekt hat eine `Url` -Eigenschaft, um eine e-Mail-Nachricht wurde ein `From` -Eigenschaft und eine Customer-Objekt hat eine `FirstName` Eigenschaft. Objekte verfügen außerdem über Methoden, die die &quot;Verben&quot; ausführen. Beispiele für ein Objekt "Datei" `Save` -Methode, ein Image-Objekt `Rotate` -Methode, und ein e-Mail-Objekt `Send` Methode.

Arbeiten Sie werden häufig mit der `Request` -Objekt, das Sie die Informationen wie die Werte des Formulars erhalten Felder auf der Seite (Textfelder, usw.), welche Art von Browser, die URL der Seite, die Identität des Benutzers usw. angefordert. In diesem Beispiel wird gezeigt, wie Eigenschaften für den Zugriff auf die `Request` -Objekt und das Aufrufen der `MapPath` Methode der `Request` -Objekt, das Sie den absoluten Pfad der Seite auf dem Server erhalten:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Sie können Code schreiben, mit der Entscheidungen

Ein wichtiges Feature von dynamischen Webseiten ist, dass Sie ermitteln können, was Sie basierend auf Bedingungen. Die gängigste Methode hierfür ist, mit der `If` Anweisung (und optional `Else` Anweisung).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

Die Anweisung `If IsPost` ist eine schnelle Möglichkeit der Verfassung `If IsPost = True`. Zusammen mit `If` Anweisungen, es gibt eine Vielzahl von Methoden zum Testen Bedingungen, wiederholen Sie diesen Schritt Codeblöcke und usw., die beschrieben werden weiter unten in diesem Artikel.

Das Ergebnis in einem Browser angezeigt (nach dem Klicken auf **Absenden**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP-GET und POST-Methoden und die IsPost-Eigenschaft**
> 
> Das Protokoll für Webseiten (HTTP) unterstützt eine beschränkte Anzahl der Methoden (&quot;Verben&quot;), werden verwendet, um die Anforderungen an den Server zu senden. Die zwei gängigsten sind "GET", die zum Lesen einer Seite verwendet wird, und POST, die verwendet wird, um eine Seite zu senden. Im Allgemeinen zum ersten Mal, das ein Benutzer eine Seite anfordert, GET mithilfe die Seite angefordert. Wenn der Benutzer in einem Formular ausfüllt und klickt dann auf **Absenden**, der Browser sendet eine POST-Anforderung an den Server.
> 
> Web-Programmierung ist es oft hilfreich zu wissen, ob eine Seite als GET oder POST-Methode angefordert werden, damit Sie wissen, wie auf die Seite zu verarbeiten. In ASP.NET Web Pages, die Sie verwenden die `IsPost` Eigenschaft, um festzustellen, ob eine Anforderung eine Get- oder POST-Methode ist. Wenn die Anforderung einen Beitrag ist der `IsPost` Eigenschaft gibt "true" zurück, und Sie können z. B. das Lesen der Werte von Textfeldern in einem Formular. Viele Sie sehen Beispiele veranschaulichen das Verarbeiten der Seite unterschiedlich je nach dem Wert des `IsPost`.


## <a name="a-simple-code-example"></a>Ein einfaches Codebeispiel

Dieses Verfahren wird gezeigt, wie eine Seite zu erstellen, die grundlegende Programmiertechniken veranschaulicht. Im Beispiel erstellen Sie eine Seite, mit dem Benutzer, die zwei Zahlen eingeben, und fügt sie hinzu und das Ergebnis wird angezeigt.

1. Klicken Sie im Editor, erstellen Sie eine neue Datei, und nennen sie *AddNumbers.vbhtml*.
2. Kopieren Sie den folgenden Code und Markup in die Seite, und Ersetzen Sie dabei alle Elemente bereits in der Seite an.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Hier sind einige Punkte beachten:

    - Die `@` beginnt die erste Codeblock auf der Seite, und ihm vorausgeht der `totalMessage` Variable eingebettet, das am unteren Rand.
    - Der Block am oberen Rand der Seite "steht in `Code...End Code`.
    - Die Variablen `total`, `num1`, `num2`, und `totalMessage` mehrere Nummern und eine Zeichenfolge zu speichern.
    - Zugewiesene Wert Literalzeichenfolge der `totalMessage` Variable ist in doppelte Anführungszeichen.
    - Da Visual Basic-Code nicht wenn Groß-/Kleinschreibung beachtet, ist die `totalMessage` Variablen am unteren Rand der Seite verwendet wird, muss seinen Namen nur die Schreibweise der Variablendeklaration am oberen Rand der Seite überein. Die Groß-/Kleinschreibung spielt keine Rolle.
    - Der Ausdruck `num1.AsInt()`  +  `num2.AsInt()` wird gezeigt, wie zum Arbeiten mit Objekten und Methoden. Die `AsInt` -Methode für jede Variable konvertiert die Zeichenfolge, die von einem Benutzer eingegeben wird, auf eine ganze Zahl (Integer), die hinzugefügt werden können.
    - Die `<form>` Tag enthält eine `method="post"` Attribut. Dies gibt an, wenn der Benutzer klickt auf **hinzufügen**, wird die Seite an den Server mithilfe der HTTP POST-Methode gesendet werden. Wenn die Seite übermittelt wird, den Code `If IsPost` True ergibt, die bedingte code ausgeführt wird, das Ergebnis der Addition der Zahlen anzeigen.
3. Speichern Sie die Seite, und führen Sie es in einem Browser. (Stellen Sie sicher, dass die Seite ist ausgewählt, der **Dateien** Arbeitsbereich vor der Ausführung.) Geben Sie zwei ganzen Zahlen, und klicken Sie dann auf die **hinzufügen** Schaltfläche.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Visual Basic-Sprache und Syntax

Zuvor haben Sie gesehen ein einfaches Beispiel zum Erstellen einer ASP.NET-Webseite und wie Sie HTML-Markup Servercode hinzufügen können. Hier erfahren Sie die Grundlagen der Verwendung von Visual Basic ASP.NET Servercode mithilfe der Razor-Syntax &#8212;schreiben; d. h. die Programmierung Sprachregeln.

Wenn Sie bereits Erfahrung mit Programmierung (insbesondere, wenn Sie C, C++, c#, Visual Basic oder JavaScript verwendet haben), werden viele der hier Sie lesen vertraut sein. Sie benötigen wahrscheinlich nur mit wie Markup in WebMatrix-Code hinzugefügt wird vertraut *vbhtml* Dateien.

### <a id="BM_CombiningTextMarkupAndCode"></a>Kombinieren von Text, Markup und Code in Codeblöcken

In Server-Codeblöcke sollten Sie häufig zur Ausgabe von Text und Markup auf der Seite. Wenn ein servercodeblock Text enthält, die sich nicht Code und, stattdessen gerendert werden soll, ist, muss ASP.NET Code, in die Text unterscheiden können. Dafür stehen verschiedene Möglichkeiten zur Verfügung:

- Schließen Sie den Text in ein HTML-Block-Element, z. B. `<p></p>` oder `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    Das HTML-Element kann Text, zusätzliche HTML-Elemente und Servercode Ausdrücke enthalten. Wenn ASP.NET sieht das öffnende HTML-Tag (z. B. `<p>`), rendert alles, was das Element und dessen Inhalt als für den Browser (und löst die Servercode Ausdrücke) ist.

- Verwenden der `@:` Operator oder die `<text>` Element. Die `@:` gibt eine einzige Codezeile Inhalt, der nur-Text oder HTML-Tags werden nicht übereinstimmende; enthält die `<text>` Element einschließt, mehrere Zeilen ausgegeben. Diese Optionen eignen sich nicht als Teil der Ausgabe ein HTML-Elements gerendert werden sollen.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    Im folgenden Beispiel wird die im vorherige Beispiel wiederholt allerdings verwendet ein einzelnes Paar von `<text>` Tags aus, um den Text zum Rendern einschließen.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    Im folgenden Beispiel die `<text>` und `</text>` Tags einschließen drei Zeilen, die alle einige nicht enthaltene Text und HTML-Tags, die nicht übereinstimmenden haben (`<br />`), sowie der Servercode und übereinstimmende HTML-Tags. Erneut, können Sie auch jede Zeile einzeln mit voranstellen der `@:` Operator; beide Weise funktionieren.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Wenn Sie Text Ausgabe wie in diesem Abschnitt &#8212;dargestellt; ein HTML-Element mit dem `@:` -Operator, oder die `<text>` -Element &#8212; ASP.NET die Ausgabe nicht HTML-codiert. (Wie bereits erwähnt, wird die Ausgabe der Codeausdrücke Server und Server Codeblöcke, die vorangestellt werden von ASP.NET codiert `@`, außer in die Sonderfälle, in diesem Abschnitt erwähnt wurden.)

### <a name="whitespace"></a>Whitespace

Zusätzliche Leerzeichen in einer Anweisung (und außerhalb von einem Zeichenfolgenliteral) wirken sich nicht auf die Anweisung aus:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Lange Anweisungen in mehrere Zeilen aufgeteilt

Mehrere Zeilen eine lange codeanweisung unterbrechen möchten, verwenden Sie den Unterstrich `_` (in Visual Basic wird aufgerufen, die *Fortsetzungszeichen*) nach jeder Zeile des Codes. Fügen Sie um eine Anweisung auf die nächste Zeile unterbrechen, am Ende der Zeile ein Leerzeichen und dann das Fortsetzungszeichen aus. Fortsetzen Sie die Anweisung bei der nächsten Zeile. Sie können die Anweisungen auf so viele Zeilen wie erforderlich, um die Lesbarkeit zu verbessern umschließen. Die folgenden Anweisungen entsprechen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

Allerdings können keine Sie eine Zeile in der Mitte ein Zeichenfolgenliteral umschließen. Im folgende Beispiel funktioniert nicht:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Eine lange Zeichenfolge kombinieren, die mehrere Zeilen wie der Code oben umbrochen wird, müssen Sie verwenden die *Verkettungsoperator* (`&`), die Sie später in diesem Artikel sehen.

### <a name="code-comments"></a>Codekommentare

Kommentare können Sie die Anmerkungen zu dieser selbst oder von anderen Benutzern zu lassen. Kommentare für Razor-Syntax vorangestellt `@*` und enden mit `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

In Codeblöcken können Sie die Kommentare der Razor-Syntax, oder Sie können normale Visual Basic-Kommentarzeichen, also ein einfaches Anführungszeichen (`'`) als Präfix für jede Zeile.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variablen

Eine Variable ist ein benanntes Objekt, mit denen Sie Daten zu speichern. Variablen beliebig benennen, aber der Name muss mit einem alphanumerischen Zeichen beginnen und darf keine Leerzeichen oder reservierte Zeichen. Zuvor haben Sie gesehen, ist die Groß-/Kleinschreibung der Buchstaben im Namen einer Variablen in Visual Basic unerheblich.

### <a name="variables-and-data-types"></a>Variablen und Datentypen

Eine Variable kann einen bestimmten Datentyp besitzen, der angibt, welche Art von Daten in der Variablen gespeichert ist. Können Sie Zeichenfolgenvariablen, in denen Zeichenfolgenwerte gespeichert haben (z. B. &quot;Hello World&quot;), Ganzzahlvariablen, die ganzzahlige Werte (z. B. 3 oder 79) speichern und Datenvariablen, die Datumswerte in einer Vielzahl von Formaten (z. B. 4/12/2012 oder März 2009 speichern ). Und es gibt viele andere Datentypen, die Sie verwenden können.

Sie haben jedoch keinen Typ für eine Variable angeben. In den meisten Fällen können ASP.NET herausfinden, welche basierend auf wie die Daten in die Variable verwendet wird. (Gelegentlich müssen Sie einen Typ angeben, sehen Sie Beispiele, in denen dies "true".)

Um eine Variable zu deklarieren, ohne Angabe eines Typs verwenden `Dim` plus den Variablennamen ein (z. B. `Dim myVar`). Um eine Variable mit einem Typ zu deklarieren, verwenden `Dim` plus den Variablennamen ein, gefolgt von `As` , und klicken Sie dann den Typnamen (z. B. `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

Das folgende Beispiel zeigt einige Inlineausdrücke, die die Variablen in einer Webseite verwenden.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

Das Ergebnis in einem Browser angezeigt:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Konvertieren und Testen von Datentypen

Obwohl ASP.NET einen Datentyp in der Regel automatisch ermitteln kann, in einigen Fällen kann es nicht. Aus diesem Grund müssen Sie die ASP.NET unterstützen auf, indem Sie eine explizite Konvertierung durchführen. Auch wenn Sie keine Typen konvertiert haben, ist es manchmal hilfreich, testen, um festzustellen, welche Art von Daten Sie arbeiten können.

Der häufigste Fall ist, Sie eine Zeichenfolge in einen anderen Typ wie z. B. in eine ganze Zahl oder ein Datum zu konvertieren müssen. Das folgende Beispiel zeigt einem typischen Fall, in dem eine Zeichenfolge in eine Zahl konvertiert werden muss.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

– In der Regel eine Benutzereingabe werden Sie als Zeichenfolgen. Auch wenn Sie den Benutzer zur Eingabe einer Zahl aufgefordert haben und auch wenn sie eine Ziffer eingegeben haben, wenn Benutzereingaben gesendet wird und Sie im Code lesen, sind die Daten im Zeichenfolgenformat. Aus diesem Grund müssen Sie die Zeichenfolge in eine Zahl konvertieren. Im Beispiel wenn Sie versuchen, die Durchführung von Berechnungen auf den Werten ohne zu konvertieren, führt der folgende Fehler, da ASP.NET die beiden Zeichenfolgen hinzufügen können:

`Cannot implicitly convert type 'string' to 'int'.`

Um die Werte zu einer ganzen Zahl zu konvertieren, rufen Sie die `AsInt` Methode. Wenn die Konvertierung erfolgreich ist, können Sie dann die Zahlen hinzufügen.

Die folgende Tabelle enthält einige allgemeine Konvertierung und Test-Methoden für die Variablen an.

| **Methode** | **Beschreibung** | **Beispiel** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Konvertiert eine Zeichenfolge, die eine ganze Zahl darstellt (z. B. &quot;593&quot;) in eine ganze Zahl. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
| `AsBool(), IsBool()` | Konvertiert eine Zeichenfolge wie &quot;"true"&quot; oder &quot;"false"&quot; zu einem Boolean-Typ. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
| `AsFloat(), IsFloat()` | Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Gleitkommazahl. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
| `AsDecimal(), IsDecimal()` | Konvertiert eine Zeichenfolge, die einen decimal-Wert wie &quot;1.3&quot; oder &quot;7.439&quot; in eine Dezimalzahl. (In ASP.NET ist eine Dezimalzahl genauer als eine Gleitkommazahl.) | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` | Konvertiert eine Zeichenfolge, die einen Wert für Datum und Uhrzeit der ASP.NET darstellt `DateTime` Typ. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
| `ToString()` | Konvertiert von einem beliebigen anderen Datentyp in eine Zeichenfolge. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a>Operatoren

Ein Operator ist ein Schlüsselwort oder ein Zeichen, die ASP.NET, welche Art von Befehl zum Ausführen in einem Ausdruck mitteilt. Visual Basic unterstützt viele Operatoren, jedoch müssen Sie nur wenige für den Einstieg in die Entwicklung von ASP.NET Web Pages zu erkennen. In der folgenden Tabelle werden die am häufigsten verwendeten Operatoren zusammengefasst.

| **Operator** | **Beschreibung** | **Beispiele** |
| --- | --- | --- |
| `+ - * /` | Mathematische Operatoren, die in numerischen Ausdrücken verwendet. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)] |
| `=` | Zuweisung und auf Gleichheit. Je nach Kontext entweder weist den Wert auf der rechten Seite einer Anweisung mit dem Objekt auf der linken Seite oder die Werte hinsichtlich ihrer Gleichheit überprüft. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)] |
| `<>` | Ungleichheit. Gibt `True` , wenn die Werte nicht gleich sind. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)] |
| `< > <= >=` | Kleiner als, größer als, kleiner als oder gleich und größer als oder gleich. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)] |
| `&` | Verkettung verwendet wird, um Zeichenfolgen zu verknüpfen. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
| `+= -=` | Die Inkrement- und Dekrement-Operatoren, die Addition und Subtraktion 1 (bzw.) aus einer Variablen. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)] |
| `.` | Punkt. Verwendet, um Objekte und ihre Eigenschaften und Methoden zu unterscheiden. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)] |
| `()` | Klammern. Verwendet, um gruppenausdrücken zum Übergeben von Parametern zu Methoden und den Zugriff auf Member des Arrays und Sammlungen. | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
| `Not` | Nicht. Kehrt einen true-Wert auf "false" und umgekehrt. In der Regel als eine schnelle Möglichkeit zum Testen verwendet `False` (d. h. für nicht `True`). | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)] |
| `AndAlso OrElse` | Logisches AND und Bedingungen, die verwendet werden, um die Verknüpfung zusammen. | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)] |

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
| Virtueller Pfad | */humanresources/CompanyPolicy.htm* |
| Physischer Pfad | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

Das virtuelle Stammverzeichnis ist /, wie der Stamm von Laufwerk C: Laufwerk ist \. (Virtuelle Ordnerpfaden verwenden immer Schrägstriche). Der virtuelle Pfad eines Ordners verfügt nicht über den gleichen Namen wie der physische Ordner haben; Es kann ein Alias sein. (Auf Produktionsservern, der virtuelle Pfad selten einen genaue physischen Pfad entspricht.)

Bei der Arbeit mit Dateien und Ordnern im Code müssen Sie manchmal verweisen auf den physischen Pfad und in einigen Fällen einen virtuellen Pfad, je nachdem welche Objekte mit dem Sie arbeiten. ASP.NET bietet Ihnen diese Tools zum Arbeiten mit Datei- und Ordnerpfade in Code: der `Server.MapPath` -Methode, und die `~` Operator und `Href` Methode.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Konvertieren von virtuellen und physischen Pfade: die Server.MapPath-Methode

Der `Server.MapPath` Methode konvertiert einen virtuellen Pfad (z. B. */default.cshtml*) in einen absoluten physischen Pfad (z. B. *C:\WebSites\MyWebSiteFolder\default.cshtml*). Mithilfe dieser Methode können Sie jederzeit einen vollständigen physischen Pfad. Ein typisches Beispiel ist beim Lesen oder Schreiben einer Textdatei oder einer Bilddatei auf dem Webserver.

Der absoluten physischen Pfad der Website auf einer hosting-Site-Server in der Regel nicht kennen, damit diese Methode konvertieren des Pfads können Sie kennen, den virtuellen Pfad – auf den entsprechenden Pfad auf dem Server für Sie. Sie den virtuellen Pfad zu einer Datei oder eines Ordners an die Methode übergeben, und gibt den physischen Pfad zurück:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Verweisen auf das virtuelle Stammverzeichnis: der ~-Operator und Href-Methode

In einem *cshtml* oder *vbhtml* -Datei können Sie verweisen, den virtuellen Stamm Pfad mit der `~` Operator. Dies ist sehr nützlich, da Sie Seiten, an einem Standort navigieren können, und alle Links, die sie zu anderen Seiten enthalten nicht unterbrochen. Es ist auch hilfreich, für den Fall, dass Sie Ihre Website jemals an einen anderen Speicherort verschieben. Hier einige Beispiele:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Wenn die Website `http://myserver/myapp`, sieht wie ASP.NET diese Pfade behandelt wird, wenn die Seite ausgeführt wird:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Diese Pfade nicht tatsächlich als die Werte der Variablen angezeigt, aber ASP.NET Pfade behandelt, als wäre, was waren ist.)

Sie können die `~` Operator im Servercode (wie oben) und im Markup, wie folgt:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Verwenden Sie im Markup der `~` Operator, um Pfade zu Ressourcen, wie Bilddateien, andere Webseiten und CSS-Dateien zu erstellen. Wenn die Seite ausgeführt wird, über die Seite "" (Code und Markup) sucht und behebt alle ASP.NET die `~` Verweise auf den entsprechenden Pfad.

## <a name="conditional-logic-and-loops"></a>Bedingter Logik und Schleifen

Servercode ASP.NET ermöglicht Ihnen die Durchführung von Aufgaben basierend auf Bedingungen und Schreiben Sie Code, durch die Anweisung wiederholt, die eine bestimmte Anzahl von Malen, also code, der eine Schleife ausgeführt wird).

### <a name="testing-conditions"></a>Testen von Bedingungen

Um eine einfache Bedingung zu testen, Sie verwenden, die `If...Then` Anweisung, die zurückkehrt `True` oder `False` anhand eines Tests, die Sie angeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

Die `If` Schlüsselwort beginnt einen Block. Der eigentliche Test (Bedingung) folgt die `If` -Schlüsselwort und gibt "true" oder "false". Die `If` Anweisung endet mit `Then`. Die Anweisungen, die ausgeführt werden, wenn der Test "true" ist durch eingeschlossen sind `If` und `End If`. Ein `If` Anweisung zählen eine `Else` Datenblock, Anweisungen, die ausgeführt werden, wenn die Bedingung "false" ist:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Wenn ein `If` Anweisung beginnt einen Codeblock, müssen Sie keine verwenden die normalen `Code...End Code` Anweisungen, um die Blöcke einzuschließen. Sie können einfach hinzufügen `@` an den Block, und er funktioniert. Dieser Ansatz funktioniert mit `If` sowie andere Visual Basic-Schlüsselwörter, deren Codeblöcke, einschließlich folgen Programmierung `For`, `For Each`, `Do While`usw.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Sie können mehrere Bedingungen, die mit einem oder mehreren hinzufügen `ElseIf` blockiert:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

In diesem Beispiel, wenn das erste Problem, das in der `If` Block ist nicht "true", die `ElseIf` Bedingung aktiviert ist. Wenn diese Bedingung erfüllt ist, die Anweisungen in der `ElseIf` Block ausgeführt werden. Wenn keine der Bedingungen erfüllt sind, die Anweisungen in der `Else` Block ausgeführt werden. Sie können eine beliebige Anzahl von hinzufügen `ElseIf` blockiert, und schließen Sie dann mit einer `Else` Blockieren der &quot;alles andere&quot; Bedingung.

Um eine große Anzahl von Bedingungen zu testen, verwenden Sie eine `Select Case` blockieren:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

Der zu testende Wert steht in Klammern (in dem Beispiel wird die Variable Wochentag). Jeder einzelne Test verwendet einen `Case` -Anweisung, die einen Wert enthält. Wenn der Wert des einem `Case` Anweisung entspricht dem Testwert, den Code in diesem `Case` Block ausgeführt wird.

Das Ergebnis der letzten beiden bedingte Blöcke in einem Browser angezeigt:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Durchlaufen von code

Sie müssen häufig die gleichen Anweisungen wiederholt ausführen. Dazu müssen Sie Schleifen. Beispielsweise führen Sie häufig die gleichen Anweisungen für jedes Element in einer Auflistung von Daten. Wenn Sie wissen, genau wie oft Sie in einer Schleife möchten, können Sie eine `For` Schleife. Diese Art von Schleife ist besonders hilfreich für das Zählen von oben oder nach unten zählen:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

Die Schleife beginnt mit der `For` Schlüsselwort, gefolgt von drei Elementen:

- Unmittelbar nach der `For` -Anweisung, deklarieren Sie eine Zählervariable (Sie müssen nicht verwenden `Dim`) und geben Sie den Bereich an, wie in `i = 10 to 20`. Dies bedeutet, dass die Variable `i` Zähler bei 10 beginnt und weiterhin bis 20 (inklusiv) erreicht.
- Zwischen der `For` und `Next` Anweisungen befindet sich der Inhalt des Blocks. Dies kann eine oder mehrere Code-Anweisungen mit jeder Schleife enthalten.
- Die `Next i` Anweisung wird die Schleife beendet. Der Zähler wird inkrementiert, und startet die nächste Iteration der Schleife.

Die Zeile des Codes zwischen den `For` und `Next` Zeilen enthält den Code, der für jede Iteration der Schleife ausgeführt wird. Das Markup erstellt einen neuen Absatz (`<p>` Element) jedes Mal und fügt eine Zeile an die Ausgabe des Werts eines i (Leistungsindikator). Wenn Sie auf dieser Seite ausführen, erstellt das Beispiel 11 Zeilen, die die Ausgabe mit dem Text in jeder Zeile an, mit dem Element angezeigt.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Wenn Sie eine Auflistung oder ein Array verwenden, verwenden Sie häufig eine `For Each` Schleife. Eine Auflistung ist eine Gruppe von ähnliche Objekte, und die `For Each` Schleife können, Sie aus einer Aufgabe für jedes Element in der Auflistung führen. Diese Art der Schleife eignet sich für Auflistungen, da im Gegensatz zu einer `For` Schleifen, Sie müssen keine Erhöhen des Zählerwerts und einzuschränken. Stattdessen die `For Each` Schleife Code wird einfach in der Auflistung fortgesetzt, bis er abgeschlossen ist.

In diesem Beispiel gibt die Elemente in der `Request.ServerVariables` Auflistung (enthält Informationen zu Ihrem Webserver). Er verwendet ein `For Each` Schleife, um den Namen der einzelnen Elemente anzeigen, indem Sie beim Erstellen eines neuen `<li>` Element in einem HTML-Aufzählung.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

Die `For Each` Schlüsselwort folgt durch eine Variable, die ein einzelnes Element in der Auflistung darstellt (im Beispiel `myItem`), gefolgt von den `In` Schlüsselwort, gefolgt von der Auflistung durchlaufen werden sollen. Im Hauptteil der `For Each` Schleife, können Sie das aktuelle Element über die Variable, die Sie zuvor deklarierte zugreifen.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Verwenden Sie zum Erstellen einer allgemeinen Schleife die `Do While` Anweisung:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Diese Schleife beginnt mit der `Do While` Schlüsselwort, gefolgt von einer Bedingung, gefolgt von den Block zu wiederholen. Schleifen in der Regel erhöhen (hinzufügen) oder dekrementiert (subtrahiert) eine Variable oder ein Objekt, das für die Inventur verwendet. Im Beispiel die `+=` Operator fügt 1 auf den Wert einer Variablen bei jeder Ausführung die Schleife. (Um eine Variable in einer Schleife zu verringern, die nach-unten zählt, verwenden Sie die Dekrement-Operators `-=`.)

## <a name="objects-and-collections"></a>Objekte und Auflistungen

Nahezu alles in einer ASP.NET-Website handelt es sich um ein Objekt, einschließlich der Webseite selbst. Dieser Abschnitt beschreibt einige wichtige Objekte, die Sie häufig in Ihrem Code arbeiten mit.

### <a name="page-objects"></a>Seite "-Objekte

Das grundlegendste Objekt in ASP.NET ist die Seite. Sie können die Eigenschaften der Page-Objekt direkt ohne alle qualifizierenden Objekt zugreifen. Der folgende Code Ruft den Dateipfad mit der Seite mit den `Request` Objekt von der Seite ":

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Können Sie Eigenschaften der `Page` Objekt, eine Vielzahl von Informationen, wie z. B. abgerufen werden:

- `Request` Wie Sie bereits gesehen haben, ist dies eine Auflistung von Informationen über die aktuelle Anforderung, z. B., welche Art von Browser, die URL der Seite, die Identität des Benutzers usw. angefordert.
- `Response` Dies ist eine Auflistung von Informationen über die Antwort (Seite), die an den Browser gesendet wird, wenn der Code ausgeführt wurde. Beispielsweise können Sie diese Eigenschaft verwenden, um Informationen in die Antwort zu schreiben.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Auflistungsobjekte (Arrays und Wörterbücher)

Eine Auflistung ist eine Gruppe von Objekten desselben Typs, z. B. eine Auflistung von `Customer` Objekte aus einer Datenbank. ASP.NET enthält viele integrierte Auflistungen, wie z. B. die `Request.Files` Auflistung.

Sie werden häufig in Auflistungen mit Daten arbeiten. Zwei allgemeine Auflistungstypen sind die *Array* und *Wörterbuch*. Ein Array ist nützlich, wenn Sie eine Auflistung mit ähnlichen Elementen gespeichert werden soll, aber nicht, erstellen Sie eine separate Variable, um jedes Element möchten:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Bei Arrays, Sie deklarieren einen bestimmten Datentyp z. B. `String`, `Integer`, oder `DateTime`. Um anzugeben, dass die Variable kann ein Array enthalten, Sie fügen Sie dem Variablennamen in der Deklaration Klammern (z. B. `Dim myVar() As String`). Sie erreichen die Elemente in einem Array mit ihrer Position (Index) oder mithilfe der `For Each` Anweisung. Array Indizes sind nullbasiert &#8212; Das heißt, das erste Element ist an position 0, das zweite Element ist an Position 1 und So weiter.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Sie können die Anzahl der Elemente in einem Array zu bestimmen, durch Abrufen der `Length` Eigenschaft. Die Position eines bestimmten Elements im Array abrufen (d. h., das Array gesucht werden soll), verwenden die `Array.IndexOf` Methode. Sie können auch Aktionen wie Reverse den Inhalt eines Arrays (die `Array.Reverse` Methode) oder Sortieren des Inhalts (die `Array.Sort` Methode).

Die Ausgabe von der String-Array-Code in einem Browser angezeigt:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Ein Wörterbuch ist eine Auflistung von Schlüssel/Wert-Paaren, in dem Sie den Schlüssel (oder den Namen) festlegen oder Abrufen des entsprechenden Werts eingeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Um ein Wörterbuch zu erstellen, verwenden Sie die `New` Schlüsselwort, um anzugeben, dass Sie ein neues erstellen `Dictionary` Objekt. Sie können ein Wörterbuch zuweisen, um eine Variable mit dem `Dim` Schlüsselwort. Sie geben die Datentypen der Elemente im Wörterbuch mithilfe von Klammern ( `( )` ). Am Ende der Deklaration müssen Sie einen anderen Klammernpaar, hinzufügen, da es sich tatsächlich um eine Methode handelt, die ein neues Wörterbuch erstellt.

Um Elemente zum Wörterbuch hinzuzufügen, rufen Sie die `Add` Methode der Variablen Wörterbuch (`myScores` in diesem Fall), und klicken Sie dann einen Schlüssel und einen Wert angeben. Alternativ können Sie Klammern verwenden, um den Schlüssel an, und führen Sie eine einfache Zuweisung, wie im folgenden Beispiel:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Um einen Wert aus dem Wörterbuch zu erhalten, geben Sie den Schlüssel in Klammern:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Aufrufen von Methoden mit Parametern

Wie Sie weiter oben in diesem Artikel gesehen haben, verfügen über die Objekte, denen Sie Programmieren mit Methoden aus. Z. B. eine `Database` Objekt möglicherweise ein `Database.Connect` Methode. Viele Methoden verfügen auch über einen oder mehrere Parameter. Ein *Parameter* ist ein Wert, den Sie an eine Methode übergeben, die Methode zum Abschließen des Tasks zu aktivieren. Betrachten Sie z. B. eine Deklaration für die `Request.MapPath` Methode, die drei Parameter akzeptiert:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Diese Methode gibt den physischen Pfad auf dem Server, der entspricht einem angegebenen virtuellen Pfad zurück. Die drei Parameter für die Methode sind `virtualPath`, `baseVirtualDir`, und `allowCrossAppMapping`. (Beachten Sie, dass in der Deklaration der Parameter mit den Datentypen der Daten aufgeführt sind, die sie akzeptieren.) Wenn Sie diese Methode aufrufen, müssen Sie Werte für alle drei Parameter angeben.

Wenn Sie Visual Basic mit Razor-Syntax verwenden, stehen Ihnen zwei Optionen zum Übergeben von Parametern an eine Methode: *Positionsparameter* oder *benannte Parameter*. Zum Aufrufen einer Methode mit positionelle Parameter übergeben Sie die Parameter in einer festen Reihenfolge, die in der Deklaration der Methode angegeben ist. (Sie würden diese Reihenfolge in der Regel wissen, lesen Sie die Dokumentation für die Methode.) Sie müssen die Reihenfolge einhalten, und Sie können nicht fortfahren, einen der Parameter &#8212; Wenn es sich bei Bedarf können Sie eine leere Zeichenfolge übergeben (`""`) oder null für einen positionelle Parameter, die Sie für einen Wert besitzen.

Im folgende Beispiel wird angenommen, Sie haben einen Ordner namens *Skripts* auf Ihrer Website. Der Code Ruft die `Request.MapPath` -Methode auf und übergibt Werte für die drei Parameter in der richtigen Reihenfolge. Anschließend wird den resultierenden zugeordneten Pfad angezeigt.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Wenn viele Parameter für eine Methode vorhanden sind, können Sie Ihren Code übersichtlichere und besser lesbar behalten, mithilfe von benannten Parametern. Zum Aufrufen einer Methode, die benannte Parameter verwenden, geben Sie den Parameternamen, gefolgt von `:=` und geben Sie den Wert. Ein Vorteil der benannten Parameter ist, dass Sie sie in beliebiger Reihenfolge hinzufügen können, Sie möchten. (Ein Nachteil ist, dass der Methodenaufruf nicht als compact ist).

Im folgenden Beispiel wird die gleiche Methode wie oben beschrieben, aber verwendet benannte Parameter, um die Werte anzugeben:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Wie Sie sehen können, werden die Parameter in einer anderen Reihenfolge übergeben. Wenn Sie im vorherigen Beispiel und in diesem Beispiel ausführen, müssen sie den gleichen Wert zurückgeben.

## <a name="handling-errors"></a>Behandeln von Fehlern

### <a name="try-catch-statements"></a>Try-Catch-Anweisungen

Sie müssen häufig Anweisungen in Ihrem Code, die außerhalb Ihrer Kontrolle Gründen fehlschlagen. Zum Beispiel:

- Wenn Ihr Code versucht zu öffnen, erstellen, lesen oder Schreiben einer Datei alle Arten von Fehlern auftreten können. Die gewünschte Datei ist möglicherweise nicht vorhanden, wird sie möglicherweise gesperrt, der Code möglicherweise nicht über Berechtigungen verfügen, und so weiter.
- Auf ähnliche Weise, wenn Ihr Code versucht, zum Aktualisieren von Datensätzen in einer Datenbank, treten möglicherweise Berechtigungsprobleme, kann die Verbindung mit der Datenbank gelöscht werden, die zu speichernden Daten möglicherweise ungültige usw.

Programmiertechnisch gesehen Situationen heißen *Ausnahmen*. Wenn im Code eine Ausnahme auftritt, generiert er (auslöst) eine Fehlermeldung angezeigt, d. h., bestenfalls störende für Benutzer.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

In Situationen, in denen Code kann Ausnahmen auftreten, und um Fehlermeldungen dieses Typs zu vermeiden, können Sie `Try/Catch` Anweisungen. In der `Try` -Anweisung, führen Sie den Code, der Sie eingecheckt haben. Eine oder mehrere `Catch` -Anweisungen, die Sie suchen können, für bestimmte Fehler (bestimmte Arten von Ausnahmen), die möglicherweise aufgetreten sind. Sie können beliebig viele einschließen `Catch` Anweisungen, als Sie benötigen nach Fehlern suchen, die Sie vorhersehen können.

> [!NOTE]
> Es wird empfohlen, dass Sie, verwenden vermeiden die `Response.Redirect` Methode in `Try/Catch` Anweisungen, da es einen Ausnahmefehler, auf der Seite verursachen kann.


Das folgende Beispiel zeigt eine Seite, die bei der ersten Anforderung eine Textdatei erstellt und zeigt dann eine Schaltfläche, die dem Benutzer die Datei öffnen kann. Im Beispiel verwendet absichtlich einen falscher Dateiname, sodass er eine Ausnahme auslöst. Der Code enthält `Catch` -Anweisungen für die zwei möglichen Ausnahmen: `FileNotFoundException`, Dies tritt ein, wenn der Dateiname beschädigt ist und `DirectoryNotFoundException`, Dies tritt ein, wenn ASP.NET den Ordner selbst finden kann. (Sie können kommentieren Sie eine Anweisung im Beispiel um anzuzeigen, wie er ausgeführt wird, wenn alles ordnungsgemäß funktioniert.)

Wenn Ihr Code die Ausnahme behandelt hat nicht, sehen Sie eine Fehlerseite, wie der vorherige Screenshot. Allerdings die `Try/Catch` Abschnitt wird verhindert, dass den Benutzer sehen diese Arten von Fehlern.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

### <a name="reference-documentation"></a>Referenzdokumentation

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Visual Basic-Sprache](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
