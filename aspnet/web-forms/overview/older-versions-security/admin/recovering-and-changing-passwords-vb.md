---
uid: web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
title: Wiederherstellen und Ändern von Kennwörtern (VB) | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET umfasst zwei Web-Steuerelemente für die Unterstützung bei der Wiederherstellung, und Ändern von Kennwörtern. Das Steuerelement PasswordRecovery ermöglicht einen Besucher seinen Pa verlorene wiederherstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2008
ms.topic: article
ms.assetid: f9adcb5d-6d70-4885-a3bf-ed95efb4da1a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/admin/recovering-and-changing-passwords-vb
msc.type: authoredcontent
ms.openlocfilehash: 68b70708904a46e31639331bbfd0dd31240ea421
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393995"
---
<a name="recovering-and-changing-passwords-vb"></a>Wiederherstellen und Ändern von Kennwörtern (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/VB.13.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/0/e/60e1bd94-e5f9-4d5a-a079-f23c98f4f67d/aspnet_tutorial13_ChangingPasswords_vb.pdf)

> ASP.NET umfasst zwei Web-Steuerelemente für die Unterstützung bei der Wiederherstellung, und Ändern von Kennwörtern. Das Steuerelement PasswordRecovery ermöglicht einen Besucher seinen verlorenen Kennworts wiederhergestellt. ChangePassword-Steuerelement ermöglicht den Benutzer sein Kennwort zu aktualisieren. Wie die anderen Web anmeldebezogene Steuerelemente haben wir in dieser Reihe von Tutorials, die PasswordRecovery gesehen und ChangePassword steuert die Arbeit mit das mitgliedschaftsframework hinter den Kulissen zurücksetzen oder Ändern von Benutzerkennwörtern.


## <a name="introduction"></a>Einführung

Zwischen den Websites für meiner Bank, Versorgungsunternehmen, Telefongesellschaft, e-Mail-Konten und personalisierte-Webportalen habe ich, wie die meisten Leute, Dutzenden von verschiedenen Kennwörter merken müssen. Mit so viele Anmeldeinformationen merken heutzutage ist es nicht ungewöhnlich, dass Benutzer ihre Kennwörter vergessen haben. Um dies zu berücksichtigen, müssen Websites, die Benutzerkonten bieten eine Möglichkeit, ein Benutzer sein Kennwort wiederherzustellen. Dieser Prozess umfasst in der Regel ein neues, zufälliges Kennwort generieren und zu senden, e-Mail-Adresse des Benutzers auf die Datei. Die meisten Benutzer zur Website zurückkehren, und ihr Kennwort ändern, von dem nach dem Zufallsprinzip generierte auf einen einprägsameren, nach dem Empfang von des neuen Kennworts.

ASP.NET umfasst zwei Web-Steuerelemente für die Unterstützung bei der Wiederherstellung, und Ändern von Kennwörtern. Das Steuerelement PasswordRecovery ermöglicht einen Besucher seinen verlorenen Kennworts wiederhergestellt. ChangePassword-Steuerelement ermöglicht den Benutzer sein Kennwort zu aktualisieren. Wie die anderen Web anmeldebezogene Steuerelemente haben wir in dieser Reihe von Tutorials, die PasswordRecovery gesehen und ChangePassword steuert die Arbeit mit das mitgliedschaftsframework hinter den Kulissen zurücksetzen oder Ändern von Benutzerkennwörtern.

In diesem Tutorial untersuchen wir, diese beiden Steuerelemente verwenden. Wir auch erfahren, wie Sie programmgesteuert ändern und Zurücksetzen des Kennworts eines Benutzers über die `MembershipUser` Klasse `ChangePassword` und `ResetPassword` Methoden.

## <a name="step-1-helping-users-recover-lost-passwords"></a>Schritt 1: Unterstützen Benutzer wiederherstellen, die Verlust von Kennwörtern

Alle Websites, die die Benutzerkonten unterstützen müssen, Benutzern einen Mechanismus für die Wiederherstellung von vergessener Kennwörter bereitzustellen. Die gute Nachricht ist, dass solche Funktionen implementieren, in ASP.NET ein Kinderspiel, Dank der PasswordRecovery-Webserver-Steuerelement. Das PasswordRecovery-Steuerelement rendert eine Schnittstelle, die die eingabeaufforderungen für ihren Benutzernamen und ggf. die Antwort auf die Sicherheitsfrage. Diese e-Mails klicken Sie dann des Benutzers sein Kennworts.

> [!NOTE]
> Da e-Mails im nur-Text übertragen werden sind Sicherheitsrisiken bei der Kennwort eines Benutzers per e-Mail senden.


Das Steuerelement PasswordRecovery besteht aus drei Ansichten:

- **Benutzername** -werden aufgefordert, den Besucher für ihren Benutzernamen. Dies ist die anfängliche Ansicht.
- **Frage**-Username und Security-Frage des Benutzers als Text, zusammen mit einem Textfeld für den Benutzer zur Eingabe der Antwort auf seine Sicherheitsfrage angezeigt.
- **Erfolg**-zeigt eine Meldung darüber informiert den Benutzer, der sein Kennwort per e-Mail gesendet wurde.

Die Ansichten angezeigt, und Aktionen, die vom Steuerelement PasswordRecovery hängen von den folgenden Einstellungen für die Mitgliedschaft-Konfiguration:

- `RequiresQuestionAndAnswer`
- `EnablePasswordRetrieval`
- `EnablePasswordReset`

Des mitgliedschaftsframework `RequiresQuestionAndAnswer` Einstellung gibt an, ob Benutzer eine Sicherheitsfrage und die Antwort angeben müssen, wenn Sie für ein Konto zu registrieren. Wie wir unter den <a id="_msoanchor_1"> </a> [ *Erstellen von Benutzerkonten* ](../membership/creating-user-accounts-vb.md) Tutorial If `RequiresQuestionAndAnswer` ist "true" (Standard), die CreateUserWizards Schnittstelle enthält die TextBox Steuerelemente für die Sicherheitsfrage und die Antwort des neuen Benutzers Wenn `RequiresQuestionAndAnswer` "False" ist keine Informationen dieser Art werden gesammelt. Auf ähnliche Weise, wenn `RequiresQuestionAndAnswer` ist "true", dann zeigt das PasswordRecovery-Steuerelement, das die Frage anzeigen, nachdem der Benutzer seinen Benutzernamen eingegeben hat; das Kennwort nur dann, wenn der Benutzer die richtige Sicherheits-Antwort gibt wiederhergestellt wird. Wenn `RequiresQuestionAndAnswer` false festgelegt ist, aber das Steuerelement PasswordRecovery verschiebt direkt von der UserName-Ansicht auf, in der Ansicht erfolgreich.

Nachdem der Benutzer seinen Benutzernamen- oder seine Username und Security-Antwort, wenn bereitgestellt hat `RequiresQuestionAndAnswer` ist "true" – die PasswordRecovery e-Mails dem Benutzer sein Kennwort. Wenn die `EnablePasswordRetrieval` Option auf "true" festgelegt ist, wird der Benutzer wird ihr aktuelle Kennwort per e-Mail gesendet. Wenn sie auf "false" festgelegt ist und `EnablePasswordReset` "true" festgelegt ist, und klicken Sie dann das PasswordRecovery-Steuerelement wird ein neues, zufälliges Kennwort für den Benutzer generiert, und das neue Kennwort mit der sie eine e-Mail. Wenn beide `EnablePasswordRetrieval` und `EnablePasswordReset` "false", das Steuerelement PasswordRecovery löst eine Ausnahme aus.

> [!NOTE]
> Bedenken Sie, dass die `SqlMembershipProvider` Kennwörter von Benutzern in folgenden drei Formaten gespeichert: löschen oder Hashed (Standard) verschlüsselt. Der Speichermechanismus verwendet, hängt die Mitgliedschaftskonfiguration ab. die demoanwendung verwendet das Hashed-Kennwort-Format. Wenn Sie die Hashed-Kennwort-Format verwenden die `EnablePasswordRetrieval` Option muss auf "false" festgelegt werden, da das System nicht, das Kennwort des Benutzers tatsächliche von der Hash-Version in der Datenbank gespeichert bestimmen kann.


Abbildung 1 zeigt, wie die PasswordRecovery Schnittstelle und das Verhalten durch die Konfiguration der Mitgliedschaft beeinflusst wird.


[![Die RequiresQuestionAndAnswer EnablePasswordRetrieval und EnablePasswordReset beeinflussen, Darstellung und das Verhalten des PasswordRecovery-Steuerelements](recovering-and-changing-passwords-vb/_static/image2.png)](recovering-and-changing-passwords-vb/_static/image1.png)

**Abbildung 1**: die `RequiresQuestionAndAnswer`, `EnablePasswordRetrieval`, und `EnablePasswordReset` beeinflussen, Darstellung und das Verhalten des Steuerelements PasswordRecovery ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image3.png))


> [!NOTE]
> In der <a id="_msoanchor_2"> </a> [ *Erstellen des Mitgliedschaftsschemas in SQL Server* ](../membership/creating-the-membership-schema-in-sql-server-vb.md) Tutorial wir den Mitgliedschaftsanbieter konfiguriert, durch Festlegen von `RequiresQuestionAndAnswer` auf "true", `EnablePasswordRetrieval` auf False gibt an, und `EnablePasswordReset` auf "true".


### <a name="using-the-passwordrecovery-control"></a>Verwenden des Steuerelements PasswordRecovery

Wir sehen uns die Verwendung des PasswordRecovery-Steuerelements in einer ASP.NET-Seite. Open `RecoverPassword.aspx` und ziehen Sie, und legen Sie kein PasswordRecovery-Steuerelement aus der Toolbox in den Designer; festlegen seiner `ID` zu `RecoverPwd`. Wie die Anmeldung und CreateUserWizard-Webserver-Steuerelemente Rendern des Steuerelements PasswordRecovery Ansichten eine funktionsreiche zusammengesetzte Benutzeroberfläche, die Bezeichnungen, Textfelder, Schaltflächen und Steuerelemente zur gültigkeitsprüfung enthält. Sie können die Darstellung der Sichten durch das die Eigenschaften des Steuerelements oder durch die Konvertierung von Ansichten in Vorlagen anpassen. Ich lassen Sie dieses als Übung für den interessierten Leser.

