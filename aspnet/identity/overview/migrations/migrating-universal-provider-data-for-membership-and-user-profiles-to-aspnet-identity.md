---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrieren von Daten eines universellen Anbieters für Mitgliedschaften und Benutzerprofilen nach ASP.NET Identity (c#) | Microsoft-Dokumentation
author: rustd
description: In diesem Tutorial wird beschrieben, die erforderlichen Schritte zum Migrieren von Benutzer und Daten der Rolle und Benutzerprofildaten, die mit der Universal Providers einer vorhandenen App erstellt wird...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 6115b2a6caca05659f1c35ce97954807a6fb01ae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836806"
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrieren von Daten eines universellen Anbieters für Mitgliedschaften und Benutzerprofilen nach ASP.NET Identity (c#)
====================
durch [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> In diesem Tutorial wird beschrieben, die erforderlichen Schritte zum Migrieren von Benutzer und Daten der Rolle und Benutzerprofildaten, die mit der Universal Providers einer vorhandenen Anwendung für das ASP.NET Identity-Modell erstellt wird. Der Ansatz wird hier erwähnt, zum Migrieren von Benutzerprofildaten, die in einer Anwendung mit einer SQL-Mitgliedschaft auch verwendet werden können.


Mit der Veröffentlichung von Visual Studio 2013, das ASP.NET-Team ein neues ASP.NET Identity-System eingeführt, und erfahren Sie mehr über diese Version [hier](../../index.md). Anschluss an die im Artikel zum Migrieren von Webanwendungen vor [SQL-Mitgliedschaft in der neuen Identitätssystem](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), dieser Artikel beschreibt die Schritte zur Migration vorhandener Anwendungen, die Folgen des Anbieter-Modells für Benutzer-und Rollenverwaltung das neue Identitätsmodell. Der Schwerpunkt in diesem Tutorial werden in erster Linie auf die Migration von der Benutzerprofildaten, um nahtlos in das neue System zu verknüpfen. Migrieren von Informationen für Benutzer und Rollen ist ähnlich für die SQL-Mitgliedschaft. Der Ansatz befolgt, um die Profildaten zu migrieren, kann in einer Anwendung mit SQL-Mitgliedschaft auch verwendet werden.

Als Beispiel beginnen wir mit einer Web-app erstellt haben, mithilfe von Visual Studio 2012 die das Anbieter-Modell verwendet. Wir werden dann fügen Sie Code für die Verwaltung von Profilen, registrieren Sie einen Benutzer, Profildaten für den Benutzer hinzufügen, migrieren das Datenbankschema und ändern Sie dann die Anwendung das Identitätssystem für Benutzer-und Rollenverwaltung zu verwenden. Zum Testen der Migration erstellt, mit der Universal Providers Benutzer sollten in der Lage, melden Sie sich, und neue Benutzer sollten in der Lage, zu registrieren.

> [!NOTE]
> Sie finden das vollständige Beispiel am [ https://github.com/suhasj/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations).


## <a name="profile-data-migration-summary"></a>Data Migration-Zusammenfassung

Bevor Sie beginnen mit den Migrationen, wir sehen uns nun der Ablauf der Speichern von Profildaten in die Anbieter-Modell. Profildaten für die Anwendung aus, die Benutzern auf verschiedene Weisen gespeichert werden können, die am häufigsten verwendeten untereinander wird mithilfe von integrierten Profilanbieter zusammen mit der Universal Providers ausgeliefert. Die Schritte sind.

1. Fügen Sie eine Klasse, die Eigenschaften, die zum Speichern von Profildaten verwendet hat.
2. Fügen Sie eine Klasse, die 'ProfileBase' erweitert und implementiert Methoden, um die oben genannten Profildaten für den Benutzer zu erhalten.
3. Aktivieren mithilfe von Standard-Profilanbieter in die *"Web.config"* Datei, und definieren Sie die Klasse, die in Schritt #2 verwendet werden, den Zugriff auf Informationen zum Eigenschaftsprofil deklariert.

Die Profilinformationen wird als serialisierte Xml und binäre Daten in der Tabelle "Profile" in der Datenbank gespeichert.

Nach der Migration der Anwendung zur Verwendung der neuen ASP.NET Identity-Systems an, wird die Profilinformationen deserialisiert und als Eigenschaften auf der Klasse gespeichert. Jede Eigenschaft können Sie dann auf Spalten in der Benutzertabelle zugeordnet werden. Dies hat den Vorteil, dass die Eigenschaften bearbeitet werden können, auf die Klasse in Ergänzung nicht zu serialisieren/Deserialisieren von Dateninformationen direkt mit Uhrzeit darauf zugreift.

## <a name="getting-started"></a>Erste Schritte

1. Erstellen Sie eine neue ASP.NET 4.5 Web Forms-Anwendung in Visual Studio 2012. Das aktuelle Beispiel verwendet die Web Forms-Vorlage, aber Sie können auch MVC-Anwendung verwenden.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Erstellen Sie einen neuen Ordner "Modelle" zum Speichern von Profilinformationen  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Beispielsweise können wir kein speichern Sie Datum Geburtsdatum, City, Höhe und die Gewichtung des Benutzers im Profil. Die Höhe und Breite werden als eine benutzerdefinierte Klasse namens "PersonalStats" gespeichert. Speichern und rufen Sie das Profil, benötigen wir eine Klasse, die "ProfileBase" Erweitert. Wir erstellen eine neue Klasse "AppProfile" abrufen und speichern Profilinformationen.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Aktivieren des Profils in der *"Web.config"* Datei. Geben Sie den Klassennamen an, die verwendet werden, um Speicher/Abrufen von Benutzerinformationen, die in Schritt #3 erstellt haben.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Fügen Sie eine Web Forms-Seite im Ordner "'Account'" auf den Profildaten des Benutzers abrufen und speichern Sie sie hinzu. Klicken Sie mit der rechten Maustaste auf Projekt, und wählen Sie "Neues Element hinzufügen". Fügen Sie eine neue Web Forms-Seite mit der Masterseite "AddProfileData.aspx". Kopieren Sie im Abschnitt "MainContent" Folgendes ein:

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Fügen Sie den folgenden Code im CodeBehind:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Fügen Sie den Namespace, die unter der, den appprofile Klasse definiert ist, um Kompilierungsfehler zu entfernen.
6. Führen Sie die app, und erstellen Sie einen neuen Benutzer mit dem Benutzernamen "**Olduser".** Navigieren Sie zur Seite "AddProfileData", und fügen Sie Profilinformationen für den Benutzer hinzu.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Sie können überprüfen, ob die Daten im serialisierten XML-Format in der Tabelle "Profile", die mit dem Server-Explorer-Fenster gespeichert werden. Wählen Sie in Visual Studio im Menü 'Ansicht' "Server-Explorer" ein. Muss eine Datenverbindung für die Datenbank definiert, der *"Web.config"* Datei. Durch Klicken auf die Datenverbindung zeigt die verschiedenen Unterkategorien. Erweitern Sie "Tabellen" Anzeigen der verschiedenen Tabellen in der Datenbank, und klicken Sie mit der rechten Maustaste auf 'Profile' aus, und wählen "Tabellendaten anzeigen", zum Anzeigen der Profildaten, die in der Profile-Tabelle gespeichert.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migrieren des Datenbankschemas

Damit wird die vorhandene Datenbank mit der Identity-System arbeiten, müssen Sie das Schema in der Datenbank Identität, um die Felder zu unterstützen, die wir, mit der ursprünglichen Datenbank hinzugefügt aktualisieren. Dies kann erfolgen mithilfe von SQL-Skripts zum Erstellen neuer Tabellen und kopieren Sie die vorhandene Informationen. Erweitern Sie im Fenster "Server-Explorer" "DefaultConnection", um die Tabellen anzuzeigen. Klicken Sie mit der rechten Maustaste auf die Tabellen aus, und wählen Sie "Neue Abfrage"

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Fügen Sie das SQL-Skript aus [ https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt ](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) und führen Sie sie. Wenn die "DefaultConnection" aktualisiert wird, sehen wir, dass die neuen Tabellen hinzugefügt werden. Sie können überprüfen, dass die Daten in den Tabellen zu erkennen, dass die Informationen migriert wurde.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migrieren der Anwendung zur Verwendung von ASP.NET Identity

1. Installieren Sie die Nuget-Pakete, die für ASP.NET Identity erforderlich sind:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Weitere Informationen zum Verwalten von Nuget-Pakete finden Sie [hier](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Um mit vorhandenen Daten in der Tabelle arbeiten zu können, müssen wir ViewModel-Klassen zu erstellen, die an die Tabellen zugeordnet, und verknüpfen sie in das Identitätssystem. Als Teil des Vertrags Identität die Modellklassen sollten entweder die Schnittstellen in der Dll Identity.Core implementieren oder erweitern können, die vorhandene Implementierung dieser Schnittstellen in "Microsoft.Aspnet.Identity.EntityFramework" verfügbar. Wir werden die vorhandenen Klassen für die Rolle, die benutzeranmeldungen und die Ansprüche des Benutzers verwenden. Wir müssen einen eigenen Benutzer für unser Beispiel zu verwenden. Klicken Sie mit der rechten Maustaste auf das Projekt, und erstellen Sie neuen Ordner "IdentityModels". Fügen Sie eine neue 'User'-Klasse, wie unten dargestellt:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Beachten Sie, dass die "ProfileInfo" nun eine Eigenschaft für die Klasse ist. Daher können wir die User-Klasse verwenden, direkt mit Profildaten zu arbeiten.

Kopieren Sie die Dateien in die **IdentityModels** und **IdentityAccount** Ordner aus der Quelle herunterladen ( [ https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations ](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Sie verfügen über die verbleibenden Modellklassen und die neuen Seiten, die für Benutzer- und Rollenverwaltung mithilfe der ASP.NET Identity-APIs benötigt. Der verwendete Ansatz ähnelt der SQL-Mitgliedschaft und finden Sie ausführliche erläuterungen [hier](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Kopieren von Profildaten in die neuen Tabellen

Wie bereits erwähnt, müssen wir deserialisiert die XML-Daten in den Tabellen der Profile aus, und speichern Sie sie in den Spalten der Tabelle "aspnetusers". Die neuen Spalten wurden in der Tabelle im vorherigen Schritt erstellt, also nur noch die Spalten mit den erforderlichen Daten zu füllen. Zu diesem Zweck verwenden wir eine Konsolenanwendung das einmal zum Auffüllen der neu erstellten Spalten in der Tabelle ausgeführt wird.

1. Erstellen Sie eine neue Konsolenanwendung in der vorhandenen Projektmappe ein.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Installieren Sie die neueste Version des Entity Framework-Pakets.
3. Fügen Sie die Webanwendung, die als Verweis auf die Konsolenanwendung zuvor erstellte hinzu. Hierzu dies mit der rechten Maustaste auf das Projekt, und klicken Sie dann "Verweise hinzufügen", klicken Sie dann die Projektmappe, klicken Sie auf das Projekt, und klicken Sie auf OK.
4. Kopieren der folgenden Code in der Datei "Program.cs"-Klasse. Diese Logik liest Profildaten für jeden Benutzer, serialisiert es als 'ProfileInfo'-Objekt und speichert sie in der Datenbank.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Einige der verwendeten Modelle werden im Ordner "IdentityModels" das Webanwendungsprojekt, definiert, daher müssen Sie die zugehörigen Namespaces einschließen.
5. Der obige Code funktioniert, auf die Datenbankdatei in der App\_Ordner das Webanwendungsprojekt "Data", die in den vorherigen Schritten erstellt haben. Aktualisieren Sie die Verbindungszeichenfolge in der Datei "App.config" der Konsolenanwendung, die auf, durch die Verbindungszeichenfolge in der Datei web.config der Webanwendung. Geben Sie auch des vollständigen physischen Pfads, in der Eigenschaft "AttachDbFilename" ein.
6. Öffnen Sie eine Eingabeaufforderung, und navigieren Sie zu dem Ordner "Bin" der oben genannten Konsolenanwendung. Führen Sie die ausführbare Datei, und überprüfen Sie die Ausgabe des Protokolls, wie in der folgenden Abbildung dargestellt.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Öffnen Sie in der Tabelle "AspNetUsers" im Server-Explorer aus, und überprüfen Sie die Daten in die neuen Spalten, die die Eigenschaften enthalten. Sie sollten mit der entsprechenden Eigenschaftswerte aktualisiert werden.

## <a name="verify-functionality"></a>Überprüfen der Funktionalität

Verwenden Sie die neu hinzugefügte Mitgliedschaftsseiten, die mithilfe von ASP.NET Identity Anmelden ein Benutzers aus der alten Datenbank implementiert werden. Der Benutzer sollte mit den gleichen Anmeldeinformationen anmelden können. Die anderen Funktionen wie das Hinzufügen von OAuth, Erstellen eines neuen Benutzers ein, das Ändern des Kennworts, Hinzufügen von Rollen, versuchen Sie es Hinzufügen von Benutzern zu Rollen usw.

Die Profildaten für den alten Benutzer und die neuen Benutzer sollten abgerufen und in der Tabelle gespeichert werden. Die alte Tabelle darf nicht mehr verwiesen werden.

## <a name="conclusion"></a>Schlussbemerkung

Der Artikel wurde beschrieben, den Prozess der Migration von Webanwendungen, die das Anbietermodell für die Mitgliedschaft in ASP.NET Identity verwendet wird. Im Artikel beschriebenen darüber Migrieren von Profildaten für Benutzer in das Identitätssystem eingebunden werden soll. Hinterlassen Sie Kommentare unten für Fragen und Probleme bei der Migration Ihrer app.

*Vielen Dank Rick Anderson und Robert McMurray für die im Artikel.*
