---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters | Microsoft-Dokumentation
author: raquelsa
description: ASP.NET Identity ist ein erweiterbares System auf das diese Weise können Sie Ihren eigenen Anbieter erstellen und es in Ihre Anwendung einbinden, ohne die anwendungse erneut verarbeitet werden...
ms.author: aspnetcontent
ms.date: 05/22/2015
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: e5784991a95dcff0db38d87707bc83555da716df
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805674"
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters
====================
durch [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity ist ein erweiterbares System auf das diese Weise können Sie Ihren eigenen Anbieter erstellen und es in Ihre Anwendung einbinden, ohne die Anwendung erneut verarbeitet werden muss. Dieses Thema beschreibt, wie Sie einen MySQL-Speicheranbieter für ASP.NET Identity zu erstellen. Eine Übersicht über benutzerdefinierte Speicheranbieter erstellen, finden Sie unter [Übersicht über die der benutzerdefinierte Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Um dieses Tutorial abzuschließen, müssen Sie Visual Studio 2013 mit Update 2 verfügen.
> 
> Dieses Tutorial verwendet:
> 
> - Veranschaulicht das Erstellen einer MySQL-Datenbankinstanz in Azure.
> - Verwenden Sie eine MySQL-Client-Tool (MySQL Workbench) Tabellen erstellen und Verwalten der remote-Datenbank in Azure veranschaulicht.
> - Veranschaulicht die Standard-Speicher-Implementierung von ASP.NET Identity durch unsere benutzerdefinierte Implementierung für ein MVC-Anwendungsprojekt zu ersetzen.
> 
> In diesem Tutorial wurde ursprünglich von Raquel Soares De Almeida und Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Das Beispielprojekt wurde für Identität 2.0 von Suhas Joshi aktualisiert. Das Thema wurde für Identität 2.0 von Tom FitzMacken aktualisiert.


## <a name="download-completed-project"></a>Heruntergeladenen Projekt

Am Ende dieses Tutorials müssen Sie ein MVC-Anwendungsprojekt mit ASP.NET Identity arbeiten mit einer MySQL-Datenbank in Azure gehostet.

Sie können den abgeschlossenen MySQL-Speicheranbieter auf [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Die Schritte, die Sie ausführen

In diesem Lernprogramm führen Sie folgende Aktionen ausführen:

1. Erstellen Sie eine MySQL-Datenbank in Azure
2. Erstellen von Tabellen in MySQL für die ASP.NET Identity
3. Erstellen Sie eine MVC-Anwendung und konfigurieren Sie, dass den MySQL-Anbieter verwendet
4. Ausführen der App

In diesem Thema werden nicht behandelt, die Architektur von ASP.NET Identity und die Entscheidungen, die Sie bei der Implementierung eines Speicheranbieters Kunden treffen müssen. Diese Informationen finden Sie unter [Übersicht über die der benutzerdefinierte Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Überprüfen Sie die MySQL-Storage-Anbieterklassen

Vor der Installation die Schritte zum Erstellen des MySQL-Speicheranbieters, sehen wir uns die Klassen, die den Speicheranbieter bilden. Sie benötigen, die die Datenbankvorgänge zu verwalten und Klassen, die von der Anwendung zum Verwalten von Benutzern und Rollen aufgerufen werden.

### <a name="storage-classes"></a>Speicherklassen

- ["Identityuser"](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -Eigenschaften für den Benutzer enthält.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -Vorgänge zum Hinzufügen, aktualisieren oder Abrufen von Benutzern enthält.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -Eigenschaften für Rollen enthält.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -Vorgänge zum Hinzufügen, löschen, aktualisieren und Abrufen von Rollen enthält.

### <a name="data-access-layer-classes"></a>Data Access Layer Klassen

In diesem Beispiel enthalten die Ebene der Datenzugriffsklassen SQL-Anweisungen für die Arbeit mit den Tabellen; in Ihrem Code möchten jedoch kann objektrelationales Mapping (ORM) wie Entity Framework oder NHibernate zu verwenden. Insbesondere kann Ihre Anwendung eine schlechte Leistung ohne ORM auftreten, die lazy Loading und das Zwischenspeichern von Objekten enthält. Weitere Informationen finden Sie unter [ASP.NET Identity 2.0 ohne Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -enthält die Verbindung für MySQL-Datenbank und die Methoden zum Ausführen von Datenbankoperationen. UserStore und RoleStore, die beide mit einer Instanz dieser Klasse instanziiert werden.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -Datenbankvorgänge für die Tabelle, die Rollen speichert enthält.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -Datenbankvorgänge für die Tabelle, die Ansprüche des Benutzers speichert enthält.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -Datenbankvorgänge für die Tabelle, die Anmeldeinformationen des Benutzers speichert enthält.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -enthält-Datenbankvorgänge für die Tabelle, gespeichert, welche Benutzer welchen Rollen zugewiesen werden.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -Datenbankvorgänge für die Tabelle, die Benutzer speichert enthält.

## <a name="create-a-mysql-database-instance-on-azure"></a>Erstellen Sie eine MySQL-Datenbankinstanz in Azure

1. Melden Sie sich beim [Azure-Portal](https://manage.windowsazure.com/) an.
2. Klicken Sie auf **+ neu** am unteren Rand der Seite, und wählen Sie dann **STORE**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. In der **auswählen und der Add-On-** Assistenten **ClearDB MySQL-Datenbank** und klicken Sie auf den Vorwärtspfeil unten rechts im Dialogfeld.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Behalten Sie den Standardwert **Free** planen, und ändern Sie die **Namen** zu **IdentityMySQLDatabase**. Wählen Sie die Region in Ihrer Nähe, und klicken Sie dann auf den Pfeil "Weiter".  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Klicken Sie auf das Häkchen, um die Datenbankerstellung abzuschließen.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Nachdem Ihre Datenbank erstellt wurde, können Sie verwalten, von der **-Add-Ons** Registerkarte im Verwaltungsportal.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Sie können die Datenbankverbindungsinformationen abrufen, indem Sie durch Klicken auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche "Kopieren" aus, und speichern Sie, um Sie weiter unten in der MVC-Anwendung verwenden können.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Erstellen von Tabellen in einer MySQL-Datenbank für die ASP.NET Identity

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installieren von MySQL Workbench-Tool, um eine Verbindung herstellen und Verwalten von MySQL-Datenbank

1. Installieren Sie die **MySQL Workbench** tool die [MySQL-Downloadseite](http://dev.mysql.com/downloads/windows/installer/)
2. Starten Sie die app aus, und klicken Sie auf Hinzufügen der **MySQLConnections +** , um eine neue Verbindung hinzuzufügen. Verwenden Sie die Verbindungsdaten für die Zeichenfolge, die Sie aus der Azure-MySQL-Datenbank kopiert, die Sie zuvor in diesem Tutorial erstellt haben.
3. Nach dem Herstellen der Verbindung an, öffnen Sie ein neues **Abfrage** Registerkarte; fügen Sie die Befehle von [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) in die Abfrage und führen Sie es um die Datenbanktabellen erstellen.
4. Sie haben jetzt alle ASP.NET Identity erforderlichen Tabellen in einer MySQL-Datenbank in Azure gehostet werden, wie unten gezeigt erstellt.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Erstellen Sie eine MVC-Anwendungsprojekt aus Vorlage und konfigurieren Sie, um MySQL-Anbieters zu verwenden.

Installieren Sie bei Bedarf entweder [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) mit Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Laden Sie das Projekt ASP.NET.Identity.MySQL von CodePlex herunter

1. Navigieren Sie zu die Repository-URL auf [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Den Quellcode herunterladen.
3. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner ein.
4. Öffnen Sie die AspNet.Identity.MySQL-Projektmappe, und erstellen Sie sie.

### <a name="create-a-new-mvc-application-project-from-template"></a>Erstellen Sie ein neues MVC-Anwendungsprojekt aus Vorlage

1. Klicken Sie mit der rechten Maustaste auf die **AspNet.Identity.MySQL** Lösung und **hinzufügen**, **neues Projekt**
2. In der **neues Projekt hinzufügen** Dialogfeld **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Benennen Sie Ihr Projekt **IdentityMySQLDemo**; und klicken Sie dann auf OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. In der **neues ASP.NET-Projekt** Dialogfeld Wählen Sie die MVC-Vorlage mit den Standardoptionen (dazu gehören **einzelne Benutzerkonten** als Authentifizierungsmethode), und klicken Sie auf **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des IdentityMySQLDemo-Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie im Dialogfeld im Feld Suchen Text **Identity.EntityFramework**. Wählen Sie in der Liste der Ergebnisse dieses Paket aus, und klicken Sie auf **Deinstallieren**. Sie werden zum Deinstallieren des Dependency-Pakets EntityFramework aufgefordert werden. Klicken Sie auf Ja, wie nicht mehr an diese Anwendung.
5. Klicken Sie mit der rechten Maustaste auf das IdentityMySQLDemo-Projekt, wählen Sie **hinzufügen**, **Verweis, Projektmappen, Projekten;** wählen Sie das AspNet.Identity.MySQL-Projekt, und klicken Sie auf **OK**.
6. Ersetzen Sie im Projekt IdentityMySQLDemo alle Verweise auf  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   durch  
     `using AspNet.Identity.MySQL;`
7. Legen Sie im IdentityModels.cs, **ApplicationDbContext** abgeleitet **MySqlDatabase** und fügen Sie einen Konstruktor, der einen einzelnen Parameter mit dem Namen der Verbindung.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Öffnen Sie die IdentityConfig.cs-Datei. In der **ApplicationUserManager.Create** -Methode, instanziieren die UserManager durch den folgenden Code ersetzen:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Öffnen Sie die Datei "Web.config" ein, und Ersetzen Sie die Zeichenfolge DefaultConnection mit diesem Eintrag, und Ersetzen Sie dabei die hervorgehobenen Werte durch die Verbindungszeichenfolge der MySQL-Datenbank, die Sie auf den vorherigen Schritten erstellt haben:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Führen Sie die app und eine Verbindung mit der MySQL-Datenbank

1. Klicken Sie mit der rechten Maustaste auf die **IdentityMySQLDemo** Projekt, und wählen **als Startprojekt festlegen**
2. Drücken Sie **STRG + F5** erstellen und Ausführen der app.
3. Klicken Sie auf **registrieren** Registerkarte oben auf der Seite.
4. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Der neue Benutzer ist nun registriert und angemeldet.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wechseln Sie zurück zu dem Tool MySQL Workbench, und überprüfen Sie die **IdentityMySQLDatabase** des Tabelleninhalts. Überprüfen der Tabelle für die Einträge an, wie Sie neue Benutzer registrieren.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Aktivieren der anderen Authentifizierungsmethoden in dieser app finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Um zu erfahren, wie Sie Ihre Datenbank mit OAuth zu integrieren und Rollen einrichten, um Benutzern den Zugriff auf Ihre app zu beschränken, finden Sie unter [bereitstellen eine sicheren ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
