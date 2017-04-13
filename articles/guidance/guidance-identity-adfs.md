<properties
   pageTitle="Käyttöönoton ADFS Azure | Microsoft Azure"
   description="Miten toteuttavien suojatun hybrid verkko-arkkitehtuuri, Active Directory Federation-palvelun todennus ja Azure."
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>Azure Active Directory-liittoutumispalvelut (ADFS) toteuttaminen

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

Tässä artikkelissa kerrotaan parhaita käytäntöjä, joka ulottuu paikallisen verkon Azure ja, joka käyttää [Active Directory Federation Services (ADFS)] suojatun hybrid verkoston[ active-directory-federation-services] suorittamiseen liitetyt todennus- ja pilveen-osaa. Tämä arkkitehtuuri laajentaa [Laajentaminen Azure Active Directory]on kuvattu rakenteen[implementing-active-directory].

> [AZURE.NOTE] Azure on kaksi eri käyttöönoton mallit: [Resurssienhallinta] [ resource-manager-overview] ja perinteinen. Viite-arkkitehtuuri käyttää Resurssienhallinta, johon Microsoft suosittelee uuden käyttöönotoissa.

ADFS suorittamisen paikallisen, mutta jos sovellukset sijaitsevat Azure hybrid tilanne voi olla tehokkaampaa toteuttaa tämä toiminnallisuus pilveen. Tavallisesti tämä arkkitehtuuri laatikkomäärät ovat seuraavat:

- Hybrid sovellukset missä työmääriä Suorita osittain paikallisen ja osittain tekstimuodossa Azure.

- Ratkaisuja, jotka käyttävät liitetyt lupa, voit näyttää web-sovellusten kumppanin organisaatioille.

- Järjestelmät, jotka tukevat selaimet käynnissä organisaation palomuurin ulkopuolella käytöltä.

- Järjestelmät, joiden avulla käyttäjät voivat käyttää web-sovellusten yhdistämällä valtuutettujen ulkoisen laitteilla, kuten etätietokoneiden, muistikirjat ja muille mobiililaitteille. 

Katso lisätietoja ADFS: N toiminnasta [Active Directory Federation Services-palvelujen yleiskatsaus][active-directory-federation-services-overview]. Lisäksi on artikkelissa [Azure ADFS: N käyttöönottoa] [ adfs-intro] sisältää yksityiskohtaiset vaiheittaiset johdanto ADFS: N käyttöönoton Azure-tietokannassa.

## <a name="architecture-diagram"></a>Arkkitehtuurikaavio

Seuraavassa kaaviossa korostaa tärkeitä osia tämän arkkitehtuuri (*Lähennä*). Lue lisätietoja elementtinä ei liity ADFS [käyttöönoton suojatun hybrid-verkoston arkkitehtuuri Azure-tietokannassa][implementing-a-secure-hybrid-network-architecture], [käyttöönoton suojatun hybrid verkko-arkkitehtuuri Azure Internet-yhteyden][implementing-a-secure-hybrid-network-architecture-with-internet-access], ja [toteuttaminen suojatun hybrid verkko-arkkitehtuuri, ja Azure Active Directory käyttäjätiedot][implementing-active-directory]:

[! [0]][0]

>[AZURE.NOTE] Tässä kaaviossa on esitetty käyttöä seuraavissa tapauksissa:
>
>- Sovelluksen koodin suorittaminen kumppaniorganisaation sisällä noutaa Isännöidyt sisäpuolelle Azure VNet web-sovelluksen.
>
>- Ulkoinen, rekisteröidyn käyttäjän (ja lisää sisällä tallennettuja tunnistetietoja) Isännöidyt sisäpuolelle Azure VNet web-sovelluksen käyttämiseen.
>
>- Yhteyden muodostaminen oman VNet valtuutettujen laitteella ja suorittamalla Isännöidyt sisäpuolelle Azure VNet web-sovelluksen käyttäjä.
>
>Lisäksi tässä arkkitehtuuri ohjeessa on passiivinen federaatio, jossa liittoutumispalvelimia päättää, milloin ja miten käyttäjä todennetaan. Käyttäjän odotetaan antamaan kirjautumistiedot, kun sovellus käynnistyy. Tämä on useimmin käytetyt selaimissa järjestelmä ja liittyy protokolla, joka ohjaa selaimessa sivustoon, johon käyttäjä voi lisätä tunnistetietoja. ADFS tukee myös aktiivinen federaatio, jolla sovellus kestää vastuu toimittavat tunnistetiedot ilman muita käyttäjän toimia, mutta tässä tapauksessa on tämän arkkitehtuuri ulkopuolella.

- **Lisää aliverkon.** Lisää palvelimet sisältyvät omia aliverkon. NSG sääntöjen avulla voit suojata Lisää palvelimet, ja se voi tarjota palomuuri vastaan odottamattomia lähteistä.

- **Lisää palvelimet.** Nämä ovat käynnissä VMs pilveen toimialueen ohjaimet. Seuraavissa palvelimissa voi todentaa paikallisen toimialueen käyttäjätietoja.

- **ADFS aliverkon.** ADFS-palvelimet voi olla oman aliverkon sijaitsevat NSG sääntöjen visuaalisessa muodossa palomuuri.

