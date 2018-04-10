---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-cs
title: Benutzerbasierte Autorisierung (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm betrachten wir Beschränken des Zugriffs auf Seiten und Seitenebene-Funktionalität über eine Vielzahl von Techniken einschränken.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 3c815a9e-2296-4b9b-b945-776d54989daa
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a0d476ffaf1f176c21b245520fa943f66e8c0d5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="user-based-authorization-c"></a>Benutzerbasierte Autorisierung (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_cs.pdf)

> In diesem Lernprogramm betrachten wir Beschränken des Zugriffs auf Seiten und Seitenebene-Funktionalität über eine Vielzahl von Techniken einschränken.


## <a name="introduction"></a>Einführung

Die meisten Webanwendungen, die Benutzerkonten bieten holen Sie dies im Webpart bestimmte Besucher den Zugriff auf bestimmte Seiten innerhalb der Website einschränken. In den meisten online Diskussionsforum ein Standorten z. B. alle Benutzer - anonyme als auch authentifizierte - Diskussionsforum vorhanden ist, wird die ein Beiträge anzeigen werden, aber nur authentifizierte Benutzer besuchen der Webseite, um eine neue Bereitstellung erstellen. Und möglicherweise Administratorseiten, die nur für einen bestimmten Benutzer (oder eine bestimmte Gruppe von Benutzern) zugänglich sind. Darüber hinaus kann auf Seitenebene Funktionalität auf Basis von Benutzer unterscheiden. Beim Anzeigen einer Liste der Beiträge werden authentifizierte Benutzern eine Schnittstelle für die Bewertung jedes Post angezeigt, während diese Schnittstelle nicht anonyme Besucher verfügbar ist.

ASP.NET vereinfacht die Benutzer-basierten Autorisierungsregeln zu definieren. Mit nur einem gewissen Grad Markup in `Web.config`, bestimmte Webseiten oder vollständigen Verzeichnissen können werden gesperrt, damit sie nur für eine angegebene Teilmenge von Benutzern zugänglich sind. Seitenebene Funktionalität kann ein- oder ausschalten basierend auf den aktuell angemeldeten Benutzers programmgesteuerte und deklarative Weise aktiviert werden.

In diesem Lernprogramm betrachten wir Beschränken des Zugriffs auf Seiten und Seitenebene-Funktionalität über eine Vielzahl von Techniken einschränken. Fangen wir an!

## <a name="a-look-at-the-url-authorization-workflow"></a>Ein Blick auf den URL-Autorisierung-Workflow

Entsprechend der Anleitung unter dem [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) Lernprogramm, wenn die ASP.NET-Laufzeit eine Anforderung verarbeitet, für eine ASP.NET-Ressource die Anforderung eine Anzahl von Ereignissen während des Lebenszyklus löst. *HTTP-Module* verwaltete Klassen, deren Code wird als Reaktion auf ein bestimmtes Ereignis am Anforderungslebenszyklus ausgeführt, werden. Im Lieferumfang von ASP.NET sind einer Anzahl von HTTP-Module, die wichtige Aufgaben im Hintergrund ausführen.

Eine HTTP-Modul ist [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Wie im vorherigen Lernprogrammen, die primäre Funktion des erläutert die `FormsAuthenticationModule` besteht darin, die Identität des die aktuelle Anforderung festzulegen. Dies geschieht durch Überprüfen das Formularauthentifizierungsticket, mit denen in einem Cookie gespeichert oder in die URL eingebettet ist. Diese Identifikation findet während der [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Ein anderes wichtig HTTP-Modul ist die [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), dem wird ausgelöst, als Antwort auf die [ `AuthorizeRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (die geschieht nach der `AuthenticateRequest` Ereignis). Die `UrlAuthorizationModule` untersucht Konfiguration Markup in `Web.config` bestimmen, ob die Identität des aktuelle Autorität für die angegebene Seite besuchen verfügt. Dieser Prozess wird als bezeichnet *URL-Autorisierung*.

Untersucht die Syntax für die URL-Autorisierungsregeln in Schritt 1, aber zuerst wir sehen Sie sich, was die `UrlAuthorizationModule` ist abhängig davon, ob die Anforderung oder nicht autorisiert ist. Wenn die `UrlAuthorizationModule` feststellt, dass die Anforderung autorisiert ist, es keine Aktion ausgeführt wird, und die Anforderung wird, während des Lebenszyklus fortgesetzt. Allerdings ist die Anforderung *nicht* autorisiert, und dann die `UrlAuthorizationModule` bricht den Lebenszyklus und weist die `Response` zurückzugebenden Objekts ein [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html) Status. Bei Verwendung der Formularauthentifizierung dieser HTTP 401-Status wird nie zurückgegeben, an den Client da Wenn die `FormsAuthenticationModule` erkennt HTTP 401 wird der Status ändert, ein [HTTP 302-Umleitung](http://www.checkupdown.com/status/E302.html) zur Anmeldeseite.

Abbildung 1 veranschaulicht den Workflow der ASP.NET-Pipeline die `FormsAuthenticationModule`, und die `UrlAuthorizationModule` bei eine nicht autorisierte Anforderung eingeht. Abbildung 1 zeigt eine Anforderung von einem anonymen Besucher für `ProtectedPage.aspx`, die eine Seite, die für anonyme Benutzer den Zugriff verweigert wird. Da der Besucher anonym ist, wird die `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Die `FormsAuthenticationModule` 401 Status wird dann in eine 302-Umleitung Anmeldeseite konvertiert. Nach der Authentifizierung des Benutzers über die Anmeldeseite umgeleitet `ProtectedPage.aspx`. Dieses Mal die `FormsAuthenticationModule` identifiziert den Benutzer basierend auf seinem Authentifizierungsticket. Nun, dass der Besucher authentifiziert wird, die `UrlAuthorizationModule` ermöglicht den Zugriff auf die Seite.


[![Die Formularauthentifizierung und URL-Autorisierung-Workflow](user-based-authorization-cs/_static/image2.png)](user-based-authorization-cs/_static/image1.png)

**Abbildung 1**: die Formularauthentifizierung und Workflow für URL-Autorisierung ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image3.png))


Abbildung 1 zeigt die Aktivität, die auftritt, wenn ein anonyme Besucher versucht, auf eine Ressource zuzugreifen, die nicht für anonyme Benutzer verfügbar ist. In einem solchen Fall wird der anonyme Besucher zur Anmeldeseite mit der Seite umgeleitet, die sie in der Abfragezeichenfolge angegebene besuchen wollten. Sobald der Benutzer erfolgreich angemeldet hat, wird er automatisch wieder auf die Ressource umgeleitet werden, die sie ursprünglich wollte anzeigen.

Wenn nicht autorisierte Anforderung durch einen anonymen Benutzer hergestellt wird, wird dieser Workflow ist einfach und ist für den Besucher zu verstehen, was geschehen ist einfach und deren Ursachen. Jedoch behalten in ausmacht, die der `FormsAuthenticationModule` leitet *alle* nicht autorisierte Benutzer die Anmeldeseite, selbst wenn von einem authentifizierten Benutzer angefordert wird. Dies kann zu einer verwirrend Benutzeroberfläche führen, wenn ein authentifizierter Benutzer versucht, eine Website besuchen, für die sie die Autorität für die fehlt.

Stellen Sie sich vor, dass unsere Website der URL-Autorisierungsregeln konfiguriert wurde, dass der ASP.NET-Seite `OnlyTito.aspx` Zugriff nur für Tito wurde. Angenommen, dass Sam die Website besucht anmeldet, und anschließend versucht, besuchen `OnlyTito.aspx`. Die `UrlAuthorizationModule` hält am Anforderungslebenszyklus und eine HTTP 401 Unauthorized Rückgabestatus, der die `FormsAuthenticationModule` erkennt, und klicken Sie dann Sam zur Anmeldeseite umgeleitet. Da Sam bereits angemeldet hat, aber sie sich vielleicht, warum er wieder zur Anmeldeseite gesendet wurde. Sie können Grund, dass ihre Anmeldeinformationen aus irgendeinem Grund verloren gegangen sind oder dass er ungültige Anmeldeinformationen eingegeben haben. Wenn Sam seiner Anmeldeinformationen auf der Anmeldeseite erneut sie protokolliert auf (erneut) und umgeleitet zu `OnlyTito.aspx`. Die `UrlAuthorizationModule` erkennt, dass Sam kann nicht auf dieser Seite besuchen, und sie zur Anmeldeseite zurückgegeben werden werden.

Abbildung 2 zeigt dieses verwirrend Workflows.


[![Standardworkflow kann zu einem Zyklus verwirrend führen.](user-based-authorization-cs/_static/image5.png)](user-based-authorization-cs/_static/image4.png)

**Abbildung 2**: der Standard Workflow kann führen zu einem Zyklus verwirrend erscheinen ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image6.png))


