---
uid: web-pages/overview/security/16-adding-security-and-membership
title: "Mitgliedschaft und Sicherheit zu einer ASP.NET-Webseite hinzufügen Pages (Razor) Website | Microsoft Docs"
author: tfitzmac
description: "Das Kapitel zeigt, wie Sie Ihre Website zu sichern, sodass einige Seiten nur an Personen verfügbar, die sind in anmelden. (Gewusst wie: Erstellen von Seiten Tha... entwicklungserfahrung"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: af2eeb128cff554e7ae3d903e2117861087344e9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages (Razor) Standort
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie eine Website für ASP.NET Web Pages (Razor) geschützt werden, da einige Seiten nur an Personen verfügbar sind, die in anmelden. (Sie müssen auch finden Sie unter Vorgehensweise: Erstellen von Seiten, die alle Benutzer zugreifen können.)
> 
> **Lernen Sie:** 
> 
> - Vorgehensweise zum Erstellen einer Website, die eine Registrierungsseite und einer Anmeldeseite verfügt, sodass für einige Seiten nur Mitgliedern Zugriff zu beschränken.
> - Vorgehensweise: Erstellen von Seiten "public" und "nur Member.
> - Definieren von Rollen, die Gruppen sind, die haben unterschiedliche Sicherheitsberechtigungen auf Ihrer Website, und zum Zuweisen von Benutzern zu einer Rolle.
> - Wie CAPTCHA verwenden, um zu verhindern, dass automatisierte Programme (Bots) Member Konten erstellen.
>   
> 
> Dies sind die Funktionen von ASP.NET im Artikel:
> 
> - Der WebMatrix **Starter Site** Vorlage.
> - Die `WebSecurity` Helper und `Roles` Klasse.
> - Die `ReCaptcha` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


Sie können Ihre Website einrichten, sodass Benutzer, in &#8212;anmelden können; d. h., damit der Standort unterstützt *Mitgliedschaft*. Dies kann aus vielen Gründen nützlich sein. Z. B. möglicherweise Ihre Site Seiten, die nur für Elemente verfügbar sein sollen. In einigen Fällen müssen Sie möglicherweise Benutzer anmelden, um Sie Feedback senden oder einen Kommentar.

Auch wenn Ihre Website Mitgliedschaft unterstützt, nicht Benutzern unbedingt erforderlich, um sich anzumelden, bevor sie einige der Seiten auf der Website verwenden. Benutzer angemeldet sind, werden als bezeichnet *anonyme Benutzer*.

Ein Benutzer auf Ihrer Website registrieren kann und kann dann melden Sie sich an den Standort. Die Website erfordert einen Benutzernamen (e-Mail-Adresse) und ein Kennwort zur Bestätigung, dass Benutzer, die er vorgibt sind zu sein. Dieser anmelden und Überprüfen der Identität eines Benutzers wird bezeichnet als *Authentifizierung*.

Sie können Mitgliedschaft und Sicherheit auf unterschiedliche Weise einrichten:

- Wenn Sie WebMatrix verwenden, wird eine einfache Möglichkeit für die Erstellung als neue Website auf der Grundlage der **Starter Site** Vorlage. Diese Vorlage für Sicherheit und die Mitgliedschaft bereits konfiguriert ist und bereits eine Registrierungsseite, eine Anmeldeseite usw.

    Der Standort, von der Vorlage erstellt hat auch eine Option, damit Benutzer melden Sie sich mit einer externen Website wie Facebook, Google, oder Twitter.
- Wenn Sicherheit in eine vorhandene Site hinzufügen möchten, oder Sie verwenden möchten, nicht die **Starter Site** Vorlage können Sie eigene Registrierungsseite, Anmeldeseite usw. erstellen.

