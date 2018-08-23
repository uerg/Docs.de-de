---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Programmgesteuertes Festlegen der Parameterwerte (VB) dem ObjectDataSource-Steuerelement | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir das Hinzufügen einer Methode für unsere DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert und gibt Daten zurück. Im Beispiel wird dieser Parameter wird festgelegt...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f823d1db7f98dcbbef12d20df4a28e39fae0ac26
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823760"
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Programmgesteuertes Festlegen von Parameterwerten für das ObjectDataSource-Steuerelement (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunter](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) oder [PDF-Datei herunterladen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> In diesem Tutorial betrachten wir das Hinzufügen einer Methode für unsere DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert und gibt Daten zurück. Im Beispiel wird dieser Parameter programmgesteuert festlegen.


## <a name="introduction"></a>Einführung

Wie wir, in gesehen der [vorherigen Tutorial](declarative-parameters-vb.md), eine Reihe von Optionen für das Übergeben von Parameterwerten deklarativ an Methoden mit dem ObjectDataSource-Steuerelement verfügbar sind. Wenn der Wert des Parameters hartcodiert ist, stammt aus einer Websteuerelement auf der Seite oder befindet sich in einer anderen Quelle, die von einer Datenquelle gelesen werden kann `Parameter` Objekt zu verwenden, z. B., dass der Wert an den Eingabeparameter gebunden werden kann, ohne eine einzige Zeile Code schreiben zu müssen.

Möglicherweise gibt es jedoch vorkommen, wenn der Wert des Parameters aus einer Quelle, die noch nicht berücksichtigt, von einem der integrierten Datenquelle stammt `Parameter` Objekte. Wenn unsere Website Benutzerkonten unterstützt, sollten wir legen Sie den Parameter, die basierend auf dem aktuell angemeldeten des Besuchers Benutzer-ID Oder es muss der Wert des Parameters vor dem Senden an dem ObjectDataSource-Steuerelement zugrunde liegende Objekt die Methode anpassen.

Wenn dem ObjectDataSource-Steuerelement `Select` Methode wird aufgerufen, löst Sie zunächst dem ObjectDataSource-Steuerelement seine [auswählen-Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Anschließend wird die Methode mit dem ObjectDataSource-Steuerelement zugrunde liegende Objekt aufgerufen. Nach Abschluss des dem ObjectDataSource-Steuerelement [ausgewählte Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) ausgelöst wird (Abbildung 1 zeigt diese Abfolge von Ereignissen). Die Methode mit dem ObjectDataSource-Steuerelement zugrunde liegende Objekt übergebenen Parameterwerte werden festgelegt oder nach Anpassung können in einem Ereignishandler für die `Selecting` Ereignis.


[![Dem ObjectDataSource-Steuerelement ausgewählt und Auswählen von Ereignissen auslösen vor und nach einem zugrunde liegenden Objekts-Methode wird aufgerufen.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Abbildung 1**: der ObjectDataSource `Selected` und `Selecting` Fire Ereignisse vor und nach einem zugrunde liegenden Objekts-Methode wird aufgerufen ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))


In diesem Tutorial betrachten wir das Hinzufügen einer Methode für unsere DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert `Month`, des Typs `Integer` und gibt eine `EmployeesDataTable` diejenigen Mitarbeiter, die die stellenausschreibung Anniversary in das angegebene Objekt `Month`. In unserem Beispiel wird dieser Parameter programmgesteuert basierend auf dem aktuellen Monat mit einer Liste von "Employee Geburtstage in diesem Monat." festgelegt.

Fangen wir an!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Schritt 1: Hinzufügen einer Methode`EmployeesTableAdapter`

Unserem ersten Beispiel müssen wir eine Möglichkeit zum Abrufen von diejenigen Mitarbeiter hinzufügen, deren `HireDate` in einem angegebenen Monat aufgetreten ist. Diese Funktionalität gemäß unserer Architektur bereitstellen, wir zuerst erstellen Sie eine Methode in müssen `EmployeesTableAdapter` , der die richtigen SQL-Anweisung zugeordnet. Zu diesem Zweck öffnen Sie zunächst der typisierte Datasets Northwind. Mit der rechten Maustaste auf die `EmployeesTableAdapter` bezeichnen, und wählen Sie die Abfrage hinzufügen.


[![Fügen Sie eine neue Abfrage hinzu, um die EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Abbildung 2**: Fügen Sie eine neue Abfrage, die `EmployeesTableAdapter` ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))


Wählen Sie eine SQL­Anweisung hinzufügen, die Zeilen zurückgibt. Sie beim Erreichen der Specify eine `SELECT` Anweisung Bildschirm standardmäßig `SELECT` -Anweisung für die `EmployeesTableAdapter` werden bereits geladen. Fügen Sie einfach die `WHERE` Klausel: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/library/ms174420.aspx) wird eine T-SQL-Funktion, die einen bestimmtes Datumsteil gibt eine `datetime` geben; in diesem Fall verwenden wir `DATEPART` den Monat des zurückzugebenden der `HireDate` Spalte.


[![Nur die Zeilen, in dem das HireDate-Spalte "Return" ist kleiner als oder gleich der @HiredBeforeDate Parameter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Abbildung 3**: Zurückgeben nur die Zeilen, in denen die `HireDate` Spalte ist kleiner als oder gleich der `@HiredBeforeDate` Parameter ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))


Ändern Sie schließlich die `FillBy` und `GetDataBy` Methode zeichenfolgeneigenschaftsnamen `FillByHiredDateMonth` und `GetEmployeesByHiredDateMonth`bzw.


[![Wählen Sie geeignetere Methodennamen als FillBy und GetDataBy aus](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Abbildung 4**: Wählen Sie besser geeignete Methode Namen als `FillBy` und `GetDataBy` ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))


Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen und zurück auf das DataSet-Entwurfsoberfläche. Die `EmployeesTableAdapter` enthalten sollte nun einen neuen Satz von Methoden für den Zugriff auf Mitarbeiter in einem angegebenen Monat eingestellt.


[![Die neuen Methoden werden in das DataSet-Entwurfsoberfläche angezeigt.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Abbildung 5**: die neuen Methoden werden in des Datasets-Entwurfsoberfläche ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Schritt 2: Hinzufügen der`GetEmployeesByHiredDateMonth(month)`Methode, um die Geschäftslogikebene

Da unsere Anwendung Architektur verwendet eine Separate Ebene für die Geschäftslogik und Daten Logik zuzugreifen, müssen wir unsere BLL eine Methode hinzufügen, die Aufrufe auf die DAL zum Abrufen von Mitarbeitern vor einem bestimmten Datum eingestellt. Öffnen der `EmployeesBLL.vb` Datei, und fügen Sie die folgende Methode hinzu:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Wie bei unseren anderen Methoden in dieser Klasse `GetEmployeesByHiredDateMonth(month)` ruft einfach in die DAL nach unten, und die Ergebnisse zurückgibt.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Schritt 3: Anzeigen von Mitarbeitern, deren Einstellung Jahrestag in diesem Monat wird

Der letzte Schritt in diesem Beispiel werden die Mitarbeiter angezeigt, deren Einstellung Jahrestag in diesem Monat besteht. Durch das Hinzufügen einer GridView-Ansicht zum Starten der `ProgrammaticParams.aspx` auf der Seite die `BasicReporting` Ordner, und fügen Sie eine neue "ObjectDataSource" als Datenquelle hinzu. Konfigurieren Sie mit dem ObjectDataSource-Steuerelement die `EmployeesBLL` -Klasse mit der `SelectMethod` festgelegt `GetEmployeesByHiredDateMonth(month)`.


[![Verwenden Sie die EmployeesBLL-Klasse](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Abbildung 6**: Verwenden der `EmployeesBLL` Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))


[![Wählen Sie aus der GetEmployeesByHiredDateMonth(month)-Methode](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Abbildung 7**: Wählen Sie aus der `GetEmployeesByHiredDateMonth(month)` Methode ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))


Der letzten Seite fordert uns die Bereitstellung der `month` Parameterwert der Quelle. Da wir diesen Wert programmgesteuert festgelegt werden, lassen Sie die Quelle des Parameters auf den Standardwert None festgelegt aus, und klicken Sie auf "Fertig stellen".


[![Behalten Sie die Parameter die Quelle auf "None"](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Abbildung 8**: lassen Sie die Parameter Quelle festgelegt auf "None" ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))


Dadurch entsteht eine `Parameter` Objekt in das "ObjectDataSource" `SelectParameters` -Auflistung, die kein Wert angegeben ist.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Um diesen Wert programmgesteuert festzulegen, müssen wir einen Ereignishandler für das ObjectDataSource-Steuerelement erstellen `Selecting` Ereignis. Um dies zu erreichen, navigieren Sie zur Entwurfsansicht, und doppelklicken Sie auf dem ObjectDataSource-Steuerelement. Alternativ wählen Sie das "ObjectDataSource", wechseln Sie zu dem Fenster "Eigenschaften", und klicken Sie auf das Blitzsymbol. Als Nächstes entweder Doppelklicken Sie in das Textfeld neben der `Selecting` Ereignis- oder geben Sie den Namen der Ereignishandler, die Sie verwenden möchten. Als dritte Möglichkeit können Sie den Ereignishandler erstellen, indem Sie dem ObjectDataSource-Steuerelement auswählen und die zugehörige `Selecting` Ereignis der Dropdown-Listen am oberen Rand der Seite Code-Behind-Klasse.


![Klicken Sie auf das Blitzsymbol im Fenster "Eigenschaften" eines Websteuerelements Ereignisse auflisten](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Abbildung 9**: Klicken Sie auf das Blitzsymbol im Fenster "Eigenschaften" eines Websteuerelements Ereignisse auflisten


Alle drei Ansätze fügen Sie einen neuen Ereignishandler für das "ObjectDataSource" `Selecting` Ereignis auf der Seite Code-Behind-Klasse. In diesem Ereignishandler können gelesen und geschrieben werden, der Parameterwerte mit `e.InputParameters(parameterName)`, wobei *`parameterName`* ist der Wert des der `Name` -Attribut in der `<asp:Parameter>` Tag (die `InputParameters` Auflistung kann auch sein, Ordnungszahl, wie in indizierten `e.InputParameters(index)`). Festlegen der `month` fügen Sie den folgenden Parameter, um den aktuellen Monat die `Selecting` -Ereignishandler:


[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Beim Zugriff auf diese Seite über einen Browser sehen wir, nur ein Mitarbeiter eingestellt wurde in diesem Monat (März) Laura Callahan, die in der Firma seit 1994 wurde.


[![Diejenigen Mitarbeiter, deren Geburtstage in diesem Monat angezeigt werden](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Abbildung 10**: die Mitarbeiter, deren Geburtstage dieses Monats werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))


## <a name="summary"></a>Zusammenfassung

Während Werte für die der ObjectDataSource gewöhnlich deklarativ festgelegt werden können ohne eine einzige Zeile Code, ist es einfach, die Parameterwerte programmgesteuert festzulegen. Wir müssen lediglich einen Ereignishandler für das ObjectDataSource-Steuerelement erstellen `Selecting` -Ereignis, das ausgelöst wird, bevor die Methode des zugrunde liegenden Objekts aufgerufen wird, und Manuelles Festlegen der Werte für einen oder mehrere Parameter über die `InputParameters` Auflistung.

Dieses Lernprogramm schließt das Kapitel zur grundlegenden Berichterstellung. Die [nächsten Tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) startet im Abschnitt filtern und Master / Detail-Szenarien, in dem wir Techniken für das Zulassen des Besuchers zum Filtern von Daten und einen Drilldown aus einem master-Bericht in einen Bericht.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial ist Hilton Giesenow. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](declarative-parameters-vb.md)
