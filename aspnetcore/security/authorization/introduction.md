---
title: "Einführung in die Autorisierung"
author: rick-anderson
description: "Dieses Dokument bietet eine grundlegende Erklärung der Autorisierung und erläutert, wie die Autorisierung auf ASP.NET Core bezieht."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/introduction
ms.openlocfilehash: 3fef6d38672af8871c04b65834789a39a7df8487
ms.sourcegitcommit: d43c84c4c80527c85e49d53691b293669557a79d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/20/2018
---
# <a name="introduction"></a>Einführung

<a name="security-authorization-introduction"></a>

Autorisierung bezieht sich auf den Prozess, der bestimmt, was ein Benutzer ausführen können, ist. Beispielsweise ist ein Administrator zum Erstellen einer Dokumentbibliothek Dokumente hinzufügen, Bearbeiten von Dokumenten und löschen Sie diese zulässig. Ein Benutzer ohne Administratorrechte arbeiten mit der Bibliothek ist nur autorisiert, auf die Dokumente zu lesen.

Autorisierung ist orthogonale und unabhängig von der Authentifizierung, dabei der wird ermitteln, wer ein Benutzer ist. Authentifizierung kann eine oder mehrere Identitäten für den aktuellen Benutzer zu erstellen.

## <a name="authorization-types"></a>Autorisierungstypen

ASP.NET Core Autorisierung bietet ein einfaches, deklaratives [Rolle](roles.md) und eine umfangreiche [richtlinienbasierten](policies.md) Modell. Autorisierung ist in Anforderungen ausgedrückt und Handler auswerten ein Benutzer Ansprüche für Anforderungen. Imperative Überprüfungen können basieren auf einfachen oder Richtlinien für die Bewerten der Benutzeridentität und die Eigenschaften der Ressource, die der Benutzer zugreifen möchte.

## <a name="namespaces"></a>Namespaces

Autorisierungskomponenten, einschließlich der `AuthorizeAttribute` und `AllowAnonymousAttribute` Attribute, befinden sich in der `Microsoft.AspNetCore.Authorization` Namespace.

In der Dokumentation auf [einfache Autorisierung](xref:security/authorization/simple).
