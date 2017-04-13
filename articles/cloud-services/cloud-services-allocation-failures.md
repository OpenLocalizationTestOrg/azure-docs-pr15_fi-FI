<properties
    pageTitle="Vianmääritys pilvipalvelussa kohdistus virhe | Microsoft Azure"
    description="Vianmääritys kohdistus virheen asentaessasi pilvipalveluihin Azure-tietokannassa"
    services="azure-service-management, cloud-services"
    documentationCenter=""
    authors="simonxjx"
    manager="felixwu"
    editor=""
    tags="top-support-issue"/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="v-six"/>



# <a name="troubleshooting-allocation-failure-when-you-deploy-cloud-services-in-azure"></a>Vianmääritys kohdistus-virhe, kun otat käyttöön pilvipalveluihin Azure-tietokannassa

## <a name="summary"></a>Yhteenveto
Kun käyttöönotto esiintymät pilvipalveluun tai lisätä uuden sivuston tai työntekijän rooli esiintymät, Microsoft Azure varaa resursseja suorittaminen. Näyttöön voi tulla virheitä toisinaan suoritettaessa näitä toimintoja, myös ennen Azure tilauksen rajoituksiin pääsyä. Tässä artikkelissa kerrotaan joistakin yleisiä kohdistus virheiden syitä, ja ehdottaa mahdollista korjaus. Tietoja myös voi olla hyötyä palvelujen käyttöönottoa.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

### <a name="background--how-allocation-works"></a>Taustan – kohdistus toiminta
Azure palvelinkeskusten palvelimissa on osioitu klustereihin. Uusi cloud kohdistus palvelupyyntö yritetään useita klustereissa. Kun ensimmäisen esiintymän otetaan käyttöön pilvipalvelussa (valitse joko väliaikaisen tai tuotannon), joka cloud palvelun saa kiinnitetty klusteriin. Mitä tahansa muita ominaisuuksissa cloud-palvelun tapahtuu samassa klusterissa. Tässä artikkelissa viitataan tämä kuin "kiinnitetty klusterin". Alle 1 kaaviossa on kuvattu Normaali kohdistus on liikaa useita klustereissa, joka tapauksessa 2 kaaviossa on kuvattu kohdistuksen kirjainkokoa, joka on kiinnitetty klusterin 2, koska se on missä aiemmin Cloud-palvelun CS_1 isännöidään.

![Kohdistus-kaavio](./media/cloud-services-allocation-failure/Allocation1.png)

### <a name="why-allocation-failure-happens"></a>Miksi kohdistus virhe tapahtuu
Kun kohdistuspyyntö on kiinnitetty klusteriin, käytössäsi on suurempi mahdollisuutta etsiä maksuttomia resursseja, koska käytettävissä resurssivarannon on rajoitettu klusterin kaatuvat. Lisäksi jos kohdistuspyyntö on kiinnitetty klusteriin, mutta voit pyytää resurssin lajin ei tue kyseistä klusterin, pyyntö epäonnistuu vaikka klusterin on ilmainen resurssi. Alla 3 kaaviossa on kuvattu missä kiinnitettyjen perustaso epäonnistuu, koska vain candidate-klusterin ei ole vapaa resursseja kirjainkokoa. 4 kaaviossa on kuvattu missä kiinnitettyjen perustaso epäonnistuu, koska vain candidate-klusterin ei tue AM koko on pyydetty, vaikka klusterin on ilmainen resurssien kirjainkokoa.

![Kiinnitetty kohdistus-virhe](./media/cloud-services-allocation-failure/Allocation2.png)

## <a name="troubleshooting-allocation-failure-for-cloud-services"></a>Kohdistus epäonnistumisesta pilvipalveluihin vianmääritys
### <a name="error-message"></a>Virhesanoma
Voit saada seuraavan virhesanoman:

    "Azure operation '{operation id}' failed with code Compute.ConstrainedAllocationFailed. Details: Allocation failed; unable to satisfy constraints in request. The requested new service deployment is bound to an Affinity Group, or it targets a Virtual Network, or there is an existing deployment under this hosted service. Any of these conditions constrains the new deployment to specific Azure resources. Please retry later or try reducing the VM size or number of role instances. Alternatively, if possible, remove the aforementioned constraints or try deploying to a different region."

