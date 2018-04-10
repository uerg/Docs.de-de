---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
title: Erstellen und Verwalten von Rollen (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework untersucht. Danach werden Webseiten zum Erstellen und Löschen Rollen erstellen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/24/2008
ms.topic: article
ms.assetid: 83af9f5f-9a00-4f83-8afc-e98bdd49014e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-vb
msc.type: authoredcontent
ms.openlocfilehash: 75ca9b1c36f9a74d755ef05717f03d139d0b29ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="creating-and-managing-roles-vb"></a>Erstellen und Verwalten von Rollen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.09.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_vb.pdf)

> In diesem Lernprogramm werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework untersucht. Danach werden Webseiten zum Erstellen und Löschen Rollen erstellen.


## <a name="introduction"></a>Einführung

In der <a id="_msoanchor_1"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Lernprogramm wird erläutert, mit der URL-Autorisierung auf um bestimmte Benutzer aus einem Satz von Seiten zu beschränken und deklarative untersucht und programmgesteuerte Verfahren zum Anpassen einer ASP.NET-Seite-Funktionalität, die basierend auf dem Benutzer besuchen. Erteilen der Berechtigung für den Zugriff auf oder Funktionen auf Grundlage von Benutzer, kann jedoch eine Wartung des Verlusts in Szenarien werden, wobei viele Benutzerkonten vorhanden sind oder wenn Benutzer Berechtigungen häufig ändern. Jedes Mal ein Benutzer erhält oder abgibt Autorisierung für eine bestimmte Aufgabe auszuführen, muss der Administrator die entsprechenden URL-Autorisierungsregeln, deklaratives Markup und Code zu aktualisieren.

In der Regel werden können, um Benutzer in Gruppen zu klassifizieren oder *Rollen* und anschließend eine Rolle von Rolle Berechtigungen gelten. Die meisten Webanwendungen haben z. B. einen bestimmten Satz von Seiten oder Aufgaben, die nur für Administratoren reserviert sind. Mit den Verfahren haben gelernt, der *Benutzerbasierte Autorisierung* Lernprogramm fügen die entsprechenden URL-Autorisierungsregeln, deklaratives Markup und Code, um die angegebenen Benutzerkonten zum Ausführen von Verwaltungsaufgaben zu ermöglichen. Aber, wenn ein neuer Administrator hinzugefügt wurde, oder gegebenenfalls vorhandener Administratoren ihre Administratorrechte widerrufen haben, haben wir zurückgeben und aktualisieren Sie die Konfigurationsdateien und die Webseiten. Mit Rollen konnten wir jedoch Erstellen einer Rolle für Administratoren und der "Administratoren" vertrauenswürdige Benutzer zuweisen. Wir würden anschließend die entsprechende URL-Autorisierungsregeln, deklaratives Markup und Code zum Zulassen der "Administratoren" für verschiedene Verwaltungsaufgaben hinzufügen. Mit dieser Infrastruktur ist der Standort neue Administratoren hinzufügen oder entfernen vorhandene so einfach wie ein- oder den Benutzer aus der Administratorrolle entfernen. Es sind keine Steuerelementkonfiguration, deklaratives Markup oder Änderungen am Code erforderlich.

ASP.NET bietet ein Framework Rollen zum Definieren von Rollen und Benutzerkonten zuordnen. Hinzufügen von mit dem Rollen-Framework können wir erstellen und Löschen von Rollen, Benutzern oder Benutzer aus einer Rolle entfernen, ermitteln, welche Benutzer, die zu einer bestimmten Rolle gehören, und feststellen, ob ein Benutzer zu einer bestimmten Rolle gehört. Nach dem Konfigurieren der Rollen-Framework können wir Beschränken des Zugriffs auf Seiten auf Basis der Rolle "Rolle" "durch" über URL-Autorisierungsregeln und ein- und ausblenden, zusätzliche Informationen oder Funktionen auf einer Seite auf der Grundlage des aktuell angemeldeten Benutzers Benutzerrollen.

In diesem Lernprogramm werden die erforderlichen Schritte zum Konfigurieren des Rollen-Framework untersucht. Danach werden Webseiten zum Erstellen und Löschen Rollen erstellen. In der <a id="_msoanchor_2"> </a> [ *Zuweisen von Rollen an Benutzer* ](assigning-roles-to-users-vb.md) Lernprogramm betrachten wir zum Hinzufügen und Entfernen von Benutzern Rollen. Und klicken Sie in der <a id="_msoanchor_3"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Lernprogramm wird gezeigt, wie den Zugriff auf Seiten regelmäßig Rolle nach Rolle sowie zum Anpassen der Seite Funktionen je nach einschränken auf der Website besucht Rolle des Benutzers. Fangen wir an!

