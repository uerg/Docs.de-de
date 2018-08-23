---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
title: Entsperren und Genehmigen von Benutzerkonten (c#) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird gezeigt, wie eine Webseite für Administratoren zur Verwaltung der Benutzer gesperrt, und Status genehmigt. Wir sehen auch o für neue Benutzer genehmigen...
ms.author: riande
ms.date: 04/01/2008
ms.assetid: 5346aab1-9974-489f-a065-ae3883b8a350
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 1a8373f62833c3a76d2e7f96193e5ecbe2d9c593
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826588"
---
<a name="unlocking-and-approving-user-accounts-c"></a>Entsperren und Genehmigen von Benutzerkonten (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.14.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_cs.pdf)

> In diesem Tutorial wird gezeigt, wie eine Webseite für Administratoren zur Verwaltung der Benutzer gesperrt, und Status genehmigt. Wir sehen auch die neue Benutzer genehmigen wird nur verwendet werden, nachdem sie ihre e-Mail-Adresse überprüft haben.


## <a name="introduction"></a>Einführung

Zusammen mit einem Benutzernamen, Kennwort und e-Mail-Adresse, enthält jedes Benutzerkonto zwei Statusfelder, die darüber entscheiden, ob der Benutzer an der Website anmelden kann: gesperrt und genehmigt. Ein Benutzer wird automatisch gesperrt, wenn sie ungültige Anmeldeinformationen für eine angegebene Anzahl von Malen innerhalb einer angegebenen Anzahl von Minuten bereitstellen (die Standardeinstellungen ein Benutzers Sperre nach 5 ungültigen Anmeldeversuchen innerhalb von 10 Minuten). Der Status "genehmigt" ist nützlich in Szenarien, in dem eine Aktion anzeigen, bevor ein neuer Benutzer der Website anmelden kann. Beispielsweise kann ein Benutzer müssen zuerst überprüfen ihre e-Mail-Adresse oder von einem Administrator genehmigt werden, bevor Sie anmelden können.

Da ein gesperrter oder nicht genehmigte Benutzer nicht anmelden, ist es nur natürlich, Fragen Sie sich, wie diesen Status zurückgesetzt werden können. ASP.NET umfasst keine keine integrierte Funktionalität oder Web-Steuerelemente für die Verwaltung der Benutzer gesperrt, und Status, teilweise genehmigt, da diese Entscheidungen auf Grundlage von Standorten behandelt werden müssen. Einige Websites möglicherweise automatisch genehmigen Sie alle neuen Benutzerkonten (das Standardverhalten). Andere müssen ein Administrator genehmigen Sie neue Konten oder Benutzer nicht genehmigt, bis sie finden Sie auf einen Link, um die e-Mail-Adresse bereitgestellt, wenn sie sich registriert gesendet. Ebenso einige Websites können Benutzer sperren, bis ein Administrator ihren Status zurückgesetzt, während andere Standorte eine e-Mail an die Sperre auch Benutzer mit einer URL senden sie zum Entsperren ihres Kontos besuchen können.

In diesem Tutorial wird gezeigt, wie eine Webseite für Administratoren zur Verwaltung der Benutzer gesperrt, und Status genehmigt. Wir sehen auch die neue Benutzer genehmigen wird nur verwendet werden, nachdem sie ihre e-Mail-Adresse überprüft haben.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Schritt 1: Verwalten von Benutzern gesperrt, und Status genehmigt

In der <a id="Tutorial12"> </a> [ *erstellen eine Schnittstelle zum Auswählen eines Benutzerkontos aus vielen* ](building-an-interface-to-select-one-user-account-from-many-cs.md) GridView gefiltert, Tutorial, die wir erstellt eine Seite, die jedem Benutzerkonto in einem ausgelagerten, aufgeführt. Im Raster sind die Namen des Benutzers und e-Mail-Adresse, deren Status genehmigt und gesperrt, gibt an, ob sie derzeit online sind und alle Kommentare über den Benutzer aufgeführt. Zum Verwalten von Benutzern genehmigt und Status werden gesperrt, wir konnten dieses Raster bearbeitet werden. Um den Status "genehmigt" (eines Benutzers zu ändern, würde der Administrator zuerst suchen Sie das Benutzerkonto und bearbeiten Sie dann die entsprechende GridView-Zeile aus, aktivieren oder deaktivieren das Kontrollkästchen genehmigte. Alternativ können wir der genehmigten und gesperrt Status über eine separate ASP.NET-Seite verwalten.

In diesem Tutorial verwenden wir zwei ASP.NET-Seiten: `ManageUsers.aspx` und `UserInformation.aspx`. Die Idee dabei ist, die `ManageUsers.aspx` Listet die Benutzerkonten im System, während `UserInformation.aspx` ermöglicht dem Administrator der genehmigten und gesperrt Status für einen bestimmten Benutzer zu verwalten. Unsere erste Tagesordnung ist zum Verbessern der GridView in `ManageUsers.aspx` sollen eine HyperLinkField, das als eine Spalte von Links gerendert wird. Sollten auf jeden Link, um zu zeigen `UserInformation.aspx?user=UserName`, wobei *Benutzername* ist der Name des Benutzers zu bearbeiten.

> [!NOTE]
> Wenn Sie den Code für heruntergeladen haben die <a id="Tutorial13"> </a> [ *wiederherstellen und Ändern von Kennwörtern* ](recovering-and-changing-passwords-cs.md) Tutorial haben Sie vielleicht bemerkt, die die `ManageUsers.aspx` Seite enthält bereits einen Satz von " Verwalten von"Links und die `UserInformation.aspx` Seite stellt eine Schnittstelle bereit, für das Kennwort des ausgewählten Benutzers ändern. Ich entschied mich nicht um diese Funktionalität in den Code in diesem Tutorial verknüpft ist, da es hat funktioniert durch umgehen der Membership-API und den Betrieb von direkt mit der SQL Server-Datenbank so ändern Sie das Kennwort eines Benutzers zu replizieren. Dieses Tutorial beginnt von Grund auf neu, mit der `UserInformation.aspx` Seite.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Hinzufügen von Links zu "verwalten" die`UserAccounts`GridView

Öffnen der `ManageUsers.aspx` Seite, und fügen Sie eine HyperLinkField auf die `UserAccounts` GridView. Legen Sie die HyperLinkField des `Text` Eigenschaft auf "Verwalten" und die zugehörige `DataNavigateUrlFields` und `DataNavigateUrlFormatString` Eigenschaften `UserName` und "UserInformation.aspx?user={0}" bzw. Diese Einstellungen die HyperLinkField so konfigurieren, dass alle der Links anzuzeigen, dass des Texts "Verwalten", aber jeder Link in den entsprechenden übergibt *Benutzername* Wert in der Abfragezeichenfolge.

Nehmen Sie nach dem Hinzufügen der HyperLinkField an die GridView, einen Moment Zeit, an die `ManageUsers.aspx` Seite über einen Browser. Wie in Abbildung 1 gezeigt, enthält jede GridView-Zeile jetzt einen Link "Verwalten" aus. Der Link "Verwalten" für Bruce verweist auf `UserInformation.aspx?user=Bruce`, während auf der Link "Verwalten" Dave zeigt `UserInformation.aspx?user=Dave`.


[![Fügt der HyperLinkField ein](unlocking-and-approving-user-accounts-cs/_static/image2.png)](unlocking-and-approving-user-accounts-cs/_static/image1.png)

**Abbildung 1**: die HyperLinkField Fügt einen Link "Verwalten" für jedes Benutzerkonto ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image3.png))


