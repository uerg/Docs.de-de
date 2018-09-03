---
title: Routing in ASP.NET Core
author: ardalis
description: In diesem Artikel erfahren Sie, wie mithilfe der ASP.NET Core-Routingfunktionalität einem Routenhandler eine eingehende Anforderung zugeordnet wird.
ms.author: riande
ms.custom: mvc
ms.date: 08/20/2018
uid: fundamentals/routing
ms.openlocfilehash: 94fa6a278466c8cc9926d7893d1ef71d83b865df
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2018
ms.locfileid: "41870850"
---
# <a name="routing-in-aspnet-core"></a>Routing in ASP.NET Core

Von [Ryan Nowak](https://github.com/rynowak), [Steve Smith](https://ardalis.com/) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Mithilfe der Routingfunktionalität wird einem Routenhandler eine eingehende Anforderung zugeordnet. Routen werden in der App definiert und beim Start der App konfiguriert. Eine Route kann optional Werte aus der URL extrahieren, die in der Anforderung enthalten ist. Diese Werte können anschließend für die Verarbeitung der Anforderung verwendet werden. Mit Routeninformationen aus der App lassen sich über die Routingfunktionalität URLs generieren, die Routenhandlern zugeordnet werden. So kann durch Routing entweder ein Routenhandler auf der Grundlage einer URL ermittelt oder mithilfe von Routenhandlerinformationen eine URL bestimmt werden, die einem bestimmten Routenhandler zugeordnet ist.

> [!IMPORTANT]
> In diesem Artikel wird das Low-Level-Routing in ASP.NET Core beschrieben. Weitere Informationen zum Routing mit ASP.NET Core MVC finden Sie unter <xref:mvc/controllers/routing>.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="routing-basics"></a>Routinggrundlagen

Beim Routing werden *Routen* (Implementierungen von <xref:Microsoft.AspNetCore.Routing.IRouter>) für Folgendes verwendet:

* Zuordnen von eingehenden Anforderungen zu *Routenhandlern*
* Generieren von URLs für Antworten

Eine App verfügt normalerweise über genau eine Routenauflistung. Sobald eine Anforderung eintrifft, werden die Routen der Auflistung der Reihe nach verarbeitet. Für die eingehende Anforderung wird eine Route gesucht, die der Anforderungs-URL entspricht. Dazu wird die Methode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> für jede verfügbare Route der Routenauflistung aufgerufen. Im Gegensatz hierzu kann bei einer Antwort Routing verwendet werden, um URLs zu generieren (z.B. für Umleitungen oder Links), die auf Routeninformationen basieren. Dadurch wird die Erstellung von hartcodierten URLs verhindert, was wiederrum zur Wartbarkeit beiträgt.

Die Routingfunktionalität wird über die Klasse <xref:Microsoft.AspNetCore.Builder.RouterMiddleware> mit der [Middlewarepipeline](xref:fundamentals/middleware/index) verbunden. Bei [ASP.NET Core MVC](xref:mvc/overview) wird im Rahmen der Konfiguration die Routingfunktionalität der Middlewarepipeline hinzugefügt. Informationen darüber, wie Sie Routing als eigenständige Komponente verwenden, finden Sie im Abschnitt [Verwenden von Routingmiddleware](#use-routing-middleware).

### <a name="url-matching"></a>URL-Zuordnung

Bei einer URL-Zuordnung werden eingehende Anforderungen durch Routing an einen *Handler* gesendet. Für diesen Prozess werden üblicherweise die Daten des URL-Pfads verwendet. Es können jedoch auch alle Daten der Anforderung genutzt werden. Für die Skalierung der Größe und Komplexität einer App ist das Versenden von Anforderungen an unterschiedliche Handler entscheidend.

Eingehende Anforderungen werden vom `RouterMiddleware`-Objekt bearbeitet, das die Methode <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> für alle Routen nacheinander aufruft. In der <xref:Microsoft.AspNetCore.Routing.IRouter>-Instanz wird entschieden, ob die Anforderung *verarbeitet* wird, indem für den [RouteContext.Handler](xref:Microsoft.AspNetCore.Routing.RouteContext.Handler*) ein <xref:Microsoft.AspNetCore.Http.RequestDelegate>-Delegat festgelegt wird, der nicht NULL ist. Wenn von einer Route ein Handler für die Anforderung festgelegt wird, endet die Routenverarbeitung, und der Handler wird zur Verarbeitung der Anforderung aufgerufen. Wenn alle Routen getestet werden und kein Handler für die Anforderung gefunden werden kann, ruft die Middleware *next* auf. Daraufhin wird die nächste Middleware in der Anforderungspipeline aufgerufen.

Die Haupteingabe für `RouteAsync` ist der [RouteContext.HttpContext](xref:Microsoft.AspNetCore.Routing.RouteContext.HttpContext*), der der aktuellen Anforderung zugeordnet ist. `RouteContext.Handler` und [RouteContext.RouteData](xref:Microsoft.AspNetCore.Routing.RouteContext.RouteData*) sind Ausgaben, die festgelegt werden, nachdem eine Übereinstimmung für eine Route ermittelt wurde.

Durch eine Zuordnung während der Ausführung von `RouteAsync` werden außerdem die Eigenschaften von `RouteContext.RouteData` auf Grundlage der bisher verarbeiteten Anforderungen auf entsprechende Werte festgelegt. Wenn eine Route einer Anforderung zugeordnet wird, enthält `RouteContext.RouteData` wichtige Zustandsinformationen zum *Ergebnis*.

[RouteData.Values](xref:Microsoft.AspNetCore.Routing.RouteData.Values*) ist ein Wörterbuch mit *Routenwerten*, das aus der Route erstellt wird. Die Werte werden in der Regel durch die Tokenisierung der URL ermittelt und können so verwendet werden, dass Benutzereingaben akzeptiert oder weitere Entscheidungen in der App zum Versenden von Anforderungen getroffen werden.

[RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*) ist eine Eigenschaftensammlung mit zusätzlichen Daten zur zugeordneten Route. `DataTokens` werden bereitgestellt, damit sich jeder Route Zustandsdaten zuordnen lassen, sodass in der App später Entscheidungen auf Grundlage der zugeordneten Route getroffen werden können. Diese Werte werden vom Entwickler vorgegeben und beeinflussen das Routingverhalten **in keiner Weise**. Im Gegensatz zu Routenwerten, die sich leicht in bzw. aus Zeichenfolgen konvertieren lassen müssen, können Datentokenwerte außerdem einem beliebigen Typ entsprechen.

[RouteData.Routers](xref:Microsoft.AspNetCore.Routing.RouteData.Routers*) ist eine Liste der Routen, die an der Zuordnung der Anforderung beteiligt waren. Routen können in anderen Routen geschachtelt werden. Die `Routers`-Eigenschaft stellt den Pfad mithilfe der logischen Routenstruktur dar, die zu der Zuordnung geführt hat. Üblicherweise ist das erste Element in `Routers` die Routensammlung. Dieses sollte zur URL-Generierung verwendet werden. Das letzte Element in `Routers` ist der Routenhandler, für den eine Zuordnung vorgenommen wurde.

### <a name="url-generation"></a>URL-Generierung

Bei der URL-Generierung wird durch Routing ein URL-Pfad basierend auf mehreren Routenwerten erstellt. Dies ermöglicht eine logische Trennung zwischen den Handlern und den URLs, die auf die Handler zugreifen.

Auch für die URL-Generierung wird ein iterativer Prozess verwendet, bei dem allerdings zuerst vom Benutzer- oder Frameworkcode die <xref:Microsoft.AspNetCore.Routing.IRouter.GetVirtualPath*>-Methode der Routensammlung aufgerufen wird. Anschließend wird nacheinander für jede *Route* die zugehörige `GetVirtualPath`-Methode aufgerufen, bis ein <xref:Microsoft.AspNetCore.Routing.VirtualPathData>-Objekt zurückgegeben wird, das nicht NULL ist.

Die primären Eingaben für `GetVirtualPath` sind:

* [VirtualPathContext.HttpContext](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.HttpContext*)
* [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*)
* [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*)

Für Routen werden überwiegend die Routenwerte verwendet, die von `Values` und `AmbientValues` bereitgestellt werden. Dadurch wird ermittelt, wo eine URL erstellt werden kann und welche Werte diese enthalten soll. `AmbientValues` sind Routenwerte, die durch die Zuordnung von aktueller Anforderung und Routingsystem erstellt wurden. Im Gegensatz dazu sind `Values` die Routenwerte, die angeben, wie die gewünschte URL für den aktuellen Vorgang generiert werden soll. `HttpContext` wird bereitgestellt, falls für eine Route Dienste oder zusätzliche Daten, die mit dem aktuellen Kontext verknüpft sind, abgerufen werden müssen.

> [!TIP]
> [VirtualPathContext.Values](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.Values*) kann als Menge von überschriebenen Eigenschaften für [VirtualPathContext.AmbientValues](xref:Microsoft.AspNetCore.Routing.VirtualPathContext.AmbientValues*) betrachtet werden. Bei der URL-Generierung wird versucht, Routenwerte der aktuellen Anforderung wiederzuverwenden, um so URLs für Links generieren zu können, die dieselbe Route oder dieselben Routenwerte verwenden.

Die Ausgabe von `GetVirtualPath` ist ein `VirtualPathData`-Objekt. `VirtualPathData` verhält sich analog zu `RouteData`. `VirtualPathData` enthält das `VirtualPath`-Objekt für die Ausgabe-URL und einige zusätzlichen Eigenschaften, die von der Route festgelegt werden sollten.

Die [VirtualPathData.VirtualPath](xref:Microsoft.AspNetCore.Routing.VirtualPathData.VirtualPath*)-Eigenschaft enthält den *virtuellen Pfad*, der von der Route erstellt wird. Je nach Anforderungen muss der Pfad eventuell noch weiter verarbeitet werden. Wenn die generierte URL in HTML gerendert werden soll, müssen Sie den App-Basispfad voranstellen.

[VirtualPathData.Router](xref:Microsoft.AspNetCore.Routing.VirtualPathData.Router*) ist ein Verweis auf die Route, mit der die URL generiert wurde.

Die [VirtualPathData.DataTokens](xref:Microsoft.AspNetCore.Routing.VirtualPathData.DataTokens*)-Eigenschaften stellen ein Wörterbuch mit zusätzlichen Daten zur Route dar, mit der die URL generiert wurde. Diese Eigenschaften verhalten sich analog zu [RouteData.DataTokens](xref:Microsoft.AspNetCore.Routing.RouteData.DataTokens*).

### <a name="creating-routes"></a>Erstellen von Routen

Für die Routingfunktionalität wird die <xref:Microsoft.AspNetCore.Routing.Route>-Klasse als Standardimplementierung von <xref:Microsoft.AspNetCore.Routing.IRouter> zur Verfügung gestellt. `Route` verwendet die Syntax für *Routenvorlagen* und definiert so Muster, die mit dem URL-Pfad abgeglichen werden, wenn <xref:Microsoft.AspNetCore.Routing.IRouter.RouteAsync*> aufgerufen wird. `Route` nutzt dieselbe Routenvorlage zum Generieren einer URL, wenn `GetVirtualPath` aufgerufen wird.

Die meisten Apps erstellen Routen, indem sie <xref:Microsoft.AspNetCore.Builder.MapRouteRouteBuilderExtensions.MapRoute*> oder eine ähnliche Erweiterungsmethode aufrufen, die in <xref:Microsoft.AspNetCore.Routing.IRouteBuilder> definiert ist. All diese Methoden erstellen eine Instanz von <xref:Microsoft.AspNetCore.Routing.Route> und fügen diese der Routensammlung hinzu.

Von `MapRoute` wird kein Routenhandlerparameter akzeptiert. Durch `MapRoute` werden nur Routen hinzugefügt, die vom <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*> verarbeitet werden. Da der Standardhandler ein `IRouter`-Objekt ist, verarbeitet dieser die Anforderung eventuell nicht. Beispielsweise ist ASP.NET Core MVC üblicherweise als Standardhandler konfiguriert, der nur Anforderungen verarbeitet, die mit einem verfügbaren Controller und einer Aktion übereinstimmen. Weitere Informationen zum Routing zu MVC finden Sie unter <xref:mvc/controllers/routing>.

Im folgenden Codebeispiel wird ein `MapRoute`-Aufruf dargestellt, der von einer typischen ASP.NET Core MVC-Routendefinition verwendet wird:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Mit dieser Vorlage wird durch einen Abgleich beispielsweise der URL-Pfad `/Products/Details/17` gefunden, und die Routenwerte `{ controller = Products, action = Details, id = 17 }` werden extrahiert. Diese werden ermittelt, indem der URL-Pfad in Segmente aufgeteilt wird und jedes Segment mit dem Namen des *Routenparameters* in der Routenvorlage abgeglichen wird. Jeder Routenparameter hat einen Namen. Dieser wird von Klammern `{ ... }` eingeschlossen und dadurch definiert.

Die obige Vorlage würde bei einem Abgleich auch den URL-Pfad `/` finden und daraus die Werte `{ controller = Home, action = Index }` generieren. Dies liegt daran, dass die Routenparameter `{controller}` und `{action}` über Standardwerte verfügen und der Routenparameter `id` optional ist. Hinter dem Namen des Routenparameters steht das Gleichheitszeichen (`=`), auf das der Wert folgt, der als Standardwert für den Parameter definiert wird. Durch ein Fragezeichen (`?`) nach dem Namen des Routenparameters wird der Parameter als optional festgelegt. Durch Routenparameter mit einem Standardwert wird *immer* ein Routenwert erzeugt, wenn eine Übereinstimmung für die Route ermittelt wird. Bei optionalen Parametern wird kein Routenwert generiert, wenn kein entsprechendes URL-Pfadsegment vorhanden ist.

Eine ausführliche Beschreibung zu den Features und zur Syntax der Routenvorlage finden Sie unter [Referenz für Routenvorlagen](#route-template-reference).

Das folgende Beispiel demonstriert eine *Routeneinschränkung*:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id:int}");
```

Durch diese Vorlage wird bei einem Abgleich beispielsweise der URL-Pfad `/Products/Details/17`, aber nicht `/Products/Details/Apples` gefunden. In der Routenparameterdefinition `{id:int}` wird eine [Routeneinschränkung](#route-constraint-reference) für den Routenparameter `id` festgelegt. In derartigen Einschränkungen wird <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> implementiert, und die Routenwerte werden auf Gültigkeit geprüft. Im obigen Beispiel muss sich der Routenwert `id` in einen Integer konvertieren lassen. Eine ausführliche Beschreibung der Routeneinschränkungen, die vom Framework bereitgestellt werden, finden Sie in der [Referenz zu Routeneinschränkungen](#route-constraint-reference).

Bei zusätzlichen Überladungen von `MapRoute` werden Werte für `constraints`, `dataTokens` und `defaults` akzeptiert. Diese zusätzlichen Parameter von `MapRoute` werden als `object`-Typ definiert. Üblicherweise werden diese Parameter so verwendet, dass ein Objekt eines anonymen Typs übergeben wird, in dem die Eigenschaftennamen des anonymen Typs mit den Routenparameternamen abgeglichen werden.

In den beiden folgenden Beispielen werden identische Routen erstellt:

```csharp
routes.MapRoute(
    name: "default_route",
    template: "{controller}/{action}/{id?}",
    defaults: new { controller = "Home", action = "Index" });

routes.MapRoute(
    name: "default_route",
    template: "{controller=Home}/{action=Index}/{id?}");
```

> [!TIP]
> Für einfache Routen eignet sich die Inline-Syntax zum Definieren von Einschränkungen und Standardwerten. Bestimmte Features wie Datentoken werden von dieser allerdings nicht unterstützt.

Das folgende Beispiel veranschaulicht einige weitere Szenarios:

```csharp
routes.MapRoute(
    name: "blog",
    template: "Blog/{*article}",
    defaults: new { controller = "Blog", action = "ReadArticle" });
```

Mit dieser Vorlage wird durch einen Abgleich z.B. der URL-Pfad `/Blog/All-About-Routing/Introduction` gefunden, und die Werte `{ controller = Blog, action = ReadArticle, article = All-About-Routing/Introduction }` werden extrahiert. Die Standardroutenwerte für `controller` und `action` werden von der Route generiert, obwohl keine entsprechenden Routenparameter in der Vorlage vorhanden sind. Standardwerte können in der Routenvorlage angegeben werden. Der Routenparameter `article` wird durch ein Sternchen (`*`) vor dem zugehörigen Namen als *Catch-All-Parameter* definiert. So wird der verbleibende Teil des URL-Pfads erfasst, und auch leere Zeichenfolgen können gefunden werden.

Im nächsten Beispiel werden Routeneinschränkungen und Datentoken hinzugefügt:

```csharp
routes.MapRoute(
    name: "us_english_products",
    template: "en-US/Products/{id}",
    defaults: new { controller = "Products", action = "Details" },
    constraints: new { id = new IntRouteConstraint() },
    dataTokens: new { locale = "en-US" });
```

Mit dieser Vorlage wird durch einen Abgleich z.B. der URL-Pfad `/en-US/Products/5` gefunden, und die Werte `{ controller = Products, action = Details, id = 5 }` sowie die Datentoken `{ locale = en-US }` werden extrahiert.

![Gebietsschemas, Windows-Tokens](routing/_static/tokens.png)

### <a name="url-generation"></a>URL-Generierung

Durch die Kombination von mehreren Routenwerten und der zugehörigen Routenvorlage kann die `Route`-Klasse auch URLs generieren. Dieser Prozess verläuft umgekehrt zum Abgleich eines URL-Pfads.

> [!TIP]
> Wenn Sie die URL-Generierung besser nachvollziehen möchten, sollten Sie sich überlegen, welche URL generiert werden soll und wie diese mithilfe der Routenvorlage abgeglichen wird. Dabei sollten Sie auch darüber nachdenken, welche Werte erstellt werden. Dieser Vorgang entspricht in etwa der URL-Generierung in der `Route`-Klasse.

Im nächsten Beispiel wird eine einfache ASP.NET Core MVC-Route verwendet:

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
```

Die Route generiert mithilfe der Routenwerte `{ controller = Products, action = List }` die URL `/Products/List`. Die Routenwerte werden als Ersatz für die entsprechenden Routenparameter verwendet, wodurch ein URL-Pfad erstellt werden kann. Da `id` ein optionaler Parameter ist, muss dieser über keinen Wert verfügen.

Die Route generiert mithilfe der Routenwerte `{ controller = Home, action = Index }` die URL `/`. Die bereitgestellten Routenwerte entsprechen den Standardwerten, sodass die Segmente, die mit diesen Werten übereinstimmen, problemlos ausgelassen werden können. Beachten Sie, dass mit beiden generierten URLs und dieser Routendefinition ein Roundtrip ausgeführt wird und dieselben Routenwerte erstellt werden, die zur Generierung der URL verwendet wurden.

> [!TIP]
> Eine auf ASP.NET Core MVC basierende App sollte zur Generierung von URLs die Routingfunktion nicht direkt aufrufen, sondern <xref:Microsoft.AspNetCore.Mvc.Routing.UrlHelper> verwenden.

Weitere Informationen zum URL-Generierungsprozess finden Sie unter [Referenz für URL-Generierung](#url-generation-reference).

## <a name="use-routing-middleware"></a>Verwenden von Routingmiddleware

::: moniker range=">= aspnetcore-2.1"

Legen Sie das [Microsoft.AspNetCore.App-Metapaket](xref:fundamentals/metapackage-app) in der Projektdatei der App als Verweis fest.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Legen Sie das [Microsoft.AspNetCore.All-Metapaket](xref:fundamentals/metapackage) in der Projektdatei der App als Verweis fest.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Legen Sie [Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/) in der Projektdatei der App als Verweis fest.

::: moniker-end

Fügen Sie anschließend dem Dienstcontainer in `Startup.ConfigureServices` die Routingfunktionalität hinzu:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_ConfigureServices&highlight=3)]

::: moniker-end

Routen müssen in der `Startup.Configure`-Methode konfiguriert werden. In der Beispiel-App werden die folgenden APIs verwendet:

* `RouteBuilder`
* `Build`
* `MapGet`: Nur HTTP-GET-Anforderungen werden abgeglichen.
* `UseRouter`

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_RouteHandler)]

