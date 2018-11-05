---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) – Handbuch zur Problembehandlung | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Artikel beschreibt Probleme, die möglicherweise bei der Arbeit mit ASP.NET Web Pages (Razor) und einige Lösungsvorschläge. Softwareversionen ASP.NET Web Pag...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec8cdda5c5b298736a650f82cd6b52d73b6dfe3d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021195"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) – Handbuch zur Problembehandlung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt Probleme, die möglicherweise bei der Arbeit mit ASP.NET Web Pages (Razor) und einige Lösungsvorschläge.
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.


Dieses Thema enthält folgende Abschnitte:

- [Probleme beim Ausführen von Seiten](#Issues_Running_.cshtml_Pages)
- [Probleme mit Razor-Code](#IssuesWithRazorCode)
- [Probleme mit der Sicherheits- und Mitgliedschaftsberechtigungen](#membership)
- [Probleme beim Senden von e-Mail-Adresse](#email)
- [Additional Resources](#AdditionalResources) (Zusätzliche MSBuild-Ressourcen)

Allgemeine Fragen, finden Sie unter [ASP.NET Web Pages (Razor) – häufig gestellte Fragen](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Probleme beim Ausführen von Seiten

Eine Reihe von Problemen kann verhindern, dass *.cshtml* und *vbhtml* Seiten aus ordnungsgemäß ausgeführt. In diesem Abschnitt werden die allgemeinen Fehlermeldungen aufgeführt und wahrscheinliche Ursachen.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP-Fehler 403 - Verboten: Zugriff verweigert

*Sie haben keine Berechtigung zum Anzeigen dieses Verzeichnis oder diese Seite, die mit den Anmeldeinformationen, die Sie angegeben.*

Dieser Fehler kann auftreten, wenn der Server nicht die richtige Version von .NET Framework ausgeführt wird. Stellen Sie sicher, dass es sich bei der Computer mit dem Server (lokal oder Remote) ist mindestens .NET Framework 4 installiert ist. Außerdem stellen Sie sicher, dass die Anwendung selbst konfiguriert ist, führen Sie die richtige Version.

Wenn dieses Problem lokal angezeigt, wenn Sie in WebMatrix arbeiten, klicken Sie auf die **Site** Arbeitsbereich, und klicken Sie dann in der Strukturansicht auf **Einstellungen**. In der **Wählen der .NET Framework-Version** wählen **.NET 4 (integriert)**. Wenn diese Version bereits festgelegt ist, versuchen Sie WebMatrix als Administrator ausführen.

Stellen Sie sicher, dass mindestens eine für der Stamm Ihrer Website verfügt über *.cshtml* Datei.

Wenn dieser Fehler angezeigt wird, wenn der Webserver auf einem Remoteserver ist, wenden Sie sich an den Server-Administrator. Stellen Sie sicher, dass der Server die .NET Framework 4 oder höher installiert. Außerdem stellen Sie sicher, dass die Anwendung in einem Anwendungspool ausgeführt wird, die mit dieser Version von.NET Framework konfiguriert ist.

Wenn Sie die Kontrolle über den Server verfügen, stellen Sie sicher, dass er die richtige Version von .NET Framework ausgeführt wird. Sie können auch versuchen, reparieren Sie die Installation durch Ausführen der `aspnet_regiis -iru` Befehl. (Z. B. Wenn Sie IIS nach der Installation von .NET Framework installieren, wird IIS nicht ordnungsgemäß konfiguriert werden ASP.NET-Seiten ausführt.) Weitere Informationen finden Sie unter [ASP.NET IIS-Registrierungstool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>HTTP-Fehler beim 403.14 - verboten

*Der Webserver ist so konfiguriert, um den Inhalt dieses Verzeichnisses nicht aufgelistet.*

Dieser Fehler kann auftreten, wenn Sie eine Ressource anfordern, die geschützt ist (z. B. die *"Web.config"* Datei) oder in einem Ordner, der geschützt wird (wie *App\_Daten* oder *App\_Code*).

### <a name="http-error-40417---not-found"></a>HTTP Error 404.17 –, die nicht gefunden.

*Der angeforderte Inhalt Skript, und es wird nicht vom Handler statische Dateien verarbeitet werden.*

Dieser Fehler kann auftreten, wenn der Server zur Verwendung von .NET Framework 4 ist nicht ordnungsgemäß konfiguriert oder höher, und daher nicht den Code in erkennt `@{ }` Blöcke. Siehe die Beschreibung weiter oben für *HTTP-Fehler 403 - Verboten: Zugriff verweigert*.

### <a name="http-error-4047---not-found"></a>HTTP-Fehler 404.7 - wurde nicht gefunden.

*Die Modul für die anforderungsfilterung ist so konfiguriert, dass die Dateierweiterung verweigern*

Dieser Fehler kann auftreten, wenn *.cshtml* oder *vbhtml* Erweiterungen explizit auf dem Server blockiert wurden. Ein Symptom dieses Problems ist die Arbeit URLs aus, wenn sie nicht beinhalten, die Erweiterung, aber die URLs, die enthalten *.cshtml* oder *vbhtml* funktionieren nicht. Eine mögliche Lösung ist so aktivieren Sie erneut die Erweiterungen in der Website *"Web.config"* Datei. Das folgende Beispiel zeigt, wie Sie aktivieren die *.cshtml* Erweiterung.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP-Fehler 404.8 - wurde nicht gefunden.

*Die Modul für die anforderungsfilterung ist konfiguriert, um einen Pfad in der URL zu verweigern, die einen Abschnitt HiddenSegment enthält.*

Dieser Fehler kann auftreten, wenn Sie eine Ressource anfordern, die geschützt ist (z. B. die *"Web.config"* Datei) oder in einem Ordner, der geschützt wird (wie *App\_Daten* oder *App\_Code*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Diese Art von Seite wird (Serverfehler in Anwendung '/') nicht bereitgestellt werden.

Siehe Beschreibung weiter oben für HTTP-Fehler 404.17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Probleme mit Razor-code

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Der Name '*Klasse*' im aktuellen Kontext nicht vorhanden

Häufig ein Grund, diese Fehlermeldung angezeigt wird, die `class` Verweise, die eine Hilfsklasse, aber das Hilfsprogramm ist nicht installiert. Wenn Sie versuchen, eine Hilfsprogramm zu verwenden, aber wenn Sie das Paket von NuGet installiert haben, müssen Sie z. B. dieser Fehler angezeigt. Verwenden Sie den Katalog in WebMatrix zum Suchen und installieren das Hilfsprogramm.

Wenn das Hilfsprogramm installiert ist, aber die Seite noch nicht erkennen, versuchen Sie es durch das Hinzufügen einer `using` Anweisung, um den Code. In der `using` -Anweisung, den Namespace, die das Hilfsprogramm enthält-Referenz. Die grundlegende Hilfsprogramme, die in der ASP.NET Web Helpers Paket sind beispielsweise befinden sich in der `System.Web.Helpers` Namespace. Fügen Sie am oberen Rand der Seite, in dem Sie das Hilfsobjekt verwenden möchten, diese Zeile:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Probleme mit der Sicherheits- und Mitgliedschaftsberechtigungen

Wenn Sie das System (Mitgliedschaft) der integrierten Sicherheit in ASP.NET Web Pages (Razor) verwenden, können Sie die folgenden Probleme auftreten.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Um diese Methode aufrufen, muss die Eigenschaft "Membership.Provider" eine Instanz von "ExtendedMembershipProvider" sein.

Dieser Fehler kann darauf hinweisen, die keine `AspNetSqlMembershipProvider` -Klasse konfiguriert ist. (Ein Symptom ist, dass die Website lokal gut funktioniert, aber löst diesen Fehler aus, mit einem Hostinganbieter Server veröffentlichen.) Eine Lösung für dieses Problem ist, um einfache Mitgliedschaft explizit zu aktivieren, indem Sie Folgendes auf der Website hinzufügen *"Web.config"* Datei:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Probleme beim Senden von e-Mail-Adresse

Probleme mit der e-Mail können schwierig sein, um zu debuggen. Ein anfängliches Problem möglich, dass Sie keine Verbindung zum SMTP-Server herstellen können. Wenn die Verbindung erfolgreich ist, übergibt ASP.NET die Nachricht an den SMTP-Server. Es kann jedoch Probleme mit der Nachricht selbst, die verhindert, dass den SMTP-Server senden.

Wenn Ihre Anwendung nicht erfolgreich-e-Mail gesendet wird, versuchen Sie Folgendes ein:

- Der SMTP-Servername ist häufig mit etwa `smtp.provider.com` oder `smtp.provider.net`. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlichen, der SMTP-Servernamen an diesem Punkt möglicherweise jedoch `localhost`. Diese Situation tritt auf, da nach dem Sie veröffentlicht haben, und Ihre Website auf dem Server des Anbieters ausgeführt wird, der SMTP-Server lokal aus der Perspektive der Anwendung sein kann. Diese Änderung in Server-Namen kann bedeuten, müssen Sie den SMTP-Servernamen als Teil des Veröffentlichungsprozesses zu ändern.
- Die Nummer des Ports ist in der Regel 25. Allerdings erfordern einige Anbieter Port 587 oder einige andere Ports. Überprüfen Sie mit dem Besitzer des SMTP-Servers, welche Portnummer Sie verwenden, erwarten.
- Stellen Sie sicher, dass Sie die richtigen Anmeldeinformationen verwenden. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlicht haben, verwenden Sie die Anmeldeinformationen, die der Anbieter zum Abrufen von e-Mails werden insbesondere angegeben hat. Mit diesen Anmeldeinformationen können sich von der Anmeldeinformationen unterscheiden, die mit der Sie veröffentlicht.
- Manchmal benötigen Sie keine Anmeldeinformationen überhaupt. Wenn Sie mit Ihren persönlichen Internetdienstanbieter-e-Mail senden, kann Ihre Anmeldeinformationen von Ihren e-Mail-Anbieter bereits kennen. Nachdem Sie veröffentlicht haben, müssen Sie andere Anmeldeinformationen als wenn Sie auf dem lokalen Computer testen, verwenden.
- Wenn Ihre e-Mail-Anbieter Verschlüsselung verwendet wird, legen Sie `WebMail.EnableSsl` zu `true`.

Ist ein Fehler beim Senden von e-Mail-Adresse, können Sie eine standardmäßige ASP.NET-Fehlermeldung finden Sie unter der so aussieht:

![ASP.NET Fehlermeldung angezeigt, wenn es ein Problem mit e-Mail-Adresse](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Sie können auch Probleme mit der Senden von e-Mails mit Debuggen einer `try-catch` blockieren, wie im folgenden Beispiel gezeigt. Bei Verwendung einer `try-catch` ASP.NET-Block nicht die Standard-Fehlermeldungen angezeigt. Sie können stattdessen den Fehler im Erfassen der `catch` Teil des Blocks.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Ersetzen Sie die entsprechenden Werte für `your-SMTP-server-name`und so weiter. Einige der Fehlermeldungen, die Sie auf diese Weise finden Sie möglicherweise die folgenden:

- *Fehler beim Senden von e-Mail.*

    - oder - 

    *Ein Verbindungsversuch ist fehlgeschlagen, da die Gegenstelle nach einer Zeitspanne nicht ordnungsgemäß reagiert hat, oder die hergestellte Verbindung war fehlerhaft, da der verbundene Host reagiert nicht*

    Dieser Fehler bedeutet normalerweise, dass die Anwendung nicht an den SMTP-Server verbinden kann. Überprüfen Sie den Namen des Servers und die Portnummer.
- <em>Postfach nicht verfügbar. Die Serverantwort war: 5.1.0 &lt; someuser@invaliddomain &gt; Absender abgelehnt: Ungültige Absender-Domäne</em>

    Diese Meldung kann darauf hinweisen, die die `From` Adresse ist nicht korrekt, oder ist nicht vorhanden.
- *Die angegebene Zeichenfolge ist nicht in der Form, die eine e-Mail-Adresse erforderlich.*

    Dieser Fehler kann darauf hinweisen, die den Wert des der `To` oder `From` Eigenschaften sind nicht als e-Mail-Adressen erkannt. (ASP.NET kann nicht überprüfen, ob die e-Mail-Adresse ist gültig, nur, dass die It ist das richtige Format, z. B. *name@domain.com*.)

> [!NOTE]
> Entfernen Sie das Markup, das den Fehler anzeigt (`@errorMessage`), bevor Sie die Seite auf einem live-Website veröffentlichen. Es ist nicht ratsam, können Benutzer Fehlermeldungen angezeigt werden, die Sie von einem Server zu erhalten.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET-Webseiten (Razor) – FAQs](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix und ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) -Forum auf der ASP.NET-Website
