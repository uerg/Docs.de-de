---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
title: Interaktion mit der Seite Inhalt aus der Gestaltungsvorlage (c#) | Microsoft Docs
author: rick-anderson
description: Untersucht, wie Methoden aufrufen, legen Sie Eigenschaften usw. der Inhaltsseite aus Code in der Masterseite.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 3282df5e-516c-4972-8666-313828b90fb5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 923d291d84a47e64b31d99bcb13cfe53e5806444
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="interacting-with-the-content-page-from-the-master-page-c"></a>Interaktion mit der Seite Inhalt aus der Gestaltungsvorlage (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_CS.pdf)

> Untersucht, wie Methoden aufrufen, legen Sie Eigenschaften usw. der Inhaltsseite aus Code in der Masterseite.


## <a name="introduction"></a>Einführung

Das vorherigen Lernprogramm untersucht, wie programmgesteuerte Interaktion mit Gestaltungsvorlage Inhaltsseite haben. Wie bereits erwähnt, wurden die Gestaltungsvorlage Einbeziehung ein GridView-Steuerelements, das die fünf zuletzt aufgeführt hinzugefügt Produkte. Danach haben wir eine Inhaltsseite, in der der Benutzer ein neues Produkt hinzufügen konnte, erstellt. Inhaltsseite musste beim Hinzufügen eines neuen Produkts, weisen Sie die Gestaltungsvorlage an die GridView zu aktualisieren, damit, dass das Produkt gerade hinzugefügten enthalten würde. Diese Funktionalität wurde erreicht, indem die Gestaltungsvorlage, aktualisiert die Daten an die GridView gebunden eine öffentliche Methode hinzugefügt, und dann Aufrufen dieser Methode aus der Seite Inhalt.

Die am häufigsten verwendete Form des Inhalts und der Gestaltungsvorlage Interaktion stammt aus der Seite Inhalt. Allerdings es ist möglich, für die Gestaltungsvorlage auf die Seite für den aktuellen Inhalte in Aktion Rouse verwaltet, und solche Funktionen notwendig sein, wenn die Gestaltungsvorlage Elemente der Benutzeroberfläche enthält, mit denen Benutzer Daten ändern, die auch auf der Seite Inhalt angezeigt wird. Nehmen wir eine Inhaltsseite, zeigt der Produkte in einer GridView steuern und eine Masterseite, die eine Schaltfläche enthält steuern, die beim Klicken auf, verdoppelt die Preise aller Produkte. Ähnlich wie das Beispiel in dem vorherigen Lernprogramm gilt die GridView muss nach der doppelten Preis aktualisiert werden, auf die Schaltfläche geklickt wird, damit die neuen Preise angezeigt, aber in diesem Szenario ist es die Gestaltungsvorlage, die auf die Seite Inhalte in Aktion Rouse verwaltet muss.

In diesem Lernprogramm wird untersucht, wie die Gestaltungsvorlage in der Inhaltsseite definierte Funktion aufgerufen haben.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Betreffende programmgesteuerte Interaktion über ein Ereignis und Ereignishandler

Aufrufen der Inhaltsseite Funktionen von einer Masterseite ist schwieriger als umgekehrt. Da eine Inhaltsseite eine Masterseite hat, wenn die programmgesteuerte Interaktion von Inhaltsseite Clientabfragen zurückzugeben kennen wir öffentliche Methoden und Eigenschaften zur Verfügung. Eine Masterseite kann jedoch vielen verschiedenen Inhaltsseiten, jeweils über einen eigenen Satz von Eigenschaften und Methoden verfügen. Wie kann, wir schreiben dann Code in die Masterseite führen Sie eine Aktion in der Inhaltsseite, wenn es nicht wissen, welche Inhaltsseite bis zur Laufzeit aufgerufen wird?

Erwägen Sie eine ASP.NET Web-Steuerelement, z. B. das Schaltflächen-Steuerelement aus. Ein Button-Steuerelement kann auf eine beliebige Anzahl von ASP.NET-Seiten angezeigt werden und benötigt einen Mechanismus, mit dem sie der Seite Warnungen erhalten kann, dass er auf den geklickt wurde. Dies erfolgt mit *Ereignisse*. Insbesondere das Schaltflächen-Steuerelement löst die `Click` Ereignis wenn darauf geklickt wird; die ASP.NET-Seite mit der Schaltfläche optional auf reagieren kann diese Benachrichtigung über eine *Ereignishandler*.

Dieses gleiche Muster kann verwendet werden, eine Masterseite Trigger-Funktionalität in seiner Inhaltsseiten haben:

1. Fügen Sie ein Ereignis der Masterseite.
2. Lösen Sie das Ereignis, wenn die Gestaltungsvorlage für die Kommunikation mit der Seite Inhalt muss. Z. B. wenn die Gestaltungsvorlage muss seine Inhaltsseite ein Hinweis, dass der Benutzer die Preise verdoppelt hat, würde seine Ereignis ausgelöst werden unmittelbar nach dem verdoppelt die Preise aufweisen.
3. Erstellen Sie einen Ereignishandler in diese Inhaltsseiten, die Maßnahmen ergreifen müssen.

Diese Rest dieses Lernprogramms implementiert das Beispiel beschrieben, die in der Einführung. steuern, nämlich Inhaltsseite, die die Produkte in der Datenbank aufgeführt sind und einer Masterseite, die eine Schaltfläche enthält, um die Preise doppelklicken.

## <a name="step-1-displaying-products-in-a-content-page"></a>Schritt 1: Anzeigen von Produkten in einer Inhaltsseite

Erste Order Of Geschäftsverkehr ist auf eine Inhaltsseite zu erstellen, die die Produkte aus der Northwind-Datenbank aufgeführt sind. (Wir dem Projekt die Northwind-Datenbank hinzugefügt, in dem vorherigen Lernprogramm [ *Interaktion mit der Masterseite von der Inhaltsseite*](interacting-with-the-master-page-from-the-content-page-cs.md).) Starten, indem Sie eine neue ASP.NET-Seite, um die `~/Admin` Ordner mit dem Namen `Products.aspx`, sodass Sie sicher, dass so binden Sie es der `Site.master` Masterseite. Abbildung 1 zeigt im Projektmappen-Explorer aus, nachdem dieser Seite auf der Website hinzugefügt wurde.


[![Fügen Sie eine neue ASP.NET-Seite, um den Ordner Admin](interacting-with-the-content-page-from-the-master-page-cs/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image1.png)

**Abbildung 01**: Fügen Sie eine neue ASP.NET-Seite, um die `Admin` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image3.png))


