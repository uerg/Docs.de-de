---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
title: "Wiederherstellen und Ändern von Kennwörtern (c#) | Microsoft Docs"
author: rick-anderson
description: "ASP.NET umfasst zwei Websteuerelemente für die Unterstützung bei der Wiederherstellung oder Kennwörter ändern. Das Steuerelement PasswordRecovery ermöglicht einen Besucher seine verloren Pa wiederherstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: 19c4d042-4e34-4b44-9f1d-6bf2253ba366
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-cs
msc.type: authoredcontent
ms.openlocfilehash: 76c02a3da7dffad25a7bee03efff6b693f261d85
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="recovering-and-changing-passwords-c"></a>Wiederherstellen und Ändern von Kennwörtern (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/CS.13.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_cs.pdf)

> ASP.NET umfasst zwei Websteuerelemente für die Unterstützung bei der Wiederherstellung oder Kennwörter ändern. Das PasswordRecovery-Steuerelement aktiviert einen Besucher seine eines verlorenen Kennworts wiederhergestellt. Das Steuerelement ChangePassword kann der Benutzer sein Kennwort aktualisieren. Wie die anderen Anmeldenamen bezogene Websteuerelemente kennen gelernt haben in dieser Reihe von Lernprogrammen, die PasswordRecovery und ChangePassword steuert die Arbeit mit dem Framework Mitgliedschaft im Hintergrund zurücksetzen oder Ändern von Benutzerkennwörtern.


## <a name="introduction"></a>Einführung

Zwischen den Websites für meine Bank, Hilfsprogramm Unternehmen Phone Unternehmens, e-Mail-Konten und personalisierten Webportale müssen, wie die meisten Personen Dutzende von verschiedene Kennwörter zu merken. Mit so vielen Anmeldeinformationen heutzutage merken ist es nicht ungewöhnlich, dass Benutzer ihr Kennwort vergessen. Um dies zu berücksichtigen, müssen Websites, die Benutzerkonten bieten eine Möglichkeit für einen Benutzer sein Kennwort wiederherstellen. Dieser Prozess schließt normalerweise ein neues, zufälliges Kennwort generiert und an den Benutzer e-Mail-Adresse auf die Datei zu senden. Nach dem Empfang von ihr neuen Kennworts die meisten Benutzer zu der Website zurückkehren, und ihr Kennwort ändern, von dem nach dem Zufallsprinzip generierte in einen einprägsameren.

ASP.NET umfasst zwei Websteuerelemente für die Unterstützung bei der Wiederherstellung oder Kennwörter ändern. Das PasswordRecovery-Steuerelement aktiviert einen Besucher seine eines verlorenen Kennworts wiederhergestellt. Das Steuerelement ChangePassword kann der Benutzer sein Kennwort aktualisieren. Wie die anderen Anmeldenamen bezogene Websteuerelemente kennen gelernt haben in dieser Reihe von Lernprogrammen, die PasswordRecovery und ChangePassword steuert die Arbeit mit dem Framework Mitgliedschaft im Hintergrund zurücksetzen oder Ändern von Benutzerkennwörtern.

In diesem Lernprogramm untersuchen wir, diese beiden Steuerelemente verwenden. Siehe wir auch das programmgesteuerte Ändern und Zurücksetzen des Kennworts eines Benutzers über die `MembershipUser` Klasse `ChangePassword` und `ResetPassword` Methoden.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Schritt 1: Helfen Benutzern wiederherstellen Kennwörter verloren

Alle Websites, die Benutzerkonten zu unterstützen, müssen Benutzern einen Mechanismus zum Wiederherstellen ihrer vergessene Kennwörter bereitstellen. Die gute Nachricht ist, dass solche Funktionen in ASP.NET implementieren ein Kinderspiel Dank des PasswordRecovery Websteuerelements. Das PasswordRecovery-Steuerelement gerendert wird, eine Schnittstelle, die die eingabeaufforderungen für ihren Benutzernamen und ggf. die Antwort auf die Sicherheitsfrage. Es per e-Mail klicken Sie dann die Benutzer ihr Kennwort.

> [!NOTE]
> Da e-Mail-Nachrichten über die Verbindung im nur-Text übertragen werden entstehen Sicherheitsrisiken bei der Kennwort eines Benutzers per e-Mail senden.


Das PasswordRecovery-Steuerelement besteht aus drei Ansichten:

- **Benutzername** -fordert den Besucher für ihren Benutzernamen. Dies ist die anfängliche Ansicht.
- **Frage**-zeigt die Frage des Benutzers Benutzernamen und Sicherheit als Text, zusammen mit einem Textfeld für den Benutzer zur Eingabe der Antwort auf seine Sicherheitsfrage.
- **Erfolg**-zeigt eine Meldung, die des Benutzers, der sein Kennwort per e-Mail gesendet wurde.

