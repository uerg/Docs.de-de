---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Grundlegendes zu Teilseite mit ASP.NET-AJAX aktualisiert | Microsoft Docs
author: scottcate
description: Die sichtbarste-Funktion von ASP.NET AJAX-Erweiterungen wird eventuell die Möglichkeit, eine Seite teilweise oder inkrementelle Updates ausführen, ohne dass auf diese Weise eines komplettes Postbacks zum t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 91a98bf1c9a71ae84c569f7ae40930422cb652e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Grundlegendes zu Teilseite aktualisiert mit ASP.NET-AJAX
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Möglicherweise ist die sichtbarste-Funktion von ASP.NET AJAX-Erweiterungen die Möglichkeit, eine Seite teilweise oder inkrementelle Updates ausführen, ohne dass auf diese Weise eines komplettes Postbacks zum Server, ohne Änderungen am Code und minimalen Markupänderungen an. Die Vorteile sind umfangreiche – der Status Ihrer Multimedia (z. B. Adobe Flash, oder klicken Sie mit der Windows Media) unverändert, Bandbreitenkosten werden verringert, und der Client treten keine der in Verbindung mit dem ein Postback Flackern.


## <a name="introduction"></a>Einführung

Microsoft Technologie für ASP.NET bietet ein Programmiermodell objektorientierte und ereignisgesteuerte und ihn mit den Vorteilen des kompilierten Codes vereinigt. Die serverseitige Verarbeitungsmodell hat jedoch einige Nachteile, die eine inhärente Eigenschaft der Technologie:

- Seite "-Updates erfordern einen Roundtrip zum Server, der eine Seite-Aktualisierung erforderlich ist.
- Roundtrips werden von Javascript oder andere Client-Side-Technologie (z. B. Adobe Flash) generierten Effekte nicht beibehalten werden.
- Während des Postbacks unterstützen Browser als Microsoft Internet Explorer nicht automatisch die Bildlaufposition wiederherstellen. Und auch in Internet Explorer, besteht noch immer Flimmern wie die Seite aktualisiert wird.
- Postbacks möglicherweise einen hohen Betrag an Bandbreite als umfassen die \_ \_Formularfelds "ViewState" speichern kann anwachsen, insbesondere bei der Arbeit mit Steuerelemente, z. B. die GridView-Steuerelements oder Repeater.
- Es gibt kein einheitliches Modell für den Zugriff auf Webdienste über JavaScript oder andere Client-Side-Technologie.

Geben Sie den Microsoft ASP.NET AJAX-Erweiterungen. Das steht für AJAX **ein** synchrone **J** AvaScript **ein** Nd **X** ML, ist ein integriertes Framework zum Bereitstellen von inkrementellen Seite Updates über plattformübergreifende JavaScript, bestehend aus serverseitigem Code, der das Microsoft AJAX-Framework und eine Skriptkomponente wird aufgerufen, der Microsoft AJAX-Skript-Bibliothek umfasst. Die ASP.NET AJAX-Erweiterungen bieten auch plattformübergreifender Unterstützung für den Zugriff auf ASP.NET-Webdienste über JavaScript.

In diesem Whitepaper überprüft die Funktionalität Teilseite Updates der ASP.NET AJAX-Erweiterungen, die enthält die ScriptManager-Komponente, das UpdatePanel-Steuerelement und das UpdateProgress-Steuerelement, und berücksichtigt Szenarien, in denen sie sollten oder darf nicht sein genutzt.

In diesem Whitepaper basiert auf der Beta 2-Version von Visual Studio 2008 und .NET Framework 3.5, die ASP.NET AJAX-Erweiterungen in die Basisklassenbibliothek integrieren (, bei denen sie zuvor eine Add-on-Komponente, die für ASP.NET 2.0 verfügbar war). In diesem Whitepaper wird davon ausgegangen, dass Sie Visual Studio 2008 und nicht Visual Web Developer Express Edition verwenden; Einige Projektvorlagen, auf die verwiesen wird, sind möglicherweise nicht für Benutzer von Visual Web Developer Express verfügbar.

## <a name="partial-page-updates"></a>Seitenteilaktualisierungen

Möglicherweise ist die sichtbarste-Funktion von ASP.NET AJAX-Erweiterungen die Möglichkeit, eine Seite teilweise oder inkrementelle Updates ausführen, ohne dass auf diese Weise eines komplettes Postbacks zum Server, ohne Änderungen am Code und minimalen Markupänderungen an. Die Vorteile sind umfangreiche - der Status Ihrer Multimedia (z. B. Adobe Flash, oder klicken Sie mit der Windows Media) unverändert, Bandbreitenkosten werden verringert, und der Client treten keine der in Verbindung mit dem ein Postback Flackern.

