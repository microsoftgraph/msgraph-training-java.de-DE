---
ms.openlocfilehash: 3b1e366ded1e9d59f048eec8da23a686a121dedf
ms.sourcegitcommit: eb935a250f8531b04a42710356072b80d46ee3a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/11/2020
ms.locfileid: "49661074"
---
<!-- markdownlint-disable MD002 MD041 -->

<span data-ttu-id="343ff-101">In diesem Lernprogramm erfahren Sie, wie Sie eine Java Console-app erstellen, die die Microsoft Graph-API zum Abrufen von Kalenderinformationen für einen Benutzer verwendet.</span><span class="sxs-lookup"><span data-stu-id="343ff-101">This tutorial teaches you how to build a Java console app that uses the Microsoft Graph API to retrieve calendar information for a user.</span></span>

> [!TIP]
> <span data-ttu-id="343ff-102">Wenn Sie das fertige Lernprogramm einfach herunterladen möchten, können Sie das GitHub- [Repository](https://github.com/microsoftgraph/msgraph-training-java)herunterladen oder Klonen.</span><span class="sxs-lookup"><span data-stu-id="343ff-102">If you prefer to just download the completed tutorial, you can download or clone the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="343ff-103">Voraussetzungen</span><span class="sxs-lookup"><span data-stu-id="343ff-103">Prerequisites</span></span>

<span data-ttu-id="343ff-104">Bevor Sie mit diesem Lernprogramm beginnen, sollten Sie das [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) und [Gradle](https://gradle.org/) auf Ihrem Entwicklungscomputer installiert haben.</span><span class="sxs-lookup"><span data-stu-id="343ff-104">Before you start this tutorial, you should have the [Java SE Development Kit (JDK)](https://java.com/en/download/faq/develop.xml) and [Gradle](https://gradle.org/) installed on your development machine.</span></span> <span data-ttu-id="343ff-105">Wenn Sie nicht über das JDK oder Gradle verfügen, besuchen Sie die vorherigen Links für Downloadoptionen.</span><span class="sxs-lookup"><span data-stu-id="343ff-105">If you do not have the JDK or Gradle, visit the previous links for download options.</span></span>

<span data-ttu-id="343ff-106">Sie sollten auch über ein persönliches Microsoft-Konto mit einem Postfach auf Outlook.com oder ein Microsoft-Arbeits-oder Schulkonto verfügen.</span><span class="sxs-lookup"><span data-stu-id="343ff-106">You should also have either a personal Microsoft account with a mailbox on Outlook.com, or a Microsoft work or school account.</span></span> <span data-ttu-id="343ff-107">Wenn Sie kein Microsoft-Konto haben, gibt es mehrere Optionen, um ein kostenloses Konto zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="343ff-107">If you don't have a Microsoft account, there are a couple of options to get a free account:</span></span>

- <span data-ttu-id="343ff-108">Sie können [sich für ein neues persönliches Microsoft-Konto registrieren](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span><span class="sxs-lookup"><span data-stu-id="343ff-108">You can [sign up for a new personal Microsoft account](https://signup.live.com/signup?wa=wsignin1.0&rpsnv=12&ct=1454618383&rver=6.4.6456.0&wp=MBI_SSL_SHARED&wreply=https://mail.live.com/default.aspx&id=64855&cbcxt=mai&bk=1454618383&uiflavor=web&uaid=b213a65b4fdc484382b6622b3ecaa547&mkt=E-US&lc=1033&lic=1).</span></span>
- <span data-ttu-id="343ff-109">Sie können sich [für das Office 365 Entwicklerprogramm registrieren](https://developer.microsoft.com/office/dev-program) , um ein kostenloses Office 365-Abonnement zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="343ff-109">You can [sign up for the Office 365 Developer Program](https://developer.microsoft.com/office/dev-program) to get a free Office 365 subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="343ff-110">Dieses Lernprogramm wurde mit der openjdk-Version 14.0.0.36 und Gradle 6.7.1 geschrieben.</span><span class="sxs-lookup"><span data-stu-id="343ff-110">This tutorial was written with OpenJDK version 14.0.0.36 and Gradle 6.7.1.</span></span> <span data-ttu-id="343ff-111">Die Schritte in diesem Leitfaden funktionieren möglicherweise mit anderen Versionen, jedoch nicht getestet.</span><span class="sxs-lookup"><span data-stu-id="343ff-111">The steps in this guide may work with other versions, but that has not been tested.</span></span>

## <a name="feedback"></a><span data-ttu-id="343ff-112">Feedback</span><span class="sxs-lookup"><span data-stu-id="343ff-112">Feedback</span></span>

<span data-ttu-id="343ff-113">Geben Sie Feedback zu diesem Lernprogramm im [GitHub-Repository](https://github.com/microsoftgraph/msgraph-training-java)an.</span><span class="sxs-lookup"><span data-stu-id="343ff-113">Please provide any feedback on this tutorial in the [GitHub repository](https://github.com/microsoftgraph/msgraph-training-java).</span></span>
