---
title: Anspruchsbasierte Autorisierung
author: rick-anderson
description: "Dieses Dokument erläutert, wie Ansprüche Überprüfungen für die Autorisierung in einer ASP.NET Core-app hinzugefügt."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 76b6566df4a427836eb5060f7d80e1039e479884
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="cdb9f-103">Anspruchsbasierte Autorisierung</span><span class="sxs-lookup"><span data-stu-id="cdb9f-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="cdb9f-104">Wenn eine Identität erstellt wird möglicherweise eine oder mehrere Ansprüche, die von einer vertrauenswürdigen Partei ausgestellt zugewiesen werden.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="cdb9f-105">Ein Anspruch ist der Name-Wert-Paar, das stellt, welche den Betreff dar ist, nicht welche das Subjekt möglich.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="cdb9f-106">Sie möglicherweise z. B. ein Führerschein, von einer lokalen treibenden Lizenz Zertifizierungsstelle ausgestellt.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="cdb9f-107">Der Führerschein weist Ihr Geburtsdatum auf.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="cdb9f-108">In diesem Fall wäre die anspruchsnamen `DateOfBirth`, der Wert des Anspruchs wäre Ihr Geburtsdatum, z. B. `8th June 1970` und der Aussteller wäre die Autorität für die steuernde Lizenz.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="cdb9f-109">Anspruchsbasierte Autorisierung in seiner einfachsten Form überprüft den Wert des Anspruchs und ermöglicht den Zugriff auf eine Ressource auf Grundlage dieses Werts.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="cdb9f-110">Für das Beispiel, wenn Sie möchten den Autorisierungsprozess Zugriff auf eine Nacht Club sein könnten:</span><span class="sxs-lookup"><span data-stu-id="cdb9f-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="cdb9f-111">Die Tür Sicherheitsbeauftragten ausgewertet den Wert der das Datum des Anspruchs Geburtsdatum und gibt an, ob sie den Aussteller (die steuernde Lizenz Authority) vertrauen, bevor Sie Ihnen den Zugriff gewähren.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="cdb9f-112">Eine Identität kann mehrere Ansprüche mit mehreren Werten enthalten und kann mehrere Ansprüche vom selben Typ enthalten.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="cdb9f-113">Hinzufügen von Anspruchsanbieter-Überprüfungen</span><span class="sxs-lookup"><span data-stu-id="cdb9f-113">Adding claims checks</span></span>

<span data-ttu-id="cdb9f-114">Anspruch basierend autorisierungsprüfungen sind deklarative – der Entwickler bettet sie innerhalb ihres Codes für einen Controller oder einer Aktion innerhalb eines Controllers angeben Ansprüche die muss der aktuelle Benutzer besitzt und optional des Werts der Anspruch muss für den Zugriff auf die angeforderte Ressource.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="cdb9f-115">Ansprüche an, dass Anforderungen richtlinienbasierten sind, muss der Entwickler erstellen und registrieren eine Richtlinie auszudrücken, die Ansprüche-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="cdb9f-116">Die einfachste Art des Anspruchs Richtlinie sieht das Vorhandensein eines Anspruchs und überprüfen Sie den Wert nicht.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="cdb9f-117">Zuerst müssen Sie zum Erstellen und registrieren die Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-117">First you need to build and register the policy.</span></span> <span data-ttu-id="cdb9f-118">Dies erfolgt im Rahmen der Dienstkonfiguration Autorisierung nimmt normalerweise Teil in `ConfigureServices()` in Ihrer *Startup.cs* Datei.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="cdb9f-119">In diesem Fall die `EmployeeOnly` Richtlinie überprüft das Vorhandensein einer `EmployeeNumber` Anspruch auf die aktuelle Identität.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="cdb9f-120">Wenden Sie dann die Richtlinie mithilfe der `Policy` Eigenschaft auf die `AuthorizeAttribute` -Attribut angeben, den Richtliniennamen;</span><span class="sxs-lookup"><span data-stu-id="cdb9f-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="cdb9f-121">Die `AuthorizeAttribute` Attribut kann auf eine gesamte Controller, in dieser Instanz angewendet werden, nur die Abgleichsrichtlinie Identitäten den Zugriff auf eine beliebige Aktion auf dem Controller werden darf.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="cdb9f-122">Wenn Sie über einen verwaltungscontroller verfügen, die durch geschützt ist die `AuthorizeAttribute` -Attribut jedoch anonymen Zugriff auf bestimmte Aktionen ermöglichen Sie anwenden möchten die `AllowAnonymousAttribute` Attribut.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="cdb9f-123">Die meisten Ansprüche verfügen über einen Wert.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-123">Most claims come with a value.</span></span> <span data-ttu-id="cdb9f-124">Sie können eine Liste der zulässigen Werte angeben, wenn die Richtlinie erstellen.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="cdb9f-125">Im folgende Beispiel würde nur für Mitarbeiter erfolgreich verlaufen, deren Spaltenanzahl von Mitarbeiter 1, 2, 3, 4 oder 5 wurde.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="cdb9f-126">Mehrfache Richtlinienauswertung</span><span class="sxs-lookup"><span data-stu-id="cdb9f-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="cdb9f-127">Wenn Sie mehrere Richtlinien auf einen Controller oder eine Aktion anwenden, müssen alle Richtlinien übergeben, bevor der Zugriff gewährt wird.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="cdb9f-128">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="cdb9f-128">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="cdb9f-129">Im obigen Beispiel Identität, die erfüllt die `EmployeeOnly` Richtlinie erreichen der `Payslip` erreichen Sie, wenn diese Richtlinie wird erzwungen, auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="cdb9f-130">Jedoch für den Aufruf der `UpdateSalary` Aktion, die die Identität muss erfüllen *beide* der `EmployeeOnly` Richtlinie und die `HumanResources` Richtlinie.</span><span class="sxs-lookup"><span data-stu-id="cdb9f-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="cdb9f-131">Wenn etwas kompliziertere Richtlinien werden sollen, z. B. ein Datum Geburtsdatum Anspruchs geschaltet wurde, ein Alter daraus berechnen, und klicken Sie dann überprüfen die ALTER 21 oder ältere stellen Sie Sie schreiben müssen [benutzerdefinierte Richtlinie Handler](policies.md).</span><span class="sxs-lookup"><span data-stu-id="cdb9f-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
