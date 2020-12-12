---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661046"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="f6228-101">In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren.</span><span class="sxs-lookup"><span data-stu-id="f6228-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="f6228-102">Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.</span><span class="sxs-lookup"><span data-stu-id="f6228-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="f6228-103">Implementieren eines Authentifizierungsanbieters</span><span class="sxs-lookup"><span data-stu-id="f6228-103">Implement an authentication provider</span></span>

<span data-ttu-id="f6228-104">Das Microsoft Graph-SDK für Java erfordert eine Implementierung der `IAuthenticationProvider` Schnittstelle, um das zugehörige Objekt zu instanziieren `GraphServiceClient` .</span><span class="sxs-lookup"><span data-stu-id="f6228-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="f6228-105">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **SimpleAuthProvider. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="f6228-106">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="f6228-106">Get user details</span></span>

1. <span data-ttu-id="f6228-107">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Graph. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

    ```java
    package graphtutorial;

    import java.time.LocalDateTime;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.util.LinkedList;
    import java.util.List;
    import java.util.Set;

    import com.microsoft.graph.logger.DefaultLogger;
    import com.microsoft.graph.logger.LoggerLevel;
    import com.microsoft.graph.models.extensions.Attendee;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.EmailAddress;
    import com.microsoft.graph.models.extensions.Event;
    import com.microsoft.graph.models.extensions.IGraphServiceClient;
    import com.microsoft.graph.models.extensions.ItemBody;
    import com.microsoft.graph.models.extensions.User;
    import com.microsoft.graph.models.generated.AttendeeType;
    import com.microsoft.graph.models.generated.BodyType;
    import com.microsoft.graph.options.HeaderOption;
    import com.microsoft.graph.options.Option;
    import com.microsoft.graph.options.QueryOption;
    import com.microsoft.graph.requests.extensions.GraphServiceClient;
    import com.microsoft.graph.requests.extensions.IEventCollectionPage;
    import com.microsoft.graph.requests.extensions.IEventCollectionRequestBuilder;

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
                .select("displayName,mailboxSettings")
                .get();

            return me;
        }
    }
    ```

1. <span data-ttu-id="f6228-108">Fügen Sie die folgende `import` Anweisung oben in **app. Java** hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="f6228-109">Fügen Sie den folgenden Code in **app. Java** direkt vor der `Scanner input = new Scanner(System.in);` Codezeile hinzu, um den Benutzer abzurufen und den Anzeigenamen des Benutzers auszugeben.</span><span class="sxs-lookup"><span data-stu-id="f6228-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="f6228-110">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="f6228-110">Run the app.</span></span> <span data-ttu-id="f6228-111">Nachdem Sie sich angemeldet haben, werden Sie von der APP nach Namen begrüßt.</span><span class="sxs-lookup"><span data-stu-id="f6228-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="f6228-112">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="f6228-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="f6228-113">Fügen Sie die folgende Funktion zur `Graph` Klasse in **Graph. Java** hinzu, um Ereignisse aus dem Kalender des Benutzers abzurufen.</span><span class="sxs-lookup"><span data-stu-id="f6228-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="f6228-114">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="f6228-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="f6228-115">Die URL, die aufgerufen wird, lautet `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="f6228-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="f6228-116">`QueryOption` Objekte werden verwendet, um die `startDateTime` -und-Parameter hinzuzufügen und `endDateTime` den Anfang und das Ende der Kalenderansicht festzulegen.</span><span class="sxs-lookup"><span data-stu-id="f6228-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="f6228-117">Ein `QueryOption` -Objekt wird zum Hinzufügen des `$orderby` Parameters verwendet, wobei die Ergebnisse nach Startzeit sortiert werden.</span><span class="sxs-lookup"><span data-stu-id="f6228-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="f6228-118">Ein `HeaderOption` -Objekt wird verwendet, um die Kopfzeile hinzuzufügen `Prefer: outlook.timezone` , wodurch die Anfangs-und Endzeiten an die Zeitzone des Benutzers angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="f6228-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="f6228-119">Die `select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die von der APP tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="f6228-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="f6228-120">Die `top` Funktion schränkt die Anzahl der Ereignisse in der Antwort auf maximal 25 ein.</span><span class="sxs-lookup"><span data-stu-id="f6228-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="f6228-121">Die `getNextPage` -Funktion wird verwendet, um zusätzliche Seiten mit Ergebnissen anzufordern, wenn in der aktuellen Woche mehr als 25 Ereignisse vorhanden sind.</span><span class="sxs-lookup"><span data-stu-id="f6228-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="f6228-122">Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **GraphToIana. Java** , und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="f6228-123">Diese Klasse implementiert eine einfache Suche zum Konvertieren von Windows-Zeitzonennamen in IANA-IDs und zum Generieren einer **Zonen** -ID basierend auf einem Windows-Zeitzonennamen.</span><span class="sxs-lookup"><span data-stu-id="f6228-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="f6228-124">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="f6228-124">Display the results</span></span>

