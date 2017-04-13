<properties
    pageTitle="Kapasiteetin näennäiskoneiden ja Azure palauttaminen fyysinen palvelimet suunnitteleminen | Microsoft Azure"
    description="Azure palauttaminen koordinoi replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet sijaitsevat paikallisen Azure tai toissijaisen paikallisen-sivustoon." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="07/12/2016" 
    ms.author="raynew"/>

# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Kapasiteetin näennäiskoneiden ja Azure palauttaminen fyysinen palvelimet suunnitteleminen

Azure sivuston palauttaminen kapasiteetin suunnittelu-työkalulla voit selvittää, kapasiteetin tarpeen Hyper-V VMs, VMware VMs ja Windows-tai Linux fyysinen palvelinten Azure palauttaminen.


## <a name="overview"></a>Yleiskatsaus

Sivuston palauttaminen Capacity Planner avulla voit analysoida lähde-ympäristön ja toiminnoista ja selvittää kaistanleveyden tarpeet, tarvitset tietolähteen sijainti palvelinresurssien ja resurssit (näennäiskoneiden ja tallennustilaa muille), jota tarvitaan kohteen sijainti. 

Voit suorittaa työkalun muutamalla tilasta:

- **Nopea suunnittelu**: Suorita työkalu tässä tilassa saat verkko- ja ennusteiden perusteella VMs, levyjä, varasto ja muuta korko keskimääräinen määrä.
- **Yksityiskohtainen suunnittelu**:-työkalun suorittaminen tässä tilassa ja kunkin työmäärää AM tasolla tiedot. Analysoi AM yhteensopivuus ja poistaa verkko- ja ennusteiden.

## <a name="before-you-start"></a>Ennen aloittamista

Ennen kuin suoritat työkalun:

