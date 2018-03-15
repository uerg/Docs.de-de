# <a name="cookie-sharing-sample-app"></a><span data-ttu-id="c2fed-101">Cookie-Freigabe-Beispiel-App</span><span class="sxs-lookup"><span data-stu-id="c2fed-101">Cookie Sharing Sample App</span></span>

<span data-ttu-id="c2fed-102">Im Beispiel werden alle drei apps, die Cookieauthentifizierung gemeinsam Cookie veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="c2fed-102">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

| <span data-ttu-id="c2fed-103">Projekt</span><span class="sxs-lookup"><span data-stu-id="c2fed-103">Project</span></span>                             | <span data-ttu-id="c2fed-104">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c2fed-104">Description</span></span> |
| ----------------------------------- | ----------- |
| <span data-ttu-id="c2fed-105">CookieAuth.Core</span><span class="sxs-lookup"><span data-stu-id="c2fed-105">CookieAuth.Core</span></span>                     | <span data-ttu-id="c2fed-106">ASP.NET 2.0-Razor-Seiten Core app ohne Verwendung von ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="c2fed-106">ASP.NET Core 2.0 Razor Pages app without using ASP.NET Core Identity</span></span> |
| <span data-ttu-id="c2fed-107">CookieAuthWithIdentity.Core</span><span class="sxs-lookup"><span data-stu-id="c2fed-107">CookieAuthWithIdentity.Core</span></span>         | <span data-ttu-id="c2fed-108">ASP.NET Core 2.0 MVC-Anwendung mit ASP.NET Core Identität</span><span class="sxs-lookup"><span data-stu-id="c2fed-108">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span> |
| <span data-ttu-id="c2fed-109">CookieAuthWithIdentity.NETFramework</span><span class="sxs-lookup"><span data-stu-id="c2fed-109">CookieAuthWithIdentity.NETFramework</span></span> | <span data-ttu-id="c2fed-110">ASP.NET Framework 4.6.1 MVC-Anwendung mit ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="c2fed-110">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span> |

<span data-ttu-id="c2fed-111">Anweisungen:</span><span class="sxs-lookup"><span data-stu-id="c2fed-111">Instructions:</span></span>

1. <span data-ttu-id="c2fed-112">Führen Sie die CookieAuth.Core-app.</span><span class="sxs-lookup"><span data-stu-id="c2fed-112">Run the CookieAuth.Core app.</span></span> <span data-ttu-id="c2fed-113">Registrieren eines Benutzers an.</span><span class="sxs-lookup"><span data-stu-id="c2fed-113">Register a user.</span></span> <span data-ttu-id="c2fed-114">Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist.</span><span class="sxs-lookup"><span data-stu-id="c2fed-114">The app authenticates the user when the user is registered.</span></span> <span data-ttu-id="c2fed-115">Melden Sie sich des Benutzers ab.</span><span class="sxs-lookup"><span data-stu-id="c2fed-115">Sign the user out.</span></span>
1. <span data-ttu-id="c2fed-116">Führen Sie in der gleichen Browsersitzung die CookieAuthWithIdentity.Core-app ein.</span><span class="sxs-lookup"><span data-stu-id="c2fed-116">In the same browser session, run the CookieAuthWithIdentity.Core app.</span></span> <span data-ttu-id="c2fed-117">Registrieren Sie denselben Benutzer wie die Core-App verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2fed-117">Register the same user as used with the Core app.</span></span> <span data-ttu-id="c2fed-118">Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist.</span><span class="sxs-lookup"><span data-stu-id="c2fed-118">The app authenticates the user when the user is registered.</span></span> <span data-ttu-id="c2fed-119">Melden Sie sich des Benutzers ab.</span><span class="sxs-lookup"><span data-stu-id="c2fed-119">Sign the user out.</span></span>
1. <span data-ttu-id="c2fed-120">Führen Sie in der gleichen Browsersitzung die CookieAuthWithIdentity.NETFramework-app ein.</span><span class="sxs-lookup"><span data-stu-id="c2fed-120">In the same browser session, run the CookieAuthWithIdentity.NETFramework app.</span></span> <span data-ttu-id="c2fed-121">Registrieren Sie den gleichen Benutzer als mit anderen apps verwendet.</span><span class="sxs-lookup"><span data-stu-id="c2fed-121">Register the same user as used with the other apps.</span></span> <span data-ttu-id="c2fed-122">Die app authentifiziert den Benutzer auf, wenn der Benutzer registriert ist.</span><span class="sxs-lookup"><span data-stu-id="c2fed-122">The app authenticates the user when the user is registered.</span></span> <span data-ttu-id="c2fed-123">Melden Sie sich des Benutzers ab.</span><span class="sxs-lookup"><span data-stu-id="c2fed-123">Sign the user out.</span></span>
1. <span data-ttu-id="c2fed-124">Melden Sie sich der Benutzer auf den drei Apps.</span><span class="sxs-lookup"><span data-stu-id="c2fed-124">Sign in the user to any of the three apps.</span></span> <span data-ttu-id="c2fed-125">Das Authentifizierungscookie ist von den apps gemeinsam genutzt.</span><span class="sxs-lookup"><span data-stu-id="c2fed-125">The authentication cookie is shared among the apps.</span></span> <span data-ttu-id="c2fed-126">Beachten Sie, dass der Benutzer automatisch in die anderen zwei apps angemeldet ist.</span><span class="sxs-lookup"><span data-stu-id="c2fed-126">Note that the user is automatically signed into the other two apps.</span></span>
1. <span data-ttu-id="c2fed-127">Melden Sie den Benutzer aus den Apps ein.</span><span class="sxs-lookup"><span data-stu-id="c2fed-127">Sign out the user from any of the apps.</span></span> <span data-ttu-id="c2fed-128">Beachten Sie, dass der Benutzer von anderen apps automatisch abgemeldet wird.</span><span class="sxs-lookup"><span data-stu-id="c2fed-128">Note that the user is automatically signed out of the other two apps.</span></span>

<span data-ttu-id="c2fed-129">In diesem Beispiel wird veranschaulicht, die in beschriebenen Funktionen der [Cookies zwischen apps freigeben](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) Thema.</span><span class="sxs-lookup"><span data-stu-id="c2fed-129">This sample demonstrates the features described in the [Sharing cookies among apps](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) topic.</span></span>