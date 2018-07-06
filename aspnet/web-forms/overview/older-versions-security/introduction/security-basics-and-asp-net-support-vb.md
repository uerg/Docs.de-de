---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
title: Grundlagen der Sicherheit und Unterstützung für ASP.NET (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Dies ist im ersten Tutorial einer Reihe von Tutorials, in denen Verfahren zum Authentifizieren der Besucher über ein Webformular Autorisieren des Zugriffs auf partic untersucht wird...
ms.author: aspnetcontent
ms.date: 01/13/2008
ms.assetid: ab68a92b-fc81-40a4-a7dc-406625d2c5d4
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-vb
msc.type: authoredcontent
ms.openlocfilehash: ebd4e52720fc36bfcf86b7ef4205afcca7e2bc4a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820875"
---
<a name="security-basics-and-aspnet-support-vb"></a>Grundlagen der Sicherheit und Unterstützung für ASP.NET (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_vb.pdf)

> Dies ist im ersten Tutorial einer Reihe von Tutorials, die Techniken für die Besucher über ein Webformular authentifizieren, Autorisieren des Zugriffs auf bestimmte Seiten und Funktionen und Verwalten von Benutzerkonten in einer ASP.NET-Anwendung vorgestellt wird.


## <a name="introduction"></a>Einführung

Was ist die eins-Foren, e-Commerce-Websites, online-e-Mail-Websites, Portalwebsites und sozialen Netzwerk-Standorten, die alle gemeinsam haben? Alle angebotenen *Benutzerkonten*. Sites, die Benutzerkonten zu bieten, müssen eine Reihe von Diensten bereitstellen. Neue Besucher müssen Sie ein Konto erstellen können, und zurückgeben Besucher müssen in der Lage, melden Sie sich, mindestens. Solche Webanwendungen können Entscheidungen basierend auf dem angemeldeten Benutzer: Einige Seiten oder Aktionen können eingeschränkt werden, nur Benutzer oder eine bestimmte Teilmenge von Benutzern protokolliert andere Seiten möglicherweise Informationen bestimmte für den angemeldeten Benutzer oder anzeigen können, werden mehr oder weniger Informationen angezeigt, je nachdem, welche Benutzer auf die Seite angezeigt wird.

Dies ist im ersten Tutorial einer Reihe von Tutorials, die Techniken für die Besucher über ein Webformular authentifizieren, Autorisieren des Zugriffs auf bestimmte Seiten und Funktionen und Verwalten von Benutzerkonten in einer ASP.NET-Anwendung vorgestellt wird. Im Verlauf des in diesen Tutorials wird untersucht, wie Sie:

- Identifizieren und melden Sie sich Benutzer in einer Websites
- Verwenden Sie ASP. NET mitgliedschaftsframework zum Verwalten von Benutzerkonten
- Erstellen, aktualisieren und Löschen von Benutzerkonten
- Beschränken des Zugriffs auf eine Webseite, Verzeichnis oder bestimmte Funktionen, die basierend auf dem angemeldeten Benutzer
- Verwenden Sie ASP. NET Rollen Framework Rollen Benutzerkonten zugeordnet werden soll
- Verwalten von Benutzerrollen
- Beschränken des Zugriffs auf eine Webseite, Verzeichnis oder bestimmte Funktionen, die basierend auf Rolle des angemeldeten Benutzers
- Anpassen und Erweitern von ASP. NET Security-Websteuerelemente

Diese Lernprogramme sind darauf ausgerichtet, präzise und enthalten schrittweise Anleitungen mit zahlreichen Screenshots, die Sie visuell durch den Prozess geführt. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und umfasst ein Download, der den vollständigen Code verwendet. (In diesem ersten Tutorial konzentriert sich auf Schlüsselbegriffe der Sicherheit von einem Standpunkt auf hoher Ebene und enthält daher keine zugehörigen Code.)

In diesem Tutorial wird erörtert, wichtige Sicherheitskonzepte und welche Funktionen in ASP.NET zur Unterstützung bei der Implementierung der Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen verfügbar sind. Fangen wir an!

> [!NOTE]
> Sicherheit ist ein wichtiger Aspekt jeder Anwendung, die umfasst physischer, technologischer und Richtlinien Entscheidungen zu treffen und erfordert ein hohes Maß an Planung und Domänenkenntnisse. Diese tutorialreihe ist nicht als Leitfaden für die Entwicklung von sicheren Webanwendungen vorgesehen. Stattdessen konzentriert Sie sich speziell auf das Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen. Während einige Schlüsselbegriffe der Sicherheit drehen, um diese Probleme in dieser Serie behandelt werden, bleiben andere nicht untersuchten.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Authentifizierung, Autorisierung, Benutzerkonten und Rollen

Authentifizierung, Autorisierung, Benutzerkonten und Rollen werden vier Bedingungen, die sehr häufig in dieser tutorialreihe verwendet werden, daher möchte ich schnell diese Begriffe im Kontext des websicherheit definieren werden. In einem Client / Server-Modell, z. B. das Internet gibt es viele Szenarien, in denen der Server den Client die Anforderung zu identifizieren muss. *Authentifizierung* ist der Feststellung der Identität des Clients. Ein Client, der erfolgreich identifiziert wurde gilt als *authentifiziert*. Eine unbekannte Clients gilt als *nicht authentifizierte* oder *anonyme*.

Sichere Authentifizierungssysteme umfassen mindestens eine der folgenden drei Facets: [etwas, das Sie wissen, haben etwas oder etwas Sie](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Die meisten Webanwendungen beruhen auf ein beliebiges Objekt sein, die der Client bekannt, z. B. ein Kennwort oder PIN sind. Die Informationen, die zum Identifizieren eines Benutzers – ihr Benutzername und Kennwort ein, z. B. – verwendet werden als bezeichnet *Anmeldeinformationen*. Im Mittelpunkt dieser tutorialreihe *Formularauthentifizierung*, d.h. ein Authentifizierungsmodell, in denen Benutzer melden Sie sich an den Standort mit ihren Anmeldeinformationen in einem Formular Webseite. Wir haben alle ist, diese Art der Authentifizierung vor. Rufen Sie alle e-Commerce-Website. Wenn Sie bereit sind, sehen Sie sich werden Sie aufgefordert, für die Anmeldung durch Eingabe Ihres Benutzernamens und Kennworts in die Textfelder auf einer Webseite.

Zusätzlich zur Identifizierung von Clients, kann ein Server einschränken, welche Ressourcen oder Funktionen zugegriffen werden kann, abhängig von der Client die Anforderung werden müssen. *Autorisierung* ist der Prozess des bestimmens, ob ein bestimmter Benutzer die Autorität für die Zugriff auf eine bestimmte Ressource oder die Funktionalität verfügt.

Ein *Benutzerkonto* ist ein Speicher für persistente Daten zu einem bestimmten Benutzer. Benutzerkonten müssen minimal Informationen enthalten, die der Benutzer, z. B. Anmeldung und dieses Kennwort des Benutzers eindeutig identifiziert. Zusammen mit dieser wichtige Informationen, können Benutzerkonten Dinge wie enthalten:-e-Mail-Adresse des Benutzers; das Datum und Uhrzeit der Erstellung des Kontos; das Datum und die Uhrzeit, bei denen sie zuletzt angemeldet; Name der ersten und letzten; Telefonnummer; und e-Mail-Adresse. Bei Verwendung der Formularauthentifizierung wird die Benutzerkontoinformationen in der Regel in einer relationalen Datenbank wie Microsoft SQL Server gespeichert.

Webanwendungen mit Unterstützung für Benutzerkonten können optional Gruppieren von Benutzern in *Rollen*. Eine Rolle ist einfach eine Bezeichnung, die für einen Benutzer angewendet wird, und stellt eine Abstraktion zum Definieren von Autorisierungsregeln und Seitenebene Funktionalität bereit. Eine Website kann z. B. eine Administratorrolle mit Autorisierungsregeln umfassen, die jeder Benutzer jedoch ein Administrator auf einen bestimmten Satz von Webseiten zu verhindern. Darüber hinaus kann eine Reihe von Seiten, die für alle Benutzer (einschließlich nicht-Administratoren) zugänglich sind zusätzliche Daten anzeigen oder bieten zusätzliche Funktionalität, wenn Benutzer in der Rolle "Administratoren" aufgerufen. Bei Verwendung von Rollen können wir diese Autorisierungsregeln für eine Rolle von gruppenrollen anstelle von Benutzer-an-Benutzer definieren.

## <a name="authenticating-users-in-an-aspnet-application"></a>Authentifizieren von Benutzern in einer ASP.NET-Anwendung

Wenn ein Benutzer eine URL in das Adressfeld des Browsers oder -Klicks auf einen Link eingibt, sendet der Browser eine [Hypertext Transfer Protocol (HTTP)](http://en.wikipedia.org/wiki/HTTP) an den Webserver für den angegebenen Inhalt anfordern, z. B. auf eine ASP.NET Seite, ein Bild, ein JavaScript Datei oder einen anderen Typ von Inhalt. Der Webserver wird damit beauftragt, Zurückgeben des angeforderten Inhalts. In diesem Fall müssen sie bestimmen, eine Reihe von Informationen über die Anforderung, einschließlich, die die Anforderung gesendet und gibt an, ob diese Identität zum Abrufen des angeforderten Inhalts autorisiert ist.

Standardmäßig senden Browser HTTP-Anforderungen, die jede Art von ID-Informationen fehlt. Aber wenn der Browser enthält Informationen für die Authentifizierung des Servers startet dann den Workflow der Authentifizierung, die versucht, den Client, der die Anforderung zu identifizieren. Die Schritte des Workflows Authentifizierung richten sich nach den Typ der Authentifizierung, die von der Webanwendung verwendet wird. ASP.NET unterstützt drei Arten der Authentifizierung: Windows, Passport- und Forms. Diese tutorialreihe konzentriert sich auf die Formularauthentifizierung, aber lassen Sie uns eine Minute dauern zu vergleichen und gegenüberstellen Benutzerspeichern für Windows-Authentifizierung und Workflow.

### <a name="authentication-via-windows-authentication"></a>Authentifizierung über die Windows-Authentifizierung

Workflow der Windows-Authentifizierung verwendet einen der folgenden Authentifizierungsmethoden:

- Standardauthentifizierung.
- Digestauthentifizierung
- Integrierte Windows-Authentifizierung

Alle drei Techniken funktionieren auf ungefähr die gleiche Weise: Wenn eine nicht autorisierte, anonyme Anforderung eingeht, der Webserver sendet eine HTTP-Antwort, der angibt, Autorisierung ist erforderlich, um den Vorgang fortzusetzen. Der Browser zeigt ein modales Dialogfeld an, das der Benutzer Benutzername und Kennwort (siehe Abbildung 1) aufgefordert. Diese Information wird dann zurück an den Webserver über einen HTTP-Header gesendet.


![Ein modales Dialogfeld fordert den Benutzer nach seinen Anmeldeinformationen](security-basics-and-asp-net-support-vb/_static/image1.png)

**Abbildung 1**: ein modales Dialogfeld fordert den Benutzer nach seinen Anmeldeinformationen


Die angegebenen Anmeldeinformationen werden für der Webservers Windows-Benutzer-Store überprüft. Dies bedeutet, dass jeder authentifizierte Benutzer in Ihrer Webanwendung ein Windows-Konto in Ihrer Organisation benötigen. Dies ist üblich, die im Intranetszenarien. Stellt bei Verwendung von integrierten Windows-Authentifizierung in einem Intranet-Einstellung der Browser automatisch den Webserver mit den Anmeldeinformationen zur Anmeldung mit dem Netzwerk und Unterdrücken von dem in Abbildung 1 dargestellten Dialogfeld. Während der Windows-Authentifizierung für Intranetanwendungen ist, ist es normalerweise unmöglich von Internetanwendungen, da Sie nicht möchten, um Windows-Konten für jeden Benutzer zu erstellen, die sich an Ihrem Standort registriert.

### <a name="authentication-via-forms-authentication"></a>Authentifizierung über die Formularauthentifizierung

Formularauthentifizierung ist auf der anderen Seite ideal für Internet-Webanwendungen. Denken Sie daran, dass die, dass die Formularauthentifizierung den Benutzer identifiziert, indem Sie aufgefordert werden, ihre Anmeldeinformationen über ein Webformular eingeben. Daher, wenn ein Benutzer versucht, eine nicht autorisierte Ressource zuzugreifen, wird er automatisch zur Anmeldeseite umgeleitet, wo sie ihre Anmeldeinformationen eingeben können. Die übermittelten Anmeldeinformationen werden dann anhand eines benutzerdefinierten Benutzerspeichers – in der Regel in einer Datenbank überprüft.

Nachdem Sie überprüft haben, damit die übermittelten Anmeldeinformationen eine *forms-Authentifizierungsticket* wird für den Benutzer erstellt. Dieses Ticket gibt an, dass der Benutzer wurde authentifiziert, und Identifizieren von Informationen, z. B. den Benutzernamen enthält. Das Formularauthentifizierungsticket wird (normalerweise) als ein Cookie auf dem Clientcomputer gespeichert. Deshalb werden in nachfolgende Besuchen der Website das Formularauthentifizierungsticket in der HTTP-Anforderung, wodurch die Webanwendung zur Identifizierung des Benutzers, nachdem sie sich angemeldet haben.

Abbildung 2 zeigt den Workflow der Forms-Authentifizierung über einen Aussichtspunkt auf hoher Ebene. Beachten Sie, wie die Teile für Authentifizierung und Autorisierung in ASP.NET als zwei separate Entitäten fungieren. Das Authentifizierungssystem Forms identifiziert den Benutzer (oder gibt an, dass sie anonym sind). Das Autorisierungssystem wird bestimmt, ob der Benutzer den Zugriff auf die angeforderte Ressource. Wenn der Benutzer ist nicht autorisiert, (wie sie beim Versuch, anonym ProtectedPage.aspx finden in Abbildung 2 sind), das Autorisierungssystem des Benutzers abgelehnt wird, verursacht die Formulare Authentifizierungssystem, um den Benutzer automatisch zur Anmeldeseite umzuleiten, meldet.

Sobald der Benutzer erfolgreich angemeldet hat, sind nachfolgende HTTP-Anforderungen das Formularauthentifizierungsticket. Das Authentifizierungssystem Forms identifiziert nur den Benutzer – es ist das Autorisierungssystem, das bestimmt, ob der Benutzer auf die angeforderte Ressource zugreifen kann.


![Workflow der Forms-Authentifizierung](security-basics-and-asp-net-support-vb/_static/image2.png)

**Abbildung 2**: Workflow der Forms-Authentifizierung


Wir werden untersuchen Formularauthentifizierung viel ausführlicher in den nächsten beiden Tutorials[eine Übersicht der Formularauthentifizierung](an-overview-of-forms-authentication-vb.md) und [Konfiguration der Formularauthentifizierung und Weiterführende Themen](forms-authentication-configuration-and-advanced-topics-vb.md). Weitere Informationen zu ASP. NET Authentifizierungsoptionen, finden Sie unter [ASP.NET-Authentifizierung](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Beschränken des Zugriffs auf Webseiten, Verzeichnisse und die Funktionen der Seite

ASP.NET umfasst zwei Möglichkeiten, um zu bestimmen, ob ein bestimmter Benutzer Autorität für die auf eine bestimmte Datei oder ein Verzeichnis wurde:

- **Datei Autorisierung** – da ASP.NET-Seiten und -Webdienste implementiert werden, wie Dateien, die im Dateisystem des Webservers den Zugriff auf diese Dateien befinden, die über Zugriffssteuerungslisten (ACLs) angegeben werden können. Dateiautorisierung wird am häufigsten mit der Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formularauthentifizierung verwenden, werden alle Anforderungen der Betriebssystem - und -Datei auf Systemebene von dem gleichen Windows-Konto, unabhängig von der Website für den Benutzer ausgeführt.
- **URL-Autorisierung**-mit URL-Autorisierung, gibt der Seitenentwickler Autorisierungsregeln in "Web.config". Diese Autorisierungsregeln Geben Sie was Benutzer oder Rollen zugreifen dürfen oder den Zugriff auf bestimmte Seiten oder die Verzeichnisse in der Anwendung verweigert werden.

Dateiautorisierung und URL-Autorisierung definieren Autorisierungsregeln für den Zugriff auf eine bestimmte ASP.NET-Seite oder für alle ASP.NET-Seiten in einem bestimmten Verzeichnis. Mit diesen Techniken können wir ASP.NET anzuweisen, Anforderungen für eine bestimmte Seite für einen bestimmten Benutzer ablehnen oder Zulassen des Zugriffs auf eine Gruppe von Benutzern und Zugriff auf alle anderen Benutzer verweigern. Wie sieht es Szenarien, in dem alle Benutzer können auf die Seite zuzugreifen, aber der Seite Funktionen, die vom Benutzer abhängig? Beispielsweise haben viele Standorte, die die Benutzerkonten unterstützen Seiten, die unterschiedliche Inhalte oder Daten für authentifizierte Benutzer im Vergleich zu anonymen Benutzern anzeigen. Ein anonymer Benutzer kann ein Link angezeigt, auf die Website, melden Sie sich während ein authentifiziertes Benutzers stattdessen diese Meldung angezeigt würde, Willkommen zurück, wie *Benutzername* zusammen mit einem Link zum Abmelden. Ein weiteres Beispiel: beim Anzeigen eines Elements auf einer Auktionswebsite finden Sie verschiedene Informationen, je nachdem, ob Sie einen Bieter oder der das Element auctioning befinden.

Solche Anpassungen auf Seitenebene können deklarativ oder programmgesteuert erfolgen. Zum Anzeigen anderer Inhalte für anonyme als authentifizierte Benutzer einfach ziehen Sie eine [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) auf der Seite, und geben Sie den entsprechenden Inhalt in die AnonymousTemplate und LoggedInTemplate Vorlagen. Alternativ Sie können programmgesteuert zu ermitteln, ob die aktuelle Anforderung authentifiziert wird, die der Benutzer ist und welche Rollen sie gehören zum (sofern vorhanden). Sie können diese Informationen verwenden, um dann ein- oder Ausblenden von Spalten in einem Raster oder Bereiche auf der Seite.

Diese Reihe enthält drei Lernprogramme, die sich auf der Autorisierung zu konzentrieren. ***Benutzerbasierte Autorisierung***untersucht, wie Sie den Zugriff auf eine Seite oder Seiten in ein Verzeichnis für bestimmte Benutzerkonten zu beschränken. ***Role-Based Authorization*** untersucht die Autorisierungsregeln auf die Rolle bereitstellen, Ebene; und schließlich die ***Anzeigen von Inhalt basierend auf dem derzeit protokolliert In Benutzer*** Tutorial wird beschrieben, ein bestimmtes ändern die Seite Inhalt und die Funktionalität, die basierend auf dem Benutzer, die auf der Seite. Weitere Informationen zu ASP. Autorisierungsoptionen, dem die NET finden Sie unter [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx).

## <a name="user-accounts-and-roles"></a>Benutzerkonten und Rollen

ASP. NET Formular-Authentifizierung bietet eine Infrastruktur für die Benutzer melden Sie sich an einem Standort und authentifizierte Zustand gespeichert, auf der Seite besuchen. Und URL-Autorisierung bietet ein Framework zum Einschränken des Zugriffs auf bestimmte Dateien oder Ordner in einer ASP.NET-Anwendung. Keines dieser Features, bietet jedoch eine Möglichkeit zum Speichern von Informationen für das Benutzerkonto ein, oder Verwalten von Rollen.

Vor ASP.NET 2.0 konnten Entwickler dafür verantwortlich, ihren eigenen Speicher Benutzer und die Rolle erstellen. Es waren auch der Hook für die Benutzeroberflächen entwerfen und Schreiben von Code für wichtige Benutzer Seiten beziehen, z. B. die Anmeldungsseite und die Seite, um ein neues Konto, unter anderem zu erstellen. Ohne alle integriertes Benutzer-Konto-Framework in ASP.NET jeder Entwickler musste, dass die implementierende Benutzerkonten auf seinen eigenen entwurfsentscheidungen auf Fragen, eingehen werden wie Kennwörter oder andere sensible Daten gespeichert? und welche Richtlinien sollte ich in Bezug auf Kennwortlänge und Sicherheit?

Implementieren von Benutzerkonten in einer ASP.NET-Anwendung ist heute einfacher Dank der *mitgliedschaftsframework* und die integrierte Anmeldung Web steuert. Das mitgliedschaftsframework ist eine Reihe von Klassen in der [Namespace System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) , die Funktionalität zum Ausführen von Aufgaben für wichtige Benutzer im Zusammenhang mit Konto bereitstellen. Ist die Hauptklasse in das mitgliedschaftsframework der [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), die über Methoden wie verfügt:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Das mitgliedschaftsframework verwendet die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), die ordnungsgemäß trennt das mitgliedschaftsframework-API von ihrer Implementierung. Dies ermöglicht es Entwicklern, eine gemeinsame API verwenden, aber Sie können sie eine Implementierung zu verwenden, die benutzerdefinierte Anforderungen ihrer Anwendung erfüllt. Kurz gesagt, die Mitgliedschaft-Klasse definiert die grundlegende Funktionalität des Frameworks (wie die Methoden, Eigenschaften und Ereignisse), aber nicht tatsächlich keine Implementierungsdetails bereitstellt. Stattdessen rufen die Methoden der Klasse für die Mitgliedschaft den konfigurierten Anbieter, der ist, führt die eigentliche Arbeit. Beim Aufrufen der Methode CreateUser der Membership-Klasse nicht die Mitgliedschaft-Klasse z. B. auch die Details der Speicher des Benutzers kennen. Er weiß nicht, wenn Benutzer in einer Datenbank, in eine XML-Datei oder in einen anderen Speicher verwaltet werden. Klasse Membership überprüft die Konfiguration der Webanwendung, um zu bestimmen, welche Anbieter delegieren den Aufruf von, und diese Provider-Klasse ist zuständig für das neue Benutzerkonto tatsächlich in den Speicher des entsprechenden Benutzers zu erstellen. Diese Interaktion ist in Abbildung 3 dargestellt.

Microsoft liefert zwei Membership-Provider-Klassen in .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -Membership-API in Active Directory und Active Directory Application Mode (ADAM)-Server implementiert.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -Membership-API in einer SQL Server-Datenbank implementiert.

Diese tutorialreihe konzentriert sich ausschließlich auf die SqlMembershipProvider.


[![Die Anbieter-Modell ermöglicht es verschiedenen Implementierungen nahtlos integriert in die Anwendungsframework](security-basics-and-asp-net-support-vb/_static/image4.png)](security-basics-and-asp-net-support-vb/_static/image3.png)

**Abbildung 03**: der Anbieter-Modell können verschiedene Implementierungen nahtlos integriert in die Anwendungsframework ([klicken Sie, um das Bild in voller Größe anzeigen](security-basics-and-asp-net-support-vb/_static/image5.png))


Der Vorteil des Anbieter-Modells ist, dass alternative Implementierungen von Microsoft, Drittanbietern und einzelne Entwickler entwickelt und nahtlos an das mitgliedschaftsframework angeschlossen werden können. Microsoft hat z. B. [eine Membership-Provider für Microsoft Access-Datenbanken](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Weitere Informationen zu den Mitgliedschaftsanbieter, finden Sie in der [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), der enthält einer Exemplarische Vorgehensweise der Mitgliedschaftsanbieter, benutzerdefinierte Beispielanbieter, mehr als 100 Seiten Dokumentation für das Providermodell, und die Führen Sie Quellcode für die integrierte Mitgliedschaftsanbieter (nämlich ActiveDirectoryMembershipProvider und SqlMembershipProvider) ein.

ASP.NET 2.0 verwendet auch das Rollen-Framework. Wie das mitgliedschaftsframework wird das Framework für die Rollen auf dem Anbietermodell erstellt. Die API verfügbar gemacht wird, über die [Rollen Klasse](https://msdn.microsoft.com/library/system.web.security.roles.aspx) und .NET Framework im Lieferumfang von drei Anbieterklassen:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -Informationen in einem Autorisierungs-Manager-Richtlinienspeicher, z. B. Active Directory- oder ADAM verwaltet.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -Rollen in einer SQL Server-Datenbank implementiert.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -Rolleninformationen auf Grundlage des Besuchers Windows-Gruppe verknüpft. Diese Methode wird in der Regel mit der Windows-Authentifizierung verwendet.

Diese tutorialreihe konzentriert sich ausschließlich auf SqlRoleProvider.

Da das Providermodell eine einzelne Forward-Facing-API (die Klassen für die Mitgliedschaft und Rollen) enthält, es ist möglich, Funktionen, um die API zu erstellen, ohne die Details der Implementierung kümmern – die von den Anbietern ausgewählt, die von der Seite behandelt werden Entwickler. Diese einheitliche API ermöglicht für Microsoft und Drittanbietern zum Erstellen von Websteuerelementen dieser Schnittstelle mit den Frameworks Mitgliedschaften und Rollen. ASP.NET enthält eine Reihe von [Anmeldung websteuerungselemente](https://msdn.microsoft.com/library/ms178329.aspx) für die Implementierung von gemeinsamen Benutzeroberflächen für Benutzer-Konto. Z. B. die [Anmeldesteuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) fordert den Benutzer zur Eingabe der Anmeldeinformationen, überprüft diese und dann wieder im über die Formularauthentifizierung. Die [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) bietet Vorlagen für die Anzeige von unterschiedliche Markups für anonyme Benutzer im Vergleich zu authentifizierten Benutzern oder anderen Ansichtsmarkups basierend auf den die Rolle des Benutzers. Und die [Steuerelement CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) stellt eine assistentenartige Benutzeroberfläche zum Erstellen eines neuen Benutzerkontos.

Die Mitgliedschaft und Rollen Frameworks interagieren die verschiedenen Steuerelemente für die Anmeldung, unter der Hauptebene. Die meisten Steuerelemente für die Anmeldung können implementiert werden, ohne eine einzige Codezeile schreiben zu müssen. Diese Steuerelemente ausführlicher untersuchen wir in zukünftigen Lernprogramme, darunter auch Techniken zum Erweitern und Anpassen von ihrer Funktionalität.

## <a name="summary"></a>Zusammenfassung

Alle Webanwendungen, die die Benutzerkonten zu unterstützen, erfordern ähnliche Funktionen wie z. B.: die Möglichkeit für Benutzer, sich anzumelden und ihre Log in den Status auf der Seite besuchen, gespeichert haben eine Webseite für neue Besucher ein Konto zu erstellen. und die Möglichkeit, den Seitenentwickler, die angeben, welche Ressourcen, Daten und Funktionen, für welche Benutzer oder Rollen verfügbar sind. Die Tasks authentifizieren und Autorisieren von Benutzern und zum Verwalten von Benutzerkonten und Rollen ist erstaunlich einfach in ASP.NET-Anwendungen Dank der Formularauthentifizierung, URL-Autorisierung und die Frameworks Mitgliedschaften und Rollen umgesetzt werden können.

Im Laufe der nächsten mehrere Lernprogramme, untersuchen wir diese Aspekte von schrittweise eine funktionierende-Web-Anwendung von Grund auf erstellen. In den nächsten beiden Lernprogramm wird der Formularauthentifizierung im Detail erläutert. Wir sehen den Workflow der Forms-Authentifizierung in Aktion, Mal genauer an das Formularauthentifizierungsticket, mit denen die Sicherheitsrisiken und erfahren Sie, wie das Forms-Authentifizierungssystem zu konfigurieren – alle während der Erstellung einer Web-Anwendung, die ermöglicht, dass Besucher, sich anzumelden, und melden Sie sich ab.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET 2.0 Mitgliedschaft, Rollen, Formularauthentifizierung und Sicherheitsressourcen](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0-Sicherheitsrichtlinien](https://msdn.microsoft.com/library/ms998258.aspx)
- [Authentifizierung in ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Übersicht über ASP.NET-Anmeldungssteuerelemente](https://msdn.microsoft.com/library/ms178329.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Wie schütze I: meiner Website mit Mitgliedschaften und Rollen?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Einführung in die Mitgliedschaft](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professional ASP.NET 2.0 Security, Mitgliedschafts- und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Der Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben Büchern zu ASP/ASP.NET und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), arbeitet mit Microsoft-Web-Technologien seit 1998. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird [*Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er ist unter [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog finden Sie unter [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde dieses Tutorial, in die Reihe von viele hilfreiche Reviewer überprüft wurde. Führendes Prüfer für dieses Tutorial sind Alicja Maziarz, John Suru und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](forms-authentication-configuration-and-advanced-topics-cs.md)
> [Weiter](an-overview-of-forms-authentication-vb.md)
