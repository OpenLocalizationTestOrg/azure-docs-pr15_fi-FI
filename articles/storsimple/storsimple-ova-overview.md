<properties
   pageTitle="StorSimple Virtual matriisin yleiskatsaus | Microsoft Azure"
   description="Tässä artikkelissa kuvataan StorSimple Virtual matriisi, integroitu tallennustilan-ratkaisun, joka hallitsee tallennustilan tehtävät paikallisen virtual laitteen ja Microsoft Azure-pilvitallennustilaa välillä."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/06/2016"
   ms.author="alkohli" />

# <a name="introduction-to-the-storsimple-virtual-array"></a>Johdanto StorSimple Virtual matriisi

## <a name="overview"></a>Yleiskatsaus

Tervetuloa käyttämään Microsoft Azure StorSimple Virtual matriisin integroitu tallennustilan-ratkaisun, joka hallitsee tallennustilan tehtävät-paikallisen virtual laitteessa hypervisor ja Microsoft Azure-pilvitallennustilaa välillä. Virtuaalinen matriisi (tunnetaan myös nimellä StorSimple paikallisen virtual laite) on tehokas, edullinen ja helposti ohjattavan tiedostopalvelimeen tai iSCSI server-ratkaisun, joka korjaa useita ongelmia ja yrityksen tallennustilan ja tietojen suojaukseen liittyviä kulut. Virtuaalinen matriisi on erityisen sovellu remote office/haaran office (ROBO) skenaarioita.

Tämä yleiskatsaus keskitytään Virtual matriisi. 

- Yleiskatsaus StorSimple 8000 sarjan, siirry [StorSimple 8000 sarja: hybrid cloud-ratkaisun](storsimple-overview.md). 

