---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web Pages (Razor) Handbuch zur Problembehandlung | Microsoft Docs
author: tfitzmac
description: "Dieser Artikel beschreibt die Probleme, die Sie möglicherweise beim Arbeiten mit ASP.NET Web Pages (Razor) und einige vorgeschlagenen Lösungen. Softwareversionen ASP.NET Web Pag..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: e507d97903583c7233456e9139e1a869f8bf75bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Pages (Razor) Handbuch zur Problembehandlung
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt die Probleme, die Sie möglicherweise beim Arbeiten mit ASP.NET Web Pages (Razor) und einige vorgeschlagenen Lösungen.
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.


Dieses Thema enthält folgende Abschnitte:

- [Probleme beim Ausführen von Seiten](#Issues_Running_.cshtml_Pages)
- [Probleme mit Razor-Code](#IssuesWithRazorCode)
- [Probleme mit Mitgliedschaft und Sicherheit](#membership)
- [Probleme beim Senden von E-Mail](#email)
- [Additional Resources](#AdditionalResources) (Zusätzliche MSBuild-Ressourcen)

Allgemeine Fragen finden Sie unter [ASP.NET Web Pages (Razor) – häufig gestellte Fragen](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Probleme beim Ausführen von Seiten

Eine Vielzahl von Problemen kann verhindern, dass *cshtml* und *vbhtml* Seiten aus ordnungsgemäß ausgeführt wird. In diesem Abschnitt werden allgemeine Fehlermeldungen aufgeführt und wahrscheinlich verursacht.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP-Fehler 403 - Verboten: Zugriff wurde verweigert.

*Sie haben nicht die Berechtigung zum Anzeigen dieses Verzeichnis oder diese Seite mit den Anmeldeinformationen, die Sie angegeben.*

Dieser Fehler kann auftreten, wenn der Server nicht die richtige Version von .NET Framework ausgeführt wird. Stellen Sie sicher, dass der Computer mit dem Server (lokal oder Remote) mindestens .NET Framework 4 installiert ist. Stellen Sie außerdem sicher, dass die Anwendung selbst konfiguriert ist, um die richtige Version auszuführen.

Wenn dieses Problem lokal angezeigt, wenn Sie in WebMatrix arbeiten, klicken Sie auf die **Website** Arbeitsbereich, und klicken Sie dann in der Strukturansicht auf **Einstellungen**. In der **.NET Framework-Version auswählen** wählen **.NET 4 (integrierter Modus)**. Wenn diese Version noch festgelegt ist, versuchen Sie WebMatrix als Administrator ausführen.

Stellen Sie sicher, dass das Stammverzeichnis der Website mindestens hat *cshtml* Datei.

Wenn dieser Fehler angezeigt wird, wenn der Webserver auf einem Remoteserver gespeichert ist, wenden Sie sich an den Serveradministrator. Stellen Sie sicher, dass der Server die .NET Framework 4 oder höher installiert. Stellen Sie außerdem sicher, dass die Anwendung in einem Anwendungspool ausgeführt wird, die Verwendung dieser Version von.NET Framework konfiguriert ist.

Wenn Sie die Kontrolle über den Server haben, stellen Sie sicher, dass sie die richtige Version von .NET Framework ausgeführt wird. Sie können auch versuchen, reparieren Sie die Installation durch Ausführen der `aspnet_regiis -iru` Befehl. (Z. B. Wenn Sie IIS nach der Installation von .NET Framework installieren, wird IIS nicht ordnungsgemäß konfiguriert werden ASP.NET-Seiten ausführen.) Weitere Informationen finden Sie unter [ASP.NET IIS Registration-Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>HTTP-Fehler 403.14 - verboten

*Der Webserver ist konfiguriert, um den Inhalt dieses Verzeichnisses nicht aufgelistet.*

Dieser Fehler kann auftreten, wenn Sie beim Anfordern einer Ressource, die geschützt ist (z. B. die *"Web.config"* Datei) oder, die sich in einem Ordner, der geschützt wird (z. B. *App\_Daten* oder *App\_Code*).

### <a name="http-error-40417---not-found"></a>HTTP-Fehler 404.17 - wurde nicht gefunden.

*Der angeforderte Inhalt Skript und wird nicht vom Handler für statische Dateien verarbeitet werden.*

Dieser Fehler kann auftreten, wenn der Server zur Verwendung von .NET Framework 4 ist nicht ordnungsgemäß konfiguriert oder höher, und daher nicht den Code in erkennt `@{ }` blockiert. Siehe die Beschreibung weiter oben für *HTTP-Fehler 403 - Verboten: Zugriff verweigert*.

### <a name="http-error-4047---not-found"></a>HTTP-Fehler 404.7 - wurde nicht gefunden.

*Das anforderungsfilterungsmodul ist so konfiguriert, dass die Dateierweiterung abgelehnt*

Dieser Fehler kann auftreten, wenn *cshtml* oder *vbhtml* Erweiterungen wurden auf dem Server explizit blockiert wurde. Ein Anzeichen für dieses Problem ist arbeitende URLs an, wenn sie keine einschließen der Erweiterung, aber die URLs, die enthalten *cshtml* oder *vbhtml* funktionieren nicht. Eine mögliche Lösung besteht darin, die Erweiterungen für des Standorts erneut zu aktivieren *"Web.config"* Datei. Im folgende Beispiel wird gezeigt, wie zum Aktivieren der *cshtml* Erweiterung.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP-Fehler 404.8 - wurde nicht gefunden.

*Das anforderungsfilterungsmodul ist so konfiguriert, dass Sie einen Pfad in der URL abgelehnt wird, einen Abschnitt "HiddenSegment" enthalten, wird.*

Dieser Fehler kann auftreten, wenn Sie beim Anfordern einer Ressource, die geschützt ist (z. B. die *"Web.config"* Datei) oder, die sich in einem Ordner, der geschützt wird (z. B. *App\_Daten* oder *App\_Code*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Diese Art von Seite wird (Serverfehler in '/' Anwendung) nicht verarbeitet werden.

Finden Sie in der Beschreibung oben für HTTP-Fehler 404.17.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Probleme mit Razor-code

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Der Name "*Klasse*" im aktuellen Kontext nicht vorhanden

Häufig ein Grund diese Fehlermeldung angezeigt wird, `class` Verweise ein Hilfsprogramm, aber das Hilfsprogramm ist nicht installiert. Wenn Sie versuchen, eine Hilfsprogramm verwenden, aber wenn Sie das Paket von NuGet installiert haben, müssen Sie z. B. dieser Fehler angezeigt. Verwenden Sie den Katalog in WebMatrix zum Suchen und installieren das Hilfsprogramm.

Wenn das Hilfsprogramm installiert ist, aber die Seite noch nicht erkennt, versuchen Sie es durch das Hinzufügen einer `using` Anweisung, um den Code. In der `using` Anweisung Verweis der Namespace, der das Hilfsobjekt enthält. Die grundlegende Hilfsprogramme, die im Paket ASP.NET Web-Hilfsprogrammen sind beispielsweise befinden sich in der `System.Web.Helpers` Namespace. Am oberen Rand der Seite, in dem Sie das Hilfsobjekt verwenden möchten, fügen Sie die folgende Zeile hinzu:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Probleme mit Mitgliedschaft und Sicherheit

Wenn Sie das integrierte Sicherheitssystem (Mitgliedschaft) in ASP.NET Web Pages (Razor) verwenden, können Sie die folgenden Probleme auftreten.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Um diese Methode aufrufen, muss die Eigenschaft "Membership.Provider" eine Instanz von "ExtendedMembershipProvider" sein.

Dieser Fehler kann darauf hinweisen, die keine `AspNetSqlMembershipProvider` -Klasse konfiguriert ist. (Ein Symptom ist, dass die Website lokal funktioniert, aber diesen Fehler beim Veröffentlichen eines Hostinganbieters Server löst.) Eine Lösung für dieses Problem ist der einfache Mitgliedschaft explizit aktivieren, durch das Hinzufügen der folgenden an des Standorts *"Web.config"* Datei:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>Probleme beim Senden von E-Mail

Probleme beim Senden von e-Mail können schwierig sein, um zu debuggen. Eine anfängliche Problem kann sein, dass der SMTP-Server hergestellt werden kann. Wenn die Verbindung erfolgreich ist, übergibt ASP.NET die Nachricht an den SMTP-Server. Es kann jedoch Probleme mit der Nachricht selbst, die verhindert, dass den SMTP-Server senden.

Wenn Ihre Anwendung nicht erfolgreich e-Mails senden, versuchen Sie Folgendes ein:

- Der SMTP-Servername ist häufig etwa `smtp.provider.com` oder `smtp.provider.net`. Jedoch wenn Sie Ihre Website mit einem Hostinganbieter veröffentlichen, der SMTP-Servernamen zu diesem Zeitpunkt möglicherweise `localhost`. Diese Situation tritt möglicherweise nach dem Sie veröffentlicht haben, und Ihre Website auf dem Server des Anbieters ausgeführt wird, der SMTP-Server aus der Perspektive der Anwendung lokale. Diese Änderung in Server-Namen bedeutet möglicherweise, dass Sie den SMTP-Servernamen als Teil der Veröffentlichungsprozess ändern müssen.
- Die Nummer des Ports ist in der Regel 25. Allerdings erfordern einige Anbieter Port 587 oder ein anderer Port. Überprüfen Sie mit dem Besitzer des SMTP-Servers, welche Portnummer Sie verwenden rechnen.
- Stellen Sie sicher, dass Sie die richtigen Anmeldeinformationen verwenden. Wenn Sie Ihre Website mit einem Hostinganbieter veröffentlicht haben, verwenden Sie die Anmeldeinformationen, die der Anbieter zum Abrufen von e-Mails sind speziell angegeben hat. Diese Anmeldeinformationen möglicherweise von den Anmeldeinformationen unterscheiden, die Sie zum Veröffentlichen verwenden.
- In einigen Fällen benötigen nicht Sie die Anmeldeinformationen an. Wenn Sie mit Ihrem persönlichen ISP-e-Mail gesendet, möglicherweise Ihre Anmeldeinformationen von Ihrem e-Mail-Anbieter bereits kennen. Nachdem Sie veröffentlicht haben, müssen Sie andere Anmeldeinformationen als beim Testen auf dem lokalen Computer verwenden.
- Wenn Ihre e-Mail-Anbieter Verschlüsselung verwendet wird, legen Sie `WebMail.EnableSsl` auf `true`.

Ist ein Fehler beim Senden von e-Mail, eine standardmäßige ASP.NET Fehlermeldung möglicherweise die wie folgt aussieht:

![ASP.NET Fehlermeldung, wenn es ein Problem mit e-Mail-Adresse](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Sie können auch Debuggen, Probleme mit dem Senden von e-Mails mit einem `try-catch` Block, wie im folgenden Beispiel. Bei Verwendung einer `try-catch` Block, ASP.NET nicht die Standard-Fehlermeldungen angezeigt. Stattdessen können Sie die Fehler beim Erfassen der `catch` Teil des Blocks.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

Ersetzen Sie die entsprechenden Werte für `your-SMTP-server-name`und so weiter. Einige der Fehlermeldungen, die Sie auf diese Weise sehen könnten umfassen Folgendes:

- *Fehler beim Senden von e-Mail.*

    - oder - 

    *Ein Verbindungsversuch ist fehlgeschlagen, da die Gegenstelle nach einer Zeitspanne nicht ordnungsgemäß reagiert hat, oder die hergestellte Verbindung war fehlerhaft, da der Host verbunden hat nicht reagiert*

    Dieser Fehler bedeutet normalerweise, dass die Anwendung mit dem SMTP-Server keine Verbindung hergestellt werden konnte. Überprüfen Sie den Servernamen und Portnummer.
- *Postfach nicht verfügbar. Die Serverantwort war: 5.1.0 &lt; someuser@invaliddomain &gt; Absender abgelehnt: Ungültige Absender-Domäne*

    Diese Meldung kann darauf hinweisen, die die `From` Adresse ist ungültig oder fehlt.
- *Die angegebene Zeichenfolge ist nicht in Form einer e-Mail-Adresse erforderlich.*

    Dieser Fehler kann darauf hinweisen, die den Wert der `To` oder `From` Eigenschaften werden nicht als e-Mail-Adressen erkannt. (ASP.NET können nicht überprüft werden, dass die e-Mail-Adresse gültig ist, wird nur die it's im richtigen Format, z. B.  *name@domain.com* .)

> [!NOTE]
> Entfernen Sie das Markup, das den Fehler angezeigt (`@errorMessage`) vor dem Veröffentlichen der Seite mit einem live-Standort. Es ist nicht ratsam, können Benutzer, die Fehlermeldungen anzuzeigen, die Sie von einem Server zu erhalten.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET Web Pages (Razor) – häufig gestellte Fragen](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix und ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) -Forum auf der ASP.NET-Website