## <a name="step-1-adding-new-aspnet-pages"></a>Schritt 1: Hinzufügen von neuen ASP.NET-Seiten

In diesem Lernprogramm und die nächsten beiden werden wir verschiedene Funktionen im Zusammenhang mit Rollen und Funktionen untersuchen. Wir benötigen eine Reihe von ASP.NET-Seiten in den Themen in der gesamten Ausführung dieser Lernprogramme untersucht implementieren. Wir diese Seiten erstellen und Aktualisieren der Siteübersicht.

Starten, indem Sie einen neuen Ordner erstellen, in das Projekt mit dem Namen `Roles`. Als Nächstes fügen Sie vier neue ASP.NET-Seiten, um die `Roles` Ordner, verknüpfen jede Seite mit den `Site.master` Masterseite. Benennen Sie die Seiten:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

An diesem Punkt sollte Projektmappen-Explorer des Projekts im Screenshot, der in Abbildung 1 dargestellt aussehen.


[![Vier neue Seiten wurden Ordners "Roles" hinzugefügt](creating-and-managing-roles-vb/_static/image2.png)](creating-and-managing-roles-vb/_static/image1.png)

**Abbildung 1**: vier neue Seiten wurden hinzugefügt, um die `Roles` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image3.png))


Jede Seite sollte, an diesem Punkt haben, die zwei Inhaltssteuerelemente, jeweils einen für die Gestaltungsvorlage ContentPlaceHolders: `MainContent` und `LoginContent`.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample1.aspx)]

Bedenken Sie, dass die `LoginContent` ContentPlaceHolders Standardmarkup zeigt einen Link zum Anmelden oder Abmelden von der Website, je nachdem, ob der Benutzer authentifiziert wird. Das Vorhandensein der `Content2` Content Steuerelement auf der ASP.NET-Seite überschreibt jedoch Standardmarkup für die Gestaltungsvorlage. Wie in erläutert <a id="_msoanchor_4"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Lernprogramm, überschreiben den Standardcode eignet sich für Seiten, in denen wir nicht anzeigen Anmeldung bezogene möchten, die Optionen in der linken Spalte.

Diese vier Seiten, allerdings möchten wir Standardmarkup für die Gestaltungsvorlage Anzeigen der `LoginContent` ContentPlaceHolder. Daher sollten Sie entfernen das deklarative Markup für die `Content2` Inhaltssteuerelement. Nachdem Sie auf diese Weise sollte jedes der vier Seitenmarkup nur ein Inhaltssteuerelement enthalten.

Schließlich aktualisieren wir die Siteübersicht (`Web.sitemap`) diese neue Webseiten einschließen. Die folgenden XML-Code nach dem Hinzufügen der `<siteMapNode>` für die Lernprogramme Mitgliedschaft wurde hinzugefügt.

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample2.xml)]

Suchen Sie mit der Siteübersicht aktualisiert die Website über einen Browser aus. Wie in Abbildung 2 gezeigt, enthält die Navigation auf der linken Seite Elemente für die Rollen-Lernprogramme.


[![Vier neue Seiten wurden Ordners "Roles" hinzugefügt](creating-and-managing-roles-vb/_static/image5.png)](creating-and-managing-roles-vb/_static/image4.png)

**Abbildung 2**: vier neue Seiten wurden hinzugefügt, um die `Roles` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image6.png))


## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Schritt 2: Angeben und konfigurieren die Rollen-Framework-Datenanbieter