- **ADFS-palvelimiin.** ADFS-palvelimissa on liitetty todennus ja käyttöoikeuksien. Tämä arkkitehtuuri ne suorittaa seuraavia tehtäviä:

    - He saavat suojauksen tunnusten sisältävä saatavat tekemät kumppanin liittoutumispalvelimen kumppanin käyttäjän puolesta. ADFS voit varmistaa, että näiden tunnusten ovat kelvollisia ennen kulkeva saatavat Azure web-sovelluksen avulla. (Valitse Azure) yrityksen web-sovelluksen avulla nämä vaateita, jotka sallivat pyynnöt. Tässä skenaariossa yrityksen web-sovelluksen on *käyttäisit osapuolen*ja se on kumppani liittoutumispalvelimen antaa vastuu väittää, jotka ovat ymmärrä yrityksen web-sovelluksen. Kumppanin liittoutumispalvelimia kutsutaan *tilin kumppanien* koska lähettämistä käyttöoikeuspyyntöjen todennetut tilit kumppanin organisaation puolesta. ADFS-palvelimia kutsutaan *resurssin kumppanit* , koska ne sisältävät resurssien (Tässä tapauksessa web sovellukset).

    - Hän voi todentaa (Lisää ja [Active Directory-laitteen rekisteröinti-palvelun]kautta[ADDRS]) ja sallia ulkoisten käyttäjien selaimen tai laitteesta, sen yrityksen web-sovellusten käyttöoikeudet-pyynnöt. 

    ADFS-palvelimet on määritetty klusteria Azure kuormituksen kautta. Tämä rakenne auttaa parantamaan käytettävyyttä ja skaalattavuus. Huomaa, että ADFS-palvelimet eikä niitä julkaista yhteys Internetiin, ADFS: N web-sovelluksen välityspalvelimet ja jos kyseessä on suodatettu sen sijaan, että kaikki Internet-liikenne.

- **ADFS välityspalvelimen aliverkon.** ADFS-välityspalvelimet voi sisältää omia aliverkon suojan tarjoavan NSG säännöillä. Tämän aliverkon palvelimissa niitä julkaista Internetissä verkon virtual laitteita, jotka tarjoavat Azure virtual verkon ja Internetin välille palomuurin läpi.

- **ADFS: N web-sovelluksen välityspalvelimet (WAP).** Näiden tietokoneiden edustajana pyynnöt kumppanin organisaatiot ja ulkoiset laitteissa ADFS-palvelimeen. Vaihda-palvelimet toimia suodattimena Suojaavassa suoraan Accessista julkisesta Internetistä ADFS-palvelimiin. ADFS-palvelinten kanssa käyttöönotto WAP palvelinfarmiin kanssa kuormituksen tasaamisen avulla voit käyttää suurempi käytettävyys ja skaalattavuus kuin erillisissä palvelimissa kokoelma käyttöönotto.

    >[AZURE.NOTE] Lisätietoja WAP palvelinten asentamisesta on kohdassa [asentaminen ja määrittäminen Web-sovelluksen välityspalvelimen][install_and_configure_the_web_application_proxy_server]

- **Kumppaniorganisaatio.** Tämä on organisaation Esimerkki kumppani, joka suoritetaan, joka pyytää access web-sovelluksen Azure web-sovelluksen avulla. Kumppanin organisaatio liittoutumispalvelimen todentaa paikallisesti pyyntöjä ja lähettää suojauksen tunnusten sisältävä saatavia ADFS Azure käynnissä. ADFS Azure vahvistaa suojaus-tunnusten ja jos ne ovat kelvollisia voi kulua saatavat ne sallivat Azure käynnissä verkkosovellukseen.

    >[AZURE.NOTE] Voit myös määrittää VPN-tunnelin tutustuminen tarjota ADFS kumppaneille Luotetut Azure yhdyskäytävän avulla. Nämä yhteistyökumppanien pyyntöjen Älä välitä WAP palvelinten kautta.

## <a name="recommendations"></a>Suosituksia

Tässä osassa on yhteenveto suosituksia ADFS Azure-tietokannassa, joka kattaa:

- AM suosituksia.

- Verkko-suosituksia.

- Käytettävyys suosituksia.

- Suojausta koskevia suosituksia.

- ADFS: N asennuksen suosituksia.

- ADFS luota suosituksia.

### <a name="vm-recommendations"></a>AM suositukset

Voit luoda VMs käsitellään liikenteen odotettu äänenvoimakkuuden riittävät resurssit. Käytä aiemmin koneet isännöinnin ADFS paikallinen lähtökohtana kokoa. Voit seurata resurssien käyttö. Voit VMs kokoa ja skaalata, jos ne ovat liian suuri.

Noudata [käynnissä Windows-AM Azure-]luettelossa suosituksia[vm-recommendations].

### <a name="networking-recommendations"></a>Verkko-suositukset

Määrittää isännöinnin ADFS ja WAP palvelinten staattinen yksityisten IP-osoitteiden VMs Verkkokäyttöliittymän.

Anna ADFS VMs julkiseen IP-osoitteet. Lisätietoja on artikkelissa [suojausasiat][security-considerations].

Määritä verkkoliittymät jokaisella ADFS ja WAP AM viitata Lisää VMs (joka on oltava käynnissä DNS) ensisijainen ja Toissijainen DNS-palvelimilta IP-osoite. Tämä vaihe on tarpeen, jotta jokaisen AM toimialueeseen.

