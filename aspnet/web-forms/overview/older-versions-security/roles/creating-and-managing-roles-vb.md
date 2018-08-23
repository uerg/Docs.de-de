---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: Erstellen und Verwalten von Rollen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework. Danach werden wir Webseiten zum Erstellen und Löschen Rollen erstellen.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: e51fa6de3d2fe7b5c9cd84900d154070eb1960b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826521"
---
<a name="creating-and-managing-roles-vb"></a>Erstellen und Verwalten von Rollen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework. Danach werden wir Webseiten zum Erstellen und Löschen Rollen erstellen.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Tutorial wir mithilfe von URL-Autorisierung auf gewisse Benutzer beschränken, aus einer Reihe von Seiten betrachtet und untersucht die deklarative und programmgesteuerte Verfahren für die Anpassung einer ASP.NET-Seite-Funktionen, die basierend auf den Benutzer aufgerufen. Gewähren der Berechtigung für den Zugriff auf oder Funktionen pro Benutzer mit dem Benutzernamen, kann jedoch ein verwaltungsalbtraum in Szenarien werden, wobei viele Benutzerkonten vorhanden sind, oder wenn Benutzer Berechtigungen häufig ändern. Jedes Mal ein Benutzer erhält oder verliert der Autorisierung für eine bestimmte Aufgabe auszuführen, muss der Administrator die entsprechenden Regeln auf URL-Autorisierung, deklaratives Markup und Code zu aktualisieren.

Es hilft Ihnen bei der um Benutzer in Gruppen zu klassifizieren oder *Rollen* und Anwenden von Berechtigungen pro Rolle-von-Rolle. Die meisten Webanwendungen haben z. B. einen bestimmten Satz von Seiten oder Aufgaben, die nur für Administratoren reserviert sind. Die Verfahren verwenden, Erkenntnisse in die *Benutzerbasierte Autorisierung* Tutorial, würden wir hinzufügen, die entsprechende URL-Autorisierungsregeln, deklaratives Markup und Code für die angegebenen Benutzerkonten administrative Aufgaben ausführen können. Aber wenn ein neuer Administrator hinzugefügt wurde oder ein bestehenden Administrator erforderlich, damit ihre Administratorrechte widerrufen, müssten wir zurück, und aktualisieren die Konfigurationsdateien und Webseiten. Mit Rollen konnten wir allerdings erstellen Sie eine Rolle namens Administratoren und weisen diese vertrauenswürdigen Benutzer die Rolle "Administratoren". Als Nächstes fügen wir die entsprechende URL-Autorisierungsregeln, deklaratives Markup und Code zu der Rolle "Administratoren" für die verschiedenen Verwaltungsaufgaben hinzu. Mit dieser Infrastruktur vorhanden ist ist der Website neue Administratoren hinzufügen oder Entfernen von vorhandenen so einfach wie das ein- oder Entfernen des Benutzers aus der Rolle "Administratoren". Es sind keine Konfiguration, deklaratives Markup oder Änderungen am Code erforderlich.

ASP.NET bietet ein Rollen-Framework zum Definieren von Rollen, und mit Benutzerkonten verknüpft werden. Fügen Sie Benutzer oder Benutzer aus einer Rolle entfernen, bestimmen die Gruppe von Benutzern, die zu einer bestimmten Rolle gehören, und feststellen, ob ein Benutzer einer bestimmten Rolle angehört, mit dem Rollen-Framework, das können wir erstellen und Löschen von Rollen. Nachdem das Framework für die Rollen konfiguriert wurde, können wir beschränken den Zugriff auf Seiten pro Rolle-von-Role durch URL-Autorisierungsregeln und ein- oder ausblenden Zusatzinformationen oder Funktionen auf einer Seite, die basierend auf Rollen des angemeldeten Benutzers.

In diesem Tutorial werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework. Danach werden wir Webseiten zum Erstellen und Löschen Rollen erstellen. In der <a id="_msoanchor_2"> </a> [ *Zuweisen von Rollen an Benutzer* ](assigning-roles-to-users-vb.md) Tutorial betrachten wir das Hinzufügen und Entfernen von Benutzern Rollen. Und klicken Sie in der <a id="_msoanchor_3"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Tutorial wird gezeigt, wie mit den Zugriff auf Seiten pro Rolle-von-Rolle sowie Gewusst wie: Anpassen der Seite auf Funktionalität je beschränken Klicken Sie auf die Website besucht Rolle des Benutzers. Fangen wir an!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von ASP.NET-Seiten

