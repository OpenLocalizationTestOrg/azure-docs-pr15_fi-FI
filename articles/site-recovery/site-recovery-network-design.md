<properties
    pageTitle="Suunnittelet palauttaminen verkkoinfrastruktuuria | Microsoft Azure"
    description="Tässä artikkelissa käsitellään verkon suunnittelu Huomioitavaa Azure sivuston palauttaminen"
    services="site-recovery"
    documentationCenter=""
    authors="prateek9us"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="09/19/2016"
    ms.author="pratshar"/>

#  <a name="designing-your-network-infrastructure-for-disaster-recovery"></a>Palauttaa verkkoinfrastruktuuria suunnitteleminen

Tässä artikkelissa on IT-ammattilaisille, jotka ovat vastuussa suunnittelee, toteuttaminen, ja Liiketoiminnan jatkuvuus ja tietojen palauttaminen (BCDR) infrastruktuuri ja jotka haluavat hyödyntää Microsoft Azure sivuston Palauttamista voit tukea ja parantaa BCDR niiden palveluiden ohjataan. Tässä asiakirjassa kerrotaan System Center virtuaalikoneen hallinta server deployment, ammattilaisille ja laajennetun aliverkosta aliverkon automaattisesti ja haittoja käytännön huomioitavista asioista ja käytön rakennetta palauttaminen Microsoft Azure virtual sivustoihin.

## <a name="overview"></a>Yleiskatsaus

