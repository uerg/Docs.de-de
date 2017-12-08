---
uid: mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
title: Erstellen von benutzerdefinierten HTML-Hilfsmethoden (VB) | Microsoft Docs
author: microsoft
description: "Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen. Durch die Nutzung von HTML-Hilfsobjekt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: f96f4800-19ef-44c0-b457-55e777eb5de8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: e389a03228995ce0a6926a53af38f26ad51372d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-custom-html-helpers-vb"></a>Erstellen von benutzerdefinierten HTML-Hilfsmethoden (VB)
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_VB.pdf)

> Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen. Durch die Nutzung von HTML-Hilfsmethoden, können Sie die Menge des mühsam Eingabe der HTML-Tags, die Sie ausführen müssen, um eine standard-HTML-Seite erstellen reduzieren.


Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie benutzerdefinierte HTML-Hilfsmethoden erstellen können, die Sie im MVC-Ansichten zur Verfügung stehen. Durch die Nutzung von HTML-Hilfsmethoden, können Sie die Menge des mühsam Eingabe der HTML-Tags, die Sie ausführen müssen, um eine standard-HTML-Seite erstellen reduzieren.

Im ersten Teil dieses Lernprogramms beschreiben ich einige der vorhandenen HTML-Hilfsmethoden, die mit ASP.NET MVC-Framework enthalten. Als Nächstes ich werden zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden beschrieben: Ich wird erläutert, wie zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden durch Erstellen einer freigegebenen Methode und durch Erstellen einer Erweiterungsmethode.

## <a name="understanding-html-helpers"></a>Grundlegendes zu HTML-Hilfsmethoden

Ein HTML-Hilfsobjekt ist nur eine Methode, die eine Zeichenfolge zurückgibt. Die Zeichenfolge kann jede Art von Inhalt darstellen, die Sie möchten. Beispielsweise können Sie HTML-Hilfsmethoden zum Rendern von HTML-Tags wie HTML-standard `<input>` und `<img>` Tags. HTML-Hilfsmethoden können auch komplexeren Inhalt, z. B. eine Registerkartenleiste oder eine HTML-Tabelle der Datenbankdaten rendern.

ASP.NET MVC-Framework enthält die folgenden standardmäßigen HTML-Hilfsmethoden (Dies ist keine vollständige Liste):

- Html.ActionLink()
- Html.BeginForm()
- Html.CheckBox()
- Html.DropDownList()
- Html.EndForm()
- Html.Hidden()
- Html.ListBox()
- Html.Password()
- Html.RadioButton()
- Html.TextArea()
- Html.TextBox()

Betrachten Sie beispielsweise das Formular im Codebeispiel 1 aus. Dieses Formular wird mithilfe von zwei der standardmäßigen HTML-Hilfsmethoden gerendert (siehe Abbildung 1). Dieses Formular verwendet die `Html.BeginForm()` und `Html.TextBox()` Hilfsmethoden.


[![Seite gerendert wird, mit HTML-Hilfsmethoden](creating-custom-html-helpers-vb/_static/image2.png)](creating-custom-html-helpers-vb/_static/image1.png)

**Abbildung 01**: Seite gerendert wird, mit HTML-Hilfsmethoden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-custom-html-helpers-vb/_static/image3.png))


**Auflisten von 1 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample1.aspx)]

Die `Html.BeginForm()` Hilfsmethode wird verwendet, um die öffnenden und schließenden HTML erstellen `<form>` Tags. Beachten Sie, dass die `Html.BeginForm()` Methode wird aufgerufen, in einer mit Anweisung. Die Anweisung wird sichergestellt, dass die `<form>` Tag geschlossen wird, am Ende der mit Block.

Wunsch statt einer mit blockieren, können Sie die Html.EndForm() Hilfsmethode schließen Aufrufen der `<form>` Tag. Verwenden Sie unabhängig davon, welche Vorgehensweise erstellen eine öffnende und schließende `<form>` Tag, die für Sie besonders intuitiv erscheint.

Die `Html.TextBox()` Hilfsmethoden werden zum Rendern von HTML in Codebeispiel 1 verwendet `<input>` Tags. Wenn Sie in Ihrem Browser Quelltext anzeigen auswählen sehen Sie die HTML-Quelle in 2 aufgelistet. Beachten Sie, dass die Datenquelle und des standard-HTML-Tags enthält.

> [!IMPORTANT]
> Beachten Sie, dass die `Html.TextBox()`- HTML-Hilfsobjekt wird mit gerendert `<%= %>` anstelle von tags `<% %>` Tags. Wenn Sie das Gleichheitszeichen nicht einschließen, ruft nichts an den Browser gerendert.

ASP.NET MVC-Framework enthält einen kleinen Satz von Hilfsmethoden. In den meisten Fällen müssen Sie das MVC-Framework mit der benutzerdefinierten HTML-Hilfsmethoden zu erweitern. Im weiteren Verlauf dieses Lernprogramms erfahren Sie zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden.

