---
uid: web-pages/overview/security/16-adding-security-and-membership
title: Hinzufügen von Sicherheits- und Mitgliedschaftsberechtigungen zu einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Das Kapitel zeigt, wie Sie Ihre Website sichern, damit einige Seiten nur für Personen verfügbar sind, die sich in. (Sie werden auch zum Erstellen von Seiten Tha... sehen
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/24/2014
ms.topic: article
ms.assetid: 7a77c2c0-deea-4290-a9c3-97958891758e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/security/16-adding-security-and-membership
msc.type: authoredcontent
ms.openlocfilehash: 468a6661df6442705c2e2e178c01aad01bf5765c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383463"
---
<a name="adding-security-and-membership-to-an-aspnet-web-pages-razor-site"></a>Hinzufügen von Sicherheit und die Mitgliedschaft in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Sie eine Website für ASP.NET Web Pages (Razor) schützen, damit einige Seiten nur für Personen verfügbar sind, die sich in. (Sie müssen auch finden Sie unter Vorgehensweise: Erstellen von Seiten, die jeder Benutzer zugreifen kann.)
> 
> **Sie lernen Folgendes:** 
> 
> - Informationen zum Erstellen einer Website, die eine Seite zum Registrieren und eine Anmeldeseite verfügt, damit für einige Seiten Sie Zugriff auf nur Elemente einschränken können.
> - Vorgehensweise: Erstellen von Seiten "public" und "nur für Mitglieder.
> - Gewusst wie: Definieren von Rollen, die Gruppen sind, die haben unterschiedliche Sicherheitsberechtigungen auf Ihrer Website, und das Zuweisen von Benutzern zu einer Rolle.
> - So erfolgreich die CAPTCHAPRÜFUNG verwenden, um zu verhindern, dass die automatisierten Programmen (Bots) Erstellen von Konten für den Member.
>   
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Der WebMatrix **Starter Site** Vorlage.
> - Die `WebSecurity` Helper und `Roles` Klasse.
> - Die `ReCaptcha` Helper.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helpers Library


Sie können Ihre Website einrichten, damit, dass Benutzer anmelden können &#8212; , also so, dass der Standort unterstützt *Mitgliedschaft*. Dies kann aus vielen Gründen nützlich sein. Ihre Website kann z. B. Seiten haben, die nur für Mitglieder verfügbar sein sollen. In einigen Fällen können Benutzer aufgefordert werden, melden Sie sich, um Feedback senden oder einen Kommentar hinterlassen.

Auch wenn Ihre Website Mitgliedschaft unterstützt, nicht die Benutzer unbedingt erforderlich ist, anmelden, bevor sie einige der Seiten auf der Website verwenden. Benutzer, die nicht bei angemeldet sind, werden als bezeichnet *anonyme Benutzer*.

Ein Benutzer auf Ihrer Website registrieren kann und kann dann melden Sie sich an den Standort. Die Website erfordert einen Benutzernamen (e-Mail-Adresse) und ein Kennwort zu bestätigen, dass die Benutzer sind, die er vorgibt zu sein. Dieser Prozess der Anmeldung und zum Bestätigen der Identität eines Benutzers wird als bezeichnet *Authentifizierung*.

Sie können Sicherheits- und mitgliedschaftsberechtigungen auf unterschiedliche Weise einrichten:

- Wenn Sie WebMatrix verwenden, wird eine einfache Möglichkeit ist, so erstellen Sie als neue Website auf Grundlage der **Starter Site** Vorlage. Diese Vorlage ist bereits für die Sicherheits- und mitgliedschaftsberechtigungen konfiguriert und verfügt bereits über einer Registrierungsseite, eine Anmeldeseite und So weiter.

    Die Website, die von der Vorlage erstellten verfügt auch über die Option, damit die Benutzer melden Sie sich mit einem externen Standort wie Facebook, Google oder Twitter.
- Wenn Sie Sicherheit zu einer vorhandenen Website hinzufügen möchten, oder Sie verwenden möchten, nicht die **Starter Site** Vorlage können Sie Ihre eigenen Registrierungsseite, Seite ' Anmeldung ' usw. erstellen.

Dieser Artikel konzentriert sich auf die erste Option &mdash; Gewusst wie: Hinzufügen von Sicherheit mithilfe der **Starter Site** Vorlage. Es bietet auch einige grundlegende Informationen dazu, wie Sie Ihre Sicherheit selbst implementieren und Wiederherstellungsprozessen sowie Links zu weiteren Informationen dazu, wie Sie dies tun. Es gibt auch Informationen zum Aktivieren externer Anmeldungen, die in einem separaten Artikel ausführlicher beschrieben wird.

## <a name="creating-website-security-using-the-starter-site-template"></a>Website-Sicherheit mithilfe der Vorlage Starter Site erstellen

In WebMatrix können Sie die **Starter Site** Vorlage zum Erstellen einer Website mit den folgenden Komponenten:

- Eine Datenbank, die zum Speichern von Benutzernamen und Kennwörter für Ihre Elemente verwendet wird.
- Eine Registrierungsseite, in denen anonyme (neu) Benutzer registrieren kann.
- Eine Anmeldung und Abmeldung-Seite.
- Ein Kennwort Wiederherstellung und Zurücksetzen der Seite.

Das folgende Verfahren beschreibt die Website erstellen und konfigurieren.

1. Starten Sie WebMatrix und klicken Sie in der **Schnellstart** Seite **Websitevorlage aus**.
2. Wählen Sie die **Starter Site** -Vorlage, und klicken Sie dann auf **OK**. WebMatrix erstellt einen neuen Standort.
3. Klicken Sie im linken Bereich auf die **Dateien** arbeitsbereichsauswahl.
4. Öffnen Sie im Stammordner der Website, die  *\_AppStart.cshtml* Datei, d.h. eine spezielle Datei, die verwendet wird, um globale Einstellungen enthalten. Sie enthält einige Anweisungen, die mit auskommentiert werden die `//` Zeichen:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample1.cs)]

    Konfigurieren Sie diese Anweisungen die `WebMail` Hilfsmethode, die zum Senden von e-Mails verwendet werden kann. Das Mitgliedschaftssystem kann-e-Mail verwenden, um Bestätigungsnachrichten zu senden, wenn Benutzer registrieren, oder wenn sie ihre Kennwörter ändern möchten. (Z. B. nach dem Benutzer registrieren, erhalten sie einer e-Mails, die einen Link, den sie klicken können enthält, um den Registrierungsprozess abzuschließen.)

    Senden von e-Mails erfordert Zugriff auf einen SMTP-Server, wie in beschrieben [Hinzufügen von e-Mail-Adresse zu einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=202899). Speichern Sie die e-Mail-Einstellungen in dieser zentralen  *\_AppStart.cshtml* Datei, sodass Sie keine sie wiederholt auf jeder Seite ein code, die e-Mail-Adresse senden können. (Sie müssen keine SMTP-Einstellungen zum Einrichten einer Registrierungsdatenbank zu konfigurieren; lediglich SMTP-Einstellungen, wenn Sie möchten die Benutzer über ihre e-Mail-Alias überprüfen und können Benutzer ein vergessenes Kennwort zurückzusetzen.)
