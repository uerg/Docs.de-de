---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: "Forms Authentifizierungskonfiguration und Weiterführende Themen (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm wird die verschiedenen Einstellungen für die Formularauthentifizierung untersuchen und finden Sie unter wie sie über das Forms-Element zu ändern. Dadurch wird eine detaillierte gelten..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: c57722965b510ac4f5cf0c06c7c01c8cea26384f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfiguration der Formularauthentifizierung und Weiterführende Themen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In diesem Lernprogramm wird die verschiedenen Einstellungen für die Formularauthentifizierung untersuchen und finden Sie unter wie sie über das Forms-Element zu ändern. Dadurch wird eine ausführliche Übersicht über das Anpassen der Timeoutwert für das Formularauthentifizierungsticket, verwenden eine Anmeldeseite mit einer benutzerdefinierten URL (z. B. SignIn.aspx anstelle von "Login.aspx") und cookieless Formularauthentifizierungstickets gelten.


## <a name="introduction"></a>Einführung

In der [vorherigen Lernprogramm](an-overview-of-forms-authentication-cs.md) erläutert die Schritte zum Implementieren der Formularauthentifizierung in einer ASP.NET-Anwendung, von der Angabe von Konfigurationseinstellungen in der Datei "Web.config" zum Erstellen eines Protokolls für die verschiedenen auf der Seite anzeigen Inhalt für authentifizierte und anonyme Benutzer. Beachten Sie, dass wir die Verwendung der Formularauthentifizierung durch Festlegen der Mode-Attribut-Website konfiguriert die &lt;Authentifizierung&gt; Element Formulare. Die &lt;Authentifizierung&gt; Element kann optional enthalten eine &lt;Forms&gt; untergeordnetes Element, über die eine Sammlung von Einstellungen für die Formularauthentifizierung angegeben werden kann.

In diesem Lernprogramm wir untersuchen die unterschiedlichen Einstellungen für die Formularauthentifizierung und erfahren Sie, wie sie durch Ändern der &lt;Forms&gt; Element. Dadurch wird eine ausführliche Übersicht über das Anpassen der Timeoutwert für das Formularauthentifizierungsticket, verwenden eine Anmeldeseite mit einer benutzerdefinierten URL (z. B. SignIn.aspx anstelle von "Login.aspx") und cookieless Formularauthentifizierungstickets gelten. Außerdem wird die Zusammensetzung das Formularauthentifizierungsticket genauer untersuchen und finden Sie unter der Vorsichtsmaßnahmen, die ASP.NET benötigt, um sicherzustellen, dass das Ticket Daten aus dem überprüfungs- und Manipulationen sicher ist. Schließlich untersuchen wir wie zusätzliche Benutzerdaten in das Formularauthentifizierungsticket gespeichert und wie diese Daten über einen benutzerdefinierten principal-Objekt zu modellieren.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Schritt 1: Untersuchen der &lt;Forms&gt; -Konfigurationseinstellungen