**Auflisten von 2 –`Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-shared-methods"></a>Erstellen von HTML-Hilfsprogrammen mit freigegebener Methoden

Die einfachste Möglichkeit zum Erstellen neuer HTML-Hilfsobjekt ist eine freigegebene Methode erstellen, die eine Zeichenfolge zurückgibt. Stellen Sie sich vor, z. B., dass Sie eine neue HTML-Hilfsobjekt erstellen, der eine HTML rendert möchten `<label>` Tag. Sie können mithilfe die Klasse 2 auflisten Rendern einer `<label>`.

**Auflisten von 2 –`Helpers\LabelHelper.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample3.vb)]

Es gibt keine besonderen über die Klasse 2 auflisten. Die `Label()` -Methode einfach eine Zeichenfolge zurückgibt.

Die geänderte Indexansicht auflisten 3 verwendet den `LabelHelper` zum Rendern von HTML `<label>` Tags. Beachten Sie, die die Sicht enthält eine `<%@ imports %>` -Direktive, die den Application1.Helpers-Namespace importiert.

**Auflisten von 2 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Erstellen von HTML-Hilfsprogrammen mit Erweiterungsmethoden [c#]

HTML-Hilfsmethoden funktioniert wie der standardmäßigen HTML-Hilfsmethoden, die in ASP.NET MVC-Framework einbezogen wird, müssen Sie zum Erstellen von Erweiterungsmethoden [c#] erstellt werden soll. Erweiterungsmethoden können Sie einer vorhandenen Klasse neue Methoden hinzufügen. Wenn Sie ein HTML-Hilfsmethode erstellen, Sie neue Methoden zum Hinzufügen der `HtmlHelper` Klasse, die durch eine Sicht HTML-Eigenschaft dargestellt wird.

Das Visual Basic-Modul im Codebeispiel 3 fügt eine Erweiterungsmethode mit dem Namen `Label()` auf die `HtmlHelper` Klasse. Es gibt mehrere Dinge, die Sie Informationen zu diesem Modul beachten sollten. Beachten Sie, dass das Modul mit ergänzt wird zuerst, den `<Extension()>` Attribut. Um dieses Attribut verwenden, müssen Sie importieren die `System.Runtime.CompilerServices` Namespace

Zweitens, beachten Sie, dass der erste Parameter der `Label()` Methode stellt der `HtmlHelper` Klasse. Der erste Parameter einer Erweiterungsmethode gibt an, die Klasse, die die Erweiterungsmethode erweitert wird.

**Auflisten von 3:`Helpers\LabelExtensions.vb`**

[!code-vb[Main](creating-custom-html-helpers-vb/samples/sample5.vb)]

Nachdem Sie eine Erweiterungsmethode zu erstellen und die Anwendung erfolgreich erstellen, wird die Erweiterungsmethode in Visual Studio Intellisense wie bei allen anderen Methoden einer Klasse angezeigt (siehe Abbildung 2). Der einzige Unterschied ist diese Erweiterung, die Methoden mit einem besonderen Symbol neben dem Namen (Symbol nach unten weisenden Pfeil) angezeigt werden.


[![Verwenden die Erweiterungsmethode Html.Label()](creating-custom-html-helpers-vb/_static/image5.png)](creating-custom-html-helpers-vb/_static/image4.png)

**Abbildung 02**: mit der Erweiterungsmethode Html.Label() ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-custom-html-helpers-vb/_static/image6.png))


Die geänderte Indexansicht Auflisten von 4 verwendet die Erweiterungsmethode Html.Label() aller gerendert seine &lt;Bezeichnung&gt; Tags.

**Auflisten von 4 –`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-vb/samples/sample6.aspx)]

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, zwei Methoden zum Erstellen von benutzerdefinierten HTML-Hilfsmethoden. Zunächst haben Sie gelernt, wie erstellen eine benutzerdefinierten `Label()` HTML-Hilfsobjekt, indem Sie eine freigegebene Methode erstellen, gibt eine Zeichenfolge zurück. Als Nächstes haben Sie gelernt, wie zum Erstellen eines benutzerdefinierten `Label()` HTML-Hilfsmethode durch Erstellen einer Erweiterungsmethode für die `HtmlHelper` Klasse.

In diesem Lernprogramm konzentriert sich ich auf eine sehr einfache HTML-Hilfsmethode erstellen. Beachten Sie, dass ein HTML-Hilfsobjekt ebenso kompliziert, wie gewünscht werden. Sie können HTML-Hilfsmethoden erstellen, die Inhalte wie Strukturansichten, Menüs oder Tabellen der Datenbankdaten rendern.

>[!div class="step-by-step"]
[Zurück](asp-net-mvc-views-overview-vb.md)
[Weiter](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
