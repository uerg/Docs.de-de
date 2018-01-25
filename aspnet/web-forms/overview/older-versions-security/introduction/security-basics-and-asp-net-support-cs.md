---
uid: web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
title: "Grundlagen der Codezugriffssicherheit und Unterstützung für ASP.NET (c#) | Microsoft Docs"
author: rick-anderson
description: "Dies ist eine Reihe von Lernprogrammen, die Techniken zum Authentifizieren von Besuchern über ein Webformular Autorisieren des Zugriffs auf partic durchsuchen, wird im ersten Lernprogramm..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2008
ms.topic: article
ms.assetid: 07e15538-2f29-40c6-b2e7-e6115075ac83
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/security-basics-and-asp-net-support-cs
msc.type: authoredcontent
ms.openlocfilehash: 4080e3ccaffefd02c76b89a77e320e963f854961
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="security-basics-and-aspnet-support-c"></a>Grundlagen der Codezugriffssicherheit und Unterstützung für ASP.NET (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF herunterladen](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial01_Basics_cs.pdf)

> Dies ist eine Reihe von Lernprogrammen, die Techniken zum Besucher über ein Webformular authentifizieren und Autorisieren des Zugriffs auf bestimmte Seiten und Funktionen Verwalten von Benutzerkonten in einer ASP.NET-Anwendung zu durchsuchen, wird im ersten Lernprogramm.


## <a name="introduction"></a>Einführung

Was ist die aufpassen Foren, e-Commerce-Websites, online-e-Mail-Websites, Portalwebsites und soziale Netzwerkstandorte, die alle Gemeinsamkeit aufweisen? Alle bieten *Benutzerkonten*. Standorte, die Benutzerkonten bieten müssen eine Reihe von Diensten bereitstellen. Mindestens neue Besucher müssen ein Konto erstellen können, und Rückgabe Besucher müssen in der Lage, melden Sie sich. Solche Webanwendungen können Entscheidungen basierend auf dem angemeldeten Benutzer: Einige Seiten oder Aktionen können eingeschränkt werden, nur in der Benutzer oder auf eine bestimmte Teilmenge der Benutzer angemeldet andere Seiten möglicherweise Informationen bestimmte für den angemeldeten Benutzer oder anzeigen können anzeigen mehr oder weniger Informationen, je nachdem, welche Benutzer auf die Seite angezeigt wird.

Dies ist eine Reihe von Lernprogrammen, die Techniken zum Besucher über ein Webformular authentifizieren und Autorisieren des Zugriffs auf bestimmte Seiten und Funktionen Verwalten von Benutzerkonten in einer ASP.NET-Anwendung zu durchsuchen, wird im ersten Lernprogramm. Im Verlauf der Ausführung dieser Lernprogramme untersuchen wir Vorgehensweise:

- Identifizieren und melden Sie sich Benutzer in einer Websites
- Verwenden von ASP. NET Mitgliedschaft Framework zum Verwalten von Benutzerkonten
- Erstellen, aktualisieren und Löschen von Benutzerkonten
- Einschränken des Zugriffs auf eine Webseite, Verzeichnis oder bestimmte Funktionen, die basierend auf dem angemeldeten Benutzer
- Verwenden von ASP. NET Rollen Framework Rollen Benutzerkonten zugeordnet werden soll
- Verwalten von Benutzerrollen
- Einschränken des Zugriffs auf eine Webseite, Verzeichnis oder bestimmte Funktionen, die basierend auf der Rolle des angemeldeten Benutzers
- Anpassen und Erweitern von ASP. NET Sicherheit-Websteuerelemente

Diese Lernprogramme sind darauf ausgerichtet, um präzise sein, und geben Sie schrittweise Anweisungen mit Screenshots, Sie durch den Prozess visuell zu durchlaufen. Jedes Lernprogramm ist in c# und Visual Basic-Versionen verfügbar und enthält einen Download der den vollständigen Code verwendet. (Dieser ersten Lernprogramm konzentriert sich auf Schlüsselbegriffe der Sicherheit aus einer Sicht auf hoher Ebene und daher keine zugehörigen Code enthält.)

