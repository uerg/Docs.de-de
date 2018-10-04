---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: XSRF/CSRF-Schutz in ASP.NET MVC und Webseiten | Microsoft-Dokumentation
author: Rick-Anderson
description: Websiteübergreifende anforderungsfälschung (auch bekannt als XSRF oder CSRF) ist ein Angriff gegen Web gehostete Anwendungen, die durch eine schädliche Website die Interakti beeinflussen können...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 5db661cccc58d1101f95091b069ab5cbfe78a378
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577937"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>XSRF/CSRF-Schutz in ASP.NET MVC und Webseiten
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Cross-Site-Anforderung, dass anforderungsfälschung (auch bekannt als XSRF oder CSRF) einen Angriff auf das Web gehostete Anwendungen wird durch eine schädliche Website die Interaktion zwischen einem Clientbrowser und einer Website, die von diesem Browser als vertrauenswürdig eingestuft beeinflussen können. Diese Angriffe sind möglich, da Webbrowser Authentifizierungstoken automatisch mit jeder Anforderung an eine Website gesendet werden. Das kanonische Beispiel ist ein Authentifizierungscookie, z. B. ASP. NET Forms-Authentifizierungsticket. Allerdings können Websites, die einen persistenten Authentifizierungsmechanismus (z. B. Windows-Authentifizierung, Basic, usw.) verwenden diese Angriffe verwendet werden.
> 
> Ein XSRF-Angriff unterscheidet sich von einem Phishing-Angriff. Phishing-Angriffe erfordern Beihilfe des Opfers. Bei einem Phishing-Angriff der entsprechenden Website eine schädliche Website imitiert und das Opfer wird dazu verleitet vertraulichen Informationen an den Angreifer. Bei einem XSRF-Angriff besteht häufig keine Interaktion des Opfers erforderlich. Stattdessen der Angreifer im Browser automatisch alle relevanten Cookies an die Ziel-Website senden setzt voraus, dass.
> 
> Weitere Informationen finden Sie unter den [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).


## <a name="anatomy-of-an-attack"></a>Anatomie eines Angriffs

Um ein XSRF-Angriff zu durchlaufen, sollten Sie in ein Benutzer, der einige Transaktionen Onlinebanking ausführen möchte. Dieser Benutzer besucht zuerst WoodgroveBank.com und Protokolle, an diesem, die Punkt der Antwortheader ihr Authentifizierungscookie enthält:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Da das Authentifizierungscookie ein Sitzungscookie ist, wird er automatisch vom Browser zurückgesetzt, wenn der Browserprozess beendet wird. Allerdings wird der Browser bis zu diesem Zeitpunkt automatisch das Cookie mit jeder Anforderung an WoodgroveBank.com enthalten. Der Benutzer möchte nun $1000 an ein anderes Konto übertragen wird, so dass er ein Formular auf die Banking-Site füllt, und der Browser diese Anforderung an dem Server sendet:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Da dieser Vorgang einen Nebeneffekt hat (es wird eine finanzielle Transaktion initiiert), hat die Banking-Site möchten erfordert einen HTTP-POST, um diesen Vorgang zu initiieren. Der Server liest das Authentifizierungstoken aus der Anforderung, sucht der Kontonummer des aktuellen Benutzers, stellt sicher, dass ausreichende Geldmittel vorhanden sein, und klicken Sie dann die Transaktion in das Zielkonto initiiert.

Ihrer Onlinebanking abgeschlossen haben, der Benutzer navigiert, Weg von der Banking-Site und wechselt zu anderen Speicherorten im Web. Eine dieser Websites – "Fabrikam.com" – enthält das folgende Markup auf einer Seite eingebettet in ein &lt;Iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Welche führt dazu, dass den Browser für diese Anforderung:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

Der Angreifer missbraucht die Tatsache, dass der Benutzer möglicherweise weiterhin ein gültiges Authentifizierungstoken für die Ziel-Website, und sie ein kleines Snippet von Javascript verwendet ist, auf den Browser dazu bringt, stellen eine HTTP-POST automatisch an den Zielstandort. Wenn das Authentifizierungstoken noch gültig ist, initiiert die Banking-Site eine Übertragung von 250 USD in das Konto des Angreifers auszuwählen.

