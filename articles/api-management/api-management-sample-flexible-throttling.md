<properties
    pageTitle="Lisäasetukset pyynnön Azure API hallinnan rajoittaminen"
    description="Lue, miten luomiseen ja käyttämiseen joustavia kiintiön ja korko rajoittaminen käytännöt Azure API hallinnan."
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>


# <a name="advanced-request-throttling-with-azure-api-management"></a>Lisäasetukset pyynnön Azure API hallinnan rajoittaminen

Ei voi rajoita pyynnöt on avaimen rooli Azure API hallinta. Joko ohjaamalla suuruuden pyynnöt tai pyynnöt ja tietojen yhteensä siirretty API hallinnan avulla API tarjoajat suojaamaan niiden ohjelmointirajapinnan väärinkäyttö ja luoda eri API tuotteen tasojen arvo.

## <a name="product-based-throttling"></a>Tuotteen rajoittaminen
Päivämäärä, korko rajoittimen ominaisuuksia on on rajoitettu parhaillaan suodatetut tuotteen tilauksessa (käytettävä avain), määritetään API hallinta publisher-portaalissa. Tästä on hyötyä koskevat rajoitukset, joka on kirjautunut käyttämään niiden API kehittäjät API Internet-palveluntarjoajaan, mutta se ei auta, esimerkiksi rajoittimen yksittäisiä käyttäjiä API. Se on mahdollista, että yhden käyttäjän tarjoaman koko kiintiön ja estää muiden asiakkaiden kehittäjä ei voi käyttää sovelluksen kehittäjän sovelluksen. Myös useita asiakkaat, joilla voi luoda pyynnöt määrää voi rajoittaa ajoittaiset käyttäjien pääsyn.

## <a name="custom-key-based-throttling"></a>Mukautetun avaimen mukaan rajoittaminen
Uudet [korko-raja-mukaan-avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kiintiön avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) käytännöt tarjota merkittävästi joustavammat ratkaisun liikenne ohjausobjektiin. Uusi käytännöt avulla voit määrittää lausekkeiden tunnistavan näppäimet, joita käytetään liikenne seurantaa. Tämä toimii tapaa, jolla on esitelty easiest ja esimerkki. 

## <a name="ip-address-throttling"></a>IP-osoite rajoittaminen
Käytettävissään seuraavat käytännöt rajoittaa yksittäisen asiakkaan IP-osoite puhelut vain 10 minuutin välein 1,000,000 puhelut korkeintaan ja kaistanleveyden kuukaudessa 10 000 kilotavua. 

    <rate-limit-by-key  calls="10"
              renewal-period="60"
              counter-key="@(context.Request.IpAddress)" />

    <quota-by-key calls="1000000"
              bandwidth="10000"
              renewal-period="2629800"
              counter-key="@(context.Request.IpAddress)" />

Jos kaikki asiakkaat Internetissä yksilöllinen IP-osoite, tämä saattaa olla tehokas tapa rajoittaminen käyttäjän käyttö. On kuitenkin todennäköisesti useita käyttäjiä on yksittäinen julkiseen IP-osoite vuoksi ne käyttäminen Internetin kautta NAT-laitteen jakaminen. Huolimatta tämän lomakkeen API, jotka sallivat Todentamattomille access `IpAddress` voi olla paras vaihtoehto.

## <a name="user-identity-throttling"></a>Käyttäjän tunnistetiedot rajoittaminen
Jos käyttäjä todennetaan sitten rajoittava näppäintä voidaan luoda tietoja, joka yksilöi, joka perustuu käyttäjän.

    <rate-limit-by-key calls="10"
        renewal-period="60"
        counter-key="@(context.Request.Headers.GetValueOrDefault("Authorization","").AsJwt()?.Subject)" />

Tässä esimerkissä on Pura Authorization-otsikko, muunna `JWT` objekti ja määritä käyttäjälle ja käyttää niitä rajoittaminen avain korko tunnuksen aihe avulla. Jos käyttäjätietoja on tallennettu `JWT` , kun jokin toinen väittää sitten arvon voi käyttää sijaintia.

## <a name="combined-policies"></a>Yhdistetyn käytännöt
Vaikka uusia rajoittava käytäntöjä on enemmän kuin aiemmin rajoittava käytännöt, on edelleen arvo yhdistäminen sekä ominaisuuksia. Rajoittaminen tuotteen tilauksen avaimella ([raja puhelun korko tilauksen](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja [Määritä käyttökiintiö tilauksen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota)) on erinomainen tapa ottaa käyttöön monetizing API soveltamalla käyttöoikeustasot perusteella. Finer hakuindeksointi voidaan suojata valvonnasta, että voit rajoita käyttäjä on komplementtivirhefunktion ja estää yhden käyttäjän toiminnan heikennä toisen kokemus. 

## <a name="client-driven-throttling"></a>Asiakkaan perustuva rajoittaminen
Kun rajoittava avain on määritetty käyttämällä [käytännön lauseketta](https://msdn.microsoft.com/library/azure/dn910913.aspx), se on API-palvelu, joka on valitsemalla miten rajoitus on määritetty. Kehittäjä voi kuitenkin haluta määrittää, miten ne korko raja omia asiakkaille. Tämä voi ottaa käyttöön API-palvelun tuomalla mukautetun otsikon sallimaan kehittäjän asiakassovellus viestintä ohjelmointirajapinnan avain.

    <rate-limit-by-key calls="100"
              renewal-period="60"
              counter-key="@(request.Headers.GetValueOrDefault("Rate-Key",""))"/>

Näin voit valita, kuinka he haluavat luoda rajoittaminen avain korko kehittäjän asiakassovellukseen. Hieman ingenuity asiakkaan kehittäjä voi luoda omia korko tasoa näppäimet tietojoukkoa kohdistaminen käyttäjät ja kiertämällä avaimen käyttö.

## <a name="summary"></a>Yhteenveto
Azure API hallinta antaa korko ja tarjouksen rajoitusten sekä suojaaminen ja Lisää arvo API-palveluun. Mukautetun tarkasteltavan sääntöjen uudet rajoittava käytännöt mahdollistavat finer hakuindeksointi voidaan suojata hallintaoikeutta kyseiset käytännöt käyttöön asiakkaita näkyisivät paremmin sovelluksia. Tämän artikkelin esimerkkejä kuvaavat uusi käytännöt tarkoitus nopeuden rajoittaminen näppäimet asiakkaan IP-osoitteet, käyttäjätiedot ja asiakkaan luotu arvojen mukaan. On kuitenkin monia muita, joita voi käyttää esimerkiksi käyttäjäagentti URL-polku osat, viestin koko viestin osia.

## <a name="next-steps"></a>Seuraavat vaiheet
Anna meille palautetta Disqus keskusteluketjun tämän aiheen. Olisi hienoa kuulet muut mahdolliset avainarvot, joka on looginen vaihtoehto oman tilanteissa.

## <a name="watch-a-video-overview-of-these-policies"></a>Katso video yleiskatsaus käytännöt
Lisätietoja tämän artikkelin [korko-raja-mukaan-avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kiintiön avain](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) käytäntöjä Katso seuraava video.

> [AZURE.VIDEO advanced-request-throttling-with-azure-api-management]
