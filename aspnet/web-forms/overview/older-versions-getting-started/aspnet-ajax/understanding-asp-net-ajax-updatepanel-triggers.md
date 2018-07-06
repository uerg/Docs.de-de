---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Grundlegendes zu UpdatePanel-Triggern in ASP.NET AJAX | Microsoft-Dokumentation
author: scottcate
description: Bei der Arbeit in den Markup-Editor in Visual Studio können Sie (von IntelliSense) Beachten Sie, dass es zwei untergeordnete Elemente von einem UpdatePanel-Steuerelement gibt. Einer der w-fragewörter...
ms.author: aspnetcontent
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1b2e70a2b074d3c0a2fa4b669b3645ae5f7f790e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811930"
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Grundlegendes zu UpdatePanel-Triggern in ASP.NET AJAX
====================
durch [Scott Cate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Bei der Arbeit in den Markup-Editor in Visual Studio können Sie (von IntelliSense) Beachten Sie, dass es zwei untergeordnete Elemente von einem UpdatePanel-Steuerelement gibt. Einer davon ist das Trigger-Element gibt an, die Steuerelemente auf der Seite (oder das Benutzersteuerelement, wenn Sie einen verwenden), der auslöst einer partiellen Rendering des UpdatePanel-Steuerelements in der das Element befindet.


## <a name="introduction"></a>Einführung

Die Technologie von Microsoft ASP.NET bietet ein Programmiermodell, das objektorientierte und ereignisgesteuerte und vereinen sie die Vorteile des kompilierten Codes. Das Modell für die serverseitige Verarbeitung hat jedoch einige Nachteile, die sich von der Technologie, von die viele durch die neuen Features in den Microsoft ASP.NET 3.5-AJAX-Erweiterungen behandelt werden können. Mithilfe dieser Erweiterungen können viele neue rich-Client-Features, darunter Teilrendering von Seiten ohne einer vollständigen seitenaktualisierung, den Zugriff auf Webdienste über Clientskript (einschließlich ASP.NET profilerstellungs-API) und eine umfangreiche clientseitige API entwickelt, um viele der Steuerelement-Schemas finden Sie in den Satz der ASP.NET serverseitigen Steuerelements zu spiegeln.

In diesem Whitepaper werden die XML-Trigger-Funktionalität von ASP.NET AJAX untersucht `UpdatePanel` Komponente. XML-Trigger geben Sie eine präzise Kontrolle über die Komponenten, die dazu partielles Rendering für bestimmte UpdatePanel-Steuerelemente.

In diesem Whitepaper basiert auf der Version von .NET Framework 3.5 Beta 2 und Visual Studio 2008. ASP.NET AJAX Extensions, zuvor eine Add-on-Assembly, die auf ASP.NET 2.0 ausgerichtet sind, sind jetzt in der .NET Framework-Basisklassenbibliothek integriert. In diesem Whitepaper wird vorausgesetzt, dass Sie mit Visual Studio 2008 nicht Visual Web Developer Express arbeiten werden und bieten Exemplarische Vorgehensweisen, gemäß der Benutzeroberfläche von Visual Studio (obwohl codeauflistungen vollständig unabhängig von kompatibel sind Entwicklungsumgebung).

## <a name="triggers"></a>*Trigger*

Trigger für eine bestimmte UpdatePanel standardmäßig enthalten automatisch alle untergeordneten Steuerelemente, die ein Postback handeln, einschließlich der TextBox-Steuerelemente, die (z. B.) aufrufen ihre `AutoPostBack` -Eigenschaftensatz auf **"true"**. Trigger können jedoch auch deklarativ mithilfe von Markup enthalten sein. Dies erfolgt im der `<triggers>` Teil der Deklaration des UpdatePanel-Steuerelement. Obwohl Trigger über zugegriffen werden können die `Triggers` Auflistungseigenschaft, es wird empfohlen, dass keine partiellen Rendering Trigger zur Laufzeit registriert werden (z. B. wenn ein Steuerelement zur Entwurfszeit nicht verfügbar ist) mit der `RegisterAsyncPostBackControl(Control)` -Methode der der ScriptManager Objekt für die Seite in der `Page_Load` Ereignis. Denken Sie daran, dass die Seiten sind zustandslos und daher sollte erneut registrieren dieser Steuerelemente jedes Mal, wenn sie erstellt werden.

Automatische untergeordneten Trigger einschließen kann auch deaktiviert werden (so, dass untergeordnete Steuerelemente, die Postbacks erstellen teilweise rendert nicht automatisch auslöst) durch Festlegen der `ChildrenAsTriggers` Eigenschaft **"false"**. Dies gibt Ihnen die größte Flexibilität beim Zuweisen von bestimmten steuert rufen möglicherweise eine Seite gerendert, und es wird empfohlen, sodass ein Entwickler opt-in wird zum Reagieren auf ein Ereignis, anstatt die Behandlung der Ereignisse, die auftreten können.

Beachten Sie, dass wenn UpdatePanel-Steuerelemente geschachtelt sind, wenn die UpdateMode auf festgelegt ist **bedingte**, wenn das untergeordnete Element UpdatePanel wird ausgelöst, aber das übergeordnete Element ist nicht der Fall, klicken Sie dann nur die untergeordneten UpdatePanel aktualisiert wird. Jedoch, wenn das übergeordnete Element UpdatePanel aktualisiert wird, klicken Sie dann das untergeordnete Element UpdatePanel auch aktualisiert werden.

## <a name="the-lttriggersgt-element"></a>*Die &lt;Trigger&gt; Element*

Wenn Sie in den Markup-Editor in Visual Studio arbeiten, Sie bemerken (von IntelliSense), dass es zwei untergeordnete Elemente gibt von einer `UpdatePanel` Steuerelement. Das am häufigsten verwendeten angezeigte Element ist die `<ContentTemplate>` -Element, das im Wesentlichen auf den Inhalt kapselt, die mit dem Update-Bereich gespeichert werden (der Inhalt für das werden wir partielles Rendering aktivieren). Das andere Element ist die `<Triggers>` Element, das die Steuerelemente auf der Seite (oder das Benutzersteuerelement, wenn Sie einen verwenden) angibt, der eine partielle Rendering des UpdatePanel-Steuerelements in der auslöst der &lt;Trigger&gt; Element befindet.

Die `<Triggers>` Element kann eine beliebige Anzahl jedes von zwei untergeordnete Knoten enthalten: `<asp:AsyncPostBackTrigger>` und `<asp:PostBackTrigger>`. Beide übernehmen zwei Attribute, `ControlID` und `EventName`, und geben Sie jedes Steuerelement in der aktuellen Einheit der Kapselung kann (z. B. wenn sich das UpdatePanel-Steuerelement in einer Web-Benutzersteuerelement befindet, Sie sollten nicht versuchen, auf ein Steuerelement zu verweisen. die Seite, auf der das Steuerelement befindet).

Die `<asp:AsyncPostBackTrigger>` Element ist besonders nützlich, ein Ereignis von einem Steuerelement ausgeführt werden können, die als untergeordnetes Element vorhanden ist, *alle* UpdatePanel-Steuerelement in der Einheit Kapselungsschichten kommt, nicht nur das UpdatePanel, unter dem dieser Trigger ein untergeordnetes Element ist, . Daher kann jedes Steuerelement vorgenommen werden, um eine partielle Aktualisierung auszulösen.

Auf ähnliche Weise die `<asp:PostBackTrigger>` Element kann verwendet werden, um den Trigger, die eine partielle Seite zu rendern, aber eine, die einen vollständige Roundtrip zum Server erforderlich sind. Dieser Trigger-Element kann auch verwendet werden, um eine gesamte Seite rendern zu erzwingen, wenn ein Steuerelement ein renderobjekt Teilseite andernfalls normalerweise auslösen würde (z. B. wenn ein `Button` Steuerelement vorhanden ist, der `<ContentTemplate>` Element von einem UpdatePanel-Steuerelement). In diesem Fall kann das PostBackTrigger Element jedes Steuerelement angeben, die ein untergeordnetes Element eines beliebigen UpdatePanel-Steuerelements in der aktuellen Einheit der Kapselung ist.

## <a name="lttriggersgt-element-reference"></a>*&lt;Trigger&gt; -Elementverweis*

*Nachfolger von Markup:*

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;asp:AsyncPostBackTrigger&gt; | Gibt an, ein Steuerelement und ein Ereignis, das einer partiellen seitenaktualisierung UpdatePanel verursachen kann, die diese Trigger-Verweis enthält. |
| &lt;asp:PostBackTrigger&gt; | Gibt an, ein Steuerelement und ein Ereignis, das einer vollständigen seitenaktualisierung (einer vollständigen seitenaktualisierung) führt. Dieses Tag kann verwendet werden, um eine vollständige Aktualisierung zu erzwingen, wenn ein Steuerelement andernfalls partielles Rendering auslösen würde. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Exemplarische Vorgehensweise: Cross-UpdatePanel-Triggern*

1. Erstellen Sie eine neue ASP.NET-Seite mit einem ScriptManager-Objekt, das zum Aktivieren des partiellen Renderings festlegen. Zwei UpdatePanels zu dieser Seite – in der ersten hinzugefügt werden, enthalten ein Label-Steuerelement (Label1) und zwei Schaltflächen-Steuerelemente (Button1 und Button2). "Button1" besser gesagt, klicken Sie auf, um beide zu aktualisieren und Button2 besser gesagt, klicken Sie auf, um diese oder eine ähnliche Funktion wie diese Zeilen zu aktualisieren. Enthalten Sie in der zweiten UpdatePanel nur ein Label-Steuerelement "Label2 ("), aber legen Sie die ForeColor-Eigenschaft auf einen anderen Wert als den Standardwert zur Kennzeichnung.
2. Legen Sie die UpdateMode-Eigenschaft der beiden Tags UpdatePanel **bedingte**.

