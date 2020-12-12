---
ms.openlocfilehash: c03403209c6985dd3235488891a263e447c72149
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661140"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="88077-101">In diesem Abschnitt können Sie die Möglichkeit zum Erstellen von Ereignissen im Kalender des Benutzers hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="88077-101">In this section you will add the ability to create events on the user's calendar.</span></span>

1. <span data-ttu-id="88077-102">Öffnen Sie **/graphtutorial/src/main/java/graphtutorial/Graph.Java** , und fügen Sie die folgende Funktion zur **Graph** -Klasse hinzu.</span><span class="sxs-lookup"><span data-stu-id="88077-102">Open **./graphtutorial/src/main/java/graphtutorial/Graph.java** and add the following function to the **Graph** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/Graph.java" id="CreateEventSnippet":::

1. <span data-ttu-id="88077-103">Öffnen Sie **/graphtutorial/src/main/java/graphtutorial/app.Java** , und fügen Sie der **App** -Klasse die folgende Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="88077-103">Open **./graphtutorial/src/main/java/graphtutorial/App.java** and add the following function to the **App** class.</span></span>

    :::code language="java" source="../demo/graphtutorial/src/main/java/graphtutorial/App.java" id="CreateEventSnippet":::

    <span data-ttu-id="88077-104">Diese Funktion fordert den Benutzer für Betreff, Teilnehmer, Start, Ende und Text an und verwendet dann diese Werte zum Aufrufen `Graph.createEvent` .</span><span class="sxs-lookup"><span data-stu-id="88077-104">This function prompts the user for subject, attendees, start, end, and body, then uses those values to call `Graph.createEvent`.</span></span>

1. <span data-ttu-id="88077-105">Fügen Sie das folgende direkt nach dem `// Create a new event` Kommentar in der `Main` -Funktion hinzu.</span><span class="sxs-lookup"><span data-stu-id="88077-105">Add the following just after the `// Create a new event` comment in the `Main` function.</span></span>

    ```java
    createEvent(accessToken, user.mailboxSettings.timeZone, input);
    ```

1. <span data-ttu-id="88077-106">Speichern Sie alle Änderungen, und führen Sie die APP aus.</span><span class="sxs-lookup"><span data-stu-id="88077-106">Save all of your changes and run the app.</span></span> <span data-ttu-id="88077-107">Wählen Sie die Option **Ereignis hinzufügen** aus.</span><span class="sxs-lookup"><span data-stu-id="88077-107">Choose the **Add an event** option.</span></span> <span data-ttu-id="88077-108">Antworten Sie auf die Eingabeaufforderungen, um ein neues Ereignis im Kalender des Benutzers zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="88077-108">Respond to the prompts to create a new event on the user's calendar.</span></span>
