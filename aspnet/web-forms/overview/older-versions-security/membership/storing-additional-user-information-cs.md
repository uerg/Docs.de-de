---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Speichern von zusätzliche Benutzerinformationen (c#) | Microsoft Docs
author: rick-anderson
description: In diesem Lernprogramm werden wir diese Frage beantworten, durch das Erstellen einer Anwendung sehr rudimentär Gästebuch. In diesem Fall werden wir verschiedene Optionen für Modeli betrachten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e484f63a82ad9ecf1f376143bdc1924e231e0801
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="storing-additional-user-information-c"></a>Das Speichern von zusätzliche Benutzerinformationen (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> In diesem Lernprogramm werden wir diese Frage beantworten, durch das Erstellen einer Anwendung sehr rudimentär Gästebuch. In diesem Fall werden wir sehen uns unterschiedliche Optionen für die Modellierung von Benutzerinformationen in einer Datenbank und klicken Sie dann Informationen, die Benutzerkonten, die vom Framework Mitgliedschaft erstellt diese Daten zugeordnet werden soll.


## <a name="introduction"></a>Einführung

ASP IST. NET Mitgliedschaft Framework bietet eine flexible Benutzeroberfläche zum Verwalten von Benutzern. Die Mitgliedschaft-API umfasst Methoden für die Anmeldeinformationen überprüft, Abrufen von Informationen zu den derzeit angemeldeten Benutzer, ein neues Benutzerkonto erstellen und Löschen eines Benutzerkontos, unter anderem. Jedes Benutzerkonto in Mitgliedschaft Framework enthält nur die Eigenschaften, die für die Überprüfung von Anmeldeinformationen und wichtige kontobezogene Benutzeraufgaben Ausführen benötigt. Dies wird durch die Methoden und Eigenschaften der entspricht der [ `MembershipUser` Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), das ein Benutzerkonto in das Framework Mitgliedschaft modelliert. Diese Klasse verfügt über Eigenschaften, z. B. [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), und [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), und Methoden wie [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) und [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Anwendungen müssen häufig, nicht in die Mitgliedschaft Framework enthalten zusätzliche Benutzerinformationen zu speichern. Beispielsweise kann ein müssen kann jeder Benutzer speichern ihre Liefer-und Rechnungsadresse, die Zahlungsinformationen, die Einstellungen für die Übermittlung "und" Kontakt Telefonnummer. Darüber hinaus ist jede Bestellung in das System mit einem bestimmten Benutzerkonto verknüpft.

Die `MembershipUser` Klasse enthält keine Eigenschaften, z. B. `PhoneNumber` oder `DeliveryPreferences` oder `PastOrders`. Wie wir überwachen die Benutzerinformationen von der Anwendung benötigt werden und mit Mitgliedschaft Framework integrieren lassen? In diesem Lernprogramm werden wir diese Frage beantworten, durch das Erstellen einer Anwendung sehr rudimentär Gästebuch. In diesem Fall werden wir sehen uns unterschiedliche Optionen für die Modellierung von Benutzerinformationen in einer Datenbank und klicken Sie dann Informationen, die Benutzerkonten, die vom Framework Mitgliedschaft erstellt diese Daten zugeordnet werden soll. Fangen wir an!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Schritt 1: Erstellen der Anwendung Gästebuch Datenmodell

Es gibt eine Vielzahl von Techniken, die zum Erfassen von Benutzerinformationen in einer Datenbank und die Benutzerkonten, die vom Framework Mitgliedschaft erstellt zuordnen verwendet werden kann. Um diese Verfahren zu veranschaulichen, müssen wir die Lernprogramm Web-Anwendung zu erweitern, damit sie irgendeine der benutzerspezifischen Daten erfasst. (Derzeit Datenmodell für die Anwendung enthält, nur die Anwendungsdienste Tabellen erforderlich sind, indem Sie die `SqlMembershipProvider`.)

Erstellen wir eine sehr einfache Gästebuch-Anwendung, in dem ein authentifizierter Benutzer einen Kommentar abzugeben kann. Zusätzlich zum Speichern von Gästebuchkommentaren, ermöglichen Sie wir jedem Benutzer seine home Stadt, die Startseite und die Signatur zu speichern. Wenn angegeben, home Stadt des Benutzers, wird zur Startseite und Signatur für jede Nachricht angezeigt, die er angezeigten verlassen hat.

### <a name="adding-theguestbookcommentstable"></a>Hinzufügen der`GuestbookComments`Tabelle

Um die Gästebuchkommentaren erfassen möchten, müssen wir eine Datenbanktabelle mit dem Namen können `GuestbookComments` Spalten wie dessen `CommentId`, `Subject`, `Body`, und `CommentDate`. Außerdem müssen wir jeden Datensatz der `GuestbookComments` Tabelle Verweis der Benutzer, den Kommentar zu verlassen.

Um die Datenbank in dieser Tabelle hinzugefügt haben, fahren Sie mit der Datenbank-Explorer in Visual Studio und Drilldown in die `SecurityTutorials` Datenbank. Mit der rechten Maustaste auf den Ordner Tabellen, und wählen Sie die neue Tabelle hinzufügen. Daraufhin wird eine Schnittstelle, die zum Definieren der Spalten für die neue Tabelle ermöglicht.


[![Hinzufügen einer neuen Tabelle in der Datenbank SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Tabelle an die `SecurityTutorials` Datenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image3.png))


Als Nächstes definieren Sie die `GuestbookComments`Spalten. Starten Sie durch Hinzufügen einer Spalte mit dem Namen `CommentId` vom Typ `uniqueidentifier`. Diese Spalte wird eindeutig identifizieren jede Kommentar im angezeigten, also zu unterbinden `NULL` s und kennzeichnen Sie es als Primärschlüssel der Tabelle. Anstatt bereitzustellen, das einen Wert für die `CommentId` für jedes Feld `INSERT`, es kann darauf hinweisen, die ein neues `uniqueidentifier` -Wert sollte automatisch generiert für dieses Feld auf `INSERT` durch Festlegen der Standardwert der Spalte auf `NEWID()`. Nach dem Hinzufügen dieses ersten Feld, markieren es als die primary key- und die Einstellungen für den Standardwert sollte der Bildschirm in Abbildung 2 dargestellte Screenshot ähneln.


[![Fügen Sie eine Primärschlüsselspalte mit dem Namen CommentId hinzu.](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Abbildung 2**: Hinzufügen einer primären Spalte mit dem Namen `CommentId` ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image6.png))


Fügen Sie eine Spalte mit dem Namen `Subject` des Typs `nvarchar(50)` und eine Spalte mit dem Namen `Body` des Typs `nvarchar(MAX)`, Deaktivierung `NULL` s in beide Spalten. Danach fügen Sie eine Spalte mit dem Namen `CommentDate` vom Typ `datetime`. Unterbinden `NULL` s und legen die `CommentDate` Standardwert der Spalte `getdate()`.

Übrig bleibt, eine Spalte hinzuzufügen, die ein Benutzerkonto mit jeder Gästebuch Kommentar zuordnet. Eine Option zum Hinzufügen einer Spalte mit dem Namen wäre `UserName` vom Typ `nvarchar(256)`. Dies ist angemessen, wenn mithilfe von einem Mitgliedschaftsanbieter außer der `SqlMembershipProvider`. Aber bei Verwendung der `SqlMembershipProvider`, wie in diesem Lernprogramm Reihe werden die `UserName` Spalte in der `aspnet_Users` Tabelle ist nicht unbedingt eindeutig. Die `aspnet_Users` ist der Primärschlüssel der Tabelle `UserId` und Typ `uniqueidentifier`. Aus diesem Grund die `GuestbookComments` Tabelle benötigt eine Spalte mit dem Namen `UserId` des Typs `uniqueidentifier` (untersagen `NULL` Werte). Fahren Sie fort, und fügen Sie diese Spalte hinzu.

> [!NOTE]
> Wie in erläutert die [ *erstellen das Schema für die Mitgliedschaft in SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) Lernprogramm, das Mitgliedschaft Framework wurde entworfen, um mehrere Webanwendungen unterschiedliche Benutzerkonten, die denselben zu aktivieren Benutzerspeicher. Dies geschieht durch das Partitionieren von Benutzerkonten in verschiedenen Anwendungen. Und zwar alle Benutzernamen innerhalb einer Anwendung eindeutig ist unbedingt, kann der gleiche Benutzername in anderen Anwendungen, die mit den gleichen Speicher des Benutzers verwendet werden. Es ist ein zusammengesetzter `UNIQUE` -Einschränkung in der `aspnet_Users` -Tabelle auf die `UserName` und `ApplicationId` Felder, aber keiner auf nur die `UserName` Feld. Daher ist es möglich, dass das Aspnet\_Tabelle haben Sie zwei (oder mehr) Datensätze mit dem gleichen Benutzer `UserName` Wert. Vorhanden ist, jedoch eine `UNIQUE` -Einschränkung für die `aspnet_Users` tabellenspezifischen `UserId` Feld (da es sich um den Primärschlüssel handelt). Ein `UNIQUE` Einschränkung ist wichtig, da ohne wir eine foreign Key-Einschränkung zwischen herstellen kann die `GuestbookComments` und `aspnet_Users` Tabellen.


Nach dem Hinzufügen der `UserId` Spalte Speichern der Tabelle durch Klicken auf das Symbol "Speichern" in der Symbolleiste. Benennen Sie die neue Tabelle `GuestbookComments`.

Wir haben eine letzte Problem teilnehmen, mit der `GuestbookComments` Tabelle: Wir erstellen müssen eine [foreign Key-Einschränkung](https://msdn.microsoft.com/library/ms175464.aspx) zwischen der `GuestbookComments.UserId` Spalte und die `aspnet_Users.UserId` Spalte. Um dies zu erreichen, klicken Sie auf das Symbol "Beziehung" in der Symbolleiste, um das Dialogfeld Foreign Key Relationships aufzurufen. (Alternativ können Sie dieses Dialogfeld starten, indem im Begriff, das Menü Tabellen-Designer und Beziehungen auswählen.)

Klicken Sie auf die Schaltfläche "hinzufügen" in der unteren linken Ecke des Dialogfelds Foreign Key Relationships. Dadurch wird eine neue foreign Key-Einschränkung, hinzugefügt, obwohl wir noch benötigen, definieren Sie die Tabellen, die einbezogen, in der Beziehung.


[![Verwenden Sie das Dialogfeld Fremdschlüssel-Beziehungen, um eine Tabelle Foreign Key-Einschränkungen zu verwalten](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Abbildung 3**: Verwenden Sie das Dialogfeld Foreign Key-Beziehungen zum Verwalten einer Tabelle Foreign Key-Einschränkungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image9.png))


Klicken Sie dann auf das Symbol "Ellipse" in der Zeile "Tabelle und Spaltenspezifikation" auf der rechten Seite. Hierdurch wird das Dialogfeld Tabellen und Spalten, aus dem können wir an der Primärschlüsseltabelle und die Spalte und die Fremdschlüsselspalte aus der `GuestbookComments` Tabelle. Wählen Sie insbesondere `aspnet_Users` und `UserId` als die Primärschlüsseltabelle und die Spalte, und `UserId` aus der `GuestbookComments` Tabelle als Fremdschlüsselspalte (siehe Abbildung 4). Nach dem Definieren der Primär- und Fremdschlüssel Key Tabellen und Spalten, klicken Sie auf OK, um das Dialogfeld Foreign Key Relationships zurückzukehren.


[![Richten Sie eine Foreign Key-Einschränkung zwischen der Aspnet_Users und GuesbookComments Tabellen](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Abbildung 4**: Einrichten einer Foreign Key-Einschränkung zwischen den `aspnet_Users` und `GuesbookComments` Tabellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image12.png))


An diesem Punkt hat die foreign Key-Einschränkung eingerichtet wurde. Das Vorhandensein dieser Einschränkung wird sichergestellt, dass [relationale Integrität](http://en.wikipedia.org/wiki/Referential_integrity) zwischen den beiden Tabellen und garantiert ist, dass es nie ein Gästebuch-Eintrag verweist auf ein nicht vorhandenes Benutzerkonto an. Eine foreign Key-Einschränkung wird standardmäßig einen übergeordneter Datensatz gelöscht werden soll, wenn der entsprechende untergeordnete Datensätze unterbinden. Also wird ein Benutzer ändert eine oder mehrere Gästebuchkommentaren, und klicken Sie dann versuchen, dieses Benutzerkonto löschen, der Löschvorgang fehl, wenn seine Gästebuchkommentaren zuerst gelöscht werden.

Foreign Key-Einschränkungen können konfiguriert werden, um die zugehörigen untergeordneten Datensätze automatisch zu löschen, wenn ein übergeordneter Datensatz gelöscht wird. Das heißt, können wir diese foreign Key-Einschränkung einrichten, damit ein Benutzer Gästebuch Einträge automatisch gelöscht werden, wenn ihr Benutzerkonto gelöscht wird. Um dies zu erreichen, erweitern Sie den Abschnitt "INSERT und UPDATE-Spezifikation", und legen Sie die Eigenschaft "Regel löschen", um Cascade.


[![Konfigurieren Sie die Foreign Key-Einschränkung auf kaskadierte Löschvorgänge](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Abbildung 5**: Konfigurieren der Foreign Key-Einschränkung auf Löschweitergaben ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image15.png))


Um die foreign Key-Einschränkung zu speichern, klicken Sie auf die Schaltfläche "Schließen", um die Foreign Key Relationships zu beenden. Klicken Sie dann auf das Symbol "Speichern" in der Symbolleiste, um die Tabelle und der this speichern Beziehung.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Speichern des Benutzers Home Örtlichkeit Homepage und Signatur

Die `GuestbookComments` Tabelle veranschaulicht, wie zum Speichern von Informationen, die eine 1: n-Beziehung und Benutzerkonten gemeinsam verwendet wird. Da jedes Benutzerkonto kann eine beliebige Anzahl von zugehörigen Kommentare muss, wird diese Beziehung modelliert, durch das Erstellen einer Tabelle zum Speichern von des Satz von Kommentaren, der eine Spalte enthält, dass jeder Kommentar zurück auf einen bestimmten Benutzer verweist. Bei Verwendung der `SqlMembershipProvider`, diesen Link wird am besten durch Erstellen einer Spalte mit dem Namen eingerichtet `UserId` des Typs `uniqueidentifier` und zwischen dieser Spalte einer foreign Key-Einschränkung und `aspnet_Users.UserId`.

Jetzt müssen wir jedes Benutzerkonto zum Speichern des Benutzers home Örtlichkeit Homepage und Signatur, die in seinem Gästebuchkommentaren angezeigt werden drei Spalten zuordnen. Es gibt Anzahl von Möglichkeiten, dies zu erreichen:

- <strong>Hinzufügen von neuen Spalten mit den</strong><strong>`aspnet_Users`</strong><strong>oder</strong><strong>`aspnet_Membership`</strong><strong>Tabellen.</strong> Ich würde dieser Ansatz nicht empfohlen, da es sich um das von verwendete Schema ändert die `SqlMembershipProvider`. Diese Entscheidung kann zurückkehren, um Sie dem Weg zu entsorgen werden. Was geschieht, wenn eine zukünftige Version von ASP.NET ein anderes verwendet beispielsweise `SqlMembershipProvider` Schema. Microsoft ist eventuell ein Tool zum Migrieren von ASP.NET 2.0 `SqlMembershipProvider` Daten auf das neue Schema, aber wenn Sie die ASP.NET 2.0 vorgenommenen `SqlMembershipProvider` Schema, eine solche Konvertierung möglicherweise nicht möglich.

- **Verwenden von ASP. NET Profil Framework eine Profileigenschaft für home Örtlichkeit, Homepage und Signatur definieren.** ASP.NET umfasst ein Profil-Framework, das entworfen wurde, um zusätzliche benutzerspezifische Daten zu speichern. Wie das Framework Mitgliedschaft wird das Profil Framework Anbietermodell erstellt. Lieferumfang von .NET Framework eine `SqlProfileProvider` abgeschlossenen Profildaten in einer SQL Server-Datenbank gespeichert. Tatsächlich verwendet die Datenbank bereits die verwendete Tabelle der `SqlProfileProvider` (`aspnet_Profile`), wie er hinzugefügt wurde, wenn wir hinzugefügt, sichern die Anwendungsdienste der <a id="_msoanchor_2"> </a> [ *erstellen das Schema für die Mitgliedschaft in SQL Server* ](creating-the-membership-schema-in-sql-server-cs.md) Lernprogramm.   
  Der wichtigste Vorteil von der Profil-Framework ist, dass es Entwicklern, definieren Sie die Profileigenschaften in zulässt `Web.config` – kein Code geschrieben werden, um die Profildaten in und aus den zugrunde liegenden Datenspeicher serialisieren muss. Kurz gesagt, ist es äußerst einfach, eine Reihe von Profileigenschaften definieren und im Code arbeiten. Das Profilsystem hinterlässt jedoch viel wünschenswert sein, bei der versionsverwaltung verwendet, wenn Sie verfügen über eine Anwendung, in denen Sie erwarten, dass neue benutzerspezifische Eigenschaften hinzugefügt werden, bei einem späteren Zeitpunkt oder vorhandene entfernt oder geändert werden, und klicken Sie dann die Profil-Framework möglicherweise nicht, die  beste Option. Darüber hinaus die `SqlProfileProvider` speichert die Profileigenschaften in einer hochgradig denormalisierten Weise, somit unmöglich zum Ausführen von Abfragen direkt gegen die Profildaten (z. B. wie viele Benutzer eine home Örtlichkeit von New York haben).   
  Weitere Informationen über das Profil-Framework finden Sie im Abschnitt "Weitere Messwerte" am Ende dieses Lernprogramms.

- <strong>Fügen Sie diese drei Spalten in eine neue Tabelle in der Datenbank und eine 1: 1 Beziehung zwischen dieser Tabelle herstellen und</strong><strong>`aspnet_Users`</strong><strong>.</strong> Dieser Ansatz umfasst mehr als arbeiten mit dem Profil-Framework, jedoch bietet maximale Flexibilität wie die zusätzlichen Eigenschaften in der Datenbank erstellt werden. Dies ist die Option, die in diesem Lernprogramm verwendet werden.

Wir erstellen eine neue Tabelle namens `UserProfiles` home Örtlichkeit, Homepage und Signatur für jeden Benutzer gespeichert. Mit der rechten Maustaste auf den Ordner Tabellen im Datenbank-Explorer-Fenster, und wählen Sie eine neue Tabelle erstellen. Den Namen der ersten Spalte `UserId` und legen Sie deren Typ auf `uniqueidentifier`. Unterbinden `NULL` Werte und die Spalte als Primärschlüssel kennzeichnen. Als Nächstes fügen Sie Spalten mit dem Namen: `HomeTown` des Typs `nvarchar(50)`; `HomepageUrl` des Typs `nvarchar(100)`; und die Signatur des Typs `nvarchar(500)`. Jede dieser drei Spalten lässt eine `NULL` Wert.


[![Erstellen Sie die Tabelle %Benutzerprofile%](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Abbildung 6**: Erstellen der `UserProfiles` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image18.png))


Speichern Sie die Tabelle, und nennen Sie sie `UserProfiles`. Schließlich einrichten eine foreign Key-Einschränkung zwischen den `UserProfiles` tabellenspezifischen `UserId` Feld und die `aspnet_Users.UserId` Feld. Wie wir mit der foreign Key-Einschränkung zwischen den `GuestbookComments` und `aspnet_Users` Tabellen vorhanden sind, diese Einschränkung kaskadiert werden gelöscht. Da die `UserId` Feld `UserProfiles` ist der primäre Schlüssel, dadurch wird sichergestellt, dass sich nicht mehr als einen Datensatz in der `UserProfiles` Tabelle für jedes Benutzerkonto. Diese Art von Beziehung wird als 1: 1 bezeichnet.

Nachdem wir das Datenmodell erstellt haben, können wir, ihn zu verwenden. In den Schritten 2 und 3 betrachten wir wie der derzeit angemeldete Benutzer anzeigen und ihre home Örtlichkeit Homepage und Signatur-Informationen bearbeiten kann. In Schritt 4 wird die Schnittstelle für den authentifizierten Benutzern das Senden neue Kommentare an angezeigten und zeigen die vorhandenen erstellt.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Schritt 2: Anzeigen des Benutzers Home Örtlichkeit Homepage und Signatur

Es gibt eine Vielzahl von Möglichkeiten zum Zulassen des derzeit angemeldeten Benutzers zum Anzeigen und bearbeiten seine home Örtlichkeit Homepage und Signatur-Informationen. Es konnte die Benutzeroberfläche manuell erstellt, mit dem Textfeld, und Bezeichnungsfeld-Steuerelementen oder es konnte Datentyps Web Controls, z. B. DetailsView-Steuerelement verwenden. Um die Datenbank ausführen `SELECT` und `UPDATE` Anweisungen geschrieben werden, um ADO.NET code in unserer Seite Code-Behind-Klasse, oder alternativ einen deklarativen Ansatz, mit dem SqlDataSource einsetzen. Im Idealfall würde die Anwendung eine mehrstufige Architektur enthalten, die wir von der Seite Code-Behind-Klasse oder deklarativ über das ObjectDataSource-Steuerelement programmgesteuert entweder aufrufen kann.

Da diese Reihe von Lernprogrammen auf formularbasierte Authentifizierung, Autorisierung, Benutzerkonten und Rollen konzentriert, wird es nicht mehr eine ausführliche Beschreibung dieser Zugriffsoptionen unterschiedliche Daten oder warum eine mehrstufige Architektur bevorzugt wird, über das Ausführen von SQL-Anweisungen direkt von der ASP.NET-Seite. Ich werde exemplarisch erklärt, mit einem DetailsView und SqlDataSource – die schnellste und einfachste Option – erörterten Konzepte können jedoch sicherlich angewendet werden, alternative Zugriffslogik für die Steuerelemente und die von Web. Weitere Informationen zum Arbeiten mit Daten in ASP.NET finden Sie in meinem * [arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md) * Reihe von Lernprogrammen.

Öffnen der `AdditionalUserInfo.aspx` auf der Seite der `Membership` Ordner und Hinzufügen eines DetailsView-Steuerelements auf der Seite festlegen seiner `ID` Eigenschaft `UserProfile` und gelöscht wird, seine `Width` und `Height` Eigenschaften. Erweitern Sie die DetailsView Smarttag, und binden Sie es an eine neue Datenquellen-Steuerelements. Hierdurch wird der DataSource-Konfigurations-Assistent (siehe Abbildung 7). Der erste Schritt aufgefordert, den Datenquellentyp angeben. Da wir werden direkt an die Verbindung der `SecurityTutorials` Datenbank, wählen Sie das Datenbanksymbol angeben der `ID` als `UserProfileDataSource`.


[![Fügen Sie ein neues SqlDataSource-Steuerelement mit dem Namen UserProfileDataSource hinzu.](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Abbildung 7**: Hinzufügen einer neuen SqlDataSource-Steuerelement mit dem Namen `UserProfileDataSource` ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image21.png))


Der nächste Bildschirm fordert für die Datenbank verwendet. Wir haben bereits eine Verbindungszeichenfolge im definiert `Web.config` für die `SecurityTutorials` Datenbank. Diese Verbindungszeichenfolgenname – `SecurityTutorialsConnectionString` – sollte sich in der Dropdown-Liste. Wählen Sie diese Option aus, und klicken Sie auf Weiter.


[![Wählen Sie SecurityTutorialsConnectionString aus der Dropdown Liste](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Abbildung 8**: Wählen Sie `SecurityTutorialsConnectionString` aus der Dropdown-Liste ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image24.png))


Im folgenden Bildschirm werden Sie gefragt, geben Sie die Tabelle und die Spalten aus, die Abfrage. Wählen Sie die `UserProfiles` Tabelle aus der Dropdown-Liste, und überprüfen Sie alle Spalten.


[![Schalten Sie alle Spalten aus der Tabelle %Benutzerprofile% sichern](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Abbildung 9**: Schalten Sie wieder alle Spalten aus der `UserProfiles` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image27.png))


Der aktuellen Abfrage in Abbildung 9 gibt *alle* Datensätze in `UserProfiles`, aber wir nur der derzeit angemeldete Benutzer Datensatz interessiert sind. Hinzufügen einer `WHERE` -Klausel, klicken Sie auf die `WHERE` Schaltfläche, um das Hinzufügen `WHERE` Klausel Dialogfeld (siehe Abbildung 10). Hier können Sie die Spalte, nach dem gefiltert, den Operator und die Quelle des Parameters Filter auswählen. Wählen Sie `UserId` als Spalte und den Operator "=".

Leider besteht keine integrierte Parameterquelle zurückzugebenden des aktuell angemeldeten Benutzers `UserId` Wert. Wir müssen diesen Wert programmgesteuert zu ziehen. Deshalb Festlegen der Quellliste Dropdown-Menü aus auf "Keine" auf Hinzufügen Schaltfläche, um die Parameter hinzuzufügen, und klicken Sie dann auf OK.


[![Fügen Sie einen Filterparameter für die Benutzer-ID-Spalte](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Abbildung 10**: Fügen Sie einen Filter-Parameter auf die `UserId` Spalte ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image30.png))


Nach dem Klicken auf OK werden Sie auf dem Bildschirm in Abbildung 9 gezeigt zurückgegeben. Dieses Mal jedoch die SQL-Abfrage am unteren Rand des Bildschirms sollte enthalten eine `WHERE` Klausel. Klicken Sie auf "Weiter", um zum Bildschirm "Testabfrage" zu navigieren, auf. Hier können Sie die Abfrage ausführen und die Ergebnisse anzuzeigen. Klicken Sie auf ' Fertig stellen ', um den Assistenten abzuschließen.

Nach Abschluss der DataSource-Konfigurations-Assistent, erstellt Visual Studio die SqlDataSource-Steuerelement, das basierend auf den im Assistenten angegebenen Einstellungen. Darüber hinaus führt es manuell fügt BoundFields, DetailsView für jede Spalte, die der SqlDataSource zurückgegebenes `SelectCommand`. Besteht keine Notwendigkeit zum Anzeigen der `UserId` Feld, DetailsView, da der Benutzer nicht wissen, dieser Wert muss. Sie können dieses Feld direkt von DetailsView-Steuerelement deklarativem Markup entfernen oder durch Klicken auf "Felder bearbeiten" aus seinem Smarttag.

An diesem Punkt sollte deklarativem Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Wir müssen die SqlDataSource-Steuerelement programmgesteuert festlegen `UserId` Parameter für des aktuell angemeldeten Benutzers `UserId` bevor die Daten ausgewählt werden. Dies geschieht durch Erstellen eines ereignishandlers für die ': SqlDataSource `Selecting` Ereignis und das folgende Codebeispiel gibt es:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

Der obige Code gestartet wird, erhalten Sie einen Verweis auf den derzeit angemeldeten Benutzer durch Aufrufen der `Membership` Klasse `GetUser` Methode. Dies gibt eine `MembershipUser` Objekt, dessen `ProviderUserKey` Eigenschaft enthält die `UserId`. Die `UserId` Wert wird anschließend der SqlDataSource zugewiesen `@UserId` Parameter.

> [!NOTE]
> Die `Membership.GetUser()` Methode gibt Informationen zu den derzeit angemeldeten Benutzer zurück. Wenn ein anonymer Benutzer die Seite besucht, wird zurückgegeben, der Wert `null`. In diesem Fall führt dies zu einer `NullReferenceException` in der folgenden Zeile des Codes beim Lesen der `ProviderUserKey` Eigenschaft. Natürlich wir kümmern brauchen `Membership.GetUser()` Zurückgeben einer `null` Wert in der `AdditionalUserInfo.aspx` Seite, da wir URL-Autorisierung in einem vorherigen Lernprogramm so konfiguriert, dass nur authentifizierte Benutzer die ASP.NET-Ressourcen in diesen Ordner zugreifen konnten. Wenn Sie müssen Zugriff auf Informationen über den derzeit angemeldeten Benutzer auf einer Seite, wo der anonymer Zugriff erlaubt ist, vergewissern Sie sich, die einen nicht-`null MembershipUser` vom zurückgegebenen Objekts die `GetUser()` -Methode vor dem verweisen auf seine Eigenschaften.


Wenn Sie besuchen die `AdditionalUserInfo.aspx` Seite über einen Browser werden Sie eine leere Seite angezeigt, da wir noch, der alle Zeilen hinzugefügt haben die `UserProfiles` Tabelle. In Schritt 6 betrachten wir zum Anpassen der CreateUserWizard-Steuerelement, um automatisch eine neue Zeile hinzufügen der `UserProfiles` Tabelle, wenn ein neues Benutzerkonto erstellt wird. Jetzt jedoch müssen wir einen Datensatz in der Tabelle manuell erstellen.

Navigieren Sie zu der Datenbank-Explorer in Visual Studio, und erweitern Sie den Ordner Tabellen. Mit der rechten Maustaste auf die `aspnet_Users` Tabelle und wählen Sie "Tabellendaten anzeigen", um die Datensätze in der Tabelle anzuzeigen, führen Sie dieselben Schritte für die `UserProfiles` Tabelle. Abbildung 11 zeigt diese Ergebnisse, wenn vertikal angeordnet. In der Datenbank zur Zeit bestehen `aspnet_Users` Datensätze für Bruce Fred und Tito, aber keine Datensätze in der `UserProfiles` Tabelle.


[![Der Inhalt der Aspnet_Users und %Benutzerprofile% Tabellen werden angezeigt.](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Abbildung 11**: der Inhalt der `aspnet_Users` und `UserProfiles` Tabellen angezeigt werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image33.png))


Fügen Sie einen neuen Datensatz zu den `UserProfiles` Tabelle manuell eingeben von Werten für die `HomeTown`, `HomepageUrl`, und `Signature` Felder. Die einfachste Möglichkeit, eine gültige get `UserId` Wert in der neuen `UserProfiles` Datensatz ist, wählen Sie die `UserId` Feld aus einem bestimmten Benutzerkonto in der `aspnet_Users` Tabelle kopieren und fügen Sie ihn in die `UserId` Feld `UserProfiles`. Abbildung 12 zeigt die `UserProfiles` Tabelle, nachdem Sie ein neuer Datensatz für Bruce hinzugefügt wurde.


[![Ein Datensatz wurde auf %Benutzerprofile% für Bruce hinzugefügt.](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Abbildung 12**: A-Eintrag wurde hinzugefügt, um `UserProfiles` für Bruce ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image36.png))


Zurück zu den `AdditionalUserInfo.aspx` Seite im Bruce protokolliert. Wie in Abbildung 13 gezeigt, werden die Einstellungen des Bruce angezeigt.


[![Die zurzeit Zugriff auf Benutzer wird gezeigt, seiner Einstellungen](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Abbildung 13**: die zurzeit Zugriff auf Benutzer wird gezeigt, seiner Einstellungen ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image39.png))


> [!NOTE]
> Wechseln Sie im voraus und manuell hinzufügen Datensätze in der `UserProfiles` Tabelle für jeden Mitgliedschaftsbenutzer. In Schritt 6 betrachten wir zum Anpassen der CreateUserWizard-Steuerelement, um automatisch eine neue Zeile hinzufügen der `UserProfiles` Tabelle, wenn ein neues Benutzerkonto erstellt wird.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Schritt 3: Ermöglicht die Benutzer, seinen Home Örtlichkeit Homepage und Signatur bearbeiten

An diesem Punkt den aktuell angemeldeten Benutzers anzeigen kann, ihre Startseite Örtlichkeit Homepage und Signatur-Einstellung, aber sie nicht noch geändert werden können. Wir aktualisieren DetailsView-Steuerelement, damit die Daten bearbeitet werden können.

Im ersten Schritt erforderlich ist, Hinzufügen einer `UpdateCommand` für die ': SqlDataSource angeben der `UPDATE` auszuführende Anweisung und die entsprechenden Parameter. Wählen Sie die SqlDataSource und im Eigenschaftenfenster klicken Sie auf die Schaltfläche mit den Auslassungszeichen neben der UpdateQuery-Eigenschaft, um das Dialogfeld Befehls- und Parameter-Editor zu öffnen. Geben Sie den folgenden `UPDATE` Anweisung in das Textfeld ein:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

Klicken Sie anschließend auf die Schaltfläche "Parameter aktualisieren", die einen Parameter in der SqlDataSource-Steuerelement erstellt werden `UpdateParameters` Auflistung für jeden der Parameter in der `UPDATE` Anweisung. Lassen Sie die Quelle für alle festgelegten Parameter auf None, und klicken Sie auf die Schaltfläche "OK", um das Dialogfeld zu schließen.


[![Geben Sie der SqlDataSource UpdateCommand und UpdateParameters](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Abbildung 14**: Geben Sie der SqlDataSource `UpdateCommand` und `UpdateParameters` ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image42.png))


Aufgrund von Virtual Machine Additions wurde auf SqlDataSource-Steuerelement, DetailsView Steuerelement jetzt bearbeiten unterstützen. Aus der DetailsView Smart Tag das Kontrollkästchen Sie "Bearbeiten aktivieren". Dadurch wird des Steuerelements eine CommandField hinzugefügt `Fields` Auflistung mit seiner `ShowEditButton` -Eigenschaft auf "true" festgelegt. Dies rendert eine Schaltfläche "Bearbeiten", wenn DetailsView im Nur-Lese-Modus und Update angezeigt wird und Schaltflächen "Abbrechen", bei der Anzeige im Bearbeitungsmodus befindet. Anstatt dass der Benutzer "Bearbeiten" klicken, jedoch können schon dem DetailsView Rendern im Zustand "immer bearbeitbaren" durch Festlegen des DetailsView-Steuerelements [ `DefaultMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) auf `Edit`.

Mit diesen Änderungen sollte deklarativem Markup des Steuerelements DetailsView etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Beachten Sie das Hinzufügen der CommandField und die `DefaultMode` Eigenschaft.

Fahren Sie fort, und Testen Sie diese Seite über einen Browser. Wenn Sie mit einem Benutzer besuchen, die einen entsprechenden Datensatz in `UserProfiles`, Einstellungen des Benutzers werden in einer bearbeitbaren Schnittstelle angezeigt.


[![DetailsView rendert eine bearbeitbare-Schnittstelle](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Abbildung 15**: DetailsView rendert eine bearbeitbare Schnittstelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image45.png))


Wiederholen Sie dann die Werte ändern, und klicken Sie auf die Schaltfläche "Aktualisieren". Es wird angezeigt, als ob nichts geschieht. Es wird ein Postback und die Werte in der Datenbank gespeichert sind, aber es existiert keine visuelle Bestätigung, die der Speichervorgang aufgetreten sind.

Zur Behebung des Problems, zurück zu Visual Studio, und fügen Sie ein Bezeichnungsfeld-Steuerelement oben DetailsView hinzu. Festlegen seiner `ID` auf `SettingsUpdatedMessage`, dessen `Text` Eigenschaft "haben die Einstellungen aktualisiert wurden," und die zugehörige `Visible` und `EnableViewState` Eigenschaften `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Wir müssen zum Anzeigen der `SettingsUpdatedMessage` bezeichnen, sobald die DetailsView aktualisiert wird. Um dies zu erreichen, erstellen Sie einen Ereignishandler für die DetailsView `ItemUpdated` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Zurück zu den `AdditionalUserInfo.aspx` Seite über einen Browser, und aktualisieren Sie die Daten. Diesmal ist eine hilfreiche Statusmeldung angezeigt.


[![Eine kurze Nachricht wird angezeigt, wenn die Einstellungen werden aktualisiert.](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Abbildung 16**: eine kurze Nachricht wird angezeigt, wenn die Einstellungen aktualisiert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image48.png))


> [!NOTE]
> DetailsView-Steuerelement des Schnittstelle bewirkt, dass viel zu wünschen übrig bearbeiten. Standardgröße Textfelder verwendet, aber das Signaturfeld muss wahrscheinlich ein mehrzeiliges Textfeld. Ein RegularExpressionValidator sollte verwendet werden, um sicherzustellen, dass die URL zur Startseite Wenn eingegeben haben, mit "http://" oder "https://" beginnt. Darüber hinaus seit DetailsView-Steuerelement seine `DefaultMode` -Eigenschaftensatz auf `Edit`, die Schaltfläche "Abbrechen" ist keine Aktion ausgeführt. Es sollten entweder entfernt oder, wenn angeklickt, leiten Sie den Benutzer zu einigen anderen Seite (z. B. `~/Default.aspx`). Ich lassen diese Verbesserungen als Übung für den Leser.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Beim Hinzufügen einer Verknüpfung auf der`AdditionalUserInfo.aspx`Seite im Master

Die Website bietet derzeit keine Links zu den `AdditionalUserInfo.aspx` Seite. Die einzige Möglichkeit, die es erreicht ist, geben Sie die Seiten-URL direkt in die Adressleiste des Browsers angezeigt. Fügen Sie einen Link zu dieser Seite in der `Site.master` Masterseite.

Beachten Sie, dass die Gestaltungsvorlage ein LoginView-Websteuerelement in enthält die `LoginContent` ContentPlaceHolder, in dem anderes Markup authentifiziert und anonym Besuchern angezeigt. Aktualisieren Sie des LoginView-Steuerelements `LoggedInTemplate` enthalten einen Link zu der `AdditionalUserInfo.aspx` Seite. Nach diesen Änderungen die LoginView sollte deklarativem Markup des Steuerelements etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Beachten Sie das Hinzufügen der `lnkUpdateSettings` Linksteuerelement auf die `LoggedInTemplate`. Mit diesem Link erfüllt können schnell authentifizierte Benutzern springen, zur Seite, um ihren Heim-Örtlichkeit Homepage und Signatur-Einstellungen anzeigen und ändern.

## <a name="step-4-adding-new-guestbook-comments"></a>Schritt 4: Hinzufügen von neuen Gästebuchkommentaren

Die `Guestbook.aspx` Seite ist, auf dem authentifizierten Benutzer angezeigten anzeigen und einen Kommentar können. Beginnen Sie mit dem Erstellen der Schnittstelle, um neue Gästebuchkommentaren hinzuzufügen.

Öffnen der `Guestbook.aspx` Seite in Visual Studio und erstellen Sie eine neue Benutzeroberfläche besteht aus zwei TextBox-Steuerelemente enthalten, von denen eine für den neuen Kommentar Betreff und eine für Text. Legen Sie des ersten Textfeld-Steuerelements `ID` Eigenschaft, um `Subject` und seine `Columns` -Eigenschaft auf 40; legen Sekunde `ID` auf `Body`, dessen `TextMode` auf `MultiLine`, und die zugehörige `Width` und `Rows` Eigenschaften, die "95 %" und 8 bzw.. Um die Benutzeroberfläche zu abzuschließen, fügen Sie eine Schaltfläche-Websteuerelement, der mit dem Namen `PostCommentButton` und legen Sie dessen `Text` -Eigenschaft auf "Your Comment" Post".

Da jeder Gästebuch Kommentar Betreff und den Nachrichtentext erforderlich ist, fügen Sie einen RequiredFieldValidator für jede der Textfelder hinzu. Festlegen der `ValidationGroup` Eigenschaft von diesen Steuerelementen "EnterComment"; ebenso festlegen, die `PostCommentButton` des Steuerelements `ValidationGroup` -Eigenschaft auf "EnterComment". Weitere Informationen zu ASP. NET Validierungssteuerelemente, Auschecken [Formularvalidierung in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [unterzogen, wodurch der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx), und die [Überprüfung Server Steuerelemente Lernprogramm](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) auf [W3Schools](http://www.w3schools.com/).

Deklaratives Markup Ihrer Seite sollte etwa wie folgt aussehen, nach dem Erstellen der Benutzeroberfläche:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Mit der vollständigen Benutzeroberfläche ist unsere nächste Aufgabe zum Einfügen eines neuen Datensatzes in die `GuestbookComments` bei Verwendung der `PostCommentButton` geklickt wird. Dies kann auf vielfältige Weise erreicht werden: können Sie die Schaltfläche ADO.NET-Code schreiben `Click` Ereignishandler; wir können die Seite ein SqlDataSource-Steuerelement hinzugefügt haben, konfigurieren Sie seine `InsertCommand`, und rufen Sie dann seine `Insert` Methode aus der `Click` Ereignis Handler. oder wir erstellen eine mittlere Ebene, die zum Einfügen von neuen Gästebuchkommentaren verantwortlich war, und rufen Sie diese Funktion von der `Click` -Ereignishandler. Da erläutert, mit einem SqlDataSource in Schritt 3, verwenden Sie ADO.NET-Code sehen wir hier.

> [!NOTE]
> Die programmgesteuerte Zugriff auf Daten aus einer Microsoft SQL Server-Datenbank verwendeten ADO.NET-Klassen befinden sich in der `System.Data.SqlClient` Namespace. Möglicherweise müssen Sie diesen Namespace in der Seite Code-Behind-Klasse zu importieren (d. h. `using System.Data.SqlClient;`).


Erstellen Sie einen Ereignishandler für das `PostCommentButton`des `Click` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

Die `Click` Ereignishandler gestartet wird, indem Sie überprüfen, dass die vom Benutzer bereitgestellten Daten gültig ist. Wenn sie nicht der Fall ist, wird der Ereignishandler vor dem Einfügen eines Datensatzes beendet. Sofern die bereitgestellten Daten sind gültig, des aktuell angemeldeten Benutzers `UserId` Wert abgerufen und gespeichert, der `currentUserId` lokale Variable. Dieser Wert ist erforderlich, da wir angeben, muss ein `UserId` Wert, der beim Einfügen eines Datensatzes in `GuestbookComments`.

Nach, dass die Verbindungszeichenfolge für die `SecurityTutorials` Datenbank entnommen `Web.config` und `INSERT` SQL-Anweisung angegeben ist. Ein `SqlConnection` Objekt dann erstellt und geöffnet. Als Nächstes wird ein `SqlCommand` -Objekt erstellt wird und die Werte für die Parameter verwendet, der `INSERT` zugewiesen sind. Die `INSERT` Anweisung anschließend ausgeführt wird und die Verbindung geschlossen. Am Ende der Ereignishandler die `Subject` und `Body` Textfelder `Text` Eigenschaften werden gelöscht, sodass der Benutzer Werte für das Postback nicht beibehalten werden.

Fahren Sie fort, und Testen Sie diese Seite in einem Browser. Da sich diese Seite in der `Membership` Ordner ist nicht verfügbar für anonyme Besucher. Aus diesem Grund müssen Sie zuerst melden Sie sich (falls Sie noch nicht getan haben). Geben Sie einen Wert in der `Subject` und `Body` Textfelder ein, und klicken Sie auf die `PostCommentButton` Schaltfläche. Dadurch wird einen neuen Datensatz hinzuzufügende `GuestbookComments`. Beim Postback werden die Betreffzeile und dem Textkörper der eingegebene in die Textfelder ein zurückgesetzt.

Nach dem Klicken auf die `PostCommentButton` Schaltfläche es existiert keine visuelle Bestätigung, die der Kommentar angezeigten hinzugefügt wurde. Wir müssen noch aktualisieren diese Seite, um es geschieht in Schritt 5 werden die vorhandenen Gästebuchkommentaren anzuzeigen. Sobald wir, die zu erreichen, erscheint der gerade hinzugefügten Kommentar in der Liste von Kommentaren, ausreichend visuelles Feedback bereitstellen. Jetzt bestätigen, dass Ihr Gästebuch Kommentar gespeichert wurde, durch Untersuchen des Inhalts der `GuestbookComments` Tabelle.

Abbildung 17 zeigt den Inhalt der `GuestbookComments` Tabelle nach zwei Kommentare geblieben sind.


[![Sie können die Gästebuchkommentaren in der Tabelle GuestbookComments sehen.](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Abbildung 17**: sehen Sie die Gästebuchkommentaren in der `GuestbookComments` Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image51.png))


> [!NOTE]
> Gefährliche Markup – z. B. HTML – ASP.NET wird ausgelöst, wenn ein Benutzer versucht, einen Gästebuch Kommentar einfügen, die potenziell enthält eine `HttpRequestValidationException`. Weitere Informationen finden Sie Informationen zu dieser Ausnahme, warum es ausgelöst wird, und wie Benutzer zum Übermitteln von potenziell gefährlichen Werte zulässt, finden Sie in der [anfordern Überprüfung Whitepaper](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Schritt 5: Auflisten von vorhandenen Gästebuchkommentaren

Zusätzlich zu verlassen Kommentare, die ein Benutzer Zugriff auf die `Guestbook.aspx` Seite sollte auch in der Lage, vorhandene Kommentare des angezeigten anzuzeigen. Um dies zu erreichen, fügen Sie einem ListView-Steuerelement mit dem Namen `CommentList` zum unteren Rand der Seite.

> [!NOTE]
> ListView-Steuerelement ist neu in ASP.NET Version 3.5. Es dient zum Anzeigen einer Liste von Elementen in einem Layout sehr anpassbar und flexible noch immer noch bieten integrierte bearbeiten, einfügen, löschen, paging und Sortieren von Funktionen wie die GridView. Wenn Sie ASP.NET 2.0 verwenden, müssen Sie stattdessen das DataList oder Wiederholungsmodul-Steuerelement verwenden. Weitere Informationen finden Sie unter der ListView, finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [die Asp: ListView-Steuerelement](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx), und Meine Artikel [Anzeigen von Daten mit dem ListView-Steuerelement](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Öffnen Sie das ListView-Smarttag und aus der Datenquelle auswählen Dropdown-Liste binden Sie das Steuerelement an eine neue Datenquelle. Wie in Schritt2 veranschaulichte Filtermenü, wird hierdurch der Konfigurations-Assistent gestartet. Wählen Sie das Symbol "Datenbank" aus, nennen Sie die resultierende SqlDataSource `CommentsDataSource`, und klicken Sie auf OK. Wählen Sie als Nächstes die `SecurityTutorialsConnectionString` Verbindung die Zeichenfolge, aus der Dropdown Liste, und klicken Sie auf Weiter.

An diesem Punkt in Schritt2 angegebenen die Daten in der Abfrage durch Entnahme der `UserProfiles` Tabelle aus der Dropdown-Liste, und wählen Sie die Spalten zurückgegeben (siehe Abbildung 9). Diesmal allerdings wir auf Wunsch eine SQL-Anweisung zu erstellen, die nicht nur die Datensätze aus sichern Synchronisierungsmodul `GuestbookComments`, aber auch die eingeblendet home Örtlichkeit, Startseite, Signatur und Benutzername. Daher wählen Sie das Optionsfeld "Geben Sie eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur", und klicken Sie auf Weiter.

Hierdurch wird der Bildschirm "Definieren Sie benutzerdefinierte Anweisungen oder gespeicherten Prozeduren" angezeigt. Klicken Sie auf die Schaltfläche Abfrage-Generator, um die Abfrage grafisch zu erstellen. Abfrage-Generator startet, indem Sie aufgefordert, die Tabellen angeben, die abgefragt werden soll. Wählen Sie die `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen aus, und klicken Sie auf OK. Dadurch werden alle drei Tabellen, die der Entwurfsoberfläche hinzugefügt. Da es sich um foreign Key-Einschränkungen bei der `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen, den Abfrage-Generator automatisch `JOIN` s Diese Tabellen.

Alle bleibt ist die Angabe der Spalten zurückgegeben. Aus der `GuestbookComments` Tabelle die `Subject`, `Body`, und `CommentDate` Spalten; zurückgeben der `HomeTown`, `HomepageUrl`, und `Signature` Spalten aus der `UserProfiles` Tabelle; und zurückgeben `UserName` aus `aspnet_Users`. Darüber hinaus hinzufügen "`ORDER BY CommentDate DESC`" am Ende der `SELECT` abzufragen, damit die neuesten Beiträge zuerst zurückgegeben werden. Nachdem Sie die Auswahl vornehmen, sollte Ihre Abfrage-Generator-Schnittstelle im Screenshot in Abbildung 18 ähneln.


[![Die Abfrage erstellt verknüpft werden soll, die GuestbookComments %Benutzerprofile% und Aspnet_Users Tabellen](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Abbildung 18**: die erstellte Abfrage `JOIN` s der `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image54.png))


Klicken Sie auf OK, um das Abfrage-Generator-Fenster zu schließen und auf dem Bildschirm "Definieren Sie benutzerdefinierte Anweisungen oder gespeicherten Prozeduren" zurückgegeben. Klicken Sie auf Weiter, um zum Bildschirm "Testabfrage", in dem Sie die Ergebnisse der Abfrage anzeigen können, indem Sie auf die Schaltfläche "Testabfrage". Wenn Sie bereit sind, klicken Sie auf "Fertig stellen", um die Datenquelle konfigurieren-Assistenten zu beenden.

Wenn wir das Konfigurieren von Datenquellen-Assistenten in Schritt2, DetailsView zugeordneten Steuerelements des abgeschlossen `Fields` Auflistung wurde aktualisiert, damit ein BoundField für jede Spalte zurückgegebene enthalten die `SelectCommand`. ListView, bleibt jedoch unverändert. Wir müssen noch das zugehörige Layout zu definieren. Das Layout der ListView kann manuell über seine deklarativem Markup oder die Option "ListView konfigurieren" in seinem Smarttag erstellt werden. Ich in der Regel bevorzugen das Markup manuell definieren, aber verwenden die geeignete Methode für Sie am häufigsten natürliche ist.

Ich sah mithilfe des folgenden `LayoutTemplate`, `ItemTemplate`, und `ItemSeparatorTemplate` für ListView-Steuerelement:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

Die `LayoutTemplate` definiert das Markup dagegen vom Steuerelement ausgegeben, die `ItemTemplate` rendert jedes Element der SqlDataSource zurückgegebenes. Die `ItemTemplate`des resultierenden Markup befindet sich der `LayoutTemplate`des `itemPlaceholder` Steuerelement. Zusätzlich zu den `itemPlaceholder`, die `LayoutTemplate` enthält ein DataPager-Steuerelement, das ListView mit 10 Gästebuchkommentaren pro Seite (Standard) beschränkt und rendert eine Auslagerungsdatei-Schnittstelle.

Meine `ItemTemplate` zeigt jeder Gästebuch Kommentar Betreff in ein `<h4>` Element mit dem Text unterhalb der Antragsteller. Beachten Sie, dass die Syntax für das Anzeigen von Text verwendet die zurückgegebene Daten akzeptiert die `Eval("Body")` Databinding-Anweisung in eine Zeichenfolge konvertiert und Zeilenumbrüche mit ersetzt die `<br />` Element. Diese Konvertierung ist erforderlich, um die Zeilenumbrüche eingegeben, wenn den Kommentar zu senden, da Leerzeichen von HTML ignoriert wird angezeigt. Der Benutzer-Signatur wird unterhalb der Text kursiv, gefolgt von der Startseite des Benutzers Örtlichkeit, die einen Link zu seiner Startseite, das Datum und Uhrzeit der Kommentar erstellt wurde und der Benutzername der Person, die den Kommentar links angezeigt.

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser anzuzeigen. Die Kommentare, die Sie in Schritt 5, die hier angezeigten angezeigten hinzugefügt haben, sollte angezeigt werden.


[![GuestBook.aspx jetzt zeigt des angezeigten Kommentare an.](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Abbildung 19**: `Guestbook.aspx` zeigt jetzt angezeigtens Kommentare ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image57.png))


Wiederholen Sie den angezeigten einen neuen Kommentar hinzugefügt. Wenn Sie auf die `PostCommentButton` Schaltfläche Seite sendet zurück und der Kommentar wird in der Datenbank hinzugefügt, aber das ListView-Steuerelement ist nicht aktualisiert, um den neuen Kommentar anzuzeigen. Dies kann entweder durch korrigiert werden:

- Aktualisieren der `PostCommentButton` Schaltfläche `Click` Ereignishandler so, dass die It des ListView-Steuerelements ruft `DataBind()` Methode auf, nachdem der neue Kommentar wird in der Datenbank eingefügt oder
- Festlegen der ListView-Steuerelement `EnableViewState` Eigenschaft `false`. Dieser Ansatz funktioniert, da durch das Deaktivieren Ansichtszustand des Steuerelements, der zugrunde liegenden Daten für jedes Postback binden müssen.

Die Tutorial Website herunterladbaren aus diesem Lernprogramm werden beide Verfahren veranschaulicht. ListView-Steuerelement `EnableViewState` Eigenschaft `false` und der Code erforderlich, um die Daten in der Listenansicht programmgesteuert binden ist vorhanden, in der `Click` Ereignishandler, d. h. jedoch auskommentiert ist.

> [!NOTE]
> Derzeit die `AdditionalUserInfo.aspx` ermöglicht es dem Benutzer anzeigen und bearbeiten ihre home Örtlichkeit Homepage und Signatur-Einstellungen. Ist es möglicherweise aktualisieren nice `AdditionalUserInfo.aspx` anzuzeigenden des angemeldeten Benutzers Gästebuchkommentaren. D. h. zusätzlich zu untersuchen, und ändern ihre Informationen können Benutzer besuchen der `AdditionalUserInfo.aspx` Bild, um anzuzeigen, welche Gästebuch Kommentare in der Vergangenheit getroffen wird. Ich lassen Sie dieses als Übung für die gewünschten Reader.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Schritt 6: Anpassen des Steuerelements CreateUserWizard, um eine Schnittstelle für die Startseite Örtlichkeit, Homepage und Signatur enthalten.

Die `SELECT` von verwendeten Abfrage der `Guestbook.aspx` Seite verwendet ein `INNER JOIN` kombiniert die verknüpften Datensätzen unter der `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen. Wenn ein Benutzer, die kein Datensatz in `UserProfiles` macht eine Gästebuch Kommentar, der Kommentar wird nicht in der Listenansicht angezeigt werden, da die `INNER JOIN` wird nur zurückgegeben `GuestbookComments` Datensätze bei übereinstimmenden Datensätze in `UserProfiles` und `aspnet_Users`. Und wie in Schritt 3 veranschaulichte Filtermenü verfügt ein Benutzer keinen Datensatz `UserProfiles` sie nicht anzeigen oder bearbeiten Sie ihre Einstellungen in der `AdditionalUserInfo.aspx` Seite.

Selbstverständlich aufgrund unserer Entwurf Entscheidungen, die es ist wichtig, dass jedem Benutzerkonto in das Mitgliedschaftssystem einen übereinstimmenden haben zeichnen Sie in der `UserProfiles` Tabelle. Das Ziel ist für einen entsprechenden Eintrag hinzugefügt werden `UserProfiles` sobald ein neues Mitgliedskonto für Benutzer über die CreateUserWizard erstellt wird.

Wie in beschrieben die [ *Erstellen von Benutzerkonten* ](creating-user-accounts-cs.md) löst Lernprogramm, nachdem das neue Benutzerkonto von Mitgliedschaft CreateUserWizard-Steuerelement erstellt wird seine [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Wir können einen Ereignishandler für dieses Ereignis erstellen, erhalten die Benutzer-ID für den gerade erstellten Benutzer und dann fügen Sie einen Datensatz in der `UserProfiles` Tabelle mit Standardwerten für die `HomeTown`, `HomepageUrl`, und `Signature` Spalten. Dazu ist es möglich, die den Benutzer für diese Werte aufgefordert werden, durch das Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Textfelder einschließen.

Sehen wir uns zunächst an, wie eine neue Zeile hinzufügen der `UserProfiles` -Tabelle in der `CreatedUser` -Ereignishandler mit Standardwerten. Danach sehen wir zum Anpassen des Steuerelements CreateUserWizard Benutzeroberfläche Einbeziehung zusätzlicher Formularfelder, um des neuen Benutzers home Örtlichkeit Homepage und Signatur zu sammeln.

### <a name="adding-a-default-row-touserprofiles"></a>Hinzufügen einer Standard-Zeile auf`UserProfiles`

In der [ *Erstellen von Benutzerkonten* ](creating-user-accounts-cs.md) Lernprogramm, die wir, ein Steuerelement CreateUserWizard hinzugefügt der `CreatingUserAccounts.aspx` auf der Seite der `Membership` Ordner. Um die CreateUserWizard haben Steuerelement einen Datensatz hinzufügen `UserProfiles` Tabelle nach Benutzerkonto erstellen, müssen wir die CreateUserWizard Funktionalität des Steuerelements zu aktualisieren. Statt diesen Änderungen an der `CreatingUserAccounts.aspx` Seite, fügen Sie ein neues CreateUserWizard-Steuerelement, um stattdessen `EnhancedCreateUserWizard.aspx` Seite, und nehmen Sie die Änderungen für dieses Lernprogramm vorhanden.

Öffnen der `EnhancedCreateUserWizard.aspx` Seite in Visual Studio, und ziehen Sie ein CreateUserWizard-Steuerelement aus der Toolbox auf die Seite. Legen Sie die CreateUserWizard `ID` Eigenschaft `NewUserWizard`. Wie in erläutert die <a id="_msoanchor_5"> </a> [ *Erstellen von Benutzerkonten* ](creating-user-accounts-cs.md) Lernprogramm, das CreateUserWizard Standardbenutzeroberfläche fordert den Besucher für die erforderlichen Informationen. Sobald diese Informationen angegeben wurden, erstellt das Steuerelement ein neues Benutzerkonto intern im Framework Mitgliedschaft ohne uns eine einzige Codezeile schreiben müssen.

Das Steuerelement CreateUserWizard löst eine Anzahl von Ereignissen während des Workflows aus. Nachdem ein Besucher die Anforderungsinformationen bereitstellt und das Formular übermittelt, das Steuerelement CreateUserWizard anfänglich ausgelöst wird seine [ `CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Wenn es ein Problem während des Erstellungsvorgangs liegt der [ `CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst wird; jedoch, wenn der Benutzer erfolgreich erstellt wurde, wird das [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst wird. In der <a id="_msoanchor_6"> </a> [ *Erstellen von Benutzerkonten* ](creating-user-accounts-cs.md) Lernprogramm wir einen Ereignishandler für erstellt das `CreatingUser` Ereignis, um sicherzustellen, dass der angegebene Benutzername keine führenden enthielt oder nachstehenden Leerzeichen sowie, dass der Benutzername nicht an einer beliebigen Stelle im Kennwort angezeigt.

Zum Hinzufügen einer Zeile in der `UserProfiles` Tabelle für den gerade erstellten Benutzer müssen wir erstellen Sie einen Ereignishandler für das `CreatedUser` Ereignis. Nach der Dauer der `CreatedUser` Ereignis ausgelöst wurde, das Benutzerkonto wurde bereits in Mitgliedschaft Framework, aktivieren uns um das Konto UserId Wert abzurufen.

Erstellen Sie einen Ereignishandler für das `NewUserWizard`des `CreatedUser` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

Der obige Code Menschen durch Abrufen der Benutzer-ID für das gerade hinzugefügte Benutzerkonto. Dies erfolgt mithilfe der `Membership.GetUser(username)` Methode zum Zurückgeben von Informationen über einen bestimmten Benutzer, und klicken Sie dann mit der `ProviderUserKey` Eigenschaft, für ihre Benutzer-ID abgerufen. Der vom Benutzer im Steuerelement CreateUserWizard eingegebene Benutzername ist über seine [ `UserName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Als Nächstes wird die Verbindungszeichenfolge abgerufen, von `Web.config` und `INSERT` -Anweisung angegeben. ADO.NET-Objekte instanziiert werden, und der Befehl ausgeführt. Weist der Code eine [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) -Instanz, auf die `@HomeTown`, `@HomepageUrl`, und `@Signature` -Parameter, die Datenbank einfügen, hat `NULL` Werte für die `HomeTown`, `HomepageUrl`, und `Signature` Felder.

Besuchen Sie die `EnhancedCreateUserWizard.aspx` Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto. Nach der Anmeldung zurück zu Visual Studio, und Untersuchen des Inhalts der `aspnet_Users` und `UserProfiles` Tabellen (wie in Abbildung 12). Daraufhin sollte das neue Benutzerkonto in `aspnet_Users` und einer entsprechenden `UserProfiles` Zeile (mit `NULL` Werte für `HomeTown`, `HomepageUrl`, und `Signature`).


[![Ein neues Benutzerkonto und %Benutzerprofile% Datensatz wurden hinzugefügt](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Abbildung 20**: ein neues Benutzerkonto und `UserProfiles` Datensatz hinzugefügt wurden ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image60.png))


Nachdem der Besucher seinen neuen Kontoinformationen angegeben und auf die Schaltfläche "Benutzer erstellen" geklickt hat, das Benutzerkonto erstellt wird und eine Zeile hinzugefügt, die `UserProfiles` Tabelle. Zeigt die CreateUserWizard dann seine `CompleteWizardStep`, woraufhin eine Erfolgsmeldung und eine Schaltfläche "Weiter". Klicken auf die Schaltfläche "Weiter" bewirkt, dass ein Postback handeln, jedoch wird keine Aktion ausgeführt, verlässt des Benutzers auf hängen die `EnhancedCreateUserWizard.aspx` Seite.

Wir können Geben Sie eine URL zum Senden des Benutzers, wenn die Schaltfläche "Weiter", über des CreateUserWizard Steuerelements geklickt wird [ `ContinueDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Legen Sie die `ContinueDestinationPageUrl` -Eigenschaft auf "~ / Membership/AdditionalUserInfo.aspx". Dies hat den neuen Benutzer zu `AdditionalUserInfo.aspx`, wobei sie können anzeigen und aktualisieren Sie ihre Einstellungen.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Anpassen der CreateUserWizard-Schnittstelle, um nach des neuen Benutzers Home-Stadt, Homepage und Signatur

Standardschnittstelle CreateUserWizard-Steuerelement ist ausreichend, einfach Erstellung Szenarien, in denen nur Core Benutzerkontoinformationen wie Benutzername, Kennwort und e-Mail-gesammelt werden muss. Aber was geschieht, wenn die Besucher ihrer home Örtlichkeit Homepage und Signatur geben beim Erstellen von ihrem Konto aufgefordert werden sollten? Es ist möglich, Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Informationen bei der Registrierung zu sammeln und diese Informationen kann verwendet werden, der `CreatedUser` -Ereignishandler, um zusätzliche Datensätze in der zugrunde liegenden Datenbank einzufügen.

Das Steuerelement CreateUserWizard erweitert das ASP.NET Assistenten-Steuerelement, das ein Steuerelement der Entwickler einer Seite zum Definieren einer Reihe von geordneten ermöglicht `WizardSteps`. Das Assistenten-Steuerelement rendert aktiven Schritt und bietet eine Navigationsschnittstelle, die den Besucher so verschieben Sie diese Schritte aus, ermöglicht. Das Assistenten-Steuerelement ist ideal für einen langen Task in mehrere kurze Schritte unterteilen. Weitere Informationen im Assistenten zum Steuerelement finden Sie unter [erstellen eine schrittweise Anleitung-Benutzeroberfläche mit dem Assistenten-Steuerelement in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Die CreateUserWizard Steuerelementmarkup Standard definiert zwei `WizardSteps`: `CreateUserWizardStep` und `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

Die erste `WizardStep`, `CreateUserWizardStep`, rendert die Schnittstelle, die der Benutzername, Kennwort, e-Mail usw. dazu aufgefordert werden. Nachdem der Besucher diese Informationen werden bereitgestellt, und klickt auf "Create User", wird sie angezeigt der `CompleteWizardStep`, es zeigt bestätigungsmeldung und eine Schaltfläche "Weiter".

Zum Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Felder enthalten, können wir folgende Aktionen ausführen:

- <strong>Erstellen Sie eine oder mehrere neue</strong><strong>`WizardStep`</strong><strong>s, um die zusätzlichen Benutzeroberflächenelemente enthalten</strong>. Zum Hinzufügen einer neuen `WizardStep` die CreateUserWizard, klicken Sie auf der "hinzufügen/entfernen `WizardSteps`" Verknüpfung von Smart Tag zum Starten der `WizardStep` Auflistungs-Editor. Von dort aus können Sie hinzufügen, entfernen oder Neuanordnen von Schritten im Assistenten. Dies ist die Vorgehensweise, die für dieses Lernprogramm verwendet wird.

- <strong>Konvertieren der</strong><strong>`CreateUserWizardStep`</strong><strong>in einer bearbeitbaren</strong><strong>`WizardStep`</strong><strong>.</strong> Dies ersetzt die `CreateUserWizardStep` durch ein Äquivalent `WizardStep` , deren Markups definiert eine Benutzeroberfläche, die entspricht der `CreateUserWizardStep`"s. Durch Konvertieren der `CreateUserWizardStep` in einem `WizardStep` können wir die Steuerelemente neu positionieren oder fügen Sie zusätzliche Elemente der Benutzeroberfläche für diesen Schritt. Konvertiert die `CreateUserWizardStep` oder `CompleteWizardStep` in einer bearbeitbaren `WizardStep`, klicken Sie auf die "Benutzer anpassen-erstellen-Schritt" oder "Schritts anpassen" aus dem Smarttag des Steuerelements zu verknüpfen.

- **Verwenden Sie eine besondere Kombination der oben genannten beiden Optionen.**

Wichtig zu bedenken ist, dass Steuerelement CreateUserWizard Erstellungsprozess seine Benutzer-Konto ausgeführt wird, wenn die Schaltfläche "Benutzer erstellen", innerhalb geklickt wird dessen `CreateUserWizardStep`. Es spielt keine Rolle, wenn zusätzliche `WizardStep` s nach der `CreateUserWizardStep` oder nicht.

Beim Hinzufügen eines benutzerdefinierten `WizardStep` an das Steuerelement CreateUserWizard zusätzliche Benutzereingaben, die benutzerdefinierte erfassen `WizardStep` platziert werden kann, vor oder nach der `CreateUserWizardStep`. Wenn es vorangeht der `CreateUserWizardStep` dann die zusätzliche Benutzereingabe gesammelt, aus dem benutzerdefinierten `WizardStep` steht für die `CreatedUser` -Ereignishandler. Jedoch wenn die benutzerdefinierte `WizardStep` kommt nach `CreateUserWizardStep` dann nach der Zeit die benutzerdefinierte `WizardStep` wird angezeigt, das neue Benutzerkonto wurde bereits erstellt und die `CreatedUser` Ereignis bereits ausgelöst.

Abbildung 21 zeigt den Workflow bei der hinzugefügten `WizardStep` steht vor der `CreateUserWizardStep`. Seit dem Zeitpunkt die weiteren Benutzerinformationen gesammelt wurden die `CreatedUser` Ereignis ausgelöst wird, müssen wir tun Update ist die `CreatedUser` -Ereignishandler, um diese Eingaben abrufen und verwenden Sie diese für die `INSERT` Parameterwerte-Anweisung (statt `DBNull.Value`).


[![Die CreateUserWizard-Workflow, wenn eine zusätzliche WizardStep der CreateUserWizardStep vorangestellt ist.](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Abbildung 21**: die CreateUserWizard Workflow bei einer zusätzlichen `WizardStep` Precedes der `CreateUserWizardStep` ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image63.png))


Wenn die benutzerdefinierte `WizardStep` befindet *nach* der `CreateUserWizardStep`, allerdings den Prozess zum Erstellen eines Benutzers Konto tritt auf, bevor der Benutzer eine Möglichkeit, ihre Startseite Örtlichkeit, Startseite oder Signatur eingeben hatte. In diesem Fall muss diese zusätzlichen Informationen in die Datenbank eingefügt werden, nachdem das Benutzerkonto erstellt wurde, wie in Abbildung 22 dargestellt.


[![Die CreateUserWizard-Workflow, wenn eine zusätzliche WizardStep nach der CreateUserWizardStep kommt](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Abbildung 22**: die CreateUserWizard Workflow bei einer zusätzlichen `WizardStep` stammt nach der `CreateUserWizardStep` ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image66.png))


Mit dem Einfügen eines Datensatzes in der Abbildung 22 Workflow wartet, die `UserProfiles` Tabelle erst nach Abschluss von Schritt2. Wenn Besucher ihrer Browser nach Schritt 1 geschlossen wurde, allerdings haben wir haben erreicht einen Status, in dem ein Benutzerkonto erstellt wurde, aber kein Datensatz wurde hinzugefügt, um `UserProfiles`. Eine Lösung besteht darin, einen Datensatz mit `NULL` oder Standardwerte eingefügt `UserProfiles` in die `CreatedUser` Ereignishandler (nach Schritt 1 ausgelöst wird), und aktualisieren Sie dann diese nach Abschluss von Schritt 2 erfassen. Dadurch wird sichergestellt, dass eine `UserProfiles` Datensatz wird für das Benutzerkonto hinzugefügt werden, selbst wenn der Benutzer die Registrierung Prozess genau über beendet.

Für dieses Lernprogramm erstellen Sie nun ein neues `WizardStep` Vorgang, der nach der `CreateUserWizardStep` aber vor der `CompleteWizardStep`. Wir sehen rufen Sie zuerst die WizardStep in platzieren und konfiguriert aus, und klicken Sie dann wir uns den Code.

Smart Tag CreateUserWizard-Steuerelement, wählen Sie in der "hinzufügen/entfernen `WizardStep` s", die bringt die `WizardStep` Auflistungs-Editor-Dialogfeld. Fügen Sie einen neuen `WizardStep`wird durch das Festlegen der `ID` auf `UserSettings`, dessen `Title` auf "Ihre Einstellungen" und die zugehörige `StepType` auf `Step`. Positionieren Sie es so, dass es nach geht die `CreateUserWizardStep` ("Registrieren für Ihr neues Konto") und vor der `CompleteWizardStep` ("abgeschlossen"), wie in Abbildung 23 dargestellt.


[![Hinzufügen einer neuen WizardStep an das Steuerelement CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Abbildung 23**: Hinzufügen ein neuen `WizardStep` an das Steuerelement CreateUserWizard ([klicken Sie hier, um das Bild in voller Größe angezeigt](storing-additional-user-information-cs/_static/image69.png))


Klicken Sie auf OK, um das Schließen der `WizardStep` Auflistungs-Editor-Dialogfeld. Die neue `WizardStep` wird durch das CreateUserWizard Steuerelement aktualisierte deklarativem Markup gezeigt:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Beachten Sie die neue `<asp:WizardStep>` Element. Wir müssen die Benutzeroberfläche zum Erfassen des neuen Benutzers home Örtlichkeit, Homepage und Signatur hier hinzufügen. Sie können diesen Inhalt in die deklarative Syntax oder mithilfe des Designers eingeben. Um den Designer zu verwenden, wählen Sie im Schritt "Ihre Einstellungen" aus der Dropdown-Liste in das Smarttag, um den Schritt im Designer finden Sie unter.

> [!NOTE]
> Eine schrittweise durchlaufen das Smarttag-Dropdownliste auswählen des Steuerelements CreateUserWizard aktualisiert [ `ActiveStepIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), gibt den Index des ersten Schritts an. Wenn Sie dieses Dropdown-Liste verwenden, um den Schritt "Ihre Einstellungen" im Designer bearbeiten, werden daher sicher, dass es wieder auf "Anmelden für Ihr neues Konto" festlegen, so, dass dieser Schritt, beim ersten Besuch angezeigt wird der `EnhancedCreateUserWizard.aspx` Seite.


Erstellen einer Benutzeroberfläche innerhalb der Schritt "Ihre Einstellungen", die drei TextBox-Steuerelemente, die mit dem Namen enthält `HomeTown`, `HomepageUrl`, und `Signature`. Nach dem Erstellen dieser Schnittstelle, sollte die CreateUserWizard deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Fahren Sie fort, und finden Sie auf dieser Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto, und geben Sie Werte für den home Örtlichkeit Homepage und Signatur angeben. Nach Abschluss der `CreateUserWizardStep` ist das Benutzerkonto, das in die Mitgliedschaft-Framework erstellt und die `CreatedUser` -Ereignishandler ausgeführt, das eine neue Zeile hinzufügt `UserProfiles`, jedoch mit einer Datenbank `NULL` Wert für `HomeTown`, `HomepageUrl`, und `Signature`. Die eingegebenen Werte für die home-Stadt, Homepage und Signatur werden nie verwendet. Das Ergebnis ist ein neues Benutzerkonto mit einem `UserProfiles` aufzeichnen, deren `HomeTown`, `HomepageUrl`, und `Signature` Felder besitzen noch angegeben werden.

Wir müssen zum Ausführen von Code nach dem Schritt "Ihre Einstellungen", die nimmt die home Örtlichkeit Honepage und Signatur Werte, die vom Benutzer eingegeben und aktualisiert den entsprechenden `UserProfiles` Datensatz. Jedes Mal, die der Benutzer wechselt zwischen den Schritten des Assistenten zu steuern, des Assistenten [ `ActiveStepChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) ausgelöst wird. Wir können erstellen Sie einen Ereignishandler für dieses Ereignis und das Update der `UserProfiles` Tabelle, wenn der Schritt "Ihre Einstellungen" abgeschlossen wurde.

Fügen Sie einen Ereignishandler für das CreateUserWizard `ActiveStepChanged` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

Der obige Code beginnt mit bestimmen, ob wir einfach den Schritt "Vollständig" erreicht haben. Da der Schritt "Vollständig" unmittelbar nach dem Schritt "Ihre Einstellungen" auftritt, erreicht der Besucher Schritt klicken Sie dann "Abgeschlossen", das bedeutet, dass sie nur den Schritt "Ihre Einstellungen" abgeschlossen.

In einem solchen Fall muss innerhalb der TextBox-Steuerelemente programmgesteuert verweisen die `UserSettings WizardStep`. Dies wird erreicht, indem Sie zuerst die `FindControl` Methode, um programmgesteuert verweisen auf die `UserSettings WizardStep`, und dann erneut aus, um die Textfelder ein heraus verweisen die `WizardStep`. Nachdem die Textfelder ein verwiesen wurde, können wir bereit zur Ausführung der `UPDATE` Anweisung. Die `UPDATE` Anweisung verfügt über die gleiche Anzahl von Parametern wie der `INSERT` -Anweisung in der `CreatedUser` Ereignishandler, d. h. jedoch hier verwenden wir die home Örtlichkeit Homepage und Signatur Werte, die vom Benutzer bereitgestellt werden.

Mit diesem Ereignishandler vorhanden, besuchen Sie die `EnhancedCreateUserWizard.aspx` Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto mit dem Sie Werte für den home Örtlichkeit Homepage und Signatur angeben. Nach der Erstellung des neuen Kontos sollten Sie umgeleitet die `AdditionalUserInfo.aspx` Seite, in dem die gerade eingegebenen home Örtlichkeit Homepage und Signatur Informationen werden angezeigt.

> [!NOTE]
> Unserer Website verfügt derzeit über zwei Seiten, die von dem Besucher ein neues Konto erstellen kann: `CreatingUserAccounts.aspx` und `EnhancedCreateUserWizard.aspx`. Website Sitemap und Anmeldeseite zeigen Sie auf die `CreatingUserAccounts.aspx` Seite, aber die `CreatingUserAccounts.aspx` Seite fordert ihre Startseite Örtlichkeit Homepage und Signatur-Informationen vom Benutzer nicht und werden nicht zu eine entsprechende Zeile hinzugefügt `UserProfiles`. Aktualisieren Sie daher entweder die `CreatingUserAccounts.aspx` Seite, damit er diese Funktionalität bietet, oder die Seite Sitemap und melden Sie sich auf "Aktualisieren" `EnhancedCreateUserWizard.aspx` anstelle von `CreatingUserAccounts.aspx`. Wenn Sie letztere Option auswählen, müssen Sie beim Aktualisieren der `Membership` des Ordners `Web.config` Datei, um anonymen Benutzern Zugriff auf die `EnhancedCreateUserWizard.aspx` Seite.


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm wurde Techniken zum Modellieren von Daten, die auf Benutzerkonten innerhalb des Frameworks Mitgliedschaft bezieht. Insbesondere erläutert wir Modellieren von Entitäten, die gemeinsam eine 1: n-Beziehung mit Benutzerkonten als auch Daten, die eine direkte Beziehung gemeinsam nutzen. Darüber hinaus wurde erläutert, wie diese Informationen kann angezeigt, eingefügt und aktualisiert werden, mit einigen Beispielen SqlDataSource-Steuerelement und andere Beziehung ADO.NET-Code verwenden.

Dieses Lernprogramm ist unsere Betrachtung der Benutzerkonten abgeschlossen. Beginnend mit den nächsten Lernprogrammen wird unsere Aufmerksamkeit Rollen aktivieren. Über den nächsten mehrere Lernprogramme, betrachten wir das Framework Rollen, finden Sie unter Vorgehensweise: Erstellen Sie neue Rollen an Benutzer, Rollen zuweisen, wie um zu bestimmen, welche Rollen ein Benutzer angehört, und wie rollenbasierte Autorisierung anzuwenden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0-Assistent-Steuerelement](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Erstellen eine schrittweise Benutzeroberfläche mit dem ASP.NET 2.0-Assistent-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Erstellen von benutzerdefinierten DataSource Steuerelementparameter](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Anpassen des Steuerelements CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Schnellstarts DetailsView-Steuerelement](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Anzeigen von Daten mit dem ListView-Steuerelement](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Unterzogen, wodurch die Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Bearbeiten von einfügen und Löschen von Daten](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Formularvalidierung in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Sammeln von benutzerdefinierten-Registrierungsinformationen](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profile in ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Die Asp: ListView-Steuerelement](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [User Profile Schnellstart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informationen zum Autor

Scott Mitchell, Autor von mehreren ASP/ASP.NET-Büchern und Gründer von 4GuysFromRolla.com, bereits seit 1998 mit Microsoft-Web-Technologien gearbeitet. Scott fungiert als ein unabhängiger Berater, Trainer und Writer. Sein neueste Buch wird * [Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese Reihe von Lernprogrammen wurde durch viele nützliche Bearbeiter überprüft. Meine bevorstehende MSDN-Artikel Überprüfen von Interesse? Wenn dies der Fall ist, löschen Sie mich zeilenweise [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Zurück](user-based-authorization-cs.md)
> [Weiter](creating-the-membership-schema-in-sql-server-vb.md)
