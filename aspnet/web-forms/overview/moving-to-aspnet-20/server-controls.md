---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: -Steuerelemente | Microsoft-Dokumentation
author: microsoft
description: ASP.NET 2.0 wird die Serversteuerelemente in vielerlei Hinsicht verbessert. In diesem Modul wird über die architektonischen Änderungen auf die Möglichkeit, ASP.NET 2.0 und Visual Studio-200 behandelt...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: da06429f3949a47a02fccef45666d1220781e473
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837068"
---
<a name="server-controls"></a>Serversteuerelemente
====================
durch [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 wird die Serversteuerelemente in vielerlei Hinsicht verbessert. Klicken Sie in diesem Modul erläutert einige der architektonischen Änderungen wie ASP.NET 2.0, und Visual Studio 2005-Steuerelemente behandelt.


ASP.NET 2.0 wird die Serversteuerelemente in vielerlei Hinsicht verbessert. Klicken Sie in diesem Modul erläutert einige der architektonischen Änderungen wie ASP.NET 2.0, und Visual Studio 2005-Steuerelemente behandelt.

## <a name="view-state"></a>Ansichtszustand

Die wichtigste Änderung im Ansichtszustand in ASP.NET 2.0 ist eine erhebliche Verringerung der Größe. Erwägen Sie eine Seite mit nur einem Kalendersteuerelement auf. Hier ist der Ansichtszustand in ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Jetzt ist hier der Ansichtszustand für eine identische Seite in ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Dies ist eine bedeutende Änderung, und in Betracht ziehen, dass der Ansichtszustand hin-und über das Netzwerk übertragen wird, diese Änderung erhalten Entwickler eine erhebliche Leistungssteigerung. Die Verringerung der Größe des Ansichtstatus ist größtenteils aufgrund der Art, die wir es intern verarbeiten. Denken Sie daran, dass die Ansicht, dass sich befindet ein Base64-codierten Zeichenfolge. Um die Änderung im Ansichtszustand in ASP.NET 2.0 besser zu verstehen, werfen wir einen Blick auf die entschlüsselte Werte aus den obigen Beispielen.

Hier ist der 1.1 Ansichtszustand decodiert:

[!code-css[Main](server-controls/samples/sample3.css)]

Sieht dies möglicherweise ein wenig wie Schritte unverständlich sind, aber es gibt hier ein Muster. In ASP.NET 1.x, wir mit dem einzigen Zeichen-Datentypen und getrennte Werte, die mit der &lt; &gt; Zeichen. Das "t" im obigen Ansicht Zustand Beispiel stellt eine Dreiergruppe dar. Die Dreiergruppe enthält ein Paar von Arraylisten (die "l" steht eine ArrayList.) Eine dieser Arraylisten enthält ein Int32-Wert ("i") mit einem Wert von 1 und die andere enthält eine andere Dreiergruppe. Die Dreiergruppe enthält ein Paar von Arraylisten usw. an. Wichtig zu beachten ist, dass wir Dreiergruppen, die Paare enthalten, wir die Datentypen über ein Buchstabe identifizieren, und wir verwenden die &lt; und &gt; Zeichen als Trennzeichen.

In ASP.NET 2.0 aussieht ein wenig anders der decodierte Ansichtszustand.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Sie sollten eine enorme Änderung an der Darstellung des decodierten Ansichtszustands feststellen. Diese Änderung hat mehrere Architekturprinzipien. Den Ansichtszustand in ASP.NET 1.x die LosFormatter zum Serialisieren von Daten verwendet. In 2.0 verwenden wir die neue ObjectStateFormatter-Klasse. Diese Klasse wurde speziell entwickelt, um die Serialisierung und Deserialisierung von Ansichts- und Steuerelementzustand zu erleichtern. (Steuerelementzustand wird im nächsten Abschnitt behandelt werden.) Es gibt viele Vorteile durch Ändern der Methode, die durch die Serialisierung und Deserialisierung durchgeführt werden. Eines der wichtigste ist die Tatsache, dass im Gegensatz zu den LosFormatter der TextWriter verwendet, die ObjectStateFormatter ein BinaryWriter verwendet. Dadurch wird ein ASP.NET 2.0 zum Anzeigen des Status einer Reihe von Bytes anstelle von Zeichenfolgen zu speichern. Nehmen Sie z. B. eine ganze Zahl. In ASP.NET 1.1 erforderlich, eine ganze Zahl 4 Bytes der Ansichtszustand. In ASP.NET 2.0 erfordert, dieselbe ganze Zahl nur 1 Byte. Weitere Verbesserungen wurden vorgenommen, um die Menge des Ansichtszustands zu verringern, die gespeichert werden. Mithilfe einer TickCount anstelle einer Zeichenfolge werden DateTime-Werte, z. B. jetzt gespeichert.

Wie bei allen, die nicht genug wäre, besondere Aufmerksamkeit auf die Tatsache bezahlt, dass eine der größten Consumer des Ansichtszustands im 1.x das Datenraster und die Steuerelemente mit ähnlichen wurde. Steuerelemente wie DataGrid, in dem Ansichtszustand betrifft, ein großen Nachteil ist, dass sie häufig große Mengen von wiederholten Informationen enthält. In ASP.NET 1.x, die wiederholt Informationen einfach über und was in einem sehr groß Ansichtszustand gespeichert wurde. In ASP.NET 2.0 verwenden wir die neue IndexedString-Klasse, um solche Daten zu speichern. Tritt wiederholt auf eine Zeichenfolge, speichern wir nur das Token für die IndexedString und den Index in eine Tabelle der ausgeführten IndexedString Objekte an.

## <a name="control-state"></a>Steuerelementzustand

Eines der wichtigsten nörgeleien zu, denen Entwickler mit Ansichtszustand hatten wurde die Größe, die sie an die HTTP-Nutzlast hinzugefügt. Wie bereits erwähnt, eine der größten Consumer des Ansichtszustands ist dem DataGrid-Steuerelement. Um die großen Mengen des Ansichtszustands durch ein DataGrid-Steuerelement generiert zu vermeiden, deaktiviert viele Entwickler einfach Ansichtszustand für dieses Steuerelement. Leider nicht die Lösung immer eine gute raus. Den Ansichtszustand in ASP.NET 1.x nicht nur Daten für die korrekte Funktion des Steuerelements enthält. Sie enthält auch Informationen über den Zustand der Benutzeroberfläche des Steuerelements. Dies bedeutet, dass wenn Sie zulassen möchten, für die Paginierung auf ein DataGrid, müssen Sie Ansichtszustand aktivieren, auch wenn Sie nicht benötigen alle UI-Informationen, die anzeigen Zustand enthält. Es wird nichts.

In ASP.NET 2.0 Problem problemlos über die Einführung des Steuerelementzustands ist Steuerelementzustand gelöst. Steuerelementzustand enthält die Daten, die für die ordnungsgemäße Funktionalität eines Steuerelements absolut notwendig ist. Steuerelementzustand kann nicht deaktiviert werden, im Gegensatz zu den Ansichtszustand. Aus diesem Grund ist es wichtig, dass die Daten im Steuerelementzustand dennoch sorgfältig kontrolliert werden.

> [!NOTE]
> Steuerelementzustand wird beibehalten, zusammen mit den Ansichtszustand in das \_ \_VIEWSTATE ausgeblendeten Formularfelds.


Dieses Video ist eine exemplarische Vorgehensweise für die Ansichts- und Steuerelementzustand.


![](server-controls/_static/image1.png)


[Open Vollbild-Video](server-controls/_static/state1.wmv)


In der Reihenfolge nach einem Serversteuerelement zu lesen und Schreiben in den Status zu steuern müssen Sie drei Schritte ausführen.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Schritt 1: Rufen Sie die RegisterRequiresControlState-Methode

Die RegisterRequiresControlState-Methode informiert ASP.NET ein Steuerelement muss Steuerelementzustand beibehalten werden. Es dauert ein Argument des Typs steuern, welche das Steuerelement ist, das registriert wird.

Es ist wichtig zu beachten, dass die Registrierung von Anforderung zu Anforderung nicht beibehalten wird. Diese Methode muss daher bei jeder Anforderung aufgerufen werden, wenn ein Steuerelement ist Steuerelementzustand beibehalten werden. Es wird empfohlen, dass die Methode OnInit aufgerufen werden.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Schritt 2: Überschreiben Sie SaveControlState

Die Methode SaveControlState speichert Steuerelement Änderungen am Ansichtszustand für ein Steuerelement seit dem letzten Postback-Ereignis. Es gibt ein Objekt, das den Zustand des Steuerelements darstellt.

## <a name="step-3-override-loadcontrolstate"></a>Schritt 3: Außer Kraft setzen Sie LoadControlState

Die Methode LoadControlState lädt den gespeicherten Zustand in ein Steuerelement. Die Methode akzeptiert ein Argument vom Typ Objekt, das den gespeicherten Zustand für das Steuerelement enthält.

## <a name="full-xhtml-compliance"></a>Vollständige XHTML-Kompatibilität

Jeder Webentwickler weiß die Wichtigkeit der Standards in Webanwendungen. Um eine auf Standards basierende Entwicklungsumgebung zu gewährleisten, ist die ASP.NET 2.0 vollständig XHTML-kompatibles. Daher sind alle Tags gerenderten gemäß den Standards XHTML in Browsern, die mit Unterstützung für HTML 4.0 oder höher.

Die DOCTYPE-Definition in ASP.NET 1.1 war wie folgt aus:

[!code-html[Main](server-controls/samples/sample6.html)]

In ASP.NET 2.0 lautet die DOCTYPE-Default-Definition:

[!code-html[Main](server-controls/samples/sample7.html)]

Wenn Sie auswählen, können Sie die standardmäßige XHML Kompatibilität über den XhtmlConformance-Knoten in der Konfigurationsdatei ändern. Beispielsweise ändert der folgende Knoten in der Datei "Web.config" XHTML-Kompatibilität in XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Falls gewünscht, können Sie auch konfigurieren, ASP.NET, um verwenden Sie die ältere Konfiguration in ASP.NET verwendet 1.x wie folgt:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptive Rendern mithilfe von Adaptern

In ASP.NET 1.x, die Konfigurationsdatei enthalten eine &lt;BrowserCaps&gt; -Abschnitt, der einem HttpBrowserCapabilities-Objekt aufgefüllt. Dieses Objekt zulässig, ein Entwickler zu bestimmen, welche Geräte eine bestimmte Anforderung stammt und Code entsprechend rendern. In ASP.NET 2.0 wird das Modell wurde verbessert und verwendet jetzt die neue ControlAdapter-Klasse. Die ControlAdapter-Klasse überschreibt die Ereignisse im Lebenszyklus von des Steuerelements und steuert das Rendering von Steuerelementen auf Grundlage der Benutzer-Agent-Funktionen. Die Funktionen eines bestimmten Benutzer-Agents werden durch eine Browser-Definitionsdatei (eine Datei mit einer .browser-Dateierweiterung) gespeichert, in der c:\windows\microsoft.net\framework\v2.0 definiert. \* \* \* \*\CONFIG\Browsers-Ordner.

> [!NOTE]
> Die ControlAdapter-Klasse ist eine abstrakte Klasse.


Ähnlich wie die &lt;BrowserCaps&gt; Abschnitt in der Browserdefinitionsdatei 1.x verwendet einen regulären Ausdruck, um die Benutzeragentzeichenfolge zu analysieren, um den anfordernden Browser zu identifizieren. Es werden bestimmte Funktionen für diesen Benutzeragent definiert. Die ControlAdapter rendert das Steuerelement über die Render-Methode. Aus diesem Grund, wenn Sie die Render-Methode überschreiben, dürfen Sie nicht Rendern in der Basisklasse aufrufen. Auf diese Weise kann dazu führen, dass Rendern und einmal für den Adapter und einmal für das Steuerelement selbst erfolgen.

## <a name="developing-a-custom-adapter"></a>Entwickeln eines benutzerdefinierten Adapters

Sie können einen eigenen benutzerdefinierten Adapter entwickeln, durch Erben von ControlAdapter. Darüber hinaus können Sie von der abstrakten Klasse PageAdapter in Fällen erben, wo ein Adapter für eine Seite erforderlich ist. Zuordnung der Steuerelemente an Ihren benutzerdefinierten Adapter erfolgt über die &lt;ControlAdapters&gt; Element in der Browserdefinitionsdatei. Das folgende XML aus einer Definitionsdatei für den Browser ordnet z. B. das Menüsteuerelement der MenuAdapter-Klasse:

[!code-html[Main](server-controls/samples/sample10.html)]

Dieses Modell verwenden, wird es ganz einfach für einen Steuerelemententwickler, die ein bestimmtes Gerät oder einen Browser. Es ist auch ziemlich einfach, ein Entwickler hat vollständige Kontrolle über die Seiten wie auf jedem Gerät zu rendern.

## <a name="per-device-rendering"></a>Pro-Gerät-Rendering

Eigenschaften des Server-Steuerelemente in ASP.NET 2.0 können angegebenen pro Gerät mit einem Präfix browserspezifischen sein. Der folgende Code ändert beispielsweise den Text einer Bezeichnung, je nachdem welches Gerät verwendet wird, um die Seite zu navigieren.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Wenn die Seite, die mit dieser Bezeichnung von Internet Explorer angezeigt wird, wird die Bezeichnung Anzeigetext sagen "Sie in InternetExplorer durchsuchen." Wenn die Seite aus einer Firefox durchsucht wird, wird die Bezeichnung der Text angezeigt "Sie aus einer Firefox durchsuchen." Wenn die Seiten von jedem anderen Gerät aus aufgerufen werden, werden angezeigt "Sie sind über ein unbekanntes Gerät durchsuchen." Eine beliebige Eigenschaft kann mit dieser speziellen Syntax angegeben werden.

## <a name="setting-focus"></a>Festlegen des Fokus

Häufig gestellte ASP.NET 1.x-Entwickler zu Vorgehensweise anfänglich auf den für ein bestimmtes Steuerelement festgelegt. Klicken Sie auf eine Anmeldeseite ist es beispielsweise sinnvoll, die Benutzer-ID-Textfeld den Fokus erhalten beim ersten der Seite laden. Erforderlich, in ASP.NET 1.x, dadurch einige Client-seitige Skript schreiben. Obwohl solche ein Skript eine einfache Aufgabe ist, ist es nicht mehr in ASP.NET 2.0 Dank der SetFocus-Methode erforderlich. Die SetFocus-Methode akzeptiert ein Argument, der angibt, des Steuerelements, das Fokus erhalten soll. Dieses Argument kann es sich entweder um die Client-ID des Steuerelements als Zeichenfolge oder den Namen des Steuerelements als ein Steuerelementobjekt sein. Um den Anfangsfokus auf einem TextBox-Steuerelement namens TxtUserID, beim ersten der Seite laden festzulegen, fügen Sie z. B. den folgenden Code zur Seite\_laden:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

-- oder

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 wird den Webresource.axd-Handler (wie zuvor erläutert) verwendet, um eine Client-Side-Funktion zu rendern, die den Fokus festlegt. Der Name der Funktion der clientseitigen ist WebForm\_AutoFocus wie hier gezeigt:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternativ können Sie die Focus-Methode für ein Steuerelement, auf dieses Steuerelement den Anfangsfokus. Die Focus-Methode wird von der Control-Klasse abgeleitet und ist für alle Steuerelemente von ASP.NET 2.0 verfügbar. Es ist auch möglich, den Fokus zu einem bestimmten Steuerelement festlegen, wenn ein Validierungsfehler auftritt. Wird in einem Modul weiter unten erläutert.

## <a name="new-server-controls-in-aspnet-20"></a>Neue Serversteuerelemente in ASP.NET 2.0

Es folgen neue Steuerelemente in ASP.NET 2.0. Wir werden die ausführlicher auf einige der in späteren Module.

## <a name="imagemap-control"></a>ImageMap-Steuerelement

Die ImageMap-Steuerelement ermöglicht es Ihnen um Hotspots zu einem Bild hinzuzufügen, die ein Postback-Ereignis zu initiieren oder zu einer URL navigieren können. Es gibt drei Arten von Hotspots verfügbar; CircleHotSpot RectangleHotSpot und polygonförmigen. Hotspots werden über ein auflistungs-Editor in Visual Studio oder programmgesteuert in Code hinzugefügt. Steht keine Benutzeroberfläche für das Zeichnen von Hotspots in einem Bild zur Verfügung. Die Koordinaten und Größe oder RADIUS-des-Hotspots muss deklarativ angegeben werden. Es ist auch keine visuelle Darstellung eines Hotspots im Designer. Wenn Sie ein Hotspot für die Navigation zu einer URL konfiguriert ist, wird die URL über die NavigateUrl-Eigenschaft des Hotspots angegeben. Sichern Sie bei einer Post-Hotspot, der PostBackValue Eigenschaft können Sie serverseitigen Code übergeben eine Zeichenfolge in der POST-Anforderung zurück, die abgerufen werden kann.


![HotSpot-Auflistungs-Editor in Visual Studio](server-controls/_static/image1.jpg)

**Abbildung 1**: HotSpot-Auflistungs-Editor in Visual Studio


## <a name="bulletedlist-control"></a>BulletedList-Steuerelement

Der BulletedList-Steuerelement ist eine Aufzählung, die ganz einfach Daten gebunden werden können. Die Liste (nummerierte) sortiert werden kann oder nicht über die BulletStyle-Eigenschaft sortiert. Jedes Element in der Liste wird durch ein ListItem-Objekt dargestellt.


![BulletedList-Steuerelement in Visual Studio](server-controls/_static/image1.gif)

**Abbildung 2**: BulletedList-Steuerelement in Visual Studio


## <a name="hiddenfield-control"></a>HiddenField-Steuerelement

Das HiddenField-Steuerelement fügt ein ausgeblendetes Formularfeld auf die Seite, deren, die Wert in serverseitigem Code verfügbar ist. Der Wert des einem ausgeblendeten Formularfeld muss in der Regel zwischen Postbacks unverändert bleiben. Allerdings ist es möglich, dass ein böswilliger Benutzer den Wert vor, um zurückgesendet wird, zu ändern. In diesem Fall wird das HiddenField-Steuerelement, das ValueChanged-Ereignis ausgelöst. Wenn Sie vertrauliche Informationen, im HiddenField-Steuerelement haben und Sie möchten sicherstellen, dass es unverändert bleibt, sollten Sie das ValueChanged-Ereignis in Ihrem Code behandeln.

## <a name="fileupload-control"></a>"FileUpload"-Steuerelement

Der FileUpload-Steuerelement in ASP.NET 2.0 ermöglicht das Hochladen von Dateien auf einem Webserver über eine ASP.NET-Seite. Dieses Steuerelement ist sehr ähnlich ist, auf die ASP.NET 1.x HtmlInputFile-Klasse mit wenigen Ausnahmen. In ASP.NET 1.x war es empfohlen, dass die PostedFile-Eigenschaft auf Null überprüft werden, um zu bestimmen, ob Sie eine gute Datei hatten. Der FileUpload-Steuerelement in ASP.NET 2.0 fügt eine neue HasFile-Eigenschaft, Sie für denselben Zweck können und es etwas effizienter ist.

Die PostedFile-Eigenschaft ist für den Zugriff auf ein Objekt HttpPostedFile weiterhin verfügbar, aber einige Funktionen des die HttpPostedFile ist jetzt systemintern mit dem "FileUpload"-Steuerelement verfügbar. Z. B. speichern eine hochgeladenen Datei in ASP.NET 1.x, rufen Sie die SaveAs-Methode für das HttpPostedFile-Objekt. Mithilfe des Steuerelements "FileUpload" in ASP.NET 2.0 würden Sie die SaveAs-Methode für das "FileUpload"-Steuerelement selbst aufrufen.

Eine weitere wichtige Änderung in der 2.0-Verhalten (und wahrscheinlich die wichtigste Neuerung) ist, dass es nicht mehr erforderlich, um eine gesamte hochgeladene Datei in den Arbeitsspeicher geladen werden, bevor die Änderungen gespeichert ist. In 1.x, wird jede Datei, die hochgeladen wurde vollständig in den Arbeitsspeicher vor der geschriebenen gespeichert auf dem Datenträger. Diese Architektur wird verhindert, dass das Hochladen großer Dateien.

In ASP.NET 2.0 das RequestLengthDiskThreshold-Attribut des HttpRuntime-Elements können Sie konfigurieren, wie viele Kilobyte befinden sich in einem Puffer im Arbeitsspeicher geschrieben werden, bevor Sie auf dem Datenträger.

**WICHTIGE**: MSDN-Dokumentation (und Dokumentation an anderer Stelle) gibt an, dass dieser Wert in Bytes (nicht "KB") ist standardmäßig 256 lautet. Der Wert tatsächlich in Kilobyte angegeben wird, und der Standardwert ist 80. Wenn Sie einen Standardwert von 80 KB, stellen wir sicher, dass der Puffer nicht auf den großen Objektheap erhalten wird.

## <a name="wizard-control"></a>Assistenten-Steuerelement

Es ist recht häufig Probleme mit der beim Abrufen von Informationen in einer Reihe von "Seiten" Verwenden von Bereichen oder durch die Übertragung von Seite zu Seite ASP.NET-Entwickler auftreten. Meistens, die Unterfangen ist ein frustrierend und Zeit in Anspruch nehmen. Das neue Assistenten-Steuerelement löst die Probleme durch die Funktionen für lineare und nicht linearen Schritte in eine Assistentenschnittstelle, der Benutzer mit vertraut sind. Die Assistenten-Steuerelement bietet Eingabeformulare in eine Reihe von Schritten an. Jeder Schritt ist eines bestimmten Typs, die von der StepType-Eigenschaft des Steuerelements angegeben. Die verfügbaren Typen lauten wie folgt aus:

| **Schritttyp** | **Erläuterung** |
| --- | --- |
| Auto | Der Assistent bestimmt automatisch den Typ des Schritts auf Grundlage seiner Position innerhalb der Hierarchie Schritt. |
| Starten | Der erste Schritt, häufig verwendet, um eine einführende Anweisung darzustellen. |
| Schritt | Ein normaler Schritt. |
| Fertig stellen | Der letzte Schritt, in der Regel verwendet, um eine Schaltfläche zum Beenden des Assistenten zu präsentieren. |
| Abgeschlossen | Stellt eine Nachricht, die Kommunikation von Erfolg oder Fehler. |

> [!NOTE]
> Die Assistenten-Steuerelement verfolgt des seinen Zustand mithilfe von ASP.NET Steuerelementzustand. Aus diesem Grund kann die EnableViewState-Eigenschaft auf "false", ohne alle Nachteil festgelegt werden.


Dieses Video ist eine exemplarische Vorgehensweise für die Assistenten-Steuerelement.


![](server-controls/_static/image2.png)


[Open Vollbild-Video](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localize-Steuerelement

Das Localize-Steuerelement ist ein literales Steuerelement ähnelt. Das Localize-Steuerelement hat jedoch einen **Modus** Eigenschaft, die steuert, wie das Markup, das hinzugefügt wird gerendert wird. Die Mode-Eigenschaft unterstützt die folgenden Werte:

| **Modus** | **Erläuterung** |
| --- | --- |
| Transformation | Markup wird nach dem Protokoll des Browsers die Anforderung umgewandelt. |
| PassThrough | Als Markup gerendert – ist. |
| Codieren | Mit der HtmlEncode-Markup, das dem Steuerelement hinzugefügt wird codiert. |

## <a name="multiview-and-view-controls"></a>MultiView und Anzeigesteuerelemente

Das MultiView-Steuerelement fungiert als Container für Steuerelemente und das Steuerelement fungiert als Container für andere Steuerelemente (ähnlich einem Auswahlbereich-Steuerelement). Jede Sicht in ein MultiView-Steuerelement wird von einem einzelnen Steuerelement dargestellt. Die erste Ansicht-Steuerelement in der MultiView ist die Ansicht 0, die zweite Ansicht 1, usw. Sie können zwischen Sichten wechseln, durch Angabe der ActiveViewIndex von MultiView-Steuerelement.

## <a name="substitution-control"></a>Ersetzungssteuerelement

Das Steuerelement für die Ersetzung wird in Verbindung mit der Zwischenspeicherung in ASP.NET verwendet. In Fällen, in denen Sie Zwischenspeichern nutzen möchten, aber Sie Teile einer Seite, die bei jeder Anforderung (das heißt, Teile einer Seite, die von der Zwischenspeicherung ausgenommen sind) müssen aktualisiert werden, haben, enthält die Ersetzung-Komponente eine hervorragende Lösung. Das Steuerelement rendern nicht tatsächlich keine Ausgabe selbst. Stattdessen ist es für eine Methode im serverseitigen Code gebunden. Wenn die Seite angefordert wird, wird die Methode wird aufgerufen, und das zurückgegebene Markup wird anstelle der Ersetzung-Steuerelements gerendert.

Die Methode, die an die das Ersatz-Steuerelement gebunden ist angegeben ist, über die **MethodName** Eigenschaft. Diese Methode muss die folgenden Kriterien erfüllen:

- Es muss eine statische (shared in VB) Methode sein.
- Es akzeptiert einen Parameter vom Typ "HttpContext".
- Es gibt eine Zeichenfolge, die das Markup, das das Steuerelement auf der Seite ersetzen soll.

Das Steuerelement für die Ersetzung besitzt nicht die Berechtigung zum Ändern anderer Steuerelemente auf der Seite, aber er besitzt Zugriff auf den aktuellen HttpContext über als Parameter.

## <a name="gridview-control"></a>GridView-Steuerelement

Das GridView-Steuerelement ist der Ersatz für das DataGrid-Steuerelement. Dieses Steuerelement wird in einem Modul später ausführlicher behandelt werden.

## <a name="detailsview-control"></a>DetailsView-Steuerelement

DetailsView-Steuerelement können Sie einen einzelnen Datensatz aus einer Datenquelle anzeigen und bearbeiten oder löschen. Es wird in einem Modul später ausführlicher behandelt.

## <a name="formview-control"></a>FormView-Steuerelement

Das FormView-Steuerelement wird zum Anzeigen eines einzelnen Datensatzes aus einer Datenquelle in einer konfigurierbaren-Schnittstelle. Es wird in einem Modul später ausführlicher behandelt.

## <a name="accessdatasource-control"></a>AccessDataSource-Steuerelement

AccessDataSource-Steuerelement wird verwendet, um Daten zu binden eine Access-Datenbank. Es wird in einem Modul später ausführlicher behandelt.

## <a name="objectdatasource-control"></a>ObjectDataSource-Steuerelement

Das ObjectDataSource-Steuerelement wird verwendet, um eine Architektur mit drei Ebenen zu unterstützen, sodass Steuerelemente Daten-gebunden werden können, um ein Geschäftsobjekt der mittleren Ebene im Gegensatz zu einem Modell mit zwei Ebenen, in denen Steuerelemente direkt an die Datenquelle gebunden. Sie werden in einem Modul weiter unten ausführlicher erläutert.

## <a name="xmldatasource-control"></a>XmlDataSource-Steuerelement

XmlDataSource-Steuerelement wird für die Datenbindung an eine XML-Datenquelle verwendet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource-Steuerelement

Das Steuerelement SiteMapDataSource bietet es sich um die Datenbindung für Website-Navigationssteuerelemente, die basierend auf einer Siteübersicht. Sie werden in einem Modul weiter unten ausführlicher erläutert.

## <a name="sitemappath-control"></a>SiteMapPath-Steuerelement

Das Steuerelement SiteMapPath zeigt eine Reihe von Navigationslinks, die gemeinhin als der Breadcrumb-Leiste. Es wird in einem Modul später ausführlicher behandelt.

## <a name="menu-control"></a>Menu-Steuerelement

Das Steuerelement zeigt dynamische Menüs, die mithilfe von DHTML an. Es wird in einem Modul später ausführlicher behandelt.

## <a name="treeview-control"></a>TreeView-Steuerelement

Das TreeView-Steuerelement wird verwendet, um eine hierarchische Ansicht der Daten anzuzeigen. Es wird in einem Modul später ausführlicher behandelt.

## <a name="login-control"></a>Login-Steuerelement

Die Login-Steuerelement stellt für einen Mechanismus zum Anmelden an einer Websites bereit. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginview-control"></a>LoginView-Steuerelement

Das LoginView-Steuerelement ermöglicht die Anzeige von unterschiedliche Vorlagen, die basierend auf den Anmeldestatus eines Benutzers. Es wird in einem Modul später ausführlicher behandelt.

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das Steuerelement PasswordRecovery wird vergessene Kennwörter abrufen, die Benutzer von einer ASP.NET-Anwendung verwendet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginstatus"></a>LoginStatus

Das LoginStatus-Steuerelement zeigt den Status eines Benutzers Anmeldung. Es wird in einem Modul später ausführlicher behandelt.

## <a name="loginname"></a>LoginName

Das LoginName-Steuerelement zeigt den Benutzernamen von Benutzern nach der Anmeldung bei einer ASP.NET-Anwendung. Es wird in einem Modul später ausführlicher behandelt.

## <a name="createuserwizard"></a>CreateUserWizard

Die CreateUserWizard ist ein Assistent konfiguriert die Benutzern die Möglichkeit zum Erstellen eines ASP.NET Membership-Kontos zur Verwendung in einer ASP.NET-Anwendung bietet. Es wird in einem Modul später ausführlicher behandelt.

## <a name="changepassword"></a>ChangePassword

ChangePassword-Steuerelement ermöglicht das Ändern seines Kennworts für eine ASP.NET-Anwendung. Es wird in einem Modul später ausführlicher behandelt.

## <a name="various-webparts"></a>Verschiedene WebParts

Im Lieferumfang von ASP.NET 2.0 sind verschiedene Webparts. Diese werden in einem Modul weiter unten ausführlich behandelt.
