<properties
   pageTitle="StorSimple Virtual matriisin järjestelmävaatimukset"
   description="Lisätietoja ohjelmiston ja StorSimple Virtual matriisin verkko vaatimukset"
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/17/2016"
   ms.author="alkohli"/>

# <a name="storsimple-virtual-array-system-requirements"></a>StorSimple Virtual matriisin järjestelmävaatimukset

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa kuvataan tärkeitä järjestelmävaatimukset Microsoft Azure StorSimple Virtual matriisin (tunnetaan myös nimellä StorSimple paikallisen virtual tai StorSimple virtual laite) ja tallennustilaa-asiakkaiden käyttäminen matriisin. On suositeltavaa, että luet tiedot huolellisesti ennen käyttöönoton StorSimple järjestelmän ja sitten takaisin siihen viitataan tarvittaessa käyttöönotto ja myöhempiä toiminnon aikana.

Järjestelmävaatimukset ovat seuraavat:

-   **Tallennustilan asiakkaiden ohjelmistovaatimukset** - kuvaa tuetut virtualisointi-ympäristöissä, selaimet, iSCSI laitteistokäynnistäjät, SMB asiakkaat, pienin virtual laitteen vaatimukset ja näiden käyttöjärjestelmien lisävaatimukset.

-   **Vaatimukset StorSimple laitteen verkko** -, joka on oltava käynnissä sallimaan iSCSI, cloud tai liikenteen hallinta palomuurin porttien tietoja.

Tämän artikkelin julkaistu StorSimple järjestelmävaatimukset tiedot koskee vain StorSimple Virtual matriiseja.

- Siirry 8000 sarjan laitteiden [StorSimple 8000 sarjan laitteen järjestelmävaatimukset](storsimple-system-requirements.md).
 