Der Workflow in Abbildung 2 dargestellten kann auch die meisten Computer clevere Besucher schnell befuddle. Wir werden die Möglichkeiten, dies zu verhindern, dass verwirrend erscheinen Zyklus in Schritt2 betrachten.

> [!NOTE]
> ASP.NET verwendet zwei Mechanismen, um zu bestimmen, ob der aktuelle Benutzer eine bestimmte Webseite zugreifen kann: URL-Autorisierung und Autorisierung. Autorisierung wird implementiert, indem die [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), der Autorität für die bestimmt, indem Sie die angeforderten Dateien ACLs lesen. Autorisierung wird am häufigsten mit Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formularauthentifizierung verwenden, werden alle Betriebssystem- und den Dateinamen auf Systemebene Anforderungen von dem gleichen Windows-Konto, unabhängig von der Website für den Benutzer ausgeführt. Da diese Reihe von Lernprogrammen Formularauthentifizierung konzentriert, wird es nicht dateiauthentifizierung erörtert.


### <a name="the-scope-of-url-authorization"></a>Der Bereich der URL-Autorisierung

Die `UrlAuthorizationModule` wird verwalteter Code, der die ASP.NET-Laufzeit gehört. Vor Version 7 der Microsofts [(Internet Information Services, IIS)](https://www.iis.net/) Webserver, es wurde eine unterschiedliche Barriere zwischen IISs HTTP-Pipeline und die ASP.NET-Laufzeit Pipeline. Kurz gesagt, in IIS 6 und früher ASP. NET `UrlAuthorizationModule` nur ausgeführt wird, wenn eine Anforderung an die ASP.NET-Laufzeit aus IIS delegiert werden kann. Wird standardmäßig IIS selbst - statische Inhalte wie HTML-Seiten und CSS, JavaScript und Bilddateien - verarbeitet und nur Anforderungen an die ASP.NET-Laufzeit, wenn eine Seite mit der Erweiterung des aktivitätsbezeichners `.aspx`, `.asmx`, oder `.ashx` angefordert wird.

IIS 7, ermöglicht jedoch integrierten IIS und ASP.NET Pipelines. Einige Konfigurationseinstellungen können Sie IIS 7 aufzurufenden Einrichten der `UrlAuthorizationModule` für *alle* Clientanforderungen, was bedeutet, dass die URL-Autorisierungsregeln für Dateien eines beliebigen Typs definiert werden können. Darüber hinaus umfasst IIS 7 eigene URL-Autorisierung-Modul. Weitere Informationen für die Integration von ASP.NET und IIS 7 des systemeigenen URL-Autorisierung-Funktionen finden Sie unter [Verständnis IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Übernehmen Sie für eine eingehendere Betrachtung ASP.NET und IIS 7-Integration, eine Kopie Shahram Khosravis Buch *Professional IIS 7 und ASP.NET integrierten Programmierung* (ISBN: 978 0470152539).

Kurz gesagt, werden in Versionen vor IIS 7, URL-Autorisierungsregeln nur auf Ressourcen, die durch die ASP.NET-Laufzeit behandelt angewendet. Mit IIS 7 Es kann jedoch mithilfe IISs-systemeigenen URL Authorization-Feature oder ASP zu integrieren. NET `UrlAuthorizationModule` in IISs HTTP-Pipeline, wodurch erweitern diese Funktionalität für alle Anforderungen.

> [!NOTE]
> Es gibt einige geringfügige noch wichtige Unterschiede bei der Art ASP. NET `UrlAuthorizationModule` und IIS 7 URL Authorization-Feature die Autorisierungsregeln verarbeitet. Dieses Lernprogramm ist nicht untersuchen, IIS 7-URL-Autorisierung Funktionen oder die Unterschiede im wie Autorisierungsregeln, die im Vergleich zum Analysieren der `UrlAuthorizationModule`. Weitere Informationen zu diesen Themen finden Sie in der IIS 7-Dokumentation auf MSDN oder am [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Schritt 1: Definieren von URL-Autorisierungsregeln in`Web.config`

Die `UrlAuthorizationModule` bestimmt, ob gewähren oder Verweigern des Zugriffs auf eine angeforderte Ressource für eine bestimmte Identität basierend auf den URL-Autorisierungsregeln, die in der Konfigurationsdatei der Anwendung definiert. Die Autorisierungsregeln werden buchstabiert den [ `<authorization>` Element](https://msdn.microsoft.com/library/8d82143t.aspx) in Form von `<allow>` und `<deny>` untergeordnete Elemente. Jede `<allow>` und `<deny>` untergeordnetes Element angeben kann:

- Einen bestimmten Benutzer
- Eine durch Trennzeichen getrennte Liste von Benutzern
- Alle anonymen Benutzer, gekennzeichnet durch ein Fragezeichen (?)
- Alle Benutzer, die durch ein Sternchen gekennzeichnet (\*)

Das folgende Markup veranschaulicht, wie die URL-Autorisierungsregeln verwenden, um Benutzer Tito und Scott zulassen und verweigern alle anderen:

[!code-xml[Main](user-based-authorization-cs/samples/sample1.xml)]

Die `<allow>` -Element definiert, welche Benutzer - Tito "und" Scott - dürfen während der `<deny>` Element anweist, dass *alle* Benutzern verweigert werden.

> [!NOTE]
> Die `<allow>` und `<deny>` Elemente können auch die Autorisierungsregeln für Rollen angeben. Untersuchen wir rollenbasierte Autorisierung in einem späteren Lernprogramm.


Die folgende Einstellung gewährt den Zugriff auf Personen, die keine Sam (einschließlich anonyme Besucher):

[!code-xml[Main](user-based-authorization-cs/samples/sample2.xml)]

Um nur durch authentifizierte Benutzer zu ermöglichen, verwenden Sie die folgende Konfiguration, die für alle anonymen Benutzer den Zugriff verweigert:

[!code-xml[Main](user-based-authorization-cs/samples/sample3.xml)]

Die Autorisierungsregeln werden definiert, in der `<system.web>` Element im `Web.config` und gelten für alle Ressourcen in der Webanwendung ASP.NET. Häufig, weist eine Anwendung verschiedene Autorisierungsregeln für die verschiedenen Abschnitte. Z. B. auf einer e-Commerce-Website alle Besucher konfigurieren möglicherweise Instrumentationsdaten die Produkte, finden Sie unter Product Reviews Suchkatalog für die und so weiter. Nur authentifizierte Benutzer können jedoch das Auschecken oder die Seiten zum Verwalten einer Person versandverlaufs erreicht werden. Darüber hinaus gibt es möglicherweise Teile des Standorts, die nur Benutzer auswählen, z. B. Websiteadministratoren zugänglich sind.

ASP.NET vereinfacht die verschiedenen Autorisierungsregeln für verschiedene Dateien und Ordner am Standort zu definieren. Die Autorisierungsregeln, die in des Stammordners angegebenen `Web.config` Datei anwenden, um alle ASP.NET-Ressourcen am Standort. Allerdings können diese Standardeinstellungen für die Autorisierung für einen bestimmten Ordner überschrieben werden, durch Hinzufügen einer `Web.config` mit einem `<authorization>` Abschnitt.

Wir aktualisieren unsere Website, sodass nur authentifizierte Benutzer in die ASP.NET-Seiten besuchen können die `Membership` Ordner. Um dies zu erreichen wir müssen einen `Web.config` Datei wird in der `Membership` Ordner, und legen Sie dessen autorisierungseinstellungen, anonyme Benutzern zu verweigern. Mit der rechten Maustaste die `Membership` Ordner im Projektmappen-Explorer, wählen Sie im Menü neues Element hinzufügen im Kontextmenü, und fügen Sie eine neue Web-Konfigurationsdatei mit dem Namen `Web.config`.


[![Hinzufügen einer Datei "Web.config" zu dem Ordner Mitgliedschaft](user-based-authorization-cs/_static/image8.png)](user-based-authorization-cs/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` Datei wird in der `Membership` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image9.png))


An diesem Punkt Komponente im Projekt sollten zwei `Web.config` Dateien: einer in das Stammverzeichnis und einen in den `Membership` Ordner.


[![Die Anwendung sollte jetzt zwei "Web.config"-Dateien enthalten.](user-based-authorization-cs/_static/image11.png)](user-based-authorization-cs/_static/image10.png)

**Abbildung 4**: Ihre Anwendung sollte jetzt zwei enthalten `Web.config` Dateien ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image12.png))


Aktualisieren Sie die Konfigurationsdatei in die `Membership` Ordner so, dass die It für anonyme Benutzer Zugriff verhindert.

[!code-xml[Main](user-based-authorization-cs/samples/sample4.xml)]

Das ist alles vorhanden ist!

Um diese Änderung zu testen, finden Sie auf der Startseite in einem Browser, und stellen Sie sicher, dass Sie abgemeldet. Da das Standardverhalten einer ASP.NET-Anwendung darin besteht, alle Besucher konfigurieren können, und da wir keine Autorisierung Änderungen in des Stammverzeichnis vorgenommen haben `Web.config` -Datei werden wir können die Dateien in das Stammverzeichnis als eine anonyme Besucher besuchen.

Klicken Sie auf das Erstellen von Benutzerkonten in der linken Spalte gefunden. Dadurch gelangen Sie zu der `~/Membership/CreatingUserAccounts.aspx`. Da die `Web.config` in der Datei die `Membership` Ordner definiert Autorisierungsregeln, um anonymen Zugriff zu verhindern die `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Die `FormsAuthenticationModule` dies 302 Umleitung Status, senden uns zur Anmeldeseite ändert. Beachten Sie, dass die Seite wir versuchen zuzugreifen (`CreatingUserAccounts.aspx`) übergeben wird, zu der Anmeldeseite über die `ReturnUrl` Querystring-Parameter.


[![Seit der URL-Autorisierung Regeln verbieten anonymen Zugriff werden wir die Anmeldeseite umgeleitet](user-based-authorization-cs/_static/image14.png)](user-based-authorization-cs/_static/image13.png)

**Abbildung 5**: Da die URL-Autorisierung anonymen Zugriff zulassen Regeln, werden wir zur Anmeldeseite umgeleitet ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image15.png))


Nach erfolgreich anmelden, werden wir umgeleitet die `CreatingUserAccounts.aspx` Seite. Dieses Mal die `UrlAuthorizationModule` ermöglicht den Zugriff auf die Seite, da es nicht mehr anonym sind.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Anwenden von URL-Autorisierungsregeln zu einer bestimmten Position

Die autorisierungseinstellungen definiert, der `<system.web>` Abschnitt `Web.config` gelten für alle Ressourcen in diesem Verzeichnis und seinen Unterverzeichnissen ASP.NET (bis andernfalls durch eine andere überschrieben `Web.config` Datei). In einigen Fällen können wir jedoch alle ASP.NET-Ressourcen in einem bestimmten Verzeichnis eine spezifische autorisierungskonfiguration mit Ausnahme von einer oder zwei bestimmte Seiten haben möchten. Dies kann durch Hinzufügen einer `<location>` Element im `Web.config`, zeigen Sie ihn in die Datei, deren Autorisierungsregeln unterscheiden sich, und seine eindeutige Autorisierungsregeln darin zu definieren.

Um zu veranschaulichen, mit der `<location>` Element, um die Konfigurationseinstellungen für eine bestimmte Ressource zu überschreiben, wir die autorisierungseinstellungen so anpassen, dass nur Tito besuchen kann `CreatingUserAccounts.aspx`. Um dies zu erreichen, fügen einen `<location>` Element an der `Membership` des Ordners `Web.config` Datei, und aktualisieren Sie das Markup, sodass er wie folgt aussieht:

[!code-xml[Main](user-based-authorization-cs/samples/sample5.xml)]

Die `<authorization>` Element im `<system.web>` definiert die Standard-URL-Autorisierungsregeln für ASP.NET-Ressourcen in der `Membership` Ordner und seinen Unterordnern. Die `<location>` Element erlaubt es uns, die diese Regeln für eine bestimmte Ressource zu überschreiben. Im obigen Markup der `<location>` Elementverweise der `CreatingUserAccounts.aspx` Seite, und gibt an, z. B. Tito, ermöglichen, seine Autorisierungsregeln verweigern aber alle anderen Benutzer.

Um diese Änderung für die Autorisierung zu testen, starten Sie auf der Website als anonymer Benutzer. Wenn Sie versuchen, eine andere Seite in besuchen die `Membership` Ordner, z. B. `UserBasedAuthorization.aspx`, die `UrlAuthorizationModule` verweigert die Anforderung und Sie werden zur Anmeldeseite umgeleitet. Nach der Anmeldung als Scott sagen, besuchen Sie eine andere Seite in der `Membership` Ordner *außer* für `CreatingUserAccounts.aspx`. Bei dem Versuch, besuchen `CreatingUserAccounts.aspx` als jeder angemeldet Tito führt jedoch bei einem Versuch vor nicht autorisiertem Zugriff, Sie wieder auf die Anmeldeseite umgeleitet.

> [!NOTE]
> Die `<location>` Element muss außerhalb der Konfigurations angezeigt `<system.web>` Element. Sie benötigen ein separates `<location>` -Element für jede Ressource, deren autorisierungseinstellungen, die Sie überschreiben möchten.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Ein Blick auf wie die`UrlAuthorizationModule`verwendet die Autorisierungsregeln zum gewähren oder Verweigern des Zugriffs

Die `UrlAuthorizationModule` bestimmt, ob eine bestimmte Identität für eine bestimmte URL zu autorisieren, indem Sie die URL-Autorisierung analysieren einzeln nacheinander Regeln, beginnend mit der ersten Aktivität und ihre Arbeitsweise nach unten. Sobald eine Übereinstimmung gefunden wird, If je nach der Übereinstimmung der Benutzer wird Zugriff gewährt oder verweigert, ein `<allow>` oder `<deny>` Element. <strong>Wenn keine Übereinstimmung gefunden wird, wird dem Benutzer Zugriff gewährt.</strong> Wenn Sie den Zugriff einschränken möchten, ist es daher unabdinglich, Sie verwenden eine `<deny>` Element als das letzte Element in der Konfiguration der URL-Autorisierung. <strong>Wenn Sie weglassen eine</strong><strong>`<deny>`</strong><strong>Element, alle Benutzer werden Zugriff gewährt werden.</strong>

Um besser zu verstehen, den Vorgang, mit dem `UrlAuthorizationModule` um Autorität zu bestimmen, betrachten Sie das Beispiel URL-Autorisierungsregeln, die weiter oben in diesem Schritt erläutert. Die erste Regel ist ein `<allow>` -Element, das Zugriff auf Tito und Scott ermöglicht. Die zweite Regeln wird ein `<deny>` -Element, das auf "Jeder" Zugriff verweigert. Wenn ein anonymer Benutzer besucht, die `UrlAuthorizationModule` gestartet wird, indem Sie anfordern, wird anonymen Scott oder Tito? Die Antwort ist offensichtlich, Nein, es folglich wechselt in die zweite Regel. Ist in der Menge der jeder anonym? Da die Antwort hier ist "Ja", die `<deny>` Regel wird wirksam und der Besucher auf der Anmeldeseite umgeleitet wird. Auf ähnliche Weise, wenn das Unternehmen besuchende Jisun ist die `UrlAuthorizationModule` beginnt mit dem anfordern, wird Jisun Scott oder Tito? Da sie nicht der Fall ist die `UrlAuthorizationModule` geht auf die zweite Frage ist Jisun im Satz von jedem? Sie ist, damit er, ebenfalls der Zugriff verweigert wird. Abschließend, wenn Tito besucht, die erste Frage Bedrohung durch die `UrlAuthorizationModule` ist eine positive Antwort Tito Zugriff gewährt wird.

Da die `UrlAuthorizationModule` Prozesse, die die Autorisierungsregeln von oben nach unten beenden am Übereinstimmung, es ist wichtig, die spezifischen Regeln, die vor den weniger spezifischen ab stehen haben. D. h. um Autorisierungsregeln zu definieren, die Jisun und anonyme Benutzer unterbunden, aber alle anderen authentifizierten Benutzern erlauben, Sie zunächst die spezifischste Regel – die eine weitreichendsten Jisun - und fahren Sie mit den weniger spezifischen Regeln - den alle anderen zulassen Authentifizierte Benutzer, aber alle anonymen Benutzer verweigern. Die folgenden URL-Autorisierungsregeln implementiert diese Richtlinie, indem zuerst Jisun verweigern und dann nur alle anonymen Benutzer. Jeder authentifizierte Benutzer als Jisun wird Zugriff gewährt, da beide Methoden `<deny>` Anweisungen entspricht.

[!code-xml[Main](user-based-authorization-cs/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Schritt 2: Korrigieren, und des Workflows für nicht autorisierte, authentifizierten Benutzer

Wie weiter oben in diesem Lernprogramm in der ein sehen Sie sich den Abschnitt für URL-Autorisierung erläutert, können Sie jederzeit eine nicht autorisierte Anforderung herausstellt, die `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Dieser Status 401 geändert wird, indem die `FormsAuthenticationModule` in eine 302 umleiten Status, die der Benutzer auf der Anmeldeseite gesendet. Dieser Workflow erfolgt für alle nicht autorisierten Anforderung, selbst wenn der Benutzer authentifiziert ist.

Zurückgeben eines authentifizierten Benutzers auf der Anmeldeseite wird wahrscheinlich diese verwechseln, da sie bereits in das System angemeldet haben. Mit ein wenig Arbeit können wir dieses Workflows verbessert, durch das Umleiten von authentifizierten Benutzer, die nicht autorisierte Anfragen zu einer Seite, aus der hervorgeht, dass sie versucht haben, eine zugriffsbeschränkte Seite den Zugriff auf.

Erstellen eine neue ASP.NET-Seite im Stammordner der Webanwendung, die mit dem Namen zunächst `UnauthorizedAccess.aspx`; vergessen Sie nicht, ordnen Sie diese Seite mit den `Site.master` Masterseite. Entfernen Sie nach dem Erstellen dieser Seite an, das Inhaltssteuerelement, das verweist auf die `LoginContent` ContentPlaceHolder, damit die Gestaltungsvorlage Standardinhaltstyp wird angezeigt. Als Nächstes fügen Sie eine Meldung, die Situation, nämlich erläutert wird, dass der Benutzer hat versucht, eine geschützte Ressource zuzugreifen. Nach dem Hinzufügen einer solchen Nachricht die `UnauthorizedAccess.aspx` deklarativen Seitenmarkup sollte etwa wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-cs/samples/sample7.aspx)]

Jetzt müssen wir den Workflow zu ändern, sodass sie eine nicht autorisierte Anforderung von einem authentifizierten Benutzer durchgeführt wird, gesendet werden die `UnauthorizedAccess.aspx` Seite anstelle der Anmeldeseite. Die Logik, die nicht autorisierte Anfragen an die Anmeldeseite umgeleitet wird in eine private Methode von verborgen der `FormsAuthenticationModule` Klasse, damit wir dieses Verhalten nicht anpassen kann. Was wir jedoch ausführen kann, ist unser eigenes Logik zur Anmeldeseite hinzufügen, die den Benutzer umleitet `UnauthorizedAccess.aspx`, sofern erforderlich.

Wenn die `FormsAuthenticationModule` leitet einen nicht autorisierten Besucher zur Anmeldeseite fügt er die angeforderte, nicht autorisierte URL, die Abfragezeichenfolge mit dem Namen `ReturnUrl`. Angenommen, ein nicht autorisierter Benutzer versucht, besuchen `OnlyTito.aspx`, `FormsAuthenticationModule` würde weiterleiten, damit `Login.aspx?ReturnUrl=OnlyTito.aspx`. Daher, wenn die Anmeldeseite von einem authentifizierten Benutzer mit einer Abfragezeichenfolge erreicht wird, die enthält die `ReturnUrl` Parameter, und wir wissen, dass dieser nicht authentifizierte Benutzer nur versucht, eine Seite besuchen sie keine zum Anzeigen Berechtigung. In einem solchen Fall ist es ihr, umleiten möchten `UnauthorizedAccess.aspx`.

Um dies zu erreichen, fügen Sie den folgenden Code der Anmeldeseite `Page_Load` Ereignishandler:

[!code-csharp[Main](user-based-authorization-cs/samples/sample8.cs)]

Der obige Code leitet authentifiziert ist, nicht autorisierte Benutzer auf die `UnauthorizedAccess.aspx` Seite. Um diese Logik in Aktion zu sehen, wie eine anonyme Besucher der Website, und klicken Sie auf den Link Erstellen von Benutzerkonten in der linken Spalte auf. Dadurch gelangen Sie zu der `~/Membership/CreatingUserAccounts.aspx` Seite, die in Schritt 1 wird so, dass nur konfiguriert Tito Zugriff erhält. Da anonyme Benutzer nicht zulässig ist, die `FormsAuthenticationModule` uns zurück an die Anmeldeseite umgeleitet.

An diesem Punkt sind wir anonym, sodass `Request.IsAuthenticated` gibt `false` und wir werden nicht auf umgeleitet `UnauthorizedAccess.aspx`. Stattdessen wird die Anmeldeseite angezeigt. Melden Sie sich mit einem anderen Benutzer als Tito, z. B. Bruce. Geben Sie die entsprechenden Anmeldeinformationen ein, die Anmeldeseite leitet uns wieder an `~/Membership/CreatingUserAccounts.aspx`. Jedoch, da auf dieser Seite nur für Tito zugänglich ist, wir sind nicht autorisiert, um ihn anzuzeigen und werden zur Anmeldeseite sofort zurückgegeben. Dieses Mal jedoch `Request.IsAuthenticated` gibt `true` (und die `ReturnUrl` Querystring-Parameter vorhanden ist), sodass wir, um umgeleitet werden die `UnauthorizedAccess.aspx` Seite.


[![Authentifiziert sind, werden nicht autorisierte Benutzer zu UnauthorizedAccess.aspx umgeleitet.](user-based-authorization-cs/_static/image17.png)](user-based-authorization-cs/_static/image16.png)

**Abbildung 6**: authentifiziert, nicht autorisierte Benutzer umgeleitet werden `UnauthorizedAccess.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image18.png))


Diese benutzerdefinierten Workflows sorgt für eine mehr aussagefähiger und unkompliziert Benutzer von Kurzschlussoperationen den Zyklus in Abbildung 2 dargestellt.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Schritt 3: Funktionalität, die basierend auf dem derzeit angemeldeten Benutzer beschränkt.

URL-Autorisierung vereinfacht die grob Autorisierungsregeln angeben. Wie in Schritt 1 wurde erläutert, können mit URL-Autorisierung wir kurz: welche Identitäten zulässig sind und welche verweigert werden, eine bestimmte Seite oder alle Seiten in einem Ordner anzeigen In bestimmten Szenarien kann, wir möchten jedoch ermöglicht allen Benutzern finden Sie auf einer Seite, aber der Seite funktionseinschränkungen basierend auf den Besucher.

Betrachten Sie den Fall einer e-Commerce-Website, die authentifizierten Besucher ihrer Produkte überprüfen können. Wenn ein anonymer Benutzer ein Produkt Seite besucht, wird sie sehen nur die Produktinformationen und würde nicht die Möglichkeit, eine Kritik zu lassen gegeben werden. Allerdings würde ein authentifizierter Benutzer Zugriff auf derselben Seite Überprüfung Schnittstelle finden Sie unter. Wenn der authentifizierte Benutzer dieses Produkt noch nicht überprüft wurde, würde die Schnittstelle sie senden Sie eine Überprüfung aktivieren; Andernfalls würden sie sie zuvor gesendeten Überprüfung anzuzeigen. Um dieses Szenario einen Schritt nehmen Sie darüber hinaus die Produktseite möglicherweise zusätzliche Informationen anzeigen und bieten erweiterte Funktionen für Benutzer, die für das e-Commerce-Unternehmen arbeiten. Beispielsweise kann die Produktseite Liste der Bestand im Lager und enthalten Optionen, um des Produkts Preis und eine Beschreibung, die von einem Mitarbeiter besucht bearbeiten.

Solche möglich, differenzierte Autorisierungsregeln können entweder deklarativ oder programmgesteuert (oder über eine Kombination aus beiden) implementiert werden. Im nächsten Abschnitt sehen wir, wie möglich, differenzierte Autorisierung über das LoginView-Steuerelement implementiert werden. Danach wird programmgesteuerte Techniken vorgestellt. Bevor wir Autorisierungsregeln möglich, differenzierte anwenden untersuchen können, müssen jedoch wir zuerst erstellen Sie eine Seite der Besucher, deren Funktionalität abhängig.

Erstellen Sie eine Seite, die die Dateien in einem bestimmten Verzeichnis innerhalb einer GridView auflistet. Auflisten von Namen für jede Datei, Größe und andere Informationen, die GridView wird enthalten neben LinkButtons zwei Spalten: eine mit dem Titel anzeigen und mit dem Titel löschen. Wenn die Ansicht LinkButton geklickt wird, werden der Inhalt der ausgewählten Datei angezeigt; Wenn die LinkButton Löschen geklickt wird, wird die Datei gelöscht. Zunächst erstellen Sie diese Seite, dass die Ansicht "und" Delete-Funktionen für alle Benutzer verfügbar sind. In der mit den Abschnitten LoginView-Steuerelement und Funktionalität programmgesteuert beschränken, die wir sehen werden, wie Sie aktivieren oder deaktivieren diese Funktionen, die basierend auf dem Benutzer Zugriff auf die Seite.

> [!NOTE]
> Die ASP.NET-Seite an, die wir erstellen verwendet eine GridView-Steuerelements, um eine Liste der Dateien anzuzeigen. Seit diesem Lernprogramm Reihe auf formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, sollten ich keine Erörterung der internen Funktionsweise von des GridView-Steuerelements zu viel Zeit. Während dieses Lernprogramm spezifische schrittweise Anweisungen zum Einrichten dieser Seite enthält, ist es nicht stürzen die Details Warum Auswahl bestimmter vorgenommen wurden, oder was auf der gerenderten Ausgabe Auswirkungen bestimmte Eigenschaften haben. Eine gründliche Prüfung des GridView-Steuerelements, finden Sie in meinem *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Reihe von Lernprogrammen.


Öffnen Sie zunächst die `UserBasedAuthorization.aspx` in der Datei die `Membership` Ordner und Hinzufügen eines GridView-Steuerelements auf der Seite mit dem Namen `FilesGrid`. Der GridView Smart Tag klicken Sie auf den Spalten bearbeiten-Link, um das Dialogfeld Felder zu starten. Von hier aus, deaktivieren Sie Kontrollkästchen Felder automatisch generiert, in der unteren linken Ecke. Fügen Sie eine Schaltfläche auswählen, eine Schaltfläche "löschen" und zwei BoundFields aus der oberen linken Ecke (die Schaltflächen "auswählen" und "Delete" kann unter den CommandField Typ gefunden). Legen Sie der Schaltfläche auswählen `SelectText` Eigenschaft anzeigen und die erste BoundField- `HeaderText` und `DataField` Eigenschaften mit Namen. Legen Sie die zweite BoundField- `HeaderText` Eigenschaft, um die Größe in Bytes, die `DataField` Eigenschaft auf die Länge, seine `DataFormatString` Eigenschaft, um {0: N0} und die zugehörige `HtmlEncode` Eigenschaft auf "false".

Nach dem Konfigurieren der GridView-Spalten, klicken Sie auf OK, um das Dialogfeld "Felder" zu schließen. Legen Sie im Fenster Eigenschaften der GridView `DataKeyNames` Eigenschaft `FullName`. An diesem Punkt sollte die GridView deklarative Markup wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-cs/samples/sample9.aspx)]

Durch die GridView-Markup erstellt haben können wir den Code zu schreiben, der die Dateien in einem bestimmten Verzeichnis abrufen und an die GridView zu binden. Fügen Sie den folgenden Code auf der Seite `Page_Load` Ereignishandler:

[!code-csharp[Main](user-based-authorization-cs/samples/sample10.cs)]

Der obige Code verwendet die [ `DirectoryInfo` Klasse](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) um eine Liste der Dateien im Stammordner der Anwendung abzurufen. Die [ `GetFiles()` Methode](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) aller Dateien im Verzeichnis zurückgegeben, als ein Array von [ `FileInfo` Objekte](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), die dann an die GridView gebunden ist. Die `FileInfo` Objekt verfügt über eine Sammlung von Eigenschaften, z. B. `Name`, `Length`, und `IsReadOnly`, o. ä. Wie Sie aus der deklarativem Markup sehen können, zeigt die GridView lediglich die `Name` und `Length` Eigenschaften.

> [!NOTE]
> Die `DirectoryInfo` und `FileInfo` Klassen befinden sich in der [ `System.IO` Namespace](https://msdn.microsoft.com/library/system.io.aspx). Aus diesem Grund werden Sie entweder erforderlich, stellen Sie diese Klassennamen mit ihren Namespacenamen oder den Namespace in die Klassendatei importiert haben (über `using System.IO`).


Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser besuchen. Es zeigt die Liste der Dateien, die sich im Stammverzeichnis der Anwendung befinden. Durch Klicken auf die Ansicht bzw. löschen LinkButtons führt dazu, dass einen Postback, jedoch keine Aktion tritt auf, da wir noch, haben die erforderlichen Ereignishandler zu erstellen.


[![Die GridView Listet die Dateien im Stammverzeichnis der Webanwendung](user-based-authorization-cs/_static/image20.png)](user-based-authorization-cs/_static/image19.png)

**Abbildung 7**: GridView Listet die Dateien im Stammverzeichnis der Webanwendung ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image21.png))