Wie das Framework Mitgliedschaft wird das Rollen-Framework Anbietermodell erstellt. Wie in beschrieben die <a id="_msoanchor_5"> </a> [ *Sicherheitsgrundlagen und die ASP.NET-Unterstützung* ](../introduction/security-basics-and-asp-net-support-vb.md) Lernprogramm .NET Framework enthält drei integrierte Rollen Anbieter: [ `AuthorizationStoreRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx) , [ `WindowsTokenRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx), und [ `SqlRoleProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Diese Reihe von Lernprogrammen konzentriert sich auf die `SqlRoleProvider`, welches das Microsoft SQL Server-Datenbank als rollenspeicher verwendet.

Im Hintergrund der Rollen-Framework und `SqlRoleProvider` wie das Framework Mitgliedschaft Geschäfts- und `SqlMembershipProvider`. .NET Framework enthält eine `Roles` -Klasse, die als die API für Rollen Framework dient. Die `Roles` Klasse verfügt über Methoden wie freigegebene `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`usw. lauten. Wenn eine dieser Methoden aufgerufen wird, die `Roles` Klasse der Aufruf an den konfigurierten Anbieter delegiert. Die `SqlRoleProvider` arbeitet mit den Tabellen rollenspezifische (`aspnet_Roles` und `aspnet_UsersInRoles`) in der Antwort.

Zum Verwenden der `SqlRoleProvider` Anbieter in der vorliegenden Anwendung, um anzugeben, welche Datenbank als Speicher verwenden müssen. Die `SqlRoleProvider` erwartet, dass die angegebene Rolle Store auf bestimmte Tabellen, Sichten und gespeicherte Prozeduren haben. Diese erforderlichen Datenbankobjekte können hinzugefügt werden, mithilfe der [ `aspnet_regsql.exe` Tool](https://msdn.microsoft.com/library/ms229862.aspx). An diesem Punkt haben wir bereits eine Datenbank mit dem Schema für die `SqlRoleProvider`. In der <a id="_msoanchor_6"> </a> [ *erstellen das Schema für die Mitgliedschaft in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) Lernprogramm wir eine Datenbank namens erstellt `SecurityTutorials.mdf` und verwendet `aspnet_regsql.exe` um die Anwendung hinzuzufügen Dienste, die die erforderlichen Datenbankobjekte enthalten die `SqlRoleProvider`. Daher es muss lediglich das Rollen-Framework zur rollenunterstützung zu aktivieren und zu teilen der `SqlRoleProvider` mit der `SecurityTutorials.mdf` Datenbank als der rollenspeicher.

Die Rollen-Framework wird konfiguriert, über die `<roleManager>` Element in der Anwendung `Web.config` Datei. Standardmäßig ist die rollenunterstützung deaktiviert. Um sie zu aktivieren, müssen Sie festlegen der [ `<roleManager>` ](https://msdn.microsoft.com/library/ms164660.aspx) des Elements `enabled` -Attribut `true` wie folgt:

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample3.xml)]

Standardmäßig verfügen alle Webanwendungen über einen Rollenanbieter mit dem Namen `AspNetSqlRoleProvider` vom Typ `SqlRoleProvider`. Diese Standardanbieter ist registriert `machine.config` (am `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample4.xml)]

Des Anbieters `connectionStringName` Attribut gibt an, der rollenspeicher, der verwendet wird. Die `AspNetSqlRoleProvider` Anbieter wird dieses Attribut auf `LocalSqlServer`, ebenfalls definiert `machine.config` und verweist, wird standardmäßig mit einer SQL Server 2005 Express Edition-Datenbank in der `App_Data` Ordner mit dem Namen `aspnet.mdf`.

