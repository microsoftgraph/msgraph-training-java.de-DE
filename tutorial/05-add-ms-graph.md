---
ms.openlocfilehash: c26e5b8ab0b7c5c62b926e3f5416b94e3f10b601
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661046"
---
<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung werden Sie das Microsoft Graph in die Anwendung integrieren. Für diese Anwendung verwenden Sie das [Microsoft Graph-SDK für Java](https://github.com/microsoftgraph/msgraph-sdk-java) , um Anrufe an Microsoft Graph zu tätigen.

## <a name="implement-an-authentication-provider"></a>Implementieren eines Authentifizierungsanbieters

Das Microsoft Graph-SDK für Java erfordert eine Implementierung der `IAuthenticationProvider` Schnittstelle, um das zugehörige Objekt zu instanziieren `GraphServiceClient` .

1. Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **SimpleAuthProvider. Java** , und fügen Sie den folgenden Code hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Benutzerdetails abrufen

1. Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **Graph. Java** , und fügen Sie den folgenden Code hinzu.

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

1. Fügen Sie die folgende `import` Anweisung oben in **app. Java** hinzu.

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Fügen Sie den folgenden Code in **app. Java** direkt vor der `Scanner input = new Scanner(System.in);` Codezeile hinzu, um den Benutzer abzurufen und den Anzeigenamen des Benutzers auszugeben.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Führen Sie die App aus. Nachdem Sie sich angemeldet haben, werden Sie von der APP nach Namen begrüßt.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie die folgende Funktion zur `Graph` Klasse in **Graph. Java** hinzu, um Ereignisse aus dem Kalender des Benutzers abzurufen.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Überlegen Sie sich, was dieser Code macht.

- Die URL, die aufgerufen wird, lautet `/me/calendarview`.
  - `QueryOption` Objekte werden verwendet, um die `startDateTime` -und-Parameter hinzuzufügen und `endDateTime` den Anfang und das Ende der Kalenderansicht festzulegen.
  - Ein `QueryOption` -Objekt wird zum Hinzufügen des `$orderby` Parameters verwendet, wobei die Ergebnisse nach Startzeit sortiert werden.
  - Ein `HeaderOption` -Objekt wird verwendet, um die Kopfzeile hinzuzufügen `Prefer: outlook.timezone` , wodurch die Anfangs-und Endzeiten an die Zeitzone des Benutzers angepasst werden.
  - Die `select` Funktion schränkt die für jedes Ereignis zurückgegebenen Felder auf diejenigen ein, die von der APP tatsächlich verwendet werden.
  - Die `top` Funktion schränkt die Anzahl der Ereignisse in der Antwort auf maximal 25 ein.
- Die `getNextPage` -Funktion wird verwendet, um zusätzliche Seiten mit Ergebnissen anzufordern, wenn in der aktuellen Woche mehr als 25 Ereignisse vorhanden sind.

1. Erstellen Sie eine neue Datei im **./graphtutorial/src/main/java/graphtutorial** -Verzeichnis mit dem Namen **GraphToIana. Java** , und fügen Sie den folgenden Code hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Diese Klasse implementiert eine einfache Suche zum Konvertieren von Windows-Zeitzonennamen in IANA-IDs und zum Generieren einer **Zonen** -ID basierend auf einem Windows-Zeitzonennamen.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

1. Fügen Sie die folgenden `import` Anweisungen in **app. Java** hinzu.

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

1. Fügen Sie die folgende Funktion zur- `App` Klasse hinzu, um die [dateTimeTimeZone](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) -Eigenschaften aus Microsoft Graph in ein benutzerfreundliches Format zu formatieren.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Fügen Sie die folgende Funktion zur `App` -Klasse hinzu, um die Ereignisse des Benutzers abzurufen und Sie in der Konsole auszugeben.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Fügen Sie das folgende direkt nach dem `// List the calendar` Kommentar in der `main` -Funktion hinzu.

    ```java
    listCalendarEvents(accessToken);
    ```

1. Speichern Sie alle Änderungen, erstellen Sie die APP, und führen Sie Sie aus. Wählen Sie die Option **Listen Kalenderereignisse** aus, um eine Liste der Ereignisse des Benutzers anzuzeigen.

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
