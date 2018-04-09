---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: MVC Database First-Site in Azure veröffentlichen | Microsoft Docs
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 839bbceba6f0e098303facd40dbb1496bd449ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a>Veröffentlichen Sie MVC Database First-Site in Azure.
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert. Der generierte Code entspricht den Spalten in der Datenbanktabelle.
> 
> In diesem Teil der Reihe konzentriert sich auf die Web-app und die Datenbank in Azure veröffentlichen. Erfahren Sie in diesem Thema erfahren Sie mehr über das Veröffentlichen einer Web-app und einer Datenbank, aber tatsächlich auf die Schritte, die Sie am Anfang des Lernprogramms beginnen müssen. Finden Sie unter [Einstieg](setting-up-database.md).


## <a name="deploy-your-web-app-on-azure"></a>Ihre Web-app in Azure bereitstellen

Sie benötigen ein Azure-Konto dieses Lernprogramms:

- Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -erhalten Sie Gutschriften können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
- Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.

Um Ihre Web-app zu veröffentlichen, mit der rechten Maustaste des Projekts, und wählen Sie **veröffentlichen**.

![Website veröffentlichen](publish-to-azure/_static/image1.png)

Wählen Sie Microsoft Azure-Websites.

![Auswählen von Azure](publish-to-azure/_static/image2.png)

Wenn Sie nicht bei Azure angemeldet sind, geben Sie Ihre Azure-Konto-Anmeldeinformationen. Wählen Sie anschließend neu, um eine neue Web-app zu erstellen.

![neue Website](publish-to-azure/_static/image3.png)

Erstellen Sie einen eindeutigen Namen für Ihre Web-app. Sie werden wissen, dass der Name eindeutig ist, wenn Sie rechts neben dem Namensfeld ein grünes Häkchen angezeigt. Wählen Sie eine Region für Ihre Web-app. Wählen Sie **Erstellen neuer Server** für die Datenbank, und geben Sie einen Benutzernamen und ein Kennwort für diesen neuen Datenbankserver. Wenn Sie fertig sind, klicken Sie auf **erstellen**.

![Website erstellen](publish-to-azure/_static/image4.png)

Die Verbindungswerte werden jetzt alle festgelegt. Sie können diese Werte unverändert lassen.

![connection](publish-to-azure/_static/image5.png)

Klicken Sie auf **Weiter**.

Einstellungen, beachten Sie, dass zwei Verbindungen Datenbank angegebene - ApplicationDBContext und ContosoUniversityDataEntities. ApplicationDBContext ist die Verbindung für Benutzertabellen-Konto. Diese Werte werden nur die Verbindungszeichenfolgen für die Datenbanken anzeigen. Es bedeutet nicht, dass diese Datenbanken veröffentlicht werden, wenn Sie Ihre Website veröffentlichen. Veröffentlichen Sie Ihr Datenbankprojekt Sie nach dem Veröffentlichen der Web-app.

![](publish-to-azure/_static/image6.png)

Die Auslassungspunkte (...) neben der Verbindung mit der Datenbank werden Sie die Details der Verbindungszeichenfolge. Klicken Sie auf die Auslassungspunkte neben ContosoUniversityDataEntities.

![](publish-to-azure/_static/image7.png)

Notieren Sie den Namen des Datenbankservers und der Datenbank. Den Namen des Servers wird nach dem Zufallsprinzip generiert. Der Datenbankname ist einfach der Name Ihres Standorts mit  **\_Db** angefügt. Sie benötigen beide Namen später, wenn Sie Ihre Datenbank veröffentlichen.

Klicken Sie auf **OK** Verbindungsfenster Zeichenfolge Datenbank schließen.

Im Fenster "Web veröffentlichen", klicken Sie auf **Weiter** der Vorschau anzeigen.

![](publish-to-azure/_static/image8.png)

Sie können die Vorschau zum Anzeigen einer Liste der Dateien veröffentlichen starten klicken. Da dies das erste Mal Sie dieser Website veröffentlicht haben ist, ist die Liste alle relevanten Dateien im Projekt aus.

