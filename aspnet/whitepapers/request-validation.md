---
uid: whitepapers/request-validation
title: Anforderungsvalidierung – Schutz vor Angriffen durch Skripteinschleusung | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Artikel wird beschrieben, die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung verhindert wird von der Verarbeitung nicht codierte HTML-Inhalt senden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 783fb1ae27d88f9c6d6d3484d26d3e206e7f2fba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372928"
---
<a name="request-validation---preventing-script-attacks"></a>Anforderungsvalidierung – Schutz vor Angriffen durch Skripteinschleusung
====================
> Dieser Artikel beschreibt die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung verhindert wird von der Verarbeitung nicht codierte HTML-Inhalt an den Server gesendet. Diese Anforderung Überprüfung-Funktion kann deaktiviert werden, wenn die Anwendung entwickelt wurde, um HTML-Daten sicher zu verarbeiten.
> 
> Gilt für ASP.NET 1.1 und ASP.NET 2.0.


Anforderungsvalidierung, ein Feature von ASP.NET, seit Version 1.1, wird verhindert, dass der Server Inhalte mit nicht codierten HTML akzeptiert. Dieses Feature wurde entwickelt, um zu verhindern einige Script-Injection-Angriffen, bei dem Clientskriptcode oder HTML unbewusst an einen Server übermittelt, gespeichert und werden kann, klicken Sie dann anderen Benutzern angezeigt. Es wird weiterhin dringend empfohlen, dass Sie überprüfen, dass alle Eingabedaten und HTML codieren, die bei Bedarf.

Beispielsweise erstellen Sie eine Webseite, die anfordert, die e-Mail-Adresse eines Benutzers und dann speichert, die e-Mail-Adresse in einer Datenbank. Wenn der Benutzer eingibt &lt;Skript&gt;Warnung ("Hello from Script")&lt;/SCRIPT&gt; anstelle einer gültigen e-Mail-Adresse, wenn die Daten angezeigt werden, dieses Skript kann ausgeführt werden, wenn der Inhalt nicht ordnungsgemäß codiert wurde. Die Anforderung Überprüfungsfunktion von ASP.NET verhindert dies.

## <a name="why-this-feature-is-useful"></a>Warum ist diese Funktion nützlich

Viele Websites sind nicht beachten Sie, dass sie für einfache Skript-Injection-Angriffe geöffnet sind. Ob der Zweck dieser Art von der Website mit HTML handelt oder potenziell Clientskript zum Umleiten des Benutzers auf ein Hacker die Website ausgeführt ist, werden von Angriffen durch skripteinschleusung ein Problem, das Webentwickler mit befassen müssen.

Von Angriffen durch skripteinschleusung sind alle Webentwickler relevant, ob der ASP.NET, ASP oder anderen webtechnologien für die Entwicklung verwendet werden.

Die ASP.NET-Validierungsfunktion für die Anforderung wird verhindert, dass proaktiv diese Angriffe durch nicht codierte HTML-Inhalt vom Server verarbeitet werden, es sei denn, der Entwickler entscheidet, um zuzulassen, dass der Inhalt nicht zugelassen.

## <a name="what-to-expect-error-page"></a>Was Sie erwarten: Fehler (Seite)

Der nachstehende Screenshot zeigt einige Beispiel-ASP-Code:

![](request-validation/_static/image1.png)

Dieses Codes führt in eine einfache Seite, die Ihnen ermöglicht, geben Sie Text im Textfeld für die ausführen, klicken Sie auf die Schaltfläche, und zeigen Sie den Text in das Label-Steuerelement an:

![](request-validation/_static/image2.png)

Allerdings waren Sie z. B. JavaScript, `<script>alert("hello!")</script>` eingegeben und übermittelt, erhalten wir eine Ausnahme:

![](request-validation/_static/image3.png)

Die Fehlermeldung gibt an, dass eine "potenziell gefährliche Request.Form Value was detected" und enthält weitere Details in der Beschreibung, genau was aufgetreten ist und wie Sie das Verhalten zu ändern. Zum Beispiel:

Request-Überprüfung hat einen Eingabewert potenziell gefährliche Client erkannt und Verarbeitung der Anforderung wurde abgebrochen. Dieser Wert möglicherweise auf die Sicherheit Ihrer Anwendung, z. B. einen Cross-Site-scripting-Angriff beeinträchtigt. Sie können die Request-Überprüfung deaktivieren, durch Festlegen von `validateRequest=false` in der Page-Direktive oder im Konfigurationsabschnitt. Es wird jedoch dringend empfohlen, dass Ihre Anwendung explizit alle Eingaben in diesem Fall zu überprüfen.

## <a name="disabling-request-validation-on-a-page"></a>Deaktivieren der Anforderungsvalidierung auf einer Seite

Anforderungsvalidierung auf einer Seite zu deaktivieren, Sie festlegen müssen, die `validateRequest` Attribut der Page-Direktive auf `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Wenn Request-Überprüfung deaktiviert ist, kann Inhalte auf eine Seite gesendet werden; Es ist die Verantwortung für den Entwickler der Seite, um sicherzustellen, dass der Inhalt ordnungsgemäß codiert oder verarbeitet wird.

## <a name="disabling-request-validation-for-your-application"></a>Request-Überprüfung für Ihre Anwendung deaktivieren

Um die Anforderungsvalidierung für Ihre Anwendung zu deaktivieren, müssen Sie ändern oder erstellen Sie eine Web.config-Datei für Ihre Anwendung und das ValidateRequest-Attribut des der `<pages />` Abschnitt `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Wenn Sie die Anforderungsvalidierung für alle Anwendungen auf dem Server deaktivieren möchten, können Sie diese Änderung der Datei "Machine.config" vornehmen.

> [!CAUTION]
> Wenn die Request-Überprüfung deaktiviert ist, kann Inhalte für Ihre Anwendung gesendet werden; Es ist die Verantwortung für den Anwendungsentwickler, um sicherzustellen, dass der Inhalt ordnungsgemäß codiert oder verarbeitet wird.

Der folgende Code wird geändert, um die Anforderungsvalidierung zu deaktivieren:

![](request-validation/_static/image4.png)

Wenn Sie der folgende JavaScript-Code in das Textfeld eingegeben wurde jetzt `<script>alert("hello!")</script>` das Ergebnis wäre:

![](request-validation/_static/image5.png)

Damit dies nicht geschieht, mit der Request-Überprüfung deaktiviert, müssen wir HTML-Codierung Inhalt.

## <a name="how-to-html-encode-content"></a>Wie in HTML codieren Inhalt

Wenn Sie den Request-Überprüfung deaktiviert haben, ist es empfiehlt sich, HTML-Codierung von Inhalten, die für die zukünftige Verwendung gespeichert werden. HTML-Codierung wird automatisch ersetzen "&lt;'oder'&gt;" (zusammen mit mehreren anderen Symbolen) mit ihren entsprechenden HTML-codierte Darstellung. Z. B. "&lt;"wird ersetzt durch"&amp;Lt;' und '&gt;"wird ersetzt durch"&amp;Gt;". Browser verwenden diese speziellen Codes zum Anzeigen der "&lt;'oder'&gt;" im Browser.

Inhalt kann ganz einfach HTML-codiert sein, auf dem Server mithilfe der `Server.HtmlEncode(string)` API. Inhalt kann auch sein, einfach HTML-decodiert, d. h. auf standard HTML mit zurückgesetzt der `Server.HtmlDecode(string)` Methode.

![](request-validation/_static/image6.png)

Was zu:

![](request-validation/_static/image7.png)
