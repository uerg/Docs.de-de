---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Ermöglicht nur bestimmte Zeichen in einem Textfeld (VB) | Microsoft Docs
author: wenz
description: ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind. Jedoch ist dies immer noch nicht eingeben ungültige verhindern...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b63a3582c09e08310c97d4adfc7b8273458a723
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870153"
---
<a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Ermöglicht nur bestimmte Zeichen in einem Textfeld (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind. Jedoch ist dies noch nicht ungültige Zeichen eingeben, und versuchen Sie zum Senden des Formulars verhindern.


## <a name="overview"></a>Übersicht

ASP.NET-Validierungssteuerelementen können sicherstellen, dass nur bestimmte Zeichen im Benutzereingaben zulässig sind. Jedoch ist dies noch nicht ungültige Zeichen eingeben, und versuchen Sie zum Senden des Formulars verhindern.

## <a name="steps"></a>Schritte

Das ASP.NET AJAX-Steuerelement-Toolkit enthält die `FilteredTextBox` steuern, welche ein Textfeld, das erweitert. Nach der Aktivierung kann nur ein bestimmten Satz von Zeichen in das Feld eingegeben werden.

Damit dies funktioniert, wird zunächst wie gewohnt ASP.NET AJAX `ScriptManager` lädt die JavaScript-Bibliotheken, die auch von ASP.NET AJAX-Steuerelement-Toolkit verwendet werden:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

Klicken Sie dann, benötigen wir ein Textfeld:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Schließlich die `FilteredTextBoxExtender` übernimmt die Kontrolle einschränken, die Zeichen, die der Benutzer berechtigt ist, auf den Typ. Legen Sie zuerst die `TargetControlID` -Attribut auf die `ID` von der `TextBox` Steuerelement. Wählen Sie dann eine der verfügbaren `FilterType` Werte:

- `Custom` Standardeinstellung; Sie müssen eine Liste der gültigen Zeichen angeben.
- `LowercaseLetters` dazu nur Kleinbuchstaben
- `Numbers` nur Ziffern
- `UppercaseLetters` nur Großbuchstaben.

Wenn die `Custom FilterType` verwendet wird, die `ValidChars` Eigenschaft muss festgelegt werden, und geben Sie eine Liste von Zeichen, die eingegeben werden kann. Übrigens: Wenn Sie versuchen, den Text in das Textfeld einfügen, werden alle ungültige Zeichen entfernt.

Hier wird das Markup für die `FilteredTextBoxExtender` Steuerelement, das nur Ziffern zulässt (etwas, das auch mit möglich gewesen wäre `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Führen Sie die Seite, und es wurde versucht, einen Buchstaben eingeben, wenn JavaScript aktiviert ist, können sie nicht verwendet werden; Ziffern werden jedoch auf der Seite angezeigt. Beachten Sie jedoch, dass der Schutz `FilteredTextBox` bietet ist nicht sichere: Falls JavaScript aktiviert ist, müssen Sie zusätzliche Validierung bedeutet, d. h. ASP verwenden, können keine Daten in das Textfeld eingegeben werden. NET Validierungssteuerelemente.


[![Es können nur Ziffern eingegeben werden](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Es können nur Ziffern eingegeben werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](allowing-only-certain-characters-in-a-text-box-cs.md)