::: moniker-end

In der folgenden Tabelle werden die Antworten mit den angegebenen URIs aufgelistet:

| URI                  | Antwort                                          |
| -------------------- | ------------------------------------------------- |
| /package/create/3    | Hallo! Route values: [operation, create], [id, 3] |
| /package/track/-3    | Hallo! Route values: [operation, track], [id, -3] |
| /package/track/-3/   | Hallo! Route values: [operation, track], [id, -3] |
| /package/track/      | &lt;Fall through, no match&gt; (Fehlgeschlagen, keine Übereinstimmung)                    |
| GET /hello/Joe       | Hi, Joe!                                          |
| POST /hello/Joe      | &lt;Fall through, matches HTTP GET only&gt; (Fehlgeschlagen, nur für eine HTTP-GET-Anforderung wird eine Übereinstimmung ermittelt)       |
| GET /hello/Joe/Smith | &lt;Fall through, no match&gt; (Fehlgeschlagen, keine Übereinstimmung)                    |

Wenn Sie eine einzelne Route konfigurieren, müssen Sie <xref:Microsoft.AspNetCore.Builder.RoutingBuilderExtensions.UseRouter*> aufrufen und eine `IRouter`-Instanz übergeben. <xref:Microsoft.AspNetCore.Routing.RouteBuilder> muss nicht verwendet werden.

