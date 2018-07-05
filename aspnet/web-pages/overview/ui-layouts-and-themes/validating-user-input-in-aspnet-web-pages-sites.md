---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Überprüfen der Benutzereingabe in der ASP.NET Web Pages (Razor) Sites | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass die Benutzer geben Sie gültige Informationen in HTML forms in einem Auftragsschritt...
ms.author: aspnetcontent
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: d412f3fa4ca144a8a9107c971279f7bf2663cfe5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819261"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Überprüfen der Benutzereingabe in ASP.NET Web Pages (Razor)-Websites
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Informationen zu überprüfen, Sie von Benutzern erhalten &mdash; , also stellen Sie sicher, dass die Benutzer geben Sie gültige Informationen in HTML forms in einer ASP.NET Web Pages (Razor)-Website.
> 
> Sie lernen Folgendes:
> 
> - So prüfen Sie, dass eine Benutzereingaben entspricht Überprüfungskriterien, die Sie definieren.
> - Informationen zum bestimmen, ob alle Überprüfungstests bestanden haben.
> - Vorgehensweise beim Anzeigen von Validierungsfehlern (und wie sie formatiert).
> - So überprüfen Sie die Daten, die direkt von Benutzern keine.
> 
> Hierbei handelt es sich um das ASP.NET-PROGRAMMIERMODELL in diesem Artikel vorgestellten Konzepten:
> 
> - Die `Validation` Helper.
> - Die `Html.ValidationSummary` und `Html.ValidationMessage` Methoden.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


Dieser Artikel enthält folgende Abschnitte:

