---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Die ASP.NET 2.0-Seitenmodell | Microsoft Docs
author: microsoft
description: In ASP.NET 1.x Entwickler hatte eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. Code-Behind-kann verwenden entweder das Src-Attr implementiert werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30891294"
---
<a name="the-aspnet-20-page-model"></a>Die ASP.NET 2.0-Seite-Modells
====================
durch [Microsoft](https://github.com/microsoft)

> In ASP.NET 1.x Entwickler hatte eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. Code-Behind-verwenden das Src-Attribut oder die CodeBehind-Attribut des implementiert werden kann die @Page Richtlinie. In ASP.NET 2.0 können Entwickler haben weiterhin die Wahl zwischen Inlinecode und Code-Behind, jedoch deutliche Verbesserungen bei der Code-Behind-Modells stattgefunden haben.


In ASP.NET 1.x Entwickler hatte eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. Code-Behind-verwenden das Src-Attribut oder die CodeBehind-Attribut des implementiert werden kann die @Page Richtlinie. In ASP.NET 2.0 können Entwickler haben weiterhin die Wahl zwischen Inlinecode und Code-Behind, jedoch deutliche Verbesserungen bei der Code-Behind-Modells stattgefunden haben.

## <a name="improvements-in-the-code-behind-model"></a>Verbesserungen im Code-Behind-Modell

Um die Änderungen im Code-Behind-Modell in ASP.NET 2.0 vollständig zu verstehen, empfiehlt es sich, schnell zu überprüfen, das Modell wie vorhanden waren, in ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Der Code-Behind-Modells in ASP.NET 1.x

In ASP.NET 1.x, die Code-Behind-Modells umfasste eine ASPX-Datei (Webform) und ein Code-Behind-Datei, die mit Programmcode. Die beiden Dateien verbunden waren, mithilfe der @Page -Direktive in der ASPX-Datei. Jedes Steuerelement auf der ASPX-Seite wurde eine entsprechende Deklaration in der Code-Behind-Datei als Instanzvariable. Die Code-Behind-Datei auch Code für die Ereignis-Bindung enthalten und generiert Code, der für den Visual Studio-Designer erforderlich. Dieses Modell ziemlich gut funktioniert hat, aber da jedes Element von ASP.NET in der ASPX-Seite den entsprechenden Code im Code-Behind-Datei erforderlich, es wurde keine "true" Trennung von Code und Inhalt. Z. B. wenn ein Designer ein neues Serversteuerelement in einer ASPX-Datei außerhalb von Visual Studio-IDE hinzugefügt, wird die Anwendung aufgrund der Abwesenheit eine Deklaration für das entsprechende Steuerelement in der CodeBehind-Datei unterbrochen.

## <a name="the-code-behind-model-in-aspnet-20"></a>Der Code-Behind-Modells in ASP.NET 2.0

ASP.NET 2.0 wird bei diesem Modell erheblich verbessert. In ASP.NET 2.0 wird Code-Behind implementiert, mit dem neuen *Teilklassen* in ASP.NET 2.0 bereitgestellt. Der Code-Behind-Klasse in ASP.NET 2.0 ist definiert als eine partielle Klasse, was bedeutet, dass sie nur einen Teil der Klassendefinition enthält. Der verbleibende Teil der Definition der Klasse wird von ASP.NET 2.0 mithilfe der ASPX-Seite, oder wenn die Website vorkompiliert ist zur Laufzeit dynamisch generiert. Die Verknüpfung zwischen dem Code-Behind-Datei und die ASPX-Seite wird weiterhin hergestellt, mit der @ Page-Direktive. Anstelle einer CodeBehind oder Src-Attributs verwendet ASP.NET 2.0 jedoch jetzt das CodeFile-Attribut. Das Inherits-Attribut wird auch verwendet, den Klassennamen für die Seite an.

Eine typische @ Page-Direktive kann wie folgt aussehen:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Eine normale Klassendefinition in einer ASP.NET 2.0-Code-Behind-Datei kann wie folgt aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# und Visual Basic werden nur verwalteten Sprachen, die derzeit partielle Klassen unterstützen. Entwickler mit j# können daher nicht mithilfe des Code-Behind-Modells in ASP.NET 2.0 sein.


Das neue Modell verbessert die Code-Behind-Modells, da Entwickler nun Codedateien verfügt, die nur den Code enthalten, den sie erstellt haben. Darüber hinaus eine "true" Trennung von Code und Inhalt, da es keine Instanz-Variablendeklarationen im Code-Behind-Datei gibt.

> [!NOTE]
> Da die partielle Klasse für die ASPX-Seite handelt, in dem Ereignis Bindung stattfindet, können Visual Basic-Entwickler eine geringfügige Leistungssteigerung mit sich bringen mit dem Handles-Schlüsselwort im Code-Behind-Ereignisse binden. C# verfügt über keine entsprechende Schlüsselwort.


## <a name="new--page-directive-attributes"></a>Neue Attribute für @ Page-Direktive

ASP.NET 2.0 hinzugefügt die @ Page-Direktive viele neue Attribute. Die folgenden Attribute sind neu in ASP.NET 2.0.

## <a name="async"></a>Async

Das Async-Attribut können Sie so konfigurieren Sie die Seite, die asynchron ausgeführt werden. Gut Deckblätter asynchrone weiter unten in diesem Modul aus.

## <a name="asynctimeout"></a>AsyncTimeout

Das Timeout für die asynchrone Seiten wird angegeben. Der Standardwert ist 45 Sekunden.

## <a name="codefile"></a>CodeFile

Das CodeFile-Attribut ist der Ersatz für die CodeBehind-Attribut in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Das CodeFileBaseClass-Attribut wird in Fällen verwendet, in denen mehrere Seiten aus einer einzigen Basisklasse abgeleitet werden soll. Aufgrund der Implementierung der partiellen Klassen in ASP.NET ohne dieses Attribut eine Basisklasse, die freigegebene allgemeine Felder verwendet, um die Steuerelemente in einer ASPX-Seite deklariert verweisen funktioniert nicht ordnungsgemäß da ASP. Netze Kompilierung Modul erstellt automatisch neue Member, die basierend auf Steuerelemente auf der Seite. Aus diesem Grund Wenn Sie eine allgemeine Basisklasse für zwei oder mehr Seiten in ASP.NET möchten, benötigen Sie zum Definieren Ihrer Basisklasse in das CodeFileBaseClass-Attribut angeben, und klicken Sie dann jede Seiten-Klasse ableiten, von dieser Basisklasse. Das CodeFile-Attribut ist auch erforderlich, wenn dieses Attribut verwendet wird.

## <a name="compilationmode"></a>CompilationMode

Dieses Attribut können Sie die Eigenschaft CompilationMode die ASPX-Seite festgelegt. Die CompilationMode-Eigenschaft ist eine Enumeration, die mit den Werten **immer**, **Auto**, und **nie**. Die Standardeinstellung ist **immer**. Die **Auto** Einstellung verhindert, dass ASP.NET dynamisch möglichst Kompilieren der Seite. Ausschließen von Seiten von der dynamischen Kompilierung wird die Leistung erhöht. Jedoch, wenn eine Seite, die ausgeschlossen ist dieser Code, die kompiliert werden muss enthält, wird ein Fehler ausgelöst werden beim Durchsuchen der Seite.

## <a name="enableeventvalidation"></a>EnableEventValidation

Dieses Attribut gibt an, und zwar unabhängig davon, ob Postback und Rückrufereignisse überprüft werden. Wenn diese Option aktiviert ist, werden Argumente für das postback oder Rückrufereignisse überprüft, um sicherzustellen, dass sie das Webserversteuerelement stammt, die ursprünglich gerendert.

## <a name="enabletheming"></a>EnableTheming

Dieses Attribut gibt an, und zwar unabhängig davon, ob ASP.NET-Designs auf einer Seite verwendet werden. Der Standardwert ist **FALSE**. ASP.NET-Designs finden Sie [Modul 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Dieses Attribut gibt an, ob während der Kompilierung Zeilenpragmas hinzugefügt werden soll. Zeilenpragmas sind Optionen, die von Debuggern verwendet wird, um bestimmte Abschnitte des Codes zu markieren.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Dieses Attribut gibt an, und zwar unabhängig davon, ob JavaScript in die Seite eingefügt wird, um die Bildlaufposition zwischen Postbacks aufrechtzuerhalten. Dieses Attribut ist **"false"** standardmäßig.

Wenn dieses Attribut ist **"true"**, ASP.NET hinzufügen wird ein &lt;Skript&gt; Sperre für das Postback, die wie folgt aussieht:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Beachten Sie, dass das Src für dieses Skriptblocks "WebResource.axd" ist. Diese Ressource ist kein physischen Pfad. Wenn dieses Skript angefordert wird, erstellt das Skript ASP.NET dynamisch.

### <a name="masterpagefile"></a>MasterPageFile

Dieses Attribut gibt die Masterseitendatei für die aktuelle Seite. Der Pfad kann relativ oder absolut sein. Gestaltungsvorlagen werden in behandelt [Modul 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Dieses Attribut können Sie Benutzeroberfläche Darstellungseigenschaften, die durch ein ASP.NET 2.0 Design definierten überschreiben. Designs werden in behandelt [Modul 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Design

Gibt das Design für die Seite an. Wenn ein Wert für das StyleSheetTheme-Attribut nicht angegeben ist, überschreibt das Theme-Attribut alle Stile an Steuerelemente auf der Seite.

## <a name="title"></a>Titel

Legt den Titel für die Seite fest. Der hier angegebene Wert wird angezeigt, der &lt;Titel&gt; Element der gerenderten Seite.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Legt den Wert für die ViewStateEncryptionMode-Enumeration. Die verfügbaren Werte sind **immer**, **Auto**, und **nie**. Der Standardwert ist **Auto**. Wenn dieses Attribut auf einen Wert festgelegt ist **Auto**, "ViewState" Speichern verschlüsselt wird ein Steuerelement fordert er durch Aufrufen der **RegisterRequiresViewStateEncryption** Methode.

## <a name="setting-public-property-values-via-the--page-directive"></a>Festlegen von öffentlichen Eigenschaftswerten über den @ Page-Direktive

Eine weitere neue Funktion der @ Page-Direktive in ASP.NET 2.0 ist die Möglichkeit, den anfänglichen Wert des öffentlichen Eigenschaften einer Basisklasse festzulegen. Angenommen, z. B. stehen Ihnen eine öffentliche Eigenschaft mit dem Namen **Irgendein_text** in Ihrer Basisklasse und d, wie es Sie initialisiert werden, um **Hello** beim Laden einer Seite. Sie können dies bewerkstelligen, indem einfach der Wert in der @ Page-Direktive wie folgt:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Die **Irgendein_text** -Attribut der @ Page-Direktive legt den Anfangswert der Irgendein_text-Eigenschaft in der Basisklasse *Hello!*. Im folgenden Video wird eine exemplarische Vorgehensweise zum der Anfangswert von einer öffentlichen Eigenschaft in einer Basisklasse, die mit der @ Page-Direktive.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Neue öffentliche Eigenschaften der Page-Klasse

Die folgenden öffentlichen Eigenschaften sind neu in ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Gibt den anwendungsrelativen Pfad zu der Seite oder ein Steuerelement zurück. Z. B. für eine Seite, die am http://app/folder/page.aspx, gibt die Eigenschaft ~ / Ordner /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Gibt den relativen Pfad zu der Seite oder ein Steuerelement zurück. Beispiel für eine Seite, die am http://app/folder/page.aspx, gibt die Eigenschaft ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Ruft ab oder legt das Timeout für den Umgang mit asynchronen Seite verwendet. (Asynchrone Seiten werden weiter unten in diesem Modul behandelt.)

## <a name="clientquerystring"></a>ClientQueryString

Eine schreibgeschützte Eigenschaft, die Zeichenfolge Abfrageteil der angeforderten URL zurückgibt. Dieser Wert ist die URL-codiert. Die UrlDecode-Methode der Klasse HttpServerUtility können decodiert werden.

## <a name="clientscript"></a>ClientScript

Diese Eigenschaft gibt ein ClientScriptManager-Objekt, das zum Verwalten von ASP.NETs Emission von clientseitigem Skript verwendet werden kann. (Die ClientScriptManager-Klasse wird weiter unten in diesem Modul behandelt.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Diese Eigenschaft steuert, ob Ereignis Validierung für Postback und Rückruf-Ereignisse aktiviert ist. Wenn aktiviert, werden Argumente für das postback oder Rückrufereignisse überprüft, um sicherzustellen, dass sie das Webserversteuerelement stammt, die ursprünglich gerendert.

## <a name="enabletheming"></a>EnableTheming

Diese Eigenschaft ruft ab oder legt einen booleschen Wert ab, der angibt, ob eine ASP.NET 2.0 Design auf der Seite angewendet wird.

## <a name="form"></a>Formular

Diese Eigenschaft gibt das HTML-Formular auf der ASPX-Seite als ein HtmlForm-Objekt zurück.

## <a name="header"></a>Header

Diese Eigenschaft gibt einen Verweis auf ein HtmlHead-Objekt, das den Seitenkopf enthält. Das zurückgegebene Objekt der HtmlHead können Get/Set-Stylesheets, Metatags usw.

## <a name="idseparator"></a>IdSeparator

Diese schreibgeschützte Eigenschaft ruft das Zeichen, das verwendet wird, um das Steuerelement-IDs zu trennen, wenn ASP.NET eine eindeutige ID für die Steuerelemente auf einer Seite aufbaut. Sie sollte nicht direkt im Code verwendet werden.

## <a name="isasync"></a>IsAsync

Diese Eigenschaft ermöglicht die asynchrone Seiten. Asynchrone Seiten werden weiter unten in diesem Modul erläutert.

## <a name="iscallback"></a>IsCallback

Gibt diese schreibgeschützte Eigenschaft **"true"** der Seite "ist das Ergebnis eines Aufrufs zurück. Rückfragen werden weiter unten in diesem Modul erläutert.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Gibt diese schreibgeschützte Eigenschaft **"true"** Wenn die Seite ein Postback Cross-Seite gehört. Cross-Seite Postbacks werden weiter unten in diesem Modul behandelt.

## <a name="items"></a>Elemente

Gibt einen Verweis auf ein IDictionary-Instanz, die alle Objekte in den Kontext Seiten enthält. Sie können dieses IDictionary-Objekt Elemente hinzu, und werden während der Lebensdauer des Kontexts zur Verfügung.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Diese Eigenschaft steuert, ob ASP.NET JavaScript ausgibt, die verwaltet, dass die Seiten Bildlaufposition im Browser nach einem Postback. (Details zu dieser Eigenschaft wurden zuvor in diesem Modul erläutert.)

## <a name="master"></a>Master

Diese schreibgeschützte Eigenschaft gibt einen Verweis auf die Instanz MasterPage für eine Seite, die eine Masterseite angewendet wurde.

## <a name="masterpagefile"></a>MasterPageFile

Ruft ab oder legt den Dateinamen der Gestaltungsvorlage für die Seite. Diese Eigenschaft kann nur in der PreInit-Methode festgelegt werden.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Diese Eigenschaft ruft ab oder legt die maximale Länge für den Status der Seiten in Bytes fest. Wenn die Eigenschaft auf eine positive Zahl festgelegt wird, wird der Ansichtszustand von Seiten in mehrere ausgeblendete Felder aufgeteilt werden, damit es die angegebene Anzahl von Bytes nicht überschreitet. Wenn die Eigenschaft eine negative Zahl ist, wird der Ansichtszustand nicht in Blöcke aufgeteilt werden.

## <a name="pageadapter"></a>PageAdapter

Gibt einen Verweis auf die PageAdapter-Objekt, das die Seite für den anfordernden Browser ändert.

## <a name="previouspage"></a>PreviousPage

Gibt einen Verweis zur vorherigen Seite bei einem Server.Transfer oder ein Postback Cross-Seite zurück.

## <a name="skinid"></a>SkinID

Gibt an, die ASP.NET 2.0-Designs, das auf der Seite anwenden.

## <a name="stylesheettheme"></a>StyleSheetTheme

Diese Eigenschaft ruft ab oder legt das Stylesheet, das auf einer Seite angewendet wird.

## <a name="templatecontrol"></a>TemplateControl

Gibt einen Verweis auf das entsprechende Steuerelement für die Seite.

## <a name="theme"></a>Design

Ruft ab oder legt den Namen des Designs ASP.NET 2.0 auf der Seite "angewendet. Dieser Wert muss vor der PreInit-Methode festgelegt werden.

## <a name="title"></a>Titel

Diese Eigenschaft ruft ab oder legt den Titel für die Seite fest, wie aus dem Header Seiten abgerufen.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Abrufen oder Festlegen der ViewStateEncryptionMode Rand der Seite. Finden Sie ausführliche Informationen zu dieser Eigenschaft weiter oben in diesem Modul aus.

## <a name="new-protected-properties-of-the-page-class"></a>Neue geschützte Eigenschaften der Page-Klasse

Es folgen die neue geschützten Eigenschaften der Page-Klasse in ASP.NET 2.0.

## <a name="adapter"></a>Adapter

Gibt ein Verweis auf die ControlAdapter, die der Seite auf dem Gerät, rendert sie angefordert.

## <a name="asyncmode"></a>AsyncMode

Diese Eigenschaft gibt an, und zwar unabhängig davon, ob die Seite asynchron verarbeitet wird. Es ist für die Verwendung durch die Common Language Runtime und nicht direkt im Code vorgesehen.

## <a name="clientidseparator"></a>ClientIDSeparator

Diese Eigenschaft gibt das Zeichen als Trennzeichen verwendet werden, wenn eindeutige IDs für Steuerelemente erstellen. Es ist für die Verwendung durch die Common Language Runtime und nicht direkt im Code vorgesehen.

## <a name="pagestatepersister"></a>PageStatePersister

Diese Eigenschaft gibt das PageStatePersister für die Seite zurück. Diese Eigenschaft wird hauptsächlich von Steuerelemententwicklern ASP.NET verwendet.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Diese Eigenschaft gibt eine eindeutige Suffic, das den Dateipfad für das caching Browser angefügt wird. Der Standardwert ist \_ \_Ufps = und eine Zahl 6 Ziffern.

## <a name="new-public-methods-for-the-page-class"></a>Neue öffentliche Methoden für die Page-Klasse

Die folgenden öffentlichen Methoden sind noch nicht mit der Page-Klasse in ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Diese Methode registriert Ereignishandlerdelegaten für asynchrone seitenausführung. Asynchrone Seiten werden weiter unten in diesem Modul erläutert.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Die Seite die Eigenschaften in einem Stylesheet Seiten zugewiesen.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Diese Methode einer Sprache umsetzen einer asynchronen Aufgabe.

### <a name="getvalidators"></a>GetValidators

Gibt eine Auflistung der Validierungssteuerelemente für die der angegebenen Validierungsgruppe oder die Standardgruppe für die Validierung zurück, falls keiner angegeben ist.

## <a name="registerasynctask"></a>RegisterAsyncTask

Diese Methode wird eine neue asynchrone Aufgabe registriert. Asynchrone Seiten werden weiter unten in diesem Modul behandelt.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Diese Methode weist ASP.NET, dass der Steuerelementzustand Seiten beibehalten werden muss.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Diese Methode weist ASP.NET, dass die Seiten "ViewState" Speichern Verschlüsselung erforderlich ist.

## <a name="resolveclienturl"></a>ResolveClientUrl

Gibt eine relative URL, die für Clientanforderungen für Bilder usw. verwendet werden kann.

## <a name="setfocus"></a>SetFocus

Diese Methode wird den Fokus auf das Steuerelement gesetzt werden, der beim erstmaligen der Seite laden angegeben wird.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Diese Methode wird das Steuerelement aufgehoben, das an sie übergeben wird, nicht mehr erfordern, Statuspersistenz Steuerelement.

## <a name="changes-to-the-page-lifecycle"></a>Änderungen an der Seite "-Lebenszyklus

Der Lebenszyklus der Seite in ASP.NET 2.0 wurde nicht erheblich geändert, aber es gibt einige neuen Methoden, die Sie kennen sollten. Der Lebenszyklus der ASP.NET 2.0-Seite wird im folgenden erläutert.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (neu in ASP.NET 2.0)

Das PreInit-Ereignis ist der frühesten Stufe im Lebenszyklus, den ein Entwickler zugreifen können. Das Hinzufügen dieses Ereignisses erleichtert programmgesteuert ändern von ASP.NET 2.0-Designs, Masterseiten, Eigenschaften für eine ASP.NET 2.0-Profil usw. zugreifen können. Wenn Sie befinden sich in einem postback Zustand, dessen wichtig, beachten Sie, dass "ViewState" speichern noch nicht auf Steuerelemente zu diesem Zeitpunkt im Lebenszyklus angewendet wurden. Aus diesem Grund, wenn ein Entwickler eine Eigenschaft eines Steuerelements in dieser Phase geändert wird, werden sie wahrscheinlich später im Lebenszyklus Seiten überschrieben.

## <a name="init"></a>Init

Die Init-Ereignis wurde von ASP.NET 1.x. Dies ist, in dem Sie möchten lesen oder initialisieren die Eigenschaften der Steuerelemente auf der Seite. Auf dieser Stufe, Masterseiten, Designs usw. bereits gelten für die Seite.

## <a name="initcomplete-new-in-20"></a>InitComplete (neu in 2.0)

Das Ereignis InitComplete wird am Ende der Initialisierungsphase Seiten aufgerufen. Zu diesem Zeitpunkt im Lebenszyklus, Sie können Zugriff auf Steuerelemente auf der Seite, aber ihren Status wurde noch nicht aufgefüllt.

## <a name="preload-new-in-20"></a>Vorab zu laden (neu in 2.0)

Dieses Ereignis wird aufgerufen, nachdem alle Postbackdaten angewendet wurde und unmittelbar vor Seite\_laden.

## <a name="load"></a>Load

Das Load-Ereignis wurde von ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (neu in 2.0)

Das LoadComplete-Ereignis ist das letzte Ereignis in der Ladephase Seiten. Zu diesem Zeitpunkt alle Postback und "ViewState" speichern Daten angewendet wurde auf der Seite.

## <a name="prerender"></a>PreRender

Wenn Sie für "ViewState" Speichern für Steuerelemente ordnungsgemäß verwaltet werden, die dynamisch auf der Seite hinzugefügt werden soll, ist das PreRender-Ereignis die letzte Gelegenheit, um sie hinzuzufügen.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (neu in 2.0)

In der Phase PreRenderComplete Seite alle Steuerelemente hinzugefügt wurden, und die Seite gerendert werden kann. Das PreRenderComplete-Ereignis ist das letzte Ereignis ausgelöst wird, vor dem Speichern der Seiten "ViewState" speichern.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (neu in 2.0)

Das SaveStateComplete-Ereignis wird aufgerufen, unmittelbar nachdem alle Seite "ViewState" Speichern und Steuerelementzustand gespeichert wurde. Dies ist das letzte Ereignis, bevor die Seite tatsächlich an den Browser gerendert wird.

## <a name="render"></a>Rendern

Die Render-Methode wurde nicht geändert, seit ASP.NET 1.x. Dies ist, in dem das HtmlTextWriter wird initialisiert, und die Seite an den Browser gerendert wird.

## <a name="cross-page-postback-in-aspnet-20"></a>Cross-Seite Postback in ASP.NET 2.0

In ASP.NET 1.x Postbacks musste zur selben Seite bereitstellen. Cross-Seite Postbacks wurden nicht zulässig. ASP.NET 2.0 bietet die Möglichkeit zum Zurücksenden an eine andere Seite über die IButtonControl-Schnittstelle. Jedes Steuerelement, das die neue IButtonControl-Schnittstelle (Schaltfläche LinkButton und ImageButton zusätzlich zu benutzerdefinierten Steuerelementen eines Drittanbieters) implementiert kann diese neue Funktionalität über die Verwendung des Attributs PostBackUrl nutzen. Der folgende Code zeigt ein Schaltflächen-Steuerelement, das an eine zweite Seite zurückgesendet.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Wenn die Seite zurückgesendet wird, ist die Seite, die das Postback initiiert über die PreviousPage-Eigenschaft auf der zweiten Seite zugegriffen werden kann. Diese Funktionalität wird über die neue WebForm implementiert\_DoPostBackWithOptions clientseitige Funktion, die ASP.NET 2.0 auf der Seite gerendert wird, wenn ein Steuerelement an eine andere Seite zurückgesendet. Die JavaScript-Funktion wird vom Handler für die neue "WebResource.axd" bereitgestellt, das Skript an den Client ausgibt.

Im folgenden Video wird eine exemplarische Vorgehensweise für ein Postback Cross-Seite.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Weitere Informationen zu den seitenübergreifend Postbacks

### <a name="viewstate"></a>"ViewState" Speichern

Möglicherweise haben sich bereits Informationen dazu, was auf die "ViewState" Speichern von der ersten Seite in einem Szenario mit Cross-Seite postback. Nachdem alle jedes Steuerelement, das keine IPostBackDataHandler implementiert bleiben erhalten, seinen Status über "ViewState" speichern, also zum Zugriff auf die Eigenschaften des Steuerelements auf der zweiten Seite des ein Postback Cross-Seite haben, benötigen Sie Zugriff auf die "ViewState" speichern, für die Seite. ASP.NET 2.0 kümmert sich dieses Szenario mit einem neuen ausgeblendeten Feld in der zweiten Seite aufgerufen \_ \_PREVIOUSPAGE. Die \_ \_PREVIOUSPAGE Formularfeld enthält die "ViewState" speichern, für die erste Seite, damit Sie Zugriff auf die Eigenschaften aller Steuerelemente in der zweiten Seite haben können.

### <a name="circumventing-findcontrol"></a>FindControl umgehen

Im dem video zur exemplarischen Vorgehensweise ein Postback Cross-Seite verwendet die FindControl-Methode zum Abrufen eines Verweises auf das Textfeld-Steuerelement auf der ersten Seite. Diese Methode eignet sich gut für diesen Zweck aber FindControl ist teuer, und es muss zusätzlicher Code geschrieben. Glücklicherweise bietet ASP.NET 2.0 eine Alternative zum FindControl für diesen Zweck, die in vielen Szenarien geeignet ist. Die PreviousPageType-Direktive können Sie einen stark typisierten Verweis zur vorherigen Seite mit den angegebenen Typnamen oder das Attribut "VirtualPath" haben. Die TypeName-Attribut können Sie geben Sie den Typ der vorherigen Seite aus, wenn Sie das Attribut "VirtualPath" können Sie zur vorherigen Seite mit einem virtuellen Pfad zu verweisen. Nach dem Festlegen der PreviousPageType-Direktive, müssen Sie verfügbar machen, den Zugriff mithilfe von öffentlichen Eigenschaften werden soll usw. und Steuerelemente.

## <a name="lab-1-cross-page-postback"></a>Übungseinheit 1 seitenübergreifend Postback

In dieser Übung erstellen Sie eine Anwendung, die die neue postback Cross-Page-Funktionen von ASP.NET 2.0 verwendet.

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie eine neue Webform page2.aspx aufgerufen.
3. Öffnen Sie die "default.aspx" in der Entwurfsansicht, und fügen ein Schaltflächen-Steuerelement und ein TextBox-Steuerelement. 

    1. Geben Sie dem Schaltflächensteuerelement ID **SubmitButton** und die TextBox-Steuerelement ID **Benutzername**.
    2. Legen Sie die PostBackUrl-Eigenschaft der Schaltfläche, um page2.aspx.
4. Öffnen Sie in der Quellansicht page2.aspx.
5. Fügen Sie eine @ PreviousPageType-Direktive hinzu, wie unten dargestellt:
6. Fügen Sie den folgenden Code zur Seite\_Laden von Page2.aspx des Code-Behind: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Erstellen Sie das Projekt, indem Sie im Menü erstellen auf Build.
8. Fügen Sie den folgenden Code, um den Code-Behind für "default.aspx": 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Die Seite "ändern"\_Laden in page2.aspx, der dem folgenden: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Erstellen Sie das Projekt.
11. Führen Sie das Projekt aus.
12. Geben Sie Ihren Namen in das Textfeld ein, und klicken Sie auf die Schaltfläche "".
13. Was ist das Ergebnis?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchrone Seiten in ASP.NET 2.0

Viele Konflikte Probleme bei der ASP.NET werden durch die Latenz von externen aufrufen (z. B. Web-Dienst oder die Datenbank-Aufrufe), Datei-e/a-Latenz usw. verursacht. Wird für eine ASP.NET-Anwendung eine Anforderung gestellt wird, verwendet ASP.NET eine von den Arbeitsthreads um diese Anforderung zu erfüllen. Diese Anforderung besitzt dieser Thread, bis die Anforderung abgeschlossen ist und die Antwort gesendet wurde. ASP.NET 2.0 versucht, Probleme mit dieser Art von Problem zu beheben, indem Sie die Möglichkeit, Seiten asynchron ausgeführt werden soll. Das bedeutet, dass ein Arbeitsthread kann die startanforderung für und dann zusätzliche Ausführung an einen anderen Thread leiten, dadurch schnell Rückgabe an den Threadpool verfügbar. Wenn die Datei-e/a, Datenbankaufruf usw. abgeschlossen wurde, wird ein neuer Thread aus dem Threadpool zum Beenden der Anforderung abgerufen.

Der erste Schritt darin, eine Seite, die asynchron ausgeführt wird, Festlegen der **Async** -Attribut der Seitendirektive wie folgt:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Dieses Attribut weist ASP.NET die IHttpAsyncHandler für die Seite zu implementieren.

Der nächste Schritt ist die AddOnPreRenderCompleteAsync-Methode zu einem Zeitpunkt im Lebenszyklus der Seite vor PreRender aufgerufen. (Diese Methode wird in der Regel auf der Seite aufgerufen\_laden.) Die AddOnPreRenderCompleteAsync Methode akzeptiert zwei Parameter; BeginEventHandler und eine EndEventHandler. BeginEventHandler gibt eine "IAsyncResult", die dann an die EndEventHandler als Parameter übergeben wird.

Das Bild unten ist eine exemplarische Vorgehensweise für eine asynchrone Seitenanforderung.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Eine Async-Seite wird nicht an den Browser gerendert, bis die EndEventHandler abgeschlossen ist. Zweifellos jedoch werden einige Entwickler asynchrone Anforderungen ähnelt asynchrone Rückrufe vorstellen. Es ist wichtig zu beachten, dass sie nicht sind. Die Vorteile für die asynchrone Anforderungen ist, dass der erste Thread im Threadpool für neue Anforderungen, gesenkt und Konflikte aufgrund eines e/a-gebunden, usw. zurückgegeben werden kann.


## <a name="script-callbacks-in-aspnet-20"></a>Skript-Rückrufe in ASP.NET 2.0

Webentwickler müssen immer für Möglichkeiten, um zu verhindern, dass das Flimmern einem Rückruf zugeordnet gesucht. In ASP.NET 1.x SmartNavigation wurde die häufigste Methode zum Vermeiden von Flimmern jedoch SmartNavigation verursachte Probleme für einige Entwickler wegen der Komplexität von seiner Implementierung auf dem Client. ASP.NET 2.0 löst dieses Problem mit dem Skriptrückrufe. Skript-Rückrufe verwenden XMLHttp-Anforderungen für den Webserver über JavaScript. Die XMLHttp-Anforderung gibt XML-Daten, die anschließend bearbeitet werden können über den Browser DOM. XMLHttp Code wird der Benutzer vom neuen Handler "WebResource.axd" ausgeblendet.

Es sind mehrere Schritte erforderlich, um einen Rückruf Skript in ASP.NET 2.0 konfigurieren.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Schritt 1: Implementieren Sie die ICallbackEventHandler-Schnittstelle.

In der Reihenfolge für ASP.NET Ihrer Seite als die Teilnahme an eines Rückrufs Skript erkennt müssen Sie die ICallbackEventHandler-Schnittstelle implementieren. Hierzu können Sie in der Code-Behind-Datei wie folgt:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Sie können auch dazu @ Implements-Direktive Like so:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Sie würden die @ Implements-Anweisung in der Regel verwenden, wenn ASP.NET Inlinecode zu verwenden.

## <a name="step-2--call-getcallbackeventreference"></a>Schritt 2: Rufen Sie GetCallbackEventReference

Wie bereits erwähnt, wird der Aufruf XMLHttp im Ereignishandler "WebResource.axd" gekapselt. Wenn die Seite gerendert wird, wird in ASP.NET einen Aufruf von WebForm hinzugefügt\_DoCallback, ein Clientskript, das von "WebResource.axd" bereitgestellt wird. Der WebForm\_DoCallback-Funktion ersetzt die \_ \_DoPostBack-Funktion für einen Rückruf. Beachten Sie, dass \_ \_DoPostBack programmgesteuert übermittelt das Formular auf der Seite. In einem Rückrufszenario Sie ein Postback handeln, daher verhindern möchten \_ \_DoPostBack reicht nicht aus.

> [!NOTE]
> \_\_DoPostBack wird weiterhin auf die Seite in einem Szenario mit Clients Skript Rückruf gerendert. Es wird jedoch nicht für den Rückruf verwendet.


Die Argumente für die WebForm\_DoCallback clientseitige Funktion werden bereitgestellt, über die Funktion serverseitige GetCallbackEventReference die normalerweise auf der Seite aufgerufen werden, würde\_laden. Ein typische Aufruf GetCallbackEventReference kann wie folgt aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> In diesem Fall ist cm einer Instanz von ClientScriptManager. Die Klasse ClientScriptManager wird weiter unten in diesem Modul erläutert.


Es gibt mehrere überladene Versionen GetCallbackEventReference. In diesem Fall sind die Argumente wie folgt ein:

`this`

Ein Verweis auf das Steuerelement, in denen GetCallbackEventReference aufgerufen wird. In diesem Fall ist es die Seite selbst.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Ein Zeichenfolgenargument, das an das serverseitige-Ereignis aus dem clientseitigen Code übergeben wird. In diesem Fall wird Im übergeben des Werts der Dropdownliste DdlCompany aufgerufen.

`ShowCompanyName`

Der Name der clientseitigen-Funktion, die den Rückgabewert aus dem serverseitigen Rückrufereignis (als Zeichenfolge) akzeptiert. Diese Funktion wird nur aufgerufen werden, wenn der Rückruf serverseitige erfolgreich durchgeführt wurde. Aus diesem Grund wird aus Gründen der Stabilität, im Allgemeinen empfohlen, die überladene Version der GetCallbackEventReference verwenden, die eine zusätzliche Zeichenfolge-Argument, das Angeben des Namens einer Client-Side-Funktion, die im Falle eines Fehlers ausgeführt werden.

`null`

Eine Zeichenfolge, die eine clientseitige Funktion, die vor dem Rückruf an den Server initiiert wurde. In diesem Fall besteht keine solchen Skript, damit das Argument null ist.

`true`

Ein boolescher Wert, der angibt, ob den Rückruf asynchron durchführen.

Der Aufruf von WebForm\_DoCallback auf dem Client diese Argumente übergeben werden. Aus diesem Grund, wenn auf dieser Seite auf dem Client gerendert wird, diesen Code sieht wie folgt:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Beachten Sie, dass die Signatur der Funktion auf dem Client etwas anders ist. Die clientseitige Funktion übergibt 5 Zeichenfolgen und einen booleschen Wert. Die zusätzliche Zeichenfolge (die im obigen Beispiel null ist) enthält die Client-Side-Funktion, die eventuelle Fehler bei der serverseitigen Rückruf verarbeitet.

## <a name="step-3--hook-the-client-side-control-event"></a>Schritt 3: Verknüpfen Sie, das clientseitige Steuerelement-Ereignis

Beachten Sie, dass der Rückgabewert der oben genannten GetCallbackEventReference an eine Zeichenfolgenvariable zugewiesen wurde. Diese Zeichenfolge wird verwendet, um ein clientseitiges Ereignis für das Steuerelement zu verknüpfen, die den Rückruf initiiert. In diesem Beispiel wird der Rückruf von einer Dropdownliste auf der Seite initiiert, damit verknüpft werden soll die *OnChange* Ereignis.

Das Client-Side-Ereignis zu verknüpfen, fügen Sie einfach einen Handler auf die clientseitigen Markups wie folgt:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Bedenken Sie, dass *CbRef* ist der Rückgabewert des Aufrufs von GetCallbackEventReference. Es enthält den Aufruf von WebForm\_DoCallback, die oben gezeigt wurde.

## <a name="step-4--register-the-client-side-script"></a>Schritt 4: Registrieren Sie die Skriptdatei auf Clientseite

Wie bereits erwähnt, die der Aufruf von GetCallbackEventReference angegeben, dass ein clientseitiges Skript aufgerufen **ShowCompanyName** würde ausgeführt werden, wenn der Rückruf serverseitige erfolgreich abgeschlossen wurde. Dieses Skript muss auf der Seite mit einer Instanz ClientScriptManager hinzugefügt werden. (ClientScriptManager-Klasse wird weiter unten in diesem Modul Dicussed sein.) Sie führen, wie so aus:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Schritt 5: Rufen Sie die Methoden der ICallbackEventHandler-Schnittstelle

Die ICallbackEventHandler enthält zwei Methoden, die Sie in Ihrem Code implementieren müssen. Sie sind **RaiseCallbackEvent** und **GetCallbackEvent**.

**RaiseCallbackEvent** akzeptiert eine Zeichenfolge als Argument und gibt nichts zurück. Das Zeichenfolgenargument übergeben wird, durch den Aufruf einer clientseitigen WebForm\_DoCallback. In diesem Fall wird der Wert der *Wert* Attribut der Dropdownliste DdlCompany aufgerufen. Serverseitigen Code sollte in der RaiseCallbackEvent-Methode. Z. B. sollte des Rückrufs WebRequest für eine externe Ressource stammt, der Code in RaiseCallbackEvent platziert werden.

**GetCallbackEvent** ist verantwortlich für die Rückgabe des Rückrufs an den Client verarbeiten. Er akzeptiert keine Argumente und gibt eine Zeichenfolge zurück. Die Zeichenfolge, die zurückgegeben wird übergeben werden als Argument an die Funktion die clientseitige in diesem Fall *ShowCompanyName*.

Wenn Sie die oben genannten Schritte abgeschlossen haben, können Sie einen Skript Rückruf in ASP.NET 2.0 ausführen.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/callback1.wmv)


Skript-Rückrufe in ASP.NET sind in beliebigen Browsern unterstützt, die dadurch wird das XMLHttp-Aufrufe unterstützt. Die umfasst alle modernen Browsern verwendet heute. Internet Explorer verwendet das XMLHttp ActiveX-Objekt, während andere moderne Browser (einschließlich der bevorstehenden Internet Explorer 7) ein systeminternes XMLHttp-Objekt verwenden. Um programmgesteuert zu ermitteln, wenn ein Browser Rückrufe unterstützt, können Sie die **Request.Browser.SupportCallback** Eigenschaft. Diese Eigenschaft zurück **"true"** , wenn der anfordernde Client Skriptrückrufe unterstützt.

## <a name="working-with-client-script-in-aspnet-20"></a>Arbeiten mit Clientskripts in ASP.NET 2.0

Clientskripts in ASP.NET 2.0 werden über die Verwendung der Klasse ClientScriptManager verwaltet. Die Klasse ClientScriptManager der nachverfolgt Clientskripts mit einem Typ und einen Namen. Dadurch wird verhindert, dass das gleiche Skript programmgesteuert mehr als einmal auf einer Seite einfügen.

> [!NOTE]
> Nachdem Sie ein Skript auf einer Seite erfolgreich registriert wurde, führt jeder nachfolgende Versuch, registrieren Sie das gleiche Skript einfach in das Skript kein zweites Mal registriert wird. Keine doppelten Skripts hinzugefügt werden und keine Ausnahme auftritt. Um unnötige Berechnung zu vermeiden, gibt es Methoden, die Sie verwenden können, um festzustellen, ob ein Skript bereits registriert ist, damit Sie nicht versuchen, mehr als einmal zu registrieren.


Die Methoden der ClientScriptManager sollten alle aktuellen ASP.NET-Entwickler vertraut sein:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Diese Methode fügt ein Skript an den Anfang der gerenderten Seite. Dies ist hilfreich für das Hinzufügen von Funktionen, die explizit auf dem Client aufgerufen wird.

Es gibt zwei überladene Versionen dieser Methode. Drei der vier Argumente gibt es für sie. Dies sind:

`type (string)`

Die ***Typ*** Argument identifiziert einen Typ für das Skript. Es wird im Allgemeinen empfiehlt sich, verwenden Sie den Seitentyp (diese. GetType()) für den Typ.

`key (string)`

Die ***Schlüssel*** Argument ist ein benutzerdefiniertes Schlüssel für das Skript. Dies sollte für jedes Skript eindeutig sein. Wenn Sie versuchen, ein Skript mit dem gleichen Schlüssel und ein bereits hinzugefügte Skripttyp hinzufügen, wird sie nicht hinzugefügt werden.

`script (string)`

Die ***Skript*** Argument ist eine Zeichenfolge, enthält das eigentliche Skript hinzufügen. Es wird empfohlen, dass Sie einen StringBuilder verwenden, um das Skript zu erstellen und verwenden anschließend die ToString()-Methode StringBuilder Zuweisen der ***Skript*** Argument.

Wenn Sie die überladenen RegisterClientScriptBlock, die nur drei Argumente akzeptiert verwenden, müssen Sie Script-Elemente enthalten (&lt;Skript&gt; und &lt;/script&gt;) in Ihrem Skript.

Möglicherweise möchten die Überladung der RegisterClientScriptBlock verwenden, die ein viertes Argument akzeptiert. Das vierte Argument ist ein boolescher Wert, der angibt, ob in ASP.NET Script-Elemente für Sie hinzugefügt werden soll. Wenn dieses Argument **"true"**, das Skript darf keinen enthalten die Skriptelemente explizit.

Verwenden Sie die IsClientScriptBlockRegistered-Methode, um zu bestimmen, ob ein Skript bereits registriert wurde. Dadurch können Sie vermeiden, dass einen Versuch, ein Skript, die bereits registriert wurde, erneut registrieren.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (neu in 2.0)

Das Tag RegisterClientScriptInclude erstellt einen Skriptblock, das einen Link zu einer externen Skriptdatei. Er verfügt über zwei Überladungen. Eine Überladung einen Schlüssel und einer URL. Der zweite Befehl fügt ein drittes Argument den Typ angibt.

Im folgende Code wird z. B. mit Verknüpfungen zu jsfunctions.js im Stammverzeichnis des Ordner Scripts der Anwendung die einem Skriptblock generiert:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Dieser Code erzeugt den folgenden Code in der gerenderten Seite:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Der Skriptblock wird am unteren Rand der Seite gerendert.


Verwenden Sie die IsClientScriptIncludeRegistered-Methode, um zu bestimmen, ob ein Skript bereits registriert wurde. Dadurch können Sie zur Vermeidung der Versuch, ein Skript erneut registrieren.

## <a name="registerstartupscript"></a>RegisterStartupScript

Die RegisterStartupScript-Methode akzeptiert die gleichen Argumente wie die RegisterClientScriptBlock-Methode. Ein Skript mit RegisterStartupScript registriert ausgeführt wird, nachdem die Seite geladen, aber vor dem OnLoad clientseitige-Ereignis. In 1.X RegisterStartupScript registrierten Skripts eingefügt wurden, unmittelbar vor dem schließenden &lt;/form&gt; während RegisterClientScriptBlock registrierten Skripts, sofort nach dem öffnenden gespeichert wurden tag &lt;Formular&gt; Tag. In ASP.NET 2.0 sind beide unmittelbar vor der schließenden platziert &lt;/form&gt; Tag.

> [!NOTE]
> Wenn Sie eine Funktion mit RegisterStartupScript registrieren, wird diese Funktion nicht ausgeführt, bis Sie explizit im clientseitigen Code aufrufen.


Verwenden Sie die IsStartupScriptRegistered-Methode, um zu bestimmen, ob ein Skript bereits registriert wurde, und vermeiden Sie Versuch, ein Skript erneut registrieren.

## <a name="other-clientscriptmanager-methods"></a>Andere ClientScriptManager-Methoden

Hier sind einige andere nützlicher Methoden der ClientScriptManager-Klasse.


|  <strong>GetCallbackEventReference</strong>   |                                                 Finden Sie unter Skriptrückrufe weiter oben in diesem Modul aus.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ruft einen JavaScript-Verweis (Javascript:&lt;Aufrufen&gt;), die verwendet werden kann, um wieder von der ein clientseitiges Ereignis gesendet werden.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ruft eine Zeichenfolge, die zum Initiieren einer POST-Anforderung vom Client verwendet werden kann.                                    |
|      <strong>GetWebResourceUrl</strong>       | Gibt eine URL auf eine Ressource, die in einer Assembly eingebettet ist. Muss verwendet werden, zusammen mit <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registriert eine Webressource mit der Seite. Hierbei handelt es sich um Ressourcen in einer Assembly eingebettet und von den neuen "WebResource.axd" Handler behandelt.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registriert ein ausgeblendetes Formularfeld mit der Seite.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registriert die clientseitige Code, der ausgeführt wird, wenn das HTML-Formular gesendet wird.                                   |

