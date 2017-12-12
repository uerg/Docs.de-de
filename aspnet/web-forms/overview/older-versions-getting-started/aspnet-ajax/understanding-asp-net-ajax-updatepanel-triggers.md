---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Grundlegendes zu ASP.NET AJAX UpdatePanel Triggern | Microsoft Docs
author: scottcate
description: "Bei der Arbeit im Markup-Editor in Visual Studio können Sie (von IntelliSense) Beachten Sie, dass zwei untergeordnete Elemente eines Steuerelements UpdatePanel vorhanden sind. Einer der w-fragewörter..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2008
ms.topic: article
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: 1338ef0763d9bfab451bc30cafa39f715200153d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Grundlegendes zu ASP.NET AJAX UpdatePanel-Triggern
====================
durch [Scott Kate](https://github.com/scottcate)

[PDF herunterladen](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Bei der Arbeit im Markup-Editor in Visual Studio können Sie (von IntelliSense) Beachten Sie, dass zwei untergeordnete Elemente eines Steuerelements UpdatePanel vorhanden sind. Von denen der Trigger Element ist, der angibt, die Steuerelemente auf der Seite "(oder das Benutzersteuerelement, wenn Sie eine verwenden), der auslöst teilweise Rendern des Steuerelements UpdatePanel in der sich das Element befindet.


## <a name="introduction"></a>Einführung

Microsoft Technologie für ASP.NET bietet ein Programmiermodell objektorientierte und ereignisgesteuerte und ihn mit den Vorteilen des kompilierten Codes vereinigt. Die serverseitige Verarbeitungsmodell hat jedoch mehrere Nachteile inhärenten in die Technologie, von die viele mit der neuen Features in den Microsoft ASP.NET 3.5 AJAX-Erweiterungen adressiert werden können. Diese Erweiterungen ermöglichen viele neue rich-Client-Funktionen, einschließlich Teilrendering von Seiten ohne eine vollständige Seite-Aktualisierung, den Zugriff auf Webdienste über Clientskripts (einschließlich ASP.NET profilerstellungs-API) und eine umfangreiche clientseitige API entwickelt, um viele die Steuerelement-Schemas in der Menge der ASP.NET serverseitiges Steuerelement sichtbar zu spiegeln.

In diesem Whitepaper überprüft die XML-Trigger-Funktionalität von ASP.NET AJAX `UpdatePanel` Komponente. XML-Trigger bieten eine präzise Kontrolle über die Komponenten, die für bestimmte UpdatePanel Steuerelemente Teilrendering verursachen kann.

In diesem Whitepaper basiert auf der Beta 2-Version von .NET Framework 3.5 und Visual Studio 2008. Die ASP.NET AJAX-Erweiterungen, zuvor ein Add-on-Assembly, die auf ASP.NET 2.0 ausgerichtet sind jetzt in der .NET Framework-Basisklassenbibliothek integriert. In diesem Whitepaper wird außerdem davon ausgegangen, dass Sie mit Visual Studio 2008 nicht Visual Web Developer Express arbeiten und gebe (obwohl die codeauflistungen vollständig kompatibel ist, unabhängig von sind exemplarische Vorgehensweisen gemäß der Benutzeroberfläche von Visual Studio Entwicklungsumgebung).

## <a name="triggers"></a>*Trigger*

Trigger für eine bestimmte UpdatePanel, wird standardmäßig automatisch berücksichtigt alle untergeordneten Steuerelemente, die ein Postback handeln, einschließlich der TextBox-Steuerelemente haben (z. B.) aufrufen ihre `AutoPostBack` -Eigenschaftensatz auf **"true"**. Trigger können jedoch auch deklarativ mit Markup enthalten sein. Dies erfolgt in der `<triggers>` Teil der Deklaration der UpdatePanel-Steuerelement. Obwohl Trigger über zugegriffen werden können die `Triggers` Auflistungseigenschaft, es wird empfohlen, dass Sie keine teilweise Rendern Trigger zur Laufzeit registrieren (z. B. wenn ein Steuerelement zur Entwurfszeit nicht verfügbar ist) mithilfe der `RegisterAsyncPostBackControl(Control)` Methode der ScriptManager-Objekt für die Seite in der `Page_Load` Ereignis. Denken Sie daran, dass Seiten zustandslos sind und daher sollte erneut registrieren dieser Steuerelemente jedes Mal, wenn sie erstellt werden.

Automatische untergeordneten Trigger Aufnahme kann auch deaktiviert werden (so, dass untergeordnete Steuerelemente, die Postbacks erstellen partielle gerendert wird nicht automatisch auslöst) durch Festlegen der `ChildrenAsTriggers` Eigenschaft **"false"**. Dies gibt Ihnen die größte Flexibilität in welche bestimmten Steuerelementen Zuweisen einer Seite rendern kann aufrufen und empfiehlt es sich, so, dass ein Entwickler opt-in wird zum Reagieren auf ein Ereignis, anstatt die Behandlung der Ereignisse, die auftreten können.

Beachten Sie, dass wenn UpdatePanel Steuerelemente geschachtelt sind, wenn die UpdateMode festgelegt ist, **bedingte**, wenn das untergeordnete Element UpdatePanel wird ausgelöst, aber das übergeordnete Element ist nicht der Fall, klicken Sie dann nur die untergeordneten UpdatePanel wird aktualisiert. Allerdings ist das übergeordnete Element UpdatePanel aktualisiert, werden klicken Sie dann das untergeordnete Element UpdatePanel auch aktualisiert.

## <a name="the-lttriggersgt-element"></a>*Die &lt;Trigger&gt; Element*

Bei der Arbeit im Markup-Editor in Visual Studio Sie möglicherweise feststellen (von IntelliSense), es zwei untergeordnete Elemente sind von einem `UpdatePanel` Steuerelement. Die am häufigsten verwendeten betrachtete Element ist das `<ContentTemplate>` -Element, das im Wesentlichen den Inhalt kapselt, die von der Update-Panels gespeichert werden (der Inhalt für das werden wir Teilrendering aktivieren). Das andere Element ist die `<Triggers>` Element, das die Steuerelemente auf der Seite "(oder das Benutzersteuerelement, wenn Sie eine verwenden) angibt, der teilweise Rendern des Steuerelements in der UpdatePanel auslöst der &lt;Trigger&gt; Element befindet.

Die `<Triggers>` Element kann eine beliebige Anzahl jeder von zwei untergeordnete Knoten enthalten: `<asp:AsyncPostBackTrigger>` und `<asp:PostBackTrigger>`. Beide akzeptiert zwei Attribute `ControlID` und `EventName`, und jedes Steuerelement in der aktuellen Einheit für die Kapselung angeben (z. B. wenn das UpdatePanel-Steuerelement in einem Web-Benutzersteuerelement befindet, Sie sollten nicht versuchen, auf ein Steuerelement verweisen die Seite, auf der sich das Benutzersteuerelement befinden).

Die `<asp:AsyncPostBackTrigger>` Element ist besonders nützlich, insofern, dass jedes Ereignis von einem Steuerelement abzielen können, das als untergeordnetes Element vorhanden ist *alle* UpdatePanel-Steuerelement in der Maßeinheit für die Kapselung, nicht nur die UpdatePanel, unter dem dieser Trigger ein untergeordnetes Element ist, . Daher kann jedes Steuerelement vorgenommen werden, um ein Teilseite Update auszulösen.

Auf ähnliche Weise die `<asp:PostBackTrigger>` -Element Trigger, die eine partielle Seite gerendert, aber einen vollständige Roundtrip zum Server erfordert eine verwendet werden kann. Dieses Element Trigger kann auch verwendet werden, eine vollständige Seite rendern zu erzwingen, wenn ein Steuerelement rendern Teilseite andernfalls normalerweise auslösen würde (z. B. wenn ein `Button` Steuerelement vorhanden ist, der `<ContentTemplate>` Element eines Steuerelements UpdatePanel). Das PostBackTrigger-Element kann erneut, jedes Steuerelement angeben, das ein untergeordnetes Element eines beliebigen UpdatePanel-Steuerelements in der aktuellen Einheit für die Kapselung ist.

## <a name="lttriggersgt-element-reference"></a>*&lt;Trigger&gt; -Elementverweis*

*Markup Nachfolger:*

| **Tag** | **Beschreibung** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Gibt ein Steuerelement und ein Ereignis, das ein Teilseite Update UpdatePanel verursachen kann, die diese Trigger Verweis enthält. |
| &lt;ASP PostBackTrigger:&gt; | Gibt ein Steuerelement und ein Ereignis, das bewirkt eine vollständige Seite-Aktualisierung (eine vollständige Seite-Aktualisierung). Eine vollständige Aktualisierung zu erzwingen, wenn ein Steuerelement andernfalls Teilrendering auslöst, kann dieses Tag verwendet werden. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Exemplarische Vorgehensweise: Cross-UpdatePanel-Triggern*

1. Erstellen Sie eine neue ASP.NET-Seite mit einem ScriptManager-Objekt, das Teilrendering aktiviert festgelegt. Diese Seite – in der ersten beiden UpdatePanels hinzugefügt haben, gehören ein Bezeichnungsfeld-Steuerelement (Label1) und zwei Button-Steuerelementen (Button1 und Button2). Button1 sage klicken Sie hier, um beide zu aktualisieren und Button2 sage klicken Sie hier, um dies oder etwas Ähnliches zu aktualisieren. In der zweiten UpdatePanel nur ein Bezeichnungsfeld-Steuerelement (Label2) enthalten, aber durch Festlegen der ForeColor-Eigenschaft auf einen anderen Wert als den Standardwert, um diese zu unterscheiden.
2. Legen Sie die Eigenschaft UpdateMode beider UpdatePanel-Tags auf **bedingte**.

**Auflisten von 1: Markup für "default.aspx":** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. Angegeben Sie in dem Click-Ereignishandler für Button1 Label1.Text und Label2.Text etwas anderes zeitabhängigen (z. B. DateTime.Now.ToLongTimeString()). Legen Sie für das Click-Ereignishandler für Button2 nur Label1.Text auf der zeitabhängige-Wert.

**Auflisten von 2: Codebehind (gekürzt) in "default.aspx.cs":** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Drücken Sie F5, um das Projekt erstellen und ausführen. Beachten Sie, dass beide Bezeichnungen Text, wenn Sie sowohl Bereiche von Update klicken ändern, Wenn Sie dieses Update-Panels klicken, wird jedoch nur Label1 aktualisiert.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))


