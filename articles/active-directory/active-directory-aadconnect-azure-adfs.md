<properties
    pageTitle="Azure Active Directory Federation Services-palvelujen | Microsoft Azure"
    description="Tässä asiakirjassa kerrotaan AD FS Azure-tietokannassa ottamisesta hyvin laiduntamismahdollisuuksien."
    keywords="käyttöönotto-azure AD FS, käyttöönotto azure adfs, azure adfs, azure ad fs, ADFS: n käyttöön, käyttöön ad fs adfs Azure-tietokannassa, käyttöönotto adfs Azure-tietokannassa, adfs azure AD FS azure, adfs azure AD FS, Azure AD FS Azure-tietokannassa, iaas, ADFS-johdanto käyttöönotto siirtäminen"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/03/2016"
    ms.author="anandy;billmath"/>

# <a name="ad-fs-deployment-in-azure"></a>Azure AD FS käyttöönottoa 

AD FS on yksinkertaistettu, suojattuun tunnistetietojen yhdistämisessä ja Web kertakirjautuminen (SSO) ominaisuuksia. N Azure AD yhdistämisessä tai O365 avulla käyttäjä voi todentaa paikallisen tunnistetiedoilla ja käyttää cloud kaikki resurssit. Tuloksena on tärkeää on erittäin käytettävissä AD FS-infrastruktuurin paikallisen sekä resurssien käytön varmistamiseksi ja pilveen. Käyttöönotto-Azure AD FS auttaa pyytää mahdollisimman vähän tehokkuutta suuren käytettävyyden saavuttamiseksi.
On monia etuja otat käyttöön AD FS Azure-tietokannassa, muutaman ne on lueteltu alla:

* **Suuri käytettävyys** - Azure käytettävyys joukot power voit varmistaa käytettävyyden infrastruktuurin.
* **Helppokäyttöinen näytöstä** – on suorituskykyä? Siirtää helposti tehokkaampia koneet avulla vain muutamalla hiiren napsautuksella Azure-tietokannassa
* **Usean Geo Redundancy** – kanssa Azure Geo Redundancy voit voidaan varmistaa, että maapallon infrastruktuurin on erittäin käytettävissä
* **Helppokäyttöinen hallinta** – Azure-portaalissa pelkistetty hallinta-asetusten hallinta infrastruktuurin on hyvin helppoa ja helppokäyttöinen 

## <a name="design-principles"></a>Rakenne-periaatteet

![Käyttöönotto-rakenne](./media/active-directory-aadconnect-azure-adfs/deployment.png)

Yllä olevassa kaaviossa näkyy suositellut basic topologian Aloita käyttöönotto Azure AD FS-infrastruktuuria. Periaatteiden topologian eri osien takana on lueteltu alla:

* **Ohjauskoneen / ADFS-palvelimien**: Jos sinulla on vähemmän kuin 1 000 käyttäjää voit asentaa AD FS-roolin riittää, että toimialue-ohjaimet. Jos et halua, että kaikki toimialueen ohjauskoneen vaikutus suorituskykyyn tai jos ympäristössä on yli 1 000 käyttäjää, ota käyttöön AD FS eri palvelimiin.
* **Vaihda Server** – on tarpeen ottamaan Web Application välityspalvelimet, niin, että käyttäjät voivat pääse AD FS, kun ne eivät ole yrityksen verkossa myös.
* **DMZ**: Web-sovelluksen välityspalvelimen sijoitetaan DMZ ja vain TCP/443 käyttö on sallittu DMZ ja sisäinen aliverkon välillä.
* **Lataa tasoitusmääritykset**: Jotta suuren käytettävyyden AD FS- ja Web-sovelluksen välityspalvelimen palvelinten, suosittelemme, että käytät sisäinen kuormituksen AD FS palvelinten ja Azure ladata tasaustoiminto Web-sovelluksen välityspalvelimia varten.
* **Käytettävyys joukot**: tarjoaa luotettavuutta AD FS-käyttöönoton, on suositeltavaa ryhmittelet kahden tai useamman näennäiskoneiden-käytettävyys määrittäminen samalla toiminnoista. Tässä määrityksessä varmistaa, että aikana joko suunniteltu tai suunnittelematon ylläpitotapahtuman vähintään yksi virtual machine ovat käytettävissä
* **Tallennustilan tilejä**: on suositeltavaa on kaksi tallennustilan tilejä. Ilman yksittäisen tallennustilaa tiliä voi aiheuttaa virheen yksittäisen kohdan luominen ja heikentää käyttöönoton eivät ole käytettävissä, epätodennäköistä tilanteessa, jossa tallennustilan tilin siirtyy alaspäin. Kahden tallennustilan tilin avulla liittää tallennustilan tilien vika kullekin riville.
* **Verkon eriytymistä**: Web-sovelluksen välityspalvelimet erillisen DMZ-verkossa käyttöön. Voit jakaa kahden aliverkon yhdessä virtual verkossa ja käyttöönotto erillään aliverkon Web-sovelluksen välityspalvelimen palvelimiin. Voit vain verkon suojausasetusten kunkin aliverkon määrittäminen ja Salli vain tarvittavat viestintä kahden aliverkon välillä. Lisätietoja annetaan käyttöönottotapa alla kohden

