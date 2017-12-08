---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'HTML-Formulardaten in ASP.NET Web-API senden: Datei hochladen und Multipart / MIME | Microsoft Docs'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3df59aab2a0c43f4a4f5c59530b0655f68d95cc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Senden von HTML-Formulardaten in ASP.NET Web-API: hochladen und Multipart / MIME-Datei
====================
durch [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Teil 2: Datei hochladen und Multipart / MIME-

Dieses Lernprogramm zeigt, wie zum Hochladen von Dateien in eine Web-API. Es beschreibt auch, wie zum Verarbeiten von mehrteiliger MIME-Daten.

> [!NOTE]
> [Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Hier ist ein Beispiel für ein HTML-Formular für das Hochladen einer Datei ein:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Dieses Formular enthält, eines Texteingabe-Steuerelements und Eingabesteuerelement einer Datei. Wenn ein Formular Eingabesteuerelement einer Datei enthält die **Enctype** Attribut muss immer werden &quot;Multipart/Form-Data&quot;, der angibt, dass das Formular als einer mehrteiligen MIME-Nachricht gesendet werden.

Das Format einer mehrteiligen MIME-Nachricht ist am einfachsten, zu verstehen, betrachten ein Beispiel für eine Anforderung:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Diese Meldung wird in zwei unterteilt *Teile*, eine für jede Form-Steuerelement. Teil-Grenzen werden durch die Zeilen angezeigt, die mithilfe von Gedankenstrichen beginnen.

> [!NOTE]
> Die Grenze Teil umfasst eine zufällige-Komponente (&quot;41184676334&quot;), stellen Sie sicher, dass die Trennungszeichenfolge nicht versehentlich in einem Nachrichtenteil angezeigt wird.


Jeder Teil der Nachricht enthält eine oder mehrere Header, gefolgt von den Inhalts des Nachrichtenteils.

- Der Content-Disposition-Header enthält den Namen des Steuerelements. Für Dateien enthält er auch den Dateinamen ein.
- Der Content-Type-Header beschreibt die Daten in den Bereich. Wenn dieser Header fehlt, ist die Standardeinstellung Text/Plain an.

Im vorherigen Beispiel hochgeladen der Benutzer eine Datei namens GrandCanyon.jpg, mit dem Inhaltstyp Image/Jpeg; und der Wert der Texteingabe war &quot;Summer Urlaub&quot;.

## <a name="file-upload"></a>Datei hochladen

Jetzt sehen wir uns einen Web-API-Controller, die Dateien aus einer mehrteiligen MIME-Nachricht liest. Der Controller wird für die Dateien asynchron gelesen werden. Web-API unterstützt die asynchrone Aktionen, die mit der [aufgabenbasierten Programmiermodell](https://msdn.microsoft.com/library/dd460693.aspx). Zunächst hier stammt der Code bei der Sie Ausrichtung auf .NET Framework 4.5, das unterstützt die **Async** und **"await"** Schlüsselwörter.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Beachten Sie, dass der Controlleraktion keine Parameter akzeptiert. Da wir den Hauptteil der Anforderung innerhalb der Aktion verarbeiten, ohne einen medientypformatierer aufzurufen ist.

Die **IsMultipartContent** Methode überprüft, ob die Anforderung eine mehrteilige MIME-Nachricht enthält. Wenn dies nicht der Fall ist, wird der Controller zurückgegeben, Statuscode "HTTP" 415 (nicht unterstützter Medientyp).

Die **MultipartFormDataStreamProvider** Klasse ist Hilfsobjekt, das Dateistreams für hochgeladene Dateien belegt wird. Um den mehrteiligen MIME-Nachricht zu lesen, rufen Sie die **ReadAsMultipartAsync** Methode. Diese Methode alle Teile der Nachricht extrahiert und schreibt sie in die Datenströme gebotenen der **MultipartFormDataStreamProvider**.

Wenn die Methode abgeschlossen ist, erhalten Sie Informationen zu den Dateien aus dem **FileData** -Eigenschaft, die eine Auflistung von **MultipartFileData** Objekte.

- **MultipartFileData.FileName** ist der Name der lokalen Datei auf dem Server, in dem die Datei gespeichert wurde.
- **MultipartFileData.Headers** enthält der Header Teil (*nicht* Anforderungsheader). Hiermit können Sie den Zugriff auf die Inhalte\_Disposition "und" Content-Type-Header.

Wie der Name bereits vermuten lässt, **ReadAsMultipartAsync** ist eine asynchrone Methode. Verwenden Sie Aufgaben nach Abschluss der Methode, eine [Fortsetzungsaufgabe](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) oder die **"await"** Schlüsselwort (.NET 4.5).

Hier ist die .NET Framework 4.0-Version des vorherigen Codes ein:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lesen von Formulardaten-Steuerelement

Das HTML-Formular, das zuvor gezeigten mussten ein Texteingabe-Steuerelements.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Sie erhalten den Wert des Steuerelements aus der **FormData** Eigenschaft von der **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** ist ein **NameValueCollection** enthält Name/Wert-Paare für die Steuerelemente des Formulars. Die Auflistung kann doppelte Schlüssel enthalten. Betrachten Sie dieses Formular ein:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Der Anforderungstext kann wie folgt aussehen:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In diesem Fall die **FormData** Auflistung würde die folgenden Schlüssel/Wert-Paare enthalten:

- Reise: Round-Trip
- Optionen: ohne Unterbrechung bis
- Optionen: Datumsangaben
- Arbeitsplätze: Fenster
