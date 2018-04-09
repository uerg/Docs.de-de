---
uid: web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
title: Entsperren und Genehmigen von Benutzerkonten (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer Webseite für Administratoren zur Verwaltung des Benutzers gesperrt, und Status genehmigt. Wir sehen auch neue Benutzer o genehmigen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 041854a5-ea8c-4de0-82f1-121ba6cb2893
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/unlocking-and-approving-user-accounts-vb
msc.type: authoredcontent
ms.openlocfilehash: 977ed9d1e68e635eadd7aa8339670f5a5d209a7b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="unlocking-and-approving-user-accounts-vb"></a>Entsperren und Genehmigen von Benutzerkonten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.14.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial14_UnlockAndApprove_vb.pdf)

> In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer Webseite für Administratoren zur Verwaltung des Benutzers gesperrt, und Status genehmigt. Wir sehen auch neue Benutzer zu genehmigen, nachdem sie ihre e-Mail-Adresse überprüft haben.


## <a name="introduction"></a>Einführung

Zusammen mit einem Benutzernamen, Kennwort und eine e-Mail, hat jedes Benutzerkonto zwei Statusfelder, die vorgeben, ob der Benutzer bei der Website anmelden kann: gesperrt und genehmigt. Ein Benutzer wird automatisch gesperrt, wenn sie ungültigen Anmeldeinformationen angeben (die Standardeinstellungen ausgesperrt ein Benutzers nach 5 ungültigen Anmeldeversuchen innerhalb von 10 Minuten) eine angegebene Anzahl von Malen innerhalb einer angegebenen Anzahl von Minuten. Der Status "genehmigt" ist nützlich in Szenarien, in denen eine Aktion Zeitspanne für ein neuer Benutzer der Website anmelden kann. Benutzer müssen möglicherweise zuerst Überprüfen ihrer e-Mail-Adresse oder von einem Administrator genehmigt werden, bevor er anmelden können.

Da eine gesperrte oder nicht genehmigte Benutzer anmelden kann nicht, ist es nur natürlichen weiß, wie die von diesen Statuswerten zurückgesetzt werden können. ASP.NET enthält keine integrierten Funktionen oder Websteuerelemente zum Verwalten von Benutzern gesperrt und Statusangaben, teilweise genehmigt, da diese Entscheidungen auf Basis von Standorten verarbeitet werden müssen. Einige Websites können alle neuen Benutzerkonten (das Standardverhalten) automatisch genehmigt werden. Andere besitzen von einem Administrator neue Konten zu genehmigen oder Benutzer nicht genehmigt, bis sie einen Link, um die e-Mail-Adresse bereitgestellt, wenn sie sich registriert gesendet besuchen. Ebenso einige Websites können Benutzer sperren, bis ihr Status von einem Administrator zurückgesetzt, während andere Standorte eine e-Mail an die Sperre auch Benutzer mit einer URL senden sie um ihr Konto zu entsperren besuchen können.

In diesem Lernprogramm wird gezeigt, wie zum Erstellen einer Webseite für Administratoren zur Verwaltung des Benutzers gesperrt, und Status genehmigt. Wir sehen auch neue Benutzer zu genehmigen, nachdem sie ihre e-Mail-Adresse überprüft haben.

## <a name="step-1-managing-users-locked-out-and-approved-statuses"></a>Schritt 1: Verwalten von Benutzern gesperrt, und Status genehmigt

In der <a id="Tutorial12"> </a> [ *erstellen eine Schnittstelle, wählen Sie ein Benutzerkonto an, über viele* ](building-an-interface-to-select-one-user-account-from-many-vb.md) es erstellt eine Seite, die jedem Benutzerkonto in einem ausgelagerten aufgeführten Lernprogramm gefiltert GridView. Die Raster werden die Namen des Benutzers und e-Mail-, deren Status genehmigte und gesperrt, ob sie zurzeit online sind und Kommentare zu dem Benutzer aufgeführt. Zum Verwalten von Benutzern genehmigt und gesperrt, Status, wir können dieses Raster bearbeitet werden. Um einen Benutzer Status "genehmigt" zu ändern, würde der Administrator zunächst suchen Sie das Benutzerkonto und bearbeiten Sie dann die entsprechende GridView Zeile aus, aktivieren oder deaktivieren das Kontrollkästchen genehmigte. Alternativ kann es der genehmigten und gesperrten Status über eine separate ASP.NET-Seite verwalten.