### <a name="availability-recommendations"></a>Käytettävyys suositukset

Luo ADFS-klusterin palvelinten vähintään kaksi niin, että käytettävyys ADFS-palvelun kanssa.

Käytä kunkin ADFS AM klusterissa eri tallennustilan tilit. Tämän menetelmän avulla voidaan varmistaa, että epäonnistui yksittäisen tallennustilaa tilillä ei tee koko palvelinfarmin ei ole käytettävissä.

Luoda erilliset Azure käytettävyys ADFS ja WAP VMs. Varmista, että kussakin joukossa on vähintään kaksi VMs. Käytettävyys kukin ehtojoukko on oltava vähintään kaksi Päivitä toimialueet ja kaksi vika toimialuetta.

Määritä kuormituksen tasoitusmääritykset ADFS VMs ja WAP VMs seuraavasti:

- Käytä Azure kuormituksen ulkoisen käytön tarjota WAP VMs ja jakaa ADFS-klusterin ADFS-palvelinten kuormituksen sisäinen kuormituksen.

- Välitä vain näkyvä porttiin 443 (HTTPS) ADFS/WAP-palvelimien liikenteen.

- Anna kuormituksen staattinen IP-osoite.

- Luo kunto-näytteenottimen käyttää TCP-protokolla HTTPS-vaihtoehdon sijaan. Ping-komennon porttiin 443, tarkista, että ADFS-palvelimeen toimii.

    >[AZURE.NOTE] ADFS-palvelimet käyttää palvelimen nimi merkintä (SNI)-protokollaa, niin yritetään tutkia käyttämällä päätepisteen HTTPS-kuormituksen tasauspalvelun epäonnistuu.

- DNS- *A* -tietueen lisääminen toimialueen ADFS kuormituksen. 

    Määritä kuormituksen IP-osoite ja anna sille nimi toimialueen (kuten adfs.contoso.com). Tämä on, jolla asiakkaat ja WAP-palvelimet käyttää ADFS palvelinklusterin nimi.

### <a name="security-recommendations"></a>Suojausta koskevia suosituksia

Estä suoraan näyttäminen ADFS-palvelimiin Internetissä. ADFS-palvelimia toimialueeseen liittyneet tietokoneissa, joissa on koko käyttöoikeuksien myöntäminen suojauksen tunnusten. ADFS-palvelimeen on ongelmia, jos joku lähettää täydet tunnusten kaikista verkkosovelluksista ja liittoutumispalvelimia, jotka on suojattu ADFS. Jos järjestelmässä on käsiteltävä pyynnöt Ulkoiset käyttäjät eivät välttämättä yhteyden luotettujen sivustoista, WAP palvelinten avulla voit käsitellä nämä pyynnöt. Lisätietoja on artikkelissa [sijaintipaikka Liittoutumispalvelimen välityspalvelimen][where-to-place-an-fs-proxy].

Paikkaan ADFS-palvelimia ja WAP palvelimet erillisessä aliverkosta omia palomuurit kanssa. NSG sääntöjen avulla voit määrittää palomuurisäännöt. Jos tarvitset monipuolisemman suojaus voit ottaa käyttöön lisäsuojauksen kehyksen reunan palvelinten ympärille käyttämällä aliverkosta ja NVAs, [käyttöönoton suojatun hybrid verkko-arkkitehtuuri Internet-yhteyden Azure]asiakirja kuvatulla[implementing-a-secure-hybrid-network-architecture-with-internet-access]. Huomaa, että kaikki palomuurit sallittava liikenne porttiin 443 (HTTPS).

Rajoittaa suoraan kirjautuminen ADFS ja WAP-palvelimiin. Vain DevOps henkilökunnan pitäisi muodostaa.

Älä yhdistä WAP palvelimet toimialueeseen.

### <a name="adfs-installation-recommendations"></a>ADFS: N asennuksen suositukset

Artikkelin [käyttöönotto Federation palvelinfarmiin] [ Deploying_a_federation_server_farm] on tarkat ohjeet asentaminen ja määrittäminen ADFS. Suorita seuraavat toimet ennen määrittäminen klusterissa ensimmäisen ADFS-palvelimeen:

1. Hanki julkisesti luotetun sertifikaatin palvelimen todennuksen tekemistä varten. *Aihe* on oltava nimi, jonka asiakkaat käyttää federation-palvelua. Tämä voi rekisteröidä kuormituksen, kuten *adfs.contoso.com* DNS-nimi (välttää käyttää yleismerkkiä nimet, kuten **. contoso.com*, tietoturvasyistä). Käytä samaa varmennetta VMs kaikki ADFS-palvelimeen. Voit ostaa sertifikaatin Luotetut varmenteiden myöntäjältä, mutta jos organisaatiosi käyttää Active Directory-sertifikaatin palveluihin voit luoda omia. 

    *Aiheen vaihtoehtoinen nimi* käytetään DRS käyttämiseksi ulkoisen laitteilla. Tämä on oltava lomakkeen *enterpriseregistration.contoso.com*.

    Lisätietoja on artikkelissa [hankkiminen ja määrittäminen ADFS: N SSL-varmenne][adfs_certificates].

