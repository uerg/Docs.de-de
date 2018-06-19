---
uid: mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
title: Überprüfen mit einer Dienstebene (c#) | Microsoft Docs
author: StephenWalther
description: Erfahren Sie, wie Ihre Validierungslogik aus Ihrer Controlleraktionen und in einer separaten Dienstebene zu verschieben. In diesem Lernprogramm Stephen Walther wird erläutert, wie Sie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4eabc535-b8a1-43f5-bb99-cfeb86db0fca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-a-service-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 06042ac197cc54da767a94a44c57eb09bb3db9fa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869035"
---
<a name="validating-with-a-service-layer-c"></a>Überprüfen mit einer Dienstebene (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Erfahren Sie, wie Ihre Validierungslogik aus Ihrer Controlleraktionen und in einer separaten Dienstebene zu verschieben. In diesem Lernprogramm wird Stephen Walther erläutert, wie Sie eine Spitze Trennung von Anliegen beibehalten können, indem die Dienstebene aus Ihrer Controller Layer isolieren.


Das Ziel dieses Lernprogramms ist eine Methode zum Ausführen der Überprüfung in einer ASP.NET MVC-Anwendung beschreiben. In diesem Lernprogramm erfahren Sie, wie Ihre Validierungslogik aus Ihrem Domänencontroller und in einer separaten Dienstebene zu verschieben.

## <a name="separating-concerns"></a>Trennung von Anliegen

Bei der Erstellung einer ASP.NET MVC-Anwendung sollten Sie Ihre Datenbanklogik nicht innerhalb der Controlleraktionen platzieren. Mischen Ihre Datenbank und Controller-Logik macht Ihre Anwendung schwieriger, die im Zeitverlauf. Es wird empfohlen, dass Sie alle Ihre Datenbanklogik in einer separaten Repository Ebene platzieren.

Auflisten von 1 enthält beispielsweise ein einfaches Repository mit dem Namen der ProductRepository. Die Produkt-Repository enthält alle Datenzugriffscode für die Anwendung. Die Liste enthält auch die IProductRepository-Schnittstelle, die das Produkt Repository implementiert.

**1 – Models\ProductRepository.cs auflisten**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample1.cs)]

Der Controller im Codebeispiel 2 verwendet die Repository-Ebene für die Aktionen hinsichtlich der Index() und der Create()-. Beachten Sie, dass dieser Domänencontroller keine Datenbanklogik enthält. Erstellen eine Repository-Ebene können Sie eine klare Trennung von Anliegen zu verwalten. Controller für datenflusskontrolllogik von Anwendung verantwortlich sind, und das Repository ist für Datenzugriffslogik zuständig.

**Auflisten von 2 – Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample2.cs)]

## <a name="creating-a-service-layer"></a>Erstellen einer Dienstebene

Deshalb datenflusskontrolllogik von Anwendung gehört, in einem Controller und darauf von Datenzugriffslogik gehört, in einem Repository. In diesem Fall, in dem nehmen Sie Ihre Validierungslogik? Eine Möglichkeit besteht darin, platzieren Sie die Validierungslogik in einem *Dienstschicht*.

Eine Dienstebene ist eine zusätzliche Ebene in einer ASP.NET MVC-Anwendung, die Kommunikation zwischen einem Controller und Repository-Ebene vermittelt. Die Dienstebene enthält Geschäftslogik. Insbesondere enthält die Validierungslogik.

Beispielsweise verfügt die Product-Dienstebene auflisten 3 eine CreateProduct()-Methode. Die CreateProduct()-Methode ruft die ValidateProduct()-Methode, um ein neues Produkt zu überprüfen, bevor das Produkt mit der Produkt-Repository übergeben.

**3 – Models\ProductService.cs auflisten**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample3.cs)]

Der Product-Controller wurde aktualisiert, 4 auflisten, die Dienstebene anstelle der Repository-Ebene verwendet. Die Controller-Schicht kommuniziert mit der Dienstebene. Die Dienstebene kommuniziert mit der Repository-Ebene. Jede Ebene weist eine separaten Verantwortung.