Für dieses Lernprogramm nutzen wir zwei ASP.NET-Seiten: `ManageUsers.aspx` und `UserInformation.aspx`. Der Grundgedanke besteht, die `ManageUsers.aspx` Listet die Benutzerkonten im System, während `UserInformation.aspx` ermöglicht dem Administrator der genehmigten und gesperrten Status für einen bestimmten Benutzer zu verwalten. Erste Order Of Geschäftsverkehr wird erweitern, die GridView in `ManageUsers.aspx` um eine HyperLinkField einzuschließen, die als eine Spalte von Links gerendert wird. Jeder Link auf Being `UserInformation.aspx?user=UserName`, wobei *Benutzername* ist der Name des Benutzers zu bearbeiten.

> [!NOTE]
> Wenn Sie den Code heruntergeladen haben die <a id="Tutorial13"> </a> [ *Recovering und Ändern von Kennwörtern* ](recovering-and-changing-passwords-vb.md) Lernprogramm Ihnen möglicherweise aufgefallen, die die `ManageUsers.aspx` Seite enthält bereits eine Reihe von " Verwalten von"Links und die `UserInformation.aspx` Seite stellt eine Schnittstelle für das Kennwort des ausgewählten Benutzers ändern. Ich möchte nicht replizieren diese Funktion in der Code in diesem Lernprogramm zugeordnet, da der Vorgang erfolgreich war, von der Mitgliedschaft-API zu umgehen und direkt mit der SQL Server-Datenbank so ändern Sie das Kennwort eines Benutzers ausgeführt. In diesem Lernprogramm beginnt von Grund auf neu, mit der `UserInformation.aspx` Seite.


### <a name="adding-manage-links-to-theuseraccountsgridview"></a>Hinzufügen von Links zu "verwalten" die`UserAccounts`GridView

Öffnen der `ManageUsers.aspx` Seite, und fügen Sie eine HyperLinkField auf die `UserAccounts` GridView. Legen Sie die HyperLinkField `Text` -Eigenschaft auf "Verwalten" und die zugehörige `DataNavigateUrlFields` und `DataNavigateUrlFormatString` Eigenschaften `UserName` und "UserInformation.aspx?user={0}", bzw. Diese Einstellungen der HyperLinkField so konfigurieren, dass alle der folgenden Hyperlinks Option wird der Text "Manage", wobei jedoch jeder Link, in der entsprechenden weiterleitet *Benutzername* Wert in die Abfragezeichenfolge.

Nehmen Sie nach dem Hinzufügen der HyperLinkField an die GridView, einen Moment Zeit zum Anzeigen der `ManageUsers.aspx` Seite über einen Browser. Wie in Abbildung 1 gezeigt, enthält jede Zeile GridView nun einen Link "Verwalten". Der Link "Verwalten" für Bruce verweist auf `UserInformation.aspx?user=Bruce`, während auf der Link "Verwalten" für Dave zeigt `UserInformation.aspx?user=Dave`.


[![Fügt der HyperLinkField ein](unlocking-and-approving-user-accounts-vb/_static/image2.png)](unlocking-and-approving-user-accounts-vb/_static/image1.png)

**Abbildung 1**: die HyperLinkField Fügt einen Link "Verwalten" für jedes Benutzerkonto ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image3.png))


