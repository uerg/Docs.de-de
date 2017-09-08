---
title: Tag-Hilfsprogramme in ASP.NET Core erstellen
author: rick-anderson
description: "Erfahren Sie mehr über das Tag-Hilfsprogramme in ASP.NET Core erstellen."
keywords: ASP.NET Core, Tag-Hilfsprogramme
ms.author: riande
manager: wpickett
ms.date: 6/14/2017
ms.topic: article
ms.assetid: 4f16d978-5695-4abf-a785-fdaabf3bbcb9
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f16af1184a29b891a9aab0b38ab833836c326c44
ms.sourcegitcommit: e6a8f171f26fab1b2195a2d7f14e7d258a2e690e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/23/2017
---
# <a name="authoring-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>Erstellen von Tag-Hilfsprogramme in ASP.NET Core, eine exemplarische Vorgehensweise mit Beispielen

Durch [Rick Anderson](https://twitter.com/RickAndMSFT)

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample)

## <a name="getting-started-with-tag-helpers"></a>Erste Schritte mit dem Tag-Hilfsprogramme

Dieses Lernprogramm enthält eine Einführung in die Programmierung Tag-Hilfsprogramme. [Einführung in die Tag-Hilfsprogrammen](intro.md) beschreibt die Vorteile, die Tag-Hilfsprogramme bereitstellen.

Ein Tag-Hilfsprogramm ist jede Klasse, implementiert die `ITagHelper` Schnittstelle. Jedoch wenn Sie eine Tag Hilfsprogramm erstellen, in der Regel von abzuleiten, `TagHelper`, tun dies der Fall ist ermöglicht Ihnen Zugriff auf die `Process` Methode.

1. Erstellen Sie ein neues ASP.NET Core-Projekt namens **AuthoringTagHelpers**. Sie brauchen nicht Authentifizierung für dieses Projekt.

2. Erstellen Sie einen Ordner zum Speichern von Tag Hilfsprogramme aufgerufen *TagHelpers*. Die *TagHelpers* Ordner *nicht* erforderlich, aber es ist eine angemessene Konvention. Nun beginnen wir zunächst einige einfache Tag Hilfsmethoden schreiben.

## <a name="a-minimal-tag-helper"></a>Ein minimaler Tag-Hilfsprogramm

In diesem Abschnitt schreiben Sie eine Tag-Hilfsprogramm, die eine e-Mail-Tag aktualisiert. Zum Beispiel:

```html
<email>Support</email>
   ```