Die Möglichkeit, Integrieren von Teilrendering von Seiten ist mit minimalen Änderungen in Ihr Projekt in ASP.NET integriert.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Exemplarische Vorgehensweise: Teilweises Rendern in ein vorhandenes Projekt integrieren


1. Erstellen Sie ein neues ASP.NET-Websiteprojekt in Microsoft Visual Studio 2008, navigieren Sie zu <em>Datei</em>  <em>- &gt; neu</em>  <em>- &gt; Website</em> , und wählen im Dialogfeld ASP.NET-Website. Name kann sich auf einen beliebigen Namen, und kann entweder im Dateisystem oder in Internet Information Services (IIS) installieren.
2. Es wird mit der leeren Standardseite mit grundlegenden ASP.NET-Markup angezeigt werden (ein Formular für die serverseitige und ein `@Page` Richtlinie). Löschen Sie eine Bezeichnung namens `Label1` und eine Schaltfläche namens `Button1` auf die Seite in Form-Elements. Sie können deren Texteigenschaften auf einen beliebigen Namen festlegen.
3. Doppelklicken Sie in der Entwurfsansicht auf `Button1` um einen Code-Behind-Ereignishandler zu generieren. Legen Sie innerhalb dieser Ereignishandler `Label1.Text` auf die Schaltfläche geklickt haben! sein.

**Auflisten von 1: Markup für "default.aspx", bevor das Teilrendering aktiviert ist**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Auflisten von 2: Codebehind (gekürzt) in "default.aspx.cs"**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Drücken Sie F5, um Ihre Website zu starten. Visual Studio fordert Sie zum Hinzufügen einer Datei "Web.config", um Debuggen zu aktivieren; Wenn Sie dies tun. Wenn Sie die Schaltfläche klicken, beachten Sie, dass die Seite aktualisiert wird, zum Ändern des Texts in der Bezeichnung und eine kurze Flackern vorhanden ist, wie die Seite neu gezeichnet wird.
2. Nach dem Schließen Ihres Browserfensters, zurück zu Visual Studio, und klicken Sie auf der Seite "Markup". Führen Sie einen Bildlauf nach unten in der Visual Studio-Toolbox, und suchen Sie die Registerkarte mit der Bezeichnung AJAX-Erweiterungen. (Wenn Sie nicht über auf dieser Registerkarte verfügen, da Sie eine ältere Version von AJAX oder Atlas Erweiterungen verwenden, finden Sie in der exemplarischen Vorgehensweise für die Registrierung der AJAX-Erweiterungen Toolboxelemente weiter unten in diesem Whitepaper, oder installieren Sie die aktuelle Version mit dem Windows Installer zum Herunterladen von der Website).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))


1. <em>Bekanntes Problem:</em>bei der Installation von Visual Studio 2008 auf einem Computer, die bereits Visual Studio 2005 mit ASP.NET 2.0 AJAX-Erweiterungen installiert wurde, wird Visual Studio 2008 Toolboxelemente AJAX-Erweiterungen importiert. Sie können bestimmen, ob dies der Fall ist, mithilfe der QuickInfo der Komponenten; Sie sollten Version 3.5.0.0 angenommen. Wenn sie z. Version 2.0.0.0 b. dann Ihre alten Toolboxelemente importiert haben, und müssen manuell mit das Dialogfeld "Toolboxelemente auswählen" in Visual Studio importieren. Sie werden zum Hinzufügen von Version 2-Steuerelementen über den Designer.

2. Bevor Sie die `<asp:Label>` Tag beginnt, erstellen Sie eine Zeile an Leerzeichen und doppelklicken Sie auf das UpdatePanel-Steuerelement in der Toolbox. Beachten Sie, dass ein neues `@Register` Richtlinie ist enthalten, am oberen Rand der Seite, gibt an, dass Steuerelemente innerhalb des System.Web.UI Namespaces importiert werden soll, mithilfe der `asp:` Präfix.
3. Ziehen Sie das schließende `</asp:UpdatePanel>` kennzeichnen das Ende der Button-Element, sodass das Element mit der Bezeichnung und Schaltflächen-Steuerelementen, die umschlossen wohlgeformt ist.
4. Nach der öffnenden `<asp:UpdatePanel>` zu kennzeichnen, öffnen ein neues Tag zu beginnen. Beachten Sie, dass IntelliSense Sie mit beiden Optionen dazu aufgefordert werden. In diesem Fall erstellen Sie eine `<ContentTemplate>` Tag. Achten Sie darauf, dass Sie dieses Tag, um die Bezeichnung und eine Schaltfläche zu umschließen, damit das Markup wohlgeformt ist.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))


1. An einer beliebigen Stelle innerhalb der `<form>` Element, enthalten ein ScriptManager-Steuerelement durch Doppelklicken auf die `ScriptManager` Element in der Toolbox.
2. Bearbeiten der `<asp:ScriptManager>` kennzeichnen, sodass sie das Attribut umfasst `EnablePartialRendering= true`.