Erstellen Sie die Benutzeroberfläche und code für die `UserInformation.aspx` auf der Seite einen Moment, aber zunächst sehen wir reden über programmgesteuert einen Benutzer ändern gesperrt, und Status genehmigt. Die [ `MembershipUser` Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) hat [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) und [ `IsApproved` Eigenschaften](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Die `IsLockedOut` Eigenschaft ist schreibgeschützt. Es gibt keinen Mechanismus, um programmgesteuert zu sperren, bis ein Benutzer ein. Verwenden Sie zum Entsperren eines Benutzers die `MembershipUser` Klasse [ `UnlockUser` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). Die `IsApproved` -Eigenschaft ist lesbar und schreibbar. Um Änderungen an dieser Eigenschaft zu speichern, müssen wir rufen die `Membership` Klasse [ `UpdateUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), und übergeben Sie das geänderte `MembershipUser` Objekt.

Da die `IsApproved` Eigenschaft lesbar und schreibbar, ein CheckBox-Steuerelement wahrscheinlich beste Element der Benutzeroberfläche zum Konfigurieren dieser Eigenschaft. Allerdings ein Kontrollkästchen funktionieren nicht für die `IsLockedOut` Eigenschaft, da ein Benutzer ein Administrator Sperren nicht möglich, kann sie nur einen Benutzer freigeben. Eine geeignete Benutzeroberfläche für die `IsLockedOut` -Eigenschaft ist eine Schaltfläche, entsperrt Sie beim Klicken auf das Benutzerkonto an. Diese Schaltfläche, sollte nur aktiviert werden, wenn der Benutzer gesperrt ist.

### <a name="creating-theuserinformationaspxpage"></a>Erstellen der`UserInformation.aspx`Seite

Wir können nun zum Implementieren der Benutzeroberfläche in `UserInformation.aspx`. Öffnen Sie diese Seite, und fügen Sie die folgenden Web-Steuerelemente hinzu:

- Ein Link zu steuern, die, wenn angeklickt, gibt den Administrator die `ManageUsers.aspx` Seite.
- Ein Label-Websteuerelement für die Anzeige des ausgewählten Benutzers ein. Legen Sie diese Bezeichnung `ID` zu `UserNameLabel` und lösche die `Text` Eigenschaft.
- Ein Kontrollkästchen-Steuerelement, das mit dem Namen `IsApproved`. Legen Sie dessen `AutoPostBack` Eigenschaft `true`.
- Ein Bezeichnungssteuerelement zum Anzeigen des Benutzers die zuletzt gesperrt, bis Datum. Benennen Sie diese Bezeichnung `LastLockedOutDateLabel` und lösche die `Text` Eigenschaft.
- Eine Schaltfläche für das Entsperren des Benutzers. Benennen Sie diese Schaltfläche `UnlockUserButton` und legen Sie dessen `Text` Eigenschaft "Entsperren von Benutzer".
- Ein Bezeichnungssteuerelement zum Anzeigen von statusmeldungen, z. B. "Status" genehmigt "(des Benutzers aktualisiert wurde." Nennen Sie dieses Steuerelement `StatusMessage`, deaktivieren Sie die `Text` -Eigenschaft, und legen dessen `CssClass` Eigenschaft `Important`. (Die `Important` CSS-Klasse wird definiert, der `Styles.css` Stylesheet-Datei den entsprechenden Text in einer großen, Rote Schrift angezeigt.)

Nach dem Hinzufügen dieser Steuerelemente, sollte die Entwurfsansicht in Visual Studio ähnlich wie im Screenshot in Abbildung 2 aussehen.


[![Erstellen der Benutzeroberfläche für UserInformation.aspx](unlocking-and-approving-user-accounts-cs/_static/image5.png)](unlocking-and-approving-user-accounts-cs/_static/image4.png)

**Abbildung 2**: Erstellen der Benutzeroberfläche für `UserInformation.aspx` ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image6.png))


Mit der Benutzeroberfläche abgeschlossen ist, ist unser Nächstes Festlegen der `IsApproved` Kontrollkästchen und andere Steuerelemente basierend auf Informationen des ausgewählten Benutzers. Erstellen Sie einen Ereignishandler für die Seite `Load` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample1.cs)]

Der obige Code zunächst sicherstellen, dass diese ersten Besuch der Seite und nicht auf ein nachfolgendes Postback handelt. Es liest dann den Benutzernamen durch Übergeben der `user` Querystring-Feld und ruft Informationen über dieses Benutzerkonto über die `Membership.GetUser(username)` Methode. Wenn kein Benutzername wurde über die Abfragezeichenfolge bereitgestellt werden soll, oder wenn der angegebene Benutzer nicht gefunden werden konnte, erhält des Administrators an der `ManageUsers.aspx` Seite.

Die `MembershipUser` des Objekts `UserName` Wert wird dann angezeigt, der `UserNameLabel` und `IsApproved` aktiviert ist basierend auf den `IsApproved` -Eigenschaftswert.

Die `MembershipUser` des Objekts [ `LastLockoutDate` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) gibt eine `DateTime` Wert, der angibt, wenn der Benutzer entsprechend seiner letzten gesperrt. Wenn der Benutzer nicht gesperrt wurde, hängt der zurückgegebene Wert den Mitgliedschaftsanbieter ab. Wenn ein neues Konto erstellt wird, die `SqlMembershipProvider` legt die `aspnet_Membership` tabellenspezifischen `LastLockoutDate` Feld `1754-01-01 12:00:00 AM`. Der obige Code zeigt eine leere Zeichenfolge in die `LastLockoutDateLabel` Wenn die `LastLockoutDate` wird vor dem Jahr 2000 ist, andernfalls die Date-Teil der `LastLockoutDate` Eigenschaft wird in der Bezeichnung angezeigt. Die `UnlockUserButton'` s `Enabled` -Eigenschaftensatz auf des Benutzers gesperrt, Status, was bedeutet, dass diese Schaltfläche wird nur aktiviert werden, wenn der Benutzer gesperrt ist.

Testen in Ruhe die `UserInformation.aspx` Seite über einen Browser. Natürlich, dass gestartet `ManageUsers.aspx` , und wählen Sie ein Benutzerkonto verwalten. Beim Eintreffen `UserInformation.aspx`, beachten Sie, dass die `IsApproved` Kontrollkästchen ist nur aktiviert, wenn der Benutzer genehmigt wird. Wenn der Benutzer immer gesperrt wurde, wird ihrem letzten gesperrt, Datum angezeigt. Die Schaltfläche "Benutzer entsperren" nur aktiviert, wenn der Benutzer zurzeit gesperrt ist. Aktivieren oder Deaktivieren der `IsApproved` Kontrollkästchen oder indem Sie auf die Schaltfläche "Benutzer entsperren" bewirkt, dass einen Postback, aber keine Änderungen an das Benutzerkonto, das vorgenommen werden, da wir noch um haben Ereignishandler für diese Ereignisse erstellen.

Zurück zu Visual Studio, und Erstellen von Ereignishandlern für die `IsApproved` Kontrollkästchens `CheckedChanged` Ereignis und die `UnlockUser` Schaltfläche `Click` Ereignis. In der `CheckedChanged` Ereignishandler, Festlegen des Benutzers `IsApproved` Eigenschaft, um die `Checked` -Eigenschaft des Kontrollkästchens, und klicken Sie dann speichern Sie die Änderungen über einen Aufruf an `Membership.UpdateUser`. In der `Click` -Ereignishandler rufen einfach die `MembershipUser` des Objekts `UnlockUser` Methode. In beiden Ereignishandler, zeigen Sie eine entsprechende Meldung in die `StatusMessage` Bezeichnung.

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample2.cs)]

