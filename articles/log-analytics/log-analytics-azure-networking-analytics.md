<properties
    pageTitle="Azure verkko Analytics-ratkaisun Log Analytics | Microsoft Azure"
    description="Lokitiedoston Analytics Azure verkko Analytics-ratkaisun avulla voit tarkastella Azure verkon suojauksen ryhmän lokit ja Azure sovelluksen yhdyskäytävän lokit."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure verkko Analytics (esikatselu)-ratkaisun Log Analytics

>[AZURE.NOTE] Tämä on [Esikatselu-ratkaisun](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Lokitiedoston Analytics Azure verkko Analytics-ratkaisun avulla voit tarkastella Azure sovelluksen yhdyskäytävän lokit ja Azure verkon suojauksen ryhmän lokitiedot.

Voit ottaa kirjaamisen Azure sovelluksen yhdyskäytävän lokit ja Azure verkon käyttöoikeusryhmät. Lokitiedostot kirjoitetaan Blob storage, johon ne sitten voi indeksoida Log Analytics haku-ja analysointia varten.

Seuraavat lokit tuetaan sovelluksen yhdyskäytävien:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

Seuraavat lokit tuetaan verkko-käyttöoikeusryhmät:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Asenna ja määritä ratkaisu

Seuraavien ohjeiden avulla voit asentaa ja määrittää Azure verkko Analytics-ratkaisua:

1.  Ota käyttöön vianmäärityksen kirjaus käyttöön resursseja, joita haluat seurata:
  + [Sovelluksen yhdyskäytävän](../application-gateway/application-gateway-diagnostics.md)
  + [Verkko-käyttöoikeusryhmän](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Määritä lokin analyysitiedot lokit lukea Blob-objektien tallennustilaan käyttämällä [JSON tiedostot Blob-objektien tallennustilaan](../log-analytics/log-analytics-azure-storage-json.md)kuvatut toimet.
3.  Ota käyttöön Azure verkko Analytics-ratkaisun avulla [lisätä Log Analytics-ratkaisuja ratkaisuvalikoimasta](log-analytics-add-solutions.md)kuvatut toimet.  

Jos otat ei Diagnostiikan kirjaus käyttöön tietyn resurssilaji-koontinäytön näiden kyseiselle resurssille näkyy tyhjänä.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Azure verkko Analytics sivustokokoelman tietojen tarkasteleminen

Azure verkko Analytics-ratkaisun kerää Azure-Blob-säiliö diagnostiikka lokit Azure sovelluksen yhdyskäytävien ja verkon käyttöoikeusryhmät.
Tietojen kerääminen tarvitaan ei agentti.

Seuraavassa taulukossa on tietoja sivustokokoelman menetelmistä ja muita tietoja siitä, miten Azure verkko Analytics kerätä tietoja.

| Käyttöympäristö | Suora agentti | Systems Center Operations Manager (SCOM)-agentti | Azure-tallennustilan | SCOM tarvitaan? | SCOM agentti tietojen lähetetyissä kutsuissa hallinta-ryhmä | Sivustokokoelman korkojakso |
|---|---|---|---|---|---|---|
|Azure|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Kyllä](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minuutin|

## <a name="use-azure-networking-analytics"></a>Azure verkko-analyysin avulla

Kun olet asentanut ratkaisun, voit tarkastella asiakkaan yhteenveto ja palvelimen virheet oman valvottu sovelluksen yhdyskäytävien **Azure verkko-analyysin** avulla ruutu Log Analytics **Yhteenveto** -sivu.

![Kuva Azure verkko Analytics-ruutu](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Kun olet valinnut **Yhteenveto** -ruutu, voit tarkastella yhteenvetojen lokit ja siirtyminen sitten seuraavat tiedot:

+ Sovelluksen yhdyskäytävän Access kirjaa
  - Asiakas- ja virheet sovelluksen yhdyskäytävän access-lokit
  - Pyyntöjä kunkin sovelluksen Gatewayn tunnissa
  - Kunkin sovelluksen yhdyskäytävän pyyntöjä tunnissa epäonnistui
  - Sovelluksen yhdyskäytävien käyttäjäagentti virheet
+ Sovelluksen yhdyskäytävän suorituskyky
  - Sovelluksen Gatewayn kunto Host (isäntä)
  - Sovelluksen yhdyskäytävän epäonnistui pyynnöt Prosenttipistettä vähimmäis- ja 95.
+ Verkko-käyttöoikeusryhmän estetyt työnkulut
  - Verkko-ryhmän suojaussäännöt estettyjen työnkulkuihin
  - MAC-osoitteet estettyjen työnkulkuihin
+ Verkko-käyttöoikeusryhmän sallittu työnkulut
  - Verkon suojaussäännöt ryhmän sallittujen työnkulkuihin
  - MAC-osoitteet, sallittujen työnkulkuihin


![Kuva Azure verkko analysoinnin koontinäytössä](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Kuva Azure verkko analysoinnin koontinäytössä](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Kuva Azure verkko analysoinnin koontinäytössä](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Kuva Azure verkko analysoinnin koontinäytössä](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Jos haluat tarkastella tiedot, mihin tahansa lokin yhteenveto

1. Valitse **sivulla** Napsauta **Azure verkko Analytics** -ruutua.
2. **Azure verkko analysoinnin** koontinäytössä Tarkista yhteenvetotiedot jossakin näiden ja valitse sitten jokin voit tarkastella yksityiskohtaisia tietoja haku-sivulla.

    Mihin tahansa log etsintäsivuja näet tulokset aika, yksityiskohtaiset tulokset ja log hakujen. Voit myös suodattaa muita tarkentamalla hakutuloksia mukaan.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Log rajaamalla Log Analytics](log-analytics-log-searches.md) avulla voit tarkastella yksityiskohtaisia Azure verkko Analytics-tiedot.