**Auflisten von 3: Markup für "default.aspx" mit Teilrendering aktiviert**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Öffnen Sie die Datei "Web.config". Beachten Sie, dass Visual Studio automatisch ein Verweis Kompilierung System.Web.Extensions.dll hinzugefügt wurde.

1. Neues in Visual Studio 2008: die Datei "Web.config", die automatisch mit den Projektvorlagen ASP.NET-Website stammen enthält alle erforderlichen Verweise auf die ASP.NET AJAX-Erweiterungen Abschnitte gegliedert: kommentierten von Konfigurationsinformationen, die sein können nicht kommentierten auf zusätzliche Funktionen zu aktivieren. Visual Studio 2005 mussten ähnliche Vorlagen aus, wenn ASP.NET 2.0 AJAX-Erweiterungen installiert wurden. Allerdings sind die AJAX-Erweiterungen in Visual Studio 2008 opt-Out in der Standardeinstellung (d. h. sie werden standardmäßig verwiesen, aber können entfernt werden, da Verweise).


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))


1. Drücken Sie F5, um Ihre Website zu starten. Beachten Sie, wie keine quellcodeänderungen musste Teilrendering unterstützen – nur Markup wurde geändert.

Wenn Sie Ihre Website starten, sollten Sie sehen, dass Teilrendering nun aktiviert, da beim Klicken auf die Schaltfläche es keine Flackern werden, noch wird keine Änderung an der Seite Bildlaufposition (in diesem Beispiel wird nicht veranschaulicht) vorhanden sein. Würden Sie eine Quelle des gerenderten Seite Suchen Sie nach dem Klicken auf die Schaltfläche wird es zu bestätigen, dass in der Tat eine nach der Sicherung ist nicht aufgetreten – der Originaltext der Bezeichnung immer noch Teil der Quellmarkup ist und die Bezeichnung über JavaScript geändert hat.

Visual Studio 2008 wird nicht angezeigt, auf die im Lieferumfang einer vordefinierten Vorlage für eine ASP.NET AJAX-aktivierten-Website. Allerdings war solchen Vorlage in Visual Studio 2005 verfügbar, wenn der Visual Studio 2005 und ASP.NET 2.0 AJAX-Erweiterungen installiert wurden. Folglich sind Konfigurieren einer Website, und starten mit der Vorlage für AJAX-Website wahrscheinlich noch einfacher, da die Vorlage eine vollständig konfigurierte Datei "Web.config"-Datei (Unterstützung aller ASP.NET AJAX-Erweiterungen, einschließlich des Zugriffs für Webdienste enthalten soll und JSON-Serialisierung - JavaScript Object Notation) und ein UpdatePanel und ContentTemplate in Web Forms-Hauptseite standardmäßig enthält. Teilweises Rendern mit diesem Standardseite aktivieren ist so einfach wie Schritt 10 dieser exemplarischen Vorgehensweise überarbeiten und Ablegen von Steuerelementen auf der Seite.

## <a name="the-scriptmanager-control"></a>Die ScriptManager-Steuerelement

## <a name="scriptmanager-control-reference"></a>ScriptManager-Steuerelement-Referenz

Eigenschaften von Markup aktiviert:

| **Eigenschaftenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| AllowCustomErrors-Redirect | Bool | Gibt an, ob der benutzerdefinierte Fehlerabschnitt der Datei "Web.config" zu verwenden, um Fehler zu behandeln. |
| AsyncPostBackError-Message | Zeichenfolge | Ruft ab oder legt die Fehlermeldung an den Client gesendet werden, wenn ein Fehler ausgelöst wird. |
| AsyncPostBack-Timeout | Int32 | Ruft ab oder legt die Standarddauer ein, die ein Client, für die asynchrone Anforderung warten soll abgeschlossen. |
| EnableScript-Globalization | Bool | Ruft ab oder legt fest, ob das Skript Globalisierung aktiviert ist. |
| EnableScript-Localization | Bool | Ruft ab oder legt fest, ob das Skript Lokalisierung aktiviert ist. |
| ScriptLoadTimeout | Int32 | Bestimmt die Anzahl der Sekunden, die zum Laden von Skripts in den Client zulässig |
| ScriptMode | Enum (Auto, Debuggen, freigeben, erben) | Ruft ab oder legt ihn fest, ob gerendert werden Releaseversionen von Skripts |
| ScriptPath | Zeichenfolge | Ruft ab oder legt den Root-Pfad zum Speicherort der Skriptdateien, die an den Client gesendet werden. |

Eigenschaften von reinen:

| **Eigenschaftenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-Manager | Ruft Details zu den ASP.NET-Authentifizierungsdienst-Proxy, der an den Client gesendet werden sollen. |
| IsDebuggingEnabled | Bool | Ruft ab, ob Skripts und zu Codedebuggen ist aktiviert. |
| IsInAsyncPostback | Bool | Ruft ab, ob die Seite derzeit in eine asynchrone Postback Anforderung ist. |
| ProfileService | ProfileService-Manager | Ruft Details zu den ASP.NET-Dienst Profiling-Proxy, der an den Client gesendet werden sollen. |
| Skripts | Collection&lt;Script-Reference&gt; | Ruft eine Auflistung von Skriptverweisen, die an den Client gesendet werden sollen. |
| Dienste | Auflistung&lt;Dienstverweis&gt; | Ruft eine Auflistung von Web Service Proxyverweisen, die an den Client gesendet werden sollen. |
| SupportsPartialRendering | Bool | Ruft ab, ob der aktuelle Client Teilrendering unterstützt. Wenn diese Eigenschaft gibt **"false"**, und klicken Sie dann alle Seitenanforderungen standard Postbacks werden. |

Methoden in öffentlichen Code:

| **Methodenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| SetFocus(string) | Void | Legt den Fokus des Clients zu einem bestimmten Steuerelement fest, wenn die Anforderung abgeschlossen wurde. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;AuthenticationService&gt; | Enthält Details über den Proxy auf den Authentifizierungsdienst ASP.NET. |
| &lt;ProfileService&gt; | Stellt Details über den Proxy an den Dienst für die ASP.NET-PROFILERSTELLUNG bereit. |
| &lt;Skripts&gt; | Bietet zusätzliche Skriptverweise. |
| &lt;asp:ScriptReference&gt; | Gibt einen bestimmten Skriptverweis an. |
| &lt;Dienst&gt; | Enthält zusätzliche Webdienst-Verweise die Proxyklassen generiert haben. |
| &lt;asp:ServiceReference&gt; | Gibt einen bestimmten Webdienst-Verweis an. |

Die ScriptManager-Steuerelement wird der grundlegende Kern des ASP.NET AJAX-Erweiterungen. Es ermöglicht den Zugriff auf den Skriptbibliothek (einschließlich das Typsystem umfangreiche clientseitigem Skript), unterstützt teilweises Rendern und bietet umfangreiche Unterstützung für zusätzliche ASP.NET-Dienste (z. B. Authentifizierung und die profilerstellung, sondern auch andere Webdienste). Die ScriptManager-Steuerelement bietet auch Unterstützung von Globalisierung und Lokalisierung für die Clientskripts.

## <a name="providing-alterative-and-supplemental-scripts"></a>Bereitstellen von Alterative und zusätzliche Skripts

Während die Microsoft ASP.NET 2.0 AJAX-Erweiterungen schließen Sie den gesamten Skriptcode in sowohl Debug- und Editionen in der referenzierten Assemblys eingebettete Ressourcen freigeben, sind Entwickler frei, um ScriptManager an benutzerdefinierten Skriptdateien umleiten, als auch registrieren zusätzliche erforderlichen Skripts.

Um die standardbindung für die Skripts in der Regel enthalten (z. B. diejenigen, bei denen Sys.WebForms-Namespace und dem benutzerdefinierten Typisierung System unterstützen) zu überschreiben, können Sie registrieren für die `ResolveScriptReference` Ereignis der ScriptManager-Klasse. Wenn diese Methode aufgerufen wird, hat der Ereignishandler die Möglichkeit, den Pfad zu der fraglichen Skriptdatei zu ändern; der Skript-Manager wird eine andere oder angepasste Kopie des Skripts klicken Sie dann an den Client senden.

Darüber hinaus Skriptverweise (dargestellt durch die `ScriptReference` Klasse) kann programmgesteuert oder über Markup enthalten sein. Zu diesem Zweck entweder programmgesteuert ändern die `ScriptManager.Scripts` Auflistung oder `<asp:ScriptReference>` tags unter der `<Scripts>` Tags, das auf der obersten Ebene das ScriptManager-Steuerelement untergeordnet ist.

## <a name="custom-error-handling-for-updatepanels"></a>Benutzerdefinierte Fehlerbehandlung UpdatePanels

Obwohl Updates durch Trigger, die gemäß UpdatePanel Steuerelemente behandelt werden, wird die Unterstützung für die Fehlerbehandlung und benutzerdefinierte Fehlermeldungen von einer Seite ScriptManager-Steuerelement-Instanz behandelt. Dies erfolgt durch ein Ereignis verfügbar machen `AsyncPostBackError`, um die Seite kann dann benutzerdefinierte Ausnahmebehandlung Logik bereitstellen.

Durch die Nutzung des AsyncPostBackError-Ereignisses, Sie können angeben, die `AsyncPostBackErrorMessage` -Eigenschaft, klicken Sie dann daraufhin ein Warnfeld nach Abschluss des Rückrufs ausgelöst werden soll.