Denken Sie daran, dass in der [ *Titel, Meta-Tags und andere HTML-Header angeben, der Gestaltungsvorlage* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) Lernprogramm wir eine benutzerdefinierte Basisseite-Klasse, die mit dem Namen erstellt `BasePage` , die den Titel der Seite generiert wird, ist er nicht Festlegen Sie explizit. Wechseln Sie zu der `Products.aspx` Seite des Code-Behind-Klasse, und Ihr abgeleitet sein `BasePage` (anstelle von aus `System.Web.UI.Page`).

Aktualisieren Sie schließlich die `Web.sitemap` Datei, die einen Eintrag in dieser Lektion hinzugefügt. Fügen Sie das folgende Markup unterhalb der `<siteMapNode>` für den Inhalt zur Interaktion der Master-Seite Lektion:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample1.xml)]

Das Hinzufügen dieses `<siteMapNode>` Elements in den Lektionen wiedergegeben wird (siehe Abbildung 5).

Zurück zu `Products.aspx`. Im Inhaltssteuerelement für `MainContent`, fügen Sie eine GridView-Steuerelement hinzu, und nennen Sie sie `ProductsGrid`. Die GridView zu binden, um ein neues SqlDataSource-Steuerelement mit dem Namen `ProductsDataSource`.