In diesem Lernprogramm wird Folgendes erörtert wichtige Konzepte und welche Funktionen in ASP.NET unterstützen Sie beim Implementieren der Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen verfügbar sind. Fangen wir an!

> [!NOTE]
> Sicherheit ist ein wichtiger Aspekt jeder Anwendung, die physische, technologischen umfasst, und Richtlinie Entscheidungen und erfordert ein hohes Maß an Planung und Kenntnisse. Diese Reihe von Lernprogrammen ist nicht als Leitfaden für sichere Anwendungsentwicklung vorgesehen. Stattdessen konzentriert sich sie insbesondere auf formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen. Während einige Schlüsselbegriffe der Sicherheit drehen, um diese Probleme in dieser Serie erläutert wird sind, bleiben andere unexplored.


## <a name="authentication-authorization-user-accounts-and-roles"></a>Authentifizierung, Autorisierung, Benutzerkonten und Rollen

Authentifizierung, Autorisierung, Benutzerkonten und Rollen werden vier-Begriffe, die in der gesamten dieser Reihe von Lernprogrammen, sehr häufig verwendet werden sollen, damit ich einen schnellen Moment Zeit, diese Begriffe im Kontext der websicherheit definieren möchten. Es gibt viele Szenarien, die in denen der Server der anfordernde Client identifizieren muss, in einem Client / Server-Modell, z. B. im Internet. *Authentifizierung* versteht man die Identität des Clients zu ermitteln. Ein Client, der erfolgreich identifiziert wurde gilt als *authentifiziert*. Eine unbekannte Clients gilt als *nicht authentifizierte* oder *anonyme*.