## <a name="under-the-hood"></a>*Hinter den Kulissen*

Nutzen das Beispiel, das wir soeben erstellt haben, können wir sehen Sie sich ausgeführten ASP.NET AJAX und zu deren Funktionsweise Trigger unsere UpdatePanel-Cross-Bereich. Voraussetzung hierfür ist, arbeiten wir mit der generierten Seitenquelle HTML als auch die Mozilla Firefox-Erweiterung namens FireBug - mithilfe dieser Option können wir die AJAX-Postbacks leichter überprüfen. Wir verwenden auch das Tool .NET Reflector von Lutz Roeder. Beide Tools sind online kostenlos verfügbar und können mit einem Internetsuche gefunden werden.

Eine Prüfung des Quellcodes Seite zeigt fast nichts Ungewöhnliches. UpdatePanel-Steuerelemente werden als gerendert `<div>` Container, und wir können finden Sie unter der Ressource "Script" enthält, sofern von der `<asp:ScriptManager>`. Es gibt auch einige neue AJAX-spezifischen Aufrufe PageRequestManager, die für die AJAX-Skript-Clientbibliothek intern sind. Abschließend sehen Sie die beiden UpdatePanel-Container – eine mit das gerenderte `<input>` Schaltflächen mit den beiden `<asp:Label>` Steuerelemente gerendert wird, als `<span>` Container. (Wenn Sie die DOM-Struktur in FireBug untersuchen, bemerken Sie, dass die Bezeichnungen abgeblendet sind, um anzugeben, dass sie nicht sichtbaren Inhalt Standardstartseite).