Dieser Artikel konzentriert sich auf die erste Option &mdash; zum Hinzufügen von Sicherheit mithilfe der **Starter Site** Vorlage. Es bietet auch einige grundlegende Informationen dazu, wie Sie Ihre eigenen Sicherheit zu implementieren und Wiederherstellungsprozessen sowie Links zu weiteren Informationen über das nachholen. Es gibt auch Informationen zum Aktivieren externer Anmeldungen, dies wird ausführlicher in einem separaten Artikel beschrieben.

## <a name="creating-website-security-using-the-starter-site-template"></a>Website-Sicherheit mithilfe der Startvorlage-Site erstellen

In WebMatrix können Sie die **Starter Site** Vorlage zum Erstellen einer Website mit den folgenden Komponenten:

- Eine Datenbank, die zum Speichern von Benutzernamen und Kennwörter für Ihre Elemente verwendet wird.
- Eine Registrierungsseite, wo anonyme (neu) Benutzer registrieren können.
- Eine Seite Anmelde- und Abmeldeereignisse.
- Ein Kennwort Wiederherstellung und Zurücksetzen der Seite.

Das folgende Verfahren beschreibt, wie die Website erstellen und zu konfigurieren.

1. Starten von WebMatrix und klicken Sie in der **Schnellstart** Seite **Websitevorlage aus**.
2. Wählen Sie die **Starter Site** Vorlage, und klicken Sie dann auf **OK**. WebMatrix erstellt einen neuen Standort.
3. Klicken Sie im linken Bereich auf die **Dateien** Arbeitsbereich Selektor.
4. Öffnen Sie im Stammordner der Website, die  *\_AppStart.cshtml* Datei, die eine spezielle Datei, die verwendet wird, um globale Einstellungen enthalten. Es enthält einige Anweisungen, die mit auskommentiert werden die `//` Zeichen:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Konfigurieren Sie diese Anweisungen die `WebMail` Hilfsprogramm, das zum Senden von e-Mail verwendet werden kann. Das Mitgliedschaftssystem kann e-Mail-Dienst verwenden, um Bestätigungsnachrichten versendet werden, wenn Benutzer registrieren, oder wenn sie ihre Kennwörter ändern möchten. (Z. B. nachdem ein Benutzer registrieren, erhalten sie eine e-Mail, die einen Link, den sie klicken können enthält, um den Registrierungsprozess abzuschließen.)

    Senden von e-Mail erfordert Zugriff auf einen SMTP-Server, wie in beschrieben [E-Mail hinzufügen, um eine ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202899). Sie müssen die e-Mail-Einstellungen in dieser Mitte speichern  *\_AppStart.cshtml* Datei, damit Sie nicht, diese wiederholt in jeder Seite zu codieren, die e-Mails senden kann. (Sie müssen die SMTP-Einstellungen zum Einrichten einer Registrierungsdatenbank konfigurieren; Sie brauchen nur SMTP-Einstellungen, wenn Benutzer ihre e-Mail-Alias überprüfen und können Benutzer ein vergessenes Kennwort zurückgesetzt werden sollen.)
5. Kommentieren Sie die Anweisungen durch das Entfernen `//` vor jeweils.

    Wenn Sie keine e-Mail-Bestätigung einrichten möchten, können Sie diesen Schritt und den nächsten Schritt überspringen. Wenn der SMTP-Werte nicht festgelegt werden, wird das neue Konto ohne eine e-Mail zur kaufbestätigung sofort verfügbar.
