---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Teil 6: ASP.NET-Mitgliedschaft | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: c847db058bc03115210f12eeb0c3c76fecc8a31e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826449"
---
<a name="part-6-aspnet-membership"></a>Teil 6: ASP.NET-Mitgliedschaft
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 6 fügt ASP.NET-Mitgliedschaft hinzu.


## <a id="_Toc260221672"></a>  Verwenden von ASP.NET-Mitgliedschaft

![](tailspin-spyworks-part-6/_static/image1.png)

Klicken Sie auf Sicherheit

![](tailspin-spyworks-part-6/_static/image1.jpg)

Stellen Sie sicher, dass die Formularauthentifizierung verwendet werden.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Verwenden Sie den Link "Benutzer erstellen", um eine Reihe von Benutzern zu erstellen.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Abschließend finden Sie in Projektmappen-Explorer, und aktualisieren Sie die Ansicht.

![](tailspin-spyworks-part-6/_static/image2.png)

Beachten Sie, dass der ASPNETDB. MDF-Datei in Ordnung wurde erstellt. Diese Datei enthält die Tabellen, um die ASP.NET Hauptdienste wie Mitgliedschaften unterstützen.

Jetzt können wir die Implementierung des Auscheckvorgangs beginnen.

Beginnen Sie mit der Erstellung einer CheckOut.aspx-Seite.

Die Seite "CheckOut.aspx" sollte nur für Benutzer verfügbar sein, die in angemeldet sind, damit wir den Zugriff beschränken wird angemeldeter Benutzer und umleitungs-Benutzer, die auf die Anmeldeseite nicht angemeldet sind.

Zu diesem Zweck fügen wir die folgenden für den Konfigurationsabschnitt der Datei "Web.config".

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Die Vorlage für ASP.NET Web Forms-Anwendungen automatisch einen Abschnitt für die Authentifizierung zu Ihrer Datei "Web.config" hinzugefügt und die Standardanmeldeseite hergestellt.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

Wir müssen ändern, dass "Login.aspx" CodeBehind-Datei, eine anonyme Warenkorb zu migrieren, wenn der Benutzer anmeldet. Ändern Sie die Seite\_Load-Ereignis wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Fügen Sie dann einen "LoggedIn"-Ereignishandler wie folgt auf den Namen der Sitzung auf den neu angemeldeten Benutzer festgelegt, und ändern die temporäre Sitzungs-Id in den Einkaufswagen mit der der Benutzer durch Aufrufen der MigrateCart-Methode in unserer MyShoppingCart-Klasse hinzu. (Implementiert in der CS-Datei)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementieren Sie die MigrateCart()-Methode wie folgt aus.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In checkout.aspx verwenden EntityDataSource und einer GridView-Ansicht in unseren sehen Sie sich die Seite wir ähnlich wie wir in unserer Warenkorb-Seite haben.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Beachten Sie, dass das GridView-Steuerelement einen "den Ondatabound"-Ereignishandler mit dem Namen MyList gibt\_RowDataBound wir implementieren diesen Ereignishandler wie folgt.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Diese Methode hält, eine laufende Summe der Einkaufswagen Einkaufswagen, wie jede Zeile gebunden ist und die untersten Zeile der GridView aktualisiert.

Zu diesem Zeitpunkt haben wir eine "Kritik" Präsentation von der Reihenfolge platziert werden implementiert.

Lassen Sie uns eine leere einkaufswagenszenario behandeln, indem Sie einige Codezeilen hinzufügen, klicken Sie auf unserer Seite\_Load-Ereignis:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Klickt der Benutzer auf die Schaltfläche "Absenden" führt es in der Senden-Schaltfläche klicken Sie auf-Ereignishandler den folgenden Code.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

Der "fleischanteil" von dem bestellannahmeprozess besteht darin, in der Methode SubmitOrder() unserer MyShoppingCart-Klasse implementiert werden.

SubmitOrder führt Folgendes aus:

- Nehmen Sie alle Artikel im Warenkorb, und verwenden Sie, um einen neuen Datensatz für den Auftrag und die zugehörigen OrderDetails-Datensätze zu erstellen.
- Berechnet das Versanddatum.
- Deaktivieren Sie den Einkaufswagen.


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Im Rahmen dieser beispielanwendung werden wir ein Lieferdatum berechnen, indem Sie auf das aktuelle Datum zwei Tage hinzufügen.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

Die Anwendung jetzt ausführen gestattet, den Einkaufswagen Prozess von Anfang bis Ende zu testen.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-5.md)
> [Weiter](tailspin-spyworks-part-7.md)