Wir benötigen eine Möglichkeit, um den Inhalt der ausgewählten Datei anzuzeigen. Zurück zu Visual Studio, und fügen Sie ein Textfeld mit dem Namen `FileContents` oben GridView. Legen Sie seine `TextMode` Eigenschaft, um `MultiLine` und dessen `Columns` und `Rows` Eigenschaften 95 %, 10, bzw.

[!code-aspx[Main](user-based-authorization-cs/samples/sample11.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für der GridView [ `SelectedIndexChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](user-based-authorization-cs/samples/sample12.cs)]

Dieser Code verwendet der GridView `SelectedValue` -Eigenschaft zum Bestimmen der vollständige Dateiname der ausgewählten Datei. Intern die `DataKeys` Auflistung verwiesen wird, um das Abrufen der `SelectedValue`, daher ist es zwingend erforderlich, dass Sie festlegen, dass der GridView `DataKeyNames` Eigenschaft Namen, wie weiter oben in diesem Schritt beschrieben. Die [ `File` Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) wird verwendet, um den Inhalt der ausgewählten Datei in eine Zeichenfolge gelesen, die dann zugewiesen wird die `FileContents` Textfeldss `Text` -Eigenschaft, und zeigt den Inhalt der ausgewählten Datei auf der Seite.


[![Inhalt der ausgewählten Datei werden im Textfeld angezeigt.](user-based-authorization-cs/_static/image23.png)](user-based-authorization-cs/_static/image22.png)

**Abbildung 8**: Inhalt der ausgewählten Datei in das Textfeld angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image24.png))


> [!NOTE]
> Wenn Sie Anzeigen des Inhalts einer Datei, die HTML-Markup enthält, und dann versuchen, anzeigen oder Löschen einer Datei, erhalten Sie eine `HttpRequestValidationException` Fehler. Dies tritt auf, weil beim Postback Inhalt des Textfelds zurück an den Webserver gesendet werden. Löst standardmäßig ASP.NET eine `HttpRequestValidationException` Fehler, wenn potenziell gefährliche postback Inhalt, z. B. HTML-Markup erkannt wird. Um diesen Fehler aus, zu deaktivieren, deaktivieren Sie anforderungsüberprüfung für die Seite durch Hinzufügen von `ValidateRequest="false"` auf die `@Page` Richtlinie. Weitere Informationen zu den Vorteilen der anforderungsüberprüfung als sowie welche Vorsichtsmaßnahmen Sie beim Ausführen sollten, deaktivieren, finden Sie unter [Anforderungsvalidierung - Skript-Angriffe verhindert](https://asp.net/learn/whitepapers/request-validation/).


Fügen Sie abschließend einen Ereignishandler durch den folgenden Code hinzu, für der GridView [ `RowDeleting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-csharp[Main](user-based-authorization-cs/samples/sample13.cs)]

Der Code einfach zeigt den vollständigen Namen der Datei, löschen Sie in der `FileContents` Textfeld *ohne* tatsächlich die Datei zu löschen.


[![Klicken auf die Schaltfläche "löschen" wird tatsächlich die Datei löschen nicht](user-based-authorization-cs/_static/image26.png)](user-based-authorization-cs/_static/image25.png)

**Abbildung 9**: auf die Datei der löschen Schaltfläche wird nicht tatsächlich gelöscht ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image27.png))