Wir erstellen der Benutzeroberfläche und code für die `UserInformation.aspx` auf der Seite einen Moment Zeit, doch zunächst sehen wir reden über so ändern Sie einen Benutzer programmgesteuert gesperrt und Status genehmigt. Die [ `MembershipUser` Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) hat [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx) und [ `IsApproved` Eigenschaften](https://msdn.microsoft.com/library/system.web.security.membershipuser.isapproved.aspx). Die `IsLockedOut` Eigenschaft ist schreibgeschützt. Es gibt keinen Mechanismus zum programmgesteuert zu sperren, bis ein Benutzer; Verwenden Sie zum Entsperren eines Benutzers die `MembershipUser` Klasse [ `UnlockUser` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx). Die `IsApproved` Eigenschaft ist lesbar und schreibbar. Zum Speichern von Änderungen an dieser Eigenschaft müssen wir rufen die `Membership` Klasse [ `UpdateUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx), und übergeben Sie das geänderte `MembershipUser` Objekt.

Da die `IsApproved` Eigenschaft lesbar und beschreibbar ist, ein CheckBox-Steuerelement ist wahrscheinlich die beste Element der Benutzeroberfläche für das Konfigurieren dieser Eigenschaft. Allerdings ein Kontrollkästchen funktionieren nicht bei der `IsLockedOut` Eigenschaft, da ein Administrator ein Benutzer gesperrt werden kann, kann sie nur einen Benutzer freigeben. Eine geeignete Benutzeroberfläche für die `IsLockedOut` Eigenschaft ist eine Schaltfläche, die beim Klicken auf Entsperren Sie das Benutzerkonto ein. Diese Schaltfläche sollte nur aktiviert werden, wenn der Benutzer gesperrt ist.

### <a name="creating-theuserinformationaspxpage"></a>Erstellen der`UserInformation.aspx`Seite

Wir können jetzt in die Benutzeroberfläche implementieren `UserInformation.aspx`. Öffnen Sie die Seite, und fügen Sie die folgenden Web-Steuerelemente hinzu:

- Ein Link zu steuern, die, wenn angeklickt, gibt den Administrator die `ManageUsers.aspx` Seite.
- Ein Websteuerelement Bezeichnung für die Anzeige von den ausgewählten Benutzernamen an. Legen Sie diese Bezeichnung `ID` auf `UserNameLabel` und seine Texteigenschaft zu löschen.
- Ein CheckBox-Steuerelement mit dem Namen `IsApproved`. Legen Sie dessen `AutoPostBack` Eigenschaft `True`.
- Label-Steuerelement für die Anzeige des Benutzers des letzten gesperrt, bis Datum. Benennen Sie diese Bezeichnung `LastLockedOutDateLabel` und löschen, dessen `Text` Eigenschaft.
- Eine Schaltfläche zum Entsperren des Benutzers. Nennen Sie diese Schaltfläche `UnlockUserButton` und legen Sie dessen `Text` Eigenschaft "Entsperren von Benutzer".
- Label-Steuerelement zum Anzeigen von statusmeldungen, z. B. "Status" genehmigt "(der Benutzer aktualisiert wurde." Nennen Sie dieses Steuerelement `StatusMessage`deaktivieren, dessen `Text` Eigenschaft, und legen seine `CssClass` Eigenschaft `Important`. (Die `Important` CSS-Klasse definiert ist, der `Styles.css` Stylesheet-Datei den entsprechenden Text in einer Schriftart große, rot angezeigt.)

Nach dem Hinzufügen der folgenden Steuerelemente, sollte die Entwurfsansicht in Visual Studio ähnlich wie im Screenshot in Abbildung 2 aussehen.


[![Erstellen der Benutzeroberfläche für UserInformation.aspx](unlocking-and-approving-user-accounts-vb/_static/image5.png)](unlocking-and-approving-user-accounts-vb/_static/image4.png)

**Abbildung 2**: Erstellen der Benutzeroberfläche für `UserInformation.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image6.png))


Mit der vollständigen Benutzeroberfläche unsere nächste Aufgabe besteht, Festlegen der `IsApproved` Kontrollkästchen und anderen Steuerelementen auf Basis des ausgewählten Benutzers Informationen. Erstellen Sie einen Ereignishandler für der Seite `Load` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample1.vb)]

Der obige Code wird gestartet, indem Sie sicherstellen, dass dies die ersten Besuch der Seite und keiner nachfolgenden Postbacks ist. Er liest dann den Benutzernamen durch Übergeben der `user` Querystring-Feld und ruft Informationen über das Benutzerkonto über die `Membership.GetUser(username)` Methode. Wenn kein Benutzername, über die Abfragezeichenfolge angegeben wurde oder wenn der angegebene Benutzer nicht gefunden werden kann, wird der Administrator gesendet, an die `ManageUsers.aspx` Seite.

Die `MembershipUser` des Objekts `UserName` Wert wird dann angezeigt, der `UserNameLabel` und `IsApproved` aktiviert ist auf der Grundlage der `IsApproved` Eigenschaftswert.

Die `MembershipUser` des Objekts [ `LastLockoutDate` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membershipuser.lastlockoutdate.aspx) gibt eine `DateTime` Wert, der angibt, wenn der Benutzer zuletzt wurde gesperrt. Wenn der Benutzer nicht gesperrt wurde, hängt der zurückgegebene Wert den Mitgliedschaftsanbieter ab. Wenn ein neues Konto erstellt wird, die `SqlMembershipProvider` legt die `aspnet_Membership` tabellenspezifischen `LastLockoutDate` Feld `1754-01-01 12:00:00 AM`. Der obige Code zeigt eine leere Zeichenfolge in der `LastLockoutDateLabel` Wenn die `LastLockoutDate` -Eigenschaft tritt vor dem Jahr 2000 ist, andernfalls der Date-Teil der `LastLockoutDate` Eigenschaft wird in der Bezeichnung angezeigt. Die `UnlockUserButton`des `Enabled` Eigenschaft ist gesperrt, Status, was bedeutet, dass diese Schaltfläche nur aktiviert werden, wird Wenn der Benutzer gesperrt wird, für des Benutzers festgelegt.

Erkundet zum Testen der `UserInformation.aspx` Seite über einen Browser. Natürlich müssen Sie am start `ManageUsers.aspx` , und wählen Sie ein Benutzerkonto zu verwalten. Beim Eintreffen an `UserInformation.aspx`, beachten Sie, dass die `IsApproved` Kontrollkästchen ist nur aktiviert, wenn der Benutzer genehmigt wird. Wenn der Benutzer jemals gesperrt wurde, wird ihre letzte Datum gesperrt angezeigt. Die Schaltfläche "Benutzer entsperren" nur aktiviert, wenn der Benutzer aktuell gesperrt ist. Aktivieren oder Deaktivieren der `IsApproved` Kontrollkästchen oder durch Klicken auf die Schaltfläche "Benutzer entsperren" bewirkt, dass einen Postback, jedoch werden mit dem Benutzerkonto keine Änderungen vorgenommen, da wir noch, haben die Ereignishandler für diese Ereignisse erstellen.

Zurück zu Visual Studio und Erstellen von Ereignishandlern für die `IsApproved` Kontrollkästchens `CheckedChanged` Ereignis und die `UnlockUser` Schaltfläche `Click` Ereignis. In der `CheckedChanged` Ereignishandler, d. h. Festlegen des Benutzers `IsApproved` Eigenschaft, um die `Checked` Eigenschaft des Kontrollkästchens und speichern Sie die Änderungen über einen Aufruf an `Membership.UpdateUser`. In der `Click` Ereignishandler, d. h. einfach Aufruf der `MembershipUser` des Objekts `UnlockUser` Methode. In beiden Ereignishandler anzeigen eine geeignete Nachricht in die `StatusMessage` Bezeichnung.

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample2.vb)]

### <a name="testing-theuserinformationaspxpage"></a>Testen der`UserInformation.aspx`Seite

Rufen Sie die Seite mit diesen Ereignishandler vorhanden, und nicht genehmigte Benutzer. Wie in Abbildung 3 gezeigt, sehen Sie eine kurze, die auf der Seite zeigt an, dass des Benutzers Nachrichten `IsApproved` Eigenschaft wurde erfolgreich geändert.


[![Chris wurde nicht genehmigt wurden.](unlocking-and-approving-user-accounts-vb/_static/image8.png)](unlocking-and-approving-user-accounts-vb/_static/image7.png)

**Abbildung 3**: Chris wurde der nicht genehmigten ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image9.png))


Abmelden und versuchen Sie es als der Benutzer, dessen Konto anmelden, wurde als Nächstes einfach nicht genehmigt. Da der Benutzer nicht genehmigt wird, kann nicht bei der Anmeldung. Standardmäßig werden das Steuerelement für die Anmeldung dieselbe Nachricht angezeigt, wenn der Benutzer, müssen Sie unabhängig von der Ursache anmelden kann. Aber in der <a id="Tutorial6"> </a> [ *überprüfen Benutzer Anmeldeinformationen für die Mitgliedschaft Benutzerspeicher* ](../membership/validating-user-credentials-against-the-membership-user-store-vb.md) Lernprogramm erläutert, erweitern das Anmelde-Steuerelement, um eine besser geeignete Meldung anzeigen. Wie in Abbildung 4 gezeigt, wird Chris dargestellt, eine Meldung angezeigt, dass er sich anmelden kann, da sein Konto noch nicht genehmigt ist.


[![Torsten kann nicht da His Anmeldekonto ist nicht genehmigt](unlocking-and-approving-user-accounts-vb/_static/image11.png)](unlocking-and-approving-user-accounts-vb/_static/image10.png)

**Abbildung 4**: Chris kann nicht da His Anmeldekonto ist der nicht genehmigten ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image12.png))


Um die Sperre auch Funktionalität zu testen, versuchen Sie, eine genehmigte Benutzer angemeldet, aber verwenden ein falsches Kennwort. Wiederholen Sie diesen Prozess die erforderliche Anzahl an, wie oft, bis das Konto des Benutzers gesperrt wurde. Das Steuerelement für die Anmeldung wurde ebenfalls aktualisiert, um das Anzeigen einer benutzerdefiniertes Meldung beim Versuch, auf die von einem gesperrten Konto anzumelden. Sie wissen, dass ein Konto gesperrt wurde, nachdem Sie die folgende Meldung auf der Anmeldeseite bandbreitenvorlagen: "Ihr Konto wurde gesperrt, da zu viele ungültige Anmeldeversuche. Wenden Sie sich an den Administrator, um Ihr Konto entsperrt haben."

Zurück zu den `ManageUsers.aspx` Seite, und klicken Sie auf den Link "verwalten" für den Benutzer gesperrt. Wie in Abbildung 5 gezeigt wird, sehen Sie einen Wert in der `LastLockedOutDateLabel` die Schaltfläche "Benutzer entsperren" aktiviert werden soll. Klicken Sie auf die Schaltfläche mit den Benutzer entsperren, um das Benutzerkonto zu entsperren. Nachdem Sie die Benutzer freigegeben haben, werden sie sich erneut anmelden.


[![Dave wurde aus dem System gebunden.](unlocking-and-approving-user-accounts-vb/_static/image14.png)](unlocking-and-approving-user-accounts-vb/_static/image13.png)

**Abbildung 5**: winformsexampleupdates von Dave hat wurde gesperrt, des Systems ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image15.png))


## <a name="step-2-specifying-new-users-approved-status"></a>Schritt 2: Angeben von neuer Benutzers Status genehmigt

Der Status "genehmigt" ist nützlich in Szenarien, in dem Sie eine Aktion ausgeführt werden, bevor ein neuer Benutzer kann sich anmelden und auf die benutzerspezifischen-Funktionen des Standorts. Beispielsweise können Sie eine private Website ausgeführt werden, in dem alle Seiten, mit Ausnahme der Seiten Anmeldung und Registrierung nur für authentifizierte Benutzer zugegriffen werden kann. Aber was geschieht, wenn Sie ein fremden Ihrer Website erreicht wird die Anmeldeseite und erstellt ein Konto? Um dies zu vermeiden, die Sie auf die Anmeldeseite verschieben konnte ein `Administration` Ordner und erfordern, dass ein Administrator jedes Konto manuell erstellen. Alternativ können Sie zulassen des Zugriffs auf Registrierung jedoch Zugriff auf die Website verhindert werden soll, bis der Administrator das Benutzerkonto bestätigt.

Standardmäßig genehmigt Steuerelement CreateUserWizard neue Konten. Sie können dieses Verhalten mithilfe des Steuerelements konfigurieren [ `DisableCreatedUser` Eigenschaft](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx). Legen Sie diese Eigenschaft auf `True` neue Benutzerkonten nicht genehmigen.

> [!NOTE]
> Standardmäßig protokolliert das Steuerelement CreateUserWizard automatisch auf das neue Benutzerkonto an. Dieses Verhalten wird durch des Steuerelements vorgegeben [ `LoginCreatedUser` Eigenschaft](https://msdn.microsoft.com/en-gb/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx). Da die Site nicht genehmigte Benutzer anmelden können beim `DisableCreatedUser` ist `True` das neue Benutzerkonto wird nicht in der Website, unabhängig vom Wert der protokolliert die `LoginCreatedUser` Eigenschaft.


Wenn Sie neue Benutzerkonten mit programmgesteuert erstellen, werden die `Membership.CreateUser` -Methode zum Erstellen eines Benutzerkontos nicht genehmigte verwenden eine der Überladungen, die des neuen Benutzers akzeptieren `IsApproved` Eigenschaftswert als Eingabeparameter.

## <a name="step-3-approving-users-by-verifying-their-email-address"></a>Schritt 3: Genehmigen von Benutzern durch Überprüfen der e-Mail-Adresse

Viele Websites, die Benutzerkonten unterstützen neue Benutzer genehmigen, bis sie die e-Mail-Adresse, die sie überprüfen bei der Registrierung angegeben. Diese Überprüfung wird häufig verwendet, um Bots, Spammern und andere Ne'er-do-wells vereiteln erfordert eine eindeutige, bestätigte e-Mail-Adresse und einen zusätzlichen Schritt in den Registrierungsvorgang hinzugefügt. Mit diesem Modell Wenn ein neuer Benutzer registriert werden diese eine e-Mail-Nachricht gesendet, die einen Link zu einer Seite "Überprüfung" enthält. Besuchen Sie den Link hat der Benutzer sich als, dass sie die e-Mail-Adresse empfangen und daher, dass die e-Mail-Adresse, sofern gültig ist. Die Seite "Überprüfung" dient zum Genehmigen des Benutzers zur Verfügung. Dies kann vorkommen, automatisch zu genehmigen und jeder Benutzer, die auf dieser Seite erreicht oder erst, nachdem der Benutzer zusätzliche Informationen, wie z. B. bietet eine [CAPTCHA](http://en.wikipedia.org/wiki/Captcha).

Um dieses Workflows zu berücksichtigen, müssen wir zunächst die Seite "Konto erstellen" zu aktualisieren, sodass neue Benutzer nicht genehmigt sind. Öffnen der `EnhancedCreateUserWizard.aspx` auf der Seite der `Membership` Ordner und Set-Steuerelement CreateUserWizard `DisableCreatedUser` Eigenschaft `True`.

Als Nächstes müssen wir das CreateUserWizard-Steuerelement, um eine e-Mail an den neuen Benutzer mit Anweisungen zum Überprüfen von ihrem Konto senden zu konfigurieren. Insbesondere enthalten wir einen Link in der e-Mail, um die `Verification.aspx` Seite (der noch zu wir erstellen), und übergeben Sie des neuen Benutzers `UserId` über die Abfragezeichenfolge. Die `Verification.aspx` Seite für den angegebenen Benutzer zu suchen und markieren sie die genehmigt werden.

### <a name="sending-a-verification-email-to-new-users"></a>Eine Überprüfung der E-Mail an neue Benutzer zu senden.

Um eine e-Mail-Nachricht aus dem Steuerelement CreateUserWizard senden möchten, konfigurieren die `MailDefinition` Eigenschaft entsprechend. Wie in beschrieben die <a id="Tutorial13"> </a> [vorherigen Lernprogramm](recovering-and-changing-passwords-vb.md), die ChangePassword und PasswordRecovery Steuerelemente umfassen eine [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) , funktioniert in gleicher Weise wie CreateUserWizard des Steuerelements.

> [!NOTE]
> Verwenden der `MailDefinition` Eigenschaft müssen Sie die e-Mail-Übermittlung an pipelineoptionen `Web.config`. Weitere Informationen finden Sie unter [Senden von E-Mails in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).


Starten, indem Sie die Erstellung einer neuen e-Mail-Vorlage, die mit dem Namen `CreateUserWizard.txt` in den `EmailTemplates` Ordner. Verwenden Sie den folgenden Text für die Vorlage an:

[!code-aspx[Main](unlocking-and-approving-user-accounts-vb/samples/sample3.aspx)]

Legen Sie die `MailDefinition`des `BodyFileName` -Eigenschaft auf "~ / EmailTemplates/CreateUserWizard.txt" und die zugehörige `Subject` -Eigenschaft auf "Meine Website Willkommen! Aktivieren Sie Ihr Konto."

Beachten Sie, dass die `CreateUserWizard.txt` e-Mail-Vorlage enthält einen `<%VerificationUrl%>` Platzhalter. Dies ist, wenn die URL für die `Verification.aspx` Seite gespeichert werden sollen. Die CreateUserWizard automatisch ersetzt die `<%UserName%>` und `<%Password%>` -Platzhalter durch des neuen Kontos Benutzername und Kennwort, aber es keine integrierte ist `<%VerificationUrl%>` Platzhalter. Wir müssen manuell mit der entsprechenden überprüfungs-URL zu ersetzen.

Um dies zu erreichen, erstellen Sie einen Ereignishandler für die CreateUserWizard [ `SendingMail` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.sendingmail.aspx) und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample4.vb)]

Die `SendingMail` Ereignis wird ausgelöst, nachdem die `CreatedUser` Ereignis, was bedeutet, dass nach der Zeit, die oben genannten-Ereignishandler ausgeführt, den neuen Benutzer wird, Konto bereits erstellt wurde. Wir können des neuen Benutzers zugreifen `UserId` -Wert durch Aufrufen der `Membership.GetUser` -Methode auf und übergibt die `UserName` in das Steuerelement CreateUserWizard eingegeben. Als Nächstes wird die überprüfungs-URL gebildet wird. Die Anweisung `Request.Url.GetLeftPart(UriPartial.Authority)` gibt die `http://yourserver.com` Teil der URL; `Request.ApplicationPath` gibt Pfad, in dem die Anwendung als Stammelement. Überprüfung der URL wird dann als definiert `Verification.aspx?ID=userId`. Diese zwei Zeichenfolgen werden dann verkettet, um die vollständige URL zu bilden. Zum Schluss die e-Mail-Nachrichtentext (`e.Message.Body`) verfügt über alle Vorkommen von `<%VerificationUrl%>` mit der vollständigen URL ersetzt.

Im Endeffekt ist, dass neue Benutzer sind nicht genehmigt, was bedeutet, dass sie bei der Website anmelden können. Darüber hinaus sind sie automatisch eine e-Mail mit einem Link gesendet, um die überprüfungs-URL (siehe Abbildung 6).


[![Der neue Benutzer erhält eine E-Mail mit einem Link, um die Überprüfungs-URL](unlocking-and-approving-user-accounts-vb/_static/image17.png)](unlocking-and-approving-user-accounts-vb/_static/image16.png)

**Abbildung 6**: der neue Benutzer erhält eine E-Mail mit einem Link, um die Überprüfungs-URL ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image18.png))