6. Ändern Sie die folgenden e-Mail-bezogenen Einstellungen in den Code ein:

    - Legen Sie `WebMail.SmtpServer` auf den Namen des SMTP-Servers, die Sie können zugreifen.
    - Lassen Sie `WebMail.EnableSsl` festgelegt `true`. Diese Einstellung sichert die Anmeldeinformationen, die an den SMTP-Server gesendet werden, indem sie verschlüsselt werden.
    - Legen Sie `WebMail.UserName` mit dem Benutzernamen für Ihr SMTP-Server-Konto.
    - Legen Sie `WebMail.Password` auf das Kennwort für Ihr SMTP-Server-Konto.
    - Legen Sie `WebMail.From` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird.

    > [!NOTE] 
    > 
    > **Tipp** zusätzliche Informationen zu den Werten für diese Eigenschaften finden Sie unter [Konfigurieren von e-Mail-Einstellungen](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) in [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Speichern und schließen Sie  *\_AppStart.cshtml*.
8. Führen Sie die *Default.cshtml* Seite in einem Browser.

    ![Sicherheit-Mitgliedschaft-2](16-adding-security-and-membership/_static/image1.png)

    > [!NOTE]
    > Wenn Sie eine Fehlermeldung, die besagt angezeigt, dass eine Eigenschaft einer Instanz des Transportservers `ExtendedMembershipProvider`, der Standort möglicherweise nicht verwenden, die ASP.NET Web Pages-Mitgliedschaftssystems (SimpleMembership) konfiguriert werden. Dies kann manchmal auftreten, wenn es sich bei einem Hostinganbieter Server anders als Ihr lokaler Server konfiguriert ist. Um dieses Problem zu beheben, fügen Sie das folgende Element mit des Standorts *"Web.config"* Datei:
    > 
    > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
    > 
    > Fügen Sie dieses Element als untergeordnetes Element von der `<configuration>` Element sowie einen Peer der `<system.web>` Element.
9. Klicken Sie in der oberen rechten Ecke der Seite, auf die **registrieren** Link. Die *Register.cshtml* Seite wird angezeigt.
10. Geben Sie einen Benutzernamen und ein Kennwort, und klicken Sie dann auf **registrieren**.

    ![Sicherheit-Mitgliedschaft-3](16-adding-security-and-membership/_static/image2.png)

    Beim Erstellen der Website aus der **Starter Site** Vorlage, eine Datenbank namens *StarterSite.sdf* am Standort der Erstellung *App\_Daten* Ordner. Während der Registrierung ist Ihre Benutzerinformationen in der Datenbank hinzugefügt. Wenn Sie die SMTP-Werte festlegen, wird eine Nachricht an die e-Mail-Adresse gesendet, dass Sie verwendet werden, damit Sie die Registrierung abschließen können.

    ![Sicherheit-Mitgliedschaft-4](16-adding-security-and-membership/_static/image3.png)
11. Rufen Sie Ihre e-Mail-Programm, und suchen Sie die Nachricht, die Bestätigungscode und einen Link zur Website verfügt.
12. Klicken Sie auf den Link, um Ihr Konto zu aktivieren. Der Link zur Bestätigung wird eine Bestätigungsseite Registrierung geöffnet.

    ![Sicherheit-Mitgliedschaft-5](16-adding-security-and-membership/_static/image4.png)
- Klicken Sie auf die **Anmeldung** verknüpfen, und melden Sie sich mit dem Konto, den Sie registriert.

    Nach dem Anmelden die **Anmeldung** und **registrieren** Links werden durch ersetzt eine **Logout** Link. Der Anmeldename wird als Link angezeigt. (Link können Sie die zu einer Seite zu wechseln, in dem Sie Ihr Kennwort ändern.)

    ![Sicherheit-Mitgliedschaft-6](16-adding-security-and-membership/_static/image5.png)

    > [!NOTE]
    > Standardmäßig senden ASP.NET Web Pages Anmeldeinformationen an den Server in Klartext (wie einem von Menschen lesbaren Text). Eine Produktionswebsite zu verwendende secure HTTP (https://, auch bekannt als die *secure Sockets Layer* oder SSL) zum Verschlüsseln von vertraulichen Informationen, die mit dem Server ausgetauscht werden. Können Sie die erforderlichen-e-Mail Nachrichten gesendet werden mit SSL durch Festlegen von `WebMail.EnableSsl=true` wie im vorherigen Beispiel. Weitere Informationen zu SSL finden Sie unter [Web Sichern der Kommunikation: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Zusätzliche Mitgliedschaftsfunktionen auf der Website

Ihre Website enthält anderer Funktionen, die Benutzer ihre Konten verwalten kann. Benutzer können Folgendes tun:

- Ändern Sie ihre Kennwörter. Nach der Anmeldung können sie den Benutzernamen klicken (die einen Link handelt). Dadurch gelangen sie zu einer Seite, in dem sie ein neues Kennwort erstellen (*Account/ChangePassword.cshtml*).
- Wiederherzustellen Sie ein vergessenes Kennwort. Auf der Anmeldeseite wird ein Link (**haben Sie Ihr Kennwort vergessen?**), die Benutzer zu einer Seite akzeptiert (*Account/ForgotPassword.cshtml*) in dem sie eine e-Mail-Adresse eingeben können. Die Website sendet sie eine e-Mail-Nachricht, die einen Link, die sie klicken können enthält, um ein neues Kennwort festlegen (*Account/PasswordReset.cshtml*).

Ferner können Benutzer melden sich mit einem externen Standort kann auch wie nachfolgend erläutert.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Erstellen eine Seite Mitgliedsinterne

Wird, kann alle Benutzer auf eine andere Seite in Ihrer Website durchsuchen. Sie möchten jedoch möglicherweise über Seiten verfügen, die nur an Personen verfügbar sind, die in angemeldet sind (d. h. Mitglieder). ASP.NET können Sie die Seiten zu erstellen, die nur von Mitgliedern von Anmeldeinformationen zugegriffen werden kann. Wenn anonyme Benutzer versuchen, eine Seite nur Member zuzugreifen, leiten Sie diese in der Regel zur Anmeldeseite.

In diesem Verfahren erstellen Sie einen Ordner aus, der Seiten enthält, die nur für angemeldete Benutzer verfügbar sind.

1. Erstellen Sie einen neuen Ordner im Stammverzeichnis der Website. (Klicken Sie im Menüband auf den Pfeil unterhalb **neu** und wählen Sie dann **neuer Ordner**.)
2. Nennen Sie diesen Ordner *Elemente*.
3. Innerhalb der *Elemente* Ordner, erstellen Sie eine neue Seite und den Namen *MembersInformation.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code und Markup:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Dieser Code überprüft die `IsAuthenticated` Eigenschaft von der `WebSecurity` -Objekt, das zurückgegeben `true` , wenn der Benutzer angemeldet hat. Wenn der Benutzer nicht, in der Code ruft angemeldet ist `Response.Redirect` den Benutzer zum Senden der *Login.cshtml* auf der Seite der *Konto* Ordner.

    Die URL der Umleitung enthält eine `returnUrl` Wert der Abfragezeichenfolge, die verwendet `Request.Url.LocalPath` um den Pfad der aktuellen Seite festzulegen. Wenn Sie festlegen, die `returnUrl` Wert in der Abfragezeichenfolge wie folgt (und bei die Rückgabe-URL ein lokaler Pfad ist), die Anmeldeseite wird Benutzer zu dieser Seite zurückkehren, nach der Anmeldung.

    Der Code setzt auch  *\_SiteLayout.cshtml* Seite wie die Seite "Layout". (Weitere Informationen zu Layoutseiten finden Sie unter [erstellen ein konsistentes Layout in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Führen Sie den Standort an. Wenn Sie weiterhin angemeldet sind, klicken Sie auf die **Logout** Schaltfläche am oberen Rand der Seite.
6. Fordern Sie im Browser die Seite */Mitglieder/MembersInformation*. Die URL kann beispielsweise wie folgt aussehen:

    `http://localhost:38366/Members/MembersInformation`

    (Die Portnummer (38366) wird wahrscheinlich in die URL anders sein.)

    Sie sind umgeleitet, um die *Login.cshtml* Seite, da Sie nicht angemeldet sind.
- Melden Sie sich mit dem Konto, das Sie zuvor erstellt haben. Sie sind umgeleitet, an die *MembersInformation* Seite. Da Sie angemeldet sind, sehen Sie diesmal den Seiteninhalt zu.

Um den Zugriff auf mehreren Seiten zu sichern, können Sie dies tun:

- Fügen Sie die sicherheitsüberprüfung auf jeder Seite hinzu.
- Erstellen einer  *\_PageStart.cshtml* Seite in den Ordner mit der Sie geschützte Seiten beibehalten, und fügen es die sicherheitsüberprüfung. Die  *\_PageStart.cshtml* Seite fungiert als eine Art von globaler Seite für alle Seiten im Ordner "". Diese Technik wird ausführlicher im [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Erstellen von Sicherheit für Gruppen von Benutzern (Rollen)

Wenn Ihre Website zahlreiche Elemente enthält, ist es nicht effizient, um die Berechtigung für jeden Benutzer einzeln überprüfen, bevor Sie eine Seite zu können. Möglichkeiten zum Erstellen von Gruppen, stattdessen werden oder *Rollen*, einzelne Elemente gehören. Sie können dann basierte auf Rolle Berechtigungen überprüfen. In diesem Abschnitt erstellen Sie eine &quot;Admin&quot; Rolle und erstellen Sie eine Seite, die für Benutzer verfügbar ist, sind in (wer angehören) dieser Rolle.

Das ASP.NET-Mitgliedschaftssystem wird eingerichtet, um Rollen zu unterstützen. Jedoch im Gegensatz zu Mitgliedschaft Registrierung und Anmeldung die **Starter Site** Vorlage keine Seiten, mit denen Sie Rollen verwalten. (Verwalten von Rollen ist eine Verwaltungsaufgabe statt einer Benutzeraufgabe.) Allerdings können Sie direkt in der Mitgliedschaftsdatenbank in WebMatrix Gruppen hinzufügen.

1. Klicken Sie in WebMatrix, auf die **Datenbanken** Arbeitsbereich Selektor.
2. Öffnen Sie im linken Bereich der *StarterSite.sdf* geöffneten Knoten die **Tabellen** -Knoten aus, und doppelklicken Sie dann auf die *Webseiten\_Rollen* Tabelle.

    ![Sicherheit-Mitgliedschaft-7](16-adding-security-and-membership/_static/image6.png)
3. Hinzufügen einer Rolle mit dem Namen &quot;Admin&quot;. Die *RoleId* Feld wird automatisch ausgefüllt. (Es ist der Primärschlüssel und verfügt über ein Feld identifizieren sein festgelegt wurde, wie in beschrieben [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Wie Sie sehen, lautet der Wert für die *RoleId* Feld. (Wenn dies die erste Rolle, die Sie definieren ist, wird 1 sein.)

    ![Sicherheit-Mitgliedschaft-8](16-adding-security-and-membership/_static/image7.png)
5. Schließen der *Webseiten\_Rollen* Tabelle.
6. Öffnen der *UserProfile* Tabelle.
7. Notieren Sie sich die *UserId* Wert von mindestens einer der Benutzer in der Tabelle, und schließen Sie dann die Tabelle.
8. Öffnen der *Webseiten\_UserInRoles* Tabelle, und geben Sie einen *UserID* und ein *RoleID* Wert in der Tabelle. Um beispielsweise Benutzer 2 in der &quot;Admin&quot; Rolle, geben Sie diese Werte:

    ![Sicherheit-Mitgliedschaft-9](16-adding-security-and-membership/_static/image8.png)
9. Schließen der *Webseiten\_UsersInRoles* Tabelle.

    Nun, da Sie Rollen definiert haben, können Sie eine Seite konfigurieren, die für Benutzer verfügbar ist, die in dieser Rolle sind.
10. Erstellen Sie in den Stammordner der Website eine neue Seite mit dem Namen *AdminError.cshtml* , und Ersetzen Sie den vorhandenen Inhalt durch folgenden Code. Dies ist die Seite wird, der an einen Benutzer umgeleitet werden, wenn sie den Zugriff auf eine Seite nicht gestattet ist.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Erstellen Sie in den Stammordner der Website eine neue Seite mit dem Namen *AdminOnly.cshtml* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Die `Roles.IsUserInRole` -Methode zurückkehrt `true` ist der aktuelle Benutzer ein Mitglied der angegebenen Rolle (in diesem Fall wird die Rolle "Admin").
12. Führen Sie *Default.cshtml* in einem Browser eingeben, aber nicht anmelden. (Wenn Sie bereits angemeldet sind, melden Sie sich ab.)
13. Fügen Sie in der Adressleiste des Browsers *AdminOnly* in der URL. (Das heißt, anfordern der *AdminOnly.cshtml* Datei.) Sie sind zur umgeleitet der *AdminError.cshtml* Seite, da Sie derzeit als Benutzer, in angemeldet sind der &quot;Admin&quot; Rolle.
14. Zurück zu *Default.cshtml* und melden Sie sich als der Benutzer, die Sie hinzugefügt, um haben die &quot;Admin&quot; Rolle.
15. Navigieren Sie zu *AdminOnly.cshtml* Seite. Dieses Mal wird die Seite angezeigt.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Verhindert, dass automatisierte Programme verknüpfen Ihre Website

Die Anmeldeseite wird nicht mehr automatisierte Programme (auch bezeichnet als *web Roboter* oder *Bots*) mit Ihrer Website registriert. Hier wird beschrieben, wie einen ReCaptcha-Test für die Seite "Registrierung" aktiviert.

![/media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrieren Sie Ihre Website unter ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einem privaten Schlüssel.
2. Der ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [installieren-Hilfsprogramme in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
3. In der *Konto* Ordner öffnen die Datei mit dem Namen *Register.cshtml*.
4. Suchen Sie im Code am oberen Rand der Seite mit den folgenden Zeilen und kommentieren Sie sie durch das Entfernen der `//` Kommentarzeichen:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Ersetzen Sie `PRIVATE_KEY` durch Ihren eigenen privaten ReCaptcha-Schlüssel.
6. Entfernen Sie das Markup der Seite, die `@*` und `*@` Kommentarzeichen aus, um die folgenden Zeilen im Markup Seite:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Ersetzen Sie `PUBLIC_KEY` durch Ihren Schlüssel.
8. Wenn Sie es bereits entfernt noch nicht, entfernen die `<div>` Element, das Text, der enthält mit "Zu ermöglichen, CAPTCHA-Prüfung..." beginnt. (Entfernen Sie den gesamten `<div>` Element und dessen Inhalt.)

1. Führen Sie *Default.cshtml* in einem Browser. Wenn Sie bei der Website angemeldet sind, klicken Sie auf die **Logout** Link.
2. Klicken Sie auf die **registrieren** verknüpfen und die Registrierung mithilfe des CAPTCHA-Tests zu testen.

    ![Sicherheit-Mitgliedschaft-10](16-adding-security-and-membership/_static/image9.png)

Weitere Informationen zu den `ReCaptcha` Helper, finden Sie unter [eine CATPCHA verwenden, um zu verhindern, dass automatisierte Programme (Bots) aus mithilfe der ASP.NET Web Site](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Wenn Benutzer melden Sie sich mit einer externen Website

Die **Starter Site** Vorlage enthält Code und Markup, mit dem Benutzer melden Sie sich mit Facebook, Windows Live, Twitter, Google oder Yahoo. Standardmäßig ist diese Funktionalität nicht aktiviert. Das allgemeine Verfahren für die Verwendung von Infrastrukturcode Benutzer melden sich mit diesen externen Anbietern ist dies:

- Entscheiden Sie, welche von externen Standorten unterstützt werden soll.
- Falls erforderlich, wechseln Sie zu diesem Standort, und richten Sie eine app für die Anmeldung. (Z. B. müssen Sie dies erforderlich ist, um die Facebook-Anmeldungen zu ermöglichen.)
- Konfigurieren Sie auf der Website des Anbieters ein. In den meisten Fällen nur müssen Teil des Codes im Entfernen der  *\_AppStart.cshtml* Datei.
- Fügen Sie auf der Registrierungsseite aus, in dem Personen kann Markup Verknüpfung mit der externen Website für die Anmeldung. Sie können das Markup in der Regel kopieren, das Sie benötigen, und ändern Sie den Text etwas.

Eine schrittweise Anleitung finden Sie im Thema [Aktivieren der Anmeldung von externen Standorten in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=251969).

Nachdem ein Benutzer von einem anderen Standort anmeldet, wird der Benutzer auf Ihrer Website zurückkehrt und *ordnet* , melden Sie sich mit Ihrer Website. Aktiviert ist, wird dies ein Mitgliedschaftseintrag an Ihrem Standort für die externe Anmeldung des Benutzers erstellt. Dadurch können Sie die normalen Funktionen der Mitgliedschaft (z. B. Rollen) mit der externen Anmeldung zu verwenden.

## <a name="adding-security-to-an-existing-website"></a>Hinzufügen von Sicherheit zu einer vorhandenen Website

Das Verfahren weiter oben in diesem Artikel verwendet wird, zur Verwendung der **Starter Site** Vorlage als Grundlage für die Website-Sicherheit. Wenn Sie zum Starten von nicht die **Starter Site** Vorlage oder um die relevanten Seiten von einer Website, auf Grundlage dieser Vorlage zu kopieren, können Sie den gleichen Typ der Sicherheit in Ihrer eigenen Site implementieren, indem Sie selbst codieren. Sie erstellen die gleichen Arten von Seiten – Registrierung, Anmeldung usw. – und verwenden Sie Hilfsmethoden und Klassen zum Einrichten der Mitgliedschaft.

Der grundlegende Prozess wird im Blogbeitrag beschrieben [die grundlegendste Möglichkeit zum Implementieren von ASP.NET Razor-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Die meiste Arbeit erfolgt mithilfe der folgenden Methoden und Eigenschaften der `WebSecurity` Hilfsprogramm:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Mit diesen Methoden können Sie bestimmen, ob ein Benutzer bereits registriert ist und sie zu registrieren.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Diese Eigenschaft können Sie bestimmen, ob der aktuelle Benutzer angemeldet ist. Dies ist nützlich für Benutzer zu einer Anmeldeseite umzuleiten, wenn sie nicht bereits angemeldet haben.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Diese Methoden meldet einen Benutzer aus, oder verkleinern.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Diese Eigenschaft ist nützlich für die Anzeige des aktuellen Benutzers angemeldeten Namen (wenn der Benutzer angemeldet ist).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Diese Methode ist nützlich, wenn Sie die e-Mail-Bestätigung für die Registrierung eingerichtet. (Details finden Sie im Blogbeitrag [mithilfe der Funktion zur ereignisbestätigung für ASP.NET Web Pages-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Um Rollen zu verwalten, können Sie die [Rollen](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) und [Mitgliedschaft](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) Klassen, wie in der Blogeintrag beschrieben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anpassen des Verhaltens von Websiteseiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sichern von Webkommunikation: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [DIE grundlegendste Möglichkeit zum Implementieren von ASP.NET Razor-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) und [mithilfe der Funktion zur ereignisbestätigung für ASP.NET Web Pages-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Hierbei handelt es sich um Blogbeiträge, die zum Implementieren von Mitgliedschaft ASP.NET-Funktionen ohne beschreiben die **Starter Site** Vorlage.
- [Aktivieren der Anmeldung über externe Websites einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity-Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [SimpleRoleProvider Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [SimpleMembershipProvider Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
