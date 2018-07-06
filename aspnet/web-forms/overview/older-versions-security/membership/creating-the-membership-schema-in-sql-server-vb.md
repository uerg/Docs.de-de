---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Erstellen des Mitgliedschaftsschemas in SQLServer (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial wird anhand der Verfahren für das erforderliche Schema in der Datenbank hinzufügen, um die SqlMembershipProvider verwenden gestartet. Danach wir WLAN...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 6fe5ab7e2748a7502f47cb29ecc0b93b6cb07d6b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396987"
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Erstellen des Mitgliedschaftsschemas in SQLServer (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> In diesem Tutorial wird anhand der Verfahren für das erforderliche Schema in der Datenbank hinzufügen, um die SqlMembershipProvider verwenden gestartet. Danach wird untersuchen Sie die wichtigsten Tabellen im Schema und erläutert deren Zweck und die Wichtigkeit. In diesem Tutorial endet mit einem Blick auf das von einer ASP.NET-Anwendung erfahren, welche Provider das mitgliedschaftsframework verwenden soll.


## <a name="introduction"></a>Einführung

Die vorherigen beiden Tutorials, die untersucht, unter Verwendung der Formularauthentifizierung zum Identifizieren von Websitebesucher. Das Forms-Authentifizierung-Framework erleichtert das für Entwickler, die einen Benutzer bei einer Website anmelden und diese auf der Seite besuchen, durch die Verwendung von Authentifizierungstickets merken müssen. Die `FormsAuthentication` Klasse enthält Methoden zum Generieren des Tickets und des Besuchers Cookies hinzugefügt. Die `FormsAuthenticationModule` untersucht alle eingehende Anforderungen, für Personen mit einem gültigen Authentifizierungsticket, erstellt und ordnet einen `GenericPrincipal` und `FormsIdentity` Objekt mit der aktuellen Anforderung. Die Formularauthentifizierung ist lediglich einen Mechanismus zum Gewähren des ein Authentifizierungsticket für einem Besucher, bei der Anmeldung in und, bei nachfolgenden Anforderungen, Analysieren dieses Ticket, um die Identität des Benutzers zu bestimmen. Für eine Webanwendung, um Benutzerkonten zu unterstützen müssen wir noch Implementierung eines Benutzerspeichers und Hinzufügen von Funktionen, um Anmeldeinformationen zu überprüfen, neue Benutzer, und die unzähligen anderen Aufgaben im Zusammenhang mit Konto von Benutzer zu registrieren.

Vor ASP.NET 2.0 befanden sich Entwickler auf der Hook für das implementieren alle diese Benutzer-Konto-bezogene Aufgaben. Glücklicherweise ist das ASP.NET-Team dieser Mangel erkannt und das mitgliedschaftsframework mit ASP.NET 2.0 eingeführt. Das mitgliedschaftsframework ist eine Reihe von Klassen in .NET Framework, die eine programmgesteuerte Schnittstelle zum Ausführen von Aufgaben im Zusammenhang mit Konto für Core Benutzer bereitstellen. Dieses Framework basiert auf der [Anbietermodell](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), wodurch Entwickler eine benutzerdefinierte Implementierung in einer standardisierten API integrieren können.

Siehe die <a id="Tutorial1"> </a> [ *Grundlagen der Sicherheit und Unterstützung für ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) Tutorial .NET Framework ausgeliefert wird, mit zwei integrierten Anbietern von Mitgliedschaft: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) und [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Wie der Name schon sagt, den `SqlMembershipProvider` verwendet eine Microsoft SQL Server-Datenbank als Speicher des Benutzers. Um diesen Anbieter in einer Anwendung verwenden zu können, müssen es um dem Anbieter mitzuteilen, welche Datenbank als Speicher verwendet. Wie Sie sich vorstellen können, die `SqlMembershipProvider` erwartet, dass die Benutzer-Speicherdatenbank um bestimmte Datenbanktabellen, Ansichten und gespeicherte Prozeduren zu erhalten. Wir müssen diese erwartete Schema der ausgewählten Datenbank hinzufügen.

Dieses Tutorial beginnt, mithilfe von Techniken für das erforderliche Schema in der Datenbank hinzufügen, um verwenden die `SqlMembershipProvider`. Danach wird untersuchen Sie die wichtigsten Tabellen im Schema und erläutert deren Zweck und die Wichtigkeit. In diesem Tutorial endet mit einem Blick auf das von einer ASP.NET-Anwendung erfahren, welche Provider das mitgliedschaftsframework verwenden soll.

Fangen wir an!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Schritt 1: Entscheiden, wo die Benutzer-Store

Daten von einer ASP.NET-Anwendung ist häufig in eine Reihe von Tabellen in einer Datenbank gespeichert. Bei der Implementierung der `SqlMembershipProvider` Datenbankschema müssen wir entscheiden, ob das Schema für die Mitgliedschaft in der gleichen Datenbank wie die Anwendungsdaten oder in einer anderen Datenbank zu speichern.

Ich empfehle, suchen das Schema für die Mitgliedschaft in der gleichen Datenbank wie die Anwendungsdaten aus den folgenden Gründen:

- **Verwaltbarkeit** eine Anwendung, deren Daten in einer Datenbank gekapselt, ist einfacher zu verstehen, verwalten und Bereitstellen als eine Anwendung mit zwei separate Datenbanken.
- **Relationale Integrität** durch die Mitgliedschaft bezogenen Tabellen in der gleichen Datenbank zu suchen, wie die Anwendung diese Tabellen ist möglich, herstellen [foreign Key-Einschränkungen](http://en.wikipedia.org/wiki/Foreign_key) zwischen den Primärschlüsseln in den Mitgliedschaft-bezogenen und verwandte Application-Tabellen.

Entkopplung von Benutzerdaten speichern und Anwendung auf unterschiedliche Datenbanken ist nur sinnvoll, wenn Sie über mehrere Anwendungen verfügen, dass jeweils separate Datenbanken verwenden, aber einen allgemeiner Store für Benutzer freigeben möchten.

### <a name="creating-a-database"></a>Erstellen einer Datenbank

Die Anwendung, die wir erstellen müssen, da das zweite Tutorial eine Datenbank noch nicht erforderlich ist. Wir benötigen einen jetzt jedoch für den Speicher des Benutzers. Wir erstellen, und klicken Sie dann hinzufügen, das Schema erforderlich, durch die `SqlMembershipProvider` Anbieter (Siehe Schritt2).

> [!NOTE]
> In dieser tutorialreihe verwenden wir eine [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) Datenbank zum Speichern von unserer Anwendungstabellen und die `SqlMembershipProvider` Schema. Diese Entscheidung wurde getroffen, aus zwei Gründen: zuerst aufgrund der Kosten – free – die Express Edition ist die am häufigsten readably barrierefreie Version des SQL Server 2005; Andererseits können SQL Server 2005 Express Edition-Datenbanken platziert werden, direkt in der Webanwendung `App_Data` Ordner, sodass ein Kinderspiel, Packen die Datenbank und-Webanwendung zusammen in einer ZIP-Datei und es spezielle Einrichtung ohne erneute Bereitstellung oder Konfigurationsoptionen. Wenn Sie mit einer nicht - Express Edition-Version von SQL Server nachvollziehen möchten, können. Die Schritte sind nahezu identisch. Die `SqlMembershipProvider` Schema arbeitet mit einer beliebigen Version von Microsoft SQL Server 2000 und einrichten.

Im Projektmappen-Explorer mit der Maustaste auf die `App_Data` Ordner, und wählen Sie auf Neues Element hinzufügen. (Wenn Sie nicht sehen ein `App_Data` Ordner in Ihrem Projekt mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, wählen Sie ASP.NET-Ordner hinzufügen und auswählen `App_Data`.) Wählen Sie im Dialogfeld "Neues Element hinzufügen" zum Hinzufügen einer neuen SQL-Datenbank, die mit dem Namen `SecurityTutorials.mdf`. In diesem Tutorial fügen wir die `SqlMembershipProvider` Schema auf diese Datenbank, in den nachfolgenden Tutorials wir zusätzliche erstellen Tabellen, um die Anwendungsdaten zu erfassen.


[![Hinzufügen einer neuen SQL-Datenbank, die mit dem Namen SecurityTutorials.mdf-Datenbank, in dem Ordner "App_Data"](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen SQL-Datenbank mit dem Namen `SecurityTutorials.mdf` -Datenbank in die `App_Data` Ordner ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Hinzufügen einer Datenbank, die `App_Data` Ordner enthält es automatisch in der Datenbank-Explorer-Ansicht. (In der nicht - Express Edition-Version von Visual Studio wird die Datenbank-Explorer den Server-Explorer bezeichnet.) Wechseln Sie zu der Datenbank-Explorer, und erweitern Sie den gerade hinzugefügten `SecurityTutorials` Datenbank. Wenn Sie Datenbank-Explorer nicht auf dem Bildschirm angezeigt werden, finden Sie unter dem Menü "Ansicht" und Datenbank-Explorer auswählen oder Strg + Alt + S drücken. Wie in Abbildung 2 gezeigt, die `SecurityTutorials` -Datenbank ist leer: er enthält keine Tabellen keine Ansichten und keine gespeicherten Prozeduren.


[![Die SecurityTutorials-Datenbank ist derzeit leer.](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Abbildung 2**: die `SecurityTutorials` Datenbank ist derzeit leer ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Schritt 2: Hinzufügen der`SqlMembershipProvider`Schema in der Datenbank

Die `SqlMembershipProvider` erfordert einen bestimmten Satz von Tabellen, Sichten und gespeicherten Prozeduren in der Benutzerdatenbank für den Store installiert werden. Diese erforderlichen Datenbankobjekte können hinzugefügt werden, mithilfe der [ `aspnet_regsql.exe` Tool](https://msdn.microsoft.com/library/ms229862.aspx). Diese Datei befindet sich der `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` Ordner.

> [!NOTE]
> Die `aspnet_regsql.exe` Tool bietet sowohl eine grafische Benutzeroberfläche als auch über die Befehlszeilenfunktionalität. Die grafische Oberfläche ist Benutzerfreundlicher und was wir in diesem Tutorial untersucht. Die Befehlszeilenschnittstelle ist nützlich, wenn das Hinzufügen der `SqlMembershipProvider` Schema muss automatisiert werden, z. B. in Build Skripts oder automatisierte Testszenarien.

Die `aspnet_regsql.exe` Tool wird verwendet, um das Hinzufügen oder Entfernen von *ASP.NET-Anwendungsdienste* in eine angegebene SQL Server-Datenbank. Die ASP.NET-Anwendungsdienste umfassen die Schemas für die `SqlMembershipProvider` und `SqlRoleProvider`, zusammen mit den für die SQL-basierten Anbieter für andere Frameworks ASP.NET 2.0. Wir müssen zwei Bits an Informationen zum Angeben der `aspnet_regsql.exe` Tool:

- Gibt an, ob beim Hinzufügen oder Entfernen von Anwendungsdiensten, werden sollten und
- Die Datenbank aus dem Hinzufügen oder entfernen Sie die Anwendung services-schema

Dass aufgefordert wird, für die Datenbank zu verwenden, die `aspnet_regsql.exe` Tool fordert uns, Sie geben den Namen des Servers, die Datenbank, auf die Sicherheitsanmeldeinformationen befindet für die Verbindung mit der Datenbank, und der Name der Datenbank. Wenn Sie den nicht - Express-Edition von SQL Server verwenden, sollten Sie diese Informationen bereits wissen, wie die gleichen Informationen, die Sie über eine Verbindungszeichenfolge angeben müssen, bei der Arbeit mit der Datenbank mithilfe einer ASP.NET-Webseite. Bestimmen der Namen des Servers und der Datenbank bei Verwendung einer SQL Server 2005 Express Edition-Datenbank in der `App_Data` Ordner ist jedoch etwas komplizierter.

Im folgenden Abschnitt werden eine einfache Möglichkeit zum Angeben des Namens Server und die Datenbank für eine SQL Server 2005 Express Edition-Datenbank in der `App_Data` Ordner. Wenn Sie nicht gerne zu überspringenden, das Installieren von SQL Server 2005 Express Edition verwenden, werden im Abschnitt für Anwendungsdienste.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Bestimmen die Server- und Datenbanknamen für SQL Server 2005 Express Edition-Datenbank in der`App_Data`Ordner

Zum Verwenden der `aspnet_regsql.exe` Tool, das wir wissen, Server-und Datenbank. Der Servername ist `localhost\InstanceName`. Höchstwahrscheinlich wird die *InstanceName* ist `SQLExpress`. Aber wenn Sie SQL Server 2005 Express Edition manuell installiert haben (d. h., Sie nicht installiert haben sie automatisch bei der Installation von Visual Studio), ist es möglich, dass Sie einen anderen Instanznamen ausgewählt.

Der Name der Datenbank ist etwas schwieriger ist, um zu bestimmen. Datenbanken in der `App_Data` Ordner verfügen in der Regel einen Datenbanknamen an, die enthält eine [global eindeutigen Bezeichner](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) zusammen mit dem Pfad zur Datei. Wir müssen diesen Datenbanknamen zu bestimmen, um die Anwendung Dienstschema durch Hinzufügen `aspnet_regsql.exe`.

Die einfachste Möglichkeit zum Ermitteln der Name der Datenbank werden über SQL Server Management Studio untersucht werden. SQL Server Management Studio bietet eine grafische Benutzeroberfläche zum Verwalten von SQL Server 2005-Datenbanken, aber es wird nicht mit der Express Edition von SQL Server 2005 geliefert. Die gute Nachricht ist, die [download](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) die kostenlose Express-Edition von SQL Server Management Studio.

> [!NOTE]
> Wenn Sie sich auch um eine nicht - Express Edition-Version von SQL Server 2005 auf dem Desktop installiert haben, ist die Vollversion von Management Studio wahrscheinlich installiert. Die vollständige Version können um der Name der Datenbank zu bestimmen, die gleichen Schritte wie unten kurz umrissen, für die Express Edition.

Beginnen Sie, indem das Schließen von Visual Studio, um sicherzustellen, dass alle Sperren, die von Visual Studio auf die Datenbankdatei auferlegt werden geschlossen. Als Nächstes starten Sie SQL Server Management Studio und Herstellen einer Verbindung mit der `localhost\InstanceName` -Datenbank für SQL Server 2005 Express Edition. Wie bereits erwähnt, wahrscheinlich werden der Instanzname ist `SQLExpress`. Wählen Sie für die Authentifizierungsoption Windows-Authentifizierung.


[![Verbinden Sie mit SQL Server 2005 Express Edition-Instanz](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Abbildung 3**: Verbinden mit der SQL Server 2005 Express Edition-Instanz ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Nach dem Herstellen einer Verbindung mit der SQL Server 2005 Express Edition-Instanz, zeigt Management Studio den Ordner für die Datenbanken, die Sicherheitseinstellungen, die Serverobjekte und So weiter. Wenn Sie die Registerkarte "Datenbanken" erweitern, sehen Sie, die die `SecurityTutorials.mdf` Datenbank *nicht* wir zuerst anfügen die Datenbank müssen in der Datenbankinstanz – erfasst.

Mit der rechten Maustaste auf den Ordner "Datenbanken" aus, und wählen Sie im Kontextmenü anfügen. Dadurch wird das Dialogfeld Anfügen von Datenbanken angezeigt. Von hier aus, klicken Sie auf die Schaltfläche "hinzufügen", navigieren Sie zu der `SecurityTutorials.mdf` Datenbank, und klicken Sie auf OK. Abbildung 4 zeigt das Dialogfeld Datenbanken anfügen, nachdem die `SecurityTutorials.mdf` Datenbank ausgewählt wurde. Abbildung 5 zeigt die Objekt-Explorer von Management Studio, nachdem die Datenbank wurde erfolgreich angefügt wurde.


[![Fügen Sie die Datenbank SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Abbildung 4**: Fügen Sie der `SecurityTutorials.mdf` Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![Die SecurityTutorials.mdf-Datenbank wird im Ordner "Datenbanken" angezeigt.](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Abbildung 5**: die `SecurityTutorials.mdf` Datenbank wird in den Ordner "Datenbanken" angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Wie in Abbildung 5 gezeigt, die `SecurityTutorials.mdf` Datenbank verfügt über einen eher abstruse Namen. Ändern Sie es in einen einprägsameren (und lässt sich einfacher eingeben) Namen. Mit der rechten Maustaste auf die Datenbank, wählen Sie im Kontextmenü der Umbenennung, und benennen Sie sie `SecurityTutorialsDatabase`. Dadurch wird der Dateiname nicht geändert, nur der Namen der Datenbank verwendet, um sich mit SQL Server zu identifizieren.


[![Die Datenbank in SecurityTutorialsDatabase umbenennen](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Abbildung 6**: Benennen Sie die Datenbank in `SecurityTutorialsDatabase`([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


An diesem Punkt wissen wir die Server- und Namen für die `SecurityTutorials.mdf` Datenbankdatei: `localhost\InstanceName` und `SecurityTutorialsDatabase`bzw. Wir sind jetzt bereit zum Installieren der Anwendungsdienste über das `aspnet_regsql.exe` Tool.

### <a name="installing-the-application-services"></a>Installation der Anwendungsdienste

Zum Starten der `aspnet_regsql.exe` tool, wechseln Sie im Startmenü, und wählen Sie ausführen. Geben Sie `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` in das Textfeld ein, und klicken Sie auf OK. Alternativ können Sie Windows-Explorer verwenden, um einen Drilldown zum entsprechenden Ordner, und doppelklicken Sie auf die `aspnet_regsql.exe` Datei. Beide Ansätze werden die gleichen Ergebnisse net.

Ausführen der `aspnet_regsql.exe` Tool ohne Befehlszeilenargumente wird die grafische Benutzeroberfläche für die ASP.NET SQL Server-Setup-Assistent gestartet. Der Assistent erleichtert das Hinzufügen oder entfernen die ASP.NET-Anwendungsdienste für eine angegebene Datenbank. Im erste Bildschirm des Assistenten dargestellt in Abbildung 7: Beschreibt den Zweck des Tools.


[![Verwenden Sie die ASP.NET SQL Server-Setup-Assistent erleichtert des Mitgliedschaftsschemas hinzufügen](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Abbildung 7**: Verwenden Sie die ASP.NET SQL Server-Setup-Assistent macht des Mitgliedschaftsschemas hinzufügen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


Der zweite Schritt im Assistenten werden wir gefragt, gibt an, ob wir die Anwendungsdienste hinzufügen oder entfernen möchten. Da wir die Tabellen, Sichten und gespeicherte Prozeduren, die für möchten die `SqlMembershipProvider`, konfigurieren Sie SQL Server-Dienste-Option "Anwendung" auswählen. Später, wenn Sie dieses Schema aus der Datenbank entfernen möchten, führen Sie diesen Assistenten erneut aus, aber stattdessen wählen Sie Anwendungsinformationen Services entfernen aus einer vorhandenen Datenbankoption aus.


[![Wählen Sie die Konfiguration von SQLServer für die Anwendung der Option "Services"](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Abbildung 8**: Wählen Sie das Konfigurieren von SQL Server für die Anwendung der Option "Services" ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


Der dritte Schritt aufgefordert, die Datenbankinformationen: den Namen des Servers, Authentifizierungsinformationen und der Name der Datenbank. Wenn Sie für dieses Tutorial folgt wurde haben und hinzugefügt haben, die `SecurityTutorials.mdf` -Datenbank einen `App_Data`, angefügt, damit `localhost\InstanceName`, und umbenannt `SecurityTutorialsDatabase`, verwenden Sie dann die folgenden Werte:

- Server: `localhost\InstanceName`
- Windows-Authentifizierung
- Datenbank: `SecurityTutorialsDatabase`


[![Geben Sie die Datenbankinformationen](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Abbildung 9**: Geben Sie die Datenbankinformationen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Nach dem Eingeben der Informationen zur Datenbank, klicken Sie auf "Weiter". Der letzte Schritt ist zusammengefasst, die ausgeführt werden soll. Klicken Sie auf "Weiter", um die Anwendungsdienste zu installieren und dann auf Fertig stellen, um den Assistenten abzuschließen.

> [!NOTE]
> Wenn Sie Management Studio verwendet haben, fügen Sie die Datenbank, und benennen Sie die Datenbankdatei, achten Sie darauf, dass Sie die Datenbank trennen, und schließen Sie Management Studio vor Visual Studio öffnen. Trennen der `SecurityTutorialsDatabase` Datenbank, mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie im Aufgabenmenü trennen.

Nach Abschluss des Assistenten zu Visual Studio zurück, und navigieren Sie zu der Datenbank-Explorer. Erweitern Sie den Ordner "Tabellen". Daraufhin sollte eine Reihe von Tabellen, deren Namen mit dem Präfix beginnen `aspnet_`. Ebenso kann eine Vielzahl von Ansichten und gespeicherte Prozeduren in den Ansichten und gespeicherte Prozeduren Ordnern gefunden werden. Diese Datenbankobjekte bilden zusammen das Dienstschema für die Anwendung. Untersuchen wir die Mitgliedschaft und Rollen-spezifischer Datenbankobjekte in Schritt 3.


[![Eine Vielzahl von Tabellen, Sichten und gespeicherten Prozeduren der Datenbank hinzugefügt wurden](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Abbildung 10**: eine Reihe von Tabellen, Sichten und gespeicherten Prozeduren wurden hinzugefügt, die Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> Die `aspnet_regsql.exe` grafische Benutzeroberfläche des Tools wird das Dienstschema für die gesamte Anwendung installiert. Aber beim Ausführen `aspnet_regsql.exe` über die Befehlszeile können Sie angeben, welche bestimmten Anwendung services-Komponenten zu installieren (oder entfernen). Aus diesem Grund sollten Sie nur die Tabellen, Ansichten, und gespeicherte Prozeduren, die für die `SqlMembershipProvider` und `SqlRoleProvider` Anbieter führen `aspnet_regsql.exe` über die Befehlszeile. Alternativ Sie können manuell ausführen, die entsprechende Teilmenge von T-SQL-Skripts, die von verwendet erstellen `aspnet_regsql.exe`. Diese Skripts befinden sich in der `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` Ordner mit Namen wie z. B. `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`und so weiter.

Wir haben an diesem Punkt die erforderlichen Datenbankobjekte erstellt die `SqlMembershipProvider`. Aber wir benötigen, um dem mitgliedschaftsframework anzuweisen, verwendet werden soll die `SqlMembershipProvider` (im Vergleich zu, z. B., wird die `ActiveDirectoryMembershipProvider`) und die `SqlMembershipProvider` sollten die `SecurityTutorials` Datenbank. Wie an welchen Anbieter verwenden und Anpassen von Einstellungen für den ausgewählten Anbieter in Schritt 4 näher. Zuerst wird aber werfen wir einen tieferen Einblick in die Datenbankobjekte, die gerade erstellt wurden.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Schritt 3: Einen Blick auf die Haupttabellen des Schemas

Bei der Arbeit mit den Frameworks Mitgliedschaften und Rollen in einer ASP.NET-Anwendung werden die Details der Implementierung durch den Anbieter gekapselt. In Zukunft wir eine mit dieser Frameworks über die .NET Framework Verbindung werden Tutorials `Membership` und `Roles` Klassen. Wenn Sie diese APIs auf höherer Ebene verwenden wir müssen nicht befassen uns mit den Low-Level-Details, wie z. B. welche Abfragen ausgeführt werden oder welche Tabellen geändert werden, indem die `SqlMembershipProvider` und `SqlRoleProvider`.

Angesichts konnte wir zuverlässig die Mitgliedschaft und Rollen Frameworks verwenden, ohne dass untersucht das Datenbankschema, die in Schritt2 erstellt haben. Beim Erstellen der Tabellen zum Speichern von Anwendungsdaten müssen wir jedoch Entitäten zu erstellen, die Benutzern oder Rollen beziehen. Werden kann, Vertrautheit mit der `SqlMembershipProvider` und `SqlRoleProvider` Schemas beim Herstellen der foreign key Einschränkungen zwischen der Anwendung von Datentabellen und diese Tabellen, die in Schritt2 erstellt haben. Darüber hinaus in bestimmten seltenen Fällen müssen wir möglicherweise die Benutzeroberfläche und die Webrolle speichert, direkt auf Datenbankebene (statt über die `Membership` oder `Roles` Klassen).

### <a name="partitioning-the-user-store-into-applications"></a>Partitionieren die Benutzer-Store in Anwendungen

Die Mitgliedschaft und Rollen Frameworks sind so entworfen, dass viele verschiedene Anwendungen ein einzelnen Benutzer und die Rolle der Speicher freigegeben werden kann. Eine ASP.NET-Anwendung, die die Mitgliedschaft oder Rollen-Frameworks verwendet, muss zu verwenden, welche Anwendungspartition angeben. Kurz gesagt, können sich mehrere Webanwendungen auf den gleichen Speicher für Benutzer und die Rolle aus. Abbildung 11 zeigt Benutzer und die Rolle speichert, die in drei Anwendungen partitioniert sind: HRSite CustomerSite und SalesSite. Diese drei Webanwendungen, jeweils ihre eigenen eindeutigen Benutzer und Rollen haben, aber physisch speichern sie alle ihre Benutzer-Konto und die entsprechende Informationen in den gleichen Datenbanktabellen.


[![Benutzerkonten können über mehrere Anwendungen hinweg partitioniert werden](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Abbildung 11**: Benutzerkonten möglicherweise werden partitioniert, über mehrere Anwendungen ([klicken Sie, um das Bild in voller Größe anzeigen](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


Die `aspnet_Applications` Tabelle ist, was diese Partitionen definiert. Jede Anwendung, die die Datenbank zum Speichern von Informationen für das Benutzerkonto verwendet, wird durch eine Zeile in dieser Tabelle dargestellt. Die `aspnet_Applications` Tabelle weist vier Spalten: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, und `Description`.`ApplicationId` ist vom Typ [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) und ist der Primärschlüssel der Tabelle `ApplicationName` stellt einen eindeutigen Benutzerfreundlicher Namen für jede Anwendung bereit.

Die anderen Mitgliedschaft und Rollen-bezogenen Tabellen zu verknüpfen, an die `ApplicationId` Feld `aspnet_Applications`. Z. B. die `aspnet_Users` -Tabelle, die einen Datensatz für jedes erstellte Benutzerkonto enthält, verfügt über eine `ApplicationId` Fremdschlüsselfeld; dasselbe gilt für die `aspnet_Roles` Tabelle. Die `ApplicationId` Feld in diesen Tabellen gibt das Benutzerkonto, das an der Anwendungspartition oder Rolle gehört.

### <a name="storing-user-account-information"></a>Das Speichern von Benutzerkontoinformationen

Informationen für das Benutzerkonto wird in zwei Tabellen mit einem: `aspnet_Users` und `aspnet_Membership`. Die `aspnet_Users` Tabelle enthält Felder, die die grundlegende Informationen zum Benutzerkonto enthalten. Die drei wichtigsten Spalten sind:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` ist der primäre Schlüssel (und vom Typ `uniqueidentifier`). `UserName` ist vom Typ `nvarchar(256)` und, zusammen mit dem Kennwort die Anmeldeinformationen des Benutzers besteht. (Das Kennwort eines Benutzers befindet sich in der `aspnet_Membership` Tabelle.) `ApplicationId` verknüpft das Benutzerkonto, das auf eine bestimmte Anwendung in `aspnet_Applications`. Es ist eine Zusammensetzung [ `UNIQUE` Einschränkung](https://msdn.microsoft.com/library/ms191166.aspx) auf die `UserName` und `ApplicationId` Spalten. Dadurch wird sichergestellt, dass in einer bestimmten Anwendung alle Benutzernamen eindeutig ist, aber es, für den gleichen ermöglicht `UserName` in verschiedenen Anwendungen verwendet werden.

Die `aspnet_Membership` Tabelle enthält zusätzliche Benutzerkontoinformationen, z. B. das Kennwort des Benutzers, e-Mail-Adresse, die letzte Anmeldedatum und Zeit und So weiter. Besteht eine 1: 1-Entsprechung zwischen Datensätzen in der `aspnet_Users` und `aspnet_Membership` Tabellen. Diese Beziehung wird durch die sichergestellt, dass die `UserId` Feld `aspnet_Membership`, der als Primärschlüssel der Tabelle dient. Wie die `aspnet_Users` Tabelle `aspnet_Membership` enthält ein `ApplicationId` Feld, das diese Informationen, um eine bestimmte Anwendungspartition gebunden.

### <a name="securing-passwords"></a>Schützen von Kennwörtern

Informationen zum Kennwort befindet sich in der `aspnet_Membership` Tabelle. Die `SqlMembershipProvider` für Kennwörter in der Datenbank, die mit einer der folgenden drei Methoden gespeichert werden können:

- **Klare** -das Kennwort wird in der Datenbank als nur-Text gespeichert. Ich Raten dringend ab, mit dieser Option. Wenn die Datenbank beschädigt ist es von einem Hacker werden, findet eine Hintertür oder ein verärgerter Mitarbeiter, wer Datenbankzugriff hat - - sind Anmeldeinformationen für jeden einzelnen Benutzer es nehmen.
- **Gehasht** --Kennwörter werden mithilfe eines unidirektionalen Hashalgorithmus und eines zufällig generierten Saltwerts. Dieser Hashwert (zusammen mit den Salt-Wert) wird in der Datenbank gespeichert.
- **Verschlüsselt** -eine verschlüsselte Version des Kennworts in der Datenbank gespeichert ist.

Das Kennwort-Speicher-Verfahren verwendet, hängt die `SqlMembershipProvider` in angegebenen Einstellungen `Web.config`. Wir sehen uns das Anpassen der `SqlMembershipProvider` Einstellungen werden in Schritt 4. Das Standardverhalten ist zum Speichern der Hash des Kennworts.

Die Spalten, die verantwortlich für das Speichern des Kennworts sind `Password`, `PasswordFormat`, und `PasswordSalt`. `PasswordFormat` ist ein Feld vom Typ `int` , deren Wert angibt, das Verfahren zum Speichern des Kennworts: 0 für deaktivieren; 1 für Hashed; 2 für verschlüsselt. `PasswordSalt` wird eine zufällig generierte Zeichenfolge unabhängig vom verwendeten Verfahren Storage Kennwort zugewiesen. der Wert des `PasswordSalt` wird nur verwendet, bei der Berechnung der Hash des Kennworts. Zum Schluss die `Password` Spalte enthält die tatsächlichen Daten, z. B. auf das nur-Text-Kennwort angegeben haben, wird der Hash des Kennworts oder das verschlüsselte Kennwort.

Tabelle 1 wird veranschaulicht, wie diese drei Spalten für die verschiedenen Speicher-Techniken aussehen könnte beim Speichern des Kennworts MeinGeheimnis! sein.

| **Speicher-Technik&lt;\_o3a\_p /&gt;** | **Kennwort&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | MeinGeheimnis! | 0 | tTnkPlesqissc2y2SMEygA== |
| Hashwert | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Verschlüsselt | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabelle 1**: Beispielwerte für die Kennwort-bezogene Felder beim Speichern der Kennwort-MeinGeheimnis!

> [!NOTE]
> Die bestimmten Verschlüsselung oder von verwendeten Hashalgorithmus die `SqlMembershipProvider` richtet sich nach den Einstellungen in der `<machineKey>` Element. Dieses Konfigurationselement in Schritt 3 des erörtert die <a id="Tutorial3"> </a> [ *Konfiguration der Formularauthentifizierung und Weiterführende Themen* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) Tutorial.

### <a name="storing-roles-and-role-associations"></a>Das Speichern von Rollen und die Rollenzuordnungen

Die Rollen-Framework kann Entwickler eine Reihe von Rollen zu definieren und angeben, welche Benutzer welche Rollen angehören. Diese Informationen werden erfasst, in der Datenbank über zwei Tabellen: `aspnet_Roles` und `aspnet_UsersInRoles`. Jeder Datensatz in die `aspnet_Roles` Tabelle stellt eine Rolle für eine bestimmte Anwendung dar. Ähnlich wie die `aspnet_Users` Tabelle, die `aspnet_Roles` Tabelle enthält drei Spalten, die für unsere Diskussion relevant sind:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` ist der primäre Schlüssel (und vom Typ `uniqueidentifier`). `RoleName` ist vom Typ `nvarchar(256)`. Und `ApplicationId` verknüpft das Benutzerkonto, das auf eine bestimmte Anwendung in `aspnet_Applications`. Es ist eine Zusammensetzung `UNIQUE` -Einschränkung für die `RoleName` und `ApplicationId` Spalten, die sicherstellen, dass in einer bestimmten Anwendung jeder Rollenname eindeutig ist.

Die `aspnet_UsersInRoles` Tabelle dient als eine Zuordnung zwischen Benutzern und Rollen. Es gibt nur zwei Spalten – `UserId` und `RoleId` – und sie bilden zusammen einen zusammengesetzten Primärschlüssel.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Schritt 4: Angeben des Anbieters und Anpassung der Einstellungen

Alle von den Frameworks, die Unterstützung für das Providermodell – z. B. die Mitgliedschaft und Rollen Frameworks - Implementierungsdetails selbst fehlt und stattdessen diese Aufgabe delegieren, auf eine Anbieterklasse. Im Fall von das mitgliedschaftsframework der `Membership` Klasse definiert die API zum Verwalten von Benutzerkonten, aber es interagiert nicht direkt mit jedem Benutzerspeicher. Vielmehr die `Membership` die Anforderung an den konfigurierten Anbieter - Klasse Methoden übergeben wir verwenden die `SqlMembershipProvider`. Wenn wir Aufrufen einer der Methoden in der `Membership` -Klasse, wie das mitgliedschaftsframework weiß, delegieren den Aufruf der `SqlMembershipProvider`?

Die `Membership` -Klasse verfügt über eine [ `Providers` Eigenschaft](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) , die einen Verweis auf alle registrierten Anbieterklassen verfügbar, für die Verwendung durch das mitgliedschaftsframework enthält. Jeder registrierter Anbieter verfügt über einen zugeordneten Namen und Typ. Der Name bietet eine Möglichkeit Benutzerfreundlicher auf einen bestimmten Anbieter in der `Providers` Sammlung, während Sie den Typ die Provider-Klasse angibt. Darüber hinaus kann jedes registrierter Anbieter-Konfigurationseinstellungen enthalten. Konfigurationseinstellungen für das mitgliedschaftsframework `PasswordFormat` und `requiresUniqueEmail`, unter anderem. Finden Sie in Tabelle 2 für eine vollständige Liste der von verwendeten Konfigurationseinstellungen die `SqlMembershipProvider`.

Die `Providers` Inhalt Eigenschaft angegeben sind, über die Konfigurationseinstellungen für die Webanwendung. Standardmäßig alle Webanwendungen verfügen über einen Anbieter, die mit dem Namen `AspNetSqlMembershipProvider` des Typs `SqlMembershipProvider`. In diesem Standard-Mitgliedschaftsanbieter registriert ist `machine.config` (am `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Als Markup oben dargestellt die [ `<membership>` Element](https://msdn.microsoft.com/library/1b9hw62f.aspx) definiert die Konfigurationseinstellungen für das mitgliedschaftsframework während der [ `<providers>` untergeordnetes Element](https://msdn.microsoft.com/library/6d4936ht.aspx) gibt an, den registrierten Anbieter. Anbieter können hinzugefügt oder entfernt werden mithilfe der [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) oder [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) Elemente; verwenden Sie stattdessen die [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) Elements, das alle derzeit entfernt registrierten Anbieter. Als Markup oben dargestellt `machine.config` Fügt einen Anbieter mit dem Namen `AspNetSqlMembershipProvider` des Typs `SqlMembershipProvider`.

Zusätzlich zu den `name` und `type` Attribute, die `<add>` Element enthält Attribute, die die Werte für verschiedene Konfigurationseinstellungen zu definieren. Tabelle 2 werden die verfügbaren `SqlMembershipProvider`-bestimmte Konfigurationseinstellungen festgelegt werden, zusammen mit einer Beschreibung der einzelnen.

> [!NOTE]
> In Tabelle 2 notiert haben Standardwerte finden Sie auf die Standardwerte definiert, der `SqlMembershipProvider` Klasse. Beachten Sie, dass nicht alle Konfigurationseinstellungen in `AspNetSqlMembershipProvider` entsprechen die Standardwerte für die `SqlMembershipProvider` Klasse. Angenommen, nicht in einen Mitgliedschaftsanbieter angegeben die `requiresUniqueEmail` standardmäßig auf "true" festlegen. Allerdings die `AspNetSqlMembershipProvider` überschreibt diesen Wert durch explizite Angabe der Wert `false`.

| **Festlegen von&lt;\_o3a\_p /&gt;** | **Beschreibung&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Denken Sie daran, dass das mitgliedschaftsframework für einen einzelnen Benutzerspeicher über mehrere Anwendungen hinweg partitioniert werden kann. Diese Einstellung gibt an, den Namen der Anwendungspartition vom Mitgliedschaftsanbieter verwendet wird. Wenn dieser Wert nicht explizit angegeben ist, festgelegt ist, zur Laufzeit, um den Wert der virtuelle Stammpfad der Anwendung. |
| `commandTimeout` | Gibt den Timeoutwert für SQL-Befehl (in Sekunden) an. Der Standardwert ist 30. |
| `connectionStringName` | Der Name der Verbindungszeichenfolge in der `<connectionStrings>` Element, das für die Verbindung mit der Geschäftsdatenbank Benutzer verwendet. Dieser Wert ist erforderlich. |
| `description` | Enthält eine Beschreibung Benutzerfreundlicher, von der registrierten Anbieter. |
| `enablePasswordRetrieval` | Gibt an, ob Benutzer, deren Kennwort vergessen haben abrufen können. Der Standardwert ist `false`. |
| `enablePasswordReset` | Gibt an, ob Benutzer zum Zurücksetzen des Kennworts zulässig sind. Wird standardmäßig auf `true` festgelegt. |
| `maxInvalidPasswordAttempts` | Die maximale Anzahl nicht erfolgreicher Anmeldeversuche, die für einen bestimmten Benutzer, während des angegebenen auftreten können `passwordAttemptWindow` bevor der Benutzer gesperrt wird. Der Standardwert ist 5. |
| `minRequiredNonalphanumericCharacters` | Die minimale Anzahl von nicht-alphanumerische Zeichen, die in das Kennwort eines Benutzers angezeigt werden müssen. Dieser Wert muss zwischen 0 und 128 liegen; Der Standardwert ist 1. |
| `minRequiredPasswordLength` | Die minimale Anzahl von Zeichen im Kennwort erforderlich. Dieser Wert muss zwischen 0 und 128 liegen; Der Standardwert ist 7. |
| `name` | Der Name des der registrierten Anbieter. Dieser Wert ist erforderlich. |
| `passwordAttemptWindow` | Die Anzahl der Minuten, während der Fehler bei der Anmeldung, die versuchen, nachverfolgt werden. Wenn ein Benutzer ungültige Anmeldeinformationen bereitstellt `maxInvalidPasswordAttempts` Zeiträume in diesem Fenster, die sie gesperrt werden. Der Standardwert ist 10. |
| `PasswordFormat` | Das Speicherformat Kennwort: `Clear`, `Hashed`, oder `Encrypted`. Die Standardeinstellung ist `Hashed`. |
| `passwordStrengthRegularExpression` | Wenn angegeben, wird dieser reguläre Ausdruck verwendet, die Stärke des ausgewählten Kennwort des Benutzers, beim Erstellen eines neuen Kontos oder ausgewertet, wenn Sie ihr Kennwort zu ändern. Der Standardwert ist eine leere Zeichenfolge. |
| `requiresQuestionAndAnswer` | Gibt an, ob ein Benutzer auf seine Sicherheitsfrage, die beim Abrufen oder Zurücksetzen seines Kennworts beantworten muss. Der Standardwert ist `true`. |
| `requiresUniqueEmail` | Gibt an, ob alle Benutzerkonten in einer bestimmten Anwendungspartition eine eindeutige e-Mail-Adresse verfügen müssen. Der Standardwert ist `true`. |
| `type` | Gibt den Typ des Anbieters. Dieser Wert ist erforderlich. |

**Tabelle 2**: Mitgliedschaft und `SqlMembershipProvider` -Konfigurationseinstellungen

Zusätzlich zu `AspNetSqlMembershipProvider`, andere Mitgliedschaftsanbieter können auf Basis von Anwendung registriert werden, durch das Hinzufügen von-Markup ähnlich dem `Web.config` Datei.

> [!NOTE]
> Im Wesentlichen die gleiche Weise funktioniert das Framework-Rollen: Es gibt einen registrierten Standardrollenanbieter in `machine.config` und registrierten Anbieter können angepasst werden, auf der Grundlage von Anwendung in `Web.config`. Wir werden in einem späteren Tutorial das Rollen-Framework und dessen konfigurationsmarkup im Detail untersuchen.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Anpassen der`SqlMembershipProvider`Einstellungen

Der Standardwert `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) verfügt über seine `connectionStringName` -Attributsatz auf `LocalSqlServer`. Wie die `AspNetSqlMembershipProvider` -Anbieter, der Name der Verbindungszeichenfolge `LocalSqlServer` ist definiert `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Wie Sie sehen können, definiert diese Verbindungszeichenfolge steht eine SQL 2005 Express Edition-Datenbank sich befindet | DataDirectory|aspnetdb. mdf befindet. Die Zeichenfolge | DataDirectory | wird zur Laufzeit, um zu zeigen übersetzt die `~/App_Data/` Verzeichnis, also den Datenbankpfad | DataDirectory|aspnetdb. mdf befindet übersetzt `~/App_Data` / `aspnet.mdf`.

Wenn wir keine Anbieterinformationen zur Mitgliedschaft in der Anwendung angegeben haben `Web.config` -Datei, die Anwendung verwendet die registrierten Standardmitgliedschaftsanbieter, `AspNetSqlMembershipProvider`. Wenn die `~/App_Data/aspnet.mdf` Datenbank ist nicht vorhanden, die ASP.NET-Laufzeit automatisch erstellt, und fügen Sie das Schema der Application-Dienste. Aber wir möchten nicht verwenden die `aspnet.mdf` Datenbank; stattdessen verwendet werden soll die `SecurityTutorials.mdf` Datenbank, die wir in Schritt2 erstellt haben. Diese Änderung kann auf zwei Arten erfolgen:

- <strong>Geben Sie einen Wert für die</strong><strong>`LocalSqlServer`</strong><strong>Namen der Verbindungszeichenfolge der</strong><strong>`Web.config`</strong><strong>.</strong> Durch Überschreiben der `LocalSqlServer` Name für Verbindungszeichenfolgenwert in `Web.config`, wir können den registriert Standardmitgliedschaftsanbieter verwenden (`AspNetSqlMembershipProvider`), damit er ordnungsgemäß funktioniert mit der `SecurityTutorials.mdf` Datenbank. Dieser Ansatz ist in Ordnung, wenn Sie mit den Konfigurationseinstellungen, die anhand des Inhalts sind `AspNetSqlMembershipProvider`. Weitere Informationen zu dieser Technik finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)Blogbeitrag [Konfigurieren von ASP.NET 2.0-Anwendungsdienste für Verwenden von SQL Server 2000 oder SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Fügen Sie einen neuen registrierten Anbieter vom Typ</strong><strong>`SqlMembershipProvider`</strong><strong>und konfigurieren Sie die</strong><strong>`connectionStringName`</strong><strong>Einstellung aus, um auf dieverweisen</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>Datenbank.</strong> Dieser Ansatz ist hilfreich in Szenarien, in dem Sie anderen Konfigurationseigenschaften, zusätzlich zu der Datenbank-Verbindungszeichenfolge anpassen möchten. In meinen eigenen Projekten verwenden ich diesen Ansatz immer aufgrund seiner Flexibilität und besseren Lesbarkeit.

Bevor wir einen neuen registrierten Anbieter hinzufügen können, die verweist die `SecurityTutorials.mdf` -Datenbank, müssen wir zuerst eine geeignete Verbindungszeichenfolgenwert in Hinzufügen der `<connectionStrings>` im Abschnitt `Web.config`. Das folgende Markup Fügt eine neue Verbindungszeichenfolge, die mit dem Namen `SecurityTutorialsConnectionString` , die auf der SQL Server 2005 Express Edition `SecurityTutorials.mdf` -Datenbank in die `App_Data` Ordner.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Wenn Sie eine andere Datenbank-Datei verwenden, aktualisieren Sie die Verbindungszeichenfolge. Weitere Informationen dazu, die die richtige Verbindungszeichenfolge bilden, finden Sie unter [ConnectionStrings.com](http://www.connectionstrings.com/).

Fügen Sie das folgende konfigurationsmarkup von Mitgliedschaft, um die `Web.config` Datei. Dieses Markup registriert einen neuen Anbieter mit dem Namen `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Neben der Registrierung der `SecurityTutorialsSqlMembershipProvider` -Anbieter, das obenstehende Markup definiert die `SecurityTutorialsSqlMembershipProvider` als Standardanbieter (über die `defaultProvider` -Attribut in der `<membership>` Element). Denken Sie daran, dass das mitgliedschaftsframework mehrere registrierte Anbieter haben kann. Da `AspNetSqlMembershipProvider` registriert, ist als der erste Anbieter in `machine.config`, er dient als Standardanbieter, es sei denn, es anders angegeben.

Derzeit ist unsere Anwendung zwei registrierten Anbieter: `AspNetSqlMembershipProvider` und `SecurityTutorialsSqlMembershipProvider`. Jedoch vor der Registrierung der `SecurityTutorialsSqlMembershipProvider` Anbieter, die wir haben Sie alle zuvor deaktiviert könnten registrierten Anbieter durch das Hinzufügen einer [ `<clear />` Element](https://msdn.microsoft.com/library/t062y6yc.aspx) unmittelbar vor unserer `<add>` Element. Diese möchten, deaktivieren Sie die `AspNetSqlMembershipProvider` aus der Liste der registrierten Anbieter, was bedeutet, dass die `SecurityTutorialsSqlMembershipProvider` wäre, die nur registrierte Mitgliedschaftsanbieter. Wenn wir diesen Ansatz verwendet, müssen wir nicht markieren die `SecurityTutorialsSqlMembershipProvider` als Standardanbieter, da es wäre, die nur registrierten Mitgliedschaftsanbieter. Weitere Informationen zur Verwendung von `<clear />`, finden Sie unter [Using `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Beachten Sie, dass die `SecurityTutorialsSqlMembershipProvider`des `connectionStringName` Festlegen von verweisen die gerade hinzugefügte `SecurityTutorialsConnectionString` Name der Verbindungszeichenfolge, und dass die `applicationName` Einstellung auf einen Wert von SecurityTutorials festgelegt wurde. Darüber hinaus die `requiresUniqueEmail` Einstellung festgelegt wurde `true`. Alle anderen Konfigurationsoptionen sind identisch mit den Werten in `AspNetSqlMembershipProvider`. Gerne hier keine konfigurationsänderungen vornehmen, wenn Sie möchten. Sie können z. B. die kennwortsicherheit durch zwei nicht-alphanumerischen Zeichen anstelle eines anfordern oder erhöhen die Länge des Kennworts auf acht Zeichen anstelle von sieben anziehen.

> [!NOTE]
> Denken Sie daran, dass das mitgliedschaftsframework für einen einzelnen Benutzerspeicher über mehrere Anwendungen hinweg partitioniert werden kann. Des Mitgliedschaftsanbieters `applicationName` Einstellung gibt an, welche Anwendung den Anbieter verwendet, bei der Arbeit mit den Speicher des Benutzers. Es ist wichtig, dass Sie explizit einen Wert für die `applicationName` Konfiguration festlegen, da Wenn die `applicationName` nicht explizit festgelegt ist, wird die Webanwendung der virtuelle Stammpfad zur Laufzeit zugewiesen wird. Dies funktioniert gut, solange der virtuelle Stammpfad der Anwendung nicht ändert, aber wenn Sie die Anwendung in einen anderen Pfad ein, Verschieben der `applicationName` Einstellung zu ändern ist. In diesem Fall wird der Mitgliedschaftsanbieter arbeiten mit einer anderen Anwendungspartition als zuvor verwendet wurde. Benutzerkonten, die vor der Verschiebung erstellt in einer anderen Anwendungspartition befinden, und diese Benutzer werden nicht mehr in der Lage, sich bei der Site anmelden. Eine ausführlichere Erläuterung dieser Angelegenheit, finden Sie unter [legen Sie immer die `applicationName` Eigenschaft beim Konfigurieren von ASP.NET 2.0 Mitgliedschaft und anderen Anbietern](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Zusammenfassung

An diesem Punkt haben wir eine Datenbank mit der konfigurierten Anwendungsdienste (`SecurityTutorials.mdf`) und unserer Webanwendung konfiguriert haben, sodass das mitgliedschaftsframework verwendet die `SecurityTutorialsSqlMembershipProvider` Anbieter, die wir gerade registriert. Dieser registrierter Anbieter ist vom Typ `SqlMembershipProvider` und verfügt über seine `connectionStringName` legen Sie auf die entsprechende Verbindungszeichenfolge (`SecurityTutorialsConnectionString`) und die zugehörige `applicationName` Wert explizit festgelegt.

Wir können nun das mitgliedschaftsframework aus unserer Anwendung verwenden. Im nächsten Tutorial untersuchen wir die neue Benutzerkonten erstellen. Danach wird näher eingegangen wird, Authentifizieren von Benutzern, Benutzerbasierte Autorisierung durchführen und zusätzliche benutzerbezogenen Informationen in der Datenbank gespeichert.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Legen Sie immer die `applicationName` Eigenschaft beim Konfigurieren von ASP.NET 2.0 Mitgliedschaft und anderen Anbietern](https://weblogs.asp.net/scottgu/443634)
- [Konfigurieren von ASP.NET 2.0 Services-Anwendung zum Verwenden von SQLServer 2000 oder SQLServer 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Herunterladen von SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Untersuchen von ASP.NET 2.0 s Mitgliedschaft, Rollen und Profile](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Die `<add>` -Element für Providers für Membership](https://msdn.microsoft.com/library/whae3t94.aspx)
- [Die `<membership>` Element](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [Die `<providers>` -Element für Mitgliedschaft](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Mithilfe von `<clear />` beim Hinzufügen von Anbietern](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Direktes Arbeiten mit der `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>In den Themen in diesem Tutorial-Videotraining

- [Grundlegendes zu ASP.NET-Mitgliedschaften](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Konfigurieren von SQL für die Arbeit mit Mitgliedschaftsschemas](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Ändern der Mitgliedschaftseinstellungen im Mitgliedschaftsschema](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Führendes Prüfer für dieses Tutorial wurde Alicja Maziarz. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](storing-additional-user-information-cs.md)
> [Weiter](creating-user-accounts-vb.md)