### <a name="testing-theuserinformationaspxpage"></a>Testen der`UserInformation.aspx`Seite

Bei diesen Ereignishandlern vorhanden, noch einmal auf die Seite und nicht genehmigte Benutzer. Wie in Abbildung 3 gezeigt wird, sollte eine kurze, die auf der Seite zeigt an, dass des Benutzers die Meldung `IsApproved` Eigenschaft wurde erfolgreich geändert.


[![Chris hat nicht genehmigt wurde.](unlocking-and-approving-user-accounts-cs/_static/image8.png)](unlocking-and-approving-user-accounts-cs/_static/image7.png)

**Abbildung 3**: Chris wurde nicht genehmigt ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image9.png))


Abmelden und versuchen Sie es, als der Benutzer, dessen Konto anmelden, wurde als Nächstes einfach nicht genehmigt. Da der Benutzer nicht genehmigt wird, kann nicht bei der Anmeldung. Standardmäßig zeigt das Login-Steuerelement dieselbe Nachricht, wenn der Benutzer, unabhängig von der Ursache für die Anmeldung kann nicht. Aber in der <a id="Tutorial6"> </a> [ *Überprüfen von Benutzer-Anmeldeinformationen für die Mitgliedschaft Benutzer Store* ](../membership/validating-user-credentials-against-the-membership-user-store-cs.md) Tutorial erläutert, auf die Verbesserung der Login-Steuerelement, um einen besser geeigneten Nachricht anzuzeigen. Wie in Abbildung 4 gezeigt, wird Chris eine Meldung darüber informiert, dass er anmelden kann nicht, da es sich bei seinem Konto noch nicht genehmigt wird angezeigt.