Die Ansichten angezeigt, und das Steuerelement PasswordRecovery ausgeführten Aktionen hängen die folgenden Konfigurationseinstellungen für die Mitgliedschaft:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Die Mitgliedschaft Framework `RequiresQuestionAndAnswer` Einstellung gibt an, ob Benutzer eine Sicherheitsfrage und eine Antwort angeben müssen, wenn Sie für ein Konto zu registrieren. Wie in erläutert die <a id="_msoanchor_1"> </a> [ *Erstellen von Benutzerkonten* ](../membership/creating-user-accounts-cs.md) Lernprogramm If `RequiresQuestionAndAnswer` "true" (Standard) ist, und klicken Sie dann die CreateUserWizard-Schnittstelle Textfeld enthält Steuerelemente für des neuen Benutzers Sicherheitsfrage und Antwort Wenn `RequiresQuestionAndAnswer` "false" ist keine derartigen Informationen gesammelt. Auf ähnliche Weise, wenn `RequiresQuestionAndAnswer` ist "true", und klicken Sie dann auf die PasswordRecovery-Steuerelement zeigt die Frage anzeigen, nachdem der Benutzer ihren Benutzernamen eingegeben hat, das Kennwort wird wiederhergestellt, nur dann, wenn der Benutzer die richtigen Sicherheitsantwort eingibt. Wenn `RequiresQuestionAndAnswer` allerdings wird "false", das PasswordRecovery-Steuerelement verschiebt direkt von der UserName-Ansicht in der Ansicht erfolgreich.

Nachdem der Benutzer seinen Benutzernamen- oder seinen Benutzernamen und Sicherheit-Antwort, wenn bereitgestellt hat `RequiresQuestionAndAnswer` ist "true" - die PasswordRecovery dem Benutzer sein Kennwort per e-Mail. Wenn die `EnablePasswordRetrieval` Option auf "true" festgelegt ist, wird der Benutzer ist ihr aktuelle Kennwort per e-Mail gesendet. Wenn sie auf "false" festgelegt ist und `EnablePasswordReset` auf "true" festgelegt wird, dann wird die PasswordRecovery Control generiert ein neues, zufälliges Kennwort für den Benutzer und das neue Kennwort Ihnen per e-Mail. Wenn beide `EnablePasswordRetrieval` und `EnablePasswordReset` "false" werden, das Steuerelement PasswordRecovery löst eine Ausnahme aus.

> [!NOTE]
> Bedenken Sie, dass die `SqlMembershipProvider` speichert Kennwörter von Benutzern in einem der drei Formate: Clear, Hashed (Standard) oder verschlüsselt. Speichermechanismus verwendet hängt von der Mitgliedschaft-Konfigurationseinstellungen ab. die demoanwendung verwendet die Hashed-Kennwort-Format. Wenn die Hashed-Kennwort-Format mit der `EnablePasswordRetrieval` Option muss auf "false" festgelegt werden, da das System tatsächlich das Kennwort des Benutzers aus der Hashwert berechnet wurde, in der Datenbank gespeicherten Version ermittelt werden kann.


Abbildung 1 zeigt an, wie die PasswordRecovery Schnittstelle und das Verhalten durch die Konfiguration der Mitgliedschaft beeinflusst wird.


[![RequiresQuestionAndAnswer, EnablePasswordRetrieval und EnablePasswordReset beeinflussen Aussehen und Verhalten des PasswordRecovery-Steuerelements](recovering-and-changing-passwords-cs/_static/image2.png)](recovering-and-changing-passwords-cs/_static/image1.png)

