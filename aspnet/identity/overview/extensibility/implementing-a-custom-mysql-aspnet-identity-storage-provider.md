---
uid: identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
title: Implementieren einen benutzerdefinierten MySQL ASP.NET-Identität Speicheranbieter | Microsoft Docs
author: raquelsa
description: ASP.NET Identity ist ein erweiterbares System können Sie Ihren eigenen Speicheranbieter erstellen und in die Anwendung eingebunden werden, ohne die anwe erneut verarbeitet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 248f5fe7-39ba-40ea-ab1e-71a69b0bd649
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/implementing-a-custom-mysql-aspnet-identity-storage-provider
msc.type: authoredcontent
ms.openlocfilehash: d843b31e011fe520aad6cfdab0beca2d12477f12
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="implementing-a-custom-mysql-aspnet-identity-storage-provider"></a>Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters
====================
durch [Raquel Soares De Almeida](https://github.com/raquelsa), [Suhas Joshi](https://github.com/suhasj), [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity ist ein erweiterbares System können Sie Ihren eigenen Speicheranbieter erstellen und in die Anwendung eingebunden werden, ohne die Anwendung erneut verarbeitet werden. Dieses Thema beschreibt, wie einen MySQL-Speicheranbieter für ASP.NET Identity zu erstellen. Einen Überblick über das Erstellen von benutzerdefinierten Speicheranbietern finden Sie unter [Übersicht der benutzerdefinierten Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).
> 
> Um dieses Lernprogramm abzuschließen, müssen Sie Visual Studio 2013 Update 2 verfügen.
> 
> In diesem Lernprogramm wird:
> 
> - Zeigen Sie, wie eine MySQL-Datenbankinstanz auf Azure zu erstellen.
> - Veranschaulichen Sie, wie ein MySQL-Clienttool (MySQL-Workbench) Tabellen erstellen und Verwalten der remote-Datenbank in Azure.
> - Erläutert die Standard-Implementierung von ASP.NET Identity Speicher durch unsere benutzerdefinierte Implementierung in einer MVC-Anwendungsprojekt zu ersetzen.
> 
> Dieses Lernprogramm wurde ursprünglich von Raquel Soares De Almeida und Rick Anderson geschrieben ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ). Das Beispielprojekt wurde vom Suhas Joshi für Identität 2.0 aktualisiert. Das Thema wurde vom Tom FitzMacken für Identität 2.0 aktualisiert.


## <a name="download-completed-project"></a>Download abgeschlossen-Projekt

Am Ende dieses Lernprogramms müssen Sie ein MVC-Anwendungsprojekt mit ASP.NET Identity arbeiten mit einer MySQL-Datenbank unter Azure gehostet.

Sie können den abgeschlossenen MySQL-Speicheranbieter am herunterladen [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).

## <a name="the-steps-you-will-perform"></a>Die Schritte, die Sie durchführen möchten

In diesem Lernprogramm führen Sie folgende Aktionen ausführen:

1. Erstellen Sie eine MySQL-Datenbank in Azure
2. Erstellen Sie die ASP.NET Identity-Tabellen in MySQL
3. Erstellen einer MVC-Anwendung und konfigurieren, um MySQL-Anbieter zu verwenden.
4. Ausführen der App

In diesem Thema behandelt nicht die Architektur von ASP.NET Identity, sowie die Entscheidungen, die Sie beim Implementieren eines Anbieters der Customer-Speicher vornehmen müssen. Diese Informationen finden Sie unter [Übersicht der benutzerdefinierten Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md).

## <a name="review-mysql-storage-provider-classes"></a>Überprüfen Sie die MySQL-Anbieter Speicherklassen

Vor der Installation die Schritte zum Erstellen des MySQL-Speicheranbieters, sehen wir uns die Klassen, die den Speicheranbieter bilden. Sie benötigen, Klassen, die die Datenbankvorgänge zu verwalten und Klassen, die von der Anwendung zum Verwalten von Benutzern und Rollen aufgerufen werden.

### <a name="storage-classes"></a>Speicherklassen

- [IdentityUser](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs) -Eigenschaften für den Benutzer enthält.
- [UserStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs) -Vorgänge zum Hinzufügen, aktualisieren oder Abrufen von Benutzern enthält.
- [IdentityRole](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs) -Eigenschaften für Rollen enthält.
- [RoleStore](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) -Vorgänge zum Hinzufügen, löschen, aktualisieren und Abrufen von Rollen enthält.

### <a name="data-access-layer-classes"></a>Datenzugriffsklassen Ebene

