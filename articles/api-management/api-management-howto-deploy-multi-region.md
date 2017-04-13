<properties
    pageTitle="Azure API Management-palvelun esiintymän ottamisesta käyttöön useita Azure alueita"
    description="Opi ottamaan Azure API Management-palvelun esiintymän Azure alueille." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Azure API Management-palvelun esiintymän ottamisesta käyttöön useita Azure alueita

API hallinta tukee monille käyttöönoton, joka mahdollistaa API julkaisijat haluat jakaa yhden API hallintapalvelun jokin muu luku haluamasi Azure alueiden välillä. Tämä auttaa vähentämään pyynnön viive, jonka maantieteellisesti jaettu Ohjelmointirajapinnan kuluttajille ja parantaa palvelun saatavuus myös, jos jonkin alueen menee offline-tilaan. 

API hallintapalvelun luomisen alun perin se sisältää vain yhden [yksikön][] ja sijaitsee yhden Azure alue, joka on määritetty ensisijainen alue. Lisää alueita voi helposti lisätä Azure perinteinen Portal. API Management Gatewayn server otetaan käyttöön kunkin alueen ja puhelun liikenne reititetään lähinnä yhdyskäytävän. Jos alueen menee offline-tilaan, liikenne on automaattisesti uudelleen ohjattu seuraavaan lähinnä yhdyskäytävään. 

> [AZURE.IMPORTANT] Monille käyttöönoton on käytettävissä vain **[Premium][]** taso.

## <a name="add-region"> </a>API Management-palvelun esiintymän käyttöön uuden alueen

Aloita valitsemalla Azure perinteinen portaalissa API Management-palvelun **hallinta** . Tämä avaa API hallinta publisher-portaalin.

![Publisher-portaalissa][api-management-management-console]

>Jos et ole luonut erillisen service API hallinta-kohdassa [Azure API hallinnan käytön aloittaminen][] -opetusohjelma [Luo erillisen service API hallinta][] .

Siirry API Management-palvelun esiintymän Azure perinteinen Portal **Mittakaava** -välilehti. 

![Mittakaava-välilehti][api-management-scale-service]

Voit ottaa käyttöön uuden alueen, valitse avattavan luettelon alapuolella ensisijainen alue ja valitse luettelosta alue.

![Lisää alue][api-management-add-region]

Kun haluamasi alue on valittuna, valitse uusi alue yksikkö avattavasta luettelosta yksiköiden määrän.

![Mittayksiköt][api-management-select-units]

Kun haluamasi alueet ja yksiköt on määritetty, valitse **Tallenna**.

## <a name="remove-region"> </a>API Management-palvelun esiintymän poistaminen alue

Jos haluat poistaa API Management-palvelun esiintymän alueen, siirry API Management-palvelun esiintymän Azure perinteinen Portal **Mittakaava** -välilehti. 

![Mittakaava-välilehti][api-management-scale-service]

Valitse Poista halutun alueen oikealla puolella **X** .  

![Poista alue][api-management-remove-region]

Kun haluamasi alueet on poistettu, valitse **Tallenna**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Luo erillisen service API hallinta]: api-management-get-started.md#create-service-instance
[Azure API hallinnan käytön aloittaminen]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[yksikkö]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

