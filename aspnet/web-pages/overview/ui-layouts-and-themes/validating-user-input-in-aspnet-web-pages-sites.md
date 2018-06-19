---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Validieren von Benutzereingaben in ASP.NET Web Pages (Razor) Standorte | Microsoft Docs
author: tfitzmac
description: In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass Benutzer geben Sie gültige Informationen in HTML forms in einem Auftragsschritt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899177"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Validieren von Benutzereingaben in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass Benutzer geben Sie gültige Informationen in HTML forms an einem Standort der ASP.NET Web Pages (Razor).
> 
> Lernen Sie:
> 
> - So überprüfen, dass eine Benutzereingabe entspricht Überprüfungskriterien, die Sie definieren.
> - Wie Sie bestimmen, ob alle Überprüfungstests bestanden wurden.
> - Vorgehensweise beim Anzeigen von Validierungsfehlern (und wie diese formatiert).
> - So überprüfen Sie die Daten, die direkt von Benutzern keine.
> 
> Hierbei handelt es sich um den ASP.NET Programmierungskonzepte eingeführt, die im Artikel:
> 
> - Die `Validation` Helper.
> - Die `Html.ValidationSummary` und `Html.ValidationMessage` Methoden.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


Dieser Artikel enthält folgende Abschnitte:

