---
title: Zweck Zeichenfolgen
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c96ed361-c382-4980-8933-800e740cfc38
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/consumer-apis/purpose-strings
ms.openlocfilehash: cc33bcfab4945e6d6f9ca7e61edeff4d1837661a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2017
---
# <a name="purpose-strings"></a>Zweck Zeichenfolgen

<a name=data-protection-consumer-apis-purposes></a>

Komponenten, die über IDataProtectionProvider nutzen müssen eine eindeutige übergeben *Zwecke* Parameter an die CreateProtector-Methode. Im Sinne *Parameter* ist für die Sicherheit der Datenschutzsystem integriert, da hiermit Isolation zwischen kryptografischen Consumern, selbst wenn der Stamm-Kryptografieschlüssel identisch sind.

Wenn ein Consumer einen Zweck angegeben ist, den Zweck Zeichenfolge zusammen mit der Stamm-Kryptografieschlüssel dient zum eindeutigen kryptografischen Unterschlüssel Consumer abgeleitet werden. Dadurch wird den Consumer aus anderen kryptografischen Consumer in der Anwendung isoliert: keine andere Komponente die Nutzlasten lesen kann, und eine andere Komponente Nutzlasten nicht gelesen. Diese Isolierung rendert auch unmöglich gesamte Kategorien von Angriff auf die Komponente.

![Beispiel für ein Diagramm Zweck](purpose-strings/_static/purposes.png)

In der Abbildung oben IDataProtector Instanzen A und B **kann nicht** Lesen des jeweils anderen Nutzlasten und nur ihre eigenen.

Die Zweck Zeichenfolge keine geheim sein. Dies sollte einfach insofern eindeutig sein, dass keine andere gut konzipierte Komponente immer den gleichen Zweck Zeichenfolge bereitstellt.

>[!TIP]
> Unter Verwendung des Namespace und Typ der Komponente, die Datenschutz-APIs nutzen, ist eine gute Faustregel, wie in der Praxis, die diese Informationen nicht in Konflikt steht.
>
>Eine Contoso autorisierten-Komponente, die für das trägertoken minting zuständig ist möglicherweise Contoso.Security.BearerToken als seine Zweck Zeichenfolge verwenden. Oder es möglicherweise - sogar - Contoso.Security.BearerToken.v1 als seine Zweck Zeichenfolge verwenden. Ermöglicht das Anfügen der Versionsnummer einer zukünftigen Version Contoso.Security.BearerToken.v2 als ihren Zweck zu verwenden, und die verschiedenen Versionen wäre vollständig voneinander isoliert, soweit Nutzlasten wechseln.

Da der Parameter Zwecke CreateProtector ein Array von Zeichenfolgen ist, konnte die oben genannten stattdessen als ["Contoso.Security.BearerToken", "v1"] wurde angegeben. Dies ermöglicht das Erstellen einer Hierarchie von Zwecken und mehrinstanzenfähigkeit Szenarios mit dem Data Protection System öffnet.

<a name=data-protection-contoso-purpose></a>

>[!WARNING]
> Nicht vertrauenswürdige Benutzereingaben als alleinige Quelle der Eingabe für die Zwecke Kette möglich Komponenten nicht.
>
>Betrachten Sie beispielsweise eine Komponente Contoso.Messaging.SecureMessage, die zum Speichern von sicheren Nachrichten zuständig ist. Wenn die sichere Messagingkomponente CreateProtector ([Username]) aufgerufen wurden, ein böswilliger Benutzer möglicherweise erstellen Sie ein Konto mit dem Benutzernamen "Contoso.Security.BearerToken" in beim Abrufen der Komponente aufrufen CreateProtector([" Contoso.Security.BearerToken"]), kann daher versehentlich verursacht die sichere Nachrichtensystem zur MinZ Nutzlasten, die als Authentifizierungstoken wahrgenommen werden.
>
>Eine bessere Zwecke-Kette für die Messagingkomponente wäre CreateProtector (["Contoso.Messaging.SecureMessage", "Benutzer: Benutzername"]), die richtigen Isolation bietet.

Die Isolierung durch und das Verhalten des IDataProtectionProvider, IDataProtector und Zwecke sind wie folgt:

* Für ein bestimmtes IDataProtectionProvider-Objekt erstellt die CreateProtector-Methode ein IDataProtector-Objekt gebunden eindeutig des IDataProtectionProvider-Objekts, das Erstellung und den Zweck-Parameter, den an die Methode übergeben wurde.

* Der Zweck-Parameter darf nicht null sein. (Wenn Zwecke als Bytearray angegeben ist, bedeutet dies, dass das Array nicht der Länge Null sein darf und alle Elemente des Arrays müssen ungleich Null sein.) Ein leere Zeichenfolge Zweck ist technisch zulässig, wird jedoch abgeraten.

* Argumente für zwei Zwecke sind gleichwertig, wenn sie die gleichen Zeichenfolgen (über einen ordinalen Vergleich) in der gleichen Reihenfolge enthalten. Ein Argument für die einzelnen Zweck ist gleichbedeutend mit der entsprechenden Einzelelement-Zwecke Array.

* Zwei IDataProtector-Objekte sind gleichwertig, wenn sie von gleichwertige IDataProtectionProvider Objekte mit den Parametern entspricht Zwecke erstellt werden.

* Für einen bestimmten IDataProtector-Objekt ein Aufruf von Unprotect(protectedData) der ursprünglichen UnprotectedData zurück, wenn und nur wenn ProtectedData: = Protect(unprotectedData) für ein gleichwertiges IDataProtector-Objekt.

> [!NOTE]
> Wir sind nicht die Groß-/Kleinschreibung in Erwägung ziehen, in denen eine Komponente absichtlich eine Zeichenfolge Zweck wählt was einen Konflikt mit einer anderen Komponente bezeichnet wird. Eine solche Komponente im Wesentlichen betrachtet werden böswillige, und dieses System ist nicht vorgesehen, Sicherheitsgarantien angeben, dass bösartiger Code innerhalb der Arbeitsprozess bereits ausgeführt wird.