Klicken Sie auf die Schaltfläche "dieser Update-Panels", und beachten Sie, dass die obere UpdatePanel mit dem aktuellen Server aktualisiert werden. Wählen Sie in FireBug der Registerkarte "Console", sodass Sie, die Anforderung prüfen können. Überprüfen Sie zunächst die POST-Anforderungsparameter:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))


Beachten Sie, dass die UpdatePanel an den serverseitigen AJAX-Code angegeben hat, genau die Struktur des Steuerelements über den Parameter ScriptManager1 ausgelöst wurde: `Button1` von der `UpdatePanel1` Steuerelement. Klicken Sie nun auf die Schaltfläche Aktualisieren beide Bereiche auf. Anschließend sehen untersuchen die Antwort, wir eine senkrechte Striche getrennte Reihe von Variablen in einer Zeichenfolge festgelegt; insbesondere wir sehen, dass die obere UpdatePanel `UpdatePanel1`, vollständig die HTML-Dateien an den Browser gesendet wurde. Die AJAX-Skript-Clientbibliothek ersetzt die UpdatePanel ursprünglichen HTML-Inhalt mit dem neuen Inhalt über die `.innerHTML` -Eigenschaft, und damit der Server mit den geänderten Inhalt vom Server als HTML sendet.

Jetzt, klicken Sie auf die Schaltfläche "Aktualisieren beide Bereiche", und überprüfen Sie die Ergebnisse vom Server. Die Ergebnisse sehr ähneln – beide UpdatePanels neuem HTML-Code vom Server empfangen. Wie bei der vorherigen Rückruf wird zusätzlich eine Berichtsseite Zustand gesendet.

