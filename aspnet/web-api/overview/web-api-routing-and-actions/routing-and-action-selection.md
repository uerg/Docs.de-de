---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing- und Aktionsauswahl in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/27/2012
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: b4912d3ee1e13651f2a63d54d7dbfd92e00f85f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838988"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing- und Aktionsauswahl in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API eine HTTP-Anforderung an eine bestimmte Aktion für einen Controller weitergeleitet.

> [!NOTE]
> Eine allgemeine Übersicht über routing, finden Sie unter [Routing in ASP.NET Web-API](routing-in-aspnet-web-api.md).


Dieser Artikel behandelt die Details der routing-Prozess. Wenn Sie ein Web-API-Projekt erstellen, und suchen, die einige Anforderungen werden nicht weitergeleitet, die Möglichkeit, die Sie erwarten, hilft dieser Artikel hoffentlich.

Routing verfügt über drei Hauptphasen aus:

1. Mit den URI, der eine routenvorlage übereinstimmen.
2. Wählen einen Controller.
3. Auswählen einer Aktion.

Sie können einige Teile des Prozesses durch Ihre eigenen benutzerdefinierten Verhalten ersetzen. In diesem Artikel beschreibe ich das Standardverhalten. Beachten Sie am Ende ich die Stellen, in dem Sie das Verhalten anpassen können.

## <a name="route-templates"></a>Routenvorlagen

Eine routenvorlage ähnelt einem URI-Pfad, aber es kann Platzhalterwerte, mit geschweiften Klammern angegeben haben:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Wenn Sie eine Route erstellen, können Sie die Standardwerte für einige oder alle Platzhalter angeben:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Sie können auch Einschränkungen angeben, die eingeschränkt wie einen Platzhalter mit einer URI-Segment verglichen werden kann:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Das Framework versucht, die die Segmente im URI-Pfad der Vorlage übereinstimmen. Literale in der Vorlage müssen genau übereinstimmen. Ein Platzhalter entspricht ein beliebiger Wert, es sei denn, Sie Einschränkungen angeben. Das Framework stimmt nicht mit anderen Teilen des URIS, wie z. B. den Hostnamen oder die Abfrageparameter überein. Das Framework wählt die erste Route in der Routingtabelle, die den URI entspricht.

Es gibt zwei spezielle Platzhalter: "{Controller}" und "{Action}".

- "{Controller}" enthält den Namen des Controllers.
- "{Action}" enthält den Namen der Aktion. Die übliche Konvention werden in der Web-API weglassen von "{Action}".

### <a name="defaults"></a>der Arbeitszeittabelle

Wenn Sie Standardwerte angeben, entspricht die Route einen URI, der diese Segmente nicht vorhanden ist. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Der URI "`http://localhost/api/products`" diese Route. Das Segment "{Category}" wird den Standardwert "alle" zugewiesen.

### <a name="route-dictionary"></a>Routenwörterbuchs

Wenn das Framework eine Übereinstimmung für einen URI findet, wird ein Wörterbuch, das den Wert für jeden Platzhalter enthält. Die Schlüssel sind die Platzhalternamen, nicht einschließlich der geschweiften Klammern. Die Werte stammen aus der URI-Pfad oder die Standardwerte. Das Wörterbuch befindet sich in der **IHttpRouteData** Objekt.

Während dieser Phase übereinstimmende Route sind die speziellen "{Controller}" und "{Action}" Platzhalter wie die anderen Platzhalter behandelt. Sie werden einfach in das Wörterbuch mit den anderen Werten gespeichert.

Ein Standardwert kann den speziellen Wert ist **RouteParameter.Optional**. Wenn ein Platzhalter, diesen Wert zugewiesen wird, wird der Wert des Routenwörterbuchs nicht hinzugefügt. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Für den URI-Pfad "api/Produkte" enthält des Routenwörterbuchs Folgendes:

- Controller: "Produkte"
- Kategorie: "all"