[![Chris kann nicht da His Anmeldekonto ist nicht genehmigt](unlocking-and-approving-user-accounts-cs/_static/image11.png)](unlocking-and-approving-user-accounts-cs/_static/image10.png)

**Abbildung 4**: Chris kann nicht da His Anmeldekonto ist nicht genehmigt ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image12.png))


Versuch der um die Sperre auch Funktionalität zu testen, melden Sie sich als eines genehmigten Benutzers, aber verwenden ein falsches Kennwort. Wiederholen Sie diesen Prozess die erforderlichen Anzahl von Wiederholungen, bis das Konto des Benutzers gesperrt wurde. Die Login-Steuerelement wurde ebenfalls aktualisiert, zum Anzeigen einer benutzerdefiniertes Meldung, wenn versucht wird, über ein gesperrtes Konto anmelden. Sie wissen, dass ein Konto gesperrt wurde, nachdem Sie gestartet haben die folgende Meldung auf der Anmeldeseite wird angezeigt: "Ihr Konto wurde gesperrt, da zu viele ungültige Anmeldeversuche. Wenden Sie sich an den Administrator, um Ihr Konto entsperrt hat."

Wechseln Sie zurück zur der `ManageUsers.aspx` Seite, und klicken Sie auf den Link "verwalten" für den Benutzer gesperrt. Wie in Abbildung 5 gezeigt, sehen Sie einen Wert in der `LastLockedOutDateLabel` die Schaltfläche "Benutzer entsperren" aktiviert werden soll. Klicken Sie auf die Schaltfläche "Benutzer entsperren", um das Benutzerkonto zu entsperren. Nachdem Sie die Benutzer nicht entsperrt haben, werden sie sich erneut anmelden.