Der Server wird unserer e-Mail-Tag-Hilfsprogramm verwenden, konvertieren Sie das Markup in der folgenden:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
   ```

D. h., dass dadurch ein Ankertag, die einen e-Mail-Link. Möglicherweise möchten vorgehen, wenn Sie eine Blog-Engine schreiben und Sie ihn benötigen, um das Senden von e-Mails für Marketing, Support und andere Kontaktpersonen alle an der gleichen Domäne.

1.  Fügen Sie die folgenden `EmailTagHelper` Klasse, um die *TagHelpers* Ordner.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Hinweise:**
    
    * Tag-Hilfen verwenden eine Benennungskonvention, die auf Elemente des Klassennamens Stamm (abzüglich der *taghelpers* Teil des Namens der Klasse). In diesem Beispiel den Namen des **E-Mail**taghelpers ist *e-Mail*, sodass die `<email>` Tag wird als Ziel haben. Diese Namenskonvention sollte für die meisten Tag Hilfsprogramme funktionieren, später werde ich zeigen, wie zu überschreiben.
    
    * Die `EmailTagHelper`-Klasse wird von `TagHelper` abgeleitet. Die `TagHelper` Klasse stellt Methoden und Eigenschaften für das Schreiben von Hilfsprogrammen Tag.
    
    * Die überschriebene `Process` Methode steuert Wirkungsweise das Tag-Hilfsobjekt, bei der Ausführung. Die `TagHelper` Klasse bietet auch eine asynchrone Version (`ProcessAsync`) mit denselben Parametern.
    
    * Der Kontextparameter zu `Process` (und `ProcessAsync`) enthält Informationen, die die Ausführung des aktuellen HTML-Tags zugeordnet.
    
    * Die Output-Parameter `Process` (und `ProcessAsync`) enthält eine zustandsbehaftete HTML-Element, die repräsentativ für die ursprüngliche Quelle, die zum Generieren einer HTML-Tags und Inhalt verwendet.
    
    * Unsere Klassenname wurde als Suffix **taghelpers**, also *nicht* erforderlich, aber es wurde eine best Practice-Konvention angesehen. Sie können die Klasse als deklarieren:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Vornehmen der `EmailTagHelper` Klasse für alle Razor-Ansichten verfügbar, fügen Sie der `addTagHelper` -Direktive die *Views/_ViewImports.cshtml* Datei: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Der obige Code verwendet die Platzhaltersyntax, um anzugeben, dass die Tag-Hilfsprogramme in unsere Assembly zur Verfügung stehen. Die erste Zeichenfolge nach `@addTagHelper` gibt die Tag-Hilfsmethode zum Laden (Verwendung "*" für alle Tags Hilfsprogramme), und die zweite Zeichenfolge "AuthoringTagHelpers" gibt die Assembly, die das Tag-Hilfsprogramm im ist. Beachten Sie, dass die zweite Zeile in die Core ASP.NET-MVC Tag-Hilfsprogramme, die mit der Platzhaltersyntax Schaltet (diese Hilfsprogramme werden im erläutert [Einführung in die Tag-Hilfsprogrammen](intro.md).) Es ist die `@addTagHelper` -Direktive, die die Tag-Hilfsprogramm an der Razor-Ansicht zur Verfügung stellt. Alternativ können Sie den voll gekennzeichneten Namen (FQN) der wie folgt ein Tag-Hilfsprogramm bereitstellen:
    
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
    
    Zum Hinzufügen eines Tag-Hilfsprogramms zu einer Ansicht mithilfe einer FQN Sie zunächst die FQN hinzufügen (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), und klicken Sie dann den Assemblynamen (*AuthoringTagHelpers*). Die Platzhaltersyntax verwenden, werden die meisten Entwickler bevorzugen. [Einführung in die Tag-Hilfsprogrammen](intro.md) im Detail auf Helper hinzufügen, entfernen, Hierarchie- und Platzhalter Tagsyntax wird.
    
3.  Aktualisieren Sie das Markup in der *Views/Home/Contact.cshtml* Datei mit diesen Änderungen:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Führen Sie die app und Ihrem bevorzugten Browser verwenden, der HTML-Quelle an, damit Sie überprüfen können, dass die e-Mail-Tags mit Anker Markup ersetzt werden (z. B. `<a>Support</a>`). *Unterstützung* und *Marketing* werden als Links, gerendert, jedoch nicht über ein `href` Attribut, um funktionsfähig zu machen. Wir beheben, die im nächsten Abschnitt.

Hinweis: Wie HTML-Tags und Attribute, Tags, Klassennamen und Attribute in Razor und c# sind keine Groß-/Kleinschreibung beachtet.

## <a name="setattribute-and-setcontent"></a>SetAttribute und SetContent

Wir werden in diesem Abschnitt Aktualisieren der `EmailTagHelper` , damit eine gültige Ankertag zum Abrufen von e-Mails erstellt wird. Wir werden aktualisieren, um die Daten aus einer Razor-Ansicht werden (in Form einer `mail-to` Attribut) und verwenden, die beim Generieren des Ankers.

Update der `EmailTagHelper` Klasse durch Folgendes:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Hinweise:**

* In Pascal-Schreibweise angegeben Klassen- und Eigenschaftennamen für Tag Hilfsprogramme übersetzt ihre [senken Kebab Groß-/Kleinschreibung](http://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101#12273101). Aus diesem Grund verwendet der `MailTo` -Attribut, verwenden Sie `<email mail-to="value"/>` entspricht.

* Die letzte Zeile legt die abgeschlossenen für unsere minimal funktionale Tag-Hilfsprogramm.

* Die hervorgehobene Zeile zeigt die Syntax für das Hinzufügen von Attributen:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Dieser Ansatz funktioniert für das Attribut "Href" so lange nicht derzeit in der attributauflistung vorhanden. Sie können auch die `output.Attributes.Add` Methode, um eine taghilfsattribut bis zum Ende der Auflistung von Tagattribute hinzuzufügen.

1.  Aktualisieren Sie das Markup in der *Views/Home/Contact.cshtml* Datei mit diesen Änderungen: [!code-html [Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Führen Sie die app, und stellen Sie sicher, dass sie die richtige Links generiert.
    
    > [!NOTE]
    >Wenn Sie die e-Mail-Tag selbst schließende geschrieben wurden (`<email mail-to="Rick" />`), das endgültige Ergebnis wäre auch selbstschließende. So aktivieren Sie die Möglichkeit zum Schreiben der Transponder mit nur einem Starttag (`<email mail-to="Rick">`) versehen Sie die Klasse durch Folgendes:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Mit der ein selbstschließende e-Mail-Tag-Hilfsprogramm, wäre die Ausgabe `<a href="mailto:Rick@contoso.com" />`. Selbstschließende Ankertags sind nicht gültig HTML, empfiehlt es sich um ein Tag-Hilfsprogramm zu erstellen, die selbstschließende ist, sodass Sie nicht empfehlenswert, um eines zu erstellen. Tag-Hilfsprogrammen legen Sie den Typ des der `TagMode` Eigenschaft nach dem Lesen eines Tags.
    
### <a name="processasync"></a>ProcessAsync

In diesem Abschnitt schreiben wir eine asynchrone-e-Mail-Hilfe.

1.  Ersetzen Sie die `EmailTagHelper` Klasse durch den folgenden Code:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Hinweise:**

    * Diese Version verwendet die asynchrone `ProcessAsync` Methode. Der asynchrone `GetChildContentAsync` gibt eine `Task` , enthält die `TagHelperContent`.

    * Verwenden der `output` Parameter beim Abrufen von Inhalten des HTML-Elements.

2.  Nehmen Sie die folgende Änderung an der *Views/Home/Contact.cshtml* Datei, sodass das Tag-Hilfsobjekt, der Ziel-e-Mail abrufen kann.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Führen Sie die app, und stellen Sie sicher, dass sie gültige e-Mail-Links generiert.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent und PostContent.SetHtmlContent

1.  Fügen Sie die folgenden `BoldTagHelper` Klasse, um die *TagHelpers* Ordner.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Hinweise:**
    
    * Die `[HtmlTargetElement]` -Attribut übergibt ein Attributparameter, der angibt, dass jeder HTML-Element, das ein HTML‑Attribut enthält, mit dem Namen "bold" entspricht, und die `Process` Überschreibungsmethode in der Klasse ausgeführt wird. In diesem Beispiel die `Process` Methode entfernt das Attribut "Fett" und schließt das Markup enthält, mit dem `<strong></strong>`.
    
    * Da Sie nicht den vorhandenen Inhalt Tags ersetzen möchten, müssen Sie das öffnende schreiben `<strong>` tag mit der `PreContent.SetHtmlContent` -Methode und das schließende `</strong>` tag mit der `PostContent.SetHtmlContent` Methode.
    
2.  Ändern der *About.cshtml* anzeigen enthalten einen `bold` Attributwert. Der vollständige Code wird unten gezeigt.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Führen Sie die app an. Sie können Ihrem bevorzugten Browser verwenden, überprüfen die Quelle, und überprüfen das Markup.

    Die `[HtmlTargetElement]` Attribut abzielt nur HTML-Markup, das einen Attributnamen an, der "Fett" bereitstellt. Die `<bold>` Element durch das Hilfsprogramm Tag nicht geändert wurde.

4. Kommentieren Sie Sie aus der `[HtmlTargetElement]` Attributzeile auf, und es werden standardmäßig als Ziel `<bold>` Tags, d. h. HTML-Markup des Formulars `<bold>`. Beachten Sie, dass die Dateinamenskonvention entspricht der Name der Klasse **fett**taghelpers um `<bold>` Tags.

5. Die app auszuführen, und überprüfen Sie, ob die `<bold>` Tag durch die Tag-Hilfsprogramm verarbeitet wird.

Indem eine Klasse mit mehreren `[HtmlTargetElement]` Attribute führt eine logische OR-Ziele. Verwenden den folgenden Code, wird z. B. die fett-Tag oder fett-Attribut entspricht.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Wenn mehrere Attribute der gleichen Anweisung hinzugefügt werden, behandelt die Common Language Runtime diese als eine logische and Beispielsweise in den folgenden Code ein HTML-Element muss den Namen "bold" mit einem Attribut mit dem Namen "bold" (`<bold bold />`) übereinstimmen.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Sie können auch die `[HtmlTargetElement]` so ändern Sie den Namen des entsprechenden Elements. Angenommen, Sie möchten die `BoldTagHelper` Ziel `<MyBold>` Tags, verwenden Sie das folgende Attribut:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="passing-a-model-to-a-tag-helper"></a>Übergeben ein Modell in einem Tag-Hilfsprogramm

1.  Hinzufügen einer *Modelle* Ordner.

2.  Fügen Sie die folgenden `WebsiteContext` Klasse, um die *Modelle* Ordner:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Fügen Sie die folgenden `WebsiteInformationTagHelper` Klasse, um die *TagHelpers* Ordner.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Hinweise:**
    
    * Wie bereits erwähnt, Tag Hilfsprogramme übersetzt Pascal-Schreibweise verwendet C#-Klassennamen und Eigenschaften für den Tag-Hilfsprogramme in [senken Kebab Groß-/Kleinschreibung](http://c2.com/cgi/wiki?KebabCase). Aus diesem Grund verwendet der `WebsiteInformationTagHelper` in Razor, die Sie schreiben `<website-information />`.
    
    * Das Zielelement mit explizit nicht identifiziert werden die `[HtmlTargetElement]` Attribut, sodass die Standardeinstellung von `website-information` ist für die. Wenn Sie das folgende Attribut (wobei es handelt es sich nicht um Kebab Groß-/Kleinschreibung jedoch stimmt mit dem Klassennamen) angewendet haben:
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Der untere Kebab Kiste `<website-information />` stimmen nicht überein. Wenn Sie möchten die `[HtmlTargetElement]` -Attribut, verwenden Sie Kebab Groß-/Kleinschreibung wie unten dargestellt:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Elemente, die selbstschließende sind haben keinen Inhalt. In diesem Beispiel wird das Markup Razor ein selbstschließendes Tags verwenden, aber das Tag-Hilfsobjekt erstellen eine [Abschnitt](http://www.w3.org/TR/html5/sections.html#the-section-element) Element (nicht selbstschließende ist und Sie werden das Schreiben von Inhalt in die `section` Element). Aus diesem Grund müssen Sie festlegen `TagMode` auf `StartTagAndEndTag` Ausgabe zu schreiben. Sie können alternativ die Einstellung für die Zeile auskommentieren `TagMode` und Schreiben von Markup mit dem ein Endtag. (Beispielmarkup wird weiter unten in diesem Lernprogramm bereitgestellt.)
    
    * Die `$` (Dollarzeichen) in der folgenden Zeile verwendet ein [interpoliert Zeichenfolge](https://msdn.microsoft.com/library/Dn961160.aspx):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Fügen Sie das folgende Markup zum Rendern der *About.cshtml* anzeigen. Das hervorgehobene Markup zeigt die Website-Informationen.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > Im unten gezeigten Razor-Markup:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor weiß der `info` Attribut ist eine Klasse, die keine Zeichenfolge, und C#-Code geschrieben werden soll. Alle keine Zeichenfolgenmethoden taghilfsattribut geschrieben werden soll, ohne die `@` Zeichen.
    
5.  Führen Sie die app, und navigieren Sie zu der Info-Ansicht, um die Website-Informationen finden Sie unter.

    >[!NOTE]
    >Sie können das folgende Markup wird ein schließendes Tag mit und entfernen Sie die Zeile mit `TagMode.StartTagAndEndTag` in der Tag-Hilfsprogramm:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Bedingung Tag-Hilfsprogramm

Die Bedingung Tag Hilfsprogramm rendert die Ausgabe, wenn der Wert "true" übergeben.

1.  Fügen Sie die folgenden `ConditionTagHelper` Klasse, um die *TagHelpers* Ordner.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Ersetzen Sie den Inhalt von der *Views/Home/Index.cshtml* Datei durch Folgendes Markup:

    <!-- literal_block {"xml:space": "preserve", "source": "mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml", "ids": [], "linenos": false, "highlight_args": {"linenostart": 1}} -->
    
    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=Model />
        <div condition="Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Ersetzen Sie die `Index` Methode in der `Home` -Controller mit den folgenden Code:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Führen Sie die app, und navigieren Sie zur Startseite. Das Markup in der bedingte `div` nicht gerendert werden. Fügen Sie die Abfragezeichenfolge `?approved=true` an die URL (z. B. `http://localhost:1235/Home/Index?approved=true`). `approved`ist auf True festgelegt und die bedingte Markup wird angezeigt.

