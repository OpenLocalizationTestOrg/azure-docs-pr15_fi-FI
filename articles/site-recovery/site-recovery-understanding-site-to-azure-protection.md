<properties
    pageTitle="Azure palauttaminen Hyper-V replikointia | Microsoft Azure"
    description="Tämän artikkelin avulla voit selvittää tekniset käsitteestä, jotka auttavat onnistuneesti asentaminen, määrittäminen ja hallinta Azure palauttaminen."
    services="site-recovery"
    documentationCenter=""
    authors="Rajani-Janaki-Ram"
    manager="mkjain"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/12/2016"
    ms.author="rajanaki"/>  


# <a name="hyper-v-replication-with-azure-site-recovery"></a>Hyper-V replikointia Azure sivuston palauttaminen

Tässä artikkelissa kuvataan tekninen käsitteestä, jotka helpottavat onnistuneesti määrittäminen ja hallinta Hyper-V-sivuston tai System Center Virtual Machine Manager (VMM) sivuston Azure suojaus Azure palauttaminen avulla.

## <a name="setting-up-the-source-environment-for-replication-between-on-premises-and-azure"></a>Replikoinnin paikallisen ja Azure lähde-ympäristön määrittäminen

Osana palauttaminen paikallisen ja Azure välillä on määritetty muista ladata ja asentaa Azure sivuston palauttaminen tarjoajan VMM palvelimeen. Asenna Azure palautus Services agentti kunkin Hyper-V isännän.

