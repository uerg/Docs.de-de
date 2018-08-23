---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-cs
title: Rollenbasierte Autorisierung (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Tutorial beginnt mit einem Blick auf wie das Framework Rollen die Rollen eines Benutzers mit seinem Sicherheitskontext zuordnet. Klicken Sie dann untersucht rollenbasierte URL anwenden...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 4d9b63fa-c3d4-4e85-82b1-26ae3ba3ca1c
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f947e35164724b99507858a19bdd9cd1154768a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836242"
---
<a name="role-based-authorization-c"></a>Rollenbasierte Autorisierung (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.11.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_cs.pdf)

> Dieses Tutorial beginnt mit einem Blick auf wie das Framework Rollen die Rollen eines Benutzers mit seinem Sicherheitskontext zuordnet. Klicken Sie dann untersucht wie URL-Autorisierungsregeln rollenbasierte angewendet. Befolgen, dass wir mithilfe von deklarativen und eine programmgesteuerte Möglichkeit zum Ändern der angezeigten Daten und die Funktionalität von einer ASP.NET-Seite aussehen wird.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial wurde erläutert, wie URL-Autorisierung zu verwenden, um anzugeben, welche Benutzer eine bestimmte Gruppe von Seiten besuchen können. Mit nur wenig von Markup im `Web.config`, wir könnten ASP.NET anzuweisen, nur authentifizierte Benutzer zum Besuch einer Seite zu ermöglichen. Oder wir können vorgeben, dass nur die Benutzer Tito und Bob anzugeben, dass alle authentifizierten Benutzer mit Ausnahme von Sam zulässig wären.

Zusätzlich zu der URL-Autorisierung haben Sie auch deklarativ und programmatisch Verfahren zum Steuern der angezeigten Daten und die Funktionalität von einer Seite, die basierend auf dem Benutzer Zugriff auf. Insbesondere haben wir eine Seite, die den Inhalt des aktuellen Verzeichnisses aufgeführt. Jeder Benutzer kann auf dieser Seite finden Sie unter, aber nur authentifizierte Benutzer können den Inhalt der Dateien anzuzeigen, und nur Tito konnte die Dateien gelöscht werden.

Anwenden von Autorisierungsregeln pro Benutzer mit dem Benutzernamen kann in einem Albtraum Problematik anwachsen. Ein besser verwaltbaren Ansatz ist die Verwendung der rollenbasierten Autorisierung. Die gute Nachricht ist, dass die Tools zur Verfügung zum Anwenden von Autorisierungsregeln genauso funktionieren gut mit Rollen, wie bei Benutzerkonten. URL-Autorisierungsregeln können Rollen und nicht für Benutzer angeben. Das LoginView-Steuerelement, das andere Ausgabe für authentifizierte und anonyme Benutzer gerendert wird, kann für die unterschiedlichen Inhalt basierend auf den angemeldeten Benutzer Rollen anzuzeigen konfiguriert werden. Und die Rollen-API enthält Methoden zum Bestimmen der Rollen des angemeldeten Benutzers.

Dieses Tutorial beginnt mit einem Blick auf wie das Framework Rollen die Rollen eines Benutzers mit seinem Sicherheitskontext zuordnet. Klicken Sie dann untersucht wie URL-Autorisierungsregeln rollenbasierte angewendet. Befolgen, dass wir mithilfe von deklarativen und eine programmgesteuerte Möglichkeit zum Ändern der angezeigten Daten und die Funktionalität von einer ASP.NET-Seite aussehen wird. Fangen wir an!

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Grundlegendes zu wie Rollen zugeordnet sind Sicherheitskontext des Benutzers

Wenn eine Anforderung die ASP.NET-Pipeline erreicht, ist es mit einem Sicherheitskontext, einschließlich Informationen zur Bestimmung des anforderers verknüpft. Wenn Sie die Formularauthentifizierung verwenden, wird ein Authentifizierungsticket als ein Identitätstoken verwendet. Wie wir unter den <a id="_msoanchor_2"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) und <a id="_msoanchor_3"> </a> [ *Formulare Authentifizierungskonfiguration und fortgeschrittenen Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) Tutorials, die `FormsAuthenticationModule` dient zum Bestimmen der Identität des anforderers, was während geschieht der [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Wenn ein gültiger, nicht abgelaufenen Authentifizierungsticket gefunden wird, die `FormsAuthenticationModule` decodiert, um die Identität des Antragstellers zu ermitteln. Er erstellt ein neues `GenericPrincipal` -Objekt und weist diese Option, um die `HttpContext.User` Objekt. Der Zweck eines Prinzipals, wie `GenericPrincipal`, besteht darin, identifizieren Sie den Namen des authentifizierten Benutzers und welche Rollen sie angehören. Dazu wird durch die Tatsache, die alle Prinzipalobjekte offensichtlich eine `Identity` Eigenschaft und eine `IsInRole(roleName)` Methode. Die `FormsAuthenticationModule`, jedoch nicht von Bedeutung ist beim Aufzeichnen von Informationen und die `GenericPrincipal` Objekt erstellt, gibt keinen Rollen.

Wenn das Framework für die Rollen aktiviert ist, die [ `RoleManagerModule` ](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) HTTP-Modul Schritte nach der die `FormsAuthenticationModule` und identifiziert den authentifizierten Benutzer Rollen während der [ `PostAuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), wird ausgelöst, nachdem die `AuthenticateRequest` Ereignis. Wenn die Anforderung von einem authentifizierten Benutzer ist, wird die `RoleManagerModule` überschreibt die `GenericPrincipal` Objekt erstellt wurde, indem Sie die `FormsAuthenticationModule` und ersetzt diese durch eine [ `RolePrincipal` Objekt](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). Die `RolePrincipal` Klasse verwendet die Rollen-API, um zu bestimmen, welche Rollen der Benutzer angehört.

Abbildung 1 zeigt den ASP.NET-Pipeline-Workflow, bei der Formularauthentifizierung und die Rollen-Framework verwenden. Die `FormsAuthenticationModule` wird zuerst ausgeführt, wird der Benutzer über ihr Authentifizierungsticket identifiziert und erstellt ein neues `GenericPrincipal` Objekt. Als Nächstes die `RoleManagerModule` ein und überschreibt die `GenericPrincipal` Objekt mit einem `RolePrincipal` Objekt.

Wenn ein anonymer Benutzer die Website besucht, die weder der `FormsAuthenticationModule` noch die `RoleManagerModule` ein Prinzipalobjekt erstellt.


[![Die Ereignisse der ASP.NET-Pipeline für einen authentifizierten Benutzer bei Verwendung der Formularauthentifizierung und die Rollen-Framework](role-based-authorization-cs/_static/image2.png)](role-based-authorization-cs/_static/image1.png)

**Abbildung 1**: die ASP.NET-Pipelineereignisse für einen authentifizierten Benutzer beim Verwenden der Formularauthentifizierung und die Rollen-Framework ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image3.png))


### <a name="caching-role-information-in-a-cookie"></a>Rolleninformationen zum Zwischenspeichern in einem Cookie

Die `RolePrincipal` des Objekts `IsInRole(roleName)` Methodenaufrufe `Roles.GetRolesForUser` zum Abrufen der Rollen für den Benutzer, um zu bestimmen, ob der Benutzer ein Mitglied *RoleName*. Bei Verwendung der `SqlRoleProvider`, dies führt zu einer Abfrage mit die Rolle-Informationsspeicher-Datenbank. Bei Verwendung von URL-Autorisierungsregeln rollenbasierte der `RolePrincipal`des `IsInRole` aufgerufene Methode bei jeder Anforderung an eine Seite, die durch die rollenbasierte URL-Autorisierungsregeln geschützt ist. Anstatt bei jeder Anforderung der Informationen in der Datenbank gesucht haben, enthält die Rollen-Framework eine Option aus, um die Rollen des Benutzers in einem Cookie zwischengespeichert.

Wenn das Framework für die Rollen für die Rollen des Benutzers in einem Cookie zwischengespeichert konfiguriert ist die `RoleManagerModule` erstellt das Cookie während der ASP.NET-Pipeline [ `EndRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx). Dieses Cookie wird verwendet, in nachfolgenden Anforderungen in der `PostAuthenticateRequest`, d.h. wenn der `RolePrincipal` Objekt wird erstellt. Wenn das Cookie gültig ist und nicht abgelaufen ist, die Daten in das Cookie analysiert und verwendet, um die Rollen des Benutzers, und Speichern von füllen die `RolePrincipal` erspart, um einen Aufruf an die `Roles` Klasse, um die Rollen des Benutzers zu bestimmen. Abbildung 2 zeigt diesen Workflow.


[![Die Informationen des Benutzers-Rolle kann in einem Cookie zur Verbesserung der Leistung gespeichert werden](role-based-authorization-cs/_static/image5.png)](role-based-authorization-cs/_static/image4.png)

**Abbildung 2**: der Benutzer die Rolle Daten gespeichert werden können in einem Cookie zum Verbessern der Leistung ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image6.png))