Das Framework stellt mehrere Erweiterungsmethoden zum Erstellen von Routen zur Verfügung:

* `MapRoute`
* `MapGet`
* `MapPost`
* `MapPut`
* `MapDelete`
* `MapVerb`

Für einige Methoden wie `MapGet` muss ein `RequestDelegate`-Delegat bereitgestellt werden. `RequestDelegate` wird als *Routenhandler* verwendet, wenn der Abgleich für die Route erfolgreich ist. Für andere Erweiterungsmethoden kann eine Middlewarepipeline konfiguriert werden, die als Routenhandler verwendet wird. Wenn die *Map*-Methode einen Handler wie `MapRoute` nicht akzeptiert, verwendet sie <xref:Microsoft.AspNetCore.Routing.RouteBuilder.DefaultHandler*>.

In den `Map[Verb]`-Methoden werden Einschränkungen verwendet, um die Route auf das HTTP-Verb im Methodennamen zu beschränken. Beispielsweise unter <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapGet*> und <xref:Microsoft.AspNetCore.Routing.RequestDelegateRouteBuilderExtensions.MapVerb*>.

## <a name="route-template-reference"></a>Referenz für Routenvorlagen

Token in geschweiften Klammern (`{ ... }`) definieren *Routenparameter*, die beim Abgleich der Route gebunden werden. Sie können in einem Routensegment mehrere Routenparameter definieren, wenn Sie sie durch einen Literalwert trennen. `{controller=Home}{action=Index}` ist z.B. keine gültige Route, da sich zwischen `{controller}` und `{action}` kein Literalwert befindet. Diese Routenparameter müssen einen Namen besitzen und können zusätzliche Attribute aufweisen.

