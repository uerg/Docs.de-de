---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Serversteuerelemente | Microsoft Docs
author: microsoft
description: "ASP.NET 2.0 verbessert Serversteuerelemente in vielerlei Hinsicht. In diesem Modul müssen wir einige der Änderungen an der Architektur auf ASP.NET 2.0 und das Visual Studio-200 abdecken..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 09f1a2e4de024e5778e69fdd691d9cb0040459f3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="server-controls"></a>Serversteuerelemente
====================
durch [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 verbessert Serversteuerelemente in vielerlei Hinsicht. In diesem Modul wir behandeln einige der Änderungen an der Architektur auf die Möglichkeit, die ASP.NET 2.0 und Visual Studio 2005 befasst sich mit Serversteuerelemente.


ASP.NET 2.0 verbessert Serversteuerelemente in vielerlei Hinsicht. In diesem Modul wir behandeln einige der Änderungen an der Architektur auf die Möglichkeit, die ASP.NET 2.0 und Visual Studio 2005 befasst sich mit Serversteuerelemente.

## <a name="view-state"></a>Ansichtszustand

Die wichtigste Änderung im Ansichtszustand in ASP.NET 2.0 ist eine erhebliche Verringerung der Größe. Erwägen Sie eine Seite mit nur einem Kalendersteuerelement darauf aus. Hier ist der Ansichtszustand in ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Jetzt sieht der Ansichtszustand für eine identische Seite in ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Dies ist eine bedeutende Änderung, und erwägen, dass Ansichtszustand unverschlüsselt hin und her übergeben werden, diese Änderung kann bieten Entwicklern eine erhebliche Leistungssteigerung. Die Reduzierung der Größe der Ansichtszustand ist aufgrund der Möglichkeit, die wir intern behandelt. Denken Sie daran, dass die Ansicht, dass der Status einer Base64 ist Zeichenfolge codiert. Um die Änderung im Anzeigezustand auf ASP.NET 2.0 besser zu verstehen, lassen Sie uns einen Blick auf die entschlüsselte Werte aus der obigen Beispiele.

So sieht die 1.1 Ansichtszustand decodiert aus:

[!code-css[Main](server-controls/samples/sample3.css)]

Sieht dies möglicherweise etwas wie sinnlosen, aber es ist ein Muster hier. In ASP.NET 1.x, wir einzelne Zeichen verwendet, um Datentypen zu identifizieren und als Trennzeichen für Werte, die mit der &lt; &gt; Zeichen. Im obigen Beispiel Status "t" stellt eine Dreiergruppe dar. Die Dreiergruppe enthält ein Paar von Arraylisten (die "l" steht für ArrayList.) Eine dieser Arraylisten enthält ein Int32-Wert ("i") mit einem Wert von 1 und die andere enthält eine andere Informationen. Die Dreiergruppe enthält ein Paar von Arraylisten usw. an. Wichtig, daran zu denken Sie daran ist, verwenden wir Dreiergruppen, die Paare enthalten, wir die Datentypen über ein Buchstabe ermitteln und wir verwenden die &lt; und &gt; Zeichen als Trennzeichen.

In ASP.NET 2.0 sieht die decodierte Ansichtszustand etwas anders.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Sie sollten eine enorme Änderung an der Darstellung des Ansichtszustands decodierte feststellen. Diese Änderung hat mehrere Architekturprinzipien. Den Ansichtszustand in ASP.NET 1.x der LosFormatter zum Serialisieren von Daten verwendet. In 2.0 verwenden wir die neue ObjectStateFormatter-Klasse. Diese Klasse wurde speziell für die Serialisierung und Deserialisierung der Ansichtszustand und Steuerelementzustand verwendet. (Steuerelementzustand wird im nächsten Abschnitt erläutert.) Es gibt viele Vorteile durch Ändern der Methode, die durch die Serialisierung und Deserialisierung stattfinden. Keines der deutlichste handelt es sich um die Tatsache, dass im Gegensatz zu den LosFormatter der TextWriter verwendet, die ObjectStateFormatter eine BinaryWriter verwendet. Dadurch können ASP.NET 2.0 zum Anzeigen des Status einer Reihe von Bytes anstatt Zeichenfolgen zu speichern. Nehmen Sie z. B. eine ganze Zahl. In ASP.NET 1.1 erforderlich, eine ganze Zahl 4 Bytes der Ansichtszustand. In ASP.NET 2.0 ist die gleiche Ganzzahl nur 1 Byte erforderlich. Weitere Verbesserungen wurden vorgenommen, um den Ansichtszustand zu verringern, der gespeichert werden. DateTime-Werte werden nun z. B. gespeichert mithilfe einer TickCount anstelle einer Zeichenfolge.

Wie bei allen, die nicht genug wäre, wurde besondere Aufmerksamkeit auf die Tatsache bezahlt, dass eine der größten Consumer des Anzeigestatus in 1.x das DataGrid und die Steuerelemente ähnlichen wurde. Ein Haupt-Nachteil der Steuerelemente, z. B. das DataGrid-Steuerelement, in dem Ansichtszustand geht, ist, dass sie häufig große Mengen von wiederholten Informationen enthält. In ASP.NET 1.x, die wiederholt Informationen wurde gegenüber und erneut in einem sehr großer Ansichtszustand resultierende einfach gespeichert. In ASP.NET 2.0 verwenden wir neue IndexedString-Klasse, um solche Daten speichern. Tritt wiederholt auf eine Zeichenfolge, speichern wir einfach das Token für die IndexedString und der Index in eine Tabelle der ausgeführten Objekte IndexedString an.

## <a name="control-state"></a>Steuerelementzustand

Eines der wichtigsten Gripes, die Entwickler mit Ansichtszustand mussten wurde die Größe, die sie die HTTP-Nutzlast hinzugefügt. Wie bereits erwähnt, eine der größten Consumer des Ansichtszustands des DataGrid-Steuerelements ist. Um die großen Datenmengen Ansichtszustand generiert, die für ein DataGrid-Steuerelement zu vermeiden, deaktiviert viele Entwickler einfach Ansichtszustand für das Steuerelement. Leider war die Projektmappe immer eine gute nicht. Den Ansichtszustand in ASP.NET 1.x enthält nicht nur Daten für die richtige Funktionalität des Steuerelements erforderlich sind. Es enthält auch Informationen über den Zustand der Benutzeroberfläche des Steuerelements. Dies bedeutet, dass wenn für die Paginierung auf ein DataGrid-Steuerelement zu ermöglichen, müssen Sie Anzeigezustand aktivieren, auch wenn Sie nicht alle UI-Informationen, die angezeigt werden sollen Zustand enthält. Es ist ein Szenario nichts.

In ASP.NET 2.0 löst Steuerelementzustand dieses Problem gleichmäßig über die Einführung der Steuerelementzustand. Steuerelementzustand enthält die Daten, die für die ordnungsgemäße Funktionalität eines Steuerelements unbedingt notwendig ist. Im Gegensatz zu den Ansichtszustand kann der Steuerelementzustand deaktiviert werden. Aus diesem Grund ist es wichtig, dass die Daten gespeichert werden, in der Steuerelementzustand sorgfältig kontrolliert werden.

> [!NOTE]
> Steuerelementzustand wird beibehalten, zusammen mit den Ansichtszustand in der \_ \_ausgeblendetes Formularfeld "ViewState" speichern.


In diesem Video wird eine exemplarische Vorgehensweise Ansichtszustand und Steuerelementzustand.


![](server-controls/_static/image1.png)


[Open Vollbild-Video](server-controls/_static/state1.wmv)


In der Reihenfolge nach einem Serversteuerelement, Lese- und Schreibberechtigungen für den Status zu steuern müssen Sie drei Schritte ausführen.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Schritt 1: Rufen Sie die RegisterRequiresControlState-Methode

Die Methode RegisterRequiresControlState informiert ASP.NET, dass ein Steuerelement Steuerelementzustand beibehalten werden muss. Es akzeptiert ein Argument des Typs steuern, welche dem Steuerelement befindet, das registriert wird.

Es ist wichtig zu beachten, dass die Registrierung nicht aus einer Anforderung auf Anforderung beibehalten wird. Diese Methode muss daher bei jeder Anforderung aufgerufen werden, ist ein Steuerelement Steuerelementzustand beibehalten werden. Es wird empfohlen, dass die Methode in OnInit aufgerufen werden.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Schritt 2: Außerkraftsetzung SaveControlState

Die Methode SaveControlState speichert Steuerelement statusänderungen für ein Steuerelement seit der letzten Postback. Es gibt ein Objekt, das den Zustand des Steuerelements darstellt.

## <a name="step-3-override-loadcontrolstate"></a>Schritt 3: Außerkraftsetzung LoadControlState

Die Methode LoadControlState lädt gespeicherten Zustand in ein Steuerelement. Die Methode akzeptiert ein Argument vom Typ Objekt, das den gespeicherten Zustand für das Steuerelement enthält.

## <a name="full-xhtml-compliance"></a>Vollständige XHTML-Kompatibilität

Jeder Webentwickler kennt die Wichtigkeit von Standards in Webanwendungen. Um eine Standards basierende Entwicklungsumgebung zu wahren, ist ASP.NET 2.0 vollständig XHTML-kompatibel. Daher sind alle Tags gerenderten entsprechend XHTML-Standards in Browsern, die mit Unterstützung für HTML 4.0 oder höher.

Die DOCTYPE-Definition in ASP.NET, Version 1.1, lautet wie folgt:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2.0 weist die DOCTYPE-Standarddefinition Folgendes:

[!code-html[Main](server-controls/samples/sample7.html)]

Falls gewünscht, können Sie die standardmäßige XHML Kompatibilität über den XhtmlConformance Knoten in der Konfigurationsdatei ändern. Beispielsweise werden die folgenden Knoten in der Datei "Web.config" XHTML-Kompatibilität in XHTML 1.0 strikte ändern:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Falls gewünscht, können Sie auch konfigurieren, auf ASP.NET verwenden, die ältere Konfiguration in ASP.NET verwendeten 1.x wie folgt:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptive mithilfe von Adaptern Rendern

In ASP.NET 1.x, die Konfigurationsdatei enthaltenen eine &lt;BrowserCaps&gt; Abschnitt, der ein Objekt HttpBrowserCapabilities aufgefüllt. Dieses Objekt zulässig, ein Entwickler zu bestimmen, welches Gerät eine bestimmte Anforderung stammt und Code entsprechend zu rendern. In ASP.NET 2.0 wird das Modell wurde verbessert und verwendet jetzt die neue ControlAdapter-Klasse. Die ControlAdapter-Klasse überschreibt Ereignisse im Lebenszyklus des Steuerelements und steuert das Rendern von Steuerelementen, die auf Grundlage der Benutzer-Agent-Funktionen. Die Funktionen eines bestimmten Benutzer-Agents werden durch eine Browserdefinitionsdatei (eine Datei mit der Erweiterung Browser) in die c:\windows\microsoft.net\framework\v2.0 gespeicherten definiert. \* \* \* \*\CONFIG\Browsers-Ordner.

> [!NOTE]
> Die ControlAdapter-Klasse ist eine abstrakte Klasse.


Ähnlich wie die &lt;BrowserCaps&gt; Abschnitt in der Browserdefinitionsdatei 1.x verwendet einen regulären Ausdruck, um die Benutzer-Agent-Zeichenfolge zu analysieren, um den Browser zu identifizieren. Er bestimmte Funktionen für diesen Benutzeragent definiert. Die ControlAdapter rendert das Steuerelement über die Render-Methode. Daher, wenn Sie die Render-Methode überschreiben, sollten Sie nicht Rendern in der Basisklasse aufrufen. Auf diese Weise kann dazu führen, dass Rendern einmal für den Adapter und einmal für das Steuerelement selbst zweimal ausgeführt.

## <a name="developing-a-custom-adapter"></a>Entwickeln eines benutzerdefinierten Adapters

Sie können einen eigenen benutzerdefinierten Adapter entwickeln, durch Vererben von ControlAdapter. Darüber hinaus können Sie von der abstrakten Klasse PageAdapter in Fällen erben, in dem ein Adapter für eine Seite erforderlich ist. Zuordnung von Steuerelementen zu Ihren benutzerdefinierten Adapter erfolgt über die &lt;ControlAdapters&gt; Element in der Browserdefinitionsdatei ab. Die folgenden XML-Code aus einer Browserdefinitionsdatei ordnet z. B. das Menüsteuerelement MenuAdapter-Klasse:

[!code-html[Main](server-controls/samples/sample10.html)]

Dieses Modell verwenden, wird er lässt sich ganz einfach für einen Entwickler von Steuerelementen an ein bestimmtes Gerät oder einen Browser. Es ist auch ganz einfach für Entwickler haben vollständige Kontrolle über die Seiten auf jedem Gerät wie gerendert.

## <a name="per-device-rendering"></a>Pro Gerät-Rendering

Server-Steuerelementeigenschaften in ASP.NET 2.0 können angegebenen pro Gerät mit einem Browser-spezifische Präfix sein. Beispielsweise ändert der folgende Code den Text einer Bezeichnung, je nachdem welches Gerät verwendet wird, um die Seite zu navigieren.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Wenn die Seite mit dieser Bezeichnung von Internet Explorer angezeigt wird, wird die Bezeichnung Anzeigetext besagt "Von InternetExplorer durchsuchen." Wenn die Seite von Firefox durchsucht wird, die Bezeichnung der Text wird angezeigt "Sie sind von Firefox durchsuchen." Wenn die Seite von einem beliebigen anderen Gerät durchsucht wird, werden angezeigt "Sie sind von einem unbekannten Gerät durchsuchen." Eine Eigenschaft kann mithilfe dieser speziellen Syntax angegeben werden.

## <a name="setting-focus"></a>Festlegen des Fokus

Häufig gestellte 1.x ASP.NET-Entwickler über wie Anfangsfokus für ein bestimmtes Steuerelement festgelegt. Auf einer Anmeldeseite ist es beispielsweise sinnvoll, die Benutzer-ID-Textfeld den Fokus erhalten beim ersten die Seite laden zu verwenden. In ASP.NET erforderlich 1.x, die auf diese Weise einige clientseitigem Skript schreiben. Obwohl solche ein Skript eine einfache Aufgabe ist, ist es nicht mehr erforderlich in ASP.NET 2.0 Dank der SetFocus-Methode. Die SetFocus-Methode akzeptiert ein Argument, der angibt, des Steuerelements, das Fokus erhalten soll. Dieses Argument kann es sich entweder um die Client-ID des Steuerelements als Zeichenfolge oder den Namen des Serversteuerelements als ein Control-Objekt sein. Fügen Sie z. B. zum Festlegen der Anfangsfokus auf ein TextBox-Steuerelement namens TxtUserID beim ersten Laden der Seite "den folgenden Code zur Seite\_laden:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-- oder

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 verwendet "WebResource.axd" Handler (zuvor erläutert), um eine Client-Side-Funktion zu rendern, die den Fokus festlegt. Der Name der clientseitigen-Funktion ist WebForm\_AutoFocus wie hier gezeigt:

[!code-html[Main](server-controls/samples/sample14.html)]

Der Fokus-Methode für ein Steuerelement können Sie alternativ auf das entsprechende Steuerelement Anfangsfokus. Die Methode den Fokus leitet sich von der Control-Klasse und für alle Steuerelemente von ASP.NET 2.0 verfügbar ist. Es ist auch möglich, den Fokus zu einem bestimmten Steuerelement festgelegt, wenn ein Validierungsfehler auftritt. Die in einem späteren Modul abgedeckt werden.

## <a name="new-server-controls-in-aspnet-20"></a>Neue Serversteuerelemente in ASP.NET 2.0

Im folgenden werden neue Serversteuerelemente in ASP.NET 2.0. Es wird auf einigen von ihnen in späteren Modulen befassen.

## <a name="imagemap-control"></a>ImageMap-Steuerelement

Die ImageMap-Steuerelement können Sie Hotspots zu einem Bild hinzufügen, die ein Postback initiieren oder zu einer URL navigieren können. Es gibt drei Arten von Hotspots verfügbar; CircleHotSpot RectangleHotSpot und polygonförmigen. Hotspots werden über eine auflistungs-Editor in Visual Studio oder programmgesteuert in Code hinzugefügt. Es gibt keine Benutzeroberfläche für das Zeichnen von Hotspots auf ein Bild zur Verfügung. Die Koordinaten und die Größe oder den Radius der Hotspot muss deklarativ angegeben werden. Es ist auch keine visuelle Darstellung der ein Hotspot im Designer. Wenn ein Hotspot für die Navigation zu einer URL konfiguriert ist, wird die URL über die NavigateUrl-Eigenschaft des Hotspots angegeben. Sichern Sie bei einer POST-Anforderung Hotspot, die PostBackValue-Eigenschaft können Sie serverseitigen Code übergeben Sie eine Zeichenfolge im Beitrag zurück, die abgerufen werden kann.


![HotSpot-Auflistungs-Editor in Visual Studio](server-controls/_static/image1.jpg)

**Abbildung 1**: HotSpot-Auflistungs-Editor in Visual Studio


## <a name="bulletedlist-control"></a>BulletedList-Steuerelement

Der BulletedList-Steuerelement ist eine Aufzählung, die einfach Daten gebunden werden können. Die Liste (nummerierte) sortiert werden kann oder nicht über die BulletStyle-Eigenschaft sortiert. Jedes Element in der Liste wird von einem "ListItem"-Objekt dargestellt.


![BulletedList-Steuerelement in Visual Studio](server-controls/_static/image1.gif)

**Abbildung 2**: BulletedList-Steuerelement in Visual Studio


## <a name="hiddenfield-control"></a>HiddenField-Steuerelement

HiddenField-Steuerelement wird ein ausgeblendetes Formularfeld hinzugefügt, auf die Seite, deren, die Wert in serverseitigen Code verfügbar ist. Der Wert, der ein ausgeblendetes Formularfeld muss in der Regel zwischen Postbacks unverändert bleiben. Es ist jedoch möglich, dass ein böswilliger Benutzer so ändern Sie den Wert vor, um zurückgesendet wird. In diesem Fall wird das HiddenField-Steuerelement das ValueChanged-Ereignis ausgelöst. Wenn Sie vertraulichen Informationen in das HiddenField-Steuerelement und sicherzustellen, dass es unverändert bleiben sollen, sollten Sie das ValueChanged-Ereignis im Code behandeln.

## <a name="fileupload-control"></a>FileUpload-Steuerelement

Das FileUpload-Steuerelement in ASP.NET 2.0 ermöglicht das Hochladen von Dateien auf einem Webserver über eine ASP.NET-Seite. Dieses Steuerelement ist sehr ähnlich, die ASP.NET 1.x HtmlInputFile-Klasse, mit wenigen Ausnahmen. In ASP.NET 1.x wurde empfohlen, dass die PostedFile-Eigenschaft auf Null überprüft werden, um festzustellen, ob Sie eine gute Datei hatten. Das FileUpload-Steuerelement in ASP.NET 2.0 fügt eine neue HasFile-Eigenschaft, Sie für denselben Zweck können und es etwas effizienter ist.

Die PostedFile-Eigenschaft ist für den Zugriff auf ein Objekt HttpPostedFile weiterhin verfügbar, aber einige der Funktionen von der HttpPostedFile steht jetzt systemintern mit dem FileUpload-Steuerelement. Z. B. speichern eine hochgeladene Datei in ASP.NET 1.x, rufen Sie die SaveAs-Methode für das Objekt HttpPostedFile. Verwenden das FileUpload-Steuerelement in ASP.NET 2.0, würden Sie SaveAs-Methode auf dem FileUpload-Steuerelement selbst aufrufen.

Weitere signifikante Änderung in der 2.0-Verhalten (und wahrscheinlich die wichtigste Änderung) ist, dass es nicht mehr notwendig, eine gesamte hochgeladene Datei in den Arbeitsspeicher geladen, bevor Sie Sie speichern. In 1.x, jede Datei, die hochgeladen wurde vollständig in den Arbeitsspeicher vor geschriebenen gespeichert wird auf den Datenträger. Diese Architektur wird verhindert, dass das Hochladen großer Dateien.

In ASP.NET 2.0 das RequestLengthDiskThreshold-Attribut des HttpRuntime-Elements können Sie konfigurieren, wie viele Kilobyte werden in einem Puffer im Arbeitsspeicher vor geschriebenen aufrechterhalten auf den Datenträger.

**WICHTIGE**: MSDN-Dokumentation (und Dokumentation an anderer Stelle) gibt an, dass dieser Wert in Bytes (nicht in Kilobyte) und der Standardwert 256 ist. Der Wert tatsächlich in Kilobyte angegeben wird, und der Standardwert ist 80. Da einen Standardwert von 80 KB, stellen wir sicher, dass der Puffer nicht auf einen großen Objektheap einrichten endet.

## <a name="wizard-control"></a>Assistenten-Steuerelement

Es ist häufig mit der ASP.NET-Entwickler Versuch zum Sammeln von Informationen in einer Reihe von "Seiten" Verwenden von Bereichen oder durch die Übertragung von einer Seite zu kämpfen auftreten. Mehr häufig als nicht der Vorgang ist eine frustrierend und zeitaufwändig. Das neue Assistenten Steuerelement löst die Probleme bei linear und nichtlineare Schritte in einem Assistentenschnittstelle, der Benutzer mit vertraut sind. Das Assistenten-Steuerelement zeigt als Eingabeformulare in eine Reihe von Schritten an Jeder Schritt ist eines bestimmten Typs von der StepType-Eigenschaft des Steuerelements angegeben. Die verfügbaren Schritttypen lauten folgendermaßen:

| **Schritttyp** | **Erläuterung** |
| --- | --- |
| Auto | Der Assistent bestimmt automatisch den Typ des Schritts basierend auf seiner Position innerhalb der Hierarchie Schritt. |
| Starten | Der erste Schritt, häufig verwendet, um eine einführende Anweisung darzustellen. |
| Schritt | Ein normaler Schritt. |
| Fertig stellen | Der letzte Schritt in der Regel verwendet, um eine Schaltfläche zum Beenden des Assistenten zu präsentieren. |
| Abgeschlossen | Stellt eine Nachricht, die Kommunikation erfolgreich oder fehlerhaft war. |

> [!NOTE]
> Das Assistenten-Steuerelement der nachverfolgt Datenbankzustands Steuerelementzustand ASP.NET verwenden. Aus diesem Grund kann die EnableViewState-Eigenschaft auf "false", ohne alle Nachteil festgelegt werden.


In diesem Video wird eine exemplarische Vorgehensweise des Assistenten-Steuerelements.


![](server-controls/_static/image2.png)


[Open Vollbild-Video](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Lokalisieren von Steuerelement

Localize-Steuerelement ist ein Literal Steuerelement ähnlich. Allerdings Localize-Steuerelement verfügt über eine **Modus** , steuert das Rendern Markup, das dem Plan hinzugefügt wird. Die Mode-Eigenschaft unterstützt die folgenden Werte:

| **Modus** | **Erläuterung** |
| --- | --- |
| Transformation | Markup wird nach dem Protokoll des Browsers die Anforderung transformiert. |
| Pass-Through- | Als Markup gerendert-ist. |
| Codieren | Markup, das dem Steuerelement hinzugefügt wird mit HtmlEncode codiert. |

## <a name="multiview-and-view-controls"></a>MultiView und -Steuerelemente

Das MultiView Steuerelement fungiert als Container für Steuerelemente, und das Steuerelement fungiert als Container für andere Steuerelemente (ähnlich wie ein Panel-Steuerelement). Jede Ansicht in einem MultiView-Steuerelement wird von einem einzelnen Steuerelement dargestellt. Beim erste Steuerelement Ansicht der MultiView ist 0, die zweite Ansicht 1, usw. Sie können durch Angabe des Steuerelements MultiView ActiveViewIndex wechseln.

## <a name="substitution-control"></a>Substitution-Steuerelement

Das Steuerelement für die Ersetzung wird in Verbindung mit der Zwischenspeicherung in ASP.NET verwendet. In Fällen, in denen Sie caching nutzen möchten, aber die Teile einer Seite, die für jede Anforderung (das heißt, Teile einer Seite, die von der Zwischenspeicherung ausgenommen sind) müssen aktualisiert werden, stehen Ihnen, bietet die Ersetzung-Komponente eine großartige Lösung. Keine Ausgabe selbst rendern nicht tatsächlich des Steuerelements. Stattdessen wird sie an eine Methode in serverseitigen Code gebunden. Wenn die Seite angefordert wird, die Methode wird aufgerufen, und das zurückgegebene Markup wird anstelle der Ersetzungssteuerelement gerendert.

Die Methode, die an die das Ersatz-Steuerelement gebunden ist wird angegeben, über die **MethodName** Eigenschaft. Diese Methode muss die folgenden Kriterien erfüllen:

- Es muss eine statische (freigegebene in VB) Methode.
- Er akzeptiert einen Parameter des Typs HttpContext.
- Es gibt eine Zeichenfolge, die das Markup, das das Steuerelement auf der Seite "ersetzt werden soll.

Das Ersatz-Steuerelement besitzt nicht die Berechtigung zum Ändern von jedem anderen Steuerelement auf der Seite, aber sie verfügt über Zugriff auf die aktuelle HttpContext über seine Parameter.

## <a name="gridview-control"></a>GridView-Steuerelements

GridView-Steuerelements ist der Ersatz für das DataGrid-Steuerelement. Dieses Steuerelement wird in einem Modul später ausführlicher erläutert.

## <a name="detailsview-control"></a>DetailsView-Steuerelement

DetailsView-Steuerelement können Sie auf die Anzeige eines einzelnen Datensatzes aus einer Datenquelle und zu bearbeiten oder zu löschen. Es wird in einem Modul später ausführlicher behandelt.

## <a name="formview-control"></a>FormView-Steuerelement

Die FormView-Steuerelement wird verwendet, um einen einzelnen Datensatz aus einer Datenquelle in einer konfigurierbaren Oberfläche anzuzeigen. Es wird in einem Modul später ausführlicher behandelt.

## <a name="accessdatasource-control"></a>AccessDataSource-Steuerelement

AccessDataSource-Steuerelement wird verwendet, um Daten zu binden eine Access-Datenbank. Es wird in einem Modul später ausführlicher behandelt.

## <a name="objectdatasource-control"></a>ObjectDataSource-Steuerelement

Das ObjectDataSource-Steuerelement wird verwendet, eine drei-Ebenen-Architektur unterstützt, sodass Steuerelemente Daten-gebunden werden können, um ein Geschäftsobjekt der mittleren Ebene im Gegensatz zu einem Modell mit zwei Ebenen, in denen Steuerelemente direkt an die Datenquelle gebunden. Sie wird in einem Modul später ausführlicher besprochen.

## <a name="xmldatasource-control"></a>XmlDataSource-Steuerelement

XmlDataSource-Steuerelement wird zur Datenbindung an eine XML-Datenquelle verwendet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource-Steuerelement

Die Datenbindung für die Website Navigationssteuerelemente basierend auf einer Siteübersicht des SiteMapDataSource gesteuert. Sie wird in einem Modul später ausführlicher besprochen.

## <a name="sitemappath-control"></a>SiteMapPath-Steuerelement

Das SiteMapPath-Steuerelement zeigt eine Reihe von Navigationslinks gemeinhin als Breadcrumbs bezeichnet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="menu-control"></a>Menu-Steuerelement

Das Menüsteuerelement Zeigt dynamische Menüs mit DHTML-Codes angezeigt. Es wird in einem Modul später ausführlicher behandelt.

## <a name="treeview-control"></a>TreeView-Steuerelement

TreeView-Steuerelement wird verwendet, um eine hierarchische Ansicht der Daten anzuzeigen. Es wird in einem Modul später ausführlicher behandelt.

## <a name="login-control"></a>Steuerelement für die Anmeldung

Das Steuerelement für die Anmeldung stellt für einen Mechanismus zum Anmelden an einer Websites bereit. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginview-control"></a>LoginView-Steuerelement

LoginView-Steuerelement ermöglicht die Anzeige von Vorlagen basierend auf den Anmeldestatus eines Benutzers. Es wird in einem Modul später ausführlicher behandelt.

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das PasswordRecovery-Steuerelement ist vergessene Kennwörter abrufen von Benutzern einer ASP.NET-Anwendung verwendet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginstatus"></a>loginStatus

Das LoginStatus-Steuerelement zeigt Anmeldestatus eines Benutzers. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginname"></a>LoginName

Die LoginName-Steuerelement zeigt den Benutzernamen von Benutzern nach einer ASP.NET-Anwendung angemeldet wird. Es wird in einem Modul später ausführlicher behandelt.

## <a name="createuserwizard"></a>CreateUserWizard

Die CreateUserWizard ist eine konfigurierbare-Assistent die Benutzer die Berechtigung zum Erstellen einer ASP.NET Mitgliedskonto für die Verwendung in einer ASP.NET-Anwendung gewährt. Es wird in einem Modul später ausführlicher behandelt.

## <a name="changepassword"></a>ChangePassword

Das Steuerelement ChangePassword ermöglicht Benutzern das Ändern seines Kennworts für eine ASP.NET-Anwendung. Es wird in einem Modul später ausführlicher behandelt.

## <a name="various-webparts"></a>Verschiedene WebParts

Im Lieferumfang von ASP.NET 2.0 sind verschiedene Webparts. Diese werden in einer späteren Modul ausführlich beschrieben.