Für "api/Produkte/Toys/123" enthält jedoch des Routenwörterbuchs:

- Controller: "Produkte"
- Kategorie: "Toys"
- id: "123"

Die Standardwerte können auch einen Wert, der nicht, eine beliebige Stelle angezeigt wird in der routenvorlage enthalten. Wenn die Route übereinstimmt, wird dieser Wert im Wörterbuch gespeichert. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Wenn der URI-Pfad "api/Root/8" ist, wird das Wörterbuch zwei Werte enthalten:

- Controller: "Customers"
- ID: "8"

## <a name="selecting-a-controller"></a>Auswählen eines Controllers

Auswahl der Domänencontroller erfolgt durch die **IHttpControllerSelector.SelectController** Methode. Diese Methode akzeptiert eine **HttpRequestMessage** -Instanz und gibt eine **HttpControllerDescriptor**. Die standardmäßige Implementierung erfolgt über die **DefaultHttpControllerSelector** Klasse. Diese Klasse wird einen einfachen Algorithmus verwendet:

1. Suchen Sie in das Wörterbuch der Route für den Schlüssel "Controller".
2. Der Wert für diesen Schlüssel, und fügen Sie die Zeichenfolge "Controller", um den Namen des Controllers erhalten.
3. Suchen Sie nach einem Web-API-Controller mit diesem Namen geben.

Wenn das Route-Wörterbuch der Schlüssel-Wert-Paar "Controller" enthält = "Produkte", z. B. wird der Typ des Controllers "ProductsController". Wenn keine übereinstimmenden Typ oder mehrere Übereinstimmungen vorhanden ist, gibt das Framework einen Fehler an den Client zurück.

Schritt 3 **DefaultHttpControllerSelector** verwendet die **IHttpControllerTypeResolver** -Schnittstelle zum Abrufen der Liste der Web-API-Controller-Typen. Die standardmäßige Implementierung des **IHttpControllerTypeResolver** gibt alle öffentlichen Klassen, die (a) implementieren **IHttpController**, (b) werden nicht abstrahieren und (c) verfügen über einen Namen, die "Controller" enden.

## <a name="action-selection"></a>Aktionsauswahl

Nach der Auswahl des Controllers an, das Framework wählt die Aktion durch Aufrufen der **IHttpActionSelector.SelectAction** Methode. Diese Methode akzeptiert eine **HttpControllerContext** und gibt eine **HttpActionDescriptor**.

Die standardmäßige Implementierung erfolgt über die **ApiControllerActionSelector** Klasse. Um eine Aktion auswählen, sieht es unter dem folgenden:

- Die HTTP-Methode der Anforderung.
- Der Platzhalter "{Action}" in der routenvorlage, falls vorhanden.
- Die Parameter der Aktionen auf dem Controller.

Vor dem Betrachten des Auswahlalgorithmus, müssen wir einige Dinge zu Controlleraktionen zu verstehen.

