---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
title: Erstellen von Benutzerkonten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird erläutert, mit, dass das mitgliedschaftsframework (über die SqlMembershipProvider) neue Benutzerkonten erstellen. Wir sehen uns neu zu erstellen...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 9ef3e893-bebe-4b13-9fe5-8b71720dd85e
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: fe5e55df3fa9f65a94199c2064a785255f231537
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815342"
---
<a name="creating-user-accounts-vb"></a>Erstellen von Benutzerkonten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_vb.pdf)

> In diesem Tutorial wird erläutert, mit, dass das mitgliedschaftsframework (über die SqlMembershipProvider) neue Benutzerkonten erstellen. Wir sehen wie neue Benutzer programmgesteuert und über ASP erstellt. NET integrierten Steuerelements CreateUserWizard.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [vorherigen Lernprogramm](creating-the-membership-schema-in-sql-server-vb.md) wir das Schema der Anwendung Dienste in einer Datenbank, die die Tabellen, Sichten, hinzugefügt und gespeicherte Prozeduren, die erforderlich sind, indem Sie installiert die `SqlMembershipProvider` und `SqlRoleProvider`. Dies erstellt die Infrastruktur, die wir für den Rest der Lernprogramme in dieser Reihe benötigen. In diesem Lernprogramm wird untersucht, mit dem Framework für die Mitgliedschaft (über die `SqlMembershipProvider`) um neue Benutzerkonten erstellen. Wir sehen wie neue Benutzer programmgesteuert und über ASP erstellt. NET integrierten Steuerelements CreateUserWizard.

Zusätzlich zu wissen, wie Sie neue Benutzerkonten erstellen, es müssen auch die Demo-Website aktualisieren wir zuerst erstellt, in haben der *<a id="_msoanchor_2"></a>[eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)* Lernprogramm, und klicken Sie dann in verbessert die *<a id="_msoanchor_3"></a>[Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* Lernprogramm. Unsere Demo-Webanwendung verfügt über eine Anmeldeseite angezeigt, die Anmeldeinformationen von Benutzern mit hartcodierten Benutzernamen/Kennwort-Paare überprüft. Darüber hinaus `Global.asax` enthält Code, der benutzerdefinierte erstellt `IPrincipal` und `IIdentity` Objekte für authentifizierte Benutzer. Die Anmeldeseite, um die Anmeldeinformationen für das Framework für die Mitgliedschaft von Benutzern zu überprüfen und entfernen Sie die benutzerdefinierte Logik für Haupt- und Identitätsobjekte wird aktualisiert.

Fangen wir an!

## <a name="the-forms-authentication-and-membership-checklist"></a>Die Formularauthentifizierung und die Checkliste der Mitgliedschaft

Bevor wir mit der das mitgliedschaftsframework beginnen, werfen wir einen Moment Zeit, um die wichtigsten Schritte zu überprüfen, die wir erstellt haben, um dieser Punkt erreicht wird. Bei Verwendung der mitgliedschaftsframework, mit der `SqlMembershipProvider` in einem Szenario mit Forms basierende Authentifizierung die folgenden Schritte müssen ausgeführt werden, vor der Implementierung der Mitgliedschaftsfunktion in Ihrer Webanwendung:

1. **Aktivieren Sie die formularbasierte Authentifizierung.** Wie in erläutert *<a id="_msoanchor_4"></a>[eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)*, Formularauthentifizierung wird aktiviert, indem Sie die Bearbeitung `Web.config` verwendet wird und die `<authentication>` Elements `mode` -Attribut `Forms`. Bei der Formularauthentifizierung aktiviert wird untersucht jede eingehende Anforderung für eine *forms-Authentifizierungsticket*, falls vorhanden, identifiziert den anforderer.
2. **Fügen Sie das Dienstschema für die Anwendung an die entsprechende Datenbank hinzu.** Bei Verwendung der `SqlMembershipProvider` wir müssen das Schema der Anwendung Dienste in einer Datenbank zu installieren. In der Regel wird dieses Schema auf dieselbe Datenbank hinzugefügt, das Datenmodell der Anwendung enthält. Die *<a id="_msoanchor_5"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Lernprogramm erläutert, mit der `aspnet_regsql.exe` Tool, um dies zu erreichen.
3. **Anpassen der Webanwendung-Einstellungen, um die Datenbank aus Schritt 2 zu verweisen.** Die *Erstellen des Mitgliedschaftsschemas in SQL Server* Tutorial wurde gezeigt, zwei Möglichkeiten zum Konfigurieren der Web-Anwendung, damit die `SqlMembershipProvider` würden in Schritt 2 ausgewählte Datenbank verwenden: durch Ändern der `LocalSqlServer` Verbindungszeichenfolgenname; oder durch einen neuen registrierten Anbieter zur Liste der Framework-Mitgliedschaftsanbieter hinzufügen und Anpassen dieser neuen Anbieter Verwendung die Datenbank aus Schritt 2.

Beim Erstellen einer Web-Anwendung, die verwendet die `SqlMembershipProvider` und Forms basierende Authentifizierung, Sie müssen zum Ausführen dieser drei Schritte vor der Verwendung der `Membership` Klasse oder die Anmeldung von ASP.NET-Webanwendungen-Steuerelemente. Da wir diese Schritte bereits in vorherigen Tutorials ausgeführt haben, können wir beim Einstieg in das mitgliedschaftsframework!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von ASP.NET-Seiten

In diesem Tutorial und die folgenden drei werden wir verschiedene Mitgliedschaft-bezogenen Funktionen und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in diesen Tutorials untersucht implementieren. Erstellen wir die Seiten und klicken Sie dann auf eine Siteübersichtsdatei `(Web.sitemap)`.

Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `Membership`. Fügen Sie fünf neue ASP.NET-Seiten auf die `Membership` Ordner, verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten ein:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

An diesem Punkt sollte Ihr Projekts des Projektmappen-Explorer im Screenshot dargestellt in Abbildung 1 ähneln.


[![Fünf neue Seiten wurden in den Ordner Mitgliedschaft hinzugefügt](creating-user-accounts-vb/_static/image2.png)](creating-user-accounts-vb/_static/image1.png)

**Abbildung 1**: fünf neue Seiten hinzugefügt wurden, die `Membership` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image3.png))


Jede Seite, an diesem Punkt müssen die beiden Inhaltssteuerelemente, einer für jede von der Masterseite ContentPlaceHolder-Steuerelemente: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample1.aspx)]

Bedenken Sie, dass die `LoginContent` ContentPlaceHolder Standard-Markup zeigt einen Link zum Anmelden oder Abmelden auf den Standort, je nachdem, ob der Benutzer authentifiziert wird. Das Vorhandensein der `Content2` Inhaltssteuerelement wird jedoch die Masterseite Standardmarkup überschreibt. Wie in erläutert *<a id="_msoanchor_6"></a>[eine Übersicht der Formularauthentifizierung](../introduction/an-overview-of-forms-authentication-vb.md)* Lernprogramm, dies ist hilfreich bei Seiten, in denen es sollten keine Anmeldeoptionen in der linken Spalte anzuzeigen.

Für diese fünf Seiten, allerdings möchten wir zum Anzeigen der Masterseite Standardmarkup für das `LoginContent` ContentPlaceHolder. Daher entfernen deklarative Markup für die `Content2` Inhaltssteuerelement. Danach sollte jedes der fünf Seitenmarkup nur einem Inhaltssteuerelement enthalten.

