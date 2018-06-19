---
uid: web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
title: Verwendung der HTML-Editor-Steuerelement (VB) | Microsoft Docs
author: microsoft
description: HTMLEditor ist ein ASP.NET AJAX-Steuerelement, mit dem Sie leicht erstellen und Bearbeiten von HTML-Inhalt über die Schaltflächen in einer Symbolleiste.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 32ec9321-7c8c-4b0f-8234-99acb56df6b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/htmleditor/how-do-i-use-the-html-editor-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 4833949a54fa9ae12eaf7b596a5fe1ddfd1f7b7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873299"
---
<a name="how-do-i-use-the-html-editor-control-vb"></a>Verwendung der HTML-Editor-Steuerelement (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> HTMLEditor ist ein ASP.NET AJAX-Steuerelement, mit dem Sie leicht erstellen und Bearbeiten von HTML-Inhalt über die Schaltflächen in einer Symbolleiste.


Ziel dieses Lernprogramms werden Sie einen Überblick über die AJAX-Steuerelement-Toolkit enthaltene HTML-Editor-Steuerelement zur Verfügung stellen. Der HTML-Editor bietet Optionen zum Ändern der Größe der Schriftart, Auswählen einer Schriftart, Hintergrundfarbe ändern, ändern die Vordergrundfarbe Hinzufügen von Links, Hinzufügen von Bildern, ändern die textausrichtung und Ausführen Ausschneiden, kopieren und einfügen (siehe Abbildung 1).


[![Der HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image1.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image1.png)

**Abbildung 01**: im HTML-Editor ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-html-editor-control-vb/_static/image2.png))


Der HTML-Editor können Sie Inhalt mithilfe einer Entwurfsmodus eingeben oder HTML direkt eingeben. Auch zur Verfügung mit der Option aus, um die HTML-Inhalt in der Vorschau anzeigen (siehe Abbildung 2).


[![Entwurf, HTML und Vorschau Schaltflächen](how-do-i-use-the-html-editor-control-vb/_static/image2.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image3.png)

**Abbildung 02**: Entwurf, HTML und Vorschau Schaltflächen ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-html-editor-control-vb/_static/image4.png))


In diesem Lernprogramm erfahren Sie, wie der HTML-Editor angezeigt, wie Sie die Schaltflächen der Symbolleiste anpassen, die im HTML-Editor angezeigt werden und wie Cross-Site Scripting-Angriffe zu vermeiden.

## <a name="displaying-the-html-editor"></a>Anzeigen des HTML-Editors

Bevor Sie die HTML-Editor auf einer ASP.NET-Seite verwenden können, müssen Sie zuerst ein ScriptManager-Steuerelement auf der Seite hinzufügen. Die ScriptManager-Steuerelement befindet sich unterhalb der Registerkarte "AJAX-Erweiterungen" in der Visual Studio/Visual Web Developer Express-Toolbox.

Sie sollten das ScriptManager-Steuerelement am oberen Rand der Seite vor allen anderen Steuerelementen auf der Seite platzieren. Beispielsweise können Sie diesen sofort platzieren, unterhalb der öffnenden serverseitige &lt;Formular&gt; Tag.

Das HTML-Editor-Steuerelement befindet sich in der Toolbox mit dem Rest der AJAX-Steuerelement-Toolkit-Steuerelemente. Es ist die Editor-Steuerelement mit dem Namen (siehe Abbildung 3).


[![Das HTML-Editor-Steuerelement](how-do-i-use-the-html-editor-control-vb/_static/image3.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image5.png)

**Abbildung 03**: im HTML-Editor-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-html-editor-control-vb/_static/image6.png))


Nachdem Sie die HTML-Editor auf einer Seite ziehen, können Sie seine Eigenschaften im Eigenschaftenfenster festlegen. Sie möchten z. B. normalerweise zum Festlegen der Eigenschaften für Höhe und Breite. Auflisten von 1 enthält die Quelle für eine ASP.NET-Seite, die einen HTML-Editor enthält.

**1 – SimpleEditor.aspx auflisten**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample1.aspx)]

Die Seite im Codebeispiel 1 enthält ein HTML-Editor-Steuerelement, ein Schaltflächen- und ein Literal-Steuerelement. Wenn Sie die Schaltfläche klicken, wird der Inhalt des HTML-Editor angezeigt, im Literalsteuerelement (siehe Abbildung 4).


[![Senden eines Formulars mit HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image4.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image7.png)

**Abbildung 04**: Senden eines Formulars mit HTML-Editor ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-html-editor-control-vb/_static/image8.png))


Der HTML-Editor-Content-Eigenschaft wird verwendet, zum Abrufen des HTML-Inhalts in den HTML-Editor eingegeben werden. Denken Sie daran, dass diese HTML-Inhalt JavaScript enthalten kann. Im nächsten Abschnitt wird erläutert, wie Sie JavaScript-Injection-Angriffe zu verhindern, dass können.

## <a name="customizing-the-html-editor-toolbar"></a>Anpassen der HTML-Editor-Symbolleiste

