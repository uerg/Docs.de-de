---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Einmaliges Anmelden (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 82f2f99154d94074b03d580a0f491053d6f53bde
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873884"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Einmaliges Anmelden (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Es gibt viele Sicherheitsprobleme zu bedenken, wenn Sie ein Cloud-app entwickeln, aber für diese Reihe konzentrieren wir uns auf nur einem: einmaliges Anmelden. Eine Frage, die Benutzer in den häufigsten Fragen ist dies: "Ich bin in erster Linie Erstellen von apps für die Mitarbeiter des Unternehmens; Gewusst wie ich diese apps in der Cloud zu hosten und weiterhin ermöglichen, die auf das gleiche Sicherheitsmodell verwendet, das eigene Mitarbeiter kennen, und in einer lokalen Umgebung verwenden, wenn sie apps, die ausgeführt werden gehostet werden innerhalb der Firewall?" Einer der Methoden, die wir dieses Szenario aktivieren wird Azure Active Directory (Azure AD) aufgerufen. Azure AD ermöglicht es Ihnen, Enterprise Line-of-Business (LOB)-apps über das Internet verfügbar machen, und erlaubt es Ihnen, diese apps auch Geschäftspartnern zur Verfügung stellen.

## <a name="introduction-to-azure-ad"></a>Einführung in Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) bietet [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in der Cloud. Die folgenden: Hauptfunktionen

- In einer lokalen Active Directory integriert.
- Sie können einmaliges Anmelden bei Ihren apps.
- Es unterstützt offener Standards, z. B. [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), und [OAuth 2.0](http://oauth.net/2/).
- Enterprise unterstützt [Graph-REST-API](https://msdn.microsoft.com/library/hh974476.aspx).

Nehmen wir an, dass Sie eine lokale Windows Server Active Directory-Umgebung verfügen, mit denen Sie Mitarbeiter Intranet apps anmelden zu aktivieren:

![](single-sign-on/_static/image1.png)

Welche Azure AD ist, können Sie ist ein Verzeichnis in der Cloud zu erstellen. Es ist ein kostenloses Feature und einfach einrichten.

Es kann vollständig unabhängig von Ihrem lokalen Active Directory sein. Sie können jeder einfügen, darin werden soll, und im Internet-apps zu authentifizieren.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Oder Sie können in integrieren, mit dem lokalen AD.

![AD und WAAD-Integration](single-sign-on/_static/image3.png)

Alle Mitarbeiter, die lokal authentifiziert werden können, können jetzt auch über das Internet – authentifizieren, ohne dass Sie eine Firewall zu öffnen oder neue Server in Ihrem Datencenter bereitstellen. Sie können weiterhin die vorhandene Active Directory-Umgebung nutzen, die Sie kennen und heute verwenden, um Ihre internen apps einmaligem Anmelden auf Basis zu erteilen.

Sobald Sie haben diese Verbindung zwischen AD und Azure AD vorgenommen, Sie auch Ihre Web-apps und Ihre mobilen Geräte Ihrer Mitarbeiter in der Cloud-Authentifizierung aktivieren und Drittanbieter-apps wie Office 365, SalesForce.com oder Google apps können akzeptieren kann Ihre Anmeldeinformationen für Mitarbeiter. Wenn Sie Office 365 verwenden, sind Sie bereits bei Azure AD eingerichtet, da Office 365, Azure AD für die Authentifizierung und Autorisierung verwendet.

![3. apps von Drittanbietern](single-sign-on/_static/image4.png)

Der Vorteil dieses Ansatzes ist, die einem beliebigen Zeitpunkt Ihre Organisation hinzufügt oder löscht einen Benutzer oder ändert ein Benutzer ein Kennwort, Sie verwenden den gleichen Prozess, den Sie heute in Ihrer lokalen Umgebung verwenden. Alle von Ihrem lokalen AD Änderungen automatisch in die Cloud-Umgebung weitergegeben werden.

Wenn Ihr Unternehmen oder das Verschieben in Office 365, die gute Nachricht ist, müssen Sie Azure AD automatisch eingerichtet werden, da Office 365, Azure AD für die Authentifizierung verwendet. Daher können Sie problemlos in Ihre eigenen apps die gleiche Authentifizierung verwenden, die Office 365 verwendet.

## <a name="set-up-an-azure-ad-tenant"></a>Einrichten von Azure AD-Mandanten

Azure AD-Verzeichnis wird aufgerufen, ein Azure AD [Mandanten](https://technet.microsoft.com/library/jj573650.aspx), und das Einrichten eines Mandanten ist recht einfach. Wir zeigen Ihnen wie es im Azure-Verwaltungsportal ausgeführt wird, um die Konzepte veranschaulicht, aber natürlich wie die anderen Portal Funktionen auch durchführen mithilfe eines Skripts oder die Verwaltungs-API.

Klicken Sie im Verwaltungsportal auf der Registerkarte "Active Directory".

![WAAD-Portal](single-sign-on/_static/image5.png)

Automatisch ein Azure AD-Mandanten für Ihre Azure-Konto verfügen, und klicken Sie auf die **hinzufügen** am unteren Rand der Seite, um weitere Verzeichnisse erstellen. Eine für eine testumgebung und eine für die Produktion, empfiehlt z. B. Überlegen Sie sorgfältig Sie ein neues Verzeichnis benennen. Wenn Sie die Namen für das Verzeichnis verwenden, und klicken Sie dann Ihren Namen erneut für die ein Benutzer verwenden, kann die verwirrend sein.

![Verzeichnis hinzufügen](single-sign-on/_static/image6.png)

Das Portal verfügt über vollständige Unterstützung für das Erstellen, löschen und Verwalten von Benutzern in dieser Umgebung. Z. B. hinzufügen ein Benutzers wechseln Sie zu der **Benutzer** Registerkarte, und klicken Sie auf die **Add User** Schaltfläche.

![Benutzer-Schaltfläche "hinzufügen"](single-sign-on/_static/image7.png)

![Benutzerdialogfeld "hinzufügen"](single-sign-on/_static/image8.png)

Können Sie einen neuen Benutzer vorhanden ist nur in diesem Verzeichnis erstellen, oder Sie können eine Microsoft-Account als ein Benutzer in dieses Verzeichnis oder die Registrierung oder einen Benutzer aus einem anderen Azure AD-Verzeichnis als ein Benutzer in diesem Verzeichnis registrieren. (In einem realen Verzeichnis als Standarddomäne ContosoTest.onmicrosoft.com wäre. Sie können auch eine Domäne Ihrer Wahl, z. B. "contoso.com" verwenden.)

![Benutzertypen](single-sign-on/_static/image9.png)

![Benutzerdialogfeld "hinzufügen"](single-sign-on/_static/image10.png)

Sie können die Benutzer einer Rolle zuweisen.

![Benutzerprofil](single-sign-on/_static/image11.png)

Und das Konto wird ein temporäres Kennwort erstellt.

![Vorübergehendes Kennwort](single-sign-on/_static/image12.png)

Die Benutzer, die Sie auf diese Weise erstellen, können sofort auf Ihre Web-apps, die mit dieser Cloud-Verzeichnis anmelden.

Was sich hervorragend für einmaliges Anmelden für Unternehmen, allerdings ist die **Verzeichnisintegration** Registerkarte:

![Registerkarte "Verzeichnis Integration"](single-sign-on/_static/image13.png)

Wenn Sie die Directory-Integration aktivieren und [herunterladen ein Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), diesen Cloud-Verzeichnis mit Ihrem vorhandenen lokalen Active Directory, die Sie bereits innerhalb Ihrer Organisation verwenden, synchronisiert werden kann. Dann werden alle Benutzer in Ihrem Verzeichnis gespeichert in dieser Cloud-Verzeichnis angezeigt. Ihre Cloud-apps können jetzt alle Mitarbeiter mit ihren vorhandenen Active Directory-Anmeldeinformationen zu authentifizieren. Und all dies ist kostenlos – Synchronisierungstool und Azure Active Directory selbst.

Das Tool ist ein Assistent, der einfach zu verwenden, ist, wie Sie in diese Screenshots sehen können. Dies sind keine umfassende, nur um ein Beispiel der grundlegende Prozess angezeigt. Ausführlichere Vorgehensweise-to--IT-Abteilung finden Sie unter den Links in der [Ressourcen](#resources) Abschnitt am Ende des Kapitels.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image14.png)

Klicken Sie auf **Weiter**, und geben Sie Ihre Azure Active Directory-Anmeldeinformationen.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image15.png)

Klicken Sie auf **Weiter**, und geben Sie dann mit die lokalen AD-Anmeldeinformationen.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image16.png)

Klicken Sie auf **Weiter**, und klicken Sie dann an, ob es sich bei einen Hash des AD-Kennwörter in der Cloud gespeichert werden soll.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image17.png)

Der Hashwert des Kennworts, die in der Cloud gespeichert werden können, werden als unidirektionaler Hash; tatsächliche Kennwörter werden nie in Azure AD gespeichert. Wenn Sie gegen Hashes in der Cloud speichern möchten, stehen Ihnen verwenden [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Es gibt auch [Weitere zu berücksichtigenden Faktoren entscheiden, ob ein Verwenden von AD FS](https://technet.microsoft.com/library/jj573653.aspx). Die AD FS-Option erfordert einige zusätzliche Konfigurationsschritte erforderlich.

Wenn Sie fertig sind,-Hashes in der Cloud gespeichert werden sollen und das Tool Verzeichnisse synchronisieren startet, wenn Sie auf **Weiter**.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image18.png)

Und in wenigen Minuten Sie fertig sind.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image19.png)

Sie müssen sich nur auf einem Domänencontroller in der Organisation, die unter Windows 2003 oder höher ausführen. Und keine Notwendigkeit, neu zu starten. Wenn Sie fertig sind, alle Ihre Benutzer in der Cloud sind und Sie erreichen einmaliges Anmelden aus beliebigen Web oder mobile Anwendung, die mithilfe von SAML, OAuth oder WS-Fed.

– In einigen Fällen erhalten wir aufgefordert, zu wie sicheren sieht Microsoft verwendet diese für ihre eigenen vertraulichen Geschäftsdaten? Und die Antwort ist Ja, wir erledigen. Angenommen, Sie die interne Microsoft SharePoint-Website unter Fortfahren [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), erhalten Sie zum Anmelden aufgefordert.

![Office 365-Anmeldung](single-sign-on/_static/image20.png)

Microsoft hat AD FS, aktiviert, wenn Sie eine Microsoft-ID eingeben, um ein AD FS-Anmeldeseite umgeleitet abrufen.

![AD FS-Anmeldung](single-sign-on/_static/image21.png)

Und nach der Eingabe von Anmeldeinformationen auf ein internes Microsoft AD-Konto gespeichert sind, haben Sie Zugriff auf diese internen Anwendung.

![MS-SharePoint-Website](single-sign-on/_static/image22.png)

Wir verwenden einen AD-Server vor allem, da wir AD FS einrichten bereits, bevor Azure AD ist verfügbar seit, aber die Log-Vorgang ist ein Azure AD-Verzeichnis, in der Cloud durchlaufen. Wir werden unsere wichtige Dokumente, Datenquellen-Steuerelements, Leistung Verwaltungsdateien, Verkaufsberichte und mehr in der Cloud und diese genau derselben Projektmappe Sicherheit verwenden.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Erstellen einer ASP.NET app, die Azure AD für einmaliges Anmelden verwendet.

Visual Studio erleichtert wirklich so erstellen eine app, die Azure AD für einmaliges Anmelden verwendet, wie Sie in wenigen Screenshots sehen können.

Bei der Erstellung einer neuen ASP.NET-Anwendung MVC oder Web Forms, ist die Standardauthentifizierungsmethode ASP.NET Identity. Um, die in Azure AD zu ändern, klicken Sie Sie auf eine **ändern Authentifizierung** Schaltfläche.

![Authentifizierung ändern](single-sign-on/_static/image23.png)

Wählen Sie Organisationskonten, geben Sie Ihren Domänennamen ein, und wählen Sie einmaliges Anmelden.

![Authentifizierung-Dialogfeld "konfigurieren"](single-sign-on/_static/image24.png)

Sie können auch Geben Sie die app Lese- oder Lese-/Schreibberechtigung für die Verzeichnisdaten. Wenn Sie dies tun, können sie die [Azure Graph-REST-API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) zum Nachschlagen der Telefonnummer des Benutzers herausfinden, ob sie im Büro sind, wenn sie zuletzt usw. angemeldet.

Dies alles ist, was - Visual Studio fordert die Anmeldeinformationen für einen Administrator für Ihren Azure AD-Mandanten, und klicken Sie dann das Projekt und Azure AD-Mandanten für die neue Anwendung konfiguriert.

Wenn Sie das Projekt ausführen, sehen Sie eine Anmeldeseite, und Sie können melden Sie sich mit Anmeldeinformationen eines Benutzers in Ihrem Azure AD-Verzeichnis.

![Organisations-Konto anmelden](single-sign-on/_static/image25.png)

![Angemeldet](single-sign-on/_static/image26.png)

Wenn Sie die app in Azure bereitstellen, müssen Sie lediglich ausgewählt ist ein **Organisations-Authentifizierung aktivieren** Kontrollkästchen und noch einmal Visual Studio übernimmt die gesamte Konfiguration für Sie.

![Web veröffentlichen](single-sign-on/_static/image27.png)

Diese Screenshots stammen aus einer vollständigen schrittweises Lernprogramm, das zeigt, wie eine Anwendung erstellen, die Azure AD-Authentifizierung verwendet: [ASP.NET-Apps entwickeln, mit Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel haben Sie gesehen, dass Azure Active Directory, Visual Studio und ASP.NET verwenden, um einmaliges Anmelden für im Internet-Anwendungen für Benutzer in Ihrer Organisation einzurichten erleichtern. Ihre Benutzer können im Internet-apps, die mit denselben Anmeldeinformationen an, die sie verwenden, für die Anmeldung bei der Verwendung von Active Directory in Ihrem internen Netzwerk anmelden.

Die [nächsten Kapitels](data-storage-options.md) prüft die Datenspeicheroptionen für eine Cloud-app verfügbar.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Azure Active Directory-Dokumentation](https://docs.microsoft.com/azure/active-directory/). Portalseite zum Azure AD-Dokumentation auf der Website windowsazure.com. Schritt-für-Schritt-Lernprogramme finden Sie unter der **entwickeln** Abschnitt.
- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Portalseite Dokumentation zu Multi-Factor Authentication in Azure.
- [Organisations-Konto-Authentifizierungsoptionen](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Erläuterung der Azure AD-Authentifizierungsoptionen im der Visual Studio 2013-Dialogfeld "Neues Projekt".
- [Microsoft Patterns and Practices - Federated Identity Muster](https://msdn.microsoft.com/library/dn589790.aspx).
- [So wird's gemacht: Installieren der Azure Active Directory-Synchronisierungstool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory-Verbunddienste 2.0-Inhalten](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Links in der Dokumentation zu AD FS 2.0.
- [Rollen- und ACL-basierte Autorisierung in einer Windows Azure AD-Anwendung](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Die beispielanwendung.
- [Azure Active Directory Graph-API-Blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Zugriffssteuerung in BYOD und Directory-Integration in einer Hybrid-Identitätsinfrastruktur](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Eduard 2014 Sitzung video von Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Zurück](web-development-best-practices.md)
> [Weiter](data-storage-options.md)
