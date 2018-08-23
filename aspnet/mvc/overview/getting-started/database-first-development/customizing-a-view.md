---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Database First mit ASP.NET MVC: Anpassen einer Ansicht | Microsoft-Dokumentation'
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: riande
ms.date: 10/01/2014
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: ce450af93459f2a69557b3fe0d1ead813ae99986
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833627"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="927ee-104">EF Database First mit ASP.NET MVC: Anpassen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="927ee-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="927ee-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="927ee-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="927ee-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="927ee-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="927ee-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="927ee-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="927ee-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="927ee-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="927ee-109">Dieser Teil der Serie konzentriert sich auf die automatisch generierten Ansichten zur Verbesserung der Präsentation zu ändern.</span><span class="sxs-lookup"><span data-stu-id="927ee-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="927ee-110">Registrierte Kurse für Schüler und Studenten Details hinzufügen</span><span class="sxs-lookup"><span data-stu-id="927ee-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="927ee-111">Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung bietet jedoch nicht unbedingt alle Funktionen, die Sie in Ihrer Anwendung benötigen.</span><span class="sxs-lookup"><span data-stu-id="927ee-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="927ee-112">Sie können den Code, um die bestimmten Anforderungen Ihrer Anwendung anpassen.</span><span class="sxs-lookup"><span data-stu-id="927ee-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="927ee-113">Die Anwendung wird derzeit nicht die registrierten Kurse für den ausgewählten Studenten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="927ee-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="927ee-114">In diesem Abschnitt fügen Sie die registrierten Kurse für jeden Kursteilnehmer auf die **Details** Ansicht für den Studenten.</span><span class="sxs-lookup"><span data-stu-id="927ee-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="927ee-115">Öffnen **Students/Details.cshtml**, und klicken Sie unterhalb der letzten &lt;/DL&gt; Registerkarte aber vor dem schließenden &lt;/div&gt; markieren, fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="927ee-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="927ee-116">Dieser Code erstellt eine Tabelle, die eine Zeile für jeden Datensatz in der Enrollment-Tabelle für den ausgewählten Studenten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="927ee-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="927ee-117">Die **Anzeige** -Methode rendert die HTML für das Objekt (ModelItem), die den Ausdruck darstellt.</span><span class="sxs-lookup"><span data-stu-id="927ee-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="927ee-118">Verwenden Sie die Anzeige-Methode (anstatt einfach den Wert der Eigenschaft im Code) um sicherzustellen, dass der Wert ist ordnungsgemäß basierend auf den Typ und die Vorlage für diesen Typ formatiert.</span><span class="sxs-lookup"><span data-stu-id="927ee-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="927ee-119">In diesem Beispiel jeder Ausdruck gibt eine einzelne Eigenschaft des aktuellen Datensatzes in der Schleife und die Werte sind primitive Typen, die als Text gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="927ee-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="927ee-120">Navigieren Sie erneut an die Schüler/Studenten/Index-Ansicht aus, und wählen Sie **Details** für eines der Schüler/Studenten.</span><span class="sxs-lookup"><span data-stu-id="927ee-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="927ee-121">Sie sehen, dass die registrierten Kurse in der Ansicht hinzugefügt wurden.</span><span class="sxs-lookup"><span data-stu-id="927ee-121">You will see the enrolled courses have been included in the view.</span></span>

!["Student" mit Registrierung](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="927ee-123">[Zurück](changing-the-database.md)
> [Weiter](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="927ee-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