**Codebeispiel 1: Markup für "default.aspx":** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Legen Sie im Click-Ereignishandler für Button1 Label1.Text und Label2.Text auf etwas zeitabhängige (z. B. DateTime.Now.ToLongTimeString()). Legen Sie für das Click-Ereignishandler für Button2 nur Label1.Text auf den zeitabhängige-Wert.

**Codebeispiel 2: Codebehind (hier gekürzt), in der Datei default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Drücken Sie F5, um das Projekt erstellen und ausführen. Beachten Sie, dass beide Bezeichnungen Text, wenn Sie Updatebereiche, die sowohl Klicken ändern, Wenn Sie dieses Update-Panels klicken, wird jedoch nur "Label1" aktualisiert.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Einblick in die Hintergründe*

Verwenden das Beispiel, die, das wir soeben erstellt, können wir sehen Sie sich Was macht ASP.NET AJAX und die Funktionsweise von unserem UpdatePanel-Triggern in Cross-Bereich. Zu diesem Zweck arbeiten wir mit der generierten Seitenprofilklasse HTML-Quelle als auch die FireBug - wird aufgerufen, damit, Mozilla Firefox-Erweiterung können wir einfach die AJAX-Postbacks untersuchen. Wir verwenden auch das Tool .NET Reflector von Lutz Roeder. Beide Tools sind kostenlos online, und Sie können mit einer Internetsuche gefunden werden.

