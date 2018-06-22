---
title: Hash-Kennwörtern in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Kennwörter, die mithilfe der ASP.NET Core Data Protection-APIs zu hashen.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: aef22ab91e76afdb5f54dc37bcee7128420b6f3b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272986"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="d9c5d-103">Hash-Kennwörtern in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9c5d-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="d9c5d-104">Die Data Protection Codebasis enthält ein Paket *Microsoft.AspNetCore.Cryptography.KeyDerivation* die kryptografischen Schlüssel Ableitung Funktionen enthält.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="d9c5d-105">Dieses Paket ist eine eigenständige Komponente und ist völlig unabhängig von der Rest des Systems Schutz Daten.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="d9c5d-106">Es kann vollständig unabhängig verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-106">It can be used completely independently.</span></span> <span data-ttu-id="d9c5d-107">Die Quelle, die zusammen mit den Data Protection Codebasis als Annehmlichkeit vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="d9c5d-108">Das Paket gibt es derzeit eine Methode `KeyDerivation.Pbkdf2` , sodass ein Kennwort verwendet Hashverfahren der [PBKDF2 Algorithmus](https://tools.ietf.org/html/rfc2898#section-5.2).</span><span class="sxs-lookup"><span data-stu-id="d9c5d-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="d9c5d-109">Diese API ist vergleichbar mit der .NET Framework vorhandenen [Rfc2898DeriveBytes Typ](/dotnet/api/system.security.cryptography.rfc2898derivebytes), aber es drei wichtige Unterschiede gibt:</span><span class="sxs-lookup"><span data-stu-id="d9c5d-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="d9c5d-110">Die `KeyDerivation.Pbkdf2` Methode unterstützt den Verbrauch von mehreren PRFs (derzeit `HMACSHA1`, `HMACSHA256`, und `HMACSHA512`), während die `Rfc2898DeriveBytes` geben unterstützt nur `HMACSHA1`.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="d9c5d-111">Die `KeyDerivation.Pbkdf2` Methode das aktuelle Betriebssystem erkennt und versucht, die am häufigsten optimierte Implementierung der Routine, viel bessere Leistung in bestimmten Fällen bereitstellen auswählen.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="d9c5d-112">(Unter Windows 8 bietet ungefähr 10 Mal den Durchsatz von `Rfc2898DeriveBytes`.)</span><span class="sxs-lookup"><span data-stu-id="d9c5d-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="d9c5d-113">Die `KeyDerivation.Pbkdf2` Methode muss der Aufrufer angeben aller Parameter (PRF und Anzahl der Szenarioiterationen Salzkonzentration).</span><span class="sxs-lookup"><span data-stu-id="d9c5d-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="d9c5d-114">Die `Rfc2898DeriveBytes` Typ stellt die Standardwerte für diese bereit.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="d9c5d-115">Finden Sie im Quellcode für ASP.NET Core Identity `PasswordHasher` Typ für einen realen Groß-/Kleinschreibung verwendet.</span><span class="sxs-lookup"><span data-stu-id="d9c5d-115">See the source code for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