**Abbildung 1**: die `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, und `EnablePasswordReset` Aussehen und Verhalten des PasswordRecovery-Steuerelements beeinflussen ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image3.png))


> [!NOTE]
> In der <a id="_msoanchor_2"> </a> [ *erstellen das Schema für die Mitgliedschaft in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-cs.md) Lernprogramm wir den Mitgliedschaftsanbieter konfiguriert, indem `RequiresQuestionAndAnswer` auf "true", `EnablePasswordRetrieval` an "False" und `EnablePasswordReset` auf "true".


### <a name="using-the-passwordrecovery-control"></a>Verwenden des PasswordRecovery-Steuerelements

Betrachten Sie mithilfe des PasswordRecovery-Steuerelements auf einer ASP.NET-Seite. Open `RecoverPassword.aspx` und ziehen Sie und drop eine PasswordRecovery-Steuerelement aus der Toolbox in den Designer; legen Sie seine `ID` auf `RecoverPwd`. Wie die Websteuerelemente Anmeldung und CreateUserWizard Rendern des PasswordRecovery-Steuerelements Ansichten eine umfangreiche zusammengesetzte Oberfläche, die Bezeichnungen, Textfelder, Schaltflächen und Validierungssteuerelemente enthält. Sie können die Darstellung der Ansichten über die Stileigenschaften des Steuerelements oder konvertieren die Ansichten in Vorlagen anpassen. Ich lassen Sie dieses als Übung für die gewünschten Reader.

Wenn ein Benutzer auf dieser Seite besucht wird sie geben Sie ihren Benutzernamen, und klicken Sie auf die Schaltfläche "Absenden". Da wir festgelegt haben die `RequiresQuestionAndAnswer` Eigenschaft auf "true" in unseren Mitgliedschaft-Konfigurationseinstellungen, die PasswordRecovery gesteuert wird, dann zeigen Sie die Frage an. Nachdem der Benutzer ihre richtigen Sicherheitsantwort eingibt und Absenden klickt, wird das Steuerelement PasswordRecovery aktualisieren das Kennwort des Benutzers mit einer zufällig generierten und e-Mail-dieses Kennwort an die e-Mail-Adresse auf die Datei. All dies war es möglich ohne uns eine einzige Codezeile schreiben müssen.

Bevor Sie diese Seite testen, besteht ein abschließendes Teil der Konfiguration an, in der Regel: Wir benötigen, geben Sie die e-Mail-übermittlungseinstellungen in `Web.config`. PasswordRecovery-Steuerelement basiert auf diese Einstellungen für die e-Mail-Adresse senden.

Die Konfiguration der e-Mail-Übermittlung wird angegeben, über die [ `<system.net>` Element](https://msdn.microsoft.com/library/6484zdc1.aspx)des [ `<mailSettings>` Element](https://msdn.microsoft.com/library/w355a94k.aspx). Verwenden der [ `<smtp>` Element](https://msdn.microsoft.com/library/ms164240.aspx) die Übermittlungsmethode und der standardmäßige Absenderadresse an. Das folgende Markup konfiguriert e-Mail-Einstellungen zum Verwenden eines Netzwerk-SMTP-Servers mit dem Namen `smtp.example.com` an Port 25 und mit Benutzername/Kennwort-Anmeldeinformationen des Benutzernamens und Kennworts.

> [!NOTE]
> `<system.net>`ist ein untergeordnetes Element des Stamms `<configuration>` Element und ein gleichgeordnetes Element eines `<system.web>`. Fügen Sie daher nicht die `<system.net>` Element innerhalb der `<system.web>` -Element; stattdessen fügen Sie sie auf der gleichen Ebene.


[!code-xml[Main](recovering-and-changing-passwords-cs/samples/sample1.xml)]

Zusätzlich zur Verwendung eines SMTP-Servers im Netzwerk, können Sie auch ein pickup-Verzeichnis angeben, in dem e-Mail-Nachrichten gesendet werden Änderungstabellen abgelegt werden soll.

Nachdem Sie die SMTP-Einstellungen konfiguriert haben, besuchen Sie die `RecoverPassword.aspx` Seite über einen Browser. Versuchen Sie zunächst, einen Benutzernamen, der nicht in den Speicher des Benutzers vorhanden eingeben. Wie in Abbildung 2 gezeigt, zeigt das Steuerelement PasswordRecovery eine Meldung, dass die Benutzerinformationen konnte nicht zugegriffen werden. Der Text der Nachricht über des Steuerelements angepasst werden [ `UserNameFailureText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Eine Fehlermeldung angezeigt wird, wenn Sie einen ungültigen Benutzernamen eingeben](recovering-and-changing-passwords-cs/_static/image5.png)](recovering-and-changing-passwords-cs/_static/image4.png)

**Abbildung 2**: eine Fehlermeldung wird angezeigt, wenn Sie einen ungültigen Benutzernamen eingegeben wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image6.png))