- Lisätietoja StorSimple 5000/7000 sarjan laitteella Siirry [StorSimple Online-ohje](http://onlinehelp.storsimple.com/).

Virtuaalinen matriisin tukee iSCSI tai lohkon (SMB)-protokollaa. Sen aiempaan hypervisor-infrastruktuuriin suoritetaan ja tarjoaa tiering pilveen, cloud varmuuskopiointi, nopea palauttaminen, Kohdetason palauttaminen ja tietojen palauttaminen ominaisuuksia.

Seuraavassa taulukossa on yhteenveto Virtual matriisin tärkeitä ominaisuuksia.

| Toiminto | Virtuaalinen matriisi |
| ------- | ------------- |
|Asennusvaatimukset | Käyttää virtualization infrastruktuuri (Hyper-V tai VMware)|
| Käytettävyys | Yksittäinen solmu |
| Kokonaiskapasiteetti (mukaan lukien cloud) |64 TT käytettävä kapasiteetin virtual laite |
| Paikallinen kapasiteetti | 390 gt 6.4 TT käytettävä kapasiteettiin virtual laite (tarvetta valmisteleminen – 8 TT 500 Gigatavua levytilaa)|
| Alkuperäisen protokollat | iSCSI tai SMB |
| Palautus aika tavoitteen (RTO) | iSCSI: pienempi kuin 2 minuuttia koosta riippumatta |
| Palautus piste tavoitteen (RPO) | Päivittäinen varmuuskopiot ja tarvittaessa varmuuskopiointi |
| Tallennustilan tiering | Käyttää lämmitetään yhdistämisen, voit selvittää, mitä tietoja olisi tasoisen sisään tai ulos |
| Tuki | Toimittajan tukemat Virtualization infrastruktuuri |
| Suorituskyky | Vaihtelee riippuen pohjana oleva infrastruktuuri |
| Tietoja liikkuvuus | Voit palauttaa samaan laitteeseen tai Kohdetason palauttaminen (tiedoston server) |
| Tallennustilan tasoa | Paikallinen hypervisor säilytys- ja pilveen |
| Jaa kokoa |Tasoisen: enintään 20 Teratavua; paikallisesti kiinnitetyt: enintään 2 Teratavua |
| Asemakoko |Tasoisen: 5 Teratavua; määrittäminen paikallisesti kiinnitetyt: enintään 500 Gigatavua |
| Tilannevedokset | Yhtenäinen kaatumisen |
| Kohdetason palauttaminen | Kyllä. käyttäjät voivat palauttaa osakkeet |

## <a name="why-use-storsimple"></a>StorSimple käyttötarkoitus

StorSimple muodostaa yhteyden Azure tallennustilan käyttäjien ja palvelinten minuutteina sovelluksen tekemättä muutoksia.

Seuraavassa taulukossa on kuvattu joitakin keskeisiä toimintoja, joka sisältää Virtual matriisi-ratkaisun.

| Toiminto | Etu |
|---------|---------|
| Läpinäkyvä integrointi | Virtuaalinen matriisin tukee iSCSI tai SMB-protokollaa. Paikallinen ylätasolla ja cloud tason tietojen-siirto on saumattoman ja läpinäkyvä käyttäjälle.|
| Rajoitetun tallennustilan kustannukset | StorSimple, jossa valmistella riittävästi paikallinen tallennus täyttämään nykyisen useimmin käytetyt kuuma tiedot. Tallennustilan tarpeiden kasvaa, StorSimple tasoa kylmän tietojen tuominen edullinen cloud tallennustilan. Tietoja deduplicated ja pakattu ennen kuin lähetät pilveen Pienennä Lisää tallennustilaa ja kulut.|
| Vaivaton tallennusvälineiden hallinta | StorSimple tarjoaa keskitetyn hallinnan StorSimple hallinnan avulla voit hallita useita laitteita pilveen.| 
| Parannettu palauttaminen ja vaatimustenmukaisuus | StorSimple helpottaa nopeampaa palauttaminen palauttaminen metatiedot välittömästi ja palauttaminen tietoja tarpeen mukaan. Tämä tarkoittaa mahdollisimman vähän häiriöitä jatkaa Normaali toimintoja.|
| Tietoja liikkuvuus | Tietoja tasoisen pilveen niitä voi käyttää muiden sivustojen palauttaminen ja siirtoa varten. Huomaa, että voit palauttaa tiedot vain alkuperäisen Virtual matriisi. Voit palauttaa koko matriisin Virtual Virtual toisen taulukon tietojen palauttaminen ominaisuuksien avulla.|

## <a name="storsimple-workload-summary"></a>StorSimple kuormituksen yhteenveto

Alla on taulukkomuotoinen yhteenveto tuettujen StorSimple toiminnoista.

| Skenaario                | Kuormituksen              | Tuettu |  Rajoitukset                                  | Versio              |
|-------------------------|-----------------------|-----------|------------------------------------------------|----------------------|
| ROBO yhteistyö      | Tiedostojen jakaminen          | Kyllä       | Katso [tiedostopalvelimeen suurin rajoitukset](storsimple-ova-limits.md). <br>Katso [Tuetut SMB-versiot järjestelmävaatimukset](storsimple-ova-system-requirements.md).   | Kaikki versiot      |


## <a name="workflows"></a>Työnkulut

StorSimple Virtual matriisin sopii erityisen seuraavat työnkulut:

- [Pilvipohjainen hallinta](#cloud-based-storage-management)
- [Sijainti riippumattomalla varmuuskopiointi](#location-independent-backup)
- [Tietojen suojauksen ja tietojen palauttaminen](#data-protection-and-disaster-recovery)

### <a name="cloud-based-storage-management"></a>Pilvipohjainen hallinta

StorSimple hallintapalvelu käynnissä Azure perinteinen-portaalissa voit hallita useita laitteita ja useisiin sijainteihin tallennettuja tietoja. Tämä on erityisen hyödyllinen hajautettu haaran tilanteissa. Huomaa, että sinun on luotava eri esiintymien StorSimple hallintapalvelu Virtual matriisin ja fyysisten StorSimple laitteiden hallinta. 

![pilvipohjainen hallinta](./media/storsimple-ova-overview/cloud-based-storage-management.png)

### <a name="location-independent-backup"></a>Sijainti riippumattomalla varmuuskopiointi

Virtuaalinen matriisi, jossa cloud tilannevedoksia säätää äänenvoimakkuutta tai Jaa sijainti riippumattomalla, ajankohta kopio. Cloud tilannevedoksia ovat oletusarvoisesti käytössä ja ei voi poistaa käytöstä. Kaikki asemat ja osakkeet ovat varmuuskopioinnin määrittäminen samanaikaisesti yhden päivän varmuuskopion käytännön avulla ja saat käyttöösi lisää tietokoneiden välinen varmuuskopioiden tarvittaessa.

### <a name="data-protection-and-disaster-recovery"></a>Tietojen suojauksen ja tietojen palauttaminen

Virtuaalinen matriisin tukee tietojen suojauksen ja tietojen palauttaminen tilanteista:

- **Äänenvoimakkuutta tai Jaa palauttaminen** – käyttää Palauta uusi työnkulku palauttamaan äänenvoimakkuutta tai Jaa. Tämän menetelmän avulla voit palauttaa koko äänenvoimakkuutta tai Jaa.
- **Kohdetason palautus** – osakkeet Salli yksinkertaistettu käyttää viimeisimmät varmuuskopiot. Voit palauttaa yksittäisen tiedoston helposti pilveen käytettävissä erityistä .backup-kansiosta. Palauta tämä ominaisuus on käyttäjän perustuva ja järjestelmänvalvojan mitään toimia ei tarvita.
- **Tietojen palauttaminen** – Käytä palauttaa kaikki asemat tai osakkeet uuden Virtual matriisin automaattisesti-ominaisuus. Luo uuden Virtual matriisin ja rekisteröi StorSimple hallinta-palvelussa ja valitse epäonnistua alkuperäisen Virtual matriisin päälle. Uuden Virtual matriisin sitten olettaa valmistellun resurssit. 

## <a name="virtual-array-components"></a>Virtuaalinen taulukon osat

Virtuaalinen taulukko sisältää seuraavat osat:

- [Virtuaalinen matriisin](#virtual-array) – hybrid cloud tallennusväline valmisteltu virtualisoidussa ympäristössä tai hypervisoria virtual koneen perusteella.  
- [StorSimple hallinta](#storsimple-manager-service) – tiedostotunnistetta Azure perinteinen portaalin, jolla voit hallita vähintään yksi StorSimple laitteiden yksittäisen web-liittymän, voit käyttää eri paikoista. Voit luoda ja palveluiden hallinta, tarkastella ja laitteet ja ilmoitusten hallinta ja asemat, osakkeet ja aiemmin tilannevedoksia hallinta StorSimple hallintapalvelu.
- [Paikallisen sivuston käyttöliittymä](#local-web-user-interface) – verkkopohjaisia Käyttöliittymä, jota käytetään laitteen määrittäminen niin, että se Muodosta yhteys paikalliseen verkkoon ja rekisteröi sitten laitteen StorSimple hallinta-palvelussa. 
- [Käyttöliittymä](#command-line-interface) – Windows PowerShell-liittymän, joiden avulla voit aloittaa tuki-istunnon Virtual matriisi.
Seuraavissa osissa kuvataan tarkemmin näistä osat sekä kerrotaan, miten ratkaisun järjestää tiedot, varaa tallennustilan ja helpottaa tallennusvälineiden hallinta- ja tietosuojan.

### <a name="virtual-array"></a>Virtuaalinen matriisi

Virtuaalinen matriisi on yksi solmu tallennustilan ratkaisun, joka ensisijainen tallennustila, hallitsee pilvitallennustilaa tietoliikenteen ja varmistaa suojaus ja luottamuksellisuus kaikki tiedot, jotka on tallennettu laitteeseen.

Virtuaalinen matriisi on käytettävissä yksi malli, joka on ladattavissa. Tallennustilan-matriisissa on suurin kapasiteetti 6.4 teratavua laitteen (ja pohjana olevan tallennustilan vaatimus 8 teratavua)-ja 64 TT mukaan lukien cloud tallennustilan. 

Virtuaalinen matriisissa on seuraavia ominaisuuksia:

- On edullinen. Se helpottaa Käytä aiemmin virtualisointi-infrastruktuuria ja voit ottaa käyttöön aiemmin Hyper-V tai VMware hypervisor.
- Se sijaitsee joten ja voi määrittää iSCSI-palvelimeen tai tiedostopalvelimeen. 
- Se on integroitu pilveen.
- Varmuuskopioiden on tallennettu pilvipalveluun, jotka helpottavat tietojen palauttaminen ja yksinkertaistaa Kohdetason palauttaminen (ILR). 
- Voit käyttää päivitykset Virtual matriisia vain, kun käyttää niitä fyysinen laitteeseen.

>[AZURE.NOTE] Virtuaalinen matriisin ei voi laajentaa. Tämän vuoksi on tärkeää valmistelu riittävä tallennustilan, kun luot virtual laite. 

### <a name="storsimple-manager-service"></a>StorSimple hallinta

Microsoft Azure StorSimple on verkkopohjainen käyttöliittymä (StorSimple-hallintapalvelu), jonka avulla voit hallita palvelinkeskuksen ja tallennustilaa cloud keskitetysti. StorSimple hallinta-palvelun avulla voit suorittaa seuraavia tehtäviä:

- Hallitse useita StorSimple Virtual matriisin yhden-palvelusta. 
- Määritä ja StorSimple laitteet tietoturva-asetusten hallinta. (Salaus pilveen on riippuvainen Microsoft Azure-ohjelmointirajapinnan.)
- Tallennustilan tilin tunnistetiedot ja ominaisuuksien määrittäminen.
- Määritä ja hallitse asemat tai osakkeet.
- Varmuuskopiointi ja palauttaminen asemat tai osakkeet tiedot.
- Valvoa suorituskykyä.
- Tarkista järjestelmäasetuksista ja mahdolliset ongelmat.

StorSimple hallinta-palvelun avulla voit suorittaa päivittäiset hallinnan Virtual matriisin.

Lisätietoja on Katso [StorSimple hallintapalvelu ammattimainen StorSimple laitteen](storsimple-manager-service-administration.md).

### <a name="local-web-user-interface"></a>Paikallisen sivuston käyttöliittymä

Virtuaalinen taulukko sisältää verkkopohjaisia Käyttöliittymä, jota käytetään erikseen määritys ja StorSimple hallinta-palvelussa laitteen rekisteröinti. Sen avulla voit Sammuta ja Käynnistä Virtual matriisi, diagnostiikan testit, päivitä ohjelmisto, laitteen järjestelmänvalvojan salasanan vaihtaminen, tarkastella järjestelmälokit ja ota yhteyttä Microsoftin Support, palvelupyynnön. 

Siirry tietoja verkkopohjaisia käyttöliittymässä on [ammattimainen StorSimple Virtual matriisin verkkopohjaisia-Käyttöliittymän käyttöä](storsimple-ova-web-ui-admin.md).

### <a name="command-line-interface"></a>Käyttöliittymä

Sisältää Windows PowerShell-liittymän avulla voidaan aloittaa tuki-istunnon Microsoft Support niin, että ne auttavat ja ratkaista ongelmat, jotka voi tulla virtual laitteessasi.

## <a name="storage-management-technologies"></a>Tallennustilan hallinta tekniikat

Virtuaalinen taulukon ja muiden osien lisäksi StorSimple-ratkaisun käyttää ottaa nopeasti käyttöön tärkeitä tietoja ja vähentää tallennustilan kulutus Virtual matriisin tallennettujen tietojen suojaaminen seuraavia ohjelmistotekniikoita:

- [Automaattinen tallennustilan tiering](#automatic-storage-tiering) 
- [Paikallisesti kiinnitetty osakkeet ja asemat](#locally-pinned-shares-and-volumes)
- [Kopioinnin peruuttaminen ja tietojen pakkaus tasoisen tai pilvipalveluun varmuuskopioida](#deduplication-and-compression-for-data-tiered/backed-up-to-the-cloud) 
- [Aikataulun mukainen ja tarvittaessa varmuuskopioinnin](#scheduled-and-on-demand-backups)

### <a name="automatic-storage-tiering"></a>Automaattinen tallennustilan tiering

Virtuaalinen matriisin käyttää uuden tiering järjestelmä Virtual matriisi ja pilveen tallennettujen tietojen hallintaa. On vain kaksi tasoa: paikallisen Virtual matriisi ja Azure cloud tallennustilan. StorSimple Virtual matriisin järjestää tiedot automaattisesti tasoa lämpökarttaan, joka seuraa nykyinen käyttö, ikä ja yhteyksiä eri tietojen perusteella. Tiedot, jotka ovat Aktiivisimmat (Upeimmat) tallennetaan paikallisesti, kun vähemmän aktiivinen ja passiiviset tiedot siirretään automaattisesti pilveen. (Kaikki varmuuskopiot on tallennettu pilvipalveluun.) StorSimple säätää ja järjestää tietoja ja tallennustilaa palvelujen käyttöä käyttötavat muuttaminen. Esimerkiksi tietoja voi olla pienempi active ajan kuluessa. Kun muuttuu vähitellen vähemmän aktiivinen, se on tasoisen ulos pilveen. Jos samaa tiedot aktivoituu uudelleen, se on tasoisen tallennustilan matriisia.

Tietoja tietyn Porrastettu Jaa ja äänenvoimakkuus on taata oma paikallinen ylätasolla tilaa. (noin 10 prosenttia valmistellun tilaa yhteensä Jaa tai asema). Kun tämä pienentää tallennustilaa virtual laitteeseen Jaa tai asema, se varmistaa, että tiering yhden Jaa tai asema ei vaikuttaa muihin osakkeet tai asemat tiering tarpeisiin. Näin varattu työmäärää Jaa tai asema ei voi pakottaa kaikki muut pilveen työmäärät. 

![Automaattinen tallennustilan tiering](./media/storsimple-ova-overview/automatic-storage-tiering.png)

>[AZURE.NOTE] Voit määrittää aseman kuin paikallisesti kiinnitetty, jolloin tiedot säilyvät Virtual matriisi ja ei koskaan tasoisen pilveen. Jos haluat lisätietoja, siirry [kiinnitetyt paikallisesti osakkeet ja asemat](#locally-pinned-shares-and-volumes).

### <a name="locally-pinned-shares-and-volumes"></a>Paikallisesti kiinnitetty osakkeet ja asemat

Voit luoda haluamasi osakkeet ja asemat kiinnitetyt kuin paikallisesti. Tätä ominaisuutta varmistaa, että tärkeät sovellukset vaatii tietojen pysyy Virtual matriisissa, ja on aina tasoisen pilveen. Paikallisesti kiinnitetty osakkeet ja asemat ovat seuraavat toiminnot: 

- Ne eivät ole cloud viiveet suurempia tai yhteysongelmat.
- Ne edelleen hyötyvät StorSimple cloud varmuuskopiointi- ja tietojen palauttaminen ominaisuuksia.

Voit palauttaa paikallisesti kiinnitetty Jaa tai asema, kuten tasoisen tai Porrastettu Jaa tai asema kuin paikallisesti kiinnitetyt. 

Lisätietoja paikallisesti kiinnitetty asemat Tutustu [StorSimple hallintapalvelu hallittavan asemat](storsimple-manage-volumes-u2.md).

### <a name="deduplication-and-compression-for-data-tiered-or-backed-up-to-the-cloud"></a>Kopioinnin peruuttaminen ja tietojen pakkaus tasoisen tai pilvipalveluun varmuuskopioida

StorSimple käyttää kopioinnin peruuttaminen ja tietojen pakkaus vähentää edelleen tallennuksen vaatimukset pilveen. Kopioinnin peruuttaminen vähentää yleinen poistamalla redundanssin tallennetut tietojoukon tallennetut tiedot. Kun tiedot muuttuvat, StorSimple ohittaa muuttumattomina tiedot ja sieppaa vain muutokset. Lisäksi StorSimple vähentää tallennettujen tietojen tunnistaminen ja poistamalla samoja tietoja. 

>[AZURE.NOTE] Virtuaalinen matriisin tallennetut tiedot deduplicated tai pakata ei. Kaikki deduplication ja pakkaaminen, ennen kuin tiedot lähetetään pilveen.

### <a name="scheduled-and-on-demand-backups"></a>Aikataulun mukainen ja tarvittaessa varmuuskopioinnin

StorSimple tietojen suojauksen ominaisuuksien avulla voit luoda tarvittaessa varmuuskopiot. Lisäksi oletusarvoisesti varmuuskopio aikataulun varmistaa, että tiedot varmuuskopioidaan päivittäin. Vaiheittainen tilannevedoksia, jotka on tallennettu pilvipalveluun muodossa otetaan varmuuskopioita. Tilannevedoksia, joka tallentaa vain muutokset edellisen varmuuskopioinnin jälkeen, voit luoda ja palauttaa nopeasti. Nämä tilannevedoksia voi olla hyvin tärkeä tietojen palauttaminen tilanteissa, koska korvaa toissijainen tallennusväline järjestelmien (kuten nauha varmuuskopio) ja voit palauttaa tietoja oman palvelinkeskukseen tai vaihtoehtoinen sivustoihin tarvittaessa.

## <a name="next-steps"></a>Seuraavat vaiheet

Lisätietoja valmistelemisesta [Virtual matriisi-portaalissa](storsimple-ova-deploy1-portal-prep.md).


