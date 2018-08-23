---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migrieren einer vorhandenen Website aus SQL-Mitgliedschaftsanbieter nach ASP.NET Identity | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial werden die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer- und Rolle mithilfe von SQL-Mitgliedschaft in der neuen ASP.NET Identity s erstellt...
ms.author: riande
ms.date: 12/19/2014
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 51b97ee413ea0304177d5963b5fd9d7253778d4f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827823"
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrieren einer vorhandenen Website aus SQL-Mitgliedschaftsanbieter nach ASP.NET Identity
====================
durch [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Dieses Tutorial veranschaulicht die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer- und Rolle mithilfe von SQL-Mitgliedschaftsanbieter auf das neue ASP.NET Identity-System erstellt. Dieser Ansatz umfasst eine Änderung der vorhandenen Datenbankschema auf den von der ASP.NET Identity und Hook in die alte/neue Klassen, die es benötigt. Nachdem Sie diesen Ansatz, nach der Migration Ihrer Datenbank, werden zukünftige Updates für Identität mühelos verarbeitet.


Für dieses Tutorial dauert es eine Webanwendungsvorlage (Webformulare) aus, die mithilfe von Visual Studio 2010 zum Erstellen von Benutzer-und Rolle erstellt. SQL-Skripts wird anschließend verwendet, die vorhandene Datenbank zu Tabellen, die von der Identity-System benötigt werden migriert. Als Nächstes wir installieren die erforderlichen NuGet-Pakete und Hinzufügen von neuen Konto-Verwaltungsseiten auf die das Identity-System zur Verwaltung der Gruppenmitgliedschaft verwendet. Zum Testen der Migration mit SQL-Mitgliedschaftsanbieter erstellten Benutzer sollten in der Lage, melden Sie sich, und neue Benutzer sollten in der Lage, zu registrieren. Sie finden das vollständige Beispiel [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Siehe auch [Migration von ASP.NET-Mitgliedschaft zu ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Erste Schritte

### <a name="creating-an-application-with-sql-membership"></a>Erstellen einer Anwendung mit SQL-Mitgliedschaft

1. Wir müssen mit einer vorhandenen Anwendung zu starten, verwendet die SQL-Mitgliedschaft und Benutzer und Rolle aufweist. In diesem Artikel sehen wir eine Webanwendung in Visual Studio 2010 erstellen.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Erstellen Sie mithilfe des Konfigurationstools von ASP.NET 2 Benutzer: **OldAdminUser** und **OldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Erstellen Sie eine Rolle mit dem Namen Administrators, und fügen Sie "OldAdminUser" als Benutzer in dieser Rolle hinzu.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Erstellen Sie einen Abschnitt "Admin" der Website mit einer "default.aspx" ein. Legen Sie das Tag für die Autorisierung in der Datei "Web.config" So aktivieren Sie nur den Zugriff auf Benutzer in Administratorrollen. Weitere Informationen finden Sie hier [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Zeigen Sie die Datenbank im Server-Explorer, die das Mitgliedschaftssystem SQL erstellten Tabellen zu verstehen. Die Anmeldedaten des Benutzers befindet sich in der Aspnet\_-Benutzer und Aspnet\_Mitgliedschaft enthalten sind, während Daten der Rolle in der Aspnet gespeichert werden\_Tabelle "Roles". Informationen darüber, welche Benutzer welche Rollen in der Aspnet gespeichert sind\_UsersInRoles-Tabelle. Für die Verwaltung von Basis-Mitgliedschaft ist es ausreichend, um die Informationen in den Tabellen oben auf das ASP.NET Identity-System-port.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrieren zu Visual Studio 2013

1. Installieren von Visual Studio Express 2013 für Web- oder Visual Studio 2013, zusammen mit den [neuesten](https://www.microsoft.com/download/details.aspx?id=44921).
2. Öffnen Sie das obige Projekt in Ihrer installierten Version von Visual Studio. Wenn SQL Server Express auf dem Computer nicht installiert ist, wird eine Aufforderung angezeigt, wenn Sie das Projekt öffnen, da die Verbindungszeichenfolge SQL Express verwendet. Sie können wahlweise zum Installieren von SQL Express oder als umgehen, ändern die Verbindungszeichenfolge auf LocalDb. In diesem Artikel werden wir es in LocalDb ändern.
3. Öffnen Sie die Datei "Web.config", und ändern Sie die Verbindungszeichenfolge aus. SQLExpess auf V11. 0 (LocalDb). Entfernen Sie "User Instance = True" aus der Verbindungszeichenfolge.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Öffnen Sie Server-Explorer, und stellen Sie sicher, dass das Tabellenschema und die Daten angezeigt werden können.
5. Das ASP.NET Identity-System, funktioniert mit Version 4.5 oder höher des Frameworks. Umleiten der Anwendung auf Version 4.5 oder höher.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Erstellen Sie das Projekt aus, um sicherzustellen, dass keine Fehler vorliegen.

### <a name="installing-the-nuget-packages"></a>Installieren der Nuget-Pakete

1. Klicken Sie im Projektmappen-Explorer mit der Maustaste des Projekts &gt; **NuGet-Pakete verwalten**. Geben Sie in das Suchfeld "Asp.net Identity" aus. Wählen Sie das Paket in der Liste der Ergebnisse, und klicken Sie auf installieren. Akzeptieren Sie den Lizenzvertrag durch Klicken auf die Schaltfläche "Ich stimme". Beachten Sie, dass dieses Paket die abhängigkeitspakete installiert wird: EntityFramework und Microsoft ASP.NET Identity Core. Auf ähnliche Weise installieren Sie die folgenden Pakete (überspringen die letzten 4 OWIN-Pakete aus, wenn Sie nicht OAuth-Anmeldung aktivieren möchten):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrieren der Datenbank in das neue Identity-system

Der nächste Schritt ist zum Migrieren der vorhandenen Datenbank in ein Schema, das ASP.NET Identity-System erforderlich. Um zu erreichen wir eine SQL ausführen Skript die über eine Reihe von Befehlen zum Erstellen neuer Tabellen und migrieren vorhandene Benutzerinformationen zu den neuen Tabellen verfügt. Die Skriptdatei finden Sie [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Diese Skriptdatei ist spezifisch für dieses Beispiel. Wenn das Schema für die Tabellen erstellt mithilfe der SQL-Mitgliedschaftsanbieter wird angepasst oder geändert Skripts müssen entsprechend geändert werden.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Wie das SQL-Skript für die Migration des Schemas generiert

Für ASP.NET Identity-Klassen, die im Lieferumfang der Daten mit den vorhandenen Benutzern arbeiten müssen wir das Schema der Datenbank, die von ASP.NET Identity benötigt zu migrieren. Dies ist möglich durch Hinzufügen neuer Tabellen und kopieren die vorhandene Informationen in diesen Tabellen. Standardmäßig verwendet ASP.NET Identity EntityFramework die Modellklassen Identität in der Datenbank zum Speichern/Abrufen von Informationen zuordnen. Diese Modellklassen implementieren die Core Identity-Schnittstellen definieren von Benutzer- und Role-Objekte. Auf diese Modellklassen basieren die Tabellen und Spalten in der Datenbank. Die EntityFramework-Modellklassen in Identity 2.1.0 und deren Eigenschaften werden wie unten definiert.

| **IdentityUser** | **Type** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | Zeichenfolge | Id | RoleId | Providerkeyzurück | Id |
| Benutzername | Zeichenfolge | name | UserId | UserId | "ClaimType" |
| PasswordHash | Zeichenfolge |  |  | LoginProvider | ClaimValue |
| SecurityStamp | Zeichenfolge |  |  |  | Benutzer\_Id |
| E-Mail | Zeichenfolge |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Telefonnummer | Zeichenfolge |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| "Accessfailedcount" | int |  |  |  |  |

Tabellen mit Spalten, die Eigenschaften für jedes dieser Modelle aufweisen müssen. Die Zuordnung zwischen Klassen und Tabellen wird definiert, der `OnModelCreating` Methode der `IdentityDBContext`. Dies bezeichnet man als die fluent-API-Methode der Konfiguration und Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/data/jj591617.aspx). Die Konfiguration für die Klassen ist, wie unten beschrieben.

| **Klasse** | **Table** | **Primärschlüssel** | **Fremdschlüssel** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | Benutzer-ID und der RoleId | Benutzer\_-Id -&gt;"aspnetusers" RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Providerkeyzurück + Benutzer-ID + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Benutzer\_-Id -&gt;"aspnetusers" |

Mit diesen Informationen können wir die SQL-Anweisungen zum Erstellen neuer Tabellen erstellen. Wir können jeder Anweisung einzeln schreiben oder generieren Sie das gesamte Skript mithilfe von EntityFramework-PowerShell-Befehle, die wir, klicken Sie dann bearbeiten können nach Bedarf. In VS öffnen Sie dazu die **-Paket-Manager-Konsole** aus der **Ansicht** oder **Tools** Menü

- Befehl "Run" "Enable-Migrations" EntityFramework-Migrationen zu aktivieren.
- Ausführen von Befehl "Add-Migration Initial" erstellt die anfängliche Einrichtungscode zum Erstellen der Datenbank in c# / VB.
- Der letzte Schritt ist die Ausführung "Update-Database-Skript"-Befehl, der das SQL-Skript generiert auf Grundlage der ViewModel-Klassen.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Dieses Skript zur Proxygenerierung Datenbank kann als Ausgangspunkt verwendet werden, wo wir werden zusätzliche Änderungen sein um neue Spalten hinzuzufügen, und Kopieren von Daten. Der Vorteil ist, dass wir generieren die `_MigrationHistory` Tabelle, die um das Datenbankschema zu ändern, wenn das Modell ändern, die für zukünftige Versionen von Identity-Releases Klassen von EntityFramework verwendet wird. 

Die SQL-Mitgliedschaftsbenutzerinformationen mussten andere Eigenschaften zusätzlich zu den in der Identity-Benutzer-Modellklasse namely-e-Mail Kennworteingaben, Datum der letzten Anmeldung, Datum der letzten Sperrung usw. an. Hierbei handelt es sich um nützliche Informationen, und wir möchten sie in das Identitätssystem übernommen werden. Dies kann erfolgen, indem das Benutzermodell zusätzliche Eigenschaften hinzugefügt, und wieder an, die die Tabellenspalten in der Datenbank. Können wir dies durch Hinzufügen einer Klasse, Unterklassen der `IdentityUser` Modell. Wir können die Eigenschaften für diese benutzerdefinierte Klasse hinzufügen und bearbeiten Sie das SQL-Skript, um die entsprechenden Spalten hinzuzufügen, beim Erstellen der Tabelle. Der Code für diese Klasse ist in diesem Artikel ausführlich beschrieben. Das SQL-Skript zum Erstellen der `AspnetUsers` Tabelle nach dem Hinzufügen neuer Eigenschaften wäre

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Als Nächstes müssen wir die vorhandene Informationen aus der Mitgliedschaftsdatenbank von SQL in den neu hinzugefügten Tabellen für die Identität zu kopieren. Dies kann über SQL erfolgen durch Kopieren von Daten direkt aus einer Tabelle in eine andere. Um Daten in den Zeilen der Tabelle hinzuzufügen, verwenden wir die `INSERT INTO [Table]` zu erstellen. Zum Kopieren von aus einer anderen Tabelle wir verwenden die `INSERT INTO` Anweisung zusammen mit den `SELECT` Anweisung. Um die Benutzerinformationen zu erhalten, wir zum Abfragen müssen, der *Aspnet\_Benutzer* und *Aspnet\_Mitgliedschaft* Tabellen und kopieren Sie die Daten, die *"aspnetusers"* Tabelle. Wir verwenden die `INSERT INTO` und `SELECT` zusammen mit `JOIN` und `LEFT OUTER JOIN` Anweisungen. Weitere Informationen zum Abfragen und Kopieren von Daten zwischen Tabellen finden Sie unter [dies](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) Link. Darüber hinaus sind die Tabellen "aspnetuserlogins" und "aspnetuserclaims" leer zunächst, da es keine Informationen in der SQL-Mitgliedschaft sind, die dies standardmäßig zugeordnet ist. Die einzige Information, die kopiert, ist für Benutzer und Rollen. Für das Projekt, in den vorherigen Schritten erstellt haben wäre die SQL-Abfrage, um Daten in der Tabelle kopieren

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

In der obigen SQL-Anweisung, Informationen zu jedem Benutzer aus der *Aspnet\_Benutzer* und *Aspnet\_Mitgliedschaft* Tabellen wird in die Spalten kopiert die  *"Aspnetusers"* Tabelle. Die einzige Änderung, die hier ist, wenn wir das Wiederherstellungskennwort zu kopieren. Da der Verschlüsselungsalgorithmus für Kennwörter in der SQL-Mitgliedschaftsanbieter 'PasswordSalt' und "PasswordFormat" verwendet, kopieren wir, die zu zusammen mit dem verschlüsselten Kennwort, damit sie verwendet werden kann zum Entschlüsseln des Kennworts durch Identität. Dies wird erläutert, weiter unten in diesem Artikel, wenn Sie ein benutzerdefiniertes Kennwort Hasher einbinden. 

Diese Skriptdatei ist spezifisch für dieses Beispiel. Für Anwendungen, die zusätzliche Tabellen haben, können Entwickler einen ähnlichen Ansatz zum Hinzufügen von weiteren Eigenschaften auf der Model-Klasse, und ordnen sie Spalten in der Tabelle "aspnetusers" folgen. Um das Skript auszuführen,

1. Öffnen Sie Server-Explorer. Erweitern Sie die Verbindung "ApplicationServices", um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf den Knoten "Tabellen", und wählen Sie die Option "Neue Abfrage"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Klicken Sie im Abfragefenster kopieren Sie, und fügen Sie das gesamte SQL-Skript aus der Datei Migrations.sql. Führen Sie die Skriptdatei, indem Sie auf die Pfeilschaltfläche 'Ausführen'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aktualisieren Sie die Server-Explorer-Fenster. Fünf neue Tabellen werden in der Datenbank erstellt.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Unten ist wie die Informationen in den mitgliedschaftstabellen SQL zum neuen System Identität zugeordnet werden.

    ASPNET\_Rollen –&gt; AspNetRoles

    ASP\_NetUsers und Asp\_NetMembership--&gt; "aspnetusers"

    ASPNET\_UserInRoles--&gt; "aspnetuserroles"

    Wie im vorherigen Abschnitt erläutert wird, sind die Tabellen "aspnetuserclaims" und "aspnetuserlogins" leer. Das Feld "Discriminator" in der Tabelle AspNetUser sollte der Name des Modells Klasse übereinstimmen, die im nächsten Schritt definiert ist. Auch die PasswordHash Spalte ist in der Form ' verschlüsselte Kennwort | Saltwert des Kennworts | Kennwortformat ". Dadurch können Sie spezielle Logik für die Verschlüsselung von SQL Mitgliedschaft verwenden, damit Sie alte Kennwörter wiederverwenden können. Wird im später in diesem Artikel erläutert.

### <a name="creating-models-and-membership-pages"></a>Erstellen von Modellen und Mitgliedschaftsseiten

Wie bereits erwähnt, wird die Identity-Funktion Entity Framework für die Kommunikation mit der Datenbank zum Speichern von Kontoinformationen werden standardmäßig verwendet. Um mit vorhandenen Daten in der Tabelle arbeiten zu können, müssen wir ViewModel-Klassen zu erstellen, die an die Tabellen zugeordnet, und verknüpfen sie in das Identitätssystem. Als Teil des Vertrags Identität die Modellklassen sollten entweder die Schnittstellen in der Dll Identity.Core implementieren oder erweitern können, die vorhandene Implementierung dieser Schnittstellen in "Microsoft.Aspnet.Identity.EntityFramework" verfügbar.

In unserem Beispiel haben die Tabellen AspNetRoles und "aspnetuserclaims", AspNetLogins AspNetUserRole Spalten, die ähnlich wie die vorhandene Implementierung von Identity-System. Daher können wir die vorhandenen Klassen, die an diesen Tabellen zuordnen wiederverwenden. In der Tabelle AspNetUser verfügt über einige zusätzlichen Spalten, die zum Speichern zusätzlicher Informationen aus der Mitgliedschaft in SQL-Tabellen verwendet werden. Dies kann durch Erstellen einer Modellklasse, die die vorhandene Implementierung von "IdentityUser" erweitern und fügen Sie die zusätzlichen Eigenschaften zugeordnet werden.

1. Erstellt einen Modelle Ordner im Projekt und fügen Sie eine Benutzer-Klasse. Der Name der Klasse sollte die in der Spalte "Discriminator" der Tabelle "aspnetusers" "" hinzugefügten Daten übereinstimmen.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Erweitern Sie die Benutzer-Klasse sollte die IdentityUser-Klasse finden Sie in der *"Microsoft.Aspnet.Identity.EntityFramework"* Dll. Deklarieren Sie die Eigenschaften in der Klasse, die an die AspNetUser Spalten zuordnen. Die Eigenschaften-ID, Benutzername, PasswordHash und SecurityStamp in der "identityuser" definiert sind und daher ausgelassen. Unten ist der Code für die Klasse, die alle Eigenschaften verfügt.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Eine Entity Framework DbContext-Klasse ist erforderlich, um Daten in Modellen, die an Tabellen speichern und Abrufen von Daten aus Tabellen in den Modellen zu füllen. *Microsoft.AspNet.Identity.EntityFramework* dll defines the IdentityDbContext class which interacts with the Identity tables to retrieve and store information. IdentityDbContext&lt;Tuser&gt; akzeptiert eine 'TUser'-Klasse, die jede Klasse sein kann, die die Klasse "identityuser" Erweitert.

    Erstellen Sie eine neue Klasse ApplicationDBContext, das erweitert IdentityDbContext "Modelle" im Ordner "" in der "User"-Klasse, die in Schritt 1 erstellte übergeben

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Benutzerverwaltung in das neue Identitätssystem erfolgt mithilfe der UserManager&lt;Tuser&gt; Klasse definiert die *"Microsoft.Aspnet.Identity.EntityFramework"* Dll. Wir müssen eine benutzerdefinierte Klasse erstellen, die UserManager, übergeben in der "User"-Klasse, die in Schritt 1 erstellte erweitert.

    Erstellen Sie im Ordner "Models" eine neue Klasse UserManager, die UserManager erweitert&lt;Benutzer&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Die Kennwörter der Benutzer der Anwendung werden verschlüsselt und in der Datenbank gespeichert. Die Crypto-Algorithmus in SQL-Mitgliedschaftsanbieter verwendet, unterscheidet sich von dem das neue Identitätssystem. Um die alte Kennwörter wiederverwenden müssen Kennwörter selektiv zu entschlüsseln, wenn alte Benutzer mithilfe des SQL-Mitgliedschaften-Algorithmus bei der Verwendung von der Crypto-Algorithmus in Identity für neue Benutzer anmelden.

    Die UserManager-Klasse verfügt über eine Eigenschaft "PasswordHasher" auf dem eine Instanz einer Klasse gespeichert, der 'Ipasswordhasher.'-Schnittstelle implementiert. Hiermit wird zum Verschlüsseln/Kennwörter während der Authentifizierung von Benutzertransaktionen entschlüsseln. In der UserManager-Klasse, die in Schritt 3 definierten, erstellen Sie eine neue Klasse SQLPasswordHasher, und kopieren Sie den folgenden Code.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Beheben Sie Kompilierungsfehler, indem Sie den System.Text "und" System.Security.Cryptography-Namespace importieren.

    Die Methode EncodePassword verschlüsselt das Kennwort entsprechend die standardmäßige SQL-Mitgliedschaft Crypto-Implementierung. Dies ist die Dll "System.Web" entnommen werden. Wenn die alte Anwendung um eine benutzerdefinierte Implementierung verwendet sollte er hier angegeben. Sie müssen zwei weitere Methoden definieren *HashPassword* und *VerifyHashedPassword* , verwenden die *EncodePassword* Methode, um ein bestimmtes Kennwort hash oder Überprüfen von nur-Text das Kennwort, mit dem einer vorhandenen in der Datenbank.

    Das Mitgliedschaftssystem SQL verwendet PasswordHash und PasswordSalt mit PasswordFormat, einen Hashwert des Kennworts, die von Benutzern eingegeben werden, wenn sie sich registrieren oder ihr Kennwort ändern. Während der Migration befinden sich alle drei Felder in der Spalte PasswordHash, in der AspNetUser-Tabelle, die getrennt von der ' |' Zeichen. Wenn ein Benutzer anmeldet, und das Kennwort diese Felder ist, verwenden wir der SQL-Mitgliedschaft Kryptografie, um das Kennwort zu überprüfen; Andernfalls verwenden wir das Identitätssystem Standard-Verschlüsselung, um zu überprüfen, ob das Kennwort. Auf diese Weise müssten alte Benutzer nicht zum Ändern ihrer Kennwörter nach der Migration der app.
5. Den Konstruktor für die UserManager-Klasse zu deklarieren und dies als die SQLPasswordHasher der Eigenschaft im Konstruktor zu übergeben.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Verwaltungsseiten für neues Konto erstellen

Der nächste Schritt bei der Migration ist Verwaltungsseiten Konto hinzufügen, mit denen einen Benutzer registrieren und melden Sie sich. Die alte Konto-Seiten aus der SQL-Mitgliedschaft verwenden Sie Steuerelemente, die nicht mit dem neuen Identitätssystem funktionieren. Beim Hinzufügen des neuen Benutzers Verwaltungsseiten für das Tutorial unter diesem Link [ https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project ](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) beginnend bei Schritt "Web Forms für die Registrierung von Benutzern zu Ihrer Anwendung hinzufügen", da wir bereits des Projekts und Hinzufügen des NuGet-Pakets Pakete.

Wir müssen einige Änderungen für das Beispiel mit dem Projekt funktioniert, haben wir hier machen.

- Der Register.aspx.cs und Login.aspx.cs CodeBehind-Klassen verwenden die `UserManager` von Identity-Paketen zum Erstellen eines Benutzers. In diesem Beispiel verwenden Sie die UserManager im Ordner "Models" hinzugefügt werden, mithilfe der oben genannten Schritte.
- Verwenden Sie die Benutzer-Klasse, die erstellt wurden, sodass die "identityuser" in Register.aspx.cs und Login.aspx.cs CodeBehind-Klassen. Dies klinkt sich in unsere benutzerdefinierte Klasse in das Identitätssystem.
- Das Part, für die Datenbank zu erstellen, kann übersprungen werden.
- Der Entwickler muss die Anwendungs-ID für den neuen Benutzer entsprechend der aktuellen Anwendung-ID festgelegt. Dies kann erfolgen, indem Sie die Anwendungs-ID für diese Anwendung Abfragen, bevor ein Objekt in der Register.aspx.cs-Klasse erstellt wird und vor dem Erstellen des Benutzers festlegen. 

    Beispiel:

    Definieren Sie eine Methode auf Abfragen das Aspnet-Register.aspx.cs\_Anwendungen Tabelle, und erhalten Sie die Anwendungs-Id nach Anwendungsname

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Jetzt erhalten legen Sie diese Einstellung für das Benutzerobjekt

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Verwenden Sie alte Benutzername und Kennwort, um einen vorhandenen Benutzer anmelden. Verwenden Sie die Seite "registrieren", um einen neuen Benutzer erstellen. Außerdem stellen Sie sicher, dass die Benutzer in Rollen wie erwartet.

Portieren auf die Identity-System hilft dem Benutzer, die offene Authentifizierung (OAuth) an die Anwendung hinzuzufügen. Finden Sie im Beispiel [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) das OAuth aktiviert ist.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben wir zum Portieren von Benutzern von SQL-Mitgliedschaftsanbieter nach ASP.NET Identity, aber wir nicht haben die Profildaten port. Im nächsten Tutorial untersuchen wir die in das Portieren von Profildaten aus SQL-Mitgliedschaftsanbieter auf das neue Identity-System.

Lassen Sie Ihr Feedback über das Ende dieses Artikels.

*Nochmals vielen Dank, Tom Dykstra und Rick Anderson für die im Artikel.*