In diesem Beispiel enthalten die Ebene Datenzugriffsklassen SQL-Anweisungen zum Arbeiten mit Tabellen; Allerdings können in Ihrem Code Sie ORM (Objektrelationales Mapping) wie z. B. Entity Framework oder NHibernate verwenden möchten. Insbesondere kann die Anwendung eine schlechte Leistung, ohne ein ORM auftreten, die verzögertes Laden und Zwischenspeichern von Objekten enthält. Weitere Informationen finden Sie unter [ASP.NET Identity 2.0 ohne Entity Framework?](https://aspnetidentity.codeplex.com/discussions/561828)

- [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) -enthält die Verbindung für MySQL-Datenbank und die Methoden zum Ausführen von Datenbankoperationen. UserStore und RoleStore, die beide mit einer Instanz dieser Klasse instanziiert werden.
- [RoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) -enthält Sie Datenbankvorgängen für die Tabelle, in Rollen gespeichert.
- [UserClaimsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) -enthält Sie Datenbankvorgängen für die Tabelle, die Benutzeransprüche speichert.
- [UserLoginsTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) -enthält Sie Datenbankvorgängen für die Tabelle, die Anmeldeinformationen des Benutzers speichert.
- [UserRoleTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) -enthält Sie Datenbankvorgängen für die Tabelle, in der gespeichert wird, welche Benutzer auf welche Rollen zugewiesen werden.
- [UserTable](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) -enthält Sie Datenbankvorgängen für die Tabelle, in der Benutzer gespeichert.

## <a name="create-a-mysql-database-instance-on-azure"></a>Erstellen Sie eine MySQL-Datenbankinstanz auf Azure

1. Melden Sie sich auf die [Azure-Portal](https://manage.windowsazure.com/).
2. Klicken Sie auf **+ neu** am unteren Rand der Seite, und wählen Sie dann **STORE**.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.png)
3. In der **auswählen und Add-Ons** Assistenten **ClearDB MySQL-Datenbank** und klicken Sie auf den weiter-Pfeil unten rechts im Dialogfeld.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.png)
4. Behalten Sie den Standardwert **frei** planen, und ändern Sie die **Namen** auf **IdentityMySQLDatabase**. Wählen Sie die Region Ihrer Nähe, und klicken Sie dann auf den Vorwärtspfeil.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.png)
5. Klicken Sie auf das Häkchen, um die Datenbankerstellung abgeschlossen.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image4.png)
6. Nachdem die Datenbank erstellt wurde, können Sie ihn aus verwalten die **ADD-ONS** Registerkarte im Verwaltungsportal.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image5.png)
7. Erhalten Sie durch Klicken auf die Datenbank-Verbindungsinformationen **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image6.png)
8. Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche "Kopieren", und speichern Sie, um Sie weiter unten in der MVC-Anwendung verwenden können.   
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image7.png)

## <a name="create-the-aspnet-identity-tables-in-a-mysql-database"></a>Erstellen von Tabellen in einer MySQL-Datenbank für die ASP.NET Identity

### <a name="install-mysql-workbench-tool-to-connect-and-manage-mysql-database"></a>Installieren von MySQL-Workbench-Tool, um eine Verbindung herstellen und Verwalten von MySQL-Datenbank

1. Installieren der **MySQL-Workbench** -tool aus dem [downloads für MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Starten Sie die app, und klicken Sie auf Hinzufügen der **MySQLConnections +** Schaltfläche, um eine neue Verbindung hinzufügen. Verwenden Sie die Verbindungsdaten für die Zeichenfolge, die Sie aus der Azure-MySQL-Datenbank kopiert, die Sie zuvor in diesem Lernprogramm erstellt haben.
3. Nach dem Herstellen der Verbindung an, öffnen Sie ein neues **Abfrage** Registerkarte ""; Fügen Sie die Befehle von [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql) in das Abfragefenster, und führen Sie sie aus, um den Tabellen zu erstellen.
4. Sie haben jetzt alle ASP.NET Identity erforderlichen Tabellen erstellt, die auf eine MySQL-Datenbank unter Azure gehostet werden, wie unten dargestellt.  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image1.jpg)

## <a name="create-an-mvc-application-project-from-template-and-configure-it-to-use-mysql-provider"></a>Erstellen Sie ein MVC-Anwendungsprojekt aus Vorlage und konfigurieren Sie, um MySQL-Anbieter zu verwenden.

Installieren Sie bei Bedarf entweder [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) mit Update 2.