##<a name="steps-to-deploy-ad-fs-in-azure"></a>Käyttöönotto-Azure AD FS vaiheet

Tässä osassa vaiheissa kuvataan oppaan, käyttöönotto edellisessä AD FS-infrastruktuuria Azure alapuolella.

### <a name="1-deploying-the-network"></a>1. verkon käyttöönotto

Edellä kuvatulla voit voit joko luoda kahden aliverkon yhdessä virtual verkossa tai muuten luominen kahden eri virtual verkon (VNet). Tässä artikkelissa keskittyä käyttöönotto yhden virtual verkoston ja jakaa kahden aliverkon. Tämä on tällä hetkellä helpompaa toimintatavan kuin kaksi erillistä VNets edellyttäisi VNet VNet yhdyskäytävään yhteyksiä varten.

**1.1 virtual verkoston luominen**

![Luo VPN](./media/active-directory-aadconnect-azure-adfs/deploynetwork1.png)
    
Azure-portaalissa Valitse virtual verkkoon, ja voit ottaa käyttöön virtual verkko- ja yksi aliverkon heti yhdellä napsautuksella. INT aliverkon määritetään myös ja on nyt valmis VMs lisätään.
Seuraavaksi voit lisätä toisen aliverkon verkkoon, eli DMZ aliverkon. Voit luoda DMZ-aliverkon yksinkertaisesti

* Valitse juuri luomasi verkko
* Valitse ominaisuuksien aliverkon
* Aliverkon Ohjauspaneelin Napsauta Lisää-painiketta
* Anna aliverkon nimi ja osoite tilaa tiedot aliverkon luominen

![Aliverkon](./media/active-directory-aadconnect-azure-adfs/deploynetwork2.png)


![Aliverkon DMZ](./media/active-directory-aadconnect-azure-adfs/deploynetwork3.png)

**1.2. luominen verkossa käyttöoikeusryhmät**

Verkon käyttöoikeusryhmän (NSG) sisältää luettelon Käyttöoikeusluettelon (Access Control) säännöt, jotka Salli tai estä verkkoliikennettä AM-esiintymissä Virtual verkossa. NSGs voi olla aliverkosta tai yksittäiset AM esiintymät, aliverkon sisällä. Jos aliverkon liitetään NSG, kaikki kyseisen aliverkon AM esiintymät koskevat Käyttöoikeusluettelon säännöt.
Nämä ohjeet varten luodaan kahden NSGs: yksi sisäisen verkon ja DMZ. Ne lukee NSG_INT ja NSG_DMZ tarpeen mukaan.

![Luo NSG](./media/active-directory-aadconnect-azure-adfs/creatensg1.png)

Kun NSG on luotu, ole 0 saapuvan ja 0 lähtevän liikenteen säännöt. Kun roolit palvelimeen on asennettu ja toimintojen, saapuvan ja lähtevän liikenteen säännöt voidaan haluamasi suojaustason mukaan.

![NSG alusta](./media/active-directory-aadconnect-azure-adfs/nsgint1.png)

Kun NSGs on luotu, liittää NSG_INT aliverkon INT- ja NSG_DMZ aliverkon DMZ kanssa. Esimerkki näyttökuva on alla:

![NSG määrittäminen](./media/active-directory-aadconnect-azure-adfs/nsgconfigure1.png)

* Valitse aliverkosta Avaa aliverkosta-paneeli
* Valitse liitettävä NSG aliverkon 

