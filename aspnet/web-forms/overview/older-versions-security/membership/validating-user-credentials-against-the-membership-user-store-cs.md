---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
title: "Überprüfen die Anmeldeinformationen des Benutzers für die Mitgliedschaft Benutzerspeicher (c#) | Microsoft Docs"
author: rick-anderson
description: "In diesem Lernprogramm werden wie beim Überprüfen der Anmeldeinformationen des Benutzers, für die Mitgliedschaft Benutzerspeicher programmgesteuerte Möglichkeit und das Steuerelement für die Anmeldung mit untersucht..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 61aa4e08-aa81-4aeb-8ebe-19ba7a65e04c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-cs
msc.type: authoredcontent
ms.openlocfilehash: 66ebcebfc3eda3bce416da1adea0fd4d7f39a5e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="validating-user-credentials-against-the-membership-user-store-c"></a>Überprüfen die Anmeldeinformationen des Benutzers für die Mitgliedschaft Benutzerspeicher (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_cs.pdf)

> In diesem Lernprogramm werden wie beim Überprüfen der Anmeldeinformationen des Benutzers, für die Mitgliedschaft Benutzerspeicher programmgesteuerte Möglichkeit und das Steuerelement für die Anmeldung mit untersucht. Wir werden zum Anpassen der Darstellung und das Verhalten des Steuerelements für die Anmeldung betrachten.


## <a name="introduction"></a>Einführung

In der <a id="Tutorial05"> </a> [vorherigen Lernprogramm](creating-user-accounts-cs.md) Vorgehensweise: erstellen ein neuen Benutzerkontos in Mitgliedschaft Framework erläutert. Erläutert, zuerst Programmgesteuertes Erstellen von Benutzerkonten über die `Membership` -Klasse `CreateUser` -Methode, und klicken Sie dann überprüft der CreateUserWizard-Websteuerelement. Allerdings überprüft die Anmeldeseite derzeit die angegebenen Anmeldeinformationen für eine vorprogrammierte Liste von Benutzername und Kennwort-Paaren. Wir müssen die Anmeldeseite Logik aktualisieren, damit sie die Anmeldeinformationen für die Mitgliedschaft Framework Benutzerspeicher überprüft.

Ähnlich können z. B. mit dem Erstellen von Benutzerkonten, Anmeldeinformationen programmgesteuert oder deklarativ überprüft werden. Die Mitgliedschaft-API beinhaltet eine Methode für die Anmeldeinformationen eines Benutzers für den Speicher des Benutzers programmgesteuert überprüfen. Und ASP.NET im Lieferumfang des Websteuerelements Anmeldung, die eine Benutzeroberfläche mit Textfelder für den Benutzernamen und Kennwort und eine Schaltfläche zum Anmelden rendert.

In diesem Lernprogramm werden wie beim Überprüfen der Anmeldeinformationen des Benutzers, für die Mitgliedschaft Benutzerspeicher programmgesteuerte Möglichkeit und das Steuerelement für die Anmeldung mit untersucht. Wir werden zum Anpassen der Darstellung und das Verhalten des Steuerelements für die Anmeldung betrachten. Fangen wir an!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Schritt 1: Die Anmeldeinformationen für den Benutzerspeicher Mitgliedschaft überprüft.

Für Websites, die Formularauthentifizierung verwenden, meldet einen Benutzer auf der Website besuchen einer Anmeldeseite, und geben ihre Anmeldeinformationen. Diese Anmeldeinformationen werden dann mit den Speicher des Benutzers verglichen. Wenn sie gültig sind, wird der Benutzer ein Formularauthentifizierungsticket gewährt, ein Sicherheitstoken, die die Identität und die Authentizität des Besuchers angibt.

Verwenden Sie zum Überprüfen von eines Benutzers für das Framework für die Mitgliedschaft der `Membership` Klasse [ `ValidateUser` Methode](https://msdn.microsoft.com/en-us/library/system.web.security.membership.validateuser.aspx). Die `ValidateUser` -Methode übernimmt zwei Eingabeparameter -  *`username`*  und  *`password`*  - und gibt einen booleschen Wert, der angibt, ob die Anmeldeinformationen gültig sind. Wie Sie mit der `CreateUser` Methode, die im vorherigen Lernprogramm untersucht die `ValidateUser` Methode delegiert die tatsächliche Validierung an den konfigurierten Mitgliedschaftsanbieter.

Die `SqlMembershipProvider` überprüft die angegebenen Anmeldeinformationen erhalten Sie das angegebene Kennwort des Benutzers über die `aspnet_Membership_GetPasswordWithFormat` gespeicherte Prozedur. Bedenken Sie, dass die `SqlMembershipProvider` speichert von Benutzerkennwörtern, die mit einer der drei Formate: Clear, verschlüsselt oder ein Hashwert erstellt. Die `aspnet_Membership_GetPasswordWithFormat` gespeicherte Prozedur gibt das Kennwort im raw-Format zurück. Für verschlüsselte oder eine verschlüsselte Kennwörter der `SqlMembershipProvider` transformiert die  *`password`*  übergebenen Wert die `ValidateUser` in die entsprechende Methode verschlüsselt oder Status gehasht und vergleicht ihn dann mit der Rückgabe aus der die Datenbank. Wenn das Kennwort in der Datenbank gespeichert, das formatierte, vom Benutzer eingegebene Kennwort übereinstimmt, sind die Anmeldeinformationen gültig.

Wir aktualisieren unsere Anmeldeseite (~ /`Login.aspx`), damit sie die angegebenen Anmeldeinformationen für den Speicher des Benutzers Mitgliedschaft Framework überprüft. Wir diese Anmeldeseite erstellt zurück in die <a id="Tutorial02"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) Lernprogramm erstellen eine Schnittstelle mit zwei Textfelder für den Benutzernamen und das Kennwort, eine Kontrollkästchen, und eine Anmeldeschaltfläche speichern (siehe Abbildung 1). Der Code überprüft die eingegebenen Anmeldeinformationen für eine vorprogrammierte Liste von Benutzername und Kennwort-Paaren (Scott/Kennwort, Jisun/Kennwort und Sam/Kennwort). In der <a id="Tutorial03"> </a> [ *Konfiguration der Formularauthentifizierung und erweiterte Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) Lernprogramm wir aktualisiert, die Anmeldeseite Code aus, um zusätzliche Informationen in den Formen zu speichern. der Authentifizierungsticket `UserData` Eigenschaft.


