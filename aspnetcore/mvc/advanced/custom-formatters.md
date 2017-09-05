---
title: Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs
author: tdykstra
description: "Informationen Sie zum Erstellen und Verwenden von benutzerdefinierten Formatierer für Web-APIs in ASP.NET Core."
keywords: ASP.NET Core, web-api, benutzerdefinierten Formatierer
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 1fb6fdc2-e199-4469-9012-b909d1913422
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: af3b2174c73583832868d2062e6c7ab4689a1229
ms.sourcegitcommit: 9d3f27a1ee5b7014fb40e4f2ec9b2a9cd744751c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2017
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>Benutzerdefinierten Formatierer in ASP.NET Core MVC-Web-APIs

Durch [Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC verfügt über integrierte Unterstützung für den Datenaustausch im Web-APIs mit JSON, XML oder nur-Text-Formate. In diesem Artikel wird die Unterstützung für zusätzliche Formate hinzufügen, durch das Erstellen von benutzerdefinierten Formatierer veranschaulicht.

[Anzeigen oder Herunterladen des Beispiels aus GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/Sample).

## <a name="when-to-use-custom-formatters"></a>Verwenden von benutzerdefinierten Formatierer

Verwenden Sie ein benutzerdefiniertes Formatierungsprogramm aus, wenn Sie möchten die [content-Aushandlung](xref:mvc/models/formatting) Prozess, um einen Inhaltstyp zu unterstützen, die von den integrierten Formatierungsprogrammen (JSON, XML und nur-Text) nicht unterstützt.

Wenn einige Clients für Ihre Web-API verarbeiten kann z. B. die [Protobuf](https://github.com/google/protobuf) Format, sollten Sie die Protobuf mit diesen Clients zu verwenden, da sie effizienter ist.  Vielleicht möchten Sie Ihre Web-API zum Senden von Kontaktnamen und Adressen in [vCard](https://en.wikipedia.org/wiki/VCard) Format, eine häufig verwendete Format zum Austauschen von Daten. Die Beispiel-app, die mit diesem Artikel bereitgestellte implementiert einen einfachen vCard-Formatierer.

## <a name="overview-of-how-to-use-a-custom-formatter"></a>Übersicht darüber, wie ein benutzerdefiniertes Formatierungsprogramm verwenden

Es folgen die Schritte zum Erstellen und verwenden ein benutzerdefiniertes Formatierungsprogramm:

* Erstellen Sie eine Ausgabe Formatierer-Klasse, wenn Serialisieren von Daten, die an den Client gesendet werden sollen.
* Erstellen Sie eine Eingabe Formatierungsprogrammklasse, wenn beim Deserialisieren von Daten, die vom Client empfangen werden sollen. 
* Fügen Sie Instanzen der Formatierer den `InputFormatters` und `OutputFormatters` Auflistungen in [MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions).

Die folgenden Abschnitte enthalten Anleitungen und Codebeispiele für jede der folgenden Schritte aus.

## <a name="how-to-create-a-custom-formatter-class"></a>Vorgehensweise: erstellen eine benutzerdefinierten Formatierungsklasse

So erstellen Sie einen Formatierer

* Leiten Sie Klasse aus der entsprechenden Basisklasse.
* Geben Sie gültige Medientypen und Codierungen im Konstruktor.
* Überschreiben Sie `CanReadType` / `CanWriteType` Methoden
* Überschreiben Sie `ReadRequestBodyAsync` / `WriteResponseBodyAsync` Methoden
  
### <a name="derive-from-the-appropriate-base-class"></a>Von der entsprechenden Basisklasse abgeleitet werden

Text-Medientypen (z. B. vCard), leiten Sie von der [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter) oder [TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter) Basisklasse.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

Leiten Sie für binäre Typen aus den [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter) oder [OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter) Basisklasse.

### <a name="specify-valid-media-types-and-encodings"></a>Geben Sie gültige Medientypen und Codierungen

Geben Sie im Konstruktor gültige Medientypen und Codierungen durch Hinzufügen der `SupportedMediaTypes` und `SupportedEncodings` Sammlungen.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> Abhängigkeitsinjektion in einer Formatierungsklasse Konstruktor ist nicht möglich. Beispielsweise kann keine Protokollierung durch Hinzufügen eines Parameters für die Protokollierung an den Konstruktor nicht abgerufen werden. Um auf Dienste zuzugreifen, müssen Sie das Context-Objekt verwenden, das in Ihrer Methoden übergeben werden. Ein Codebeispiel hierfür [unten](#read-write) veranschaulicht dies.

### <a name="override-canreadtypecanwritetype"></a>Überschreiben Sie CanReadType/CanWriteType 

Geben Sie den können in deserialisiert oder Serialisieren von durch Überschreiben der `CanReadType` oder `CanWriteType` Methoden. Beispielsweise nur möglicherweise können zum Erstellen von vCard Text aus einer `Contact` Typ und umgekehrt.

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>Die CanWriteResult-Methode

In einigen Szenarien müssen Sie außer Kraft setzen `CanWriteResult` anstelle von `CanWriteType`. Verwendung `CanWriteResult` wenn Folgendes zutrifft:

  * Die Aktionsmethode gibt eine Modellklasse zurück.
  * Es sind abgeleitete Klassen, die zur Laufzeit zurückgegeben werden können.
  * Sie müssen wissen zur Laufzeit die abgeleitete Klasse wird von der Aktion zurückgegeben wurde.  

Nehmen wir beispielsweise an Ihre Aktion Methodensignatur gibt eine `Person` aufweisen, aber es gelegten eine `Student` oder `Instructor` aus abgeleiteter Typ `Person`. Wenn der Formatierer, nur behandeln soll `Student` Objekte, überprüfen Sie den Typ des [Objekt](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object) in das bereitgestellte Kontextobjekt der `CanWriteResult` Methode. Beachten Sie, dass es nicht notwendig, verwenden Sie `CanWriteResult` bei Rückgabe der Aktionsmethode `IActionResult`; in diesem Fall die `CanWriteType` Methode empfängt den Laufzeittyp.

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>Überschreiben Sie ReadRequestBodyAsync/WriteResponseBodyAsync 

Führen Sie den eigentlichen deserialisiert oder Serialisieren `ReadRequestBodyAsync` oder `WriteResponseBodyAsync`.  Die hervorgehobenen Zeilen im folgenden Beispiel werden veranschaulicht Services von der abhängigkeitseinschleusungscontainer abrufen (Sie können nicht aus Konstruktorparameter sie erhalten).

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>Konfigurieren von MVC, um ein benutzerdefiniertes Formatierungsprogramm verwenden
 
Um ein benutzerdefiniertes Formatierungsprogramm verwenden, fügen Sie eine Instanz der Klasse Formatierer in die `InputFormatters` oder `OutputFormatters` Auflistung.

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

Formatierungsprogramme werden ausgewertet, in der Reihenfolge an, dass Sie sie einfügen. Die erste Vorlage hat Vorrang vor. 

## <a name="next-steps"></a>Nächste Schritte

Finden Sie unter der [beispielanwendung](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/Sample), implementiert einfache vCard Eingabe und Ausgabe-Formatierer.  Die Anwendung liest und schreibt vCards, die wie im folgenden Beispiel aussehen:

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

Sehen vCard auszugeben, führen Sie die Anwendung und senden eine Get-Anforderung mit Accept-Header "Text/Vcard" `http://localhost:63313/api/contacts/` (Wenn in Visual Studio ausgeführt wird) oder `http://localhost:5000/api/contacts/` (wenn von der Befehlszeile aus ausgeführt wird).

Um die in-Memory-Sammlung von Kontakten vCard hinzugefügt haben, senden Sie eine Post-Anforderung zur gleichen URL, mit der Content-Type-Header "Text/Vcard" und vCard Text im Hauptteil, wie im Beispiel oben formatiert.
