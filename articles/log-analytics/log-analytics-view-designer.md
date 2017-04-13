<properties
    pageTitle="Kirjaudu Analytics näkymän suunnittelu | Microsoft Azure"
    description="Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilössä OMS konsolissa. Tässä artikkelissa on yleiskatsaus näkymän suunnittelu ja luonnin ja muokata mukautettuja näkymiä."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer"></a>Lokitiedoston Analytics näkymän suunnittelu
Näkymän suunnittelu Log Analytics avulla voit luoda mukautettuja näkymiä, jotka sisältävät erilaisia visualisointeja tietojen OMS säilössä OMS konsolissa. Tässä artikkelissa on yleiskatsaus näkymän suunnittelu ja luonnin ja muokata mukautettuja näkymiä.

Muita artikkeleita näkymän suunnittelu käytettävissä ovat seuraavat:

- [Ruutu-viittaus](log-analytics-view-designer-tiles.md) - asetukset ovat käytettävissä mukautettuja näkymiä ruutujen viittaus. 
- [Visualisoinnin osan viittaus](log-analytics-view-designer-parts.md) - asetukset ovat käytettävissä mukautettuja näkymiä ruutujen viittaus. 


## <a name="concepts"></a>Käsitteitä
Näytä Designerin avulla luotuja näkymiä elementtien seuraavassa taulukossa.

| Osa | Kuvaus |
|:--|:--|
| Ruutu | Näkyviin tärkeimmät Log Analytics yleiskatsaus hallintanäkymään.  Sisältää visual yhteenvetojen mukautetun näkymän sisältämiä tietoja.  Ruutu erilaista on erilaisia visualisointeja tietueiden OMS säilö.  Valitse Avaa mukautettu näkymä-ruutu. |
| Mukautettu näkymä | Näkyviin, kun käyttäjä napsauttaa-ruutu.  Sisältää vähintään yhden visualisoinnin osia. |
| Visualisoinnin osat | Vähintään yhden [lokin haut](log-analytics-log-searches.md)perusteella OMS säilössä tietojen visualisoinnin.  Useimpien osia sisältää otsikon, joka sisältää korkean tason visualisointi ja yläreunan tulosluettelossa.  Toinen osa tyypit on erilaisia visualisointeja tietueiden OMS säilö.  Valitse osat antamisen yksityiskohtaisia log haku-osassa. |

![Suunnittelu yhteenveto](media/log-analytics-view-designer/overview.png)

## <a name="add-view-designer-to-your-workspace"></a>Näkymän suunnittelu lisääminen työtilaan
Näkymän suunnittelu ollessa esikatselussa on lisättävä se lisääminen työtilaan valitsemalla **Esikatselutoiminnot** OMS-portaalin **asetukset** -osassa.

![Esikatselun ottaminen käyttöön](media/log-analytics-view-designer/preview.png)

## <a name="creating-and-editing-views"></a>Luominen ja muokkaaminen näkymät

### <a name="create-a-new-view"></a>Luo uusi näkymä
Avaa uusi näkymä **Näkymän suunnittelu** OMS dashboard-pääsivun näkymän suunnittelu-ruutu valitsemalla.

![Näkymän suunnittelu-ruutu](media/log-analytics-view-designer/view-designer-tile.png)

### <a name="edit-an-existing-view"></a>Muokkaa aiemmin luotuun näkymään
Jos haluat muokata aiemmin luodun näkymän näkymän suunnittelun, avaa valitsemalla Valitse kanavan ruudun tärkeimmät OMS raporttinäkymät-ikkunassa.  Napsauta näkymän avaaminen näkymän suunnittelu **Muokkaa** -painiketta.

![Mukauttaminen](media/log-analytics-view-designer/menu-edit.png)

### <a name="clone-an-existing-view"></a>Kloonaa aiemmin luotuun näkymään
Kun Kloonaa näkymän, se luo uuden näkymän ja avaa se näkymän suunnittelu.  Uusi näkymä on on sama nimi kuin alkuperäisen kanssa "Kopioi" liitetyssä sen loppuun.  Jos haluat Kloonaa näkymän, Avaa aiemmin luotu näkymä valitsemalla Valitse kanavan ruudun tärkeimmät OMS raporttinäkymät-ikkunassa.  Napsauta näkymän avaaminen näkymän suunnittelun **Kloonaa** -painiketta.

