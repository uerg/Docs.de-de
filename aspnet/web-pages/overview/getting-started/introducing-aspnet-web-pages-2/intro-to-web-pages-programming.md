---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
title: Einführung in ASP.NET Web Pages - Programmierung Grundlagen | Microsoft Docs
author: tfitzmac
description: 'Dieses Lernprogramm bietet Ihnen einen Überblick über das Programm in ASP.NET Web Pages mit Razor-Syntax. Lernen Sie: die grundlegende Razor-Syntax, die für Pr verwendet...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/17/2015
ms.topic: article
ms.assetid: 7526ed45-a97d-4e8a-8301-01324ef0eff9
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/intro-to-web-pages-programming
msc.type: authoredcontent
ms.openlocfilehash: 60115dd06a27bf856427953de29e993194afb991
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2018
---
<a name="introducing-aspnet-web-pages---programming-basics"></a>Einführung in ASP.NET Web Pages - Grundlagen der Programmierung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieses Lernprogramm bietet Ihnen einen Überblick über das Programm in ASP.NET Web Pages mit Razor-Syntax.
> 
> Lernen Sie:
> 
> - Die grundlegende Syntax für "Razor", die Sie für die Programmierung in ASP.NET Web Pages verwenden.
> - Einige einfache C#-, also der Programmiersprache ab, die verwendet werden.
> - Einige grundlegende Programmierkonzepte für Webseiten.
> - Zum Installieren von Paketen (Komponenten, die bereits erstellte Code enthalten), um für Ihre Website verwenden.
> - Gewusst wie: Verwenden Sie *Hilfsprogramme* zum Ausführen allgemeiner Programmieraufgaben.
>   
> 
> Features/Technologien erläutert:
> 
> - NuGet- und die Paket-Manager.
> - Die `Gravatar` Helper.


Dieses Lernprogramm ist in erster Linie eine Übung im einführen Sie in der Programmiersyntax, die Sie für ASP.NET Web Pages verwenden. Sie erfahren *Razor-Syntax* und Programmiersprache für Code, der in c# geschrieben ist. Sie erhalten einen Überblick über diese Syntax im vorherigen Lernprogramm haben; In diesem Lernprogramm erläutern wir die weitere Syntax.

Wir versprechen, dass dieses Lernprogramm die häufigsten Programmieren umfasst, dass Sie in einem einzelnen Lernprogramm sehen und, dass es das einzige Lernprogramm ist *nur* zur Programmierung. In den verbleibenden Lernprogramme in diesem Satz erstellen Sie tatsächlich Seiten, die interessante Dinge tun.

Sie erfahren auch über die *Hilfsprogramme*. Ein Hilfsprogramm ist eine Komponente, ein App-Paket von Programmcode –, die Sie zu einer Seite hinzufügen können. Das Hilfsprogramm führt die Arbeit für Sie, die sonst als zu aufwendig oder komplex, um manuell erfolgen kann.

## <a name="creating-a-page-to-play-with-razor"></a>Erstellen eine Seite mit Razor wiedergeben

In diesem Abschnitt müssen Sie ein wenig mit Razor wiedergeben, sodass Sie einen Eindruck von die grundlegende Syntax abrufen können.

Starten Sie WebMatrix, wenn er nicht bereits ausgeführt wird. Verwenden Sie die Website, die Sie im vorherigen Lernprogramm erstellt haben ([erste Schritte mit Web Pages](https://go.microsoft.com/fwlink/?LinkId=251578)). Um es erneut zu öffnen, klicken Sie auf **Meine Standorte** , und wählen Sie **WebPageMovies**:

![WebMatrix-Startseite öffnen Site-Optionen und Meine Standorte hervorgehoben angezeigt](intro-to-web-pages-programming/_static/image1.png)

Wählen Sie die **Dateien** Arbeitsbereich.

Klicken Sie im Menüband auf **neu** zum Erstellen eines Seitenblob. Wählen Sie **CSHTML** und nennen Sie die neue Seite *TestRazor.cshtml*.

Klicken Sie auf **OK**.

Kopieren Sie Folgendes in die Datei vollständig zu ersetzen, was bereits vorhanden ist.

> [!NOTE]
> Wenn Sie Code oder Markup aus den Beispielen in eine Seite kopieren, den Einzug und die Ausrichtung im Lernprogramm identisch möglicherweise nicht. Einzug und Ausrichtung beeinflussen nicht, wie der Code, obwohl ausgeführt wird.


[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample1.cshtml)]

## <a name="examining-the-example-page"></a>Untersuchen die Seite "Beispiel"

Sehen Sie die meisten ist normale HTML. Oben ist jedoch in diesem Codeblock:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample2.cshtml)]

Beachten Sie Folgendes zum in diesem Codeblock:

- Das @-Zeichen derzufolge ASP.NET Razor-Code, nicht HTML folgt. ASP.NET behandelt alles nach dem @-Zeichen als Code, bis er erneut in HTML-Code ausgeführt wird. (In diesem Fall, die die &lt;! DOCTYPE&gt; Element.
- Die geschweiften Klammern ({und}) einen Block von Razor-Code einschließen, wenn der Code mehr als eine Zeile verfügt. Die geschweiften Klammern Teilen ASP.NET, in dem der Code für den Block beginnt und endet.
- Der / / Zeichen markieren Sie einen Kommentar – d. h. ein Teil des Codes, die ausgeführt wird, wird nicht.
- Jede Anweisung muss mit einem Semikolon (;) enden. (Keine Kommentare, obwohl.)
- Sie können Werte in speichern *Variablen*, die Sie erstellen (*deklarieren*) mit dem Schlüsselwort "var". Wenn Sie eine Variable zu erstellen, Sie geben Sie ihm einen Namen, z. Buchstaben, Zahlen und Unterstriche b. (\_). Variablennamen können nicht mit einer Ziffer beginnen und darf nicht den Namen des ein Programmiersprachen-Schlüsselwort (z. B. Var) verwenden.
- Schließen Sie Zeichenfolgen (z. B. "ASP.NET" und "Web Pages"), in Anführungszeichen ein. (Es müssen doppelte Anführungszeichen eingeschlossen sein.) Zahlen sind nicht in Anführungszeichen ein.
- Außerhalb der Anführungszeichen ein Leerzeichen spielt keine Rolle. Zeilenumbrüche nicht größtenteils von Bedeutung sind; die Ausnahme wird mit eine Zeichenfolge in Anführungszeichen nicht über mehrere Zeilen aufgeteilt werden können. Einzug und die Ausrichtung Bedeutung keine.

Etwas, das nicht von diesem Beispiel offensichtlich ist, ist, dass der gesamte Code Groß-/Kleinschreibung beachtet wird. Dies bedeutet, dass die Summe der Variable-Parameter eine andere Variable als Variablen, die die Summe-Parameter Summe-Parameter benannte Typen handeln kann. Auf ähnliche Weise Var ist ein Schlüsselwort, aber Var ist nicht.

### <a name="objects-and-properties-and-methods"></a>Objekte und Eigenschaften und Methoden

Klicken Sie dann ist den Ausdruck DateTime.Now vorhanden. Einfach ausgedrückt, "DateTime" ist ein *Objekt*. Ein Objekt ist eine Sache, die Sie mit programmieren können – eine Seite, ein Textfeld, eine Datei, ein Bild, einer webanforderung, eine e-Mail-Nachricht, einen Kundendatensatz usw. Objekte verfügen über eine oder mehrere *Eigenschaften* , die ihren Merkmalen beschreiben. Ein Text-Box-Objekt hat eine Texteigenschaft (neben anderen), eine Request-Objekt verfügt über eine Url-Eigenschaft (und andere), eine e-Mail-Nachricht eine From-Eigenschaft und eine Eigenschaft an, und so weiter. Objekte verfügen außerdem über *Methoden* , sind die "Verben" ausführen. Sie müssen Objekte viel arbeiten.

Wie aus dem Beispiel sehen ist, ist "DateTime" ein Objekt, das Sie Programm Datumsangaben und Uhrzeiten kann. Es wurde eine Eigenschaft namens jetzt, die das aktuelle Datum und die Uhrzeit zurückgibt.

### <a name="using-code-to-render-markup-in-the-page"></a>Verwenden von Code zum Rendern von Markup in der Seite "

Beachten Sie im Hauptteil der Seite "Folgendes ein:

[!code-html[Main](intro-to-web-pages-programming/samples/sample3.html)]

Erneut, das @-Zeichen weist ASP.NET folgt, die nicht HTML-Code ist. Im Markup Sie können hinzufügen gefolgt von eines Codeausdrucks und ASP.NET wird den Wert dieses Ausdrucks rechts an diesem Punkt gerendert. Im Beispiel @a wird dargestellt, wie der Wert der benannten Variablen aufweist, @product gerendert wird, was in der Variable benannte Produkt, usw. ist.

Sie sind nicht auf Variablen, begrenzt jedoch. In einigen Fällen hier das @-Zeichen vorangestellt ist einen Ausdruck:

- @(eine\*b) rendert das Produkt der in der Variablen ein, und b. (Die \* Operator bedeutet Multiplikation.)
- @(Technologie + "" + Produkt) die Werte in den Variablen-Technologie und dem Produkt nach verketten und das Hinzufügen von Leerzeichen in der Zwischenzeit rendert. Der Operator (+) für das Verketten von Zeichenfolgen ist identisch mit dem Operator für das Hinzufügen von Zahlen. ASP.NET in der Regel ist ersichtlich, ob Sie mit Zahlen oder Zeichenfolgen arbeiten und führt die Aufgabe aus rechten mit dem + -Operator.
- @Request.Url Rendert die Url-Eigenschaft für das Anforderungsobjekt. Das Request-Objekt enthält Informationen zur aktuellen Anforderung aus dem Browser, und natürlich die Url-Eigenschaft enthält die URL der aktuellen Anforderung.

Das Beispiel ist auch konzipiert, um anzuzeigen, dass Sie, die Sie können auf unterschiedliche Weise funktionieren. Sie können Berechnungen im Codeblock oben ausführen, fügen Sie die Ergebnisse in eine Variable und klicken Sie dann die Variable in Markup gerendert. Oder Sie können Berechnungen in einem Ausdruck rechts im Markup. Der Ansatz, den Sie verwenden, hängen von was Sie tun und zu einem gewissen Grad auf Ihren eigenen Anforderungen entsprechend.

### <a name="seeing-the-code-in-action"></a>Sehen den Code in Aktion

Mit der rechten Maustaste in des Namens der Datei, und wählen Sie dann **in Browser starten**. Sie finden Sie auf der Seite im Browser mit den Werten und Ausdrücken, die auf der Seite aufgelöst.

!["TestRazor"-Seite im Browser ausgeführt wird](intro-to-web-pages-programming/_static/image2.png)

Betrachten Sie die Quelle im Browser aus.

!['Test Razor' Seitenquelle im browser](intro-to-web-pages-programming/_static/image3.png)

Wie von Ihren Erfahrungen im vorherigen Lernprogramm erwartet ist keiner der Razor-Code auf der Seite. Alles, was, die Sie sehen, werden die tatsächlichen Werte. Beim Ausführen einer Seite können Sie tatsächlich eine Anforderung an dem Webserver ausführen, die in WebMatrix integriert ist. Wenn die Anforderung empfangen wird, wird von ASP.NET aufgelöst wird, die Werte und Ausdrücke und ihre Werte in die Seite rendert. Anschließend wird die Seite an den Browser sendet.

> [!TIP] 
> 
> **Razor und c#**
> 
> Bis jetzt haben gesagt, dass die Arbeit mit Razor-Syntax. Das "true" ist, aber es ist nicht die vollständige Story. Die tatsächliche Programmiersprache, die Sie verwenden, die aufgerufen wird *c#*. C#-stellt von Microsoft erstellt wurde, die über einem Jahrzehnt vor und eine der primären Programmiersprachen für das Windows-apps erstellen. Alle Regeln, die Sie Informationen zum Benennen Sie einer Variablenverweis und Gewusst wie: Erstellen von Anweisungen usw. gesehen haben, werden tatsächlich alle Regeln der Programmiersprache c#.
> 
> Razor bezieht sich auf den kleinen Satz von Konventionen für, wie Sie diesen Code in eine Seite einbetten. Z. B. der Konvention von mit Code auf der Seite zu markieren und mit @ {}, um einen Codeblock einzubetten ist der Razor-Aspekt von einer Seite. Hilfsprogramme gelten ebenfalls als Razor verwendet werden. Razor-Syntax ist an mehr Stellen als einfach in ASP.NET Web Pages verwendet. (Beispielsweise ist es in ASP.NET MVC-Ansichten sowie verwendet.)
> 
> Wir erwähnt, da Wenn Sie Informationen zur Programmierung ASP.NET Web Pages ansehen, Sie zahlreiche Verweise auf Razor finden. Ein Großteil dieser Verweise nicht gelten jedoch für was Sie dies und können daher verwirrend sein. Und in der Tat viele Ihrer Fragen Programmierung sind tatsächlich werden zum Arbeiten mit c# oder Arbeiten mit ASP.NET. Also wenn Sie speziell für Informationen zu Razor ansehen, möglicherweise nicht die finden Sie Antworten, die Sie benötigen.


## <a name="adding-some-conditional-logic"></a>Bedingten Logik hinzufügen

Eine großartige Funktion zur Verwendung von Code in einer Seite ist, dass Sie ändern können, was basierend auf verschiedenen Bedingungen geschieht. In diesem Teil des Lernprogramms werden Sie experimentieren mit einige Methoden zum Ändern der Anzeige auf der Seite.

Im Beispiel werden einfach und leicht kompliziert, so dass wir auf die Bedingungslogik konzentrieren können. Die Seite erstellen Sie nun führt diese Schritte aus:

- Zeigen Sie verschiedene Text auf der Seite an, je nachdem, ob er zum ersten Mal wird die Seite wird angezeigt, oder gibt an, ob Sie eine Schaltfläche, um die Seite senden geklickt haben. Die der ersten Prüfung werden.
- Die Nachricht nur angezeigt, wenn ein bestimmter Wert in der Abfragezeichenfolge der URL (http://...?show=true) übergeben wird. Dies wird der zweite Prüfung sein.

Klicken Sie in WebMatrix erstellen Sie eine Seite, und nennen Sie sie *TestRazorPart2.cshtml*. (Klicken Sie im Menüband auf **neu**, wählen Sie **CSHTML**, nennen Sie die Datei, und klicken Sie dann auf **OK**.)

Ersetzen Sie den Inhalt der Seite mit den folgenden:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample4.cshtml)]

