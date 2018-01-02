---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Verwendung der ComboBox-Steuerelement (VB) | Microsoft Docs
author: microsoft
description: "Kombinationsfeld-Steuerelement ist ein ASP.NET AJAX-Steuerelement, das die Flexibilität eines Textfelds mit einer Liste von Optionen kombiniert, in dem Benutzer auswählen können."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 54e36cf275dcc4b85253dc3b8bb5b0dbb027af77
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-combobox-control-vb"></a>Verwendung der ComboBox-Steuerelement (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Kombinationsfeld-Steuerelement ist ein ASP.NET AJAX-Steuerelement, das die Flexibilität eines Textfelds mit einer Liste von Optionen kombiniert, in dem Benutzer auswählen können.


Das Ziel dieses Lernprogramms ist die AJAX-Steuerelement-Toolkit ComboBox-Steuerelement zu erläutern. Das Kombinationsfeld funktioniert wie eine Kombination zwischen einem Standardsteuerelement DropDownList mit ASP.NET und ein TextBox-Steuerelement. Sie können entweder aus einer vorhandenen Liste von Elementen auswählen oder geben Sie ein neues Element.

Das Kombinationsfeld ähnelt der AutoVervollständigen-Extendersteuerelement, aber die Steuerelemente in verschiedenen Szenarien verwendet werden. Der AutoComplete-Extender fragt einen Webdienst, um übereinstimmende Einträge zu erhalten. ComboBox-Steuerelement wird hingegen mit einem Satz von Elementen initialisiert. Mit der AutoVervollständigen-Extender wird ist sinnvoll, wenn Sie mit einer großen Datenmenge (Millionen von Car Teile) arbeiten, bei der Verwendung der ComboBox-Steuerelement sinnvoll bei der Arbeit mit einem kleinen Satz von Daten (Dutzende von Car Teile).

## <a name="selecting-from-a-static-list-of-items"></a>Auswahl aus einer statischen Liste von Elementen

Lassen Sie s mit einer einfachen Beispiel zur Verwendung der ComboBox-Steuerelement zu starten. Stellen Sie sich vor, dass eine statische Liste von Elementen in einer Dropdownliste angezeigt werden soll. Sie möchten jedoch geöffnet bleiben die Gefahr, dass die Liste nicht vollständig ist. Möchten Sie einem Benutzer ermöglichen, einen benutzerdefinierten Wert in der Liste eingeben.

Wir ll, erstellen Sie eine neue ASP.NET Web Forms-Seite, und verwenden Sie das ComboBox-Steuerelement auf der Seite. Fügen Sie die neue ASP.NET-Seite zu Ihrem Projekt hinzu, und wechseln Sie zur Entwurfsansicht.

Wenn Sie das ComboBox-Steuerelement auf der Seite verwenden möchten, müssen Sie ein ScriptManager-Steuerelement auf der Seite hinzufügen. Ziehen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" auf die Oberfläche des Designers aus. Sie sollten das ScriptManager-Steuerelement am oberen Rand der Seite hinzufügen. können Sie ihn hinzufügen unmittelbar unterhalb der öffnenden serverseitige &lt;Formular&gt; Tag.

Als Nächstes ziehen Sie das ComboBox-Steuerelement auf die Seite. Sie finden das ComboBox-Steuerelement in der Toolbox mit den anderen AJAX-Steuerelement-Toolkit-Steuerelemente und -Extender (siehe Abbildung 1).


[![Einfaches Formular zum Erstellen einer Business-Karte](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Abbildung 01**: das ComboBox-Steuerelement aus der Toolbox auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image2.png))


Wir ll verwenden das ComboBox-Steuerelement, um eine statische Liste mit Auswahlmöglichkeiten anzuzeigen. Der Benutzer kann eine bestimmte Ebene der Spiciness für ihre Nahrungsmittel aus einer Liste von drei Optionen auswählen: leichte, Mittel und Hot (siehe Abbildung 2).


[![Auswahl aus einer statischen Liste von Elementen](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Abbildung 02**: Auswahl aus einer statischen Liste von Elementen ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image4.png))


Es gibt zwei Möglichkeiten, dass Sie diese Optionen für das ComboBox-Steuerelement hinzufügen können. Zuerst die Option Bearbeitungsoptionen Task beim Bewegen der Maus über dem Steuerelement in der Entwurfsansicht und der Element-Editor zu öffnen (siehe Abbildung 3).


[![ComboBox-Elemente bearbeiten](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Abbildung 03**: Bearbeiten von ComboBox-Elemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image6.png))