Jetzt geben Sie einen Benutzernamen ein. Verwenden Sie der Benutzernamen eines Kontos in das System mit der e-Mail-Adresse, wenn Sie können auf zugreifen, deren Sicherheit beantworten Sie kennen. Nach dem Eingeben des Benutzernamens und Benutzer auf Absenden klickt, zeigt das PasswordRecovery-Steuerelement seinen Ansichtszustand Frage. Als UserName-Ansicht bei der Eingabe eines falschen beantwortet die PasswordRecovery-Steuerelement zeigt eine Fehlermeldung angezeigt (siehe Abbildung 3). Verwenden der [ `QuestionFailureText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) diese Fehlermeldung anpassen.


[![Eine Fehlermeldung wird angezeigt, wenn der Benutzer eine ungültige Sicherheitsantwort eingibt.](recovering-and-changing-passwords-cs/_static/image8.png)](recovering-and-changing-passwords-cs/_static/image7.png)

**Abbildung 3**: eine Fehlermeldung wird angezeigt, wenn der Benutzer eingibt, eine ungültige Sicherheitsantwort ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image9.png))


Schließlich geben Sie die richtige Sicherheitsantwort aus, und klicken Sie auf Absenden. Hinter den Kulissen PasswordRecovery-Steuerelement wird ein zufälliges Kennwort generiert, mit dem Benutzerkonto zugewiesen, sendet eine e-Mail zu informieren des Benutzers ihr neues Kennwort (siehe Abbildung 4), und klicken Sie dann zeigt die Erfolg an.


[![Der Benutzer wird eine E-Mail mit seiner neuen Kennwort gesendet werden.](recovering-and-changing-passwords-cs/_static/image11.png)](recovering-and-changing-passwords-cs/_static/image10.png)

**Abbildung 4**: der Benutzer wird eine E-Mail mit seiner neuen Kennwort gesendet ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image12.png))


### <a name="customizing-the-email"></a>Anpassen von die e-Mail-Adresse

Standard-e-Mail gesendet, die vom Steuerelement PasswordRecovery ist vielmehr stumpf (siehe Abbildung 4). Die Nachricht wird gesendet, von dem im angegebenen Konto die `<smtp>` des Elements `from` Attribut mit dem Betreff Kennwort und den nur-Text-Text:

Zurück zur Website, und melden Sie sich mit den folgenden Informationen.

Benutzername: *Benutzername*

Kennwort: *Kennwort*

Diese Meldung kann programmgesteuert über einen Ereignishandler für des PasswordRecovery Steuerelements angepasst werden [ `SendingMail` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), oder deklarativ über die [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Betrachten Sie diese beiden Optionen aus.

Die `SendingMail` Ereignis wird ausgelöst, unmittelbar bevor die e-Mail-Nachricht wird gesendet, und unsere letzte Möglichkeit ist, die e-Mail-Nachricht programmgesteuert anzupassen. Wenn dieses Ereignis ausgelöst wird, wird der Ereignishandler ein Objekt des Typs übergeben [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), dessen `Message` Eigenschaft enthält einen Verweis auf die e-Mail gesendet werden soll.

Erstellen Sie einen Ereignishandler für das `SendingMail` Ereignis und fügen Sie folgenden Code, das programmgesteuert hinzufügt `webmaster@example.com` der CC-Liste.

[!code-csharp[Main](recovering-and-changing-passwords-cs/samples/sample2.cs)]

Die e-Mail-Nachricht kann auch deklarative Weise konfiguriert werden. Die PasswordRecovery `MailDefinition` Eigenschaft ist ein Objekt des Typs [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Die `MailDefinition` Klasse bietet eine Vielzahl von e-Mail-bezogenen Eigenschaften, einschließlich `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`, und andere. Legen Sie zunächst die [ `Subject` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) in einen aussagekräftigeren als an denjenigen in der Standardeinstellung (Kennwort) verwendet werden, z. B. Ihr Kennwort zurückgesetzt wurde...

Um den Text der e-Mail-Nachricht anzupassen, wir erstellen eine separate e-Mail-Vorlagendatei müssen, enthält, die Text-Inhalt. Starten, indem Sie einen neuen Ordner erstellen, auf der Website mit dem Namen `EmailTemplates`. Fügen Sie eine neue Textdatei in diesem Ordner mit dem Namen `PasswordRecovery.txt` und fügen Sie den folgenden Inhalt hinzu:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample3.aspx)]

Beachten Sie die Verwendung der Platzhalter `<%UserName%>` und `<%Password%>`. Das PasswordRecovery-Steuerelement ersetzt diese zwei Platzhalter automatisch mit Benutzernamen und die wiederhergestellten Kennwort vor dem Senden der e-Mail des Benutzers.

Zeigen Sie schließlich die `MailDefinition`des [ `BodyFileName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) der e-Mail-Vorlage, die soeben erstellt wurde (`~/EmailTemplates/PasswordRecovery.txt`).

Nachdem diese Teit Änderungen auf die `RecoverPassword.aspx` Seite, und geben Sie Ihre Antwort Benutzernamen und Sicherheit. Sie erhalten eine e-Mail, die in Abbildung 5 aussehen sollte. Beachten Sie, dass `webmaster@example.com` wurde CC würde und dass die Betreffzeile und dem Textkörper aktualisiert wurden.


[![Der Betreff, Text und CC-Liste wurden aktualisiert](recovering-and-changing-passwords-cs/_static/image14.png)](recovering-and-changing-passwords-cs/_static/image13.png)

**Abbildung 5**: der Betreff, Text und CC Liste aktualisiert wurden ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image15.png))


Legen Sie eine HTML-formatierte e-Mail-Nachricht senden [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) auf "true" (der Standardwert ist "false") und aktualisieren Sie die e-Mail-Vorlage HTML einzuschließen.

Die `MailDefinition` Eigenschaft ist nicht eindeutig, auf die PasswordRecovery-Klasse. Da wir in Schritt2 angezeigt wird, das Steuerelement ChangePassword verfügt zudem über eine `MailDefinition` Eigenschaft. Darüber hinaus enthält das Steuerelement CreateUserWizard eine solche Eigenschaft konfiguriert werden können, um automatisch eine Willkommen-e-Mail-Nachricht an neue Benutzer senden.

> [!NOTE]
> Derzeit keine Links im linken Navigationsbereich erreichen, sind die `RecoverPassword.aspx` Seite. Benutzer wären nur interessiert, werden auf dieser Seite besuchen, wenn er nicht erfolgreich melden Sie sich bei der Website konnte. Aktualisieren Sie deshalb die `Login.aspx` Seite, um einen Link zu der `RecoverPassword.aspx` Seite.


### <a name="programmatically-resetting-a-users-password"></a>Programmgesteuertes Zurücksetzen eines Benutzerkennworts

Beim Zurücksetzen eines Benutzerkennworts das PasswordRecovery Aufrufe steuern die `MembershipUser` des Objekts [ `ResetPassword` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Diese Methode verfügt über zwei Überladungen:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)**-das Kennwort eines Benutzers zurückgesetzt. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` ist "false".
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)**-setzt nur ein Benutzer das Kennwort, wenn das angegebene *SecurityAnswer* richtig ist. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` ist "true".

Beide Überladungen geben das neue, zufällig generiertes Kennwort zurück.