Der Codeblock am oberen initialisiert, eine Variable namens Nachricht mit etwas Text. Den Inhalt der Nachricht Variablen abfragedarstellung im Hauptteil der Seite innerhalb einer &lt;p&gt; Element. Das Markup enthält auch eine &lt;Eingabe&gt; Element zum Erstellen einer **senden** Schaltfläche.

Führen Sie die Seite, um zu sehen, wie er nun funktioniert. Jetzt ist es im Grunde eine statische Seite auch dann, wenn Sie die **Absenden** Schaltfläche.

Wechseln Sie zurück in WebMatrix. Fügen Sie folgenden hervorgehobenen Code hinzu, in den Codeblock *nach* die Zeile, die Nachricht initialisiert:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample5.cshtml?highlight=4-6)]

### <a name="the-if---block"></a>Die If {}-Block

Was Sie gerade hinzugefügt haben, wurde ein, wenn Bedingung. Im Code wird die Bedingung verfügt eine Struktur wie folgt:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample6.cs)]

Die zu testende Bedingung wird in Klammern angegeben. Es muss ein Wert oder ein Ausdruck, der "true" oder "false" zurückgibt. Wenn die Bedingung "true" ist, führt ASP.NET den-Anweisungen, die in der geschweiften Klammern stehen. (Diese sind die *dann* Teil der *If-Then-* Logik.) Wenn die Bedingung "false" ist, wird der Codeblock übersprungen.

