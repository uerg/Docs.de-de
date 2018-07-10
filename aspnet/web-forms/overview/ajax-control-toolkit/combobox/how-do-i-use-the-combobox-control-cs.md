---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: Wie verwende ich das ComboBox-Steuerelement? (C#) | Microsoft-Dokumentation
author: microsoft
description: "\"ComboBox\" ist ein ASP.NET AJAX-Steuerelement, das die Flexibilität, ein Textfeld mit einer Liste von Optionen kombiniert, aus denen Benutzer auswählen können."
ms.author: aspnetcontent
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 07da594647c9b43bd31644aa8c7fedb256a12916
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839280"
---
<a name="how-do-i-use-the-combobox-control-c"></a>Wie verwende ich das ComboBox-Steuerelement? (C#)
====================
durch [Microsoft](https://github.com/microsoft)

> "ComboBox" ist ein ASP.NET AJAX-Steuerelement, das die Flexibilität, ein Textfeld mit einer Liste von Optionen kombiniert, aus denen Benutzer auswählen können.


Das Ziel in diesem Tutorial wird das AJAX Control Toolkit ComboBox-Steuerelement beschrieben. Das Kombinationsfeld funktioniert wie eine Verbindung zwischen einem standardmäßigen ASP.NET DropDownList-Steuerelement und ein TextBox-Steuerelement. Sie können entweder aus einer vorhandenen Liste von Elementen oder geben Sie ein neues Element.

Das Kombinationsfeld ist vergleichbar mit dem AutoComplete-Extender-Steuerelement, aber die Steuerelemente in verschiedenen Szenarien verwendet werden. Der AutoComplete-Extender fragt einen Webdienst, um übereinstimmende Einträge zu erhalten. Das Steuerelement "ComboBox" wird mit einem Satz von Elementen im Gegensatz dazu initialisiert. Mit der AutoComplete-Extender wird ist sinnvoll, wenn Sie mit einem großen Satz von Daten (Millionen von Auto-Teile) arbeiten, bei der Verwendung von Kombinationsfeld-Steuerelement sinnvoll bei der Arbeit mit einer kleinen Gruppe von Daten (Dutzende von Autos Teilen).

## <a name="selecting-from-a-static-list-of-items"></a>Auswahl aus einer statischen Liste von Elementen

Lassen Sie s beginnen mit einem einfachen Beispiel der Verwendung von Kombinationsfeld-Steuerelement. Stellen Sie sich, dass Sie eine statische Liste von Elementen in einer Dropdownliste angezeigt werden soll. Sie möchten jedoch geöffnet bleiben die Möglichkeit, dass die Liste nicht vollständig ist. Möchten Sie einem Benutzer ermöglichen, einen benutzerdefinierten Wert in der Liste eingeben.

Wir alle eine neue ASP.NET Web Forms-Seite erstellen und verwenden Sie das ComboBox-Steuerelement auf der Seite. Fügen Sie der neuen ASP.NET-Seite zu Ihrem Projekt, und wechseln Sie zur Entwurfsansicht.

Wenn Sie das ComboBox-Steuerelement auf der Seite verwenden möchten, müssen Sie ein ScriptManager-Steuerelement auf der Seite hinzufügen. Ziehen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" auf die Oberfläche des Designers ein. Sie sollten das ScriptManager-Steuerelement am oberen Rand der Seite hinzufügen; hinzuzufügen, können Sie direkt unterhalb der Serverseite öffnen &lt;Formular&gt; Tag.

Als Nächstes ziehen Sie das ComboBox-Steuerelement, auf der Seite zu erhalten. Sie finden das ComboBox-Steuerelement in der Toolbox mit anderen AJAX Control Toolkit-Steuerelemente und Extender (siehe Abbildung 1).


[![Einfaches Formular zum Erstellen einer Visitenkarte](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Abbildung 01**: das ComboBox-Steuerelement aus der Toolbox auswählen ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Wir verwenden das ComboBox-Steuerelement eine statische Liste mit Optionen angezeigt. Der Benutzer kann eine bestimmte Ebene des Spiciness für die Nahrungsmittel aus einer Liste von drei Optionen auswählen: in Cartoons ein, mittelgroßen und großen "heiß" (siehe Abbildung 2).


[![Auswahl aus einer statischen Liste von Elementen](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Abbildung 02**: Auswahl aus einer statischen Liste von Elementen ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Es gibt zwei Möglichkeiten, dass Sie diese Optionen für das ComboBox-Steuerelement hinzufügen können. Zuerst die Option Bearbeitungsoptionen Tasks beim Darüberbewegen des Mauszeigers des Mauszeigers über dem Steuerelement in der Entwurfsansicht, und öffnen Sie den Element-Editor (siehe Abbildung 3).


[![Bearbeiten von "ComboBox"-Elementen](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Abbildung 03**: Bearbeiten von "ComboBox"-Elemente ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image6.png))


Die zweite Option ist die Liste der Elemente zwischen den öffnenden und schließenden hinzufügen &lt;Asp: "ComboBox"&gt; Tags in der Quellansicht. Diese Seite in Codebeispiel 1 enthält die aktualisierte "ComboBox", die die Liste der Elemente enthält.

**1 – Static.aspx auflisten**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Wenn Sie die Seite in Codebeispiel 1 öffnen, können Sie eine der vorhandenen Optionen aus dem Kombinationsfeld auswählen. Das heißt, funktioniert die "ComboBox" genau wie ein DropDownList-Steuerelement ein.

Allerdings müssen Sie auch die Möglichkeit, eine neue Wahl (z. B. Super großartige), die nicht in der vorhandenen Liste eingeben. Das Kombinationsfeld funktioniert auch wie ein TextBox-Steuerelement.

Unabhängig davon, ob Sie eine bereits vorhandene auswählen, geben Sie ein Element oder ein benutzerdefiniertes Element, wenn Sie das Formular übermitteln Ihrer Wahl, die in das Label-Steuerelement angezeigt wird. Wenn Sie das Formular übermittelt, die BtnSubmit\_Klick-Handler ausgeführt wird und aktualisiert die Bezeichnung (siehe Abbildung 4).


[![Das ausgewählte Element anzeigen](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Abbildung 04**: das ausgewählte Element anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image8.png))


Das Kombinationsfeld unterstützt die gleichen Eigenschaften wie das DropDownList-Steuerelement, für das ausgewählte Element abrufen, nachdem ein Formular übermittelt wird:

- SelectedItem.Text – zeigt den Wert der Text-Eigenschaft des ausgewählten Elements an.
- SelectedItem.Value – zeigt den Wert der Value-Eigenschaft des ausgewählten Elements ab oder zeigt den Text in der ComboBox eingegeben.
- SelectedValue - identisch mit SelectedItem.Value, mit dem Unterschied, dass diese Eigenschaft, die Sie das ausgewählte Element mit Standardwert (ersten) angeben kann.

Wenn Sie eingeben wird eine benutzerdefinierte Auswahl in das Kombinationsfeld klicken Sie dann die benutzerdefinierte Auswahl SelectedItem.Text sowohl SelectedItem.Value Eigenschaften zugewiesen.

## <a name="selecting-the-list-of-items-from-the-database"></a>Die Liste der Elemente auswählen aus der Datenbank

Sie können die Liste der Elemente, die das Kombinationsfeld anzeigt aus einer Datenbank abrufen. Beispielsweise können Sie das Kombinationsfeld ein SqlDataSource-Steuerelement, ein ObjectDataSource-Steuerelement, ein LinqDataSource oder EntityDataSource binden.

Stellen Sie sich, dass Sie eine Liste von Filmen in einem Kombinationsfeld angezeigt werden soll. Die Liste der Filme aus der Tabelle der Datenbank Filme abgerufen werden sollen. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine Seite mit dem Namen Movies.aspx
2. Fügen Sie ein ScriptManager-Steuerelement auf der Seite, indem Sie im ScriptManager unter der Registerkarte "AJAX-Erweiterungen" in der Toolbox auf die Seite ziehen.
3. Fügen Sie ein ComboBox-Steuerelement durch Ziehen von das Kombinationsfeld auf der Seite auf der Seite hinzu.
4. In der Entwurfsansicht, bewegen Sie die Maus über das ComboBox-Steuerelement, und wählen Sie die **Datenquelle auswählen** aufgabenoption (siehe Abbildung 5). Der Konfigurations-Assistent wird gestartet.
5. In der **Auswählen einer Datenquelle** Schritt wählen die &lt;neue Datenquelle&gt; Option.
6. In der **wählen Sie einen Datenquellentyp** Schritt, wählen Sie die Datenbank.
7. In der **wählen Sie Ihre Datenverbindung** Schritt, wählen Sie Ihre Datenbank (z. B. MoviesDB.mdf).
8. In der **Verbindungszeichenfolge in der Anwendungskonfigurationsdatei speichern** Schritt, wählen Sie die Option zum Speichern der Verbindungszeichenfolge.
9. In der **konfigurieren Sie die Select-Anweisung** Schritt, wählen Sie die Datenbanktabelle Filme aus, und wählen Sie alle Spalten.
10. In der **Testabfrage** Schritt, klicken Sie auf die Schaltfläche "Fertig stellen".
11. In der **Datenquelle auswählen** Schritt wählen die Spalte Titel für das anzuzeigende Feld und die Id-Spalte, für die Daten Feld (siehe Abbildung).
12. Klicken Sie auf die Schaltfläche "OK", um den Assistenten zu schließen.


[![Auswählen einer Datenquelle](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Abbildung 05**: Auswählen einer Datenquelle ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Die Datenfelder für Text und Wert auswählen](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Abbildung 06**: Wählen die Datenfelder für Text und Wert ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Nachdem Sie die oben genannten Schritte abgeschlossen haben, ist das Kombinationsfeld zu einem SqlDataSource-Steuerelement gebunden, das die Filme aus der Tabelle der Datenbank Filme darstellt. Die Quelle für die Seite sieht wie Codebeispiel 2 (ich bereinigt, die die Formatierung ein bisschen).

**Codebeispiel 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Beachten Sie, dass das Steuerelement "ComboBox" eine Eigenschaft DataSourceID-Wert, die auf dem SqlDataSource-Steuerelement zeigt. Wenn Sie die Seite in einem Browser öffnen, wird die Liste der Filme aus der Datenbank angezeigt (siehe Abbildung 7). Sie können entweder eine Abholung einen Film aus der Liste, oder geben Sie einen neuen Film durch Eingabe des Films in das Kombinationsfeld.


[![Zeigt eine Liste von Filmen](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Abbildung 07**: Zeigt eine Liste von Filmen ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Festlegen der DropDownStyle

Sie können die ComboBox DropDownStyle-Eigenschaft verwenden, um das Verhalten des "ComboBox" ändern. Diese Eigenschaft akzeptiert es mögliche Werte:

- Dropdown "" - können (Standardwert) der "ComboBox" zeigt eine Dropdownliste, wenn Sie den Pfeil und klicken Sie auf einen benutzerdefinierten Wert eingeben.
- Einfach: das Kombinationsfeld wird automatisch eine Dropdownliste angezeigt, und Sie können einen benutzerdefinierten Wert eingeben.
- DropDownList - funktioniert das Kombinationsfeld, genauso wie ein DropDownList-Steuerelement.

Der Unterschied zwischen Dropdownliste und einfach ist, wenn die Liste von Elementen angezeigt wird. Im Fall von einfachen wird die Liste angezeigt, sofort Wenn Sie an das Kombinationsfeld Fokus. Im Fall von Dropdown-Liste müssen Sie den Pfeil, um die Liste der Elemente finden Sie unter klicken.

Die DropDownList-Wert bewirkt, dass das ComboBox-Steuerelement funktioniert wie ein standard DropDownList-Steuerelement. Allerdings besteht auch hier ein wichtiger Unterschied. Ältere Versionen von Internet Explorer anzeigen ein DropDownList-Steuerelement mit einem unendlichen Z-Index, also vor jedem Steuerelement platziert werden, vor das Steuerelement angezeigt wird. Da das Kombinationsfeld eine HTML rendert &lt;Div&gt; Tag anstelle von einem HTML &lt;wählen&gt; -Tag, Z-Reihenfolge für das Kombinationsfeld ordnungsgemäß berücksichtigt werden.

## <a name="setting-the-autocompletemode"></a>Festlegen der AutoCompleteMode

Sie verwenden die ComboBox AutoCompleteMode-Eigenschaft, um anzugeben, was geschieht, wenn ein Benutzer Text in der ComboBox eingibt. Diese Eigenschaft akzeptiert die folgenden möglichen Werten:

- None: (Standardwert), die das Kombinationsfeld keine AutoComplete-Verhalten bietet.
- Vorschlagen - Kombinationsfeld zeigt die Liste, und das übereinstimmende Element in der Liste werden hervorgehoben (siehe Abbildung 8).
- Fügen Sie - Kombinationsfeld wird die Liste nicht angezeigt, und er fügt das übereinstimmende Element aus der Liste auf, was Sie eingegeben haben (siehe Abbildung 9).
- SuggestAppend - Kombinationsfeld sowohl die Liste und fügt das entsprechende Element aus der Liste auf, was Sie eingegeben haben (siehe Abbildung 10).


[![Das Kombinationsfeld macht einen Vorschlag](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Abbildung 08**: die "ComboBox" macht einen Vorschlag ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[!["ComboBox" fügt die übereinstimmenden text](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Abbildung 09**: "ComboBox" fügt die übereinstimmenden Text ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![Das Kombinationsfeld schlägt vor, und fügt](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Abbildung 10**: die "ComboBox" schlägt vor, und fügt ([klicken Sie, um das Bild in voller Größe anzeigen](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie das ComboBox-Steuerelement zu verwenden, um einen festen Satz von Elementen anzuzeigen. Wir gebunden das ComboBox-Steuerelement, sowohl um einen statischen Satz von Elementen und einer Datenbanktabelle. Schließlich haben Sie das Verhalten des "ComboBox" durch Festlegen seiner Eigenschaften DropDownStyle und AutoCompleteMode ändern.

> [!div class="step-by-step"]
> [Nächste](how-do-i-use-the-combobox-control-vb.md)