### <a name="common-issues"></a>Yleisiä ongelmia
Seuraavassa on yleisiä kohdistus-tilanteita, joissa aiheuttaa kohdistuksen-pyyntö, voit kiinnittää yhden klusterin.

- Käyttöönotto väliaikaisen paikka -, jos pilvipalveluun on käyttöönoton joko paikka, valitse koko pilvipalvelussa on kiinnitetty tiettyyn klusteri.  Tämä tarkoittaa, että jos käyttöönoton jo olemassa tuotannon paikka, sitten uusi väliaikaisen käyttöönoton vain voidaan varata samalla klusterin kuin tuotannon paikka. Jos klusterin on lähestyvä kapasiteettia, pyyntö saattaa epäonnistua.

- Skaalaus - uusia esiintymiä lisääminen aiemmin luotuun pilvipalvelussa on varata samalla klusterin.  Pieni skaalaus pyynnöt yleensä voi varata, mutta eivät aina. Jos klusterin on lähestyvä kapasiteetista, pyyntö saattaa epäonnistua.

- Affiniteetti ryhmä - uuden tyhjän pilvipalveluun käyttöympäristön voidaan jakaa-klusterin alueella olevat kangasta ellei käytössäsi on kiinnitetty affiniteetti ryhmään. Saman klusterin yritetään ominaisuuksissa samaan affiniteetti ryhmään. Jos klusterin on lähestyvä kapasiteetista, pyyntö saattaa epäonnistua.

- Affiniteetti ryhmän vNet - vanhempia Virtual verkot on sidottu affiniteetti ryhmiin sen sijaan, että alueet ja cloud Services-palvelujen Virtual verkostojen kiinnittää affiniteetti ryhmän klusterin. Tämäntyyppisen VPN ominaisuuksissa yritetään kiinnitetty klusterin. Jos klusterin on lähestyvä kapasiteettia, pyyntö saattaa epäonnistua.

## <a name="solutions"></a>Ratkaisut

1. Ota uusi pilvipalveluun uudelleen – Tämä ratkaisu on todennäköisesti onnistuisi parhaiten, kun se sallii alueen kaikki varausyksiköt valittavana ympäristö.

    - Ottaa käyttöön työmäärää uusi pilvipalveluun  

    - Päivitä CNAME-TIETUE tai tietueen liikenne osoittamaan uusi pilvipalvelussa

    - Kun nolla liikenne suorittaminen vanhan sivuston, voit poistaa vanhan pilvipalvelussa. Tämä ratkaisu olisi maksamaan nolla käyttökatkot.

2. Poista tuotannon ja väliaikaisen paikkojen - ratkaisun säilyttää aiemmin DNS-nimeä, mutta aiheuttaa käyttökatkot sovelluksen.

    - Poista tuotannon ja väliaikaisen aiemmin pilvipalvelussa paikkojen niin, että pilvipalvelussa on tyhjä, ja valitse sitten

    - Luo uusi käyttöönotto aiemmin pilvipalvelussa. Tämä yrittää uudelleen kaikki klustereiden alueen kohdistus. Varmista käytössäsi ei ole sidottu affiniteetti ryhmään.

3. Varattu IP - ratkaisun säilyttää aiemmin IP verkko-osoitteissa, mutta aiheuttaa käyttökatkot sovelluksen.  

    - Luo ReservedIP varten luodun käyttöönoton PowerShellin avulla

    ```
    New-AzureReservedIP -ReservedIPName {new reserved IP name} -Location {location} -ServiceName {existing service name}
    ```

    - Noudata #2 yläpuolelta varmistetaan, että voit määrittää uuden ReservedIP palvelun CSCFG.

4. Poista affiniteetti ryhmän uusien versioiden - affiniteetti ryhmille ei enää suositella. Noudattamalla edellä ottaa käyttöön uuden pilvipalvelussa #1 ohjeita. Varmista pilvipalvelussa ei ole affiniteetti-ryhmässä.

5. Muuntaa alueellisen näennäinen verkko - Katso [siirtäminen affiniteetti ryhmistä alueellisen näennäinen verkkoon (VNet)](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
