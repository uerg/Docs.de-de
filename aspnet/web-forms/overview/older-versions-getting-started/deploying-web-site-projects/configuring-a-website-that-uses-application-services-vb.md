---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Konfigurieren einer Website, die Anwendungsdiensten (VB) | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET-Version 2.0 eingeführt, eine Reihe von Anwendungsdiensten, die Teil von .NET Framework und dienen als eine Sammlung von Baustein "yo" Dienste...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 6d4852fe4a8589ddcb23c7afa4ed5c5e74ef0af1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841691"
---
<a name="configuring-a-website-that-uses-application-services-vb"></a>Konfigurieren einer Websites mit Anwendungsdiensten (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> ASP.NET-Version 2.0 eingeführt, eine Reihe von Anwendungsdiensten, die Teil von .NET Framework und dienen als eine Sammlung von Baustein-Dienste, die Sie verwenden können, um umfangreiche Funktionen zu Ihrer Webanwendung hinzuzufügen. In diesem Tutorial erfahren Sie, wie eine Website in der produktionsumgebung von Anwendungsdiensten konfigurieren und behandelt verbreitete Probleme mit der Verwaltung von Benutzerkonten und Rollen in der produktionsumgebung.


## <a name="introduction"></a>Einführung

ASP.NET-Version 2.0 eingeführt, eine Reihe von *Anwendungsdienste*, die Teil von .NET Framework und dienen als eine Sammlung von Baustein, die Sie Dienste können umfangreiche Funktionen zu Ihrer Webanwendung hinzufügen. Die Anwendungsdienste umfassen:

- **Mitgliedschaft** : eine API zum Erstellen und Verwalten von Benutzerkonten.
- **Rollen** : eine API zum Kategorisieren von Benutzern in Gruppen.
- **Profil** : eine API für benutzerdefinierte, benutzerspezifische Inhalte gespeichert.
- **Siteübersicht** : eine API zum Definieren einer logischen Standort s-Struktur in Form einer Hierarchie, die über Steuerelemente zur Seitennavigation, z. B. Menüs und der Breadcrumb-Leiste angezeigt werden können.
- **Personalisierung** : eine API zum Verwalten von Einstellungen von Modifizierern Anpassung, mit den meisten Fällen verwendet [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Systemüberwachung** : eine API für die Überwachung der Leistung, Sicherheit, Fehler und andere System integritätsmetriken für eine ausgeführte Webanwendung.
  

Die Anwendungsdienste APIs sind nicht auf eine bestimmte Implementierung gebunden werden. Stattdessen weisen Sie den Anwendungsdiensten, um eine bestimmte *Anbieter*, und dieser Anbieter implementiert den Dienst mit einer bestimmten Technologie. Die am häufigsten verwendeten Anbieter für Internet-basierte Webanwendungen, die an ein Webhostingunternehmen gehostet sind diese Anbieter unterstützen, die eine Implementierung von SQL Server-Datenbank verwenden. Z. B. die `SqlMembershipProvider` ist ein Anbieter für die Mitgliedschafts-API, die Benutzerkontoinformationen in einer Microsoft SQL Server-Datenbank speichert.

Mit dem Application-Dienste und SQL Server-Anbieter fügt einige Herausforderungen beim Bereitstellen der Anwendung. Zunächst müssen die Anwendung Dienste Datenbankobjekte ordnungsgemäß für die Entwicklungs- und produktionsumgebungen Datenbanken erstellt und entsprechend initialisiert. Es gibt auch wichtige Konfigurationseinstellungen, die vorgenommen werden müssen.

> [!NOTE]
> Die Anwendungsdienste APIs entwickelt wurden, mit der [ *Anbietermodell*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), ein Entwurfsmuster, die für eine API s Implementierungsdetails zur Laufzeit bereitgestellt werden kann. .NET Framework ausgeliefert wird, mit einer Reihe von Application Service Provider, die z. B. verwendet werden können, die `SqlMembershipProvider` und `SqlRoleProvider`, welche Anbieter für die Mitgliedschaft und Rollen-APIs, die eine SQL Server-Datenbank Implementierung. Sie können auch erstellen und -Plug-in einen benutzerdefinierten Anbieter. In der Tat die Book Reviews-Webanwendung bereits enthält einen benutzerdefinierten Anbieter für das Site Map-API (`ReviewSiteMapProvider`), die erstellt der Sitemap aus den Daten in die `Genres` und `Books` Tabellen in der Datenbank.


Dieses Tutorial beginnt mit einer Betrachtung darüber, wie ich die Book Reviews-Webanwendung mit der Mitgliedschaft und Rollen APIs erweitert. Sie werden dann durch die Bereitstellung einer Webanwendung, die Dienste der Anwendung mit einer SQL Server-Datenbank-Implementierung verwendet, und endet mit dem adressieren häufige Probleme mit der Verwaltung von Benutzerkonten und Rollen in der produktionsumgebung geführt.

## <a name="updates-to-the-book-reviews-application"></a>Updates für den Book Reviews-Anwendung

Führen in den letzten paar Tutorials bieten, die die Book Reviews-Webanwendung aus einer statischen Website auf eine dynamische, datengesteuerte Webanwendung aktualisiert wurde, mit einem Satz von Verwaltungsseiten für die Verwaltung von Genres und Berichte. Allerdings in diesem Verwaltungsabschnitt ist zurzeit nicht geschützt: jeder Benutzer, der die URL der Administration kennt (oder errät) kann Walzer und erstellen, bearbeiten oder Löschen von Überprüfungen auf unserer Website. Eine allgemeine Möglichkeit, bestimmte Teile einer Website zu schützen, werden von Benutzerkonten zu implementieren und verwenden Sie dann den Zugriff auf bestimmte Benutzer oder Rollen einschränken URL-Autorisierungsregeln ab. Die Book Reviews-Webanwendung, die heruntergeladen, die in diesem Tutorial wird unterstützt, Benutzerkonten und Rollen. Es verfügt über eine einzelne Rolle definiert, mit dem Namen Administrators, und nur Benutzer in dieser Rolle können auf die Verwaltungsseiten zugreifen.

> [!NOTE]
> Ich haben drei Benutzerkonten erstellt, in der Webanwendung Book Reviews: Scott Jisun und Alice. Alle drei Benutzer das gleiche Kennwort aufweisen: **Kennwort!** Scott und Jisun befinden sich in der Rolle "Administrator", Alice nicht. Die s nicht-Websiteverwaltungsseiten sind weiterhin zugänglich für anonyme Benutzer. Das heißt, dass Sie nicht müssen melden Sie sich bei der Website besuchen, wenn Sie es, zu verwalten möchten in diesem Fall Sie in der Rolle "Administrator" als Benutzer sich anmelden müssen.


Die Masterseite Book Reviews Anwendung s wurde aktualisiert, um eine andere Benutzeroberfläche für authentifizierte und anonyme Benutzer einzuschließen. Wenn ein anonymer Benutzer die Website besucht, sieht sie einen Anmeldelink in der oberen rechten Ecke. Ein authentifizierter Benutzer sieht die Meldung "Willkommen zurück, *Benutzername*!" und einen Link zum Abmelden. Es gibt auch eine Anmeldeseite-s (`~/Login.aspx`), ein Login-Steuerelement, das die Benutzeroberfläche und Logik bereitstellt, für die Authentifizierung eines Besuchers enthält. Nur Administratoren können neue Konten erstellen. (Sind Seiten erstellen und Verwalten von Benutzerkonten in der `~/Admin` Ordner.)

### <a name="configuring-the-membership-and-roles-apis"></a>Konfigurieren die Mitgliedschaft und Rollen-APIs

Die Book Reviews-Webanwendung verwendet die Mitgliedschaft und Rollen-APIs, um Benutzerkonten zu unterstützen und um die Benutzer in Rollen (d. h., der Administratorrolle) zu gruppieren. Die `SqlMembershipProvider` und `SqlRoleProvider` Anbieterklassen werden verwendet, da Konto und die entsprechende Informationen in einer SQL Server-Datenbank gespeichert werden soll.

> [!NOTE]
> Dieses Tutorial dient nicht dazu, eine detaillierte Untersuchung mit der Konfiguration von Web-Apps die Mitgliedschaft und Rollen-APIs unterstützt werden. Umfassende Informationen zu dieser APIs und die erforderlichen Schritte Sie durchführen müssen, um eine Website, um diese zu konfigurieren, lesen Sie meine [ *Website-Lernprogramme zur ASP.NET-Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Um die Anwendungsdienste mit SQL Server-Datenbank verwenden müssen Sie zunächst hinzufügen Datenbankobjekte verwendet von diesen Anbietern in der Datenbank, in dem Sie das Benutzerkonto an sowie die Rolleninformationen gespeichert. Diese erforderlichen Datenbankobjekte enthalten eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren. Sofern nicht angegeben, andernfalls der `SqlMembershipProvider` und `SqlRoleProvider` -Anbieterklassen verwenden eine SQL Server Express Edition-Datenbank, die mit dem Namen `ASPNETDB` befindet sich in der Anwendung s `App_Data` Ordner Wenn z.B. eine Datenbank nicht vorhanden ist, wird er automatisch erstellt mit den erforderlichen Datenbankobjekte von diesen Anbietern zur Laufzeit.

Es ist möglich, und in der Regel optimal ist, erstellen die Anwendung Dienste Datenbankobjekte in der gleichen Datenbank, in dem die Website s anwendungsspezifische Daten gespeichert ist. .NET Framework ausgeliefert wird, mit einem Tool, mit dem Namen `aspnet_regsql.exe` installiert, die die Datenbankobjekte, auf einer angegebenen Datenbank. Ich habe ADMtemp und dieses Tool verwendet, um diese Objekte hinzuzufügen der `Reviews.mdf` -Datenbank in die `App_Data` Ordner (der Development-Datenbank). Wir sehen, wie Sie dieses Tool weiter unten in diesem Tutorial verwenden, wenn wir diese Objekte in der Produktionsdatenbank hinzufügen.

Wenn Sie die Anwendung Dienste Datenbankobjekte in einer Datenbank als hinzufügen `ASPNETDB` anpassen, dass die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter Klassen Konfigurationen aus, sodass sie die entsprechende Datenbank verwenden. Zum Anpassen der Membership-Provider Hinzufügen einer [  *&lt;Mitgliedschaft&gt; Element* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) innerhalb der `<system.web>` im Abschnitt `Web.config`; verwenden Sie die [  *&lt;RoleManager&gt; Element* ](https://msdn.microsoft.com/library/ms164660.aspx) zum Konfigurieren des Rollen-Anbieters. Der folgende Codeausschnitt stammt aus der Book Reviews Anwendung s `Web.config` und zeigt die Einstellungen für die Mitgliedschaft und Rollen-APIs konfigurieren. Beachten Sie, dass beide registrieren Sie einen neuen Anbieter - `ReviewMembership` und `ReviewRole` –, verwenden die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter bzw.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

Die `Web.config` Datei s `<authentication>` Element auch zur Unterstützung von formularbasierte Authentifizierung konfiguriert wurde.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Beschränken des Zugriffs auf den Verwaltungsseiten

ASP.NET macht es einfach zu gewähren oder Verweigern des Zugriffs auf eine bestimmte Datei oder Ordner nach Benutzer oder Rolle über die *URL-Autorisierung* Feature. (Wir kurz erläutert, URL-Autorisierung in der *Core Unterschiede zwischen IIS und ASP.NET Development Server* Tutorial und gezeigt, wie IIS und ASP.NET Development Server URL-Autorisierungsregeln für statische unterschiedlich angewendet im Vergleich zu dynamischen Inhalt.) Da wir den Zugriff auf nicht zulassen möchten die `~/Admin` Ordner mit Ausnahme von dieser Benutzer in der Rolle "Administrator", wir müssen URL-Autorisierungsregeln in diesen Ordner hinzufügen. Insbesondere müssen die URL-Autorisierungsregeln ermöglicht es Benutzern in der Rolle "Administrator" und alle anderen Benutzer verweigern. Dies geschieht durch Hinzufügen einer `Web.config` -Datei in die `~/Admin` Ordner mit dem folgenden Inhalt:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Für Weitere Informationen zu ASP.NET s URL-Autorisierungsfeature und wie Sie es verwenden, um Autorisierungsregeln für Benutzer und Rollen, darlegen sein, lesen Sie unbedingt die [ *Benutzerbasierte Autorisierung* ](../../older-versions-security/membership/user-based-authorization-vb.md) und [ *Rollenbasierte Autorisierung* ](../../older-versions-security/roles/role-based-authorization-vb.md) Tutorials aus meinem [ *Website-Lernprogramme zur ASP.NET-Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Bereitstellen einer Webanwendung, die Dienste der Anwendung verwendet.

Wenn Sie eine Website bereitstellen, die Dienste der Anwendung und einen Anbieter, der die Anwendungsinformationen für Dienste in einer Datenbank speichert, ist es zwingend erforderlich, dass die Datenbankobjekte, die erforderlich sind, durch die Anwendungsdienste in der Produktionsdatenbank erstellt werden. Die Produktionsdatenbank enthält anfänglich keine diese Objekte können also bei der ersten Bereitstellung der Anwendung (oder bei der Bereitstellung zum ersten Mal nach der Anwendungsdienste hinzugefügt wurden), Sie müssen zusätzliche Schritte, um diese erforderlichen Datenbankobjekte zu erhalten, auf die die Produktionsdatenbank.

Eine weitere Herausforderung kann auftreten, wenn eine Website bereitstellen, die Dienste der Anwendung verwendet wird, wenn Sie die Benutzerkonten erstellt haben, in der Entwicklungsumgebung in der produktionsumgebung replizieren möchten. Abhängig von der Konfiguration der Mitgliedschaft und Rollen ist es möglich, dass selbst wenn Sie erfolgreich die Benutzerkonten, die in der Entwicklungsumgebung in der Produktionsdatenbank erstellt wurden kopieren, diese Benutzer bei der Webanwendung in der Produktion sich nicht anmelden können. Wir sehen Sie sich die Ursache des Problems und beschrieben, wie es zu verhindern.

ASP.NET im Lieferumfang von eine gute [ *Web Site Administration Tool (WSAT) zur Verfügung* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) , die in Visual Studio gestartet werden und ermöglicht dem Benutzer das Konto, Rollen und Autorisierung Regeln für die über eine webbasierte Verwaltung -Schnittstelle. Leider funktioniert das WSAT nur für lokale Websites, was bedeutet, dass es zur Remoteverwaltung von Benutzerkonten, Rollen und Autorisierungsregeln für die Webanwendung in der produktionsumgebung verwendet werden kann. Wir werden verschiedene Möglichkeiten zum Implementieren von WSAT-ähnliche Verhalten Ihrer Produktionswebsite betrachten.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Hinzufügen der Datenbank-Objekte mithilfe von Aspnet\_regsql.exe

Die *Bereitstellen einer Datenbank* Tutorial wurde gezeigt, wie Sie Tabellen und Daten aus der Entwicklungsdatenbank in der Produktionsdatenbank zu kopieren und diese Techniken können sicherlich verwendet werden, um die Anwendung Dienste zu Datenbankobjekte Kopieren der die Produktionsdatenbank. Eine andere Möglichkeit ist die `aspnet_regsql.exe` -Tool, das hinzugefügt oder entfernt die Anwendung Dienste Datenbankobjekte aus einer Datenbank.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool erstellt die Datenbankobjekte in einer angegebenen Datenbank. Es werden Daten in die Datenbankobjekte nicht aus der Entwicklungsdatenbank in der Produktionsdatenbank migriert. Verwenden Sie die Techniken, die in behandelt, wenn die Benutzer-Konto und die entsprechende Informationen in der Entwicklungsdatenbank in der Produktionsdatenbank Kopieren der *Bereitstellen einer Datenbank* Tutorial.


S wie die Produktions-Datenbank mithilfe von Datenbankobjekten hinzugefügt betrachten können die `aspnet_regsql.exe` Tool. Starten Sie Windows Explorer öffnen, und navigieren Sie auf das Verzeichnis der .NET Framework Version 2.0 auf Ihrem Computer %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Dort sollten Sie finden die `aspnet_regsql.exe` Tool. Dieses Tool kann über die Befehlszeile verwendet werden, aber es enthält auch eine grafische Benutzeroberfläche; Doppelklicken Sie auf die `aspnet_regsql.exe` Datei, um die grafische-Komponente zu starten.

Das Tool startet durch Anzeigen eines Begrüßungsbildschirms erläutert den Zweck. Klicken Sie auf Weiter, um zum Fenster "Wählen Sie ein Setup-Option" wählen, das in Abbildung 1 dargestellt ist. Von hier aus können Sie auswählen, der die Anwendungsdienste Datenbankobjekte oder entfernen sie aus einer Datenbank hinzugefügt wird. Da diese Objekte in der Produktionsdatenbank hinzugefügt werden soll, wählen Sie die Option "Konfigurieren von SQLServer für Anwendungsdienste", und klicken Sie auf Weiter.


[![Wählen Sie SQLServer für Anwendungsdienste konfigurieren](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Abbildung 1**: Wählen Sie zum Konfigurieren von SQL Server für Anwendungsdienste ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))


In "Wählen Sie die Server und Datenbank" fordert Bildschirm Informationen zur Verbindung mit der Datenbank. Geben Sie den Datenbankserver, die Anmeldeinformationen und den Datenbanknamen, die Ihnen von Ihrem Webhosting bereitgestellt, und klicken Sie auf Weiter.

> [!NOTE]
> Nach der Eingabe den Datenbankserver und die Anmeldeinformationen erhalten Sie einen Fehler beim Erweitern der Datenbank-Dropdown-Liste. Die `aspnet_regsql.exe` tool Abfragen die `sysdatabases` Systemtabelle zum Abrufen einer Liste der Datenbanken auf dem Server, aber einige Web-hosting-Unternehmen Sperren auf ihren Datenbankserver, damit diese Informationen nicht öffentlich verfügbar sind. Wenn Sie diese Fehlermeldung erhalten, können Sie den Datenbanknamen direkt in der Dropdown Liste eingeben.


[![Geben Sie das Tool mit Ihrer Datenbank-s-Verbindungsinformationen](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Abbildung 2**: Geben Sie das Tool mit der Datenbank-s-Verbindungsinformationen ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))


Im folgenden Bildschirm werden die Aktionen, die sind im Begriff, nämlich stattfinden, zusammengefasst, die die Datenbankobjekte der Anwendung Dienste in der angegebenen Datenbank hinzugefügt werden sollen. Klicken Sie neben der vollständigen diese Aktion aus. Nach einigen Augenblicken wird der letzten Seite angezeigt, beachten Sie, dass die Datenbankobjekte hinzugefügt wurden (siehe Abbildung 3).


[![Success! Die Anwendung Dienste Datenbankobjekte wurden in der Produktionsdatenbank hinzugefügt.](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Abbildung 3**: Erfolg! Die Anwendung Dienste Datenbank Objekte hinzugefügt wurden in der Produktionsdatenbank ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))


Um sicherzustellen, dass die Anwendung Dienste Datenbankobjekte in der Produktionsdatenbank erfolgreich hinzugefügt wurden, öffnen Sie SQL Server Management Studio und eine Verbindung mit der Produktionsdatenbank. Wie in Abbildung 4 gezeigt, Sie sollten jetzt sehen die Tabellen der Anwendung Dienste in Ihrer Datenbank `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`und so weiter.


[![Vergewissern Sie sich, dass Datenbankobjekte in der Produktionsdatenbank hinzugefügt wurden](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Abbildung 4**: Vergewissern Sie sich, dass Datenbankobjekte in der Produktionsdatenbank hinzugefügt wurden ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))


Sie müssen nur mit der `aspnet_regsql.exe` tool bei der Bereitstellung Ihrer Webanwendung zum ersten Mal oder zum ersten Mal, nachdem Sie gestartet haben, mit der die Anwendungsdienste. Sobald diese Datenbankobjekte für die Produktionsdatenbank sind gewonnen sie t muss erneut hinzugefügt oder geändert werden.

### <a name="copying-user-accounts-from-development-to-production"></a>Kopieren von Benutzerkonten von der Entwicklung zur Produktion

Bei Verwendung der `SqlMembershipProvider` und `SqlRoleProvider` Anbieterklassen zum Speichern von Informationen zu Diensten der Anwendung in einer SQL Server-Datenbank befindet sich die Benutzer-Konto und die entsprechende Informationen in einer Vielzahl von Datenbanktabellen, einschließlich `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, und `aspnet_UsersInRoles`, u. a. Wenn während der Entwicklung Sie Benutzerkonten, in der Entwicklungsumgebung erstellen können Sie diese Benutzerkonten in der produktionsumgebung replizieren, kopieren Sie die entsprechenden Datensätze aus den entsprechenden Datenbanktabellen. Wenn Sie den Datenbankveröffentlichungs-Assistent zum Bereitstellen der Anwendung Services-Datenbank-Objekte verwendet möglicherweise auch ausgewählt wurde der Datensätze kopieren dabei es in den Benutzerkonten erstellt haben, in der Entwicklung kommt, auch produktiv genutzt werden. Aber je nach den Einstellungen für die Konfiguration möglicherweise, dass Benutzer, deren Konten in der Entwicklung erstellt und kopiert in der produktionsumgebung wurden, nicht von der Produktionswebsite anmelden können. Woran liegt das?

Die `SqlMembershipProvider` und `SqlRoleProvider` Anbieterklassen wurden so entworfen, dass eine einzelne Datenbank für mehrere Anwendungen, in denen jede Anwendung Theoretisch könnte, mit sich überschneidenden Benutzernamen-Benutzer und Rollen mit dem gleichen Namen haben wie ein Benutzerspeicher verwendet werden kann. Um diese Flexibilität ermöglichen, die Datenbank verwaltet eine Liste der Anwendungen in der `aspnet_Applications` Tabelle und jeder Benutzer eine dieser Anwendungen zugeordnet ist. Insbesondere die `aspnet_Users` Tabelle besitzt eine `ApplicationId` Spalte, die jeder Benutzer mit einem Datensatz im verbindet die `aspnet_Applications` Tabelle.

Zusätzlich zu den `ApplicationId` Spalte, die `aspnet_Applications` Tabelle enthält auch eine `ApplicationName` Spalte, die einen mehr Benutzerfreundlicher Namen für die Anwendung bereitstellt. Wenn eine Website versucht, arbeiten mit einem Benutzerkonto an, z. B. Überprüfen einer Benutzeranmeldeinformationen s über die Anmeldeseite, er muss erkennen die `SqlMembershipProvider` welche Anwendung zur Zusammenarbeit mit Klasse. Dies ist in der Regel der Fall ist dies durch den Namen der Anwendung bereitstellen und dieser Wert stammt aus der Anbieterkonfiguration s in `Web.config` – insbesondere über die `applicationName` Attribut.

Aber was geschieht, wenn die `applicationName` -Attribut nicht angegeben ist, im `Web.config`? In diesem Fall die Mitgliedschaft System verwendet den Stammpfad der Anwendung als der `applicationName` Wert. Wenn die `applicationName` Attribut nicht ausdrücklich festgelegt ist im `Web.config`, dann ist die Möglichkeit, dass die Entwicklungsumgebung und die produktionsumgebung Stammverzeichnis einer anderen Anwendung verwendet und daher anderen Anwendung zugeordnet die Namen in der die Anwendungsdienste. Wenn eine derartige fehlende Übereinstimmung tritt ein, und klicken Sie dann die Benutzer in der Entwicklungsumgebung erstellt haben, werden ein `ApplicationId` -Wert, der mit nicht entspricht der `ApplicationId` Wert für die produktionsumgebung. Das Ergebnis ist, dass t gewonnen Benutzer sich anmelden.

> [!NOTE]
> Wenn Sie sich in diesem Fall – mit Benutzerkonten, die in die produktionsumgebung eine nicht übereinstimmende kopiert `ApplicationId` -Wert: Sie könnten eine Abfrage, um diese falsche aktualisieren `ApplicationId` -Werte in der `ApplicationId` für Produktion verwendet. Nach der Aktualisierung, wäre die Benutzer, deren Konten in der Entwicklungsumgebung erstellt wurden, nun in der Webanwendung auf Produktions-anmelden können.


Die gute Nachricht ist, dass es ein einfacher Schritt, Sie ergreifen können, um sicherzustellen, dass die beiden Umgebungen die gleiche verwenden `ApplicationId` – explizit die `applicationName` -Attribut im `Web.config` für alle von Ihrer Anwendung Services-Datenanbietern. Ich explizit festlegen der `applicationName` "BookReviews"-Attribut der `<membership>` und `<roleManager>` Elemente wie dieser Ausschnitt `Web.config` zeigt.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Weitere Informationen zum Festlegen der `applicationName` -Attribut und die Gründe, warum, finden Sie unter [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) s Blogbeitrag [ *immer legen Sie die ApplicationName Eigenschaft, die beim Konfigurieren von ASP.NET-Mitgliedschaft und anderen Anbietern*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Verwalten von Benutzerkonten in der Produktionsumgebung

Die ASP.NET Web Site Administration Tool (WSAT) erleichtert das Erstellen und Verwalten von Benutzerkonten, definieren und Anwenden von Rollen und werden die Autorisierungsregeln für Benutzer und rollenbasierte. Sie können die WSAT aus Visual Studio starten, indem Sie im Projektmappen-Explorer und durch Klicken auf das Symbol "ASP.NET-Konfiguration" oder indem Sie auf die Website oder ein Projekt Menüs und Auswählen des Menüelements ASP.NET-Konfiguration. Leider kann die WSAT nur lokale Websites angewendet werden. Sie können nicht aus diesem Grund die WSAT von Ihrer Arbeitsstation verwenden, für die Verwaltung der Website in der produktionsumgebung.

Die gute Nachricht ist, dass alle Funktionen verfügbar gemacht werden von der WSAT bereitgestellten programmgesteuert über die Mitgliedschaft und Rollen-APIs verfügbar ist. Darüber hinaus verwenden viele WSAT Bildschirme der standardmäßigen ASP.NET anmeldebezogene Steuerelemente. Kurz gesagt, können Sie ASP.NET-Seiten hinzufügen, zu Ihrer Website, die die erforderlichen Funktionen zu bieten.

Beachten Sie, dass es sich bei einem früheren Tutorial die Book Reviews-Webanwendung einschließen aktualisiert eine `~/Admin` , und dieser Ordner ist konfiguriert, dass nur Benutzer in der Rolle "Administrator" können. Dieser Ordner mit dem Namen eine Seite hinzugefügt `CreateAccount.aspx` aus dem ein Administrator ein neues Benutzerkonto erstellen können. Auf dieser Seite mithilfe des Steuerelements CreateUserWizard die Benutzerlogik-Schnittstelle und Back-End zum Erstellen eines neuen Benutzerkontos angezeigt. Neuheiten mehr angepasst ich das Steuerelement, um ein Kontrollkästchen enthalten, die fordert, ob der neue Benutzer auch die Rolle "Administrator" hinzugefügt werden soll (siehe Abbildung 5). Mit ein wenig Arbeit können Sie einen benutzerdefinierten Satz von Seiten erstellen, die die Benutzer und die Rolle verwaltungsbezogenen Aufgaben implementiert, die andernfalls von der WSAT bereitgestellt werden sollen.

> [!NOTE]
> Für Weitere Informationen zur Verwendung der Mitgliedschaft und Rollen APIs zusammen mit der ASP.NET Web anmeldebezogene Steuerelemente, achten Sie darauf, lesen Sie meine [ *Website-Lernprogramme zur ASP.NET-Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Informationen zum Anpassen des Steuerelements CreateUserWizard finden Sie in der [ *Erstellen von Benutzerkonten* ](../../older-versions-security/membership/creating-user-accounts-vb.md) und [ *Speichern von zusätzlichen Benutzerinformationen* ](../../older-versions-security/membership/storing-additional-user-information-vb.md) Lernprogramme, oder sehen Sie sich [ *Erich Peterson* ](http://www.erichpeterson.com/) Artikel, [ *Anpassen des Steuerelements CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Administratoren können neue Benutzerkonten erstellen.](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Abbildung 5**: Administratoren können erstellen Sie neue Benutzerkonten ([klicken Sie, um das Bild in voller Größe anzeigen](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))


Bei Bedarf den vollen Funktionsumfang des Auschecken WSAT [ *parallelen Ihrer eigenen Web Site Administration Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), in dem Dan Clem führt Sie durch den Prozess der Erstellung eines benutzerdefinierten WSAT-ähnlichen Tools. Dan teilt seine s Quellcode der Anwendung (in c#) und bietet eine schrittweise Anleitung für Ihre gehostete Website hinzugefügt.

## <a name="summary"></a>Zusammenfassung

Beim Bereitstellen einer Webanwendung, die die Anwendung Dienste datenbankimplementierung verwendet müssen Sie zunächst sicherstellen, dass die Produktionsdatenbank erforderlichen Datenbankobjekte. Diese Objekte können hinzugefügt werden, mithilfe der Verfahren den *Bereitstellen einer Datenbank* Tutorials; Alternativ können Sie die `aspnet_regsql.exe` Tools, wie in diesem Tutorial erläutert. Andere Herausforderungen wurden wir Center, um den Namen der Anwendung verwendet in der Entwicklungs- und produktionsumgebungen Umgebungen (das ist wichtig, wenn der Benutzer und Rollen, die in der Entwicklungsumgebung erstellt, für die Produktion gültig sein soll) und Techniken zum Synchronisieren verwalten die Benutzer und Rollen in der produktionsumgebung.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [*ASP.NET SQL Server-Registrierungstool (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Erstellen der Datenbank für die Anwendungsdienste für SQLServer*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Erstellen des Mitgliedschaftsschemas in SQLServer*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Untersuchen von ASP.NET s Mitgliedschaft, Rollen und Profile*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Paralleles eigene Websiteverwaltungs-Tool*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Website-Lernprogramme zur ASP.NET-Sicherheit*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Website-Tool (Übersicht)*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Zurück](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Weiter](strategies-for-database-development-and-deployment-vb.md)
