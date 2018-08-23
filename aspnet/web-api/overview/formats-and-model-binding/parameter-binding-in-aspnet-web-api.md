---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: Parameterbindung in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d29f087cd658faf1fadb0d9a85e9f32c03a2b3f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838030"
---
<a name="parameter-binding-in-aspnet-web-api"></a>Parameterbindung in der ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Bei Web-API eine Methode auf einem Controller aufruft, müssen sie festlegen, dass Werte für die Parameter, ein so genanntes *Bindung*. In diesem Artikel wird beschrieben, wie Web-API-Parameter gebunden wird und wie Sie den Bindungsprozess anpassen können.

Web-API verwendet standardmäßig die folgenden Regeln, um Parameter zu binden:

- Wenn der Parameter "einfach" ein, versucht Web-API, den Wert aus dem URI abzurufen. Einfache Typen enthalten, die .NET [primitive Typen](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**Int**, **"bool"**, **doppelte**usw.), plus **TimeSpan**, **"DateTime"**, **Guid**, **decimal**, und **Zeichenfolge**, *plus* alle Geben Sie mit einem Typkonverter, der aus einer Zeichenfolge konvertieren kann. (Weitere Informationen zu Typkonvertern weiter unten.)
- Verwenden Sie für komplexe Typen, Web-API versucht, den Wert aus dem Nachrichtentext zu lesen, eine [medientypformatierer](media-formatters.md).

Hier ist z. B. eine typische Web-API-Controller-Methode:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

Die *Id* -Parameter ist ein &quot;einfache&quot; eingeben, damit die Web-API versucht, den Wert aus der Anforderungs-URI abzurufen. Die *Element* Parameter ist ein komplexer Typ, damit die Web-API einen medientypformatierer verwendet, um den Wert aus dem Anforderungstext lesen.

Rufen Sie einen Wert aus dem URI sieht Web-API in den Routendaten und der URI-Abfragezeichenfolge. Die Routendaten werden aufgefüllt, wenn das Routingsystem den URI analysiert und ihn mit einer Route vergleicht. Weitere Informationen finden Sie unter [Routing- und Aktionsauswahl](../web-api-routing-and-actions/routing-and-action-selection.md).

In der Rest dieses Artikels zeige ich, wie Sie den modellbindungsprozess anpassen können. Für komplexe Typen, sollten Sie jedoch nach Möglichkeit medientypformatierer. Ein Hauptprinzip von HTTP ist, dass Ressourcen im Nachrichtentext, mit der Aushandlung von Inhalten an die Darstellung der Ressource gesendet werden. Medientypformatierer wurden für genau diesen Zweck entwickelt.

## <a name="using-fromuri"></a>Verwenden [FromUri]

Um Web-API zum Lesen eines komplexen Typs aus dem URI zu erzwingen, Hinzufügen der **[FromUri]** -Attribut auf den Parameter. Das folgende Beispiel definiert eine `GeoPoint` Typs führen, sowie eine Controllermethode, die Ruft die `GeoPoint` aus dem URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

Der Client kann die Werte für Breiten- und Längengrad platzieren, in der Abfragezeichenfolge und Web-API verwenden sie zum Erstellen einer `GeoPoint`. Zum Beispiel:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>Verwenden [FromBody]

Um Web-API, um einen einfachen Typ aus dem Anforderungstext lesen zu erzwingen, Hinzufügen der **[FromBody]** -Attribut auf den Parameter:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

In diesem Beispiel-Web-API verwendet einen medientypformatierer, lesen den Wert der *Namen* aus dem Anforderungstext. Hier ist eine beispielanforderung für den Client.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Wenn ein Parameter [FromBody] verfügt, verwendet Web-API den Content-Type-Header, um einen Formatierer auszuwählen. In diesem Beispiel wird der Inhaltstyp &quot;Application/Json&quot; und der Anforderungstext ist eine unformatierte JSON-Zeichenfolge (nicht in ein JSON-Objekt).

Höchstens einen Parameter aus dem Nachrichtentext lesen kann. Daher wird dies nicht funktioniert:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Der Grund für diese Regel ist, dass der Hauptteil der Anforderung in einem nicht gepufferten Stream gespeichert werden kann, die nur einmal gelesen werden kann.

## <a name="type-converters"></a>Typkonverter

Sie können die Web-API, die eine Klasse als ein einfacher Typ behandelt werden sollen (sodass Web-API versucht, aus dem URI bindet, bindet es) vornehmen, durch das Erstellen einer **TypeConverter** und eine zeichenfolgenkonvertierung.