Kun määritys-aliverkosta paneelin pitäisi näyttää kuten alla:

![Aliverkosta NSG jälkeen](./media/active-directory-aadconnect-azure-adfs/nsgconfigure2.png)

**1.3. Luo yhteys paikalliseen**

Tarvitsemme paikallisen yhteyden käyttöönotossa toimialueen ohjauskoneen Azure-tietokannassa. Azure on eri paikallisen infrastruktuurin muodostaa Azure infrastruktuurin connectivity vaihtoehtoja.

* Pisteen sivuston
* VPN sivusto sivusto
* ExpressRoute

On suositeltavaa käyttää ExpressRoute. ExpressRoute voit luoda yksityinen Azure palvelinkeskusten ja infrastruktuurin, joka on paikallisesti tai rinnakkain ympäristössä väliset yhteydet. ExpressRoute yhteydet ei siirry julkisen Internetin välityksellä. Lisää luotettavasti, nopeampi nopeuksia, pienet viiveet suurempia ja suurempi kuin tavallinen yhteydet suojauksen tarjoavat Internetin välityksellä.
Kun kannattaa käyttää ExpressRoute, voit valita minkä tahansa organisaatiolle parhaiten soveltuu yhteystapa. Saat lisätietoja ExpressRoute ja ExpressRoute käyttämällä eri connectivity vaihtoehdoista, lukemalla [ExpressRoute teknisiä tietoja](https://aka.ms/Azure/ExpressRoute).

### <a name="2-create-storage-accounts"></a>2. tallennustilan tilien luominen

Säilyttää suuren käytettävyyden ja välttää riippuvuus yksittäisen tallennustilan tilin, jotta voit luoda kaksi tallennustilan tilit. Käytettävyys kussakin joukossa koneet jakautuvan kaksi ryhmää ja määritä sitten kunkin ryhmän erillisessä tallennustilan tilin.

![Tallennustilan tilien luominen](./media/active-directory-aadconnect-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3. käytettävyys joukkojen luominen

Kunkin roolin (toimialueen Ohjauskoneen ja AD FS ja WAP) luo käytettävyys joukot, joka sisältää 2 koneet kunkin vähintään. Tämä auttaa suurempi käytettävyys kunkin roolin saavuttamiseksi. Kun luominen käytettävyys asettaa, on tärkeää päättää seuraavasti:
* **Virhe-toimialueet**: näennäiskoneiden saman vika toimialueen jakaminen samaan power lähde- ja fyysinen verkko valitsin. Vähintään 2 vika toimialueiden on suositeltavaa. Oletusarvo on 3 ja voit jättää se on tämä käyttöönottoa varten
* **Päivitä toimialueen**: kuuluvat saman Päivitä toimialueen koneet käynnistyvät yhdessä päivityksen aikana. Haluat vähintään 2 Päivitä toimialueen. Oletusarvo on 5 ja voit jättää se on tämä käyttöönottoa varten

![Käytettävyys joukot](./media/active-directory-aadconnect-azure-adfs/availabilityset1.png)

Seuraavat käytettävyys joukkojen luominen

| Käytettävyys määrittäminen | Rooli | Vian toimialueet | Päivitä toimialueet |
|:----------------:|:----:|:-----------:|:-----------|
| contosodcset | TOIMIALUEEN OHJAUSKONEEN/ADFS | 3 | 5 |
| contosowapset | VAIHDA | 3 | 5 |

### <a name="4--deploy-virtual-machines"></a>4. näennäiskoneiden käyttöönotto
Seuraavaksi näennäiskoneiden, joka isännöi infrastruktuurin eri roolien otetaan käyttöön. Vähintään kaksi koneet, on suositeltavaa käytettävyys kussakin joukossa. Luo six näennäiskoneiden basic käyttöönottoa varten.

| Tietokoneen | Rooli | Aliverkon | Käytettävyys määrittäminen | Tallennustilan tilin | IP-osoite |
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|contosodc1|TOIMIALUEEN OHJAUSKONEEN/ADFS|KOKONAISLUKU|contosodcset|contososac1|Staattinen|
|contosodc2|TOIMIALUEEN OHJAUSKONEEN/ADFS|KOKONAISLUKU|contosodcset|contososac2|Staattinen|
|contosowap1|VAIHDA|DMZ|contosowapset|contososac1|Staattinen|
|contosowap2|VAIHDA|DMZ|contosowapset|contososac2|Staattinen|

Kun olet ehkä jo huomannut, ei ole NSG ei ole määritetty. Tämä johtuu siitä azure avulla voit käyttää NSG aliverkon tasolla. Sitten voit hallita tietokoneen verkkoliikennettä avulla yksittäiset NSG liittyvät joko aliverkon tai muuten NIC-objekti. Lue lisää käytössä [verkon suojauksen ryhmän (NSG) ominaisuudet](https://aka.ms/Azure/NSG).
Jos hallinnoit DNS, on suositeltavaa staattinen IP-osoite. Voit käyttää Azure DNS ja sen sijaan toimialueen DNS-tietueet viittaa uudet tietokoneet niiden Azure täydelliset toimialuenimet mukaan.
Virtuaalikoneen-ruudussa pitäisi näyttää samalta kuin alla käyttöönoton jälkeen:

![Näennäiskoneiden on otettu käyttöön](./media/active-directory-aadconnect-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5. toimialueen ohjauskoneen määrittämisestä ja AD FS-palvelimet
 Todentaa saapuva-pyyntö, jotta AD FS on yhteyttä toimialueen ohjauskoneen. Jos haluat tallentaa kallista työmatkan azuren paikallisen toimialueen Ohjauskoneen todennusta varten, on suositeltavaa ottamaan käyttävien Azure toimialueen ohjauskoneen. Suuren käytettävyyden saavuttamiseksi suositellaan vähintään-2 toimialueen ohjaimet käytettävyys-joukon.

|Toimialueen ohjauskoneen|Rooli|Tallennustilan tilin|
|:-----:|:-----:|:-----:|
|contosodc1|Replikan|contososac1|
|contosodc2|Replikan|contososac2|

* Siirrä ylemmälle tasolle kaksi palvelimet kuin replikan toimialueen ohjaimet DNS
* AD FS-palvelimien määrittäminen asentamalla AD FS-roolin palvelimenhallinnan avulla.

###<a name="6---deploying-internal-load-balancer-ilb"></a>6. käyttöönotto sisäinen kuormituksen (ILB)

**6.1. ILB luominen**

Ottaa käyttöön ILB, valitse kuormituksen tasoitusmääritykset Azure portal ja valitsemalla Lisää (+).
>[AZURE.NOTE] Jos ei ole näkyvissä **Kuormituksen tasoitusmääritykset** -valikkoon, napsauta **Selaa** portaalin ja vieritä vasemmassa alakulmassa, kunnes näet **Kuormituksen tasoitusmääritykset**.  Valitse Lisää-valikkoon Keltainen tähti. Valitse seuraavaksi uusi kuormituksen tasauspalvelun kuvake avataksesi Ohjauspaneelin Aloita kuormituksen määrittäminen.

![Selaa kuormituksen](./media/active-directory-aadconnect-azure-adfs/browseloadbalancer.png)

* **Nimi**: Anna minkä tahansa sopiva nimi kuormituksen
* **Malli**: koska tämä kuormituksen sijoitetaan AD FS-palvelimet eteen ja on tarkoitettu sisäisten verkkoyhteyksien työkirjassa vain "Sisäinen"
* **VPN**: Valitse virtual verkko Jos otat AD FS
* **Aliverkon**: Valitse sisäinen aliverkon
* **IP-osoitteiden määritys**: dynaaminen

![Sisäinen kuormituksen](./media/active-directory-aadconnect-azure-adfs/ilbdeployment1.png)
 
Kun valitset luominen ja ILB on otettu käyttöön, pitäisi näkyä kuormituksen tasoitusmääritykset luettelossa:

![Kuormituksen tasaajiksi ILB jälkeen](./media/active-directory-aadconnect-azure-adfs/ilbdeployment2.png)
 
Seuraavaksi Taustajärjestelmä resurssivarantoon ja Taustajärjestelmä näytteenottimen määrittäminen.

**6.2. ILB Taustajärjestelmä sovellussarjan määrittäminen**

Valitse juuri luomasi ILB kuormituksen tasoitusmääritykset. Se avaa asetukset-paneeli. 
1.  Valitse asetukset-paneeli Taustajärjestelmä jakavat
2.  Valitse Lisää-toiminnolla Taustajärjestelmä resurssivarantoon Lisää virtuaalikoneen
3.  Voit valita jommankumman ja paneeli, jossa voit valita käytettävyyden määrittäminen
4.  Määritä AD FS-käytettävyys

![Määritä ILB Taustajärjestelmä resurssivarantoon](./media/active-directory-aadconnect-azure-adfs/ilbdeployment3.png)
 
**6.3. näytteenottimen määrittäminen**

Valitse keräysputkien ILB asetukset-paneeli.
1.  Napsauta Lisää
2.  Antaa tiedot näytteenottimen. **Nimi**: tutkia nimi b. **Protokolla**: TCP c-näppäinyhdistelmää. **Portti**: 443 (HTTPS) d. **Aikavälin**: 5 (oletusarvo) – tämä on lisättävä aikaväli, jolla ILB tutkia Taustajärjestelmä varannon e koneet. **Perusasemassa raja**: 2 (oletus val luokkaan UE asti) – tämä on peräkkäisiä näytteenottimen virheet, minkä jälkeen ILB määritellä koneen Taustajärjestelmä varannon pysähtyvän ja Lopeta liikenne lähettäminen raja-arvon.

![Määritä ILB näytteenottimen](./media/active-directory-aadconnect-azure-adfs/ilbdeployment4.png)
 
**6.4. kuormituksen sääntöjen luominen**

Liikenne suhteuttaa tehokkaasti, jotta ILB on määritettävä kuormituksen hallinta sääntöjen avulla. Jos haluat luoda säännön, kuormituksen 
1.  Valitse kuormituksen sääntöä ILB asetukset-paneeli
2.  Valitse Lisää sääntö Ohjauspaneelin kuormituksen
3.  Lisää-kuormituksen tasaamisen säännön Ohjauspaneelin. **Nimi**: Anna säännölle b nimeä. **Protokolla**: Valitse TCP c-näppäinyhdistelmää. **Portti**: 443 d. **Taustajärjestelmä portti**: 443 e. **Taustajärjestelmä resurssivarantoon**: Valitse olet luonut AD FS klusterin aiemmissa f vähäisen. **Näytteenotin**: Valitse aiemmin luotu AD FS-palvelinten näytteenottimen

![ILB tasaamisen sääntöjen määrittäminen](./media/active-directory-aadconnect-azure-adfs/ilbdeployment5.png)

**6.5. Päivitä DNS ILB**

Siirry DNS-palvelin ja luo CNAME ILB. CNAME-TIETUE on oltava federation-palvelun IP-osoite ILB osoittamalla IP-osoitteeseen. Esimerkki Jos ILB DIP-osoite on 10.3.0.8 ja asennettu federation-palvelu on fs.contoso.com, Luo fs.contoso.com 10.3.0.8 osoittamalla CNAME.
Näin varmistat, kaikki viestintä koskeva fs.contoso.com end ILB osoitteessa ylös ja reititetään asianmukaisesti.

###<a name="7---configuring-the-web-application-proxy-server"></a>7. Web-sovelluksen välityspalvelimen määrittäminen

**7.1. Web-sovelluksen välityspalvelimet saavuttaa AD FS-palvelimien määrittäminen**

Web-sovelluksen välityspalvelimet pysty muodostamaan yhteyttä takana ILB AD FS-palvelimiin varmistamiseksi Luo tietue %systemroot%\system32\drivers\etc\hosts ILB varten. Huomaa, että DN-nimi tulee federation palvelunimi, esimerkiksi fs.contoso.com. Ja IP-merkinnän pitäisi olla, joka ILB IP-osoite (esimerkiksi 10.3.0.8).

**7.2. Web-sovelluksen välityspalvelimen roolin asentaminen**

Kun olet varmistanut, että Web-sovelluksen välityspalvelimet ovat pysty muodostamaan yhteyttä takana ILB AD FS-palvelimiin, voit asentaa Web-sovelluksen välityspalvelimet Seuraava. Web-sovelluksen välityspalvelimien ei ole liitetty toimialue. Asenna Web-sovelluksen välityspalvelimen roolit kaksi Web Application-välityspalvelimet valitsemalla etäkäyttöpalvelimen rooli. Palvelimen hallinta auttaa sinua WAP-asennuksen viimeistelemiseen.
Lue lisätietoja WAP ottamisesta käyttöön on [asentaminen ja määrittäminen Web-sovelluksen välityspalvelinta](https://technet.microsoft.com/library/dn383662.aspx).

###<a name="8---deploying-the-internet-facing-public-load-balancer"></a>8. vastakkaisten (julkisten) kuormituksen Internet käyttöönotto

**8.1. vastakkaisten (julkisten) kuormituksen Internet luominen**
 
Azure-portaalissa Valitse kuormituksen tasoitusmääritykset ja valitse sitten Lisää. Luo kuormituksen tasauspalvelun-ruudussa Kirjoita seuraavat tiedot
1. **Nimi**: kuormituksen nimi
2. **Malli**: julkisen – tämä vaihtoehto kertoo Azure tämän kuormituksen on julkinen osoite.
3. **IP-osoite**: Luo uusi IP-osoite (dynaaminen)

![Internetiin yhteydessä olevan kuormituksen](./media/active-directory-aadconnect-azure-adfs/elbdeployment1.png)

Käyttöönoton jälkeen kuormituksen näkyvät kuormituksen tasoitusmääritykset-luettelossa.

![Kuormituksen tasauspalvelun luettelo](./media/active-directory-aadconnect-azure-adfs/elbdeployment2.png)
 
**8.2. määrittää julkisen IP DNS-otsikko**

Valitse juuri luomasi kuormituksen tasauspalvelun tapahtuma ja tuo näkyviin kokoonpanon Ohjauspaneelin kuormituksen tasoitusmääritykset-ruudussa. Noudata alla ohjeita voit määrittää DNS-otsikon julkiseen IP-osoite:
1.  Valitse julkinen IP-osoite. Tämä avaa paneelin julkiseen IP-ja asetukset
2.  Valitse määritys
3.  Anna DNS-otsikko. Tämä muuttuu julkisen DNS selite, jota voit käyttää missä tahansa, kuten contosofs.westus.cloudapp.azure.com. Voit lisätä merkinnän ulkoiseen DNS: ää federation palvelu (esimerkiksi fs.contoso.com), joka korjaa ulkoisen kuormituksen (contosofs.westus.cloudapp.azure.com) DNS-otsikko.

![Määritä internet vastakkaisten kuormituksen](./media/active-directory-aadconnect-azure-adfs/elbdeployment3.png) 

![Määritä internet vastakkaisten kuormituksen (DNS)](./media/active-directory-aadconnect-azure-adfs/elbdeployment4.png)

**8.3. Taustajärjestelmä sovellussarjan määrittäminen kuormituksen Internet vastakkaisten (julkinen)** 

Toimi samoin kuin luominen sisäinen kuormituksen Taustajärjestelmä resurssivarantoon määrittämiseksi Internet vastakkaisten (julkisten) ladata tasaustoiminto kuin määrittäminen WAP palvelimia käytettävyys. Esimerkiksi contosowapset.

![Määritä Internet vastakkaisten kuormituksen resurssivarantoon taustaan](./media/active-directory-aadconnect-azure-adfs/elbdeployment5.png)
 
**8,4. näytteenottimen määrittäminen**

Toimi samoin kuin määrittäminen sisäinen kuormituksen määrittäminen keräysputken WAP palvelinten Taustajärjestelmä ryhmää varten.

![Määritä Internet vastakkaisten kuormituksen näytteenottimen](./media/active-directory-aadconnect-azure-adfs/elbdeployment6.png)
 
**8.5. kuormituksen sääntöjen luominen**

Suorita sitten samat vaiheet ILB voit määrittää kuormituksen säännön TCP 443, kuten.

![Määritä Internet vastakkaisten kuormituksen Vastatilin säännöt](./media/active-directory-aadconnect-azure-adfs/elbdeployment7.png)
 
###<a name="9---securing-the-network"></a>9. suojaamiseen verkossa

**9.1. sisäinen aliverkon suojaaminen**

Yleistä tarvitset seuraavien sääntöjen suojaamiseen tehokkaasti sisäinen aliverkon ulkopuolella (siinä järjestyksessä, kuten seuraavassa)

|Sääntö|Kuvaus|Työnkulku|
|:----|:----|:------:|
|AllowHTTPSFromDMZ| Salli viestintä HTTPS DMZ | Saapuva |
|DenyInternetOutbound| Ei voi käyttää internet | Lähtevän |

![INT-käyttösäännöt (saapuvat)](./media/active-directory-aadconnect-azure-adfs/nsg_int.png)

[kommentti]: <> (![INT käyttösäännöt (saapuvat)](./media/active-directory-aadconnect-azure-adfs/nsgintinbound.png)) [kommentti]: <> (![INT käyttösäännöt (lähtevät)](./media/active-directory-aadconnect-azure-adfs/nsgintoutbound.png))
 
**9.2. DMZ aliverkon suojaaminen**

|Sääntö|Kuvaus|Työnkulku|
|:----|:----|:------:|
|AllowHTTPSFromInternet| Salli HTTPS DMZ Internetistä | Saapuva|
|DenyInternetOutbound|  Jokin muu kuin HTTPS Internet-yhteys on estetty | Lähtevän |

![Tunniste käyttösäännöt (saapuvat)](./media/active-directory-aadconnect-azure-adfs/nsg_dmz.png)

[kommentti]: <> (![tunniste käyttösäännöt (saapuvat)](./media/active-directory-aadconnect-azure-adfs/nsgdmzinbound.png)) [kommentti]: <> (![tunniste käyttösäännöt (lähtevät)](./media/active-directory-aadconnect-azure-adfs/nsgdmzoutbound.png))

>[AZURE.NOTE] Jos asiakkaan käyttäjän varmenteen todentaminen (asiakkaan TLS-salauksella todennuksen X509 käyttäjävarmenteet) tarvitaan, valitse AD FS edellyttää, että TCP portin 49443 saapuvien ottamista käyttöön.

###<a name="10--test-the-ad-fs-sign-in"></a>10. testin AD FS-kirjautuminen

Helpoin tapa on, voit testata AD FS on IdpInitiatedSignon.aspx-sivulla. Jotta voit valita, että se on tarpeen, jotta IdpInitiatedSignOn AD FS-ominaisuuksia. Noudata seuraavia ohjeita vahvistamiseksi AD FS-asetukset
1.  Suorita alapuolella cmdlet-komento, AD FS-palvelimessa, käyttämällä PowerShell-asetukseksi käytössä.
    Määritä AdfsProperties - EnableIdPInitiatedSignonPage $true 
2.  Valitse kaikki ulkoisen tietokoneen access https://adfs.thecloudadvocate.com/adfs/ls/IdpInitiatedSignon.aspx  
3.  Näyttöön tulee AD FS-sivu, kuten alla:

![Kirjaudu sisään testisivu](./media/active-directory-aadconnect-azure-adfs/test1.png)

Onnistuneiden Kirjaudu sisään se antaa sinulle onnistui-sanoma alla kuvatulla tavalla:

![Testaa onnistui](./media/active-directory-aadconnect-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Käyttöönotto-Azure AD FS-malli

Malli otetaan käyttöön 6 tietokoneen asetukset-2 toimialueen ohjaimet, AD FS ja WAP.

[AD FS Azure käyttöönoton mallissa](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

Voit käyttää aiemmin virtual verkkoon tai luoda uuden VNET otettaessa tätä mallia. Käytettävissä olevista käyttöönoton eri parametrit on lueteltu alla käyttö käyttöönottoprosessin parametrin kuvausta. 

| Parametri | Kuvaus |
|:--------|:-----|
|Sijainti| Ottaa käyttöön resurssit käännetään, esimerkiksi Itä US alue. |
|StorageAccountType| Luotu tallennustilan tilin tyyppi|
|VirtualNetworkUsage| Ilmaisee, jos uuden virtual verkon luodaan tai Käytä aiemmin luotua|
|VirtualNetworkName| Luo pakollinen sekä olemassa oleva vai uusi VPN-käyttö Virtual verkon nimi|
|VirtualNetworkResourceGroupName| Määrittää olemassa olevan virtual verkoston sijainti resurssiryhmän nimi. Olemassa olevan virtual verkoston käytettäessä tämä on pakollinen parametri, käyttöönoton avulla voit etsiä olemassa olevan virtual verkoston tunnus|
|VirtualNetworkAddressRange| Uusi VNET pakollinen, jos luot uuden virtual verkon osoitealue|
|InternalSubnetName| Sisäinen aliverkon pakollinen käyttöönotto sekä VPN käyttö (uuteen tai aiemmin luotuun) nimi|
|InternalSubnetAddressRange| Sisäinen aliverkon, joka sisältää toimialueohjaimia ja ADFS palvelimet, pakollisia, jos luot uuden virtual verkon osoitteen alue.|
|DMZSubnetAddressRange| Dmz-aliverkon, joka sisältää Windows sovelluksen välityspalvelimet, pakollinen, jos luot uuden virtual verkon osoitteen alue.|
|DMZSubnetName| Sisäinen aliverkon pakollinen käyttöönotto sekä VPN käyttö (uuteen tai aiemmin luotuun) nimi. |
|ADDC01NICIPAddress| Sisäinen ensimmäisen toimialueen ohjauskoneen IP-osoite, IP-osoitteen Palvelinkeskus nimipalvelimet määritetään ja on oltava sisäinen aliverkon sisällä kelvollinen ip-osoite|
|ADDC02NICIPAddress| Toisen toimialueen ohjauskoneen sisäinen IP-osoite, IP-osoitteen Palvelinkeskus nimipalvelimet määritetään ja on oltava sisäinen aliverkon sisällä kelvollinen ip-osoite|
|ADFS01NICIPAddress| Ensimmäinen ADFS-palvelimeen sisäinen IP-osoite, tämä IP-osoite määritetään nimipalvelimet ADFS-palvelimeen, ja on oltava sisäinen aliverkon sisällä kelvollinen ip-osoite|
|ADFS02NICIPAddress| Toinen ADFS-palvelimeen sisäinen IP-osoite, tämä IP-osoite määritetään nimipalvelimet ADFS-palvelimeen, ja on oltava sisäinen aliverkon sisällä kelvollinen ip-osoite|
|WAP01NICIPAddress| Sisäinen ensimmäisen WAP palvelimen IP-osoite, tämä IP-osoite määritetään nimipalvelimet WAP palvelimeen ja on oltava DMZ aliverkon sisällä kelvollinen ip-osoite|
|WAP02NICIPAddress| Sisäinen toisen WAP palvelimen IP-osoite, IP-osoitteen määritetään nimipalvelimet WAP palvelimeen ja on oltava DMZ aliverkon sisällä kelvollinen ip-osoite|
|ADFSLoadBalancerPrivateIPAddress| ADFS kuormituksen sisäinen IP-osoite, IP-osoitteen kuormituksen nimipalvelimet määritetään ja on oltava sisäinen aliverkon sisällä kelvollinen ip-osoite|
|ADDCVMNamePrefix| Virtuaalikoneen etuliite toimialueen ohjaimet|
|ADFSVMNamePrefix| ADFS-palvelinten virtuaalikoneen etuliite|
|WAPVMNamePrefix| Virtuaalikoneen etuliite WAP palvelimia|
|ADDCVMSize| Toimialueen ohjaimet AM kokoa|
|ADFSVMSize| ADFS-palvelimet AM kokoa|
|WAPVMSize| Vaihda-palvelimet AM kokoa|
|AdminUserName| Näennäiskoneiden paikallisen järjestelmänvalvojan nimi|
|AdminPassword| Näennäiskoneiden paikallisen järjestelmänvalvojatilin salasanan|

## <a name="additional-resources"></a>Lisäresursseja
* [Käytettävyys joukot](https://aka.ms/Azure/Availability ) 
* [Azure kuormituksen](https://aka.ms/Azure/ILB)
* [Sisäinen kuormituksen](https://aka.ms/Azure/ILB/Internal)
* [Internet-yhteydessä kuormituksen](https://aka.ms/Azure/ILB/Internet)
* [Tallennustilan tilit](https://aka.ms/Azure/Storage )
* [Azure Virtual verkot](https://aka.ms/Azure/VNet)
* [AD FS ja Web-sovelluksen välityspalvelimen linkit](http://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>Seuraavat vaiheet

* [Azure Active Directory-integraation paikallisen käyttäjätietoja](active-directory-aadconnect.md)
* [Määrittämisestä ja hallinnasta että AD FS Azure AD Connect käyttäminen](active-directory-aadconnectfed-whatis.md)
* [Suuren käytettävyyden rajat maantieteelliset AD FS käyttöönoton Azure-tietokannassa Azure liikenteen hallinta](active-directory-adfs-in-azure-with-azure-traffic-manager.md)