[![Dave wurde aus dem System gebunden.](unlocking-and-approving-user-accounts-cs/_static/image14.png)](unlocking-and-approving-user-accounts-cs/_static/image13.png)

**Abbildung 5**: Dave hat wurde gesperrt, des Systems ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Schritt 2: Angeben von neuen Benutzer Status genehmigt

Der Status "genehmigt" ist nützlich in Szenarien, in dem eine Aktion ausgeführt werden, bevor ein neuer Benutzer kann die Anmeldung und die benutzerdefinierte Funktionen der Website zugreifen sollen. Beispielsweise können Sie eine private Website ausgeführt werden, in dem alle Seiten, mit Ausnahme der Seiten Anmeldung und Registrierung nur für authentifizierte Benutzer zugegriffen werden kann. Aber was geschieht, wenn eine unbekannte Ihre Website erreicht wird von der Seite für die Registrierung und erstellt ein Konto? Um dies zu verhindern, die Sie auf der Seite für die Registrierung verschieben konnte ein `Administration` Ordner erforderlich, dass ein Administrator manuell jedes Konto erstellen. Alternativ könnten Sie für die Registrierung zulassen, während verbieten Zugriff auf die Website aus, bis der Administrator das Benutzerkonto bestätigt.

In der Standardeinstellung genehmigt Steuerelement CreateUserWizard neue Konten. Sie können konfigurieren, dass dieses Verhalten des Steuerelements mit [ `DisableCreatedUser` Eigenschaft](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Legen Sie diese Eigenschaft auf `true` So genehmigen Sie neue Benutzerkonten nicht.

> [!NOTE]
> Standardmäßig protokolliert das Steuerelement CreateUserWizard automatisch auf das neue Benutzerkonto. Dieses Verhalten hängt von des Steuerelements [ `LoginCreatedUser` Eigenschaft](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Da nicht genehmigte Benutzer der Website anmelden darf nicht bei `DisableCreatedUser` ist `true` das neue Benutzerkonto wird nicht protokolliert, auf der Website, unabhängig vom Wert von der `LoginCreatedUser` Eigenschaft.


Wenn Sie programmgesteuert über neue Benutzerkonten erstellen die `Membership.CreateUser` Methode zum Erstellen eines Benutzerkontos nicht genehmigte verwenden eine der Überladungen, die des neuen Benutzers akzeptieren `IsApproved` Eigenschaftswert als Eingabeparameter.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Schritt 3: Benutzer genehmigen, indem Sie ihre e-Mail-Adresse überprüfen

Viele Websites, die die Benutzerkonten zu unterstützen sind neue Benutzer nicht genehmigt, bis sie die e-Mail-Adresse überprüft werden, die sie bereitgestellt werden, bei der Registrierung. Diese Überprüfung wird häufig verwendet, um Bots, Spammern und andere Ne'er-do-wells entgegenzuwirken, muss eine eindeutige, überprüften e-Mail-Adresse und einen zusätzlichen Schritt in den Registrierungsvorgang hinzugefügt. Bei diesem Modell Wenn ein neuer Benutzer registriert werden diese eine e-Mail-Nachricht gesendet, die einen Link zu einer Überprüfungsseite enthält. Besuchen Sie den Link hat sich der Benutzer herausgestellt, dass sie die e-Mail-Adresse empfangen, und daher, dass die e-Mail-bereitgestellte Adresse gültig ist. Die Seite "Überprüfung" ist verantwortlich für die Genehmigung des Benutzers. Dies kann vorkommen, automatisch zu genehmigen und jeder Benutzer, die auf dieser Seite erreicht oder, wenn der Benutzer die zusätzliche Informationen, wie z. B. gibt einen [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Um diesen Workflow zu unterstützen, müssen wir die kontoerstellungsseite zuerst ein update, damit neue Benutzer nicht genehmigt sind. Öffnen der `EnhancedCreateUserWizard.aspx` auf der Seite die `Membership` Ordner "und" Set-Steuerelement CreateUserWizard `DisableCreatedUser` Eigenschaft `true`.

Als Nächstes müssen wir das Steuerelement CreateUserWizard zum Senden einer e-Mail an den neuen Benutzer mit Anweisungen zum Überprüfen ihres Kontos konfigurieren. Insbesondere werden wir enthalten einen Link in der e-Mail, um die `Verification.aspx` Seite (die haben wir noch zu erstellen), und übergeben Sie des neuen Benutzers `UserId` über die Abfragezeichenfolge. Die `Verification.aspx` Seite den angegebenen Benutzer zu suchen und markieren sie genehmigt.

### <a name="sending-a-verification-email-to-new-users"></a>Senden eine E-Mail zur Verifizierung an neue Benutzer

Konfigurieren Sie zum Senden einer e-Mail aus dem Steuerelement CreateUserWizard, dessen `MailDefinition` Eigenschaft entsprechend. Siehe die <a id="Tutorial13"> </a> [vorherigen Tutorial](recovering-and-changing-passwords-cs.md), ChangePassword und PasswordRecovery Steuerelementen gehören eine [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) , funktioniert in gleicher Weise wie CreateUserWizard des Steuerelements.

> [!NOTE]
> Verwenden der `MailDefinition` Eigenschaft, um e-Mail-Übermittlung anzugeben Sie müssen, die Optionen auf `Web.config`. Weitere Informationen finden Sie unter [Senden von e-Mail-Adresse in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Zunächst erstellen Sie eine neue e-Mail-Vorlage, die mit dem Namen `CreateUserWizard.txt` in die `EmailTemplates` Ordner. Verwenden Sie den folgenden Text, für die Vorlage:

[!code-aspx[Main](unlocking-and-approving-user-accounts-cs/samples/sample3.aspx)]

Legen Sie die `MailDefinition'` s `BodyFileName` Eigenschaft, um "~ / EmailTemplates/CreateUserWizard.txt" und die zugehörige `Subject` Eigenschaft auf "Willkommen in meiner Website! Aktivieren Sie Ihr Konto."

Beachten Sie, dass die `CreateUserWizard.txt` -e-Mail-Vorlage enthält einen `<%VerificationUrl%>` Platzhalter. Dies ist, wenn die URL für die `Verification.aspx` Seite platziert werden. Die CreateUserWizard automatisch ersetzt die `<%UserName%>` und `<%Password%>` Platzhalter mit Benutzername und Kennwort, des neuen Kontos, aber es keine integrierten gibt `<%VerificationUrl%>` Platzhalter. Wir müssen manuell durch die entsprechenden überprüfungs-URL ersetzen.

Um dies zu erreichen, erstellen Sie einen Ereignishandler für die CreateUserWizard des [ `SendingMail` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample4.cs)]

Die `SendingMail` Ereignis wird ausgelöst, nachdem die `CreatedUser` -Ereignis, was bedeutet, dass mit der Zeit, die der oben genannten-Ereignishandler ausgeführt, den neuen Benutzer wird Konto bereits erstellt wurde. Können wir auf des neuen Benutzers zugreifen `UserId` -Wert durch Aufrufen der `Membership.GetUser` Methode und übergeben die `UserName` in das Steuerelement CreateUserWizard eingegeben. Als Nächstes wird die überprüfungs-URL gebildet wird. Die Anweisung `Request.Url.GetLeftPart(UriPartial.Authority)` gibt die `http://yourserver.com` Teil der URL; `Request.ApplicationPath` gibt den Pfad, in dem die Anwendung wurden die nutzungsbeschränkungen entfernt. Die Überprüfung der URL wird dann als definiert `Verification.aspx?ID=userId`. Klicken Sie dann werden diese zwei Zeichenfolgen verkettet, um die vollständige URL zu bilden. Zum Schluss die e-Mail-Texts (`e.Message.Body`) verfügt über alle Vorkommen von `<%VerificationUrl%>` mit der vollständigen URL ersetzt.

Das Endergebnis ist, dass neue Benutzer nicht genehmigt, was bedeutet, dass sie an der Website anmelden darf nicht. Darüber hinaus werden sie automatisch eine e-Mail mit einem Link gesendet, um die überprüfungs-URL (siehe Abbildung 6).


[![Der neue Benutzer erhält eine E-Mail mit einem Link zu die Überprüfungs-URL](unlocking-and-approving-user-accounts-cs/_static/image17.png)](unlocking-and-approving-user-accounts-cs/_static/image16.png)

**Abbildung 6**: der neue Benutzer erhält eine E-Mail mit einem Link zu die Überprüfungs-URL ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image18.png))