Daher, wenn wir das Framework Rollen einfach aktivieren, ohne alle Anbieterinformationen in der vorliegenden Anwendungsverzeichnis `Web.config` Datei, die Anwendung verwendet die registrierten Rollen Standardanbieter, `AspNetSqlRoleProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank ist nicht vorhanden, den die ASP.NET-Laufzeit automatisch erstellt, und fügen Sie die Anwendung Dienstschema hinzu. Allerdings möchten wir verwenden die `aspnet.mdf` Datenbank; stattdessen verwendet werden soll die `SecurityTutorials.mdf` Datenbank, die wir bereits erstellt und die Anwendung Dienstschema hinzugefügt haben. Diese Änderung kann auf zwei Arten erfolgen:

- <strong>Geben Sie einen Wert für die</strong><strong>`LocalSqlServer`</strong><strong>Verbindungszeichenfolgenname in</strong><strong>`Web.config`</strong><strong>.</strong> Durch Überschreiben der `LocalSqlServer` Zeichenfolge Verbindungswert in `Web.config`, verwenden wir den Standardanbieter für registrierte Rollen (`AspNetSqlRoleProvider`) und haben sie die korrekte Arbeitsweise mit der `SecurityTutorials.mdf` Datenbank. Weitere Informationen zu dieser Technik finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogbeitrag [Konfigurieren von ASP.NET 2.0-Anwendungsdienste für Verwenden von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Hinzufügen einen neuen registrierten Anbieter vom Typ</strong><strong>`SqlRoleProvider`</strong><strong>und konfigurieren Sie die</strong><strong>`connectionStringName`</strong><strong>Einstellung aus, um auf dieverweisen</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank.</strong> Dies ist der Ansatz ich empfohlen und verwendet die <a id="_msoanchor_7"> </a> [ *erstellen das Schema für die Mitgliedschaft in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) Lernprogramm und ist der Ansatz ich verwende die in diesem Lernprogramm auch.

Das folgende Markup der Rollen-Konfiguration zum Hinzufügen der `Web.config` Datei. Dieses Markup registriert einen neuen Anbieter mit dem Namen `SecurityTutorialsSqlRoleProvider.`

[!code-xml[Main](creating-and-managing-roles-vb/samples/sample5.xml)]

Die oben genannten Markup definiert die `SecurityTutorialsSqlRoleProvider` als Standardanbieter (über die `defaultProvider` Attribut in der `<roleManager>` Element). Außerdem wird die `SecurityTutorialsSqlRoleProvider`des `applicationName` auf `SecurityTutorials`, also die gleiche `applicationName` Einstellung, die von der Mitgliedschaftsanbieter verwendet (`SecurityTutorialsSqlMembershipProvider`). Während nicht hier gezeigt die [ `<add>` Element](https://msdn.microsoft.com/library/ms164662.aspx) für die `SqlRoleProvider` enthält auch eine `commandTimeout` Attribut Dauer für das Datenbank-Timeout in Sekunden an. Der Standardwert ist 30.

Mit dieser Konfiguration Markup erfüllt können wir einzusteigen rollenfunktion innerhalb der Anwendung.

> [!NOTE]
> Die oben genannten Konfiguration Markup veranschaulicht die Verwendung der `<roleManager>` des Elements `enabled` und `defaultProvider` Attribute. Es gibt eine Reihe anderer Attribute, die beeinflussen, wie das Framework Rollen Rolleninformationen auf Basis von Benutzer ordnet. Untersuchen wir diese Einstellungen in der <a id="_msoanchor_8"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Lernprogramm.


## <a name="step-3-examining-the-roles-api"></a>Schritt 3: Überprüfen der Rollen-API.

Die Rollen-Framework-Funktionalität wird bereitgestellt, über die [ `Roles` Klasse](https://msdn.microsoft.com/library/system.web.security.roles.aspx), 13 freigegebene Methoden zum Ausführen von Vorgängen mit der rollenbasierten enthält. Wann wird untersucht, erstellen und Löschen von Rollen in die Schritte4 und 6 werden verwendet, die [ `CreateRole` ](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) und [ `DeleteRole` ](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) -Methoden, die hinzufügen oder entfernen eine Rolle aus dem System.

Um eine Liste aller Rollen im System abzurufen, verwenden die [ `GetAllRoles` Methode](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (siehe Schritt 5). Die [ `RoleExists` Methode](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) gibt einen booleschen Wert, der angibt, ob eine angegebene Rolle vorhanden ist.

In den nächsten Lernprogrammen untersuchen wir zum Zuordnen von Benutzern zu Rollen. Die `Roles` Klasse [ `AddUserToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [ `AddUserToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [ `AddUsersToRole` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx), und [ `AddUsersToRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) Methoden hinzufügen eine oder mehrere Benutzer eine oder mehrere Rollen. Verwenden Sie zum Entfernen von Benutzern aus Rollen der [ `RemoveUserFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [ `RemoveUserFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [ `RemoveUsersFromRole` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx), oder [ `RemoveUsersFromRoles` ](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) Methoden.

In der <a id="_msoanchor_9"> </a> [ *rollenbasierte Autorisierung* ](role-based-authorization-vb.md) Lernprogramm wir Möglichkeiten betrachten, um programmgesteuert ein- oder Ausblenden von Funktionen, die anhand des aktuell angemeldeten Benutzers-Rolle. Um dies zu erreichen, können wir die rollenklasse [ `FindUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [ `GetRolesForUser` ](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [ `GetUsersInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx), oder [ `IsUserInRole` ](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) Methoden.

> [!NOTE]
> Beachten Sie, die jedes Mal eine dieser Methoden aufgerufen wird, die `Roles` Klasse der Aufruf an den konfigurierten Anbieter delegiert. In unserem Fall bedeutet dies, dass der Aufruf an gesendet werden die `SqlRoleProvider`. Die `SqlRoleProvider` führt dann die entsprechende Datenbank-Vorgang auf der Grundlage der aufgerufenen Methode. Z. B. der Code `Roles.CreateRole("Administrators")` führt der `SqlRoleProvider` Ausführen der `aspnet_Roles_CreateRole` gespeicherte Prozedur einen neuen Datensatz in fügt der `aspnet_Roles` Tabelle mit dem Namen Administrators.


Im weiteren Verlauf dieses Lernprogramms verwenden prüft die `Roles` Klasse `CreateRole`, `GetAllRoles`, und `DeleteRole` Methoden, um die Rollen im System zu verwalten.

## <a name="step-4-creating-new-roles"></a>Schritt 4: Erstellen von neuen Rollen

Rollen bieten eine Möglichkeit, nach dem Zufallsprinzip Benutzer der Gruppe, und diese Gruppierung wird am häufigsten für ein Bequemerer Weg zum Anwenden von Autorisierungsregeln verwendet. Aber um Rollen als Autorisierungsmechanismus für die zu verwenden, müssen Sie zuerst definieren, welche Rollen in der Anwendung vorhanden sind. ASP.NET umfasst leider nicht über ein CreateRoleWizard-Steuerelement. Um neue Rollen hinzufügen, müssen wir erstellen eine geeignete Benutzeroberfläche und das Aufrufen der API-Rollen uns. Die gute Nachricht ist, dass dies ganz einfach zu erreichen.

> [!NOTE]
> Es gibt zwar keine CreateRoleWizard-Websteuerelement, besteht die [ASP.NET Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/ms228053.aspx), also in einer lokalen ASP.NET-Anwendung soll als mit der Anzeige und Verwaltung der Konfiguration für Ihre Webanwendung helfen. Ich bin jedoch kein großer Fan von das ASP.NET Websiteverwaltungs-Tool für gibt es zwei Gründe. Zunächst ist es etwas fehlerbehaftete und erforderlichen Erfahrungsgrad des Benutzers bleibt viel wünschenswert sein. Zweitens, das ASP.NET Websiteverwaltungs-Tool dient in nur lokal, dies bedeutet, dass Sie Ihre eigene Rolle Management Webseiten zu erstellen, wenn Sie die Remoteverwaltung von Rollen auf einem live-Standort müssen müssen. Diese zwei Gründen konzentriert in diesem Lernprogramm und die nächste sich auf die Rolle "erforderlich"-Verwaltungstools in eine Webseite erstellen, statt der vertrauenden Seite auf das ASP.NET Websiteverwaltungs-Tool.


Öffnen der `ManageRoles.aspx` auf der Seite der `Roles` Ordner und fügen Sie ein Textfeld und einer Schaltfläche Websteuerelement auf der Seite. Legen Sie das Textfeld-Steuerelement `ID` Eigenschaft `RoleName` und der Schaltfläche `ID` und `Text` Eigenschaften `CreateRoleButton` und Create Role bzw. An diesem Punkt sollte deklarativem Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample6.aspx)]

