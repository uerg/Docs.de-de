---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameterbindung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parameterbindung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Beim Web-API auf einem Domänencontroller eine Methode aufruft, müssen sie Werte für die Parameter, einen so genannten festgelegt *Bindung*. In diesem Artikel wird beschrieben, wie Web-API-Parameter gebunden und Anpassung des Bindungsvorgangs.

Standardmäßig verwendet die Web-API die folgenden Regeln zum Binden von Parametern:

- Wenn der Parameter ein "simple"-Typ ist, versucht Web-API zum Abrufen des Werts aus dem URI. Zu einfachen Typen gehören .NET [Grundtypen](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**Int**, **Bool**, **doppelte**usw.), plus **TimeSpan**, **"DateTime"**, **Guid**, **decimal**, und **Zeichenfolge**, *plus* alle Geben Sie einen Typkonverter, der aus einer Zeichenfolge konvertiert werden kann. (Weitere Informationen zu einem späteren Zeitpunkt den Einsatz von Typkonvertern.)
- Verwenden Sie für komplexe Typen, Web-API zum Lesen des Werts aus dem Nachrichtentext versucht, eine [medientypformatierer](media-formatters.md).

Hier ist z. B. eine typische Web-API-Controller-Methode:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Die *Id* Parameter ist ein &quot;einfache&quot; eingeben, damit der Web-API versucht, den Wert aus der Anforderungs-URI abzurufen. Die *Element* Parameter ist ein komplexer Typ, damit Web-API einen medientypformatierer verwendet, um den Wert aus dem Anforderungstext zu lesen.

Um einen Wert aus dem URI zu erhalten, sucht Web-API in der Routendaten und der URI-Abfragezeichenfolge. Die Routendaten werden aufgefüllt, wenn das routing System den URI analysiert und ihn mit einer Route vergleicht. Weitere Informationen finden Sie unter [Routing und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md).

Im Rest dieses Artikels zeige ich, wie Sie dem modellbindungsprozess anpassen können. Für komplexe Typen, sollten Sie jedoch nach Möglichkeit medientypformatierer. Ein Hauptprinzip von HTTP ist, dass Ressourcen im Nachrichtentext, mithilfe der inhaltsaushandlung an die Darstellung der Ressource gesendet werden. Medientypformatierer wurden für genau diesen Zweck konzipiert.

## <a name="using-fromuri"></a>Verwenden [FromUri]

Um Web-API zum Lesen eines komplexen Typs aus dem URI zu erzwingen, Hinzufügen der **[FromUri]** -Attribut für den Parameter. Das folgende Beispiel definiert eine `GeoPoint` Typs führen, sowie einen Controllermethode, die Ruft die `GeoPoint` aus dem URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Der Client kann die Breiten- und Längengrad Werte in der Abfragezeichenfolge abgelegt und Web-API verwenden sie zum Erstellen einer `GeoPoint`. Zum Beispiel:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Verwenden [FromBody]

Um Web-API zum Lesen von eines einfachen Typs aus dem Anforderungstext zu erzwingen, Hinzufügen der **[FromBody]** -Attribut für den Parameter:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In diesem Beispiel wird Web-API einen medientypformatierer verwenden, lesen Sie den Wert der *Namen* aus dem Anforderungstext. Hier ist eine Beispiel für Client-Anforderung.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Wenn ein Parameter [FromBody] verfügt, verwendet Web-API Content-Type-Headers, um ein Formatierungsprogramm auszuwählen. In diesem Beispiel wird der Inhaltstyp &quot;Application/Json&quot; und der Anforderungstext ist eine unformatierte JSON-Zeichenfolge (kein JSON-Objekt).

Über höchstens einen Parameter aus dem Nachrichtentext lesen darf. Daher wird dies nicht funktioniert:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Der Grund für diese Regel ist, dass der Anforderungstext in einem nicht gepufferten Datenstrom gespeichert werden kann, die nur einmal gelesen werden kann.

## <a name="type-converters"></a>Typkonverter

Sie können die Web-API, die eine Klasse als ein einfacher Typ behandelt werden sollen (damit, dass Web-API versucht, sie aus dem URI Binden) vornehmen, durch das Erstellen einer **TypeConverter** und bietet eine zeichenfolgenkonvertierung.