Eine Untersuchung der Quellcode der Seite wird fast nichts Ungewöhnliches angezeigt. Das UpdatePanel-Steuerelemente werden als gerendert `<div>` Container, und sehen die Skriptressource enthält, angegeben durch die `<asp:ScriptManager>`. Es gibt auch einige neue AJAX-spezifischen Aufrufe der PageRequestManager, die für die AJAX-Skript-Clientbibliothek intern sind. Zum Schluss sehen Sie die beiden UpdatePanel-Container – eine mit der gerenderten `<input>` Schaltflächen mit den beiden `<asp:Label>` Steuerelemente gerendert wird, als `<span>` Container. (Wenn Sie die DOM-Struktur in FireBug untersuchen, bemerken Sie, dass der Bezeichnungen abgeblendet sind, um anzugeben, dass sie nicht sichtbaren Inhalt erstellen).

Klicken Sie auf die Schaltfläche "Update dieses Panel", und beachten Sie, dass mit dem aktuellen Server Schlüsselzeit obere UpdatePanel aktualisiert wird. Wählen Sie in FireBug die Registerkarte "Console", damit Sie die Anforderung untersuchen können. Überprüfen Sie zuerst die Parameter der POST-Anforderung:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Beachten Sie, dass UpdatePanel auf den serverseitigen AJAX-Code angegeben hat, genau die Steuerelementstruktur über den Parameter ScriptManager1 ausgelöst wurde: `Button1` von der `UpdatePanel1` Steuerelement. Klicken Sie nun auf die Schaltfläche "Updatebereiche, die sowohl" auf. Klicken Sie dann dazu finden Sie untersuchen die Antwort, wir eine durch senkrechte Striche getrennte Reihe von Variablen, die in einer Zeichenfolge festgelegt. insbesondere sehen wir die obere UpdatePanel `UpdatePanel1`, hat die an den Browser gesendeten HTML-Dateien während des gesamten Entwicklungsprozesses. Die AJAX-Clientbibliothek Skript ersetzt das UpdatePanel ursprünglichen HTML-Inhalt mit dem neuen Inhalt über die `.innerHTML` -Eigenschaft, und der Server den geänderten Inhalt auf dem Server im HTML-Format sendet.