[![Die GridView zu binden, zu einem neuen SqlDataSource-Steuerelement](interacting-with-the-content-page-from-the-master-page-cs/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image4.png)

**Abbildung 02**: GridView zu binden, um ein neues SqlDataSource-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image6.png))


Konfigurieren Sie den Assistenten so, dass sie die Northwind-Datenbank verwendet. Wenn Sie das vorherige Lernprogramm durchgearbeitet haben Sie bereits eine Verbindungszeichenfolge mit dem Namen `NorthwindConnectionString` in `Web.config`. Wählen Sie diese Verbindungszeichenfolge aus der Dropdown-Liste, wie in Abbildung 3 gezeigt.


[![Konfigurieren Sie die SqlDataSource Verwendung der Northwind-Datenbank](interacting-with-the-content-page-from-the-master-page-cs/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image7.png)

**Abbildung 03**: Konfigurieren der SqlDataSource Verwendung der Northwind-Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image9.png))


Geben Sie anschließend des Datenquellensteuerelements `SELECT` Erklärung die Products-Tabelle aus der Dropdown-Liste auswählen und die Rückgabe der `ProductName` und `UnitPrice` Spalten (siehe Abbildung 4). Klicken Sie auf Weiter, und beenden Sie dann zum Abschließen des Assistenten für die Datenquelle konfigurieren.


[![ProductName und UnitPrice Felder aus der Tabelle Products zurückgegeben.](interacting-with-the-content-page-from-the-master-page-cs/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image10.png)

**Abbildung 04**: Zurückgeben der `ProductName` und `UnitPrice` Felder aus der `Products` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image12.png))


Das ist alles vorhanden ist! Nach Abschluss des Assistenten fügt Visual Studio zwei BoundFields an die GridView, die zwei Felder zurückgegebenes SqlDataSource-Steuerelement zu spiegeln. Die GridView und SqlDataSource-Steuerelemente Markup folgt. Abbildung 5 zeigt die Ergebnisse, wenn Sie über einen Browser angezeigt.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample2.aspx)]


[![Jedes Produkt ihren Preis aufgeführt, die GridView](interacting-with-the-content-page-from-the-master-page-cs/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image13.png)

**Abbildung 05**: jedes Produkt ihren Preis aufgeführt, die GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image15.png))


> [!NOTE]
> Wahlweise können die Darstellung der GridView bereinigen. Einige Vorschläge enthalten die Formatierung des angezeigten UnitPrice-Wertes als Währung und mithilfe von Hintergrundfarben und Schriftarten zum Optimieren der Darstellung des Datenblatts. Weitere Informationen zum Anzeigen und Formatieren von Daten in ASP.NET finden Sie in meinem [arbeiten mit Tutorial Datenreihe](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Schritt 2: Hinzufügen einer doppelten Preise-Schaltfläche auf der Seite "Master"

Unsere nächste Aufgabe ist das Hinzufügen einer Schaltfläche Websteuerelement mit dem übergeordneten Seite, die beim Klicken auf doppelt auf den Preis aller Produkte in der Datenbank wird. Öffnen der `Site.master` Seite "master", und ziehen Sie eine Schaltfläche aus der Toolbox in den Designer, und platzieren es unterhalb der `RecentProductsDataSource` SqlDataSource-Steuerelement, die wir im vorherigen Lernprogramm hinzugefügt. Legen Sie der Schaltfläche `ID` Eigenschaft `DoublePrice` und seine `Text` -Eigenschaft auf "Doppelte Produktpreise".

Fügen Sie ein SqlDataSource-Steuerelement auf der Masterseite benennen `DoublePricesDataSource`. Diesen SqlDataSource wird zum Ausführen der `UPDATE` Anweisung alle Preise verdoppeln. Insbesondere müssen wir Festlegen seiner `ConnectionString` und `UpdateCommand` Eigenschaften der entsprechenden Verbindungszeichenfolge und `UPDATE` Anweisung. Wir müssen Sie diesen SqlDataSource-Steuerelement aufrufen `Update` Methode bei der `DoublePrice` Schaltfläche geklickt wird. Festlegen der `ConnectionString` und `UpdateCommand` Eigenschaften, wählen Sie das SqlDataSource-Steuerelement, und fahren Sie mit dem Eigenschaftenfenster. Die `ConnectionString` Eigenschaft aufgeführt, die bereits in gespeicherten Verbindungszeichenfolgen `Web.config` in einer Dropdownliste; wählen Sie die `NorthwindConnectionString` option wie in Abbildung 6 veranschaulicht.


[![Konfigurieren Sie die SqlDataSource Verwendung der NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-cs/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image16.png)

**Abbildung 06**: Konfigurieren der SqlDataSource verwenden die `NorthwindConnectionString` ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image18.png))