In Schritt 1 konfiguriert es die URL-Autorisierungsregeln, um zu verhindern, dass anonyme Benutzer anzeigen von Seiten in der `Membership` Ordner. Um möglich, differenzierte Authentifizierung eine bessere Leistung aufweisen, können wir anonyme Benutzer besuchen der `UserBasedAuthorization.aspx` Seite, aber mit eingeschränkter Funktionalität. Um diese Seite zu öffnen Sie durch alle Benutzer zugegriffen werden kann, fügen Sie die folgenden `<location>` Element auf der `Web.config` in der Datei die `Membership` Ordner:

[!code-xml[Main](user-based-authorization-cs/samples/sample14.xml)]

Nach dem Hinzufügen dieses `<location>` -Element, die neue URL-Autorisierungsregeln durch Anmeldung aus der Website testen. Als anonymer Benutzer sollten Sie über eine Berechtigung für besuchen die `UserBasedAuthorization.aspx` Seite.

Jeder authentifizierte oder anonyme Benutzer besuchen derzeit die `UserBasedAuthorization.aspx` Seite und anzeigen oder Löschen von Dateien. Stellen sie so, dass nur authentifizierte Benutzer können den Inhalt einer Datei anzeigen, und nur Tito kann eine Datei gelöscht. Solche möglich, differenzierte Autorisierungsregeln können deklarativ, programmgesteuert oder über eine Kombination beider Methoden angewendet werden. Ermöglicht die Verwendung deklarativen Ansatzes zum einschränken, wer den Inhalt einer Datei anzeigen kann; Wir verwenden die programmgesteuerten Ansatz bis zur Grenze, die eine Datei löschen kann.

