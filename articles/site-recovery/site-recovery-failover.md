<properties
    pageTitle="Sivuston palauttaminen automaattisesti | Microsoft Azure" 
    description="Azure palauttaminen koordinoi replikoinnin, automaattisesti ja näennäiskoneiden ja fyysinen palvelimiin. Lisätietoja Azure tai toissijainen palvelinkeskuksen automaattisesti." 
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
    ms.date="10/05/2016" 
    ms.author="raynew"/>

# <a name="failover-in-site-recovery"></a>Sivuston palauttaminen automaattisesti

Azure palauttaminen-palvelun osaltaan mukaan orchestrating replikoinnin, automaattisesti ja näennäiskoneiden ja fyysiset palvelimet yrityksen liiketoiminnan jatkuvuuden ja tietojen palauttaminen (BCDR) määrittäminen. Koneet voi replikoida Azure tai toissijaisen paikallisen tietokeskuksen. Lue pikaisesti [Azure palauttaminen ominaisuudet?](site-recovery-overview.md)

## <a name="overview"></a>Yleiskatsaus

Tässä artikkelissa on tietoja ja ohjeet palauttaminen (yli ja puuttuessa takaisin kaatuvat) näennäiskoneiden ja fyysinen palvelimissa, jotka on suojattu palauttaminen. 

