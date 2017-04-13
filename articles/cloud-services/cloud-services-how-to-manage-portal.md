<properties 
    pageTitle="Yleisiä cloud-palvelun hallintatehtäviä | Microsoft Azure" 
    description="Opi hallitsemaan pilvipalveluihin Azure-portaalissa. Näissä esimerkeissä Azure portaalin." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/02/2016"
    ms.author="adegeo"/>


# <a name="how-to-manage-cloud-services"></a>Voit hallita pilvipalveluihin

> [AZURE.SELECTOR]
- [Azure portal](cloud-services-how-to-manage-portal.md)
- [Azure perinteinen portal](cloud-services-how-to-manage.md)

Pilvipalvelussa sijaitsevaan hallitaan Azure portaalin **Pilvipalveluihin (perinteinen)** -alueella. Tässä artikkelissa on joitakin yleisiä toimintoja luomisessa noudatetaan samalla kun hallitset cloud Services-palvelut. Joka sisältää päivittäminen, poistaminen, skaalaus ja tuotannon vaiheistettu käyttöönoton edistäminen.

Lisätietoja skaalata cloud-palvelu on käytettävissä [seuraavassa](cloud-services-how-to-scale-portal.md).

## <a name="how-to-update-a-cloud-service-role-or-deployment"></a>Toimintaohje: Päivitä cloud roolin tai käyttöönotto

Jos haluat päivittää sovelluksen koodi cloud-palvelusta, käytä cloud palvelun sivu **Päivitä** . Voit päivittää rooliin tai kaikki roolit. Jos haluat päivittää, voit ladata uuteen palvelupakettiin tai palvelun määritystiedostoa.

1. Valitse [Azure portal][]pilvipalvelussa, jonka haluat päivittää. Tämä vaihe avaa cloud palvelun esiintymä-sivu.

2. Valitse sivu, **Päivitä** -painiketta.

    ![Päivitä-painike](./media/cloud-services-how-to-manage-portal/update-button.png)

3. Päivitä käyttöönoton uusi palvelu-pakettitiedosto (.cspkg) ja palvelun kokoonpanotiedosto (.cscfg).

    ![UpdateDeployment](./media/cloud-services-how-to-manage-portal/update-blade.png)

4. **Voit myös** päivittää käyttöönotto-otsikko ja tallennustilaa-tili. 

5. Jos mitään rooleja on vain yksi rooli esiintymän, valitse **käyttöön, vaikka useita rooleja sisältävät yhden esiintymän** käyttöön Jatka päivitys. 

    Azure takaa vain 99.95 prosenttia palvelun saatavuus cloud päivittämisessä jos kullakin roolilla on vähintään kaksi roolin esiintymät (näennäiskoneiden). Kaksi roolin esiintymät yhden virtual machine käsitellä asiakkaiden pyynnöt samalla, kun toinen päivitetään.

6. Valitse **Aloita käyttöönotto** päivitys voimaan, kun paketin lataaminen on valmis.

7. Valitse Aloita päivitys palvelun **OK** .



## <a name="how-to-swap-deployments-to-promote-a-staged-deployment-to-production"></a>Toimintaohje: Vaihda ominaisuuksissa edistää tuotannon vaiheistettu käyttöönotto

Kun päätät käyttöönotto pilvipalveluun, vaihe uuden version ja testaa uudessa versiossa cloud palvelun väliaikaisen ympäristössä. **Vaihda** avulla voit siirtyä URL-osoitteet, jolla kaksi ominaisuuksissa on osoitettu ja edistää uusien tuotannon. 

Voit muuttaa ominaisuuksissa **Cloud Services** -sivulta tai koontinäytön.

1. Valitse [Azure portal][]pilvipalvelussa, jonka haluat päivittää. Tämä vaihe avaa cloud palvelun esiintymä-sivu.

2. Napsauta sivu- **Vaihda** -painiketta.

    ![Cloud Services vaihtaminen](./media/cloud-services-how-to-manage-portal/swap-button.png)

3. Seuraavat vahvistusviesti tulee näkyviin.

    ![Cloud Services vaihtaminen](./media/cloud-services-how-to-manage-portal/swap-prompt.png)

4. Kun olet varmistanut tietoja käyttöönotosta, valitsemalla Vaihda käyttöönotoissa **OK** .

    Vaihda käyttöönoton kauaa koska riittää, joka muuttuu virtual IP-osoitteet (VIPs) käyttöönotoissa.

    Tallenna Laske kustannukset, voit poistaa väliaikaisesta käyttöönoton sen jälkeen, kun olet varmistanut, että tuotannon käyttöönoton toimii odotetulla tavalla.

## <a name="how-to-link-a-resource-to-a-cloud-service"></a>Toimintaohje: Linkitä resurssin pilvipalveluun

Azure portaalin Linkitä resurssien yhdessä, esimerkiksi nykyisen Azure perinteinen portaalin onko. Lisäresursseja käyttöön sen sijaan saman resurssiryhmän Cloud-palvelu käytössä.

## <a name="how-to-delete-deployments-and-a-cloud-service"></a>Toimintaohje: Poista ominaisuuksissa ja pilvipalveluun

Ennen kuin voit poistaa pilvipalveluun, sinun on poistettava kunkin luodun käyttöönotto.

Tallenna Laske kustannukset, voit poistaa väliaikaisesta käyttöönoton sen jälkeen, kun olet varmistanut, että tuotannon käyttöönoton toimii odotetulla tavalla. Laske kustannusten käyttöön roolin esiintymien, joka on pysäytetty on laskutettu.

Seuraavien ohjeiden avulla voit poistaa käyttöönoton tai pilvipalvelussa sijaitsevaan. 

1. Valitse [Azure portal][]pilvipalvelussa, jonka haluat poistaa. Tämä vaihe avaa cloud palvelun esiintymä-sivu.

2. Valitse sivu, **Poista** -painiketta.

    ![Cloud Services vaihtaminen](./media/cloud-services-how-to-manage-portal/delete-button.png)

3. Voit poistaa koko pilvipalvelussa **pilvipalvelussa ja sen käyttöönoton** valitsemalla tai valitse **tuotannon käyttöönoton** tai **väliaikaisen käyttöönotto**.

    ![Cloud Services vaihtaminen](./media/cloud-services-how-to-manage-portal/delete-blade.png) 

4. Napsauta alareunassa **Poista** -painiketta.

5. Voit poistaa pilvipalvelussa valitsemalla **Poista pilvipalvelussa**. Kun vahvistusviesti tulee näkyviin, valitse **Kyllä**.

> [AZURE.NOTE]
> Kun pilvipalveluun poistetaan ja yksityiskohtainen seuranta on määritetty, sinun on poistettava tiedot manuaalisesti tallennustilan-tililtä. Lisätietoja löydät arvot taulukot on [Tässä](cloud-services-how-to-monitor.md) artikkelissa.

[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Seuraavat vaiheet

* [Yleisten määritysten cloud-palvelun](cloud-services-how-to-configure-portal.md).
* Lisätietoja käyttöönottamisesta [pilvipalveluun](cloud-services-how-to-create-deploy-portal.md).
* Määritä [mukautettua toimialuenimeä](cloud-services-custom-domain-name-portal.md).
* Määritä [ssl-varmenteita](cloud-services-configure-ssl-certificate-portal.md).