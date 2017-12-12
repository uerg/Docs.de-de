---
uid: whitepapers/request-validation
title: Anforderungsvalidierung - Script-Angriffe verhindert | Microsoft Docs
author: rick-anderson
description: "Dieses Dokument beschreibt die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung daran gehindert wird Verarbeitung nicht codierte HTML-Inhalt senden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 61a96b75fdc29bdd1510ed689ee0356ef30e03fc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="request-validation---preventing-script-attacks"></a>Anforderungsvalidierung - Script-Angriffe zu verhindern
====================
> Dieses Whitepaper beschreibt die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung daran gehindert wird Verarbeitung nicht codierte HTML-Inhalt an den Server gesendet. Diese Anforderung Überprüfungsfunktion kann deaktiviert werden, wenn die Anwendung entwickelt wurde, um HTML-Daten sicher zu verarbeiten.
> 
> Gilt für ASP.NET 1.1 und ASP.NET 2.0.


Anforderungsüberprüfung, eine Funktion von ASP.NET seit Version 1.1, wird verhindert, dass den Server akzeptiert Inhalts enthaltendes uncodierten HTML. Dieses Feature wurde entwickelt, um zu verhindern einige Script-Injection-Angriffen, bei dem Client-Skriptcode oder HTML kann werden unbewusst mit einem Server übermittelt, gespeichert und dann an andere Benutzer angezeigt. Es wird weiterhin dringend empfohlen, Sie überprüfen, dass alle Eingabedaten und die HTML-Codierung, ihn gegebenenfalls.

Beispielsweise erstellen Sie eine Webseite, die e-Mail-Adresse eines Benutzers anfordert und speichert dann die e-Mail-Adresse in einer Datenbank. Wenn der Benutzer eingibt, &lt;Skript&gt;Warnung ("Hello aus dem Skript")&lt;/SCRIPT&gt; anstelle einer gültigen e-Mail-Adresse, wenn diese Daten dargestellt werden, dieses Skript kann ausgeführt werden, wenn der Inhalt nicht ordnungsgemäß codiert wurde. Die Anforderung Überprüfungsfunktion von ASP.NET wird dies verhindert.

## <a name="why-this-feature-is-useful"></a>Warum diese Funktion hilfreich ist.

Viele Websites sind nicht beachten Sie, dass sie für einfache Skript-Injection-Angriffe geöffnet sind. Der Zweck dieser Art von Angriffen die Website durch Anzeigen von HTML handelt oder potenziell Clientskripts zum Umleiten des Benutzers ein Hacker Standort ausgeführt ist, sind Script-Injection-Angriffe ein Problem, das Webentwickler bewältigen müssen.

Script-Injection-Angriffe sind Besorgnis alle Webentwickler, unabhängig davon, ob sie ASP.NET, ASP oder andere webtechnologien für die Entwicklung sind.

Das Feature zur Validierung von ASP.NET Anforderung verhindert proaktiv diese Angriffe durch nicht codierte HTML-Inhalte vom Server verarbeitet werden, es sei denn, der Entwickler entschieden hat, um zuzulassen, dass der Inhalt nicht zugelassen.

## <a name="what-to-expect-error-page"></a>Was Sie erwartet: Fehler (Seite)

Der Screenshot zeigt einige Beispiel-ASP-Code:

![](request-validation/_static/image1.png)

Dieser Code Ergebnisse in eine einfache Seite, die Ihnen ermöglicht, geben Sie Text im Textfeld ausgeführt werden, klicken Sie auf die Schaltfläche und zeigt den Text in das Label-Steuerelement:

![](request-validation/_static/image2.png)

Wurden jedoch JavaScript, z. B. `<script>alert("hello!")</script>` eingegeben und erhalten wir eine Ausnahme übermittelt werden:

![](request-validation/_static/image3.png)

Die Fehlermeldung gibt an, dass eine "potenziell gefährlichen Request.Form Wert erkannt wurde." und erhalten Sie weitere Informationen in der Beschreibung zu genau den Vorfall und das Verhalten zu ändern. Zum Beispiel:

Anforderungsüberprüfung hat einen potenziell gefährliche Client Eingabewert erkannt, und die Verarbeitung der Anforderung wurde abgebrochen. Dieser Wert zeigt möglicherweise einen Versuch gefährdet die Sicherheit Ihrer Anwendung, z. B. einen Cross-Site scripting-Angriff an. Sie können die Anforderungsvalidierung deaktivieren, indem `validateRequest=false` in der Seitendirektive oder im Konfigurationsabschnitt. Es wird jedoch dringend empfohlen, dass die Anwendung explizit in diesem Fall alle Eingaben überprüfen.

## <a name="disabling-request-validation-on-a-page"></a>Deaktivieren der Anforderungsvalidierung auf einer Seite

Um Anforderungsvalidierung auf einer Seite zu deaktivieren, müssen Sie festlegen, der `validateRequest` -Attribut der Seitendirektive auf `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Bei der anforderungsüberprüfung deaktiviert ist, kann Inhalte auf eine Seite gesendet werden; Es ist die Verantwortung des Entwicklers Seite um sicherzustellen, dass der Inhalt korrekt codiert oder verarbeitet wird.

## <a name="disabling-request-validation-for-your-application"></a>Deaktivieren der Anforderungsvalidierung für Ihre Anwendung

Um die Anforderungsvalidierung für Ihre Anwendung zu deaktivieren, müssen Sie ändern oder erstellen Sie eine Datei "Web.config" für Ihre Anwendung und legen Sie das Attribut ValidateRequest der `<pages />` Abschnitt `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Wenn Sie die Anforderungsvalidierung für alle Anwendungen auf dem Server deaktivieren möchten, können Sie diese Änderung der Datei "Machine.config" vornehmen.

> [!CAUTION]
> Bei der anforderungsüberprüfung deaktiviert ist, kann Inhalte für Ihre Anwendung gesendet werden; Es ist die Verantwortung des Anwendungsentwicklers, um sicherzustellen, dass der Inhalt korrekt codiert oder verarbeitet wird.

Im folgenden Code wird geändert, um die Anforderungsvalidierung zu deaktivieren:

![](request-validation/_static/image4.png)

Wenn Sie der folgenden JavaScript-Code in das Textfeld eingegeben wurde nun `<script>alert("hello!")</script>` wäre das Ergebnis:

![](request-validation/_static/image5.png)

Um dies zu vermeiden, mit der Anforderungsvalidierung deaktiviert zu verhindern, müssen wir HTML-Codierung Inhalt.

## <a name="how-to-html-encode-content"></a>Wie in HTML codieren Inhalt

Wenn Sie die Anforderungsvalidierung deaktiviert haben, ist es empfiehlt sich, HTML-codieren Inhalte, die für die zukünftige Verwendung gespeichert werden soll. HTML-Codierung wird automatisch ersetzen "&lt;'oder'&gt;' (zusammen mit mehreren anderen Symbolen) mit ihren entsprechenden HTML-codierte Darstellung. Z. B. "&lt;"wird ersetzt durch"&amp;Lt;" und "&gt;"wird ersetzt durch"&amp;Gt;". Browser verwenden diese speziellen Codes zum Anzeigen der "&lt;'oder'&gt;" im Browser.

Inhalt kann problemlos HTML-codiert sein, auf dem Server mithilfe der `Server.HtmlEncode(string)` API. Inhalt kann auch sein problemlos HTML-decodiert, d. h. auf standard HTML mit zurückgesetzt die `Server.HtmlDecode(string)` Methode.

![](request-validation/_static/image6.png)

Hatte:

![](request-validation/_static/image7.png)