5. Kommentieren Sie die Anweisungen durch das Entfernen `//` vor jeder.

    Wenn Sie nicht, um e-Mail-Bestätigung einzurichten möchten, können Sie diesen Schritt und den nächsten Schritt überspringen. Wenn die SMTP-Werte nicht festgelegt werden, ist das neue Konto sofort verfügbar ist, ohne eine Bestätigung per e-Mail.
6. Ändern Sie die folgenden e-Mail-bezogenen Einstellungen in den Code ein:

   - Legen Sie `WebMail.SmtpServer` auf den Namen des SMTP-Servers, die Sie können zugreifen.
   - Lassen Sie `WebMail.EnableSsl` festgelegt `true`. Diese Einstellung sichert die Anmeldeinformationen, die an den SMTP-Server gesendet werden, indem sie verschlüsselt werden.
   - Legen Sie `WebMail.UserName` , den Benutzernamen für Ihr SMTP-Server-Konto.
   - Legen Sie `WebMail.Password` auf das Kennwort für Ihr SMTP-Server-Konto.
   - Legen Sie `WebMail.From` an Ihre eigene e-Mail-Adresse. Dies ist die e-Mail-Adresse, der aus die Nachricht gesendet wird.

     > [!NOTE] 
     > 
     > **Tipp** finden Sie weitere Informationen zu den Werten für diese Eigenschaften [Konfigurieren von e-Mail-Einstellungen](https://go.microsoft.com/fwlink/?LinkID=202906#configuring_email_settings) in [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202906).
7. Speichern und schließen Sie  *\_AppStart.cshtml*.
8. Führen Sie die *Default.cshtml* Seite in einem Browser.

    ![Security-Mitgliedschaft-2](16-adding-security-and-membership/_static/image1.png)

   > [!NOTE]
   > Wenn Sie eine Fehlermeldung angezeigt, die besagt, dass eine Eigenschaft einer Instanz von sein muss `ExtendedMembershipProvider`, der Standort kann nicht konfiguriert werden, um die ASP.NET Web Pages-Mitgliedschaftssystems (SimpleMembership) zu verwenden. Dies kann manchmal auftreten, wenn einem Hostinganbieter Server anders als die des lokalen Servers konfiguriert ist. Um dieses Problem zu beheben, fügen Sie das folgende Element auf der Website *"Web.config"* Datei:
   > 
   > [!code-xml[Main](16-adding-security-and-membership/samples/sample2.xml)]
   > 
   > Fügen Sie dieses Element als untergeordnetes Element der `<configuration>` Element sowie einen Peer der `<system.web>` Element.
9. Klicken Sie in der oberen rechten Ecke der Seite auf die **registrieren** Link. Die *Register.cshtml* angezeigt wird.
10. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**.

    ![Security-Mitgliedschaft-3](16-adding-security-and-membership/_static/image2.png)

    Beim Erstellen der Website der **Starter Site** Vorlage, eine Datenbank namens *StarterSite.sdf* erstellt wurde, in der Website *App\_Daten* Ordner. Während der Registrierung wird Ihre Benutzerinformationen der Datenbank hinzugefügt. Wenn Sie die SMTP-Werte festlegen, wird eine Nachricht an die e-Mail-Adresse gesendet, dass Sie verwendet werden, sodass Sie die Registrierung abschließen können.

    ![Security-Mitgliedschaft-4](16-adding-security-and-membership/_static/image3.png)
11. Wechseln Sie zu Ihr e-Mail-Programm, und suchen Sie die Nachricht, die Bestätigungscode und einen Link mit dem Standort haben wird.
12. Klicken Sie auf den Link, um Ihr Konto zu aktivieren. Der Link zur Bestätigung wird eine Bestätigungsseite für die Registrierung geöffnet.

    ![Security-Mitgliedschaft-5](16-adding-security-and-membership/_static/image4.png)
13. Klicken Sie auf die **Anmeldung** verknüpfen, und melden Sie sich mit dem Konto, das Sie registriert.

      Nachdem Sie sich angemeldet haben, die **Anmeldung** und **registrieren** Links werden ersetzt, indem eine **Logout** Link. Ihr Benutzername ist als Link angezeigt. (Den Link können Sie die zu einer Seite zu wechseln, in dem Sie Ihr Kennwort ändern.)

      ![Security-Mitgliedschaft-6](16-adding-security-and-membership/_static/image5.png)

      > [!NOTE]
      > Standardmäßig senden ASP.NET-Webseiten Anmeldeinformationen an den Server als Klartext (als Benutzer lesbarer Text). Eine Produktionswebsite sollten sicheres HTTP verwenden (https://, auch bekannt als die *secure Sockets Layer* oder SSL) zum Verschlüsseln von vertraulichen Informationen, die mit dem Server ausgetauscht werden. Können Sie die erforderlichen-e-Mail Nachrichten gesendet werden mithilfe von SSL durch Festlegen von `WebMail.EnableSsl=true` wie im vorherigen Beispiel. Weitere Informationen zu SSL finden Sie unter [Webkommunikation sichern: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660).

## <a name="additional-membership-functionality-in-the-site"></a>Zusätzliche Mitgliedschaftsfunktionen auf der Website

Ihre Site enthält andere Funktionen, die Benutzer ihre Konten verwalten kann. Benutzer können Folgendes tun:

- Ändern Sie ihrer Kennwörter. Nach der Anmeldung können sie den Benutzernamen klicken (das einen Link ist). Dadurch gelangen sie zu einer Seite, in dem sie ein neues Kennwort erstellen (*Account/ChangePassword.cshtml*).
- Wiederherstellen eines vergessenen Kennworts. Auf der Anmeldeseite, es ist ein Link (**haben Sie Ihr Kennwort vergessen?**) Benutzer zu einer Seite (*Account/ForgotPassword.cshtml*) in dem sie eine e-Mail-Adresse eingeben können. Der Standort sendet ihnen eine e-Mail-Nachricht, die einen Link, die sie klicken können enthält, um ein neues Kennwort festlegen (*Account/PasswordReset.cshtml*).

Sie können auch über die Benutzer melden sich mit einem externen Standort kann auch wie nachfolgend erläutert.

<a id="Creating_a_Members-Only_Page"></a>
## <a name="creating-a-members-only-page"></a>Erstellen einer Seite nur für Mitglieder verfügbar

Vorerst, kann alle Benutzer zu einer beliebigen Seite auf Ihrer Website durchsuchen. Jedoch empfiehlt es sich um Seiten zu erhalten, die nur für Personen verfügbar sind, die sich angemeldet haben (d. h. Mitglieder). ASP.NET können Sie die Seiten zu erstellen, die nur von Mitgliedern von angemeldeten zugegriffen werden kann. Wenn anonyme Benutzer versuchen, eine Seite nur Member zuzugreifen, leiten Sie sie in der Regel auf die Anmeldeseite.

In diesem Verfahren erstellen Sie einen Ordner aus, der Seiten enthält, die nur für angemeldete Benutzer verfügbar sind.

1. Erstellen Sie einen neuen Ordner im Stammverzeichnis der Website. (Klicken Sie im Menüband auf den Pfeil unterhalb **neu** und wählen Sie dann **neuer Ordner**.)
2. Nennen Sie diesen Ordner *Mitglieder*.
3. In der *Mitglieder* , erstellen Sie eine neue Seite und den Namen *MembersInformation.cshtml*.
4. Ersetzen Sie den vorhandenen Inhalt, mit dem folgenden Code und Markup:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample3.cshtml)]

    Dieser Code überprüft die `IsAuthenticated` Eigenschaft der `WebSecurity` Objekt, das zurückgegeben `true` , wenn der Benutzer angemeldet hat. Wenn der Benutzer nicht, in der Code ruft angemeldet ist `Response.Redirect` zum Senden von des Benutzers die *Login.cshtml* auf der Seite die *Konto* Ordner.

    Die URL der Umleitung enthält eine `returnUrl` Wert der Abfragezeichenfolge, die verwendet `Request.Url.LocalPath` den Pfad der aktuellen Seite fest. Setzen Sie die `returnUrl` Wert in der Abfragezeichenfolge wie folgt (und die Rückgabe-URL ist ein lokaler Pfad), die Anmeldeseite wird Benutzer zu dieser Seite zurückkehren, nachdem er sich anmeldet.

    Der Code legt auch fest  *\_SiteLayout.cshtml* Seite wie die Seite "Layout". (Weitere Informationen zu Layoutseiten, finden Sie unter [Erstellen eines konsistenten Layouts in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202891).)