Die zweite Möglichkeit besteht, um die Liste der Elemente zwischen den öffnenden und schließenden hinzuzufügen &lt;Asp: ComboBox&gt; Tags in der Quellansicht. Die Seite im Codebeispiel 1 enthält die aktualisierte ComboBox, die die Liste der Elemente verfügt.

**1 – Static.aspx auflisten**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Beim Öffnen der Seite im Codebeispiel 1 können Sie eine der vorhandenen Optionen aus dem Kombinationsfeld auswählen. Das heißt, funktioniert der ComboBox wie ein DropDownList-Steuerelement.

Allerdings müssen Sie auch die Möglichkeit, eine neue Auswahl (z. B. Super Spicy), die nicht in der vorhandenen Liste eingeben. Daher funktioniert der ComboBox auch wie ein TextBox-Steuerelement.

Unabhängig davon, ob Sie ein bereits vorhandener auswählen, geben Sie Element oder Sie ein benutzerdefiniertes Element bei der Übermittlung des Formulars Ihrer Wahl in das Label-Steuerelement angezeigt wird. Wenn Sie das Formular, das BtnSubmit senden\_auf Handler ausgeführt wird, und aktualisiert die Bezeichnung (siehe Abbildung 4).


[![Das ausgewählte Element anzeigen](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Abbildung 04**: das ausgewählte Element anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image8.png))


Das Kombinationsfeld unterstützt die gleichen Eigenschaften wie das DropDownList-Steuerelement für das ausgewählte Element abrufen, nachdem ein Formular gesendet wird:

- SelectedItem.Text - zeigt den Wert der Text-Eigenschaft des ausgewählten Elements.
- SelectedItem.Value - zeigt den Wert der Value-Eigenschaft des ausgewählten Elements ab oder zeigt den Text in der ComboBox eingegeben.
- "SelectedValue" - identisch mit SelectedItem.Value, mit dem Unterschied, dass diese Eigenschaft Ihnen die ermöglicht Angabe den (ersten) standardmäßig ausgewählten Element.

Bei der Eingabe wird eine benutzerdefinierte Auswahl in der ComboBox klicken Sie dann die benutzerdefinierte Auswahl sowohl die SelectedItem.Value der SelectedItem.Text zugewiesen.

## <a name="selecting-the-list-of-items-from-the-database"></a>Die Liste der Elemente auswählen aus der Datenbank

Sie können die Liste der Elemente, die im Kombinationsfeld zeigt aus einer Datenbank abrufen. Beispielsweise können Sie das Kombinationsfeld an ein SqlDataSource-Steuerelement, einem ObjectDataSource-Steuerelement, einem LinqDataSource oder EntityDataSource binden.

Angenommen Sie, eine Liste von Filmen in einem Kombinationsfeld angezeigt werden soll. Die Liste von Filmen aus der Datenbanktabelle Filme abgerufen werden sollen. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine Seite mit dem Namen Movies.aspx
2. Fügen Sie ein ScriptManager-Steuerelement durch Ziehen von ScriptManager aus der Registerkarte "AJAX-Erweiterungen" in der Toolbox auf der Seite auf der Seite hinzu.
3. Fügen Sie ein ComboBox-Steuerelement durch Ziehen das Kombinationsfeld auf der Seite auf der Seite hinzu.
4. Zeigen Sie in der Entwurfsansicht mit der Maus über das ComboBox-Steuerelement, und wählen Sie die **Datenquelle auswählen** Aufgabe Option (siehe Abbildung 5). Der Konfigurations-Assistent wird gestartet.
5. In der **wählen Sie eine Datenquelle** Schritt wählen Sie die &lt;neue Datenquelle&gt; Option.
6. In der **wählen Sie einen Datenquellentyp** Schritt: Wählen Sie die Datenbank.
7. In der **wählen Sie Ihre Datenverbindung** Schritt: Wählen Sie die Datenbank (z. B. MoviesDB.mdf).
8. In der **Verbindungszeichenfolge in der Anwendungskonfigurationsdatei speichern** Schritt: Wählen Sie die Option zum Speichern der Verbindungszeichenfolge.
9. In der **konfigurieren Sie die Select-Anweisung** Schritt, wählen Sie die Datenbanktabelle Filme aus, und wählen Sie alle Spalten.
10. In der **Testabfrage** Schritt, klicken Sie auf "Fertig stellen".
11. In der **Datenquelle auswählen** Schritt wählen Sie den Titel für das Feld angezeigt und die Id-Spalte für die Daten Feld (siehe Abbildung).
12. Klicken Sie auf die Schaltfläche "OK", um den Assistenten zu schließen.


[![Auswählen einer Datenquelle](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Abbildung 05**: Auswählen einer Datenquelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image10.png))


[![Wählen die Datenfelder für Text- und Wertdaten](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Abbildung 06**: Wählen die Datenfelder für Text- und Wertdaten ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image12.png))


