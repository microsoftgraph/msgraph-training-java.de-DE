---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661140"
---
<!-- markdownlint-disable MD002 MD041 -->

In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.

1. Öffnen Sie **/graphtutorial/src/main/java/graphtutorial/Graph.Java** , und fügen Sie die folgende Funktion zur **Graph** -Klasse hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. Öffnen Sie **/graphtutorial/src/main/java/graphtutorial/app.Java** , und fügen Sie der **App** -Klasse die folgende Funktion hinzu.

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    Diese Funktion fordert den Benutzer für Betreff, Teilnehmer, Start, Ende und Text an und verwendet dann diese Werte zum Aufrufen `Graph.createEvent` .

1. Fügen Sie das folgende direkt nach dem `// Create a new event` Kommentar in der `Main` -Funktion hinzu.

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. Speichern Sie alle Änderungen, und führen Sie die APP aus. Wählen Sie die Option **Ereignis hinzufügen** aus. Antworten Sie auf die Eingabeaufforderungen, um ein neues Ereignis im Kalender des Benutzers zu erstellen.