![Replikoinnin paikallisen ja Azure VMM käyttöönotto](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Hyper-V hallitun infrastruktuurin lähde-ympäristö on määritetty muistuttaa VMM hallitun infrastruktuurin lähde-ympäristön luomiseen. Ainoa ero on, että palvelu ja agentti on asennettu Hyper-V isännän itse. VMM-ympäristössä palvelu on asennettu VMM palvelimessa ja agentti on asennettu Hyper-V-isäntien (Jos Azure replikoinnin).

## <a name="workflows"></a>Työnkulut

### <a name="enable-protection"></a>Ota suojaus käyttöön
Kun suojaat virtual machine Azure portal tai paikallisen, **Ota suojaus käyttöön** -niminen palauttaminen työn käynnistyy. Voit valvoa sitä **työt** -välilehdessä.

!["Ota suojaus käyttöön" projektin työt-luettelosta](media/site-recovery-understanding-site-to-azure-protection/image001.PNG)

**Ota suojaus käyttöön** -työ tarkistaa edellytykset ennen [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) menetelmän käynnistämistä. Tämä menetelmä luo Azure-replikoinnin käyttämällä syötteitä, jotka on määritetty suojaus aikana.

**Ota suojaus käyttöön** -työ käynnistyy ensimmäisen replikoinnin paikallisen kutsumalla [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) -menetelmää. Tämä menetelmä lähettää virtual machine virtual levyjen Azure.

!["Ota suojaus käyttöön" projektin tiedot](media/site-recovery-understanding-site-to-azure-protection/IMAGE002.PNG)

### <a name="finalize-protection-on-the-virtual-machine"></a>Suojauksen virtuaalikoneen viimeisteleminen
[Hyper-V AM tilannevedoksen](https://technet.microsoft.com/library/dd560637.aspx) otetaan, kun ensimmäisen replikoinnin käynnistyy. Virtual kiintolevyillä ovat käsitellyt yksi kerrallaan, kunnes kaikki levyjen ladataan Azure. Tämä yleensä kestää jonkin aikaa lopuksi levyn kokoa ja kaistanleveyden perusteella. Optimoi verkon käyttö-kohdassa [paikalliseen, Azure suojaus verkon kaistanleveyden käytön hallinnasta](https://support.microsoft.com/kb/3056159).

Ensimmäisen replikoinnin päätyttyä **Viimeistele suojauksen virtuaalikoneen** työn määrittää verkko- ja jälkeinen replikoinnin asetuksia. Kun ensimmäisen replikoinnin on käynnissä:

- Levyjen kaikkia muutoksia seurataan. 
- Lisätietoja levytilasta kulutettu tilannevedoksen ja Hyper-V replikan Log (HRL)-tiedostoja.

Ensimmäisen replikoinnin valmiiksi Hyper-V AM tilannevedoksen poistetaan. Tämä poistaminen johtaa tietojen muutosten yhdistäminen jälkeen ensimmäisen replikoinnin pääkohde-levylle.

!["Viimeisteleminen virtuaalikoneen suojauksen" projektin työt-luettelosta](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### <a name="delta-replication"></a>Delta sallittuja
Hyper-V replikan replikoinnin seuranta, joka on osa Hyper-V replikan replikoinnin hakukone, jäljittää muutokset virtual kiintolevylle Hyper-V replikan Log (*.hrl)-tiedostoina. HRL tiedostot ovat samassa kansiossa kuin liittyvän levyjä.

Kunkin levyn, joka on määritetty replikoinnin sisältää siihen liittyvän HRL-tiedoston. Tallennustilan asiakastilin lähetetään lokia ensimmäisen replikoinnin päätyttyä. Kun lokiin on Azure siirrossa, ensisijainen muutoksia seurataan toisen lokitiedoston samassa kansiossa.

Ensimmäisen replikoinnin tai delta replikoinnin aikana voit valvoa AM replikoinnin kunto AM-näkymän [replikoinnin kunnon valvonta virtuaalikoneen](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)mainittu.  

### <a name="resynchronization"></a>Uudelleensynkronointi
Virtual machine on merkitty uudelleensynkronointi, kun delta replikointi epäonnistuu sekä alustava replikointi on kallista kaistanleveys tai aika. Esimerkiksi kun HRL tiedostokoko yhdistelmässä enintään 50 prosenttia levyn koon, virtuaalikoneen merkitään uudelleensynkronointi varten. Uudelleensynkronointi mahdollisimman vähän tiedot lähetetään verkon kautta tietojenkäsittely tarkistussummat lähde- ja virtuaalikoneen levyjä ja lähettämällä vain eroa.

Kun uudelleensynkronointi on tehty, Normaali delta replikoinnin pitäisi jatkaa. Voit jatkaa uudelleensynkronointi verkon käyttökatkosta tai toisen käyttökatkosta yhteydessä.

Oletusarvon mukaan ajoitetussa uudelleensynkronointi on määritetty tehtävä ulkopuolisten työtuntien. Jos virtuaalikoneen uudelleen manuaalisesti synkronoituina, valitse virtuaalikoneen-portaalista ja valitse **Synkronoi**.

![Manuaalinen uudelleensynkronointi](media/site-recovery-understanding-site-to-azure-protection/image04.png)

Uudelleensynkronointi käyttää kiinteä estä paloittelun algoritmin missä lähde-ja jaetaan kiinteä näkyvissä. Jokainen lohko tarkistussummat luodaan ja sitten verrattuna määrittääksesi, joka estää lähteestä on käytetty kohteeseen.

### <a name="retry-logic"></a>Yritä logiikka
Tällä valmiin uudelleen logiikan replikoinnin virheet. Tämä logiikka voidaan luokitella kahteen luokkaan:

| Luokka                  | Skenaariot                                    |
|---------------------------|----------------------------------------------|
| Ei palautettavissa virhe     | Yritetään ei uudelleen. Virtuaalikoneen replikoinnin tila on **Kriittinen**ja järjestelmänvalvojan toimia ei tarvita. Esimerkkejä: <ul><li>Katkennut Näennäiskiintolevyn ketju</li><li>Replikan virtuaalikoneen virheellisessä tilassa</li><li>Verkon todennus-virhe</li><li>Todennus-virhe</li><li>Virtual machine, joka ei löydy, kyseessä yksittäinen Hyper-V-palvelin</li></ul>|
| Ratkaistavissa oleva virhe         | Uudelleenyritykset ilmetä jokaisen replikoinnin väli Eksponentiaalinen Edellinen-käytöstä, joka Lisää väli alusta alkaen ensimmäisellä kerralla (1, 2, 4, 8, 10 minuuttia) avulla. Jos virhe toistuu, yritä 30 minuutin välein. Esimerkkejä: <ul><li>Verkkovirhe</li><li>Vähän levytilaa</li><li>Muisti ehto</li></ul>|

## <a name="hyper-v-virtual-machine-protection-and-recovery-life-cycle"></a>Hyper-V virtuaalikoneen ja palauttamisesta elinkaari

![Hyper-V virtuaalikoneen ja palauttamisesta elinkaari](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## <a name="other-references"></a>Muut viittaukset

- [Valvo ja suojauksen VMware VMM, Hyper-V ja fyysisten sivustojen vianmääritys](./site-recovery-monitoring-and-troubleshooting.md)
- [Microsoft Support tavoittelevat](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [Yleisiä Azure palauttaminen virheitä ja niiden ratkaisuja](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
