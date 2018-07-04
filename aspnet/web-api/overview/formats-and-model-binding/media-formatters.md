---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medienformatierer in der ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 824fea5a8837ff8b09af832b7d2a094c7d82907b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368915"
---
<a name="media-formatters-in-aspnet-web-api-2"></a>Medienformatierer in der ASP.NET-Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

In diesem Tutorial wird gezeigt, wie zusätzliche Formate in ASP.NET Web-API unterstützt.

## <a name="internet-media-types"></a>Internet-Medientypen

Ein Medientyp, der einen MIME-Typ, so genannte identifiziert das Format der Daten. In HTTP beschreiben Medientypen das Format des Nachrichtentexts. Ein anderes Medium besteht aus zwei Zeichenfolgen, einen Typ und Untertyp. Zum Beispiel:

- Text/html
- Image/png
- application/json

Wenn eine HTTP-Nachricht über einen Entitätskörper enthält, gibt die Content-Type-Headers das Format des Nachrichtentexts. Dies weist dem Empfänger auf den Inhalt des Nachrichtentexts zu analysieren.

Wenn eine HTTP-Antwort ein PNG-Bild enthält, kann z. B. die Antwort die folgenden Header erhalten haben.

[!code-console[Main](media-formatters/samples/sample1.cmd)]

Wenn der Client eine Anforderungsnachricht sendet, kann er einen Accept-Header enthalten. Der Accept-Header informiert, dass der Server, auf die Medien auf den Client Typs vom Server will. Zum Beispiel:

[!code-console[Main](media-formatters/samples/sample2.cmd)]

Dieser Header weist den Server, dass der Client entweder HTML, XHTML oder XML-möchte.

Der Medientyp bestimmt, wie Sie Web-API zum Serialisieren und Deserialisieren von HTTP-Nachrichtentext. Web-API bietet integrierte Unterstützung für XML, JSON, BSON und Form-Urlencoded-Daten, und Sie können zusätzliche Medientypen unterstützen, indem Sie das Schreiben einer *medienformatierer*.

Um einen medienformatierer zu erstellen, leiten Sie sich von einer dieser Klassen:

- [MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx). In dieser Klasse verwendet den asynchronen Lesevorgang und Write-Methoden.
- [BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx). Diese Klasse wird von **MediaTypeFormatter** aber werden Sychronous Lese-/Schreibzugriff Methoden verwendet.

Ableiten von **BufferedMediaTypeFormatter** ist einfacher, da es keinen asynchroner Code gibt, aber es bedeutet auch, dass der aufrufende Thread kann während der e/a blockiert.

## <a name="example-creating-a-csv-media-formatter"></a>Beispiel: Erstellen einer CSV-Medienformatierer

Das folgende Beispiel zeigt einen medientypformatierer, die ein Product-Objekt in ein Format mit kommagetrennten Werten (CSV) serialisieren kann. In diesem Beispiel verwendet den Produkt-Typ, der im Tutorial definiert [erstellen eine Web-API, CRUD-Vorgänge unterstützt](../older-versions/creating-a-web-api-that-supports-crud-operations.md). Hier ist die Definition der Product-Objekt:

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

Um ein CSV-Formatierungsprogramm zu implementieren, definieren Sie eine abgeleitete Klasse **BufferedMediaTypeFormater**:

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

Fügen Sie im Konstruktor die Medientypen, die den Formatierer unterstützt. In diesem Beispiel der Formatierer einen einziger Medientyp unterstützt &quot;Text/Csv&quot;:

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

Überschreiben der **CanWriteType** Methode, um anzugeben, das das Formatierungsprogramm Typen serialisieren kann:

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

In diesem Beispiel kann das Formatierungsprogramm einzelne Serialisieren `Product` -Objekten sowie Sammlungen von `Product` Objekte.

Auf ähnliche Weise überschreiben die **CanReadType** Methode, um anzugeben, welchen den Formatierer Typs deserialisieren kann. In diesem Beispiel das Formatierungsprogramm unterstützt keine Deserialisierung ab, damit die Methode gibt einfach zurück **"false"**.

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

Überschreiben Sie abschließend die **WriteToStream** Methode. Diese Methode serialisiert einen Typ durch Schreiben in einen Stream. Wenn Ihr Formatierer Deserialisierung unterstützt, auch außer Kraft setzen der **ReadFromStream** Methode.

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a>Die Web-API-Pipeline hinzugefügt eine Medienformatierer

Verwenden Sie zum Hinzufügen einer medientypformatierer an die Web-API-Pipeline die **Formatierer** Eigenschaft für die **HttpConfiguration** Objekt.

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a>Zeichencodierungen

Optional kann eine medienformatierer mehrere zeichencodierungen, z. B. UTF-8 oder ISO 8859-1 unterstützen.

Fügen Sie im Konstruktor, eine oder mehrere [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) Typen, für die **SupportedEncodings** Auflistung. Fügen Sie die erste für die standardcodierung.

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

In der **WriteToStream** und **ReadFromStream** -Methoden aufrufen, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) die bevorzugte zeichenverschlüsselung auswählen. Diese Methode entspricht der Header der Anforderung anhand der Liste der unterstützten Codierungen. Verwenden Sie das zurückgegebene **Codierung** beim Schreiben oder Lesen aus dem Stream:

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