Wenn ein Benutzer diese Seite besucht werden sie geben Sie den Benutzernamen, und klicken Sie auf die Schaltfläche "Senden". Da wir festgelegt haben die `RequiresQuestionAndAnswer` Eigenschaft auf "true" in unserem Mitgliedschaft-Konfigurationseinstellungen, die PasswordRecovery steuern werden dann die Frageansicht angezeigt. Nachdem der Benutzer ihre richtigen Sicherheitsantwort wechselt und absendet, wird das Steuerelement PasswordRecovery aktualisieren das Kennwort des Benutzers zu einem zufällig generierten und e-Mail-dieses Kennwort, um die e-Mail-Adresse hinterlegt. All dies war möglich, ohne uns, dass eine einzige Codezeile schreiben müssen!

Bevor Sie diese Seite testen, ist es einem letzten Schritt der Konfiguration zu neigen dazu: Wir müssen die Einstellungen für die e-Mail-Übermittlung in angeben `Web.config`. Das PasswordRecovery-Steuerelement verwendet diese Einstellungen für das Senden von e-Mail.

Die e-Mail-Übermittlung-Konfiguration wird angegeben, über die [ `<system.net>` Element](https://msdn.microsoft.com/library/6484zdc1.aspx)des [ `<mailSettings>` Element](https://msdn.microsoft.com/library/w355a94k.aspx). Verwenden der [ `<smtp>` Element](https://msdn.microsoft.com/library/ms164240.aspx) die Übermittlungsmethode und die Absenderadresse an. Das folgende Markup konfiguriert e-Mail-Einstellungen, Verwendung ein Netzwerk-SMTP-Servers mit dem Namen `smtp.example.com` über Port 25 und mit Benutzername/Kennwort-Anmeldeinformationen eines Benutzernamens und Kennworts.

> [!NOTE]
> `<system.net>` ist ein untergeordnetes Element des Stamms `<configuration>` -Element und ein gleichgeordnetes Element eines `<system.web>`. Platzieren Sie daher nicht die `<system.net>` Element innerhalb der `<system.web>` -Element; stattdessen legen Sie sie auf der gleichen Ebene.


[!code-xml[Main](recovering-and-changing-passwords-vb/samples/sample1.xml)]

Zusätzlich zur Verwendung eines SMTP-Servers im Netzwerk, können Sie auch ein pickup-Verzeichnis angeben, in dem e-Mail-Nachrichten gesendet werden-Änderungstabellen abgelegt werden soll.

Nachdem Sie die SMTP-Einstellungen konfiguriert haben, besuchen Sie die `RecoverPassword.aspx` Seite über einen Browser. Versuchen Sie zunächst die Eingabe eines Benutzernamens, das nicht in den Speicher des Benutzers vorhanden ist. Wie in Abbildung 2 gezeigt, zeigt das Steuerelement PasswordRecovery eine Meldung gibt an, dass die Benutzerinformationen konnte nicht zugegriffen werden. Der Text der Nachricht kann angepasst werden, mithilfe des Steuerelements [ `UserNameFailureText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.usernamefailuretext.aspx).


[![Eine Fehlermeldung wird angezeigt, wenn ein ungültiger Benutzername eingegeben wird](recovering-and-changing-passwords-vb/_static/image5.png)](recovering-and-changing-passwords-vb/_static/image4.png)

**Abbildung 2**: eine Fehlermeldung wird angezeigt, wenn ein ungültiger Benutzername eingegeben wird ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image6.png))


Jetzt geben Sie einen Benutzernamen ein. Verwenden Sie der Benutzernamen eines Kontos in das System mit einer e-Mail-Adresse, die Sie zugreifen können und zu beantworten, deren Sicherheit Sie kennen. Nach dem Eingeben des Benutzernamens und Benutzer auf Absenden klickt, zeigt das PasswordRecovery-Steuerelement die Frageansicht. Als mit UserName-Ansicht bei Eingabe eines falschen beantworten zeigt das PasswordRecovery-Steuerelement, das eine Fehlermeldung angezeigt (siehe Abbildung 3). Verwenden der [ `QuestionFailureText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.questionfailuretext.aspx) zum Anpassen dieser Fehlermeldung.


[![Eine Fehlermeldung wird angezeigt, wenn der Benutzer ein ungültiges Sicherheitsantwort eingibt.](recovering-and-changing-passwords-vb/_static/image8.png)](recovering-and-changing-passwords-vb/_static/image7.png)

**Abbildung 3**: eine Fehlermeldung wird angezeigt, wenn der Benutzer ein ungültiges Sicherheitsantwort eingibt ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image9.png))


Abschließend geben Sie die richtige Sicherheits-Antwort, und klicken Sie auf Absenden. Hinter den Kulissen wird das Steuerelement PasswordRecovery ein zufälliges Kennwort generiert, weist sie dem Benutzerkonto, sendet eine e-Mail darüber informiert den Benutzer, der dem neuen Kennwort (siehe Abbildung 4), und anschließend die Ansicht erfolgreich angezeigt.


[![Der Benutzer wird eine E-Mail mit neuen His-Kennwort gesendet.](recovering-and-changing-passwords-vb/_static/image11.png)](recovering-and-changing-passwords-vb/_static/image10.png)

**Abbildung 4**: der Benutzer erhält eine E-Mail mit neuen His-Kennwort ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image12.png))


### <a name="customizing-the-email"></a>Anpassen der e-Mail-Adresse

Standard-e-Mail gesendet, die vom Steuerelement PasswordRecovery ist vielmehr gesamteinwirkung (siehe Abbildung 4). Die Nachricht wird gesendet, von dem im angegebenen Konto die `<smtp>` des Elements `from` -Attribut mit dem Betreff Kennwort und den nur-Text-Text:

Zur Website zurückkehren Sie, und melden Sie sich mit den folgenden Informationen.

Benutzername: *Benutzername*

Kennwort: *Kennwort*

Diese Meldung kann programmgesteuert über einen Ereignishandler für die PasswordRecovery-Steuerelements angepasst werden [ `SendingMail` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.sendingmail.aspx), oder deklarativ über die [ `MailDefinition` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.passwordrecovery.maildefinition.aspx). Betrachten Sie diese beiden Optionen aus.

Die `SendingMail` Ereignis wird ausgelöst, unmittelbar bevor die e-Mail-Nachricht wird gesendet, und unsere letzte Möglichkeit ist, programmgesteuert e-Mail-Nachricht anzupassen. Wenn dieses Ereignis ausgelöst wird, wird der Ereignishandler ein Objekt des Typs übergeben [ `MailMessageEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.mailmessageeventargs.aspx), dessen `Message` Eigenschaft enthält einen Verweis auf die e-Mail-Adresse gesendet werden soll.

Erstellen Sie einen Ereignishandler für die `SendingMail` Ereignis und fügen Sie den folgenden Code, der programmgesteuert fügt `webmaster@example.com` der CC-Liste.

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample2.vb)]

Die e-Mail-Nachricht kann auch auf deklarative Weise konfiguriert werden. Die PasswordRecoverys `MailDefinition` -Eigenschaft ist ein Objekt des Typs [ `MailDefinition` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.aspx). Die `MailDefinition` -Klasse bietet eine Vielzahl von e-Mail-bezogenen Eigenschaften verwendet werden, einschließlich `From`, `CC`, `Priority`, `Subject`, `IsBodyHtml`, `BodyFileName`, und andere. Legen Sie zunächst die [ `Subject` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.subject.aspx) in einen aussagekräftigeren als die Standardeinstellung (Kennwort) verwendet werden, z. B. Ihr Kennwort zurückgesetzt wurde...

Um den Text der e-Mail anpassen wir eine separate e-Mail-Vorlagendatei zu erstellen müssen enthält, die Text-Inhalt. Zunächst erstellen Sie einen neuen Ordner auf der Website mit dem Namen `EmailTemplates`. Fügen Sie eine neue Textdatei in diesem Ordner mit dem Namen `PasswordRecovery.txt` und fügen Sie folgenden Inhalt hinzu:

[!code-aspx[Main](recovering-and-changing-passwords-vb/samples/sample3.aspx)]

Beachten Sie die Verwendung der Platzhalter `<%UserName%>` und `<%Password%>`. Das Steuerelement PasswordRecovery werden diese beiden Platzhalter mit Benutzernamen und einem wiederhergestellten Kennwort vor dem Senden der e-Mail-Adresse des Benutzers automatisch ersetzt.

Zeigen Sie abschließend die `MailDefinition`des [ `BodyFileName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) der e-Mail-Vorlage, die wir gerade erstellt haben (`~/EmailTemplates/PasswordRecovery.txt`).

Nach dem vornehmen, diese erneut ändert die `RecoverPassword.aspx` Seite, und geben Sie Ihre Benutzernamen und Security-Antwort. Sie erhalten eine e-Mail, die wie in Abbildung 5 aussehen sollte. Beachten Sie, dass `webmaster@example.com` wurde, CC und würde, dass der Betreff und Text aktualisiert wurden.


[![Der Betreff, Nachrichtentext und CC-Liste wurden aktualisiert](recovering-and-changing-passwords-vb/_static/image14.png)](recovering-and-changing-passwords-vb/_static/image13.png)

**Abbildung 5**: der Betreff, Nachrichtentext und CC Liste aktualisiert wurden ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image15.png))


Legen Sie zum Senden einer HTML-formatierte e-Mail [ `IsBodyHtml` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.isbodyhtml.aspx) zum "true" (der Standardwert ist "false"), und Aktualisieren der e-Mail-Vorlage, um HTML-Code enthalten.

Die `MailDefinition` Eigenschaft ist nicht nur für die PasswordRecovery-Klasse. Wie wir in Schritt2 sehen, bietet das ChangePassword-Steuerelement auch eine `MailDefinition` Eigenschaft. Darüber hinaus enthält das Steuerelement CreateUserWizard eine solche Eigenschaft, die Sie konfigurieren können, um eine begrüßungs-e-Mail-Nachricht automatisch an neue Benutzer zu senden.

> [!NOTE]
> Derzeit stehen keine Links im linken Navigationsbereich erreichen, die `RecoverPassword.aspx` Seite. Ein Benutzer wird nur für die auf dieser Seite aus, wenn er nicht erfolgreich melden Sie sich bei der Site konnte interessiert sein. Aktualisieren Sie daher die `Login.aspx` Seite, um einen Link zu der `RecoverPassword.aspx` Seite.


### <a name="programmatically-resetting-a-users-password"></a>Programmgesteuertes Zurücksetzen des Kennworts eines Benutzers

Wenn das Zurücksetzen des Kennworts eines Benutzers die PasswordRecovery kontrollaufrufe der `MembershipUser` des Objekts [ `ResetPassword` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.resetpassword.aspx). Diese Methode verfügt über zwei Überladungen:

- **[`ResetPassword`](https://msdn.microsoft.com/library/d94bdzz2.aspx)** -das Kennwort eines Benutzers zurückgesetzt. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` ist "false".
- **[`ResetPassword(securityAnswer)`](https://msdn.microsoft.com/library/d90zte4w.aspx)** – setzt das Kennwort nur, wenn ein Benutzer die angegebene *SecurityAnswer* richtig ist. Verwenden Sie diese Überladung, wenn `RequiresQuestionAndAnswer` ist "true".

Beide Überladungen geben die neue, zufällig generierten Kennwort zurück.

Mit den anderen Methoden in das mitgliedschaftsframework, wie die `ResetPassword` -Methode delegiert an die konfigurierten Anbieter. Die `SqlMembershipProvider` Ruft die `aspnet_Membership_ResetPassword` gespeicherte Prozedur übergeben, den Benutzernamen des Benutzers, das neue Kennwort und die angegebene Kennwortantwort, u.a. Die gespeicherte Prozedur wird sichergestellt, dass die Antwort entspricht und das Kennwort des Benutzers aktualisiert.

Ein paar Hinweise zur Implementierung der Low-Level:

- Ein Benutzer gesperrter kann ihr Kennwort nicht zurücksetzen. Allerdings kann ein nicht genehmigter Benutzer. Wir besprechen die gesperrt und Zustände ausführlicher genehmigt die <a id="_msoanchor_3"> </a> [ *entsperren und Genehmigen von Benutzer* ](unlocking-and-approving-user-accounts-vb.md) Konten Tutorial.
- Wenn die Antwort für das Kennwort falsch ist, wird der Benutzer des Kennworts Antwort Anzahl erhöht. Wenn eine angegebene Anzahl von ungültigen Sicherheit Kennwortantwortversuche innerhalb eines angegebenen Zeitfensters auftreten, wird der Benutzer gesperrt.

### <a name="a-word-on-how-the-random-passwords-are-generated"></a>Ein Wort auf wie die zufällige Kennwörter generiert werden

Die zufällig generierte Kennwörter in die e-Mail-Nachrichten in den Abbildungen 4 und 5 gezeigten werden erstellt, indem die Mitgliedschaftsklasse [ `GeneratePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx). Diese Methode akzeptiert zwei ganzzahlige Parameter Eingabe - *Länge* und *NumberOfNonAlphanumericCharacters* - und mindestens eine Zeichenfolge zurückgibt *Länge* Zeichen lange mit an mindestens *NumberOfNonAlphanumericCharacters* Anzahl von nicht-alphanumerischen Zeichen. Wenn diese Methode innerhalb der Membership-Klassen oder Web anmeldebezogene Steuerelemente aus aufgerufen wird, werden die Werte für diese beiden Parameter durch die Mitgliedschaft in der Konfiguration bestimmt `MinRequiredPasswordLength` und `MinRequiredNonalphanumericCharacters` Eigenschaften, die wir auf 7 bzw. auf 1 festgelegt.

Die `GeneratePassword` Methode verwendet einen kryptographisch starken Zufallszahlengenerator, um sicherzustellen, dass es keine Verschiebung in welche zufällige Zeichen markiert sind. Darüber hinaus `GeneratePassword` ist `Public`, was bedeutet, dass Sie es direkt aus Ihrer ASP.NET-Anwendung verwenden können, wenn Sie generieren zufälliger Zeichenfolgen oder Kennwörter müssen.

> [!NOTE]
> Die `SqlMembershipProvider` Klasse immer ein zufälliges Kennwort generiert mindestens 14 Zeichen lang sein, wenn also `MinRequiredPasswordLength` beträgt weniger als 14, und klicken Sie dann ihr Wert wird ignoriert.


## <a name="step-2-changing-passwords"></a>Schritt 2: Ändern von Kennwörtern

Die zufällig generierte Kennwörter sind schwer zu merken. Betrachten Sie das Kennwort, das in Abbildung 4 dargestellte: `WWGUZv(f2yM:Bd`. Testen Sie Ausführen eines Commits für, die in den Speicher Nachdem ein Benutzer ein zufällig generiertes Kennwort dieser Art gesendet wurde, sollten sie natürlich das Kennwort in einen einprägsameren ändern.

Verwenden Sie die ChangePassword-Steuerelement, um eine Schnittstelle für einen Benutzer zum Ändern des Kennworts zu erstellen. Viel wie das Steuerelement PasswordRecovery, ChangePassword-Steuerelement besteht aus zwei Ansichten: Kennwort ändern und den Erfolg. Der Benutzer für ihre alte und neue Kennwörter werden von die Ansicht Kennwort ändern aufgefordert. Bei der Bereitstellung das richtige Kennwort und ein neues Kennwort, das die minimale Länge und ein nicht alphanumerisches Zeichen-Anforderungen erfüllt, ChangePassword-Steuerelement aktualisiert das Kennwort des Benutzers und die Ansicht erfolgreich angezeigt.

> [!NOTE]
> ChangePassword-Steuerelement ändert das Kennwort des Benutzers durch den Aufruf der `MembershipUser` des Objekts [ `ChangePassword` Methode](https://msdn.microsoft.com/library/system.web.security.membershipuser.changepassword.aspx). Die ChangePassword-Methode akzeptiert zwei `String` Eingabeparameter - *OldPassword* und *NewPassword*- und aktualisiert das Konto des Benutzers mit der *NewPassword*, Wenn die angegebene *OldPassword* richtig ist.


Öffnen der `ChangePassword.aspx` Seite, und fügen Sie ein ChangePassword-Steuerelement auf der Seite, und nennen Sie es `ChangePwd`. An diesem Punkt sollte die Entwurfsansicht das Ändern von Kennwörtern anzeigen angezeigt werden (siehe Abbildung 6). Wie können mit dem Steuerelement PasswordRecovery Sie zwischen den Ansichten über Smart Tag des Steuerelements wechseln. Darüber hinaus sind diese Sichten Darstellungen anpassbar, über die verschiedene Eigenschaften oder diese in eine Vorlage konvertieren.


[![Ein ChangePassword-Steuerelement auf der Seite hinzufügen](recovering-and-changing-passwords-vb/_static/image17.png)](recovering-and-changing-passwords-vb/_static/image16.png)

**Abbildung 6**: ein ChangePassword-Steuerelement auf der Seite hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image18.png))


ChangePassword-Steuerelement kann das Kennwort des aktuell angemeldeten Benutzers aktualisieren *oder* das Kennwort des Benutzers von einem anderen, angegeben. Wie in Abbildung 6 gezeigt wird, rendert die Standardansicht für das Ändern von Kennwörtern nur drei TextBox-Steuerelemente: eines für das alte Kennwort und zwei für das neue Kennwort. Diese Standardschnittstelle wird verwendet, um das Kennwort des angemeldeten Benutzers zu aktualisieren.

Um die ChangePassword-Steuerelement beim Aktualisieren eines anderen Benutzers Kennworts zu verwenden, legen Sie die [ `DisplayUserName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.changepassword.displayusername.aspx) auf "true". Auf diese Weise wird der Seite Aufforderung für den Benutzernamen des Benutzers, dessen Kennwort zu ändern, ein viertes Textfeld hinzugefügt.

Festlegen von `DisplayUserName` auf "true" ist nützlich, wenn Sie einen out protokollierten Benutzer, die das Kennwort zu ändern, ohne dass melden Sie sich informieren möchten. Ich persönlich glaube, dass nichts auszusetzen, dass der Benutzer, die Anmeldung vor, sodass ihr Zugriff auf das Kennwort ändern muss. Aus diesem Grund lassen `DisplayUserName` auf "false" (die Standardeinstellung) festgelegt. In diese Entscheidung treffen, sind jedoch wir im Wesentlichen anonyme Benutzer auf dieser Seite erreicht was ausschließt. Aktualisieren der Website-URL-Autorisierungsregeln um finden Sie unter für anonyme Benutzer Verweigern `ChangePassword.aspx`. Wenn Sie Ihren Speicher auf die Syntax der URL-Autorisierung-Regel aktualisieren möchten, verweisen zurück auf die <a id="_msoanchor_4"> </a> [ *Benutzerbasierte Autorisierung* ](../membership/user-based-authorization-vb.md) Tutorial.

> [!NOTE]
> Es mag, die die `DisplayUserName` Eigenschaft ist nützlich für das Zulassen von Administratoren anderer Benutzer Kennwörter ändern. Auch wenn `DisplayUserName` festgelegt ist, auf "true", das richtige Kennwort muss bekannt sein und eingegeben haben. Zu den Verfahren zum ermöglichen es Administratoren, die zum Ändern von Benutzerkennwörtern in Schritt 3 vorgestellt werden.


Besuchen Sie die `ChangePassword.aspx` Seite über einen Browser, und Ihr Kennwort ändern. Beachten Sie, dass eine Fehlermeldung angezeigt wird, wenn Sie ein neues Kennwort eingeben, die nicht dem Kennwortlänge und nicht-alphanumerische Zeichen in der Konfiguration der Mitgliedschaft angegeben erfüllen (siehe Abbildung 7).


[![Ein ChangePassword-Steuerelement auf der Seite hinzufügen](recovering-and-changing-passwords-vb/_static/image20.png)](recovering-and-changing-passwords-vb/_static/image19.png)

**Abbildung 7**: ein ChangePassword-Steuerelement auf der Seite hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image21.png))


Klicken Sie auf das richtige Kennwort für die alte und eine gültige neue Kennwort einzugeben, des angemeldeten Benutzers Kennwort geändert wird, und die Ansicht erfolgreich angezeigt.

### <a name="sending-a-confirmation-email"></a>Eine Bestätigung per E-Mail senden

Standardmäßig sendet das ChangePassword-Steuerelement keine e-Mail-Nachricht an den Benutzer, dessen Kennwort gerade aktualisiert wurde. Wenn Sie eine e-Mail senden möchten, ganz einfach konfigurieren, des Steuerelements `MailDefinition` Eigenschaft. Lassen Sie uns die ChangePassword-Steuerelement so konfigurieren, dass dem Benutzer eine HTML-formatierte e-Mail gesendet wird, die das neue Kennwort enthält.

Zunächst erstellen Sie eine neue Datei in die `EmailTemplates` Ordner mit dem Namen `ChangePassword.htm`. Fügen Sie das folgende Markup hinzu:

[!code-html[Main](recovering-and-changing-passwords-vb/samples/sample4.html)]

Legen Sie als Nächstes die ChangePassword-Steuerelement `MailDefinition` Eigenschaft `BodyFileName`, `IsBodyHtml`, und `Subject` Eigenschaften ~ / EmailTemplates/ChangePassword.htm "true" und Ihr Kennwort wurde geändert! bzw.

Nach diesen Änderungen, die Seite, und Ihr Kennwort erneut ändern. Dieses Mal sendet die ChangePassword-Steuerelement ein benutzerdefiniertes HTML-formatierte e-Mail-an-e-Mail-Adresse des Benutzers auf die Datei (siehe Abbildung 8).


[![Eine e-Mail-Nachricht informiert das, dass deren Kennwort des Benutzers wurde geändert](recovering-and-changing-passwords-vb/_static/image23.png)](recovering-and-changing-passwords-vb/_static/image22.png)

**Abbildung 8**: eine e-Mail-Nachricht informiert den Benutzer, deren Kennwort geändert hat ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image24.png))