Doppelklicken Sie anschließend auf die `CreateRoleButton` Schaltfläche Steuerelement im Designer zum Erstellen einer `Click` -Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample7.vb)]

Der obige Code beginnt durch Zuweisen der Rolle "gekürzt" Name im die `RoleName` Textfeld auf die `newRoleName` Variable. Als Nächstes wird der `Roles` Klasse `RoleExists` Methode wird aufgerufen, um festzustellen, wo die Rolle `newRoleName` bereits im System vorhanden ist. Wenn die Rolle nicht vorhanden ist, wird es erstellt über einen Aufruf an die `CreateRole` Methode. Wenn die `CreateRole` -Methode übergeben den Namen einer Rolle, die in das System bereits eine `ProviderException` Ausnahme wird ausgelöst. Daher verändert sich der Code zuerst überprüft, um sicherzustellen, dass die Rolle im System vor dem Aufrufen nicht bereits vorhanden ist `CreateRole`. Die `Click` Ereignishandler endet, wenn gelöscht wird, erscheint die `RoleName` Textfeldss `Text` Eigenschaft.

> [!NOTE]
> Sie vielleicht was passiert, wenn der Benutzer einen beliebigen Wert in eingeben, nicht die `RoleName` Textfeld. Wenn der Wert in der `CreateRole` Methode ist `Nothing` oder eine leere Zeichenfolge ist, eine Ausnahme ausgelöst. Ebenso, wenn der Rollenname ein Komma enthält wird eine Ausnahme ausgelöst. Daher sollte die Seite Validierungssteuerelemente, um sicherzustellen, dass der Benutzer eine Rolle gibt und dass keine Kommas enthalten sind enthalten. Ich lassen als Übung für den Leser.


Erstellen Sie eine Rolle mit dem Namen Administrators. Besuchen Sie die `ManageRoles.aspx` Seite über einen Browser, geben Sie in das Textfeld Administratoren (siehe Abbildung 3), und klicken Sie dann auf die Schaltfläche "Rolle erstellen".


