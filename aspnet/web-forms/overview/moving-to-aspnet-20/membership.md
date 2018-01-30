---
uid: web-forms/overview/moving-to-aspnet-20/membership
title: Mitgliedschaft | Microsoft Docs
author: microsoft
description: "ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung bietet eine einfache Möglichkeit, Incorp..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: f2339485-5d78-4c5e-8c0a-dc9b8a315345
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/membership
msc.type: authoredcontent
ms.openlocfilehash: 1a5a495845b60f9aac51c9776311af67f5dc8767
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="membership"></a>Mitgliedschaft
====================
durch [Microsoft](https://github.com/microsoft)

> ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung bietet eine einfache Möglichkeit, Ihre ASP.NET-Anwendung ein Anmeldeformulars integrieren und Überprüfen von Benutzern für eine Datenbank oder andere Datenspeicher.


ASP.NET-Mitgliedschaft baut auf den Erfolg der Forms-Authentifizierungsmodell von ASP.NET 1.x. ASP.NET-Formularauthentifizierung bietet eine einfache Möglichkeit, Ihre ASP.NET-Anwendung ein Anmeldeformulars integrieren und Überprüfen von Benutzern für eine Datenbank oder andere Datenspeicher. Die Member der Klasse FormsAuthentication ermöglichen es, die Cookies für die Authentifizierung zu behandeln, überprüfen Sie eine gültige Anmeldung, meldet einen Benutzer, usw. Implementieren Formularauthentifizierung in einer ASP.NET-Anwendung für die 1.x kann jedoch viel Code erfordern.

Mitgliedschaft in ASP.NET 2.0 ist eine erhebliche Verbesserung gegenüber Formularauthentifizierung allein. (Ist mindestens die Mitgliedschaft robusteste, wenn bei der Formularauthentifizierung gekoppelt, aber verwenden der Formularauthentifizierung ist nicht erforderlich.) Sehen Sie so schnell, können ASP.NET-Mitgliedschaft und der Anmeldesteuerelemente in ASP.NET 2.0 Sie um eine leistungsfähige Mitgliedschaftssystem zu implementieren, ohne zu viel Code schreiben.

## <a name="implementing-membership-in-aspnet-20"></a>Implementieren von Mitgliedschaft in ASP.NET 2.0

Mitgliedschaft wird implementiert, indem Sie die folgenden vier Schritte. Bedenken Sie, dass viele untergeordnete Schritte, die sowie optionale Konfiguration beteiligt sind, die ebenfalls implementiert werden können. Diese Schritte müssen zum Veranschaulichen der kompletten Überblick über Mitgliedschaft konfigurieren.

1. Erstellen Sie die Mitgliedschaftsdatenbank (wenn es sich um eine SQL Server als Mitgliedschaftsspeicher verwendet wird).
2. Geben Sie die Mitgliedschaftsoptionen in den Konfigurationsdateien für Clientanwendungen. (Mitgliedschaft ist standardmäßig aktiviert.)
3. Bestimmen Sie den Typ des Speichers von Mitgliedschaft, die Sie verwenden möchten. Optionen sind verfügbar: 

    - Microsoft SQL Server (Version 7.0 oder höher)
    - Active Directory Store
    - Benutzerdefinierter Mitgliedschaftsanbieter
4. Konfigurieren Sie die Anwendung für die Formularauthentifizierung mit ASP.NET. Noch einmal: Mitgliedschaft Formularauthentifizierung nutzen ausgelegt ist, aber unter Verwendung der Formularauthentifizierung ist nicht erforderlich.
5. Definieren Sie Benutzerkonten für die Mitgliedschaft und konfigurieren Sie Rollen, falls gewünscht.

## <a name="creating-the-membership-database"></a>Erstellen die Mitgliedschaftsdatenbank

Wenn keine weiteren mithilfe von SQL Server 7.0 oder höher ist als Ihre Mitgliedschaftsspeicher können Sie das Aspnet\_Regsql-Dienstprogramm (am einfachsten über die Visual Studio .NET 2005-Eingabeaufforderung verfügbar) so konfigurieren Sie Ihre Datenbank. Das Aspnet\_Regsql Hilfsprogramm kann verwendet werden, als eine eingabeaufforderungs-Hilfsprogramm oder über eine GUI-Assistenten. Diese Methode ist die einfachste Möglichkeit, Ihre Datenbank konfigurieren. Um den Assistenten zuzugreifen, führen Sie einfach den folgenden Befehl ein:

`aspnet_regsql W`

Nachdem Sie diesen Befehl ausführen, wird die ASP.NET SQL Server-Setup-Assistenten angezeigt werden wie unten dargestellt.


![](membership/_static/image1.jpg)

**Abbildung 1**


Die ASP.NET SQL Server-Setup-Assistent erstellt die Website in der Instanz, die Sie im Assistenten angeben. Allerdings wird ASP.NET die Verbindungszeichenfolge in der Datei "Machine.config" verwenden, für die Verbindung mit Ihrer Datenbank. Standardmäßig zeigen diese Verbindungszeichenfolge auf eine SQL Server 2005-Instanz, wenn Sie eine Instanz von SQL Server 2000 oder SQL Server 7.0 verwenden, Sie die Verbindungszeichenfolge in der Datei "Machine.config" ändern müssen. Diese Verbindungszeichenfolge kann hier befinden:

[!code-xml[Main](membership/samples/sample1.xml)]

Leider, wenn Sie die Verbindungszeichenfolge nicht ändern, wird ASP.NET eine beschreibende Fehlermeldung Ihnen nicht. Es wird weiterhin nur sagen darüber beschweren, dass Sie die Datenbank erstellt haben. Im obigen Fall vorgenommenen ich die Verbindungszeichenfolge auf meinem lokalen SQL Server 2000-Instanz verweisen.

## <a name="specifying-configuration-and-adding-users-and-roles"></a>Konfiguration und Hinzufügen von Benutzern und Rollen angeben

Der nächste Schritt beim Konfigurieren der Mitgliedschaft werden die erforderlichen Informationen in die Datei "Web.config" der Anwendung hinzufügen. In ASP.NET schwierig 1.x, ändern die Datei "Web.config" in einigen Fällen aufgrund der Verwendung von LowerCamelCase und die von Intellisense. 2005 von Visual Studio .NET kann die Aufgabe, die viel einfacher mithilfe von Intellisense für Konfigurationsdateien, aber ASP.NET 2.0 im Laufe der einen Schritt weiter eine Webschnittstelle für das Bearbeiten von Konfigurationsdateien bereitstellt.

Sie können die Weboberfläche starten, indem der ASP.NET-Konfiguration-Schaltfläche auf der Symbolleiste Projektmappen-Explorer, wie unten dargestellt. Sie können auch die Webschnittstelle über Popups starten, die angezeigt werden, wenn Anmeldesteuerelementen eingefügt werden.


![](membership/_static/image2.jpg)

**Abbildung 2**


Dadurch wird das ASP.NET Websiteverwaltungs-Tool unten gezeigten gestartet. Der ASP.NET Web Site-Verwaltung ist ein vier – Registerkarte ""-Schnittstelle, die zum Verwalten von Anwendungseinstellungen erleichtert. Die folgenden Registerkarten sind verfügbar:

- **Startseite**
- **Sicherheit** Benutzer, Rollen und Zugriff zu konfigurieren.
- **Anwendung** Anwendungseinstellungen konfigurieren.
- **Anbieter** konfigurieren und Testen Sie Ihre Anwendungen Mitgliedschaftsanbieter.

Die Websiteverwaltungs-Tool können Sie problemlos neue Benutzer erstellen, neue Rollen erstellen und Verwalten von Benutzern und Rollen. Diese Funktion ist in der Windows-Benutzeroberfläche nicht verfügbar. Die Windows-Benutzeroberfläche können Sie problemlos autorisierungseinstellungen definieren und hinzufügen, löschen und Verwalten der Anbieter, Funktionen, die nicht in die Websiteverwaltungs-Tool enthalten sind.

Um die Windows-Benutzeroberfläche zu starten, öffnen Sie das MMC-Snap-in Internetinformationsdienste (IIS), mit der rechten Maustaste auf die Anwendung, und wählen Sie Eigenschaften. Klicken Sie auf der Registerkarte "ASP.NET", und klicken Sie dann auf die Schaltfläche "-Konfiguration bearbeiten". (Die Anwendung muss unter ASP.NET 2.0 für die Schaltfläche "-Konfiguration bearbeiten" zu aktivierenden ausgeführt werden. Sie können die Version von ASP.NET als auch im Dialogfeld für ASP.NET konfigurieren.) Die Konfigurationseinstellungen für ASP.NET-Dialogfeld wird angezeigt, wie unten dargestellt.


![](membership/_static/image3.jpg)

**Abbildung 3**


Auf der Registerkarte "Allgemein" Verbindungszeichenfolgen und Anwendungseinstellungen aufgeführt. Alle Einstellungen in Kursivschrift werden in einer übergeordneten Konfigurationsdatei (die "Machine.config" oder "Web.config" auf einer höheren Ebene) definiert, und Einstellungen nicht in Kursivschrift sind, aus der Konfigurationsdatei der Anwendung. Wenn eine Einstellung hinzugefügt wird, wird entfernt oder bearbeitet werden, auf der Anwendungsebene ASP.NET hinzufügen, entfernen oder ändern Sie die Einstellung auf die Anwendung Ebenen "Web.config" anstelle von Löschung der Einstellung aus der Konfigurationsdatei, aus der sie geerbt wird.

Nachfolgend finden Sie die Registerkarte "Datenquellenauthentifizierung". Dies ist, in dem Sie Ihre mitgliedschaftseinstellungen konfigurieren. Authentifizierungseinstellungen, Mitgliedschaftsanbieter, Formulare und Rollenanbieter hier konfiguriert werden können.


![](membership/_static/image4.jpg)

**Abbildung 4**


## <a name="implementing-membership-in-your-application"></a>Implementieren die Mitgliedschaft in der Anwendung

Die einfachste Möglichkeit zum Implementieren von ASP.NET 2.0-Mitgliedschaft in der Anwendung ist die Verwendung der angegebenen Anmeldung Steuerelemente. Diese Methode ermöglicht Ihnen die Grundlagen von ASP.NET 2.0-Mitgliedschaft zu implementieren, ohne überhaupt Code schreiben müssen.

Die folgenden Logon-Steuerelemente sind in ASP.NET 2.0 verfügbar:

## <a name="login-control"></a>Steuerelement für die Anmeldung

Das Steuerelement für die Anmeldung stellt eine Schnittstelle für eine Person in Ihrem Mitgliedschaftssystem anmelden. Sie versorgt Sie mit einem Benutzernamen und Kennwort Textboxt und eine Anmeldeschaltfläche. Viele andere allgemeinen Funktionen wie z. B. einen Link zum Registrieren für Personen, die noch nicht durchgeführt haben, damit ein Kontrollkästchen, die dem Benutzer automatisch die Anmeldung bei folgeaufrufen ermöglicht, einen Link für eine kennworterinnerung usw. Alle Funktionen des Steuerelements Anmeldung können über die Eigenschaften des Steuerelements angepasst werden.

In ASP.NET 1.x mussten Entwickler schreiben, viel Code wird eine Suche, wenn die Formularauthentifizierung verwenden. Mit ASP.NET 2.0-Mitgliedschaft können Sie Benutzer überprüfen, ohne überhaupt Code schreiben müssen. ASP.NET wird die Suche des Benutzers automatisch für Sie übernimmt. (Wenn Sie das Steuerelement für die Anmeldung ohne Verwendung von ASP.NET-Mitgliedschaft verwenden, können Sie mithilfe der **OnAuthenticate** Methode, um den Benutzer zu überprüfen.)

## <a name="loginview-control"></a>LoginView-Steuerelement

LoginView-Steuerelement ist ein Steuerelement mit Vorlagen, das standardmäßig zwei Vorlagen enthält. die AnonymousTemplate und LoggedInTemplate. Die Vorlage, die angezeigt wird, richtet sich nach, und zwar unabhängig davon, ob der Benutzer in Ihrem Mitgliedschaftssystem angemeldet ist. Dieses Steuerelement dient in der Regel ein Anmeldungssteuerelement, wenn ein Benutzer noch nicht angemeldet ist und ein Steuerelement LoginStatus und/oder anderen Anmeldesteuerelementen angezeigt wird, wenn der Benutzer angemeldet hat. Bei Verwendung von Rollenverwaltung in einer ASP.NET-Anwendung kann LoginView-Steuerelement auf eine bestimmte Vorlage basierend auf der Rolle Benutzer anzeigen. (Weitere wird auf ASP.NET Rollenverwaltung weiter unten erläutert.)

## <a name="passwordrecovery-control"></a>PasswordRecovery-Steuerelement

Das Steuerelement PasswordRecovery kann Benutzer erhält eine e-Mail mit ihrer aktuellen Kennwörter oder Zurücksetzen ihres Kennworts. Klartext und verschlüsselte Kennwörter können wiederhergestellt werden und per e-Mail an Benutzer gesendet werden. Wenn das Kennwort ein Hashwert erstellt wird, kann nicht wiederhergestellt werden. Stattdessen kann der Benutzer für das Zurücksetzen eines Kennwortes erforderlich sein.

## <a name="loginstatus-control"></a>LoginStatus-Steuerelement

LoginStatus-Steuerelement ist ein Indikator der Anmeldung für Benutzer, die nicht angemeldet sind und einen Indikator für die Abmeldung für Benutzer angezeigt, die derzeit, in angemeldet sind verwendet. Die Request.IsAuthenticated-Eigenschaft wird verwendet, um zu bestimmen, welche Indikator angezeigt. Der Indikator im LoginStatus-Steuerelement angezeigt wird kann es sich um Text (implementiert, über die **LoginText** und **LogoutText** Eigenschaften) oder Bilder (implementiert, über die **LoginImageUrl**und **LogoutImageUrl** Eigenschaften.)

Wenn ein Benutzer über das Steuerelement LoginStatus abmeldet, er wird mit der vom angegebenen URL umgeleitet der **LogoutPageUrl** Eigenschaft. Wenn diese Eigenschaft nicht festgelegt ist, wird die aktuelle Seite aktualisiert. Da der Standort wahrscheinlich durch Formularauthentifizierung geschützt ist, wird die Aktualisierung der aktuellen Seite durch Umleiten des Benutzers zur Anmeldeseite für den Standort.

## <a name="loginname-control"></a>LoginName-Steuerelement

Die LoginName-Steuerelement zeigt den Benutzernamen des Benutzers, der die Website derzeit angemeldet sind.

## <a name="createuserwizard-control"></a>CreateUserWizard Control

Das Steuerelement CreateUserWizard bietet Benutzern eine einfache Möglichkeit, die für Ihr Mitgliedschaftssystem zu registrieren. Sie können über die Schnittstelle, die nachfolgend aufgeführten Schritte (implementiert als eine Auflistung von WizardSteps) hinzufügen.


![](membership/_static/image5.jpg)

**Abbildung 5**


Die CreateUserWizard ist ein Steuerelement mit Vorlagen, das die Assistenten-Klasse abgeleitet und stellt die folgenden Vorlagen bereit:

- **HeaderTemplate** dieser Vorlage steuert die Darstellung des Headers des Assistenten.
- **SidebarTemplate** dieser Vorlage steuert die Darstellung der Randleiste des Assistenten.
- **StartNavigationTemplate** diese Vorlage steuert die Darstellung der im Navigationsbereich des Assistenten für den Schritt an Start sind.
- **StepNavigationTemplate** dieser Vorlage steuert die Darstellung des Navigationsbereichs nicht in der Schritts "Start" oder "Fertig stellen.
- **FinishNavigationTemplate** dieser Vorlage steuert die Darstellung des Navigationsbereichs auf den Schritt "Fertig stellen".

Darüber hinaus wird die ASP.NET für die einzelnen Schritte, mit denen Sie den Assistenten hinzugefügt, eine benutzerdefinierte Vorlage erstellen, die eine ContentTemplate und eine CustomNavigationTemplate für diesen Schritt enthält. Ausführliche Informationen zum Anpassen der CreateUserWizard finden Sie in der VS-2005-Dokumentation:

## <a name="changepassword-control"></a>ChangePassword Control

Das Steuerelement ChangePassword kann Benutzer sein Kennwort ändern. Wenn die DisplayUserName-Eigenschaft ist true (es ist standardmäßig "false"), kann der Benutzer sein eigenes Kennwort ändern, wenn sie nicht angemeldet sind. Wenn der Benutzer *ist* bereits angemeldet und die DisplayUserName-Eigenschaft ist "true", der Benutzer wird in der Lage, das Kennwort eines anderen Benutzers zu ändern, die nicht protokolliert werden, sie wissen, dass die Benutzer-ID des Benutzers bereitzustellen.

Bedenken Sie, wenn Benutzer in der Lage, Kennwörter zu ändern, melden Sie sich ohne sein sollen, müssen Sie sicherstellen, dass die Seite, auf der das ChangePassword-Steuerelement angezeigt wird, anonymen Zugriff zulässt. Natürlich müssen Benutzer ihr alte Kennwort angeben, um ihr Kennwort zu ändern.

## <a name="role-management"></a>Rollenverwaltung

Rollenverwaltung können Sie zum Zuweisen von Benutzern zu einer bestimmten Rolle, und klicken Sie dann Beschränken des Zugriffs auf bestimmte Dateien oder Ordnern, die anhand dieser Rolle. Rollenverwaltung bietet darüber hinaus eine API, sodass Sie programmgesteuert Someones Rolle bestimmen oder alle Benutzer in einer bestimmten Rolle ermitteln und entsprechend reagieren.

Rollenverwaltung ist keine Voraussetzung im ASP.NET-Mitgliedschaft noch ist die Mitgliedschaft zwingend um Rollenverwaltung zu verwenden. Allerdings die beiden miteinander ordentlich ergänzen und ist es wahrscheinlicher, dass Entwickler diese zusammen mit anderen verwenden.

Um die Rollenverwaltung in der Anwendung zu ermöglichen, stellen Sie die folgende Änderung in der Datei "Web.config":

[!code-xml[Main](membership/samples/sample2.xml)]

Wenn die **zurückgibt** -Attributsatz zur "true" ASP.NET die Rollenmitgliedschaft eines Benutzers in einem Cookie auf dem Client zwischengespeichert. Dadurch wird die Rolle Suchvorgänge ohne Aufrufe an die RoleProvider auftreten. Entwicklern wird empfohlen, sicherzustellen, dass, wenn Sie dieses Attribut verwenden, die **CookieProtection** -Attribut für alle festgelegt ist. (Dies ist die Standardeinstellung.) Dadurch wird sichergestellt, dass die Cookiedaten verschlüsselt werden und werden sichergestellt, dass der Inhalt des Cookies nicht verändert wurden. Rollen können mit dem Website-Verwaltungstool hinzugefügt werden. Sie können Sie problemlos Rollen definieren, konfigurieren den Zugriff auf Teile der Website basierend auf diesen Rollen und Zuweisen von Benutzern zu Rollen.


![](membership/_static/image6.jpg)

**Abbildung 6**


Wie oben gezeigt, können neue Rollen hinzugefügt werden, indem Sie einfach den Namen der Rolle eingeben und dann auf Rolle hinzufügen. Vorhandene Rollen verwaltet, oder indem Sie auf den entsprechenden Link in der Liste der vorhandenen Rollen gelöscht werden können.

Wenn Sie eine Rolle verwalten, können Sie hinzufügen oder Entfernen Benutzer aus, wie unten dargestellt.


![](membership/_static/image7.jpg)

**Abbildung 7**


Überprüfen Sie das Kontrollkästchen für die Benutzerrolle, können Sie problemlos einen Benutzer zu einer bestimmten Rolle hinzufügen. ASP.NET wird die Mitgliedschaftsdatenbank mit den entsprechenden Einträgen automatisch aktualisiert werden. Sie sollten auch so konfigurieren Sie Zugriffsregeln für Ihre Anwendung. 1.x ASP.NET-Entwickler sind dadurch über Kenntnisse der &lt;Autorisierung&gt; Element in der Datei "Web.config", und diese Option ist auch weiterhin in ASP.NET 2.0. Allerdings Regeln, die einfacher zu verwalten des Zugriffs verwenden die Websiteverwaltungs-Tool wie unten.


![](membership/_static/image8.jpg)

**Abbildung 8**


In diesem Fall wird der Ordner Verwaltung hervorgehoben (dessen schwierig, die angezeigt werden, da das Tool hellgrau markiert) und "Administratoren" Zugriff erteilt wurde. Alle anderen Benutzer werden abgelehnt. Sie können auf das Symbol "Head", wählen Sie eine Regel, und klicken Sie dann die Schaltflächen nach oben und nach unten verwenden, um die Regeln zu ordnen klicken. Wie bei ASP.NET &lt;Autorisierung&gt; Element Regeln werden verarbeitet, in der Reihenfolge, in der sie angezeigt werden. In anderen Worten, wenn die Reihenfolge der Regeln in der obigen Screenshot rückgängig gemacht wurden, würde niemand in den Ordner Verwaltung zugreifen, da die erste Regel, die ASP.NET auftreten würde die Regel wäre, die "Jeder" zu dem Ordner verweigert.

ASP.NET 2.0 fügt eine Datei "Web.config" zu dem Ordner, für den Sie eine Regel angeben. Zugriffsregeln können über die Konfigurationsdatei oder über die Websiteverwaltungs-Tool bearbeitet werden. Die Websiteverwaltungs-Tool ist also einfach eine Schnittstelle, die über die Konfigurationsdatei in einer benutzerfreundlichen Umgebung bearbeitet werden kann.

## <a name="using-roles-in-code"></a>Verwenden von Rollen in Code

Die API für die Verwaltung der Rolle "" wurde nicht geändert, seit Version 1.x. Die **IsInRole** Methode wird verwendet, um zu bestimmen, ob ein Benutzer in einer bestimmten Rolle ist.

[!code-csharp[Main](membership/samples/sample3.cs)]

ASP.NET erstellt auch eine RolePrincipal-Instanz als Member des aktuellen Kontexts. Die RolePrincipal-Objekt kann verwendet werden, um aller Rollen abzurufen, der die Benutzer wie folgt angehört:

[!code-csharp[Main](membership/samples/sample4.cs)]

## <a name="using-rolegroups-with-the-loginview-control"></a>Verwenden RoleGroups mit dem LoginView-Steuerelement

Nun, dass Sie einen Überblick über die Mitgliedschaft und Rollenverwaltung haben, können kurz erläutert, wie das LoginView-Steuerelement diese Funktionalität in ASP.NET 2.0 nutzt. Wie zuvor erwähnt ist das LoginView-Steuerelement ein Steuerelement mit Vorlagen, das standardmäßig zwei Vorlagen enthält. die AnonymousTemplate und LoggedInTemplate. Innerhalb der LoginView Aufgaben Dialogfeld einen Link (siehe unten) ist, die Sie RoleGroups bearbeiten können.


![](membership/_static/image9.jpg)

**Abbildung 9**


Jedes Objekt RoleGroup enthält ein Array von Zeichenfolgen, welche Rollen definiert, für die RoleGroup gilt. Um eine neue RoleGroup LoginView-Steuerelement hinzuzufügen, klicken Sie auf den Link RoleGroups bearbeiten. In der obigen Abbildung sehen Sie sich, dass ich eine neue RoleGroup für Administratoren hinzugefügt haben. Durch Auswahl dieser RoleGroup (RoleGroup[0]) aus der Dropdownliste Ansichten, ich können konfigurieren, eine Vorlage, die nur für Mitglieder der Rolle "Administratoren" angezeigt wird. In der folgenden Abbildung wurden ich eine neue RoleGroup hinzugefügt, die für Mitglieder der Rolle "Vertrieb" und "Verteilungspunkt" gilt. Dadurch wird eine zweite RoleGroup in der Dropdownliste Sichten im Dialogfeld "LoginView-Aufgaben", und nichts in der Vorlage hinzugefügt werden von einem Benutzer in der Sales oder der Verteilung angezeigt Rolle.


![](membership/_static/image10.jpg)

**Abbildung 10**


## <a name="overriding-the-existing-membership-provider"></a>Überschreiben des vorhandenen Mitgliedschaftsanbieters

Es gibt mehrere Möglichkeiten, die Funktionalität von ASP.NET-Mitgliedschaft zu erweitern. Erstens können Sie die vorhandene Funktionalität der Klasse SqlMembershipProvider offensichtlich ändern, indem von ihm erben, und ihre Methoden überschreiben. Wenn Sie möchten Ihre eigenen Funktionalität zu implementieren, wenn Benutzer erstellt werden, können Sie z. B. eine eigene Klasse erstellen, die wie folgt von SqlMembershipProvider erbt:

[!code-csharp[Main](membership/samples/sample5.cs)]

Wenn andererseits, Ihren eigenen Anbieter (zum Speichern von Informationen zu Ihrer Mitgliedschaft in einer Access-Datenbank, z. B.) erstellen möchten, können Sie einen eigenen Anbieter erstellen.

## <a name="creating-your-own-membership-provider"></a>Erstellen Ihre eigenen Mitgliedschaftsanbieter

Um Ihren eigenen Mitgliedschaftsanbieter erstellen zu können, müssen Sie zuerst eine Klasse erstellen, die die MembershipProvider-Klasse erbt. Bei Verwendung von VB.NET wird Visual Studio 2005 die Stubs für alle Methoden hinzufügen, die Sie überschreiben müssen. Wenn Sie C#-, die bis zu der Sie die Stubs hinzufügen verwenden.

Sie müssen die folgenden außer Kraft setzen:

- Parameter "ApplicationName"-Eigenschaft
- ChangePassword-Funktion
- ChangePasswordQuestionAndAnswer function
- CreateUser-Funktion
- "DeleteUser"-Funktion
- EnablePasswordReset-Eigenschaft
- EnablePasswordRetrieval-Eigenschaft
- FindUsersByEmail-Funktion
- FindUsersByName-Funktion
- GetAllUsers-Funktion
- GetNumberOfUsersOnline function
- GetPassword-Funktion
- GetUser-Funktion
- GetUserNameByEmail function
- MaxInvalidPasswordAttempts-Eigenschaft
- MinRequiredNonAlphanumericCharacters-Eigenschaft
- MinRequiredPasswordLength-Eigenschaft
- PasswordAttemptWindow-Eigenschaft
- PasswordFormat-Eigenschaft
- PasswordStrengthRegularExpression-Eigenschaft
- RequiresQuestionAndAnswer property
- RequiresUniqueEmail-Eigenschaft
- Accounts-Funktion
- Unlock Benutzerfunktion
- UpdateUser-Funktion
- ValidateUser-Funktion

Thats eine ziemlich Liste als C#-Entwickler zu implementieren. Sie finden es vielleicht einfacher, erstellen Sie die Klasse in VB.NET ohne jede Implementierung und verwenden Sie .NET Reflector oder ein ähnliches Tool, um den Code in c# konvertiert.

Auf ihre Standardwerte in die Initialize-Methode sollte die Verbindungszeichenfolge und andere Eigenschaften festgelegt werden. (Die Initialize-Methode wird ausgelöst, wenn der Anbieter zur Laufzeit geladen wird.) Der zweite Parameter für die Initialize-Methode ist vom Typ System.Collections.Specialized.NameValueCollection und ist ein Verweis auf die &lt;hinzufügen&gt; Element, das einen benutzerdefinierten Anbieter in der Datei "Web.config" zugeordnet ist. Dieser Eintrag sieht wie folgt aus:

[!code-xml[Main](membership/samples/sample6.xml)]

Hier ist ein Beispiel für die Initialize-Methode.

[!code-csharp[Main](membership/samples/sample7.cs)]

Um den Benutzer zu überprüfen, wenn Benutzer Ihre Anmeldeformular übermitteln, müssen Sie die ValidateUser-Methode verwenden. Diese Methode wird ausgelöst, wenn der Benutzer die Anmeldung klickt, die in das Steuerelement für die Anmeldung. Setzen Sie den Code, der die Nachschlagevorgangs durch den Benutzer in dieser Methode ist.

Wie Sie sehen können, Schreiben Ihren eigenen Mitgliedschaftsanbieter nicht und können Sie dieses leistungsstarke Funktionen von ASP.NET 2.0 zu erweitern.
