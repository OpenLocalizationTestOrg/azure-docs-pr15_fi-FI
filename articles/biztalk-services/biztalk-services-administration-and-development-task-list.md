<properties
    pageTitle="Hallinta- ja tehtäväluettelo BizTalk Services-palveluissa | Microsoft Azure"
    description="Suunnittelu ja työn tuki käyttöönotto Azure BizTalk palvelut."
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="administration-and-development-task-list-in-biztalk-services"></a>Hallinta ja kehitys tehtäväluettelon BizTalk Services-palveluissa  

## <a name="getting-started"></a>Käytön aloittaminen
Kun käsittelet Microsoft Azure BizTalk Services, käytettävissä on useita paikallisen ja pilvipohjainen osien huomioitavia. Aloita, ota huomioon seuraavat prosessinkulku:  

|Vaihe|Kuka vastaa|Tehtävä|Aiheeseen liittyvät linkit|
|----|----|----|----|
|1.|Järjestelmänvalvoja|Microsoft Azure-tilauksen, Microsoft-tili tai organisaation tilillä luominen|[Azure perinteinen portal](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
|2.|Järjestelmänvalvoja|Luo tai valmistella BizTalk palvelu.|[Luo BizTalk palvelu Azure perinteinen-portaalissa](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
|3.|Järjestelmänvalvoja|Rekisteröi sinä tai yrityksen BizTalk Services-palvelujen käyttöönottaminen|[Rekisteröiminen ja päivittäminen BizTalk palvelun käyttöönoton BizTalk palvelut-portaalissa](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
|4.|Järjestelmänvalvoja|Jos sovellus käyttää BizTalk verkkosovittimen palvelun muodostaa paikalliseen järjestelmään, liiketoiminta-(LOB) tai käyttää jonossa tai aiheen kohde.  Luo Azure palvelun Bus Namespace. Antaa tämän nimitilan, palvelun Bus myöntäjän nimi ja palvelun Bus myöntäjän avaimen arvot kehittäjä.|[Toimintaohje: Voit luoda tai muokata palvelun Bus Service-Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) ja [Hae myöntäjän nimi ja myöntäjän avaimen arvot](biztalk-issuer-name-issuer-key.md)|
|5.|Developer|Asenna SDK ja luo BizTalk Service-projekti Visual Studiossa.|[Asenna Azure BizTalk Services SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) ja [luoda monipuolisia Azure tekstiviesti päätepisteet](https://msdn.microsoft.com/library/azure/hh689766.aspx)|
|6.|Developer|Ottaa käyttöön projektin BizTalk-palveluun isännöimät Azure BizTalk-palvelussa.|[Käyttöönotto ja päivittäminen BizTalk Services-projekti](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
|7.|Järjestelmänvalvoja|Ottaa käyttöön, jos käytössäsi on Muokkaa.  Voit lisätä kumppanit ja toimeenpano luominen Microsoft Azure BizTalk palvelut-portaalissa. Kun luot sopimuksen, voit lisätä silta ja/tai muunnoksia luotu kehittäjä sopimus-asetukset.|[Muokkaa AS2 ja EDIFACT määrittäminen BizTalk palvelut-portaalissa](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
|8.|Järjestelmänvalvoja|Käytä Azure perinteinen portaalin, tarkkailla BizTalk-palvelussa, mukaan lukien suorituskyvyn mittarit.|[BizTalk Services: Raporttinäkymät-ikkunan, näyttö ja skaalaus-välilehdet](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
|9.|Järjestelmänvalvoja|Käyttämällä Microsoft Azure BizTalk palvelut-portaalin hallinta palvelutiedot käyttämä BizTalk palvelut ja seuraa viestejä, kun niitä käsitellään silta-tiedostoista.|[BizTalk palvelut-portaalissa](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
|10.|Järjestelmänvalvoja|Luo varmuuskopio aiot varmuuskopioida BizTalk-palvelun.|[Liiketoiminnan jatkuvuus ja palauttaminen BizTalk Services-palveluissa](https://msdn.microsoft.com/library/azure/dn509557.aspx) |  
## <a name="next-steps"></a>Seuraavat vaiheet
[Opetusohjelmat ja mallit](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Projektin luominen Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Asenna Azure BizTalk Services SDK: ssa](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Käsitteitä
[Projektin luominen Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[Muokkaa AS2 ja EDIFACT Messaging (Business Business)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  
## <a name="other-resources"></a>Muut resurssit  
[Lisää lähde, kohde ja Messaging päätepisteet silta](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Lue ja Luo viesti kartat ja muunnokset](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[BizTalk-sovittimen (BAS)-palvelun avulla](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk palvelut](http://go.microsoft.com/fwlink/p/?LinkID=303664)