[Azure sivuston Palauttamista](https://azure.microsoft.com/services/site-recovery/) on Microsoft Azure-palvelu, orchestrates suojaus ja palautus virtualisoidussa sovellustesi liiketoiminnan jatkuvuuden tietojen palauttaminen (BCDR) tarpeisiin. Tämä asiakirja on tarkoitettu opas lukijaa suunnitteluprosessi-verkoissa keskitytään suunnittelee IP-alueita ja aliverkosta tietojen palauttaminen-sivustossa, kun replikoiminen näennäiskoneiden (VMs) palauttaminen käyttämällä.

Lisäksi tässä artikkelissa kerrotaan, miten palauttaminen mahdollistaa suunnittelee ja toteuttaa multisite virtual palvelinkeskukseen BCDR tukipalveluita testi tai tietojen aikaan.

Maailman, jossa kaikki odottaa 24/7 yhteyden on enemmän tärkeämpi koskaan, jos haluat säilyttää infrastruktuuri ja sovellusten ja käytössä. Liiketoiminnan jatkuvuus ja tietojen palauttaminen (BCDR) tarkoituksena on palauttaa epäonnistui osat, jotta organisaation nopeasti jatka työskentelyä tavalliseen. Kehittäminen varalle epätodennäköistä, devastating tapahtumat käsitellä on erittäin hankalaa. Tämä on on luontaisen vaikea korrelaatio tulevaisuudessa, erityisesti, miten se liittyy improbable tapahtumien ja antamaan riittäviä toimenpiteitä vastaan kauaskantoista catastrophes hyvin kustannukset. 

Tärkeitä BCDR suunnittelu-palautus aika tavoitteen (RTO) ja palautus pisteen tavoitteen (RPO) on määritettävä tietojen palauttaminen suunnitelma osana. Kun ilmenee ongelmia asiakkaan data Centerissä käyttämällä Azure sivuston palautus-asiakkaat voivat nopeasti (pieni RTO) tuoda verkossa sijaitsevat toissijainen tietokeskuksen tai Microsoft Azure pienin tietojen menettämisen (pieni RPO) ja niiden replikoitua näennäiskoneiden. 

Automaattisesti tehdään mahdollista ASR, joka alun perin kopioi nimettyjen näennäiskoneiden ensisijainen tietokeskuksen toissijainen tietokeskuksen tai Azure (toimintamallin) ja päivittää replikat säännöllisin väliajoin. Infrastruktuurin suunnitteluvaiheessa verkon suunnittelu olisi katsottava mahdolliset pullonkaula, jotka saattavat estää olet kokouksen yrityksestä RTO ja RPO tavoitteet.  

Kun järjestelmänvalvojat aiot tietojen palauttaminen ratkaisun käyttöönotto-yksi mieltään avaimen kysymyksiin on miten virtuaalikoneen olisi tavoitettavissa vikasietotilaa päättymisen jälkeen. ASR avulla voit valita, johon liitetään virtual machine jälkeen automaattisesti verkon järjestelmänvalvoja. Jos ensisijainen sivuston hallitsee VMM palvelimeen, valitse tämä saavutetaan käyttämällä verkon yhdistäminen. Saat lisätietoja [valmisteleminen verkon määritystä varten](site-recovery-network-mapping.md) .

Suunnitellessasi verkon palautus-sivuston järjestelmänvalvojan on kaksi vaihtoehtoa:

- Käytä eri IP-osoitealueita palautus sivustosta verkkoa. Tässä skenaariossa virtuaalikoneen jälkeen automaattisesti saavat uuden IP-osoitteen ja järjestelmänvalvojan on suoritettava DNS-päivitys. Lue lisätietoja siitä, miten voit tehdä DNS Päivitä [tähän](site-recovery-vmm-to-vmm.md#test-your-deployment) 
- Käytä samaa IP-osoitealueita palautus-sivustosta verkkoa. Tietyissä tilanteissa järjestelmänvalvojat Haluan säilyttää IP-osoitteet, joihin heillä on ensisijainen sivustossa vaikka vikasietotilaa. Tavallinen tilanne järjestelmänvalvojan on päivitettävä tiet IP-osoitteiden uuteen paikkaan. Mutta tilanteessa, jossa laajennetun VLAN otetaan käyttöön pääsivujen ja palautus-sivustojen välillä, säilyttämiseen näennäiskoneiden IP-osoitteet tulee houkuttelevien vaihtoehto. Pitäminen saman IP osoitteet yksinkertaistaa palauttaminen tehokkaasta poissa verkon liittyvät kirjaa automaattisesti vaiheet.


Kun järjestelmänvalvojat aiot tietojen palauttaminen ratkaisun käyttöönotto-jokin keskeisiä kysymyksiä niiden mielessä on miten sovellukset ovat tavoitettavissa vikasietotilaa päättymisen jälkeen. Nykyaikainen sovellusten riippuvat melkein aina jonkin verran, joten siirtämiseen palvelun sivustosta toiseen edustaa verkko hankala verkon. On kahdella pääasiallisella tavalla, ongelma on käsitellä tietojen palauttaminen ratkaisuja. Ensimmäinen menetelmä on pitämään kiinteä IP-osoitteet. Sovellusten kestää huolimatta siirtäminen services ja että fyysinen käyttäjien isännöintipalvelu palvelinten, heidän kanssaan IP-osoite-määritys uuteen sijaintiin. Toinen tapa liittyy muuttaminen kokonaan aikana haluamasi siirtymän palautetun sivuston IP-osoite. Kunkin lähestymistapa on useita käyttöönoton muunnoksia, johon on koottu alapuolella.

Suunnitellessasi verkon palautus-sivuston järjestelmänvalvojan on kaksi vaihtoehtoa:

## <a name="option-1-retain-ip-addresses"></a>Vaihtoehto 1: Säilytä IP-osoitteet 

Tietojen palauttaminen prosessin näkökulmasta käyttämällä kiinteiden IP osoitteet on helpoin tapa ottaa käyttöön, mutta se on mahdollinen haasteisiin, jotka käytännössä ansiosta vähiten suosittu tapa määrä. Azure palauttaminen mahdollistaa säilytetään kaikissa tilanteissa IP-osoitteet. Ennen kuin yksi päättää säilyttää IP-tarvittavat haluat näyttää antaa sen asettamatta automaattisesti ominaisuuksia rajoitukset. Anna meidän Määritä tekijöiden, jotka auttavat päätöksentekoa säilyttää IP-osoite vai ei. Tämä onnistuu kahdella tavalla, käyttämällä laajennetun aliverkon tai tekemällä koko aliverkon automaattisesti.

### <a name="stretched-subnet"></a>Laajennetun aliverkon

Tähän aliverkon soitetaan käytettävissä samanaikaisesti perus- ja DR sijainnit. Yksinkertainen ehdot Tällöin voit siirtää toisen sivuston palvelin ja sen (Layer 3) IP-määritys ja verkon reitittää liikenteen uuteen sijaintiin automaattisesti. Tämä on vähäpätöinen palvelimen näkökulmasta käsiteltävät, mutta se on haasteita määrä:

- Tason 2 (tietojen linkin layer)-näkökulmasta viestin laadittava verkko laitteet, jotka voivat hallita laajennetun VLAN, mutta tämä on tullut pienempi ongelma, sillä se on nyt käytettävissä. Toisen ja vaikeaa ongelma on venyttämällä VLAN mahdollisen virheen toimialueen on laajennettu molemmat sivustot jatkossa pääosin virheen yhden pisteen. Samalla, kun kyseessä on epätodennäköistä esiintymää, on tapahtunut lähetyksen myrsky alkanut, mutta ei voitu erillään. Olemme löytänyt erilaiset lausunnot viimeisen tästä ongelmasta ja tullut monta onnistuneen käyttöotot sekä "on aina toteuttaa tämä tekniikka".
- Laajennetun aliverkon ei ole mahdollista, jos käytössäsi on Microsoft Azure kuin DR sivusto.


### <a name="subnet-failover"></a>Aliverkon automaattisesti

On mahdollista toteuttamisesta aliverkon automaattisesti ilman venyttämällä aliverkon sivustoissa kuvattuun laajennetun aliverkon ratkaisu hyödyn. Tässä kaikki tietyn aliverkon olisi ole sivuston 1 tai 2, mutta ei molemmissa paikoissa samanaikaisesti. Säilyttää IP-osoitetilaa virhetilanteessa, on mahdollista valmistautua ohjelmallisesti reitittimen infrastruktuuri aliverkosta siirtäminen sivustosta toiseen. Automaattisesti tilanne aliverkosta siirtää liittyvät suojattu VMs. Tämän menetelmän tärkeimmät haitta on Jos sinun on siirrettävä koko aliverkon, joka voi olla OK, mutta se voi vaikuttaa automaattisesti rakeisuuden huomioon otettavia seikkoja. 

Tarkastellaan nyt, kuinka kuvitteellista yritys, Contoso-niminen on voit toistaa sen VMs palautuksen sijainti, kun epäonnistumista koko aliverkon päälle. Olemme ensin näyttää, miten Contoso on hallitsevan niiden aliverkosta aikana replikoiminen VMs kahden paikallisen sijainnit ja sitten käsittelemme aliverkon automaattisesti toiminta milloin [Azure käytetään tietojen palauttaminen sivuston](#failover-to-azure).

#### <a name="failover-to-a-secondary-on-premises-site"></a>Siirtyy toissijaisen paikallisen sivuston automaattisesti

Anna meidän Tarkista tilanne missä haluamme säilyttää kaikkien VMs IP- ja korvaava valmis aliverkon yhdessä. Ensisijainen sivustossa on aliverkon 192.168.1.0/24-sovellukset. Vikasietotilaa tapahtuu, kun kaikki tämän aliverkon osana olevat näennäiskoneiden epäonnistuu päälle palautus-sivustoon ja säilyttää niiden IP-osoitteet. Tiet on otettu muokattava vastaamaan siitä, että kaikki kuuluvat aliverkon 192.168.1.0/24 näennäiskoneiden on nyt siirretty palautus-sivustoon. 

Seuraavassa kuvassa ensisijaisessa ja palauttaminen sivuston, kolmas sivuston ja ensisijaisessa, ja kolmas sivuston ja palautus-sivuston tiet on otettu muokataan. 

Seuraavissa kuvissa näkyy aliverkosta ennen vikasietotilaa. Aliverkon 192.168.0.1/24 on aktiivinen ennen vikasietotilaa ensisijaisessa ja aktivoituu palautus-sivuston vikasietotilaa jälkeen 

![Ennen automaattisesti](./media/site-recovery-network-design/network-design2.png)

Ennen automaattisesti


Alla olevassa kuvassa näkyy verkkojen ja aliverkosta jälkeen automaattisesti.
    
![Kun olet automaattisesti](./media/site-recovery-network-design/network-design3.png)

Kun olet automaattisesti

Toissijainen sivustossa on paikallisen ja VMM palvelimen avulla hallita sitä sitten tiettyä virtual konetta suojaus on otettu käyttöön, kun ASR varata verkko resursseja seuraavista työnkulun mukaan:

- ASR varaa virtuaalikoneen kunkin verkkoliittymän IP-osoitetta käytetään varannon staattinen IP address määritetty asiaa verkossa System Center VMM jokaiselle esiintymälle.
- Jos järjestelmänvalvoja määrittää verkon samaa IP-osoite resurssivarantoon palautus-sivustossa, IP-osoite resurssivarantoon verkoston ensisijainen-sivustossa, kun kohdistaminen replikan virtuaalikoneen ASR IP-osoite varata sama IP-osoite, ensisijainen virtuaalikoneen.  IP on varattu VMM, mutta ei tue IP määritelty. Tue IP määritetään vain keskeneräiset ennen.

![Säilytä IP-osoite](./media/site-recovery-network-design/network-design4.png)
    
Kuva 5

Kuva 5 näkyy replikan virtuaalikoneen automaattisesti TCP/IP-asetukset (Hyper-V console). Nämä asetukset täyttää, ennen kuin virtuaalikoneen aloitetaan automaattisesti kun

Jos saman IP ei ole käytettävissä, ASR varata joitakin käytettävissä IP-osoitteen määritetty IP-osoite olevilla. 

Kun voit käyttää seuraavia Esimerkki komentosarja, joka on kohdistettu virtuaalikoneen IP vahvistamiseksi suojaus on käytössä AM. Sama IP tapauksessa määritetään automaattisesti IP ja myönnetyt AM automaattisesti, kun:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

>[AZURE.NOTE] Tilanne, jossa näennäiskoneiden käyttää DHCP-IP-osoitteiden hallinta on kokonaan ASR ohjausobjektin ulkopuolella. Järjestelmänvalvoja on varmistettava, että käsittelevä palauttaminen sivuston IP-osoitteiden luokaksi voi toimia samaan alueelta, joka on ensisijainen sivustossa.

#### <a name="failover-to-azure"></a>Azure automaattisesti

Azure sivuston Palauttamista avulla Microsoft Azure käytettävä tietojen palauttaminen sivuston oman näennäiskoneiden. Tässä tapauksessa sinun on käsitellä yksi Lisää rajoite. 

Tarkastellaan nyt nimeltä Yrityspankin kuvitteelliseen yritykseen, joiden on paikallisen infrastruktuurin isännöinnin niiden rivin yrityssovellusten tilanne ja niiden isännöit niiden mobile Azure-sovelluksia. Yrityspankin pankin VMs Azuren ja paikallisten palvelinten väliset yhteydet annetaan mukaan sivusto (S2S) näennäisen yksityisverkon (VPN). S2S VPN sallii Yrityspankin 's virtual verkon Azure pitää tiedostotunnistetta Yrityspankin käyttäjän paikalliseen verkkoon. S2S VPN käytössä tämän viestintä Yrityspankin reunan ja Azure virtual verkkoon. Nyt Yrityspankin haluaa ASR avulla voit toistaa sen käynnissä sen joten Azure toiminnoista. Tämä vaihtoehto vastaa Yrityspankin, joka haluaa taloudellisia DR-vaihtoehdon ja voi tallentaa tiedot julkisen cloud-ympäristössä tarpeisiin. Yrityspankin pitää käsitellä sovellukset ja määritykset, jotka riippuvat koodattu IP-osoitteet, näin ollen heillä on pyyntö säilyttää verkkosovelluksistaan IP-osoitteiden epäonnistumista päälle, Azure jälkeen.

Azure käynnissä resursseja on päättänyt Yrityspankin Määritä IP-osoitteiden IP-osoitealueita (172.16.1.0/24, 172.16.2.0/24).

Yrityspankin voi replikoida sen näennäiskoneiden Azure säilyttäen IP-osoitteiden Virtual Azure-verkko on luotava. Paikallisen verkoston tunniste on oltava siten, että sovellukset voivat automaattisesti paikallisen sivustosta Azure saumattomasti. Azure avulla voit lisätä virtual verkkoihin Azure luotu sivusto sivusto sekä pisteen sivuston VPN-yhteys. Sivuston sivuston yhteyden järjestäessäsi Azure verkon avulla voit reitittää liikenteen paikallisen sijainti (Azure soittaa sen paikallisen verkon) vain, jos IP-osoitealueita on erilainen kuin paikallisen IP-osoitealueita Azure tue Venytys aliverkosta.  Tämä tarkoittaa, että jos aliverkon 192.168.1.0/24 paikallisen, et voi lisääminen paikalliseen verkkoon 192.168.1.0/24 Azure verkossa. Tämä on normaalia Azure ei tunnista aliverkon ei ole aktiivinen VMs ja että aliverkon luodaan vain DR tarkoituksiin. Jos haluat reitittää oikein verkkoliikenteen ulos Azure verkon aliverkosta verkko-ja Paikallinen-verkko saa olla ristiriidassa. 

![Ennen aliverkon automaattisesti](./media/site-recovery-network-design/network-design7.png)

Ennen automaattisesti

Jotta Yrityspankin business niiden vaatimukset täyttävät annettava Toteuta seuraavat työnkulut:

- Luo muita verkko, anna sille uutiskirjeistä palautus verkkoon, jossa epäonnistui valittavia näennäiskoneiden luodaan.
- Voit varmistaa AM IP säilytetään jälkeen automaattisesti, siirry AM ominaisuuksien määrittäminen-välilehden Määritä AM on paikallisen saman IP ja valitse sitten Tallenna. Kun AM epäonnistui päälle, Azure palauttaminen määrittää annettu IP virtuaalikoneen. 

![Verkon ominaisuudet](./media/site-recovery-network-design/network-design8.png)

Kun vikasietotilaa käynnistyy ja näennäiskoneiden luodaan palautus-verkostoa haluamasi IP-verkoston yhteys voi muodostaa [Vnet Vnet-yhteyden](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md)avulla. Tämä toiminto voidaan komentosarjatoimintojen tarvittaessa.  Kun kerrottu edellisessä osassa aliverkon automaattisesti, vaikka kyseessä Azure-automaattisesti tietoja tiet on muokataan asianmukaisesti vastaamaan kyseisen 192.168.1.0/24 siirtyvät nyt Azure. 

![Kun olet aliverkon automaattisesti](./media/site-recovery-network-design/network-design9.png)

Kun olet automaattisesti

Jos sinulla ei ole "Azure verkon", kuten seuraavassa kuvassa on esitetty. Voit luoda sivuston-sivusto vpn-yhteyden 'Ensisijaisessa' ja 'Palautus verkossa' jälkeen vikasietotilaa.  


## <a name="option-2-changing-ip-addresses"></a>Vaihtoehto 2: Muuttaminen IP-osoitteet

Tämän menetelmän todennäköisesti eniten haittaohjelmien perusteella olemme on tullut. Vie muodossa muuttaminen jokaisen AM, joka liittyy vikasietotilaa IP-osoite. Haitta on tämän menetelmän edellyttää saat "", että sovellus, joka oli IPx on nyt IPy saapuvat verkkoon. Vaikka IPx ja IPy on looginen nimiä, DNS-arvojen on yleensä on muutettava tai tyhjennetty verkkoon, koko ja välimuistiin tallennetut tapahtumat verkon taulukoista on päivitetty tai tyhjennetty, sen vuoksi käyttökatkot nähdä sen mukaan, miten DNS-julkaisuinfrastruktuuri on jo asetukset. Ongelmaan on joiden lievity käyttämällä pienen TTL-arvot kyseessä intranet-sovellukset ja [Azure liikenteen hallinta ASR](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) internet-pohjaisten sovellusten

### <a name="changing-the-ip-addresses---illustration"></a>Muuttaminen IP-osoitteet - kuva

Anna meidän Tarkista skenaarion Jos aiot käyttää eri IP-osoitteet on ensisijainen ja palautus-sivustot. Seuraavassa esimerkissä on myös on kolmas sivuston-missä sovellukset isännöidään ensisijainen tai palauttaminen sivuston niitä voi käyttää.

![Eri IP - ennen automaattisesti](./media/site-recovery-network-design/network-design10.png)

Kuva 11

Kuva 11 ovat sovelluksia ylläpidettävä aliverkon 192.168.1.0/24 aliverkon ensisijainen sivustossa, ja ne on määritetty tulee aliverkon 172.16.1.0/24 palautus-sivustossa automaattisesti jälkeen. VPN-yhteydet tai verkon tiet on määritetty oikein, niin, että kaikki kolme sivustoa voivat käyttää toisiinsa.
 
Kun kuva 12 näyttää jälkeen epäonnistumista sovelluksia päälle, ne palautetaan palautus aliverkon. Tässä tapauksessa emme ovat ei rajoitettu voit siirtää koko aliverkon samanaikaisesti. Määrittämään VPN- tai verkon tiet ei tarvita muutoksia. Automaattisesti ja jotkin DNS-päivitykset varmistaa, että sovellukset ovat käytettävissä. Jos DNS on määritetty sallimaan dynaamisia päivityksiä näennäiskoneiden rekisteröityä itse käyttämällä uusi IP, kun he alkavat jälkeen automaattisesti. 

![Eri IP - jälkeen automaattisesti](./media/site-recovery-network-design/network-design11.png)

Kuva 12

Epäonnistuneen valittavia jälkeen replikan virtuaalikoneen on ehkä IP-osoite, jota ei ole sama kuin ensisijaisen virtuaalikoneen IP-osoite. Näennäiskoneiden päivittää DNS-palvelimeen, joita hän käyttää sen jälkeen, kun he alkavat. DNS-arvojen yleensä on muutettu tai tyhjennetty koko verkossa, ja välimuistiin tallennetut tapahtumat verkon taulukoista on päivitetty tai tyhjennetty, niin usein käyttökatkot kärsivät, kun tilan muutokset asetetaan. Tämä ongelma voi olla, joiden avulla lievity:

- Pieni TTL-arvot käytön intranet-sovellukset.
- Azure liikenteen hallinta käyttäminen ASR Internet-pohjaisia sovelluksia.
- Käyttämällä seuraavaa komentosarjaa sisällä palautus palvelupaketin Päivitä DNS-palvelin varmistaa ajoissa päivitys, (komentosarja ei edellytetä Jos dynaamisen DNS-rekisteröinti on määritetty)

        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord


### <a name="changing-the-ip-addresses--dr-to-azure"></a>IP-osoitteiden – DR Azure muuttaminen

[Microsoft Azure tietojen palauttaminen sivustojen infrastruktuurin asennus verkko](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) -blogimerkinnässä kerrotaan määrittäminen edellyttää Azure verkkoinfrastruktuuria, kun säilytetään IP-osoitteita ei ole tarpeen. Se alkaa kuvaava sovellus ja Etsi verkon paikallisen: n määrittäminen ja Azure ja sitten tekeminen kuinka toimia testi automaattisesti ja suunnitellun automaattisesti.



## <a name="next-steps"></a>Seuraavat vaiheet

[Lue](site-recovery-network-mapping.md) , kuinka sivuston palauttaminen kartat lähde- ja verkkojen kun VMM palvelimen käytetään ensisijainen sivuston hallinta. 