>[!NOTE]
>Verwenden der [Nameof](https://msdn.microsoft.com/library/dn986596.aspx) Operator, um das Ziel, statt eine Zeichenfolge angeben, wie Sie mit den fett formatierten Tag-Hilfsprogramm-Attribut angeben:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>Die [Nameof](https://msdn.microsoft.com/library/dn986596.aspx) Operator schützt den Code sollte es jemals umgestaltet werden (es sollten so ändern Sie den Namen in `RedCondition`).

### <a name="avoiding-tag-helper-conflicts"></a>Vermeiden von Konflikten Tag-Hilfsprogramm

In diesem Abschnitt schreiben Sie ein Paar von Tag-Hilfsprogrammen automatische Verknüpfungen. Die erste ersetzt Markup, enthält eine URL mit HTTP-ein HTML-Anker Tag mit der gleichen URL (und und gibt daher einen Link zu der URL) beginnt. Die zweite wird die für eine URL Kunst WWW ab.

Da diese zwei Hilfsmethoden sind eng miteinander verknüpft, und Sie sie in der Zukunft Umgestalten können, müssen wir sie in der gleichen Datei bleiben.

1.  Fügen Sie die folgenden `AutoLinkerHttpTagHelper` Klasse, um die *TagHelpers* Ordner.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >Die `AutoLinkerHttpTagHelper` -Klasse Ziele `p` Elemente und verwendet [Regex](https://msdn.microsoft.com/library/system.text.regularexpressions.regex.aspx) Anker zu erstellen.

2.  Fügen Sie das folgende Markup bis zum Ende der *Views/Home/Contact.cshtml* Datei:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Führen Sie die app, und stellen Sie sicher, dass das Tag-Hilfsobjekt Anker ordnungsgemäß gerendert wird.

4.  Update der `AutoLinker` Klasse einbeziehen, die `AutoLinkerWwwTagHelper` konvertiert der Www-Text in ein Ankertag, der auch den ursprünglichen Www Text enthält. Im folgenden wird der aktualisierte Code hervorgehoben:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Führen Sie die app an. Beachten Sie, dass der Www-Text wird als Link gerendert der HTTP-Text ist jedoch nicht. Wenn Sie einen Haltepunkt in beiden Klassen einfügen, sehen Sie sich, dass die HTTP-Tag-Hilfsklasse zuerst ausgeführt wird. Das Problem ist, dass die Tag-Helper-Ausgabe zwischengespeichert wird, und wenn der WWW-Tag-Hilfsprogramm ausgeführt wird, werden im Cache gespeicherte Ausgabe aus dem HTTP-Tag-Hilfsprogramm überschrieben. Später in diesem Lernprogramm sehen wir, wie Sie die Reihenfolge steuern, die Tag-Hilfsprogramme in ausgeführt. Wir beheben den Code durch Folgendes:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >Sie haben in der Ausgabe zuerst die Hilfsprogramme für die automatische Verknüpfungen Tag den Inhalt des Ziels durch den folgenden Code:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >D. h., Sie rufen `GetChildContentAsync` mithilfe der `TagHelperOutput` übergebenen die `ProcessAsync` Methode. Wie bereits erwähnt zuvor, da die Ausgabe zwischengespeichert wird, der letzten tag Helper Wins ausgeführt. Sie haben dieses Problem behoben, durch den folgenden Code:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Der obige Code überprüft, um festzustellen, ob der Inhalt geändert wurde und aufweist, ruft den Inhalt aus dem Ausgabepuffer ab.

6.  Führen Sie die app, und stellen Sie sicher, dass die beiden Links funktionieren wie erwartet. Hat sich zwar als es angezeigt werden kann, dass unsere automatisch Linker Tag Helper richtig und vollständig ist, eine geringfügige Problem. Wenn der WWW-Tag-Hilfsmethode zuerst ausgeführt wird, sind der Www-Links nicht richtig. Aktualisieren Sie den Code durch Hinzufügen der `Order` Überladung zur Steuerung der Reihenfolge, die das Tag in ausgeführt wird. Die `Order` Eigenschaft bestimmt die Ausführungsreihenfolge relativ zu anderen Tag-Hilfsprogramme, die für dasselbe Element. Der standardreihenfolgenwert ist 0 (null), und Instanzen mit niedrigeren Werten werden zuerst ausgeführt.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Der Code oben wird sichergestellt, dass das HTTP-Tag-Hilfsobjekt vor dem WWW-Tag-Hilfsprogramm ausgeführt wird. Änderung `Order` auf `MaxValue` und stellen Sie sicher, dass das Markup für den WWW-Transponder generiert falsch ist.

## <a name="inspecting-and-retrieving-child-content"></a>Überprüfen und Abrufen von untergeordneten Inhalt

Der Tag-Hilfsvorlagen können mehrere Eigenschaften zum Abrufen von Inhalt.

-  Das Ergebnis des `GetChildContentAsync` angefügt werden können, um `output.Content`.
-  Sie können prüfen, dass das Ergebnis des `GetChildContentAsync` mit `GetContent`.
-  Wenn Sie ändern `output.Content`, taghelpers Text wird nicht ausgeführt oder gerendert werden, es sei denn, Sie rufen `GetChildContentAsync` wie unsere Auto-Linker-Beispiel:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Mehrere Aufrufe `GetChildContentAsync` wird derselbe Wert zurückgegeben und wird nicht erneut ausgeführt. die `TagHelper` body, wenn Sie in einem "false"-Parameter, der angibt, verwenden Sie nicht das zwischengespeicherte Resultset bewegen.