Der folgende code zeigt eine `GeoPoint` -Klasse, die einen geografischen Punkt, darstellt sowie **TypeConverter** , der von Zeichenfolgen, die konvertiert `GeoPoint` Instanzen. Die `GeoPoint` Klasse ergänzt wird, mit einem **[TypeConverter]** -Attribut den Typkonverter angegeben. (In diesem Beispiel wurde dadurch inspiriert, Blogbeitrag von Mike Stall des [Gewusst wie: Binden an benutzerdefinierte Objekte in aktionssignaturen in MVC/WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Jetzt Web-API behandelt `GeoPoint` als einen einfachen Typ, d. h. es wird versucht, binden `GeoPoint` Parameter aus dem URI. Sie müssen nicht einschließen **[FromUri]** für den Parameter.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

Der Client kann die Methode mit einem URI wie folgt aufrufen:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Modellbinder

Eine Option für mehr Flexibilität als ein Typkonverter ist, um einen benutzerdefinierten Modellbinder zu erstellen. Mit einem Modellbinder haben Sie Zugriff auf Dinge wie die HTTP-Anforderung, die Beschreibung der Aktion und die unformatierte Werte aus den Routendaten.

Um einen Modellbinder erstellen, implementieren die **IModelBinder** Schnittstelle. Diese Schnittstelle definiert eine einzelne Methode, **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Hier ist eine modellbindung für `GeoPoint` Objekte.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Ein Modellbinder Ruft die Rohdaten der Eingabewerten aus einer *Wertanbieter*. Dieser Entwurf trennt werden zwei verschiedene Funktionen:

- Der Wertanbieter die HTTP-Anforderung akzeptiert und füllt ein Wörterbuch von Schlüssel-Wert-Paaren.
- Die modellbindung verwendet dieses Wörterbuch zum Auffüllen des Modells.

Der Standardanbieter-Wert in der Web-API ruft Werte aus den Routendaten und die Abfragezeichenfolge ab. Wenn der URI ist z. B. `http://localhost/api/values/1?location=48,-122`, der Wertanbieter erstellt die folgenden Schlüssel-Wert-Paare:

- id = &quot;1&quot;
- Standort = &quot;48,122&quot;

(Ich gehe davon aus der Standardvorlage für die Route, handelt es sich &quot;api / {Controller} / {Id}&quot;.)

Der Name des Parameters, der Bindung wird gespeichert, der **ModelBindingContext.ModelName** Eigenschaft. Die modellbindung sucht nach einem Schlüssel mit diesem Wert im Wörterbuch. Wenn der Wert vorhanden ist, und kann, in konvertiert werden einem `GeoPoint`, die modellbindung weist den gebundenen Wert der **ModelBindingContext.Model** Eigenschaft.

Beachten Sie, dass die modellbindung nicht auf eine einfache typkonvertierung beschränkt ist. In diesem Beispiel die modellbindung sucht zuerst in eine Tabelle mit bekannten Speicherorten und wenn dies fehlschlägt, verwendet es typkonvertierung.

**Festlegen der Modellbindung**

Es gibt mehrere Möglichkeiten, einen Modellbinder festzulegen. Sie können zunächst Hinzufügen einer **[ModelBinder]** -Attribut auf den Parameter.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Sie können auch Hinzufügen einer **[ModelBinder]** -Attribut auf den Typ. Web-API wird den angegebene Modellbinder für alle Parameter dieses Typs verwendet werden.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Schließlich können Sie einen modellbinderanbieter zum Hinzufügen der **HttpConfiguration**. Ein modellbinderanbieter ist einfach eine Factoryklasse, die eine modellbindung erstellt. Sie können einen Anbieter erstellen, durch Ableiten von der [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) Klasse. Wenn Ihre modellbindung einen einzelnen Typ behandelt wird, ist es jedoch einfacher mithilfe die integrierten **SimpleModelBinderProvider**, die für diesen Zweck dient. Dies wird im folgenden Code veranschaulicht.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Mit einem modellbindungs-Anbieter müssen dennoch hinzufügen der **[ModelBinder]** -Attribut auf den Parameter, um Web-API-mitzuteilen, dass eine modellbindung und nicht auf einem medientypformatierer verwendet werden soll. Jetzt müssen Sie jedoch nicht auf das Attribut den Typ der modellbindung angeben:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Wertanbieter

Ich habe erwähnt, dass eine modellbindung von einem Wertanbieter Ruft Werte ab. Um einen benutzerdefinierten Wertanbieter zu schreiben, implementieren die **IValueProvider** Schnittstelle. Hier ist ein Beispiel, das Werte aus den Cookies in der Anforderung abruft:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Sie müssen auch eine Wertanbieter-Factory zu erstellen, durch Ableiten von der **ValueProviderFactory** Klasse.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Hinzufügen der Wertanbieter-Factory, die **HttpConfiguration** wie folgt.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web-API erstellt alle von den Wertanbietern, also wenn ein Modellbinder aufruft **ValueProvider.GetValue**, die modellbindung empfängt den Wert aus der ersten Wertanbieter, die sie erzeugt werden kann.

Sie können alternativ die Wertanbieter-Factory auf der Ebene der Parameter festlegen, mit der **ValueProvider** Attribut wie folgt:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Dadurch wird die Web-API mit modellbindung mit dem angegebenen Wertanbieter-Factory und nicht auf den registrierten Wertanbietern verwendet.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Modellbinder entsprechen einer bestimmten Instanz eines allgemeineren Mechanismus. Bei Betrachtung der **[ModelBinder]** -Attribut, sehen Sie, dass es von der abstrakten abgeleitetes **ParameterBindingAttribute** Klasse. Diese Klasse definiert eine einzelne Methode, **GetBinding**, gibt ein **HttpParameterBinding** Objekt:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Ein **HttpParameterBinding** ist verantwortlich für die ein Parameter auf einen Wert gebunden. Im Fall von **[ModelBinder]**, gibt das Attribut ein **HttpParameterBinding** -Implementierung, verwendet eine **IModelBinder** , führen Sie die tatsächliche Bindung. Sie können auch implementieren Ihre eigenen **HttpParameterBinding**.

Nehmen wir beispielsweise an, die Sie ETags aus abrufen möchten `if-match` und `if-none-match` Header in der Anforderung. Wir beginnen mit dem Definieren einer Klasse zur Darstellung von ETags.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Wir definieren auch eine Enumeration, um anzugeben, ob das ETag abgerufen werden. die `if-match` Header oder der `if-none-match` Header.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Hier ist ein **HttpParameterBinding** , ruft das ETag ab, aus dem gewünschten Header und bindet ihn an einen Parameter vom Typ ETag:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

Die **ExecuteBindingAsync** Methode führt die Bindung. Fügen Sie innerhalb dieser Methode wird den gebundene Parameter-Wert, der **ActionArgument** Wörterbuch in der **HttpActionContext**.

> [!NOTE]
> Wenn Ihre **ExecuteBindingAsync** Methode liest den Text der Anforderungsnachricht, überschreiben die **WillReadBody** -Eigenschaft "true" zurückgeben. Der Anforderungstext ist möglicherweise, dass ein nicht gepufferten Datenstream, der nur einmal gelesen werden kann, sodass Web-API einer Regel, dass höchstens eine Bindung setzt den Nachrichtentext lesen kann.


Anwenden ein benutzerdefinierten **HttpParameterBinding**, Sie können definieren, ein Attribut, das von abgeleitet ist **ParameterBindingAttribute**. Für `ETagParameterBinding`, definieren wir zwei Attribute: eine für `if-match` Header und eine für `if-none-match` Header. Beide werden von einer abstrakten Klasse abgeleitet.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Hier ist eine Controllermethode, verwendet der `[IfNoneMatch]` Attribut.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Darüber hinaus **ParameterBindingAttribute**, es wird eine andere Hook für das Hinzufügen eines benutzerdefiniertes **HttpParameterBinding**. Auf der **HttpConfiguration** -Objekt, das **ParameterBindingRules** Eigenschaft ist eine Sammlung von Anomymous Funktionen des Typs (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Sie können beispielsweise eine Regel, ein ETag-Parameter für eine GET-Methode verwendet, hinzufügen `ETagParameterBinding` mit `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

Die Funktion zurückgeben soll `null` für Parameter, die die Bindung, in denen nicht anwendbar ist.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Der gesamte Prozess für die bindenden Parameter wird gesteuert, von einem Dienst austauschbare **IActionValueBinder**. Die standardmäßige Implementierung des **IActionValueBinder** bewirkt Folgendes:

1. Suchen Sie nach einer **ParameterBindingAttribute** für den Parameter. Dies schließt **[FromBody]**, **[FromUri]**, und **[ModelBinder]**, oder benutzerdefinierte Attribute.
2. Suchen Sie andernfalls **HttpConfiguration.ParameterBindingRules** für eine Funktion, eine Wert ungleich Null zurückgibt **HttpParameterBinding**.
3. Verwenden Sie andernfalls die Standardregeln, die ich zuvor beschrieben. 

    - Wenn der Parametertyp ist "einfach" oder einen Typkonverter, binden Sie aus dem URI. Dies entspricht dem Platzieren der **[FromUri]** Attribut für den Parameter.
    - Versuchen Sie andernfalls den Parameter aus dem Nachrichtentext zu lesen. Dies ist äquivalent zum Einfügen von **[FromBody]** für den Parameter.

Falls gewünscht, können Sie die gesamte ersetzen **IActionValueBinder** mit einer benutzerdefinierten Implementierung.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Beispiel einer benutzerdefinierten Parameter](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike Stall habe eine gute Reihe von Blogbeiträgen zur Bindung der Web-API-Parameter:

- [Wie Web-API binden?](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [MVC-Stil parameterbindung für Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Gewusst wie: Binden an benutzerdefinierte Objekte in aktionssignaturen in der MVC-Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Vorgehensweise: erstellen ein benutzerdefinierten Wertanbieters in Web-API](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Web-API-parameterbindung im Hintergrund](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
