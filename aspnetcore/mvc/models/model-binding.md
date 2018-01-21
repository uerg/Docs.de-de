---
title: Wurden die Modellbindung
author: rick-anderson
description: 
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 84b9c5dc3a87b739affaeaecaa180d1b01f49b8e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="model-binding"></a>Wurden die Modellbindung

Durch [Rachel Appel](https://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>Einführung in die Bindung zu modellieren.

Wurden die modellbindung in ASP.NET Core MVC ordnet Daten von HTTP-Anforderungen an die Aktionsmethodenparameter. Die Parameter werden einfache Typen wie Zeichenfolgen, ganze Zahlen und Gleitkommazahlen, oder es handelt sich um komplexe Typen. Dies ist eine großartige Funktion von MVC, da Zuordnen von eingehenden Daten zu einem Gegenstück ein Szenario häufig wiederholte unabhängig von der Größe und Komplexität der Daten ist. MVC löst dieses Problem, indem ohne Bindung Entwickler müssen also keine etwas abweichende Version des gleichen Codes in jeder app umschreiben zu halten. Schreiben Ihren eigenen Text Konvertercode Typ ist mühsam, und fehleranfällig.

## <a name="how-model-binding-works"></a>Funktionsweise der modellbindung

Wenn MVC eine HTTP-Anforderung empfängt, leitet sie es an eine bestimmte Aktionsmethode eines Controllers weiter. Bestimmt die Aktionsmethode ausgeführt basierend auf die in der Routendaten, und klicken Sie dann Werte aus der HTTP-Anforderung an diese Aktionsmethode Parameter gebunden. Betrachten Sie beispielsweise die folgende URL:

`http://contoso.com/movies/edit/2`

Da die routenvorlage, aussieht `{controller=Home}/{action=Index}/{id?}`, `movies/edit/2` leitet an die `Movies` Controller, und die zugehörige `Edit` Aktionsmethode. Außerdem akzeptiert einen optionalen Parameter namens `id`. Der Code für die Aktionsmethode sollte etwa wie folgt aussehen:

```csharp
public IActionResult Edit(int? id)
   ```

Hinweis: Die Zeichenfolgen in der URL-Route sind nicht Groß-/Kleinschreibung beachtet.

MVC versucht, die Anforderungsdaten an die Aktionsparameter anhand des Namens zu binden. MVC sucht nach Werten für jeden Parameter mit dem Parameternamen und die Namen der öffentlichen festlegbaren Eigenschaften. Im obigen Beispiel ist die einzige Aktionsparameter heißt `id`, MVC auf den Wert mit dem gleichen Namen in die Routenwerte gebunden. Zusätzlich zu den Routenwerte MVC wird Binden von Daten aus verschiedenen Teilen der Anforderung und wird in eine festgelegte Reihenfolge. Im folgenden finden Sie eine Liste der Datenquellen in der Reihenfolge, die über diese wurden die modellbindung aussieht:

1. `Form values`: Hierbei handelt es sich um Formularwerte, die in der HTTP-Anforderung POST-Methode verwenden. (z. B. jQuery-POST-Anforderungen).

2. `Route values`: Der Satz von Routenwerte gebotenen [Routing](../../fundamentals/routing.md)

3. `Query strings`: Die Zeichenfolge Abfrageteil des URIS.

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

Hinweis: Bilden Sie Werte Routendaten und Abfrage-Zeichenfolgen als Name / Wert-Paare gespeichert sind.

Da wurden die modellbindung, einen Schlüssel Namens aufgefordert `id` nichts muss mit dem Namen `id` in die Formularwerte es in verschoben, die Routenwerte für diesen Schlüssel zu suchen. In unserem Beispiel ist es eine Übereinstimmung. Bindung erfolgt, und der Wert wird auf die ganze Zahl 2 konvertiert. Bearbeiten (Zeichenfolgen-Id) mit derselben Anforderung würde in die Zeichenfolge "2" konvertiert werden.

Bisher wird die einfache Typen verwendet. Simple-Typen sind in MVC eine beliebige .NET primitiven Typ oder ein Typ mit einen Typkonverter für die Zeichenfolge an. Wenn eine Klasse von der Aktionsmethode Parameter wie z. B. wurden der `Movie` -Typ, der einfache und komplexe Typen enthält, wie die Eigenschaften MVCs-Modell Bindung wird weiterhin ordentlich behandeln. Er verwendet Reflektion und Rekursion, um die Eigenschaften der komplexen Typen, die Suche nach Übereinstimmungen zu durchlaufen. Wurden die modellbindung sucht nach dem Muster *parameter_name.property_name* an Eigenschaften Werte zu binden. Wenn es nicht übereinstimmende Werte dieses Formulars findet, versucht zu binden, indem einfach den Namen der Eigenschaft. Für diese Typen wie z. B. `Collection` Typen, wurden die modellbindung sucht nach Übereinstimmungen mit *Parameter_name [Index]* oder einfach *[Index]*. Modell Bindung behandelt `Dictionary` Typen auf ähnliche Weise, in der die *Parameter_name [Key]* oder einfach *[Key]*, solange die Schlüssel einfache Typen sind. Schlüssel, die unterstützt werden entsprechen den Feldnamen HTML und den Tag-Hilfsprogramme, die für den Typ des gleichen Modells generiert. Dies ermöglicht Round-Tripping-Werte, die Felder mit der Eingabe des Benutzers, für deren Vereinfachung gefüllt beispielsweise bleiben, wenn gebundene Daten aus einer erstellen oder bearbeiten die Validierung nicht bestanden haben.

Damit Bindung aufweisen muss die Klasse einen öffentlichen Standardkonstruktor verfügen und Member zu bindenden muss auf öffentlichen beschreibbare Eigenschaften. Wenn die modellbindung erfolgt, dass die Klasse mit dem öffentlichen Standardkonstruktor nur instanziiert werden, können die Eigenschaften festgelegt werden.

Wenn ein Parameter gebunden ist, wurden die modellbindung beendet die Suche nach Werten, die mit diesem Namen verschoben und auf den nächsten Parameter binden. Andernfalls wird das Standardverhalten für Modell Bindung Parameter mit ihren Standardwerten je nach ihrem Typ:

* `T[]`: Mit Ausnahme des Arrays des Typs `byte[]`, Bindung legt Parameter des Typs `T[]` auf `Array.Empty<T>()`. Arrays des Typs `byte[]` festgelegt `null`.

* Verweistypen: Bindung erstellt eine Instanz einer Klasse mit dem Standardkonstruktor ohne Festlegen von Eigenschaften. Allerdings Modell Bindung legt `string` Parameter `null`.

* Auf NULL festlegbare Typen: Typen mit Nullwert festgelegt `null`. Im obigen Beispiel Modell Bindung legt `id` auf `null` , da er vom Typ ist `int?`.

* Werttypen: NULL-Wert-Typen des Typs `T` festgelegt `default(T)`. Beispielsweise wurden die modellbindung Festlegen eines Parameters wird `int id` auf 0. Können Sie modellvalidierung oder auf NULL festlegbare Typen verwenden, statt der vertrauenden Seite auf die Standardwerte.

Wenn die Bindung ein Fehler auftritt, löst MVC keine Fehler. Jede Aktion, die eine Benutzereingabe akzeptiert Prüfen der `ModelState.IsValid` Eigenschaft.

Hinweis: Jeder Eintrag in des Controllers `ModelState` Eigenschaft ist ein `ModelStateEntry` , enthält eine `Errors` Eigenschaft. Es ist nur selten notwendig, diese Sammlung selbst abzufragen. Verwenden Sie stattdessen `ModelState.IsValid`.

Darüber hinaus sind einige spezielle Datentypen, die beim Ausführen der modellbindung MVC berücksichtigen:

* `IFormFile`, `IEnumerable<IFormFile>`: Eine oder mehrere hochgeladene Dateien, die Teil der HTTP-Anforderung sind.

* `CancellationToken`: Wird verwendet, um die Aktivität in asynchrone Controller "Abbrechen".

Diese Typen können Action-Parameter oder Eigenschaften eines Klassentyps gebunden werden.

Nach Abschluss der modellbindung [Überprüfung](validation.md) auftritt. Standard-modellbindung funktioniert hervorragend für die meisten Entwicklungsszenarien. Es ist außerdem erweiterbar, sodass, wenn Sie spezielle Anforderungen haben Sie die integrierten Verhalten anpassen können.

## <a name="customize-model-binding-behavior-with-attributes"></a>Modell Bindungsverhalten mit Attributen anpassen

MVC enthält mehrere Attribute, die Sie zum Weiterleiten von sein Standardverhalten für die Bindung von Modell mit einer anderen Datenquelle verwenden können. Sie können beispielsweise angeben, ob die Bindung für eine Eigenschaft erforderlich ist, oder wenn er überhaupt niemals mit geschehen soll die `[BindRequired]` oder `[BindNever]` Attribute. Alternativ können Sie die Standarddatenquelle überschreiben, und geben Sie den Modellbinder-Datenquelle. Es folgt eine Liste von Modell Binden von Attributen:

* `[BindRequired]`: Dieses Attribut fügt einen Modellfehler für Status an, wenn Bindung erfolgen kann.

* `[BindNever]`: Weist den Modellbinder nie an diesen Parameter zu binden.

* `[FromHeader]`, `[FromQuery]`, `[FromRoute]`, `[FromForm]`: Verwenden Sie diese an die genaue Bindungsquelle, anwenden möchten.

* `[FromServices]`: Dieses Attribut verwendet [Abhängigkeitsinjektion](../../fundamentals/dependency-injection.md) zum Binden von Parametern von Diensten.

* `[FromBody]`: Verwenden Sie die konfigurierten Formatierer zum Binden von Daten aus dem Anforderungstext ein. Der Formatierer ausgewählt ist, basierend auf den Inhaltstyp der Anforderung.

* `[ModelBinder]`: Wird verwendet, um den Standardmodellbinder, Bindungsquelle und den Namen zu überschreiben.

Attribute sind sehr hilfreiche Tools aus, wenn Sie das Standardverhalten der modellbindung überschreiben müssen.

## <a name="binding-formatted-data-from-the-request-body"></a>Bindung von formatierten Daten aus dem Anforderungstext

Anforderungsdaten können in einer Vielzahl von Formaten, einschließlich JSON, XML und viele andere stammen. Wenn Sie das Attribut [FromBody] verwenden, um anzugeben, dass Sie einen Parameter an Daten im Anforderungstext binden möchten, verwendet MVC eine konfigurierte Sammlung der Formatierer, um die Daten basierend auf den Inhaltstyp zu behandeln. Standardmäßig MVC umfasst eine `JsonInputFormatter` -Klasse für die Behandlung von JSON-Daten, aber Sie die zusätzliche Formatierungsprogramme hinzufügen kann, für die Behandlung von XML- und anderen benutzerdefinierten Formaten.

> [!NOTE]
> Es kann höchstens einen Parameter pro Aktion mit ergänzt `[FromBody]`. Die ASP.NET-MVC-Kern-Laufzeit delegiert die Verantwortung der Anforderungsdatenstrom an dem Formatierer zu lesen. Sobald der Anforderungsdatenstrom für einen Parameter gelesen wird, kann im Allgemeinen nicht zum Lesen des Anforderungsstreams erneut zum Binden von anderen `[FromBody]` Parameter.

> [!NOTE]
> Die `JsonInputFormatter` ist der Standardformatierer und basiert auf [Json.NET](https://www.newtonsoft.com/json).

ASP.NET wählt Eingabe Formatierungsprogramme basierend auf der [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) Header und den Typ des Parameters, es sei denn, es ein Attribut angewendet wird ist, andernfalls angibt. Wenn Sie XML-Code verwenden möchten, oder ein anderes Format Sie sie in konfigurieren müssen der *Startup.cs* -Datei, aber Sie ggf. zuerst müssen Sie einen Verweis auf erhalten `Microsoft.AspNetCore.Mvc.Formatters.Xml` mithilfe von NuGet. Der Startcode sollte etwa wie folgt aussehen:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddXmlSerializerFormatters();
   }
```

Im Code der *Startup.cs* -Datei enthält eine `ConfigureServices` Methode mit einer `services` Argument Sie Dienste für Ihre ASP.NET-Anwendung zu erstellen können. In diesem Beispiel werden wir eine XML-Formatierer als Dienst hinzugefügt, die für diese app MVC bereitstellt. Die `options` übergebenen Argument den `AddMvc` Methode ermöglicht das Hinzufügen und Verwalten von Filtern, Formatierungsprogramme und andere von Systemoptionen von MVC beim Start der app. Wenden Sie dann die `Consumes` -Attribut auf Controllerklassen oder Aktionsmethoden, die mit dem Format verwendet werden sollen.

### <a name="custom-model-binding"></a>Benutzerdefinierte wurden die Modellbindung

Sie können wurden die modellbindung erweitern, indem Sie Ihre eigenen benutzerdefinierten Modellbinder schreiben. Erfahren Sie mehr über [benutzerdefinierte wurden die modellbindung](../advanced/custom-model-binding.md).