Der folgende code zeigt eine `GeoPoint` -Klasse, die einen geographischen Punkt darstellt zuzüglich einer **TypeConverter** , die aus Zeichenfolgen konvertiert `GeoPoint` Instanzen. Die `GeoPoint` Klasse ergänzt wird, mit einem **[TypeConverter]** -Attribut den Typkonverter angeben. (In diesem Beispiel wurde von Mike Stall des Blogbeitrag Inspiriertes [Gewusst wie: Binden an benutzerdefinierten Objekte in Aktion Signaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Nachdem Web-API behandelt, `GeoPoint` als ein einfacher Typ, d. h. es wird versucht, binden `GeoPoint` Parameter aus dem URI. Sie müssen nicht enthalten sein **[FromUri]** für den Parameter.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Der Client kann die Methode mit einem URI wie folgt aufrufen:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Modellbinder

Erstellen Sie einen benutzerdefinierten Modellbinder ist eine flexiblere Option als einen Typkonverter werden. Mit einem Modellbinder haben Sie Zugriff auf die HTTP-Anforderung und die Beschreibung der Aktion sowie die unformatierte Werte von den Routendaten.

Um einen Modellbinder zu erstellen, implementieren die **IModelBinder** Schnittstelle. Diese Schnittstelle definiert eine einzelne Methode **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Hier ist ein Modellbinder für `GeoPoint` Objekte.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Ein Modellbinder ruft unformatierte Eingabewerten aus einer *Wertanbieter*. Dieser Entwurf trennt werden zwei verschiedene Funktionen:

- Der Wertanbieter die HTTP-Anforderung akzeptiert und ein Wörterbuch von Schlüssel-Wert-Paaren aufgefüllt.
- Der Modellbinder verwendet dieses Wörterbuch aufweist und zum Auffüllen des Modells.

Der Standard-Wertanbieter in Web-API ruft die Routendaten und der Abfragezeichenfolge Werte ab. Wenn der URI ist z. B. `http://localhost/api/values/1?location=48,-122`, Wertanbieter erstellt der folgenden Schlüssel-Wert-Paaren:

- id = &quot;1&quot;
- Speicherort = &quot;48,122&quot;

(Ich verwende die Standardvorlage für die Route, also vorausgesetzt &quot;api / {Controller} / {Id}&quot;.)

Der Name des Parameters gebunden wird gespeichert, der **ModelBindingContext.ModelName** Eigenschaft. Der Modellbinder sucht nach einem Schlüssel mit diesem Wert im Wörterbuch. Wenn der Wert vorhanden ist, und kann, in konvertiert werden einem `GeoPoint`, der Modellbinder weist den gebundenen Wert, der die **ModelBindingContext.Model** Eigenschaft.

Beachten Sie, dass die modellbindung nicht auf eine einfache typkonvertierung beschränkt ist. In diesem Beispiel der Modellbinder sucht zuerst in eine Tabelle mit den bekannten Speicherorten, und wenn dies fehlschlägt, verwendet typkonvertierung.

**Den Modellbinder festlegen**

Es gibt mehrere Möglichkeiten, einen Modellbinder festzulegen. Sie können fügen Sie zunächst eine **[ModelBinder]** -Attribut für den Parameter.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Sie können auch Hinzufügen einer **[ModelBinder]** -Attribut auf den Typ. Web-API wird den angegebene Modellbinder für alle Parameter dieses Typs verwendet.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Schließlich können Sie einen Modellbinder Anbieter zum Hinzufügen der **HttpConfiguration**. Ein Modellbinder Anbieter ist einfach eine Factoryklasse, die einen Modellbinder erstellt. Sie können einen Anbieter erstellen, durch Ableiten von der [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) Klasse. Wenn Ihre Modellbinder ein einzelnes Typs behandelt, es ist jedoch einfacher mithilfe die integrierten **SimpleModelBinderProvider**, die für diesen Zweck dient. Dies wird im folgenden Code veranschaulicht.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Mit einem Anbieter modellbindungs-Sie benötigen Sie weitere hinzufügen der **[ModelBinder]** -Attribut für den Parameter, Web-API zu informieren, dass es einen Modellbinder und nicht auf einem medientypformatierer verwendet werden sollen. Jetzt müssen Sie jedoch nicht den Typ der modellbindung im Attribut angeben:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Wertanbieter

Ich schon gesagt habe, ein Modellbinder von einem Wertanbieter Ruft Werte ab. Um einen benutzerdefinierten Wertanbieter zu schreiben, implementieren die **IValueProvider** Schnittstelle. Hier ist ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Sie müssen auch eine Wertanbieterfactory zu erstellen, durch Ableiten von der **ValueProviderFactory** Klasse.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Hinzufügen der Wertanbieterfactory auf die **HttpConfiguration** wie folgt.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web-API verfasst aller Wertanbieter also wenn aufruft, ein Modellbinder **ValueProvider.GetValue**, der Modellbinder erhält den Wert aus der ersten Wertanbieter, die daraus resultieren kann.

Sie können alternativ die Wertanbieterfactory Ebene der Parameter festlegen, mit der **ValueProvider** -Attribut wie folgt:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Dies teilt Web-API, mit der angegebenen Wertanbieterfactory wurden die modellbindung verwenden und nicht auf eine der anderen registrierten Wertanbieter verwenden.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Modellbinder entsprechen einer bestimmten Instanz eines allgemeineren Mechanismus. Bei Betrachtung der **[ModelBinder]** -Attribut, sehen Sie, dass es von der abstrakten abgeleitet **ParameterBindingAttribute** Klasse. Diese Klasse definiert eine einzelne Methode **GetBinding**, welche gibt eine **HttpParameterBinding** Objekt:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Ein **HttpParameterBinding** ist verantwortlich für einen Parameter auf einen Wert zu binden. Im Fall von **[ModelBinder]**, das Attribut gibt eine **HttpParameterBinding** Implementierung, die verwendet ein **IModelBinder** die tatsächliche Bindung ausführen. Sie können auch implementieren Ihre eigenen **HttpParameterBinding**.

Nehmen wir beispielsweise an, die Sie ETags aus abrufen möchten `if-match` und `if-none-match` Header in der Anforderung. Wir beginnen, durch die Definition einer Klasse zum Darstellen des ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Definieren wir eine Enumeration angeben, ob das ETag des abzurufenden auch die `if-match` Header oder der `if-none-match` Header.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Hier ist ein **HttpParameterBinding** , ruft das ETag aus den gewünschten Headertyp als ab und bindet sie an einen Parameter vom Typ ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Die **ExecuteBindingAsync** -Methode führt die Bindung. Fügen Sie innerhalb dieser Methode den gebundenen Parameterwert die **ActionArgument** Wörterbuch in das **HttpActionContext**.

> [!NOTE]
> Wenn Ihre **ExecuteBindingAsync** Methode liest den Text der Anforderungsnachricht, überschreiben die **WillReadBody** -Eigenschaft auf "true" zurückgeben. Der Anforderungstext ist möglicherweise, dass ein ungepufferten Datenstrom, der nur einmal gelesen werden kann, sodass Web-API einer Regel, dass höchstens einer Bindung erzwingt den Nachrichtentext lesen kann.


Anwenden ein benutzerdefinierten **HttpParameterBinding**, können Sie ein Attribut, das von abgeleitet ist definieren **ParameterBindingAttribute**. Für `ETagParameterBinding`, definieren wir zwei Attribute: eine für `if-match` Kopf- und eine für `if-none-match` Header. Beide werden von einer abstrakten Klasse abgeleitet.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Hier ist eine Controllermethode, verwendet die `[IfNoneMatch]` Attribut.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Neben **ParameterBindingAttribute**, es ist eine andere Hook zum Hinzufügen eines benutzerdefiniertes **HttpParameterBinding**. Auf der **HttpConfiguration** -Objekt, das **ParameterBindingRules** Eigenschaft ist eine Sammlung von Anomymous Funktionen des Typs (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Sie können z. B. eine Regel, ein ETag-Parameter für eine GET-Methode verwendet, hinzufügen `ETagParameterBinding` mit `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Die Funktion zurückgeben sollte `null` für Parameter, die die Bindung, in denen nicht anwendbar ist.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Der gesamte parameterbindung Prozess wird gesteuert, von einem Dienst austauschbare **IActionValueBinder**. Die standardmäßige Implementierung des **IActionValueBinder** bewirkt Folgendes:

1. Suchen Sie nach einer **ParameterBindingAttribute** für den Parameter. Dies schließt **[FromBody]**, **[FromUri]**, und **[ModelBinder]**, oder benutzerdefinierte Attribute.
2. Andernfalls, suchen Sie im **HttpConfiguration.ParameterBindingRules** für eine Funktion, eine Wert ungleich Null zurückgibt **HttpParameterBinding**.
3. Verwenden Sie andernfalls die Standardregeln, die ich zuvor beschrieben. 

    - Wenn der Parametertyp ist "einfach" oder einen Typkonverter, binden Sie aus dem URI. Dies entspricht dem Platzieren der **[FromUri]** Attribut für den Parameter.
    - Andernfalls versucht, den Parameter aus dem Nachrichtentext lesen. Dies entspricht dem platzieren **[FromBody]** für den Parameter.

Falls gewünscht, konnte Sie ersetzt die gesamte **IActionValueBinder** Service durch eine benutzerdefinierte Implementierung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Benutzerdefinierte Parameter Bindung-Beispiel](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall wurde zu Web-API-parameterbindung eine gute Reihe von Blogbeiträgen geschrieben:

- [Wie funktioniert Web-API Parameterbindung](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [MVC-Stil parameterbindung für Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Gewusst wie: Binden an benutzerdefinierten Objekte in Aktion Signaturen in MVC/Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Vorgehensweise: Erstellen Sie einen benutzerdefinierten Wertanbieter in Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Web-API-parameterbindung hinter den Kulissen](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