Nun, klicken Sie auf die Schaltfläche "Updatebereiche, die sowohl" aus, und überprüfen Sie die Ergebnisse vom Server. Die Ergebnisse sind sehr ähnlich – beide UpdatePanels neuen HTML-Code vom Server empfangen. Wie bei den vorherigen Rückruf wird zusätzliche seitenspezifischen Status gesendet.

Wie wir sehen, da keine spezieller Code zum Ausführen eines AJAX-Postbacks verwendet wird, kann die AJAX-Skript-Clientbibliothek Formular Postbacks ohne zusätzlichen Code abfangen. Serversteuerelemente nutzen automatisch JavaScript, damit sie das Formular nicht automatisch senden – ASP.NET automatisch Code, für die formularvalidierung und Status noch in erster Linie durch automatische Skripts Ressource einschließen, die PostBackOptions-Klasse erreicht einfügt. , und der ClientScriptManager-Klasse.

Betrachten Sie beispielsweise ein Kontrollkästchen-Steuerelement ein; Überprüfen Sie die Disassembly Klasse in .NET Reflector. Zu diesem Zweck stellen Sie sicher, dass die Assembly "System.Web" geöffnet ist, und navigieren Sie zu der `System.Web.UI.WebControls.CheckBox` -Klasse, öffnen die `RenderInputTag` Methode. Suchen Sie nach einer Bedingung, die überprüft die `AutoPostBack` Eigenschaft:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Wenn Automatisches Postback aktiviert ist, auf eine `CheckBox` steuern (über die AutoPostBack-Eigenschaft wird "true"), die resultierenden `<input>` Tag gerendert wird daher mit einem ASP.NET-Ereignis, das Behandeln von Skript in die `onclick` Attribut. Das Abfangen der Übermittlung des Formulars, klicken Sie dann, ermöglicht ASP.NET AJAX nonintrusively, auf der Seite eingefügt werden wichtige Änderungen, die auftreten können, durch die Verwendung einer möglicherweise ungenaue zeichenfolgenersetzungen potenzielle vermeiden helfen. Darüber hinaus ermöglicht dies *alle* benutzerdefiniertes ASP.NET-Steuerelement nutzen die Leistungsfähigkeit der ASP.NET AJAX ohne zusätzlichen Code in einem UpdatePanel-Container unterstützen.

Die `<triggers>` Funktionalität entspricht den Werten, die im Aufruf der PageRequestManager initialisiert \_UpdateControls (Beachten Sie, dass die ASP.NET AJAX-Clientbibliothek Skript der Konvention verwendet, diese Methoden, Ereignisse und Feldnamen, beginnen sind mit einem Unterstrich als intern markiert und sind nicht dafür vorgesehen, für die Verwendung außerhalb von die Bibliothek selbst). Mithilfe dieser Option können wir beobachten, welche Steuerelemente AJAX-Postbacks bewirken sollen.

Beispielsweise fügen Sie zwei zusätzliche Steuerelemente auf der Seite ein Steuerelement außerhalb von UpdatePanels völlig verlassen, und lassen eine in einem UpdatePanel-Steuerelement. Wir fügen ein Kontrollkästchen-Steuerelement in der oberen UpdatePanel, und löschen eine DropDownList mit einer Anzahl von Farben in der Liste definiert. Hier ist das neue Markup:

**Codebeispiel 3: Neue Markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Und hier ist das neue Code-Behind:

**Codebeispiel 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Die Idee hinter dieser Seite ist, dass die Dropdown Liste, eine von drei Farben wählt, die zweite Bezeichnung angezeigt, dass Sie dieses Kontrollkästchen legt fest, gibt an, ob es fett formatiert ist, und gibt an, ob die Bezeichnungen für das Datum als auch die Zeit anzuzeigen. Das Kontrollkästchen sollte nicht dazu, dass ein AJAX-Update, aber die Dropdown Liste sollten, obwohl es nicht innerhalb eines UpdatePanel befindet.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Wie im obigen Screenshot ersichtlich ist, wurde die neuesten Schaltfläche geklickt wird mit der rechten Maustaste dieses Aktualisierungsbereich, die unabhängig von oberen Zeit der unteren Zeit aktualisiert. Das Datum wurde auch zwischen den Klicks, deaktiviert, wie das Datum in die untere Bezeichnung angezeigt wird. Schließlich von Interesse ist die untere Bezeichnung die Farbe: aktueller als der Bezeichnungstext, der zeigt, dass Steuerelementzustand wichtig ist, aktualisiert wurde und die Benutzer erwarten, dass sie über AJAX-Postbacks beibehalten werden. *Jedoch*, die Zeit wurde nicht aktualisiert. Die Zeit wurde automatisch durch die Persistenz erneut aufgefüllt. die \_ \_VIEWSTATE-Feld der Seite von der ASP.NET-Laufzeit interpretiert wird, wenn das Steuerelement auf dem Server erneut gerendert wurde. Der ASP.NET AJAX-Servercode erkennt nicht die Methoden der Steuerelemente in dem Zustand ändern; Es füllt einfach aus dem Ansichtszustand und führt dann die Ereignisse, die geeignet sind.

Es sollte verwiesen, jedoch, dass ich die Zeit auf der Seite initialisiert hatte\_Load-Ereignis, die Zeit wurde würde ordnungsgemäß inkrementiert. Folglich Entwickler sollte vor, dass der entsprechende Code bei der die entsprechenden Ereignishandler ausgeführt wird und vermeiden Sie die Verwendung der Seite\_laden, wenn ein Handler für Steuerelementereignisse geeignet wären.

## <a name="summary"></a>Zusammenfassung

Das UpdatePanel von ASP.NET AJAX Extensions-Steuerelement ist vielseitig und kann eine Reihe von Methoden für die Identifizierung von Ereignissen, die sie aktualisiert werden, verursachen sollen verwendet werden. Unterstützt durch seine untergeordneten Steuerelemente automatisch aktualisiert wird, sondern können auch auf Steuerelementereignisse an anderer Stelle auf der Seite reagieren.

Um das Potenzial für die Verarbeitung der Serverlast zu verringern, es wird empfohlen, die `ChildrenAsTriggers` Eigenschaft von einem UpdatePanel-Steuerelement festgelegt werden, um `false`, und dass Ereignisse abonniert-in nicht standardmäßig enthalten sein. Dies wird auch verhindert, dass nicht benötigte Ereignisse verursacht potenziell unerwünschte Effekte, einschließlich Überprüfung und Änderungen in Eingabefeldern. Diese Arten von Fehlern können schwierig sein, zu isolieren, da die Seite wird für den Benutzer transparent aktualisiert, und die Ursache daher möglicherweise nicht sofort ersichtlich.

Posten anhand der internen Funktionsweise des ASP.NET AJAX-Formulars Interception-Modell, konnten wir feststellen, dass es sich um das Framework, die bereits von ASP.NET bereitgestellt wird. In diesem Fall es behält maximalen Kompatibilität mit Steuerelementen, die mithilfe desselben Frameworks entworfen und eingreift, nur minimal auf alle weiteren JavaScript-Code für die Seite geschrieben.

## <a name="bio"></a>Biografie

Rob Paveza ist senior .NET Application Entwickler Terralever ([www.terralever.com](http://www.terralever.com)), ein führender interaktive Marketingunternehmen in Tempe, AZ. Er ist unter [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), und sein Blog befindet sich im [ http://geekswithblogs.net/robp/ ](http://geekswithblogs.net/robp/).

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und ist Vorsitzender der myKB.com ([www.myKB.com](http://www.myKB.com)), in dem er spezialisiert sich auf das Schreiben von ASP.NET basierende Anwendungen, die mit dem Schwerpunkt Knowledge Base-softwarelösungen. Scott hergestellt werden kann, per e-Mail unter [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Zurück](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Weiter](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