Eine Literalzeichenfolge, die nicht den Routenparametern entspricht (z.B. `{id}`), muss zusammen mit dem Pfadtrennzeichen `/` mit dem URL-Text übereinstimmen. Beim Abgleich von Text wird nicht zwischen Groß-/Kleinbuchstaben unterschieden, und die Übereinstimmung basiert auf der decodierten Repräsentation des URL-Pfads. Damit das Trennzeichen `{` oder `}` der Routenparameter-Literalzeichenfolge bei einem Abgleich gefunden wird, muss es doppelt vorhanden sein (`{{` oder `}}`), was einem Escapezeichen entspricht.

Bei einem URL-Muster, durch das ein Dateiname mit einer optionalen Erweiterung erfasst werden soll, sind noch weitere Aspekte zu berücksichtigen. Dies wird z.B. anhand der Vorlage `files/{filename}.{ext?}` deutlich. Wenn sowohl `filename` als auch `ext` vorhanden sind, werden für beide Parameter Werte festgelegt. Wenn nur `filename` in der URL vorhanden ist, wird für die Route eine Übereinstimmung ermittelt, da der nachstehende Punkt (`.`) optional ist. Für die folgenden URLs wird eine Übereinstimmung für die Route ermittelt:

* `/files/myFile.txt`
* `/files/myFile`