> [!NOTE]
> Den CreateUserWizard Standard CreateUserWizard Schritt zeigt eine Meldung, die der Benutzers, die ihrem Konto erstellt wurde, und zeigt eine Schaltfläche "Weiter". Dadurch wird der Benutzer an der URL des Steuerelements gemäß `ContinueDestinationPageUrl` Eigenschaft. Die CreateUserWizard in `EnhancedCreateUserWizard.aspx` wird konfiguriert, um neue Benutzer zu senden, die `~/Membership/AdditionalUserInfo.aspx`, die Bestätigung des Benutzers ihre Hometown, die URL der Homepage und die Signatur. Da diese Informationen nur durch hinzugefügt werden kann Benutzer angemeldet sind, wird es ist sinnvoll, aktualisieren Sie diese Eigenschaft, um Benutzer zu senden, an der Website-Homepage (`~/Default.aspx`). Darüber hinaus die `EnhancedCreateUserWizard.aspx` Seite oder den Schritt CreateUserWizard um den Benutzer zu informieren, dass sie eine Überprüfung der e-Mail-Adresse gesendet wurden und ihrem Konto nicht aktiviert, bis sie die Anweisungen in dieser e-Mail entsprechen verbessert werden. Ich lassen diese Änderungen als Übung für den Leser.


