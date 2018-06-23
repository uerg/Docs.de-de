---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrieren von universellen Anbieterdaten Mitgliedschaft und Benutzerprofilen zu ASP.NET Identity (c#) | Microsoft Docs
author: rustd
description: In diesem Lernprogramm werden die Schritte beschrieben, die erforderlich sind, um Benutzer und Daten der Rolle und erstellt mithilfe einer vorhandenen App Universal Providers Benutzerprofildaten migrieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 75d273d9fdb5d8ff0f7a910f42abe8bcce6e397d
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2018
ms.locfileid: "36313999"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrieren von universellen Anbieterdaten Mitgliedschaft und Benutzerprofilen zu ASP.NET Identity (c#)
====================
durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> In diesem Lernprogramm wird beschrieben, die zum Migrieren von Benutzer- und Daten der Rolle und Benutzerprofildaten, die mithilfe von einer vorhandenen Anwendung und das Identitätsmodell ASP.NET Universal Providers erstellt werden. Der Ansatz wird hier genannten, in einer Anwendung mit SQL-Mitgliedschaft sowie Informationen zum Migrieren von Benutzerprofildaten verwendet werden können.


Mit der Version von Visual Studio 2013, das ASP.NET-Team ein neues ASP.NET Identity-System eingeführt, und erfahren Sie mehr über diese Version [hier](../../index.md). Befolgen Sie für den Artikel zum Migrieren von Webanwendungen aus [SQL Mitgliedschaft auf die neue Identitätssystem](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), in diesem Artikel werden die Schritte zum Migrieren von vorhandener Anwendungen, die das Anbieter-Modell für die Verwaltung von Benutzern und Rollen folgen veranschaulicht. um das neue Identitätsmodell. Der Schwerpunkt dieses Lernprogramms werden in erster Linie auf die Migration der Benutzerprofildaten es nahtlos in das neue System zu verknüpfen. Migrieren von Informationen für Benutzer und Rollen ist für die Mitgliedschaft in SQL vergleichbar. Die Vorgehensweise zum Migrieren von Profildaten gefolgt kann in einer Anwendung mit SQL-Mitgliedschaft sowie verwendet werden.

Als Beispiel wird zunächst mit einer Web-app mit Visual Studio 2012 verwendet das Anbieter-Modell erstellt. Wir müssen dann fügen Sie Code für die Verwaltung von Profilen, einen Benutzer registrieren, Profildaten für den Benutzer, migrieren das Datenbankschema und ändern Sie die Anwendung das Identitätssystem für die Verwaltung von Benutzern und Rollen zu verwenden. Migration zu testen Benutzer erstellt wurden, mithilfe der Universal Providers sollte in der Lage, melden Sie sich, und neue Benutzer sollten in der Lage, zu registrieren.

> [!NOTE]
> Sie finden das vollständige Beispiel am [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Zusammenfassung von Daten-migration

Vor dem Beginn der Migration, betrachten Sie wir die Erfahrung des Speicherns von Profildaten im Anbieter-Modell. Profildaten für die Anwendung, die Benutzer auf verschiedene Weise gespeichert werden können, die am häufigsten verwendeten zwischen ihnen wird Verwendung des integrierte Profilanbieter zusammen mit dem Universal Providers geliefert. Die Schritte umfassen würde

1. Fügen Sie eine Klasse, die Eigenschaften, die zum Speichern von Profildaten verwendet wurde.
2. Fügen Sie eine Klasse, die 'ProfileBase' erweitert und implementiert die Methoden zum Abrufen der oben genannten Profildaten für den Benutzer hinzu.
3. Die Aktivierung mit Profil Standardanbieter in der *"Web.config"* Datei, und definieren Sie die Klasse, die in Schritt #2 verwendet werden, beim Zugriff auf die Profilinformationen deklariert.

Die Profilinformationen wird als serialisierten XML-Code und die binären Daten in der Tabelle "Profile" in der Datenbank gespeichert.

Nach der Migration der Anwendung zur Verwendung der neuen ASP.NET Identity-Systems an, die Profilinformationen deserialisiert und als Eigenschaften auf der Klasse gespeichert. Jede Eigenschaft können Sie dann auf Spalten in der Benutzertabelle zugeordnet werden. Hat der Vorteil ist, dass die Eigenschaften bearbeitet werden können, direkt in die Klasse in Ergänzung nicht auf die Serialisierung/Deserialisierung der Dateninformationen Zeit bei darauf zugreift.

## <a name="getting-started"></a>Erste Schritte

1. Erstellen Sie eine neue ASP.NET 4.5 Web Forms-Anwendung in Visual Studio 2012. Im aktuellen Beispiel verwendet die Web Forms-Vorlage, jedoch können Sie MVC-Anwendung auch.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Erstellen Sie einen neuen Ordner "Modelle", um Profilinformationen zu speichern.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Beispielsweise können wir kein speichern Sie Datum Geburtsdatum, City, Höhe und die Gewichtung des Benutzers im Profil. Die Höhe und Breite werden als eine benutzerdefinierte Klasse mit dem Namen "PersonalStats" gespeichert. Zum Speichern und Abrufen des Profils, benötigen wir eine Klasse, die "ProfileBase" Erweitert. Erstellen wir eine neue Klasse "AppProfile" abgerufen und Profilinformationen zu speichern.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Aktivieren des Profils in der *"Web.config"* Datei. Geben Sie den Namen der Klasse, die verwendet werden, um den Store/Abrufen von Benutzerinformationen in Schritt #3 erstellt haben.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Fügen Sie im Ordner "Account", die Profildaten aus der Benutzer abrufen und speichern Sie sie einer Web Forms-Seite hinzu. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie "Neues Element hinzufügen". Fügen Sie eine neue Web Forms-Seite mit Gestaltungsvorlage "AddProfileData.aspx". Kopieren Sie im Abschnitt "MainContent" Folgendes ein:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Fügen Sie im Code hinter den folgenden Code hinzu:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Fügen Sie den Namespace, die unter der, den appprofile Klasse definiert ist, um Kompilierungsfehler zu entfernen.
6. Führen Sie die app, und erstellen Sie einen neuen Benutzer mit dem Benutzernamen "**Olduser".** Navigieren Sie zur Seite "AddProfileData", und fügen Sie Profilinformationen für den Benutzer.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Sie können überprüfen, ob die Daten im serialisierten XML-Format in der Tabelle "Profile" mit dem Server-Explorer-Fenster gespeichert werden. Wählen Sie in Visual Studio im Menü "Anzeige", "Server-Explorer" ein. Muss eine Datenverbindung für die Datenbank definiert, der *"Web.config"* Datei. Durch Klicken auf die Datenverbindung zeigt verschiedene Unterkategorien. Erweitern Sie "Tabellen", um die verschiedenen Tabellen in der Datenbank anzeigen und dann klicken Sie mit der rechten Maustaste auf 'Profile' aus, und wählen "Tabellendaten anzeigen", die in der Tabelle Profile gespeicherten Profildaten anzeigen.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrieren des Datenbankschemas

Um die vorhandene Datenbank, die Arbeit mit der Identität des Systems zu machen, müssen Sie das Schema in der Datenbank Identität, um die Felder unterstützt werden, die wir, die ursprüngliche Datenbank hinzugefügt aktualisieren. Dies kann mithilfe von SQL-Skripts zum Erstellen neuer Tabellen, und kopieren Sie die vorhandene Informationen. Erweitern Sie im Fenster "Server-Explorer" "DefaultConnection", um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf die Tabellen, und wählen Sie "Neue Abfrage"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Fügen Sie das SQL-Skript aus [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) und führen Sie sie. Wenn die "DefaultConnection" aktualisiert wird, sehen wir, dass die neuen Tabellen hinzugefügt werden. Sie können überprüfen, dass die Daten innerhalb der Tabellen aus, um anzuzeigen, dass die Daten migriert wurden.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrieren Sie die Anwendung für die Verwendung von ASP.NET Identity

1. Installieren Sie die NuGet-Pakete, die für ASP.NET Identity erforderlich sind:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Weitere Informationen zum Verwalten von NuGet-Pakete verwendbaren [hier](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Zum Arbeiten mit vorhandenen Daten in der Tabelle müssen wir Modellklassen zu erstellen, das die Tabellen zugeordnet und in das Identitätssystem einbinden. Im Rahmen des Vertrags Identität die Modellklassen sollten entweder in der Dll Identity.Core Schnittstellen implementieren oder können erweitern, die vorhandene Implementierung dieser Schnittstellen in Microsoft.AspNet.Identity.EntityFramework verfügbar. Wir wird die vorhandenen Klassen für Rollen, Benutzernamen und Benutzeransprüche verwenden. Wir müssen einen eigenen Benutzer für das Beispiel zu verwenden. Klicken Sie mit der rechten Maustaste auf das Projekt, und erstellen Sie neue Ordner "IdentityModels". Fügen Sie eine neue 'User'-Klasse hinzu, wie unten dargestellt:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Beachten Sie, dass die "ProfileInfo" jetzt eine Eigenschaft für die Klasse ist. Daher können wir die Klasse verwenden, direkt mit Profildaten arbeiten.

Kopieren Sie die Dateien in den **IdentityModels** und **IdentityAccount** Ordner aus der Downloadquelle ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Sie verfügen über die verbleibenden Modellklassen und die neue Seiten, die für Benutzer- und Rollenverwaltung mithilfe der ASP.NET Identity-APIs benötigt. Ansatz wird die Mitgliedschaft in SQL ähnelt und finden Sie ausführliche erläuterungen [hier](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopieren den neuen Tabellen Profildaten

Wie bereits erwähnt, müssen wir deserialisieren Sie die XML-Daten in den Tabellen Profile, und speichern Sie sie in den Spalten der Tabelle AspNetUsers. Die neuen Spalten wurden in der Tabelle "Benutzer" im vorherigen Schritt erstellt, also noch die Spalten mit den erforderlichen Daten zu füllen. Zu diesem Zweck verwenden wir eine Konsolenanwendung das einmal ausgeführt wird, um die neu erstellten Spalten in der Benutzertabelle aufzufüllen.

1. Erstellen Sie eine neue Konsolenanwendung in der vorhandenen Projektmappe ein.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installieren Sie die neueste Version des Entity Framework-Pakets.
3. Fügen Sie die Webanwendung, die als Verweis auf die Konsolenanwendung zuvor erstellte hinzu. Klicken Sie hierzu mit der rechten Maustaste auf Projekt, und klicken Sie dann "Verweis hinzufügen", klicken Sie dann die Projektmappe, klicken Sie auf das Projekt, und klicken Sie auf OK.
4. Kopieren der folgenden Code in der Datei "Program.cs"-Klasse. Diese Logik liest Profildaten für jeden Benutzer, als "ProfileInfo"-Objekt serialisiert und speichert es wieder in der Datenbank.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Einige Modelle verwendet werden, damit Sie die entsprechende Namespaces gehören müssen im Ordner "IdentityModels" "" das Webanwendungsprojekt definiert.
5. Der obige Code funktioniert auf die Datenbankdatei in der App\_Datenordner der das Webanwendungsprojekt, die in den vorherigen Schritten erstellt haben. Aktualisieren Sie die Verbindungszeichenfolge in der Datei "App.config" der Konsolenanwendung, die auf verweisen möchten, mit der Verbindungszeichenfolge in der Datei "Web.config" der Webanwendung. Geben Sie auch den vollständigen physischen Pfad in der Eigenschaft "AttachDbFilename" ein.
6. Öffnen Sie ein Eingabeaufforderungsfenster, und navigieren Sie zu dem Ordner "Bin", der oben genannten Konsolenanwendung. Führen Sie die ausführbare Datei, und überprüfen Sie die Ausgabe des Protokolls, wie in der folgenden Abbildung dargestellt.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Öffnen Sie die Tabelle "AspNetUsers" im Server-Explorer, und überprüfen Sie die Daten in die neuen Spalten, die die Eigenschaften enthalten. Sie sollten mit den entsprechenden Eigenschaftswerte aktualisiert werden.

## <a name="verify-functionality"></a>Überprüfen Sie die Funktionalität

Verwenden Sie die neu hinzugefügte Mitgliedschaftsseiten, die mithilfe von ASP.NET Identity bei der Anmeldung eines Benutzers von der alten Datenbank implementiert werden. Der Benutzer sollte mit den gleichen Anmeldeinformationen anmelden können. Hinzufügen von Benutzern zu Rollen usw. andere Funktionen wie das Hinzufügen von OAuth, erstellen einen neuen Benutzer, das Ändern des Kennworts, Hinzufügen von Rollen, versuchen Sie es.

Die Profildaten für den alten Benutzer sowie die neuen Benutzer sollten abgerufen und in der Benutzertabelle gespeichert werden. Die alte Tabelle sollte nicht mehr verwiesen werden.

## <a name="conclusion"></a>Schlussbemerkung

Im Artikel beschrieben, wie Migrieren von Webanwendungen, die das Anbietermodell für die Mitgliedschaft in ASP.NET Identity verwendet. Im Artikel beschriebenen darüber Migrieren von Profildaten für Benutzer in das Identitätssystem eingebunden werden soll. Lassen Sie Kommentare unten für Fragen und Probleme bei der Migration Ihrer app.

*Unser Dank gilt Rick Anderson und Robert McMurray zum Überprüfen des Artikels.*
