---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Zulassen von nur bestimmten Zeichen in einem Textfeld (VB) | Microsoft-Dokumentation
author: wenz
description: ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind. Jedoch ist dies immer noch nicht über die Eingabe ungültig, dass Benutzer...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: aec5a3af98cf40e460f4164fb8950e8029002937
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827400"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Zulassen von nur bestimmten Zeichen in einem Textfeld (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind. Aber dies immer noch Benutzer ungültige Zeichen eingeben, und versuchen, die zum Senden des Formulars verhindert nicht.


## <a name="overview"></a>Übersicht

ASP.NET-Steuerelemente zur gültigkeitsprüfung können stellen Sie sicher, dass nur bestimmten Zeichen in einer Benutzereingabe zulässig sind. Aber dies immer noch Benutzer ungültige Zeichen eingeben, und versuchen, die zum Senden des Formulars verhindert nicht.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX Control Toolkit enthält die `FilteredTextBox` steuern, welche in einem Textfeld, das erweitert. Nach der Aktivierung kann nur ein bestimmten Satz von Zeichen in das Feld eingegeben werden.

Damit dies funktioniert, wir zunächst wie gewohnt das ASP.NET AJAX `ScriptManager` lädt die JavaScript-Bibliotheken, die auch von der ASP.NET AJAX Control Toolkit verwendet werden:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Klicken Sie dann benötigen wir ein Textfeld:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Zum Schluss die `FilteredTextBoxExtender` Steuerelement übernimmt beschränken, die Zeichen, die Benutzer in den Typ. Legen Sie zuerst die `TargetControlID` -Attribut auf die `ID` von der `TextBox` Steuerelement. Wählen Sie eine der verfügbaren `FilterType` Werte:

- `Custom` Standardwert Sie müssen eine Liste der gültigen Zeichen angeben.
- `LowercaseLetters` nur Kleinbuchstaben
- `Numbers` nur Ziffern
- `UppercaseLetters` nur Großbuchstaben

Wenn die `Custom FilterType` verwendet wird, die `ValidChars` Eigenschaft muss festgelegt werden, und geben Sie eine Liste von Zeichen, die eingegeben werden kann. Übrigens: Wenn Sie versuchen, Text in das Textfeld einzufügen, werden alle ungültige Zeichen entfernt.

Dies ist das Markup für die `FilteredTextBoxExtender` -Steuerelement, das nur die Ziffern kann (etwas, das auch mit möglich gewesen wäre `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Führen die Seite, und es wurde versucht, einen Buchstaben eingeben, falls JavaScript aktiviert ist, funktioniert es nicht. Ziffern werden jedoch auf der Seite angezeigt. Beachten Sie jedoch, dass der Schutz `FilteredTextBox` bietet ist nicht hundertprozentig: Wenn JavaScript aktiviert ist, müssen Sie zusätzliche Überprüfungen, z. B. ASP Verwendung, können alle Daten in das Textfeld eingegeben werden. NET Steuerelementen zur gültigkeitsprüfung.


[![Es können nur Ziffern eingegeben werden](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Es können nur Ziffern eingegeben werden ([klicken Sie, um das Bild in voller Größe anzeigen](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](allowing-only-certain-characters-in-a-text-box-cs.md)
