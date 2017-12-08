---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: "Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity | Microsoft Docs"
author: tfitzmac
description: "ASP.NET Identity ist ein erweiterbares System können Sie Ihren eigenen Speicheranbieter erstellen und in die Anwendung eingebunden werden, ohne die anwe erneut verarbeitet werden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2014
ms.topic: article
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 1ea779cb10661512690e3fec16ae73be0f40d15a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity ist ein erweiterbares System können Sie Ihren eigenen Speicheranbieter erstellen und in die Anwendung eingebunden werden, ohne die Anwendung erneut verarbeitet werden. Dieses Thema beschreibt das Erstellen eines benutzerdefinierten Speicheranbieters für ASP.NET Identity. Wichtiger Konzepte, die zum Erstellen eigener Speicheranbieter behandelt, aber es ist nicht schrittweise Anleitungen zum Implementieren eines benutzerdefinierten Speicheranbieters.
> 
> Ein Beispiel ein benutzerdefinierten Speicheranbieters implementieren, finden Sie unter [Implementieren eines benutzerdefinierten MySQL ASP.NET Identity Speicheranbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Dieses Thema wurde für ASP.NET Identity 2.0 aktualisiert.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Visual Studio 2013 mit Update 2
> - ASP.NET Identity 2


## <a name="introduction"></a>Einführung

Standardmäßig die ASP.NET Identity-System speichert Benutzerinformationen in einer SQL Server-Datenbank und Entity Framework Code First verwendet, um die Datenbank zu erstellen. Für viele Anwendungen eignet sich dieser Ansatz gut. Allerdings auch eine andere Art der Persistenz-Mechanismus, wie z. B. Azure-Tabellenspeicher verwenden oder möglicherweise verfügen Sie bereits mit einer sehr unterschiedliche Struktur als die standardmäßige Implementierung Datenbanktabellen. In beiden Fällen können Sie einen benutzerdefinierten Anbieter für Ihre Speichermechanismus schreiben und stecken Sie diesen Anbieter in Ihrer Anwendung.

ASP.NET Identity ist standardmäßig in vielen der Visual Studio 2013 Vorlagen enthalten. Sie erhalten Updates zu ASP.NET Identity über [Microsoft AspNet Identity EntityFramework NuGet-Paket](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

Dieses Thema enthält die folgenden Abschnitte:

- [Grundkenntnisse der Architektur](#architecture)
- [Verstehen Sie die Daten, die gespeichert werden](#data)
- [Die Datenzugriffsebene erstellen](#dal)
- [Anpassen der User-Klasse](#user)
- [Passen Sie den Speicher des Benutzers](#userstore)
- [Anpassen der Rolle ""-Klasse](#role)
- [Anpassen der rollenspeicher](#rolestore)
- [Konfigurieren Sie die Anwendung für die Verwendung der neuen Speicheranbieter](#reconfigure)
- [Andere Implementierungen von benutzerdefinierten Speicheranbieter](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Grundkenntnisse der Architektur

ASP.NET Identity besteht aus Klassen, die aufgerufen wird, Manager und speichert. -Manager sind allgemeine Klassen, die ein Anwendungsentwickler verwendet wird, um die Vorgänge, z. B. Erstellen eines Benutzers in die ASP.NET Identity-System ausführen. Geschäfte sind an darunter liegende Klassen, die angeben, wie die Entitäten, z. B. Benutzer und Rollen, beibehalten werden. Geschäfte sind eng gekoppelt, mit der Dauerhaftigkeit Manager wurden jedoch entkoppelt von Geschäften dies, dass Sie die Dauerhaftigkeit ersetzen können bedeutet, ohne Unterbrechung der gesamten Anwendung.

Das folgende Diagramm zeigt, wie die Webanwendung mit Managern interagiert, und speichert, die mit der Datenzugriffsebene interagieren.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Zum Erstellen eines benutzerdefinierten Speicheranbieters für ASP.NET Identity müssen Sie die Datenquelle, die Datenzugriffsebene und die Store-Klassen, die Interaktion mit diesem Datenzugriffsebene erstellen. Sie können weiterhin, mit dem gleichen-Manager-APIs zum Ausführen von Datenvorgängen für den Benutzer aber jetzt, dass Daten in einem anderen Speichersystem gespeichert werden.

Sie müssen nicht die Managerklassen anzupassen, weil beim Erstellen einer neuen Instanz eines UserManager oder RoleManager den Typ der Benutzerklasse bereitstellen und eine Instanz der Klasse Store als Argument übergeben. Dieser Ansatz können Sie Ihren benutzerdefinierten Klassen in der bestehenden Struktur einbinden. Sehen Sie, wie UserManager und RoleManager mit Ihren benutzerdefinierten Store Klassen im Abschnitt zu instanziieren [konfigurieren Sie die Anwendung für die Verwendung der neuen Speicheranbieter](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Verstehen Sie die Daten, die gespeichert werden

Zum Implementieren eines benutzerdefinierten Speicheranbieters müssen Sie verstehen, die Arten von Daten mit ASP.NET Identity verwendet, und entscheiden, welche Funktionen für Ihre Anwendung wichtig sind.

| Daten | Beschreibung |
| --- | --- |
| Benutzer | Registrierte Benutzer Ihrer Website. Enthält die Benutzer-Id und den Namen an. Möglicherweise enthalten ein Kennwort ein Hashwert erstellt wurde, wenn Benutzer sich mit den Anmeldeinformationen anmelden, die auf Ihrer Website spezifisch sind (und nicht mithilfe der Anmeldeinformationen aus einer externen Website wie Facebook), und sicherheitsstempel, um anzugeben, ob alle Elemente in die Anmeldeinformationen des Benutzers geändert wurde. Möglicherweise enthalten auch die e-Mail-Adresse, Telefonnummer, ob zweistufige Authentifizierung aktiviert ist, wird die aktuelle Anzahl der fehlgeschlagene Anmeldungen, und gibt an, ob ein Konto gesperrt wurde. |
| Benutzeransprüche | Ein Satz von Anweisungen (oder Ansprüche), über den Benutzer, die die Identität des Benutzers darstellen. Kann größer Ausdruck, der die Identität des Benutzers als über Rollen erzielt werden kann. |
| Benutzernamen | Informationen zu den externen Authentifizierungsanbieter (z. B. Facebook) verwenden, wenn Sie einen Benutzer anmelden. |
| Rollen | Eine Autorisierungsgruppe für Ihre Website. Enthält den Rollennamen-Id und die Rolle (z. B. "Admin" oder "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Die Datenzugriffsebene erstellen

In diesem Thema wird davon ausgegangen, dass Sie mit der Persistenz-Mechanismus, den Sie verwenden möchten, und zum Erstellen von Entitäten für diesen Mechanismus vertraut sind. Dieses Thema bietet detaillierte Informationen zum Erstellen des Repositorys oder Datenzugriffsklassen; Stattdessen stellt er einige Vorschläge für die entwurfsentscheidungen, dass Sie beim Arbeiten mit ASP.NET Identity vornehmen müssen.

Sie haben viele Freiheit beim Entwerfen der Repositorys für einen benutzerdefinierten Anbieter zu speichern. Sie müssen nur Repositorys für Funktionen zu erstellen, die Sie in Ihrer Anwendung verwenden möchten. Z. B. Wenn Sie keine Rollen in Ihrer Anwendung verwenden, müssen nicht Sie Speicher für Rollen oder Benutzerrollen erstellen. Die Technologie und die vorhandene Infrastruktur möglicherweise eine Struktur, die die standardmäßige Implementierung des ASP.NET Identity erheblich unterscheiden. In der Datenzugriffsebene Geben Sie die Logik zum Arbeiten mit der Struktur Ihrer Repositorys.

Eine MySQL-Implementierung des Datenrepositorys für ASP.NET Identity 2.0, finden Sie unter [MySQLIdentity.sql](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

In der Datenzugriffsebene Geben Sie die Logik, um die Daten von ASP.NET Identity in Ihrer Datenquelle zu speichern. Die Datenzugriffsebene für Ihre benutzerdefinierten Speicheranbieter kann die folgenden Klassen zum Speichern von Benutzer-und Rolle enthalten.

| Klasse | Beschreibung | Beispiel |
| --- | --- | --- |
| Kontext | Kapselt die Informationen zum Herstellen einer Verbindung mit Ihrem Dauerhaftigkeit und Ausführen von Abfragen. Diese Klasse ist der Datenzugriffsebene von zentraler Bedeutung. Die anderen Datenklassen ist eine Instanz dieser Klasse für ihre Vorgänge erforderlich. Sie werden auch die Store-Klassen mit einer Instanz dieser Klasse initialisieren. | [MySQLDatabase](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Benutzerspeicher | Speichert und ruft Benutzerinformationen (z. B. Benutzer und Kennwort-Hash) ab. | [UserTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Rolle Speicher | Speichert Rolleninformationen und abruft (z. B. die Rolle ""). | [RoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Speicherung von userclaims. | Speichert und Anspruch Benutzerinformationen (z. B. den Anspruchstyp und-Wert) abruft. | [UserClaimsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Speicherung von userlogins. | Speichert und ruft die Benutzer-Anmeldeinformationen (z. B. eines externen Authentifizierungsanbieters) ab. | [UserLoginsTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Speicher der Benutzerrollen | Speichert und ruft ab, welche Rollen ein Benutzer zugewiesen ist. | [UserRoleTable (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

Erneut verwenden, müssen Sie nur den Klassen implementieren, die Sie in Ihrer Anwendung verwenden möchten.

In der Datenzugriffsklassen Geben Sie Code zum Ausführen von Datenvorgängen für einen bestimmten Persistenz-Mechanismus. Innerhalb der MySQL-Implementierung enthält z. B. die UserTable-Klasse eine Methode zum Einfügen eines neuen Datensatzes in die Datenbanktabelle Benutzer an. Die Variable mit dem Namen `_database` ist eine Instanz der MySQLDatabase-Klasse.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Nach dem Erstellen der Datenzugriffsklassen, müssen Sie die Store-Klassen erstellen, die die spezifischen Methoden in der Datenzugriffsebene aufrufen.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Anpassen der User-Klasse

Wenn Sie Ihren eigenen Speicheranbieter implementieren zu können, müssen, erstellen Sie eine Klasse entspricht der [IdentityUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace:

Das folgende Diagramm zeigt die IdentityUser-Klasse, die Sie erstellen müssen und die Schnittstelle in dieser Klasse implementiert.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

Die [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) Schnittstelle definiert die Eigenschaften, die der UserManager versucht, der beim Ausführen von Vorgängen angefordert aufgerufen. Die Schnittstelle enthält zwei Eigenschaften - Id und den Benutzernamen an. Die [IUser&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613291(v=vs.108).aspx) -Schnittstelle ermöglicht Ihnen die Angabe den Typ des Schlüssels für den Benutzer durch die generische **TKey** Parameter. Der Typ der Id-Eigenschaft entspricht dem Wert des Parameters TKey.

Das Identity-Framework bietet außerdem die [IUser](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) Schnittstelle (ohne den generischen Parameter) Wenn Sie einen Zeichenfolgenwert für den Schlüssel verwenden möchten.

Die Klasse IdentityUser IUser implementiert und enthält zusätzliche Eigenschaften oder Konstruktoren für Benutzer auf Ihrer Website. Das folgende Beispiel zeigt eine IdentityUser-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das Feld "Id"-Wert von **Int** mit dem Wert des generischen Parameters übereinstimmen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Eine vollständige Implementierung finden Sie unter [IdentityUser (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Passen Sie den Speicher des Benutzers

Sie erstellen außerdem eine UserStore-Klasse, die die Methoden für alle Vorgänge für den Benutzer bereitstellt. Diese Klasse entspricht dem [UserStore&lt;TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn315446(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace. In der UserStore-Klasse, die Sie implementieren die [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) und jede der optionalen Schnittstellen. Sie wählen die optionale Schnittstellen implementiert auf Grundlage der Funktionalität, die Sie in der Anwendung bereitstellen möchten.

Die folgende Abbildung zeigt die UserStore-Klasse, die Sie erstellen müssen und die entsprechenden Schnittstellen.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

Die Standard-Projektvorlage in Visual Studio enthält Code, der voraussetzt, dass viele optionalen Schnittstellen im Benutzerspeicher implementiert wurden. Wenn Sie mit einem benutzerdefinierten Benutzerspeicher die Standardvorlage verwenden, müssen Sie optionale Schnittstellen implementieren, in Ihrem Benutzerspeicher oder ändern den Code zum nicht mehr in den Schnittstellen Aufrufen von Methoden, die nicht implementiert wurde.

 Das folgende Beispiel zeigt eine einfache Store-Klasse. Die **TUser** generischer Parameter akzeptiert den Typ des User-Klasse die in der Regel die IdentityUser-Klasse ist Sie definiert. Die **TKey** generischer Parameter akzeptiert, die Ihre Benutzerschlüssel-Typ. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 In diesem Beispiel wird mit dem Namen des Konstruktors, der einen Parameter akzeptiert *Datenbank* des Typs ExampleDatabase nur eine Abbildung wie in der Data Access-Klasse übergeben wird. So dauert beispielsweise in der MySQL-Implementierung dieser Konstruktor einen Parameter vom Typ MySQLDatabase. 

In dieser Klasse UserStore verwenden Sie die Datenzugriffsklassen, die Sie erstellt haben, um Vorgänge auszuführen. Beispielsweise weist die UserStore-Klasse in der Implementierung der MySQL CreateAsync-Methode, die eine Instanz von UserTable verwendet, um einen neuen Datensatz einzufügen. Die **einfügen** Methode für die **UserTable** Objekt ist die gleiche Methode, die im vorherigen Abschnitt gezeigt wurde. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Schnittstellen implementiert werden, wenn Benutzerspeicher anpassen

Die nächste Abbildung zeigt ausführliche Informationen zu Funktionen in jede Schnittstelle definiert. Alle optionalen Schnittstellen erben von IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
 Die [IUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613278(v=vs.108).aspx) -Schnittstelle ist die einzige müssen Sie in Ihrem Benutzerspeicher implementieren. Definiert Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Benutzern.
- **IUserClaimStore**  
 Die [IUserClaimStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613265(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher zum Aktivieren von benutzeransprüchen implementieren. Es enthält die Methoden oder hinzufügen, entfernen und das Abrufen von benutzeransprüchen.
- **IUserLoginStore**  
 Die [IUserLoginStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613272(v=vs.108).aspx) definiert die Methoden müssen Sie in Ihrem Benutzerspeicher So aktivieren Sie die externe Authentifizierungsanbieter implementieren. Enthält Methoden zum Hinzufügen, entfernen und Abrufen von benutzeranmeldungen und eine Methode zum Abrufen von einem Benutzer basierend auf der Anmeldeinformationen.
- **IUserRoleStore**  
 Die [IUserRoleStore&lt;TKey, TUser&gt; ](https://msdn.microsoft.com/en-us/library/dn613276(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher zur Zuordnung von Benutzern zu einer Rolle implementieren. Enthält Methoden zum Hinzufügen, entfernen und Abrufen von Rollen eines Benutzers und eine Methode zum Überprüfen, ob ein Benutzer einer Rolle zugewiesen ist.
- **IUserPasswordStore**  
 Die [IUserPasswordStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613273(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrem Benutzerspeicher persistent implementieren gehasht Kennwörter. Enthält Methoden zum Abrufen und festlegen, das verschlüsselte Kennwort und eine Methode, die angibt, ob der Benutzer ein Kennwort festgelegt wurde.
- **IUserSecurityStampStore**  
 Die [IUserSecurityStampStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613277(v=vs.108).aspx) Schnittstelle definiert die Methoden müssen Sie in Ihrer Benutzerspeicher einen sicherheitsstempel für den, der angibt, ob die Benutzerkontoinformationen geändert hat implementieren . Dieser Zeitstempel wird aktualisiert, wenn ein Benutzer das Kennwort ändert oder hinzufügt oder entfernt Anmeldungen. Enthält Methoden zum Abrufen und Festlegen der sicherheitsstempel.
- **IUserTwoFactorStore**  
 Die [IUserTwoFactorStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613279(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren, implementieren zweistufige Authentifizierung müssen. Enthält Methoden zum Abrufen und festlegen, ob zweistufige Authentifizierung für einen Benutzer aktiviert ist.
- **IUserPhoneNumberStore**  
 Die [IUserPhoneNumberStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613275(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren müssen, um die Telefonnummern von Benutzern zu speichern. Enthält Methoden zum Abrufen und Festlegen der Telefonnummer und gibt an, ob die Telefonnummer bestätigt ist.
- **IUserEmailStore**  
 Die [IUserEmailStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613143(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren müssen, um e-Mail-Adressen von Benutzern zu speichern. Enthält Methoden zum Abrufen und festlegen, die e-Mail-Adresse und gibt an, ob die e-Mail bestätigt ist.
- **IUserLockoutStore**  
 Die [IUserLockoutStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613271(v=vs.108).aspx) Schnittstelle definiert die Methoden, die Sie implementieren müssen, um Informationen zum Sperren eines Kontos zu speichern. Es enthält Methoden zum Abrufen der aktuellen Anzahl von zugriffsversuchsfehlern, abrufen und festlegen, ob das Konto gesperrt werden kann, abrufen und Festlegen der Sperre Enddatum, erhöht die Anzahl der fehlerhafte Anmeldeversuche und die Anzahl der gescheiterten Versuche zurückgesetzt.
- **IQueryableUserStore**  
 Die [IQueryableUserStore&lt;TUser, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613267(v=vs.108).aspx) Schnittstelle definiert die Elemente, die Sie implementieren müssen, um einen abfragbaren benutzerpeicher bereitzustellen. Sie enthält eine Eigenschaft, die abgefragt werden Benutzer enthält.

 Implementieren Sie Schnittstellen, die erforderlich sind, in der Anwendung; z. B. Schnittstellen die IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore und IUserSecurityStampStore wie unten dargestellt. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Eine vollständige Implementierung (einschließlich aller Schnittstellen), finden Sie unter [UserStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim IdentityUserLogin und IdentityUserRole

Der Microsoft.AspNet.Identity.EntityFramework-Namespace enthält die Implementierung der [IdentityUserClaim](https://msdn.microsoft.com/en-us/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/en-us/library/dn613251(v=vs.108).aspx), und [IdentityUserRole](https://msdn.microsoft.com/en-us/library/dn613252(v=vs.108).aspx) Klassen. Wenn Sie diese Funktionen verwenden, empfiehlt es sich, eine eigene Versionen dieser Klassen erstellen und definieren Sie die Eigenschaften für Ihre Anwendung. Manchmal ist es jedoch effizienter, diese Entitäten nicht in den Arbeitsspeicher geladen, beim Ausführen von grundlegenden Vorgänge (z. B. hinzufügen oder Entfernen eines Benutzers Anspruch). Stattdessen können die Back-End-Speicher-Klassen diese Vorgänge direkt auf die Datenquelle ausgeführt werden. Die Methode UserStore.GetClaimsAsync() kann zum Beispiel die UserClaimTable.FindByUserId(user. aufrufen. ID) Methode zum Ausführen einer Abfrage auf, die Tabelle direkt und eine Liste von Ansprüchen zurückgeben.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Anpassen der Rolle ""-Klasse

Wenn Sie Ihren eigenen Speicheranbieter implementieren zu können, müssen, erstellen Sie eine rollenklasse entspricht der [IdentityRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) -Klasse in der [Microsoft.ASP.NET.Identity.EntityFramework](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) Namespace:

Das folgende Diagramm zeigt die IdentityRole-Klasse, die Sie erstellen müssen und die Schnittstelle in dieser Klasse implementiert.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

Die [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) Schnittstelle definiert die Eigenschaften, die RoleManager versucht, der beim Ausführen von Vorgängen angefordert aufgerufen. Die Schnittstelle enthält zwei Eigenschaften - Id und Name. Die [IRole&lt;TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613268(v=vs.108).aspx) -Schnittstelle ermöglicht Ihnen das Festlegen den Typ des Schlüssels für die Rolle über die generische **TKey** Parameter. Der Typ der Id-Eigenschaft entspricht dem Wert des Parameters TKey.

Das Identity-Framework bietet außerdem die [IRole](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) Schnittstelle (ohne den generischen Parameter) Wenn Sie einen Zeichenfolgenwert für den Schlüssel verwenden möchten.

Das folgende Beispiel zeigt eine IdentityRole-Klasse, die eine ganze Zahl für den Schlüssel verwendet. Das Feld "Id" wird in ganzzahligen Typ festgelegt, mit dem Wert des generischen Parameters übereinstimmen. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Eine vollständige Implementierung finden Sie unter [IdentityRole (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Anpassen der rollenspeicher

Sie erstellen außerdem eine RoleStore-Klasse, die die Methoden für alle Vorgänge für Rollen bereitstellt. Diese Klasse entspricht dem [RoleStore&lt;TRole&gt; ](https://msdn.microsoft.com/en-us/library/dn468181(v=vs.108).aspx) Klasse im Microsoft.ASP.NET.Identity.EntityFramework-Namespace. In der RoleStore-Klasse, die Sie implementieren die [IRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613266(v=vs.108).aspx) und optional die [IQueryableRoleStore&lt;TRole, TKey&gt; ](https://msdn.microsoft.com/en-us/library/dn613262(v=vs.108).aspx) -Schnittstelle.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

Das folgende Beispiel zeigt eine Rolle Store-Klasse. Der generische TRole-Parameter akzeptiert den Typ der rollenklasse, die in der Regel die IdentityRole-Klasse ist, die Sie definiert. Der generische Parameter des TKey nimmt den Typ Ihres Schlüssels Rolle. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
 Die [IRoleStore](https://msdn.microsoft.com/en-us/library/dn468195.aspx) Schnittstelle definiert die Methoden, die in Ihrer Rolle Store-Klasse implementiert. Enthält Methoden zum Erstellen, aktualisieren, löschen und Abrufen von Rollen.
- **RoleStore&lt;TRole&gt;**  
 Zum RoleStore anpassen, erstellen Sie eine Klasse, die IRoleStore-Schnittstelle implementiert. Sie müssen nur diese Klasse implementieren, wenn Rollen auf Ihrem System verwendet werden soll. Der Konstruktor, einen Parameter namens akzeptiert *Datenbank* des Typs ExampleDatabase nur eine Abbildung wie in der Data Access-Klasse übergeben wird. So dauert beispielsweise in der MySQL-Implementierung dieser Konstruktor einen Parameter vom Typ MySQLDatabase.  
  
 Eine vollständige Implementierung finden Sie unter [RoleStore (MySQL)](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Konfigurieren Sie die Anwendung für die Verwendung der neuen Speicheranbieter

Sie haben Ihre neue Speicheranbieter implementiert. Nun müssen Sie die Anwendung verwendet diesen Speicheranbieter konfigurieren. Wenn der Standardanbieter Speicher in Ihrem Projekt enthalten ist, müssen Sie entfernen den Standardanbieter und Ersetzen Sie sie bei Ihrem Anbieter.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Ersetzen Sie die Standard-Speicheranbieter in MVC-Projekt

1. In der **NuGet-Pakete verwalten** Fenster, deinstallieren Sie die **Microsoft ASP.NET Identity EntityFramework** Paket. Sie können dieses Paket nach Dateien suchen der installierten Pakete Identity.EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png)Sie werden gefragt, ob Sie auch Entity Framework deinstallieren möchten. Wenn Sie ihn nicht in anderen Teilen der Anwendung benötigen, können Sie es deinstallieren.
2. In der Datei im Ordner Models IdentityModels.cs löschen, oder kommentieren Sie Sie aus der **ApplicationUser** und **ApplicationDbContext** Klassen. In einer MVC-Anwendung können Sie die gesamte IdentityModels.cs Datei löschen. Klicken Sie in einer Web Forms-Anwendung die beiden Klassen gelöscht, aber stellen Sie sicher, dass Sie die Hilfsklasse beibehalten, die auch in der Datei IdentityModels.cs befindet.
3. Wenn es sich bei Ihrem Speicheranbieter in einem separaten Projekt befindet, fügen Sie einen Verweis darauf in der Webanwendung an.
4. Ersetzen Sie alle Verweise auf `using Microsoft.AspNet.Identity.EntityFramework;` mit einer using-Anweisung für den Namespace von Ihrem Speicheranbieter.
5. In der **Startup.Auth.cs** Klasse, ändern Sie die **ConfigureAuth** zu eine einzelne Instanz von den entsprechenden Kontext zu verwendende Methode. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. In der App\_Startordner öffnen **IdentityConfig.cs**. Ändern Sie in der Klasse ApplicationUserManager der **erstellen** Methode, um eine Benutzer-Manager zurückzugeben, die Ihre benutzerdefinierten Benutzerspeicher verwendet. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Ersetzen Sie alle Verweise auf **ApplicationUser** mit **IdentityUser**.
8. Das Standardprojekt enthält einige Member in User-Klasse, die nicht in der IUser-Schnittstelle definiert sind; wie E-Mail, PasswordHash und GenerateUserIdentityAsync. Wenn User-Klasse nicht diese Member verfügt, müssen Sie diese implementieren oder ändern Sie den Code, der diesen Member verwendet.
9. Wenn Sie alle Instanzen von RoleManager erstellt haben, ändern Sie Code, verwenden Sie die neue RoleStore-Klasse.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. Das Standardprojekt dient für eine Klasse, die einen Zeichenfolgenwert für den Schlüssel verfügt. Wenn User-Klasse auf einen anderen Typ für den Schlüssel (z. B. eine ganze Zahl) verfügt, müssen Sie das Projekt zur Bearbeitung von Ihrem Typs ändern. Finden Sie unter [Ändern der Primärschlüssel für Benutzer in ASP.NET Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Fügen Sie ggf. die Verbindungszeichenfolge in der Datei "Web.config" hinzu.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- Blog: [Implementieren von ASP.NET Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Lernprogramm und GIT-Code: [Simple.Data Asp.Net Identity Provider](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Lernprogramm:[die grundlegende Identität Konten einrichten und zeigen sie auf eine externe DB](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Durch [ @xivSolutions ](https://twitter.com/xivSolutions).
- Lernprogramm[: Implementieren eines benutzerdefinierten MySQL ASP.NET Identity-Speicheranbieters](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [CodeFluent Entitäten](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) von [SoftFluent](http://www.softfluent.com/)
- [Azure-Tabellenspeicher](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) von James Randall.
- Azure-Tabellenspeicher: [AspNet.Identity.TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) von [ @stuartleeks ](https://twitter.com/stuartleeks).
- [CouchDB / Cloudant von Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elastische Suchzeichenfolge[h: elastischen Identität](https://github.com/bmbsqd/elastic-identity) von Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) von Jonathan Sheely Jonathan Sheely.
- [NHibernate.AspNet.Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) von Antônio Milesi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) von [ @tourismgeek ](https://twitter.com/tourismgeek).
- [RavenDB.AspNet.Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) von [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis.AspNet.Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- T4-Vorlagen zum Generieren von EF code für ein Geschäft auf "Datenbank zuerst" Benutzer: [AspNet.Identity.EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
