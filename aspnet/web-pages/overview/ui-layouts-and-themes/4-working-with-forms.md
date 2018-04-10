---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Arbeiten mit HTML-Formularen in ASP.NET Web Pages (Razor)-Websites | Microsoft Docs
author: tfitzmac
description: Ein Formular ist, einen Abschnitt eines HTML-Dokuments, in denen Benutzereingaben-Steuerelemente, z. B. Textfelder, Kontrollkästchen, Optionsfelder und Pull-Dropdownlisten abgelegt, wird. Verwenden Sie Formulare geringer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Arbeiten mit HTML-Formularen in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie zum Verarbeiten von einem HTML-Formular (mit den Schaltflächen und Textfelder) bei der Arbeit in einer ASP.NET Web Pages (Razor)-Website.
> 
> **Lernen Sie:** 
> 
> - Vorgehensweise: Erstellen von einem HTML-Formular.
> - Wie Benutzereingaben aus dem Formular gelesen.
> - Zum Überprüfen von Benutzereingaben.
> - Wie Formularwerte wiederhergestellt, nachdem die Seite übermittelt wird.
> 
> Hierbei handelt es sich um den ASP.NET Programmierungskonzepte eingeführt, die im Artikel:
> 
> - Das `Request`-Objekt.
> - Validierung von Benutzereingaben.
> - HTML-Codierung.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Erstellen einen einfachen HTML-Formular

1. Erstellen einer neuen Website.
2. Erstellen Sie eine Webseite mit dem Namen in den Stammordner *Form.cshtml* , und geben Sie das folgende Markup:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Starten Sie die Seite in Ihrem Browser. (In WebMatrix, in der **Dateien** Arbeitsbereich mit der rechten Maustaste in der das, und wählen Sie dann **in Browser starten**.) Ein einfaches Formular mit drei Eingabefeldern und ein **Absenden** -Schaltfläche angezeigt wird.

    ![Screenshot eines Formulars mit drei Textfelder.](4-working-with-forms/_static/image1.jpg)

    An diesem Punkt, wenn Sie auf die **Absenden** Schaltfläche, geschieht nichts. Um das Formular nützlich ist, müssen Sie Code hinzufügen, der auf dem Server ausgeführt wird.

## <a name="reading-user-input-from-the-form"></a>Lesen Sie Benutzereingaben aus dem Formular

Um das Formular zu verarbeiten, fügen Sie Code, der die übermittelten Feldwerte liest und verarbeitet diese. Dieses Verfahren wird gezeigt, wie lesen die Felder und die Benutzereingabe auf der Seite angezeigt. (In einer produktionsanwendung werden im Allgemeinen Weitere interessante Vorgänge mit Benutzereingaben. Sie müssen, die im Artikel zum Arbeiten mit Datenbanken ausführen.)

1. Am oberen Rand der *Form.cshtml* Datei, geben Sie den folgenden Code:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Wenn der Benutzer zunächst die Seite anfordert, wird nur die leere Form angezeigt. Der Benutzer (die werden) im Formular aufgefüllt und klickt dann auf **Absenden**. Dies übermittelt (bereitstellen) der Benutzereingabe mit dem Server. Standardmäßig wird die Anforderung zur selben Seite wechselt (d. h., *Form.cshtml*).

    Wenn Sie dieses Mal die Seite senden, werden die eingegebenen Werte oberhalb des Formulars angezeigt:

    ![Screenshot, der zeigt die Werte, die Sie eingegeben haben, auf der Seite angezeigt.](4-working-with-forms/_static/image2.jpg)

    Betrachten Sie den Code für die Seite aus. Verwenden Sie zunächst die `IsPost` Methode, um zu bestimmen, ob die Seite zurückgesendet wird, wird &#8212; d. h., ob ein Benutzer geklickt hat die **Absenden** Schaltfläche. Ist dies ein Post `IsPost` "Wahr" zurückgegeben. Dies ist die Standardmethode in ASP.NET Web Pages zu bestimmen, ob Sie eine ursprüngliche Anforderung (eine GET-Anforderung) oder ein Postback (eine POST-Anforderung) verwenden. (Weitere Informationen zu GET und POST, finden Sie in der Randleiste "HTTP-GET und POST und die IsPost-Eigenschaft" in [Einführung in ASP.NET Web Pages Programmierung mithilfe der Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Anschließend rufen Sie die Werte, die der Benutzer von ausgefüllt werden, in der `Request.Form` -Objekt, und fügen Sie sie Variablen für die spätere. Die `Request.Form` Objekt enthält alle Werte, die auf der Seite gesendet wurden, jeweils mit einem Schlüssel identifiziert. Der Schlüssel ist die Entsprechung der `name` Attribut des Formularfelds, die Sie lesen möchten. Z. B. zum Lesen der `companyname` Feld (Textfeld), verwenden Sie `Request.Form["companyname"]`.

    Formularwerte werden gespeichert, der `Request.Form` Objekt als Zeichenfolgen. Sie müssen als eine Zahl oder ein Datum oder einen anderen Typ mit einem Wert arbeiten, müssen Sie aus diesem Grund aus einer Zeichenfolge in diesen Typ zu konvertieren. Im Beispiel die `AsInt` Methode der `Request.Form` wird verwendet, um den Wert des Felds Mitarbeiter (die Anzahl der Mitarbeiter enthält) in eine ganze Zahl zu konvertieren.
