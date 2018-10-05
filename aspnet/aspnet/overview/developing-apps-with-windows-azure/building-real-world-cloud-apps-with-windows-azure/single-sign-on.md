---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Einmaliges Anmelden (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 22ef4c2908783e513bfb6fb63364e71378cb8719
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578431"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Einmaliges Anmelden (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Es gibt viele Sicherheitsprobleme zu bedenken, wenn Sie eine Cloud-app entwickeln, aber für diese Reihe konzentrieren wir uns nur auf einem: einmaliges Anmelden. Eine Frage häufig die Frage gestellt, ist dies: "Ich bin in erster Linie Erstellen von apps für Mitarbeiter von meinem Unternehmen; Wie kann ich diese apps in der Cloud zu hosten und trotzdem aktivieren sie das gleiche Sicherheitsmodell verwenden, das meiner Mitarbeiter kennen und in der lokalen Umgebung verwenden, wenn sie apps, die ausgeführt werden gehostet werden innerhalb der Firewall?" Einer der Methoden, die wir dieses Szenarios aktivieren wird Azure Active Directory (Azure AD) aufgerufen. Azure AD ermöglicht Ihnen, die Enterprise-Line-of-Business-apps (LOB) über das Internet verfügbar machen, und sie können diese apps auch Geschäftspartner zur Verfügung stellen.

## <a name="introduction-to-azure-ad"></a>Einführung in Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) bietet [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in der Cloud. Die folgenden: Schlüsselfunktionen

- Es ist in einem lokalen Active Directory integriert.
- Sie können einmaliges Anmelden mit Ihren apps.
- Unterstützt offene Standards wie z. B. [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), und [OAuth 2.0](http://oauth.net/2/).
- Er unterstützt Unternehmen [Graph-REST-API](https://msdn.microsoft.com/library/hh974476.aspx).

Nehmen wir an, dass Sie eine lokalen Windows Server Active Directory-Umgebung verfügen, die Sie verwenden, um Mitarbeitern für die Anmeldung bei Intranet-apps zu ermöglichen:

![](single-sign-on/_static/image1.png)

Welche Azure AD ermöglicht Ihnen ist ein Verzeichnis in der Cloud zu erstellen. Es ist ein kostenloses Feature und einfach einrichten.

Es kann jedoch vollständig unabhängig von Ihrem lokalen Active Directory sein. Sie können jeder einfügen, es werden soll, und authentifizieren Sie sie in Internet-apps.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Oder Sie können es in Ihrer lokalen integrieren AD.

![Active Directory und WAAD-Integration](single-sign-on/_static/image3.png)

Alle Mitarbeiter, die auf die lokale Authentifizierung können können jetzt auch über das Internet – authentifizieren, ohne dass Sie eine Firewall öffnen oder neue Server in Ihrem Datencenter bereitstellen. Sie können weiterhin die vorhandene Active Directory-Umgebung nutzen, die Sie kennen und nutzen Ihre internen apps SSO-Funktion zu gewähren.

Nachdem Sie diese Verbindung zwischen AD und Azure AD vorgenommen haben, Sie auch Ihre Web-apps und Ihre mobilen Geräte Ihrer Mitarbeiter in der Cloud-Authentifizierung aktivieren und Drittanbieter-apps, wie z.B. Office 365, SalesForce.com oder Google apps können, akzeptiert können Ihre Anmeldeinformationen für Mitarbeiter. Wenn Sie Office 365 verwenden, sind Sie bereits in Azure AD eingerichtet, da Office 365 Azure AD für Authentifizierung und Autorisierung verwendet.

![3. apps von Drittanbietern](single-sign-on/_static/image4.png)

Der Vorteil dieses Ansatzes ist, die jedes Mal, die Ihrer Organisation hinzufügt oder löscht einen Benutzer oder ändert ein Benutzer ein Kennwort, Sie verwenden den gleichen Prozess, mit denen Sie noch heute in Ihrer lokalen Umgebung. Alle von Ihrem lokalen AD-Änderungen automatisch in der Cloudumgebung weitergegeben werden.

Wenn Ihr Unternehmen verwenden oder nach Office 365 verschieben, die gute Nachricht ist, müssen Sie Azure AD automatisch eingerichtet, da es sich bei Office 365 Azure AD für die Authentifizierung verwendet. Daher können Sie problemlos in Ihre Anwendungen die gleiche Authentifizierung verwenden, die Office 365 verwendet.

## <a name="set-up-an-azure-ad-tenant"></a>Einrichten von Azure AD-Mandanten

Azure AD-Verzeichnis wird aufgerufen, eine Azure AD [Mandanten](https://technet.microsoft.com/library/jj573650.aspx), und das Einrichten eines Mandanten ist recht einfach. Wir zeigen Ihnen wie er im Azure-Verwaltungsportal abgeschlossen ist, um die Konzepte zu veranschaulichen, aber natürlich wie die anderen Portal Funktionen Sie können auch dazu verwenden ein Skript oder die Verwaltungs-API.

Klicken Sie im Verwaltungsportal auf der Registerkarte "Active Directory".

![WAAD im portal](single-sign-on/_static/image5.png)

Haben Sie automatisch eine Azure AD-Mandanten für Azure-Konto, und klicken Sie auf die **hinzufügen** am unteren Seitenrand, um weitere Verzeichnisse erstellen. Eine für eine testumgebung und eine für die Produktion, empfiehlt z. B. Überlegen Sie sorgfältig wie Sie ein neues Verzeichnis benennen. Wenn Sie Ihren Namen für das Verzeichnis, und klicken Sie dann Sie Ihren Namen erneut für einen Benutzer verwenden, kann die verwirrend sein.

![Verzeichnis hinzufügen](single-sign-on/_static/image6.png)

Das Portal verfügt über vollständige Unterstützung für das Erstellen, löschen und Verwalten von Benutzern in dieser Umgebung. Z. B. hinzufügen ein Benutzers finden Sie unter den **Benutzer** Registerkarte, und klicken Sie auf die **Add User** Schaltfläche.

![Fügen Sie die Schaltfläche "Benutzer"](single-sign-on/_static/image7.png)

![Dialogfeld "Benutzer" hinzufügen](single-sign-on/_static/image8.png)

Können Sie einen neuen Benutzer, die vorhanden ist nur in diesem Verzeichnis erstellen, oder Sie können ein Microsoft-Account als einen Benutzer in diesem Verzeichnis oder die Registrierung oder Benutzer aus einem anderen Azure AD-Verzeichnis als einen Benutzer in diesem Verzeichnis registrieren. (In einem echten Verzeichnis die Standarddomäne ContosoTest.onmicrosoft.com wäre. Sie können auch eine Domäne Ihrer Wahl, z. B. "contoso.com" verwenden.)

![Benutzertypen](single-sign-on/_static/image9.png)

![Dialogfeld "Benutzer" hinzufügen](single-sign-on/_static/image10.png)

Sie können die Benutzer einer Rolle zuweisen.

![Benutzerprofil](single-sign-on/_static/image11.png)

Und das Konto wird erstellt, ein temporäres Kennwort.

![Temporäres Kennwort](single-sign-on/_static/image12.png)

Die Benutzer, die Sie auf diese Weise erstellen können sofort auf Ihre Web-apps, die mithilfe dieses Cloud-Verzeichnis anmelden.

Was sich hervorragend für einmaliges Anmelden für Unternehmen, jedoch ist die **Verzeichnisintegration** Registerkarte:

![Registerkarte "Directory Integration"](single-sign-on/_static/image13.png)

Wenn Sie den Directory-Integration aktivieren und [Laden Sie ein Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), können Sie mit Ihrem vorhandenen lokalen Active Directory, die Sie bereits in Ihrer Organisation verwenden dieses Cloud-Verzeichnis synchronisieren. Klicken Sie dann werden alle in Ihrem Verzeichnis gespeicherten Benutzer in dieser Cloud-Verzeichnis angezeigt. Ihre Cloud-apps können alle Mitarbeiter mit ihren vorhandenen Active Directory-Anmeldeinformationen authentifizieren. Und ist kostenlos – Synchronisierungstool und Azure AD abgemeldet.

Das Tool ist ein Assistent, der einfach zu verwenden, ist, wie Sie diese Screenshots sehen können. Diese sind nicht nur ein Beispiel, das grundlegende Verfahren zeigt eine vollständige Anleitung. How-to--It Informationen finden Sie unter den Links in der [Ressourcen](#resources) Abschnitt am Ende des Kapitels.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image14.png)

Klicken Sie auf **Weiter**, und geben Sie Ihre Azure Active Directory-Anmeldeinformationen.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image15.png)

Klicken Sie auf **Weiter**, und geben Sie dann auf Ihrem lokalen AD-Anmeldeinformationen.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image16.png)

Klicken Sie auf **Weiter**, und klicken Sie dann angeben, wenn Sie einen Hash der AD-Kennwörter in der Cloud speichern möchten.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image17.png)

Der Hashwert des Kennworts, die Sie in der Cloud speichern können, werden als unidirektionaler Hash; tatsächliche Kennwörter werden nie in Azure AD gespeichert. Wenn Sie sich entscheiden, für die Hashes in der Cloud speichern, müssen Sie mit [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Es gibt auch [andere Faktoren bedenken, wenn entscheiden, ob AD FS verwenden](https://technet.microsoft.com/library/jj573653.aspx). Die AD FS-Option erfordert einige zusätzliche Konfigurationsschritte erforderlich.

Wenn Sie Hashes in der Cloud zu speichern, Fertig, und das Tool Verzeichnisse synchronisieren startet, wenn Sie auf **Weiter**.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image18.png)

Und in wenigen Minuten sind Sie fertig.

![Konfigurations-Assistenten WAAD-Synchronisierungstools](single-sign-on/_static/image19.png)

Sie müssen sich nur auf einem Domänencontroller in der Organisation, die unter Windows 2003 oder höher ausführen. Und muss nicht neu gestartet. Wenn Sie fertig sind, man kann auch alle Ihre Benutzer in der Cloud sind-SSO von Web oder mobile Anwendung, die mithilfe von SAML, OAuth und WS-Fed.

– Gelegentlich erhalten wir gefragt, wie sicher Dies geht Microsoft verwendet diese für ihre eigenen sensible Geschäftsdaten? Und die Antwort ist Ja, die wir ausführen. Angenommen, Sie mit der internen Microsoft SharePoint-Website unter Fortfahren [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), erhalten Sie aufgefordert, sich anzumelden.

![Office 365-Anmeldung](single-sign-on/_static/image20.png)

Microsoft AD FS aktiviert hat, also wenn Sie eine Microsoft-ID eingeben, Sie auf eine AD FS-Anmeldeseite umgeleitet.

![AD FS-Anmeldung](single-sign-on/_static/image21.png)

Und sobald Sie in einem internen Microsoft-AD-Konto gespeicherte Anmeldeinformationen eingeben, haben Sie Zugriff auf diese internen Anwendung.

![MS-SharePoint-Website](single-sign-on/_static/image22.png)

Wir verwenden einen AD-anmelden-Server, hauptsächlich deshalb, weil es AD FS einrichten bereits, bevor Azure AD verfügbar ist, aber der Prozess melden Sie sich über Azure AD-Verzeichnis in der Cloud geht. Wir fügen unserer wichtige Dokumente, quellcodeverwaltung, Leistung-Management-Dateien, aufarbeiten und mehr in der Cloud und Verwenden dieser Lösung geschützt.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Erstellen einer ASP.NET-App aus, die Azure AD für einmaliges Anmelden verwendet

Visual Studio macht es wirklich einfach ist, erstellen Sie eine app, die Azure AD für einmaliges Anmelden, verwendet, wie Sie einige Screenshots sehen können.

Bei der Erstellung einer neuen ASP.NET-Anwendung, MVC oder Web Forms, ist die Standardauthentifizierungsmethode ASP.NET Identity. Um, die mit Azure AD zu ändern, Sie klicken Sie auf eine **Authentifizierung ändern** Schaltfläche.

![Authentifizierung ändern](single-sign-on/_static/image23.png)

Wählen Sie die Organisations-Konten, geben Sie Ihren Domänennamen ein, und wählen Sie dann auf einmaliges Anmelden.

![Dialogfeld "Authentifizierung" Konfigurieren](single-sign-on/_static/image24.png)

Sie können auch Geben Sie die app Lese- oder Lese-/Schreibberechtigung für Verzeichnisdaten. Wenn Sie dies tun, können sie die [Azure Graph-REST-API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) um die Telefonnummer des Benutzers zu suchen, erfahren Sie, wenn sie im Büro sind, wenn sie zuletzt, on usw. angemeldet.

Dies alles ist, müssen Sie Folgendes verwenden: Visual Studio fordert die Anmeldeinformationen für einen Administrator für Ihren Azure AD-Mandanten, und klicken Sie dann sowohl für das Projekt als auch für Azure AD-Mandanten für die neue Anwendung konfiguriert.

Wenn Sie das Projekt ausführen, sehen Sie eine Anmeldeseite angezeigt, und registrieren Sie sich die Anmeldeinformationen eines Benutzers in Azure AD-Verzeichnis.

![Organisations-Konto anmelden](single-sign-on/_static/image25.png)

![Angemeldet](single-sign-on/_static/image26.png)

Wenn Sie die app in Azure bereitstellen, müssen Sie, lediglich auf einen **Organisationsauthentifizierung aktivieren** Kontrollkästchen, und wieder Visual Studio übernimmt die Konfiguration für Sie.

![Web veröffentlichen](single-sign-on/_static/image27.png)

Diese Screenshots stammen von einer vollständigen ausführliches Tutorial, das zeigt, wie Sie eine app erstellen, die Azure AD-Authentifizierung verwendet: [Entwickeln von ASP.NET-Apps mit Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel haben Sie gesehen, dass es sich bei Azure Active Directory, ASP.NET und Visual Studio, um einmaliges Anmelden im Internet-Anwendungen für Benutzer in Ihrer Organisation einzurichten erleichtern. Ihre Benutzer können in Internet-apps, die mit denselben Anmeldeinformationen an, die sie verwenden, melden Sie sich mit Active Directory in Ihrem internen Netzwerk anmelden.

Die [im nächsten Kapitel](data-storage-options.md) untersucht die Datenspeicheroptionen für eine Cloud-app verfügbar.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

- [Dokumentation zu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Portalseite für Azure AD-Dokumentation auf der Website windowsazure.com. Schritt-für-Schritt-Tutorials finden Sie unter den **entwickeln** Abschnitt.
- [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/). Dokumentation zu Multi-Factor Authentication in Azure-Portalseite.
- [Organisations-Konto-Authentifizierungsoptionen](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Erläuterung der in das Visual Studio 2013-Dialogfeld "Neues Projekt" die Azure AD-Authentifizierungsoptionen.
- [Microsoft Patterns and Practices - Federated Identity Muster](https://msdn.microsoft.com/library/dn589790.aspx).
- [So wird's gemacht: Installieren Sie das Azure Active Directory Sync-Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 Inhaltszuordnung](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Enthält Links zu Dokumentation zu AD FS 2.0.
- [Rollen- und ACL-basierte Autorisierung in einer Windows Azure AD-Anwendung](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Die beispielanwendung.
- [Azure Active Directory Graph-API-Blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Zugriffssteuerung in BYOD und Directory-Integration in einer Hybrid-Identitätsinfrastruktur](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014-Sitzung video Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Zurück](web-development-best-practices.md)
> [Weiter](data-storage-options.md)
