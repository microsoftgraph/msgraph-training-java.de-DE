---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919529"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="35653-101">In dieser Übung integrieren Sie Microsoft Graph in die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="35653-101">In this exercise you will incorporate the Microsoft Graph into the application.</span></span> <span data-ttu-id="35653-102">Für diese Anwendung verwenden Sie das [Microsoft Graph SDK for Java,](https://github.com/microsoftgraph/msgraph-sdk-java) um Aufrufe an Microsoft Graph zu senden.</span><span class="sxs-lookup"><span data-stu-id="35653-102">For this application, you will use the [Microsoft Graph SDK for Java](https://github.com/microsoftgraph/msgraph-sdk-java) to make calls to Microsoft Graph.</span></span>

## <a name="implement-an-authentication-provider"></a><span data-ttu-id="35653-103">Implementieren eines Authentifizierungsanbieters</span><span class="sxs-lookup"><span data-stu-id="35653-103">Implement an authentication provider</span></span>

<span data-ttu-id="35653-104">Das Microsoft Graph SDK für Java erfordert eine Implementierung der `IAuthenticationProvider` Schnittstelle, um das Objekt zu `GraphServiceClient` instanziieren.</span><span class="sxs-lookup"><span data-stu-id="35653-104">The Microsoft Graph SDK for Java requires an implementation of the `IAuthenticationProvider` interface to instantiate its `GraphServiceClient` object.</span></span>

1. <span data-ttu-id="35653-105">Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"SimpleAuthProvider.java",** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="35653-105">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **SimpleAuthProvider.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a><span data-ttu-id="35653-106">Benutzerdetails abrufen</span><span class="sxs-lookup"><span data-stu-id="35653-106">Get user details</span></span>

1. <span data-ttu-id="35653-107">Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"Graph.java",** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="35653-107">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **Graph.java** and add the following code.</span></span>

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

1. <span data-ttu-id="35653-108">Fügen Sie die `import` folgende Anweisung am Anfang von **App.java hinzu.**</span><span class="sxs-lookup"><span data-stu-id="35653-108">Add the following `import` statement at the top of **App.java**.</span></span>

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. <span data-ttu-id="35653-109">Fügen Sie den folgenden Code in **App.java** direkt vor der Zeile hinzu, um den Benutzer zu erhalten und den `Scanner input = new Scanner(System.in);` Anzeigenamen des Benutzers ausausgabe.</span><span class="sxs-lookup"><span data-stu-id="35653-109">Add the following code in **App.java** just before the `Scanner input = new Scanner(System.in);` line to get the user and output the user's display name.</span></span>

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. <span data-ttu-id="35653-110">Führen Sie die App aus.</span><span class="sxs-lookup"><span data-stu-id="35653-110">Run the app.</span></span> <span data-ttu-id="35653-111">Nachdem Sie sich bei der App anmelden, werden Sie mit ihrem Namen willkommen gegrüst.</span><span class="sxs-lookup"><span data-stu-id="35653-111">After you log in the app welcomes you by name.</span></span>

## <a name="get-calendar-events-from-outlook"></a><span data-ttu-id="35653-112">Abrufen von Kalenderereignissen von Outlook</span><span class="sxs-lookup"><span data-stu-id="35653-112">Get calendar events from Outlook</span></span>

1. <span data-ttu-id="35653-113">Fügen Sie die folgende Funktion zur `Graph` Klasse in **Graph.java** hinzu, um Ereignisse aus dem Kalender des Benutzers zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="35653-113">Add the following function to the `Graph` class in **Graph.java** to get events from the user's calendar.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

<span data-ttu-id="35653-114">Überlegen Sie sich, was dieser Code macht.</span><span class="sxs-lookup"><span data-stu-id="35653-114">Consider what this code is doing.</span></span>