### <a name="creating-the-verification-page"></a>Erstellen die Seite "Überprüfung"

Unsere letzte Aufgabe ist die Erstellung der `Verification.aspx` Seite. Fügen Sie diese Seite in den Stammordner, und verknüpfen es mit der `Site.master` Masterseite. Wie es bei den meisten, von den vorherigen Inhalt Seiten der Website hinzugefügt haben, entfernen Sie das Inhaltssteuerelement, das verweist auf die `LoginContent` ContentPlaceHolder so, dass die Seite Inhalte der Masterseite verwendet standardmäßigen Inhalt.

Fügen Sie eine Bezeichnung Websteuerelement auf die `Verification.aspx` Seite seine `ID` auf `StatusMessage` und löschen Sie seine Texteigenschaft. Als Nächstes erstellen Sie die `Page_Load` -Ereignishandler folgenden Code hinzu:

[!code-vb[Main](unlocking-and-approving-user-accounts-vb/samples/sample5.vb)]

Der Großteil der obige Code stellt sicher, dass die Benutzer-ID angegeben, über die Abfragezeichenfolge vorhanden ist, dass es ungültig ist `Guid` Wert und ein vorhandenen Benutzerkonto verweist. Wenn alle Prüfungen übergeben, ist das Benutzerkonto genehmigt. Andernfalls wird eine geeignete Statusmeldung angezeigt.

