---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Ändern der Primärschlüssel für Benutzer in ASP.NET Identity | Microsoft Docs
author: tfitzmac
description: In Visual Studio 2013 verwendet die standardwebanwendung einen Zeichenfolgenwert für den Schlüssel für Benutzerkonten an. ASP.NET Identity ermöglicht es Ihnen, den Typ des Ändern der...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26498229"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Primärschlüssel für Benutzer in ASP.NET Identity ändern
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In Visual Studio 2013 verwendet die standardwebanwendung einen Zeichenfolgenwert für den Schlüssel für Benutzerkonten an. ASP.NET Identity können Sie den Typ des Schlüssels, der Ihren Anforderungen Daten zu ändern. Beispielsweise können Sie den Typ des Schlüssels aus einer Zeichenfolge in eine ganze Zahl ändern.
> 
> In diesem Thema wird gezeigt, wie start mit dem Standardwert-Webanwendung und ändern Sie den kontoschlüssel für den Benutzer in eine ganze Zahl. Die gleichen Änderungen können Sie jeder Typ des Schlüssels in Ihrem Projekt implementieren. Es zeigt, wie diese Änderungen in der standardwebanwendung übernommen, aber Sie können ähnliche Änderungen an einer benutzerdefinierten Anwendung anwenden. Es zeigt die Änderungen, die beim Arbeiten mit MVC oder Web Forms erforderlich.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Visual Studio 2013 Update 2 (oder höher)
> - ASP.NET Identity 2.1 oder höher


Um die Schritte in diesem Lernprogramm ausführen zu können, benötigen Sie Visual Studio 2013 Update 2 (oder höher) und eine Webanwendung, die mithilfe der ASP.NET Web Application-Vorlage erstellt. Die Vorlage, die in Update 3 geändert wird. In diesem Thema zeigt, wie die Vorlage in Update 2 und Update 3 zu ändern.

Dieses Thema enthält folgende Abschnitte:

- [Ändern Sie den Typ des Schlüssels in die Identity-Klasse](#userclass)
- [Hinzufügen von benutzerdefinierten Identität-Klassen, die den Typ des Schlüssels verwenden](#customclass)
- [Ändern Sie die Kontext-Klasse und Benutzer-Manager, um den Typ des Schlüssels verwenden](#context)
- [Start-Konfiguration ändern, verwenden Sie den Typ des Schlüssels](#startup)
- [Für MVC mit Update 2 ändern Sie die AccountController übergeben Sie den Typ des Schlüssels](#mvcupdate2)
- [Für MVC mit Update 3 zu ändern, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.](#mvcupdate3)
- [Ändern Sie für Web Forms Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate2)
- [Ändern Sie für Webformulare mit Update 3 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate3)
- [Führen Sie die Anwendung](#run)
- [Weitere Ressourcen](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Ändern Sie den Typ des Schlüssels in die Identity-Klasse

Geben Sie in Ihrem Projekt mithilfe der ASP.NET Web Application-Vorlage erstellt, dass die ApplicationUser-Klasse eine ganze Zahl für den Schlüssel für Benutzerkonten verwendet. Ändern Sie die ApplicationUser-Klasse aus IdentityUser erben, die den Typ aufweist, in IdentityModels.cs, **Int** für den generischen TKey-Parameter. Sie übergeben außerdem die Namen der drei benutzerdefinierte Klasse, die noch nicht implementiert.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Sie haben den Typ des Schlüssels geändert, aber standardmäßig der Rest der Anwendung weiterhin geht davon aus, dass der Schlüssel eine Zeichenfolge ist. Sie müssen explizit den Typ des Schlüssels im Code angeben, die eine Zeichenfolge annimmt.

In der **ApplicationUser** Klasse, ändern Sie die **GenerateUserIdentityAsync** Methode, um "Int", enthalten, wie in den folgenden hervorgehobenen Code gezeigt. Diese Änderung ist nicht für Web Forms-Projekte mit der Vorlage Update 3 erforderlich.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Hinzufügen von benutzerdefinierten Identität-Klassen, die den Typ des Schlüssels verwenden

Die anderen Identity-Klassen, z. B. IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, werden weiterhin einen Zeichenfolgenschlüssel verwendet eingerichtet. Erstellen Sie neue Versionen dieser Klassen, die eine ganze Zahl für den Schlüssel angeben. Sie müssen nicht viel Implementierungscode in diesen Klassen bereitstellen, Int in erster Linie nur als Schlüssel festlegen.

Die folgenden Klassen der Datei IdentityModels.cs hinzufügen.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Ändern Sie die Kontext-Klasse und Benutzer-Manager, um den Typ des Schlüssels verwenden

In IdentityModels.cs, ändern Sie die Definition der **ApplicationDbContext** Klasse zur Verwendung der neuen angepasst, Klassen und ein **Int** für den Schlüssel, wie in der hervorgehobene Code gezeigt.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

Der ThrowIfV1Schema-Parameter ist nicht mehr gültig ist, im Konstruktor. Ändern Sie den Konstruktor aus, damit er nicht auf einen ThrowIfV1Schema-Wert übergeben wird.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

IdentityConfig.cs öffnen, und Ändern der **ApplicationUserManger** Klasse mit der neue Benutzer speichern Klasse für das Beibehalten von Daten und ein **Int** für den Schlüssel.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

In der Vorlage Update 3 müssen Sie die Klasse ApplicationSignInManager ändern.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Start-Konfiguration ändern, verwenden Sie den Typ des Schlüssels

Ersetzen Sie im Startup.Auth.cs den OnValidateIdentity-Code, wie unten markiert. Beachten Sie, dass die Definition der GetUserIdCallback analysiert den Zeichenfolgenwert in eine ganze Zahl.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Wenn das Projekt nicht die generische Implementierung erkennt die **BenutzerID** -Methode, müssen Sie möglicherweise die ASP.NET Identity NuGet-Paket auf Version 2.1 aktualisieren

Sie haben den Infrastruktur-Klassen, die von ASP.NET Identity verwendet viele Änderungen vorgenommen. Wenn Sie versuchen, das Projekt kompilieren, bemerken Sie viele Fehler. Glücklicherweise ähneln die restlichen Fehler. Die Identity-Klasse erwartet eine ganze Zahl für den Schlüssel, aber der Controller (oder Web Form) ist einen String-Wert übergeben. In jedem Fall müssen Sie durch Aufrufen von einer Zeichenfolge und ganze Zahl konvertieren **BenutzerID&lt;Int&gt;**. Sie können die Fehlerliste Kompilierung zu absolvieren, oder führen Sie die folgenden Änderungen.

Die verbleibenden Änderungen hängt vom Typ des Projekts, die Sie erstellen und die Updates, die Sie in Visual Studio installiert haben. Sie können direkt an den entsprechenden Abschnitt über die folgenden Links wechseln.

- [Für MVC mit Update 2 ändern Sie die AccountController übergeben Sie den Typ des Schlüssels](#mvcupdate2)
- [Für MVC mit Update 3 zu ändern, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.](#mvcupdate3)
- [Ändern Sie für Web Forms Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate2)
- [Ändern Sie für Webformulare mit Update 3 Konto-Seiten, um den Typ des Schlüssels zu übergeben.](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>Für MVC mit Update 2 ändern Sie die AccountController übergeben Sie den Typ des Schlüssels

Öffnen Sie die AccountController.cs-Datei. Sie müssen die folgenden Methoden ändern.

**ConfirmEmail** Methode

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Heben Sie die Zuordnung** Methode

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
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Für MVC mit Update 3 zu ändern, die AccountController und ManageController, um den Typ des Schlüssels zu übergeben.

Öffnen Sie die AccountController.cs-Datei. Sie müssen die folgende Methode zu ändern.

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
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Ändern Sie für Web Forms Update 2 Konto-Seiten, um den Typ des Schlüssels zu übergeben.

Für Web Forms Update 2 müssen Sie die folgenden Seiten zu ändern.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Sie können jetzt [führen Sie die Anwendung](#run) und einen neuen Benutzer registrieren.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Ändern Sie für Webformulare mit Update 3 Konto-Seiten, um den Typ des Schlüssels zu übergeben.

Für Web Forms mit Update 3 müssen Sie die folgenden Seiten zu ändern.

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
## <a name="run-application"></a>Führen Sie die Anwendung

Sie haben alle erforderlichen Änderungen an Standardeinstellung Web Application-Vorlage. Führen Sie die Anwendung, und registrieren Sie einen neuen Benutzer. Nach dem Registrieren des Benutzers, werden Sie feststellen, dass die AspNetUsers-Tabelle eine Id-Spalte enthält, die eine ganze Zahl ist.

![neuen Primärschlüssel](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Wenn Sie zuvor die ASP.NET-Identität Tabellen mit einem anderen primären Schlüssel erstellt haben, müssen Sie einige zusätzliche Änderungen vornehmen. Wenn möglich, löschen Sie einfach die vorhandene Datenbank ein. Die Datenbank wird mit dem richtigen Entwurf neu erstellt werden, wenn führen Sie die Webanwendung und fügen Sie einen neuen Benutzer hinzu. Wenn der Löschvorgang nicht möglich ist, führen Sie Code first-Migrationen um Tabellen zu ändern. Allerdings wird der neue Primärschlüssel ganze Zahl nicht als eine SQL-IDENTITY-Eigenschaft in der Datenbank eingerichtet werden. Sie müssen manuell die Id-Spalte als Identität festlegen.

<a id="other"></a>
## <a name="other-resources"></a>Weitere Ressourcen

- [Übersicht über benutzerdefinierte Speicheranbieter für ASP.NET Identity](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrieren einer vorhandenen Website von einem SQL-Mitgliedschaftsanbieter nach ASP.NET Identity](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrieren von universellen Anbieterdaten Mitgliedschaft und zu ASP.NET Identity Benutzerprofile](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Beispielanwendung](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) mit geänderten Primärschlüssel