Die clientseitige Anpassung ist auch möglich, anstatt das Standard Warnung; Sie möchten z. B. eine benutzerdefinierte anzeigen `<div>` Element anstelle der Standardeinstellung Browser modales Dialogfeld. In diesem Fall können Sie den Fehler in Clientskripts behandeln:

**Auflisten von 5: Clientseitigem Skript benutzerdefinierte angezeigt werden sollen**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Das obige Skript registriert einen Rückruf ganz einfach mit der die clientseitige AJAX-Laufzeit für, wenn die asynchrone Anforderung abgeschlossen wurde. Es wird dann geprüft, ob ein Fehler gemeldet wurde, und wenn dies der Fall ist, verarbeitet die Details des Zertifikats, schließlich, der angibt, für die Laufzeit, dass der Fehler in benutzerdefinierten Skripts behandelt wurde.

## <a name="globalization-and-localization-support"></a>Globalisierung und Lokalisierungsunterstützung

Die ScriptManager-Steuerelement bietet umfangreiche Unterstützung für die Lokalisierung von Skriptzeichenfolgen und Benutzeroberflächenkomponenten; Allerdings ist dieses Thema außerhalb des Bereichs dieses Whitepaper verfasst. Weitere Informationen finden Sie im Whitepaper Globalisierung-Unterstützung in ASP.NET AJAX-Erweiterungen.

## <a name="the-updatepanel-control"></a>UpdatePanel-Steuerelement

## <a name="updatepanel-control-reference"></a>UpdatePanel-Steuerelement-Referenz

Eigenschaften von Markup aktiviert:

| **Eigenschaftenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Gibt an, ob die untergeordneten Steuerelemente automatisch aktualisieren beim Postback aufrufen. |
| RenderMode | Enum ("blockieren", "Inline") | Gibt an, wie der Inhalt visuell angezeigt werden. |
| UpdateMode | Enum (immer, bedingte) | Gibt an, ob UpdatePanel immer, während eine teilweise Rendern aktualisiert wird oder sie nur aktualisiert wird, wenn ein Trigger erreicht wird. |

Eigenschaften von reinen:

| **Eigenschaftenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| IsInPartialRendering | bool | Ruft ab, ob die UpdatePanel Teilrendering für die aktuelle Anforderung unterstützt wird. |
| ContentTemplate | ITemplate | Ruft das Markup-Vorlage für die updateanforderung ab. |
| ContentTemplateContainer | Steuerelement | Ruft die programmgesteuerte Vorlage für die updateanforderung ab. |
| Trigger | UpdatePanel - TriggerCollection | Ruft die Liste der aktuellen UpdatePanel zugeordneten Trigger ab. |

Methoden in öffentlichen Code:

| **Methodenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| Update() | Void | Der angegebene UpdatePanel aktualisiert programmgesteuert. Ermöglicht es eine Anforderung einer teilweise Rendern von UpdatePanel andernfalls untriggered auslösen. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ContentTemplate&gt; | Gibt das Markup zum Rendern verwendet werden, um das Teilrendering Ergebnis zu rendern. Untergeordnetes Element des &lt;Asp: UpdatePanel&gt;. |
| &lt;Trigger&gt; | Gibt eine Auflistung von *n* Kontrollen hinsichtlich dieser UpdatePanel aktualisieren. Untergeordnetes Element des &lt;Asp: UpdatePanel&gt;. |
| &lt;asp:AsyncPostBackTrigger&gt; | Gibt einen Trigger, der für den angegebenen UpdatePanel-Teilseite Rendern aufruft. Dies kann oder ein Steuerelement darf nicht als ein Nachfolger des fraglichen UpdatePanel sein. Genau ist, um den Namen des Ereignisses. Untergeordnetes Element des &lt;Trigger&gt;. |
| &lt;asp:PostBackTrigger&gt; | Gibt ein Steuerelement, das bewirkt, die gesamte Seite dass zu aktualisieren. Dies kann oder ein Steuerelement darf nicht als ein Nachfolger des fraglichen UpdatePanel sein. Genau ist, um das Objekt. Untergeordnetes Element des &lt;Trigger&gt;. |

Die `UpdatePanel` ist das Steuerelement, das den Inhalt für die serverseitige begrenzt, die die Teilrendering Funktionalität der AJAX-Erweiterungen teilnimmt. Besteht keine Einschränkung auf die Anzahl der UpdatePanel-Steuerelemente, die auf einer Seite befinden können, und können geschachtelt sein. Jede UpdatePanel ist isoliert und hat, damit jeweils unabhängig voneinander arbeiten kann (Sie können zwei UpdatePanels zur gleichen Zeit ausgeführt wird, die verschiedene Teile der Seite, unabhängig von der Seite Postback Rendern haben).