Hier sind einige Beispiele der Bedingungen, die Sie, in einer If testen können Anweisung:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample7.cs)]

Variablen, die gegen Werten oder Ausdrücken können Sie testen, indem eine <em>logischer Operator</em> oder <em>Vergleichsoperator</em>: gleich (==), größer als (&gt;), kleiner als (&lt;), größer als oder gleich (&gt;=), und kleiner als oder gleich (&lt;=). Der! =-Operator bedeutet ungleich – z. B. wenn (eine! = 0) bedeutet, dass <em>Wenn</em> <em>eine</em><em>ist nicht gleich 0</em>.

> [!NOTE]
> Stellen Sie sicher, dass Sie feststellen, dass der Vergleichsoperator für gleich (==) nicht der = identisch ist. Den =-Operator wird nur zum Zuweisen von Werten verwendet (Var eine = 2). Wenn Sie diese Operatoren verwechseln, entweder erhalten Sie einen Fehler, oder Sie unerwartete Ergebnissen erhalten.


Um zu testen, ob "true" ist, wird die vollständige Syntax If(IsDone == true). Aber Sie können auch die Verknüpfung if(IsDone) verwenden. Ist keine Vergleichsoperator, wird ASP.NET davon ausgegangen, dass Sie für "true" testen.

Die! Operator allein bedeutet eine logische NOT. Z. B. die Bedingung If (! IsPost) bedeutet, dass *Wenn IsPost nicht "true" ist*.

Kombinieren Sie Bedingungen, mit der eine logische AND-Operation (&amp; &amp; Operator) oder logischen OR (||-Operator). Beispielsweise der letzten zurück, wenn die Bedingungen in den vorherigen Beispielen bedeutet *Wenn FileProcessingIsDone nicht festgelegt ist, auf "true" und DisplayMessage auf "false" festgelegt ist*.

### <a name="the-else-block"></a>Else-block

Eine endgültige Sache If Blöcke: ein Wenn Block eine else-Block folgen kann. Else-Block ist nützlich ist, müssen Sie die anderen Code, der ausgeführt wird, wenn die Bedingung "false" ist. Hier ist ein einfaches Beispiel:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample8.cs)]

Einige Beispiele für die sehen in späteren Lernprogrammen in dieser Serie Sie einen else-Block nützlich, verwenden ist.

### <a name="testing-whether-the-request-is-a-submit-post"></a>Testen, ob die Anforderung übermitteln (Post) ist.

Es gibt weitere, aber wir zurückkehren zur im Beispiel, das die Bedingung if(IsPost) {...} hat. IsPost ist tatsächlich eine Eigenschaft der aktuellen Seite. Der ersten Seite angefordert wird, IsPost "false" zurückgegeben. Jedoch wenn Sie auf eine Schaltfläche klicken, oder andernfalls senden die Seite – d. h. veröffentlichen – IsPost "Wahr" zurückgegeben. Daher kann IsPost zu bestimmen, ob Sie mit einer Formularübergabe arbeiten. (Im Hinblick auf HTTP-Verben, wenn die Anforderung einen GET-Vorgang ist IsPost "false" zurückgegeben. Wenn die Anforderung eine POST-Vorgang ist, gibt IsPost "true".) In einem späteren Lernprogramm arbeiten Sie mit als Eingabeformulare, in dem dieser Test besonders nützlich ist.

