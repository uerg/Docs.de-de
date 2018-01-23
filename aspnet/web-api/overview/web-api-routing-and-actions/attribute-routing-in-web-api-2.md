---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing in ASP.NET Web API 2-Attribut | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 67ab1536b4a72abf8c0d3ed5aa0c48bc79a8fb5f
ms.sourcegitcommit: 3d512ea991ac36dfd4c800b7d1f8a27bfc50635e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/23/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Routing in ASP.NET Web API 2-Attribut
====================
durch [Mike Wasson](https://github.com/MikeWasson)

*Routing* ist wie Web-API einen URI zu einer Aktion entspricht. Web-API 2 unterstützt einen neuen Typ wird aufgerufen, Routing, *routing-Attribut*. Wie der Name schon sagt, verwendet das routing-Attribut Attribute zum Definieren von Routen. Routing-Attribut bietet Ihnen mehr Kontrolle über die URIs in Ihre Web-API. Beispielsweise können Sie problemlos erstellen URIs, die Hierarchien von Ressourcen zu beschreiben.

Das frühere Format Routing, konventionsbasierte aufgerufen wird routing, weiterhin vollständig unterstützt. Tatsächlich können Sie beide Verfahren im selben Projekt kombinieren.

In diesem Thema wird gezeigt, wie routing-Attribut zu aktivieren und beschreibt die verschiedenen Optionen für das routing-Attribut. Eine End-to-End-Lernprogramm, das routing-Attribut verwendet, finden Sie unter [erstellen Sie eine REST-API mit Routing-Attribut in der Web-API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

[Visual Studio-2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition

Verwenden Sie alternativ NuGet-Paket-Manager zum Installieren der erforderlichen Pakete an. Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie den folgenden Befehl in der Paket-Manager-Konsole aus:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Warum zurückführen Routing?

Der ersten Version von Web-API verwendet *konventionsbasierter* routing. In diesem Typ von routing Sie definieren eine oder mehr routenvorlagen, die im Grunde werden parametrisiert Zeichenfolgen. Wenn das Framework eine Anforderung empfängt, liegt eine Übereinstimmung mit den URI für die routenvorlage. (Weitere Informationen zum routing konventionsbasierte finden Sie unter [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).

Ein Vorteil von routing konventionsbasierte ist, dass die Vorlagen werden an einem Ort definiert und die Senderegeln konsistent über alle Controller angewendet werden. Leider ist routing konventionsbasierte schwer zu bestimmten URI-Muster unterstützt, die häufig in Rest-APIs sind. Beispielsweise Ressourcen häufig untergeordneten Ressourcen enthalten: Kunden Bestellungen haben, Filme Akteure, Bücher weisen Autoren und usw. Es ist zum Erstellen von URIs, die diese Beziehungen widerspiegeln natürliche:

`/customers/1/orders`

Dieser URI ist schwierig, mithilfe von routing konventionsbasierte zu erstellen. Obwohl es möglich ist, skalieren nicht die Ergebnisse, gut, wenn Sie viele Domänencontroller oder Ressourcentypen zur Verfügung haben.

Mit routing-Attribut ist es triviale so definieren Sie eine Route für diesen URI. Ein Attribut wird einfach an die Controlleraktion hinzufügen:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Hier sind einige weitere Muster, die einfach routing macht-Attribut ein.

**API-versionsverwaltung**

In diesem Beispiel "/ api/v1/Products" wäre an einen anderen Controller als "/ api/v2/Products".

`/api/v1/products`  
`/api/v2/products`

**Überladene URI-Segmente**

In diesem Beispiel "1" ist eine Auftragsnummer "Ausstehend" ordnet einer Auflistung.

`/orders/1`  
`/orders/pending`

**Mehrere Parametertypen**

In diesem Beispiel "1" ist eine Auftragsnummer ein, aber "2013/06/16" gibt ein Datum an.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Aktivieren der Routing-Attribut

Rufen Sie zum Aktivieren der routing-Attribut **MapHttpAttributeRoutes** während der Konfiguration. Diese Erweiterungsmethode ist definiert, der **System.Web.Http.HttpConfigurationExtensions** Klasse.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Routing-Attribut kann zusammen mit [konventionsbasierter](routing-in-aspnet-web-api.md) routing. Rufen Sie zum Definieren von Routen konventionsbasierte der **MapHttpRoute** Methode.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Weitere Informationen zum Konfigurieren von Web-API finden Sie unter [Konfigurieren von ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Hinweis: Migrieren von Web-API 1

Vor der Web-API 2 generiert die Projektvorlagen für Web-API-Code wie folgt:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Wenn routing-Attribut aktiviert ist, wird dieser Code eine Ausnahme ausgelöst. Wenn Sie ein vorhandenes Web-API-Projekt zur Verwendung von routing-Attribut aktualisieren, stellen Sie sicher, dieser Konfigurationscode folgt aktualisieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Weitere Informationen finden Sie unter [Konfigurieren des Web-API mit ASP.NET-Hosting](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Hinzufügen von Routenattributen

Hier ist ein Beispiel für eine Route mit einem Attribut definiert:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Die Zeichenfolge &quot;Kunden / {CustomerId} / orders&quot; ist die URI-Vorlage für die Route. Web-API versucht, die im Anforderungs-URI der Vorlage übereinstimmen. In diesem Beispiel "Customers" und "Orders" literal Segmente sind, und "{CustomerId}" ist eine Variable-Parameter. Die folgenden URIs würde diese Vorlage übereinstimmen:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Sie können einschränken, den Datenabgleich mithilfe [Einschränkungen](#constraints), weiter unten in diesem Thema beschrieben.

Beachten Sie, dass die &quot;{CustomerId}&quot; Parameter in der routenvorlage entspricht dem Namen der *CustomerId* Parameter der Methode. Beim Web-API-Controlleraktion aufgerufen wird, wird versucht, die Routenparameter zu binden. Angenommen, wenn der URI `http://example.com/customers/1/orders`, Web-API versucht, den Wert "1" zu binden, um die *CustomerId* Parameter in der Aktion.

Eine URI-Vorlage kann mehrere Parameter haben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Alle Domänencontroller-Methoden, die nicht über eine Route-Attribut verfügen verwenden konventionsbasierte routing. Auf diese Weise können Sie beide Arten von routing im selben Projekt kombinieren.

## <a name="http-methods"></a>HTTP-Methoden

Web-API wählt auch Aktionen, die auf der Grundlage der HTTP-Methode der Anforderung (GET, POST usw.). Standardmäßig sucht Web-API für die Groß-/Kleinschreibung Übereinstimmung mit dem Start des Methodennamens Controller. Z. B. einen Controllermethode, die mit dem Namen `PutCustomers` eine HTTP PUT-Anforderung übereinstimmt.

Sie können diese Konvention überschreiben werden, indem die Methode mit die folgenden Attributen:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Das folgende Beispiel ordnet die CreateBook-Methode für HTTP POST-Anforderungen.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Für alle anderen HTTP-Methoden, einschließlich der nicht standardmäßige Methoden verwenden das **AcceptVerbs** Attribut, das eine Liste der HTTP-Methoden behandelt.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Routenpräfixe

Häufig, um die Routen in einem Controller alle mit dem gleichen Präfix beginnen. Zum Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Sie können ein gemeinsames Präfix für einen gesamten Controller festlegen, mit der **[Routenpräfix]** Attribut:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Verwenden Sie eine Tilde (~) für das Attribut für die Methode, um das Routenpräfix überschreiben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Das Routenpräfix kann Parameter enthalten:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Routeneinschränkungen

Routeneinschränkungen können Sie einschränken, wie die Parameter in der routenvorlage abgeglichen werden. Die allgemeine Syntax lautet &quot;{Parameter: Einschränkung}&quot;. Zum Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Hier werden die ersten Route wird nur ausgewählt, wenn die &quot;Id&quot; die URI-Segment ist eine ganze Zahl. Andernfalls wird die zweite Route ausgewählt.

Die folgende Tabelle enthält die Einschränkungen, die unterstützt werden.

| Constraint | Beschreibung | Beispiel |
| --- | --- | --- |
| Alpha | Übereinstimmungen Groß- oder Kleinbuchstaben Zeichen des lateinischen Alphabets (a-Z, A-Z) | {x:alpha} |
| bool | Entspricht einem booleschen Wert. | {x:bool} |
| datetime | Entspricht einem **"DateTime"** Wert. | {x:datetime} |
| decimal | Entspricht einen decimal-Wert. | {x:decimal} |
| double | Entspricht einer 64-Bit-Gleitkommawert. | {x:double} |
| float | Entspricht einem 32-Bit-Gleitkommawert. | {x:float} |
| guid | Entspricht einen GUID-Wert. | {x:guid} |
| int | Entspricht einem 32-Bit-Ganzzahl-Wert. | {x:int} |
| Länge | Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines bestimmten Bereichs Länge. | {x:length(6)} {x:length(1,20)} |
| long | Entspricht einem 64-Bit-Ganzzahl-Wert. | {x:long} |
| max | Entspricht einer ganzen Zahl mit einem Maximalwert. | {x:max(10)} |
| MaxLength | Entspricht einer Zeichenfolge mit einer maximalen Länge. | {X: maxlength(10)} |
| Min. | Entspricht einer ganzen Zahl mit einem Mindestwert. | {x:min(10)} |
| "minLength" | Mit einer Zeichenfolge mit einer minimalen Länge überein. | {X: minlength(10)} |
| range | Entspricht einer ganzen Zahl innerhalb eines Bereichs von Werten. | {x:range(10,50)} |
| regex | Mit einem regulären Ausdruck übereinstimmt. | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

Beachten Sie, dass einige Einschränkungen, wie z. B. &quot;min&quot;, Argumente in Klammern. Sie können mehrere Einschränkungen auf einen Parameter, die durch einen Doppelpunkt getrennt anwenden.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Benutzerdefinierte Routeneinschränkungen

Sie können benutzerdefinierte routeneinschränkungen erstellen, durch die Implementierung der **IHttpRouteConstraint** Schnittstelle. Die folgende Einschränkung schränkt z. B. einen Parameter auf einen Wert ungleich NULL ganze Zahl an.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Der folgende Code zeigt, wie Sie die Einschränkung zu registrieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Jetzt können Sie die Einschränkung in Ihre Routen anwenden:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Sie können auch die gesamte ersetzen **DefaultInlineConstraintResolver** Klasse durch Implementieren der **IInlineConstraintResolver** Schnittstelle. Auf diese Weise wird ersetzt alle integrierten Einschränkungen, es sei denn, Ihre Implementierung von **IInlineConstraintResolver** speziell hinzugefügt.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Optionale URI-Parameter und Standardwerte

Sie können einen URI-Parameter optional machen, indem ein Fragezeichen routenparameters hinzufügen. Wenn ein Routenparameter optional ist, müssen Sie einen Standardwert für den Methodenparameter definieren.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In diesem Beispiel `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurückgegeben.

Alternativ können Sie einen Standardwert in der routenvorlage wie folgt angeben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Dies ist fast identisch mit dem vorherigen Beispiel, jedoch ist es ein geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.

- Im ersten Beispiel ("{Lcid?}") ist der Standardwert 1033 an, direkt an der Methodenparameter, zugewiesen, damit die Parameter dieser genauen Wert hat.
- Im zweiten Beispiel ("{Lcid = 1033}"), der Wert von "1033" durchläuft die modellbindung. Der Standardmodellbinder werden in den numerischen Wert 1033 "1033" konvertiert. Sie können jedoch in einen benutzerdefinierten Modellbinder anschließen die etwas anderes sind möglicherweise.

(In den meisten Fällen, wenn Sie benutzerdefinierte Modellbinder in Ihrer Pipeline haben die beiden Formen entsprechende werden.)

<a id="route-names"></a>
## <a name="route-names"></a>Routennamen

In der Web-API hat jede Route einen Namen an. Routennamen eignen sich zum Generieren von Links, sodass Sie einen Link in einer HTTP-Antwort enthalten können.

Der Routenname, legen die **Namen** -Eigenschaft des Attributs. Im folgenden Beispiel wird veranschaulicht, wie den Routennamen festlegen sowie den Routennamen verwenden, wenn einen Link zu generieren.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Routenreihenfolge

Wenn das Framework versucht, einen URI mit einer Route übereinstimmen, wird er die Routen in einer bestimmten Reihenfolge ausgewertet. Um die Reihenfolge anzugeben, legen die **RouteOrder** -Eigenschaft für die routenattribut. Niedrigere Werte werden zuerst ausgewertet. Der standardreihenfolgenwert ist 0 (null).

Hier wird die gesamte Sortierung festgelegt:

1. Vergleichen der **RouteOrder** -Eigenschaft des Attributs Route.
2. Betrachten Sie jede URI-Segment in der routenvorlage aus. Bestellen Sie für jedes Segment wie folgt aus: 

    1. Literal-Segmente.
    2. Routenparameter mit Einschränkungen.
    3. Routenparameter ohne Einschränkungen.
    4. Parameter-platzhaltersegmente mit Einschränkungen.
    5. Parameter-platzhaltersegmente ohne Einschränkungen.
3. Bei einem gleichwertigen Objekt werden Routen nach einem Ordinalzeichenfolgenvergleich sortiert ([OrdinalIgnoreCase](https://msdn.microsoft.com/en-us/library/system.stringcomparer.ordinalignorecase.aspx)) für die routenvorlage.

Im Folgenden ein Beispiel. Nehmen Sie an, dass Sie die folgenden Controller definieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Mit diesen Routen werden folgendermaßen sortiert.

1. Aufträge/details
2. Aufträge / {Id}
3. orders/{customerName}
4. Aufträge / {\*Date}
5. Aufträge / ausstehend

Beachten Sie, dass "Details" ein literal Segment ist und vor der "{Id}" wird angezeigt, aber "Ausstehend" wird, zuletzt angezeigt da die **RouteOrder** Eigenschaft ist 1. (In diesem Beispiel wird vorausgesetzt, dass sind keine Kunden mit dem Namen "Details" oder "Ausstehend". Im Allgemeinen versucht, mehrdeutige Routen zu vermeiden. In diesem Beispiel eine bessere routenvorlage für `GetByCustomer` ist "Kunden / {Kundenname}")
