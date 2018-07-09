---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Benutzerbasierte Autorisierung (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial betrachten wir Beschränken des Zugriffs auf Seiten und Funktionen über eine Vielzahl von Techniken für auf Seitenebene eingeschränkt.
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: 107983494350ddc06b6d3a20557baff4f4e6f9f4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834373"
---
<a name="user-based-authorization-vb"></a>Benutzerbasierte Autorisierung (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> In diesem Tutorial betrachten wir Beschränken des Zugriffs auf Seiten und Funktionen über eine Vielzahl von Techniken für auf Seitenebene eingeschränkt.


## <a name="introduction"></a>Einführung

Die meisten Webanwendungen, die Benutzerkonten bieten holen Sie dies im Teil, um bestimmte Besucher aus den Zugriff auf bestimmte Seiten innerhalb der Website zu beschränken. Die meisten online-Diskussionsforum ein-Websites z. B. alle Benutzer - anonyme und authentifizierte - Diskussionsforum vorhanden ist, wird der ein Beiträge anzeigen können, aber nur authentifizierte Benutzer besuchen die Website eine neue Bereitstellung erstellen. Und es gibt möglicherweise administrative Seiten, die nur für einen bestimmten Benutzer (oder eine bestimmte Gruppe von Benutzern) zugänglich sind. Darüber hinaus kann sich auf Seitenebene Funktionalität pro Benutzer mit dem Benutzernamen unterscheiden. Wenn Sie eine Liste der Beiträge anzeigen zu können, werden authentifizierte Benutzern eine Schnittstelle für jeden Beitrag, Bewertung angezeigt, während diese Schnittstelle nicht verfügbar für anonyme Besucher ist.

ASP.NET vereinfacht das benutzerbasierte Autorisierungsregeln zu definieren. Mit geringem des Markups in `Web.config`, bestimmte Webseiten oder ganze Verzeichnisse können gesperrt werden, damit sie nur für eine angegebene Teilmenge von Benutzern zugänglich sind. Auf Seitenebene Funktionalität kann ein- oder ausschalten basierend auf dem aktuell angemeldeten Benutzer auf programmgesteuerte und deklarative Weise aktiviert werden.

In diesem Tutorial betrachten wir Beschränken des Zugriffs auf Seiten und Funktionen über eine Vielzahl von Techniken für auf Seitenebene eingeschränkt. Fangen wir an!

## <a name="a-look-at-the-url-authorization-workflow"></a>Ein Blick auf den URL-Autorisierung-Workflow

Siehe die [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Tutorial, wenn die ASP.NET-Laufzeitumgebung eine Anforderung verarbeitet, für eine ASP.NET-Ressource die Anforderung eine Anzahl von Ereignissen während des Lebenszyklus löst. *HTTP-Module* sind verwaltete Klassen, deren Code als Reaktion auf ein bestimmtes Ereignis im Anforderungslebenszyklus von ausgeführt wird. Im Lieferumfang von ASP.NET sind einer Reihe von HTTP-Modulen, die Durchführung wichtiger Aufgaben im Hintergrund.

Ein solches HTTP-Modul ist [ `FormsAuthenticationModule` ](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Wie in die primäre Funktion des vorherigen Tutorials erläutert die `FormsAuthenticationModule` besteht darin, die Identität des die aktuelle Anforderung zu bestimmen. Dies erfolgt durch Überprüfen das Formularauthentifizierungsticket aus, die entweder in einem Cookie gespeichert oder in der URL eingebettet ist. Diese Identifikation findet statt, während die [ `AuthenticateRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Eine andere wichtige HTTP-Modul ist der [ `UrlAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), die ausgelöst wird, als Reaktion auf die [ `AuthorizeRequest` Ereignis](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (dies der Fall. nach der `AuthenticateRequest` Ereignis). Die `UrlAuthorizationModule` untersucht konfigurationsmarkup in `Web.config` bestimmen, ob die Identität des aktuelle Autorität für die angegebene Seite besuchen verfügt. Dieser Prozess wird als bezeichnet *URL-Autorisierung*.

Untersucht die Syntax für die URL-Autorisierungsregeln in Schritt 1, aber zuerst sehen wir sehen Sie sich, was die `UrlAuthorizationModule` ist abhängig davon, ob die Anforderung oder nicht autorisiert ist. Wenn die `UrlAuthorizationModule` feststellt, dass die Anforderung autorisiert wird, und klicken Sie dann und keine Auswirkungen hat, und die Anforderung in seinem gesamten Lebenszyklus weiterhin. Allerdings ist die Anforderung *nicht* autorisiert, die `UrlAuthorizationModule` bricht ab, den Lebenszyklus und weist die `Response` zurückzugebenden Objekts ein [HTTP 401 Unauthorized](http://www.checkupdown.com/status/E401.html) Status. Bei Verwendung der Formularauthentifizierung dieser HTTP 401-Status wird nie zurückgegeben, an den Client da Wenn die `FormsAuthenticationModule` erkennt eine HTTP 401-Status wird geändert, damit ein [HTTP 302-Umleitung](http://www.checkupdown.com/status/E302.html) zur Anmeldeseite.

Abbildung 1 veranschaulicht den Workflow der ASP.NET-Pipeline die `FormsAuthenticationModule`, und die `UrlAuthorizationModule` Wenn eine nicht autorisierte Anforderung empfangen wird. Abbildung 1 zeigt insbesondere, eine Anfrage von einem anonymen Besucher für `ProtectedPage.aspx`, d.h., dass eine Seite, die für anonyme Benutzer den Zugriff verweigert. Da der Besucher anonym ist, wird die `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Die `FormsAuthenticationModule` anschließend konvertiert der 401-Status in eine 302-Umleitung zur Anmeldeseite. Nach der Authentifizierung des Benutzers über die Anmeldeseite umgeleitet `ProtectedPage.aspx`. Dieses Mal die `FormsAuthenticationModule` identifiziert den Benutzer basierend auf seinem Authentifizierungsticket. Nun, da der Besucher authentifiziert wurde, die `UrlAuthorizationModule` ermöglicht Zugriff auf die Seite.


[![Die Formularauthentifizierung und den URL-Autorisierungsworkflow](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Abbildung 1**: die Formularauthentifizierung und Workflow für URL-Autorisierung ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image3.png))


Abbildung 1 zeigt die Interaktion, die auftritt, wenn ein anonyme Besucher versucht, auf eine Ressource zuzugreifen, die nicht für anonyme Benutzer verfügbar ist. In diesem Fall wird der anonyme Besucher auf die Anmeldeseite für die Seite umgeleitet, die er es wurde versucht, die in der Abfragezeichenfolge angegebene finden Sie unter. Sobald der Benutzer erfolgreich angemeldet hat, werden sie automatisch an die Ressource umgeleitet, die sie ursprünglich versucht hat, um anzuzeigen.

Wenn die nicht autorisierte Anforderung von einem anonymen Benutzer erfolgt, wird dieser Workflow ist unkompliziert und ist einfach, für den Besucher zu verstehen, was passiert ist und warum. Aber beachten Sie folgende Punkte, die die `FormsAuthenticationModule` leitet *alle* nicht autorisierte Benutzer die Anmeldeseite, selbst wenn von einem authentifizierten Benutzer angefordert wird. Dies kann zu einer verwirrenden nutzererfahrung führen, wenn ein authentifizierter Benutzer versucht, eine Seite zu besuchen, für die sie die Autorität für die fehlt.

Stellen Sie sich vor, dass unsere Website, die URL-Autorisierungsregeln konfiguriert haben, dass der ASP.NET-Seite `OnlyTito.aspx` Zugriff nur für Tito wurde. Angenommen, Sam, anmeldet und anschließend versucht, besuchen die Site besucht `OnlyTito.aspx`. Die `UrlAuthorizationModule` hält den Lebenszyklus und eine HTTP 401 Unauthorized Rückgabestatus, der die `FormsAuthenticationModule` erkennt und an Sam zur Anmeldeseite weitergeleitet. Da Sam bereits angemeldet hat, jedoch vielleicht sie, warum sie zurück zur Anmeldeseite gesendet wurde. Sie können Grund, dass sie die Anmeldeinformationen aus irgendeinem Grund verloren gegangen sind oder sie ungültige Anmeldeinformationen eingegeben haben. Wenn Sam über ihre Anmeldeinformationen auf der Anmeldeseite von erneut sie protokolliert auf (erneut) und zur `OnlyTito.aspx`. Die `UrlAuthorizationModule` erkennt, dass Sam kann nicht auf dieser Seite finden Sie auf, und sie zur Anmeldeseite zurückgegeben werden werden.

Abbildung 2 zeigt diesen Workflow verwirrend.


[![Der Standardworkflow kann zu einem Zyklus verwirrend führen.](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Abbildung 2**: der Standard-Workflow kann führen zu einem Zyklus verwirrend ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image6.png))


Der Workflow, der in Abbildung 2 dargestellten kann sogar die meisten Computer begabte Besucher schnell befuddle. Wir betrachten Möglichkeiten, dies zu verhindern, dass diese verwirrende Zyklus in Schritt2.

> [!NOTE]
> ASP.NET verwendet die zwei Mechanismen, um zu bestimmen, ob der aktuelle Benutzer einer bestimmten Webseite zugreifen kann: URL-Autorisierung und Dateiautorisierung. Dateiautorisierung wird implementiert, indem die [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), der Autorität für die von der angeforderten Dateien ACLs consulting bestimmt. Dateiautorisierung wird am häufigsten mit der Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formularauthentifizierung verwenden, werden alle Anforderungen der Betriebssystem - und -Datei auf Systemebene von dem gleichen Windows-Konto, unabhängig von der Website für den Benutzer ausgeführt. Da dieser tutorialreihe auf Formularauthentifizierung ausgerichtet ist, wird es nicht Dateiautorisierung erörtert.


### <a name="the-scope-of-url-authorization"></a>Der Bereich der URL-Autorisierung

Die `UrlAuthorizationModule` ist verwalteter Code, der Teil der ASP.NET-Laufzeit ist. Vor Version 7 der Microsoft [(Internet Information Services, IIS)](https://www.iis.net/) Webserver, gab es eine unterschiedliche Barriere zwischen IISs-HTTP-Pipeline und der ASP.NET-Laufzeit-Pipeline. Kurz gesagt, in IIS 6 und früher ASP. NET `UrlAuthorizationModule` nur ausgeführt wird, wenn eine Anforderung von IIS für die ASP.NET-Laufzeitumgebung delegiert wird. Standardmäßig IIS selbst – statische Inhalte wie HTML-Seiten und CSS-, JavaScript- und Bilddateien - verarbeitet und übergibt Sie Anforderungen an die ASP.NET-Laufzeitumgebung, wenn eine Seite mit der Erweiterung nur `.aspx`, `.asmx`, oder `.ashx` angefordert wird.

IIS 7, ermöglicht jedoch integrierte IIS und ASP.NET Pipelines. Einige Konfigurationseinstellungen können Sie IIS 7 aufzurufende Einrichten der `UrlAuthorizationModule` für *alle* Anforderungen, was bedeutet, dass die URL-Autorisierungsregeln für Dateien eines beliebigen Typs definiert werden können. Darüber hinaus umfasst IIS 7 eine eigene URL-Autorisierung-Engine. Weitere Informationen zu Integration von ASP.NET und IIS 7 native-URL-Autorisierung-Funktionen, finden Sie unter [Verständnis IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Eine ausführlichere Betrachtung der Integration von ASP.NET und IIS 7, wählen Sie eine Sicherungskopie der Shahram Khosravis Buchs *Professional IIS 7 und ASP.NET integrierte Programmierung* (ISBN: 978-0470152539).

Kurz gesagt, werden in Versionen vor IIS 7, Regeln auf URL-Autorisierung nur auf Ressourcen, die von der ASP.NET-Laufzeit behandelt angewendet. Aber bei IIS 7 ist es möglich, verwenden Sie die systemeigene IIS URL-Autorisierungsfeature oder zur Integration von ASP. NET `UrlAuthorizationModule` in IIS HTTP-Pipeline so erweitern Sie diese Funktion auf alle Anforderungen.

> [!NOTE]
> Es gibt einige geringfügige und dennoch wichtige Unterschiede bei ASP. NET `UrlAuthorizationModule` und IIS 7 URL-Autorisierungsfeature verarbeiten die Autorisierungsregeln. In diesem Tutorial nicht IIS 7-URL-Autorisierung Funktionen oder wie sie Autorisierungsregeln, die im Vergleich zu analysiert die Unterschiede untersucht die `UrlAuthorizationModule`. Weitere Informationen zu diesen Themen finden Sie in der IIS 7-Dokumentation auf MSDN oder [www.iis.net](https://www.iis.net/).


## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Schritt 1: Definieren von URL-Autorisierungsregeln in`Web.config`

Die `UrlAuthorizationModule` bestimmt, ob gewähren oder Verweigern des Zugriffs auf eine angeforderte Ressource für eine bestimmte Identität basierend auf den URL-Autorisierungsregeln in der Konfigurationsdatei der Anwendung definiert. Die Autorisierungsregeln ausgeschrieben werden die [ `<authorization>` Element](https://msdn.microsoft.com/library/8d82143t.aspx) in Form von `<allow>` und `<deny>` untergeordnete Elemente. Jede `<allow>` und `<deny>` untergeordneten-Element angeben:

- Ein bestimmter Benutzer
- Eine durch Trennzeichen getrennte Liste von Benutzern
- Alle anonymen Benutzer, gekennzeichnet durch ein Fragezeichen (?)
- Alle Benutzer, die von einem Sternchen gekennzeichnet (\*)

Das folgende Markup zeigt, wie die URL-Autorisierungsregeln zum gewähren Benutzern Tito und Scott und verweigern alle anderen:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

Die `<allow>` -Element definiert, welche Benutzer - Tito und Scott - dürfen während der `<deny>` Element anweist, dass *alle* Benutzern verweigert werden.

> [!NOTE]
> Die `<allow>` und `<deny>` Elemente können auch die Autorisierungsregeln für Rollen angeben. Wir werden rollenbasierte Autorisierung in einem späteren Tutorial zu untersuchen.


Die folgende Einstellung gewährt Zugriff auf alle Benutzer, die als Sam (darunter auch anonyme Besucher):

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Um nur authentifizierte Benutzer zu ermöglichen, verwenden Sie die folgende Konfiguration, die für alle anonymen Benutzer den Zugriff verweigert:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

Die Autorisierungsregeln werden definiert, in der `<system.web>` Element im `Web.config` und gelten für alle Ressourcen in der Webanwendung ASP.NET. Häufig hat eine Anwendung verschiedene Autorisierungsregeln für die verschiedenen Abschnitte. Z. B. auf einer e-Commerce-Website alle Besucher möglicherweise führen die Produkte, finden Sie unter produktprüfungen, Durchsuchen des Katalogs, usw. Nur authentifizierte Benutzer können jedoch das Auschecken oder die Seiten zum Verwalten der versandverlaufs erreicht werden. Darüber hinaus gibt es möglicherweise Teile des Standorts, die nur auf Benutzer, z. B. Administratoren zugänglich sind.

ASP.NET erleichtert es, um verschiedene Autorisierungsregeln für verschiedene Dateien und Ordner auf der Website zu definieren. Die Autorisierungsregeln, die in des Stammordners des angegebenen `Web.config` Datei, die für alle ASP.NET-Ressourcen auf der Website gelten. Allerdings können diese Standardeinstellungen für die Autorisierung für einen bestimmten Ordner überschrieben werden, durch das Hinzufügen einer `Web.config` mit einer `<authorization>` Abschnitt.

Aktualisieren wir unsere Website, sodass nur authentifizierte Benutzer die ASP.NET-Seiten in besuchen können die `Membership` Ordner. Um dies zu erreichen, wir müssen, einen `Web.config` -Datei in die `Membership` Ordner und legen Sie die autorisierungseinstellungen anonyme Benutzern verweigern. Mit der rechten Maustaste die `Membership` Ordner im Projektmappen-Explorer, wählen Sie das Menü "Neues Element hinzufügen" aus dem Kontextmenü, und fügen Sie eine neue Web-Konfigurationsdatei, die mit dem Namen `Web.config`.


[![Web.config-Datei in den Ordner für Mitgliedschaft hinzufügen](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen einer `Web.config` -Datei in die `Membership` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image9.png))


An diesem Punkt das Projekt sollte enthält zwei `Web.config` Dateien: eine in das Stammverzeichnis und in der `Membership` Ordner.


[![Ihre Anwendung sollte nun zwei Dateien Web.config enthalten.](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Abbildung 4**: Ihre Anwendung sollte jetzt zwei enthalten `Web.config` Dateien ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image12.png))


Aktualisieren Sie die Konfigurationsdatei in die `Membership` Ordner so, dass die It, anonymen Benutzern Zugriff untersagt.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

Das ist alles schon!

Um diese Änderung zu testen, finden Sie auf der Startseite in einem Browser, und stellen Sie sicher, dass Sie sich angemeldet sind. Da das Standardverhalten einer ASP.NET-Anwendung ist, dass alle Besucher, und da wir keine Autorisierung Änderungen in des Stammverzeichnis untergebracht `Web.config` -Datei sind die Dateien im Stammverzeichnis, da eine anonyme Besucher finden Sie unter.

Klicken Sie auf das Erstellen von Benutzerkonten finden Sie in der linken Spalte. Dadurch gelangen Sie zu der `~/Membership/CreatingUserAccounts.aspx`. Da die `Web.config` Datei die `Membership` Ordner definiert Autorisierungsregeln, um den anonymen Zugriff nicht zulassen der `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Die `FormsAuthenticationModule` ändert dies auf eine 302 Umleitungs-Status, senden uns auf die Anmeldeseite. Beachten Sie, dass die Seite "Wir versuchen, den Zugriff auf (`CreatingUserAccounts.aspx`) übergeben wird, auf die Anmeldeseite, über die `ReturnUrl` Querystring-Parameter.


[![Da die URL-Autorisierung Regeln nicht zulassen anonymer Zugriff werden wir auf die Anmeldeseite umgeleitet](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Abbildung 5**: Da die URL-Autorisierung nicht zulassen anonymer Zugriff Regeln, werden wir auf die Anmeldeseite umgeleitet ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image15.png))


Nach erfolgreicher Anmeldung wird, die wir zur umgeleitet werden die `CreatingUserAccounts.aspx` Seite. Dieses Mal die `UrlAuthorizationModule` ermöglicht Zugriff auf die Seite, da wir nicht mehr anonym sind.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Anwenden von URL-Autorisierungsregeln an einem bestimmten Speicherort

Die autorisierungseinstellungen, die definiert, der `<system.web>` Abschnitt `Web.config` gelten für alle Ressourcen in diesem Verzeichnis und seinen Unterverzeichnissen ASP.NET (bis andernfalls durch einen anderen außer Kraft gesetzt `Web.config` Datei). In einigen Fällen können wir jedoch alle ASP.NET-Ressourcen in ein bestimmtes Verzeichnis aus, um eine spezifische autorisierungskonfiguration mit Ausnahme von einer oder zwei spezifische Seiten zu erhalten möchten. Dies kann erreicht werden, durch das Hinzufügen einer `<location>` Element im `Web.config`, zeigen Sie ihn in die Datei, deren Autorisierungsregeln unterscheiden sich, und seine eindeutige Autorisierungsregeln darin zu definieren.

Zur Veranschaulichung der Verwendung der `<location>` Element überschreiben die Konfigurationseinstellungen für eine bestimmte Ressource, lassen Sie uns die autorisierungseinstellungen so anpassen, dass nur Tito besuchen kann `CreatingUserAccounts.aspx`. Um dies zu erreichen, fügen einen `<location>` Element der `Membership` des Ordners `Web.config` Datei und deren Markup zu aktualisieren, damit sie wie folgt aussieht:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

Die `<authorization>` Element im `<system.web>` definiert die Standard-URL-Autorisierungsregeln für ASP.NET-Ressourcen in der `Membership` Ordner und seinen Unterordnern. Die `<location>` Element kann wir diese Regeln für eine bestimmte Ressource außer Kraft setzen. Im obigen Markup der `<location>` Elementverweise der `CreatingUserAccounts.aspx` Seite, und gibt an, die Autorisierung Regeln, z. B. Tito, ermöglichen verweigern aber alle anderen Benutzer.

Um diese Änderung für die Autorisierung zu testen, starten Sie über die Website als anonymer Benutzer. Wenn Sie versuchen, eine andere Seite in finden Sie unter der `Membership` Ordner, z. B. `UserBasedAuthorization.aspx`, `UrlAuthorizationModule` die Anfrage abgelehnt werden und Sie werden zur Anmeldeseite umgeleitet werden. Nach der Anmeldung als, z. B. Scott, wechseln Sie zu einer beliebigen Seite in der `Membership` Ordner *außer* für `CreatingUserAccounts.aspx`. Es wird versucht, besuchen `CreatingUserAccounts.aspx` angemeldet als jeder andere Tito führt jedoch bei einem Versuch nicht autorisiertem Zugriff, Sie zurück zur Anmeldeseite umgeleitet.

> [!NOTE]
> Die `<location>` Element muss enthalten sein, außerhalb der Konfigurations des `<system.web>` Element. Sie müssen eine separate `<location>` -Element für jede Ressource, deren autorisierungseinstellungen, die Sie überschreiben möchten.


### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Ein Blick auf die`UrlAuthorizationModule`verwendet Sie die Autorisierungsregeln zum gewähren oder Verweigern des Zugriffs

Die `UrlAuthorizationModule` bestimmt, ob eine bestimmte Identität für eine bestimmte URL zu autorisieren, durch die Analyse der URL-Autorisierung einzeln nacheinander Regeln, vom ersten Starten und arbeiten die Möglichkeit, uns. Sobald eine Übereinstimmung gefunden wird, je nach auf, wenn die Übereinstimmung der Benutzer wird Zugriff gewährt oder verweigert, ein `<allow>` oder `<deny>` Element. <strong>Wenn keine Übereinstimmung gefunden wird, wird der Benutzer Zugriff gewährt.</strong> Daher sollten Sie den Zugriff einzuschränken, ist es zwingend erforderlich, dass Sie verwenden eine `<deny>` Element als das letzte Element in der Konfiguration der URL-Autorisierung. <strong>Wenn Sie nicht angeben einer</strong><strong>`<deny>`</strong><strong>-Element, allen Benutzern Zugriff gewährt.</strong>

Um herauszufinden, den Vorgang, mit der `UrlAuthorizationModule` um Autorität zu bestimmen, betrachten Sie das Beispiel URL-Autorisierungsregeln, die wir weiter oben in diesem Schritt betrachtet. Die erste Regel ist eine `<allow>` -Element, das Zugriff auf Tito und Scott ermöglicht. Die zweite Regeln wird ein `<deny>` -Element, das Zugriff auf alle Benutzer verweigert. Wenn ein anonymer Benutzer besucht, die `UrlAuthorizationModule` gestartet wird, werden aufgefordert, anonym ist entweder "Scott" oder "Tito? Die Antwort ist Nein, natürlich, damit für die zweite Regel setzt. Ist anonym, in der Menge der jeder? Da die Antwort "Ja", hier ist die `<deny>` Regel ist in Kraft setzen und der Besucher auf die Anmeldeseite umgeleitet wird. Auf ähnliche Weise, wenn Jisun das Unternehmen besuchen ist, die `UrlAuthorizationModule` beginnt mit dem aufzufordern, Jisun ist entweder "Scott" oder "Tito? Da sie nicht der Fall ist die `UrlAuthorizationModule` geht zur zweiten Frage: ist Jisun in der Menge der jeder? Sie ist, damit sie ebenfalls der Zugriff verweigert wird. Abschließend Wenn Tito besucht, die erste Frage gestellt durch die `UrlAuthorizationModule` ist eine positive Antwort, damit Tito Zugriff gewährt wird.

Da die `UrlAuthorizationModule` Prozesse, die die Autorisierung Regeln werden von oben nach unten, und stoppt an dem eine Übereinstimmung, es ist wichtig, damit die spezifischeren Regeln, die vor den weniger spezifischen ab stehen. D. h. um Autorisierungsregeln definieren, die verbieten Jisun und anonyme Benutzer, aber alle anderen authentifizierten Benutzern erlauben, klicken Sie zunächst die spezifischste Regel – die eine beeinträchtigen Jisun - und fahren Sie mit den weniger spezifischen Regeln – diese können alle anderen Authentifizierte Benutzer, aber alle anonymen Benutzer verweigern. Die folgenden URL-Autorisierungsregeln implementiert diese Richtlinie, indem zuerst verweigern Jisun und dann nur jeder anonymen Benutzer. Jeder authentifizierte Benutzer als Jisun wird Zugriff gewährt, da keines dieser `<deny>` Anweisungen entspricht.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Schritt 2: Beheben den Workflow für nicht autorisierte, authentifizierte Benutzer

Wie weiter oben in diesem Lernprogramm in der ein sehen Sie sich den Autorisierungsworkflow URL-Abschnitt erläutert, jederzeit eine nicht autorisierte Anforderung herausstellt, die `UrlAuthorizationModule` bricht die Anforderung ab und gibt einen HTTP 401 Unauthorized Status zurück. Dieser Status 401 geändert wird, indem die `FormsAuthenticationModule` in eine 302-Umleitungs-Status, der der Benutzer zur Anmeldeseite sendet. Dieser Workflow tritt auf, auf alle nicht autorisierten Anforderung, selbst wenn der Benutzer authentifiziert wurde.

Zurückgeben eines authentifizierten Benutzers auf die Anmeldeseite wird wahrscheinlich sie verwechseln, da sie bereits in das System angemeldet haben. Mit ein wenig Arbeit können wir diesen Workflow durch Umleiten von authentifizierten Benutzern, die nicht autorisierte Anforderungen zu einer Seite, aus der hervorgeht, dass sie versucht haben, eine zugriffsbeschränkte Seite den Zugriff auf verbessern.

Zunächst erstellen Sie eine neue ASP.NET-Seite in der Webanwendung-Stammordner, die mit dem Namen `UnauthorizedAccess.aspx`, vergessen Sie nicht, ordnen Sie diese Seite mit den `Site.master` Masterseite. Nach dem Erstellen dieser Seite, entfernen Sie das Inhaltssteuerelement, das verweist auf die `LoginContent` ContentPlaceHolder, damit die Masterseite Standardinhaltstyp wird angezeigt. Fügen Sie eine Nachricht, die erläutert, die Situation, d. h., dass der Benutzer versucht, eine geschützte Ressource zuzugreifen. Nach dem Hinzufügen einer Nachricht, die `UnauthorizedAccess.aspx` deklaratives Markup der Seite sollte etwa wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Jetzt müssen wir den Workflow zu ändern, sodass sie eine nicht autorisierte Anforderung von einem authentifizierten Benutzer durchgeführt wird, gesendet werden die `UnauthorizedAccess.aspx` Seite anstelle der Anmeldeseite. Die Logik, die nicht autorisierte Anforderungen an die Anmeldeseite umgeleitet ist verborgen, in eine private Methode von der `FormsAuthenticationModule` Klasse, damit wir dieses Verhalten nicht anpassen kann. Was wir jedoch ausführen kann, ist unsere eigene Logik zur Anmeldeseite hinzufügen, die den Benutzer umleitet `UnauthorizedAccess.aspx`, falls erforderlich.

Wenn die `FormsAuthenticationModule` leitet einen nicht autorisierte Besucher auf die Anmeldeseite, dem sie die angeforderten, nicht autorisierte-URL für die Abfragezeichenfolge durch den Namen angefügt `ReturnUrl`. Beispielsweise ein nicht autorisierter Benutzer versucht, besuchen `OnlyTito.aspx`, `FormsAuthenticationModule` würde eine Umleitung zum `Login.aspx?ReturnUrl=OnlyTito.aspx`. Aus diesem Grund, wenn die Anmeldeseite von einem authentifizierten Benutzer mit einer Abfragezeichenfolge erreicht ist, die enthält die `ReturnUrl` -Parameter, und wir wissen, dass dieser nicht authentifizierte Benutzer nur versucht, eine Seite besuchen sie nicht autorisiert ist, anzuzeigen. In diesem Fall möchten wir beim Umleiten `UnauthorizedAccess.aspx`.

Um dies zu erreichen, fügen Sie den folgenden Code auf der Anmeldeseite `Page_Load` -Ereignishandler:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

Der obige Code leitet authentifizierte, nicht autorisierte Benutzern das `UnauthorizedAccess.aspx` Seite. Um diese Logik in Aktion sehen zu können, finden Sie auf der Website als eine anonyme Besucher, und klicken Sie auf den Link Erstellen von Benutzerkonten in der linken Spalte auf. Dadurch gelangen Sie zu der `~/Membership/CreatingUserAccounts.aspx` Seite, die in Schritt 1, die wir, um nur konfiguriert Tito Zugriff erhält. Da anonyme Benutzer nicht zulässig sind, die `FormsAuthenticationModule` uns zurück zur Anmeldeseite umgeleitet.

An diesem Punkt sind wir anonym, sodass `Request.IsAuthenticated` gibt `False` und wir werden nicht zur `UnauthorizedAccess.aspx`. Stattdessen wird die Anmeldeseite angezeigt. Melden Sie sich als ein anderer Benutzer als Tito, z. B. Bruce. Geben Sie die entsprechenden Anmeldeinformationen ein, die Anmeldeseite leitet uns wieder an `~/Membership/CreatingUserAccounts.aspx`. Jedoch, da auf dieser Seite nur für Tito zugänglich ist, wir sind nicht autorisiert, um sie anzuzeigen und werden zur Anmeldeseite sofort zurückgegeben. Dieses Mal jedoch `Request.IsAuthenticated` gibt `True` (und die `ReturnUrl` Querystring-Parameter vorhanden ist), sodass wir auf umgeleitet werden die `UnauthorizedAccess.aspx` Seite.


[![Authentifiziert wird, werden nicht autorisierte Benutzer zu UnauthorizedAccess.aspx umgeleitet.](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Abbildung 6**: authentifiziert, nicht autorisierte Benutzer werden zur `UnauthorizedAccess.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image18.png))


Diese benutzerdefinierten Workflows stellt eine Benutzeroberfläche mehr sinnvolle und einfache kurzschließen der Zyklus, das in Abbildung 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Schritt 3: Funktionalität, die basierend auf dem aktuell angemeldeten Benutzer beschränkt.

URL-Autorisierung erleichtert es grob Autorisierungsregeln angeben. Wie in Schritt 1 beschrieben, können mit URL-Autorisierung wir kurz angeben, welche Identitäten zulässig sind und welche verweigert werden, eine bestimmte Seite oder alle Seiten in einem Ordner anzeigen. In bestimmten Szenarien ist jedoch, sollten wir ermöglicht allen Benutzern, eine Website besuchen, jedoch basierend auf dem Benutzer, die sie auf der Seite-Funktionen eingeschränkt.

Betrachten Sie eine e-Commerce-Website, die authentifizierte Besucher ihre Produkte überprüfen kann. Wenn ein anonymer Benutzer ein Produkt auf der Seite besucht, werden sie sehen nur die Produktinformationen und würde nicht die Möglichkeit, eine Überprüfung lassen zugewiesen werden. Ein authentifizierter Benutzer Zugriff auf die gleiche Seite würde jedoch die Überprüfung Schnittstelle angezeigt. Wenn Sie der authentifizierte Benutzer dieses Produkt noch nicht überprüft haben, ermöglichen würde die Schnittstelle zu einen Review zu übermitteln; Andernfalls würde es ihnen die zuvor übermittelter Überprüfung angezeigt. Um dieses Szenario zu einen Schritt nutzen die Produktseite von möglicherweise zusätzliche Informationen außerdem, bieten erweiterte Funktionen für Benutzer, die für das e-Commerce-Unternehmen arbeiten. Beispielsweise kann die Produktseite von Liste die Inventur auf Lager und umfassen Optionen zum Bearbeiten der Preis des Produkts und eine Beschreibung, wenn von einem Mitarbeiter aufgerufen.

Solcher Autorisierungsregeln für eine differenzierte können entweder deklarativ oder programmgesteuert (oder über eine Kombination aus beiden) implementiert werden. Im nächsten Abschnitt sehen wir, wie Sie eine differenzierte Autorisierung über das LoginView-Steuerelement zu implementieren. Danach werden wir den programmgesteuerten Methoden untersuchen. Bevor wir die Anwendung eine differenzierte Autorisierungsregeln sehen können, müssen jedoch wir eine Seite erstellen, deren Funktionalität für den Zugriff auf diese Benutzer abhängig ist.

Wir erstellen eine Seite, die die Dateien in einem bestimmten Verzeichnis in einer GridView-Ansicht enthält. Sowie das Auflisten der Dateiname, Größe und andere Informationen enthalten die GridView zwei Spalten mit LinkButtons: eine mit dem Titel anzeigen und eine mit dem Titel löschen. Wenn die Ansicht LinkButton geklickt wird, werden der Inhalt der ausgewählten Datei angezeigt werden; durch Klicken auf LinkButton gelöscht wird, wird die Datei gelöscht werden. Zunächst erstellen wir auf dieser Seite so, dass seine Funktionalität anzeigen und Löschen für alle Benutzer verfügbar ist. Die Verwendung in den Abschnitten LoginView-Steuerelement und Funktionalität programmgesteuert beschränken, die wir erfahren, wie Sie diese Features, die basierend auf dem Benutzer Zugriff auf die Seite aktivieren oder deaktivieren.

> [!NOTE]
> Die ASP.NET-Seite, die wir nun erstellen Sie verwendet ein GridView-Steuerelement, um eine Liste der Dateien anzuzeigen. Da dieses Tutorial die Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen Reihe Schwerpunkt möchte nicht ich erläutern die Innenleben des GridView-Steuerelements zu viel Zeit. In diesem Tutorial bietet eine bestimmte schrittweise Anweisungen zum Einrichten dieser Seite ist es nicht detailliert die Details, warum die Auswahl bestimmter vorgenommen wurden, oder welche Auswirkung bestimmte Eigenschaften auf der gerenderten Ausgabe verfügen. Eine gründliche Untersuchung GridView-Steuerelement, finden Sie in meinem *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Tutorial-Reihe.


Öffnen Sie zunächst die `UserBasedAuthorization.aspx` Datei die `Membership` Ordner und ein GridView-Steuerelement hinzufügen, um die Seite mit dem Namen `FilesGrid`. GridView Smarttag klicken Sie auf den Link "Spalten bearbeiten" zum Starten der Felder (Dialogfeld). Deaktivieren Sie Kontrollkästchen Felder automatisch generieren, in der unteren linken Ecke von hier aus. Fügen Sie eine auswählen-Schaltfläche eine Schaltfläche "löschen" und zwei BoundFields aus der oberen linken Ecke (die SELECT- und Delete-Schaltflächen können unter dem CommandField-Typ gefunden werden). Legen Sie der auswählen-Schaltfläche `SelectText` Eigenschaft anzeigen und die erste BoundField- `HeaderText` und `DataField` Eigenschaften, die Namen. Legen Sie die zweite BoundField- `HeaderText` Eigenschaft, um die Größe in Bytes, die `DataField` Eigenschaft, um die Länge der `DataFormatString` Eigenschaft {0:N0} und die zugehörige `HtmlEncode` Eigenschaft auf "false".

Nach dem Konfigurieren der GridView Spalten, klicken Sie auf OK, um das Dialogfeld "Felder" zu schließen. Legen Sie im Eigenschaftenfenster des GridView `DataKeyNames` Eigenschaft `FullName`. An diesem Punkt sollte den GridView deklarative Markup wie folgt aussehen:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Mit den GridView Markup erstellt sind wir bereit für den Code zu schreiben, der die Dateien in einem bestimmten Verzeichnis abrufen und diese an die GridView zu binden. Fügen Sie den folgenden Code der Seite `Page_Load` -Ereignishandler:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

Die oben aufgeführte Code verwendet die [ `DirectoryInfo` Klasse](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) zum Abrufen einer Liste der Dateien im Stammordner der Anwendung. Die [ `GetFiles()` Methode](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) aller Dateien im Verzeichnis als Bytearray zurückgegeben [ `FileInfo` Objekte](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), das dann an die GridView gebunden ist. Die `FileInfo` Objekt verfügt über eine Sammlung von Eigenschaften, z. B. `Name`, `Length`, und `IsReadOnly`, u. a. Wie Sie aus der deklarativen Markup sehen können, zeigt die GridView nur die `Name` und `Length` Eigenschaften.

> [!NOTE]
> Die `DirectoryInfo` und `FileInfo` Klassen befinden sich die [ `System.IO` Namespace](https://msdn.microsoft.com/library/system.io.aspx). Aus diesem Grund müssen Sie entweder diese Klassennamen mit dem Namespacenamen voranzustellen oder den Namespace in der Klassendatei importiert werden (über `Imports System.IO`).


Nehmen Sie einen Moment Zeit, um diese Seite über einen Browser besuchen. Es zeigt die Liste der Dateien, die sich im Stammverzeichnis der Anwendung befinden. Klicken Sie auf die Ansicht oder LinkButtons löschen führt dazu, dass einen Postback, aber keine Aktion tritt auf, da wir noch um haben die erforderlichen Ereignishandler zu erstellen.


[![Das GridView Listet die Dateien im Stammverzeichnis der Webanwendung](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Abbildung 7**: GridView Listet die Dateien im Stammverzeichnis der Webanwendung ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image21.png))


Wir benötigen eine Möglichkeit, um den Inhalt der ausgewählten Datei anzuzeigen. Zurück zu Visual Studio, und fügen Sie ein Textfeld mit dem Namen `FileContents` über GridView. Legen Sie seine `TextMode` Eigenschaft `MultiLine` und die zugehörige `Columns` und `Rows` Eigenschaften zu 95 % und 10, bzw.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für der GridView [ `SelectedIndexChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Dieser Code verwendet der GridView `SelectedValue` Eigenschaft, um zu bestimmen, der vollständige Dateiname der ausgewählten Datei. Intern die `DataKeys` Auflistung verwiesen wird, um zu ermitteln die `SelectedValue`, daher ist es zwingend erforderlich, dass Sie festlegen, dass der GridView `DataKeyNames` Eigenschaft, um den Namen, wie weiter oben in diesem Schritt beschrieben. Die [ `File` Klasse](https://msdn.microsoft.com/library/system.io.file.aspx) wird verwendet, um den Inhalt der ausgewählten Datei in eine Zeichenfolge zu lesen, klicken Sie dann das zugewiesen wird die `FileContents` Textfeldss `Text` -Eigenschaft, und zeigt den Inhalt der ausgewählten Datei auf der Seite.


[![Aktiviert den Inhalt der Datei werden in das Textfeld angezeigt.](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Abbildung 8**: die ausgewählte Datei Inhalt wird angezeigt, in das Textfeld ein ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image24.png))


> [!NOTE]
> Wenn Sie zeigen Sie den Inhalt einer Datei, die HTML-Markup enthält, und klicken Sie dann zum Anzeigen oder Löschen einer Datei versucht, erhalten Sie eine `HttpRequestValidationException` Fehler. Dies tritt auf, da beim Postback Textfeldss Inhalt zurück an den Webserver gesendet werden. Löst standardmäßig ASP.NET eine `HttpRequestValidationException` Fehler, wenn potenziell gefährliche Inhalte postback, z. B. HTML-Markup erkannt wird. Um diesen Fehler zu deaktivieren, deaktivieren Sie die Anforderungsvalidierung für die Seite durch Hinzufügen von `ValidateRequest="false"` auf die `@Page` Richtlinie. Weitere Informationen zu den Vorteilen der Anforderungsvalidierung als sowie welche Vorsichtsmaßnahmen Sie beim Ausführen sollten, lesen Sie deaktivieren, [Anforderungsvalidierung - Skript-Angriffe verhindern](https://asp.net/learn/whitepapers/request-validation/).


Fügen Sie abschließend einen Ereignishandler mit dem folgenden Code hinzu, für des GridView [ `RowDeleting` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx):

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

Der Code zeigt einfach den vollständigen Namen der Datei, löschen Sie in der `FileContents` Textfeld *ohne* tatsächlich die Datei zu löschen.


[![Klicken Sie auf die Schaltfläche "löschen" werden nicht tatsächlich die Datei gelöscht](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Abbildung 9**: auf die Datei das Löschen von Schaltfläche werden nicht tatsächlich gelöscht ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image27.png))


In Schritt 1, die wir konfiguriert der URL-Autorisierungsregeln, um zu verhindern, dass anonyme Benutzer anzeigen von Seiten in der `Membership` Ordner. Um eine differenzierte Authentifizierung besser zu weisen, lassen Sie anonyme Benutzer zum Besuch der `UserBasedAuthorization.aspx` Seite jedoch mit eingeschränkter Funktionalität. Fügen Sie zum Öffnen dieser Seite Sie von allen Benutzern Zugriff auf die folgenden `<location>` Element, das `Web.config` Datei die `Membership` Ordner:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Nach dem Hinzufügen dieser `<location>` -Element, die neuen URL-Autorisierungsregeln zu testen, indem Sie Abmelden des Standorts. Als anonymer Benutzer Sie sollten die Berechtigung erhalten, besuchen die `UserBasedAuthorization.aspx` Seite.

Derzeit kann alle authentifizierten oder anonymen Benutzer besuchen die `UserBasedAuthorization.aspx` und anzuzeigen, oder Löschen von Dateien. Lassen Sie es so, dass nur authentifizierte Benutzer können den Inhalt einer Datei anzeigen, und nur Tito kann eine Datei löschen. Solcher Autorisierungsregeln für eine differenzierte können deklarativ, programmgesteuert oder über eine Kombination beider Methoden angewendet werden. Lassen Sie uns nicht der deklarative Ansatz mit beschränken, die den Inhalt einer Datei anzeigen können. Wir verwenden den programmgesteuerten Ansatz zum Grenzwert, die eine Datei löschen können.

### <a name="using-the-loginview-control"></a>Verwenden das LoginView-Steuerelement

Wie wir in den letzten Tutorials gesehen haben, wird das LoginView-Steuerelement eignet sich für verschiedene Schnittstellen für authentifizierte und anonyme Benutzer anzeigt, und bietet eine einfache Möglichkeit, Funktionalität auszublenden, die nicht für anonyme Benutzer zugänglich ist. Da anonyme Benutzer können nicht anzeigen oder Löschen von Dateien, wir brauchen nur zum Anzeigen der `FileContents` Textfeld aus, wenn die Seite von einem authentifizierten Benutzer aufgerufen wird. Um dies zu erreichen, fügen Sie auf der Seite ein LoginView-Steuerelement hinzu, nennen Sie es `LoginViewForFileContentsTextBox`, und verschieben Sie die `FileContents` deklaratives Markup des Textfelds, in des LoginView-Steuerelements `LoggedInTemplate`.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Die Web-Steuerelemente in das LoginView Vorlagen sind nicht mehr direkt über den Code-Behind-Klasse zugegriffen werden. Z. B. die `FilesGrid` GridView `SelectedIndexChanged` und `RowDeleting` Ereignishandler derzeit verweisen die `FileContents` wie der TextBox-Steuerelement mit Code:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

Dieser Code ist jedoch nicht mehr gültig. Durch das Verschieben der `FileContents` Textfeld in die `LoggedInTemplate` Textfeld kann nicht direkt zugegriffen werden. Wir müssen stattdessen die `FindControl("controlId")` Methode, um das Steuerelement programmgesteuert zu verweisen. Update der `FilesGrid` Ereignishandlern auf das Textfeld verweisen wie folgt:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Nachdem Sie das Textfeld auf das LoginView verschoben `LoggedInTemplate` und aktualisieren den Code der Seite zu verweisen, die das Textfeld mit der `FindControl("controlId")` Muster, besuchen Sie die Seite als anonymer Benutzer. Wie in Abbildung 10 gezeigt, die `FileContents` Textfeld wird nicht angezeigt. Die Ansicht LinkButton wird jedoch weiterhin angezeigt.


[![Das LoginView-Steuerelement gerendert wird nur das Textfeld FileContents für authentifizierte Benutzer](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Abbildung 10**: das LoginView-Steuerelement nur rendert die `FileContents` TextBox für authentifizierte Benutzer ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image30.png))


Eine Möglichkeit zum Ausblenden der Schaltfläche "Ansicht" für anonyme Benutzer ist das Feld "GridView" in ein TemplateField konvertieren. Dadurch wird eine Vorlage generiert, die deklarative Markup für die Ansicht LinkButton enthält. Wir können dann das TemplateField ein LoginView-Steuerelement hinzu, und platzieren Sie LinkButton innerhalb der LoginView `LoggedInTemplate`können. Dadurch wird die Schaltfläche für die anonyme Besucher von ausblenden. Um dies zu erreichen, klicken Sie auf den Link "Spalten bearbeiten" aus des GridView Smarttag, starten Sie die Felder (Dialogfeld). Als nächstes wählen Sie die auswählen-Schaltfläche aus der Liste in der unteren linken Ecke, und klicken Sie dann auf die dieses Feld auf einen Link TemplateField konvertieren. Auf diese Weise wird die Feld deklaratives Markup aus ändern:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 Nach: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 An diesem Punkt können wir das TemplateField ein LoginView hinzufügen. Das folgende Markup zeigt die Ansicht LinkButton nur für authentifizierte Benutzer. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Wie in Abbildung 11 dargestellt, ist das Endergebnis nicht, dass ziemlich als Ansicht Spalte weiterhin angezeigt wird, obwohl die LinkButtons Ansicht innerhalb der Spalte ausgeblendet sind. Wir werden die gesamte GridView-Spalte (und nicht nur LinkButton) ausblenden im nächsten Abschnitt behandelt.


[![Das LoginView-Steuerelement blendet Sie aus der Sicht LinkButtons für anonyme Besucher](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Abbildung 11**: das LoginView-Steuerelement blendet Sie aus der Sicht LinkButtons für anonyme Besucher ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image33.png))


### <a name="programmatically-limiting-functionality"></a>Funktionalität programmgesteuert beschränkt.

In einigen Fällen sind deklarative Verfahren zum Einschränken von Funktionen zu einer Seite nicht aus. Beispielsweise kann die Verfügbarkeit von bestimmten Funktionen Seite abhängig sein Kriterien nach, ob der Benutzer Zugriff auf die Seite anonym oder authentifiziert ist. In solchen Fällen können die verschiedenen Elemente der Benutzeroberfläche angezeigt oder ausgeblendet werden, auf programmgesteuerte Weise.

Um die Funktionalität programmgesteuert zu beschränken, müssen wir zwei Tasks ausführen:

1. Bestimmen, ob der Benutzer, die auf der Seite auf die Funktionalität, zugreifen kann und
2. Programmgesteuert ändern Sie die Benutzeroberfläche, die basierend auf, ob der Benutzer Zugriff auf die betreffenden Funktionen verfügt.

Um die Anwendung diese beiden Aufgaben zu demonstrieren, lassen Sie uns nur die Tito Löschen von Dateien aus der GridView erlauben. Unsere erste Aufgabe ist dann zu bestimmen, ob es Tito, die auf der Seite ist. Nachdem die, die ermittelt wurde, müssen (oder ausblenden) der GridView löschen-Spalte. GridView Spalten können Sie über die `Columns` Eigenschaft ist nur eine Spalte gerendert, wenn die `Visible` -Eigenschaftensatz auf `True` (Standard).

Fügen Sie den folgenden Code der `Page_Load` Ereignishandler vor dem Binden der Daten an die GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Wie in erläutert die [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Tutorial `User.Identity.Name` gibt die Identität des Namen zurück. Dies entspricht den Benutzernamen in das Steuerelement für die Anmeldung eingegeben haben. Ist dies Tito Besuch der Seite, die GridView, die zweite Spalte `Visible` -Eigenschaftensatz auf `True`ist, andernfalls wird festgelegt `False`. Das Ergebnis ist, dass wenn Personen als Tito Besuchen auf der Seite, die entweder einem anderen authentifizierten Benutzer oder ein anonymer Benutzer die Löschen-Spalte nicht (siehe Abbildung 12); gerendert wird Wenn allerdings Tito auf der Seite angezeigt, die Spalte löschen ist vorhanden (siehe Abbildung 13).


[![Die Spalte löschen, wird nicht gerendert beim besucht durch eine Person außer Tito (z. B. Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Abbildung 12**: Löschen von Sloupec je Elementem nicht gerendert beim besucht durch eine Person außer Tito (z. B. Bruce) ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image36.png))


[![Die Spalte löschen wird für Tito gerendert.](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Abbildung 13**: die Löschen-Spalte für Tito gerendert wird ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image39.png))


## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Schritt 4: Anwenden von Autorisierungsregeln auf Klassen und Methoden

In Schritt 3 haben wir nicht zulässig für anonyme Benutzer anzeigen des Inhalts einer Datei und Löschen von Dateien auf alle Benutzer außer Tito untersagt. Zu diesem Zweck die zugehörigen Elemente der Benutzeroberfläche für nicht autorisierte Besucher über deklarativ und programmatisch Techniken ausblenden. In unserem einfachen Beispiel wurde durch das die Elemente der Benutzeroberfläche ordnungsgemäß ausblenden, einfache, aber was komplexere Sites, möglicherweise gibt es viele verschiedene Möglichkeiten, die die gleichen Funktionen ausführen? In dieser Funktionalität für nicht autorisierte Benutzer beschränkt, was geschieht, wenn wir es ausblenden oder deaktivieren alle Elemente der entsprechenden Benutzeroberfläche vergessen?

Eine einfache Möglichkeit, um sicherzustellen, dass für ein bestimmter Teil der Funktionalität von unbefugten Benutzern nicht zugegriffen werden kann, ergänzen Sie diese Klasse oder Methode mit ist der [ `PrincipalPermission` Attribut](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Wenn die .NET Runtime eine Klasse verwendet oder eine seiner Methoden ausführt, überprüft stellen Sie sicher, dass der aktuelle Sicherheitskontext verfügt über die Berechtigung zum Verwenden der Klasse, oder führen Sie die Methode. Die `PrincipalPermission` -Attribut stellt einen Mechanismus, über die wir können diese Regeln definieren.

Wir veranschaulichen die Verwendung der `PrincipalPermission` Attribut in des GridView `SelectedIndexChanged` und `RowDeleting` -Ereignishandler, um die Ausführung durch anonyme Benutzer und Benutzer als Tito, bzw. nicht zulassen. Alles, was erforderlich ist, wird das entsprechende Attribut auf jede Funktionsdefinition hinzufügen:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

Das Attribut für die `SelectedIndexChanged` Event Handler gibt an, die nur Benutzer authentifizierte können Ereignishandler ausführen, wobei als Attribut auf die `RowDeleting` Ereignishandler beschränkt die Ausführung des Tito.

> [!NOTE]
> Ein Attribut kann auf eine Klasse, Methode, Eigenschaft oder Ereignis angewendet werden. Wenn Sie ein Attribut hinzufügen zu können, muss es Teil der Klasse, Methode, Eigenschaft oder Ereignis-deklarationsanweisung in Verbindung. Da Visual Basic Zeilenumbrüche als Trennzeichen für die Anweisung verwendet wird, müssen Attribute in der gleichen Zeile wie die Deklaration oder direkt darüber liegenden mit Zeilenfortsetzungszeichen (Unterstrich) angezeigt werden. Im obigen Codeausschnitt wird das Zeilenfortsetzungszeichen verwendet, um das Attribut auf eine Zeile und die Deklaration der Methode auf einem anderen platzieren.


Wenn aus irgendeinem Grund, ein anderen Benutzer als Tito auszuführen versucht der `RowDeleting` -Ereignishandler oder eine nicht authentifizierte Benutzer versucht wird, führen Sie die `SelectedIndexChanged` Ereignishandler, die .NET Runtime löst eine `SecurityException`.


[![Wenn der Sicherheitskontext zum Ausführen der Methode nicht autorisiert ist, wird eine SecurityException ausgelöst.](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Abbildung 14**: Wenn der Sicherheitskontext nicht, zum Ausführen der Methode autorisiert ist, eine `SecurityException` ausgelöst ([klicken Sie, um das Bild in voller Größe anzeigen](user-based-authorization-vb/_static/image42.png))


> [!NOTE]
> Ergänzen Sie damit um mehrere Sicherheitskontexte auf eine Klasse oder Methode zugreifen zu können, die Klasse oder Methode mit einem `PrincipalPermission` -Attribut für jede Sicherheitskontext. D. h. sowohl Tito und Bruce ausführen können die `RowDeleting` -Ereignishandler hinzufügen *zwei* `PrincipalPermission` Attribute:


[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

Zusätzlich zu ASP.NET-Seiten haben viele Anwendungen auch eine Architektur mit verschiedenen Ebenen, wie z. B. von Geschäftslogik und Datenzugriffsschichten. Diese Ebenen werden in der Regel als Klassenbibliotheken implementiert und bieten Klassen und Methoden zum Ausführen von Geschäftsfunktionen Logik und die Daten beziehen. Die `PrincipalPermission` Attribut ist nützlich zum Anwenden von Autorisierungsregeln auf diese Ebenen.

Weitere Informationen zur Verwendung der `PrincipalPermission` Attribut zum Definieren von Autorisierungsregeln auf Klassen und Methoden finden [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [Autorisierungsregeln hinzufügen, Business und Daten Schichten mithilfe `PrincipalPermissionAttributes` ](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial erläutert, wie Benutzer-basierten Autorisierungsregeln angewendet. Da fingen wir einen Blick auf ASP. NET URL-Autorisierungsframework. Für jede Anforderung, die ASP.NET-Engine die `UrlAuthorizationModule` untersucht die URL-Autorisierungsregeln, die in der Konfigurationsdatei der Anwendung zu bestimmen, ob diese Identität autorisiert ist, Zugriff auf die angeforderte Ressource definiert. Kurz gesagt, vereinfacht URL-Autorisierung Autorisierungsregeln für eine bestimmte Seite oder für alle Seiten in einem bestimmten Verzeichnis angeben.

Das URL-Autorisierungsframework gilt Autorisierungsregeln pro Seite von Seite. Mit URL-Autorisierung ist entweder die anfordernde Identität Zugriff auf eine bestimmte Ressource oder nicht autorisiert. Viele Szenarien, rufen Sie jedoch für weitere eine differenzierte-Autorisierungsregeln. Anstatt zu definieren, die zulässig ist, auf eine Seite zuzugreifen, müssen wir alle Benutzer eine Seite zuzugreifen, können jedoch verschiedene Daten anzeigen oder bieten unterschiedliche Funktionen abhängig vom Benutzer auf der Seite. Auf Seitenebene Autorisierung umfasst in der Regel bestimmte Elemente der Benutzeroberfläche ausblenden, um zu verhindern, dass nicht autorisierte Benutzer Zugriff auf unzulässige Funktion. Darüber hinaus ist es möglich, die Attribute verwenden, um den Zugriff auf Klassen und die Ausführung der Methoden für bestimmte Benutzer beschränken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Hinzufügen von Autorisierungsregeln auf Geschäfts- und Datenebenen verwenden `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Änderungen zwischen IIS 6 und IIS 7-Sicherheit](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Konfigurieren bestimmte Dateien und Unterverzeichnisse](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Benutzerabhängiges basierend auf dem Benutzer](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [LoginView-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Grundlegendes zu IIS7-URL-Autorisierung](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` Technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](validating-user-credentials-against-the-membership-user-store-vb.md)
> [Weiter](storing-additional-user-information-vb.md)