**Welche Methoden auf dem Controller "Aktionen" gelten?** Wenn Sie eine Aktion auswählen, sucht das Framework nur auf Öffentliche Instanzenmethoden auf dem Controller. Darüber hinaus schließt er ["spezielle Name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) Methoden (Konstruktoren, Ereignisse, operatorüberladungen und So weiter) und von geerbten Methoden der **ApiController** Klasse.

**HTTP-Methoden.** Das Framework wählt nur die Aktionen, die die HTTP-Methode der Anforderung wie folgt bestimmt entsprechen:

1. Sie können die HTTP-Methode mit einem Attribut angeben: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **"HttpOptions"**, **HttpPatch**, **HttpPost**, oder **HttpPut**.
2. Andernfalls, wenn der Name der Controller-Methode mit "Get", "Post", "Put", "Delete", "Head", "Optionen" oder "Patch" gestartet wird, klicken Sie dann gemäß der Konvention die Aktion unterstützt diese HTTP-Methode.
3. Wenn keine der oben genannten, unterstützt die Methode POST aus.

**Der Parameterbindungen.** Eine parameterbindung ist wie Web-API auf einen Wert für einen Parameter erstellt wird. Hier ist die Standardregel für die parameterbindung:

- Einfache Typen stammen aus dem URI.
- Komplexe Typen stammen aus dem Anforderungstext.

Einfache Typen umfassen alle der [.NET Framework-primitive Typen](https://msdn.microsoft.com/library/system.type.isprimitive), plus **"DateTime"**, **Decimal**, **Guid**, **Zeichenfolge** , und **TimeSpan**. Für jede Aktion kann höchstens einen Parameter den Anforderungstext lesen.

> [!NOTE]
> Es ist möglich, die Regeln für die Bindung zu überschreiben. Finden Sie unter [WebAPI parameterbindung im Hintergrund](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Mit diesem Hintergrund sieht der Auswahlalgorithmus für die Aktion aus.

1. Erstellen Sie eine Liste mit allen Aktionen auf dem Controller an, die die HTTP-Anforderungsmethode entsprechen.
2. Wenn des Routenwörterbuchs einen Eintrag "Action" aufweist, entfernen Sie Aktionen, deren Name nicht mit diesem Wert übereinstimmt.
3. Versuchen Sie es, Action-Parameter an den URI, folgendermaßen an: 

    1. Erhalten Sie für jede Aktion eine Liste mit den Parametern, die einen einfachen Typ, sind, in dem die Bindung ruft den Parameter aus dem URI. Schließen Sie optionale Parameter.
    2. Versuchen Sie, eine Übereinstimmung mit jeder Parametername in des Routenwörterbuchs oder in der URI-Abfragezeichenfolge zu suchen, aus dieser Liste. Übereinstimmungen werden Groß-/Kleinschreibung und hängen nicht von der Reihenfolge der Parameter.
    3. Wählen Sie eine Aktion aus, in dem alle Parameter in der Liste eine Übereinstimmung im URI verfügt.
    4. Wenn mehr, eine Aktion diese Kriterien erfüllt, wählen Sie die Woche mit der meisten Parameter übereinstimmt.
4. Ignorieren Sie Aktionen mit der **[NonAction]** Attribut.

Schritt #3 ist die am häufigsten verwirrend. Die grundlegende Idee ist, dass ein Parameter den Wert entweder aus dem URI, aus dem Anforderungstext oder aus einer benutzerdefinierten Bindung abrufen kann. Für Parameter, die aus dem URI enthalten sind, möchten wir sicherstellen, dass der URI einen Wert für diesen Parameter, entweder im Pfad (über die Route-Wörterbuch) oder in der Abfragezeichenfolge enthält.

Betrachten Sie beispielsweise die folgende Aktion aus:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Die *Id* Parameter bindet an den URI. Diese Aktion kann daher nur einen URI übereinstimmen, die einen Wert für "Id", im Wörterbuch Route oder in der Abfragezeichenfolge enthält.

Optionale Parameter sind eine Ausnahme aus, da sie optional sind. Für einen optionalen Parameter ist es in Ordnung Wenn die Bindung den Wert aus dem URI nicht.

Komplexe Typen stellen eine Ausnahme für einen anderen Grund. Ein komplexer Typ kann nur über eine benutzerdefinierte Bindung an den URI binden. Aber in diesem Fall das Framework darf nicht bereits im Voraus wissen, ob der Parameter an einen bestimmten URI gebunden würde. Sie müssen herausfinden, rufen Sie die Bindung. Das Ziel der Auswahlalgorithmus ist der statische Beschreibung vor dem Aufrufen von Bindungen eine Aktion aus. Aus diesem Grund werden komplexe Typen aus den entsprechenden Algorithmus ausgeschlossen.

Nachdem die Aktion ausgewählt wurde, werden alle parameterbindungen aufgerufen.

Zusammenfassung:

- Die Aktion muss die HTTP-Methode der Anforderung übereinstimmen.
- Der Name der Aktion muss den "Action"-Eintrag im Wörterbuch Route vorhanden ist, falls vorhanden übereinstimmen.
- Für jeden Parameter der Aktion Wenn der Parameter, aus dem URI erstellt wird, muss dann der Name des Parameters im Wörterbuch Route oder in der URI-Abfragezeichenfolge gefunden werden. (Parameter mit komplexen Typen und optionale Parameter werden ausgeschlossen.)
- Versuchen Sie es mit der höchsten Anzahl von Parametern übereinstimmen. Die beste Übereinstimmung möglicherweise eine Methode ohne Parameter.

## <a name="extended-example"></a>Erweitertes Beispiel

Routen:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controller:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP-Anforderung:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Weiterleiten von übereinstimmenden

Der URI entspricht der Route, die mit dem Namen "DefaultApi". Das Wörterbuch für die Route enthält die folgenden Einträge:

- Controller: "Produkte"
- id: "1"

Des Routenwörterbuchs enthält nicht den Abfragezeichenfolgen-Parameter, "Version" und "Details", aber diese werden betrachtet werden bei der Auswahl der Aktion.

### <a name="controller-selection"></a>Auswahl der Domänencontroller

Über den "Controller"-Eintrag im Wörterbuch weiterleiten, ist der Typ des Controllers `ProductsController`.

### <a name="action-selection"></a>Aktionsauswahl

Die HTTP-Anforderung ist eine GET-Anforderung. Die Controlleraktionen, die GET zu unterstützen sind `GetAll`, `GetById`, und `FindProductsByName`. Des Routenwörterbuchs enthält keinen Eintrag für "Action", daher wir nicht mit dem Aktionsnamen übereinstimmen müssen.

Als Nächstes versuchen wir, Parameternamen für die Aktionen entsprechen nur auf den GET-Aktionen suchen.

| Aktion | Parameter zur Übereinstimmung |
| --- | --- |
| `GetAll` | Keine |
| `GetById` | "Id" |
| `FindProductsByName` | "Name" |

Beachten Sie, dass die *Version* Parameter `GetById` gilt nicht, da es sich um einen optionalen Parameter ist.

Die `GetAll` Methode entspricht im Grunde kann. Die `GetById` Methode auch entspricht, da des Routenwörterbuchs "Id" enthält. Die `FindProductsByName` Methode stimmt nicht überein.

Die `GetById` wins-Methode, da es einen Parameter, und keine Parameter für entspricht `GetAll`. Die Methode wird mit den folgenden Parameterwerten aufgerufen:

- *id* = 1
- *Version* = 1.5

Beachten Sie, dass, obwohl *Version* nicht verwendet wurde, in der Auswahlalgorithmus stammt der Wert des Parameters die URI-Abfragezeichenfolge.

## <a name="extension-points"></a>Erweiterungspunkte

Web-API stellt Erweiterungspunkte für einige Teile der routing-Prozess bereit.

| Interface | Beschreibung |
| --- | --- |
| **IHttpControllerSelector** | Wählt den Controller. |
| **IHttpControllerTypeResolver** | Ruft die Liste der Controllertypen ab. Die **DefaultHttpControllerSelector** wählt den Controllertyp aus dieser Liste. |
| **IAssembliesResolver** | Ruft die Liste der Projektassemblys ab. Die **IHttpControllerTypeResolver** Schnittstelle verwendet diese Liste, um die Controllertypen gefunden werden. |
| **IHttpControllerActivator** | Erstellen von neuen Controllerinstanzen. |
| **IHttpActionSelector** | Wählt die Aktion. |
| **IHttpActionInvoker** | Ruft die Aktion. |

Um Ihre eigene Implementierung für den folgenden Schnittstellen bereitzustellen, verwenden die **Services** Auflistung auf der **HttpConfiguration** Objekt:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