5. Führen Sie die Website. Wenn Sie weiterhin angemeldet sind, klicken Sie auf die **Logout** Schaltfläche am oberen Rand der Seite.
6. Fordern Sie die Seite im Browser */Member/MembersInformation*. Die URL kann beispielsweise wie folgt aussehen:

    `http://localhost:38366/Members/MembersInformation`

    (Die Portnummer (38366) wird wahrscheinlich in Ihrer URL unterschiedlich sein.)

    Sie werden umgeleitet, um die *Login.cshtml* Seite, da Sie nicht, in angemeldet sind.
7. Melden Sie sich mit dem Konto an, die Sie zuvor erstellt haben. Sie werden umgeleitet, an die *MembersInformation* Seite. Da Sie angemeldet sind, sehen Sie diesmal den Seiteninhalt zu.

Um den Zugriff auf mehrere Seiten zu schützen, können Sie so vorgehen:

- Fügen Sie die sicherheitsüberprüfung auf jeder Seite hinzu.
- Erstellen Sie eine  *\_PageStart.cshtml* Seite in den Ordner, in dem auf der Sie geschützte Seiten beibehalten, und fügen es für die sicherheitsüberprüfung. Die  *\_PageStart.cshtml* -Seite fungiert als eine Art der Seite "globale" für alle Seiten im Ordner "". Diese Technik wird ausführlicher erläutert [anpassen standortweite Verhalten für ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906#Using__PageStart.cshtml_to_Restrict_Folder_Access).

## <a name="creating-security-for-groups-of-users-roles"></a>Erstellen von Sicherheit für Gruppen von Benutzern (Rollen)

Wenn Ihre Website sehr viele Mitglieder hat, ist es nicht effizienter, über die Berechtigung für jeden Benutzer einzeln überprüfen, bevor Sie eine Seite angezeigt werden können. Was Sie tun können stattdessen ist das Erstellen von Gruppen oder *Rollen*, dass die einzelnen Elemente angehören. Sie können dann Berechtigungen auf Grundlage der Rolle überprüfen. In diesem Abschnitt erstellen Sie eine &quot;Admin&quot; Rolle und erstellen Sie dann auf eine Seite, die für Benutzer zugänglich ist, die in (wer angehören) die Rolle.

Das ASP.NET-Mitgliedschaftssystem wird eingerichtet, um Rollen zu unterstützen. Anders als bei Mitgliedschaft Registrierung und Anmeldung die **Starter Site** Vorlage keine Seiten, mit denen Sie Rollen verwalten. (Verwalten von Rollen ist eine administrative Aufgabe anstatt einer Benutzeraufgabe.) Allerdings können Sie direkt in der Mitgliedschaftsdatenbank in WebMatrix Gruppen hinzufügen.

1. Klicken Sie in WebMatrix die **Datenbanken** arbeitsbereichsauswahl.
2. Öffnen Sie im linken Bereich die *StarterSite.sdf* Knoten, die **Tabellen** Knoten, und doppelklicken Sie dann auf die *Webseiten\_Rollen* Tabelle.

    ![Security-Mitgliedschaft-7](16-adding-security-and-membership/_static/image6.png)
3. Hinzufügen einer Rolle mit dem Namen &quot;Admin&quot;. Die *RoleId* Feld wird automatisch ausgefüllt. (Dabei handelt es sich der Primärschlüssel festgelegt wurde, werden ein Feld, und identifizieren Siehe [Einführung in die Arbeit mit einer Datenbank in ASP.NET Web Pages-Websites](https://go.microsoft.com/fwlink/?LinkId=202893).)
4. Notieren Sie sich, der der Wert für die *RoleId* Feld. (Ist dies die erste Rolle, die Sie definieren, wird 1 sein.)

    ![Security-Mitgliedschaft-8](16-adding-security-and-membership/_static/image7.png)
5. Schließen der *Webseiten\_Rollen* Tabelle.
6. Öffnen der *UserProfile* Tabelle.
7. Notieren Sie sich die *"UserID"* Wert von mindestens einem Benutzer in der Tabelle und schließen Sie dann in der Tabelle.
8. Öffnen der *Webseiten\_UserInRoles* Tabelle, und geben Sie einen *"UserID"* und *RoleID* Wert in der Tabelle. Um beispielsweise Benutzer 2 in der &quot;Admin&quot; Rolle geben Sie diese Werte:

    ![Security-Mitgliedschaft-9](16-adding-security-and-membership/_static/image8.png)
9. Schließen der *Webseiten\_UsersInRoles* Tabelle.

    Nun, da Sie Rollen definiert haben, können Sie eine Seite konfigurieren, die für Benutzer zugänglich ist, die in dieser Rolle sind.
10. Erstellen Sie in den Stammordner der Website eine neue Seite mit dem Namen *AdminError.cshtml* , und Ersetzen Sie den vorhandenen Inhalt durch den folgenden Code. Dies ist die Seite wird, der an Benutzer umgeleitet werden, wenn sie Zugriff auf eine Seite nicht gestattet ist.

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample4.cshtml)]
11. Erstellen Sie in den Stammordner der Website eine neue Seite mit dem Namen *AdminOnly.cshtml* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample5.cshtml)]

    Die `Roles.IsUserInRole` Methodenrückgabe `true` ist der aktuelle Benutzer ein Mitglied der angegebenen Rolle (in diesem Fall die Rolle "Admin").