## <a name="step-3-allowing-administrators-to-change-users-passwords"></a>Schritt 3: Ermöglichen Administratoren, Benutzerkennwörter zu ändern.

Ein Feature in Anwendungen mit Unterstützung für Benutzerkonten ist die Möglichkeit für einen Administrator, um andere Benutzer den Kennwörter zu ändern. Diese Funktionalität ist manchmal erforderlich, da das System verfügt nicht über die die Möglichkeit für Benutzer ihre eigenen Kennwörter ändern. In diesem Fall wäre die einzige Möglichkeit für einen Benutzer ihr Kennwort vergessen haben wiederherstellen für den Administrator ein neues Kennwort zuweisen. Mit den Steuerelementen PasswordRecovery und ChangePassword müssen jedoch Administratoren nicht selbst gebucht Ändern von Benutzerkennwörtern, wie die Benutzer können diese selbst sind.

Aber was geschieht, wenn der Client gelingen, dass Administratoren anderer Benutzer Kennwörter ändern können soll? Diese Funktionalität hinzufügen kann leider etwas aufwendiger sein. Um das Kennwort eines Benutzers zu ändern, muss die alte und neue Kennwort angegeben werden die `MembershipUser` des Objekts `ChangePassword` -Methode, aber ein Administrator sollte nicht haben, das Kennwort eines Benutzers zu kennen, um es zu ändern.

