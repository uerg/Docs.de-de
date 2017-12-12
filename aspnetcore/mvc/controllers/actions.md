---
title: Behandlung von Anforderungen mit Controller in ASP.NET Core MVC
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.assetid: 9da9eb52-8583-4069-af91-155ba3529d7f
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: 5dc6c7dc70027bb79875f389d535119a2543b873
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a>Behandlung von Anforderungen mit Controller in ASP.NET Core MVC

Durch [Steve Smith](https://ardalis.com/) und [Scott Addie](https://github.com/scottaddie)

Domänencontroller, Aktionen und Aktionsergebnisse sind grundlegender Bestandteil der Entwickler apps mithilfe von ASP.NET Core MVC Aufbau auf.

## <a name="what-is-a-controller"></a>Was ist ein Domänencontroller?

Ein Controller wird verwendet, um zu definieren und eine Reihe von Aktionen zu gruppieren. Eine Aktion (oder *Aktionsmethode*) ist eine Methode auf einem Domänencontroller der Anforderungen verarbeitet. Logische Gruppierung Controller ähnliche Aktionen zusammen. Diese Aggregation von Aktionen kann allgemeine Sätze von Regeln, z. B. routing, Zwischenspeichern und Autorisierung, zusammen angewendet werden. Anforderungen werden über Aktionen zugeordnet [routing](xref:mvc/controllers/routing).

Gemäß der Konvention Controllerklassen:
* Befinden sich in der Stammebene des Projekts *Controller* Ordner
* Erben von`Microsoft.AspNetCore.Mvc.Controller`

Ein Controller ist eine instanziierbare Klasse, die in der mindestens eine der folgenden Bedingungen "true" ist:
* Der Name der Klasse wird mit "Controller" Formatangabe.
* Die Klasse erbt von einer Klasse, deren Name mit "Controller" Formatangabe ist
* Die Klasse wird mit ergänzt die `[Controller]` Attribut

Eine Controllerklasse dürfen keine zugeordnete `[NonController]` Attribut.

Domänencontroller sollten folgen der [expliziten Abhängigkeiten Prinzip](http://deviq.com/explicit-dependencies-principle/). Es gibt einige Ansätze für die Implementierung dieses Prinzips. Wenn mehrere Controlleraktionen desselben Diensts benötigen, erwägen Sie [Konstruktoreinfügung](xref:mvc/controllers/dependency-injection#constructor-injection) solcher Abhängigkeiten anfordern. Wenn der Dienst nur einer einzigen Aktion-Methode erforderlich ist, erwägen Sie [Aktion Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) zum Anfordern der Abhängigkeitsverhältnis.

Innerhalb der **M**Odel -**V**vorhandenes -**C**Ontroller-Muster, ein Controller ist verantwortlich für die ersten Verarbeitung der Anforderung und Instanziierung des Modells. Im Allgemeinen sollte die geschäftlichen Entscheidungen innerhalb des Modells ausgeführt werden.

Der Controller nutzt das Ergebnis der Verarbeitung des Modells, (sofern vorhanden) und gibt die richtige Ansicht und deren zugeordneten Daten oder das Ergebnis des API-Aufrufs. Weitere Informationen zu [Übersicht über ASP.NET Core MVC](xref:mvc/overview) und [erste Schritte mit ASP.NET Core MVC und Visual Studio](xref:tutorials/first-mvc-app/start-mvc).

Der Controller ist ein *-Benutzeroberflächenebene* Abstraktion. Ihren Aufgaben sind, um sicherzustellen, dass Anforderungsdaten gültig ist und auswählen, welche Ansicht (oder das Ergebnis für eine API) zurückgegeben werden sollen. In gut ausgearbeitete apps ist es nicht direkt Daten zugreifen oder die Geschäftslogik enthalten. Stattdessen werden der Controller an Diensten behandeln diese Aufgaben delegiert.

## <a name="defining-actions"></a>Definieren von Aktionen

Öffentliche Methoden auf einem Domänencontroller, außer denen mit ergänzt die `[NonAction]` -Attribut angegeben wird, werden die Aktionen. Parameter für Aktionen Anfordern von Daten gebunden werden und werden überprüft mithilfe von [modellbindung](xref:mvc/models/model-binding). Eine modellvalidierung erfolgt für alle Elemente, das Modell gebunden ist. Die `ModelState.IsValid` Eigenschaftswert angibt, ob die modellbindung und Überprüfung war erfolgreich.

Aktionsmethoden sollte die Logik für die Zuordnung von einer Anforderungs zu Besorgnis Business enthalten. Geschäftsprobleme sollte als Dienste, die der Controller über zugreift i. d. r. dargestellt werden [Abhängigkeitsinjektion](xref:mvc/controllers/dependency-injection). Aktionen werden Anwendungsstatus ist dann das Ergebnis der Aktion Business zuordnen.

Aktionen können nichts zurück, aber häufig zurückgeben eine Instanz von `IActionResult` (oder `Task<IActionResult>` für asynchrone Methoden), die eine Antwort erzeugt. Die Aktionsmethode ist verantwortlich für die Auswahl *welche Art von Antwort*. Das Aktionsergebnis *ist der reagiert*.

### <a name="controller-helper-methods"></a>Controller-Hilfsmethoden

Domänencontroller in der Regel erben [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), obwohl dies nicht erforderlich ist. Ableiten von `Controller` ermöglicht den Zugriff auf drei Kategorien von Hilfsmethoden:

#### <a name="1-methods-resulting-in-an-empty-response-body"></a>1. Methoden, was zu einer leeren Antworttext

Nicht `Content-Type` HTTP-Antwortheader enthalten, ist, seit der Antworttext verfügt nicht über die Inhalte zu beschreiben.

Es gibt zwei Typen in dieser Kategorie: Umleitung und HTTP-Statuscode.

* **HTTP-Statuscode**

    Dieser Typ gibt einen HTTP-Statuscode zurück. Einige Hilfsmethoden dieses Typs sind `BadRequest`, `NotFound`, und `Ok`. Beispielsweise `return BadRequest();` erzeugt einen Statuscode "400" bei der Ausführung. Bei Methoden, z. B. `BadRequest`, `NotFound`, und `Ok` sind überladen sind, sollten sie nicht mehr gelten als Responder Statuscode "HTTP", da inhaltsaushandlung stattfindet.

* **Umleiten**

    Dieser Typ gibt eine Umleitung an eine Aktion oder ein Ziel zurück (mit `Redirect`, `LocalRedirect`, `RedirectToAction`, oder `RedirectToRoute`). Beispielsweise `return RedirectToAction("Complete", new {id = 123});` leitet an `Complete`, ein anonymes Objekt übergeben.

    Der Ergebnistyp für die Umleitung unterscheidet sich von der HTTP-Statuscode-Typ in erster Linie in das Hinzufügen einer `Location` HTTP-Antwortheader.

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a>2. Methoden, die in einen nicht leeren Antworttext durch einen vordefinierten Inhaltstyp resultierende

Die meisten Hilfsmethoden in dieser Kategorie gehören eine `ContentType` Eigenschaft, die Sie festlegen, sodass die `Content-Type` Antwortheader zum Beschreiben des Antworttexts.

Es gibt zwei Typen in dieser Kategorie: [Ansicht](xref:mvc/views/overview) und [Antwort formatiert](xref:mvc/models/formatting).

* **Ansicht**

    Dieser Typ zurückgibt, eine Sicht, die ein Modell verwendet wird, um das Rendering von HTML. Beispielsweise `return View(customer);` übergibt ein Modell zur Ansicht für die Datenbindung.

* **Formatierte Antwort**

    Dieser Typ zurückgibt, JSON oder eine ähnliche Datenaustauschformat, um ein Objekt in einer bestimmten Weise darzustellen. Beispielsweise `return Json(customer);` serialisiert das angegebene Objekt in JSON-Format.
    
    Dieses Typs andere übliche Methoden umfassen `File`, `PhysicalFile`, und `VirtualFile`. Beispielsweise `return PhysicalFile(customerFilePath, "text/xml");` gibt eine XML-Datei beschrieben durch einen `Content-Type` Wert des Antwortheaders der "Text/Xml".

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a>3. Methoden, die in einen nicht leeren Antworttext resultierenden in einen Inhaltstyp, der mit dem Client ausgehandelt formatiert

Diese Kategorie ist eine bessere Leistung als **Inhaltsaushandlung**. [Inhalts-Aushandlung](xref:mvc/models/formatting#content-negotiation) gilt immer, wenn eine Aktion gibt eine [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) Typ oder einen anderen Wert als eine [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) Implementierung. Eine Aktion, die ein nicht-gibt`IActionResult` Implementierung (z. B. `object`) gibt auch eine Antwort formatiert.

Einige Hilfsmethoden dieses Typs enthalten `BadRequest`, `CreatedAtRoute`, und `Ok`. Beispiele für diese Methoden sind `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, und `return Ok(value);`zugeordnet. Beachten Sie, dass `BadRequest` und `Ok` führen Sie die inhaltsaushandlung nur, wenn ein Wert übergeben, ohne einen Wert, sie stattdessen dienen als HTTP-Statuscode: Ergebnistypen. Die `CreatedAtRoute` -Methode, andererseits, immer inhaltsaushandlungen seit seiner Überladungen, die alle erfordern, dass ein Wert übergeben werden.

### <a name="cross-cutting-concerns"></a>Querschnittliche Sicherheitsrisiken

Anwendungen gemeinsam in der Regel Teile ihrer Workflows verwenden. Beispiele sind eine app, die eine Authentifizierung auf dem Einkaufswagen erforderlich ist, oder eine app, die Daten auf einige Seiten zwischengespeichert. Um die Logik vor oder nach einer Aktionsmethode ausführen, verwenden Sie eine *Filter*. Mit [Filter](xref:mvc/controllers/filters) querschnittliche Bedenken können Duplikate, die ihnen ermöglicht, führen reduziert den [Don't wiederholen selbst (trocken)-Prinzip](http://deviq.com/don-t-repeat-yourself/).

Die meisten Attribute, z. B. filtern `[Authorize]`, auf der Ebene Controller bzw. die Aktionsmethode, die je nach der gewünschten Ebene der Granularität angewendet werden können.

Fehlerbehandlung und Zwischenspeichern von Antworten sind häufig querschnittliche bedenken:
   * [Fehlerbehandlung](xref:mvc/controllers/filters#exception-filters)
   * [Zwischenspeichern von Antworten](xref:performance/caching/response)

Viele Aspekte der querschnittliche können verarbeitet werden, mithilfe von Filtern oder benutzerdefinierte [Middleware](xref:fundamentals/middleware).
