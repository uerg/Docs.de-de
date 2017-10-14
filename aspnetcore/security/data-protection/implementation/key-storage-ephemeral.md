---
title: "Schutz für kurzlebige Datenanbieter"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: af6ea1d0-0d9d-41df-a870-5dda24978e2f
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/implementation/key-storage-ephemeral
ms.openlocfilehash: ee8dccac3ba990b110758042192779426b01fc53
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2017
---
# <a name="ephemeral-data-protection-providers"></a>Schutz für kurzlebige Datenanbieter

<a name="data-protection-implementation-key-storage-ephemeral"></a>

Es gibt Szenarien, in denen eine Anwendung einen Weg werfen IDataProtectionProvider benötigt. Z. B. der Entwickler möglicherweise nur in einer einmaligen Konsolenanwendung experimentieren werden oder die Anwendung selbst ist vorübergehend (es wird ein Skript erstellt oder ein Komponententestprojekt). Zur Unterstützung dieser Szenarios umfasst das Paket Microsoft.AspNetCore.DataProtection ein EphemeralDataProtectionProvider. Dieser Typ stellt eine grundlegende Implementierung der IDataProtectionProvider, dessen Schlüssel Repository ausschließlich im Arbeitsspeicher gehalten wird, und ist nicht in einen Sicherungsspeicher geschrieben.

Jede Instanz des EphemeralDataProtectionProvider verwendet einen eigenen eindeutigen Hauptschlüssel. Aus diesem Grund ein IDataProtector als Stammknoten einer EphemeralDataProtectionProvider generiert, wird eine geschützte Nutzlast diese Nutzlast kann nur werden nicht geschützt durch eine entsprechende IDataProtector (erhält die gleiche [Zweck](../consumer-apis/purpose-strings.md#data-protection-consumer-apis-purposes) Kette) am gleichen Stamm EphemeralDataProtectionProvider-Instanz.

Das folgende Beispiel zeigt eine EphemeralDataProtectionProvider instanziieren und verwenden ihn schützen und Aufheben des Schutzes von Daten.

```none
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
