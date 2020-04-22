---
ms.openlocfilehash: 3e4d7f11c89947da29873c85ab2808279c94265a
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612061"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="83a92-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="83a92-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="83a92-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="83a92-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="83a92-103">In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="83a92-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="83a92-104">Erstellen Sie im Verzeichnis **./src/main/resources** ein neues Verzeichnis mit dem Namen **graphtutorial** .</span><span class="sxs-lookup"><span data-stu-id="83a92-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="83a92-105">Erstellen Sie eine neue Datei im Verzeichnis **./src/main/resources/graphtutorial** mit dem Namen **oAuth. Properties**, und fügen Sie den folgenden Text in die Datei ein.</span><span class="sxs-lookup"><span data-stu-id="83a92-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="83a92-106">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID, die Sie im Azure-Portal erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="83a92-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="83a92-107">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **oAuth. Properties** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="83a92-107">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="83a92-108">Öffnen Sie **app. Java** , und fügen `import` Sie die folgenden Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="83a92-108">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="83a92-109">Fügen Sie den folgenden Code direkt vor `Scanner input = new Scanner(System.in);` der Codezeile ein, um die Datei **oAuth. Properties** zu laden.</span><span class="sxs-lookup"><span data-stu-id="83a92-109">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="83a92-110">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="83a92-110">Implement sign-in</span></span>

1. <span data-ttu-id="83a92-111">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Authentication. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="83a92-111">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="83a92-112">Fügen Sie in **app. Java**den folgenden Code unmittelbar vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um ein Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="83a92-112">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="83a92-113">Fügen Sie die folgende Reihe nach `// Display access token` dem Kommentar hinzu.</span><span class="sxs-lookup"><span data-stu-id="83a92-113">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="83a92-114">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="83a92-114">Run the app.</span></span> <span data-ttu-id="83a92-115">Die Anwendung zeigt eine URL und einen Gerätecode an.</span><span class="sxs-lookup"><span data-stu-id="83a92-115">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="83a92-116">Öffnen Sie einen Browser, und navigieren Sie zu der angezeigten URL.</span><span class="sxs-lookup"><span data-stu-id="83a92-116">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="83a92-117">Geben Sie den bereitgestellten Code ein und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="83a92-117">Enter the provided code and sign in.</span></span> <span data-ttu-id="83a92-118">Kehren Sie nach Abschluss zurück zur Anwendung, und wählen Sie die **1 aus. Option Zugriffstoken anzeigen** , um das Zugriffstoken anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="83a92-118">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>
