<properties
    pageTitle="Azure pinon Web-sovellusten yleiskatsaus | Microsoft Azure"
    description="Azure Pinotut Web-sovellusten yleiskatsaus"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="azure-stack-web-apps-overview"></a>Yleistä Azure pinon Web Apps
    
> [AZURE.NOTE] Seuraavat tiedot koskevat vain Azure pinon TP1 ominaisuuksissa.

Azure pinon Web Apps-sovelluksista on toi Azure pinon Azure App palvelun tarjoaa ensimmäinen elementti. Azure pinon Web Apps-asennusohjelma luo esiintymän kunkin viisi tarvittavat rooli ja luo myös tiedostopalvelimessa. Vaikka voit lisätä esiintymiä erityyppisten rooli, muista, että ei ole paljon tilaa VMs Technical Preview-1. Nykyinen ominaisuuksia Azure pinon Web Apps-sovellukset on ensisijaisesti foundation-ominaisuuksia, joita tarvitaan järjestelmä- ja web-sovellusten hallinta.

![Azure pinon App palvelun Web Apps Azure-pino Portal][1]

## <a name="limitations-of-the-technical-preview"></a>Teknisen ennakkoversion rajoitukset

Azure pinon App palvelun preview-versiot ei tueta. Tuotannon työmääriä ei sijoita tähän preview-versio. On myös päivittäminen ei ole välillä Azure pinon App palvelun preview-versiot. Nämä preview-versiot ensisijainen tarkoitetaan ovat näyttää, mitä Microsoft tarjoaa ja saada palautetta. 

## <a name="what-is-an-app-service-plan"></a>Mikä sovelluksen-palvelun suunnittelu?

Azure-pino verkkosovelluksissa resurssin toimittaja käyttää samaa tunnus, jolla Azure App palvelussa Azure Web Apps-ominaisuus käyttää. Tämän vuoksi joitakin yleisiä käsitteitä on kuvaava. Web Apps-verkkosovelluksissa hinnoittelu säilö kutsutaan sovelluksen palvelusopimus. On erillinen näennäiskoneiden käyttää sovelluksia joukko. Tietyn tilauksen piiriin kuuluvien voi olla useita sovelluksen palvelusopimusten vaihtoehdot. Tämä koskee myös Azure pinon Web Apps-sovelluksissa. 

Azure-on jaettu ja erillinen työntekijöiden. Jaetun työntekijä tukee HD multitenant web app-kansio ja on vain yksi joukko jaetun työntekijöiden. Erillinen palvelinten vain yhdestä vuokraajasta käyttämät ja annettava kolme kokoa: pieni, Normaali tai suuri. Paikallisen käyttäjille ei voi aina ohjeiden avulla ehdot. Azure pinon verkkosovelluksissa resurssin palvelun järjestelmänvalvojat voivat määrittää ne haluat varata siten, että heillä on useita ehtojoukkoja ja jaetun työntekijöiden tai niiden yksilöllisen sivuston isännöintiä tarpeiden perusteella erillinen työntekijöiden erilaisia arvojoukkoja työntekijä tasoa. Käytä kyseiset työntekijä taso-määritykset, ne sitten määrittää omia hinnat tuotteissa.

## <a name="portal-features"></a>Portaalin ominaisuudet

On myös tosi kanssa uudelleen, Azure pinon Web Apps käyttää samaa Käyttöliittymä, joka käyttää Azure verkkosovelluksissa. Jotkin ominaisuudet eivät ole käytössä ja ei vielä ole toiminnassa Azure Pinotut. Tämä johtuu siitä Azure-kohtaisia odotuksia tai näiden ominaisuuksien käyttöön tarvitaan palvelut eivät ole vielä käytettävissä Azure Pinotut. 

## <a name="next-steps"></a>Seuraavat vaiheet

- [Ennen kuin alat Azure pinon Web Apps-sovellusten kanssa](azure-stack-webapps-before-you-get-started.md)
- [Web-sovellusten resurssin Providerin asentaminen](azure-stack-webapps-deploy.md)

Voit myös kokeilla muita [ympäristössä, kuten (PaaS) service-palvelujen](azure-stack-tools-paas-services.md), kuten [SQL Server resurssin tarjoajaan](azure-stack-sql-rp-deploy-short.md) ja [MySQL resurssin toimittaja](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-overview/AppService_Portal.png