### <a name="ineffective-mitigations"></a>Damit Sie nicht versehentlich entschärfungen

Es ist interessant, dass in diesem Szenario die Tatsache, dass WoodgroveBank.com über SSL zugegriffen wurde, und es ein Cookie nur SSL-Authentifizierung musste nicht ausreichend, um den Angriff abzuwehren war. Der Angreifer kann an die [URI-Schema](http://en.wikipedia.org/wiki/URI_scheme) (Https) in ihrem &lt;Formular&gt; Element, und der Browser weiterhin nicht abgelaufene Cookies an den Zielstandort senden, solange diese Cookies mit dem URI konsistent sind Schema, der das gewünschte Ziel.

Man könnte argumentieren, dass der Benutzer nicht besuchen Sie einfach sollte nicht vertrauenswürdige Sites, als der Zugriff auf nur vertrauenswürdige Sites können online sicher bleiben. Es gibt einige Weg, dies allerdings leider diese Empfehlung ist nicht immer praktikabel. Vielleicht vertraut"der Benutzer" die Website regionalnachrichten ConsolidatedMessenger. ConsolidatedMessenger.com und wechselt zu finden, die stattdessen Website, aber dieser Standort verfügt über einer XSS-Schwachstelle Dadurch kann einen Angreifer, gleichen den Codeausschnitt einzufügen, die auf "Fabrikam.com" ausgeführt wurde.

Sie können sicherstellen, dass eingehende Anforderungen eine [referenzheader](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) verweisen auf Ihre Domäne. Dies beendet die Anforderungen, die von einer Drittanbieter-Domäne unwissentlich übermittelt. Allerdings einige Leute Deaktivieren des Browsers Referer-Header aus Datenschutzgründen und Angreifer können manchmal diesen Header vortäuschen, wenn das Opfer bestimmte unsichere-Software installiert ist. Überprüfen der [referenzheader](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) einen sichereren Ansatz zum Schutz vor XSRF-Angriffen wird nicht berücksichtigt.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web Stack Runtime XSRF-entschärfungen

Der ASP.NET Web Stack Common Language Runtime verwendet eine Variante der [für die domänensynchronisierung tokenmuster](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern) zur Verteidigung gegen XSRF-Angriffe. Die allgemeine Form des Tokens für die domänensynchronisierung-Musters ist, dass zwei Anti-XSRF-Token mit jeder HTTP-POST (zusätzlich zu dem Authentifizierungstoken) an den Server übermittelt werden: eine als ein Cookie, und der andere wird als ein Formularwert-Token. Die Tokenwerte generiert, die von der ASP.NET-Laufzeit sind nicht deterministisch oder von einem Angreifer vorhersagbar. Wenn die Token gesendet werden, lässt der Server die Anforderung fortgesetzt werden, nur dann, wenn beide Token ein Vergleich bestehen.

Die Anforderung XSRF-Überprüfung *Sitzungstoken* wird als ein HTTP-Cookie gespeichert und enthält derzeit die folgende Informationen in der Nutzlast:

- Ein Sicherheitstoken, bestehend aus eines zufälligen 128-Bit-Bezeichners.   
 Die folgende Abbildung zeigt das XSRF-Anforderung Überprüfung Sitzungstoken, das mit den Internet Explorer F12-Entwicklertools angezeigt: (Beachten Sie dies ist die aktuelle Implementierung und Betreff "," sogar wahrscheinlich, um zu ändern.)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

Die *Token Pole* Speicherung als eine `<input type="hidden" />` und enthält die folgende Informationen in der Nutzlast:

- Benutzername des angemeldeten Benutzers, (wenn authentifiziert).
- Zusätzlichen Daten zur Verfügung gestellt, indem ein [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Die Nutzlast der Anti-XSRF-Token werden verschlüsselt und signiert, sodass Sie den Benutzernamen nicht anzeigen können, wenn Tools verwenden, um die Token zu überprüfen. Wenn die Webanwendung ASP.NET 4.0 als Ziel verwendet, finden Sie kryptografische Dienste von der [MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) Routine. Wenn die Webanwendung ASP.NET 4.5 ausgerichtet ist, oder höher, kryptografische Dienste werden bereitgestellt, durch die [MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) -Routine, die eine bessere Leistung, Erweiterbarkeit und Sicherheit bietet. Feststellen Sie, dass die folgenden Blogbeiträge für Weitere Informationen:

- [Kryptografischen Verbesserungen in ASP.NET 4.5 "," pt ". 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Kryptografischen Verbesserungen in ASP.NET 4.5 "," pt ". 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Kryptografischen Verbesserungen in ASP.NET 4.5 "," pt ". 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generieren von Token

Um die Anti-XSRF-Token zu generieren, rufen die [ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) Methode aus einer MVC-Ansicht oder @AntiForgery.GetHtml() aus einer Razor Pages. Die Common Language Runtime führt dann die folgenden Schritte aus:

1. Wenn die aktuelle HTTP-Anforderung bereits ein Anti-XSRF-Sitzungstoken enthält (Anti-XSRF-Cookies \_ \_RequestVerificationToken), das Sicherheitstoken wird daraus extrahiert. Wenn die HTTP-Anforderung ein Anti-XSRF-Sitzungstoken nicht enthält oder wenn der Fehler bei der Extraktion des Sicherheitstokens, ein neues zufälliges Anti-XSRF-Token generiert werden.
2. Ein Feld Anti-XSRF-Token wird mithilfe des Sicherheitstokens aus den vorherigen Schritt (1) und die Identität des aktuellen Benutzers angemeldet generiert. (Weitere Informationen zum Ermitteln der Identität des Benutzers finden Sie unter den **[Szenarien mit besonderer Unterstützung](#_Scenarios_with_special)** Abschnitt weiter unten.) Darüber hinaus Wenn ein [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) ist konfiguriert, die Laufzeit ruft die [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) Methode und die zurückgegebene Zeichenfolge im Feldtoken enthalten. (Finden Sie unter den **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** Abschnitt, um weitere Informationen.)
3. Wenn in Schritt (1) ein neues Anti-XSRF-Token generiert wurde, wird ein neues Sitzungstoken dafür erstellt werden und wird der ausgehenden HTTP-Cookies-Auflistung hinzugefügt werden. Wird das Feldtoken aus Schritt (2) umschlossen werden ein `<input type="hidden" />` Element, und diese HTML-Markup wird der Rückgabewert von `Html.AntiForgeryToken()` oder `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Überprüfen von Token

Um die eingehenden Anti-XSRF-Token zu überprüfen, die Entwickler umfasst eine [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) Attribut für ihre MVC-Aktion oder des Controllers oder She Aufrufe `@AntiForgery.Validate()` aus ihrem Razor Pages. Die Laufzeit führt die folgenden Schritte aus:

1. Die eingehenden Sitzungstokens und Feldtoken werden gelesen, und die Anti-XSRF-Token extrahiert aus jedem. Die Anti-XSRF-Token müssen pro Schritt (2) in der Routine Generation identisch sein.
2. Wenn der aktuelle Benutzer authentifiziert ist, wird der Benutzernamen mit dem Benutzernamen in das Feldtoken gespeicherten verglichen. Benutzernamen müssen übereinstimmen.
3. Wenn ein [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) konfiguriert ist, ruft die Laufzeit die *ValidateAdditionalData* Methode. Die Methode muss den booleschen Wert zurückgeben *"true"*.

Wenn die Überprüfung erfolgreich ist, ist die Anforderung zulässig, um den Vorgang fortzusetzen. Wenn die Validierung fehlschlägt, löst das Framework eine *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Fehlerbedingungen

Ab der ASP.NET Web Stack Runtime v2, alle *HttpAntiForgeryException* ausgelöst, während der Validierung enthält ausführliche Informationen zum Problem geöffnet. Die aktuell definierten fehlerbedingungen sind:

- Das Sitzungstoken, das oder formulartoken ist nicht in der Anforderung vorhanden.
- Das Sitzungstoken, das oder die formulartoken kann nicht gelesen werden. Die wahrscheinlichste Ursache dieser ist eine nicht übereinstimmende Versionen von der ASP.NET Web Stack Runtime oder in einer Farm-Farm, in denen die &lt;MachineKey&gt; Element in "Web.config" unterscheidet sich zwischen Computern. Sie können ein Tool wie Fiddler verwenden, um diese Ausnahme zu erzwingen, indem Sie die Manipulation von beiden Anti-XSRF-Token.
- Das Sitzungstoken und Feldtoken wurden ausgetauscht.
- Das Sitzungstoken und Feldtoken enthalten, nicht übereinstimmende Sicherheitstoken.
- Der Benutzername, der in das Feldtoken eingebettet entspricht nicht den angemeldeten Benutzernamen des aktuellen Benutzers.
- Die *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* zurückgegebene Methode *"false"*.

Die Anti-XSRF-Funktionen können auch führen zusätzliche Überprüfungen während der Generierung von Tokens oder die Überprüfung durch, und Fehler während der diese Überprüfungen möglicherweise ausgelösten Ausnahmen. Finden Sie unter den [WIF / ACS / Claims-basierte Authentifizierung](#_WIF_ACS) und **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** Abschnitten Weitere Informationen.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Szenarien mit besonderer Unterstützung

### <a name="anonymous-authentication"></a>Anonyme Authentifizierung

Die Anti-XSRF-enthält spezielle Unterstützung für anonyme Benutzer, wobei "Anonym" als Benutzer definiert ist, in denen die *IIdentity.IsAuthenticated* -Eigenschaft gibt *"false"*. Szenarien gehört die Bereitstellung von XSRF-Schutz auf die Anmeldeseite (bevor der Benutzer authentifiziert ist) und benutzerdefinierte Authentifizierungsschemas, bei denen die Anwendung einen Mechanismus außer verwendet *IIdentity* zur Identifizierung von Benutzern.

Um diese Szenarien zu unterstützen, denken Sie daran, dass die Sitzung und Feld-Token ein Sicherheitstoken, verknüpft sind, die eine 128-Bit-zufällig generierten opaque-Bezeichner ist. Dieses Sicherheitstoken wird verwendet, um eine einzelne benutzersitzung nachzuverfolgen, wie sie die Website navigiert, damit sie effektiv den Zweck der einen anonymen Bezeichner dient. Eine leere Zeichenfolge wird anstelle der Benutzername für die Generierung und Überprüfung Routinen, die oben beschriebenen verwendet.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / Claims-basierte Authentifizierung

In der Regel die *IIdentity* Klassen, die in .NET Framework integriert haben, die Eigenschaft, die *IIdentity.Name* ist ausreichend, um einen bestimmten Benutzer innerhalb einer bestimmten Anwendung eindeutig zu identifizieren. Z. B. *FormsIdentity.Name* gibt den Benutzernamen, die in der Mitgliedschaftsdatenbank (die für alle Anwendungen, die abhängig von der Datenbank eindeutig ist), gespeicherte *WindowsIdentity.Name* gibt die domänenqualifizierte Identität des Benutzers, und So weiter. Diese Systeme bieten nicht nur die Authentifizierung; Diese auch *identifizieren* Benutzern für eine Anwendung.

Claims-basierte Authentifizierung ist auf der anderen Seite nicht unbedingt erforderlich, die einen bestimmten Benutzer identifiziert. Stattdessen die *"ClaimsPrincipal"* und *"ClaimsIdentity"* Typen sind verknüpft mit einem Satz von *Anspruch* -Instanzen, in denen die einzelnen Ansprüche möglicherweise "über 18 Jahre alt ist" oder " ist ein Administrator"zu einem anderen. Da der Benutzer noch nicht unbedingt identifiziert wurde, kann die Runtime nicht verwenden die *ClaimsIdentity.Name* Eigenschaft als eindeutiger Bezeichner für diesen Benutzer. Das Team kann angezeigt werden, praktische Beispiele, in denen *ClaimsIdentity.Name* gibt *null*, gibt einen Anzeigenamen (anzeigen) oder eine Zeichenfolge, die nicht geeignet für die Verwendung als eindeutiger Bezeichner ist, andernfalls für den Benutzer.

Viele der Bereitstellungen, die anspruchsbasierte Authentifizierung verwenden [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) insbesondere. ACS ermöglicht es dem Entwickler so konfigurieren Sie einzelne *Identitätsanbieter* (z. B. ADFS, der Microsoft-Account-Anbieter, OpenID-Anbietern wie Yahoo! usw.), und geben Sie die Identitätsanbieter zurück *Bezeichnerbenennen*. Diese Name-Bezeichner können Daten mit persönlich identifizierbaren Informationen (PII) enthalten, z. B. eine e-Mail-Adresse, oder sie können z. B. eine Private persönliche Bezeichner (PPID) anonymisiert werden. Unabhängig davon, das Tupel (Identitätsanbieter, den Namensbezeichner) ausreichend dient als entsprechenden Token für einen bestimmten Benutzer, während sie den Standort, damit die ASP.NET Web-Stack-Laufzeit das Tupel anstelle des Benutzernamens, beim Generieren von verwenden können durchsucht und Überprüfen von Anti-XSRF-Feld-Token. Die bestimmte URIs für den betreffenden Identitätsanbieter anzeigt und die Name-ID sind:

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(finden Sie in diesem [ACS Doc Seite](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) für Weitere Informationen.)

Beim generieren, oder Überprüfen eines Tokens, die ASP.NET Web Stack Laufzeit versucht zur Laufzeit Bindung an Typen:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (Für die WIF-SDK).
- `System.Security.Claims.ClaimsIdentity` (Für .NET 4.5).

Wenn diese Typen sind vorhanden und des aktuellen Benutzers *IIIIdentity* implementiert oder Unterklassen eines der folgenden Typen ist, der Anti-XSRF-Funktion (Identitätsanbieter, den Namensbezeichner) Tupel anstelle der Benutzername beim Generieren von und Überprüfen die Token an. Wenn kein solcher Tupel vorhanden ist, schlägt die Anforderung mit der Fehlermeldung beschreibt das für den Entwickler zum Konfigurieren des Anti-XSRF-Systems, um zu verstehen, den bestimmte anspruchsbasierten Authentifizierungsmechanismus verwendet fehl. Finden Sie unter den **[Konfiguration und Erweiterbarkeit](#_Configuration_and_extensibility)** Abschnitt, um weitere Informationen.

### <a name="oauth--openid-authentication"></a>OAuth / OpenID-Authentifizierung

Schließlich verfügt die Anti-XSRF-Funktion spezielle Unterstützung für Anwendungen, die OAuth- oder OpenID-Authentifizierung verwenden. Diese Unterstützung basiert auf Heuristik: Wenn die aktuelle *IIdentity.Name* beginnt mit http:// oder https://, dann Benutzernamen Vergleiche erfolgen statt einen ordinalen Vergleich der OrdinalIgnoreCase Standardvergleich.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Konfiguration und Erweiterbarkeit

In einigen Fällen möchten Entwickler möglicherweise eine bessere Kontrolle über die Anti-XSRF-Generierung und Überprüfung Verhalten. Z. B. möglicherweise die MVC und Web Pages-Hilfsprogramme Standardverhalten Automatisches Hinzufügen von HTTP-Cookies an die Antwort nicht erwünscht ist, und der Entwickler möglicherweise möchten Sie die Token an anderer Stelle beizubehalten. Vorhanden sind zwei APIs, um dies zu unterstützen:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

Die *GetTokens* Methode nimmt als Eingabe ein vorhandenes XSRF-Anforderung Überprüfung Sitzungstoken (der null sein kann) und wird als Ausgabe eine neue XSRF-Anforderung Überprüfung Sitzungstoken und Feldtoken. Die Token sind einfach opake Zeichenfolgen ohne Dekoration; die *FormToken* Wert wird für die Instanz nicht umschlossen werden ein &lt;Eingabe&gt; Tag. Die *NewCookieToken* Wert ist möglicherweise null ist; in diesem Fall wird die *OldCookieToken* Wert immer noch gültig ist und kein neues Antwortcookie muss festgelegt werden. Der Aufrufer *GetTokens* ist verantwortlich für alle erforderlichen Antwortcookies beibehalten oder generieren alle notwendigen Markups; die *GetTokens* Methode selbst wirkt sich nicht auf die Antwort als Nebeneffekt. Die *überprüfen* Methode akzeptiert die eingehende Sitzung und Feld-Token und die oben genannten Validierungslogik durchläuft.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

Der Entwickler kann das System Anti-XSRF-Anwendung konfigurieren\_starten. Die Konfiguration ist die programmgesteuerte. Die Eigenschaften der statischen *AntiForgeryConfig* Typ werden unten beschrieben. Die meisten Benutzer, die Ansprüche verwenden sollten, die UniqueClaimTypeIdentifier-Eigenschaft festzulegen.

| **Property** | **Beschreibung** |
| --- | --- |
| **AdditionalDataProvider** | Ein [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , stellt zusätzliche Daten während der Generierung von Tokens und zusätzliche Daten während der Validierung von token verwendet. Der Standardwert ist *null*. Weitere Informationen finden Sie unter den [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) Abschnitt. |
| **CookieName** | Eine Zeichenfolge, die den Namen des HTTP-Cookies bereitstellt, die zum Speichern der Anti-XSRF-Sitzungstoken verwendet wird. Wenn dieser Wert nicht festgelegt ist, wird ein Name automatisch basierend auf der Anwendung bereitgestellten virtuellen Pfad generiert werden. Der Standardwert ist *null*. |
| **RequireSsl** | Ein boolescher Wert, der angibt, ob die Anti-XSRF-Token über einen SSL-gesicherten Kanal gesendet werden müssen. Wenn dieser Wert ist *"true"*, alle automatisch generierten Cookies müssen die "secure" Kennzeichen festgelegt, und die Anti-XSRF-APIs löst aus, wenn Sie von innerhalb einer Anforderung aufgerufen wird, die nicht über SSL gesendet wird. Der Standardwert ist *FALSE*. |
| **SuppressIdentityHeuristicChecks** | Ein boolescher Wert, der angibt, ob die Anti-XSRF-System die Unterstützung für anspruchsbasierte Identitäten deaktiviert werden soll. Wenn dieser Wert ist *"true"*, geht das System davon aus, die *IIdentity.Name* eignet sich für die Verwendung als eine eindeutige pro-Benutzer-ID und versucht nicht, besondere Schreibweisen *IClaimsIdentity*oder *ClClaimsIdentity* wie beschrieben in der [WIF / ACS / Claims-basierte Authentifizierung](#_WIF_ACS) Abschnitt. Der Standardwert ist `false`. |
| **UniqueClaimTypeIdentifier** | Eine Zeichenfolge, die angibt, welcher Anspruchstyp eignet sich für die Verwendung als eine eindeutige pro-Benutzer-ID. Wenn dieser Wert Satz und der aktuelle *IIdentity* wird angegeben, wird versucht, einen Anspruch des Typs zu extrahieren, Claims-basierte *UniqueClaimTypeIdentifier*, und der entsprechende Wert verwendet werden anstelle den Benutzernamen des Benutzers beim Generieren der Feld-Token. Wenn Sie der Anspruchstyp "" nicht gefunden wird, wird das System die Anforderung fehl. Der Standardwert ist *null*, die angibt, dass das System zu verwendende (Identitätsanbieter, den Namensbezeichner) Tupel anstelle der Benutzername des Benutzers wie zuvor beschrieben. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

Die *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* Typ ermöglicht Entwicklern, die das Verhalten des Anti-XSRF-Systems durch zusätzliche Round-Tripping-Daten in jedem Token zu erweitern. Die *GetAdditionalData* Methode jedes Mal aufgerufen, wird ein Token generiert und der zurückgegebene Wert in das generierte Token eingebettet ist. Eine Implementierung kann einen Zeitstempel, eine Nonce oder einen anderen Wert an, die, den er wünscht, dass, von dieser Methode zurückgeben.

Auf ähnliche Weise die *ValidateAdditionalData* Methode jedes Mal aufgerufen, wird ein Token überprüft, und die "zusätzliche Daten"-Zeichenfolge, die im Token eingebettet Waren an die Methode übergeben wird. Die Validierungsroutine kann ein Timeout (durch überprüfen die aktuelle Zeit mit dem Zeitpunkt, der gespeichert wurde, wenn das Token erstellt wurde) implementiert, eine Nonce überprüfen, Routine oder einen anderen gewünschten Logik.

## <a name="design-decisions-and-security-considerations"></a>Entwurfsentscheidungen und Überlegungen zur Sicherheit

Das Sicherheitstoken, das die Sitzung und Feld-Token verknüpft ist technisch nur erforderlich, wenn Sie versuchen, anonyme / nicht authentifizierte Benutzer vor XSRF-Angriffen zu schützen. Wenn der Benutzer authentifiziert ist, kann das Authentifizierungstoken selbst der (vermutlich in Form eines Cookies übermittelt) verwendet werden, als eine Hälfte der eine für die domänensynchronisierung Tokenpaars. Es sind allerdings gültige Szenarien für den Schutz von Anmeldeseiten, die von nicht authentifizierten Benutzern erreicht, und die Anti-XSRF-Logik durch immer generieren und überprüfen das Sicherheitstoken, sogar für authentifizierte Benutzer wurde vereinfacht. Es stellt auch einige zusätzlichen Schutz bereit, die ein Token von einem Angreifer, festlegen oder zu erraten, dass das Sitzungstoken, das eine andere Hürde für Angreifer zu überwinden wäre je kompromittiert wurde.

Entwickler sollten Vorsicht verwenden, wenn mehrere Anwendungen in einer einzelnen Domäne gehostet werden. Beispielsweise, obwohl *example1.cloudapp.net* und *example2.cloudapp.net* sind verschiedene Hosts besteht ein implizites Vertrauensverhältnis zwischen allen Hosts der  *\*. cloudapp.net* Domäne. Diese implizite Vertrauensstellung [können potenziell nicht vertrauenswürdige Hosts beeinflussen die Cookies des anderen](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (die gleichen Ursprungs-Richtlinien, die steuern, AJAX-Anforderungen unbedingt gelten nicht für HTTP-Cookies). Die ASP.NET Web-Stack-Laufzeit enthält Korrekturen, der Benutzernamen in das Feldtoken eingebettet ist, also auch wenn eine böswillige Unterdomäne einen Sitzungstoken überschreiben kann es kann nicht zum Generieren eines gültigen Felds Tokens für den Benutzer. Allerdings können nicht beim Hosten in einer solchen Umgebung die integrierte Anti-XSRF-Routinen weiterhin Sitzungsübernahmen oder XSRF-Anmeldung abzuwehren.

Die Anti-XSRF-Routinen sind derzeit nicht für verteidigen [Clickjacking](https://www.owasp.org/index.php/Clickjacking). Anwendungen, die sich selbst gegen Clickjacking verwendet werden soll können dies ganz einfach durch Senden eine X-Frame-Options: SAMEORIGIN-Header mit jeder Antwort. Dieser Header wird von allen aktuellen Browsern unterstützt. Weitere Informationen finden Sie unter den [IE-Blog](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), [SDL-Blog](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx), und [OWASP](https://www.owasp.org/index.php/Clickjacking). Die ASP.NET Web Stack Runtime kann in eine zukünftige Version stellen die MVC und Webseiten Anti-XSRF-Hilfsprogramme diesen Header automatisch festgelegt, damit Anwendungen gegen solche Angriffe automatisch geschützt sind.

Webentwickler sollten weiterhin, um sicherzustellen, dass ihre Website nicht für XSS-Angriffe anfällig ist. XSS-Angriffe sind sehr leistungsstark und ein erfolgreicher Exploit wird auch die ASP.NET Web Stack Runtime Schutzmaßnahmen vor XSRF-Angriffen unterbrochen.

## <a name="acknowledgment"></a>Bestätigung

[@LeviBroderick](https://twitter.com/LeviBroderick), ein Großteil der ASP-Sicherheit-Code den Großteil dieser Informationen geschrieben.
