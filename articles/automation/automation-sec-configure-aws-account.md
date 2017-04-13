<properties
   pageTitle="Käyttöoikeuksien määrittäminen Amazon Web Services | Microsoft Azure"
   description="Tässä artikkelissa kuvataan, miten voit luoda ja vahvista Azure automaatio sisältyy AWS resurssien hallinta runbooks sisältyy AWS tunnistetiedot."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn"
   keywords="sisältyy AWS todennus sisältyy aws määrittäminen"/>
<tags
   ms.service="automation"
   ms.workload="tbd"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.date="09/12/2016"
   ms.author="magoedte"/>

# <a name="authenticate-runbooks-with-amazon-web-services"></a>Todennetaan Runbooks Amazon Web Services
Resurssit-Amazon Web Services (sisältyy AWS) yleisten tehtävien automatisointi voidaan toteuttaa automaatio runbooks Azure-tietokannassa.  Voit automatisoida useita tehtäviä sisältyy AWS automaatio runbooks samalla tavalla kuin Azure resurssien avulla.  Kaikki, jotka tarvitaan on kaksi asiaa:

* Tilauksen sisältyy AWS ja joukko tunnistetietoja.  Erityisesti sisältyy AWS pikanäppäin huolehtivat salausavaimen.  Lisätietoja Tarkista artikkelin [Sisältyy AWS tunnistetietojen avulla](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html).
* Azure-tilauksen ja automaatio-tili.  Lisätietoja Azure automaatio-tilin määrittämisestä on Tarkista artikkelin [Määritä Azure Suorita kuin tili](../automation/automation-sec-configure-azure-runas-account.md).  

Todentamismenetelmä sisältyy AWS, sinun on määritettävä joukko sisältyy AWS tunnistetiedot tarkistamiseen oman runbooks Azure automaatio suorittaminen. Jos sinulla on jo luotu automaatio-tili ja haluat käyttää, joka sisältyy AWS todentamismenetelmä, voit noudattaa seuraavan osan ohjeita.  Voit halutessasi tilin runbooks kohdentaminen sisältyy AWS resursseille varattu tulee Luo uuden [tilin automaatio Suorita nimellä](../automation/automation-sec-configure-azure-runas-account.md) (Ohita luontivaihtoehto palvelun lyhennys) ja noudata sitten seuraavia ohjeita.

## <a name="configure-automation-account"></a>Automaatio-tilin määrittäminen
Azure automatisointiin pitää yhteyttä sisältyy AWS ensin tarvitset noutaa sisältyy AWS tunnistetietosi ja tallentaa ne Azure automaatio kohteita.  Seuraavien toimien kuvattu sisältyy AWS asiakirjan [Sisältyy AWS tilin hallinta pikanäppäinten](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) pikanäppäimen luominen ja kopioiminen **Access avaimen tunnus** ja **Salaisuus pikanäppäin** (Lataa voit myös tallentaa sen turvallisessa avaimen tiedoston).

Kun olet luonut ja kopioida sisältyy AWS suojaus-näppäimiä, sinun on tunnistetiedon kohteiden luominen Azure automaatio-tilillä turvallisesti tallentaa ne ja viittaavat niiden oman runbooks kanssa.  **Luo uusi tunnistetiedon** osion [tunnistetiedon kohteita Azure automaatio](../automation/automation-certificates.md/###To create a new credential with the Azure portal) -artikkelin ohjeiden mukaisesti ja seuraavat tiedot:

1. Kirjoita **nimi** -ruutuun **AWScred** tai seuraavan nimeämiskäytännön standardeja sopiva arvo.  
2. Kirjoita **käyttäjänimi** -ruutuun omaan **Access-tunnus** ja **salasana** ja **Vahvista salasana** -ruutuun **Salaisuus pikanäppäin** .   

## <a name="next-steps"></a>Seuraavat vaiheet

- Reivew ratkaisu on artikkelissa luominen runbooks sisältyy AWS tehtävien automatisointi [Automating käyttöönoton AM Amazon Web Services-palveluissa](../automation/automation-scenario-aws-deployment.md) .
