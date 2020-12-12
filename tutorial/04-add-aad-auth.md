---
ms.openlocfilehash: 4c04317462240ff0696ac1381fae886481db7847
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661067"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="a80cf-101">In dieser Übung erweitern Sie die Anwendung aus der vorherigen Übung, um die Authentifizierung mit Azure AD zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="a80cf-101">In this exercise you will extend the application from the previous exercise to support authentication with Azure AD.</span></span> <span data-ttu-id="a80cf-102">Dies ist erforderlich, um das erforderliche OAuth-Zugriffstoken zum Aufrufen von Microsoft Graph zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="a80cf-102">This is required to obtain the necessary OAuth access token to call the Microsoft Graph.</span></span> <span data-ttu-id="a80cf-103">In diesem Schritt werden Sie die [Microsoft-Authentifizierungsbibliothek (MSAL) für Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="a80cf-103">In this step you will integrate the [Microsoft Authentication Library (MSAL) for Java](https://github.com/AzureAD/microsoft-authentication-library-for-java) into the application.</span></span>

1. <span data-ttu-id="a80cf-104">Erstellen Sie im Verzeichnis **./src/main/resources** ein neues Verzeichnis mit dem Namen **graphtutorial** .</span><span class="sxs-lookup"><span data-stu-id="a80cf-104">Create a new directory named **graphtutorial** in the **./src/main/resources** directory.</span></span>

1. <span data-ttu-id="a80cf-105">Erstellen Sie eine neue Datei im Verzeichnis **./src/main/resources/graphtutorial** mit dem Namen **oAuth. Properties**, und fügen Sie den folgenden Text in die Datei ein.</span><span class="sxs-lookup"><span data-stu-id="a80cf-105">Create a new file in the **./src/main/resources/graphtutorial** directory named **oAuth.properties**, and add the following text in that file.</span></span> <span data-ttu-id="a80cf-106">Ersetzen `YOUR_APP_ID_HERE` Sie durch die Anwendungs-ID, die Sie im Azure-Portal erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="a80cf-106">Replace `YOUR_APP_ID_HERE` with the application ID you created in the Azure portal.</span></span>

    :::code language="ini" source="../demo/graphtutorial/src/main/resources/graphtutorial/oAuth.properties.example":::

    <span data-ttu-id="a80cf-107">Der Wert von `app.scopes` enthält die Berechtigungs Bereiche, die die Anwendung benötigt.</span><span class="sxs-lookup"><span data-stu-id="a80cf-107">The value of `app.scopes` contains the permission scopes the application requires.</span></span>

    - <span data-ttu-id="a80cf-108">**User. Read** ermöglicht der APP den Zugriff auf das Profil des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="a80cf-108">**User.Read** allows the app to access the user's profile.</span></span>
    - <span data-ttu-id="a80cf-109">**Mailbox Settings. Read** ermöglicht der APP den Zugriff auf Einstellungen aus dem Postfach des Benutzers, einschließlich der konfigurierten Zeitzone des Benutzers.</span><span class="sxs-lookup"><span data-stu-id="a80cf-109">**MailboxSettings.Read** allows the app to access settings from the user's mailbox, including the user's configured time zone.</span></span>
    - <span data-ttu-id="a80cf-110">Mit Calendars **. ReadWrite** kann die APP den Kalender des Benutzers auflisten und dem Kalender neue Ereignisse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="a80cf-110">**Calendars.ReadWrite** allows the app to list the user's calendar and add new events to the calendar.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="a80cf-111">Wenn Sie die Quellcodeverwaltung wie git verwenden, wäre es jetzt ein guter Zeitpunkt, die Datei **oAuth. Properties** aus der Quellcodeverwaltung auszuschließen, um unbeabsichtigtes Auslaufen ihrer APP-ID zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="a80cf-111">If you're using source control such as git, now would be a good time to exclude the **oAuth.properties** file from source control to avoid inadvertently leaking your app ID.</span></span>