[![Erstellen Sie eine Administratorrolle](creating-and-managing-roles-vb/_static/image8.png)](creating-and-managing-roles-vb/_static/image7.png)

**Abbildung 3**: Erstellen Sie eine Administratorrolle ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image9.png))


Was ist los? Ein Postback erfolgt, aber es gibt keinen visuellen Hinweis, der die Rolle tatsächlich wurde dem System hinzugefügt. Wir aktualisieren diese Seite in Schritt 5 visuelles Feedback einschließen. Jetzt jedoch überprüfen, ob die Rolle erstellt wurde, navigieren Sie zu der `SecurityTutorials.mdf` Datenbank und zum Anzeigen der Daten aus der `aspnet_Roles` Tabelle. Wie in Abbildung 4 gezeigt, die `aspnet_Roles` Tabelle enthält einen Datensatz für den gerade hinzugefügten Administratorrollen.


[![Die Aspnet_Roles-Tabelle enthält eine Zeile für die Administratoren](creating-and-managing-roles-vb/_static/image11.png)](creating-and-managing-roles-vb/_static/image10.png)

**Abbildung 4**: die `aspnet_Roles` Tabelle enthält eine Zeile für die Administratoren ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image12.png))


## <a name="step-5-displaying-the-roles-in-the-system"></a>Schritt 5: Anzeigen der Rollen im System

Wir ergänzen die `ManageRoles.aspx` Seite, um eine Liste der aktuellen Rollen im System enthalten. Um dies zu erreichen, fügen Sie eine GridView-Steuerelements auf der Seite, und legen Sie dessen `ID` Eigenschaft `RoleList`. Als Nächstes fügen Sie eine Methode auf der Seite Code-Behind-Klasse, die mit dem Namen `DisplayRolesInGrid` mit dem folgenden Code:

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample8.vb)]

Die `Roles` Klasse `GetAllRoles` Methodenrückgabe aller Rollen im System als ein Array von Zeichenfolgen. Dieses Array von Zeichenfolgen wird dann an die GridView gebunden. Um die Liste der Rollen an die GridView gebunden, beim ersten Laden der Seite ", müssen wir rufen die `DisplayRolesInGrid` Methode auf der Seite `Page_Load` -Ereignishandler. Der folgende Code ruft diese Methode auf, wenn die Seite zuerst aufgerufen wird, aber nicht bei nachfolgenden Postbacks.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample9.vb)]

Finden Sie mit diesem Code werden auf der Seite über einen Browser. Wie Abbildung 5 zeigt, sollte ein Raster mit einer einzelnen Spalte mit der Bezeichnung Element angezeigt werden. Das Raster enthält eine Zeile für die Rolle "Administratoren", die in Schritt 4 hinzugefügt wurden.


[![Die GridView werden die Rollen in einer einzelnen Spalte angezeigt.](creating-and-managing-roles-vb/_static/image14.png)](creating-and-managing-roles-vb/_static/image13.png)

**Abbildung 5**: GridView werden die Rollen in einer einzelnen Spalte angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image15.png))


Die GridView zeigt eine einzelne Spalte mit dem Element bezeichnet, da der GridView `AutoGenerateColumns` ist auf "true" (Standard), wodurch die GridView zum automatischen Erstellen von einer Spalte für jede Eigenschaft im Eigenschaftensatz seine `DataSource`. Ein Array verfügt über eine einzelne Eigenschaft, die die Elemente im Array, daher die einzelne Spalte in der GridView darstellt.

Zum Anzeigen von Daten mit einer GridView bevorzugen ich explizit Meine Spalten definieren, anstatt sie implizit durch die GridView zu generieren. Wobei die Spalten explizit definiert ist es viel einfacher Formatieren der Daten, die Spalten neu anordnen und andere häufige Aufgaben ausführen. Aus diesem Grund aktualisieren wir die GridView deklarative Markup, damit ihre Spalten explizit definiert sind.

Starten, indem Sie der GridView `AutoGenerateColumns` Eigenschaft auf "false". Als Nächstes fügen Sie ein TemplateField zum Raster, Festlegen seiner `HeaderText` Eigenschaft Rollen, und konfigurieren Sie seine `ItemTemplate` , damit er den Inhalt des Arrays zeigt. Um dies zu erreichen, fügen Sie eine Bezeichnung-Websteuerelement, der mit dem Namen `RoleNameLabel` auf die `ItemTemplate` und binden Sie seine `Text` Eigenschaft `Container.DataItem.`

