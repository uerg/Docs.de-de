---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Die ASP.NET 2.0-Seitenmodell | Microsoft-Dokumentation
author: microsoft
description: In ASP.NET 1.x, Entwickler hatten eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. CodeBehind kann implementiert werden, verwenden entweder das Src-Attr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379886"
---
<a name="the-aspnet-20-page-model"></a>Die ASP.NET 2.0-Seitenmodell
====================
durch [Microsoft](https://github.com/microsoft)

> In ASP.NET 1.x, Entwickler hatten eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. CodeBehind kann implementiert werden, verwenden entweder das Src-Attribut oder das CodeBehind-Attribut von der @Page Richtlinie. In ASP.NET 2.0 Entwickler haben immer noch die Wahl zwischen Inline-Code und Code-Behind, es gibt aber deutliche Verbesserungen bei der Code-Behind-Modells.


In ASP.NET 1.x, Entwickler hatten eine Wahl zwischen einer Inline-Codemodell und ein Code-Behind-Codemodell. CodeBehind kann implementiert werden, verwenden entweder das Src-Attribut oder das CodeBehind-Attribut von der @Page Richtlinie. In ASP.NET 2.0 Entwickler haben immer noch die Wahl zwischen Inline-Code und Code-Behind, es gibt aber deutliche Verbesserungen bei der Code-Behind-Modells.

## <a name="improvements-in-the-code-behind-model"></a>Verbesserungen bei der Code-Behind-Modells

Um die Änderungen im Code-Behind-Modell in ASP.NET 2.0 vollständig zu verstehen, empfiehlt es sich, schnell zu überprüfen, das Modell wie vorhanden waren, in ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Der Code-Behind-Modells in ASP.NET 1.x

In ASP.NET 1.x, die Code-Behind-Modells umfasste eine ASPX-Datei (die Webform) und eine Code-Behind-Datei, die mit der Programmcode. Die beiden Dateien verbunden waren, mit der @Page -Direktive in der ASPX-Datei. Jedes Steuerelement auf der ASPX-Seite hatte eine entsprechende Deklaration in der CodeBehind-Datei als Instanzvariable. Code-Behind-Datei auch Code für die ereignisbindung enthalten und generiert Code für Visual Studio-Designer erforderlich sind. Dieses Modell funktioniert recht gut, aber da jedes Element von ASP.NET in die ASPX-Seite den entsprechenden Code im Code-Behind-Datei erforderlich, es wurde keine "true" Trennung von Code und Inhalt. Z. B. wenn ein Designer ein neues Serversteuerelement eine ASPX-Datei außerhalb von Visual Studio-IDE hinzugefügt haben, wird die Anwendung die fehlt eine Deklaration für dieses Steuerelement in der CodeBehind-Datei unterbrochen.

## <a name="the-code-behind-model-in-aspnet-20"></a>Der Code-Behind-Modells in ASP.NET 2.0

ASP.NET 2.0 wird bei diesem Modell erheblich verbessert. In ASP.NET 2.0 wird Code-Behind implementiert, mit dem neuen *Teilklassen* in ASP.NET 2.0 bereitgestellt. Der Code-Behind-Klasse in ASP.NET 2.0 ist definiert als eine partielle Klasse, was bedeutet, dass sie nur einen Teil der Klassendefinition enthält. Der verbleibende Teil der Klassendefinition wird dynamisch von ASP.NET 2.0 mithilfe der ASPX-Seite, oder wenn die Website vorkompiliert wird zur Laufzeit generiert. Die Verknüpfung zwischen Code-Behind-Datei und die ASPX-Seite wird weiterhin hergestellt, mit der @ Page-Direktive. Anstatt ein CodeBehind oder Src-Attribut verwendet ASP.NET 2.0 jedoch jetzt das CodeFile-Attribut. Das Inherits-Attribut wird auch verwendet, den Klassennamen für die Seite an.

Eine typische @ Page-Direktive kann wie folgt aussehen:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Eine typische Klassendefinition in einer ASP.NET 2.0-Code-Behind-Datei kann wie folgt aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# und Visual Basic sind nur verwalteten Sprachen, die derzeit von partiellen Klassen. Aus diesem Grund werden verwenden j# Entwickler kann nicht der Code-Behind-Modells in ASP.NET 2.0 verwendet.


Das neue Modell verbessert die Code-Behind-Modells aus, da Entwickler haben nun die Codedateien werden, die nur den Code enthalten, den sie erstellt haben. Darüber hinaus eine "true" Trennung von Code und Inhalt da keine Instanz-Variablendeklarationen im Code-Behind-Datei vorhanden sind.

> [!NOTE]
> Da die partielle Klasse für die ASPX-Seite ist, in denen ereignisbindung stattfindet, können Visual Basic-Entwicklern eine leichte Leistungssteigerung umsetzen können mithilfe der Handles-Schlüsselwort im Code-Behind-um Ereignisse zu binden. C# verfügt über keine entsprechende Schlüsselwort ein.


## <a name="new--page-directive-attributes"></a>Neue Attribute der @ Page-Direktive

ASP.NET 2.0 fügt viele neue Attribute hinzu, der @ Page-Direktive. Die folgenden Attribute sind neu in ASP.NET 2.0.

## <a name="async"></a>Async

Das Async-Attribut können Sie so konfigurieren Sie die Seite asynchron ausgeführt werden. Behandeln Sie auch asynchrone Seiten in diesem Modul.

## <a name="asynctimeout"></a>AsyncTimeout

Das Timeout für asynchrone Seiten wird angegeben. Der Standardwert beträgt 45 Sekunden.

## <a name="codefile"></a>CodeFile

Das CodeFile-Attribut ist der Ersatz für die CodeBehind-Attribut in Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Das Attribut CodeFileBaseClass ist in Fällen verwendet, in denen mehrere Seiten auf eine einzige Basisklasse abgeleitet werden soll. Aufgrund der Implementierung des partiellen Klassen in ASP.NET, ohne dieses Attribut eine Basisklasse, die gemeinsam genutzten allgemeine Felder verwendet, um die Steuerelemente, die in einer ASPX-Seite deklariert verweisen würde nicht ordnungsgemäß funktioniert, weil ASP. Netze Kompilierung-Engine erstellt automatisch neue Member, die basierend auf der Seite. Aus diesem Grund sollten Sie eine allgemeine Basisklasse für zwei oder mehr Seiten in ASP.NET, Sie benötigen zum Definieren Ihrer Basisklasse im CodeFileBaseClass-Attribut angeben, und klicken Sie dann jede Seiten-Klasse aus dieser Basisklasse abgeleitet. Das CodeFile-Attribut ist auch erforderlich, wenn dieses Attribut verwendet wird.

## <a name="compilationmode"></a>CompilationMode

Dieses Attribut ermöglicht Ihnen, die CompilationMode-Eigenschaft, der die ASPX-Seite festgelegt. Die CompilationMode-Eigenschaft ist eine Enumeration, die mit den Werten **immer**, **automatisch**, und **nie**. Der Standardwert ist **immer**. Die **automatisch** Einstellung verhindert, dass ASP.NET dynamisch nach Möglichkeit Kompilieren der Seite. Ausschließen von Seiten von der dynamischen Kompilierung die Leistung erhöht. Allerdings, wenn eine Seite, die ausgeschlossen ist dieser Code, die kompiliert werden muss enthält, wird ein Fehler ausgelöst werden, wenn die Seite im Browser geöffnet wird.

## <a name="enableeventvalidation"></a>EnableEventValidation

Dieses Attribut gibt an, und zwar unabhängig davon, ob Postback-und Rückrufereignisse überprüft werden. Wenn diese Option aktiviert ist, werden Argumente für das postback oder Rückrufereignisse überprüft, um sicherzustellen, dass sie über das Steuerelement stammt, der ursprünglich gerendert.

## <a name="enabletheming"></a>EnableTheming

Dieses Attribut gibt an, ob ASP.NET-Designs auf einer Seite verwendet werden. Der Standardwert ist **FALSE**. ASP.NET-Designs finden Sie im [Unterrichtseinheit 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Dieses Attribut gibt an, ob während der Kompilierung Zeilenpragmas hinzugefügt werden soll. Zeilenpragmas sind Optionen, die von Debuggern verwendet wird, um bestimmte Abschnitte des Codes zu markieren.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Dieses Attribut gibt an, und zwar unabhängig davon, ob JavaScript auf der Seite eingefügt wird, um die Bildlaufposition zwischen Postbacks zu gewährleisten. Dieses Attribut ist **"false"** standardmäßig.

Wenn dieses Attribut ist **"true"**, ASP.NET fügen eine &lt;Skript&gt; Block beim Postback, die folgendermaßen aussieht:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Beachten Sie, dass für diesen Block Script Src WebResource.axd. Diese Ressource ist kein physischer Pfad. Wenn dieses Skript angefordert wird, erstellt ASP.NET dynamisch das Skript aus.

### <a name="masterpagefile"></a>MasterPageFile

Dieses Attribut gibt an, die Masterseitendatei für die aktuelle Seite. Der Pfad kann relativ oder absolut sein. Masterseiten finden Sie im [Modul 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Dieses Attribut können Sie Benutzeroberflächen-Darstellungseigenschaften, die durch ein ASP.NET 2.0-Design definiert, außer Kraft setzen. Designs finden Sie im [Unterrichtseinheit 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Design

Gibt an, das Design für die Seite. Wenn Sie ein Wert für das StyleSheetTheme-Attribut nicht angegeben ist, überschreibt Theme-Attribut alle Stile für Steuerelemente auf der Seite.

## <a name="title"></a>Titel

Legt den Titel für die Seite fest. Der hier angegebene Wert wird angezeigt, der &lt;Titel&gt; Element der gerenderten Seite.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Legt den Wert für die ViewStateEncryptionMode-Enumeration. Die verfügbaren Werte sind **immer**, **automatisch**, und **nie**. Der Standardwert ist **automatisch**. Wenn dieses Attribut auf einen Wert festgelegt ist **automatisch**, Viewstate wird verschlüsselt ist, ein Steuerelement anfordert es durch Aufrufen der **RegisterRequiresViewStateEncryption** Methode.

## <a name="setting-public-property-values-via-the--page-directive"></a>Festlegen von Werten für öffentliche Eigenschaft über die @ Page-Direktive

Eine weitere neue Funktion der @ Page-Direktive in ASP.NET 2.0 ist die Möglichkeit, den Anfangswert der öffentlichen Eigenschaften der Basisklasse festzulegen. Angenommen beispielsweise, dass man eine öffentliche Eigenschaft namens **Irgendein_text** in Ihrer Basisklasse und d, wie es Sie initialisiert werden, um **Hello** beim Laden einer Seite. Sie erreichen dies, indem einfach der Wert in der @ Page-Direktive wie folgt:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

Die **Irgendein_text** Attribut der @ Page-Direktive wird der Anfangswert der Irgendein_text-Eigenschaft in der Basisklasse zu *Hello!*. Im folgenden Video wird eine exemplarische Vorgehensweise für das Festlegen des Anfangswert einer öffentlichen Eigenschaft in einer Basisklasse, die mit der @ Page-Direktive.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Neue öffentliche Eigenschaften der Page-Klasse

Die folgenden öffentlichen Eigenschaften sind neu in ASP.NET 2.0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Gibt den webanwendungsbezogenen Pfad zu der Seite oder das Steuerelement zurück. Z. B. für eine Seite, die am http://app/folder/page.aspx, gibt die Eigenschaft ~ / Folder /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Gibt die relative virtuelle Verzeichnispfad zu der Seite oder das Steuerelement zurück. Beispiel für eine Seite, die am http://app/folder/page.aspx, gibt die Eigenschaft ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Übernimmt oder bestimmt den Timeout für das asynchrone Seite behandeln. (Asynchrone Seiten werden weiter unten in diesem Modul behandelt werden.)

## <a name="clientquerystring"></a>ClientQueryString

Eine schreibgeschützte Eigenschaft, die den Abfragezeichenfolgenabschnitt der angeforderten URL zurückgibt. Dieser Wert ist die URL-codiert. Sie können die UrlDecode-Methode, der HttpServerUtility-Klasse verwenden, dieser zunächst decodiert.

## <a name="clientscript"></a>ClientScript

Diese Eigenschaft gibt ein ClientScriptManager-Objekt, das zum Verwalten von ASP.NETs Emissionen von Client-seitige Skript verwendet werden kann. (Die ClientScriptManager-Klasse wird später in diesem Modul behandelt.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Diese Eigenschaft steuert, ob die ereignisvalidierung für Postback-und Rückrufereignisse aktiviert ist. Wenn aktiviert, werden Argumente für das postback oder Rückrufereignisse überprüft, um sicherzustellen, dass sie über das Steuerelement stammt, der ursprünglich gerendert.

## <a name="enabletheming"></a>EnableTheming

Diese Eigenschaft ruft ab oder legt einen booleschen Wert, der angibt, und zwar unabhängig davon, ob ein Design von ASP.NET 2.0 auf der Seite angewendet wird.

## <a name="form"></a>Formular

Diese Eigenschaft gibt das HTML-Formular auf der ASPX-Seite als ein HtmlForm-Objekt zurück.

## <a name="header"></a>Header

Diese Eigenschaft gibt einen Verweis auf ein HtmlHead-Objekt, das den Seitenkopf enthält. Sie können das zurückgegebene HtmlHead-Objekt verwenden, um abzurufen und festzulegen, Stylesheets, Meta-Tags usw.

## <a name="idseparator"></a>IdSeparator

Diese schreibgeschützte Eigenschaft ruft das Zeichen, das verwendet wird, um Steuerelement-IDs zu trennen, wenn ASP.NET eine eindeutige ID für die Steuerelemente auf einer Seite erstellt. Sie sollte nicht direkt im Code verwendet werden.

## <a name="isasync"></a>IsAsync

Diese Eigenschaft ermöglicht für asynchrone Seiten. Asynchrone Seiten werden weiter unten in diesem Modul erläutert.

## <a name="iscallback"></a>IsCallback

Gibt diese schreibgeschützte Eigenschaft **"true"** die Seite ist das Ergebnis eines Aufrufs zurück. Rückfragen werden weiter unten in diesem Modul erläutert.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Gibt diese schreibgeschützte Eigenschaft **"true"** Wenn die Seite Bestandteil eines seitenübergreifenden Postbacks ist. Seitenübergreifende Postbacks werden weiter unten in diesem Modul behandelt.

## <a name="items"></a>Elemente

Gibt einen Verweis auf ein IDictionary-Instanz, die alle Objekte in der Seiten-Kontext enthält. Sie können Elemente hinzufügen, um dieses IDictionary-Objekt, und sie werden während der gesamten Lebensdauer des Kontexts zur Verfügung.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Diese Eigenschaft steuert, ob ASP.NET JavaScript ausgibt, die verwaltet, dass die Seiten Bildlaufposition, die im Browser nach einem Postback. (Details zu dieser Eigenschaft wurden weiter oben in diesem Modul erläutert.)

## <a name="master"></a>Master

Diese schreibgeschützte Eigenschaft gibt einen Verweis auf die MasterPage-Instanz für eine Seite, auf der eine Masterseite angewendet wurde.

## <a name="masterpagefile"></a>MasterPageFile

Ruft ab oder legt den Dateinamen der Masterseite für die Seite fest. Diese Eigenschaft kann nur in der PreInit-Methode festgelegt werden.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Diese Eigenschaft ruft ab oder legt die maximale Länge für den Status der Seiten in Bytes fest. Wenn die Eigenschaft auf eine positive Zahl festgelegt ist, wird der Ansichtszustand für Seiten in mehrere ausgeblendete Felder unterteilt werden, damit er die angegebene Anzahl von Bytes nicht überschreitet. Wenn die Eigenschaft eine negative Zahl ist, wird der Ansichtszustand nicht in Blöcke unterteilt werden.

## <a name="pageadapter"></a>PageAdapter

Gibt einen Verweis auf die PageAdapter-Objekt, das die Seite für den anfordernden Browser ändert.

## <a name="previouspage"></a>PreviousPage

Gibt einen Verweis zur vorherigen Seite im Fall von "Server.Transfer" oder eines seitenübergreifenden Postbacks zurück.

## <a name="skinid"></a>SkinID

Gibt an, die ASP.NET 2.0-Designs, das auf der Seite angewendet.

## <a name="stylesheettheme"></a>StyleSheetTheme

Diese Eigenschaft ruft ab oder legt das Stylesheet, das auf eine Seite angewendet wird.

## <a name="templatecontrol"></a>TemplateControl

Gibt einen Verweis auf das enthaltende Steuerelement für die Seite zurück.

## <a name="theme"></a>Design

Übernimmt oder bestimmt den Namen des Designs ASP.NET 2.0 auf der Seite angewendet. Dieser Wert muss vor der PreInit-Methode festgelegt werden.

## <a name="title"></a>Titel

Diese Eigenschaft ruft ab oder legt den Titel aus, für die Seite fest, wie aus dem Header Seiten abgerufen.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Übernimmt oder bestimmt den ViewStateEncryptionMode der Seite. Finden Sie ausführliche Informationen zu dieser Eigenschaft weiter oben in diesem Modul aus.

## <a name="new-protected-properties-of-the-page-class"></a>Neue geschützte Eigenschaften der Page-Klasse

Im folgenden sind die neuen geschützten Eigenschaften der Seitenklasse in ASP.NET 2.0.

## <a name="adapter"></a>Adapter

Gibt zurück, der ein Verweis auf die ControlAdapter, die der Seite, auf dem Gerät, rendert, die sie angefordert hat.

## <a name="asyncmode"></a>AsyncMode

Diese Eigenschaft gibt an, und zwar unabhängig davon, ob die Seite asynchron verarbeitet wird. Es ist für die Verwendung von der Laufzeit und nicht direkt im Code vorgesehen.

## <a name="clientidseparator"></a>ClientIDSeparator

Diese Eigenschaft gibt das Zeichen als Trennzeichen verwendet werden, wenn Sie eindeutige IDs für Steuerelemente erstellen. Es ist für die Verwendung von der Laufzeit und nicht direkt im Code vorgesehen.

## <a name="pagestatepersister"></a>PageStatePersister

Diese Eigenschaft gibt das PageStatePersister für die Seite zurück. Diese Eigenschaft wird hauptsächlich von Entwicklern von ASP.NET verwendet.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Diese Eigenschaft gibt eine eindeutige Suffic, das den Dateipfad für zwischenspeichernde Browser angefügt wird. Der Standardwert ist \_ \_Ufps = und eine Reihe von 6 Ziffern.

## <a name="new-public-methods-for-the-page-class"></a>Neue öffentliche Methoden für das Page-Klasse

Die folgenden öffentlichen Methoden sind neu in der Seitenklasse in ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Diese Methode registriert Ereignishandlerdelegaten für die Ausführung der asynchronen Seite. Asynchrone Seiten werden weiter unten in diesem Modul erläutert.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Die Eigenschaften in einem Stylesheet Seiten gültig für die Seite.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Diese Methode ab einer asynchronen Aufgabe.

### <a name="getvalidators"></a>GetValidators

Gibt eine Auflistung von Validierungssteuerelementen, für die angegebene Validierungsgruppe oder die Standardvalidierungsgruppe zurück, wenn keine Angabe erfolgt.

## <a name="registerasynctask"></a>RegisterAsyncTask

Diese Methode registriert eine neue asynchrone Aufgabe. Asynchrone Seiten werden weiter unten in diesem Modul behandelt.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Diese Methode weist ASP.NET, dass die Seiten-Steuerelementzustand beibehalten werden muss.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Diese Methode weist ASP.NET, dass die Seiten Viewstate Verschlüsselung erforderlich ist.

## <a name="resolveclienturl"></a>ResolveClientUrl

Gibt eine relative URL, die für Clientanforderungen für Images usw. verwendet werden kann.

## <a name="setfocus"></a>SetFocus

Diese Methode wird den Fokus auf das Steuerelement festlegen, die angegeben wird, wenn die Seite anfänglich geladen wird.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Diese Methode wird das Steuerelement, das zuvor übergeben wird, nicht mehr eine erforderliche Statuspersistenz Steuerelement, die Registrierung aufheben.

## <a name="changes-to-the-page-lifecycle"></a>Änderungen an der Lebenszyklus der Seite

Der Lebenszyklus der Seite in ASP.NET 2.0 noch nicht erheblich geändert, aber es gibt einige neuen Methoden, die Sie kennen sollten. Der Lebenszyklus der ASP.NET 2.0-Seite wird im folgenden erläutert.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (neu in ASP.NET 2.0)

Das PreInit-Ereignis ist der frühesten Stufe des Lebenszyklus, die ein Entwickler zugreifen können. Das Hinzufügen dieses Ereignisses ermöglicht das programmgesteuerte Ändern von ASP.NET 2.0-Designs, Masterseiten, den Zugriff auf Eigenschaften für eine ASP.NET 2.0-Profil usw. Wenn Sie befinden sich in einem postback Zustand wichtig zu wissen, dass Viewstate noch nicht für Steuerelemente, die an diesem Punkt im Lebenszyklus angewendet wurde. Aus diesem Grund, wenn ein Entwickler eine Eigenschaft eines Steuerelements in dieser Phase geändert wird, werden sie wahrscheinlich später im Lebenszyklus von Seiten überschrieben.

## <a name="init"></a>Init

Die Init-Ereignis wurde nicht geändert, von ASP.NET 1.x. Dies ist, in denen Sie möchten lesen oder Initialisieren der Eigenschaften von Steuerelementen auf der Seite. Auf dieser Stufe, Masterseiten, Designs usw. sind bereits auf die Seite angewendet.

## <a name="initcomplete-new-in-20"></a>InitComplete (neu in 2.0)

Die InitComplete-Ereignis wird am Ende der Initialisierungsphase Seiten aufgerufen. An diesem Punkt im Lebenszyklus, Sie können auf Steuerelemente auf der Seite, aber deren Status wurde noch nicht aufgefüllt.

## <a name="preload-new-in-20"></a>Vorab zu laden (neu in 2.0)

Dieses Ereignis wird immer dann aufgerufen, nachdem alle Daten angewendet wurde, und unmittelbar vor Seite\_laden.

## <a name="load"></a>Load

Die Load-Ereignis wurde nicht geändert, von ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (neu in 2.0)

Das Ereignis LoadComplete ist das letzte Ereignis in der Ladephase der Seiten an. Zu diesem Zeitpunkt alle Postback und Viewstate-Daten angewendet wurde auf der Seite.

## <a name="prerender"></a>PreRender

Wenn Sie möchten für den Ansichtszustand für Steuerelemente ordnungsgemäß verwaltet werden, die dynamisch auf der Seite hinzugefügt werden, ist das PreRender-Ereignis die letzte Gelegenheit, die sie hinzufügen.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (neu in 2.0)

In der Phase PreRenderComplete wurden alle Steuerelemente auf der Seite hinzugefügt, und die Seite gerendert werden kann. Das PreRenderComplete-Ereignis ist das letzte Ereignis wird ausgelöst, bevor die Seiten-Ansichtszustand gespeichert wird.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (neu in 2.0)

Die SaveStateComplete-Ereignis wird immer dann aufgerufen, unmittelbar, nachdem alle Seitenstatus Viewstate und Steuerelement gespeichert wurde. Dies ist das letzte Ereignis, bevor die Seite tatsächlich an den Browser gerendert wird.

## <a name="render"></a>Rendern

Die Render-Methode wurde nicht geändert, seit ASP.NET 1.x. Dies ist, in denen die HtmlTextWriter initialisiert wird und die Seite im Browser gerendert wird.

## <a name="cross-page-postback-in-aspnet-20"></a>Seitenübergreifenden Postback in ASP.NET 2.0

In ASP.NET 1.x, Postbacks mussten auf derselben Seite senden. Seitenübergreifende Postbacks wurden nicht zulässig. ASP.NET 2.0 fügt die Fähigkeit zum Zurücksenden an eine andere Seite über die IButtonControl-Schnittstelle hinzu. Jedes Steuerelement, das die neue IButtonControl-Schnittstelle (Schaltfläche LinkButton und ImageButton zusätzlich zu benutzerdefinierten Steuerelementen von Drittanbietern) implementiert kann diese neue Funktionalität über die Verwendung des Attributs PostBackUrl nutzen. Der folgende Code zeigt ein Schaltflächen-Steuerelement, das an einer zweiten Seite zurückgesendet.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Wenn die Seite zurückgesendet wird, ist die Seite, die das Postback initiiert über die PreviousPage-Eigenschaft auf der zweiten Seite zugegriffen werden kann. Diese Funktionalität wird über die neue WebForm implementiert\_DoPostBackWithOptions clientseitige Funktion, die ASP.NET 2.0 auf der Seite gerendert wird, wenn ein Steuerelement an eine andere Seite zurückgesendet. Die JavaScript-Funktion wird von der neuen WebResource.axd-Handler bereitgestellt, das Skript an den Client ausgibt.

Im folgenden Video wird eine exemplarische Vorgehensweise eines seitenübergreifenden Postbacks.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Weitere Informationen zu seitenübergreifende Postbacks

### <a name="viewstate"></a>ViewState

Möglicherweise haben sich bereits darüber, was von der ersten Seite in einem Szenario mit Cross-Page Postbacks im Ansichtszustand geschieht. Schließlich beibehalten jedes Steuerelement, das nicht IPostBackDataHandler implementiert seinen Status über den Ansichtszustand, also Zugriff auf die Eigenschaften des Steuerelements auf der zweiten Seite eines seitenübergreifenden Postbacks, benötigen Sie Zugriff auf den Ansichtszustand für die Seite. ASP.NET 2.0 übernimmt die dieses Szenario verwenden ein neues ausgeblendetes Feld auf der zweiten Seite namens \_ \_PREVIOUSPAGE. Die \_ \_PREVIOUSPAGE-Formularfeld enthält den Ansichtszustand für die erste Seite, damit Sie auf die Eigenschaften aller Steuerelemente in der zweiten Seite zugreifen können.

### <a name="circumventing-findcontrol"></a>FindControl umgehen

In den video zur exemplarischen Vorgehensweise eines seitenübergreifenden Postbacks habe ich die FindControl-Methode einen Verweis auf das TextBox-Steuerelement auf der ersten Seite abzurufen. Diese Methode eignet sich gut für diesen Zweck aber FindControl ist teuer, und erfordert das Schreiben von zusätzlichem Code. Zum Glück bietet ASP.NET 2.0 eine Alternative zum FindControl für diesen Zweck, die in vielen Szenarien funktionieren. PreviousPageType-Direktive können Sie einen stark typisierten Verweis zur vorherigen Seite mit den TypeName oder VirtualPath-Attribut aufweisen. Die TypeName-Attribut können Sie den Typ der vorherigen Seite angeben, während das VirtualPath-Attribut auf der vorherigen Seite, die mit einem virtuellen Pfad verweisen kann. Nach dem Festlegen der PreviousPageType-Direktive müssen Sie verfügbar machen die Steuerelemente usw., um den Zugriff über öffentliche Eigenschaften ermöglichen werden soll.

## <a name="lab-1-cross-page-postback"></a>Übungseinheit 1 seitenübergreifenden Postbacks

In dieser Übungseinheit erstellen Sie eine Anwendung, die die neue seitenübergreifenden postback-Funktionalität von ASP.NET 2.0 verwendet.

1. Öffnen Sie Visual Studio 2005 und erstellen Sie eine neue ASP.NET-Website.
2. Fügen Sie eine neue Webform page2.aspx aufgerufen.
3. Öffnen Sie die "default.aspx" in der Entwurfsansicht, und fügen Sie ein Schaltflächen-Steuerelement und ein TextBox-Steuerelement hinzu. 

    1. Dem Schaltflächen-Steuerelement-ID geben **SubmitButton** und das Textfeld steuern ID **Benutzername**.
    2. Legen Sie die PostBackUrl-Eigenschaft der Schaltfläche, um page2.aspx.
4. Öffnen Sie in der Quellansicht page2.aspx.
5. Fügen Sie eine @ PreviousPageType-Direktive hinzu, wie unten dargestellt:
6. Fügen Sie den folgenden Code auf der Seite\_Page2.aspx des Code-Behind-Auslastung: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Erstellen Sie das Projekt, indem Sie die bei der Erstellung im Menü erstellen auf.
8. Fügen Sie den folgenden Code, um den Code-Behind für "default.aspx": 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Ändern Sie die Seite\_Last in page2.aspx folgt: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Erstellen Sie das Projekt.
11. Führen Sie das Projekt aus.
12. Geben Sie Ihren Namen in das Textfeld ein, und klicken Sie auf die Schaltfläche.
13. Was ist das Ergebnis?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchrone Seiten in ASP.NET 2.0

Viele Konflikte in ASP.NET werden durch Latenz von externen Aufrufe (z. B. Web-Dienst oder die Datenbank-Aufrufe), Datei-e/a-Latenz usw. verursacht. Wenn eine Anforderung für eine ASP.NET-Anwendung erfolgt, verwendet ASP.NET eine von den Arbeitsthreads, um diese Anforderung zu erfüllen. Diese Anforderung besitzt dieser Thread, bis die Anforderung abgeschlossen ist und die Antwort gesendet wurde. ASP.NET 2.0 versucht, die hohe Wartezeiten bei der Verwendung dieser Probleme zu beheben, durch Hinzufügen der Fähigkeit auf Seiten asynchron ausgeführt werden soll. Das bedeutet, dass ein Arbeitsthread kann die Anforderung zum Starten und dann zusätzliche Ausführung zu einem anderen Thread leiten, so schnell an den Pool verfügbaren Thread zurückgibt. Wenn die Datei-e/a, Datenbankaufruf usw. abgeschlossen wurde, wird ein neuer Thread vom Threadpool zum Beenden der Anforderung abgerufen.

Der erste Schritt darin, eine Seite, die asynchron ausgeführt wird, legen Sie die **Async** -Attribut der Seitendirektive wie folgt:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Dieses Attribut teilt ASP.NET zum Implementieren der IHttpAsyncHandler für die Seite.

Im nächste Schritt wird die AddOnPreRenderCompleteAsync-Methode zu einem Zeitpunkt im Lebenszyklus der Seite vor dem PreRender aufzurufen. (Diese Methode wird in der Regel auf aufgerufen\_laden.) Die AddOnPreRenderCompleteAsync-Methode akzeptiert zwei Parameter an; BeginEventHandler und eine EndEventHandler. Die BeginEventHandler gibt ein IAsyncResult, das dann als Parameter an die EndEventHandler übergeben wird.

Das Video unten ist eine exemplarische Vorgehensweise für einer asynchronen Seitenanforderung.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Eine asynchrone Seite rendert nicht an den Browser bis zum Abschluss der EndEventHandler. Keine Zweifel daran bestehen, die aber werden einige Entwickler asynchrone Anforderungen weitgehend für asynchrone Rückrufe vorstellen. Es ist wichtig zu wissen, dass dies nicht der Fall. Der Vorteil für asynchrone Anforderungen ist, dass der erste Thread an den Threadpool neue Anforderungen verarbeiten, wodurch Konflikte aufgrund von e/a-gebunden, usw. zurückgegeben werden kann.


## <a name="script-callbacks-in-aspnet-20"></a>Skriptrückrufe in ASP.NET 2.0

Webentwickler haben immer nach Möglichkeiten, um zu verhindern, dass das Flimmern einen Rückruf zugeordnet haben. Klicken Sie in ASP.NET 1.x SmartNavigation war der am häufigsten verwendete Methode zur Vermeidung von Flimmern, aber SmartNavigation führte zu Problemen für einige Entwickler aufgrund der Komplexität seiner Implementierung auf dem Client. ASP.NET 2.0 löst dieses Problem mit dem Skript-Rückrufe. Skript-Rückrufe verwenden XMLHttp-Anforderungen für den Webserver über JavaScript. Die XMLHttp-Anforderung gibt XML-Daten, die anschließend bearbeitet werden können, über-DOM des Browsers. XMLHttp-Code wird der Benutzer durch den neuen WebResource.axd-Handler ausgeblendet.

Es gibt mehrere Schritte erforderlich, um einen Rückruf für die Skripts in ASP.NET 2.0 zu konfigurieren.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Schritt 1: Implementieren der ICallbackEventHandler-Schnittstelle

Damit für ASP.NET Ihre Seite erkennt, als in einem Skript Rückruf teilnehmen kann müssen Sie die ICallbackEventHandler-Schnittstelle implementieren. Hierzu können Sie in der CodeBehind-Datei wie folgt:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Sie können auch dazu @ Implements-Direktive-ähnliches also:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Sie würden die @ Implements-Direktive in der Regel verwenden, wenn ASP.NET Inlinecode zu verwenden.

## <a name="step-2--call-getcallbackeventreference"></a>Schritt 2: Rufen Sie GetCallbackEventReference

Wie bereits erwähnt, ist der XMLHttp-Aufruf in der WebResource.axd-Handler gekapselt. Wenn Ihre Seite gerendert wird, fügt ASP.NET einen Aufruf von WebForm\_DoCallback, ein Clientskript, die vom WebResource.axd bereitgestellt wird. Der WebForm\_DoCallback-Funktion ersetzt die \_ \_DoPostBack-Funktion für einen Rückruf. Beachten Sie, dass \_ \_DoPostBack programmgesteuert übermittelt das Formular auf der Seite. In einem Rückrufszenario mit Sie verhindern möchten, ein Postback handeln, also \_ \_DoPostBack ist nicht ausreichend.

> [!NOTE]
> \_\_DoPostBack wird immer noch auf der Seite in einem Clientskriptrückruf gerendert. Es wird jedoch nicht für den Rückruf verwendet.


Die Argumente für die WebForm\_DoCallback clientseitige Funktion werden bereitgestellt, über die serverseitige Funktion GetCallbackEventReference, die normalerweise in der Seite aufgerufen werden, würde\_laden. Ein typischer Aufruf zum GetCallbackEventReference könnte folgendermaßen aussehen:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> Cm ist in diesem Fall eine Instanz von ClientScriptManager. Die ClientScriptManager-Klasse wird später in diesem Modul behandelt werden.


Es gibt mehrere überlastete Versionen der GetCallbackEventReference. In diesem Fall sind die Argumente wie folgt ein:

`this`

Ein Verweis auf das Steuerelement, in denen GetCallbackEventReference aufgerufen wird. In diesem Fall ist es sich um die Seite selbst.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Ein Zeichenfolgenargument, die aus den clientseitigen Code mit dem serverseitigen Ereignis übergeben werden. In diesem Fall wird Im übergibt den Wert, der eine Dropdownliste DdlCompany aufgerufen.

`ShowCompanyName`

Der Name der clientseitige Funktion, die den Rückgabewert (als Zeichenfolge) das serverseitige Rückrufereignis angenommen werden sollen. Diese Funktion wird nur aufgerufen werden, wenn der Rückruf für die serverseitige erfolgreich ist. Aus diesem Grund wird aus Gründen der Stabilität, es im Allgemeinen empfohlen, die überladene Version von GetCallbackEventReference verwenden, die eine zusätzliche Zeichenfolgenargument angeben des Namens einer clientseitigen Funktion im Falle eines Fehlers ausgeführt.

`null`

Eine Zeichenfolge, die eine clientseitige-Funktion, die sie vor dem Rückruf an den Server initiiert. In diesem Fall besteht keine solche Skripts, sodass das Argument null ist.

`true`

Ein boolescher Wert, der angibt, ob den Rückruf asynchron durchzuführen.

Der Aufruf von WebForm\_DoCallback auf dem Client werden diese Argumente übergeben. Aus diesem Grund, wenn diese Seite, auf dem Client gerendert wird, diesen Code sieht wie folgt:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Beachten Sie, dass die Signatur der Funktion auf dem Client etwas anders ist. Die Client-Side-Funktion übergibt 5 Zeichenfolgen und einen booleschen Wert. Die zusätzliche Zeichenfolge an (die im obigen Beispiel null ist) enthält die Client-Side-Funktion, die über den Rückruf serverseitigen Fehler behandelt.

## <a name="step-3--hook-the-client-side-control-event"></a>Schritt 3: Verknüpfen Sie das clientseitige Steuerelement-Ereignis

Beachten Sie, dass der Rückgabewert der oben genannten GetCallbackEventReference eine String-Variable zugewiesen wurde. Diese Zeichenfolge wird verwendet, um einen clientseitigen Ereignis für das Steuerelement zu verknüpfen, die den Rückruf initiiert. In diesem Beispiel wird der Rückruf von einer Dropdownliste auf der Seite initiiert, möchte ich Verknüpfen der *OnChange* Ereignis.

Das clientseitige Ereignis verknüpfen, fügen Sie einfach einen Handler an das Client-Side-Markup wie folgt:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Zur Erinnerung: *CbRef* ist der Rückgabewert aus dem Aufruf von GetCallbackEventReference. Sie enthält den Aufruf von WebForm\_DoCallback, der weiter oben gezeigt wurde.

## <a name="step-4--register-the-client-side-script"></a>Schritt 4: Registrieren Sie das clientseitige Skript

Beachten Sie, die der Aufruf von GetCallbackEventReference angegeben, dass eine Client-seitige Skript aufgerufen **ShowCompanyName** würde ausgeführt werden, wenn der Rückruf für die serverseitige erfolgreich abgeschlossen wurde. Dieses Skript muss auf der Seite mit einer ClientScriptManager-Instanz hinzugefügt werden. (Die ClientScriptManager-Klasse werden Dicussed weiter unten in diesem Modul.) So führen Sie, wie:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Schritt 5: Rufen Sie die Methoden der ICallbackEventHandler-Schnittstelle

Die ICallbackEventHandler enthält zwei Methoden, die Sie in Ihrem Code implementieren müssen. Sie sind **RaiseCallbackEvent** und **GetCallbackEvent**.

**RaiseCallbackEvent** nimmt eine Zeichenfolge als Argument und gibt nichts zurück. Das Zeichenfolgenargument übergeben wird, durch den Aufruf der clientseitigen WebForm\_DoCallback. Dieser Wert in diesem Fall ist die *Wert* Attribut im Dropdownmenü DdlCompany aufgerufen. Der serverseitige Code sollte in der RaiseCallbackEvent-Methode platziert werden. Z. B. sollte der Rückruf ein WebRequest auf eine externe Ressource stammt, dieser Code in RaiseCallbackEvent platziert werden.

**GetCallbackEvent** ist verantwortlich für die Verarbeitung des Rückrufs an den Client zurückgegeben. Es akzeptiert keine Argumente und gibt eine Zeichenfolge zurück. Die Zeichenfolge, die zurückgegeben wird übergeben werden als Argument an die Client-Side-Funktion, in diesem Fall *ShowCompanyName*.

Nachdem Sie die oben genannten Schritte abgeschlossen haben, können Sie einen Rückruf an Skript in ASP.NET 2.0 ausführen.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Open Vollbild-Video](the-asp-net-2-0-page-model/_static/callback1.wmv)


Skriptrückrufe in ASP.NET sind in allen Browsern unterstützt, die Sie dabei XMLHttp-Aufrufe unterstützt. Die allen modernen Browsern verwendet den heutigen Tag einschließt. Internet Explorer verwendet das XMLHttp ActiveX-Objekt an, während andere moderne Browser (einschließlich der anstehenden IE 7) ein systeminternes XMLHttp-Objekt verwenden. Um programmgesteuert zu ermitteln, wenn ein Browser Rückrufe unterstützt, können Sie mithilfe der **Request.Browser.SupportCallback** Eigenschaft. Diese Eigenschaft gibt **"true"** , wenn der anfordernde Client Skriptrückrufe unterstützt.

## <a name="working-with-client-script-in-aspnet-20"></a>Arbeiten mit Clientskripts in ASP.NET 2.0

Clientskripts in ASP.NET 2.0 werden über die Verwendung der ClientScriptManager-Klasse verwaltet. Die ClientScriptManager-Klasse verfolgt des Clientskripts, die mit einem Typ und einen Namen. Dadurch wird verhindert, dass das gleiche Skript programmgesteuert mehr als einmal auf einer Seite einfügen.

> [!NOTE]
> Nachdem ein Skript auf einer Seite erfolgreich registriert wurde, führt ein anschließenden Versuch, registrieren Sie das gleiche Skript einfach in das Skript kein zweites Mal registriert wird. Keine doppelten Skripts hinzugefügt werden, und keine Ausnahme auftritt. Um unnötige Berechnung zu vermeiden, gibt es Methoden, die Sie verwenden können, um festzustellen, ob ein Skript bereits registriert ist, sodass Sie nicht versuchen, mehr als einmal registrieren.


Die Methoden des ClientScriptManager sollten alle aktuellen ASP.NET-Entwickler vertraut sein:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Diese Methode fügt ein Skript an den Anfang der gerenderten Seite. Dies ist nützlich für das Hinzufügen von Funktionen, die explizit auf dem Client aufgerufen wird.

Es gibt zwei überladene Versionen dieser Methode ein. Drei der vier Argumente sind häufig zwischen ihnen. Dies sind:

`type (string)`

Die ***Typ*** Argument identifiziert einen Typ für das Skript. Es ist im Allgemeinen eine gute Idee, verwenden Sie den Typ der Seite (diese. GetType()) für den Typ.

`key (string)`

Die ***Schlüssel*** Argument ist einem benutzerdefinierten Schlüssel für das Skript. Dies sollte für jedes Skript eindeutig sein. Wenn Sie versuchen, ein Skript mit dem gleichen Schlüssel und Typ eines bereits hinzugefügte Skripts hinzufügen, wird er nicht hinzugefügt werden.

`script (string)`

Die ***Skript*** Argument ist eine Zeichenfolge, enthält das eigentliche Skript hinzufügen. Es wird empfohlen, dass Sie eine "StringBuilder" verwenden, erstellen Sie das Skript aus, und klicken Sie dann mithilfe der ToString()-Methode auf das "StringBuilder" Zuweisen der ***Skript*** Argument.

Wenn Sie die überladenen RegisterClientScriptBlock, die nur drei Argumente akzeptiert verwenden, müssen Sie Skriptelemente einschließen (&lt;Skript&gt; und &lt;/script&gt;) in Ihrem Skript.

Sie können auswählen, die Überladung von RegisterClientScriptBlock zu verwenden, die ein viertes Argument akzeptiert. Das vierte Argument ist ein boolescher Wert, der angibt, und zwar unabhängig davon, ob ASP.NET Script-Elemente für Sie hinzugefügt werden soll. Wenn dieses Argument **"true"**, das Skript darf nicht enthalten die Skriptelemente explizit.

Verwenden Sie die IsClientScriptBlockRegistered-Methode, um festzustellen, ob ein Skript bereits registriert wurde. Dadurch können Sie vermeiden, Versuch, ein Skript, die bereits registriert wurde, erneut registrieren.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (neu in 2.0)

Das Tag RegisterClientScriptInclude erstellt einen Skriptblock mit einem Link zu einer externen Skriptdatei an. Es verfügt über zwei Überladungen. Eine verwendet einen Schlüssel und eine URL. Der zweite Befehl fügt ein drittes Argument, das den Typ angibt.

Im folgende Code werden z. B. mit einem Link zu jsfunctions.js im Stammverzeichnis der Ordner "Scripts" der Anwendung einem Skriptblock wird generiert:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Dieser Code erzeugt den folgenden Code in der gerenderten Seite:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Der Skriptblock wird am unteren Rand der Seite gerendert.


Verwenden Sie die IsClientScriptIncludeRegistered-Methode, um festzustellen, ob ein Skript bereits registriert wurde. Dadurch können Sie einen Versuch zum erneuten Registrieren eines Skripts zu vermeiden.

## <a name="registerstartupscript"></a>B. RegisterStartupScript

Die RegisterStartupScript-Methode akzeptiert die gleichen Argumente wie die RegisterClientScriptBlock-Methode. Ein Skript, das registriert RegisterStartupScript ausgeführt wird, nachdem die Seite geladen, aber vor das OnLoad-Ereignis für die clientseitige. In 1.X RegisterStartupScript registrierten Skripts platziert wurden, direkt vor dem abschließenden &lt;/form&gt; tag während RegisterClientScriptBlock registrierten Skripts unmittelbar nach dem öffnenden platzierten &lt;Formular&gt; Tag. In ASP.NET 2.0 sind beide direkt vor dem Endtag platziert &lt;/form&gt; Tag.

> [!NOTE]
> Wenn Sie eine Funktion mit RegisterStartupScript registrieren, wird diese Funktion nicht ausgeführt, bis Sie explizit im clientseitigen Code aufrufen.


Verwenden Sie die IsStartupScriptRegistered-Methode, um zu bestimmen, ob ein Skript bereits registriert wurde, und vermeiden Versuch, ein Skript erneut zu registrieren.

## <a name="other-clientscriptmanager-methods"></a>Andere Methoden des ClientScriptManager

Hier sind einige andere nützliche Methoden der ClientScriptManager-Klasse.


|  <strong>GetCallbackEventReference</strong>   |                                                 Skript-Rückrufe weiter oben in diesem Modul wird angezeigt.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Ruft einen JavaScript-Verweis (Javascript:&lt;Aufrufen&gt;), die von einem clientseitigen Ereignis veröffentlichen verwendet werden kann.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Ruft eine Zeichenfolge, die zum Initiieren eines Beitrags vom Client verwendet werden kann.                                    |
|      <strong>GetWebResourceUrl</strong>       | Gibt eine URL auf eine Ressource, die in einer Assembly eingebettet ist. Muss verwendet werden, zusammen mit <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registriert eine Webressource mit der Seite. Hierbei handelt es sich um Ressourcen in eine Assembly eingebettet und von der neuen WebResource.axd-Handler behandelt.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registriert ein ausgeblendetes Formularfeld mit der Seite.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registriert clientseitige-Code, der ausgeführt wird, wenn das HTML-Formular übermittelt wird.                                   |