Standardmäßig ist die Role-Cache-Cookie-Mechanismus deaktiviert. Es kann aktiviert werden, über die `<roleManager>` konfigurationsmarkup in `Web.config`. Erläutert, mit der [ `<roleManager>` Element](https://msdn.microsoft.com/library/ms164660.aspx) an Rollenanbieter in der <a id="_msoanchor_4"> </a> [ *erstellen und Verwalten von Rollen* ](creating-and-managing-roles-cs.md) Tutorial Damit Sie dieses Element bereits in Ihrer Anwendungsverzeichnis haben soll `Web.config` Datei. Die Cookie-Einstellungen für die Rolle Cache werden als Attribute angegeben, dass die `<roleManager>` Element, und sind in Tabelle 1 zusammengefasst werden.

> [!NOTE]
> Die Konfigurationseinstellungen, die in Tabelle 1 aufgeführten Geben Sie die Eigenschaften des sich ergebenden Rollencookie Cache. Weitere Informationen zu Cookies, wie sie funktionieren und die verschiedenen Eigenschaften zu erhalten, lesen [in diesem Tutorial Cookies](http://www.quirksmode.org/js/cookies.html).


| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Beschreibung</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Ein boolescher Wert, der angibt, ob das Zwischenspeichern von Cookies verwendet wird. Wird standardmäßig auf `false` festgelegt.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Der Name des Cache Rollencookie. Der Standardwert ist ". ASPXROLES".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                Der Pfad für den Namen des Rollencookies. Das Path-Attribut kann ein Entwickler den Umfang eines Cookies zu einer bestimmten Verzeichnishierarchie zu beschränken. Der Standardwert ist "/", die informiert des Browsers, um das Ticket Authentifizierungscookie an alle Anforderungen an die Domäne zu senden.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Gibt an, welche Verfahren verwendet werden, um Cache Rollencookie zu schützen. Die zulässigen Werte sind: `All` (Standard); `Encryption`; `None`; und `Validation`. Siehe Schritt 3 des der <a id="_anchor_5"> </a> [ *Konfiguration der Formularauthentifizierung und Weiterführende Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) Tutorial Weitere Informationen zu den Schutzstufen.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Ein boolescher Wert, der angibt, ob eine SSL-Verbindung erforderlich ist, um das Authentifizierungscookie zu übermitteln. Der Standardwert ist `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Ein boolescher Wert, der angibt, ob das Cookie des Timeout jedes Mal zurückgesetzt, wird der Benutzer die Website während einer Sitzung besucht. Der Standardwert ist `false`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` nastaven NA hodnotu `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Gibt die Zeit in Minuten an, nach denen das Ticket Authentifizierungscookie abläuft. Der Standardwert ist `30`. Dieser Wert ist nur relevant, wenn `createPersistentCookie` nastaven NA hodnotu `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Ein boolescher Wert, der angibt, ob die Rolle Cache Cookie ein Sitzungscookie oder dauerhaftes Cookie. Wenn `false` (Standardeinstellung), ein Sitzungscookie wird verwendet, die gelöscht, wenn der Browser geschlossen wird. Wenn `true`, ein permanentes Cookie wird verwendet; es abgelaufen `cookieTimeout` Anzahl der Minuten nach ihrer Erstellung oder nach der vorherigen Besuch, abhängig vom Wert `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Gibt an, der Wert des Cookies Domäne. Der Standardwert ist eine leere Zeichenfolge und bewirkt, dass der Browser mit der Domäne, von der es (z. B. www.yourdomain.com) ausgestellt wurde. In diesem Fall das Cookie wird <strong>nicht</strong> gesendet werden, wenn die Erstellung von Unterdomänen, z. B. admin.yourdomain.com Anforderungen. Wenn das Cookie an alle Unterdomänen übergeben werden soll, müssen Sie zum Anpassen der `domain` -Attribut, wenn diese Option auf "ihredomaene.com".                                                                                                                                                 |
|    `maxCachedResults`     | Gibt die maximale Anzahl von Rollennamen, aus denen, die zwischengespeichert werden in das Cookie an. Der Standard ist 25. Die `RoleManagerModule` erstellt ein Cookie für Benutzer, die zu gehören nicht mehr als `maxCachedResults` Rollen. Daher die `RolePrincipal` des Objekts `IsInRole` Methode verwendet die `Roles` Klasse, um die Rollen des Benutzers zu bestimmen. Der Grund `maxCachedResults` vorhanden ist, da viele Benutzer-Agents nicht größer als 4.096 Bytes Cookies zulassen. Dieses Endes soll also diese größenbeschränkung überschreiten die Wahrscheinlichkeit zu reduzieren. Wenn Sie sehr lange Rollennamen haben, möchten Sie möglicherweise Geben Sie ggf. eine kleinere `maxCachedResults` Wert; contrariwise, wenn Sie sehr kurz Rollennamen verfügen, können Sie wahrscheinlich erhöhen dieses Werts. |

**Tabelle 1:** die Role-Cache Cookie-Konfigurationsoptionen

Konfigurieren wir unsere Anwendung zur Verwendung von nicht persistenten Cache Rollencookies. Aktualisieren Sie zu diesem Zweck die `<roleManager>` Element im `Web.config` auf die folgenden Cookie-bezogene Attribute enthalten:

[!code-xml[Main](role-based-authorization-cs/samples/sample1.xml)]

Ich habe aktualisiert die `<roleManager>` Elements durch Hinzufügen von drei Attribute: `cacheRolesInCookie`, `createPersistentCookie`, und `cookieProtection`. Durch Festlegen von `cacheRolesInCookie` zu `true`, `RoleManagerModule` nun automatisch die Rollen des Benutzers in einem Cookie zwischengespeichert wird, anstatt zum Suchen von Informationen für jede Anforderung des Benutzers. Ich explizit festlegen der `createPersistentCookie` und `cookieProtection` Attribute `false` und `All`bzw. Technisch gesehen, ich möchte nicht angeben, dass die Werte für diese Attribute aus, da ich nur werden auf ihre Standardwerte zurück zugewiesen, aber ich diese hier um erleichtern explizit löschen, verwende ich nicht permanente Cookies und ob das Cookie sowohl verschlüsselt und überprüft.

Das ist alles schon! Damit werden die Rollen-Framework werden die Benutzerrollen in Cookies zwischengespeichert. Wenn der Browser des Benutzers keine Cookies unterstützt oder ihre Cookies gelöscht wird oder verloren gegangen sind, irgendwie, es keine große Sache ist – der `RolePrincipal` -Objekt einfach verwendet die `Roles` Klasse in der Fall, dass kein Cookie (oder eine ungültige oder abgelaufene) verfügbar ist.

> [!NOTE]
> Der Microsoft Patterns &amp; Practices-Gruppe wird eine verhindert die Verwendung von persistenten Rolle Cache Cookies. Da der Cache Rollencookie besitzt ausreichend, um die Mitgliedschaft in Datenbankrolle, zu bestätigen ist, wenn ein Hacker irgendwie eines gültigen Benutzers Cookie zugreifen kann, kann er diesen Benutzer imitieren. Die Wahrscheinlichkeit, dass dies seltener geschieht nimmt zu, wenn das Cookie auf den Browser des Benutzers beibehalten wird. Weitere Informationen zu dieser sicherheitsempfehlung als auch andere Sicherheitsaspekte, finden Sie in der [Frage Sicherheitsliste für ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx).


## <a name="step-1-defining-role-based-url-authorization-rules"></a>Schritt 1: Definieren von rollenbasierten URL-Autorisierungsregeln

Siehe die <a id="_msoanchor_6"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial URL-Autorisierung bietet eine Möglichkeit, den Zugriff auf eine Reihe von Seiten auf einem Benutzer nach Benutzer oder Rolle-von-Role einschränken Grundlage. Die URL-Autorisierungsregeln ausgeschrieben werden `Web.config` mithilfe der [ `<authorization>` Element](https://msdn.microsoft.com/library/8d82143t.aspx) mit `<allow>` und `<deny>` untergeordnete Elemente. Zusätzlich zu den benutzerbezogenen Autorisierungsregeln, die in vorherigen Tutorials behandelt jede `<allow>` und `<deny>` untergeordnete Element kann auch enthalten:

- Eine bestimmte Rolle
- Eine durch Trennzeichen getrennte Liste von Rollen

Klicken Sie beispielsweise die URL-Autorisierungsregeln gewähren Zugriff auf die Benutzer in den Rollen-Administratoren "und" Supervisor, aber Verweigern des Zugriffs auf alle anderen:

[!code-xml[Main](role-based-authorization-cs/samples/sample2.xml)]

Die `<allow>` Element in das obenstehende Markup gibt an, dass die Administratoren und Supervisor Rollen zulässig sind; die `<deny>` Element anweist, dass *alle* Benutzern verweigert werden.

Konfigurieren wir unsere Anwendung, damit die `ManageRoles.aspx`, `UsersAndRoles.aspx`, und `CreateUserWizardWithRoles.aspx` Seiten sind nur verfügbar, auf die Benutzer in der Rolle "Administratoren", während die `RoleBasedAuthorization.aspx` Seite ist für alle Besucher zugänglich sind.

Um dies zu erreichen, starten Sie durch das Hinzufügen einer `Web.config` -Datei in die `Roles` Ordner.


[![Fügen Sie eine Web.config-Datei in das Verzeichnis für Rollen](role-based-authorization-cs/_static/image8.png)](role-based-authorization-cs/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` -Datei in die `Roles` Verzeichnis ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image9.png))


