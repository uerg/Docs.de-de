---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Senden von HTML-Formulardaten in ASP.NET Web-API: Dateiupload und mehrteiligen MIME-Nachrichten | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/21/2012
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 875f9ac62901dfbafc8224af2982c1daf3afc9c5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829536"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Senden von HTML-Formulardaten in ASP.NET Web-API: Dateiupload und mehrteiligen MIME-Nachrichten
====================
durch [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Teil 2: Dateiupload und mehrteiligen MIME-

In diesem Tutorial wird das Hochladen von Dateien in einer Web-API veranschaulicht. Es wird beschrieben, wie zum Verarbeiten von mehrteiliger MIME-Daten.

> [!NOTE]
> [Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Hier ist ein Beispiel für ein HTML-Formular zum Hochladen einer Datei ein:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Dieses Formular enthält ein Texteingabe-Steuerelement und ein Datei-Steuerelement. Wenn ein Formular ein Steuerelement, Datei enthält die **Enctype** Attribut muss immer &quot;Multipart/Form-Data&quot;, der angibt, dass das Formular als eine mehrteilige MIME-Nachricht gesendet wird.

Das Format einer mehrteiligen MIME-Nachricht ist am einfachsten, zu verstehen, betrachten ein Beispiel für eine Anforderung:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Diese Meldung wird in zwei unterteilt *Teile*, eine für die einzelnen Steuerelemente. Teil-Grenzen werden durch die Zeilen angezeigt, die mit Bindestrichen beginnen.

> [!NOTE]
> Die Grenze Teil umfasst eine zufällige-Komponente (&quot;41184676334&quot;) um sicherzustellen, dass die Trennungszeichenfolge nicht versehentlich in einem Teil der Nachricht angezeigt wird.


Jeder Teil der Nachricht enthält eine oder mehrere Kopfzeilen, gefolgt von den Inhalts des Nachrichtenteils.

- Der Content-Disposition-Header enthält den Namen des Steuerelements. Für Dateien enthält sie auch den Dateinamen ein.
- Der Content-Type-Header werden die Daten im Webpart beschrieben. Wenn dieser Header fehlt, ist die Standardeinstellung Text/unformatiert.

Im vorherigen Beispiel hochgeladen der Benutzer eine Datei namens GrandCanyon.jpg, mit dem Content-Type-Image/Jpeg; und den Wert der Texteingabe &quot;Sommer Urlaub&quot;.

## <a name="file-upload"></a>Dateiupload

Jetzt sehen wir uns einen Web-API-Controller, die Dateien aus einer mehrteiligen MIME-Nachricht liest. Der Controller wird für die Dateien asynchron gelesen werden. Web-API unterstützt asynchrone Aktionen, die mit der [aufgabenbasierten Programmiermodell](https://msdn.microsoft.com/library/dd460693.aspx). Zunächst der Code hier ist, wenn Sie .NET Framework 4.5 verwenden möchten, die unterstützt die **Async** und **"await"** Schlüsselwörter.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Beachten Sie, dass die Controlleraktion keine Parameter annimmt. Das ist da wir den Text der Anforderung innerhalb der Aktion verarbeiten, ohne einen medientypformatierer aufzurufen.

Die **IsMultipartContent** Methode überprüft, ob die Anforderung eine mehrteilige MIME-Nachricht enthält. Wenn dies nicht der Fall ist, wird der Controller Gibt HTTP-Statuscode 415 (nicht unterstützter Medientyp).

Die **MultipartFormDataStreamProvider** Klasse ist ein Hilfsobjekt, der Datenströme für hochgeladene Dateien zuordnet. Rufen Sie zum Lesen von mehrteiligen MIME-Nachricht, die **ReadAsMultipartAsync** Methode. Diese Methode alle Teile der Nachricht und schreibt sie in der von bereitgestellten Streams der **MultipartFormDataStreamProvider**.

Wenn die Methode abgeschlossen ist, erhalten Sie Informationen zu den Dateien aus dem **FileData** -Eigenschaft, die eine Auflistung von **MultipartFileData** Objekte.

- **MultipartFileData.FileName** ist der Name der lokalen Datei auf dem Server, in dem die Datei gespeichert wurde.
- **MultipartFileData.Headers** enthält den Teil-Header (*nicht* Header der Anforderung). Hiermit können Sie den Zugriff auf die Inhalte\_Disposition und Content-Type-Header.

Wie der Name schon sagt, **ReadAsMultipartAsync** ist eine asynchrone Methode. Verwenden Sie zum Ausführen von Aufgaben nach Abschluss der Methode eine [Fortsetzungsaufgabe](https://msdn.microsoft.com/library/ee372288.aspx) ((.NET 4.0) oder die **"await"** Schlüsselwort ((.NET 4.5).

Hier ist der .NET Framework 4.0-Version des obigen Codes:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Lesen von Form-Steuerelementdaten

Das HTML-Formular, das weiter oben gezeigt wurde, hatte ein Texteingabe-Steuerelement.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Sie erhalten den Wert des Steuerelements, aus der **FormData** Eigenschaft der **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** ist eine **NameValueCollection** , Name/Wert-Paare für Steuerelemente des Formulars enthält. Die Auflistung kann doppelte Schlüssel enthalten. Betrachten Sie dieses Formular aus:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

Text der Anforderung kann wie folgt aussehen:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

In diesem Fall die **FormData** Auflistung enthält die folgenden Schlüssel/Wert-Paare:

- Reise: Round-Trip
- Optionen: direkten Weg
- Optionen: Datumsangaben
- Arbeitsplatz: Fenster