[![Die Anmeldeseite Schnittstelle enthält, zwei Textfelder, einer CheckBoxList und eine Schaltfläche](validating-user-credentials-against-the-membership-user-store-cs/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image1.png)

**Abbildung 1**: die Anmeldeseite Schnittstelle enthält zwei Textfelder, einer CheckBoxList und eine Schaltfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image3.png))


Benutzeroberfläche für die Anmeldeseite kann bleiben unverändert, aber wir benötigen der Anmeldeschaltfläche ersetzen `Click` -Ereignishandler durch den Code, der den Benutzer für die Mitgliedschaft Framework Benutzerspeicher überprüft. Aktualisieren Sie den Ereignishandler, sodass der Code wie folgt aussieht:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample1.cs)]

Dieser Code ist erstaunlich einfach. Beginnen wir durch Aufrufen der `Membership.ValidateUser` -Methode auf und übergibt den angegebenen Benutzernamen und das Kennwort. Wenn diese Methode gibt "true" zurück, der Benutzer angemeldet ist, in der Website über die `FormsAuthentication` -Methode der Basisklasse RedirectFromLoginPage. (Wie in erläutert die <a id="Tutorial2"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) Lernprogramm die `FormsAuthentication.RedirectFromLoginPage` erstellt die Formulare Authentifizierungsticket, und klicken Sie dann leitet den Benutzer um die entsprechende Seite.) Wenn die Anmeldeinformationen jedoch ungültig ist, sind die `InvalidCredentialsMessage` Bezeichnung angezeigt wird, den Benutzer darüber informiert, dass ihre Benutzername oder Kennwort falsch war.

Das ist alles vorhanden ist!

Um zu testen, dass die Anmeldeseite erwartungsgemäß funktioniert, versuchen Sie mit einem Benutzerkonto anmelden, die Sie in dem vorherigen Lernprogramm erstellt haben. Oder, wenn Sie ein Konto noch nicht erstellt haben, fahren Sie fort, und erstellen Sie eine aus der `~/Membership/CreatingUserAccounts.aspx` Seite.