Fügen Sie das folgende konfigurationsmarkup zu `Web.config`:

[!code-xml[Main](role-based-authorization-cs/samples/sample3.xml)]

Die `<authorization>` Element in der `<system.web>` Abschnitt gibt an, dass nur Benutzer in der Rolle "Administratoren" ASP.NET-Ressourcen in zugreifen können die `Roles` Verzeichnis. Die `<location>` Element definiert eine Alternative Gruppe von URL-Autorisierungsregeln für den `RoleBasedAuthorization.aspx` Seite, sodass alle Benutzer die Seite besuchen.

Nach dem Speichern Ihrer Änderungen an `Web.config`, melden Sie sich als ein Benutzer, der nicht in der Rolle "Administratoren" ist, und wiederholen Sie dann eine der geschützten Seiten besuchen. Die `UrlAuthorizationModule` erkennt, dass Sie nicht berechtigt sind, besuchen die angeforderte Ressource; daher die `FormsAuthenticationModule` werden Sie zur Anmeldeseite umgeleitet. Die Anmeldeseite leitet dann Sie die `UnauthorizedAccess.aspx` Seite (siehe Abbildung 4). Diese letzte Umleitung von der Anmeldeseite zum `UnauthorizedAccess.aspx` tritt aufgrund der Code, die wir, auf die Anmeldeseite in Schritt2 hinzugefügt der <a id="_msoanchor_7"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial. Insbesondere, leitet die Anmeldeseite automatisch allen authentifizierten Benutzern `UnauthorizedAccess.aspx` die Abfragezeichenfolge enthält eine `ReturnUrl` Parameter als dieser Parameter gibt an, dass der Benutzer die Anmeldeseite nach dem Versuch, eine Seite angezeigt, er nicht war, eingetroffen Berechtigung zum Anzeigen.


[![Nur Benutzer in der Administratorrolle können geschützten Seiten anzeigen.](role-based-authorization-cs/_static/image11.png)](role-based-authorization-cs/_static/image10.png)

**Abbildung 4**: nur Benutzer in der Rolle "Administratoren" können die geschützte Seiten anzeigen ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image12.png))


Melden Sie sich ab und melden Sie sich als Benutzer, der in der Rolle "Administratoren" ist. Jetzt sollten Sie die drei geschützten Seiten anzeigen können.


[![Tito finden befindet sich die UsersAndRoles.aspx Seite da er in der Rolle "Administratoren"](role-based-authorization-cs/_static/image14.png)](role-based-authorization-cs/_static/image13.png)

**Abbildung 5**: Tito Besuchen der `UsersAndRoles.aspx` Seite da er befindet sich in der Rolle "Administratoren" ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image15.png))


