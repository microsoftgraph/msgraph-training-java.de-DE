---
ms.openlocfilehash: c21adae303c65e52ec2402c56e174066f879f40b
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612085"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="553d7-101">In dieser Übung erstellen Sie mithilfe des Azure Active Directory Admin Center eine neue Azure AD Anwendung.</span><span class="sxs-lookup"><span data-stu-id="553d7-101">In this exercise you will create a new Azure AD application using the Azure Active Directory admin center.</span></span>

1. <span data-ttu-id="553d7-102">Öffnen Sie einen Browser, und navigieren Sie zum [Azure Active Directory Admin Center](https://aad.portal.azure.com). Melden Sie sich mit einem **persönlichen Konto** (auch: Microsoft-Konto) oder einem **Geschäfts- oder Schulkonto** an.</span><span class="sxs-lookup"><span data-stu-id="553d7-102">Open a browser and navigate to the [Azure Active Directory admin center](https://aad.portal.azure.com) and login using a **personal account** (aka: Microsoft Account) or **Work or School Account**.</span></span>

1. <span data-ttu-id="553d7-103">Wählen Sie in der linken Navigationsleiste **Azure Active Directory** aus, und wählen Sie dann **App-Registrierungen** unter **Verwalten** aus.</span><span class="sxs-lookup"><span data-stu-id="553d7-103">Select **Azure Active Directory** in the left-hand navigation, then select **App registrations** under **Manage**.</span></span>

    ![<span data-ttu-id="553d7-104">Screenshot der APP-Registrierungen</span><span class="sxs-lookup"><span data-stu-id="553d7-104">A screenshot of the App registrations</span></span> ](./images/aad-portal-app-registrations.png)

1. <span data-ttu-id="553d7-105">Wählen Sie **Neue Registrierung** aus.</span><span class="sxs-lookup"><span data-stu-id="553d7-105">Select **New registration**.</span></span> <span data-ttu-id="553d7-106">Legen Sie auf der Seite **Anwendung registrieren** die Werte wie folgt fest.</span><span class="sxs-lookup"><span data-stu-id="553d7-106">On the **Register an application** page, set the values as follows.</span></span>

    - <span data-ttu-id="553d7-107">Legen Sie **Name** auf `Java Graph Tutorial` fest.</span><span class="sxs-lookup"><span data-stu-id="553d7-107">Set **Name** to `Java Graph Tutorial`.</span></span>
    - <span data-ttu-id="553d7-108">Legen Sie **Unterstützte Kontotypen** auf **Konten in allen Organisationsverzeichnissen und persönliche Microsoft-Konten** fest.</span><span class="sxs-lookup"><span data-stu-id="553d7-108">Set **Supported account types** to **Accounts in any organizational directory and personal Microsoft accounts**.</span></span>
    - <span data-ttu-id="553d7-109">Ändern Sie unter **Umleitungs-URI**das Dropdown-Menü auf **Public Client (Mobile & Desktop)**, `https://login.microsoftonline.com/common/oauth2/nativeclient`und legen Sie den Wert auf fest.</span><span class="sxs-lookup"><span data-stu-id="553d7-109">Under **Redirect URI**, change the dropdown to **Public client (mobile & desktop)**, and set the value to `https://login.microsoftonline.com/common/oauth2/nativeclient`.</span></span>

    ![Screenshot der Seite "Anwendung registrieren"](./images/aad-register-an-app.png)

1. <span data-ttu-id="553d7-111">Wählen Sie **Registrieren** aus.</span><span class="sxs-lookup"><span data-stu-id="553d7-111">Choose **Register**.</span></span> <span data-ttu-id="553d7-112">Kopieren Sie auf der Seite **Java Graph Tutorial** den Wert der **Anwendungs-ID (Client)** , und speichern Sie ihn, die Sie im nächsten Schritt benötigen.</span><span class="sxs-lookup"><span data-stu-id="553d7-112">On the **Java Graph Tutorial** page, copy the value of the **Application (client) ID** and save it, you will need it in the next step.</span></span>

    ![Screenshot der Anwendungs-ID der neuen App-Registrierung](./images/aad-application-id.png)

1. <span data-ttu-id="553d7-114">Wählen Sie unter **Verwalten** die Option **Authentifizierung** aus.</span><span class="sxs-lookup"><span data-stu-id="553d7-114">Select **Authentication** under **Manage**.</span></span> <span data-ttu-id="553d7-115">Suchen Sie den Abschnitt **Erweiterte Einstellungen** , und ändern Sie die Option **Anwendung als öffentlichen Client** Umschalten auf **Ja**, und wählen Sie dann **Speichern**aus.</span><span class="sxs-lookup"><span data-stu-id="553d7-115">Locate the **Advanced settings** section and change the **Treat application as a public client** toggle to **Yes**, then choose **Save**.</span></span>

    ![Ein Screenshot des Typs "Standard Clienttyp"](./images/aad-default-client-type.png)
