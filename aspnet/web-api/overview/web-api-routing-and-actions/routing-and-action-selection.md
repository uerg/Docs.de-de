---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing und Aktionsauswahl in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 02c2a01ef8ec2b5a49f2c303ee61f02702a3ba54
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a>Routing und Aktionsauswahl in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Artikel wird beschrieben, wie ASP.NET Web-API eine HTTP-Anforderung an eine bestimmte Aktion auf einem Domänencontroller weiterleitet.

> [!NOTE]
> Eine allgemeine Übersicht zum routing finden Sie unter [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).


Dieser Artikel beschäftigt sich mit den Details der routing-Prozess. Wenn Sie ein Web-API-Projekt erstellen, und suchen, die einige Anforderungen erhalten nicht weitergeleitet, die Möglichkeit, die Sie erwarten, wird hoffentlich treten bei diesen Artikel nützlich.

Routing hat drei Hauptphasen:

1. Übereinstimmende den URI, der eine routenvorlage.
2. Bei der Auswahl eines Controllers.
3. Aktion auswählen.

Sie können einige Teile des Prozesses durch Ihre eigenen benutzerdefinierten Verhalten ersetzen. In diesem Artikel wird das standardmäßige Verhalten beschrieben. Zum Schluss beachten ich überall, wo Sie das Verhalten anpassen können.

## <a name="route-templates"></a>Routenvorlagen

Eine routenvorlage ähnelt einem URI-Pfad kann, er jedoch Platzhalterwerte, mit geschweiften Klammern angegeben:

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

Wenn Sie eine Route erstellen, können Sie Standardwerte für einige oder alle der Platzhalter bereitstellen:

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

Sie können auch Einschränkungen, angeben, die einschränken, wie einen Platzhalter für ein URI-Segment verglichen werden kann:

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

Das Framework versucht, die Segmente im URI-Pfad der Vorlage übereinstimmen. Literale in der Vorlage müssen genau übereinstimmen. Ein Platzhalter entspricht keinem Wert, es sei denn, Sie Einschränkungen angeben. Das Framework stimmt nicht mit anderen Teile des URIS, z. B. den Hostnamen oder die Abfrageparameter überein. Das Framework wählt die erste Route in der Routingtabelle, die den URI entspricht.

Es gibt zwei spezielle Platzhalter: "{Controller}" und "{Aktion}".

- "{Controller}" enthält den Namen des Controllers.
- "{Aktion}" enthält den Namen der Aktion. Die übliche Konvention werden in der Web-API "{Aktion}" weggelassen werden soll.

### <a name="defaults"></a>der Arbeitszeittabelle

Wenn Sie die Standardwerte angeben, entspricht die Route einen URI, den dieser Segmente nicht vorhanden ist. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

Der URI "`http://localhost/api/products`" diese Route. Das Segment "{Kategorie}" ist den Standardwert "alle" zugeordnet.

### <a name="route-dictionary"></a>Routenwörterbuchs

Wenn das Framework eine Übereinstimmung für einen URI findet, erstellt es ein Wörterbuch, das den Wert für jeden Platzhalter enthält. Die Schlüssel sind die Platzhalternamen, nicht einschließlich der geschweiften Klammern. Die Werte stammen aus dem URI-Pfad oder von den Standardwerten. Das Wörterbuch wird gespeichert, der **IHttpRouteData** Objekt.

Während dieser Phase übereinstimmende Route werden die speziellen "{Controller}" und "{Aktion}" Platzhalter wie die anderen Platzhalter behandelt. Sie werden einfach in das Wörterbuch mit den anderen Werten gespeichert.

Ein Standardwert kann den speziellen Wert ist **RouteParameter.Optional**. Wenn ein Platzhalter für diesen Wert zugewiesen wird, wird der Wert des Routenwörterbuchs nicht hinzugefügt. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

Für den URI-Pfad "-api/Products" enthält des Routenwörterbuchs:

- Controller: "Products"
- Kategorie: "alle"

