<properties 
   pageTitle="StorSimple tilannevedoksen hallinta varmuuskopion työt | Microsoft Azure"
   description="Tässä artikkelissa käsitellään tarkastella ja hallita ajoitetun, käynnissä ja valmiit varmuuskopion työt StorSimple tilannevedoksen hallinnan MMC-laajennuksen avulla."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/26/2016"
   ms.author="v-sharos" />


# <a name="use-storsimple-snapshot-manager-to-view-and-manage-backup-jobs"></a>Tarkastella ja hallita varmuuskopiointityön StorSimple tilannevedoksen hallinnan avulla

## <a name="overview"></a>Yleiskatsaus

**Työt** -solmu **laajuus** -ruudussa näkyy **ajoitettu**, **viimeisen 24 tuntia**ja **käynnissä** varmuuskopioinnin tehtäviin, jotka olet aloittanut vuorovaikutteisesti tai määritetty käytäntö. 

Tässä opetusohjelmassa kerrotaan käyttämisestä **työt** -solmu tiedon ajoitetun, viimeisimmät ja käynnissä varmuuskopioinnin työt. (Työt ja vastaavat tiedot näkyvät **tulosruudussa** .) Voit lisäksi, napsauta luettelossa työn sekä tarkastella pikavalikko, jossa näkyvät käytettävissä olevat toiminnot.

## <a name="view-scheduled-jobs"></a>Tarkastele ajoitetuissa

Seuraavien ohjeiden avulla voit tarkastella varmuuskopion ajoitetuissa.

#### <a name="to-view-scheduled-jobs"></a>Jos haluat tarkastella ajoitetuissa

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta. 

2. **Laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse **ajoitettu**. **Tulokset** -ruudussa näkyy seuraavat tiedot:

    - **Nimi** – aikataulutettu tilannevedos nimi

    - **Suorita seuraava** – päivämäärän ja kellonajan seuraava ajoitettu tilannevedoksen

    - **Edellinen suoritus** – päivämäärän ja kellonajan viimeisimmän ajoitetun tilannevedoksen

    >[AZURE.NOTE] Erikseen vain tilannevedoksia **Seuraava Suorita** - ja **Edellinen suoritus** on sama. 
 
    ![Varmuuskopion ajoitetuissa](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
 
3. Tietyn työn muita toimintojen suorittamiseen **tulosruudussa** työ-nimeä hiiren kakkospainikkeella ja valitse valikon vaihtoehdoista.

## <a name="view-recent-jobs"></a>Näytä viimeksi käytetyt työt

Seuraavien ohjeiden avulla voit tarkastella varmuuskopiointi ja palauttaminen työt, jotka on tehty viimeisen 24 tunnin aikana.

#### <a name="to-view-recent-jobs"></a>Voit tarkastella viimeisimmät töitä

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse **viimeisen 24 tuntia**. **Tulokset** -ruudussa näkyy varmuuskopiointityön (, enintään 64 työt) viimeisen 24 tuntia. Seuraavat tiedot näkyvät **tulosruudussa **näkymän** asetusten mukaan** :

    - **Nimi** – aikataulutettu tilannevedos nimi.
 
    - **Aloittaminen** – päivämäärä ja kellonaika, jolloin tilannevedoksen alkamisen.

    - **Pysäytetty** – päivämäärä ja kellonaika, kun tilannevedoksen valmis tai on lopetettu.

    - **Kulunut** – ajan **aloittaminen** ja **pysäytetty** välisen ajan.

    - **Tila** – viimeksi valmiin työn tilan. **Success** ilmaisee, että Varmuuskopiointi onnistui. **Epäonnistui** ilmaisee, että projektin ei suoritettu onnistuneesti.

    - **Tietoja** – virheen syy.

    - **Tavua käsitteleminen (Mt)** – aseman ryhmä, johon käsiteltiin (Valitse MBs) tietojen määrää. 

    ![Työt, jotka suoritettiin viimeisen 24 tunnin aikana](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 

3. Tietyn työn muita toimintojen suorittamiseen **tulosruudussa** työ-nimeä hiiren kakkospainikkeella ja valitse valikon vaihtoehdoista.

    ![Työn poistaminen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png) 
     
## <a name="view-currently-running-jobs"></a>Näytä käynnissä olevat työt

Noudattamalla seuraavia vaiheita näkymän projekteille, jotka ovat parhaillaan käynnissä.

#### <a name="to-view-currently-running-jobs"></a>Jos haluat tarkastella parhaillaan käynnissä olevat työt

1. Napsauta työpöydän StorSimple tilannevedoksen hallinnan käynnistäminen kuvaketta.

2. **Laajuus** -ruudussa Laajenna **Projektit** -solmu ja valitse **käynnissä**. Sen mukaan, voit määrittää **näkymän** asetukset **tulokset** -ruudussa näkyy seuraavat tiedot: 

    - **Nimi** – aikataulutettu tilannevedos nimi.

    - **Aloittaminen** – päivämäärä ja kellonaika, jolloin tilannevedoksen alkamisen.

    - **Tarkistuspiste** – varmuuskopioinnin toiminto.

    - **Tila** – valmistumisprosenttia.
    
    - **Kulunut** – ajan, joka on kulunut varmuuskopioinnin alkamisen. 

    - **Keskimääräinen siirtonopeuden (Mt)** – tietojen käsitteleminen, joka käsittelee (MBs) kokonaisaika tavujen suhde.

    - **Tavua käsitteleminen (Mt)** – tavujen tietojen käsitteleminen (Valitse MBs).

    - **Tavua kirjoitettu (Mt)** – tavujen tietojen kirjoitettu (MBs). Se sisältää tiedot sekä metatiedot ja on siksi yleensä suurempi kuin tavua käsitteleminen.

    ![Käynnissä olevat työt](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)

3. Tietyn työn muita toimintojen suorittamiseen **tulosruudussa** työ-nimeä hiiren kakkospainikkeella ja valitse valikon vaihtoehdoista.

## <a name="next-steps"></a>Seuraavat vaiheet

- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta ammattimainen StorSimple ratkaisu](storsimple-snapshot-manager-admin.md).
- Lisätietoja käyttämisestä [StorSimple tilannevedoksen hallinta hallittavan varmuuskopion luettelon](storsimple-snapshot-manager-manage-backup-catalog.md).















            


 

