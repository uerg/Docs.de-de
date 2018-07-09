---
uid: web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
title: Überprüfen der Anmeldeinformationen anhand der Mitgliedschaft Benutzer Store (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial untersuchen wir die Anmeldeinformationen eines Benutzers anhand des mitgliedschaftsbenutzerspeichers mit dem Login-Steuerelement und programmgesteuerte Möglichkeit zu überprüfen...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: 17772912-b47b-4557-9ce9-80f22df642f7
msc.legacyurl: /web-forms/overview/older-versions-security/membership/validating-user-credentials-against-the-membership-user-store-vb
msc.type: authoredcontent
ms.openlocfilehash: c5eeb67c8d175173f38ffcbc1b01fd5a5931866e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821403"
---
<a name="validating-user-credentials-against-the-membership-user-store-vb"></a>Überprüfen der Anmeldeinformationen anhand der Mitgliedschaft Benutzer Store (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_06_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial06_LoggingIn_vb.pdf)

> In diesem Tutorial untersuchen wir die Anmeldeinformationen eines Benutzers anhand des mitgliedschaftsbenutzerspeichers mit dem Login-Steuerelement und programmgesteuerte Möglichkeit zu überprüfen. Wir sehen auch an, wie des Anmeldesteuerelement Aussehen und Verhalten anpassen.


## <a name="introduction"></a>Einführung

In der <a id="Tutorial05"> </a> [vorherigen Lernprogramm](creating-user-accounts-vb.md) erläutert, wie Sie ein neues Benutzerkonto in das mitgliedschaftsframework zu erstellen. Programmgesteuertes Erstellen von Benutzerkonten über zunächst wurde die `Membership` -Klasse `CreateUser` -Methode, und klicken Sie dann untersucht das Steuerelement CreateUserWizard verwenden. Die Anmeldeseite wird jedoch derzeit die angegebenen Anmeldeinformationen für eine hartcodierte Liste der Paare aus Benutzername und Kennwort überprüft. Wir müssen Logik für die Anmeldeseite zu aktualisieren, sodass sie die Anmeldeinformationen für das mitgliedschaftsframework Benutzerspeicher überprüft.

Viel können z. B. mit dem Erstellen von Benutzerkonten die, Anmeldeinformationen programmgesteuert oder deklarativ überprüft werden. Membership-API enthält eine Methode zum programmgesteuerten Überprüfung der Anmeldeinformationen eines Benutzers für den Speicher des Benutzers. Und ASP.NET im Lieferumfang von des Websteuerelements Anmeldung, das eine Benutzeroberfläche mit Textfelder für den Benutzernamen und Kennwort und eine Schaltfläche für die Anmeldung rendert.

In diesem Tutorial untersuchen wir die Anmeldeinformationen eines Benutzers anhand des mitgliedschaftsbenutzerspeichers mit dem Login-Steuerelement und programmgesteuerte Möglichkeit zu überprüfen. Wir sehen auch an, wie des Anmeldesteuerelement Aussehen und Verhalten anpassen. Fangen wir an!

## <a name="step-1-validating-credentials-against-the-membership-user-store"></a>Schritt 1: Überprüfen der Anmeldeinformationen für die Mitgliedschaft Benutzer Store

Für Websites, die die Formularauthentifizierung verwenden, meldet sich der Benutzer auf der Website durch Zugriff auf eine Anmeldeseite und die Anmeldeinformationen einzugeben. Diese Anmeldeinformationen werden dann mit den Speicher des Benutzers verglichen. Wenn sie gültig sind, dann erhält der Benutzer ein Formularauthentifizierungsticket, d. h. ein Sicherheitstoken, die Identität und die Authentizität des Besuchers angibt.

Verwenden Sie zum Überprüfen eines Benutzers für das mitgliedschaftsframework der `Membership` Klasse [ `ValidateUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx). Die `ValidateUser` Methode nimmt zwei Eingabeparameter - `username` und `password` - und gibt einen booleschen Wert, der angibt, ob die Anmeldeinformationen gültig sind. Wie Sie mit der `CreateUser` Methode, die wir im vorherigen Tutorial untersucht die `ValidateUser` Methode delegiert die eigentliche Validierung an den konfigurierten Mitgliedschaftsanbieter.

Die `SqlMembershipProvider` überprüft die angegebenen Anmeldeinformationen durch Abrufen des angegebenen Benutzers Kennwort über die `aspnet_Membership_GetPasswordWithFormat` gespeicherte Prozedur. Bedenken Sie, dass die `SqlMembershipProvider` speichert Kennwörter für Benutzer mit einer der drei Formaten: löschen, die verschlüsselt oder Hash. Die `aspnet_Membership_GetPasswordWithFormat` gespeicherte Prozedur gibt das Kennwort in ihrem Rohformat zurück. Für verschlüsselte oder verschlüsselte Kennwörter die `SqlMembershipProvider` transformiert die `password` übergebenen Wert die `ValidateUser` Methode in die entsprechende verschlüsselt oder Status gehasht und verglichen, was aus der Datenbank zurückgegeben wurde. Wenn das Kennwort in der Datenbank gespeichert, das formatierte, vom Benutzer eingegebene Kennwort entspricht, sind die Anmeldeinformationen gültig.

Aktualisieren wir unsere Anmeldeseite (~ /`Login.aspx`), damit es überprüft, ob die angegebenen Anmeldeinformationen anhand des mitgliedschaftsbenutzerspeichers für Framework. Wir diese Anmeldeseite erstellt in der <a id="Tutorial02"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Tutorial erstellen eine Schnittstelle mit zwei Textfelder für den Benutzernamen und das Kennwort ein Anmeldedaten speichern aktiviert, und eine Schaltfläche "Anmelden" (siehe Abbildung 1). Der Code überprüft, ob die eingegebenen Anmeldeinformationen für eine hartcodierte Liste von Benutzername und Kennwort-Paaren (Scott/Kennwort, Jisun/Kennwort und Sam und Kennwort). In der <a id="Tutorial03"> </a> [ *Konfiguration der Formularauthentifizierung und Weiterführende Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Tutorial wir die Anmeldeseite-Code zum Speichern zusätzlicher Informationen in den Versionen aktualisiert die Authentifizierungsticket `UserData` Eigenschaft.


[![Die Anmeldeseite Schnittstelle enthält zwei Textfelder, einer "CheckBoxList" und eine Schaltfläche](validating-user-credentials-against-the-membership-user-store-vb/_static/image2.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image1.png)

**Abbildung 1**: die Anmeldeseites Schnittstelle enthält zwei Textfelder, einer "CheckBoxList" und eine Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image3.png))


Benutzeroberfläche für die Anmeldeseite kann unverändert bleiben, aber wir müssen der Login-Schaltfläche ersetzen `Click` -Ereignishandler durch Code, der den Benutzer anhand des mitgliedschaftsbenutzerspeichers für Framework überprüft. Aktualisieren Sie den Ereignishandler, sodass der Code wie folgt aussieht:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample1.vb)]

Dieser Code ist erstaunlich einfach. Wir rufen Sie zunächst die `Membership.ValidateUser` Methode auf und übergibt den angegebenen Benutzernamen und das Kennwort. Wenn diese Methode gibt "true" zurück, der Benutzer angemeldet ist, auf der Website über die `FormsAuthentication` RedirectFromLoginPage-Methode der-Klasse. (Wie in erläutert die <a id="Tutorial02"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Tutorial die `FormsAuthentication.RedirectFromLoginPage` erstellt die Forms-Authentifizierungsticket, und klicken Sie dann leitet den Benutzer die entsprechende Seite.) Wenn jedoch die Anmeldeinformationen ungültig ist, sind die `InvalidCredentialsMessage` Bezeichnung wird angezeigt, den Benutzer darüber informiert, dass die Benutzernamen oder das Kennwort falsch war.

Das ist alles schon!

Um zu testen, dass die Anmeldeseite wie erwartet funktioniert, versuchen Sie mit einem der Benutzerkonten anmelden, die Sie im vorherigen Tutorial erstellt haben. Oder, wenn Sie ein Konto noch nicht erstellt haben, fahren Sie fort, und erstellen Sie eine der `~/Membership/CreatingUserAccounts.aspx` Seite.

> [!NOTE]
> Wenn der Benutzer ihre Anmeldeinformationen eingegeben und das Anmeldeformular für die Seite übermittelt, werden die Anmeldeinformationen, einschließlich Kennwort, über das Internet an den Webserver in übertragen *nur-Text*. Das bedeutet, dass alle Hacker man den Netzwerkverkehr des Benutzernamens und Kennworts angezeigt. Um dies zu verhindern, ist es erforderlich, den Netzwerkdatenverkehr mit verschlüsseln [Secure Socket Layer (SSL)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Dadurch wird sichergestellt, dass die Anmeldeinformationen (sowie die gesamte Seite HTML-Markup) ab dem Zeitpunkt verschlüsselt werden, die sie den Browser verlassen, bis sie vom Webserver empfangen werden.


### <a name="how-the-membership-framework-handles-invalid-login-attempts"></a>Behandlung von ungültigen Anmeldeversuchen durch das Mitgliedschaftsframework

Wenn ein Besucher die Anmeldeseite erreicht und seine Anmeldeinformationen übermittelt, bewirkt, dass ihre Browser eine HTTP-Anforderung an die Anmeldeseite. Wenn die Anmeldeinformationen gültig sind, enthält die HTTP-Antwort das Authentifizierungsticket in einem Cookie. Aus diesem Grund könnte ein Hacker versucht, die an Ihrem Standort zu unterbrechen, ein Programm erstellen, die HTTP-Anforderungen umfassend auf die Anmeldeseite mit einem gültigen Benutzernamen und ein Vermutung in Bezug auf das Kennwort sendet. Wenn der Schätzwert Kennwort richtig ist, gibt die Anmeldeseite das Authentifizierungscookie Ticket und zurück zu diesem, das Zeitpunkt weiß das Programm, dass er auf eine gültige Benutzername/Kennwort-Paar gestolpert ist. Durch brute-Force einem solchen Programm Probleme stoßen, auf das Kennwort eines Benutzers, möglicherweise insbesondere dann, wenn das Kennwort schwach ist.

Um solche brute-Force-Angriffe zu verhindern, sperrt das mitgliedschaftsframework ein Benutzer ein, wenn eine bestimmte Anzahl nicht erfolgreicher Anmeldeversuche innerhalb eines bestimmten Zeitraums Zeit vorhanden sind. Die genauen Parameter, die über die folgenden zwei Membership-Provider-Konfigurationseinstellungen konfiguriert werden:

- `maxInvalidPasswordAttempts` -Gibt an, wie viele wurde ein ungültiges Kennwort Verbindungsversuche sind zulässig, für den Benutzer innerhalb des Zeitraums, bevor das Konto gesperrt wird. Der Standardwert ist 5.
- `passwordAttemptWindow` -Gibt den Zeitraum in Minuten, in dem die angegebene Anzahl von ungültigen Anmeldeversuchen führt dazu, dass das Konto gesperrt wird. Der Standardwert ist 10.

Wenn ein Benutzer gesperrt wurde, kann nicht sie sich erst anmelden, ein Administrator ihr Konto entsperrt. Wenn ein Benutzer gesperrt ist die `ValidateUser` Methode *immer* zurückgeben `False`, auch wenn gültige Anmeldeinformationen angegeben werden. Während dadurch die Wahrscheinlichkeit, die ein Hacker durch brute-Force-Methoden in Ihre Website unterbrochen wird verringert, können sie schließlich Sperrung ein gültiger Benutzer, die einfach ihr Kennwort vergessen hat oder versehentlich besitzt die FESTSTELLTASTE auf oder mit unglückstag eingeben.

Leider ist es kein integriertes Tool für ein Benutzerkonto zu entsperren. Um ein Konto zu entsperren, können Sie die Datenbank ändern direkt – ändern der `IsLockedOut` -Feld in der `aspnet_Membership` Tabelle aus, für das entsprechende Benutzerkonto - oder eine webbasierte Schnittstelle erstellen, die listet gesperrte Konten mit Optionen, um sie zu entsperren. Untersuchen wir erstellen administratoroberflächen für häufig verwendete Benutzeraufgaben Konto und rollenbezogenen in einem späteren Tutorial ausführen.

> [!NOTE]
> Ein Nachteil der `ValidateUser` Methode ist, wenn die angegebenen Anmeldeinformationen ungültig sind, keine Erklärung, warum bereitgestellt wird. Die Anmeldeinformationen möglicherweise ungültig, da keine entsprechenden Benutzernamen/Kennwort-Paar in den Speicher des Benutzers vorhanden ist oder da der Benutzer noch nicht genehmigt wurde oder weil der Benutzer gesperrt wurde. In Schritt 4 sehen wir, wie Sie eine weitere ausführliche Meldung für dem Benutzer anzeigen, wenn ihre Anmeldeversuch fehlschlägt.


## <a name="step-2-collecting-credentials-through-the-login-web-control"></a>Schritt 2: Sammeln von Anmeldeinformationen über das Websteuerelement für Login

Die [Anmeldung Websteuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx) rendert eine Standard-Benutzeroberfläche sehr ähnlich, die wir erstellt, wieder in haben die <a id="Tutorial02"> </a> [ *eine Übersicht der Formularauthentifizierung* ](../introduction/an-overview-of-forms-authentication-vb.md) Tutorial. Mit dem Login-Steuerelement hilft uns, die Arbeit, die Schnittstelle zum Erfassen des Besuchers Anmeldeinformationen erstellen zu müssen. Darüber hinaus das Steuerelement für die Anmeldung automatisch angemeldet des Benutzers (vorausgesetzt, dass die übermittelten Anmeldeinformationen gültig sind), und speichern wir keinen Code schreiben müssen.

Aktualisieren wir `Login.aspx`, und Ersetzen Sie dabei den manuell erstellte Schnittstelle und code mit einem Login-Steuerelement. Entfernen Sie zuerst das vorhandene Markup und code im `Login.aspx`. Sie können es 30-tägiges löschen oder kommentieren Sie ihn aus. Um deklaratives Markup auskommentieren, setzen Sie sie mit der `<%--` und `--%>` Trennzeichen. Sie können diese Trennzeichen manuell eingeben oder, wie in Abbildung 2 gezeigt, können Sie den Text kommentieren Sie Sie aus, und klicken Sie auf den Kommentar aus der ausgewählten Zeilen auf der Symbolleiste auswählen. Auf ähnliche Weise können Sie den Kommentar aus der ausgewählten Zeilen auskommentieren, den ausgewählten Code in der CodeBehind-Klasse.


[![Kommentieren Sie die vorhandenen deklaratives Markup und Quellcode in "Login.aspx"](validating-user-credentials-against-the-membership-user-store-vb/_static/image5.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image4.png)

**Abbildung 2**: Kommentar, der vorhandenen deklaratives Markup und Quellcode in "Login.aspx" ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image6.png))


> [!NOTE]
> Der Kommentar aus der ausgewählten Zeilen ist nicht verfügbar, beim Anzeigen von deklarativen Markups in Visual Studio 2005. Wenn Sie nicht Visual Studio 2008 verwenden, werden Sie manuell hinzufügen müssen die `<%--` und `--%>` Trennzeichen.


Als Nächstes ein Anmeldesteuerelement aus der Toolbox auf die Seite ziehen, und legen Sie dessen `ID` Eigenschaft `myLogin`. An diesem Punkt sollte Ihr Bildschirm wie in Abbildung 3 aussehen. Beachten Sie, dass das Anmeldesteuerelement Standardschnittstelle TextBox-Steuerelemente für den Benutzernamen und Kennwort, eine Anmeldedaten Kontrollkästchen und eine Schaltfläche "Protokoll In" enthält. Es gibt auch `RequiredFieldValidator` Steuerelemente für die beiden Textfelder ein.


[![Fügen Sie auf der Seite ein Anmeldesteuerelement hinzu](validating-user-credentials-against-the-membership-user-store-vb/_static/image8.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image7.png)

**Abbildung 3**: Fügen Sie auf der Seite ein Anmeldesteuerelement hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image9.png))


Und wir sind fertig! Wenn dem Login-Steuerelement die Schaltfläche "Anmelden" geklickt wird, wird ein Postback tritt auf, und das Login-Steuerelement ruft die `Membership.ValidateUser` Methode und übergeben des eingegebenen Benutzernamens und Kennworts. Wenn die Anmeldeinformationen ungültig sind, zeigt das Steuerelement für die Anmeldung eine Meldung angezeigt, z. B. an. Wenn Sie jedoch die Anmeldeinformationen gültig sind, klicken Sie dann die Login-Steuerelement erstellt die Forms-Authentifizierungsticket und leitet den Benutzer an die entsprechende Seite.

Das Steuerelement für die Anmeldung verwendet vier Faktoren, um zu bestimmen, die entsprechende Seite zum Umleiten des Benutzers, nach einer erfolgreichen Anmeldung:

- Gibt an, ob das Steuerelement für die Anmeldung auf der Anmeldeseite wird gemäß der `loginUrl` ist der Standardwert für diese Einstellung, die in die Konfiguration der Formularauthentifizierung; festlegen `Login.aspx`
- Das Vorhandensein einer `ReturnUrl` Querystring-Parameter
- Der Wert, der des Anmeldesteuerelement [ `DestinationUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.destinationpageurl.aspx)
- Die `defaultUrl` Wert angegeben, in den Formen Konfigurationseinstellungen für die Authentifizierung ist diese Einstellung den Standardwert "default.aspx"

Abbildung 4 zeigt, wie das Steuerelement für die Anmeldung verwendet diese vier Parameter, um ihre Entscheidung für die entsprechende Seite zu erreichen.


[![Fügen Sie auf der Seite ein Anmeldesteuerelement hinzu](validating-user-credentials-against-the-membership-user-store-vb/_static/image11.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image10.png)

**Abbildung 4**: Fügen Sie auf der Seite ein Anmeldesteuerelement hinzu ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image12.png))


