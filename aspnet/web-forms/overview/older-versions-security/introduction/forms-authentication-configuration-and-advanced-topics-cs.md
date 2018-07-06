---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Konfiguration der Formularauthentifizierung und Weiterführende Themen (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir untersuchen Sie die verschiedenen Einstellungen für die Formularauthentifizierung und erfahren Sie, wie sie über das Forms-Element zu ändern. Dadurch wird eine detaillierte gelten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: 707c47917450bad409a04b3b5b80edc560b7c756
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372773"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfiguration der Formularauthentifizierung und Weiterführende Themen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In diesem Tutorial werden wir untersuchen Sie die verschiedenen Einstellungen für die Formularauthentifizierung und erfahren Sie, wie sie über das Forms-Element zu ändern. Dadurch wird eine ausführliche Übersicht über das Anpassen der Timeoutwert für das Formularauthentifizierungsticket über eine Anmeldeseite mit einer benutzerdefinierten URL (z. B. SignIn.aspx anstelle von "Login.aspx") und ohne Cookies Formularauthentifizierungstickets gelten.


## <a name="introduction"></a>Einführung

In der [vorherigen Tutorial](an-overview-of-forms-authentication-cs.md) wir haben uns über die erforderlichen Schritte zum Implementieren der Formularauthentifizierung in einer ASP.NET-Anwendung aus, von der Angabe von Konfigurationseinstellungen in Web.config-Datei zum Erstellen eines Protokolls für die andere Seite für die Anzeige Inhalte für authentifizierte und anonyme Benutzer. Denken Sie daran, dass wir die Website, um die Verwendung der Formularauthentifizierung durch Festlegen des modusattributs des konfiguriert die &lt;Authentifizierung&gt; Element für Formulare. Die &lt;Authentifizierung&gt; Element kann optional enthalten eine &lt;Forms&gt; untergeordnetes Element darstellt, kann eine Sammlung von Einstellungen für die Formularauthentifizierung über die angegeben werden.

In diesem Tutorial werden wir untersuchen Sie die verschiedenen Einstellungen für die Formularauthentifizierung und finden Sie unter Vorgehensweise: Ändern sie in der &lt;Forms&gt; Element. Dadurch wird eine ausführliche Übersicht über das Anpassen der Timeoutwert für das Formularauthentifizierungsticket über eine Anmeldeseite mit einer benutzerdefinierten URL (z. B. SignIn.aspx anstelle von "Login.aspx") und ohne Cookies Formularauthentifizierungstickets gelten. Wir auch die Zusammensetzung des Formularauthentifizierungstickets genauer untersuchen, und die Vorsichtsmaßnahmen, die ASP.NET benötigt, um sicherzustellen, dass das Ticket für die Daten vor Überprüfungen und Manipulation sicher ist. Abschließend werden wir wie zusätzliche Benutzerdaten in das Formularauthentifizierungsticket gespeichert und wie diese Daten über einen benutzerdefinierten Prinzipalobjekts modelliert betrachten.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Schritt 1: Untersuchen der &lt;Forms&gt; -Konfigurationseinstellungen

Das Authentifizierungssystem "Forms" in ASP.NET bietet es sich um eine Reihe von Konfigurationseinstellungen, die auf Basis von Anwendung angepasst werden kann. Hierzu zählen Einstellungen wie: die Lebensdauer der Formularauthentifizierung ticket; Welche Art von Schutz wird auf das Ticket angewendet; Tickets werden verwendet, unter welchen Bedingungen Authentifizierung ohne Cookies verwendet. der Pfad zur Anmeldeseite. und andere Informationen. Um die Standardwerte zu ändern, fügen eine [ &lt;Forms&gt; Element](https://msdn.microsoft.com/library/1d3t3c61.aspx) als untergeordnetes Element der [ &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx), diese Eigenschaft angeben Werte, die Sie als XML-Attribute anpassen möchten, wie folgt:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

In Tabelle 1 sind die Eigenschaften, die mithilfe von angepasst werden, können die &lt;Forms&gt; Element. Da "Web.config" eine XML-Datei ist, sind die Namen der Attribute in der linken Spalte Groß-/Kleinschreibung beachtet.


| <strong>Attribut</strong> |                                                                                                                                                                                                                                     <strong>Beschreibung</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         ohne Cookies         |                                                                                                                Dieses Attribut gibt an, unter welchen Bedingungen das Authentifizierungsticket in ein Cookie im Vergleich zu, die in der URL eingebettet gespeichert ist. Zulässige Werte sind: UseCookies; UseUri; AutoErmittlung; und UseDeviceProfile (Standard). Schritt 2 werden diese Einstellung im Detail untersucht.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Gibt die URL, die Benutzer in umgeleitet werden, nach der Anmeldung über die Anmeldeseite, wenn kein umleitungs-URL-in der Abfragezeichenfolge angegeben Wert. Der Standardwert ist "default.aspx".                                                                                                                                                         |
|           Domäne           | Wenn Sie cookiebasierte Authentifizierungstickets zu verwenden, gibt diese Einstellung Domäne Cookiewert. Der Standardwert ist eine leere Zeichenfolge und bewirkt, dass der Browser mit der Domäne, von der es (z. B. www.yourdomain.com) ausgestellt wurde. In diesem Fall das Cookie wird <strong>nicht</strong> gesendet werden, wenn die Erstellung von Unterdomänen, z. B. admin.yourdomain.com Anforderungen. Wenn Sie möchten das Cookie an alle Unterdomänen übergeben werden, Sie die Festlegung auf "ihredomaene.com" Domänenattribut anpassen müssen. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Ein boolescher Wert, der angibt, ob authentifizierte Benutzer gespeichert werden, wenn URLs in anderen Webanwendungen auf demselben Server umgeleitet. Der Standardwert ist false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Die URL der Anmeldeseite. Der Standardwert ist "Login.aspx".                                                                                                                                                                                                                      |
|            Name            |                                                                                                                                                                                                   Wenn Sie cookiebasierte Authentifizierungstickets, die den Namen des Cookies verwenden zu können. Der Standardwert ist. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Wenn Sie cookiebasierte Authentifizierungstickets zu verwenden, gibt diese Einstellung das Cookie des Path-Attribut. Das Path-Attribut kann ein Entwickler den Umfang eines Cookies zu einer bestimmten Verzeichnishierarchie zu beschränken. Der Standardwert ist /, dem informiert des Browsers, um das Ticket Authentifizierungscookie an alle Anforderungen an die Domäne zu senden.                                                                              |
|         Schutz         |                                                                                                                                            Gibt an, welche Verfahren verwendet werden, um das Formularauthentifizierungsticket zu schützen. Die zulässigen Werte sind: alle (Standard); Verschlüsselung None; und Überprüfung. Diese Einstellungen werden in Schritt 3 ausführlich erläutert.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungscookie zu übermitteln. Der Standardwert ist false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Ein boolescher Wert, der angibt, ob das Authentifizierungscookie Timeout jedes Mal zurückgesetzt, wird der Benutzer die Website während einer Sitzung besucht. Der Standardwert ist true. Die Ticket-Timeoutwert Authentifizierungsrichtlinie wird ausführlicher in der Angabe im Ticket-Timeoutwert-Abschnitt.                                                                                                 |
|          timeout           |                                                                                                                               Gibt die Zeit in Minuten an, nach denen das Ticket Authentifizierungscookie abläuft. Der Standardwert ist 30. Die Ticket-Timeoutwert Authentifizierungsrichtlinie wird ausführlicher in der Angabe im Ticket-Timeoutwert-Abschnitt.                                                                                                                               |

**Tabelle 1**: eine Zusammenfassung der &lt;Forms&gt; Attribute des Elements

In ASP.NET 2.0 und jenseits der Standardgrenze sind Forms Authentication Werte an, in der FormsAuthenticationConfiguration-Klasse in .NET Framework hartcodiert. Alle Änderungen müssen auf Basis von Anwendung in der Datei "Web.config" angewendet werden. Dies unterscheidet sich von ASP.NET 1.x, in dem die Standardwerte für Forms Authentication wurden gespeichert, in der Datei "Machine.config" (und können daher geändert werden, per Bearbeitung der Datei machine.config). Klicken Sie auf das Thema von ASP.NET 1.x, lohnt es sich, ganz zu schweigen, dass eine Anzahl von den Einstellungen für Formularauthentifizierung System unterschiedliche Werte in ASP.NET 2.0 haben und darüber hinaus als in ASP.NET 1.x. Wenn Sie Ihre Anwendung aus einer ASP.NET 1.x-Umgebung migrieren, ist es wichtig, diese Unterschiede berücksichtigen. Wenden Sie sich an [der &lt;Forms&gt; Element technische Dokumentation](https://msdn.microsoft.com/library/1d3t3c61.aspx) eine Liste der Unterschiede.

> [!NOTE]
> Verschiedene Einstellungen für die Formularauthentifizierung, z. B. das Timeout, die Domäne und den Pfad, geben Sie Details für die resultierende Ticket Formularauthentifizierungscookies. Weitere Informationen zu Cookies, wie sie funktionieren und die verschiedenen Eigenschaften zu erhalten, lesen [in diesem Tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Der Tickettimeoutwert angeben

Das Formularauthentifizierungsticket ist ein Token, das eine Identität darstellt. Mit cookiebasierte Authentifizierungstickets wird dieses Token, die in der Form eines Cookies gespeichert und bei jeder Anforderung an den Webserver gesendet. Besitz des Tokens, im Wesentlichen deklariert, ich bin *Benutzername*, ich bereits angemeldet sind, und wird verwendet, damit die Identität eines Benutzers gespeichert werden kann, auf der Seite besuchen.

Das Formularauthentifizierungsticket nicht nur die Identität des Benutzers enthält, sondern enthält auch Informationen, um die Integrität und Sicherheit des Tokens zu gewährleisten. Schließlich möchten wir keinen böswilligen Benutzer zum Erstellen eines Tokens gefälschten oder ein legitim Token underhanded irgendwie ändern kann.

Eine solche Bit der Informationen in das Ticket ist eine *Ablauf*, Datum und Uhrzeit, die das Ticket nicht mehr gültig ist ist. Jedes Mal die FormsAuthenticationModule ein Authentifizierungsticket untersucht wird sichergestellt, dass das Ticket für den Ablauf noch nicht überschritten hat. Falls Ja, ignoriert das Ticket und den Benutzer als anonyme identifiziert. Diese Schutzmaßnahme Schutz vor Replay-Angriffe. Ohne ein Ablauf, wenn ein Hacker ihr praxisnahe gültiges Authentifizierungsticket eines Benutzers - möglicherweise abrufen, indem physischer Zugriff auf ihren Computer und über ihre Cookies rooting konnte-sie eine Anforderung, an den Server mit diesem gestohlenen Authentifizierungsticket senden können und gewinnen Sie Eintrag. Während des Ablaufs dieses Szenario zu verhindern, dass nicht, wird es im Fenster beschränkt, in dem ein solcher Angriff erfolgreich ausgeführt werden kann.

> [!NOTE]
> Schritt 3 Details zusätzliche Techniken, die dem Authentifizierungssystem Formulare für das Authentifizierungsticket geschützt.


Wenn Sie das Authentifizierungsticket erstellen zu können, bestimmt das Authentifizierungssystem Forms der ablaufzeitperiode von consulting der Timeouteinstellung festgelegt. Dies bedeutet, dass es sich bei Ablauf dieser Frist auf Datum und Uhrzeit in der Zukunft 30 Minuten festgelegt ist, wenn das Formularauthentifizierungsticket erstellt wird, wie in Tabelle 1, das Festlegen von Standardwerten auf 30 Minuten Timeout erwähnt.

Die Ablaufzeit definiert eine absolute Uhrzeit des Ablaufs das Formularauthentifizierungsticket in der Zukunft. Aber in der Regel Entwickler einen gleitenden Ablauf, einer implementieren, die zurückgesetzt wird, jedes Mal, wenn der Benutzer die Website erneut ansehen möchten. Dieses Verhalten wird durch die SlidingExpiration Einstellungen bestimmt. Wenn Sie auf "true" (Standard), jedes Mal die FormsAuthenticationModule einen Benutzer authentifiziert festgelegt, wird das Ticket für den Ablauf aktualisiert. Wenn es sich bei false wird der Ablauf nicht bei jeder Anforderung aktualisiert wird, erstellt, wodurch das Ticket abläuft genau Timeouts Anzahl der Minuten nach, wenn das Ticket wurde.

> [!NOTE]
> Der Ablauf des in das Authentifizierungsticket gespeichert ist, ein absolutes Datum und Uhrzeitwert, z. B. 2. August 2008 11:34 Uhr. Darüber hinaus sind das Datum und die Uhrzeit relativ zur Ortszeit des Servers ein. Diese entwurfsentscheidung haben einige interessante Nebenwirkungen auf Sommerzeit (DST), handelt es sich bei der Uhren in den Vereinigten Staaten jetzt eine Stunde (vorausgesetzt, dass der Webserver in einem Gebietsschema gehostet wird, in dem Sommerzeit erkannt wird) verschoben werden. Beachten Sie, was für eine ASP.NET-Website mit einer 30-minütigen Ablauf der Zeitanzeige passieren würde, die Sommerzeit beginnt (Dies ist um 2:00 Uhr). Angenommen Sie, ein Besucher mit dem Standort am 11. März 2008 um 1:55 Uhr anmeldet. Dadurch würde ein Formularauthentifizierungsticket generiert, die am 11. März 2008 um 2:25 Uhr (30 Minuten in der Zukunft) abläuft. Allerdings nach um 02:00 Uhr wird, springt die Uhr auf 3:00 Uhr aufgrund der Sommerzeit. Wenn der Benutzer eine neue Seite sechs Minuten nach der Anmeldung (um 15:01 Uhr) geladen wird, vermerkt die FormsAuthenticationModule an, dass das Ticket ist abgelaufen, und den Benutzer zur Anmeldeseite leitet. Für eine ausführlichere Erläuterung dieser und anderen Authentifizierung Ticket Timeout eigentümlichkeiten, sowie problemumgehungen, wählen Sie eine Kopie des Stefan Schackow *Professional ASP.NET 2.0 Security, Mitgliedschafts- und Rollenverwaltung* (ISBN-Nummer: 978-0-7645-9698-8).


Abbildung 1 veranschaulicht den Workflow aus, wenn SlidingExpiration auf "false" festgelegt wird und das Timeout auf 30 festgelegt ist. Beachten Sie, dass das Authentifizierungsticket, die bei der Anmeldung generiert, das Ablaufdatum enthält aus, und dieser Wert wird bei nachfolgenden Anforderungen nicht aktualisiert. Wenn die FormsAuthenticationModule feststellt, dass das Ticket abgelaufen ist, wird diese verworfen und behandelt die Anforderung als anonym.


[![Es ist eine Grafische Darstellung der SlidingExpiration für das Formularauthentifizierungsticket Ablauf bei "false"](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Abbildung 01**: eine Grafische Darstellung des Formularauthentifizierungstickets Ablauf beim SlidingExpiration ist "false" ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Abbildung 2 zeigt den Workflow aus, wenn SlidingExpiration festgelegt ist, auf "true" und Timeout auf 30 festgelegt ist. Wenn eine authentifizierte Anforderung, (mit einer Ticket nicht abgelaufene empfangen wird) wird der ablaufzeitperiode Timeouts Anzahl von Minuten in der Zukunft aktualisiert.


[![Eine Grafische Darstellung der das Formularauthentifizierungsticket SlidingExpiration wird bei "true"](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Abbildung 02**: eine Grafische Darstellung des Formularauthentifizierungstickets SlidingExpiration wird bei "true" ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Wenn Sie cookiebasierte Authentifizierungstickets (Standard) zu verwenden, wird hier ein wenig verwirrender, da es sich bei Cookies auch ihre eigenen Cacheobjekte angegeben haben, können. Ein Cookie Ablauf (oder deren Fehlen) weist den Browser auf, wenn das Cookie zerstört werden soll. Wenn das Cookie nicht über ein Ablaufdatum verfügt, wird es zerstört, wenn der Browser geschlossen. Wenn eine ablaufangabe vorhanden ist, jedoch das Cookie auf dem Computer des Benutzers gespeichert bleibt, bis das Datum und die in der Ablauf des angegebenen Zeit vergangen. Wenn vom Browser ein Cookie zerstört wird, wird es nicht mehr an den Webserver gesendet. Aus diesem Grund ist die Zerstörung eines Cookies analog zu den Benutzer, die Abmeldung von der Website.

> [!NOTE]
> Natürlich kann ein Benutzer proaktiv alle Cookies auf ihrem Computer gespeicherten entfernen. In Internet Explorer 7 würden Sie wechseln Sie zu Extras, Optionen, und klicken Sie auf die Schaltfläche "löschen" im Abschnitt Verlauf durchsuchen. Klicken Sie dort auf die Schaltfläche zum Löschen von Cookies.


Das Forms-Authentifizierungssystem erstellt sitzungsbasierte oder Ablauf-basierten Cookies je nach Wert übergeben die *PersistCookie* Parameter. Beachten Sie, mit denen Methoden für das FormsAuthentication-Klasse GetAuthCookie, SetAuthCookie und RedirectFromLoginPage in zwei Eingabeparameter: *Benutzername* und *PersistCookie*. Die Anmeldeseite, die wir im vorherigen Tutorial erstellt haben, enthalten ein Anmeldedaten Kontrollkästchen, die bestimmt, ob ein persistentes Cookie erstellt wurde. Dauerhafte Cookies sind Ablauf-basiert. nicht beständige Cookies sind sitzungsbasiert.

Bereits vorgestellten Konzepte Timeout und SlidingExpiration gelten identisch für beide Sitzungen und Ablauf-basierende Cookies. Es gibt nur einen kleinerer Unterschied bei der Ausführung: Verwendung von Cookies Ablauf-basierte mit SlidingTimeout auf "true" festgelegt ist, das Cookie des Ablauf wird nur aktualisiert, wenn mehr als die Hälfte der angegebenen Zeit abgelaufen.

Aktualisieren wir unsere Website, die Authentifizierungsrichtlinien Ticket Timeout, damit, das Timeout nach einer Stunde (60 Minuten), die mit einer gleitenden Ablaufzeit tickets. Diese Änderung wirksam, aktualisieren Sie die Datei "Web.config" Hinzufügen einer &lt;Forms&gt; Element der &lt;Authentifizierung&gt; Element durch das folgende Markup:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Verwenden eine URL für die Anmeldeseite als "Login.aspx"

Da die FormsAuthenticationModule nicht autorisierte Benutzer automatisch zur Anmeldeseite umgeleitet wird, muss sie die Anmeldeseite-URL kennen. Diese URL wird angegeben, durch das Attribut "LoginUrl" in der &lt;Forms&gt; Element und der Standardwert ist "Login.aspx". Wenn Sie über eine vorhandene Website portieren, müssen Sie bereits eine Anmeldeseite mit einer anderen URL, eine möglicherweise, die bereits mit einem Lesezeichen versehen und von Suchmaschinen indiziert wurden. Anstatt die vorhandene Anmeldeseite an "Login.aspx" umbenennen und wichtige Links und Lesezeichen, Benutzer, können Sie stattdessen das Attribut "LoginUrl", zeigen Sie auf der Anmeldeseite ändern.

Beispielsweise wenn Ihre Anmeldeseite SignIn.aspx benannt wurde, befand sich im Verzeichnis Benutzer Sie konnte zeigen die Konfigurationseinstellung LoginUrl ~/Users/SignIn.aspx wie folgt:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Da unsere aktuelle Anwendung bereits eine Anmeldeseite mit dem Namen "Login.aspx" aufweist, ist es nicht erforderlich, geben Sie einen benutzerdefinierten Wert in der &lt;Forms&gt; Element.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Schritt 2: Verwenden von ohne Cookies Formularauthentifizierungstickets

Standardmäßig bestimmt der Forms-Authentifizierungssystem, ob die Authentifizierungstickets in der Auflistung der Cookies speichern oder Sie betten sie in der URL, die basierend auf der Website für den Benutzer-Agent. Alle grundlegenden Desktopbrowser wie Cookies für Internet Explorer, Firefox, Opera und Safari unterstützt, aber nicht alle mobilen Geräte tun.

Die Cookierichtlinie, die vom System Forms-Authentifizierung verwendet, hängt von der ohne Cookies-Einstellung in der &lt;Forms&gt; -Element, das einen von vier Werten annehmen kann:

- UseCookies - gibt an, dass cookiebasierte Authentifizierungstickets immer verwendet werden.
- UseUri - gibt an, dass Tickets von Cookie-basierte Authentifizierung nie verwendet werden.
- AutoErmittlung – Wenn das Geräteprofil Cookies, cookiebasierte Authentifizierungstickets nicht unterstützt wird, werden nicht verwendet. Wenn das Geräteprofil Cookies unterstützt, wird ein Überprüfungsmechanismus verwendet, um festzustellen, ob Cookies aktiviert sind.
- UseDeviceProfile – der Standardwert; verwendet cookiebasierte Authentifizierungstickets, nur dann, wenn das Geräteprofil Cookies unterstützt. Es wird keine Testmechanismus verwendet.

Die Einstellungen automatisch erkennen und UseDeviceProfile basieren auf einer *Geräteprofil* in der Feststellung, ob cookiebasierte oder ohne Cookies Authentifizierungstickets verwendet. ASP.NET verwaltet eine Datenbank von verschiedenen Geräten und ihre Funktionen, wie z. B., ob sie Cookies unterstützen, welche Version von JavaScript, die sie unterstützen und So weiter. Jedes Mal ein Gerät fordert eine Webseite von einem Webserver, er entlang sendet, einer *Benutzer-Agent* HTTP-Header, der den Gerätetyp identifiziert. ASP.NET entspricht automatisch die bereitgestellten Benutzer-Agent-Zeichenfolge mit dem entsprechenden Profil in der Datenbank angegeben.

> [!NOTE]
> Diese Datenbank von Gerätefunktionen befindet sich in einer Reihe von XML-Dateien, die folgen, der [Browserdefinitionsdatei-Schema](https://msdn.microsoft.com/library/ms228122.aspx). Die Standard-Gerät Profildateien befinden sich in % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Sie können auch benutzerdefinierte Dateien hinzufügen, Ihrer Anwendung App\_Ordner "Browser". Weitere Informationen finden Sie unter [so wird's gemacht: Erkennen der Browsertypen in ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Da die Standardeinstellung UseDeviceProfile ist, werden ohne Cookies Formularauthentifizierungstickets verwendet werden, wenn die Website von einem Gerät zugegriffen wird, dessen Profil gibt an, dass es keine Cookies unterstützt.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Das Authentifizierungsticket in der URL-Codierung

Cookies sind eine natürliche Mittel zum Einfügen von Informationen über den Browser in jede Anforderung an einer bestimmten Website, die standardmäßigen Einstellungen für die Formularauthentifizierung Cookies verwenden, wenn die Website besucht Gerät unterstützt wird. Wenn Cookies nicht unterstützt werden, muss eine alternative Möglichkeit für die Übergabe von des Authentifizierungstickets vom Client an den Server eingesetzt werden. Die Cookiedaten in der URL zu codieren ist eine häufig verwendete problemumgehung in Umgebungen mit ohne Cookies verwendet werden.

Die beste Möglichkeit, um festzustellen, wie solche Informationen in der URL eingebettet werden kann ist den Standort ohne Cookies Authentifizierungstickets zu erzwingen. Dies kann durch die ohne Cookies Konfigurationseinstellung auf UseUri erfolgen:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Nachdem Sie diese Änderung vorgenommen haben, besuchen Sie die Website über einen Browser ein. Wenn als anonymer Benutzer besuchen, sieht die URLs, genau wie zuvor. Beispielsweise zeigt Adressleiste des Browsers beim Besuch der Seite "default.aspx" die folgende URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Nach der Anmeldung wird das Formularauthentifizierungsticket jedoch in der URL eingebettet. Z. B. nach dem Zugriff auf die Anmeldeseite und Anmeldung als Sam, ich bin zurückgegeben, auf der Seite "default.aspx", aber die URL dieses Mal ist:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Das Formularauthentifizierungsticket wurde in der URL eingebettet. Die Zeichenfolge (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) stellt den Hexadezimal-codierten Informationen für das Formularauthentifizierungsticket aus, und sind die gleichen Daten, die in der Regel in einem Cookie gespeichert sind.

In der Reihenfolge für die Authentifizierung ohne Cookies Tickets arbeiten das System muss alle URLs auf der Seite enthält die Authentifizierungsticketdaten codieren, andernfalls das Authentifizierungsticket verloren klickt der Benutzer auf einen Link. Glücklicherweise wird dieses einbetten Logik automatisch ausgeführt. Klicken Sie zum demonstrieren dieser Funktionalität, öffnen Sie die Seite "default.aspx", und fügen Sie ein HyperLink-Steuerelement, das bzw. die die Eigenschaften für Text und NavigateUrl Testlink und SomePage.aspx, festlegen. Es spielt keine Rolle, dass es darüber gar keine Seite in Ihrem Projekt mit dem Namen SomePage.aspx.

Speichern Sie die Änderungen an "default.aspx", und besuchen Sie es dann über einen Browser. Melden Sie sich an den Standort so, dass das Formularauthentifizierungsticket in der URL eingebettet ist. Als Nächstes "default.aspx", klicken Sie auf den Link "testen"-Link. Was ist passiert? Wenn keine Seite, die mit dem Namen SomePage.aspx vorhanden ist, klicken Sie dann einen 404-Fehler ist aufgetreten, aber nicht wichtig, hier ist. Konzentrieren Sie stattdessen auf der Adressleiste in Ihrem Browser. Beachten Sie, dass sie in der URL das Formularauthentifizierungsticket enthält.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

Die SomePage.aspx URL, unter dem Link automatisch in eine URL konvertiert wurde, der das Authentifizierungsticket - enthalten schon nicht jeglichen Code zu schreiben! Das Authentifizierungsticket Formular automatisch eingebettet wird, in der URL für alle Links, die nicht mit beginnen `http://` oder `/`. Es spielt keine Rolle, wenn der Link angezeigt, in einem Aufruf von Response.Redirect, in ein HyperLink-Steuerelement oder in ein HTML-Ankerelement wird (d. h. `<a href="...">...</a>`). Solange die URL ist nicht, etwa `http://www.someserver.com/SomePage.aspx` oder `/SomePage.aspx`, das Formularauthentifizierungsticket eingebettet wird, für uns.

> [!NOTE]
> Ohne Cookies Formularauthentifizierungstickets befolgen die gleichen Timeout-Richtlinien als cookiebasierte Authentifizierungstickets. Allerdings sind die Authentifizierung ohne Cookies Tickets anfälliger für Angriffe wiedergeben, da das Authentifizierungsticket direkt in der URL eingebettet ist. Stellen Sie sich ein Benutzer, der eine Website aufruft, Anmeldung und fügt dann die URL in eine e-Mail an Kollegen. Der Kollegen auf diesen Link klickt, bevor die Ablaufzeit erreicht wird, werden sie als Benutzer angemeldet sein, die die e-Mail-Adresse gesendet.


## <a name="step-3-securing-the-authentication-ticket"></a>Schritt 3: Sichern das Authentifizierungsticket

Das Formularauthentifizierungsticket wird über das Netzwerk übertragen entweder in einem Cookie oder direkt in der URL eingebettet. Zusätzlich zu den Informationen zur Identität kann das Authentifizierungsticket auch Benutzerdaten enthalten, (wie in Schritt 4). Daher ist es wichtig, dass das Ticket für die Daten verschlüsselt werden vor neugierigen Blicken und (noch wichtiger ist), kann das Authentifizierungssystem Forms garantieren, dass das Ticket nicht manipuliert wurde.

Um die Privatsphäre des Tickets zu gewährleisten, kann die Forms-Authentifizierungssystem die Ticketdaten verschlüsseln. Fehler beim Verschlüsseln der Ticketdaten sendet potenziell vertraulichen Informationen über das Netzwerk im nur-Text.

Um ein Ticket für die Authentizität zu gewährleisten, muss das Authentifizierungssystem Forms *überprüfen* des Tickets. Überprüfung wird der Vorgang, um sicherzustellen, dass ein bestimmtes Datenelement nicht geändert wurde, und es über erfolgt eine  *[message Authentication Code (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Kurz gesagt, ist der MAC ein kleiner Teil der Informationen, die die Daten zu, die identifizieren (in diesem Fall das Ticket) überprüft werden muss. Wenn die von den MAC dargestellten Daten geändert werden, werden dann dem MAC und die Daten nicht übereinstimmen. Darüber hinaus ist es rechnerisch schwer Hackern den Zugriff auf die Daten ändern, sowohl generieren eigene MAC, um die geänderten Daten entsprechen.

Beim Erstellen (oder ändern) ist ein Ticket, das Forms-Authentifizierungssystem erstellt einen MAC und auf das Ticket für die Daten angefügt. Wenn eine nachfolgende Anforderung eingeht, vergleicht das Authentifizierungssystem Forms die Mac- und Ticket-Daten, um die Authentizität des Ticketdaten überprüft. Abbildung 3 zeigt diesen Workflow grafisch an.


[![Das Ticket für die Authentizität wird über einen MAC sichergestellt.](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Abbildung 03**: The Ticket Authentizität gewährleistet ist, über einen MAC ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Welche Sicherheitsmaßnahmen für das Authentifizierungsticket angewendet werden, hängt davon ab, der schutzeinstellung in der &lt;Forms&gt; Element. Die schutzeinstellung kann einen der folgenden drei Werte zugewiesen werden:

- Alle – das Ticket ist verschlüsselt und digital signiert (Standard).
- Verschlüsselung – nur Verschlüsselung angewendet wird – keine MAC wird generiert.
- None: das Ticket ist also weder verschlüsselt noch digital signiert.
- Überprüfung: einen MAC wird generiert, aber die Ticketdaten über das Netzwerk im nur-Text gesendet werden.

Microsoft empfiehlt dringend, mit der alle Einstellung.

### <a name="setting-the-validation-and-decryption-keys"></a>Festlegen der Validierung und Entschlüsselungsschlüssel

Die verschlüsselungs- und Hashalgorithmen, die von dem Authentifizierungssystem Forms verwendet werden, um zu verschlüsseln, und überprüfen das Authentifizierungsticket über anpassbare sind die [ &lt;MachineKey&gt; Element](https://msdn.microsoft.com/library/w8h3skw9.aspx) in "Web.config". Tabelle 2 zeigt die &lt;MachineKey&gt; Attribute des Elements und ihren möglichen Werten.

| **Attribut** | **Beschreibung** |
| --- | --- |
| Entschlüsselung | Gibt den Algorithmus an, für die Verschlüsselung verwendet. Dieses Attribut kann einen der folgenden vier Werten aufweisen: - Auto - die Standardeinstellung; Bestimmt den Algorithmus basierend auf der Länge des DecryptionKey-Attributs. -AES - verwendet die [erweiterte Verschlüsselung (Advanced Encryption Standard, AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) Algorithmus. -DES - verwendet die [Standard DES (Data Encryption)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) dieser Algorithmus rechnerisch schwache gilt und sollte nicht verwendet werden. -3DES - verwendet die [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) -Algorithmus, der durch Anwenden des DES-Algorithmus drei Mal funktioniert. |
| decryptionKey | Der geheime Schlüssel, die von den Verschlüsselungsalgorithmus verwendet. Dieser Wert muss entweder eine hexadezimale Zeichenfolge die entsprechende Länge (basierend auf den Wert in der Entschlüsselung), automatisch generieren oder einer der Werte mit der Endung IsolateApps. Hinzufügen von IsolateApps weist ASP.NET einen eindeutigen Wert für jede Anwendung zu verwenden. Der Standardwert ist AutoGenerate, IsolateApps. |
| Validierung | Gibt an, der für die Validierung verwendeten Algorithmus. Dieses Attribut kann einen der folgenden vier Werten aufweisen: - AES - verwendet den Advanced Encryption Standard (AES)-Algorithmus. -MD5 - verwendet die [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) Algorithmus. -SHA1 - verwendet die [SHA1](http://en.wikipedia.org/wiki/Sha1) Algorithmus (Standard). -3DES - wird den Triple DES-Algorithmus verwendet. |
| validationKey | Der geheime Schlüssel, die von den Validierungsalgorithmus verwendet wird. Dieser Wert muss entweder eine hexadezimale Zeichenfolge die entsprechende Länge (basierend auf den Wert in Überprüfung), automatisch generieren oder einer der Werte mit der Endung IsolateApps. Hinzufügen von IsolateApps weist ASP.NET einen eindeutigen Wert für jede Anwendung zu verwenden. Der Standardwert ist AutoGenerate, IsolateApps. |

**Tabelle 2**: die &lt;MachineKey&gt; Elementattribute

Eine ausführliche Beschreibung dieser Optionen Verschlüsselung und gültigkeitsprüfung, sowie die vor- und Nachteile der verschiedenen Algorithmen ist nicht Gegenstand dieses Tutorials. Für eine ausführliche sehen Sie sich diese Probleme, einschließlich Anleitungen, welche Algorithmen für Verschlüsselung und gültigkeitsprüfung zu verwenden, welche Schlüssellängen und zu verwenden, und wie diese Schlüssel generiert haben, finden am besten *Professional ASP.NET 2.0 Security, Mitgliedschafts- und Rollenverwaltung* .

Klicken Sie in der Standardeinstellung die Schlüssel für die Verschlüsselung und gültigkeitsprüfung für jede Anwendung automatisch generiert werden, und diese Schlüssel werden in die lokale Sicherheitsinstanz (LSA) gespeichert. Kurz gesagt, garantieren die Standardeinstellungen für eindeutige Schlüssel für eine Web-Server-von-Web-Server und jede Anwendung durch. Dieses Standardverhalten funktioniert daher nicht für die zwei folgenden Szenarien:

- **Webfarmen** – in einem [Webfarm](http://en.wikipedia.org/wiki/Web_farm) Szenario einer Webanwendung auf mehreren Webservern für Zwecke der Skalierbarkeit und Redundanz gehostet wird. Jede eingehende Anforderung wird gesendet, auf einem Server in der Farm, was bedeutet, dass während der Lebensdauer der Sitzung eines Benutzers, unterschiedliche-Servern verwendet werden können, um seine verschiedenen Anforderungen zu verarbeiten. Daher jeden Server muss die gleichen Schlüssel für Verschlüsselung und gültigkeitsprüfung verwenden, sodass das Formularauthentifizierungsticket erstellt, verschlüsselt und überprüft auf einem Server entschlüsselt und überprüft Sie auf einem anderen Server in der Farm.
- **Plattformübergreifende, Anwendungsfreigabe Ticket** – ein einziger Webserver kann mehrere ASP.NET-Anwendungen hosten. Wenn Sie für diese verschiedenen Anwendungen ein einzelnes Formularauthentifizierungsticket gemeinsam nutzen müssen, ist es zwingend erforderlich, dass ihre Verschlüsselung und gültigkeitsprüfung Schlüssel übereinstimmen.

Bei der Arbeit in einer Webfarm festlegen oder das Freigeben von Authentifizierungstickets für Anwendungen auf demselben Server müssen Sie konfigurieren die &lt;MachineKey&gt; Element in der betroffenen Anwendungen, damit ihre DecryptionKey und ValidationKey Werte übereinstimmen.

Während keines der oben genannten Szenarien für unsere beispielanwendung gilt, können wir ValidationKey-Werte und explizite DecryptionKey angeben und definieren die Algorithmen, die verwendet werden. Hinzufügen einer &lt;MachineKey&gt; auf die Datei "Web.config" festlegen:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Weitere Informationen finden Sie [so wird's gemacht: Konfigurieren von MachineKey in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Die Werte für DecryptionKey und ValidationKey entnommen wurden [Steve Gibson](http://www.grc.com/stevegibson.htm)des [perfekte Kennwörter Webseite](https://www.grc.com/passwords.htm), generiert 64 zufällige Hexadezimalzeichen auf jeder Seite finden Sie unter. Um die Wahrscheinlichkeit, dass dieser Schlüssel, sodass ihren Weg in Ihre Produktionsanwendungen zu verringern, sollten Sie sich, die obigen Schlüssel mit zufällig generierten Werten, die auf der perfekte Kennwörter zu ersetzen.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Schritt 4: Zusätzliche Benutzerdaten in das Ticket speichern

Viele Webanwendungen Anzeigen von Informationen zu, oder der Seite anzeigen als Grundlage für den derzeit angemeldeten Benutzer. Beispielsweise kann eine Webseite angezeigt, den Benutzernamen und das Datum, die, das Sie zuletzt in der oberen Ecke von jeder Seite angemeldet haben, auf. Das Formularauthentifizierungsticket gespeichert, den derzeit angemeldeten Benutzernamen des Benutzers muss, wenn alle anderen Informationen benötigt wird, die Seite aber im Benutzerspeicher – in der Regel eine Datenbank – suchen Sie die Informationen, die nicht in das Authentifizierungsticket gespeichert.

Mit nur wenigen Codezeilen können wir zusätzliche Informationen in das Formularauthentifizierungsticket speichern. Solche Daten ausgedrückt werden können, über die [FormsAuthenticationTicket-Klasse](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)des [UserData-Eigenschaft](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Dies ist nützlich zum Ablegen kleine Mengen von Informationen über den Benutzer zu platzieren, die im Allgemeinen benötigt wird. In der Eigenschaft ist als Bestandteil des Authentifizierungscookies Ticket und, wie die anderen Felder Ticket verschlüsselt und überprüft UserData angegebene Wert basierend auf der Forms-Authentifizierung Konfiguration des Systems. Standardmäßig ist die UserData eine leere Zeichenfolge.

Um die Benutzerdaten in das Authentifizierungsticket speichern zu können, müssen wir ein paar Codezeilen in die Anmeldeseite zu schreiben, die die benutzerspezifische Informationen erfasst und speichert sie in das Ticket. Da UserData eine Eigenschaft vom Typzeichenfolge ist, müssen die darin gespeicherten Daten ordnungsgemäß als Zeichenfolge serialisiert werden. Angenommen Sie, dass unsere Speicher des Benutzers, das Geburtsdatum des Benutzers und den Namen des Arbeitgebers enthalten, und wir diese zwei Eigenschaftswerte in das Authentifizierungsticket speichern möchten. Wir konnten diese Werte in eine Zeichenfolge serialisiert werden durch die Verkettung des Benutzers Datum Geburtsdatum der Zeichenfolge durch einen senkrechten Strich (|), gefolgt vom Arbeitgeber-Namen. Für einen Benutzer, der auf 15 August 1974 geborenen, das für Northwind Traders ist, wir würden der UserData-Eigenschaft die Zeichenfolge zuzuweisen: 1974-08-15 | Northwind Traders.

Wenn wir die Daten in das Ticket zugreifen müssen, können wir dazu die aktuelle Anforderung FormsAuthenticationTicket abrufen und Deserialisieren der UserData-Eigenschaft. Bei das Datum der Geburtsdatum und Arbeitgeber Beispielname würden wir die UserData-Zeichenfolge in beiden Teilzeichenfolgen, die auf Grundlage des Trennzeichens (|) aufgeteilt.


[![Zusätzliche Informationen kann in das Authentifizierungsticket gespeichert werden](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Abbildung 04**: zusätzliche Benutzer Daten gespeichert werden können in das Authentifizierungsticket ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Schreiben von Informationen in UserData

Leider ist es nicht so einfach, wie zu erwarten wäre, die ein Formularauthentifizierungsticket benutzerspezifische Informationen hinzugefügt. Die Eigenschaft "UserData" der FormsAuthenticationTicket-Klasse ist schreibgeschützt und kann nur über den Klassenkonstruktor FormsAuthenticationTicket angegeben werden. Wenn Sie die UserData-Eigenschaft im Konstruktor angeben, wir müssen auch angeben, das Ticket ist, andere Werte: den Benutzernamen, das Datum, Ablauf und So weiter. Wenn wir die Anmeldeseite im vorherigen Tutorial erstellt haben, wurde dies alle uns durch die FormsAuthentication-Klasse behandelt. Wenn UserData FormsAuthenticationTicket hinzufügen, müssen wir Code schreiben, um den Großteil der Funktionen, die bereits von der FormsAuthentication-Klasse bereitgestellte replizieren.

Sehen wir uns den erforderlichen Code für die Arbeit mit UserData durch Aktualisieren der Seite "Login.aspx", um zusätzliche Informationen über den Benutzer an das Authentifizierungsticket aufzuzeichnen. Nehmen Sie, dass unsere Benutzerspeicher enthält Informationen über das Unternehmen, die für die der Benutzer arbeitet und deren Position und, dass wir diese Informationen in das Authentifizierungsticket erfassen möchten. Aktualisieren Sie der Seite "Login.aspx" LoginButton Click-Ereignishandler, sodass der Code wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Gehen wir Codezeile zu einem Zeitpunkt. Die Methode beginnt, durch die Definition von vier Zeichenfolgenarrays: Benutzer, Kennwörter, CompanyName und TitleAtCompany. Diese Arrays enthalten den Benutzernamen, Kennwörter, Firmennamen und Titel für die Benutzerkonten im System, der es sind drei: Scott Jisun und Sam. In einer echten Anwendung würden diese Werte aus dem Benutzerspeicher, nicht hartcodiert im Quellcode der Seite abgefragt werden.

Im vorherigen Tutorial Wenn die angegebenen Anmeldeinformationen gültig sind aufgerufen wir einfach FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), die die folgenden Schritte ausgeführt:

1. Erstellt die Forms-Authentifizierungsticket
2. Das Ticket in der entsprechenden Speicher geschrieben. Für Cookies basierende Authentifizierungstickets wird die Auflistung der Cookies im Browser auf die verwendet. für die Authentifizierung ohne Cookies Tickets werden die Ticketdaten in der URL serialisiert.
3. Den Benutzer umgeleitet auf die entsprechende Seite

Diese Schritte werden im obigen Code repliziert. Zunächst wird die Zeichenfolge, die wir schließlich in der UserData-Eigenschaft gespeichert wird gebildet, durch die Kombination der Unternehmensname und den vollständigen Titel, begrenzen die beiden Werte mit einem senkrechten Strich (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Als Nächstes die FormsAuthentication.GetAuthCookie Methode aufgerufen wird, der das Authentifizierungsticket erstellt, verschlüsselt gemäß den Konfigurationseinstellungen überprüft und platziert sie in einem HttpCookie-Objekt.

HttpCookie AuthCookie = FormsAuthentication.GetAuthCookie (UserName.Text, RememberMe.Checked);

Um mit der in das Cookie eingebettet FormAuthenticationTicket arbeiten, müssen wir die FormAuthentication-Klasse aufrufen, [entschlüsseln Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), und übergeben Sie den Cookiewert.

FormsAuthenticationTicket Ticket = FormsAuthentication.Decrypt(authCookie.Value);

Anschließend erstellen wir eine *neue* FormsAuthenticationTicket-Instanz auf Grundlage der vorhandenen FormsAuthenticationTicket-Werte. Dieses neue Ticket enthält jedoch die benutzerspezifische Informationen (UserDataString).

FormsAuthenticationTicket NewTicket = neue FormsAuthenticationTicket (Ticket. Ticket-Version. Name der Ticket. IssueDate, ticket. Ablaufzeit Ticket. IsPersistent UserDataString);

Wir dann verschlüsseln (und überprüfen) die neue FormsAuthenticationTicket-Instanz durch Aufrufen der [verschlüsseln Methode](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx), und fügen Sie diese Daten verschlüsselten (und überprüften) zurück in AuthCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Klicken Sie abschließend AuthCookie der Response.Cookies-Auflistung hinzugefügt wird, und die GetRedirectUrl-Methode wird aufgerufen, um zu bestimmen, die entsprechende Seite, um den Benutzer zu senden.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

All dieser Code ist erforderlich, da die UserData-Eigenschaft ist schreibgeschützt, und die FormsAuthentication-Klasse stellt Methoden zum Angeben von UserData-Informationen in den Methoden GetAuthCookie SetAuthCookie oder RedirectFromLoginPage nicht bereit.

> [!NOTE]
> Der Code, den wir gerade untersucht speichert benutzerspezifische Informationen in einem Ticket Cookies basierende Authentifizierung. Die Klassen, die zum Serialisieren von Formularauthentifizierungstickets an die URL sind für das .NET Framework intern. Lange Rede kurzer, können nicht Sie Benutzerdaten in einer ohne Cookies Formularauthentifizierungsticket speichern.


### <a name="accessing-the-userdata-information"></a>Zugreifen auf die UserData-Informationen

An diesem Punkt Unternehmensname und den Titel jedes Benutzers in das Formularauthentifizierungsticket UserData Eigenschaft gespeichert, wenn er sich anmeldet. Diese Informationen kann über das Authentifizierungsticket auf einer beliebigen Seite zugegriffen werden, ohne dass eine Reise zu den Speicher des Benutzers. Aktualisieren Sie zur Veranschaulichung, wie diese Informationen aus der UserData-Eigenschaft abgerufen werden kann, lassen Sie uns "default.aspx", sodass eine Begrüßung enthält, nicht nur den Namen des Benutzers, aber auch das Unternehmen, die für die Zusammenarbeit und deren Position.

"Default.aspx" wird derzeit ein AuthenticatedMessagePanel-Bereich mit einem Bezeichnungssteuerelement, das mit dem Namen WelcomeBackMessage enthält. In diesem Bereich wird nur für authentifizierte Benutzer angezeigt. Aktualisieren Sie den Code in die Seite "default.aspx" des\_Ereignishandler zum Laden, damit sie wie folgt aussieht:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Wenn Request.IsAuthenticated "true ist", dann die WelcomeBackMessages Text-Eigenschaft zunächst auf Willkommen zurück, *Benutzername*. Klicken Sie dann wird die User.Identity-Eigenschaft auf ein FormsIdentity-Objekt umgewandelt, damit wir den zugrunde liegenden FormsAuthenticationTicket zugreifen können. Sobald wir FormsAuthenticationTicket haben, Deserialisieren wir die UserData-Eigenschaft in den Firmennamen und den Titel an. Dies erfolgt durch Teilen der Zeichenfolge in einen senkrechten Strich. Der Firmenname und Titel werden dann in die WelcomeBackMessage Bezeichnung angezeigt.

Abbildung 5 zeigt einen Screenshot der diese Anzeige in Aktion. Anmeldung als Scott in einer Meldung Willkommen zurück, die Unternehmen und Titel von Scott enthält.


[![Der derzeit angemeldeten Benutzer des Unternehmens und Titel werden angezeigt.](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Abbildung 05**: der derzeit angemeldeten Benutzer des Unternehmens und Titel angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Das Authentifizierungsticket UserData-Eigenschaft dient als Cache für den Speicher des Benutzers. Z. B. einen Cache muss sie aktualisiert werden, wenn die zugrunde liegenden Daten geändert werden. Ist eine Webseite, in der Benutzer ihre Profile aktualisieren können, müssen z. B. die Felder, die zwischengespeichert werden, in der UserData-Eigenschaft aktualisiert werden, die vom Benutzer vorgenommene Änderungen widerzuspiegeln.


## <a name="step-5-using-a-custom-principal"></a>Schritt 5: Verwenden eines benutzerdefinierten Prinzipals

Versucht, jede eingehende Anforderung die FormsAuthenticationModule zum Authentifizieren des Benutzers. Wenn ein nicht abgelaufene Authentifizierungsticket vorhanden ist, weist der FormsAuthenticationModule die HttpContext.User-Eigenschaft auf ein neues GenericPrincipal-Objekt. Diese GenericPrincipal-Objekt verfügt über eine Identität vom Typ FormsIdentity, die einen Verweis auf das Formularauthentifizierungsticket enthält. GenericPrincipal-Klasse enthält, der bare Mindestfunktionalität, die von einer Klasse, die IPrincipal implementiert benötigt werden – sie muss lediglich eine Identity-Eigenschaft und eine "IsInRole"-Methode.

Das Prinzipalobjekt hat zwei Aufgaben: welche Rollen gibt an, der Benutzer angehört, und die Identitätsinformationen zu bieten. Dies erfolgt über die IPrincipal-Schnittstelle "IsInRole" (*RoleName*)-Methode und die Identity-Eigenschaft bzw. GenericPrincipal-Klasse ermöglicht ein Zeichenfolgenarray mit Rollennamen, aus denen über seinen Konstruktor angegeben werden. die "IsInRole" (*RoleName*)-Methode lediglich überprüft, ob das übergebene in *RoleName* innerhalb der Zeichenfolgen-Array vorhanden ist. Wenn die FormsAuthenticationModule GenericPrincipal erstellt, übergibt sie an GenericPrincipals-Konstruktor in ein leeres Zeichenfolgenarray. Daher wird jeder Aufruf von "IsInRole" immer "false" zurückgegeben.

GenericPrincipal-Klasse erfüllt die Anforderungen für die meisten Szenarien des Forms-Authentifizierung, in denen Rollen nicht verwendet werden. Für diese Fälle, in denen die Standardbehandlung für die Rolle nicht ausreichend ist, oder wenn Sie ein benutzerdefiniertes IIdentity-Objekt mit dem Benutzer zuordnen möchten, erstellen Sie ein benutzerdefiniertes IPrincipal-Objekt während des Workflows für die Authentifizierung und die HttpContext.User-Eigenschaft zuweisen.

> [!NOTE]
> Wie wir in Zukunft Tutorials, sehen beim ASP. NET Rollen-Framework ist aktiviert. er erstellt ein benutzerdefiniertes principal-Objekt des Typs [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) und überschreibt die Forms-Authentifizierung erstellten GenericPrincipal-Objekt. Dies wird zum Anpassen des Prinzipals "IsInRole"-Methode für die Kommunikation mit dem Rollen-Framework-API.


Da wir nicht selbst mit Rollen noch befasst haben, wäre der einzige Grund für das Erstellen von einem benutzerdefinierten Prinzipal an diesem Punkt müsste eine benutzerdefinierte IIdentity-Objekte, die dem Prinzipal verknüpfen. In Schritt 4 erläutert Speichern von zusätzlichen Benutzerinformationen in das Authentifizierungsticket UserData-Eigenschaft, insbesondere bei den Namen des Benutzers Unternehmen und deren Position. Die UserData-Informationen ist jedoch nur Zugriff über das Authentifizierungsticket und nur dann als Zeichenfolge serialisiert, was bedeutet, dass jedes Mal, wenn wir die Benutzerinformationen im Ticket gespeichert werden soll, wir beim Analysieren der UserData-Eigenschaft müssen.

Wir verbessern die entwicklererfahrung durch Erstellen einer Klasse, die IIdentity implementiert und CompanyName "und" Title "enthält. Auf diese Weise ein Entwickler kann Unternehmensname des aktuell angemeldeten Benutzers zugreifen und Titel direkt über die Eigenschaften "CompanyName" und "Title" ohne erforderlich sind, wissen, wie Sie die UserData-Eigenschaft zu analysieren.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Erstellen von benutzerdefinierten Identitäts- und Prinzipalklassen

In diesem Tutorial erstellen Sie lassen Sie uns die benutzerdefinierten Objekte für Haupt- und Identitätsobjekte, in der App\_Ordner "Code". Hinzufügen einer App zunächst\_Ordner zu Ihrem Projekt Code – mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, wählen Sie die Option ASP.NET-Ordner hinzufügen, und wählen Sie eine App\_Code. Die App\_Ordner "Code" ist ein spezieller ASP.NET-Ordner, der enthält Dateien spezifisch für die Website.

> [!NOTE]
> Die App\_Ordner "Code" sollte nur verwendet werden, wenn Sie Ihrem Projekt über das Website-Modell zu verwalten. Bei Verwendung der [Webanwendungsprojekt-Modell](https://msdn.microsoft.com/asp.net/Aa336618.aspx), erstellen Sie einen standard-Ordner, und fügen Sie der Klassen hinzu, die. Sie können z. B. Fügen Sie einen neuen Ordner namens Klassen hinzu und legen Ihren Code dort.


Fügen Sie zwei neue Klassendateien für die App\_Ordner "Code", eine benannte CustomIdentity.cs und eine mit dem Namen CustomPrincipal.cs.


[![Hinzufügen von der CustomIdentity und CustomPrincipal-Klassen zu Ihrem Projekt](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Abbildung 06**: Hinzufügen von der CustomIdentity und CustomPrincipal-Klassen in Ihr Projekt ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


Der CustomIdentity-Klasse ist verantwortlich für die Implementierung der IIdentity-Schnittstelle, die die AuthenticationType IsAuthenticated und Name-Eigenschaften definiert. Zusätzlich zu diesen erforderlichen Eigenschaften interessieren uns die zugrunde liegenden Formularauthentifizierungsticket als auch Eigenschaften für Unternehmensname und den Titel des Benutzers verfügbar zu machen. Geben Sie den folgenden Code aus, in der CustomIdentity-Klasse.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Beachten Sie, dass die Klasse eine FormsAuthenticationTicket-Membervariable enthält (\_Ticket) und die diesem Ticketinformationen muss durch den Konstruktor bereitgestellt werden. Dieses Ticketdaten werden in das Zurückgeben der Identität des Namens verwendet. Die UserData-Eigenschaft wird analysiert, um die Werte für die CompanyName "und" Title "zurückzugeben.

Erstellen Sie als Nächstes die CustomPrincipal-Klasse. Da wir nicht mit Rollen an diesem Punkt sind, akzeptiert der Konstruktor der CustomPrincipal-Klasse nur ein CustomIdentity-Objekt; die "IsInRole"-Methode gibt immer "false" zurück.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Sicherheitskontext für die eingehende Anforderung ein CustomPrincipal-Objekt zugewiesen

Wir haben jetzt eine Klasse, die IIdentity-Standardspezifikation CompanyName "und" Title "Einbeziehung erweitert, als auch eine benutzerdefinierte Prinzipalklasse, die die benutzerdefinierte Identität verwendet. Wir können mit Schritt in der ASP.NET-Pipeline und Sicherheitskontext für die eingehende Anforderung unsere benutzerdefinierte Prinzipalobjekt zuweisen.

Die ASP.NET-Pipeline eine eingehende Anforderung akzeptiert und verarbeitet diese durch eine Reihe von Schritten. Bei jedem Schritt wird ein bestimmtes Ereignis ausgelöst, wodurch Entwickler profitieren von der ASP.NET-Pipeline und ändern Sie die Anforderung an bestimmten Punkten im Lebenszyklus. Die FormsAuthenticationModule wartet z. B. ASP.NET zum Auslösen der [AuthenticateRequest-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), an diesem Punkt die eingehende Anforderung für ein Authentifizierungsticket überprüft. Wenn ein Authentifizierungsticket gefunden wird, wird ein GenericPrincipal-Objekt erstellt und die HttpContext.User-Eigenschaft zugewiesen.

Nach dem das AuthenticateRequest-Ereignis, löst die ASP.NET-Pipeline die [PostAuthenticateRequest-Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), ist, in dem wir das GenericPrincipal-Objekt, das von der FormsAuthenticationModule mit einer Instanz von erstellt ersetzen können unsere CustomPrincipal-Objekt. Abbildung 7 zeigt diesen Workflow.


[![GenericPrincipal wird durch eine CustomPrincipal im Ereignis PostAuthenticationRequest ersetzt.](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Abbildung 07**: das GenericPrincipal wird durch eine CustomPrincipal im Ereignis PostAuthenticationRequest ersetzt ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Um Code als Reaktion auf eine Pipelineereignis von ASP.NET ausführen, können wir entweder den passenden Ereignishandler in "Global.asax" oder erstellen eigene HTTP-Modul. In diesem Tutorial erstellen Sie den Ereignishandler wir in "Global.asax". Starten Sie durch das Hinzufügen von "Global.asax" zu Ihrer Website. Mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und fügen Sie ein Element vom Typ globale Anwendungsklasse mit dem Namen "Global.asax" hinzu.


[![Fügen Sie eine Datei "Global.asax" zu Ihrer Website](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Abbildung 08**: Hinzufügen einer Global.asax-Datei auf Ihrer Website ([klicken Sie, um das Bild in voller Größe anzeigen](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Die Standardvorlage für die "Global.asax" enthält die Ereignishandler für eine Reihe von ASP.NET-Pipelineereignisse, einschließlich der Start, Ende und [Fehlerereignis](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), u. a. Diese Ereignishandler entfernt werden, da wir nicht für diese Anwendung benötigt werden können. Das Ereignis, dem wir interessiert sind, ist PostAuthenticateRequest. Aktualisieren Sie die Datei "Global.asax" aus, damit dessen Markup sieht etwa wie folgt aus:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Die Anwendung\_OnPostAuthenticateRequest-Methode führt jedes Mal die ASP.NET-Laufzeit löst das PostAuthenticateRequest-Ereignis, das einmal für jede eingehende Seitenanforderung durchgeführt. Der Ereignishandler wird gestartet, indem überprüft wird, wenn der Benutzer authentifiziert wird und über Formularauthentifizierung authentifiziert wurde. Wenn dies der Fall ist, wird ein neues CustomIdentity-Objekt erstellt und die aktuelle Anforderung Authentifizierungsticket in seinem Konstruktor übergeben. Danach wird ein CustomPrincipal-Objekt erstellt und übergeben den gerade erstellten CustomIdentity-Objekt in seinem Konstruktor. Schließlich wird der Sicherheitskontext für die aktuelle Anforderung auf das neu erstellte CustomPrincipal-Objekt zugewiesen.

Beachten Sie, dass der letzte Schritt - Sicherheitskontext der Anforderung das CustomPrincipal-Objekt zugeordnet: den Prinzipal auf zwei Eigenschaften zugewiesen: HttpContext.User und Thread.CurrentPrincipal. Diese beiden Zuweisungen sind erforderlich, aufgrund der Art und Weise, die Sicherheitskontexten, die in ASP.NET verarbeitet werden. .NET Framework ordnet jeden ausgeführten Thread einen Sicherheitskontext; Diese Informationen sind verfügbar, als ein IPrincipal-Objekt, durch die [Threadobjekt](https://msdn.microsoft.com/library/system.threading.thread.aspx)des [CurrentPrincipal-Eigenschaft](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Was ist eine verwirrend ist, dass ASP.NET einen eigenen Sicherheitskontextinformationen (HttpContext.User).

In bestimmten Szenarien wird die Eigenschaft "Thread.CurrentPrincipal" untersucht, wenn den Sicherheitskontext bestimmt; in anderen Szenarien wird die HttpContext.User verwendet. Z. B. es wurden Sicherheitsfunktionen in .NET, mit denen Entwickler deklarativ angeben, welche Benutzer oder Rollen können eine Klasse instanziieren oder bestimmte Methoden aufrufen (finden Sie unter [Autorisierungsregeln hinzufügen, Business und Daten mithilfe von Ebenen PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Im Hintergrund bestimmen diese deklarative Verfahren den Sicherheitskontext, über die Eigenschaft "Thread.CurrentPrincipal".

In anderen Szenarien wird die HttpContext.User-Eigenschaft verwendet. Zum Beispiel im vorherigen Tutorial haben wir verwendet diese Eigenschaft den derzeit angemeldeten Benutzernamen des Benutzers angezeigt. Klicken Sie dann, es ist zwingend erforderlich, dass die Sicherheitskontextinformationen in den Eigenschaften Thread.CurrentPrincipal und HttpContext.User übereinstimmen.

Die ASP.NET-Laufzeit synchronisiert diese Eigenschaftswerte automatisch für uns. Aber diese Synchronisierung tritt ein, nachdem das AuthenticateRequest-Ereignis, aber *vor* das PostAuthenticateRequest-Ereignis. Daher beim Hinzufügen von eines benutzerdefinierten Prinzipals im PostAuthenticateRequest-Ereignis müssen wir bestimmte sonst Thread.CurrentPrincipal der Thread.CurrentPrincipal manuell zugewiesen werden, und HttpContext.User wird nicht mehr synchron. Finden Sie unter [Context.User Visual Studio. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) für eine ausführlichere Erläuterung zu diesem Problem.

### <a name="accessing-the-companyname-and-title-properties"></a>Zugreifen auf die CompanyName und Eigenschaften

Jedes Mal, wenn eine Anforderung eingeht und wird an die ASP.NET-Engine, die Anwendung gesendet\_OnPostAuthenticateRequest-Ereignishandler in "Global.asax" wird ausgelöst. Wenn die Anforderung von der FormsAuthenticationModule erfolgreich authentifiziert wurde, erstellt der Ereignishandler ein neues CustomPrincipal-Objekt mit einem CustomIdentity-Objekt, das basierend auf das Formularauthentifizierungsticket. Mit dieser Logik vorhanden ist erfolgt der Zugriff auf Informationen zu den Firmennamen und den Titel des aktuell angemeldeten Benutzers unglaublich einfach.

Kehren Sie zur Seite\_Load-Ereignishandler in "default.aspx", in dem in Schritt 4 wir Code zum Abrufen des Authentifizierungstickets Formular und analysieren die UserData-Eigenschaft schrieb, um Unternehmensname und den Titel des Benutzers anzuzeigen. Mit den CustomPrincipal und CustomIdentity-Objekten in jetzt verwenden ist es nicht erforderlich, die Werte aus UserData-Eigenschaft des Tickets zu analysieren. Rufen Sie stattdessen einfach einen Verweis auf das CustomIdentity-Objekt und verwenden Sie ihre Eigenschaften CompanyName "und" Title:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial untersucht es die Forms Authentication Standardeinstellungen des Systems über "Web.config" anpassen. Erläutert, wie das Authentifizierungsticket Ablauf erfolgt und wie die Verschlüsselung und gültigkeitsprüfung Schutzvorrichtungen verwendet werden, um das Ticket vor Überprüfung und Änderung zu schützen. Abschließend wird erläutert, mit das Authentifizierungsticket UserData-Eigenschaft zum Speichern von zusätzlichen Benutzerinformationen in das Ticket selbst und wie Sie benutzerdefinierte Haupt- und Identitätsobjekte-Objekte verwenden, um diese Informationen auf Entwickler Benutzerfreundlicher Weise verfügbar zu machen.

Dieses Lernprogramm schließt unserer Untersuchung der Formularauthentifizierung in ASP.NET. Im nächste Tutorial wird die unserem Weg in das mitgliedschaftsframework gestartet.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Analyse der Formularauthentifizierung](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Erläutert: Formularauthentifizierung in ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Gewusst wie: Schützen der Formularauthentifizierung in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 Security, Mitgliedschafts- und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Sichern die Steuerelemente für die Anmeldung](https://msdn.microsoft.com/library/ms178346.aspx)
- [Die &lt;Authentifizierung&gt; Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Die &lt;Forms&gt; -Element für &lt;Authentifizierung&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Die &lt;MachineKey&gt; Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Grundlegendes zu den Formularauthentifizierungsticket und Cookies](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>In den Themen in diesem Tutorial-Videotraining

- [Gewusst wie: Ändern der Eigenschaften der Formularauthentifizierung](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Wie Sie die Einrichtung und Verwendung Authentifizierung ohne Cookies in einer ASP.NET-Anwendung](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Verschieben der ASP-Formularanmeldung](../../../videos/authentication/asp-forms-login-relocation.md)
- [Benutzerdefinierte Schlüsselkonfiguration für die Formularanmeldung](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Hinzufügen von benutzerdefinierten Daten zur Authentifizierungsmethode](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Verwenden von benutzerdefinierten Prinzipalobjekten](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Alicja Maziarz. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Zurück](an-overview-of-forms-authentication-cs.md)
> [Weiter](security-basics-and-asp-net-support-vb.md)