Führen Sie die Seite. Da dies das erste Mal ist sind angeforderte der Seite "This is erstmalig haben Sie die Seite angefordert" angezeigt. Diese Zeichenfolge ist der Wert, dem Sie die Nachrichtenvariable initialisiert. Es ist ein Test if(IsPost), aber, die im Moment "false" zurückgegeben, damit der Code in der If blockieren Sie nicht ausgeführt werden.

Klicken Sie auf die **Absenden** Schaltfläche. Die Seite wird erneut angefordert. Als vor, die Message-Variable festgelegt, zu "This is erstmalig..." klicken. Jedoch dieses Mal die Test-if(IsPost) Gibt true zurück, damit der Code in der If-Block ausgeführt. Der Code ändert den Wert der Variablen Nachricht an einen anderen Wert, der angibt, was im Markup gerendert wird.

Fügen Sie nun eine, wenn die Bedingung im Markup. Unterhalb der &lt;p&gt; -Element, enthält die **Absenden** Schaltfläche, fügen Sie das folgende Markup hinzu:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample9.cshtml)]

Sie fügen Code innerhalb des Markups, daher Sie zum Einstieg müssen @. Es ist eine, wenn Test ähnlich derjenigen, die Sie zuvor hinzugefügten Parameter oben im Codeblock. In der geschweiften Klammern, Sie fügen gewöhnliches HTML – mindestens, ist es gewöhnliche bis erreicht wird @DateTime.Now. Dies auf eine andere wenig Razor-Code ist, müssen daher erneut Sie @ davor hinzufügen.

Der Punkt hier ist, dass Sie hinzufügen können, wenn beide Bedingungen der Code im oberen Bereich und in das Markup blockieren. Bei Verwendung von ein, wenn Markup oder-Code Bedingung im Text der Seite, die Zeilen innerhalb des Blocks möglich. In diesem Fall und als "true" wird jedes Mal, wenn Sie Markup und Code mischen, müssen Sie zu verdeutlichen sie ASP.NET ist, in dem der Code @ verwenden.

Führen Sie die Seite, und klicken Sie auf **Absenden**. Diesmal sehen Sie nicht nur eine andere Meldung, wenn Sie übermitteln ("nachdem Sie gesendet haben...") können Sie jedoch eine neue Nachricht, die das Datum und die Uhrzeit werden aufgelistet.

!['Test Razor-2' Seite im Browser ausgeführt wird, mit einem Zeitstempel, der anzeigt, die nach dem Senden](intro-to-web-pages-programming/_static/image4.png)

### <a name="testing-the-value-of-a-query-string"></a>Testen den Wert der Abfragezeichenfolge

Eine weitere Tests. Diesmal, fügen Sie eine If-Block, der testet, einen Wert mit dem Namen anzeigen, die in der Abfragezeichenfolge übergeben werden kann. (Wie folgt: `http://localhost:43097/TestRazorPart2.cshtml?show=true`) ändern Sie die Seite, damit die Nachricht Sie angezeigt wurde haben ("This is erstmalig...", usw.) wird nur angezeigt, wenn der Wert des anzeigen auf "true" festgelegt ist.

An die untere (jedoch inneren) der Codeblock am oberen Rand der Seite "Folgendes ein:

[!code-csharp[Main](intro-to-web-pages-programming/samples/sample10.cs)]

Der vollständige Code Block jetzt aussehen wie im folgenden Beispiel. (Beachten Sie, dass wenn Sie den Code in die Seite kopieren, der Einzug anders aussehen könnte. Aber, von denen nicht beeinflusst, wie der Code ausgeführt wird.)

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample11.cshtml)]

Der neue Code im Block initialisiert, eine Variable namens ShowMessage auf "false". Es wird dann eine, wenn Tests für einen Wert in der Abfragezeichenfolge gesucht werden soll. Wenn Sie die Seite zuerst anfordern, weist sie eine URL wie die folgende:

`http://localhost:43097/TestRazorPart2.cshtml`

Der Code bestimmt, ob die URL eine Variablen mit dem Namen in der Abfragezeichenfolge an, wie diese Version der URL anzeigen enthält:

`http://localhost:43097/TestRazorPart2.cshtml`? anzeigen = "true"

Der Test selbst prüft die QueryString-Eigenschaft für das Anforderungsobjekt ab. Wenn die Abfragezeichenfolge ein Element mit dem Namen anzeigen enthält, und dieses Element festgelegt ist, auf "true", die bei Block ausgeführt wird, und legt die ShowMessage-Variable auf "true" fest.

Es gibt ein Trick, wie Sie sehen können. Wie der Name sagt, ist die Zeichenfolge der Abfrage eine Zeichenfolge an. Allerdings können Sie nur "true" und "false" testen, wenn der Wert, den Sie testen, einen booleschen Wert ("true" / falsch) ist. Bevor Sie den Wert der Variablen anzeigen in der Abfragezeichenfolge testen können, müssen Sie es in einen booleschen Wert konvertiert. Was bewirkt, dass die Methode AsBool ist – er nimmt eine Zeichenfolge als Eingabe und konvertiert ihn in einen booleschen Wert. Wenn die Zeichenfolge "Wahr" gesetzt ist, konvertiert die AsBool-Methode eindeutig, diesen Wert auf "true". Wenn der Wert der Zeichenfolge ein etwas anderes angegeben ist, zurückgegeben AsBool "false".