Sie können anpassen, genau welche Schaltflächen im Editor angezeigt. Sie möchten z. B. Entfernen der Registerkarte "HTML", um zu verhindern, dass Benutzer die HTML-Editor in HTML-Modus wechseln. Oder Sie können die Schriftart Größe Dropdown-Liste, um zu verhindern, dass Benutzer erstellen übermäßig groß Text in einem Forum entfernen möchten (siehe Abbildung 5) nach Nachrichten.


[![Eine benutzerdefinierte HTML-Editor](how-do-i-use-the-html-editor-control-vb/_static/image5.jpg)](how-do-i-use-the-html-editor-control-vb/_static/image9.png)

**Abbildung 05**: eine benutzerdefinierte HTML-Editor ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-html-editor-control-vb/_static/image10.png))


Durch die Editor-Basisklasse abgeleitet ein neuer HTML-Editor können Sie die Schaltflächen der Symbolleiste anpassen. Beispielsweise enthält der benutzerdefinierte Editor auflisten 2 nur Symbolleisten-Schaltflächen für fett- und kursivformatierung. Symbolleisten-Schaltflächen wurden entfernt. Darüber hinaus die Registerkarte "HTML" wurde aus den unteren Bereich des Editors entfernt (jedoch die Registerkarten "Entwurf" und "Vorschau" weiterhin vorhanden sind).

**Auflisten von 2 – App\_Code\CustomEditor.vb**

[!code-vb[Main](how-do-i-use-the-html-editor-control-vb/samples/sample2.vb)]

Müssen Sie die Klasse 2 Auflisten Ihrer App hinzufügen\_Ordner Code, sodass die Klasse automatisch kompiliert wird. Wenn die App\_Codeordner in Ihrer Website nicht vorhanden, dann können Sie einfach den Ordner hinzufügen.

Nach der Erstellung eines benutzerdefinierten Editors können Sie es zu einer ASP.NET-Seite auf die gleiche Weise hinzufügen wie Sie die normale HTML-Editor (Siehe auflisten von 3) hinzufügen.

**3 – ShowCustomEditor.aspx auflisten**

[!code-aspx[Main](how-do-i-use-the-html-editor-control-vb/samples/sample3.aspx)]

## <a name="avoiding-cross-site-scripting-xss-attacks"></a>Vermeiden von siteübergreifendem Skripting (XSS)-Angriffe

Wenn Sie Benutzereingaben akzeptieren, und die Eingabe auf Ihrer Website anzuzeigen, öffnen Sie möglicherweise Ihre Website für Cross-Site-Skripting (XSS)-Angriffe. Theoretisch kann ein böswilliger Hacker JavaScript-Code senden, der ausgeführt wird, wenn die Eingabe erneut angezeigt wird. Der JavaScript-Code kann verwendet werden, Kennwörter oder andere sensible Daten zu stehlen.

Normalerweise können Sie zunichte machen XSS-Angriffen durch HTML-Codierung der Eingabe, die Sie von einem Benutzer abrufen, vor der Anzeige auf einer Webseite. Allerdings codiert HTML-Codierung die Ausgabe im HTML-Editor würde nicht nur &lt;Skript&gt; Tags, würde auch codiert werden alle HTML-Tags. Das heißt, würden Sie die Formatierung wie z. B. Schriftart, Schriftgrad und Hintergrundfarbe verlieren.

Wenn Sie vertraulichen Informationen von Ihren Benutzern – z. B. Kennwörter, Kreditkartennummern- und Sozialversicherungsnummern - sammeln soll dann nicht uncodierten Inhalt angezeigt, die Sie von einem Benutzer mit dem HTML-Editor abrufen. Sie sollten nur in Situationen, in dem Sie sind nicht die erneute den HTML-Inhalt anzeigen, oder der HTML-Inhalt ist gesendet wird, der HTML-Editor verwenden, auf Ihre Website von einer vertrauenswürdigen Partei.

Stellen Sie sich vor, z. B., dass Sie eine Bloganwendung erstellt werden. In diesem Fall sinnvoll es HTML-Editor verwenden, wenn Blogbeiträge zu verfassen. Sie sind der einzige, die einen Blogbeitrag sendet, und es empfiehlt sich, Sie sich nicht um bösartigen JavaScript übermitteln vertrauen können. Allerdings ist es nicht HTML-Editor verwenden, wenn anonyme Benutzer kommentieren sinnvoll. Sie sollten in Situationen besonders vorsichtig sein in denen Benutzer vertraulichen Informationen wie Kennwörter senden. Ein böswilliger Benutzer konnte möglicherweise, einen Kommentar gesendet werden, der die richtige JavaScript für stiehlt ein Kennwort enthält.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurden das HTML-Editor-Steuerelement in das AJAX-Steuerelement-Toolkit enthalten eine kurze Übersicht über bereitgestellt. Sie haben gelernt, wie HTML-Editor mit der Inhalte von einem Benutzer akzeptieren und den Inhalt an den Server übermitteln. Es wird erläutert, wie Sie die Schaltflächen der Symbolleiste anpassen können, die vom HTML-Editor angezeigt werden. Schließlich haben Sie gelernt, wie Cross-Site Scripting Angriffe vermieden werden, wenn der HTML-Editor mit potenziell schädlichen Eingaben akzeptieren.

> [!div class="step-by-step"]
> [Vorherige](how-do-i-use-the-html-editor-control-cs.md)
