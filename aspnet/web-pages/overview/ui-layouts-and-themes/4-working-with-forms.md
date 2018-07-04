---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Arbeiten mit HTML-Formularen in ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: tfitzmac
description: Ein Formular ist ein Teil eines HTML-Dokuments, in dem Sie die Benutzereingabe-Steuerelemente, z. B. Textfelder, Kontrollkästchen, Optionsfelder und Pull-Dropdownlisten einfügen. Verwenden Sie Formulare w-fragewörter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8d52cb6406859c77687622b7a101cf67781b863d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380346"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Arbeiten mit HTML-Formularen in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird beschrieben, wie ein HTML-Formular (mit Textfeldern und Schaltflächen) verarbeitet bei der Arbeit in einer ASP.NET Web Pages (Razor)-Website.
> 
> **Sie lernen Folgendes:** 
> 
> - Vorgehensweise: Erstellen Sie ein HTML-Formular.
> - So lesen Sie Benutzereingaben aus dem Formular.
> - Informationen zum Überprüfen von Benutzereingaben.
> - Informationen zum Formularwerte wiederherstellen, nachdem die Seite übermittelt wird.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Konzepten:
> 
> - Das `Request`-Objekt.
> - Validierung von Benutzereingaben.
> - HTML-Codierung.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Erstellen ein einfaches HTML-Formular

1. Erstellen einer neuen Website.
2. Erstellen Sie im Stammordner, einer Webseite, die mit dem Namen *Form.cshtml* , und geben Sie das folgende Markup:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Starten Sie die Seite in Ihrem Browser. (In WebMatrix, in der **Dateien** Arbeitsbereich mit der rechten Maustaste in der das, und wählen Sie dann **in Browser starten**.) Ein einfaches Formular mit drei Felder für die Eingabe und eine **senden** -Schaltfläche angezeigt wird.

    ![Screenshot des ein Formular mit drei Textfelder.](4-working-with-forms/_static/image1.jpg)

    An diesem Punkt, wenn Sie auf die **senden** Schaltfläche geschieht nichts. Um das Formular zu nutzen, müssen Sie Code hinzufügen, der auf dem Server ausgeführt wird.

## <a name="reading-user-input-from-the-form"></a>Auswerten von Benutzereingaben aus dem Formular

Um das Formular zu verarbeiten, fügen Sie Code, der die übermittelten Feldwerte liest und tut etwas mit ihnen. Dieses Verfahren zeigt, lesen die Felder aus, und die Benutzereingabe auf der Seite angezeigt. (In einer produktionsanwendung führen Sie in der Regel viel interessantere Dinge mit Benutzereingaben. Haben Sie Gelegenheit, die im Artikel zum Arbeiten mit Datenbanken.)

1. Am oberen Rand der *Form.cshtml* Datei, geben Sie den folgenden Code:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Wenn der Benutzer zuerst die Seite anfordert, wird nur die leere Formular angezeigt. Der Benutzer (die Sie sein wird), füllt das Formular, und klickt dann auf **senden**. Dadurch wird eine (bereitstellen) der Benutzereingabe an den Server. Standardmäßig wird die Anforderung zur selben Seite wechselt (d. h., *Form.cshtml*).

    Wenn Sie dieses Mal die Seite senden, werden die eingegebenen Werte direkt über das Formular angezeigt werden:

    ![Screenshot mit den von die Ihnen eingegebenen Werte auf der Seite angezeigt.](4-working-with-forms/_static/image2.jpg)

    Sehen Sie sich den Code für die Seite. Verwenden Sie zunächst die `IsPost` Methode, um zu bestimmen, ob die Seite zurückgesendet wird, wird &#8212; , also an, ob der Benutzer geklickt hat die **senden** Schaltfläche. Ist dies eine Post `IsPost` "Wahr" zurückgegeben. Dies ist das Standardverfahren in ASP.NET Web Pages zu bestimmen, ob Sie mit der ersten Anforderung (eine GET-Anforderung) oder ein Postback (eine POST-Anforderung) arbeiten. (Weitere Informationen zu den Get- und POST, finden Sie unter der Randleiste "HTTP-GET und POST und der IsPost-Eigenschaft" in [Einführung in ASP.NET Web Pages-Programmierung verwenden die Razor-Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Als Nächstes rufen Sie die Werte, die der Benutzer von, in ausgefüllt der `Request.Form` -Objekt verwenden, und fügen Sie sie Variablen für die spätere Verwendung. Die `Request.Form` Objekt enthält alle Werte, die auf der Seite gesendet wurden, jeweils durch einen Schlüssel identifiziert werden. Der Schlüssel ist, entspricht die `name` Attribut des Formularfelds, die Sie lesen möchten. Beispielsweise, um das Lesen der `companyname` Feld (Textfeld), verwenden Sie `Request.Form["companyname"]`.

    Formularwerte werden gespeichert, der `Request.Form` Objekt als Zeichenfolgen. Wenn Sie mit einem Wert als eine Zahl oder ein Datum oder eine andere Art arbeiten müssen, müssen Sie aus diesem Grund, aus einer Zeichenfolge in diesen Typ zu konvertieren. Im Beispiel die `AsInt` Methode der `Request.Form` wird verwendet, um den Wert des Felds Mitarbeiter (mit einer Anzahl der Mitarbeiter) in eine ganze Zahl zu konvertieren.