> [!TIP] 
> 
> **Datentypen und As()-Methoden**
> 
> Wir haben nur bisher beschlossen, wenn Sie eine Variable zu erstellen, verwenden Sie das Schlüsselwort "var". Dies ist jedoch nicht den gesamten Text. Um Werte zu bearbeiten, Zahlen, hinzufügen oder Verketten von Zeichenfolgen, oder Vergleichen von Datumsangaben oder für "true" / "false" Testen – c# wurde mit einer entsprechenden internen Darstellung des Werts. C# können *in der Regel* herausfinden, welche dieser Darstellung werden soll (d. h. was *Typ* die Daten sind) basierend auf den mit den Werten tun. Jetzt und dann jedoch, die keine Chance. Wenn dies nicht der Fall ist, mit deren Hilfe indem Sie explizit angeben, wie c# die Daten darstellen soll. Die AsBool-Methode wird das – teilt mit c#, dass ein String-Wert "true" oder "false" als einen booleschen Wert behandelt werden sollen. Ähnliche Methoden, die zum Darstellen von Zeichenfolgen als auch andere Typen wie AsInt (Treat als ganze Zahl), AsDateTime (Treat als einen Datums-/Uhrzeitwert), AsFloat (Treat als Gleitkommazahl) usw. vorhanden sein. Wenn Sie diese als ()-Methoden verwenden handelt es sich bei c# den Zeichenfolgenwert kann nicht wie angefordert, wird einen Fehler angezeigt.


Entfernen Sie in das Markup der Seite, oder kommentieren Sie Sie aus diesem Element (Hier wird angezeigt als Kommentare formatiert):

[!code-html[Main](intro-to-web-pages-programming/samples/sample12.html)]

Rechts, in denen Sie entfernt oder auskommentiert Text, fügen Sie die folgenden:

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample13.cshtml)]

Die wenn Test gerendert werden Text, wenn die Variable ShowMessage "true" ist ein &lt;p&gt; Element mit dem Wert der Variablen angezeigt.

### <a name="summary-of-your-conditional-logic"></a>Überblick über die bedingte Logik

Für den Fall, dass Sie nicht vollständig genau wissen sind, was Sie gerade getan haben, wird hier eine Zusammenfassung an.

- Die Nachrichtenvariable wird initialisiert, um eine standardmäßige Zeichenfolge ("This is erstmalig...").
- Wenn die Seitenanforderung übermitteln (Post) ergibt, wird der Wert der Nachricht zum geändert "nachdem Sie gesendet haben..."
- Die ShowMessage-Variable wird auf "false" initialisiert.
- Wenn die Abfragezeichenfolge enthält? anzeigen = "true", wird die Variable ShowMessage festgelegt auf "true".
- Im Markup, wenn ShowMessage "true" ist ein &lt;p&gt; -Element gerendert, die den Wert der Nachricht anzeigt. (Wenn ShowMessage auf "false" festgelegt ist, wird nichts an diesem Punkt im Markup gerendert.)
- Im Markup, wenn die Anforderung einen Beitrag ist ein &lt;p&gt; -Element gerendert, in dem das Datum und die Uhrzeit angezeigt.

Führen Sie die Seite. Es gibt keine Meldung auf, da ShowMessage "false" ist, sodass im Markup der if(showMessage)-Test "false" zurückgibt.

Klicken Sie auf **Senden**. Daraufhin wird das Datum und Uhrzeit, aber noch keine Nachricht.

In Ihrem Browser, wechseln Sie in das Feld URL ein, und fügen Sie Folgendes am Ende der URL:? anzeigen = "true", und drücken Sie dann die EINGABETASTE.

!['Test Razor-2' Seite im Browser, die mit einer Abfragezeichenfolge](intro-to-web-pages-programming/_static/image5.png)

Die Seite wird erneut angezeigt. (Da Sie die URL geändert haben, ist dies eine neue Anforderung, nicht übermitteln.) Klicken Sie auf **senden** erneut aus. Die Nachricht wird erneut angezeigt, wie das Datum und die Uhrzeit ist.

![Seite 'Test Razor-2', nach gesendet werden, wenn eine Abfragezeichenfolge vorhanden ist](intro-to-web-pages-programming/_static/image6.png)

Ändern Sie die URL? anzeigen = true? anzeigen = "false", und drücken Sie die EINGABETASTE. Senden Sie die Seite erneut. Die Seite zurück, wie Sie gestartet wird, keine Nachricht.

Wie bereits erwähnt, ist die Logik der in diesem Beispiel wird ein kleines kompliziert. Jedoch, wenn also in vielen Ihren Seiten aufgerufen werden und dauert mindestens einer der Formen, die Sie kennen gelernt haben hier.

