---
ms.openlocfilehash: 93688a97872ad640c12c7137f4cc09ede4a98416
ms.sourcegitcommit: 189f87d879c57b11992e7bc75580b4c69e014122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/21/2020
ms.locfileid: "43612068"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="28173-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="28173-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="28173-102">Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="28173-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="28173-103">Implementieren eines Authentifizierungsanbieters</span><span class="sxs-lookup"><span data-stu-id="28173-103">Implement an authentication provider</span></span>

<span data-ttu-id="28173-104">Das Microsoft Graph `IAuthenticationProvider` -SDK für Java erfordert eine Implementierung der Schnittstelle, um `GraphServiceClient` das zugehörige Objekt zu instanziieren.</span><span class="sxs-lookup"><span data-stu-id="28173-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="28173-105">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **SimpleAuthProvider. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="28173-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="28173-106">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="28173-106">Get user details</span></span>

1. <span data-ttu-id="28173-107">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Graph. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="28173-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.util.LinkedList;
    import java.util.List;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;

    /**
     * Graph
     */
    public class Graph {

        private static IGraphServiceClient graphClient = null;
        private static SimpleAuthProvider authProvider = null;

        private static void ensureGraphClient(String accessToken) {
            if (graphClient == null) {
                // Create the auth provider
                authProvider = new SimpleAuthProvider(accessToken);

                // Create default logger to only log errors
                DefaultLogger logger = new DefaultLogger();
                logger.setLoggingLevel(LoggerLevel.ERROR);

                // Build a Graph client
                graphClient = GraphServiceClient.builder()
                    .authenticationProvider(authProvider)
                    .logger(logger)
                    .buildClient();
            }
        }

        public static User getUser(String accessToken) {
            ensureGraphClient(accessToken);

            // GET /me to get authenticated user
            User me = graphClient
                .me()
                .buildRequest()
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="28173-108">Fügen Sie die `import` folgende Anweisung oben in **app. Java**hinzu.</span><span class="sxs-lookup"><span data-stu-id="28173-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="28173-109">Fügen Sie den folgenden Code in **app. Java** direkt vor `Scanner input = new Scanner(System.in);` der Codezeile hinzu, um den Benutzer abzurufen und den Anzeigenamen des Benutzers auszugeben.</span><span class="sxs-lookup"><span data-stu-id="28173-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println();
    ```

1. <span data-ttu-id="28173-110">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="28173-110">Run the app.</span></span> <span data-ttu-id="28173-111">Nachdem Sie sich angemeldet haben, werden Sie von der APP nach Namen begrüßt.</span><span class="sxs-lookup"><span data-stu-id="28173-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="28173-112">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="28173-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="28173-113">Fügen Sie die folgende Funktion zur `Graph` Klasse in **Graph. Java** hinzu, um Ereignisse aus dem Kalender des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="28173-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="28173-114">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="28173-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="28173-115">Die URL, die aufgerufen wird, lautet `/me/events`.</span><span class="sxs-lookup"><span data-stu-id="28173-115">The URL that will be called is `/me/events`.</span></span>
- <span data-ttu-id="28173-116">Die `select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die von der APP tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="28173-116">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
- <span data-ttu-id="28173-117">A `QueryOption` wird verwendet, um die Ergebnisse nach dem Datum und der Uhrzeit, zu der Sie erstellt wurden, zu sortieren, wobei das letzte Element zuerst angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="28173-117">A `QueryOption` is used to sort the results by the date and time they were created, with the most recent item being first.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="28173-118">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="28173-118">Display the results</span></span>

1. <span data-ttu-id="28173-119">Fügen Sie die `import` folgenden Anweisungen in **app. Java**hinzu.</span><span class="sxs-lookup"><span data-stu-id="28173-119">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.LocalDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.FormatStyle;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="28173-120">Fügen Sie die folgende Funktion zur `App` -Klasse hinzu, um die [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Eigenschaften aus Microsoft Graph in ein benutzerfreundliches Format zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="28173-120">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="28173-121">Fügen Sie die folgende Funktion zur `App` -Klasse hinzu, um die Ereignisse des Benutzers abzurufen und Sie in der Konsole auszugeben.</span><span class="sxs-lookup"><span data-stu-id="28173-121">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="28173-122">Fügen Sie das folgende direkt nach `// List the calendar` dem Kommentar in `main` der-Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="28173-122">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="28173-123">Speichern Sie alle Änderungen, erstellen Sie die APP, und führen Sie Sie aus.</span><span class="sxs-lookup"><span data-stu-id="28173-123">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="28173-124">Wählen Sie die Option **Listen Kalenderereignisse** aus, um eine Liste der Ereignisse des Benutzers anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="28173-124">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. List calendar events
    2
    Events:
    Subject: Team meeting
      Organizer: Adele Vance
      Start: 5/22/19, 3:00 PM (UTC)
      End: 5/22/19, 4:00 PM (UTC)
    Subject: Team Lunch
      Organizer: Adele Vance
      Start: 5/24/19, 6:30 PM (UTC)
      End: 5/24/19, 8:00 PM (UTC)
    Subject: Flight to Redmond
      Organizer: Adele Vance
      Start: 5/26/19, 4:30 PM (UTC)
      End: 5/26/19, 7:00 PM (UTC)
    Subject: Let's meet to discuss strategy
      Organizer: Patti Fernandez
      Start: 5/27/19, 10:00 PM (UTC)
      End: 5/27/19, 10:30 PM (UTC)
    Subject: All-hands meeting
      Organizer: Adele Vance
      Start: 5/28/19, 3:30 PM (UTC)
      End: 5/28/19, 5:00 PM (UTC)
    ```