## <a name="step-2-creating-the-site-map"></a>Schritt 2: Erstellen der Sitezuordnung

Alle außer den einfachen Websites eine Standardnavigations-Benutzeroberfläche implementieren müssen. Die Benutzeroberfläche für die Navigation kann es sich um eine einfache Liste mit Links zu den verschiedenen Abschnitten der Website sein. Alternativ können diese Links in Menüs oder Strukturansichten angeordnet werden. Als Seitenentwickler ist das Erstellen der Standardnavigations-Benutzeroberfläche nur die Hälfte der Story. Wir benötigen auch eine Möglichkeit zum Definieren von logischen Struktur des Standorts in einer Weise verwaltet und aktualisiert. Neue Seiten hinzugefügt oder vorhandene Seiten entfernt, kann eine einzelne Quelle – die Sitemap - aktualisiert werden soll, und diese Änderungen auf der Website Standardnavigations-Benutzeroberfläche wiederzugeben.

Diese beiden Aufgaben - definieren die Sitemap und Implementierung einer Standardnavigations-Benutzeroberfläche basierend auf der Sitemap - sind einfach zu erreichen Dank der Sitemap-Framework, und die Navigation websteuerungselemente, hinzugefügt in ASP.NET Version 2.0. Das Site Map-Framework kann Entwickler die Sitemap zu definieren und dann über eine programmgesteuerte API darauf zugreifen (die [ `SiteMap` Klasse](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Enthalten die integrierten websteuerungselemente, Navigation eine [Menu-Steuerelement](https://msdn.microsoft.com/library/bz09dy46.aspx), ["TreeView"-Steuerelement](https://msdn.microsoft.com/library/3eafky27.aspx), und die [SiteMapPath-Steuerelement](https://msdn.microsoft.com/library/3eafky27.aspx).

Das Site Map-Framework basiert wie die Mitgliedschaft und Rollen-Frameworks, auf die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). Die Aufgabe der Sitemap-Provider-Klasse ist, die ein, die in-Memory-erzeugen die `SiteMap` Klasse aus einem persistenten Datenspeicher, z. B. eine XML-Datei oder einer Datenbanktabelle. .NET Framework im Lieferumfang von einem Standard-Siteübersichtsanbieter, die die Sitemap-Daten aus einer XML-Datei liest ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)), und dies ist der Anbieter, die wir in diesem Tutorial verwenden. Finden Sie bei einigen alternativen Site Map providerimplementierungen im Abschnitt Weitere Messwerte am Ende dieses Tutorials.

Die Standard-Siteübersichtsanbieter erwartet, dass eine richtig formatierte XML-Datei mit dem Namen `Web.sitemap` Stammverzeichnis vorhanden sein. Da wir diese Standardanbieter verwenden, müssen wir eine solche Datei hinzufügen und definieren die Sitemap-Struktur, in das entsprechende XML-Format. Fügen Sie die Datei, mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Neues Element hinzufügen. Aktivieren Sie im Dialogfeld zum Hinzufügen einer Datei vom Typ mit dem Namen Sitemap `Web.sitemap`.


[![Fügen Sie die Datei Web.sitemap zum Stammverzeichnis des Projekts](creating-user-accounts-vb/_static/image5.png)](creating-user-accounts-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer Datei mit dem Namen `Web.sitemap` zum Stammverzeichnis des Projekts ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image6.png))


Der XML-Sitezuordnungsdatei definiert der Website-Struktur als Hierarchie an. Diese hierarchische Beziehung wird in der XML-Datei über die Ahnenreihe des modelliert die `<siteMapNode>` Elemente. Die `Web.sitemap` muss beginnen mit einer `<siteMap>` übergeordnete Knoten mit genau einem `<siteMapNode>` untergeordneten. Diesem auf oberster Ebene `<siteMapNode>` Element stellt den Stamm der Hierarchie dar, und möglicherweise eine beliebige Anzahl von untergeordneten Knoten. Jede `<siteMapNode>` -Element muss enthalten eine `title` Attribut, und kann optional enthalten `url` und `description` Attribute, u. a.; jeder nicht leeren `url` Attribut muss eindeutig sein.

Geben Sie den folgenden XML-Code in die `Web.sitemap` Datei:

[!code-xml[Main](creating-user-accounts-vb/samples/sample2.xml)]

Das obenstehende Markup der Site-Zuordnung definiert die Hierarchie, die in Abbildung 3 dargestellt.


[![Die Siteübersicht darstellt, eine hierarchische Navigationsstruktur](creating-user-accounts-vb/_static/image8.png)](creating-user-accounts-vb/_static/image7.png)

**Abbildung 3**: die Siteübersicht darstellt, einer hierarchischen Navigationsstruktur ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image9.png))


## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Schritt 3: Aktualisieren der Masterseite Einbeziehung eine Standardnavigations-Benutzeroberfläche

ASP.NET umfasst eine Reihe von navigationsbezogenen Websteuerelementen für das Entwerfen einer Benutzeroberfläche. Dazu gehören das Menü, TreeView und SiteMapPath-Steuerelemente. Die Steuerelemente Menu und TreeView Rendern der Siteübersichtsstruktur in einem Menü oder eine Struktur, bzw. ein, während der SiteMapPath Breadcrumb anzeigt, den aktuellen Knoten und dessen Vorgänger aufgerufen wird. Die Sitemap-Daten an andere websteuerungselemente, verwenden die SiteMapDataSource Daten gebunden werden kann und programmgesteuert zugegriffen werden kann, über die `SiteMap` Klasse.

Da ist eine ausführliche Beschreibung der Siteübersicht-Framework und die Navigationssteuerelemente Gegenstand dieser Reihe von Lernprogrammen, sondern als eingesparten Zeit, die wir Erstellen von eigenen Benutzeroberfläche für die Navigation stattdessen leihen verwendet meinem *[ Arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Reihe von Lernprogrammen, die Wiederholungsmodul-Steuerelement verwendet eine zwei-Deep Aufzählung der Navigationslinks, anzeigen, wie in Abbildung 4 dargestellt.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Eine Liste zwei Ebenen von Links hinzufügen in der linken Spalte

Um diese Schnittstelle zu erstellen, fügen Sie das folgende deklarative Markup zu der `Site.master` Masterseite linken Spalte, in dem der Text-to-do: Menü werden hier... derzeit befindet.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample3.aspx)]