Klicken Sie auf **Veröffentlichen**.

Im Ausgabebereich wird das Ergebnis der Publikation angezeigt.

![](publish-to-azure/_static/image9.png)

Der Standort wird nach der Veröffentlichung Immedialely in einem Webbrowser gestartet. Ihre Website bereitgestellt wurde, und Sie können einen neuen Benutzer mit dem Standort registrieren; Allerdings wurden die Tabellen im Projekt ContosoUniversityData noch nicht veröffentlicht wurde. Wenn Sie auf die Liste der Schüler Link klicken, wird eine Fehlermeldung.

## <a name="publish-database-to-sql-azure"></a>Veröffentlichen Sie die Datenbank in SQL Azure

Vor dem Veröffentlichen der Datenbank, müssen Sie sicherstellen, dass es sich bei Ihrem lokale Computer eine Verbindung mit dem Datenbankserver herstellen kann. Die Firewall für den Datenbankserver schränkt die Computer mit der Datenbank herstellen können. Sie müssen die IP-Adresse des Computers zu den zulässigen IP-Adressen für die Firewall hinzufügen.

Melden Sie sich Ihr Azure-Konto über das Azure-Portal.

Wählen Sie die neue Datenbank, und wählen Sie **verwalten**.

![Verwalten](publish-to-azure/_static/image10.png)

Sie müssen Ihr Datenbankserver zum Zulassen von Verbindungen von Ihrem Computer konfigurieren. Wenn Sie auf "verwalten" auswählen, werden Sie gefragt, um die aktuelle IP-Adresse hinzuzufügen, auf dem Datenbankserver erlaubt. Wählen Sie Ja.

![IP-Adresse hinzufügen](publish-to-azure/_static/image11.png)

Es besteht die Möglichkeit, die die IP-Adresse, die Sie im vorherigen Schritt hinzugefügt haben nicht die einzige IP-Adresse ist, die Sie für Verbindungen zu konfigurieren müssen. Sie können versuchen, bei der Anmeldung auf die Datenbank, um festzustellen, ob die Verbindungen ordnungsgemäß eingerichtet wurden. Geben Sie den Benutzer und das Kennwort an, das Sie zuvor erstellt haben.

![Anmeldung](publish-to-azure/_static/image12.png)

Wenn Sie eine Fehlermeldung erhalten, müssen Sie eine andere IP-Adresse hinzufügen. Klicken Sie auf die Fehlermeldung, um weitere Details zum Fehler anzuzeigen. In den Details sehen Sie die IP-Adresse, die Sie hinzufügen müssen. Beachten Sie diese IP-Adresse ein.

![nicht zulässig](publish-to-azure/_static/image13.png)

Schließen Sie dieses Fenster für die Anmeldung, und kehren Sie zum Azure-Portal zurück.

Navigieren Sie zum Dashboard für Ihre Datenbank. Klicken Sie auf **zulässige IP-Adressen verwalten**.

![IP-Adressen verwalten](publish-to-azure/_static/image14.png)

Jetzt müssen Sie die IP-Adresse aus der Fehlermeldung hinzufügen. Ändern Sie den Bereich der zulässigen IP-Adressen von der Fehlermeldung eingeschlossen oder fügen Sie dieser IP-Adresse als ein separater Eintrag hinzu.

![neue Adresse hinzufügen](publish-to-azure/_static/image15.png)

Speichern Sie die Änderung zu zulässigen IP-Adressen an.

Klicken Sie auf verwalten, und versuchen Sie es erneut mit der Datenbank anmelden. Sie müssen möglicherweise einige Minuten, bevor die zulässigen IP-Adressen für die Firewall ordnungsgemäß konfiguriert sind. Wenn Sie in der Datenbank anmelden können, haben Sie die Verbindung mit der Datenbank eingerichtet.

Sie können dieses Fenster geöffnet lassen, da Sie das Ergebnis der Bereitstellung der Datenbank in Kürze wird geprüft.