![Kloonaa näkymä](media/log-analytics-view-designer/edit-menu-clone.png)

### <a name="delete-an-existing-view"></a>Poista aiemmin luotu näkymä
Jos haluat poistaa aiemmin luodun näkymän, avaa valitsemalla Valitse kanavan ruudun tärkeimmät OMS raporttinäkymät-ikkunassa.  Valitse näkymän avaaminen näkymän suunnittelun **Muokkaa** -painiketta ja valitse **Poista näkymä**.

![Näkymän poistaminen](media/log-analytics-view-designer/edit-menu-delete.png)

### <a name="export-an-existing-view"></a>Vie aiemmin luotu näkymä
Voit viedä näkymän JSON-tiedostoon, voit tuoda työtiloissa tai [Azure Resurssienhallinta-malli](../resource-group-authoring-templates.md).  Avaa vietävän aiemmin luotuun näkymään valitsemalla Valitse kanavan ruudun tärkeimmät OMS raporttinäkymät-ikkunassa.  Valitse Luo tiedosto selaimen latauskansioon **Vie** -painiketta.  Tiedoston nimi on näkymä, jossa tunniste *omsview*nimi.

![Näkymän vieminen](media/log-analytics-view-designer/edit-menu-export.png)

### <a name="import-an-existing-view"></a>Tuo aiemmin luotu näkymä
Voit tuoda *omsview* -tiedosto, jonka olet vienyt hallinta-ryhmästä toiseen.  Voit tuoda olemassa oleva näkymä, Luo uusi näkymä.  Valitse **Tuo** -painiketta ja valitse *omsview* -tiedosto.  Tiedoston määrittäminen kopioidaan olemassa olevaan näkymään.

![Näkymän vieminen](media/log-analytics-view-designer/edit-menu-import.png)

## <a name="working-with-view-designer"></a>Näkymän suunnittelu käsitteleminen
Näkymän suunnittelu on kolme ruutua.  **Ulkoasu** -ruudun edustaa mukautetussa näkymässä.  Kun lisäät ruudut ja osat **hallinta** -ruudusta **ne lisätään näkymään suunnitteluruutuun** .  **Ominaisuudet** -ruudussa näkyy ruutu tai valitun osan ominaisuudet.

![Näkymän suunnittelu](media/log-analytics-view-designer/view-designer-screenshot.png)

### <a name="configure-view-tile"></a>Määritä Näytä-ruutu
Mukautetun näkymän voi olla vain yksi osa.  Valitse **hallinta** -ruudussa voit tarkastella nykyiseen ruutuun tai valitse vaihtoehtoinen **vierekkäin** -välilehti.  **Ominaisuudet** -ruudussa näkyy nykyiseen ruutuun ominaisuudet.  Mukaan yksityiskohtaiset tiedot vierekkäin-ominaisuuksien määrittäminen [Viittaus-ruutu](log-analytics-view-designer-tiles.md) ja valitse **Käytä** Tallenna muutokset.

### <a name="configure-visualization-parts"></a>Määritä visualisoinnin osat
Näkymän voi sisältää kaikki visualisoinnin osien määrä.  Valitse **Näytä** -välilehti ja visualisointi-osan lisääminen näkymään.  **Ominaisuudet** -ruudussa näkyy valitun osan ominaisuudet.  Näytä ominaisuuksien mukaan yksityiskohtaiset tiedot määrittäminen [visualisoinnin osan viitteen](log-analytics-view-designer-parts.md) ja valitse **Käytä** Tallenna muutokset.

### <a name="delete-a-visualization-part"></a>Visualisoinnin osan poistaminen
Voit poistaa visualisoinnin osan näkymästä valitsemalla osan oikeassa yläkulmassa olevaa **X** -painiketta.

### <a name="rearrange-visualization-parts"></a>Visualisoinnin osien järjestyksen muuttaminen
Näkymien sisältää vain yhden rivin visualisoinnin osaa.  Järjestä aiempia osia näkymässä napsauttamalla ja vetämällä ne uuteen paikkaan.


## <a name="next-steps"></a>Seuraavat vaiheet

- [Ruutujen](log-analytics-view-designer-tiles.md) lisääminen mukautetussa näkymässä.
- Lisää mukautettu näkymä [Visualisoinnin osat](log-analytics-view-designer-parts.md) .