Nehmen Sie einen Moment Zeit, um das Steuerelement für die Anmeldung durch Besuchen der Website über einen Browser und die Anmeldung als einen vorhandenen Benutzer in das mitgliedschaftsframework zu testen.

Das Anmeldesteuerelement gerenderten-Schnittstelle ist hochgradig konfigurierbar. Es gibt eine Reihe von Eigenschaften, die die Darstellung zu beeinflussen. kann das Steuerelement für die Anmeldung in eine Vorlage für die genaue Kontrolle über das Layout der Elemente der Benutzeroberfläche konvertiert werden. Der Rest dieses Schritts wird untersucht, wie die Darstellung und das Layout anpassen.

### <a name="customizing-the-login-controls-appearance"></a>Anpassen der Darstellung des Steuerelements für die Anmeldung

Das Anmeldesteuerelement standardeigenschafteneinstellungen Rendern einer Benutzeroberfläche mit einem Titel (Anmeldung), beim nächsten Mal von TextBox und Label-Steuerelemente für die Eingaben Benutzername und Kennwort, eine Erinnerung aktiviert, und eine Schaltfläche "Anmelden". Die Darstellung dieser Elemente sind alle konfigurierbaren über zahlreiche Eigenschaften des Login-Steuerelements. Darüber hinaus können festlegen, eine Eigenschaft oder zwei zusätzliche Elemente der Benutzeroberfläche – z. B. einen Link zu einer Seite zum Erstellen eines neuen Benutzerkontos - hinzugefügt werden.