12. Führen Sie *Default.cshtml* in einem Browser, aber nicht anmelden. (Wenn Sie bereits angemeldet sind, melden Sie sich an.)
13. Fügen Sie in der Adressleiste des Browsers *AdminOnly* in der URL. (Das heißt, fordern die *AdminOnly.cshtml* Datei.) Sie werden umgeleitet, um die *AdminError.cshtml* Seite, da Sie zurzeit als Benutzer angemeldet sind nicht die &quot;Admin&quot; Rolle.
14. Wechseln Sie zurück zur *Default.cshtml* und melden Sie sich als der Benutzer, die Sie hinzugefügt, um haben die &quot;Admin&quot; Rolle.
15. Navigieren Sie zu *AdminOnly.cshtml* Seite. Dieses Mal die Seite angezeigt.

## <a name="preventing-automated-programs-from-joining-your-website"></a>Verhindert, dass automatisierte Programme verknüpfen Ihre Website

Beendet die Anmeldeseite nicht automatisierte Programme (auch bezeichnet als *web-Roboter* oder *Bots*) beim Registrieren Ihrer Website. Diesem Verfahren wird beschrieben, wie Sie einen ReCaptcha-Test für die Registrierungsseite aktivieren.

![/Media/38777/ch16securitymembership-18.jpg](16-adding-security-and-membership/_static/image1.jpg)

