---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "Programmgesteuertes Festlegen von Parameterwerten für das ObjectDataSource (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm lernen wir Hinzufügen einer Methode zu unserer DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert und gibt Daten zurück. Im Beispiel wird dieser Parameter festgelegt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a009d57f97838feb5b4a3253c6de9a872a9e9ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>Programmgesteuertes Festlegen von Parameterwerten für das ObjectDataSource (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Beispiel-App herunterladen](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) oder [PDF herunterladen](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> In diesem Lernprogramm lernen wir Hinzufügen einer Methode zu unserer DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert und gibt Daten zurück. Im Beispiel wird dieser Parameter programmgesteuert festlegen.


## <a name="introduction"></a>Einführung

Wie wir gesehen, in haben der [vorherigen Lernprogramm](declarative-parameters-cs.md), eine Reihe von Optionen stehen für deklarativ übergeben von Parameterwerten an das ObjectDataSource-Methoden. Wenn der Wert des Parameters fest programmiert ist, ergibt sich aus der ein Websteuerelement auf der Seite oder befindet sich in einer anderen Quelle, die von einer Datenquelle gelesen werden kann `Parameter` Objekt, z. B., dass der Wert für den Eingabeparameter gebunden werden kann, ohne eine Codezeile schreiben zu müssen.

Möglicherweise gibt es Zeiten, jedoch, wenn der Wert des Parameters aus einer Quelle, die noch nicht berücksichtigt, von einem der integrierten Datenquelle stammen `Parameter` Objekte. Wenn unsere Website Benutzerkonten unterstützt, sollten wir die Parameter basierend auf den aktuell angemeldeten Besucher des Benutzer-ID festgelegt Oder es müssen möglicherweise den Parameterwert anpassen, vor dem Senden an zugrunde liegende Objekt das ObjectDataSource-Methode.