- <span data-ttu-id="35653-115">Die URL, die aufgerufen wird, lautet `/me/calendarview`.</span><span class="sxs-lookup"><span data-stu-id="35653-115">The URL that will be called is `/me/calendarview`.</span></span>
  - <span data-ttu-id="35653-116">`QueryOption` -Objekte werden verwendet, um die Parameter und Parameter hinzuzufügen, indem der Anfang und das Ende `startDateTime` `endDateTime` der Kalenderansicht festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="35653-116">`QueryOption` objects are used to add the `startDateTime` and `endDateTime` parameters, setting the start and end of the calendar view.</span></span>
  - <span data-ttu-id="35653-117">Ein `QueryOption` Objekt wird verwendet, um den Parameter hinzuzufügen `$orderby` und die Ergebnisse nach Startzeit zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="35653-117">A `QueryOption` object is used to add the `$orderby` parameter, sorting the results by start time.</span></span>
  - <span data-ttu-id="35653-118">Ein Objekt wird zum Hinzufügen der Kopfzeile verwendet, wodurch die Start- und Endzeiten an die Zeitzone des `HeaderOption` `Prefer: outlook.timezone` Benutzers angepasst werden.</span><span class="sxs-lookup"><span data-stu-id="35653-118">A `HeaderOption` object is used to add the `Prefer: outlook.timezone` header, causing the start and end times to be adjusted to the user's time zone.</span></span>
  - <span data-ttu-id="35653-119">Die Funktion beschränkt die für jedes Ereignis zurückgegebenen Felder auf die Felder, die `select` von der App tatsächlich verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="35653-119">The `select` function limits the fields returned for each event to just those the app will actually use.</span></span>
  - <span data-ttu-id="35653-120">Die Funktion beschränkt die Anzahl der Ereignisse in der Antwort auf `top` maximal 25.</span><span class="sxs-lookup"><span data-stu-id="35653-120">The `top` function limits the number of events in the response to a maximum of 25.</span></span>
- <span data-ttu-id="35653-121">Die Funktion wird verwendet, um zusätzliche Seiten mit Ergebnissen an fordern, wenn in der aktuellen Woche mehr als `getNextPage` 25 Ereignisse enthalten sind.</span><span class="sxs-lookup"><span data-stu-id="35653-121">The `getNextPage` function is used to request additional pages of results if there are more than 25 events in the current week.</span></span>

1. <span data-ttu-id="35653-122">Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"GraphToIana.java",** und fügen Sie den folgenden Code hinzu.</span><span class="sxs-lookup"><span data-stu-id="35653-122">Create a new file in the **./graphtutorial/src/main/java/graphtutorial** directory named **GraphToIana.java** and add the following code.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    <span data-ttu-id="35653-123">Diese Klasse implementiert eine einfache Suche zum Konvertieren von Windows-Zeitzonennamen in IANA-Bezeichner und zum Generieren einer **ZoneId** basierend auf einem Windows-Zeitzonennamen.</span><span class="sxs-lookup"><span data-stu-id="35653-123">This class implements a simple lookup to convert Windows time zone names to IANA identifiers, and to generate a **ZoneId** based on a Windows time zone name.</span></span>

## <a name="display-the-results"></a><span data-ttu-id="35653-124">Anzeigen der Ergebnisse</span><span class="sxs-lookup"><span data-stu-id="35653-124">Display the results</span></span>

1. <span data-ttu-id="35653-125">Fügen Sie die folgenden `import` Anweisungen in **App.java hinzu.**</span><span class="sxs-lookup"><span data-stu-id="35653-125">Add the following `import` statements in **App.java**.</span></span>

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

1. <span data-ttu-id="35653-126">Fügen Sie der Klasse die folgende Funktion hinzu, um die `App` [dateTimeTimeZone-Eigenschaften](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) aus Microsoft Graph in ein benutzerfreundliches Format zu formatieren.</span><span class="sxs-lookup"><span data-stu-id="35653-126">Add the following function to the `App` class to format the [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) properties from Microsoft Graph into a user-friendly format.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. <span data-ttu-id="35653-127">Fügen Sie der Klasse die folgende Funktion hinzu, um die Ereignisse des Benutzers zu erhalten und `App` sie an die Konsole ausausgabe.</span><span class="sxs-lookup"><span data-stu-id="35653-127">Add the following function to the `App` class to get the user's events and output them to the console.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. <span data-ttu-id="35653-128">Fügen Sie folgendes direkt nach dem `// List the calendar` Kommentar in der Funktion `main` hinzu.</span><span class="sxs-lookup"><span data-stu-id="35653-128">Add the following just after the `// List the calendar` comment in the `main` function.</span></span>

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. <span data-ttu-id="35653-129">Speichern Sie alle Änderungen, erstellen Sie die App, und führen Sie sie aus.</span><span class="sxs-lookup"><span data-stu-id="35653-129">Save all of your changes, build the app, then run it.</span></span> <span data-ttu-id="35653-130">Wählen Sie **die Option "Kalenderereignisse auflisten"** aus, um eine Liste der Ereignisse des Benutzers anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="35653-130">Choose the **List calendar events** option to see a list of the user's events.</span></span>

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