**4 – Controllers\ProductController.cs auflisten**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample4.cs)]

Beachten Sie, dass der Product-Dienst in der Product-Controller-Konstruktor erstellt wird. Wenn der Product-Dienst erstellt wird, wird das Modellzustandswörterbuch an den Dienst übergeben. Der Product-Dienst verwendet den Modellzustand Validierung Fehlermeldungen an den Controller zurückgesendet.

## <a name="decoupling-the-service-layer"></a>Trennen die Dienstebene

Wir konnten nicht den Controller- und Dienst Ebenen in einen Bezug zu isolieren. Der Controller und die Dienst-Ebenen, kommunizieren über Modellstatus. Das heißt, hat die Dienstebene eine Abhängigkeit auf eine bestimmte Funktion von ASP.NET MVC-Framework.

Wir möchten die Dienstebene aus unserem Controller Ebene so weit wie möglich zu isolieren. Theoretisch sollten wir die Dienstebene mit jedem Typ der Anwendung und nicht nur eine ASP.NET MVC-Anwendung verwenden können. In der Zukunft, wir möchten eine WPF-Front-End für die Anwendung zu erstellen. Es sollte eine Möglichkeit zum Entfernen der Abhängigkeit von ASP.NET MVC Modellzustand aus unserem Dienstschicht gefunden.

Auflisten von 5 wurde die Dienstebene aktualisiert, sodass der Modellzustand nicht mehr verwendet. Stattdessen wird jede Klasse, die IValidationDictionary-Schnittstelle implementiert.

**Auflisten von 5 – Models\ProductService.cs (entkoppelt)**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample5.cs)]

Auflisten von 6 ist die IValidationDictionary-Schnittstelle definiert. Diese einfache Schnittstelle verfügt über eine einzelne Methode und eine einzelne Eigenschaft.

**6 – Models\IValidationDictionary.cs auflisten**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample6.cs)]

Die Klasse in 7 auflisten, mit dem Namen der Klasse ModelStateWrapper implementiert die IValidationDictionary-Schnittstelle. Sie können die ModelStateWrapper-Klasse instanziieren, indem eine Modellzustandswörterbuch an den Konstruktor übergeben.

**Auflisten von 7 - Models\ModelStateWrapper.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample7.cs)]

Schließlich verwendet der aktualisierte Controller in 8 Auflisten der ModelStateWrapper beim Erstellen von der Dienstebene in seinem Konstruktor.

**Auflisten von 8 - Controllers\ProductController.cs**

[!code-csharp[Main](validating-with-a-service-layer-cs/samples/sample8.cs)]

Mithilfe der IValidationDictionary können Schnittstelle und die Klasse ModelStateWrapper wir unsere Dienstebene aus unserem Controller Ebene vollständig zu isolieren. Die Dienstebene ist nicht mehr Modellstatus abhängig. Sie können eine beliebige Klasse übergeben, die um die Dienstebene der IValidationDictionary-Schnittstelle implementiert. Beispielsweise kann eine WPF-Anwendung die IValidationDictionary-Schnittstelle mit einer einfachen Auflistungsklasse implementieren.

## <a name="summary"></a>Zusammenfassung

Das Ziel dieses Lernprogramms wurde ein Verfahren zur Ausführung der Validierung in ASP.NET MVC-Anwendung zu besprechen. In diesem Lernprogramm haben Sie gelernt, wie Sie alle Ihre Validierungslogik aus Ihrem Domänencontroller und in einer separaten Dienstebene zu verschieben. Außerdem haben Sie gelernt, die Dienstebene aus Ihrer Controller Layer zu isolieren, indem Sie eine ModelStateWrapper-Klasse erstellen.

> [!div class="step-by-step"]
> [Zurück](validating-with-the-idataerrorinfo-interface-cs.md)
> [Weiter](validation-with-the-data-annotation-validators-cs.md)
