---
title: "Formatieren von Antwortdaten in ASP.NET-MVC für Core"
author: ardalis
description: Informationen Sie zum Formatieren von Antwortdaten in ASP.NET-MVC-Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85398928164e75ec27c91870f03ee1c81725e080
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Einführung in die Antwort Formatierungsdaten in ASP.NET-MVC für Core

Durch [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC verfügt über integrierte Unterstützung für die Formatierung von Antwortdaten, die mithilfe von fester Formaten oder als Antwort auf Client-Spezifikationen.

[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Format-spezifische Aktionsergebnisse

Einige Aktionsergebnistypen sind spezifisch für ein bestimmtes Format, z. B. `JsonResult` und `ContentResult`. Aktionen können bestimmte Ergebnisse zurückgeben, die immer in einer bestimmten Weise formatiert werden. Zurückgeben von z. B. eine `JsonResult` JSON-formatierte Daten, unabhängig von Clienteinstellungen zurück. Ebenso Zurückgeben einer `ContentResult` Zeichenfolge im Format von nur-Text-Daten (wie eine Zeichenfolge zurückzugeben) zurück.

> [!NOTE]
> Eine Aktion ist nicht erforderlich, um keinen bestimmten Typ zurückgeben; MVC unterstützt Objekt-Rückgabewert. Wenn eine Aktion gibt eine `IActionResult` Implementierung und den Controller erbt von `Controller`, Entwickler haben viele Hilfsmethoden, die viele der Optionen für. Ergebnisse von Aktionen, die Objekte zurückgeben, sind nicht `IActionResult` Typen serialisiert, die mit dem entsprechenden `IOutputFormatter` Implementierung.

Um Daten in einem bestimmten Format von einem Controller zurückzugeben, die von erben die `Controller` Basisklasse, verwenden Sie die integrierte Hilfsmethode `Json` zurückzugebenden JSON und `Content` für nur-Text. Die Aktionsmethode sollte die bestimmten Ergebnistyp zurückgeben (z. B. `JsonResult`) oder `IActionResult`.

Zurückgeben von JSON-formatierte Daten:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Beispielantwort aus dieser Aktion:

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Application/json](formatting/_static/json-response.png)

Beachten Sie, dass der Inhaltstyp der Antwort `application/json`, die sowohl in der Liste der Anforderungen über das Netzwerk und im Antwortheader Abschnitt gezeigt. Beachten Sie außerdem die Liste der Optionen, die vom Browser im Accept-Header im Anforderungsheader Abschnitt (in diesem Fall Microsoft Edge) angezeigt. Das aktuelle Verfahren ist dieser Header wird ignoriert; Es obeying wird nachfolgend erläutert.

Nur-Text formatiert Daten zurückgeben möchten, verwenden `ContentResult` und `Content` Hilfsprogramm:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Eine Antwort von dieser Aktion:

![Registerkarte "Netzwerk" der Developer Tools in Microsoft Edge mit den Inhaltstyp der Antwort ist Text/plain](formatting/_static/text-response.png)

Beachten Sie in diesem Fall die `Content-Type` zurückgegebene `text/plain`. Sie können dieses Verhalten mit nur einem Zeichenfolgentyp Antwort auch erreichen:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Für nicht triviale Aktionen mit mehreren Typen oder Optionen (z. B. verschiedene HTTP-Statuscodes, die abhängig vom Ergebnis der durchgeführten Operationen) zurückgeben, bevorzugen `IActionResult` als Rückgabetyp.

## <a name="content-negotiation"></a>Inhaltsaushandlung

Inhalts-Aushandlung (*Conneg* kurz) tritt auf, wenn der Client gibt eine [Accept-Header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Das Standardformat von Core ASP.NET-MVC verwendet ist JSON. Inhaltsaushandlung wird dadurch implementiert, `ObjectResult`. Es ist auch der bestimmten Aktionsergebnisse aus den Hilfsmethoden zurückgegebene Statuscode integriert (die basieren alle auf `ObjectResult`). Sie können auch einen Modelltyp (eine Klasse, die Sie, als der Datentyp für die Übertragung definiert haben) zurückgeben, und das Framework wird automatisch umgebrochen, ihn in ein `ObjectResult` für Sie.

Die folgende Aktionsmethode verwendet die `Ok` und `NotFound` Hilfsmethoden:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Eine JSON-formatierte Antwort wird zurückgegeben, es sei denn, ein anderes Format wurde angefordert, und der Server kann die angeforderte Format zurück. Sie können ein Tool wie [Fiddler](http://www.telerik.com/fiddler) erstellen eine Anforderung, die einen Accept-Header enthält, und geben Sie ein anderes Format. In diesem Fall, wenn der Server hat eine *Formatierer* können erzeugt eine Antwort im gewünschten Format, das Ergebnis wird zurückgegeben werden im Format Client bevorzugt.

![Fiddler-Konsole, die mit einem manuell erstellten GET-Anforderung mit einem Wert des Accept-Header Application/XML-](formatting/_static/fiddler-composer.png)

Im obigen Screenshot Fiddler Ersteller wurde verwendet, um eine Anforderung generieren angeben `Accept: application/xml`. Standardmäßig ASP.NET Core MVC unterstützt nur JSON, selbst wenn ein anderes Format angegeben wird, das zurückgegebene Ergebnis ist JSON-formatiert. Sie sehen, dass zusätzliche Formatierungsprogramme im nächsten Abschnitt hinzufügen.

Controlleraktionen können POCOs (Plain Old CLR Objects), zurück in diesem Fall ASP.NET MVC erstellt automatisch ein `ObjectResult` , der das Objekt umschließt. Der Client erhält das formatierte serialisierte Objekt (JSON-Format ist die Standardeinstellung; Sie können XML-Index oder anderen Formaten konfigurieren). Wenn das Objekt, das zurückgegeben wird `null`, und klicken Sie dann das Framework zurückgegeben wird ein `204 No Content` Antwort.

Zurückgeben eines Objekttyps:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

In diesem Beispiel erhält eine Anforderung für eine gültige Autor-Alias eine 200 OK-Antwort mit der Autor-Daten. Eine Anforderung für einen ungültigen Alias erhalten eine 204 No Content-Antwort. Screenshots, die mit der Antwort in XML und JSON-Formate werden unten gezeigt.

### <a name="content-negotiation-process"></a>Inhaltsaushandlung-Prozess

Inhalt *Aushandlung* nur erfolgt, wenn ein `Accept` Header angezeigt wird, in der Anforderung. Wenn eine Anforderung einen Accept-Header enthält, wird das Framework Listet die Medientypen im Accept-Header in der Rangfolge und versucht, einen Formatierer finden, der eine Antwort in einem der Formate, die durch den Accept-Header angegebenen erstellen kann. Für den Fall, dass kein Formatierer gefunden wird, kann die Anforderung des Clients erfüllen, wird das Framework versucht, den ersten Formatierer gefunden, die eine Antwort erstellen kann (es sei denn, der Entwickler die Option für konfiguriert hat `MvcOptions` zurückzugebenden 406 nicht akzeptabel stattdessen). Wenn die Anforderung XML-Datei gibt, aber der XML-Formatierer nicht konfiguriert wurde, wird der JSON-Formatierer verwendet werden. Üblicher, wenn kein Formatierer konfiguriert ist, der das angeforderte Format bereitstellen kann, wird der erste Formatierer, der das Objekt nicht formatieren kann verwendet. Keine Header angegeben ist, wird der erste Formatierer, der das Objekt, das zurückgegeben werden bewältigen können zum Serialisieren der Antwort verwendet werden. In diesem Fall gibt es keine stattgefunden hat Aushandlung - Server ist die Festlegung, in welchem Format verwendet werden.

> [!NOTE]
> Wenn die Accept-Header enthält `*/*`, der Header wird ignoriert, es sei denn, `RespectBrowserAcceptHeader` auf festgelegt ist auf "true" `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Browser und Inhaltsaushandlung

Im Gegensatz zu typischen-API-Clients Webbrowsern angeben tendenziell `Accept` Header, die eine Vielzahl von Formaten, einschließlich Platzhalter enthalten. Standardmäßig, wenn das Framework erkennt, dass die Anforderung von einem Browser eingeben, stammt ignoriert er die `Accept` Header und stattdessen return der Inhalt in der Anwendung konfigurierten Standardformat (JSON Wenn nicht anders konfiguriert). Dies bietet eine konsistente Umgebung Verwendung von verschiedenen Browsern APIs nutzen.

Wenn Sie Ihre Anwendung akzeptieren Browser accept-Header möchten, können Sie diese als Teil der Konfiguration des MVC-Konfiguration, durch Festlegen von `RespectBrowserAcceptHeader` auf `true` in der `ConfigureServices` Methode in *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Konfigurieren der Formatierer

Wenn Ihre Anwendung zusätzliche Formate jenseits der Standardgrenze von JSON unterstützen muss, können Sie NuGet-Pakete hinzufügen und Konfigurieren von MVC, um diese zu unterstützen. Es gibt separate Formatierungsprogramme für ein- und Ausgaben. Eingabe Formatierungsprogramme werden von verwendet [Modell Bindung](model-binding.md); Ausgabe Formatierungsprogramme werden verwendet, um Antworten zu formatieren. Sie können auch konfigurieren, [benutzerdefinierten Formatierer](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Hinzufügen von Unterstützung für XML-Format

Installieren Sie zum Hinzufügen der Unterstützung für XML-Formatierung der `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet-Paket.

Fügen Sie die XmlSerializerFormatters MVCs-Konfiguration in *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternativ können Sie einfach das Ausgabe-Formatierungsprogramm hinzufügen:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Diese beiden Ansätze werden Serialisierung von Ergebnissen mit `System.Xml.Serialization.XmlSerializer`. Falls gewünscht, können Sie die `System.Runtime.Serialization.DataContractSerializer` durch seine zugeordneten Formatierer hinzufügen:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Nachdem Sie die Unterstützung für XML-Formatierung hinzugefügt haben, sollte Ihre Controllermethoden das entsprechende Format basierend auf der Anforderung zurückgeben `Accept` -Header, als diese Fiddler veranschaulicht:

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Xml. Der Registerkarte "Raw" für die Antwort zeigt den Content-Type-Headerwert Application/XML.](formatting/_static/xml-response.png)

Sehen Sie in der Registerkarte "Inspektoren", die die Rohdaten GET-Anforderung erstellt wurde, mit einer `Accept: application/xml` Header festlegen. Die Antwort Bereich zeigt die `Content-Type: application/xml` -Header, und die `Author` Objekt in XML serialisiert wurde.

Verwenden Sie die Registerkarte "Composer" So ändern Sie die Anforderung an `application/json` in der `Accept` Header. Führen Sie die Anforderung und die Antwort als JSON formatiert:

![Fiddler-Konsole: das unformatierte Registerkarte für die Anforderung wird der Wert des Accept-Header ist Application/Json. Der Registerkarte "Raw" für die Antwort zeigt den Content-Type-Headerwert, der Anwendung/Json.](formatting/_static/json-response-fiddler.png)

In dieser Abbildung sehen Sie die Anforderung legt Header `Accept: application/json` und die Antwort gibt an, die identisch mit der `Content-Type`. Die `Author` Objekt im Text der Antwort im JSON-Format dargestellt wird.

### <a name="forcing-a-particular-format"></a>Erzwingen ein bestimmtes Format

Wenn Sie die Antwortformate für eine bestimmte Aktion einschränken möchten, können Sie, können Sie anwenden der `[Produces]` Filter. Die `[Produces]` Filter gibt die Antwortformate für eine bestimmte Aktion (oder Domänencontroller). Wie die meisten [Filter](../controllers/filters.md), dies kann auf die Aktion, Controller, oder im globalen Gültigkeitsbereich angewendet werden.

```csharp
[Produces("application/json")]
public class AuthorsController
```

Die `[Produces]` Filter wird erzwingen, dass alle Aktionen innerhalb der `AuthorsController` JSON-formatierte Antworten zurückgeben, selbst wenn andere Formatierungsprogramme für die Anwendung und vom Client bereitgestellten konfiguriert wurden ein `Accept` Anfordern von einem anderen Header verfügbar das Format. Finden Sie unter [Filter](../controllers/filters.md) erfahren Sie mehr, einschließlich Informationen zum Anwenden von Filtern Global.

### <a name="special-case-formatters"></a>Spezielle Formatierungsprogramme für die Groß-/Kleinschreibung

Einige Sonderfälle werden mithilfe der integrierten Formatierer implementiert. Standardmäßig `string` Rückgabetypen formatiert als *Text/Plain* (*Text/html* ggf. über `Accept` Header). Dieses Verhalten kann entfernt werden, durch das Entfernen der `TextOutputFormatter`. Entfernen Sie Formatierer in die `Configure` Methode im *Startup.cs* (siehe unten). Aktionen, die ein Modellobjekt haben zurückgeben wird Rückgabetyp 204 ohne Inhalt Antwort beim Zurückgeben der `null`. Dieses Verhalten kann entfernt werden, durch das Entfernen der `HttpNoContentOutputFormatter`. Das folgende Codebeispiel entfernt die `TextOutputFormatter` und `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Ohne die `TextOutputFormatter`, `string` Typen zurückgeben 406 nicht akzeptabel ist, z. B. Beachten Sie, dass wenn ein XML-Formatierer vorhanden ist, wird es formatieren `string` Typen zurückgeben, wenn die `TextOutputFormatter` wird entfernt.

Ohne die `HttpNoContentOutputFormatter`, null-Objekte mithilfe des konfigurierten Formatierers formatiert sind. Beispielsweise wird das JSON-Formatierungsprogramm einfach eine Antwort mit einem Text des zurückgeben `null`, während der XML-Formatierer ein leeres XML-Element mit dem Attribut zurückliefert `xsi:nil="true"` festgelegt.

## <a name="response-format-url-mappings"></a>Antwort Format URL Zuordnungen

Clients können Sie ein bestimmtes Format als Teil der URL, z. B. in der Abfragezeichenfolge oder einen Teil des Pfads oder mithilfe von Format-spezifische Erweiterung z. B. XML- oder JSON anfordern. In der Route an, die die API verwendet, sollte die Zuordnung von Anforderungspfad angegeben werden. Zum Beispiel:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Diese Route entschied sich auf das angeforderte Format als Erweiterung optional angegeben werden. Die `[FormatFilter]` Attribut überprüft das Vorhandensein des Werts Format in die `RouteData` und das Antwortformat wird an den entsprechenden Formatierer zugeordnet werden, wenn die Antwort erstellt wird.

| Route                      | Formatter                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | Der Standardformatierer-Ausgabe       |
| `/products/GetById/5.json` | Das JSON-Formatierungsprogramm (sofern konfiguriert) |
| `/products/GetById/5.xml`  | Der XML-Formatierer (sofern konfiguriert)  |