Sie können das `*`-Zeichen als Präfix für einen Routenparameter verwenden, um eine Bindung zum verbleibenden Teil des URI herzustellen. Hierbei wird von einem *Catch-All*-Parameter gesprochen. Durch `blog/{*slug}` wird beispielsweise jeder URI ermittelt, der mit `/blog` beginnt und dahinter einen beliebigen Wert aufweist (der dann dem `slug`-Routenwert zugeordnet wird). Durch Catch-All-Parameter können auch leere Zeichenfolgen gefunden werden.

Routenparameter können über mehrere *Standardwerte* verfügen, die nach dem Parameternamen angegeben werden und durch ein Gleichheitszeichen (`=`) abgetrennt werden. Mit `{controller=Home}` wird beispielsweise `Home` als Standardwert für `controller` definiert. Der Standardwert wird verwendet, wenn kein Wert in der Parameter-URL vorhanden ist. Routenparameter können nicht nur Standardwerte aufweisen, sondern darüber hinaus auch als optional definiert werden, indem am Ende des Parameternamens ein Fragezeichen (`?`) angefügt wird. Dies ist beispielsweise bei `id?` der Fall. Der Unterschied zwischen optionalen Parametern und Routenparametern mit einem Standardwert besteht darin, dass mithilfe der letzteren immer ein Wert erzeugt wird. Ein optionaler Parameter verfügt demgegenüber nur dann über einen Wert, wenn ein solcher von der Anforderungs-URL bereitgestellt wird.