> [!NOTE]
> Wenn der Benutzer ihre Anmeldeinformationen eingibt und das Anmeldeformular Seite sendet, werden die Anmeldeinformationen an, z. B. das Kennwort über das Internet an den Webserver in übertragen *Klartext*. Das bedeutet, dass alle Hacker sniffing den Netzwerkdatenverkehr des Benutzernamens und Kennworts angezeigt werden. Um dies zu verhindern, es ist wichtig, den Netzwerkdatenverkehr mit verschlüsseln [Secure Socket Layer (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dadurch wird sichergestellt, dass die Anmeldeinformationen (ebenso wie die gesamte Seite HTML-Markup) ab dem Zeitpunkt verschlüsselt werden, die sie den Browser lassen, bis sie vom Webserver empfangen werden.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Behandlung von ungültigen Anmeldeversuchen durch das Framework Mitgliedschaft

Wenn ein Besucher die Anmeldeseite erreicht und übermittelt ihre Anmeldeinformationen, sendet ihren Browser eine HTTP-Anforderung zur Anmeldeseite. Wenn die Anmeldeinformationen gültig sind, enthält die HTTP-Antwort das Authentifizierungsticket in einem Cookie. Daher kann ein Hacker versucht, unterbrechen Ihre Website ein Programm erstellen, die umfassend HTTP-Anforderungen an die Anmeldeseite mit einem gültigen Benutzernamen und einen Schätzwert ein, an das Kennwort sendet. Wenn das Kennwort Schätzung richtig ist, gibt die Anmeldeseite das Authentifizierungscookie Ticket zurück zu diesem, das Zeitpunkt weiß das Programm, dass er auf eine gültige Benutzername/Kennwort-Paar gestoßen ist. Durch Brute-Force solches Programm auf das Kennwort eines Benutzers, Fehlernachrichten möglicherweise besonders, wenn das Kennwort schwach ist.

Um solche Brute-Force-Angriffe zu verhindern, sperrt das Framework der Mitgliedschaft eines Benutzers, wenn eine bestimmte Anzahl von nicht erfolgreichen Anmeldeversuche innerhalb einer bestimmten Zeitspanne vorhanden sind. Die genauen Parameter können über die folgenden zwei Mitgliedschaft Anbieter Konfigurationseinstellungen konfiguriert werden:

- `maxInvalidPasswordAttempts`-Gibt an, wie viele wurde ein ungültiges Kennwort Versuche dürfen für den Benutzer innerhalb des Zeitraums, bevor das Konto gesperrt ist. Der Standardwert ist 5.
- `passwordAttemptWindow`-Gibt den Zeitraum in Minuten, während dessen die angegebene Anzahl von ungültigen Anmeldeversuchen führt dazu, dass das Konto gesperrt wird. Der Standardwert ist 10.

Wenn ein Benutzer gesperrt wurde, kann nicht er sich erst anmelden, ein Administrator ihr Konto entsperrt. Wenn ein Benutzer gesperrt ist die `ValidateUser` Methode wird *immer* zurückgeben `false`, selbst wenn Sie gültige Anmeldeinformationen angegeben werden. Während dieses Verhalten die Wahrscheinlichkeit, die ein Hacker durch Brute-Force-Methoden in Ihrer Website unterbrochen wird verringert, kann er ein gültiger Benutzer, einfach das Kennwort vergessen hat oder versehentlich die FESTSTELLTASTE aktiviert ist, auf oder hat einen ungültigen Eingabe Tag, Sperrung annehmen.

Leider ist kein integriertes Tool zum Entsperren eines Benutzerkontos. Um ein Konto entsperren zu können, können Sie Änderungen an der Datenbank direkt - ändern der `IsLockedOut` -Feld in der `aspnet_Membership` für das entsprechende Benutzerkonto - Tabelle oder eine webbasierte Schnittstelle erstellen, die listet gesperrt, Konten mit Optionen, um sie zu entsperren. Untersuchen wir erstellen administratorschnittstellen für das Ausführen der häufig anfallenden Benutzeraufgaben Konto und rollenbezogenen in einem späteren Lernprogramm.

> [!NOTE]
> Ein Nachteil der `ValidateUser` Methode ist, wenn die angegebenen Anmeldeinformationen ungültig sind, kein Erklärung verdeutlichen, warum bereitgestellt wird. Die Anmeldeinformationen möglicherweise ungültig, da keine übereinstimmenden Benutzername/Kennwort-Paar im Speicher Benutzers vorhanden ist oder, da der Benutzer noch nicht genehmigt wurde oder weil der Benutzer gesperrt wurde. In Schritt 4 sehen wir, wie eine detaillierte Meldung für dem Benutzer angezeigt, wenn ihre Anmeldeversuch einen Fehler erzeugt.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Schritt 2: Sammeln von Anmeldeinformationen über die Login-Websteuerelement

Die [Anmeldung Websteuerelement](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.aspx) rendert eine Standardbenutzeroberfläche sehr ähnlich, mit dem es erstellt, wieder in haben die <a id="SKM5"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-cs.md) Lernprogramm. Verwenden das Steuerelement für die Anmeldung erspart uns die erstellen die Schnittstelle, um den Besucher-s-Anmeldeinformationen zu sammeln. Darüber hinaus signiert das Steuerelement für die Anmeldung automatisch in der Benutzerliste (vorausgesetzt, dass die übermittelten Anmeldeinformationen gültig sind), und Speichern von uns keinen Code schreiben müssen.

Wir aktualisieren `Login.aspx`, und Ersetzen Sie dabei den manuell erstellten Benutzeroberfläche und code mit einem Anmeldungssteuerelement. Entfernen Sie zuerst das vorhandene Markup und code in `Login.aspx`. Sofortiges zu löschen, oder kommentieren Sie es aus. Um deklaratives Markup auskommentieren, setzen Sie sie mit der `<%--` und `--%>` Trennzeichen. Sie können diese Trennzeichen manuell eingeben, oder wie in Abbildung 2 gezeigt, kann, wählen Sie den Text zu kommentieren Sie Sie aus, und klicken Sie dann auf die Kommentieren Sie das Symbol "ausgewählte Zeilen" in der Symbolleiste. Auf ähnliche Weise können Sie die Kommentieren Sie das Symbol für die ausgewählten Zeilen verwenden, so kommentieren Sie den ausgewählten Code in der CodeBehind-Klasse.


[![Kommentieren Sie die vorhandenen deklarativem Markup und den Quellcode in "Login.aspx"](validating-user-credentials-against-the-membership-user-store-cs/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image4.png)

**Abbildung 2**: Kommentar, der vorhandenen deklarativem Markup und Quellcode in `Login.aspx` ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image6.png))


> [!NOTE]
> Die Kommentieren Sie das Symbol "ausgewählte Zeilen" ist nicht verfügbar, beim Anzeigen von deklarativem Markup in Visual Studio 2005. Wenn Sie nicht Visual Studio 2008 verwenden, müssen manuell hinzufügen der `<%--` und `--%>` Trennzeichen.


Ziehen Sie eine Anmeldesteuerelement aus der Toolbox auf die Seite, und legen Sie dessen `ID` Eigenschaft `myLogin`. An diesem Punkt sollte Ihr Bildschirm in Abbildung 3 ähneln. Beachten Sie, dass die Anmeldung des Steuerelements Standardschnittstelle TextBox-Steuerelemente für den Benutzernamen und Kennwort, eine Anmeldedaten Kontrollkästchen und eine Schaltfläche "Protokoll In" enthält. Es gibt auch `RequiredFieldValidator` Steuerelemente für die beiden Textfelder ein.


[![Hinzufügen eines Steuerelements für die Anmeldung zu der Seite "](validating-user-credentials-against-the-membership-user-store-cs/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image7.png)

**Abbildung 3**: Hinzufügen eines Steuerelements für die Anmeldung zu der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image9.png))


Und wir sind fertig! Bei der Anmeldung des Steuerelements anmelden Schaltfläche klicken, wird ein Postback tritt auf, und rufen Sie das Steuerelement für die Anmeldung wird die `Membership.ValidateUser` Methode und übergeben des eingegebenen Benutzernamens und Kennworts. Wenn die Anmeldeinformationen ungültig sind, zeigt das Steuerelement für die Anmeldung eine Meldung angezeigt, z. B. an. Wenn Sie jedoch die Anmeldeinformationen gültig sind, klicken Sie dann das Steuerelement für die Anmeldung erstellt die Formulare Authentifizierungsticket, und leitet den Benutzer an die entsprechende Seite.

Das Steuerelement für die Anmeldung verwendet vier Faktoren, die entsprechende Seite zum Umleiten des Benutzers, um nach einer erfolgreichen Anmeldung festzulegen:

- Gibt an, ob das Steuerelement für die Anmeldung auf der Anmeldeseite wird gemäß der Definition von `loginUrl` ist der Standardwert für diese Einstellung festlegen, in die Konfiguration der Formularauthentifizierung;`Login.aspx`
- Das Vorhandensein einer `ReturnUrl` Querystring-Parameter
- Der Wert des Steuerelements für die Anmeldung [ `DestinationUrl` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- Die `defaultUrl` Wert, der in den Formularen Konfigurationseinstellungen für die Authentifizierung angegeben; der Standardwert für diese Einstellung ist`Default.aspx`

Abbildung 4 zeigt, wie das Steuerelement für die Anmeldung verwendet diese vier Parameter, die entsprechende Seite Entscheidung ankommen.


[![Hinzufügen eines Steuerelements für die Anmeldung zu der Seite "](validating-user-credentials-against-the-membership-user-store-cs/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image10.png)

**Abbildung 4**: Hinzufügen eines Steuerelements für die Anmeldung zu der Seite "([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image12.png))


Nehmen Sie einen Moment Zeit, von der Website über einen Browser und Anmeldung als einen vorhandenen Benutzer in die Mitgliedschaft Framework das Steuerelement für die Anmeldung zu testen.

Gerenderten des Login-Steuerelements-Schnittstelle ist hochgradig konfigurierbar. Es gibt eine Reihe von Eigenschaften, die ihre Darstellung zu beeinflussen. Darüber kann das Steuerelement für die Anmeldung zu einer Vorlage für präzise Kontrolle über das Layout der Elemente der Benutzeroberfläche konvertiert werden. Der Rest dieses Schritts untersucht, wie die Darstellung und das Layout anpassen.

### <a name="customizing-the-login-controls-appearance"></a>Anpassen der Darstellung des Steuerelements für die Anmeldung

Standardeinstellungen für Eigenschaften des Steuerelements für die Anmeldung Rendern einer Benutzeroberfläche mit einem Titel (Anmelden), TextBox und Label-Steuerelemente für die Eingaben Benutzername und Kennwort, eine Anmeldedaten beim nächsten Mal aktiviert, und eine Schaltfläche "Anmelden". Die Darstellung eines dieser Elemente sind alle konfigurierbar über zahlreiche Eigenschaften des Login-Steuerelements. Darüber hinaus können zusätzliche Elemente der Benutzeroberfläche – wie z. B. einen Link zu einer Seite zum Erstellen eines neuen Benutzerkontos - durch Festlegen einer Eigenschaft oder zwei hinzugefügt werden.

Werfen wir kurz einen Augenblick, um die Darstellung von Steuerelement für die Anmeldung gussy. Da die `Login.aspx` Seite verfügt bereits über Text am oberen Rand der Seite, die besagt, Anmeldenamen dass, Titel des Steuerelements für die Anmeldung ist überflüssig. Löschen Sie daher, den [ `TitleText` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.titletext.aspx) Wert um den Titel des Steuerelements für die Anmeldung zu entfernen.

Der Benutzername: und Kennwort: Bezeichnungen auf der linken Seite des zwei TextBox-Steuerelemente können angepasst werden, über die [ `UserNameLabelText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) und [ `PasswordLabelText` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)bzw. Ändern Sie den Benutzernamen ein: Bezeichnung Username lesen:. Die Bezeichnung und einer TextBox-Stile sind konfigurierbar über die [ `LabelStyle` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.labelstyle.aspx) und [ `TextBoxStyle` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.textboxstyle.aspx)bzw.

Die Anmeldedaten weiter Zeit Kontrollkästchen Text-Eigenschaft werden, über die Anmelde-Steuerelement festgelegt kann [ `RememberMeText property` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.remembermetext.aspx), und der Standardwert überprüft Zustand ist konfigurierbar über die [ `RememberMeSet property` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.remembermeset.aspx) (die Der Standardwert ist "false"). Fahren Sie fort, und legen Sie die `RememberMeSet` Eigenschaft auf "true", damit die Anmeldedaten beim nächsten Mal Kontrollkästchen wird standardmäßig aktiviert sein.

Das Steuerelement für die Anmeldung bietet zwei Eigenschaften für das Layout der Steuerelemente der Benutzeroberfläche anpassen. Die [ `TextLayout property` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.textlayout.aspx) gibt an, ob der Benutzername: und Kennwort: Bezeichnungen auf der linken Seite die entsprechenden Textfelder ein (Standard) oder darüber liegenden angezeigt werden. Die [ `Orientation property` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.orientation.aspx) gibt an, ob der Benutzername und Kennwort Eingaben vertikal befinden (übereinander) oder horizontal. Ich werde diese beiden Eigenschaften auf ihre Standardwerte festgelegt zu lassen, jedoch sollten Sie versuchen, diese beiden Eigenschaften festlegen, auf die nicht standardmäßige Werte, die sich ergebende Auswirkung anzuzeigen.

> [!NOTE]
> Im nächsten Abschnitt betrachten konfigurieren Layout des Steuerelements für die Anmeldung, wir definieren Sie die präzise Layout der Elemente der Benutzeroberfläche des Layout-Steuerelements mithilfe von Vorlagen.


Wrappen von Einstellungen für die Anmeldung des Steuerelements durch Festlegen der [ `CreateUserText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.createusertext.aspx) und [ `CreateUserUrl` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.createuserurl.aspx) auf Not noch registriert? Erstellen Sie ein Konto! und `~/Membership/CreatingUserAccounts.aspx`zugeordnet. Dadurch wird die Anmeldung Schnittstelle des Steuerelements auf der Seite "wird, in erstellt einen Link hinzugefügt der <a id="SKM6"> </a> [vorherigen Lernprogramm](creating-user-accounts-cs.md). Die Anmeldung des Steuerelements [ `HelpPageText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.helppagetext.aspx) und [ `HelpPageUrl` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.helppageurl.aspx) und [ `PasswordRecoveryText` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) und [ `PasswordRecoveryUrl` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funktionieren in die gleiche Weise Rendern von Links zu einer Hilfeseite und eine Seite "Kennwort Recovery".

Nachdem diese eigenschaftenänderungen vorgenommen wurden, sollte deklarativem Markup und die Darstellung des Steuerelements Anmeldung, die in Abbildung 5 dargestellten ähneln.


[![Eigenschaften des Steuerelements für die Anmeldung festgelegt seine Darstellung](validating-user-credentials-against-the-membership-user-store-cs/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image13.png)

**Abbildung 5**: Eigenschaften des Login-Steuerelements Werte diktieren seine Darstellung ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurieren die Anmeldung das Steuerelementlayout

Die Login-Websteuerelement-Standardbenutzeroberfläche angeordnet die Schnittstelle in einem HTML- `<table>`. Aber was geschieht, wenn wir eine präzisere Steuerung der gerenderten Ausgabe benötigen? Vielleicht möchten wir ersetzen die `<table>` mit einer Reihe von `<div>` Tags. Oder was geschieht, wenn die Anwendung zusätzliche Anmeldeinformationen für die Authentifizierung benötigt? Viele finanzielle Websites erfordern z. B. Benutzer, nicht nur einen Benutzernamen und ein Kennwort, aber auch eine Anzahl PIN (Personal Identification), oder andere identifizierende Informationen einzugeben. Alle von Ihnen gewünschten Gründe sein können, ist es möglich, das Steuerelement für die Anmeldung zu einer Vorlage konvertiert aus dem wir explizit die Schnittstelle deklarativem Markup definieren können.

Wir benötigen zwei Aktionen zum Aktualisieren der Anmelde-Steuerelement, um zusätzliche Anmeldeinformationen zu sammeln:

1. Aktualisieren Sie die Anmeldung Schnittstelle des Steuerelements Einbeziehung von Web-Steuerelemente, um die zusätzlichen Anmeldeinformationen zu sammeln.
2. Überschreiben Sie die Anmeldung Steuerelementlogik interne Authentifizierung, sodass nur ein Benutzer authentifiziert wurde, ob Benutzername und Kennwort gültig ist und deren zusätzlichen Anmeldeinformationen ungültig sind.

Um die erste Aufgabe zu erreichen, müssen wir das Steuerelement für die Anmeldung in eine Vorlage konvertieren und die erforderlichen Web-Steuerelemente hinzufügen. Wie für die zweite Aufgabe kann Authentifizierungslogik des Login-Steuerelements durch Erstellen eines ereignishandlers für des Steuerelements abgelöst werden [ `Authenticate` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.authenticate.aspx).

Wir aktualisieren das Steuerelement für die Anmeldung an, damit Benutzer ihren Benutzernamen, Kennwort und e-Mail-Adresse aufgefordert und den Benutzer nur, authentifiziert Wenn die angegebene e-Mail-Adresse ihre e-Mail-Adresse übereinstimmt. Wir müssen zunächst die Login-Schnittstelle in einer Vorlage zu konvertieren. Smarttag für die Anmeldung des Steuerelements wählen Sie in Vorlagenoption konvertieren.


[![Das Steuerelement für die Anmeldung in eine Vorlage konvertieren](validating-user-credentials-against-the-membership-user-store-cs/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image16.png)

**Abbildung 6**: das Steuerelement für die Anmeldung in eine Vorlage konvertieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image18.png))


> [!NOTE]
> Um das Steuerelement Anmeldung mit ihrer vorab template Version wiederherzustellen, klicken Sie auf die Verknüpfung zurücksetzen Smarttag des Steuerelements.


Fügt das Steuerelement für die Anmeldung in eine Vorlage konvertieren einer `LayoutTemplate` , deklaratives Markup mit HTML-Elemente und Websteuerelemente Definieren der Benutzeroberfläche des Steuerelements. Wie Abbildung 7 zeigt ein Beispiel, das Steuerelement in eine Vorlage konvertieren entfernt eine Reihe von Eigenschaften über das Eigenschaftenfenster, wie z. B. `TitleText`, `CreateUserUrl`usw., da diese Eigenschaftswerte ignoriert werden, wenn eine Vorlage verwenden.


[![Weniger Eigenschaften sind verfügbar bei der Anmeldung Steuerung in einer Vorlage konvertiert wird](validating-user-credentials-against-the-membership-user-store-cs/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image19.png)

**Abbildung 7**: weniger Eigenschaften sind verfügbar bei der Anmeldung Steuerung in eine Vorlage konvertiert wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image21.png))


Das HTML-Markup in der `LayoutTemplate` kann nach Bedarf geändert werden. Ebenso können Sie die Vorlage keine neuen Websteuerelemente hinzufügen. Allerdings ist es wichtig, Anmeldung des Steuerelements den Core-Websteuerelemente bleiben in der Vorlage, und behalten ihre zugewiesenen `ID` Werte. Insbesondere nicht entfernen oder Umbenennen der `UserName` oder `Password` Textfelder ein, die `RememberMe` Kontrollkästchen deaktivieren, können die `LoginButton` Schaltfläche, die `FailureText` Bezeichnung, oder die `RequiredFieldValidator` Steuerelemente.

Um die e-Mail-Adresse des Besuchers zu sammeln, müssen wir ein Textfeld zur Vorlage hinzufügen. Fügen Sie das folgende deklarative Markup zwischen der Tabellenzeile (`<tr>`), enthält die `Password` Textfeld und die Tabellenzeile, die die Anmeldedaten neben enthält den Zeitpunkt Kontrollkästchen:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample2.aspx)]