Abbildung 7 zeigt ein Beispiel der `Verification.aspx` Startseite bei der über einen Browser besucht hat.


[![Das neue Benutzerkonto wird nun genehmigt](unlocking-and-approving-user-accounts-vb/_static/image20.png)](unlocking-and-approving-user-accounts-vb/_static/image19.png)

**Abbildung 7**: das neue Benutzerkonto wird nun genehmigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](unlocking-and-approving-user-accounts-vb/_static/image21.png))


## <a name="summary"></a>Zusammenfassung

Alle Benutzerkonten für die Mitgliedschaft haben zwei Status, die bestimmen, ob der Benutzer bei der Website anmelden kann: `IsLockedOut` und `IsApproved`. Beide Eigenschaften muss `True` für den Benutzer, die Anmeldung.

Der Benutzer gesperrt der Status wird als Sicherheitsmaßnahme verwendet, um die Wahrscheinlichkeit, dass ein Hacker wichtige zu einem Standort, über Brute-Force-Methoden zu reduzieren. Insbesondere ist ein Benutzer gesperrt, wenn eine bestimmte Anzahl von ungültigen Anmeldeversuchen innerhalb eines bestimmten Fensters Zeit vorhanden sind. Diese Grenzen sind nicht konfigurierbar über die Mitgliedschaft anbietereinstellungen in `Web.config`.

Der Status "genehmigt" wird häufig als Mittel verwendet, um zu verhindern, dass neue Benutzer anmelden, bis eine Aktion vergangen ist. Vielleicht die Site erforderlich ist, dass neue Konten zuerst vom Administrator genehmigt werden oder wie in Schritt 3 veranschaulichte Filtermenü durch ihre e-Mail-Adresse überprüfen.

Viel Spaß beim Programmieren!

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Vorherige](recovering-and-changing-passwords-vb.md)
