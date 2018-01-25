---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medienformatierer in der ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 9103574597df126a22e21a2f51815f608e46f47f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Medienformatierer in der ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Lernprogramm werden unterstützen zusätzliche Media-Formate wie in ASP.NET Web-API.

## <a name="internet-media-types"></a>Internet-Medientypen

Ein Medientyp, der eine MIME-Typ, gibt das Format der Daten. Bei HTTP beschreiben Medientypen das Format des Nachrichtentexts. Ein Medientyp besteht aus zwei Zeichenfolgen, die einen Typ und Untertyp. Zum Beispiel:

- Text/html
- image/png
- application/json

Wenn eine HTTP-Nachricht über einen Entitätskörper enthält, gibt der Content-Type-Header das Format des Nachrichtentexts. Dieser Wert ermöglicht dem Empfänger, Analysieren des Inhalts des Nachrichtentexts.

Z. B. eine HTTP-Antwort ein PNG-Bild enthält, möglicherweise die Antwort die folgenden Header enthalten.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Wenn der Client eine Anforderungsnachricht sendet, kann er einen Accept-Header umfassen. Der Accept-Header informiert, dass es sich bei der Server, auf die Medien den Client Typs vom Server möchte. Zum Beispiel:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Dieser Header weist den Server, dass der Client entweder HTML, XHTML oder XML-wünscht.

Der Medientyp bestimmt wie serialisiert und deserialisiert den Nachrichtentext für die HTTP-Web-API. Web-API verfügt über integrierte Unterstützung für XML, JSON, BSON und Formular codierte Daten, und Sie können zusätzliche Medientypen unterstützen, indem Sie das Schreiben von einem *medienformatierer*.

Um einen medienformatierer zu erstellen, werden Sie von einer dieser Klassen abgeleitet:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). In dieser Klasse verwendet den asynchronen Lesevorgang und Write-Methoden.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Diese Klasse leitet sich von **MediaTypeFormatter** jedoch Sychronous Lese-/Schreibzugriff Methoden verwendet.

Ableiten von **BufferedMediaTypeFormatter** ist einfacher, da kein asynchronen Code vorhanden ist, aber es bedeutet auch, dass der aufrufende Thread kann während der e/a blockiert.

## <a name="example-creating-a-csv-media-formatter"></a>Beispiel: Erstellen einer CSV-Medienformatierer

Das folgende Beispiel zeigt einen medientypformatierer, die ein Produktobjekt in ein Format mit kommagetrennten Werten (CSV) serialisieren kann. Dieses Beispiel verwendet die Product-Typs definiert, die im Lernprogramm [erstellen eine Web-API, CRUD-Vorgänge unterstützt](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Hier ist die Definition des Objekts Produkt:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Um ein CSV-Formatierungsprogramm zu implementieren, definieren Sie eine Klasse, die abgeleitet **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Fügen Sie im Konstruktor die Medientypen, die den Formatierer unterstützt. In diesem Beispiel wird der Formatierer einen einziger Medientyp unterstützt &quot;Text/Csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Überschreiben Sie die **CanWriteType** Methode, um anzugeben, welchen den Formatierer Typs serialisieren kann:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In diesem Beispiel kann das Formatierungsprogramm einzelne Serialisieren `Product` sowie Auflistungen von Objekten `Product` Objekte.

Auf ähnliche Weise, überschreiben die **CanReadType** Methode, um anzugeben, welchen den Formatierer Typs deserialisieren kann. In diesem Beispiel wird das Formatierungsprogramm unterstützt keine Deserialisierung ab, damit die Methode gibt einfach auftragsantwortnachrichten zurück **"false"**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Überschreiben Sie abschließend die **WriteToStream** Methode. Diese Methode serialisiert einen Typ durch Schreiben in einen Stream. Wenn der Formatierer Deserialisierung unterstützt, auch überschreiben, die **ReadFromStream** Methode.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Die Web-API-Pipeline hinzugefügt eine Medienformatierer

Verwenden Sie zum Hinzufügen einer medientypformatierer für die Web-API-Pipeline die **Formatierungsprogramme** Eigenschaft auf die **HttpConfiguration** Objekt.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Zeichencodierungen

Optional kann ein medienformatierer mehrere zeichencodierungen, z. B. UTF-8 oder ISO 8859-1 unterstützen.

Fügen Sie im Konstruktor, eine oder mehrere [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Typen für die **SupportedEncodings** Auflistung. Fügen Sie zuerst die standardcodierung.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

In der **WriteToStream** und **ReadFromStream** Aufrufen von Methoden, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) , wählen Sie die bevorzugte zeichencodierung. Diese Methode entspricht der Anforderungsheader für die Liste der unterstützten Codierungen. Verwenden Sie das zurückgegebene **Codierung** beim Lesen oder aus dem Stream schreiben:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
