<properties
   pageTitle="Ratkaisujen toimintojen hallinta-ohjelmistopaketin (OMS) | Microsoft Azure"
   description="Ratkaisujen laajentaa toimintoja toimintojen hallinta Suite (OMS) tarjoamalla pakattu hallinta tilanteita, joissa asiakkaat voit lisätä niiden OMS työtilaan.  Tässä artikkelissa on tietoja siitä, miten mukautetut ratkaisut luoma asiakkaat ja kumppanit."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="bwren" />

# <a name="management-solutions-in-operations-management-suite-oms-preview"></a>Ratkaisujen-toimintojen hallinta Suite (OMS) (ennakkoversio)

>[AZURE.NOTE]Tämä on OMS, jotka ovat tällä hetkellä esikatselu-ratkaisujen alustava.    

Ratkaisujen laajentaa toimintoja toimintojen hallinta Suite (OMS) antamalla pakattu hallinta tilanteita, joissa asiakkaat voi lisätä niiden ympäristössä.  Lisäksi on [Microsoftin toimittaman ratkaisuja](../log-analytics/log-analytics-add-solutions.md)kumppanit ja asiakkaat voit luoda oman ympäristössä käytettävä ratkaisujen ja asiakkaiden yhteisön kautta.

## <a name="finding-and-installing-management-solutions"></a>Etsiminen ja ratkaisujen asentaminen
On useita tapoja etsiminen ja ratkaisujen asentaminen ohjeiden mukaisesti seuraavissa osissa.

### <a name="azure-marketplace"></a>Azure Marketplacesta
Microsoftin toimittaman ratkaisujen ja luotettujen kumppaneille voidaan asentaa Azure-portaalissa Azure Marketplacesta.

