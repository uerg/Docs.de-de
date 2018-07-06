---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Mitgliedschaft | Microsoft-Dokumentation
author: microsoft
description: ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung stellt eine bequeme Möglichkeit, Incorp...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: ebba0c25fd3c7d6de7182c14559add7902cafe83
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388395"
---
<a name="membership"></a>Primäre BAM-Importdatenbank
====================
durch [Microsoft](https://github.com/microsoft)

> ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung bietet eine bequeme Möglichkeit, Ihre ASP.NET-Anwendung ein Anmeldeformular integrieren, und Überprüfen von Benutzern für eine Datenbank oder einem anderen Datenspeicher.


ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung bietet eine bequeme Möglichkeit, Ihre ASP.NET-Anwendung ein Anmeldeformular integrieren, und Überprüfen von Benutzern für eine Datenbank oder einem anderen Datenspeicher. Der Member der Klasse FormsAuthentication erleichtern die Cookies für die Authentifizierung herzustellen, überprüfen Sie für eine gültige Anmeldung, melden Sie sich einen Benutzer abzumelden usw. können. Allerdings ist das Implementieren der Formularauthentifizierung in ASP.NET 1.x-Anwendung kann eine beträchtliche Menge Code erforderlich.

Mitgliedschaft in ASP.NET 2.0 ist eine erhebliche Verbesserung gegenüber der Verwendung der Formularauthentifizierung allein. (Ist die Mitgliedschaft in Kombination mit der Formularauthentifizierung stabilste, aber unter Verwendung der Formularauthentifizierung ist nicht erforderlich.) Wie Sie bald sehen werden, können ASP.NET-Mitgliedschaft und die Anmeldesteuerelemente in ASP.NET 2.0 Sie eine leistungsstarke Mitgliedschaftssystem zu implementieren, ohne viel Code zu schreiben.

## <a name="implementing-membership-in-aspnet-20"></a>Implementieren die Mitgliedschaft in ASP.NET 2.0

Mitgliedschaft wird implementiert, indem Sie die folgenden vier Schritte erforderlich. Bedenken Sie, dass viele untergeordnete Schritte, die als auch optionale Konfiguration beteiligt sind, die ebenfalls implementiert werden können. Diese Schritte sollen einen Überblick über das Konfigurieren der Mitgliedschaft zu veranschaulichen.

1. Erstellen Sie Ihre in Mitgliedschaftsdatenbank (wenn es sich um eine SQL Server als die Mitgliedschaftsspeicher verwendet wird).
2. Geben Sie die Mitgliedschaftsoptionen in Ihren Anwendungen Konfigurationsdateien. (Mitgliedschaft ist standardmäßig aktiviert.)
3. Bestimmen Sie den Typ des Speichers für Mitgliedschaft, die Sie verwenden möchten. Optionen sind: 

    - Microsoft SQL Server (Version 7.0 oder höher)
    - Active Directory-Store
    - Benutzerdefinierter Mitgliedschaftsanbieter
4. Konfigurieren Sie die Anwendung für die Formularauthentifizierung mit ASP.NET. Auch hier Mitgliedschaft Formular-Authentifizierung nutzen soll, aber unter Verwendung der Formularauthentifizierung ist nicht erforderlich.
5. Definieren von Benutzerkonten für die Mitgliedschaft und Rollen zu konfigurieren, falls gewünscht.

## <a name="creating-the-membership-database"></a>Erstellen der Mitgliedschaftsdatenbank

Wenn Sie verwenden SQL Server 7.0 oder höher ist als Ihre Mitgliedschaftsspeicher, können Sie das Aspnet\_Regsql-Dienstprogramm (am einfachsten aus der Visual Studio .NET 2005-Eingabeaufforderung verfügbar) so konfigurieren Sie Ihre Datenbank. Das Aspnet\_Regsql Hilfsprogramm kann verwendet werden, als eine eingabeaufforderungs-Hilfsprogramm oder über einen Assistenten mit grafischer Benutzeroberfläche. Diese Methode ist die einfachste Möglichkeit zum Konfigurieren der Datenbank. Um den Assistenten zuzugreifen, müssen Sie einfach führen Sie den folgenden Befehl aus:

`aspnet_regsql W`

Nachdem Sie diesen Befehl ausführen, wird die ASP.NET SQL Server-Setup-Assistenten angezeigt werden wie unten dargestellt.


![](membership/_static/image1.jpg)

**Abbildung 1**


Die ASP.NET SQL Server-Setup-Assistenten erstellt die Website in der Instanz, die Sie im Assistenten angeben. ASP.NET wird allerdings die Verbindungszeichenfolge in der Datei machine.config verwenden, für die Verbindung mit Ihrer Datenbank. Standardmäßig verweist diese Verbindungszeichenfolge in eine SQL Server 2005-Instanz, sodass, wenn Sie eine SQL Server 2000 oder SQL Server 7.0-Instanz verwenden, Sie die Verbindungszeichenfolge in der Datei "Machine.config" ändern müssen. Diese Verbindungszeichenfolge kann hier befinden:

[!code-xml[Main](membership/samples/sample1.xml)]

Wenn Sie die Verbindungszeichenfolge nicht ändern, ASP.NET erhalten Sie keine beschreibende Fehlermeldung. Diese Funktion bleibt weiterhin nur Spruch beschweren sich, dass Sie die Datenbank erstellt haben. Im obigen Fall vorgenommenen ich die Verbindungszeichenfolge, um auf meinem lokalen SQL Server 2000-Instanz zu verweisen.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Angeben von Konfiguration und Hinzufügen von Benutzern und Rollen

Der nächste Schritt im Konfigurieren der Mitgliedschaft werden die erforderlichen Informationen in die Datei "Web.config" der Anwendung hinzufügen. In ASP.NET war 1.x, ändern die Datei "Web.config" manchmal aufgrund der Verwendung von LowerCamelCase und das Fehlen von Intellisense schwierig. Visual Studio .NET 2005 vereinfacht die Aufgabe mit Intellisense für Konfigurationsdateien, aber ASP.NET 2.0 geht einen Schritt weiter durch die Bereitstellung einer Web-Schnittstelle für die Bearbeitung von Konfigurationsdateien.

Sie können die Weboberfläche starten, mit der ASP.NET-Konfiguration-Schaltfläche auf der Symbolleiste Projektmappen-Explorer, wie unten dargestellt. Sie können auch die Web-Schnittstelle über Popups starten, die angezeigt werden, wenn Steuerelemente für die Anmeldung eingefügt werden.


![](membership/_static/image2.jpg)

**Abbildung 2**


Dadurch wird die ASP.NET Websiteverwaltungs-Tool unten angezeigt. Die ASP.NET Web Site Administration ist eine vier-Registerkarte-Schnittstelle, die zum Verwalten von Anwendungseinstellungen vereinfacht. Die folgenden Registerkarten sind verfügbar:

- **Home**
- **Sicherheit** Benutzer, Rollen und den Zugriff zu konfigurieren.
- **Anwendung** konfigurieren Sie die Anwendungseinstellungen.
- **Anbieter** konfigurieren und Testen Sie Ihre Anwendungen Membership-Provider.

Die Websiteverwaltungs-Tool können Sie problemlos neue Benutzer erstellen, neue Rollen erstellen und Verwalten von Benutzern und Rollen. Diese Funktion ist in der Windows-Schnittstelle nicht verfügbar. Die Windows-Schnittstelle können Sie problemlos autorisierungseinstellungen definieren und hinzufügen, löschen und Verwalten von Anbietern, Funktionen, die nicht in der Web Site Administration Tool enthalten sind.

Um die Windows-Benutzeroberfläche zu starten, öffnen Sie das MMC-Snap-in Internet Information Services, mit der rechten Maustaste auf Ihre Anwendung aus, und wählen Sie Eigenschaften. Klicken Sie auf der Registerkarte "ASP.NET", und klicken Sie dann auf die Schaltfläche "-Konfiguration bearbeiten". (Die Anwendung muss unter ASP.NET 2.0 für die Konfiguration bearbeiten-Schaltfläche aktiviert wird, werden ausgeführt werden. Sie können die ASP.NET-Version auch im Dialogfeld für ASP.NET konfigurieren.) Das Dialogfeld "ASP.NET-Einstellungen für die Konfiguration" wird angezeigt, wie unten dargestellt.


![](membership/_static/image3.jpg)

**Abbildung 3**


Klicken Sie auf der Registerkarte "Allgemein" werden die Verbindungszeichenfolgen und Anwendungseinstellungen aufgeführt. Alle Einstellungen in Kursivschrift werden in einer übergeordneten Konfigurationsdatei (die "Machine.config" oder eine Web.config-Datei auf einer höheren Ebene) definiert, und Einstellungen nicht in Kursivschrift werden aus der Konfigurationsdatei für die Anwendungen. Wenn eine Einstellung hinzugefügt wird, wird entfernt oder bearbeitet werden, auf der Anwendungsebene ASP.NET hinzufügen, entfernen oder ändern Sie die Einstellung auf die Anwendung Ebenen Datei "Web.config" anstelle von entfernen die Einstellung aus der Konfigurationsdatei, die von der es geerbt ist.

Die Registerkarte "Datenquellenauthentifizierung" ist unten dargestellt. Dies ist, in dem Sie Ihre mitgliedschaftseinstellungen konfigurieren. Authentifizierungseinstellungen Mitgliedschaftsanbieter, bildet und Rollenanbieter hier konfiguriert werden können.


![](membership/_static/image4.jpg)

**Abbildung 4**


## <a name="implementing-membership-in-your-application"></a>Implementieren die Mitgliedschaft in der Anwendung

Die einfachste Möglichkeit zum Implementieren von ASP.NET 2.0-Mitgliedschaft in der Anwendung ist die Verwendung der angegebenen Anmeldung Steuerelemente. Diese Methode können Sie die Grundlagen von ASP.NET 2.0-Mitgliedschaft zu implementieren, ohne überhaupt Code schreiben.

Die folgenden Steuerelemente für die Anmeldung sind in ASP.NET 2.0 verfügbar:

## <a name="login-control"></a>Login-Steuerelement

Die Login-Steuerelement stellt eine Schnittstelle für eine Person im Mitgliedschaftssystem anmelden bereit. Es bietet Ihnen eine Textboxt Benutzernamen und das Kennwort und eine Schaltfläche "Anmelden". Viele andere allgemeinen Funktionen wie z. B. einen Link für die Benutzer zu registrieren, die noch nicht abgeschlossen haben, damit ein Kontrollkästchen, die den Benutzer die automatisch die Anmeldung bei nachfolgenden Besuchen können, einen Link für eine kennworterinnerung usw. Alle Funktionen der Login-Steuerelement sind anpassbare über die Eigenschaften des Steuerelements.

In ASP.NET 1.x mussten Entwickler viel Code für eine Suche ausführen, wenn es sich bei Verwendung der Formularauthentifizierung zu schreiben. Mit ASP.NET 2.0-Mitgliedschaft können Sie Benutzer überprüfen, ohne überhaupt Code schreiben. ASP.NET wird die Suche des Benutzers automatisch für Sie übernimmt. (Wenn Sie das Steuerelement für die Anmeldung ohne Verwendung von ASP.NET-Mitgliedschaft verwenden, können Sie mithilfe der **OnAuthenticate** Methode, um den Benutzer zu überprüfen.)

## <a name="loginview-control"></a>LoginView-Steuerelement

Das LoginView-Steuerelement ist ein Steuerelement mit Vorlagen, das zwei Vorlagen standardmäßig bereitstellt. die AnonymousTemplate und LoggedInTemplate. Die Vorlage, die angezeigt wird, wird durch, und zwar unabhängig davon, ob der Benutzer, im Mitgliedschaftssystem angemeldet ist bestimmt. Dieses Steuerelement wird in der Regel verwendet, ein Login-Steuerelement, wenn ein Benutzer noch nicht angemeldet ist und ein LoginStatus-Steuerelement und/oder andere Anmeldesteuerelemente angezeigt, wenn der Benutzer angemeldet hat. Wenn Sie Rollenverwaltung in Ihrer ASP.NET-Anwendung verwenden, kann das LoginView-Steuerelement eine bestimmte Vorlage auf Grundlage der Benutzerrolle anzeigen. (Mehr wird auf ASP.NET Rollenverwaltung weiter unten erläutert.)

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das Steuerelement PasswordRecovery kann Benutzer erhalten eine e-Mail mit dem aktuellen Kennwort oder das Kennwort zurückzusetzen. Klartext und verschlüsselte Kennwörter können wiederhergestellt werden und per e-Mail an Benutzer gesendet werden. Wenn das Kennwort ein Hashwert erstellt wird, kann nicht wiederhergestellt werden. Stattdessen wird der Benutzer für das Zurücksetzen eines Kennworts erforderlich sein.

## <a name="loginstatus-control"></a>LoginStatus-Steuerelement

Das LoginStatus-Steuerelement wird verwendet, einen Indikator Anmeldung für Benutzer nicht angemeldet sind und einen Indikator für die Abmeldung für Benutzer anzeigen, die derzeit angemeldet sind. Die Request.IsAuthenticated-Eigenschaft wird verwendet, um zu bestimmen, welche Indikator angezeigt. Der Indikator, der durch das LoginStatus-Steuerelement angezeigten kann es sich um Text (über implementiert die **LoginText** und **LogoutText** Eigenschaften) oder Bilder (über implementiert die **LoginImageUrl**und **LogoutImageUrl** Eigenschaften.)

Wenn ein Benutzer über das LoginStatus-Steuerelement meldet, er umgeleitet an die URL, die gemäß der **LogoutPageUrl** Eigenschaft. Wenn diese Eigenschaft nicht festgelegt ist, wird die aktuelle Seite aktualisiert. Da der Standort wahrscheinlich von der Formularauthentifizierung geschützt ist, wird die Aktualisierung der aktuellen Seite den Benutzer zur Anmeldeseite für den Standort umleiten.

## <a name="loginname-control"></a>LoginName-Steuerelement

Das LoginName-Steuerelement zeigt den Benutzernamen des auf der Website angemeldeten Benutzers.

## <a name="createuserwizard-control"></a>Steuerelement CreateUserWizard

Das Steuerelement CreateUserWizard bietet Benutzern eine bequeme Möglichkeit, für das Mitgliedschaftssystem zu registrieren. Sie können über die Schnittstelle, die nachfolgend aufgeführten Schritte, die (implementiert als eine Auflistung von WizardSteps) hinzufügen.


![](membership/_static/image5.jpg)

**Abbildung 5**


Die CreateUserWizard ist ein Steuerelement mit Vorlagen, das die Assistenten-Klasse abgeleitet und enthält die folgenden Vorlagen:

- **HeaderTemplate** mit dieser Vorlage steuert die Darstellung des Headers des Assistenten.
- **SidebarTemplate** mit dieser Vorlage steuert die Darstellung der Randleiste des Assistenten.
- **StartNavigationTemplate** dieser Vorlage steuert, die Darstellung der im Navigationsbereich des Assistenten auf der Startphase.
- **StepNavigationTemplate** mit dieser Vorlage steuert die Darstellung des Navigationsbereichs nicht in den Anfang oder Ende Schritt.
- **FinishNavigationTemplate** mit dieser Vorlage steuert die Darstellung des Navigationsbereichs auf den Schritt "Fertig stellen".

Darüber hinaus wird die ASP.NET für die einzelnen Schritte, die Sie dem Assistenten hinzufügen, eine benutzerdefinierte Vorlage erstellt, die eine ContentTemplate und eine CustomNavigationTemplate für diesen Schritt enthält. Ausführliche Informationen zum Anpassen der CreateUserWizard finden Sie in der VS 2005-Dokumentation:

## <a name="changepassword-control"></a>ChangePassword-Steuerelement

ChangePassword-Steuerelement kann Benutzer ihr Kennwort zu ändern. Wenn die DisplayUserName-Eigenschaft ist true (es ist standardmäßig "false"), kann der Benutzer sein Kennwort ändern, wenn sie nicht angemeldet sind. Wenn der Benutzer *ist* bereits angemeldet und die DisplayUserName-Eigenschaft ist "true", der Benutzer wird in der Lage, das Kennwort eines anderen Benutzers zu ändern, die bei der Bereitstellung, sie wissen, dass die Benutzer-ID des Benutzers nicht angemeldet ist.

Bedenken Sie, dass wenn Sie die Benutzer, um Kennwörter zu ändern, ohne sich anmelden zu können möchten, müssen Sie sicherstellen, dass die Seite, auf der das ChangePassword-Steuerelement angezeigt wird, anonymen Zugriff zulässt. Natürlich müssen Benutzer ihr alte Kennwort angeben, um ihr Kennwort zu ändern.

## <a name="role-management"></a>Rollenverwaltung

Rollenverwaltung können Sie zum Zuweisen von Benutzern zu einer bestimmten Rolle, und klicken Sie dann Beschränken des Zugriffs auf bestimmte Dateien oder Ordnern, die basierend auf dieser Rolle. Rollenverwaltung bietet auch eine API, damit Sie programmgesteuert einfacher zu Rolle bestimmen oder alle Benutzer in einer bestimmten Rolle ermitteln und entsprechend reagieren können.

Rollenverwaltung ist nicht erforderlich in ASP.NET-Mitgliedschaft, noch ist die Mitgliedschaft einer Anforderung um Rollenverwaltung zu verwenden. Allerdings die beiden ergänzen einander, gut und ist es wahrscheinlich, dass Entwickler sie in Verbindung mit anderen verwenden.

Stellen Sie zum Aktivieren der Rollenverwaltung in Ihrer Anwendung die folgende Änderung in der Datei "Web.config" ein:

[!code-xml[Main](membership/samples/sample2.xml)]

Wenn die **zurückgibt** -Attributsatz auf "true", ASP.NET die Rollenmitgliedschaft eines Benutzers in einem Cookie auf dem Client zwischenspeichert. Dadurch wird die Rolle Suchvorgänge ohne Aufrufe in die RoleProvider auftreten. Entwicklern wird empfohlen, sicherzustellen, dass, wenn Sie dieses Attribut verwenden, die **CookieProtection** -Attribut für alle festgelegt ist. (Dies ist die Standardeinstellung.) Dadurch wird sichergestellt, dass die Cookiedaten verschlüsselt werden und werden sichergestellt, dass der Inhalt des Cookies nicht verändert wurden. Rollen können mithilfe der Web Site Administration Tool hinzugefügt werden. Sie können problemlos Rollen definieren, konfigurieren den Zugriff auf Teile der Site, die basierend auf diesen Rollen und Zuweisen von Benutzern zu Rollen.


![](membership/_static/image6.jpg)

**Abbildung 6:**


Wie oben gezeigt, können neue Rollen hinzugefügt werden, indem einfach den Namen der Rolle eingeben und dann auf Rolle hinzufügen. Vorhandene Rollen verwaltet werden, oder indem Sie auf den entsprechenden Link in der Liste der vorhandenen Rollen gelöscht werden können.

Wenn Sie eine Rolle verwalten, können Sie hinzufügen oder Entfernen Benutzer aus, wie unten dargestellt.


![](membership/_static/image7.jpg)

**Abbildung 7**


Überprüfen Sie das Kontrollkästchen für die Benutzerrolle, können Sie problemlos einen Benutzer zu einer bestimmten Rolle hinzufügen. ASP.NET aktualisiert automatisch die Mitgliedschaftsdatenbank mit den entsprechenden Einträgen. Sie sollten auch die Regeln für den Zugriff für Ihre Anwendung zu konfigurieren. ASP.NET 1.x-Entwickler kennen dies über die &lt;Autorisierung&gt; Element in der Datei "Web.config", und diese Option ist in ASP.NET 2.0 noch verfügbar sind. Allerdings Regel einfacher zum Verwalten des Zugriffs mithilfe der Web Site Administration Tool wie unten.


![](membership/_static/image8.jpg)

**Abbildung 8**


In diesem Fall wird der Ordner "Verwaltung" hervorgehoben (ist immer schwierig, angezeigt werden, da das Tool hellgrau markiert) und die Rolle "Administratoren" Zugriff gewährt wurde. Alle anderen Benutzer werden abgelehnt. Klicken Sie auf das Symbol "Head", wählen Sie eine Regel, und klicken Sie dann mithilfe der Schaltflächen nach oben und nach unten anordnen von Regeln. Wie bei ASP.NET &lt;Autorisierung&gt; Element Regeln werden verarbeitet, in der Reihenfolge, in der sie angezeigt werden. Das heißt, wenn die Reihenfolge der Regeln in der obigen Screenshot rückgängig gemacht wurden, würde niemand auf den administrationsordner zugreifen, da die erste Regel, die ASP.NET auftreten würde die Regel wäre, die für den Ordner für jeden verweigert.

ASP.NET 2.0 fügt eine web.config-Datei in den Ordner für den Sie eine Zugriffsregel angeben. Regeln für den Zugriff können über die Konfigurationsdatei oder über das Websiteverwaltungs-Tool bearbeitet werden. Die Websiteverwaltungs-Tool ist also einfach eine Schnittstelle, die über die Konfigurationsdatei in einer benutzerfreundlichen Umgebung bearbeitet werden kann.

## <a name="using-roles-in-code"></a>Verwenden von Rollen in Code

Die API für die Rollenverwaltung wurde nicht geändert, seit der Version 1.x. Die **"IsInRole"** Methode wird verwendet, um zu bestimmen, ob ein Benutzer in einer bestimmten Rolle ist.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET erstellt auch eine RolePrincipal-Instanz als Member des aktuellen Kontexts. Die RolePrincipal-Objekt kann verwendet werden, um alle Rollen zu erhalten, die der Benutzer wie folgt angehört:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Verwenden RoleGroups mit dem LoginView-Steuerelement

Nun, da Sie einen Überblick über die Rollenverwaltung und Mitgliedschaft verfügen, können Erörtern Sie kurz, wie das LoginView-Steuerelement diese Funktion in ASP.NET 2.0 nutzt. Wie bereits erwähnt ist das LoginView-Steuerelement ein Steuerelement mit Vorlagen, die standardmäßig zwei Vorlagen enthält. die AnonymousTemplate und LoggedInTemplate. In der LoginView Aufgaben, die Dialog eine Verknüpfung (siehe unten) ist, die Sie RoleGroups bearbeiten können.


![](membership/_static/image9.jpg)

**Abbildung 9**


Jedes Objekt RoleGroup enthält ein Array von Zeichenfolgen, welche Rollen definiert, für die RoleGroup gilt. Um eine neue RoleGroup dem LoginView-Steuerelement hinzuzufügen, klicken Sie auf den Link RoleGroups bearbeiten. In der obigen Abbildung sehen Sie sich, dass ich eine neue RoleGroup für Administratoren hinzugefügt haben. Durch Auswahl dieser RoleGroup (RoleGroup[0]) aus der Dropdownliste "Ansichten", ich kann konfigurieren eine Vorlage, die nur für Mitglieder der Rolle der Administratoren angezeigt werden. In der folgenden Abbildung haben ich eine neue RoleGroup hinzugefügt, die für Mitglieder der Rolle "Sales" und "Verteilungspunkt" gilt. Dadurch wird eine zweite RoleGroup der Ansichten-Dropdownliste im Dialogfeld "LoginView-Aufgaben" hinzugefügt, und nichts hinzugefügt, wird die Vorlage werden von einem Benutzer in der Umsätze oder die Verteilung Rolle.


![](membership/_static/image10.jpg)

**Abbildung 10**


## <a name="overriding-the-existing-membership-provider"></a>Überschreiben des vorhandenen Mitgliedschaftsanbieters

Es gibt mehrere Möglichkeiten, dass Sie die Funktionalität von ASP.NET-Mitgliedschaft erweitern können. Zunächst einmal können Sie die bestehende Funktionalität des SqlMembershipProvider Klasse erben, und ihre Methoden überschreiben offensichtlich ändern. Wenn Sie eigene Funktionen implementieren, wenn Benutzer erstellt werden sollen, können Sie z. B. eine eigene Klasse erstellen, die von SqlMembershipProvider erbt wie folgt:

[!code-csharp[Main](membership/samples/sample5.cs)]

Wenn Sie auf der anderen Seite Sie Ihren eigenen Anbieter (zum Speichern von Informationen zu Ihrer Mitgliedschaft in einer Access-Datenbank, z. B.) erstellen möchten, können Sie Ihren eigenen Anbieter erstellen.

## <a name="creating-your-own-membership-provider"></a>Erstellen einen eigenen Mitgliedschaftsanbieter

Um einen eigenen Mitgliedschaftsanbieter erstellen zu können, müssen Sie zuerst eine Klasse erstellen, die die MembershipProvider-Klasse erbt. Wenn Sie VB.NET verwenden, wird Visual Studio 2005 die Stubs für alle Methoden hinzufügen, die Sie überschreiben müssen. Wenn Sie c#, nun ist es an die Stubs hinzufügen verwenden.

Sie müssen die folgenden überschreiben:

- ApplicationName-Eigenschaft
- ChangePassword-Funktion
- ChangePasswordQuestionAndAnswer-Funktion
- CreateUser-Funktion
- "DeleteUser"-Funktion
- EnablePasswordReset-Eigenschaft
- EnablePasswordRetrieval-Eigenschaft
- FindUsersByEmail-Funktion
- FindUsersByName-Funktion
- GetAllUsers-Funktion
- GetNumberOfUsersOnline-Funktion
- GetPassword-Funktion
- GetUser-Funktion
- GetUserNameByEmail-Funktion
- MaxInvalidPasswordAttempts-Eigenschaft
- MinRequiredNonAlphanumericCharacters-Eigenschaft
- MinRequiredPasswordLength-Eigenschaft
- PasswordAttemptWindow-Eigenschaft
- PasswordFormat-Eigenschaft
- PasswordStrengthRegularExpression-Eigenschaft
- Eigenschaft RequiresQuestionAndAnswer
- RequiresUniqueEmail-Eigenschaft
- Accounts-Funktion
- Entsperren Sie die Funktion "User"
- UpdateUser-Funktion
- ValidateUser-Funktion

Thats eine Liste aus, um als c#-Entwickler zu implementieren. Möglicherweise ist es einfacher erstellen Sie die Klasse in VB.NET ohne jegliche Implementierung, und klicken Sie dann mithilfe von .NET Reflector oder ein ähnliches Tool, um den Code in c# zu konvertieren.

Die Verbindungszeichenfolge und andere Eigenschaften müssen die Standardwerte in der Initialize-Methode festgelegt werden. (Die Initialize-Methode wird ausgelöst, wenn der Anbieter zur Laufzeit geladen wird.) Der zweite Parameter für die Initialize-Methode ist vom Typ System.Collections.Specialized.NameValueCollection und ist ein Verweis auf die &lt;hinzufügen&gt; -Element, das den benutzerdefinierten Anbieter in der Datei "Web.config" zugeordnet ist. Dieser Eintrag sieht folgendermaßen aus:

[!code-xml[Main](membership/samples/sample6.xml)]

Hier ist ein Beispiel für die Initialize-Methode.

[!code-csharp[Main](membership/samples/sample7.cs)]

Um den Benutzer zu überprüfen, wenn sie Ihr Anmeldeformular übermitteln, müssen Sie die ValidateUser-Methode verwenden. Diese Methode wird ausgelöst, wenn der Benutzer die Schaltfläche "Anmelden", in dem Login-Steuerelement klickt. Setzen Sie den Code, der die Nachschlagevorgangs durch den Benutzer in dieser Methode ist.

Wie Sie sehen können, schreiben einen eigenen Mitgliedschaftsanbieter ist nicht schwierig und können Sie diese leistungsstarken Funktionen von ASP.NET 2.0 zu erweitern.
