---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migration von einer vorhandenen Website von SQL-Mitgliedschaft zu ASP.NET Identity | Microsoft Docs
author: Rick-Anderson
description: Dieses Lernprogramm veranschaulicht die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer- und Rollendaten mit SQL-Mitgliedschaft in der neuen ASP.NET Identity s erstellt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/19/2014
ms.topic: article
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: b88cd54040c02c977a83e20d7af7fda4fff969c1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migration von einer vorhandenen Website von SQL-Mitgliedschaft zu ASP.NET Identity
====================
durch [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)

> Dieses Lernprogramm veranschaulicht die Schritte zum Migrieren einer vorhandenen Webanwendung mit Benutzer- und Rollendaten mithilfe von SQL-Mitgliedschaft auf das neue ASP.NET Identity-System erstellt. Bei diesem Ansatz muss, ändern das vorhandene Datenbankschema auf eine von den ASP.NET Identity und Hook in den alten/neue Klassen darauf benötigt. Nachdem Sie diesen Ansatz verwenden, nach der Migration Ihrer Datenbank, werden zukünftige Updates Identität Analyseergebnisse behandelt.


Für dieses Lernprogramm dauert es eine Webanwendungsvorlage aus (Webformulare) mithilfe von Visual Studio 2010 zum Erstellen von Benutzer-und Rolle erstellt. Wir wird dann SQL-Skripts verwenden, um die vorhandene Datenbank zu Tabellen erforderlich sind, vom System Identität zu migrieren. Als Nächstes wir die nötigen NuGet-Pakete installieren und Verwaltungsseiten für den neuen Konto die verwenden das Identitätssystem für die Verwaltung der Mitgliedschaft hinzuzufügen. Migration zu testen sollte Benutzer, die mit SQL-Mitgliedschaft erstellt sollte in der Lage, melden Sie sich neue Benutzer auch registrieren. Sie können das vollständige Beispiel finden [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/). Siehe auch [Migrieren von ASP.NET-Mitgliedschaft zu ASP.NET Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Erste Schritte

### <a name="creating-an-application-with-sql-membership"></a>Erstellen einer Anwendung mit SQL-Mitgliedschaft

1. Wir müssen mit einer vorhandenen Anwendung zu starten, verwendet die SQL-Mitgliedschaft und Daten für Benutzer und die Rolle hat. Erstellen Sie zu diesem Artikel wir eine Webanwendung in Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Mit dem Tool ASP.NET-Konfiguration 2 Benutzer erstellen: **OldAdminUser** und **OldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Erstellen Sie eine Rolle mit dem Namen Administrators, und fügen Sie "OldAdminUser" als Benutzer in dieser Rolle hinzu.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Erstellen Sie einen Benutzeradministrator (Abschnitt) des Standorts mit einer "default.aspx" ein. Legen Sie das Tag für die Autorisierung in der Datei "Web.config" für den Zugriff nur auf Benutzer mit Admin-Rollen. Weitere Informationen finden Sie hier [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Zeigen Sie die Datenbank im Server-Explorer, um zu verstehen, die Tabellen erstellt, indem das Mitgliedschaftssystem SQL an. Die Benutzerdaten für die Anmeldung werden gespeichert, in das Aspnet\_Benutzer und Aspnet\_Mitgliedschaft enthalten sind, während Daten der Rolle in der Aspnet gespeichert ist\_Rollen-Tabelle. Informationen darüber, welche Benutzer sind die Rollen in der Aspnet gespeichert werden\_UsersInRoles-Tabelle. Für die Verwaltung von Basis-Mitgliedschaft ist es ausreichend, um die Informationen in den obigen Tabellen mit dem ASP.NET Identity-System-port.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrieren zu Visual Studio 2013

1. Installieren von Visual Studio Express 2013 für Web oder Visual Studio 2013 zusammen mit den [neuesten Updates](https://www.microsoft.com/en-us/download/details.aspx?id=44921).
2. Öffnen Sie die oben genannten Projekt in Ihrer installierten Version von Visual Studio. Wenn SQL Server Express auf dem Computer nicht installiert ist, wird eine Aufforderung angezeigt, wenn das Projekt öffnen, da die Verbindungszeichenfolge SQL Express verwendet. Sie können wahlweise installieren Sie SQL Express oder als umgehen, ändern die Verbindungszeichenfolge auf "LocalDb". Für diesen Artikel werden wir sie auf "LocalDb" ändern.
3. Öffnen Sie die Datei "Web.config", und ändern Sie die Verbindungszeichenfolge aus. SQLExpess, V11. 0 (LocalDb). Entfernen Sie "User Instance = True" aus der Verbindungszeichenfolge.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Öffnen Sie Server-Explorer, und stellen Sie sicher, dass das Tabellenschema und Daten angezeigt werden können.
5. Die ASP.NET Identity-System funktioniert mit Version 4.5 oder höher des Frameworks. Richten Sie die Anwendung auf 4.5 oder höher neu.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Erstellen Sie das Projekt, um sicherzustellen, dass keine Fehler vorliegen.

### <a name="installing-the-nuget-packages"></a>Installieren die NuGet-Pakete

1. Im Projektmappen-Explorer mit der Maustaste des Projekts &gt; **NuGet-Pakete verwalten**. Geben Sie in das Suchfeld "Asp.net Identity" aus. Wählen Sie das Paket in der Liste der Ergebnisse, und klicken Sie auf installieren. Akzeptieren Sie den Lizenzvertrag, klicken Sie auf die Schaltfläche "Akzeptieren". Beachten Sie, dass dieses Paket die abhängigkeitspakete installiert wird: EntityFramework und Microsoft ASP.NET Identity Core. Auf ähnliche Weise installieren Sie die folgenden Pakete (überspringen die letzten 4 OWIN-Pakete aus, wenn Sie nicht OAuth-Protokoll-in aktivieren möchten):

    - Microsoft.AspNet.Identity.Owin
    - "Microsoft.owin.Host.systemweb"
    - "Microsoft.owin.Security.Facebook"
    - "Microsoft.owin.Security.Google"
    - "Microsoft.owin.Security.MicrosoftAccount"
    - "Microsoft.owin.Security.Twitter"

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrieren Sie die Datenbank auf die neue Identitätssystem

Der nächste Schritt besteht darin die vorhandene Datenbank zu einem Schema des Systems ASP.NET Identity zu migrieren. Um zu erreichen wir führen Sie eine SQL Skript die über eine Reihe von Befehlen zum Erstellen neuer Tabellen und migrieren vorhandene Benutzerinformationen zu den neuen Tabellen verfügt. Die Skriptdatei verwendbaren [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Mit der Skriptdatei ist spezifisch für dieses Beispiel. Wenn das Schema für die Tabellen erstellt mithilfe von SQL-Mitgliedschaft ist eine benutzerdefinierte Aktivität oder Skripts müssen entsprechend geändert werden geändert.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Gewusst wie: Generieren Sie das SQL-Skript für die Migration von Schemas

Für ASP.NET Identity-Klassen, um sofort mit den Daten von vorhandenen Benutzern verwendet werden müssen wir das Datenbankschema auf das von ASP.NET Identity benötigt zu migrieren. Wir können dies durch neue Tabellen hinzugefügt werden, und kopieren die vorhandene Informationen in diesen Tabellen durchführen. Standardmäßig verwendet die ASP.NET Identity EntityFramework zuordnen die Identität Modellklassen wieder in der Datenbank zum Speichern/Abrufen von Informationen. Diese Modellklassen implementieren Schnittstellen Identity Core Benutzer- und Role-Objekte definieren. Die Tabellen und Spalten in der Datenbank basieren auf diese Modellklassen. Die EntityFramework Modellklassen in Identity v2.1.0 und ihre Eigenschaften werden wie unten definiert

| **IdentityUser** | **Typ** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Id | string | Id | RoleID-Wert | Dem "providerkey" | Id |
| Benutzername | string | Name | Benutzer-ID | Benutzer-ID | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Benutzer\_Id |
| E-Mail | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| Telefonnummer | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| "Accessfailedcount" | int |  |  |  |  |

Tabellen mit Spalten, die Eigenschaften für jedes dieser Modelle aufweisen müssen. Die Zuordnung zwischen Klassen und Tabellen wird definiert, der `OnModelCreating` Methode der `IdentityDBContext`. Dies bezeichnet man die fluent-API-Methode der Konfiguration und Weitere Informationen finden Sie [hier](https://msdn.microsoft.com/en-us/data/jj591617.aspx). Die Konfiguration für die Klassen ist, wie unten beschrieben.

| **Klasse** | **Table** | **Primärschlüssel** | **Fremdschlüssel** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Id |  |
| IdentityRole | AspnetRoles | Id |  |
| IdentityUserRole | AspnetUserRole | Benutzer-ID + RoleId | Benutzer\_-Id -&gt;AspnetUsers RoleId -&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | Dem "providerkey" + UserId + LoginProvider | UserId -&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Id | Benutzer\_-Id -&gt;AspnetUsers |

Mit diesen Informationen können wir die SQL-Anweisungen zum Erstellen von neuer Tabellen erstellen. Wir können jede Anweisung einzeln schreiben oder generieren das gesamte Skript mithilfe von EntityFramework-PowerShell-Befehle, die wir, klicken Sie dann bearbeiten können nach Bedarf. Dazu in Visual Studio öffnen die **Package Manager Console** aus der **Ansicht** oder **Tools** Menü

- Führen Sie "Enable-Migrations"-Befehls zum Aktivieren von EntityFramework Migrationen.
- Ausführen des Befehls "Add-Migration anfängliche", die das Anfangssetup Code zum Erstellen der Datenbank in c# erstellt / VB dar.
- Der letzte Schritt besteht, führen Sie "Update-Database – Skript"-Befehl, der das SQL-Skript generiert basierend auf der Modell-Klasse.

Diese Generation Datenbankskripts kann anfangs verwendet werden, in dem wir zusätzliche Änderungen verdienen werden, um neue Spalten hinzuzufügen, und Kopieren von Daten. Der Vorteil dieses ist, dass wir generieren die `_MigrationHistory` Tabelle, die das Datenbankschema zu ändern, wenn das Modell für zukünftige Versionen von Identität Versionen Änderung Klassen von EntityFramework verwendet wird. 

SQL-Mitgliedschaftsbenutzerinformationen mussten einer anderen Eigenschaften außer den im Modell Identität Benutzerklasse nämlich-e-Mail, Kennworteingaben, Datum der letzten Anmeldung, Datum der letzten Sperrung usw. an. Dies ist nützlich, Informationen, und wir möchten es auf das Identitätssystem übertragen werden. Dies kann durch Hinzufügen von zusätzlichen Eigenschaften zu Benutzermodell und ihre Zuordnung wieder zu Tabellenspalten in der Datenbank erfolgen. Wir hierzu eine Klasse hinzufügen, Unterklassen der `IdentityUser` Modell. Wir können diese benutzerdefinierte Klasse Eigenschaften hinzu, und bearbeiten die SQL-Skript aus, um die entsprechenden Spalten hinzufügen, beim Erstellen der Tabelle. Der Code für diese Klasse wird im Artikel beschrieben. Das SQL-Skript zum Erstellen der `AspnetUsers` Tabelle nach dem Hinzufügen neuer Eigenschaften wäre

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

Als Nächstes müssen wir die vorhandene Informationen aus der Datenbank der SQL-Mitgliedschaft für Identität mit den neu hinzugefügten Tabellen zu kopieren. Dies kann über SQL erfolgen Daten direkt aus einer Tabelle in eine andere kopieren. Um Daten in den Zeilen der Tabelle hinzuzufügen, verwenden wir die `INSERT INTO [Table]` erstellen. So kopieren Sie aus einer anderen Tabelle wir verwenden die `INSERT INTO` Anweisung zusammen mit der `SELECT` Anweisung. Um die Benutzerinformationen zu erhalten, wir Abfragen müssen, der *Aspnet\_Benutzer* und *Aspnet\_Mitgliedschaft* Tabellen, und kopieren Sie die Daten auf die *AspNetUsers*Tabelle. Wir verwenden die `INSERT INTO` und `SELECT` zusammen mit `JOIN` und `LEFT OUTER JOIN` Anweisungen. Weitere Informationen zu Abfragen und Kopieren von Daten zwischen Tabellen, finden Sie unter [dies](https://technet.microsoft.com/en-us/library/ms190750%28v=sql.105%29.aspx) Link. Darüber hinaus sind die AspnetUserLogins AspnetUserClaims Tabellen und leere zunächst, da es sind keine Informationen in der SQL-Mitgliedschaften, die dies standardmäßig zugeordnet. Die einzige Informationen, die kopiert wird für Benutzer und Rollen. Für das Projekt in den vorherigen Schritten erstellt haben wäre die SQL-Abfrage, um Informationen zur Users-Tabelle kopieren

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

In der obigen SQL-Anweisung, Informationen zu jedem Benutzer aus der *Aspnet\_Benutzer* und *Aspnet\_Mitgliedschaft* in den Spalten der Tabellen kopiert die  *AspnetUsers* Tabelle. Die einzige Änderung, die es bereits hier getan wird, wenn das Kennwort zu kopieren. Da der Verschlüsselungsalgorithmus für Kennwörter in SQL-Mitgliedschaft "PasswordSalt" und "PasswordFormat" verwendet, kopieren es, zu zusammen mit dem verschlüsselten Kennwort, damit es verwendet werden kann, das Kennwort Entschlüsseln von Identität. Dies wird erläutert, in dem Artikel weiter, wenn Sie ein benutzerdefiniertes Kennwort Hasher einbinden. 

Mit der Skriptdatei ist spezifisch für dieses Beispiel. Für Anwendungen, die zusätzliche Tabellen haben, können Entwickler einen ähnlichen Ansatz zum Hinzufügen zusätzlicher Eigenschaften für die Modell-Klasse, und ordnen sie Spalten in der Tabelle AspnetUsers folgen. Zum Ausführen des Skripts

1. Öffnen Sie Server-Explorer. Erweitern Sie die Verbindung "ApplicationServices", um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf den Knoten "Tabellen", und wählen Sie die Option 'Neue Abfrage'

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. Kopieren Sie im Abfragefenster die gesamte SQL-Skript aus der Datei Migrations.sql. Führen Sie die Skriptdatei, indem Sie auf die Pfeilschaltfläche 'Execute'.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Aktualisieren Sie das Server-Explorer-Fenster. Fünf neue Tabellen werden in der Datenbank erstellt.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    Unten ist wie die Informationen in der SQL-Mitgliedertabellen auf das neue Identitätssystem zugeordnet sind.

    ASPNET\_Rollen--&gt; AspNetRoles

    ASP\_NetUsers und Asp\_NetMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Wie im vorherigen Abschnitt erläutert wird, sind die Tabellen AspNetUserClaims und AspNetUserLogins leer. Das Feld "Unterscheidungseigenschaft" in der Tabelle AspNetUser übereinstimmen der Modellname-Klasse, die im nächsten Schritt definiert ist. Auch die PasswordHash Spalte ist in der Form ' verschlüsselte Kennwort | Saltwert des Kennworts | Kennwortformat ". Dadurch können Sie spezielle SQL Mitgliedschaft Crypto Logik verwenden, damit Sie alte Kennwörter wiederverwenden können. Wird in später in diesem Artikel erläutert.

### <a name="creating-models-and-membership-pages"></a>Erstellen von Modellen und Mitgliedschaftsseiten

Wie bereits erwähnt, mithilfe der Funktion für die Entity Framework zum Kommunizieren mit der Datenbank zum Speichern von Kontoinformationen in der Standardeinstellung verwendet. Zum Arbeiten mit vorhandenen Daten in der Tabelle müssen wir Modellklassen zu erstellen, das die Tabellen zugeordnet und in das Identitätssystem einbinden. Im Rahmen des Vertrags Identität die Modellklassen sollten entweder in der Dll Identity.Core Schnittstellen implementieren oder können erweitern, die vorhandene Implementierung dieser Schnittstellen in Microsoft.AspNet.Identity.EntityFramework verfügbar.

In diesem Beispiel haben die Tabellen AspNetRoles, AspNetUserClaims, AspNetLogins und AspNetUserRole Spalten, die die vorhandene Implementierung von das Identitätssystem ähnlich sind. Daher können wir die vorhandenen Klassen zuordnen zu diesen Tabellen wiederverwenden. Die AspNetUser-Tabelle enthält einige zusätzlichen Spalten, die verwendet werden, um zusätzliche Informationen aus der Mitgliedschaft SQL-Tabellen speichern. Dies kann durch Erstellen einer Modellklasse, die erweitert die vorhandenen Implementierung von "IdentityUser", und fügen die zusätzlichen Eigenschaften zugeordnet werden.

1. Modelle erstellt einen Ordner im Projekt und fügen Sie eine Benutzer-Klasse. Der Name der Klasse übereinstimmen, die Daten in der ' Unterscheidungsspalte "der Tabelle"AspnetUsers"hinzugefügt.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    Erweitern Sie die Benutzer-Klasse sollten die IdentityUser Klasse in der *Microsoft.AspNet.Identity.EntityFramework* Dll. Deklarieren Sie die Eigenschaften in der Klasse, die die AspNetUser Spalten zugeordnet. Die Eigenschaften-ID, Benutzername, PasswordHash und SecurityStamp in der IdentityUser definiert sind und daher ausgelassen. Im folgenden wird der Code für die Klasse, die alle Eigenschaften aufweist

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Eine Entity Framework DbContext-Klasse ist erforderlich, um Daten in Modellen an Tabellen beibehalten und Abrufen von Daten aus Tabellen in die Modelle zu füllen. *Microsoft.AspNet.Identity.EntityFramework* Dll definiert die Interaktion mit der Identity-Tabellen abrufen und Speichern von Informationen IdentityDbContext-Klasse. IdentityDbContext&lt;Tuser&gt; akzeptiert eine "TUser"-Klasse, die jede Klasse sein kann, die die IdentityUser-Klasse erweitert.

    Erstellen Sie eine neue Klasse ApplicationDBContext, der sich unterhalb des Ordners "Modelle" in der 'User'-Klasse, die in Schritt 1 erstellte übergeben IdentityDbContext erstreckt

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. Benutzerverwaltung in das neue Identitätssystem erfolgt mithilfe der UserManager&lt;Tuser&gt; in Klasse definiert die *Microsoft.AspNet.Identity.EntityFramework* Dll. Wir müssen eine benutzerdefinierte Klasse zu erstellen, die UserManager, übergeben in der 'User'-Klasse, die in Schritt 1 erstellte erweitert.

    Erstellen Sie im Ordner "Modelle" eine neue Klasse UserManager, die UserManager erweitert&lt;Benutzer&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Die Kennwörter der Benutzer der Anwendung werden verschlüsselt und in der Datenbank gespeichert. Der kryptografischen Algorithmus in SQL-Mitgliedschaft unterscheidet sich von dem das neue Identitätssystem. Alte Kennwörter wiederverwenden müssen Kennwörter selektiv zu entschlüsseln, wenn der alte Benutzer melden Sie sich mit dem SQL-Mitgliedschaften-Algorithmus bei der Verwendung von der Crypto-Algorithmus in Identität für die neuen Benutzer.

    Die UserManager-Klasse verfügt über eine Eigenschaft "PasswordHasher" auf dem eine Instanz einer Klasse gespeichert, der 'Ipasswordhasher.'-Schnittstelle implementiert. Dient zum Verschlüsseln und während der Authentifizierung Benutzertransaktionen Entschlüsseln von Kennwörtern. In der UserManager-Klasse, die in Schritt 3 definierten, erstellen Sie eine neue Klasse SQLPasswordHasher, und kopieren Sie den folgenden Code.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Beheben Sie Kompilierungsfehler, indem Sie die System.Text und System.Security.Cryptography-Namespace importieren.

    Die Methode EncodePassword verschlüsselt das Kennwort entsprechend die standardmäßige SQL-Mitgliedschaft Crypto-Implementierung. Dies ist die Dll System.Web entnommen. Wenn die alte app eine benutzerdefinierte Implementierung verwendet sollte er hier angegeben. Es müssen zwei andere Methoden definieren *HashPassword* und *VerifyHashedPassword* , bei denen die *EncodePassword* Methode, um ein bestimmtes Kennwort hash oder Überprüfen von nur-Text Kennwort mit dem einer vorhandenen in der Datenbank.

    Das Mitgliedschaftssystem SQL verwendet PasswordHash, PasswordSalt und PasswordFormat Hash des Kennworts, die vom Benutzer eingegeben werden, wenn sie registrieren, oder ihr Kennwort ändern. Während der Migration werden alle drei Felder gespeichert, in der PasswordHash-Spalte in der AspNetUser-Tabelle, getrennt durch die ' |' Zeichen. Wenn ein Benutzer anmeldet, und das Kennwort diese Felder besitzt, verwenden wir, überprüfen Sie das Kennwort der SQL-Mitgliedschaft Kryptografie; Verwenden andernfalls das Identitätssystem Standard Crypto zum Überprüfen des Kennworts. Auf diese Weise würde alte Benutzer keine, ihre Kennwörter zu ändern, nachdem die app migriert wird.
5. Deklarieren Sie den Konstruktor für die UserManager Klasse und übergeben Sie das als die SQLPasswordHasher der Eigenschaft im Konstruktor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Verwaltungsseiten für neues Konto erstellen

Der nächste Schritt der Migration besteht darin Verwaltungsseiten Konto hinzuzufügen, mit denen einen Benutzer registrieren und anmelden. Das alte Konto Seiten aus der SQL-Mitgliedschaft in verwenden Sie Steuerelemente, die mit der neuen Identität-System nicht unterstützt. Beim Hinzufügen des neuen Benutzers Verwaltungsseiten Lernprogramm unter diesem Link folgen [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) beginnend mit dem Schritt " Hinzufügen von Web Forms für die Registrierung von Benutzern für Ihre Anwendung ", da es bereits das Projekt erstellt und die NuGet-Pakete hinzugefügt haben.

Wir müssen nehmen einige Änderungen für das Beispiel mit dem Projekt arbeiten, das wir hier haben.

- Die Register.aspx.cs und Login.aspx.cs CodeBehind-Klassen verwenden die `UserManager` von Identity-Paketen zum Erstellen eines Benutzers. Verwenden Sie für dieses Beispiel die UserManager im Ordner Models anhand der oben genannten Schritte hinzugefügt.
- Verwenden Sie die Benutzer-Klasse, die anstelle der IdentityUser Register.aspx.cs und Login.aspx.cs Code-behind Klassen erstellt. Dies hooks in unsere benutzerdefinierte Klasse in das Identitätssystem.
- Das Webpart zum Erstellen der Datenbank kann übersprungen werden.
- Der Entwickler muss die Anwendungs-ID für den neuen Benutzer entsprechend die aktuellen Anwendung-ID festgelegt. Dies kann erfolgen, indem Sie die Anwendungs-ID für diese Anwendung Abfragen, bevor ein Benutzerobjekt in der Register.aspx.cs-Klasse erstellt wird und vor dem Erstellen eines Benutzers festlegen. 

    Beispiel:

    Definieren Sie eine Methode Register.aspx.cs auf der Seite das Aspnet Abfragen\_Anwendungen Tabelle und die Anwendung Id nach Anwendungsname

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Jetzt erhalten legen Sie diese Einstellung für das Benutzerobjekt

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Verwenden Sie die alte Benutzername und Kennwort anmelden, einen vorhandenen Benutzer. Verwenden Sie der Seite "registrieren", um einen neuen Benutzer erstellen. Außerdem stellen Sie sicher, dass der Benutzer Mitglied der Rollen sind wie erwartet.

Portieren auf die Identitätssystem hilft dem Benutzer, die offene Authentifizierung (OAuth) der Anwendung hinzufügen. Finden Sie in dem Beispiel [hier](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/SQLMembership-Identity-OWIN/) das OAuth aktiviert wurde.

## <a name="next-steps"></a>Nächste Schritte

In diesem Lernprogramm wird gezeigt, wie so portieren Sie Benutzer aus der SQL-Mitgliedschaft in ASP.NET Identity, aber wir haben nicht Profildaten Ports. In den nächsten Lernprogrammen erfahren Sie in die Profildaten aus der SQL-Mitgliedschaft in der neuen Identität System portieren.

Sie können Feedback am Ende dieses Artikels lassen.

*Unser Dank gilt Tom Dykstra und Rick Anderson zum Überprüfen des Artikels.*