Nach dem Hinzufügen der `Email` TextBox, besuchen Sie die Seite über einen Browser. Wie in Abbildung 8 gezeigt, enthält die Benutzeroberfläche des Steuerelements für die Anmeldung nun eine dritte Textfeld.


[![Das Steuerelement für die Anmeldung enthält nun ein Textfeld für die e-Mail-Adresse des Benutzers](validating-user-credentials-against-the-membership-user-store-cs/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image22.png)

**Abbildung 8**: das Steuerelement für die Anmeldung enthält jetzt ein Textfeld für die e-Mail-Adresse des Benutzers ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image24.png))


Das Steuerelement für die Anmeldung wird an diesem Punkt noch verwenden die `Membership.ValidateUser` Methode, um die bereitgestellten Anmeldeinformationen zu überprüfen. Entsprechend der eingegebene Wert in der `Email` Textfeld hat keinen Einfluss auf, ob der Benutzer anmelden kann. In Schritt 3 betrachten wir das Authentifizierungslogik des Login-Steuerelements überschreiben, damit die Anmeldeinformationen dann sind nur gültig ist, wenn Benutzername und Kennwort gültig sind und die angegebene e-Mail-Adresse, mit der e-Mail-Adresse auf Datei eingerichtet übereinstimmt.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Schritt 3: Ändern des Login-Steuerelements Authentifizierungslogik