Für "api/Produkte/Toys/123" wird die Routenwörterbuchs enthalten:

- Controller: "Products"
- Kategorie: "Toys"
- ID: "123"

Die Standardwerte können auch einen Wert, der nicht, eine beliebige Stelle angezeigt wird in der routenvorlage einschließen. Wenn die Route übereinstimmt, wird dieser Wert im Wörterbuch gespeichert. Zum Beispiel:

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

Wenn der URI-Pfad "Root/api/8" ist, wird das Wörterbuch zwei Werte enthalten:

- Controller: "Customers"
- ID: "8"

## <a name="selecting-a-controller"></a>Auswählen eines Controllers

Auswahl der Domänencontroller erfolgt durch die **IHttpControllerSelector.SelectController** Methode. Diese Methode nimmt ein **HttpRequestMessage** Instanz und gibt eine **HttpControllerDescriptor**. Die standardmäßige Implementierung erfolgt durch die **DefaultHttpControllerSelector** Klasse. Diese Klasse verwendet eine einfache Algorithmen:

1. Suchen Sie in der Routenwörterbuchs für den Schlüssel "Controller".
2. Der Wert für diesen Schlüssel, und fügen Sie die Zeichenfolge "Controller" zum Abrufen des Namens der Controller-Typ.
3. Suchen Sie nach einem Web-API-Controller mit diesem Namen geben.

Des Routenwörterbuchs enthält die Schlüssel / Wert-Paar "Controller" = "Products", z. B. der Typ des Controllers "ProductsController" ist. Wenn keine übereinstimmenden Typ oder mehrere Übereinstimmungen vorhanden ist, gibt das Framework einen Fehler an den Client zurück.

Schritt 3 **DefaultHttpControllerSelector** verwendet die **IHttpControllerTypeResolver** Schnittstelle, um die Liste der Web-API-Controllertypen abzurufen. Die standardmäßige Implementierung des **IHttpControllerTypeResolver** gibt alle öffentlichen Klassen implementieren (a) **IHttpController**, (b) sind nicht abstrakt und (c) verfügen über einen Namen, die mit "Controller" enden.

## <a name="action-selection"></a>Aktionsauswahl

Nach der Auswahl des Controllers, wählt das Framework die Aktion durch Aufrufen der **IHttpActionSelector.SelectAction** Methode. Diese Methode nimmt ein **HttpControllerContext** und gibt eine **HttpActionDescriptor**.

Die standardmäßige Implementierung erfolgt durch die **ApiControllerActionSelector** Klasse. Um eine Aktion auswählen, prüft es Folgendes:

- Die HTTP-Methode der Anforderung.
- Der Platzhalter für "{Aktion}" in der routenvorlage, falls vorhanden.
- Die Parameter der Aktionen auf dem Controller.

Vor dem ansehen des Auswahl-Algorithmus, müssen wir einige Dinge Controlleraktionen zu verstehen.