Festlegen der `UpdateCommand` -Eigenschaft, suchen Sie im Fenster Eigenschaften die UpdateQuery-Option. Diese Eigenschaft bei Auswahl dieser Option wird eine Schaltfläche mit Auslassungszeichen angezeigt; Klicken Sie auf diese Schaltfläche, um das Dialogfeld Befehls- und Parameter-Editor, siehe Abbildung 7 anzuzeigen. Geben Sie Folgendes `UPDATE` Anweisung in das Dialogfeld Textfeld ein:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample3.sql)]

Bei der Ausführung dieser Anweisung Doppelklicken wird die `UnitPrice` Wert für jeden Datensatz in der `Products` Tabelle.


[![Festlegen des SqlDataSource UpdateCommand-Eigenschaft](interacting-with-the-content-page-from-the-master-page-cs/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image19.png)

**Abbildung 07**: Legen Sie SqlDataSource `UpdateCommand` Eigenschaft ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image21.png))


Nach dem Festlegen dieser Eigenschaften, sollte die Schaltfläche und SqlDataSource-Steuerelemente deklarativem Markup ähnlich der folgenden aussehen:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample4.aspx)]

Alle bleibt besteht im Aufrufen seiner `Update` Methode bei der `DoublePrice` Schaltfläche geklickt wird. Erstellen einer `Click` -Ereignishandler für die `DoublePrice` Schaltfläche und fügen Sie den folgenden Code hinzu:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample5.cs)]

Um diese Funktion testen möchten, besuchen Sie die `~/Admin/Products.aspx` Seite, die wir in Schritt 1 erstellt haben, und klicken Sie auf die Schaltfläche "Doppelte Produktpreise". Auf die Schaltfläche einen Postback verursacht und führt die `DoublePrice` Schaltfläche `Click` Ereignishandler, d. h. verdoppelt die Preise aller Produkte. Klicken Sie dann die Seite erneut gerendert wird, und das Markup zurückgegeben und erneut im Browser angezeigt. Die GridView auf der Inhaltsseite aufgeführt jedoch die gleichen Preise vor "Doppelte Produktpreise" geklickt wurde. Dies ist, da die Daten in die GridView anfänglich geladenen den Zustand im Ansichtszustand gespeichert wird hatte, sodass es nicht bei Postbacks neu geladen wird, wenn andernfalls aufgefordert. Wenn Sie eine andere Seite besuchen und wieder die `~/Admin/Products.aspx` Seite sehen Sie die aktualisierte Preise.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Schritt 3: Wenn die Preise ein Ereignis auslösen werden verdoppelt.

