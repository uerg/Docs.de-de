---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Wie verwende ich das HTML-Editor-Steuerelement? (VB) | Microsoft-Dokumentation
author: microsoft
description: HTMLEditor ist ein ASP.NET AJAX-Steuerelement mit dem Sie ganz einfach erstellen und Bearbeiten von HTML-Inhalte über Schaltflächen in einer Symbolleiste an.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4b3882f631799eb99f3c63da89097daba1fca3ff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401966"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Wie verwende ich das HTML-Editor-Steuerelement? (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> HTMLEditor ist ein ASP.NET AJAX-Steuerelement mit dem Sie ganz einfach erstellen und Bearbeiten von HTML-Inhalte über Schaltflächen in einer Symbolleiste an.


Das Ziel dieses Lernprogramms ist eine Übersicht über das HTML-Editor-Steuerelement, das mit dem AJAX Control Toolkit enthalten bereit. Der HTML-Editor enthält Optionen zum Ändern der Größe der Schriftart, Auswählen einer Schriftart, Hintergrundfarbe ändern, ändern die die Vordergrundfarbe darstellt, Hinzufügen von Links, Bilder, hinzufügen, ändern die textausrichtung und Ausführen Ausschneiden, kopieren und einfügen (siehe Abbildung 1).


[![Der HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Abbildung 01**: der HTML-Editor ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


Der HTML-Editor können Sie Inhalte mithilfe von Entwurfsmodus eingeben, oder Sie HTML direkt eingeben. Sie auch erhalten die Möglichkeit, Ihren HTML-Inhalt (Vorschau) (siehe Abbildung 2).


[![Entwurf, HTML und Vorschau Schaltflächen](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Abbildung 02**: Entwurf, HTML und Vorschau-Schaltflächen ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


In diesem Tutorial erfahren Sie, wie der HTML-Editor angezeigt, wie Sie die Schaltflächen der Symbolleiste anpassen, die in der HTML-Editor angezeigt werden und Cross-Site Scripting-Angriffe zu vermeiden.

## <a name="displaying-the-html-editor"></a>Der HTML-Editor anzeigen

Bevor Sie den HTML-Editor in einer ASP.NET-Seite verwenden können, müssen Sie zuerst ein ScriptManager-Steuerelement auf der Seite hinzufügen. Das ScriptManager-Steuerelement befindet sich unterhalb der Registerkarte AJAX-Erweiterungen in der Visual Studio/Visual Web Developer Express-Toolbox.

Sie sollten das ScriptManager-Steuerelement am oberen Rand der Seite vor allen anderen Steuerelementen auf der Seite platzieren. Angenommen, Sie können platzieren Sie es sofort unter das öffnendes serverseitige &lt;Formular&gt; Tag.

Das HTML-Editor-Steuerelement befindet sich in der Toolbox mit dem Rest der Steuerelemente des AJAX Control Toolkit. Sie heißt das Editorsteuerelement (siehe Abbildung 3).


[![Das HTML-Editor-Steuerelement](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Abbildung 03**: der HTML-Editor-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Nachdem Sie auf eine Seite der HTML-Editor ziehen, können Sie seine Eigenschaften im Eigenschaftenfenster festlegen. Sie möchten z. B. normalerweise zum Festlegen der Eigenschaften von Breite und Höhe. Codebeispiel 1 enthält die Quelle für eine ASP.NET-Seite, die einen HTML-Editor enthält.

**1 – SimpleEditor.aspx auflisten**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

Die Seite in Codebeispiel 1 enthält ein HTML-Editor-Steuerelement, ein Schaltflächen-Steuerelement und ein literales Steuerelement. Wenn Sie die Schaltfläche klicken, den Inhalt des HTML-Editors angezeigt werden, im Literalsteuerelement (siehe Abbildung 4).


[![Senden eines Formulars mit einem HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Abbildung 04**: Senden eines Formulars mit einem HTML-Editor ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


Die HTML-Editor-Content-Eigenschaft wird verwendet, zum Abrufen des HTML-Inhalts in der HTML-Editor eingegeben werden. Denken Sie daran, dass diese HTML-Inhalt JavaScript enthalten kann. Im nächsten Abschnitt wird erläutert, wie JavaScript-Injection-Angriffe verhindern können.

## <a name="customizing-the-html-editor-toolbar"></a>Anpassen der HTML-Editor-Symbolleiste

Sie können anpassen, dass genau welche Schaltflächen im Editor dargestellt. Beispielsweise empfiehlt es sich um die Registerkarte "HTML", um zu verhindern, dass Benutzer der HTML-Editor in HTML-Modus wechseln zu entfernen. Oder, entfernen Sie die Schriftart Größe Dropdown-Liste aus, um zu verhindern, dass Benutzer übermäßig großen Text in einem Forum erstellen möchten (siehe Abbildung 5) nach Nachrichten.


[![Einen benutzerdefinierten HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Abbildung 05**: eine angepasste HTML-Editor ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Durch Ableiten einer neuen HTML-Editor von der Basisklasse-Editor können Sie die Schaltflächen der Symbolleiste anpassen. Beispielsweise enthält der benutzerdefinierte Editor Programmausdruck 2 nur Symbolleisten-Schaltflächen für fett- und kursivformatierung. Alle anderen Symbolleisten-Schaltflächen wurden entfernt. Darüber hinaus wurde zwischen dem unteren Rand der Editor die Registerkarte "HTML" entfernt (jedoch die Entwurf und Vorschau-Registerkarten sind weiterhin vorhanden).

**Codebeispiel 2 - App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Sie müssen die Klasse im Codebeispiel 2 zu Ihrer App hinzufügen\_Ordner Code, sodass die Klasse automatisch kompiliert wird. Wenn die App\_Ordner "Code" in Ihre Website nicht vorhanden, und klicken Sie dann können Sie einfach den Ordner hinzufügen.

Nach der Erstellung eines benutzerdefinierten Editors können Sie es für eine ASP.NET-Seite auf die gleiche Weise hinzufügen wie Sie die normale HTML-Editor (siehe Codebeispiel 3) hinzufügen.

**Codebeispiel 3 - ShowCustomEditor.aspx**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Vermeiden von siteübergreifendem Skripting (XSS)-Angriffe

Wenn Sie Benutzereingaben akzeptieren und wieder, in der Eingabe auf Ihrer Website anzuzeigen, öffnen Sie möglicherweise Ihre Website für Cross-Site Scripting (XSS)-Angriffe. Theoretisch kann ein böswilliger Hacker JavaScript-Code senden, die ausgeführt wird, wenn die Eingabe erneut angezeigt wird. Der JavaScript-Code kann verwendet werden, um Benutzerkennwörter oder andere sensible Daten zu stehlen.

In der Regel können Sie Punkten, XSS-Angriffe durch HTML-Codierung einfach alle Eingaben, die Sie vor der Anzeige auf einer Webseite von einem Benutzer abrufen. Allerdings in HTML codierte Ausgabe im HTML-Editor wird nicht nur codieren &lt;Skript&gt; Tags, wäre es auch alle HTML-Tags codieren. Das heißt, würden Sie die Formatierung wie z. B. die Schriftart, Schriftgrad und Hintergrundfarbe verlieren.

Wenn Sie vertraulichen Informationen von Ihren Benutzern – z. B. Kennwörter, -Kreditkartennummern und Sozialversicherungsnummern - sammeln sollten dann Sie nicht nicht codierten Inhalt anzeigen, die Sie von einem Benutzer mit dem HTML-Editor abrufen. Sie sollten nur in Situationen, in dem Sie werden nicht erneut den HTML-Inhalt anzeigen, oder der HTML-Inhalt wird übermittelt wird, der HTML-Editor verwenden, zu Ihrer Website von einer vertrauenswürdigen Partei.

Stellen Sie sich vor, z. B., dass Sie eine Bloganwendung erstellen. In diesem Fall vereinfacht sinnvoll, den HTML-Editor zu verwenden, beim Verfassen von Blogbeiträgen. Sie sind der einzige, die einen Blogbeitrag übermittelt, und es empfiehlt sich, Sie können darauf vertrauen selbst nicht, um schädlichen JavaScript-Code zu übermitteln. Allerdings ist es nicht sinnvoll, der HTML-Editor zu verwenden, wenn anonyme Benutzer zum Posten von Kommentaren zulassen einzurichten. Sie sollten in Situationen besonders vorsichtig sein in denen Benutzer die vertraulichen Daten wie Kennwörter senden. Potenziell könnte ein böswilliger Benutzer einen Kommentar zu Posten, der die richtigen JavaScript-Code für ein Kennwort zu stehlen.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial wurden eine kurze Übersicht über das HTML-Editor-Steuerelement enthalten, die im AJAX Control Toolkit bereitgestellt. Sie haben gelernt, wie umfangreiche Inhalte von einem Benutzer akzeptieren und übermitteln den Inhalt an den Server mit der HTML-Editor. Es wird auch erläutert, wie Sie die Schaltflächen der Symbolleiste anpassen können, die von der HTML-Editor angezeigt werden. Schließlich haben Sie Gewusst, Cross-Site Scripting-Angriffe zu vermeiden, wenn es sich bei der HTML-Editor mit potenziell schädlichen Eingaben akzeptieren.

> [!div class="step-by-step"]
> [Vorherige](how-do-i-use-the-html-editor-control-cs.md)
