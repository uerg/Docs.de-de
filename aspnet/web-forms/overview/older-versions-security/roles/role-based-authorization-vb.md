---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Rollenbasierte Autorisierung (VB) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm beginnt mit einem Blick auf die beschreibt, wie das Framework Rollen Rollen eines Benutzers mit seinem Sicherheitskontext verknüpft. Klicken Sie dann untersucht erläutert, wie rollenbasierte URL angewendet..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 973e5705fc36b13e5e6ec861dd2ca6adfc0f50fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="role-based-authorization-vb"></a>Rollenbasierte Autorisierung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> In diesem Lernprogramm beginnt mit einem Blick auf die beschreibt, wie das Framework Rollen Rollen eines Benutzers mit seinem Sicherheitskontext verknüpft. Gewusst wie: Anwenden von URL-Autorisierungsregeln mit der rollenbasierten untersucht. Im Anschluss an, dass wir betrachten verwenden deklarative und eine programmgesteuerte Möglichkeit zum Ändern der angezeigten Daten und die Funktionalität, die von einer ASP.NET-Seite angeboten wird.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm wurde erläutert, wie URL-Autorisierung zu verwenden, um anzugeben, welche Benutzer eine bestimmte Gruppe von Seiten besuchen können. Mit nur wenig von Markup im `Web.config`, wir konnte ASP.NET anzuweisen, nur durch authentifizierte Benutzer besuchen eine Seite zu ermöglichen. Oder es konnte vorgibt, dass nur Benutzer Tito und Bob zugelassen wurden anzugeben, dass alle authentifizierten Benutzer mit Ausnahme von Sam zulässig wären.

Zusätzlich zu den URL-Autorisierung haben wir auch deklarative und programmgesteuerte Verfahren zum Steuern der angezeigten Daten und die Funktionalität von einer Seite basierend auf dem Benutzer Zugriff auf. Insbesondere haben wir eine Seite, die den Inhalt des aktuellen Verzeichnisses aufgeführt. Jeder Benutzer kann auf dieser Seite finden Sie auf, aber nur durch authentifizierte Benutzer können die Dateien Inhalt anzeigen, und nur Tito konnte die Dateien gelöscht.

Anwenden von Autorisierungsregeln pro vom Benutzer kann in einem vertiefen des Verlusts anwachsen. Ein sind besser verwaltbar Ansatz ist, verwenden Sie rollenbasierte Autorisierung. Die gute Nachricht ist, dass die Tools zur Verfügung zum Anwenden von Autorisierungsregeln gleichermaßen funktionieren gut mit Rollen, wie für Benutzerkonten. URL-Autorisierungsregeln können Rollen nicht für Benutzer angeben. LoginView-Steuerelement, das andere Ausgabe für authentifizierte und anonyme Benutzer rendert, kann unterschiedliche Inhalte auf Basis des angemeldeten Benutzers der Rollen zeigen konfiguriert werden. Und die Rollen-API umfasst Methoden zum Bestimmen der Rollen des angemeldeten Benutzers.

In diesem Lernprogramm beginnt mit einem Blick auf die beschreibt, wie das Framework Rollen Rollen eines Benutzers mit seinem Sicherheitskontext verknüpft. Gewusst wie: Anwenden von URL-Autorisierungsregeln mit der rollenbasierten untersucht. Im Anschluss an, dass wir betrachten verwenden deklarative und eine programmgesteuerte Möglichkeit zum Ändern der angezeigten Daten und die Funktionalität, die von einer ASP.NET-Seite angeboten wird. Fangen wir an!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Grundlegendes zu wie Rollen zugeordnet sind Sicherheitskontext eines Benutzers