Wie wir sehen können, da keine spezieller Code verwendet wird, um ein AJAX-Postback auszuführen, kann die AJAX-Skript-Clientbibliothek Formular Postbacks ohne zusätzlichen Code abzufangen. Serversteuerelemente nutzen automatisch JavaScript, damit sie das Formular nicht automatisch gesendet – ASP.NET automatisch Code für die formularvalidierung und Status noch in erster Linie durch automatische Skript Ressource einschließen, die Klasse PostBackOptions erreicht fügt , und der ClientScriptManager-Klasse.

Betrachten Sie beispielsweise ein CheckBox-Steuerelement aus; Überprüfen Sie die Klasse Disassembly in .NET Reflector. Zu diesem Zweck stellen Sie sicher, dass die Assembly System.Web geöffnet ist, und navigieren Sie zu der `System.Web.UI.WebControls.CheckBox` -Klasse, öffnen die `RenderInputTag` Methode. Suchen Sie nach einer Bedingung, die überprüft die `AutoPostBack` Eigenschaft:


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))


Wenn Automatisches Postback aktiviert ist, auf eine `CheckBox` steuern (über die AutoPostBack-Eigenschaft wird "true"), die resultierenden `<input>` Tag wird daher mit einem Behandlung von Skripts in ASP.NET-Ereignis gerendert seine `onclick` Attribut. Das Abfangen der Übermittlung des Formulars, klicken Sie dann ermöglicht ASP.NET AJAX nonintrusively, in die Seite eingefügt werden und helfen, wichtige Änderungen, die auftreten können, durch die Verwendung einer möglicherweise ungenaue zeichenfolgenersetzungen mögliche vermeiden. Darüber hinaus kann diese *alle* benutzerdefinierte ASP.NET-Steuerelements nutzen die Leistungsfähigkeit von ASP.NET AJAX ohne zusätzlichen Code, der seine Verwendung innerhalb eines Containers UpdatePanel unterstützen können.

Die `<triggers>` Funktionen entsprechen den Werten, die im Aufruf der PageRequestManager initialisiert \_UpdateControls (Beachten Sie, dass die ASP.NET AJAX-Skript-Clientbibliothek der Konvention nutzt, Methoden, Ereignisse und Feldnamen, beginnen sind mit einem Unterstrich als intern markiert und sind nicht für die Verwendung außerhalb der Bibliothek selbst vorgesehen). Mithilfe dieser Option können wir beobachten, welche Steuerelemente zur AJAX Postbacks verursachen vorgesehen sind.

Beispielsweise fügen Sie zwei zusätzliche Steuerelemente auf der Seite ein Steuerelement außerhalb der UpdatePanels vollständig zu verlassen, und eine innerhalb eines UpdatePanel verlassen. Wir fügen Sie ein CheckBox-Steuerelement innerhalb der Obergrenze UpdatePanel hinzu, und löschen eine DropDownList mit einer Anzahl von Farben in der Liste definiert. Hier wird das neue Markup:

**Auflisten von 3: Neue Markup**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Und hier ist der neue Code-Behind:

**Auflisten von 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

Das Konzept hinter dieser Seite ist, dass die Dropdown Liste einen der drei Farben wählt auf die zweite Bezeichnung angezeigt, dass das Kontrollkästchen legt fest, gibt an, ob es fett formatiert ist, und gibt an, ob die Bezeichnungen für das Datum sowie der Zeit anzuzeigen. Das Kontrollkästchen sollte nicht dazu, dass ein AJAX-Update, aber die Dropdown Liste sollten, obwohl er nicht innerhalb eines UpdatePanel untergebracht ist.