Das Authentifizierungssystem Formularen in ASP.NET bietet eine Reihe von Konfigurationseinstellungen, die auf eine Anwendungsbasis angepasst werden kann. Hierzu zählen Einstellungen wie: die Lebensdauer der Formularauthentifizierung ticket; Welche Art des Schutzes wird auf das Ticket angewendet; Tickets werden verwendet, unter welchen Bedingungen Authentifizierung ohne Cookies. der Pfad zur Anmeldeseite. und andere Informationen. Um die Standardwerte zu ändern, fügen einen [ &lt;Forms&gt; Element](https://msdn.microsoft.com/library/1d3t3c61.aspx) als untergeordnetes Element von der [ &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx), diese Eigenschaft angeben Werte, die Sie als XML-Attribute anpassen möchten, wie folgt:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabelle 1 werden die Eigenschaften zusammengefasst, die über angepasst werden können die &lt;Forms&gt; Element. Da die Datei "Web.config" eine XML-Datei ist, sind die Namen der Attribute in der linken Spalte Groß-/Kleinschreibung beachtet.

| **Attribut** | **Beschreibung** |
| --- | --- |
| ohne Cookies | Dieses Attribut gibt an, unter welchen Bedingungen das Authentifizierungsticket in einem Cookie und wird in die URL eingebettet gespeichert ist. Zulässige Werte sind: UseCookies; UseUri; AutoErmittlung; und UseDeviceProfile (Standard). Schritt 2 überprüft diese Einstellung im Detail. |
| defaultUrl | Gibt die URL, an die Benutzer nach der Anmeldung auf der Anmeldeseite von es ist kein umleitungs-URL-Wert, der in der Abfragezeichenfolge angegeben umgeleitet werden. Der Standardwert ist "default.aspx". |
| Domäne | Bei Verwendung von Cookie-basierte Authentifizierungstickets wird mit dieser Einstellung Domänenwert für das Cookie. Der Standardwert ist eine leere Zeichenfolge, die bewirkt, dass der Browser die Domäne verwenden, von der aus (z. B. www.yourdomain.com) ausgestellt wurde. In diesem Fall das Cookie wird **nicht** gesendet werden, wenn Sie festlegen, dass untergeordnete Domänen, z. B. admin.yourdomain.com anfordert. Wenn Sie möchten das Cookie an alle Unterdomänen übergeben werden, Sie die Festlegung auf IhreDomäne.com Domänenattribut anpassen müssen. |
| enableCrossAppRedirects | Ein boolescher Wert, der angibt, ob der authentifizierte Benutzer gespeichert werden, wenn URLs in andere Webanwendungen auf demselben Server umgeleitet. Der Standardwert ist false. |
| loginUrl | Die URL der Anmeldeseite. Der Standardwert ist "Login.aspx". |
| Name | Wenn Tickets Cookie-basierte Authentifizierung, die den Namen des Cookies verwenden zu können. Der Standardwert ist. ASPXAUTH. |
| path | Bei Verwendung von Cookie-basierte Authentifizierungstickets wird mit dieser Einstellung das Cookie Path-Attribut. Der Path-Attribut kann der Entwickler den Umfang eines Cookies zu einem bestimmten Verzeichnishierarchie zu beschränken. Der Standardwert ist / dem informiert des Browsers, um das Ticket Authentifizierungscookie zu jeder Anforderung an die Domäne zu senden. |
| Schutz | Gibt an, welche Methoden verwendet werden, um das Formularauthentifizierungsticket zu schützen. Die zulässigen Werte sind: alle (Standardeinstellung); Verschlüsselung; None; und -Validierung. Diese Einstellungen werden in Schritt 3 im Detail erläutert. |
| requireSSL | Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungscookie zu übermitteln. Der Standardwert ist false. |
| slidingExpiration | Ein boolescher Wert, der angibt, ob das Authentifizierungscookie Timeout jedes Mal zurückgesetzt wird der Benutzer die Website während einer Sitzung besucht wird. Der Standardwert ist true. Timeout die Authentifizierungsrichtlinie für Ticket wird ausführlicher in den unter Angabe der Ticket-Timeoutwert-Abschnitt. |
| timeout | Gibt die Zeit in Minuten, nach denen das Ticket Authentifizierungscookie abläuft. Der Standardwert ist 30. Timeout die Authentifizierungsrichtlinie für Ticket wird ausführlicher in den unter Angabe der Ticket-Timeoutwert-Abschnitt. |

**Tabelle 1**: eine Zusammenfassung der &lt;Forms&gt; Attribute des Elements

In ASP.NET 2.0 und jenseits der Standardgrenze wird Forms Authentifizierungswerte in der FormsAuthenticationConfiguration-Klasse in .NET Framework hartcodiert sind. Alle Änderungen müssen auf Basis von Anwendung in der Datei "Web.config" angewendet werden. Dies unterscheidet sich von ASP.NET 1.x, in dem die Standardwerte der Forms-Authentifizierung in der Datei "Machine.config" gespeichert wurden (und deshalb über das Bearbeiten von "Machine.config" geändert werden). Zeit für das Thema von ASP.NET 1.x, lohnt es sich, nennen Sie, dass eine Anzahl von den Einstellungen für Formularauthentifizierung System unterschiedliche Standardwerte in ASP.NET 2.0 haben und mehr als in ASP.NET 1.x. Wenn Sie Ihre Anwendung von einer ASP.NET 1.x-Umgebung migrieren, ist es wichtig, diese Unterschiede bewusst sein. Wenden Sie sich an [der &lt;Forms&gt; Element technische Dokumentation](https://msdn.microsoft.com/library/1d3t3c61.aspx) eine Liste der Unterschiede.

> [!NOTE]
> Verschiedene Einstellungen für die Formularauthentifizierung, z. B. Timeout, Domäne und den Pfad, geben Sie Details für das resultierende Ticket Formularauthentifizierungscookies. Weitere Informationen auf Cookies, deren Funktionsweise und ihre verschiedenen Eigenschaften lesen [dieses Lernprogramms Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Des Tickets-Timeoutwert angeben

Das Formularauthentifizierungsticket ist ein Token, das eine Identität darstellt. Mit dem Cookie-basierte Authentifizierungstickets dieses Token in Form eines Cookies gespeichert und bei jeder Anforderung an den Webserver gesendet. Besitz des Tokens, im Wesentlichen wird deklariert, ich bin *Benutzername*, ich bereits angemeldet sind, und wird verwendet, damit der Identität eines Benutzers über die Besuche Seite gespeichert werden kann.

Das Formularauthentifizierungsticket umfasst nicht nur die Identität des Benutzers, sondern enthält auch Informationen, um die Integrität und Sicherheit des Tokens zu gewährleisten. Nachdem alle soll nicht wir einen böswilligen Benutzer in der Lage, um eine gefälschte Token zu erstellen und ändern ein legitim Token in irgendeiner underhanded sein.

Ein solcher Bit von Tickets enthalten Informationen eine *Ablauf*, der das Datum und die Uhrzeit, die das Ticket nicht mehr gültig ist ist. Jedes Mal die FormsAuthenticationModule ein Authentifizierungsticket untersucht wird sichergestellt, dass das Ticket Ablauf noch nicht überschritten hat. Wenn dies der Fall, ignoriert das Ticket und identifiziert den Benutzer als anonyme. Diese Schutzmaßnahme schützt vor Replay-Angriffen. Ohne ein Ablauf, wenn ein Hacker auf gültige Authentifizierungsticket eines Benutzers - vielleicht erhalten von physischen Zugriff auf ihren Computer verschafft und über ihre Cookies rooting konnte-einer Anforderungs an den Server mit diesem gestohlenen Authentifizierungsticket Versand konnte und Eintrag zu erhalten. Während des Ablaufs dieses Szenario verhindern nicht, wird es im Fenster beschränkt, währenddessen ein solcher Angriff erfolgreich ausgeführt werden kann.

> [!NOTE]
> Schritt 3 Details zusätzlichen Techniken ebenfalls hilfreich, in dem Forms-Authentifizierungssystem das Authentifizierungsticket geschützt.


Wenn Sie das Authentifizierungsticket zu erstellen, bestimmt der Forms-Authentifizierungssystem der ablaufzeitperiode durch consulting Timeouteinstellung. Dies bedeutet, dass es sich bei der ablaufzeitperiode in ein Datum und Uhrzeit in der Zukunft 30 Minuten festgelegt ist, wenn das Formularauthentifizierungsticket erstellt wird, wie in Tabelle 1, das Festlegen von Standardwerten auf 30 Minuten Timeout erwähnt.

Die Ablaufzeit definiert eine absolute Uhrzeit des Ablaufs das Formularauthentifizierungsticket in der Zukunft. Aber in der Regel Entwickler möchte eine gleitende Ablaufwert zu implementieren, die zurückgesetzt wird, jedes Mal, wenn der Benutzer die Website erneut ansehen. Dieses Verhalten wird durch die Einstellungen SlidingExpiration bestimmt. Wenn auf "true" (Standard), jedes Mal die FormsAuthenticationModule einen Benutzer authentifiziert festgelegt, das Ticket Ablauf aktualisiert. Wenn false wird der Ablauf nicht bei jeder Anforderung aktualisiert wird, erstellt das Ticket abläuft genau Timeout Anzahl von Minuten nach, wann das Ticket wurde und somit verursacht.

> [!NOTE]
> Die Ablaufzeit in das Authentifizierungsticket gespeichert ist, ein absolutes Datum und Zeitwert, z. B. 2 August 2008 11:34 Uhr. Darüber hinaus sind das Datum und die Uhrzeit relativ zur Ortszeit des Servers ein. Diesen Entwurf entscheiden kann einige interessante Nebenwirkungen auf Daylight Saving Time (DST) verfügen, wenn Uhren in den Vereinigten Staaten im Voraus eine Stunde (vorausgesetzt, dass der Webserver in einem Gebietsschema gehostet wird, in dem Sommerzeit eingehalten wird) verschoben werden. Beachten Sie, was für eine ASP.NET-Website mit einer 30 Minuten nach Ablauf der Zeitanzeige passieren würde, die DST beginnt (Hierbei handelt es um 2:00 Uhr). Angenommen Sie, ein Besucher mit dem Standort am 11. März 2008 um 1:55 Uhr anmeldet. Dadurch würde ein Formularauthentifizierungsticket generiert, die am 11. März 2008 um 2:25 Uhr (30 Minuten in der Zukunft) abläuft. Allerdings nach um 02:00 Uhr führt ein, springt die Uhr um 3:00 Uhr aufgrund DST. Wenn der Benutzer eine neue Seite sechs Minuten nach dem Anmelden bei (um 15:01 Uhr) lädt, notiert die FormsAuthenticationModule an, dass das Ticket abgelaufen ist und den Benutzer zur Anmeldeseite leitet. Übernehmen Sie für eine ausführlichere Beschreibung dieser und anderen Authentifizierung Ticket Timeout eigentümlichkeiten, als auch problemumgehungen, eine Kopie des Stefan Schackow *Professional ASP.NET 2.0-Sicherheit, Mitgliedschaft und Rollenverwaltung* (ISBN-Nummer: 978-0-7645-9698-8).


Abbildung 1 zeigt den Workflow aus, wenn SlidingExpiration auf "false" festgelegt ist und den Timeout auf 30 festgelegt ist. Beachten Sie, dass das Authentifizierungsticket, die bei der Anmeldung generiert, das Ablaufdatum enthält und dieser Wert wird bei nachfolgenden Anforderungen nicht aktualisiert. Wenn die FormsAuthenticationModule feststellt, dass das Ticket abgelaufen ist, wird diese verworfen und die Anforderung als anonym behandelt.


[![Eine Grafische Darstellung der das Formularauthentifizierungsticket Ablauf beim SlidingExpiration wird "false"](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Abbildung 01**: eine Grafische Darstellung der das Formularauthentifizierungsticket Ablauf beim SlidingExpiration wird "false" ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Abbildung 2 zeigt den Workflow aus, wenn SlidingExpiration festgelegt ist, auf "true" und das Timeout auf 30 festgelegt ist. Wenn eine authentifizierte Anforderung, (mit einem nicht abgelaufenen Ticket empfangen wird) wird der ablaufzeitperiode Timeout Anzahl der Minuten in der Zukunft aktualisiert.


[![Eine Grafische Darstellung der das Formularauthentifizierungsticket SlidingExpiration ist bei "true"](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Abbildung 02**: eine Grafische Darstellung der das Formularauthentifizierungsticket SlidingExpiration ist bei "true" ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Bei Verwendung von Cookie-basierte Authentifizierungstickets (Standard) wird in dieser Diskussion etwas verwirrend, da Cookies auch ihre eigenen Cacheobjekte angegeben haben, können. Ein Cookie-Ablauf (oder fehlen) der Browser angewiesen, wenn das Cookie zerstört werden soll. Wenn das Cookie nicht über einem Ablaufwert verfügt, ist es zerstört, wenn der Browser geschlossen. Wenn einem Ablaufwert vorhanden ist, jedoch das Cookie bleibt bis zum Ablaufdatum auf dem Computer des Benutzers gespeichert und im Ablauf angegebenen Zeit vergangen. Wenn Cookies vom Browser zerstört wird, wird er nicht mehr an den Webserver gesendet. Aus diesem Grund ist die Zerstörung eines Cookies analog für den Benutzer aus der Website anmelden.

> [!NOTE]
> Natürlich kann ein Benutzer Cookies auf ihrem Computer gespeichert proaktiv entfernen. In Internet Explorer 7 würden Sie wechseln Sie zu Extras, Optionen, und klicken Sie auf die Schaltfläche "löschen" im Bereich Verlauf durchsuchen. Klicken Sie dort auf die Schaltfläche zum Löschen von Cookies.


Der Forms-Authentifizierungssystem erstellt sitzungsbasierte oder Ablauf-basierten Cookies je nach dem Wert, der zum Übergeben der *PersistCookie* Parameter. Wie bereits erwähnt, mit denen das FormsAuthentication-Klasse, GetAuthCookie SetAuthCookie und RedirectFromLoginPage Methoden in zwei Eingabeparameter: *Benutzername* und *PersistCookie*. Die Anmeldeseite in dem vorherigen Lernprogramm erstellten enthalten ein Anmeldedaten Kontrollkästchen, der bestimmt, ob ein permanentes Cookie erstellt wurde. Permanente Cookies sind Ablauf-basiert. nicht persistenter Cookies sind sitzungsbasiert.

Bereits vorgestellten Konzepte Timeout und SlidingExpiration gelten die gleiche für beide und Ablauf-sitzungsbasierte Cookies. Es ist nur ein geringfügiger Unterschied bei der Ausführung: bei Ablauf-basierte Cookies mit SlidingTimeout auf "true" festgelegt ist Ablaufdatum für das Cookie wird nur aktualisiert, wenn mehr als die Hälfte der angegebenen Zeit abgelaufen.

Wir aktualisieren unsere Website Authentifizierungsrichtlinien Ticket Timeout damit, das Timeout nach einer Stunde (60 Minuten), verwenden eine gleitende Ablaufzeit tickets. Diese Änderung wirksam, aktualisieren Sie die Datei "Web.config" Hinzufügen einer &lt;Forms&gt; Element an der &lt;Authentifizierung&gt; Element durch das folgende Markup:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Verwenden eine URL für die Anmeldeseite als "Login.aspx"

Da die FormsAuthenticationModule nicht autorisierte Benutzer automatisch zur Anmeldeseite umgeleitet, muss der Anmeldeseite-URL kennen. Diese URL wird angegeben, indem das Attribut "LoginUrl" in der &lt;Forms&gt; Element und der Standardwert ist "Login.aspx". Wenn Sie über eine vorhandene Website portieren, möglicherweise verfügen Sie bereits eine Anmeldeseite mit einer anderen URL, eine, die bereits mit einem Lesezeichen versehene und von Suchmaschinen indiziert wurden. Anstelle die vorhandenen Anmeldeseite an "Login.aspx" umbenennen und wichtige Links und Lesezeichen, Benutzer, können Sie stattdessen die LoginUrl Attribut auf der Anmeldeseite ändern.

Beispielsweise, wenn Ihre Anmeldeseite SignIn.aspx benannt wurde, befand sich im Verzeichnis Benutzer Sie konnte zeigen die Konfigurationseinstellung LoginUrl ~/Users/SignIn.aspx wie folgt:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Da die aktuelle Anwendung bereits eine Anmeldeseite mit dem Namen "Login.aspx" aufweist, besteht keine Notwendigkeit, geben Sie einen benutzerdefinierten Wert in der &lt;Forms&gt; Element.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Schritt 2: Verwenden von Cookieless Formularauthentifizierungstickets

Standardmäßig bestimmt der Forms-Authentifizierungssystem, ob die Authentifizierungstickets in der Auflistung der Cookies zu speichern oder diese in der URL, die basierend auf der Website für den Benutzer-Agent einbetten. Alle grundlegenden Desktopbrowsern wie Internet Explorer, Firefox, Opera und Safari unterstützt Cookies, doch nicht alle mobilen Geräte führen.

Die Cookierichtlinie verwendet wird, von dem Authentifizierungssystem Forms hängt von der cookieless Einstellung in der &lt;Forms&gt; -Element, das einen von vier Werten zugewiesen werden kann:

- UseCookies - gibt an, dass die Cookie-basierte Authentifizierungstickets immer verwendet werden.
- UseUri - gibt an, dass die Cookie-basierte Authentifizierungstickets nie verwendet werden.
- AutoErmittlung – Wenn das Geräteprofil Cookies, Cookie-basierte Authentifizierungstickets nicht unterstützt werden nicht verwendet. Wenn das Geräteprofil Cookies unterstützt, wird ein Überprüfungsmechanismus verwendet, um festzustellen, ob Cookies aktiviert sind.
- UseDeviceProfile - die Standardeinstellung; Cookie-basierte Authentifizierungstickets verwendet, nur, wenn das Geräteprofil Cookies unterstützt. Keine Rückschlüsse auf eventuelle Mechanismus nicht verwendet wird.

Einstellungen für die automatische Erkennung und UseDeviceProfile basieren auf einer *Geräteprofil* Punkte, ob Cookie-basierte oder cookieless Authentifizierungstickets verwendet. ASP.NET verwaltet eine Datenbank von verschiedenen Geräten und ihre Funktionen, z. B., ob sie Cookies unterstützen, welche Version von JavaScript unterstützen und So weiter. Jedes Mal fordert ein Gerät von einem Webserver, er entlang sendet, einer Webseite ein *Benutzer-Agent-* HTTP-Header, der den Gerätetyp identifiziert. ASP.NET entspricht automatisch die angegebenen Benutzer-Agent-Zeichenfolge mit dem entsprechenden Profil in der Datenbank angegeben.

> [!NOTE]
> Diese Datenbank von Gerätefunktionen befindet sich in einer Reihe von XML-Dateien, die Folgen der [Browserdefinitionsdatei Schema](https://msdn.microsoft.com/library/ms228122.aspx). Die Standardeinstellung Gerät Profildateien befinden sich in % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Sie können auch benutzerdefinierte Dateien hinzufügen, um Ihre Anwendung App\_Browsern-Ordner. Weitere Informationen finden Sie unter [Vorgehensweise: Erkennen von Browsertypen in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Da die Standardeinstellung UseDeviceProfile ist, werden cookieless Formularauthentifizierungstickets verwendet werden, bei die Website von einem Gerät zugegriffen wird, deren Profil meldet, dass es keine Cookies unterstützt.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Das Authentifizierungsticket in der URL-Codierung

Cookies sind eine natürliche "Mittel" für einschließlich Informationen über den Browser in jede Anforderung an eine bestimmte Website, die ist, warum die Formulare standardauthentifizierungseinstellungen Cookies verwendet werden, wenn das Gerät über unterstützt. Wenn Cookies nicht unterstützt werden, müssen ein alternativen Verfahren für die Übergabe von des Authentifizierungstickets vom Client an den Server verwendet werden. Eine häufig verwendete problemumgehung in Umgebungen ohne Cookies verwendet wird um die Cookiedaten in der URL zu codieren.

Die beste Möglichkeit, um festzustellen, wie solche Informationen in die URL eingebettet werden kann, ist cookieless Authentifizierungstickets Nutzung zu erzwingen. Dies kann durch die cookieless Konfigurationseinstellung auf UseUri erfolgen:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Nachdem Sie diese Änderung vorgenommen haben, besuchen Sie die Website über einen Browser aus. Wenn als anonymer Benutzer besuchen, sieht die URLs, genau wie vor funktionsfähig. Zeigt z. B. beim Besuch der Seite "default.aspx" Adressleiste des Browsers die folgende URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Allerdings wird nach der Anmeldung das Formularauthentifizierungsticket in die URL eingebettet ist. Z. B. nach dem Zugriff auf die Anmeldeseite und als Sam anmelden, ich bin auf der Seite "default.aspx" zurückgegeben, aber die URL diesmal ist:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Das Formularauthentifizierungsticket wurde in die URL eingebettet. Die Zeichenfolge (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) stellt den Hexadezimal codierten Ticket Authentifizierungsinformationen dar und sind die gleichen Daten, die in der Regel in einem Cookie gespeichert ist.

Damit cookieless Authentifizierungstickets arbeiten kann das System muss alle URLs auf der Seite auf Einbeziehung die Authentifizierungsticketdaten codieren, andernfalls das Authentifizierungsticket geht verloren, wenn der Benutzer auf einen Link klickt. Glücklicherweise wird diese Einbetten von Logik automatisch ausgeführt. Um diese Funktion zu zeigen, öffnen Sie die Seite "default.aspx", und fügen Sie ein Linksteuerelement bzw. Eigenschaften Text und NavigateUrl Testlink und SomePage.aspx, festlegen. Es spielt keine Rolle, dass es eine Seite im Projekt mit dem Namen SomePage.aspx nicht.

Speichern Sie die Änderungen in "default.aspx", und besuchen sie dann über einen Browser. Melden Sie sich an den Standort so, dass das Formularauthentifizierungsticket in die URL eingebettet ist. Als Nächstes "default.aspx", klicken Sie auf den Link Link testen. Was ist passiert? Wenn keine Seite mit dem Namen SomePage.aspx vorhanden ist, klicken Sie dann einen 404-Fehler ist aufgetreten, aber ist nicht wichtig hier. Konzentrieren Sie stattdessen die Adressleiste in Ihrem Browser. Beachten Sie, dass sie das Formularauthentifizierungsticket in der URL enthalten.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Die URL SomePage.aspx unter folgendem Link automatisch in eine URL konvertiert wurde, die das Authentifizierungsticket - enthalten haben nicht, müssen Sie jeglichen Code schreiben! Das Authentifizierungsticket Formular wird automatisch in die URL für alle Links, die nicht mit http:// beginnen eingebettet werden oder /. Es spielt keine Rolle, wenn der Link angezeigt, in einem Aufruf von Response.Redirect, in einem Linksteuerelement oder in ein HTML-Ankerelement wird (d. h. &lt;eine Href = "..."&gt;... &lt;/a&gt;). Solange die URL wie http://www.someserver.com/SomePage.aspx oder /SomePage.aspx nicht, wird das Formularauthentifizierungsticket für uns eingebettet werden.

> [!NOTE]
> Cookieless Formularauthentifizierungstickets entsprechen, auf die gleiche Timeout Richtlinien als Cookie-basierte Authentifizierungstickets. Es gibt jedoch cookieless Authentifizierungstickets mehr anfällig für replay-Angriffe, da das Authentifizierungsticket direkt in die URL eingebettet ist. Angenommen Sie, ein Benutzer eine Website aufruft, anmeldet und fügt dann die URL in eine e-Mail an einen Kollegen aufzuheben. Wenn der Kollege auf diesen Link klickt, bevor die Ablaufzeit erreicht ist, werden sie als Benutzer angemeldet, die die e-Mail zugesandt!


## <a name="step-3-securing-the-authentication-ticket"></a>Schritt 3: Sichern der das Authentifizierungsticket

Das Formularauthentifizierungsticket wird unverschlüsselt übertragen entweder in einem Cookie oder direkt in die URL eingebettet. Zusätzlich zu den Informationen zur Dienstidentität zählen das Authentifizierungsticket auch Benutzerdaten, (wie wir in Schritt 4 angezeigt werden). Daher ist es wichtig, dass das Ticket Daten verschlüsselt werden von neugierige und (noch wichtiger), die das Forms-Authentifizierungssystem kann zu gewährleisten, dass das Ticket nicht manipuliert wurde.

Um den Datenschutz von Daten für das Ticket zu gewährleisten, kann die Formulare Authentifizierungssystem Ticketdaten verschlüsseln. Fehler beim Verschlüsseln der Ticketdaten sendet potenziell vertraulichen Informationen über die Verbindung im nur-Text.

Um ein Ticket Authentizität zu gewährleisten, muss der Forms-Authentifizierungssystem *überprüfen* des Tickets. Überprüfung dient die sicherstellen, dass ein bestimmtes Datenelement nicht geändert wurde, erfolgt über eine  *[message Authentication Code (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Kurz gesagt, ist die MAC ein kleiner Teil der Informationen, die die Daten zu, die identifizieren (in diesem Fall das Ticket) überprüft werden muss. Wenn die durch den MAC dargestellten Daten, geändert werden und klicken Sie dann die Mac- und die Daten nicht übereinstimmen. Darüber hinaus ist es rechnerisch schwierig, Zugriff auf die Daten ändern, sowohl generieren seine eigenen MAC, um die geänderten Daten entsprechen.

Beim Erstellen von (oder Ändern von) wird ein Ticket, das Forms-Authentifizierungssystem einen MAC erstellt und fügt ihn das Ticket Daten. Wenn eine nachfolgende Anforderung eingeht, vergleicht der Forms-Authentifizierungssystem die Mac- und Ticket-Daten, um die Echtheit des Ticketdaten überprüfen. Abbildung 3 wird dieser Workflow grafisch dargestellt.


[![Das Ticket Authentizität gewährleistet ist über einen MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Abbildung 03**: The Ticket Authentizität gewährleistet ist, über einen MAC ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Welche Maßnahmen auf das Authentifizierungsticket angewendet werden, hängt davon ab, die Einstellung für den Schutz in der &lt;Forms&gt; Element. Die schutzeinstellung kann einen der folgenden drei Werte zugewiesen werden:

- Alle - das Ticket ist verschlüsselt und signiert (Standard).
- Verschlüsselung – nur die Verschlüsselung wird angewendet: keine MAC wird generiert.
- None: das Ticket ist also weder verschlüsselt noch signiert.
- Überprüfung - einem MAC ist generiert, aber die Ticketdaten über die Verbindung im nur-Text gesendet werden.

Microsoft empfiehlt dringend, über die Einstellung "alle".

### <a name="setting-the-validation-and-decryption-keys"></a>Festlegen der Validierung und den Entschlüsselungsschlüssel

Die Verschlüsselung und Hashalgorithmen, die von dem Authentifizierungssystem Forms verwendet, um zu verschlüsseln, und überprüfen das Authentifizierungsticket sind anpassbare über die [ &lt;MachineKey&gt; Element](https://msdn.microsoft.com/library/w8h3skw9.aspx) in "Web.config". Tabelle 2 zeigt die &lt;MachineKey&gt; Attribute des Elements und ihren möglichen Werten.

| **Attribut** | **Beschreibung** |
| --- | --- |
| Entschlüsselung | Gibt den Algorithmus für die Verschlüsselung verwendet. Dieses Attribut kann einen der folgenden vier Werten haben: - Auto - die Standardeinstellung. Bestimmt den Algorithmus basierend auf der Länge des Attributs DecryptionKey an. -AES - verwendet die [erweiterte Verschlüsselung (Advanced Encryption Standard, AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) Algorithmus. -DES - verwendet die [Standard DES (Data Encryption)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) dieser Algorithmus wird als rechnerisch schwache und sollte nicht verwendet werden. -3DES - verwendet die [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) -Algorithmus, der durch Anwenden des DES-Algorithmus dreimal funktioniert. |
| decryptionKey | Der geheime Schlüssel des Verschlüsselungsalgorithmus verwendet. Dieser Wert muss entweder eine hexadezimale Zeichenfolge, die entsprechende Länge (basierend auf den Wert in der Entschlüsselung), automatisch generieren oder entweder Wert angefügt, IsolateApps. Hinzufügen von IsolateApps weist ASP.NET für einen eindeutigen Wert für jede Anwendung verwenden. Der Standardwert ist AutoGenerate, IsolateApps. |
| Validierung | Gibt den Algorithmus für die Überprüfung verwendet. Dieses Attribut kann einen der folgenden vier Werten haben: - AES - verwendet den Algorithmus für die erweiterte Verschlüsselung (Advanced Encryption Standard, AES). -MD5 - verwendet die [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) Algorithmus. -SHA1 - verwendet die [SHA1](http://en.wikipedia.org/wiki/Sha1) Algorithmus (Standard). -3DES - mithilfe des Triple DES-Algorithmus. |
| validationKey | Der geheime Schlüssel, die von den Validierungsalgorithmus verwendet wird. Dieser Wert muss entweder eine hexadezimale Zeichenfolge, die entsprechende Länge (basierend auf den Wert bei der Validierung), automatisch generieren oder entweder Wert angefügt, IsolateApps. Hinzufügen von IsolateApps weist ASP.NET für einen eindeutigen Wert für jede Anwendung verwenden. Der Standardwert ist AutoGenerate, IsolateApps. |

**Tabelle 2**: die &lt;MachineKey&gt; Elementattribute

Eine ausführliche Beschreibung dieser Optionen Verschlüsselung und gültigkeitsprüfung, und die vor- und Nachteile der verschiedenen Algorithmen ist nicht Gegenstand dieses Lernprogramm. Für eine ausführliche sehen Sie sich diese Probleme, einschließlich Anleitungen auf welche Verschlüsselung und gültigkeitsprüfung Algorithmen zu verwenden, welche Schlüssellängen und verwenden, und wie auf diese Schlüssel zu generieren, finden Sie am besten *Professional ASP.NET 2.0-Sicherheit, Mitgliedschaft und Rollenverwaltung* .

Standardmäßig werden für die Verschlüsselung und Überprüfung verwendeten Schlüssel automatisch für jede Anwendung generiert und diese Schlüssel werden in der Local Security Authority (LSA) gespeichert. Kurz gesagt, garantiert die Standardeinstellungen eindeutige Schlüssel für einen Server von Web-Webserver und jede Anwendung durch. Dieses Standardverhalten funktioniert daher nicht für die beiden folgenden Szenarien:

- **Webfarmen** – in einem [Webfarm](http://en.wikipedia.org/wiki/Web_farm) Szenario einer einzelnen Web-Anwendung auf mehrere Webserver für Zwecke der Skalierbarkeit und Redundanz gehostet wird. Jede eingehende Anforderung ist verteilt, auf einen Server in der Farm, was bedeutet, dass während der gesamten Lebensdauer der Sitzung eines Benutzers, verschiedene Servern verwendet werden können, um seine verschiedenen Anforderungen zu verarbeiten. Folglich jeder Server muss die gleichen Schlüssel für Verschlüsselung und gültigkeitsprüfung verwenden, sodass das Formularauthentifizierungsticket erstellt, verschlüsselte und überprüfte auf einem Server auf einem anderen Server in der Farm überprüft und entschlüsselt werden kann.
- **Cross-Ticket Anwendungsfreigabe** -ein einzelne Webserver kann mehrere ASP.NET-Anwendungen hosten. Wenn Sie für diese verschiedenen Anwendungen eine einzelne Formularauthentifizierungsticket gemeinsam nutzen müssen, ist es unabdinglich, deren Verschlüsselung und gültigkeitsprüfung Schlüssel überein.

Wenn in einer Webfarm festlegen oder Authentifizierungstickets alle Anwendungen auf demselben Server gemeinsam zu arbeiten, müssen so konfigurieren Sie die &lt;MachineKey&gt; Element, in welche Anwendungshosts betroffen sind, damit ihre DecryptionKey und Richten Sie Werte für "ValidationKey" entspricht.

Während keiner der beiden oben genannten Szenarien für unsere beispielanwendung gültig ist, können wir geben Sie explizite DecryptionKey und Werte für "ValidationKey" entspricht und definieren die Algorithmen, die verwendet werden. Hinzufügen einer &lt;MachineKey&gt; auf die Datei "Web.config" festlegen:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Weitere Informationen finden Sie [Vorgehensweise: Konfigurieren von MachineKey in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Die Werte DecryptionKey und "ValidationKey" entspricht entnommen wurden [Steve Gibson](http://www.grc.com/stevegibson.htm)des [perfekten Kennwörter Webseite](https://www.grc.com/passwords.htm), die zufällige 64-Hexadezimalzeichen auf jeder Seite Besuch generiert. Um zu verringern, die Wahrscheinlichkeit dieser Schlüssel, wie in der Produktionsanwendungen vornehmen, sollten Sie die oben aufgeführten Schlüssel auf der Seite perfekten Kennwörter durch zufällig generierten zu ersetzen.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Schritt 4: Speichern der zusätzliche Benutzerdaten im Ticket

Viele Webanwendungen Anzeigen von Informationen zu oder Anzeigen der Seite als Grundlage für den derzeit angemeldeten Benutzer. Z. B. möglicherweise eine Webseite anzeigen, den Namen des Benutzers und das Datum, an das sie zuletzt in der oberen Ecke von jeder Seite angemeldet haben, auf. Das Formularauthentifizierungsticket speichert den aktuell angemeldeten Benutzernamen des Benutzers, aber wenn alle anderen Informationen benötigt wird, muss die Seite wechseln Sie zu Benutzerspeicher - typischerweise eine Datenbank - nachgeschlagen werden die Informationen, die nicht in das Authentifizierungsticket gespeichert.

Mit ein wenig Code können wir zusätzliche Benutzerinformationen in das Formularauthentifizierungsticket gespeichert. Diese Daten können durch ausgedrückt werden, die [FormsAuthenticationTicket Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)des [UserData Eigenschaft](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Dies ist nützlich zum Ablegen kleine Mengen von Informationen über den Benutzer zu konzentrieren, die häufig erforderlich ist. In "UserData"-Eigenschaft ist Bestandteil des Authentifizierungscookies Ticket und wie die anderen Felder der Ticket, verschlüsselt und überprüft angegebene Wert basierend auf dem Forms-Authentifizierungssystem-Konfiguration. Standardmäßig ist die UserData eine leere Zeichenfolge.

Damit Benutzerdaten in das Authentifizierungsticket speichern, müssen wir viel Code auf der Anmeldungsseite zu schreiben, holt der benutzerspezifische Informationen und speichert ihn in das Ticket. Da UserData eine Eigenschaft vom Typzeichenfolge ist, müssen die darin gespeicherten Daten ordnungsgemäß als eine Zeichenfolge serialisiert werden. Angenommen Sie, dass unsere Benutzerspeicher, das Geburtsdatum des Benutzers und den Namen ihres Arbeitgebers enthalten, und wollten wir diese zwei Eigenschaftswerte in das Authentifizierungsticket zu speichern. Wir konnten diese Werte in eine Zeichenfolge serialisiert werden durch Verketten des Benutzers Datum Geburtsdatum der Zeichenfolge mit einem senkrechten Strich (|), gefolgt vom Namen Arbeitgeber. Für einen Benutzer auf 15 August 1974 geboren, die von Northwind Traders arbeitet, es würde die UserData-Eigenschaft zuweisen die Zeichenfolge: 1974-08-15 | Northwind Traders.

Es benötigt Zugriff auf die Daten in das Ticket gespeichert werden, können wir zu diesem Zweck die aktuelle Anforderung FormsAuthenticationTicket grabbing und Deserialisieren der UserData-Eigenschaft. Bei das Datum der Geburtsdatum und Arbeitgeber Beispiel würden wir die UserData-Zeichenfolge in beiden Teilzeichenfolgen, die auf Grundlage des Trennzeichens (|) aufgeteilt.


[![Zusätzliche Benutzerinformationen kann in das Authentifizierungsticket gespeichert werden](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Abbildung 04**: zusätzliche Benutzer Daten gespeichert werden können in das Authentifizierungsticket ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Schreiben von Informationen in UserData

Leider ist es nicht so einfach wie zu erwarten wäre, die eine Formularauthentifizierungsticket benutzerspezifische Informationen hinzugefügt. Die UserData-Eigenschaft der Klasse FormsAuthenticationTicket ist schreibgeschützt und kann nur über den Klassenkonstruktor FormsAuthenticationTicket angegeben werden. Wenn Sie die Eigenschaft UserData im Konstruktor angeben, müssen ebenfalls das ticket's andere Werte angeben: den Benutzernamen, das Datum, Ablauf und So weiter. Wenn die Anmeldeseite in dem vorherigen Lernprogramm erstellt haben, wurde dies alle für uns auf die FormsAuthentication-Klasse behandelt. Wenn UserData FormsAuthenticationTicket hinzufügen, müssen wir den Code schreiben, um den Großteil der Funktionalität, die bereits von FormsAuthentication-Klasse bereitgestellte replizieren.

Betrachten Sie den erforderlichen Code für die Arbeit mit Benutzerdaten durch Aktualisieren der Seite "Login.aspx", um zusätzliche Informationen zu dem Benutzer das Authentifizierungsticket aufzuzeichnen. Nehmen Sie an, dass unsere Benutzerspeicher enthält Informationen über das Unternehmen für der Benutzer arbeitet und ihre Titel und wir diese Informationen in das Authentifizierungsticket erfassen möchten. Aktualisieren Sie den Anzeigezustand der Seite "Login.aspx" LoginButton-Click-Ereignishandler, sodass der Code wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Nehmen wir ein Durchlaufen dieser Code zeilenweise zu einem Zeitpunkt. Die Methode startet, durch die Definition von vier Zeichenfolgenarrays: Benutzer, Kennwörter, CompanyName und TitleAtCompany. Diese Arrays enthalten den Benutzernamen, Kennwörter, Firmennamen und Titel für die Benutzerkonten im System, der diese stehen drei: Scott Jisun und Sam. In einer realen Anwendung würde diese Werte aus dem Benutzerspeicher nicht hartcodiert im Quellcode für die Seite abgefragt werden.

Im vorherigen Lernprogramm Wenn die angegebenen Anmeldeinformationen gültig sind aufgerufen wir einfach FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), die die folgenden Schritte ausgeführt:

1. Erstellt die Formulare Authentifizierungsticket
2. Das Ticket in der entsprechenden Speicher geschrieben. Für Cookies basierende Authentifizierungstickets wird der Browser Cookies Auflistung verwendet. cookieless Authentifizierungstickets werden die Ticketdaten in der URL serialisiert.
3. Den Benutzer wird an die entsprechende Seite umgeleitet.

Diese Schritte werden im obigen Code repliziert. Zunächst wird die Zeichenfolge, die wir in der Eigenschaft UserData schließlich gespeichert werden gebildet, indem Sie den Firmennamen und den vollständigen Titel, begrenzen die beiden Werte durch einen senkrechten Strich (|) kombiniert.

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Als Nächstes die FormsAuthentication.GetAuthCookie Methode aufgerufen wird, wodurch das Authentifizierungsticket entsteht, verschlüsselt es gemäß den Konfigurationseinstellungen überprüft und platziert es in einem HttpCookie-Objekt.

HttpCookie AuthCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked);

Um mit der das Cookie eingebettet FormAuthenticationTicket arbeiten, müssen wir die FormAuthentication Klasse aufrufen, [entschlüsseln Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), und übergeben Sie den Cookiewert.

FormsAuthenticationTicket Ticket = FormsAuthentication.Decrypt(authCookie.Value);

Dann erstellen Sie eine *neue* FormsAuthenticationTicket-Instanz basierend auf den vorhandenen FormsAuthenticationTicket-Werte. Dieses neue Ticket enthält jedoch die benutzerspezifische Informationen (UserDataString).

FormsAuthenticationTicket NewTicket = neue FormsAuthenticationTicket (Ticket. Ticket-Version. Der Name, Ticket. IssueDate, ticket. Ablauf, Ticket. IsPersistent UserDataString);

Wir dann verschlüsseln (und überprüfen) die neue FormsAuthenticationTicket-Instanz durch Aufrufen der [verschlüsseln Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx), und diese verschlüsselten (und überprüften) Daten zurück in AuthCookie gelegt.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Schließlich AuthCookie der Response.Cookies-Auflistung hinzugefügt wird, und die GetRedirectUrl-Methode wird aufgerufen, um zu bestimmen, die entsprechende Seite, um den Benutzer zu senden.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Alle mit diesem Code ist erforderlich, da die UserData-Eigenschaft ist schreibgeschützt und das FormsAuthentication-Klasse keine Methoden zum Angeben von UserData Informationen in den Methoden GetAuthCookie, SetAuthCookie oder RedirectFromLoginPage bereit.

> [!NOTE]
> Der Code, den wir soeben untersucht speichert benutzerspezifische Informationen in eine auf Cookies basierende Authentifizierungsticket. Die Klassen, die zum Serialisieren von des Formularauthentifizierungstickets an die URL sind für das .NET Framework intern. Lange Story kurze, Sie können nicht Benutzerdaten in einem cookieless Formularauthentifizierungsticket speichern.


### <a name="accessing-the-userdata-information"></a>Zugreifen auf die UserData-Informationen

An diesem Punkt Firmennamen und Titel des Benutzers in das Formularauthentifizierungsticket UserData Eigenschaft gespeichert bei der Anmeldung. Diese Informationen kann über das Authentifizierungsticket auf einer beliebigen Seite zugegriffen werden, ohne einen Trip zur Speicher Benutzers. Um zu veranschaulichen, wie diese Informationen aus der UserData-Eigenschaft abgerufen werden kann, wir aktualisieren "default.aspx", sodass die Begrüßungsnachricht enthält, nicht nur den Benutzernamen, jedoch auch das Unternehmen aus, dem sie für die funktionieren und deren Titel.

"Default.aspx" wird derzeit ein AuthenticatedMessagePanel-Bereich mit einem Label-Steuerelement mit dem Namen WelcomeBackMessage enthält. In diesem Bereich wird nur für authentifizierte Benutzer angezeigt. Aktualisieren Sie den Code in die Seite "default.aspx" des\_Laden Sie Ereignishandler, sodass er wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Wenn Request.IsAuthenticated ist "true", Text-Eigenschaft der WelcomeBackMessage Willkommen zurück eingerichtet ist *Benutzername*. Klicken Sie dann wird die User.Identity-Eigenschaft mit einem FormsIdentity-Objekt umgewandelt, damit wir den zugrunde liegenden FormsAuthenticationTicket zugreifen können. Sobald wir FormsAuthenticationTicket haben, Deserialisieren wir die UserData-Eigenschaft in den Firmennamen und Titel. Dies wird erreicht, und teilen Sie die Zeichenfolge in einen senkrechten Strich. Anschließend werden die Firmennamen und Titel in der WelcomeBackMessage-Bezeichnung angezeigt.

Abbildung 5 zeigt einen Screenshot, der die Anzeige in Aktion an. Anmelden als Scott zeigt eine Begrüßungsnachricht zurück an, die Unternehmen und Titel des Scott enthält.


[![Die derzeit angemeldeten Benutzer des Unternehmens und Titel werden angezeigt.](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Abbildung 05**: des derzeit angemeldeten Benutzer des Unternehmens und Titel angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Das Authentifizierungsticket UserData Eigenschaft dient als Cache für den Speicher des Benutzers. Wie alle Cache muss aktualisiert werden, wenn die zugrunde liegenden Daten geändert werden. Ist eine Webseite, von der aus Benutzer seines Profils aktualisieren können, müssen z. B. die Felder in der Eigenschaft UserData zwischengespeichert aktualisiert werden, die vom Benutzer vorgenommene Änderungen widerzuspiegeln.


## <a name="step-5-using-a-custom-principal"></a>Schritt 5: Verwenden von einem benutzerdefinierten Prinzipal

Jede eingehende Anforderung versucht die FormsAuthenticationModule zur Authentifizierung des Benutzers. Wenn ein nicht abgelaufenen Authentifizierungsticket vorhanden ist, weist der FormsAuthenticationModule die HttpContext.User-Eigenschaft auf ein neues GenericPrincipal-Objekt. Dieses GenericPrincipal-Objekt hat eine Identität vom Typ FormsIdentity, die einen Verweis auf das Formularauthentifizierungsticket enthält. GenericPrincipal-Klasse enthält die bare mindestfunktionen, die durch eine Klasse, die IPrincipal implementiert erforderlich – Es muss nur noch eine Identity-Eigenschaft und eine IsInRole-Methode.

Das principal-Objekt hat zwei Aufgaben: welche Rollen gibt an, der Benutzer gehört, und die Identität der Anmeldeinformationen. Dies erfolgt über die IPrincipal-Schnittstelle IsInRole (*RoleName*)-Methode und die Identity-Eigenschaft bzw. GenericPrincipal-Klasse ermöglicht ein Zeichenfolgenarray mit Rollennamen, aus denen über ihren Konstruktor angegeben werden. die IsInRole (*RoleName*)-Methode lediglich überprüft, ob die übergebene in *RoleName* innerhalb des Zeichenfolgenarrays vorhanden ist. Die FormsAuthenticationModule GenericPrincipal erstellt, übergibt er in ein leeres Zeichenfolgenarray an die GenericPrincipal-Konstruktor. Deshalb wird jeder Aufruf von IsInRole immer "false" zurückgegeben.

GenericPrincipal-Klasse erfüllt die Anforderungen für die meisten Forms zertifikatbasierte Authentifizierungsszenarien, in denen Rollen nicht verwendet werden. Für solche Situationen, in denen die Rolle Standardbehandlung reicht nicht aus, oder wenn Sie ein benutzerdefiniertes Objekt für "IIdentity" wird der Benutzer zuordnen müssen, erstellen Sie ein benutzerdefiniertes IPrincipal Objekt während des Workflows für die Authentifizierung und die HttpContext.User-Eigenschaft zuweisen.

> [!NOTE]
> Da wir in Zukunft, Lernprogramme angezeigt wird, wenn ASP. NET Rollen-Framework ist aktiviert er erstellt ein benutzerdefinierte principal-Objekt des Typs [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) und überschreibt das Forms-Authentifizierung erstellten GenericPrincipal-Objekt. Dies geschieht, um dem Prinzipal IsInRole-Methode für die Kommunikation mit dem Rollen-Framework-API anpassen.


Da wir nicht uns mit Rollen noch Bedenken haben, wäre der einzige Grund, den wir, zum Erstellen von eines benutzerdefinierten Prinzipals an diesem Punkt den Client haben würden ein benutzerdefiniertes Objekt für "IIdentity" wird dem Prinzipal zuordnen. In Schritt 4 erläutert wird, zusätzliche Benutzerinformationen in das Authentifizierungsticket UserData-Eigenschaft, insbesondere zu speichern, des Benutzers Firmennamen und deren Titel. Die UserData Informationen ist jedoch nur über das Authentifizierungsticket zugegriffen werden kann und nur dann eine serialisierte Zeichenfolge übereinstimmt, was bedeutet, dass jedes Mal, wenn wir den Benutzerinformationen in das Ticket anzeigen möchten, wir analysieren, die UserData-Eigenschaft müssen.

Wir können die entwicklererfahrung durch Erstellen einer Klasse, die "IIdentity" wird implementiert und enthält Eigenschaften CompanyName und Titel verbessern. Auf diese Weise ein Entwickler Firmenname des aktuell angemeldeten Benutzers zugreifen kann und Titel direkt über die Eigenschaften "CompanyName" und "Title" ohne zu wissen, wie die Eigenschaft UserData analysieren erforderlich.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Erstellen das benutzerdefinierte Identitäts- und Prinzipalklassen

Für dieses Lernprogramm erstellen Sie wir die benutzerdefinierten Haupt- und Identitätsobjekte Objekte in der App\_Codeordner. Starten, indem Sie eine App\_Code Ordner zu Ihrem Projekt – mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, wählen Sie die Option ASP.NET-Ordner hinzufügen, und wählen Sie die App\_Code. Die App\_Code ist ein spezieller ASP.NET-Ordner, die Dateien Klasse speziell für die Website enthält.

> [!NOTE]
> Die App\_Codeordner sollte nur verwendet werden, wenn Sie das Projekt über das Website-Projektmodell verwalten. Bei Verwendung der [Webanwendungsprojekt-Modell](https://msdn.microsoft.com/asp.net/Aa336618.aspx), erstellen Sie einen Standardordner und Hinzufügen von Klassen mit. Sie könnten z. B. einen neuen Ordner namens Klassen hinzufügen und platzieren Sie es Ihren Code.


Fügen Sie zwei neue Klassendateien an die App\_Codeordner, eine benannte CustomIdentity.cs und eine mit dem Namen CustomPrincipal.cs.


[![Hinzufügen von der CustomIdentity und CustomPrincipal-Klassen zum Projekt](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Abbildung 06**: Hinzufügen von der CustomIdentity und der CustomPrincipal-Klassen zu einem Projekt ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


Der CustomIdentity-Klasse dient zum Implementieren der Schnittstelle "IIdentity" wird, die Eigenschaften für den Authentifizierungstyp, IsAuthenticated und Namen definiert. Zusätzlich zu diesen erforderlichen Eigenschaften sind wir interessiert, Verfügbarmachen der zugrunde liegenden Formularauthentifizierungsticket als auch Eigenschaften für den Firmennamen und Titel des Benutzers. Geben Sie den folgenden Code in der CustomIdentity-Klasse.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Beachten Sie, dass die Klasse eine Membervariable FormsAuthenticationTicket beinhaltet (\_Ticket) und, die diese Ticketinformationen muss durch den Konstruktor angegeben werden. Diese Ticketdaten werden beim Zurückgeben der Name der Identität verwendet. seine UserData-Eigenschaft wird analysiert, um die Werte für die Eigenschaften CompanyName "und" Title zurückzugeben.

Als Nächstes erstellen Sie die CustomPrincipal-Klasse. Da wir in diesem Zusammenhang nicht betroffenen Rollen sind akzeptiert Konstruktor der CustomPrincipal-Klasse nur ein CustomIdentity-Objekt. die IsInRole-Methode gibt immer "false" zurück.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Zuweisen einer CustomPrincipal-Objekt an die eingehende Anforderung Sicherheitskontext

Wir haben jetzt eine Klasse, die "IIdentity" wird Standardspezifikation CompanyName "und" Title-Eigenschaften verwenden erweitert, als auch eine benutzerdefinierte principal-Klasse, die benutzerdefinierte Identität verwendet. Wir sind bereit, um einen Einzelschritt der ASP.NET-Pipeline und weisen unsere benutzerdefinierte principal-Objekt an die eingehende Anforderung Sicherheitskontext.

Die ASP.NET-Pipeline eine eingehende Anforderung akzeptiert und verarbeitet diese durch eine Reihe von Schritten. Bei jedem Schritt wird ein bestimmtes Ereignis ausgelöst, wodurch für Entwickler, tippen Sie in der ASP.NET-Pipeline auf und ändern die Anforderung an bestimmten Punkten im Lebenszyklus. Die FormsAuthenticationModule wartet z. B. ASP.NET zum Auslösen der [Ereignis AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), an diesem Punkt er die eingehende Anforderung für ein Authentifizierungsticket untersucht. Wenn ein Authentifizierungsticket gefunden wird, wird ein GenericPrincipal-Objekt erstellt und der HttpContext.User-Eigenschaft zugewiesen.

Nach dem Ereignis AuthenticateRequest die ASP.NET-Pipeline löst die [PostAuthenticateRequest Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), welche ist, in dem wir das GenericPrincipal-Objekt erstellt, indem die FormsAuthenticationModule mit einer Instanz von ersetzen können unsere CustomPrincipal-Objekt. Abbildung 7 zeigt dieses Workflows.


[![GenericPrincipal wird durch eine CustomPrincipal im Ereignis PostAuthenticationRequest ersetzt.](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Abbildung 07**: die GenericPrincipal wird durch eine CustomPrincipal im Ereignis PostAuthenticationRequest ersetzt ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Um Code in ASP.NET Pipeline Ereignisses ausführen, können wir den passenden Ereignishandler in "Global.asax" erstellen oder unser eigenes HTTP-Modul erstellen. Erstellen Sie für dieses Lernprogramm nehmen wir die Ereignishandler in "Global.asax". Starten Sie, indem Sie die Website "Global.asax" hinzugefügt. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und fügen Sie ein Element vom Typ globale Anwendungsklasse mit dem Namen "Global.asax" hinzu.


[![Hinzufügen einer Datei "Global.asax" zu Ihrer Website](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Abbildung 08**: Hinzufügen einer Datei "Global.asax" zu Ihrer Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Die Standardvorlage "Global.asax" enthält die Ereignishandler für eine Reihe von ASP.NET-Pipelineereignisse, einschließlich Start und Ende und [Fehlerereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), o. ä. Gerne an diese Ereignishandler entfernt werden, da es nicht für diese Anwendung benötigt werden. Das Ereignis, dem wir interessiert ist PostAuthenticateRequest. Aktualisieren Sie die Datei "Global.asax" aus, damit das Markup etwa wie folgt aussieht:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Die Anwendung\_OnPostAuthenticateRequest-Methode wird jedes Mal, den die ASP.NET-Laufzeit löst das PostAuthenticateRequest-Ereignis, die einmal für jede eingehende Seitenanforderung ausgeführt wird. Der Ereignishandler wird gestartet, indem überprüft wird, um festzustellen, ob der Benutzer authentifiziert wird und über Formularauthentifizierung authentifiziert wurde. Wenn dies der Fall ist, wird ein neues CustomIdentity-Objekt erstellt und die aktuelle Anforderung Authentifizierungsticket in seinem Konstruktor übergeben. Danach ein CustomPrincipal-Objekt erstellt und übergeben das gerade erstellte CustomIdentity-Objekt in seinem Konstruktor. Schließlich wird der Sicherheitskontext für die aktuelle Anforderung auf das neu erstellte CustomPrincipal-Objekt zugewiesen.

Beachten Sie, dass der letzte Schritt - Sicherheitskontext für die Anforderung der CustomPrincipal-Objekt zuordnen - zwei Eigenschaften den Prinzipal zugewiesen: HttpContext.User und Thread.CurrentPrincipal. Diese zwei Zuweisungen sind erforderlich, durch das Verfahren begründet Sicherheitskontexten in ASP.NET behandelt werden. .NET Framework ordnet jeden ausgeführten Thread einen Sicherheitskontext; Diese Informationen sind verfügbar, als eine IPrincipal-Objekt, über die [Threadobjekt](https://msdn.microsoft.com/library/system.threading.thread.aspx)des [CurrentPrincipal Eigenschaft](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Was ist eine verwirrend ist, dass ASP.NET eigene Sicherheitskontextinformationen (HttpContext.User).

In bestimmten Szenarien wird die Eigenschaft Thread.CurrentPrincipal untersucht, beim Bestimmen des Sicherheitskontexts; in anderen Szenarien wird HttpContext.User verwendet. Beispielsweise bestehen Sicherheitsfunktionen, die Entwicklern deklarativ angeben, welche Benutzer ermöglichen, in .NET oder Rollen können eine Klasse instanziieren oder bestimmte Methoden aufrufen (finden Sie unter [Hinzufügen von Autorisierungsregeln für Geschäft und Daten mithilfe von Ebenen PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Im Hintergrund bestimmen diese deklarative Techniken des Sicherheitskontexts über die Thread.CurrentPrincipal-Eigenschaft.

In anderen Szenarien ist die HttpContext.User-Eigenschaft verwendet. Beispielsweise verwendet im vorherigen Lernprogramm wir diese Eigenschaft auf den angemeldeten Benutzernamen des Benutzers anzuzeigen. Es ist offensichtlich, ist es dann zwingend erforderlich, dass die Sicherheitskontextinformationen in den Eigenschaften des Thread.CurrentPrincipal und HttpContext.User überein.

Die ASP.NET-Laufzeit wird automatisch diese Eigenschaftswerte für uns synchronisiert. Allerdings erfolgt die Synchronisierung nach dem Ereignis AuthenticateRequest jedoch *vor* das PostAuthenticateRequest-Ereignis. Daher beim Hinzufügen von eines benutzerdefinierten Prinzipals im Ereignis PostAuthenticateRequest müssen bestimmte die Thread.CurrentPrincipal, da sonst Thread.CurrentPrincipal manuell zugewiesen werden und HttpContext.User wird nicht mehr synchron. Finden Sie unter [Context.User Vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) für eine ausführlichere Erläuterung zu diesem Problem.

### <a name="accessing-the-companyname-and-title-properties"></a>Zugreifen auf die CompanyName und Titeleigenschaften

Wenn eine Anforderung eingeht und an das Modul für ASP.NET, der Anwendung verteilt wird\_OnPostAuthenticateRequest-Ereignishandler in "Global.asax" ausgelöst wird. Wenn die Anforderung von der FormsAuthenticationModule erfolgreich authentifiziert wurden, wird der Ereignishandler ein neues CustomPrincipal-Objekt mit einem CustomIdentity-Objekt, das das Formularauthentifizierungsticket Grundlage erstellen. Mit dieser Logik vorhanden ist ist der Zugriff auf Informationen zu den Firmennamen und Titel des aktuell angemeldeten Benutzers äußerst einfach.

Kehren Sie zur Seite\_Load-Ereignishandler in "default.aspx" in Schritt 4 wird, in dem Code das Authentifizierungsticket Formular abrufen und analysieren die UserData-Eigenschaft geschrieben, um Firmennamen und Titel des Benutzers anzuzeigen. Mit der CustomPrincipal und CustomIdentity-Objekte jetzt ist es nicht erforderlich, die Werte aus dem Ticket UserData Eigenschaft zu analysieren. Rufen Sie stattdessen einfach einen Verweis auf das CustomIdentity-Objekt und verwenden Sie ihre Eigenschaften CompanyName "und" Title:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm untersucht wir zum Anpassen der Forms-Authentifizierungssystem-Einstellungen über die Datei "Web.config". Erläutert, wie das Authentifizierungsticket Ablauf behandelt wird und wie mit den Schutzvorrichtungen Verschlüsselung und gültigkeitsprüfung verwendet werden, um das Ticket vor überprüfungs- und die Änderung zu schützen. Abschließend wird erläutert, mit das Authentifizierungsticket UserData Eigenschaft zum Speichern von zusätzliche Benutzerinformationen in das Ticket selbst und wie benutzerdefinierte Haupt- und Identitätsobjekte Objekte verwenden, um diese Informationen in einer Weise mehr entwicklerfreundlichsten verfügbar machen.

In diesem Lernprogramm endet unsere Untersuchung der Formularauthentifizierung in ASP.NET. Der nächste Lernprogrammen beginnt unsere Reise in die Mitgliedschaft-Framework.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Formularauthentifizierung unterzogen, wodurch](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Erläutert: Formularauthentifizierung in ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Gewusst wie: Schützen Sie Formularauthentifizierung in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professionelle ASP.NET 2.0 Sicherheit, Mitgliedschaft und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sichern von Anmeldesteuerelementen](https://msdn.microsoft.com/library/ms178346.aspx)
- [Die &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Die &lt;Forms&gt; -Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Die &lt;MachineKey&gt; Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Grundlegendes zu den Formularauthentifizierungsticket und Cookies](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Lehrvideos auf die Themen in diesem Lernprogramm

- [Gewusst wie: Ändern der Authentifizierungseigenschaften Formulare](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Zum Einrichten und Verwenden von Cookies Authentifizierung in einer ASP.NET-Anwendung](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Verschieben der ASP-Formularanmeldung](../../../videos/authentication/asp-forms-login-relocation.md)
- [Benutzerdefinierte Schlüsselkonfiguration für die Formularanmeldung](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Hinzufügen von benutzerdefinierten Daten zur Authentifizierungsmethode](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Verwenden von benutzerdefinierten Prinzipalobjekten](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Alicja Maziarz. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Zurück](an-overview-of-forms-authentication-cs.md)
[Weiter](security-basics-and-asp-net-support-vb.md)