2. Die Seite in Ihrem Browser zu starten, geben die Formularfelder, und klicken Sie auf **senden**. Die Seite zeigt die Werte, die Sie eingegeben haben.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>HTML-Codierung für die Darstellung und Sicherheit
> 
> HTML hat spezielle Verwendungen für Zeichen, beispielsweise die `<`, `>`, und `&`. Wenn diese Sonderzeichen nicht angezeigt, in denen sie nicht erwartet haben, können sie die Darstellung und Funktionalität Ihrer Webseite auflöst. Z. B. Browser interpretiert die `<` (es sei denn, gefolgt von einem Leerzeichen ist) als Anfang eines HTML-Elements, wie Zeichen `<b>` oder `<input ...>`. Wenn der Browser das Element nicht erkennt, verwirft er einfach die Zeichenfolge, die mit beginnt `<` bis auf etwas, das erkennt es noch mal. Natürlich kann dies einige merkwürdige Darstellung auf der Seite führen.
> 
> HTML-Codierung ersetzt diesen reservierten Zeichen mit einem Code, den Browser als das richtige Symbol interpretiert werden soll. Z. B. die `<` Zeichen wird durch ersetzt `&lt;` und `>` Zeichen wird durch ersetzt `&gt;`. Der Browser rendert diese Ersetzungszeichenfolgen als eines der Zeichen, die Sie anzeigen möchten.
> 
> Es ist eine gute Idee, die mit HTML-Codierung von einem beliebigen Zeitpunkt, die Sie anzeigen, dass die Zeichenfolgen (Eingabe), Sie von einem Benutzer erstellt. Falls nicht, kann ein Benutzer versuchen, mit Ihrer Webseite ein böswilliges Skript ausführen oder eine andere stellt, die eine Beeinträchtigung der Sicherheit Ihrer Site oder einfach nicht gewünscht ist. (Dies ist besonders wichtig, wenn Sie Benutzereingaben, Stelle speichern, und klicken Sie dann später &#8212; z. B. einen Blogkommentar, Benutzer-Überprüfung oder so was.)
> 
> Zur Vermeidung dieser Probleme, die ASP.NET Web Pages automatisch HTML-codiert Text Inhalte, die Sie aus Ihrem Code ausgeben. Wenn Sie z. B. den Inhalt einer Variablen oder einen Ausdruck mit Code wie anzeigen `@MyVar`, ASP.NET Web Pages verschlüsselt automatisch die Ausgabe.


## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

Benutzer machen Fehler. Sie bitten Sie ihn, geben Sie in einem Feld, und sie vergessen haben, oder Sie bitten Sie ihn, die Anzahl der Mitarbeiter geben ein, und geben sie stattdessen einen Namen ein. Um sicherzustellen, dass ein Formular ordnungsgemäß ausgefüllt wurde, bevor Sie sie verarbeiten, überprüfen Sie die Eingabe des Benutzers.

Diese Prozedur zeigt, wie Sie alle drei Felder Sie sicherstellen, dass der Benutzer diese leer lassen nicht zu überprüfen. Sie stellen Sie auch sicher, dass der Wert für die Mitarbeiter eine Zahl ist. Wenn Fehler vorhanden sind, zeigen Sie Fehler Nachricht, die dem Benutzer, welche Werte mitteilt wurde nicht die Überprüfung bestanden.

1. In der *Form.cshtml* Datei, ersetzen Sie den ersten Block des Codes, mit dem folgenden Code: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Um Benutzereingaben zu validieren, verwenden Sie die `Validation` Helper. Sie registrieren erforderlichen Felder aus, durch den Aufruf `Validation.RequireField`. Registrieren Sie andere Arten der Validierung, durch den Aufruf `Validation.Add` und gibt das Feld aus, um zu überprüfen und den Typ der durchzuführenden Überprüfung.

    Wenn die Seite ausgeführt wird, führt ASP.NET alle die Überprüfung für Sie. Sehen Sie die Ergebnisse durch Aufrufen von `Validation.IsValid`, dem gibt true zurück, wenn alle Elemente übergeben und "false", wenn ein Überprüfungsfehler für jedes Feld. Rufen Sie in der Regel `Validation.IsValid` vor dem Ausführen von Verarbeitung auf der Benutzereingabe.
2. Update der `<body>` Elements durch Hinzufügen von drei Aufrufe an die `Html.ValidationMessage` -Methode wie folgt:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Um Überprüfungsfehlermeldungen anzuzeigen, können Sie Html aufrufen.`ValidationMessage` und übergeben sie den Namen des Felds, die die Nachricht für soll.
3. Führen Sie die Seite. Die Felder leer lassen, und klicken Sie auf **senden**. Sie können Fehlermeldungen angezeigt.

    ![Screenshot mit Fehlermeldungen angezeigt, wenn die Eingabe des Benutzers die Überprüfung nicht besteht.](4-working-with-forms/_static/image3.jpg)
4. Eine Zeichenfolge (z. B. "ABC") hinzufügen, um die **Anzahl der Mitarbeiter** ein, und klicken Sie auf **senden** erneut. Dieses Mal ein Fehler angezeigt, der angibt, die nicht der Zeichenfolge im richtigen Format, d. h., eine ganze Zahl.

    ![Screenshot mit Fehlermeldungen angezeigt, wenn der Benutzer eine Zeichenfolge für das Feld "Mitarbeiter" eingeben.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages bietet mehr Optionen für die Überprüfung von Benutzereingaben, einschließlich der Möglichkeit, automatisch unter Verwendung eines Clientskripts, Validierung, damit Benutzer ein unmittelbares Feedback im Browser abrufen. Finden Sie unter den [Zusatzressourcen](#Additional_Resources) Abschnitt weiter unten finden Sie weitere Informationen.

## <a name="restoring-form-values-after-postbacks"></a>Wiederherstellen von Formularwerte nach Postbacks

Wenn Sie die Seite im vorherigen Abschnitt getestet haben, möglicherweise, die haben Sie bemerkt Wenn Sie einen Validierungsfehler mussten, alles, was Sie eingegeben haben (nicht nur die ungültigen Daten) nicht mehr vorhanden wurde und mussten Sie Werte für alle Felder erneut eingeben. Dies ist einen wichtigen Punkt: Wenn Sie eine Seite übermitteln, verarbeiten und dann die Seite erneut rendern, wird die Seite von Grund auf neu erstellt. Wie Sie gesehen haben, bedeutet dies, dass alle Werte, die auf der Seite wurden, wenn er übermittelt wurde, nicht mehr vorhanden sind.

Sie können dies problemlos, jedoch beheben. Sie haben Zugriff auf die Werte, die gesendet wurden (in der `Request.Form` Objekt, damit Sie diese Werte wieder in die Felder ausfüllen können, wenn die Seite gerendert wird.

1. In der *Form.cshtml* Datei, ersetzen Sie die `value` Attribute der `<input>` Elemente mithilfe der `value` Attribut.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    Die `value` Attribut der `<input>` Elemente festgelegt wurde, den Wert des Felds von dynamisch zu lesen der `Request.Form` Objekt. Beim ersten, die die Seite angefordert wird, die Werte in der `Request.Form` Objekt sind alle leer. Dies ist in Ordnung, da auf diese Weise das Formular leer ist.
2. Die Seite in Ihrem Browser zu starten, geben Sie die Formularfelder oder diese Angaben leer lassen, und auf **senden**. Eine Seite mit den übermittelten Werten wird angezeigt.

    ![Forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [1.001 Möglichkeiten zur Eingabe von Webbenutzern zu erhalten.](https://msdn.microsoft.com/library/ms971057.aspx)
- [Verwenden von Formularen und Verarbeitung von Benutzereingaben](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Überprüfen der Benutzereingabe in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Verwenden von AutoVervollständigen in HTML-Formularen](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
