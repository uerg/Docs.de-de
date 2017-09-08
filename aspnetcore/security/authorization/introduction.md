---
title: "Einführung"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a6a556ed-ba59-4107-9358-44cf20e5931b
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/introduction
ms.openlocfilehash: 040525505a982fc1be1901effb9186a8fe1cdbdf
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="introduction"></a><span data-ttu-id="b8f9f-103">Einführung</span><span class="sxs-lookup"><span data-stu-id="b8f9f-103">Introduction</span></span>

<a name=security-authorization-introduction></a>

<span data-ttu-id="b8f9f-104">Autorisierung bezieht sich auf den Prozess, der bestimmt, was ein Benutzer ausführen können, ist.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="b8f9f-105">Beispielsweise ist ein Administrator zum Erstellen einer Dokumentbibliothek Dokumente hinzufügen, Bearbeiten von Dokumenten und löschen Sie diese zulässig.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="b8f9f-106">Ein Benutzer ohne Administratorrechte arbeiten mit der Bibliothek ist nur autorisiert, auf die Dokumente zu lesen.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="b8f9f-107">Autorisierung ist orthogonale und unabhängig von der Authentifizierung, dabei der wird ermitteln, wer ein Benutzer ist.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-107">Authorization is orthogonal and independent from authentication, which is the process of ascertaining who a user is.</span></span> <span data-ttu-id="b8f9f-108">Authentifizierung kann eine oder mehrere Identitäten für den aktuellen Benutzer zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-108">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="b8f9f-109">Autorisierungstypen</span><span class="sxs-lookup"><span data-stu-id="b8f9f-109">Authorization Types</span></span>

<span data-ttu-id="b8f9f-110">ASP.NET Core Autorisierung bietet eine einfache deklarative [Rolle](roles.md#security-authorization-role-based) und ein [umfangreiche richtlinienbasierten](policies.md#security-authorization-policies-based) Modell.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-110">ASP.NET Core authorization provides a simple declarative [role](roles.md#security-authorization-role-based) and a [rich policy based](policies.md#security-authorization-policies-based) model.</span></span> <span data-ttu-id="b8f9f-111">Autorisierung ist in Anforderungen ausgedrückt und Handler auswerten ein Benutzer Ansprüche für Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-111">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="b8f9f-112">Imperative Überprüfungen können basieren auf einfachen oder Richtlinien für die Bewerten der Benutzeridentität und die Eigenschaften der Ressource, die der Benutzer zugreifen möchte.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-112">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="b8f9f-113">Namespaces</span><span class="sxs-lookup"><span data-stu-id="b8f9f-113">Namespaces</span></span>

<span data-ttu-id="b8f9f-114">Autorisierungskomponenten, einschließlich der `AuthorizeAttribute` und `AllowAnonymousAttribute` Attribute befinden sich in der `Microsoft.AspNetCore.Authorization` Namespace.</span><span class="sxs-lookup"><span data-stu-id="b8f9f-114">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>