Sichere Authentifizierungssysteme umfassen mindestens eine der folgenden drei Facets: [etwas Sie wissen, etwas stehen Ihnen oder etwas Domänenmodus](http://www.cs.cornell.edu/Courses/cs513/2005fa/NNLauthPeople.html). Die meisten Webanwendungen beruhen auf der Client bekannt, z. B. eines Kennworts oder einer PIN ist ein. Die Informationen zum Identifizieren eines Benutzers - ihres Benutzernamens und Kennworts, z. B. - verwendet als bezeichnet *Anmeldeinformationen*. Diese Reihe von Lernprogrammen zu realisieren *Formularauthentifizierung*, also ein Authentifizierungsmodell, in denen Benutzer melden Sie sich an den Standort durch die Bereitstellung ihrer Anmeldeinformationen in einem Formular Webseite. Wir haben alle diese Art von Zertifizierung vor aufgetreten ist. Wechseln Sie zu einer e-Commerce-Website. Wenn Sie auschecken möchten, werden Sie gefragt, melden Sie sich durch Eingabe von Benutzernamen und Kennwort in die Textfelder auf einer Webseite.

Zusätzlich zu den Clients identifiziert, müssen ein Server einzuschränken, welche Ressourcen oder Funktionalitäten abhängig von der anfordernde Client zugänglich sind. *Autorisierung* ist der Prozess, um zu bestimmen, ob ein bestimmter Benutzer die Berechtigung zum Zugriff auf eine bestimmte Ressource und Funktionen aufweist.

Ein *Benutzerkonto* ist ein Speicher für das Beibehalten von Informationen zu einem bestimmten Benutzer. Benutzerkonten müssen minimal Informationen enthalten, die der Benutzer, z. B. Anmeldung und dasselbe Kennwort des Benutzers eindeutig identifiziert. Benutzerkonten eventuell zusammen mit diesem wichtige Informationen, z. B.: e-Mail-Adresse des Benutzers; das Datum und die Uhrzeit der Erstellung des Kontos; das Datum und die Uhrzeit der letzten angemeldeten; vor- und Nachnamen; Telefonnummer; und e-Mail-Adresse. Bei der Formularauthentifizierung verwenden, ist die Benutzerkontoinformationen in der Regel in einer relationalen Datenbank wie Microsoft SQL Server gespeichert.

Webanwendungen, die Benutzerkonten unterstützen möglicherweise optional Gruppieren von Benutzern in *Rollen*. Eine Rolle ist einfach eine Bezeichnung, die für einen Benutzer angewendet wird, und stellt eine Abstraktion zum Definieren von Autorisierungsregeln und Seitenebene Funktionalität bereit. Eine Website kann z. B. eine Administratorrolle mit Autorisierungsregeln umfassen, die jeder Benutzer aber ein Administrator auf einen bestimmten Satz von Webseiten verhindert werden soll. Darüber hinaus kann eine Vielzahl von Seiten, die für alle Benutzer (einschließlich nicht-Administratoren) zugegriffen werden zusätzliche Daten anzeigen oder bieten zusätzlichen Funktionalität, wenn vom Benutzer in der "Administratoren" aktiviert. Bei Verwendung von Rollen können wir diese Autorisierungsregeln für eine Rolle von Rolle verwenden, statt die vom Benutzer definieren.

## <a name="authenticating-users-in-an-aspnet-application"></a>Authentifizieren von Benutzern in einer ASP.NET-Anwendung

Wenn ein Benutzer eine URL in ihren Browsers-Fenster oder Klicks auf einen Link eingibt, wird der Browser eine [Hypertext Transfer Protocol (HTTP)](http://en.wikipedia.org/wiki/HTTP) an den Webserver für den angegebenen Inhalt anfordern, werden sie eine ASP.NET Seite, ein Bild, JavaScript Datei oder eines anderen Typs von Inhalt. Der Webserver wird damit beauftragt, Zurückgeben des angeforderten Inhalts. In diesem Fall müssen sie bestimmen, diverse Aspekte der Anforderung, einschließlich die Anforderung und gibt an, ob die Identität zum Abrufen des angeforderten Inhalts autorisiert ist.

Standardmäßig senden Browser HTTP-Anforderungen, die keine Art von Informationen zur Identifizierung. Wenn der Browser Authentifizierungsinformationen enthält dann der Webserver, startet aber den authentifizierungsworkflow, die versucht, die der anfordernde Client identifizieren. Die Schritte des Workflows Authentifizierung richten sich nach den Typ der Authentifizierung, die von der Webanwendung verwendet wird. ASP.NET unterstützt drei Authentifizierungsarten: Windows, Passport- und Formularen. Diese Reihe von Lernprogrammen konzentriert sich auf Formularauthentifizierung, aber wir nehmen Sie sich zu vergleichen und vergleichen Sie Workflow und Windows-Authentifizierung Benutzer speichert.

### <a name="authentication-via-windows-authentication"></a>Authentifizierung mittels Windows-Authentifizierung

Der Windows-Authentifizierung-Workflow verwendet einen der folgenden Methoden der Authentifizierung:

- Standardauthentifizierung.
- Die Digest-Authentifizierung
- Integrierte Windows-Authentifizierung

Alle drei Techniken funktionieren auf ungefähr die gleiche Weise: Wenn ein nicht autorisierter anonyme Anforderung eingeht, der Webserver sendet eine HTTP-Antwort, der angibt, Autorisierung ist erforderlich, um den Vorgang fortzusetzen. Der Browser zeigt ein modales Dialogfeld an, das der Benutzer Benutzername und Kennwort (siehe Abbildung 1) aufgefordert. Diese Informationen wird dann wieder an den Webserver über einen HTTP-Header gesendet.


![Ein modales Dialogfeld wird der Benutzer seine Anmeldeinformationen aufgefordert.](security-basics-and-asp-net-support-cs/_static/image1.png)

**Abbildung 1**: ein modales Dialogfeld wird der Benutzer seine Anmeldeinformationen aufgefordert.


Die angegebenen Anmeldeinformationen werden anhand des Webservers Windows Benutzerspeicher überprüft. Dies bedeutet, dass jeder authentifizierte Benutzer in Ihrer Webanwendung ein Windows-Konto in Ihrer Organisation benötigen. Dies ist inzwischen in Intranetszenarien. In der Tat bietet bei Verwendung von integrierten Windows-Authentifizierung in einem Intranet-Einstellung des Browsers automatisch den Webserver mit den Anmeldeinformationen zur Anmeldung auf des Netzwerks, wodurch ein zukünftiges unterdrückt das Dialogfeld in Abbildung 1 dargestellt. Während der Windows-Authentifizierung für Intranetanwendungen schwerwiegend sind, ist es in der Regel nicht realisierbar für Internetanwendungen, da keine Windows-Konten für jede einzelne Benutzer zu erstellen, die sich an Ihrem Standort registriert werden soll.

### <a name="authentication-via-forms-authentication"></a>Authentifizierung über die Formularauthentifizierung verwenden

Formularauthentifizierung eignet sich dagegen auf für Internet-Webanwendungen. Denken Sie daran, dass die Formularauthentifizierung den Benutzer identifiziert, von der Aufforderung zur Eingabe von Anmeldeinformationen durch ein Web Form. Deshalb, wenn ein Benutzer versucht, eine nicht autorisierte Ressource zuzugreifen, wird er automatisch zur Anmeldeseite umgeleitet, wo sie ihre Anmeldeinformationen eingeben können. Die übermittelten Anmeldeinformationen werden dann anhand eines benutzerdefinierten Benutzerspeichers – in der Regel in einer Datenbank überprüft.

Nach der Überprüfung der übermittelten Anmeldeinformationen eine *bildet Authentifizierungsticket* für den Benutzer erstellt wird. Dieses Ticket gibt an, dass der Benutzer authentifiziert wurde und Identifikationsinformationen, z. B. den Benutzernamen enthält. Das Formularauthentifizierungsticket wird (in der Regel) als ein Cookie auf dem Clientcomputer gespeichert. Daher enthalten folgeaufrufen der Website das Formularauthentifizierungsticket in der HTTP-Anforderung, dadurch wird die Webanwendung zur Identifizierung des Benutzers, sobald sie sich angemeldet haben.

Abbildung 2 zeigt den Forms-Authentifizierung-Workflow aus einer allgemeinen Ausgangspunkt. Beachten Sie, wie die Authentifizierung und Autorisierung Teile in ASP.NET als zwei separate Entitäten fungieren. Die Formulare Authentifizierungssystem identifiziert den Benutzer (oder gibt an, dass sie anonym sind). Die Autorisierungssystem wird bestimmt, ob der Benutzer den Zugriff auf die angeforderte Ressource verfügt. Wenn der Benutzer ist nicht autorisiert, (wie sie beim Versuch, die anonym ProtectedPage.aspx finden in Abbildung 2 sind), die Autorisierungssystem meldet, dass der Benutzer verweigert wird, verursacht die Formulare Authentifizierungssystem, um den Benutzer automatisch zur Anmeldeseite umzuleiten.

Sobald der Benutzer erfolgreich angemeldet hat, sind nachfolgende HTTP-Anforderungen das Formularauthentifizierungsticket. Der Forms-Authentifizierungssystem lediglich identifiziert den Benutzer – ist das Autorisierungssystem, das bestimmt, ob der Benutzer die angeforderte Ressource zugreifen kann.


![Der Forms-Authentifizierung-Workflow](security-basics-and-asp-net-support-cs/_static/image2.png)

**Abbildung 2**: der Forms-Authentifizierung-Workflow


Wir wird die Formularauthentifizierung wesentlich ausführlicher in den nächsten beiden Lernprogrammen Näheres[eine Übersicht der Formularauthentifizierung](an-overview-of-forms-authentication-cs.md) und [Konfiguration der Formularauthentifizierung und erweiterte Themen](forms-authentication-configuration-and-advanced-topics-cs.md). Weitere Informationen über die ASP. NET Authentifizierungsoptionen, finden Sie unter [ASP.NET-Authentifizierung](https://msdn.microsoft.com/library/eeyk640h.aspx).

## <a name="limiting-access-to-web-pages-directories-and-page-functionality"></a>Beschränken des Zugriffs auf Webseiten, Verzeichnisse und die Funktionen der Seite

ASP.NET bietet zwei Möglichkeiten, um zu bestimmen, ob ein bestimmter Benutzer Zugriff auf eine bestimmte Datei oder ein Verzeichnis hat:

- **Datei Autorisierung** – da ASP.NET-Seiten und Web Services implementiert werden, wie Dateien, die auf dem Webserver-Dateisystem, Zugriff auf diese Dateien befinden, die über Zugriffssteuerungslisten (ACLs) angegeben werden können. Autorisierung wird am häufigsten mit Windows-Authentifizierung verwendet, da ACLs Berechtigungen sind, die für Windows-Konten gelten. Wenn Sie die Formularauthentifizierung verwenden, werden alle Betriebssystem- und den Dateinamen auf Systemebene Anforderungen von dem gleichen Windows-Konto, unabhängig von der Website für den Benutzer ausgeführt.
- **URL-Autorisierung**-mit URL-Autorisierung, gibt der Seitenentwickler Autorisierungsregeln in "Web.config". Diese Autorisierungsregeln angeben, welche Benutzer oder Rollen können zugreifen bzw. verweigert den Zugriff auf bestimmte Seiten oder der Verzeichnisse in der Anwendung.

Autorisierung und URL-Autorisierung definieren Autorisierungsregeln für den Zugriff auf eine bestimmte ASP.NET-Seite oder für alle ASP.NET-Seiten in einem bestimmten Verzeichnis ein. Mit diesen Techniken können wir anweisen, ASP.NET ablehnen von Anforderungen an eine bestimmte Seite für einen bestimmten Benutzer oder ermöglichen den Zugriff auf eine Gruppe von Benutzern und allen anderen Benutzern den Zugriff verweigert. Was ist mit Szenarien, in dem alle Benutzer können auf die Seite zugreifen, aber die Seite-Funktion basiert auf der Benutzer? Beispielsweise haben viele Websites, die Benutzerkonten unterstützen Seiten, die unterschiedliche Inhalte oder Daten für authentifizierte Benutzer im Vergleich zu anonymen Benutzern anzeigen. Ein anonymer Benutzer möglicherweise einen Link zum Anmelden bei der Website angezeigt, während ein authentifiziertes Benutzers stattdessen würde eine Meldung angezeigt, wie z. B., Willkommen zurück, *Benutzername* zusammen mit einem Link zum Abmelden. Ein weiteres Beispiel: Wenn Sie ein Element an einer Auktionswebsite anzeigen finden Sie verschiedene Informationen abhängig davon, ob Sie ein Käufer oder dem auctioning des Elements sind.

Solche Seitenebene Anpassungen können deklarativ oder programmgesteuert erfolgen. Anzuzeigenden unterschiedliche Inhalte für anonyme als authentifizierten Benutzer einfach ziehen Sie eine [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) auf das Zeichenblatt, und geben Sie den entsprechenden Inhalt in dessen AnonymousTemplate und LoggedInTemplate Vorlagen. Alternativ Sie können programmgesteuert zu ermitteln, ob die aktuelle Anforderung authentifiziert wird, der Benutzer und welche Rolle sie angehören (sofern vorhanden). Sie können diese Informationen verwenden, klicken Sie dann anzeigen oder Ausblenden von Spalten in einem Raster oder Bereiche auf der Seite.

Diese Reihe umfasst drei Lernprogramme, die auf der Autorisierung zu konzentrieren. ***Benutzerbasierte Autorisierung***untersucht, wie den Zugriff auf eine Seite oder Seiten in einem Verzeichnis für bestimmte Benutzerkonten; begrenzen ***Role-Based Autorisierung*** Autorisierungsregeln auf der Rolle angeben, Ebene; schließlich prüft die ***Anzeigen von Inhalt basierend auf dem derzeit protokolliert In Benutzer*** Lernprogramm wird erklärt, mit eine bestimmten ändern die Seite Inhalt und die Funktionen, die basierend auf dem Benutzer Zugriff auf die Seite. Weitere Informationen über die ASP. NET Autorisierungsoptionen, finden Sie unter [ASP.NET Authorization](https://msdn.microsoft.com/library/wce3kxhd.aspx).


## <a name="user-accounts-and-roles"></a>Benutzerkonten und Rollen

ASP IST. NET Formular-Authentifizierung bietet eine Infrastruktur für Benutzer zum Anmelden an einem Standort und authentifizierten Status über Seite Besuche gespeichert. Und URL-Autorisierung bietet ein Framework zum Beschränken des Zugriffs auf bestimmte Dateien oder Ordner in einer ASP.NET-Anwendung. Weder Funktion stellt jedoch eine Möglichkeit zum Speichern von Informationen für das Benutzerkonto oder Verwalten von Rollen.

Vor dem ASP.NET 2.0 wurden die Entwickler verantwortlich für das Erstellen von eigenen Speicher Benutzer und die Rolle. Sie haben auch die Hookfunktion zum Entwerfen von Benutzeroberflächen und das Schreiben des Codes für wichtige Benutzer kontobezogene Seiten wie die Anmeldeseite und der Seite, um ein neues Konto, unter anderem erstellen. Ohne alle integriertes Konto Framework in ASP.NET jeder implementierende Benutzerkonten auf Fragen like, sein eigenes entwurfsentscheidungen ankommen mussten Entwickler werden wie Kennwörter oder andere vertrauliche Informationen gespeichert? und welche Richtlinien sollte ich hinsichtlich Kennwortlänge und Stärke erzwingen?

Implementieren von Benutzerkonten in einer ASP.NET-Anwendung ist viel einfacher Dank heute die *Mitgliedschaft Framework* und die integrierte Anmeldung Websteuerelemente. Das Framework Mitgliedschaft ist eine Handvoll Klassen in der [Namespace System.Web.Security](https://msdn.microsoft.com/library/system.web.security.aspx) , die Funktionalität für Benutzer wichtige kontobezogene Aufgaben bereitstellen. Die Hauptklasse in Mitgliedschaft Framework ist die [Mitgliedschaftsklasse](https://msdn.microsoft.com/library/system.web.security.membership.aspx), dem verfügt über Methoden wie z. B.:

- CreateUser
- DeleteUser
- GetAllUsers
- GetUser
- UpdateUser
- ValidateUser

Das Framework Mitgliedschaft verwendet die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), die saubersten trennt die Mitgliedschaft Framework-API über die Implementierung. Dies ermöglicht es Entwicklern, eine gemeinsame-API verwenden, jedoch können eine Implementierung verwendet, die ihre benutzerdefinierte anwendungsanforderungen erfüllt werden. Kurz gesagt, die Mitgliedschaft-Klasse definiert die grundlegende Funktionalität des Frameworks (Methoden, Eigenschaften und Ereignisse), aber nicht tatsächlich Implementierungsdetails bereitstellt. Stattdessen rufen Sie die Methoden der Klasse Mitgliedschaft den konfigurierten Anbieter, der ist, was die eigentliche Arbeit ausführt. Z. B. wenn die Mitgliedschaftsklasse CreateUser-Methode aufgerufen wird, weiß nicht, die Mitgliedschaft-Klasse die Details der Speicher des Benutzers. Es ist nicht bekannt, wenn Benutzer in einer Datenbank oder in eine XML-Datei oder in einem anderen Speicher verwaltet werden. Die Mitgliedschaftsklasse untersucht die Webanwendung-Konfiguration, um zu bestimmen, welche Anbieter delegieren den Aufruf von, und dieser Anbieterklasse ist verantwortlich für das tatsächlich das neue Benutzerkonto erstellen, in den Speicher des entsprechenden Benutzers. Diese Aktivität wird in Abbildung 3 veranschaulicht.

Microsoft bietet zwei Mitgliedschaft-Anbieterklassen in .NET Framework:

- [ActiveDirectoryMembershipProvider](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) -implementiert die API für die Mitgliedschaft in Active Directory und Active Directory Application Mode (ADAM)-Servern.
- [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) -implementiert die API für die Mitgliedschaft in einer SQL Server-Datenbank.

Diese Reihe von Lernprogrammen konzentriert sich ausschließlich auf die SqlMembershipProvider.


[![Das Modell ermöglicht unterschiedliche Implementierungen von Schlüsselspeicheranbietern nahtlos angeschlossen in das Framework werden&lt;/ strong&gt;](security-basics-and-asp-net-support-cs/_static/image4.png)](security-basics-and-asp-net-support-cs/_static/image3.png)

**Abbildung 03**: der Anbieter Modell können verschiedene Implementierungen nahtlos angeschlossen in das Framework sein ([klicken Sie hier, um das Bild in voller Größe angezeigt](security-basics-and-asp-net-support-cs/_static/image5.png))


Der Vorteil, dass das Anbietermodell ist, dass alternative Implementierungen von Microsoft, von Drittanbietern oder einzelnen Entwicklern entwickelt und nahtlos an das Framework Mitgliedschaft angeschlossen werden können. Microsoft stellt z. B. [einem Mitgliedschaftsanbieter für Microsoft Access-Datenbanken](https://download.microsoft.com/download/5/5/b/55bc291f-4316-4fd7-9269-dbf9edbaada8/sampleaccessproviders.vsi). Weitere Informationen zu den Mitgliedschaftsanbieter, finden Sie in der [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx), der enthält einer Exemplarische Vorgehensweise den Mitgliedschaftsanbieter, Beispiel benutzerdefinierte Anbieter, mehr als 100 Seiten der Dokumentation für das Anbietermodell und die Führen Sie Quellcode für die integrierte Mitgliedschaftsanbieter (nämlich ActiveDirectoryMembershipProvider und SqlMembershipProvider).

ASP.NET 2.0 verwendet auch das Rollen-Framework. Wie das Framework Mitgliedschaft wird das Rollen-Framework Anbietermodell erstellt. Seine API verfügbar gemacht wird, über die [Rollen Klasse](https://msdn.microsoft.com/library/system.web.security.roles.aspx) und .NET Framework enthält drei Anbieterklassen:

- [AuthorizationStoreRoleProvider](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) -Rolleninformationen in einem Autorisierungs-Manager Richtlinienspeicher, z. B. Active Directory- oder ADAM verwaltet.
- [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) -Rollen in einer SQL Server-Datenbank implementiert.
- [WindowsTokenRoleProvider](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx) -ordnet Rolleninformationen basierend auf Windows-Gruppe für den Besucher. Diese Methode wird in der Regel mit Windows-Authentifizierung verwendet.

Diese Reihe von Lernprogrammen konzentriert sich ausschließlich auf SqlRoleProvider.

Da das Anbietermodell eine einheitliche vorwärts gerichteten-API (die Klassen für die Mitgliedschaft und Rollen) enthält, ist es möglich, Funktionen, um diese API zu erstellen, ohne den Implementierungsdetails zu kümmern – die von den Anbietern ausgewählt, die von der Seite behandelt werden Developer. Diese einheitliche API ermöglicht Microsoft und Drittanbietern Websteuerelemente dieser Schnittstelle mit dem Frameworks Mitgliedschaft und Rollen erstellen. ASP.NET mit einer Anzahl von geliefert [Anmeldung Websteuerelemente](https://msdn.microsoft.com/library/ms178329.aspx) für allgemeine Benutzer Konto Benutzeroberflächen implementieren. Z. B. die [Steuerelement für die Anmeldung](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) überprüft und protokolliert in über Formularauthentifizierung wird ein Benutzer zur Eingabe ihrer Anmeldeinformationen aufgefordert. Die [LoginView-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) bietet Vorlagen zur Anzeige von verschiedenem Markup für anonyme Benutzer im Vergleich zu authentifizierten Benutzern oder anderen Ansichtsmarkups basierend auf den die Rolle des Benutzers. Und die [Steuerelement CreateUserWizard](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.aspx) bietet eine schrittweise Benutzeroberfläche zum Erstellen eines neuen Benutzerkontos.

Die Mitgliedschaft und Rollen Frameworks interagieren verschiedene Anmeldesteuerelementen, im Hintergrund. Die meisten Anmeldesteuerelementen können implementiert werden, ohne eine einzige Codezeile schreiben zu müssen. Diese Steuerelemente ausführlicher untersuchen wir in zukünftigen Lernprogrammen, einschließlich der Techniken zum Erweitern und Anpassen von ihrer Funktionalität.

## <a name="summary"></a>Zusammenfassung

Alle Webanwendungen, die Benutzerkonten zu unterstützen, erfordern ähnliche Funktionen wie z. B.: die Möglichkeit für Benutzer zum Anmelden und der Anmeldung über die Seite Besuche; gespeicherte Status haben eine Webseite für neue Besucher ein Konto zu erstellen. sowie die Möglichkeit der Entwickler der Seite angeben, welche Ressource, die Daten und die Funktionalität für welche Benutzer oder Rollen verfügbar sind. Die Aufgaben zu authentifizieren und Autorisieren von Benutzern und Verwalten von Benutzerkonten und Rollen ist erstaunlich einfach, in ASP.NET-Anwendungen Dank Formularauthentifizierung, URL-Autorisierung und die Mitgliedschaft und Rollen-Frameworks zu erreichen.

Im Verlauf der nächsten mehrere Lernprogramme untersuchen wir diese Aspekte schrittweise eine funktionierende Web-Anwendung von Grund auf neu erstellen. In den nächsten beiden Lernprogramm wird Formularauthentifizierung ausführlich untersucht. Wir sehen der Forms-Authentifizierung-Workflow in Aktion, untersuchen Sie das Formularauthentifizierungsticket genau ansehen, erläutert Sicherheitsaspekte und finden Sie unter Konfigurieren der Forms-Authentifizierungssystem - alle beim Erstellen einer Webanwendung, die ermöglicht, dass Besucher zum Anmelden und Abmelden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ASP.NET 2.0 Mitgliedschaft, Rollen, Formularauthentifizierung und Sicherheitsressourcen](https://weblogs.asp.net/scottgu/ASP.NET-2.0-Membership_2C00_-Roles_2C00_-Forms-Authentication_2C00_-and-Security-Resources-)
- [ASP.NET 2.0-Sicherheitsrichtlinien](https://msdn.microsoft.com/library/ms998258.aspx)
- [Authentifizierung in ASP.NET](https://msdn.microsoft.com/library/eeyk640h.aspx)
- [ASP.NET-Autorisierung](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [ASP.NET Anmeldung steuert (Übersicht)](https://msdn.microsoft.com/library/ms178329.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Sichern I: wie meine Website mithilfe von Mitgliedschaft und Rollen?](https://asp.net/learn/videos/video-45.aspx) (Video)
- [Einführung in die Mitgliedschaft](https://msdn.microsoft.com/library/yh26yfzy.aspx)
- [MSDN Security Developer Center](https://msdn.microsoft.com/security/default.aspx)
- [Professionelle ASP.NET 2.0 Sicherheit, Mitgliedschaft und Rollenverwaltung](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Anbietertoolkit](https://msdn.microsoft.com/asp.net/aa336558.aspx)

## <a name="about-the-author"></a>Informationen zum Autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor von sieben ASP/ASP.NET-Büchern und Gründer von [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web-Technologien seit 1998 arbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird [ *Sams Schulen selbst ASP.NET 2.0 in 24 Stunden*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Er die erreicht werden kann, zur [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) oder über seinen Blog die finden Sie unter [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde in diesem Lernprogramm aus, die Reihe von viele nützliche Bearbeiter überprüft wurde. Lead Prüfer für dieses Lernprogramm enthalten Alicja Maziarz, John Suru und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Nächste](an-overview-of-forms-authentication-cs.md)