> [!NOTE]
> CreateUserWizard des Steuerelements CreateUserWizard Standardschritten in einer Meldung darüber informiert den Benutzer, den Ihrem Konto erstellt wurde, und zeigt eine Schaltfläche "Weiter". Durch Klicken gelangt der Benutzer an der URL des Steuerelements gemäß `ContinueDestinationPageUrl` Eigenschaft. Die CreateUserWizard in `EnhancedCreateUserWizard.aspx` ist konfiguriert, um neue Benutzer zum Senden der `~/Membership/AdditionalUserInfo.aspx`, für ihre Heimatstadt Homepage-URL und Signatur der Benutzer wird aufgefordert. Da diese Informationen nur durch kann angemeldete Benutzer hinzugefügt werden, ist es sinnvoll, diese Eigenschaft, um Benutzer zu senden, an der Website-Homepage aktualisieren (`~/Default.aspx`). Darüber hinaus die `EnhancedCreateUserWizard.aspx` Seite oder der CreateUserWizard Schritt sollte dem Benutzer informieren, dass sie eine e-Mail zur Verifizierung gesendet wurden, und ihr Konto wird nicht aktiviert werden, bis sie die Anweisungen in dieser e-Mail folgen verbessert werden. Ich lassen Sie diese Änderungen als Übung für den Leser.