UpdatePanel steuern in erster Linie die Aufträge mit Triggern Steuerelement – standardmäßig wird jedes Steuerelement innerhalb eines UpdatePanels enthalten `ContentTemplate` , was zu einen Postback erstellt, die als Trigger für die UpdatePanel registriert ist. Dies bedeutet, dass UpdatePanel kann mit der Standardeinstellung von datengebundenen Steuerelementen (z. B. die GridView) mit Benutzersteuerelementen arbeiten, und sie im Skript programmiert werden können.

Standardmäßig können Sie beim Rendern Teilseite ausgelöst wird, alle UpdatePanel-Steuerelemente auf der Seite aktualisiert werden, davon, ob die UpdatePanel steuert Trigger für eine solche Aktion definiert. Beispielsweise werden, wenn ein UpdatePanel ein Schaltflächen-Steuerelement definiert, und diese Schaltflächen-Steuerelement geklickt wird, alle UpdatePanel-Steuerelemente auf dieser Seite standardmäßig aktualisiert. Dies liegt daran, dass standardmäßig die `UpdateMode` von UpdatePanel ist-Eigenschaftensatz auf `Always`. Alternativ können Sie die UpdateMode-Eigenschaft festlegen, um `Conditional`, was bedeutet, dass UpdatePanel wird nur aktualisiert werden, wenn Sie ein bestimmten Trigger erreicht wird.

## <a name="custom-control-notes"></a>Benutzerdefiniertes Steuerelement Hinweise

UpdatePanel kann beliebige Benutzersteuerelement oder benutzerdefinierte Steuerelemente hinzugefügt werden; die Seite, auf denen diese Steuerelemente enthalten sind, umfasst jedoch auch ein ScriptManager-Steuerelement mit der Eigenschaft EnablePartialRendering auf **"true"**.

Eine Möglichkeit, in denen Sie möglicherweise Konto für diesen Wenn benutzerdefinierte Websteuerelemente mit den geschützten überschreiben `CreateChildControls()` Methode der `CompositeControl` Klasse. Auf diese Weise können Sie eine UpdatePanel zwischen untergeordneten Elemente des Steuerelements und der Außenwelt einfügen, wenn Sie feststellen, dass die Seite Teilrendering unterstützt; Andernfalls können Sie einfach die untergeordneten Steuerelemente überlagern Sie die in einem Container `Control` Instanz.

## <a name="updatepanel-considerations"></a>UpdatePanel-Überlegungen

UpdatePanel arbeitet sich in etwa eine Blackbox wrapping ASP.NET-Postbacks innerhalb des Kontexts von einem JavaScript XMLHttpRequest. Es gibt jedoch erhebliche Leistungsaspekte Bedenken im Hinblick auf das Verhalten und die Geschwindigkeit der Diagnosefunktion berücksichtigen. Zum Verständnis der Funktionsweise von UpdatePanel, damit Sie am besten können Sie entscheiden, wann die Verwendung geeignet sind, sollten Sie die AJAX-Exchange untersuchen. Im folgenden Beispiel wird einem vorhandenen Standort und Mozilla Firefox, mit der Firebug-Erweiterung (Firebug erfasst XMLHttpRequest-Daten).

Betrachten Sie ein Formular, die u. a. ein Textfeld für die Postleitzahl besitzt, die ein Feld für Stadt und Bundesstaat in einem Formular oder Steuerelement Auffüllen soll. Dieses Formular werden letztlich Mitgliedschaftsinformationen, einschließlich Name, Adresse und Kontaktinformationen des Benutzers erfasst. Es gibt viele entwurfsüberlegungen zu berücksichtigen, auf der Grundlage der Anforderungen eines bestimmten Projekts.


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))


In der ursprünglichen Iteration dieser Anwendung wurde ein Steuerelement erstellt, die die Gesamtheit der Registrierung Benutzerdaten, einschließlich Postleitzahl, City und State aufgenommen. Das gesamte Steuerelement war innerhalb eines UpdatePanel umschlossen und ein Webformular abgelegt. Wenn die Postleitzahl, die vom Benutzer eingegeben wird, erkennt der UpdatePanel das Ereignis (das entsprechende TextChanged-Ereignis mit dem Back-End, entweder durch Angabe der Trigger oder über die ChildrenAsTriggers-Eigenschaft auf "true" festgelegt). AJAX sendet alle Felder innerhalb der UpdatePanel wie FireBug erfasst (Siehe das Diagramm auf der rechten Seite).

Wie die Bildschirmaufnahme hervorgeht, werden Werte aus jeder Steuerelement innerhalb der UpdatePanel übermittelt (in diesem Fall werden alle leer), sowie das Feld "ViewState" speichern. Insgesamt wird mehr als 9 kb Daten gesendet, wenn tatsächlich nur fünf Bytes der Daten für diese bestimmte Anforderung benötigt wurden. Die Antwort ist sogar noch stärker sehr großer: insgesamt 57kb einfach, um ein Textfeld und ein Dropdown-Feld aktualisieren an den Client gesendet.