2. Toimialueen ohjauskoneen Luo uusi pääkansion avain avain jakauman-palveluun. Määritä todellista aikaa on 10 tuntia (tämän määrityksen vähentää viive, joita voi syntyä jakaminen ja synkronoiminen näppäimet koko toimialueella) miinus nykyisen kellonajan. Tämä vaihe on tarvittavat valmiudet luominen ryhmän palvelutilin, joita käytetään ADFS-palvelun. Powershell-komentoa näkyy, miten toiminto:

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. Lisää kunkin ADFS-palvelimeen AM toimialueeseen.

>[AZURE.NOTE] Voit asentaa ADFS-käynnissä PDC-emulointikoneen FSMO-roolin toimialueen ohjauskoneen on oltava käynnissä ja käytettävissä olevat ADFS-VMs.

### <a name="adfs-trust-recommendations"></a>ADFS luota suositukset

Muodostaa federation luota ADFS: N asennus ja organisaatioita kumppani, liittoutumispalvelimia välillä. Suodattaminen vaateet ja -määritys pakollinen määrittäminen. Tämä toimenpide on:

- DevOps henkilöstön **osoitteessa kunkin kumppaniorganisaation** Lisää varmenteen käyttäjän osapuolen Salli web-sovellusten kautta ADFS-palvelimiin.

- **Organisaation** määrittäminen saatavat tarjoaja luota käyttöön, joissa on kumppani organisaatiot claims luotetaanko ADFS palvelinten henkilöstön DevOps.

- **Organisaation** määrittäminen ADFS välittää saatavat voin organisaation verkkosovellusten henkilöstön DevOps.

    Lisätietoja on artikkelissa [Vahvistamiseksi Federation luota][establishing-federation-trust].

Organisaation verkkosovellusten ja ne ovat käytettävissä ulkopuolisten kumppaneiden, käyttämällä esitarkistusta WAP palvelinten kautta. Lisätietoja on ohjeaiheessa [julkaista sovellusten ADFS Preauthentication][publish_applications_using_AD_FS_preauthentication]

Huomaa, että ADFS tukee suojaustunnuksen muunnos ja siellä. Azure Active Directory ei tarjoa tätä ominaisuutta. ADFS, kun määrität luottamussuhteet, voit:

- Määritä ryhmän muunnoksia, käyttöoikeuksien myöntämissääntöjä. Voit esimerkiksi yhdistää ryhmän suojaus jotakin, mitä, Lisää voit antaa organisaation-Microsoft kumppaniorganisaation käyttämän esitys.

- Muunna saatavat muodosta toiseen. Esimerkiksi voit yhdistää SAML 2.0 SAML 1.1 Jos sovellus tukee vain SAML 1.1 saatavat. 

## <a name="availability-considerations"></a>Käytettävyys huomioon otettavia seikkoja

Voit käyttää SQL Server tai Windowsin sisäisen tietokannan (WID) ADFS kokoonpanotietoja pitoon. WID tarjoaa basic redundanssin. Muutokset kirjoitetaan suoraan vain yksi ADFS-klusterin ADFS-tietokantojen samalla, kun muut palvelimet avulla salaus puretaan replikoinnin tietokantansa pitäminen ajan tasalla. SQL Serverin avulla voit antaa koko tietokannan redundanssin ja suuren käytettävyyden käyttämällä klusterointi tai peilaukseen automaattisesti.

## <a name="security-considerations"></a>Suojausasiat

ADFS käyttämällä HTTPS-protokollan, joten varmista, että sivuston sisältävän aliverkon NSG säännöt taso VMs Salli HTTPS-pyyntöjä. Nämä pyynnöt voit peräisin paikalliseen verkkoon, aliverkosta, joka sisältää web-taso, business taso, tietojen taso, yksityinen DMZ, julkisen DMZ ja aliverkon sisältävä ADFS-palvelimiin.

Harkitse joukko verkon virtual laitteita, joiden kirjautuu liikenne sallita ohittaa virtual verkkoa reunan tarkistettaviksi tarkoituksiin yksityiskohtaiset tiedot.

## <a name="scalability-considerations"></a>Skaalattavuus huomioon otettavia seikkoja

Seuraavat asiat, yhteenveto [ADFS: N käyttöönoton]asiakirjasta[plan-your-adfs-deployment], anna pohjana for koon ADFS klustereihin:

- Jos ympäristössä on alle 1 000 käyttäjää, Luo oma ADFS-palvelimet, mutta sen sijaan asentaa ADFS kunkin Lisää palvelimet pilvipalvelussa (Varmista, että sinulla on vähintään kaksi Lisää palvelimia pitämään käytettävyys). Luo yksittäisen WAP palvelimen.

- Jos sinulla on 1 000 ja 15000 käyttäjien välillä, luo kaksi erillinen ADFS-palvelimia ja kaksi erillinen WAP-palvelimiin.

- Jos sinulla on 15000 ja 60000 käyttäjien välillä, luo kolme ja viisi erillinen ADFS-palvelimia ja vähintään kaksi erillinen WAP-palvelinten välillä.

Näiden lukuja oletetaan, että käytössäsi on kaksi quad core VMs (vakio D4_v2 tai parempi) isännöimiseen Azure-palvelimiin.