Da die GridView in die `~/Admin/Products.aspx` Seite nicht sofort reflektiert der Preis verdoppelt, ein Benutzer eventuell verständlicherweise der Meinung, dass sie nicht auf die Schaltfläche "Doppelte Produktpreise" geklickt haben oder nicht erfolgreich war. Sie können versuchen, auf die Schaltfläche weitere Male verdoppelt die Preise wieder. Zum Beheben dieses wir das Raster im Inhalt müssen für die Seitenanzeige der neuen Preise sofort, nachdem sie verdoppelt werden.

Wie weiter oben in diesem Lernprogramm erläutert wird, müssen wir ein Ereignis in die Masterseite auslösen, wenn der Benutzer klickt auf die `DoublePrice` Schaltfläche. Ein Ereignis ist eine Möglichkeit für eine Klasse (ein Ereignisherausgeber), ein anderes eine Reihe von anderen Klassen (Ereignisabonnenten) benachrichtigt wird, die etwas Interessantes aufgetreten ist. In diesem Beispiel wird die Gestaltungsvorlage der Ereignisherausgeber; Diese Inhaltsseiten für, wenn interessieren die `DoublePrice` geklickt wird als Abonnenten.

Eine Klasse abonniert ein Ereignis durch das Erstellen einer *Ereignishandler*, dies ist eine Methode, die als Antwort auf ausgelösten Ereignisses ausgeführt wird. Der Verleger definiert die Ereignisse, die er durch definieren löst einer *Ereignisdelegaten*. Der Ereignisdelegat gibt an, welche Eingabeparameter der Ereignishandler zustimmen muss. In .NET Framework Ereignisdelegaten keinen Wert zurückgeben und akzeptieren zwei Eingabeparameter:

- Ein `Object`, identifiziert die Ereignisquelle und
- Eine abgeleitete Klasse`System.EventArgs`

Der zweite Parameter, die an einen Ereignishandler übergeben kann zusätzliche Informationen zum Ereignis enthalten. Während die Base `EventArgs` Klasse übergibt nicht auf alle Informationen, die .NET Framework enthält eine Reihe von Klassen, mit denen erweitert `EventArgs` und umfasst zusätzliche Eigenschaften. Z. B. eine `CommandEventArgs` Instanz an, auf die reagiert Ereignishandler übergeben wird die `Command` Ereignis, und enthält zwei informative Eigenschaften: `CommandArgument` und `CommandName`.

> [!NOTE]
> Weitere Informationen zum Erstellen, durch das Auslösen und Behandeln von Ereignissen finden Sie unter [Ereignissen und Delegaten](https://msdn.microsoft.com/en-us/library/17sde2xt.aspx) und [Ereignisdelegaten in einfachen englische](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Zum definieren ein Ereignisses, verwenden die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample6.cs)]

Da wir brauchen nur auf die Seite Inhalte warnen, wenn der Benutzer geklickt hat die `DoublePrice` Schaltfläche und müssen nicht auf zusätzliche Informationen zu übergeben, können wir den Ereignisdelegaten `EventHandler`, definiert einen Ereignishandler, die akzeptiert, als die zweite der Parameter ein Objekt des Typs `System.EventArgs`. Um das Ereignis in die Masterseite zu erstellen, fügen Sie auf der Masterseite Code-Behind-Klasse die folgende Codezeile hinzu:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample7.cs)]

Der obige Code Fügt ein öffentliches Ereignis auf der Masterseite mit dem Namen `PricesDoubled`. Jetzt müssen wir lösen Sie dieses Ereignis nach dem verdoppelt die Preise aufweisen. Verwenden zum Auslösen ein Ereignisses die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample8.cs)]

Wobei *Absender* und *EventArgs* sind die Werte, die an den Abonnenten-Ereignishandler übergeben werden sollen.

Update der `DoublePrice` `Click` -Ereignishandler durch den folgenden Code:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample9.cs)]