Eine problemumgehung besteht darin, zuerst das Kennwort des Benutzers zurückgesetzt, und ändern Sie ihn dann in das neue Kennwort mit dem folgenden Code:

[!code-vb[Main](recovering-and-changing-passwords-vb/samples/sample5.vb)]

Dieser Code beginnt, durch das Abrufen von Informationen zu *Benutzername*, d.h., dass der Benutzer, dessen Kennwort möchte, dass der Administrator zu ändern. Als Nächstes die `ResetPassword` Methode wird aufgerufen, welche zugewiesen und dem Benutzer ein neues, zufälliges Kennwort. Diese willkürlich generiertem Kennwort ist von der Methode zurückgegeben und in der Variablen gespeicherten `resetPwd`. Nun, wir wissen, dass das Kennwort des Benutzers, kann geändert werden, es über einen Aufruf an `ChangePassword`.

Das Problem besteht darin, dass dieser Code funktioniert nur, wenn die Konfiguration der Mitgliedschaft System festgelegt ist, dass `RequiresQuestionAndAnswer` ist "false". Wenn `RequiresQuestionAndAnswer` ist "true", da es in unserer Anwendung die `ResetPassword` Methode die Sicherheitsantwort übergeben werden muss, andernfalls wird es eine Ausnahme ausgelöst.

Wenn das mitgliedschaftsframework so konfiguriert ist, dass Sie eine Sicherheitsfrage und eine Antwort erforderlich, und der Client noch gelingen, dass Administratoren können Benutzerkennwörter zu ändern, haben Sie drei Optionen:

- Lösen Sie sich in der Luft, und teilen Sie dem Client, dass dies nur einen Aspekt ist, die nicht ausgeführt werden kann.
- Legen Sie `RequiresQuestionAndAnswer` auf "false". Dies führt zu einer weniger sicheren Anwendung. Angenommen Sie, die ein böswilligen Benutzer den Zugriff auf die e-Mail-Posteingang eines anderen Benutzers erlangt hat. Vielleicht kompromittierte Benutzer ihrem Schreibtisch aus, wechseln Sie in der Mittagspause verlassen hat, und nicht zu ihrer Arbeitsstation sperren, oder vielleicht sie Zugriff auf ihre e-Mails über das öffentliche Terminal, und melden Sie sich nicht ab. In beiden Fällen kann der böswilligen Benutzer besuchen Sie die `RecoverPassword.aspx` Seite, und geben Sie den Benutzernamen des Benutzers. Das System wird dann die wiederhergestellte Kennwort per e-Mail ohne die Sicherheitsantwort.
- Umgehen Sie die Abstraktionsschicht, die durch das mitgliedschaftsframework und arbeiten direkt mit SQL Server-Datenbank erstellt. Das Schema der Mitgliedschaft enthält eine gespeicherte Prozedur namens `aspnet_Membership_SetPassword` , legt das Kennwort eines Benutzers und erfordert die Sicherheitsantwort oder das alte Kennwort nicht um die Aufgabe.

Keine dieser Optionen sind besonders attraktiv, aber das ist wie das Leben eines Entwicklers in einigen Fällen geht.