Huomaa, että jos käytössäsi on Windows sisäisessä tietokannassa ADFS: N määritystietojen tallentamiseen voit rajoitettu klusterissa kahdeksan ADFS-palvelimiin. Jos arvelet, että hänen nimeään tarvitse Lisää, käytä SQL Server. Lisätietoja [Roolin ADFS kokoonpanotietokannan][adfs-configuration-database].

## <a name="management-considerations"></a>Hallinnan huomioon otettavia seikkoja

DevOps henkilökunnan laaditaan voit suorittaa seuraavia tehtäviä:

- Hallinta liittoutumispalvelimia (ADFS-klusterin hallinta, luota käytännön federation palvelimissa, hallinta ja hallinta liittoutumispalvelut sertifikaatit).

- Hallinta WAP palvelimet (WAP-klusterin hallinta WAP varmenteet hallinta).

- (Määrittäminen varmenteen käyttäjän osapuolille, todennusmenetelmät ja saatavat yhdistämismääritykset) verkkosovellusten hallinta

- Varmuuskopioidaan ADFS-osia.

## <a name="monitoring-considerations"></a>Huomioon otettavia seikkoja seuranta

[Microsoft System Center Management Pack for Active Directory Federation Services 2012 R2: n] [ oms-adfs-pack] tarjoaa ennakoivan- ja uudelleenaktivointi seuranta ADFS: N käyttöönoton federation palvelimen. Tämä hallintapaketin valvoo:

- Tapahtumien ADFS palvelun tietueiden ADFS tapahtumalokit.

- ADFS: N suorituskyvyn laskureita kerätä suorituskykytietoja. 

- ADFS: N järjestelmä- ja web sovellusten (varmenteen käyttäjän osapuolet), yleinen kunto sekä annetaan kriittisten virheiden ja varoitusten ilmoitukset.

## <a name="solution-components"></a>Ratkaisun osat

Esimerkki ratkaisu komentosarja [Käyttöönotto ReferenceArchitecture.ps1][solution-script], on käytettävissä, voit toteuttaa arkkitehtuuri, joka on tässä artikkelissa kuvattuja suosituksia. Tämä komentosarja käyttämällä Azure Resurssienhallinta malleja. Joukko keskeisiä rakenneosien kukin ottavat suorittaa tietyn toiminnon, kuten VNet luomista tai määrittämistä NSG käytettävissä olevat mallit. Komentosarjan tarkoituksena on orchestrate mallin käyttöönottoa.

Mallit ovat Parametroitu parametreilla erillisessä JSON-tiedostoissa. Voit muokata näitä tiedostoja määrittäminen vastaamaan omia tarpeita käyttöönoton parametrit. Sinun ei tarvitse muuttamiseksi itse mallit. Huomaa, että et saa muuttaa parametritiedostot objektit rakenteet.

Mallien muokkaaminen, luominen objekteja, jotka noudattavat kuvattu [Suositellut nimeäminen nimeämiskäytännön Azure resurssien]nimeämiskäytännöt[naming-conventions].

Näyte Luo ja määrittää ympäristön web taso, business taso ja tietojen käytön osia, VPN-yhdyskäytävän ja hallinnan taso Lisää aliverkon ja palvelimissa, ADFS aliverkon ja palvelimissa, ADFS välityspalvelimen aliverkon ja -palvelimet-DMZ pilveen. Näyte myös vaihtoehtoinen määritys Simuloitu paikallisen ympäristön luomiseen.

Seuraavissa osissa kuvaillaan elementit paikallisia, sekä cloud määrityksiä.

### <a name="on-premises-components"></a>Paikallisen osat

>[AZURE.NOTE] Nämä osat eivät ole arkkitehtuuri, joka on kuvattu tämän asiakirjan ensisijaisen kohdistuksen ja on tarkoitettu vain antaa sinulle mahdollisuuden Testaa cloud-ympäristössä turvallisesti reaali tuotantoympäristössä sijaan. Tästä syystä tämän osan yhteenveto vain avaimen parametritiedostot. Voit muuttaa asetuksia, kuten IP-osoitteet tai VMs koot, mutta se on suositeltavaa, että jätä monet muut parametrit ei muutu.

Tässä ympäristössä käsittää AD-metsää contoso.com-toimialueen. Toimialueen sisältää kaksi IP-osoitteita 192.168.0.4 ja 192.168.0.5 Lisää palvelimiin. Nämä kaksi palvelimet myös suorittamalla DNS-palvelusta. Paikallisen järjestelmänvalvojatilin sekä VMs kutsutaan `testuser` salasanalla `AweS0me@PW`. Lisäksi määritykset määrittää VPN-yhdyskäytävän muodostamisesta VNet pilveen. Voit muokata määritykset muokkaamalla seuraavat JSON tiedostot sijaitsevat [**Parametrit/paikallisesti käytettävät versiot**] [ on-premises-folder] kansiossa:

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**. Tämä tiedosto määrittää paikallisen ympäristön verkko-osoitetilaa varten.

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**. Tiedoston nimi sisältää Lisää isännöintipalveluina paikallisen VMs määrityskohde. Kaksi *Vakio-DS3-v2* VMs luodaan oletusarvon mukaan.

