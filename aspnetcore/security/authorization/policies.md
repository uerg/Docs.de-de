---
title: Gruppenrichtlinien-basierte Autorisierung in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Erstellen und Verwenden von Authorization Policy Handler zum Erzwingen von autorisierungsanforderungen in einer ASP.NET Core-app.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Gruppenrichtlinien-basierte Autorisierung in ASP.NET Core

Im Hintergrund [rollenbasierte Autorisierung](xref:security/authorization/roles) und [anspruchsbasierte Autorisierung](xref:security/authorization/claims) erforderlich, einen Handler für die Anforderung und eine vorkonfigurierte Richtlinie verwenden. Diese Bausteine unterstützen den Ausdruck der auswertungen für die Autorisierung im Code. Das Ergebnis ist eine umfangreichere, wiederverwendbare, testfähig Autorisierung-Struktur.

Eine Autorisierungsrichtlinie besteht aus einer oder mehreren Anforderungen. Wird als Teil der Dienstkonfiguration Autorisierung in registriert die `Startup.ConfigureServices` Methode:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

Im vorherigen Beispiel wird eine Richtlinie "AtLeast21" erstellt. Es wurde eine einzelne Anforderung&mdash;, die von einem Mindestalter, die als Parameter an die Anforderung bereitgestellt wird.

Richtlinien werden angewendet, mit der `[Authorize]` Attribut mit dem Richtliniennamen. Zum Beispiel:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Anforderungen

Eine autorisierungsanforderung ist eine Auflistung von Datenparametern, die eine Richtlinie zum Auswerten des aktuellen Benutzerprinzipals verwenden können. In unserer "AtLeast21"-Richtlinie, die Anforderung ist ein einzelner Parameter&mdash;das Mindestalter. Implementiert eine Anforderung [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), dies ist eine leere Markierungsschnittstelle. Eine parametrisierte Mindestalter-Anforderung kann wie folgt implementiert:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Eine Anforderung benötigen nicht Daten oder Eigenschaften.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Autorisierung Handler

Ein Authorization-Handler ist für die Auswertung von Eigenschaften für eine Anforderung verantwortlich. Der Handler für die Autorisierung wertet die Anforderungen für ein bereitgestelltes [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) zu bestimmen, ob der Zugriff zulässig ist.