Es kann auch von Interesse sind, um festzustellen, wie die Darstellung von ASP.NET AJAX aktualisiert sein. Der Antwortbereich eines der UpdatePanel Update-Anforderung wird in der Konsole Firebug auf der linken Seite angezeigt; Es ist eine speziell formuliert senkrechte Striche getrennte Zeichenfolge, die durch das Clientskript unterteilt ist, und klicken Sie dann auf der Seite zusammengesetzt. Insbesondere ASP.NET AJAX legt die *InnerHTML* Eigenschaft des HTML-Elements auf dem Client, der Ihre UpdatePanel darstellt. Wie der Browser das DOM erneut generiert, besteht eine geringfügige Verzögerung kommen, abhängig vom Umfang der Informationen, die verarbeitet werden muss.

Die erneute Generierung des DOM löst einige zusätzliche Probleme:


[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))


- Wenn UpdatePanel HTML-Elements mit Fokus befindet, verloren den Fokus. Deshalb für Benutzer, die die Tab-Taste, um das Textfeld für die Postleitzahl Beenden geklickt, hätten ihr Ziel weitere im Textfeld "City". Allerdings nach der Aktualisierung der Anzeige durch den UpdatePanel Form den Fokus nicht mehr wären, und durch Drücken von Tab würde gestartet wurden Hervorhebung der Fokus-Elemente (z. B. Links).
- Wenn jede Art von benutzerdefinierten Skriptdatei auf Clientseite verwendet wird, greift auf DOM-Elemente, Verweise, die von Funktionen beibehalten kann nach einem Postback partielle außer Kraft.

UpdatePanels dienen nicht zur Catchall-Lösungen zu werden. Vielmehr, sie bieten eine schnelle Lösung für bestimmte Situationen, einschließlich Erstellen von Prototypen, kleine Steuerelement Updates, und geben Sie eine vertraute Oberfläche für ASP.NET-Entwickler, die möglicherweise mit dem Objektmodell von .NET vertraut sind, aber damit weniger mit dem DOM Es gibt eine Reihe von behandelt, die eine bessere Leistung, je nach Anwendungsszenario führen kann:

- PageMethods in Betracht, und JSON (JavaScript Object Notation) ermöglicht dem Entwickler, statische Methoden, die auf einer Seite zu aufzurufen, als ob ein Aufruf des Webdiensts aufgerufen wurde. Da die Methoden statisch sind, ist kein Zustand erforderlich. der Aufrufer Skript liefert die Parameter, und das Ergebnis wird asynchron zurückgegeben.
- Wenn ein einzelnes Steuerelement an mehreren Stellen innerhalb einer Anwendung verwendet werden muss, sollten Sie einen Webdienst und JSON in Betracht ziehen. Diese erneut kaum besonderen Schritte erfordert, und arbeitet asynchron.

Integrieren von Funktionen über Webdienste oder Seitenmethoden hat auch Nachteile. Zunächst sind tendenziell ASP.NET-Entwickler in der Regel kleine Komponenten von Funktionalität in Benutzersteuerelemente (ASCX-Dateien) zu erstellen. Seitenmethoden können nicht in diesen Dateien gehostet werden. Sie müssen innerhalb der Klasse des tatsächlichen ASPX-Seite gehostet werden. Webdienste, müssen innerhalb der ASMX-Klasse auf ähnliche Weise gehostet werden. Abhängig von der Anwendung dieser Architektur dem Prinzip der einzigen Verantwortung, verletzen könnten, da die Funktionalität für eine einzelne Komponente jetzt über mindestens zwei physische Komponenten verteilt wird, die nur wenig oder keine zusammenhängende Ties verfügen können.

Wenn eine Anwendung erfordert, dass UpdatePanels verwendet werden, sollte die folgenden Richtlinien schließlich mit der Problembehandlung und Wartung unterstützen.

- **Schachteln Sie UpdatePanels so wenig wie möglich, nicht nur in Einheiten, sondern auch in Einheiten von Code.** Beispielsweise ist ein UpdatePanel auf einer Seite, die ein Steuerelement umschließt, während das Steuerelement auch UpdatePanel, das ein anderes Steuerelement, die eine UpdatePanel enthält enthält enthält, Cross-Einheit Schachtelung. Dies hilft zu deaktivieren, welche Elemente aktualisiert werden soll, und verhindert, dass unerwartete Aktualisierungen an untergeordneten UpdatePanels.
- **Behalten Sie die *ChildrenAsTriggers* Eigenschaft auf "false" festgelegt und explizit festgelegt Auslösen von Ereignissen.** Nutzung der `<Triggers>` Auflistung ist eine sehr viel genauere Möglichkeit zum Behandeln von Ereignissen und möglicherweise zu unerwartetes Verhalten führen, hilft Ihnen bei der Wartungsaufgaben und Erzwingen von Entwicklern, sich für ein Ereignis teilnehmen verhindern.
- **Verwenden Sie die kleinste mögliche Einheit, um die Funktionalität zu erzielen.** Wie bereits erwähnt, dass nur die absolute Mindestanforderungen für den Server, insgesamt Verarbeitung und der Speicherbedarf der Exchange-Client / Server-Zeit reduziert wird, bei der Erläuterung der Postleitzahl-Dienst, umschließen, Verbessern der Leistung.