## <a name="installing-a-helper-displaying-a-gravatar-image"></a>Installieren eine Hilfsmethode (Anzeigen eines a. mit Gravatar-Images)

Einige Aufgaben, die Benutzer häufig auf Webseiten viel Code erfordern, oder zusätzliche Kenntnisse erfordern. Beispiele: Anzeigen eines Diagramms für Daten; Platzieren eine Facebook "Wie"-Schaltfläche auf einer Seite; sendende e-Mail aus Ihrer Website; Zuschneiden oder Ändern der Bildgröße; Verwenden von PayPal für Ihre Website. Um diese Arten von Aufgaben zu erleichtern, können Sie mithilfe von ASP.NET Web Pages *Hilfsprogramme*. Komponenten, die Sie für einen Standort installieren und, mit deren Hilfe Sie die typischen Aufgaben mit wenigen Codezeilen Razor, sind Hilfsmethoden.

ASP.NET Web Pages verfügt über einige integrierte Hilfsprogramme. Viele Hilfsprogramme stehen jedoch in Paketen (-add-ins), die mit der NuGet-Paket-Manager bereitgestellt werden. NuGet ermöglicht die Auswahl ein Pakets installiert, und klicken Sie dann diese übernimmt alle Details der Installation.

In diesem Teil des Lernprogramms installieren Sie eine Hilfsprogramm, mit dem Sie ein Image a. mit Gravatar ("Global erkannten Avatar") anzeigen können. Sie erfahren, dass zwei Dinge. Eine ist zum Suchen und installieren eine Hilfsmethode. Außerdem erfahren, wie eine Hilfsprogramm ganz einfach eine Aktion auszuführen, die Sie andernfalls benötigen würden mithilfe viel Code für die selbst schreiben.