Wenn ein Anna Besucher steuern Anmeldeinformationen und klickt, die die Schaltfläche "Anmelden", was zu einem Postback erfolgt und die Anmeldung durchläuft der authentifizierungsworkflow. Der Workflow gestartet wird, indem Sie durch das Auslösen der [ `LoggingIn` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loggingin.aspx). Alle diesem Ereignis zugeordnete Ereignishandler können das Protokoll in Vorgang abbrechen, indem die `e.Cancel` Eigenschaft `true`.

Wenn das Protokoll in der Vorgang nicht abgebrochen wird, wechselt der Workflow durch Auslösen der [ `Authenticate` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.authenticate.aspx). Es ist ein Ereignishandler für das `Authenticate` Ereignis, dann werden ist zuständig für das ermitteln, ob die angegebenen Anmeldeinformationen gültig sind. Wenn kein Ereignishandler angegeben wird, wird das Steuerelement für die Anmeldung verwendet die `Membership.ValidateUser` Methode, um die Gültigkeit der Anmeldeinformationen festzulegen.

Wenn die angegebenen Anmeldeinformationen gültig sind, und klicken Sie dann das Formularauthentifizierungsticket erstellt wird, die [ `LoggedIn` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loggedin.aspx) ausgelöst wird, und der Benutzer an die entsprechende Seite umgeleitet werden. Wenn Sie jedoch die Anmeldeinformationen als ungültig eingestuft werden und dann die [ `LoginError` Ereignis](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.loginerror.aspx) wird ausgelöst, und eine Meldung angezeigt wird, den Benutzer darüber informiert, dass ihre Anmeldeinformationen ungültig sind. Standardmäßig bei der Anmeldung Steuerelement einfach legt seine `FailureText` bezeichnen Text-Eigenschaft des Steuerelements, um eine Fehlermeldung (die Anmeldung war nicht erfolgreich. Versuchen Sie es). Jedoch wenn des Login-Steuerelements [ `FailureAction` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.failureaction.aspx) auf festgelegt ist `RedirectToLoginPage`, klicken Sie dann die Anmeldung Steuerelement Probleme eine `Response.Redirect` zur Anmeldeseite Anhängen des Querystring-Parameters `loginfailure=1` (Dadurch wird die Anmeldung Steuerelement, das die Fehlermeldung anzuzeigen.)

Abbildung 9 bietet es sich um ein Flussdiagramm des Workflows Authentifizierung.


[![Die Anmeldung des Steuerelements Authentifizierungsworkflow](validating-user-credentials-against-the-membership-user-store-cs/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image25.png)

**Abbildung 9**: Authentifizierungsworkflow der Login-Steuerung ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image27.png))


> [!NOTE]
> Wenn Sie wissen möchten, wenn Sie verwenden würden die `FailureAction`des `RedirectToLogin` Seite die Option, sollten Sie das folgende Szenario. Jetzt unsere `Site.master` Masterseite hat derzeit den Text Hello, in der linken Spalte durch einen anonymen Benutzer besucht stranger angezeigt, aber angenommen, wir, ersetzen Sie diesen Text mit einem Anmeldungssteuerelement möchten. Dies würde es sich um einen anonymen Benutzer aus einer beliebigen Seite auf der Website, anstatt sie direkt die Login-Seite besuchen Anmelden ermöglichen. Jedoch wenn ein Benutzer nicht anmelden über das Steuerelement für die Anmeldung durch die Masterseite gerendert wurde, es möglicherweise sinnvoll, sie zur Anmeldeseite umgeleitet (`Login.aspx`) da diese Seite, die wahrscheinlich zusätzliche Anweisungen, Links und anderen Hilfe - z. B. Links zu erstellen enthält ein Neues Konto oder Abrufen eines verlorenen Kennworts -, die nicht der Masterseite hinzugefügt wurden.


### <a name="creating-theauthenticateevent-handler"></a>Erstellen der`Authenticate`-Ereignishandler

Um unsere benutzerdefinierte Authentifizierungslogik einbinden, müssen wir erstellen Sie einen Ereignishandler für die Anmeldung des Steuerelements `Authenticate` Ereignis. Erstellen einen Ereignishandler für das `Authenticate` Ereignis wird die folgende Definition der Ereignis-Handler generiert:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample3.cs)]

