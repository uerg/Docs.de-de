---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Erstellen von Benutzerkonten (VB) | Microsoft Docs
author: rick-anderson
description: "In diesem Lernprogramm wird untersucht, mithilfe von Mitgliedschaft Framework (über die SqlMembershipProvider) neue Benutzerkonten erstellen. Wir sehen uns neu zu erstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 61621ffaae98ac74c16b2ff014ba9d85c2c10b3a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="creating-user-accounts-vb"></a>Erstellen von Benutzerkonten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> In diesem Lernprogramm wird untersucht, mithilfe von Mitgliedschaft Framework (über die SqlMembershipProvider) neue Benutzerkonten erstellen. Es wird zum Erstellen neuer Benutzer programmgesteuert und in ASP angezeigt. NET integriertes CreateUserWizard-Steuerelement.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [vorherigen Lernprogramm](creating-the-membership-schema-in-sql-server-vb.md) wir installiert das Schema der Anwendung Dienste in einer Datenbank, die die Tabellen, Sichten, hinzugefügt und gespeicherte Prozeduren, die erforderlich sind, indem Sie die `SqlMembershipProvider` und `SqlRoleProvider`. Dies erstellt die Infrastruktur, die wir für den Rest der Lernprogramme in dieser Serie benötigen. In diesem Lernprogramm wird untersucht, mit dem Framework Mitgliedschaft (über die `SqlMembershipProvider`) neue Benutzerkonten erstellen. Es wird zum Erstellen neuer Benutzer programmgesteuert und in ASP angezeigt. NET integriertes CreateUserWizard-Steuerelement.

