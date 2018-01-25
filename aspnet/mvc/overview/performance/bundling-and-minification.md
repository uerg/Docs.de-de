---
uid: mvc/overview/performance/bundling-and-minification
title: Bundling und Minimierung | Microsoft Docs
author: Rick-Anderson
description: "Bundling und Minimierung sind zwei Techniken können Sie in ASP.NET 4.5 um Ladezeit für die Anforderung zu verbessern. Bundling und Minimierung verbessert die Ladezeit von Reducin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 7192481de46c36f7de71164766e68afdbba74f6d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="bundling-and-minification"></a>Bundling und Minimierung
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Bundling und Minimierung sind zwei Techniken können Sie in ASP.NET 4.5 um Ladezeit für die Anforderung zu verbessern. Bündelung und Minimierung verbessert die Ladezeit von reduziert die Anzahl der Anforderungen an den Server und reduziert die Größe der angeforderten Objekte (z. B. CSS- und JavaScript.)


Die meisten der aktuellen wichtigen Browser schränken die Anzahl der [gleichzeitige Verbindungen](http://www.browserscope.org/?category=network) pro jeden Hostnamen auf 6. Das heißt, während sechs Anforderungen verarbeitet werden, zusätzliche Anforderungen für Ressourcen auf einem Host vom Browser in die Warteschlange gestellt werden. Zeigt, in der folgenden Abbildung wird die Internet Explorer F12 Developer Tools Netzwerk Registerkarten der zeitlichen Steuerung für Objekte, die durch die Info-Sicht einer Beispiel-Anwendung erforderlich sind.

![B/M](bundling-and-minification/_static/image1.png)

Die grauen Balken zeigen die Uhrzeit die Anforderung vom Browser warten auf den Grenzwert für sechs Verbindungen in die Warteschlange gestellt wird. Die gelbe Leiste ist die Anforderungszeit zum ersten Byte, d. h. die Zeitdauer für die Anforderung senden und Empfangen der ersten Antwort vom Server. Die blauen Striche zeigen die Zeitdauer für die Antwortdaten vom Server empfangen. Sie können auf ein Medienobjekt abzurufenden ausführliche Zeitsteuerungsinformationen doppelklicken. Die folgende Abbildung zeigt z. B. die Details der zeitlichen Steuerung für das Laden der */Scripts/MyScripts/JavaScript6.js* Datei.

![](bundling-and-minification/_static/image2.png)

Der vorherigen Abbildung wird die **starten** welche gibt die Zeit, die die Anforderung wurde in der Warteschlange aufgrund der Browser die Anzahl gleichzeitiger Verbindungen-Ereignis. In diesem Fall wurde die Anforderung 46 Millisekunden wartet auf eine andere Anforderung zum Abschließen der in die Warteschlange eingereiht.

## <a name="bundling"></a>Bundling

Bundling ist ein neues Feature in ASP.NET 4.5, die ganz einfach kombinieren oder mehrere Dateien in einer einzelnen Datei bündeln. Sie können CSS, JavaScript und andere Pakete erstellen. Weniger Dateien bedeutet, dass weniger HTTP-Anforderungen und die erste Seite Last verbessern können.

Die folgende Abbildung zeigt die gleiche Timing-Sicht, der die Info-Sicht, die zuvor, aber diesmal mit das Bündelung und Minimierung aktiviert angezeigt.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minimierung

Minimierung führt eine Vielzahl von anderen Code, der Optimierungen, Skripts oder Css, beispielsweise das Entfernen unnötiger Leerzeichen und Kommentare und Verkürzung Variablennamen in einem Zeichen. Betrachten Sie die folgenden JavaScript-Funktion.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Nach der Minimierung wird die Funktion auf die folgenden reduziert:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Zusätzlich zu entfernen, Kommentare und Leerzeichen nicht erforderlich, wurden die folgenden Parameter und Variablen umbenannt (gekürzt) wie folgt:

| **Original** | **Umbenannt** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | t |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Auswirkungen der Bündelung und Minimierung

Die folgende Tabelle zeigt einige wichtige Unterschiede zwischen alle Objekte einzeln auflisten und Bündelung und Minimierung (B/M) in das Beispielprogramm verwenden.

|  | **Mithilfe von B/M** | **Ohne B/M** | **Ändern** |
| --- | --- | --- | --- |
| **Dateianforderungen** | 9 | 34 | 256% |
| **Gesendete KB** | 3.26 | 11.92 | 266% |
| **KB empfangen** | 388.51 | 530 | 36% |
| **Ladezeit** | 510 MS | 780 MS | 53% |

Der gesendeten Bytes hat zu eine erhebliche Reduzierung mit das Bündelung wie Browsern Recht ausführlich mit den HTTP-Headern, die sie für Anforderungen gelten. Verkürzung der Dauer des empfangenen Bytes nicht so groß ist. da die größten Dateien (*Scripts\jquery-ui-1.8.11.min.js* und *Scripts\jquery-1.7.1.min.js*) bereits verkleinert werden. Hinweis: Die Anzeigedauer auf das Beispielprogramm verwendet die [Fiddler](http://www.fiddler2.com/fiddler2/) Tool ein langsames Netzwerk zu simulieren. (Aus der Fiddler **Regeln** klicken Sie im Menü **Leistung** dann **simulieren Modem Geschwindigkeiten**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Debuggen von JavaScript verkleinert und gebündelt

Ist ganz einfach zum Debuggen des JavaScript in einer Entwicklungsumgebung (, in dem die [Kompilierung Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der *"Web.config"* Datei wird festgelegt, um `debug="true"` ), weil die JavaScript-Dateien nicht zusammengefasst sind oder verkleinert. Sie können auch einen Releasebuild debuggen, in dem die JavaScript-Dateien gebündelt und verkleinert werden. Verwenden den Internet Explorer F12-Entwicklungstools, Debuggen Sie eine JavaScript-Funktion enthalten, die in einem verkleinerte Paket wie folgt vorgehen:

1. Wählen Sie die **Skript** Registerkarte, und wählen Sie dann die **Starten des Debuggens** Schaltfläche.
2. Wählen Sie das Paket mit der JavaScript-Funktion, die Sie mithilfe der Schaltfläche "Bestand" Debuggen möchten.  
    ![](bundling-and-minification/_static/image4.png)
3. Formatieren Sie die verkleinerte JavaScript durch Auswahl der **Schaltfläche Konfiguration** ![](bundling-and-minification/_static/image5.png), auswählen und dann **Format JavaScript**.
4. In der **Suche Skript** t-Eingabefeld, wählen Sie den Namen der Funktion, die Sie debuggen möchten. In der folgenden Abbildung **AddAltToImg** eingegeben wurde die **Suche Skript** t Eingabefeld.  
    ![](bundling-and-minification/_static/image6.png)

Weitere Informationen zum Debuggen mit den F12 Entwicklertools finden Sie im MSDN-Artikel [mit den F12 Entwicklertools zum Debuggen von JavaScript-Fehler](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Steuern des Bündelung und Minimierung

Bündelung und Minimierung aktiviert oder deaktiviert, indem Sie den Wert des Attributs "Debuggen" in der [Kompilierung Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der *"Web.config"* Datei. Im folgenden XML `debug` ist auf diesem "true" festgelegt ist Bündelung und Minimierung deaktiviert ist.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Legen Sie zum Aktivieren von Bündelung und Minimierung der `debug` Wert auf "False". Sie überschreiben können die *"Web.config"* festlegen, mit der `EnableOptimizations` Eigenschaft auf die `BundleTable` Klasse. Der folgende Code ermöglicht Bündelung und Minimierung und überschreibt alle Einstellungen in der *"Web.config"* Datei.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> Es sei denn, `EnableOptimizations` ist `true` oder das Debug-Attribut in der [Kompilierung Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der *"Web.config"* Datei wird festgelegt, um `false`, Dateien nicht gebündelt oder verkleinert werden. Darüber hinaus die .min Version der Dateien wird nicht verwendet werden, werden die vollständige Debugversionen ausgewählt werden. `EnableOptimizations`überschreibt das Debug-Attribut in der [Kompilierung Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in der *"Web.config"* Datei


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Mithilfe von Bündelung und Minimierung mit ASP.NET Web Forms und Web Pages

- Für Webseiten, finden Sie im Blogeintrag [Weboptimierung hinzufügen, um eine Web Pages-Website](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Web Forms, finden Sie im Blogeintrag [hinzufügen Bündelung und Minimierung mit Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Mithilfe von Bündelung und Minimierung mit ASP.NET MVC

In diesem Abschnitt werden wir eine ASP.NET MVC Projekt untersuchen Bündelung und Minimierung erstellen. Erstellen Sie zunächst ein neues ASP.NET MVC-Internet-Projekt namens **MvcBM** ohne die Standardwerte ändern.

Öffnen der *App\_Start\BundleConfig.cs* Datei, und überprüfen Sie die `RegisterBundles` Methode dient zum Erstellen, registrieren und Konfigurieren der Pakete. Der folgende Code zeigt einen Teil der `RegisterBundles` Methode.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

Der vorangehende Code erstellt ein neues JavaScript-Bundle, das mit dem Namen *~/bundles/jquery* , umfasst der entsprechenden (, aber nicht verkleinert oder wird Debug. *Das Vsdoc*) Dateien in den *Skripts* Ordner, die die Platzhalter-Zeichenfolge ".js ~/Scripts/jquery-{Version}" entsprechen. Für ASP.NET MVC 4, bedeutet dies, mit einer Debugkonfiguration Datei *Jquery-1.7.1.js* wird, die dem Paket hinzugefügt werden. In einer Releasekonfiguration *Jquery-1.7.1.min.js* hinzugefügt werden. Das bundling Framework folgt mehrere allgemeine Konventionen wie z. B.:

- ".Min"-Datei für Version auswählen, wenn "FileX.min.js" und "FileX.js" vorhanden sind.
- Die nicht ".min"-Version für das Debuggen auszuwählen.
- Ignorieren von "-Vsdoc" Dateien (z. B. Jquery-1.7.1-vsdoc.js), die nur von IntelliSense verwendet werden.

Die `{version}` Platzhalterzeichen oben gezeigten dient zum automatischen Erstellen von einem jQuery-Paket mit der entsprechenden Version von jQuery in Ihre *Skripts* Ordner. In diesem Beispiel enthält einen Platzhalter mit die folgenden Vorteile:

- Können Sie NuGet verwenden, um auf eine neuere Version von jQuery zu aktualisieren, ohne die vorangehenden Bündeln von Code oder jQuery-Verweise in Ihrem Ansichtsseiten zu ändern.
- SELECT-Anweisungen werden automatisch die vollständige Version für Debugkonfigurationen als auch die Version ".min" für Version erstellt.

## <a name="using-a-cdn"></a>Verwenden von CDN

 Der folgende Code ersetzt die lokale jQuery-Paket mit einem CDN-jQuery-Paket.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

Im obigen Code wird vom CDN jQuery angefordert werden, während in Version Modus und die Debugversion von jQuery lokal im Debugmodus abgerufen werden. Bei einem CDN zu verwenden, sollten Sie einen fallback-Mechanismus verfügen, für den Fall, dass die CDN-Anforderung schlägt fehl. Das folgende Markup fragment vom Ende Layout zeigt das Skript für die Anforderungszeit jQuery die CDN-Fehler sollten hinzugefügt.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Erstellen ein Paket

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `Include` Methode nimmt ein Array von Zeichenfolgen, wobei jede Zeichenfolge einen virtuellen Pfad zu der Ressource ist. Der folgende Code aus der RegisterBundles-Methode in der *App\_Start\BundleConfig.cs* Datei zeigt, wie mehrere Dateien auf ein Paket hinzugefügt werden:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `IncludeDirectory` Methode wird bereitgestellt, um alle Dateien in ein Verzeichnis (und optional alle Unterverzeichnisse) hinzufügen, die einem Suchmuster entsprechen. Die [Bundle](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) Klasse `IncludeDirectory` API wird unten gezeigt:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Pakete werden in Ansichten, die mit der Render-Methode verwiesen wird ( `Styles.Render` für CSS und `Scripts.Render` für JavaScript). Das folgende Markup aus der *Views\Shared\\_Layout.cshtml* -Datei zeigt, wie die Standardansichten für ASP.NET Internet Projekt CSS- und JavaScript-Pakete verweisen.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Beachten Sie die Render-Methoden wird ein Array von Zeichenfolgen, aus, damit Sie mehrere Pakete in eine Codezeile hinzufügen können. Im Allgemeinen sollten Sie die Render-Methoden verwenden, die die erforderliche HTML, um das Medienobjekt verweisen, erstellen. Sie können die `Url` Methode zum Generieren der URL für das Medienobjekt ohne das Markup erforderlich, um das Medienobjekt verweisen. Angenommen Sie möchten, verwenden Sie das neue HTML5 [Async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) Attribut. Der folgende Code veranschaulicht die Verwendung von Modernizr verweisen die `Url` Methode.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Mithilfe der "\*" Platzhalterzeichen, um Dateien auszuwählen.

Die im angegebenen virtuellen Pfad der `Include` -Methode, und suchen Sie die Muster den `IncludeDirectory` Methode akzeptiert eine "\*" Platzhalterzeichen als Präfix oder Suffix in das letzte Pfadsegment. Die Suchzeichenfolge wird Groß-/Kleinschreibung nicht beachtet. Die `IncludeDirectory` Methode hat die Möglichkeit, Unterverzeichnisse durchsucht.

Angenommen Sie, ein Projekt mit den folgenden JavaScript-Dateien:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![Dir imag](bundling-and-minification/_static/image7.png)

Die folgende Tabelle zeigt die Dateien auf ein Paket mithilfe des Platzhalterzeichens gezeigten hinzugefügt:

| **Call** | **Dateien, die hinzugefügt oder eine Ausnahme ausgelöst** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Ausnahme für ungültiges Muster. Das Platzhalterzeichen ist nur für das Präfix oder Suffix zulässig. |
| Include("~/Scripts/Common/\*og.\*") | Ausnahme für ungültiges Muster. Nur ein Platzhalterzeichen ist zulässig. |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | Ausnahme für ungültiges Muster. Ein reines Platzhaltersegment ist ungültig. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory("~/Scripts/Common", "T\*",true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Jede Datei explizit auf ein Paket hinzugefügt wird im Allgemeinen die bevorzugte über Platzhalter-Laden von Dateien aus den folgenden Gründen:

- Skripts hinzugefügt standardmäßig Platzhalter in alphabetischer Reihenfolge wird in der Regel nicht laden. CSS und JavaScript-Dateien müssen häufig in einer bestimmten Reihenfolge (nicht-alphabetische) hinzugefügt werden. Sie können dieses Risiko verringern, indem das Hinzufügen einer benutzerdefinierten [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) Implementierung, aber das explizite hinzufügen jeder Datei ist weniger fehleranfällig. Beispielsweise möglichweise neue Ressourcen in einen Ordner in der Zukunft die Sie ändern, erfordern möglicherweise Ihre [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) Implementierung.
- Ansicht bestimmte Dateien in ein Verzeichnis mit Platzhalter laden hinzugefügt werden, können in allen Ansichten, verweisen auf dieses Paket aufgenommen werden. Wenn die Ansichtsskript auf ein Paket hinzugefügt wird, erhalten Sie möglicherweise einen JavaScript-Fehler auf andere Sichten, die das Paket zu verweisen.
- CSS-Dateien, die anderen Dateien zu importieren, führen in den importierten Dateien zweimal geladen. Der folgende Code erstellt z. B. ein Paket mit einem Großteil der jQuery UI Design CSS-Dateien, zwei Mal geladen. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

 Die Platzhalter-Auswahl "\*CSS" wird in jeder CSS-Datei im Ordner "", einschließlich der *Content\themes\base\jquery.ui.all.css* Datei. Die *jquery.ui.all.css* Datei andere CSS-Dateien importiert.

## <a name="bundle-caching"></a>Bündeln, Zwischenspeichern

Pakete legen Sie den HTTP-Header abläuft ein Jahr aus, wenn das Paket erstellt wird. Wenn Sie zu einer zuvor angezeigten Seite Fiddler zeigt navigieren, die Internet Explorer nicht mit eine bedingte Anforderung für das Paket, spielt also stehen keine HTTP-GET-Anforderungen von Internet Explorer für das Paket aus und keine 304 HTTP-Antworten vom Server. Sie können IE einer bedingten Anforderung für jedes Paket mit der F5-Taste (was in einer 304 HTTP-Antwort für jedes Paket) zu erzwingen. Sie können eine vollständige Aktualisierung erzwingen, indem Sie mit ^ F5 (was in einer Antwort "HTTP 200" für jedes Paket).

Die folgende Abbildung zeigt die **Caching** Fiddler-Antwort-Bereich auf der Registerkarte:

![Fiddler-caching-Bild](bundling-and-minification/_static/image8.png)

Die Anforderung   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 für das Paket wird **AllMyScripts** und enthält eine Abfrage Zeichenfolgenpaar **V = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. Die Abfragezeichenfolge **v** verfügt über einen Wert, d. h. einen eindeutigen Bezeichner, der zum Zwischenspeichern verwendeten token. Solange das Paket nicht ändern, fordert die ASP.NET-Anwendung die **AllMyScripts** bündeln diese Token verwenden. Wenn alle Dateien im Paket geändert wird, wird das ASP.NET-Framework für die Optimierung generiert ein neues Token, um zu garantieren, dass Browseranforderungen für das Paket, das aktuelle Paket erhält.

Wenn Sie die IE9 F12 Entwicklertools ausgeführt, und navigieren Sie zu einem zuvor geladenen Seite, zeigt IE falsch bedingte GET-Anforderungen an jedes Paket und dem Server, die HTTP-304 zurückgeben. Erfahren Sie, warum IE9 Probleme, die bestimmen, ob eine bedingte Anforderung, im Blogeintrag gestellt wurde hat [CDNs verwenden und Expires Website verbessern der Leistung](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>WENIGER, CoffeeScript, SCSS, Sass bündeln.

Die Bündelung und Minimierung-Framework bietet einen Mechanismus zum Verarbeiten intermediate Sprachen wie z. B. [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [weniger](http://www.dotlesscss.org/) oder [Coffeescript ](http://coffeescript.org/), und Anwenden von Transformationen, z. B. Minimierung auf das resultierende Paket. Um beispielsweise hinzufügen [. less](http://www.dotlesscss.org/) dem MVC 4-Projekt Dateien:

1. Erstellen Sie einen Ordner für Ihre weniger Inhalte. Im folgenden Beispiel wird die *Content\MyLess* Ordner.
2. Hinzufügen der [. less](http://www.dotlesscss.org/) NuGet-Paket **ohne Punkte** zu Ihrem Projekt.  
    ![NuGet-ohne Punkte installieren](bundling-and-minification/_static/image9.png)
3. Fügen Sie eine Klasse, die implementiert die [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) Schnittstelle. Fügen Sie für die Transformation. less dem Projekt den folgenden Code hinzu.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Erstellen Sie eine Zusammenstellung von LESS-Dateien mit der `LessTransform` und [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformieren. Fügen Sie folgenden Code, der `RegisterBundles` Methode in der *App\_Start\BundleConfig.cs* Datei.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Fügen Sie den folgenden Code an Sichten, die weniger Paket verweist.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Bundle-Überlegungen

Eine gute Namenskonvention zu befolgen, wenn Pakete zu erstellen ist, als ein Präfix im Bundle-Namen einschließen "bündeln". Hierdurch wird ein mögliches [Konflikt beim routing](https://forums.asp.net/post/5012037.aspx).

Nachdem Sie eine Datei in einem Paket aktualisieren, ein neues Token ist für Paket Abfragezeichenfolgen-Parameters generiert, und das vollständige Paket das nächste Mal, das eine Seite, enthält das Paket von einem Client angefordert, heruntergeladen werden muss. In herkömmlichen Markup, in dem jedes Medienobjekt einzeln aufgeführt ist, würde nur die geänderte Datei heruntergeladen werden. Ressourcen, die häufig geändert werden möglicherweise nicht geeignet für bündeln.

Bundling und Minimierung wird in erster Linie die erste Anforderung Seitenladezeit verbessern. Sobald eine Webseite angefordert wurde, speichert der Browser Objekte (JavaScript, CSS und Bilder), um Bündelung und Minimierung wird kein Leistungssteigerung beim Anfordern von derselben Seite bieten oder Seiten auf dem gleichen Standort die gleichen Ressourcen anfordern. Wenn Sie nicht Festlegen der Header ordnungsgemäß auf Ihre Medienobjekte abläuft und verwenden Sie nicht Bündelung und Minimierung, Browsern Aktualität Heuristik kennzeichnet die Medienobjekte veraltete nach ein paar Tagen und Browser eine validierungsanforderung für jedes Medienobjekt erforderlich. In diesem Fall geben Bündelung und Minimierung eine Leistungssteigerung nach der ersten Seitenanforderung. Weitere Informationen finden Sie im Blogbeitrag [CDNs verwenden und Expires Website verbessern der Leistung](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

Die Browser-Einschränkung des sechs gleichzeitige Verbindungen pro jeden Hostnamen können Sie minimieren, indem Sie mit einem [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Da das CDN einen anderen Hostnamen als hosting-Website besitzt, werden Asset-Anforderungen vom CDN nicht gegen das sechs Limit für gleichzeitige Verbindungen auf Ihrer Hostingumgebung gezählt. Ein CDN kann auch allgemeine Paket caching und Edge-caching Vorteile bereitstellen.

Pakete sollten von Seiten partitioniert werden, die sie benötigen. Erstellt z. B. standardmäßig ASP.NET MVC-Projektvorlage für eine internetanwendung ein Paket der jQuery-Validierung von jQuery getrennt. Da die Standardansichten erstellt keine Eingabespalte haben und nicht Werte buchen, enthalten nicht sie das Paket für die Überprüfung.

Die `System.Web.Optimization` Namespace in System.Web.Optimization.DLL implementiert ist. Genutzt WebGrease-Bibliothek (WebGrease.dll) für Minimierung-Funktionen, die wiederum Antlr3.Runtime.dll verwendet.

*Ich verwende Twitter schnelle Beiträge und teilen Sie Links aus. Meine Twitter-Handle ist*:[@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- Video:[Bündelung und Optimieren von](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) von [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Eine Web Pages-Website Weboptimierung hinzugefügt](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Hinzufügen von Bündelung und Minimierung mit WebForms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Leistungseinbußen bei der Bündelung und Minimierung auf Browsen im Internet](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) von [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen)[@frystyk](https://twitter.com/frystyk)
- [Verwenden des CDNs und zur Verbesserung der Leistung der Website läuft](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) von Rick Anderson[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimieren Sie RTT (Roundtripzeit)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Contributors

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