### <a name="download-the-aspnetidentitymysql-project-from-codeplex"></a>Laden Sie das Projekt ASP.NET.Identity.MySQL von CodePlex herunter

1. Navigieren Sie zu der Repository-URL auf [AspNet.Identity.MySQL (CodePlex)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/).
2. Den Quellcode herunterladen.
3. Extrahieren Sie die ZIP-Datei in einen lokalen Ordner.
4. Öffnen Sie die Projektmappe AspNet.Identity.MySQL, und erstellen Sie sie.

### <a name="create-a-new-mvc-application-project-from-template"></a>Ein neues MVC-Anwendungsprojekt aus Vorlage erstellen

1. Klicken Sie mit der rechten Maustaste auf die **AspNet.Identity.MySQL** Lösung und **hinzufügen**, **neues Projekt**
2. In der **neues Projekt hinzufügen** Dialogfeld wählen **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**. Namen für das Projekt **IdentityMySQLDemo**; und klicken Sie dann auf OK.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image2.jpg)
3. In der **neues ASP.NET-Projekt** Dialogfeld Wählen Sie die MVC-Vorlage mit den Standardoptionen (enthält **einzelne Benutzerkonten** als Authentifizierungsmethode), und klicken Sie auf **OK** .![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image3.jpg)
4. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des IdentityMySQLDemo-Projekts, und wählen **NuGet-Pakete verwalten**. Geben Sie im Textfeld "Suchen" im Dialogfeld **Identity.EntityFramework**. Wählen Sie dieses Paket in der Liste der Ergebnisse, und klicken Sie auf **Deinstallieren**. Sie werden aufgefordert, die zum Deinstallieren des abhängigkeitspakets EntityFramework dar. Klicken Sie auf Ja, wie wir nicht mehr auf dieses Paket auf diese Anwendung wird.
5. Klicken Sie mit der mit der rechten Maustaste auf das Projekt IdentityMySQLDemo, wählen Sie **hinzufügen**, **Verweis, Projektmappen, Projekte;** wählen Sie das AspNet.Identity.MySQL-Projekt, und klicken Sie auf **OK**.
6. Ersetzen Sie alle Verweise auf IdentityMySQLDemo im Projekt  
    `using Microsoft.AspNet.Identity.EntityFramework;`  
   durch  
     `using AspNet.Identity.MySQL;`
7. Legen Sie im IdentityModels.cs, **ApplicationDbContext** Ableitung **MySqlDatabase** und umfassen einen Konstruktor, der einen einzelnen Parameter mit dem Verbindungsnamen.  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample1.cs)]
8. Öffnen Sie die IdentityConfig.cs-Datei. In der **ApplicationUserManager.Create** -Methode, instanziieren UserManager durch den folgenden Code ersetzen:  

    [!code-csharp[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample2.cs)]
9. Öffnen Sie die Datei "Web.config", und Ersetzen Sie die Zeichenfolge DefaultConnection durch diesen Eintrag, und Ersetzen Sie dabei die hervorgehobenen Werte mit der Verbindungszeichenfolge der MySQL-Datenbank, die Sie auf den vorherigen Schritten erstellt haben:  

    [!code-xml[Main](implementing-a-custom-mysql-aspnet-identity-storage-provider/samples/sample3.xml?highlight=2)]

## <a name="run-the-app-and-connect-to-the-mysql-db"></a>Führen Sie die app und Verbinden mit der MySQL-Datenbank

1. Klicken Sie mit der rechten Maustaste auf die **IdentityMySQLDemo** Projekt, und wählen Sie **als Startprojekt festlegen**
2. Drücken Sie **STRG + F5** erstellen und Ausführen der app.
3. Klicken Sie auf **registrieren** Registerkarte oben auf der Seite.
4. Geben Sie einen neuen Benutzernamen und ein Kennwort, und klicken Sie dann auf **registrieren**.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image8.png)
5. Der neue Benutzer ist nun registriert und angemeldet.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image9.png)
6. Wechseln Sie zurück zu den MySQL-Workbench-Tool, und Überprüfen der **IdentityMySQLDatabase** des Inhaltsverzeichnisses. Überprüfen Sie die Users-Tabelle nach Einträgen wie Sie neue Benutzer registrieren.  
  
    ![](implementing-a-custom-mysql-aspnet-identity-storage-provider/_static/image10.png)

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Aktivieren der anderen Authentifizierungsmethoden auf diese app finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).

Wie die DB OAuth integriert und Rollen einrichten, um Benutzern den Zugriff auf Ihre app beschränken finden Sie unter [eine Secure ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereitstellen](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).