Ich habe und der dritte Ansatz darin, implementiert, auf das Schreiben von Code, das die umgeht die `Membership` und `MembershipUser` Klassen und direkt für den Zugriff der `SecurityTutorials` Datenbank.

> [!NOTE]
> Arbeiten Sie direkt mit der Datenbank, ist die Kapselung zur Verfügung gestellt durch das mitgliedschaftsframework davongeweht. Diese Entscheidung bindet werden uns die `SqlMembershipProvider`, wodurch unsere kleiner Portable. Darüber hinaus kann dieser Code funktioniert nicht wie in zukünftigen Versionen von ASP.NET erwartet, wenn das Schema für die Mitgliedschaft ändert. Dieser Ansatz ist dieses Problem zu umgehen und, wie die meisten problemumgehungen kein Beispiel für bewährte Methoden.


Der Code hat einige unschöne Bits und ist ziemlich umfassend. Aus diesem Grund möchte ich dieses Tutorial mit einer ausführlichen Untersuchung des Zertifikats mit überflüssigen Daten gefüllt. Wenn Sie weiteren interessiert sind, laden Sie den Code für dieses Tutorial und besuchen Sie die `~/Administration/ManageUsers.aspx` Seite. Diese Seite, die wir in den erstellt die <a id="_msoanchor_5"> </a> [vorherigen Lernprogramm](building-an-interface-to-select-one-user-account-from-many-vb.md), jeder Benutzer führt. Ich habe die GridView enthält einen Link zu aktualisiert die `UserInformation.aspx` Seite, die den ausgewählten Benutzernamen des Benutzers über die Abfragezeichenfolge übergeben. Die `UserInformation.aspx` Seite zeigt Informationen zu den ausgewählten Benutzer und die Textfelder für ihr Kennwort ändern (siehe Abbildung 9).

Nachdem das neue Kennwort eingegeben, bestätigen es in das zweite Textfeld ein und klicken Sie auf die Schaltfläche "Benutzer aktualisieren", ein Postback erfolgt und die `aspnet_Membership_SetPassword` gespeicherte Prozedur aufgerufen wird, aktualisieren das Kennwort des Benutzers. Ich empfehle diese Funktionalität interessiert Leser, die mit dem Code vertraut, und versuchen Sie es zum Erweitern der Funktionalität enthält, das Senden einer e-Mail an den Benutzer, dessen Kennwort geändert wurde.


[![Ein Administrator kann das Kennwort eines Benutzers ändern.](recovering-and-changing-passwords-vb/_static/image26.png)](recovering-and-changing-passwords-vb/_static/image25.png)

**Abbildung 9**: ein Administrator kann das Kennwort eines Benutzers ändern ([klicken Sie, um das Bild in voller Größe anzeigen](recovering-and-changing-passwords-vb/_static/image27.png))


> [!NOTE]
> Die `UserInformation.aspx` Seite derzeit funktioniert nur, wenn das mitgliedschaftsframework zum Speichern von Kennwörtern im deaktivieren oder die Hashed-Format konfiguriert ist. Er verfügt nicht über den Code, um das neue Kennwort, Verschlüsselung aus, auch wenn Sie gefragt werden, um diese Funktionalität hinzufügen. Die Möglichkeit, empfehle ich den erforderlichen Code hinzufügen, ist die Verwendung von Decompiler wie [Reflector](http://www.aisto.com/roeder/dotnet/) untersuchen den Quellcode für Methoden in .NET Framework mit der Untersuchung der `SqlMembershipProvider` Klasse `ChangePassword` Methode. Dies ist das Verfahren, mit denen den Code zum Erstellen von eines Hashs des Kennworts zu schreiben.


## <a name="summary"></a>Zusammenfassung

ASP.NET bietet zwei Steuerelemente können Benutzer ihre Kennwörter zu verwalten. Das Steuerelement PasswordRecovery ist nützlich für diejenigen, die ihre Kennwörter vergessen haben. Je nach Konfiguration für das mitgliedschaftsframework wird der Benutzer entweder ihr vorhandenes Kennwort oder ein neues, zufällig generierten Kennwort per e-Mail gesendet. ChangePassword-Steuerelement ermöglicht einen Benutzer aktualisieren seines Kennworts aufgefordert.

Wie die Anmeldung und CreateUserWizard-Steuerelemente rendern die PasswordRecovery und ChangePassword-Steuerelemente eine umfangreiche Benutzeroberfläche ohne jeglichen deklaratives Markup oder Codezeile schreiben zu müssen. Wenn die Standardbenutzeroberfläche Ihren Anforderungen nicht erfüllt, können Sie es über verschiedene Eigenschaften anpassen. Alternativ können der Steuerelemente Schnittstellen in Vorlagen für ein noch Feinerer Grad an Steuerelement konvertiert werden. Hinter den Kulissen diese Steuerelemente verwenden der Mitgliedschafts-API Aufrufen der `MembershipUser` des Objekts `ResetPassword` und `ChangePassword` Methoden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [ChangePassword-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/changepassword.aspx)
- [PasswordRecovery-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/passwordrecovery.aspx)
- [Senden von E-Mails in ASP.NET](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx)
- [`System.Net.Mail` Häufig gestellte Fragen](http://www.systemnetmail.com/)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial enthalten Michael Emmings und Suchi Banerjee. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Zurück](building-an-interface-to-select-one-user-account-from-many-vb.md)
> [Weiter](unlocking-and-approving-user-accounts-vb.md)
