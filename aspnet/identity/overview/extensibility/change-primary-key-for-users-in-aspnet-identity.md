---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Ändern des Primärschlüssels für Benutzer in ASP.NET Identity | Microsoft-Dokumentation
author: Rick-Anderson
description: In Visual Studio 2013 verwendet die standardwebanwendung einen Zeichenfolgenwert für den Schlüssel für Benutzerkonten an. ASP.NET Identity können Sie den Typ des Ändern der...
ms.author: riande
ms.date: 09/30/2014
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: d2856ce1ca61a29e091bfbd16647b673e6fc659b
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021104"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Ändern des Primärschlüssels für Benutzer in ASP.NET Identity
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In Visual Studio 2013 verwendet die standardwebanwendung einen Zeichenfolgenwert für den Schlüssel für Benutzerkonten an. ASP.NET Identity ermöglicht Ihnen, den Typ des Schlüssels, der Ihre Daten zu Anforderungen ändern. Beispielsweise können Sie den Typ des Schlüssels aus einer Zeichenfolge in eine ganze Zahl ändern.
> 
> In diesem Thema wird gezeigt, wie für den Einstieg Standard, web-Anwendung und ändern den kontoschlüssel für den Benutzer auf eine ganze Zahl. Sie können die gleichen Änderungen verwenden, um jede Art von Schlüssel in Ihrem Projekt zu implementieren. Es zeigt, wie Sie diese Änderungen in der standardwebanwendung übernommen, aber Sie können ähnliche Änderungen an einer benutzerdefinierten Anwendung anwenden. Es zeigt die Änderungen, die erforderlich sind, bei der Arbeit mit MVC oder Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Visual Studio 2013 mit Update 2 (oder höher)
> - ASP.NET Identity 2.1 oder höher


Um die Schritte in diesem Tutorial ausführen zu können, müssen Sie Visual Studio 2013 Update 2 (oder höher), und eine Webanwendung, die aus der ASP.NET Web Application-Vorlage erstellten verfügen. Die Vorlage, die in Update 3 geändert wird. In diesem Thema veranschaulicht, wie die Vorlage in Update 2 und Update 3 ändern.

Dieses Thema enthält folgende Abschnitte:

- [Ändern Sie den Typ des Schlüssels in der Identity-Klasse](#userclass)
- [Hinzufügen der benutzerdefinierte Identität-Klassen, die den Typ des Schlüssels verwenden](#customclass)
- [Ändern Sie die Kontext-Klasse und Benutzer-Manager, um den Schlüsseltyp zu verwenden](#context)
- [Start-Konfiguration ändern, verwenden Sie den Typ des Schlüssels](#startup)
- [Ändern Sie für MVC mit Update 2 AccountController-Komponente um den Typ des Schlüssels zu übergeben.](#mvcupdate2)
- [Für MVC mit Update 3 ändern Sie, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.](#mvcupdate3)
- [Ändern Sie für Web Forms mit Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate2)
- [Für Webformulare mit Update 3 ändern Sie die Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate3)
- [Ausführen der Anwendung](#run)
- [Weitere Ressourcen](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Ändern Sie den Typ des Schlüssels in der Identity-Klasse

Geben Sie in Ihrem Projekt aus der ASP.NET Web Application-Vorlage erstellt haben, dass die Klasse "applicationuser" eine ganze Zahl für den Schlüssel für Benutzerkonten verwendet. Ändern Sie die Klasse "applicationuser" von "identityuser" erben, der den Typ aufweist, im IdentityModels.cs, **Int** für den generischen TKey-Parameter. Sie übergeben außerdem die Namen der drei benutzerdefinierte Klasse mit der Sie noch nicht implementiert wurde.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Sie haben den Typ des Schlüssels geändert, aber in der Standardeinstellung die übrigen Teile der Anwendung weiterhin wird davon ausgegangen, dass der Schlüssel eine Zeichenfolge ist. Sie müssen explizit den Typ des Schlüssels im Code angeben, die eine Zeichenfolge annimmt.

In der **"applicationuser"** Klasse, Ändern der **GenerateUserIdentityAsync** Methode, um ganze Zahl, einzufügen, wie im folgenden hervorgehobenen Code gezeigt. Diese Änderung ist nicht erforderlich für Web Forms-Projekte mit der Update 3-Vorlage.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Hinzufügen der benutzerdefinierte Identität-Klassen, die den Typ des Schlüssels verwenden

Die anderen Identitätsklassen, z. B. IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, werden immer noch mit einem Zeichenfolgenschlüssel eingerichtet. Erstellen Sie neue Versionen dieser Klassen, die eine ganze Zahl für den Schlüssel angeben. Sie müssen nicht viel Code zur Implementierung in diesen Klassen bereitstellen, legen Sie in erster Linie nur Int als Schlüssel.

Die folgenden Klassen der IdentityModels.cs-Datei hinzufügen.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Ändern Sie die Kontext-Klasse und Benutzer-Manager, um den Schlüsseltyp zu verwenden

In IdentityModels.cs, ändern Sie die Definition der **ApplicationDbContext** Klasse zur Verwendung der neuen benutzerdefinierten Klassen und eine **Int** für den Schlüssel, wie in den hervorgehobenen Code gezeigt.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Der ThrowIfV1Schema-Parameter ist nicht mehr gültig ist, im Konstruktor. Ändern Sie den Konstruktor aus, damit er nicht auf einen ThrowIfV1Schema-Wert übergeben wird.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs öffnen, und Ändern der **ApplicationUserManger** Ihr neuen Benutzer zu verwendende Klasse an Klasse zum Beibehalten von Daten zu speichern und ein **Int** für den Schlüssel.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

In der Vorlage Update 3 müssen Sie die ApplicationSignInManager-Klasse ändern.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Start-Konfiguration ändern, verwenden Sie den Typ des Schlüssels

Ersetzen Sie in Startup.Auth.cs den OnValidateIdentity-Code, wie unten markiert. Beachten Sie, dass die Definition der GetUserIdCallback analysiert den Zeichenfolgenwert in eine ganze Zahl.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Wenn Ihr Projekt nicht die generische Implementierung erkennt das **GetUserId** -Methode, müssen Sie möglicherweise das ASP.NET Identity-NuGet-Paket auf Version 2.1 aktualisieren

Sie haben zahlreiche Änderungen vorgenommen, auf die Infrastrukturklassen, die von ASP.NET Identity verwendet. Wenn Sie versuchen, Kompilieren des Projekts, bemerken Sie viele Fehler. Glücklicherweise sind die übrigen Fehler ähnliche. Die Identity-Klasse erwartet eine ganze Zahl für den Schlüssel, aber der Controller (oder Web Form) ist einen String-Wert übergeben. In jedem Fall müssen Sie durch Aufrufen von einer Zeichenfolge und ganze Zahl konvertieren **GetUserId&lt;Int&gt;**. Sie können die Fehlerliste aus der Kompilierung funktioniert oder führen Sie die nachfolgenden Änderungen.

Die verbleibenden Änderungen richten sich nach dem Typ des Projekts, die Sie erstellen und zu denen die updateverwaltung, die Sie in Visual Studio installiert haben. Sie können direkt zu dem entsprechenden Abschnitt über die folgenden Links wechseln.

- [Ändern Sie für MVC mit Update 2 AccountController-Komponente um den Typ des Schlüssels zu übergeben.](#mvcupdate2)
- [Für MVC mit Update 3 ändern Sie, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.](#mvcupdate3)
- [Ändern Sie für Web Forms mit Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate2)
- [Für Webformulare mit Update 3 ändern Sie die Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Ändern Sie für MVC mit Update 2 AccountController-Komponente um den Typ des Schlüssels zu übergeben.

Öffnen Sie die Datei "AccountController.cs". Sie müssen die folgenden Methoden ändern.

**ConfirmEmail** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Aufheben der Zuordnung** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Sie können jetzt [führen Sie die Anwendung](#run) und einen neuen Benutzer registrieren.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Für MVC mit Update 3 ändern Sie, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.

Öffnen Sie die Datei "AccountController.cs". Sie müssen die folgende Methode zu ändern.

**ConfirmEmail** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Öffnen Sie die ManageController.cs-Datei. Sie müssen die folgenden Methoden ändern.

**Index** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** Methoden

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** Methoden

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Sie können jetzt [führen Sie die Anwendung](#run) und einen neuen Benutzer registrieren.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Ändern Sie für Web Forms mit Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.

Für Webformulare mit Update 2 müssen Sie die folgenden Seiten ändern.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Sie können jetzt [führen Sie die Anwendung](#run) und einen neuen Benutzer registrieren.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Für Webformulare mit Update 3 ändern Sie die Konto-Seiten, um den Typ des Schlüssels zu übergeben.

Für Webformulare mit Update 3 müssen Sie die folgenden Seiten ändern.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Ausführen der Anwendung

Sie haben alle erforderlichen Änderungen an der Standardvorlage für Web-Anwendung abgeschlossen. Führen Sie die Anwendung und registrieren Sie einen neuen Benutzer. Nach der Registrierung des Benutzers werden Sie feststellen, dass die Tabelle "aspnetusers" eine Id-Spalte eine ganze Zahl ist.

![neuen Primärschlüssel](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Wenn Sie zuvor die ASP.NET Identity Tabellen mit einem anderen primären Schlüssel erstellt haben, müssen Sie einige zusätzlichen Änderungen vornehmen. Wenn möglich, nur löschen Sie die vorhandene Datenbank. Die Datenbank wird mit den richtigen Entwurf neu erstellt werden, wenn Sie die Webanwendung ausführen und einen neuen Benutzer hinzufügen. Wenn der Löschvorgang nicht möglich ist, führen Sie Code first-Migrationen zum Ändern von Tabellen. Allerdings wird der neue ganzzahlige Primärschlüssel nicht als eine SQL-IDENTITY-Eigenschaft in der Datenbank eingerichtet werden. Sie müssen manuell die Id-Spalte als Identität festlegen.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- [Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrieren einer vorhandenen Website von einem SQL-Mitgliedschaftsanbieter nach ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrieren von Daten eines universellen Anbieters für Mitgliedschaften und Benutzerprofilen nach ASP.NET Identity](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Beispielanwendung](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) mit geänderten Primärschlüssel