- Siirry 7000 sarjan laitteiden [StorSimple 5000 7000 sarjan laitteen järjestelmävaatimukset](http://onlinehelp.storsimple.com/1_StorSimple_System_Requirements).


## <a name="software-requirements"></a>Ohjelmistovaatimukset

Ohjelmistovaatimukset sisältää tuetut selaimet, SMB-versiot, virtualization alustojen ja pienin virtual laitteen vaatimukset tiedot.

### <a name="supported-virtualization-platforms"></a>Tuetut virtualization ympäristöissä


| **Hypervisor** | **Versio**                          |
|----------------|--------------------------------------|
| Hyper-V        | Windows Server 2008 R2 SP1 ja sitä uudemmissa versioissa |
| VMware ESXi    | 5,5 ja uudempi versio                        |

### <a name="virtual-device-requirements"></a>Virtuaalinen laitteen vaatimukset

| **Osa**                                | **Vaatimus**            |
|----------------------------------------------|----------------------------|
| Vähimmäismäärä virtual suorittimen (sydämiä) | 4                          |
| Pienin RAM-muistia                         | 8 GIGATAVUN                       |
| Levyn tila<sup>1</sup>                       | OS levy - 80 gt <br></br>Tietoja levy - 8 TT 500 Gigatavua|
| Verkko-liittymän vähimmäismäärä       | 1                          |
| Pienin Internet-kaistanleveyden<sup>2</sup>       | 5 Mbps                     |

<sup>1</sup> - thin valmistelun yhteydessä

<sup>2</sup> - verkon vähimmäisvaatimukset voivat vaihdella päivittäinen tietojen muuttaminen kurssi mukaan. Esimerkiksi jos laite on varmuuskopioida 10 Gigatavun tai lisämuutoksia päivän aikana, sitten varmuuskopioinnin 5 Mbps-yhteyden kautta voi kestää jopa 4,25 tuntia (Jos tietoja ei voi pakata tai deduplicated).

### <a name="supported-web-browsers"></a>Tuetut selaimet

| **Osa**     | **Versio** | **Muut vaatimukset ja huomautukset** |
|-------------------|-----------------|-----------------------------------|
| Microsoft Edge    | Uusin versio  |                                   |
| Internet Explorerissa | Uusin versio  | Internet Explorer 11 testattu  |
| Google Chrome     | Uusin versio  | Chromessa 46 testattu             |

### <a name="supported-storage-clients"></a>Tuetut tallennustilan asiakkaat 

Seuraavat ohjelmistovaatimukset koskevat iSCSI laitteistokäynnistäjät, joka käyttää StorSimple Virtual matriisin (määritetty iSCSI-palvelin).

| **Tuetut käyttöjärjestelmät** | **Vaadittava versio** | **Muut vaatimukset ja huomautukset** |
| --------------------------- | ---------------- | ------------- |
| Windows Server              | 2008R2 SP1, 2012 2012R2 |StorSimple voit luoda levinneet valmisteltu ja täysin valmistellun asemat. Se ei voi luoda osittain valmistellun asemat. StorSimple iSCSI tietomääristä tuetaan vain: <ul><li>Yksinkertainen Windows basic levyjen asemat.</li><li>Windowsin NTFS aseman muotoiluun.</li>|


Seuraavat ohjelmistovaatimukset ovat SMB-asiakkaille, jotka käyttävät StorSimple Virtual matriisin (määritetty tiedostopalvelimeen).

| **SMB-versio** |
|-------------|
| SMB 2.x     |
| SMB 3.0     |
| SMB 3.02    |

> [AZURE.IMPORTANT] Älä kopioi tai tallentaa tiedostoja, jotka on suojattu StorSimple Virtual matriisi; tiedostopalvelimeen mukaan Windows salaaminen File System (EFS) Tämä aiheuttaa ei tueta määrityksessä. 

## <a name="networking-requirements"></a>Verkko-vaatimukset 

Seuraavassa taulukossa on lueteltu portit, jotka on avattava palomuurin siten, että iSCSI, SMB, cloud tai liikenteen hallinta. Tässä taulukossa *-* tai *Saapuvan* viittaa siihen suuntaan, josta asiakkaan pyynnöt käyttää laitteen. *Ulos* tai *Lähtevät* viittaa siihen suuntaan, jossa StorSimple laitteen lähettää tietoja ulkoisesti-käyttöönoton jälkeen: esimerkiksi lähtevän Internetiin.

| **Portti nro<sup>1</sup>** | **Sisään tai ulos** | **Portti laajuus** | **Pakollinen**              | **Huomautuksia**                                                                                                            |
|--------------------------|---------------|----------------|---------------------------|----------------------------------------------------------------------------------------------------------------------|
| TCP 80 (HTTP)            | Ulos           | WAN            | Ei                        | Lähtevän portin käytetään Internet-yhteys Nouda päivitykset. <br></br>Lähtevän web-välityspalvelimen on määritettävä käyttäjä. |
| TCP 443 (HTTPS)          | Ulos           | WAN            | Kyllä                       | Lähtevän portin käytetään tietojen pilveen käyttäminen. <br></br>Lähtevän web-välityspalvelimen on määritettävä käyttäjä. |
| UDP 53 (DNS)             | Ulos           | WAN            | Joissakin tapauksissa Katso huomautuksia. | Tämä portti on pakollinen vain, jos käytössäsi on Internet-pohjaisia DNS-palvelimeen. <br></br> **Huomautus**: Jos otat tiedostopalvelimessa, on suositeltavaa käyttää paikalliset DNS-palvelimeen.|
| UDP 123 (NTP)            | Ulos           | WAN            | Joissakin tapauksissa Katso huomautuksia. | Tämä portti on pakollinen vain, jos käytössäsi on Internet-pohjaisten NTP-palvelimeen.<br></br> **Huomautus:** Jos otat tiedostopalvelimessa, suosittelemme ajan synkronointi Active Directory-toimialueen ohjaimet kanssa.  |
| TCP 80 (HTTP)           | Valitse            | LÄHIVERKON            | Kyllä                       | Tämä on paikallinen Käyttöliittymän StorSimple laitteeseen paikallisen hallinnan saapuvien portti. <br></br> **Huomautus**: paikallisen Käyttöliittymän käyttäminen HTTP ohjaa automaattisesti HTTPS.|
| TCP 443 (HTTPS)          | Valitse            | LÄHIVERKON            | Kyllä                       | Tämä on paikallinen Käyttöliittymän StorSimple laitteeseen paikallisen hallinnan saapuvien portti.|
| TCP-3260 (iSCSI)         | Valitse            | LÄHIVERKON            | Ei                        | Tämä portti käytetään käyttämään tietoja iSCSI päälle.|

<sup>1</sup> ei ole saapuvien porttien on voi avata julkisen Internetissä.

> [AZURE.IMPORTANT] Varmista, että palomuurin ei muokata tai purkaa SSL liikenteen StorSimple laitteen ja Azure välillä.


### <a name="url-patterns-for-firewall-rules"></a>URL-kuvioiden määrittäminen palomuurisäännöt 

Verkoston järjestelmänvalvojat voivat määrittää usein Lisäasetukset palomuurisäännöt URL-osoite-kuvioiden avulla ja suodattaa saapuvan ja lähtevän tietoliikenteen perusteella. Virtuaalinen matriisi ja StorSimple hallintapalvelu määräytyvät muiden Microsoft-sovellusten, kuten Azure palvelun Bus, Azure Active Directory käyttöoikeuksien valvonta, tallennustilan tilit ja Microsoft Update-palvelimiin. Näiden sovellusten URL-Osoitteen kuviot voidaan määrittää palomuurisäännöt. On tärkeää ymmärtää, että nämä sovellukset liittyvä URL-Osoitteen kuviot voi muuttaa. Tämä puolestaan edellyttää verkon järjestelmänvalvoja voi valvoa ja Päivitä palomuurisäännöt kuin oman StorSimple ja tarvittaessa. 

On suositeltavaa, että määrität palomuurisäännöt IP-osoitteet, kiinteä liberally useimmissa tapauksissa StorSimple lähtevän tietoliikenteen. Voit kuitenkin määrittää Lisäasetukset palomuurisäännöt, joita tarvitaan Luo suojatun ympäristöissä alla olevia tietoja.

> [AZURE.NOTE] 
> 
> - Laitteen (lähde) IP-osoitteet aina arvoksi cloud käyttävä Verkkoliittymät. 
> - Kohde IP-osoitteet on asetettava [Azure palvelinkeskuksen IP-alueita](https://www.microsoft.com/en-us/download/confirmation.aspx?id=41653).


| URL-kaava                                                      | Osan/toiminnot                                           |
|------------------------------------------------------------------|---------------------------------------------------------------|
| `https://*.storsimple.windowsazure.com/*`<br>`https://*.accesscontrol.windows.net/*`<br>`https://*.servicebus.windows.net/*`   | StorSimple hallinta<br>Access Control-palvelu<br>Azure palvelun Bus|
|`http://*.backup.windowsazure.com`|Laitteen rekisteröinti|
|`http://crl.microsoft.com/pki/*`<br>`http://www.microsoft.com/pki/*`|Sertifikaattien kumoaminen |
| `https://*.core.windows.net/*`<br>`https://*.data.microsoft.com`<br>`http://*.msftncsi.com` | Azure-tallennustilan asiakkaat ja seuranta |
| `http://*.windowsupdate.microsoft.com`<br>`https://*.windowsupdate.microsoft.com`<br>`http://*.update.microsoft.com`<br> `https://*.update.microsoft.com`<br>`http://*.windowsupdate.com`<br>`http://download.microsoft.com`<br>`http://wustat.windows.com`<br>`http://ntservicepack.microsoft.com`| Microsoft Update-palvelimet<br>                        |
| `http://*.deploy.akamaitechnologies.com`                         |Akamai CDN |
| `https://*.partners.extranet.microsoft.com/*`                    | Tuki-paketti                                                  |
| `http://*.data.microsoft.com `                   | Telemetriatietojen-palvelusta on Windows-kohdassa [asiakkaan kokemukset ja diagnostiikan telemetriatietojen päivitys](https://support.microsoft.com/en-us/kb/3068708)                                                  |

## <a name="next-step"></a>Seuraava vaihe

-   [Ottaa käyttöön StorSimple Virtual matriisin portaalin valmisteleminen](storsimple-ova-deploy1-portal-prep.md)
