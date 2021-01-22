---
ms.openlocfilehash: 16e96edc78ed2f6955bc14654edba1cb26323648
ms.sourcegitcommit: 2c0e0d2d6de994022dfa0faa10131582fb10e9b1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "49919529"
---
<!-- markdownlint-disable MD002 MD041 -->

In dieser Übung integrieren Sie Microsoft Graph in die Anwendung. Für diese Anwendung verwenden Sie das [Microsoft Graph SDK for Java,](https://github.com/microsoftgraph/msgraph-sdk-java) um Aufrufe an Microsoft Graph zu senden.

## <a name="implement-an-authentication-provider"></a>Implementieren eines Authentifizierungsanbieters

Das Microsoft Graph SDK für Java erfordert eine Implementierung der `IAuthenticationProvider` Schnittstelle, um das Objekt zu `GraphServiceClient` instanziieren.

1. Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"SimpleAuthProvider.java",** und fügen Sie den folgenden Code hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/SimpleAuthProvider.java" id="AuthProviderSnippet":::

## <a name="get-user-details"></a>Benutzerdetails abrufen

1. Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"Graph.java",** und fügen Sie den folgenden Code hinzu.

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

1. Fügen Sie die `import` folgende Anweisung am Anfang von **App.java hinzu.**

    ```java
    import com.microsoft.graph.models.extensions.User;
    ```

1. Fügen Sie den folgenden Code in **App.java** direkt vor der Zeile hinzu, um den Benutzer zu erhalten und den `Scanner input = new Scanner(System.in);` Anzeigenamen des Benutzers ausausgabe.

    ```java
    // Greet the user
    User user = Graph.getUser(accessToken);
    System.out.println("Welcome " + user.displayName);
    System.out.println("Time zone: " + user.mailboxSettings.timeZone);
    System.out.println();
    ```

1. Führen Sie die App aus. Nachdem Sie sich bei der App anmelden, werden Sie mit ihrem Namen willkommen gegrüst.

## <a name="get-calendar-events-from-outlook"></a>Abrufen von Kalenderereignissen von Outlook

1. Fügen Sie die folgende Funktion zur `Graph` Klasse in **Graph.java** hinzu, um Ereignisse aus dem Kalender des Benutzers zu erhalten.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="GetEventsSnippet":::

Überlegen Sie sich, was dieser Code macht.

- Die URL, die aufgerufen wird, lautet `/me/calendarview`.
  - `QueryOption` -Objekte werden verwendet, um die Parameter und Parameter hinzuzufügen, indem der Anfang und das Ende `startDateTime` `endDateTime` der Kalenderansicht festgelegt werden.
  - Ein `QueryOption` Objekt wird verwendet, um den Parameter hinzuzufügen `$orderby` und die Ergebnisse nach Startzeit zu sortieren.
  - Ein Objekt wird zum Hinzufügen der Kopfzeile verwendet, wodurch die Start- und Endzeiten an die Zeitzone des `HeaderOption` `Prefer: outlook.timezone` Benutzers angepasst werden.
  - Die Funktion beschränkt die für jedes Ereignis zurückgegebenen Felder auf die Felder, die `select` von der App tatsächlich verwendet werden.
  - Die Funktion beschränkt die Anzahl der Ereignisse in der Antwort auf `top` maximal 25.
- Die Funktion wird verwendet, um zusätzliche Seiten mit Ergebnissen an fordern, wenn in der aktuellen Woche mehr als `getNextPage` 25 Ereignisse enthalten sind.

1. Erstellen Sie eine neue Datei im **Verzeichnis "./graphtutorial/src/main/java/graphtutorial"** mit dem Namen **"GraphToIana.java",** und fügen Sie den folgenden Code hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/GraphToIana.java" id="zoneMappingsSnippet":::

    Diese Klasse implementiert eine einfache Suche zum Konvertieren von Windows-Zeitzonennamen in IANA-Bezeichner und zum Generieren einer **ZoneId** basierend auf einem Windows-Zeitzonennamen.

## <a name="display-the-results"></a>Anzeigen der Ergebnisse

1. Fügen Sie die folgenden `import` Anweisungen in **App.java hinzu.**

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

1. Fügen Sie der Klasse die folgende Funktion hinzu, um die `App` [dateTimeTimeZone-Eigenschaften](/graph/api/resources/datetimetimezone?view=graph-rest-1.0) aus Microsoft Graph in ein benutzerfreundliches Format zu formatieren.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="FormatDateSnippet":::

1. Fügen Sie der Klasse die folgende Funktion hinzu, um die Ereignisse des Benutzers zu erhalten und `App` sie an die Konsole ausausgabe.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="ListEventsSnippet":::

1. Fügen Sie folgendes direkt nach dem `// List the calendar` Kommentar in der Funktion `main` hinzu.

    ```java
    listCalendarEvents(accessToken, user.mailboxSettings.timeZone);
    ```

1. Speichern Sie alle Änderungen, erstellen Sie die App, und führen Sie sie aus. Wählen Sie **die Option "Kalenderereignisse auflisten"** aus, um eine Liste der Ereignisse des Benutzers anzuzeigen.

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