Betrachten Sie einen Moment, um die Darstellung des unsere Anmeldesteuerelement gussy an. Da die `Login.aspx` Seite bereits Text am oberen Rand der Seite mit dem Anmeldenamen Text ist, wird das Anmeldesteuerelement Titel lautet überflüssig. Aus diesem Grund lösche die [ `TitleText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.titletext.aspx) Wert um den Titel des Steuerelements für die Anmeldung zu entfernen.

Der Benutzername: und Kennwort: Bezeichnungen auf der linken Seite des zwei TextBox-Steuerelemente können angepasst werden, über die [ `UserNameLabelText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.usernamelabeltext.aspx) und [ `PasswordLabelText` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordlabeltext.aspx)bzw. Ändern wir den Namen des Benutzers: Bezeichnung, die Benutzernamen zu lesen:. Die Bezeichnung und TextBox-Stile sind konfigurierbar, über die [ `LabelStyle` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.labelstyle.aspx) und [ `TextBoxStyle` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textboxstyle.aspx)bzw.

Die Anmeldedaten weiter Zeit Kontrollkästchens Text-Eigenschaft werden, über des Anmeldesteuerelement festgelegt kann [ `RememberMeText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermetext.aspx), und die Standardeinstellung überprüft Status wird über die [ `RememberMeSet` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.remembermeset.aspx)(der Standardwert "false"). Fahren Sie fort, und legen Sie die `RememberMeSet` Eigenschaft auf "true", damit die Anmeldedaten beim nächsten Mal Kontrollkästchen wird standardmäßig aktiviert sein.

Die Login-Steuerelement bietet zwei Eigenschaften für das Layout der Steuerelemente der Benutzeroberfläche anpassen. Die [ `TextLayout` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.textlayout.aspx) gibt an, ob der Benutzername: und das Kennwort: Bezeichnungen auf der linken Seite die entsprechenden Textfelder ein (Standard) oder über sie angezeigt werden. Die [ `Orientation` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.orientation.aspx) gibt an, ob die Eingaben für Benutzername und Kennwort vertikal befinden (übereinander) oder horizontal. Ich werde diese beiden Eigenschaften auf ihre Standardwerte festgelegt zu lassen, jedoch sollten Sie versuchen, diese beiden Eigenschaften festlegen, auf die nicht standardmäßige Werte, die sich ergebende Auswirkung anzuzeigen.

> [!NOTE]
> Im nächsten Abschnitt betrachten konfigurieren das Anmeldesteuerelement Layout, wir mithilfe von Vorlagen zum Definieren des genauen Layouts der Elemente der Benutzeroberfläche des Layout-Steuerelements.


Wrappen Sie eigenschafteneinstellungen für die Login-Steuerelement durch Festlegen der [ `CreateUserText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createusertext.aspx) und [ `CreateUserUrl` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.createuserurl.aspx) auf nicht noch registriert? Erstellen Sie ein Konto! und `~/Membership/CreatingUserAccounts.aspx`bzw. Dadurch wird einen Link hinzugefügt, auf dem Login-Steuerelement-Schnittstelle auf der Seite zeigen wir erstellt, in haben der <a id="Tutorial05"> </a> [vorherigen Lernprogramm](creating-user-accounts-vb.md). Des Anmeldesteuerelement [ `HelpPageText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppagetext.aspx) und [ `HelpPageUrl` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.helppageurl.aspx) und [ `PasswordRecoveryText` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoverytext.aspx) und [ `PasswordRecoveryUrl` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.passwordrecoveryurl.aspx) funktionieren auf dieselbe Weise, Links zu einer Hilfeseite ein, und eine Kennwortwiederherstellungsseite rendern.

Nach dieser Eigenschaft Änderungen sollte deklaratives Markup und die Darstellung des Steuerelements für Ihre Anmeldung, die in Abbildung 5 dargestellten ähneln.


[![Das Anmeldesteuerelement Eigenschaften Werte vorgeben, dessen Darstellung](validating-user-credentials-against-the-membership-user-store-vb/_static/image14.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image13.png)

**Abbildung 5**: die Login-Steuerelement die Eigenschaften Werte vorgeben seine Darstellung ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image15.png))


### <a name="configuring-the-login-controls-layout"></a>Konfigurieren das Anmeldesteuerelement-Layout

Die Login-Websteuerelement-Standardbenutzeroberfläche ordnet die Schnittstelle in einem HTML- `<table>`. Aber was geschieht, wenn eine präzisere Kontrolle über die gerenderte Ausgabe erforderlich? Vielleicht möchten wir ersetzen die `<table>` mit einer Reihe von `<div>` Tags. Oder was geschieht, wenn die Anwendung zusätzliche Anmeldeinformationen erfordert, für die Authentifizierung? Viele Websites finanzielle müssen z. B. Benutzer nicht nur einen Benutzernamen und Kennwort, aber auch eine Anzahl PIN (Personal Identification) oder andere identifizierende Informationen angeben. Was die Gründe sein mag, ist es möglich, das Login-Steuerelement in eine Vorlage konvertieren aus dem wir explizit deklaratives Markup der Schnittstelle definieren können.

Wir müssen zwei Schritte zum Aktualisieren der Login-Steuerelement zum Sammeln zusätzlicher Anmeldeinformationen ausführen:

1. Aktualisieren Sie das Anmeldesteuerelement-Schnittstelle, um die Web-Steuerelemente, um die zusätzlichen Anmeldeinformationen zu erfassen sind.
2. Überschreiben Sie die Anmeldung Logik des Steuerelements interne Authentifizierung, sodass nur ein Benutzer authentifiziert wird, wenn Benutzername und Kennwort gültig ist und seine zusätzlichen Anmeldeinformationen gültig sind, zu.

Um die erste Aufgabe zu erreichen, müssen wir das Login-Steuerelement in eine Vorlage konvertieren und die erforderlichen Web-Steuerelemente hinzufügen. Wie bei der zweiten Aufgabe kann durch Erstellen eines ereignishandlers für des Steuerelements die Authentifizierungslogik die Login-Steuerelement ersetzt werden [ `Authenticate` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx).

Das Steuerelement für die Anmeldung aktualisieren wir, damit Benutzer ihren Benutzernamen, Kennwort und e-Mail-Adresse aufgefordert und den Benutzer nur, authentifiziert Wenn die bereitgestellte e-Mail-Adresse die e-Mail-Adresse auf die Datei übereinstimmt. Wir müssen zunächst das Anmeldesteuerelement-Schnittstelle in eine Vorlage konvertieren. Wählen Sie aus des Login-Steuerelements Smarttag in der Vorlagenoption konvertieren.


[![Das Steuerelement für die Anmeldung in eine Vorlage konvertieren](validating-user-credentials-against-the-membership-user-store-vb/_static/image17.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image16.png)

**Abbildung 6**: das Steuerelement für die Anmeldung in eine Vorlage konvertieren ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image18.png))


> [!NOTE]
> Die Login-Steuerelement, auf die vorab template Version zurücksetzen möchten, klicken Sie auf die Links "Zurücksetzen" von Smart Tag des Steuerelements.


Konvertieren das Steuerelement für die Anmeldung in eine Vorlage fügt eine `LayoutTemplate` zum deklarativen Markup mit HTML-Elemente und Websteuerelemente Definieren der Benutzeroberfläche des Steuerelements. Wie in Abbildung 7 dargestellt, konvertieren das Steuerelement in eine Vorlage entfernt eine Reihe von Eigenschaften im Eigenschaftenfenster, z. B. `TitleText`, `CreateUserUrl`usw., da diese Eigenschaftswerte ignoriert werden, wenn Sie eine Vorlage zu verwenden.


[![Weniger Eigenschaften sind verfügbar bei der Login-Steuerelement in eine Vorlage konvertiert wird](validating-user-credentials-against-the-membership-user-store-vb/_static/image20.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image19.png)

**Abbildung 7**: weniger Eigenschaften sind verfügbar bei der Login-Steuerelement in eine Vorlage konvertiert wird ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image21.png))


Das HTML-Markup in der `LayoutTemplate` kann je nach Bedarf geändert werden. Ebenso können Sie alle neuen Websteuerelemente zur Vorlage hinzufügen. Allerdings ist es wichtig, Login-Steuerelement-Core-Websteuerelemente bleiben in der Vorlage, und behalten ihre zugewiesenen `ID` Werte. Insbesondere nicht entfernen oder Umbenennen der `UserName` oder `Password` Textfelder ein, die `RememberMe` Kontrollkästchen die `LoginButton` Schaltfläche der `FailureText` Bezeichnung, oder die `RequiredFieldValidator` Steuerelemente.

Um die e-Mail-Adresse des Besuchers zu erfassen, müssen wir die Vorlage ein Textfeld hinzugefügt. Fügen Sie das folgende deklarative Markup zwischen die Tabellenzeile (`<tr>`), enthält die `Password` TextBox "und" die Zeile der Tabelle, die die Anmeldedaten neben enthält Zeit Kontrollkästchen:

[!code-aspx[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample2.aspx)]

Nach dem Hinzufügen der `Email` Textfeld finden Sie auf der Seite über einen Browser. Wie in Abbildung 8 gezeigt, enthält die Benutzeroberfläche des Steuerelements für die Anmeldung jetzt eine dritte Textfeld.


[![Die Login-Steuerelement enthält jetzt ein TextBox-Element für die e-Mail-Adresse des Benutzers](validating-user-credentials-against-the-membership-user-store-vb/_static/image23.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image22.png)

**Abbildung 8**: das Login-Steuerelement enthält jetzt ein TextBox-Element für die e-Mail-Adresse des Benutzers ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image24.png))


An diesem Punkt ist das Anmeldesteuerelement weiterhin mithilfe der `Membership.ValidateUser` Methode, um die angegebenen Anmeldeinformationen zu überprüfen. Dementsprechend der eingegebene Wert in der `Email` Textfeld hat keinen Einfluss auf gibt an, ob der Benutzer anmelden kann. In Schritt 3 betrachten wir wie das Anmeldesteuerelement Authentifizierungslogik zu überschreiben, sodass die Anmeldeinformationen dann sind nur gültig ist, wenn Benutzername und Kennwort gültig sind, und die angegebenen e-Mail-Adresse mit der e-Mail-Adresse hinterlegt stimmt.

## <a name="step-3-modifying-the-login-controls-authentication-logic"></a>Schritt 3: Ändern der Authentifizierungslogik die Login-Steuerelement

Wenn ein Besucher ihrer bereitstellt steuern, Anmeldeinformationen und Klicks, die die Schaltfläche anmelden, ein Postback erfolgt und die Anmeldung durchläuft der Workflow der Authentifizierung. Der Workflow startet durch Auslösen der [ `LoggingIn` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggingin.aspx). Diesem Ereignis zugeordnete Ereignishandler können das Protokoll in Vorgang abbrechen, indem die `e.Cancel` Eigenschaft `True`.

Wenn das Protokoll in der Vorgang nicht abgebrochen wird, wechselt der Workflow durch Auslösen der [ `Authenticate` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.authenticate.aspx). Es ist ein Ereignishandler für die `Authenticate` -Ereignis, dann ist zuständig für das ermitteln, ob die angegebenen Anmeldeinformationen gültig oder nicht sind. Wenn kein Ereignishandler angegeben wird, mit das Login-Steuerelement verwendet die `Membership.ValidateUser` Methode, um die Gültigkeit der Anmeldeinformationen zu bestimmen.

Wenn die angegebenen Anmeldeinformationen gültig sind, wird das Formularauthentifizierungsticket erstellt, die [ `LoggedIn` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loggedin.aspx) ausgelöst wird, und der Benutzer wird an die richtige Seite umgeleitet. Wenn Sie jedoch die Anmeldeinformationen als ungültig eingestuft werden und dann die [ `LoginError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.loginerror.aspx) wird ausgelöst, und eine Meldung angezeigt wird, den Benutzer darüber informiert, dass ihre Anmeldeinformationen ungültig waren. Bei der Anmeldung Steuerelement einfach legt standardmäßig die `FailureText` Bezeichnung des Steuerelements Text-Eigenschaft eine entsprechende Fehlermeldung (die Anmeldung verlief war nicht erfolgreich. Bitte versuchen Sie es). Jedoch wenn des Anmeldesteuerelement [ `FailureAction` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failureaction.aspx) nastaven NA hodnotu `RedirectToLoginPage`, klicken Sie dann das Steuerelement Problemen bei der Anmeldung eine `Response.Redirect` auf die Anmeldeseite den Querystring-Parameter Anfügen `loginfailure=1` (wodurch die Anmeldung Steuerelement, das die Fehlermeldung anzuzeigen.)