### <a name="using-the-loginview-control"></a>Verwenden das LoginView-Steuerelement

Wie wir im letzten Lernprogramme gesehen haben, das LoginView-Steuerelement eignet sich zum Anzeigen von anderen Schnittstellen für authentifizierte und anonyme Benutzer, und bietet ein einfaches Verfahren, um Funktionen auszublenden, die für anonyme Benutzer nicht zugänglich ist. Da anonyme Benutzer können nicht anzeigen oder Löschen von Dateien, wir brauchen nur zum Anzeigen der `FileContents` Textfeld aus, wenn die Seite von einem authentifizierten Benutzer besucht wird. Um dies zu erreichen, die Seite ein LoginView-Steuerelement hinzugefügt haben, nennen Sie sie `LoginViewForFileContentsTextBox`, und verschieben Sie die `FileContents` Textfeldss deklarativem Markup in des LoginView-Steuerelements `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-cs/samples/sample15.aspx)]

Die Web-Steuerelemente in der LoginView-Vorlagen sind nicht mehr direkt aus der CodeBehind Klasse zugegriffen werden. Z. B. die `FilesGrid` GridView `SelectedIndexChanged` und `RowDeleting` Ereignishandler derzeit verweisen die `FileContents` wie TextBox-Steuerelement durch Code:

[!code-csharp[Main](user-based-authorization-cs/samples/sample16.cs)]

Dieser Code ist jedoch nicht mehr gültig. Durch Verschieben der `FileContents` Textfeld in der `LoggedInTemplate` Textfeld kann nicht direkt zugegriffen werden. Stattdessen müssen wir verwenden die `FindControl("controlId")` Methode, um das Steuerelement programmgesteuert zu verweisen. Update der `FilesGrid` -Ereignishandler auf das Textfeld verweisen wie folgt:

[!code-csharp[Main](user-based-authorization-cs/samples/sample17.cs)]

Nach dem Verschieben des Textfelds auf die LoginView `LoggedInTemplate` und aktualisieren den Code der Seite zu verweisen, die das Textfeld mit dem `FindControl("controlId")` Muster, besuchen Sie die Seite als anonymer Benutzer. Wie in Abbildung 10 gezeigt, die `FileContents` Textfeld wird nicht angezeigt. Die Ansicht LinkButton ist jedoch weiterhin angezeigt.


[![LoginView-Steuerelement gerendert wird nur das Textfeld FileContents für authentifizierte Benutzer](user-based-authorization-cs/_static/image29.png)](user-based-authorization-cs/_static/image28.png)

**Abbildung 10**: die LoginView-Steuerelement nur rendert die `FileContents` TextBox für authentifizierte Benutzer ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image30.png))


Eine Möglichkeit zum Ausblenden der Schaltfläche "anzeigen" für anonyme Benutzer werden die GridView-Feld in ein TemplateField konvertiert. Dadurch wird eine Vorlage, die für die Sicht LinkButton deklarative Markup enthält. Wir können dann die TemplateField LoginView-Steuerelement hinzu, und platzieren LinkButton innerhalb der LoginView `LoggedInTemplate`, wodurch die Schaltfläche "Ansicht" anonyme Besucher von ausblenden. Um dies zu erreichen, klicken Sie auf die Verknüpfung Spalten bearbeiten der GridView-Smarttag, um die Felder (Dialogfeld) zu starten. Als nächstes wählen Sie die Schaltfläche auswählen, aus der Liste in der unteren linken Ecke aus, und klicken Sie dann auf dieses Feld auf einen Link TemplateField konvertieren. Auf diese Weise wird das Feld deklarativem Markup aus ändern:

[!code-aspx[Main](user-based-authorization-cs/samples/sample18.aspx)]

 Nach: 

[!code-aspx[Main](user-based-authorization-cs/samples/sample19.aspx)]

An diesem Punkt können wir die TemplateField eine LoginView hinzufügen. Das folgende Markup zeigt die Ansicht LinkButton nur für authentifizierte Benutzer.

[!code-aspx[Main](user-based-authorization-cs/samples/sample20.aspx)]

Wie Abbildung 11 zeigt, ist das Endergebnis nicht, dass alles ziemlich wie die Sicht Spalte weiterhin angezeigt wird, obwohl die Sicht LinkButtons innerhalb der Spalte ausgeblendet sind. Wir werden im nächsten Abschnitt zum Ausblenden von der gesamten GridView-Spalte (und nicht nur die LinkButton) betrachten.


[![LoginView-Steuerelement blendet Sie aus der Sicht LinkButtons für anonyme Besucher](user-based-authorization-cs/_static/image32.png)](user-based-authorization-cs/_static/image31.png)

**Abbildung 11**: LoginView-Steuerelement blendet Sie aus der Sicht LinkButtons für anonyme Besucher ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funktionalität programmgesteuert beschränkt.

In einigen Fällen reichen deklarative Techniken zum Einschränken der Funktionen zu einer Seite. Die Verfügbarkeit von bestimmten Funktionen Seite möglicherweise z. B. Kriterien hinausgehen, ob der Benutzer Zugriff auf die Seite anonym oder authentifizierte abhängig. In solchen Fällen können die verschiedenen Elemente der Benutzeroberfläche angezeigt oder durch programmgesteuerte Möglichkeit ausgeblendet werden.

Um die Funktionalität programmgesteuert zu beschränken, müssen wir zwei Aufgaben ausführen:

1. Bestimmen, ob der Benutzer Zugriff auf der Seite auf die Funktionalität zugreifen kann und
2. Programmgesteuert ändern der Benutzeroberfläche, die Grundlage, ob der Benutzer Zugriff auf die betreffenden Funktionen verfügt.

Um die Anwendung dieser beiden Aufgaben zu veranschaulichen, sehen wir nur die Tito zum Löschen von Dateien aus der GridView erlauben. Unsere erste Aufgabe ist, um festzustellen, ob es Tito besuchen die Seite ist. Sobald, ermittelt wurde, müssen wir die GridView Delete Spalte auszublenden (oder anzeigen). Die GridView-Spalten werden über seine `Columns` Eigenschafts-wird nur eine Spalte gerendert, wenn seine `Visible` -Eigenschaftensatz auf `true` (Standard).

Fügen Sie folgenden Code, der `Page_Load` -Ereignishandler vor dem Binden der Daten an die GridView:

[!code-csharp[Main](user-based-authorization-cs/samples/sample21.cs)]

Wie in erläutert die [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) Lernprogramm `User.Identity.Name` gibt den Namen der Identität. Dies entspricht in das Steuerelement für die Anmeldung beim eingegebenen Benutzernamen. Tito besuchen die Seite, die GridView zweite Spalte ist `Visible` -Eigenschaftensatz auf `true`ist, andernfalls wird festgelegt ist, dass `false`. Das Ergebnis ist, dass wenn eine Person als Tito Seite, ein anderer authentifizierten Benutzer oder einen anonymen Benutzer besucht die Spalte Löschen nicht (siehe Abbildung 12); gerendert wird Wenn Tito auf die Seite besucht, die Spalte löschen ist jedoch vorhanden (siehe Abbildung 13).


[![Die Spalte löschen, wird nicht gerendert beim besucht durch eine Person außer Tito (z. B. Bruce)](user-based-authorization-cs/_static/image35.png)](user-based-authorization-cs/_static/image34.png)

**Abbildung 12**: Spalte für das Löschen wird nicht gerendert beim besucht durch eine Person außer Tito (z. B. Bruce) ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image36.png))


[![Die Spalte löschen wird für Tito gerendert.](user-based-authorization-cs/_static/image38.png)](user-based-authorization-cs/_static/image37.png)

**Abbildung 13**: die Spalte mit löschen für Tito gerendert wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Schritt 4: Anwenden von Autorisierungsregeln auf Klassen und Methoden

In Schritt 3 werden nicht zulässig für anonyme Benutzer anzeigen des Inhalts einer Datei und Löschen von Dateien auf alle Benutzer außer Tito untersagt. Dies wurde durch das Ausblenden von der zugehörigen Elemente der Benutzeroberfläche für nicht autorisierte Besucher über deklarative und programmgesteuerte Verfahren erreicht. In unserem Beispiel einfache ordnungsgemäß die Elemente der Benutzeroberfläche ausgeblendet wurde, einfache, aber wie sieht eine komplexere Standorten, wobei es möglicherweise viele verschiedene Arten, führen Sie die gleiche Funktionalität? In dieser Funktionalität für nicht autorisierte Benutzer beschränkt, was geschieht, wenn wir ausblenden oder deaktivieren alle Elemente der entsprechenden Benutzeroberfläche vergessen?

Eine einfache Möglichkeit, stellen Sie sicher, dass ein nicht autorisierter Benutzer auf ein bestimmter Teil der Funktionalität zugreifen kann diese Klasse oder Methode mit ergänzt ist die [ `PrincipalPermission` Attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Wenn der .NET Common Language Runtime eine Klasse verwendet oder eine der Methoden führt, überprüft stellen Sie sicher, dass der aktuelle Sicherheitskontext verfügt über die Berechtigung zum Verwenden der Klasse, oder führen Sie die Methode. Die `PrincipalPermission` Attribut bietet einen Mechanismus, der über den können wir diese Regeln definieren.

Wir veranschaulicht die Verwendung der `PrincipalPermission` Attribut für der GridView `SelectedIndexChanged` und `RowDeleting` Ereignishandler Ausführung durch anonyme Benutzer, und als Tito, bzw. verhindert werden soll. Alles, was erforderlich ist, das entsprechende Attribut über jede Funktionsdefinition hinzufügen:

[!code-csharp[Main](user-based-authorization-cs/samples/sample22.cs)]

Das Attribut für die `SelectedIndexChanged` Ereignishandler gibt an, die nur Benutzer authentifizierte können den Ereignishandler ausführen, wobei als das Attribut auf die `RowDeleting` Ereignishandler beschränkt die Ausführung des Tito.

Wenn aus irgendeinem Grund, ein anderen Benutzer als Tito auszuführen versucht die `RowDeleting` Ereignishandler oder eine nicht authentifizierte Benutzer versucht, führen Sie die `SelectedIndexChanged` Ereignishandler, d. h. der .NET Common Language Runtime löst eine `SecurityException`.


[![Wenn der Sicherheitskontext zum Ausführen der Methode nicht autorisiert ist, wird eine SecurityException ausgelöst.](user-based-authorization-cs/_static/image41.png)](user-based-authorization-cs/_static/image40.png)

**Abbildung 14**: Wenn der Sicherheitskontext nicht, zur Ausführung der Methode autorisiert ist ein `SecurityException` ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](user-based-authorization-cs/_static/image42.png))


> [!NOTE]
> Um mehrere Sicherheitskontexten Zugriff auf eine Klasse oder Methode zuzulassen, ergänzen Sie die Klasse oder Methode mit einem `PrincipalPermission` Attribut für jede Sicherheitskontext. D. h. um Tito und Bruce ausführen können die `RowDeleting` Ereignishandler, d. h. hinzufügen *zwei* `PrincipalPermission` Attribute:


[!code-csharp[Main](user-based-authorization-cs/samples/sample23.cs)]

Zusätzlich zu ASP.NET-Seiten haben viele Anwendungen auch eine Architektur, die verschiedenen Ebenen, wie z. B. von Geschäftslogik und Datenzugriffsebenen Daten enthält. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftsfunktionen Logik und Daten, die verknüpft. Die `PrincipalPermission` Attribut eignet sich für das Anwenden von Autorisierungsregeln auf diesen Ebenen gehört.

Weitere Informationen zur Verwendung der `PrincipalPermission` Attribut Autorisierungsregeln für Klassen und Methoden definieren, finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [Hinzufügen von Autorisierungsregeln zu Business und Ebenen mithilfe`PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde erläutert, wie benutzerbasierte Autorisierungsregeln angewendet. Wir beginnen mit einem Blick auf ASP. NET URL-Autorisierung-Framework. Für jede Anforderung, das Modul für ASP.NET `UrlAuthorizationModule` untersucht die URL-Autorisierungsregeln, die definiert, in der Anwendungskonfigurationsdatei, um zu bestimmen, ob die Identität autorisiert ist, auf die angeforderte Ressource zuzugreifen. Kurz gesagt, vereinfacht URL-Autorisierung Autorisierungsregeln für eine bestimmte Seite oder für alle Seiten in einem bestimmten Verzeichnis angeben.