Nachdem Sie die oben beschriebenen Schritte abgeschlossen haben, ist das Kombinationsfeld an ein SqlDataSource-Steuerelement gebunden, die aus der Datenbanktabelle Filme Filme darstellt. Die Quelle für die Seite sieht wie folgt auflisten 2 (ich bereinigt, die die Formatierung etwas).

**Auflisten von 2 – Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Beachten Sie, dass das ComboBox-Steuerelement eine DataSourceID-Eigenschaft verfügt, die auf die SqlDataSource-Steuerelement zeigt. Wenn Sie die Seite in einem Browser öffnen, wird die Liste von Filmen aus der Datenbank angezeigt (siehe Abbildung 7). Sie können eine Auswahlliste einen Film aus der Liste, oder geben Sie einen neuen Film Sie Films in der ComboBox eingeben.


[![Anzeigen einer Liste von Filmen](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Abbildung 07**: Anzeigen einer Liste von Filmen ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Festlegen der DropDownStyle

Der ComboBox DropDownStyle-Eigenschaft können so ändern Sie das Verhalten des Kombinationsfelds. Diese Eigenschaft akzeptiert es mögliche Werte:

- DropDown - (Standardwert) das Kombinationsfeld zeigt eine Dropdownliste, wenn klicken Sie auf den Pfeil, und Sie können einen benutzerdefinierten Wert eingeben.
- Einfache - Kombinationsfeld zeigt eine Dropdownliste automatisch, und Sie können einen benutzerdefinierten Wert eingeben.
- DropDownList - funktioniert das Kombinationsfeld, wie ein DropDownList-Steuerelement.

Die verschiedenen zwischen Dropdownliste und einfach ist, wenn die Liste der Elemente angezeigt wird. Im Fall von einfachen wird die Liste angezeigt, sofort Wenn Sie den Fokus auf das Kombinationsfeld verschieben. Im Fall von Dropdownliste müssen Sie den Pfeil, um die Liste der Elemente finden Sie unter klicken.

Der DropDownList-Wert bewirkt, dass das ComboBox-Steuerelement funktioniert wie ein standard DropDownList-Steuerelement. Es ist jedoch hier ein wichtiger Unterschied. Ältere Versionen von Internet Explorer anzeigen eines DropDownList-Steuerelements mit einem unendlichen Z-Index, damit das Steuerelement vor jedem Steuerelement platziert davor angezeigt wird. Da der ComboBox HTML rendert &lt;Div&gt; Tag anstelle einer HTML &lt;wählen&gt; Tag, das Kombinationsfeld ordnungsgemäß respektiert Z-Reihenfolge.

## <a name="setting-the-autocompletemode"></a>Festlegen der AutoCompleteMode

Sie verwenden die ComboBox AutoCompleteMode-Eigenschaft, um anzugeben, was passiert, wenn ein Benutzer Text in der ComboBox eingibt. Diese Eigenschaft akzeptiert die folgenden möglichen Werte an:

- None: (Standardwert) der ComboBox bietet keines Verhalten automatisch zu vervollständigen.
- Vorschlagen - Kombinationsfeld zeigt die Liste, und das entsprechende Element in der Liste hervorgehoben (siehe Abbildung 8).
- Anfügen: ComboBox wird die Liste nicht angezeigt, und er fügt die übereinstimmenden Elements aus der Liste auf, was Sie eingegeben haben (siehe Abbildung 9).
- SuggestAppend - ComboBox sowohl zeigt die Liste an und fügt die übereinstimmende Element aus der Liste auf, was Sie eingegeben haben (siehe Abbildung 10).


[![Das Kombinationsfeld macht einen Vorschlag](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Abbildung 08**: das Kombinationsfeld macht einen Vorschlag ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image16.png))


[![ComboBox fügt die übereinstimmenden text](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Abbildung 09**: ComboBox fügt die übereinstimmenden Text ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image18.png))


[![Das Kombinationsfeld schlägt vor, und fügt](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Abbildung 10**: das Kombinationsfeld schlägt vor, und fügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](how-do-i-use-the-combobox-control-vb/_static/image20.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie das ComboBox-Steuerelement zu verwenden, um einen festen Satz von Elementen anzuzeigen. Wir gebunden ComboBox-Steuerelement, um einen statischen Satz von Elementen und in einer Datenbanktabelle. Schließlich haben Sie gelernt, wie das Verhalten der ComboBox zu ändern, indem Sie ihre DropDownStyle und AutoCompleteMode Eigenschaften festlegen.

>[!div class="step-by-step"]
[Zurück](how-do-i-use-the-combobox-control-cs.md)
