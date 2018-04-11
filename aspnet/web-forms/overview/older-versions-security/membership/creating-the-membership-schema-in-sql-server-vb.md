---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Erstellen das Schema für die Mitgliedschaft in SQLServer (VB) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm beginnt mit dem Untersuchen von Techniken für das erforderliche Schema der Datenbank hinzufügen, um die SqlMembershipProvider zu verwenden. Nach, die wir wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 44458e8022f1f0d52cf136ad7fbaa5dd1f546632
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Erstellen das Schema für die Mitgliedschaft in SQLServer (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> In diesem Lernprogramm beginnt mit dem Untersuchen von Techniken für das erforderliche Schema der Datenbank hinzufügen, um die SqlMembershipProvider zu verwenden. Nach, die wir untersuchen Sie die wichtigsten Tabellen im Schema und erläutert deren Zweck und Wichtigkeit. In diesem Lernprogramm endet mit einem Betrachtung der Verwendung einer ASP.NET-Anwendung mitteilen, welcher Anbieter ist die Mitgliedschaft-Framework verwendet werden soll.


## <a name="introduction"></a>Einführung

In den vorherigen zwei Lernprogrammen verwenden der Formularauthentifizierung zur Identifizierung der Websitebesucher untersucht. Das Framework der Forms-Authentifizierung ganz einfach für Entwickler, die einen Benutzer bei einer Website anmelden und sie über die Seite Besuche durch die Verwendung von Authentifizierungstickets merken zu müssen. Die `FormsAuthentication` Klasse enthält Methoden zum Generieren von das Ticket und dem Besucher Cookies hinzugefügt. Die `FormsAuthenticationModule` untersucht alle eingehende Anforderungen, für die mit einem gültigen Authentifizierungstyp-Ticket, erstellt und ordnet einen `GenericPrincipal` und ein `FormsIdentity` Objekt mit der aktuellen Anforderung. Die Formularauthentifizierung ist lediglich einen Mechanismus zum Gewähren von ein Authentifizierungsticket für einem Besucher, bei der Anmeldung in und, bei nachfolgenden Anforderungen, Analysieren dieses Ticket, um die Identität des Benutzers zu bestimmen. Für eine Web-Anwendung, um Benutzerkonten zu unterstützen müssen wir noch implementieren einen Benutzerspeicher und Hinzufügen von Funktionen zur Anmeldeinformationen zu überprüfen, neue Benutzer und einer Vielzahl von anderen kontobezogene Benutzeraufgaben registrieren.

Vor dem ASP.NET 2.0 wurden Entwickler auf die Hookfunktion für diese Benutzer kontobezogene Aufgaben zu implementieren. Glücklicherweise das ASP.NET-Team dieser Schwierigkeit erkannt und die Mitgliedschaft-Framework mit ASP.NET 2.0 eingeführt. Das Framework Mitgliedschaft ist eine Reihe von Klassen in .NET Framework, die eine programmgesteuerte Schnittstelle für die Erledigung Core Benutzeraufgaben kontobezogene bereitstellen. Dieses Framework wird erstellt, über die [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), dem können Entwickler benutzerdefinierte Implementierung in eine standardisierte API eingebunden.

Entsprechend der Anleitung unter dem <a id="Tutorial1"> </a> [ *Sicherheitsgrundlagen und die ASP.NET-Unterstützung* ](../introduction/security-basics-and-asp-net-support-vb.md) Lernprogramm .NET Framework enthält zwei integrierte Mitgliedschaftsanbieter: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) und [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Wie der Name schon sagt, den `SqlMembershipProvider` verwendet eine Microsoft SQL Server-Datenbank als Speicher des Benutzers. Um diesen Anbieter in einer Anwendung verwenden zu können, müssen wir dem Anbieter mitteilen, welche Datenbank als Speicher zu verwenden ist. Man kann, die `SqlMembershipProvider` erwartet, dass die Benutzer-Speicherdatenbank auf bestimmte Tabellen, Sichten und gespeicherte Prozeduren haben. Wir müssen diese erwartete Schema mit der ausgewählten Datenbank hinzufügen.

In diesem Lernprogramm beginnt mit dem Untersuchen von Techniken für das erforderliche Schema der Datenbank hinzufügen, um verwenden die `SqlMembershipProvider`. Nach, die wir untersuchen Sie die wichtigsten Tabellen im Schema und erläutert deren Zweck und Wichtigkeit. In diesem Lernprogramm endet mit einem Betrachtung der Verwendung einer ASP.NET-Anwendung mitteilen, welcher Anbieter ist die Mitgliedschaft-Framework verwendet werden soll.

Fangen wir an!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Schritt 1: Entscheiden, platzieren Sie den Speicher des Benutzers

Eine ASP.NET-Anwendung Daten werden häufig in einer Reihe von Tabellen in einer Datenbank gespeichert. Bei der Implementierung der `SqlMembershipProvider` Datenbankschema wir müssen Sie entscheiden, ob das Schema für die Mitgliedschaft in der gleichen Datenbank wie die Anwendungsdaten oder in einer anderen Datenbank platziert werden.

Suchen das Schema für die Mitgliedschaft in der gleichen Datenbank wie die Anwendungsdaten aus den folgenden Gründen sollten:

- **Verwaltbarkeit** eine Anwendung, deren Daten in einer Datenbank gekapselt, ist leichter zu verstehen, verwalten und als eine Anwendung mit zwei getrennten Datenbanken bereitstellen.
- **Relationale Integrität** durch suchen die Mitgliedschaft bezogenen Tabellen in derselben Datenbank wie die Anwendung es Tabellen ist möglich, herstellen [foreign Key-Einschränkungen](http://en.wikipedia.org/wiki/Foreign_key) zwischen der primären Schlüssel in der Mitgliedschaft-bezogenen Tabellen und verknüpften Tabellen.

Trennen die Benutzerdaten speichern und die Anwendung in separaten Datenbanken ist nur sinnvoll, wenn Sie mehrere Anwendungen haben, dass jeweils separate Datenbanken verwenden, aber einen allgemeine Benutzerspeicher gemeinsam nutzen muss.

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Die Anwendung, die wir erstellen müssen, da das zweite Lernprogramm eine Datenbank noch nicht benötigt hat. Wir benötigen ein jetzt jedoch für den Speicher des Benutzers. Wir erstellen eine und hinzufügen, das erforderliche Schema der `SqlMembershipProvider` Anbieter (Siehe Schritt2).

> [!NOTE]
> In diesem Lernprogramm Reihe wir verwenden eine [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) Datenbank zum Speichern von unseren Anwendungstabellen und `SqlMembershipProvider` Schema. Diese Entscheidung wurde getroffen haben, zwei Gründen: zuerst, aufgrund dessen Kosten - frei - Express Edition ist die am häufigsten readably zugänglich Version von SQL Server 2005; Zweitens können SQL Server 2005 Express Edition-Datenbanken platziert werden, direkt in der Webanwendung `App_Data` Ordner, sodass ein Kinderspiel verpacken die Datenbank und-Webanwendung zusammen in einer ZIP-Datei und ihn ohne besonderen Setup-Anweisungen neu bereitstellen oder Konfigurationsoptionen. Wenn Sie lieber mit einer nicht - Express Edition-Version von SQL Server nachvollziehen möchten, können Sie hierher. Die Schritte sind nahezu identisch. Die `SqlMembershipProvider` Schema wird mit einer Version von Microsoft SQL Server 2000 arbeiten und einrichten.

Projektmappen-Explorer mit der Maustaste auf die `App_Data` Ordner, und wählen Sie auf Neues Element hinzufügen. (Wenn Sie nicht sehen ein `App_Data` Ordner des Projekts mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, wählen Sie ASP.NET-Ordner hinzufügen, und wählen Sie `App_Data`.) Wählen Sie im Dialogfeld "Neues Element hinzufügen" zum Hinzufügen einer neuen SQL-Datenbank mit dem Namen `SecurityTutorials.mdf`. In diesem Lernprogramm fügen wir der `SqlMembershipProvider` Schema in dieser Datenbank; in den nachfolgenden Lernprogrammen, die wir zusätzliche erstellen Tabellen erfassen unsere Anwendungsdaten verwendet werden.


[![Hinzufügen einer neuen SQL­Datenbank mit dem Namen SecurityTutorials.mdf-Datenbank auf den Ordner "App_Data"](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen SQL-Datenbank mit dem Namen `SecurityTutorials.mdf` -Datenbank in die `App_Data` Ordner ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Hinzufügen einer Datenbank, die `App_Data` Ordner enthält es automatisch in der Datenbank-Explorer-Ansicht. (In der nicht - Express Edition-Version von Visual Studio wird die Datenbank-Explorer den Server-Explorer bezeichnet.) Wechseln Sie zu der Datenbank-Explorer und erweitern Sie den gerade hinzugefügten `SecurityTutorials` Datenbank. Wenn Sie die Datenbank-Explorer nicht auf dem Bildschirm angezeigt werden, wechseln Sie zum Menü "Ansicht" und Datenbank-Explorer auswählen oder Strg + Alt + S drücken. Wie in Abbildung 2 gezeigt, die `SecurityTutorials` Datenbank ist leer: sie enthält keine Tabellen, die keine Sichten und keine gespeicherten Prozeduren.


[![Die SecurityTutorials-Datenbank ist derzeit leer.](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Abbildung 2**: die `SecurityTutorials` Datenbank ist derzeit leer ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Schritt 2: Hinzufügen der`SqlMembershipProvider`Schema in der Datenbank

Die `SqlMembershipProvider` erfordert einen bestimmten Satz von Tabellen, Sichten und gespeicherten Prozeduren in der Benutzerdatenbank-Store installiert werden. Diese erforderlichen Datenbankobjekte können hinzugefügt werden, mithilfe der [ `aspnet_regsql.exe` Tool](https://msdn.microsoft.com/library/ms229862.aspx). Diese Datei befindet sich der `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` Ordner.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool bietet Funktionen über die Befehlszeile und eine grafische Benutzeroberfläche. Die grafische Benutzeroberfläche Benutzerfreundlicher ist und was wir in diesem Lernprogramm untersucht wird. Die Befehlszeilenschnittstelle ist hilfreich, wenn das Hinzufügen der `SqlMembershipProvider` Schema automatisiert werden muss, z. B. in Build Skripts oder Testszenarien automatisiert.

Die `aspnet_regsql.exe` Tool wird verwendet, um das Hinzufügen oder Entfernen von *ASP.NET-Anwendungsdiensten* in eine angegebene SQL Server-Datenbank. Die ASP.NET-Anwendungsdiensten umfassen die Schemas für die `SqlMembershipProvider` und `SqlRoleProvider`, zusammen mit der Schemas für die SQL-basierten Anbieter für andere Frameworks ASP.NET 2.0. Es müssen zwei Bits optional Informationen zum Bereitstellen der `aspnet_regsql.exe` Tool:

- Gibt an, ob wir möchten hinzufügen oder Entfernen von Application-Dienste und
- Die Datenbank, zum Hinzufügen oder entfernen die Anwendung Dienste schema

In der Bestätigung für die Datenbank zu verwenden, die `aspnet_regsql.exe` Tool stellt uns, geben Sie den Namen des Servers, der die Datenbank befindet sich auf die Anmeldeinformationen zum Herstellen einer Verbindung mit der Datenbank und den Datenbanknamen. Wenn Sie die nicht - Express Edition von SQL Server verwenden, sollten Sie diese Informationen bereits wissen, wie die gleichen Informationen, die Sie über eine Verbindungszeichenfolge angeben müssen, bei der Arbeit mit der Datenbank über eine ASP.NET-Webseite. Namen des Servers und der Datenbank bestimmen, bei Verwendung einer SQL Server 2005 Express Edition-Datenbank in der `App_Data` Ordner ist jedoch etwas komplizierter.

Im folgenden Abschnitt werden eine einfache Möglichkeit zum Angeben des Servers und der Datenbank namens für eine SQL Server 2005 Express Edition-Datenbank in den `App_Data` Ordner. Wenn Sie nicht gerne überspringen, das Installieren von SQL Server 2005 Express Edition verwenden, werden die Anwendungsdienste-Abschnitt.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Bestimmen die Server- und Datenbankname für SQL Server 2005 Express Edition-Datenbank in der`App_Data`Ordner

Zum Verwenden der `aspnet_regsql.exe` Tool wir die Server- und -Namen kennen müssen. Der Servername ist `localhost\InstanceName`. Sehr wahrscheinlich die *InstanceName* ist `SQLExpress`. Jedoch, wenn Sie SQL Server 2005 Express Edition manuell installiert (d. h. Sie nicht installieren es automatisch während der Installation von Visual Studio), ist es möglich, dass Sie einen anderen Instanznamen ausgewählt.

Der Datenbankname ist etwas komplizierter, um zu bestimmen. Datenbanken in der `App_Data` Ordner haben normalerweise einen Datenbanknamen an, die enthält eine [global eindeutigen Bezeichner](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) zusammen mit dem Pfad zur Datenbankdatei. Wir müssen diesen Datenbanknamen zu bestimmen, um die Anwendung Dienstschema durch Hinzufügen `aspnet_regsql.exe`.

Die einfachste Möglichkeit zum Ermitteln der Datenbankname ist über SQL Server Management Studio untersuchen. SQL Server Management Studio bietet eine grafische Oberfläche zum Verwalten von SQL Server 2005-Datenbanken, aber es ist nicht im Lieferumfang der Express Edition von SQL Server 2005. Die gute Nachricht ist, die [download](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) die kostenlose Express Edition von SQL Server Management Studio.

> [!NOTE]
> Wenn Sie auch eine nicht - Express Edition-Version von SQL Server 2005 auf Ihrem PC installiert haben, ist die Vollversion von Management Studio wahrscheinlich installiert. Die vollständige Version können der Name der Datenbank bestimmt die gleichen Schritte wie unten kurz umrissen, für die Express Edition.

Starten Sie durch das Schließen von Visual Studio, um sicherzustellen, dass alle Sperren, die von Visual Studio auf die Datenbankdatei auferlegt geschlossen werden. Als Nächstes starten Sie SQL Server Management Studio und Verbinden mit der `localhost\InstanceName` -Datenbank für SQL Server 2005 Express Edition. Wie bereits erwähnt, Chancen sind der Instanzname `SQLExpress`. Wählen Sie für die Authentifizierungsoption Windows-Authentifizierung.


[![Herstellen einer Verbindung mit SQL Server 2005 Express Edition-Instanz](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Abbildung 3**: Herstellen einer Verbindung mit der SQL Server 2005 Express Edition-Instanz ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Nach dem Herstellen einer Verbindung mit der Instanz von SQL Server 2005 Express Edition, zeigt Management Studio den Ordner für Datenbanken, die Sicherheitseinstellungen, Serverobjekte und So weiter. Wenn Sie die Registerkarte "Datenbanken" erweitern, sehen Sie, die die `SecurityTutorials.mdf` Datenbank ist *nicht* in der Datenbankinstanz - registriert wir müssen zunächst die Datenbank anfügen.

Mit der rechten Maustaste auf den Ordner "Datenbanken", und wählen Sie anfügen aus dem Kontextmenü. Dadurch wird das Dialogfeld Anfügen von Datenbanken angezeigt. Klicken Sie hier, klicken Sie auf die Schaltfläche "hinzufügen", navigieren Sie zu der `SecurityTutorials.mdf` Datenbank, und klicken Sie auf OK. Abbildung 4 zeigt das Dialogfeld "Datenbanken anfügen" nach der `SecurityTutorials.mdf` Datenbank ausgewählt wurde. Abbildung 5 zeigt Management Studio im Objekt-Explorer aus, nachdem die Datenbank erfolgreich angefügt wurde.


[![Fügen Sie die Datenbank SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Abbildung 4**: Fügen Sie der `SecurityTutorials.mdf` Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![Die SecurityTutorials.mdf-Datenbank wird in den Ordner "Datenbanken" angezeigt.](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Abbildung 5**: die `SecurityTutorials.mdf` Datenbank wird in den Ordner "Datenbanken" angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Wie in Abbildung 5 gezeigt, die `SecurityTutorials.mdf` Datenbank hat einen stattdessen abstruse Namen. Wir ändern, um einen einprägsameren (und einfacher eingegeben) Namen. Mit der rechten Maustaste auf die Datenbank, wählen Sie im Kontextmenü den Befehl Umbenennen, und benennen Sie sie `SecurityTutorialsDatabase`. Dies ändert nicht den Dateinamen, nur der Namen der Datenbank verwendet, um sich zu SQL Server identifizieren.


[![Benennen Sie die Datenbank in SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Abbildung 6**: Benennen Sie die Datenbank in `SecurityTutorialsDatabase`([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


Wir wissen zu diesem Zeitpunkt die Server- und Namen für die `SecurityTutorials.mdf` Datenbankdatei: `localhost\InstanceName` und `SecurityTutorialsDatabase`zugeordnet. Wir können jetzt die Anwendungsdienste durch Installieren der `aspnet_regsql.exe` Tool.

### <a name="installing-the-application-services"></a>Installieren die Anwendungsdienste

Zum Starten der `aspnet_regsql.exe` -tool, wechseln Sie im Startmenü aus, und wählen Sie ausführen. Geben Sie `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` in das Textfeld ein, und klicken Sie auf OK. Alternativ können Sie Windows-Explorer verwenden, um einen Drilldown zum entsprechenden Ordner, und doppelklicken Sie auf die `aspnet_regsql.exe` Datei. Beide Vorgehensweisen werden die gleichen Ergebnisse net.

Ausführen der `aspnet_regsql.exe` Tool ohne Befehlszeilenargumente startet die grafische Benutzeroberfläche für die ASP.NET SQL Server-Setup-Assistenten. Der Assistent erleichtert das Hinzufügen oder entfernen die ASP.NET-Anwendungsdienste für eine angegebene Datenbank. Der erste Bildschirm des Assistenten angezeigt, die in Abbildung 7: Beschreibt den Zweck des Tools.


[![Verwenden Sie die ASP.NET SQL Server-Setup-Assistent erleichtert das Mitgliedschaft Schema hinzufügen](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Abbildung 7**: Verwenden Sie die ASP.NET SQL Server-Setup-Assistent sorgt für Mitgliedschaft Schema hinzufügen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


Der zweite Schritt im Assistenten werden wir gefragt, gibt an, ob wir die Anwendungsdienste hinzufügen oder entfernen möchten. Da wir die Tabellen, Sichten und gespeicherte Prozeduren für möchten die `SqlMembershipProvider`, konfigurieren Sie SQL Server-Anwendungsoption-Dienste auswählen. Später, wenn Sie dieses Schema aus der Datenbank entfernen möchten, führen Sie diesen Assistenten erneut aus, aber stattdessen wählen Sie Anwendungsinformationen Services entfernen aus einer vorhandenen Datenbankoption aus.


[![Wählen Sie die SQLServer für die Anwendung Dienste Option konfigurieren.](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Abbildung 8**: Wählen Sie das Konfigurieren von SQL Server für die Anwendung Dienste Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


Der dritte Schritt aufgefordert, die Datenbankinformationen: den Namen des Servers, Informationen zur Benutzerauthentifizierung und den Datenbanknamen. Wenn Sie dieses Tutorial folgen wurde haben und hinzugefügt haben, die `SecurityTutorials.mdf` Datenbank `App_Data`, angefügt, `localhost\InstanceName`, und umbenannt `SecurityTutorialsDatabase`, verwenden Sie die folgenden Werte:

- Server: `localhost\InstanceName`
- Windows-Authentifizierung
- Datenbank: `SecurityTutorialsDatabase`


[![Geben Sie die Datenbankinformationen](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Abbildung 9**: Geben Sie die Datenbankinformationen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Klicken Sie nach der Eingabe der Datenbankinformationen auf "Weiter". Der letzte Schritt sind die Schritte zusammengefasst, die ausgeführt werden soll. Klicken Sie auf "Weiter", um die Anwendungsdienste zu installieren und dann auf Fertig stellen, um den Assistenten abzuschließen.

> [!NOTE]
> Wenn Sie Management Studio, fügen Sie die Datenbank, und benennen Sie die Datenbankdatei verwendet, achten Sie darauf, dass Sie zum Trennen der Datenbank, und schließen Management Studio vor dem erneuten Öffnen des Visual Studio. Beim Trennen der `SecurityTutorialsDatabase` Datenbank, mit der rechten Maustaste auf den Datenbanknamen und wählen Sie aus dem Menü "Aufgaben" trennen.

Klicken Sie nach Abschluss des Assistenten zurück zu Visual Studio, und navigieren Sie zu der Datenbank-Explorer. Erweitern Sie den Ordner Tabellen. Daraufhin sollte eine Reihe von Tabellen, deren Namen mit dem Präfix beginnen `aspnet_`. Ebenso kann eine Vielzahl von Sichten und gespeicherten Prozeduren in den Sichten und gespeicherten Prozeduren Ordnern gefunden werden. Diese Datenbankobjekte bilden zusammen den Anwendungsschema Dienste. Untersuchen wir die Mitgliedschaft und rollenspezifische Datenbankobjekte in Schritt 3.


[![Eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren der Datenbank hinzugefügt wurden](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Abbildung 10**: eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren wurden hinzugefügt, die Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> Die `aspnet_regsql.exe` grafische Benutzeroberfläche des Tools wird das Schema der gesamten Anwendung Dienste installiert. Jedoch beim Ausführen von `aspnet_regsql.exe` über die Befehlszeile können Sie angeben, welche Anwendungsspezifischer services-Komponenten zu installieren (oder entfernen). Deshalb anzeigt, wenn Sie nur die Tabellen hinzufügen möchten, und gespeicherte Prozeduren für die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter, führen Sie `aspnet_regsql.exe` über die Befehlszeile aus. Alternativ Sie können manuell ausführen, die entsprechende Teilmenge von T-SQL-Erstellungsskripts verwendeten `aspnet_regsql.exe`. Diese Skripts befinden sich der `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` Ordner mit Namen wie `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`und so weiter.

Zu diesem Zeitpunkt haben wir die erforderlichen Datenbankobjekte erstellt die `SqlMembershipProvider`. Jedoch trotzdem müssen wir die Mitgliedschaft Framework angewiesen wird, dass es verwendet werden soll die `SqlMembershipProvider` (versus, sagen, die `ActiveDirectoryMembershipProvider`) und dass der `SqlMembershipProvider` verwenden, sollten die `SecurityTutorials` Datenbank. Sehen wir uns an, welche Anbieter verwendet und zum Anpassen der Einstellungen für den ausgewählten Anbieter in Schritt 4. Jedoch zunächst werfen wir einen tieferen Einblick in die Datenbankobjekte, die gerade erstellt wurden.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Schritt 3: Einen Blick auf das Schema Haupttabellen

Bei der Arbeit mit den Frameworks Mitgliedschaft und Rollen in einer ASP.NET-Anwendung werden die Implementierungsdetails vom Anbieter gekapselt. In Zukunft wir eine mit dieser Frameworks über den .NET Framework Verbindung werden Lernprogramme `Membership` und `Roles` Klassen. Bei Verwendung dieser allgemeinen APIs müssen wir nicht betreffen uns mit den Details auf niedriger Ebene, z. B. welche Abfragen ausgeführt werden oder welche Tabellen geändert werden, indem die `SqlMembershipProvider` und `SqlRoleProvider`.

Angesichts könnten wir zuverlässig die Mitgliedschaft und Rollen-Frameworks verwenden, ohne dass untersucht das Datenbankschema, die in Schritt2 erstellt haben. Beim Erstellen der Tabellen zum Speichern von Anwendungsdaten müssen wir Entitäten zu erstellen, die Benutzern oder Rollen beziehen. Vertrautheit mit ist können, um die `SqlMembershipProvider` und `SqlRoleProvider` Schemas beim Einrichten von foreign key Einschränkungen zwischen Datentabellen Anwendung und die Tabellen, die in Schritt2 erstellt haben. Darüber hinaus in bestimmten seltenen Fällen müssen wir möglicherweise die Benutzeroberfläche und Rolle speichert direkt auf Datenbankebene (statt über die `Membership` oder `Roles` Klassen).

### <a name="partitioning-the-user-store-into-applications"></a>Partitionieren den Speicher des Benutzers in Anwendungen

Die Mitgliedschaft und Rollen Frameworks sind so konzipiert, dass ein einzelnen Benutzer und die Rolle Speicher für viele verschiedene Anwendungen freigegeben werden kann. Eine ASP.NET-Anwendung, die die Domänenmitgliedschaft oder der Rollen-Frameworks verwendet, muss zu verwenden, welche Anwendungspartition angeben. Kurz gesagt, können sich mehrere Webanwendungen auf den gleichen Speicher für Benutzer und die Rolle aus. Abbildung 11 zeigt Benutzer und die Rolle speichert, die in drei Anwendungen partitioniert werden: HRSite CustomerSite und SalesSite. Diese drei Webanwendungen jeder haben ihre eigenen eindeutigen Benutzer und Rollen, aber sie alle ihre Benutzerinformationen Konto und die entsprechende physisch in den gleichen Datenbanktabellen gespeichert.


[![Benutzerkonten möglicherweise anwendungsübergreifende partitioniert](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Abbildung 11**: Konten möglicherweise werden partitioniert über mehrere Benutzeranwendungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


Die `aspnet_Applications` Tabelle wird diese Partitionen definiert. Jede Anwendung, die die Datenbank zum Speichern von Informationen für das Benutzerkonto verwendet, wird durch eine Zeile in dieser Tabelle dargestellt. Die `aspnet_Applications` -Tabelle enthält vier Spalten: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, und `Description`.`ApplicationId` ist vom Typ [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) und ist der Primärschlüssel der Tabelle; `ApplicationName` einen eindeutigen Human benutzerfreundliche Namen für jede Anwendung enthält.

Die anderen Mitgliedschaft und rollenbezogenen Tabellen verknüpfen, an die `ApplicationId` Feld `aspnet_Applications`. Z. B. die `aspnet_Users` Tabelle, die einen Datensatz für jedes Benutzerkonto enthält, hat ein `ApplicationId` foreign Key-Feld; Ditto für die `aspnet_Roles` Tabelle. Die `ApplicationId` Feld in diesen Tabellen der Anwendungspartition gibt das Benutzerkonto oder die Rolle angehört.

### <a name="storing-user-account-information"></a>Das Speichern von Benutzerkontoinformationen

Benutzerkontoinformationen wird in zwei Tabellen untergebrachte: `aspnet_Users` und `aspnet_Membership`. Die `aspnet_Users` Tabelle enthält die Felder, die wichtige Benutzerkontoinformationen enthalten. Die drei wichtigsten Spalten sind:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` ist der Primärschlüssel (vom Typ `uniqueidentifier`). `UserName` ist vom Typ `nvarchar(256)` und sowie das Kennwort, bildet die Anmeldeinformationen des Benutzers. (Das Kennwort eines Benutzers befindet sich in der `aspnet_Membership` Tabelle.) `ApplicationId` verknüpft das Benutzerkonto, das auf eine bestimmte Anwendung in `aspnet_Applications`. Es ist ein zusammengesetzter [ `UNIQUE` Einschränkung](https://msdn.microsoft.com/library/ms191166.aspx) auf die `UserName` und `ApplicationId` Spalten. Dadurch wird sichergestellt, dass in einer bestimmten Anwendung jeder Benutzername eindeutig ist, noch für dieselbe können `UserName` in verschiedenen Anwendungen verwendet werden.

Die `aspnet_Membership` Tabelle enthält zusätzliche Benutzerkontoinformationen, z. B. das Kennwort des Benutzers, e-Mail-Adresse, der letzten Anmeldung Datum und Uhrzeit, und So weiter. Besteht eine 1: 1-Entsprechung zwischen Datensätzen in der `aspnet_Users` und `aspnet_Membership` Tabellen. Diese Beziehung wird sichergestellt, indem Sie die `UserId` Feld `aspnet_Membership`, die als Primärschlüssel der Tabelle dient. Wie die `aspnet_Users` Tabelle `aspnet_Membership` enthält ein `ApplicationId` Feld, das diese Informationen zu einer bestimmten Anwendungspartition verknüpft.

### <a name="securing-passwords"></a>Sichern von Kennwörtern

Kennwortinformationen befindet sich in der `aspnet_Membership` Tabelle. Die `SqlMembershipProvider` für Kennwörter in der Datenbank mit einer der folgenden drei Methoden gespeichert werden können:

- **Clear** -das Kennwort wird in der Datenbank als nur-Text gespeichert. Ich dringend davon abgeraten, verwenden diese Option aus. Wenn die Datenbank es von einem Hacker sein, der eine Hintertür oder einen verärgerten Mitarbeiter Datenbankzugriff hat - findet gefährdet ist-müssen Anmeldeinformationen für jeden einzelnen Benutzer für das übernehmen.
- **Ein Hashwert erstellt** --Kennwörter werden mithilfe eines unidirektionalen Hashalgorithmus und eines zufällig generierten Saltwerts. Dieser Hashwert (zusammen mit den Salt-Wert) wird in der Datenbank gespeichert.
- **Verschlüsselt** -eine verschlüsselte Version des Kennworts in der Datenbank gespeichert ist.

Verwendete Kennwort Speicher Technik richtet sich nach der `SqlMembershipProvider` festgelegten Einstellungen `Web.config`. Betrachten wir Anpassen der `SqlMembershipProvider` Einstellungen in Schritt 4. Das Standardverhalten besteht darin den Hash des Kennworts zu speichern.

Die Spalten, die verantwortlich für das Speichern des Kennworts sind `Password`, `PasswordFormat`, und `PasswordSalt`. `PasswordFormat` ist ein Feld vom Typ `int` , deren Wert angibt, das Verfahren zum Speichern des Kennworts verwendet: 0 für deaktiviert; 1 für Hashed; 2 für verschlüsselt. `PasswordSalt` wird eine zufällig generierte Zeichenfolge unabhängig von der verwendete Technik Speicher Kennwort zugewiesen. der Wert des `PasswordSalt` wird nur verwendet werden, wenn der Hash des Kennworts zu berechnen. Schließlich die `Password` Spalte enthält die tatsächlichen Kennwortdaten, z. B. auf das nur-Text-Kennwort, den Hash des Kennworts oder das verschlüsselte Kennwort.

Tabelle 1 wird veranschaulicht, wie diese drei Spalten für die verschiedenen Speicher Techniken aussehen könnte beim Speichern des Kennworts MySecret! sein.

| **Speicher-Technik&lt;\_o3a\_p /&gt;** | **Kennwort&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Ein Hashwert erstellt | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Verschlüsselt | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabelle 1**: Beispielwerte für die Kennwort-bezogene Felder beim Speichern der Kennwort MySecret!

> [!NOTE]
> Die bestimmten Verschlüsselung oder von verwendeten Hashalgorithmus die `SqlMembershipProvider` richtet sich nach den Einstellungen in der `<machineKey>` Element. Dieses Konfigurationselements in Schritt 3 des erörtert die <a id="Tutorial3"> </a> [ *Konfiguration der Formularauthentifizierung und erweiterte Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Lernprogramm.

### <a name="storing-roles-and-role-associations"></a>Das Speichern von Rollen und Rolle Zuordnungen

Das Rollen-Framework kann Entwickler einen Satz von Rollen zu definieren und angeben, welche Benutzer welche Rollen angehören. Diese Informationen werden in der Datenbank über zwei Tabellen erfasst: `aspnet_Roles` und `aspnet_UsersInRoles`. Jeder Datensatz in der `aspnet_Roles` Tabelle stellt eine Rolle für eine bestimmte Anwendung dar. Ähnlich wie die `aspnet_Users` Tabelle, die `aspnet_Roles` Tabelle enthält drei Spalten, die für unsere Diskussion relevant sind:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` ist der Primärschlüssel (vom Typ `uniqueidentifier`). `RoleName` ist vom Typ `nvarchar(256)`. Und `ApplicationId` verknüpft das Benutzerkonto, das auf eine bestimmte Anwendung in `aspnet_Applications`. Es ist ein zusammengesetzter `UNIQUE` -Einschränkung für die `RoleName` und `ApplicationId` Spalten, um sicherzustellen, dass in einer bestimmten Anwendung jeder Rollenname eindeutig ist.

Die `aspnet_UsersInRoles` Tabelle dient als eine Zuordnung zwischen Benutzern und Rollen. Es gibt nur zwei Spalten - `UserId` und `RoleId` – und sie bilden zusammen einen zusammengesetzten Primärschlüssel.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Schritt 4: Angeben des Anbieters und Anpassen von Einstellungen

Alle Frameworks, die das Anbietermodell – z. B. die Mitgliedschaft und Rollen Frameworks - unterstützen fehlender Implementierungsdetails selbst und stattdessen diese Aufgabe delegieren, auf eine Anbieterklasse. Im Fall von der Mitgliedschaft-Framework die `Membership` Klasse definiert die API zum Verwalten von Benutzerkonten, aber es gibt keine Interaktion zwischen direkt mit beliebigen Benutzer speichern. Vielmehr die `Membership` -Klasse Methoden übergeben Sie die Anforderung an den konfigurierten Anbieter – wir verwenden die `SqlMembershipProvider`. Wenn wir Aufrufen einer der Methoden in der `Membership` -Klasse, wie das Framework Mitgliedschaft kennt den Aufruf zum Delegieren der `SqlMembershipProvider`?

Die `Membership` -Klasse verfügt über eine [ `Providers` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , einen Verweis auf alle registrierten Anbieterklassen für die Verwendung durch das Framework für die Mitgliedschaft enthält. Jeder registrierte Anbieter verfügt über einen zugehörigen Namen und Typ. Der Name bietet Human benutzerfreundliche Möglichkeit, einen bestimmten Anbieter in verweisen die `Providers` Sammlung, während Sie den Typ der Anbieterklasse identifiziert. Darüber hinaus kann jede registrierte Anbieter Konfigurationseinstellungen enthalten. Konfigurationseinstellungen für die Mitgliedschaft Framework zählen `PasswordFormat` und `requiresUniqueEmail`, u. a. viele. Siehe Tabelle 2 für eine vollständige Liste der von verwendeten Konfigurationseinstellungen die `SqlMembershipProvider`.

Die `Providers` Inhalt-Eigenschaft des über die Konfigurationseinstellungen für die Webanwendung angegeben werden. Standardmäßig verfügen alle Webanwendungen über einen Anbieter namens `AspNetSqlMembershipProvider` vom Typ `SqlMembershipProvider`. In diesem Standardmitgliedschaftsanbieter registriert ist `machine.config` (am `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Als Markup oben dargestellt die [ `<membership>` Element](https://msdn.microsoft.com/library/1b9hw62f.aspx) definiert die Konfigurationseinstellungen für die Mitgliedschaft Framework während der [ `<providers>` untergeordnetes Element](https://msdn.microsoft.com/library/6d4936ht.aspx) gibt den registrierten Anbieter. Anbieter hinzugefügt werden, oder mit entfernt die [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) oder [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) Elemente; verwenden Sie die [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) Element alle derzeit entfernen registrierten Anbieter. Als Markup oben dargestellt `machine.config` Fügt einen Anbieter mit dem Namen `AspNetSqlMembershipProvider` vom Typ `SqlMembershipProvider`.

Zusätzlich zu den `name` und `type` Attribute, die `<add>` Element enthält Attribute, die die Werte für verschiedene Konfigurieren von Einstellungen definieren. Tabelle 2 werden die verfügbaren `SqlMembershipProvider`-bestimmte Konfigurationseinstellungen festgelegt werden, zusammen mit einer Beschreibung der einzelnen.

> [!NOTE]
> In Tabelle 2 notiert haben Standardwerte verweisen auf die Standardwerte definiert, der `SqlMembershipProvider` Klasse. Beachten Sie, dass nicht alle Konfigurationseinstellungen in `AspNetSqlMembershipProvider` entsprechen die Standardwerte für die `SqlMembershipProvider` Klasse. Angenommen, nicht in einem Mitgliedschaftsanbieter angegeben die `requiresUniqueEmail` standardmäßig auf "true" festlegen. Allerdings die `AspNetSqlMembershipProvider` dieser Standardwert von Wert explizit angeben, überschreibt `false`.

| **Festlegen von&lt;\_o3a\_p /&gt;** | **Beschreibung&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Beachten Sie, dass die Mitgliedschaft für eine einzelne Benutzerspeicher anwendungsübergreifende partitioniert werden können. Diese Einstellung gibt den Namen der Anwendungspartition, die von der Mitgliedschaftsanbieter verwendet. Wenn dieser Wert wird nicht explizit angegeben, es wird festgelegt, während der Laufzeit auf den Wert des virtuellen Stammpfad der Anwendung. |
| `commandTimeout` | Gibt den Timeoutwert für SQL-Befehl (in Sekunden) an. Der Standardwert ist 30. |
| `connectionStringName` | Der Name der Verbindungszeichenfolge in der `<connectionStrings>` Element für die Verbindung mit der Benutzerdatenbank-Speicher verwendet. Dieser Wert ist erforderlich. |
| `description` | Enthält eine Human benutzerfreundliche Beschreibung des registrierten Anbieters. |
| `enablePasswordRetrieval` | Gibt an, ob Benutzer ihr Kennwort vergessen haben abgerufen werden können. Der Standardwert ist `false`. |
| `enablePasswordReset` | Gibt an, ob Benutzer zulässig sind, um sein Kennwort zurückzusetzen. Wird standardmäßig auf `true` festgelegt. |
| `maxInvalidPasswordAttempts` | Die maximale Anzahl von nicht erfolgreichen Anmeldeversuche, die für einen bestimmten Benutzer bei der angegebenen `passwordAttemptWindow` , bevor der Benutzer gesperrt ist. Der Standardwert ist 5. |
| `minRequiredNonalphanumericCharacters` | Die minimale Anzahl von nicht-alphanumerische Zeichen, die in das Kennwort eines Benutzers angezeigt werden müssen. Dieser Wert muss zwischen 0 und 128 liegen; der Standard ist 1. |
| `minRequiredPasswordLength` | Die minimale Anzahl von Zeichen in einem Kennwort erforderlich sind. Dieser Wert muss zwischen 0 und 128 liegen; Der Standardwert ist 7. |
| `name` | Der Name des registrierten Anbieters. Dieser Wert ist erforderlich. |
| `passwordAttemptWindow` | Die Anzahl der Minuten, während der Fehler bei der Anmeldung, die Versuche nachverfolgt werden. Wenn ein Benutzer ungültige Anmeldeinformationen bereitstellt `maxInvalidPasswordAttempts` innerhalb dieser Zeiträume Fenster, sie gesperrt werden. Der Standardwert ist 10. |
| `PasswordFormat` | Das Speicherformat Kennwort: `Clear`, `Hashed`, oder `Encrypted`. Die Standardeinstellung ist `Hashed`. |
| `passwordStrengthRegularExpression` | Wenn angegeben, wird dieser reguläre Ausdruck verwendet, um die Stärke des ausgewählten Kennwort des Benutzers, beim Erstellen eines neuen Kontos oder auszuwerten, wenn Sie ihr Kennwort ändern. Der Standardwert ist eine leere Zeichenfolge. |
| `requiresQuestionAndAnswer` | Gibt an, ob ein Benutzer seine Sicherheitsfrage, die beim Abrufen oder das Zurücksetzen seines Kennworts beantworten muss. Der Standardwert ist `true`. |
| `requiresUniqueEmail` | Gibt an, ob alle Benutzerkonten in einer bestimmten Anwendungspartition eine eindeutige e-Mail-Adresse benötigen. Der Standardwert ist `true`. |
| `type` | Gibt den Typ des Anbieters. Dieser Wert ist erforderlich. |

**Tabelle 2**: Mitgliedschaft und `SqlMembershipProvider` -Konfigurationseinstellungen

Zusätzlich zu `AspNetSqlMembershipProvider`, andere Mitgliedschaftsanbieter möglicherweise auf ein Anwendungsbasis durch Hinzufügen von ähnlichen Markup registriert die `Web.config` Datei.

> [!NOTE]
> Das Framework Rollen funktioniert im Wesentlichen die gleiche Weise: Es ist eine eingetragene Standardrollenanbieter in `machine.config` und die registrierten Anbieter können angepasst werden, auf Basis von Anwendung in `Web.config`. Wir werden die Rollen-Framework und dessen Konfiguration Markup im Detail in einem späteren Lernprogramm untersucht.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Anpassen der`SqlMembershipProvider`Einstellungen

Die Standardeinstellung `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) hat seine `connectionStringName` -Attributsatz zur `LocalSqlServer`. Wie die `AspNetSqlMembershipProvider` -Anbieter, der Name der Verbindungszeichenfolge `LocalSqlServer` ist definiert `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Wie Sie sehen können, definiert diese Verbindungszeichenfolge eine SQL 2005 Express Edition-Datenbank finden Sie unter | DataDirectory|aspnetdb. mdf befindet. Die Zeichenfolge | "DataDirectory" | wird zur Laufzeit auf übersetzt die `~/App_Data/` Verzeichnis, sodass der Datenbankpfad | DataDirectory|aspnetdb. mdf befindet übersetzt `~/App_Data` / `aspnet.mdf`.

Wenn wir keine Anbieterinformationen zur Mitgliedschaft in der vorliegenden Anwendungsverzeichnis angegeben haben `Web.config` Datei, die Anwendung verwendet die registrierten Standardmitgliedschaftsanbieter, `AspNetSqlMembershipProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank ist nicht vorhanden, den die ASP.NET-Laufzeit automatisch erstellt, und fügen Sie die Anwendung Dienstschema hinzu. Allerdings wir nicht verwenden möchten die `aspnet.mdf` Datenbank; stattdessen verwendet werden soll die `SecurityTutorials.mdf` wir in Schritt2 erstellte Datenbank. Diese Änderung kann auf zwei Arten erfolgen:

- <strong>Geben Sie einen Wert für die</strong><strong>`LocalSqlServer`</strong><strong>Verbindungszeichenfolgenname in</strong><strong>`Web.config`</strong><strong>.</strong> Durch Überschreiben der `LocalSqlServer` Zeichenfolge Verbindungswert in `Web.config`, verwenden wir den registriert Standardmitgliedschaftsanbieter (`AspNetSqlMembershipProvider`) und haben sie die korrekte Arbeitsweise mit der `SecurityTutorials.mdf` Datenbank. Dieser Ansatz eignet sich gut, wenn Sie Inhalt mit der vom angegebenen Konfigurationseinstellungen sind `AspNetSqlMembershipProvider`. Weitere Informationen zu dieser Technik finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogbeitrag [Konfigurieren von ASP.NET 2.0-Anwendungsdienste für Verwenden von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Hinzufügen einen neuen registrierten Anbieter vom Typ</strong><strong>`SqlMembershipProvider`</strong><strong>und konfigurieren Sie die</strong><strong>`connectionStringName`</strong><strong>Einstellung aus, um auf dieverweisen</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank.</strong> Dieser Ansatz ist hilfreich in Szenarien, in dem anderen Konfigurationseigenschaften, zusätzlich zu den Datenbank-Verbindungszeichenfolge angepasst werden soll. Eigene Projekte verwenden ich immer dieser Ansatz aufgrund seiner Flexibilität und Lesbarkeit.

Bevor wir einen neuen registrierten Anbieter hinzufügen können, die verweist auf die `SecurityTutorials.mdf` Datenbank müssen Sie zuerst in eine entsprechende verbindungszeichenwert Hinzufügen der `<connectionStrings>` im Abschnitt `Web.config`. Das folgende Markup Fügt eine neue Verbindungszeichenfolge mit dem Namen `SecurityTutorialsConnectionString` , verweist der SQL Server 2005 Express Edition `SecurityTutorials.mdf` -Datenbank in die `App_Data` Ordner.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie eine alternative Datenbank-Datei verwenden, aktualisieren Sie die Verbindungszeichenfolge. Weitere Informationen über die richtige Verbindungszeichenfolge bilden, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).

Fügen Sie das folgende Mitgliedschaft Konfiguration Markup zum Rendern der `Web.config` Datei. Dieses Markup registriert einen neuen Anbieter mit dem Namen `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Zusätzlich zum Registrieren der `SecurityTutorialsSqlMembershipProvider` Anbieter, die oben genannten Markup definiert die `SecurityTutorialsSqlMembershipProvider` als Standardanbieter (über die `defaultProvider` Attribut in der `<membership>` Element). Beachten Sie, dass das Framework Mitgliedschaft mehrere registrierte Anbieter haben kann. Da `AspNetSqlMembershipProvider` registriert ist, als der erste Anbieter in `machine.config`, es dient als Standardanbieter, es sei denn, es anders angegeben.

Gegenwärtig kann die Anwendung zwei registrierten Anbieter: `AspNetSqlMembershipProvider` und `SecurityTutorialsSqlMembershipProvider`. Jedoch vor der Registrierung der `SecurityTutorialsSqlMembershipProvider` Anbieter, die wir, alle zuvor verhinderten konnte registrierten Anbieter durch Hinzufügen einer [ `<clear />` Element](https://msdn.microsoft.com/library/t062y6yc.aspx) unmittelbar vor unserer `<add>` Element. Dies würde löschen, die `AspNetSqlMembershipProvider` aus der Liste der registrierten Anbieter, was bedeutet, dass die `SecurityTutorialsSqlMembershipProvider` wäre die nur registrierte Mitgliedschaftsanbieter. Wenn wir diesen Ansatz verwendet, müssen wir nicht markieren der `SecurityTutorialsSqlMembershipProvider` als Standardanbieter, da es wäre die nur registrierten Mitgliedschaftsanbieter. Weitere Informationen zur Verwendung von `<clear />`, finden Sie unter [Using `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Beachten Sie, dass die `SecurityTutorialsSqlMembershipProvider`des `connectionStringName` Festlegen von Verweisen den gerade hinzugefügten `SecurityTutorialsConnectionString` Verbindungszeichenfolgenname und dass seine `applicationName` Einstellung auf einen Wert von SecurityTutorials festgelegt wurde. Darüber hinaus die `requiresUniqueEmail` Einstellung vorsieht `true`. Alle anderen Konfigurationsoptionen sind identisch mit den Werten in `AspNetSqlMembershipProvider`. Können Sie hier keine konfigurationsänderungen vornehmen, wenn Sie möchten. Sie konnten z. B. der Kennwortstärke dazu müssen zwei nicht-alphanumerische Zeichen anstelle eines, oder erhöhen die Länge des Kennworts auf acht Zeichen anstelle von sieben erhöhen.

> [!NOTE]
> Beachten Sie, dass die Mitgliedschaft für eine einzelne Benutzerspeicher anwendungsübergreifende partitioniert werden können. Des Mitgliedschaftsanbieters `applicationName` Einstellung gibt an, welche Anwendung den Anbieter verwendet, bei der Arbeit mit den Speicher des Benutzers. Es ist wichtig, dass Sie explizit einen Wert für Festlegen der `applicationName` Konfiguration festlegen, da Wenn die `applicationName` nicht explizit festgelegt ist, wird ihm zugewiesenen virtuellen Stammpfad der Webanwendung, zur Laufzeit. Dies funktioniert gut, solange der virtuelle Stammpfad der Anwendung nicht ändern, aber wenn Sie die Anwendung auf einen anderen Pfad verschieben der `applicationName` Einstellung geändert wird. In diesem Fall wird der Mitgliedschaftsanbieter funktionieren mit einer anderen Anwendungspartition als zuvor verwendet wurde. Vor der Übernahme erstellte Benutzerkonten werden in einer anderen Anwendungspartition befinden, und diese Benutzer werden nicht mehr in der Lage, bei der Website anmelden. Eine ausführliche Erklärung zu diesem Thema finden Sie unter [legen Sie immer die `applicationName` Eigenschaft beim Konfigurieren von ASP.NET 2.0 Mitgliedschaft und anderen Anbietern](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Zusammenfassung

An diesem Punkt haben wir eine Datenbank mit den Diensten für die konfigurierte Anwendung (`SecurityTutorials.mdf`) und haben unsere Webanwendung so konfiguriert, dass das Framework Mitgliedschaft verwendet die `SecurityTutorialsSqlMembershipProvider` Anbieter, die wir gerade registriert haben. Diese registrierten Anbieter ist vom Typ `SqlMembershipProvider` und verfügt über seine `connectionStringName` legen Sie auf die entsprechende Verbindungszeichenfolge (`SecurityTutorialsConnectionString`) und die zugehörige `applicationName` Wert explizit festgelegt.

Wir können nun das Framework Mitgliedschaft von der Anwendung verwenden. In den nächsten Lernprogrammen untersuchen wir neue Benutzerkonten erstellen. Im Anschluss an, dass wir Authentifizieren von Benutzern untersuchen, Benutzerbasierte Autorisierung und zusätzliche benutzerspezifischen Informationen in der Datenbank gespeichert.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Legen Sie immer die `applicationName` Eigenschaft beim Konfigurieren von ASP.NET 2.0 Mitgliedschaft und anderen Anbietern](https://weblogs.asp.net/scottgu/443634)
- [Konfigurieren von ASP.NET 2.0-Dienstanwendung mit SQLServer 2000 oder SQLServer 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Herunterladen von SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Untersuchen ASP.NET 2.0 s Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Die `<add>` -Element für Anbieter für Mitgliedschaft](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Die `<membership>` Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Die `<providers>` -Element für Mitgliedschaft](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Mithilfe von `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Direktes Arbeiten mit der `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Lehrvideos auf die Themen in diesem Lernprogramm

- [Grundlegendes zu ASP.NET-Mitgliedschaften](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurieren von SQL für die Arbeit mit Mitgliedschaftsschemas](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Ändern der Mitgliedschaftseinstellungen im Mitgliedschaftsschema](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird  *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Lead Prüfer für dieses Lernprogramm wurde Alicja Maziarz. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](storing-additional-user-information-cs.md)
> [Weiter](creating-user-accounts-vb.md)