Mit den anderen Methoden in der Mitgliedschaft-Framework, z. B. die `ResetPassword` -Methode delegiert an den konfigurierten Anbieter. Die `SqlMembershipProvider` Ruft die `aspnet_Membership_ResetPassword` gespeicherte Prozedur übergeben, in den Benutzernamen des Benutzers, das neue Kennwort und die angegebene Kennwortantwort zwischen anderen Feldern. Die gespeicherte Prozedur wird sichergestellt, dass die Kennwortantwort entspricht, und klicken Sie dann das Kennwort des Benutzers aktualisiert.

Ein paar Hinweise zur Implementierung auf niedriger Ebene:

- Ein gesperrter Benutzer kann nicht das Kennwort zurückgesetzt. Allerdings kann ein nicht genehmigter Benutzer. Wir besprechen die gesperrt und genehmigt Zustände ausführlicher in den <a id="_msoanchor_3"> </a> [ *entsperren und Genehmigen von Benutzer* ](unlocking-and-approving-user-accounts-cs.md) Konten Lernprogramm.
- Wenn die Antwort für das Kennwort falsch ist, wird der Benutzer Kennworts beantworten Anzahl inkrementiert. Wenn eine angegebene Anzahl von versuchen, ungültige Sicherheit Antwort innerhalb eines angegebenen Zeitfensters auftreten, wird der Benutzer gesperrt.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Ein Wort auf wie die zufällig erzeugten Kennwörtern werden generiert.

