---
title: Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt
author: rick-anderson
description: Ermitteln Sie Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt.
manager: wpickett
ms.author: riande
ms.date: 11/30/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/individual
ms.openlocfilehash: 40715debb48c0a7121ce84d7843b8517b0973e74
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artikel basierend auf ASP.NET Core-Projekte mit Konten einzelner Benutzer erstellt

ASP.NET Core Identität ist in-Projektvorlagen in Visual Studio mit der Option "Einzelne Benutzerkonten" enthalten.

Die Authentifizierung Vorlagen stehen in .NET Core CLI mit `-au Individual`:

```console
dotnet new mvc -au Individual
dotnet new webapi -au Individual
dotnet new razor -au Individual
```

In den folgenden Artikeln veranschaulichen, wie den Code in ASP.NET Core Vorlagen, mit denen einzelne Benutzerkonten generiert:

* [Zweistufige Authentifizierung mit SMS](xref:security/authentication/2fa)
* [Account confirmation and password recovery in ASP.NET Core (Kontobestätigung und Kennwortwiederherstellung in ASP.NET Core)](xref:security/authentication/accconfirm)
* [Erstellen einer ASP.NET Core-app mit Benutzerdaten durch Autorisierung geschützt](xref:security/authorization/secure-data)