**Welche Methoden auf dem Controller "Aktionen" berücksichtigt werden?** Wenn Sie eine Aktion auswählen, prüft das Framework nur öffentliche Instanzmethoden auf dem Controller. Darüber hinaus schließt er ["spezielle Name"](https://msdn.microsoft.com/en-us/library/system.reflection.methodbase.isspecialname) Methoden (Konstruktoren, Ereignisse, operatorüberladungen usw.) und von geerbten Methoden der **ApiController** Klasse.

**HTTP-Methoden.** Das Framework entscheidet sich nur auf Aktionen, die entsprechen die HTTP-Methode der Anforderung, wie folgt bestimmt:

1. Sie können die HTTP-Methode mit einem Attribut angeben: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **"HttpOptions"**, **HttpPatch**, **HttpPost**, oder **HttpPut**.
2. Andernfalls, wenn der Name der Controllermethode mit "Get", "Post", "Put", "Delete", "Head", "Optionen" oder "Patch-" beginnt, klicken Sie dann gemäß der Konvention die Aktion unterstützt HTTP-Methode.
3. Wenn keine der genannten Möglichkeiten, unterstützt die Methode POST aus.

**Parameterbindungen.** Eine parameterbindung ist wie Web-API einen Wert für einen Parameter erstellt wird. So sieht die Standardregel für die parameterbindung aus:

- Einfache Typen stammen aus dem URI.
- Komplexe Typen stammen aus dem Anforderungstext.

Zu einfachen Typen gehören alle von der [.NET Framework-primitive Typen](https://msdn.microsoft.com/en-us/library/system.type.isprimitive), plus **"DateTime"**, **Decimal**, **Guid**, **Zeichenfolge** , und **TimeSpan**. Für jede Aktion kann höchstens einen Parameter Anforderungstext lesen.

> [!NOTE]
> Es ist möglich, die Standardregeln für die Bindung zu überschreiben. Finden Sie unter [WebAPI parameterbindung hinter den Kulissen](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).


Hier ist der Aktion Auswahl-Algorithmus, mit diesen Hintergrund.

1. Erstellen Sie eine Liste aller Aktionen auf dem Controller an, die die HTTP-Anforderungsmethode entsprechen.
2. Wenn des Routenwörterbuchs ein "Aktion"-Eintrag vorhanden ist, entfernen Sie die Aktionen, die, deren Name nicht mit diesem Wert übereinstimmt.
3. Versuchen Sie, entsprechend der Action-Parameter an den URI wie folgt: 

    1. Erhalten Sie für jede Aktion eine Liste der Parameter, die einen einfachen Typ sind, in dem die Bindung ruft des Parameters aus dem URI. Schließen Sie die optionale Parameter.
    2. Versuchen Sie, eine Übereinstimmung mit jeder Parametername in des Routenwörterbuchs oder in der URI-Abfragezeichenfolge zu suchen, aus dieser Liste. Übereinstimmungen werden Groß-/Kleinschreibung und hängen die Reihenfolge der Parameter nicht.
    3. Wählen Sie eine Aktion aus, in dem alle Parameter in der Liste eine Übereinstimmung im URI verfügt.
    4. Wenn mehr, einer Aktion diese Kriterien erfüllt, wählen Sie das Laufwerk mit der meisten Parameter entspricht.
4. Ignorieren von Aktionen mit den **[NonAction]** Attribut.

Schritt &#3; ist der am häufigsten verwirrend. Die Grundidee ist, dass ein Parameter den Wert entweder aus dem URI, aus dem Anforderungstext oder aus einer benutzerdefinierten Bindung abrufen kann. Für Parameter, die aus dem URI stammen, möchten wir stellen Sie sicher, dass der URI tatsächlich einen Wert für diesen Parameter, die in den Pfad (über des Routenwörterbuchs) oder in der Abfragezeichenfolge enthält.

Betrachten Sie beispielsweise die folgende Aktion aus:

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

Die *Id* Parameter bindet an den URI. Diese Aktion kann daher nur einen URI übereinstimmen, die einen Wert für "Id", in der Routenwörterbuchs oder in der Abfragezeichenfolge enthalten.

Optionale Parameter sind eine Ausnahme aus, da sie optional sind. Ist es für einen optionalen Parameter OK aus, wenn die Bindung den Wert aus dem URI abrufen kann.

Komplexe Typen sind eine Ausnahme für einen anderen Grund. Ein komplexer Typ kann nur durch eine benutzerdefinierte Bindung an den URI binden. Aber in diesem Fall das Framework nicht wissen im voraus, ob der Parameter an einen bestimmten URI gebunden werden würde. Um herauszufinden, müssten sie die Bindung aufrufen. Das Ziel des Algorithmus Auswahl ist, wählen Sie eine Aktion aus der statischen Beschreibung vor dem Aufrufen von Bindungen. Aus diesem Grund werden komplexe Typen aus dem übereinstimmenden Algorithmus ausgeschlossen.

Nachdem die Aktion aktiviert ist, werden alle parameterbindungen aufgerufen.

Zusammenfassung:

- Die Aktion muss die HTTP-Methode der Anforderung überein.
- Der Aktionsname muss den Eintrag "Aktion" in der Routenwörterbuchs angefordertes übereinstimmen.
- Für jeden Parameter der Aktion stammt aus dem URI, der Parameter muss dann der Name des Parameters des Routenwörterbuchs oder im URI-Abfragezeichenfolge gefunden werden. (Optionale Parameter und Parameter mit komplexen Typen sind ausgenommen.)
- Versuchen Sie es mit der höchsten Anzahl von Parametern übereinstimmen. Die beste Übereinstimmung möglicherweise eine Methode ohne Parameter.

## <a name="extended-example"></a>Erweitertes Beispiel

Routen:

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

Controller:

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

HTTP-Anforderung:

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a>Übereinstimmende Route

Der URI mit der Route, die mit dem Namen "DefaultApi" übereinstimmt. Die Route-Wörterbuch enthält die folgenden Einträge:

- Controller: "Products"
- ID: "1"

Des Routenwörterbuchs enthält nicht den Abfragezeichenfolgen-Parametern, "Version" und "Details", aber diese werden weiterhin während Aktionsauswahl berücksichtigt werden.

### <a name="controller-selection"></a>Auswahl der Domänencontroller

Von der "Controller" Eintrag im Wörterbuch Route ist der Typ des Controllers ist `ProductsController`.

### <a name="action-selection"></a>Aktionsauswahl

Die HTTP-Anforderung ist eine GET-Anforderung. Die Controlleraktionen, die GET unterstützen sind `GetAll`, `GetById`, und `FindProductsByName`. Des Routenwörterbuchs enthält keinen Eintrag für "Aktion", daher wir brauchen mit dem Aktionsnamen übereinstimmen.

Als Nächstes verwenden wir Parameternamen für die Aktionen entsprechend betrachten die GET-Aktionen.

| Aktion | Parameter zur Übereinstimmung |
| --- | --- |
| `GetAll` | Keine |
| `GetById` | "Id" |
| `FindProductsByName` | "Name" |

Beachten Sie, dass die *Version* Parameter `GetById` nicht berücksichtigt, da es sich um einen optionalen Parameter handelt.

Die `GetAll` Methode entspricht im Grunde. Die `GetById` Methode auch entspricht, da die Routenwörterbuchs "Id" enthält. Die `FindProductsByName` Methode stimmt nicht überein.

Die `GetById` wins-Methode, da es sich um einen Parameter, und keine Parameter für entspricht `GetAll`. Die Methode wird mit den folgenden Parameterwerte aufgerufen:

- *ID* = 1
- *Version* = 1.5

Beachten Sie, dass, obwohl *Version* nicht verwendet wurde, in der Auswahl-Algorithmus stammt der Wert des Parameters aus der URI-Abfragezeichenfolge.

## <a name="extension-points"></a>Erweiterungspunkte

Web-API stellt Erweiterungspunkte bereit, für einige Teile der routing-Prozess.

| Schnittstelle | Beschreibung |
| --- | --- |
| **IHttpControllerSelector** | Wählt den Controller. |
| **IHttpControllerTypeResolver** | Ruft die Liste der Controllertypen ab. Die **DefaultHttpControllerSelector** wählt den Controllertyp aus dieser Liste. |
| **IAssembliesResolver** | Ruft die Liste der Projektassemblys ab. Die **IHttpControllerTypeResolver** Schnittstelle verwendet diese Liste, um die Controllertypen gefunden werden. |
| **IHttpControllerActivator** | Erstellt neue Domänencontroller-Instanzen. |
| **IHttpActionSelector** | Wählt die Aktion an. |
| **IHttpActionInvoker** | Ruft die Aktion an. |

Verwenden, um eine eigene Implementierung für den folgenden Schnittstellen ermöglichen die **Services** Sammlung auf die **HttpConfiguration** Objekt:

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
