---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurieren einer Website, die Anwendungsdienste (VB) | Microsoft Docs
author: rick-anderson
description: "ASP.NET-Version 2.0 eingeführt, eine Reihe von Application-Dienste, die Teil von .NET Framework und dienen als eine Sammlung von Baustein Handelsversion-SQL..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 5f908eb6c6b2d18c6c41870a38bb618737949b0a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurieren einer Website, die Anwendungsdienste (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET-Version 2.0 eingeführt, eine Reihe von Application-Dienste, die Teil von .NET Framework und dienen als eine Sammlung von Baustein-Dienste, die Sie verwenden können, Ihre Webanwendung umfangreiche Funktionen hinzu. In diesem Lernprogramm wird untersucht, wie eine Website in der produktionsumgebung verwenden Sie Application-Dienste zu konfigurieren und allgemeine Probleme mit der Verwaltung von Benutzerkonten und Rollen in der produktionsumgebung behandelt.


## <a name="introduction"></a>Einführung

ASP.NET-Version 2.0 eingeführt, eine Reihe von *Anwendungsdienste*, die Teil von .NET Framework und dienen als eine Sammlung von Baustein Dienste können Sie umfangreiche Funktionen für Ihre Webanwendung hinzufügen. Application-Dienste umfassen:

- **Mitgliedschaft** – eine API zum Erstellen und Verwalten von Benutzerkonten.
- **Rollen** – eine API zum Kategorisieren von Benutzern in Gruppen.
- **Profil** – eine API für benutzerdefinierte, benutzerspezifische Sprachen gespeichert.
- **Siteübersicht** – eine API zum Definieren einer logischen Standort s-Struktur in Form einer Hierarchie, die dann über Steuerelemente zur Seitennavigation, z. B. Menüs und praktische Brotkrümelnavigation angezeigt werden können.
- **Personalisierung** – eine API für die Aufrechterhaltung der Anpassung Voreinstellungen, die am häufigsten mit [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Systemüberwachung** – eine API für die Überwachung der Leistung, Sicherheit, Fehler und andere System integritätsmetriken für eine ausgeführte Webanwendung.
  

Die Anwendungsdienste APIs sind nicht auf eine bestimmte Implementierung gebunden. Stattdessen weisen Sie die Anwendungsdienste für eine bestimmte *Anbieter*, und dieser Anbieter implementiert den Dienst mit einer bestimmten Technologie. Die am häufigsten verwendeten Anbieter für internetbasierte Webanwendungen, die auf einen Webhostinganbieter gehostet sind diese Anbieter unterstützen, die eine Implementierung der SQL Server-Datenbank verwenden. Z. B. die `SqlMembershipProvider` ist ein Anbieter für die Mitgliedschaft-API, die Benutzerkontoinformationen in einer Microsoft SQL Server-Datenbank speichert.

Unter Verwendung des Application-Dienste und Anbieter für SQL Server fügt eine Herausforderung dar, beim Bereitstellen der Anwendung. Zunächst müssen die Anwendung Dienste Datenbankobjekte ordnungsgemäß für die Entwicklungs- und produktionsumgebungen Datenbanken erstellt und ordnungsgemäß initialisiert. Es gibt auch wichtige Konfigurationseinstellungen, die vorgenommen werden müssen.

> [!NOTE]
> Die Anwendungsdienste APIs entworfen wurden, mithilfe der [ *Anbietermodell*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), ein Entwurfsmuster, die für eine API s Implementierungsdetails zur Laufzeit zur Verfügung gestellt werden kann. .NET Framework ausgeliefert wird, mit einer Anzahl von Anwendungsdienstanbieter, z. B. verwendet werden kann, die die `SqlMembershipProvider` und `SqlRoleProvider`, welche Anbieter für die Mitgliedschaft und Rollen-APIs, die ein SQL Server Datenbank-Implementierung. Sie können auch erstellen und -Plug-in einen benutzerdefinierten Anbieter. In der Tat die Buch Reviews Webanwendung enthält bereits einen benutzerdefinierten Anbieter für die Website-Map-API (`ReviewSiteMapProvider`), die aus den Daten in die Siteübersicht erstellt die `Genres` und `Books` Tabellen in der Datenbank.


In diesem Lernprogramm beginnt mit einem Blick auf wie ich die Webanwendung Buch überprüft die Mitgliedschaft und Rollen-APIs verwenden erweitert. Führt dann durch Bereitstellen einer Webanwendung, die Anwendungsdienste mit einer Implementierung der SQL Server-Datenbank verwendet, und behandeln Sie häufige Probleme mit der Verwaltung von Benutzerkonten und Rollen in der produktionsumgebung endet.

## <a name="updates-to-the-book-reviews-application"></a>Anwendungsupdates Buch Reviews

Führen Sie die letzen Lernprogramme, an die Webanwendung Buch überprüft für eine dynamische, datengesteuerte Webanwendung aus einer statischen Website aktualisiert wurde mit einem Satz von Verwaltungsseiten für die Verwaltung von Genres und Berichte. Allerdings dieser Bereich "Verwaltung" ist zurzeit nicht geschützt: jeder Benutzer, der die URL der Administration weiß (oder errät) kann Walzer und erstellen, bearbeiten oder Löschen des Reviews auf unserer Website. Eine gängige Methode zum Schutz von bestimmte Teile einer Website besteht darin Benutzerkonten zu implementieren und verwenden Sie dann die URL-Autorisierungsregeln, um den Zugriff auf bestimmte Benutzer oder Rollen einschränken. Das Buch Reviews-Web-Anwendung zum Download für dieses Lernprogramm unterstützt Benutzerkonten und Rollen. Es wurde eine einzelne Rolle definiert, mit dem Namen Admin und nur Benutzer in dieser Rolle können die Verwaltungsseiten zugreifen.

> [!NOTE]
> Ich gibt es drei Benutzerkonten erstellt, in der Webanwendung Buch Reviews: Scott Jisun und Alice. Alle drei Benutzer haben das gleiche Kennwort: **Kennwort!** Scott und Jisun befinden sich in der Rolle "Admin", Alice nicht. Die Website s nicht Administration-Seiten sind weiterhin für anonyme Benutzer verfügbar. D. h., Sie müssen nicht anmelden, um die Website, es sei denn, es, verwaltet werden soll in diesem Fall Sie in der Rolle "Admin" als Benutzer anmelden müssen.


Das Buch Reviews s Anwendungsmasterseite wurde aktualisiert, um eine andere Benutzeroberfläche für authentifizierte und anonyme Benutzer gehören. Wenn ein anonymer Benutzer die Website sieht einen Anmeldelink in der oberen rechten Ecke besucht. Ein authentifizierter Benutzer sieht die Nachricht "Willkommen zurück, *Benutzername*!" sowie einen Link zum Abmelden. Dort s auch eine Anmeldeseite (`~/Login.aspx`), enthält ein Anmeldename-Websteuerelement, die die Benutzeroberfläche und die Logik für die Authentifizierung eines Besuchers bereitstellt. Nur Administratoren können neue Konten erstellen. (Handelt es sich zum Erstellen und Verwalten von Benutzerkonten in Seiten der `~/Admin` Ordner.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurieren die Mitgliedschaft und Rollen-APIs

Die Webanwendung Buch Reviews verwendet die Mitgliedschaft und Rollen APIs zur Unterstützung von Benutzerkonten und zum Gruppieren von Benutzern in Rollen (d. h., der Admin-Rolle). Die `SqlMembershipProvider` und `SqlRoleProvider` -Anbieterklassen werden verwendet, da wir Konto-und Rolle in einer SQL Server-Datenbank gespeichert werden soll.

> [!NOTE]
> Dieses Lernprogramm richtet sich nicht um eine detaillierte Prüfung zur Konfiguration einer Webanwendung zur Unterstützung der Mitgliedschaft und Rollen APIs werden. Ein gründlicher Betrachtung der diese APIs und die Schritte, die Sie ausführen, so konfigurieren Sie eine Website, um sie verwenden müssen, lesen Sie meine [ *Website Sicherheit Lernprogramme*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Um die Anwendungsdienste mit einer SQL Server-Datenbank verwenden müssen Sie zuerst hinzufügen Datenbankobjekte verwendet von diesen Anbietern an die Datenbank, in dem Sie das Benutzerkonto sowie die Rolleninformationen gespeichert. Diese erforderlichen Datenbankobjekte enthalten eine Vielzahl von Tabellen, Sichten und gespeicherte Prozeduren. Sofern nicht anders angegeben die `SqlMembershipProvider` und `SqlRoleProvider` -Anbieterklassen verwenden eine SQL Server Express Edition-Datenbank mit dem Namen `ASPNETDB` befindet sich in der Anwendung s `App_Data` Ordner; Wenn eine Datenbank nicht vorhanden ist, wird es automatisch erstellt mit den erforderlichen Datenbankobjekte von diesen Anbietern zur Laufzeit.

Es ist möglich, und in der Regel optimal, zum Erstellen die Anwendung Dienste Datenbankobjekte in der gleichen Datenbank, in dem die Website s anwendungsspezifische Daten gespeichert ist. .NET Framework ausgeliefert wird, ein Tool, mit dem Namen `aspnet_regsql.exe` installiert, die die Datenbankobjekte in einer angegebenen Datenbank. Ich habe fehlend fort und verwendet dieses Tool diese Objekte zum Hinzufügen der `Reviews.mdf` -Datenbank in die `App_Data` Ordner (der Entwicklungsdatenbank). Wir sehen, wie Sie dieses Tool weiter unten in diesem Lernprogramm verwenden, wenn wir diese Objekte in der Produktionsdatenbank hinzufügen.

Wenn Sie die Anwendung Dienste Datenbankobjekte in einer Datenbank außer hinzufügen `ASPNETDB` müssen Sie zum Anpassen der `SqlMembershipProvider` und `SqlRoleProvider` Anbieter Klassen Konfigurationen aus, sodass sie die entsprechende Datenbank verwenden. Zum Anpassen der Mitgliedschaftsanbieter Hinzufügen einer [  *&lt;Mitgliedschaft&gt; Element* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) innerhalb der `<system.web>` im Abschnitt `Web.config`; verwenden Sie die [  *&lt;RoleManager&gt; Element* ](https://msdn.microsoft.com/library/ms164660.aspx) zum Konfigurieren des Rollen-Anbieters. Der folgende Codeausschnitt stammt aus dem Buch Reviews Anwendung s `Web.config` und zeigt die Einstellungen für die Mitgliedschaft und Rollen-APIs konfigurieren. Beachten Sie, dass beide registrieren Sie einen neuen Anbieter - `ReviewMembership` und `ReviewRole` -, bei denen die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter bzw.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Die `Web.config` Datei "s" `<authentication>` Element auch zur Unterstützung der formularbasierte Authentifizierung konfiguriert wurde.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Beschränken des Zugriffs auf den Verwaltungsseiten

ASP.NET erleichtert das gewähren oder Verweigern des Zugriffs auf eine bestimmte Datei oder Ordner nach Benutzer oder Rolle über seine *URL-Autorisierung* Funktion. (Wir kurz erläutert, URL-Autorisierung in der *Core Unterschiede zwischen IIS und ASP.NET Development Server* Lernprogramm und wurde gezeigt, wie IIS und ASP.NET Development Server URL-Autorisierungsregeln für statische unterschiedlich anwenden im Vergleich zu dynamischen Inhalt.) Da soll verhindert werden soll den Zugriff auf die `~/Admin` Ordner mit Ausnahme von dieser Benutzer in der Rolle "Admin" URL-Autorisierungsregeln zu diesem Ordner hinzufügen müssen. Insbesondere sollten die URL-Autorisierungsregeln Benutzer in der Rolle "Admin" zulassen und verweigern alle anderen Benutzer. Dies geschieht durch Hinzufügen einer `Web.config` Datei wird in der `~/Admin` Ordner mit dem folgenden Inhalt:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Für Weitere Informationen zu ASP.NET s URL Authorization-Feature und wie Sie es verwenden, um Autorisierungsregeln für Benutzer und Rollen, ausgedrückt werden, lesen Sie die [ *Benutzerbasierte Autorisierung* ](../../older-versions-security/membership/user-based-authorization-vb.md) und [ *Rollenbasierte Autorisierung* ](../../older-versions-security/roles/role-based-authorization-vb.md) Lernprogramme unter meinem [ *Website Sicherheit Lernprogramme*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Bereitstellen einer Webanwendung, die Anwendungsdienste verwendet

Wenn Sie eine Website bereitstellen, die Anwendungsdienste und einen Anbieter, der die Anwendungsinformationen von Services in einer Datenbank speichert, ist es unabdinglich, die Datenbankobjekte, die erforderlich sind, durch die Anwendungsdienste in der Produktionsdatenbank erstellt werden. Die Produktionsdatenbank enthält anfänglich keine diese Objekte können daher bei der Bereitstellung der Anwendung (oder bei der Bereitstellung zum ersten Mal nach Application-Dienste hinzugefügt wurden), Sie müssen zusätzliche Schritte abzurufenden diese erforderlichen Datenbankobjekte, auf die die Produktionsdatenbank.

Eine weitere Herausforderung kann auftreten, wenn Sie eine Website bereitstellen, die Anwendungsdienste verwendet werden, wenn Sie beabsichtigen, die in der Entwicklungsumgebung in die produktionsumgebung erstellten Benutzerkonten zu replizieren. Abhängig von der Konfiguration Mitgliedschaft und Rollen ist es möglich, dass diese Benutzer auch, wenn Sie die Benutzerkonten, die in der Entwicklungsumgebung in der Produktionsdatenbank erstellt wurden erfolgreich kopieren, in der Webanwendung in Produktion anmelden können. Wir sehen Sie sich die Ursache dieses Problems und erläutert, um es zu vermeiden.

ASP.NET im Lieferumfang eine hübsche [ *Web Site Administration Tool (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , die von Visual Studio gestartet werden kann, und ermöglicht dem Benutzer Konto, Rollen und Autorisierung Regeln, die über eine webbasierte verwaltet werden -Schnittstelle. Leider funktioniert die WSAT nur für lokale Websites, was bedeutet, dass die Remoteverwaltung von Benutzerkonten, Rollen und Autorisierungsregeln für die Webanwendung in der produktionsumgebung verwendet werden kann. Sehen wir uns die verschiedenen Möglichkeiten zum Implementieren von WSAT-ähnliches Verhalten über Ihre Produktionswebsite.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Hinzufügen der Datenbank Objekte mithilfe von Aspnet\_regsql.exe

Die *Bereitstellen einer Datenbank* Lernprogramm wurde gezeigt, wie Tabellen und Daten aus der Entwicklungsdatenbank in der Produktionsdatenbank kopieren und diese Techniken können sicherlich verwendet werden, um die Anwendung Dienste-Datenbankobjekte, die die Produktionsdatenbank. Eine andere Möglichkeit ist die `aspnet_regsql.exe` Tool, das hinzugefügt oder entfernt die Anwendung Dienste Datenbankobjekte aus einer Datenbank.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool erstellt die Datenbankobjekte in einer angegebenen Datenbank. Es werden keine Daten in die Datenbankobjekte aus der Entwicklungsdatenbank in der Produktionsdatenbank migriert. Verwenden Sie die Techniken, die in behandelt, wenn Sie meinen, so kopieren Sie die Benutzerinformationen Konto und die Rolle in der Entwicklungsdatenbank in der Produktionsdatenbank der *Bereitstellen einer Datenbank* Lernprogramm.


Betrachten zum Hinzufügen von Datenbankobjekten zu Datenbank für die Produktion mit s können die `aspnet_regsql.exe` Tool. Starten von Windows-Explorer öffnen und das Navigieren zu .NET Framework Version 2.0-Verzeichnis auf Ihrem Computer %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Es sollten Sie finden die `aspnet_regsql.exe` Tool. Dieses Tool kann über die Befehlszeile verwendet werden, aber es enthält auch eine grafische Benutzeroberfläche; Doppelklicken Sie auf die `aspnet_regsql.exe` Datei zum zugehörigen grafische-Komponente zu starten.

Das Tool startet durch einen Begrüßungsbildschirm erklärt den Zweck anzeigen. Klicken Sie auf Weiter, um zum Bildschirm "Wählen Sie ein Setup-Option", die in Abbildung 1 dargestellt wird. Von hier aus können Sie auswählen, die Anwendungsdienste-Datenbankobjekte oder entfernen sie aus einer Datenbank hinzufügen. Da wir diese Objekte in der Produktionsdatenbank hinzufügen möchten, wählen Sie die Option "Konfigurieren von SQLServer für Application-Dienste", und klicken Sie auf Weiter.


[![Wählen Sie SQLServer für Anwendungsdienste konfigurieren](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Abbildung 1**: Wählen Sie zum Konfigurieren von SQL Server für Anwendungsdienste ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


In "Wählen Sie die Server und Datenbank" fordert Bildschirm Informationen zur Verbindung mit der Datenbank. Geben Sie den Datenbankserver, die Anmeldeinformationen und den Datenbanknamen, die Ihnen von Ihrem Webhosting bereitgestellt, und klicken Sie auf Weiter.

> [!NOTE]
> Nach der Eingabe den Datenbankserver und die Anmeldeinformationen möglicherweise erhalten Sie einen Fehler beim Erweitern der Datenbank Dropdown-Liste. Die `aspnet_regsql.exe` tool Abfragen der `sysdatabases` Systemtabelle zum Abrufen einer Liste der Datenbanken auf dem Server, aber einige Webhosting Unternehmen Sperren ihren Datenbankserver, sodass diese Informationen nicht öffentlich verfügbar ist. Wenn Sie diese Fehlermeldung erhalten, können Sie direkt in der Dropdownliste den Datenbanknamen eingeben.


[![Geben Sie das Tool mit dem Datenbank-s-Verbindungsinformationen](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Abbildung 2**: Geben Sie das Tool mit der Datenbank-s-Verbindungsinformationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Im folgenden Bildschirm werden die Aktionen, die im Begriff sind, d. h. stattfinden zusammengefasst, die die Anwendung Dienste Datenbankobjekte sollen in der angegebenen Datenbank hinzugefügt werden. Klicken Sie neben vollständige diese Aktion aus. Nach einigen Augenblicken wird im letzte Bildschirm angezeigt, beachten Sie, dass die Datenbankobjekte (siehe Abbildung 3) hinzugefügt wurden.


[![War erfolgreich! Die Anwendung Dienste Datenbankobjekte wurden in der Produktionsdatenbank hinzugefügt.](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Abbildung 3**: Erfolg! Die Anwendung Dienste Datenbank Objekte hinzugefügt wurden in der Produktionsdatenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Um sicherzustellen, dass die Anwendung Dienste Datenbankobjekte in der Produktionsdatenbank erfolgreich hinzugefügt wurden, öffnen Sie SQL Server Management Studio, und Herstellen einer Verbindung mit der Produktionsdatenbank. Wie in Abbildung 4 gezeigt, nun sollte die Anwendung Dienste Datenbanktabellen in Ihrer Datenbank `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`usw. lauten.


[![Vergewissern Sie sich, dass die Datenbankobjekte in der Produktionsdatenbank hinzugefügt wurden](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Abbildung 4**: Vergewissern Sie sich, dass die Datenbankobjekte in der Produktionsdatenbank hinzugefügt wurden ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Sie müssen nur mithilfe der `aspnet_regsql.exe` -tool beim Bereitstellen der Webanwendung zum ersten Mal oder zum ersten Mal, nachdem Sie gestartet haben, verwenden die Anwendungsdienste. Sobald diese Datenbankobjekte in der Produktionsdatenbank sind gewonnen sie t muss erneut hinzugefügt oder geändert werden soll.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopieren von Benutzerkonten von der Entwicklung bis hin zur Produktion

Bei Verwendung der `SqlMembershipProvider` und `SqlRoleProvider` -Anbieterklassen zum Speichern der Anwendung Dienste in einer SQL Server-Datenbank, die Benutzerinformationen für Konto "und" Rolle befindet sich in einer Vielzahl von Datenbanktabellen, einschließlich `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, und `aspnet_UsersInRoles`, o. ä. Wenn während der Entwicklung von Benutzerkonten in der Entwicklungsumgebung erstellen können Sie die Benutzerkonten in der Produktion replizieren, durch die entsprechenden Datensätze aus den Tabellen für die betroffene Datenbank kopieren. Wenn Sie den Datenbankveröffentlichungs-Assistenten zum Bereitstellen der Anwendung Dienste Datenbankobjekte verwendet möglicherweise auch Zahlungsoption die Datensätze zu kopieren, die in der Entwicklung zur auch produktiv genutzt werden erstellten Benutzerkonten führen würde. Jedoch je nach den Konfigurationseinstellungen können Sie feststellen, dass die Benutzer, deren Konten erstellt, in der Entwicklung und bis hin zur Produktion kopiert wurden, nicht aus der Produktionswebsite anmelden können. Was gibt?

Die `SqlMembershipProvider` und `SqlRoleProvider` -Anbieterklassen wurden so konzipiert, dass eine einzelne Datenbank als Benutzerspeicher verwendet werden kann, für mehrere Anwendungen, in dem jede Anwendung, theoretisch können kann, Benutzer mit sich überschneidenden Benutzernamen und Rollen mit dem gleichen Namen besitzen. Um nach dieser Flexibilität zu ermöglichen, die Datenbank verwaltet eine Liste der Anwendungen in der `aspnet_Applications` Tabelle und jeder Benutzer eine dieser Anwendungen zugeordnet ist. Insbesondere die `aspnet_Users` Tabelle besitzt eine `ApplicationId` Spalte, die jeder Benutzer mit einem Datensatz im bindet die `aspnet_Applications` Tabelle.

Zusätzlich zu den `ApplicationId` Spalte, die `aspnet_Applications` Tabelle enthält auch eine `ApplicationName` Spalte, die einen mehr Human benutzerfreundliche Namen für die Anwendung bereitstellt. Wenn eine Website versucht, arbeiten mit einem Benutzerkonto an, z. B. Überprüfen einer Benutzeranmeldeinformationen s aus der Anmeldeseite Weise ihm mitgeteilt der `SqlMembershipProvider` -Klasse Anwendung arbeiten. Es ist normalerweise der Fall ist dies, indem Sie den Namen der Anwendung angeben und dieser Wert stammt aus der Anbieterkonfiguration s in `Web.config` – insbesondere über die `applicationName` Attribut.

Aber was geschieht, wenn die `applicationName` -Attribut nicht angegeben ist, im `Web.config`? In diesem Fall die Mitgliedschaft System verwendet den Stammpfad der Anwendung als der `applicationName` Wert. Wenn die `applicationName` Attribut nicht ausdrücklich festgelegt ist `Web.config`, dann besteht die Möglichkeit, dass die Entwicklungsumgebung und der produktionsumgebung verwenden einen andere Anwendungsstamm und aus diesem Grund andere Anwendung zugeordnet werden die Namen in der Application-Dienste. Wenn eine derartige fehlende Übereinstimmung tritt auf, und klicken Sie dann die Benutzer in der Entwicklungsumgebung erstellt haben, werden ein `ApplicationId` -Wert, der nicht mit übereinstimmt die `ApplicationId` Wert für die produktionsumgebung. Das Ergebnis ist, dass diese Benutzer/t gewonnen anmelden können.

> [!NOTE]
> Wenn Sie sich in dieser Situation - Benutzerkonten, die in der Produktion mit einer nicht übereinstimmenden kopiert `ApplicationId` Value - konnte eine Abfrage aus, um diese falsche aktualisieren schreiben `ApplicationId` -Werte in der `ApplicationId` für die Produktion verwendet. Nach der Aktualisierung, wäre die Benutzer, deren Konten in der Entwicklungsumgebung erstellt wurden, nun die Webanwendung in Produktion anmelden können.


Die gute Nachricht ist, ergibt sich ein einfacher Schritt sicher, dass die beiden Umgebungen identisch sind möglich `ApplicationId` - explizit festgelegten der `applicationName` -Attribut im `Web.config` für alle Ihre Anwendung-Services-Anbieter. Ich explizit festlegen, dass die `applicationName` -Attribut auf "BookReviews" in der `<membership>` und `<roleManager>` Elemente in diesem Codeausschnitt aus `Web.config` zeigt.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Weitere Informationen zum Einrichten der `applicationName` Attribut und die Gründe, finden Sie unter [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s Blogbeitrag [ *immer legen Sie den Parameter "ApplicationName" Eigenschaft, die beim Konfigurieren von ASP.NET-Mitgliedschaft und anderen Anbietern*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Verwalten von Benutzerkonten in der Produktionsumgebung

Der ASP.NET Web Site Administration Tool (WSAT) erleichtert das Erstellen und Verwalten von Benutzerkonten, definieren und Anwenden von Rollen und Autorisierungsregeln Benutzer- und rollenbasierte ausgeschrieben. Indem Sie im Projektmappen-Explorer aufrufen, und klicken Sie auf das Symbol "ASP.NET-Konfiguration" oder indem Sie die Website oder ein Projekt Menüs aufrufen und dabei das Menüelement ASP.NET-Konfiguration auswählen können Sie die WSAT aus Visual Studio starten. Leider kann die WSAT nur mit lokalen Websites verwendet werden. Aus diesem Grund können die WSAT aus Ihrer Arbeitsstation Sie um die Website in der produktionsumgebung zu verwalten.

Die gute Nachricht ist, dass alle Funktionen verfügbar gemacht werden von der WSAT bereitgestellten programmgesteuert über die Mitgliedschaft und Rollen-APIs verfügbar; Darüber hinaus verwenden viele WSAT-Bildschirme Standardsteuerelemente ASP.NET Anmeldung verknüpft. Kurz gesagt, können Sie ASP.NET-Seiten hinzufügen, zu Ihrer Website, die die erforderlichen Management-Funktionen bieten.

Beachten Sie, dass eine frühere Lernprogramm die Webanwendung Buch Reviews einschließen aktualisiert eine `~/Admin` , und dieser Ordner ist konfiguriert, dass nur Benutzer in der Rolle "Admin" können. Dieser Ordner mit dem Namen eine Seite hinzugefügt `CreateAccount.aspx` aus dem ein Administrator ein neues Benutzerkonto erstellen können. Auf dieser Seite verwendet der CreateUserWizard-Steuerelement, um die Benutzer-Schnittstelle und Back-End-Logik für das Erstellen eines neuen Benutzerkontos angezeigt. Welche weitere, s angepasst ich das Steuerelement, um ein Kontrollkästchen enthalten, die gefragt, ob der neue Benutzer der Rolle "Admin" auch hinzugefügt werden sollen (siehe Abbildung 5). Mit ein wenig Arbeit können Sie einen benutzerdefinierten Satz von Seiten erstellen, die die Benutzer und die Rolle verwaltungsbezogenen Aufgaben implementiert, die andernfalls durch die WSAT bereitgestellt wird.

> [!NOTE]
> Für Weitere Informationen zur Verwendung der Mitgliedschaft und Rollen-APIs sowie die Anmeldung bezogene ASP.NET Web-Steuerelemente, lesen Sie unbedingt die werden meine [ *Website Sicherheit Lernprogramme*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Weitere Informationen über das Anpassen des Steuerelements CreateUserWizard finden Sie in der [ *Erstellen von Benutzerkonten* ](../../older-versions-security/membership/creating-user-accounts-vb.md) und [ *zusätzliche Benutzerinformationen speichern* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) Lernprogramme oder Auschecken [ *Erich Peterson* ](http://www.erichpeterson.com/) s-Artikel [ *Anpassen des Steuerelements CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Administratoren können neue Benutzerkonten erstellen.](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Abbildung 5**: Administratoren können erstellen Sie neue Benutzerkonten ([klicken Sie hier, um das Bild in voller Größe angezeigt](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Wenn Sie die vollständige Funktionalität des Auschecken des WSAT benötigen [ *parallelen Ihrer eigenen Websiteverwaltungs-Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), in welchem Autor Dan Clem führt Sie durch den Prozess der Erstellung eines benutzerdefinierten WSAT-ähnliche Tools. Dan teilt seine s-Anwendungsquellcode (in c#) und bietet eine schrittweise Anleitung für Ihre gehostete Website hinzugefügt.

## <a name="summary"></a>Zusammenfassung

Beim Bereitstellen einer Webanwendung, die die Anwendung Dienste datenbankimplementierung verwendet müssen Sie zunächst sicherstellen, dass die Produktionsdatenbank erforderlichen Datenbankobjekte verfügt. Diese Objekte können hinzugefügt werden, die beschriebenen Verfahren verwenden die *Bereitstellen einer Datenbank* Lernprogramm; Alternativ können Sie die `aspnet_regsql.exe` Tools, wie in diesem Lernprogramm wurde erläutert. Andere Herausforderungen annehmen können werden wir berührt, im Mittelpunkt der Anwendungsname verwendet, die in den Entwicklungs- und produktionsumgebungen-Umgebungen (das ist wichtig, wenn Sie Benutzer und Rollen, die in der Entwicklungsumgebung erstellt, für die Produktion gültig sein sollen) und Techniken zum Synchronisieren verwalten die Benutzer und Rollen in der produktionsumgebung.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [*Registrierungstool für ASP.NET SQL-Server (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Erstellen der Datenbank für die Anwendungsdienste für SQLServer*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Erstellen das Schema für die Mitgliedschaft in SQLServer*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Untersuchen von ASP.NET s Mitgliedschaft, Rollen und Profile*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Paralleles eigene Websiteverwaltungs-Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Lernprogramme für Website-Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Website-Tool (Übersicht)*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

>[!div class="step-by-step"]
[Zurück](configuring-the-production-web-application-to-use-the-production-database-vb.md)
[Weiter](strategies-for-database-development-and-deployment-vb.md)