In diesem Tutorial und die nächsten beiden werden wir verschiedene Funktionen im Zusammenhang mit Rollen und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in diesen Tutorials untersucht implementieren. Diese Seiten zu erstellen, und die Sitemap zu aktualisieren.

Zunächst erstellen Sie einen neuen Ordner im Projekt mit dem Namen `Roles`. Fügen Sie vier neue ASP.NET-Seiten, die `Roles` Ordner, verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten ein:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

An diesem Punkt sollte Ihr Projekts des Projektmappen-Explorer im Screenshot dargestellt in Abbildung 1 ähneln.


[![Vier neue Seiten wurden in den Ordner Rollen hinzugefügt](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**Abbildung 1**: vier neue Seiten hinzugefügt wurden, die `Roles` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image3.png))


Jede Seite, an diesem Punkt müssen die beiden Inhaltssteuerelemente, einer für jede von der Masterseite ContentPlaceHolder-Steuerelemente: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

Bedenken Sie, dass die `LoginContent` ContentPlaceHolder Standard-Markup zeigt einen Link zum Anmelden oder Abmelden auf den Standort, je nachdem, ob der Benutzer authentifiziert wird. Das Vorhandensein der `Content2` Content-Steuerelement auf der ASP.NET-Seite wird jedoch die Masterseite Standardmarkup überschreibt. Wie wir unter <a id="_msoanchor_4"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) überschreiben das Standardmarkup Tutorial eignet sich für Seiten, in dem wir nicht anzeigen anmeldebezogene möchten, die Optionen in der linken Spalte.

Für diese vier Seiten, allerdings möchten wir zum Anzeigen der Masterseite Standardmarkup für das `LoginContent` ContentPlaceHolder. Daher entfernen deklarative Markup für die `Content2` Inhaltssteuerelement. Danach sollte jede der vier Seitenmarkup nur ein Inhaltssteuerelement enthalten.

Zum Schluss aktualisieren wir die Sitemap (`Web.sitemap`) sollen diese neue Webseiten. Die folgenden XML-Code nach dem Hinzufügen der `<siteMapNode>` wir für die Mitgliedschaft Lernprogramme hinzugefügt.

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

Besuchen Sie die Website über einen Browser, mit der sitezuordnung aktualisiert. Wie in Abbildung 2 gezeigt, enthält die Navigation auf der linken Seite nun Elemente für die Rollen-Lernprogramme.