## <a name="the-updateprogress-control"></a>UpdateProgress-Steuerelement

## <a name="updateprogress-control-reference"></a>UpdateProgress-Steuerelement-Referenz

Eigenschaften von Markup aktiviert:

| **Eigenschaftenname** | **Type** | **Beschreibung** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | Zeichenfolge | Gibt die ID des UpdatePanel, die auf diese UpdateProgress gemeldet werden sollen. |
| DisplayAfter | Int | Gibt das Timeout in Millisekunden, bevor dieses Steuerelement angezeigt wird, nach dem Beginn der asynchronen Anforderung. |
| DynamicLayout | bool | Gibt an, ob der Status dynamisch gerendert wird. |

Markup Nachfolger:

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Enthält die Steuerelementvorlage legen Sie für den Inhalt, der mit diesem Steuerelement angezeigt wird. |

UpdateProgress-Steuerelement bietet Ihnen ein Maß für Feedback zu Ihrer Benutzer zu überwachende während der Durchführung der notwendigen Schritte zum Transport mit dem Server. Dadurch können die Benutzer wissen, dass Sie etwas tun, obwohl offensichtlich, möglicherweise nicht besonders, da die Seite aktualisieren, und markieren Sie auf der Statusleiste wird angezeigt, die meisten Benutzer verwendet werden.

Als ein Knoten können UpdateProgress-Steuerelementen an einer beliebigen Stelle auf einer Seitenhierarchie angezeigt werden. Allerdings postbacks in Fällen, in denen ein partielles Postback von einem untergeordneten UpdatePanel initiiert wird (wobei UpdatePanel innerhalb einer anderen UpdatePanel geschachtelt ist), Trigger das untergeordnete Element für das untergeordnete Element UpdatePanel UpdateProgress Vorlagen angezeigt werden zurückgesetzt UpdatePanel als auch das übergeordnete Element UpdatePanel. Aber der Auslöser direkt untergeordnetes Element des übergeordneten Elements UpdatePanel ist, werden nur die UpdateProgress-Vorlagen, die mit dem übergeordneten Element angezeigt.

## <a name="summary"></a>Zusammenfassung

Die Microsoft ASP.NET AJAX-Erweiterungen sind anspruchsvolle Produkte, die entwickelt, um Webinhalte leichter zugänglich machen unterstützen und eine umfangreichere benutzererfahrung auf die Webanwendungen. Als Teil der ASP.NET AJAX-Erweiterungen, die Teilseite Rendern von Steuerelementen, sind auch die ScriptManager, UpdatePanel und UpdateProgress einige sichtbarste Komponenten des Toolkits.

Die ScriptManager-Komponente wird die Bereitstellung für die Erweiterungen des JavaScript-Clients integriert sowie kann die verschiedenen Server und die clientseitige Komponenten funktionieren nun zusammen mit minimalen Entwicklung Investitionen.

UpdatePanel-Steuerelements ist das offensichtlich Magic - Markup in UpdatePanel serverseitige Codebehind verfügen muss, damit eine Seite-Aktualisierung nicht auslösen. UpdatePanel-Steuerelemente können geschachtelt sein, und Sie können Steuerelemente in dies abhängig sein. Standardmäßig behandeln UpdatePanels alle Postbacks aufgerufen, indem die untergeordneten Steuerelemente, obwohl diese Funktionalität fein, deklarativ oder programmgesteuert optimiert werden kann.

Wenn Sie das Steuerelement UpdatePanel verwenden zu können, sollten Entwickler die Leistungseinbußen bei bewusst sein, die möglicherweise auftreten können. Mögliche Alternativen enthalten Webdienste und Seitenmethoden, obwohl Sie der Entwurf der Anwendung berücksichtigt werden sollten.

Die UpdateProgress ermöglicht eine Steuerung den Benutzer wissen, dass er wird nicht ignoriert, und, dass die hinter den Kulissen Anforderung während die Seite wird auf andernfalls nicht übernehmen, um auf Benutzereingabe zu reagieren. Darüber hinaus die Möglichkeit, das Teilrendering Ergebnisse abzubrechen.

Diese Tools zusammen unterstützen, erstellen eine umfangreiche und eine nahtlose benutzererfahrung funktioniert für die Benutzer weniger offensichtlich und weniger Unterbrechung des Workflows.

## <a name="bio"></a>Lebenslauf

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Nächste](understanding-asp-net-ajax-updatepanel-triggers.md)