Wie zuvor die `Click` Ereignishandler startet durch Aufrufen der `DoublePricesDataSource` SqlDataSource-Steuerelement `Update` Methode, um die Preise aller Produkte doppelklicken. Im Anschluss an, dass zwei Ergänzungen an den Ereignishandler vorhanden sind. Zunächst wird die `RecentProducts` GridView Daten werden aktualisiert. Diese GridView Masterseite hinzugefügt wurde, in dem vorherigen Lernprogramm und zeigt die fünf zuletzt hinzugefügten Produkte. Wir müssen dieses Raster zu aktualisieren, sodass sie die gerade verdoppelt Preise für diese fünf Produkte angezeigt wird. Nach, dass die `PricesDoubled` Ereignis wird ausgelöst. Ein Verweis auf die master-Seite selbst (`this`) wird an den Ereignishandler gesendet, die Ereignisquelle sowie ein leeres `EventArgs` Objekt gesendet wird, als die Ereignisargumente.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Schritt 4: Behandlung des Ereignisses in der Inhaltsseite

Zu diesem Zeitpunkt die Gestaltungsvorlage löst die `PricesDoubled` Ereignis immer die `DoublePrice` Button-Steuerelement geklickt wird. Allerdings Dies ist nur die Hälfte der Kampf – wir benötigen Sie für die Ereignisbehandlung in den Abonnenten. Dies umfasst zwei Schritte: Erstellen den Ereignishandler und Ereignis Verkabelungscode hinzufügen, sodass der Ereignishandler ausgeführt wird, wenn das Ereignis ausgelöst wird.

Erstellen einen Ereignishandler namens zunächst `Master_PricesDoubled`. Aufgrund wie wir definiert die `PricesDoubled` Ereignis in die Masterseite muss der Ereignishandler zwei Eingabeparameter Typen `Object` und `EventArgs`zugeordnet. Im Aufruf Ereignis-Handler die `ProductsGrid` GridView `DataBind` Methode, um die Daten in den Entwurfsbereich zu binden.


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample10.cs)]

Der Code für den Ereignishandler ist abgeschlossen, aber wir noch auf der Masterseite von Netzwerkdaten `PricesDoubled` Ereignis an diesen Ereignishandler. Der Abonnent verbindet ein Ereignis mit einem Ereignishandler, über die folgende Syntax:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample11.cs)]

*Publisher* ist ein Verweis auf das Objekt, das das Ereignis bietet *EventName*, und *MethodName* ist der Name des ereignishandlers definiert, in dem Abonnenten, die entsprechende Signatur aufweist um die *EventDelegate*. Das heißt, wenn das Ereignis Delegieren ist `EventHandler`, klicken Sie dann *MethodName* muss der Namen einer Methode auf dem Abonnenten, die keinen Wert zurückgibt und nimmt zwei Eingabeparameter Typen `Object` und `EventArgs`, bzw.

Diese Verkabelung der Ereigniscode muss auf dem ersten Besuch der Seite und die nachfolgenden Postbacks ausgeführt werden und sollte auftreten, zu einem Zeitpunkt im Lebenszyklus Seite, die vor, wenn das Ereignis ausgelöst werden kann. Ein guter Zeitpunkt hinzuzufügende Verkabelung der Ereigniscode ist in der Phase PreInit sehr frühen Zeitpunkt im Lebenszyklus Seite angezeigt.

Open `~/Admin/Products.aspx` , und erstellen Sie eine `Page_PreInit` Ereignishandler:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample12.cs)]

Um diesen Verkabelungscode abzuschließen benötigen wir einen programmgesteuerte Verweis auf die Gestaltungsvorlage aus der Seite Inhalt. Wie im vorherigen Lernprogramm erwähnt, gibt es zwei Möglichkeiten:

- Durch die lose typisierten Umwandlung `Page.Master` Eigenschaft in den entsprechenden Masterseite-Typ oder
- Durch Hinzufügen einer `@MasterType` -Direktive in der `.aspx` Seite, und klicken Sie dann mit der stark typisierten `Master` Eigenschaft.