1. Registrieren Sie Ihre Website in ReCaptcha.Net ([http://recaptcha.net](http://recaptcha.net)). Wenn Sie die Registrierung abgeschlossen haben, erhalten Sie einen öffentlichen Schlüssel und einen privaten Schlüssel.
2. Die ASP.NET Web Helpers Library zu Ihrer Website hinzufügen, wie in beschrieben [Hilfsprogramme installieren, in einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=252372), sofern Sie noch nicht geschehen.
3. In der *Konto* Ordner die Datei mit dem Namen *Register.cshtml*.
4. Suchen Sie die folgenden Zeilen im Code am oberen Rand der Seite, und kommentieren Sie sie durch das Entfernen der `//` Kommentarzeichen:

    [!code-csharp[Main](16-adding-security-and-membership/samples/sample6.cs)]
5. Ersetzen Sie dies `PRIVATE_KEY` durch Ihren eigenen privaten ReCaptcha-Schlüssel.
6. Entfernen Sie in das Markup der Seite, die `@*` und `*@` Kommentarzeichen aus, um die folgenden Zeilen im Markup Seite:

    [!code-cshtml[Main](16-adding-security-and-membership/samples/sample7.cshtml)]
7. Ersetzen Sie dies `PUBLIC_KEY` durch Ihren Schlüssel.
8. Wenn Sie es bereits entfernt nicht getan haben, entfernen die `<div>` Element, das Text, der enthält mit "Zum Aktivieren der CAPTCHA-Prüfung..." beginnt. (Entfernen Sie den gesamten `<div>` -Element und dessen Inhalt.)

9. Führen Sie *Default.cshtml* in einem Browser. Wenn Sie sich bei der Website angemeldet sind, klicken Sie auf die **Logout** Link.
10. Klicken Sie auf die **registrieren** verknüpfen und Testen Sie die Registrierung mit der CAPTCHA-Test.

     ![Security-Mitgliedschaft-10](16-adding-security-and-membership/_static/image9.png)

Weitere Informationen zu den `ReCaptcha` Helper, finden Sie unter [eine CATPCHA verwenden, um zu verhindern, dass automatisierten Programmen (Bots) aus mithilfe der ASP.NET Web Site](https://go.microsoft.com/fwlink/?LinkId=251967).

<a id="Additional_Resources"></a>
## <a name="letting-users-log-in-using-an-external-site"></a>Benutzer melden Sie sich mit einem externen Standort

Die **Starter Site** Vorlage enthält Code und Markup, mit dem Benutzer melden Sie sich mit Facebook, Windows Live, Twitter, Google und Yahoo. Standardmäßig ist diese Funktionalität nicht aktiviert. Das allgemeine Verfahren für die Verwendung von können Benutzer melden sich mit dieser externen Anbietern ist dies:

- Entscheiden Sie, welche von externen Websites unterstützt werden soll.
- Falls erforderlich, wechseln Sie zu diesem Standort aus, und richten Sie eine app für die Anmeldung. (Z. B. müssen Sie dazu, um Facebook-Logins zu ermöglichen.)
- Konfigurieren Sie auf der Website des Anbieters ein. In den meisten Fällen müssen einfach Kommentieren von Code in die  *\_AppStart.cshtml* Datei.
- Fügen Sie auf der Registrierungsseite aus, die Benutzer kann Markup Link zu der externen Website für die Anmeldung. Sie können das Markup in der Regel kopieren, das Sie benötigen, und ändern Sie den Text etwas.

Schrittweise Anweisungen finden Sie im Thema [Aktivieren der Anmeldung über externe Websites einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=251969).

Nachdem ein Benutzer von einer anderen Website anmeldet, wird der Benutzer auf Ihrer Website zurückkehrt und *ordnet* , melden Sie sich mit Ihrer Website. Aktiviert ist, erstellt dies eine Mitgliedschaftseintrag in Ihre Site für eine externe Anmeldung des Benutzers. So können Sie die normalen Funktionen der Mitgliedschaft (z. B. Rollen) mit der externen Anmeldung zu verwenden.

## <a name="adding-security-to-an-existing-website"></a>Sicherheit von einer vorhandenen Website

Das Verfahren weiter oben in diesem Artikel beruht auf der Verwendung der **Starter Site** Vorlage als Grundlage für die Website-Sicherheit. Wenn es nicht praktikabel, zum Starten von der **Starter Site** Vorlage oder um die relevanten Seiten von einer Website, auf Grundlage dieser Vorlage zu kopieren, können Sie die gleiche Art von Sicherheit auf der eigenen Website implementieren, indem Sie ihn selbst codieren. Sie erstellen die gleichen Arten der Seiten, Registrierung, Anmeldung und So weiter, und verwenden Sie Hilfsmethoden und Klassen zum Einrichten der Mitgliedschaft.

Das grundlegende Verfahren wird beschrieben, in dem Blogbeitrag [die einfachste Möglichkeit zum Implementieren der ASP.NET Razor-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240). Die meiste Arbeit erfolgt mithilfe der folgenden Methoden und Eigenschaften der `WebSecurity` Hilfsprogramm:

- [WebSecurty.UserExists](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.userexists(v=vs.99).aspx), [WebSecurity.CreateUserAndAccount](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.createuserandaccount(v=vs.99).aspx). Mit diesen Methoden können Sie bestimmen, ob jemand bereits registriert ist und sie zu registrieren.
- [WebSecurty.IsAuthenticated](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.isauthenticated(v=vs.99).aspx). Diese Eigenschaft können Sie bestimmen, ob der aktuelle Benutzer angemeldet ist. Dies ist nützlich, um Benutzer zu einer Anmeldeseite umgeleitet werden, wenn sie nicht bereits angemeldet haben.
- [WebSecurity.Login](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.login(v=vs.99).aspx), [WebSecurity.Logout](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.logout(v=vs.99).aspx). Diese Methoden meldet einen Benutzer aus, oder verkleinern.
- [WebSecurity.CurrentUserName](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity.currentusername(v=vs.99).aspx). Diese Eigenschaft ist nützlich für die Anzeige der Namen des aktuellen Benutzers angemeldet, (wenn der Benutzer angemeldet ist).
- [WebSecurity.ConfirmAccount](https://msdn.microsoft.com/library/gg569286(v=vs.99).aspx). Diese Methode ist nützlich, wenn Sie die e-Mail-Bestätigung für die Registrierung einrichten. (Details werden im Blogbeitrag beschrieben [mithilfe der Funktion zur ereignisbestätigung für ASP.NET Web Pages-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267).)

Um Rollen zu verwalten, können Sie die [Rollen](https://msdn.microsoft.com/library/gg538398(v=vs.99).aspx) und [Mitgliedschaft](https://msdn.microsoft.com/library/gg569035(v=vs.99).aspx) Klassen, wie in den Blogeintrag beschrieben.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Anpassen des Verhaltens von Websiteseiten](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Sichern von Web-Kommunikation: Zertifikate, SSL und https://](https://go.microsoft.com/fwlink/?LinkId=208660)
- [Die einfachste Möglichkeit zum Implementieren der ASP.NET Razor-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2240) und [mithilfe der Funktion zur ereignisbestätigung für ASP.NET Web Pages-Sicherheit](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2267). Hierbei handelt es sich um Blogbeiträge, die beschreiben, wie ASP.NET Membershipfeatures ohne implementiert die **Starter Site** Vorlage.
- [Aktivieren der Anmeldung über externe Websites einer ASP.NET Web Pages-Website](https://go.microsoft.com/fwlink/?LinkId=251969)
- [WebSecurity-Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.websecurity(v=vs.99)) (MSDN)
- [SimpleRoleProvider-Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.simpleroleprovider(v=vs.99)) (MSDN)
- [SimpleMembershipProvider-Klasse-API-Referenz](https://msdn.microsoft.com/library/webmatrix.webdata.simplemembershipprovider(v=vs.99)) (MSDN)