1. Kirjaudu sisään Azure-portaaliin.
2. Valitse vasemmanpuoleisessa ruudussa **Lisää palveluja**.
3. Vieritä **ratkaisuja** tai kirjoita *ratkaisuja* **Suodatin** -valintaikkuna.
4. Napsauta **+ Lisää** -painiketta.
5. Etsi ratkaisuja, jotka olet kiinnostunut joko selaaminen, napsauttamalla **Suodata** -painiketta tai kirjoittamalla **Everthing haku** -ruutuun.
6. Valitse marketplace-kohdetta, voit tarkastella sen yksityiskohtaiset tiedot.
4. Valitse **Luo** Avaa **Lisää ratkaisu** -ruutu.
5. Pakollinen tietoja, kuten [OMS työtilan ja automaatio tilin](#oms-workspace-and-automation-account) parametreja ratkaisun arvojen lisäksi tulee kehotus.
6. Valitse **Luo** ratkaisun asentaminen.

### <a name="oms-portal"></a>OMS Portal
Microsoftin ratkaisujen voidaan asentaa ratkaisuvalikoimasta OMS-portaalissa.

1. Kirjaudu sisään OMS-portaaliin.
2. Napsauta ruutua **Ratkaisuvalikoimaan** .
2. Lisätietoja OMS ratkaisuvalikoiman sivulla kunkin käytettävissä ratkaisu. Napsauta nimeä, jonka haluat lisätä OMS-ratkaisun.
3. Ratkaisun, jonka valitsit sivulla näkyy ratkaisun yksityiskohtaisia tietoja. Valitse **Lisää**.
4. Uusi ruutu ratkaisun, jonka lisäsit näkyy sivun OMS ja voit kokeilla sitä käytännössä jälkeen OMS-palvelun käsittelee tietojen yhteenveto.

### <a name="azure-quickstart-templates"></a>Azure pikaopas mallit
Yhteisön jäsenet voivat lähettää ratkaisujen Azure pikaopas malleja.  Voit ladata mallit myöhemmin asennuksen tai tarkastaa ne kerrotaan, miten voit [luoda omia ratkaisuja](#creating-a-solution).

1. Noudata [OMS työtilan ja automaatio tilin](#oms-workspace-and-automation-account) linkittää työtilan ja tilin kuvatut toimet.
2. Siirry [Azure pikaopas malleja](https://azure.microsoft.com/documentation/templates/).  
3. Etsi ratkaisu, joka käyttämällä.
4. Valitse ratkaisun tuloksia voidaan tarkastella sen tietoja.
5. Napsauta **käyttöönotto Azure** -painiketta.
6. Voit pyydetään antamaan parametreja ratkaisun tietoja, kuten resurssiryhmä ja arvojen lisäksi sijainti.
7. Valitse **Osta** ratkaisun asentaminen.

### <a name="deploy-azure-resource-manager-template"></a>Azure Resurssienhallinta mallin ottaminen käyttöön
Ratkaisut, näkyviin tulee yhteisöstä tai Resurssienhallinta-mallina, voit [luoda itse](#creating-a-solution) on otettu käyttöön ja voit käyttää mitä tahansa vakio-menetelmää käyttöönoton [mallin](../resource-group-template-deploy-portal.md).  Huomaa, että ennen asennusta ratkaisu on luotava ja linkitä [OMS työtilan ja automaatio-tili](#oms-workspace-and-automation-account).

## <a name="oms-workspace-and-automation-account"></a>OMS työtilan ja automaatio-tili
Useimmat ratkaisujen edellyttävät [OMS työtilan](../log-analytics/log-analytics-manage-access.md) sisältämään näkymiä ja [automaatio-tili](../automation/automation-security-overview.md#automation-account-overview) sisältää runbooks ja siihen liittyvät resurssit. Työtilan ja tili on täytettävä seuraavat vaatimukset.

- Ratkaisua voi käyttää vain yksi OMS työtila ja yksi automaatio-tili.  
- OMS työtilan ja ratkaista käyttämä automaatio-tili on linkitetty toisiinsa. OMS työtilan voidaan linkittää vain yhteen automaatio-tilille ja automaatio-tili voidaan linkittää vain yhteen OMS työtilaan.
- Linkitetään, OMS työtilan ja automaatio-tilin on oltava sama resurssiryhmä- ja alueasetusten.  Poikkeus on OMS-työtilan Yhdysvaltojen Itä-alueella ja ja Yhdysvaltojen Itä 2 automaatio-tili.

### <a name="creating-a-link-between-an-oms-workspace-and-automation-account"></a>Linkin OMS työtilan ja automaatio-tilin luominen
Miten voit määrittää OMS työtilan ja automaatio-tilin määräytyy asennustapa-ratkaisun muutosaluetta.

- Kun olet asentanut Microsoft-ratkaisun palvelun OMS-portaalissa, se on asennettu nykyisessä OMS työtilassa ja automaatio ei tarvita.

- Azure Marketplace-ratkaisun asennuksen yhteydessä, sinua kehotetaan OMS työtilan ja automaatio-tiliin ja niiden välinen linkki on luonut puolestasi.  

- Ratkaisujen ulkopuolella Azure Marketplace-Linkitä OMS työtilan ja automaatio tilin ennen asennusta ratkaisu.  Voit tehdä tämän valitsemalla minkä tahansa ratkaisu Azure Marketplacesta ja valitsemalla OMS työtilan ja automaatio-tili.  Sinun ei tarvitse asentaa todella ratkaisun, koska linkin luodaan, kun OMS työtilan ja automaatio-tili on valittuna.  Kun linkki luodaan jälkeen voit käyttää kyseisen OMS työtilan ja automaatio-tilin minkä tahansa ratkaisu. 

### <a name="verifying-the-link-between-an-oms-workspace-and-automation-account"></a>Linkki OMS työtilan ja automaatio-tilin vahvistaminen
Voit tarkistaa OMS-työtilan ja seuraavalla tavalla automaatio-tilin välisen linkin.

1. Valitse Automaattiset tili Azure-portaalissa.
2. Siirry **asetukset** -ruudussa.
3. Jos näkyvissä on osan nimeltä **OMS resurssien** **asetukset** -ruudussa, tähän tiliin on liitetty OMS-työtilassa.
4. Valitse **työtilassa** voit tarkastella OMS työtilan linkitetty automaatio tähän tiliin.


## <a name="listing-management-solutions"></a>Luettelon ratkaisujen
Noudattamalla seuraavia vaiheita voit katsella ratkaisujen linkitetty Azure tilauksen työtilat.

1. Kirjaudu sisään Azure-portaaliin.
2. Valitse vasemmanpuoleisessa ruudussa **Lisää palveluja**.
3. Vieritä **ratkaisuja** tai kirjoita *ratkaisuja* **Suodatin** -valintaikkuna.
4. Kaikki työtiloista asennettu ratkaisut näkyvät.

Huomaa, että voit tarkastella vain nykyisessä työtilassa OMS-portaalissa asennettuna Microsoft-ratkaisuja.

## <a name="removing-a-management-solution"></a>Management-ratkaisun poistaminen
Kun management-ratkaisun poistetaan, kaikki resurssit ratkaisun poistetaan.  

1. Etsi ratkaisu [luettelo ratkaisuista](#listing-solutions)ohjeen Azure-portaalissa.
2. Valitse poistettava ratkaisu.
3. Napsauta **Poista** -painiketta.

## <a name="creating-a-management-solution"></a>Management-ratkaisun luominen
Valmis ohjeita ratkaisujen luomisesta on osoitteessa [luominen ratkaisuja toimintojen hallinta Suite (OMS)](operations-management-suite-solutions-creating.md). 


## <a name="next-steps"></a>Seuraavat vaiheet

- Esimerkkejä eri Resurssienhallinta malleja haku [Azure pikaopas malleja](https://azure.microsoft.com/documentation/templates) .
- Luo omia [ratkaisujen](operations-management-suite-solutions-creating.md).
 