Wenn das ObjectDataSource `Select` Methode wird aufgerufen, löst das ObjectDataSource zunächst seine [Ereignis auswählen](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Zugrunde liegende Objekt das ObjectDataSource-Methode wird aufgerufen. Sobald das ObjectDataSource ist abgeschlossen [ausgewählte Ereignis](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) ausgelöst wird (Abbildung 1 zeigt diese Abfolge von Ereignissen). Die zugrunde liegende Objekt das ObjectDataSource-Methode übergebenen Parameterwerte festgelegt oder in einem Ereignishandler für angepasst werden können die `Selecting` Ereignis.


[![Das ObjectDataSource ausgewählte und Auswählen von Ereignisse auslösen vor und nach einem zugrunde liegenden Objekts-Methode wird aufgerufen](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Abbildung 1**: das ObjectDataSource `Selected` und `Selecting` Ereignisse auslösen vor und nach einem zugrunde liegenden Objekts-Methode aufgerufen wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


In diesem Lernprogramm lernen wir unsere DAL und BLL, die einen einzelnen Eingabeparameter akzeptiert eine Methode hinzugefügt `Month`, des Typs `int` und gibt eine `EmployeesDataTable` diejenigen Mitarbeiter, die ihre Mitarbeiter Anniversary in das angegebene-Objekt `Month`. Unserem Beispiel wird dieser Parameter programmgesteuert basierend auf dem aktuellen Monat zeigt eine Liste von "Employee Geburtstage in diesem Monat." festgelegt.

Fangen wir an!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Schritt 1: Hinzufügen einer Methode um`EmployeesTableAdapter`

In unserem ersten Beispiel wir müssen eine Möglichkeit, diejenigen Mitarbeiter abzurufen, deren `HireDate` in einem angegebenen Monat aufgetreten ist. Diese Funktionalität in Übereinstimmung mit unseren Architektur bereitstellen, wir zuerst erstellen Sie eine Methode in müssen `EmployeesTableAdapter` , der die richtigen SQL-Anweisung zugeordnet. Um dies zu erreichen, öffnen Sie zunächst die Northwind typisierte DataSet. Mit der rechten Maustaste auf die `EmployeesTableAdapter` Bezeichnung, und wählen Sie die Abfrage hinzufügen.


[![Fügen Sie eine neue Abfrage auf die EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer neuen Abfrage den `EmployeesTableAdapter` ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Wählen Sie eine SQL-Anweisung hinzufügen, die Zeilen zurückgibt. Sie beim Erreichen der angeben einer `SELECT` Anweisung Bildschirm standardmäßig `SELECT` -Anweisung für die `EmployeesTableAdapter` bereits geladen werden. Fügen Sie einfach die `WHERE` -Klausel: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/en-us/library/ms174420.aspx) wird eine T-SQL-Funktion, die einen bestimmtes Datumsteil gibt eine `datetime` Adresstyp ";" in diesem Fall verwenden wir die `DATEPART` den Monat des zurückzugebenden der `HireDate` Spalte.


[![Nur die Zeilen, in denen das HireDate-Spalte "Return" ist kleiner als oder gleich der @HiredBeforeDate Parameter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Abbildung 3**: Zurückgeben nur die Zeilen, in denen die `HireDate` Spalte ist kleiner als oder gleich der `@HiredBeforeDate` Parameter ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Abschließend ändern Sie die `FillBy` und `GetDataBy` Methodennamen auf `FillByHiredDateMonth` und `GetEmployeesByHiredDateMonth`zugeordnet.


[![Wählen Sie geeignetere Methodennamen als FillBy und GetDataBy aus](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Abbildung 4**: Wählen Sie besser geeignete Methode Namen als `FillBy` und `GetDataBy` ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen und zurück zu den DataSet-Entwurfsoberfläche. Die `EmployeesTableAdapter` enthalten sollte nun einen neuen Satz von Methoden für den Zugriff auf Mitarbeiter in einem angegebenen Monat eingestellt.


[![Die neuen Methoden werden in der DataSet-Entwurfsoberfläche angezeigt.](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Abbildung 5**: die neuen Methoden werden in der DataSet-Entwurfsoberfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Schritt 2: Hinzufügen der`GetEmployeesByHiredDateMonth(month)`Methode, um die Geschäftslogikschicht

Da unsere Anwendung Architektur verwendet eine Separate Ebene für die Geschäftslogik und die Daten Logik für den Zugriff auf müssen wir unsere BLL eine Methode hinzugefügt, die Aufrufe an die DAL abzurufenden Mitarbeiter vor einem bestimmten Datum eingestellt. Öffnen der `EmployeesBLL.cs` Datei, und fügen Sie die folgende Methode hinzu:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Wie bei anderen Methoden in dieser Klasse `GetEmployeesByHiredDateMonth(month)` einfach nach unten der DAL aufruft und die Ergebnisse zurückgegeben werden.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Schritt 3: Anzeigen von Mitarbeitern, deren Mitarbeiter Anniversary dieses Monats ist

Der letzte Schritt in diesem Beispiel werden diejenigen Mitarbeiter anzuzeigen, deren Mitarbeiter Anniversary in diesem Monat ist. Durch Hinzufügen einer GridView zum Starten der `ProgrammaticParams.aspx` auf der Seite der `BasicReporting` Ordner und fügen Sie eine neue ObjectDataSource als Datenquelle. Konfigurieren der ObjectDataSource verwenden die `EmployeesBLL` -Klasse mit der `SelectMethod` festgelegt `GetEmployeesByHiredDateMonth(month)`.


[![Verwenden Sie die EmployeesBLL-Klasse](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Abbildung 6**: Verwenden der `EmployeesBLL` Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![Wählen Sie aus der GetEmployeesByHiredDateMonth(month)-Methode](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Abbildung 7**: Wählen Sie aus der `GetEmployeesByHiredDateMonth(month)` Methode ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


Im letzte Bildschirm gefragt werden, geben Sie die `month` Parameter Wertquelle. Da wir diesen Wert programmgesteuert festgelegt werden, lassen, die die Quelle des Parameters auf den Standardwert None festgelegt aus, und klicken Sie auf ' Fertig stellen '.


[![Behalten Sie die Parameter Quelle keine](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Abbildung 8**: lassen Sie die Quelle Parametersatz auf None ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Dadurch entsteht eine `Parameter` Objekt in das ObjectDataSource `SelectParameters` -Auflistung, die nicht über einen angegebenen Wert verfügt.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Um diesen Wert programmgesteuert festzulegen, müssen wir einen Ereignishandler für das ObjectDataSource erstellen `Selecting` Ereignis. Um dies zu erreichen, wechseln Sie zur Entwurfsansicht, und doppelklicken Sie auf das ObjectDataSource. Alternativ wählen Sie das ObjectDataSource, wechseln Sie zu dem Eigenschaftenfenster, und klicken Sie auf das Blitzsymbol. Als Nächstes entweder Doppelklicken Sie in das Textfeld neben der `Selecting` Ereignis oder den Namen der Ereignishandler, die Sie verwenden möchten.


![Klicken Sie auf das Blitzsymbol im Eigenschaftenfenster in der Liste ein Websteuerelement-Ereignisse](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Abbildung 9**: Klicken Sie auf das Blitzsymbol im Eigenschaftenfenster in der Liste ein Websteuerelement-Ereignisse


Beide Ansätze fügen Sie einen neuen Ereignishandler für das ObjectDataSource `Selecting` Ereignis auf der Seite Code-Behind-Klasse. In diesem Ereignishandler können wir lesen und Schreiben in die Parameterwerte mit `e.InputParameters[parameterName]`, wobei  *`parameterName`*  ist der Wert des der `Name` Attribut in der `<asp:Parameter>` Tag (die `InputParameters` Auflistung kann auch sein Ordnungszahl, wie in indizierten `e.InputParameters[index]`). Festlegen der `month` Parameter auf den aktuellen Monat, fügen Sie Folgendes an der `Selecting` Ereignishandler:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Wenn der Zugriff auf diese Seite über einen Browser sehen wir, nur ein Mitarbeiter eingestellt wurde in diesem Monat (März) Laura Callahan, die in der Firma seit 1994 befunden hat.


[![Diejenigen Mitarbeiter, deren Geburtstage in diesem Monat angezeigt werden](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Abbildung 10**: diejenigen Mitarbeiter, deren Geburtstage dieses Monats werden angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Zusammenfassung

Das ObjectDataSource Parameterwerte deklarativ in der Regel festgelegt werden können ohne eine Codezeile ist es einfach, die Parameterwerte programmgesteuert festzulegen. Wir müssen lediglich ist, erstellen einen Ereignishandler für das ObjectDataSource `Selecting` Ereignis, das ausgelöst wird, bevor das zugrunde liegende Objekt-Methode aufgerufen wird, und manuelles der Werte für einen oder mehrere Parameter über festlegen die `InputParameters` Auflistung.

In diesem Lernprogramm wird im Abschnitt Basisberichte abgeschlossen. Die [nächsten Lernprogramm](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) filtern und Master / Detail-Szenarien-Abschnitt, in dem wir sehen uns Techniken für das Zulassen des Besuchers zum Filtern von Daten und Drilldown von einer Masterbericht einen Detailbericht müssen startet.

Viel Spaß beim Programmieren!

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Hilton Giesenow. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](declarative-parameters-cs.md)
[Weiter](displaying-data-with-the-objectdatasource-vb.md)