> [!NOTE]
> Beim Angeben von URL-Autorisierungsregeln: für Rollen oder Benutzer – ist es wichtig zu bedenken, die die Regeln analysierten jeweils nur ein, von oben nach unten. Sobald eine Übereinstimmung gefunden wird, je nach auf, wenn die Übereinstimmung der Benutzer wird Zugriff gewährt oder verweigert, ein `<allow>` oder `<deny>` Element. **Wenn keine Übereinstimmung gefunden wird, wird der Benutzer Zugriff gewährt.** Daher sollten Sie den Zugriff auf ein oder mehrere Benutzerkonten einzuschränken, ist es zwingend erforderlich, dass Sie verwenden eine `<deny>` Element als das letzte Element in der Konfiguration der URL-Autorisierung. **Wenn nicht von URL-Autorisierungsregeln verwenden eine**`<deny>`**-Element, allen Benutzern Zugriff gewährt.** Eine ausführlichere Erläuterung wie die URL-Autorisierungsregeln analysiert werden, finden Sie wieder in die "untersuchen, wie die `UrlAuthorizationModule` die Autorisierungsregeln zum gewähren oder Verweigern des Zugriffs verwendet" im Abschnitt der <a id="_msoanchor_8"> </a> [  *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial.


## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Schritt 2: Funktionalität, die auf Basis des aktuell angemeldeten Benutzers der Rollen beschränkt.

URL-Autorisierung macht es einfach an grob Autorisierung Regeln diesen Zustand an, welche Identitäten sind zulässig und werden, welche von der Anzeige einer bestimmten Seite (oder alle Seiten in einem Ordner und seine Unterordner) verweigert. Möglicherweise möchten wir jedoch in bestimmten Fällen ermöglicht allen Benutzern von finden Sie auf eine Seite, aber der Seite-Funktionen, die basierend auf der Website besucht Rollen des Benutzers zu beschränken. Dies kann zur Folge, anzeigen oder Ausblenden von Daten basierend auf die Rolle des Benutzers oder bietet zusätzliche Funktionalität für Benutzer, die zu einer bestimmten Rolle gehören.

Solche eine differenzierte rollenbasierte Autorisierungsregeln können entweder deklarativ oder programmgesteuert (oder über eine Kombination aus beiden) implementiert werden. Im nächsten Abschnitt sehen wir, wie deklarative eine differenzierte Autorisierung über das LoginView-Steuerelement zu implementieren. Danach werden wir den programmgesteuerten Methoden untersuchen. Bevor wir die Anwendung eine differenzierte Autorisierungsregeln sehen können, müssen jedoch wir eine Seite erstellen, deren Funktionalität für die Rolle des Benutzers Zugriff auf diese abhängig ist.

Wir erstellen eine Seite, die alle Benutzerkonten im System in einer GridView-Ansicht aufgelistet sind. Das GridView enthalten Benutzernamen, e-Mail-Adresse, Datum der letzten Anmeldung und Kommentare über den Benutzer des Benutzers. Zusätzlich zur Anzeige der Informationen des Benutzers, wird die GridView bearbeiten und Löschen von Funktionen. Zunächst erstellen Sie diese Seite mit der Bearbeitung und Löschfunktionen, die allen Benutzern zur Verfügung. In den Abschnitten "Verwenden der LoginView-Steuerelement" und "Programmgesteuert beschränken Funktionalität" sehen wir, aktivieren oder deaktivieren Sie diese Funktionen, die basierend auf der Website besucht Rolle des Benutzers.

> [!NOTE]
> Die ASP.NET-Seite, die wir nun erstellen Sie verwendet ein GridView-Steuerelement, um die Benutzerkonten anzuzeigen. Da dieses Tutorial die Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen Reihe Schwerpunkt möchte nicht ich erläutern die Innenleben des GridView-Steuerelements zu viel Zeit. In diesem Tutorial bietet eine bestimmte schrittweise Anweisungen zum Einrichten dieser Seite ist es nicht detailliert die Details, warum die Auswahl bestimmter vorgenommen wurden, oder welche Auswirkung bestimmte Eigenschaften auf der gerenderten Ausgabe verfügen. Für eine gründliche Prüfung des GridView-Steuerelements, sehen Sie sich meine *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Tutorial-Reihe.


Öffnen Sie zunächst die `RoleBasedAuthorization.aspx` auf der Seite die `Roles` Ordner. Ziehen Sie von einer GridView-Ansicht auf der Seite auf den Designer und den Satz der `ID` zu `UserGrid`. In Kürze schreiben wir Code, der Aufrufe der `Membership.GetAllUsers` Methode und bindet die resultierende `MembershipUserCollection` Objekt an die GridView. Die `MembershipUserCollection` enthält eine `MembershipUser` Objekt für jedes Benutzerkonto in Systems festgelegt wird. `MembershipUser` Objekte verfügen über Eigenschaften wie `UserName`, `Email`, `LastLoginDate`und so weiter.

Bevor wir den Code, der die Benutzerkonten in das Raster gebunden wird schreiben, lassen Sie uns zuerst definieren Sie den GridView Felder. Klicken Sie in des GridView Smarttag auf den Link "Spalten bearbeiten", um das Dialogfeld "Felder" zu starten (siehe Abbildung 6). Deaktivieren Sie das Kontrollkästchen "Felder automatisch generieren" in der unteren linken Ecke von hier aus. Da wir möchten diese GridView enthalten, bearbeiten und Löschen von Funktionen, eine CommandField hinzufügen und Festlegen der `ShowEditButton` und `ShowDeleteButton` Eigenschaften auf "true". Fügen Sie vier Felder für die Anzeige der `UserName`, `Email`, `LastLoginDate`, und `Comment` Eigenschaften. Verwenden Sie für die zwei schreibgeschützten Eigenschaften einer BoundField (`UserName` und `LastLoginDate`) und von TemplateFields für die beiden bearbeitbaren Felder (`Email` und `Comment`).

Haben Sie die erste BoundField-Anzeige der `UserName` Eigenschaft; Set seine `HeaderText` und `DataField` Eigenschaften auf "UserName". Dieses Feld nicht bearbeitet werden, legen Sie daher die `ReadOnly` Eigenschaft auf "true". Konfigurieren der `LastLoginDate` BoundField durch Festlegen der `HeaderText` in "Letzten Login" und die zugehörige `DataField` auf "" LastLoginDate". Wir Formatieren der Ausgabe dieses BoundField, sodass nur das Datum (anstelle von Datum und Uhrzeit) angezeigt wird. Um dies zu erreichen, legen Sie diese BoundField des `HtmlEncode` Eigenschaft auf "false" und die zugehörige `DataFormatString` Eigenschaft "{0:d}". Legen Sie auch die `ReadOnly` Eigenschaft auf "true".

Legen Sie die `HeaderText` Eigenschaften der beiden von TemplateFields "Email" und "Kommentar".


[![GridView Felder kann konfiguriert werden, über die Felder (Dialogfeld)](role-based-authorization-cs/_static/image17.png)](role-based-authorization-cs/_static/image16.png)

**Abbildung 6**: der GridView Felder können werden konfiguriert über die Felder (Dialogfeld) ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image18.png))


Jetzt müssen wir definieren die `ItemTemplate` und `EditItemTemplate` für den "Email" und "Kommentar" von TemplateFields. Fügen Sie ein Label-Steuerelement, auf die einzelnen der `ItemTemplate` s und binden ihrer `Text` Eigenschaften, die die `Email` und `Comment` Eigenschaften, bzw.

Für das TemplateField "Email", fügen Sie ein Textfeld mit dem Namen `Email` auf seine `EditItemTemplate` und binden die `Text` Eigenschaft, um die `Email` Eigenschaft, die bidirektionale Datenbindung verwenden. Hinzufügen eines RequiredFieldValidator und RegularExpressionValidator auf die `EditItemTemplate` um sicherzustellen, dass ein Besucher, bearbeiten die e-Mail-Eigenschaft eine gültige e-Mail-Adresse eingegeben hat. Fügen Sie einem mehrzeiligen Textfeld mit dem Namen für das TemplateField "Kommentar" `Comment` auf seine `EditItemTemplate`. Textfeldss festgelegt `Columns` und `Rows` Eigenschaften 40 und 4, und binden dann seine `Text` Eigenschaft, um die `Comment` Eigenschaft, die bidirektionale Datenbindung verwenden.

