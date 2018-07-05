---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attributrouting in der ASP.NET Web API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 52164fde1a2277bb9ef07f2a5476ca78b71a8e52
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802756"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>Attribut-Routing in ASP.NET Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

*Routing* ist wie Web-API einen URI zu einer Aktion entspricht. Web-API 2 unterstützt einen neuen Typ Namens des Routings, *attributrouting*. Wie der Name schon sagt, wird das attributrouting Attribute zum Definieren von Routen verwendet. Attribut-routing bietet Ihnen mehr Kontrolle über die URIs in Ihrer Web-API. Beispielsweise können Sie ganz einfach erstellen URIs, die Hierarchien von Ressourcen beschreiben.

Das frühere Format des Routings, aufgerufen wird, auf Konventionen basierendes wird routing weiterhin vollständig unterstützt. In der Tat können Sie beide Verfahren im selben Projekt kombinieren.

In diesem Thema wird gezeigt, wie das attributrouting aktivieren und beschreibt die verschiedenen Optionen für die Attribut-routing. Ein End-to-End-Lernprogramm, das das attributrouting verwendet, finden Sie unter [erstellen Sie eine REST-API mit Attributrouting in der Web-API 2](create-a-rest-api-with-attribute-routing.md).


## <a name="prerequisites"></a>Erforderliche Komponenten

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition

Alternativ verwenden Sie NuGet-Paket-Manager, um die erforderlichen Pakete zu installieren. Von der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>Warum Attributrouting?