Ermöglicht die Verwendung des zweiten Ansatzes. Fügen Sie die folgenden `@MasterType` -Direktive am Anfang der deklarativen Seitenmarkup:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample13.aspx)]

Fügen Sie den folgenden Ereignis Verkabelungscode in die `Page_PreInit` Ereignishandler:


[!code-csharp[Main](interacting-with-the-content-page-from-the-master-page-cs/samples/sample14.cs)]

Mit diesem Code werden die GridView auf der Inhaltsseite aktualisiert wird immer die `DoublePrice` Schaltfläche geklickt wird.

Abbildung 8 und 9 veranschaulichen dieses Verhalten. Abbildung 8 zeigt die Seite beim ersten Mal besucht hat. Beachten Sie, die Werte in den beiden Preis der `RecentProducts` GridView (in der linken Spalte der Masterseite) und die `ProductsGrid` GridView (auf der Inhaltsseite). Abbildung 9 zeigt die gleiche Seite unmittelbar nach der `DoublePrice` Schaltfläche geklickt wurde. Wie Sie sehen können, werden die neuen Preise sofort in beide GridViews wiedergegeben.


[![Die anfängliche preiswerten](interacting-with-the-content-page-from-the-master-page-cs/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image22.png)

**Abbildung 08**: die anfänglichen preiswerten ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image24.png))


[![Die Just-Doubled Preise werden in der GridViews angezeigt.](interacting-with-the-content-page-from-the-master-page-cs/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-cs/_static/image25.png)

**Abbildung 09**: The Just-Doubled Preise werden in der GridViews angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](interacting-with-the-content-page-from-the-master-page-cs/_static/image27.png))


## <a name="summary"></a>Zusammenfassung

Im Idealfall einer Masterseite und die Inhaltsseiten vollständig voneinander getrennt sind, und Sie erfordern keine Ebene Interaktion. Jedoch, wenn Sie eine Gestaltungsvorlage oder Inhaltsseite, das Daten anzeigt, die über die Gestaltungsvorlage oder Inhaltsseite geändert werden können, müssen Sie möglicherweise die Masterseite der Inhaltsseite (oder umgekehrt a) Warnung haben Wenn Daten geändert werden, damit die Anzeige aktualisiert werden kann. In dem vorherigen Lernprogramm wurde erläutert, wie eine programmgesteuerte Interaktion mit Gestaltungsvorlage Inhaltsseite verfügen; In diesem Lernprogramm erläutert wir, wie eine Masterseite initiieren die Interaktion.

Während Sie den Inhalt oder die Masterseite programmgesteuerte Interaktion zwischen einem Inhalt und die Masterseite stammen kann, hängt verwendete Interaktionsmuster der Ursprung ab. Die Unterschiede werden in den Umstand zurückzuführen, dass eine Inhaltsseite eine einzelne Masterseite enthält, aber einer Masterseite kann viele verschiedene Inhaltsseiten. Anstatt eine direkte Interaktion mit einer Inhaltsseite Gestaltungsvorlage ist ein besserer Ansatz die Masterseite Auslösen eines Ereignisses, um zu signalisieren, dass eine Aktion stattgefunden hat. Diese Inhaltsseiten, dass die Aktion von Bedeutung sind Ereignishandler erstellen kann.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Ereignisse und Delegaten](https://msdn.microsoft.com/en-us/library/17sde2xt.aspx)
- [Übergeben von Informationen zwischen dem Inhalt und Masterseiten](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Arbeiten mit Daten in ASP.NET-Lernprogramme](../../data-access/index.md)

### <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor mehrerer ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com arbeitet mit Microsoft-Web-Technologien seit 1998. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 3.5 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott erreicht werden kann, zur [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Suchi Banerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](interacting-with-the-master-page-from-the-content-page-cs.md)
[Weiter](master-pages-and-asp-net-ajax-cs.md)