Kirjaa kommentteja tai kysymyksiä alaosassa on tämän artikkelin tai [Azure palautus Services keskustelupalstalle](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="types-of-failover"></a>Automaattisesti tyyppisiä

Kun suojaus on käytössä näennäiskoneiden ja fyysiset palvelimet ja ne on replikoiminen voit suorittaa failovers tarpeen vaatiessa. Sivuston palauttaminen tukee useita automaattisesti tyyppisiä.

**Automaattisesti** | **Milloin haluat suorittaa** | **Tiedot** | **Prosessi**
---|---|---|---
**Testaa automaattisesti** | Vahvista replikoinnin strategia tai suorittaa tietojen palauttaminen-alirakenne suorittaminen | Ei tietojen menettämisen tai käyttökatkot.<br/><br/>Replikointia ei ole vaikutusta<br/><br/>Ei ole vaikutusta tuotannon-ympäristöön | Käynnistä vikasietotilaa<br/><br/>Määritä, miten testi koneet yhdistetään verkkoihin automaattisesti jälkeen<br/><br/>Seurata edistymistä **työt** -välilehti. Testaa luodaan ja Käynnistä toissijainen sijainnissa<br/><br/>Azure - muodostaa yhteyttä tietokoneeseen Azure-portaalissa<br/><br/>Toissijaisen sivuston - käyttää samaa host ja cloud tietokoneen<br/><br/>Tee testaus ja testaa automaattisesti asetukset automaattisesti Puhdista.
**Suunniteltu automaattisesti** | Suorita yhteensopivuuden ehtojen mukaan<br/><br/>Suorita suunniteltu ylläpito<br/><br/>Suorita epäonnistuu pitämään työmääriä käyttöjärjestelmä tunnetut katkokset - esimerkiksi odotettu power-virhe tai vakavia sää tietojen päällä<br/><br/>Suorita tuntisesta-ensisijainen ja toissijainen automaattisesti jälkeen | Ei tietojen menetystä<br/><br/>Käyttökatkot on syntynyt aikana noutaa sen toissijainen sijaintiin ja valitse ensisijainen virtuaalikoneen Sammuta kuluvaa aikaa.<br/><br/>Näennäiskoneiden ovat vaikutus kohde koneet tulee lähde koneet jälkeen automaattisesti. | Käynnistä vikasietotilaa<br/><br/>Seurata edistymistä **työt** -välilehti. Tietolähteen koneet Sammuta<br/><br/>Replikan koneet Käynnistä toissijaisen sijainnissa<br/><br/>Azure - yhteyden muodostaminen replikan koneen Azure-portaalissa<br/><br/>Toissijaisen sivuston - käyttää tietokoneen saman isännän ja samaa pilvessä<br/><br/>Vahvista vikasietotilaa
**Suunnittelematon automaattisesti** | Suorita tällaista automaattisesti uudelleenaktivointi tavalla, kun ensisijainen sivuston on ei käytettävissä odottamattomia tapahtuma, kuten käyttökatkosta tai virus hyökkäyksen vuoksi <br/><br/> Voit avata suunnittelematon automaattisesti voidaan toteuttaa, vaikka ensisijainen ei ole käytettävissä. | Tietojen menetyksen määräytyy korkojakso Replikointiasetukset<br/><br/>Tiedot ovat ajan tasalla se synkronoitiin viimeksi mukaisesti | Käynnistä vikasietotilaa<br/><br/>Seurata edistymistä **työt** -välilehti. Voit myös kokeilla näennäiskoneiden Sammuta ja synkronoida uusimmat tiedot<br/><br/>Replikan koneet Käynnistä toissijainen sijainnissa<br/><br/>Azure - yhteyden muodostaminen replikan koneen Azure-portaalissa<br/><br/>Toissijaisen sivuston käyttöoikeuden koneen saman isännän ja samaa pilvessä<br/><br/>Vahvista vikasietotilaa


Tiedostotyypit, joita tuetaan failovers määräytyvät käyttöönoton käyttämässäsi skenaariossa.

**Automaattisesti suunta** | **Testaa automaattisesti** | **Suunniteltu automaattisesti** | **Suunnittelematon automaattisesti**
---|---|---|---
VMM ensisijaisessa toissijainen VMM-sivustoon | Tuettu | Tuettu | Tuettu 
Toissijainen VMM sivuston ensisijainen VMM-sivustoon | Tuettu | Tuettu | Tuettu
Cloud pilvipalveluun (yksi VMM server) |  Tuettu | Tuettu | Tuettu
Azure VMM-sivusto | Tuettu | Tuettu | Tuettu 
Azure VMM-sivustoon | Ei tueta | Tuettu | Ei tueta 
Azure Hyper-V-sivusto | Tuettu | Tuettu | Tuettu
Azure Hyper-V-sivustoon | Ei tueta | Tuettu | Ei tueta
Azure VMware-sivusto | Tuettu (parannettu vaihtoehto)<br/><br/> Ei tueta (vanha vaihtoehto) |Ei tueta | Tuettu
Fyysinen Azure-palvelimeen | Tuettu (parannettu vaihtoehto)<br/><br/> Ei tueta (vanha vaihtoehto) | Ei tueta | Tuettu

## <a name="failover-and-failback"></a>Automaattisesti ja tuntisesta

Asiakas ei päälle toissijainen paikallisen sivuston tai Azuren näennäiskoneiden käyttöönoton mukaan. Azure virtuaalikoneen luodaan laitteeseen, joka ei Azure päälle. Voi epäonnistua virtual yhteen tietokoneeseen tai fyysistä palvelinta tai palautus-suunnitelma. Yksi koostuu palautus suunnitelmien tai Lisää vyöhykkeen ryhmiä, jotka sisältävät suojatun näennäiskoneiden tai fyysiset palvelimet. Niitä käytetään, orchestrate automaattisesti useita yhdistelmiä, jotka epäonnistua päälle ryhmän mukaan ne ovat. [Lue lisää](site-recovery-create-recovery-plans.md) palautus-palvelupakettia. 

Kun automaattisesti päättää ja tietokoneesi ovat käytön toissijainen sijainnissa Huomaa, että:

- Jos olet epäonnistui päälle Azure, kun automaattisesti koneet eivät ole suojattu tai replikoiminen ensisijaisen tai toissijaisen sijainti. Azure näennäiskoneiden on tallennettu geo replikoida tallennustilaan, joka sisältää vikasietoisuudelle, mutta ei ole sallittuja.
- Jos suunnittelematon automaattisesti toissijaisen sivuston, kun automaattisesti koneet toissijainen sijainnissa ei ole suojattu tai replikoiminen.
- Jos suunniteltu automaattisesti toissijaisen sivuston automaattisesti koneet toissijaisen sijainnissa jälkeen on suojattu.
 

### <a name="failback-from-secondary-site"></a>Tuntisesta toissijaisen-sivustosta

Ensisijaisen ja toissijaisen sivuston tuntisesta käyttää samoja ohjeita kuin siirtyy toissijaisen peräisin automaattisesti. Kun vikasietotilaa kuin ensisijaisen toissijainen palvelinkeskukseen on vahvistettu ja valmis, voit aloittaa käänteisen replikoinnin kun ensisijainen sivustosi on käytettävissä. Käänteinen replikoinnin käynnistää replikoinnin – toissijaisen sivuston ensisijainen synkronoimalla delta tiedot. Käänteinen replikoinnin yhdistää näennäiskoneiden suojattu tila, mutta toissijaisen palvelinkeskuksen on edelleen aktiivinen sijainti. Siirrä ensisijainen sivuston aktiivinen kohtaan käynnistät suunnitellun automaattisesti-toissijaista perus-toiseen käänteisessä replikoinnin perään.

### <a name="failback-from-azure"></a>Azuren tuntisesta

Jos olet epäonnistui päälle, Azure oman näennäiskoneiden on suojattu näennäiskoneiden Azure vikasietoisuudelle-ominaisuuksia. Jotta alkuperäinen ensisijaisessa aktiivinen kohtaan suoritat suunnitellun automaattisesti. Takaisin sen alkuperäiseen sijaintiin tai vaihtoehtoisen sijainnin saattaa epäonnistua, jos alkuperäisen sivuston ei ole käytettävissä. Aloita uudelleen tuntisesta ensisijainen sijainti replikoiminen käynnistät käänteisen replikoinnin.

### <a name="failover-considerations"></a>Automaattisesti huomioon otettavia seikkoja

- **IP-osoitteen automaattisesti jälkeen**– oletusarvon mukaan epäonnistunutta päälle koneen on eri kuin lähdetietokoneen IP-osoite. Jos haluat säilyttää saman IP-osoite-kohdassa: 
    - **Toissijaisen**– Jos olet kaatuvat päälle toissijainen sivustoon ja haluat säilyttää IP address [Lue](http://blogs.technet.com/b/scvmm/archive/2014/04/04/retaining-ip-address-after-failover-using-hyper-v-recovery-manager.aspx) tämän artikkelin. Huomaa, että voit säilyttää julkiseen IP-osoite, jos Palveluntarjoajasi tukee sitä.
    - **Azure**– Jos olet kaatuvat päälle Azure, voit määrittää IP-osoite, jolle haluat määrittää virtuaalikoneen ominaisuuksien **määrittäminen** -välilehdessä. Ei voi säilyttää julkisen IP-osoitteen automaattisesti Azure jälkeen. Voit säilyttää RFC 1918 osoite välilyöntejä, joita käytetään sisäisiä osoitteita.
- **Osittainen automaattisesti**, jos haluat epäonnistua sivuston osan päälle, vaan koko sivuston Huomaa, että: 
    - **Toinen**, jos haluat muodostaa yhteys ensisijainen sivustoon ensisijaisessa toissijainen sivustoon osa epäonnistuu, yhteysvirhe sovellusten toissijaisen sivuston ensisijainen sivustossa infrastruktuurin osien päälle sivusto sivusto VPN-yhteyden avulla. Jos koko aliverkon epäonnistuu kautta voit säilyttää virtuaalikoneen IP-osoite. Jos epäonnistuvat osittaisen aliverkon päälle ei voi säilyttää virtuaalikoneen IP-osoite, koska aliverkosta ei voi jakaa sivustojen välinen.
    - **Azure**— Jos epäonnistua Azure osittaisen sivuston päälle ja haluat muodostaa yhteys ensisijainen sivustoon, voit käyttää sivuston sivuston VPN muodostaa epäonnistunutta infrastruktuurin osien ensisijainen sivustossa Azure-sovelluksen kautta. Huomaa, että jos koko aliverkon-epäonnistuu päälle, jonka voit säilyttää virtuaalikoneen IP-osoite. Jos epäonnistuvat osittaisen aliverkon päälle ei voi säilyttää virtuaalikoneen IP-osoite, koska aliverkosta ei voi jakaa sivustojen välinen.
 
- **Kirjain**, jos haluat säilyttää näennäiskoneiden-kirjain automaattisesti jälkeen voit määrittää virtuaalikoneen SAN-käytännön **käyttöön**. Virtuaalikoneen levyjen sisältyvät online automaattisesti. [Lue lisää](https://technet.microsoft.com/library/gg252636.aspx).
- **Reitin asiakkaan pyyntöihin**– palauttaminen toimii Azure liikenteen hallinta reitin asiakkaan pyyntöihin sovelluksen jälkeen automaattisesti.




## <a name="run-a-test-failover"></a>Suorita testi automaattisesti

Kun suoritat testin automaattisesti sinua pyydetään valitsemaan verkkoasetukset testi replikan tietokoneissa. Sinulla on useita asetuksia.  

**Testaa automaattisesti-vaihtoehto** | **Kuvaus** | **Automaattisesti-valintaruutu** | **Tiedot**
---|---|---|---
**Ei onnistu Azure kautta – ilman verkossa** | Älä valitse kohde Azure verkossa | Automaattisesti tarkistukset, jotka testata virtuaalikoneen käynnistyy odotetulla tavalla Azure | Kaikki Palauta suunnitelma testi näennäiskoneiden on lisätty yhden pilvipalvelussa, ja voit muodostaa yhteyden toistensa<br/><br/>Tietokoneet eivät ole yhteydessä Azure verkkoon jälkeen automaattisesti.<br/><br/>Käyttäjät voivat muodostaa yhteyden testaaminen koneet julkiseen IP-osoite
**Ei onnistu Azure päälle, verkoston kanssa** | Valitse kohde Azure verkossa | Automaattisesti tarkistaa, että testi koneet yhteydessä verkkoon | Luo Azure verkkoon, joka on erillään Azure tuotannon verkon ja määritetty oikein infrastruktuuri replikoitua virtuaalikoneen toimii odotetulla tavalla.<br/><br/>Testaa virtuaalikoneen aliverkon perustuu aliverkon, joina epäonnistunutta virtuaalikoneen odotetaan muodostaa yhteyden, jos suunniteltu tai suunnittelematon vikasietotila ilmenee.
**Ei onnistu toissijainen VMM-sivuston kautta – ilman verkossa** | Älä valitse AM verkossa | Automaattisesti tarkistaa testi koneet luodaan.<br/><br/>Testaa virtuaalikoneen luodaan samaan isännän kuin isäntä, jossa replikan virtuaalikoneen on olemassa. Se ei ole lisätty pilveen, jossa replikan virtuaalikoneen sijaitsee. | <p>Epäonnistunutta koneen päälle ei liitettävä minkä tahansa verkkoon.<br/><br/>Koneen voidaan yhdistää AM verkkoon sen luomisen jälkeen
**Ei onnistu toissijaisen VMM-sivuston kautta – verkoston kanssa** | Valitse olemassa olevan AM-verkoston | Automaattisesti tarkistaa, että näennäiskoneiden on luotu | Testaa virtuaalikoneen luodaan samaan isännän kuin isäntä, jossa replikan virtuaalikoneen on olemassa. Se ei ole lisätty pilveen, jossa replikan virtuaalikoneen sijaitsee.<br/><br/>Luo AM verkkoon, joka on eristetty tuotannon verkosta<br/><br/>Jos käytät VLAN-pohjainen verkko on suositeltavaa luoda erilliset looginen verkko (ei käytössä tuotannon) VMM-tähän tarkoitukseen. Loogisen verkoston käytetään luomaan AM verkkojen testi automaattisesti varten.<br/><br/>Looginen verkko on oltava vähintään yksi kaikkien isännöinnin näennäiskoneiden Hyper-V palvelinten verkkosovittimien liity.<br/><br/>VLAN looginen verkkojen looginen verkko on lisätty verkkosivustot on oltava erillään.<br/><br/>Jos käytät Windows verkon Virtualization-pohjainen looginen verkko-Azure palauttaminen luo automaattisesti eristettyjen AM verkot.
**Ei onnistu toissijainen VMM-sivuston kautta – Luo verkko** | Tilapäinen testi verkon luodaan automaattisesti **Looginen verkko** -ja sen liittyvät verkkosivustot määrittämäsi asetuksen perusteella | Automaattisesti tarkistaa, että näennäiskoneiden on luotu | Käytä tätä vaihtoehtoa, jos palautus-palvelupaketti on käytössä useampi kuin yksi AM verkon. Jos käytät Windows verkon Virtualization verkkoja, tämä asetus, voit luoda AM verkot samoja asetuksia (aliverkosta ja IP-osoite jakavat) automaattisesti replikan virtuaalikoneen verkon. AM verkostojen leikkauskohdat siivotaan automaattisesti testi vikasietotilaa päätyttyä.</p><p>Testaa virtuaalikoneen luodaan samaan isännän kuin isäntä, jossa replikan virtuaalikoneen on olemassa. Se ei ole lisätty pilveen, jossa replikan virtuaalikoneen sijaitsee.

>[AZURE.NOTE] Sama kuin vastaanottaa milloin IP-osoite virtual machine aikana testi automaattisesti annettu IP-osoite jokaisen suunniteltu tai suunnittelematon automaattisesti, (aihe IP-osoite on käytettävissä testi automaattisesti verkostossa. Jos sama IP-osoite ei ole käytettävissä testi automaattisesti verkon virtuaalikoneen saavat toisen testin automaattisesti verkon käytettävissä IP-osoite.



### <a name="run-a-test-failover-from-on-premises-to-azure"></a>Testaa automaattisesti pitäminen paikallisen Azure

Tässä kuvataan, miten Suorita testi-automaattisesti palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhteen tietokoneeseen vikasietotilaa **näennäiskoneiden** -välilehdessä.

1. Valitse **Palautus-Palvelupaketit** > *recoveryplan_name*. Valitse **automaattisesti** > **testi automaattisesti**.
2. Määritä **Testin automaattisesti Vahvista** -sivulla miten replikan koneet yhdistetään Azure verkon jälkeen automaattisesti.
3. Jos olet kaatuvat päälle, Azure ja salauksen on käytössä pilveen, valitse **Salausavaimen** varmenne, jota on myönnetty otit salauksen tarjoajan asennuksen aikana. 
4. Automaattisesti edistymisen seuraaminen **työt** -välilehden. Sinun pitäisi nähdä testi replikan koneen Azure-portaalissa.
5. Voit käyttää replikan koneet Azure-tietokannassa Valmistele paikallisen sivuston RDP-yhteys voit virtuaalikoneen. portti 3389 on oltava käynnissä virtuaalikoneen päätepisteen.
5. Kun olet valmis, kun vikasietotilaa **valmiina testaaminen** vaiheeseen, valitse **Koko testin** loppuun.
5. **Muistiinpanojen** tallentaminen ja Tallenna testi vikasietotilaa liittyvät huomautukset.
8. Valitse automaattisesti ajoitettuina testiympäristössä **testi vikasietotilaa on valmis** . Kun tämä on täydellinen testi vikasietotilaa näkyvät C**omplete** tila.

> [AZURE.NOTE] Jos testi automaattisesti yhä enemmän kuin kaksi viikkoa se valmis voimassa. Kaikki osien ja luo automaattisesti aikana testi vikasietotilaa näennäiskoneiden poistetaan.
  

### <a name="run-a-test-failover-from-a-primary-on-premises-site-to-a-secondary-on-premises-site"></a>Suorita testi automaattisesti ensisijainen paikallisen sivuston toissijainen paikallisen sivuston

Sinun on suoritettava useita Suorita testi automaattisesti, kuten toimialueen ohjauskoneen kopiointi ja siten testi DHCP ja DNS-palvelimet testiympäristössä. Voit tehdä tämän usealla tavalla:

- Jos haluat suorittaa käyttämällä olemassa olevan verkoston testi-automaattisesti, valmistella Active Directory, DHCP ja DNS verkkoon.
- Jos haluat suorittaa ohjelmassa toiminnolla Luo AM verkot automaattisesti testi-automaattisesti, Lisää manuaalinen vaihe ennen ryhmän 1 aiot käyttää testi vikasietotilaa ja lisää sitten infrastruktuurin resurssit automaattisesti luotu verkon ennen kuin suoritat testin keskeneräiset palautus-suunnitelma.

#### <a name="things-to-note"></a>Huomioita

- Kun replikoiminen toissijainen sivustoon, replikan tietokoneen käyttämän verkon tyypin ei tarvitse vastaa loogisen testin automaattisesti käytettävät verkon, mutta jotkin kombinaatioiden ei ehkä toimi. Jos se on käytössä DHCP ja VLAN perustuva eristyksen, se AM verkosta ei tarvitse staattinen IP-osoite resurssivarantoon. Jotta käyttämällä Windows verkon Virtualization testi vikasietotilaa ei toimi, koska osoite ei ole jakavat ovat käytettävissä. Lisäksi testi automaattisesti eivät toimi, jos replikan verkko on n eristystaso ja testaa verkko on Windows verkon Virtualization. Tämä johtuu siitä ei eristystaso verkon ei ole tarvitaan Windows verkon Virtualization verkon aliverkosta.
- Mitä replikan näennäiskoneiden muodostanut yhteyden tapa yhdistetty AM verkkojen jälkeen automaattisesti riippuu siitä, miten AM-verkko on määritetty VMM konsolissa:
    - **AM verkon määritetty ilman eristystaso tai VLAN eristystaso**— Jos DHCP AM-verkko on määritetty, replikan virtuaalikoneen yhdistetään käyttämällä asetuksia, jotka on määritetty liittyvät looginen verkon verkon sivuston VLAN-tunnus. Virtuaalikoneen saa käytettävissä luokaksi IP-osoitetta. Sinun ei tarvitse staattinen IP address resurssivarantoon määritetty kohde AM verkkoon. Jos staattinen IP-osoite resurssivarantoon käytetään AM verkon replikan virtuaalikoneen yhdistetään käyttämällä asetuksia, jotka on määritetty liittyvät looginen verkon verkon sivuston VLAN-tunnus. Virtuaalikoneen saavat IP-osoitetta käytetään varannon määritetty AM verkkoon. Jos staattinen IP-osoite resurssivarantoon ei ole määritetty kohde AM verkossa, IP-osoite kohdistus epäonnistuu. IP-osoite resurssivarantoon luodaanko lähde- ja kohdesivustojen VMM palvelimissa, jotka aiot käyttää suojaus ja palauttaminen.
    - **AM verkoston kanssa Windows verkon virtualization**— Jos AM verkko on määritetty kohde AM verkkoa varten on määritettävä staattinen resurssivarantoon asetusta, riippumatta siitä, onko lähteen AM verkko on määritetty käyttämään DHCP tai staattinen IP sähköpostiosoitteen resurssivarantoon. Jos määrität DHCP, VMM palvelimesta sellaisena toimia ja antaa varannon, joka on määritetty IP-osoitteen kohde AM verkossa. Jos lähde-palvelin on määritetty staattinen IP-osoite resurssivarannon käyttäminen, VMM palvelimesta varata olevilla IP-osoite. Kummassakin tapauksessa IP-osoite kohdistus epäonnistuu, jos staattinen IP-osoite resurssivarantoon ei ole määritetty.

#### <a name="run-test"></a>Suorita testi

Tässä kuvataan, miten Suorita testi-automaattisesti palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhden virtuaalikoneen tai fyysinen Serverin keskeneräiset **näennäiskoneiden** -välilehden.

1. Valitse **Palautus-Palvelupaketit** > *recoveryplan_name*. Valitse **automaattisesti** > **testi automaattisesti**.
2. **Testaa automaattisesti Vahvista** -sivulla Määritä kuinka näennäiskoneiden tulee olla yhdistetty verkkoihin jälkeen testi vikasietotilaa.
3. Automaattisesti edistymisen seuraaminen **työt** -välilehden. Kun vikasietotilaa saavuttaa** valmiina testaaminen** vaiheeseen, valitse Valmis testi vikasietotilaa **Valmis testata** .
4. Valitse **muistiinpanot** ja tallentaminen testi vikasietotilaa liittyvät huomautukset.
4. Tarkista sen jälkeen, kun se on valmis näennäiskoneiden käynnistäminen onnistuu.
5. Kun olet varmistanut, että näennäiskoneiden käynnistäminen onnistuu, Suorita testi vikasietotilaa tyhjentääksesi eristetyssä ympäristössä. Jos valitsit luodaan automaattisesti AM verkkojen, uudelleenjärjestäminen poistaa kaikki testi näennäiskoneiden ja testaa verkot.

> [AZURE.NOTE] Jos testi automaattisesti yhä enemmän kuin kaksi viikkoa se valmis voimassa. Kaikki osien ja luo automaattisesti aikana testi vikasietotilaa näennäiskoneiden poistetaan.


#### <a name="prepare-dhcp"></a>Valmistele DHCP

Jos näennäiskoneiden osallistuvat testi automaattisesti käytä DHCP, Testaa DHCP-palvelimen luodaanko erillään verkoston, joka on luotu testi automaattisesti varten.


### <a name="prepare-active-directory"></a>Active Directory valmisteleminen
Suorita testi-automaattisesti sovelluksen testaus on Active Directory-tuotantoympäristössä kopio testiympäristössä. Lisätietoja [testata automaattisesti huomioon otettavia seikkoja active Directory](site-recovery-active-directory.md#considerations-for-test-failover) -osassa kerrotaan. 


### <a name="prepare-dns"></a>Valmistele DNS

DNS-palvelin valmisteleminen testi vikasietotilaa seuraavasti:

- **DHCP**— Jos näennäiskoneiden DHCP, Testaa luokaksi päivittäminen DNS-testin IP-osoite. Jos käytät Windows verkon Virtualization verkkotyypin, VMM palvelimen toimii luokaksi. Tämän vuoksi DNS IP-osoite voi päivittää testi automaattisesti verkossa. Tässä tapauksessa näennäiskoneiden Rekisteröi itse asiaa DNS-palvelimeen.
- **Staattinen osoite**— Jos näennäiskoneiden staattinen IP-osoite, Testaa DNS-palvelimen IP-osoitteen päivitetty testi automaattisesti verkossa. Voit joutua päivittämään DNS testi näennäiskoneiden IP-osoite. Voit käyttää seuraavaa komentosarjaa otoksen tätä varten: 

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-planned-failover-primary-to-secondary"></a>Suorita suunnitellun automaattisesti (ensisijainen ja toissijainen)

 Tässä kuvataan, miten suorittamiseen suunnitellun automaattisesti palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhteen virtual tietokoneeseen vikasietotilaa **näennäiskoneiden** -välilehdessä.

1. Ennen kuin aloitat, varmista, että kaikki haluamasi epäonnistuu päälle näennäiskoneiden suorittanut ensimmäisen replikoinnin.
2. Valitse **Palautus-Palvelupaketit** > *recoveryplan_name*. Valitse **automaattisesti** > **suunniteltu automaattisesti**. 
3. Valitse **Vahvista suunniteltu automaattisesti **-sivulla lähde- ja sijainnit. Huomautus automaattisesti suunta.

    - Jos kaikki virtuaalikoneen-palvelimet sijaitsevat lähde- tai kohdeluettelon sijainti edellisessä failovers toimi odotetulla tavalla, automaattisesti suunta tiedot koskevat vain tiedot. 
    - Jos näennäiskoneiden on aktiivinen lähde- ja kohdesivustojen sijainnit, **Muuta suunta** -painike tulee näkyviin. Tällä painikkeella voit muuttaa ja määrittää siihen suuntaan, johon vikasietotilaa suoritetaan.

5. Jos olet kaatuvat päälle, Azure ja salauksen on käytössä pilveen, valitse **Salausavaimen** varmenne, jota on myönnetty otit salauksen VMM palvelimessa tarjoajan asennuksen aikana. 
6. Kun suunnitellun automaattisesti alkaa ensimmäiseksi on varmistaa ei tietojen menetystä näennäiskoneiden sulkeutumaan. Voit seurata automaattisesti edistymistä **työt** -välilehden. Jos vikasietotilaa (joko virtual tietokoneeseen tai komentosarjan, joka sisältyy palautus-palvelupaketti), ilmenee virhe suunnitellun vikasietotilaa palautus-palvelupaketin lopettaa. Voit aloittaa vikasietotilaa uudelleen.
8. Replikan näennäiskoneiden luomisen jälkeen ne ovat Vahvista Odottaa-tilassa. Valitse **Vahvista** vahvistusta vikasietotilaa. 
9. Kun replikointi on valmiit näennäiskoneiden käynnistyksen toissijaisen sijainnissa. 

## <a name="run-an-unplanned-failover"></a>Suorita suunnittelematon automaattisesti

Tässä kuvataan suorittaminen suunnittelematon automaattisesti, palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhden virtuaalikoneen tai fyysinen Serverin keskeneräiset **näennäiskoneiden** -välilehden.

1. Valitse **Palautus-Palvelupaketit** > *recoveryplan_name*. Valitse **automaattisesti** > **suunnittelematon automaattisesti**. 
3. Valitse **Vahvista suunnittelematon automaattisesti **-sivulla lähde- ja sijainnit. Huomautus automaattisesti suunta.

    - Jos kaikki virtuaalikoneen-palvelimet sijaitsevat lähde- tai kohdeluettelon sijainti edellisessä failovers toimi odotetulla tavalla, automaattisesti suunta tiedot koskevat vain tiedot. 
    - Jos näennäiskoneiden on aktiivinen lähde- ja kohdesivustojen sijainnit, **Muuta suunta** -painike tulee näkyviin. Tällä painikkeella voit muuttaa ja määrittää siihen suuntaan, johon vikasietotilaa suoritetaan.

4. Jos olet kaatuvat päälle, Azure ja salauksen on käytössä pilveen, valitse **Salausavaimen** varmenne, jota on myönnetty otit salauksen VMM palvelimessa tarjoajan asennuksen aikana. 
5. Valitse **Sammuta näennäiskoneiden ja synkronoida uusimmat tiedot** voit määrittää, että sivuston palauttaminen yrittää suojatun näennäiskoneiden Sammuta ja synkronoida tiedot niin, että tiedot ovat uusimmat epäonnistuu päälle. Jos et valitse tätä asetusta tai yritä ei onnistu vikasietotilaa ovat uusimmat virtuaalikoneen käytettävissä palautuspiste.
6. Voit seurata automaattisesti edistymistä **työt** -välilehden. Huomaa, että vaikka suunnittelematon automaattisesti aikana ilmenee virheitä, palautus-suunnitelma suoritetaan ennen kuin se on valmis.
7. Kun vikasietotilaa, näennäiskoneiden on **Vahvista odottaa** -tilassa. Valitse **Vahvista** vahvistusta vikasietotilaa.
8. Jos määrität replikoinnin useita palautus pisteitä, muuta palautus-kohdassa voit valita käyttämään palautuspiste, joka ei ole uusimmat (uusin versio käytetään oletusarvon mukaan). Kun olet vahvistanut muita palautus pisteet poistetaan.
9. Replikoinnin päätyttyä näennäiskoneiden käynnistäminen ja toissijainen kohtaan käytössä. Mutta ne eivät ole suojattu tai replikoiminen. Kun ensisijainen sivusto on käytettävissä uudelleen samaan pohjana oleva infrastruktuuri, valitse Aloita käänteisen replikoinnin **Käänteisen replikoida** . Näin varmistat, että kaikki tiedot replikoida ensisijainen sivustoon ja että virtuaalikoneen on valmis automaattisesti uudelleen. Peruuttaa replikoinnin, kun suunnittelematon automaattisesti veloitetaan tiedonsiirto katseltavan. Siirron käyttää samaa menetelmää, joka on määritetty alkuperäinen Replikointiasetukset pilveen.

## <a name="failback-from-secondary-to-primary"></a>Valitse ensisijainen ja toissijainen tuntisesta

 Siirtyy toissijaiseen sijaintiin peräisin automaattisesti, kun replikoitua näennäiskoneiden ei ole suojattu palauttaminen ja toissijainen sijainti on nyt visuaalisessa muodossa ensisijainen. Näiden ohjeiden avulla voit takaisin alkuperäiseen ensisijainen sivustoon. Tässä kuvataan, miten suorittamiseen suunnitellun automaattisesti palautus-palvelupaketteihin. Vaihtoehtoisesti voit suorittaa yhteen virtual tietokoneeseen vikasietotilaa **näennäiskoneiden** -välilehdessä.

1. Valitse **Palautus-Palvelupaketit** > *recoveryplan_name*. Valitse **automaattisesti** > **suunniteltu automaattisesti**.
2. Valitse **Vahvista suunniteltu automaattisesti **-sivulla lähde- ja sijainnit. Huomautus automaattisesti suunta. Jos ensisijainen-keskeneräiset kuin toiminta ja kaikki näennäiskoneiden on toissijainen sijainnissa, tämä on tietoja vain.
3. Jos epäonnistui tuntemattomasta takaisin Azure-Valitse asetukset- **Tietojen synkronointi**:

    - **Synkronoi tietoja ennen automaattisesti (vain Synchonize delta muutokset)**— tämä vaihtoehto minimoi käyttökatkot näennäiskoneiden, kun se synkronoi ilman niitä suljetaan. Tekee seuraavat toimet:
        - Vaihe 1: Hakee virtuaalikoneen tilannevedoksen Azure ja kopioi paikallisen Hyper-V isäntään. Koneen säilyy Azure käynnissä.
        - Vaihe 2: Sulkee virtuaalikoneen Azure-tietokannassa, jotta yhdistettäviä uusia muutoksia hallita olemassa. Viimeiset muutokset siirretään paikallisen palvelimeen ja paikallisen virtuaalikoneen käynnistetään.
    

    - **Synkronoi tiedot automaattisesti vain aikana (koko lataaminen)**– Käytä tätä vaihtoehtoa, jos olet jo käytössä Azure pitkään. Tämä asetus on nopeampaa, koska Odotamme, että suurin osa levy on muutettu ja et halua käyttää aikaa tarkistussumma laskentaan. Lataa levyn suorittaa. Se on hyötyä myös silloin, kun paikallinen virtuaalikoneen on poistettu.
    
    > [AZURE.NOTE] Suosittelemme, että käytät tätä vaihtoehtoa, jos olet jo käytössä Azure jonkin aikaa (kuukausi tai enemmän) tai paikallinen AM on poistettu. Tämä asetus ei tarkistussumma minkä tahansa laskelmia varten.
    
5. Jos olet kaatuvat päälle, Azure ja salauksen on käytössä pilveen, valitse **Salausavaimen** varmenne, jota on myönnetty otit salauksen VMM palvelimessa tarjoajan asennuksen aikana. 
5. Viimeisen palautus-kohtaa käytetään oletusarvoisesti, mutta **Muuta palautus** -kohdassa voit määrittää eri palautus-kohtaa. 
6. Napsauta Käynnistä tuntisesta valintamerkkiä.  Voit seurata automaattisesti edistymistä **työt** -välilehden. 
7. f vaihtoehto, jos haluat synkronoida tiedot ennen vikasietotilaa, kun aloitustiedot synkronointi on valmis ja olet valmis-Azuren näennäiskoneiden Sammuta valittuna Valitse **työt**  >  <planned failover job name> **Valmiiksi automaattisesti**. Tämä Azure tietokoneen sammuttamisen, siirtää uusimmat muutokset paikallisen virtuaalikoneen ja käynnistää sen.
8. Voit nyt Kirjaudu virtuaalikoneen Vahvista se on käytettävissä odotetulla tavalla. 
9. Vahvista Odottava tila on virtuaalikoneen. Valitse **Vahvista** vahvistusta vikasietotilaa.
10. Suorittaminen tuntisesta Valitse nyt **Käänteinen replikoida** Aloita suojaaminen virtuaalikoneen ensisijainen-sivustossa.



## <a name="failback-to-an-alternate-location"></a>Tuntisesta vaihtoehtoiseen sijaintiin

Jos olet ottanut suojaus [Hyper-V sivuston ja Azure](site-recovery-hyper-v-site-to-azure.md) mahdollisuus tuntisesta azuren sinun on vaihtoehtoisen paikalliset sijainti. Tämä on kätevä, jos haluat määrittää uuden paikallisen laitteen. Seuraavassa on, kuinka se tehdään.

1. Jos olet määrittämässä uuden laitteen Asenna Windows Server 2012 R2 ja Hyper-V-roolin palvelimessa.
2. Luo VPN-valitsin, jossa on sama nimi, joka oli alkuperäisessä palvelimessa.
3. Valitse **Suojattu kohteet** -> **Suojaus-ryhmä**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> epäonnistua takaisin ja valitse **Suunniteltu automaattisesti**.
4. Valitse **Vahvista suunniteltu automaattisesti** **Luo paikallisen virtuaalikoneen Jos sitä ei ole**. 
5. Kirjoita **Isäntänimi** Valitse Hyper-V host palvelin uusi, johon haluat sijoittaa virtuaalikoneen.
6. Tietojen synkronointi on suositeltavaa, asetus on valittuna **synkronoi tietoja ennen vikasietotilaa**. Tämä minimoi käyttökatkot näennäiskoneiden, kun se synkronoi ilman niitä suljetaan. Tekee seuraavat toimet:

    - Vaihe 1: Hakee virtuaalikoneen tilannevedoksen Azure ja kopioi paikallisen Hyper-V isäntään. Koneen säilyy Azure käynnissä.
    - Vaihe 2: Sulkee virtuaalikoneen Azure-tietokannassa, jotta yhdistettäviä uusia muutoksia hallita olemassa. Viimeiset muutokset siirretään paikallisen palvelimeen ja paikallisen virtuaalikoneen käynnistetään.
    
7. Valitse Aloita vikasietotilaa (tuntisesta) valintamerkki.
8. Kun ensimmäinen synkronointi on valmis ja olet valmis Sammuta virtuaalikoneen Azure-tietokannassa, valitse **työt** > <planned failover job> > **Valmiiksi automaattisesti**. Tämä Azure tietokoneen sammuttamisen, siirtää uusimmat muutokset paikallisen virtuaalikoneen ja käynnistää sen.
9. Voit kirjautua paikallisen virtuaalikoneen tarkistamaan, että kaikki toimii odotetulla tavalla. Valitse **Vahvista** Viimeistele vikasietotilaa.
10. Valitse Aloita suojaaminen paikallisen virtuaalikoneen **Käänteinen replikoida** .

    >[AZURE.NOTE] Jos peruutat tuntisesta työn ollessa tietojen synkronointi vaihe, paikallisen AM on vioittuneet-tilaan. Tämä johtuu siitä tietojen synkronoinnin kopiot Azure AM uusimmat tiedot levyjen paikallinen tietojen levyille ja untill synkronointi on valmis, levyn tiedot eivät välttämättä ole yhdenmukaisia tilaan. Jos käytössä-on prem AM käynnistetään, kun tietojen synkronoinnin peruutetaan, se saattaa ei käynnisty. Käynnistää uudelleen automaattisesti suorittamiseen tietojen synkronoinnin.
 