- [Übersicht über die Validierung von Benutzereingaben](#Overview_of_User_Input_Validation)
- [Validieren von Benutzereingaben](#Validating_User_Input)
- [Die clientseitige Validierung hinzufügen](#Adding_Client-Side_Validation)
- [Formatieren von Validierungsfehlern](#Formatting_Validation_Errors)
- [Überprüfen von Daten, die direkt von Benutzern keine](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Übersicht über die Validierung von Benutzereingaben

Wenn Sie Benutzer zur Eingabe von Informationen auf einer Seite Fragen – z. B. in ein Formular – es ist wichtig, sicherzustellen, dass die Werte, die sie eingeben, gültig sind. Sie möchten z. B. keinem Formular zu verarbeiten, die wichtigen Informationen nicht vorhanden ist.

Wenn Benutzer in einem HTML-Formular Werte eingeben, werden die Werte, die sie eingeben, Zeichenfolgen. In vielen Fällen sind die Werte, die Sie benötigen einige andere Datentypen, wie ganze Zahlen oder Datumsangaben. Daher müssen Sie sicherstellen, dass die Werte, die Benutzer eingeben richtig in die entsprechenden Datentypen konvertiert werden können.

Sie können auch die Werte bestimmte Einschränkungen sein. Selbst wenn Benutzer ordnungsgemäß eine ganze Zahl eingeben, können z. B. müssen Sie sicherstellen, dass der Wert innerhalb eines bestimmten Bereichs liegt.

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Wichtige** Validieren von Benutzereingaben ist auch wichtig für die Sicherheit. Wenn Sie die Werte, die Benutzer in Formularen eingeben können beschränken, verringern Sie die Wahrscheinlichkeit, dass ein Benutzer einen Wert eingeben kann, der die Sicherheit Ihrer Website gefährden können.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Validieren von Benutzereingaben

In ASP.NET Web Pages 2 können Sie die `Validator` Hilfsmethode zum Testen von Benutzereingaben. Die grundlegende Vorgehensweise besteht darin, wie folgt vorgehen:

1. Bestimmen Sie, welche Elemente (Felder) Eingabe-, die Sie überprüfen möchten.

    In der Regel überprüfen Sie Werte in `<input>` Elemente in einem Formular. Allerdings ist es empfiehlt sich zu überprüfen aller Eingaben, die auch Eingaben, die eine eingeschränkte Element wie stammen eine `<select>` Liste. Dadurch wird sichergestellt, dass Benutzer umgehen die Steuerelemente auf einer Seite und ein Formulars senden nicht.
2. In den Seitencode Hinzufügen einzelner validierungsüberprüfungen für jedes Element mithilfe der Methoden der Eingabe der `Validation` Helper.

    Verwenden Sie zum Überprüfen für Pflichtfelder `Validation.RequireField(field, [error message])` (für ein einzelnes Feld) oder `Validation.RequireFields(field1, field2, ...))` (für eine Liste von Feldern). Verwenden Sie für andere Typen von Validierung, `Validation.Add(field, ValidationType)`. Für `ValidationType`, Sie können diese Optionen verwenden:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Wenn die Seite übermittelt wird, überprüfen Sie, ob Überprüfung durch die Überprüfung bestanden hat `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Wenn gültigkeitsprobleme bestehen, überspringen Sie normale Seite Verarbeitung. Beispielsweise ist der Zweck der Seite zum Aktualisieren einer Datenbank, tun nicht Sie dies, bis alle Validierungsfehler behoben wurden.
4. Wenn gültigkeitsprobleme bestehen, können Sie Fehlermeldungen im Markup der Seite anzeigen, indem Sie mit `Html.ValidationSummary` oder `Html.ValidationMessage`, oder beides.

Das folgende Beispiel zeigt eine Seite, die diese Schritte veranschaulicht.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Um zu sehen, wie die Validierung funktioniert, führen Sie diese Seite, und machen Sie Fehler bewusst. Beispielsweise sieht die Seite wie nicht vergessen, wenn Sie zur Eingabe eines Namens Kurs bei der Eingabe ein, und wenn Sie ein ungültiges Datum eingeben:

![Überprüfungsfehler in der gerenderten Seite](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Die clientseitige Validierung hinzufügen

Standardmäßig eine Benutzereingabe überprüft werden, nachdem der Benutzer die Seite senden – d. h. die Validierung in Servercode. Ein Nachteil dieses Ansatzes ist, dass Benutzer nicht wissen, dass sie Fehler erst nach dem Senden der Seite vorgenommen haben. Wenn ein Formular lang oder zu komplex ist, kann die berichtfehlern erst nach der Übermittlung der Seite unpraktisch, der Benutzer sein.

Sie können die Unterstützung zum Ausführen einer Validierung in Clientskripts hinzufügen. In diesem Fall wird die Überprüfung ausgeführt, da Benutzer im Browser arbeiten. Nehmen Sie z. B. an, dass Sie angeben, dass ein Wert eine ganze Zahl sein sollte. Wenn ein Benutzer einen nicht ganzzahlige Wert eingibt, wird der Fehler gemeldet werden, sobald der Benutzer das Feld verlässt. Benutzer erhalten unmittelbar Feedback, das für sie einfacher ist. Clientbasierte Überprüfung kann die Anzahl der Male senken, denen der Benutzer zum Senden des Formulars, um mehrere Fehler zu beheben.

> [!NOTE]
> Auch wenn Sie die clientseitige Validierung verwenden, ist Überprüfung immer auch in Servercode durchgeführt. Validierung ist im Servercode eine Sicherheitsmaßnahme dar, für den Fall, dass Benutzer clientbasierten Überprüfung zu umgehen.


1. Registrieren Sie die folgenden JavaScript-Bibliotheken auf der Seite an:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Zwei der Bibliotheken können über ein Netzwerk für Inhaltsübermittlung (CDN) geladen werden, daher ist es nicht unbedingt Komponenten auf Ihrem Computer oder Server befinden. Sie benötigen jedoch eine lokale Kopie der *jquery.validate.unobtrusive.js*. Wenn Sie nicht bereits mit einer Vorlage WebMatrix arbeiten (z. B. **Starter Site** ), die die Bibliothek enthält, erstellen Sie eine Web Pages-Website auf der Grundlage **Starter Site**. Kopieren Sie dann die *js* Datei auf Ihrer aktuellen Website.
2. Fügen Sie im Markup für jedes Element, das Sie überprüfen möchten, die einen Aufruf von `Validation.For(field)`. Diese Methode gibt die Attribute, die durch die clientseitige Validierung verwendet werden. (Statt tatsächliche JavaScript-Code ausgeben, die Methode ausgibt Attribute wie `data-val-...`. Diese Attribute unterstützen unaufdringliche Clientvalidierung, die jQuery verwendet, um die Aktionen auszuführen.)

Die folgende Seite wird gezeigt, wie im Beispiel weiter oben dargestellten clientseitige Validierung Funktionen hinzugefügt.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nicht alle Überprüfungen, die auf dem Client ausgeführt werden. Insbesondere nicht die Datentyp-Überprüfung (ganze Zahl, Datum usw.) auf dem Client ausgeführt. Die folgenden Prüfungen auf dem Client und Server arbeiten:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In diesem Beispiel funktioniert nicht, die Tests für ein gültiges Datum im Clientcode. Allerdings wird der Test im Servercode durchgeführt werden.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formatieren von Validierungsfehlern

Sie können steuern, wie Validierungsfehler angezeigt werden, durch Definieren von CSS-Klassen, die die folgenden reservierten Namen aufweisen:

- `field-validation-error` Definiert die Ausgabe von der `Html.ValidationMessage` Methode, wenn es einen Fehler angezeigt wird.
- `field-validation-valid` Definiert die Ausgabe von der `Html.ValidationMessage` Methode, wenn kein Fehler vorliegt.
- `input-validation-error` Definiert, wie `<input>` Elemente werden gerendert, wenn ein Fehler vorliegt. (Z. B. können Sie diese Klasse verwenden, zum Festlegen der Hintergrundfarbe für eine &lt;input&gt; Element in eine andere Farbe, wenn der Wert ungültig ist.) Diese CSS-Klasse wird nur während der Clientvalidierung (in ASP.NET Web Pages 2) verwendet.
- `input-validation-valid` Definiert die Darstellung von `<input>` Elemente angezeigt, wenn kein Fehler vorliegt.
- `validation-summary-errors` Definiert die Ausgabe von der `Html.ValidationSummary` Methode, die eine Liste von Fehlern anzeigt.
- `validation-summary-valid` Definiert die Ausgabe von der `Html.ValidationSummary` Methode, wenn kein Fehler vorliegt.

Die folgenden `<style>` Block werden Regeln für fehlerbedingungen.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Wenn Sie diese Stilblock in der Beispielseiten weiter oben im Artikel enthalten, wird die Anzeige wie in der folgenden Abbildung aussehen:

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Wenn Sie nicht die Clientvalidierung in ASP.NET Web Pages 2 verwenden, die CSS-Klassen für die `<input>` Elemente (`input-validation-error` und `input-validation-valid` haben Sie keine Auswirkung.


### <a name="static-and-dynamic-error-display"></a>Statische und dynamische Fehleranzeige

Die CSS-Regeln sind einander paarweise zugeordnet, wie z. B. `validation-summary-errors` und `validation-summary-valid`. Diese Paare können Sie die Regeln für beide Bedingungen definieren: eine fehlerbedingung und eine "normale" (nicht-Fehler)-Bedingung. Es ist wichtig zu verstehen, dass das Markup für die Anzeige immer gerendert wird, auch wenn keine Fehler vorliegen. Angenommen, eine Seite hat eine `Html.ValidationSummary` Methode im Markup Quellcode für die Seite enthält das folgende Markup auch, wenn die Seite zum ersten Mal angefordert wird:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Das heißt, die `Html.ValidationSummary` Methode rendert immer eine `<div>` Element- und eine Liste, auch wenn die Fehlerliste leer ist. Auf ähnliche Weise die `Html.ValidationMessage` Methode rendert immer eine `<span>` Element als Platzhalter für ein einzelnes Feld Fehler, auch wenn kein Fehler vorliegt.

In einigen Situationen können dazu führen, dass die Seite Umbruch eine Fehlermeldung angezeigt und können dazu führen, dass Elemente auf der Seite zu navigieren. Die CSS-Regeln, die auf Enden `-valid` können Sie ein Layout zu definieren, mit deren Hilfe kann dieses Problem zu vermeiden. Sie können z. B. definieren `field-validation-error` und `field-validation-valid` sowohl haben die gleiche Größe behoben. Auf diese Weise der Anzeigebereich für das Feld ist statisch und wird nicht den Seite Datenfluss ändern, wenn eine Fehlermeldung angezeigt wird.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Überprüfen von Daten, die direkt von Benutzern keine

In einigen Fällen müssen Sie Informationen zu überprüfen, die direkt aus einem HTML-Formular stammen nicht. Ein typisches Beispiel ist eine Seite, in dem ein Wert in einer Abfragezeichenfolge, wie im folgenden Beispiel übergeben wird:

`http://server/myapp/EditClassInformation?classid=1022`

In diesem Fall soll sicherstellen, dass der Wert, der an die Seite übergeben wird (hier 1022 für den Wert des `classid`) ist ungültig. Sie können nicht direkt verwenden die `Validation` Hilfsmethode zum Ausführen dieser Überprüfung. Allerdings können Sie andere Funktionen des Systems Validierung, z. B. die Fähigkeit zum Anzeigen von Überprüfungsfehlermeldungen.

> [!NOTE] 
> 
> **Wichtige** überprüfen Sie immer alle Werte, die Sie aus abrufen *alle* Quellcode, einschließlich Formularfeld Werte, Abfragezeichenfolgenwerte und Cookiewerte. Es ist einfach für Personen, die diese Werte (z. B. für böswillige Zwecke) zu ändern. Daher müssen Sie diese Werte überprüfen, um die Anwendung zu schützen.


Das folgende Beispiel zeigt, wie Sie einen Wert überprüfen können, der in einer Abfragezeichenfolge übergeben wird. Der Code überprüft, dass der Wert nicht leer ist und dass es sich um eine ganze Zahl handelt.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Beachten Sie, dass der Test ausgeführt wird, wenn die Anforderung keiner Formularübergabe ist (`if(!IsPost)`). Dieser Test würde der ersten, der die Seite angefordert wird übergeben, aber nicht bei die Anforderung ist ein Formularübermittlung.

Um diesen Fehler anzuzeigen, können Sie den Fehler zur Liste der Validierungsfehler hinzufügen, durch den Aufruf `Validation.AddFormError("message")`. Wenn die Seite mit einen Aufruf von enthält die `Html.ValidationSummary` Methode, die Fehler vorhanden ist, wie Benutzereingaben Überprüfung ein Fehler angezeigt.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Arbeiten mit HTML-Formularen in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkID=202892)
