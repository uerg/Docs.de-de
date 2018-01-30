---
title: Kennwort-Hashing
author: rick-anderson
description: "Dieses Dokument erläutert, wie Kennwörter, die mit der ASP.NET Core-Datenschutz APIs Hash."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 536499687865b4e4d5b1d9c4076623b5ac1ffdbe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="password-hashing"></a>Kennwort-Hashing

Die Data Protection Codebasis enthält ein Paket *Microsoft.AspNetCore.Cryptography.KeyDerivation* die kryptografischen Schlüssel Ableitung Funktionen enthält. Dieses Paket ist eine eigenständige Komponente und ist völlig unabhängig von der Rest des Systems Schutz Daten. Es kann vollständig unabhängig verwendet werden. Die Quelle, die zusammen mit den Data Protection Codebasis als Annehmlichkeit vorhanden ist.

Das Paket gibt es derzeit eine Methode `KeyDerivation.Pbkdf2` , sodass ein Kennwort verwendet Hashverfahren der [PBKDF2 Algorithmus](https://tools.ietf.org/html/rfc2898#section-5.2). Diese API ist vergleichbar mit der .NET Framework vorhandenen [Rfc2898DeriveBytes Typ](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes), aber es drei wichtige Unterschiede gibt:

1. Die `KeyDerivation.Pbkdf2` Methode unterstützt den Verbrauch von mehreren PRFs (derzeit `HMACSHA1`, `HMACSHA256`, und `HMACSHA512`), während die `Rfc2898DeriveBytes` geben unterstützt nur `HMACSHA1`.

2. Die `KeyDerivation.Pbkdf2` Methode das aktuelle Betriebssystem erkennt und versucht, die am häufigsten optimierte Implementierung der Routine, viel bessere Leistung in bestimmten Fällen bereitstellen auswählen. (Unter Windows 8 bietet ungefähr 10 Mal den Durchsatz von `Rfc2898DeriveBytes`.)

3. Die `KeyDerivation.Pbkdf2` Methode muss der Aufrufer angeben aller Parameter (PRF und Anzahl der Szenarioiterationen Salzkonzentration). Die `Rfc2898DeriveBytes` Typ stellt die Standardwerte für diese bereit.

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

Finden Sie im Quellcode für ASP.NET Core Identity `PasswordHasher` Typ für einen realen Groß-/Kleinschreibung verwendet.
