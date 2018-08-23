---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity | Microsoft-Dokumentation
author: tfitzmac
description: ASP.NET Identity ist ein erweiterbares System auf das diese Weise können Sie Ihren eigenen Anbieter erstellen und es in Ihre Anwendung einbinden, ohne die anwendungse erneut verarbeitet werden...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c92084265ff821bcec25244195a3511b71714836
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837731"
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity ist ein erweiterbares System auf das diese Weise können Sie Ihren eigenen Anbieter erstellen und es in Ihre Anwendung einbinden, ohne die Anwendung erneut verarbeitet werden muss. Dieses Thema beschreibt, wie Sie einen benutzerdefinierte Speicheranbieter für ASP.NET Identity zu erstellen. Hierin sind die entscheidenden Konzepte für Ihren eigenen Anbieter erstellen, aber es ist nicht schrittweise Anleitungen zum Implementieren eines benutzerdefinierten Speicheranbieters.
> 
> Ein Beispiel zum Implementieren eines benutzerdefinierten Speicheranbieters, finden Sie unter [Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Dieses Thema wurde für ASP.NET Identity 2.0 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Visual Studio 2013 mit Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Einführung

Standardmäßig wird der ASP.NET Identity-System speichert Benutzerinformationen in einer SQL Server-Datenbank und verwendet Entity Framework Code First zum Erstellen der Datenbank. Bei vielen Anwendungen eignet sich dieser Ansatz gut. Allerdings Sie eine andere Art von persistenzmechanismus, z. B. Azure-Tabellenspeicher verwenden möchten, oder möglicherweise verfügen Sie bereits von Tabellen mit einer ganz anderen Struktur als die standardmäßige Implementierung. In beiden Fällen können einen benutzerdefinierten Anbieter für den Storage-Mechanismus zu schreiben und diesen Anbieter in Ihre Anwendung einbinden.

ASP.NET Identity ist standardmäßig in vielen der Vorlagen für Visual Studio 2013 enthalten. Sie erhalten Updates nach ASP.NET Identity über [Microsoft AspNet Identität EntityFramework NuGet-Paket](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Dieses Thema enthält die folgenden Abschnitte:

- [Grundlegendes zur Architektur](#architecture)
- [Verstehen Sie die Daten, die gespeichert werden](#data)
- [Erstellen der Datenzugriffsebene](#dal)
- [Anpassen der-Klasse](#user)
- [Passen Sie den Speicher des Benutzers](#userstore)
- [Anpassen der Role-Klasse](#role)
- [Anpassen der rollenspeicher](#rolestore)
- [Konfigurieren der Anwendung zur Verwendung der neuen Speicheranbieter](#reconfigure)
- [Andere Implementierungen von benutzerdefinierte Speicheranbieter](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Grundlegendes zur Architektur

ASP.NET Identity besteht aus Klassen, die aufgerufen wird, Manager und Speicher. -Manager sind die allgemeinen Klassen, die Anwendungsentwickler zum Ausführen von Vorgängen, z. B. das Erstellen eines Benutzers in der ASP.NET Identity-System verwendet. Geschäfte sind Low-Level-Klassen, die angeben, wie Entitäten, z. B. Benutzer und Rollen, beibehalten werden. Geschäfte sind eng gekoppelt mit den Dauerhaftigkeitsmechanismus, aber Managern von entkoppelt sind speichert dies, dass Sie den Dauerhaftigkeitsmechanismus ersetzen können bedeutet, ohne die gesamte Anwendung stören.

Das folgende Diagramm zeigt, wie die Webanwendung mit Managern interagiert, und speichert, die mit der Datenzugriffsebene interagieren.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Um eine benutzerdefinierte Speicheranbieter für ASP.NET Identity zu erstellen, müssen Sie der Datenquelle, die Datenzugriffsebene und den Store-Klassen, die Interaktion mit diesen Datenzugriffsebene zu erstellen. Sie können weiterhin den gleichen-Manager-APIs zum Ausführen von Datenvorgängen für den Benutzer aber nun, da Daten in ein anderes Speicherkonto System gespeichert werden.

Sie müssen nicht die Managerklassen anzupassen, da es beim Erstellen einer neuen Instanz UserManager oder RoleManager auch den Typ der Benutzerklasse angegeben, und übergeben Sie eine Instanz der Store-Klasse als Argument. Dadurch können Sie Ihre benutzerdefinierten Klassen in der vorhandenen Struktur eingebunden. Erfahren Sie, wie Sie die UserManager und RoleManager mit Ihren benutzerdefinierten Store Klassen im Abschnitt zu instanziieren [konfigurieren Sie die Anwendung zur Verwendung der neuen Speicheranbieter](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Verstehen Sie die Daten, die gespeichert werden

Zum Implementieren eines benutzerdefinierten Speicheranbieters müssen Sie verstehen, die Arten von Daten, die mit ASP.NET Identity verwendet, und entscheiden, welche Funktionen für Ihre Anwendung relevant sind.

| Daten | Beschreibung |
| --- | --- |
| Benutzer | Registrierte Benutzer Ihrer Website. Enthält die Benutzer-Id und den Namen an. Sind ein Hashwert des Kennworts, wenn Benutzer sich mit Anmeldeinformationen anzumelden, die auf Ihrer Website spezifisch sind (und nicht mithilfe der Anmeldeinformationen von einem externen Standort wie etwa Facebook), und sicherheitsstempel an, ob etwas in die Anmeldeinformationen des Benutzers geändert wurde. Kann auch e-Mail-Adresse, Telefonnummer, die die aktuelle Anzahl der Fehler bei Anmeldungen, ob die zweistufige Authentifizierung aktiviert ist, und gibt an, ob ein Konto gesperrt wurde. |
| Benutzeransprüche | Ein Satz von Anweisungen (oder Ansprüche) über den Benutzer, die die Identität des Benutzers darstellen. Sie können größer Ausdruck, der die Identität des Benutzers als über Rollen erreicht werden kann. |
| Benutzeranmeldungen | Informationen zu den externen Authentifizierungsanbieter (z. B. Facebook) beim Anmelden eines Benutzers verwenden. |
| Rollen | Autorisierungsgruppe für Ihre Website. Enthält den Rollennamen Id und der Rolle (z. B. "Admin" oder "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Erstellen der Datenzugriffsebene

In diesem Thema wird davon ausgegangen, dass Sie mit der persistenzmechanismus, den Sie beabsichtigen, verwenden Sie "und" Vorgehensweise: Erstellen von Entitäten für diesen Mechanismus vertraut sind. Dieses Thema bietet keine Informationen zum Erstellen des Repositorys oder Datenzugriffsklassen. Stattdessen stellt er einige Vorschläge dazu die Entscheidungen, die Sie benötigen bei der Arbeit mit ASP.NET Identity.

Sie haben nur wenige freiheiten beim Entwerfen der Repositorys für eine benutzerdefinierte Anbieter zu speichern. Sie müssen nur die Repositorys für Funktionen zu erstellen, die Sie in Ihrer Anwendung verwenden möchten. Z. B. Wenn Sie keine Rollen in Ihrer Anwendung verwenden, müssen nicht Sie Speicher für Rollen oder Benutzerrollen zu erstellen. Ihre Technologie und die vorhandene Infrastruktur möglicherweise eine Struktur, die sehr stark von der Standardimplementierung von ASP.NET Identity ist. In der Datenzugriffsebene Geben Sie die Logik zum Arbeiten mit der Struktur Ihrer Repositorys.

Einen MySQL-gesammelte Daten Repositorys für ASP.NET Identity 2.0, finden Sie unter [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

In der Datenzugriffsebene Geben Sie die Logik, um die Daten von ASP.NET Identity in Ihrer Datenquelle zu speichern. Die Datenzugriffsebene für Ihre benutzerdefinierte Speicheranbieter sind zum Beispiel die folgenden Klassen zum Speichern von Informationen für Benutzer und die Rolle.

| Klasse | Beschreibung | Beispiel |
| --- | --- | --- |
| Kontext | Kapselt die Informationen zur Verbindung mit Ihrem persistenzmechanismus aus, und Abfragen ausführen. Diese Klasse ist der Datenzugriffsebene von zentraler Bedeutung. Die anderen Datenklassen erfordert eine Instanz dieser Klasse, um ihre Vorgänge auszuführen. Sie werden auch die Store-Klassen mit einer Instanz dieser Klasse initialisieren. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Benutzerspeicher | Speichert und Benutzerinformationen (z. B. Benutzer und Kennwort-Hash) abruft. | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rolle Speicher | Speichert und ruft Rolleninformationen (z. B. der Role-Name) ab. | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Speicherung von userclaims. | Speichert und Anspruch Benutzerinformationen (z. B. den Anspruchstyp und-Wert) abruft. | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Speicherung von userlogins. | Speichert und ruft die Benutzeranmeldeinformationen (z. B. ein externer Authentifizierungsanbieter) ab. | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Speicherauslastung | Speichert und ruft ab, welche Rollen ein Benutzer zugewiesen ist. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

In diesem Fall müssen Sie nur die Klassen implementieren, die Sie in Ihrer Anwendung verwenden möchten.

In den Data Access-Klassen Geben Sie Code zum Ausführen von Datenvorgängen für Ihre bestimmte persistenzmechanismus aus. In der MySQL-Implementierung enthält die UserTable-Klasse z. B. eine Methode, um einen neuen Datensatz in die Benutzer-Datenbanktabelle einzufügen. Die Variable mit dem Namen `_database` ist eine Instanz der MySQLDatabase-Klasse.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Nach dem Erstellen Ihrer Datenzugriffsklassen, müssen Sie die Store-Klassen erstellen, die die spezifischen Methoden in der Datenzugriffsebene aufrufen.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Anpassen der-Klasse

Wenn Sie Ihren eigenen Anbieter implementieren zu können, müssen Sie eine Benutzerklasse entspricht erstellen die ["identityuser"](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace:

Das folgende Diagramm zeigt die Klasse "identityuser", die Sie erstellen müssen und die Schnittstelle zur in dieser Klasse zu implementieren.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Die [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) Schnittstelle definiert die Eigenschaften, die versucht, dass der UserManager aufgerufen werden, wenn Vorgänge werden entfernungsaktionen ausgeführt angeforderte. Die Schnittstelle enthält zwei Eigenschaften - Id und den Benutzernamen an. Die [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) Schnittstelle können Sie zur Angabe des Schlüssels für den Benutzer durch die generische **TKey** Parameter. Der Typ der Id-Eigenschaft entspricht dem Wert der TKey-Parameter.

Das Identity-Framework bietet außerdem die [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) Schnittstelle (ohne den generischen Parameter) Wenn Sie einen Zeichenfolgenwert für den Schlüssel verwenden möchten.

Die Klasse "identityuser" IUser implementiert und enthält alle zusätzlichen Eigenschaften oder Konstruktoren für Benutzer auf Ihrer Website. Das folgende Beispiel zeigt eine "identityuser"-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das Id-Feld nastaven NA hodnotu **Int** mit dem Wert des generischen Parameters übereinstimmen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Eine vollständige Implementierung finden Sie unter ["identityuser" (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Passen Sie den Speicher des Benutzers

Sie erstellen außerdem eine UserStore-Klasse, die Methoden für alle Vorgänge für den Benutzer bereitstellt. Diese Klasse ist, entspricht die [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace. In die UserStore-Klasse, die Sie implementieren die ["iuserstore"&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) und alle optionalen Schnittstellen. Die Auswahl der optionalen zu implementierenden Schnittstellen basierend auf den Funktionen, die, den Sie in Ihrer Anwendung bereitstellen möchten.

Die folgende Abbildung zeigt die UserStore-Klasse, die Sie erstellen müssen, und die entsprechenden Schnittstellen.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Die Standard-Projektvorlage in Visual Studio enthält Code, der voraussetzt, dass viele optionalen Schnittstellen im Benutzerspeicher implementiert wurden. Wenn Sie die Standardvorlage mit eines benutzerdefinierten Benutzerspeichers verwenden, müssen Sie optionale Schnittstellen implementieren, aus Ihrem Benutzerspeicher oder ändern den Vorlagencode zum nicht mehr in den Schnittstellen Aufrufen von Methoden, die Sie nicht implementiert haben.

 Das folgende Beispiel zeigt eine einfache Store-Klasse. Die **TUser** generischer Parameter ist vom Typ Ihrer Benutzer-Klasse handelt es sich in der Regel die Klasse "identityuser" Sie definiert. Die **TKey** generischer Parameter ist vom Typ Ihrer Benutzer-Schlüssel. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In diesem Beispiel ist der Konstruktor, der einen Parameter mit dem Namen *Datenbank* des Typs ExampleDatabase nur veranschaulicht, wie in der Data Access-Klasse übergeben wird. In der MySQL-Implementierung akzeptiert z. B. diesen Konstruktor einen Parameter vom Typ MySQLDatabase an. 

In die UserStore-Klasse verwenden Sie die Access-Datenklassen, die Sie erstellt haben, um Vorgänge auszuführen. Beispielsweise hat die UserStore-Klasse in der MySQL-Implementierung, die CreateAsync-Methode, die eine Instanz von UserTable verwendet wird, um einen neuen Datensatz einzufügen. Die **einfügen** Methode für die **UserTable** Objekt ist dieselbe Methode, die im vorherigen Abschnitt gezeigt wurde. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Schnittstellen zu implementieren, wenn der Speicher des Benutzers anpassen

Die nächste Abbildung zeigt weitere Details zu den Funktionen, die in jede Schnittstelle definiert. Alle optionalen Schnittstellen erben von "iuserstore".

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  Die ["iuserstore"&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) Schnittstelle ist die einzige Schnittstelle, die Sie aus Ihrem Benutzerspeicher implementieren müssen. Definiert Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Benutzern.
- **IUserClaimStore**  
  Die [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher zum Aktivieren von benutzeransprüchen implementieren. Sie enthält Methoden oder hinzufügen, entfernen und Abrufen von Ansprüchen des Benutzers.
- **IUserLoginStore**  
  Die [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) definiert die Methoden müssen Sie in Ihrem Benutzerspeicher externe Authentifizierungsanbieter zu implementieren. Sie enthält Methoden zum Hinzufügen, entfernen und Abrufen von benutzeranmeldungen und eine Methode zum Abrufen von einem Benutzer basierend auf den Anmeldeinformationen.
- **IUserRoleStore**  
  Die [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher für die Zuordnung von Benutzern zu einer Rolle implementieren. Sie enthält Methoden zum Hinzufügen, entfernen, und rufen Sie die Rollen eines Benutzers und eine Methode zum Überprüfen, ob ein Benutzer eine Rolle zugewiesen ist.
- **IUserPasswordStore**  
  Die ["iuserpasswordstore"&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher beibehalten implementieren Kennwörter gehasht. Sie enthält Methoden zum Abrufen und festlegen, das verschlüsselte Kennwort, und eine Methode, die angibt, ob der Benutzer ein Kennwort festgelegt hat.
- **IUserSecurityStampStore**  
  Die [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher verwenden einen sicherheitsstempel für den, der angibt, ob die Kontoinformationen des Benutzers geändert hat implementieren . Dieser Zeitstempel wird aktualisiert, wenn ein Benutzer ändert das Kennwort ein, oder hinzufügt oder entfernt Anmeldungen. Sie enthält Methoden zum Abrufen und Festlegen des sicherheitsstempels.
- **IUserTwoFactorStore**  
  Die [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, implementieren müssen, an die zweistufige Authentifizierung. Sie enthält Methoden zum Abrufen und festlegen, ob die zweistufige Authentifizierung für einen Benutzer aktiviert ist.
- **IUserPhoneNumberStore**  
  Die [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren müssen, zum Speichern von Telefonnummern des Benutzers. Sie enthält Methoden zum Abrufen und festlegen, die Telefonnummer und gibt an, ob die Telefonnummer bestätigt ist.
- **IUserEmailStore**  
  Die [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie zum Speichern von Benutzer-e-Mail-Adressen implementieren müssen. Sie enthält Methoden zum Abrufen und festlegen, die e-Mail-Adresse und gibt an, ob die e-Mail bestätigt ist.
- **IUserLockoutStore**  
  Die [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie zum Speichern von Informationen zum Sperren eines Kontos implementieren müssen. Sie enthält Methoden zum Abrufen der aktuellen Anzahl fehlerhafter Zugriffsversuche, abrufen und festlegen, ob das Konto gesperrt werden kann, abrufen und Festlegen der Sperre Enddatum, erhöht die Anzahl der Fehlversuche und Zurücksetzen der Anzahl fehlgeschlagener Versuche.
- **IQueryableUserStore**  
  Die [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) Schnittstelle definiert die Elemente Sie implementieren müssen, um einen abfragbaren benutzerpeicher bereitzustellen. Sie enthält eine Eigenschaft, die abgefragt werden Benutzer enthält.

  Implementieren Sie Schnittstellen, die erforderlich sind, in der Anwendung; z. B. Schnittstellen die IUserClaimStore, IUserLoginStore, IUserRoleStore, "iuserpasswordstore" und IUserSecurityStampStore wie unten dargestellt. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Eine vollständige Implementierung (einschließlich aller Schnittstellen), finden Sie unter [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin und IdentityUserRole

Der Microsoft.AspNet.Identity.EntityFramework-Namespace enthält die Implementierungen der [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx), und [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) Klassen. Wenn Sie diese Funktionen verwenden, empfiehlt es sich um Ihre eigenen Versionen dieser Klassen zu erstellen und definieren Sie die Eigenschaften für Ihre Anwendung. Manchmal ist es jedoch sehr viel effizienter, diese Entitäten nicht in den Arbeitsspeicher geladen, beim Ausführen von grundlegenden Vorgängen (z. B. hinzufügen oder Entfernen eines Benutzers Anspruch). Die Back-End-Speicher-Klassen können stattdessen diese Vorgänge direkt auf die Datenquelle ausführen. Beispielsweise können die UserStore.GetClaimsAsync()-Methode der UserClaimTable.FindByUserId(user. aufrufen ID)-Methode zum Ausführen einer Abfrage auf, die Tabelle direkt und eine Liste mit Ansprüchen zurückgeben.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Anpassen der Role-Klasse

Wenn Sie Ihren eigenen Anbieter implementieren zu können, müssen Sie eine rollenklasse entspricht erstellen die [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace:

Das folgende Diagramm zeigt die IdentityRole-Klasse, die Sie erstellen müssen, und die Schnittstelle, die in dieser Klasse zu implementieren.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Die [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) Schnittstelle definiert die Eigenschaften, die versucht, dass die RoleManager aufgerufen werden, wenn Vorgänge werden entfernungsaktionen ausgeführt angeforderte. Die Schnittstelle enthält zwei Eigenschaften - Id und Name. Die [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) Schnittstelle können Sie zur Angabe des Schlüssels für die Rolle über die generische **TKey** Parameter. Der Typ der Id-Eigenschaft entspricht dem Wert der TKey-Parameter.

Das Identity-Framework bietet außerdem die [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) Schnittstelle (ohne den generischen Parameter) Wenn Sie einen Zeichenfolgenwert für den Schlüssel verwenden möchten.

Das folgende Beispiel zeigt eine IdentityRole-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das Id-Feld wird in ganzzahligen Typ festgelegt, mit dem Wert des generischen Parameters übereinstimmen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Eine vollständige Implementierung finden Sie unter [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Anpassen der rollenspeicher

Sie erstellen außerdem eine RoleStore-Klasse, die Methoden für alle Vorgänge für Rollen bereitstellt. Diese Klasse ist, entspricht die [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) -Klasse im Namespace Microsoft.ASP.NET.Identity.EntityFramework. In Ihrer RoleStore-Klasse, die Sie implementieren die [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) und optional die [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) -Schnittstelle.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Das folgende Beispiel zeigt eine Rolle Store-Klasse. Der generische TRole-Parameter hat, den Typ der rollenklasse, die in der Regel die IdentityRole-Klasse ist, die Sie definiert wird. Der generische TKey-Parameter hat es sich um den Typ des Schlüssels Rolle. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  Die [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) Schnittstelle definiert die Methoden, die in Ihrer Rolle Store-Klasse implementieren. Sie enthält Methoden zum Erstellen, aktualisieren, löschen und Rollen werden abgerufen.
- **RoleStore&lt;TRole&gt;**  
  Zum RoleStore anpassen, erstellen Sie eine Klasse, die die IRoleStore-Schnittstelle implementiert. Sie müssen nur diese Klasse implementieren, wenn auf Ihrem System Rollen verwenden möchten. Der Konstruktor, der einen namens Parameter *Datenbank* des Typs ExampleDatabase nur veranschaulicht, wie in der Data Access-Klasse übergeben wird. In der MySQL-Implementierung akzeptiert z. B. diesen Konstruktor einen Parameter vom Typ MySQLDatabase an.  
  
  Eine vollständige Implementierung finden Sie unter [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Konfigurieren der Anwendung zur Verwendung der neuen Speicheranbieter

Sie haben Ihre neuen Speicheranbieter implementiert. Nun müssen Sie Ihrer Anwendung zur Verwendung dieses Storage-Anbieter konfigurieren. Wenn der Standardanbieter für die Speicherung in Ihrem Projekt enthalten war, müssen Sie den Standardanbieter zu entfernen und Ersetzen Sie ihn mit Ihrem Anbieter.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Ersetzen der Standardanbieter für Speicher in MVC-Projekt

1. In der **NuGet-Pakete verwalten** Fenster, deinstallieren Sie die **Microsoft ASP.NET Identity EntityFramework** Paket. Sie können dieses Paket finden, indem Sie in den Paketen installiert werden Identity.EntityFramework gesucht.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) Sie werden aufgefordert werden, wenn Sie auch Entity Framework deinstallieren möchten. Wenn Sie ihn nicht in anderen Teilen der Anwendung benötigen, können Sie sie deinstallieren.
2. Löschen Sie in der Datei IdentityModels.cs im Ordner "Models", oder kommentieren Sie die **"applicationuser"** und **ApplicationDbContext** Klassen. In einer MVC-Anwendung können Sie die gesamte IdentityModels.cs-Datei löschen. Löschen Sie die beiden Klassen, aber stellen Sie sicher, dass Sie die Hilfsklasse beibehalten, die auch in der Datei IdentityModels.cs befindet, in einer Web Forms-Anwendung.
3. Wenn Ihr Storage-Anbieter in einem separaten Projekt befindet, fügen Sie einen Verweis darauf in Ihrer Webanwendung.
4. Ersetzen Sie alle Verweise auf `using Microsoft.AspNet.Identity.EntityFramework;` mit einer using-Anweisung für den Namespace von Ihrem Speicheranbieter.
5. In der **Startup.Auth.cs** Klasse, Ändern der **ConfigureAuth** Methode, um eine einzelne Instanz von den geeigneten Kontext verwenden. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. In der App\_Startordner **IdentityConfig.cs**. Ändern Sie in der ApplicationUserManager-Klasse, die **erstellen** Methode, um eine Benutzer-Manager zurückzugeben, die Ihrem benutzerdefinierten Benutzerspeicher verwendet. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Ersetzen Sie alle Verweise auf **"applicationuser"** mit **"identityuser"**.
8. Das Standardprojekt enthält einige Elemente in der User-Klasse, die nicht in die "IUser"-Schnittstelle definiert sind; z. B. E-Mail, PasswordHash und GenerateUserIdentityAsync. Wenn die User-Klasse nicht über diese Elemente besitzt, müssen Sie sie implementieren oder ändern Sie den Code, der diese Elemente verwendet.
9. Wenn Sie alle Instanzen von RoleManager erstellt haben, ändern Sie diesen Code, um die neue RoleStore-Klasse zu verwenden.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Das Standardprojekt dient für eine Klasse, die einen Zeichenfolgenwert für den Schlüssel verfügt. Wenn die User-Klasse für den Schlüssel (z. B. eine ganze Zahl) einen anderen Typ aufweist, müssen Sie das Projekt, um die Arbeit mit Ihrem Typ ändern. Finden Sie unter [Ändern des Primärschlüssels für Benutzer in ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Fügen Sie ggf. die Verbindungszeichenfolge in die Datei "Web.config" hinzu.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- Blog: [Implementieren von ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Tutorial und GIT-Code: [Simple.Data Asp.Net Identity Provider](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[die grundlegende Identität Konten einrichten, und zeigen sie auf eine externe Datenbank](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Durch [ @xivSolutions ](https://twitter.com/xivSolutions).
- Tutorial[: Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent Entitäten](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) von [SoftFluent](http://www.softfluent.com/)
- [Azure-Tabellenspeicher](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) von James Randall.
- Azure-Tabellenspeicher: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) von [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant von Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Für elastische Datenbanken suchen[h: für elastische Datenbanken Identität](https://github.com/bmbsqd/elastic-identity) von Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) von Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) von Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) von [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) von [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- T4-Vorlagen zum Generieren von EF code für ein "database zuerst" Benutzerspeicher: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