Die erste Version von Web-API verwendet *auf Konventionen basierendes* routing. In diesem Typ des Routings Sie definieren eine oder weitere routenvorlagen, die im Wesentlichen sind parametrisierte Zeichenfolgen aus. Wenn das Framework eine Anforderung empfängt, liegt eine Übereinstimmung der URI für die routenvorlage mit. (Weitere Informationen zum konventionsbasierten routing finden Sie unter [Routing in ASP.NET Web-API](routing-in-aspnet-web-api.md).

Ein Vorteil des konventionsbasierten Routings ist, dass Vorlagen in einem einzigen Ort definiert sind, und die Routingregeln werden konsistent auf alle Controller angewendet. Leider ist das konventionsbasierte routing schwer zu bestimmten URI-Muster unterstützen, die in die RESTful-APIs gelten. Beispielsweise Ressourcen häufig untergeordnete Ressourcen enthalten: Kunden Bestellungen, Filme haben, Actors, Bücher, Autoren von Steuerelementen und so weiter. Es ist normal, Erstellen von URIs, die diese Beziehungen widerspiegeln:

`/customers/1/orders`

Dieser Typ von URI ist schwierig, erstellen Sie mithilfe der Konventionen basierten routing. Obwohl es möglich ist, skalieren nicht die Ergebnisse, gut, wenn Sie viele Controller oder Ressourcentypen zur Verfügung haben.

Das attributrouting ist es einfach, eine Route für diesen URI zu definieren. Sie fügen einfach ein Attribut, um die Controlleraktion:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

Hier sind einige andere Muster, die Attribut einfach routing macht.

**API-versionsverwaltung**

In diesem Beispiel "/ api/v1/Produkte" wäre weitergeleitet, auf einen anderen Controller als "/ api/v2/Produkte".

`/api/v1/products`  
`/api/v2/products`

**Überladene URI-Segmente**

In diesem Beispiel ist "1" ist eine Bestellnummer, aber "Ausstehend" ist einer Sammlung zugeordnet.

`/orders/1`  
`/orders/pending`

**Mehrere Parametertypen**

In diesem Beispiel ist "1" ist eine Bestellnummer, aber "2013/06/16" gibt ein Datum an.

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>Aktivieren der Attribut-Routing

Rufen Sie zum Aktivieren der Attribut-routing **MapHttpAttributeRoutes** während der Konfiguration. Diese Erweiterungsmethode wird definiert, der **System.Web.Http.HttpConfigurationExtensions** Klasse.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

Beim attributrouting kann kombiniert werden, mit [auf Konventionen basierendes](routing-in-aspnet-web-api.md) routing. Rufen Sie zum Definieren von Routen auf Konventionen basierendes der **"maphttproute"** Methode.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Weitere Informationen zum Konfigurieren von Web-API finden Sie unter [Konfigurieren von ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>Hinweis: Die Migration von Web-API 1

Vor der Web-API 2 generiert die Projektvorlagen für Web-API-Code wie folgt:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

Wenn die Attribut-routing aktiviert ist, wird dieser Code eine Ausnahme ausgelöst. Wenn Sie ein vorhandenes Web-API-Projekt zu verwenden, das attributrouting aktualisieren, müssen Sie dieser Konfigurationscode folgt aktualisieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> Weitere Informationen finden Sie unter [Konfigurieren des Web-API mit Hosten von ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>Hinzufügen von Routenattributen

Hier ist ein Beispiel für eine Route mit einem Attribut definiert:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

Die Zeichenfolge &quot;Customers / {CustomerId} / Bestellungen&quot; ist die URI-Vorlage für die Route. Web-API versucht, die im Anforderungs-URI der Vorlage übereinstimmen. In diesem Beispiel "Customers" und "Orders" sind literal-Segmente, und "{CustomerId}" ist eine Variable-Parameter. Die folgenden URIs entsprechen diese Vorlage:

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

Sie können einschränken, den entsprechenden mit [Einschränkungen](#constraints), die weiter unten in diesem Thema beschrieben.

Beachten Sie, dass die &quot;{CustomerId}&quot; Parameter in der routenvorlage übereinstimmt, den Namen der *"CustomerID"* Parameter in der Methode. Wenn die Web-API die Controlleraktion aufgerufen wird, versucht, die die Routenparameter zu binden. Wenn der URI ist z. B. `http://example.com/customers/1/orders`, Web-API versucht, den Wert "1" zu binden, um die *"CustomerID"* Parameter in der Aktion.

Eine URI-Vorlage kann mehrere Parameter haben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

Alle Methoden des Controllers, die nicht über eine Route-Attribut verfügen verwenden konventionsbasierten routing. Auf diese Weise können Sie beide Arten von routing im selben Projekt kombinieren.

## <a name="http-methods"></a>HTTP-Methoden

Web-API wählt auch Aktionen basierend auf den HTTP-Methode der Anforderung (GET, POST usw.). Standardmäßig sucht die Web-API für die Groß-/Kleinschreibung Übereinstimmung mit dem Anfang der Name der Controller-Methode. Z. B. eine Controllermethode, die mit dem Namen `PutCustomers` eine HTTP PUT-Anforderung übereinstimmt.

Sie können diese Konvention überschreiben werden, indem die Methode mit einem der folgenden Attribute:

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **["HttpOptions"]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

Im folgende Beispiel ordnet die CreateBook-Methode auf HTTP-POST-Anforderungen.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

Für alle anderen HTTP-Methoden, einschließlich der nicht standardmäßige Methoden verwenden die **AcceptVerbs** -Attribut, das eine Liste von HTTP-Methoden akzeptiert.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>Routenpräfixe

Häufig die Routen in einem Controller, die alle mit dem gleichen Präfix beginnen. Zum Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

Sie können ein gemeinsames Präfix für einen gesamten Controller festlegen, mit der **[RoutePrefix]** Attribut:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

Verwenden Sie eine Tilde (~) auf das Method-Attribut, um das Routenpräfix außer Kraft setzen:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

Das Routenpräfix kann Parameter enthalten:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>Einschränkungen

Einschränkungen können Sie einschränken, wie die Parameter in der routenvorlage abgeglichen werden. Die allgemeine Syntax lautet &quot;: {parametereinschränkungen}&quot;. Zum Beispiel:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

Hier werden die erste Route wird nur ausgewählt, wenn die &quot;Id&quot; -Segment des URI ist eine ganze Zahl. Andernfalls wird die zweite Route ausgewählt.

Die folgende Tabelle enthält die Einschränkungen, die unterstützt werden.

| Constraint | Beschreibung | Beispiel |
| --- | --- | --- |
| Alpha | Übereinstimmungen in Großbuchstaben oder Kleinbuchstaben Zeichen des lateinischen Alphabets (a-Z, A-Z) | {x:alpha} |
| bool | Entspricht einem booleschen Wert. | {x:bool} |
| datetime | Entspricht einem **"DateTime"** Wert. | {x:datetime} |
| decimal | Entspricht einem decimal-Wert. | {x:decimal} |
| double | Entspricht einen 64-Bit-Gleitkommawert. | {x:double} |
| float | Entspricht einen 32-Bit-Gleitkommawert. | {x:float} |
| guid | Entspricht einem GUID-Wert. | {x:guid} |
| int | Entspricht einem 32-Bit-Ganzzahl-Wert. | {x:int} |
| Länge | Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines bestimmten Bereichs Länge. | {X: length(6)} {X: length(1,20)} |
| long | Entspricht einem 64-Bit-Ganzzahl-Wert. | {x:long} |
| max | Entspricht einer ganzen Zahl mit einem maximalen Wert. | {x:max(10)} |
| MaxLength | Eine Zeichenfolge mit einer maximalen Länge entspricht. | {X: maxlength(10)} |
| Min. | Entspricht einer ganzen Zahl mit einem Mindestwert. | {X: min(10)} |
| MinLength | Entspricht einer Zeichenfolge mit einer Mindestlänge. | {X: minlength(10)} |
| range | Entspricht einer ganzen Zahl in einem Bereich von Werten. | {X: range(10,50)} |
| regex | Mit einem regulären Ausdruck übereinstimmt. | {X: regex(^\d{3}-\d{3}-\d{4}$)} |

Beachten Sie, dass einige der Einschränkungen, wie z. B. &quot;min&quot;, akzeptieren Sie die Argumente in Klammern. Sie können mehrere Einschränkungen auf einen Parameter durch einen Doppelpunkt getrennt anwenden.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>Benutzerdefinierte Route-Einschränkungen

Sie können benutzerdefinierte routeneinschränkungen erstellen, durch die Implementierung der **IHttpRouteConstraint** Schnittstelle. Die folgende Einschränkung schränkt z.B. einen Parameter auf einen Wert ungleich NULL ganze Zahl.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

Der folgende Code zeigt, wie Sie die Einschränkung zu registrieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

Jetzt können Sie die Einschränkung in Ihre Routen anwenden:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

Sie können auch die gesamte ersetzen **DefaultInlineConstraintResolver** Klasse durch die Implementierung der **IInlineConstraintResolver** Schnittstelle. Auf diese Weise ersetzt die integrierte Einschränkungen, es sei denn, Ihre Implementierung von **IInlineConstraintResolver** speziell hinzugefügt.

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>Optionaler URI-Parameter mit Standardwerten

Sie können einen URI-Parameter optional konfigurieren, indem Sie ein Fragezeichen routenparameters hinzufügen. Wenn ein Routenparameter optional ist, müssen Sie einen Standardwert für den Methodenparameter definieren.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

In diesem Beispiel `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurückgegeben.

Alternativ können Sie einen Standardwert in die routenvorlage, wie folgt angeben:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

Dies ist fast identisch mit dem vorherigen Beispiel, aber es wird ein geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.

- Im ersten Beispiel ("{Lcid?}") wird standardmäßig den Wert 1033 direkt der Methodenparameter, zugewiesen hat also der Parameter dieses genauen Wert.
- Im zweiten Beispiel ("{Lcid = 1033}"), der Standardwert von "1033" durchläuft die modellbindung. Der Standardmodellbinder werden auf den numerischen Wert 1033 "1033" konvertiert. Allerdings können Sie in einen benutzerdefinierten Modellbinder, einfügen, die etwas anderes erledigen kann.

(In den meisten Fällen, es sei denn, Sie benutzerdefinierte modellbindungen in Ihrer Pipeline, werden die beiden Formen entsprechen.)

<a id="route-names"></a>
## <a name="route-names"></a>Routennamen

In der Web-API hat jede Route einen Namen an. Routennamen eignen sich zum Generieren von Links, damit Sie einen Link in einer HTTP-Antwort einschließen können.

Der Routenname, legen die **Namen** -Eigenschaft des Attributs. Das folgende Beispiel zeigt, wie der Routenname festgelegt, und wie der Routenname zu verwenden, wenn Sie einen Link zu generieren.

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>Reihenfolge

Wenn Sie das Framework versucht, die einen URI mit einer Route übereinstimmen, wird die Routen in einer bestimmten Reihenfolge ausgewertet. Legen Sie zum Angeben der Reihenfolge der **RouteOrder** Eigenschaft für die Route-Attribut. Niedrigere Werte werden zuerst ausgewertet. Der Wert der Standardreihenfolge ist 0 (null).

Hier ist wie der gesamten Sortierung bestimmt wird:

1. Vergleichen Sie die **RouteOrder** -Eigenschaft des Attributs Route.
2. Sehen Sie sich jede URI-Segment in der routenvorlage. Bestellen Sie für die einzelnen Segmente wie folgt: 

    1. Literal-Segmente.
    2. Der Routenparameter mit Einschränkungen.
    3. Der Routenparameter ohne Einschränkungen.
    4. Parameter-platzhaltersegmente mit Einschränkungen.
    5. Parameter-platzhaltersegmente ohne Einschränkungen.
3. Routen werden im Fall von einem gleichwertigen Objekt ist durch einen Ordinalzeichenfolgenvergleich sortiert ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) von der routenvorlage.

Im Folgenden ein Beispiel. Nehmen wir an, dass Sie den folgenden Controller definieren:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

Diese Routen werden wie folgt sortiert.

1. Aufträge/details
2. Aufträge / {Id}
3. Aufträge / {CustomerName}
4. Aufträge / {\*Datum}
5. Aufträge / ausstehend

Beachten Sie, dass "Details" ein literal-Segment ist und vor "{Id}" angezeigt wird, aber "Ausstehend" angezeigt wird: zuletzt die **RouteOrder** -Eigenschaft ist 1. (In diesem Beispiel wird vorausgesetzt, dass sind keine Kunden namens "Details" oder "Ausstehend". Versuchen Sie es im Allgemeinen zu mehrdeutige Routen zu vermeiden. In diesem Beispiel für eine bessere routenvorlage `GetByCustomer` ist "Customers / {CustomerName}")
