---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interaktion mit der Inhaltsseite von der Masterseite (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Untersucht, wie Sie rufen Methoden, Eigenschaften usw. von der Seite Inhalt im Code auf der Masterseite festgelegt.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 815752ee70eb761d7f9da24c9eada9d4c0c833a7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837454"
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interaktion mit der Inhaltsseite von der Masterseite (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Untersucht, wie Sie rufen Methoden, Eigenschaften usw. von der Seite Inhalt im Code auf der Masterseite festgelegt.


## <a name="introduction"></a>Einführung

Im vorherige Tutorial untersucht, wie programmgesteuerte Interaktion mit der Masterseite der Inhaltsseite. Beachten Sie, dass wir die Masterseite ein GridView-Steuerelement enthält, die die fünf zuletzt aufgeführt aktualisiert hinzugefügt Produkte. Wir erstellt dann eine Inhaltsseite, in der der Benutzer ein neues Produkt hinzufügen kann. Die Seite Inhalte musste beim Hinzufügen eines neuen Produkts, weisen Sie die Masterseite an die GridView zu aktualisieren, sodass sie das gerade hinzugefügte Produkt enthält. Diese Funktion wurde durch Hinzufügen einer öffentlichen Methode der Masterseite, aktualisiert die Daten an die GridView gebunden, und klicken Sie dann Aufrufen dieser Methode auf der Seite Inhalt erreicht.

Die häufigste Form des Inhalts und der Masterseite Interaktion stammt von der Seite Inhalt ab. Allerdings es ist möglich, für die Gestaltungsvorlage auf die Seite für den aktuelle Inhalte in Aktion Rouse verwaltet, und solche Funktionen notwendig sein, wenn die Masterseite Elemente der Benutzeroberfläche enthält, mit denen Benutzer Daten ändern, die auch auf der Seite Inhalt angezeigt wird. Erwägen Sie eine Inhaltsseite, zeigt die Informationen zu Produkten in einer GridView-Ansicht steuern und eine Masterseite, die eine Schaltfläche enthält steuern, die beim Klicken auf, verdoppelt die Preise für alle Produkte. Ähnlich wie im Beispiel im vorherigen Tutorial GridView muss nach der doppelten Preis aktualisiert werden, auf die Schaltfläche geklickt wird, damit die neuen Preise angezeigt, aber in diesem Szenario ist es die Masterseite, die auf die Seite Inhalte in Aktion Rouse verwaltet.

In diesem Tutorial erfahren Sie, wie die Masterseite, die in der Seite Inhalt definierte Funktion aufgerufen haben.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Clientabfragen das Programminteraktion über ein Ereignis und Ereignishandler zurückzugeben

Aufrufen von Funktionen der Seite "Inhalt" von einer Masterseite ist schwieriger als anders herum. Da eine Inhaltsseite eine einzige masterdseite hat, wenn die Programminteraktion auf der Seite Inhalt Clientabfragen zurückzugeben kennen wir öffentliche Methoden und Eigenschaften zur Verfügung. Eine Masterseite, haben jedoch viele verschiedene Inhaltsseiten, jeweils einen eigenen Satz von Eigenschaften und Methoden. Wie können Sie dann wir Code Schreiben in die Masterseite, führen Sie eine Aktion in ihre Inhaltsseite, wenn wir nicht wissen, welche Seite bis zur Laufzeit aufgerufen wird?

Erwägen Sie ein ASP.NET Web-Steuerelement, z. B. das Schaltflächen-Steuerelement. Ein Schaltflächen-Steuerelement kann auf eine beliebige Anzahl von ASP.NET-Seiten angezeigt werden und benötigt einen Mechanismus, mit dem der Seite Hinweisen zu können, dass sie bereits geklickt wurde. Dies erfolgt mit *Ereignisse*. Insbesondere das Schaltflächen-Steuerelement löst die `Click` Ereignis wenn darauf geklickt wird; die ASP.NET-Seite mit der Schaltfläche kann optional reagieren auf diese Benachrichtigung über eine *Ereignishandler*.

Dieses Muster kann verwendet werden, um eine Masterseite Trigger-Funktion auf die Inhaltsseiten zu erhalten:

1. Fügen Sie ein Ereignis, auf die Masterseite.
2. Auslösen des Ereignisses, wenn die Masterseite für die Kommunikation mit der Inhaltsseite benötigt. Z. B., wenn die Masterseite muss ihre Inhaltsseite aufmerksam zu machen, die der Benutzer die Preise verdoppelt hat, würde das Ereignis ausgelöst werden unmittelbar nach dem verdoppelt die Preise haben.
3. Erstellen Sie einen Ereignishandler auf diese Inhaltsseiten, die Maßnahmen ergreifen müssen.

Diese weiteren Verlauf dieses Tutorials implementiert das Beispiel in der Einführung beschrieben ist. Steuern eine Inhaltsseite, die die Produkte in der Datenbank aufgelistet werden und eine Masterseite, die eine Schaltfläche enthält, nämlich um die Preise doppelklicken.

## <a name="step-1-displaying-products-in-a-content-page"></a>Schritt 1: Darstellen von Produkten in einer Inhaltsseite

Unsere erste Tagesordnung ist eine Inhaltsseite erstellen, die die Produkte aus der Northwind-Datenbank aufgeführt sind. (Wir die Northwind-Datenbank zum Projekt hinzugefügt haben, im vorherigen Tutorial [ *Interaktion mit der Masterseite der Inhaltsseite*](interacting-with-the-master-page-from-the-content-page-cs.md).) Starten, indem Sie eine neue ASP.NET-Seite zum Hinzufügen der `~/Admin` Ordner mit dem Namen `Products.aspx`, und bindet, bindet es an der `Site.master` Masterseite. Abbildung 1 zeigt den Projektmappen-Explorer, nachdem dieser Seite auf der Website hinzugefügt wurde.


[![Fügen Sie eine neue ASP.NET-Seite, um den Ordner Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen einer neuen ASP.NET-Seite zu den `Admin` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Denken Sie daran, dass in der [ *Titel, Meta-Tags und anderer HTML-Header angeben, auf der Masterseite* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm eine benutzerdefinierten Basisseite-Klasse, die mit dem Namen erstellten `BasePage` , der den Titel der Seite generiert, wenn er nicht ist Festlegen Sie explizit. Wechseln Sie zu der `Products.aspx` Seite des Code-Behind-Klasse und deren abgeleitet `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie abschließend die `Web.sitemap` hinzu, um einen Eintrag in dieser Lektion einzubeziehen. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für den Inhalt zur Interaktion der Master-Seite Lektion:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements in den Lektionen wiedergegeben wird (siehe Abbildung 5).

Wechseln Sie zurück zur `Products.aspx`. Im Inhaltssteuerelement für `MainContent`, fügen Sie ein GridView-Steuerelement hinzu, und nennen Sie sie `ProductsGrid`. Die GridView zu binden, um ein neues SqlDataSource-Steuerelement, das mit dem Namen `ProductsDataSource`.


[![GridView zu binden, um ein neues SqlDataSource-Steuerelement](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Abbildung 02**: GridView zu binden, um ein neues SqlDataSource-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Konfigurieren Sie den Assistenten aus, sodass sie die Northwind-Datenbank verwendet. Wenn Sie das vorherige Tutorial durchgearbeitet haben Sie sollten bereits eine Verbindungszeichenfolge, die mit dem Namen `NorthwindConnectionString` in `Web.config`. Wählen Sie diese Verbindungszeichenfolge aus der Dropdown-Liste ein, wie in Abbildung 3 dargestellt.


[![Konfigurieren Sie die Verwendung der Northwind-Datenbank dem SqlDataSource-Steuerelement](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Abbildung 03**: Konfigurieren der Verwendung der Northwind-Datenbank dem SqlDataSource-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Geben Sie als Nächstes des Datenquellensteuerelements `SELECT` Anweisung durch die Products-Tabelle aus der Dropdown-Liste auswählen und Zurückgeben der `ProductName` und `UnitPrice` Spalten (siehe Abbildung 4). Klicken Sie auf Weiter, und schließen Sie zum Abschließen des Assistenten für die Datenquelle konfigurieren.


[![Zurückzugeben Sie die UnitPrice-Felder "und" ProductName, in der Tabelle Products](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Abbildung 04**: Zurückgeben der `ProductName` und `UnitPrice` Felder aus der `Products` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Das ist alles schon! Nach Abschluss des Assistenten fügt Visual Studio zwei BoundFields an die GridView, spiegeln die beiden Felder, die von dem SqlDataSource-Steuerelement zurückgegeben. Die GridView- und SqlDataSource-Steuerelemente Markup folgt. Abbildung 5 zeigt die Ergebnisse, wenn Sie über einen Browser angezeigt.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Jedes Produkt und seinem Preis finden Sie in den GridView-Ansicht](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Abbildung 05**: jedes Produkt und seinem Preis finden Sie in den GridView-Ansicht ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> Können Sie, um die Darstellung der GridView zu bereinigen. Einige Vorschläge enthalten die Formatierung des angezeigten UnitPrice-Wertes als Währung und mithilfe von Hintergrundfarben und Schriftarten zum Optimieren der Darstellung von des Rasters. Weitere Informationen zum Anzeigen und zum Formatieren von Daten in ASP.NET finden Sie in meinem [arbeiten mit Daten tutorialreihe](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Schritt 2: Hinzufügen einer doppelten Preise-Schaltfläche auf der Masterseite

Unsere nächste Aufgabe ist, hinzufügen ein Websteuerelements der Schaltfläche mit dem übergeordneten Seite, die beim Klicken auf doppelt auf den Preis aller Produkte in der Datenbank wird. Öffnen der `Site.master` Masterseite, und ziehen Sie eine Schaltfläche aus der Toolbox auf den Designer, und platzieren es unter der `RecentProductsDataSource` SqlDataSource-Steuerelement, die wir im vorherigen Tutorial hinzugefügt. Legen Sie die `ID` Eigenschaft `DoublePrice` und die zugehörige `Text` Eigenschaft auf "Doppelte Produkt-Preise".

Fügen Sie ein SqlDataSource-Steuerelement auf die Masterseite, nennen Sie es `DoublePricesDataSource`. Diese SqlDataSource-Steuerelement wird verwendet, um das Ausführen der `UPDATE` Anweisung, um alle Preise doppelklicken. Insbesondere müssen wir legen Sie dessen `ConnectionString` und `UpdateCommand` Eigenschaften auf die entsprechende Verbindungszeichenfolge und `UPDATE` Anweisung. Dann wir diese SqlDataSource-Steuerelement aufrufen müssen `Update` Methode bei der `DoublePrice` geklickt wird. Festlegen der `ConnectionString` und `UpdateCommand` Eigenschaften, wählen Sie das SqlDataSource-Steuerelement, und fahren Sie mit dem Fenster "Eigenschaften". Die `ConnectionString` Eigenschaftenlisten diese Verbindungszeichenfolgen, die bereits im gespeicherten `Web.config` wählen Sie in einem Dropdown-Liste; die `NorthwindConnectionString` wie in Abbildung 6 gezeigt.


[![Konfigurieren Sie die Verwendung der NorthwindConnectionString SqlDataSource-Steuerelement](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Abbildung 06**: dem SqlDataSource-Steuerelement zu verwendenden konfigurieren die `NorthwindConnectionString` ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Festlegen der `UpdateCommand` -Eigenschaft finden Sie im Fenster Eigenschaften die UpdateQuery-Option. Diese Eigenschaft bei Auswahl dieser Option wird eine Schaltfläche mit Auslassungszeichen angezeigt; Klicken Sie auf diese Schaltfläche, um das Dialogfeld Befehls- und Parameter-Editor dargestellt in Abbildung 7 angezeigt. Geben Sie die folgenden `UPDATE` -Anweisung in das Dialogfeld Textfeld:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Diese Anweisung ausgeführt wird, verdoppelt sich die `UnitPrice` Wert für jeden Datensatz in die `Products` Tabelle.


[![Legen Sie die UpdateCommand-Eigenschaft des SqlDataSource-Steuerelement](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Abbildung 07**: Legen Sie SqlDataSource `UpdateCommand` Eigenschaft ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Nach dem Festlegen dieser Eigenschaften können sollte die Schaltfläche "und" SqlDataSource-Steuerelemente deklaratives Markup ähnelt dem folgenden aussehen:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Übrig bleibt, rufen Sie die `Update` Methode bei der `DoublePrice` geklickt wird. Erstellen Sie eine `Click` -Ereignishandler für die `DoublePrice` Schaltfläche, und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Um diese Funktionalität testen zu können, finden Sie auf die `~/Admin/Products.aspx` Seite, die wir in Schritt 1 erstellt haben, und klicken Sie auf die Schaltfläche "Doppelte Produktpreisen". Auf die Schaltfläche ein Postback auslöst und führt die `DoublePrice` Schaltfläche `Click` -Ereignishandler verdoppelt die Preise für alle Produkte. Klicken Sie dann die Seite erneut gerendert wird, und das Markup zurückgegeben wird, und erneut im Browser angezeigt. GridView auf der Inhaltsseite, führt jedoch die gleichen Preisen wie vor die "doppelte Produktpreise" Schaltfläche geklickt wurde. Dies ist, weil die Daten zunächst in den GridView-Ansicht geladen, den Zustand im Ansichtszustand gespeichert werden haben, sodass sie nicht bei Postbacks geladen wird, anders. Wenn Sie finden Sie auf eine andere Seite, und fahren Sie dann mit der `~/Admin/Products.aspx` Seite sehen Sie die aktualisierten Preise.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Schritt 3: Wenn die Preise ein Ereignis auslösen werden verdoppelt.

Da die GridView in die `~/Admin/Products.aspx` Seite sofort spiegelt nicht die Verdoppelung der Preis, ein Benutzer eventuell verständlicherweise der Meinung, dass sie nicht auf die Schaltfläche "Doppelte Produktpreisen" geklickt haben, oder es hat nicht funktioniert. Sie können versuchen, auf die Schaltfläche einige weitere Male verdoppelt die Preise immer wieder neu. Zum Korrigieren wir benötigen das Raster in den Inhalt für die Seitenanzeige die neuen Preise unmittelbar nach dem sie verdoppelt werden.

Wie weiter oben in diesem Tutorial erläutert wird, müssen wir ein Ereignis in die Masterseite auslösen, wenn der Benutzer klickt auf die `DoublePrice` Schaltfläche. Ein Ereignis ist eine Möglichkeit für eine Klasse (ein Ereignisherausgeber), um einen anderen eine Reihe von anderen Klassen (die Ereignisabonnenten) informieren, die etwas Interessantes aufgetreten ist. In diesem Beispiel ist die Masterseite vom Herausgeber des Ereignisses; Diese Inhaltsseiten, die interessieren, wenn die `DoublePrice` geklickt wird als Abonnenten.

Eine Klasse abonniert ein Ereignis durch das Erstellen einer *Ereignishandler*, dies ist eine Methode, die als Reaktion auf das ausgelöste Ereignis ausgeführt wird. Der Herausgeber definiert die Ereignisse, die er löst aus, indem Sie definieren eine *Ereignisdelegaten*. Der Ereignisdelegat gibt an, welche Eingabeparameter, der der Ereignishandler akzeptieren muss. In .NET Framework Ereignisdelegaten nicht gibt keinen Wert zurück und akzeptiert zwei Parameter:

- Ein `Object`, identifiziert die Ereignisquelle und
- Eine abgeleitete Klasse `System.EventArgs`

Der zweite Parameter an einen Ereignishandler übergeben kann zusätzliche Informationen zum Ereignis enthalten. Während die Base `EventArgs` Klasse übergibt die nicht über alle Informationen, die .NET Framework umfasst eine Reihe von Klassen, die erweitern `EventArgs` und zusätzliche Eigenschaften umfassen. Z. B. eine `CommandEventArgs` Instanz wird übergeben, Ereignis-Handlern, die auf Antworten der `Command` -Ereignis, und enthält zwei informative Eigenschaften: `CommandArgument` und `CommandName`.

> [!NOTE]
> Weitere Informationen zum Erstellen, das Auslösen und Behandeln von Ereignissen, finden Sie unter [Ereignisse und Delegaten](https://msdn.microsoft.com/library/17sde2xt.aspx) und [Ereignisdelegaten in einfachen englische](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Zum definieren ein Ereignisses verwenden die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Da wir brauchen nur auf die Seite Inhalte warnen, wenn der Benutzer geklickt hat die `DoublePrice` Schaltfläche und müssen keine zusätzliche Informationen übergeben, verwenden wir den Ereignisdelegaten `EventHandler`, die definiert einen Ereignishandler, die akzeptiert, wie die zweite der Parameter ein Objekt des Typs `System.EventArgs`. Um das Ereignis auf der Masterseite zu erstellen, fügen Sie auf der Masterseite CodeBehind-Klasse die folgende Codezeile hinzu:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Der obige Code Fügt ein öffentliches Ereignis auf der Masterseite, die mit dem Namen `PricesDoubled`. Nun müssen wir lösen Sie dieses Ereignis nach dem verdoppelt die Preise haben. Zum Auslösen ein Ereignisses verwenden die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Wo *Absender* und *EventArgs* sind die Werte des Abonnenten-Ereignishandler übergeben werden sollen.

Update der `DoublePrice` `Click` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Wie zuvor die `Click` Ereignishandler startet, durch den Aufruf der `DoublePricesDataSource` SqlDataSource-Steuerelement `Update` Methode, um die Preise für alle Produkte doppelklicken. Danach hat es gibt zwei Hinzufügungen an den Ereignishandler. Zunächst wird die `RecentProducts` GridView Daten werden aktualisiert. Diese GridView Masterseite, die im vorherigen Tutorial hinzugefügt wurde, und zeigt die fünf am häufigsten kürzlich hinzugefügte Produkte. Wir müssen Sie dieses Raster zu aktualisieren, damit, dass die Just-in-verdoppelt Preise für die folgenden fünf Produkte angezeigt. Danach die `PricesDoubled` Ereignis wird ausgelöst. Ein Verweis auf die Masterseite selbst (`this`) wird an den Ereignishandler als die Ereignisquelle und einen leeren übermittelt `EventArgs` Objekt wird als Ereignisargumente gesendet.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Schritt 4: Behandeln des Ereignisses auf der Inhaltsseite

An diesem Punkt die Masterseite löst die `PricesDoubled` Ereignis immer die `DoublePrice` Schaltflächen-Steuerelement geklickt wird. Allerdings Dies ist nur die halbe Miete – wir benötigen zum Behandeln des Ereignisses auf dem Abonnenten. Dies umfasst zwei Schritte: Erstellen den Ereignishandler und Hinzufügen von Ereigniscode-verknüpfen, damit der Ereignishandler ausgeführt wird, wenn das Ereignis ausgelöst wird.

Zunächst erstellen Sie einen Ereignishandler namens `Master_PricesDoubled`. Aufgrund der wir definiert haben wie die `PricesDoubled` Ereignis in die Masterseite muss zwei Eingabeparameter für den Ereignishandler der Typen `Object` und `EventArgs`bzw. In der Ereignis-Handler-Aufruf die `ProductsGrid` GridView `DataBind` Methode, um die Daten in das Raster binden.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Der Code für den Ereignishandler ist abgeschlossen, aber wir aber erst verknüpfen Sie der Masterseite `PricesDoubled` Ereignis an diesen Ereignishandler. Der Abonnent verbindet ein Ereignis an einem Ereignishandler über die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Publisher* ist ein Verweis auf das Objekt, das das Ereignis bietet *EventName*, und *MethodName* ist der Name des ereignishandlers definiert, auf dem Abonnenten, die eine Signatur, entspricht die *EventDelegate*. Das heißt, wenn das Ereignis Delegieren ist `EventHandler`, klicken Sie dann *MethodName* muss der Name einer Methode auf dem Abonnenten, die keinen Wert zurück und nimmt zwei Eingabeparameter Typen sein `Object` und `EventArgs`, bzw.

Dieses Ereignis verknüpfen Code muss ausgeführt werden, auf der Seite zum ersten Mal besuchen und die nachfolgenden Postbacks und erfolgen soll, zu einem Zeitpunkt im Lebenszyklus Seite, der vorangestellt ist, wenn das Ereignis ausgelöst werden kann. Ein guter Zeitpunkt zum Hinzufügen von Ereignis verknüpfen Code ist in der PreInit-Phase, die sehr früh im Lebenszyklus Seite auftritt.

Open `~/Admin/Products.aspx` , und erstellen Sie eine `Page_PreInit` -Ereignishandler:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Zur Durchführung dieser verknüpfen Code benötigen wir einen programmgesteuerten Verweis auf die Masterseite der Inhaltsseite. Wie im vorherigen Tutorial erwähnt, gibt es zwei Möglichkeiten zur Verfügung:

- Durch das umwandeln, die lose typisierte `Page.Master` Eigenschaft in den Typ entsprechende Masterseite oder
- Durch das Hinzufügen einer `@MasterType` -Direktive in der `.aspx` Seite und dann die stark typisierte `Master` Eigenschaft.

Wir verwenden den zweiten Ansatz. Fügen Sie die folgenden `@MasterType` -Direktive am Anfang der deklarativen Markup der Seite:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Fügen Sie den folgenden Ereignis verknüpfen Code in die `Page_PreInit` -Ereignishandler:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Mit diesem Code werden, auf der Inhaltsseite GridView aktualisiert wird immer die `DoublePrice` geklickt wird.

Abbildungen 8 und 9 veranschaulichen dieses Verhalten auf. Abbildung 8 zeigt die Seite beim ersten Mal besucht hat. Beachten Sie, die Werte in beiden Preis der `RecentProducts` GridView (in der linken Spalte der Masterseite) und die `ProductsGrid` GridView (in der Seite Inhalt). Abbildung 9 zeigt die gleiche Seite unmittelbar nach der `DoublePrice` Schaltfläche geklickt wurde. Wie Sie sehen können, werden die neuen Preise sofort in beiden GridViews wiedergegeben.


[![Die erste preiswerten](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Abbildung 08**: der Preis Anfangswerte ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Die Just-Doubled Preise werden in der GridViews angezeigt.](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Abbildung 09**: The Just-Doubled Preise werden in der GridViews angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Im Idealfall eine Masterseite und die Inhaltsseiten sind vollständig voneinander getrennt, und Sie benötigen keine Interaktion. Aber wenn Sie haben eine Gestaltungsvorlage oder eine Inhaltsseite, das Daten anzeigt, die aus der Masterseite oder die Seite "Inhalt" geändert werden kann, dann müssen Sie möglicherweise haben Sie die Masterseite der Inhaltsseite (oder a-umgekehrt) zu warnen Wenn Daten geändert werden, sodass die Anzeige aktualisiert werden kann. Im vorherigen Tutorial wurde erläutert, wie eine Inhaltsseite, die programmgesteuerte Interaktion mit der Masterseite verfügen; In diesem Tutorial erläutert, wie Sie eine Masterseite initiieren die Interaktion.

Während das Programminteraktion zwischen einem Inhalt und die Masterseite der Inhalte oder Masterseite stammen kann, hängt das Interaktionsmuster verwendet den Ursprung. Die Unterschiede sind darauf zurückzuführen, eine Inhaltsseite eine einzige masterdseite, jedoch wurde eine Masterseite kann viele verschiedene Inhaltsseiten. Ein besserer Ansatz werden anstatt einer Masterseite, die direkte Interaktion mit einer Inhaltsseite zu verwenden, müssen die Masterseite lösen eine Ereignis, um zu signalisieren, dass eine Aktion stattgefunden hat. Diese Inhaltsseiten, die die Aktion interessieren, können Ereignishandler erstellen.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Ereignisse und Delegaten](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Übergeben von Informationen zwischen Inhalt und Masterseiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in den ASP.NET-Tutorials](../../data-access/index.md)

### <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neuestes Buch heißt [ *Sams Teach selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Suchi Banerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](interacting-with-the-master-page-from-the-content-page-cs.md)
> [Weiter](master-pages-and-asp-net-ajax-cs.md)
