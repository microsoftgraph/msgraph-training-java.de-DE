---
ms.openlocfilehash: 50206b77504979c1cf67b4d0daa0b6f6576bde32
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661053"
---
# <a name="how-to-run-the-completed-project"></a><span data-ttu-id="97e3a-101">Vorgehensweise Ausführen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="97e3a-101">How to run the completed project</span></span>

## <a name="prerequisites"></a><span data-ttu-id="97e3a-102">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="97e3a-102">Prerequisites</span></span>

<span data-ttu-id="97e3a-103">Um das abgeschlossene Projekt in diesem Ordner auszuführen, benötigen Sie Folgendes:</span><span class="sxs-lookup"><span data-stu-id="97e3a-103">To run the completed project in this folder, you need the following:</span></span>

- <span data-ttu-id="97e3a-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Gradle](https://gradle.org/) auf Ihrem Entwicklungscomputer installiert.</span><span class="sxs-lookup"><span data-stu-id="97e3a-104">[Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="97e3a-105">Wenn Sie nicht über das JDK oder Gradle verfügen, besuchen Sie die vorherigen Links für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="97e3a-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span> <span data-ttu-id="97e3a-106">(**Hinweis:** dieses Lernprogramm wurde mit der openjdk-Version 14.0.0.36 und Gradle 6.7.1 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="97e3a-106">(**Note:** This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="97e3a-107">Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, aber das wurde nicht getestet.)</span><span class="sxs-lookup"><span data-stu-id="97e3a-107">The steps in this guide may work with other versions, but that has not been tested.)</span></span>
- <span data-ttu-id="97e3a-108">Ein Microsoft-Arbeits-oder Schulkonto.</span><span class="sxs-lookup"><span data-stu-id="97e3a-108">A Microsoft work or school account.</span></span>

<span data-ttu-id="97e3a-109">Wenn Sie kein Microsoft-Konto haben, können Sie [sich für das Office 365 Entwicklerprogramm registrieren](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="97e3a-109">If you don't have a Microsoft account, you can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

## <a name="register-a-web-application-with-the-azure-active-directory-admin-center"></a><span data-ttu-id="97e3a-110">Registrieren einer Webanwendung im Azure-Active Directory Admin Center</span><span class="sxs-lookup"><span data-stu-id="97e3a-110">Register a web application with the Azure Active Directory admin center</span></span>

1. <span data-ttu-id="97e3a-111">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="97e3a-111">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com).</span></span> <span data-ttu-id="97e3a-112">Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="97e3a-112">Login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="97e3a-113">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-113">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="97e3a-114">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="97e3a-114">A screenshot of the App registrations</span></span> ](/tutorial/images/aad-portal-app-registrations.png)

1. <span data-ttu-id="97e3a-115">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-115">Select **New registration**.</span></span> <span data-ttu-id="97e3a-116">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="97e3a-116">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="97e3a-117">Legen Sie **Name** auf `Java Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="97e3a-117">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="97e3a-118">Legen Sie **unterstützte Kontotypen** auf **Konten in einem beliebigen Organisations Verzeichnis** fest.</span><span class="sxs-lookup"><span data-stu-id="97e3a-118">Set **Supported account types** to **Accounts in any organizational directory**.</span></span>
    - <span data-ttu-id="97e3a-119">Lassen Sie **URI umleiten** leer.</span><span class="sxs-lookup"><span data-stu-id="97e3a-119">Leave **Redirect URI** empty.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](/tutorial/images/aad-register-an-app.png)

1. <span data-ttu-id="97e3a-121">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-121">Choose **Register**.</span></span> <span data-ttu-id="97e3a-122">Kopieren Sie auf der Seite **Java Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, die Sie im nächsten Schritt benötigen.</span><span class="sxs-lookup"><span data-stu-id="97e3a-122">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](/tutorial/images/aad-application-id.png)

1. <span data-ttu-id="97e3a-124">Klicken Sie auf den Link **Umleitungs-URI hinzufügen** .</span><span class="sxs-lookup"><span data-stu-id="97e3a-124">Select the **Add a Redirect URI** link.</span></span> <span data-ttu-id="97e3a-125">Suchen Sie auf der Seite " **Umleitungs-URIs** " den Abschnitt **vorgeschlagene Umleitungs-URIs für öffentliche Clients (Mobile, Desktop)** .</span><span class="sxs-lookup"><span data-stu-id="97e3a-125">On the **Redirect URIs** page, locate the **Suggested Redirect URIs for public clients (mobile, desktop)** section.</span></span> <span data-ttu-id="97e3a-126">Wählen Sie den `https://login.microsoftonline.com/common/oauth2/nativeclient` URI aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-126">Select the `https://login.microsoftonline.com/common/oauth2/nativeclient` URI.</span></span>

    ![Screenshot der Seite "Umleitungs-URIs"](/tutorial/images/aad-redirect-uris.png)

1. <span data-ttu-id="97e3a-128">Suchen Sie den **standardmäßigen Clienttyp** Abschnitt, und ändern Sie die Option **Anwendung als öffentlichen Client** Umschalten auf **Ja**, und wählen Sie dann **Speichern** aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-128">Locate the **Default client type** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Ein Screenshot des Typs "Standard Clienttyp"](/tutorial/images/aad-default-client-type.png)

## <a name="configure-the-sample"></a><span data-ttu-id="97e3a-130">Konfigurieren des Beispiels</span><span class="sxs-lookup"><span data-stu-id="97e3a-130">Configure the sample</span></span>

1. <span data-ttu-id="97e3a-131">Benennen `oAuth.properties.example` Sie die Datei in `oAuth.properties` .</span><span class="sxs-lookup"><span data-stu-id="97e3a-131">Rename the `oAuth.properties.example` file to `oAuth.properties`.</span></span>
1. <span data-ttu-id="97e3a-132">Bearbeiten Sie die `oAuth.properties` Datei, und nehmen Sie die folgenden Änderungen vor.</span><span class="sxs-lookup"><span data-stu-id="97e3a-132">Edit the `oAuth.properties` file and make the following changes.</span></span>
    1. <span data-ttu-id="97e3a-133">Ersetzen `YOUR_APP_ID_HERE` Sie durch die **Anwendungs-ID** , die Sie im App-Registrierungs Portal erhalten haben.</span><span class="sxs-lookup"><span data-stu-id="97e3a-133">Replace `YOUR_APP_ID_HERE` with the **Application Id** you got from the App Registration Portal.</span></span>

## <a name="build-and-run-the-sample"></a><span data-ttu-id="97e3a-134">Erstellen und Ausführen des Beispiels</span><span class="sxs-lookup"><span data-stu-id="97e3a-134">Build and run the sample</span></span>

<span data-ttu-id="97e3a-135">Navigieren Sie in der Befehlszeilenschnittstelle (CLI) zum Projektverzeichnis, und führen Sie die folgenden Befehle aus.</span><span class="sxs-lookup"><span data-stu-id="97e3a-135">In your command-line interface (CLI), navigate to the project directory and run the following commands.</span></span>

```Shell
./gradlew --console plain run
```