1. Kerää tietoja ympäristön, mukaan lukien VMs-levyjä kohti AM-tallennustilan levyä kohden.
2. Määritä päivittäinen muuttaminen (churn)-kurssi replikoitua tiedoille. Toiminto:

    - Jos olet replikoiminen Hyper-V VMs sitten Lataa [Hyper-V kapasiteetin suunnittelutyökalussa](https://www.microsoft.com/download/details.aspx?id=39057) saat muuta korko. [Lue lisää](site-recovery-capacity-planning-for-hyper-v-replication.md) siitä, että tämä työkalu. Suosittelemme tämän työkalun kannattaa tallentaa keskiarvojen viikon ajan.
    - Jos olet replikoiminen VMware näennäiskoneiden, churn nopeus voi selvittää [vSphere kapasiteetin suunnittelu laitteen](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance) avulla.
    - Jos olet replikoiminen fyysiset palvelimet, sinun on arvio manuaalisesti.

## <a name="run-the-quick-planner"></a>Suorita nopeasti suunnittelu
1.  Lataa ja Avaa [Azure sivuston palauttaminen kapasiteetin](http://aka.ms/asr-capacity-planner-excel) suunnittelutyökalua. Sinun on suorittaa makroja joten valitse muokkauksen ottaminen käyttöön ja ottaa käyttöön sisällön pyydettäessä. 
2.  **Suunnittelu-tyypin valitseminen** Valitse luettelosta-ruutuun **Lyhyt suunnittelu** .

    ![Käytön aloittaminen](./media/site-recovery-capacity-planner/getting-started.png)

3.  Kirjoita laskentataulukon **Capacity Planner** tarvittavat tiedot. Täytä kaikki ympyröity punaisella alla olevassa näyttökuvassa kentät.

    - Valitse **Valitse käyttämässäsi skenaariossa** **Hyper-V Azure** tai **VMware/fyysisiä Azure avulla**.
    - Sinulla on tarvitsemasi- **keskiarvo päivittäiset tiedot muuttuvat korko (%)** , tiedot sijoitetaan [Hyper-V kapasiteetin suunnittelutyökalussa](site-recovery-capacity-planning-for-hyper-v-replication.md) tai [vSphere kapasiteetin suunnittelu laitteen](https://labs.vmware.com/flings/vsphere-replication-capacity-planning-appliance).  
    - **Pakkaamisen** koskee vain tarjota, kun replikoiminen VMware VMs tai fyysinen palvelimia Azure pakkaus. Olemme arvioida 30 % tai enemmän, mutta voit muuttaa asetusta halutulla tavalla. Jos replikoiminen Hyper-V VMs Azure pakkaamisen voit kolmannen osapuolen laitteen esimerkiksi Riverbed. 
    -  Määritä **Säilytys syötteiden** kuinka kauan replikoiden säilytetään. Jos olet replikoiminen VMware tai fyysiset palvelimet arvosarjan arvon päivinä. Jos olet replikoiminen Hyper-V Määritä aika tunteina.
    -  **Mitä ensimmäisen replikoinnin näennäiskoneiden erän tuntien täyttävän** ja **näennäiskoneiden ensimmäisen replikoinnin erää kohti määrä** syötät asetuksia, joita käytetään ensimmäisen replikoinnin vaatimukset laskemiseen.  Kun sivuston palauttaminen otetaan käyttöön koko alkuperäisen tietojoukon voi ladata. 

    ![Syötteiden](./media/site-recovery-capacity-planner/inputs.png)

2.  Kun olet lisännyt haluamasi arvot lähde-ympäristössä, näytössä tulos sisältää:

    - **Kaistanleveyden vaaditaan delta sallittuja** (Mt/s). Keskimääräinen päivittäinen tietojen-Muuta määrä lasketaan delta replikoinnin kaistanleveys.
    - **Kaistanleveyden vaaditaan ensimmäisen replikoinnin** (Mt/s). Olet sijoittanut ensimmäisen replikoinnin arvot lasketaan ensimmäisen replikoinnin kaistanleveys. 
    - **Tallennustilaa edellyttämät (gigatavun)** on pakollinen Azure kokonaistallennustila.
    - **Yhteensä IOPS vakio tallennustilan tilien** lasketaan 8 K IOPS kokoa vakio tallennustilan tilien perusteella.  Muuta korko nopea työsuunnitteluapuohjelma määrä lasketaan lähde VMs levyille ja päivittäisten tietojen perusteella. Muuta näiden VMs määrä yksityiskohtainen suunnittelu määrä lasketaan kokonaismäärän VMs, jotka on yhdistetty vakio Azure VMs ja tietojen perusteella. 
    - **Tilien vakio tallennustilan määrä** on vakio tallennustilan asiakkaiden suojaaminen VMs tarvitaan kokonaismäärän. Huomaa, että vakio tallennustilan tilin voivat sisältää enintään 20000 IOPS yli kaikki vakio tallennustilan VMs enintään 500 IOPS tuettu levyä kohden. 
    - **Blob-levyjen määrä pakollinen** palauttaa levyjä, jotka luodaan Azure tallennustilan määrän.
    - **Premium-tallennustilan tilin määrä pakollinen** on premium tallennustilan asiakkaiden suojaaminen VMs tarvitaan kokonaismäärän. Huomaa, että lähteen AM hyvin IOPS (suurempi kuin 20000) kanssa on premium-tallennustilan tilin. Premium-tallennustilan tilin voivat sisältää enintään 80000 IOPS.
    - **Valitse premium tallennustilan kokonaismäärä IOPS** lasketaan 256 K IOPS kokoa yhteensä premium tallennustilan tilien perusteella.  Muuta korko nopea työsuunnitteluapuohjelma määrä lasketaan lähde VMs levyille ja päivittäisten tietojen perusteella. Muuta näiden VMs määrä yksityiskohtainen suunnittelu määrä lasketaan kokonaismäärän VMs, jotka on yhdistetty premium Azure AM (DS ja GS sarja) ja tietojen perusteella. 
    - **Määritysten palvelimien määrä pakollinen** näyttää kuinka monta määritys-palvelimet tarvittavat käyttöönoton (1)
    - **Prosessit-palvelimien määrä pakollinen** näyttää tarvitaanko prosessit-palvelimet lisäksi prosessin palvelimeen, joka on määritetty oletusarvoisesti määritys-palvelimeen.
    - **100 %: n tallennustilaa lähteen** näyttää vaaditaanko lisätallennustilaa lähdesijainnista.
            
    ![Tulos](./media/site-recovery-capacity-planner/output.png)
 
## <a name="run-the-detailed-planner"></a>Suorita yksityiskohtaiset suunnittelu


1.  Lataa ja Avaa [Azure sivuston palauttaminen kapasiteetin](http://aka.ms/asr-capacity-planner-excel) suunnittelutyökalua. Sinun on suorittaa makroja joten valitse muokkauksen ottaminen käyttöön ja ottaa käyttöön sisällön pyydettäessä. 
2.  **Valitse planner tyyppi** Valitse avattavasta luetteloruudusta **Yksityiskohtaiset suunnittelu** .

    ![Käytön aloittaminen](./media/site-recovery-capacity-planner/getting-started-2.png)

3.  **Kuormituksen pätevyys** laskentataulukon Anna vaaditut tiedot. Täytä merkityt kentät.

    - Määritä **suoritin sydämiä** sydämiä kokonaismäärän lähde-palvelimessa.
    - Määritä **muistin varaamista megatavua** RAM-Muistia koon Lähdepalvelimen. 
    - **NIC numero** määrittää minuuttimäärän, jonka verkkosovittimien lähde-palvelimessa. 
    -  Määritä kokonaiskoko AM-tallennustilan **kokonaismäärä tallennustilan (gt)** . Esimerkki Jos lähdepalvelimeen on 3 levyä 500 gt kanssa ja sitten tallennustilan koko on 1 500 Gigatavua.
    -  Määritä levyjen lähde-palvelimen kokonaismäärän **liitetty levyjen määrä** .
    -  Määritä **levyn kapasiteetin käyttö** keskimääräinen käyttö.
    -  **Muuta päivittäin korko (%)** Määritä päivittäiset tiedot muuttuvat korkokannan lähdepalvelimeen.
    -  Kirjoita **Yhdistäminen Azure koon** Azure AM kokoa, jonka haluat yhdistää. Jos et halua manuaalisesti valitsemalla**Laske IaaS VMs**. Huomaa, että jos syötteen manuaalinen asetus ja valitse sitten Laske IaaS VMs manuaalinen asetus ehkä voi korvata, koska Laske prosessin tunnistaa parhaiten Azure AM koon automaattisesti.

    ![Kuormituksen pätevyys](./media/site-recovery-capacity-planner/workload-qualification.png)

4.  Jos valitset **Laske IaaS VMs** tässä on kuvaus:

    - Tarkistaa pakollinen syötteiden.
    - Laskee IOPS ja ehdottaa parhaat Azure AM aize kunkin VMs vastaavuuden, joka on oikeutettu Azure-replikoinnin. Jos virhe on myönnetty tarvittavat koosta Azure AM ei tunnisteta. Esimerkki Jos määrä levyt liitetyn-65 virhe on ongelmia jälkeen korkein koon Azure AM on 64.
    - Ehdottaa tallennustilan-tili, jota voi käyttää Azure-AM.
    - Laskee vakio tallennustilan ja premium tallennustilan tilien pakollinen kuormituksen kokonaismäärän. Vieritä alaspäin oikeuden tarkastella Azure tallennustyyppi ja tallennustilaa tili, jota voi käyttää lähdepalvelin
    - Päättää ja lajittelee tarvittavat tallennustyyppi (vakio tai premium) AM ja liitetty levyjen määrä määritetty perustuvat taulukon loppuun. Näyttää kaikki VMs, jotka täyttävät varmuuskopion Azure-sarakkeen A (on AM qualified?) Kyllä. Jos AM ei palautettua enintään Azure virhe on näkyvissä.

AU AA sarakkeet ovat tulostus ja kunkin AM tiedot.

![Kuormituksen pätevyys](./media/site-recovery-capacity-planner/workload-qualification-2.png)


### <a name="example"></a>Esimerkki
Esimerkki kuusi VMs arvot näkyvät seuraavassa taulukossa työkalu laskee ja määrittää Azure AM parhaiten ja Azure tallennustilan vaatimuksia.

![Kuormituksen pätevyys](./media/site-recovery-capacity-planner/workload-qualification-3.png)

- Esimerkki tulosteesta Huomautus seuraavasti:
    
    - Ensimmäinen sarake on vahvistus sarakkeen VMs, levyille ja churn.
    - Kaksi vakio tallennustilan asiakkaat ja yksi premium-tallennustilan tilin tarvitaan viisi VMs. 
    -  VM3 ei oikeuta suojaus koska pienempään on yli 1 TT.
    -  VM1 ja VM2 voivat käyttää ensin vakio tallennustilan-tili
    -  VM4 käyttämällä vakio tallennustilan toinen tili.
    -  VM5 ja VM6 on premium-tallennustilan tilin ja sekä käyttää tilistä.

    >[AZURE.NOTE]  Valitse vakio- ja premium tallennustilan IOPS lasketaan AM tasolla ja ei levyä tasolla. Vakio virtual machine voit käsitellä enintään 500 IOPS levyä kohden. Jos DVD-levyllä IOPS on yli 500 tarvitset premium-tallennustilan. Kuitenkin jos DVD-levyllä IOPS on yli 500 mutta IOPS yhteensä AM levyjen rajoissa tuki vakio Azure AM (AM koon, levyjen sovittimet suorittimen, muistin määrän määrä) sitten suunnittelija valitsee standardi AM ja etkä DS tai GS sarja. Sinun on päivitettävä manuaalisesti sopiva DS: ÄÄN tai GS sarjan AM yhdistäminen Azure size-soluun.

5. Kun kaikki tiedot ovat paikallaan, valitse **Lähetä tiedot suunnittelu-työkalu** Avaa **Capacity Planner** toiminnoista on korostettu näyttää, onko ne oikeuta suojaus vai ei.


### <a name="submit-data-in-the-capacity-planner"></a>Lähetä tiedot kapasiteetin suunnittelu

1.  Kun avaat laskentataulukon **Capacity Planner** se lisätään määrittämiesi asetusten perusteella. Sanan **Infra syötteiden lähde** osoittamaan, että syötteen **Työmäärää pätevyys** laskentataulukon solussa näkyy "Työmäärää". 
2.  Jos haluat tehdä muutoksia, sinun on Muokkaa **Työmäärää pätevyys** laskentataulukko ja valitse suunnittelu-työkalu Lähetä tiedot uudelleen.  

    ![Kapasiteetin suunnittelu](./media/site-recovery-capacity-planner/capacity-planner.png)


