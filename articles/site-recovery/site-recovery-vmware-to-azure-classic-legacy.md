<properties
    pageTitle="Replikoida VMware näennäiskoneiden ja fyysiset palvelimet Azure kanssa Azure sivuston palauttaminen (vanha) | Microsoft Azure" 
    description="Kerrotaan, miten voit toistaa paikallisen VMs ja Windows-tai Linux Azure fyysinen palvelinten käyttäminen Azure palauttaminen vanha käyttöönoton perinteinen-portaalissa." 
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="raynew"/>

# <a name="replicate-vmware-virtual-machines-and-physical-servers-to-azure-with-azure-site-recovery-using-the-classic-portal-legacy"></a>Toistaa VMware näennäiskoneiden ja fyysiset palvelimet Azure kanssa Azure palauttaminen perinteinen portaalissa (vanha)

> [AZURE.SELECTOR]
- [Azure Portal](site-recovery-vmware-to-azure.md)
- [Perinteinen Portal](site-recovery-vmware-to-azure-classic.md)
- [Perinteinen Portal (vanha)](site-recovery-vmware-to-azure-classic-legacy.md)


Tervetuloa käyttämään Azure palauttaminen! Tässä artikkelissa kuvataan aiempien versioiden käyttöönottoa varten replikoiminen paikallisen VMware näennäiskoneiden tai Windows-tai Linux fyysinen palvelimia Azure käyttämällä Azure sivuston perinteinen-portaalissa.

## <a name="overview"></a>Yleiskatsaus

Organisaatioiden on BCDR strategia, joka määrittää, miten sovellukset ja työmääriä pysy käynnissä ja käytettävissä aikana suunnitellut ja suunnittelemattomat käyttökatkot ja palauttaa normaalijakauman käytön edellytykset mahdollisimman pian. BCDR strategia kannattaa säilyttäminen yritystietojen turvassa ja palautettavissa ja varmistetaan, että työmääriä pysyvät jatkuvasti käytettävissä, kun tietojen tapahtuu.

Sivuston palautus on Azure-palvelu prosentuaalista osuutta BCDR strategia mukaan orchestrating replikoinnin paikallisen fyysiset palvelimet ja näennäiskoneiden pilveen (Azure) tai toissijainen palvelinkeskukseen. Kun katkokset ensisijainen sijainti-asiakas ei päälle toissijainen sijainnin vastaamaan itsellesi sovellukset ja toiminnoista, jotka ovat käytettävissä. Asiakas ei takaisin ensisijainen sijainti, kun se palauttaa normaalijakauman toimintoihin. Lisätietoja [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)


>[AZURE.WARNING] Tässä artikkelissa on **Vanha ohjeita**. Älä käytä sen uuden käyttöönotoissa. Sen sijaan, [Noudata seuraavia ohjeita](site-recovery-vmware-to-azure.md) sivustojen palauttamisen käyttöönotto Azure portal tai voit määrittää parannettu käyttöönoton perinteinen portaalin [ohjeiden avulla](site-recovery-vmware-to-azure-classic.md) . Jos olet jo asentanut tässä artikkelissa kuvatulla, on suositeltavaa siirtäminen parannettu käyttöönoton perinteinen-portaalissa.


## <a name="migrate-to-the-enhanced-deployment"></a>Parannettu käyttöönoton siirtäminen

Tässä osassa on vain, jos olet jo asentanut palauttaminen tämän artikkelin ohjeiden mukaisesti.

Voit siirtää olemassa olevat käyttöönoton sinun on:

1. Ottaa käyttöön uuden paikallisen sivuston palauttaminen osat.
2. Määritä automaattinen etsiminen VMware VMs tunnistetietojen uusi määrityspalvelimen.
3. Tutustu VMware palvelimet uusi määrityspalvelimen kanssa.
3. Luo uusi suojaus-ryhmä uusi määrityspalvelimen kanssa.


Ennen aloittamista:

- On suositeltavaa määrittäminen ylläpito-ikkuna, jossa siirto.
- **Siirtää koneet** -asetus on käytettävissä vain, jos sinulla on olemassa olevia suojaus-ryhmiä, jotka on luotu vanha käyttöönoton aikana.
- Kun olet suorittanut siirron vaiheet voi kestää 15 minuuttia tai kauemmin tunnistetietoja, päivittäminen ja tutustu ja Päivitä näennäiskoneiden, jotta voit lisätä ne suojaus-ryhmä. Voit päivittää manuaalisesti odottaa sijaan. 

Siirtää seuraavasti:

1. Lisätietoja on [parannettu käyttöönoton perinteinen-portaalissa](site-recovery-vmware-to-azure-classic.md#enhanced-deployment). Tarkista parannettu [arkkitehtuuri](site-recovery-vmware-to-azure-classic.md#scenario-architecture)ja [edellytykset](site-recovery-vmware-to-azure-classic.md#before-you-start-deployment).
2. Poista Mobility-palvelu on tällä hetkellä replikoiminen koneet. Palvelun uusi versio asennetaan tietokoneissa, kun lisäät ne uuteen suojaus-ryhmä.
3. Lataa [säilö rekisteröinti avain](site-recovery-vmware-to-azure-classic.md#step-4-download-a-vault-registration-key) ja [Suorita ohjattu määritystoiminto yhdistetty](site-recovery-vmware-to-azure-classic.md#step-5-install-the-management-server) määrityspalvelimen, prosessi-palvelin ja palvelimen perusmuodon osia asentamiseen. Lisätietoja [kapasiteetin suunnittelua](site-recovery-vmware-to-azure-classic.md#capacity-planning).
4. [Määritä tunnistetiedot](site-recovery-vmware-to-azure-classic.md#step-6-set-up-credentials-for-the-vcenter-server) kyseisen sivuston palauttaminen avulla voit käyttää VMware palvelimen tuttuihin VMware VMs automaattisesti. Lisätietoja [tarvittavista käyttöoikeuksista](site-recovery-vmware-to-azure-classic.md#vmware-permissions-for-vcenter-access).
5. Lisää [vCenter palvelimia tai vSphere isännät](site-recovery-vmware-to-azure-classic.md#step-7-add-vcenter-servers-and-esxi-hosts). Voi kestää 15 minuuttia, lisää palvelinten näkyvän sivuston palautus-portaalissa.
6. Luo [Uusi suojaus-ryhmä](site-recovery-vmware-to-azure-classic.md#step-8-create-a-protection-group). Voi kestää niin, että näennäiskoneiden löydetään ja näkyvät päivittämiseen portaalin 15 minuutin kuluttua. Jos et halua odottaa voit korostaa hallinnan palvelimen nimi (Älä valitse) > **Päivitä**.
7. Valitse uusi suojaus-ryhmä **Siirtää koneet**.

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration1.png)

8. Valitse **Valitse koneet** suojaus-ryhmä, jonka haluat siirtää, ja koneet, jotka haluat siirtää.

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration2.png)

9. Määritä **Target asetusten määrittäminen** , haluatko käyttää samoja asetuksia kaikissa tietokoneissa ja valitse prosessi-palvelin ja Azure-tallennustilan tilin. Jos sinulla ei ole erillinen prosessi palvelimen tämä on määrityspalvelimen palvelimen IP-osoite.


    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration3.png)

    > [AZURE.NOTE] [Siirron tallennustilan tilien](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue tallennustilan tilejä sivustojen palauttamisen käyttöönotto.

10. **Määritä tilejä**Valitse tili, jonka loit prosessin palvelimen käyttämään koneen push Mobility-palvelun uuden version.

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration4.png)

11. Sivuston palauttaminen siirtää replikoitua tietojen Azure tallennustilan tilille, jonka ilmoitit. Voit myös käyttää käytit vanha käyttöönoton tallennustilan tilin.
12. Projektin päätyttyä näennäiskoneiden Synkronoi automaattisesti. Synkronoinnin päätyttyä näennäiskoneiden voit poistaa vanhoja suojaus-ryhmä.
13. Kun olet siirtänyt kaikissa tietokoneissa voit poistaa vanhoja suojaus-ryhmä.
14. Muista määrittää automaattisesti ominaisuuksien koneet ja Azure verkkoasetukset synkronoinnin jälkeen.
15. Jos sinulla on aiemmin palautus-palvelupakettia, voit siirtää niitä parannettu-ympäristö, jonka **Siirtää palautus suunnittelu** -vaihtoehto. Vain muuta tämän jälkeen kaikissa suojatun tietokoneissa on siirretty. 

    ![Tilin lisääminen](./media/site-recovery-vmware-to-azure-classic-legacy/legacy-migration5.png)

>[AZURE.NOTE] Kun olet lisännyt siirron [parannettu artikkelissa](site-recovery-vmware-to-azure-classic.md)Jatka. Loput vanha sisältö ei ole enää asiaa ja sinun ei tarvitse enää it ** kuvattujen vaiheiden seuraaminen.




## <a name="what-do-i-need"></a>Mitä minun tulee?

Tässä kaaviossa on esitetty käyttöönotto-osat.

![Uusi säilö](./media/site-recovery-vmware-to-azure-classic-legacy/architecture.png)

Näin käytön edellytykset:

**Osa** | **Käyttöönotto** | **Tiedot**
--- | --- | ---
**Määrityspalvelimen** | Azure vakio A3 virtual tämän tietokoneen palauttaminen saman tilauksen. | Configuration-palvelin koordinoi välisen suojatun koneet, prosessi-palvelin ja Azure perusmuodon kohde-palvelimet. Se määrittää replikoinnin ja Azure koordinaatit palautuksen automaattisesti yhteydessä.
**Perusmuodon palvelimesta** | Azure virtuaalikoneen – joko Windows server Windows Server 2012 R2-valikoiman kuva (Windows-tietokoneissa suojaamiseen) tai Linux palvelimeksi perusteella OpenLogic CentOS 6.6-valikoiman kuva (suojaamiseen Linux koneet).<br/><br/> Kolme koonmuuttokahvan asetukset ovat käytettävissä – vakio A4, vakio D14 ja standardin DS4.<br/><br/> Palvelin on yhdistetty samaan Azure verkon määritys-palvelimeksi.<br/><br/> Sivuston palauttaminen-portaalin määrittäminen | Se saa ilmoituksen ja säilyttää oman suojatun koneet liitetty näennäiskiintolevyjen Blob-objektien tallennustilaan Azure tallennustilan tilisi on luotu käyttämällä replikoitua tiedot.<br/><br/> Valitse vakio DS4 erityisesti määrittämiseen suojauksen työmääriä yhdenmukaisia erinomainen suorituskyky ja lyhyt odotusaika Premium-tallennustilan tilin avulla.
**Prosessi-palvelin** | Käytössä Windows Server 2012 R2-paikallisen virtual tai fyysinen palvelimen<br/><br/> Suosittelemme, että se sijoitetaan samassa verkossa ja Lähiverkon segmenttiä koneet, jonka haluat suojata, mutta se voidaan suorittaa eri verkossa, kunhan suojatun tietokoneissa on tason 3 verkon näkyvyyden siihen.<br/><br/> Määrittämällä ja rekisteröitävä se määrityspalvelimen sivuston palautus-portaalissa. | Suojatun koneet replikoinnin tietojen lähettäminen on paikallinen prosessi-palvelin. Siinä on levylle välimuisti-välimuistin replikoinnin tiedot, jotka se saa. Se on suorittanut toimintojen määrä, jonka tietotyypiksi.<br/><br/> Optimoi tietojen tallentaminen välimuistiin, pakkaaminen ja salaaminen ennen sen lähettämistä perusmuodon kohde-palvelimeen.<br/><br/> Se käsittelee push asennuksen Mobility-palvelun.<br/><br/> Se on suorittanut VMware näennäiskoneiden Automaattinen etsiminen.
**Paikallisen koneet** | Paikallisen VMware näennäiskoneiden tai fyysinen Windows- tai Linux-palvelimiin. | Voit määrittää Replikointiasetukset, joka käyttää vähintään. Voit voi epäonnistua yksittäisiä tietokoneen päällä tai yleensä useita koneet, voit kerätä yhteen yhdeksi palautus suunnitelma. 
**Mobility-palvelu** | Kunkin virtuaalikoneen tai haluat suojata fyysisesti palvelimelle asennettu<br/><br/> Voidaan asentaa manuaalisesti tai miten ja asentaa automaattisesti prosessi-palvelin kun otat replikoinnin tietokoneelle. | Mobility-palvelu lähettää tietoja prosessin palvelimeen aikana ensimmäisen replikoinnin (Synkronoi uudelleen). Kun tietokone on suojattu tila (päätyttyä Synkronoi uudelleen) Mobility-palvelun sieppaa kirjoituksia levyn muistissa ja lähettää ne prosessi-palvelimeen. Windows-palvelimien Applicationconsistency saavutetaan projektinimi käyttäminen
**Azure palauttaminen säilöön** | Voit luoda sivuston palautus-säilö Azure-tilauksen ja rekisteröi palvelinten säilö. | Säilö koordinoi ja orchestrates tietojen replikoinnin automaattisesti ja palautus paikallisen sivuston ja Azure välillä.
**Replikoinnin järjestelmä** | **Päälle Internet**– suojatun paikallisten palvelimien kommunikoi ja rinnakkaisnäytteitä tiedoilla käyttämällä suojattu SSL/TLS Azure kanavan Internetin välityksellä. Tämä on oletusarvo.<br/><br/> **VPN/ExpressRoute**– kommunikoi ja rinnakkaisnäytteitä paikallisten palvelimien ja tietojen Azure VPN-yhteyden kautta. Sinun on sivuston sivuston VPN- tai ExpressRoute välisessä paikallisen sivuston ja Azure verkon määrittäminen.<br/><br/> Voit valita miten haluat toistaa palauttaminen käyttöönoton aikana. Et voi muuttaa on, sen jälkeen, kun se on määritetty ilman vaikuttavat aiemmin koneet replikoinnin. | Kumpikaan vaihtoehto edellyttää, voit avata minkä tahansa saapuvien verkkoportit suojatun tietokoneissa. Kaikki verkkoliikennettä on käynnistetty paikallisen sivuston. 

## <a name="capacity-planning"></a>Kapasiteetin suunnittelu

Sinun on huomioitavia pääaluetta:

- **Lähde-ympäristössä**, VMware infrastruktuuri, lähteen Laiteasetukset ja vaatimuksia.
- **Osan palvelimet**, prosessi-palvelimeen, määrityspalvelimen ja perusmuodon palvelimesta 

### <a name="considerations-for-the-source-environment"></a>Huomioon otettavia seikkoja lähde-ympäristössä

- **Levyn enimmäiskoko**– levy, johon on liitetty virtual machine nykyinen enimmäiskoko on 1 Teratavua. Näin enimmäiskokoa lähdelevy, joka voi replikoida on myös rajoitettu 1 TT.
- **Enimmäiskoko kohti lähde**– yhteen lähde-koneen enimmäiskoko on 31 TT (kanssa 31 levyjen) ja D14-esiintymän valmisteltu perusmuodon kohde-palvelimen kanssa. 
- **Perusmuodon kohdepalvelinta kohti lähteiden määrä**– useita lähde-tietokoneissa voidaan suojata yksittäisen perusmuodon kohdepalvelimen kanssa. Yhteen lähde-koneen et voi kuitenkin suojattu useita perusmuodon kohde palvelinten, koska kuin levyjen replikoida Näennäiskiintolevyn, joka vastaa levyn koon luodaan Azure-blob-säiliö ja liitteenä tietojen DVD-levyllä perusmuodon kohde-palvelimeen.  
- **Muutos enintään päivittäin lähde korvauksen**– kolme eri tekijät, jotka on otettava huomioon, kun lähteen suositellut muuta korvauksen. Kohde mukaan huomioon otettavia seikkoja kaksi IOPS tarvitaan kutakin toimintoa lähteen kohde-levylle. Tämä johtuu siitä vanha tietojen luku ja kirjoita uudet tiedot tapahtuu kohde-levylle. 
    - **Päivittäinen muuttaminen korko, prosessi-palvelin ei tue**– lähdetietokoneen ei kattavat useita prosessi-palvelimia. Yksittäisen prosessin palvelin tukee enintään 1 TT: n päivittäinen muuta kurssi. Näin ollen 1 TT on suurin päivittäinen tiedot muuttuvat tuetut lähdetietokoneen korko. 
    - **Suurin nopeus tukemat levy**, enintään churn lähde-levyä kohden ei voi olla enintään 144 gt/päivä (jonka 8 K kirjoitus-koko). Katso siirtonopeuden ja IOPs kohteen perusmuodon kohde-osassa taulukkoa varten eri kokoja Kirjoita. Määrä on jaettava kahdella, koska kunkin tietolähteen IOP Luo 2 IOPS kohde-levylle. Lisätietoja [Azure skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) määritettäessä premium-tallennustilan tilin kohde.
    - **Suurin nopeus tallennustilan tilille tukemat**– lähteen et voi kattavat tallennustilan useita tilejä. Valita tallennustilan tilin kestää enintään 20 000 pyynnöt sekunnissa ja kunkin tietolähteen IOP Luo 2 IOPS perusmuodon kohde palvelimessa, on suositeltavaa pidät IOPS määrän lähteen yli 10 000. Lisätietoja [Azure skaalattavuus ja suorituskyvyn kohteet](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts) määritettäessä premium-tallennustilan tilin lähde.

### <a name="considerations-for-component-servers"></a>Osan palvelinten huomioon otettavia seikkoja

Taulukko 1 on yhteenveto virtuaalikoneen koon määrittäminen ja perusmuodon kohde-palvelimiin.

**Osa** | **Käyttöön Azure esiintymiä** | **Sydämiä** | **Muisti** | **Maks-levyjä** | **Levyn koko**
--- | --- | --- | --- | --- | ---
Määrityspalvelimen | Vakio A3 | 4 | 7 GIGATAVUA | 8 | 1023 GT
Perusmuodon palvelimesta | Vakio A4 | 8 | 14 GT | 16 | 1023 GT
 | Vakio D14 | 16 | 112 GT | 32 | 1023 GT
 | Vakio DS4 | 8 | 28 GT | 16 | 1023 GT

**Taulukko 1**

#### <a name="process-server-considerations"></a>Käsittele palvelimen huomioon otettavia seikkoja

Yleensä prosessin palvelimen koonmuuttokahvan määräytyy päivittäinen muuta kurssi yli kaikki suojatun työmäärät.


- Sinun on riittävästi Laske tehtäviä, kuten tekstiin pakkaus ja salausta.
- Prosessi-palvelin käyttää levyn mukaan välimuistiin. Varmista, että suositellut välimuistin välilyönti ja levy on käytettävissä helpottaa tietojen muutokset tallennettu verkon pullonkaula tai käyttökatkosta. 
- Varmista kaistanleveys on riittävä niin, että prosessin palvelimen ladata tiedot perusmuodon kohdepalvelin, joka tarjoaa jatkuvan tietojen suojauksen. 

Taulukon 2 yhteenvedon prosessin palvelimen ohjeiden.

**Tietojen muuttaminen korko** | **SUORITIN** | **Muisti** | **Levyn välimuisti**| **Välimuistin levyn siirtonopeuden** | **Kaistanleveyden tunkeutumisen/juniin**
--- | --- | --- | --- | --- | ---
< 300 gt | 4 vCPUs (2 sockets * 2 sydämiä @ 2,5 GHz) | VÄHINTÄÄN 4 GT | 600 GT | 7-10 Mt sekunnissa | 30 Mbps/21 Mbps
300 600 gt | 8 vCPUs (2 sockets * 4 sydämiä @ 2,5 GHz) | 6 GIGATAVUA | 600 GT | 11-15 Mt sekunnissa | 60 Mbps/42 Mbps
1 TT 600 Gigatavua | 12 vCPUs (2 sockets * 6 sydämiä @ 2,5 GHz) | 8 GIGATAVUN | 600 GT | 16-20 Mt sekunnissa | 100 Mbps/70 Mbps
> 1 TT | Toinen prosessi Serverin käyttöönotto | | | | 

**Taulukon 2**

Jos: 

- Tunkeutumisen on download kaistanleveyden (intranet välillä lähde- ja prosessi-palvelin).
- Juniin on ladattava kaistanleveyden (internet prosessin palvelimen ja perusmuodon kohde). Juniin numerot katsottava keskimääräinen 30 % prosessin palvelimen pakkaus.
- Välimuistin levyn pienin 128 gigatavua erillisessä OS DVD-levyllä suositellaan prosessin palvelimilta.
- Välimuistin levyn siirtonopeuden, seuraavat tallennustilan käytettiin esikuva: 10 K JÄLLEENMYYNTIHINNAN RAID 10 määrityksissä 8 SAS asemat.

#### <a name="configuration-server-considerations"></a>Määritysten palvelimen huomioon otettavia seikkoja

Kunkin määrityspalvelimen tukee enintään 100 lähde-koneet, 3 ja 4 tietomääristä kanssa. Jos käyttöönoton on suurempi Suosittelemme toiseen määrityspalvelimen otetaan käyttöön. Taulukon 1 saat virtuaalikoneen oletusominaisuudet määrityspalvelimen. 

#### <a name="master-target-server-and-storage-account-considerations"></a>Perusmuodon kohde-palvelimen nimen ja tallennustilaa tilin huomioon otettavia seikkoja

Tallennustilan kullekin perustyylisivujen kohde-palvelimelle sisältää OS-levyn, säilytys-asema ja tietojen levyjä. Säilytys-asema ylläpitää levyn muutokset lehdessä toistaminen määritetty sivuston palautus-portaalissa säilytys-ikkuna.  Taulukko 1 viitata virtuaalikoneen ominaisuuksien perusmuodon palvelimesta. Taulukon 3 näyttää, miten A4 levyjä käytetään.

**Esiintymän** | **OS levy** | **Säilytys** | **Tietoja levyjen**
--- | --- | --- | ---
 | | **Säilytys** | **Tietoja levyjen**
Vakio A4 | 1-levy (1 * 1023 gt) | 1-levy (1 * 1023 gt) | 15 levyjen (15 * 1023 gt)
Vakio D14 |  1-levy (1 * 1023 gt) | 1-levy (1 * 1023 gt) | 31 levyjen (15 * 1023 gt)
Vakio DS4 |  1-levy (1 * 1023 gt) | 1-levy (1 * 1023 gt) | 15 levyjen (15 * 1023 gt)

**Taulukon 3**

Kapasiteetin suunnittelu perusmuodon kohde palvelimen määräytyy:

- Azure-tallennustilan suorituskyky ja rajoitukset
    - Enimmäismäärä on erittäin käyttää levyjen vakio taso-AM, on noin 40 (20 000/500 IOPS levyä kohden) yksittäisen tallennustilaa tilillä. Lue tietoja [Vakio tallennustilan sccounts skaalattavuus kohteet](../storage/storage-scalability-targets.md#scalability-targets-for-standard-storage-accounts) ja [premium tallennustilan sccounts](../storage/storage-scalability-targets.md#scalability-targets-for-premium-storage-accounts).
-   Muuta päivittäin korko 
-   Säilytys-asema tallennuspaikka.

Huomaa, että:

- Yksi lähde ei voi olla usean tallennustilan tilin. Tämä koskee tietojen levylle, valitse valittuna, kun määrität suojaus tallennustilan tilit. Siirry automaattisesti käyttöön tallennustilan tilin yleensä käyttöjärjestelmän ja säilytys-levyjä.
- Pakollinen säilytys tallennustilan äänenvoimakkuuden määräytyy päivittäinen muuta kurssi ja säilytys päivien määrän. Perusmuodon kohdepalvelinta kohti säilytys muistitilaa = yhteensä churn lähteestä päivässä * säilytys päivien lukumäärä. 
- Kunkin perustyylin kohde-palvelimeen on vain yksi säilytys-asema. Säilytys-asema jaetaan perusmuodon palvelimesta yhdistetty levyjä. Esimerkki:
    - Jos kunkin levyn Luo 120 IOPS (8K koon) lähteen ei lähde tietokoneessa, jossa 5 levyjä, tämä vastaa 240 IOPS levylle (2 toimenpiteet levy lähde IO kohden). 240 IOPS on Azure levyn IOPS raja 500 kohden.
    - Säilytys-asemassa tämä on 120 * 5 = 600 IOPS ja tämä voi tulla pullot kaulan. Tässä skenaariossa hyvä strategia olisi enemmän levyjä säilytys-asema ja kattavat sitä kautta, kuin RAID raita-määritys. Tämä parantaa suorituskykyä, koska IOPS jakautuu useita asemat. Asemat säilytys äänenvoimakkuuden lisättäväksi määrä on seuraavasti:
        - Yhteensä IOPS lähde-ympäristöstä / 500
        - Yhteensä churn päivässä lähde-ympäristöstä (pakkaamattomat) / 287 Gigatavua. 287 gt on suurin nopeus tukemat kohde DVD-levyllä päivässä. Tämä arvo vaihtelee kerrointa 8K, kirjoita muuta kokoa koska tällöin 8K on olemassa oletetaan kirjoittaminen kokoa. Esimerkiksi jos kirjoitus on 4K kohteen siirtonopeuden tulee 287/2. Ja jos kirjoittaminen koko on 16K sitten siirtonopeuden 287 * 2.
- Numero pakollinen tallennustilan tilien = lähde yhteensä IOPs/10000.


## <a name="before-you-start"></a>Ennen aloittamista

**Osa** | **Vaatimukset** | **Tiedot**
--- | --- | --- 
**Azure-tili** | Tarvitset [Microsoft Azure](https://azure.microsoft.com/) -tiliin. Voit aloittaa [maksuttoman kokeiluversion käyttäjäksi](https://azure.microsoft.com/pricing/free-trial/).
**Azure-tallennustilan** | Sinun on Azure-tallennustilan tilin replikoitua tietojen tallentamiseen<br/><br/> Joko tilin on oltava [Normaali Geo tarpeettomat tallennustilan-tili](../storage/storage-redundancy.md#geo-redundant-storage) tai [Premium-tallennustilan tilin](../storage/storage-premium-storage.md).<br/><br/> Saman alueen Azure palauttaminen-palvelu on, ja saman tilauksen yhdistetä. Siirrä tallennustilan tilien luotu [uuden Azure portaalin](../storage/storage-create-storage-account.md) resurssin ryhmien ei tueta.<br/><br/> Lue lisätietoja [Johdanto Microsoft Azuren tallennustilaan](../storage/storage-introduction.md)
**Azure virtual verkossa** | Tarvitset Azure virtual verkon joina määrityspalvelimen ja perusmuodon kohdepalvelimen otetaan käyttöön. Olisi sama tilaus ja alue kuin Azure palauttaminen säilö. Jos haluat kopioida tiedot ExpressRoute tai VPN-yhteyden Azure virtual verkon on yhdistettävä paikalliseen verkkoon ExpressRoute-yhteyden tai sivuston sivuston VPN-yhteyttä.
**Azure-resurssit** | Varmista, että sinulla on tarpeeksi Azure resursseja Ota käyttöön kaikki komponentit. Lue lisää tekstimuodossa [Azure tilauksen rajoitukset](../azure-subscription-service-limits.md).
**Azure-virtuaalikoneissa** | Suojattava näennäiskoneiden on mukaisia [Azure edellytykset](site-recovery-best-practices.md).<br/><br/> **Levyn Laske**– 31 levyjen enintään tukee yhteen suojatun palvelimeen<br/><br/> **Levyn koot**– yksittäisiä levyn kapasiteetti ei kannata olla yli 1023 Gigatavua<br/><br/> **Clustering**– liitetty palvelinten ei tueta<br/><br/> **Käynnistyksen**– yhdistetyn Extensible laitteisto Interface (UEFI) / Extensible laitteisto Interface(EFI) uudelleenkäynnistys ei tueta<br/><br/> **Asemat**– Bitlocker salattuja tietomääristä ei tueta<br/><br/> **Palvelinten nimet**, nimissä saa olla 1 – 63 merkkiä (kirjaimia, numeroita ja väliviivoja). Nimi on alkaa kirjaimen tai numeron ja kirjaimen tai numeron perässä. Kun kone suojataan voit muuttaa Azure nimi.
**Määrityspalvelimen** | Vakio Azure sivuston palauttaminen Windows Server 2012 R2-valikoiman kuva perusteella A3 virtuaalikoneen luodaan configuration-palvelin-tilaukseesi. Se luodaan uusi pilvipalvelussa ensimmäisen esiintymän. Jos tyypiksi julkinen Internet-yhteys määritys-palvelimen pilvipalvelussa sijaitsevaan luodaan varatut julkiseen IP-osoite.<br/><br/> Asennuspolku pitäisi olla vain englannin kielen merkkejä.
**Perusmuodon palvelimesta** | Virtual-koneen Azure-standardin A4, D14 tai DS4.<br/><br/> Asennuspolku pitäisi olla vain englannin kielen merkkejä. Esimerkiksi polku on oltava **/usr/local/ASR** Linux-perusmuodon kohde-palvelimesta.
**Prosessi-palvelin** | Voit ottaa käyttöön prosessin palvelimen fyysisiä tai virtual tietokoneen käyttöjärjestelmä on Windows Server 2012 R2: n uusimmat päivitykset. C: asentaminen /.<br/><br/> Suosittelemme, että voit sijoittaa palvelimen samassa verkossa ja aliverkon koneet, jonka haluat suojata.<br/><br/> Asenna VMware vSphere CLI 5.5.0 prosessi-palvelimeen. VMware vSphere CLI osan vaaditaan prosessi-palvelimeen, jotta näennäiskoneiden hallitsee vCenter palvelimeen tai ESXi isännän näennäiskoneiden.<br/><br/> Asennuspolku pitäisi olla vain englannin kielen merkkejä.<br/><br/> ReFS tiedostojärjestelmässä ei tueta.
**VMware** | VMware vCenter palvelin, VMware vSphere-hypervisors hallinta. Se on suoritettava vCenter versio 5.1 tai 5.5 ja viimeisimmät päivitykset.<br/><br/> Yhden tai useamman vSphere hypervisors sisältävä VMware näennäiskoneiden, jonka haluat suojata. Hypervisor suoritetaan ESX/ESXi versio 5.1 tai 5.5 ja viimeisimmät päivitykset.<br/><br/> VMware näennäiskoneiden on oltava asennettuna ja käynnissä VMware työkalut. 
**Windows-tietokoneissa** | Suojatun fyysinen palvelimia tai VMware näennäiskoneiden Windows on vaatimukset määrä.<br/><br/> A tueta 64-bittinen käyttöjärjestelmä: **Windows Server 2012 R2**, **Windows Server 2012**- tai **Windows Server 2008 R2 ja milloin vähintään SP1**.<br/><br/> Isäntänimi, liityntäkohdat, laite nimet, Windowsin järjestelmäpolku (esimerkiksi: C:\Windows) olisi vain englannin kielellä.<br/><br/> Käyttöjärjestelmä on asennettava C:\ asemaan.<br/><br/> Tuetaan vain basic levyjä. Dynaamisiksi ei tueta.<br/><br/> Palomuurisäännöt suojatun tietokoneissa avulla he voivat saavuttaa Azure.p määritys ja perustyyli kohde palvelimissa ><p>Sinun on annettava järjestelmänvalvojatilin (paikallisen järjestelmänvalvojan Windows-tietokoneessa on oltava) push asentaa Mobility-palvelu Windows-palvelimiin. Jos sinun on käyttäjän etäkäyttöpalvelimen ohjausobjektin paikallisessa tietokoneessa käytöstä toimialue tili on annettu tili. Voit tehdä tämän Lisää LocalAccountTokenFilterPolicy DWORD-rekisterimerkinnän arvo on 1, valitse HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Lisää rekisterimerkintä CLI Avaa cmd tai powershell ja lisää **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. [Lue lisää](https://msdn.microsoft.com/library/aa826699.aspx) siitä, että käyttöoikeuksien hallinta.<br/><br/> Jälkeen automaattisesti, jos haluat yhteyden muodostaminen Windows Azure-tietokannassa etätyöpöydän kanssa näennäiskoneiden Varmista, että Etätyöpöytä on käytössä paikallisen tietokoneen. Jos olet muodostamassa ei VPN-verkon kautta, palomuurisäännöt olisi Salli etätyöpöydän yhteydet Internetin välityksellä.
**Linux koneet** | Tuetut 64-bittinen käyttöjärjestelmä: **6.4, 6.5, 6.6 Centos**; **Oracle yrityksen Linux 6.4 6.5, jossa punainen on yhteensopiva ydin tai Unbreakable yrityksen ydin versio 3 (UEK3)**, **SUSE Linux Enterprise Server 11 SP3**.<br/><br/> Palomuurisäännöt suojatun tietokoneissa avulla he voivat määritys ja Azure perusmuodon kohde-palvelimet.<br/><br/> suojatun tietokoneissa /ETC/Hosts tiedostojen pitäisi näkyä tapahtumat, jotka vastaavat paikallisen isäntänimi kaikki NIC liittyvät IP-osoitteet <br/><br/> Jos haluat muodostaa yhteyden Azure virtual-tietokoneessa, jossa Linux automaattisesti suojattu runko Client-asiakkaalla (ssh) jälkeen, varmista, että suojatun tietokoneen suojattu runko-palvelu on määritetty alkamaan automaattisesti järjestelmän käynnistyksen ja palomuurisäännöt salli ssh yhteyden siihen.<br/><br/> Isäntänimi, liityntäkohdat, laite nimet ja Linux järjestelmän polut ja tiedostonimet (esimerkiksi/jne /; /usr) on oltava englanniksi vain.<br/><br/> Suojauksen voi ottaa käyttöön seuraavat tallentamiseen koneet paikallisen:-<br>Tiedostojärjestelmän: EXT3 ETX4, ReiserFS, XFS<br>Ohjelmiston monipolku laitteilla Mapper (monipolku)<br>Levyn hallinta: LVM2<br>HP CCISS ohjauskoneen tallennustilan fyysinen palvelinten ei tueta.
**Kolmannen osapuolen** | Tässä skenaariossa käyttöönoton osia määräytyvät kolmannen osapuolen ohjelmisto toimii oikein. Täydellinen luettelo on [kolmannen osapuolen ohjelmiston ilmoituksia ja tietoja](#third-party)


### <a name="network-connectivity"></a>Verkkoyhteyden

Käytettävissä on kaksi vaihtoehtoista määritettäessä verkkoyhteyden paikallisen sivuston ja Azure virtual verkossa, jossa infrastruktuuri-osat (määrityspalvelimen, master kohde-palvelimet) on otettu käyttöön. Sinun on tulostusvaihtoehdon verkon yhteys käytetään ennen niiden käyttöönottoa configuration-palvelin. Sinun on valita tämän asetuksen käyttöönoton yhteydessä. Se ei voi muuttaa myöhemmin.

**Internet:** Viestintä- ja paikallisen palvelimet (prosessin palvelimeen, suojattu koneet) ja Azure infrastruktuurin osan-palvelimiin (määrityspalvelimen ja perusmuodon kohde server) tietojen replikoinnin tapahtuu päälle suojattu SSL/TLS yhteyden paikallisen julkisen päätepisteet määritys ja perustyyli kohde-palvelimiin. (Ainoa poikkeus on prosessi-palvelin ja perusmuodon kohde palvelimeen TCP-portin 9080 salaamaton eli välinen yhteys. Vain ohjausobjektin liittyvät replikoinnin protokolla replikoinnin asennuksen projektitietojen tätä yhteyttä.)

![Käyttöönoton kaavio internet](./media/site-recovery-vmware-to-azure-classic-legacy/internet-deployment.png)

**VPN**: viestintä- ja paikallisen palvelimet (prosessin palvelimeen, suojattu koneet) ja Azure infrastruktuurin osan-palvelimiin (määrityspalvelimen ja perusmuodon kohde server) tietojen replikoinnin tapahtuu paikallisen verkon ja Azure virtual verkossa, jossa määrityspalvelimen ja perusmuodon kohdepalvelinten otetaan käyttöön VPN-yhteyden kautta. Varmista, paikallisen verkon on kytketty Azure virtual verkkoon ExpressRoute-yhteyden tai sivuston sivuston VPN-yhteyden.

![Käyttökaavio VPN](./media/site-recovery-vmware-to-azure-classic-legacy/vpn-deployment.png)


## <a name="step-1-create-a-vault"></a>Vaihe 1: Luo säilöön

1. Kirjautuminen [hallinta-portaalin](https://portal.azure.com).


2. Laajenna- **Tietopalvelut** > **Palautus Services** ja sitten **Sivuston palauttaminen säilö**.


3. Valitse **Luo uusi** > **nopea luominen**.

4. Kirjoita **nimi**-kenttään kutsumanimi tunnistavan säilö.

5. Valitse **alue**-säilö maantieteellinen alue. Tarkista tuettujen alueiden artikkelissa [Azure sivuston palauttaminen hinnat tiedot](https://azure.microsoft.com/pricing/details/site-recovery/) maantieteelliset käytettävyys

6. Valitse **Luo säilö**.

    ![Uusi säilö](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-create-vault.png)

Valitse Vahvista, että säilö on luotu tilarivillä. Säilö merkitään **aktiiviseksi** pääsivulta **Palautus-palvelut** .

## <a name="step-2-deploy-a-configuration-server"></a>Vaihe 2: Määritys-Serverin käyttöönotto

### <a name="configure-server-settings"></a>Asetusten määrittäminen

1. Valitse Pika-aloitus-sivun avaamiseen säilö **Palautus palveluita** -sivulle. Pikaopas voi avata myös milloin tahansa napsauttamalla kuvaketta.

    ![Pika-aloitusopas-kuvake](./media/site-recovery-vmware-to-azure-classic-legacy/quick-start-icon.png)

2. Valitse avattavasta luettelosta **paikallisen-sivusto, jossa VMware/fyysisiä palvelimia ja Azure välillä**.
3. Valitse **Valmistele Target(Azure) resurssien** **Käyttöön määrityspalvelimen**.

    ![Ottaa käyttöön määrityspalvelimen](./media/site-recovery-vmware-to-azure-classic-legacy/deploy-cs2.png)

4. Määritä **Uusi palvelimen tiedot** :

    - Nimi määrityspalvelimen ja muodostaa siihen tunnistetiedot.
    - Kirjoita verkon connectivity avattava Valitse **Julkinen Internet** - tai **VPN-yhteyttä**. Huomaa, että et voi muokata tätä asetusta, kun sitä käytetään.
    - Valitse Azure verkko, palvelimen voidaan sijaitsee. Jos käytät VPN-merkki Varmista, että Azure verkko on yhdistetty paikalliseen verkkoon odotetulla tavalla. 
    - Määritä sisäisen IP-osoite ja aliverkon, joka määritetään palvelimeen. Huomaa, että kaikki aliverkon neljä ensimmäistä IP-osoitteet on varattu sisäinen Azure käyttö. Käytä mitä tahansa muita käytettävissä IP-osoite.
    
    ![Ottaa käyttöön määrityspalvelimen](./media/site-recovery-vmware-to-azure-classic-legacy/cs-details.png)

5. Kun valitset **OK** vakio A3 virtual koneen perusteella Azure sivuston palauttaminen Windows Server 2012 R2-valikoiman kuva luodaan configuration-palvelin-tilaukseesi. Se luodaan uusi pilvipalvelussa ensimmäisen esiintymän. Jos olet valinnut Internetin välityksellä pilvipalvelussa luodaan varattu julkiseen IP-osoite. Voit valvoa edistymistä **työt** -välilehti.

    ![Näytön edistyminen](./media/site-recovery-vmware-to-azure-classic-legacy/monitor-cs.png)

6.  Jos olet muodostamassa Internetin kautta, jälkeen määrityspalvelimen käyttöön Huomautus julkinen IP-osoite määritetään sen **näennäiskoneiden** -sivulla Azure portaalissa. Valitse **päätepisteet** -välilehden Huomautus julkisen HTTPS-portin yhdistetty yksityinen porttiin 443. Tarvitset tiedot myöhemmin, kun rekisteröidyt perusmuodon kohde ja prosessin palvelinten määrityspalvelimen kanssa. Configuration-palvelin on otettu käyttöön näiden päätepisteet:

    - HTTPS: Julkisen porttia käytetään järjestämiseen välisen osan palvelimia ja Azure Internetin välityksellä. Yksityinen porttiin 443 käytetään järjestämiseen välisen osan palvelimia ja Azure VPN-verkon kautta.
    - Mukautettu: Julkinen portti käytetään tuntisesta työkalun viestintä Internetin välityksellä. Yksityinen porttia 9443 käytetään tuntisesta työkalun viestintään VPN-verkon kautta.
    - PowerShellin: Private port 5986
    - Etätyöpöytä: yksityinen portti 3389
    
    ![AM päätepisteet](./media/site-recovery-vmware-to-azure-classic-legacy/vm-endpoints.png)

    >[AZURE.WARNING] Älä poista tai muuta luotu määritys Serverin käyttöönoton aikana päätepisteet julkinen tai yksityinen porttinumero.

Automaattisesti luotu Azure cloud-palvelun varattu IP-osoite määrityspalvelimen on otettu käyttöön. Varattu osoite tarvita, että määritysten cloud osoite pysyy sama yli (mukaan lukien configuration-palvelin) näennäiskoneiden käynnistyy cloud-palvelusta. Varattu julkiseen IP-osoite on oltava manuaalisesti varauksettoman, kun configuration-palvelin on poistettu käytöstä tai se säilyy varattu. Tällä oletusrajoitus 20 varattu julkiseen IP-osoitteiden tilauskohtaisten. [Lue lisää](../virtual-network/virtual-networks-reserved-private-ip.md) siitä varattu IP-osoitteet. 

### <a name="register-the-configuration-server-in-the-vault"></a>Rekisteröi määrityspalvelimen-säilö

1. **Pika-aloitus** -sivulla Valitse **Valmistele kohde resurssien** > **Lataa rekisteröinti-näppäintä**. Avaimen tiedosto luodaan automaattisesti. Se on voimassa viisi päivää sen luonnin jälkeen. Kopioi määritys-palvelimeen.
2. **Näennäiskoneiden** näennäiskoneiden luettelosta configuration-palvelin. Avaa **koontinäyttö** -välilehti ja valitse **Yhdistä**. **Avaa** ladattu tiedosto RDP etätyöpöydän määrityspalvelimen kirjautuminen. Jos käytät VPN-käyttää sisäinen IP-osoite (kirjautumiskerralla käyttöön määrityspalvelimen osoite) Etätyöpöytäyhteys paikallisen-sivustosta. Azure sivuston palauttaminen määritysten Serverin ohjatun käynnistystoiminnon silloin, kun kirjaudut ensimmäisen kerran.

    ![Rekisteröinti](./media/site-recovery-vmware-to-azure-classic-legacy/splash.png)

3. Valitse **hyväksyn** ladattava ja asennettava MySQL **Kolmannen osapuolen Ohjelmistoasennus** .

    ![MySQL-asennus](./media/site-recovery-vmware-to-azure-classic-legacy/sql-eula.png)

4. Luo tunnistetiedot kirjautua MySQL server-esiintymän **MySQL palvelimen tiedot** .

    ![MySQL-tunnistetiedot](./media/site-recovery-vmware-to-azure-classic-legacy/sql-password.png)

5. Määritä **Internet-asetukset** miten configuration-palvelin muodostaa yhteys Internetiin. Huomaa, että:

    - Jos haluat käyttää mukautettua välityspalvelimen olisi määritystä ennen Providerin asentaminen.
    - Kun valitset **Seuraava** testi suoritetaan tarkistamaan yhteyden välityspalvelimen kautta.
    - Jos käytössäsi on mukautettu välityspalvelimen tai oletusarvo-välityspalvelimen edellyttää käyttöoikeuden tarkistusta, sinun on Anna välityspalvelimen tiedot, kuten osoite, portin ja tunnistetiedot.
    - Seuraava URL-osoitteiden on oltava käytettävissä välityspalvelimen kautta:
        - *. hypervrecoverymanager.windowsazure.com
        - *. accesscontrol.windows.net
        - *. backup.windowsazure.com
        - *. blob.core.windows.net
        - *. store.core.windows.net
    - Jos sinulla on IP-osoite-pohjainen palomuurisäännöt varmistaa, että sääntöjä on määritetty sallimaan tietoliikenne määrityspalvelimen kuvattu [Azure-palvelinkeskuksen IP-alueita](https://msdn.microsoft.com/library/azure/dn175718.aspx) ja HTTPS (443)-protokollaa IP-osoitteisiin. Sinun on valkoinen luettelo IP-alueita Azure alueen, jota aiot käyttää, ja että Länsi US.

    ![Välityspalvelimen rekisteröinti](./media/site-recovery-vmware-to-azure-classic-legacy/register-proxy.png)

6. Määritä **Tarjoajan virhe lokalisoinnin viestiasetukset** , mitä kieltä haluat näkyvän virhesanomia.

    ![Virhe viestin rekisteröinti](./media/site-recovery-vmware-to-azure-classic-legacy/register-locale.png)

7. **Azure sivuston palauttaminen** rekisteröimiseen Selaa ja valitse palvelimeen kopioimasi avaimen tiedosto.

    ![Avaimen tiedosto rekisteröinti](./media/site-recovery-vmware-to-azure-classic-legacy/register-vault.png)

8. Valitse ohjatun toiminnon sivulla suorittamista vaihtoehdoista:

    - Valitse **Käynnistä tilin hallinta-valintaikkunassa** voit määrittää, että tilien hallinta-valintaikkunan avaaminen, kun ohjatun toiminnon.
    - Valitse **Luo Cspsconfigtool työpöydän kuvaketta** , jos haluat lisätä työpöydän pikakuvakkeen määritysten palvelimessa niin, että voit avata **Tilien hallinta** -valintaikkunan milloin tahansa tarvitsematta ohjatun toiminnon uudelleen.

    ![Valmis rekisteröinti](./media/site-recovery-vmware-to-azure-classic-legacy/register-final.png)

9. Valitse ohjattu toiminto **loppuun** . Luodaan salasana. Kopioi turvalliseen paikkaan. Sinun on sen todennusta ja rekisteröi prosessin ja perusmuodon kohdepalvelinten määrityspalvelimen kanssa. Sitä käytetään myös tärkeimpiä kanavan eheyttä määritysten palvelimen yhteyksissä. Voit luoda salasana uudelleen, mutta valitse sinun on rekisteröitävä uudelleen perusmuodon kohde ja käsitellä palvelinten käyttäminen uusi salasana.

    ![Salasana](./media/site-recovery-vmware-to-azure-classic-legacy/passphrase.png)

Rekisteröinnin jälkeen määrityspalvelimen näkyvät säilö **Configuration Servers** -sivulla.

### <a name="set-up-and-manage-accounts"></a>Määritä ja hallitse tilit

Käyttöönoton aikana palauttaminen pyytää tunnistetietojen seuraavat toimet:

- VMware tili niin thatSite palautus voi automaattisesti etsiminen VMs vCenter tai vSphere isännät. 
- Kun lisäät suojaus-koneet niin, että sivuston palauttaminen voit asentaa ne Mobility-palvelun.

Configuration-palvelin rekisteröitymisen jälkeen voit avata **Tilien hallinta** -valintaikkunassa voit lisätä ja käsitellä tilit, jota käytetään nämä toiminnot. Muutama tapoja seuraavasti:

- Olet valinnut luotu valintaikkunan asetukset määrityspalvelimen (cspsconfigtool) viimeisellä sivulla pikavalikon avaaminen
- Avaa-valintaikkunan Viimeistele määritys palvelimen asennuksen.

1. Valitse **Tilien hallinta** -kohdassa **Lisää tili**. Voit myös muokata ja poistaa aiemmin luotujen tilien.

    ![Tilien hallinta](./media/site-recovery-vmware-to-azure-classic-legacy/manage-account.png)

2. Kirjoita **Tilin tiedot** tilin nimi, jota käytetään Azure ja tunnistetiedot (toimialueen/käyttäjänimi mm). 

    ![Tilien hallinta](./media/site-recovery-vmware-to-azure-classic-legacy/account-details.png)

### <a name="connect-to-the-configuration-server"></a>Yhteyden muodostaminen configuration-palvelin 

Määritysten palvelimeen kahdella tavalla:

- VPN sivusto sivusto tai ExpressRoute-yhteyden kautta
- Internetin välityksellä 

Huomaa, että:

- Internet-yhteys käytetään virtuaalikoneen päätepisteet yhdessä julkinen virtual IP-osoite palvelimen kanssa.
- VPN-yhteyden käyttää sisäisiä IP-osoite palvelimen päätepisteen yksityinen portit.
- Se on paikallisen palvelinten erikseen päätös valitseminen yhteyden (ohjausobjekti ja replikoinnin tiedot) eri osa-palvelimiin (määrityspalvelimen ja perusmuodon kohde server) käynnissä Azure VPN-yhteyden tai Internetin välityksellä. Et voi muuttaa tätä asetusta myöhemmin. Jos tarvitset reprotect tietokoneesi ja ota uudelleen skenaariota.  


## <a name="step-3-deploy-the-master-target-server"></a>Vaihe 3: Perustyylin kohde-Serverin käyttöönotto

1. Valitse **Valmistele Target(Azure) resurssit** > **käyttöönotto perusmuodon kohde palvelimeen**.
2. Määritä perusmuodon kohde palvelimen tiedot ja tunnistetiedot. Palvelimen otetaan käyttöön määrityspalvelimen Azure samassa verkossa. Kun napsautat suorittamiseen Azure virtuaalikoneen luodaan ja Windows- tai Linux-valikoiman kuva.

    ![Kohde-palvelinasetukset](./media/site-recovery-vmware-to-azure-classic-legacy/target-details.png)

Huomaa, että kaikki aliverkon neljä ensimmäistä IP-osoitteet on varattu sisäinen Azure käyttö. Määritä kaikki käytettävissä olevat IP-osoitteen.

>[AZURE.NOTE] Valitse vakio DS4 määritettäessä suojauksen toiminnoista, jotka edellyttävät yhdenmukaista i/o-tehokas ja pieni viive jotta isännöidä i/o tehostettu työmääriä [Premium-tallennustilan tilin](../storage/storage-premium-storage.md)avulla.


3. Windows perusmuodon kohdepalvelimen päätepisteen luodaan AM. Huomaa, että julkisen päätepisteet luodaan vain, jos yhteyden Internetin välityksellä.

    - Mukautettu: Julkinen portti käyttävät prosessin palvelimen replikoinnin tietojen lähettäminen Internetin kautta. Yksityinen porttia 9443 käytetään prosessin palvelin replikoinnin tietojen lähettäminen perusmuodon palvelimesta VPN-verkon kautta.
    - Custom1: Julkinen portti käyttävät prosessin palvelimen metatietojen lähettäminen Internetin kautta. Yksityinen porttia 9080 käytetään prosessin palvelin perusmuodon kohde-palvelimeen lähetetään metatietojen VPN-verkon kautta.
    - PowerShellin: Private port 5986
    - Etätyöpöytä: yksityinen portti 3389

4. Linux perusmuodon kohdepalvelimen AM luodaan nämä päätepisteet. Huomaa, että julkisen päätepisteet luodaan vain, jos olet muodostamassa Internetin välityksellä.

    - Mukautettu: Julkinen portti prosessin palvelimen käyttämä replikoinnin tietojen lähettäminen Internetin kautta. Yksityinen porttia 9443 käytetään prosessin palvelin replikoinnin tietojen lähettäminen perusmuodon palvelimesta VPN-verkon kautta.
    - Custom1: Julkinen portti käytetään prosessin palvelin metatietojen lähettäminen Internetin kautta. Yksityinen porttia 9080 käytetään prosessin palvelin perusmuodon kohde-palvelimeen lähetetään metatietojen VPN-verkon kautta
    - SSH: Private port 22

    >[AZURE.WARNING] Älä poista tai muuta julkinen tai yksityinen porttinumero minkä tahansa päätepisteet luoda perusmuodon kohde Serverin käyttöönoton aikana.

5. Aloita virtuaalikoneen **näennäiskoneiden** odota.

    - Jos ohjelma remote työpöydän yksityiskohtaiset Windows server huomautuksen.
    - Huomaa virtuaalikoneen sisäinen IP-osoite, jos se on Linux-palvelin ja muodostat yhteyden VPN-verkon kautta. Jos olet muodostamassa Internetin välityksellä Huomautus julkiseen IP-osoitteeseen.

6.  Kirjaudu asennuksessa palvelimeen ja rekisteröi määrityspalvelimen kanssa. 
7.  Jos käytössäsi on Windows:

    1. Aloittaa Etätyöpöytäyhteys virtuaalikoneen avulla. Komentosarjan kirjautuminen ensimmäistä kertaa suoritetaan PowerShell-ikkunassa. Älä sulje se. Kun se päättyy Host agentti Config-työkalun avautuu automaattisesti rekisteröidä palvelimeen.
    2. Määritä **Host (isäntä)-agentti Config** määrityspalvelimen ja porttiin 443 sisäinen IP-osoite. Voit käyttää sisäistä osoite ja yksityiset porttiin 443 vaikka jollet ole muodostamassa VPN-verkon kautta koska virtuaalikoneen on liitetty Azure samassa verkossa kuin configuration-palvelin. Jätä **Käytä HTTPS** käytössä. Kirjoita muistiin kirjoittamasi aiemmin määrityspalvelimen salasana. Valitse **OK** , jos haluat rekisteröidä palvelimen. Voit ohittaa NAT-asetukset. Niitä ei käytetään.
    3. Jos arvioitu säilytys-asema vaatimus on yli 1 TT: Voit määrittää virtual levyn ja [tallennustilaa välilyöntejä](http://blogs.technet.com/b/askpfeplat/archive/2013/10/21/storage-spaces-how-to-configure-storage-tiers-with-windows-server-2012-r2.aspx) säilytys äänenvoimakkuuden (R:)
    
    ![Windowsin perusmuodon palvelimesta](./media/site-recovery-vmware-to-azure-classic-legacy/target-register.png)

8. Jos käytössäsi on Linux:
    1. Varmista, että olet asentanut uusimman Linux Integration Services (LIS) asennettuna, ennen kuin asennat perusmuodon kohdepalvelimen. Löydät sekä ohjeita LIS uusimman version asentamisesta [tähän](https://www.microsoft.com/download/details.aspx?id=46842). Käynnistä tietokone uudelleen, kun LIS asentanut.
    2. Valitse **Valmistele kohde (Azure) resurssien** **lataaminen ja asentaminen lisäohjelmistoa (vain varten Linux Master kohde Server)**. Kopioi sftp-asiakas virtuaalikoneen ladatut tar-tiedosto. Vaihtoehtoisesti voit kirjautua sisään palvelimeen käyttöön linux perusmuodon kohde ja ladata *wget http://go.microsoft.com/fwlink/?LinkID=529757&clcid=0x409* avulla tiedosto.
    2. Kirjaudu sisään käyttämällä suojattu runko asiakkaan palvelimeen. Jos olet yhdistettynä Azure verkkoon VPN-verkon kautta käyttämällä sisäinen IP-osoite. Muussa käyttää ulkoisen IP-osoite ja SSH julkisen päätepiste.
    3. Pura tiedostot gzipped installer suorittamalla: **Kohdekeh – xvzf Microsoft-ASR_UA_8.4.0.0_RHEL6-64***
    ![Linux perusmuodon palvelimesta](./media/site-recovery-vmware-to-azure-classic-legacy/linux-tar.png)
    4. Varmista, että olet hakemistossa, johon purit tar-tiedoston sisällön.
    5. Kopioi määritysten palvelimen salasana paikallinen tiedosto-komennon avulla * *Kaiku* `<passphrase>` * > passphrase.txt**
    6. Suorita-komento "**sudo. / -t asentaa molemmat - a isännöidä -R MasterTarget -d /usr/local/ASR -i* `<Configuration server internal IP address>` * -p 443 -s y - c https -P passphrase.txt**".

    ![Rekisteröi palvelimesta](./media/site-recovery-vmware-to-azure-classic-legacy/linux-mt-install.png)

9. Odota muutama minuutti (10-15) ja Tarkista sivulla että perusmuodon kohdepalvelin näy **palvelimissa**rekisteröity > **Configuration Servers** **Palvelimen tiedot** -välilehti. Jos käytössäsi on Linux ja se ei rekisteröinyt Suorita isäntä config-työkalun uudelleen /usr/local/ASR/Vx/bin/hostconfigcli. Sinun on määritettävä käyttöoikeudet suorittamalla chmod kuin ylimmällä tasolla.

    ![Tarkista kohdepalvelin](./media/site-recovery-vmware-to-azure-classic-legacy/target-server-list.png)

>[AZURE.NOTE] Voi kestää jopa 15 minuutin rekisteröinnin päätyttyä perusmuodon kohde palvelimen portaalissa näkyvän. Päivitä heti, valitse **Päivitä** **Määritysten palvelimia** -sivulla.

## <a name="step-4-deploy-the-on-premises-process-server"></a>Vaihe 4: Paikallisen prosessi-Serverin käyttöönotto

Ennen kuin aloitat Suosittelemme, että määrität staattinen IP-osoite prosessin palvelimen niin, että se on taata olevan pysyvä koko käynnistyy.

1. Valitse pika-Aloitus > **Asenna prosessin Server paikallisen** > **lataaminen ja asentaminen prosessi-palvelimeen**.

    ![Prosessin Serverin asentaminen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-deploy.png)

2.  Kopioi ladattu zip-tiedosto, jonka aiot asentaa prosessi-palvelimen palvelimeen. Zip-tiedosto sisältää kaksi asennustiedostojen:

    - Microsoft-ASR_CX_TP_8.4.0.0_Windows*
    - Microsoft-ASR_CX_8.4.0.0_Windows*

3. Pura arkistoon ja Kopioi asennustiedostot palvelimeen.
4. Suorita **Microsoft-ASR_CX_TP_8.4.0.0_Windows*** asennus tiedoston ja noudata ohjeita. Tämä asentaa kolmannen osapuolen osat tarvittavat käyttöönotto.
5. Suorita **Microsoft-ASR_CX_8.4.0.0_Windows***.
6. Valitse **Palvelintila** sivulla **Prosessin palvelimeen**.
7. **Ympäristön tiedot** -sivulla seuraavasti:


    - Jos haluat suojata VMware virtual koneet valitsemalla **Kyllä**
    - Jos haluat vain suojaa fyysiset palvelimet ja näin ollen tarvitse asentaa palvelimelle prosessin VMware vCLI. Valitse **ei,** ja jatka.

8. Kun asennat VMware vCLI seuraavat seikat:

    - **Vain VMware vSphere CLI 5.5.0 tuetaan**. Prosessi-palvelin ei toimi muissa versioissa tai vSphere CLI päivitykset.
    - Lataa vSphere CLI 5.5.0- [tähän.](https://my.vmware.com/web/vmware/details?downloadGroup=VCLI550&productId=352)
    - Jos olet asentanut vSphere CLI juuri ennen kuin olet aloittanut prosessin palvelimen asentaminen ja asennusohjelma ei havaitse sitä, odota enintään viisi minuuttia, ennen kuin ryhdyt käyttämään asennusohjelma uudelleen. Näin varmistat, että kaikki ympäristömuuttujat tarvitaan vSphere CLI tunnistaminen on valmisteltu oikein.

9.  Valitse **NIC valinnan prosessin palvelimen** verkkosovittimen, prosessi-palvelimen tulisi käyttää.

    ![Valitse sovittimen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-nic.png)

10. **Palvelimen tiedot**:

    - IP-osoite ja porttia, jos olet muodostamassa VPN-verkon kautta Määritä määritysten palvelimen ja portin 443 sisäinen IP-osoite. Määritä julkinen virtual IP-osoite ja yhdistetyt julkisen HTTP-päätepisteen muussa.
    - Kirjoita salasana määrityspalvelimen.
    - Poista **Tarkista Mobility palvelun ohjelmiston allekirjoitus** , jos haluat poistaa käytöstä vahvistusta, kun käytät automaattinen push-palvelun asentaminen. Allekirjoituksen tarkistaminen on internet-yhteys, prosessi-palvelimesta.
    - Valitse **Seuraava**.

    ![Rekisteröi määrityspalvelimen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cs.png)


11. Valitse **Valitse asennusasemaan** välimuisti-asema. Prosessi-palvelin täytyy välimuisti-asema, jossa on vähintään 600 Gigatavua vapaata tilaa. Valitse **Asenna**. 

    ![Rekisteröi määrityspalvelimen](./media/site-recovery-vmware-to-azure-classic-legacy/ps-cache.png)

12. Huomaa, että voit joutua käynnistämään palvelimeen ja viimeistele asennus. Valitse **Määrityspalvelimen** > **Palvelimen tiedot** Tarkista prosessin palvelimen tulee näkyviin ja rekisteröidään onnistuneesti säilö.

>[AZURE.NOTE]Voi kestää jopa 15 minuutin rekisteröinnin päätyttyä prosessin palvelimen tulee näkyviin, kun configuration-palvelin-kohdasta. Päivitä heti, Päivitä määrityspalvelimen valitsemalla palvelimen määrityssivulla alaosassa Päivitä-painike
 
![Vahvista prosessi-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/ps-register.png)

Jos poistat ei ole allekirjoituksen tarkistaminen Mobility-palvelun asentaessasi prosessi-palvelimeen, voit tehdä sen myöhemmin seuraavasti:

1. Kirjaudu sisään järjestelmänvalvojana prosessi-palvelimeen, ja Avaa tiedosto C:\pushinstallsvc\pushinstaller.conf muokkausta varten. Valitse osan **[PushInstaller.transport]** lisäämällä tämän rivin: **SignatureVerificationChecks = "0"**. Tallenna ja sulje tiedosto.
2. Käynnistä InMage PushInstall-palvelu uudelleen.


## <a name="step-5-update-site-recovery-components"></a>Vaihe 5: Update-sivuston palauttaminen osat

Sivuston palauttaminen osat päivitetään aika. Kun uusia päivityksiä on saatavilla asenna ne seuraavassa järjestyksessä:

1. Määrityspalvelimen
2. Prosessi-palvelin
3. Perusmuodon palvelimesta
4. Tuntisesta työkalu (vContinuum)

### <a name="obtain-and-install-the-updates"></a>Hanki ja asenna päivitykset


1. Voit hankkia päivitykset määritys-, prosessi- ja perusmuodon kohdepalvelinten palauttaminen **raporttinäkymät-ikkunan**. Linux asennuksen poimiminen gzipped installer tiedostot ja suorita-komento "sudo. / asentaa" Asenna päivitys.
2. [Lataa](http://go.microsoft.com/fwlink/?LinkID=533813) tuntisesta tool(vContinuum) uusimman päivityksen.
3. Jos käytössäsi on näennäiskoneiden tai fyysinen palvelimet, joilla on jo asennettu Mobility-palvelu, voit päivittää palvelun seuraavasti:

    - **Vaihtoehto 1**: Lataa päivitykset:
        - [Windows Server (vain 64-bittinen)](http://download.microsoft.com/download/8/4/8/8487F25A-E7D9-4810-99E4-6C18DF13A6D3/Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe)
        - [CentOS 6.4,6.5,6.6 (vain 64-bittinen)](http://download.microsoft.com/download/7/E/D/7ED50614-1FE1-41F8-B4D2-25D73F623E9B/Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz)
        - [Oracle Enterprise Linux 6.4,6.5 (vain 64-bittinen)](http://download.microsoft.com/download/5/2/6/526AFE4B-7280-4DC6-B10B-BA3FD18B8091/Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz)
        - [SUSE Linux Enterprise Server SP3 (vain 64-bittinen)](http://download.microsoft.com/download/B/4/2/B4229162-C25C-4DB2-AD40-D0AE90F92305/Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz)
        - Prosessi-palvelimen päivittämisen jälkeen Mobility-palvelun päivitetyn version ovat käytettävissä prosessin palvelimessa C:\pushinstallsvc\repository-kansiossa.
    - **Vaihtoehto 2**: Jos sinulla on tietokoneessa, jossa on asennettuna Mobility-palvelun vanhempi versio, voit päivittää tietokoneen Mobility-palvelu automaattisesti hallinta-portaalin.

        1. Varmistaa, että prosessin palvelin on päivitetty.
        2. Varmista, että suojattujen kone mukainen [edellytykset](#install-the-mobility-service-automatically) valitseminen Mobility-palvelun automaattisesti niin, että päivitys toimii odotetulla tavalla.
        2. Valitse Suojaus-ryhmä, korosta suojattu tietokone ja valitse **Päivitä Mobility-palvelun**. Tämä painike on käytettävissä vain silloin, ei Mobility-palvelun uudemman version. 

            ![Valitse vCenter-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/update-mobility.png)

Valitse tilit Määritä järjestelmänvalvojatili, jota käytetään päivittämään suojatun palvelimessa mobility-palvelua. Valitse OK ja odota, kunnes käynnistettävät työn suorittamiseen.


## <a name="step-6-add-vcenter-servers-or-vsphere-hosts"></a>Vaihe 6: Lisää vCenter palvelimia tai vSphere isännät

1. Valitse **palvelimia** > **Configuration Servers** > määrityspalvelimen > Lisää vCenter palvelimeen tai vSphere host**Lisää vCenter palvelimeen** .

    ![Valitse vCenter-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter.png)

2. Määritä palvelimeen tai host tiedot ja valitse prosessi-palvelin, jota käytetään löytää tiedoston.

    - Jos vCenter palvelin ei ole käynnissä 443 oletusportti Määritä porttinumero, jossa vCenter palvelin on käynnissä.
    - Prosessi-palvelin on oltava samassa verkossa kuin vCenter palvelin/vSphere isäntä ja pitäisi olla VMware vSphere CLI 5.5.0 asennettuna.

    ![vCenter palvelimen asetukset](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter4.png)


3. Etsinnän päätyttyä vCenter palvelimen kohdasta palvelimen tiedot.

    ![vCenter palvelimen asetukset](./media/site-recovery-vmware-to-azure-classic-legacy/add-vcenter2.png)

4. Jos käytät järjestelmänvalvojatilin Lisää palvelimeen tai host, varmista, että tili on seuraavat oikeudet:

    - vCenter tilit olisi on Palvelinkeskukseen, Datastore, kansion, Host, verkon, resurssi, tallennustilan näkymät, virtuaalikoneen vSphere Distributed valitsin käytössä.
    - vSphere host tilit on oltava-Palvelinkeskukseen, Datastore, kansion, Host, verkon, resurssi, virtuaalikoneen ja vSphere Distributed valitsin oikeudet käytössä



## <a name="step-7-create-a-protection-group"></a>Vaihe 7: Luo suojaus-ryhmä

1. Avaa **Suojattu kohteet** > **Suojaus-ryhmä** > **Luo suojaus-ryhmä**.

    ![Luo suojaus-ryhmä](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg1.png)

2. Määritä **Määritä ryhmäasetukset** -sivulla ryhmän nimi ja valitse configuration-palvelin, johon haluat luoda ryhmän.

    ![Ryhmän suojausasetuksia](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg2.png)

3. **Määritä toistoa asetukset** -sivulla, jota käytetään kaikissa tietokoneissa ryhmässä replikoinnin-asetusten määrittäminen.

    ![Suojauksen ryhmän sallittuja](./media/site-recovery-vmware-to-azure-classic-legacy/create-pg3.png)

4. Asetukset:
    - **Usean AM yhdenmukaisuuden**: Jos otat tämän luomilleen jaetun sovelluksen yhdenmukaisia palautus pisteiden suojaus-ryhmä-tietokoneissa. Tämä asetus on tärkeimpiä, kun kaikki suojaus-ryhmä koneet on käytössä sama työmäärää. Kaikissa tietokoneissa palautetaan saman arvopisteeseen. Käytettävissä vain Windows-palvelimiin.
    - **RPO kynnysarvo**: ilmoitusten luodaan, kun jatkuvan tietojen suojauksen replikoinnin RPO ylittää määritetyn RPO raja-arvo.
    - **Palautus valitsemalla säilytys**: määrittää säilytys-ikkuna. Suojatun koneet voidaan palauttaa mihin tahansa kohtaan tässä ikkunassa.
    - **Sovelluksen yhdenmukaista tilannevedoksen korkojakso**: määrittää, kuinka usein palautus pistettä sisältävä sovelluksen yhdenmukaista tilannevedoksia luodaan.

Voit valvoa suojaus-ryhmä, kun ne on luotu **Suojatut osat** -sivulla.

## <a name="step-8-set-up-machines-you-want-to-protect"></a>Vaihe 8: Koneet, jonka haluat suojata määrittäminen

Tarvitset Mobility-palvelun asentaminen näennäiskoneiden ja fyysinen palvelimissa, jotka haluat suojata. Voit tehdä tämän kahdella tavalla:

- Push- ja -palvelun asentaminen tietokoneeseen kunkin prosessin palvelimesta automaattisesti.
- Manuaalinen asennus palvelu. 

### <a name="install-the-mobility-service-automatically"></a>Asenna Mobility-palvelun automaattisesti

Kun lisäät koneet suojaus-ryhmä Mobility-palvelun miten ja asennettuna tietokoneeseen kunkin prosessin-palvelimen automaattisesti. 

**Automaattisesti push Asenna Windows-palvelimien mobility-palvelu:** 

1. Asenna uusimmat päivitykset prosessin palvelimen kuvatulla tavalla [Vaihe 5: Asenna uusimmat päivitykset](#step-5-install-latest-updates), ja varmista, että prosessin palvelin on käytettävissä. 
2. Varmista verkkoyhteys lähdetietokoneen ja prosessin palvelimen välillä ja että lähde-tietokone on käytettävissä prosessin palvelimesta.  
3. Jos haluat, että **tiedostojen ja tulostimien jakaminen** ja **Ohjattu toiminto**Windowsin palomuurin. Valitse Windowsin palomuuri-asetukset-valitse vaihtoehto "Salli sovelluksen tai ominaisuuden läpäistä palomuuri" ja valitse sovellukset, kuten alla olevassa kuvassa. Voit määrittää palomuurin käytännön Ryhmäkäytäntöobjekti, jos tietokoneissa, joihin kuuluvat toimialueeseen.

    ![Palomuuriasetukset](./media/site-recovery-vmware-to-azure-classic-legacy/push-firewall.png)

4. Push-asennuksen suorittamiseen käytettävän tilin on oltava Järjestelmänvalvojat-ryhmän suojattava tietokoneeseen. Näitä tunnistetietoja käytetään vain push-asennuksen Mobility-palvelun ja anna ne, kun lisäät koneen suojaus-ryhmä.
5. Jos annettu tili ei ole toimialueen-tili, sinun on käyttäjän etäkäyttöpalvelimen ohjausobjektin paikallisessa tietokoneessa käytöstä. Voit tehdä tämän Lisää LocalAccountTokenFilterPolicy DWORD-rekisterimerkinnän arvo on 1, valitse HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System. Lisää rekisterimerkintä CLI Avaa cmd tai powershell ja lisää **`REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1`**. 

**Push Asenna mobility-palvelun automaattisesti Linux palvelimissa:**

1. Asenna uusimmat päivitykset prosessin palvelimen kuvatulla tavalla [Vaihe 5: Asenna uusimmat päivitykset](#step-5-install-latest-updates), ja varmista, että prosessin palvelin on käytettävissä.
2. Varmista verkkoyhteys lähdetietokoneen ja prosessin palvelimen välillä ja että lähde-tietokone on käytettävissä prosessin palvelimesta.  
3. Varmista, että tili on Linux lähdepalvelimeen pääkansion käyttäjän.
4. Varmista, että Linux lähdepalvelimeen /etc/hosts-tiedosto sisältää tapahtumia, jotka vastaavat paikallisen isäntänimi kaikki NIC liittyvät IP-osoitteet.
5. Asenna uusimmat openssh, openssh-palvelin, openssl pakettien suojattava tietokoneeseen.
6. Varmista SSH on käytössä ja käytössä porttiin 22. 
7. Ottaa käyttöön SFTP Alirakenne ja salasanan todennuksen sshd_config tiedoston seuraavasti: 

    - a) Kirjaudu sisään ylimmällä tasolla.
    - b) Etsi /etc/ssh/sshd_config tiedosto, joka alkaa **PasswordAuthentication**rivi.
    - (c) rivin kommentointi ja muuta "ei", "Kyllä" arvosta.

        ![Linux liikkuvuus](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push.png)

    - d) Etsi rivi, joka alkaa Alirakenne ja kommentointi rivi.
    
        ![Linux push liikkuvuus](./media/site-recovery-vmware-to-azure-classic-legacy/linux-push2.png)    

8. Varmista lähde koneen Linux-muuttuja tuetaan. 
 
### <a name="install-the-mobility-service-manually"></a>Mobility-palvelun asentaminen manuaalisesti

Käyttää Mobility-palvelun asentaminen ohjelmistopakettien ovat C:\pushinstallsvc\repository prosessi-palvelimessa. Kirjaudu prosessi-palvelimeen ja kopioi haluamasi asennuspaketin lähdetietokoneen alla olevan taulukon perusteella:-

| Lähde-käyttöjärjestelmä                               | Mobility-palvelupakettiin, prosessi-palvelimeen                                                            |
|---------------------------------------------------    |------------------------------------------------------------------------------------------------------ |
| Windows Server (vain 64-bittinen)                          | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe`         |
| CentOS 6.4, 6.5 6.6 (vain 64-bittinen)                    | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_RHEL6-64_GA_28Jul2015_release.tar.gz`     |
| SUSE Linux Enterprise Server 11 SP3 (vain 64-bittinen)     | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_SLES11-SP3-64_GA_28Jul2015_release.tar.gz`|
| Oracle Enterprise Linux 6.4, 6.5 (vain 64-bittinen)        | `C:\pushinstallsvc\repository\Microsoft-ASR_UA_8.4.0.0_OL6-64_GA_28Jul2015_release.tar.gz`       |


**Manuaalisesti Windows-palvelimessa Mobility-palvelun asentaminen**, toimi seuraavasti:

1. Kopioi edellä olevan taulukon lähde-koneen luetellut prosessin palvelimen tiedostopolku **Microsoft ASR_UA_8.4.0.0_Windows_GA_28Jul2015_release.exe** -paketti.
2. Asenna Mobility-palvelu suorittamalla suoritettavan tiedoston lähde-koneessa.
3. Noudata asennusohjelman ohjeita.
4. Valitse roolissa **Mobility-palvelu** ja valitse **Seuraava**.
    
    ![Mobility-palvelun asentaminen](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install.png)

5. Jätä asennuksen kansio kuin oletusarvoinen asennuspolku ja valitse **Asenna**.
6. Määritä **Host (isäntä)-agentti Config** IP-osoite ja HTTPS-portin määrittäminen palvelimen.

    - Jos olet muodostamassa Internetin välityksellä Määritä portti Julkinen virtual IP-osoite ja julkisen HTTPS päätepiste.
    - Jos olet muodostamassa VPN-verkon kautta Määritä sisäisen IP-osoite ja porttiin 443. Jätä **Käytä HTTPS** valittuna.

    ![Mobility-palvelun asentaminen](./media/site-recovery-vmware-to-azure-classic-legacy/ms-install2.png)

7. Määritä määritysten palvelimen salasana ja valitse **OK** , jos haluat rekisteröidä Mobility-palvelun määrityspalvelimen kanssa.

**Voit suorittaa komentoriviltä seuraavasti:**

1. Kopioi salasana CX palvelimessa "C:\connection.passphrase"-tiedostoon ja suorita tämä komento. Tässä esimerkissä CX i 104.40.75.37 ja HTTPS-portin on 62519:

    `C:\Microsoft-ASR_UA_8.2.0.0_Windows_PREVIEW_20Mar2015_Release.exe" -ip 104.40.75.37 -port 62519 -mode UA /LOG="C:\stdout.txt" /DIR="C:\Program Files (x86)\Microsoft Azure Site Recovery" /VERYSILENT  /SUPPRESSMSGBOXES /norestart  -usesysvolumes  /CommunicationMode https /PassphrasePath "C:\connection.passphrase"`

**Asenna Mobility-palvelu manuaalisesti Linux-palvelimessa**.

1. Kopioi haluamasi tar arkisto yllä olevaan taulukkoon, prosessi-palvelimesta lähde-koneen perusteella.
2. Avaa shell-ohjelmasta ja Pura suorittamalla paikallisen polun tar zip-arkisto`tar -xvzf Microsoft-ASR_UA_8.2.0.0*`
3. Passphrase.txt-tiedoston luominen paikalliseen kansioon, johon olet purkanut tar arkiston sisällön kirjoittamalla *`echo <passphrase> >passphrase.txt`* -liittymän.
4. Mobility-palvelun asentaminen kirjoittamalla *`sudo ./install -t both -a host -R Agent -d /usr/local/ASR -i <IP address> -p <port> -s y -c https -P passphrase.txt`*.
5. Määritä IP-osoite ja portin:

    - Jos olet muodostamassa yhteyttä määrityspalvelimen Internetin välityksellä Määritä määritysten virtual julkiseen IP-osoite ja julkisen HTTPS päätepiste `<IP address>` ja `<port>`.
    - Jos olet muodostamassa VPN-yhteyden kautta Määritä sisäisen IP-osoite ja 443.

**Jos haluat suorittaa komentoriviltä**seuraavasti:

1. Kopioi salasana CX palvelimessa "passphrase.txt"-tiedostoon ja suorita tässä komentoja. Tässä esimerkissä CX i 104.40.75.37 ja HTTPS-portin on 62519:

Asenna tuotannon-palvelimeen:

    ./install -t both -a host -R Agent -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt
 
Asenna kohde-palvelimeen:


    ./install -t both -a host -R MasterTarget -d /usr/local/ASR -i 104.40.75.37 -p 62519 -s y -c https -P passphrase.txt

>[AZURE.NOTE] Kun lisäät koneet suojaus-ryhmä, joka on jo käytössä sopiva versio Mobility-palvelun sitten push-asennuksen ohitetaan.


## <a name="step-9-enable-protection"></a>Vaihe 9: Ota suojaus

Suojauksen käyttöön lisätään näennäiskoneiden ja fyysiset palvelimet suojaus-ryhmä. Ennen kuin aloitat, Huomaa seuraavat seikat:

- Näennäiskoneiden löytyy 15 minuutin välein, ja se voi kestää jopa 15 minuuttia, ne näkyvät Azure sivuston palautuksen jälkeen etsiminen.
- Ympäristön muutokset virtuaalikoneen (kuten VMware Työkalut asennus) niihin myös sivuston palauttaminen päivittäminen kestää 15 minuutin kuluttua.
- Voit tarkistaa havaitun viimeksi **Viimeisen YHTEYSHENKILÖN osoitteessa** -kenttään vCenter palvelin/ESXi isännän **Kokoonpano palvelimia** -sivulla.
- Jos sinulla on jo luotu suojaus-ryhmä ja lisää sen jälkeen vCenter palvelimeen tai ESXi host kestää 15 minuutin päivittämiseen Azure sivuston palautus-portaalin ja näennäiskoneiden **Lisää koneet suojaus-ryhmä** -valintaikkunassa näkyvän.
- Jos haluat jatkaa välittömästi koneet lisääminen suojaus-ryhmä ajoitetun etsiminen odottamatta, korosta määrityspalvelimen (Älä valitse) ja valitse **Päivitä** -painiketta.
- Kun lisäät näennäiskoneiden tai fyysisten laitteiden suojaus-ryhmä, prosessi-palvelimen Vie automaattisesti ja asentaa Mobility-palvelun lähdepalvelimeen jos se ei ole jo asennettu.
- Varmista automaattinen push-järjestelmä toimii suojatun koneet olet määrittänyt edellisessä vaiheessa kuvatulla tavalla.

Lisää koneet seuraavasti:

1. Valitse **suojattu kohteiden** > **Suojaus-ryhmä** > **koneet** > **Lisää koneet**. Paras käytäntö on suositeltavaa, että suojaa ryhmät peilikuvat oman työmääriä niin, että lisäät, joissa käytetään tietyn sovelluksen samaan ryhmään.
2. **Valitse** näennäiskoneiden Jos olet suojaaminen fyysiset palvelimet, **Lisää fyysisten laitteiden** ohjatun antaa IP-osoite ja kutsumanimi. Valitse käyttöjärjestelmä perhe.

    ![Lisää V keskelle-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/physical-protect.png)

3. **Valitse** näennäiskoneiden Jos olet suojaaminen VMware näennäiskoneiden, valitse vCenter-palvelin, joka hallitsee oman näennäiskoneiden (tai EXSi isännän, jossa ne on käynnissä) ja valitse sitten koneet.

    ![Lisää V keskelle-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/select-vms.png) 
4. Valitse **Määritä kohde resurssien** perusmuodon kohde palvelimet ja tallennustilaa replikoinnin ja valita, käytetäänkö asetukset, kaikki työmäärät. Valitse [Premium-tallennustilan tilin](../storage/storage-premium-storage.md) määritettäessä toiminnoista, jotka edellyttävät yhdenmukaista IO tehokas ja pieni viive jotta isännöidä IO tehostettu työmääriä suojaus. Jos haluat käyttää Premium-tallennustilan tilin työmäärää levyjen on käytettävä DS-sarjan perustyyli kohde. Et voi käyttää Premium tallennustilan levyjen perustyyli kohde DS sarjan.

    >[AZURE.NOTE] Siirrä tallennustilan tilien luotu [uuden Azure portaalin](../storage/storage-create-storage-account.md) resurssin ryhmien ei tueta.

    ![vCenter server](./media/site-recovery-vmware-to-azure-classic-legacy/machine-resources.png)

5. Valitse **Määritä tilit** Mobility-palvelun asentamisessa suojatun koneet haluamasi tili. Mobility-palvelun automaattinen asennusta varten tarvitaan tilin tunnistetiedot. Jos et voi valita-tilin Varmista, että voit määrittää yhden vaiheen 2 ohjeiden mukaisesti. Huomaa, että tällä tilillä ei voi käyttää Azure. Windows server tili on järjestelmänvalvojan oikeudet lähde-palvelimeen. Linux tilin on oltava ylimmällä tasolla.

    ![Linux tunnistetiedot](./media/site-recovery-vmware-to-azure-classic-legacy/mobility-account.png)

6. Napsauta valintamerkkiä lisännyt koneet suojaus-ryhmä ja Käynnistä ensimmäisen replikoinnin kunkin tietokoneeseen. Voit valvoa tilan **työt** -sivulta.

    ![Lisää V keskelle-palvelin](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs2.png)

7. Lisäksi voit seurata tilaa **Suojatut osat** valitsemalla > Suojaus-ryhmänimi > **näennäiskoneiden** . Kun ensimmäisen replikoinnin päättää ja koneet tietojen synkronoiminen ovat ne näkyvät **suojattu** tila.

    ![Virtuaalikoneen töitä](./media/site-recovery-vmware-to-azure-classic-legacy/pg-jobs.png)


### <a name="set-protected-machine-properties"></a>Määritä suojattu tietokoneen ominaisuudet

1. Kun kone on **suojattu** tila voit määrittää automaattisesti sen ominaisuuksia. Valitse Suojaus-ryhmän tiedot koneen ja Avaa **määritys** -välilehti.
2. Voit muokata nimeä, joka annetaan Azure koneen automaattisesti ja Azure virtuaalikoneen koon jälkeen. Voit myös valita Azure verkkoon, johon liitetty tietokoneen jälkeen automaattisesti.

    > [AZURE.NOTE] [Siirron verkostojen](../resource-group-move-resources.md) saman tilauksen piiriin kuuluvien resurssien ryhmien tai tilauksissa ei tue verkkojen käytettäviä sivustojen palauttamisen käyttöönotto.

    ![Virtuaalikoneen ominaisuuksien määrittäminen](./media/site-recovery-vmware-to-azure-classic-legacy/vm-props.png)

Huomaa, että:

- Azure tietokoneen nimen on oltava Azure vaatimusten mukainen.
- Oletusarvoisesti replikoitua näennäiskoneiden Azure-tietokannassa ei ole yhteydessä Azure verkkoon. Voit halutessasi replikoitua näennäiskoneiden kommunikoimiseen Varmista, että voit määrittää ne samassa Azure verkossa.
- Jos muutat kokoa aseman VMware Virtuaalikoneen tai fyysinen palvelimessa, se siirtyy kriittinen tila. Jos haluat muuttaa kokoa, toimi seuraavasti:

    - a) muuta kokoa-asetusta.
    - b) Valitse **näennäiskoneiden** -välilehdessä virtuaalikoneen ja valitse **Poista**.
    - (c) Valitse **Poista virtuaalikoneen** **käytöstä protection (aseman koon ja siirtyä palautus käytön)**-vaihtoehto. Tämä asetus poistaa suojauksen, mutta säilyttää Azure palautus pisteitä.

        ![Virtuaalikoneen ominaisuuksien määrittäminen](./media/site-recovery-vmware-to-azure-classic-legacy/remove-vm.png)

    - d) virtuaalikoneen suojaus uudelleen käyttöön. Kun Ota suojaus uudelleen käyttöön kokoa on muutettu äänenvoimakkuuden tiedot siirretään Azure.

    

## <a name="step-10-run-a-failover"></a>Vaihe 10: Suorita automaattisesti

Tällä hetkellä voidaan suorittaa vain suunnittelematon failovers suojatun VMware näennäiskoneiden ja fyysinen palvelimiin. Ota seuraavat seikat huomioon:



- Ennen kuin aloitat automaattisesti, varmista, että määritys ja perustyyli kohde-palvelimet ovat käynnissä ja kunnossa. Muussa tapauksessa automaattisesti epäonnistuu.
- Lähde-tietokoneet eivät ole Sammuta suunnittelematon automaattisesti osana. Tietojen replikoinnin suojatun palvelimia lopettaa suorittamiseen suunnittelematon automaattisesti. Tarvitset poistaa koneet suojaus-ryhmä ja lisätä ne uudelleen, jotta voit aloittaa suojaaminen koneet uudelleen suunnittelematon vikasietotilaa päätyttyä.
- Jos haluat epäonnistua päälle menettämättä mitään tietoja, varmista, että ensisijainen näennäiskoneiden poistetaan käytöstä, ennen kuin käynnistät vikasietotilaa.

1. **Palautus suunnitelmien** sivulla ja lisää palautus-suunnitelma. Määritä suunnitelman tiedot ja valitse **Azure** kohteena.

    ![Määritä palautus suunnitelma](./media/site-recovery-vmware-to-azure-classic-legacy/rplan1.png)

2. **Valitse virtuaalikoneen** : Valitse Suojaus-ryhmä ja valitse sitten koneet ryhmä, johon haluat lisätä palautus-suunnitelma. [Lue lisää](site-recovery-create-recovery-plans.md) palautus-palvelupakettia.

    ![Lisää näennäiskoneiden](./media/site-recovery-vmware-to-azure-classic-legacy/rplan2.png)

3. Jos tarvitaan voit mukauttaa aiot luoda ryhmiä ja järjestyksen tilauksen palautus mitä koneet palvelupaketti on epäonnistui päälle. Voit myös lisätä kehote manuaalinen toiminnot ja komentosarjoja. Komentosarjoja, kun palauttaminen Azure voidaan lisätä [Azure automaatio Runbooks](site-recovery-runbook-automation.md)avulla.

4. **Palautus suunnitelmien** -sivulla palvelupaketti, ja valitse sitten **Suunnittelematon automaattisesti**.
5. Tarkista **Vahvista automaattisesti** automaattisesti suunta (Azure) ja valitse palautus osoittamalla epäonnistua päälle.
6. Odota automaattisesti suorittaa ja tarkista sitten onnistumisen vikasietotilaa oikein ja että replikoitua näennäiskoneiden käynnistäminen onnistuu Azure projektille.




## <a name="step-11-fail-back-failed-over-machines-from-azure"></a>Vaihe 11: Epäonnistua koneet azuren kautta takaisin epäonnistui

[Lue lisää](site-recovery-failback-azure-to-vmware-classic-legacy.md) siitä, miten voit tuoda oman epäonnistui päällä, joissa käytetään Azure takaisin paikalliseen ympäristöön.


## <a name="manage-your-process-servers"></a>Prosessi-palvelinten hallinta

Prosessi-palvelin lähettää replikoinnin tietoja Azure-tietokannassa perusmuodon kohde-palvelimeen ja löytää uuden vCenter palvelimen lisätään VMware näennäiskoneiden. Seuraavissa tilanteissa haluat ehkä muuttaa käyttöönoton prosessi-palvelimeen:

- Jos nykyisen prosessin palvelimen siirtyy
- Jos organisaatiosi voi hyväksyä tasolle palautus pisteen tavoitteen (RPO).

Tarvittaessa voit siirtää joitakin tai kaikkia paikallisen VMware näennäiskoneiden ja fyysinen palvelimien replikoinnin toinen prosessi-palvelimeen. Esimerkki:

- **Virhe**, jos prosessin palvelimen epäonnistuu, tai se ei ole käytettävissä voit siirtää suojattujen kone replikoinnin toinen prosessi-palvelimeen. Metatietojen lähde machine ja replikan koneen siirretään uusi prosessi-palvelin ja tietoja synkronoidaan. Uusi prosessi-palvelin automaattisesti yhteyden automaattinen etsiminen suorittamiseen vCenter-palvelimeen. Voit valvoa prosessin palvelimissa palauttaminen Raporttinäkymät-ikkunan tila.
- **Säädä RPO kuormituksen**– parannettu kuormituksen tasaamisen, voit valita eri prosessin palvelimen palauttaminen-portaalissa, ja siirtää sen yhdestä tai useammasta koneesta replikoinnin manuaalinen kuormituksen tasaamisen. Tässä tapauksessa valitun lähde- ja replikan koneet metatiedot siirretään uuteen prosessi-palvelimeen. Alkuperäisen prosessin palvelimen säilyy vCenter palvelimeen. 

### <a name="monitor-the-process-server"></a>Näytön prosessi-palvelin

Jos prosessin palvelin on tila varoitus näkyvät sivuston palauttaminen koontinäytössä kriittinen-tilaan. Voit valita Avaa tapahtumat-välilehti ja sitten Siirry alirakenteeseen tiettyihin töihin työt-välilehden tilasta. 

### <a name="modify-the-process-server-used-for-replication"></a>Muokkaa replikoinnin käytetään prosessin palvelimeen

1. Avaa **palvelinten** > **Configuration Servers** > määrityspalvelimen > **Palvelimen tiedot**.
2. Valitse **Prosessi-palvelimia** > **Muuta prosessin palvelinta** haluat muokata palvelimen nimen vieressä.

    ![Muuta prosessin palvelinta 1](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps1.png)

3. **Muuta prosessin**Serverissä > **Kohde prosessin palvelimen** valita uuden palvelimen haluat käyttää ja valitse sitten näennäiskoneiden, jonka haluat kopioida uuteen palvelimeen. Valitse tilaa ja käytetty muistin tiedot palvelimen nimi-kohdan vieressä olevaa tiedot-kuvaketta. Keskimääräinen paikkaan, jota tarvitaan replikoida kunkin valitun virtuaalikoneen palvelimeen prosessin näkyy tehdä ladata päätöksiä.

    ![Muuta prosessin palvelinta 2](./media/site-recovery-vmware-to-azure-classic-legacy/change-ps2.png)

4. Valitse Aloita uusi prosessin palvelimeen replikoiminen valintamerkkiä. Huomaa, että jos poistat kaikki näennäiskoneiden prosessin palvelimesta, joka oli kriittinen enää näytetäänkö kriittinen varoitus koontinäytön.


## <a name="third-party-software-notices-and-information"></a>Kolmannen osapuolen ilmoituksia ja tietoja

Älä kääntäminen tai lokalisoidaan

Ohjelmiston ja laiteohjelmiston käynnissä Microsoftin tuote tai palvelu perustuu tai kattaa materiaali alla projekteista (yhdessä "kolmannen osapuolen Code").  Microsoft ei ole häneltä kolmannen osapuolen-koodin.  Alkuperäinen tekijänoikeusmerkintä sekä käyttöoikeus-kohdassa Microsoft saanut kolmannen osapuolen esimerkiksi koodi on määritetty alla.

Kohdassa A liittyy kolmannen osapuolen koodin osat alla luetellut projektit. Nämä käyttöoikeudet ja tiedot on tarkoitettu vain tiedoksi.  Kolmannen osapuolen koodi on parhaillaan relicensed sinulle Microsoft Microsoftin ohjelmistojen käyttöoikeusehdot, Microsoftin tuote tai palvelu-kohdassa.  

Tietoja b osassa liittyy kolmannen osapuolen koodin osat, jotka tehdään käytettävissä sinulle Microsoft alkuperäisen Käyttöoikeussopimuksen ehtojen mukaisesti.

Koko tiedoston löytyy [Microsoft Download Centeristä](http://go.microsoft.com/fwlink/?LinkId=529428). Microsoft pidättää kaikki oikeudet ole nimenomaisesti myönnetty, onko toteutukseen, vastaväitteitä tai muulla tavoin.