- [Übersicht über die Validierung von Benutzereingaben](#Overview_of_User_Input_Validation)
- [Überprüfen der Benutzereingabe](#Validating_User_Input)
- [Die clientseitige Validierung hinzufügen](#Adding_Client-Side_Validation)
- [Formatieren von Validierungsfehlern](#Formatting_Validation_Errors)
- [Überprüfen von Daten, die direkt von Benutzern keine](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Übersicht über die Validierung von Benutzereingaben

Wenn Sie Benutzer zur Eingabe von Informationen auf einer Seite anfordern, z. B. in einem Formular – es ist wichtig, sicherzustellen, dass die Werte, die sie eingeben gültig sind. Sie möchten z. B. keine Form zu verarbeiten, die wichtigen Informationen fehlen.

Wenn der Benutzer Werte in einem HTML-Formular eingeben, sind die Werte, die sie eingeben Zeichenfolgen. In vielen Fällen sind die Werte, die Sie benötigen einige andere Datentypen, z. B. ganze Zahlen oder Datumsangaben. Aus diesem Grund müssen Sie auch sicherstellen, dass die Werte, die Benutzer geben Sie ordnungsgemäß in die entsprechenden Datentypen konvertiert werden können.

Sie können auch bestimmte Einschränkungen für die Werte verfügen. Selbst wenn Benutzer ordnungsgemäß eine ganze Zahl eingeben, können beispielsweise müssen Sie sicherstellen, dass der Wert innerhalb eines bestimmten Bereichs liegt.

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Wichtige** Validieren von Benutzereingaben ist auch wichtig für die Sicherheit. Wenn Sie die Werte, die Benutzer in Formularen eingeben können beschränken, verringern Sie die Wahrscheinlichkeit, dass ein Benutzer einen Wert eingeben kann, der die Sicherheit Ihrer Website gefährden können.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Überprüfen der Benutzereingabe

In ASP.NET Web Pages 2 können Sie die `Validator` Hilfsmethode zum Testen der Benutzereingabe. Grundsätzlich wird die folgenden Schritte ausführen:

1. Bestimmen Sie, welche Elemente (Felder) Eingabe-, die Sie überprüfen möchten.

    Überprüfen Sie in der Regel Werte, die in `<input>` Elemente in einem Formular. Allerdings ist es empfiehlt sich, alle Eingaben überprüfen, geben Sie auch, das von einem eingeschränkten Element wie ist eine `<select>` Liste. Dadurch wird sichergestellt, dass Benutzer nicht umgehen die Steuerelemente auf einer Seite und senden Sie ein Formular.
2. Fügen Sie einzelne Überprüfungen im Seitencode, für jedes Element mithilfe der Methoden der Eingabe der `Validation` Helper.

    Verwenden Sie zum Überprüfen erforderlichen Felder `Validation.RequireField(field, [error message])` (für ein einzelnes Feld) oder `Validation.RequireFields(field1, field2, ...))` (für eine Liste von Feldern). Verwenden Sie für andere Arten der Validierung, `Validation.Add(field, ValidationType)`. Für `ValidationType`, Sie können diese Optionen verwenden:

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
3. Wenn die Seite gesendet wird, überprüfen Sie, ob Überprüfung, indem Sie überprüfen bestanden hat `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Treten Validierungsfehler auf, überspringen Sie die normale Verarbeitung. Beispielsweise ist der Zweck der Seite zum Aktualisieren einer Datenbank, tun nicht Sie dies, bis alle Validierungsfehler behoben wurden.
4. Wenn Validierungsfehler vorhanden sind, können Sie Fehlermeldungen in das Markup der Seite anzeigen, indem Sie mithilfe von `Html.ValidationSummary` oder `Html.ValidationMessage`, oder beides.

Das folgende Beispiel zeigt eine Seite, die diese Schritte veranschaulicht.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Um die Funktionsweise der Validierung angezeigt wird, führen Sie diese Seite, und machen Sie bewusst Fehler. Beispielsweise sieht die Seite wie nicht vergessen, wenn Sie zur Eingabe eines Namens Kurs bei der Eingabe ein, und wenn Sie ein ungültiges Datum eingeben:

![Überprüfungsfehler in der gerenderten Seite](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Die clientseitige Validierung hinzufügen

Standardmäßig wird der Benutzereingabe überprüft, nachdem der Benutzer die Seite senden – d. h. die Validierung wird im Servercode durchgeführt. Ein Nachteil dieses Ansatzes ist, dass Benutzer nicht wissen, dass sie einen Fehler erst nach dem sie die Seite senden vorgenommen wurden. Wenn ein Formular lang oder zu komplex ist, kann Fehler melden, erst nach der Übermittlung der Seite für den Benutzer unpraktisch.

Sie können den Support, um die Validierung im Clientskript hinzufügen. In diesem Fall wird die Überprüfung ausgeführt, während die Benutzer im Browser arbeiten. Nehmen wir beispielsweise an, dass Sie angeben, dass ein Wert eine ganze Zahl sein muss. Wenn ein Benutzer einen nicht ganzzahligen Wert eingibt, wird der Fehler gemeldet, sobald der Benutzer das Feld lässt. Benutzer erhalten unmittelbar Feedback, das für sie zweckmäßig ist. Client-basierter Validierung kann auch die Anzahl der reduzieren, denen der Benutzer zum Senden des Formulars, um mehrere Fehler zu beheben.

> [!NOTE]
> Auch wenn Sie die clientseitige Validierung verwenden, ist die Überprüfung immer auch im Servercode ausgeführt. Ausführen der Überprüfung im Server-Code ist eine Sicherheitsmaßnahme, für den Fall, dass Benutzer, Client-basierten Überprüfung umgehen.


1. Registrieren Sie die folgenden JavaScript-Bibliotheken auf der Seite an:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Zwei Bibliotheken sind aus der ein Content Delivery Network (CDN) geladen werden kann, müssen Sie unbedingt nicht damit sie auf Ihrem Computer oder Server. Allerdings benötigen Sie eine lokale Kopie des *jquery.validate.unobtrusive.js*. Wenn Sie nicht bereits mit einer WebMatrix-Vorlage arbeiten (z. B. **Starter Site** ), die die Bibliothek enthält, erstellen Sie eine Web Pages-Website auf der Grundlage **Starter Site**. Kopieren Sie dann die *js* Datei, die der aktuellen Website.
2. Fügen Sie im Markup für jedes Element, das Sie überprüfen können, die einen Aufruf von `Validation.For(field)`. Diese Methode gibt die Attribute, die durch die clientseitige Validierung verwendet werden. (Statt der tatsächlichen JavaScript-Code ausgegeben werden, gibt die Methode Attribute wie `data-val-...`. Diese Attribute unterstützen unaufdringlichen Validierung, die jQuery verwendet, um die Arbeit zu erledigen.)

Die folgende Seite zeigt wie das oben gezeigte Beispiel Validierungsfunktionen Client hinzugefügt.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Nicht alle Überprüfungen, die auf dem Client ausgeführt werden. Insbesondere nicht die Datentyp-Überprüfung (ganze Zahl, Datum usw.) auf dem Client ausgeführt. Die folgenden Prüfungen auf dem Client und Server arbeiten:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In diesem Beispiel funktioniert nicht der Test für ein gültiges Datum im Clientcode. Der Test wird jedoch im Server-Code ausgeführt werden.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formatieren von Validierungsfehlern

Sie können steuern, wie Validierungsfehler angezeigt werden, durch Definieren von CSS-Klassen, die die folgenden reservierten Namen aufweisen:

- `field-validation-error` Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn es einen Fehler angezeigt wird.
- `field-validation-valid` Definiert die Ausgabe der `Html.ValidationMessage` Methode, wenn kein Fehler vorliegt.
- `input-validation-error` Definiert, wie `<input>` Elemente werden gerendert, wenn ein Fehler auftritt. (Z. B. können diese Klasse, legen Sie die Farbe des Hintergrunds einer &lt;Eingabe&gt; Element in einer anderen Farbe, wenn der Wert ungültig ist.) Diese CSS-Klasse wird nur während der Validierung (in ASP.NET Web Pages 2) verwendet.
- `input-validation-valid` Definiert die Darstellung der `<input>` Elemente, wenn kein Fehler vorliegt.
- `validation-summary-errors` Definiert die Ausgabe der `Html.ValidationSummary` Methode, die sie eine Liste der Fehler anzeigt.
- `validation-summary-valid` Definiert die Ausgabe der `Html.ValidationSummary` Methode, wenn kein Fehler vorliegt.

Die folgenden `<style>` Block Regeln für fehlerbedingungen angezeigt.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Wenn Sie auf den Beispielseiten von weiter oben im Artikel dieses Stilblock einschließen, wird die Anzeige wie in der folgenden Abbildung aussehen:

![Validierungsfehler, die CSS-Formatklassen verwenden](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Wenn Sie nicht die Clientvalidierung in ASP.NET Web Pages 2 verwenden, die CSS-Klassen für die `<input>` Elemente (`input-validation-error` und `input-validation-valid` haben keinerlei Auswirkung.


### <a name="static-and-dynamic-error-display"></a>Statische und dynamische Fehleranzeige

Die CSS-Regeln paarweise, z. B. `validation-summary-errors` und `validation-summary-valid`. Diese Paare können Sie die Regeln für beide Bedingungen definieren: eine fehlerbedingung und eine "normale" (nicht-Fehler)-Bedingung. Es ist wichtig zu verstehen, dass immer das Markup für die Fehleranzeige gerendert wird, auch wenn keine Fehler vorliegen. Wenn eine Seite hat z. B. eine `Html.ValidationSummary` -Methode in das Markup, den Quellcode der Seite enthält das folgende Markup auch, wenn die Seite zum ersten Mal angefordert wird:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Das heißt, die `Html.ValidationSummary` Methode stellt immer eine `<div>` Element und eine Liste, auch wenn die Fehlerliste leer ist. Auf ähnliche Weise die `Html.ValidationMessage` Methode stellt immer eine `<span>` Element als Platzhalter für ein einzelnes feldfehler, auch wenn kein Fehler vorliegt.

In einigen Situationen können dazu führen, dass die Seite, um dynamisch umgebrochen eine Fehlermeldung angezeigt und können dazu führen, dass Elemente auf der Seite verschieben. Die CSS-Regeln, die auf Enden `-valid` können Sie ein Layout zu definieren, mit deren Hilfe kann dieses Problem zu vermeiden. Sie können z. B. definieren `field-validation-error` und `field-validation-valid` sowohl haben die gleiche feste Größe. Auf diese Weise der Anzeigebereich für das Feld ist statisch und die Seitenfluss wird nicht geändert werden, wenn eine Fehlermeldung angezeigt wird.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Überprüfen von Daten, die direkt von Benutzern keine

Manchmal müssen Sie an, die direkt von einem HTML-Formular keine Informationen zu überprüfen. Ein typisches Beispiel ist eine Seite, in dem ein Wert in einer Abfragezeichenfolge, wie im folgenden Beispiel übergeben wird:

`http://server/myapp/EditClassInformation?classid=1022`

In diesem Fall soll sicherstellen, dass der Wert, der auf der Seite übergeben wird (hier 1022 für den Wert des `classid`) gültig ist. Sie können nicht direkt verwenden die `Validation` Hilfsprogramm zum Ausführen dieser Validierung. Allerdings können Sie andere Funktionen des Systems Validierung, z.B. die Möglichkeit, die Überprüfungsfehlermeldungen anzuzeigen.

> [!NOTE] 
> 
> **Wichtige** immer überprüfen von Werten, die Sie von erhalten *alle* Quellcode, einschließlich Formularfeld Werte, Abfragezeichenfolgen-Werte und Cookiewerte. Es ist einfach, Personen diese Werte (z. B. für böswillige Zwecke) zu ändern. Daher müssen Sie diese Werte prüfen, um Ihre Anwendung zu schützen.


Das folgende Beispiel zeigt, wie Sie einen Wert überprüfen können, der in einer Abfragezeichenfolge übergeben wird. Der Code überprüft, dass der Wert nicht leer ist und es sich um eine ganze Zahl ist.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Beachten Sie, dass der Test ausgeführt wird, wenn die Anforderung keiner Formularübergabe (`if(!IsPost)`). Dieser Test übergeben beim ersten, die die Seite angefordert wird, aber nicht bei die Anforderung ist eine formularübertragung.

Um diesen Fehler anzuzeigen, können Sie den Fehler zur Liste der Validierungsfehler hinzufügen, durch den Aufruf `Validation.AddFormError("message")`. Wenn die Seite mit einen Aufruf von enthält die `Html.ValidationSummary` -Methode, der Fehler wird angezeigt, genau wie einen Fehler für die Validierung von Benutzereingaben.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Arbeiten mit HTML-Formularen in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkID=202892)