Zusätzlich zu wissen, wie Sie neue Benutzerkonten erstellen, es müssen auch die Demo-Website aktualisieren wir zuerst erstellt, in haben der  *<a id="_msoanchor_2"> </a> [eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)*  Lernprogramm, und klicken Sie dann in verbessert die  *<a id="_msoanchor_3"> </a> [Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  Lernprogramm. Unsere Demo-Webanwendung verfügt über eine Anmeldeseite an, die Benutzer-Anmeldeinformationen mit hartcodierten Benutzernamen/Kennwort-Paare überprüft. Darüber hinaus `Global.asax` enthält Code, der benutzerdefinierten erstellt `IPrincipal` und `IIdentity` Objekte für authentifizierte Benutzer. Die Login-Seite, um Anmeldeinformationen für das Framework für die Mitgliedschaft von Benutzern zu überprüfen, und entfernen die benutzerdefinierte Logik für Haupt- und Identitätsobjekte werden aktualisiert.

Fangen wir an!

## <a name="the-forms-authentication-and-membership-checklist"></a>Die Formularauthentifizierung und Mitgliedschaft Prüfliste

Bevor wir mit der das Framework Mitgliedschaft beginnen, werfen wir einen Moment Zeit, die wichtige Schritte zu überprüfen, die wir ausgeführt haben, um diesen Punkt zu erreichen. Bei Verwendung der Mitgliedschaft-Framework mit der `SqlMembershipProvider` in einem Szenario formularbasierte Authentifizierung müssen die folgenden Schritte vor der Implementierung der Funktionalität der Mitgliedschaft in der Webanwendung ausgeführt werden:

1. **Aktivieren Sie die formularbasierte Authentifizierung.** Wie in erläutert  *<a id="_msoanchor_4"> </a> [eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)*, Formularauthentifizierung wird aktiviert, indem Sie die Bearbeitung `Web.config` verwendet wird und die `<authentication>` Elements `mode` -Attribut `Forms`. Bei der Formularauthentifizierung aktiviert wird untersucht jede eingehende Anforderung für eine *bildet Authentifizierungsticket*, falls vorhanden, identifiziert den Requestor.
2. **Fügen Sie die Anwendung Dienstschema, mit der entsprechenden Datenbank.** Bei Verwendung der `SqlMembershipProvider` müssen wir das Schema der Anwendung Dienste in einer Datenbank zu installieren. In der Regel wird dieses Schema auf dieselbe Datenbank hinzugefügt, das Datenmodell für die Anwendung enthält. Die  *<a id="_msoanchor_5"> </a> [erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  Lernprogramm erläutert, mit der `aspnet_regsql.exe` Tool, um dies zu erreichen.
3. **Passen Sie die Webanwendung aus Schritt 2 die Datenbank verweisen.** Die *erstellen das Schema für die Mitgliedschaft in SQL Server* Lernprogramm wurde gezeigt, zwei Methoden zum Konfigurieren der Webanwendung, damit die `SqlMembershipProvider` würde die Datenbank, die in Schritt 2 ausgewählten verwenden: durch Ändern der `LocalSqlServer` Verbindungszeichenfolgenname; oder durch einen neuen registrierten Anbieter der Liste der Framework-Mitgliedschaftsanbieter hinzufügen und Anpassen von dieser neuen Anbieter Verwendung die Datenbank aus Schritt 2.

Beim Erstellen von Webanwendungen verwendet die `SqlMembershipProvider` und formularbasierte Authentifizierung, müssen Sie diese drei Schritte vor der Verwendung der `Membership` Klasse oder die Anmeldung von ASP.NET-Webseiten-Steuerelemente. Da wir diese Schritte bereits im vorherigen Lernprogrammen ausgeführt, können wir mithilfe des Frameworks Mitgliedschaft beginnen!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von neuen ASP.NET-Seiten

In diesem Lernprogramm und den nächsten drei werden wir verschiedene Funktionen im Zusammenhang mit Mitgliedschaft und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in der gesamten Ausführung dieser Lernprogramme untersucht implementieren. Erstellen wir die Seiten und dann auf einer Website-Zuordnungsdatei `(Web.sitemap)`.

Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `Membership`. Als Nächstes fügen Sie fünf neue ASP.NET-Seiten, um die `Membership` Ordner, verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

An diesem Punkt sollte Projektmappen-Explorer des Projekts im Screenshot, der in Abbildung 1 dargestellt aussehen.


[![Fünf neue Seiten wurden in den Ordner Mitgliedschaft hinzugefügt](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Abbildung 1**: fünf neue Seiten wurden hinzugefügt, um die `Membership` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image3.png))


Jede Seite sollte, an diesem Punkt haben, die zwei Inhaltssteuerelemente, jeweils einen für die Gestaltungsvorlage ContentPlaceHolders: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Bedenken Sie, dass die `LoginContent` ContentPlaceHolders Standardmarkup zeigt einen Link zum Anmelden oder Abmelden von der Website, je nachdem, ob der Benutzer authentifiziert wird. Das Vorhandensein der `Content2` Inhaltssteuerelement, überschreibt jedoch Standardmarkup für die Gestaltungsvorlage. Wie in erläutert  *<a id="_msoanchor_6"> </a> [eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)*  Lernprogramm, dies ist hilfreich bei Seiten, in denen es sollten keine Anmeldeoptionen in der linken Spalte anzuzeigen.

Für diese fünf Seiten jedoch wir anzuzeigenden Standardmarkup für die Gestaltungsvorlage der `LoginContent` ContentPlaceHolder. Daher sollten Sie entfernen das deklarative Markup für die `Content2` Inhaltssteuerelement. Nach der Anmeldung sollte jede der fünf Seitenmarkup nur einem Inhaltssteuerelement enthalten.

## <a name="step-2-creating-the-site-map"></a>Schritt 2: Erstellen der Site-Zuordnung

Alle Spalten mit Ausnahme der unbedeutendsten Websites müssen eine Form einer Benutzeroberfläche für die Navigation zu implementieren. Die Benutzeroberfläche für die Navigation möglicherweise eine einfache Liste mit Links zu den verschiedenen Abschnitten des Standorts. Alternativ können diese Links in Menüs oder Strukturansichten angeordnet werden. Als Entwickler von Seiten ist das Erstellen der Navigations-Benutzeroberfläche nur die Hälfte der Story. Wir benötigen auch eine Möglichkeit zum Definieren der logischen Struktur des Standorts in einer Weise verwaltet werden und den aktualisierbaren. Neue Seiten, die hinzugefügt oder vorhandene Seiten entfernt, kann eine einzelne Quelle - Siteübersicht - aktualisiert werden soll, und diese Änderungen auf der Website navigieren Benutzeroberfläche wiederzugeben.

Diese beiden Aufgaben - Siteübersicht definieren und implementieren eine Navigation Benutzeroberfläche basierend auf der Siteübersicht - sind ganz einfach Dank der Siteübersicht-Framework, und die Navigation Websteuerelemente hinzugefügt in ASP.NET Version 2.0. Im Siteübersicht können Entwickler auf eine Siteübersicht definieren und dann über eine programmgesteuerte-API auf Sie zugreifen (die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Die integrierte Navigation Websteuerelemente enthalten eine [Menu-Steuerelement](https://msdn.microsoft.com/library/bz09dy46.aspx), die [TreeView-Steuerelement](https://msdn.microsoft.com/library/3eafky27.aspx), und die [SiteMapPath-Steuerelement](https://msdn.microsoft.com/library/3eafky27.aspx).

Wie die Mitgliedschaft und Rollen-Frameworks, basiert der Siteübersicht Framework über die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Der Auftrag der Siteübersicht Provider-Klasse ist, generiert die in-Memory-Struktur, die verwendet werden, indem die `SiteMap` Klasse aus einem persistenten Datenspeicher, z. B. eine XML-Datei oder einer Datenbanktabelle. .NET Framework ausgeliefert wird, mit einem Standardwert Siteübersicht-Anbieter, der die Siteübersichtsdaten aus einer XML-Datei liest ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), und dies ist der Anbieter, die wir in diesem Lernprogramm verwenden. Einige alternativen Siteübersicht Implementierungen von Schlüsselspeicheranbietern finden Sie im Abschnitt Weitere Messwerte am Ende dieses Lernprogramms.

Der Standardanbieter Siteübersicht erwartet eine korrekt formatierte XML-Datei mit dem Namen `Web.sitemap` Stammverzeichnis vorhanden sein. Da wir diese Standardanbieter verwenden, müssen wir eine solche Datei hinzufügen und Definieren der Struktur der Siteübersicht in das entsprechende XML-Format. Fügen Sie die Datei, mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen neues Element hinzufügen. Abonnieren Sie im Dialogfeld zum Hinzufügen einer Datei vom Typ mit dem Namen Siteübersicht `Web.sitemap`.


[![Fügen Sie eine Datei namens Web.sitemap zum Stammverzeichnis des Projekts hinzu.](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer Datei mit dem Namen `Web.sitemap` zum Stammverzeichnis des Projekts ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image6.png))


Die XML-Datei definiert die Website-Struktur als Hierarchie an. Diese hierarchische Beziehung wird in der XML-Datei über den Besitzer modelliert die `<siteMapNode>` Elemente. Die `Web.sitemap` muss beginnen mit einem `<siteMap>` übergeordneten Knoten, der genau eine hat `<siteMapNode>` untergeordneten. Diesem auf oberster Ebene `<siteMapNode>` Element stellt den Stamm der Hierarchie dar, und möglicherweise eine beliebige Anzahl von untergeordneten Knoten. Jede `<siteMapNode>` -Element muss enthalten eine `title` Attribut, und kann optional enthalten `url` und `description` Attribute, u. a.; jeder nicht leeren `url` Attribut muss eindeutig sein.

Geben Sie den folgenden XML-Daten in die `Web.sitemap` Datei:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Die oben aufgeführten Website Zuordnung Markup definiert die Hierarchie, die in Abbildung 3 gezeigt.


[![Die Siteübersicht stellt eine hierarchische Navigationsstruktur](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Abbildung 3**: die Siteübersicht stellt einen hierarchischen Navigationsstruktur ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Schritt 3: Aktualisieren die Master-Seite, um eine Navigation Benutzeroberfläche einschließen

ASP.NET umfasst eine Reihe von Navigation bezogene Web Kontrollmechanismen für das Entwerfen einer Benutzeroberfläche. Dazu gehören das Menü, TreeView und SiteMapPath-Steuerelemente. Die Menü- und TreeView-Steuerelemente Rendern der Siteübersichtsstruktur in einem Menü oder einer Struktur bzw. während der SiteMapPath Breadcrumb ausgibt, die den aktuellen Knoten besuchte sowie seiner übergeordneten Elemente anzeigt. Die Siteübersichtsdaten zu anderen Daten, die mithilfe der SiteMapDataSource Websteuerelemente gebunden werden kann und programmgesteuert zugegriffen werden kann, über die `SiteMap` Klasse.

Da ist eine ausführliche Beschreibung der Siteübersicht-Framework und die Navigationssteuerelemente Gegenstand dieser Reihe von Lernprogrammen, sondern als eingesparten Zeit, die wir Erstellen von eigenen Benutzeroberfläche für die Navigation stattdessen leihen verwendet meinem  *[ Arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)*  Reihe von Lernprogrammen, die Wiederholungsmodul-Steuerelement verwendet eine zwei-Deep Aufzählung der Navigationslinks, anzeigen, wie in Abbildung 4 dargestellt.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Eine Liste zwei Ebenen von Links hinzufügen in der linken Spalte

Um diese Schnittstelle zu erstellen, fügen Sie das folgende deklarative Markup zum Rendern der `Site.master` Masterseite linken Spalte, in dem der Text TODO: Menü wird hier... sich zurzeit befindet.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Die oben genannten Markup bindet ein Repeater-Steuerelement namens `menu` mit einer SiteMapDataSource die Map-Standorthierarchie, die in definierten womit `Web.sitemap`. Seit des SiteMapDataSource Steuerelements [ `ShowStartingNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) auf "false" Zurückgeben der Siteübersicht-Hierarchie der Nachfolger des Knotens Startseite ab Beginn festgelegt ist. Repeater zeigt jeder dieser Knoten (zurzeit nur Mitgliedschaft) in ein `<li>` Element. Ein anderes, innere Repeater zeigt den aktuellen Knoten untergeordneten Elemente in einer geschachtelten ungeordnete Liste.

Abbildung 4 zeigt die oben genannten Markup gerenderte Ausgabe mit der Siteübersichtsstruktur, das in Schritt2 erstellt wurde. Die Repeater rendert herkömmlichen ungeordnete Liste Markup; die cascading Stylesheet-Regeln in definierten `Styles.css` sind verantwortlich für das Layout unauffälligen ansprechende. Eine ausführlichere Beschreibung der Funktionsweise der oben genannten Markup, finden Sie in der [Masterseiten und Websitenavigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx) Lernprogramm.


[![Der Navigations-Benutzeroberfläche wird gerendert Verwenden von geschachtelten ungeordnete Listen](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Abbildung 4**: der Navigations-Benutzeroberfläche wird gerendert Verwenden von geschachtelten ungeordnete Listen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Hinzufügen der Brotkrümelnavigation

Zusätzlich zu der Liste der Links in der linken Spalte, wir auch haben jedes angezeigte Seite eine [Breadcrumb](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Breadcrumb ist ein Navigation Benutzeroberflächenelement, das schnell Benutzer ihre aktuelle Position innerhalb der Standorthierarchie anzeigt. Das Steuerelement SiteMapPath verwendet das Framework der Siteübersicht, um die aktuelle Seite Position in der Siteübersicht zu bestimmen und zeigt dann Breadcrumb auf Grundlage dieser Informationen.

Insbesondere eine `<span>` Element auf der Masterseite Header `<div>` Element, und legen Sie den neuen `<span>` des Elements `class` -Attribut auf die Breadcrumb. (Die `Styles.css` Klasse enthält eine Regel für eine Breadcrumb-Klasse.) Als Nächstes fügen Sie eine SiteMapPath neuen `<span>` Element.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Abbildung 5 zeigt die Ausgabe der SiteMapPath aus, wenn das Unternehmen besuchende `~/Membership/CreatingUserAccounts.aspx`.


[![Die Breadcrumb-Leiste zeigt die aktuelle Seite und seiner Vorgänger auf der Website zuordnen](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Abbildung 5**: die Breadcrumb-Leiste zeigt die aktuelle Seite und seiner übergeordneten Elemente in der Siteübersicht ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Schritt 4: Entfernen der benutzerdefinierten Prinzipal und der Identität Logik

In der  *<a id="_msoanchor_7"> </a> [Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)*  Lernprogramm wurde erläutert, wie benutzerdefinierte Haupt- und Identitätsobjekte Objekte dem authentifizierten Benutzer verknüpfen. Wir erreicht dies durch Erstellen eines ereignishandlers in `Global.asax` für der Anwendungsverzeichnis `PostAuthenticateRequest` Ereignis, das ausgelöst, nachdem wird die `FormsAuthenticationModule` den Benutzer authentifiziert hat. In diesem Ereignishandler ersetzt die `GenericPrincipal` und `FormsIdentity` Objekte hinzugefügt, indem die `FormsAuthenticationModule` mit der `CustomPrincipal` und `CustomIdentity` Objekte wir in diesem Lernprogramm erstellt haben.

Während benutzerdefinierte Haupt- und Identitätsobjekte Objekte in bestimmten Szenarien, in den meisten Fällen werden die `GenericPrincipal` und `FormsIdentity` Objekte sind ausreichend. Folglich Meinung meiner nach lohnende wieder das Standardverhalten wäre. Diese Änderung vornehmen, indem Sie entweder entfernen oder Auskommentieren der `PostAuthenticateRequest` Ereignishandler oder durch Löschen der `Global.asax` Datei vollständig.

> [!NOTE]
> Sobald Sie kommentiert oder den Code in entfernt haben `Global.asax`, müssen Sie den Code in auskommentieren `Default.aspx's` Code-Behind-Klasse, die wandelt die `User.Identity` Eigenschaft, um eine `CustomIdentity` Instanz.


## <a name="step-5-programmatically-creating-a-new-user"></a>Schritt 5: Programmgesteuertes Erstellen eines neuen Benutzers

So erstellen ein neues Benutzerkonto mithilfe Framework Mitgliedschaft der `Membership` Klasse [ `CreateUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Diese Methode ist für den Benutzernamen, Kennwort und andere Felder benutzerspezifischen Eingabeparameter. Bei Aufruf, delegiert die Erstellung des neuen Benutzerkontos an den konfigurierten Mitgliedschaftsanbieter und gibt dann eine [ `MembershipUser` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) , das gerade erstellte Benutzerkonto darstellt.

Die `CreateUser` Methode verfügt über vier Überladungen, jeweils eine andere Anzahl von Eingabeparametern akzeptiert:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Diese vier Überladungen unterscheiden sich von der Menge der gesammelten Informationen. Die erste Überladung erfordert z. B. Benutzername und Kennwort für das neue Benutzerkonto an, während das zweite Argument auch die e-Mail-Adresse des Benutzers erfordert.

Diese Überladungen sind vorhanden, da die benötigten Informationen zum Erstellen eines neuen Benutzerkontos auf den Mitgliedschaftsanbieter Konfigurationseinstellungen abhängig ist. In der  *<a id="_msoanchor_8"> </a> [erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  Angabe Mitgliedschaft Konfigurationseinstellungen im untersucht Lernprogramm `Web.config`. Tabelle 2 enthalten eine vollständige Liste der Konfigurationseinstellungen.

Eine Konfiguration dieser Art Mitgliedschaft Anbieter festlegen, wirkt sich auf was `CreateUser` Überladungen können verwendet werden, ist die `requiresQuestionAndAnswer` Einstellung. Wenn `requiresQuestionAndAnswer` festgelegt ist, um `true` (Standard), und klicken Sie dann wir beim Erstellen eines neuen Benutzerkontos eine Sicherheitsfrage und eine Antwort angeben müssen. Diese Informationen werden später verwendet werden, wenn der Benutzer zurücksetzen oder sein Kennwort ändern muss. Insbesondere zu diesem Zeitpunkt werden sie die Sicherheitsfrage angezeigt, und sie müssen die richtige Antwort zum Zurücksetzen oder ändern ihr Kennwort eingeben. Daher, wenn die `requiresQuestionAndAnswer` auf festgelegt ist `true` und dann entweder die ersten beiden Aufrufen `CreateUser` Überladungen zu einer Ausnahme, da die Sicherheitsfrage und-Antwort fehlen. Da unsere Anwendung derzeit konfiguriert ist, um eine Sicherheitsfrage und eine Antwort erforderlich sind, müssen wir eine der letzten zwei Überladungen für die Erstellung des Benutzers programmgesteuert verwenden.

Um zu veranschaulichen, mit der `CreateUser` -Methode, erstellen wir eine Benutzeroberfläche, in denen wir den Benutzer ihren Namen, Kennwörter, e-Mail und eine Antwort auf eine vordefinierte Sicherheitsfrage auffordern. Öffnen der `CreatingUserAccounts.aspx` auf der Seite der `Membership` Ordner, und fügen Sie die folgenden Web-Steuerelementen an das Inhaltssteuerelement hinzu:

- Ein Textfeld mit dem Namen`Username`
- Ein Textfeld mit dem Namen `Password`, dessen `TextMode` Eigenschaft auf festgelegt ist`Password`
- Ein Textfeld mit dem Namen`Email`
- Eine Bezeichnung mit dem Namen `SecurityQuestion` mit seiner `Text` Eigenschaft gelöscht
- Ein Textfeld mit dem Namen`SecurityAnswer`
- Eine Schaltfläche mit dem Namen `CreateAccountButton` , deren `Text` Eigenschaft so erstellen Sie das Benutzerkonto festgelegt ist
- Ein Bezeichnungsfeld-Steuerelement mit dem Namen `CreateAccountResults` mit seiner `Text` Eigenschaft gelöscht

An diesem Punkt sollte der Bildschirm in Abbildung 6 gezeigt Screenshot ähneln.


[![Hinzufügen von den verschiedenen Websteuerelementen auf der Seite "CreatingUserAccounts.aspx"](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Abbildung 6**: Fügen Sie die verschiedenen Web-Steuerelemente, die `CreatingUserAccounts.aspx Page` ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image18.png))


Die `SecurityQuestion` Bezeichnung und `SecurityAnswer` Textfeld sollen eine vordefinierte Sicherheitsfrage anzuzeigen und die Antwort des Benutzers zu erfassen. Beachten Sie, dass die Sicherheitsfrage und die Antwort auf Grundlage einer vom Benutzer gespeichert werden, damit es möglich ist, können für jeden Benutzer ihre eigenen Sicherheitsfrage definieren. Allerdings für dieses Beispiel, das ich haben sich entschieden, eine universal Sicherheitsfrage, nämlich verwenden: was Ihre Lieblingsfarbe ist?

Wenn diese vordefinierten Sicherheitsfrage implementieren zu können, fügen Sie eine Konstante auf der Seite Code-Behind-Klasse, die mit dem Namen `passwordQuestion`, die Sicherheitsfrage zuweisen. Klicken Sie auf die `Page_Load` -Ereignishandler, weisen Sie diese Konstante, um die `SecurityQuestion` Bezeichnungsfelds `Text` Eigenschaft:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Als Nächstes erstellen Sie einen Ereignishandler für das `CreateAccountButton'` s `Click` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Die `Click` Ereignishandler beginnt mit dem Definieren einer Variablen mit dem Namen `createStatus` des Typs [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus`ist eine Enumeration, die den Status der gibt an, die `CreateUser` Vorgang. Angenommen, wenn das resultierende das Benutzerkonto erfolgreich erstellt wurde `MembershipCreateStatus` -Instanzensatz auf einen Wert von `Success;` andererseits, wenn der Vorgang fehlschlägt, da bereits einen Benutzer mit dem gleichen Benutzernamen vorhanden ist, wird festgelegt auf einen Wert von `DuplicateUserName`. In der `CreateUser` Überladung, die wir verwenden, müssen wir übergeben einer `MembershipCreateStatus` -Instanz in der Methode. Dieser Parameter festgelegt ist, auf den entsprechenden Wert in der `CreateUser` -Methode, und es untersuchen den Wert nach dem Aufruf der Methode, um zu bestimmen, ob das Benutzerkonto erfolgreich erstellt wurde.

Nach dem Aufruf `CreateUser`, und übergeben Sie `createStatus`, `Select Case` Anweisung wird verwendet, um die Ausgabe einer entsprechenden Meldung abhängig von den zugewiesenen Wert `createStatus`. Abbildung 7 zeigt die Ausgabe an, wenn ein neuer Benutzer erfolgreich erstellt wurde. Abbildungen 8 und 9 zeigen die Ausgabe auf, wenn das Benutzerkonto nicht erstellt wurde. In Abbildung 8: eingegeben Besucher eine fünfstellige ein Kennwort nicht die Stärke, wie folgt buchstabiert in der Mitgliedschaftsanbieter Konfigurationseinstellungen kennwortanforderungen erfüllt. In Abbildung 9: versucht der Besucher, erstellen Sie ein Benutzerkonto mit einem vorhandenen Benutzernamen (die gerade erstellt, die in Abbildung 7).


[![Ein neues Benutzerkonto wurde erfolgreich erstellt](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Abbildung 7**: ein neues Benutzerkonto wurde erfolgreich erstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image21.png))


[![Das Benutzerkonto wird nicht erstellt werden, weil das angegebene Kennwort zu schwach ist](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Abbildung 8**: das Benutzerkonto wird nicht erstellt werden, weil das angegebene Kennwort zu schwach ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image24.png))


[![Das Benutzerkonto ist, dass nicht erstellt, da der Benutzername wird bereits verwendet](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Abbildung 9**: das Benutzerkonto ist nicht erstellt, da der Benutzername wird bereits verwendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> Möglicherweise Fragen Sie zum Erfolg oder Fehler feststellen, bei Verwendung einer der ersten beiden `CreateUser` methodenüberladungen, weder besitzt einen Parameter vom Typ `MembershipCreateStatus`. Diese ersten beiden Überladungen lösen eine [ `MembershipCreateUserException` Ausnahme](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) im Fehlerfall, darunter eine [ `StatusCode` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) vom Typ `MembershipCreateStatus`.


Nachdem einige Benutzerkonten erstellt haben, stellen Sie sicher, dass die Konten erstellt wurden, durch die Auflistung der Verzeichnisinhalte der `aspnet_Users` und `aspnet_Membership` Tabellen in der `SecurityTutorials.mdf` Datenbank. Wie in Abbildung 10 gezeigt, hinzugefügte ich zwei Benutzer über die `CreatingUserAccounts.aspx` Seite: Tito und Bruce.


[![Es gibt zwei Benutzer im Benutzerspeicher Mitgliedschaft: Tito und Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Abbildung 10**: Es gibt zwei Benutzer im Benutzerspeicher Mitgliedschaft: Tito und Bruce ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image30.png))


Während der Mitgliedschaft Benutzerspeicher jetzt Bruce und des Tito Kontoinformationen enthält, müssen wir noch Funktionalität zu implementieren, die Bruce oder Tito melden Sie sich an den Standort zulässt. Derzeit `Login.aspx` überprüft die Anmeldeinformationen des Benutzers anhand eines Satzes hartcodierten Benutzernamen/Kennwort-Paare - dies der Fall ist *nicht* überprüft die angegebenen Anmeldeinformationen für das Framework Mitgliedschaft. Sehen Sie jetzt neue Benutzerkonten in der `aspnet_Users` und `aspnet_Membership` Tabellen müssen ausreichen. In den nächsten Lernprogrammen  *<a id="_msoanchor_9"> </a> [überprüft die Benutzer Anmeldeinformationen für die Mitgliedschaft Benutzer speichern](validating-user-credentials-against-the-membership-user-store-vb.md)*, aktualisieren wir die Anmeldeseite für den Speicher für die Mitgliedschaft überprüft.

> [!NOTE]
> Wenn Sie keinen Benutzern angezeigt werden Ihre `SecurityTutorials.mdf` Datenbank möglicherweise daran, dass die Webanwendung den Standardmitgliedschaftsanbieter verwendet wird `AspNetSqlMembershipProvider`, verwendet der `ASPNETDB.mdf` Datenbank als seine Benutzerspeicher. Um festzustellen, ob dies der Fall ist, klicken Sie auf die Schaltfläche "Aktualisieren" im Projektmappen-Explorer. Wenn eine Datenbank mit dem Namen `ASPNETDB.mdf` wurde hinzugefügt, um die `App_Data` Ordner, dies ist das Problem. Zurück zu Schritt 4 der  *<a id="_msoanchor_10"> </a> [erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)*  Tutorial Anleitungen zum ordnungsgemäßen Konfigurieren der Mitgliedschaftsanbieter.


Erstellen Sie in den meisten Benutzer Konto Szenarien, die Besucher erhält eine Oberfläche, geben ihren Benutzernamen, Kennwörter, e-Mail und andere wichtige Informationen, die an diesem Punkt ein neues Konto erstellt wird. In diesem Schritt wird erläutert, erstellen eine solche Schnittstelle per hand katalogisiert wird und dann wurde erläutert, wie mithilfe der `Membership.CreateUser` Methode, um das neue Benutzerkonto programmgesteuert hinzufügen auf der Grundlage von Eingaben des Benutzers. Getesteten Codes erstellt jedoch nur das neue Benutzerkonto an. Es nachverfolgung Aktionen, wie der Benutzer an den Standort unter dem gerade erstellten Benutzerkonto anmelden oder senden eine e-Mail zur kaufbestätigung, für den Benutzer nicht ausgeführt werden. Diese zusätzlichen Schritte, müsste zusätzlichen Code auf der Schaltfläche `Click` -Ereignishandler.

ASP.NET umfasst das CreateUserWizard-Steuerelement, das entworfen wurde, behandelt die Benutzer-Konto-Erstellungsprozess Rendering eine Benutzeroberfläche zum Erstellen neuer Benutzerkonten, erstellen das Konto in die Mitgliedschaft-Framework und nach dem Konto ausführen Erstellungsaufgaben wie das Senden einer e-Mails zur kaufbestätigung und den gerade erstellten Benutzer bei der Website anzumelden. Mit dem CreateUserWizard-Steuerelement ist so einfach wie das Steuerelement CreateUserWizard aus der Toolbox auf eine Seite ziehen und dann einige Eigenschaften festlegen. In den meisten Fällen müssen Sie wird keine einzige Codezeile schreiben. Wir untersuchen diese raffinierte Steuerelement im Detail in Schritt 6.

Wenn neue Benutzerkonten nur über eine typische Konto erstellen Webseite erstellt werden, ist es unwahrscheinlich, dass Sie Code schreiben jemals, verwendet der `CreateUser` -Methode, wie das Steuerelement CreateUserWizard wird wahrscheinlich Ihren Anforderungen entsprechen. Allerdings die `CreateUser` Methode ist nützlich in Szenarien, in dem Sie eine stark angepasste Benutzeroberfläche Konto erstellen oder wenn Sie neue Benutzerkonten über eine alternative Benutzeroberfläche programmgesteuert erstellen müssen. Sie können z. B. eine Seite erforderlich, mit dem Benutzer, eine XML-Datei hochzuladen, die Benutzerinformationen aus einer anderen Anwendung enthält. Die Seite möglicherweise analysiert den Inhalt des hochgeladene XML-Datei, und erstellen ein neues Konto für jeden Benutzer dargestellt in der XML-Code durch Aufrufen der `CreateUser` Methode.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Schritt 6: Erstellen eines neuen Benutzers mit dem CreateUserWizard-Steuerelement

Im Lieferumfang von ASP.NET sind einer Reihe von Kontrollmechanismen Anmeldung Web. Diese Steuerelemente helfen, viele allgemeine Konto und Login-bezogenen Szenarien. Die [Steuerelement CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) ist ein solches Steuerelement, die entwickelt wird, um eine Benutzeroberfläche zum Hinzufügen eines neuen Benutzerkontos Mitgliedschaft Framework.

Wie viele andere Web Anmeldung bezogene Steuerelemente kann die CreateUserWizard verwendet werden, ohne eine einzige Codezeile schreiben zu müssen. Es intuitiv bietet eine Benutzeroberfläche, die auf Grundlage des Mitgliedschaftsanbieters Konfigurationseinstellungen und intern Aufrufe der `Membership` Klasse `CreateUser` -Methode auf, nachdem der Benutzer gibt die erforderliche Informationen, und klickt auf die Schaltfläche "Create User". Das Steuerelement CreateUserWizard ist äußerst anpassbare. Es gibt ein Host von Ereignissen, die in verschiedenen Phasen des Erstellungsprozesses des Kontos auslösen. Wir können je nach Bedarf benutzerdefinierten Logik in den erstellungsworkflow für Konto einfügen Ereignishandler erstellen. Darüber hinaus ist die CreateUserWizard Darstellung sehr flexibel. Es gibt eine Reihe von Eigenschaften, die die Darstellung der Benutzeroberfläche zu definieren. Falls notwendig, das Steuerelement zu einer Vorlage konvertiert werden kann, oder Registrierungsschritten für zusätzliche Benutzer hinzugefügt werden.

Beginnen wir mit einer Einblick in die Verwendung von Standardschnittstelle und das Verhalten des CreateUserWizard-Steuerelements. Gewusst wie: Anpassen der Darstellung über Eigenschaften und Ereignisse des Steuerelements wird dann untersucht.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Untersuchen der CreateUserWizard Standardschnittstelle und Verhalten

Zurück zu den `CreatingUserAccounts.aspx` auf der Seite der `Membership` Ordner, für den Entwurf oder Split-Modus wechseln, und fügen Sie dann ein CreateUserWizard-Steuerelement am oberen Rand der Seite. Das Steuerelement CreateUserWizard unter der Toolbox-Steuerelemente anmeldeabschnitt gespeichert. Legen Sie nach dem Hinzufügen des Steuerelements an, dessen `ID` Eigenschaft `RegisterUser`. Wie die Abbildung in Abbildung 11 zeigt rendert die CreateUserWizard eine Schnittstelle mit Textfeldern für des neuen Benutzers Benutzernamen, Kennwort, e-Mail-Adresse und Sicherheitsfrage und Antwort an.


[![Das Steuerelement CreateUserWizard rendert eine generische erstellen Benutzeroberfläche](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Abbildung 11**: das Steuerelement CreateUserWizard rendert eine generische Benutzeroberfläche erstellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image33.png))


Werfen Sie einen Moment Zeit, vergleichen die Standardbenutzeroberfläche generiert, die vom Steuerelement CreateUserWizard mit der Schnittstelle, die in Schritt 5 erstellt haben. Zunächst einmal ermöglicht Steuerelement CreateUserWizard den Besucher der Sicherheitsfrage und die Antwort an, während die unserer manuell erstellter-Schnittstelle eine vordefinierte Sicherheitsfrage verwendet. Die CreateUserWizard Steuerelementschnittstellen enthält auch Validierungssteuerelemente, während mussten wir noch die Validierung auf Formularfelder unsere Schnittstelle implementiert. Und die CreateUserWizard-Schnittstelle umfasst ein Textfeld "Kennwort bestätigen" (zusammen mit einem CompareValidator, um sicherzustellen, dass der Text, das Kennwort eingegeben und Kennwort vergleichen Textfelder gleich sind).

Interessant ist, dass das Steuerelement CreateUserWizard Konfigurationseinstellungen der Mitgliedschaftsanbieter beim Rendern der Benutzeroberfläche fragt. Z. B. die Textfelder ein Sicherheit Frage und Antwort werden nur angezeigt, wenn `requiresQuestionAndAnswer` auf "true" festgelegt ist. Ebenso fügt CreateUserWizard automatisch eine RegularExpressionValidator-Steuerelement, um sicherzustellen, dass die Stärke kennwortanforderungen erfüllt sind, und legt sie fest seine `ErrorMessage` und `ValidationExpression` Eigenschaften basierend auf den `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`, und `passwordStrengthRegularExpression` Konfigurationseinstellungen.

Das Steuerelement CreateUserWizard wie der Name schon sagt, stammt aus dem [Assistenten Steuerelement](https://msdn.microsoft.com/library/s2etd1ek.aspx). Assistentensteuerelemente sind dafür entwickelt, eine Schnittstelle für das Abschließen von Aufgaben mit mehreren Schritten bereitstellen. Ein Assistent Steuerelement möglicherweise eine beliebige Anzahl von `WizardSteps`, von denen jeder eine Vorlage, die den HTML-Code definiert ist und Websteuerelemente für diesen Schritt. Das Assistenten-Steuerelement zeigt zunächst die erste `WizardStep`, zusammen mit Navigationssteuerelemente, mit denen den Benutzer von einer Stufe zur nächsten zu fortfahren oder zu den vorherigen Schritten zurückzugeben.

Wie die deklarative Markup in Abbildung 11 zeigt, das CreateUserWizard Steuerelement Standardschnittstelle enthält zwei `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Rendert die Schnittstelle zum Sammeln von Informationen zum Erstellen des neuen Benutzerkontos. Dies ist der Schritt in der Abbildung 11 gezeigt.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? Rendert eine Meldung, dass das Konto wurde erfolgreich erstellt.

Die CreateUserWizard Aussehen und Verhalten können geändert werden, indem konvertieren eine der folgenden Schritte aus, in Vorlagen, oder eigene `WizardStep` s. Betrachten wir Hinzufügen einer `WizardStep` auf die Registrierung-Schnittstelle in der *zusätzliche Benutzerinformationen speichern* Lernprogramm.

Betrachten Sie das Steuerelement CreateUserWizard in Aktion an. Besuchen Sie die `CreatingUserAccounts.aspx` Seite über einen Browser. Indem Sie Sie in der CreateUserWizard Schnittstelle einige ungültige Werte eingeben starten. Versuchen Sie es durch Eingabe eines Kennworts, die nicht entsprechen, die Stärke kennwortanforderungen oder den Benutzernamen ein Textfeld leer bleibt. Die CreateUserWizard wird eine entsprechende Fehlermeldung angezeigt. Abbildung 12 zeigt die Ausgabe, bei dem Versuch, einen Benutzer mit einem unzureichend sicheres Kennwort zu erstellen.


[![Die CreateUserWizard fügt automatisch Validierungssteuerelemente](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Abbildung 12**: die CreateUserWizard automatisch injiziert Validierungssteuerelemente ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image36.png))


Als Nächstes geben Sie entsprechende Werte in der CreateUserWizard, und klicken Sie auf die Schaltfläche "Create User". Vorausgesetzt, die erforderlichen Felder eingegeben wurden, und das Kennwort Stärke ist ausreichend, die CreateUserWizard erstellt ein neues Benutzerkonto über die Mitgliedschaft-Framework und zeigt dann die `CompleteWizardStep`der Schnittstelle (siehe Abbildung 13). Im Hintergrund der CreateUserWizard Ruft die `Membership.CreateUser` -Methode, genau wie in Schritt 5 wir haben.


[![Ein neues Benutzerkonto wurde erfolgreich erstellt wurde](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Abbildung 13**: ein neues Benutzerkonto wurde erfolgreich erstellt wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Wie in Abbildung 13 gezeigt, die `CompleteWizardStep`der Schnittstelle enthält eine Schaltfläche "Weiter". Allerdings führt die auf sie zu diesem Zeitpunkt nur ein Postback handeln, den Besucher auf derselben Seite verlassen. In dem Anpassen der CreateUserWizard Aussehen und Verhalten über seine Eigenschaften im Abschnitt wir wird untersucht, wie Sie diese Schaltfläche, die den Besucher senden lassen können `Default.aspx` (oder einige Seite ").


Nach dem Erstellen eines neuen Benutzerkontos, kehren Sie zu Visual Studio zurück, und untersuchen Sie die `aspnet_Users` und `aspnet_Membership` Tabellen wie in Abbildung 10, um sicherzustellen, dass das Konto wurde erfolgreich erstellt wurde.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Anpassen der CreateUserWizard Verhalten und Aussehen über seine Eigenschaften

In einer Vielzahl von Möglichkeiten, über Eigenschaften, die CreateUserWizard angepasst werden `WizardStep` s und Ereignishandler. In diesem Abschnitt untersuchen wir die Darstellung des Steuerelements über seine Eigenschaften anpassen; Erweitern des Verhaltens des Steuerelements über Ereignishandler ist der nächste Abschnitt untersucht.

Praktisch aller im Standardbenutzeroberfläche CreateUserWizard-Steuerelement angezeigte Text kann über ihre Fülle an Eigenschaften angepasst werden. Beispielsweise können der Benutzername, Kennwort, Kennwort bestätigen, E-mail, Sicherheitsfrage und Sicherheitsantwort Bezeichnungen auf der linken Seite der Textfelder angepasst werden, durch die [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), und [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) Eigenschaften, bzw. Ebenso, stehen die Eigenschaften zum Angeben des Texts für die Create User und Fortfahren-Schaltflächen in der `CreateUserWizardStep` und `CompleteWizardStep`, auch als ob diese Schaltflächen wie Schaltflächen, LinkButtons oder ImageButtons gerendert werden.

Die Farben, Rahmen, Schriftarten und andere visuelle Elemente werden über einen Host der Formateigenschaften konfigurierbar. CreateUserWizard-Steuerelement selbst verfügt über die allgemeine Web Control Stileigenschaften - `BackColor`, `BorderStyle`, `CssClass`, `Font`usw. - und stehen eine Reihe von Stileigenschaften für definieren die Darstellung für bestimmte Bereiche der CreateUserWizard Schnittstelle. Die [ `TextBoxStyle` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), definiert beispielsweise die Formate für die Textfelder ein, in der `CreateUserWizardStep`, während die [ `TitleTextStyle` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definiert das Format für den Titel (Registrieren für Ihre neue Konto).

Zusätzlich zu den Eigenschaften darstellungsbezogene stehen Ihnen eine Reihe von Eigenschaften, die das Verhalten der CreateUserWizard-Steuerelements beeinflussen. Die [ `DisplayCancelButton` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), legen Sie auf "true" zeigt eine Schaltfläche "Abbrechen" neben der Schaltfläche "Create User" (der Standardwert ist "false"). Wenn Sie die Schaltfläche "Abbrechen" anzeigen, müssen Sie auch festlegen, die [ `CancelDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), die angibt, dass der Seite wird der Benutzer nach dem Klicken auf "Abbrechen" an. Wie im vorherigen Abschnitt, in der Schaltfläche "Weiter" erwähnt die `CompleteWizardStep`Schnittstelle einen Postback verursacht, jedoch lässt den Besucher auf derselben Seite. Um den Besucher zu einigen anderen Seite senden nach dem Klicken auf die Schaltfläche "Weiter", geben Sie einfach die URL in die [ `ContinueDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Wir aktualisieren die `RegisterUser` CreateUserWizard-Steuerelement, um eine Schaltfläche "Abbrechen" anzeigen und den Besucher senden `Default.aspx` Wenn die Schaltflächen Abbrechen oder fortsetzen geklickt wird. Um dies zu erreichen, legen Sie die `DisplayCancelButton` -Eigenschaft auf "true", und beide die `CancelDestinationPageUrl` und `ContinueDestinationPageUrl` Eigenschaften ~ / "default.aspx". Abbildung 14 zeigt das aktualisierte CreateUserWizard, wenn Sie über einen Browser angezeigt.


[![Die CreateUserWizardStep enthält eine Schaltfläche "Abbrechen"](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Abbildung 14**: die `CreateUserWizardStep` enthält eine Schaltfläche "Abbrechen" ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image42.png))


Wenn ein Besucher, Benutzername, Kennwort, e-Mail-Adresse und Sicherheitsfrage und Antwort eingibt und klickt auf die Create User, ein neues Benutzerkonto wird erstellt, und der Besucher als dieser neu erstellten Benutzer angemeldet ist. Vorausgesetzt, dass die Person, die auf der Seite "ein neues Konto für sich selbst erstellt wird, ist dies wahrscheinlich das gewünschte Verhalten. Allerdings sollten Sie Administratoren ermöglichen, fügen Sie neue Benutzerkonten hinzu. In diesem Fall würde das Benutzerkonto, das erstellt werden, aber der Administrator bleibt angemeldet, als Administrator (und nicht als das neu erstellte Konto). Dieses Verhalten kann geändert werden, bis der boolesche Wert [ `LoginCreatedUser` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Benutzerkonten in die Mitgliedschaft Framework enthalten eine genehmigte Flag; Benutzer, die nicht genehmigte sind für die Anmeldung bei der Website ist nicht möglich. Standardmäßig ist ein neu erstelltes Konto als genehmigt, sodass der Benutzer sofort bei der Website anmelden, markiert. Es ist jedoch möglich, neue Benutzerkonten, die als nicht genehmigten markiert haben. Vielleicht möchten Sie ein Administrator neue Benutzer manuell genehmigen müssen, bevor sie sich anmelden können; oder vielleicht stellen Sie sicher, dass bei der Registrierung eingegebene e-Mail-Adresse gültig ist, bevor die Übergabe von eines Benutzers, sich anzumelden. Was der Fall sein kann, können Sie das neu erstellte Benutzerkonto als nicht genehmigten durch Festlegen des Steuerelements CreateUserWizard gekennzeichnet haben [ `DisableCreatedUser` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) auf "true" (der Standardwert ist "false").

Andere Verhalten-bezogene Eigenschaften Hinweis gehören `AutoGeneratePassword` und `MailDefinition`. Wenn die [ `AutoGeneratePassword` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) auf "true" festgelegt ist die `CreateUserWizardStep` wird nicht angezeigt, die Textfelder Kennwort und Kennwort bestätigen ein; stattdessen das Kennwort des neu erstellten Benutzers wird automatisch generiert mithilfe der `Membership` Klasse [ `GeneratePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Die `GeneratePassword` -Methode erstellt ein Kennwort mit einer angegebenen Länge und mit einer ausreichenden Anzahl von nicht-alphanumerische Zeichen, das Kennwort Stärke Anforderungen zu erfüllen.

Die [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) ist hilfreich, wenn Sie eine e-Mail an die während des Erstellungsprozesses Konto angegebene e-Mail-Adresse senden möchten. Die `MailDefinition` Eigenschaft enthält eine Reihe von untergeordneten Eigenschaften zum Definieren von Informationen zu den erstellten e-Mail-Nachricht. Diese untergeordneten Eigenschaften gehören Optionen wie `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, und `BodyFileName`. Die [ `BodyFileName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) verweist auf eine Text- oder HTML-Datei, die den Textkörper der e-Mail-Nachricht enthält. Der Text unterstützt zwei vordefinierte Platzhalter: `<%UserName%>` und `<%Password%>`. Dieser Platzhalter, falls vorhanden, in der `BodyFileName` Datei, die mit Namen und Kennwort des soeben erstellten Benutzers ersetzt werden.

> [!NOTE]
> Die `CreateUserWizard` des Steuerelements `MailDefinition` Eigenschaft gibt nur Informationen zu den e-Mail-Nachricht, die gesendet wird, wenn ein neues Konto erstellt wird. Es umfasst nicht die Details auf wie die e-Mail-Nachricht tatsächlich gesendet wird (d. h., ob ein SMTP-Server oder Mailablage-Verzeichnis verwendet wird, Authentifizierungsinformationen usw.). Diese Details auf niedriger Ebene definiert werden müssen die `<system.net>` im Abschnitt `Web.config`. Weitere Informationen zu diesen Konfigurationseinstellungen und zum Senden von e-Mails von ASP.NET 2.0 im Allgemeinen finden Sie in der [häufig gestellte Fragen zur SystemNetMail.com](http://www.systemnetmail.com/) und Meine Artikel [Senden von E-Mails in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Erweitern des Verhaltens der CreateUserWizard mithilfe von Ereignishandlern

Das Steuerelement CreateUserWizard löst eine Anzahl von Ereignissen während des Workflows aus. Nachdem ein Besucher, ihren Benutzernamen, Kennwort und andere relevante Informationen eingibt, und klickt auf die Schaltfläche "Create User", Steuerelement CreateUserWizard so löst z. B. seine [ `CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Wenn es ein Problem während des Erstellungsvorgangs liegt der [ `CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst wird; jedoch, wenn der Benutzer erfolgreich erstellt wurde, wird das [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst wird. Es gibt zusätzliche CreateUserWizard-Steuerelement-Ereignisse, die ausgelöst wird, erhalten, aber dies sind die drei am häufigsten von Belang.

In bestimmten Szenarien können wir in den Workflow CreateUserWizard Tippen Sie auf die wir tun können, indem Sie einen Ereignishandler für das entsprechende Ereignis erstellen möchten. Um dies zu veranschaulichen, sehen wir verbessern die `RegisterUser` CreateUserWizard-Steuerelement eine benutzerdefinierte Validierung für den Benutzernamen und das Kennwort enthalten. Wir erweitern insbesondere unsere CreateUserWizard, damit Benutzernamen darf keine führende oder nachgestellte Leerzeichen enthalten und der Benutzername kann nicht an einer beliebigen Stelle im Kennwort angezeigt. Kurz gesagt, möchten wir zu verhindern, dass eine Person, die aus einen Benutzernamen wie "Scott" erstellen oder eine Kombination Benutzername/Kennwort wie Scott und Scott.1234 müssen.

Zu diesem Zweck wir einen Ereignishandler für erstellen das `CreatingUser` Ereignis, um unsere zusätzliche Überprüfungen auszuführen. Wenn die bereitgestellten Daten ungültig sind muss der Erstellungsvorgang für "Abbrechen". Außerdem müssen wir ein Bezeichnung-Websteuerelement hinzufügen, zur Seite, um die Anzeige einer Meldung angezeigt, dass der Benutzername oder Kennwort ungültig ist. Starten, indem Sie ein Bezeichnungsfeld-Steuerelement unterhalb des Steuerelements CreateUserWizard Festlegen seiner `ID` Eigenschaft, um `InvalidUserNameOrPasswordMessage` und seine `ForeColor` Eigenschaft `Red`. Löschen der `Text` Eigenschaft, und legen seine `EnableViewState` und `Visible` Eigenschaften auf "false".

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für des CreateUserWizard Steuerelements `CreatingUser` Ereignis. Um einen Ereignishandler zu erstellen, wählen Sie das Steuerelement im Designer, und wechseln Sie dann auf Eigenschaftenfenster. Klicken Sie auf das Blitzsymbol, und doppelklicken Sie dann auf das entsprechende Ereignis, um den Ereignishandler zu erstellen, von dort aus.

Fügen Sie dem `CreatingUser`-Ereignishandler folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Beachten Sie, dass Benutzername und Kennwort eingegeben werden, in das Steuerelement CreateUserWizard über seine [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) und [ `Password` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)bzw. Wir verwenden diese Eigenschaften in der oben genannten Ereignishandler, um zu bestimmen, ob der angegebene Benutzername führende oder nachgestellte Leerzeichen enthält, und gibt an, ob der Benutzername in das Kennwort enthalten ist. Wenn eine dieser Bedingungen erfüllt ist, wird eine Fehlermeldung angezeigt, der `InvalidUserNameOrPasswordMessage` Bezeichnung und des ereignishandlers `e.Cancel` -Eigenschaftensatz auf `True`. Wenn `e.Cancel` festgelegt ist, um `True`, die CreateUserWizard kurzgeschlossen wird der Workflow Abbrechen effektiv den Prozess zum Erstellen der Konto des Benutzers.

Abbildung 15 zeigt einen Screenshot der `CreatingUserAccounts.aspx` bei der Eingabe eines Benutzernamens mit führenden Leerzeichen.


[![Benutzernamen bei führende oder nachgestellte Leerzeichen sind nicht zulässig.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Abbildung 15**: Benutzernamen bei führende oder nachgestellte Leerzeichen sind nicht zulässig ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> Sehen wir ein Beispiel der Verwendung des Steuerelements CreateUserWizard `CreatedUser` Ereignis in der  *<a id="_msoanchor_11"> </a> [zusätzliche Benutzerinformationen speichern](storing-additional-user-information-vb.md)*  Lernprogramm.


## <a name="summary"></a>Zusammenfassung

Die `Membership` Klasse `CreateUser` Methode erstellt ein neues Benutzerkonto in der Mitgliedschaft-Framework. Dies erfolgt durch den Aufruf an den konfigurierten Mitgliedschaftsanbieter delegieren. Im Fall von der `SqlMembershipProvider`, `CreateUser` Methode fügt einen Datensatz an die `aspnet_Users` und `aspnet_Membership` -Datenbanktabellen.

Während neue Benutzerkonten erstellt werden können, programmgesteuert (wie in Schritt 5 veranschaulichte Filtermenü), ist ein schneller und einfacher Ansatz, die CreateUserWizard-Steuerelement zu verwenden. Dieses Steuerelement wird eine mehrstufiges Benutzeroberfläche für das Sammeln von Benutzerinformationen und erstellen einen neuen Benutzer in die Mitgliedschaft-Framework gerendert. Dieses Steuerelement im Hintergrund verwendet die gleiche `Membership.CreateUser` Methode, wie in Schritt 5, aber das Steuerelement überprüft, erstellt die Benutzeroberfläche, die Validierungssteuerelemente, und antwortet auf Benutzer Erstellung Kontofehler ohne jeglichen Code schreiben zu müssen.

Zu diesem Zeitpunkt haben wir die Funktionalität in können Sie neue Benutzerkonten erstellen. Die Anmeldeseite wird jedoch weiterhin anhand dieser Anmeldeinformationen hartcodierte überprüft zurück in die zweite Lernprogramm angegebenen. In der <a id="_msoanchor_12"> </a> [nächsten Lernprogramm](validating-user-credentials-against-the-membership-user-store-vb.md) aktualisieren wir `Login.aspx` zur Überprüfung des Benutzers die Anmeldeinformationen für das Framework Mitgliedschaft angegeben.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [`CreateUser`Technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Übersicht über CreateUserWizard-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Erstellen eine Datei systembasierten Standort Kartenanbieter](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Erstellen eine schrittweise Benutzeroberfläche mit dem ASP.NET 2.0-Assistent-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Untersuchen ASP.NET 2.0 der Websitenavigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Masterseiten und Websitenavigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Der SQL-Site-Zuordnung-Anbieter haben Sie gewartet](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Zurück](creating-the-membership-schema-in-sql-server-vb.md)
[Weiter](validating-user-credentials-against-the-membership-user-store-vb.md)