Abbildung 9 bietet ein Flussdiagramm, der den Workflow der Authentifizierung.


[![Das Anmeldesteuerelement Authentifizierungsworkflow](validating-user-credentials-against-the-membership-user-store-vb/_static/image26.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image25.png)

**Abbildung 9**: Workflow für die Login-Steuerelement-Authentifizierung ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image27.png))


> [!NOTE]
> Wenn Sie wissen möchten, wenn Sie verwenden würden die `FailureAction`des `RedirectToLogin` Seite die Option, das folgende Szenario. Jetzt unsere `Site.master` Masterseite enthält derzeit noch den Text Hello, stranger angezeigt, in der linken Spalte, wenn von einem anonymen Benutzer, aufgerufen, aber angenommen, wir wollten mit einem Login-Steuerelement den Text ersetzen. Dadurch würde einen anonymen Benutzer zur Anmeldung von einer beliebigen Seite auf der Website, statt sie direkt die Anmeldeseite zu besuchen. Jedoch, wenn ein Benutzer kann nicht für die Anmeldung über die Login-Steuerelement, das von der Masterseite gerendert wurde, kann es sinnvoll, die sie zur Anmeldeseite umgeleitet (`Login.aspx`), da diese Seite wahrscheinlich zusätzliche Anweisungen, Links und andere Hilfe – z. B. Verknüpfungen erstellen enthält ein Neues Konto, oder Abrufen eines verlorenen Kennworts –, die nicht auf die Masterseite hinzugefügt wurden.


