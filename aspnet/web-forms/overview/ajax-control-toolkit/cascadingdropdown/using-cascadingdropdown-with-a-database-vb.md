---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Verwenden von CascadingDropDown mit einer Datenbank (VB) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 98e07764a3bd6afc8045221e9c016e57be44f5f7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834010"
---
<a name="using-cascadingdropdown-with-a-database-vb"></a>Verwenden von CascadingDropDown mit einer Datenbank (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. Damit dies funktioniert muss ein spezielle Web-Dienst erstellt werden.


## <a name="overview"></a>Übersicht

Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. (Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Damit dies funktioniert muss ein spezielle Web-Dienst erstellt werden.

## <a name="steps"></a>Schritte

Zunächst einmal ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist eine optionale Komponente von einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download unter [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) und fügen Sie der `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung. Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der &lt; `form` &gt; Element):

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

Im nächsten Schritt müssen zwei DropDownList-Steuerelementen. In diesem Beispiel verwenden wir die Hersteller- und wenden Sie sich an Informationen aus den AdventureWorks, daher erstellen wir eine Liste für die verfügbaren Anbieter und eine für die verfügbaren Kontakte aus:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Anschließend müssen zwei CascadingDropDown-Extender zur Seite hinzugefügt werden. Einer die erste (Anbieter)-Liste füllt und anderen Knoten die zweite (Kontakte)-Liste füllt. Die folgenden Attribute müssen festgelegt werden:

- `ServicePath`: Die URL eines Webdiensts, der Einträge in der Liste bereitstellen
- `ServiceMethod`: Bereitstellen der Einträge in der Liste Web-Methode
- `TargetControlID`: Die ID von der Dropdown-Liste
- `Category`: Kategorie Informationen, die beim Aufruf der Web-Methode übermittelt wird
- `PromptText`: Der Text angezeigt wird, wenn Sie Daten aus der Liste asynchron vom Server geladen
- `ParentControlID`: (optional), dass Trigger das Laden der aktuellen Liste der übergeordneten-Dropdownliste

Abhängig von der Programmiersprache, der verwendet wird ändert sich der Name des betreffenden-Webdiensts, aber alle anderen Attributwerte sind identisch. So sieht das CascadingDropDown-Element für das erste Dropdown-Liste aus:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Die Steuerelement-Extendern, für die zweite Liste müssen festlegen, die `ParentControlID` Attribut, damit Sie einen Eintrag auswählen, in der Anbieter Auflisten von Triggern Laden von Elementen in der Kontaktliste verknüpft ist.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Die eigentliche Arbeit im Webdienst, erfolgt das folgendermaßen eingerichtet wird. Beachten Sie, dass die `[ScriptService]` Attribut wird verwendet, andernfalls ASP.NET AJAX kann nicht erstellt werden die JavaScript-Proxy für den Zugriff auf den Webmethoden aus dem clientseitigen Skriptcode.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

Die Signatur der Webmethoden CascadingDropDown azurenode lautet wie folgt aus:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Daher der Rückgabewert muss ein Array vom Typ `CascadingDropDownNameValue` die vom Toolkit-Steuerelement definiert ist. Die `GetVendors()` Methode ist recht einfach zu implementieren: der Code eine Verbindung mit der AdventureWorks-Datenbank her und fragt die ersten 25 Hersteller. Der erste Parameter in der `CascadingDropDownNameValue` Konstruktor ist die Beschriftung des den Eintrag, der zweite Parameter der Wert (Value-Attribut in HTML &lt; `option` &gt; Element). Hier ist der Code ein:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Abrufen von zugeordneten Kontakte für einen Anbieter (Methodenname: `GetContactsForVendor()`) ist ein wenig komplizierter. Als Erstes muss der Anbieter, die in der ersten Dropdownliste ausgewählt wurden, hat bestimmt werden. Das Steuerelement-Toolkit definiert eine Hilfsmethode für diese Aufgabe: die `ParseKnownCategoryValuesString()` Methode gibt eine `StringDictionary` Element mit den Dropdown-Daten:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Aus Sicherheitsgründen müssen diese Daten zuerst überprüft werden. Wenn ein Anbieter-Eintrag vorhanden ist (da die `Category` des ersten Elements CascadingDropDown-Eigenschaftensatz auf `"Vendor"`), die dem ausgewählten Hersteller-ID kann abgerufen werden:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Der Rest der Methode ist ziemlich selbsterklärend, klicken Sie dann auf. Des Lieferanten-ID wird als Parameter für eine SQL-Abfrage verwendet, die alle zugeordneten Kontakte für diesen Kreditor abruft. Auch hier die Methode gibt ein Array vom Typ `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Laden die ASP.NET-Seite und nach einer kurzen Zeit wird die Liste der Anbieter mit 25 Einträgen gefüllt. Wählen Sie einen Eintrag, und beachten Sie, wie der zweiten Dropdownliste mit Daten gefüllt ist.


[![Die erste Liste wird automatisch ausgefüllt.](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Die erste Liste wird automatisch übernommen ([klicken Sie, um das Bild in voller Größe anzeigen](using-cascadingdropdown-with-a-database-vb/_static/image3.png))


[![Die zweite Liste wird entsprechend der Auswahl in der ersten Liste gefüllt.](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Die zweite Liste wird entsprechend der Auswahl in der ersten Liste gefüllt ([klicken Sie, um das Bild in voller Größe anzeigen](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Zurück](filling-a-list-using-cascadingdropdown-vb.md)
> [Weiter](presetting-list-entries-with-cascadingdropdown-vb.md)
