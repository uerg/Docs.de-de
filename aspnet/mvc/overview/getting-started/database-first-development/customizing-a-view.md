---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'EF Datenbank zuerst mit ASP.NET MVC: Anpassen einer Ansicht | Microsoft Docs'
author: tfitzmac
description: MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867657"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="f83bd-104">EF Datenbank zuerst mit ASP.NET MVC: Anpassen einer Ansicht</span><span class="sxs-lookup"><span data-stu-id="f83bd-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="f83bd-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f83bd-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="f83bd-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="f83bd-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="f83bd-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f83bd-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="f83bd-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="f83bd-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="f83bd-109">In diesem Teil der Reihe konzentriert sich auf das Ändern der Ansichten automatisch generiert, um die Lesbarkeit zu verbessern.</span><span class="sxs-lookup"><span data-stu-id="f83bd-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="f83bd-110">Registrierte Kurse zu Student Details hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f83bd-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="f83bd-111">Der generierte Code bietet einen guten Ausgangspunkt für Ihre Anwendung jedoch nicht notwendigerweise bietet alle Funktionen, die Sie in Ihrer Anwendung benötigen.</span><span class="sxs-lookup"><span data-stu-id="f83bd-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="f83bd-112">Sie können den Code, um die bestimmte Anforderungen der Anwendung anpassen.</span><span class="sxs-lookup"><span data-stu-id="f83bd-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="f83bd-113">Die Anwendung wird derzeit nicht registrierte Kurse für den ausgewählten Schüler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f83bd-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="f83bd-114">In diesem Abschnitt fügen Sie die registrierten Kurse für Studenten, die **Details** Ansicht für die Studenten.</span><span class="sxs-lookup"><span data-stu-id="f83bd-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="f83bd-115">Open **Students/Details.cshtml**, und unterhalb der letzten &lt;/DL&gt; Registerkarte, aber vor dem schließenden &lt;/div&gt; kennzeichnen, fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f83bd-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="f83bd-116">Dieser Code erstellt eine Tabelle, die eine Zeile für jeden Datensatz in der Enrollment-Tabelle für die ausgewählten Studenten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f83bd-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="f83bd-117">Die **Anzeige** Methode rendert die HTML für das Objekt (ModelItem), die den Ausdruck darstellt.</span><span class="sxs-lookup"><span data-stu-id="f83bd-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="f83bd-118">Verwenden Sie die Anzeige-Methode (statt einfach einbetten den Eigenschaftswert im Code) um sicherzustellen, dass der Wert wird formatiert ordnungsgemäß basierend auf den Typ und die Vorlage für diesen Typ.</span><span class="sxs-lookup"><span data-stu-id="f83bd-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="f83bd-119">In diesem Beispiel wird jeder Ausdruck gibt eine einzelne Eigenschaft aus dem aktuellen Datensatz in der Schleife und die Werte sind primitive Typen, die als Text gerendert werden.</span><span class="sxs-lookup"><span data-stu-id="f83bd-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="f83bd-120">Navigieren Sie zu der Studenten/Indexansicht erneut aus, und wählen Sie **Details** für eines der Studenten.</span><span class="sxs-lookup"><span data-stu-id="f83bd-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="f83bd-121">Sie sehen, dass die registrierten Kurse in der Ansicht enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="f83bd-121">You will see the enrolled courses have been included in the view.</span></span>

![Studenten, die bei der Registrierung](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="f83bd-123">[Zurück](changing-the-database.md)
> [Weiter](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="f83bd-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