### <a name="creating-theauthenticateevent-handler"></a>Erstellen der`Authenticate`-Ereignishandler

Um unsere benutzerdefinierte Authentifizierungslogik anschließen, müssen wir erstellen einen Ereignishandler für des Anmeldesteuerelement `Authenticate` Ereignis. Erstellen einen Ereignishandler für die `Authenticate` Ereignis wird die folgende Definition der Ereignis-Handler generiert:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample3.vb)]

Wie Sie sehen können, die `Authenticate` übergebene Ereignishandler wird ein Objekt des Typs [`AuthenticateEventArgs`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.authenticateeventargs.aspx) als der zweite Eingabeparameter. Die `AuthenticateEventArgs` -Klasse enthält eine boolesche Eigenschaft namens `Authenticated` , verwendet, um anzugeben, ob die angegebenen Anmeldeinformationen gültig sind. Unsere Aufgabe ist anschließend Code schreiben, der bestimmt, ob die angegebenen Anmeldeinformationen gültig sind, und Festlegen der `e.Authenticate` Eigenschaft entsprechend.

### <a name="determining-and-validating-the-supplied-credentials"></a>Bestimmen und die angegebenen Anmeldeinformationen überprüfen

Verwenden Sie des Anmeldesteuerelement [ `UserName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.username.aspx) und [ `Password` Eigenschaften](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.password.aspx) um zu bestimmen, die vom Benutzer eingegebene Benutzername und Kennwort-Anmeldeinformationen. Um zu ermitteln, die in jeder zusätzliche Websteuerelemente eingegebenen Werte (wie z. B. die `Email` TextBox, die wir im vorherigen Schritt hinzugefügt haben), verwenden Sie `LoginControlID.FindControl`("*`controlID`*"), die einen programmgesteuerten Verweis auf das Web erhalten Steuerelemente in der Vorlage, deren `ID` Eigenschaft *`controlID`*. Um beispielsweise einen Verweis auf die `Email` TextBox, verwenden Sie den folgenden Code:

`Dim EmailTextBox As TextBox = CType(myLogin.FindControl("Email"), TextBox)`

Um die Anmeldeinformationen des Benutzers überprüfen, die wir zwei Dinge tun müssen:

1. Stellen Sie sicher, dass der angegebene Benutzername und Kennwort gültig sind.
2. Sicherstellen Sie, dass die eingegebene e-Mail-Adresse die e-Mail-Adresse für die Datei für den Benutzer anmelden entspricht

Um die erste Überprüfung erreichen, wir einfach verwenden können die `Membership.ValidateUser` Methode wie in Schritt 1 zu sehen war. Für die zweite Überprüfung müssen wir die e-Mail-Adresse des Benutzers zu bestimmen, damit wir ihn mit der e-Mail-Adresse vergleichen können, die sie in das TextBox-Steuerelement eingegeben. Rufen Sie Informationen zu einem bestimmten Benutzer mit der `Membership` Klasse [ `GetUser` Methode](https://msdn.microsoft.com/library/system.web.security.membership.getuser.aspx).

Die `GetUser` Methode bietet eine Reihe von Überladungen. Wenn ohne Übergabe von Parametern verwendet wird, wird die Informationen über den aktuell angemeldeten Benutzer zurückgegeben. Rufen Sie zum Abrufen von Informationen zu einem bestimmten Benutzer `GetUser` in ihren Benutzernamen übergeben. In beiden Fällen `GetUser` gibt eine [ `MembershipUser` Objekt](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), die über Eigenschaften wie verfügt `UserName`, `Email`, `IsApproved`, `IsOnline`und so weiter.

Der folgende Code implementiert diese zwei Prüfungen. Wenn beide, dann übergeben `e.Authenticate` nastaven NA hodnotu `True`, andernfalls ist er zugewiesen `False`.

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample4.vb)]

Versuchen Sie mit diesem Code werden sich als ein gültiger Benutzer, die Sie eingeben, den richtigen Benutzernamen, Kennwort und e-Mail-Adresse anmelden. Wiederholen sie den Vorgang, aber dieses Mal wird absichtlich eine falsche e-Mail-Adresse verwenden (siehe Abbildung 10). Schließlich testen Sie es ein drittes Mal mit einem nicht vorhandenen Benutzernamen. Im ersten Fall Sie sollten erfolgreich angemeldet sein, den Standort, aber in den letzten beiden Fällen wurde dem Login-Steuerelement, ungültige Anmeldeinformationen angezeigt.


[![Tito Anmelden nicht, wenn Sie eine falsche e-Mail-Adresse angeben](validating-user-credentials-against-the-membership-user-store-vb/_static/image29.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image28.png)

**Abbildung 10**: Tito kann nicht Log In bei der Bereitstellung eine falsche e-Mail-Adresse ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image30.png))


> [!NOTE]
> Wie beschrieben im Abschnitt in Schritt 1, wie die Mitgliedschaft Framework verarbeitet ungültige Anmeldeversuche bei der `Membership.ValidateUser` Methode aufgerufen und ungültige Anmeldeinformationen übergeben, er verfolgt die Ungültiger Versuch der Anmeldung und den Benutzer gesperrt werden, wenn sie eine bestimmte überschreiten Schwellenwert für die ungültige Versuche innerhalb eines angegebenen Zeitfensters. Da unsere benutzerdefinierte Logik authentifizierungsaufrufe der `ValidateUser` ein falsches Kennwort für einen gültigen Benutzernamen-Methode den Zähler für ungültige Anmeldung erhöht, aber wird nicht erhöht, in dem Fall, in denen der Benutzername und Kennwort gültig ist, werden, aber die e-Mail-Adresse ist falsch. Wahrscheinlich sind, dieses Verhalten geeignet ist, ist, da es ist unwahrscheinlich, dass ein Hacker, den Benutzernamen und das Kennwort kennen wird, aber brute-Force-Techniken verwenden, um zu bestimmen, die e-Mail-Adresse des Benutzers an.


## <a name="step-4-improving-the-login-controls-invalid-credentials-message"></a>Schritt 4: Verbessern der Nachricht für die Login-Steuerelement ungültige Anmeldeinformationen

Wenn ein Benutzer versucht, die mit ungültigen Anmeldeinformationen anmelden, zeigt das Anmeldesteuerelement eine Meldung darüber informiert, dass ein Anmeldeversuch nicht erfolgreich war. Insbesondere das Steuerelement zeigt die Nachricht gemäß der [ `FailureText` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.failuretext.aspx), der Standardwert lautet die Anmeldung verlief wurde nicht erfolgreich. Versuchen Sie es erneut.

Denken Sie daran, dass es gibt viele Gründe, warum die Anmeldeinformationen eines Benutzers ungültig werden kann:

- Der Benutzername ist nicht vorhanden.
- Der Benutzername vorhanden ist, aber das Kennwort ist ungültig.
- Der Benutzername und Kennwort gültig sind, aber der Benutzer noch nicht genehmigt
- Der Benutzername und Kennwort gültig sind, aber der Benutzer gesperrt Out (sehr wahrscheinlich, da sie die Anzahl der innerhalb des angegebenen Zeitraums ungültigen Anmeldeversuchen überschritten)

Und es kann aus anderen Gründen bei Verwendung einer benutzerdefinierten Authentifizierungslogik. Beispielsweise durch den Code, den wir in Schritt 3, den Benutzernamen geschrieben und das Kennwort ist möglicherweise gültig, aber die e-Mail-Adresse möglicherweise nicht korrekt.

Unabhängig davon, warum die Anmeldeinformationen ungültig sind werden die Login-Steuerelement die gleiche Fehlermeldung angezeigt. Dieser Mangel an Feedback kann für einen Benutzer verwirrend sein, dessen Konto noch nicht genehmigt wurden, oder, die gesperrt ist. Mit ein wenig Arbeit jedoch können wir das Anmeldesteuerelement eine geeignetere Meldung angezeigt haben.

Jedes Mal, wenn ein Benutzer versucht, ungültige Anmeldeinformationen Anmeldung die Login-Steuerelement löst die `LoginError` Ereignis. Fahren Sie fort, und erstellen Sie einen Ereignishandler für dieses Ereignis, und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](validating-user-credentials-against-the-membership-user-store-vb/samples/sample5.vb)]

Der obige Code beginnt, durch Festlegen der Login-Steuerelement `FailureText` Eigenschaft auf den Standardwert (die Anmeldung verlief war nicht erfolgreich. Bitte versuchen Sie es). Er überprüft dann um festzustellen, ob der angegebene Benutzername ein vorhandenes Benutzerkonto zugeordnet ist. Wenn also die resultierende herangezogen, `MembershipUser` des Objekts `IsLockedOut` und `IsApproved` Eigenschaften zu bestimmen, ob das Konto ist gesperrt oder noch nicht genehmigt wurden. In beiden Fällen die `FailureText` Eigenschaft in einen entsprechenden Wert aktualisiert wird.

Zum Testen dieses Codes absichtlich versucht, melden Sie sich als einen vorhandenen Benutzer, aber verwenden ein falsches Kennwort. Werden von diesem fünfmal in einer Zeile innerhalb eines Zeitraums von 10 Minuten, und das Konto wird gesperrt werden. Wie Abbildung 11 zeigt, der nächsten Anmeldung versucht immer (sogar mit dem richtigen Kennwort) fehl, aber die jetzt den aussagekräftigeren angezeigt hat Ihr Konto aufgrund zu vieler ungültige Anmeldeversuche gesperrt wurde. Wenden Sie sich an den Administrator, um Ihr Konto nicht gesperrte Nachricht aus.


[![Tito zu viele ungültige Anmeldeversuche ausgeführt und wurde gesperrt](validating-user-credentials-against-the-membership-user-store-vb/_static/image32.png)](validating-user-credentials-against-the-membership-user-store-vb/_static/image31.png)

**Abbildung 11**: Tito ausgeführt zu viele ungültige Anmeldeversuche und verfügt über wurde gesperrt ([klicken Sie, um das Bild in voller Größe anzeigen](validating-user-credentials-against-the-membership-user-store-vb/_static/image33.png))


## <a name="summary"></a>Zusammenfassung

Vor diesem Tutorial unsere Anmeldeseite überprüft die angegebenen Anmeldeinformationen für eine hartcodierte Liste von Benutzername/Kennwort-Paaren. In diesem Tutorial aktualisiert wir die Seite, um die Anmeldeinformationen für das Framework für die Mitgliedschaft überprüft werden. In Schritt 1 erläutert, mit der `Membership.ValidateUser` Methode programmgesteuert. In Schritt2 ersetzten wir unsere manuell erstellte Benutzeroberfläche und den Code mit dem Login-Steuerelement.

Die Login-Steuerelement rendert eine standardanmeldung-Benutzeroberfläche und überprüft automatisch die Anmeldeinformationen des Benutzers für das mitgliedschaftsframework. Darüber hinaus bei gültige Anmeldeinformationen, das Steuerelement für die Anmeldung der Benutzer angemeldet über die Formularauthentifizierung. Kurz gesagt, ist eine voll funktionsfähige Anmeldeoberfläche für Benutzer verfügbar, durch einfaches Ziehen das Steuerelement für die Anmeldung auf einer Seite, ohne zusätzlichen deklarativen Markup oder Code erforderlich. Ist ist das Anmeldesteuerelement stark angepasst werden, ermöglicht ein Maß an Kontrolle über die gerenderten Benutzer Schnittstelle und die Authentifizierung der Logik in Ordnung.

An diesem Punkt Besucher auf unserer Website können erstellen eine neue Benutzerkonto und ein Protokoll auf den Standort, aber wir haben dafür noch zum Einschränken des Zugriffs auf Seiten auf Grundlage des authentifizierten Benutzers ansehen. Derzeit kann alle Benutzer, authentifiziert oder anonym ist, einer beliebigen Seite auf unserer Website anzeigen. Zusammen mit der Steuerung des Zugriffs auf unserer Website Seiten pro Benutzer mit dem Benutzernamen ein, wir bestimmte Seiten möglicherweise der Benutzer, deren Funktionalität abhängig. Im nächste Tutorial untersucht, wie Zugriff und die in-Page-Funktionalität, die basierend auf dem angemeldeten Benutzer zu beschränken.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Anzeigen von benutzerdefinierten Nachrichten gesperrt, und nicht genehmigte Benutzer](http://aspnet.4guysfromrolla.com/articles/050306-1.aspx)
- [Untersuchen von ASP.NET 2.0 Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Gewusst wie: Erstellen eine Anmeldung ASP.NET-Seite](https://msdn.microsoft.com/library/ms178331.aspx)
- [Technische Dokumentation für Login-Steuerelement](https://msdn.microsoft.com/library/system.web.ui.webcontrols.login.aspx)
- [Mithilfe dieser Steuerelemente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/login.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurden Teresa Murphy und Michael Olivero. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](creating-user-accounts-vb.md)
> [Weiter](user-based-authorization-vb.md)