2. Starten Sie die Seite in Ihrem Browser, füllen Sie die Felder, und klicken Sie auf **Absenden**. Die Seite zeigt die Werte, die Sie eingegeben haben.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML-Codierung für die Darstellung und Sicherheit
> 
> Gibt spezielle Verwendungsmöglichkeiten für Zeichen wie für HTML `<`, `>`, und `&`. Wenn diese Sonderzeichen angezeigt, in dem sie nicht erwartet haben, können sie die Darstellung und Funktionalität Ihrer Webseite beschädigen. Der Browser interpretiert z. B. die `<` (es sei denn, gefolgt von einem Leerzeichen ist) als Anfang eines HTML-Elements, z. B. Zeichen `<b>` oder `<input ...>`. Wenn der Browser das Element nicht erkennt, verwirft er einfach die Zeichenfolge, die mit beginnt `<` bis etwas, die erreicht wird erneut erkannt. Natürlich kann dies einige seltsames Rendering auf der Seite führen.
> 
> HTML-Codierung ersetzt diesen reservierten Zeichen durch ein Code, den Browser als das richtige Symbol interpretiert werden soll. Z. B. die `<` Zeichen wird durch ersetzt `&lt;` und `>` Zeichen wird durch ersetzt `&gt;`. Der Browser rendert diese Ersatzzeichenfolgen als eines der Zeichen, die Sie anzeigen möchten.
> 
> Es ist eine gute Idee, die HTML-Codierung jederzeit anzeigen von Zeichenfolgen verwendet (Eingabe), dass Sie von einem Benutzer erhalten haben. Wenn Sie dies nicht tun, versucht ein Benutzer kann die Webseite ein böswilliges Skript ausführen oder ein anderes Element abrufen, die Ihre Websitesicherheit kompromittiert oder einfach nicht gewünscht ist. (Dies ist besonders wichtig, wenn Sie Benutzereingaben an Unbekannter speichern, und zeigen Sie es später &#8212; z. B. einen Blogkommentar, überprüfen Sie die Benutzer oder etwa,.)
> 
> Um zu verhindern, diese Probleme zu ASP.NET Web Pages automatisch HTML-codiert Text Inhalte, die Sie aus dem Code ausgegeben. Wenn Sie z. B. den Inhalt einer Variablen oder einem Ausdruck mit Code wie anzeigen `@MyVar`, ASP.NET Web Pages codiert automatisch die Ausgabe.


## <a name="validating-user-input"></a>Validieren von Benutzereingaben

Benutzer Fehler unterlaufen. Sie bitten Sie ihn, füllen Sie ein Feld, und dies vergessen, oder Sie bitten, sie geben die Anzahl der Mitarbeiter, und geben sie stattdessen einen Namen. Um sicherzustellen, dass ein Formular ordnungsgemäß ausgefüllt wurde vor der Verarbeitung, überprüfen Sie die Eingaben des Benutzers.

Diese Prozedur zeigt, wie alle drei Formularfelder, um sicherzustellen, dass der Benutzer diese leer lassen nicht überprüft. Sie überprüfen Sie, dass der Wert für die Mitarbeiter eine Zahl ist. Wenn Fehler vorhanden sind, zeigen Sie Fehler Nachricht, die dem Benutzer, welche Werte mitteilt die Überprüfung bestanden hat nicht.

1. In der *Form.cshtml* Datei, die erste Codeblock durch den folgenden Code ersetzen: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Um Benutzereingaben zu überprüfen, verwenden Sie die `Validation` Helper. Sie registrieren Pflichtfelder durch Aufrufen von `Validation.RequireField`. Sie andere Arten von Validierung registrieren, durch den Aufruf `Validation.Add` und das Feld, um zu überprüfen und den Typ der auszuführenden Validierung angibt.

    Wenn die Seite ausgeführt wird, führt ASP.NET alle Validierungstests für Sie. Überprüfen Sie die Ergebnisse durch Aufrufen von `Validation.IsValid`, womit "true", wenn alles übergeben und "false", wenn ein der Felder Fehler bei der Überprüfung. Meist rufen Sie `Validation.IsValid` vor dem Ausführen von Verarbeitungen auf die Benutzereingabe.
2. Update der `<body>` Element durch Hinzufügen von drei Aufrufe an die `Html.ValidationMessage` -Methode, wie folgt:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Um Überprüfungsfehlermeldungen anzuzeigen, können Sie Html aufrufen.`ValidationMessage` und übergeben sie den Namen des Felds, der die Meldung für soll.
3. Führen Sie die Seite. Lassen Sie die Felder leer, und klicken Sie auf **Absenden**. Fehlermeldungen angezeigt.

    ![Screenshot zeigt, dass Fehlermeldungen angezeigt, wenn Benutzereingaben Überprüfung nicht bestanden haben.](4-working-with-forms/_static/image3.jpg)
4. Eine Zeichenfolge (z. B. "ABC") hinzufügen, um die **Anzahl der Mitarbeiter** Feld, und klicken Sie auf **senden** erneut. Dieses Mal eine Fehlermeldung angezeigt wird, der angibt, wird die Zeichenfolge in das richtige Format auf, nämlich eine ganze Zahl nicht.

    ![Screenshot, in der angezeigte Fehlermeldungen angezeigt, wenn Benutzer eine Zeichenfolge für die Mitarbeiter-Feld eingeben.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages stellt mehr Optionen zum Überprüfen von Benutzereingaben, einschließlich der Möglichkeit zum automatischen Ausführen von mit Clientskripts, Validierung, damit Benutzer im Browser unmittelbares Feedback erhalten. Finden Sie unter der [zusätzliche Ressourcen](#Additional_Resources) Abschnitt weiter unten für Weitere Informationen.

## <a name="restoring-form-values-after-postbacks"></a>Wiederherstellen von Formularwerte nach Postbacks

Wenn Sie die Seite im vorherigen Abschnitt getestet haben, möglicherweise, die haben Sie bemerkt Wenn Sie tritt ein Validierungsfehler mussten, wird alles, was Sie eingegeben haben (nicht nur die ungültigen Daten) nicht mehr vorhanden wurde und mussten Sie Werte für alle Felder erneut eingeben. Dies veranschaulicht einen wichtigen Punkt: die Seite ist von Grund auf neu erstellt, beim Absenden einer Seite, sie verarbeitet und klicken Sie dann die Seite erneut zu rendern. Wie Sie gesehen haben, bedeutet dies, dass alle Werte, die auf der Seite wurden, wenn er übermittelt wurde, nicht mehr vorhanden sind.

Sie können dies problemlos, jedoch beheben. Sie haben Zugriff auf die Werte, die gesendet wurden (in der `Request.Form` -Objekt, damit Sie diese Werte wieder in die Felder füllen können, wenn die Seite gerendert wird.

1. In der *Form.cshtml* Datei, ersetzen Sie die `value` Attribute der `<input>` Elemente mithilfe der `value` Attribut.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Die `value` Attribut von der `<input>` Elemente festgelegt wurde, den Wert des Felds, der dynamisch zu lesen der `Request.Form` Objekt. Beim ersten, der die Seite angefordert wird, die Werte in der `Request.Form` Objekt alle leer sind. Dies ist in Ordnung, da auf diese Weise das Formular leer ist.
2. Starten Sie die Seite in Ihrem Browser, füllen Sie die Felder oder leer lassen, und klicken Sie auf **Absenden**. Eine Seite, die die übermittelten Werte wird angezeigt.

    ![Formulare 5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [1.001 Möglichkeiten zum Abrufen von Eingabe von Webbenutzern](https://msdn.microsoft.com/library/ms971057.aspx)
- [Verwendung von Formularen und Verarbeitung von Benutzereingaben](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Verwenden von AutoVervollständigen in HTML-Formularen](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