Wie Sie sehen können, die `Authenticate` übergebene Ereignishandler wird ein Objekt des Typs [ `AuthenticateEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.authenticateeventargs.aspx) als der zweite Eingabeparameter. Die `AuthenticateEventArgs` Klasse enthält eine boolesche Eigenschaft namens `Authenticated` , verwendet, um anzugeben, ob die angegebenen Anmeldeinformationen gültig sind. Unsere Aufgabe ist dann hier Code schreiben, der bestimmt, ob die angegebenen Anmeldeinformationen gültig sind, und Festlegen der `e.Authenticate` Eigenschaft entsprechend.

### <a name="determining-and-validating-the-supplied-credentials"></a>Bestimmen und die angegebenen Anmeldeinformationen überprüft.

Verwenden Sie das Anmelde-Steuerelement [ `UserName` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.username.aspx) und [ `Password` Eigenschaften](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.password.aspx) um zu bestimmen, die vom Benutzer eingegebenen Benutzernamens und Kennworts-Anmeldeinformationen. Um die Werte in jeder zusätzlichen Websteuerelemente eingegeben bestimmen (wie z. B. die `Email` Textfeld wir im vorherigen Schritt hinzugefügt haben), verwenden Sie  *`LoginControlID`*  `.FindControl`(" *`controlID`* ") zum Abrufen eines programmgesteuerten Verweis auf das Websteuerelement in der Vorlage, deren `ID` -Eigenschaft gleich  *`controlID`* . Beispielsweise, um das Abrufen eines Verweises auf die `Email` TextBox, verwenden Sie den folgenden Code:

`TextBox EmailTextBox = myLogin.FindControl("Email") as TextBox;`

Um die Anmeldeinformationen des Benutzers zu überprüfen, müssen wir zwei Dinge tun:

1. Stellen Sie sicher, dass der bereitgestellte Benutzername und das Kennwort gültig sind.
2. Stellen Sie sicher, dass die eingegebene e-Mail-Adresse entspricht, die e-Mail-Adresse für die Datei für den Benutzer, bei dem Versuch, melden Sie sich

Um bei der ersten Validierung zu erreichen, wir einfach können, die `Membership.ValidateUser` Methode wie in Schritt 1 veranschaulichte Filtermenü. Für die zweite Überprüfung muss die e-Mail-Adresse des Benutzers zu ermitteln, sodass wir ihn an die e-Mail-Adresse vergleichen können, die sie in das Textfeld-Steuerelement eingegeben werden. Verwenden Sie zum Abrufen von Informationen zu einem bestimmten Benutzer die `Membership` Klasse [ `GetUser` Methode](https://msdn.microsoft.com/en-us/library/system.web.security.membership.getuser.aspx).

Die `GetUser` Methode bietet eine Reihe von Überladungen. Wenn ohne Übergabe von Parametern verwendet wird, wird die Informationen über den aktuell angemeldeten Benutzer zurückgegeben. Rufen Sie zum Abrufen von Informationen zu einem bestimmten Benutzer `GetUser` in ihren Benutzernamen übergeben. In beiden Fällen `GetUser` gibt eine [ `MembershipUser` Objekt](https://msdn.microsoft.com/en-us/library/system.web.security.membershipuser.aspx), dem verfügt über Eigenschaften, z. B. `UserName`, `Email`, `IsApproved`, `IsOnline`und so weiter.

Der folgende Code implementiert diese zwei Überprüfungen. Wenn beide, klicken Sie dann übergeben `e.Authenticate` festgelegt ist, um `true`, andernfalls ist er zugewiesen `false`.

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample4.cs)]

Versuchen Sie mit diesem Code werden, melden Sie sich als gültiger Benutzer der richtige Benutzername, Kennwort und e-Mail-Adresse eingeben. Wiederholen sie den Vorgang, aber dieses Mal wird absichtlich eine falsche e-Mail-Adresse verwenden (siehe Abbildung 10). Schließlich weiterführende ein drittes Mal einen nicht vorhandenen Benutzernamen verwenden. Im ersten Fall Sie sollten erfolgreich angemeldet sein, den Standort, aber in den letzten beiden Fällen sollte die Anmeldung Kontrollnachricht ungültige Anmeldeinformationen.


[![Tito kann nicht anmelden, wenn Sie eine falsche e-Mail-Adresse angeben.](validating-user-credentials-against-the-membership-user-store-cs/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image28.png)

**Abbildung 10**: Tito kann nicht Protokoll In bei der Angabe einer falschen e-Mail-Adresse ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image30.png))


> [!NOTE]
> Wie beschrieben im Abschnitt wie die Mitgliedschaft Framework verarbeitet ungültige Anmeldeversuche in Schritt 1, wenn die `Membership.ValidateUser` Methode wird aufgerufen, und übergeben Sie ungültige Anmeldeinformationen, er verfolgt die Ungültiger Versuch der Anmeldung und sperrt der Benutzer, wenn sie eine bestimmte überschreiten Schwellenwert für die ungültige Versuche innerhalb eines angegebenen Zeitfensters. Da unsere benutzerdefinierte Logik authentifizierungsaufrufe der `ValidateUser` -Methode, ein falsches Kennwort für einen gültigen Benutzernamen den Ungültige Anmeldung Zähler erhöht, aber wird nicht erhöht, in dem Fall, in dem der Benutzername und Kennwort gültig sind, sind, aber die e-Mail-Adresse ist falsch. Wahrscheinlich sind, dieses Verhalten geeignet ist, ist, da es ist unwahrscheinlich, dass ein Hacker, Benutzername und Kennwort kennen, jedoch Brute-Force-Techniken verwenden, um die e-Mail-Adresse des Benutzers zu bestimmen.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Schritt 4: Verbessern der Anmeldung Kontrollnachricht ungültige Anmeldeinformationen

Wenn ein Benutzer versucht, sich mit ungültigen Anmeldeinformationen anmelden, zeigt das Steuerelement für die Anmeldung eine Meldung angezeigt, dass der Versuch der Anmeldung nicht erfolgreich war. Insbesondere zeigt das Steuerelement an die Nachricht gemäß seiner [ `FailureText` Eigenschaft](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.failuretext.aspx), die den Standardwert der Anmeldeversuch hat war nicht erfolgreich. Versuchen Sie es erneut.

Beachten Sie, dass es gibt viele Gründe, warum die Anmeldeinformationen des Benutzers ungültig werden kann:

- Der Benutzername ist nicht vorhanden.
- Der Benutzername vorhanden ist, aber das Kennwort ist ungültig
- Benutzername und Kennwort gültig sind, aber der Benutzer ist noch nicht genehmigt.
- Benutzername und Kennwort gültig sind, aber der Benutzer gesperrt Out (sehr wahrscheinlich, da sie die Anzahl von ungültigen Anmeldeversuchen innerhalb des angegebenen Zeitrahmens überschreiten)

Und bei Verwendung von benutzerdefinierten Authentifizierungslogik andere Gründe vorliegen kann. Durch den Code geschrieben wir haben in Schritt 3, den Benutzernamen und Kennwort ist möglicherweise ungültig, aber die e-Mail-Adresse ist möglicherweise nicht korrekt.

Unabhängig davon, warum die Anmeldeinformationen ungültig sind zeigt das Steuerelement für die Anmeldung an die gleiche Fehlermeldung. Diese mangelnde Feedback kann für einen Benutzer verwirrend sein, dessen Konto noch nicht genehmigt wurden, oder, die gesperrt ist. Mit ein wenig Arbeit jedoch können wir das Steuerelement für die Anmeldung eine besser geeignete Meldung anzeigen haben.

Wenn ein Benutzer versucht, mit ungültigen Anmeldeinformationen anmelden, löst das Steuerelement für die Anmeldung aus, seiner `LoginError` Ereignis. Fahren Sie fort und erstellen Sie einen Ereignishandler für dieses Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](validating-user-credentials-against-the-membership-user-store-cs/samples/sample5.cs)]

Der obige Code beginnt mit dem Festlegen des Login-Steuerelements `FailureText` Eigenschaft auf den Standardwert (Ihre Anmeldung war nicht erfolgreich. Versuchen Sie es). Er überprüft dann um festzustellen, ob der angegebene Benutzername einem vorhandenen Benutzerkonto zugeordnet ist. Wenn also er das resultierende fragt `MembershipUser` des Objekts `IsLockedOut` und `IsApproved` Eigenschaften zu bestimmen, ob das Konto ist gesperrt oder noch nicht genehmigt wurden. In beiden Fällen die `FailureText` Eigenschaft in einen entsprechenden Wert aktualisiert wird.

Um diesen Code zu testen, absichtlich versuchen Sie, melden Sie sich als einen vorhandenen Benutzer, aber verwenden ein falsches Kennwort. Führen Sie diesem fünfmal in einer Zeile innerhalb eines Zeitraums von 10 Minuten, und das Konto gesperrt. Abbildung 11 zeigt, nachfolgende Anmeldung versucht immer fehl (selbst bei das richtige Kennwort), aber die jetzt den aussagekräftigeren angezeigt wurde Ihr Konto aufgrund zu vieler ungültiger Anmeldeversuche gesperrt wurde. Wenden Sie sich an den Administrator, um Ihr Konto entsperrt Nachricht haben.


[![Tito zu viele ungültige Anmeldeversuche ausgeführt und wurde gesperrt](validating-user-credentials-against-the-membership-user-store-cs/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-cs/_static/image31.png)

**Abbildung 11**: Tito ausgeführt zu viele ungültige Anmeldeversuche und verfügt über wurde gesperrt ([klicken Sie hier, um das Bild in voller Größe angezeigt](validating-user-credentials-against-the-membership-user-store-cs/_static/image33.png))


## <a name="summary"></a>Zusammenfassung

Vor diesem Lernprogramm die Anmeldeseite überprüft die angegebenen Anmeldeinformationen für eine vorprogrammierte Liste von Benutzername/Kennwort-Paare. In diesem Lernprogramm aktualisiert wird die Seite, um die Anmeldeinformationen für das Framework Mitgliedschaft zu überprüfen. In Schritt 1 erläutert mithilfe der `Membership.ValidateUser` -Methode programmgesteuert. In Schritt2 werden unsere manuell erstellte Benutzeroberfläche und den Code durch das Steuerelement für die Anmeldung ersetzt.

Das Steuerelement für die Anmeldung rendert eine standard-Login-Benutzeroberfläche und Anmeldeinformationen für das Framework für die Mitgliedschaft des Benutzers automatisch überprüft. Darüber hinaus meldet bei gültige Anmeldeinformationen das Steuerelement für die Anmeldung den Benutzer sich über formularbasierte Authentifizierung. Kurz gesagt, ist eine voll funktionsfähige Anmeldeverfahren für Benutzer verfügbar, durch Ziehen einfach das Steuerelement für die Anmeldung auf einer Seite, keine zusätzlichen deklarativem Markup oder Code, der erforderlich. Was mehr ist, ist das Steuerelement für die Anmeldung bei dem ein gut Maß an Kontrolle über die gerenderten Benutzer Schnittstelle und die Authentifizierung der Logik hochgradig anpassbar.

An diesem Punkt Besucher auf unserer Website können erstellt ein neues Benutzerkonto und ein Protokoll bei der Website, jedoch können wir noch Uhrzeitstempel Einschränken des Zugriffs auf Seiten auf Grundlage des authentifizierten Benutzers. Derzeit kann keinem Benutzer authentifizierte oder anonyme, eine andere Seite auf unserer Website anzeigen. Zusammen mit Steuern des Zugriffs auf unserer Website Seiten regelmäßig durch Benutzer, wir bestimmte Seiten möglicherweise der Benutzer, dessen Funktionalität abhängig. Die nächste Lernprogramm befasst sich mit Zugriff und die Funktionen von in-Seite basierend auf dem angemeldeten Benutzer zu beschränken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Anzeigen von benutzerdefinierten Nachrichten gesperrt und nicht genehmigte Benutzer](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Gewusst wie: Erstellen eine Anmeldung ASP.NET-Seite](https://msdn.microsoft.com/en-us/library/ms178331.aspx)
- [Technische Dokumentation zu Anmelde-Steuerelement](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.login.aspx)
- [Mithilfe dieser Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurden Teresa Murphy und Michael Olivero. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Zurück](creating-user-accounts-cs.md)
[Weiter](user-based-authorization-cs.md)