Registrieren Sie Ihren eigenen a. mit Gravatar an der a. mit Gravatar-Website unter [ http://www.gravatar.com/ ](http://www.gravatar.com/), aber es ist nicht unbedingt ein a. mit Gravatar-Konto zum Ausführen dieser Teil des Lernprogramms zu erstellen.

Klicken Sie in WebMatrix, auf die **NuGet** Schaltfläche.

![Im Dialogfeld NuGet Gallery WebMatrix](intro-to-web-pages-programming/_static/image7.png)

Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt. (Nicht alle Pakete sind Hilfen; einige Funktionen hinzufügen WebMatrix selbst, einige sind zusätzliche Vorlagen, und so weiter.) Sie erhalten möglicherweise eine Fehlermeldung über Versionskonflikts. Sie können diese Fehlermeldung ignorieren, indem Sie auf **OK** und Sie mit diesem Lernprogramm fortfahren.

![Im Dialogfeld NuGet Gallery WebMatrix](intro-to-web-pages-programming/_static/image8.png)

Geben Sie in das Suchfeld "asp.net-Hilfsprogramme" aus. NuGet zeigt die Pakete, die den Suchbegriffen übereinstimmen.

![NuGet-Katalog in WebMatrix Pakete anzeigen](intro-to-web-pages-programming/_static/image9.png)

Der ASP.NET Web Helpers Library enthält Code aus, um viele allgemeine Aufgaben, einschließlich der Verwendung von a. mit Gravatar-Images zu vereinfachen. Wählen Sie die **ASP.NET Web Helpers Library** Verpacken, und klicken Sie dann auf **installieren** um das Installationsprogramm zu starten. Wählen Sie **Ja** Wenn gefragt, ob Sie verwenden möchten, installieren Sie das Paket, und stimme den Bedingungen, um die Installation abzuschließen.

Das ist alles. NuGet heruntergeladen und installiert alle Elemente einschließlich alle zusätzlichen Komponenten, die erforderlich sind (*Abhängigkeiten*).

Wenn Sie eine Hilfsprogramm deinstalliert haben, ist der Prozess sehr ähnlich. Klicken Sie auf die **NuGet** Schaltfläche, klicken Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.

## <a name="using-a-helper-in-a-page"></a>Verwenden eine Hilfsmethode auf einer Seite

Jetzt verwenden Sie das Hilfsprogramm, das Sie gerade installiert haben. Die Vorgehensweise zum Hinzufügen eines Hilfsprogramms auf eine Seite ist für die meisten Hilfsprogramme ähnlich.

Klicken Sie in WebMatrix erstellen Sie eine Seite, und nennen Sie sie *GravatarTest.cshml*. (Sie erstellen eine spezielle Seite, um das Hilfsobjekt zu testen, aber Hilfen in eine andere Seite in Ihrer Website verwendet werden können.)

Innerhalb der &lt;Text&gt; Element, Hinzufügen einer &lt;Div&gt; Element. Innerhalb der &lt;Div&gt; Element, geben Sie Folgendes:

@Gravatar

Das @-Zeichen ist das gleiche Zeichen, die Sie zum Kennzeichnen von Razor-Code verwendet haben. **A. mit Gravatar** ist das Hilfsprogramm-Objekt, das mit dem Sie arbeiten.

Sobald Sie den Punkt (.) eingeben, zeigt WebMatrix eine Liste von *Methoden* (Funktion), dass die a. mit Gravatar-Hilfsprogramm zur Verfügung stellt:

![A. mit Gravatar Helper IntelliSense Dropdown-Liste](intro-to-web-pages-programming/_static/image10.png)

Diese Funktion wird als bezeichnet *IntelliSense*. Sie können Sie den Code, indem Sie kontextabhängige Auswahl bereitstellen. IntelliSense funktioniert mit HTML, CSS, ASP.NET Code, JavaScript und anderen Sprachen, die in WebMatrix unterstützt werden. Es ist eine weitere Funktion, die zum Entwickeln von Webseiten in WebMatrix erleichtert.

Drücken Sie G auf der Tastatur, und Sie sehen, dass es sich bei IntelliSense GetHtml-Methode sucht. Drücken Sie TAB. IntelliSense fügt der ausgewählten Methode ("GetHtml") für Sie. Geben Sie eine öffnende Klammer ein, und beachten Sie, dass die schließende Klammer automatisch hinzugefügt wird. Geben Sie Ihre e-Mail-Adresse in Anführungszeichen zwischen den zwei Klammern ein. Wenn Sie ein a. mit Gravatar-Konto verfügen, wird Ihr Profilfoto zurückgegeben. Wenn Sie nicht über ein a. mit Gravatar-Konto verfügen, wird ein Standardbild zurückgegeben. Wenn Sie fertig sind, sieht die Zeile wie folgt:

[!code-css[Main](intro-to-web-pages-programming/samples/sample14.css)]

Die Seite wird jetzt in einem Browser anzuzeigen. Das Bild oder das Standardbild wird angezeigt, je nachdem, ob Sie ein a. mit Gravatar-Konto besitzen.

![A. mit Gravatar](intro-to-web-pages-programming/_static/image11.png) ![Standardbild](intro-to-web-pages-programming/_static/image12.png)

Um einen Eindruck von Was ist das Hilfsobjekt für Sie ausführen, zeigen Sie die Quelle der Seite im Browser. Zusammen mit den HTML-Code, die Sie auf der Seite mussten, sehen Sie ein Image-Element, das kein Bezeichner eingeschlossen ist. Dies ist Code, der das Hilfsprogramm in der Seite an der Stelle gerendert, in dem Sie hatten @Gravatar.GetHtml. Das Hilfsobjekt benötigte Informationen bereitgestellt und generiert den Code, der direkt mit a. mit Gravatar kommuniziert, um das richtige Bild für das angegebene Konto wieder zu erhalten.

GetHtml-Methode können Sie das Bild anpassen, indem Sie andere Parameter bereitstellen. Der folgende Code zeigt, wie angefordert wird, ein Abbild verfügt über eine Breite und Höhe von 40 Pixel und verwendet einen angegebenen Standardbild mit dem Namen **Wavatar** , wenn das angegebene Konto nicht vorhanden ist.

[!code-javascript[Main](intro-to-web-pages-programming/samples/sample15.js)]

Dieser Code erzeugt etwa wie das folgende Ergebnis (das Standardbild variiert nach dem Zufallsprinzip).

![](intro-to-web-pages-programming/_static/image13.png)

## <a name="coming-up-next"></a>Als Nächstes kommen

Um dieses Lernprogramm kurz zu halten, mussten wir auf nur einigen Grundlagen zu konzentrieren. Natürlich ist eine *viele* mehr zu Razor und c#. Erfahren Sie, ob Sie mehr als Sie diese Lernprogramme durchlaufen. Wenn Sie lernen mehr über die Programmierung Aspekte der Razor- und C#-zurzeit interessiert sind, erfahren Sie hier eine tiefer gehendes Einführung: [Einführung in ASP.NET Web-Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890).

Nächste Lernprogramm führt Sie zum Arbeiten mit einer Datenbank. In diesem Lernprogramm beginnen Sie zum Erstellen der beispielanwendung, mit dem Sie Ihre bevorzugten Filme auflisten kann.

## <a name="complete-listing-for-testrazor-page"></a>Vollständige Auflistung für die Seite "TestRazor"

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample16.cshtml)]

## <a name="complete-listing-for-testrazorpart2-page"></a>Vollständige Auflistung für die Seite "TestRazorPart2"

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample17.cshtml)]

## <a name="complete-listing-for-gravatartest-page"></a>Vollständige Auflistung für die Seite "GravatarTest"

[!code-cshtml[Main](intro-to-web-pages-programming/samples/sample18.cshtml)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Einführung in ASP.NET-Webprogrammierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Twitter-Hilfsprogramm](../../ui-layouts-and-themes/twitter-helper.md)

> [!div class="step-by-step"]
> [Zurück](getting-started.md)
> [Weiter](displaying-data.md)
