---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Speichern von zusätzlichen Benutzerinformationen (VB) | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Tutorial werden wir diese Frage beantworten, indem Sie eine sehr rudimentäre gästebuchanwendung erstellen. Auf diese Weise untersuchen verschiedene Optionen für Modeli wir...
ms.author: aspnetcontent
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 2ea731012f8c053d1e1eac293fdbeaec66db23bf
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816037"
---
<a name="storing-additional-user-information-vb"></a>Speichern von zusätzlichen Benutzerinformationen (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> In diesem Tutorial werden wir diese Frage beantworten, indem Sie eine sehr rudimentäre gästebuchanwendung erstellen. In diesem Fall werden wir sehen Sie sich die verschiedenen Optionen für die Modellierung von Benutzerinformationen in einer Datenbank und feststellen, wie Sie die Benutzerkonten, die durch das mitgliedschaftsframework erstellt diese Daten zuordnen.


## <a name="introduction"></a>Einführung

ASP. NET mitgliedschaftsframework bietet eine flexible Benutzeroberfläche zum Verwalten von Benutzern. Membership-API enthält Methoden zum Überprüfen der Anmeldeinformationen, Abrufen von Informationen zu den derzeit angemeldeten Benutzer, ein neues Benutzerkonto erstellen und Löschen eines Benutzerkontos, unter anderem. Jedes Benutzerkonto in das mitgliedschaftsframework enthält nur die Eigenschaften für Anmeldeinformationen überprüft und das Ausführen von Aufgaben für wichtige Benutzer im Zusammenhang mit Konto erforderlich. Durch die Methoden und Eigenschaften der äußerte der [ `MembershipUser` Klasse](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), das ein Benutzerkonto in das mitgliedschaftsframework modelliert. Diese Klasse verfügt über Eigenschaften wie [ `UserName` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [ `Email` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx), und [ `IsLockedOut` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), und Methoden wie [ `GetPassword` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) und [ `UnlockUser` ](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Anwendungen müssen häufig zum Speichern von zusätzlichen Benutzerinformationen, die nicht in das mitgliedschaftsframework enthalten. Beispielsweise kann ein Onlinehändler müssen jeden Benutzer gespeichert werden, ihre Liefer-und Rechnungsadresse, die Zahlungsinformationen, die übermittlungseinstellungen und Telefonnummer des Ansprechpartners. Darüber hinaus ist jede Bestellung in das System mit einem bestimmten Benutzerkonto verknüpft.

Die `MembershipUser` Klasse enthält keine Eigenschaften wie `PhoneNumber` oder `DeliveryPreferences` oder `PastOrders`. Wie wir nachverfolgen die Benutzerinformationen von der Anwendung benötigten und deren Integration in das mitgliedschaftsframework? In diesem Tutorial werden wir diese Frage beantworten, indem Sie eine sehr rudimentäre gästebuchanwendung erstellen. In diesem Fall werden wir sehen Sie sich die verschiedenen Optionen für die Modellierung von Benutzerinformationen in einer Datenbank und feststellen, wie Sie die Benutzerkonten, die durch das mitgliedschaftsframework erstellt diese Daten zuordnen. Fangen wir an!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Schritt 1: Erstellen der Gästebuchanwendung-Datenmodell

Es gibt eine Vielzahl von Techniken, die zum Erfassen von Benutzerinformationen in einer Datenbank, und ordnen sie die Benutzerkonten, die durch das mitgliedschaftsframework erstellt eingesetzt werden können. Um diese Techniken zu veranschaulichen, müssen wir die Tutorials Webanwendung erweitern, damit es eine Art von benutzerspezifischen Daten erfasst. (Derzeit Datenmodell der Anwendung enthält, nur die Anwendung erforderlichen Tabellen services die `SqlMembershipProvider`.)

Erstellen wir eine sehr einfaches gästebuchanwendung, in denen ein authentifizierter Benutzer auf einen Kommentar abgeben kann. Zusätzlich zur Speicherung von Gästebuch Kommentare, lassen wir für jeden Benutzer zum Speichern seiner home Stadt, Startseite und der Signatur. Wenn angegeben, home Stadt des Benutzers, erscheint Startseite, und die Signatur für jede Nachricht das, die er in das Gästebuch verlassen hat.

### <a name="adding-theguestbookcommentstable"></a>Hinzufügen der`GuestbookComments`Tabelle

Um die Gästebuch-Kommentare zu erfassen, müssen wir eine Datenbanktabelle, die mit dem Namen erstellen `GuestbookComments` , die Spalten an, wie hat `CommentId`, `Subject`, `Body`, und `CommentDate`. Wir müssen auch von jeder Datensatz in die `GuestbookComments` Tabellenreferenz der Benutzer, die den Kommentar hinterlassen.

Um diese Tabelle der Datenbank hinzugefügt haben, fahren Sie mit der Datenbank-Explorer in Visual Studio, und navigieren Sie zu der `SecurityTutorials` Datenbank. Mit der rechten Maustaste auf den Ordner "Tabellen", und wählen Sie die neue Tabelle hinzufügen. Daraufhin wird eine Schnittstelle, die zum Definieren von Spalten für die neue Tabelle ermöglicht.


[![Hinzufügen einer neuen Tabelle in der Datenbank SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Abbildung 1**: Hinzufügen einer neuen Tabelle an der `SecurityTutorials` Datenbank ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image3.png))


Als Nächstes definieren Sie die `GuestbookComments`Spalten. Starten Sie durch Hinzufügen einer Spalte mit dem Namen `CommentId` des Typs `uniqueidentifier`. In dieser Spalte werden einzelnen Kommentare in das Gästebuch eindeutig zu identifizieren, also nicht zulassen `NULL` s und kennzeichnet sie als Primärschlüssel der Tabelle. Anstatt einen Wert für die `CommentId` für jedes Feld `INSERT`, es kann darauf hinweisen, die eine neue `uniqueidentifier` Wert sollte automatisch für dieses Feld generiert werden, auf `INSERT` indem Sie den Wert der Spalte standardmäßig auf `NEWID()`. Nach dem Hinzufügen dieser ersten Feld, markieren sie den Standardwert festgelegt als die primary key- und die Einstellungen, sollte Ihr Bildschirm ähnlich wie im Screenshot in Abbildung 2 dargestellt aussehen.


[![Hinzufügen einer primären Spalteninhalts, die mit dem Namen CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Abbildung 2**: Hinzufügen einer primären Spalte mit dem Namen `CommentId` ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image6.png))


Fügen Sie eine Spalte namens `Subject` des Typs `nvarchar(50)` und eine Spalte mit dem Namen `Body` des Typs `nvarchar(MAX)`, unterbinden einer `NULL` s in beide Spalten. Danach fügen Sie eine Spalte, die mit dem Namen `CommentDate` des Typs `datetime`. Nicht zulassen `NULL` s, und legen die `CommentDate` Spalte Standardwert, `getdate()`.

Übrig bleibt, eine Spalte hinzuzufügen, die ein Benutzerkonto mit den einzelnen Kommentaren Gästebuch zuordnet. Eine Möglichkeit wäre, eine Spalte mit dem Namen hinzuzufügen `UserName` des Typs `nvarchar(256)`. Dies ist eine geeignete Wahl, wenn einen Mitgliedschaftsanbieter nicht zu verwenden die `SqlMembershipProvider`. Aber bei Verwendung der `SqlMembershipProvider`, wie wir in dieser tutorialreihe sind die `UserName` -Spalte in der `aspnet_Users` Tabelle nicht notwendigerweise eindeutig sein. Die `aspnet_Users` Primärschlüssel der Tabelle ist `UserId` und ist vom Typ `uniqueidentifier`. Aus diesem Grund die `GuestbookComments` Tabelle benötigt, eine Spalte namens `UserId` des Typs `uniqueidentifier` (untersagen `NULL` Werte). Fahren Sie fort, und fügen Sie diese Spalte.

> [!NOTE]
> Wie wir unter den [ *Erstellen des Mitgliedschaftsschemas in SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md) Tutorial, das mitgliedschaftsframework ermöglicht mehrere Webanwendungen unter anderen Benutzerkonten, die denselben Speicher des Benutzers. Hierzu werden die Benutzerkonten in verschiedenen Anwendungen zu partitionieren. Und zwar alle Benutzernamen innerhalb einer Anwendung eindeutig ist unbedingt, kann der gleiche Benutzername in verschiedenen Anwendungen, die mit den gleichen Speicher des Benutzers verwendet werden. Es ist eine Zusammensetzung `UNIQUE` -Einschränkung in der `aspnet_Users` -Tabelle über die `UserName` und `ApplicationId` Felder, jedoch nicht auf nur die `UserName` Feld. Daher ist es möglich, dass der Aspnet\_Tabelle haben Sie zwei (oder mehr) Datensätze mit demselben Benutzer `UserName` Wert. Vorhanden ist, jedoch eine `UNIQUE` -Einschränkung für die `aspnet_Users` tabellenspezifischen `UserId` Feld (da es sich um den Primärschlüssel ist). Ein `UNIQUE` Einschränkung ist wichtig, da ohne wir eine fremdschlüsseleinschränkung zwischen herstellen kann die `GuestbookComments` und `aspnet_Users` Tabellen.


Nach dem Hinzufügen der `UserId` Spalte speichern in der Tabelle durch Klicken auf das Symbol "Speichern" in der Symbolleiste. Benennen Sie die neue Tabelle `GuestbookComments`.

Wir haben einen letzten Ausgabe mit Aufmerksamkeit schenken müssen die `GuestbookComments` Tabelle: müssen wir erstellen eine [foreign Key-Einschränkung](https://msdn.microsoft.com/library/ms175464.aspx) zwischen der `GuestbookComments.UserId` Spalte und die `aspnet_Users.UserId` Spalte. Um dies zu erreichen, klicken Sie auf das Symbol "Beziehung" in der Symbolleiste, um das Dialogfeld "Fremdschlüsselbeziehungen" aufzurufen. (Alternativ können Sie dieses Dialogfeld starten durch Navigieren zum Menü Tabellen-Designer und Beziehungen auswählen.)

Klicken Sie auf die Schaltfläche "hinzufügen" unten links im Dialogfeld "Fremdschlüsselbeziehungen". Dadurch wird eine neue foreign Key-Einschränkung, hinzugefügt, aber wir müssen trotzdem noch definieren Sie die Tabellen, die beteiligt sind, in der Beziehung.


[![Verwenden Sie das Dialogfeld "Fremdschlüsselbeziehungen" zum Verwalten von Foreign Key-Einschränkungen einer Tabelle](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Abbildung 3**: Verwenden Sie das Dialogfeld Foreign Key-Beziehungen zum Verwalten von Foreign Key-Einschränkungen einer Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image9.png))


Klicken Sie dann auf das Symbol "Auslassungspunkte" in der "Tabelle und Spaltenspezifikation" Zeile auf der rechten Seite. Hierdurch wird das Dialogfeld Tabellen und Spalten, die aus dem können wir angeben, der Primärschlüsseltabelle und die Spalte und die Fremdschlüsselspalte aus der `GuestbookComments` Tabelle. Wählen Sie vor allem `aspnet_Users` und `UserId` als die Primärschlüsseltabelle und die Spalte, und `UserId` aus der `GuestbookComments` Tabelle als Fremdschlüsselspalte (siehe Abbildung 4). Nach dem Definieren der Primär- und Fremdschlüssel wichtige Tabellen und Spalten, klicken Sie auf OK, um das Dialogfeld "Fremdschlüsselbeziehungen" zurückzukehren.


[![Richten Sie eine Foreign Key-Einschränkung zwischen der Aspnet_Users und GuesbookComments Tabellen](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Abbildung 4**: Einrichten einer Foreign Key-Einschränkung zwischen der `aspnet_Users` und `GuesbookComments` Tabellen ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image12.png))


An diesem Punkt wurde der foreign Key-Einschränkung hergestellt. Das Vorhandensein dieser Einschränkung stellt sicher [relationale Integrität](http://en.wikipedia.org/wiki/Referential_integrity) zwischen den beiden Tabellen, indem sichergestellt wird, gibt es nie einen Gästebuch-Eintrag verweist auf eine nicht existierende-Benutzerkonto. Standardmäßig eine foreign Key-Einschränkung nicht zu, wenn einen übergeordneter Datensatz gelöscht werden soll, wenn entsprechende untergeordnete Datensätze. Also wenn ein Benutzer einen oder mehrere Gästebuch-Kommentare macht, und klicken Sie dann wir versuchen, dieses Benutzerkonto zu löschen, fehl der Löschvorgang, wenn seine Gästebuch Kommentare zuerst gelöscht werden.

Foreign Key-Einschränkungen können konfiguriert werden, um die zugehörigen untergeordneten Datensätze automatisch zu löschen, wenn ein übergeordneter Datensatz gelöscht wird. Anders gesagt können wir diese foreign Key-Einschränkung einrichten, sodass Gästebucheinträge eines Benutzers automatisch gelöscht werden, wenn ihr Benutzerkonto gelöscht wird. Um dies zu erreichen, erweitern Sie im Abschnitt "INSERT und UPDATE-Spezifikation", und legen Sie die "Regel löschen"-Eigenschaft auf Cascade.


[![Konfigurieren von Foreign Key-Einschränkung auf Löschweitergaben](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Abbildung 5**: Konfigurieren der Foreign Key-Einschränkung auf Sie Löschweitergaben ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image15.png))


Klicken Sie auf die Schaltfläche "Schließen", um zu den Foreign Key Relationships verlassen, um die foreign Key-Einschränkung zu speichern. Klicken Sie dann auf das Symbol "Speichern" in der Symbolleiste, um das Speichern der Tabelle und dieser Beziehung.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Speichern des Benutzers Home-Stadt, Startseite und Signatur

Die `GuestbookComments` Tabelle wird veranschaulicht, wie zum Speichern von Informationen, die eine 1: n Beziehung mit den Benutzerkonten verwendet. Da jedes Benutzerkonto möglicherweise eine beliebige Anzahl von zugehörigen Kommentare hat, wird diese Beziehung modelliert, durch das Erstellen einer Tabelle zum Speichern von des Satz von Kommentaren, der eine Spalte enthält, verknüpft mit jeder Kommentar für einen bestimmten Benutzer zurück. Bei Verwendung der `SqlMembershipProvider`, diesen Link wird am besten hergestellt, indem Sie eine Spalte mit dem Namen erstellt `UserId` des Typs `uniqueidentifier` und eine fremdschlüsseleinschränkung zwischen dieser Spalte und `aspnet_Users.UserId`.

Nun müssen wir ordnen Sie drei Spalten mit jedem Konto zum Speichern des Benutzers home Town-Homepage und Signatur, die in seinem Gästebuchkommentaren angezeigt wird. Es gibt verschiedene Möglichkeiten, dies zu erreichen, Anzahl:

- <strong>Hinzufügen neuer Spalten, die</strong><strong>`aspnet_Users`</strong><strong>oder</strong><strong>`aspnet_Membership`</strong><strong>Tabellen.</strong> Ich würde dieser Ansatz nicht empfehlen, da er das Schema ein, ändert der `SqlMembershipProvider`. Diese Entscheidung kann zurückkehren auf Dich zurückfallen, ersparen. Was geschieht, wenn eine zukünftige Version von ASP.NET ein anderes verwendet beispielsweise `SqlMembershipProvider` Schema. Möglicherweise enthält die von Microsoft ein Tool zum Migrieren von ASP.NET 2.0 `SqlMembershipProvider` Daten in das neue Schema, aber wenn Sie ASP.NET 2.0 vorgenommenen `SqlMembershipProvider` Schema, eine solche Konvertierung möglicherweise nicht möglich.

- **Verwenden Sie ASP. NET Profil Framework eine Profileigenschaft für die home-Stadt, Startseite und Signatur definieren.** ASP.NET umfasst eine Profil-Framework, das zusätzliche benutzerspezifische Daten gespeichert werden soll. Wie das mitgliedschaftsframework wird das Profil-Framework auf dem Anbietermodell erstellt. Im Lieferumfang von .NET Framework eine `SqlProfileProvider` , dass ein Datenprofil in einer SQL Server-Datenbank speichert. In der Tat verfügt der Datenbank bereits in der Tabelle ein, die die `SqlProfileProvider` (`aspnet_Profile`), wie sie hinzugefügt wurde, wenn wir hinzugefügt, der die Anwendungsdienste sichern, der [ *Erstellen des Mitgliedschaftsschemas in SQL Server* ](creating-the-membership-schema-in-sql-server-vb.md)Tutorial.   
  Der Hauptvorteil der Profil-Framework ist, dass es Entwicklern ermöglicht, definieren die Profileigenschaften in `Web.config` – muss kein zusätzlicher Code geschrieben werden, um die Profildaten in und aus dem zugrunde liegenden Datenspeicher zu serialisieren. Kurz gesagt, ist es unglaublich einfach, einen Satz von Eigenschaften zu definieren und diese im Code verwenden. Das Profilsystem lässt allerdings viel zu wünschen übrig, bei der versionsverwaltung, wenn Sie verfügen über eine Anwendung, in dem Sie erwarten, dass neue benutzerspezifische Eigenschaften hinzugefügt werden, auf einen späteren Zeitpunkt oder vorhandene entfernt oder geändert werden, und klicken Sie dann das Profil-Framework möglicherweise nicht, die  beste Option. Darüber hinaus die `SqlProfileProvider` speichert die Eigenschaften des Profils in eine hochgradig denormalisierten Weise es fast unmöglich, Ausführen von Abfragen direkt für die Profildaten (z. B. wie viele Benutzer eine Startseite Town New York haben).   
  Weitere Informationen zu dem Profil-Framework finden Sie in den Abschnitt "Weitere Literatur" am Ende dieses Tutorials.

- <strong>Fügen Sie diese drei Spalten in einer neuen Tabelle in der Datenbank und eine direkte Beziehung zwischen dieser Tabelle herzustellen und</strong><strong>`aspnet_Users`</strong><strong>.</strong> Dieser Ansatz etwas mehr Aufwand als mit dem Profil-Framework umfasst, aber es bietet maximale Flexibilität wie die zusätzlichen Eigenschaften in der Datenbank erstellt werden. Dies ist die Option, die wir in diesem Tutorial verwenden.

Wir erstellen eine neue Tabelle namens `UserProfiles` der home-Stadt, Startseite und Signatur für jeden Benutzer gespeichert. Mit der rechten Maustaste auf den Ordner für Tabellen im Datenbank-Explorer-Fenster, und wählen Sie zum Erstellen einer neuen Tabelle. Den Namen der ersten Spalte `UserId` und legen Sie deren Typ auf `uniqueidentifier`. Nicht zulassen `NULL` Werte ein, und markieren Sie die Spalte als Primärschlüssel. Fügen Sie Spalten mit dem Namen: `HomeTown` des Typs `nvarchar(50)`; `HomepageUrl` des Typs `nvarchar(100)`; und die Signatur des Typs `nvarchar(500)`. Jede dieser drei Spalten lässt eine `NULL` Wert.


[![Erstellen Sie in der UserProfiles-Tabelle](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Abbildung 6**: Erstellen der `UserProfiles` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image18.png))


Speichern Sie die Tabelle, und nennen Sie sie `UserProfiles`. Einrichten und schließlich eine fremdschlüsseleinschränkung zwischen der `UserProfiles` tabellenspezifischen `UserId` Feld und die `aspnet_Users.UserId` Feld. Wie wir mit der foreign Key-Einschränkung zwischen der `GuestbookComments` und `aspnet_Users` Tabellen vorhanden sind, diese Einschränkung kaskadiert werden gelöscht. Da die `UserId` Feld `UserProfiles` ist der primäre Schlüssel, dadurch wird sichergestellt, dass es wird nicht mehr als einem Datensatz in die `UserProfiles` Tabelle für jedes Benutzerkonto. Diese Art von Beziehung wird als 1: 1 bezeichnet.

Nun, da wir das Datenmodell erstellt haben, können wir ihn verwenden. In den Schritten 2 und 3 betrachten wir der derzeit angemeldete Benutzer kann wie anzeigen und bearbeiten Sie ihre Startseite Stadt, Startseite und Signatur-Informationen. In Schritt 4 erstellen wir die Schnittstelle für authentifizierte Benutzer neue Kommentare zu den Gästebuch übermitteln, und zeigen die vorhandenen.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Schritt 2: Anzeigen von Home-Stadt, Startseite und Signatur des Benutzers

Es gibt verschiedene Arten der derzeit angemeldeten Benutzer anzeigen und bearbeiten seine home Informationen zur Stadt, Startseite und Signatur aus. Wir konnten die Benutzeroberfläche manuell erstellen, mit Textfeld, und Label-Steuerelemente oder wir können eines der Web-Steuerelemente, z. B. DetailsView-Steuerelement. Um die Datenbank ausführen `SELECT` und `UPDATE` -Anweisungen geschrieben werden, um ADO.NET programmieren Sie in unserer Seite des Code-Behind-Klasse, oder verwenden Sie alternativ einen deklarativen Ansatz mit dem SqlDataSource-Steuerelement. Im Idealfall würde die Anwendung eine geschichtete Architektur, enthalten, die wir von der Seite Code-Behind-Klasse oder deklarativ über das ObjectDataSource-Steuerelement programmgesteuert entweder aufrufen kann.

Da dieser tutorialreihe Formular-Authentifizierung, Autorisierung, Benutzerkonten und Rollen geht fallen nicht in eine gründliche Besprechung diese verschiedene Datenzugriffsoptionen oder warum eine geschichtete Architektur bevorzugt wird, über die SQL-Anweisungen direkt ausführen. von der ASP.NET-Seite. Ich möchte mit einem DetailsView und SqlDataSource-Steuerelement – die schnellste und einfachste Option – durchlaufen, aber erörterten Konzepte können sicherlich zu alternativen Web Steuerelemente und Logik für den Datenzugriff angewendet werden. Weitere Informationen zum Arbeiten mit Daten in ASP.NET finden Sie in meinem *[arbeiten mit Daten in ASP.NET 2.0](../../data-access/index.md)* Reihe von Lernprogrammen.

Öffnen der `AdditionalUserInfo.aspx` auf der Seite die `Membership` Ordner, und fügen Sie einem DetailsView-Steuerelement zum Festlegen der ID-Eigenschaft auf der Seite `UserProfile` und Beseitigen der `Width` und `Height` Eigenschaften. Erweitern Sie DetailsViews-Smarttag, und an eine neue Datenquellen-Steuerelement bindet, bindet es auch. Dadurch wird der DataSource-Konfigurations-Assistent gestartet (siehe Abbildung 7). Im ersten Schritt aufgefordert, den Typ der Datenquelle an. Da wir beabsichtigen, eine direkte Verbindung mit der `SecurityTutorials` Datenbank, wählen Sie die Datenbank-Symbol, Angeben der `ID` als `UserProfileDataSource`.


[![Fügen Sie ein neues SqlDataSource-Steuerelement, das mit dem Namen UserProfileDataSource hinzu.](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Abbildung 7**: Hinzufügen einer neuen SqlDataSource-Steuerelement mit dem Namen `UserProfileDataSource` ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image21.png))


Im nächste Bildschirm werden aufgefordert, die zu verwendende Datenbank. Wir haben bereits eine Verbindungszeichenfolge im definiert `Web.config` für die `SecurityTutorials` Datenbank. Diesen Verbindungszeichenfolgennamen – `SecurityTutorialsConnectionString` – sollte sich in der Dropdown-Liste. Wählen Sie diese Option aus, und klicken Sie auf Weiter.


[![Wählen Sie aus der Dropdown-Liste SecurityTutorialsConnectionString](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Abbildung 8**: Wählen Sie `SecurityTutorialsConnectionString` aus der Dropdown-Liste ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image24.png))


Im nachfolgenden Bildschirm wird gefragt, Tabellen und Spalten zu Abfragen angeben. Wählen Sie die `UserProfiles` Tabelle aus der Dropdown-Liste, und überprüfen Sie alle Spalten.


[![Schalten Sie alle Spalten aus der UserProfiles-Tabelle zurück](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Abbildung 9**: der Spalten aus den Wert bringen wieder alle die `UserProfiles` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image27.png))


In Abbildung 9: Gibt die aktuelle Abfrage *alle* der Datensätze in `UserProfiles`, aber interessieren wir uns nur des aktuell angemeldeten Benutzers Datensatz. Hinzufügen einer `WHERE` -Klausel, klicken Sie auf die `WHERE` Schaltfläche, um das Hinzufügen `WHERE` Klausel Dialogfeld (siehe Abbildung 10). Hier können Sie die Spalte, nach dem gefiltert, den Operator und die Quelle des Filter-Parameter auswählen. Wählen Sie `UserId` als Spalte und den Operator "=".

Leider besteht keine integrierte Parameter auf diese Datenquelle des aktuell angemeldeten Benutzers zurück `UserId` Wert. Sie müssen diesen Wert programmgesteuert abrufen. Legen Sie daher die Source-Dropdownliste auf "None," klicken Sie hinzufügen, Schaltfläche, um den Parameter hinzuzufügen, und klicken Sie dann auf OK.


[![Fügen Sie einen Filterparameter für die Spalte "UserID" hinzu.](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Abbildung 10**: Fügen Sie einen Filter-Parameter auf die `UserId` Spalte ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image30.png))


Nach dem Klicken auf OK werden Sie auf dem Bildschirm in Abbildung 9 gezeigt zurückgegeben. Dieses Mal jedoch die SQL-Abfrage am unteren Rand des Bildschirms sollte enthalten eine `WHERE` Klausel. Klicken Sie auf "Weiter", um zum Bildschirm "Testabfrage" zu navigieren, auf. Hier können Sie die Abfrage ausführen und die Ergebnisse anzuzeigen. Klicken Sie auf "Fertig stellen", um den Assistenten abzuschließen.

Nach Abschluss der DataSource-Konfigurations-Assistent, erstellt Visual Studio das SqlDataSource-Steuerelement auf Grundlage der Einstellungen im Assistenten angegeben. Darüber hinaus manuell hinzugefügt BoundFields DetailsView für jede Spalte, die von dem SqlDataSource-Steuerelement zurückgegebenen `SelectCommand`. Besteht keine Notwendigkeit zum Anzeigen der `UserId` Feld DetailsView, da der Benutzer nicht diesen Wert kennen muss. Sie können dieses Feld direkt über das DetailsView-Steuerelement deklaratives Markup entfernen oder durch Klicken auf "Bearbeiten-Felder" aus der Smarttag.

An diesem Punkt sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

Wir müssen des SqlDataSource-Steuerelements programmgesteuert festlegen `UserId` Parameter des aktuell angemeldeten Benutzers `UserId` bevor die Daten ausgewählt werden. Dies kann erreicht werden, indem Sie einen Ereignishandler für dem SqlDataSource-Steuerelement erstellen `Selecting` Ereignis- und Hinzufügen des folgenden code gibt es:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Der obige Code startet durch Abrufen der einen Verweis auf den derzeit angemeldeten Benutzer durch Aufrufen der `Membership` Klasse `GetUser` Methode. Dies gibt eine `MembershipUser` Objekt, dessen `ProviderUserKey` Eigenschaft enthält die `UserId`. Die `UserId` Wert wird anschließend dem SqlDataSource-Steuerelement zugewiesen `@UserId` Parameter.

> [!NOTE]
> Die `Membership.GetUser()` Methode gibt Informationen zu den derzeit angemeldeten Benutzer zurück. Wenn ein anonymer Benutzer die Seite besucht, wird der Wert zurückgegeben `Nothing`. In diesem Fall führt dies zu einem `NullReferenceException` in der folgenden Zeile des Codes beim Lesen der `ProviderUserKey` Eigenschaft. Natürlich nicht schon kümmern `Membership.GetUser()` zurückgeben "Nothing" in der `AdditionalUserInfo.aspx` Seite, da wir URL-Autorisierung in einem vorherigen Tutorial so konfiguriert, dass nur authentifizierte Benutzer die ASP.NET-Ressourcen in diesem Ordner zugreifen können. Wenn Sie müssen den Zugriff auf Informationen über den derzeit angemeldeten Benutzer auf einer Seite, in denen anonymer Zugriff zulässig ist, müssen Sie überprüfen, ob die `MembershipUser` Objekt, das von der `GetUser()` Methode ist nicht "Nothing" vor dem verweisen auf die Eigenschaften.


Wenn Sie besuchen die `AdditionalUserInfo.aspx` Seite über einen Browser werden Sie eine leere Seite angezeigt, da wir noch, der alle Zeilen hinzugefügt haben die `UserProfiles` Tabelle. In Schritt 6 sehen wir uns Gewusst wie: Anpassen des Steuerelements CreateUserWizard, um automatisch eine neue Zeile hinzuzufügen der `UserProfiles` Tabelle, wenn ein neues Benutzerkonto erstellt wird. Jetzt jedoch müssen wir einen Datensatz in der Tabelle manuell zu erstellen.

Navigieren Sie zu der Datenbank-Explorer in Visual Studio, und erweitern Sie den Ordner "Tabellen". Mit der rechten Maustaste auf die `aspnet_Users` Tabelle und wählen Sie "Tabellendaten anzeigen", um die Datensätze in der Tabelle anzuzeigen, führen Sie dieselben Schritte für die `UserProfiles` Tabelle. Abbildung 11 zeigt diese Ergebnisse, wenn vertikal angeordnet. In meiner Datenbank stehen derzeit `aspnet_Users` Datensätze für Bruce, Fred und Tito, jedoch keine Datensätze in der `UserProfiles` Tabelle.


[![Der Inhalt der Aspnet_Users und UserProfiles-Tabellen werden angezeigt.](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Abbildung 11**: Inhalt der `aspnet_Users` und `UserProfiles` Tabellen werden angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image33.png))


Fügen Sie einen neuen Datensatz in die `UserProfiles` Tabelle durch manuelles Eingeben von Werten für die `HomeTown`, `HomepageUrl`, und `Signature` Felder. Die einfachste Möglichkeit, eine gültige erhalten `UserId` Wert in der neuen `UserProfiles` Datensatz ist die Auswahl der `UserId` aus einem bestimmten Benutzerkonto in der `aspnet_Users` Tabelle kopieren und fügen Sie ihn in die `UserId` Feld `UserProfiles`. Abbildung 12 zeigt die `UserProfiles` Tabelle, nachdem Sie ein neuer Datensatz für Bruce hinzugefügt wurde.


[![Ein Datensatz wurde auf UserProfiles für Bruce hinzugefügt.](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Abbildung 12**: A-Datensatz wurde hinzugefügt, um `UserProfiles` für Bruce ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image36.png))


Wechseln Sie zurück zur der `AdditionalUserInfo.aspx page`, angemeldet als Bruce. Wie in Abbildung 13 gezeigt, werden Bruces Einstellungen angezeigt.


[![Die zurzeit Zugriff auf Benutzer ist His Einstellungen angezeigt](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Abbildung 13**: die zurzeit Zugriff auf Benutzer ist His Einstellungen gezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image39.png))


> [!NOTE]
> Wechseln Sie nun auch manuell hinzufügen Datensätze in der `UserProfiles` Tabelle für jeden Benutzer der Mitgliedschaft. In Schritt 6 sehen wir uns Gewusst wie: Anpassen des Steuerelements CreateUserWizard, um automatisch eine neue Zeile hinzuzufügen der `UserProfiles` Tabelle, wenn ein neues Benutzerkonto erstellt wird.


## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Schritt 3: Ermöglicht die Benutzer, seine Homepage Stadt, Startseite und Signatur bearbeiten

Der derzeit angemeldete Benutzer kann an diesem Punkt anzeigen, deren home Stadt, Startseite und Signatur-Einstellung, aber sie nicht noch geändert werden können. Aktualisieren wir DetailsView-Steuerelement, sodass die Daten bearbeitet werden können.

Das erste, was erforderlich ist, ist, fügen eine `UpdateCommand` für dem SqlDataSource-Steuerelement, Angeben der `UPDATE` auszuführende Anweisung und die entsprechenden Parameter. Wählen Sie dem SqlDataSource-Steuerelement, und klicken Sie im Eigenschaftenfenster auf die Auslassungszeichen neben der UpdateQuery-Eigenschaft, um das Dialogfeld Befehls- und Parameter-Editor zu öffnen. Geben Sie den folgenden `UPDATE` Anweisung in das Textfeld ein:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Klicken Sie dann auf die Schaltfläche "Parameter aktualisieren", die einen Parameter in des SqlDataSource-Steuerelements erstellen `UpdateParameters` Auflistung für jeden Parameter in der `UPDATE` Anweisung. Lassen Sie die Quelle für alle festgelegtem Parameter auf "None", und klicken Sie auf die Schaltfläche "OK", um das Dialogfeld zu schließen.


[![Geben Sie dem SqlDataSource-Steuerelement UpdateCommand und UpdateParameters](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Abbildung 14**: Geben Sie dem SqlDataSource-Steuerelement `UpdateCommand` und `UpdateParameters` ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image42.png))


Aufgrund der Ergänzungen, die wir an dem SqlDataSource-Steuerelement, das DetailsView-Steuerelement jetzt bearbeiten unterstützen, kann vorgenommen haben. Aus DetailsViews-Smarttag das Kontrollkästchen Sie "Bearbeiten aktivieren". Dadurch wird eine CommandField des Steuerelements hinzugefügt `Fields` Sammlung mit der `ShowEditButton` -Eigenschaft auf "true" festgelegt. Dies rendert eine Bearbeiten-Schaltfläche, wenn DetailsView in nur-Lese Modus und Update angezeigt wird, und Schaltflächen "Abbrechen", bei der Anzeige im Bearbeitungsmodus befindet. Anstatt dass sich der Benutzer auf "Bearbeiten" klicken, jedoch, wir haben die renderschaltfläche DetailsView "immer" bearbeitbar durch Festlegen der DetailsView-Steuerelement [ `DefaultMode` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) zu `Edit`.

Infolge dieser Änderungen werden sollte Ihre DetailsView-Steuerelement deklaratives Markup etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Beachten Sie das Hinzufügen der CommandField und `DefaultMode` Eigenschaft.

Fahren Sie fort, und Testen Sie diese Seite über einen Browser. Wenn einem Benutzer Zugriff auf, die einen entsprechenden Datensatz in `UserProfiles`, werden die Einstellungen des Benutzers in einer bearbeitbaren Schnittstelle angezeigt.


[![Die DetailsView rendert eine bearbeitbare-Schnittstelle](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Abbildung 15**: DetailsView rendert eine bearbeitbare Schnittstelle ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image45.png))


Wiederholen Sie die Werte ändern, und klicken Sie auf die Schaltfläche "Aktualisieren". Es wird angezeigt, als würde nichts passieren. Wird ein Postback und die Werte in der Datenbank gespeichert sind, aber es existiert keine visuelle Bestätigung, die der Speichervorgang aufgetreten sind.

Klicken Sie zur Behebung des Problems, zurück zu Visual Studio, und fügen Sie ein Label-Steuerelement oben DetailsView. Festlegen der `ID` zu `SettingsUpdatedMessage`, dessen `Text` Eigenschaft "haben die Einstellungen aktualisiert wurden," und die zugehörige `Visible` und `EnableViewState` Eigenschaften, die `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

Wir müssen zum Anzeigen der `SettingsUpdatedMessage` bezeichnen, bei jeder Aktualisierung die DetailsView. Um dies zu erreichen, erstellen Sie einen Ereignishandler für die DetailsView `ItemUpdated` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Wechseln Sie zurück zur der `AdditionalUserInfo.aspx` Seite über einen Browser, und aktualisieren Sie die Daten. Dieses Mal ist eine hilfreiche Statusmeldung angezeigt.


[![Eine kurze Nachricht wird angezeigt, wenn die Einstellungen werden aktualisiert.](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Abbildung 16**: eine kurze Nachricht wird angezeigt, wenn die Einstellungen aktualisiert werden ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image48.png))


> [!NOTE]
> Das DetailsView-Steuerelement die Schnittstelle lässt viel zu wünschen übrig bearbeiten. Textfelder Standardformaten verwendet sollten, aber das Signaturfeld wahrscheinlich einem mehrzeiligen Textfeld. Eine RegularExpressionValidator sollte verwendet werden, um sicherzustellen, dass die URL der Startseite, wenn eingegeben haben, mit "http://" oder "https://" beginnt. Darüber hinaus seit der DetailsView-Steuerelement hat seine `DefaultMode` -Eigenschaft auf festgelegt `Edit`, "Abbrechen"-Schaltfläche nicht alles. Sie sollten entweder entfernt oder, wenn geklickt haben, leiten Sie den Benutzer auf eine andere Seite (z. B. `~/Default.aspx`). Ich lassen Sie diese Erweiterungen als Übung für den Leser.


### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Hinzufügen eines Links zu den`AdditionalUserInfo.aspx`Seite auf der Masterseite

Die Website bietet derzeit kein Links zu den `AdditionalUserInfo.aspx` Seite. Die einzige Möglichkeit, dorthin zu gelangen, ist die URL der Seite direkt in die Adressleiste des Browsers eingeben. Fügen Sie einen Link zu dieser Seite in der `Site.master` Masterseite.

Denken Sie daran, dass die Masterseite ein LoginView-Steuerelement in enthält die `LoginContent` ContentPlaceHolder, in dem anderes Markup authentifiziert und anonym Besucher angezeigt. Aktualisieren Sie des LoginView-Steuerelements `LoggedInTemplate` einen Link zum Einschließen der `AdditionalUserInfo.aspx` Seite. Nach diesen Änderungen der LoginView sollte deklaratives Markup des Steuerelements etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Beachten Sie das Hinzufügen der `lnkUpdateSettings` HyperLink-Steuerelement, um die `LoggedInTemplate`. Mit diesem Link vorhanden ist können authentifizierte Benutzer schnell zur Seite anzeigen und ändern ihre Startseite Stadt, Startseite und Signatur-Einstellungen wechseln.

## <a name="step-4-adding-new-guestbook-comments"></a>Schritt 4: Hinzufügen neuer Gästebuch-Kommentare

Die `Guestbook.aspx` Seite ist, in dem authentifizierte Benutzern das Gästebuch anzeigen und einen Kommentar hinterlassen können. Erstellen wir zunächst die Schnittstelle, um neue Gästebuch Kommentare hinzufügen.

Öffnen der `Guestbook.aspx` Eigenschaftenseite in Visual Studio und erstellen Sie eine Benutzeroberfläche, bestehend aus zwei TextBox-Steuerelemente, eine für den neuen Kommentar Betreff und einen für Text. Legen Sie des ersten Textfeld-Steuerelements `ID` Eigenschaft `Subject` und die zugehörige `Columns` Eigenschaft auf 40: zweiten festgelegt ist `ID` zu `Body`, dessen `TextMode` zu `MultiLine`, und die zugehörige `Width` und `Rows` Eigenschaften, die "95 %" und 8 bzw. Fügen Sie zum Abschließen der Benutzeroberfläche mit dem Namen ein Websteuerelements auf Schaltfläche `PostCommentButton` und legen Sie dessen `Text` Eigenschaft "Nach der Kommentar".

Da jeder Gästebuch Kommentar Betreff und den Nachrichtentext erforderlich ist, fügen Sie ein RequiredFieldValidator für jeden der TextBox-Elemente hinzu. Legen Sie die `ValidationGroup` Eigenschaft dieser Steuerelemente zu "EnterComment"; legen fest, das die `PostCommentButton` des Steuerelements `ValidationGroup` Eigenschaft auf "EnterComment". Weitere Informationen zu ASP. NET Steuerelementen zur gültigkeitsprüfung, sehen Sie sich [Formularvalidierung in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [Analyse der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx), und die [Überprüfung Server Steuerelemente Lernprogramm](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) auf [W3Schools](http://www.w3schools.com/).

Nach dem Erstellen der Benutzeroberfläche sollte deklaratives Markup Ihrer Seite etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Mit der Benutzeroberfläche abgeschlossen ist, ist unser Nächstes zum Einfügen eines neuen Datensatzes in die `GuestbookComments` Tabelle, wenn die `PostCommentButton` geklickt wird. Dies kann auf verschiedene Weise erreicht werden: ADO.NET-Code schreiben wir in der Schaltfläche `Click` -Ereignishandler, es kann ein SqlDataSource-Steuerelement auf der Seite hinzufügen, konfigurieren Sie die `InsertCommand`, und rufen Sie dann die `Insert` Methode aus der `Click` Ereignis -Handler. oder wir erstellen eine mittlere Ebene, die verantwortlich für das Einfügen von neuen Gästebuch Kommentare war konnte, und rufen Sie diese Funktion von der `Click` -Ereignishandler. Da wir haben uns über die Verwendung von einem SqlDataSource-Steuerelement in Schritt 3, verwenden Sie ADO.NET-Code wir hier.

> [!NOTE]
> Die Klassen von ADO.NET verwendet, um programmgesteuert auf Daten aus einer Microsoft SQL Server-Datenbank zugreifen befinden sich in der `System.Data.SqlClient` Namespace. Möglicherweise müssen Sie diesen Namespace in Ihrer Seite Code-Behind-Klasse importieren (d. h. `Imports System.Data.SqlClient`).


Erstellen Sie einen Ereignishandler für die `PostCommentButton`des `Click` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

Die `Click` Ereignishandler startet, indem Sie überprüfen, dass die vom Benutzer bereitgestellten Daten gültig ist. Wenn sie nicht der Fall ist, wird der Ereignishandler vor dem Einfügen eines Datensatzes beendet. Sofern die bereitgestellten Daten sind gültig, des aktuell angemeldeten Benutzers `UserId` Wert abgerufen und gespeichert, der `currentUserId` lokale Variable. Dieser Wert ist erforderlich, da wir angeben, müssen eine `UserId` Wert beim Einfügen eines Datensatzes wird in `GuestbookComments`.

Danach die Verbindungszeichenfolge für die `SecurityTutorials` Datenbank wird vom abgerufen `Web.config` und `INSERT` SQL-Anweisung angegeben ist. Ein `SqlConnection` Objekt wird dann erstellt und geöffnet. Als Nächstes wird ein `SqlCommand` Objekt wird erstellt und die Werte für die Parameter in verwendet die `INSERT` zugewiesen sind. Die `INSERT` Anweisung anschließend ausgeführt wird, und die Verbindung geschlossen. Am Ende des ereignishandlers der `Subject` und `Body` Textfelder `Text` Eigenschaften gelöscht werden, damit der Benutzer Werte für das Postback nicht dauerhaft gespeichert werden.

Fahren Sie fort, und Testen Sie diese Seite in einem Browser. Da auf dieser Seite wird die `Membership` Ordner kann nicht für anonyme Besucher zugänglich sind. Aus diesem Grund müssen Sie zuerst melden Sie sich (falls Sie noch nicht getan haben). Geben Sie einen Wert in der `Subject` und `Body` Textfelder ein, und klicken Sie auf die `PostCommentButton` Schaltfläche. Dadurch wird einen neuen Datensatz hinzugefügt werden `GuestbookComments`. Beim Postback werden Betreff und Text, die Sie angegeben haben, in die Textfelder ein zurückgesetzt.

Nach dem Klicken auf die `PostCommentButton` Schaltfläche es existiert keine visuelle Bestätigung, die der Kommentar der GuestBook hinzugefügt wurde. Wir müssen immer noch auf dieser Seite zum Anzeigen der vorhandenen Gästebuch-Kommentare, den wir in Schritt 5 ausführen, zu aktualisieren. Nachdem wir dies erreicht wird, wird der gerade hinzugefügten Kommentar in der Liste der Kommentare, Bereitstellen von ausreichend visuellem Feedback angezeigt. Jetzt bestätigen Sie, dass es sich bei Ihren Kommentar Gästebuch gespeichert wurde, den Inhalt der dem `GuestbookComments` Tabelle.

Abbildung 17 zeigt den Inhalt der `GuestbookComments` Tabelle nach wurden zwei Kommentare hinterlassen haben.


[![Sie können die Gästebuch-Kommentare in der Tabelle GuestbookComments sehen.](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Abbildung 17**: sehen Sie die Gästebuch-Kommentare in der `GuestbookComments` Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image51.png))


> [!NOTE]
> Gefährliche Markup – z. B. HTML – ASP.NET wird ausgelöst, wenn ein Benutzer versucht, einen Kommentar Gästebuch einzufügen, die potenziell enthält ein `HttpRequestValidationException`. Weitere Informationen finden Sie über diese Ausnahme an, warum es ausgelöst wird, und übermitteln potenziell gefährlicher Werte durch Benutzer zulassen, finden Sie in der [anfordern Überprüfung Whitepaper](../../../../whitepapers/request-validation.md).


## <a name="step-5-listing-the-existing-guestbook-comments"></a>Schritt 5: Auflisten der vorhandenen Gästebuch-Kommentare

Zusätzlich zu kommentieren, von einem Benutzer Zugriff auf die `Guestbook.aspx` Seite sollte auch in der Lage, das Gästebuch des vorhandenen Kommentare anzuzeigen. Um dies zu erreichen, fügen Sie ein ListView-Steuerelement namens `CommentList` zum unteren Rand der Seite.

> [!NOTE]
> Das ListView-Steuerelement ist neu in ASP.NET Version 3.5. Es dient zum Anzeigen einer Liste von Elementen in einem sehr anpassbar und flexiblen Layout, aber dennoch bieten integrierte bearbeiten, einfügen, löschen, paging und Sortieren von Funktionen wie GridView. Wenn Sie ASP.NET 2.0 verwenden, müssen Sie stattdessen das DataList oder Repeater-Steuerelement. Weitere Informationen zur Verwendung der ListView finden Sie unter [Scott Guthrie](https://weblogs.asp.net/scottgu/)des Blogeintrag [die Asp: ListView-Steuerelement](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx), und in meinem Artikel [Anzeigen von Daten mit dem ListView-Steuerelement](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).


Öffnen Sie das ListView Smarttag und aus der Datenquelle auswählen Dropdown-Liste, die binden Sie das Steuerelement an eine neue Datenquelle. Wie in Schritt2 beschrieben, wird dadurch der Konfigurations-Assistent gestartet. Wählen Sie das Datenbanksymbol, nennen Sie die resultierende SqlDataSource `CommentsDataSource`, und klicken Sie auf OK. Wählen Sie als Nächstes die `SecurityTutorialsConnectionString` Verbindung, die Zeichenfolge, aus der Dropdown Liste, und klicken Sie auf Weiter.

An diesem Punkt in Schritt2 wir angegeben die Daten in der Abfrage durch Auswählen der `UserProfiles` Tabelle aus der Dropdown-Liste, und wählen Sie die Spalten zurückgegeben (siehe Abbildung 9). Diesmal allerdings möchten wir eine SQL-Anweisung zu erstellen, der nicht nur die Datensätze aus wieder abruft `GuestbookComments`, aber auch die Kommentierender des home-Stadt, Startseite, Signatur und einen Benutzernamen. Aus diesem Grund aktivieren Sie das Optionsfeld "Geben Sie eine benutzerdefinierte SQL-Anweisung oder gespeicherte Prozedur", und klicken Sie auf Weiter.

Hierdurch wird der Bildschirm "Definieren Sie benutzerdefinierte Anweisungen oder gespeicherte Prozeduren" angezeigt. Klicken Sie auf die Schaltfläche Abfrage-Generator, um die Abfrage grafisch zu erstellen. Zunächst, dass die Abfrage-Generator werden Sie aufgefordert, die Tabellen angeben, die, denen wir von Abfragen möchten. Wählen Sie die `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen aus, und klicken Sie auf OK. Dadurch wird alle drei Tabellen auf der Entwurfsoberfläche hinzugefügt. Da es sich um foreign Key-Einschränkungen für die `GuestbookComments`, `UserProfiles`, und `aspnet_Users` enthalten sind, den Abfrage-Generator automatisch `JOIN` s Diese Tabellen.

Alles, was bleibt dann an die Spalten zurückgegeben. Von der `GuestbookComments` Tabelle die `Subject`, `Body`, und `CommentDate` Spalten; die Rückgabe der `HomeTown`, `HomepageUrl`, und `Signature` Spalten aus der `UserProfiles` Tabelle; und zurückgeben `UserName` aus `aspnet_Users`. Darüber hinaus hinzufügen "`ORDER BY CommentDate DESC`" am Ende der `SELECT` abzufragen, damit die neuesten Beiträge zuerst zurückgegeben werden. Treffen Sie die Auswahl, und sieht Ihre Benutzeroberfläche des Abfragegenerators ähnlich wie im Screenshot in Abbildung 18.


[![Verknüpft die Abfrage erstellt, die GuestbookComments, UserProfiles und Aspnet_Users Tabellen](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Abbildung 18**: die erstellte Abfrage `JOIN` s der `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image54.png))


Klicken Sie auf OK, um das Abfrage-Generator-Fenster zu schließen und zurück auf dem Bildschirm "Definieren Sie benutzerdefinierte Anweisungen oder gespeicherte Prozeduren". Klicken Sie auf Weiter, um zum Fenster "Test-Query", in dem Sie die Ergebnisse der Abfrage anzeigen können, indem Sie auf die Schaltfläche "Testen von Abfragen". Wenn Sie bereit sind, klicken Sie auf "Fertig stellen", um das Konfigurieren von Datenquellen-Assistenten zu beenden.

Wenn wir den Konfigurieren von Datenquellen-Assistenten in Schritt2 das zugeordnete DetailsView-Steuerelement des abgeschlossen `Fields` Sammlung wurde aktualisiert, um eine BoundField für jede Spalte, die vom enthalten die `SelectCommand`. Die ListView bleibt jedoch unverändert; Wir müssen auch das Layout zu definieren. Das ListView Layouts kann manuell über die deklarativen Markup oder über die Option "ListView konfigurieren" im entsprechenden Smart Tag erstellt werden. Ich in der Regel lieber das Markup manuell definieren, aber verwenden die geeignete Weise naheliegendste für Sie ist.

Schließlich habe ich noch mit den folgenden `LayoutTemplate`, `ItemTemplate`, und `ItemSeparatorTemplate` für meine ListView-Steuerelement:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

Die `LayoutTemplate` das Markup definiert zwar vom Steuerelement ausgegeben, die `ItemTemplate` jedes Element zurückgegeben wird, von dem SqlDataSource-Steuerelement rendert. Die `ItemTemplate`der daraus resultierende Markup befindet sich der `LayoutTemplate`des `itemPlaceholder` Steuerelement. Zusätzlich zu den `itemPlaceholder`, `LayoutTemplate` umfasst eine DataPager-Steuerelement, das schränkt die ListView auf nur 10 Gästebuch Kommentare pro Seite (Standard) auf und rendert eine Paging-Schnittstelle.

Meine `ItemTemplate` zeigt jede Gästebuch des Kommentars Betreff in einer `<h4>` Element mit dem Text unterhalb der Antragsteller. Beachten Sie, dass die Syntax für das Anzeigen von Text verwendet die vom zurückgegebenen Daten die `Eval("Body")` Databinding-Anweisung in eine Zeichenfolge konvertiert und ersetzt Zeilenumbrüche mit der `<br />` Element. Diese Konvertierung ist erforderlich, um die Zeilenumbrüche eingegeben, wenn Sie den Kommentar zu übermitteln, da Leerzeichen, von HTML ignoriert wird anzuzeigen. Die Signatur wird angezeigt, unter der Text in Kursivdruck, gefolgt von der Startseite des Benutzers Stadt, die einen Link zu seine Homepage, das Datum und Uhrzeit, die der Kommentar vorgenommen wurde und der Benutzername der Person, die den Kommentar hinterlassen.

Nehmen Sie einen Moment Zeit, um die Seite über einen Browser anzuzeigen. Die Kommentare, die das Gästebuch in Schritt 5, die hier angezeigten hinzugefügt wurden, sollte angezeigt werden.


[![GuestBook.aspx nun zeigt das Gästebuch des Kommentare an.](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Abbildung 19**: `Guestbook.aspx` zeigt nun die Gästebuch Kommentaren ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image57.png))


Versuchen Sie, das Gästebuch einen neuen Kommentar hinzugefügt. Nach dem Klicken auf die `PostCommentButton` Daten zurückgegeben, die Seite-Schaltfläche und der Kommentar wird hinzugefügt, mit der Datenbank, aber das ListView-Steuerelement nicht aktualisiert, um den neuen Kommentar anzuzeigen. Dies kann entweder behoben werden:

- Aktualisieren der `PostCommentButton` Schaltfläche `Click` -Ereignishandler so, dass die It des ListView-Steuerelements ruft `DataBind()` Methode nach dem Einfügen von neuen Kommentars in der Datenbank, oder
- Festlegen des ListView-Steuerelements `EnableViewState` Eigenschaft `False`. Dieser Ansatz funktioniert, da durch Deaktivierung von Ansichtszustand des Steuerelements, an der zugrunde liegenden Daten bei jedem Postback binden müssen.

Die Tutorial Website, die von diesem Tutorial wird veranschaulicht, beide Verfahren. Des ListView-Steuerelements `EnableViewState` Eigenschaft `False` und der erforderlichen Code zum Binden Sie die Daten in der ListView programmgesteuert erneut in vorhanden ist die `Click` -Ereignishandler, aber auskommentiert ist.

> [!NOTE]
> Derzeit den `AdditionalUserInfo.aspx` ermöglicht es dem Benutzer zum Anzeigen und bearbeiten Sie die Startseite Stadt, Startseite und Signatur-Einstellungen. Es kann sein, zu aktualisieren `AdditionalUserInfo.aspx` anzuzeigenden des angemeldeten Benutzers Gästebuch Kommentare. D. h. zusätzlich zu untersuchen und seine Daten ändern, ein Benutzer finden das `AdditionalUserInfo.aspx` Seite, um festzustellen, welche Gästebuch Kommentare in der Vergangenheit getroffen wird. Ich lassen Sie dieses als Übung für den interessierten Leser.


## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Schritt 6: Anpassen des Steuerelements CreateUserWizard, um eine Schnittstelle für die Home-Stadt, Startseite und Signatur enthalten.

Die `SELECT` Abfrage ein, die die `Guestbook.aspx` verwendet eine `INNER JOIN` , kombinieren die verknüpften Datensätze für die `GuestbookComments`, `UserProfiles`, und `aspnet_Users` Tabellen. Wenn ein Benutzer, die keine Einträge in `UserProfiles` ein Gästebuch Kommentar, der Kommentar wird nicht in der ListView angezeigt werden, da die `INNER JOIN` gibt nur `GuestbookComments` Einträge beim stehen die übereinstimmenden Datensätze in `UserProfiles` und `aspnet_Users`. Und wie wir in Schritt 3 gesehen haben, wenn ein Benutzer einen Datensatz keinen `UserProfiles` sie nicht anzeigen oder bearbeiten Sie ihre Einstellungen in der `AdditionalUserInfo.aspx` Seite.

Natürlich aufgrund unseres Entwurfs Entscheidungen, die es ist wichtig, dass jedes Benutzerkonto in das Mitgliedschaftssystem ein entsprechendes verfügen. Notieren Sie in der `UserProfiles` Tabelle. Ist für einen entsprechenden Eintrag hinzuzufügende `UserProfiles` , wenn ein neues Mitgliedskonto für Benutzer über die CreateUserWizard erstellt wird.

Siehe die [ *Erstellen von Benutzerkonten* ](creating-user-accounts-vb.md) löst Lernprogramm, nachdem das neue Benutzerkonto für die Mitgliedschaft des Steuerelements CreateUserWizard erstellt wurde die [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Wir erstellen einen Ereignishandler für dieses Ereignis, erhalten die Benutzer-ID für den gerade erstellten Benutzer, und fügen Sie dann einen Datensatz in die `UserProfiles` Tabelle mit den Standardwerten für die `HomeTown`, `HomepageUrl`, und `Signature` Spalten. Ist es möglich, die benutzeraufforderung für diese Werte durch das Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Textfelder enthalten.

Sehen wir uns zunächst an, wie eine neue Zeile hinzufügen der `UserProfiles` -Tabelle in der `CreatedUser` -Ereignishandler mit Standardwerten. Danach sehen wir, Gewusst wie: Anpassen des Steuerelements CreateUserWizard-Benutzeroberfläche, um zusätzliche Felder zum Erfassen des neuen Benutzers home Stadt, Startseite und Signatur enthalten.

### <a name="adding-a-default-row-touserprofiles"></a>Hinzufügen einer Zeile standardmäßig auf`UserProfiles`

In der [ *Erstellen von Benutzerkonten* ](creating-user-accounts-vb.md) Tutorial, die wir, ein Steuerelement CreateUserWizard hinzugefügt der `CreatingUserAccounts.aspx` auf der Seite die `Membership` Ordner. Damit die CreateUserWizard Steuerelement hinzufügen ein Datensatzes zu `UserProfiles` Tabelle nach Benutzerkonto erstellen, müssen wir des Steuerelements CreateUserWizard zu aktualisieren. Anstatt diese Änderungen an der `CreatingUserAccounts.aspx` Seite, fügen Sie ein neues Steuerelement CreateUserWizard, stattdessen `EnhancedCreateUserWizard.aspx` Seite, und stellen Sie die Änderungen für dieses Tutorial ist es.

Öffnen der `EnhancedCreateUserWizard.aspx` Seite in Visual Studio, und ziehen Sie ein Steuerelement CreateUserWizard aus der Toolbox auf der Seite. Festlegen des Steuerelements CreateUserWizard `ID` Eigenschaft `NewUserWizard`. Wie wir unter den [ *Erstellen von Benutzerkonten* ](creating-user-accounts-vb.md) Tutorial, das CreateUserWizards Standard-Benutzeroberfläche fordert den Besucher für die erforderlichen Informationen. Nachdem diese Informationen angegeben wurden, erstellt das Steuerelement intern ein neues Benutzerkonto in das mitgliedschaftsframework alles, ohne uns, dass eine einzige Codezeile schreiben müssen.

Das Steuerelement CreateUserWizard löst eine Anzahl von Ereignissen während der Workflows aus. Nachdem ein Besucher Informationen zu den liefert und das Formular übermittelt, des Steuerelements CreateUserWizard anfänglich ausgelöst wird seine [ `CreatingUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Liegt ein Problem während des Prozesses erstellen die [ `CreateUserError` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ausgelöst wird; jedoch, wenn der Benutzer erfolgreich erstellt wurde, und klicken Sie dann die [ `CreatedUser` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) ausgelöst wird. In der [ *Erstellen von Benutzerkonten* ](creating-user-accounts-vb.md) Tutorial einen Ereignishandler für erstellte die `CreatingUser` Ereignis, um sicherzustellen, dass alle führenden oder nachgestellten Leerzeichen, und der angegebene Benutzernamen nicht enthalten hat die Benutzername wurde nicht an einer beliebigen Stelle im Kennwort angezeigt.

Zum Hinzufügen einer Zeile in der `UserProfiles` Tabelle für den gerade erstellten Benutzer müssen wir erstellen einen Ereignishandler für die `CreatedUser` Ereignis. Mit der Zeit die `CreatedUser` -Ereignis ausgelöst hat, das Benutzerkonto wurde bereits in das mitgliedschaftsframework, ermöglicht uns, die Benutzer-ID-Wert des Kontos abzurufen.

Erstellen Sie einen Ereignishandler für die `NewUserWizard`des `CreatedUser` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Der obige Code ab durch die Benutzer-ID, der das gerade hinzugefügte Benutzerkonto abrufen. Dies erfolgt mithilfe der `Membership.GetUser(username)` Methode zum Zurückgeben von Informationen über einen bestimmten Benutzer, und klicken Sie dann mit der `ProviderUserKey` Eigenschaft, um ihre Benutzer-ID abzurufen. Der vom Benutzer im Steuerelement CreateUserWizard eingegebene Benutzername ist verfügbar, über die [ `UserName` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Als Nächstes wird die Verbindungszeichenfolge abgerufen, von `Web.config` und `INSERT` -Anweisung angegeben. ADO.NET-Objekte instanziiert werden, und der Befehl ausgeführt. Der Code weist eine [ `DBNull` ](https://msdn.microsoft.com/library/system.dbnull.aspx) -Instanz, auf die `@HomeTown`, `@HomepageUrl`, und `@Signature` -Parameter, die Datenbank eingefügt hat `NULL` Werte für die `HomeTown`, `HomepageUrl`, und `Signature` Felder.

Besuchen Sie die `EnhancedCreateUserWizard.aspx` Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto. Anschließend zurück zu Visual Studio, und Untersuchen des Inhalts der `aspnet_Users` und `UserProfiles` Tabellen (wie in Abbildung 12). Daraufhin sollte das neue Benutzerkonto im `aspnet_Users` und einen entsprechenden `UserProfiles` Zeile (mit `NULL` Werte für `HomeTown`, `HomepageUrl`, und `Signature`).


[![Ein neues Benutzerkonto und UserProfiles Datensatz wurden hinzugefügt](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Abbildung 20**: ein neues Benutzerkonto an, und `UserProfiles` Datensatz hinzugefügt wurden ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image60.png))


Nachdem der Besucher seinen neuen Kontoinformationen angegeben und auf die Schaltfläche "Benutzer erstellen" geklickt hat, das Benutzerkonto erstellt wird und eine Zeile hinzugefügt, die `UserProfiles` Tabelle. Die CreateUserWizard zeigt dann die `CompleteWizardStep`, wird eine Erfolgsmeldung und eine Schaltfläche zum Fortfahren angezeigt. Klicken Sie auf die Schaltfläche "Weiter" führt dazu, dass ein Postback handeln, aber keine Aktion ausgeführt, blieb hängen mit den Benutzer bleibt bei der `EnhancedCreateUserWizard.aspx` Seite.

Wir können eine URL für den Benutzer, wenn die Schaltfläche "Weiter", mithilfe des Steuerelements CreateUserWizard geklickt wird senden angeben [ `ContinueDestinationPageUrl` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx). Legen Sie die `ContinueDestinationPageUrl` Eigenschaft "~ / Membership/AdditionalUserInfo.aspx". Dies nimmt den neuen Benutzer `AdditionalUserInfo.aspx`, wobei sie können anzeigen und aktualisieren Sie ihre Einstellungen.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Anpassen der Benutzeroberfläche der CreateUserWizard, die an der Eingabeaufforderung für des neuen Benutzers Home-Stadt, Startseite und Signatur

Standardschnittstelle des Steuerelements CreateUserWizard ist ausreichend, einfach Erstellung Szenarien, in denen nur zentrale Konto Benutzerinformationen wie Benutzernamen, Kennwort und e-Mail-Adresse gesammelt werden muss. Aber was geschieht, wenn wir den Besucher ihre Startseite Stadt, Startseite und Signatur eingeben, während der Erstellung von ihrem Konto aufgefordert möchten? Es ist möglich, Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Informationen bei der Registrierung zu sammeln und diese Informationen kann verwendet werden, der `CreatedUser` -Ereignishandler, um zusätzliche Datensätze in der zugrunde liegenden Datenbank einzufügen.

Das Steuerelement CreateUserWizard erweitert die ASP.NET Assistenten-Steuerelement, das ein Steuerelement ist, die Seitenentwickler eine Reihe von geordneten definieren können `WizardSteps`. Die Assistenten-Steuerelement gerendert wird den aktiven Schritt und bietet eine Navigationsschnittstelle, die den Besucher, bewegen die folgenden Schritte aus ermöglicht. Die Assistenten-Steuerelement ist ideal für das Aufteilen eines langwierigen Tasks in mehreren Schritten. Weitere Informationen zu den Assistenten-Steuerelement, finden Sie unter [erstellen eine schrittweise Anleitung-Benutzeroberfläche mit dem Assistenten-Steuerelement von ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Standard-Markup des Steuerelements CreateUserWizard definiert zwei `WizardSteps`: `CreateUserWizardStep` und `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

Die erste `WizardStep`, `CreateUserWizardStep`, rendert die Schnittstelle, die der Benutzername, Kennwort, e-Mail- und So weiter anfordert. Nachdem der Besucher stellt diese Informationen bereit, und klickt auf "Benutzer erstellen", sehen sie die `CompleteWizardStep`, zeigt die Success-Nachricht und eine Schaltfläche zum fortfahren.

Informationen zum Anpassen des Steuerelements CreateUserWizard-Schnittstelle, um zusätzliche Felder enthalten können:

- <strong>Erstellen Sie eine oder mehrere neue</strong><strong>`WizardStep`</strong><strong>s, um die zusätzlichen Benutzeroberflächenelemente enthalten</strong>. Zum Hinzufügen einer neuen `WizardStep` der CreateUserWizard, klicken Sie auf der "hinzufügen/entfernen `WizardStep` s" Link sein Smarttag zum Starten der `WizardStep` Auflistungs-Editor. Von dort aus können Sie hinzufügen, entfernen oder die Schritte im Assistenten neu anordnen. Dies ist der Ansatz, der für dieses Tutorial verwendet wird.

- <strong>Konvertieren der</strong><strong>`CreateUserWizardStep`</strong><strong>in einer bearbeitbaren</strong><strong>`WizardStep`</strong><strong>.</strong> Dies ersetzt die `CreateUserWizardStep` mit einem äquivalenten `WizardStep` , dessen Markup definiert eine Benutzeroberfläche, die entspricht der `CreateUserWizardStep`"s. Durch die Konvertierung der `CreateUserWizardStep` in einem `WizardStep` können wir die Steuerelemente neu positionieren oder fügen Sie zusätzliche Elemente der Benutzeroberfläche für diesen Schritt. Konvertiert die `CreateUserWizardStep` oder `CompleteWizardStep` in einer bearbeitbaren `WizardStep`, klicken Sie auf "Anpassen von Create User Step" oder "Vollständige Schritt anpassen" Verknüpfen von Smart Tag des Steuerelements.

- **Verwenden Sie eine Kombination der oben genannten beiden Optionen.**

Wichtig zu bedenken ist, dass das Steuerelement CreateUserWizard seine Benutzer-kontoerstellung ausgeführt wird, wenn auf die Schaltfläche "Benutzer erstellen" geklickt wird, innerhalb dessen `CreateUserWizardStep`. Es spielt keine Rolle, wenn zusätzliche `WizardStep` s nach der `CreateUserWizardStep` oder nicht.

Beim Hinzufügen eines benutzerdefinierten `WizardStep` zum Sammeln weiterer Benutzereingaben, das benutzerdefinierte Steuerelement CreateUserWizard `WizardStep` platziert werden kann, vor oder nach der `CreateUserWizardStep`. Wenn es vor dem geht die `CreateUserWizardStep` dann die zusätzliche Benutzereingabe gesammelt, aus dem benutzerdefinierten `WizardStep` steht für die `CreatedUser` -Ereignishandler. Jedoch wenn die benutzerdefinierte `WizardStep` kommt nach `CreateUserWizardStep` klicken Sie dann mit der Zeit die benutzerdefinierte `WizardStep` angezeigt wird das neue Benutzerkonto bereits erstellt wurde und die `CreatedUser` Ereignis bereits ausgelöst.

Abbildung 21 zeigt den Workflow bei der hinzugefügten `WizardStep` steht die `CreateUserWizardStep`. Seit dem Zeitpunkt die weiteren Benutzerinformationen gesammelt wurden die `CreatedUser` -Ereignis ausgelöst wird, müssen wir lediglich ist Update der `CreatedUser` -Ereignishandler zum Abrufen von Eingaben, und verwenden Sie diese für die `INSERT` Parameterwerte-Anweisung (statt `DBNull.Value`).


[![Den CreateUserWizard-Workflow, wenn eine zusätzliche WizardStep der CreateUserWizardStep vorangestellt ist.](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Abbildung 21**: die CreateUserWizard Workflow bei der eine zusätzliche `WizardStep` Precedes der `CreateUserWizardStep` ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image63.png))


Wenn die benutzerdefinierte `WizardStep` befindet sich *nach* der `CreateUserWizardStep`, tritt jedoch der Prozess zur Benutzer erstellen, bevor der Benutzer die Möglichkeit, ihre Startseite Stadt, Startseite oder Signatur eingeben wurde. In diesem Fall muss diese zusätzlichen Informationen in der Datenbank eingefügt werden soll, nachdem das Benutzerkonto erstellt wurde, wie in Abbildung 22 dargestellt.


[![Der CreateUserWizard Workflow nach der CreateUserWizardStep gegebener eine zusätzliche WizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Abbildung 22**: die CreateUserWizard Workflow bei der eine zusätzliche `WizardStep` ist nach der `CreateUserWizardStep` ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image66.png))


Der Workflow, der in Abbildung 22 dargestellt wartet auf den zum Einfügen eines Datensatzes in die `UserProfiles` Tabelle erst nach Abschluss von Schritt2. Wenn ihr Browser nach Schritt 1 von der Besucher geschlossen wird, jedoch wird haben wurde erreicht einen Zustand, in dem ein Benutzerkonto erstellt wurde, aber kein Datensatz wurde hinzugefügt, um `UserProfiles`. Eine problemumgehung besteht darin, einen Datensatz mit `NULL` oder Standardwerte eingefügt `UserProfiles` in die `CreatedUser` Ereignishandler (nach Schritt 1 ausgelöst wird), und diese erfassen, nach dem Abschluss von Schritt 2 aktualisieren. Dadurch wird sichergestellt, dass eine `UserProfiles` Datensatz wird für das Benutzerkonto hinzugefügt werden, auch wenn der Benutzer die Registrierung Prozess während der Ausführung beendet.

In diesem Tutorial erstellen Sie wir ein neues `WizardStep` Vorgang, der nach der `CreateUserWizardStep` aber vor der `CompleteWizardStep`. Wir betrachten rufen Sie zuerst die WizardStep in platzieren und konfiguriert und anschließend den Code.

Smart Tag des Steuerelements CreateUserWizard, wählen Sie in der "hinzufügen/entfernen `WizardStep` s", daraufhin wird die `WizardStep` Auflistungs-Editor-Dialogfeld. Fügen Sie einen neuen `WizardStep`wird durch das Festlegen der `ID` zu `UserSettings`, dessen `Title` zu "Einstellungen" und die zugehörige `StepType` zu `Step`. Positionieren Sie es so, dass es nach geht die `CreateUserWizardStep` ("Sign Up für neues Konto") und vor der `CompleteWizardStep` ("abgeschlossen"), wie in Abbildung 23 dargestellt.


[![Hinzufügen einer neuen WizardStep des Steuerelements CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Abbildung 23**: Hinzufügen ein neuen `WizardStep` an das Steuerelement CreateUserWizard ([klicken Sie, um das Bild in voller Größe anzeigen](storing-additional-user-information-vb/_static/image69.png))


Klicken Sie auf OK, um schließen die `WizardStep` Auflistungs-Editor-Dialogfeld. Die neue `WizardStep` von des Steuerelements CreateUserWizard aktualisierte deklaratives Markup zu sehen ist:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Beachten Sie das neue `<asp:WizardStep>` Element. Wir müssen die Benutzeroberfläche zum Erfassen des neuen Benutzers home Stadt, Startseite und Signatur hier hinzufügen. Sie können diesen Inhalt in der deklarativen Syntax oder mit dem Designer eingeben. Zur Verwendung des Designers, wählen Sie im Schritt "Einstellungen" aus der Dropdown-Liste im Smart Tag, um den Schritt im Designer angezeigt.

> [!NOTE]
> Wählen einen Schritt über das Smarttag-Dropdownlisten-Steuerelement CreateUserWizard aktualisiert [ `ActiveStepIndex` Eigenschaft](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx), der angibt, dass des Indexes des Schritts ab. Wenn Sie dieses Dropdown-Liste verwenden, um den Schritt "Einstellungen" im Designer bearbeiten, achten Sie daher an "SSO für Ihr neues Konto" festlegen, damit dieser Schritt angezeigt wird, wenn der Benutzer zum ersten Mal besuchen der `EnhancedCreateUserWizard.aspx` Seite.


Erstellen einer Benutzeroberfläche, in dem Schritt "Einstellungen", die drei Textfeld-Steuerelemente, die mit dem Namen enthält `HomeTown`, `HomepageUrl`, und `Signature`. Nach dem Erstellen dieser Schnittstelle, sollte die CreateUserWizards deklarative Markup etwa wie folgt aussehen:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Fahren Sie fort, und finden Sie auf dieser Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto, und geben Sie Werte für die home-Stadt, Startseite und Signatur angeben. Nach Abschluss der `CreateUserWizardStep` wird das Benutzerkonto, das in das mitgliedschaftsframework erstellt und die `CreatedUser` -Ereignishandler ausgeführt, wodurch eine neue Zeile hinzugefügt `UserProfiles`, jedoch mit einer Datenbank `NULL` Wert für `HomeTown`, `HomepageUrl`, und `Signature`. Die eingegebenen Werte für die home-Stadt, Startseite und Signatur werden nie verwendet. Das Ergebnis ist ein neues Benutzerkonto mit einem `UserProfiles` aufzeichnen, deren `HomeTown`, `HomepageUrl`, und `Signature` Felder müssen noch angegeben werden.

Wir müssen zum Ausführen von Code nach dem Schritt "Einstellungen", die die home Stadt, Honepage und Signatur Werte, die vom Benutzer eingegebene verwendet und aktualisiert die entsprechende `UserProfiles` Datensatz. Jedes Mal, die der Benutzer wechselt zwischen den Schritten des Assistenten zu steuern, des Assistenten [ `ActiveStepChanged` Ereignis](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) ausgelöst wird. Erstellen wir einen Ereignishandler für dieses Ereignis und das Update der `UserProfiles` Tabelle, wenn der Schritt "Einstellungen" abgeschlossen wurde.

Hinzufügen eines ereignishandlers für das CreateUserWizard des `ActiveStepChanged` Ereignis und fügen Sie den folgenden Code hinzu:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Der obige Code zunächst bestimmen, ob wir einfach den Schritt "Abgeschlossen" erreicht haben. Da der Schritt "Abschließen" unmittelbar nach dem Schritt "Einstellungen" auftritt, wenn der Besucher erreicht Schritt klicken Sie dann "Abgeschlossen", das bedeutet, dass den Schritt "Einstellungen", das sie gerade beendet haben.

In diesem Fall müssen wir darauf programmgesteuert in die TextBox-Steuerelemente verweisen die `UserSettings WizardStep`. Dies wird erreicht, indem Sie zuerst die `FindControl` Methode, um programmgesteuert verweisen auf die `UserSettings WizardStep`, und klicken Sie dann erneut auf die Textfelder ein innerhalb der `WizardStep`. Sobald die Textfelder ein verwiesen wird, sind wir bereit zur Ausführung der `UPDATE` Anweisung. Die `UPDATE` Anweisung verfügt über die gleiche Anzahl von Parametern wie der `INSERT` -Anweisung in der `CreatedUser` -Ereignishandler, aber hier verwenden wir die home Stadt, Startseite und Signatur Werte, die vom Benutzer bereitgestellt werden.

Mit dieser Ereignishandler vorhanden ist, finden Sie auf die `EnhancedCreateUserWizard.aspx` Seite über einen Browser, und erstellen Sie ein neues Benutzerkonto mit dem Sie Werte für die home-Stadt, Startseite und Signatur angeben. Nach der Erstellung des neuen Kontos sollten Sie umgeleitet die `AdditionalUserInfo.aspx` Seite, in denen die gerade eingegebenen home Stadt, Startseite und Signatur Informationen werden angezeigt.

> [!NOTE]
> Unsere Website verfügt derzeit über zwei Seiten, die von dem Besucher ein neues Konto erstellen kann: `CreatingUserAccounts.aspx` und `EnhancedCreateUserWizard.aspx`. Die Website-Sitemap und Anmeldeseite zeigen Sie auf die `CreatingUserAccounts.aspx` Seite aber die `CreatingUserAccounts.aspx` Seite ihre Startseite Stadt, Startseite und Signatur-Informationen vom Benutzer nicht aufgefordert, und eine entsprechende Zeile, werden nicht hinzugefügt `UserProfiles`. Aktualisieren Sie daher entweder die `CreatingUserAccounts.aspx` Seite, damit sie diese Funktionalität bietet, oder aktualisieren die Sitemap und melden Sie sich die Seite auf `EnhancedCreateUserWizard.aspx` anstelle von `CreatingUserAccounts.aspx`. Wenn Sie letztere Option auswählen, müssen Sie aktualisieren die `Membership` des Ordners `Web.config` Datei, um anonyme Benutzer Zugriff die `EnhancedCreateUserWizard.aspx` Seite.


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie Verfahren zum Modellieren von Daten, die mit der Benutzerkonten innerhalb der mitgliedschaftsframework verknüpft ist. Insbesondere erläutert Modellieren von Entitäten, die gemeinsam eine 1: n Beziehung mit Benutzerkonten als auch für Daten, die eine direkte Beziehung gemeinsam nutzen. Darüber hinaus erläutert, wie diese Informationen kann angezeigt, eingefügt und aktualisiert werden, mit einigen Beispielen, die mit dem SqlDataSource-Steuerelement und andere Beziehung mithilfe von ADO.NET-Code.

Dieses Tutorial ist abgeschlossen, unser Blick auf Benutzerkonten. Beginnend mit dem nächsten Tutorial werden wir unsere Aufmerksamkeit Rollen aktivieren. In den nächsten mehrere Lernprogramme, die wir sehen uns das Rollen-Framework finden Sie unter Vorgehensweise: Erstellen Sie neue Rollen für Benutzer und Rollen zuweisen, wie um zu bestimmen, welche Rollen ein Benutzer angehört, und wie rollenbasierte Autorisierung anzuwenden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Zugreifen auf und Aktualisieren von Daten in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [ASP.NET 2.0-Assistenten-Steuerelement](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Erstellen eine assistentenartige Benutzeroberfläche mit dem ASP.NET 2.0-Assistenten-Steuerelement](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Erstellen von benutzerdefinierten DataSource-Steuerelement](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Anpassen des Steuerelements CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [DetailsView-Steuerelement-Schnellstarts](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Anzeigen von Daten mit dem ListView-Steuerelement](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Analyse der Validierungssteuerelemente in ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Bearbeiten von einfügen und Löschen von Daten](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Form Validation in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Sammeln von benutzerdefinierten Informationen zur Produktregistrierung](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profile in ASP.NET 2.0](http://www.odetocode.com/Articles/440.aspx)
- [Die Asp: ListView-Steuerelement](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [User Profile-Schnellstart](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Der Autor

Scott Mitchell, Autor von mehreren Büchern zu ASP/ASP.NET und Gründer von 4GuysFromRolla.com, ist seit 1998 mit Microsoft-Web-Technologien gearbeitet. Er ist als ein unabhängiger Berater, Schulungsleiter und Autor. Sein neueste Buch wird *[Sams Schulen selbst ASP.NET 2.0 in 24 Stunden](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott erreicht werden kann, zur [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) oder über seinen Blog unter [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Besonderen Dank an...

Diese tutorialreihe wurde durch viele hilfreiche Reviewer überprüft. Meine zukünftigen MSDN-Artikeln überprüfen möchten? Wenn dies der Fall ist, löschen Sie mir eine Linie an [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Vorherige](user-based-authorization-vb.md)