1. <span data-ttu-id="a80cf-112">Öffnen Sie **app. Java** , und fügen Sie die folgenden `import` Anweisungen hinzu.</span><span class="sxs-lookup"><span data-stu-id="a80cf-112">Open **App.java** and add the following `import` statements.</span></span>

    ```java
    import java.io.IOException;
    import java.util.Properties;
    ```

1. <span data-ttu-id="a80cf-113">Fügen Sie den folgenden Code direkt vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um die Datei **oAuth. Properties** zu laden.</span><span class="sxs-lookup"><span data-stu-id="a80cf-113">Add the following code just before the `Scanner input = new Scanner(System.in);` line to load the **oAuth.properties** file.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="LoadSettingsSnippet":::

## <a name="implement-sign-in"></a><span data-ttu-id="a80cf-114">Implementieren der Anmeldung</span><span class="sxs-lookup"><span data-stu-id="a80cf-114">Implement sign-in</span></span>

1. <span data-ttu-id="a80cf-115">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Authentication. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="a80cf-115">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Authentication.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Authentication.java" id="AuthenticationSnippet":::

1. <span data-ttu-id="a80cf-116">Fügen Sie in **app. Java** den folgenden Code unmittelbar vor der `Scanner input = new Scanner(System.in);` Codezeile ein, um ein Zugriffstoken abzurufen.</span><span class="sxs-lookup"><span data-stu-id="a80cf-116">In **App.java**, add the following code just before the `Scanner input = new Scanner(System.in);` line to get an access token.</span></span>

    ```java
    // Get an access token
    Authentication.initialize(appId);
    final String accessToken = Authentication.getUserAccessToken(appScopes);
    ```

1. <span data-ttu-id="a80cf-117">Fügen Sie die folgende Reihe nach dem `// Display access token` Kommentar hinzu.</span><span class="sxs-lookup"><span data-stu-id="a80cf-117">Add the following line after the `// Display access token` comment.</span></span>

    ```java
    System.out.println("Access token: " + accessToken);
    ```

1. <span data-ttu-id="a80cf-118">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="a80cf-118">Run the app.</span></span> <span data-ttu-id="a80cf-119">Die Anwendung zeigt eine URL und einen Gerätecode an.</span><span class="sxs-lookup"><span data-stu-id="a80cf-119">The application displays a URL and device code.</span></span>

    ```Shell
    Java Graph Tutorial

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code F7CG945YZ to authenticate.
    ```

1. <span data-ttu-id="a80cf-120">Öffnen Sie einen Browser, und navigieren Sie zu der angezeigten URL.</span><span class="sxs-lookup"><span data-stu-id="a80cf-120">Open a browser and browse to the URL displayed.</span></span> <span data-ttu-id="a80cf-121">Geben Sie den bereitgestellten Code ein und melden Sie sich an.</span><span class="sxs-lookup"><span data-stu-id="a80cf-121">Enter the provided code and sign in.</span></span> <span data-ttu-id="a80cf-122">Kehren Sie nach Abschluss zurück zur Anwendung, und wählen Sie die **1 aus. Option Zugriffstoken anzeigen** , um das Zugriffstoken anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a80cf-122">Once completed, return to the application and choose the **1. Display access token** option to display the access token.</span></span>

> [!TIP]
> <span data-ttu-id="a80cf-123">Zugriffstoken für Microsoft work-oder School-Konten können zu Problembehandlungszwecken unter analysiert werden [https://jwt.ms](https://jwt.ms) .</span><span class="sxs-lookup"><span data-stu-id="a80cf-123">Access tokens for Microsoft work or school accounts can be parsed for troubleshooting purposes at [https://jwt.ms](https://jwt.ms).</span></span> <span data-ttu-id="a80cf-124">Zugriffstoken für persönliche Microsoft-Konten verwenden ein proprietäres Format und können nicht analysiert werden.</span><span class="sxs-lookup"><span data-stu-id="a80cf-124">Access tokens for personal Microsoft accounts use a proprietary format and cannot be parsed.</span></span>