Dem Datenbankprojekt zurück. Mit der rechten Maustaste des Projekts, und wählen Sie **veröffentlichen**.

![Veröffentlichen der Datenbank](publish-to-azure/_static/image16.png)

Wählen Sie im Fenster Datenbank veröffentlichen **bearbeiten**.

![bearbeiten](publish-to-azure/_static/image17.png)

Geben Sie den Namen des Datenbankservers und die Anmeldeinformationen für die Authentifizierung des Servers ein. Wählen Sie nach der Bereitstellung der Anmeldeinformationen, die Sie aus der Liste der verfügbaren Datenbanken erstellte Datenbank. Standardmäßig legt Visual Studio den Namen des Felds Datenbank auf den Namen des Projekts nicht identisch mit der Datenbank, die möglicherweise, den Sie erstellt haben.

![](publish-to-azure/_static/image18.png)

Klicken Sie auf OK.

Sie möchten wahrscheinlich dieses Profil zu speichern, damit Sie Updates in der Zukunft veröffentlichen können, ohne alle Verbindungsinformationen erneut eingeben zu müssen. Klicken Sie auf **Profil erstellen**.

![Profil speichern](publish-to-azure/_static/image19.png)

Es wird eine Datei erstellt, in Ihrem Projekt mit dem Namen **ContosoUniversityData.publish.xml**. Laden Sie das nächste Mal mit das Sie die Datenbank in Azure veröffentlichen möchten einfach dieses Profil dürfen.

Klicken Sie nun auf **veröffentlichen** zum Erstellen der Datenbank in Azure.

![publish](publish-to-azure/_static/image20.png)

Nach der Ausführung für eine Weile, werden die publishing Ergebnisse angezeigt.

![results](publish-to-azure/_static/image21.png)

Jetzt können Sie zurück in das Verwaltungsportal für Ihre Datenbank wechseln. Aktualisieren Sie die Entwurfsansicht, und beachten Sie, dass die 3 Tabellen mit vorab ausgefüllten Daten bereitgestellt wurden.

![neue Tabellen](publish-to-azure/_static/image22.png)

Jetzt sind Sie bereit sind, die Web-app testen, die in Azure bereitgestellt wird. Navigieren Sie zu der Web-app in Azure (z. B. http://contosositeexample.azurewebsites.net/). Klicken Sie auf den Link, um die Liste der Schüler und die Indexansicht für Studenten sollte angezeigt werden.

![Sicht](publish-to-azure/_static/image23.png)

Es kann vorkommen, benötigen die Datenbank und die Verbindung eine gewisse Zeit ordnungsgemäß konfiguriert sein. Wenn Sie eine Fehlermeldung erhalten, warten Sie einige Minuten, und wiederholen Sie dann erneut.

## <a name="conclusion"></a>Schlussbemerkung

Diese Reihe bereitgestellt, ein einfaches Beispiel zum Generieren von Code aus einer vorhandenen Datenbank, die Benutzern das Bearbeiten, aktualisieren, erstellen und Löschen von Daten ermöglicht. Sie verwendet ASP.NET MVC 5, Entity Framework und ASP.NET Gerüstbau, um das Projekt zu erstellen.

Einführende beispielsweise der Code First-Entwicklung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md).

Ein komplexeres Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Beachten Sie, dass der DbContext-API, mit denen Sie für die Arbeit mit Daten in Database First identisch mit der API Sie zum Arbeiten mit Daten in Code First verwenden. Selbst wenn Sie Database First verwenden möchten, können Sie erfahren, wie komplexere Szenarien, z. B. Lesen und Aktualisieren von verknüpften Daten zu behandeln Parallelitätskonflikte usw. aus einem Code First-Lernprogramm. Der einzige Unterschied besteht in der Datenbank, der Context-Klasse und der Entitätsklassen wie erstellt werden.

> [!div class="step-by-step"]
> [Vorherige](enhancing-data-validation.md)