[![Vier neue Seiten wurden in den Ordner Rollen hinzugefügt](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**Abbildung 2**: vier neue Seiten hinzugefügt wurden, die `Roles` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Schritt 2: Angeben, und konfigurieren den Rollen-Framework-Datenanbieter

Wie das mitgliedschaftsframework wird das Framework für die Rollen auf dem Anbietermodell erstellt. Siehe die <a id="_msoanchor_5"> </a> [ *Grundlagen der Sicherheit und Unterstützung für ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) Tutorial .NET Framework ausgeliefert wird, mit drei integrierte Rollen: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), und [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Diese tutorialreihe konzentriert sich auf die `SqlRoleProvider`, die Microsoft SQL Server-Datenbank als rollenspeicher verwendet.

Unter der Hauptebene die Rollen-Framework und `SqlRoleProvider` genau wie das mitgliedschaftsframework Geschäfts- und `SqlMembershipProvider`. Das .NET Framework enthält eine `Roles` Klasse, die als API zu Rollen Framework fungiert. Die `Roles` Klasse verfügt über Methoden wie freigegebene `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`und so weiter. Wenn eine der folgenden Methoden aufgerufen wird, die `Roles` Klasse der Aufruf an den konfigurierten Anbieter delegiert. Die `SqlRoleProvider` arbeitet mit den Tabellen rollenspezifische (`aspnet_Roles` und `aspnet_UsersInRoles`) in der Antwort.

Zum Verwenden der `SqlRoleProvider` Anbieter in unserer Anwendung, müssen wir angeben, welche Datenbank als Speicher verwendet. Die `SqlRoleProvider` erwartet, dass die angegebene Rolle Store bestimmte Datenbanktabellen, Ansichten und gespeicherte Prozeduren haben. Diese erforderlichen Datenbankobjekte können hinzugefügt werden, mithilfe der [ `aspnet_regsql.exe` Tool](https://msdn.microsoft.com/library/ms229862.aspx). An diesem Punkt haben wir bereits eine Datenbank mit dem Schema für die `SqlRoleProvider`. In der <a id="_msoanchor_6"> </a> [ *Erstellen des Mitgliedschaftsschemas in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) Tutorial, die wir erstellt, eine Datenbank namens haben `SecurityTutorials.mdf` und `aspnet_regsql.exe` um die Anwendung hinzuzufügen Dienste, die die erforderlichen Datenbankobjekte enthalten die `SqlRoleProvider`. Aus diesem Grund Es muss lediglich das Rollen-Framework, um Unterstützung zu aktivieren und verwenden, teilen die `SqlRoleProvider` mit der `SecurityTutorials.mdf` Datenbank als rollenspeicher.

Das Framework für die Rollen konfiguriert ist, über die `<roleManager>` Element in der Anwendung `Web.config` Datei. Standardmäßig ist die rollenunterstützung deaktiviert. Um es zu aktivieren, müssen Sie festlegen der [ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx) des Elements `enabled` Attribut `true` wie folgt:

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

Standardmäßig alle Webanwendungen verfügen über einen Rollenanbieter, der mit dem Namen `AspNetSqlRoleProvider` des Typs `SqlRoleProvider`. In diesem Standardanbieter registriert ist `machine.config` (am `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

Des Anbieters `connectionStringName` Attribut gibt an, die Rolle Store, die verwendet wird. Die `AspNetSqlRoleProvider` -Anbieter legt die dieses Attribut auf `LocalSqlServer`, ebenfalls definiert `machine.config` und zeigt, wird standardmäßig mit einer SQL Server 2005 Express Edition-Datenbank in der `App_Data` Ordner mit dem Namen `aspnet.mdf`.

Daher, wenn wir einfach das Rollen-Framework ermöglichen, ohne alle Anbieterinformationen in der Anwendung `Web.config` -Datei, die Anwendung verwendet, den registriert Rollen Standardanbieter `AspNetSqlRoleProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank ist nicht vorhanden, die ASP.NET-Laufzeit automatisch erstellt, und fügen Sie das Schema der Application-Dienste. Allerdings möchten wir verwenden die `aspnet.mdf` Datenbank; stattdessen verwendet werden soll die `SecurityTutorials.mdf` -Datenbank, die wir bereits erstellt und die Anwendung Dienstschema in hinzugefügt haben. Diese Änderung kann auf zwei Arten erfolgen:

- <strong>Geben Sie einen Wert für die</strong><strong>`LocalSqlServer`</strong><strong>Namen der Verbindungszeichenfolge der</strong><strong>`Web.config`</strong><strong>.</strong> Durch Überschreiben der `LocalSqlServer` Name für Verbindungszeichenfolgenwert in `Web.config`, verwenden wir den Standardanbieter für registrierte Rollen (`AspNetSqlRoleProvider`), damit er ordnungsgemäß funktioniert mit den `SecurityTutorials.mdf` Datenbank. Weitere Informationen zu dieser Technik finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)Blogbeitrag [Konfigurieren von ASP.NET 2.0-Anwendungsdienste für Verwenden von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Fügen Sie einen neuen registrierten Anbieter vom Typ</strong><strong>`SqlRoleProvider`</strong><strong>und konfigurieren Sie die</strong><strong>`connectionStringName`</strong><strong>Einstellung aus, um auf dieverweisen</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank.</strong> Dies ist der Ansatz ich empfohlen und verwendet die <a id="_msoanchor_7"> </a> [ *Erstellen des Mitgliedschaftsschemas in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) und ist der Ansatz, die ich verwende in diesem Tutorial auch.

Fügen Sie das folgende konfigurationsmarkup von Rollen, um die `Web.config` Datei. Dieses Markup wird einen neuen Anbieter mit dem Namen registriert. `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

Das obenstehende Markup definiert die `SecurityTutorialsSqlRoleProvider` als Standardanbieter (über die `defaultProvider` -Attribut in der `<roleManager>` Element). Außerdem wird die `SecurityTutorialsSqlRoleProvider`des `applicationName` auf `SecurityTutorials`, weist die gleichen `applicationName` wie für den Mitgliedschaftsanbieter (`SecurityTutorialsSqlMembershipProvider`). Obwohl hier nicht angezeigt, die [ `<add>` Element](https://msdn.microsoft.com/library/ms164662.aspx) für die `SqlRoleProvider` enthält möglicherweise auch eine `commandTimeout` Attribut, das Datenbank-Zeitlimit in Sekunden an. Der Standardwert ist 30.

Mit diesem konfigurationsmarkup vorhanden können wir zum Einstieg rollenfunktion in unserer Anwendung.

> [!NOTE]
> Das obenstehende konfigurationsmarkup wird die Verwendung der `<roleManager>` des Elements `enabled` und `defaultProvider` Attribute. Es gibt eine Reihe von anderen Attributen, die beeinflussen, wie das Framework Rollen Rolleninformationen pro Benutzer mit dem Benutzernamen zuordnet. Wir werden diese Einstellungen im Untersuchen der <a id="_msoanchor_8"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Tutorial.


## <a name="step-3-examining-the-roles-api"></a>Schritt 3: Untersuchen der Rollen-API.

Die Rollen-Framework Funktionen werden verfügbar gemacht, über die [ `Roles` Klasse](https://msdn.microsoft.com/library/system.web.security.roles.aspx), die dreizehn freigegebene Methoden zum Ausführen von Vorgängen rollenbasierte enthält. Wenn wir sehen uns erstellen und Löschen von Rollen in den Schritte4 und 6 werden verwendet, die [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) und [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) -Methoden, die hinzufügen oder entfernen eine Rolle aus dem System.

Um eine Liste aller Rollen im System abzurufen, verwenden die [ `GetAllRoles` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (siehe Schritt 5). Die [ `RoleExists` Methode](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) gibt einen booleschen Wert, der angibt, ob eine angegebene Rolle vorhanden ist.

Im nächsten Tutorial untersuchen wir zum Zuordnen von Benutzern zu Rollen. Die `Roles` Klasse [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), und [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) Methoden hinzufügen eine oder mehrere Benutzer eine oder mehrere Rollen. Um Benutzern Rollen zu entfernen, verwenden die [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), oder [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) -Methoden.

In der <a id="_msoanchor_9"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Tutorial betrachten wir Methoden zum programmgesteuerten ein- oder Ausblenden von Funktionen anhand der Rolle des aktuell angemeldeten Benutzers. Zu diesem Zweck verwenden wir die rollenklasse [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), oder [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) Methoden.

> [!NOTE]
> Denken Sie daran, die einem beliebigen Zeitpunkt eine der folgenden Methoden aufgerufen wird, die `Roles` Klasse der Aufruf an den konfigurierten Anbieter delegiert. In unserem Fall ist dies bedeutet, dass der Aufruf an gesendet werden die `SqlRoleProvider`. Die `SqlRoleProvider` führt dann die entsprechende Datenbank-Vorgang basierend auf der aufgerufenen Methode. Z. B. den Code `Roles.CreateRole("Administrators")` führt die `SqlRoleProvider` Ausführen der `aspnet_Roles_CreateRole` gespeicherte Prozedur, die Fügt einen neuen Datensatz in die `aspnet_Roles` Tabelle, die mit dem Namen Administrators.


Prüft, dass der Rest dieses Tutorials mit der `Roles` Klasse `CreateRole`, `GetAllRoles`, und `DeleteRole` Methoden, um die Rollen im System zu verwalten.

## <a name="step-4-creating-new-roles"></a>Schritt 4: Erstellen von neuen Rollen

Rollen bieten eine Möglichkeit, Benutzer nach dem Zufallsprinzip aus, und diese Gruppierung wird am häufigsten für eine praktischere Möglichkeit, Sie gelten die Autorisierungsregeln verwendet. Sondern um Rollen als ein Autorisierungsmechanismus für die zu verwenden, müssen wir zuerst definieren, welche Rollen in der Anwendung vorhanden sind. Leider ist ein CreateRoleWizard-Steuerelement von ASP.NET nicht enthalten. Um neue Rollen hinzuzufügen, müssen wir eine geeignete Benutzeroberfläche und Aufrufen der API für die Rollen selbst. Die gute Nachricht ist, dass dies sehr einfach zu erreichen ist.

> [!NOTE]
> Es gibt zwar keine CreateRoleWizard-Websteuerelement, besteht die [ASP.NET Web Site Administration Tool](https://msdn.microsoft.com/library/ms228053.aspx), d.h. einer lokalen ASP.NET-Anwendung entwickelt, um Unterstützung bei der Anzeige und Verwaltung der Konfiguration für Ihre Webanwendung. Ich bin jedoch nicht aus zwei Gründen ein großer Fan von der ASP.NET Web Site Administration Tool. Zunächst ist es ein wenig fehlerhaften und die Benutzeroberfläche verlässt, viel zu wünschen übrig. Andererseits ist das ASP.NET Websiteverwaltungs-Tool ausgelegt nur lokal bedeutet, dass Sie Ihre eigene Rolle Management-Webseiten erstellen, wenn Sie zur Remoteverwaltung von Rollen auf einer Livewebsite müssen hat. Aus diesen zwei Gründen in diesem Tutorial und dem nächsten konzentriert sich auf die Rolle "erforderlich"-Verwaltungstools auf einer Webseite erstellen, statt Sie zu der das ASP.NET Websiteverwaltungs-Tool.


Öffnen der `ManageRoles.aspx` auf der Seite die `Roles` Ordner, und fügen Sie einem Textfeld und einer Schaltfläche-Websteuerelement zur Seite hinzu. Legen Sie das Textfeld-Steuerelement `ID` Eigenschaft `RoleName` und der Schaltfläche `ID` und `Text` Eigenschaften `CreateRoleButton` und die Rolle zu erstellen bzw. An diesem Punkt sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

Doppelklicken Sie dann auf die `CreateRoleButton` Schaltflächen-Steuerelement im Designer zum Erstellen einer `Click` -Ereignishandler und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

Der obige Code beginnt durch Zuweisen der verkürzten Rollenname eingegeben haben, der `RoleName` Textfeldern nicht das für die `newRoleName` Variable. Als Nächstes die `Roles` Klasse `RoleExists` aufgerufen, um zu bestimmen, ob die Rolle `newRoleName` bereits im System vorhanden ist. Wenn die Rolle nicht vorhanden ist, wird er erstellt, über einen Aufruf an die `CreateRole` Methode. Wenn die `CreateRole` -Methode übergeben den Namen einer Rolle, die bereits im System vorhanden ist. eine `ProviderException` Ausnahme ausgelöst. Daher ist der Code zunächst prüft, um sicherzustellen, dass die Rolle im System vor dem Aufruf nicht bereits vorhanden ist `CreateRole`. Die `Click` Ereignishandler abgeschlossen wird, indem Sie Sie deaktivieren die `RoleName` Textfeldss `Text` Eigenschaft.

> [!NOTE]
> Vielleicht was passiert, wenn der Benutzer einen beliebigen Wert in eingeben, nicht die `RoleName` Textfeld. Wenn der übergebene Wert. in der `CreateRole` Methode `Nothing` oder eine leere Zeichenfolge ist, eine Ausnahme ausgelöst. Ebenso, wenn der Name der Rolle ein Komma enthält wird eine Ausnahme ausgelöst. Die Seite sollte daher Validierungssteuerelementen, um sicherzustellen, dass der Benutzer eine Rolle gibt und sie keine Kommas enthält enthalten. Ich lassen Sie für den Leser als Übung.


Wir erstellen eine Rolle mit dem Namen Administrators ein. Besuchen Sie die `ManageRoles.aspx` Seite über einen Browser, und geben Sie in der Administratoren, in das Textfeld ein (siehe Abbildung 3), und klicken Sie dann auf die Schaltfläche "Create Role".


[![Erstellen Sie eine Rolle "Administratoren"](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**Abbildung 3**: Erstellen Sie eine Rolle "Administratoren" ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image9.png))


Was ist los? Ein Postback auftritt, aber es gibt keinen visuellen Hinweis, der die Rolle tatsächlich wurde dem System hinzugefügt. Wir aktualisieren diese Seite in Schritt 5 visuelles Feedback einschließen. Im Moment jedoch überprüfen, ob die Rolle erstellt wurde, indem Sie auf die `SecurityTutorials.mdf` -Datenbank und zum Anzeigen der Daten aus der `aspnet_Roles` Tabelle. Wie in Abbildung 4 gezeigt, die `aspnet_Roles` Tabelle enthält einen Datensatz für den gerade hinzugefügten Administratorrollen.


[![Die Aspnet_Roles Tabelle enthält eine Zeile für Administratoren](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**Abbildung 4**: die `aspnet_Roles` Tabelle enthält eine Zeile für die Administratoren ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Schritt 5: Anzeigen von Rollen im System

Wir erweitern die `ManageRoles.aspx` Seite, um eine Liste der aktuellen Rollen im System enthalten. Um dies zu erreichen, fügen Sie ein GridView-Steuerelement auf der Seite, und legen Sie dessen `ID` Eigenschaft `RoleList`. Fügen Sie eine Methode auf der Seite des Code-Behind-Klasse, die mit dem Namen `DisplayRolesInGrid` mithilfe des folgenden Codes:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

Die `Roles` Klasse `GetAllRoles` Methodenrückgabe aller Rollen im System als ein Array von Zeichenfolgen. Diese Zeichenfolgen-Array wird anschließend an die GridView gebunden. Um die Liste der Rollen an die GridView binden, beim ersten die Seite laden, müssen wir rufen die `DisplayRolesInGrid` Methode aus der Seite `Page_Load` -Ereignishandler. Der folgende Code ruft diese Methode auf, wenn die Seite zuerst aufgerufen wird, aber nicht bei nachfolgenden Postbacks.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

Finden Sie mit diesem Code werden auf der Seite über einen Browser. Wie in Abbildung 5 gezeigt, sehen Sie ein Raster mit einer einzelnen Spalte mit der Bezeichnung Element. Das Raster enthält eine Zeile für die Rolle "Administratoren", die in Schritt 4 hinzugefügt wurden.


[![GridView zeigt die Rollen in einer einzelnen Spalte an.](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**Abbildung 5**: GridView zeigt die Rollen in einer einzelnen Spalte ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image15.png))


GridView zeigt eine einzelne Spalte mit dem Element bezeichnet, weil der GridView `AutoGenerateColumns` -Eigenschaftensatz auf "true" (Standard), wodurch die GridView automatisch eine Spalte für jede Eigenschaft im Erstellen der `DataSource`. Ein Array verfügt über eine einzelne Eigenschaft, die Elemente im Array, daher die einzelne Spalte in den GridView-Ansicht darstellt.

Wenn Sie Daten mit einer GridView-Ansicht anzeigen, bevorzuge ich explizit definieren Meine Kolumnen statt sie implizit von der GridView generiert. Durch die Spalten explizit zu definieren ist es einfacher, formatieren Sie die Daten, die Spalten neu anordnen und andere häufige Aufgaben ausführen. Aus diesem Grund aktualisieren wir den GridView deklarative Markup, damit die Spalten explizit definiert sind.

Starten, indem Sie des GridView `AutoGenerateColumns` Eigenschaft auf "false". Fügen Sie ein TemplateField zum Raster, legen Sie als Nächstes die `HeaderText` Eigenschaft zuweisen, und konfigurieren Sie die `ItemTemplate` , damit sie den Inhalt des Arrays angezeigt. Zu diesem Zweck fügen Sie ein Label-Steuerelement mit dem Namen `RoleNameLabel` auf die `ItemTemplate` und binden die `Text` Eigenschaft `Container.DataItem.`

Diese Eigenschaften und die `ItemTemplate`des Inhalts können deklarativ oder über den GridView Felder (Dialogfeld) und Vorlagen bearbeiten-Schnittstelle festgelegt werden. Um die Felder im Dialogfeld gelangen, klicken Sie auf den Link "Spalten bearbeiten" in des GridView Smarttag. Als Nächstes, deaktivieren Sie das Kontrollkästchen des Automatisches Generieren von-Felder festlegen der `AutoGenerateColumns` Eigenschaft auf "false", und fügen Sie ein TemplateField an die GridView, Festlegen der `HeaderText` Eigenschaft, um die Rolle. Zum Definieren der `ItemTemplate`Inhalt, wählen Sie die Vorlagen bearbeiten-Option von GridView Smarttag. Ziehen Sie ein Label-Steuerelement auf der `ItemTemplate`legen die `ID` Eigenschaft `RoleNameLabel`, und seine Databinding-Einstellungen zu konfigurieren, damit die `Text` Eigenschaft gebunden ist `Container.DataItem`.

Unabhängig davon, welche Methode Sie verwenden, sieht GridView resultierende deklarative Markup etwa wie folgt, wenn Sie fertig sind.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> Inhalt des Arrays werden angezeigt, mit der Datenbindungssyntax `<%# Container.DataItem %>`. Eine ausführliche Beschreibung der warum diese Syntax verwendet wird, beim Anzeigen des Inhalts eines Arrays an die GridView gebunden ist, würde den Rahmen dieses Tutorials. Weitere Informationen zu dieser Angelegenheit, finden Sie unter [binden ein Array von skalaren auf ein Websteuerelement Daten](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Derzeit den `RoleList` GridView ist nur an die Liste der Rollen gebunden, wenn zuerst die Seite besucht wird. Wir müssen das Raster zu aktualisieren, wenn eine neue Rolle hinzugefügt wird. Aktualisieren Sie zu diesem Zweck die `CreateRoleButton` Schaltfläche `Click` Ereignishandler aufruft und die It die `DisplayRolesInGrid` Methode, wenn eine neue Rolle erstellt wird.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

Jetzt bei der Benutzer fügt eine neue Rolle der `RoleList` GridView zeigt die gerade hinzugefügte Rolle beim Postback, Bereitstellen von visuellem Feedback, dass die Rolle erfolgreich erstellt wurde. Um dies zu veranschaulichen, finden Sie auf die `ManageRoles.aspx` Seite über einen Browser, und fügen Sie eine Rolle mit dem Namen Vorgesetzte. Nach dem Klicken auf die Schaltfläche "Create Role", wird ein Postback zur Folge haben, und das Raster wird aktualisiert, und Administratoren als auch für die neue Rolle, Supervisor gehören.


[![Der Supervisor-Rolle zugewiesen wurde, wurde hinzugefügt](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**Abbildung 6**: die Rolle der Vorgesetzte wurde hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Schritt 6: Löschen von Rollen

An diesem Punkt ein Benutzer eine neue Rolle erstellen kann, und zeigen Sie alle vorhandene Rollen aus der `ManageRoles.aspx` Seite. Wir können Benutzer auch Rollen löschen. Die `Roles.DeleteRole` Methode verfügt über zwei Überladungen:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Löscht die Rolle *RoleName*. Eine Ausnahme wird ausgelöst, wenn die Rolle ein oder mehrere Elemente enthält.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Löscht die Rolle *RoleName*. Wenn *ThrowOnPopulateRole* ist `True`, und klicken Sie dann eine Ausnahme ausgelöst wird, wenn die Rolle ein oder mehrere Elemente enthält. Wenn *ThrowOnPopulateRole* ist `False`, und klicken Sie dann die Rolle gelöscht wird, ob sie alle Elemente oder nicht enthält. Intern wird die `DeleteRole(roleName)` Methodenaufrufe `DeleteRole(roleName, True)`.

Die `DeleteRole` Methode löst außerdem eine Ausnahme aus, wenn *RoleName* ist `Nothing` oder eine leere Zeichenfolge oder, wenn *RoleName* enthält ein Komma. Wenn *RoleName* ist nicht im System `DeleteRole` fehlschlägt, im Hintergrund, ohne eine Ausnahme auszulösen.

Wir erweitern die GridView in `ManageRoles.aspx` sollen eine Löschen-Schaltfläche geklickt wird, löscht die ausgewählte Rolle. Starten Sie, indem eine Löschen-Schaltfläche an die GridView hinzufügen, indem Sie die Felder (Dialogfeld) und Hinzufügen einer Schaltfläche "löschen", der sich unter der Option "CommandField" befindet. Stellen Sie das Löschen die Spalte ganz linke Schaltfläche, und legen Sie dessen `DeleteText` Eigenschaft, um die Rolle zu löschen.


[![Hinzufügen einer Schaltfläche "löschen" an die RoleList GridView](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**Abbildung 7**: Hinzufügen einer Schaltfläche löschen, um die `RoleList` GridView ([klicken Sie, um das Bild in voller Größe anzeigen](creating-and-managing-roles-vb/_static/image21.png))


Nach dem Hinzufügen der Schaltfläche "löschen", sollte Ihre GridView deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für der GridView `RowDeleting` Ereignis. Dies ist das Ereignis, das beim Postback ausgelöst wird, wenn die Rolle löschen-Schaltfläche geklickt wird. Fügen Sie dem Ereignishandler folgenden Code hinzu.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

Der Code beginnt, durch Programmgesteuertes Verweisen auf die `RoleNameLabel` Webserver-Steuerelement in der Zeile, deren Rolle löschen-Schaltfläche geklickt wurde. Die `Roles.DeleteRole` Methode wird dann aufgerufen, übergibt die `Text` von der `RoleNameLabel` und `False`, wodurch löschen Sie die Rolle, unabhängig davon, ob alle Benutzer der Rolle zugeordnet sind. Zum Schluss die `RoleList` GridView wird aktualisiert, damit die Rolle nur gelöschte nicht mehr im Raster angezeigt wird.

> [!NOTE]
> Die Schaltfläche "Löschen der Rolle" erfordert ein beliebiges Bestätigung durch den Benutzer vor dem Löschen der Rolle nicht. Eine der einfachsten Möglichkeiten, eine Aktion zu bestätigen ist über ein Dialogfeld für die clientseitige bestätigen. Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen der clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


## <a name="summary"></a>Zusammenfassung

Viele Webanwendungen verfügen über bestimmte Autorisierungsregeln oder auf Seitenebene-Funktionen, die nur bestimmte Klassen von Benutzern zur Verfügung steht. Möglicherweise gibt es z. B. ein Satz von Webseiten, die nur Administratoren zugreifen können. Anstatt definieren diese Autorisierungsregeln pro Benutzer mit dem Benutzernamen, ist es häufig sinnvoller, die Regeln basierend auf einer Rolle zu definieren. Also anstatt explizit ermöglicht Benutzern Scott und Jisun auf die administrative Seiten zugreifen, ein besser verwaltbaren Ansatz sind Mitglieder der Rolle "Administratoren" Zugriff auf diese Seiten, und klicken Sie dann auf, um anzugeben, Scott und Jisun als Benutzer, die die Rolle "Administratoren".

Die Rollen-Framework erleichtert das Erstellen und Verwalten von Rollen. In diesem Tutorial wurde beschrieben, wie so konfigurieren Sie die Rollen-Framework mit der `SqlRoleProvider`, die Microsoft SQL Server-Datenbank als rollenspeicher verwendet. Es wurde außerdem eine Webseite, die Listet die vorhandenen Rollen im System, und ermöglicht neue Rollen erstellt werden soll, und vorhandene gelöscht werden soll. In den nachfolgenden Tutorials wird zum Zuweisen von Benutzern zu Rollen und zum Anwenden der rollenbasierten Autorisierung angezeigt.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Gewusst wie: Verwenden von Rollen-Manager in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Rollenanbieter](https://msdn.microsoft.com/library/aa478950.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Technische Dokumentation für die `<roleManager>` Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Verwenden die Mitgliedschaft und Rollen-Manager-APIs](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial enthalten Alicja Maziarz Suchi Banerjee und Teresa Murphy. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](role-based-authorization-cs.md)
> [Weiter](assigning-roles-to-users-vb.md)