Die URL-Autorisierungsframework gilt Autorisierungsregeln auf Basis von Seite. Mit URL-Autorisierung ist entweder die anfordernde Identität autorisiert, auf eine bestimmte Ressource zugreifen, oder nicht. Viele Szenarien, rufen Sie jedoch für weitere möglich, differenzierte Autorisierungsregeln. Anstatt zu definieren, die Zugriff auf eine Seite zulässig ist, müssen wir möglicherweise "Jeder" Zugriff auf eine Seite zu ermöglichen, aber das Anzeigen von anderer Daten oder bieten unterschiedliche Funktionen aufgerufen, abhängig von der Benutzer Zugriff auf die Seite. Autorisierung auf Seitenebene bedingt normalerweise zum Ausblenden von bestimmte Elemente der Benutzeroberfläche, um zu verhindern, dass nicht autorisierte Benutzer Zugriff auf unzulässige Funktion. Darüber hinaus ist es möglich, die Attribute verwenden, um den Zugriff auf Klassen und Ausführung der Methoden für bestimmte Benutzer beschränken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungsregeln für Geschäft und Ebenen der Daten mithilfe von `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Unterschiede zwischen der IIS 6 und IIS 7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurieren bestimmte Dateien und Unterverzeichnisse](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Datenfunktionen Änderung basierend auf dem Benutzer beschränken](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs.md)
- [LoginView-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Grundlegendes zu IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](validating-user-credentials-against-the-membership-user-store-cs.md)
> [Weiter](storing-additional-user-information-cs.md)