1. <span data-ttu-id="f6228-125">Fügen Sie die folgenden `import` Anweisungen in **app. Java** hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-125">Add the following `import` statements in **App.java**.</span></span>

    ```java
    import java.time.DayOfWeek;
    import java.time.LocalDateTime;
    import java.time.ZoneId;
    import java.time.ZonedDateTime;
    import java.time.format.DateTimeFormatter;
    import java.time.format.DateTimeParseException;
    import java.time.format.FormatStyle;
    import java.time.temporal.ChronoUnit;
    import java.time.temporal.TemporalAdjusters;
    import java.util.HashSet;
    import java.util.List;
    import com.microsoft.graph.models.extensions.DateTimeTimeZone;
    import com.microsoft.graph.models.extensions.Event;
    ```

1. <span data-ttu-id="f6228-126">Fügen Sie die folgende Funktion zur- `App` Klasse hinzu, um die [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Eigenschaften aus Microsoft Graph in ein benutzerfreundliches Format zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="f6228-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="f6228-127">Fügen Sie die folgende Funktion zur `App` -Klasse hinzu, um die Ereignisse des Benutzers abzurufen und Sie in der Konsole auszugeben.</span><span class="sxs-lookup"><span data-stu-id="f6228-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="f6228-128">Fügen Sie das folgende direkt nach dem `// List the calendar` Kommentar in der `main` -Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="f6228-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken);
    ```

1. <span data-ttu-id="f6228-129">Speichern Sie alle Änderungen, erstellen Sie die APP, und führen Sie Sie aus.</span><span class="sxs-lookup"><span data-stu-id="f6228-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="f6228-130">Wählen Sie die Option **Listen Kalenderereignisse** aus, um eine Liste der Ereignisse des Benutzers anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="f6228-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

    ```Shell
    Welcome Adele Vance

    Please choose one of the following options:
    0. Exit
    1. Display access token
    2. View this week's calendar
    3. Add an event
    2
    Events:
    Subject: Weekly meeting
      Organizer: Lynne Robbins
      Start: 12/7/20, 2:00 PM (Pacific Standard Time)
      End: 12/7/20, 3:00 PM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/7/20, 4:00 PM (Pacific Standard Time)
      End: 12/7/20, 5:30 PM (Pacific Standard Time)
    Subject: Tailspin Toys Proposal Review + Lunch
      Organizer: Lidia Holloway
      Start: 12/8/20, 12:00 PM (Pacific Standard Time)
      End: 12/8/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/8/20, 3:00 PM (Pacific Standard Time)
      End: 12/8/20, 4:30 PM (Pacific Standard Time)
    Subject: Company Meeting
      Organizer: Christie Cline
      Start: 12/9/20, 8:30 AM (Pacific Standard Time)
      End: 12/9/20, 11:00 AM (Pacific Standard Time)
    Subject: Carpool
      Organizer: Lynne Robbins
      Start: 12/9/20, 4:00 PM (Pacific Standard Time)
      End: 12/9/20, 5:30 PM (Pacific Standard Time)
    Subject: Project Team Meeting
      Organizer: Lidia Holloway
      Start: 12/10/20, 8:00 AM (Pacific Standard Time)
      End: 12/10/20, 9:30 AM (Pacific Standard Time)
    Subject: Weekly Marketing Lunch
      Organizer: Adele Vance
      Start: 12/10/20, 12:00 PM (Pacific Standard Time)
      End: 12/10/20, 1:00 PM (Pacific Standard Time)
    Subject: Project Tailspin
      Organizer: Lidia Holloway
      Start: 12/10/20, 3:00 PM (Pacific Standard Time)
      End: 12/10/20, 4:30 PM (Pacific Standard Time)
    Subject: Lunch?
      Organizer: Lynne Robbins
      Start: 12/11/20, 12:00 PM (Pacific Standard Time)
      End: 12/11/20, 1:00 PM (Pacific Standard Time)
    Subject: Friday Unwinder
      Organizer: Megan Bowen
      Start: 12/11/20, 4:00 PM (Pacific Standard Time)
      End: 12/11/20, 5:00 PM (Pacific Standard Time)
    ```