### <a name="creating-the-verification-page"></a>Erstellen die Seite "Überprüfung"

Unsere letzte Aufgabe ist die Erstellung der `Verification.aspx` Seite. Fügen Sie diese Seite in den Stammordner, ordnet ihm dabei die `Site.master` Masterseite. Wie bei den meisten von den vorherigen Inhalt Seiten der Website hinzugefügt haben, entfernen Sie das Inhaltssteuerelement, das verweist auf die `LoginContent` ContentPlaceHolder so, dass die Seite Inhalte der Masterseite verwendet Standardinhaltstyp.

Fügen Sie ein Label-Steuerelement auf der `Verification.aspx` Seite die `ID` zu `StatusMessage` und lösche die Text-Eigenschaft. Erstellen Sie als Nächstes die `Page_Load` -Ereignishandler, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](unlocking-and-approving-user-accounts-cs/samples/sample5.cs)]

Der größte Teil der obige Code überprüft, ob die `UserId` angegebenen über die Abfragezeichenfolge vorhanden ist, dass er ein gültiger `Guid` -Wert, und verweist, ein bestehendes Benutzerkonto an. Wenn alle diese Überprüfungen übergeben, ist das Benutzerkonto, das genehmigt. Andernfalls wird eine geeignete Statusmeldung angezeigt.

