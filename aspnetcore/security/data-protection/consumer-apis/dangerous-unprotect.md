---
title: "Aufheben des Schutzes Nutzlasten, deren Schlüssel gesperrt wurden."
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 6c4e6591-45d2-4d25-855e-062ad352d648
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/dangerous-unprotect
ms.openlocfilehash: 44f21f380b994f46a8bb7368bca0cfc6e438ec4d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="unprotecting-payloads-whose-keys-have-been-revoked"></a><span data-ttu-id="119c9-103">Aufheben des Schutzes Nutzlasten, deren Schlüssel gesperrt wurden.</span><span class="sxs-lookup"><span data-stu-id="119c9-103">Unprotecting payloads whose keys have been revoked</span></span>

<a name=data-protection-consumer-apis-dangerous-unprotect></a>

<span data-ttu-id="119c9-104">Die ASP.NET Core Datenschutz-APIs sind nicht in erster Linie für unbestimmte Persistenz des vertraulichen Nutzlasten gedacht.</span><span class="sxs-lookup"><span data-stu-id="119c9-104">The ASP.NET Core data protection APIs are not primarily intended for indefinite persistence of confidential payloads.</span></span> <span data-ttu-id="119c9-105">Andere Technologien wie [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) und [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) eignen sich besser auf das Szenario der unbegrenzten Speicher und Verwaltungsfunktionen für entsprechend starken haben.</span><span class="sxs-lookup"><span data-stu-id="119c9-105">Other technologies like [Windows CNG DPAPI](https://msdn.microsoft.com/library/windows/desktop/hh706794%28v=vs.85%29.aspx) and [Azure Rights Management](https://technet.microsoft.com/library/jj585024.aspx) are more suited to the scenario of indefinite storage, and they have correspondingly strong key management capabilities.</span></span> <span data-ttu-id="119c9-106">Dies bedeutet, dass keine verbietet einen Entwickler mithilfe der ASP.NET Core Datenschutz-APIs für den langfristigen Schutz von vertraulichen Daten.</span><span class="sxs-lookup"><span data-stu-id="119c9-106">That said, there is nothing prohibiting a developer from using the ASP.NET Core data protection APIs for long-term protection of confidential data.</span></span> <span data-ttu-id="119c9-107">Schlüssel werden nie aus dem Ring Schlüssel entfernt, damit IDataProtector.Unprotect immer vorhandene Nutzlasten wiederherstellen können, solange die Schlüssel verfügbar und gültig sind.</span><span class="sxs-lookup"><span data-stu-id="119c9-107">Keys are never removed from the key ring, so IDataProtector.Unprotect can always recover existing payloads as long as the keys are available and valid.</span></span>

<span data-ttu-id="119c9-108">Allerdings tritt ein Problem auf, wenn der Entwickler beim Aufheben des Schutzes Daten, die mit einem gesperrten Schlüssel geschützt wurden, wie IDataProtector.Unprotect in diesem Fall eine Ausnahme auslöst, versucht.</span><span class="sxs-lookup"><span data-stu-id="119c9-108">However, an issue arises when the developer tries to unprotect data that has been protected with a revoked key, as IDataProtector.Unprotect will throw an exception in this case.</span></span> <span data-ttu-id="119c9-109">Dies kann gut für kurzlebige oder vorübergehender Nutzlasten (z. B.-Authentifizierungstoken) sein, wie diese Arten von Nutzlasten einfach vom System erstellt werden können, und im schlimmsten Fall der Besucher der Website möglicherweise erforderlich, um sich erneut anmelden.</span><span class="sxs-lookup"><span data-stu-id="119c9-109">This might be fine for short-lived or transient payloads (like authentication tokens), as these kinds of payloads can easily be recreated by the system, and at worst the site visitor might be required to log in again.</span></span> <span data-ttu-id="119c9-110">Aber für persistente Nutzlasten Unprotect lösen müssen kann zu Datenverlust nicht akzeptabel.</span><span class="sxs-lookup"><span data-stu-id="119c9-110">But for persisted payloads, having Unprotect throw could lead to unacceptable data loss.</span></span>

## <a name="ipersisteddataprotector"></a><span data-ttu-id="119c9-111">IPersistedDataProtector</span><span class="sxs-lookup"><span data-stu-id="119c9-111">IPersistedDataProtector</span></span>

<span data-ttu-id="119c9-112">Um das Szenario ermöglicht die Nutzlasten als auch bei widerrufenen Schlüssel ungeschützt zu unterstützen, enthält die Datenschutzsystem einen IPersistedDataProtector-Typ.</span><span class="sxs-lookup"><span data-stu-id="119c9-112">To support the scenario of allowing payloads to be unprotected even in the face of revoked keys, the data protection system contains an IPersistedDataProtector type.</span></span> <span data-ttu-id="119c9-113">Zum Abrufen einer Instanz von IPersistedDataProtector Abrufen einer Instanz von IDataProtector in die normale Art und Weise und wiederholen Sie dann die IDataProtector zum IPersistedDataProtector umwandeln.</span><span class="sxs-lookup"><span data-stu-id="119c9-113">To get an instance of IPersistedDataProtector, simply get an instance of IDataProtector in the normal fashion and try casting the IDataProtector to IPersistedDataProtector.</span></span>

> [!NOTE]
> <span data-ttu-id="119c9-114">Nicht alle IDataProtector-Instanzen können in IPersistedDataProtector umgewandelt werden.</span><span class="sxs-lookup"><span data-stu-id="119c9-114">Not all IDataProtector instances can be cast to IPersistedDataProtector.</span></span> <span data-ttu-id="119c9-115">Entwickler sollten verwenden C#-als-Operator oder ähnliche Laufzeitausnahmen vermieden, die durch ungültige Umwandlungen verursacht, und sie sollten darauf vorbereitet, die Groß-/Kleinschreibung Fehler entsprechend behandelt.</span><span class="sxs-lookup"><span data-stu-id="119c9-115">Developers should use the C# as operator or similar to avoid runtime exceptions caused by invalid casts, and they should be prepared to handle the failure case appropriately.</span></span>

<span data-ttu-id="119c9-116">IPersistedDataProtector macht die folgenden API-Oberfläche:</span><span class="sxs-lookup"><span data-stu-id="119c9-116">IPersistedDataProtector exposes the following API surface:</span></span>

```csharp
DangerousUnprotect(byte[] protectedData, bool ignoreRevocationErrors,
     out bool requiresMigration, out bool wasRevoked) : byte[]
   ```

<span data-ttu-id="119c9-117">Diese API nimmt die geschützte Nutzlast (als Bytearray) und gibt die ungeschützte Nutzlast.</span><span class="sxs-lookup"><span data-stu-id="119c9-117">This API takes the protected payload (as a byte array) and returns the unprotected payload.</span></span> <span data-ttu-id="119c9-118">Keine Überladung zeichenfolgenbasierte ist vorhanden.</span><span class="sxs-lookup"><span data-stu-id="119c9-118">There is no string-based overload.</span></span> <span data-ttu-id="119c9-119">Die beiden out-Parameter lauten wie folgt aus.</span><span class="sxs-lookup"><span data-stu-id="119c9-119">The two out parameters are as follows.</span></span>

* <span data-ttu-id="119c9-120">RequiresMigration: Einstellung "true", wenn der Schlüssel zum Schützen dieser Nutzlast nicht mehr das aktive Standardschlüssel ist, z. B. der Schlüssel zum Schützen dieser Nutzlast veraltet sind und ein parallelen Vorgang Schlüssel, seitdem hat durchzuführenden platzieren.</span><span class="sxs-lookup"><span data-stu-id="119c9-120">requiresMigration: will be set to true if the key used to protect this payload is no longer the active default key, e.g., the key used to protect this payload is old and a key rolling operation has since taken place.</span></span> <span data-ttu-id="119c9-121">Der Aufrufer möchten möglicherweise sollten die Nutzlast je nach ihren geschäftsanforderungen erneut zu schützen.</span><span class="sxs-lookup"><span data-stu-id="119c9-121">The caller may wish to consider reprotecting the payload depending on their business needs.</span></span>

* <span data-ttu-id="119c9-122">WasRevoked: wird auf "true", wenn der Schlüssel zum Schützen dieser Nutzlast widerrufen wurde festgelegt.</span><span class="sxs-lookup"><span data-stu-id="119c9-122">wasRevoked: will be set to true if the key used to protect this payload was revoked.</span></span>

>[!WARNING]
> <span data-ttu-id="119c9-123">Führen Sie äußersten vorsichtig vor, bei der Übergabe von IgnoreRevocationErrors: true, wenn die DangerousUnprotect-Methode.</span><span class="sxs-lookup"><span data-stu-id="119c9-123">Exercise extreme caution when passing ignoreRevocationErrors: true to the DangerousUnprotect method.</span></span> <span data-ttu-id="119c9-124">Wenn nach dem Aufrufen dieser Methode der WasRevoked-Wert "true" ist, klicken Sie dann der Schlüssel zum Schützen dieser Nutzlast wurde gesperrt und die Nutzlast Authentizität als fehlerverdächtig behandelt werden soll.</span><span class="sxs-lookup"><span data-stu-id="119c9-124">If after calling this method the wasRevoked value is true, then the key used to protect this payload was revoked, and the payload's authenticity should be treated as suspect.</span></span> <span data-ttu-id="119c9-125">In diesem Fall nur weiterhin für die Nutzlast der ungeschützt ausgeführt wird, haben einige separate Gewissheit, dass es authentisch ist, z. B., dass die It des stammen aus einer geschützten Datenbank, anstatt von einem nicht vertrauenswürdigen WebClient gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="119c9-125">In this case only continue operating on the unprotected payload if you have some separate assurance that it is authentic, e.g. that it's coming from a secure database rather than being sent by an untrusted web client.</span></span>

<span data-ttu-id="119c9-126">[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span><span class="sxs-lookup"><span data-stu-id="119c9-126">[!code-none[Main](dangerous-unprotect/samples/dangerous-unprotect.cs)]</span></span>