Routenparameter können des Weiteren Einschränkungen aufweisen, die mit dem gebundenen Routenwert der URL übereinstimmen müssen. Eine *Inline-Einschränkung* für einen Routenparameter geben Sie an, indem Sie hinter dem Namen des Routenparameters einen Doppelpunkt (`:`) und einen Einschränkungsnamen hinzufügen. Wenn für die Einschränkung Argumente erforderlich sind, werden diese nach dem Einschränkungsnamen in Klammern (`( )`) angegeben. Mehrere Inline-Einschränkungen können festgelegt werden, indem ein weiterer Doppelpunkt (`:`) und Einschränkungsname hinzugefügt wird. Der Einschränkungsname wird dem <xref:Microsoft.AspNetCore.Routing.IInlineConstraintResolver>-Dienst übergeben, wodurch eine Instanz von <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> für die URL-Verarbeitung erstellt werden kann. In der Routenvorlage `blog/{article:minlength(10)}` wird beispielsweise die Einschränkung `minlength` mit dem Argument `10` festgelegt. Weitere Informationen zu Routeneinschränkungen und eine Liste der vom Framework bereitgestellten Einschränkungen finden Sie im Abschnitt [Referenz für Routeneinschränkungen](#route-constraint-reference).

In der folgenden Tabelle werden mehrere Routenvorlagen und deren Verhalten beschrieben:

| Routenvorlage                         | Beispiel-URL für Abgleich  | Hinweise                                                                  |
| -------------------------------------- | --------------------- | ---------------------------------------------------------------------- |
| hello                                  | /hello                | Nur für den Pfad `/hello` wird eine Übereinstimmung ermittelt.                                  |
| {Page=Home}                            | /                     | Eine Übereinstimmung wird ermittelt, und `Page` wird auf `Home` festgelegt.                                      |
| {Page=Home}                            | /Contact              | Eine Übereinstimmung wird ermittelt, und `Page` wird auf `Contact` festgelegt.                                   |
| {controller}/{action}/{id?}            | /Products/List        | `Products` wird „controller“ und `List` „action“ zugeordnet.                       |
| {controller}/{action}/{id?}            | /Products/Details/123 |  `Products` wird „controller“ und `Details` „action“ zugeordnet.  `id` wird auf 123 festgelegt. |
| {controller=Home}/{action=Index}/{id?} | /                     |  `Home` wird „controller“ und `Index` „action“ zugeordnet. `id` wird ignoriert.       |

Mit Vorlagen lässt sich Routing besonders leicht durchführen. Einschränkungen und Standardwerte können auch außerhalb der Routenvorlage angegeben werden.

> [!TIP]
> Wenn Sie die [Protokollierung](xref:fundamentals/logging/index) aktivieren, erfahren Sie, wie die integrierten Routingimplementierungen (z.B. `Route`) Übereinstimmungen für Anforderungen ermitteln.

## <a name="reserved-routing-names"></a>Reservierte Routingnamen

Die folgenden Schlüsselwörter sind reservierte Namen und können nicht als Routennamen oder Parameter verwendet werden:

* `action`
* `area`
* `controller`
* `handler`
* `page`

## <a name="route-constraint-reference"></a>Referenz für Routeneinschränkungen

Routeneinschränkungen werden ausgeführt, wenn `Route` die Syntax der eingehenden URL abgeglichen und den URL-Pfad mit einer Tokenisierung in Routenwerte umgewandelt hat. In der Regel wird mit Routeneinschränkungen der Routenwert der zugehörigen Vorlage geprüft. Dabei wird bestimmt, ob der Wert gültig ist. Für einige Routeneinschränkungen werden anstelle des Routenwerts andere Daten verwendet, um zu ermitteln, ob das Routing einer Anforderung möglich ist. <xref:Microsoft.AspNetCore.Routing.Constraints.HttpMethodRouteConstraint> kann beispielsweise auf der Grundlage des HTTP-Verbs eine Anforderung entweder annehmen oder ablehnen.

> [!WARNING]
> Verzichten Sie bei der **Eingabevalidierung** auf Einschränkungen, da ungültige Eingaben den Fehler *404 - Not Found* (404 – Nicht gefunden) zur Folge haben. Stattdessen sollte jedoch der Fehler *400 - Bad Request* (400 – Fehlerhafte Anforderung) ausgelöst und mit einer entsprechenden Fehlermeldung angezeigt werden. Verwenden Sie Routeneinschränkungen nicht, um Eingaben für eine bestimmte Route zu überprüfen, sondern um ähnlichen Routen zu **unterscheiden**.

In der folgenden Tabelle werden mehrere Routeneinschränkungen und deren Verhalten beschrieben:

| Einschränkung | Beispiel | Beispiele für Übereinstimmungen | Hinweise |
| ---------- | ------- | --------------- | ----- |
| `int` | `{id:int}` | `123456789`, `-123456789`  | Für jeden Integer wird eine Übereinstimmung ermittelt. |
| `bool` | `{active:bool}` | `true`, `FALSE` | Für `true` oder `false` wird eine Übereinstimmung ermittelt (keine Unterscheidung zwischen Groß-/Kleinbuchstaben). |
| `datetime` | `{dob:datetime}` | `2016-12-31`, `2016-12-31 7:32pm`  | Für einen gültigen `DateTime`-Wert wird eine Übereinstimmung ermittelt (in der invarianten Kultur; siehe Warnung). |
| `decimal` | `{price:decimal}` | `49.99`, `-1,000.01` | Für einen gültigen `decimal`-Wert wird eine Übereinstimmung ermittelt (in der invarianten Kultur; siehe Warnung). |
| `double` | `{weight:double}` | `1.234`, `-1,001.01e8` | Für einen gültigen `double`-Wert wird eine Übereinstimmung ermittelt (in der invarianten Kultur; siehe Warnung). |
| `float` | `{weight:float}` | `1.234`, `-1,001.01e8` | Für einen gültigen `float`-Wert wird eine Übereinstimmung ermittelt (in der invarianten Kultur; siehe Warnung). |
| `guid` | `{id:guid}` | `CD2C1638-1638-72D5-1638-DEADBEEF1638`, `{CD2C1638-1638-72D5-1638-DEADBEEF1638}` | Für einen gültigen `Guid`-Wert wird eine Übereinstimmung ermittelt. |
| `long` | `{ticks:long}` | `123456789`, `-123456789` | Für einen gültigen `long`-Wert wird eine Übereinstimmung ermittelt. |
| `minlength(value)` | `{username:minlength(4)}` | `Rick` | Die Zeichenfolge muss mindestens eine Länge von 4 Zeichen aufweisen. |
| `maxlength(value)` | `{filename:maxlength(8)}` | `Richard` | Die Zeichenfolge darf maximal eine Länge von 8 Zeichen aufweisen. |
| `length(length)` | `{filename:length(12)}` | `somefile.txt` | Die Zeichenfolge muss genau 12 Zeichen aufweisen. |
| `length(min,max)` | `{filename:length(8,16)}` | `somefile.txt` | Die Zeichenfolge muss mindestens eine Länge von 8 und darf maximal eine Länge von 16 Zeichen aufweisen. |
| `min(value)` | `{age:min(18)}` | `19` | Der Integerwert muss mindestens 18 sein. |
| `max(value)` | `{age:max(120)}` | `91` | Der Integerwert darf nicht größer als 120 sein. |
| `range(min,max)` | `{age:range(18,120)}` | `91` | Der Integerwert muss zwischen 18 und 120 liegen. |
| `alpha` | `{name:alpha}` | `Rick` | Die Zeichenfolge muss aus mindestens einem alphabetische Zeichen bestehen (`a`-`z`, keine Unterscheidung zwischen Groß-/Kleinbuchstaben). |
| `regex(expression)` | `{ssn:regex(^\\d{{3}}-\\d{{2}}-\\d{{4}}$)}` | `123-45-6789` | Die Zeichenfolge muss dem regulären Ausdruck entsprechen (siehe Tipp zum Definieren eines regulären Ausdrucks) |
| `required` | `{name:required}` | `Rick` |  Hierdurch wird erzwungen, dass ein Wert, der kein Parameter ist, für die URL-Generierung vorhanden sein muss. |

Auf einen einzelnen Parameter können mehrere durch Doppelpunkte getrennte Einschränkungen angewendet werden. Durch die folgende Einschränkung wird ein Parameter beispielsweise auf einen Integerwert größer oder gleich 1 beschränkt:

```csharp
[Route("users/{id:int:min(1)}")]
public User GetUserById(int id) { }
```

> [!WARNING]
> Für Routeneinschränkungen, mit denen die URL überprüft wird und die in den CLR-Typ umgewandelt werden (beispielsweise `int` oder `DateTime`), wird immer die invariante Kultur verwendet. Diese Einschränkungen setzen voraus, dass die URL nicht lokalisierbar ist. Die vom Framework bereitgestellten Routeneinschränkungen ändern nicht die Werte, die in Routenwerten gespeichert sind. Alle Routenwerte, die aus der URL analysiert werden, werden als Zeichenfolgen gespeichert. Durch die `float`-Einschränkung wird beispielsweise versucht, den Routenwert in einen Gleitkommawert zu konvertieren. Mit dem konvertierten Wert wird allerdings nur überprüft, ob eine Umwandlung überhaupt möglich ist.

## <a name="regular-expressions"></a>Reguläre Ausdrücke

Im ASP.NET Core-Framework wird dem Konstruktor für reguläre Ausdrücke `RegexOptions.IgnoreCase | RegexOptions.Compiled | RegexOptions.CultureInvariant` hinzugefügt. Eine Beschreibung dieser Member finden Sie unter <xref:System.Text.RegularExpressions.RegexOptions>.

In regulären Ausdrücken werden Trennzeichen und Token verwendet, die auch beim Routing und in der Programmiersprache C# in ähnlicher Weise verwendet werden. Token, die reguläre Ausdrücke enthalten, müssen mit einem Escapezeichen versehen werden. Wenn Sie den regulären Ausdruck `^\d{3}-\d{2}-\d{4}$` für das Routing verwenden möchten, müssen für den Ausdruck die `\`-Zeichen in der C#-Quelldatei in der Form `\\` eingegeben werden, damit die Funktion des Escapezeichens `\` aufgehoben wird (falls keine [ausführlichen Zeichenfolgenliterale](/dotnet/csharp/language-reference/keywords/string) verwendet werden). Die Zeichen `{`, `}`, `[` und `]` werden mit einem Escapezeichen versehen, indem sie verdoppelt werden. Dadurch werden wiederum die Trennzeichen der Routingparameter mit einem Escapezeichen versehen. In der folgenden Tabelle werden reguläre Ausdrücke und Ausdrücke mit den entsprechenden Escapezeichen aufgeführt:

| Ausdruck            | Mit Escapezeichen versehen                        |
| --------------------- | ------------------------------ |
| `^\d{3}-\d{2}-\d{4}$` | `^\\d{{3}}-\\d{{2}}-\\d{{4}}$` |
| `^[a-z]{2}$`          | `^[[a-z]]{{2}}$`               |

Für das Routing verwendete reguläre Ausdrücke beginnen häufig mit dem Zeichen `^` (Anfangsposition der abzugleichenden Zeichenfolge) und enden mit dem Zeichen `$` (Endposition der abzugleichenden Zeichenfolge). Mit den Zeichen `^` und `$` wird sichergestellt, dass der reguläre Ausdruck mit dem vollständigen Routenparameterwert übereinstimmt. Ohne die Zeichen `^` und `$` werden mit dem regulären Ausdruck alle Teilzeichenfolgen ermittelt, was häufig nicht gewünscht ist. In der folgenden Tabelle finden Sie mehrere Beispiele für reguläre Ausdrücke. Außerdem wird erklärt, warum ein Abgleich erfolgreich ist oder fehlschlägt.

| Ausdruck   | Zeichenfolge    | Match | Kommentar               |
| ------------ | --------- |  ---- |  -------------------- |
| `[a-z]{2}`   | hello     | Ja   | Teilzeichenfolge stimmt überein     |
| `[a-z]{2}`   | 123abc456 | Ja   | Teilzeichenfolge stimmt überein     |
| `[a-z]{2}`   | mz        | Ja   | Ausdruck stimmt überein    |
| `[a-z]{2}`   | MZ        | Ja   | keine Unterscheidung zwischen Groß-/Kleinbuchstaben    |
| `^[a-z]{2}$` |  hello    | Nein    | siehe Erläuterungen zu `^` und `$` oben |
| `^[a-z]{2}$` | 123abc456 | Nein    | siehe Erläuterungen zu `^` und `$` oben |

Weitere Informationen zur Syntax von regulären Ausdrücken finden Sie unter [Sprachelemente für reguläre Ausdrücke – Kurzübersicht](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference).

Einen regulären Ausdruck können Sie verwenden, um einen Parameter auf zulässige Werte einzuschränken. Mit `{action:regex(^(list|get|create)$)}` werden beispielsweise für den `action`-Routenwert nur die Werte `list`, `get` oder `create` abgeglichen. Wenn die Zeichenfolge `^(list|get|create)$` dem Einschränkungswörterbuch übergeben wird, führt dies zum gleichen Ergebnis. Auch Einschränkungen, die dem zugehörigen Wörterbuch hinzugefügt werden und mit keiner vorgegebenen Einschränkung übereinstimmen , werden als reguläre Ausdrücke behandelt. Dies gilt allerdings nicht für Inline-Einschränkungen in einer Vorlage.

## <a name="url-generation-reference"></a>Referenz für URL-Generierung

Im folgenden Beispiel wird gezeigt, wie Sie einen Link zu einer Route unter Berücksichtigung eines vorhandenen Wörterbuchs mit Routenwerten und einer <xref:Microsoft.AspNetCore.Routing.RouteCollection> erstellen.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](routing/samples/2.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](routing/samples/1.x/RoutingSample/Startup.cs?name=snippet_Dictionary)]