- ** [virtualNetworkGateway.parameters.json] [ on-premises-virtualnetworkgateway-parameters] ** ja ** [connection.parameters.json][on-premises-connection-parameters]**. Nämä tiedostot pidä Azure VPN-yhdyskäytävän VPN-yhteyden asetusten pilvipalvelussa, mukaan lukien jaetun käytettävän suojaaminen liikenne sallita ohittaa yhdyskäytävän avaimen.

Jäljellä olevat tiedostot-kansiossa sisältää paikallisen toimialueen, käyttämällä tämän infrastruktuuria luomiseen käytetyt tiedot. Niiden avulla asentaa lisää, Määritä DNS, metsää luominen ja määrittäminen replikoinnin-sivustojen metsää määrittäminen.

### <a name="cloud-components"></a>Cloud osat

Nämä osat muodostavat tämän arkkitehtuuri core. [**Parametrien/azure**] [ azure-folder] kansio sisältää seuraavat parametrin tiedostojen määrittäminen komponentit:

- ** [virtualNetwork.parameters.json][vnet-parameters]**. Tämä tiedosto määrittää rakenteen VNet VMs ja muita osia pilveen. Se sisältää asetuksia, kuten nimi, osoitetilaa, aliverkosta ja minkä tahansa DNS-palvelimet tarvittavat osoitteet. Huomaa, että DNS-osoitteet Oheisen esimerkin viitata paikalliset DNS-palvelimet ja oletusarvon Azure DNS-palvelimen IP-osoitteet. Muokata näitä osoitteita viittaamaan omia DNS-asetukset, jos et käytä otoksen paikallisen ympäristön seuraavasti:

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **. Tämä tiedosto määrittää virtuaalilaitteiksi Lisää pilveen. Kokoonpanon koostuu kahdesta VMs. Sinun on muutettava järjestelmänvalvojan käyttäjänimeä ja salasanaa `virtualMachineSettings` osa ja voit halutessasi muuttaa AM kokoa vastaamaan toimialueen vaatimukset:

    Lisätietoja on artikkelissa [Laajentaminen Azure Active Directoryn][extending-ad-to-azure].

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- ** [Lisää-Lisää-toimialue-controller.parameters.json][add-adds-domain-controller-parameters]**. Tiedoston nimi sisältää asetuksia luomisen kestävät Lisää palvelimet CONTOSO-toimialueeseen. Tiedostossa käytetään mukautettuja laajennuksia, joka vahvistaa toimialueen ja lisätä siihen lisää-palvelimiin. Ellet luo muita Lisää servers (Tässä tapauksessa kannattaa lisätä ne `vms` matriisi), muuttaa niiden oletusarvoisen tai haluat luoda toimialueen eri nimellä, sinun ei tarvitse tehdä tämän tiedoston muokkaamiseen.

- ** [loadBalancer adfs.parameters.json] [loadbalancer-adfs-parameters] ** Tiedostossa on kaksi joukkoa määrityksiä. `virtualMachineSettings` Osa määrittää VMs, jotka isännöivät pilveen ADFS-palvelun. Oletusarvon mukaan komentosarja luo näistä VMs käytettävyys samat:

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    `loadBalancerSettings` Osassa on kuvaus kuormituksen näiden VMs. Kuormituksen välittää niitä VMs liikenteestä, joka näkyy porttiin 443 (HTTPS):

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs-klusterin-toimialue-join.parameters.json ] [ adfs-farm-domain-join-parameters] **. Tiedoston nimi sisältää ADFS-palvelimien lisääminen CONTOSO-toimialueeseen asetuksia. Haluat muokata tätä tiedostoa, jos olet luonut muut ADFS-palvelimet (Päivitä `vms` matriisin tällöin), tai olet muuttanut toimialuenimi.

- ** [gmsa.parameters.json][gmsa-parameters]**, ** [adfs-klusterin-first.parameters.json][adfs-farm-first-parameters]**, ja ** [adfs-klusterin-rest.parameters.json][adfs-farm-rest-parameters]**. Komentosarja käyttää asetuksia nämä tiedostot ADFS palvelinklusterin luomiseen. 

    *Gmsa.parameters.json* -tiedosto sisältää ADFS-palvelun käyttämä ryhmän hallitun palvelutilin asetuksia. Voit muokata tätä tiedostoa, jos haluat muuttaa tili tai toimialueen nimi.

    *Adfs-klusterin-first.parameters.json* -tiedosto sisältää ADFS palvelinklusterin luominen ja lisääminen ensimmäisen palvelimen tarvittavat tiedot. Jos olet muuttanut toimialueen tai ryhmän hallitun palvelutilin nimi, Päivitä tiedosto.

    *Adfs-klusterin-rest.parameters.json* -tiedostoa käytetään Lisää jäljellä olevat ADFS-palvelimet klusteriin. Uudelleen, jos olet muuttanut toimialueen tai ryhmän hallitun palvelutilin nimi, Päivitä tiedosto. Update `vms` matriisin, jos olet luonut muita ADFS-palvelimiin.

