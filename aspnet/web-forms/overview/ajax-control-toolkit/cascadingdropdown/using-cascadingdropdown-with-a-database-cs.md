---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Verwendung von CascadingDropDown mit einer Datenbank (c#) | Microsoft Docs
author: wenz
description: Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879126"
---
<a name="using-cascadingdropdown-with-a-database-c"></a>Verwendung von CascadingDropDown mit einer Datenbank (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)

> Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. Damit dies funktioniert muss ein spezielle Webdienst erstellt werden.


## <a name="overview"></a>Übersicht

Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Damit dies funktioniert muss ein spezielle Webdienst erstellt werden.

## <a name="steps"></a>Schritte

Erstens ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der &lt; `form` &gt; Element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

Im nächsten Schritt sind zwei DropDownList-Steuerelemente erforderlich. In diesem Beispiel verwenden wir die Hersteller- und erhalten Sie Informationen aus AdventureWorks, daher erstellen wir eine Liste für die verfügbaren Lieferanten und eine für die verfügbaren Kontakte:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

Anschließend müssen zwei CascadingDropDown Extender zur Seite hinzugefügt werden. Einer die erste (Anbieter) Liste füllt und anderen Knoten der zweiten (Kontaktliste) füllt. Die folgenden Attribute müssen festgelegt werden:

- `ServicePath`: Die URL eines Webdiensts, die Übermittlung von Einträge in der Liste
- `ServiceMethod`: Einträge in der Liste zu übermitteln Web-Methode
- `TargetControlID`: Die ID der Dropdownliste
- `Category`: Kategorie Informationen, die beim Aufruf der Web-Methode übermittelt wird
- `PromptText`: Der Text angezeigt, wenn Sie Daten asynchron vom Server zu laden
- `ParentControlID`: (optional), dass Trigger das Laden der aktuellen Liste der übergeordneten-Dropdownliste

Je nach Programmiersprache verwendet ändert sich der Name des Webdiensts fraglichen, aber alle anderen Attributwerte sind identisch. So sieht das CascadingDropDown-Element der ersten Dropdown-Liste aus:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

Müssen die Steuerelement-Extender für die zweite Liste festlegen, die `ParentControlID` Attribut, damit Elemente in der Liste "Kontakte" wählen einen Eintrag in der Liste Lieferanten veranlassen Sie, dass verknüpft werden.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

Die eigentliche Arbeit erfolgt dann im Webdienst, der wie folgt eingerichtet. Beachten Sie, dass die `[ScriptService]` Attribut verwendet wird, andernfalls kann nicht ASP.NET AJAX JavaScript-Proxy für den Zugriff auf die Methoden des Webdiensts von clientseitigem Skript-Code erstellen.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

Die Signatur der Webmethoden aufgerufen werden, indem CascadingDropDown lautet wie folgt:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

Daher muss der Rückgabewert ein Array des Typs sein `CascadingDropDownNameValue` vom Toolkit Steuerelement definiert sind. Die `GetVendors()` Methode ist recht einfach, zu implementieren: der Code eine Verbindung mit der AdventureWorks-Datenbank her und fragt die ersten 25 Hersteller. Der erste Parameter in der `CascadingDropDownNameValue` Konstruktor wird die Beschriftung des den Eintrag, der dem zweiten Ausdruck seinen Wert (Value-Attribut in der HTML- &lt; `option` &gt; Element). Hier ist der Code ein:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

Abrufen der zugeordneten Kontakte für einen Anbieter (Methodennamen: `GetContactsForVendor()`) ist ein wenig schwieriger. Erstens muss der Hersteller der in der ersten Dropdownliste ausgewählt wurde, bestimmt werden. Das Toolkit Steuerelement definiert eine Hilfsmethode für diese Aufgabe: die `ParseKnownCategoryValuesString()` Methode gibt ein `StringDictionary` Element mit den Dropdown-Daten:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

Aus Gründen der Sicherheit müssen diese Daten zuerst überprüft werden. Wenn ein Hersteller-Eintrag vorhanden ist (da die `Category` Eigenschaft des ersten Elements CascadingDropDown auf festgelegt ist `"Vendor"`), die dem ausgewählten Hersteller-ID abgerufen werden kann:

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

Der Rest der Methode ist ziemlich selbsterklärend, klicken Sie dann auf. Der Hersteller-ID wird als Parameter für eine SQL-Abfrage verwendet, die alle zugeordneten Kontakte für Anbieter abruft. Noch einmal: die Methode gibt ein Array vom Typ `CascadingDropDownNameValue`.

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

Laden die ASP.NET-Seite, und die Hersteller-Liste ist nach kurzer Zeit mit 25 Einträge gefüllt. Wählen Sie einen Eintrag, und beachten Sie, wie der zweiten Dropdownliste mit Daten gefüllt wird.


[![Die erste Liste wird automatisch ausgefüllt.](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)

Die erste Liste wird automatisch ausgefüllt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-cascadingdropdown-with-a-database-cs/_static/image3.png))


[![Die zweite Liste wird entsprechend der Auswahl in der ersten Liste gefüllt.](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)

Die zweite Liste entsprechend der Auswahl in der ersten Liste gefüllt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-cascadingdropdown-with-a-database-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Zurück](filling-a-list-using-cascadingdropdown-cs.md)
> [Weiter](presetting-list-entries-with-cascadingdropdown-cs.md)
