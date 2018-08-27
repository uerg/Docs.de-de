---
title: DevOps mit ASP.NET Core und Azure | Nächste Schritte
author: CamSoper
description: Ein Leitfaden, der End-to-End-Anleitungen zum Erstellen einer DevOps-Pipeline für eine in Azure gehostete ASP.NET Core-App bereitstellt.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/next-steps
ms.openlocfilehash: 7a0f1b1b56a33b1870e0657d8ba465adb84f5a02
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "42909325"
---
# <a name="next-steps"></a>Nächste Schritte

In diesem Handbuch haben Sie eine DevOps-Pipeline für eine ASP.NET Core-Beispiel-app erstellt. Herzlichen Glückwunsch! Wir hoffen, Sie fanden lernen, wie Sie ASP.NET Core-Web-apps in Azure App Service veröffentlichen, und automatisieren die kontinuierliche Integration von Änderungen.

Nach der Bereitstellung auf einem Webserver, und klicken Sie mit der DevOps hat Azure eine Vielzahl von Platform-as-a-Service (PaaS)-Dienste, die für ASP.NET Core-Entwickler nützlich. Dieser Abschnitt enthält eine kurze Übersicht über einige der am häufigsten verwendeten Dienste.

## <a name="storage-and-databases"></a>Speicher und Datenbanken

[Redis-Cache](https://docs.microsoft.com/azure/redis-cache/) hohem Durchsatz und geringer Latenz datenzwischenspeicherung als Dienst verfügbar ist. Es kann für das Zwischenspeichern der Seitenausgabe datenbankanforderungen zu reduzieren und Bereitstellen von ASP.NET Core-Sitzungszustand über mehrere Instanzen einer App verwendet werden.

[Azure-Speicher](https://docs.microsoft.com/azure/storage/) massiv skalierbarer cloudspeicher von Azure ist. Entwickler können nutzen [Queue Storage](https://docs.microsoft.com/azure/storage/queues/storage-queues-introduction) zuverlässiges Message Queuing, und [Table Storage](https://docs.microsoft.com/azure/storage/tables/table-storage-overview) ist ein NoSQL-Schlüssel-Wert-Datenspeicher für die schnelle Entwicklung mithilfe von massive, teilweise strukturierten Datasets entwickelt.

[Azure SQL-Datenbank](https://docs.microsoft.com/azure/sql-database/) bietet relationale Datenbankfunktionalität als mit der Microsoft SQL Server-Engine-Dienst vertraut.

[COSMOS DB](https://docs.microsoft.com/azure/cosmos-db/) Global verteilter NoSQL-Datenbankdienst. Mehrere APIs sind verfügbar, einschließlich SQL-API (früher als DocumentDB bezeichnet), Cassandra und MongoDB.

## <a name="identity"></a>Identität

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) und [Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/) werden beide Identitätsdienste. Azure Active Directory ist für Enterprise-Szenarios konzipiert und ermöglicht der Azure AD B2B Zusammenarbeit (Business-to-Business), während Azure Active Directory B2C gewünschten Unternehmen-zu-Kunde-Szenarien, einschließlich der Anmeldung für soziale Netzwerke ist.

## <a name="mobile"></a>Mobil

[Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/) ist eine plattformübergreifende und skalierbare pushbenachrichtigungs-Engine, Millionen von Nachrichten schnell an apps auf verschiedenen Arten von Geräten zu senden.

## <a name="web-infrastructure"></a>Webinfrastruktur

[Azure Container Service](https://docs.microsoft.com/azure/aks/) verwaltet Ihre gehostete Kubernetes-Umgebung, sodass schnelle und einfache Bereitstellung und Verwaltung von containeranwendungen containerorchestrierung.

[Azure Search](https://docs.microsoft.com/azure/search/) wird verwendet, um eine Lösung für Unternehmenssuche über private, heterogene Inhalte erstellen.

[Service Fabric](https://docs.microsoft.com/azure/service-fabric/) ist eine Plattform für verteilte Systeme, die ganz einfach packen, bereitstellen und verwalten skalierbarer und zuverlässiger Microservices und Containern.