Wenn eine Anforderung die ASP.NET-Pipeline erreicht ist er einen Sicherheitskontext, einschließlich Informationen zur Kennzeichnung des anforderers zugeordnet. Wenn Sie die Formularauthentifizierung verwenden, wird ein Authentifizierungsticket als ein Identitätstoken verwendet. Wie in erläutert die <a id="_msoanchor_2"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) und <a id="_msoanchor_3"> </a> [ *Formulare Konfiguration der Authentifizierung und erweiterte Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Lernprogramme, die `FormsAuthenticationModule` ist zuständig für das Ermitteln der Identität des anforderers, was aber während der Fall ist die [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.authenticaterequest.aspx).

Wenn Sie einen gültigen, nicht abgelaufenen Authentifizierungsticket gefunden wird, die `FormsAuthenticationModule` decodiert, um die Identität des Antragstellers zu ermitteln. Er erstellt ein neues `GenericPrincipal` Objekts und weist Sie diese Option, um die `HttpContext.User` Objekt. Der Zweck eines Prinzipals, wie `GenericPrincipal`, besteht im Identifizieren der authentifizierter Benutzername und was sie gehören zu Rollen. Diesem Zweck wird durch die Tatsache, der alle Hauptobjekte haben offensichtlich ein `Identity` Eigenschaft und eine `IsInRole(roleName)` Methode. Die `FormsAuthenticationModule`, ist jedoch nicht interessiert, Informationen aufzeichnen und die `GenericPrincipal` Objekt erstellt, gibt keine Rollen.

Wenn das Framework Rollen aktiviert ist, die [ `RoleManagerModule` ](https://msdn.microsoft.com/en-us/library/system.web.security.rolemanagermodule.aspx) HTTP-Modul Schritte nach der `FormsAuthenticationModule` und identifiziert den authentifizierten Benutzer Rollen während der [ `PostAuthenticateRequest` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.postauthenticaterequest.aspx), welche wird ausgelöst, nachdem die `AuthenticateRequest` Ereignis. Ist die Anforderung von einem authentifizierten Benutzer der `RoleManagerModule` überschreibt die `GenericPrincipal` Objekt erstellt, indem die `FormsAuthenticationModule` und ersetzt es mit einer [ `RolePrincipal` Objekt](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.aspx). Die `RolePrincipal` Klasse verwendet die Rollen-API, um zu bestimmen, welche Rollen der Benutzer angehört.

Abbildung 1 zeigt den ASP.NET Pipeline-Workflow aus, bei der Formularauthentifizierung und die Rollen-Framework verwenden. Die `FormsAuthenticationModule` zuerst ausgeführt, den Benutzer über ihre Authentifizierungsticket bezeichnet und erstellt ein neues `GenericPrincipal` Objekt. Als Nächstes wird der `RoleManagerModule` Schritte und überschreibt die `GenericPrincipal` -Objekt mit einer `RolePrincipal` Objekt.

Wenn ein anonymer Benutzer die Website besucht, die weder die `FormsAuthenticationModule` noch die `RoleManagerModule` principal-Objekt erstellt.


[![Die Ereignisse der ASP.NET-Pipeline für einen authentifizierten Benutzer bei Verwendung der Formularauthentifizierung und die Rollen-Framework](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Abbildung 1**: die ASP.NET-Pipelineereignisse für einen authentifizierten Benutzer beim Verwenden der Formularauthentifizierung und die Rollen-Framework ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Zwischenspeichern von Rolleninformationen in einem Cookie

Die `RolePrincipal` des Objekts `IsInRole(roleName)` Methodenaufrufe `Roles`.`GetRolesForUser` die Rollen für den Benutzer abrufen, um festzustellen, ob der Benutzer Mitglied ist *RoleName*. Bei Verwendung der `SqlRoleProvider`, dies führt zu einer Abfrage mit der Rolle Verwaltungsspeicher-Datenbank. Bei Verwendung von URL-Autorisierungsregeln rollenbasierte der `RolePrincipal`des `IsInRole` Methode wird aufgerufen, bei jeder Anforderung zu einer Seite, die durch die rollenbasierte URL-Autorisierungsregeln geschützt ist. Anstatt bei jeder Anforderung der Informationen in der Datenbank Nachschlagen der `Roles` Framework enthält eine Option aus, um die Rollen des Benutzers in einem Cookie zwischengespeichert.

Wenn die Rollen-Framework für die Rollen des Benutzers in einem Cookie zwischengespeichert konfiguriert ist die `RoleManagerModule` erstellt das Cookie während der ASP.NET-Pipeline [ `EndRequest` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.endrequest.aspx). Dieses Cookie wird verwendet, in nachfolgenden Anforderungen in der `PostAuthenticateRequest`, wenn also die `RolePrincipal` Objekt erstellt wird. Wenn das Cookie gültig ist und nicht abgelaufen ist, die Daten im Cookie analysiert und verwendet, um die Rollen des Benutzers, und speichern zu füllen der `RolePrincipal` müssen einen Aufruf der `Roles` Klasse, um die Rollen des Benutzers zu bestimmen. Abbildung 2 zeigt dieses Workflows.


[![Der Benutzer die Rolleninformationen kann in einem Cookie zur Verbesserung der Leistung gespeichert werden](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Abbildung 2**: der Benutzer Rolle Daten gespeichert werden können in einem Cookie zum Verbessern der Leistung ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image6.png))


Role-Cache-Cookie-Mechanismus ist standardmäßig deaktiviert. Dies kann aktiviert werden, über die `<roleManager>`; Configuration Markup in `Web.config`. Wir haben auch mithilfe der [ `<roleManager>` Element](https://msdn.microsoft.com/en-us/library/ms164660.aspx) an Rollenanbieter in der <a id="_msoanchor_4"> </a> [ *erstellen und Verwalten von Rollen* ](creating-and-managing-roles-vb.md) Lernprogramm Damit Sie dieses Element bereits in der Anwendungsverzeichnis haben sollten `Web.config` Datei. Cookie-Einstellungen für die Rolle Cache werden als Attribute des angegebenen der `<roleManager>`; Element und sind in Tabelle 1 zusammengefasst.

> [!NOTE]
> Die Konfigurationseinstellungen, die in Tabelle 1 aufgeführten Geben Sie die Eigenschaften des resultierenden Rolle Cache Cookies. Weitere Informationen auf Cookies, deren Funktionsweise und ihre verschiedenen Eigenschaften lesen [dieses Lernprogramms Cookies](http://www.quirksmode.org/js/cookies.html).


| **Property** | **Beschreibung** |
| --- | --- |
| `cacheRolesInCookie` | Ein boolescher Wert, der angibt, ob die cookiezwischenspeicherung verwendet wird. Wird standardmäßig auf `false` festgelegt. |
| `cookieName` | Der Name des Cookies Cache Rolle. Der Standardwert ist ". ASPXROLES". |
| `cookiePath` | Der Pfad des Rollencookies-Name. Der Path-Attribut kann der Entwickler den Umfang eines Cookies zu einem bestimmten Verzeichnishierarchie zu beschränken. Der Standardwert ist "/" der informiert des Browsers, um das Ticket Authentifizierungscookie zu jeder Anforderung an die Domäne zu senden. |
| `cookieProtection` | Gibt an, welche Techniken zum Schutz des Cache-Cookies Rolle verwendet werden. Die zulässigen Werte sind: `All` (Standard); `Encryption`; `None`; und `Validation`. Verweisen Sie zurück zu Schritt 3 des der <a id="_anchor_5"> </a> [ *Konfiguration der Formularauthentifizierung und erweiterte Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Weitere Informationen zu den Schutzstufen Tutorial. |
| `cookieRequireSSL` | Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungscookie zu übermitteln. Der Standardwert ist `false`. |
| `cookieSlidingExpiration` | Ein boolescher Wert, der angibt, ob das Cookie Timeout jedes Mal zurückgesetzt wird der Benutzer die Website während einer Sitzung besucht wird. Der Standardwert ist `false`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` festgelegt ist, um `true`. |
| `cookieTimeout` | Gibt die Zeit in Minuten, nach denen das Ticket Authentifizierungscookie abläuft. Der Standardwert ist `30`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` festgelegt ist, um `true`. |
| `createPersistentCookie` | Ein boolescher Wert, der angibt, ob das Role-Cache-Cookie ein Sitzungscookie oder ein permanentes Cookie ist. Wenn `false` (Standard), ein Sitzungscookie verwendet wird, die gelöscht wird, wenn der Browser geschlossen wird. Wenn `true`, ein permanentes Cookie wird verwendet; es abläuft `cookieTimeout` Anzahl von Minuten, nachdem es erstellt wurde oder nach der vorherigen Besuch, abhängig vom Wert des `cookieSlidingExpiration`. |
| `domain` | Gibt das Cookie Domänenwert an. Der Standardwert ist eine leere Zeichenfolge, die bewirkt, dass der Browser die Domäne verwenden, von der aus (z. B. www.yourdomain.com) ausgestellt wurde. In diesem Fall das Cookie wird **nicht** gesendet werden, wenn Sie festlegen, dass untergeordnete Domänen, z. B. admin.yourdomain.com anfordert. Wenn das Cookie an alle Unterdomänen übergeben werden soll, müssen Sie zum Anpassen der `domain` -Attribut, bei der Einstellung "IhreDomäne.com". |
| `maxCachedResults` | Gibt die maximale Anzahl von Rollennamen, die zwischengespeichert werden in das Cookie an. Der Standard ist 25. Die `RoleManagerModule` erstellt ein Cookie für Benutzer, die zu gehören nicht mehr als `maxCachedResults` Rollen. Folglich die `RolePrincipal` des Objekts `IsInRole` Methode verwendet die `Roles` Klasse, um die Rollen des Benutzers zu bestimmen. Der Grund `maxCachedResults` vorhanden ist, da viele Benutzer-Agents nicht größer als 4.096 Bytes Cookies zulassen. Daher ist dieses Endes vorgesehen, reduziert die Wahrscheinlichkeit, dass diese maximale Größe überschreiten. Wenn Sie extrem langen Rollennamen haben, möchten Sie möglicherweise Geben Sie ggf. eine kleinere `maxCachedResults` Wert; contrariwise, wenn Sie sehr kurze Rollennamen haben, können Sie wahrscheinlich steigern dieser Wert. |

**Tabelle 1**: die Rolle Cache Cookie-Konfigurationsoptionen

Wir konfigurieren Sie die Anwendung für die Rolle nicht persistenter Cache Cookies verwenden. Um dies zu erreichen, aktualisieren Sie die `<roleManager>` Element im `Web.config` auf die folgenden Cookie-bezogene Attribute enthalten:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

Ich habe aktualisiert die `<roleManager>`; Element durch Hinzufügen von drei Attribute: `cacheRolesInCookie`, `createPersistentCookie`, und `cookieProtection`. Durch Festlegen von `cacheRolesInCookie` auf `true`, die `RoleManagerModule` jetzt automatisch die Rollen des Benutzers in einem Cookie zwischengespeichert werden, anstatt zum Suchen von Rolleninformationen bei jeder Anforderung des Benutzers. Ich explizit festlegen, dass die `createPersistentCookie` und `cookieProtection` -Attribute verwenden, um `false` und `All`zugeordnet. Technisch gesehen, ich möchte nicht angeben, dass die Werte für diese Attribute, da ich nur werden mit ihren Standardwerten zugewiesen, aber ich sie hier legen, um ihn explizit machen deaktivieren, dass ich nicht permanenten Cookies verwende und, dass das Cookie wird verschlüsselt und überprüft.

Das ist alles vorhanden ist! Damit werden die Rollen-Framework werden die Benutzerrollen in Cookies zwischengespeichert. Wenn der Browser des Benutzers keine Cookies unterstützt oder ihre Cookies gelöscht wird oder verloren gegangen sind, aus irgendeinem Grund, es keine große Sache ist – die `RolePrincipal` -Objekt einfach anhand der `Roles` Klasse im Fall, dass kein Cookie (oder eine ungültige oder abgelaufene) verfügbar ist.

> [!NOTE]
> Microsofts Muster &amp; Practices-Gruppe rät davon ab, zu permanenten Rolle Cache Cookies verwenden. Da der Besitz des Cookies Cache Rolle ausreichen, um die Rollenmitgliedschaft, nachzuweisen ist, wenn ein Hacker aus irgendeinem Grund ein gültiger Benutzer Cookie zugreifen kann, kann er dieser Benutzer imitieren. Die Wahrscheinlichkeit, dass dies nimmt zu, wenn das Cookie auf den Browser des Benutzers beibehalten wird. Weitere Informationen zu diesem sicherheitsempfehlung sowie andere Sicherheitsaspekte, finden Sie in der [Frage Sicherheitsliste für ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Schritt 1: Definieren einer rollenbasierten URL-Autorisierungsregeln

Entsprechend der Anleitung unter dem <a id="_msoanchor_6"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm URL-Autorisierung bietet eine Möglichkeit, den Zugriff auf eine Reihe von Seiten auf einem vom Benutzer oder die Rolle "Rolle" "durch" beschränken Basis. Die URL-Autorisierungsregeln werden buchstabiert `Web.config` mithilfe der [ `<authorization>` Element](https://msdn.microsoft.com/en-us/library/8d82143t.aspx) mit `<allow>` und `<deny>` untergeordnete Elemente. Zusätzlich zu den benutzerspezifischen Autorisierungsregeln, die im vorherigen Lernprogrammen erläutert jede `<allow>` und `<deny>` untergeordnetes Element kann auch beinhalten:

- Eine bestimmte Rolle
- Eine durch Trennzeichen getrennte Liste von Rollen

Beispielsweise die URL-Autorisierungsregeln gewähren des Zugriffs für Benutzer in der Administratoren und Abteilungsleiter Rollen jedoch Verweigern des Zugriffs auf alle anderen:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

Die `<allow>` Element im Markup oben gibt an, dass die Administratoren und Abteilungsleiter Rollen zulässig sind; der `<deny>`; Element anweist, dass *alle* Benutzern verweigert werden.

Wir unsere Anwendung konfigurieren, damit die `ManageRoles.aspx`, `UsersAndRoles.aspx`, und `CreateUserWizardWithRoles.aspx` Seiten sind nur verfügbar, auf die Benutzer in der "Administratoren", während die `RoleBasedAuthorization.aspx` Seite bleibt für alle Besucher zugegriffen werden kann.

Um dies zu erreichen, starten Sie durch Hinzufügen einer `Web.config` Datei wird in der `Roles` Ordner.


[![Hinzufügen einer Datei "Web.config" in das Verzeichnis Rollen](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` Datei wird in der `Roles` Verzeichnis ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image9.png))


Fügen Sie die folgende Konfiguration Markup `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

Die `<authorization>` Element in der `<system.web>` Abschnitt gibt an, dass nur Benutzer in der "Administratoren" auf Ressourcen in ASP.NET zugreifen können die `Roles` Verzeichnis. Die `<location>` -Element definiert einen anderen Satz von URL-Autorisierungsregeln für die `RoleBasedAuthorization.aspx` Seite, wodurch alle Benutzer, um die Seite besuchen.

Nach dem Speichern Ihrer Änderungen an `Web.config`, melden Sie sich als ein Benutzer, der nicht in der "Administratoren" ist, und wiederholen Sie dann mit den geschützten Seiten besucht werden soll. Die `UrlAuthorizationModule` erkennt, dass Sie nicht berechtigt sind, besuchen die angeforderte Ressource; daher die `FormsAuthenticationModule` werden Sie zur Anmeldeseite umgeleitet. Die Anmeldeseite leitet dann Sie die `UnauthorizedAccess.aspx` Seite (siehe Abbildung 4). Diese letzte Umleitung von der Anmeldeseite `UnauthorizedAccess.aspx` tritt aufgrund von Code, die wir, zu der Anmeldeseite in Schritt2 hinzugefügt der <a id="_msoanchor_7"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm. Insbesondere, leitet die Anmeldeseite automatisch allen authentifizierten Benutzern das `UnauthorizedAccess.aspx` , wenn die Abfragezeichenfolge enthält ein `ReturnUrl` Parameter als dieser Parameter gibt an, dass der Benutzer die Anmeldeseite nach dem Versuch, eine Seite angezeigt, er nicht wurde, eingetroffen Berechtigung zum Anzeigen.


[![Nur Benutzer in der "Administratoren" können die geschützte Seiten anzeigen.](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Abbildung 4**: nur Benutzer in der "Administratoren" können die geschützte Seiten anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image12.png))


Melden Sie sich ab und melden Sie sich als ein Benutzer, der in der "Administratoren" ist. Jetzt sollten Sie die drei geschützte Seiten anzeigen können.


[![Tito Besuchen der UsersAndRoles.aspx Seite da er in der "Administratoren" ist](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Abbildung 5**: Tito Besuchen der `UsersAndRoles.aspx` Seite da ist er in der "Administratoren" ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image15.png))


> [!NOTE]
> Beim Angeben von URL-Autorisierungsregeln – für Rollen oder Benutzer – ist es wichtig zu bedenken, die die Regeln analysierten einer nacheinander von oben nach unten. Sobald eine Übereinstimmung gefunden wird, If je nach der Übereinstimmung der Benutzer wird Zugriff gewährt oder verweigert, ein `<allow>` oder `<deny>` Element. **Wenn keine Übereinstimmung gefunden wird, wird dem Benutzer Zugriff gewährt.** Wenn Sie den Zugriff auf eine oder mehrere Benutzerkonten einschränken möchten, ist es folglich unabdinglich, Sie verwenden eine `<deny>` Element als das letzte Element in der Konfiguration der URL-Autorisierung. **Wenn die URL-Autorisierungsregeln keine enthalten eine**`<deny>`**Element, alle Benutzer werden Zugriff gewährt werden.** Eine ausführlichere Erläuterung auf wie die URL-Autorisierungsregeln analysiert werden, finden Sie zurück in die "eine Betrachtung der Verwendung der `UrlAuthorizationModule` verwendet die Autorisierungsregeln zum gewähren oder Verweigern des Zugriffs" im Abschnitt der <a id="_msoanchor_8"> </a> [  *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Schritt 2: Funktionalität, die auf der Grundlage des aktuell angemeldeten Benutzers Benutzerrollen beschränkt.

URL Authorization macht es einfach, grob Autorisierung anzugeben, Status welche Identitäten Regeln sind zulässig und, welche werden verweigert, eine bestimmte Seite (oder alle Seiten in einem Ordner und seinen Unterordnern) anzeigen. In bestimmten Fällen sollen wir jedoch ermöglicht allen Benutzern finden Sie auf einer Seite, aber die Seite funktionseinschränkungen basierend auf der Website besucht Benutzerrollen. Dies kann der Übergang ein- oder Ausblenden von Daten basierend auf der Rolle des Benutzers oder bietet zusätzliche Funktionen für Benutzer, die zu einer bestimmten Rolle gehören.

Solche möglich, differenzierte rollenbasierte Autorisierungsregeln können entweder deklarativ oder programmgesteuert (oder über eine Kombination aus beiden) implementiert werden. Im nächsten Abschnitt sehen wir, wie deklarative möglich, differenzierte Autorisierung über das LoginView-Steuerelement implementiert werden. Danach wird programmgesteuerte Techniken vorgestellt. Betrachten wir können Autorisierungsregeln möglich, differenzierte anwenden, benötigen jedoch wir zuerst zum Erstellen einer Seite, deren Funktionalität für die Rolle des Benutzers, besuchen sie abhängig ist.

Erstellen Sie eine Seite, die alle Benutzerkonten im System in einem GridView aufgelistet sind. GridView wird jedes Benutzers Benutzernamen, e-Mail-Adresse, Datum der letzten Anmeldung und Kommentare zu dem Benutzer gehören. Zusätzlich zur Anzeige der Informationen des Benutzers, wird die GridView bearbeiten und Löschen von Funktionen. Anfänglich wird auf dieser Seite erstellen, mit dem Bearbeiten und Löschen von Funktionen, die allen Benutzern zur Verfügung. In den Abschnitten "LoginView-Steuerelement mithilfe von" und "Programmgesteuert beschränken Funktionalität" sehen wir, aktivieren oder deaktivieren diese Funktionen, die auf Basis der besuchten Benutzerrolle.

> [!NOTE]
> Die ASP.NET-Seite an, die wir erstellen verwendet eine GridView-Steuerelements, um die Benutzerkonten anzuzeigen. Seit diesem Lernprogramm Reihe auf formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, sollten ich keine Erörterung der internen Funktionsweise von des GridView-Steuerelements zu viel Zeit. Während dieses Lernprogramm spezifische schrittweise Anweisungen zum Einrichten dieser Seite enthält, ist es nicht stürzen die Details Warum Auswahl bestimmter vorgenommen wurden, oder was auf der gerenderten Ausgabe Auswirkungen bestimmte Eigenschaften haben. Eine gründliche Prüfung des GridView-Steuerelements, sehen Sie sich meine  *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)*  Reihe von Lernprogrammen.


Öffnen Sie zunächst die `RoleBasedAuthorization.aspx` auf der Seite der `Roles` Ordner. Ziehen Sie eine GridView auf der Seite auf die Designer, und legen Sie dessen `ID` auf `UserGrid`. Im Anschluss schreiben Sie Code, der Aufrufe der `Membership`.`GetAllUsers` Methode und bindet das resultierende `MembershipUserCollection` Objekt an die GridView. Die `MembershipUserCollection` enthält ein `MembershipUser` Objekt für jedes Benutzerkonto in das System; `MembershipUser` Objekte verfügen über Eigenschaften, z. B. `UserName`,`Email`,`LastLoginDate` usw. lauten.

Bevor wir den Code, der die Benutzerkonten an das Raster gebunden schreiben, wir zunächst die GridView Felder definieren. Der GridView Smart Tag, klicken Sie auf den Link "Spalten bearbeiten", um das Dialogfeld "Felder" zu starten (siehe Abbildung 6). Von hier aus, deaktivieren Sie das Kontrollkästchen "Automatisches Generieren von Feldern" in der unteren linken Ecke. Da die soll diese GridView einschließen, bearbeiten und Löschen von Funktionen, die eine CommandField hinzufügen und Festlegen der `ShowEditButton` und `ShowDeleteButton` Eigenschaften auf "true". Als Nächstes fügen Sie vier Felder für die Anzeige der `UserName`, `Email`, `LastLoginDate`, und `Comment` Eigenschaften. Verwenden Sie für die beiden schreibgeschützten Eigenschaften einer BoundField (`UserName` und `LastLoginDate`) und von TemplateFields für zwei bearbeitbaren Felder (`Email` und `Comment`).

Haben Sie die erste BoundField-Anzeige der `UserName` Eigenschaft; Set seine `HeaderText` und `DataField` Eigenschaften "UserName". Dieses Feld wird nicht mehr bearbeitet werden, so festgelegt, dessen `ReadOnly` Eigenschaft auf "true". Konfigurieren der `LastLoginDate` BoundField durch Festlegen seiner `HeaderText` "Letzte Anmeldung" und die zugehörige `DataField` auf "LastLoginDate". Wir Formatieren der Ausgabe dieser BoundField so, dass nur das Datum (anstelle von Datum und Uhrzeit) angezeigt wird. Um dies zu erreichen, legen Sie diese BoundField `HtmlEncode` Eigenschaft auf "false" und dessen `DataFormatString` Eigenschaft auf "{0: d}". Legen Sie auch die `ReadOnly` Eigenschaft auf "true".

Legen Sie die `HeaderText` Eigenschaften eines der beiden von TemplateFields "Email" und "Kommentar".


[![Die GridView Felder kann konfiguriert werden, über die Felder (Dialogfeld)](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Abbildung 6**: GridView Felder können werden konfiguriert über die Felder (Dialogfeld) ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image18.png))


Jetzt müssen wir definieren die `ItemTemplate` und `EditItemTemplate` für den "Email" und "Kommentar" von TemplateFields. Ein Bezeichnung-Websteuerelement jeder Hinzufügen der `ItemTemplates` und binden Sie ihre `Text` Eigenschaften der `Email` und `Comment` Eigenschaften bzw.

Fügen Sie ein Textfeld mit dem Namen für die TemplateField "Email" `Email` auf seine `EditItemTemplate` und binden seine `Text` Eigenschaft, um die `Email` Eigenschaft mithilfe der bidirektionalen Datenbindung. Hinzufügen eines RequiredFieldValidator und RegularExpressionValidator auf die `EditItemTemplate` , stellen Sie sicher, dass ein Besucher, bearbeiten die e-Mail-Eigenschaft eine gültige e-Mail-Adresse eingegeben wurde. Fügen Sie ein mehrzeiliges Textfeld mit dem Namen für die TemplateField "Kommentar" `Comment` auf seine `EditItemTemplate`. Legen Sie des Textfelds `Columns` und `Rows` Eigenschaften 40 und 4, und dann binden Sie seine `Text` Eigenschaft, um die `Comment` Eigenschaft mithilfe der bidirektionalen Datenbindung.

Nach dem Konfigurieren dieser von TemplateFields, sollte ihre deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Beim Bearbeiten oder Löschen eines Benutzerkontos, müssen wir wissen, des Benutzers `UserName` Eigenschaftswert. Legen Sie der GridView `DataKeyNames` Eigenschaft auf "UserName", damit diese Informationen über der GridView verfügbar sind `DataKeys` Auflistung.

Schließlich die Seite ein ValidationSummary-Steuerelement hinzu, und legen Sie dessen `ShowMessageBox` Eigenschaft auf "true" und dessen `ShowSummary` Eigenschaft auf "false". Mit diesen Einstellungen wird der ValidationSummary eine clientseitige Warnung angezeigt, wenn der Benutzer versucht, ein Benutzerkonto mit einer fehlenden oder ungültigen e-Mail-Adresse bearbeiten.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Wir haben jetzt auf dieser Seite deklarativem Markup abgeschlossen. Unsere nächste Aufgabe ist die Gruppe von Benutzerkonten an die GridView zu binden. Fügen Sie eine Methode mit dem Namen `BindUserGrid` auf die `RoleBasedAuthorization.aspx` Seite des Code-Behind-Klasse, bindet das `MembershipUserCollection` zurückgegebenes `Membership.GetAllUsers` auf die `UserGrid` GridView. Rufen Sie diese Methode aus der `Page_Load` Ereignishandler ersten Besuch der Seite.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Finden Sie mit diesem Code werden auf der Seite über einen Browser. Wie Abbildung 7 zeigt, sollte eine GridView, Auflisten von Informationen zu den einzelnen Benutzerkonten im System angezeigt werden.


[![Die UserGrid GridView listet Informationen zu jeder Benutzer im System](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Abbildung 7**: die `UserGrid` GridView listet Informationen zu jeder Benutzer im System ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image21.png))


> [!NOTE]
> Die `UserGrid` GridView zeigt eine Liste aller Benutzer in einem nicht ausgelagerten-Schnittstelle. Diese Schnittstelle einfaches Raster ist nicht für Szenarien geeignet, es mehrere Dutzend oder mehrere Benutzer sind. Eine Möglichkeit besteht darin die GridView um Paging aktivieren konfigurieren. Die `Membership.GetAllUsers` Methode verfügt über zwei Überladungen: eine, die keine Eingabeparameter akzeptiert und gibt alle Benutzer und eine, die in Ganzzahlwerte für die Seite "Index" und "Seitengröße und gibt nur die angegebene Teilmenge der Benutzer, zurück. Die zweite Überladung kann effizienter Seite durch den Benutzer verwendet werden, da präzise Untermenge von Benutzerkonten zurückgegeben statt *alle* davon. Wenn Sie Tausende von Benutzerkonten verfügen, empfiehlt es sich, eine filterbasierten-Schnittstelle, eine berücksichtigen, die zeigt nur die Benutzer für die Instanz, deren Benutzername mit einem ausgewählten Zeichen beginnt. Die [ `Membership.FindUsersByName` Methode](https://msdn.microsoft.com/en-us/library/system.web.security.membership.findusersbyname.aspx) ist ideal zum Erstellen einer Filter-basierte Benutzeroberfläche. Betrachten wir eine solche Schnittstelle in einem späteren Lernprogramm erstellen.


GridView-Steuerelements bietet integrierte bearbeiten und Löschen von Support, wenn das Steuerelement an ein ordnungsgemäß konfiguriertes Datenquellen-Steuerelement, z. B. die SqlDataSource oder ObjectDataSource gebunden ist. Die `UserGrid` GridView, verfügt jedoch über seine Daten programmgesteuert gebunden; daher wir müssen Code schreiben, um diese beiden Schritte ausführen. Insbesondere benötigen wir zum Erstellen von Ereignishandlern für der GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, und `RowDeleting` -Ereignisse, die ausgelöst werden, wenn ein Besucher der GridView klickt bearbeiten "," Abbrechen "," aktualisieren, oder Löschen von Schaltflächen.

Erstellen die Ereignishandler für der GridView zunächst `RowEditing`, `RowCancelingEdit`, und `RowUpdating` Ereignisse und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Die `RowEditing` und `RowCancelingEdit` Ereignishandler legen Sie einfach der GridView `EditIndex` -Eigenschaft, und klicken Sie dann erneut binden, die die Liste der Benutzer-mit dem Raster Konten. Interessante Dinge erfolgt in der `RowUpdating` -Ereignishandler. Dieser Ereignishandler gestartet wird, indem Sie sicherstellen, dass die Daten gültig ist, und klicken Sie dann holt die `UserName` Wert des bearbeiteten Benutzerkontos aus der `DataKeys` Auflistung. Die `Email` und `Comment` Textfelder in der von zwei TemplateFields `EditItemTemplate` s werden dann programmgesteuert auf die verwiesen wird. Ihre `Text` Eigenschaften enthalten, die bearbeitete e-Mail-Adresse und einen Kommentar.

Zum Aktualisieren von einem Benutzerkonto über die Mitgliedschaft-API müssen wir erhalten zunächst die Informationen des Benutzers, was wir über einen Aufruf an `Membership.GetUser(userName)`. Das zurückgegebene `MembershipUser` des Objekts `Email` und `Comment` Eigenschaften werden dann mit den Werten, die an die Bearbeitungsoberfläche in die beiden Textfelder ein eingegebene aktualisiert. Abschließend werden diese Änderungen gespeichert, mit einem Aufruf von [ `Membership.UpdateUser` ](https://msdn.microsoft.com/en-us/library/system.web.security.membership.updateuser.aspx). Die `RowUpdating` -Ereignishandler durch Wiederherstellen der GridView vorab bearbeiten die Benutzeroberfläche abgeschlossen wurde.

Als Nächstes erstellen Sie die `RowDeleting` RowDeleting-Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

Der oben genannten Ereignishandler beginnt mit dem grabbing der `UserName` Wert aus der GridView `DataKeys` Sammlung; diese `UserName` Wert wird dann in der Mitgliedschaft-Klasse übergeben [ `DeleteUser` Methode](https://msdn.microsoft.com/en-us/library/system.web.security.membership.deleteuser.aspx). Die `DeleteUser` Methode löscht das Benutzerkonto aus dem System, einschließlich der zugehörigen Mitgliedschaftsdaten (wie z. B. welche Rollen dieser Benutzer gehört). Nach dem Löschen des Benutzers, dem Raster `EditIndex` auf-1 festgelegt ist (für den Fall, dass der Benutzer löschen geklickt hat, während eine neue Zeile in den Bearbeitungsmodus wurde) und die `BindUserGrid` -Methode aufgerufen wird.

> [!NOTE]
> Die Schaltfläche "löschen" ist dabei jegliche Art von Bestätigung durch den Benutzer vor dem Löschen des Benutzerkontos nicht erforderlich. Ich empfehlen Ihnen, fügen eine Form der Bestätigung durch den Benutzer zu verringern, die Wahrscheinlichkeit eines Kontos versehentlich gelöscht wird. Einer der einfachsten Möglichkeiten, eine Aktion zu bestätigen erfolgt über einen clientseitigen bestätigen (Dialogfeld). Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen von clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


Stellen Sie sicher, dass diese Seite wie erwartet funktioniert. Sie muss jeder Benutzer e-Mail-Adresse und einen Kommentar zu bearbeiten, als auch ein Benutzerkonto löschen können. Da die `RoleBasedAuthorization.aspx` Seite ist für alle Benutzer verfügbar, die jeder Benutzer – sogar anonyme Besucher – kann finden Sie auf dieser Seite und bearbeiten und Löschen von Benutzerkonten! Wir aktualisieren diese Seite, damit nur Benutzer in den Rollen Abteilungsleiter und Administratoren können die e-Mail-Adresse eines Benutzers und Kommentar bearbeiten, und nur Administratoren können ein Benutzerkonto löschen.

Im Abschnitt "LoginView-Steuerelement mithilfe von" prüft mithilfe des LoginView-Steuerelements Anweisungen speziell für die Rolle des Benutzers angezeigt. Wenn eine Person in der "Administratoren" auf dieser Seite besucht, veranschaulichen wir Anweisungen zum Bearbeiten und Löschen von Benutzern. Wenn ein Benutzer in der Rolle "Abteilungsleiter" dieser Seite erreicht, veranschaulichen wir Anweisungen zum Bearbeiten von Benutzern. Und wenn Besucher anonym oder nicht Rolle Abteilungsleiter "oder" Administratoren ", zeigt es eine Meldung angezeigt, dass sie nicht bearbeiten oder löschen die Benutzerkontoinformationen. Im Abschnitt "Programmgesteuert beschränken Funktionalität" werden wir Code schreiben, die programmgesteuert anzeigen oder Ausblenden der Schaltflächen Bearbeiten und löschen, die basierend auf der Rolle des Benutzers.

### <a name="using-the-loginview-control"></a>Verwenden das LoginView-Steuerelement

Wie wir im letzten Lernprogramme gesehen haben, LoginView-Steuerelement ist nützlich, um unterschiedliche Schnittstellen für authentifizierte und anonyme Benutzer anzuzeigen, aber LoginView-Steuerelement kann auch zum Anzeigen von verschiedenen Markups basierend auf die Rollen des Benutzers verwendet werden. Wir verwenden Sie ein LoginView-Steuerelement, um andere Anweisungen, die auf Basis der besuchten Benutzerrolle anzuzeigen.

Beginnen Sie, indem Sie eine der oben genannten LoginView Hinzufügen der `UserGrid` GridView. Wie bereits erläutert haben, hat das LoginView-Steuerelement zwei integrierte Vorlagen: `AnonymousTemplate` und `LoggedInTemplate`. Geben Sie eine kurze Fehlermeldung in beide Vorlagen, die der Benutzer darüber informiert, dass sie nicht bearbeiten oder Löschen keine Benutzerinformationen ein.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Zusätzlich zu den `AnonymousTemplate` und `LoggedInTemplate`, zählen das LoginView-Steuerelement *RoleGroups*, die rollenspezifische Vorlagen sind. Jede RoleGroup enthält eine einzelne Eigenschaft `Roles`, der gibt an, welche Rollen die RoleGroup betrifft. Die `Roles` Eigenschaft kann festgelegt werden, um eine einzige Rolle (z. B. "Administratoren") oder auf eine durch Trennzeichen getrennte Liste von Rollen (z. B. "Administratoren, Abteilungsleiter").

Um die RoleGroups verwalten möchten, klicken Sie auf den Link "RoleGroups bearbeiten" aus des Steuerelements Smarttag zum Öffnen von RoleGroup Typauflistungs-Editor. Fügen Sie zwei neue RoleGroups hinzu. Legen Sie die erste RoleGroup `Roles` -Eigenschaft auf "Administratoren" und das zweite in "Abteilungsleiter".


[![Verwalten der LoginView rollenspezifische Vorlagen über RoleGroup Auflistungs-Editor](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Abbildung 8**: Verwalten der LoginView rollenspezifische Vorlagen über die RoleGroup Auflistungs-Editor ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image24.png))


Klicken Sie auf OK, um den RoleGroup Auflistungs-Editor zu schließen. aktualisiert die LoginView deklarative Markup enthalten eine `<RoleGroups>` im Abschnitt mit einer `<asp:RoleGroup>` untergeordnete Element für jeden RoleGroup definiert den RoleGroup-Auflistungs-Editor. Darüber hinaus die "Ansichten" Dropdownliste in der LoginView Smarttag - die anfänglich aufgeführt nur die `AnonymousTemplate` und `LoggedInTemplate` – umfasst jetzt auch die hinzugefügte RoleGroups.

Bearbeiten Sie die RoleGroups, sodass Benutzer in die Rolle "Abteilungsleiter" sind die angezeigten Anweisungen zum Bearbeiten von Benutzerkonten, während Benutzer in der "Administratoren" Anweisungen zum Bearbeiten und Löschen von angezeigt werden. Nachdem diese Änderungen vorgenommen wurden, sollte Ihre LoginView deklarative Markup etwa wie folgt aussehen.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Nachdem diese Änderungen vorgenommen wurden, speichern Sie die Seite, und besuchen sie dann über einen Browser. Besuchen Sie zuerst die Seite als anonymer Benutzer ein. Die Meldung angezeigt werden soll, "Sie sind nicht in das System angemeldet. Aus diesem Grund weder Sie bearbeiten noch löschen keine Benutzerinformationen." Klicken Sie dann melden Sie sich, als die eines authentifizierten Benutzers, aber weder in die Rolle "Abteilungsleiter" oder "Administratoren" ist. Dieses Mal sollten Sie eine Meldung angezeigt, "Sie sind nicht Mitglied der Rollen Abteilungsleiter oder Administratoren. Aus diesem Grund weder Sie bearbeiten noch löschen keine Benutzerinformationen."

Als Nächstes melden Sie sich als ein Benutzer ein Mitglied der Rolle "Abteilungsleiter" ist. Dieses Mal sollte der Vorgesetzte rollenspezifische message (siehe Abbildung 9). Und wenn Sie in der Rolle "sehen Sie die Administratoren rollenspezifische"-Nachricht (siehe Abbildung 10) Administratoren als Benutzer anmelden.


[![Bruce wird der Vorgesetzte rollenspezifische-Meldung angezeigt.](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Abbildung 9**: Bruce Abteilungsleiter rollenspezifische Nachricht angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image27.png))


[![Tito wird die Administratoren rollenspezifische-Meldung angezeigt.](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Abbildung 10**: Tito Administratoren rollenspezifische Nachricht angezeigt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image30.png))


Als die Screenshots in Abbildung 9 und 10 anzeigen rendert die LoginView nur eine Vorlage, auch wenn mehrere Vorlagen anwenden. Bruce und Tito sind in der Benutzer angemeldet, aber die LoginView rendert nur die übereinstimmenden RoleGroup und nicht die `LoggedInTemplate`. Darüber hinaus Tito gehört zu den Rollen "Administratoren" und "Abteilungsleiter, noch das LoginView-Steuerelement rendert die Administratoren rollenspezifische Vorlage anstelle der Vorgesetzte eine.

Abbildung 11 zeigt den Workflow vom LoginView-Steuerelement verwendet wird, um zu bestimmen, welche Vorlage zum Rendern. Beachten Sie, dass die Vorlage LoginView gerendert, wenn mehr als einmal RoleGroup angegeben vorhanden ist wird, die *erste* RoleGroup, der mit übereinstimmt. Das heißt, wenn wir die Abteilungsleiter RoleGroup als erste RoleGroup und die Administratoren als zweites platziert werden hatte, klicken Sie dann besucht Tito auf dieser Seite er würde die Abteilungsleiter Meldung angezeigt.


[![LoginView-Steuerelement-Workflow für die Bestimmung der Vorlage zum Rendern](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Abbildung 11**: die LoginView-Steuerelement-Workflow für die Bestimmung welche Vorlage zum Rendern ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funktionalität programmgesteuert beschränkt.

Während der LoginView-Steuerelement verschiedene Anweisungen basierend auf der Rolle des Benutzers auf der Seite angezeigt wird, bleiben die Schaltflächen Bearbeiten und "Abbrechen" für alle sichtbar. Programmgesteuertes Ausblenden von der Schaltflächen Bearbeiten "und" Delete "für Benutzer, die weder die Administratoren der Vorgesetzte Rolle angehören und anonyme Besucher ist erforderlich. Wir müssen die Schaltfläche "löschen" für alle Benutzer auszublenden, der kein Administrator ist. Um dies zu erreichen schreiben Sie ein wenig Code zum programmgesteuerten der CommandField bearbeiten und löschen LinkButtons und legt deren `Visible` Eigenschaften `False`, falls erforderlich.

Die einfachste Möglichkeit, programmgesteuert auf Steuerelemente in einem CommandField verweisen wird zuerst in eine Vorlage konvertieren. Um dies zu erreichen, klicken Sie auf den Link "Spalten bearbeiten" aus der GridView Smarttag, wählen Sie die CommandField aus der Liste der aktuellen Felder und klicken Sie auf den Link "Dieses Feld in ein TemplateField konvertieren". Dadurch wird die CommandField in ein TemplateField mit einer `ItemTemplate` und `EditItemTemplate`. Die `ItemTemplate` enthält die bearbeiten und löschen LinkButtons während der `EditItemTemplate` beinhaltet das Update und LinkButtons "Abbrechen".


[![Die CommandField in ein TemplateField konvertieren](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Abbildung 12**: CommandField in ein TemplateField konvertieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image36.png))


Aktualisieren Sie die bearbeiten und Löschen von LinkButtons in der `ItemTemplate`wird durch das Festlegen ihrer `ID` Eigenschaften mit Werten `EditButton` und `DeleteButton`bzw.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Wenn Daten an die GridView gebunden ist, zählt die GridView Datensätze in seiner `DataSource` Eigenschaft und generiert eine entsprechende `GridViewRow` Objekt. Als jeweils `GridViewRow` -Objekt wird erstellt, die `RowCreated` Ereignis ausgelöst wird. Um die Schaltflächen Bearbeiten und Löschen für nicht autorisierte Benutzer ausblenden möchten, müssen wir erstellen Sie einen Ereignishandler für dieses Ereignis und programmgesteuert verweisen die bearbeiten und löschen LinkButtons, Festlegen ihrer `Visible` Eigenschaften entsprechend.

Erstellen Sie einen Ereignishandler die `RowCreated` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Beachten Sie, dass die `RowCreated` Ereignis ausgelöst wird, für die *alle* GridView Zeilen, einschließlich der Header, Footer, die Pager-Schnittstelle usw. Wir möchten nur die bearbeiten und Löschen von LinkButtons programmgesteuert verweisen, wenn wir mit einer Datenzeile nicht im Bearbeitungsmodus befindet arbeiten (da die Zeile im Bearbeitungsmodus Schaltflächen für Update "und" Abbrechen "" anstelle von bearbeiten und löschen hat). Diese Überprüfung erfolgt durch die `If` Anweisung.

Wenn wir mit einer Datenzeile, die nicht im Bearbeitungsmodus befindet arbeiten, die bearbeiten und Löschen von LinkButtons verwiesen wird und ihre `Visible` Eigenschaften werden auf der Basis der zurückgegebene boolesche Werte der `User` des Objekts `IsInRole(roleName)` Methode. Die `User` -Objekt verweist auf den Prinzipal erstellt, indem die `RoleManagerModule`; daher die `IsInRole(roleName)` Methode verwendet die Rollen-API, um zu bestimmen, ob der aktuelle Besucher gehört *RoleName*.

> [!NOTE]
> Wir konnten haben verwendet die Rollen-Klasse direkt, ersetzt des Aufrufs von `User.IsInRole(roleName)` durch einen Aufruf der [ `Roles.IsUserInRole(roleName)` Methode](https://msdn.microsoft.com/en-us/library/system.web.security.roles.isuserinrole.aspx). Verwenden Sie des Prinzipalobjekt entschied ich mich `IsInRole(roleName)` Methode in diesem Beispiel ist es effizienter als die Rollen-API direkt verwenden. Weiter oben in diesem Lernprogramm konfiguriert es die Rollen-Manager, um die Rollen des Benutzers in einem Cookie zwischengespeichert. Dies Cookiedaten zwischengespeichert wird nur verwendet, wenn des Prinzipals `IsInRole(roleName)` Methode wird aufgerufen, direkte Aufrufe an die API-Rollen umfassen immer einen Trip zur rollenspeicher. Auch wenn die Rollen nicht in einem Cookie zwischengespeichert wurden, Aufrufen des Prinzipalobjekt `IsInRole(roleName)` Methode ist in der Regel effizienter, da Wenn sie aufgerufen wird, für die ersten Mal während einer Anforderung und die Ergebnisse zwischengespeichert. Die API-Rollen auf der anderen Seite führt jeglichen Zwischenspeicherns keine. Da die `RowCreated` wird ausgelöst, einmal für jede Zeile in der GridView mit `User.IsInRole(roleName)` umfasst nur einen Trip zur rollenspeicher, wohingegen `Roles.IsUserInRole(roleName)` erfordert *N* Reisen, in denen *N* ist die Anzahl von Benutzerkonten, die im Raster angezeigt.


Der Bearbeitungsschaltfläche `Visible` -Eigenschaftensatz auf `True` Wenn der Benutzer Zugriff auf diese Seite in der Rolle "Administratoren" oder Abteilungsleiter; andernfalls ist es auf festgelegt `False`. Der Schaltfläche Löschen `Visible` -Eigenschaftensatz auf `True` nur, wenn der Benutzer in der "Administratoren" ist.

Testen Sie diese Seite über einen Browser. Wenn Sie besuchen Sie die Seite als eine anonyme Besucher oder als ein Benutzer, der weder ein Supervisor noch ein Administrator ist, ist die CommandField leer. weiterhin vorhanden, aber die Schaltflächen als einen thin Splitter ohne das Bearbeiten oder löschen.

> [!NOTE]
> Es ist möglich, die CommandField ausblenden vollständig bei einer nicht Vorgesetzten und Nichtadministrator-Besuch die Seite. Ich lassen Sie dieses als Übung für den Leser.


[![Das Bearbeiten und Löschen von Schaltflächen sind für nicht-Abteilungsleiter und Nichtadministratoren ausgeblendet.](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Abbildung 13**: die bearbeiten und Löschen von Schaltflächen sind für nicht-Abteilungsleiter und Nichtadministratoren ausgeblendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image39.png))


Wenn ein Benutzer, der die Rolle "Abteilungsleiter" (jedoch nicht zur Rolle "Administratoren") gehört, besucht, sieht er nur die Schaltfläche "Bearbeiten".


[![Während die Schaltfläche "Bearbeiten" für Abteilungsleiter verfügbar ist, wird die Schaltfläche "löschen" ausgeblendet.](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Abbildung 14**: während der klicken "Bearbeiten" ist verfügbar, für Abteilungsleiter, die Schaltfläche "löschen" wird ausgeblendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image42.png))


Und wenn ein Administrator besucht, hat sie Zugriff auf die Schaltflächen Bearbeiten und löschen.


[![Das Bearbeiten und Löschen von Schaltflächen sind nur für Administratoren verfügbar](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Abbildung 15**: die bearbeiten und Löschen von Schaltflächen sind nur für Administratoren zur Verfügung ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Schritt 3: Anwenden von Regeln rollenbasierte Autorisierung auf Klassen und Methoden

Bearbeiten Sie in Schritt2, die wir eingeschränkte Funktionen für Benutzer in den Rollen Abteilungsleiter und Administratoren und löschen Sie Funktionen nur für Administratoren. Zu diesem Zweck die zugeordneten Elemente der Benutzeroberfläche für nicht autorisierte Benutzer über die programmgesteuerte Techniken ausblenden. Diese Art von Measures garantieren nicht, dass ein nicht autorisierter Benutzer einen privilegierten Vorgang kann nicht durchgeführt werden. Es gibt möglicherweise Elemente der Benutzeroberfläche, die später hinzugefügt werden oder dass wir haben vergessen, die für nicht autorisierte Benutzer ausblenden. Oder ein Hacker möglicherweise eine andere Möglichkeit zum Abrufen der ASP.NET-Seite zum Ausführen der gewünschten Methode ermitteln.

Eine einfache Möglichkeit, stellen Sie sicher, dass ein nicht autorisierter Benutzer auf ein bestimmter Teil der Funktionalität zugreifen kann diese Klasse oder Methode mit ergänzt ist die [ `PrincipalPermission` Attribut](https://msdn.microsoft.com/en-us/library/system.security.permissions.principalpermissionattribute.aspx). Wenn der .NET Common Language Runtime eine Klasse verwendet oder eine der Methoden führt, überprüft stellen Sie sicher, dass der aktuelle Sicherheitskontext-Berechtigung. Die `PrincipalPermission` Attribut bietet einen Mechanismus, der über den können wir diese Regeln definieren.

Erläutert, mit der `PrincipalPermission` -Attribut wieder in die <a id="_msoanchor_9"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm. Insbesondere wurde erläutert, wie der GridView ergänzen `SelectedIndexChanged` und `RowDeleting` -Ereignishandler so, dass sie nur von authentifizierten Benutzern und Tito, bzw. ausgeführt werden konnte. Die `PrincipalPermission` Attribut funktioniert ebenso gut mit Rollen auf.

Wir veranschaulicht die Verwendung der `PrincipalPermission` Attribut für der GridView `RowUpdating` und `RowDeleting` Ereignishandler Ausführung für nicht autorisierte Benutzer verhindert werden soll. Alles, was erforderlich ist, das entsprechende Attribut über jede Funktionsdefinition hinzufügen:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

Das Attribut für die `RowUpdating` -Ereignishandler schreibt vor, dass nur Benutzer in den Rollen "Administratoren" oder Abteilungsleiter, als das Attribut auf den Ereignishandler ausgeführt werden können die `RowDeleting` Ereignishandler beschränkt die Ausführung für Benutzer in der Administratoren die Rolle.


> [!NOTE]
> Die `PrincipalPermission` Attribut wird dargestellt, wie eine Klasse in der `System.Security.Permissions` Namespace. Achten Sie darauf, dass Sie zum Hinzufügen einer `Imports System.Security.Permissions` -Anweisung am Anfang der CodeBehind-Klasse-Datei zu diesem Namespace zu importieren.


Wenn aus irgendeinem Grund, ohne Administratorrechte auszuführen versucht die `RowDeleting` Ereignishandler oder wenn ein nicht Supervisor oder Nichtadministrator-Versuche zum Ausführen der `RowUpdating` Ereignishandler, d. h. der .NET Common Language Runtime löst eine `SecurityException`.


[![Wenn der Sicherheitskontext zum Ausführen der Methode nicht autorisiert ist, wird eine SecurityException ausgelöst.](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Abbildung 16**: Wenn der Sicherheitskontext nicht, zur Ausführung der Methode autorisiert ist ein `SecurityException` ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](role-based-authorization-vb/_static/image48.png))


Zusätzlich zu ASP.NET-Seiten haben viele Anwendungen auch eine Architektur, die verschiedenen Ebenen, wie z. B. von Geschäftslogik und Datenzugriffsebenen Daten enthält. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftsfunktionen Logik und Daten, die verknüpft. Die `PrincipalPermission` Attribut ist zum Übernehmen von Autorisierungsregeln für diese Ebenen ebenfalls hilfreich.

Weitere Informationen zur Verwendung der `PrincipalPermission` Attribut Autorisierungsregeln für Klassen und Methoden definieren, finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [Hinzufügen von Autorisierungsregeln zu Business und Ebenen mithilfe`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm erläutert, wie grob angegeben und Autorisierungsregeln möglich, differenzierte auf Grundlage der Rollen des Benutzers. ASP IST. NET URL Authorization-Feature ermöglicht den Entwickler einer Seite, um anzugeben, welche Identitäten gestattet oder verweigert den Zugriff auf welche Seiten. Wie wir gesehen, wieder in haben die <a id="_msoanchor_10"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Tutorial, URL-Autorisierung, die Regeln auf Grundlage von Benutzer angewendet werden können. Sie können auch auf der Grundlage einer Rolle von Rolle angewendet werden, wie in Schritt 1 dieses Lernprogramms wurde erläutert.

Autorisierungsregeln möglich, differenzierte können deklarativ oder programmgesteuert angewendet werden. In Schritt2 betrachtet das LoginView-Steuerelement RoleGroups-Funktion verwenden, um andere Ausgabe basierend auf der Website besucht Benutzerrollen zu rendern. Wir haben uns auch bei Methoden, um programmgesteuert zu ermitteln, wenn ein Benutzer gehört einer bestimmten Rolle und wie Sie die Funktionen der Seite entsprechend anpassen.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungsregeln für Geschäft und Ebenen der Daten mithilfe von`PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profil: Arbeiten mit Rollen](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Frage Sicherheitsliste für ASP.NET 2.0](https://msdn.microsoft.com/en-us/library/ms998375.aspx)
- [Technische Dokumentation für die `<roleManager>` Element](https://msdn.microsoft.com/en-us/library/ms164660.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm enthalten Suchi Banerjee und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](assigning-roles-to-users-vb.md)