::: moniker-end

Die `VirtualPath`-Eigenschaft, die am Ende des obigen Beispiels erstellt wird, hat den Wert `/package/create/123`. Das Wörterbuch stellt die Routenwerte von `operation` und `id` aus der Vorlage „Track Package Route“ (`package/{operation}/{id}`) bereit. Weitere Informationen finden Sie im Beispielcode im Abschnitt [Verwenden von Routingmiddleware](#use-routing-middleware) oder in der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/routing/samples).

Der zweite Parameter für den `VirtualPathContext`-Konstruktor ist eine Auflistung von *Umgebungswerten*. Mit diesen lassen sich leicht die Anzahl der Werte einschränken, die ein Entwickler innerhalb eines bestimmten Anforderungskontexts angeben muss. Die aktuellen Routenwerte der aktuellen Anforderung werden bei der Linkgenerierung als Umgebungswerte behandelt. In einer ASP.NET Core MVC-App müssen Sie innerhalb der `About`-Aktion von `HomeController` nicht den Controllerroutenwert angeben, um eine Verknüpfung mit der `Index`-Aktion herzustellen, da der Umgebungswert von `Home` verwendet wird.

Umgebungswerte werden ignoriert, wenn sie keinem Parameter entsprechen oder ein explizit bereitgestellter Wert den Parameter überschreibt (innerhalb der URL von links nach rechts).

Explizit bereitgestellte Werte, für die keine Übereinstimmungen ermittelt werden, werden der Abfragezeichenfolge hinzugefügt. In der folgenden Tabelle werden die Ergebnisse dargestellt, die aus der Verwendung der Routenvorlage `{controller}/{action}/{id?}` hervorgehen:

| Umgebungswerte                | Explizite Werte                   | Ergebnis                  |
| ----------------------------- | --------------------------------- | ----------------------- |
| controller="Home"             | action="About"                    | `/Home/About`           |
| controller="Home"             | controller="Order",action="About" | `/Order/About`          |
| controller="Home",color="Red" | action="About"                    | `/Home/About`           |
| controller="Home"             | action="About",color="Red"        | `/Home/About?color=Red` |

Wenn eine Route über einen Standardwert verfügt, der keinem Parameter entspricht, und dieser Wert explizit bereitgestellt wird, muss er dem Standardwert entsprechen:

```csharp
routes.MapRoute("blog_route", "blog/{*slug}",
    defaults: new { controller = "Blog", action = "ReadPost" });
```

Für diese Route wird nur dann ein Link generiert, wenn die übereinstimmenden Werte für „controller“ und „action“ bereitgestellt werden.