[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))


Wie im obigen Screenshot offensichtlich ist, wurde die neuesten Schaltfläche geklickt wird mit der rechten Maustaste dieser Update-Panels, die unabhängig von der Top Zeit der unteren Zeit aktualisiert. Das Datum wurde auch zwischen Klicks, deaktiviert, da das Datum in der unteren Bezeichnung angezeigt werden. Schließlich von Interesse sind Farbe des unteren Bezeichnungsfelds: Softwareupdateliste Text der Beschriftung, die veranschaulicht, dass Steuerelementzustand wichtig ist, aktualisiert wurde, und Benutzer erwarten, dass er über AJAX-Postbacks beibehalten werden. *Jedoch*, die Zeit wurde nicht aktualisiert. Die Zeit wurde automatisch erneut aufgefüllt, durch die Persistenz der \_ \_"ViewState" Speichern-Feld der Seite durch die ASP.NET-Laufzeit interpretiert werden, wenn das Steuerelement auf dem Server erneut gerendert wurde. Die ASP.NET AJAX-Servercode erkennt nicht, in dem Zustand Methoden der Steuerelemente ändern, sind; es einfach aus dem Ansichtszustand füllt und führt dann die Ereignisse, die geeignet sind.

Es sollten werden darauf hingewiesen, allerdings, dass ich die Zeit auf der Seite initialisiert war\_Load-Ereignis, die Zeit wurde würde ordnungsgemäß inkrementiert. Folglich Entwickler sollte vorsichtig, dass der entsprechende Code, während die entsprechenden Ereignishandler ausgeführt wird und vermeiden Sie die Verwendung der Seite "\_geladen, wenn ein Steuerelement Ereignishandler geeignet ist.

## <a name="summary"></a>Zusammenfassung

Das ASP.NET AJAX Extensions UpdatePanel-Steuerelement ist vielseitig und eine Reihe von Methoden zum Identifizieren von Ereignissen, die zu aktualisierenden führen sollten nutzen kann. Unterstützt von untergeordneten Steuerelemente automatisch aktualisiert, aber auch auf Steuerelementereignisse an anderer Stelle auf der Seite reagieren kann.

Um potenzielle Server Verarbeitungslast zu reduzieren, wird empfohlen, die `ChildrenAsTriggers` UpdatePanel-Eigenschaft festgelegt, um `false`, und diese Ereignisse Filtertreiber-in nicht standardmäßig enthalten sein. Dies wird auch verhindert, dass nicht benötigte Ereignisse verursacht potenziell unerwünschte Auswirkungen, einschließlich Überprüfung und Änderungen für die Eingabe von Feldern. Diese Typen von Fehlern ist möglicherweise schwierig zu isolieren, da die Seite wird für den Benutzer transparent aktualisiert und die Ursache daher möglicherweise nicht sofort bemerkbar.

Durch Untersuchen der internen Funktionsweise des ASP.NET AJAX-Formulars post Modell abfangen, konnten wir einen feststellen, dass es das Framework, die bereits von ASP.NET bereitgestellt wird. In diesem Fall sie behält maximalen Kompatibilität mit Steuerelementen, die mithilfe des gleichen Frameworks entworfen und eingreift minimal auf die zusätzlichen JavaScript geschrieben wird, für die Seite.

## <a name="bio"></a>Lebenslauf

Rob Paveza ist senior .NET Anwendungsentwickler bei Terralever ([www.terralever.com](http://www.terralever.com)), eine führende interaktive marketing hingegen in Tempe, AZ. Er die erreicht werden kann, zur [ robpaveza@gmail.com ](mailto:robpaveza@gmail.com), und seinen Blog finden Sie unter [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate arbeitet mit Microsoft-Web-Technologien seit 1997 und Präsidenten des myKB.com ist ([www.myKB.com](http://www.myKB.com)), in dem er zum Schreiben von ASP.NET spezialisiert-basierten Anwendungen, die Wissensdatenbank softwarelösungen konzentriert. Scott hergestellt werden kann, per e-Mail an [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) oder seinen Blog unter [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Zurück](understanding-partial-page-updates-with-asp-net-ajax.md)
[Weiter](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
