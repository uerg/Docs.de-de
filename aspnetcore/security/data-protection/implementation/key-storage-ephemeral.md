---
title: Kurzlebige Schutz-Datenanbietern in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr Details zur Implementierung der ASP.NET Core kurzlebige Schutz Datenanbieter.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: 9cdd3949826091807f3139f51aa0c05ddaac696a
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="ephemeral-data-protection-providers-in-aspnet-core"></a>Kurzlebige Schutz-Datenanbietern in ASP.NET Core

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Es gibt Szenarien, in denen eine Anwendung einen Weg werfen benötigt `IDataProtectionProvider`. Z. B. der Entwickler möglicherweise nur in einer einmaligen Konsolenanwendung experimentieren werden oder die Anwendung selbst ist vorübergehend (es wird ein Skript erstellt oder ein Komponententestprojekt). Um diese Szenarien zu unterstützen die [Microsoft.AspNetCore.DataProtection](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection/) Paket enthält einen Typ `EphemeralDataProtectionProvider`. Dieser Typ stellt eine grundlegende Implementierung der `IDataProtectionProvider` , dessen Schlüssel Repository ausschließlich im Arbeitsspeicher gehalten wird, und ist nicht in einen Sicherungsspeicher geschrieben.

Jede Instanz des `EphemeralDataProtectionProvider` einen eigenen eindeutigen Hauptschlüssel verwendet. Aus diesem Grund Wenn ein `IDataProtector` als Stammknoten ein `EphemeralDataProtectionProvider` generiert eine geschützte Nutzlast diese Nutzlast kann nur durch ein entsprechendes ungeschützt werden `IDataProtector` (erhält die gleiche [Zweck](xref:security/data-protection/consumer-apis/purpose-strings#data-protection-consumer-apis-purposes) Kette) am gleichen Stamm `EphemeralDataProtectionProvider` -Instanz.

Das folgende Beispiel veranschaulicht die Instanziierung einer `EphemeralDataProtectionProvider` und dazu verwenden, schützen und Aufheben des Schutzes von Daten.

```csharp
using System;
using Microsoft.AspNetCore.DataProtection;

public class Program
{
    public static void Main(string[] args)
    {
        const string purpose = "Ephemeral.App.v1";

        // create an ephemeral provider and demonstrate that it can round-trip a payload
        var provider = new EphemeralDataProtectionProvider();
        var protector = provider.CreateProtector(purpose);
        Console.Write("Enter input: ");
        string input = Console.ReadLine();

        // protect the payload
        string protectedPayload = protector.Protect(input);
        Console.WriteLine($"Protect returned: {protectedPayload}");

        // unprotect the payload
        string unprotectedPayload = protector.Unprotect(protectedPayload);
        Console.WriteLine($"Unprotect returned: {unprotectedPayload}");

        // if I create a new ephemeral provider, it won't be able to unprotect existing
        // payloads, even if I specify the same purpose
        provider = new EphemeralDataProtectionProvider();
        protector = provider.CreateProtector(purpose);
        unprotectedPayload = protector.Unprotect(protectedPayload); // THROWS
    }
}

/*
* SAMPLE OUTPUT
*
* Enter input: Hello!
* Protect returned: CfDJ8AAAAAAAAAAAAAAAAAAAAA...uGoxWLjGKtm1SkNACQ
* Unprotect returned: Hello!
* << throws CryptographicException >>
*/
```