Diese Eigenschaften und die `ItemTemplate`des Inhalt deklarativ oder über die GridView Felder (Dialogfeld) und Vorlagen bearbeiten Schnittstelle festgelegt werden können. Um die Felder im Dialogfeld erreichen, klicken Sie auf den Link "Spalten bearbeiten" in der GridView-Smarttag. Als Nächstes deaktivieren Sie das Kontrollkästchen, um festgelegt Automatisches Generieren von Feldern der `AutoGenerateColumns` Eigenschaft auf "false", und fügen Sie ein TemplateField an die GridView Festlegen seiner `HeaderText` Eigenschaft Rolle. Zum Definieren der `ItemTemplate`des Inhalts, wählen Sie die Option Vorlagen bearbeiten der GridView Smart Tag. Ziehen Sie eine Bezeichnung Web-Steuerelement auf die `ItemTemplate`legen seine `ID` Eigenschaft, um `RoleNameLabel`, und seine Databinding-Einstellungen konfigurieren, damit seine `Text` Eigenschaft gebunden ist `Container.DataItem`.

Unabhängig davon, welchen Ansatz Sie verwenden, sollte die GridView resultierende deklarative Markup Aussehen ähnlich dem folgenden, wenn Sie fertig sind.

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample10.aspx)]

> [!NOTE]
> Den Inhalt des Arrays werden angezeigt, mit der Syntax Databinding `<%# Container.DataItem %>`. Eine ausführliche Beschreibung der warum diese Syntax verwendet wird, wenn der Inhalt eines Arrays an die GridView gebunden sprengt den Rahmen dieses lehrprogramms sprengen. Weitere Informationen zu diesem Thema finden Sie unter [binden ein Skalar-Array an ein Daten-Websteuerelement](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).


Derzeit ist die `RoleList` zuerst besucht die Seite wird nur GridView zur Liste der Rollen gebunden. Wir müssen das Raster zu aktualisieren, wenn eine neue Rolle hinzugefügt wird. Um dies zu erreichen, aktualisieren Sie die `CreateRoleButton` Schaltfläche `Click` -Ereignishandler aufgerufen die `DisplayRolesInGrid` Methode, wenn eine neue Rolle erstellt wird.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample11.vb)]

Jetzt bei der Benutzer fügt eine neue Rolle die `RoleList` GridView zeigt den gerade hinzugefügten Rolle beim Postback bereitstellen visuelles Feedback, dass die Rolle erfolgreich erstellt wurde. Um dies zu veranschaulichen, finden Sie auf der `ManageRoles.aspx` Seite über einen Browser, und fügen Sie eine Rolle mit dem Namen Abteilungsleiter hinzu. Wenn Sie auf die Schaltfläche "Rolle erstellen", ein Postback wird Stellen Sie sicher, und das Raster wird aktualisiert, sodass diese Administratoren als auch für die neue Rolle, Abteilungsleiter gehören.


[![Die Rolle "Abteilungsleiter" wurde hinzugefügt](creating-and-managing-roles-vb/_static/image17.png)](creating-and-managing-roles-vb/_static/image16.png)

**Abbildung 6**: die Rolle "Abteilungsleiter" wurde hinzugefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image18.png))


## <a name="step-6-deleting-roles"></a>Schritt 6: Löschen von Rollen

An diesem Punkt ein Benutzer eine neue Rolle erstellen und Anzeigen von vorhandenen Rollen aus der `ManageRoles.aspx` Seite. Wir ermöglichen Benutzern Rollen zu löschen. Die `Roles.DeleteRole` Methode verfügt über zwei Überladungen:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) -Löscht die Rolle *RoleName*. Eine Ausnahme wird ausgelöst, wenn die Rolle ein oder mehrere Elemente enthält.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) -Löscht die Rolle *RoleName*. Wenn *ThrowOnPopulateRole* ist `True`, und klicken Sie dann eine Ausnahme ausgelöst wird, wenn die Rolle ein oder mehrere Elemente enthält. Wenn *ThrowOnPopulateRole* ist `False`, und klicken Sie dann die Rolle gelöscht wird, ob es alle Elemente enthält. Intern können die `DeleteRole(roleName)` Methodenaufrufe `DeleteRole(roleName, True)`.

Die `DeleteRole` Methode löst auch eine Ausnahme aus, wenn *RoleName* ist `Nothing` oder eine leere Zeichenfolge oder, wenn *RoleName* ein Komma enthält. Wenn *RoleName* ist nicht im System vorhanden `DeleteRole` fehlschlägt, im Hintergrund, ohne eine Ausnahme auszulösen.