Die zufällig generierte Kennwörter in der e-Mail-Nachrichten in Abbildung 4 und 5 gezeigten werden erstellt, indem die Mitgliedschaftsklasse [ `GeneratePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Diese Methode akzeptiert zwei ganze Zahl Parameter input - *Länge* und *NumberOfNonAlphanumericCharacters* - und mindestens eine Zeichenfolge zurückgibt *Länge* Zeichen lange mit an mindestens *NumberOfNonAlphanumericCharacters* Anzahl nicht alphanumerischer Zeichen. Beim Aufrufen dieser Methode von innerhalb der Mitgliedschaftsklassen oder Login-bezogene Websteuerelemente werden die Werte für diese beiden Parameter bestimmt, von der Mitgliedschaft Konfigurations `MinRequiredPasswordLength` und `MinRequiredNonalphanumericCharacters` Eigenschaften, die wir auf 7 bzw. auf 1 festgelegt.

Die `GeneratePassword` Methode verwendet einen kryptografisch sicheren Zufallszahlen-Generator, um sicherzustellen, dass es keine Verschiebung in welche zufälligen Zeichen ausgewählt sind. Darüber hinaus `GeneratePassword` ist `public`, was bedeutet, dass er direkt aus Ihrer ASP.NET-Anwendung bei verwendet werden können, müssen Sie zufällige Zeichenfolgen oder Kennwörter zu generieren.

> [!NOTE]
> Die `SqlMembershipProvider` Klasse immer ein zufälliges Kennwort generiert mindestens 14 Zeichen lang sein, wenn also `MinRequiredPasswordLength` beträgt weniger als 14 und sein Wert wird ignoriert.


## <a name="step-2-changing-passwords"></a>Schritt 2: Ändern von Kennwörtern

Die Kennwörter nach dem Zufallsprinzip generierte sind schwer zu merken. Betrachten Sie das Kennwort, das in Abbildung 4 dargestellt: `WWGUZv(f2yM:Bd`. Probieren Sie bestätigen, die in den Arbeitsspeicher. Nachdem ein Benutzer ein zufällig generiertes Kennwort dieser Art gesendet wird, sollten sie selbstverständlich in einen einprägsameren das Kennwort zu ändern.

Verwenden Sie das ChangePassword-Steuerelement, um eine Schnittstelle für einen Benutzer zum Ändern des Kennworts zu erstellen. Ähnlich wie das Steuerelement PasswordRecovery ChangePassword-Steuerelement besteht aus zwei Ansichten: Kennwort ändern und erfolgreich. Die View Change Password fordert den Benutzer für ihre alten und neuen Kennwörter. Bei stellen Sie dabei das alte Kennwort und ein neues Kennwort ein, das der Mindestlänge und nicht-alphanumerisches Zeichen Anforderungen erfüllt, das Steuerelement ChangePassword aktualisiert das Kennwort des Benutzers und zeigt die Erfolg an.

> [!NOTE]
> Das Steuerelement ChangePassword ändert das Kennwort des Benutzers durch Aufrufen der `MembershipUser` des Objekts [ `ChangePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Die ChangePassword-Methode akzeptiert zwei `string` Eingabeparameter - *OldPassword* und *NewPassword*- und aktualisiert das Konto des Benutzers mit der *NewPassword*, sofern die angegebenen *OldPassword* richtig ist.


Öffnen der `ChangePassword.aspx` Seite, und fügen Sie ein Steuerelement ChangePassword hinzu, benennen es `ChangePwd`. Die Entwurfsansicht sollte das Kennwort ändern an diesem Punkt anzeigen (siehe Abbildung 6) anzeigen. Wie können mit dem Steuerelement PasswordRecovery Sie zwischen den Ansichten über des Steuerelements Smarttag umschalten. Darüber hinaus sind diese Sichten eindeutigkeitsmetrik anpassbare über zusammengestellte Stileigenschaften oder indem Sie sie in eine Vorlage konvertieren.


[![Hinzufügen eines ChangePassword-Steuerelements auf der Seite "](recovering-and-changing-passwords-cs/_static/image17.png)](recovering-and-changing-passwords-cs/_static/image16.png)

**Abbildung 6**: Hinzufügen eines ChangePassword-Steuerelements auf der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image18.png))


Das Steuerelement ChangePassword kann das Kennwort des aktuell angemeldeten Benutzers aktualisieren *oder* das Kennwort des Benutzers von einem anderen, angegebenen. Wie in Abbildung 6 gezeigt, rendert die Standardansicht für die Change Password nur drei TextBox-Steuerelemente: eines für das alte Kennwort und zwei für das neue Kennwort. Diese Standardschnittstelle wird verwendet, um das Kennwort des angemeldeten Benutzers zu aktualisieren.

Festlegen, um das Steuerelement ChangePassword beim Aktualisieren des Kennworts eines anderen Benutzers zu verwenden, welches Steuerelement [ `DisplayUserName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) auf "true". Auf diese Weise fügt eine vierte Textfeld auf der Seite Bestätigung für den Benutzernamen des Benutzers, dessen Kennwort zu ändern.

Festlegen von `DisplayUserName` auf "true" ist hilfreich, wenn Sie out angemeldeten Benutzer das Kennwort zu ändern, melden Sie sich ohne können möchten. Persönlich vermuten ich nichts falsch mit einen Benutzer die Anmeldung erfordern, bevor Sie die Möglichkeit zum Ändern des Kennworts. Aus diesem Grund verlassen `DisplayUserName` auf "false" (Standardwert) festgelegt. Diese Entscheidung sind wir jedoch im Wesentlichen erreichen dieser Seite für anonyme Benutzer wobei. Aktualisieren Sie die Website-URL-Autorisierungsregeln um anonyme Benutzer besuchen Verweigern `ChangePassword.aspx`. Wenn Sie Ihren Speicher, auf die URL-Autorisierung Regelsyntax aktualisieren müssen, finden Sie zurück in die <a id="_msoanchor_4"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-cs.md) Lernprogramm.

> [!NOTE]
> Es mag, die die `DisplayUserName` Eigenschaft ist nützlich für das Zulassen von Administratoren anderer Benutzer Kennwörter ändern. Allerdings ist es auch bei `DisplayUserName` auf "true", das alte Kennwort muss bekannt sein und eingegeben, festgelegt ist. Wir beschäftigen Techniken für das Zulassen von Administratoren zum Ändern von Benutzerkennwörtern in Schritt 3.


Besuchen Sie die `ChangePassword.aspx` Seite über einen Browser, und Ihr Kennwort ändern. Beachten Sie, dass eine Fehlermeldung angezeigt wird, wenn Sie ein neues Kennwort eingeben, die nicht die Kennwortlänge und nicht-alphanumerisches Zeichen in der Konfiguration der Mitgliedschaft angegeben erfüllen (siehe Abbildung 7).


[![Hinzufügen eines ChangePassword-Steuerelements auf der Seite "](recovering-and-changing-passwords-cs/_static/image20.png)](recovering-and-changing-passwords-cs/_static/image19.png)

**Abbildung 7**: Hinzufügen eines ChangePassword-Steuerelements auf der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image21.png))


Beim Eintritt in das alte Kennwort und ein gültiges neue Kennwort, des angemeldeten Benutzers auf Kennwort geändert wird, und die Ansicht "Erfolg" angezeigt.

### <a name="sending-a-confirmation-email"></a>Senden eine E-Mail zur Kaufbestätigung

Standardmäßig sendet das Steuerelement ChangePassword keine e-Mail-Nachricht an den Benutzer, dessen Kennwort gerade aktualisiert wurde. Wenn Sie eine e-Mail senden möchten, konfigurieren Sie einfach des Steuerelements `MailDefinition` Eigenschaft. Wir das ChangePassword Steuerelement so konfigurieren, dass der Benutzer eine HTML-formatierte e-Mail-Nachricht gesendet wird, die ihr neue Kennwort enthält.

Starten Sie durch Erstellen einer neuen Datei in die `EmailTemplates` Ordner mit dem Namen `ChangePassword.htm`. Fügen Sie das folgende Markup hinzu:

[!code-html[Main](recovering-and-changing-passwords-cs/samples/sample4.html)]

Legen Sie anschließend des ChangePassword-Steuerelements `MailDefinition` der Eigenschaft `BodyFileName`, `IsBodyHtml`, und `Subject` Eigenschaften ~ / EmailTemplates/ChangePassword.htm "true" und Ihr Kennwort wurde geändert! zugeordnet.

Rufen Sie nachdem diese Änderungen vorgenommen wurden die Seite erneut, und ändern Sie Ihr Kennwort erneut. Dieses Mal sendet ChangePassword-Steuerelement eine benutzerdefinierte HTML-formatierte e-Mail-an-e-Mail-Adresse des Benutzers auf die Datei (siehe Abbildung 8).


[![Eine e-Mail-Nachricht informiert die Benutzer, deren Kennwort wurde geändert](recovering-and-changing-passwords-cs/_static/image23.png)](recovering-and-changing-passwords-cs/_static/image22.png)

**Abbildung 8**: eine e-Mail-Nachricht informiert die Benutzer, deren Kennwort wurde geändert ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Schritt 3: Administratoren das Ändern von Benutzerkennwörtern erlaubt.

Ein gängiges Feature in Anwendungen zur Unterstützung von Benutzerkonten ist die Möglichkeit für einen Administrator, andere Benutzer Kennwörter zu ändern. Diese Funktionalität ist in einigen Fällen erforderlich, da das System verfügt nicht über die die Möglichkeit für Benutzer ihre eigenen Kennwörter ändern. In diesem Fall wäre die einzige Möglichkeit für einen Benutzer her, deren Kennwort vergessen haben für den Administrator, ihnen ein neues Kennwort zuzuweisen. Mit den Steuerelementen PasswordRecovery und ChangePassword müssen jedoch Administratoren nicht selbst Gebucht-Informationen mit der Änderung von Benutzerkennwörtern, wie Benutzer auf diese Weise selbst sind.

Aber was geschieht, wenn Ihr Client besteht, dass Administratoren anderer Benutzer Kennwörter ändern, werden sollten? Leider kann das Hinzufügen dieser Funktionalität viel Arbeit sein. Um das Kennwort eines Benutzers zu ändern, die alte und neue Kennwort muss bereitgestellt werden die `MembershipUser` des Objekts `ChangePassword` -Methode, aber ein Administrator sollte sich nicht mit das Kennwort eines Benutzers zu kennen, um es zu ändern.

Eine Lösung besteht darin, zuerst das Kennwort des Benutzers zurückgesetzt, und klicken Sie dann auf das neue Kennwort mit Code wie folgt ändern:

[!code-aspx[Main](recovering-and-changing-passwords-cs/samples/sample5.aspx)]

Dieser Code beginnt mit dem Abrufen von Informationen zu *Benutzername*, dies ist der Benutzer, dessen Kennwort möchte, dass der Administrator zu ändern. Als Nächstes wird die `ResetPassword` Methode aufgerufen wird, welche zugewiesen und der Benutzer ein neues, zufälliges Kennwort. Diese willkürlich generiertem Kennwort von der Methode zurückgegeben und in der Variablen gespeicherte `resetPwd`. Nun, dass wir das Kennwort des Benutzers kennen, können wir Sie ändern, über einen Aufruf an `ChangePassword`.

Das Problem besteht darin, dass dieser Code funktioniert nur, wenn die Mitgliedschaft Systemkonfiguration festgelegt ist, dass `RequiresQuestionAndAnswer` ist "false". Wenn `RequiresQuestionAndAnswer` ist "true", da sie mit der Anwendung die `ResetPassword` Methode die Sicherheitsantwort übergeben werden muss, andernfalls wird ein Ausnahmefehler ausgelöst wird.

Wenn das Framework für die Mitgliedschaft so konfiguriert wird, dass Sie eine Sicherheitsfrage und eine Antwort erforderlich ist und der Client noch besteht, dass Administratoren Benutzer Kennwörter ändern können, haben Sie drei Möglichkeiten:

- Löst die Hände in der Luft aus, und teilen Sie Ihrem Client, dass dies nur eine Bedeutung ist, die nicht verwendet werden können.
- Legen Sie `RequiresQuestionAndAnswer` auf "false". Dies führt zu einer weniger sicheren Anwendung. Stellen Sie sich vor, dass es sich bei ein böswilligen Benutzer den Zugriff auf einen anderen Benutzer e-Mail-Posteingang erlangt hat. Vielleicht gefährdete Benutzer ihren Helpdesk, um Essen aufzurufen getrennt und seiner Arbeitsstation nicht gesperrt oder vielleicht sie Zugriff auf ihre e-Mails über ein öffentliches Terminal und melden Sie sich nicht ab. In beiden Fällen kann der böswilligen Benutzer besuchen Sie die `RecoverPassword.aspx` Seite, und geben Sie den Benutzernamen des Benutzers. Das System wird dann der wiederhergestellte Kennwort per e-Mail ohne für die Sicherheitsantwort.
- Umgehen der Abstraktionsschicht, die vom Framework Mitgliedschaft und arbeiten direkt mit der SQL Server-Datenbank erstellt wird. Das Schema der Mitgliedschaft enthält eine gespeicherte Prozedur namens `aspnet_Membership_SetPassword` , legt das Kennwort eines Benutzers und erfordert den Sicherheitsantwort oder das alte Kennwort keine zur Ausführung der Aufgabe.

Keine dieser Optionen sind besonders attraktiv, aber das ist wie die Lebensdauer eines Entwicklers in einigen Fällen wechselt.

Ich ein aufgetretenes Problem fort und implementiert der dritte Ansatz darin, Code schreiben, der umgeht die `Membership` und `MembershipUser` Klassen und direkt für den Zugriff der `SecurityTutorials` Datenbank.

> [!NOTE]
> Arbeiten Sie direkt mit der Datenbank, wird der von der Mitgliedschaft Framework gekapselt aufgehoben. Diese Entscheidung bindet uns die `SqlMembershipProvider`, getesteten Codes weniger Portable vornehmen. Darüber hinaus kann dieser Code funktioniert nicht wie in zukünftigen Versionen von ASP.NET erwartet, wenn das Schema für die Mitgliedschaft ändert. Dieser Ansatz ist dieses Problem zu umgehen, und wie die meisten problemumgehungen ist ein Beispiel für bewährte Methoden.


Der Code hat einige uninteressant Bits und abgebildet wird. Ich möchte aus diesem Grund nicht in diesem Lernprogramm mit ausführlichen Untersuchung von es zu überlasten. Wenn Sie mehr erfahren möchten, laden Sie den Code für dieses Lernprogramm und besuchen Sie die `~/Administration/ManageUsers.aspx` Seite. Diese Seite, die wir in erstellt die <a id="_msoanchor_5"> </a> [vorherigen Lernprogramm](building-an-interface-to-select-one-user-account-from-many-cs.md), jeder Benutzer führt. Ich habe haben die GridView einen Link auf Einbeziehung aktualisiert die `UserInformation.aspx` Seite, die den ausgewählten Benutzernamen des Benutzers über die Abfragezeichenfolge übergeben. Die `UserInformation.aspx` Seite zeigt Informationen zu den ausgewählten Benutzer und die Textfelder für ihr Kennwort ändern (siehe Abbildung 9).

Nachdem das neue Kennwort eingeben, bestätigen es in das zweite Textfeld ein und klicken Sie auf die Schaltfläche "Benutzer aktualisieren", was zu ein Postback erfolgt und die `aspnet_Membership_SetPassword` gespeicherte Prozedur aufgerufen wurde, aktualisieren das Kennwort des Benutzers. Diese Funktionalität interessiert Leser mehr vertraut, durch den Code, und versuchen Sie es Erweitern der Funktionalität um einzuschließen, senden eine e-Mail an den Benutzer, dessen Kennwort geändert wurde, wird empfohlen.


[![Ein Administrator kann das Kennwort eines Benutzers ändern.](recovering-and-changing-passwords-cs/_static/image26.png)](recovering-and-changing-passwords-cs/_static/image25.png)

**Abbildung 9**: ein Administrator kann das Kennwort eines Benutzers ändern ([klicken Sie hier, um das Bild in voller Größe angezeigt](recovering-and-changing-passwords-cs/_static/image27.png))


> [!NOTE]
> Die `UserInformation.aspx` Seite derzeit funktioniert nur, wenn das Framework Mitgliedschaft konfiguriert ist, um Kennwörter in Klartext oder Hashed-Format zu speichern. Er verfügt nicht über den Code, um das neue Kennwort verschlüsseln aus, auch wenn Sie gefragt werden, um diese Funktionalität hinzuzufügen. Die Möglichkeit, die ich wird empfohlen, den erforderlichen Code hinzufügen ist die Verwendung von dekompilieren wie [Reflector](http://www.aisto.com/roeder/dotnet/) untersuchen Sie den Quellcode für Methoden in .NET Framework untersucht, die `SqlMembershipProvider` Klasse `ChangePassword` Methode. Dies ist das Verfahren, das ich zum Schreiben von Code für ein Hashwert des Kennworts verwendet.


## <a name="summary"></a>Zusammenfassung

ASP.NET bietet zwei Steuerelemente können Benutzer ihre Kennwörter zu verwalten. Das Steuerelement PasswordRecovery eignet sich für die Benutzer ihr Kennwort vergessen haben. Abhängig von der Mitgliedschaft Framework-Konfiguration wird der Benutzer entweder ihr vorhandenes Kennwort oder ein neues, zufällig generiertes Kennwort per e-Mail gesendet. Das Steuerelement ChangePassword ermöglicht einem Benutzer zum Aktualisieren seines Kennworts aufgefordert.

Wie die Anmeldung und CreateUserWizard-Steuerelemente rendert die PasswordRecovery und ChangePassword-Steuerelemente eine umfangreiche Benutzeroberfläche ohne jeglichen deklarativem Markup oder Codezeile schreiben zu müssen. Wenn die Standardbenutzeroberfläche Ihren Anforderungen nicht erfüllt, können Sie ihn über eine Reihe von Stileigenschaften anpassen. Alternativ können die Steuerelemente Schnittstellen in Vorlagen für eine noch genauer Maß an Kontrolle konvertiert werden. Im Hintergrund verwenden Sie diese Steuerelemente Membership-API Aufrufen der `MembershipUser` des Objekts `ResetPassword` und `ChangePassword` Methoden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [ChangePassword-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [Schnellstarts PasswordRecovery-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Senden von E-Mail in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail`Häufig gestellte Fragen](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm enthalten Michael Emmings und Suchi Banerjee. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Zurück](building-an-interface-to-select-one-user-account-from-many-cs.md)
[Weiter](unlocking-and-approving-user-accounts-cs.md)
