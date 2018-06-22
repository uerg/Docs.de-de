---
title: Einführung in die Autorisierung in ASP.NET Core
author: rick-anderson
description: Grundlagen der Autorisierung und zur Funktionsweise von Autorisierung in ASP.NET Core-apps.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276866"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="e6cfa-103">Einführung in die Autorisierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e6cfa-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="e6cfa-104">Autorisierung bezieht sich auf den Prozess, der bestimmt, was ein Benutzer ausführen können, ist.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="e6cfa-105">Beispielsweise ist ein Administrator zum Erstellen einer Dokumentbibliothek Dokumente hinzufügen, Bearbeiten von Dokumenten und löschen Sie diese zulässig.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="e6cfa-106">Ein Benutzer ohne Administratorrechte arbeiten mit der Bibliothek ist nur autorisiert, auf die Dokumente zu lesen.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="e6cfa-107">Autorisierung ist orthogonale und unabhängig von der Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="e6cfa-108">Allerdings erfordert eine Autorisierung Authentifizierungsmechanismus.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="e6cfa-109">Authentifizierung versteht man das ermitteln, wer ein Benutzer ist.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="e6cfa-110">Authentifizierung kann eine oder mehrere Identitäten für den aktuellen Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="e6cfa-111">Autorisierungstypen</span><span class="sxs-lookup"><span data-stu-id="e6cfa-111">Authorization types</span></span>

<span data-ttu-id="e6cfa-112">ASP.NET Core Autorisierung bietet ein einfaches, deklaratives [Rolle](xref:security/authorization/roles) und eine umfangreiche [richtlinienbasierten](xref:security/authorization/policies) Modell.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="e6cfa-113">Autorisierung ist in Anforderungen ausgedrückt und Handler auswerten ein Benutzer Ansprüche für Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="e6cfa-114">Imperative Überprüfungen können basieren auf einfachen oder Richtlinien für die Bewerten der Benutzeridentität und die Eigenschaften der Ressource, die der Benutzer zugreifen möchte.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="e6cfa-115">Namespaces</span><span class="sxs-lookup"><span data-stu-id="e6cfa-115">Namespaces</span></span>

<span data-ttu-id="e6cfa-116">Autorisierungskomponenten, einschließlich der `AuthorizeAttribute` und `AllowAnonymousAttribute` Attribute, befinden sich in der `Microsoft.AspNetCore.Authorization` Namespace.</span><span class="sxs-lookup"><span data-stu-id="e6cfa-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="e6cfa-117">In der Dokumentation auf [einfache Autorisierung](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="e6cfa-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
