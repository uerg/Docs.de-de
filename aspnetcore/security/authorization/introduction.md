---
title: Einführung in die Autorisierung in ASP.NET Core
author: rick-anderson
description: Grundlagen der Autorisierung und zur Funktionsweise von Autorisierung in ASP.NET Core-apps.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: f969cb26d1fcddeac967b1e3d13e3c06ebc7631f
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483081"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a>Einführung in die Autorisierung in ASP.NET Core

<a name="security-authorization-introduction"></a>

Autorisierung bezieht sich auf den Prozess, der bestimmt, was ein Benutzer ausführen können, ist. Beispielsweise ist ein Administrator zum Erstellen einer Dokumentbibliothek Dokumente hinzufügen, Bearbeiten von Dokumenten und löschen Sie diese zulässig. Ein Benutzer ohne Administratorrechte arbeiten mit der Bibliothek ist nur autorisiert, auf die Dokumente zu lesen.

Autorisierung ist orthogonale und unabhängig von der Authentifizierung. Allerdings erfordert eine Autorisierung Authentifizierungsmechanismus. Authentifizierung versteht man das ermitteln, wer ein Benutzer ist. Authentifizierung kann eine oder mehrere Identitäten für den aktuellen Benutzer zu erstellen.

## <a name="authorization-types"></a>Autorisierungstypen

ASP.NET Core Autorisierung bietet ein einfaches, deklaratives [Rolle](xref:security/authorization/roles) und eine umfangreiche [richtlinienbasierten](xref:security/authorization/policies) Modell. Autorisierung ist in Anforderungen ausgedrückt und Handler auswerten ein Benutzer Ansprüche für Anforderungen. Imperative Überprüfungen können basieren auf einfachen oder Richtlinien für die Bewerten der Benutzeridentität und die Eigenschaften der Ressource, die der Benutzer zugreifen möchte.

## <a name="namespaces"></a>Namespaces

Autorisierungskomponenten, einschließlich der `AuthorizeAttribute` und `AllowAnonymousAttribute` Attribute, befinden sich in der `Microsoft.AspNetCore.Authorization` Namespace.

In der Dokumentation auf [einfache Autorisierung](xref:security/authorization/simple).
