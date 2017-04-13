<properties 
   pageTitle="Parhaat käytännöt StorSimple Virtual matriisin | Microsoft Azure"
   description="Raportissa käsitellään parhaita käytäntöjä käyttöönottoon ja hallintaan StorSimple Virtual matriisi."
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
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="alkohli" />

# <a name="storsimple-virtual-array-best-practices"></a>StorSimple Virtual matriisin parhaat käytännöt

## <a name="overview"></a>Yleiskatsaus

Microsoft Azure StorSimple Virtual matriisi on integroitu tallennustilan-ratkaisun, joka hallitsee tallennustilan tehtävät-paikallisen virtual laitteessa hypervisor ja Microsoft Azure-pilvitallennustilaa välillä. StorSimple Virtual matriisi on tehokas ja edullinen vaihtoehtoinen 8000 sarjan fyysinen matriisia. Virtuaalinen matriisi voidaan suorittaa aiemmin hypervisor-infrastruktuuria, tukee iSCSI ja SMB-protokollat ja on sovellu remote office/haaran office-skenaariot. Lisätietoja StorSimple ratkaisuja Siirry [Microsoft Azure StorSimple yleiskatsaus](https://www.microsoft.com/en-us/server-cloud/products/storsimple/overview.aspx).

Tässä artikkelissa käsitellään parhaita käytäntöjä toteutettu ensimmäisen määrityskerran, käyttöönoton ja hallinnan StorSimple Virtual matriisin aikana. Näiden vinkkien avulla vahvistettu ohjeita asennuksen ja virtual matriisin hallinta. Tässä artikkelissa suunnataan IT-järjestelmänvalvojille, jotka käyttöön ja hallita niiden palvelinkeskusten virtual matriiseja.

On suositeltavaa säännöllisiä Tarkista parhaita käytäntöjä varmistaa laite on yhä yhteensopivuuden kun asetukset tai toiminnon kulun tehdään muutoksia. Kun täytäntöön näiden parhaiden käytäntöjen virtual matriisi, [Ota yhteys Microsoftin tuotetukeen](storsimple-contact-microsoft-support.md) järjestelmänvalvojalta ongelmia ilmenee.

## <a name="configuration-best-practices"></a>Määritysten parhaat käytännöt 

Näiden vinkkien kattavat ohjeet, joka on ensimmäinen asennus ja virtual matriisien käyttöönoton aikana esitetään. Näiden vinkkien kuuluvat liittyvät virtuaalikoneen ryhmäkäytäntöasetukset koon, määrittämisestä verkko, tallennustilan tilien määrittäminen ja osakkeet ja asemat virtual matriisin luominen valmistelu. 

### <a name="provisioning"></a>Valmistelu 

StorSimple Virtual matriisi on virtual machine (AM), joka on valmisteltu hypervisor (Hyper-V tai VMware) Valitse host Serverin. Virtuaalikoneen valmisteltaessa varmistaa, että isäntä pystyy tälle riittävät resurssit. Saat lisätietoja Siirry [Pienin resurssivaatimusten](storsimple-ova-deploy2-provision-hyperv.md#step-1-ensure-that-the-host-system-meets-minimum-virtual-device-requirements) valmistelu matriisin. 

Toteuta seuraavien parhaiden käytäntöjen virtual matriisin valmisteltaessa:


|                        | Hyper-V                                                                                                                                        | VMware                                                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| **Virtuaalikoneen tyyppi**   | **Luonti 2** Käytä Windows Server 2012: ssa ja sitä uudemmissa versioissa ja *.vhdx* kuva AM. <br></br> **Luonti 1** Käytä Windows Server 2008: n ja sitä uudemmissa versioissa ja *.vhd* kuva AM.                                                                                                              | Käytä virtuaalikoneen version 8-11 *.vmdk* kuva.                                                                      |
| **Muistityyppi**            | Määritä **staattista muistin**. <br></br> Älä käytä **dynaaminen muistin** -vaihtoehto.            |                                                    |
| **Levy-tietotyyppi**         | Valmistele kuin **dynaamisesti laajentaminen**.<br></br> **Kiinteän kokoinen** kestää kauan. <br></br> Älä käytä **Erotusmenetelmä** -vaihtoehto.                                                                                                                   | Käytä **ohuen säännöstä** -vaihtoehtoa.                                                                                      |
| **Tietojen levyn muokkaaminen** | Laajentaminen ja pienentäminen ei ole sallittua. Yritys kiellä hakutulosten paikallisten tietojen laitteessa menetys.                       | Laajentaminen ja pienentäminen ei ole sallittua. Yritys kiellä hakutulosten paikallisten tietojen laitteessa menetys. |

### <a name="sizing"></a>Koon muuttaminen

Kun koon StorSimple Virtual matriisin, ota huomioon seuraavat seikat:

- Paikallinen varaus asemat tai osakkeet. Noin 12 prosenttia tila on varattu paikallinen ylätasolla kunkin valmistellun Porrastettu äänenvoimakkuutta tai Jaa. Suurin piirtein 10 % tilan myös on varattu tiedostojärjestelmässä paikallisesti kiinnitetty voimakkuutta.
- Yleiskustannus tilannevedoksen. Noin 15 % tilaa paikallinen ylätasolla on varattu tilannevedoksia.
- Palauttaa tarve. Jos teet palauttaminen kuin uuden toiminnon, koonmuuttokahvan olisi huomioon tarvittava Palauta tila. Palautus on valmis Jaa tai yhtä suuri määrä tai suurempi.
- Jotkin puskurin kohdistetaan minkä tahansa odottamattomia kasvun.

Edellisen tekijöiden perusteella koonmuuttokahvan vaatimukset voidaan esittää seuraavan kaavan avulla:

`Total usable local disk size = (Total provisioned locally pinned volume/share size including space for file system) + (Max (local reservation for a volume/share) for all tiered volumes/share) + (Local reservation for all tiered volumes/shares)`

`Data disk size = Total usable local disk size + Snapshot overhead + buffer for unexpected growth or new share or volume`


Seuraavissa esimerkeissä kuvataan, miten voit muuttaa kokoa virtual taulukko, joka on tarpeen mukaan.

#### <a name="example-1"></a>Esimerkki 1:
Virtuaalinen-matriisin haluat, että voit 

- Valmistele 2 Teratavua Porrastettu äänenvoimakkuutta tai Jaa.
- Valmistele 1 TT Porrastettu äänenvoimakkuutta tai Jaa.
- Valmistele 300 Gigatavua paikallisesti kiinnitetty äänenvoimakkuutta tai jakaa.


Edellisen asemat tai osakkeet Laske uutiskirjeistä levytila paikallinen ylätasolla. 

Ensin kunkin Porrastettu aseman/osakkeen, Paikallinen varaus olisi 12 prosenttia aseman/Jaa kokoa. Paikallisesti kiinnitetty aseman/Jaa-paikallisen varaus on 10 % paikallisesti kiinnitetty äänenvoimakkuutta tai Jaa koosta (lisäksi valmistellun kokoa). Tässä esimerkissä tarvitset

- 240 gt paikallisen varaus (for 2 Teratavua tasoisen äänenvoimakkuutta tai Jaa)
- 120 gt paikallisen varaus (1 TT: n tasoisen äänenvoimakkuutta tai Jaa)
- 330 Gigatavua paikallisesti kiinnitetty äänenvoimakkuutta tai Jaa (lisääminen 300 gt valmisteltu koon paikallisen varaus 10 prosenttia)

Tarvittava paikallinen ylätasolla mennessä yhteensä tila on: 240 gt + 120 gt + 330 gt = 690 Gigatavua.

Seuraavaksi vähintään haluamasi määrä tilaa paikallinen ylätasolla annettava suurin yhteen varauksen nimellä. Ylimääräisen summa käytetään siltä varalta, että haluat palauttaa cloud tilannevedoksen. Tässä esimerkissä suurin paikallisen varaus on 330 Gigatavua (mukaan lukien tiedostojärjestelmässä varaus), niin, että 690 gt: 690 gt + 330 gigatavua = 1020 gt Lisää.
Jos myöhemmin muita palauttaa on tehty, emme voit vapauttaa aina edellisen palautus-tilaa.

Kolmas 15 % mennessä tallentamiseen paikallisen tilannevedoksia Space yhteensä paikallisen annettava siten, että vain 85 %, se on käytettävissä. Tässä esimerkissä, joka olisi noin 1 020 gt = 0.85&ast;valmistellun tietojen levyn Teratavua. Siis valmistellun tietojen levy olisi (1020&ast;(1/0.85)) = 1 200 gt = 1,20 TT ~ 1,25 TT (pyöristää lähimpään neljännes)

Odottamattomia kasvu ja uusi palauttaa takautumisoikeuksin pitäisi valmistella Paikallinen levy ja ympärille 1,25-1,5 Teratavua.

> [AZURE.NOTE] Suosittelemme, että Paikallinen levy on levinneet valmistelun yhteydessä. Tämä suositus johtuu Palauta tila tarvitaan vain, kun haluat palauttaa tiedot, jotka ovat vanhempia kuin viisi päivää. Kohdetason palautus avulla voit palauttaa tietoja tarvitsematta ylimääräistä tilaa palauttaminen viimeksi viisi päivää.

#### <a name="example-2"></a>Esimerkki 2: 
Virtuaalinen-matriisin haluat, että voit 

- Valmistele 2 Teratavua tasoisen aseman
- paikallisesti kiinnitetyt 300 gt aseman valmistelu

Perusteella 12 prosenttia Porrastettu asemat/osakkeet varauksen paikallisen tilaa ja 10 prosenttia paikallisesti kiinnitetty asemat/osakkeet annettava

- 240 gt paikallisen varaus (for 2 Teratavua tasoisen äänenvoimakkuutta tai Jaa)
- 330 Gigatavua paikallisesti kiinnitetty äänenvoimakkuutta tai Jaa (lisääminen valmisteltu 300 gt tilaa paikallisen varaus 10 prosenttia)

Paikallinen ylätasolla tarvittava tila on: 240 gt + 330 gt = 570 gt

Tarvittava palauttaminen pienin paikallisen tila on 330 Gigatavua. 

15 % yhteensä levyn käytetään tallentamiseen tilannevedoksia siten, että vain 0.85 on käytettävissä. Tällöin levyn kokoa on (900&ast;(1/0.85)) = 1.06 TT ~ 1,25 TT (pyöristää lähimpään neljännes) 

Minkä tahansa odottamattomia kasvun takautumisoikeuksin voit valmistella 1,25-1,5 TT Paikallinen levy.


### <a name="group-policy"></a>Ryhmäkäytäntö

Ryhmäkäytäntö on infrastruktuuri, jonka avulla voit toteuttaa käyttäjien ja tietokoneiden tiettyjä määrityksiä. Ryhmäkäytäntöasetukset sisältyvät Ryhmäkäytäntö-objektit (ryhmäkäytäntöobjekti), jotka on linkitetty seuraavat Active Directory-toimialueen palveluista (AD DS) säilöjä: sivustot, toimialueiden tai organisaatioyksiköiden (organisaatioyksiköiden). 

Jos virtual matriisi on toimialueeseen liittymistä, ryhmäkäytäntöobjekteja voi suojata siihen. Nämä ryhmäkäytäntöobjektit asentaa sovellukset, kuten virustentorjuntaohjelmiston, jotka voivat haitata StorSimple virtuaalinen taulukon toiminta.

Tästä syystä suosittelemme, voit:

-   Varmista, että virtual matriisi on oma organisaatioyksikkö (OU) Active Directoryn. 

-   Varmista, että ei ole ryhmäkäytännön ryhmäkäytäntöobjekteja käytetään virtual matriisi. Voit estää periytymisen ja varmistaa, että virtual matriisin (alisolmu) automaattisesti Peri minkä tahansa ryhmäkäytäntöobjekteja ylemmän tason. Jos haluat lisätietoja, siirry [periytymisen](https://technet.microsoft.com/library/cc731076.aspx).


### <a name="networking"></a>Verkko

Verkon määritysten virtual matriisin tehdään paikallisen sivuston Käyttöliittymän avulla. VPN-liittymän on otettu käyttöön, jossa virtual matriisissa on valmisteltu hypervisor kautta. [Verkkoasetukset](storsimple-ova-deploy3-fs-setup.md) -sivun avulla voit määrittää VPN-liittymän IP-osoite, aliverkon ja yhdyskäytävä.  Voit myös määrittää ensisijaisen ja toissijaisen DNS-palvelimeen, asetuksiin ja valinnainen välityspalvelimen laitteeseesi. Useimmat verkon määritys on erikseen asetukset. Tarkista ennen käyttöönottoa virtual matriisin [StorSimple verkko vaatimuksia](storsimple-ova-system-requirements.md#networking-requirements) .

Virtuaalinen matriisin otettaessa Suosittelemme noudattamalla näitä parhaita käytäntöjä:

-   Varmista, että joka virtual matriisin otetaan käyttöön aina verkon kapasiteetin tälle 5 Mbps Internet-kaistanleveyden (tai enemmän). 

    -   Internet-kaistanleveys on vaihtelee riippuen työmäärää ominaisuudet ja tiedot muuttuvat.

    -   Tietojen muuttaminen voi käsitellä on suoraan Internet-kaistanleveys. Kun ottaen varmuuskopion esimerkiksi 5 Mbps kaistanleveyden mahtuu tietojen muutoksen 18 gigatavua kahdeksan tuntia. Neljä kertaa enemmän (20 Mbps), jossa voit käsitellä neljä kertaa Lisää tietojen muuttaminen (72 gt). 

-   Varmista Internet-yhteys on aina käytettävissä. Arviointi tai epäluotettavista Internet-yhteydet laitteisiin voi aiheuttaa pääse tietojen pilveen, mikä saattaa johtaa ei tueta määrityksessä.

-   Jos haluat ottaa laitteen iSCSI palvelimeksi: 
    -   On suositeltavaa, että poistat **Hae IP-osoite automaattisesti** -asetuksen (DHCP). 
    -   Staattinen IP-osoitteiden määrittäminen. Sinun on määritettävä ensisijainen ja Toissijainen DNS-palvelin.

    -   Jos useita verkkoliittymät määrittäminen-kohdassa virtual-matriisi, vain ensimmäinen verkkoliittymän (oletusarvoisesti liittymän on **Ethernet**) voit siirtyä pilveen. Voit määrittää, millaisia liikenne, voit luoda useita VPN-liittymät virtual matriisin (määritetty iSCSI server) ja kyseisten rajapintojen yhdistäminen eri aliverkosta.

-   Kaistanleveyden-pilvi vain (käyttämä virtual matriisi), reitittimen tai palomuurin rajoitus määritetään. Jos määrität oman hypervisor rajoitusta, se rajoita kaikki protokollat, mukaan lukien iSCSI ja SMB cloud kaistanleveyden sijaan. 

-   Varmista, että ajan synkronoinnin hypervisors on otettu käyttöön. Jos virtual matriisi käyttämällä Hyper-V, valitse Hyper-V hallintaan, siirry **asetukset &gt; Integration Services**, ja varmista, että **ajan synkronointi** on valittuna.

### <a name="storage-accounts"></a>Tallennustilan tilit

StorSimple Virtual matriisi voi liittää yhteen tallennustilan tilillä. Tallennustilan tilin voi olla automaattisesti luotu tallennustilan-tiliä saman tilauksen palvelun tili tai tallennustilan tilin liittyvät toiseen tilaukseen. Lisätietoja on artikkelissa [virtual matriisin tilien tallennustilan](storsimple-ova-manage-storage-accounts.md)hallinta.

Käytä seuraavia suosituksia tallennustilan tilien liittyvät virtual matriisi.

-   Useita virtual matriisin yksittäisen tallennustilaa tilillä linkitettäessä ottaa huomioon suurin kapasiteetti (64 TT) virtual matriisi ja tallennustilaa tilin enimmäiskoko (500 TT). Tämä rajoittaa täysikokoinen virtual matriisien, jotka voivat liittyä tallennustilan tilin tietoja 7.

-   Kun uusi tallennustilan tilin luominen
    -   On suositeltavaa, että luot sen alueen lähimpänä kohtaa, johon StorSimple Virtual matriisin otetaan käyttöön Pienennä viiveitä remote office/haaran office.

    -   Vastaa huomioon, että et voi siirtää tallennustilan tilin eri alueiden. Myös ei voi siirtää palvelu-tilauksissa.

    -   Käytä tallennustilan-tiliä, joka sisältää redundancy palvelinkeskusten välillä. Virtuaalinen matriisin käytettäväksi GEO Redundant Storage (GRS) vyöhykkeen tarpeettomat Storage (ZRS) ja paikallisesti (LRS tarpeettomat tallennustilan) tuetaan. Lisätietoja tallennustilan tilit erityyppiset Siirry [Azure tallennustilan replikoinnin](../storage/storage-redundancy.md).


### <a name="shares-and-volumes"></a>Osakkeet ja asemat

StorSimple Virtual matriisin voit valmistella osakkeet, kun se on määritetty tiedostopalvelimeen ja asemat, kun määritetty iSCSI-palvelimeen. Osakkeet ja asemat luomisen parhaat käytännöt liittyvät kokoa ja määritetty tyyppi.

#### <a name="volumeshare-size"></a>Äänenvoimakkuuden/Jaa kokoa

Virtuaalinen matriisin voit valmistella osakkeet, kun se on määritetty tiedostopalvelimeen ja asemat, kun määritetty iSCSI-palvelimeen. Osakkeet ja asemat luomisen parhaat käytännöt liittyvät kokoa ja määritetty tyyppi. 

Ota huomioon seuraavien parhaiden käytäntöjen valmisteltaessa osakkeet tai virtual laitteen asemat.

-   Porrastettu Jaa valmistellun koon suhteessa tiedostokoon saattaa olla vaikutusta tiering suorituskykyä. Suurten tiedostojen käsitteleminen saattaa johtaa hidas taso. Kun käsittelet suurta tiedostoa, suosittelemme, että suurin tiedosto on pienempi kuin 3 % Jaa koosta.

-   16 asemat/osakkeet enintään voi luoda virtuaalisia matriisi. Jos paikallisesti kiinnitetty asemat/osakkeet voi välillä 50 Gigatavua 2 Teratavua. Jos tasoisen, asemat/osakkeet on oltava välillä 500 Gigatavua 20 Teratavua. 

-   Kun luot aseman tekijä odotettu tietojen kulutus sekä kasvavan tulevaisuudessa. Kun asema ei voi laajentaa myöhemmin, voit aina palauttaa suurempaan asemaan.

-   Kun äänenvoimakkuuden on luotu, ei voi pienentää äänenvoimakkuutta StorSimple kokoa.
   
-   Kun kirjoittamista Porrastettu asemaan StorSimple, kun aseman tiedot saavuttaa tietty määrä (suhteessa varattu äänenvoimakkuuden paikallisen välilyönti), IO on rajoitettu. Jatkuvat kirjoittamaan aseman hidastaa IO merkittävästi. Vaikka voit kirjoittaa Porrastettu merkkiin (olemme et aktiivisesti Pysäytä kirjoittaminen valmistellun kapasiteetin käyttäjältä) valmistellun kapasiteettia suuremmaksi, näyttöön tulee siitä, että olet oversubscribed ilmoitusviesti. Kun näkyviin tulee ilmoitus, on välttämätöntä, että Ota korjaavat toimenpiteet, kuten Poista aseman tiedot tai palauttaa aseman suurempaan asemaan (aseman laajentamista ei tällä hetkellä tueta).

-   Tietojen palauttaminen Käytä tapauksissa sallitun osakkeet/tietomääristä määrä on 16 ja osakkeet/asemat voi käsitellä rinnakkain enimmäismäärä on myös 16 osakkeet/tietomääristä määrä ei ole RPO ja RTOs vaikuttavat. 

#### <a name="volumeshare-type"></a>Äänenvoimakkuuden/Jaa-tyyppi

StorSimple tukee käyttö perusteella äänenvoimakkuutta tai Jaa kahdenlaisia: kiinnitetyt ja tasoisen paikallisesti. Paikallisesti kiinnitetty asemat/osakkeet thickly valmistelun yhteydessä, olisi Porrastettu asemat/osakkeet levinneet valmistelun yhteydessä. 

On suositeltavaa toteuttaa seuraavien parhaiden käytäntöjen StorSimple asemat/osakkeet määritettäessä:

-   Määritä asematyyppi toiminnoista, jotka aiot ottaa käyttöön, ennen kuin luot aseman perusteella. Käytä paikallisesti kiinnitetty tietomääristä toiminnoista, jotka vaativat paikallisten tietojen oikeudet (myös aikana cloud käyttökatkosta) ja, pieni cloud viiveet suurempia. Kun luot aseman virtual matriisi, et voi muuttaa asematyyppi-paikallisesti kiinnitetty, porrastettu tai *päinvastoin*. Esimerkkinä luominen paikallisesti kiinnitetty asemat, kun otat SQL työmääriä tai työmääriä isännöinnin näennäiskoneiden (VMs); Käytä Porrastettu tietomääristä tiedoston Jaa toiminnoista.

-   Valitse vaihtoehto vähemmän yleisimpien arkistointia tietojen käsittelemiseen suuriin tiedostokokoihin. Kopioinnin peruuttaminen lohkon suurentaminen 512 k käytetään, kun tämä asetus on käytössä suorittamiseen tiedonsiirto pilveen.

#### <a name="volume-format"></a>Äänenvoimakkuus-muoto

Kun olet luonut StorSimple tietomääristä iSCSI palvelimeen, joudut alusta, ota käyttöön ja muotoilla asemat. Tämä toiminto suoritetaan isännän StorSimple laitteen yhteydessä. Seuraavia tehokkaiksi todettuja toimintatapoja suositellaan, kun käyttöönottaminen ja muotoilun StorSimple isännän asemat.

-   Suorita kaikki StorSimple asemat nopeasti muoto.

-   StorSimple aseman muotoilussa Käytä kohdistus yksikön kokoa (Australia) 64 kilotavua (oletusarvo on 4 kt). 64 Kilotavua Australia perustuu testaus valmis itse yleisten StorSimple toiminnoista ja muut toiminnoista.

-   Kun määritetty iSCSI-palvelimeksi StorSimple Virtual matriisin käyttämisen Älä käytä Kootut asemat tai dynaamisiksi asemat tai levyjen ei tue StorSimple.

#### <a name="share-access"></a>Jako-oikeuksien

Luotaessa osakkeet virtual matriisin tiedostopalvelimessa noudattamalla seuraavia ohjeita:

-   Jaettuun luotaessa määrittää käyttäjäryhmä Jaa sijaan yksittäisen käyttäjän järjestelmänvalvojana.

-   Voit hallita NTFS-käyttöoikeudet muokkaamalla Resurssienhallinnan avulla osakkeet Jaa luomisen jälkeen.

#### <a name="volume-access"></a>Aseman

Määritettäessä iSCSI asemat-StorSimple Virtual matriisin kannattaa hallita tarvittaessa. Voit selvittää, mitä host-palvelimissa käyttää asemat, luominen ja liittäminen StorSimple tietomääristä Accessin ohjausobjektin tietueiden (ACRs).

Käytä seuraavia parhaita käytäntöjä, kun ACRs määrittäminen StorSimple asemat:

-   Liittää aina vähintään yksi ACR aseman.

-   Määritä useita ACRs liitetty ympäristössä.

-   Kun määrität useamman kuin yhden ACR levylle, varmista äänenvoimakkuuden on ei ole määritetty tavalla, jossa voivat samanaikaisesti käsitellä useita ei ole liitetty isäntä. Jos olet määrittänyt useita ACRs levylle, varoitussanoma tulee arvon asetusten tarkistaminen.

### <a name="data-security-and-encryption"></a>Tietojen suojaus ja salaus

StorSimple Virtual matriisin on tietoja suojaus ja salaus ominaisuuksia, jotka varmistavat luottamuksellisuus ja tietojen eheys. Kun käytät nämä ominaisuudet, on suositeltavaa noudattamalla näitä parhaita käytäntöjä: 

-   Määritä cloud tallennustilan-salausavaimen luomiseen AES 256 salaus, ennen kuin tiedot lähetetään virtual matriisin pilvipalveluun. Tämä avain ei tarvita, jos tiedot salataan alkavan. Avain Voit luoda ja säilytetään turvallinen käyttäminen avaimen hallintajärjestelmän, kuten [Azure avaimen säilö](../key-vault/key-vault-whatis.md).

-   Tallennustilan tilin lisäämiseen StorSimple hallintapalvelu määritettäessä Varmista, että laitteesi StorSimple ja pilveen väliseen viestintään verkon suojatun kanavan luominen SSL-tila otetaan käyttöön.

-   Luo tallennustilan asiakkaiden näppäimet (muodostamalla yhteyden Azure tallennuspalvelu) säännöllisesti muutoksia, voit käyttää-tiliin järjestelmänvalvojat muutetut kohdeluetteloon perustuvan.

-   Virtuaalinen taulukon tiedot on pakattu ja deduplicated ennen sen lähettämistä Azure. Ei ole suositeltavaa tietojen kopioinnin peruuttaminen rooli-palvelun käyttäminen Windows Server host.


## <a name="operational-best-practices"></a>Toiminnallisia parhaat käytännöt

Toiminnallisia parhaista käytännöistä on ohjeita, jossa asiat esitetään päivittäinen hallinta tai virtual matriisin toiminnon aikana. Näiden käytäntöjen kattavat tietyn hallintatehtäviä, kuten ottaen varmuuskopioiminen, palauttaminen varmuuskopiosta joukon suorittamiseen automaattisesti, hallita ja poistamalla matriisin valvonta järjestelmän käyttö- ja kuntotietojen ja käynnissä virus tarkistaa virtual matriisi.

### <a name="backups"></a>Varmuuskopioiden

Virtuaalinen taulukon tiedot varmuuskopioidaan pilveen kahdella tavalla, oletusarvoisesti automaattisen varmuuskopioinnin koko laitteen käynnistäminen 22:30 tai Manuaalinen tarvittaessa varmuuskopion kautta. Oletusarvon mukaan laitteen luo automaattisesti päivittäinen cloud tilannevedoksia kaikki tiedot, jotka asuvat sitä. Lisätietoja Siirry [takaisin StorSimple Virtual matriisin](storsimple-ova-backup.md).

Korkojakso ja niiden oletusarvoisen varmuuskopioiden liittyvät säilytys ei voi muuttaa, mutta voit määrittää aika, jolloin päivittäin varmuuskopioita aloitettu päivittäin. Automaattisen varmuuskopioinnin aloitusaika määritettäessä on suositeltavaa, että:

-   Ajoita varmuuskopiot myöhemmin. Varmuuskopion alkamisaika olisi ei ole sama kuin monia host IO.

-   Aloittaa manuaalinen tarvittaessa varmuuskopion, kun suunnittelet laitteen automaattisesti tai suojata virtual taulukon tiedot edellisen ylläpito-ikkuna.

### <a name="restore"></a>Palauttaminen

Voit palauttaa varmuuskopiosta määrittäminen kahdella tavalla: Palauta toiseen äänenvoimakkuutta tai Jaa tai Kohdetason palauttamisen (käytettävissä vain virtual taulukko, joka on määritetty tiedostopalvelimeen). Kohdetason palautus avulla voit tehdä hajautetun palautus tiedostojen ja kansioiden StorSimple laitteeseen osakkeet cloud varmuuskopion. Jos haluat lisätietoja, siirry [palauttaminen varmuuskopiosta](storsimple-ova-restore.md).

Palautuksen suoritettaessa Ota huomioon seuraavat ehdot:

-   StorSimple Virtual matriisin ei tue käytönaikainen palauttaminen. Voi kuitenkin helposti saavuttaa kahdessa vaiheessa: tilaa virtuaalisen-matriisi ja palauttaa sitten toinen aseman/Jaa.

-   Kun palauttaminen paikalliseen asemaan, ota huomioon palautus on pitkä käynnissä toiminto. Vaikka äänenvoimakkuuden nopeasti tulee online-tilaan, tiedot säilyy hydratoitu taustalla.

-   Avauksen ja vaihdon tyyppi säilyy ennallaan palauttaminen aikana. Porrastettu aseman palautetaan toiseen Porrastettu asemaan ja paikallisesti kiinnitetty aseman toiseen paikallisesti kiinnitetyt äänenvoimakkuutta.

-   Yritettäessä aseman tai jaettuun palauttaminen varmuuskopiosta Määritä palautustyön epäonnistuu, jos kohde äänenvoimakkuutta tai Jaa edelleen voi luoda portaalissa. On tärkeää, että voit poistaa käyttämättömiä kohde aseman tai voit jakaa portaalissa Pienennä tulevien ongelmat johtuvat tämä elementti.

### <a name="failover-and-disaster-recovery"></a>Automaattisesti ja tietojen palauttaminen

Voit siirtää tietoja *lähde* -laitteesta joten *kohde* laitetta, joka sijaitsee samassa tai eri maantieteellisen sijainnin laitteen automaattisesti. Laitteen vikasietotilaa on koko laite. Aikana automaattisesti lähde-laitteen cloud-tiedot muuttuvat omistajuus, Kohdelaite.

Oman StorSimple Virtual matriisin voit voi vain epäonnistua kautta toiseen saman StorSimple hallintapalvelu hallitsee virtual matriisi. Automaattisesti 8000 sarja-laitteeseen tai matriisin hallitsee eri StorSimple hallinta-palvelun (kuin lähde-laitteen näkymän) ei sallita. Lisätietoja automaattisesti huomioon otettavia seikkoja, siirry [laitteen vikasietotilaa edellytyksistä](storsimple-ova-failover-dr.md).

Kun suoritat epäonnistumisen virtual matriisin kautta, ota huomioon seuraavat:

-   Se on suunniteltu automaattisesti suositellut parhaiden käytäntöjen tulevat kaikki asemat/osakkeet offline-tilassa ennen aloittamista vikasietotilaa. Jos haluat ottaa asemat/osakkeet offline-tilassa isännän ensimmäinen ja sitten offline-tilaan ne virtual laitteen ohjeiden käyttöjärjestelmäkohtaisia.

-   Tiedoston palvelimen palauttaminen (DR) Suosittelemme Liitä kohdelaitteen samaa toimialuetta lähteenä niin, että Jaa käyttöoikeudet ovat automaattisesti ratkennut. Tässä versiossa tuetaan vain keskeneräiset samaa toimialuetta kohde laitteeseen.

-   Kun DR on suoritettu onnistuneesti, Lähdelaite poistetaan automaattisesti. Laite ei ole enää käytettävissä, mutta virtuaalikoneen, valmisteltu Host (isäntä)-järjestelmässä on edelleen käyttö resurssit. On suositeltavaa, että poistat virtual tämän tietokoneen host järjestelmästä, jotta kulut eivät kerry.

-   Huomaa, että, vaikka vikasietotilaa ei onnistu, **tiedot ovat aina turvallisten pilveen**. Ota huomioon seuraavat kolme esimerkkitapausta, jossa vikasietotilaa ei onnistunut:

    -   Virhe alkuvaiheessa keskeneräiset esimerkiksi kun DR vanhat tarkistukset suoritetaan. Tässä tilanteessa laitteesi kohde on yhä käytettävissä. Voit yrittää uudelleen samaan kohde laitteeseen vikasietotilaa.

    -   Virhe todellinen automaattisesti aikana. Tässä tapauksessa Kohdelaite on merkitty ei käytettävissä. Valmistelu ja määrittää toisen virtual kohdetaulukon ja, joka käyttää automaattisesti.

    -   Vikasietotilaa oli valmis minkä Lähdelaite on poistettu mutta kohde laitteessa on ongelmia ja mitään tietoja ei voi käyttää. Tiedot on edelleen turvallisten pilvipalvelussa, ja voit helposti hakea toisen virtual taulukon luomalla ja käyttämällä sen kohde-laitetta varten DR.

### <a name="deactivate"></a>Aktivoinnin poistaminen

Kun olet poistanut StorSimple Virtual matriisi, palvelimen laite ja vastaavan StorSimple hallintapalvelu välinen yhteys. Käytöstä poistamisen on **pysyvä** toiminto, eikä niitä voi perua. Käytöstä poistetun laitteen StorSimple hallinta-palvelussa ei voi rekisteröidä uudelleen. Jos haluat lisätietoja, siirry [aktivointi ja poista StorSimple Virtual matriisin](storsimple-deactivate-and-delete-device.md).

Seuraavien parhaiden käytäntöjen pitää mielessä, kun virtual matriisin käytöstä:

-   Ota cloud tilannevedos ennen käytöstä virtual laitteen tiedot. Kun olet poistanut virtual matriisi, kaikki paikallisen laitteen tiedot menetetään. Cloud tilannevedoksen ottaen avulla voit palauttaa tietoja myöhemmässä vaiheessa.

-   Ennen kuin olet poistanut StorSimple Virtual matriisi, varmista, että voit lopettaa tai poistaa asiakkaat ja isännöi, jotka riippuvat laitteen.

-   Poista aktivointi on poistettu laitteesta, jos käytät enää niin, että se ei kertymä kulut.

### <a name="monitoring"></a>Seuranta

Varmista, että StorSimple Virtual matriisin on jatkuva kunnossa-tilaan, sinun on matriisin ja varmistaa, että tiedot saavat myös ilmoitukset-järjestelmästä. Yleistä tarkkailla virtual matriisin, Toteuta seuraavien parhaiden käytäntöjen:

- Määritä seuranta voit seurata levyn käyttö virtual taulukon tietojen levytilaa sekä OS levy. Jos käynnissä Hyper-V, voit valvoa virtualization isännät yhdistelmä System Center Virtual Machine Manager (SCVMM) ja System Center Operations Manager (SCOM).   

- Määritä sähköposti-ilmoitukset lähetetään ilmoitusten käyttö tiettyjen tasoilla virtual matriisin.                                                                                                                                                                                                

### <a name="index-search-and-virus-scan-applications"></a>Indeksihaku- ja virussuojaus skannata sovellukset

Paikallinen ylätasolla tietoja Microsoft Azure pilveen voit automaattisesti taso StorSimple Virtual matriisi. Kun jokin sovellus, kuten indeksi-haun tai virustarkistus käytetään tarkistamaan StorSimple tallennettuja tietoja, sinun täytyy huolehtia, että cloud tiedot ei käsitellä ja otettu mukaan, paikallinen ylätasolla.

Suosittelemme, että seuraavien parhaiden käytäntöjen Toteuta määritettäessä indeksi haun tai virus tarkistuksen virtual matriisi:

-   Poista käytöstä automaattisesti määritetyn Perusteellisessa toiminnot.

-   Porrastettu levyasemien indeksi haun tai virus tarkistus-sovelluksen kokoonpanon määrittäminen tekemään vaiheittainen tarkistuksen. Tarkistaa vain uudet tiedot todennäköisesti asuviin paikallinen ylätasolla. Tiedot, jotka pilveen tasoisen ei käytä vaiheittaisen toiminnon aikana.

-   Varmistaa oikean hakusuodattimet ja asetukset on määritetty siten, että vain tarkoitettu tiedostotyypit, Hae skannattu. Esimerkiksi kuva (JPEG, GIF ja TIFF) ja tekniikka-piirustuksia ei tarkistettavan aikana lisäävän tai koko indeksi Rakenna uudelleen.

Jos käytät Windows indeksoinnin prosessi, noudata seuraavia ohjeita:

-   Älä käytä Windows indeksointitoiminto Porrastettu asemat, kun se ikinä suuria tietomääriä (TBs) pilveen, jos indeksi muodostetaan uudelleen usein. Indeksi muodostetaan uudelleen noutaa kaikki tiedostotyypit indeksoida niiden sisältöä.

-   Käytä Windowsin indeksoinnin paikallisesti kiinnitetty tietomääristä prosessi, kun tämä vain käyttämään tietoja paikallisen tasoa, voit luoda hakemiston (cloud tiedot ei onnistu).

### <a name="byte-range-locking"></a>DBCS-alueen lukitseminen

Sovellusten lukita tavua tietyn alueen sisällä tiedostoja. Jos tavu alueen lukitus on otettu käyttöön sovellukset, jotka ovat kirjoittaminen oman StorSimple, valitse tiering eivät toimi virtual matriisi. Toimimaan tiering, kaikki alueet käyttää tiedostojen tulisi olla lukitus. DBCS-alueen lukitseminen ei tueta Porrastettu asemat virtual matriisi.

Suositeltavat toimenpiteet lievittämään tavu alueen lukitseminen ovat:

-   DBCS-alueen sovelluksen logiikkaa lukitseminen käytöstä.

-   Käytä paikallisesti kiinnitetyt asemat (sijaan tasoisen) varten tämän sovelluksen liittyviä tietoja. Paikallisesti kiinnitetty tietomääristä ei taso pilveen kyselyjä.

-   Kun tietomääristä paikallisesti käyttämällä kiinnitetyt tavu alueen lukitus käytössä, äänenvoimakkuuden voi tulla online, ennen kuin palautus on valmis. Tällöin sinun on odotettava palautus on valmis.

## <a name="multiple-arrays"></a>Useita taulukoita

Useita virtual matriisin täytyy mahdollisesti käyttöön kasvava Työsarja tietoja, joita saattaa spill sivulle vaikuttavia näin laitteen suorituskykyä cloud-tiliin. Tällöin on parasta asteikko laitteiden kun Työsarja kasvaa. Tämä edellyttää vähintään yksi laitteiden paikallisen tietokeskuksen lisätään. Kun lisäät laitteita, voit tehdä seuraavaa:

-   Jakaa nykyisen tietojoukolle.
-   Ottaa käyttöön uuden työmääriä uusi laite.
-   Jos käyttöönotto virtual useita taulukoita, on suositeltavaa, että-kuormituksen tasaamisen kannalta jakaa eri hypervisor isännät taulukko.

-  Useita virtual matriisien (kun määritetty tiedostopalvelimeen tai iSCSI palvelimelle) voidaan ottaa käyttöön jaettu tiedosto järjestelmä-Namespace. Yksityiskohtaisia ohjeita Siirry [Jaettu tiedosto järjestelmän Namespace-ratkaisussa, jossa Hybrid Cloud tallennustilan käyttöönotto-oppaassa](https://www.microsoft.com/download/details.aspx?id=45507). Hajautetun tiedostojärjestelmän replikointi tällä hetkellä ei suositella käytettäväksi virtual matriisi. 


## <a name="see-also"></a>Katso myös
Lisätietoja [hallinta StorSimple Virtual matriisin](storsimple-ova-manager-service-administration.md) StorSimple hallintapalvelu kautta.
