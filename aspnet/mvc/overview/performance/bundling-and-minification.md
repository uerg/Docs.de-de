---
uid: mvc/overview/performance/bundling-and-minification
title: Bündelung und Minimierung | Microsoft-Dokumentation
author: Rick-Anderson
description: Bündelung und Minimierung sind zwei Techniken können Sie in ASP.NET 4.5 zur Verbesserung der Ladezeit der Anforderung. Bündelung und Minimierung verbessert die Ladezeit von Reducin...
ms.author: aspnetcontent
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 4e72804593c07318af8cc577f9d43ab96be4de05
ms.sourcegitcommit: cb0c27fa0184f954fce591d417e6ab2a51d8bb22
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/18/2018
ms.locfileid: "39123787"
---
<a name="bundling-and-minification"></a>Bündelung und Minimierung
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> Bündelung und Minimierung sind zwei Techniken können Sie in ASP.NET 4.5 zur Verbesserung der Ladezeit der Anforderung. Bündelung und Minimierung verbessert die Ladezeit von reduziert die Anzahl der Anforderungen an den Server und Verringern der Größe des angeforderten Assets (z. B. CSS- und JavaScript.)


Die meisten aktuellen wichtigen Browser beschränken die Anzahl der [gleichzeitige Verbindungen](http://www.browserscope.org/?category=network) pro jeden Hostnamen auf 6. Das heißt, während die sechs Anforderungen verarbeitet werden, weitere Anforderungen für Ressourcen auf einem Host vom Browser in Warteschlangen gestellt werden. In der folgenden Abbildung den Internet Explorer F12 Developer Tools Netzwerk Registerkarten angezeigt wird die zeitliche Steuerung von der Ansicht "Info", der eine Beispiel-App benötigt Ressourcen.

![B/M](bundling-and-minification/_static/image1.png)

Die grauen Balken zeigen die Zeit, die die Anforderung vom Browser warten auf den Grenzwert für sechs Verbindungen in der Warteschlange befindet. Der gelbe Balken wird die Anforderungszeit zum ersten Byte, also die Zeit zum Senden der Anfrage, und die erste Antwort vom Server empfangen. Die blauen Balken zeigen die Zeit, um die Antwortdaten vom Server empfangen. Sie können auf einer Ressource erhalten ausführliche Zeitsteuerungsinformationen doppelklicken. Die folgende Abbildung zeigt beispielsweise die Details der zeitlichen Steuerung für das Laden der */Scripts/MyScripts/JavaScript6.js* Datei.

![](bundling-and-minification/_static/image2.png)

Die vorherige Abbildung zeigt die **starten** -Ereignis, dadurch erhalten die Zeit, die die Anforderung wurde aufgrund des Browser in der Warteschlange begrenzen Sie die Anzahl von gleichzeitigen Verbindungen. In diesem Fall wurde die Anforderung für 46 Millisekunden warten auf eine andere Anforderung zum Abschließen in die Warteschlange eingereiht.

## <a name="bundling"></a>Bündeln

Bündelung ist ein neues Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei gebündelt. Sie können CSS, JavaScript und anderen Paketen erstellen. Weniger Dateien bedeutet, dass weniger HTTP-Anforderungen und die erste ladeleistung für Seiten verbessern können.

Die folgende Abbildung zeigt die gleiche Ansicht für zeitliche Steuerung der Ansicht "Info" angezeigt wird, zuvor, aber dieses Mal mit Bündelung und Minimierung aktiviert.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimierung

Minimierung führt eine Vielzahl von verschiedenen codeoptimierungen, Skripts oder Css, beispielsweise das Entfernen unnötiger Leerzeichen und Kommentare und verkürzen Sie die Variablennamen, um ein Zeichen. Betrachten Sie die folgende JavaScript-Funktion.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Nach Minimierung wird die Funktion folgt verringert:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Zusätzlich zu entfernen, die Kommentare und unnötiger Leerraum, wurden die folgenden Parameter und die Namen von Variablen (gekürzt) folgendermaßen umbenannt:

| **Original** | **Umbenannt** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Auswirkungen der Bündelung und Minimierung

Die folgende Tabelle zeigt einige wichtige Unterschiede zwischen alle Objekte einzeln aufgelistet, und Verwenden von Bündelung und Minimierung (B/M) im Beispielprogramm.

|  | **Mithilfe von B/Min.** | **Ohne B/Min.** | **Änderung** |
| --- | --- | --- | --- |
| **Dateianforderungen** | 9 | 34 | 256% |
| **Gesendete KB** | 3.26 | 11.92 | 266% |
| **KB empfangen** | 388.51 | 530 | 36% |
| **Ladezeit** | 510 MS | 780 MS | 53% |

Die Bytes gesendet hatte, zu eine erhebliche Reduzierung mit bündeln wie Browser Recht ausführlich mit den HTTP-Headern, die sie für Anforderungen gelten. Die Verringerung der empfangenen Bytes nicht so groß ist. da die größten Dateien (*Skripts\\Jquery-Benutzeroberfläche – 1.8.11.min.js* und *Skripts\\Jquery-1.7.1.min.js*) sind bereits minimiert . Hinweis: Die Intervalle für das Beispielprogramm verwendet die [Fiddler](http://www.fiddler2.com/fiddler2/) Tool, um ein langsames Netzwerk zu simulieren. (Von der Fiddler **Regeln** , wählen Sie im Menü **Leistung** dann **simulieren Modem Geschwindigkeit**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debuggen von gebündelten und minimierten JavaScript

Es ist einfach, Ihr JavaScript in einer Entwicklungsumgebung Debuggen (, in denen die [Compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in die *"Web.config"* Datei nastaven NA hodnotu `debug="true"` ), da nicht die JavaScript-Dateien gebündelt werden oder minimiert. Sie können auch einen Releasebuild debuggen, in dem die JavaScript-Dateien von gebündelten und minimierten sind. Verwenden Internet Explorer F12-Entwicklertools, Debuggen Sie eine JavaScript-Funktion, die in einem minimierten Bündel mit dem folgenden Ansatz enthalten:

1. Wählen Sie die **Skript** Registerkarte, und wählen Sie dann die **Debuggen** Schaltfläche.
2. Wählen Sie das Paket mit der JavaScript-Funktion, die Sie Debuggen, verwenden die Schaltfläche "Assets" möchten.  
    ![](bundling-and-minification/_static/image4.png)
3. Formatieren von das minimierte JavaScript durch Auswählen der **Schaltfläche Konfiguration** ![](bundling-and-minification/_static/image5.png), und wählen Sie dann **Format JavaScript**.
4. In der **Suche Skript** Eingabefeld, wählen Sie den Namen der Funktion, die Sie debuggen möchten. In der folgenden Abbildung **AddAltToImg** eingegeben wurde die **Suche Skript** Eingabefeld.  
    ![](bundling-and-minification/_static/image6.png)

Weitere Informationen zum Debuggen mit den F12-Entwicklertools finden Sie im MSDN-Artikel [Verwendung F12-Entwicklertools zum Debuggen von JavaScript-Fehler](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controlling Bündelung und Minimierung

Bündelung und Minimierung aktiviert oder deaktiviert werden, indem der Wert des Attributs "Debuggen" in der [Compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in die *"Web.config"* Datei. Im folgenden XML `debug` Bündelung von diesem "Wahr" festgelegt ist und die Minimierung ist deaktiviert.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Legen Sie zum Aktivieren von Bündelung und Minimierung der `debug` Wert auf "False". Können Sie überschreiben die *"Web.config"* Einstellung mit der `EnableOptimizations` Eigenschaft für die `BundleTable` Klasse. Der folgende Code ermöglicht, Bündelung und Minimierung und überschreibt alle Einstellungen in der *"Web.config"* Datei.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Es sei denn, `EnableOptimizations` ist `true` oder das Debug-Attribut in der [Compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in die *"Web.config"* Datei festgelegt ist `false`, Dateien nicht gebündelt oder minimiert werden. Darüber hinaus die .min-Version der Dateien wird nicht verwendet werden, werden die vollständige Debugversionen ausgewählt werden. `EnableOptimizations` überschreibt das Debug-Attribut in der [Compilation-Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in die *"Web.config"* Datei


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Verwenden die Bündelung und Minimierung mit ASP.NET-Web Forms und Web Pages

- Für Webseiten, finden Sie im Blogeintrag [Weboptimierung hinzufügen, um eine Webseiten-Website](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Web Forms finden Sie im Blogeintrag [hinzufügen Bündelung und Minimierung zu Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Verwenden die Bündelung und Minimierung mit ASP.NET MVC

In diesem Abschnitt werden wir einer ASP.NET MVC-Projekt untersuchen Bündelung und Minimierung erstellen. Erstellen Sie zunächst ein neues ASP.NET MVC-Internet-Projekt namens **MvcBM** ohne Standardeinstellungen ändern zu müssen.

Öffnen der *App\\\_starten\\BundleConfig.cs* Datei, und sehen die `RegisterBundles` Methode dient zum Erstellen, registrieren und Konfigurieren von Paketen. Der folgende Code zeigt einen Teil der `RegisterBundles` Methode.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Der vorangehende Code erstellt ein neues JavaScript-Paket mit dem Namen *~/bundles/jquery* , enthält das entsprechende (, jedoch nicht verkleinert oder wird Sie debuggen. *Vsdoc*) Dateien in die *Skripts* Ordner, der die Platzhalter-Zeichenfolge "~/Scripts/jquery-{Version} .js" entsprechen. Für ASP.NET MVC 4, also mit einer Debugkonfiguration, die Datei *Jquery-1.7.1.js* , die dem Paket hinzugefügt. In einer Releasekonfiguration *Jquery-1.7.1.min.js* hinzugefügt werden. Die Bündelung Framework folgt mehrere allgemeine Konventionen wie z.B.:

- Auswählen von ".min"-Datei für Version, wenn *FileX.min.js* und *FileX.js* vorhanden sind.
- Wählen die Version nicht ".min" zum Debuggen.
- Wird ignoriert. "-Vsdoc" Dateien (z. B. *Jquery-1.7.1-Beispiel vsdoc.js*), die nur vom IntelliSense verwendet wird.

Die `{version}` Platzhalter übereinstimmende oben wird verwendet, um automatisch ein jQuery-Paket erstellen Sie mit der entsprechenden Version von jQuery in Ihre *Skripts* Ordner. In diesem Beispiel enthält einen Platzhalter die folgenden Vorteile:

- Können Sie NuGet verwenden, um auf eine neuere Version von jQuery zu aktualisieren, ohne Änderung der vorangehenden Bündelung Code oder die jQuery-Verweise in Ihren Seiten anzeigen.
- Wählt automatisch erstellt, die vollständige Version für Debugkonfigurationen und die Version ".min" für Version.

## <a name="using-a-cdn"></a>Verwenden eines CDN

 Der folgende Code ersetzt die lokalen jQuery-Pakets mit einem CDN-jQuery-Paket.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Im obigen Code wird aus dem CDN jQuery angefordert werden, während im Release-Modus und die Debugversion von jQuery lokal im Debugmodus abgerufen werden. Wenn Sie ein CDN verwenden zu können, benötigen Sie einen Fallbackmechanismus, für den Fall, dass der CDN-Anforderung ein Fehler auftritt. Das folgende Markup fragment am Ende Layout zeigt das Skript für die hinzugefügt werden, um anzufordern, dass jQuery der CDN-Fehler sollten.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Erstellen eines Pakets

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `Include` Methode akzeptiert ein Array von Zeichenfolgen, wobei jede Zeichenfolge ein virtueller Pfad zu der Ressource ist. Der folgende code aus der `RegisterBundles` -Methode in der die *App\\\_starten\\BundleConfig.cs* Datei zeigt, wie mehrere Dateien auf ein Paket hinzugefügt werden:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `IncludeDirectory` Methode wird bereitgestellt, um alle Dateien in einem Verzeichnis (und optional alle Unterverzeichnisse) zu hinzuzufügen, die einem Suchmuster entsprechen. Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `IncludeDirectory` API wird im folgenden dargestellt:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pakete werden auf die verwiesen wird in Sichten, die mithilfe der Render-Methode (`Styles.Render` für CSS und `Scripts.Render` für JavaScript). Das folgende Markup aus der *Ansichten\\Shared\\\_Layout.cshtml* -Datei veranschaulicht, wie die standardmäßige ASP.NET Internet Projektansichten CSS- und JavaScript-Pakete verweisen.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Beachten Sie, dass die Render-Methoden akzeptiert ein Array von Zeichenfolgen, sodass Sie mehrere Pakete in einer einzigen Codezeile hinzufügen können. Im Allgemeinen sollten Sie die Render-Methoden verwenden, die den erforderlichen HTML-Code verweisen, auf das Medienobjekt erstellen. Sie können die `Url` Methode zum Generieren von der URL für das Medienobjekt ohne das Markup erforderlich, um das Objekt zu verweisen. Angenommen Sie möchten, verwenden Sie das neue HTML5 [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) Attribut. Der folgende Code zeigt, wie Sie mithilfe von Modernizr verweisen die `Url` Methode.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Mithilfe der "\*" Platzhalterzeichen, um die Dateien auswählen

Die im angegebenen virtuellen Pfad der `Include` -Methode, und suchen Sie die Muster in den `IncludeDirectory` Methode akzeptiert eine "\*" Platzhalterzeichen als Präfix oder Suffix in das letzte Pfadsegment. Die Suchzeichenfolge ist Groß-/Kleinschreibung. Die `IncludeDirectory` Methode verfügt über die Option Unterverzeichnisse zu durchsuchen.

Angenommen Sie, ein Projekt mit den folgenden JavaScript-Dateien:

- *Skripts\\allgemeine\\AddAltToImg.js*
- *Skripts\\allgemeine\\ToggleDiv.js*
- *Skripts\\allgemeine\\ToggleImg.js*
- *Skripts\\allgemeine\\Sub1\\ToggleLinks.js*

![Dir imag](bundling-and-minification/_static/image7.png)

Die folgende Tabelle zeigt die Dateien, die auf ein Paket mithilfe des Platzhalterzeichens, siehe hinzugefügt:

| **Call** | **Dateien, die hinzugefügt oder eine Ausnahme ausgelöst** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Ausnahme für ungültiges Muster. Das Platzhalterzeichen ist nur für das Präfix oder Suffix zulässig. |
| Include("~/Scripts/Common/\*og.\*") | Ausnahme für ungültiges Muster. Nur ein Platzhalterzeichen ist zulässig. |
| Enthalten ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Enthalten ("~/Scripts/Common/\*") | Ausnahme für ungültiges Muster. Ein reines Platzhaltersegment ist ungültig. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*" "true") | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Hinzufügen von jeder Datei explizit auf ein Paket ist in der Regel die bevorzugte über Platzhalter-Laden von Dateien aus den folgenden Gründen:

- Hinzufügen von Skripts standardmäßig Platzhalter zum Laden diese in alphabetischer Reihenfolge, in der Regel nicht gewünscht wird. CSS und JavaScript-Dateien müssen sich häufig in einer bestimmten Reihenfolge für die (nicht-alphabetische) hinzugefügt werden. Sie können dieses Risiko verringern, indem das Hinzufügen eines benutzerdefinierten ["ibundleorderer"](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) Implementierung, aber das explizite Hinzufügen von einzelnen Dateien ist weniger fehleranfällig. Sie können z. B. neuen Assets in einem Ordner in der Zukunft, die Sie ändern, möglicherweise hinzufügen Ihrer ["ibundleorderer"](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) Implementierung.
- Ansicht bestimmte Dateien hinzugefügt, ein Verzeichnis mit dem Platzhalter laden können in allen Ansichten verweisen auf dieses Paket enthalten sein. Wenn ein Paket mit der Ansichtsskript hinzugefügt wird, erhalten Sie möglicherweise einen JavaScript-Fehler in anderen Ansichten, die das Paket zu verweisen.
- CSS-Dateien, die andere Dateien importieren, führen in den importierten Dateien zweimal geladen. Der folgende Code erstellt beispielsweise ein Paket mit einem Großteil der jQuery UI Design CSS-Dateien, die zweimal geladen. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  Die Platzhalter-Auswahl "\*CSS" wird in jeder CSS-Datei im Ordner "", einschließlich der *Content\\Designs\\Basis\\jquery.ui.all.css* Datei. Die *jquery.ui.all.css* -Datei importiert, andere CSS-Dateien.

## <a name="bundle-caching"></a>Bündeln Sie die Zwischenspeicherung

Pakete legen Sie den HTTP-Header abläuft ein Jahr lang aus, wenn das Paket erstellt wird. Wenn Sie auf die zuvor angezeigten Seite Fiddler zeigt navigieren, die Internet Explorer nicht mit eine bedingte Anforderung, für das Paket, trifft, also stehen keine HTTP-GET-Anforderungen über Internet Explorer für die Pakete und keine HTTP-304 Antworten vom Server. Sie können erzwingen, dass Internet Explorer, um eine bedingte Anforderung für jedes Paket mit der F5-Taste (was zu einer Antwort mit HTTP-304 für jedes Paket). Sie können eine vollständige Aktualisierung erzwingen, indem Sie mithilfe von ^ F5 (was in einer HTTP 200-Antwort für jedes Paket).

Die folgende Abbildung zeigt die **Caching** den Fiddler-Fensterbereich "Response" auf der Registerkarte:

![Fiddler-caching-Bild](bundling-and-minification/_static/image8.png)

Die Anforderung   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 für das Paket wird **AllMyScripts** und enthält zwei Zeichenfolge Abfrage **v r0sLDicvP58AIXN =\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Die Abfragezeichenfolge **v** verfügt über einen Wert, d. h. einen eindeutigen Bezeichner, der zum Zwischenspeichern verwendeten token. Solange das Paket nicht ändert, fordert die ASP.NET-Anwendung die **AllMyScripts** bündeln mit diesem Token. Wenn alle Dateien im Paket geändert wird, generiert das ASP.NET-Framework für die Optimierung ein neues Tokens garantieren, dass Browseranforderungen für das Paket das neueste Paket erhalten.

Wenn Sie IE9 F12-Entwicklertools ausgeführt, und navigieren Sie zu einem zuvor geladenen Seite, zeigt IE falsch bedingten GET-Anforderungen für jedes Paket und dem Server, die HTTP-304 zurückgeben. Sie erhalten, warum IE9 hat Probleme, die bestimmen, ob eine bedingte Anforderung den Blogeintrag erstellt wurde [CDNs mithilfe von "und" Expires ", Website verbessern der Leistung](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>WENIGER, Sass CoffeeScript, SCSS, bündeln.

Die Bündelung und Minimierung-Framework bietet einen Mechanismus zum Verarbeiten von intermediate Sprachen wie z. B. [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [weniger](http://www.dotlesscss.org/) oder [Coffeescript ](http://coffeescript.org/), und Anwenden von Transformationen, z. B. Minimierung auf das resultierende Paket. Um beispielsweise hinzufügen [less](http://www.dotlesscss.org/) Dateien zu Ihrem MVC 4-Projekt:

1. Erstellen Sie einen Ordner für Ihre weniger Inhalte. Im folgenden Beispiel wird die *Content\\MyLess* Ordner.
2. Hinzufügen der [less](http://www.dotlesscss.org/) NuGet-Paket **dotless** zu Ihrem Projekt.  
    ![Dotless-NuGet-Installation](bundling-and-minification/_static/image9.png)
3. Fügen Sie eine Klasse, die implementiert die ["ibundletransform"](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) Schnittstelle. Fügen Sie den folgenden Code zu Ihrem Projekt hinzu, für die Transformation less.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Erstellen Sie eine Zusammenstellung von LESS-Dateien mit der `LessTransform` und ["cssminify"](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformieren. Fügen Sie den folgenden Code der `RegisterBundles` -Methode in der die *App\\_starten\\BundleConfig.cs* Datei.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Fügen Sie den folgenden Code an Sichten, die weniger Paket verweist.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Bundle-Überlegungen

Guten Programmierstil, befolgen beim Erstellen von Paketen ist als Präfix in den Namen des Pakets einschließen "bündeln". Dadurch wird verhindert, dass ein möglicher [Konflikt beim routing](https://forums.asp.net/post/5012037.aspx).

Nachdem Sie eine Datei in einem Paket aktualisieren, wird ein neues Token für die Bundle-Abfragezeichenfolgenparameter generiert, und das vollständige Paket das nächste Mal, das eine Seite mit das Paket von einem Client angefordert, heruntergeladen werden muss. Im herkömmlichen Markup, in dem jedes Objekt einzeln aufgeführt ist, würde nur die geänderte Datei heruntergeladen werden. Objekte, die sich häufig ändern können nicht zum Bündeln von geeignet.

Bündelung und Minimierung wird in erster Linie die erste Anforderung Seitenladezeit verbessern. Sobald eine Webseite angefordert wurde, speichert der Browser die Objekte (JavaScript, CSS und Bilder), um Bündelung und Minimierung wird kein Leistungssteigerung bieten bei der Anforderung von der gleichen Seite oder Seiten auf dem gleichen Standort die gleichen Ressourcen anfordern. Wenn Sie nicht Festlegen der expires-Header ordnungsgemäß auf Ihre Ressourcen und Sie nicht verwenden, Bündelung und Minimierung, Browsern Aktualität Heuristik kennzeichnet die Assets veraltete nach einigen Tagen und der Browser wird eine Anforderung zur abonnementüberprüfung erforderlich, für jedes Medienobjekt. In diesem Fall an, Bündelung und Minimierung eine Leistungssteigerung nachdem ich die erste Seitenanforderung. Weitere Informationen finden Sie im Blog [CDNs mithilfe von "und" Expires ", Website verbessern der Leistung](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Die Browser-Einschränkung für sechs gleichzeitige Verbindungen pro jeden Hostnamen kann behoben werden, indem Sie mit einem [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Da das CDN einen anderen Hostnamen als Ihre hosting-Site verfügt, werden Asset-Anforderungen vom CDN nicht für die sechs gleichzeitige Verbindungen begrenzt Ihrer Hostingumgebung gezählt. Ein CDN kann auch allgemeine Paket Zwischenspeichern und Edgeserver für das caching Vorteile bereitstellen.

Pakete müssen von Seiten partitioniert werden, die sie benötigen. Beispielsweise erstellt der ASP.NET MVC-Vorlage für eine internetanwendung ein jQuery-Validierung-Paket von jQuery getrennt. Da die Standardansichten erstellt keine Eingabe haben, und diese Werte nicht veröffentlichen, enthalten sie nicht das Paket für die Überprüfung.

Die `System.Web.Optimization` Namespace wird in implementiert *System.Web.Optimization.dll*. Es nutzt die WebGrease-Bibliothek (*WebGrease.dll*) für die Minimierung von Funktionen, die wiederum verwendet *Antlr3.Runtime.dll*.

*Ich verwende Twitter, erstellen schnell Beiträge und Freigeben von Links. Mein Twitter-Handle ist*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Video:[bündeln und Optimieren von](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) von [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Optimierung der Webseiten-Website hinzufügen](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Hinzufügen von Bündelung und Minimierung zu Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Leistungseinbußen bei der Bündelung und Minimierung auf Webbrowsen](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) von [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Verwenden des CDNs und zur Verbesserung der Leistung der Website läuft](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) von Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimieren der RTT (Roundtripzeiten)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