Kann die Anforderung besteht [mehrere Handler](#security-authorization-policies-based-multiple-handlers). Ein Ereignishandler kann erben [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), wobei `TRequirement` ist die Anforderung behandelt werden. Alternativ können Sie ein Handler implementiert möglicherweise [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) , mehr als eine Art der Anforderung zu behandeln.

### <a name="use-a-handler-for-one-requirement"></a>Verwenden Sie einen Handler für eine Anforderung

<a name="security-authorization-handler-example"></a>

Im folgenden finden ein Beispiel für eine direkte Beziehung, in der ein Mindestalter-Handler eine einzelne Anforderung nutzt:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Der vorangehende Code bestimmt, ob der aktuelle Benutzer principal aufweist, ein Geburtsdatum Anspruch, der von einem bekannten und vertrauenswürdigen Aussteller ausgegeben wurde. Autorisierung kann nicht auftreten, wenn der Anspruch nicht vorhanden ist, in diesem Fall eine vollständige Aufgabe zurückgegeben hat. Wenn ein Anspruch vorhanden ist, wird das Alter des Benutzers berechnet. Wenn der Benutzer das Mindestalter definiert, indem Sie die Anforderung erfüllt, wird die Autorisierung erfolgreich angesehen. Wenn die Autorisierung erfolgreich war, `context.Succeed` mit zufrieden Anforderung als einzigen Parameter aufgerufen wird.

### <a name="use-a-handler-for-multiple-requirements"></a>Verwenden Sie einen Ereignishandler für mehrere Anforderungen

Im folgenden finden ein Beispiel für eine 1: n-Beziehung, in der ein Berechtigung-Handler drei Anforderungen nutzt:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Der vorangehende Code durchläuft [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;eine Eigenschaft nicht mit der Anforderungen als erfolgreich gekennzeichnet. Wenn der Benutzer eine Leseberechtigung verfügt, muss er entweder "Besitzer" oder "Sponsor" aus, Zugriff auf die angeforderte Ressource sein. Wenn der Benutzer hat bearbeiten oder löschen, muss er Besitzer Zugriff auf die angeforderte Ressource sein. Wenn die Autorisierung erfolgreich war, `context.Succeed` mit zufrieden Anforderung als einzigen Parameter aufgerufen wird.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Handler-Registrierung

Handler werden in der Auflistung der Dienste während der Konfiguration registriert. Zum Beispiel:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Jeder Handler, die Dienste-Auflistung hinzugefügt wird, durch den Aufruf `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Was sollte ein Handler zurückgeben?

Beachten Sie, dass die `Handle` Methode in der [Handler Beispiel](#security-authorization-handler-example) gibt keinen Wert zurück. Wie der Status Erfolg oder Fehler angezeigt wird?

* Ein Handler gibt die erfolgreiche Ausführung durch den Aufruf `context.Succeed(IAuthorizationRequirement requirement)`, übergeben der Anforderung, die erfolgreich überprüft wurde.

* Ein Handler muss im allgemeinen Fehler zu behandeln, wie andere Handler für die gleiche Anforderung erfolgreich ausgeführt werden können.

* Aufrufen, um Fehler, auch wenn andere Handler für die Anforderung erfolgreich zu garantieren, `context.Fail`.

Bei Festlegung auf `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) -Eigenschaft (verfügbar in ASP.NET Core 1.1 und höher) kurzgeschlossen wird die Ausführung der Ereignishandler beim `context.Fail` aufgerufen wird. `InvokeHandlersAfterFailure` Standardmäßig `true`, in diesem Fall alle Ereignishandler aufgerufen werden. Anforderungen für die Nebeneffekte, wie z. B. Protokollierung, erzeugen, die immer stattfinden dadurch auch wenn `context.Fail` in einen anderen Handler aufgerufen wurde.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Warum würde ich mehrere Handler für eine Anforderung?

In Fällen soll Auswertung auf eine **oder** Basis, mehrere Handler für eine einzelne Anforderung zu implementieren. Microsoft hat es sich beispielsweise um Türen, die nur mit Schlüssel Karten zu öffnen. Wenn Sie Ihre Schlüsselkarte zu Hause lassen, wird die Apparate druckt einen temporären Aufkleber und öffnet die Tür für Sie. In diesem Fall müssten Sie eine einzelne Anforderung *BuildingEntry*, aber mehrere Handler, die jeweils eine einzelne Anforderung überprüfen.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Stellen Sie sicher, dass die beiden Ereignishandler sind [registriert](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Wenn die beiden Ereignishandler ist erfolgreich, wenn eine Richtlinie ausgewertet wird die `BuildingEntryRequirement`, richtlinienauswertung erfolgreich ausgeführt wird.

## <a name="using-a-func-to-fulfill-a-policy"></a>Func verwenden, um eine Richtlinie zu erfüllen.

Möglicherweise gibt es Situationen, in welche, die verhindert eine Richtlinie einfach zu express im Code ist. Es ist möglich, geben Sie einen `Func<AuthorizationHandlerContext, bool>` beim Konfigurieren der Richtlinie mit den `RequireAssertion` Richtlinie-Generator.

Beispielsweise der vorherigen `BadgeEntryHandler` könnte folgendermaßen umgeschrieben werden:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Zugreifen auf die MVC-Anforderungskontext in Handlern abfangen

Die `HandleRequirementAsync` Methode, die Sie, in einem Ereignishandler für die Autorisierung implementieren verfügt über zwei Parameter: eine `AuthorizationHandlerContext` und `TRequirement` Sie verarbeiten. Frameworks wie z. B. Mvc- oder Jabbr sind frei, um ein Objekt zum Hinzufügen der `Resource` Eigenschaft auf die `AuthorizationHandlerContext` um zusätzliche Informationen zu übergeben.

Beispielsweise MVC übergibt eine Instanz des [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in der `Resource` Eigenschaft. Diese Eigenschaft ermöglicht den Zugriff auf `HttpContext`, `RouteData`, und alles, was sonst von Razor-Seiten und MVC bereitgestellt.

Die Verwendung der `Resource` Eigenschaft ist für bestimmte Framework. Mithilfe der Informationen in der `Resource` Eigenschaft schränkt die Autorisierungsrichtlinien auf bestimmten Frameworks. Sollten Sie eine Umwandlung der `Resource` Eigenschaft mit der `as` -Schlüsselwort, und bestätigen Sie dann die Umwandlung wurde erfolgreich ausgeführt werden, um sicherzustellen, dass Ihr Code keine stürzt ab mit einer `InvalidCastException` bei Ausführung auf anderen Frameworks:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