Nachdem diese von TemplateFields konfiguriert haben, sollte ihre deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](role-based-authorization-cs/samples/sample4.aspx)]

Beim Bearbeiten oder Löschen eines Benutzerkontos müssen wir wissen, des Benutzers `UserName` -Eigenschaftswert. Legen Sie des GridView `DataKeyNames` Eigenschaft auf "UserName" so, dass diese Informationen über des GridView `DataKeys` Auflistung.

Abschließend fügen Sie auf der Seite ein ValidationSummary-Steuerelement hinzu, und legen Sie seine `ShowMessageBox` Eigenschaft auf "true" und die zugehörige `ShowSummary` Eigenschaft auf "false". Mit diesen Einstellungen wird die ValidationSummary eine clientseitige Benachrichtigung angezeigt, wenn der Benutzer versucht, ein Benutzerkonto mit einem fehlenden oder ungültigen e-Mail-Adresse bearbeiten.

[!code-aspx[Main](role-based-authorization-cs/samples/sample5.aspx)]

Wir haben jetzt deklaratives Markup der Seite abgeschlossen. Unsere nächste Aufgabe besteht darin, den Satz von Benutzerkonten an die GridView zu binden. Fügen Sie eine Methode mit dem Namen `BindUserGrid` auf die `RoleBasedAuthorization.aspx` Seite des Code-Behind-Klasse, bindet der `MembershipUserCollection` zurückgegebenes `Membership.GetAllUsers` auf die `UserGrid` GridView. Rufen Sie diese Methode aus der `Page_Load` Ereignishandler auf der Seite zum ersten Mal besuchen.

[!code-csharp[Main](role-based-authorization-cs/samples/sample6.cs)]

Finden Sie mit diesem Code werden auf der Seite über einen Browser. Wie in Abbildung 7 dargestellt, sollte eine GridView Auflisten von Informationen zu den einzelnen Benutzerkonten im System angezeigt werden.


[![Das UserGrid GridView listet Informationen zu jedem Benutzer im System](role-based-authorization-cs/_static/image20.png)](role-based-authorization-cs/_static/image19.png)

**Abbildung 7**: die `UserGrid` GridView listet Informationen zu jeder Benutzer im System ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image21.png))


> [!NOTE]
> Die `UserGrid` GridView zeigt eine Liste aller Benutzer in einem nicht ausgelagerten-Schnittstelle. Diese einfache Grid-Schnittstelle ist nicht für Szenarien geeignet, mehrere Dutzend oder mehr Benutzer vorhanden sind. Eine Möglichkeit ist die GridView zum Aktivieren von Paging zu konfigurieren. Die `Membership.GetAllUsers` Methode verfügt über zwei Überladungen: eine, die keine Eingabeparameter akzeptiert und gibt alle Benutzer und eine, die ganzzahlige Werte für die Seite "Index" und "Seitengröße verwendet und gibt nur die angegebene Teilmenge der Benutzer zurück. Die zweite Überladung kann effizienter Seite über die Benutzer verwendet werden, da wird nur die genaue Teilmenge von Benutzerkonten statt *alle* davon. Wenn Sie Tausende von Benutzerkonten verfügen, empfiehlt es sich, eine filterbasierten-Schnittstelle, eine zu berücksichtigen, die nur die Benutzer angezeigt, deren Benutzername mit einem ausgewählten Zeichen, z. B. beginnt. Die [ `Membership.FindUsersByName method` ](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) eignet sich ideal für das Erstellen einer Filter-basierte Benutzeroberfläche. Betrachten wir eine solche Schnittstelle in einem späteren Tutorial erstellen.


Das GridView-Steuerelement bietet integrierte bearbeiten und Löschen von Support, wenn das Steuerelement an ein konfigurierter Datenquellen-Steuerelement, z. B. die SqlDataSource-Steuerelement oder das ObjectDataSource-Steuerelement gebunden ist. Die `UserGrid` GridView, hat jedoch die Daten, die programmgesteuert gebunden; aus diesem Grund müssen wir schreiben, Code, um diese beiden Aufgaben auszuführen. Wir müssen vor allem für das Erstellen von Ereignishandlern für der GridView `RowEditing`, `RowCancelingEdit`, `RowUpdating`, und `RowDeleting` -Ereignisse, die ausgelöst werden, wenn Besucher einer GridView klickt bearbeiten "," Abbrechen "," aktualisieren, oder Löschen von Schaltflächen.

Zunächst erstellen Sie die Ereignishandler für der GridView `RowEditing`, `RowCancelingEdit`, und `RowUpdating` Ereignisse und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample7.cs)]

Die `RowEditing` und `RowCancelingEdit` Ereignishandler legen Sie einfach des GridView `EditIndex` -Eigenschaft, und klicken Sie dann die Liste der Benutzer in das Raster Konten binden. Die interessanter erfolgt in der `RowUpdating` -Ereignishandler. Zunächst, dass dieser Ereignishandler stellt sicher, dass die Daten gültig ist, und klicken Sie dann ruft der `UserName` Wert des bearbeiteten Benutzerkontos aus der `DataKeys` Auflistung. Die `Email` und `Comment` TextBox-Elementen in der von zwei TemplateFields `EditItemTemplate` s dann programmgesteuert auf die verwiesen wird. Ihre `Text` Eigenschaften enthalten, die bearbeitete e-Mail-Adresse, und kommentieren.

Zum Aktualisieren eines Benutzerkontos über Membership-API müssen wir zunächst die Informationen des Benutzers, die wir über einen Aufruf an `Membership.GetUser(userName)`. Das zurückgegebene `MembershipUser` des Objekts `Email` und `Comment` Eigenschaften werden dann mit den Werten in die beiden Textfelder ein eingegeben werden, auf die Bearbeitungsschnittstelle aktualisiert. Abschließend werden diese Änderungen gespeichert, mit einem Aufruf von [ `Membership.UpdateUser` ](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). Die `RowUpdating` -Ereignishandler, die durch das Wiederherstellen der GridView vorab bearbeiten die Benutzeroberfläche abgeschlossen ist.

Erstellen Sie als Nächstes die `RowDeleting` -Ereignishandler und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample8.cs)]