- ** [loadBalancer adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**. Tiedosto on samanlainen rakenteen ja sisällön *loadBalancer adfs.parameters.json* -tiedosto. Se sisältää etsimisen ADFS välityspalvelimet ja kuormituksen tiedot.

- ** [adfsproxy-klusterin-first.parameters.json][adfsproxy-farm-first-parameters]**, ja ** [adfsproxy-klusterin-rest.parameters.json][adfsproxy-farm-rest-parameters]**. Komentosarja käyttää näitä tiedostoja asetuksia ADFS-välityspalvelimen palvelinklusterin luomiseen. 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**. Tiedoston nimi sisältää luomiseen käytettävä Muodosta yhteys paikalliseen verkkoon pilvipalvelussa Azure VPN-yhdyskäytävän asetuksia. Muokkaa `sharedKey` arvo `connectionsSettings` osan vastaamaan paikallisen VPN-laite. Lisätietoja on artikkelissa [käyttöönoton Hybrid-verkoston arkkitehtuuri Azure ja paikallisen VPN][hybrid-azure-on-prem-vpn].

- ** [dmz private.parameters.json] [ dmz-private-parameters] ** ja ** [dmz public.parameters.json ] [ dmz-public-parameters] **. Nämä tiedostot Määritä saapuvan (julkinen) ja lähtevien (yksityinen), jotka muodostavat DMZ suojaaminen pilveen palvelimet VMs reunassa. Saat lisätietoja elementit ja niiden määritykset [käyttöönoton DMZ Azure ja Internetin välille][implementing-a-secure-hybrid-network-architecture-with-internet-access].

- ** [loadBalancer web.parameters-json][loadBalancer-web-parameters]**, ** [loadBalancer biz.parameters-json][loadBalancer-biz-parameters]**, ja ** [loadBalancer data.parameters-json][loadBalancer-data-parameters]**. Parametrit-tiedostot sisältävät access web ja business-tasoa AM mukaisesti ja Määritä kuormituksen tasoitusmääritykset kunkin tason. Nämä ovat VMs, joka isännöi web Apps-sovellusten ja tietokantojen ja suorita business työmääriä organisaation. Voit muokata kokojen ja määrä kunkin tason VMs tarpeen mukaan.

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**. Tiedoston nimi sisältää tekstin valinta ja hallinta VMs määrityskohde. Vain on mahdollista saada kirjautuminen ja järjestelmänvalvojan oikeudet web, business ja tietojen tasoa VMs tekstin-ruutuun. Oletusarvon mukaan komentosarja luo yksittäinen *Standard_DS1_v2* AM, mutta voit muokata luoda suurentaa tai muita VMs, jos hallinta työmäärää on todennäköisesti merkittävä tiedosto.

## <a name="solution-deployment"></a>Ratkaisun käyttöönotto

Ratkaisu oletetaan, että seuraavat edellytykset:

- Sinulla on aiemmin Azure tilaus, jossa voit luoda resurssiryhmiä.

- Olet ladannut ja asentanut PowerShellin Azure uusin versio. Katso [tähän] [ azure-powershell-download] ohjeita.

Voit suorittaa komentosarjan, joka ottaa ratkaisun käyttöön seuraavasti:

1. Siirrä helposti kansioon paikalliseen tietokoneeseen ja luo seuraavat alikansiot:

    - Komentosarjojen

    - Komentosarjojen ja parametrit

    - Komentosarjojen/parametrit/paikallisesti käytettävät versiot

    - Komentosarjojen/parametrit/Azure

2. Lataa [Käyttöönotto ReferenceArchitecture.ps1] [ solution-script] tiedosto komentosarjat-kansioon

3. Lataa [Parametrit/paikallisesti käytettävät versiot] sisällön[ on-premises-folder] kansio komentosarjojen/parametrit/paikallisesti käytettävät versiot kansioon:

4. Lataa [Parametrit/azure] sisällön[ azure-folder] kansio komentosarjojen/parametrit/Azure-kansioon.

5. Muokkaa komentosarjat-kansiossa käyttöönotto ReferenceArchitecture.ps1-tiedostoa ja muuta seuraavat viivoja ja määritä resurssiryhmiä, jotka on luotu tai käyttää luoma komentosarja resurssit:

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. Muokkaa parametrin tiedostoja komentosarjojen/parametrit/paikallisesti käytettävät versiot ja komentosarjojen/parametrit/Azure-kansioista. Päivitä resurssin ryhmässä viittaukset nämä tiedostot muuttujien käyttöönotto ReferenceArchitecture.ps1-tiedostoon liitetty resurssiryhmien nimet. Seuraavassa taulukossa on kuvattu, parametritiedostot viittaus mihin resurssiryhmä. Huomaa, että *ra-adfs-työmäärää-rg*, *ra-adfs-suojaus-rg*, *ra-adfs-Lisää-rg*, *ra-adfs-adfs-rg*ja *ra-adfs-välityspalvelimen-rg* ryhmiä voi käyttää vain PowerShell-komentosarjaa ja ilmene parametri-tiedostoja.

  	|Resurssiryhmä|Parametrin tiedostot|
  	|--------------|--------------|
  	|Ra-adfs-paikallisesti käytettävät versiot-rg|parameters\onpremise\connection.parameters.JSON<br /> parameters\onpremise\virtualMachines-adds.parameters.JSON<br />parameters\onpremise\virtualNetwork-adds-DNS.parameters.JSON<br />parameters\onpremise\virtualNetwork.parameters.JSON<br />parameters\onpremise\virtualNetworkGateway.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON
  	|Ra-adfs-verkko-rg|parameters\onpremise\connection.parameters.JSON<br />parameters\azure\dmz-Private.parameters.JSON<br />parameters\azure\dmz-public.parameters.JSON<br />parameters\azure\loadBalancer-ADFS.parameters.JSON<br />parameters\azure\loadBalancer-adfsproxy.parameters.JSON<br />parameters\azure\loadBalancer-Biz.parameters.JSON<br />parameters\azure\loadBalancer-Data.parameters.JSON<br />parameters\azure\loadBalancer-Web.parameters.JSON<br />parameters\azure\virtualMachines-adds.parameters.JSON<br />parameters\azure\virtualMachines-Mgmt.parameters.JSON<br />parameters\azure\virtualNetwork-with-OnPremise-and-Azure-DNS.parameters.JSON<br />parameters\azure\virtualNetwork.parameters.JSON<br />parameters\azure\virtualNetworkGateway.parameters.JSON (*kaksi*)

    Lisäksi paikalliset kokoonpanon määrittäminen ja cloud osat-ratkaisun osien [ratkaisun osien]-kohdassa kuvatulla tavalla.

7. Avaa PowerShellin Azure-ikkuna, komentosarjat-kansioon ja suorita seuraava komento:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    Korvaa `<subscription id>` käyttämällä Azure tilauksen.

    Saat `<location>`, Määritä Azure alue, kuten `eastus` tai `westus`.

    `<mode>` Parametri voi olla jokin seuraavista arvoista:

    - `Onpremise`, luomiseen Simuloitu paikalliseen ympäristöön.

    - `Infrastructure`, luomiseen VNet infrastruktuuri ja siirtyä ruutuun pilveen.

    - `CreateVpn`, voivat laatia Azure virtual yhdyskäytävien ja muodosta yhteys paikalliseen verkkoon.

    - `AzureADDS`, muodostaa visuaalisessa muodossa Lisää palvelinten VMs Active Directory käyttöön näiden VMs ja luoda toimialueen pilveen.

    - `AdfsVm`Luo ADFS-VMs ja liittää ne pilveen toimialueeseen.

    - `ProxyVm`voivat laatia ADFS-välityspalvelimen VMs ja liittää ne pilveen toimialueeseen.

    - `Prepare`, joka suorittaa kaikki edellä mainitut tehtävät. **Tämä on suositeltavaa vaihtoehtoa, jos kokoat kokonaan uuden käyttöönotto ja sinulla ei ole olemassa olevan paikallisen-infrastruktuurin.**

    >[AZURE.NOTE] Voit lähettää arviointisivustopyynnön myös suorittamalla komentosarja kanssa `<mode>` parametri `Workload` web, business- ja tietojen taso VMs ja verkon luomiseen. Tätä asetusta ei ole osana `Prepare` tilassa.

    Jos käytössäsi `Prepare` vaihtoehto komentosarja kestää useita tunteja ja lopettaa ilmoitus *valmistelu on valmis. Asenna varmenne ADFS ja välityspalvelimen VMs.*

8.  Siirry-ruutuun (*ra-adfs-hallintoraportointi-vm1* *ra-adfs-suojaus-rg* -ryhmässä) uudelleen sallimaan sen DNS-asetukset tulevat voimaan.

9.  [Hanki SSL-varmenne ADFS varten] [ adfs_certificates] ja tämän varmenteen asentaminen ADFS-VMs. Huomaa, että voit muodostaa yhteyden ADFS-VMs kautta tekstin-ruutuun. IP-osoitteiden on *10.0.5.4* ja *10.0.5.5*. Oletuskäyttäjänimi on *contoso\testuser* salasanalla *AweSome@PW*.

    >[AZURE.NOTE] Kommenttien käyttöönotto ReferenceArchitecture.ps1 komentosarja antaa tässä vaiheessa yksityiskohtaisia ohjeita voi luoda itse allekirjoitetun Testivarmenteen ja myöntäjä avulla `makecert` komento. Älä käytä makecert tuotantoympäristössä varmenteita.

10. Suorita seuraavat Powershell-komentoa määrittäminen ADFS palvelinklusterin:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. Siirry tekstin-ruutuun *https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm* ADFS: N asennuksen (näyttöön saattaa tulla varmenteen varoituksen, jossa voit ohittaa tämän testin). Varmista, että Contoso Corporation kirjautumissivulla näyttää. Kirjaudu *contoso\testuser* salasanalla *AweS0me@PW*.

12. SSL-varmenteen asentaminen ADFS-välityspalvelimen VMs. IP-osoitteiden on *10.0.6.4* ja *10.0.6.5*.

13. Suorita seuraavat Powershell-komennolla voit määrittää ensimmäisen ADFS-välityspalvelimen:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. Noudattamalla näkyvissä komentosarja ohjeita ensimmäisen ADFS-välityspalvelimen asennuksen testaaminen.

15. Suorita seuraavat Powershell-komentoa toisen ensimmäisen ADFS-välityspalvelimen määrittäminen:

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. Noudattamalla näkyvissä komentosarja ohjeita valmis ADFS-välityspalvelimen määritysten testaaminen.

## <a name="next-steps"></a>Seuraavat vaiheet

- [Azure Active Directory][aad].

- [Azure Active Directory-B2C][aadb2c].

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "Suojattu hybrid verkoston arkkitehtuuri Active Directory-hakemistosta"
