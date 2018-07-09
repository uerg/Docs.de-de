---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Einführung in ASP.NET Web Pages - Grundlagen der Programmierung | Microsoft-Dokumentation
author: tfitzmac
description: 'In diesem Tutorial enthält eine Übersicht der Programmierung in ASP.NET Web Pages mit Razor-Syntax. Sie lernen Folgendes: die grundlegende Razor-Syntax, mit denen Sie für den Pr...'
ms.author: aspnetcontent
ms.date: 06/17/2015
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 56268943b09d366b15d3a11e641d6c6b6c95aa16
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839748"
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Einführung in ASP.NET Web Pages - Grundlagen der Programmierung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Tutorial enthält eine Übersicht der Programmierung in ASP.NET Web Pages mit Razor-Syntax.
> 
> Sie lernen Folgendes:
> 
> - Die grundlegende Syntax für "Razor", die Sie für die Programmierung in ASP.NET Web Pages verwenden.
> - Einige grundlegende C# -Code, der der Programmiersprache, die Sie verwenden möchten.
> - Einige grundlegende Programmierkonzepte für Webseiten.
> - Vorgehensweise: Installieren von Paketen (Komponenten, die Remotebereitstellung von vorgefertigtem Code enthalten) für die Verwendung mit Ihrer Website.
> - Gewusst wie: Verwenden Sie *Hilfsprogramme* zum Ausführen allgemeiner Programmieraufgaben.
>   
> 
> Features/Technologien erläutert:
> 
> - NuGet und die Paket-Manager.
> - Die `Gravatar` Helper.


Dieses Tutorial ist in erster Linie eine Übung in der Einführung in die Programmierung Syntax, die Sie für ASP.NET Web Pages verwenden. Erfahren Sie, *Razor-Syntax* und Programmiersprache für Code, der in c# geschrieben ist. Sie erhalten einen Überblick über diese Syntax im vorherigen Tutorial haben; In diesem Tutorial erläutern wir, die weitere Syntax.

Wir garantieren, dass es sich bei diesem Tutorial umfasst die meisten Programmiersprachen, dass Sie in einem einzelnen Tutorial sehen und, dass es das einzige Lernprogramm ist, *nur* zur Programmierung. In den restlichen Tutorials in dieser Gruppe erstellen Sie tatsächlich Seiten, die interessante Dinge zu erledigen.

Sie erfahren mehr über *Hilfsprogramme*. Ein Hilfsprogramm ist eine Komponente, ein Paket von Codeabschnitt –, die Sie zu einer Seite hinzufügen können. Das Hilfsprogramm führt die Arbeit für Sie, die andernfalls mühsam oder manuell erfolgen komplex sein können.

## <a name="creating-a-page-to-play-with-razor"></a>Erstellen einer Seite mit Razor spielen

In diesem Abschnitt werden Sie ein wenig mit Razor wiedergeben, um einen Überblick über die grundlegende Syntax zu erhalten.

Starten Sie WebMatrix aus, wenn er nicht bereits ausgeführt wird. Verwenden Sie die Website, die Sie im vorherigen Tutorial erstellt haben ([erste Schritte mit Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578)). Um es erneut öffnen, klicken Sie auf **My Sites** , und wählen Sie **WebPageMovies**:

![WebMatrix-Startseite mit den Optionen für die Website öffnen und persönliche Websites hervorgehoben](intro-to-web-pages-programming/_static/image1.png)

Wählen Sie die **Dateien** Arbeitsbereich.

Klicken Sie im Menüband auf **neu** zum Erstellen einer Seite. Wählen Sie **CSHTML** und nennen Sie die neue Seite *TestRazor.cshtml*.

Klicken Sie auf **OK**.

Kopieren Sie Folgendes in die Datei vollständig zu ersetzen, was bereits vorhanden ist.

> [!NOTE]
> Wenn Sie Codes oder Markups in den Beispielen auf einer Seite kopieren, den Einzug und Ausrichtung der gleiche wie in dem Tutorial möglicherweise nicht. Einzug und Ausrichtung haben keine Auswirkungen auf, wie der Code, jedoch ausgeführt wird.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Analysieren Sie die Beispielseite

Die meisten der Anzeige ist gewöhnliche HTML. Am Anfang ist jedoch diesem Codeblock:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Beachten Sie, dass die folgenden Schritte zu diesem Codeblock:

- Der @-Zeichen weist ASP.NET an, dass hier keine HTML-Razor-Code finden. ASP.NET behandelt alles nach dem @-Zeichen als Code, bis er in HTML-Code erneut ausgeführt wird. (In diesem Fall, der die &lt;! DOCTYPE&gt; Element.
- Die geschweiften Klammern ({und}) schließen Sie einen Block mit Razor-Code aus, wenn der Code mehr als eine Zeile aufweist. Die geschweiften Klammern Teilen ASP.NET, in dem der Code für diesen Block beginnt und endet.
- Die / / Zeichen kennzeichnen ein Kommentars – d. h. ein Teil des Codes, die ausgeführt wird, wird nicht.
- Jede Anweisung muss mit einem Semikolon (;) enden. (Keine Kommentare, obwohl.)
- Sie können Werte in speichern *Variablen*, die Sie erstellen (*deklarieren*) mit dem Schlüsselwort "var". Wenn Sie eine Variable erstellen, Sie geben sie einen Namen, z. Buchstaben, Zahlen und Unterstriche b. (\_). Variablennamen können nicht mit einer Zahl beginnen und darf nicht den Namen des ein Programmiersprachen-Schlüsselwort (z. B. Var) verwenden.
- Schließen Sie Zeichenfolgen (wie "ASP.NET" und "Webseiten"), in Anführungszeichen ein. (sie müssen Anführungszeichen sein.) Zahlen sind nicht in Anführungszeichen ein.
- Leerzeichen außerhalb von Anführungszeichen spielt keine Rolle. Zeilenumbrüche nicht hauptsächlich von Bedeutung sind; die Ausnahme ist, dass eine Zeichenfolge in Anführungszeichen kann nicht über mehrere Zeilen verteilt. Einzug und Ausrichtung nicht von Bedeutung sind.

Etwas, das nicht offensichtlich ist in diesem Beispiel ist ist, dass der gesamte Code Groß-/Kleinschreibung beachtet. Dies bedeutet, dass die Variable Summe eine andere Variable als Variablen, die Summe oder Summe benannt werden kann. Auf ähnliche Weise Var ist ein Schlüsselwort, Var ist jedoch nicht.

### <a name="objects-and-properties-and-methods"></a>Objekte und Eigenschaften und Methoden

Klicken Sie dann ist den Ausdruck "DateTime.Now" vorhanden. Einfach ausgedrückt, "DateTime" ist ein *Objekt*. Ein Objekt ist eine Sache, die mit der Sie programmieren können – eine Seite, ein Textfeld, eine Datei, ein Bild, eine webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz usw. Objekte verfügen über eine oder mehrere *Eigenschaften* , die ihren Merkmalen beschreiben. Ein Textfeld-Objekt hat eine Texteigenschaft (unter anderem), ein Anforderungsobjekt verfügt über eine Url-Eigenschaft (und andere), eine e-Mail-Nachricht eine From-Eigenschaft und eine Eigenschaft an, und so weiter. Objekte verfügen außerdem über *Methoden* , sind die "Verben" ausführen. Sie werden viel mit Objekten arbeiten werden.

Wie Sie aus dem Beispiel sehen können, ist "DateTime" ein Objekt, das Programm Datumsangaben und Uhrzeiten kann. Es verfügt über eine Eigenschaft namens nun, die das aktuelle Datum und die Uhrzeit zurückgibt.

### <a name="using-code-to-render-markup-in-the-page"></a>Verwenden von Code zum Rendern von Markup auf der Seite

Sehen Sie in den Text der Seite Folgendes ein:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

In diesem Fall das @-Zeichen weist ASP.NET folgt, dass keine HTML-Code ist. Im Markup hinzufügbaren, gefolgt von einem Codeausdruck, und ASP.NET wird den Wert des Ausdrucks richtig an dieser Stelle gerendert. Im Beispiel @a wird gerendert, was auch immer der Wert der Variablen, die mit dem Namen ist a, @product gerendert wird, was auch immer in das Variable benannte Produkt, und so weiter ist.

Sie sind nicht auf Variablen, begrenzt jedoch. In einigen Fällen hier das @-Zeichen vorangestellt ist in einem Ausdruck:

- @(eine\*(b) rendert das Produkt der in der Variablen ein und b. (Die \* Operator bedeutet Multiplikation.)
- @(Technologie + "" + Produkt) rendert Sie die Werte in der Variablen Technologie und nach der Verkettung und ein Leerzeichen dazwischen hinzufügen. Der Operator (+) für das Verketten von Zeichenfolgen ist identisch mit dem Operator für das Hinzufügen von Zahlen. ASP.NET können in der Regel feststellen, ob Sie mit Zahlen oder Zeichenfolgen arbeiten, und nehmen das richtige mit dem + -Operator.
- @Request.Url Rendert die Url-Eigenschaft des Anforderungsobjekts. Das Request-Objekt enthält Informationen zur aktuellen Anforderung aus dem Browser, und natürlich die Url-Eigenschaft enthält die URL des die aktuelle Anforderung.

Im Beispiel wurde auch entwickelt, um anzuzeigen, dass Sie, die Sie auf unterschiedliche Weise funktionieren. Sie können Berechnungen im Codeblock am Anfang, fügen Sie die Ergebnisse in eine Variable, und klicken Sie dann die Variable im Markup zu rendern. Oder Sie können Berechnungen in einem Ausdruck rechts im Markup. Der Ansatz, den Sie verwenden, hängt davon ab, auf was Sie tun, und klicken, in gewisser Hinsicht auf Ihre eigenen Einstellungen.

### <a name="seeing-the-code-in-action"></a>Den Code in Aktion sehen

Mit der rechten Maustaste in des Namens der Datei, und wählen Sie dann **in Browser starten**. Die Seite im Browser mit den Werten und Ausdrücken, die aufgelöst werden, auf der Seite angezeigt.

!["TestRazor"-Seite im Browser ausgeführt wird](intro-to-web-pages-programming/_static/image2.png)

Sehen Sie sich die Quelle im Browser.

![Quellcode von "Test Razor" Seite im browser](intro-to-web-pages-programming/_static/image3.png)

Wie Sie von Ihren Erfahrungen im vorherigen Tutorial erwarten, ist keiner der Razor-Code auf der Seite. Alles, was, die Sie sehen, sind die tatsächlichen Werte. Beim Ausführen einer Seite, die Sie tatsächlich eine Anforderung an dem Webserver vornehmen, die in WebMatrix integriert ist. Wenn die Anforderung empfangen wird, wird von ASP.NET aufgelöst wird, die Werte und Ausdrücke und deren Werte in die Seite rendert. Er sendet dann die Seite an den Browser.

> [!TIP] 
> 
> **Razor und c#**
> 
> Bis jetzt haben wir gesagt haben, dass Sie Razor-Syntax verwenden. Das ist richtig, aber es ist nicht die ganze Geschichte. Die tatsächliche Programmiersprache, die Sie verwenden, heißt *c#*. C# wurde in eine vor zehn Jahren von Microsoft erstellt und hat sich mittlerweile zu einer der primären Programmiersprachen zum Erstellen von Windows-apps. Alle Regeln, die Sie zu, eine Variable und das Erstellen von Anweisungen usw. gesehen haben, sind tatsächlich alle Regeln der Sprache c#.
> 
> Razor bezieht sich insbesondere auf die kleine Menge von Konventionen für, wie Sie diesen Code in eine Seite einbetten. Z. B. die Konvention der zum Markieren von Code auf der Seite und Verwenden von @ {} zum Einbetten von eines Codeblocks ist der Razor-Aspekt einer Seite. Hilfsprogramme gelten auch als Teil von Razor. Razor-Syntax wird an mehr Stellen als nur in ASP.NET Web Pages verwendet. (Beispielsweise ist es in ASP.NET MVC-Ansichten sowie verwendet.)
> 
> Wir erwähnen, da Wenn Sie Informationen zu Programmierung ASP.NET Web Pages betrachten, Sie zahlreiche Verweise auf Razor finden. Viele dieser Verweise nicht gelten jedoch, was Sie sind dabei, und aus diesem Grund sind möglicherweise verwirrend. Und tatsächlich viele Ihrer Programmierung Fragen sind wirklich werden zum Arbeiten mit c# oder Arbeiten mit ASP.NET. Also wenn Sie speziell für Informationen zu Razor ansehen, nicht die Antworten möglicherweise, die Sie benötigen.


## <a name="adding-some-conditional-logic"></a>Hinzufügen von bedingter Logik

Eines der herausragenden Features zur Verwendung von Code auf einer Seite ist, dass Sie ändern können, was basierend auf verschiedenen Bedingungen geschieht. In diesem Teil des Lernprogramms werden Sie experimentieren Sie einige Möglichkeiten zum Ändern, was auf der Seite angezeigt wird.

Im Beispiel werden einfach und ein wenig übertrieben, damit wir auf die bedingte Logik konzentrieren können. Die Seite erstellen Sie nun tun dies:

- Zeigen Sie verschiedene Text auf der Seite an, je nachdem, ob er zum ersten Mal wird die Seite wird angezeigt, oder gibt an, ob Sie eine Schaltfläche zum Übermitteln von der Seite geklickt haben. Die der ersten Prüfung werden.
- Zeigt an, nur dann, wenn ein bestimmter Wert in der Abfragezeichenfolge der URL (http://...?show=true) übergeben wird. Dies wird im zweiten bedingte Test sein.

Klicken Sie in WebMatrix erstellen Sie eine Seite, und nennen Sie sie *TestRazorPart2.cshtml*. (Klicken Sie im Menüband auf **neu**, wählen Sie **CSHTML**, nennen Sie die Datei, und klicken Sie dann auf **OK**.)

Ersetzen Sie den Inhalt dieser Seite Folgendes ein:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Der Codeblock oben Initialisiert eine Variable namens "Message" mit etwas Text. In den Text der Seite werden der Inhalt der Nachricht-Variablen angezeigt, in einem &lt;p&gt; Element. Das Markup enthält auch eine &lt;Eingabe&gt; zu erstellenden Elements eine **senden** Schaltfläche.

Führen Sie die Seite, um zu sehen, wie es jetzt funktioniert. Jetzt ist es im Grunde eine statische-Seite, auch dann, wenn Sie die **senden** Schaltfläche.

Wechseln Sie zurück in WebMatrix. Fügen Sie den Codeblock, den folgenden hervorgehobenen Code *nach* die Zeile, die Nachricht initialisiert:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>If {}-Block

Was Sie gerade hinzugefügt haben, wurde eine Bedingung. Im Code wird die Bedingung eine solchen Struktur verfügt:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Die zu testende Bedingung wird in Klammern angegeben. Es muss ein Wert oder ein Ausdruck, der "true" oder "false" zurückgibt. Wenn die Bedingung true ist, führt ASP.NET die Anweisung oder Anweisungen, die innerhalb der geschweiften Klammern sind. (Dies sind die *klicken Sie dann* Teil der *If-Then-* Logik.) Wenn die Bedingung false ist, wird der Codeblock übersprungen.

Hier sind einige Beispiele der Bedingungen, die Sie, in einer If testen können Anweisung:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Sie können Variablen mit Werten oder Ausdrücken testen, indem Sie mit einem <em>logischer Operator</em> oder <em>Vergleichsoperator</em>: gleich (==), größer als (&gt;), kleiner als (&lt;), größer als oder gleich (&gt;=), und kleiner als oder gleich (&lt;=). Die! =-Operator bedeutet ungleich – z. B. wenn (eine! = 0) bedeutet, dass <em>Wenn</em> <em>eine</em><em>ist nicht gleich 0</em>.

> [!NOTE]
> Stellen Sie sicher, dass Sie feststellen, dass der Vergleichsoperator für gleich (==) nicht das gleiche wie = ist. Die = Operator wird verwendet, nur zum Zuweisen von Werten (Var eine = 2). Wenn Sie diese Operatoren sich kombinieren, erhalten Sie entweder einen Fehler, oder Sie unerwartete Ergebnissen erhalten.


Um zu testen, ob der "true" ist, wird die vollständige Syntax If(IsDone == true). Sie können jedoch auch die Verknüpfung if(IsDone). Bei keinen Vergleichsoperator wird wird ASP.NET davon ausgegangen, dass Sie für "true" testen.

Das! Operator selbst bedeutet, dass ein logisches NOT. Z. B. die Bedingung If (! IsPost) bedeutet, dass *ist IsPost nicht "true"*.

Sie können Bedingungen kombinieren, mit der ein logisches AND (&amp; &amp; Operator) oder logisches OR (||-Operator). Z. B. die letzte von der If-Bedingungen in den vorherigen Beispielen bedeutet, dass *Wenn FileProcessingIsDone nicht festgelegt ist, auf "true" und DisplayMessage auf "false" festgelegt ist*.

### <a name="the-else-block"></a>Else-block

Eine letzte Sache darüber, ob Blöcke: eine Falls eine else-Block-Block folgen kann. Ein else-Block ist nützlich ist, müssen Sie anderen Code auszuführen, wenn die Bedingung false ergibt. Hier ist ein einfaches Beispiel:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Einige Beispiele für die sehen in späteren Tutorials dieser Serie Sie einen else-Block nützlich, verwenden ist.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testen, ob die Anforderung übermitteln (Post)

Es gibt mehr, aber vorerst zurück zur im Beispiel, das die Bedingung if(IsPost) {...} hat. IsPost ist tatsächlich eine Eigenschaft der aktuellen Seite. Beim ersten Verwenden der Seite angefordert wird, IsPost gibt false zurück. Jedoch wenn Sie auf eine Schaltfläche klicken, oder senden andernfalls die Seite, d. h., Sie stellen – IsPost "Wahr" zurückgegeben. Daher kann IsPost zu bestimmen, ob Sie mit einer Formularübergabe arbeiten. (Im Hinblick auf HTTP-Verben, wenn die Anforderung ein GET-Vorgang ist IsPost gibt false zurück. Wenn die Anforderung eine POST-Vorgang ist, IsPost gibt true zurück.) In einem späteren Tutorial arbeiten Sie mit Eingabeformulare, in dem dieser Test besonders nützlich ist.

Führen Sie die Seite. Da dies das erste Mal ist werden Ihnen angeforderte Seite, Sie finden Sie unter "Dies ist beim ersten haben Sie die Seite angefordert". Diese Zeichenfolge ist der Wert, dem die Meldung Variable initialisiert. Es ist ein Test if(IsPost), aber die, im Moment "false" zurückgibt, damit der Code in der If-block wird nicht ausgeführt.

Klicken Sie auf die **senden** Schaltfläche. Die Seite wird erneut angefordert. Als vor, die Meldung Variable festgelegt, zu "Dies ist beim ersten..." klicken. Aber dieses Mal die Test-if(IsPost) Gibt true zurück, damit der Code in der If-block ausgeführt wird. Der Code ändert den Wert der Variablen Nachricht auf einen anderen Wert an, was im Markup gerendert wird.

Fügen Sie jetzt eine If-Bedingung in das Markup. Unterhalb der &lt;p&gt; -Element, enthält die **senden** Schaltfläche, fügen Sie das folgende Markup hinzu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Sie fügen Code innerhalb des Markups, müssen Sie für den Einstieg @. Dann gibt es eine, wenn Test ähnelt derjenigen, die Sie zuvor hinzugefügt haben Sie im Codeblock. Innerhalb der geschweiften Klammern jedoch Sie hinzufügen normales HTML – oder zumindest ist es normalen bis erreicht wird @DateTime.Now. Dies eine andere wenig Razor-Code ist, müssen auch hier Sie @ davor hinzufügen.

Der Punkt hierbei ist, dass Sie hinzufügen können, wenn beide Bedingungen der Code am Anfang und das Markup blockieren. Bei Verwendung eine If-Bedingung in den Text der Seite, die Zeilen innerhalb des Blocks Markup oder Code möglich. In diesem Fall und als "true" wird jedes Mal, wenn Sie Markup und Code mischen, Sie verwenden, damit klar, auf ASP.NET ist, in dem der Code @.

Führen Sie die Seite, und klicken Sie auf **senden**. Dieses Mal angezeigt, nicht nur eine andere Meldung Wenn Sie übermitteln ("nachdem Sie gesendet haben...") können Sie jedoch eine neue Nachricht, die das Datum und Uhrzeit enthält.

!["Test Razor-2"-Seite im Browser ausgeführt wird, mit dem Zeitstempel angezeigt, die nach dem Senden](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testen den Wert einer Abfragezeichenfolge

Einem Weitere Test. Dieses Mal fügen Sie eine If-Block, der testet einen Wert mit dem Namen anzeigen, die in der Abfragezeichenfolge übergeben werden kann. (Wie folgt aus: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) ändern Sie die Seite, damit die Nachricht Sie Anzeige wurde haben ("Dies ist beim ersten...", usw.) wird nur angezeigt, wenn der Wert von zeigt "true" ist.

Auf die untere (jedoch innerhalb) der Codeblock am oberen Rand der Seite fügen Sie Folgendes hinzu:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Der vollständige Code Block jetzt aussehen wie im folgenden Beispiel. (Denken Sie daran, dass wenn Sie den Code in Ihre Seite kopieren, der Einzug anders aussehen kann. Aber, von denen nicht beeinflusst, wie der Code ausgeführt wird.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Der neue Code im Block Initialisiert eine Variable namens ShowMessage auf "false". Es wird dann eine, wenn Tests nach einem Wert in der Abfragezeichenfolge gesucht werden soll. Wenn Sie zuerst die Seite anfordern, weist es eine URL wie die folgende:

`http://localhost:43097/TestRazorPart2.cshtml`

Im Code wird ermittelt, ob die URL eine Variablen namens angezeigt, in der Abfragezeichenfolge an, wie diese Version der URL enthält:

`http://localhost:43097/TestRazorPart2.cshtml`? Anzeigen = True

Der Test selbst untersucht die QueryString-Eigenschaft des Objekts ab. Wenn die Abfragezeichenfolge ein Element mit dem Namen anzeigen enthält und das Element auf true festgelegt ist, die bei Block ausgeführt wird und die ShowMessage-Variable auf "true" festgelegt.

Es gibt ein Trick, wie Sie sehen können. Wie der Name sagt, ist die Abfragezeichenfolge einer Zeichenfolge. Allerdings können Sie nur für "true" und "false" testen, wenn der Wert, die, den Sie testen, um einen booleschen (wahr/falsch) Wert ist. Bevor Sie den Wert der Variablen anzeigen in der Abfragezeichenfolge testen können, müssen Sie ihn in einen booleschen Wert konvertieren. Das ist Was bewirkt, dass die AsBool-Methode – es nimmt eine Zeichenfolge als Eingabe und konvertiert ihn in einen booleschen Wert. Wenn die Zeichenfolge "true" ist, konvertiert die AsBool-Methode natürlich diesen Wert auf "true". Wenn der Wert der Zeichenfolge entspricht, gibt AsBool "false" zurück.

> [!TIP] 
> 
> **Datentypen und As()-Methoden**
> 
> Wir haben nur bisher angegeben, dass wenn Sie eine Variable erstellen, verwenden Sie das Schlüsselwort "var". Dies ist jedoch nicht den gesamten Text. Um Werte zu bearbeiten, Addieren von Zahlen, Zeichenfolgen verketten oder Vergleichen von Datumsangaben oder test für "true" / "false" – c# verfügt über eine entsprechende interne Darstellung des Werts standardprogrammiersprachen. C# -Code kann *in der Regel* herauszufinden, was dieser Darstellung werden soll (, also was *Typ* Daten) basierend auf was Sie mit den Werten tun. Und wieder, allerdings es nicht möglich. Wenn dies nicht der Fall ist, mit deren Hilfe indem Sie explizit angeben, wie C#-Darstellung der Daten sollten. Die AsBool-Methode wird das – teilt mit c#, dass ein String-Wert "true" oder "false" als einen booleschen Wert behandelt werden soll. Ähnliche Methoden sind zum Darstellen von Zeichenfolgen als auch andere Typen wie AsInt (Hana als eine ganze Zahl), AsDateTime (Hana als einen Datum/Uhrzeit), AsFloat (Hana als eine Gleitkommazahl) und So weiter. Wenn Sie diese als ()-Methoden verwenden, wenn c# den Zeichenfolgenwert für das Benutzeroberflächenlayout kann nicht angefordert, sehen Sie einen Fehler.


Im Markup der Seite entfernen oder kommentieren Sie dieses Element (er ist folgt auskommentierten):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Rechts, in denen Sie entfernt oder auskommentiert, Text, fügen Sie Folgendes hinzu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Die, wenn ich sage, ist die Variable ShowMessage "true", Rendern einer &lt;p&gt; Element mit dem Wert der Variablen Nachricht.

### <a name="summary-of-your-conditional-logic"></a>Überblick über Ihre bedingter Logik

Falls Sie nicht sicher, vollständig sind von was Sie gerade getan habe, ist hier eine Zusammenfassung an.

- Die Meldung Variable wird initialisiert, in eine Standardzeichenfolge ("Dies ist beim ersten...").
- Wenn die Seitenanforderung das Ergebnis eines Sendevorgangs (Post) ist, wird der Wert der Nachricht zum geändert "nachdem Sie gesendet haben..."
- Die ShowMessage-Variable wird auf "false" initialisiert.
- Wenn die Abfragezeichenfolge enthält? anzeigen = "true", die ShowMessage Variable wird festgelegt auf "true".
- Im Markup, wenn ShowMessage "true" ist ein &lt;p&gt; Element gerendert wird, die den Wert der Nachricht anzeigt. (Wenn ShowMessage auf "false" festgelegt ist, wird nichts an dieser Stelle im Markup gerendert.)
- Im Markup, wenn die Anforderung eine Post wird eine &lt;p&gt; Element gerendert wird, das Datum und Uhrzeit anzeigt.

Führen Sie die Seite. Es gibt keine Meldung auf, da ShowMessage "false" lautet, sodass im Markup der if(showMessage)-Test "false" zurückgibt.

Klicken Sie auf **Senden**. Daraufhin wird das Datum und Uhrzeit, aber noch keine Nachricht.

Klicken Sie in Ihrem Browser wechseln Sie zu das Feld URL ein, und fügen Sie Folgendes am Ende der URL:? anzeigen = "true", und drücken Sie dann die EINGABETASTE.

!["Test Razor-2"-Seite im Browser, die mit einer Abfragezeichenfolge](intro-to-web-pages-programming/_static/image5.png)

Die Seite wird erneut angezeigt. (Da Sie die URL geändert haben, ist dies eine neue Anforderung, keines Sendevorgangs.) Klicken Sie auf **senden** erneut aus. Die Nachricht wird in diesem Fall wird das Datum und die Uhrzeit angezeigt.

!['Test 2 für Razor' Seite nach dem übermitteln, bei eine Abfragezeichenfolge](intro-to-web-pages-programming/_static/image6.png)

In der URL ändern? anzeigen = WAHR? anzeigen = "false", und drücken Sie EINGABETASTE. Senden Sie die Seite erneut. Die Seite ist, an die ersten Schritte — keine Nachricht.

Wie bereits erwähnt, ist die Logik in diesem Beispiel etwas übertrieben. Jedoch, wenn vor sich geht, kommen in vielen der Seiten, und es dauert mindestens eine der Formen haben Sie erfahren hier.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installieren eines Hilfsprogramms (Anzeigen eines Gravatar-Images)

Einige Aufgaben, die Benutzer häufig auf Webseiten ausführen möchten, viel Code erfordern oder zusätzliche Kenntnisse erfordern. Beispiele: Anzeigen eines Diagramms für Daten; Einfügen von Facebook "Like"-Schaltfläche auf einer Seite Senden einer e-Mail aus Ihrer Website; Abschneiden oder zum Skalieren von Bildern; Verwenden von PayPal für Ihre Website. Um diese Arten von Elementen führen zu erleichtern, können Sie verwenden ASP.NET Web Pages *Hilfsprogramme*. Hilfsmethoden sind Komponenten, die Sie für einen Standort installieren und mit denen Sie die typischen Aufgaben mithilfe von nur wenigen Codezeilen Razor-Code ausführen können.

ASP.NET Web Pages verfügt über einige Hilfsprogramme integriert. Allerdings sind viele Hilfsprogramme in Paketen (-add-ins), die bereitgestellt werden, mit dem NuGet-Paket-Manager verfügbar. NuGet können Sie ein Paket zum Installieren auswählen und dann übernimmt es alle Details der Installation.

In diesem Teil des Tutorials installieren Sie eine Hilfsprogramm, mit dem Sie ein Image Gravatar ("Global erkannten Avatar") anzeigen können. Sie erfahren, dass zwei Dinge. Eines ist zum Suchen und installieren eine Hilfsprogramm. Außerdem erfahren, wie eine Hilfsprogramm erleichtert, etwas zu tun, die Sie andernfalls benötigen würden Sie mit der eine Menge von Code, die Sie selbst schreiben müssten.

Sie können Ihre eigenen Gravatar an der Gravatar-Website unter registrieren [ http://www.gravatar.com/ ](http://www.gravatar.com/), jedoch ist es nicht erforderlich, um ein Gravatar-Konto zum Ausführen dieser Teil des Tutorials erstellen.

Klicken Sie in WebMatrix die **NuGet** Schaltfläche.

![Dialogfeld für NuGet-Katalog in WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt. (Nicht alle den Paketen sind Hilfsprogramme, die einige Funktionen hinzufügen WebMatrix selbst, einige zusätzliche Vorlagen sind und so weiter.) Sie erhalten eine Fehlermeldung zur Versionskonflikts. Sie können diese Fehlermeldung ignorieren, indem Sie auf **OK** und mit diesem Tutorial fortfahren.

![Dialogfeld für NuGet-Katalog in WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Geben Sie in das Suchfeld "ASP.NET-helferprogramme" aus. NuGet zeigt die Pakete, die den Suchbegriffen übereinstimmen.

![NuGet-Katalog in WebMatrix Pakete anzeigen](intro-to-web-pages-programming/_static/image9.png)

Die ASP.NET Web Helpers Library enthält Code, um viele gängige Aufgaben, einschließlich der Verwendung von Gravatar-Bilder zu vereinfachen. Wählen Sie die **ASP.NET Web Helpers Library** packen, und klicken Sie dann auf **installieren** auf den Installer zu starten. Wählen Sie **Ja** Wenn gefragt, ob Sie verwenden möchten, installieren Sie das Paket, und akzeptieren Sie die Bedingungen, um die Installation abzuschließen.

Das ist alles. NuGet heruntergeladen und installiert Sie alles, einschließlich zusätzlichen Komponenten, die erforderlich sind (*Abhängigkeiten*).

Wenn Sie eine Hilfsprogramm deinstalliert haben, ist der Prozess sehr ähnlich. Klicken Sie auf die **NuGet** , zeigen Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.

## <a name="using-a-helper-in-a-page"></a>Verwenden eines Hilfsprogramms in einer Seite

Jetzt verwenden Sie das Hilfsprogramm, das Sie soeben installiert haben. Der Prozess zum Hinzufügen einer Hilfsmethode zu einer Seite ist für die meisten Hilfsmethoden ähnlich.

Klicken Sie in WebMatrix erstellen Sie eine Seite, und nennen Sie sie *GravatarTest.cshml*. (Erstellen Sie eine spezielle Seite, um das Hilfsobjekt zu testen, aber Sie Hilfsprogramme in einer beliebigen Seite auf Ihrer Website verwenden können.)

In der &lt;Text&gt; -Element, Hinzufügen einer &lt;Div&gt; Element. In der &lt;Div&gt; -Element, geben Sie Folgendes:

@Gravatar

Das @-Zeichen ist das gleiche Zeichen, die Sie mit denen Sie Razor-Code zu markieren. **Gravatar** ist das Hilfsobjekt, der mit dem Sie arbeiten.

Sobald Sie den Punkt (.) eingeben, zeigt der WebMatrix eine Liste der *Methoden* (Funktionen), dass das Gravatar-Hilfsprogramm zur Verfügung:

![Gravatar-Hilfsprogramm IntelliSense-Dropdown-Liste](intro-to-web-pages-programming/_static/image10.png)

Dieses Feature heißt *IntelliSense*. Damit können Sie den Code durch die Bereitstellung von Kontext entsprechenden Optionen. IntelliSense funktioniert mit HTML, CSS, ASP.NET Code, JavaScript und anderen Sprachen, die in WebMatrix unterstützt werden. Es ist ein weiteres Feature, das Entwickeln von Webseiten in WebMatrix vereinfacht.

Drücken Sie G auf der Tastatur, und Sie sehen, dass es sich bei IntelliSense die "GetHtml"-Methode findet. Drücken Sie TAB. IntelliSense fügt die ausgewählte Methode ("GetHtml") für Sie. Geben Sie eine öffnende Klammer ein, und beachten Sie, dass die schließende Klammer automatisch hinzugefügt wird. Geben Sie Ihre e-Mail-Adresse in Anführungszeichen, die zwischen den beiden Klammern ein. Wenn Sie ein Gravatar-Konto verfügen, wird Ihr Profilfoto zurückgegeben. Wenn Sie nicht über ein Gravatar-Konto verfügen, wird ein Standardbild zurückgegeben. Wenn Sie fertig sind, sieht die Zeile wie folgt aus:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Die Seite wird jetzt in einem Browser anzeigen. Entweder Ihr Profilbild oder das Standardbild wird angezeigt, abhängig davon, ob Sie ein Gravatar-Konto.

![Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Standardbild](intro-to-web-pages-programming/_static/image12.png)

Rufen Sie eine Vorstellung vom ist das Hilfsobjekt für Sie ausführen, zeigen Sie die Quelle der Seite im Browser. Zusammen mit den HTML-Code, die Sie in Ihrer Seite haben, sehen Sie ein Image-Element, das einen Bezeichner enthält. Dies ist Code, der das Hilfsprogramm in der Seite an der Stelle gerendert, in dem Sie mussten @Gravatar.GetHtml. Das Hilfsprogramm hat es sich um die Informationen, die Sie angegeben und den Code generiert, der direkt mit Gravatar kommuniziert werden soll, um das richtige Bild für das angegebene Konto wieder zu erhalten.

Die "GetHtml"-Methode kann auch Sie das Image anpassen, indem Sie andere Parameter bereitstellen. Der folgende Code zeigt, wie Sie anfordern, deren Breite und Höhe von 40 Pixel ein Bild und verwendet einen angegebenen Standardwert-Image mit dem Namen **Wavatar** Wenn das angegebene Konto nicht vorhanden ist.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Dieser Code erzeugt beispielsweise das folgende Ergebnis (das Standardbild variiert nach dem Zufallsprinzip).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Als Nächstes kommen

Um dieses Tutorial kurz zu halten, mussten wir nur einige Grundlagen konzentrieren. Natürlich ist eine *viele* mehr Razor und c#. Erfahren Sie, ob Sie mehr als Sie in diesen Tutorials durcharbeiten. Wenn Sie mehr über die Programmierung Aspekte von Razor und c# jetzt interessieren, können Sie eine ausführlichere Einführung hier lesen: [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890).

Im nächste Tutorial führt Sie in die Arbeit mit einer Datenbank. In diesem Tutorial beginnen Sie zum Erstellen der beispielanwendung, mit dem Sie Ihre bevorzugten Filme auflisten kann.

## <a name="complete-listing-for-testrazor-page"></a>Vollständige Liste für TestRazor-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Vollständige Liste für TestRazorPart2-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Vollständige Liste für GravatarTest-Seite

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter-Hilfsprogramm](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Zurück](getting-started.md)
> [Weiter](displaying-data.md)
