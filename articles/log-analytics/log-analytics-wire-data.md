<properties
    pageTitle="Johdon tietoratkaisun lokin Analytics | Microsoft Azure"
    description="Wire tiedot kootut tietokoneissa, joissa OMS agenttien vuoksi, mukaan lukien Operations Manager ja Windows-yhteys tekijöiden verkon ja suorituskyvyn tiedot. Verkon tiedot yhdistetään log tietoluettelon avulla voit yhdistää tietoja."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Johdon tietoratkaisun lokin Analytics

Wire tiedot kootut tietokoneissa, joissa OMS agenttien vuoksi, mukaan lukien Operations Manager ja Windows-yhteys tekijöiden verkon ja suorituskyvyn tiedot. Verkon tiedot yhdistetään log tietoluettelon avulla voit yhdistää tietoja. OMS agenttien vuoksi asennettu tietokoneisiin IT infrastruktuurin näytön verkostosi tiedot lähetetään verkon tasoissa 2-3 [verkkosegmenttiä](https://en.wikipedia.org/wiki/OSI_model) , mukaan lukien eri protokollat ja portit, joita käytetään ja sieltä pois näistä tietokoneista.

>[AZURE.NOTE] Wire tietoratkaisun ei ole tällä hetkellä käytettävissä työtilat lisättäväksi. Asiakkaat, joilla on jo käytössä Wire tietoratkaisun edelleen käyttää Wire tietoratkaisun.

Oletusarvon mukaan OMS kerää kirjaustiedot suorittimen, muistin, levyn ja verkon suorituskykytietoja Windowsin laskureita. Verkko- ja muiden tietojen kerääminen tapahtuu reaaliaikainen kunkin agentti, mukaan lukien aliverkosta ja sovellustason protokollat tietokoneen käyttämän varten. Voit lisätä muita suorituskykylaskureita lokit-välilehden asetukset-sivulla.

Jos olet käyttänyt [sFlow](http://www.sflow.org/) tai muiden ohjelmistojen [Cisco's NetFlow-protokollan](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)kanssa, Tilasto- ja tietoja, näkyviin tulee wire tiedoista olla sinulle tuttuja.

Valmiin Log haun kyselytyypeistä ovat esimerkiksi seuraavat:

- Tekijöiden wire tietoja tuovien
- Tekijöiden wire tietoja IP-osoite
- Lähtevän tietoliikenteen IP-osoitteet
- Sovellusten protokollat lähettämien tavujen määrä
- Sovelluksen-palvelun lähettämien tavujen määrä
- Eri protokollat vastaanottanut tavua
- Tavujen lähetetään ja vastaanotetaan IP-osoitteita
- IP-osoitteet, jotka ovat tehneet 10.0.0.0/8 aliverkon tekijöiden kanssa
- Keskimääräinen odotusaika yhteyksissä, joissa on luotettavasti
- Tietokoneen prosesseja, joita aloittanut tai vastaanotettu verkkoliikennettä
- Prosessin verkkoliikennettä määrä

Kun haet wire tietojen käytön, voit suodattaa ja ryhmitellä tietoja, voit tarkastella tietoja yläreunan tekijöiden ja yläreunan protokollia. Tai kyselyjä, kun tarkastelet tiettyjen tietokoneiden (IP-osoitteet ja MAC-osoitteet) tiedoksi toisiinsa, kuinka kauan ja tietojen lähetettiin--lähinnä, voit tarkastella verkkoliikennettä, joka on Hakupohjaisten metatietoa.

Kuitenkin jälkeen, kun tarkastelet metatietoja, se ei ole välttämättä hyödyllinen perusteellisempaa vianmäärityksessä. Wire tietojen OMS ei ole täydellinen siepata verkkotietojen. Näin se ei ole tarkoitettu kattavaa paketin tason vianmääritystä varten.
Käyttämällä agentti verrattuna muiden sivustokokoelman kautta, etuna on se, että ei tarvitse laitteiden asentaminen ja määrittäminen uudelleen verkon valitsimet preform monimutkaisia määrityksiä. Wire tiedot ovat vain agent-pohjainen--agentti asentaminen tietokoneeseen ja se valvoo omassa verkkoliikennettä. Toinen etu on, kun haluat seurata käynnissä cloud tarjoajat tai palvelu-isännöintipalvelu tai Microsoft Azure, johon käyttäjä ei ole kangasta kerros toiminnoista.

Sen sijaan sinulla ei ole valmis näkyvyyden mitä tapahtuu verkossa, jos et asenna agenttien vuoksi kaikissa tietokoneissa-verkkoinfrastruktuuria.

## <a name="installing-and-configuring-the-solution"></a>Asentaminen ja määrittäminen ratkaisu
Seuraavat tiedot avulla voit asentaa ja määrittää ratkaisu.

- Wire tietoratkaisun hankkii tietojen tietokoneista, Windows Server 2012 R2-, Windows 8.1- ja uudemmissa käyttöjärjestelmissä.
- Mihin hankkia wire tietojen tietokoneissa edellyttää Microsoft .NET Framework 4.0 tai uudempi.
- Lisää Wire tietoratkaisun OMS työtilan käyttämisestä [ratkaisuvalikoimasta Lisää Log Analytics-ratkaisuja](log-analytics-add-solutions.md)kuvatut toimet.  Ei ole tarvitaan lisämäärityksiä.
- Jos haluat tarkastella tietyn ratkaisu wire tiedot, sinun on oltava lisättyjen OMS työtilan ratkaisu.

## <a name="wire-data-data-collection-details"></a>Tietoja sivustokokoelman tietojen kulmien

Wire tietojen kerää verkkoliikennettä käyttämällä agenttien vuoksi, jotka on otettu käyttöön liittyviä metatietoja.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmät ja muita tietoja siitä, miten tiedot kerätään Wire tiedot.


| käyttöympäristö | Suora agentti | SCOM agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 tai uudempi)|![Kyllä](./media/log-analytics-wire-data/oms-bullet-green.png)|![Kyllä](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)|![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)| 1 minuutin välein|


## <a name="combining-wire-data-with-other-solution-data"></a>Yhdistäminen wire tietoja muiden ratkaisun tietojen kanssa

Yllä valmiiden kyselyiden palauttamia tietoja voi olla kiinnostavat yksinään. Wire tietojen käyttökelpoisuus on kuitenkin toteutunut kun yhdistät OMS ratkaisuja tiedoilla. Voit esimerkiksi käyttää suojaus ja valvonta-ratkaisun keräämät suojauksen tapahtumatiedot ja yhdistää wire tietojen etsimään epätavallisia verkon kirjautuminen yritykset nimetty kannalta.  Tässä esimerkissä käytetään IN ja DISTINCT-operaattoreita liittymään arvopisteiden hakukysely.

Vaatimukset: Voi käyttää seuraavassa esimerkissä, sinun on oltava asennettuna suojaus ja valvonta-ratkaisun. Voit kuitenkin wire tietoja samankaltaiset tulokset yhdistää tietoja ratkaisuja.

### <a name="to-combine-wire-data-with-security-events"></a>Wire tiedot yhdistetään ja suojauksen tapahtumat

1. Valitse sivulla Napsauta **WireData** -ruutua.
2. Valitse **Yleiset WireData kyselyt**-luettelosta **Summa, verkkoliikennettä (tavuina) prosessin** palautetut prosessien luettelossa.
    ![wiredata kyselyt](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Jos prosessien luettelossa on liian pitkä, voit helposti tarkastella, voit muokata muistuttamaan hakukyselyn.

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Alla olevassa esimerkissä prosessin nimi on DancingPigs.exe, joka saattaa näkyä epäilyttävistä.
    ![wiredata etsinnän tulokset](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Käytä palauttaa luettelon, valitse nimetty prosessi. Tässä esimerkissä DancingPigs.exe on napsautettu. Alla olevassa tulokset kuvaavat verkkoliikennettä, kuten lähtevän tietoliikenteen tyyppiä eri protokollat päälle.
    ![wiredata tulokset osoittavat nimetty prosessi](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Koska suojaus ja valvonta-ratkaisu on asennettu, voit tutkia suojauksen tapahtumat, jotka on sama prosessinimi kenttäarvo muokkaamalla hakukysely IN ja erillinen Etsi kysely-operaattorien kyselyjä. Toteuta, valitse wire tiedot ja muita ratkaisu lokit on arvot samassa muodossa. Muokkaa hakukysely muistuttamaan:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata tulokset sisältävä taulukko on yhdistetty](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. Yllä tuloksista näet tilin tiedot näytetään. Voit nyt muokata hakukysely nähdäksesi, kuinka usein tilin suojaus ja valvonta tiedot näkyvät käytti prosessi, jonka kyselyn yksinkertaisia:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![tilin tiedot näkyvät wiredata tulokset](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Seuraavat vaiheet

- [Etsi lokit](log-analytics-log-searches.md) tarkastella yksityiskohtaisia wire haun tietueita.
- Tarkastele, Dan's [käyttämällä Wire tietojen toimintojen hallinta Suite Log haun blogin kirjaaminen](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) on lisätietoja siitä, kuinka usein tiedot kerätään ja miten voit muokata Operations Manager tekijöiden sivustokokoelman ominaisuudet.