Das obenstehende Markup bindet ein Repeater-Steuerelement, das mit dem Namen `menu` mit einer SiteMapDataSource verbunden ist, womit die Map-Standorthierarchie, die in definierten `Web.sitemap`. Da die SiteMapDataSource-Steuerelements [ `ShowStartingNode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) festgelegt ist, auf "false", die es beginnt mit der Rückgabe die Sitemap-Hierarchie, die die Nachfolgerelemente des Home-Knotens ab. Der Repeater zeigt diesen Knoten (derzeit nur "Mitgliedschaft") in einem `<li>` Element. Ein weiterer, innere Repeater zeigt den aktuellen Knoten untergeordneten Elemente in einer geschachtelten ungeordnete Liste.

Abbildung 4 zeigt die gerenderte Ausgabe das obenstehende Markup, mit der Site Map-Struktur, die wir in Schritt2 erstellt haben. Der Repeater gibt einfaches ungeordnete Liste Markup wieder. der cascading Stylesheet-Regeln, die in definierten `Styles.css` sind verantwortlich für das Layout ästhetisch ansprechendste. Eine ausführlichere Beschreibung der Funktionsweise von des obenstehende Markups, finden Sie in der [Masterseiten und Sitenavigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx) Tutorial.


[![Die Standardnavigations-Benutzeroberfläche wird gerendert listet unter Verwendung von geschachtelten ungeordnete](creating-user-accounts-vb/_static/image11.png)](creating-user-accounts-vb/_static/image10.png)

**Abbildung 4**: die Standardnavigations-Benutzeroberfläche wird gerendert listet unter Verwendung von geschachtelten ungeordnete ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image12.png))


### <a name="adding-breadcrumb-navigation"></a>Hinzufügen von Breadcrumbnavigation

Zusätzlich zu der Liste der Links in der linken Spalte, lassen Sie uns auch haben jede Seitenanzeige eine [Breadcrumb](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Breadcrumb wird ein Landmarks Element der Benutzeroberfläche, die schnell ihre aktuellen Position innerhalb der Standorthierarchie von Benutzern angezeigt wird. Das Steuerelement SiteMapPath das Site Map-Framework verwendet, um die aktuelle Seite Speicherort in der sitezuordnung zu bestimmen, und es zeigt dann Breadcrumb basierend auf diesen Informationen.

Insbesondere eine `<span>` Element auf der Masterseite Header `<div>` -Element, und legen Sie den neuen `<span>` des Elements `class` Breadcrumb Attribut. (Die `Styles.css` -Klasse enthält eine Regel für eine Breadcrumb-Klasse.) Fügen Sie ein SiteMapPath an diesen neuen `<span>` Element.

[!code-aspx[Main](creating-user-accounts-vb/samples/sample4.aspx)]

Abbildung 5 zeigt die Ausgabe des SiteMapPath aus, wenn das Unternehmen besuchen `~/Membership/CreatingUserAccounts.aspx`.


[![Die Breadcrumb-Leiste zeigt die aktuelle Seite, und ordnen Sie dessen Vorgänger auf der Website](creating-user-accounts-vb/_static/image14.png)](creating-user-accounts-vb/_static/image13.png)

**Abbildung 5**: Breadcrumb zeigt die aktuelle Seite und dessen Vorgänger in der Siteübersicht ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image15.png))


## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Schritt 4: Entfernen, die benutzerdefinierten Prinzipal und die Identity-Logik

In der *<a id="_msoanchor_7"></a>[Konfiguration der Formularauthentifizierung und erweiterte Themen](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md)* Lernprogramm wurde erläutert, wie benutzerdefinierte Haupt- und Identitätsobjekte Objekte dem authentifizierten Benutzer verknüpfen. Wir erreicht dies durch das Erstellen eines ereignishandlers in `Global.asax` für der Anwendung die `PostAuthenticateRequest` Ereignis, das ausgelöst, nachdem wird die `FormsAuthenticationModule` den Benutzer authentifiziert hat. In diesem Ereignishandler ersetzt die `GenericPrincipal` und `FormsIdentity` Objekte hinzugefügt, indem die `FormsAuthenticationModule` mit der `CustomPrincipal` und `CustomIdentity` Objekte wir in diesem Lernprogramm erstellt haben.

Während benutzerdefinierter Objekte für Haupt- und Identitätsobjekte in bestimmten Szenarien, in den meisten Fällen werden die `GenericPrincipal` und `FormsIdentity` Objekte sind ausreichend. Daher denke ich, dass es wäre hilfreich sein, sich mit dem Standardverhalten zurück. Können Sie diese Änderung vornehmen, indem Sie entweder entfernen oder Auskommentieren der `PostAuthenticateRequest` -Ereignishandler oder durch Löschen der `Global.asax` Datei vollständig.

> [!NOTE]
> Entfernt den Code in oder auskommentiert haben `Global.asax`, müssen Sie den Code in auskommentieren `Default.aspx's` Code-Behind-Klasse, in den umgewandelt der `User.Identity` Eigenschaft, um eine `CustomIdentity` Instanz.


## <a name="step-5-programmatically-creating-a-new-user"></a>Schritt 5: Programmgesteuertes Erstellen eines neuen Benutzers

Erstellen Sie ein neues Benutzerkonto über die Mitgliedschaft Framework-Verwendung der `Membership` Klasse [ `CreateUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx). Diese Methode wurde für den Benutzernamen, Kennwort und andere Felder benutzerbezogenen Eingabeparameter. Aufgerufen werden, die Erstellung des neuen Benutzerkontos für den konfigurierten Mitgliedschaftsanbieter delegiert und gibt dann zurück eine [ `MembershipUser` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) , die das gerade erstellten Benutzerkonto darstellt.

Die `CreateUser` Methode verfügt über vier Überladungen, jeweils eine andere Anzahl von Eingabeparametern zu akzeptieren:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Diese vier Überladungen unterscheiden sich auf die Menge an Informationen, die gesammelt werden. Die erste Überladung, erfordert z. B. nur den Benutzernamen und das Kennwort für das neue Benutzerkonto an, während das zweite Argument auch die e-Mail-Adresse des Benutzers erfordert.

Diese Überladungen vorhanden sein, da die erforderlichen Informationen zum Erstellen eines neuen Benutzerkontos zu den Konfigurationseinstellungen für den Mitgliedschaftsanbieter abhängig ist. In der *<a id="_msoanchor_8"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Angabe Mitgliedschaft Konfigurationseinstellungen im untersucht Lernprogramm `Web.config`. Tabelle 2 enthalten eine vollständige Liste der Konfigurationseinstellungen.

Eine Konfiguration dieser Art Mitgliedschaft Anbieter festlegen, wirkt sich auf was `CreateUser` Überladungen können verwendet werden wird die `requiresQuestionAndAnswer` festlegen. Wenn `requiresQuestionAndAnswer` nastaven NA hodnotu `true` (Standardeinstellung), und klicken Sie dann wir beim Erstellen eines neuen Benutzerkontos eine Sicherheitsfrage und die Antwort angeben müssen. Diese Informationen werden später verwendet werden, wenn der Benutzer zum Zurücksetzen oder ihr Kennwort ändern muss. Genauer gesagt wird zu diesem Zeitpunkt werden sie die Sicherheitsfrage angezeigt, und die richtige Antwort zum Zurücksetzen oder ändern ihr Kennwort eingeben muss. Daher, wenn die `requiresQuestionAndAnswer` nastaven NA hodnotu `true` und dann eine der ersten beiden Aufrufen `CreateUser` führt zu einer Ausnahme überladen, da die Sicherheitsfrage und die Antwort nicht vorhanden sind. Da unsere Anwendung derzeit konfiguriert ist, um eine Sicherheitsfrage und eine Antwort erfordern, müssen wir eine der letzten zwei Überladungen zu verwenden, wenn des Benutzers programmgesteuert zu erstellen.

Zur Veranschaulichung der Verwendung der `CreateUser` -Methode, erstellen wir eine Benutzeroberfläche, in denen der Benutzer für ihren Namen, das Kennwort, e-Mail-Adresse und eine Antwort auf eine vordefinierte Sicherheitsfrage aufgefordert. Öffnen der `CreatingUserAccounts.aspx` auf der Seite die `Membership` Ordner, und fügen Sie die folgenden Web-Steuerelemente an das Inhaltssteuerelement hinzu:

- Ein Textfeld mit dem Namen `Username`
- Ein Textfeld mit dem Namen `Password`, dessen `TextMode` Eigenschaft auf festgelegt ist `Password`
- Ein Textfeld mit dem Namen `Email`
- Eine Bezeichnung namens `SecurityQuestion` mit seiner `Text` Eigenschaft gelöscht
- Ein Textfeld mit dem Namen `SecurityAnswer`
- Eine Schaltfläche namens `CreateAccountButton` , deren `Text` Eigenschaft so erstellen Sie das Benutzerkonto festgelegt ist
- Ein Label-Steuerelement, das mit dem Namen `CreateAccountResults` mit seiner `Text` Eigenschaft gelöscht

An diesem Punkt sollte Ihr Bildschirm im Screenshot dargestellt in Abbildung 6 ähneln.


[![Die verschiedenen Websteuerelemente auf der Seite "CreatingUserAccounts.aspx" hinzufügen](creating-user-accounts-vb/_static/image17.png)](creating-user-accounts-vb/_static/image16.png)

**Abbildung 6**: Fügen Sie die verschiedenen Web-Steuerelemente, die `CreatingUserAccounts.aspx Page` ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image18.png))


Die `SecurityQuestion` Bezeichnung und `SecurityAnswer` Textfeld sollen eine vordefinierte Sicherheitsfrage anzuzeigen und die Antwort des Benutzers erfassen. Beachten Sie, dass die Sicherheitsfrage und die Antwort pro Benutzer nach Benutzer gespeichert werden, daher ist es möglich, jeder Benutzer ihre eigenen Sicherheitsfrage definieren können. Allerdings für dieses Beispiel, das ich haben sich entschieden, verwenden Sie eine universelle Sicherheitsfrage, nämlich: Was ist, dass Ihre Lieblingsfarbe?

Um diese Sicherheitsfrage bereits definierten zu implementieren, fügen Sie eine Konstante, auf der Seite des Code-Behind-Klasse, die mit dem Namen `passwordQuestion`, ihm die Sicherheitsfrage zuweisen. Klicken Sie auf die `Page_Load` Ereignishandler, weisen Sie zu diesem Konstante, die `SecurityQuestion` Bezeichnungsfelds `Text` Eigenschaft:

[!code-vb[Main](creating-user-accounts-vb/samples/sample5.vb)]

Als Nächstes erstellen Sie einen Ereignishandler für die `CreateAccountButton'` s `Click` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample6.vb)]

Die `Click` Ereignishandler startet, durch die Definition einer Variablen namens `createStatus` des Typs [ `MembershipCreateStatus` ](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` ist eine Enumeration, die den Status der gibt an, die `CreateUser` Vorgang. Wenn das resultierende das Benutzerkonto erfolgreich erstellt wurde z. B. `MembershipCreateStatus` Instanz wird auf den Wert festgelegt werden `Success;` andererseits, wenn der Vorgang fehlschlägt, da bereits einen Benutzer mit demselben Benutzernamen vorhanden ist, wird festgelegt auf einen Wert von `DuplicateUserName`. In der `CreateUser` Überladung, die wir verwenden, müssen wir übergeben eine `MembershipCreateStatus` -Instanz in der Methode. Dieser Parameter wird festgelegt, auf den entsprechenden Wert in der `CreateUser` -Methode, und wir sehen den Wert nach dem Aufruf der Methode, um zu bestimmen, ob das Benutzerkonto erfolgreich erstellt wurde.

Nach dem Aufruf `CreateUser`, und übergeben Sie `createStatus`, `Select Case` -Anweisung verwendet, um die Ausgabe von einer entsprechenden Meldung angezeigt, abhängig von den zugewiesenen Wert `createStatus`. Abbildung 7 zeigt die Ausgabe, wenn ein neuer Benutzer erfolgreich erstellt wurde. Abbildungen 8 und 9 wird die Ausgabe auf, wenn das Benutzerkonto, das nicht erstellt wird. In Abbildung 8 eingegeben der Besucher einer fünf Buchstaben ein Kennwort nicht in den Konfigurationseinstellungen für den Mitgliedschaftsanbieter ausgeschrieben die Kennwort-sicherheitsanforderungen erfüllt. In Abbildung 9 versucht der Besucher, erstellen Sie ein Benutzerkonto mit einem vorhandenen Benutzernamen (den Knoten erstellt, die in Abbildung 7).


[![Ein neues Benutzerkonto wurde erfolgreich erstellt](creating-user-accounts-vb/_static/image20.png)](creating-user-accounts-vb/_static/image19.png)

**Abbildung 7**: ein neues-Benutzerkonto ist, wurde erfolgreich erstellt. ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image21.png))


[![Das Benutzerkonto wird nicht erstellt werden, weil das angegebene Kennwort zu schwach ist](creating-user-accounts-vb/_static/image23.png)](creating-user-accounts-vb/_static/image22.png)

**Abbildung 8**: das Benutzerkonto wird nicht erstellt werden, da das angegebene Kennwort zu schwach ist ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image24.png))


[![Das Benutzerkonto ist, dass nicht erstellt, weil der Benutzername wird bereits verwendet](creating-user-accounts-vb/_static/image26.png)](creating-user-accounts-vb/_static/image25.png)

**Abbildung 9**: das Benutzerkonto ist nicht erstellt, weil der Benutzername wird bereits verwendet ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image27.png))


> [!NOTE]
> Vielleicht fragen Sie Erfolg oder Fehler zu bestimmen, wenn eine der ersten beiden `CreateUser` methodenüberladungen, weder besitzt einen Parameter vom Typ `MembershipCreateStatus`. Lösen Sie diese zunächst zwei Überladungen einer [ `MembershipCreateUserException` Ausnahme](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) bei ein Fehler auftritt, wozu eine [ `StatusCode` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) vom Typ `MembershipCreateStatus`.


Nach dem Erstellen einigen Benutzerkonten, stellen Sie sicher, dass die Konten erstellt wurden, indem Sie den Inhalt der Auflistung der `aspnet_Users` und `aspnet_Membership` Tabellen in der `SecurityTutorials.mdf` Datenbank. Wie in Abbildung 10 gezeigt, wurden ich hinzugefügt, dass zwei Benutzer über die `CreatingUserAccounts.aspx` Seite: Tito und Bruce.


[![Zwei Benutzer in der Membership-Benutzer-Store sind: Tito und Bruce](creating-user-accounts-vb/_static/image29.png)](creating-user-accounts-vb/_static/image28.png)

**Abbildung 10**: zwei Benutzer in der Membership-Benutzer-Store sind: Tito und Bruce ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image30.png))


Während des mitgliedschaftsbenutzerspeichers jetzt Bruce und Tito den Kontoinformationen enthält, gibt es noch Funktionalität zu implementieren, die nach Bruce oder Tito, an der Website anmelden können. Derzeit `Login.aspx` überprüft die Anmeldeinformationen des Benutzers anhand eines Satzes hartcodierten Benutzernamen/Kennwort-Paare – Ja *nicht* überprüft die angegebenen Anmeldeinformationen für das mitgliedschaftsframework. Zum Anzeigen nun der neuen Benutzerkonten in der `aspnet_Users` und `aspnet_Membership` Tabellen müssen ausreichen. In den nächsten Lernprogrammen *<a id="_msoanchor_9"></a>[überprüft die Benutzer Anmeldeinformationen für die Mitgliedschaft Benutzer speichern](validating-user-credentials-against-the-membership-user-store-vb.md)*, aktualisieren wir die Anmeldeseite für den Speicher für die Mitgliedschaft überprüft.

> [!NOTE]
> Wenn Sie keinen Benutzern angezeigt werden Ihre `SecurityTutorials.mdf` Datenbank möglicherweise daran, dass Ihre Webanwendung den Standardmitgliedschaftsanbieter, verwendet `AspNetSqlMembershipProvider`, verwendet der `ASPNETDB.mdf` Datenbank als Benutzerspeicher. Um festzustellen, ob dies der Fall ist, klicken Sie auf die Schaltfläche "Aktualisieren" im Projektmappen-Explorer. Wenn eine Datenbank mit dem Namen `ASPNETDB.mdf` hinzugefügt wurde die `App_Data` Ordner, dies ist das Problem. Zurück zu Schritt 4 der *<a id="_msoanchor_10"></a>[erstellen das Schema für die Mitgliedschaft in SQL Server](creating-the-membership-schema-in-sql-server-vb.md)* Tutorial Anleitungen zum ordnungsgemäßen Konfigurieren der Mitgliedschaftsanbieter.


In den meisten erstellen Konto Szenarien, die Besucher erhält eine Schnittstelle, geben ihren Benutzernamen, Kennwort, e-Mail und andere wichtige Informationen, die an diesem Punkt ein neues Konto erstellt wird. In diesem Schritt wir untersuchten, erstellen eine solche Schnittstelle manuell, und klicken Sie dann gesehen, wie mit der `Membership.CreateUser` Methode, um das neue Benutzerkonto programmgesteuert hinzufügen auf Basis des Benutzers Eingaben. Unser Code, erstellt jedoch nur das neue Benutzerkonto. Es hat nachverfolgung Aktionen, z. B. Anmeldung des Benutzers an den Standort unter dem gerade erstellten Benutzerkonto oder senden eine Bestätigung per e-Mail an den Benutzer nicht ausgeführt. Diese zusätzlichen Schritte müssten zusätzlichen Code in der Schaltfläche `Click` -Ereignishandler.

Im Lieferumfang von ASP.NET Steuerelement CreateUserWizard, die für die Verarbeitung der kontoerstellung für Benutzer über eine Benutzeroberfläche zum Erstellen neuer Benutzerkonten, bis das Konto in das mitgliedschaftsframework erstellen und Ausführen der nach dem Konto Rendern von Erstellungsaufgaben, z. B. eine Bestätigung per e-Mail senden und den gerade erstellten Benutzer bei der Website anzumelden. Mit dem Steuerelement CreateUserWizard ist so einfach wie das Ziehen des Steuerelements CreateUserWizard aus der Toolbox auf eine Seite und klicken Sie dann einige Eigenschaften festlegen. In den meisten Fällen müssen Sie keine einzige Zeile Code schreiben. Dieses raffinierte Steuerelement im Detail in Schritt 6 wird untersucht.

Wenn neue Benutzerkonten nur mit einer typischen Webseite für die Kontoerstellung erstellt wurden, ist es unwahrscheinlich, dass Sie jemals benötigen, Code zu schreiben, verwendet der `CreateUser` -Methode, wie das Steuerelement CreateUserWizard wird wahrscheinlich Ihre Anforderungen erfüllen. Allerdings die `CreateUser` Methode ist nützlich in Szenarien, in denen Sie eine stark angepasste benutzererfahrung von Konto erstellen oder wenn Sie neue Benutzerkonten, die über eine alternative Benutzeroberfläche programmgesteuert erstellen müssen. Sie möglicherweise z. B. eine Seite, mit dem Benutzer eine XML-Datei hochladen, die Benutzerinformationen aus einer anderen Anwendung enthält. Die Seite kann analysiert den Inhalt des hochgeladene XML-Datei, und erstellen ein neues Konto für jeden Benutzer dargestellt in der XML-Code durch Aufrufen der `CreateUser` Methode.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Schritt 6: Erstellen eines neuen Benutzers mit dem Steuerelement CreateUserWizard

Im Lieferumfang von ASP.NET ist einer Anzahl von Websteuerelementen Anmeldung. Diese Steuerelemente werden in vielen gängigen Benutzer und Anmeldenamen-kontobezogene Szenarien unterstützen. Die [Steuerelement CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) ist ein solches Steuerelement, das eine Benutzeroberfläche zum Hinzufügen eines neuen Benutzerkontos, das mitgliedschaftsframework anzeigen soll.

Wie bei vielen der anderen Web anmeldebezogene Steuerelemente kann die CreateUserWizard verwendet werden, ohne eine einzige Codezeile schreiben zu müssen. Es intuitiv bietet eine Benutzeroberfläche, die basierend auf der Membership-Provider-Konfigurationseinstellungen und ruft intern die `Membership` Klasse `CreateUser` -Methode auf, nachdem der Benutzer gibt die erforderlichen Informationen, und klickt auf die Schaltfläche "Benutzer erstellen". Das Steuerelement CreateUserWizard ist sehr anpassbar. Es gibt eine Vielzahl von Ereignissen, die in verschiedenen Phasen der kontoerstellung ausgelöst werden. Wir können Ereignishandler erstellen, bei Bedarf, benutzerdefinierten Logik in den Account Creation Workflow einzufügen. Darüber hinaus ist die CreateUserWizards Darstellung sehr flexibel. Es gibt eine Reihe von Eigenschaften, die die Darstellung der Standardschnittstelle definieren. bei Bedarf das Steuerelement in eine Vorlage konvertiert werden kann, oder möglicherweise zusätzliche Schritte zur Registrierung hinzugefügt werden.

Beginnen wir mit einem Blick auf die Verwendung von Standard-Schnittstelle und das Verhalten des Steuerelements CreateUserWizard an. Gewusst wie: Anpassen der Darstellung über Eigenschaften und Ereignisse des Steuerelements wird dann vorgestellt.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Untersuchen der CreateUserWizard des Standard-Schnittstelle und das Verhalten

Wechseln Sie zurück zur der `CreatingUserAccounts.aspx` auf der Seite die `Membership` Ordner, in den Entwurf oder auf einer Trennschaltfläche-Modus wechseln, und fügen Sie dann auf den oberen Rand der Seite ein Steuerelement CreateUserWizard hinzu. Das Steuerelement CreateUserWizard ist der Toolbox für die Anmeldung im Abschnitt "Steuerelemente" abgelegt. Legen Sie nach dem Hinzufügen des Steuerelements, dessen `ID` Eigenschaft `RegisterUser`. Wie der Screenshot in Abbildung 11 zeigt, rendert die CreateUserWizard eine Schnittstelle mit der Textfelder für des neuen Benutzers Benutzername, Kennwort, e-Mail-Adresse, und die Sicherheitsfrage und Antwort.


[![Rendert das Steuerelement CreateUserWizard eine generische erstellen Benutzeroberfläche.](creating-user-accounts-vb/_static/image32.png)](creating-user-accounts-vb/_static/image31.png)

**Abbildung 11**: Steuerelement CreateUserWizard rendert eine generische Benutzeroberfläche zu erstellen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image33.png))


Werfen Sie einen Moment Zeit, die Benutzer-Standardschnittstelle, die vom Steuerelement CreateUserWizard mit der Benutzeroberfläche in Schritt 5 erstellten verglichen werden soll. Zunächst einmal ermöglicht Steuerelement CreateUserWizard den Besucher auf die Sicherheitsfrage und die Antwort angeben, während unserer manuell erstellten Schnittstelle eine vordefinierte Sicherheitsfrage verwendet. -Schnittstelle des Steuerelements CreateUserWizard hinaus Steuerelementen zur gültigkeitsprüfung, während wir noch mussten, um die Überprüfung auf Formularfelder unsere Schnittstelle zu implementieren. Und die CreateUserWizard Steuerelementschnittstellen enthält ein Textfeld Kennwort bestätigen (zusammen mit einem CompareValidator sicherstellen, dass der Text, das Kennwort eingegeben und Kennwort vergleichen Textfelder gleich sind).

Interessant ist, dass das Steuerelement CreateUserWizard Konfigurationseinstellungen für den Mitgliedschaftsanbieter, beim Rendern der Benutzeroberfläche berät. Z. B. die Textfelder ein Sicherheit Frage und Antwort werden nur angezeigt, wenn `requiresQuestionAndAnswer` auf "true" festgelegt ist. Ebenso fügt CreateUserWizard automatisch eine RegularExpressionValidator-Steuerelement, um sicherzustellen, dass die Kennwort-sicherheitsanforderungen erfüllt sind, und legt sie fest seine `ErrorMessage` und `ValidationExpression` Eigenschaften auf Grundlage der `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`, und `passwordStrengthRegularExpression` Konfigurationseinstellungen.

Das Steuerelement CreateUserWizard, wie der Name schon sagt, ergibt sich aus der [Assistenten-Steuerelement](https://msdn.microsoft.com/library/s2etd1ek.aspx). Assistenten-Steuerelementen dienen zum Bereitstellen einer Schnittstelle zum Abschließen von Aufgaben mit mehreren Schritten. Assistenten-Steuerelement möglicherweise eine beliebige Anzahl von `WizardSteps`, von denen jede eine Vorlage, die den HTML-Code definiert ist und websteuerungselemente, die für diesen Schritt. Die Assistenten-Steuerelement zeigt zu Beginn der ersten `WizardStep`, zusammen mit der Steuerelemente für die Seitennavigation, die den Benutzer von einer Stufe zur nächsten fortfahren oder zurück zu den vorherigen Schritten zu ermöglichen.

Wie in Abbildung 11 deklarative Markup zeigt, des Steuerelements CreateUserWizard Standardschnittstelle enthält zwei `WizardStep` s:

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) ? Rendert die Schnittstelle zum Erfassen von Informationen für das neue Benutzerkonto zu erstellen. Dies ist der in Abbildung 11 dargestellt.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) ? Rendert eine Meldung gibt an, dass das Konto erfolgreich erstellt wurde.

Die CreateUserWizards Aussehen und Verhalten können geändert werden, durch eine der folgenden Schritte aus Vorlagen zu konvertieren, oder indem Sie Ihre eigenen `WizardStep` s. Betrachten wir Hinzufügen einer `WizardStep` auf die Registrierung-Schnittstelle in der *Speichern von zusätzlichen Benutzerinformationen* Tutorial.

Sehen Sie das Steuerelement CreateUserWizard in Aktion an. Besuchen Sie die `CreatingUserAccounts.aspx` Seite über einen Browser. Starten Sie durch den Wechsel von einigen ungültigen Werten in der CreateUserWizard Schnittstelle. Versuchen Sie es ein Kennwort, das entsprechen nicht und die sicherheitsanforderungen für das Kennwort eingeben, oder das Textfeld Benutzername leer bleibt. Die CreateUserWizard wird eine entsprechende Fehlermeldung angezeigt. Abbildung 12 zeigt die Ausgabe, bei dem Versuch, einen Benutzer mit einem unzureichend sicheres Kennwort zu erstellen.


[![Die CreateUserWizard fügt automatisch die Steuerelemente zur gültigkeitsprüfung](creating-user-accounts-vb/_static/image35.png)](creating-user-accounts-vb/_static/image34.png)

**Abbildung 12**: die CreateUserWizard automatisch Fügt Steuerelemente zur gültigkeitsprüfung ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image36.png))


Als Nächstes geben Sie die entsprechenden Werte in der CreateUserWizard, und klicken Sie auf die Schaltfläche "Benutzer erstellen". Wenn die erforderlichen Felder eingegeben wurden, und die kennwortsicherheit der ist ausreichend, die CreateUserWizard erstellt ein neues Benutzerkonto durch das mitgliedschaftsframework und klicken Sie dann zeigt die `CompleteWizardStep`der Benutzeroberfläche (siehe Abbildung 13). Hinter den Kulissen der CreateUserWizard Ruft die `Membership.CreateUser` -Methode, wie wir in Schritt 5.


[![Ein neues Benutzerkonto wurde erfolgreich erstellt wurde](creating-user-accounts-vb/_static/image38.png)](creating-user-accounts-vb/_static/image37.png)

**Abbildung 13**: ein neues-Benutzerkonto wurde erfolgreich erstellt wurde ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image39.png))


> [!NOTE]
> Wie in Abbildung 13 gezeigt, die `CompleteWizardStep`die Schnittstelle enthält eine Schaltfläche zum fortfahren. Jedoch nur an diesem Punkt klicken, wird ein Postback, sodass Besucher auf derselben Seite führt. In der Anpassen der CreateUserWizards Darstellung und Verhalten über seine Eigenschaften im Abschnitt es wird erläutert, wie Sie diese Schaltfläche, die den Besucher senden lassen können `Default.aspx` (oder eine andere Seite).


Klicken Sie nach dem Erstellen eines neuen Benutzerkontos, zurück zu Visual Studio, und untersuchen Sie die `aspnet_Users` und `aspnet_Membership` Tabellen wie in Abbildung 10, um sicherzustellen, dass das Konto erfolgreich erstellt wurde.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Anpassen von Verhalten und die Darstellung durch die Eigenschaften der CreateUserWizard des

Die CreateUserWizard kann angepasst werden, in einer Vielzahl von Möglichkeiten, über Eigenschaften, `WizardStep` s und Ereignishandler. In diesem Abschnitt sehen wir an, wie die Darstellung des Steuerelements über seine Eigenschaften anpassen; Erweitern das Verhalten des Steuerelements über Ereignishandler ist der nächste Abschnitt untersucht.

Praktisch alle der in die Standardbenutzeroberfläche des Steuerelements CreateUserWizard angezeigte Text kann durch seine Vielzahl von Eigenschaften angepasst werden. Beispielsweise können der Benutzername, Kennwort, Kennwort bestätigen, E-mail, Sicherheitsfrage und Sicherheitsantwort Bezeichnungen auf der linken Seite der Textfelder angepasst werden, durch die [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [ `PasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [ `ConfirmPasswordLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [ `EmailLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [ `QuestionLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx), und [ `AnswerLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) Eigenschaften, bzw. Ebenso stehen die Eigenschaften zum Angeben des Texts für die Schaltflächen "Benutzer erstellen und Fortfahren" in der `CreateUserWizardStep` und `CompleteWizardStep`, auch als ob diese Schaltflächen als Schaltflächen, LinkButtons oder ImageButtons gerendert werden.

Die Farben, Rahmen, Schriftarten und andere visuelle Elemente werden über eine Vielzahl von Stileigenschaften konfigurierbar. Steuerelement CreateUserWizard selbst verfügt über die allgemeine Web-Steuerelement Stileigenschaften - `BackColor`, `BorderStyle`, `CssClass`, `Font`und so weiter – und es gibt eine Anzahl der Formateigenschaften für das Definieren der Darstellung für bestimmte Bereiche der CreateUserWizard Schnittstelle. Die [ `TextBoxStyle` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), z. B. definiert die Formatvorlagen für die Textfelder ein, in der `CreateUserWizardStep`, während die [ `TitleTextStyle` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) definiert das Format für den Titel (registrieren Sie sich für Ihre neue Konto).

Zusätzlich zu den Darstellungseigenschaften gibt es eine Reihe von Eigenschaften, die die Steuerelement CreateUserWizard Verhalten beeinflussen. Die [ `DisplayCancelButton` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), legen Sie auf "true" zeigt eine Abbrechen-Schaltfläche neben der Schaltfläche "Benutzer erstellen" (der Standardwert ist "false"). Wenn Sie auf "Abbrechen"-Schaltfläche anzuzeigen, müssen Sie auch festlegen, die [ `CancelDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), die angibt, dass der Seite wird der Benutzer nach dem Klicken auf "Abbrechen" an. Wie im vorherigen Abschnitt die Schaltfläche Weiter in die `CompleteWizardStep`Schnittstelle ein Postback auslöst, jedoch lässt den Besucher auf derselben Seite. Um eine andere Seite der Besucher nach dem Klicken auf die Schaltfläche "Weiter" zu senden, geben Sie einfach die URL in die [ `ContinueDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Aktualisieren wir die `RegisterUser` Steuerelement CreateUserWizard, auf die Schaltfläche "Abbrechen" anzeigen und zum Senden des Besuchers `Default.aspx` Wenn auf die Schaltflächen Abbrechen oder fortsetzen geklickt werden. Zu diesem Zweck legen Sie die `DisplayCancelButton` -Eigenschaft auf "true", und sowohl die `CancelDestinationPageUrl` und `ContinueDestinationPageUrl` Eigenschaften ~ / "default.aspx". Abbildung 14 zeigt die aktualisierte CreateUserWizard, wenn Sie über einen Browser angezeigt.


[![Die CreateUserWizardStep enthält eine Schaltfläche "Abbrechen"](creating-user-accounts-vb/_static/image41.png)](creating-user-accounts-vb/_static/image40.png)

**Abbildung 14**: die `CreateUserWizardStep` enthält eine Abbrechen-Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image42.png))


Wenn ein Besucher einen Benutzernamen, Kennwort, e-Mail-Adresse und einer Sicherheitsfrage und Antwort gibt und klickt der Benutzer erstellen, wird ein neues Benutzerkonto wird erstellt, und der Besucher als dieser neu erstellten Benutzer angemeldet ist. Vorausgesetzt, dass die Person, die auf der Seite ein neues Konto für sich selbst erstellt wird, ist dies wahrscheinlich das gewünschte Verhalten. Allerdings empfiehlt es sich um Administratoren hinzufügen, neue Benutzerkonten zu ermöglichen. In diesem Fall würde das Benutzerkonto, das erstellt werden, aber der Administrator bleibt angemeldet als Administrator (und nicht als dem neu erstellten Konto). Dieses Verhalten kann geändert werden, bis der boolesche Wert [ `LoginCreatedUser` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Benutzerkonten in das mitgliedschaftsframework enthalten eine genehmigte Flag; Benutzer, die nicht genehmigt sind, können nicht an der Website anmelden. Standardmäßig wird ein neu erstelltes Konto bei als genehmigt, sodass des Benutzers sofort auf der Website anmelden, markiert. Es ist jedoch möglich, neue Benutzerkonten, die als nicht genehmigt haben. Vielleicht möchten Sie ein Administrator neue Benutzer manuell zu genehmigen, bevor sie sich anmelden können, oder vielleicht möchten Sie, ob die e-Mail-Adresse bei der Registrierung gültig ist, bevor die Übergabe von eines Benutzers anmelden. Was auch immer der Fall sein kann, dass das neu erstellte Benutzerkonto als nicht genehmigtes durch Festlegen des Steuerelements CreateUserWizard [ `DisableCreatedUser` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) auf "true" (der Standardwert ist "false").

Andere Verhalten-bezogene Eigenschaften Hinweis gehören `AutoGeneratePassword` und `MailDefinition`. Wenn die [ `AutoGeneratePassword` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) ist auf True festgelegt, die `CreateUserWizardStep` zeigt nicht die Textfelder Kennwort und Kennwort bestätigen ein; stattdessen das neu erstellte Kennwort des Benutzers wird automatisch generiert mithilfe der `Membership` Klasse [ `GeneratePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Die `GeneratePassword` -Methode erstellt ein Kennwort mit einer angegebenen Länge und mit einer ausreichenden Anzahl von nicht-alphanumerische Zeichen, um die Kennwort-sicherheitsanforderungen zu erfüllen.

Die [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) ist nützlich, wenn Sie eine e-Mail an die e-Mail-Adresse, die während der kontoerstellung angegeben senden möchten. Die `MailDefinition` Eigenschaft umfasst eine Reihe von untergeordneten Eigenschaften zum Definieren von Informationen über die erstellte e-Mail-Nachricht. Dieser Untereigenschaften umfassen Konfigurationsoptionen, z.B. `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`, und `BodyFileName`. Die [ `BodyFileName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) verweist auf eine Text- oder HTML-Datei, die den Text der e-Mail-Nachricht enthält. Der Text unterstützt, zwei vordefinierte Platzhalter: `<%UserName%>` und `<%Password%>`. Diese Platzhalter, der bei Vorhandensein in den `BodyFileName` Datei, die mit Namen und das Kennwort des gerade erstellten Benutzers ersetzt werden.

> [!NOTE]
> Die `CreateUserWizard` des Steuerelements `MailDefinition` Eigenschaft gibt nur Informationen über die e-Mail-Nachricht, die gesendet wird, wenn ein neues Konto erstellt wird. Er umfasst keine Einzelheiten darüber, wie die e-Mail-Nachricht tatsächlich gesendet wird (d. h., ob ein SMTP-Server oder die e-Mail-Ablage-Verzeichnis verwendet wird, Authentifizierungsinformationen, usw.). Diese Details auf niedriger Ebene in definiert werden, müssen die `<system.net>` im Abschnitt `Web.config`. Weitere Informationen zu diesen Konfigurationseinstellungen und für das Senden von e-Mails von ASP.NET 2.0 ist es im Allgemeinen finden Sie in der [häufig gestellte Fragen, am SystemNetMail.com](http://www.systemnetmail.com/) und meinem Artikel [Senden von e-Mail-Adresse in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Erweitern Sie die CreateUserWizard Verhalten mithilfe von Ereignishandlern

Das Steuerelement CreateUserWizard löst eine Anzahl von Ereignissen während der Workflows aus. Nachdem ein Besucher eingegeben werden, ihren Benutzernamen, Kennwort und andere relevante Informationen und klickt auf die Schaltfläche "Benutzer erstellen", Steuerelement CreateUserWizard so löst z. B. die [ `CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Liegt ein Problem während des Prozesses erstellen die [ `CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst wird; jedoch, wenn der Benutzer erfolgreich erstellt wurde, und klicken Sie dann die [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst wird. Es gibt zusätzliche CreateUserWizard-Steuerelement-Ereignisse, die ausgelöst wird, erhalten, aber dies sind die drei am häufigsten von Belang.

In bestimmten Szenarien können wir in den Workflow CreateUserWizard, tippen Sie auf die wir tun können, indem Sie einen Ereignishandler für das entsprechende Ereignis erstellen möchten. Um dies zu veranschaulichen, und erweitern wir jetzt die `RegisterUser` Steuerelement CreateUserWizard sollen einige benutzerdefinierte Validierung für den Benutzernamen und Kennwort. Insbesondere erweitern wir jetzt unsere CreateUserWizard, damit Benutzernamen darf keine führende oder nachgestellte Leerzeichen enthalten und darf nicht der Benutzernamen an einer beliebigen Stelle in das Kennwort. Kurz gesagt, möchten wir aus einen Benutzernamen wie "Scott" oder zu erstellen, müssen eine Kombination Benutzername/Kennwort, wie Scott und Scott.1234 zu vermeiden.

Um dies zu erreichen, wir einen Ereignishandler für erstellen, die `CreatingUser` Ereignis, um unsere zusätzliche Überprüfungen auszuführen. Wenn die angegebenen Daten nicht gültig ist müssen wir den Erstellungsprozess abzubrechen. Wir müssen auch ein Label-Steuerelement hinzufügen, auf der Seite zum Anzeigen einer Meldung darüber informiert, dass der Benutzername oder Kennwort ungültig ist. Starten, indem ein Label-Steuerelement unterhalb des Steuerelements CreateUserWizard, Festlegen von Hinzufügen der `ID` Eigenschaft `InvalidUserNameOrPasswordMessage` und die zugehörige `ForeColor` Eigenschaft `Red`. Löschen Sie die `Text` Eigenschaft, und legen dessen `EnableViewState` und `Visible` Eigenschaften auf "false".

[!code-aspx[Main](creating-user-accounts-vb/samples/sample7.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für das Steuerelement CreateUserWizard `CreatingUser` Ereignis. Um einen Ereignishandler erstellen möchten, wählen Sie das Steuerelement im Designer, und fahren Sie mit dem Fenster "Eigenschaften". Klicken Sie von dort aus auf das Blitzsymbol, und doppelklicken Sie dann auf das entsprechende Ereignis aus, um den Ereignishandler zu erstellen.

Fügen Sie dem `CreatingUser`-Ereignishandler folgenden Code hinzu:

[!code-vb[Main](creating-user-accounts-vb/samples/sample8.vb)]

Beachten Sie, dass Benutzername und Kennwort eingegeben werden, in das Steuerelement CreateUserWizard über seine [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) und [ `Password` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx)bzw. Wir verwenden diese Eigenschaften in den oben genannten-Ereignishandler aus, um zu bestimmen, ob der angegebene Benutzername führende oder nachgestellte Leerzeichen enthält, und gibt an, ob der Benutzername gefunden wird, in das Kennwort. Wenn eine der folgenden Bedingungen erfüllt sind, wird eine Fehlermeldung angezeigt, der `InvalidUserNameOrPasswordMessage` Bezeichnung und des ereignishandlers `e.Cancel` -Eigenschaftensatz auf `True`. Wenn `e.Cancel` nastaven NA hodnotu `True`, die CreateUserWizard wird dessen Workflow, verkürzt wird effektiv der kontoerstellung für Benutzer abgebrochen.

Abbildung 15 zeigt einen Screenshot des `CreatingUserAccounts.aspx` bei der Eingabe eines Benutzernamens mit führenden Leerzeichen.


[![Benutzernamen bei führende oder nachfolgende Leerzeichen sind nicht zulässig.](creating-user-accounts-vb/_static/image44.png)](creating-user-accounts-vb/_static/image43.png)

**Abbildung 15**: Benutzernamen bei führende oder nachfolgende Leerzeichen sind nicht zulässig ([klicken Sie, um das Bild in voller Größe anzeigen](creating-user-accounts-vb/_static/image45.png))


> [!NOTE]
> Sehen wir ein Beispiel der Verwendung des Steuerelements CreateUserWizard `CreatedUser` Ereignis in der *<a id="_msoanchor_11"></a>[zusätzliche Benutzerinformationen speichern](storing-additional-user-information-vb.md)* Lernprogramm.


## <a name="summary"></a>Zusammenfassung

Die `Membership` Klasse `CreateUser` Methode erstellt ein neues Benutzerkonto in das mitgliedschaftsframework. Dies erfolgt durch den Aufruf an den konfigurierten Mitgliedschaftsanbieter delegieren. Im Fall von der `SqlMembershipProvider`, `CreateUser` Methode fügt einen Datensatz, um die `aspnet_Users` und `aspnet_Membership` Datenbanktabellen.

Während neue Benutzerkonten erstellt werden können, programmgesteuert (wie in Schritt 5 beschrieben), ist ein schneller und einfacher Ansatz, Steuerelement CreateUserWizard verwenden. Dieses Steuerelement wird eine mehrstufiger-Benutzeroberfläche für das Sammeln von Benutzerinformationen und das Erstellen eines neuen Benutzers in das mitgliedschaftsframework gerendert. Dieses Steuerelement im Hintergrund verwendet die gleichen `Membership.CreateUser` Methode wie in Schritt 5, aber das Steuerelement überprüft, erstellt die Benutzeroberfläche, die Validierungssteuerelemente, und reagiert auf Fehler bei der kontoerstellung Benutzer, ohne jeglichen Code schreiben zu müssen.

An diesem Punkt haben wir die Funktion einrichten, um neue Benutzerkonten erstellen. Die Anmeldeseite ist jedoch weiterhin für diese Anmeldeinformationen hartcodierte überprüfen, die wir in der zweiten Tutorial angegeben. In der <a id="_msoanchor_12"> </a> [nächsten Tutorial](validating-user-credentials-against-the-membership-user-store-vb.md) aktualisieren wir `Login.aspx` zur Überprüfung des Benutzers die Anmeldeinformationen für das mitgliedschaftsframework angegeben.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [`CreateUser` Technische Dokumentation](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Übersicht über die Steuerelement CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Erstellen einen Datei systembasierte Siteübersichtsanbieter](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Erstellen eine assistentenartige Benutzeroberfläche mit dem ASP.NET 2.0-Assistenten-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Untersuchen von ASP.NET 2.0 die Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Masterseiten und Sitenavigation](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [Der SQL-SiteMapProvider, die Sie gewartet haben](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Zurück](creating-the-membership-schema-in-sql-server-vb.md)
> [Weiter](validating-user-credentials-against-the-membership-user-store-vb.md)