Der oben genannten Ereignishandler startet am einfachsten mit der `UserName` Wert aus der GridView `DataKeys` Auflistung; dies `UserName` Wert wird dann in der Mitgliedschaft-Klasse übergeben [ `DeleteUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx). Die `DeleteUser` Methode löscht das Benutzerkonto aus dem System, einschließlich der zugehörigen Mitgliedschaftsdaten (wie z. B. welche Rollen dieser Benutzer gehört). Nach dem Löschen des Benutzers, dem Raster `EditIndex` auf-1 festgelegt ist (für den Fall, dass der Benutzer löschen geklickt hat, während eine andere Zeile im Bearbeitungsmodus war) und die `BindUserGrid` Methode wird aufgerufen.

> [!NOTE]
> Die Schaltfläche "löschen" ist keine Art von Bestätigung durch den Benutzer vor dem Löschen des Benutzerkontos erforderlich. Sollten Sie fügen eine Form der Bestätigung durch den Benutzer zu verringern, die Wahrscheinlichkeit, dass ein Konto versehentlich gelöscht wird. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen ist über ein Dialogfeld für die clientseitige bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-cs.aspx).


Stellen Sie sicher, dass diese Seite wie erwartet funktioniert. Sie sollten in der Lage, bearbeiten die e-Mail-Adresse des Benutzers und der Kommentar als auch ein Benutzerkonto zu löschen. Da die `RoleBasedAuthorization.aspx` Seite für alle Benutzer zugänglich ist, werden alle Benutzer – auch anonyme Besucher – kann finden Sie auf dieser Seite und bearbeiten und Löschen von Benutzerkonten. Diese Seite aktualisieren wir damit, dass nur Benutzer in den Rollen Vorgesetzte und Administratoren können die e-Mail-Adresse eines Benutzers und Kommentar bearbeiten, und nur Administratoren können ein Benutzerkonto löschen.

Im Abschnitt "Verwenden der LoginView-Steuerelement" sucht mit dem LoginView-Steuerelement in Anweisungen für die Rolle des Benutzers angezeigt werden. Wenn eine Person in der Rolle "Administratoren" auf dieser Seite besucht, wird gezeigt, Anweisungen zum Bearbeiten und Löschen von Benutzern. Wenn ein Benutzer in der Supervisor-Rolle auf dieser Seite erreicht, wird eine Anleitung zum Bearbeiten von Benutzern gezeigt. Und wenn der Besucher "anonymous ist" oder befindet sich nicht in der Vorgesetzte oder der Administratorenrolle, wird es eine Meldung darüber informiert, dass sie nicht bearbeiten oder Löschen von Informationen für das Benutzerkonto angezeigt. Im Abschnitt "Programmgesteuert beschränken Funktionalität" Schreiben wir Code, der programmgesteuert zeigt, oder blendet Sie aus den Schaltflächen Bearbeiten und löschen, die basierend auf die Rolle des Benutzers.

### <a name="using-the-loginview-control"></a>Verwenden das LoginView-Steuerelement

Wie wir in den letzten Tutorials gesehen haben, das LoginView-Steuerelement eignet sich für verschiedene Schnittstellen für authentifizierte und anonyme Benutzer anzeigt, aber das LoginView-Steuerelement kann auch zum Anzeigen von verschiedenen Ansichtsmarkups basierend auf den Rollen des Benutzers verwendet werden. Wir verwenden Sie ein LoginView-Steuerelement, um verschiedene Anweisungen basierend auf der Website besucht Rolle des Benutzers anzuzeigen.

Beginnen Sie, indem Sie eine der oben genannten LoginView Hinzufügen der `UserGrid` GridView. Wie bereits erläutert habe, hat das LoginView-Steuerelement zwei integrierte Vorlagen: `AnonymousTemplate` und `LoggedInTemplate`. Geben Sie eine kurze Nachricht in beiden Vorlagen, die der Benutzer darüber informiert, dass sie nicht bearbeiten oder löschen alle Benutzerinformationen aus.

[!code-aspx[Main](role-based-authorization-cs/samples/sample9.aspx)]

Zusätzlich zu den `AnonymousTemplate` und `LoggedInTemplate`, kann das LoginView-Steuerelement enthalten *RoleGroups*, die rollenspezifische Vorlagen sind. Jede RoleGroup enthält eine einzelne Eigenschaft `Roles`, der gibt an, welche Rollen die RoleGroup gilt. Die `Roles` Eigenschaft kann festgelegt werden, um eine einzelne Rolle (z. B. "Administratoren") oder eine durch Trennzeichen getrennte Liste von Rollen (z. B. "Administrators, Supervisor").

Um die RoleGroups verwalten möchten, klicken Sie auf den Link "Bearbeiten RoleGroups" Smart Tag des Steuerelements, um den RoleGroup Auflistungs-Editor zu öffnen. Fügen Sie zwei neue RoleGroups hinzu. Legen Sie die erste RoleGroup `Roles` Eigenschaft auf "Administratoren" und das zweite in "Supervisor".


[![Verwalten Sie das LoginView rollenspezifische Vorlagen über den RoleGroup Auflistungs-Editor](role-based-authorization-cs/_static/image23.png)](role-based-authorization-cs/_static/image22.png)

**Abbildung 8**: Verwalten der LoginView rollenspezifische Vorlagen über das RoleGroup Auflistungs-Editor ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image24.png))


Klicken Sie auf OK, um den RoleGroup Auflistungs-Editor zu schließen. Dadurch wird das LoginView deklarative Markup einschließen aktualisiert eine `<RoleGroups>` im Abschnitt mit ein `<asp:RoleGroup>` untergeordnete Element für jede RoleGroup definiert den RoleGroup-Auflistungs-Editor. Darüber hinaus das LoginView Smarttag - anfänglich aufgelistet der "Ansichten" Dropdown-Liste nur die `AnonymousTemplate` und `LoggedInTemplate` – enthält jetzt auch die hinzugefügte RoleGroups.

Bearbeiten Sie die RoleGroups, sodass Benutzer in der Supervisor-Rolle sind die angezeigten Anweisungen zum Bearbeiten von Benutzerkonten, die während der Benutzer in der Rolle "Administratoren" Anweisungen zum Bearbeiten und Löschen von angezeigt werden. Nach diesen Änderungen sollte Ihre LoginView deklarative Markup etwa wie folgt aussehen.

[!code-aspx[Main](role-based-authorization-cs/samples/sample10.aspx)]

Nach diesen Änderungen durchführen, speichern Sie die Seite, und besuchen Sie es dann über einen Browser. Besuchen Sie zuerst die Seite als anonymer Benutzer ein. Die Meldung angezeigt werden soll, "Sie sind nicht in das System angemeldet. Aus diesem Grund Sie nicht bearbeiten oder löschen Benutzerinformationen." Klicken Sie dann melden Sie sich, wie ein authentifizierter Benutzer, aber eine, die weder in die Rolle "Supervisor" und "Administratoren" ist. Dieses Mal sollten Sie sehen die Meldung "Sie sind nicht Mitglied der Rollen Vorgesetzte oder Administratoren. Aus diesem Grund Sie nicht bearbeiten oder löschen Benutzerinformationen."

Als Nächstes melden Sie sich, als ein Benutzer Mitglied der Rolle "Supervisor" ist. Dieses Mal sollte der Supervisor rollenspezifische Meldung (siehe Abbildung 9). Und wenn Sie in der Rolle, die daraufhin der Administratoren rollenspezifische Meldung (siehe Abbildung 10) Administratoren als Benutzer anmelden.


[![Bruce wird der Vorgesetzte rollenspezifische-Meldung angezeigt.](role-based-authorization-cs/_static/image26.png)](role-based-authorization-cs/_static/image25.png)

**Abbildung 9**: Bruce wird der Vorgesetzte rollenspezifische Meldung angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image27.png))


[![Tito wird den Administratoren rollenspezifische-Meldung angezeigt.](role-based-authorization-cs/_static/image29.png)](role-based-authorization-cs/_static/image28.png)

**Abbildung 10**: Tito sehen Sie die Administratoren rollenspezifische-Nachricht ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image30.png))


Als die Screenshots in den Abbildungen 9 und 10 anzeigen rendert die LoginView nur eine Vorlage, auch wenn mehrere Vorlagen anwenden. Bruce und Tito werden Benutzer protokolliert, aber das LoginView gerendert wird, nur die übereinstimmenden RoleGroup und nicht die `LoggedInTemplate`. Darüber hinaus Tito Rollen von Administratoren und Supervisor gehört, aber das LoginView-Steuerelement rendert die Administratoren rollenspezifische Vorlage anstelle der Supervisor eine.

Abbildung 11 zeigt den Workflow, von dem LoginView-Steuerelement verwendet wird, um zu bestimmen, welche Vorlage zum Rendern. Beachten Sie, dass die LoginView-Vorlage gerendert, wenn mehr als einmal RoleGroup angegeben vorhanden ist wird, die *erste* RoleGroup, der mit übereinstimmt. Das heißt, wenn wir der Vorgesetzte RoleGroup als die erste RoleGroup und die Administratoren als die zweite platziert hat, wird Klicken Sie dann beim Tito auf dieser Seite besucht er die Vorgesetzte-Nachricht angezeigt.


[![Das LoginView-Steuerelement-Workflow für die Bestimmung von welcher Vorlage zum Rendern](role-based-authorization-cs/_static/image32.png)](role-based-authorization-cs/_static/image31.png)

**Abbildung 11**: das LoginView-Steuerelement-Workflow für die Bestimmung was Vorlage zum Rendern ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funktionalität programmgesteuert beschränkt.

Während das LoginView-Steuerelement verschiedene Anweisungen basierend auf der Rolle des Benutzers auf der Seite angezeigt wird, bleiben die Schaltflächen Bearbeiten und "Abbrechen" für alle sichtbar. Programmgesteuertes Ausblenden der Schaltflächen Bearbeiten und Löschen für anonyme Besucher und Benutzer, die weder die Administratoren der Vorgesetzte Rolle angehören müssen. Wir müssen die Schaltfläche "löschen" für alle Benutzer auszublenden, der kein Administrator ist. Dazu schreiben Sie ein paar Codezeilen, die programmgesteuert verweist auf die CommandFields bearbeiten und löschen LinkButtons und legt deren `Visible` Eigenschaften `false`, falls erforderlich.

Die einfachste Möglichkeit, programmgesteuert Steuerelemente in einem CommandField verweisen wird zuerst in eine Vorlage konvertieren. Zu diesem Zweck klicken Sie auf den Link "Spalten bearbeiten" aus des GridView Smarttag, wählen Sie die CommandField aus der Liste der aktuellen Felder, und klicken Sie auf den Link "Dieses Feld in ein TemplateField konvertieren". Dadurch wird die CommandField in ein TemplateField mit einer `ItemTemplate` und `EditItemTemplate`. Die `ItemTemplate` enthält die bearbeiten und löschen LinkButtons während der `EditItemTemplate` enthält das Update und LinkButtons Abbrechen.


[![Die CommandField in ein TemplateField konvertieren](role-based-authorization-cs/_static/image35.png)](role-based-authorization-cs/_static/image34.png)

**Abbildung 12**: CommandField in ein TemplateField konvertieren ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image36.png))


Aktualisieren Sie den bearbeiten und Löschen von LinkButtons in die `ItemTemplate`wird durch das Festlegen ihrer `ID` Eigenschaften mit Werten `EditButton` und `DeleteButton`bzw.

[!code-aspx[Main](role-based-authorization-cs/samples/sample11.aspx)]

Wenn Daten an die GridView gebunden ist, listet die GridView die Datensätze in der `DataSource` Eigenschaft und generiert einen entsprechenden `GridViewRow` Objekt. Da jeder `GridViewRow` -Objekt wird erstellt, die `RowCreated` Ereignis wird ausgelöst. Um die Schaltflächen "Bearbeiten" und "Delete" für nicht autorisierte Benutzer ausblenden möchten, müssen wir erstellen einen Ereignishandler für dieses Ereignis und Programmgesteuertes Verweisen auf die bearbeiten und löschen LinkButtons, Festlegen ihrer `Visible` Eigenschaften entsprechend.

Erstellen Sie einen Ereignishandler die `RowCreated` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](role-based-authorization-cs/samples/sample12.cs)]

Beachten Sie, dass die `RowCreated` Ereignis ausgelöst wird, für die *alle* GridView Zeilen, einschließlich der Header, die Fußzeile, Pager-Schnittstelle und So weiter. Wir möchten nur die bearbeiten und Löschen von LinkButtons programmgesteuert verweisen, wenn wir mit einer Datenzeile nicht im Bearbeitungsmodus befindet arbeiten (da die Zeile im Bearbeitungsmodus Schaltflächen "Aktualisieren" und "Abbrechen" anstelle von bearbeiten und löschen hat). Diese Überprüfung erfolgt durch die `if` Anweisung.

Wenn wir mit einer Datenzeile, die nicht im Bearbeitungsmodus befindet arbeiten, das Bearbeiten und löschen LinkButtons sind auf die verwiesen wird und ihre `Visible` Eigenschaften werden auf der Basis der booleschen Werte vom die `User` des Objekts `IsInRole(roleName)` Methode. Das User-Objekt verweist auf den Prinzipal erstellt werden, indem die `RoleManagerModule`; daher die `IsInRole(roleName)` Methode, die die Rollen-API verwendet, um zu bestimmen, ob der aktuelle Besucher gehört *RoleName*.

> [!NOTE]
> Wir hätten auch direkt die Rollen-Klasse ersetzt des Aufrufs von `User.IsInRole(roleName)` durch einen Aufruf der [ `Roles.IsUserInRole(roleName)` Methode](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Ich beschloss, verwenden Sie des Prinzipalobjekt `IsInRole(roleName)` -Methode in diesem Beispiel, da dies effizienter als die Rollen-API direkt verwenden ist. Weiter oben in diesem Tutorial konfiguriert es die Rollen-Manager, um die Rollen des Benutzers in einem Cookie zwischengespeichert. Dies Cookiedaten zwischengespeichert wird nur verwendet, wenn des Prinzipals `IsInRole(roleName)` aufgerufen; direkte Aufrufe der API-Rollen umfassen immer eine Reise nach rollenspeicher. Aufrufen von selbst, wenn Rollen nicht in einem Cookie zwischengespeichert werden, des Prinzipalobjekt `IsInRole(roleName)` Methode ist in der Regel effizienter, da Wenn sie aufgerufen wird, für die ersten während einer Anforderung und die Ergebnisse zwischengespeichert. Die Rollen-API, auf der anderen Seite führt jeglichen Zwischenspeicherns keine. Da die `RowCreated` wird ausgelöst, einmal für jede Zeile in der GridView mit `User.IsInRole(roleName)` umfasst nur eine Reise nach rollenspeicher, während `Roles.IsUserInRole(roleName)` erfordert *N* Fahrten, in denen *N* ist die Anzahl von Benutzerkonten, die im Raster angezeigt werden soll.


Die Schaltfläche "Bearbeiten" des `Visible` -Eigenschaftensatz auf `true` ist der auf dieser Seite können Benutzer in der Rolle "Administratoren" oder "Supervisor"; andernfalls ist er auf festgelegt `false`. Der Schaltfläche zum Löschen des `Visible` -Eigenschaftensatz auf `true` nur dann, wenn der Benutzer in der Rolle "Administratoren" ist.

Testen Sie diese Seite über einen Browser ein. Wenn Sie die Seite besuchen als eine anonyme Besucher oder als ein Benutzer, der weder ein Supervisor noch Administrator ist, ist die CommandField leer. immer noch vorhanden, aber die Schaltflächen als eine dünne hellgrau ohne das Bearbeiten oder löschen.

> [!NOTE]
> Es ist möglich, die CommandField ausblenden vollständig Wenn eine nicht-Supervisor und nicht-Administrator wird die Seite besuchen. Ich lassen Sie dieses als Übung für den Leser.


[![Das Bearbeiten und Löschen von Schaltflächen sind für nicht-Supervisor und nicht-Administratoren ausgeblendet.](role-based-authorization-cs/_static/image38.png)](role-based-authorization-cs/_static/image37.png)

**Abbildung 13**: das Bearbeiten und Löschen von Schaltflächen für nicht-Vorgesetzte und nicht-Administratoren ausgeblendet sind ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image39.png))


Wenn ein Benutzer, der die Rolle "Supervisor" (aber nicht für die Rolle "Administratoren") gehört, besucht, sieht er nur die Schaltfläche "Bearbeiten".


[![Obwohl die Schaltfläche "Bearbeiten" zur Vorgesetzte verfügbar ist, wird die Schaltfläche "löschen" ausgeblendet.](role-based-authorization-cs/_static/image41.png)](role-based-authorization-cs/_static/image40.png)

**Abbildung 14**: während die Schaltfläche Bearbeiten, ist verfügbar für Vorgesetzte, die Schaltfläche "löschen" wird ausgeblendet ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image42.png))


Und wenn ein Administrator besucht, hat Sie Zugriff auf die Schaltflächen Bearbeiten und löschen.


[![Das Bearbeiten und Löschen von Schaltflächen sind nur verfügbar, für Administratoren](role-based-authorization-cs/_static/image44.png)](role-based-authorization-cs/_static/image43.png)

**Abbildung 15**: das Bearbeiten und Löschen von Schaltflächen sind nur verfügbar, für Administratoren ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image45.png))


## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Schritt 3: Anwenden von Regeln für rollenbasierte Autorisierung auf Klassen und Methoden

Bearbeiten Sie in Schritt2, die wir eingeschränkte Funktionen für Benutzer mit den Supervisor und Administratoren Rollen und löschen Sie Funktionen nur für Administratoren. Dies wurde durch das Ausblenden von der zugehörigen Elemente der Benutzeroberfläche für nicht autorisierte Benutzer über programmgesteuerte Methoden. Diese Maßnahmen garantieren nicht, dass ein nicht autorisierter Benutzer eine privilegierte Aktion kann nicht ausgeführt werden. Es gibt möglicherweise Elemente der Benutzeroberfläche, die später hinzugefügt werden oder es wurde vergessen, die für nicht autorisierte Benutzer ausblenden. Oder ein Hacker möglicherweise eine andere Möglichkeit zum Abrufen der ASP.NET-Seite zum Ausführen der gewünschten Methode fest.

Eine einfache Möglichkeit, um sicherzustellen, dass für ein bestimmter Teil der Funktionalität von unbefugten Benutzern nicht zugegriffen werden kann, ergänzen Sie diese Klasse oder Methode mit ist der [ `PrincipalPermission` Attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Wenn die .NET Runtime eine Klasse verwendet oder eine seiner Methoden ausführt, überprüft stellen Sie sicher, dass der aktuelle Sicherheitskontext-Berechtigung. Die `PrincipalPermission` -Attribut stellt einen Mechanismus, über die wir können diese Regeln definieren.

Erläutert, mit der `PrincipalPermission` -Attributs zurück in die <a id="_msoanchor_9"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial. Insbesondere wurde erläutert, wie GridView ergänzen `SelectedIndexChanged` und `RowDeleting` -Ereignishandler so, dass sie nur durch authentifizierte Benutzer "und" Tito, bzw. ausgeführt werden konnte. Die `PrincipalPermission` Attribut funktioniert ebenso gut mit Rollen.

Wir veranschaulichen die Verwendung der `PrincipalPermission` Attribut in des GridView `RowUpdating` und `RowDeleting` -Ereignishandler zum Verhindern der Ausführung für nicht autorisierte Benutzer. Alles, was erforderlich ist, wird das entsprechende Attribut auf jede Funktionsdefinition hinzufügen:

[!code-csharp[Main](role-based-authorization-cs/samples/sample13.cs)]

Das Attribut für die `RowUpdating` -Ereignishandler schreibt vor, dass nur Benutzer in den Rollen von Administratoren oder Vorgesetzte, als das Attribut für den Ereignishandler ausgeführt werden können die `RowDeleting` -Ereignishandler wird die Ausführung für Benutzer der Administratorgruppe beschränkt. die Rolle.


> [!NOTE]
> Die `PrincipalPermission` Attribut wird dargestellt, wie eine Klasse in der `System.Security.Permissions` Namespace. Achten Sie darauf, dass Sie zum Hinzufügen einer `using System.Security.Permissions` -Anweisung am Anfang der CodeBehind-Klasse-Datei zu diesem Namespace zu importieren.


Wenn, irgendwie nicht-Administrator versucht, führen Sie die `RowDeleting` -Ereignishandler oder wenn ein nicht-Supervisor "oder" nicht-Administrator versucht, zum Ausführen der `RowUpdating` Ereignishandler, die .NET Runtime löst eine `SecurityException`.


[![Wenn der Sicherheitskontext zum Ausführen der Methode nicht autorisiert ist, wird eine SecurityException ausgelöst.](role-based-authorization-cs/_static/image47.png)](role-based-authorization-cs/_static/image46.png)

**Abbildung 16**: Wenn der Sicherheitskontext nicht, zum Ausführen der Methode autorisiert ist, eine `SecurityException` ausgelöst ([klicken Sie, um das Bild in voller Größe anzeigen](role-based-authorization-cs/_static/image48.png))


Zusätzlich zu ASP.NET-Seiten haben viele Anwendungen auch eine Architektur mit verschiedenen Ebenen, wie z. B. von Geschäftslogik und Datenzugriffsschichten. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftsfunktionen Logik und die Daten beziehen. Die `PrincipalPermission` Attribut ist für das Anwenden von Autorisierungsregeln auf diese Ebenen ebenfalls hilfreich.

Weitere Informationen zur Verwendung der `PrincipalPermission` Attribut zum Definieren von Autorisierungsregeln auf Klassen und Methoden finden [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [Autorisierungsregeln hinzufügen, Business und Daten Schichten mithilfe `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial erläutert, wie grob angegeben, und eine differenzierte Autorisierung basierend auf den Rollen des Benutzers. ASP. NET URL-Autorisierungsfeature kann Entwickler einer Seite, um anzugeben, welche Identitäten zugelassen oder verweigert den Zugriff auf welche Seiten. Wie wir, in gesehen der <a id="_msoanchor_10"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Tutorial, URL-Autorisierung, die pro Benutzer durch die Regeln angewendet werden können. Sie können auch einzelne Rolle-von-Role angewendet werden, wie in Schritt 1 in diesem Tutorial beschrieben.

Eine differenzierte Autorisierungsregeln können deklarativ oder programmgesteuert angewendet werden. In Schritt2 haben das LoginView-Steuerelement RoleGroups-Funktion zum Rendern verwenden, andere Ausgabe basierend auf Rollen des Benutzers, auf die Website besucht. Wir haben uns auch nach Möglichkeiten, um programmgesteuert zu ermitteln, wenn ein Benutzer, eine bestimmte Rolle und Informationen angehört zu der Seite Funktionen entsprechend anpassen.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungsregeln auf Geschäfts- und Datenebenen verwenden `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile: Arbeiten mit Rollen](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Liste der Security-Frage für ASP.NET 2.0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Technische Dokumentation für die `<roleManager>` Element](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial enthalten Suchi Banerjee und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](assigning-roles-to-users-cs.md)
> [Weiter](creating-and-managing-roles-vb.md)
