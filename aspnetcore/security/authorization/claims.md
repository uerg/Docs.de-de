---
title: Anspruchsbasierte Autorisierung in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie Ansprüche Überprüfungen für die Autorisierung in einer ASP.NET Core app hinzufügen.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2018
---
# <a name="claims-based-authorization-in-aspnet-core"></a>Anspruchsbasierte Autorisierung in ASP.NET Core

<a name="security-authorization-claims-based"></a>

Wenn eine Identität erstellt wird möglicherweise eine oder mehrere Ansprüche, die von einer vertrauenswürdigen Partei ausgestellt zugewiesen werden. Ein Anspruch ist ein Name-Wert-Paar, welche das Subjekt darstellt, nicht welche das Subjekt möglich. Sie möglicherweise z. B. ein Führerschein, von einer lokalen treibenden Lizenz Zertifizierungsstelle ausgestellt. Der Führerschein weist Ihr Geburtsdatum auf. In diesem Fall wäre die anspruchsnamen `DateOfBirth`, der Wert des Anspruchs wäre Ihr Geburtsdatum, z. B. `8th June 1970` und der Aussteller wäre die Autorität für die steuernde Lizenz. Anspruchsbasierte Autorisierung in seiner einfachsten Form überprüft den Wert des Anspruchs und ermöglicht den Zugriff auf eine Ressource auf Grundlage dieses Werts. Für das Beispiel, wenn Sie möchten den Autorisierungsprozess Zugriff auf eine Nacht Club sein könnten:

Die Tür Sicherheitsbeauftragten ausgewertet den Wert der das Datum des Anspruchs Geburtsdatum und gibt an, ob sie den Aussteller (die steuernde Lizenz Authority) vertrauen, bevor Sie Ihnen den Zugriff gewähren.

Eine Identität kann mehrere Ansprüche mit mehreren Werten enthalten und kann mehrere Ansprüche vom selben Typ enthalten.

## <a name="adding-claims-checks"></a>Hinzufügen von Anspruchsanbieter-Überprüfungen

Anspruch basierend autorisierungsprüfungen sind deklarative – der Entwickler bettet sie innerhalb ihres Codes für einen Controller oder einer Aktion innerhalb eines Controllers angeben Ansprüche die muss der aktuelle Benutzer besitzt und optional des Werts der Anspruch muss für den Zugriff auf die angeforderte Ressource. Ansprüche an, dass Anforderungen richtlinienbasierten sind, muss der Entwickler erstellen und registrieren eine Richtlinie auszudrücken, die Ansprüche-Anforderungen.

Die einfachste Art des Anspruchs Richtlinie sieht das Vorhandensein eines Anspruchs und überprüfen Sie den Wert nicht.

Zuerst müssen Sie zum Erstellen und registrieren die Richtlinie. Dies erfolgt im Rahmen der Dienstkonfiguration Autorisierung nimmt normalerweise Teil in `ConfigureServices()` in Ihrer *Startup.cs* Datei.

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

In diesem Fall die `EmployeeOnly` Richtlinie überprüft das Vorhandensein einer `EmployeeNumber` Anspruch auf die aktuelle Identität.

Wenden Sie dann die Richtlinie mithilfe der `Policy` Eigenschaft auf die `AuthorizeAttribute` -Attribut angeben, den Richtliniennamen;

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

Die `AuthorizeAttribute` Attribut kann auf eine gesamte Controller, in dieser Instanz angewendet werden, nur die Abgleichsrichtlinie Identitäten den Zugriff auf eine beliebige Aktion auf dem Controller werden darf.

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

Wenn Sie über einen verwaltungscontroller verfügen, die durch geschützt ist die `AuthorizeAttribute` -Attribut jedoch anonymen Zugriff auf bestimmte Aktionen ermöglichen Sie anwenden möchten die `AllowAnonymousAttribute` Attribut.

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

Die meisten Ansprüche verfügen über einen Wert. Sie können eine Liste der zulässigen Werte angeben, wenn die Richtlinie erstellen. Im folgende Beispiel würde nur für Mitarbeiter erfolgreich verlaufen, deren Spaltenanzahl von Mitarbeiter 1, 2, 3, 4 oder 5 wurde.

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

### <a name="add-a-generic-claim-check"></a>Eine generische Anspruch Überprüfung hinzufügen

Wenn der Wert des Anspruchs ist nicht in einen einzelnen Wert oder eine Transformation erforderlich ist, verwenden Sie [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion). Weitere Informationen finden Sie unter [Func verwenden, um eine Richtlinie erfüllen](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).

## <a name="multiple-policy-evaluation"></a>Mehrfache Richtlinienauswertung

Wenn Sie mehrere Richtlinien auf einen Controller oder eine Aktion anwenden, müssen alle Richtlinien übergeben, bevor der Zugriff gewährt wird. Zum Beispiel:

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

Im obigen Beispiel Identität, die erfüllt die `EmployeeOnly` Richtlinie erreichen der `Payslip` erreichen Sie, wenn diese Richtlinie wird erzwungen, auf dem Controller. Jedoch für den Aufruf der `UpdateSalary` Aktion, die die Identität muss erfüllen *beide* der `EmployeeOnly` Richtlinie und die `HumanResources` Richtlinie.

Wenn etwas kompliziertere Richtlinien werden sollen, z. B. ein Datum Geburtsdatum Anspruchs geschaltet wurde, ein Alter daraus berechnen, und klicken Sie dann überprüfen die ALTER 21 oder ältere stellen Sie Sie schreiben müssen [benutzerdefinierte Richtlinie Handler](xref:security/authorization/policies).