Ergänzen wir die GridView in `ManageRoles.aspx` einschließen eine Delete-Schaltfläche geklickt haben, löscht die ausgewählte Rolle. Starten Sie, indem Sie eine Schaltfläche "löschen" an die GridView hinzufügen, indem Sie die Felder (Dialogfeld) aufrufen und eine Schaltfläche "löschen" unter der Option CommandField hinzufügen. Stellen Sie das Löschen die Spalte ganz linke Schaltfläche, und legen Sie dessen `DeleteText` Eigenschaft, um die Rolle "löschen".


[![Hinzufügen einer Schaltfläche "löschen" an die RoleList GridView](creating-and-managing-roles-vb/_static/image20.png)](creating-and-managing-roles-vb/_static/image19.png)

**Abbildung 7**: Hinzufügen einer Schaltfläche löschen, um die `RoleList` GridView ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-and-managing-roles-vb/_static/image21.png))


Nach dem Hinzufügen der Schaltfläche "löschen", sollte Ihre GridView deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](creating-and-managing-roles-vb/samples/sample12.aspx)]

Als Nächstes erstellen Sie einen Ereignishandler für der GridView `RowDeleting` Ereignis. Dies ist das Ereignis, das beim Postback ausgelöst wird, wenn die Rolle "löschen"-Schaltfläche geklickt wird. Fügen Sie dem Ereignishandler folgenden Code hinzu.

[!code-vb[Main](creating-and-managing-roles-vb/samples/sample13.vb)]

Der Code beginnt durch Programmgesteuertes Verweisen auf die `RoleNameLabel` Webserver-Steuerelement in der Zeile, deren Rolle "löschen" geklickt wurde. Die `Roles.DeleteRole` Methode dann aufgerufen wird, übergibt der `Text` von der `RoleNameLabel` und `False`, wodurch löschen Sie die Rolle, unabhängig davon, ob alle Benutzer der Rolle zugeordnet sind. Schließlich die `RoleList` GridView wird aktualisiert, damit die Rolle "just vorläufig gelöschte" nicht mehr im Raster angezeigt wird.

> [!NOTE]
> Die Schaltfläche "Rolle löschen" ist keine Art von Bestätigung durch den Benutzer vor dem Löschen der Rolle erforderlich. Einer der einfachsten Möglichkeiten, eine Aktion zu bestätigen erfolgt über einen clientseitigen bestätigen (Dialogfeld). Weitere Informationen zu dieser Technik finden Sie unter [Hinzufügen von clientseitigen Bestätigung beim Löschen von](https://asp.net/learn/data-access/tutorial-42-vb.aspx).


## <a name="summary"></a>Zusammenfassung

Viele Webanwendungen verfügen über bestimmte Autorisierungsregeln oder Seitenebene-Funktionen, die nur bestimmte Klassen von Benutzern zur Verfügung steht. Möglicherweise gibt es z. B. eine Reihe von Webseiten, die nur Administratoren zugreifen können. Anstatt definieren diese Autorisierungsregeln regelmäßig durch Benutzer, ist es häufig sinnvoller, die Regeln basierend auf einer Rolle zu definieren. D. h. anstatt explizit ermöglichen Benutzern Scott und Jisun administrative Webseiten zugreifen, ein sind besser verwaltbar Ansatz ist, Mitglieder der Rolle "Administratoren" Zugriff auf diese Seiten zu ermöglichen und dann zur Angabe von Scott und Jisun als Benutzer, die die "Administratoren".

Die Rollen-Framework erleichtert das Erstellen und Verwalten von Rollen. In diesem Lernprogramm zum Konfigurieren der Rollen-Framework verwenden untersucht die `SqlRoleProvider`, welches das Microsoft SQL Server-Datenbank als rollenspeicher verwendet. Es erstellt auch eine Webseite, die Listet die vorhandenen Rollen im System und ermöglicht neue Rollen erstellt werden soll, und vorhandene gelöscht werden soll. In den nachfolgenden Lernprogrammen sehen wir Gewusst wie: Zuweisen von Benutzern zu Rollen und erläutert, wie rollenbasierte Autorisierung angewendet.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Gewusst wie: Verwenden von Rollen-Manager in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Rollenanbieter](https://msdn.microsoft.com/library/aa478950.aspx)
- [Paralleles eigene Websiteverwaltungs-Tool](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Technische Dokumentation für die `<roleManager>` Element](https://msdn.microsoft.com/library/ms164660.aspx)
- [Die Mitgliedschaft und Rollen-Manager-APIs](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm enthalten Alicja Maziarz Suchi Banerjee und Teresa Murphy. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](role-based-authorization-cs.md)
> [Weiter](assigning-roles-to-users-vb.md)