Abbildung 7 zeigt die `Verification.aspx` Seite, wenn über einen Browser aufgerufen.


[![Das neue Benutzerkonto wird nun genehmigt](unlocking-and-approving-user-accounts-cs/_static/image20.png)](unlocking-and-approving-user-accounts-cs/_static/image19.png)

**Abbildung 7**: das neue Benutzerkonto wird nun genehmigt ([klicken Sie, um das Bild in voller Größe anzeigen](unlocking-and-approving-user-accounts-cs/_static/image21.png))


## <a name="summary"></a>Zusammenfassung

Alle Benutzer eines Mitgliedskonten weisen zwei Status, die bestimmen, ob der Benutzer an der Website anmelden kann: `IsLockedOut` und `IsApproved`. Diese beiden Eigenschaften muss `true` für den Benutzer, die Anmeldung.

Der Benutzer gesperrt der Status wird als Sicherheitsmaßnahme verwendet, um die Wahrscheinlichkeit, dass ein Hacker wichtige zu einem Standort, über brute-Force-Methoden zu reduzieren. Insbesondere ist ein Benutzer gesperrt, wenn eine bestimmte Anzahl von ungültigen Anmeldeversuchen innerhalb eines bestimmten Zeitfensters vorhanden sind. Diese Grenzen sind konfigurierbar, über die Membership-Provider-Einstellungen in `Web.config`.

Der Status "genehmigt" wird häufig als Mittel verwendet, um zu verhindern, dass neue Benutzer anmelden, bis eine Aktion wiederholt wurde. Vielleicht die Site erforderlich ist, dass neue Konten, zunächst vom Administrator genehmigt werden und wie wir in Schritt 3 gesehen haben, indem Sie ihre e-Mail-Adresse zu überprüfen.

Viel Spaß beim Programmieren!

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](recovering-and-changing-passwords-cs.md)
> [Weiter](building-an-interface-to-select-one-user-account-from-many-vb.md)
