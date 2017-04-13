<properties
    pageTitle="Miten ajoitettu ilmoitukset lähetetään | Microsoft Azure"
    description="Tässä ohjeaiheessa kerrotaan ajoitettu ilmoitusten käyttäminen Azure ilmoituksen keskittimet."
    services="notification-hubs"
    documentationCenter=".net"
    keywords="Push-ilmoitukset, push-ilmoitus, ajoituksen push-ilmoitukset"
    authors="ysxu"
    manager="erikre"
    editor=""/>
<tags
    ms.service="notification-hubs"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="yuaxu"/>

# <a name="how-to-send-scheduled-notifications"></a>Toimintaohje: Ajoitetun ilmoitukset lähetetään


##<a name="overview"></a>Yleiskatsaus

Jos sinulla on tilanne, jossa haluat lähettää ilmoituksen joskus tulevaisuudessa, mutta ei ole helppo tapa herättää taustatietokantaan-koodin lähetetään ilmoitus. Vakio taso ilmoituksen keskittimet tukee ominaisuutta, jonka avulla voit ajoittaa ilmoitusten määrittäminen 7 päivää myöhemmän.

Ilmoituksen lähetettäessä yksinkertaisesti käyttää [ScheduledNotification](https://msdn.microsoft.com/library/microsoft.azure.notificationhubs.schedulednotification.aspx) luokan ilmoituksen keskittimet SDK seuraavan esimerkin mukaisesti:

    Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
    var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));

Voit myös peruuttaa aiemmin on ajoitettu ilmoituksen sen notificationId avulla:

    await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);

Ei rajoja ajoitetun lähetettävien ilmoitusten määrän.